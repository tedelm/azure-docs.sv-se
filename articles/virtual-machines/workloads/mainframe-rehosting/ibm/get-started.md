---
title: IBM-arbetsbelastningar på Azure | Microsoft Docs
description: Använd en stordator-emulator och andra tjänster från Microsofts partner för att vara värd för dina IBM z/OS-arbetsbelastningar med hjälp av Microsoft Azure.
services: virtual-machines-linux
ms.service: virtual-machines-linux
documentationcenter: ''
author: njray
ms.author: edprice
manager: edprice
editor: edprice
ms.topic: conceptual
ms.date: 02/22/2019
tags: ''
keywords: ''
ms.openlocfilehash: 13c83c53cdad719d6a4bed4cc1852b85d62082e8
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/28/2020
ms.locfileid: "68834581"
---
# <a name="ibm-workloads-on-azure"></a>IBM-arbetsbelastningar på Azure

Många arbets belastningar i IBM-stordatorer som baseras på z/OS kan replikeras i Azure utan att några funktioner går förlorade och utan användare till och med märker ändringar i sina underliggande system. Att vara värd för program på Azure ger dig de stordatorer som du behöver samt elastiskhet, tillgänglighet och potentiella besparingar i molnet.

Azure har stöd för integrering med befintliga IBM-stordatorer, så att du kan migrera Applicates som gör det bäst att köra hybrid lösningar vid behov och migrera över tid. Även om du kan skriva om befintliga stordatorbaserade program för Azure, är det vanligare att vara värd för dem. När du skriver om läggs kostnaderna, komplexiteten och tiden till för migreringen. Med omvärdering kan du:

- Flytta program till en molnbaserad emulator.

- Migrera databasen till en molnbaserad databas.

- Ersätt moduler och kod med kod omvandlings motorer.

Dessutom är IBM-programvara, inklusive WebSphere och MQ, nu på Azure Marketplace. Med en licens för IBM-programvara kan du snabbt starta en virtuell dator genom att dra nytta av infrastruktur skalning på begäran som tillhandahålls av Azure.

Det finns ett omfattande partner eko system som hjälper dig att migrera IBM-stordatorer till Azure. I de flesta fall är det möjligt att återanvända en aktiv metod när det är möjligt innan du påbörjar en stegvis distribution av omskrivning eller ersättning av program. Få mer vägledning och hjälp från partner på [Azures stordatorer-flyttnings Center](https://azure.microsoft.com/migration/mainframe/).

**Nästa steg**

- [Stordator-migrering: myths och fakta](https://docs.microsoft.com/azure/architecture/cloud-adoption/infrastructure/mainframe-migration/myths-and-facts)
- [Installera IBM zD&T dev/test-miljö på Azure](./install-ibm-z-environment.md)
- [Konfigurera en programutvecklare styrd distribution (ADCD) i IBM zD&T v1](./demo.md)
- [IBM DB2-pureScale på Azure](https://docs.microsoft.com/azure/virtual-machines/linux/ibm-db2-purescale-azure)
