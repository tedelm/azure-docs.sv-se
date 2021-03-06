---
title: Flytta din tjänst resurs över flera regioner
titleSuffix: Azure Cognitive Search
description: I den här artikeln visas hur du flyttar dina Azure Kognitiv sökning-resurser från en region till en annan i Azure-molnet.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: how-to
ms.custom: subject-moving-resources
ms.date: 03/24/2020
ms.openlocfilehash: 00f16d11f7a9cd276772eda5e91d6e117ada8c9f
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/28/2020
ms.locfileid: "80246313"
---
# <a name="move-your-azure-cognitive-search-service-to-another-azure-region"></a>Flytta din Azure Kognitiv sökning-tjänst till en annan Azure-region

Ibland kan kunder fråga om att flytta en Sök tjänst till en annan region. För närvarande finns det ingen inbyggd mekanism eller verktyg som hjälper dig med uppgiften, men den här artikeln kan hjälpa dig att förstå de manuella stegen för att uppnå samma resultat.

> [!NOTE]
> I Azure Portal har alla tjänster ett kommando för att **Exportera mall** . När det gäller Azure Kognitiv sökning ger det här kommandot en grundläggande definition av en tjänst (namn, plats, nivå, replik och partition), men känner inte igen innehållet i din tjänst, inte heller med nycklar, roller eller loggar. Även om kommandot finns rekommenderar vi inte att du använder det för att flytta en Sök tjänst.

## <a name="guidance-for-moving-a-service"></a>Vägledning för att flytta en tjänst

1. Identifiera beroenden och relaterade tjänster för att förstå den fulla effekten av att flytta en tjänst om du behöver flytta mer än bara Azure Kognitiv sökning.

   Azure Storage används för att logga, skapa ett kunskaps lager och är en vanlig extern data källa för AI-anrikning och indexering. Cognitive Services är ett beroende i AI-berikning. Både Cognitive Services och din Sök tjänst måste finnas i samma region om du använder AI-anrikning.

1. Skapa en förteckning över alla objekt på tjänsten så att du vet vad du ska flytta: index, synonym kartor, indexerare, data källor, färdighetsuppsättningar. Om du har aktiverat loggning kan du skapa och arkivera eventuella rapporter som du kan behöva för en historisk post.

1. Kontrol lera priser och tillgänglighet i den nya regionen för att säkerställa tillgängligheten för Azure Kognitiv sökning plus relaterade tjänster i den nya regionen. Majoriteten av funktionerna är tillgängliga i alla regioner, men vissa förhands gransknings funktioner har begränsad tillgänglighet.

1. Skapa en tjänst i den nya regionen och publicera om från käll koden eventuella befintliga index, synonym kartor, indexerare, data källor och färdighetsuppsättningar. Kom ihåg att tjänst namn måste vara unika så att du inte kan återanvända det befintliga namnet. Kontrol lera varje färdigheter för att se om anslutningar till Cognitive Services fortfarande är giltiga i enlighet med samma regions krav. Om du har skapat kunskaps lager kan du också kontrol lera anslutnings strängarna för Azure Storage om du använder en annan tjänst.

1. Läs in index och kunskaps lager igen, om tillämpligt. Du använder antingen program kod för att skicka JSON-data till ett index eller köra indexerare på nytt för att hämta dokument från externa källor. 

1. Aktivera loggning och skapa säkerhets roller igen om du använder dem.

1. Uppdatera klient program och test paket för att använda det nya tjänst namnet och API-nycklarna och testa alla program.

1. Ta bort den gamla tjänsten när den nya tjänsten är fullständigt testad och fungerar.

## <a name="next-steps"></a>Nästa steg

Följande länkar kan hjälpa dig att hitta mer information när du slutför stegen som beskrivs ovan.

+ [Priser och regioner för Azure Kognitiv sökning](https://azure.microsoft.com/pricing/details/search/)
+ [Välj en nivå](search-sku-tier.md)
+ [Skapa en Sök tjänst](search-create-service-portal.md)
+ [Läs in Sök dokument](search-what-is-data-import.md)
+ [Aktivera loggning](search-monitor-logs.md)


<!-- To move your Azure Cognitive Service account from one region to another, you will create an export template to move your subscription(s). After moving your subscription, you will need to move your data and recreate your service.

In this article, you'll learn how to:

> [!div class="checklist"]
> * Export a template.
> * Modify the template: adding the target region, search and storage account names.
> * Deploy the template to create the new search and storage accounts.
> * Verify your service status in the new region
> * Clean up resources in the source region.

## Prerequisites

- Ensure that the services and features that your account uses are supported in the target region.

- For preview features, ensure that your subscription is whitelisted for the target region. For more information about preview features, see [knowledge stores](https://docs.microsoft.com/azure/search/knowledge-store-concept-intro), [incremental enrichment](https://docs.microsoft.com/azure/search/cognitive-search-incremental-indexing-conceptual), and [private endpoint](https://docs.microsoft.com/azure/search/service-create-private-endpoint).

## Assessment and planning

When you move your search service to the new region, you will need to [move your data to the new storage service](https://docs.microsoft.com/azure/storage/common/storage-account-move?tabs=azure-portal#configure-the-new-storage-account) and then rebuild your indexes, skillsets and knowledge stores. You should record current settings and copy json files to make the rebuilding of your service easier and faster.

## Moving your search service's resources

To start you will export and then modify a Resource Manager template.

### Export a template

1. Sign in to the [Azure portal](https://portal.azure.com).

2. Go to your Resource Group page.

> [!div class="mx-imgBorder"]
> ![Resource Group page example](./media/search-move-resource/export-template-sample.png)

3. Select **All resources**.

3. In the left hand navigation menu select **Export template**.

4. Choose **Download** in the **Export template** page.

5. Locate the .zip file that you downloaded from the portal, and unzip that file to a folder of your choice.

The zip file contains the .json files that comprise the template and scripts to deploy the template.

### Modify the template

You will modify the template by changing the search and storage account names and regions. The names must follow the rules for each service and region naming conventions. 

To obtain region location codes, see [Azure Locations](https://azure.microsoft.com/global-infrastructure/locations/).  The code for a region is the region name with no spaces, **Central US** = **centralus**.

1. In the Azure portal, select **Create a resource**.

2. In **Search the Marketplace**, type **template deployment**, and then press **ENTER**.

3. Select **Template deployment**.

4. Select **Create**.

5. Select **Build your own template in the editor**.

6. Select **Load file**, and then follow the instructions to load the **template.json** file that you downloaded and unzipped in the previous section.

7. In the **template.json** file, name the target search and storage accounts by setting the default value of the search and storage account names. 

8. Edit the **location** property in the **template.json** file to the target region for both your search and storage services. This example sets the target region to `centralus`.

```json
},
    "variables": {},
    "resources": [
        {
            "type": "Microsoft.Search/searchServices",
            "apiVersion": "2020-03-13",
            "name": "[parameters('searchServices_target_region_search_name')]",
            "location": "centralus",
            "sku": {
                "name": "standard"
            },
            "properties": {
                "replicaCount": 1,
                "partitionCount": 1,
                "hostingMode": "Default"
            }
        },
        {
            "type": "Microsoft.Storage/storageAccounts",
            "apiVersion": "2019-06-01",
            "name": "[parameters('storageAccounts_tagetstorageregion_name')]",
            "location": "centralus",
            "sku": {
                "name": "Standard_RAGRS",
                "tier": "Standard"
            },
```

### Deploy the template

1. Save the **template.json** file.

2. Enter or select the property values:

- **Subscription**: Select an Azure subscription.

- **Resource group**: Select **Create new** and give the resource group a name.

- **Location**: Select an Azure location.

3. Click the **I agree to the terms and conditions stated above** checkbox, and then click the **Select Purchase** button.

## Verifying your services' status in new region

To verify the move, open the new resource group and your services will be listed with the new region.

To move your data from your source region to the target region, please see this article's guidelines for [moving your data to the new storage account](https://docs.microsoft.com/azure/storage/common/storage-account-move?tabs=azure-portal#move-data-to-the-new-storage-account).

## Clean up resources in your original region

To commit the changes and complete the move of your service account, delete the source service account.

## Next steps

[Create an index](https://docs.microsoft.com/azure/search/search-get-started-portal)

[Create a skillset](https://docs.microsoft.com/azure/search/cognitive-search-quickstart-blob)

[Create a knowledge store](https://docs.microsoft.com/azure/search/knowledge-store-create-portal) -->