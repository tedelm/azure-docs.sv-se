---
title: Fel söknings guide för direkt uppspelning | Microsoft Docs
description: Den här artikeln innehåller förslag på hur du felsöker Azure Media Services direkt uppspelnings problem.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2019
ms.author: juliako
ms.openlocfilehash: b00df5e8176aaad86be5cf3ae4e04c736f36cf5b
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/27/2020
ms.locfileid: "74885610"
---
# <a name="troubleshooting-guide-for-live-streaming"></a>Felsökningsguide för liveuppspelning  

Den här artikeln innehåller förslag på hur du kan felsöka problem med direkt uppspelning.

## <a name="issues-related-to-on-premises-encoders"></a>Problem som rör lokala kodare
Det här avsnittet innehåller förslag på hur du felsöker problem som rör lokala kodare som är konfigurerade att skicka en enda bit ström till AMS kanaler som är aktiverade för direktsänd kodning.

### <a name="problem-would-like-to-see-logs"></a>Problem: vill du se loggar
* **Möjligt problem**: det går inte att hitta kodare loggar som kan hjälpa till att felsöka problem.
  
  * **Wirecast för multistream**: du hittar oftast loggar under C:\Users\{username} \AppData\Roaming\Wirecast\ 
  * **Grundämne Live**: du hittar länkar till loggar på hanterings portalen. Klicka på **statistik**och sedan **loggar**. På sidan **loggfiler** visas en lista över loggar för alla LiveEvent-objekt. Välj den som matchar din aktuella session. 
  * **Flash Media Live Encoder**: du hittar **logg katalogen...** genom att gå till fliken **encoding-logg** .

### <a name="problem-there-is-no-option-for-outputting-a-progressive-stream"></a>Problem: det finns inget alternativ för att placera en progressiv ström
* **Möjligt problem**: kodaren som används inte automatiskt sammanflätning. 
  
    **Fel söknings steg**: Sök efter ett alternativ för att sammanflätning i kodarens gränssnitt. När de har Aktiver ATS kan du kontrol lera inställningarna för Progressive-utdata igen. 

### <a name="problem-tried-several-encoder-output-settings-and-still-unable-to-connect"></a>Problem: det har gjorts flera inställningar för kodare och kan fortfarande inte ansluta.
* **Möjligt problem**: Azure encoding Channel har inte återställts korrekt. 
  
    **Fel söknings steg**: kontrol lera att kodaren inte längre skickas till AMS, stoppa och Återställ kanalen. När du har kört igen kan du försöka ansluta din kodare med de nya inställningarna. Om detta fortfarande inte löser problemet kan du försöka att skapa en ny kanal helt, men ibland kan kanaler skadas efter flera misslyckade försök.  
* **Möjligt problem**: inställningarna för GOP storlek eller nyckel bild är inte optimala. 
  
    **Fel söknings steg**: Rekommenderad GOP storlek eller nyckel intervall är två sekunder. Vissa kodare beräknar den här inställningen i antal ramar, medan andra använder sekunder. Exempel: när du placerar 30 fps blir GOP storlek 60 ramar, vilket motsvarar 2 sekunder.  
* **Möjligt problem**: stängda portar blockerar strömmen. 
  
    **Fel söknings steg**: när du strömmar via RTMP kontrollerar du brand Väggs-och/eller proxyinställningar för att bekräfta att de utgående portarna 1935 och 1936 är öppna. 

> [!NOTE]
> Om du efter att ha använt fel söknings stegen fortfarande inte kan strömmas, skicka ett support ärende med hjälp av Azure Portal.
> 
> 

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

