---
title: Hjälp för Azure Marketplace SEO
description: Ger vägledning om hur du maximerar sökmotor optimering (SEO).
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/09/2019
ms.author: dsindona
ms.openlocfilehash: 761cdc2233bce3619d4c2c9ce1d7d7177d3bc407
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/28/2020
ms.locfileid: "80280158"
---
# <a name="azure-marketplace-seo-guidance"></a>Hjälp för Azure Marketplace SEO

Den här artikeln förklarar hur du maximerar ditt erbjudandes identifierings möjligheter via Sök funktionen i [Azure Marketplace](https://azuremarketplace.microsoft.com) och [AppSource](https://appsource.microsoft.com). 


## <a name="general-explanation-of-algorithm"></a>Allmän förklaring av algoritmen

Microsoft Marketplace använder Azure-Kognitiv sökning för att starta platsens Sök funktioner. Algoritmen baseras på term frekvens – invertering av dokument frekvens ([TF-IDF](https://en.wikipedia.org/wiki/Tf–idf)). Standard- [Lucene Analyzer](https://lucene.apache.org/core/) används.

I allmänhet är alla textfält, kategorier och branscher och ingår i vikten av relevansen. Särskilda termer som används sällan i appar, men ofta i appen kommer att generera en högre matchnings poäng med Sök. Detta inkluderar även termer som "VM" skulle erbjuda lite mer specialiserad "Azure Search".
Nedan visas de mest relevanta fälten som du bör tänka på.

 
|  Field                   | Betydelse | Riktlinjer                                                                                            |
|  --------------------    | ----------                   | ---------------                                                                   |
| Erbjudandets namn               |  Hög      | Exakt eller nära en fullständig matchning med Sök frågan ger hög rangordning.                       |
| Utgivarens namn           |  Hög      | Exakt eller nära en fullständig matchning med Sök frågan ger hög rangordning.                       |
| Kort beskrivning        |  Medel    | Om det finns namn på appar och utgivar namn, garanterar det att det är mycket viktigt att du har en hög rangordning. I det här fallet är en kort beskrivning viktig. Håll texten koncis och punkt. Nyckelord och förväntade Sök villkor bör tas med för bästa resultat.  Till exempel "det här är den bästa detalj handels POS som skapats helt på Dynamics 365" är mindre effektiv än "Retail POS (försäljnings punkt) för Dynamics 365".  | 
| Lång beskrivning         |  Låg       | Beskrivningen är ett sätt att gå till mer djup. De mest effektiva beskrivningarna är av rimlig längd och nyckelord används.  En till-punkt-beskrivningar som använder nyckelord kommer att ha mer än lång, lång text. Se till att viktiga termer, till exempel "IoT", finns i Beskrivning.  |
| Produkt kategorier       | Medel     |  Produkt kategorier bestäms av en kombination av utgivar alternativ och Microsoft. Välj lämplig kategori så att användarna lätt kan hitta apparna i rätt kategori. |
|  |  |  |


## <a name="other-tips"></a>Andra tips

-   Sök förslag hämtar tung användar aktivitet. Den prioriterar matchningar mot App Name/Publisher. Kort beskrivning blir nyckel fältet för när Sök termen inte är en exakt matchning med utgivare/app-namn.
-   Dokument för nedladdning ingår inte i Sök viktning.
-   Dina appar faktiskt förvärv och användning påverkar också Sök ordningen. Till exempel två likvärdiga appar där en har mycket fler användare får en högre rangordning.
