---
title: Hämta API för åtgärder | Azure Marketplace
description: Hämtar alla åtgärder för erbjudandet eller för att få en viss åtgärd för angivet operationId.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: reference
ms.date: 04/08/2020
ms.author: dsindona
ms.openlocfilehash: 93b2ca700a987b86aedfdae55d58540c8ffe84ed
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/28/2020
ms.locfileid: "81255881"
---
# <a name="retrieve-operations"></a>Hämta åtgärder

> [!NOTE]
> Cloud Partner Portal API: er är integrerade med partner Center och fortsätter att fungera när dina erbjudanden har migrerats till Partner Center. I integrationen presenteras små ändringar. Granska ändringarna som anges i [Cloud Partner Portal API-referensen](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-api-overview) för att se till att koden fortsätter att fungera efter migreringen till Partner Center.

Hämtar alla åtgärder för erbjudandet eller för att få en viss åtgärd för angivet operationId. Klienten kan använda frågeparametrar för att filtrera vid pågående åtgärder.

``` https

  GET https://cloudpartner.azure.com/api/publishers/<publisherId>/offers/<offerId>/submissions/?api-version=2017-10-31&status=<filteredStatus>

  GET https://cloudpartner.azure.com/api/publishers/<publisherId>/offers/<offerId>/operations/<operationId>?api-version=2017-10-31

```


## <a name="uri-parameters"></a>URI-parametrar

|  **Namn**          |      **Beskrivning**                                                                                           | **Datatyp** |
|  ----------------  |     --------------------------------------------------------------------------------------------------------   |  -----------  |
|  publisherId       |  Utgivar identifierare, till exempel`Contoso`                                                                   |  Sträng       |
|  offerId           |  Erbjudande-ID                                                                                              |  Sträng       |
|  operationId       |  GUID som unikt identifierar åtgärden för erbjudandet. OperationId kan hämtas med hjälp av det här API: et och returneras också i HTTP-huvudet för svaret för en tids krävande åtgärd, t. ex. API för [publicerings erbjudande](./cloud-partner-portal-api-publish-offer.md) .  |   GUID   |
|  api-version       | Senaste versionen av API |    Date      |
|  |  |  |

## <a name="header"></a>Huvud


|  **Namn**          |  **Värde**           |
|  ---------------   | -------------------- |
|  Content-Type      | `application/json`   |
|  Auktorisering     | `Bearer YOUR_TOKEN`  |
|  |  |


## <a name="body-example"></a>Body-exempel

### <a name="response"></a>Svar

#### <a name="get-operations"></a>Hämta åtgärder

``` json
    [
        {
            "id": "5a63deb5-925b-4ee0-938b-7c86fbf287c5",
            "offerId": "56615b67-2185-49fe-80d2-c4ddf77bb2e8",
            "offerVersion": 1,
            "offerTypeId": "microsoft-azure-virtualmachines",
            "publisherId": "contoso",
            "submissionType": "publish",
            "submissionState": "running",
            "publishingVersion": 2,
            "slot": "staging",
            "version": 636576975611768314,
            "definition": {
                "metadata": {
                    "emails": "jdoe@contoso.com"
                }
            },
            "changedTime": "2018-03-26T21:46:01.179948Z"
        }
    ]
```

#### <a name="get-operation"></a>Hämta åtgärd

``` json
    [
        {
            "status" : "running",
            "messages" : [],
            "publishingVersion" : 2,
            "offerVersion" : 1,
            "cancellationRequestState": "canCancel",
            "steps": [
                        {
                            "estimatedTimeFrame": "< 15 min",
                            "id": "displaydummycertify",
                            "stepName": "Validate Pre-Requisites",
                            "description": "Offer settings provided are validated",
                            "status": "complete",
                            "messages": 
                            [
                                {
                                    "messageHtml": "Step completed.",
                                    "level": "information",
                                    "timestamp": "2017-03-28T19:50:36.500052Z"
                                }
                            ],
                            "progressPercentage": 100
                        },
                        {
                            "estimatedTimeFrame": "< 5 day",
                            "id": "displaycertify",
                            "stepName": "Certification",
                            "description": "Your offer is analyzed by our certification systems for issues.",
                            "status": "blocked",
                            "messages": 
                            [
                                {
                                    "messageHtml": "No virtual machine image was found for the plan contoso.",
                                    "level": "error",
                                    "timestamp": "2017-03-28T19:50:39.5506018Z"
                                },
                                {
                                    "messageHtml": "This step has not started yet.",
                                    "level": "information",
                                    "timestamp": "2017-03-28T19:50:39.5506018Z"
                                }
                            ],
                            "progressPercentage": 0
                        },
                        {
                            "estimatedTimeFrame": "< 1 day",
                            "id": "displayprovision",
                            "stepName": "Provisioning",
                            "description": "Your virtual machine is being replicated in our production systems.",
                            "status": "notStarted",
                            "messages": [],
                            "progressPercentage": 0
                        },
                        {
                            "estimatedTimeFrame": "< 1 hour",
                            "id": "displaypackage",
                            "stepName": "Packaging and Lead Generation Registration",
                            "description": "Your virtual machine is packaged for being shown to your customers. Additionally, we hookup our lead generation systems to send leads for your offer.",
                            "status": "notStarted",
                            "messages": [],
                            "progressPercentage": 0
                        },
                        {
                            "id": "publisher-signoff",
                            "stepName": "Publisher signoff",
                            "description": "Offer is available to preview. Ensure that everything looks good before making your offer live.",
                            "status": "notStarted",
                            "messages": [],
                            "progressPercentage": 0
                        },
                        {
                            "estimatedTimeFrame": "~2-5 days",
                            "id": "live",
                            "stepName": "Live",
                            "description": "Offer is publicly visible and is available for purchase.",
                            "status": "notStarted",
                            "messages": [],
                            "progressPercentage": 0
                        }
                    ],
                "previewLinks": [],
                "liveLinks": [],
            }
        }
    ]
```

### <a name="response-body-properties"></a>Egenskaper för svars text

|  **Namn**                    |  **Beskrivning**                                                                                  |
|  --------------------        |  ------------------------------------------------------------------------------------------------ |
|  id                          | GUID som unikt identifierar åtgärden                                                       |
|  submissionType              | Identifierar den typ av åtgärd som rapporteras för erbjudandet, till exempel`Publish/GoLive`      |
|  createdDateTime             | UTC datetime när åtgärden skapades                                                       |
|  lastActionDateTime          | UTC datetime när den senaste uppdateringen utfördes för åtgärden                                       |
|  status                      | `not started` \| `running` \| `failed` \| Status för åtgärden, antingen `completed`. Endast en åtgärd kan ha status `running` i taget. |
|  fel                       | Fel meddelande för misslyckade åtgärder                                                               |
|  |  |

### <a name="response-step-properties"></a>Egenskaper för svars steg

|  **Namn**                    |  **Beskrivning**                                                                                  |
|  --------------------        |  ------------------------------------------------------------------------------------------------ |
| estimatedTimeFrame | Den uppskattade varaktigheten för den här åtgärden |
| id | Den unika identifieraren för steg processen |
| description | Beskrivning av steget |
| stepName | Det egna namnet för steget |
| status | Status för steget, antingen `notStarted` \| `running` \| `failed` \|`completed` |
| meddelanden | Eventuella meddelanden eller varningar som påträffades under steget. Strängmatris |
| progressPercentage | Ett heltal mellan 0 och 100 som anger förloppet för steget |
| | |

### <a name="response-status-codes"></a>Svars status koder

| **Kod**  |   **Beskrivning**                                                                                  |
|  -------- |   -------------------------------------------------------------------------------------------------|
|  200      | `OK`-Begäran har bearbetats och de begärda Åtgärdarna returnerades.        |
|  400      | `Bad/Malformed request`– Fel svars texten kan innehålla mer information.                    |
|  403      | `Forbidden`-Klienten har inte åtkomst till den angivna namn rymden.                          |
|  404      | `Not found`-Den angivna entiteten finns inte.                                                 |
|  |  |
