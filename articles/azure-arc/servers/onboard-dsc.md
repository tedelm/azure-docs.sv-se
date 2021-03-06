---
title: Installera den anslutna dator agenten med Windows PowerShell DSC
description: I den här artikeln får du lära dig hur du ansluter datorer till Azure med hjälp av Azure Arc for Servers (för hands version) med hjälp av Windows PowerShell DSC.
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-servers
author: mgoedtel
ms.author: magoedte
ms.date: 03/12/2020
ms.topic: conceptual
ms.openlocfilehash: 1fb64463b0372202adb04c2deb304c389c7773b8
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/28/2020
ms.locfileid: "79164688"
---
# <a name="how-to-install-the-connected-machine-agent-using-windows-powershell-dsc"></a>Så här installerar du den anslutna dator agenten med hjälp av Windows PowerShell DSC

Med hjälp av [Windows PowerShell Desired State Configuration](https://docs.microsoft.com/powershell/scripting/dsc/getting-started/winGettingStarted?view=powershell-7) (DSC) kan du automatisera program varu installation och konfiguration för en Windows-dator. Den här artikeln beskriver hur du använder DSC för att installera Azure-bågen för servrar som är anslutna till dator agent på Hybrid Windows-datorer.

## <a name="requirements"></a>Krav

- Windows PowerShell version 4,0 eller senare

- [AzureConnectedMachineDsc](https://www.powershellgallery.com/packages/AzureConnectedMachineDsc/1.0.1.0) DSC-modulen

- Ett huvud namn för tjänsten som ansluter datorerna till Azure-bågen för servrar som inte är interaktivt. Följ stegen i avsnittet [skapa ett huvud namn för tjänsten för onboarding i skala](onboard-service-principal.md#create-a-service-principal-for-onboarding-at-scale) om du inte redan har skapat ett tjänst huvud namn för Arc för servrar.

## <a name="install-the-connectedmachine-dsc-module"></a>Installera ConnectedMachine DSC-modulen

1. Om du vill installera modulen manuellt kan du ladda ned käll koden och packa upp innehållet i projekt katalogen till `$env:ProgramFiles\WindowsPowerShell\Modules folder`. Eller kör följande kommando för att installera från PowerShell-galleriet med hjälp av PowerShellGet (i PowerShell 5,0):

    ```powershell
    Find-Module -Name AzureConnectedMachineDsc -Repository PSGallery | Install-Module
    ```

2. Bekräfta installationen genom att köra följande kommando och se till att du ser de tillgängliga Azure-resurserna för Azure Connected Machine.

    ```powershell
    Get-DscResource -Module AzureConnectedMachineDsc
    ```

   I utdata bör du se något som liknar följande:

   ![Bekräftelse av installations exempel för ansluten Machine DSC-modul](./media/onboard-dsc/confirm-module-installation.png)

## <a name="install-the-agent-and-connect-to-azure"></a>Installera agenten och Anslut till Azure

Resurserna i den här modulen är utformade för att hantera konfigurationen av Azure-anslutna dator agenter. Det `AzureConnectedMachineDsc\examples` finns också ett PowerShell- `AzureConnectedMachineAgent.ps1`skript som finns i mappen. Den använder community-resurser för att automatisera nedladdning och installation och upprätta en anslutning till Azure-bågen. Det här skriptet utför liknande steg som beskrivs i [ansluta hybrid datorer till Azure från Azure Portal](onboard-portal.md) artikeln.

Om datorn behöver kommunicera via en proxyserver till tjänsten måste du köra ett kommando som beskrivs [här](onboard-portal.md#configure-the-agent-proxy-setting)när du har installerat agenten. Detta anger proxyserverns system miljö variabel `https_proxy`. I stället för att köra kommandot manuellt kan du utföra det här steget med DSC genom att använda [ComputeManagementDsc](https://www.powershellgallery.com/packages/ComputerManagementDsc/6.0.0.0) -modulen.

>[!NOTE]
>För att DSC ska kunna köras måste Windows konfigureras för att ta emot PowerShell-fjärrkommandon även när du kör en localhost-konfiguration. För att enkelt konfigurera din miljö på rätt sätt `Set-WsManQuickConfig -Force` kan du bara köra i en upphöjd PowerShell-Terminal.
>

Konfigurations dokument (MOF-filer) kan tillämpas på datorn med hjälp `Start-DscConfiguration` av cmdleten.

Följande är de parametrar du skickar till PowerShell-skriptet som ska användas.

- `TenantId`: Den unika identifieraren (GUID) som representerar din dedikerade instans av Azure AD.

- `SubscriptionId`: Prenumerations-ID (GUID) för den Azure-prenumeration som du vill ha datorerna i.

- `ResourceGroup`: Namnet på den resurs grupp där du vill att dina anslutna datorer ska tillhöra.

- `Location`: Se [Azure-regioner som stöds](overview.md#supported-regions). Den här platsen kan vara samma eller olika, som resurs gruppens plats.

- `Tags`: Sträng mat ris med taggar som ska tillämpas på den anslutna dator resursen.

- `Credential`: Ett PowerShell-Credential med **ApplicationId** och **lösen ord** som används för att registrera datorer i skala med hjälp av ett [huvud namn för tjänsten](onboard-service-principal.md). 

1. I en PowerShell-konsol navigerar du till mappen där du sparade `.ps1` filen.

2. Kör följande PowerShell-kommandon för att kompilera MOF-dokumentet (information om hur du kompilerar DSC-konfigurationer finns i [DSC-konfigurationer](https://docs.microsoft.com/powershell/scripting/dsc/configurations/configurations?view=powershell-7):

    ```powershell
    .\`AzureConnectedMachineAgent.ps1 -TenantId <TenantId GUID> -SubscriptionId <SubscriptionId GUID> -ResourceGroup '<ResourceGroupName>' -Location '<LocationName>' -Tags '<Tag>' -Credential <psCredential>
    ```

3. Då skapas en `localhost.mof file` i en ny mapp med namnet `C:\dsc`.

När du har installerat agenten och konfigurerat den för att ansluta till Azure Arc for Servers (för hands version) går du till Azure Portal för att kontrol lera att servern har anslutits. Visa dina datorer i [Azure Portal](https://aka.ms/hybridmachineportal).

## <a name="adding-to-existing-configurations"></a>Lägga till i befintliga konfigurationer

Den här resursen kan läggas till i befintliga DSC-konfigurationer för att representera en slutpunkt-till-slutpunkt-konfiguration för en dator. Du kanske exempelvis vill lägga till den här resursen i en konfiguration som anger säkra inställningar för operativ systemet.

[CompsiteResource](https://www.powershellgallery.com/packages/compositeresource/0.4.0) -modulen från PowerShell-galleriet kan användas för att skapa en [sammansatt resurs](https://docs.microsoft.com/powershell/scripting/dsc/resources/authoringResourceComposite?view=powershell-7) av exempel konfigurationen för att ytterligare förenkla kombinera konfigurationer.

## <a name="next-steps"></a>Nästa steg

- Lär dig hur du hanterar din dator med hjälp av [Azure policy](../../governance/policy/overview.md), till exempel för [gäst konfiguration](../../governance/policy/concepts/guest-configuration.md)av virtuella datorer, verifiera att datorn rapporterar till den förväntade Log Analytics arbets ytan, aktivera övervakning med [Azure monitor med virtuella datorer](../../azure-monitor/insights/vminsights-enable-at-scale-policy.md)och mycket mer.

- Läs mer om den [Log Analytics agenten](../../azure-monitor/platform/log-analytics-agent.md). Log Analytics agent för Windows och Linux krävs om du vill övervaka operativ system och arbets belastningar som körs på datorn proaktivt, hantera den med hjälp av Automation-runbooks eller lösningar som Uppdateringshantering eller använda andra Azure-tjänster som [Azure Security Center](../../security-center/security-center-intro.md).