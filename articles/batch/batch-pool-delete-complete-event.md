---
title: Slutför händelse för borttagning av Azure Batch pool
description: Referens för händelsen borttagning av batch-pool slutförs. Den här händelsen genereras när en borttagning av en pool har slutförts.
ms.topic: reference
ms.date: 04/20/2017
ms.openlocfilehash: 6e009b55869b7ba8c81190837086557743d53219
ms.sourcegitcommit: 6fd8dbeee587fd7633571dfea46424f3c7e65169
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 05/21/2020
ms.locfileid: "83724062"
---
# <a name="pool-delete-complete-event"></a>Sluthändelse för borttagning av pool

 Den här händelsen genereras när en borttagning av en pool har slutförts.

 I följande exempel visas innehållet i en slutförd händelse för borttagning av pooler.

```
{
    "id": "myPool1",
    "startTime": "2016-09-09T22:13:48.579Z",
    "endTime": "2016-09-09T22:14:08.836Z"
}
```

|Element|Typ|Anteckningar|
|-------------|----------|-----------|
|`id`|Sträng|Poolens ID.|
|`startTime`|DateTime|Tiden då poolen börjar tas bort.|
|`endTime`|DateTime|Tiden då poolen togs bort.|

## <a name="remarks"></a>Kommentarer
Mer information om tillstånd och felkoder för att ändra storlek på pooler finns i [ta bort en pool från ett konto](https://docs.microsoft.com/rest/api/batchservice/delete-a-pool-from-an-account).
