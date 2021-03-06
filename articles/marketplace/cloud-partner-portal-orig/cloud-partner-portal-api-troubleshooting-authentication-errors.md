---
title: Felsöka vanliga autentiseringsfel | Azure Marketplace
description: 'Ger hjälp med vanliga autentiseringsfel när du använder Cloud Partner Portal API: er.'
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/08/2020
ms.author: dsindona
ms.openlocfilehash: d8fd1eb4bef987b4a8605e4be780512a914ec8b5
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/28/2020
ms.locfileid: "81255999"
---
# <a name="troubleshooting-common-authentication-errors"></a>Felsöka vanliga autentiseringsfel

> [!NOTE]
> Cloud Partner Portal API: er är integrerade med partner Center och fortsätter att fungera när dina erbjudanden har migrerats till Partner Center. I integrationen presenteras små ändringar. Granska ändringarna som anges i [Cloud Partner Portal API-referensen](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-api-overview) för att se till att koden fortsätter att fungera efter migreringen till Partner Center.

Den här artikeln ger hjälp med vanliga autentiseringsfel när du använder Cloud Partner Portal API: er.

## <a name="unauthorized-error"></a>Obehörigt fel

Om du får `401 unauthorized` fel meddelanden måste du kontrol lera att du har en giltig åtkomsttoken.  Om du inte redan har gjort det skapar du ett grundläggande Azure Active Directory (Azure AD)-program och ett tjänst huvud namn enligt beskrivningen i [använda portalen för att skapa ett Azure Active Directory program och tjänstens huvud namn som kan komma åt resurser](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal). Använd sedan programmet eller en enkel HTTP POST-begäran för att verifiera din åtkomst.  Du kommer att inkludera klient-ID, program-ID, objekt-ID och den hemliga nyckeln för att hämta åtkomsttoken som visas i följande bild:

![Felsöka 401-felet](./media/cloud-partner-portal-api-troubleshooting-authentication-errors/troubleshooting-401-error.jpg)


## <a name="forbidden-error"></a>Förbjudet fel

Om du får ett `403 forbidden` fel meddelande, se till att rätt tjänst huvud namn har lagts till i utgivar kontot i Cloud Partner Portal.
Följ stegen på sidan [förutsättningar](./cloud-partner-portal-api-prerequisites.md) för att lägga till tjänstens huvud namn i portalen.

Om rätt huvud namn för tjänsten har lagts till kontrollerar du all övrig information. Var noga med det objekt-ID som angavs i portalen. Det finns två objekt-ID på sidan Azure Active Directory app Registration och du måste använda det lokala objekt-ID: t. Du kan hitta rätt värde genom att gå till **Appregistreringar** sidan för appen och klicka på appens namn under **hanterat program i lokal katalog**. Då går du till appens lokala egenskaper, där du hittar rätt objekt-ID på sidan **Egenskaper** , som du ser i följande bild. Se också till att du använder rätt utgivare-ID när du lägger till tjänstens huvud namn och gör API-anropet.

![Felsöka 403-felet](./media/cloud-partner-portal-api-troubleshooting-authentication-errors/troubleshooting-403-error.jpg)
