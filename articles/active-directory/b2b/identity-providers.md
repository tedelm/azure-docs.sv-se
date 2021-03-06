---
title: Identitets leverantörer för externa identiteter – Azure AD
description: Azure Active Directory B2B-samarbete stöder Multi-Factor Authentication (MFA) för selektiv åtkomst till företagets program
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 05/19/2020
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: elisolMS
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2284d015b451872753dd0855cac42e6f1926545c
ms.sourcegitcommit: 958f086136f10903c44c92463845b9f3a6a5275f
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 05/20/2020
ms.locfileid: "83712170"
---
# <a name="identity-providers-for-external-identities"></a>Identitets leverantörer för externa identiteter

En *identitetsprovider* skapar, underhåller och hanterar identitetsinformation samt autentiseringstjänster till program. När du delar dina appar och resurser med externa användare är Azure AD standard identitets leverantören för delning. Det innebär att när du bjuder in externa användare som redan har en Azure AD eller Microsoft-konto, kan de automatiskt logga in utan ytterligare konfiguration på din sida.

Du kan dock göra det möjligt för användarna att logga in med olika identitets leverantörer.

- **Google**: Google Federation låter externa användare lösa in inbjudningar från dig genom att logga in på dina appar med egna Gmail-konton. Google Federation kan också användas i dina självbetjänings registrerings användar flöden.
   > [!NOTE]
   > Om ett användar flöde är associerat med en app i den aktuella självbetjänings förhands granskningen och du skickar en användare en inbjudan till appen, kommer användaren inte att kunna använda ett Gmail-konto för att lösa in inbjudan. Som en lösning kan användaren gå igenom registrerings processen för självbetjäning. Eller så kan de lösa in inbjudan genom att komma åt en annan app eller genom att använda deras mina apps-Portal på https://myapps.microsoft.com .

- **Facebook**: när du skapar en app kan du konfigurera registrering av självbetjäning och aktivera Facebook-Federation så att användarna kan registrera sig för din app med sina egna Facebook-konton. Facebook kan bara användas för självbetjänings registrering av användar flöden och är inte tillgängligt som ett inloggnings alternativ när användarna löser in inbjudningar från dig.

- **Direkt Federation**: du kan också konfigurera direkt Federation med en extern identitetsprovider som stöder SAML-eller WS-utfodras protokoll. Med direkt Federation kan externa användare lösa in inbjudningar från dig genom att logga in på dina appar med sina befintliga sociala konton eller företags konton. 
   > [!NOTE]
   > Direkta Federations identitets leverantörer kan inte användas i dina självbetjänings registrerings användar flöden.


## <a name="how-it-works"></a>Så här fungerar det

Med hjälp av funktionen för självbetjänings registrering i Azure AD kan användarna registrera sig med sina Azure AD-, Google-eller Facebook-konton. Om du vill konfigurera sociala identitets leverantörer i din Azure AD-klient skapar du ett program hos varje identitetsprovider och konfigurerar autentiseringsuppgifter. Du får ett klient-eller app-ID och en klient-eller app-hemlighet, som du sedan kan lägga till i din Azure AD-klient.

När du har lagt till en identitetsprovider till din Azure AD-klient:

- När du bjuder in en extern användare till appar eller resurser i organisationen kan den externa användaren logga in med sitt eget konto hos den identitets leverantören.
- När du aktiverar [självbetjänings registrering](self-service-sign-up-overview.md) för dina appar kan externa användare registrera sig för dina appar med hjälp av sina egna konton med de identitets leverantörer som du har lagt till.

> [!NOTE]
> Azure AD är aktiverat som standard för registrering via självbetjäning så att användarna alltid har möjlighet att registrera sig med ett Azure AD-konto.

När du löser in din inbjudan eller registrerar dig för din app, har den externa användaren möjlighet att logga in och autentisera med den sociala identitets leverantören:

![Skärm bild som visar inloggnings skärmen med Google-och Facebook-alternativ](media/identity-providers/sign-in-with-social-identity.png)

För en optimal inloggnings upplevelse kan du federera med identitets leverantörer närhelst det är möjligt, så att du kan ge dina inbjudna gäster en sömlös inloggnings upplevelse när de kommer åt dina appar.  

## <a name="next-steps"></a>Nästa steg

Information om hur du lägger till identitets leverantörer för inloggning till dina program finns i följande artiklar:

- [Lägg till Google](google-federation.md) i din lista över sociala identitets leverantörer
- [Lägg till Facebook](facebook-federation.md) i listan över sociala identitets leverantörer
- [Konfigurera direkt Federation](direct-federation.md) med en organisation vars IDENTITETSPROVIDER stöder SAML 2,0 eller WS-utfodras protokoll. Observera att direkt Federation inte är ett alternativ för självbetjänings registrering av användar flöden.
