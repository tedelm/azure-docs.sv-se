---
title: 'Snabb start: skapa en webbapp som startar den fördjupade läsaren med Node. js'
titleSuffix: Azure Cognitive Services
description: I den här snabb starten skapar du en webbapp från grunden och lägger till API-funktionen för avancerad läsare.
author: pasta
manager: nitinme
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: quickstart
ms.date: 01/14/2020
ms.author: pasta
ms.openlocfilehash: 749e75fed409632c613713a49154e4cd8dc265b3
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/28/2020
ms.locfileid: "75946329"
---
# <a name="quickstart-create-a-web-app-that-launches-the-immersive-reader-nodejs"></a>Snabb start: skapa en webbapp som startar den fördjupade läsaren (Node. js)

Den [fördjupade läsaren](https://www.onenote.com/learningtools) är ett särskilt utformat verktyg som implementerar beprövade tekniker för att förbättra läsningen av förståelse.

I den här snabb starten skapar du en webbapp från grunden och integrerar den fördjupade läsaren med hjälp av SDK för avancerad läsare. Ett fullständigt fungerande exempel på den här snabb starten finns [här](https://github.com/microsoft/immersive-reader-sdk/tree/master/js/samples/quickstart-nodejs).

Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) konto innan du börjar.

## <a name="prerequisites"></a>Krav

* En fördjupad läsar resurs som kon figurer ATS för Azure Active Directory autentisering. Följ [dessa instruktioner](./how-to-create-immersive-reader.md) för att konfigurera. Du behöver några av de värden som skapas här när du konfigurerar miljö egenskaperna. Spara utdata från sessionen i en textfil för framtida bruk.
* [Node. js](https://nodejs.org/) och [garn](https://yarnpkg.com)
* En IDE, till exempel [Visual Studio Code](https://code.visualstudio.com/)

## <a name="create-a-nodejs-web-app-with-express"></a>Skapa en Node. js-webbapp med Express

Skapa en Node. js-webbapp med `express-generator` verktyget.

```bash
npm install express-generator -g
express --view=pug quickstart-nodejs
cd quickstart-nodejs
```

Installera garn beroenden och Lägg till `request` beroenden och `dotenv`, som kommer att användas senare i snabb starten.

```bash
yarn
yarn add request
yarn add dotenv
```

## <a name="set-up-authentication"></a>Konfigurera autentisering

### <a name="configure-authentication-values"></a>Konfigurera värden för autentisering

Skapa en ny fil med namnet _. kuvert_ i projekt roten. Klistra in följande kod i den, ange de värden som anges när du skapade din fördjupade läsare-resurs.
Lägg inte till citat tecken eller tecknen "{" och "}".

```text
TENANT_ID={YOUR_TENANT_ID}
CLIENT_ID={YOUR_CLIENT_ID}
CLIENT_SECRET={YOUR_CLIENT_SECRET}
SUBDOMAIN={YOUR_SUBDOMAIN}
```

Se till att du inte utför den här filen i käll kontrollen eftersom den innehåller hemligheter som inte bör göras offentliga.

Öppna sedan _app. js_ och Lägg till följande överst i filen. Detta läser in egenskaperna som definierats i. kuvert-filen som miljövariabler i noden.

```javascript
require('dotenv').config();
```

### <a name="update-the-router-to-acquire-the-token"></a>Uppdatera routern för att hämta token
Öppna filen _routes\index.js_ och ersätt den automatiskt genererade koden med följande kod.

Den här koden skapar en API-slutpunkt som hämtar en Azure AD-autentiseringstoken med ditt huvud namn för tjänsten. Den hämtar också under domänen. Den returnerar sedan ett objekt som innehåller token och under domänen.

```javascript
var express = require('express');
var router = express.Router();
var request = require('request');

/* GET home page. */
router.get('/', function(req, res, next) {
  res.render('index', { title: 'Express' });
});

router.get('/GetTokenAndSubdomain', function(req, res) {
    try {
        request.post({
            headers: {
                'content-type': 'application/x-www-form-urlencoded'
            },
            url: `https://login.windows.net/${process.env.TENANT_ID}/oauth2/token`,
            form: {
                grant_type: 'client_credentials',
                client_id: process.env.CLIENT_ID,
                client_secret: process.env.CLIENT_SECRET,
                resource: 'https://cognitiveservices.azure.com/'
            }
        },
        function(err, resp, tokenResult) {
            if (err) {
                console.log(err);
                return res.status(500).send('CogSvcs IssueToken error');
            }

            var tokenResultParsed = JSON.parse(tokenResult);

            if (tokenResultParsed.error) {
                console.log(tokenResult);
                return res.send({error :  "Unable to acquire Azure AD token. Check the debugger for more information."})
            }

            var token = tokenResultParsed.access_token;
            var subdomain = process.env.SUBDOMAIN;
            return res.send({token, subdomain});
        });
    } catch (err) {
        console.log(err);
        return res.status(500).send('CogSvcs IssueToken error');
    }
});

module.exports = router;
```

**GetTokenAndSubdomain** API-slutpunkten bör skyddas bakom någon form av autentisering (till exempel [OAuth](https://oauth.net/2/)) för att hindra obehöriga användare från att hämta token som ska användas mot tjänsten för avancerad läsare och fakturering. Detta arbete ligger utanför omfånget för den här snabb starten.

## <a name="add-sample-content"></a>Lägg till exempel innehåll

Nu ska vi lägga till exempel innehåll till den här webbappen. Öppna _views\index.pug_ och ersätt den automatiskt genererade koden med det här exemplet:

```pug
doctype html
html
   head
      title Immersive Reader Quickstart Node.js

      link(rel='stylesheet', href='https://stackpath.bootstrapcdn.com/bootstrap/3.4.1/css/bootstrap.min.css')

      // A polyfill for Promise is needed for IE11 support.
      script(src='https://cdn.jsdelivr.net/npm/promise-polyfill@8/dist/polyfill.min.js')

      script(src='https://contentstorage.onenote.office.net/onenoteltir/immersivereadersdk/immersive-reader-sdk.1.0.0.js')
      script(src='https://code.jquery.com/jquery-3.3.1.min.js')

      style(type="text/css").
        .immersive-reader-button {
          background-color: white;
          margin-top: 5px;
          border: 1px solid black;
          float: right;
        }
   body
      div(class="container")
        button(class="immersive-reader-button" data-button-style="iconAndText" data-locale="en")

        h1(id="ir-title") About Immersive Reader
        div(id="ir-content" lang="en-us")
          p Immersive Reader is a tool that implements proven techniques to improve reading comprehension for emerging readers, language learners, and people with learning differences. The Immersive Reader is designed to make reading more accessible for everyone. The Immersive Reader

            ul
                li Shows content in a minimal reading view
                li Displays pictures of commonly used words
                li Highlights nouns, verbs, adjectives, and adverbs
                li Reads your content out loud to you
                li Translates your content into another language
                li Breaks down words into syllables

          h3 The Immersive Reader is available in many languages.

          p(lang="es-es") El Lector inmersivo está disponible en varios idiomas.
          p(lang="zh-cn") 沉浸式阅读器支持许多语言
          p(lang="de-de") Der plastische Reader ist in vielen Sprachen verfügbar.
          p(lang="ar-eg" dir="rtl" style="text-align:right") يتوفر \"القارئ الشامل\" في العديد من اللغات.

script(type="text/javascript").
  function getTokenAndSubdomainAsync() {
        return new Promise(function (resolve, reject) {
            $.ajax({
                url: "/GetTokenAndSubdomain",
                type: "GET",
                success: function (data) {
                    if (data.error) {
                        reject(data.error);
                    } else {
                        resolve(data);
                    }
                },
                error: function (err) {
                    reject(err);
                }
            });
        });
    }

    $(".immersive-reader-button").click(function () {
        handleLaunchImmersiveReader();
    });

    function handleLaunchImmersiveReader() {
        getTokenAndSubdomainAsync()
            .then(function (response) {
                const token = response["token"];
                const subdomain = response["subdomain"];
                // Learn more about chunk usage and supported MIME types https://docs.microsoft.com/azure/cognitive-services/immersive-reader/reference#chunk
                const data = {
                    title: $("#ir-title").text(),
                    chunks: [{
                        content: $("#ir-content").html(),
                        mimeType: "text/html"
                    }]
                };
                // Learn more about options https://docs.microsoft.com/azure/cognitive-services/immersive-reader/reference#options
                const options = {
                    "onExit": exitCallback,
                    "uiZIndex": 2000
                };
                ImmersiveReader.launchAsync(token, subdomain, data, options)
                    .catch(function (error) {
                        alert("Error in launching the Immersive Reader. Check the console.");
                        console.log(error);
                    });
            })
            .catch(function (error) {
                alert("Error in getting the Immersive Reader token and subdomain. Check the console.");
                console.log(error);
            });
    }

    function exitCallback() {
        console.log("This is the callback function. It is executed when the Immersive Reader closes.");
    }
```


Observera att all text har ett **lang** -attribut som beskriver språk i texten. Det här attributet hjälper den fördjupade läsaren att tillhandahålla relevanta språk-och grammatiska funktioner.

## <a name="build-and-run-the-app"></a>Skapa och kör appen

Vår webbapp är nu klar. Starta appen genom att köra:

```bash
npm start
```

Öppna webbläsaren och gå till _http://localhost:3000_. Du bör se följande:

![Exempelapp](./media/quickstart-nodejs/1-buildapp.png)

## <a name="launch-the-immersive-reader"></a>Starta den fördjupade läsaren

När du klickar på knappen "avancerad läsare" visas den fördjupade läsaren som lanserades med innehållet på sidan.

![Avancerad läsare](./media/quickstart-nodejs/2-viewimmersivereader.png)

## <a name="next-steps"></a>Nästa steg

* Utforska SDK: [n för avancerad läsare](https://github.com/microsoft/immersive-reader-sdk) och [Avancerad läsare SDK-referens](./reference.md)