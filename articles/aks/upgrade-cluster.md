---
title: Uppgradera ett AKS-kluster (Azure Kubernetes Service)
description: Lär dig hur du uppgraderar ett Azure Kubernetes service-kluster (AKS) för att få de senaste funktionerna och säkerhets uppdateringarna.
services: container-service
ms.topic: article
ms.date: 05/31/2019
ms.openlocfilehash: 7e9a47b7bda4cdb0ff6f1983bc884f7441a26d9b
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/28/2020
ms.locfileid: "82207981"
---
# <a name="upgrade-an-azure-kubernetes-service-aks-cluster"></a>Uppgradera ett AKS-kluster (Azure Kubernetes Service)

Som en del av livs cykeln för ett AKS-kluster måste du ofta uppgradera till den senaste versionen av Kubernetes. Det är viktigt att du tillämpar de senaste Kubernetes säkerhets versionerna eller uppgraderar för att få de senaste funktionerna. Den här artikeln visar hur du uppgraderar huvud komponenterna eller en enskild standardnode-pool i ett AKS-kluster.

Information om AKS-kluster som använder flera noder finns i [uppgradera en Node-pool i AKS][nodepool-upgrade].

## <a name="before-you-begin"></a>Innan du börjar

Den här artikeln kräver att du kör Azure CLI-version 2.0.65 eller senare. Kör `az --version` för att hitta versionen. Om du behöver installera eller uppgradera kan du läsa [Installera Azure CLI][azure-cli-install].

> [!WARNING]
> En AKS kluster uppgradering utlöser en Cordon och tömmer dina noder. Om du har en låg beräknings kvot som är tillgänglig kan uppgraderingen Miss lyckas. Mer information finns i [öka kvoterna](https://docs.microsoft.com/azure/azure-portal/supportability/resource-manager-core-quotas-request) .
> Om du kör en egen kluster distribution med autoskalning inaktiverar du den (du kan skala den till noll repliker) under uppgraderingen eftersom det är en chans att det stör uppgraderings processen. Hanterade autoskalning hanterar detta automatiskt. 

## <a name="check-for-available-aks-cluster-upgrades"></a>Sök efter tillgängliga AKS-kluster uppgraderingar

Om du vill kontrol lera vilka Kubernetes-versioner som är tillgängliga för klustret använder du kommandot [AZ AKS get-uppgraderingar][az-aks-get-upgrades] . Följande exempel söker efter tillgängliga uppgraderingar till klustret med namnet *myAKSCluster* i resurs gruppen med namnet *myResourceGroup*:

```azurecli-interactive
az aks get-upgrades --resource-group myResourceGroup --name myAKSCluster --output table
```

> [!NOTE]
> När du uppgraderar ett AKS-kluster kan Kubernetes minor-versioner inte hoppas över. Till exempel tillåts uppgraderingar mellan *1.12. x* -> *1.13. x* eller *1.13. x* -> *1.14. x* , men *1.12.* -> x*1.14. x* är inte.
>
> Uppgradera från *1.12. x* -> *1.14. x*genom att först uppgradera från *1.12.* -> *x 1.13. x*och sedan uppgradera från *1.13. x* -> *1.14. x*.

Följande exempel på utdata visar att klustret kan uppgraderas till versioner *1.13.9* och *1.13.10*:

```console
Name     ResourceGroup     MasterVersion    NodePoolVersion    Upgrades
-------  ----------------  ---------------  -----------------  ---------------
default  myResourceGroup   1.12.8           1.12.8             1.13.9, 1.13.10
```
Om ingen uppgradering är tillgänglig kommer du att få:
```console
ERROR: Table output unavailable. Use the --query option to specify an appropriate query. Use --debug for more info.
```

## <a name="upgrade-an-aks-cluster"></a>Uppgradera ett AKS-kluster

Med en lista över tillgängliga versioner för ditt AKS-kluster använder du kommandot [AZ AKS Upgrade][az-aks-upgrade] för att uppgradera. Under uppgraderings processen lägger AKS till en ny nod i klustret som kör den angivna Kubernetes-versionen, därefter noga [Cordon och tömmer][kubernetes-drain] en av de gamla noderna för att minimera störningar i program som körs. När den nya noden bekräftas som att köra Application poddar tas den gamla noden bort. Den här processen upprepas tills alla noder i klustret har uppgraderats.

I följande exempel uppgraderas ett kluster till version *1.13.10*:

```azurecli-interactive
az aks upgrade --resource-group myResourceGroup --name myAKSCluster --kubernetes-version 1.13.10
```

Det tar några minuter att uppgradera klustret, beroende på hur många noder du har. 

> [!NOTE]
> Det finns en total tillåten tid för att en kluster uppgradering ska slutföras. Den här tiden beräknas genom att ta produkten av `10 minutes * total number of nodes in the cluster`. Till exempel i ett kluster med 20 noder, måste uppgraderings åtgärder lyckas på 200 minuter eller så kan AKS inte utföra åtgärden för att undvika ett oåterkalleligt kluster tillstånd. Om du vill återställa vid uppgraderings fel, försök att uppgradera igen när tids gränsen har nåtts.

För att bekräfta att uppgraderingen har slutförts använder du kommandot [AZ AKS show][az-aks-show] :

```azurecli-interactive
az aks show --resource-group myResourceGroup --name myAKSCluster --output table
```

Följande exempel på utdata visar att klustret nu kör *1.13.10*:

```json
Name          Location    ResourceGroup    KubernetesVersion    ProvisioningState    Fqdn
------------  ----------  ---------------  -------------------  -------------------  ---------------------------------------------------------------
myAKSCluster  eastus      myResourceGroup  1.13.10               Succeeded            myaksclust-myresourcegroup-19da35-90efab95.hcp.eastus.azmk8s.io
```

## <a name="next-steps"></a>Nästa steg

Den här artikeln visar hur du uppgraderar ett befintligt AKS-kluster. Mer information om hur du distribuerar och hanterar AKS-kluster finns i självstudierna.

> [!div class="nextstepaction"]
> [AKS-självstudier][aks-tutorial-prepare-app]

<!-- LINKS - external -->
[kubernetes-drain]: https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/

<!-- LINKS - internal -->
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
[azure-cli-install]: /cli/azure/install-azure-cli
[az-aks-get-upgrades]: /cli/azure/aks#az-aks-get-upgrades
[az-aks-upgrade]: /cli/azure/aks#az-aks-upgrade
[az-aks-show]: /cli/azure/aks#az-aks-show
[nodepool-upgrade]: use-multiple-node-pools.md#upgrade-a-node-pool
