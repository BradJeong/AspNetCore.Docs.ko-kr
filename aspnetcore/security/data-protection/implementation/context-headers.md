---
title: "컨텍스트 헤더"
author: rick-anderson
description: "이 문서에는 ASP.NET Core 데이터 보호 컨텍스트 헤더의 구현 세부 사항을 설명합니다."
keywords: "ASP.NET Core, 데이터 보호, 컨텍스트 헤더"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: d026a58c-67f4-411e-a410-c35f29c2c517
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/context-headers
ms.openlocfilehash: eb8e4c9ad67d3046648aea1b45f4a675b41b3ec0
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/10/2017
---
# <a name="context-headers"></a>컨텍스트 헤더

<a name="data-protection-implementation-context-headers"></a>

## <a name="background-and-theory"></a>배경 및 이론

데이터 보호 시스템에 "키"를 제공할 수 있는 개체에는 암호화 서비스 인증을 의미 합니다. 각 키는 고유 id (GUID)로 식별 되 고 함께 전달 알고리즘 정보 및 entropic 자료. 각 키 고유 엔트로피 하지만 시스템을 적용할 수 없는 운반 하 고 키 링의 기존 키의 알고리즘 정보를 수정 하 여 수동으로 키 링을 변경할 수 있는 개발자를 위한 계정 해야 것입니다. 이러한 경우 지정 된 보안 요구 사항은 달성 하기 위해 데이터 보호 시스템에는의 개념이 [암호화 유연성](https://www.microsoft.com/en-us/research/publication/cryptographic-agility-and-its-relation-to-circular-encryption/), 안전 하 게 단일 entropic 값을 사용 하 여 여러 암호화 알고리즘에서 허용 하는 합니다.

대부분의 암호화 유연성 지원 시스템 이렇게 페이로드 내부 알고리즘에 대 한 몇 가지 식별 정보가 포함 됩니다. 알고리즘의 OID는 일반적으로이 적합 합니다. 그러나 발생 했습니다. 한 가지 문제는 동일한 알고리즘을 지정 하려면 여러 가지: "AES" (CNG) 관리 되는 Aes, AesManaged, AesCryptoServiceProvider, AesCng, 및 RijndaelManaged (부여 된 특정 매개 변수) 클래스는 모든 실제로 동일한 올바른 OID를 이러한 모든 내용의 매핑을 유지 관리 하 고 작업을 해야 합니다. 개발자를 사용자 지정 알고리즘 (또는 AES의 다른 구현!)를 제공 하 려 해당 OID 인지 파악할 수 있어야 합니다. 이 여분의 등록 단계를 수행 하면 시스템 구성 크다고 합니다.

다시 실행, 잘못 된 방향에서 문제를 접근 된 म 결정 했습니다. OID 알려 알고리즘이 무엇 인지 하지만이에 실제로 우리는 크게 관심이 있습니다. 두 개의 서로 다른 알고리즘에는 단일 entropic 값을 안전 하 게 사용 해야 하는 경우 알고리즘은 실제로 알고 있어야 하기 위해 필요는 없습니다. 어떤 실제로 관심을 어떻게 동작 하는지입니다. 모든 대칭 괜찮은 블록 암호화 알고리즘은 강력한 의사 난수 순열 (PRP)도: 입력 (체인 모드, IV, 일반 텍스트 키)를 수정 하 고 암호 텍스트 출력은 확률 과도 하 게 많아지지와 다에서를 수 다른 대칭 블록 암호 알고리즘은 동일한 입력입니다. 마찬가지로, 모든 괜찮은 키 지정된 해시 함수는 강력한 의사 난수 함수 (PRF) 되며 고정된 된 입력된 집합에 지정 된 출력 가져다 구분 됩니다 다른 키 지정된 해시 함수.

컨텍스트 헤더를 구축할의 강력한 PRPs 및 PRFs이이 개념을 사용 합니다. 이 컨텍스트 헤더 기본적으로 역할 안정적인 지문 주어진된 작업에 사용 중인 알고리즘을 통해 하며 데이터 보호 시스템에 필요한 암호화 유연성을 제공 합니다. 이 헤더를 재현할 수 이며의 일환으로 나중에 사용 되는 [파생 프로세스를 하위 키](subkeyderivation.md#data-protection-implementation-subkey-derivation)합니다. 기본 알고리즘의 작업 모드에 따라 컨텍스트 헤더를 작성 하는 두 가지 방법으로 있습니다.

## <a name="cbc-mode-encryption--hmac-authentication"></a>CBC 모드 암호화 + HMAC 인증

<a name="data-protection-implementation-context-headers-cbc-components"></a>

컨텍스트 헤더는 다음 구성 요소가 구성 됩니다.

* [16 비트] 값 00 00 표식 "CBC 암호화 + HMAC 인증"을 의미 합니다.

* [32 비트] 키 길이 (바이트, big endian) 대칭 블록 암호화 알고리즘입니다.

* [32 비트] 블록 크기 (바이트, big endian) 대칭 블록 암호화 알고리즘의 합니다.

* [32 비트] 키 길이 (바이트, big endian) HMAC 알고리즘의 합니다. (현재 키 크기는 항상 크기와 일치 다이제스트.)

* [32 비트] 다이제스트의 크기 (바이트, big endian) HMAC 알고리즘입니다.

* EncCBC (K_E, IV, ""), 지정 된 빈 문자열 입력 블록 대칭 암호화 알고리즘의 출력 인 및 IV가 모두 0 인 벡터입니다. K_E 생성에 대 한 설명은 다음과 같습니다.

* MAC (K_H, ""), 지정 된 빈 문자열 입력 HMAC 알고리즘의 출력은입니다. K_H 생성에 대 한 설명은 다음과 같습니다.

이상적으로 모두 0 인 벡터 K_E 및 K_H 전달 수 없습니다. 그러나 여기서 기본 알고리즘은 있는지 여부를 확인 weak 키 (특히 DES 및 3DES) 작업을 수행 하기 전에 모두 0 인 벡터 같은 단순 또는 반복 가능한 패턴을 사용 하 여을 배제 하는 상황을 방지 하려고 합니다.

대신, NIST SP800 108 KDF 카운터 모드로 사용 (참조 [NIST SP800-108](http://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-108.pdf), 초 5.1) 된 길이가 0 인 키, 레이블 및 컨텍스트 및 기본 PRF로 HMACSHA512 합니다. 개체를 파생할 | K_E | + | K_H | 바이트의 출력을 다음 분해 결과 K_E 및 K_H 자체입니다. 수학적으로 다음과 같이 표시 됩니다.

(K_E | | K_H) SP800_108_CTR = (prf HMACSHA512 = 키 = ""을 label = "", 컨텍스트 = "")

### <a name="example-aes-192-cbc--hmacsha256"></a>예: AES-192-CBC + HMACSHA256

예를 들어 블록 대칭 암호화 알고리즘은 AES-192-CBC 유효성 검사 알고리즘은 HMACSHA256 경우 고려해 야 합니다. 시스템은 다음 단계를 사용 하 여 컨텍스트 헤더를 생성 합니다.

먼저, (K_E | | K_H) SP800_108_CTR = (prf HMACSHA512 = key = "", 레이블 = "", 컨텍스트 = ""), 여기서 | K_E | = 192 비트 및 | K_H | = 지정 된 알고리즘 당 256 비트입니다. 그러면 K_E 5BB6 =... 21DD 및 K_H A04A =... 아래 예제에서는 00A9:

```
5B B6 C9 83 13 78 22 1D 8E 10 73 CA CF 65 8E B0
61 62 42 71 CB 83 21 DD A0 4A 05 00 5B AB C0 A2
49 6F A5 61 E3 E2 49 87 AA 63 55 CD 74 0A DA C4
B7 92 3D BF 59 90 00 A9
```

다음으로, Enc_CBC 계산 (K_E, IV, "") AES-192-CBC IV 제공 = 0 * 및 위와 같은 K_E 합니다.

결과: F474B1872B3B53E4721DE19C0841DB6F =

다음으로 MAC을 계산 (K_H, "") HMACSHA256 위와 같은 K_H 제공에 대 한 합니다.

결과: D4791184B996092EE1202F36E8608FA8FBD98ABDFF5402F264B1D7211536220C =

아래 전체 컨텍스트 헤더를 생성합니다.

```
00 00 00 00 00 18 00 00 00 10 00 00 00 20 00 00
00 20 F4 74 B1 87 2B 3B 53 E4 72 1D E1 9C 08 41
DB 6F D4 79 11 84 B9 96 09 2E E1 20 2F 36 E8 60
8F A8 FB D9 8A BD FF 54 02 F2 64 B1 D7 21 15 36
22 0C
```

이 컨텍스트 헤더에는 인증 된 암호화 알고리즘 쌍 (예: AES-192-CBC 암호화 + HMACSHA256 유효성 검사)의 지문입니다. 설명 된 대로 구성 요소 [위에](xref:security/data-protection/implementation/context-headers#data-protection-implementation-context-headers-cbc-components) 됩니다.

* 표식의 (00 00)

* 블록 암호화 키 길이 (00 00 00 18)

* 블록 암호화 블록 크기 (00 00 00 10)

* HMAC 키 길이 (00 00 00 20)

* HMAC 다이제스트 크기 (00 00 00 20)

* 블록 암호 PRP 출력 (F4 74-DB 6F) 및

* HMAC PRF 출력 (D4 79-끝).

> [!NOTE]
> CBC 모드 암호화 + HMAC 인증 컨텍스트 헤더 SymmetricAlgorithm 및 KeyedHashAlgorithm 관리 되는 형식 또는 Windows CNG에서 알고리즘 구현에서 제공 하는 여부에 관계 없이 동일한 방식으로 빌드됩니다. 서로 다른 운영 체제에서 실행 하지 않도록 할 수는 알고리즘의 구현 Os 간에 다를 경우에 안정적으로 동일한 컨텍스트 헤더를 생성 합니다. (실제로 KeyedHashAlgorithm 아니어도 적절 한 HMAC입니다. 수 있는 키 지정된 해시 알고리즘 유형입니다.)

### <a name="example-3des-192-cbc--hmacsha1"></a>예: 3DES-192-CBC + HMACSHA1

먼저, (K_E | | K_H) SP800_108_CTR = (prf HMACSHA512 = key = "", 레이블 = "", 컨텍스트 = ""), 여기서 | K_E | = 192 비트 및 | K_H | = 지정 된 알고리즘 당 160 비트입니다. 그러면 K_E A219 =... E2BB 및 K_H DC4A =... 아래 예제에서는 B464:

```
A2 19 60 2F 83 A9 13 EA B0 61 3A 39 B8 A6 7E 22
61 D9 F8 6C 10 51 E2 BB DC 4A 00 D7 03 A2 48 3E
D1 F7 5A 34 EB 28 3E D7 D4 67 B4 64
```

다음으로, Enc_CBC 계산 (K_E, IV, "") 3DES-192-CBC IV 제공 = 0 * 및 위와 같은 K_E 합니다.

결과: ABB100F81E53E10E =

다음으로 MAC을 계산 (K_H, "") HMACSHA1 위와 같은 K_H 제공에 대 한 합니다.

결과: 76EB189B35CF03461DDF877CD9F4B1B4D63A7555 =

이렇게 하면 인증 된 사용자의 지문 인 전체 컨텍스트 헤더 생성 아래에 표시 된 암호화 알고리즘 쌍 (3DES-192-CBC 암호화 + HMACSHA1 유효성 검사):

```
00 00 00 00 00 18 00 00 00 08 00 00 00 14 00 00
00 14 AB B1 00 F8 1E 53 E1 0E 76 EB 18 9B 35 CF
03 46 1D DF 87 7C D9 F4 B1 B4 D6 3A 75 55
```

구성 요소는 다음과 같이 구분:

* 표식의 (00 00)

* 블록 암호화 키 길이 (00 00 00 18)

* 블록 암호화 블록 크기 (00 00 00 08)

* HMAC 키 길이 (00 00 00 14)

* HMAC 다이제스트 크기 (00 00 00 14)

* 블록 암호 PRP 출력 (AB B1-E1 0E) 및

* HMAC PRF 출력 (-76 EB 끝).

## <a name="galoiscounter-mode-encryption--authentication"></a>Galois/카운터 모드 암호화 + 인증

컨텍스트 헤더는 다음 구성 요소가 구성 됩니다.

* [16 비트] 00 값 01 표식 "GCM 암호화 + 인증"을 의미 합니다.

* [32 비트] 키 길이 (바이트, big endian) 대칭 블록 암호화 알고리즘입니다.

* [32 비트] nonce 크기 (바이트, big endian) 인증 된 암호화 작업 중에 사용 합니다. (, 우리의 시스템에 대 한 nonce 크기로 고정 되어이 96 비트 =.)

* [32 비트] 블록 크기 (바이트, big endian) 대칭 블록 암호화 알고리즘의 합니다. (GCM에 대 한 블록 크기에 고정 됩니다이 128 비트입니다.)

* [32 비트] 인증 태그 크기 (바이트, big endian) 인증 된 암호화 기능에서 생성 합니다. (, 우리의 시스템에 대 한 태그 크기로 고정 되어이 128 비트입니다.)

* [128 비트] Enc_GCM 태그 (nonce, K_E ""), 지정 된 빈 문자열 입력 블록 대칭 암호화 알고리즘의 출력 인 및 nonce는 96 비트 모두 0 인 벡터입니다.

K_E는 CBC 암호화 + HMAC 인증 시나리오에서와 같이 동일한 메커니즘을 사용 하 여 파생 됩니다. 그러나 플레이 여기에 없는 K_H 이므로 기본적으로 있는 | K_H | = 0, 알고리즘을 축소 하 고는 아래 폼입니다.

K_E SP800_108_CTR = (prf HMACSHA512 = 키 = ""을 label = "", 컨텍스트 = "")

### <a name="example-aes-256-gcm"></a>예: AES 256 GCM

첫째, K_E 수 있도록 SP800_108_CTR = (prf HMACSHA512 = 키 = "", 레이블 = "", 컨텍스트 = ""), 여기서 | K_E | = 256 비트입니다.

K_E: 22BC6F1B171C08C4AE2F27444AF8FC8B3087A90006CAEA91FDCFB47C1B8733B8 =

다음으로 Enc_GCM의 인증 태그가 계산 (K_E, nonce, "") AES-256-GCM nonce 제공 = 096 및 K_E 위와 같이 합니다.

결과: E7DCCE66DF855A323A6BB7BD7A59BE45 =

아래 전체 컨텍스트 헤더를 생성합니다.

```
00 01 00 00 00 20 00 00 00 0C 00 00 00 10 00 00
00 10 E7 DC CE 66 DF 85 5A 32 3A 6B B7 BD 7A 59
BE 45
```

구성 요소는 다음과 같이 구분:

   * 표식의 (00 01)

   * 블록 암호화 키 길이 (00 00 00 20)

   * nonce 크기 (00 00 00 0 C)

   * 블록 암호화 블록 크기 (00 00 00 10)

   * 인증 태그 크기 (00 00 00 10) 및

   * 실행 블록 암호 인증 태그가 (E7 DC-끝).
