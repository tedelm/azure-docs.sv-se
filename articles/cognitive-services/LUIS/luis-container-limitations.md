---
title: Begränsningar för behållare – LUIS
titleSuffix: Azure Cognitive Services
description: De LUIS behållar språk som stöds.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 04/01/2020
ms.author: aahi
ms.openlocfilehash: 2061d69fdfd13683ee722951cc7aaedcb1e1750a
ms.sourcegitcommit: 493b27fbfd7917c3823a1e4c313d07331d1b732f
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 05/21/2020
ms.locfileid: "83745374"
---
# <a name="language-understanding-luis-container-limitations"></a>Language Understanding (LUIS) begränsningar för behållare

LUIS-behållare har några viktiga begränsningar. I den här artikeln beskrivs de här begränsningarna från beroenden som inte stöds för en delmängd språk som stöds.

## <a name="supported-dependencies-for-latest-container"></a>Beroenden som stöds för `latest` container

Den senaste LUIS-behållaren stöder:

* [Nya fördefinierade domäner](luis-reference-prebuilt-domains.md): de här företags fokuserade domänerna omfattar entiteter, exempel yttranden och mönster. Utöka dessa domäner för eget bruk.

## <a name="unsupported-dependencies-for-latest-container"></a>Beroenden som inte stöds för `latest` container

Om du vill [Exportera för container](luis-container-howto.md#export-packaged-app-from-luis)måste du ta bort beroenden som inte stöds från Luis-appen. När du försöker exportera för container rapporterar LUIS-portalen dessa funktioner som inte stöds och som du måste ta bort.

Du kan använda ett LUIS-program om det **inte innehåller** något av följande beroenden:

App-konfigurationer som inte stöds|Information|
|--|--|
|Container kulturer stöds inte| De nederländska ( `nl-NL` ), japanska ( `ja-JP` ) och tyska ( `de-DE` ) språken stöds bara med 1.0.2- [tokenizer](luis-language-support.md#custom-tokenizer-versions).|
|Entiteter som inte stöds för alla kulturer|Fördefinierad enhets [fras](luis-reference-prebuilt-keyphrase.md) för alla kulturer|
|Entiteter som inte stöds för engelska ( `en-US` ) kultur|[GeographyV2](luis-reference-prebuilt-geographyV2.md) fördefinierade entiteter|
|Tal Prima|Externa beroenden stöds inte i behållaren.|
|Sentimentanalys|Externa beroenden stöds inte i behållaren.|
|Stavnings kontroll i Bing|Externa beroenden stöds inte i behållaren.|

## <a name="languages-supported"></a>Språk som stöds

LUIS-behållare har stöd för en delmängd av de [språk som stöds](luis-language-support.md#languages-supported) av Luis-korrekt. LUIS-behållare kan förstå yttranden på följande språk:

| Språk | Nationell inställning | Fördefinierad domän | Fördefinierad entitet | Rekommendationer för fras lista | **[Text analys](../text-analytics/language-support.md)<br>(Sentiment och<br>Reserverade|
|--|--|:--:|:--:|:--:|:--:|
| Engelska (USA) | `en-US` | ✔️ | ✔️ | ✔️ | ✔️ |
| *[Kinesiska](#chinese-support-notes) |`zh-CN` | ✔️ | ✔️ | ✔️ | ❌ |
| Franska (Frankrike) |`fr-FR` | ✔️ | ✔️ | ✔️ | ✔️ |
| Franska (Kanada) |`fr-CA` | ❌ | ❌ | ❌ | ✔️ |
| Tyska |`de-DE` | ✔️ | ✔️ | ✔️ | ✔️ |
| Hindi | `hi-IN`| ❌ | ❌ | ❌ | ❌ |
| Italienska |`it-IT` | ✔️ | ✔️ | ✔️ | ✔️ |
| Koreanska |`ko-KR` | ✔️ | ❌ | ❌ | Endast *nyckel fras* |
| Portugisiska (Brasilien) |`pt-BR` | ✔️ | ✔️ | ✔️ | alla under kulturer |
| Spanska (Spanien) |`es-ES` | ✔️ | ✔️ |✔️|✔️|
| Spanska (Mexiko)|`es-MX` | ❌ | ❌ |✔️|✔️|
| Turkiska | `tr-TR` |✔️| ❌ | ❌ | Endast *sentiment* |

[!INCLUDE [Chinese language support notes](includes/chinese-language-support-notes.md)]

[!INCLUDE [Text Analytics support notes](includes/text-analytics-support-notes.md)]