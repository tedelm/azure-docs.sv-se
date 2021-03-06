---
title: Konfigurera grupphantering via självbetjäning – Azure Active Directory | Microsoft Docs
description: Skapa och hantera säkerhetsgrupper eller Office 365-grupper i Azure Active Directory och begära medlemskap i säkerhetsgrupper eller Office 365-grupper
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: conceptual
ms.date: 03/10/2020
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro;seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: 541deb5cf44ad5440e31641b673ed5da5b5d2b26
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/28/2020
ms.locfileid: "81768555"
---
# <a name="set-up-self-service-group-management-in-azure-active-directory"></a>Konfigurera hantering av självbetjänings grupper i Azure Active Directory 

Du kan göra det möjligt för användare att skapa och hantera egna säkerhets grupper eller Office 365-grupper i Azure Active Directory (Azure AD). Ägare av gruppen kan godkänna eller neka medlemskaps begär Anden och kan delegera kontrollen över grupp medlemskap. Funktioner för grupp hantering via självbetjäning är inte tillgängliga för e-postaktiverade säkerhets grupper eller distributions listor.

## <a name="self-service-group-membership-defaults"></a>Standardinställningar för grupp medlemskap för självbetjäning

När säkerhets grupper skapas i Azure Portal eller med hjälp av Azure AD PowerShell kan endast gruppens ägare uppdatera medlemskap. Säkerhets grupper som skapats av självbetjäning i [åtkomst panelen](https://account.activedirectory.windowsazure.com/r#/joinGroups) och alla Office 365-grupper är tillgängliga för att ansluta till alla användare, oavsett om de är ägares godkända eller automatiskt godkända. I åtkomst panelen kan du ändra medlemskaps alternativ när du skapar gruppen.

Grupper som skapats i | Standard beteende för säkerhets grupp | Standard beteende för Office 365-grupper
------------------ | ------------------------------- | ---------------------------------
[Azure AD PowerShell](groups-settings-cmdlets.md) | Endast ägare kan lägga till medlemmar<br>Synlig men inte tillgänglig för anslutning i åtkomst panelen | Öppna för att ansluta till alla användare
[Azure Portal](https://portal.azure.com) | Endast ägare kan lägga till medlemmar<br>Synlig men inte tillgänglig för anslutning i åtkomst panelen<br>Ägaren tilldelas inte automatiskt när gruppen skapas | Öppna för att ansluta till alla användare
[Åtkomst panel](https://account.activedirectory.windowsazure.com/r#/joinGroups) | Öppna för att ansluta till alla användare<br>Medlemskaps alternativ kan ändras när gruppen skapas | Öppna för att ansluta till alla användare<br>Medlemskaps alternativ kan ändras när gruppen skapas

## <a name="self-service-group-management-scenarios"></a>Självbetjänings grupp hanterings scenarier

* **Delegerad grupphantering** Ett exempel är en administratör som hanterar åtkomsten till ett SaaS-program som hennes företag använder. Eftersom det börjar bli svårt att hantera dessa åtkomstbehörigheter ber administratören företagsägaren att skapa en ny grupp. Administratören tilldelar åtkomst till programmet till den nya gruppen och lägger till gruppen alla personer som redan har åtkomst till programmet. Företagsägaren kan sedan lägga till fler användare, som automatiskt tilldelas programmet. Företagsägaren behöver inte vänta på administratören för att kunna hantera åtkomst för användarna. Om administratören ger samma behörighet till en chef i en annan affärs grupp, kan den personen även hantera åtkomsten för sina egna grupp medlemmar. Varken företags ägaren eller chefen kan visa eller hantera vart och ett av grupp medlemskapen. Administratören kan fortfarande se alla användare som har åtkomst till programmet och blockera behörigheten om det behövs.
* **Grupphantering via självbetjäning** Ett exempel på det här scenariot är två användare som båda har SharePoint Online-platser som de har konfigurerat oberoende av varandra. De vill ge var and ras team åtkomst till sina webbplatser. De kan åstadkomma detta genom att först skapa en grupp i Azure AD och därefter var och en välja samma grupp i SharePoint Online, vilket ger dem åtkomst till varandras webbplatser. När någon vill ha åtkomst begär de det från åtkomstpanelen och efter godkännande får de automatiskt åtkomst till båda SharePoint Online-platserna. Senare beslutar den ena av dem att alla användare som har åtkomst till webbplatsen även ska få åtkomst till ett visst SaaS-program. SaaS-programmets administratör kan lägga till åtkomstbehörighet för programmet till SharePoint Online-webbplatsen. När det är gjort kommer alla förfrågningar som han godkänner att ge åtkomst till de två SharePoint Online-webbplatserna och även till SaaS-programmet.

## <a name="make-a-group-available-for-user-self-service"></a>Göra en grupp tillgänglig för självbetjäning av användare

1. Logga in på [Azure AD administratörscenter](https://aad.portal.azure.com) med ett konto som är en global administratör för katalogen.
1. Välj **grupper**och välj sedan **allmänna** inställningar.
1. Ange **ägare kan hantera begär Anden om grupp medlemskap i åtkomst panelen** till **Ja**.
1. Ange **begränsa åtkomsten till grupper i åtkomst panelen** till **Nej**.
1. Om du ställer in **användare kan skapa säkerhets grupper i Azure portaler** eller **användare kan skapa Office 365-grupper i Azure portaler** för att

    - **Ja**: alla användare i din Azure AD-organisation får skapa nya säkerhets grupper och lägga till medlemmar i dessa grupper. Dessa nya grupper visas även på åtkomstpanelen för alla andra användare. Om princip inställningen i gruppen tillåter det kan andra användare skapa förfrågningar för att ansluta till dessa grupper
    - **Nej**: användarna kan inte skapa grupper och kan inte ändra befintliga grupper som de är ägare till. De kan dock fortfarande hantera medlemskap i dessa grupper och godkänna förfrågningar från andra användare om att ansluta till grupperna.

Du kan också använda **ägare som kan tilldela medlemmar som grupp ägare i Azure portaler** och **ägare som kan tilldela medlemmar som grupp ägare i Azure portaler** för att få mer detaljerad åtkomst kontroll över självbetjänings grupp hantering för dina användare.

När användare kan skapa grupper tillåts alla användare i din organisation att skapa nya grupper och kan sedan, som standard ägare, lägga till medlemmar i dessa grupper. Du kan inte ange personer som kan skapa sina egna grupper. Du kan bara ange individer för att göra en annan grupp medlem till en grupp ägare.

> [!NOTE]
> En Azure Active Directory Premium-eller P2-licens krävs för att användarna ska kunna ansluta till en säkerhets grupp eller Office 365-grupp och för att ägare ska kunna godkänna eller neka medlemskaps begär Anden. Utan en Azure Active Directory Premium licens kan användarna fortfarande hantera sina grupper i åtkomst panelen, men de kan inte skapa en grupp som kräver ägar godkännande i åtkomst panelen, och de kan inte begära att gå med i en grupp. 

## <a name="next-steps"></a>Nästa steg

Dessa artiklar innehåller ytterligare information om Azure Active Directory.

* [Hantera åtkomst till resurser med Azure Active Directory-grupper](../fundamentals/active-directory-manage-groups.md)
* [Azure Active Directory-cmdletar för att konfigurera gruppinställningar](groups-settings-cmdlets.md)
* [Programhantering i Azure Active Directory](../manage-apps/what-is-application-management.md)
* [Vad är Azure Active Directory?](../fundamentals/active-directory-whatis.md)
* [Integrera dina lokala identiteter med Azure Active Directory](../hybrid/whatis-hybrid-identity.md)
