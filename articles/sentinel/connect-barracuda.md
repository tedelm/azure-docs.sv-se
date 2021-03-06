---
title: Anslut Barracuda-data till Azure Sentinel | Microsoft Docs
description: Lär dig hur du ansluter Barracuda-data till Azure Sentinel.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.assetid: 3b33b4aa-7286-4d79-b461-8e1812edc2e1
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/30/2019
ms.author: yelevin
ms.openlocfilehash: 4e87bb57e6bdfea6307a166383da9dea187eea4f
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/28/2020
ms.locfileid: "77588492"
---
# <a name="connect-your-barracuda-appliance"></a>Anslut din Barracuda-apparat 



Med Barracuda WebApplication Firewall (WAF) Connector kan du enkelt ansluta Barracuda-loggar med din Azure Sentinel, för att visa instrument paneler, skapa anpassade aviseringar och förbättra undersökningen. Detta ger dig bättre insikt i din organisations nätverk och förbättrar dina säkerhets åtgärder. Azure Sentinel drar nytta av den inbyggda integreringen mellan **Barracuda** och Log Analytics agent för att tillhandahålla sömlös integrering. 


> [!NOTE]
> Data lagras på den geografiska platsen för den arbets yta där du kör Azure Sentinel.

## <a name="configure-and-connect-barracuda-waf"></a>Konfigurera och ansluta Barracuda-WAF
Barracuda WebApplication-brandvägg kan integrera och exportera loggar direkt till Azure Sentinel via Log Analytics agent.
1. Gå till [Barracuda WAF Configuration Flow](https://campus.barracuda.com/product/webapplicationfirewall/doc/73696965/configure-the-barracuda-web-application-firewall-to-integrate-with-the-oms-server-and-export-logs/)och följ anvisningarna för att konfigurera anslutningen med följande parametrar:
    - **Arbetsyte-ID**: kopiera värdet för ditt arbetsyte-ID från Azure Sentinel Barracuda Connector-sidan.
    - **Primär nyckel**: kopiera värdet för din primär nyckel från anslutnings sidan för Azure Sentinel Barracuda.
1. Om du vill använda det relevanta schemat i Log Analytics för Barracuda-händelserna söker du efter **CommonSecurityLog** och **barracuda_CL**.


## <a name="validate-connectivity"></a>Verifiera anslutning

Det kan ta upp till 20 minuter innan loggarna börjar visas i Log Analytics. 



## <a name="next-steps"></a>Nästa steg
I det här dokumentet har du lärt dig hur du ansluter Barracuda-enheter till Azure Sentinel. Mer information om Azure Sentinel finns i följande artiklar:
- Lär dig hur du [får insyn i dina data och potentiella hot](quickstart-get-visibility.md).
- Kom igång [med att identifiera hot med Azure Sentinel](tutorial-detect-threats-built-in.md).
- [Använd arbets böcker](tutorial-monitor-your-data.md) för att övervaka dina data.


