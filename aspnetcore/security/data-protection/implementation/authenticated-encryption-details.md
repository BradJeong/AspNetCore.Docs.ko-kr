---
title: ASP.NET Core에서 인증 된 암호화 세부 정보
author: rick-anderson
description: ASP.NET Core 데이터 보호 인증 된 암호화의 구현 세부 정보에 알아봅니다.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/implementation/authenticated-encryption-details
ms.openlocfilehash: 3ca5231e84156ede59793825e1a3e3bea0313055
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/22/2018
---
# <a name="authenticated-encryption-details-in-aspnet-core"></a><span data-ttu-id="97798-103">ASP.NET Core에서 인증 된 암호화 세부 정보</span><span class="sxs-lookup"><span data-stu-id="97798-103">Authenticated encryption details in ASP.NET Core</span></span>

<a name="data-protection-implementation-authenticated-encryption-details"></a>

<span data-ttu-id="97798-104">IDataProtector.Protect 호출 하는 인증 된 암호화 작업입니다.</span><span class="sxs-lookup"><span data-stu-id="97798-104">Calls to IDataProtector.Protect are authenticated encryption operations.</span></span> <span data-ttu-id="97798-105">Protect 메서드 신뢰성과 신뢰성을 소개 하며이 특정 IDataProtector 인스턴스 IDataProtectionProvider 루트에서 파생 하는 데 사용 된 용도 체인에 연결 되어 합니다.</span><span class="sxs-lookup"><span data-stu-id="97798-105">The Protect method offers both confidentiality and authenticity, and it's tied to the purpose chain that was used to derive this particular IDataProtector instance from its root IDataProtectionProvider.</span></span>

<span data-ttu-id="97798-106">IDataProtector.Protect은 바이트 일반 텍스트 매개 변수를 사용 하 고 바이트 보호 된 페이로드를 아래 설명 하는 형식의 생성 합니다.</span><span class="sxs-lookup"><span data-stu-id="97798-106">IDataProtector.Protect takes a byte[] plaintext parameter and produces a byte[] protected payload, whose format is described below.</span></span> <span data-ttu-id="97798-107">(이기도 문자열 일반 텍스트 매개 변수를 사용 하 고 문자열 보호 된 페이로드를 반환 하는 확장 메서드 오버 로드 합니다.</span><span class="sxs-lookup"><span data-stu-id="97798-107">(There's also an extension method overload which takes a string plaintext parameter and returns a string protected payload.</span></span> <span data-ttu-id="97798-108">이 API를 사용 하는 경우 보호 된 페이로드 형식을 계속 해 서는 구조, 아래 수는 있지만 [base64url로 인코딩된](https://tools.ietf.org/html/rfc4648#section-5).)</span><span class="sxs-lookup"><span data-stu-id="97798-108">If this API is used the protected payload format will still have the below structure, but it will be [base64url-encoded](https://tools.ietf.org/html/rfc4648#section-5).)</span></span>

## <a name="protected-payload-format"></a><span data-ttu-id="97798-109">보호 된 페이로드 형식</span><span class="sxs-lookup"><span data-stu-id="97798-109">Protected payload format</span></span>

<span data-ttu-id="97798-110">보호 된 페이로드 형식은 세 가지 기본 구성 요소 구성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="97798-110">The protected payload format consists of three primary components:</span></span>

* <span data-ttu-id="97798-111">데이터 보호 시스템의 버전을 식별 하는 32 비트 매직 헤더입니다.</span><span class="sxs-lookup"><span data-stu-id="97798-111">A 32-bit magic header that identifies the version of the data protection system.</span></span>

* <span data-ttu-id="97798-112">이 특정 페이로드를 보호 하는 데 사용 되는 키를 식별 하는 128 비트 키 id입니다.</span><span class="sxs-lookup"><span data-stu-id="97798-112">A 128-bit key id that identifies the key used to protect this particular payload.</span></span>

* <span data-ttu-id="97798-113">보호 된 페이로드의 나머지 부분이 [이 키에 의해 캡슐화 암호기 특정](xref:security/data-protection/implementation/subkeyderivation#data-protection-implementation-subkey-derivation)합니다.</span><span class="sxs-lookup"><span data-stu-id="97798-113">The remainder of the protected payload is [specific to the encryptor encapsulated by this key](xref:security/data-protection/implementation/subkeyderivation#data-protection-implementation-subkey-derivation).</span></span> <span data-ttu-id="97798-114">아래 예제에서는 키 AES-256-CBC + HMACSHA256 암호기 나타내며 페이로드는 추가로 다음과 같이 다음과 같이: \* 128 비트 키 한정자입니다.</span><span class="sxs-lookup"><span data-stu-id="97798-114">In the example below the key represents an AES-256-CBC + HMACSHA256 encryptor, and the payload is further subdivided as follows: \* A 128-bit key modifier.</span></span> <span data-ttu-id="97798-115">\*는 128 비트 초기화 벡터입니다.</span><span class="sxs-lookup"><span data-stu-id="97798-115">\* A 128-bit initialization vector.</span></span> <span data-ttu-id="97798-116">\* AES-256-CBC 출력의 48 바이트입니다.</span><span class="sxs-lookup"><span data-stu-id="97798-116">\* 48 bytes of AES-256-CBC output.</span></span> <span data-ttu-id="97798-117">\*는 HMACSHA256 인증 태그입니다.</span><span class="sxs-lookup"><span data-stu-id="97798-117">\* An HMACSHA256 authentication tag.</span></span>

<span data-ttu-id="97798-118">샘플 보호 된 페이로드 아래 그림에 나와 있습니다.</span><span class="sxs-lookup"><span data-stu-id="97798-118">A sample protected payload is illustrated below.</span></span>

```
09 F0 C9 F0 80 9C 81 0C 19 66 19 40 95 36 53 F8
AA FF EE 57 57 2F 40 4C 3F 7F CC 9D CC D9 32 3E
84 17 99 16 EC BA 1F 4A A1 18 45 1F 2D 13 7A 28
79 6B 86 9C F8 B7 84 F9 26 31 FC B1 86 0A F1 56
61 CF 14 58 D3 51 6F CF 36 50 85 82 08 2D 3F 73
5F B0 AD 9E 1A B2 AE 13 57 90 C8 F5 7C 95 4E 6A
8A AA 06 EF 43 CA 19 62 84 7C 11 B2 C8 71 9D AA
52 19 2E 5B 4C 1E 54 F0 55 BE 88 92 12 C1 4B 5E
52 C9 74 A0
```

<span data-ttu-id="97798-119">첫 번째 32 비트 또는 4 바이트일 위에 페이로드 형식 (09 F0 C9 F0) 버전을 확인 하는 매직 헤더에는</span><span class="sxs-lookup"><span data-stu-id="97798-119">From the payload format above the first 32 bits, or 4 bytes are the magic header identifying the version (09 F0 C9 F0)</span></span>

<span data-ttu-id="97798-120">다음 128 비트 또는 16 바이트는 키 식별자 (80 9 C 81 0 C 19 66 19 40 95 36 53 F8 AA FF EE 57)</span><span class="sxs-lookup"><span data-stu-id="97798-120">The next 128 bits, or 16 bytes is the key identifier (80 9C 81 0C 19 66 19 40 95 36 53 F8 AA FF EE 57)</span></span>

<span data-ttu-id="97798-121">나머지는 페이로드가 포함 되며 사용 되는 형식에만 있습니다.</span><span class="sxs-lookup"><span data-stu-id="97798-121">The remainder contains the payload and is specific to the format used.</span></span>

>[!WARNING]
> <span data-ttu-id="97798-122">지정된 된 키에 보호 되는 모든 페이로드 동일한 20 바이트 (매직 값, 키 id) 헤더로 시작 됩니다.</span><span class="sxs-lookup"><span data-stu-id="97798-122">All payloads protected to a given key will begin with the same 20-byte (magic value, key id) header.</span></span> <span data-ttu-id="97798-123">관리자는 근사치를 구하려면 페이로드가 생성 될 때 진단 목적을 위해이 팩트를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="97798-123">Administrators can use this fact for diagnostic purposes to approximate when a payload was generated.</span></span> <span data-ttu-id="97798-124">예를 들어 위의 페이로드 {0c819c80-6619-4019-9536-53f8aaffee57} 키에 해당합니다.</span><span class="sxs-lookup"><span data-stu-id="97798-124">For example, the payload above corresponds to key {0c819c80-6619-4019-9536-53f8aaffee57}.</span></span> <span data-ttu-id="97798-125">키 저장소를 확인 한 후를이 특정 키 활성화 날짜를 2015-01-01 만료 날짜를 2015-03-01 힙이고 상태일 수 없다고 해 페이로드 (변조 되지) 하는 경우 제공 해당 창 내에서 생성 된 하거나 작은 수행 어느 쪽에 오차입니다.</span><span class="sxs-lookup"><span data-stu-id="97798-125">If after checking the key repository you find that this specific key's activation date was 2015-01-01 and its expiration date was 2015-03-01, then it's reasonable to assume that the payload (if not tampered with) was generated within that window, give or take a small fudge factor on either side.</span></span>
