---
title: Konfigurera Azure Analysis Services för VNet data källor | Microsoft Docs
description: Lär dig hur du konfigurerar en Azure Analysis Services-server för att använda en gateway för data källor på Azure Virtual Network (VNet).
author: minewiskan
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 05/19/2020
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 7622b6b5855850a0150f43863fa117828e23d4bd
ms.sourcegitcommit: 595cde417684e3672e36f09fd4691fb6aa739733
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 05/20/2020
ms.locfileid: "83698866"
---
# <a name="use-gateway-for-data-sources-on-an-azure-virtual-network-vnet"></a>Använda Gateway för data källor på en Azure-Virtual Network (VNet)

I den här artikeln beskrivs egenskapen Azure Analysis Services **AlwaysUseGateway** -server som används när data källor finns på ett [Azure-Virtual Network (VNet)](../virtual-network/virtual-networks-overview.md).

## <a name="server-access-to-vnet-data-sources"></a>Server åtkomst till VNet-datakällor

Om dina data källor nås via ett VNet måste Azure Analysis Services servern ansluta till dessa data källor som om de är lokala, i din egen miljö. Du kan konfigurera egenskapen **AlwaysUseGateway** Server för att ange att servern ska få åtkomst till alla data källor via en [lokal gateway](analysis-services-gateway.md). 

Azure SQL Database hanterade instans data källor körs i Azure VNet med en privat IP-adress. Om den offentliga slut punkten är aktive rad på instansen krävs ingen gateway. Om den offentliga slut punkten inte är aktive rad krävs en lokal datagateway och egenskapen AlwaysUseGateway måste ha värdet true.

> [!NOTE]
> Den här egenskapen gäller bara när en [lokal datagateway](analysis-services-gateway.md) har installerats och kon figurer ATS. Gatewayen kan finnas i det virtuella nätverket.

## <a name="configure-alwaysusegateway-property"></a>Konfigurera egenskapen AlwaysUseGateway

1. I SSMS > Server > **Egenskaper**  >  **Allmänt**väljer du **Visa avancerade egenskaper (alla)**.
2. I **ASPaaS\AlwaysUseGateway**väljer du **Sant**.

    ![Använd alltid Gateway-egenskap](media/analysis-services-vnet-gateway/aas-ssms-always-property.png)


## <a name="see-also"></a>Se även
[Ansluta till lokala data källor](analysis-services-gateway.md)   
[Installera och konfigurera en lokal datagateway](analysis-services-gateway-install.md)   
[Azure-Virtual Network (VNET)](../virtual-network/virtual-networks-overview.md)   

