---
title: Fastställa orsaker till icke-kompatibilitet
description: När en resurs är icke-kompatibel finns det många möjliga orsaker. Lär dig hur du tar reda på vad som orsakade bristande efterlevnad.
ms.date: 05/20/2020
ms.topic: how-to
ms.openlocfilehash: 05405671900e8734ee1536cfc00fd5203a145124
ms.sourcegitcommit: 1692e86772217fcd36d34914e4fb4868d145687b
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 05/29/2020
ms.locfileid: "84168564"
---
# <a name="determine-causes-of-non-compliance"></a>Fastställa orsaker till icke-kompatibilitet

När en Azure-resurs bedöms vara icke-kompatibel med en princip regel är det bra att förstå vilken del av regeln som resursen inte är kompatibel med. Det är också bra att förstå vilken ändring som har ändrat en tidigare kompatibel resurs för att göra den icke-kompatibel. Det finns två sätt att hitta den här informationen:

> [!div class="checklist"]
> - [Information om efterlevnad](#compliance-details)
> - [Ändrings historik (förhands granskning)](#change-history)

## <a name="compliance-details"></a>Information om efterlevnad

Om en resurs är icke-kompatibel, är kompatibilitetsinformation för resursen tillgängliga **från sidan efterlevnadsprincip** . I fönstret information om efterlevnad finns följande information:

- Resurs information som namn, typ, plats och resurs-ID
- Kompatibilitetstillstånd och tidsstämpel för den senaste utvärderings versionen för den aktuella princip tilldelningen
- En lista över _orsaker_ till att resursen inte uppfyller kraven

> [!IMPORTANT]
> Eftersom kompatibilitetsinformation för en _icke-kompatibel_ resurs visar det aktuella värdet för egenskaperna för den resursen, måste användaren ha **Läs** åtgärd för resurs **typen** . Om den _icke-kompatibla_ resursen till exempel är **Microsoft. Compute/virtualMachines** måste användaren ha åtgärden **Microsoft. Compute/virtualMachines/Read** . Om användaren inte har den nödvändiga åtgärden visas ett åtkomst fel.

Följ dessa steg om du vill visa kompatibilitetsinformation:

1. Starta Azure Policy-tjänsten i Azure Portal genom att klicka på **Alla tjänster** och sedan söka efter och välja **Princip**.

1. På sidan **Översikt** eller **efterlevnad** väljer du en princip i ett **kompatibilitetstillstånd** som _inte är kompatibelt_.

1. På fliken **Resource Compliance (Resource Compliance** ) **på sidan efterlevnadsprincip** högerklickar du på eller väljer tre punkter i en resurs i ett kompatibelt **tillstånd** som _inte är kompatibelt_. Välj sedan **Visa Kompatibilitetsrapport**.

   :::image type="content" source="../media/determine-non-compliance/view-compliance-details.png" alt-text="Visa alternativ för efterlevnad" border="false":::

1. I fönstret **kompatibilitetsinformation** visas information från den senaste utvärderings versionen av resursen till den aktuella princip tilldelningen. I det här exemplet finns fältet **Microsoft. SQL/Servers/version** som _12,0_ medan princip definitionen förväntar sig _14,0_. Om resursen inte är kompatibel av flera orsaker visas varje lista i det här fönstret.

   :::image type="content" source="../media/determine-non-compliance/compliance-details-pane.png" alt-text="Fönstret information om efterlevnad och orsaker till inkompatibilitet" border="false":::

   För en **auditIfNotExists** -eller **deployIfNotExists** -princip definition innehåller informationen **information. Type** -egenskapen och eventuella valfria egenskaper. En lista finns i [auditIfNotExists egenskaper](../concepts/effects.md#auditifnotexists-properties) och [deployIfNotExists egenskaper](../concepts/effects.md#deployifnotexists-properties). Den **senaste utvärderade resursen** är en relaterad resurs från avsnittet **information** i definitionen.

   Exempel på partiell **deployIfNotExists** -definition:

   ```json
   {
       "if": {
           "field": "type",
           "equals": "[parameters('resourceType')]"
       },
       "then": {
           "effect": "DeployIfNotExists",
           "details": {
               "type": "Microsoft.Insights/metricAlerts",
               "existenceCondition": {
                   "field": "name",
                   "equals": "[concat(parameters('alertNamePrefix'), '-', resourcegroup().name, '-', field('name'))]"
               },
               "existenceScope": "subscription",
               "deployment": {
                   ...
               }
           }
       }
   }
   ```

   :::image type="content" source="../media/determine-non-compliance/compliance-details-pane-existence.png" alt-text="Informations fönstret för efterlevnad – * ifNotExists" border="false":::

> [!NOTE]
> Om ett egenskaps värde är _hemligt_ för att skydda data visas asterisker i det aktuella värdet.

Informationen förklarar varför en resurs för närvarande inte är kompatibel, men visas inte när ändringen gjordes till den resurs som gjorde att den blev inkompatibel. Mer information finns i [ändrings historik (förhands granskning)](#change-history) nedan.

### <a name="compliance-reasons"></a>Orsaker till efterlevnad

Följande matris mappar varje tänkbar _orsak_ till det ansvariga [villkoret](../concepts/definition-structure.md#conditions) i princip definitionen:

|Anledning | Villkor |
|-|-|
|Det aktuella värdet måste innehålla målvärdet som en nyckel. |containsKey eller **inte** notContainsKey |
|Det aktuella värdet måste innehålla målvärdet. |innehåller eller **inte** notContains |
|Det aktuella värdet måste vara lika med målvärdet. |lika med eller **inte** notEquals |
|Det aktuella värdet måste vara mindre än målvärdet. |mindre eller **inte** större |
|Det aktuella värdet måste vara större än eller lika med målvärdet. |Större eller **inte** mindre |
|Det aktuella värdet måste vara större än målvärdet. |större eller **icke** -lessOrEquals |
|Det aktuella värdet måste vara mindre än eller lika med målvärdet. |lessOrEquals eller **inte** större |
|Det aktuella värdet måste finnas. |finns |
|Det aktuella värdet måste vara i målvärdet. |i eller **inte** notIn |
|Det aktuella värdet måste vara samma som målvärdet. |gilla eller **inte** notLike |
|Det aktuella värdet måste Skift läges känsligt matcha målvärdet. |matcha eller **not** notMatch |
|Det aktuella värdet måste vara Skift läges okänsligt matcha målvärdet. |matchInsensitively eller **inte** notMatchInsensitively |
|Det aktuella värdet får inte innehålla målvärdet som en nyckel. |notContainsKey eller **inte** containsKey|
|Det aktuella värdet får inte innehålla målvärdet. |notContains eller **inte** innehåller |
|Det aktuella värdet får inte vara samma som målvärdet. |notEquals eller **inte** lika med |
|Det aktuella värdet får inte finnas. |finns **inte**  |
|Det aktuella värdet får inte vara i målvärdet. |notIn eller **inte** i |
|Det aktuella värdet får inte vara samma som målvärdet. |notLike eller **inte** gilla |
|Det aktuella värdet får inte Skift läges känsligt matcha målvärdet. |notMatch eller **not** match |
|Det aktuella värdet får inte Skift läges okänsligt matcha målvärdet. |notMatchInsensitively eller **inte** matchInsensitively |
|Inga relaterade resurser matchar träd informationen i princip definitionen. |En resurs av den typ som definierats i **sedan. details. Type** och relaterad till den resurs som definierats i **IF** -delen i princip regeln finns inte. |

## <a name="compliance-details-for-guest-configuration"></a>Information om efterlevnad för gäst konfiguration

För _auditIfNotExists_ -principer i kategorin _gäst konfiguration_ kan flera inställningar utvärderas i den virtuella datorn och du måste visa information om varje inställning. Om du till exempel granskar en lista över lösen ords principer och bara en av dem har status _inkompatibel_, behöver du veta vilka principer för lösen ord som inte är kompatibla och varför.

Du kanske inte har behörighet att logga in på den virtuella datorn direkt, men du måste rapportera om varför den virtuella datorn är _icke-kompatibel_.

### <a name="azure-portal"></a>Azure Portal

Börja med att följa samma steg i avsnittet ovan för att visa information om efterlevnad av policy.

I fönstret kompatibilitetsinformation klickar du på länken **senaste utvärderade resursen**.

:::image type="content" source="../media/determine-non-compliance/guestconfig-auditifnotexists-compliance.png" alt-text="Visa information om auditIfNotExists-definitioner" border="false":::

På sidan **gäst tilldelning** visas all tillgänglig information om kompatibilitet. Varje rad i vyn representerar en utvärdering som utförts i datorn. I kolumnen **orsak** visas en fras som beskriver varför gäst tilldelningen _inte är kompatibel_. Om du till exempel granskar lösen ords principer visar kolumnen **orsak** att texten innehåller det aktuella värdet för varje inställning.

:::image type="content" source="../media/determine-non-compliance/guestconfig-compliance-details.png" alt-text="Visa information om efterlevnad" border="false":::

### <a name="azure-powershell"></a>Azure PowerShell

Du kan också visa information om efterlevnad från Azure PowerShell. Kontrol lera först att du har modulen gäst konfiguration installerad.

```azurepowershell-interactive
Install-Module Az.GuestConfiguration
```

Du kan visa den aktuella statusen för alla gäst tilldelningar för en virtuell dator med hjälp av följande kommando:

```azurepowershell-interactive
Get-AzVMGuestPolicyReport -ResourceGroupName <resourcegroupname> -VMName <vmname>
```

```output
PolicyDisplayName                                                         ComplianceReasons
-----------------                                                         -----------------
Audit that an application is installed inside Windows VMs                 {[InstalledApplication]bwhitelistedapp}
Audit that an application is not installed inside Windows VMs.            {[InstalledApplication]NotInstalledApplica...
```

Om du bara vill visa _orsaken_ till varför den virtuella datorn är _icke-kompatibel_, returnerar du endast den underordnade egenskapen orsak.

```azurepowershell-interactive
Get-AzVMGuestPolicyReport -ResourceGroupName <resourcegroupname> -VMName <vmname> | % ComplianceReasons | % Reasons | % Reason
```

```output
The following applications are not installed: '<name>'.
```

Du kan också skriva ut en efterlevnadsprincip för gäst tilldelningar i omfånget för datorn. Utdata från det här kommandot innehåller information om varje rapport för den virtuella datorn.

> [!NOTE]
> Utdata kan returnera en stor mängd data. Vi rekommenderar att du lagrar utdata i en variabel.

```azurepowershell-interactive
$guestHistory = Get-AzVMGuestPolicyStatusHistory -ResourceGroupName <resourcegroupname> -VMName <vmname>
$guestHistory
```

```output
PolicyDisplayName                                                         ComplianceStatus ComplianceReasons StartTime              EndTime                VMName LatestRepor
                                                                                                                                                                  tId
-----------------                                                         ---------------- ----------------- ---------              -------                ------ -----------
[Preview]: Audit that an application is installed inside Windows VMs      NonCompliant                       02/10/2019 12:00:38 PM 02/10/2019 12:00:41 PM VM01  ../17fg0...
<truncated>
```

Använd parametern **ShowChanged** för att förenkla den här vyn. Utdata från det här kommandot innehåller bara rapporter som följde en ändring av kompatibilitetsstatus.

```azurepowershell-interactive
$guestHistory = Get-AzVMGuestPolicyStatusHistory -ResourceGroupName <resourcegroupname> -VMName <vmname> -ShowChanged
$guestHistory
```

```output
PolicyDisplayName                                                         ComplianceStatus ComplianceReasons StartTime              EndTime                VMName LatestRepor
                                                                                                                                                                  tId
-----------------                                                         ---------------- ----------------- ---------              -------                ------ -----------
Audit that an application is installed inside Windows VMs                 NonCompliant                       02/10/2019 10:00:38 PM 02/10/2019 10:00:41 PM VM01  ../12ab0...
Audit that an application is installed inside Windows VMs.                Compliant                          02/09/2019 11:00:38 AM 02/09/2019 11:00:39 AM VM01  ../e3665...
Audit that an application is installed inside Windows VMs                 NonCompliant                       02/09/2019 09:00:20 AM 02/09/2019 09:00:23 AM VM01  ../15ze1...
```

## <a name="change-history-preview"></a><a name="change-history"></a>Ändrings historik (förhands granskning)

Som en del av en ny **offentlig för hands version**är de 14 senaste dagarna i ändrings historiken tillgängliga för alla Azure-resurser som stöder [borttagning av fullständigt läge](../../../azure-resource-manager/templates/complete-mode-deletion.md). Ändrings historiken innehåller information om när en ändring upptäcktes och en _visuell skillnad_ för varje ändring. En ändrings identifiering utlöses när Resource Manager-egenskaperna läggs till, tas bort eller ändras.

1. Starta Azure Policy-tjänsten i Azure Portal genom att klicka på **Alla tjänster** och sedan söka efter och välja **Princip**.

1. På sidan **Översikt** eller **efterlevnad** väljer du en princip i valfritt **kompatibilitetstillstånd**.

1. På fliken **Resource Compliance (Resource Compliance** ) **på sidan efterlevnadsprincip** väljer du en resurs.

1. Välj fliken **ändrings historik (förhands granskning)** på sidan **resurs efterlevnad** . En lista över identifierade ändringar, om sådana finns, visas.

   :::image type="content" source="../media/determine-non-compliance/change-history-tab.png" alt-text="Fliken Azure Policy ändrings historik på sidan resurs efterlevnad" border="false":::

1. Välj en av de identifierade ändringarna. Den _visuella differensen_ för resursen visas på sidan **Ändra historik** .

   :::image type="content" source="../media/determine-non-compliance/change-history-visual-diff.png" alt-text="Azure Policy ändra historik för visuell skillnad på ändrings historik Sidan" border="false":::

Den _visuella diff_ -aides för att identifiera ändringar av en resurs. De ändringar som upptäcktes kanske inte är relaterade till resursens aktuella kompatibilitetstillstånd.

Data för ändrings historiken tillhandahålls av [Azure Resource Graph](../../resource-graph/overview.md). Information om hur du frågar den här informationen utanför Azure Portal finns i [Hämta resurs ändringar](../../resource-graph/how-to/get-resource-changes.md).

## <a name="next-steps"></a>Nästa steg

- Granska exempel i [Azure policy exempel](../samples/index.md).
- Granska [Azure Policy-definitionsstrukturen](../concepts/definition-structure.md).
- Granska [Förstå policy-effekter](../concepts/effects.md).
- Lär dig att [program mässigt skapa principer](programmatically-create.md).
- Lär dig hur du [hämtar efterlevnadsprinciper](get-compliance-data.md).
- Lär dig hur du [åtgärdar icke-kompatibla resurser](remediate-resources.md).
- Granska en hanterings grupp med [organisera dina resurser med Azures hanterings grupper](../../management-groups/overview.md).
