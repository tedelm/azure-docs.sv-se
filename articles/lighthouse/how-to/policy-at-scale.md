---
title: Distribuera Azure Policy till delegerade prenumerationer i stor skala
description: Lär dig hur Azure delegerad resurs hantering gör det möjligt att distribuera en princip definition och princip tilldelning över flera klienter.
ms.date: 11/8/2019
ms.topic: conceptual
ms.openlocfilehash: 3fe7e48c56e9a5af93e9642ee16c50cfbce34f9e
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/28/2020
ms.locfileid: "81481828"
---
# <a name="deploy-azure-policy-to-delegated-subscriptions-at-scale"></a>Distribuera Azure Policy till delegerade prenumerationer i stor skala

Som tjänst leverantör kan du ha registrerat flera kund innehavare för Azure-delegerad resurs hantering. Med [Azure Lighthouse](../overview.md) kan tjänst leverantörer utföra åtgärder i skala över flera klienter samtidigt, vilket gör hanterings uppgifter mer effektiva.

Det här avsnittet visar hur du använder [Azure policy](../../governance/policy/index.yml) för att distribuera en princip definition och princip tilldelning över flera klienter med PowerShell-kommandon. I det här exemplet säkerställer princip definitionen att lagrings konton skyddas genom att endast tillåta HTTPS-trafik.

## <a name="use-azure-resource-graph-to-query-across-customer-tenants"></a>Använd Azure Resource Graph för att fråga mellan kund klienter

Du kan använda [Azure Resource Graph](../../governance/resource-graph/index.yml) för att fråga över alla prenumerationer i de kund innehavare som du hanterar. I det här exemplet kommer vi att identifiera eventuella lagrings konton i dessa prenumerationer som inte kräver HTTPS-trafik.  

```powershell
$MspTenant = "insert your managing tenantId here"

$subs = Get-AzSubscription

$ManagedSubscriptions = Search-AzGraph -Query "ResourceContainers | where type == 'microsoft.resources/subscriptions' | where tenantId != '$($mspTenant)' | project name, subscriptionId, tenantId" -subscription $subs.subscriptionId

Search-AzGraph -Query "Resources | where type =~ 'Microsoft.Storage/storageAccounts' | project name, location, subscriptionId, tenantId, properties.supportsHttpsTrafficOnly" -subscription $ManagedSubscriptions.subscriptionId | convertto-json
```

## <a name="deploy-a-policy-across-multiple-customer-tenants"></a>Distribuera en princip över flera kund klienter

Exemplet nedan visar hur du använder en [Azure Resource Manager-mall](https://github.com/Azure/Azure-Lighthouse-samples/blob/master/templates/policy-enforce-https-storage/enforceHttpsStorage.json) för att distribuera en princip definition och princip tilldelning mellan delegerade prenumerationer i flera kund klienter. Den här princip definitionen kräver att alla lagrings konton använder HTTPS-trafik, vilket förhindrar att nya lagrings konton skapas som inte uppfyller och markerar befintliga lagrings konton utan inställningen som icke-kompatibel.

```powershell
Write-Output "In total, there are $($ManagedSubscriptions.Count) delegated customer subscriptions to be managed"

foreach ($ManagedSub in $ManagedSubscriptions)
{
    Select-AzSubscription -SubscriptionId $ManagedSub.subscriptionId

    New-AzSubscriptionDeployment -Name mgmt `
                     -Location eastus `
                     -TemplateUri "https://raw.githubusercontent.com/Azure/Azure-Lighthouse-samples/master/templates/policy-enforce-https-storage/enforceHttpsStorage.json" `
                     -AsJob
}
```

## <a name="validate-the-policy-deployment"></a>Verifiera princip distributionen

När du har distribuerat Azure Resource Manager-mallen kan du bekräfta att princip definitionen har tillämpats genom att försöka skapa ett lagrings konto med **EnableHttpsTrafficOnly** inställt på **false** i en av dina delegerade prenumerationer. På grund av princip tilldelningen bör du inte skapa det här lagrings kontot.  

```powershell
New-AzStorageAccount -ResourceGroupName (New-AzResourceGroup -name policy-test -Location eastus -Force).ResourceGroupName `
                     -Name (get-random) `
                     -Location eastus `
                     -EnableHttpsTrafficOnly $false `
                     -SkuName Standard_LRS `
                     -Verbose                  
```

## <a name="clean-up-resources"></a>Rensa resurser

När du är klar tar du bort princip definitionen och tilldelningen som skapats av distributionen.

```powershell
foreach ($ManagedSub in $ManagedSubscriptions)
{
    select-azsubscription -subscriptionId $ManagedSub.subscriptionId

    Remove-AzSubscriptionDeployment -Name mgmt -AsJob

    $Assignment = Get-AzPolicyAssignment | where-object {$_.Name -like "enforce-https-storage-assignment"}

    if ([string]::IsNullOrEmpty($Assignment))
    {
        Write-Output "Nothing to clean up - we're done"
    }
    else
    {

    Remove-AzPolicyAssignment -Name 'enforce-https-storage-assignment' -Scope "/subscriptions/$($ManagedSub.subscriptionId)" -Verbose

    Write-Output "Deployment has been deleted - we're done"
    }
}
```

## <a name="next-steps"></a>Nästa steg

- Läs mer om [Azure policy](../../governance/policy/index.yml).
- Lär dig mer om [hanterings upplevelser mellan flera innehavare](../concepts/cross-tenant-management-experience.md).
