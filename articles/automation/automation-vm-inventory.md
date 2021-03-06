---
title: Hantera Azure Automation inventerings insamling från virtuella datorer | Microsoft Docs
description: Den här artikeln beskriver hur du hanterar inventerings insamling från virtuella datorer.
services: automation
ms.subservice: change-inventory-management
keywords: inventory, automation, change, tracking
ms.date: 01/28/2020
ms.topic: conceptual
ms.openlocfilehash: d237b016b8f3430ed0b28becd2712bd0c41d17b4
ms.sourcegitcommit: 0b80a5802343ea769a91f91a8cdbdf1b67a932d3
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 05/25/2020
ms.locfileid: "83830624"
---
# <a name="manage-inventory-collection-from-vms"></a>Hantera inventeringsinsamling från virtuella datorer

Du kan aktivera inventerings spårning för en virtuell Azure-dator från datorns resurs sida. Du kan samla in och visa följande inventerings information på dina datorer:

- Windows-uppdateringar, Windows-program, tjänster, filer och register nycklar
- Linux-programpaket, bakgrunds program och filer

Azure Automation Ändringsspårning och inventering ger ett webbläsarbaserat användar gränssnitt för att konfigurera och konfigurera inventerings samlingen.

## <a name="before-you-begin"></a>Innan du börjar

Om du inte har en Azure-prenumeration kan du [skapa ett kostnads fritt konto](https://azure.microsoft.com/free/).

Den här artikeln förutsätter att du har en virtuell dator som du kan aktivera med Ändringsspårning och inventering. Om du inte har en virtuell Azure-dator kan du [skapa en virtuell dator](../virtual-machines/windows/quick-create-portal.md).

## <a name="sign-in-to-the-azure-portal"></a>Logga in på Azure Portal

Logga in på [Azure-portalen](https://portal.azure.com/).

## <a name="enable-inventory-collection-from-the-vm-resource-page"></a>Aktivera inventerings samling från resurs sidan för den virtuella datorn

1. I rutan till vänster på Azure Portal väljer du **Virtuella datorer**.
2. Välj en dator i listan över virtuella datorer.
3. Välj **inventering**under **åtgärder**på **resurs** -menyn.
4. Välj en Log Analytics arbets yta för att lagra dina data loggar.
    Om det inte finns några tillgängliga arbetsytor för dig för en regionen uppmanas du att skapa en standardarbetsyta och ett Automation-konto.
5. Om du vill börja aktivera datorn väljer du **Aktivera**.

   ![Visa integreringsalternativ](./media/automation-vm-inventory/inventory-onboarding-options.png)

    Ett statusfält meddelar dig att funktionen Ändringsspårning och inventering är aktive rad. Processen kan ta upp till 15 minuter. Under den här tiden kan du stänga fönstret, eller så kan du hålla det öppet och meddela dig när funktionen är aktive rad. Du kan övervaka distributionsstatusen via meddelandefönstret.

   ![Visa inventering](./media/automation-vm-inventory/inventory-onboarded.png)

När distributionen är klar försvinner statusfältet. Systemet samlar fortfarande in inventeringsdata och dessa data kanske inte visas än. En fullständig insamling av data kan ta 24 timmar.

## <a name="configure-your-inventory-settings"></a>Konfigurera inventeringsinställningarna

Som standard konfigureras programvara, Windows-tjänster och Linux-daemon för insamling. Om du vill inventera Windows-registret och filer kan du konfigurera inställningarna för inventeringsinsamling.

1. På sidan inventering klickar du på **Redigera inställningar** högst upp på sidan.
2. Om du vill lägga till en ny samlings inställning går du till inställnings kategorin som du vill lägga till genom att välja fliken **Windows-register**, **Windows-filer**eller Linux- **filer** .
3. Välj lämplig kategori och klicka på **Lägg till** överst på sidan.

I följande avsnitt finns information om varje egenskap som kan konfigureras för olika kategorier.

### <a name="windows-registry"></a>Windows-registret

|Egenskap  |Beskrivning  |
|---------|---------|
|Enabled     | Fastställer om inställningen tillämpas        |
|Objektnamn     | Eget namn på filen som ska spåras        |
|Grupp     | Ett gruppnamn för att gruppera filer logiskt        |
|Windows-registernyckel   | Sökvägen för att söka efter filen Till exempel: ”HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\User Shell Folders\Common Startup”      |

### <a name="windows-files"></a>Windows-filer

|Egenskap  |Beskrivning  |
|---------|---------|
|Enabled     | Sant om inställningen tillämpas och annars FALSE.        |
|Objektnamn     | Det egna namnet på filen som ska spåras.        |
|Grupp     | Ett grupp namn för logisk gruppering av filer.       |
|Ange sökväg     | Sökvägen för att söka efter filen, till exempel **c:\temp\myfile.txt**.

### <a name="linux-files"></a>Linux-filer

|Egenskap  |Beskrivning  |
|---------|---------|
|Enabled     | Sant om inställningen tillämpas och annars FALSE.        |
|Objektnamn     | Det egna namnet på filen som ska spåras.        |
|Grupp     | Ett grupp namn för logisk gruppering av filer.        |
|Ange sökväg     | Sökvägen för att söka efter filen, till exempel **/etc/*. conf**.       |
|Sökvägstyp     | Den typ av objekt som ska spåras. Värden är fil och katalog.        |
|Rekursion     | Sant om rekursion används vid sökning efter objektet som ska spåras och falskt annars.        |
|Använda Sudo     | Sant om sudo används vid sökning efter objektet och falskt annars.         |
|Länkar     | Värde som anger hur symboliska länkar hanteras när du bläddrar i kataloger. Möjliga värden: <br> Ignorera – Ignorerar symboliska länkar och inkluderar inte refererade filer/kataloger<br>Följ – Följer de symboliska länkarna under rekursion och inkluderar refererade filer/kataloger<br>Hantera – Följer de symboliska länkarna och tillåter ändring av behandling av returnerat innehåll      |

## <a name="manage-machine-groups"></a>Hantera dator grupper

Med inventering kan du skapa och Visa dator grupper i Azure Monitor loggar. Dator grupper är samlingar av datorer som definieras av en fråga i Azure Monitor loggar.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

Om du vill visa dator grupper väljer du fliken **dator grupper** på sidan inventering.

![Visa dator grupper på inventerings Sidan](./media/automation-vm-inventory/inventory-machine-groups.png)

Om du väljer en dator grupp i listan öppnas sidan dator grupper. Den här sidan visar information om dator gruppen. Den här informationen omfattar Log Analytics-frågan som används för att definiera gruppen. Längst ned på sidan finns en lista över de datorer som ingår i gruppen.

![Visa dator grupp Sidan](./media/automation-vm-inventory/machine-group-page.png)

Klona dator gruppen genom att klicka på **+ klona** . Du måste ge gruppen ett nytt namn och alias för gruppen. Definitionen kan ändras för tillfället. När du har ändrat frågan klickar du på **validera fråga** för att förhandsgranska de datorer som ska väljas. När du är nöjd med gruppen klickar du på **skapa** för att skapa dator gruppen.

Om du vill skapa en ny dator grupp klickar du på **+ skapa en dator grupp**. Den här knappen öppnar sidan **skapa en dator grupp** där du kan definiera den nya gruppen. Skapa gruppen genom att klicka på **Skapa**.

![Skapa ny dator grupp](./media/automation-vm-inventory/create-new-group.png)

## <a name="disconnect-your-vm-from-management"></a>Koppla bort den virtuella datorn från hantering

Ta bort den virtuella datorn från Ändringsspårning och inventerings hantering:

1. I den vänstra rutan i Azure Portal väljer du **Log Analytics**och väljer sedan den arbets yta som du använde när du aktiverade den virtuella datorn för ändringsspårning och inventering.
2. Öppna menyn **resurs** på sidan Log Analytics.
3. Välj **Virtual Machines** under **arbets ytans data källor**.
4. I listan väljer du den virtuella dator som du vill koppla från. Datorn har en grön bock bredvid **den här arbets ytan** i kolumnen **OMS-anslutning** .

   >[!NOTE]
   >Operations Management Suite (OMS) kallas nu Azure Monitor loggar.
   
5. Klicka på **Koppla från**överst på nästa sida.
6. I bekräftelse fönstret klickar du på **Ja** för att koppla bort datorn från hantering.

## <a name="next-steps"></a>Nästa steg

* Information om hur du arbetar med funktionen finns i [hantera ändringsspårning och inventering](change-tracking-file-contents.md).
* Om du vill veta mer om spårning av program varu ändringar, se [spåra program varu ändringar i din miljö med ändringsspårning](../log-analytics/log-analytics-change-tracking.md).
* Information om hur du felsöker allmänna problem med funktionen finns i [felsöka ändringsspårning-och inventerings problem](troubleshoot/change-tracking.md).