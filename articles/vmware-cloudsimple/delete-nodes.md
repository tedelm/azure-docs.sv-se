---
title: Ta bort noder för VMware-lösning av CloudSimple – Azure
description: Lär dig hur du tar bort noder från din VMWare med CloudSimple-distribution
author: sharaths-cs
ms.author: dikamath
ms.date: 08/05/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 122e0636f54e066ae86ed2d19cefe5863b026293
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/28/2020
ms.locfileid: "77024746"
---
# <a name="delete-nodes-from-azure-vmware-solution-by-cloudsimple"></a>Ta bort noder från Azure VMware-lösningen av CloudSimple

CloudSimple-noder mäts när de väl har skapats.  Noder måste tas bort för att stoppa datamätningen av noderna.  Du tar bort de noder som inte används från Azure Portal.

## <a name="before-you-begin"></a>Innan du börjar

En nod kan bara tas bort under följande omständigheter:

* Ett privat moln som skapats med noderna tas bort.  Information om hur du tar bort ett privat moln finns i [ta bort en Azure VMware-lösning från ett privat CloudSimple-moln](delete-private-cloud.md).
* Noden har tagits bort från det privata molnet genom att minska det privata molnet.  Information om hur du krymper ett privat moln finns i [minska Azure VMware-lösningen med CloudSimple Private Cloud](shrink-private-cloud.md).

## <a name="sign-in-to-azure"></a>Logga in på Azure

Logga in på Azure Portal på [https://portal.azure.com](https://portal.azure.com).

## <a name="delete-cloudsimple-node"></a>Ta bort CloudSimple-nod

1. Välj **Alla tjänster**.

2. Sök efter **CloudSimple-noder**.

   ![Sök CloudSimple-noder](media/create-cloudsimple-node-search.png)

3. Välj **CloudSimple-noder**.

4. Välj noder som inte tillhör ett privat moln som ska tas bort.  **Namn kolumnen privat moln** visar namnet på det privata moln som en nod tillhör.  Om en nod inte används av ett privat moln, är värdet tomt. 

    ![Välj CloudSimple-noder](media/select-delete-cloudsimple-node.png)

> [!NOTE]
> Endast noder som inte är en del av det privata molnet kan tas bort.

## <a name="next-steps"></a>Nästa steg

* Lär dig mer om [privat moln](cloudsimple-private-cloud.md)
