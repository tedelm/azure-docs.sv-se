---
title: Köra skript i en virtuell Azure Linux-dator
description: I det här avsnittet beskrivs hur du kör skript på en virtuell dator
services: automation
ms.service: virtual-machines-linux
author: bobbytreed
ms.author: robreed
ms.date: 05/02/2018
ms.topic: article
ms.openlocfilehash: 9dbfb3e76fe1c0e80dd8020b79f4edd6d32a24a3
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/28/2020
ms.locfileid: "81758594"
---
# <a name="run-scripts-in-your-linux-vm"></a>Kör skript i din virtuella Linux-dator

Om du vill automatisera uppgifter eller felsöka problem kan du behöva köra kommandon i en virtuell dator. Följande artikel innehåller en kort översikt över de funktioner som är tillgängliga för att köra skript och kommandon i de virtuella datorerna.

## <a name="custom-script-extension"></a>Anpassat skripttillägg

Det [anpassade skript tillägget](../extensions/custom-script-linux.md) används främst för konfiguration och program varu installation efter distribution.

* Hämta och kör skript på virtuella Azure-datorer.
* Kan köras med Azure Resource Manager mallar, Azure CLI, REST API, PowerShell eller Azure Portal.
* Skriptfiler kan laddas ned från Azure Storage eller GitHub, eller tillhandahållas från din dator när de körs från Azure Portal.
* Kör PowerShell-skript i Windows-datorer och bash-skript på Linux-datorer.
* Användbart för konfiguration av distribution, program varu installation och andra konfigurations-eller hanterings uppgifter.

## <a name="run-command"></a>Kör kommando

[Kommando funktionen kör](run-command.md) möjliggör hantering av virtuella datorer och program och fel sökning med skript och är tillgänglig även om datorn inte kan nås, till exempel om gäst brand väggen inte har RDP-eller SSH-porten öppen.

* Kör skript på virtuella Azure-datorer.
* Kan köras med [Azure Portal](run-command.md), [REST API](/rest/api/compute/virtual%20machines%20run%20commands/runcommand), [Azure CLI](/cli/azure/vm/run-command?view=azure-cli-latest#az-vm-run-command-invoke)eller [PowerShell](https://docs.microsoft.com/powershell/module/az.compute/invoke-azvmruncommand)
* Kör snabbt ett skript och visa utdata och upprepa efter behov i Azure Portal.
* Skriptet kan skrivas in direkt eller så kan du köra ett av de inbyggda skripten.
* Kör PowerShell-skript i Windows-datorer och bash-skript på Linux-datorer.
* Användbart för hantering av virtuella datorer och program och för att köra skript på virtuella datorer som inte kan kontaktas.

## <a name="hybrid-runbook-worker"></a>Hybrid Runbook Worker

[Hybrid Runbook Worker](../../automation/automation-hybrid-runbook-worker.md) tillhandahåller allmän hantering av datorer, program och miljöer med användarens anpassade skript lagrade i ett Automation-konto.

* Kör skript på Azure-datorer och datorer som inte är Azure-datorer.
* Kan köras med Azure Portal, Azure CLI, REST API, PowerShell, webhook.
* Skript som lagras och hanteras i ett Automation-konto.
* Kör PowerShell, PowerShell-arbetsflöde, python eller grafiska runbooks
* Ingen tids gräns för skript körnings tid.
* Flera skript kan köras samtidigt.
* Fullständigt utdata för skript returneras och lagras.
* Jobb historik tillgängligt i 90 dagar.
* Skript kan köras som lokalt system eller med användar uppgifter som angetts av användaren.
* Kräver [manuell installation](../../automation/automation-windows-hrw-install.md)

## <a name="serial-console"></a>Seriekonsol

[Seriell konsol](serial-console.md) ger direkt åtkomst till en virtuell dator, på samma sätt som ett tangent bord som är anslutet till den virtuella datorn.

* Kör kommandon i Azure Virtual Machines.
* Kan köras med hjälp av en text-baserad konsol till datorn i Azure Portal.
* Logga in på datorn med ett lokalt användar konto.
* Användbart när åtkomst till den virtuella datorn krävs, oavsett datorns nätverks-eller operativ system tillstånd.

## <a name="next-steps"></a>Nästa steg

Lär dig mer om de olika funktionerna som är tillgängliga för att köra skript och kommandon i dina virtuella datorer.

* [Anpassat skripttillägg](../extensions/custom-script-linux.md)
* [Kör kommando](run-command.md)
* [Hybrid Runbook Worker](../../automation/automation-hybrid-runbook-worker.md)
* [Seriell konsol](serial-console.md)