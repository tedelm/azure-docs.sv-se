---
title: Starta Avancerad läsare med HTML-innehåll
titleSuffix: Azure Cognitive Services
description: I den här artikeln visas hur du startar den fördjupade läsaren med HTML-innehåll.
author: metanMSFT
manager: guillasi
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: conceptual
ms.date: 01/14/2020
ms.author: metan
ms.openlocfilehash: bc7ab46113e1b819fc71a9f6e8a18400f8acfbef
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/29/2020
ms.locfileid: "75946247"
---
# <a name="how-to-launch-the-immersive-reader-with-html-content"></a>Så här startar du den fördjupade läsaren med HTML-innehåll

Den här artikeln visar hur du startar den fördjupade läsaren med HTML-innehåll.

## <a name="prepare-the-html-content"></a>Förbereda HTML-innehållet

Placera det innehåll som du vill återge i den fördjupade läsaren i ett behållar element. Se till att behållar elementet har ett `id`unikt. Den fördjupade läsaren tillhandahåller stöd för grundläggande HTML-element, se [referensen](./reference.md#html-support) för mer information.

```html
<div id='immersive-reader-content'>
    <b>Bold</b>
    <i>Italic</i>
    <u>Underline</u>
    <strike>Strikethrough</strike>
    <code>Code</code>
    <sup>Superscript</sup>
    <sub>Subscript</sub>
    <ul><li>Unordered lists</li></ul>
    <ol><li>Ordered lists</li></ol>
</div>
```

## <a name="get-the-html-content-in-javascript"></a>Hämta HTML-innehåll i Java Script

`id` Använd elementet container för att hämta HTML-innehållet i JavaScript-koden.

```javascript
const htmlContent = document.getElementById('immersive-reader-content').innerHTML;
```

## <a name="launch-the-immersive-reader-with-your-html-content"></a>Starta den fördjupade läsaren med ditt HTML-innehåll

När du `ImmersiveReader.launchAsync`anropar ställer du in `mimeType` segmentets `text/html` egenskap till för att aktivera åter givning av HTML.

```javascript
const data = {
    chunks: [{
        content: htmlContent,
        mimeType: 'text/html'
    }]
};

ImmersiveReader.launchAsync(YOUR_TOKEN, YOUR_SUBDOMAIN, data, YOUR_OPTIONS);
```

## <a name="next-steps"></a>Nästa steg

* Utforska [SDK-referensen för avancerad läsare](./reference.md)