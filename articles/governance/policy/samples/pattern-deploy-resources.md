---
title: 'Mönster: distribuera resurser med en princip definition'
description: Detta Azure Policy mönster innehåller ett exempel på hur du distribuerar resurser med en princip definition.
ms.date: 01/31/2020
ms.topic: sample
ms.openlocfilehash: a8b6528afbd21c7c667e48965574c9b48c403654
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/29/2020
ms.locfileid: "77172676"
---
# <a name="azure-policy-pattern-deploy-resources"></a>Azure Policy mönster: distribuera resurser

Med [deployIfNotExists](../concepts/effects.md#deployifnotexists) -effekterna kan du distribuera en [Azure Resource Manager-mall](../../../azure-resource-manager/templates/overview.md) när du skapar eller uppdaterar en resurs som inte är kompatibel. Den här metoden kan föredras för att använda [neka](../concepts/effects.md#deny) -åtgärden eftersom den gör att resurser kan fortsätta att skapas, men se till att ändringarna blir kompatibla.

## <a name="sample-policy-definition"></a>Exempel på princip definition

Den här princip definitionen använder **fält** operatorn för att `type` utvärdera resursen som skapats eller uppdaterats. När resursen är en _Microsoft. Network/virtualNetworks_söker principen efter en nätverks övervakare på platsen för den nya eller uppdaterade resursen. Om det inte finns någon matchande nätverks övervakare distribueras Resource Manager-mallen för att skapa den resurs som saknas.

:::code language="json" source="~/policy-templates/patterns/pattern-deploy-resources.json":::

### <a name="explanation"></a>Förklaring

#### <a name="existencecondition"></a>existenceCondition

:::code language="json" source="~/policy-templates/patterns/pattern-deploy-resources.json" range="18-23":::

**Egenskaperna. policyRule. then. information** block visar Azure policy vad som ska sökas efter den skapade eller uppdaterade resursen i **egenskaperna. policyRule. if** -block. I det här exemplet måste en nätverks övervakare i resurs gruppen **networkWatcherRG** finnas med **fältet** `location` som motsvarar platsen för den nya eller uppdaterade resursen. Med hjälp `field()` av funktionen kan **existenceCondition** få åtkomst till egenskaper för den nya eller uppdaterade resursen, särskilt `location` egenskapen.

#### <a name="roledefinitionids"></a>roleDefinitionIds

:::code language="json" source="~/policy-templates/patterns/pattern-deploy-resources.json" range="24-26":::

**RoleDefinitionIds** _mat ris_ egenskap i **egenskaperna. policyRule. then. information** -blocket anger princip definitionen som ger den hanterade identiteten behörighet att distribuera den inkluderade Resource Manager-mallen. Den här egenskapen måste anges till att omfatta roller som har de behörigheter som krävs för mallen, men bör använda begreppet "princip för minsta behörighet" och endast ha de nödvändiga åtgärderna och inget annat.

#### <a name="deployment-template"></a>Distributionsmall

**Distributions** delen av princip definitionen har ett **egenskaps** block som definierar de tre huvud komponenterna:

- **läge** – den här egenskapen anger mallens [distributions läge](../../../azure-resource-manager/templates/deployment-modes.md) .

- **mall** – den här egenskapen inkluderar själva mallen. I det här exemplet anger **platsen** för den nya nätverks övervaknings resursen platsen.

  :::code language="json" source="~/policy-templates/patterns/pattern-deploy-resources.json" range="30-44":::
  
- **parametrar** – den här egenskapen definierar parametrar som har angetts för **mallen**. Parameter namnen måste matcha vad som definieras i **mallen**. I det här exemplet heter parametern **plats** som ska matchas. Värdet för **location** använder `field()` funktionen igen för att hämta värdet för den utvärderade resursen, vilket är det virtuella nätverket i **policyRule. if** -block.

  :::code language="json" source="~/policy-templates/patterns/pattern-deploy-resources.json" range="45-49":::

## <a name="next-steps"></a>Nästa steg

- Granska andra [mönster och inbyggda definitioner](./index.md).
- Granska [Azure Policy-definitionsstrukturen](../concepts/definition-structure.md).
- Granska [Förstå policy-effekter](../concepts/effects.md).