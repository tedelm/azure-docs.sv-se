---
title: 'Självstudie: röster aktivera din robot med tal-SDK – tal-tjänst'
titleSuffix: Azure Cognitive Services
description: I den här självstudien får du skapa en eko-robot med hjälp av Microsoft bot-Framework, distribuera den till Azure och registrera den med bot-ramverkets direkta linje tal kanal. Sedan konfigurerar du en exempel klient app för Windows som låter dig tala med din robot och höra att den svarar på dig.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 02/25/2020
ms.author: trbye
ms.openlocfilehash: c55d81db848dcb1aebe9dacb03387565b3d8db48
ms.sourcegitcommit: 493b27fbfd7917c3823a1e4c313d07331d1b732f
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 05/21/2020
ms.locfileid: "83745600"
---
# <a name="tutorial-voice-enable-your-bot-using-the-speech-sdk"></a>Självstudie: röst – aktivera din robot med tal-SDK

Nu kan du använda kraften i tal tjänsten för att enkelt röst aktivera en chatt-robot.

I den här självstudien får du skapa en eko-robot med hjälp av Microsoft bot-Framework, distribuera den till Azure och registrera den med bot-ramverkets direkta linje tal kanal. Sedan konfigurerar du en exempel klient app för Windows som låter dig tala med din robot och höra att den svarar på dig.

Den här självstudien är utformad för utvecklare som bara startar sin resa med Azure, bot-Framework-robotar, direkt linje tal eller tal-SDK och vill snabbt bygga ett fungerande system med begränsad kodning. Det behövs ingen erfarenhet eller erfarenhet av dessa tjänster.

I slutet av den här övningen har du konfigurerat ett system som fungerar på följande sätt:

1. Exempel klient programmet är konfigurerat för att ansluta till direkt linje tal kanal och eko roboten
1. Ljud spelas in från standard mikrofonen vid knapp tryckning (eller kontinuerligt inspelad om anpassat nyckelord är aktiverat)
1. Du kan också välja att identifiera anpassade nyckelord, hantera ljud strömning till molnet
1. Med hjälp av tal-SDK ansluter appen till direkt linje tal kanal och strömma ljud
1. Du kan också kontrol lera att nyckelord med högre noggrannhet sker i tjänsten
1. Ljudet skickas till tal igenkännings tjänsten och skrivs till text
1. Den tolkade texten skickas till eko-roboten som en bot Framework-aktivitet 
1. Svars texten omvandlas till ljud av tjänsten text till tal (TTS) och strömmas tillbaka till klient programmet för uppspelning

![diagram-tagg](media/tutorial-voice-enable-your-bot-speech-sdk/diagram.png "Tal kanal flödet")

> [!NOTE]
> Stegen i den här självstudien kräver ingen betald tjänst. Som en ny Azure-användare kan du använda krediter från din kostnads fria utvärderings prenumeration på Azure och den kostnads fria nivån av tal tjänsten för att slutföra den här kursen.

Den här själv studie kursen beskriver följande:
> [!div class="checklist"]
> * Skapa nya Azure-resurser
> * Bygg, testa och distribuera eko robot exemplet till en Azure App Service
> * Registrera din robot med direkt linje tal kanal
> * Skapa och kör Windows Voice Assistant-klienten för att interagera med din eko-robot
> * Lägg till anpassad nyckelords aktivering
> * Lär dig att ändra språket för det identifierade och talade talet

## <a name="prerequisites"></a>Krav

Det här är vad du behöver för att slutföra den här kursen:

- En Windows 10-dator med en fungerande mikrofon och högtalare (eller hörlurar)
- [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) eller senare
- [.NET Framework runtime 4.6.1](https://dotnet.microsoft.com/download) eller högre
- Ett Azure-konto. [Registrera dig kostnads fritt](https://azure.microsoft.com/free/ai/).
- Ett [GitHub](https://github.com/) -konto
- [Git för Windows](https://git-scm.com/download/win)

## <a name="create-a-resource-group"></a>Skapa en resursgrupp

Klient programmet som du skapar i den här självstudien använder en fåtal av Azure-tjänster. Om du vill minska svars tiden för svar från din robot bör du se till att dessa tjänster finns i samma Azure-region. I det här avsnittet ska du skapa en resurs grupp i regionen **USA, västra** . Den här resurs gruppen kommer att användas när du skapar enskilda resurser för bot-ramverket, direkt linje tal kanalen och tal tjänsten.

1. <a href="https://ms.portal.azure.com/#create/Microsoft.ResourceGroup" target="_blank">Skapa en resurs grupp<span class="docon docon-navigate-external x-hidden-focus"></span></a>
1. Du uppmanas att ange viss information:
   * Ställ in **prenumeration** på **kostnads fri utvärderings version** (du kan också använda en befintlig prenumeration).
   * Ange ett namn för **resurs gruppen**. Vi rekommenderar **SpeechEchoBotTutorial-ResourceGroup**.
   * I list rutan **region** väljer du **västra USA**.
1. Klicka på **Granska och skapa**. Du bör se en banderoll som läser **verifieringen**.
1. Klicka på **Skapa**. Det kan ta några minuter att skapa resurs gruppen.
1. Precis som med de resurser du skapar senare i den här självstudien är det en bra idé att fästa resurs gruppen på instrument panelen för enkel åtkomst. Om du vill fästa resurs gruppen klickar du på PIN-ikonen längst upp till höger på instrument panelen.

### <a name="choosing-an-azure-region"></a>Välja en Azure-region

Om du vill använda en annan region för den här självstudien kan de här faktorerna begränsa dina val:

* Se till att du använder en [Azure-region som stöds](regions.md#voice-assistants).
* Den direkta linjens tal kanal använder tjänsten för text till tal, som har standard-och neurala-röster. Neurala-röster är [begränsade till särskilda Azure-regioner](regions.md#standard-and-neural-voices).
* De kostnads fria utvärderings nycklarna kan vara begränsade till en speciell region.

Mer information om regioner finns i [Azure-platser](https://azure.microsoft.com/global-infrastructure/locations/).

## <a name="create-resources"></a>Skapa resurser

Nu när du har en resurs grupp i en region som stöds, är nästa steg att skapa enskilda resurser för varje tjänst som du kommer att använda i den här självstudien.

### <a name="create-a-speech-service-resource"></a>Skapa en tjänst resurs för tal

Följ de här anvisningarna för att skapa en tal resurs:

1. <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesSpeechServices" target="_blank">Skapa en tjänst resurs för tal<span class="docon docon-navigate-external x-hidden-focus"></span></a>
4. Du uppmanas att ange viss information:
   * Ge din resurs ett **namn**. Vi rekommenderar **SpeechEchoBotTutorial-tal**
   * För **prenumeration**kontrollerar du att den **kostnads fria utvärderings versionen** har valts.
   * För **plats**väljer du **västra USA**.
   * För **pris nivå**väljer du **F0**. Detta är den kostnads fria nivån.
   * För **resurs grupp**väljer du **SpeechEchoBotTutorial-ResourceGroup**.
5. När du har angett all information som krävs klickar du på **skapa**. Det kan ta några minuter att skapa din resurs.
6. Senare i den här självstudien behöver du prenumerations nycklar för den här tjänsten. Du kan komma åt dessa nycklar när som helst från resursens **Översikt** (hantera nycklar) eller **nycklar**.

I det här läget kontrollerar du att resurs gruppen (**SpeechEchoBotTutorial-ResourceGroup**) har en tal resurs:

| Name | Typ  | Plats |
|------|-------|----------|
| SpeechEchoBotTutorial-tal | Cognitive Services | USA, västra |

### <a name="create-an-azure-app-service-plan"></a>Skapa en Azure App Service-plan

Nästa steg är att skapa en App Service-plan. En App Service-plan definierar en uppsättning beräkningsresurser som en webbapp ska köra.

1. <a href="https://ms.portal.azure.com/#create/Microsoft.AppServicePlanCreate" target="_blank">Skapa en Azure App Service plan<span class="docon docon-navigate-external x-hidden-focus"></span></a>
4. Du uppmanas att ange viss information:
   * Ställ in **prenumeration** på **kostnads fri utvärderings version** (du kan också använda en befintlig prenumeration).
   * För **resurs grupp**väljer du **SpeechEchoBotTutorial-ResourceGroup**.
   * Ge din resurs ett **namn**. Vi rekommenderar **SpeechEchoBotTutorial-AppServicePlan**
   * För **operativ system**väljer du **Windows**.
   * För **region**väljer du **västra USA**.
   * Se till att **standard S1** är valt för **pris nivå**. Detta ska vara standardvärdet. Om den inte är det, se till att du ställer in **operativ systemet** på **Windows** enligt beskrivningen ovan.
5. Klicka på **Granska och skapa**. Du bör se en banderoll som läser **verifieringen**.
6. Klicka på **Skapa**. Det kan ta några minuter att skapa resurs gruppen.

I det här läget kontrollerar du att resurs gruppen (**SpeechEchoBotTutorial-ResourceGroup**) har två resurser:

| Name | Typ  | Plats |
|------|-------|----------|
| SpeechEchoBotTutorial-AppServicePlan | App Service-plan | USA, västra |
| SpeechEchoBotTutorial-tal | Cognitive Services | USA, västra |

## <a name="build-an-echo-bot"></a>Bygg en eko-robot

Nu när du har skapat några resurser är det dags att skapa en bot. Vi kommer att börja med ekot-robot-exemplet, som namnet antyder, upprepar den text som du har angett som svar. Oroa dig inte, exempel koden är redo att användas utan några ändringar. Den har kon figurer ATS för att fungera med den direkta rad igenkännings kanalen, som vi ska ansluta när vi har distribuerat bot till Azure.

> [!NOTE]
> Anvisningarna som följer samt ytterligare information om eko roboten finns i [exempel filen Readme på GitHub](https://github.com/microsoft/BotBuilder-Samples/blob/master/samples/csharp_dotnetcore/02.echo-bot/README.md).

### <a name="run-the-bot-sample-on-your-machine"></a>Kör robot-exemplet på din dator

1. Klona exempel lagrings platsen:

   ```bash
   git clone https://github.com/Microsoft/botbuilder-samples.git
   ```

2. Starta Visual Studio.
3. I verktygsfältet väljer du **Arkiv**  >  **Öppna**  >  **projekt/lösning**och öppnar lösningen för eko-bot-projekt:

   ```
   samples\csharp_dotnetcore\02.echo-bot\EchoBot.sln
   ```

4. När projektet har lästs in trycker du på <kbd>F5</kbd> för att skapa och köra projektet.
5. En webbläsare ska starta och du ser en skärm som ser ut ungefär så här.
    > [!div class="mx-imgBorder"]
    > [![Echobot – localhost](media/tutorial-voice-enable-your-bot-speech-sdk/echobot-running-on-localhost.png "EchoBot körs på localhost")](media/tutorial-voice-enable-your-bot-speech-sdk/echobot-running-on-localhost.png#lightbox)

### <a name="test-the-bot-sample-with-the-bot-framework-emulator"></a>Testa robot-exemplet med bot Framework-emulatorn

[Bot Framework-emulatorn](https://github.com/microsoft/botframework-emulator) är en Skriv bords app som gör det möjligt för bot-utvecklare att testa och felsöka sina robotar lokalt eller via en tunnel. Emulatorn stöder skriven text som inmatad (inte röst). Roboten kommer att svara med text. Följ dessa steg om du vill använda bot Framework-emulatorn för att testa din eko-robot som körs lokalt, med text inmatningar och text utdata. När vi har distribuerat roboten till Azure kommer vi att testa den med röst indata och röst resultat.

1. Installera [bot Framework-emulatorns](https://github.com/Microsoft/BotFramework-Emulator/releases/latest) version 4.3.0 eller senare
2. Starta bot Framework-emulatorn och öppna din robot:
   * **Fil**  ->  **Öppna bot**.
3. Ange URL: en för din robot. Ett exempel:

   ```
   http://localhost:3978/api/messages
   ```
   och tryck på "Anslut".
4. Roboten bör omedelbart vara i hälsning med "Hello och Välkommen!" . Skriv ett textmeddelande och bekräfta att du får svar från roboten.
5. Detta är vad ett utbyte av kommunikation med en ECHO bot-instans kan se ut så här: [ ![bot-Framework-emulator](media/tutorial-voice-enable-your-bot-speech-sdk/bot-framework-emulator.png "Bot Framework-emulator")](media/tutorial-voice-enable-your-bot-speech-sdk/bot-framework-emulator.png#lightbox)

## <a name="deploy-your-bot-to-an-azure-app-service"></a>Distribuera din robot till en Azure App Service

Nästa steg är att distribuera eko-roboten till Azure. Det finns några sätt att distribuera en bot, men i den här självstudien fokuserar vi på att publicera direkt från Visual Studio.

> [!NOTE]
> Alternativt kan du distribuera en robot med hjälp av mallar för [Azure CLI](https://docs.microsoft.com/azure/bot-service/bot-builder-deploy-az-cli) och [distribution](https://github.com/microsoft/BotBuilder-Samples/tree/master/samples/csharp_dotnetcore/adaptive-dialog/03.core-bot).

1. Öppna eko-roboten som har kon figurer ATS för användning med direkt linje tal kanal i Visual Studio:

   ```
   samples\csharp_dotnetcore\02.echo-bot\EchoBot.sln
   ```

1. Högerklicka på projektet **EchoBot** i **Solution Explorer**och välj **publicera...**
1. Ett nytt fönster med namnet **Välj ett publicerings mål** öppnas.
1. Välj **App Service** i navigeringen för **Azure-tjänster** , Välj **Skapa ny**och klicka sedan på **Skapa profil**.
1. När fönstret **skapa App Service** visas:
   * Klicka på **Lägg till ett konto**och logga in med dina autentiseringsuppgifter för Azure-kontot. Om du redan är inloggad väljer du önskat konto i list rutan.
   * För **namnet på appen**måste du ange ett globalt unikt namn för din robot. Det här namnet används för att skapa en unik bot-URL. Ett standardvärde kommer att fyllas i, inklusive datum och tid (till exempel: "EchoBot20190805125647"). Du kan använda standard namnet för den här självstudien.
   * För **prenumeration**ställer du in den på **kostnads fri utvärderings version**
   * För **resurs grupp**väljer du **SpeechEchoBotTutorial-ResourceGroup**
   * För **värd plan**väljer du **SpeechEchoBotTutorial-AppServicePlan**
   * För **Application Insights**lämnar du som **none**
1. Klicka på **skapa**
1. Du bör se ett meddelande som visar att det är klart i Visual Studio som ser ut så här:

   ```
   Publish Succeeded.
   Web App was published successfully https://EchoBot20190805125647.azurewebsites.net/
   ```

1. Din standard webbläsare bör öppna och visa en sida som läser: "din robot är klar!".
1. I det här läget kontrollerar du resurs gruppen **SpeechEchoBotTutorial-ResourceGroup** i Azure Portal och kontrollerar att det finns tre resurser:

| Name | Typ  | Plats |
|------|-------|----------|
| EchoBot20190805125647 | App Service | USA, västra |
| SpeechEchoBotTutorial-AppServicePlan | App Service-plan | USA, västra |
| SpeechEchoBotTutorial-tal | Cognitive Services | USA, västra |

## <a name="enable-web-sockets"></a>Aktivera webb-socketar

Du måste göra en liten konfigurations ändring så att din robot kan kommunicera med den direkta rad igenkännings kanalen med hjälp av Web Sockets. Följ de här stegen för att aktivera Web Sockets:

1. Navigera till [Azure Portal](https://portal.azure.com)och leta upp din app service. Resursen ska ha namnet liknar **EchoBot20190805125647** (ditt unika app-namn).
2. I navigeringen för **Azure-tjänster** , under **Inställningar**, klickar du på **konfiguration**.
3. Välj fliken **allmänna inställningar** .
4. Leta upp växlingen för **Web Sockets** och Ställ in den på **på**.
5. Klicka på **Spara**.

> [!TIP]
> Du kan använda kontrollerna överst på Azure App Service sidan för att stoppa eller starta om tjänsten. Detta kan komma att vara användbart vid fel sökning.

## <a name="create-a-channel-registration"></a>Skapa en kanal registrering

Nu när du har skapat en Azure App Service som värd för din robot, är nästa steg att skapa en **robot Channel-registrering**. Att skapa en kanal registrering är ett krav för att registrera din robot med bot-Framework-kanaler, inklusive direkt linje tal kanal.

> [!NOTE]
> Om du vill veta mer om hur robotar använder kanaler kan du läsa [Anslut en robot till kanaler](https://docs.microsoft.com/azure/bot-service/bot-service-manage-channels?view=azure-bot-service-4.0).


1. <a href="https://ms.portal.azure.com/#create/Microsoft.BotServiceConnectivityGalleryPackage" target="_blank">Skapa en Azure bot Channel-registrering<span class="docon docon-navigate-external x-hidden-focus"></span></a>
2. Du uppmanas att ange viss information:
   * För **bot-referens**anger du **SpeechEchoBotTutorial-BotRegistration**.
   * För **prenumeration**väljer du **kostnads fri utvärdering**.
   * För **resurs grupp**väljer du **SpeechEchoBotTutorial-ResourceGroup**.
   * För **plats**väljer du **västra USA**.
     * För **pris nivå**väljer du **F0**.
     * För **meddelande slut punkt**anger du URL: en för din webbapp med `/api/messages` sökvägen som läggs till i slutet. Exempel: om ditt globalt unika app-namn var **EchoBot20190805125647**skulle meddelande slut punkten vara: `https://EchoBot20190805125647.azurewebsites.net/api/messages/` .
     * För **Application Insights**kan du ange till **av**. Mer information finns i [bot Analytics](https://docs.microsoft.com/azure/bot-service/bot-service-manage-analytics?view=azure-bot-service-4.0).
     * Ignorera **Auto skapa app-ID och lösen ord**.
5. Klicka på **skapa**längst ned på bladet **robot Channels Registration** .

I det här läget kontrollerar du resurs gruppen **SpeechEchoBotTutorial-ResourceGroup** i Azure Portal. Nu bör det Visa fyra resurser:

| Name | Typ  | Plats |
|------|-------|----------|
| EchoBot20190805125647 | App Service | USA, västra |
| SpeechEchoBotTutorial-AppServicePlan | App Service-plan | USA, västra |
| SpeechEchoBotTutorial-BotRegistration | Registrering av robot kanaler | EAN |
| SpeechEchoBotTutorial-tal | Cognitive Services | USA, västra |

> [!IMPORTANT]
> Registrerings resursen för robot kanaler visar den globala regionen även om du valde västra USA. Detta är förväntat.

## <a name="register-the-direct-line-speech-channel"></a>Registrera den direkta linjens tal kanal

Nu är det dags att registrera din robot med den direkta rad igenkännings kanalen. Den här kanalen är det som används för att skapa en anslutning mellan din eko-robot och en klient app som kompileras med talet SDK.

1. Leta upp och öppna din **SpeechEchoBotTutorial-BotRegistration-** resurs i [Azure Portal](https://portal.azure.com).
1. I navigeringen för **Azure-tjänster** väljer du **kanaler**.
   * Leta efter **fler kanaler**, leta upp och klicka på **direkt linje tal**.
   * Granska texten på sidan **Konfigurera direkt linje tal**och expandera sedan den nedrullningsbara menyn med namnet "kognitivt tjänst konto".
   * Välj den tal resurs som du skapade tidigare (t. ex. **SpeechEchoBotTutorial-tal**) på menyn för att koppla din robot till din tal prenumerations nyckel.
   * Klicka på **Spara**.

1. Klicka på **Inställningar**i navigeringen för **bot hantering** .
   * Markera kryss rutan **Aktivera direkt uppspelnings slut punkt**. Detta krävs för att aktivera ett kommunikations protokoll som bygger på Web sockets mellan din robot och den direkta rad igenkännings kanalen.
   * Klicka på **Spara**.

> [!TIP]
> Om du vill veta mer, se [Anslut en robot till direkt linje tal](https://docs.microsoft.com/azure/bot-service/bot-service-channel-connect-directlinespeech?view=azure-bot-service-4.0). Den här sidan innehåller ytterligare information och kända problem.

## <a name="run-the-windows-voice-assistant-client"></a>Kör Windows Voice Assistant-klienten

I det här steget ska du köra Windows Voice Assistant-klienten. -Klienten är en Windows Presentation Foundation-app (WPF) i C# som använder [tal-SDK](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk) för att hantera kommunikation med din robot med hjälp av direkt linje tal kanalen. Använd den för att interagera med och testa din robot innan du skriver en anpassad klient app. Det är öppen källkod, så du kan antingen hämta den körbara filen och köra den, eller skapa den själv.

Windows Voice Assistant-klienten har ett enkelt användar gränssnitt som gör att du kan konfigurera anslutningen till din robot, Visa text konversationen, Visa bot-Framework-aktiviteter i JSON-format och Visa anpassningsbara kort. Det stöder också användning av anpassade nyckelord. Du använder den här klienten för att prata med din robot och få ett röst svar.

Innan vi går vidare kontrollerar du att mikrofonen och högtalarna är aktiverade och fungerar.

1. Navigera till GitHub-lagringsplatsen för [Windows Voice Assistant-klienten](https://github.com/Azure-Samples/Cognitive-Services-Voice-Assistant/blob/master/clients/csharp-wpf/README.md).
1. Följ anvisningarna som tillhandahölls antingen
   * Hämta ett ZIP-paket som innehåller den körbara fil som ska köras eller
   * skapa den körbara filen själv, genom att klona lagrings platsen och skapa projektet.

1. Starta klient programmet och konfigurera det.
1. Klicka på **Återanslut igen** och se till att du ser meddelandet **Tryck på knappen MIC eller Skriv för att börja prata med din robot**.
1. Låt oss testa det. Klicka på mikrofon knappen och tala om några ord på engelska. Den tolkade texten visas i takt med att du talar. När du är klar kommer roboten att svara i sin egen röst och säga "echo" följt av de identifierade orden.
1. Du kan också använda text för att kommunicera med bot. Skriv bara texten i det nedre fältet. 

### <a name="troubleshooting-errors-in-windows-voice-assistant-client"></a>Fel sökning av fel i Windows Voice Assistant-klienten

Om du får ett fel meddelande i huvud fönstret i appen använder du den här tabellen för att identifiera och felsöka felet:

| Fel | Hur ska du göra? |
|-------|----------------------|
|Fel AuthenticationFailure: WebSocket-uppgraderingen misslyckades med ett autentiseringsfel (401). Sök efter rätt prenumerations nyckel (eller autentiseringstoken) och region namn| På sidan Inställningar i appen kontrollerar du att du har angett tal prenumerations nyckeln och dess region korrekt.<br>Kontrol lera att din tal nyckel och nyckel region har angetts korrekt. |
|Fel ConnectionFailure: anslutningen stängdes av den fjärranslutna värden. Felkod: 1011. Fel information: vi kunde inte ansluta till bot innan ett meddelande skickades | Kontrol lera att du har [markerat kryss rutan "Aktivera direkt uppspelnings slut punkt"](#register-the-direct-line-speech-channel) och/eller [växlade **webb-Sockets** ](#enable-web-sockets) till på.<br>Kontrol lera att din Azure App Service körs. Om det är fallet kan du försöka starta om App Service.|
|Fel ConnectionFailure: anslutningen stängdes av den fjärranslutna värden. Felkod: 1011. Fel information: svars status koden indikerar inte lyckad: 500 (InternalServerError)| Din robot har angett en neurala röst i sitt [Speak](https://github.com/microsoft/botframework-sdk/blob/master/specs/botframework-activity/botframework-activity.md#speak) -fält för utdata-aktivitet, men den Azure-region som är associerad med din tal prenumerations nyckel stöder inte neurala-röster. Se [standard-och neurala-röster](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#standard-and-neural-voices).|
|Fel ConnectionFailure: anslutningen stängdes av den fjärranslutna värden. Felkod: 1000. Fel information: överskrider maximal inaktiv varaktighet för WebSocket-anslutning (> 300000 MS)| Detta är ett förväntat fel när en anslutning till kanalen lämnas öppen och inaktiv i mer än fem minuter. |

Om problemet inte har åtgärd ATS i tabellen, se [röst assistenter: vanliga frågor och svar](faq-voice-assistants.md).

### <a name="view-bot-activities"></a>Visa bot-aktiviteter

Varje robot skickar och tar emot **aktivitets** meddelanden. I **aktivitets logg** fönstret i Windows Voice Assistant-klienten ser du de tidsstämplade loggarna med varje aktivitet som klienten har tagit emot från bot. Du kan också se de aktiviteter som klienten har skickat till roboten med hjälp av- [`DialogServiceConnector.SendActivityAsync`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.dialog.dialogserviceconnector.sendactivityasync) metoden. När du väljer ett logg objekt visas information om den associerade aktiviteten som JSON.

Här är ett exempel-JSON för en aktivitet som klienten tar emot:

```json
{
    "attachments":[],
    "channelData":{
        "conversationalAiData":{
             "requestInfo":{
                 "interactionId":"8d5cb416-73c3-476b-95fd-9358cbfaebfa",
                 "version":"0.2"
             }
         }
    },
    "channelId":"directlinespeech",
    "conversation":{
        "id":"129ebffe-772b-47f0-9812-7c5bfd4aca79",
        "isGroup":false
    },
    "entities":[],
    "from":{
        "id":"SpeechEchoBotTutorial-BotRegistration"
    },
    "id":"89841b4d-46ce-42de-9960-4fe4070c70cc",
    "inputHint":"acceptingInput",
    "recipient":{
        "id":"129ebffe-772b-47f0-9812-7c5bfd4aca79|0000"
    },
    "replyToId":"67c823b4-4c7a-4828-9d6e-0b84fd052869",
    "serviceUrl":"urn:botframework:websocket:directlinespeech",
    "speak":"<speak version='1.0' xmlns='https://www.w3.org/2001/10/synthesis' xml:lang='en-US'><voice name='Microsoft Server Speech Text to Speech Voice (en-US, AriaRUS)'>Echo: Hello and welcome.</voice></speak>",
    "text":"Echo: Hello and welcome.",
    "timestamp":"2019-07-19T20:03:51.1939097Z",
    "type":"message"
}
```

Mer information om vad som returneras i JSON-utdata finns i [fält i aktiviteten](https://github.com/microsoft/botframework-sdk/blob/master/specs/botframework-activity/botframework-activity.md). I den här självstudien kan du fokusera på fälten [text](https://github.com/microsoft/botframework-sdk/blob/master/specs/botframework-activity/botframework-activity.md#text) och [tala](https://github.com/microsoft/botframework-sdk/blob/master/specs/botframework-activity/botframework-activity.md#speak) .

### <a name="view-client-source-code-for-calls-to-the-speech-sdk"></a>Visa klient käll koden för anrop till tal-SDK

Windows Voice Assistant-klienten använder NuGet-paketet [Microsoft. CognitiveServices. Speech](https://www.nuget.org/packages/Microsoft.CognitiveServices.Speech/)som innehåller talet SDK. En bra plats för att börja granska exempel koden är metoden InitSpeechConnector () i filen [`VoiceAssistantClient\MainWindow.xaml.cs`](https://github.com/Azure-Samples/Cognitive-Services-Voice-Assistant/blob/master/clients/csharp-wpf/VoiceAssistantClient/MainWindow.xaml.cs) , som skapar dessa två tal SDK-objekt:
- [`DialogServiceConfig`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.dialog.dialogserviceconfig)– För konfigurations inställningar (t. ex. tal prenumerations nyckel, nyckel region)
- [`DialogServiceConnector`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.dialog.dialogserviceconnector.-ctor)-För att hantera kanal anslutningen och klient prenumerations händelser för hantering av identifierade tal-och robot svar.

## <a name="add-custom-keyword-activation"></a>Lägg till anpassad nyckelords aktivering

Tal-SDK: n stöder anpassad nyckelords aktivering. Precis som med "Hej Cortana" för Microsofts assistent kan du skriva en app som kontinuerligt lyssnar efter valfritt sökord. Tänk på att ett nyckelord kan vara ett enstaka ord eller en fras med flera ord.

> [!NOTE]
> *Nyckelordet* term används ofta i båda fallen med termen *aktiverings ord*, och du kan se båda som används i Microsoft-dokumentationen.

Nyckelords identifiering görs i klient programmet. Om du använder ett nyckelord strömmas ljudet bara till direkt linjens tal kanal om nyckelordet identifieras. Den direkta linjens tal kanal innehåller en komponent som kallas *nyckelords verifiering (KWV)*, som gör mer komplex bearbetning i molnet för att verifiera att det nyckelord du har valt är i början av ljud strömmen. Om verifieringen av nyckel ord lyckas kommer kanalen att kommunicera med bot.

Följ de här stegen för att skapa en nyckelords modell, konfigurera Windows Voice Assistant-klienten att använda den här modellen och testa den med din robot.

1. Följ de här anvisningarna för att [skapa ett anpassat nyckelord med hjälp av tal tjänsten](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-devices-sdk-create-kws).
2. Zippa upp modell filen som du laddade ned i föregående steg. Den ska namnges för ditt nyckelord. Du letar efter en fil med namnet `kws.table` .
3. I Windows Voice Assistant-klienten, leta upp menyn **Inställningar** (leta efter kugg hjuls ikonen längst upp till höger). Leta reda på **modell filens sökväg** och ange den fullständiga sökvägen för `kws.table` filen från steg 2.
4. Se till att markera kryss rutan med etiketten **aktive rad**. Du bör se meddelandet bredvid kryss rutan: "lyssnar efter nyckelordet vid nästa anslutning". Om du har angett fel fil eller ogiltig sökväg bör du se ett fel meddelande.
5. Ange din röst **prenumerations nyckel**, **prenumerations nyckel region**och klicka sedan på **OK** för att stänga menyn **Inställningar** .
6. Klicka på **Återanslut**. Du bör se ett meddelande som läser: "ny konversation har startats", tryck på mikrofon knappen eller säg nyckelordet ". Appen lyssnar nu kontinuerligt.
7. Tala om vilken fras som helst som börjar med ditt nyckelord. Till exempel: "**{ditt nyckelord}**, vilken tid är det?". Du behöver inte pausa efter att du har gjort ett nyckelord. När det är klart händer två saker:
   * Du ser en avskrift av det du eker
   * Strax efter kan du höra robotens svar
8. Fortsätt att experimentera med de tre indatatyper som din robot stöder:
   * Skriven text i det nedre fältet
   * Tryck på mikrofon ikonen och tala
   * Säga en fras som börjar med ditt nyckelord

### <a name="view-the-source-code-that-enables-keyword"></a>Visa käll koden som aktiverar nyckelord

Ta en titt på de här filerna i klient käll koden för Windows röst assistenten och granska koden som används för att aktivera nyckelords identifiering:

1. [`VoiceAssistantClient\Models.cs`](https://github.com/Azure-Samples/Cognitive-Services-Voice-Assistant/blob/master/clients/csharp-wpf/VoiceAssistantClient/Models.cs)innehåller ett anrop till metoden Speech SDK [`KeywordRecognitionModel.fromFile()`](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/keywordrecognitionmodel?view=azure-node-latest#fromfile-string-) som används för att instansiera modellen från en lokal fil på disk.
1. [`VoiceAssistantClient\MainWindow.xaml.cs`](https://github.com/Azure-Samples/Cognitive-Services-Voice-Assistant/blob/master/clients/csharp-wpf/VoiceAssistantClient/MainWindow.xaml.cs)innehåller ett anrop till metoden Speech SDK [`DialogServiceConnector.StartKeywordRecognitionAsync()`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.dialog.dialogserviceconnector.startkeywordrecognitionasync) som aktiverar kontinuerlig identifiering av nyckelord.

## <a name="optional-change-the-language-and-bot-voice"></a>Valfritt Ändra språk och bot-röst

Roboten som du har skapat kommer att lyssna efter och svara på engelska, med en standard engelsk text till tal-röst. Men du är inte begränsad till att använda engelska eller en standard röst. I det här avsnittet får du lära dig hur du ändrar språket som roboten ska lyssna efter och svara på. Du får också lära dig hur du väljer en annan röst för det språket.

### <a name="change-the-language"></a>Ändra språk

Du kan välja något av de språk som anges i tabellen [tal till text](language-support.md#speech-to-text) . I exemplet nedan kommer vi att ändra språket till tyska.

1. Öppna klient programmet för Windows Voice Assistant, klicka på knappen Inställningar (övre högra kugg hjuls ikonen) och ange `de-de` i fältet språk (detta är det språkvariant-värde som anges i [tal till text-](language-support.md#speech-to-text) tabellen). Detta anger det talade språket som ska identifieras och åsidosätter standardvärdet `en-us` . Det instruerar också direkt linje tal kanalen att använda en standard-tysk röst för bot-svaret.
2. Stäng sidan Inställningar och klicka på knappen Återanslut för att upprätta en ny anslutning till din eko-robot.
3. Klicka på mikrofon knappen och säg en fras på tyska. Den tolkade texten och eko-roboten visas med den tyska standard rösten.

### <a name="change-the-default-bot-voice"></a>Ändra standard röst för bot

Att välja rösten text till tal och kontrol lera uttal kan göras om bot anger svaret i form av ett [tal syntes kodspråk](speech-synthesis-markup.md) (SSML) i stället för enkel text. Eko-roboten använder inte SSML, men vi kan enkelt ändra koden för att göra det. I exemplet nedan lägger vi till SSML till eko robot svaret, till exempel att den tyska rösten Stefan Apollo (en hane-röst) kommer att användas i stället för standard kvinnligt röst. Se lista över [standard röster](language-support.md#standard-voices) och [neurala-röster](language-support.md#neural-voices) som stöds för ditt språk.

1. Vi börjar med att öppna `samples\csharp_dotnetcore\02.echo-bot\echo-bot.cs` .
2. Leta upp följande två rader:
    ```csharp
    var replyText = $"Echo: {turnContext.Activity.Text}";
    await turnContext.SendActivityAsync(MessageFactory.Text(replyText, replyText), cancellationToken);
    ```
3. Ersätt dem med:
    ```csharp
    var replyText = $"Echo: {turnContext.Activity.Text}";
    var replySpeak = @"<speak version='1.0' xmlns='https://www.w3.org/2001/10/synthesis' xml:lang='de-DE'>
                    <voice name='Microsoft Server Speech Text to Speech Voice (de-DE, Stefan, Apollo)'>" +
                    $"{replyText}" + "</voice></speak>";
    await turnContext.SendActivityAsync(MessageFactory.Text(replyText, replySpeak), cancellationToken);
    ```
4. Bygg din lösning i Visual Studio och åtgärda eventuella build-fel.

Det andra argumentet i metoden ' MessageFactory. text ' anger [aktivitetens Speak-fält](https://github.com/Microsoft/botframework-sdk/blob/master/specs/botframework-activity/botframework-activity.md#speak) i bot-svaret. Med ovanstående ändring har den ersatts av enkel text till SSML för att ange en tysk röst som inte är standard.

### <a name="redeploy-your-bot"></a>Distribuera om din robot

Nu när du har gjort den nödvändiga ändringen i roboten är nästa steg att publicera om den till din Azure App Service och testa den:

1. I fönstret Solution Explorer högerklickar du på projektet **EchoBot** och väljer **publicera**.
2. Din tidigare distributions konfiguration har redan lästs in som standard. Klicka bara på **publicera** bredvid **EchoBot20190805125647 – webb distribution**.
3. Meddelandet **publicera slutförd** visas i Visual Studio-utdatafönstret och en webb sida öppnas med meddelandet "din robot är klar!".
4. Öppna klient programmet för Windows Voice Assistant, klicka på knappen Inställningar (övre högra kugg hjuls ikonen) och kontrol lera att du fortfarande har `de-de` i fältet språk.
5. Följ anvisningarna i [köra Windows Voice Assistant-klienten](#run-the-windows-voice-assistant-client) för att återansluta med din nyligen distribuerade robot, tala på det nya språket och lyssna på ditt robot svar på det språket med den nya rösten.

## <a name="clean-up-resources"></a>Rensa resurser

Om du inte kommer att fortsätta använda eko-roboten som distribuerats i den här självstudien kan du ta bort den och alla tillhör ande Azure-resurser genom att helt enkelt ta bort Azures resurs gruppen **SpeechEchoBotTutorial-ResourceGroup**.

1. Från [Azure Portal](https://portal.azure.com)klickar du på **resurs grupper** i navigeringen för **Azure-tjänster** .
2. Hitta resurs gruppen med namnet: **SpeechEchoBotTutorial-ResourceGroup**. Klicka på de tre punkterna (...).
3. Välj **Ta bort resursgrupp**.

## <a name="next-steps"></a>Nästa steg

> [!div class="nextstepaction"]
> [Bygg din egen klient app med talet SDK](quickstart-voice-assistant-csharp-uwp.md)

## <a name="see-also"></a>Se även

* Distribuera till en [Azure-region nära dig](https://azure.microsoft.com/global-infrastructure/locations/) för att se förbättringar av robotens svars tid
* Distribuera till en [Azure-region som stöder NEURALA TTS-röster med hög kvalitet](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#standard-and-neural-voices)
* Priser som är kopplade till kanal för direkt linje tal:
  * [Priser för bot service](https://azure.microsoft.com/pricing/details/bot-service/)
  * [Tal tjänst](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/)
* Skapa och distribuera din egen röst aktive rad bot:
  * Bygg en bot [-Framework-robot](https://dev.botframework.com/). Registrera dig med [direkt kanal](https://docs.microsoft.com/azure/bot-service/bot-service-channel-connect-directlinespeech?view=azure-bot-service-4.0) och [Anpassa din robot för röst](https://docs.microsoft.com/azure/bot-service/directline-speech-bot?view=azure-bot-service-4.0)
  * Utforska befintliga [bot-Framework-lösningar](https://microsoft.github.io/botframework-solutions/index): bygga en [virtuell assistent](https://microsoft.github.io/botframework-solutions/overview/virtual-assistant-solution/) och [utöka den till direkt linje tal](https://microsoft.github.io/botframework-solutions/clients-and-channels/tutorials/enable-speech/1-intro/)
