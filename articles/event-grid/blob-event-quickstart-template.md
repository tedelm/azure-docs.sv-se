---
title: Skicka Blob Storage-händelser till webb slut punkt-mall
description: Använd Azure Event Grid och en Azure Resource Manager mall för att skapa Blob Storage-konto och prenumerera på dess händelser. Skicka händelserna till en webhook.
services: event-grid
keywords: ''
author: spelluru
ms.author: spelluru
ms.date: 02/27/2020
ms.topic: quickstart
ms.service: event-grid
ms.custom: subject-armqs
ms.openlocfilehash: 86dc7a4ed05ceae5c7a641ffef23bd75ec48ceea
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/29/2020
ms.locfileid: "81605529"
---
# <a name="route-blob-storage-events-to-web-endpoint-by-using-azure-resource-manager-template"></a>Dirigera Blob Storage-händelser till webb slut punkt med hjälp av Azure Resource Manager mall

Azure Event Grid är en händelsetjänst för molnet. I den här artikeln använder du en **Azure Resource Manager mall** för att skapa ett Blob Storage-konto, prenumerera på händelser för blob-lagringen och utlösa en händelse för att visa resultatet. Normalt kan du skicka händelser till en slutpunkt som bearbetar informationen om händelsen och utför åtgärder. Men för att enkelt beskriva den här artikeln kan skicka du händelser till en webbapp som samlar in och visar meddelanden.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt](https://azure.microsoft.com/free/) konto innan du börjar.

## <a name="prerequisites"></a>Krav

### <a name="create-a-message-endpoint"></a>Skapa en slutpunkt för meddelanden

Innan du prenumererar på händelserna för Blob Storage-kontot ska vi skapa slutpunkten för händelsemeddelandet. Slutpunkten utför vanligtvis åtgärder baserat på informationen om händelsen. För att förenkla den här snabbstarten kan du distribuera en [förskapad webbapp](https://github.com/Azure-Samples/azure-event-grid-viewer) som visar meddelanden om händelser. Den distribuerade lösningen innehåller en App Service-plan,en webbapp för App Service och källkod från GitHub.

1. Välj **Deploy to Azure** (Distribuera till Azure) för att distribuera lösningen till din prenumeration. Ange parametervärdena i Azure Portal.

    [Distribuera till Azure](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure-Samples%2Fazure-event-grid-viewer%2Fmaster%2Fazuredeploy.json)
1. Det kan ta några minuter att slutföra distributionen. Efter distributionen har slutförts kan du visa webbappen för att kontrollera att den körs. I en webbläsare navigerar du till: `https://<your-site-name>.azurewebsites.net`

1. Du ser webbplatsen men det har inte publicerats händelser till den än.

   ![Visa ny webbplats](./media/blob-event-quickstart-portal/view-site.png)

## <a name="create-a-storage-account-with-an-event-grid-subscription"></a>Skapa ett lagrings konto med en Event Grid-prenumeration

### <a name="review-the-template"></a>Granska mallen

Mallen som används i den här snabb starten är från [Azure snabb starts-mallar](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid-subscription-and-storage).

[!code-json[<Azure Resource Manager template create Blob storage Event Grid subscription>](~/quickstart-templates/101-event-grid-subscription-and-storage/azuredeploy.json)]

Två Azure-resurser definieras i mallen:

* [**Microsoft. Storage/storageAccounts**](/azure/templates/microsoft.storage/storageaccounts): skapa ett Azure Storage-konto.
* [**"Microsoft. Storage/storageAccounts/providers/eventSubscriptions**](/azure/templates/microsoft.eventgrid/eventsubscriptions): skapa en Azure Event Grid-prenumeration för lagrings kontot.

### <a name="deploy-the-template"></a>Distribuera mallen

1. Välj följande länk för att logga in på Azure och öppna en mall. Mallen skapar ett nyckelvalv och en hemlighet.

    [![Distribuera till Azure](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-event-grid-subscription-and-storage%2Fazuredeploy.json)

2. Ange **slut punkten**: Ange webb adressen till webb programmet och Lägg till `api/updates` på Start sidans URL.
3. Välj **köp** för att distribuera mallen.

  Azure Portal används här för att distribuera mallen. Du kan också använda Azure PowerShell, Azure CLI och REST API. Mer information om andra distributions metoder finns i [distribuera mallar](../azure-resource-manager/templates/deploy-powershell.md).

> [!NOTE]
> Du hittar fler Azure Event Grid mall-exempel [här](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Eventgrid).

## <a name="validate-the-deployment"></a>Verifiera distributionen

Visa ditt webbprogram igen och observera att en händelse för verifieringen av prenumerationen har skickats till den. Välj ögonikonen för att utöka informationen om händelsen. Händelserutnätet skickar valideringshändelsen så att slutpunkten kan bekräfta att den vill ta emot händelsedata. Webbappen inkluderar kod för att verifiera prenumerationen.

![Visa prenumerationshändelse](./media/blob-event-quickstart-portal/view-subscription-event.png)

Nu ska vi utlösa en händelse och se hur Event Grid distribuerar meddelandet till slutpunkten.

Du kan utlösa en händelse för Blob Storage-kontot genom att ladda upp en fil. Filen behöver inte innehålla något specifikt. Artiklarna förutsätter att du har en fil med namnet testfile.txt, men du kan använda vilken fil som helst.

När du överför filen till Azure Blob Storage skickar Event Grid ett meddelande till den slut punkt som du konfigurerade när du prenumererar. Meddelandet är i JSON-format och innehåller en matris med en eller flera händelser. I följande exempel innehåller JSON-meddelandet en matris med en händelse. Visa din webbapp och notera att en skapad blobhändelse tagits emot.

![Visa resultat](./media/blob-event-quickstart-portal/view-results.png)

## <a name="clean-up-resources"></a>Rensa resurser

[Ta bort resurs gruppen när den](../azure-resource-manager/management/delete-resource-group.md?tabs=azure-portal#delete-resource-group
)inte längre behövs.

## <a name="next-steps"></a>Nästa steg

Mer information om Azure Resource Manager-mallar finns i följande artiklar:

* [Azure Resource Manager dokumentation](/azure/azure-resource-manager)
* [Definiera resurser i Azure Resource Manager mallar](/azure/templates/)
* [Azure-snabbstartsmallar](https://azure.microsoft.com/resources/templates/)
* [Azure Event Grid mallar](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Eventgrid).
