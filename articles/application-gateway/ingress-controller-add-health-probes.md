---
title: Lägga till hälso avsökningar i din AKS-poddar
description: Den här artikeln innehåller information om hur du lägger till hälso avsökningar (beredskap och/eller Lives) för att AKS poddar med en Application Gateway.
services: application-gateway
author: caya
ms.service: application-gateway
ms.topic: article
ms.date: 11/4/2019
ms.author: caya
ms.openlocfilehash: 5d0543a3a43d53e462a6406312faddf37d2653c6
ms.sourcegitcommit: fad3aaac5af8c1b3f2ec26f75a8f06e8692c94ed
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/27/2020
ms.locfileid: "73795598"
---
# <a name="add-health-probes-to-your-service"></a>Lägg till hälso avsökningar i din tjänst
Som standard etablerar ingress Controller en HTTP GET-avsökning för exponerade poddar.
Du kan anpassa egenskaper för avsökning genom att lägga till en [beredskaps-eller Direktmigrering](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) till din `deployment` / `pod` specifikation.

## <a name="with-readinessprobe-or-livenessprobe"></a>Med `readinessProbe` eller`livenessProbe`
```yaml
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: aspnetapp
spec:
  replicas: 3
  template:
    metadata:
      labels:
        service: site
    spec:
      containers:
      - name: aspnetapp
        image: mcr.microsoft.com/dotnet/core/samples:aspnetapp
        imagePullPolicy: IfNotPresent
        ports:
        - containerPort: 80
        readinessProbe:
          httpGet:
            path: /
            port: 80
          periodSeconds: 3
          timeoutSeconds: 1
```

Kubernetes API-referens:
* [Container PROBE](https://kubernetes.io/docs/concepts/workloads/pods/pod-lifecycle/#container-probes)
* [HttpGet-åtgärd](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.14/#httpgetaction-v1-core)

> [!NOTE]
> * `readinessProbe`och `livenessProbe` stöds när de konfigureras `httpGet`med.
> * Det finns för närvarande inte stöd för avsökning på en annan port än den som exponeras på pod.
> * `HttpHeaders`, `InitialDelaySeconds`, `SuccessThreshold` stöds inte.

##  <a name="without-readinessprobe-or-livenessprobe"></a>Utan `readinessProbe` eller`livenessProbe`
Om avsökningarna ovan inte anges, gör ingångs enheten ett antagande om att tjänsten kan `Path` kontaktas för `backend-path-prefix` antecknings anteckning eller som `path` anges i `ingress` definitionen för tjänsten.

## <a name="default-values-for-health-probe"></a>Standardvärden för hälso avsökning
Standardvärden anges för en egenskap som inte kan härledas av beredskap/direktmigreringen.

| Application Gateway avsöknings egenskap | Standardvärde |
|-|-|
| `Path` | / |
| `Host` | localhost |
| `Protocol` | HTTP |
| `Timeout` | 30 |
| `Interval` | 30 |
| `UnhealthyThreshold` | 3 |