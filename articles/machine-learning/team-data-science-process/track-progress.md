---
title: Spåra förloppet för process projekt för team data vetenskap
description: Hur data vetenskaps grupps chefer, grupp ledare och projekt ledare kan följa förloppet för ett data vetenskaps projekt.
author: marktab
manager: marktab
editor: marktab
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 01/10/2020
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 8cf1e5a4d97b882e7a8d0c81041bbcde709760d1
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/28/2020
ms.locfileid: "75864204"
---
# <a name="track-the-progress-of-data-science-projects"></a>Spåra förloppet för data vetenskaps projekt

Data vetenskaps grupps chefer, grupp ledare och projekt ledare kan följa förloppet för sina projekt.  Chefer vill veta vad arbetet har gjort, vem som gjorde arbetet och vilket arbete som är kvar.   Att hantera förväntningar är ett viktigt element i framgång.

## <a name="azure-devops-dashboards"></a>Azure DevOps-instrumentpaneler

Om du använder Azure DevOps kan du bygga instrument paneler för att spåra de aktiviteter och arbets objekt som är associerade med ett angivet Agile-projekt. Mer information om instrument paneler finns i [instrument paneler, rapporter och widgetar](/azure/devops/report/dashboards/).

Instruktioner för hur du skapar och anpassar instrument paneler och widgetar i Azure DevOps finns i följande snabb starter:

- [Lägga till och hantera instrument paneler](/azure/devops/report/dashboards/dashboards)
- [Lägga till widgetar i en instrument panel](/azure/devops/report/dashboards/add-widget-to-dashboard)

## <a name="example-dashboard"></a>Exempel på instrumentpanel

Här är ett enkelt exempel på en instrument panel som spårar Sprint aktiviteter i ett flexibelt data vetenskaps projekt, inklusive antalet incheckningar till tillhör ande databaser. 

- **Nedräknings** panelen visar antalet dagar som finns kvar i aktuell Sprint. 

- De två **kod panelerna** visar antalet incheckningar i de två projekt databaserna under de senaste sju dagarna. 

- **Arbets objekt för TDSP kund projekt** visar resultatet av en fråga för alla arbets objekt och deras status. 

- Ett **ackumulerat flödes diagram** (CFD) visar antalet stängda och aktiva arbets objekt.

- **Burndown-diagrammet** visar att arbete fortfarande slutförs mot den återstående tiden i sprinten.

- **Burnup-diagrammet** visar slutfört arbete jämfört med den totala mängden arbete i Sprint.

![Instrumentpanel](./media/track-progress/dashboard.png)

## <a name="next-steps"></a>Nästa steg

[Genom gångar som kör en lista över team data vetenskaps processer](walkthroughs.md) som demonstrerar alla steg i processen. De länkade scenarierna illustrerar hur du hanterar molnet och lokala resurser i intelligenta program. 
