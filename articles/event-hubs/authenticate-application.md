---
title: Autentisera ett program för att få åtkomst till Azure Event Hubs-resurser
description: Den här artikeln innehåller information om hur du autentiserar ett program med Azure Active Directory för åtkomst till Azure Event Hubs-resurser
services: event-hubs
ms.service: event-hubs
documentationcenter: ''
author: spelluru
ms.topic: conceptual
ms.date: 02/12/2020
ms.author: spelluru
ms.openlocfilehash: 4cef49f138b96848b8e59cb5b2d0b185d4568aa9
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/28/2020
ms.locfileid: "80521000"
---
# <a name="authenticate-an-application-with-azure-active-directory-to-access-event-hubs-resources"></a>Autentisera ett program med Azure Active Directory för att få åtkomst till Event Hubs resurser
Microsoft Azure ger integrerad åtkomst kontroll hantering för resurser och program baserat på Azure Active Directory (Azure AD). En viktig fördel med att använda Azure AD med Azure Event Hubs är att du inte behöver lagra dina autentiseringsuppgifter i koden längre. I stället kan du begära en OAuth 2,0-åtkomsttoken från Microsoft Identity Platform. Resurs namnet för att begära en token `https://eventhubs.azure.net/` (för Kafka-klienter är resursen att begära en token `https://<namespace>.servicebus.windows.net`). Azure AD autentiserar säkerhets objekt (en användare, grupp eller tjänstens huvud namn) som kör programmet. Om autentiseringen lyckas returnerar Azure AD en åtkomsttoken till programmet och programmet kan sedan använda åtkomsttoken för att auktorisera begäran till Azure Event Hubs-resurser.

När en roll tilldelas till ett säkerhets objekt i Azure AD ger Azure åtkomst till dessa resurser för säkerhets objekt. Åtkomst kan begränsas till prenumerations nivån, resurs gruppen, Event Hubs namn området eller någon annan resurs under den. En Azure AD-säkerhet kan tilldela roller till en användare, en grupp, ett huvud namn för program tjänsten eller en [hanterad identitet för Azure-resurser](../active-directory/managed-identities-azure-resources/overview.md). 

> [!NOTE]
> En rolldefinition är en samling behörigheter. Rollbaserad åtkomst kontroll (RBAC) styr hur dessa behörigheter tillämpas via roll tilldelning. En rolltilldelning består av tre delar: säkerhetsobjekt, rolldefinition och omfång. Mer information finns i [förstå de olika rollerna](../role-based-access-control/overview.md).

## <a name="built-in-roles-for-azure-event-hubs"></a>Inbyggda roller för Azure Event Hubs
Azure tillhandahåller följande inbyggda RBAC-roller för att auktorisera åtkomst till Event Hubs data med hjälp av Azure AD och OAuth:

- [Azure Event Hubs data ägare](../role-based-access-control/built-in-roles.md#azure-event-hubs-data-owner): Använd den här rollen för att ge fullständig åtkomst till Event Hubs resurser.
- [Azure Event Hubs data avsändare](../role-based-access-control/built-in-roles.md#azure-event-hubs-data-sender): Använd den här rollen för att ge åtkomst till Event Hubs resurser.
- [Azure Event Hubs data mottagare](../role-based-access-control/built-in-roles.md#azure-event-hubs-data-receiver): Använd den här rollen för att ge åtkomst till Event Hubs resurser.   

> [!IMPORTANT]
> Vår för hands version har stöd för att lägga till Event Hubs behörigheter för data åtkomst till ägaren eller deltagar rollen. Däremot går det inte längre att använda behörigheter för data åtkomst för rollen ägare och deltagare. Om du använder rollen ägare eller deltagare växlar du till att använda rollen Azure Event Hubs data Owner.

## <a name="assign-rbac-roles-using-the-azure-portal"></a>Tilldela RBAC-roller med hjälp av Azure Portal  
Mer information om hur du hanterar åtkomst till Azure-resurser med RBAC och Azure Portal finns i [den här artikeln](..//role-based-access-control/role-assignments-portal.md). 

När du har bestämt lämplig omfattning för en roll tilldelning navigerar du till den resursen i Azure Portal. Visa inställningarna för åtkomst kontroll (IAM) för resursen och följ de här anvisningarna för att hantera roll tilldelningar:

> [!NOTE]
> I stegen nedan tilldelas en roll till händelsehubben under Event Hubs namnrum, men du kan följa samma steg för att tilldela en roll som är begränsad till en Event Hubs resurs.

1. I [Azure Portal](https://portal.azure.com/)navigerar du till Event Hubs namn området.
2. På sidan **Översikt** väljer du den händelsehubben som du vill tilldela en roll.

    ![Välj händelsehubben](./media/authenticate-application/select-event-hub.png)
1. Välj **Access Control (IAM)** om du vill visa inställningar för åtkomst kontroll för händelsehubben. 
1. Välj fliken **roll tilldelningar** om du vill se en lista över roll tilldelningar. Välj knappen **Lägg till** i verktygsfältet och välj sedan **Lägg till roll tilldelning**. 

    ![Knappen Lägg till i verktygsfältet](./media/authenticate-application/role-assignments-add-button.png)
1. Utför följande steg på sidan **Lägg till roll tilldelning** :
    1. Välj den **Event Hubs roll** som du vill tilldela. 
    1. Sök efter **säkerhets objekt** (användare, grupp, tjänstens huvud namn) som du vill tilldela rollen.
    1. Spara roll tilldelningen genom att välja **Spara** . 

        ![Tilldela en roll till en användare](./media/authenticate-application/assign-role-to-user.png)
    4. Identiteten som du har tilldelat rollen visas i listan under den rollen. Följande bild visar till exempel att Azure-användare är i rollen Azure Event Hubs data Owner. 
        
        ![Användare i listan](./media/authenticate-application/user-in-list.png)

Du kan följa liknande steg för att tilldela en roll som är begränsad till Event Hubs namnrymd, resurs grupp eller prenumeration. När du definierar rollen och dess omfattning kan du testa det här beteendet med exempel [på den här GitHub-platsen](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/Rbac).


## <a name="authenticate-from-an-application"></a>Autentisera från ett program
En viktig fördel med att använda Azure AD med Event Hubs är att dina autentiseringsuppgifter inte längre behöver lagras i din kod. I stället kan du begära en OAuth 2,0-åtkomsttoken från Microsoft Identity Platform. Azure AD autentiserar säkerhets objekt (en användare, en grupp eller tjänstens huvud namn) som kör programmet. Om autentiseringen lyckas returnerar Azure AD åtkomsttoken till programmet och programmet kan sedan använda åtkomsttoken för att auktorisera begär anden till Azure Event Hubs.

I följande avsnitt beskrivs hur du konfigurerar ditt egna program eller webb program för autentisering med Microsoft Identity Platform 2,0. Mer information om Microsoft Identity Platform 2,0 finns i [Översikt över Microsoft Identity Platform (v 2.0)](../active-directory/develop/v2-overview.md).

En översikt över OAuth 2,0 Code Granting Flow finns i bevilja [åtkomst till Azure Active Directory webb program med hjälp av OAuth-kod för kod beviljande i 2,0 OAuth](../active-directory/develop/v2-oauth2-auth-code-flow.md).

### <a name="register-your-application-with-an-azure-ad-tenant"></a>Registrera ditt program med en Azure AD-klient
Det första steget i att använda Azure AD för att auktorisera Event Hubs-resurser är att registrera klient programmet med en Azure AD-klient från [Azure Portal](https://portal.azure.com/). När du registrerar klient programmet anger du information om programmet till AD. Azure AD tillhandahåller sedan ett klient-ID (kallas även ett program-ID) som du kan använda för att associera ditt program med Azure AD Runtime. Mer information om klient-ID finns i [program-och tjänst huvud objekt i Azure Active Directory](../active-directory/develop/app-objects-and-service-principals.md). 

Följande bilder visar steg för att registrera ett webb program:

![Registrera ett program](./media/authenticate-application/app-registrations-register.png)

> [!Note]
> Om du registrerar ditt program som ett internt program kan du ange en giltig URI för omdirigerings-URI: n. För interna program behöver det här värdet inte vara en riktig URL. För webb program måste omdirigerings-URI: n vara en giltig URI, eftersom den anger den URL till vilken tokens anges.

När du har registrerat ditt program visas **program-ID: t (klient)** under **Inställningar**:

![Program-ID för det registrerade programmet](./media/authenticate-application/application-id.png)

Mer information om hur du registrerar ett program med Azure AD finns i [integrera program med Azure Active Directory](../active-directory/develop/quickstart-v2-register-an-app.md).


### <a name="create-a-client-secret"></a>Skapa en klient hemlighet   
Programmet behöver en klient hemlighet för att bevisa sin identitet när en token begärs. Följ dessa steg om du vill lägga till klient hemligheten.

1. Navigera till din app-registrering i Azure Portal.
1. Välj inställningen för **certifikat & hemligheter** .
1. Under **klient hemligheter**väljer du **ny klient hemlighet** för att skapa en ny hemlighet.
1. Ange en beskrivning av hemligheten och välj önskat utgångs intervall.
1. Kopiera omedelbart värdet för den nya hemligheten till en säker plats. Fill-värdet visas bara en gång.

    ![Klienthemlighet](./media/authenticate-application/client-secret.png)


### <a name="client-libraries-for-token-acquisition"></a>Klient bibliotek för hämtning av token  
När du har registrerat ditt program och beviljat IT-behörigheter för att skicka/ta emot data i Azure Event Hubs kan du lägga till kod i programmet för att autentisera ett säkerhets objekt och hämta OAuth 2,0-token. Om du vill autentisera och hämta token kan du använda antingen en av [Microsofts identitets plattforms bibliotek för autentisering](../active-directory/develop/reference-v2-libraries.md) eller ett annat bibliotek med öppen källkod som stöder OpenID eller Connect 1,0. Ditt program kan sedan använda åtkomsttoken för att auktorisera en begäran mot Azure Event Hubs.

En lista över scenarier där det finns stöd för att hämta token finns i avsnittet [scenarier](https://aka.ms/msal-net-scenarios) i [Microsoft Authentication Library (MSAL) för .net](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) GitHub-lagringsplatsen.

## <a name="samples"></a>Exempel
- [Microsoft. Azure. EventHubs-exempel](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/Rbac). 
    
    I de här exemplen används det gamla biblioteket **Microsoft. Azure. EventHubs** , men du kan enkelt uppdatera det så att det använder det senaste **Azure. Messaging. EventHubs** -biblioteket. Om du vill flytta exemplet från att använda det gamla biblioteket till ett nytt, se [guiden för att migrera från Microsoft. Azure. EventHubs till Azure. Messaging. EventHubs](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/eventhub/Azure.Messaging.EventHubs/MigrationGuide.md).
- [Exempel för Azure. Messaging. EventHubs](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Azure.Messaging.EventHubs/ManagedIdentityWebApp)

    Det här exemplet har uppdaterats för att använda det senaste **Azure. Messaging. EventHubs** -biblioteket.

## <a name="next-steps"></a>Nästa steg
- Mer information om RBAC finns i [Vad är rollbaserad åtkomst kontroll (RBAC)](../role-based-access-control/overview.md)?
- Information om hur du tilldelar och hanterar RBAC-roll tilldelningar med Azure PowerShell, Azure CLI eller REST API finns i följande artiklar:
    - [Hantera rollbaserad åtkomst kontroll (RBAC) med Azure PowerShell](../role-based-access-control/role-assignments-powershell.md)  
    - [Hantera rollbaserad åtkomst kontroll (RBAC) med Azure CLI](../role-based-access-control/role-assignments-cli.md)
    - [Hantera rollbaserad åtkomst kontroll (RBAC) med REST API](../role-based-access-control/role-assignments-rest.md)
    - [Hantera rollbaserad åtkomst kontroll (RBAC) med Azure Resource Manager mallar](../role-based-access-control/role-assignments-template.md)

Se följande relaterade artiklar:
- [Autentisera en hanterad identitet med Azure Active Directory för att få åtkomst till Event Hubs resurser](authenticate-managed-identity.md)
- [Autentisera begär anden till Azure Event Hubs med signaturer för delad åtkomst](authenticate-shared-access-signature.md)
- [Ge åtkomst till Event Hubs resurser med Azure Active Directory](authorize-access-azure-active-directory.md)
- [Ge åtkomst till Event Hubs resurser med signaturer för delad åtkomst](authorize-access-shared-access-signature.md)

