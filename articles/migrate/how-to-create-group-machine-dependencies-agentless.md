---
title: Konfigurera beroende analys utan agent i Azure Migrate Server bedömning
description: Konfigurera en agent lös beroende analys i Azure Migrate Server bedömning.
ms.topic: how-to
ms.date: 2/24/2020
ms.openlocfilehash: af767bf73a3b9a6f2a91298987f11974499fd694
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/28/2020
ms.locfileid: "79455714"
---
# <a name="set-up-agentless-dependency-visualization"></a>Konfigurera beroende visualisering för agent utan agent 

I den här artikeln beskrivs hur du konfigurerar en agent lös beroende analys i Azure Migrate: Server bedömning. Beroende [analys](concepts-dependency-visualization.md) hjälper dig att identifiera och förstå beroenden mellan datorer som du vill utvärdera och migrera till Azure.


> [!IMPORTANT]
> Det finns för närvarande en för hands version av beroende visualisering för virtuella VMware-datorer som har identifierats med verktyget Azure Migrate: Server bedömning.
> Funktioner kan vara begränsade eller ofullständiga.
> Den här för hands versionen täcks av kund support och kan användas för produktions arbets belastningar.
> Mer information finns i [Kompletterande villkor för användning av Microsoft Azure-förhandsversioner](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).



## <a name="before-you-start"></a>Innan du börjar

- [Läs mer om](concepts-dependency-visualization.md#agentless-analysis) beroende analys av agenter.
- [Granska](migrate-support-matrix-vmware.md#agentless-dependency-analysis-requirements) kraven och support kraven för att konfigurera en agent utan beroende visualisering för virtuella VMware-datorer
- Se till att du har [skapat](how-to-add-tool-first-time.md) ett Azure Migrate-projekt.
- Om du redan har skapat ett projekt kontrollerar du att du har [lagt till](how-to-assess.md) Azure Migrate: Server utvärderings verktyget.
- Se till att du har konfigurerat en [Azure Migrate-apparat](migrate-appliance.md) för att identifiera dina lokala datorer. Lär dig hur du konfigurerar en installation för virtuella [VMware](how-to-set-up-appliance-vmware.md) -datorer. Enheten identifierar lokala datorer och skickar metadata-och prestanda data till Azure Migrate: Server utvärdering.


## <a name="current-limitations"></a>Aktuella begränsningar

- Just nu kan du inte lägga till eller ta bort en server från en grupp i vyn beroende analys.
- En beroende karta för en Server grupp är inte tillgänglig för tillfället.
- Beroende data kan för närvarande inte hämtas i tabell format.

## <a name="create-a-user-account-for-discovery"></a>Skapa ett användar konto för identifiering

Konfigurera ett användar konto så att Server utvärderingen kan komma åt den virtuella datorn för identifiering. [Läs mer](migrate-support-matrix-vmware.md#agentless-dependency-analysis-requirements) om konto krav.


## <a name="add-the-user-account-to-the-appliance"></a>Lägg till användar kontot till enheten

Lägg till användar kontot till enheten.

1. Öppna appen för hantering av appar. 
2. Navigera till panelen **Tillhandahåll vCenter-information** .
3. I **identifiera program och beroenden på virtuella datorer**klickar du på **Lägg till autentiseringsuppgifter**
3. Välj **operativ system**, ange ett eget namn för kontot och**lösen ordet** för **användar namn**/
6. Klicka på **Spara**.
7. Klicka på **Spara och starta identifiering**.

    ![Lägg till användar konto för virtuell dator](./media/how-to-create-group-machine-dependencies-agentless/add-vm-credential.png)

## <a name="start-dependency-discovery"></a>Starta beroende identifiering

Välj de datorer där du vill aktivera beroende identifiering.

1. Klicka på **identifierade servrar**i **Azure Migrate: Server bedömning**.
2. Klicka på ikonen **beroende analys** .
3. Klicka på **Lägg till servrar**.
3. På sidan **Lägg till servrar** väljer du den installation som identifierar relevanta datorer.
4. Välj datorerna i listan dator.
5. Klicka på **Lägg till servrar**.

    ![Starta beroende identifiering](./media/how-to-create-group-machine-dependencies-agentless/start-dependency-discovery.png)

Du kan visualisera beroenden runt sex timmar efter start av beroende identifiering.

## <a name="visualize-dependencies"></a>Visualisera beroenden

1. Klicka på **identifierade servrar**i **Azure Migrate: Server bedömning**.
2. Sök efter den dator som du vill visa.
3. I kolumnen **beroenden** klickar du på **Visa beroenden**
4. Ändra den tids period som du vill visa kartan med i list rutan **tids längd** .
5. Expandera **klient** gruppen för att visa en lista med datorer som är beroende av den valda datorn.
6. Expandera **port** gruppen om du vill visa en lista med datorer som är beroende av den valda datorn.
7. Om du vill navigera till vyn karta för någon av de beroende datorerna klickar du på dator namnet > **belastnings Server karta**

    ![Expandera server port grupp och läsa in Server karta](./media/how-to-create-group-machine-dependencies-agentless/load-server-map.png)

    ![Expandera klient grupp ](./media/how-to-create-group-machine-dependencies-agentless/expand-client-group.png)

8. Expandera den valda datorn om du vill visa information på process nivå för varje beroende.

    ![Expandera Server för att visa processer](./media/how-to-create-group-machine-dependencies-agentless/expand-server-processes.png)

> [!NOTE]
> Process information för ett beroende är inte alltid tillgängligt. Om det inte är tillgängligt visas beroendet med processen markerad som "okänd process".

## <a name="stop-dependency-discovery"></a>Stoppa beroende identifiering

Välj de datorer där du vill stoppa beroende identifiering.

1. Klicka på **identifierade servrar**i **Azure Migrate: Server bedömning**.
2. Klicka på ikonen **beroende analys** .
3. Klicka på **ta bort servrar**.
3. På sidan **ta bort servrar** **väljer du den installation som identifierar** de virtuella datorer där du vill stoppa beroende identifiering.
4. Välj datorerna i listan dator.
5. Klicka på **ta bort servrar**.


## <a name="next-steps"></a>Nästa steg

[Gruppera datorerna](how-to-create-a-group.md) för utvärdering.
