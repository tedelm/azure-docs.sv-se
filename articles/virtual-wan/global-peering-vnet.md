---
title: Konfigurera global VNet-peering för Azure Virtual WAN | Microsoft Docs
description: Anslut ett VNet i en annan region till din virtuella WAN-hubb.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: cherylmc
ms.openlocfilehash: 340472f84d2dd2c4f46d180992745a57e8ad1884
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/28/2020
ms.locfileid: "73588231"
---
# <a name="configure-global-vnet-peering-cross-region-vnet-for-virtual-wan"></a>Konfigurera global VNet-peering (VNet i flera regioner) för virtuellt WAN

Du kan ansluta ett VNet i en annan region till din virtuella WAN-hubb.

## <a name="before-you-begin"></a>Innan du börjar

Kontrol lera att du har uppfyllt följande kriterier:

* Cross-regions-VNet (eker) är inte anslutet till en annan virtuell WAN-hubb. En eker kan bara anslutas till ett virtuellt nav.
* VNet (eker) innehåller ingen virtuell nätverksgateway (till exempel ett Azure VPN Gateway-eller ExpressRoute-Gateway för virtuella nätverk). Om VNet innehåller en virtuell nätverksgateway måste du ta bort gatewayen innan du ansluter det virtuella ekraret till hubben.

## <a name="register-this-feature"></a><a name="register"></a>Registrera funktionen

Du kan registrera dig för den här funktionen med hjälp av PowerShell. Om du väljer "prova" från exemplet nedan öppnas Azure Cloud-Shell och du behöver inte installera PowerShell-cmdlets lokalt på datorn. Om det behövs kan du ändra prenumerationer med hjälp av cmdleten Select- <subid>AzSubscription-SubscriptionId.

```azurepowershell-interactive
Register-AzProviderFeature -FeatureName AllowCortexGlobalVnetPeering -ProviderNamespace Microsoft.Network
Register-AzResourceProvider -ProviderNamespace 'Microsoft.Network'
```

## <a name="verify-registration"></a><a name="verify"></a>Verifiera registrering

```azurepowershell-interactive
Get-AzProviderFeature -FeatureName AllowCortexGlobalVnetPeering -ProviderNamespace Microsoft.Network
```

## <a name="connect-a-vnet-to-the-hub"></a><a name="hub"></a>Ansluta ett VNet till hubben

I det här steget skapar du peering-anslutningen mellan hubben och det virtuella nätverket för flera regioner. Upprepa de här stegen för varje virtuellt nätverk du vill ansluta.

1. På sidan för det virtuella WAN-nätverket klickar du på **Virtuella nätverksanslutningar**.
2. På sidan för virtuell nätverksanslutning klickar du på **+Lägg till anslutning**.
3. Fyll i följande fält på sidan **Lägg till anslutning**:

    * **Anslutningsnamn** – Namnge anslutningen.
    * **Hubbar** – Välj den hubb du vill koppla till anslutningen.
    * **Prenumeration** – Kontrollera prenumerationen.
    * **Virtuellt nätverk** – Välj det virtuella nätverk du vill ansluta till hubben. Det virtuella nätverket får inte ha någon befintlig gateway för virtuellt nätverk.
4. Klicka på **OK** för att skapa peering-anslutningen.

## <a name="next-steps"></a>Nästa steg

Mer information om virtuellt WAN finns i [Översikt över virtuella WAN-nätverk](virtual-wan-about.md).