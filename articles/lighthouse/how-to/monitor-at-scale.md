---
title: Övervaka delegerade resurser i stor skala
description: Lär dig hur du effektivt använder Azure Monitor loggar på ett skalbart sätt över de kund innehavare som du hanterar.
ms.date: 02/03/2020
ms.topic: conceptual
ms.openlocfilehash: 9a600cec524c9f715ada9da09dea35f3baa98151
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/28/2020
ms.locfileid: "80985209"
---
# <a name="monitor-delegated-resources-at-scale"></a>Övervaka delegerade resurser i stor skala

Som tjänst leverantör kan du ha registrerat flera kund innehavare för Azure-delegerad resurs hantering. Med [Azure Lighthouse](../overview.md) kan tjänst leverantörer utföra åtgärder i skala över flera klienter samtidigt, vilket gör hanterings uppgifter mer effektiva.

Det här avsnittet visar hur du använder [Azure Monitor loggar](../../azure-monitor/platform/data-platform-logs.md) på ett skalbart sätt över de kund innehavare som du hanterar.

## <a name="create-log-analytics-workspaces"></a>Skapa Log Analytics arbets ytor

För att kunna samla in data måste du skapa Log Analytics arbets ytor. Dessa Log Analytics arbets ytor är unika miljöer för data som samlas in av Azure Monitor. Varje arbets yta har sin egen data lagrings plats och konfiguration, och data källor och lösningar har kon figurer ATS för att lagra data i en viss arbets yta.

Vi rekommenderar att du skapar dessa arbets ytor direkt i kundens klienter. På så sätt förblir deras data i sina klienter i stället för att exporteras till dina. Det möjliggör också centraliserad övervakning av resurser eller tjänster som stöds av Log Analytics, vilket ger dig mer flexibilitet för vilka typer av data du övervakar.

Du kan skapa en Log Analytics arbets yta genom att använda [Azure Portal](../../azure-monitor/learn/quick-create-workspace.md), med hjälp av [Azure CLI](../../azure-monitor/learn/quick-create-workspace-cli.md)eller genom att använda [Azure PowerShell](../../azure-monitor/learn/quick-create-workspace-posh.md).

## <a name="deploy-policies-that-log-data"></a>Distribuera principer som loggar data

När du har skapat dina Log Analytics arbets ytor kan du distribuera [Azure policy](../../governance/policy/index.yml) över dina kundhierarkier så att diagnostikdata skickas till lämplig arbets yta i varje klient. De exakta principer som du distribuerar kan variera beroende på de resurs typer som du vill övervaka.

Mer information om hur du skapar principer finns i [Självstudier: skapa och hantera principer för att genomdriva efterlevnad](../../governance/policy/tutorials/create-and-manage.md). Det här [Community-verktyget](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/tools/azure-diagnostics-policy-generator) ger ett skript som hjälper dig att skapa principer för att övervaka de resurs typer som du väljer.

När du har bestämt vilka principer som ska distribueras kan du [distribuera dem till dina delegerade prenumerationer i stor skala](policy-at-scale.md).

## <a name="analyze-the-gathered-data"></a>Analysera insamlade data

När du har distribuerat dina principer kommer data att loggas i Log Analytics arbets ytor som du har skapat i varje kund klient organisation. För att få insikter över alla hanterade kunder kan du använda verktyg som [Azure Monitor arbets böcker](../../azure-monitor/platform/workbooks-overview.md) för att samla in och analysera information från flera data källor.

## <a name="next-steps"></a>Nästa steg

- Läs mer om [Azure Monitor](../../azure-monitor/index.yml).
- Lär dig mer om [Azure Monitor loggar](../../azure-monitor/platform/data-platform-logs.md).
- Lär dig mer om [hanterings upplevelser mellan flera innehavare](../concepts/cross-tenant-management-experience.md).
