---
title: Reparera inkompatibla Azure Automation tillstånds konfigurations servrar
description: Den här artikeln beskriver hur du tillämpar konfigurationer på begäran på servrar där konfigurations status har Aktiver ATS.
services: automation
ms.service: automation
ms.subservice: dsc
author: mgreenegit
ms.author: migreene
ms.topic: conceptual
ms.date: 07/17/2019
manager: nirb
ms.openlocfilehash: ff785bf3ace7c65f83fe8e505f0544edd24776d8
ms.sourcegitcommit: 0b80a5802343ea769a91f91a8cdbdf1b67a932d3
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 05/25/2020
ms.locfileid: "83836863"
---
# <a name="remediate-noncompliant-azure-automation-state-configuration-servers"></a>Reparera inkompatibla Azure Automation tillstånds konfigurations servrar

När servrar registreras med Azure Automation tillstånds konfiguration är konfigurations läget inställt på `ApplyOnly` , `ApplyandMonitor` eller `ApplyAndAutoCorrect` . Om läget inte är inställt på `ApplyAndAutoCorrect` är servrar som reser från ett kompatibelt tillstånd av någon anledning fortfarande inkompatibla tills de har korrigerats manuellt.

Azure Compute erbjuder en funktion med namnet kör kommando som gör att kunder kan köra skript på virtuella datorer.
Det här dokumentet innehåller exempel skript för den här funktionen när du korrigerar konfigurations avvikelser manuellt.

## <a name="correct-drift-of-windows-virtual-machines-using-powershell"></a>Korrigera virtuella Windows-datorer med PowerShell

Du kan korrigera drift av virtuella Windows-datorer med hjälp av `Run` kommando funktionen. Se [kör PowerShell-skript i din virtuella Windows-dator med kommandot kör](/azure/virtual-machines/windows/run-command).

Använd cmdleten [Update-DscConfiguration](/powershell/module/psdesiredstateconfiguration/update-dscconfiguration) om du vill tvinga fram en nod för konfiguration av Azure Automation tillstånd för att ladda ned den senaste konfigurationen och tillämpa den.

```powershell
Update-DscConfiguration -Wait -Verbose
```

## <a name="correct-drift-of-linux-virtual-machines"></a>Korrekt drift av virtuella Linux-datorer

För virtuella Linux-datorer har du inte möjlighet att använda `Run` kommandot. Du kan bara korrigera driften för dessa datorer genom att upprepa registrerings processen. 

För Azure-noder kan du korrigera avvikelser från Azure Portal eller genom att använda cmdlets för AZ-moduler. Information om den här processen finns dokumenterad i [Aktivera en virtuell dator med hjälp av Azure Portal](automation-dsc-onboarding.md#enable-a-vm-using-azure-portal).

För Hybrid-noder kan du korrigera driften med python-skripten. Se [utföra DSC-åtgärder från Linux-datorn](https://github.com/Microsoft/PowerShell-DSC-for-Linux#performing-dsc-operations-from-the-linux-computer).

## <a name="next-steps"></a>Nästa steg

- En PowerShell-cmdlet-referens finns i [AZ. Automation](https://docs.microsoft.com/powershell/module/az.automation/?view=azps-3.7.0#automation).
- Om du vill se ett exempel på hur du använder Azure Automation tillstånds konfiguration i en pipeline för kontinuerlig distribution, se [Konfigurera kontinuerlig distribution med choklad](automation-dsc-cd-chocolatey.md).