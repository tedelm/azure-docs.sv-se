---
title: Delegera ett undernät till Azure NetApp Files | Microsoft Docs
description: Beskriver hur du delegerar ett undernät till Azure NetApp Files.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/04/2020
ms.author: b-juche
ms.openlocfilehash: 5f36e40091ada27f411adc2ffa78b6d4a58f8cca
ms.sourcegitcommit: e0330ef620103256d39ca1426f09dd5bb39cd075
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 05/05/2020
ms.locfileid: "82791416"
---
# <a name="delegate-a-subnet-to-azure-netapp-files"></a>Delegera ett undernät till Azure NetApp Files 

Du måste delegera ett undernät till Azure NetApp Files.   När du skapar en volym kan behöva du ange det delegerade undernätet.

## <a name="considerations"></a>Överväganden
* Guiden för att skapa ett nytt undernät har som standard en /24-nätverksmask som tillhandahåller 251 tillgängliga IP-adresser. En /28-nätverksmask som tillhandahåller 16 användbara IP-adresser räcker för tjänsten.
* Endast ett undernät kan delegeras till Azure NetApp Files i varje Azure Virtual Network (VNet).   
   Med Azure kan du skapa flera delegerade undernät i ett VNet.  Försök att skapa en ny volym kommer dock att Miss lyckas om du använder mer än ett delegerat undernät.  
   Du kan bara ha ett enda delegerat undernät i ett VNet. Ett NetApp-konto kan distribuera volymer i flera virtuella nätverk, vart och ett har sitt eget delegerade undernät.  
* Du kan inte ange en grupp eller en tjänstslutpunkt i delegerade undernät. Det leder till att delegeringen av undernätet misslyckas.
* Åtkomst till en volym från ett globalt peer-kopplat virtuellt nätverk stöds inte för närvarande.
* Det går inte att skapa [användardefinierade anpassade vägar](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview#custom-routes) för virtuella dator under nät med adressprefix (mål) till ett undernät som har delegerats till Azure NetApp Files. Detta påverkar VM-anslutningen.

## <a name="steps"></a>Steg 
1.  Gå till bladet **Virtuella nätverk** i Azure-portalen och välj det virtuella nätverket som du vill använda för Azure NetApp Files.    

1. Välj **Undernät** på bladet Virtuella nätverk och klicka på knappen **+Undernät**. 

1. Skapa ett nytt undernät att använda för Azure NetApp Files genom att fylla i följande obligatoriska fält på sidan Lägg till undernät:
    * **Namn**: Ange under nätets namn.
    * **Adress intervall**: Ange IP-adressintervall.
    * **Under näts delegering**: Välj **Microsoft. NetApp/Volumes**. 

      ![Delegering av undernät](../media/azure-netapp-files/azure-netapp-files-subnet-delegation.png)
    
Du kan också skapa och delegera ett undernät när du [skapar en volym för Azure NetApp Files](azure-netapp-files-create-volumes.md). 

## <a name="next-steps"></a>Nästa steg  
* [Skapa en volym för Azure NetApp Files](azure-netapp-files-create-volumes.md)
* [Läs om integrering av virtuella nätverk för Azure-tjänster](https://docs.microsoft.com/azure/virtual-network/virtual-network-for-azure-services)


