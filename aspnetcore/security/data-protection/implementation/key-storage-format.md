---
title: ASP.NET Core에서 키 저장소 형식
author: rick-anderson
description: ASP.NET Core 데이터 보호는 키 저장소 형식이의 구현 세부 정보에 알아봅니다.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/implementation/key-storage-format
ms.openlocfilehash: 1a5912f246708355e6677c60034d982d053c3938
ms.sourcegitcommit: a19261eb82b948af6e4a1664fcfb8dabb16150e3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/14/2018
---
# <a name="key-storage-format-in-aspnet-core"></a>ASP.NET Core에서 키 저장소 형식

<a name="data-protection-implementation-key-storage-format"></a>

개체는 XML 표현에 저장 됩니다. 키 저장소에 대 한 기본 디렉터리는 %LOCALAPPDATA%\ASP.NET\DataProtection-Keys\ 합니다.

## <a name="the-key-element"></a>\<키 > 요소

키의 키 저장소에는 최상위 개체도 존재합니다. 키 파일 이름에는 규칙에 따라 **키-{guid}.xml**, 여기서 {guid}이 키의 id입니다. 이러한 각 파일에는 단일 키를 포함합니다. 파일의 형식은 다음과 같습니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<key id="80732141-ec8f-4b80-af9c-c4d2d1ff8901" version="1">
  <creationDate>2015-03-19T23:32:02.3949887Z</creationDate>
  <activationDate>2015-03-19T23:32:02.3839429Z</activationDate>
  <expirationDate>2015-06-17T23:32:02.3839429Z</expirationDate>
  <descriptor deserializerType="{deserializerType}">
    <descriptor>
      <encryption algorithm="AES_256_CBC" />
      <validation algorithm="HMACSHA256" />
      <enc:encryptedSecret decryptorType="{decryptorType}" xmlns:enc="...">
        <encryptedKey>
          <!-- This key is encrypted with Windows DPAPI. -->
          <value>AQAAANCM...8/zeP8lcwAg==</value>
        </encryptedKey>
      </enc:encryptedSecret>
    </descriptor>
  </descriptor>
</key>
```

\<키 > 요소는 다음과 같은 특성 및 자식 요소를 포함 합니다.

* 키 id입니다. 이 값은 정식으로; 처리 파일 이름이 휴먼 가독성을 높이기 위해 못했던 단순히입니다.

* 버전의는 \<키 > 요소를 1에서 현재 고정 합니다.

* 키의 생성, 활성화, 및 만료 날짜입니다.

* A \<설명자 >이 키에 포함 된 인증 된 암호화 구현에 대 한 정보를 포함 하는 요소입니다.

위의 예제에서는 키의 id가 {80732141-ec8f-4b80-af9c-c4d2d1ff8901}, 만들어지고 2015 년 3 월 19 일에 활성화 된 및 90 일의 수명은 합니다. (경우에 따라 활성화 날짜 약간 있을 수 있으며 다음이 예제와 같이 작성 날짜 전에 합니다. 이것은 Api 작동 하 고는 무시 해도 실제로 하는 방식에서 nit 인해.)

## <a name="the-descriptor-element"></a>\<설명자 > 요소

외부 \<설명자 > 요소는이 IAuthenticatedEncryptorDescriptorDeserializer를 구현 하는 형식의 정규화 된 어셈블리 이름을 특성 deserializerType 포함 합니다. 이 형식은 내부 읽는 역할 \<설명자 > 요소 내에 포함 된 정보를 구문 분석 하 고 있습니다.

특정 형식의 \<설명자 > 요소는 키에 의해 캡슐화 된 인증 된 암호기 구현에 따라 및 각 deserializer 유형에 대해이 약간 다른 형식을 필요로 합니다. 일반적으로 하지만,이 요소를 포함 합니다 알고리즘 정보 (이름, 형식, Oid를 또는 유사한 곳) 및 비밀 키 자료. 위의 예에서 설명자가이 키는 AES-256-CBC 암호화 + HMACSHA256 유효성 검사를 래핑하는 지정 합니다.

## <a name="the-encryptedsecret-element"></a>\<encryptedSecret > 요소

<encryptedSecret> 비밀 키 자료의 암호화 된 형태를 포함 하는 요소 있을 수 경우 [비밀 미사용 데이터 암호화가 설정](xref:security/data-protection/implementation/key-encryption-at-rest#data-protection-implementation-key-encryption-at-rest)합니다. 특성 decryptorType IXmlDecryptor를 구현 하는 형식의 정규화 된 어셈블리 이름이 됩니다. 이 형식은 내부 읽는 역할 <encryptedKey> 요소와 복구를 원래의 일반 텍스트로 암호를 해독 합니다.

와 마찬가지로 \<설명자 >, 특정 형식의 <encryptedSecret> 요소 사용에서-휴지 암호화 메커니즘에 따라 달라 집니다. 위의 예에 마스터 키가 주석 당 Windows DPAPI를 사용 하 여 암호화 합니다.

## <a name="the-revocation-element"></a>\<해지 > 요소

해지 키 저장소에는 최상위 개체도 존재합니다. 규칙에 따라 해지는 파일 이름을 가진 **해지-{timestamp}.xml** (에 대 한 특정 날짜 이전의 모든 키를 해지) 또는 **해지-{guid}.xml** (예: 특정 키를 해지). 각 파일에는 단일 \<해지 > 요소입니다.

해지의 개별 키에 대 한 파일 내용이 됩니다 다음과 같이 합니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<revocation version="1">
  <revocationDate>2015-03-20T22:45:30.2616742Z</revocationDate>
  <key id="eb4fc299-8808-409d-8a34-23fc83d026c9" />
  <reason>human-readable reason</reason>
</revocation>
```

이 경우 지정된 된 키가 해지 되었습니다. 그러나 키 id가 "*",에서 같이 아래 예제에서는 모든 키를 가진 생성 날짜를 지정한 해지 날짜 전에 취소 됩니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<revocation version="1">
  <revocationDate>2015-03-20T15:45:45.7366491-07:00</revocationDate>
  <!-- All keys created before the revocation date are revoked. -->
  <key id="*" />
  <reason>human-readable reason</reason>
</revocation>
```

\<이유 > 요소는 시스템에서 읽지 않습니다. 단순히 해지 사람이 읽을 수 있는 이유를 저장할 수 있는 편리한 위치는
