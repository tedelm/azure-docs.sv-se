---
title: Avancerat skydd för Azure Cosmos DB
description: Lär dig hur Azure Cosmos DB tillhandahåller kryptering av data i vila och hur de implementeras.
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/13/2019
ms.custom: seodec18
ms.author: memildin
author: memildin
manager: rkarlin
ms.openlocfilehash: 0b37f9be5eb3313cc451e65f90d7f2713c68806d
ms.sourcegitcommit: 1f25aa993c38b37472cf8a0359bc6f0bf97b6784
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 05/26/2020
ms.locfileid: "83845992"
---
# <a name="advanced-threat-protection-for-azure-cosmos-db-preview"></a>Avancerat skydd för Azure Cosmos DB (för hands version)

Avancerat skydd för Azure Cosmos DB ger ytterligare ett lager med säkerhets information som identifierar ovanliga och potentiellt skadliga försök att komma åt eller utnyttja Azure Cosmos DB-konton. Det här skydds lagret gör att du kan åtgärda hot, även utan att vara säkerhets expert, och integrera dem med centrala säkerhets övervaknings system.

Säkerhets varningar utlöses när avvikelser i aktivitet inträffar. Dessa säkerhets aviseringar är integrerade med [Azure Security Center](https://azure.microsoft.com/services/security-center/)och skickas också via e-post till prenumerations administratörer med information om den misstänkta aktiviteten och rekommendationer om hur du undersöker och åtgärdar hoten.

> [!NOTE]
>
> * Avancerat skydd för Azure Cosmos DB är för närvarande endast tillgängligt för SQL-API: et.
> * Avancerat skydd för Azure Cosmos DB är för närvarande inte tillgängligt i moln regioner i Azure myndigheter och i vår suveräna region.

För en fullständig utredning av säkerhets aviseringar rekommenderar vi att du aktiverar [diagnostikloggning i Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/logging), som loggar åtgärder på själva databasen, inklusive CRUD åtgärder på alla dokument, behållare och databaser.

## <a name="threat-types"></a>Hottyper

Avancerat skydd för Azure Cosmos DB identifierar avvikande aktiviteter som visar ovanliga och potentiellt skadliga försök att komma åt eller utnyttja databaser. Den kan för närvarande utlösa följande aviseringar:

- **Åtkomst från ovanliga platser**: den här aviseringen utlöses när åtkomst mönstret i ett Azure Cosmos-konto ändras, där någon har anslutit till Azure Cosmos DB slut punkten från en ovanlig geografisk plats. I vissa fall identifierar aviseringen en legitim åtgärd, vilket innebär ett nytt program eller en utvecklares underhålls åtgärd. I andra fall identifierar aviseringen en skadlig åtgärd från en tidigare anställd, extern angripare osv.

- **Ovanlig data extrahering**: den här aviseringen utlöses när en klient extraherar en ovanlig mängd data från ett Azure Cosmos DB-konto. Detta kan vara symptom på vissa data exfiltrering som utförs för att överföra alla data som lagras i kontot till ett externt data lager.



## <a name="configure-advanced-threat-protection"></a>Konfigurera Advanced Threat Protection

Du kan konfigurera Avancerat skydd på flera sätt, som beskrivs i följande avsnitt.

### <a name="portal"></a>[Portal](#tab/azure-portal)

1. Starta Azure Portal på [https://portal.azure.com](https://portal.azure.com/) .

2. Från Azure Cosmos DB-kontot går du till menyn **Inställningar** och väljer **avancerad säkerhet**.

    ![Konfigurera ATP](./media/cosmos-db-advanced-threat-protection/cosmos-db-atp.png)

3. På bladet **Avancerad säkerhets** konfiguration:

    * Klicka på alternativet **Avancerat skydd** för att ställa in det på **på**.
    * Klicka på **Spara** för att spara den nya eller uppdaterade Advanced Threat Protection-principen.   

### <a name="rest-api"></a>[REST-API](#tab/rest-api)

Använd REST API-kommandon för att skapa, uppdatera eller Hämta inställningen för avancerat skydd för ett bestämt Azure Cosmos DB konto.

* [Avancerat skydd – skapa](https://go.microsoft.com/fwlink/?linkid=2099745)
* [Avancerat skydd – Hämta](https://go.microsoft.com/fwlink/?linkid=2099643)

### <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Använd följande PowerShell-cmdletar:

* [Aktivera Advanced Threat Protection](https://go.microsoft.com/fwlink/?linkid=2099607&clcid=0x409)
* [Hämta Avancerat skydd](https://go.microsoft.com/fwlink/?linkid=2099608&clcid=0x409)
* [Inaktivera Avancerat skydd](https://go.microsoft.com/fwlink/?linkid=2099709&clcid=0x409)

### <a name="arm-template"></a>[ARM-mall](#tab/arm-template)

Använd en Azure Resource Manager ARM-mall (ARM) för att konfigurera Cosmos DB med avancerat skydd aktiverat.
Mer information finns i [skapa ett CosmosDB-konto med avancerat skydd](https://azure.microsoft.com/resources/templates/201-cosmosdb-advanced-threat-protection-create-account/).

### <a name="azure-policy"></a>[Azure Policy](#tab/azure-policy)

Använd en Azure Policy för att aktivera avancerat skydd mot Cosmos DB.

1. Starta sidan Azure **policy-definitions** och Sök efter principen **distribuera avancerat skydd för Cosmos DB** .

    ![Sök princip](./media/cosmos-db-advanced-threat-protection/cosmos-db.png) 

1. Klicka på principen **distribuera Avancerat skydd för CosmosDB** och klicka sedan på **tilldela**.

    ![Välj prenumeration eller grupp](./media/cosmos-db-advanced-threat-protection/cosmos-db-atp-policy.png)


1. Klicka på de tre punkterna i fältet **scope** , Välj en Azure-prenumeration eller resurs grupp och klicka sedan på **Välj**.

    ![Sidan princip definitioner](./media/cosmos-db-advanced-threat-protection/cosmos-db-atp-details.png)


1. Ange de andra parametrarna och klicka på **tilldela**.




## <a name="manage-atp-security-alerts"></a>Hantera säkerhets aviseringar för ATP

När Azure Cosmos DB aktivitets avvikelser uppstår utlöses en säkerhets avisering med information om den misstänkta säkerhets händelsen. 

 Från Azure Security Center kan du granska och hantera dina aktuella [säkerhets aviseringar](../security-center/security-center-alerts-overview.md).  Klicka på en avisering i [Security Center](https://ms.portal.azure.com/#blade/Microsoft_Azure_Security/SecurityMenuBlade/0) om du vill visa möjliga orsaker och rekommenderade åtgärder för att undersöka och minimera det potentiella hotet. Följande bild visar ett exempel på aviserings information som finns i Security Center.

 ![Hot information](./media/cosmos-db-advanced-threat-protection/cosmos-db-alert-details.png)

Ett e-postmeddelande skickas också med aviserings information och rekommenderade åtgärder. Följande bild visar ett exempel på ett e-postmeddelande om aviseringar.

 ![Aviseringsinformation](./media/cosmos-db-advanced-threat-protection/cosmos-db-alert.png)

## <a name="cosmos-db-atp-alerts"></a>Cosmos DB ATP-aviseringar

 Om du vill se en lista över de aviseringar som genererats när du övervakar Azure Cosmos DB konton, se avsnittet [Cosmos DB aviseringar](https://docs.microsoft.com/azure/security-center/alerts-reference#alerts-azurecosmos) i Azure Security Center-dokumentationen.

## <a name="next-steps"></a>Nästa steg

* Lär dig mer om [diagnostisk loggning i Azure Cosmos DB](cosmosdb-monitor-resource-logs.md)
* Läs mer om [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro)
