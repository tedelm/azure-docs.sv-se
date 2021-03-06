---
title: 'Snabb start: skapa en Azure-brandvägg och IP-grupper – Resource Manager-mall'
description: Lär dig hur du använder en Resource Manager-mall för att skapa en Azure-brandvägg och IP-grupper.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: quickstart
ms.custom: subject-armqs
ms.date: 04/06/2020
ms.author: victorh
ms.openlocfilehash: 403aaafebcae680f337aeff551b81a80a9549252
ms.sourcegitcommit: 50673ecc5bf8b443491b763b5f287dde046fdd31
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 05/20/2020
ms.locfileid: "83680567"
---
# <a name="quickstart-create-an-azure-firewall-and-ip-groups---resource-manager-template"></a>Snabb start: skapa en Azure-brandvägg och IP-grupper – Resource Manager-mall

I den här snabb starten använder du en Resource Manager-mall för att distribuera en Azure-brandvägg med exempel på IP-grupper som används i en nätverks regel och en program regel. En IP-grupp är en resurs på den översta nivån där du kan definiera och gruppera IP-adresser, intervall och undernät i ett enda objekt. Detta är användbart för att hantera IP-adresser i Azure brand Väggs regler. Du kan antingen ange IP-adresser manuellt eller importera dem från en fil.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

## <a name="prerequisites"></a>Krav

- Ett Azure-konto med en aktiv prenumeration. [Skapa ett konto kostnads fritt](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="create-an-azure-firewall-and-ip-groups"></a>Skapa en Azure-brandvägg och IP-grupper

Den här mallen skapar en Azure-brandvägg och IP-grupper, tillsammans med nödvändiga resurser för att stödja Azure-brandväggen.

### <a name="review-the-template"></a>Granska mallen

Mallen som används i den här snabb starten är från [Azure snabb starts-mallar](https://azure.microsoft.com/resources/templates/101-azurefirewall-create-with-ipgroups-and-linux-jumpbox).

:::code language="json" source="~/quickstart-templates/101-azurefirewall-create-with-ipgroups-and-linux-jumpbox/azuredeploy.json" range="001-512" highlight="118-141":::

Flera Azure-resurser definieras i mallen:

- [**Microsoft. Network/ipGroups**](/azure/templates/microsoft.network/ipGroups)
- [**Microsoft. Storage/storageAccounts**](/azure/templates/microsoft.storage/storageAccounts)
- [**Microsoft. Network/routeTables**](/azure/templates/microsoft.network/routeTables)
- [**Microsoft. Network/networkSecurityGroups**](/azure/templates/microsoft.network/networksecuritygroups)
- [**Microsoft. Network/virtualNetworks**](/azure/templates/microsoft.network/virtualnetworks)
- [**Microsoft. Network/publicIPAddresses**](/azure/templates/microsoft.network/publicipaddresses)
- [**Microsoft. Network/networkInterfaces**](/azure/templates/microsoft.network/networkinterfaces)
- [**Microsoft. Compute/virtualMachines**](/azure/templates/microsoft.compute/virtualmachines)
- [**Microsoft. Network/azureFirewalls**](/azure/templates/microsoft.network/azureFirewalls)

### <a name="deploy-the-template"></a>Distribuera mallen

Distribuera Resource Manager-mall till Azure:

1. Välj **distribuera till Azure** för att logga in på Azure och öppna mallen. Mallen skapar en Azure-brandvägg, nätverks infrastrukturen och två virtuella datorer.

   [![Distribuera till Azure](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-azurefirewall-create-with-ipgroups-and-linux-jumpbox%2Fazuredeploy.json)

2. I portalen, på sidan **skapa en Azure-brandvägg med IpGroups** , skriver eller väljer du följande värden:
   - Prenumeration: Välj från befintliga prenumerationer 
   - Resurs grupp: Välj från befintliga resurs grupper eller Välj **Skapa ny**och välj **OK**.
   - Plats: Välj en plats
   - Virtual Network namn: Ange ett namn för det nya virtuella nätverket (VNet) 
   - IP-grupp namn 1: typ namn för IP-grupp 1 
   - IP-grupp namn 2: typ namn för IP-grupp 2 
   - Admin-användar namn: Ange användar namn för administratörs användar kontot 
   - Autentisering: Välj sshPublicKey eller lösen ord 
   - Administratörs lösen ord: Ange ett administratörs lösen ord eller en nyckel

3. Välj **Jag accepterar de villkor som anges ovan** och välj sedan **köp**. Distributionen kan ta 10 minuter eller längre att slutföra.

## <a name="review-deployed-resources"></a>Granska distribuerade resurser

I Azure Portal granskar du de distribuerade resurserna, särskilt de brand Väggs regler som använder IP-grupper.

:::image type="content" source="media/quick-create-ipgroup-template/ipgroups.png" alt-text="IP-grupper.":::

:::image type="content" source="media/quick-create-ipgroup-template/network-rule.png" alt-text="Nätverks regler.":::

Mer information om JSON-syntaxen och egenskaperna för en brand vägg i en mall finns i [Microsoft. Network azureFirewalls Template Reference](https://docs.microsoft.com/azure/templates/Microsoft.Network/2019-11-01/azureFirewalls).

## <a name="clean-up-resources"></a>Rensa resurser

När du inte längre behöver de resurser som du skapade med brand väggen tar du bort resurs gruppen. Detta tar bort brand väggen och alla relaterade resurser.

Anropa cmdleten om du vill ta bort resurs gruppen `Remove-AzResourceGroup` :

```azurepowershell-interactive
Remove-AzResourceGroup -Name "<your resource group name>"
```

## <a name="next-steps"></a>Nästa steg

> [!div class="nextstepaction"]
> [Självstudie: Distribuera och konfigurera Azure-brandväggen i ett hybrid nätverk med hjälp av Azure Portal](tutorial-hybrid-portal.md)