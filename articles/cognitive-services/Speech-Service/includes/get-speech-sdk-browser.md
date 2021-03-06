---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 03/27/2020
ms.author: trbye
ms.openlocfilehash: 5545b10a4228448d49849e7cd52728febf14ce2d
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/29/2020
ms.locfileid: "81400008"
---
:::row:::
    :::column span="3":::
        Java Script Speech SDK är tillgängligt som ett NPM-paket, se <a href="https://www.npmjs.com/package/microsoft-cognitiveservices-speech-sdk" target="_blank">Microsoft-cognitiveservices-Speech- <span class="docon docon-navigate-external x-hidden-focus"></span> SDK</a> och det är medföljande GitHub databas <a href="https://github.com/Microsoft/cognitive-services-speech-sdk-js" target="_blank">kognitiv-Services-Speech-SDK- <span class="docon docon-navigate-external x-hidden-focus"> </span>JS </a>.
    :::column-end:::
    :::column:::
        <br>
        <div class="icon is-large">
            <img alt="JavaScript" src="https://docs.microsoft.com/media/logos/logo_js.svg"  width="60px">
        </div>
    :::column-end:::
:::row-end:::

> [!TIP]
> Även om Java scripts Speech SDK är tillgängligt som ett NPM-paket, så kan både klientens webbläsare och Node. js använda det – Tänk på de olika arkitektoniska konsekvenserna för varje miljö. <a href="https://en.wikipedia.org/wiki/Document_Object_Model" target="_blank">Dokument objekts modellen (dom <span class="docon docon-navigate-external x-hidden-focus"></span> )</a> är till exempel inte tillgänglig för program på Server sidan, precis som <a href="https://nodejs.org/api/fs.html" target="_blank">fil systemet <span class="docon docon-navigate-external x-hidden-focus"></span> </a> inte är tillgängligt för program på klient sidan.

### <a name="nodejs-package-manager-npm"></a>Node. js-paket hanteraren (NPM)

Installera Java Script Speech SDK genom att köra följande `npm install` kommando nedan.

```nodejs
npm install microsoft-cognitiveservices-speech-sdk
```

### <a name="html-script-tag"></a>HTML-skriptfil

Alternativt kan du direkt ta med en `<script>` tagg i HTML- `<head>` elementet, som förlitar sig på <a href="https://www.jsdelivr.com/package/npm/microsoft-cognitiveservices-speech-sdk" target="_blank"> **JSDelivr** NPM- <span class="docon docon-navigate-external x-hidden-focus"> </span>syndikering </a>.

```html
<script src="https://cdn.jsdelivr.net/npm/microsoft-cognitiveservices-speech-sdk@1.10.1/distrib/lib/microsoft.cognitiveservices.speech.sdk.min.js">
</script>
```

Mer information finns i <a href="https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/javascript/browser" target="_blank">snabb <span class="docon docon-navigate-external x-hidden-focus"> </span>starten för webbläsarens tal-SDK </a>.
