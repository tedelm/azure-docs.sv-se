---
title: Lär dig att granska innehållet i virtuella datorer
description: Lär dig hur Azure Policy använder gäst konfigurations agenten för att granska inställningar i virtuella datorer.
ms.date: 05/20/2020
ms.topic: conceptual
ms.openlocfilehash: 6ff24f14281712497798f2c5231a8d98d7d89055
ms.sourcegitcommit: 50673ecc5bf8b443491b763b5f287dde046fdd31
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 05/20/2020
ms.locfileid: "83684290"
---
# <a name="understand-azure-policys-guest-configuration"></a>Förstå Azure Policys gäst konfiguration

Utöver att granska och [Reparera](../how-to/remediate-resources.md) Azure-resurser kan Azure policy granska inställningarna i en dator. Verifieringen utförs av gästkonfigurationstillägget och klienten. Tillägget kontrollerar inställningar via klienten, till exempel:

- Operativ systemets konfiguration
- Programkonfiguration eller förekomst
- Miljöinställningar

För närvarande granskar de flesta Azure Policy principer för gäst konfiguration bara inställningarna på datorn.
De använder inte konfigurationer. Undantaget är en inbyggd princip som [refereras nedan](#applying-configurations-using-guest-configuration).

## <a name="resource-provider"></a>Resursprovider

Innan du kan använda gäst konfiguration måste du registrera resurs leverantören. Resurs leverantören registreras automatiskt om tilldelning av en princip för gäst konfiguration görs via portalen. Du kan registrera dig manuellt via [portalen](../../../azure-resource-manager/management/resource-providers-and-types.md#azure-portal), [Azure POWERSHELL](../../../azure-resource-manager/management/resource-providers-and-types.md#azure-powershell)eller [Azure CLI](../../../azure-resource-manager/management/resource-providers-and-types.md#azure-cli).

## <a name="extension-and-client"></a>Tillägg och klient

Om du vill granska inställningarna i en dator är ett [tillägg för virtuell dator](../../../virtual-machines/extensions/overview.md) aktiverat. Tillägget hämtar tillämplig princip tilldelning och motsvarande konfigurations definition.

> [!IMPORTANT]
> Gäst konfigurations tillägget krävs för att utföra granskningar på virtuella Azure-datorer. Tilldela följande princip definitioner för att distribuera tillägget i skala: 
>  - [Distribuera krav för att aktivera principen för gäst konfiguration på virtuella Windows-datorer.](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F0ecd903d-91e7-4726-83d3-a229d7f2e293)
>  - [Distribuera krav för att aktivera principen för gäst konfiguration på virtuella Linux-datorer.](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Ffb27e9e0-526e-4ae1-89f2-a2a0bf0f8a50)

### <a name="limits-set-on-the-extension"></a>Begränsningar som angetts för tillägget

Om du vill begränsa tillägget från att påverka program som körs på datorn får gäst konfigurationen inte överstiga mer än 5% CPU. Den här begränsningen finns för både inbyggda och anpassade definitioner.

### <a name="validation-tools"></a>Verifierings verktyg

I datorn använder gäst konfigurations klienten lokala verktyg för att köra granskningen.

I följande tabell visas en lista över de lokala verktyg som används för varje operativ system som stöds:

|Operativsystem|Validerings verktyg|Anteckningar|
|-|-|-|
|Windows|[Windows PowerShell Desired State Configuration](/powershell/scripting/dsc/overview/overview) v2| |
|Linux|[Chefs INSPEC](https://www.chef.io/inspec/)| Om ruby och python inte finns på datorn installeras de av gäst konfigurations tillägget. |

### <a name="validation-frequency"></a>Validerings frekvens

Klienten för gäst konfiguration söker efter nytt innehåll var 5: e minut. När en gäst tilldelning har tagits emot, kontrol leras inställningarna för den konfigurationen på 15 minuters intervall. Resultat skickas till resurs leverantören för gäst konfigurationen när granskningen är klar. När en [utlösare](../how-to/get-compliance-data.md#evaluation-triggers) för princip utvärdering inträffar skrivs datorns tillstånd till resurs leverantören för gäst konfiguration. Den här uppdateringen gör att Azure Policy utvärdera Azure Resource Manager egenskaper. En utvärdering på begäran Azure Policy hämtar det senaste värdet från resurs leverantören för gäst konfigurationen. Den utlöser dock inte en ny granskning av konfigurationen på datorn.

## <a name="supported-client-types"></a>Klient typer som stöds

Konfigurations principer för gäster är inklusive nya versioner. Äldre versioner av operativ system som är tillgängliga på Azure Marketplace ingår inte om gäst konfigurations agenten inte är kompatibel.
I följande tabell visas en lista över operativ system som stöds på Azure-avbildningar:

|Publisher|Name|Versioner|
|-|-|-|
|Canonical|Ubuntu Server|14,04 och senare|
|Credativ|Debian|8 och senare|
|Microsoft|Windows Server|2012 och senare|
|Microsoft|Windows-klient|Windows 10|
|OpenLogic|CentOS|7,3 och senare|
|Red Hat|Red Hat Enterprise Linux|7,4 och senare|
|SUSE|SLES|12 SP3 och senare|

Anpassade avbildningar av virtuella datorer stöds av principer för gäst konfiguration så länge de är ett av operativ systemen i tabellen ovan.

### <a name="unsupported-client-types"></a>Klient typer som inte stöds

Windows Server Nano Server stöds inte i någon version.

## <a name="guest-configuration-extension-network-requirements"></a>Nätverks krav för gäst konfigurations tillägg

För att kunna kommunicera med resurs leverantören för gäst konfiguration i Azure måste datorer ha utgående åtkomst till Azure-datacenter på port **443**. Om ett nätverk i Azure inte tillåter utgående trafik konfigurerar du undantag med regler för [nätverks säkerhets grupper](../../../virtual-network/manage-network-security-group.md#create-a-security-rule) . [Service tag-](../../../virtual-network/service-tags-overview.md) GuestAndHybridManagement kan användas för att referera till gäst konfigurations tjänsten.

## <a name="managed-identity-requirements"></a>Krav för hanterade identiteter

**DeployIfNotExists** -principerna som lägger till tillägget på virtuella datorer aktiverar också en systemtilldelad hanterad identitet, om det inte finns någon.

> [!WARNING]
> Undvik att aktivera användare som tilldelats hanterad identitet till virtuella datorer inom omfånget för principer som aktiverar systemtilldelad hanterad identitet. Användarens tilldelade identitet ersätts och datorn slutar svara.

## <a name="guest-configuration-definition-requirements"></a>Krav för konfigurations definition för gäst

Varje konfiguration för gransknings körning av gäst kräver två princip definitioner, en **DeployIfNotExists** -definition och en **AuditIfNotExists** -definition.

**DeployIfNotExists** princip definition verifierar och korrigerar följande objekt:

- Verifiera att datorn har tilldelats en konfiguration som ska utvärderas. Om ingen tilldelning för närvarande finns kan du hämta tilldelningen och förbereda datorn genom att:
  - Autentisera till datorn med en [hanterad identitet](../../../active-directory/managed-identities-azure-resources/overview.md)
  - Installera den senaste versionen av **Microsoft. GuestConfiguration** -tillägget
  - Installera [validerings verktyg](#validation-tools) och beroenden, om det behövs

Om tilldelningen **DeployIfNotExists** är icke-kompatibel kan en [reparations uppgift](../how-to/remediate-resources.md#create-a-remediation-task) användas.

När **DeployIfNotExists** -tilldelningen är kompatibel, avgör **AuditIfNotExists** -princip tilldelningen om gäst tilldelningen är kompatibel eller inte kompatibel. Verifierings verktyget tillhandahåller resultatet till klienten för gäst konfiguration. Klienten vidarebefordrar resultaten till gäst tillägget, som gör dem tillgängliga via resurs leverantören för gäst konfiguration.

Azure Policy använder **complianceStatus** -egenskapen för gäst konfiguration för att rapportera efterlevnad i noden **efterlevnad** . Mer information finns i [Hämta efterlevnadsprinciper](../how-to/get-compliance-data.md).

> [!NOTE]
> **DeployIfNotExists** -principen krävs för att **AuditIfNotExists** -principen ska returnera resultat. Utan **DeployIfNotExists**visar **AuditIfNotExists** -principen "0 av 0" resurser som status.

Alla inbyggda principer för gäst konfiguration ingår i ett initiativ för att gruppera definitionerna för användning i tilldelningar. Den inbyggda för _ \[ hands versionen med namnet Preview \] : granska lösen ords säkerhet i Linux-och Windows-datorer_ innehåller 18 principer. Det finns sex **DeployIfNotExists** -och **AuditIfNotExists** -par för Windows och tre par för Linux. [Princip definitions](definition-structure.md#policy-rule) logiken verifierar att endast mål operativ systemet utvärderas.

#### <a name="auditing-operating-system-settings-following-industry-baselines"></a>Granska operativ system inställningar efter bransch bas linjer

Ett initiativ i Azure Policy ger möjlighet att granska inställningarna för operativ systemet efter en "bas linje". Definition, för _ \[ hands version \] : granska virtuella Windows-datorer som inte matchar inställningarna för Azures säkerhets bas linje_ innehåller en uppsättning regler som baseras på Active Directory Grupprincip.

De flesta av inställningarna är tillgängliga som parametrar. Med parametrar kan du anpassa vad som granskas.
Justera principen med dina krav eller mappa principen till information från tredje part, till exempel bransch regelverks standarder.

Vissa parametrar har stöd för ett heltals värde intervall. Till exempel kan inställningen högsta ålder för lösen ord granska inställningen gällande grupprincip. Ett "1, 70"-intervall bekräftar att användarna måste ändra sina lösen ord minst var 70 dag, men inte mindre än en dag.

Om du tilldelar principen med hjälp av en mall för Azure Resource Manager distribution använder du en parameter fil för att hantera undantag. Checka in filerna till ett versions kontroll system, till exempel git. Kommentarer om fil ändringar ger bevis varför en tilldelning är ett undantag till det förväntade värdet.

#### <a name="applying-configurations-using-guest-configuration"></a>Använda konfigurationer med gäst konfiguration

Den senaste funktionen i Azure Policy konfigurerar inställningar i datorer. Definitionen _Konfigurera tids zonen på Windows-datorer_ gör ändringar i datorn genom att konfigurera tids zonen.

När du tilldelar definitioner som börjar med _Konfigurera_måste du också tilldela _krav för definitions distribution för att aktivera principen för gäst konfiguration på virtuella Windows-datorer_. Du kan kombinera dessa definitioner i ett initiativ om du väljer.

#### <a name="assigning-policies-to-machines-outside-of-azure"></a>Tilldela principer till datorer utanför Azure

De gransknings principer som är tillgängliga för gäst konfiguration är resurs typen **Microsoft. HybridCompute/Machines** . Alla datorer som har publicerats till [Azure-bågen för servrar](../../../azure-arc/servers/overview.md) som omfattas av princip tilldelningen ingår automatiskt.

### <a name="multiple-assignments"></a>Flera tilldelningar

Principer för gäst konfiguration stöder för närvarande bara tilldelning av samma gäst tilldelning en gång per dator, även om princip tilldelningen använder olika parametrar.

## <a name="client-log-files"></a>Loggfiler för klienter

Gäst konfigurations tillägget skriver loggfiler till följande platser:

Aktivitets`C:\ProgramData\GuestConfig\gc_agent_logs\gc_agent.log`

Linux`/var/lib/GuestConfig/gc_agent_logs/gc_agent.log`

Där `<version>` refererar till det aktuella versions numret.

### <a name="collecting-logs-remotely"></a>Samla in loggar via fjärr anslutning

Det första steget i fel sökning av konfigurationer eller moduler för gäst konfiguration bör vara att använda `Test-GuestConfigurationPackage` cmdleten genom att följa stegen hur du [skapar en anpassad princip för gäst konfigurations granskning för Windows](../how-to/guest-configuration-create.md#step-by-step-creating-a-custom-guest-configuration-audit-policy-for-windows).
Om det inte lyckas kan insamling av klient loggar hjälpa till att diagnostisera problem.

#### <a name="windows"></a>Windows

Samla in information från loggfiler med [kommandot för körning av virtuell Azure-dator](../../../virtual-machines/windows/run-command.md). följande PowerShell-skript kan vara till hjälp.

```powershell
$linesToIncludeBeforeMatch = 0
$linesToIncludeAfterMatch = 10
$logPath = 'C:\ProgramData\GuestConfig\gc_agent_logs\gc_agent.log'
Select-String -Path $logPath -pattern 'DSCEngine','DSCManagedEngine' -CaseSensitive -Context $linesToIncludeBeforeMatch,$linesToIncludeAfterMatch | Select-Object -Last 10
```

#### <a name="linux"></a>Linux

Samla in information från loggfiler med [kommandot Azure VM Run](../../../virtual-machines/linux/run-command.md), det kan vara bra med följande exempel på bash-skript.

```Bash
linesToIncludeBeforeMatch=0
linesToIncludeAfterMatch=10
logPath=/var/lib/GuestConfig/gc_agent_logs/gc_agent.log
egrep -B $linesToIncludeBeforeMatch -A $linesToIncludeAfterMatch 'DSCEngine|DSCManagedEngine' $logPath | tail
```

## <a name="guest-configuration-samples"></a>Exempel på gäst konfiguration

Inbyggda princip exempel för gäst konfiguration finns på följande platser:

- [Inbyggda princip definitioner – gäst konfiguration](../samples/built-in-policies.md#guest-configuration)
- [Inbyggda initiativ – gäst konfiguration](../samples/built-in-initiatives.md#guest-configuration)
- [Azure Policy exempel GitHub lagrings platsen](https://github.com/Azure/azure-policy/tree/master/built-in-policies/policySetDefinitions/Guest%20Configuration)

## <a name="next-steps"></a>Nästa steg

- Lär dig hur du visar information om varje inställning från [vyn efterlevnad för gäst konfiguration](../how-to/determine-non-compliance.md#compliance-details-for-guest-configuration)
- Granska exempel i [Azure policy exempel](../samples/index.md).
- Granska [Azure Policy-definitionsstrukturen](definition-structure.md).
- Granska [Förstå policy-effekter](effects.md).
- Lär dig att [program mässigt skapa principer](../how-to/programmatically-create.md).
- Lär dig hur du [hämtar efterlevnadsprinciper](../how-to/get-compliance-data.md).
- Lär dig hur du [åtgärdar icke-kompatibla resurser](../how-to/remediate-resources.md).
- Granska en hanterings grupp med [organisera dina resurser med Azures hanterings grupper](../../management-groups/overview.md).
