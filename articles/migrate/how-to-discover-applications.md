---
title: Identifiera appar, roller och funktioner på lokala servrar med Azure Migrate
description: Lär dig hur du identifierar appar, roller och funktioner på lokala servrar med Azure Migrate Server bedömning.
ms.topic: article
ms.date: 03/12/2020
ms.openlocfilehash: ff9f5489b513cd1405e6b093d7537e4cbcead041
ms.sourcegitcommit: 3beb067d5dc3d8895971b1bc18304e004b8a19b3
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 05/04/2020
ms.locfileid: "82744620"
---
# <a name="discover-machine-apps-roles-and-features"></a>Identifiera appar, roller och funktioner i datorn

I den här artikeln beskrivs hur du identifierar program, roller och funktioner på lokala servrar med hjälp av Azure Migrate: Server bedömning.

Att identifiera inventeringen av appar, och roller/funktioner som körs på dina lokala datorer hjälper dig att identifiera och planera en migrerings Sök väg till Azure som är skräddarsydd för dina arbets belastningar.

> [!NOTE]
> App Discovery är för närvarande endast för för hands versioner för virtuella VMware-datorer och är begränsat till enbart identifiering. Vi erbjuder ännu inte app-baserad utvärdering. Dator-baserad utvärdering för lokala virtuella VMware-datorer, virtuella Hyper-V-datorer och fysiska servrar.

Identifiering av app med Azure Migrate: Server utvärderingen är agent lös. Inget är installerat på datorer och virtuella datorer. Server utvärderingen använder Azure Migrate-installationen för att utföra identifiering tillsammans med autentiseringsuppgifter för dator gäst. Enheten ansluter till VMware-datorerna via VMware-API: er.


## <a name="before-you-start"></a>Innan du börjar

1. Se till att du har [skapat](how-to-add-tool-first-time.md) ett Azure Migrate-projekt.
2. Kontrol lera att du har [lagt](how-to-assess.md) till verktyget Azure Migrate: Server utvärderings verktyg i ett projekt.
4. Kontrol lera [VMware-kraven](migrate-support-matrix-vmware.md#vmware-requirements) för att identifiera och utvärdera virtuella VMware-datorer med Azure Migrate-installationen.
5. Kontrol lera [kraven](migrate-appliance.md) för att distribuera Azure Migrate-enheten.
6. [Verifiera support och krav](migrate-support-matrix-vmware.md#application-discovery) för program identifiering.



## <a name="deploy-the-azure-migrate-appliance"></a>Distribuera Azure Migrate-enheten

1. [Granska](migrate-appliance.md#appliance---vmware) kraven för att distribuera Azure Migrate-enheten.
2. Granska de Azure-URL: er som krävs för att få åtkomst till [molnet](migrate-appliance.md#government-cloud-urls) [offentliga](migrate-appliance.md#public-cloud-urls) och myndigheter.
3. [Granska data](migrate-appliance.md#collected-data---vmware) som samlas in under identifiering och bedömning.
4. [Antecknings](migrate-support-matrix-vmware.md#port-access) portens åtkomst krav för produkten.
5. [Distribuera Azure Migrate-apparaten](how-to-set-up-appliance-vmware.md) för att starta identifieringen. För att distribuera installationen kan du hämta och importera en ägg-mall till VMware för att skapa installationen som en virtuell VMware-dator. Du konfigurerar installationen och registrerar den sedan med Azure Migrate.
6. När du distribuerar installationen kan du starta kontinuerlig identifiering genom att ange följande:
    - Namnet på vCenter Server som du vill ansluta till.
    - Autentiseringsuppgifter som du har skapat för installationen av för att ansluta till vCenter Server.
    - De kontoautentiseringsuppgifter som du skapade för installationen av för att ansluta till virtuella Windows-och Linux-datorer.

När installationen har distribuerats och du har angett autentiseringsuppgifter, startar installationen kontinuerlig identifiering av VM-metadata och prestanda data, tillsammans med och identifiering av appar, funktioner och roller.  Längden på appens identifiering är beroende av hur många virtuella datorer du har. Det tar vanligt vis en timme för app-identifiering av 500 virtuella datorer.

## <a name="prepare-a-user-account"></a>Förbereda ett användar konto

Skapa ett konto som ska användas för identifiering och Lägg till det i enheten.

### <a name="create-a-user-account-for-discovery"></a>Skapa ett användar konto för identifiering

Konfigurera ett användar konto så att Server utvärderingen kan komma åt den virtuella datorn för identifiering. [Läs mer](migrate-support-matrix-vmware.md#application-discovery) om konto krav.


### <a name="add-the-user-account-to-the-appliance"></a>Lägg till användar kontot till enheten

Lägg till användar kontot till enheten.

1. Öppna appen för hantering av appar. 
2. Navigera till panelen **Tillhandahåll vCenter-information** .
3. I **identifiera program och beroenden på virtuella datorer**klickar du på **Lägg till autentiseringsuppgifter**
3. Välj **operativ system**, ange ett eget namn för kontot och**lösen ordet** för **användar namn**/
6. Klicka på **Spara**.
7. Klicka på **Spara och starta identifiering**.

    ![Lägg till användar konto för virtuell dator](./media/how-to-create-group-machine-dependencies-agentless/add-vm-credential.png)





## <a name="review-and-export-the-inventory"></a>Granska och exportera inventeringen

Om du har angett autentiseringsuppgifter för identifiering av appar när identifieringen har slutförts, kan du granska och exportera program inventeringen i Azure Portal.

1. I **Azure Migrate-servrar** > **Azure Migrate: Server utvärdering**klickar du på det visade antalet för att öppna sidan **identifierade servrar** .

    > [!NOTE]
    > I det här skedet kan du också konfigurera beroende mappning för identifierade datorer, så att du kan visualisera beroenden mellan datorer som du vill utvärdera. [Läs mer](how-to-create-group-machine-dependencies.md).

2. I **program som identifierats**klickar du på det visade antalet.
3. I **program inventering**kan du granska identifierade appar, roller och funktioner.
4. Exportera inventeringen genom att klicka på **Exportera app Inventory**i **identifierade servrar**.

App-inventeringen exporteras och hämtas i Excel-format. **Program inventerings** bladet visar alla appar som identifierats på alla datorer.

## <a name="next-steps"></a>Nästa steg

- [Skapa en utvärdering](how-to-create-assessment.md) av migreringen och flyttningen av de identifierade servrarna.
- Utvärdera en SQL Server databaser med [Azure Migrate: databas utvärdering](https://docs.microsoft.com/sql/dma/dma-assess-sql-data-estate-to-sqldb?view=sql-server-2017).
