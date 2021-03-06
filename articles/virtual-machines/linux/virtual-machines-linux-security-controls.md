---
title: Säkerhets kontroller för Azure Virtuella Linux-datorer – Linux
description: En check lista över säkerhets kontroller för utvärdering av Azure-Virtuella Linux-datorer
services: virtual-machines
ms.service: virtual-machines
documentationcenter: ''
author: msmbaldwin
manager: rkarlin
ms.topic: conceptual
ms.date: 09/04/2019
ms.author: mbaldwin
ms.openlocfilehash: 300acaf4a9d2a11ef107e19df99452c909257d54
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/28/2020
ms.locfileid: "77190576"
---
# <a name="security-controls-for-linux-virtual-machines"></a>Säkerhets kontroller för Virtuella Linux-datorer

I den här artikeln dokumenteras de säkerhets kontroller som är inbyggda i Virtuella Linux-datorer.

[!INCLUDE [Security controls header](../../../includes/security-controls-header.md)]

## <a name="network"></a>Nätverk

| Säkerhets kontroll | Ja/nej | Obs! |
|---|---|--|
| Stöd för tjänst slut punkt| Ja | |
| Stöd för VNet-injektering| Ja | |
| Stöd för nätverks isolering och brand vägg| Ja |  |
| Stöd för Tvingad tunnel trafik| Ja | Se [Konfigurera Tvingad tunnel trafik med Azure Resource Manager distributions modell](/azure/vpn-gateway/vpn-gateway-forced-tunneling-rm). |

## <a name="monitoring--logging"></a>Övervaka & loggning

| Säkerhets kontroll | Ja/nej | Obs!|
|---|---|--|
| Azure Monitoring support (Log Analytics, App Insights osv.)| Ja | Se [övervaka och uppdatera en virtuell Linux-dator i Azure](/azure/virtual-machines/linux/tutorial-monitoring). |
| Loggning och granskning av kontroll-och hanterings plan| Ja |  |
| Loggning och granskning av data planet | Nej |  |

## <a name="identity"></a>Identitet

| Säkerhets kontroll | Ja/nej | Obs!|
|---|---|--|
| Autentisering| Ja |  |
| Auktorisering| Ja |  |

## <a name="data-protection"></a>Dataskydd

| Säkerhets kontroll | Ja/nej | Obs! |
|---|---|--|
| Kryptering på Server sidan i vila: Microsoft-hanterade nycklar | Ja | Se [Azure Disk Encryption för virtuella Linux-datorer](disk-encryption-overview.md). |
| Kryptering under överföring (till exempel ExpressRoute-kryptering, i VNet-kryptering och VNet-VNet-kryptering)| Ja | Azure Virtual Machines stöder [ExpressRoute](/azure/expressroute) och VNET-kryptering. Se [kryptering under överföring i virtuella datorer](/azure/security/security-azure-encryption-overview#in-transit-encryption-in-vms). |
| Kryptering på Server sidan på rest: Kundhanterade nycklar (BYOK) | Ja | Kundhanterade nycklar är ett Azure-krypterings scenario som stöds. Se [Översikt över Azure-kryptering](/azure/security/security-azure-encryption-overview#in-transit-encryption-in-vms).|
| Kryptering på kolumn nivå (Azure Data Services)| Ej tillämpligt | |
| Krypterade API-anrop| Ja | Via HTTPS och TLS. |

## <a name="configuration-management"></a>Konfigurationshantering

| Säkerhets kontroll | Ja/nej | Obs!|
|---|---|--|
| Konfigurations hanterings stöd (konfigurations version osv.)| Ja |  | 

## <a name="next-steps"></a>Nästa steg

- Lär dig mer om de [inbyggda säkerhets kontrollerna i Azure-tjänster](../../security/fundamentals/security-controls.md).
