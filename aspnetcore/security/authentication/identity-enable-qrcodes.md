---
title: "ASP.NET Core에서 인증자 앱에 대 한 QR 코드를 생성 하도록 설정"
author: rick-anderson
description: "ASP.NET Core에서 인증자 앱에 대 한 QR 코드를 생성 하도록 설정"
keywords: "ASP.NET Core, MVC에서 QR 코드를 생성, 인증자, 2FA"
ms.author: riande
manager: wpickett
ms.date: 09/24/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity-enable-qrcodes
ms.openlocfilehash: a3029e68164dd91d1bc43704c5e96bd591bcae05
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/10/2017
---
# <a name="enabling-qr-code-generation-for-authenticator-apps-in-aspnet-core"></a>ASP.NET Core에서 인증자 앱에 대 한 QR 코드를 생성 하도록 설정

참고:이 항목을 적용할 ASP.NET Core 2.x

ASP.NET Core authenticator 응용 프로그램에서 개별 인증에 대 한 지원이 포함 되어 있습니다. 시간 기반 일회용 암호 알고리즘 (TOTP)를 사용 하 여 요소 인증 (2FA) 인증자 앱, 두 가지 권장 접근법 2FA에 대 한 업계 있습니다. 2FA TOTP를 사용 하 여는 SMS 2FA 하는 것이 좋습니다. Authenticator 앱에 있는 사용자는 자신의 사용자 이름과 암호를 확인 한 후에 들어가야 합니다. 6 to 8 자리 코드를 제공 합니다. 일반적으로 ऍ 스마트 폰에 설치 됩니다.

ASP.NET Core 웹 응용 프로그램 템플릿 인증자를 지원 하지만 QRCode 생성에 대 한 지원을 제공 하지 않습니다. QRCode 생성기 쉽게 2FA 설치. 이 문서 안내할 추가 [QR 코드](https://wikipedia.org/wiki/QR_code) 2FA 구성 페이지를 생성 합니다.

## <a name="adding-qr-codes-to-the-2fa-configuration-page"></a>2FA 구성 페이지에 QR 코드를 추가합니다.

이 지침은 사용 *qrcode.js* https://davidshimjs.github.io/qrcodejs/ 리포지토리에서 합니다.

* 다운로드는 [qrcode.js javascript 라이브러리](https://davidshimjs.github.io/qrcodejs/) 에 `wwwroot\lib` 프로젝트의 폴더에에서 있습니다.

* *Pages\Account\Manage\EnableAuthenticator.cshtml* (Razor 페이지) 또는 *Views\Manage\EnableAuthenticator.cshtml* (MVC)을 찾아는 `Scripts` 파일의 끝 섹션:

```cshtml
@section Scripts {
    @await Html.PartialAsync("_ValidationScriptsPartial")
}
```

* 업데이트는 `Scripts` 섹션에 대 한 참조를 추가 하는 `qrcodejs` 추가한 라이브러리 및 호출을 QR 코드를 생성 합니다. 다음과 같이 표시 됩니다.

```cshtml
@section Scripts {
    @await Html.PartialAsync("_ValidationScriptsPartial")

    <script type="text/javascript" src="~/lib/qrcode.js"></script>
    <script type="text/javascript">
        new QRCode(document.getElementById("qrCode"),
            {
                text: "@Html.Raw(Model.AuthenticatorUri)",
                width: 150,
                height: 150
            });
    </script>
}
```

* 이러한 지침에 대 한 링크를 제공 하는 단락을 삭제 합니다.

응용 프로그램을 실행 하 고 QR 코드 스캔 하 고 인증자 증명 코드 유효성을 검사할 수 있는지를 확인 합니다.

## <a name="change-the-site-name-in-the-qr-code"></a>QR 코드에 사이트 이름을 변경 합니다.

처음 프로젝트를 만들 때 선택한 프로젝트 이름에서 QR 코드에 사이트 이름을 가져옵니다. 검색 하 여 변경할 수는 `GenerateQrCodeUri(string email, string unformattedKey)` 에서 메서드는 *Pages\Account\Manage\EnableAuthenticator.cshtml.cs* (Razor 페이지) 파일 또는 *Controllers\ManageController.cs* (MVC) 파일입니다. 

서식 파일에서 기본 코드의 모양은 다음과 같습니다.

```c#
private string GenerateQrCodeUri(string email, string unformattedKey)
{
    return string.Format(
        AuthenicatorUriFormat,
        _urlEncoder.Encode("Razor Pages"),
        _urlEncoder.Encode(email),
        unformattedKey);
}
```

에 대 한 호출에서 두 번째 매개 변수 `string.Format` 은 솔루션 이름에서 가져온 사이트 이름입니다. 모든 값을 변경할 수는 있지만 URL로 인코딩된 항상 존재 해야 합니다.

## <a name="using-a-different-qr-code-library"></a>다른 QR 코드 라이브러리를 사용 하 여

기본 라이브러리에 있는 QR 코드 라이브러리를 바꿀 수 있습니다. HTML을 포함 한 `qrCode` 라이브러리에 제공 하는 요소는 어떤 메커니즘으로 QR 코드를 배치할 수 있습니다.

QR 코드에 대 한 올바른 형식의 URL은에서 사용할 수는 있습니다.

* `AuthenticatorUri`모델의 속성입니다.
* `data-url`속성에는 `qrCodeData` 요소입니다. 

사용 하 여 `@Html.Raw` 뷰에서 모델 속성에 액세스 (url에서 앰퍼샌드 double 인코딩할 및 QR 코드의 레이블 매개 변수는 무시 합니다. 그렇지 않은 경우).

## <a name="totp-client-and-server-time-skew"></a>TOTP 클라이언트 및 서버 시간 차가

TOTP 인증 서버와 인증자 장치 정확한 시간을 필요에 따라 달라 집니다. 토큰 30 초만 지속 됩니다. TOTP 2FA 로그인 실패 하는 경우 서버 시간이 정확 하 고 정확한 NTP 서비스에 동기화 된 가급적 인지 확인 합니다.
