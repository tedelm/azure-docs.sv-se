---
title: Så här bläddrar du igenom Sök Resultat – API:er för Bing-sökresultat
titleSuffix: Azure Cognitive Services
description: Lär dig hur du bläddrar genom Sök Resultat från API:er för Bing-sökresultat.
services: cognitive-services
author: aahill
manager: nitinme
ms.assetid: 26CA595B-0866-43E8-93A2-F2B5E09D1F3B
ms.service: cognitive-services
ms.subservice: bing-web-search
ms.topic: conceptual
ms.date: 10/31/2019
ms.author: aahi
ms.openlocfilehash: ea883bb294a8769b3c9be1e0eafc2e3e7c811b48
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/29/2020
ms.locfileid: "73481741"
---
# <a name="how-to-page-through-results-from-the-bing-search-apis"></a>Så här bläddrar du genom resultat från API:er för Bing-sökresultat

När du skickar ett anrop till webb-, anpassade-, bild-, diskussions-eller Videosökning-API: er för Bing returnerar Bing en delmängd av det totala antalet resultat som kan vara relevanta för frågan. Om du vill få det uppskattade totala antalet tillgängliga resultat kan du komma `totalEstimatedMatches` åt svars objektets fält. 

Ett exempel: 

```json
{
    "_type" : "SearchResponse",
    "webPages" : {
        "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=3A43CA...",
        "totalEstimatedMatches" : 262000,
        "value" : [...]
    }
}  
```

## <a name="paging-through-search-results"></a>Växling genom Sök Resultat

Om du vill bläddra igenom de tillgängliga resultaten använder `count` du `offset` parametrarna och för att skicka din begäran.  

> [!NOTE]
>
> * Sid indelning med Bing Video-, bild-och diskussions grupps-API:`/video/search`erna gäller bara för`/news/search`allmänna video ()`/image/search`, Nyheter () och bild ()-sökningar. Det går inte att växla mellan olika trender och kategorier.  
> * `TotalEstimatedMatches` Fältet är en uppskattning av det totala antalet Sök Resultat för den aktuella frågan. När du ställer in `count` parametrarna `offset` och kan den här uppskattningen ändras.

| Parameter | Beskrivning                                                                                                                                                                |
|-----------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `count`   | Anger antalet resultat som ska returneras i svaret. Observera att standardvärdet för `count`och det maximala antalet resultat som kan begäras varierar beroende på API. Du hittar dessa värden i referens dokumentationen under [Nästa steg](#next-steps). |
| `offset`  | Anger antalet resultat som ska hoppas över. `offset` Är noll-baserat och måste vara mindre än (`totalEstimatedMatches` - `count`).                                           |

Om du till exempel vill visa 15 resultat per sida, anger `count` du till 15 och `offset` 0 för att få den första resultat sidan. För varje efterföljande API-anrop skulle du öka `offset` med 15. I följande exempel begärs 15 webb sidor som börjar vid förskjutningen 45.

```  
GET https://api.cognitive.microsoft.com/bing/v7.0/search?q=sailing+dinghies&count=15&offset=45&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
Host: api.cognitive.microsoft.com  
```

Om du använder `count` standardvärdet behöver du bara ange `offset` frågeparametern i dina API-anrop.  

```  
GET https://api.cognitive.microsoft.com/bing/v7.0/search?q=sailing+dinghies&offset=45&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
Host: api.cognitive.microsoft.com  
```

När du använder Bing-avbildningen och video-API: erna `nextOffset` kan du använda värdet för att undvika dubbla Sök resultat. Hämta värdet från- `Images` eller `Videos` -svar-objekten och Använd det i dina begär Anden med `offset` parametern.  

> [!NOTE]
> API för webbsökning i Bing returnerar Sök resultat som kan innehålla webb sidor, bilder, videor och nyheter. När du bläddrar genom Sök resultatet från API för webbsökning i Bing, är du bara att växla [webb sidor](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-web-api-v7-reference#webpage)och inte andra svars typer, till exempel bilder eller nyheter. Sök resultat i `WebPage` objekt kan innehålla resultat som även visas i andra svars typer.
>
> Om du använder `responseFilter` Frågeparametern utan att ange några filter värden ska du `count` inte använda parametrarna och `offset` . 

## <a name="next-steps"></a>Nästa steg

* [Vad är Webbsökning i Bing API: er?](bing-api-comparison.md)
* [Referens för API för webbsökning i Bing v7](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-web-api-v7-reference)
* [API för anpassad Bing-sökning v7-referens](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-custom-search-api-v7-reference)
* [API för nyhetssökning i Bing v7-referens](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-news-api-v7-reference)
* [API för videosökning i Bing v7-referens](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-video-api-v7-reference)
* [API för bildsökning i Bing v7-referens](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference)
