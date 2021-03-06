---
title: ta med fil
description: ta med fil
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/21/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 5b9e036816aa532d32b1b4305ef6ae646ae05bae
ms.sourcegitcommit: fad3aaac5af8c1b3f2ec26f75a8f06e8692c94ed
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/27/2020
ms.locfileid: "67187232"
---
I följande tabell visas kraven för principbaserad-och Routningsbaserad VPN-gatewayer. Tabellen gäller både för Resource Manager- och den klassiska distributionsmodellen. För den klassiska modellen är principbaserad VPN-gatewayer samma som statiska gatewayer och vägbaserade gatewayer är samma som dynamiska gatewayer.

|  | **Principbaserad Basic VPN Gateway** | **Routningsbaserad Basic VPN Gateway** | **Routningsbaserad standard VPN Gateway** | **Routningsbaserad med höga prestanda VPN Gateway** |
| --- | --- | --- | --- | --- |
| **Plats-till-plats-anslutning   (S2S)** |Principbaserad VPN-konfiguration |Routningsbaserad VPN-konfiguration |Routningsbaserad VPN-konfiguration |Routningsbaserad VPN-konfiguration |
| **Punkt-till-plats-anslutning (P2S**) |Stöds inte |Stöds (Kan samexistera med S2S) |Stöds (Kan samexistera med S2S) |Stöds (Kan samexistera med S2S) |
| **Autentiseringsmetod** |I förväg delad nyckel |I förväg delad nyckel för S2S-anslutning, certifikat för P2S-anslutning |I förväg delad nyckel för S2S-anslutning, certifikat för P2S-anslutning |I förväg delad nyckel för S2S-anslutning, certifikat för P2S-anslutning |
| **Maximalt antal S2S-anslutningar** |1 |10 |10 |30 |
| **Maximalt antal P2S-anslutningar** |Stöds inte |128 |128 |128 |
| **Stöd för aktiv routning (BGP)** |Stöds inte |Stöds inte |Stöds |Stöds |
