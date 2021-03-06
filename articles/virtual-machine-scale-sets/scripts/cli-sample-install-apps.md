---
title: Azure CLI-exempel – installera appar
description: Det här skriptet skapar en VM-skalningsuppsättning som kör Ubuntu och använder det anpassade skripttillägget för att installera ett grundläggande webbprogram.
author: mimckitt
ms.author: mimckitt
ms.topic: sample
ms.service: virtual-machine-scale-sets
ms.subservice: cli
ms.date: 03/27/2018
ms.reviewer: jushiman
ms.custom: mimckitt
ms.openlocfilehash: 2e284032cfc6723fb56454376edafa6d99ae7e0a
ms.sourcegitcommit: 595cde417684e3672e36f09fd4691fb6aa739733
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 05/20/2020
ms.locfileid: "83699706"
---
# <a name="install-applications-into-a-virtual-machine-scale-set-with-the-azure-cli"></a>Installera program till en skalningsuppsättning för en virtuell dator med Azure CLI
Det här skriptet skapar en VM-skalningsuppsättning som kör Ubuntu och använder det anpassade skripttillägget för att installera ett grundläggande webbprogram. När skriptet har körts kan du komma åt webbappen via en webbläsare.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Exempelskript
[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine-scale-sets/install-apps/install-apps.sh "Install apps into a scale set")]

## <a name="clean-up-deployment"></a>Rensa distribution
Kör följande kommando för att ta bort resursgruppen, skalningsuppsättningen och alla relaterade resurser.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Förklaring av skript
Det här skriptet använder följande kommandon för att skapa en resursgrupp, en VM-skalningsuppsättning och alla relaterade resurser. Varje kommando i tabellen länkar till kommandospecifik dokumentation.

| Kommando | Anteckningar |
|---|---|
| [az group create](/cli/azure/ad/group) | Skapar en resursgrupp där alla resurser lagras. |
| [az vmss create](/cli/azure/vmss) | Skapar VM-skalningsuppsättningen och ansluter den till det virtuella nätverket, undernätet och nätverkssäkerhetsgruppen. En lastbalanserare skapas även för att distribuera trafik till flera virtuella datorinstanser. Det här kommandot anger även den virtuella datoravbildning som ska användas samt administrativa autentiseringsuppgifter.  |
| [az vmss extension set](/cli/azure/vmss/extension) | Installerar det anpassade Azure-skripttillägget för att köra ett skript som förbereder datadiskarna på varje virtuell datorinstans. |
| [az network lb rule create](/cli/azure/network/lb/rule) | Skapar en regel för lastbalanseraren för att distribuera trafik på TCP-port 80 till virtuella datorinstanser i skalningsuppsättningen. |
| [az network public-ip show](/cli/azure/network/public-ip) | Hämtar information om den offentliga IP-adress som används av lastbalanseraren. |
| [az group delete](/cli/azure/ad/group) | Tar bort en resursgrupp, inklusive alla kapslade resurser. |

## <a name="next-steps"></a>Nästa steg
Mer information om Azure CLI finns i [Azure CLI-dokumentationen](https://docs.microsoft.com/cli/azure/overview).
