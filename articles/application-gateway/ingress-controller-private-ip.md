---
title: Använd privat IP-adress för intern routning för en ingress-slutpunkt
description: Den här artikeln innehåller information om hur du använder privata IP-adresser för intern Routning och därmed exponerar slut punkten i ett kluster till resten av det virtuella nätverket.
services: application-gateway
author: caya
ms.service: application-gateway
ms.topic: article
ms.date: 11/4/2019
ms.author: caya
ms.openlocfilehash: 570f28ce559ff1c1180ffaacb781b9120b1890a2
ms.sourcegitcommit: fad3aaac5af8c1b3f2ec26f75a8f06e8692c94ed
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/27/2020
ms.locfileid: "73795496"
---
# <a name="use-private-ip-for-internal-routing-for-an-ingress-endpoint"></a>Använd privat IP för intern routning för en ingress-slutpunkt 

Den här funktionen gör det möjligt att exponera ingångs `Virtual Network` slut punkten i med en privat IP-adress.

## <a name="pre-requisites"></a>Förutsättningar  
Application Gateway med en [privat IP-konfiguration](https://docs.microsoft.com/azure/application-gateway/configure-application-gateway-with-private-frontend-ip)

Det finns två sätt att konfigurera styrenheten att använda privat IP för ingress,

## <a name="assign-to-a-particular-ingress"></a>Tilldela till ett visst ingress
Om du vill exponera ett visst ingress över privat IP använder du anteckningen [`appgw.ingress.kubernetes.io/use-private-ip`](./ingress-controller-annotations.md#use-private-ip) i ingress.

### <a name="usage"></a>Användning
```yaml
appgw.ingress.kubernetes.io/use-private-ip: "true"
```

För programgatewayer utan privata IP-adresser ignoreras ingressen som `appgw.ingress.kubernetes.io/use-private-ip: "true"` kommenteras med. Detta visas i loggen ingångs händelser och AGIC pod.

* Fel som anges i ingress-händelsen

    ```bash
    Events:
    Type     Reason       Age               From                                                                     Message
    ----     ------       ----              ----                                                                     -------
    Warning  NoPrivateIP  2m (x17 over 2m)  azure/application-gateway, prod-ingress-azure-5c9b6fcd4-bctcb  Ingress default/hello-world-ingress requires Application Gateway 
    applicationgateway3026 has a private IP address
    ```

* Fel som anges i AGIC-loggar

    ```bash
    E0730 18:57:37.914749       1 prune.go:65] Ingress default/hello-world-ingress requires Application Gateway applicationgateway3026 has a private IP address
    ```


## <a name="assign-globally"></a>Tilldela globalt
Om det är ett krav är det att begränsa alla ingångs händelser som ska exponeras `appgw.usePrivateIP: true` över `helm` privat IP, använda i config.

### <a name="usage"></a>Användning
```yaml
appgw:
    subscriptionId: <subscriptionId>
    resourceGroup: <resourceGroupName>
    name: <applicationGatewayName>
    usePrivateIP: true
```

Detta gör att en ingångs kontroll filtrerar IP-adresserna för en privat IP-adress när du konfigurerar klient delens lyssnare på Application Gateway.
AGIC kommer att panik och krascha om `usePrivateIP: true` ingen privat IP-adress har tilldelats.

> [!NOTE]
> Application Gateway v2-SKU kräver en offentlig IP-adress. Om du behöver Application Gateway vara privat måste du ansluta en [`Network Security Group`](https://docs.microsoft.com/azure/virtual-network/security-overview) till Application Gateway-undernätet för att begränsa trafiken.
