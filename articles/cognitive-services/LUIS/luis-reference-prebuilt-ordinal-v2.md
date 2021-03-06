---
title: Ordnings tal v2 för inbyggd entitet – LUIS
titleSuffix: Azure Cognitive Services
description: Den här artikeln innehåller en fördefinierad entitetsinformation för ordinal v2 i Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 09/27/2019
ms.author: diberry
ms.openlocfilehash: 5e852313db75e598da647ea0f985e2ee18af16de
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/29/2020
ms.locfileid: "78270490"
---
# <a name="ordinal-v2-prebuilt-entity-for-a-luis-app"></a>Ordnings tal v2, fördefinierad entitet för en LUIS-app
Ordnings talet v2 Number expanderar [ordnings tal](luis-reference-prebuilt-ordinal.md) för att `next`ge `last`relativa referenser `previous`som, och. Dessa extraheras inte med den fördefinierade ordnings ordningen.

## <a name="resolution-for-prebuilt-ordinal-v2-entity"></a>Lösning för fördefinierad ordnings tal v2-entitet

Följande enhets objekt returneras för frågan:

`what is the second to last choice in the list`

#### <a name="v3-response"></a>[V3-svar](#tab/V3)

Följande JSON- `verbose` parameter har angetts till `false`:

```json
"entities": {
    "ordinalV2": [
        {
            "offset": -1,
            "relativeTo": "end"
        }
    ]
}
```

#### <a name="v3-verbose-response"></a>[V3 utförlig Response](#tab/V3-verbose)

Följande JSON- `verbose` parameter har angetts till `true`:

```json
"entities": {
    "ordinalV2": [
        {
            "offset": -1,
            "relativeTo": "end"
        }
    ],
    "$instance": {
        "ordinalV2": [
            {
                "type": "builtin.ordinalV2.relative",
                "text": "the second to last",
                "startIndex": 8,
                "length": 18,
                "modelTypeId": 2,
                "modelType": "Prebuilt Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            }
        ]
    }
}
```
#### <a name="v2-response"></a>[V2-svar](#tab/V2)

I följande exempel visas upplösningen för entiteten **Builtin. ordinalV2** .

```json
"entities": [
    {
        "entity": "the second to last",
        "type": "builtin.ordinalV2.relative",
        "startIndex": 8,
        "endIndex": 25,
        "resolution": {
            "offset": "-1",
            "relativeTo": "end"
        }
    }
]
```
* * *

## <a name="next-steps"></a>Nästa steg

Läs mer om [v3 förutsägelse slut punkten](luis-migration-api-v3.md).

Lär dig mer om enheterna [procent](luis-reference-prebuilt-percentage.md), [telefonnummer](luis-reference-prebuilt-phonenumber.md)och [temperatur](luis-reference-prebuilt-temperature.md) .
