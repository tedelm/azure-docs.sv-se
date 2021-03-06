---
title: Ansluta Azure till offentliga moln | Microsoft Docs
description: Beskriv olika sätt att ansluta Azure till andra offentliga moln
services: expressroute
author: osamazia
ms.service: expressroute
ms.topic: article
ms.date: 07/24/2019
ms.author: osamaz
ms.openlocfilehash: b8a454c2a104dfe8545cf734bf0b020b8f749bb1
ms.sourcegitcommit: 6a4fbc5ccf7cca9486fe881c069c321017628f20
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/27/2020
ms.locfileid: "73889631"
---
# <a name="connecting-azure-with-public-clouds"></a>Ansluta Azure med offentliga moln

Många företag har en strategi för flera moln på grund av affärs-och teknik mål. Detta omfattar kostnader, flexibilitet, funktions tillgänglighet, redundans, data suveränitet osv. Den här strategin hjälper dem att utnyttja det bästa av båda molnen. 

Den här metoden innebär också utmaningar för företaget vad gäller nätverks-och program arkitektur. Några av dessa utmaningar är svars tid och data data flöde. För att lösa dessa utmaningar kan kunderna ansluta till flera moln direkt. Vissa tjänst leverantörer tillhandahåller en lösning för att ansluta flera moln leverantörer till kunderna. I andra fall kan kunden distribuera sin egen router för att ansluta flera offentliga moln.
## <a name="connectivity-via-expressroute"></a>Anslutning via ExpressRoute
Med ExpressRoute kan kunderna utöka sina lokala nätverk till Microsoft-molnet via en privat anslutning som under lättas av en anslutnings leverantör. Med ExpressRoute kan kunderna upprätta anslutningar till Microsofts moln tjänster.

Det finns tre sätt att ansluta via ExpressRoute.

1. Layer3-Provider
2. Layer2-Provider
3. Direkt anslutning

### <a name="layer3-provider"></a>Layer3-Provider

Layer3-providers kallas ofta IP VPN-eller MPLS VPN-providrar. Kunder utnyttjar dessa leverantörer för multipoint-anslutning mellan sina data Center, grenar och molnet. Kunder ansluter till L3-providern via BGP eller via statisk standard väg. Tjänste leverantören meddelar vägar mellan kund platser, data Center och offentliga moln. 
 
När du ansluter via Layer3-providern kommer Microsoft att annonsera kundens VNET-vägar till tjänst leverantören via BGP. Providern kan ha två olika implementeringar.

![](media/expressroute-connect-azure-to-public-cloud/azure-to-public-clouds-l3.png)

Leverantören kan ringa varje moln leverantör i en separat VRF, om trafik från alla moln leverantörer kommer till kundroutern. Om kunden kör BGP med tjänst leverantören annonseras dessa vägar på nytt till andra moln leverantörer som standard. 

Om tjänste leverantören är vilplan för alla moln leverantörer i samma VRF, annonseras vägar till andra moln leverantörer från tjänst leverantören direkt. Detta förutsätter en standard-BGP-åtgärd där eBGP vägar annonseras till andra eBGP-grannar som standard.

Varje offentligt moln har olika prefix så när du distribuerar väg tjänst leverantören bör vara försiktig med att distribuera vägarna.

### <a name="layer2-provider-and-direct-connection"></a>Layer2-Provider och direkt anslutning

Även om den fysiska anslutningen i båda modellerna är annorlunda, men på Layer3 BGP upprättas direkt mellan MSEE: N och kund routern. För ExpressRoute Direct-kunden ansluter till MSEE: N direkt. I händelse av layer2, utökar-tjänst leverantören VLAN från kund lokalt till molnet. Kunder som kör BGP ovanpå layer2-nätverket kan ansluta sina domänkontrollanter till molnet.
![](media/expressroute-connect-azure-to-public-cloud/azure-to-public-clouds-l2.png)
I båda fallen kommer kunden att ha punkt-till-punkt-anslutningar till var och en av de offentliga molnen. Kunden upprättar en separat BGP-anslutning till varje offentligt moln. Vägar som tas emot av en moln leverantör annonseras som standard till en annan moln leverantör. Varje moln leverantör har olika prefix så att du kan annonsera om kund tjänsten ska ta hand om dessa gränser. Kunden kan använda vanliga BGP-rattar med Microsoft vid annonsering av vägar från andra offentliga moln.

## <a name="direct-connection-with-expressroute"></a>Direkt anslutning med ExpressRoute

Kunderna kan välja att ansluta ExpressRoute direkt till moln leverantörens direkt anslutnings erbjudande. Två moln leverantörer kommer att anslutas tillbaka till och med BGP upprättas direkt mellan sina routrar. Den här typen av anslutning är tillgänglig med Oracle idag.

## <a name="site-to-site-vpn"></a>Plats-till-plats-VPN

Kunder kan utnyttja Internet för att ansluta sina instanser i Azure med andra offentliga moln. Nästan alla moln leverantörer erbjuder plats-till-plats-VPN-funktioner. Det kan dock finnas inkompatibiliteter på grund av brist på vissa varianter. Vissa moln leverantörer stöder till exempel bara IKEv1, så det finns en VPN-slutpunkt som krävs i det molnet. För de moln leverantörer som stöder IKEv2 kan en direkt tunnel upprättas mellan VPN-gatewayer i båda moln leverantörer.

Plats-till-plats-VPN anses inte vara en hög genom strömning och en lösning med låg latens. Det kan dock användas som en säkerhets kopia för fysisk anslutning.

## <a name="next-steps"></a>Nästa steg
Se [vanliga][ER-FAQ] frågor och svar om ExpressRoute för ytterligare frågor om ExpressRoute och virtuell nätverks anslutning.

Se [Konfigurera direkt anslutning mellan Azure och Oracle-molnet][ER-OCI] för anslutning mellan Azure och Oracle

<!--Link References-->
[ER-FAQ]: https://docs.microsoft.com/azure/expressroute/expressroute-faqs
[ER-OCI]: https://docs.microsoft.com/azure/virtual-machines/workloads/oracle/configure-azure-oci-networking



