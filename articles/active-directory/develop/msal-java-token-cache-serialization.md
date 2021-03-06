---
title: Anpassad token cache-serialisering (MSAL4j)
titleSuffix: Microsoft identity platform
description: Lär dig hur du serialiserar token-cachen för MSAL för Java
services: active-directory
author: sangonzal
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 11/07/2019
ms.author: sagonzal
ms.reviewer: nacanuma
ms.custom: aaddev
ms.openlocfilehash: bcb34d83365112b97769186ad74dfd762b05c2e8
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/28/2020
ms.locfileid: "76696171"
---
# <a name="custom-token-cache-serialization-in-msal-for-java"></a>Anpassad token cache-serialisering i MSAL för Java

Om du vill bevara token-cachen mellan instanser av ditt program måste du anpassa serialiseringen. Java-klasser och gränssnitt som ingår i cachelagring av token är följande:

- [ITokenCache](https://static.javadoc.io/com.microsoft.azure/msal4j/0.5.0-preview/com/microsoft/aad/msal4j/ITokenCache.html): gränssnitt som representerar cache för säkerhetstoken.
- [ITokenCacheAccessAspect](https://static.javadoc.io/com.microsoft.azure/msal4j/0.5.0-preview/com/microsoft/aad/msal4j/ITokenCacheAccessAspect.html): gränssnitt som representerar körning av kod före och efter åtkomst. Du skulle @Override *beforeCacheAccess* och *afterCacheAccess* med den logik som ansvarar för serialisering och deserialisering av cacheminnet.
- [ITokenCacheContext](https://static.javadoc.io/com.microsoft.azure/msal4j/0.5.0-preview/com/microsoft/aad/msal4j/ITokenCacheAccessContext.html): gränssnitt som representerar kontexten där token cache används. 

Nedan visas en naïve-implementering av anpassad serialisering för serialisering/deserialisering för token. Kopiera och klistra inte in detta i en produktions miljö.

```Java
static class TokenPersistence implements ITokenCacheAccessAspect {
String data;

TokenPersistence(String data) {
        this.data = data;
}

@Override
public void beforeCacheAccess(ITokenCacheAccessContext iTokenCacheAccessContext) {
        iTokenCacheAccessContext.tokenCache().deserialize(data);
}

@Override
public void afterCacheAccess(ITokenCacheAccessContext iTokenCacheAccessContext) {
        data = iTokenCacheAccessContext.tokenCache().serialize();
}
```

```Java
// Loads cache from file
String dataToInitCache = readResource(this.getClass(), "/cache_data/serialized_cache.json");

ITokenCacheAccessAspect persistenceAspect = new TokenPersistence(dataToInitCache);

// By setting *TokenPersistence* on the PublicClientApplication, MSAL will call *beforeCacheAccess()* before accessing the cache and *afterCacheAccess()* after accessing the cache. 
PublicClientApplication app = 
PublicClientApplication.builder("my_client_id").setTokenCacheAccessAspect(persistenceAspect).build();
```

## <a name="learn-more"></a>Läs mer

Lär dig mer om [att hämta och ta bort konton från token-cachen med MSAL för Java](msal-java-get-remove-accounts-token-cache.md).
