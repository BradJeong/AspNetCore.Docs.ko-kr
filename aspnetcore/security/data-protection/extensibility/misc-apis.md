---
title: 기타 ASP.NET Core 데이터 보호 Api
author: rick-anderson
description: ASP.NET Core Data Protection ISecret 인터페이스에 대해 알아봅니다.
ms.author: riande
ms.date: 10/14/2016
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/data-protection/extensibility/misc-apis
ms.openlocfilehash: e9de92233468e9e07791df608b1c37ffb3b29949
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/26/2020
ms.locfileid: "85408502"
---
# <a name="miscellaneous-aspnet-core-data-protection-apis"></a>기타 ASP.NET Core 데이터 보호 Api

<a name="data-protection-extensibility-mics-apis"></a>

>[!WARNING]
> 다음 인터페이스 중 하나를 구현 하는 형식은 여러 호출자에 대해 스레드로부터 안전 해야 합니다.

## <a name="isecret"></a>ISecret

`ISecret`인터페이스는 암호화 키 자료와 같은 비밀 값을 나타냅니다. 다음 API 화면을 포함 합니다.

* `Length`: `int`

* `Dispose()`: `void`

* `WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`

`WriteSecretIntoBuffer`메서드는 제공 된 버퍼를 원시 비밀 값으로 채웁니다. 이 API는을 직접 반환 하지 않고 버퍼를 매개 변수로 사용 하는 이유는 `byte[]` 호출자에 게 버퍼 개체를 고정 하 여 관리 되는 가비지 수집기에 대 한 비밀 노출을 제한 하는 기회를 제공 하는 것입니다.

`Secret`이 형식은 `ISecret` 비밀 값이 in-process 메모리에 저장 되는의 구체적 구현입니다. Windows 플랫폼에서 비밀 값은 [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx)를 통해 암호화 됩니다.
