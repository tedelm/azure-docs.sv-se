---
title: Nyheter Viktig information – Azure Active Directory | Microsoft Docs
description: Lär dig mer om vad som är nytt med Azure Active Directory, till exempel senaste versions information, kända problem, fel korrigeringar, inaktuella funktioner och kommande ändringar.
services: active-directory
author: msmimart
manager: daveba
featureFlags:
- clicktale
ms.assetid: 06a149f7-4aa1-4fb9-a8ec-ac2633b031fb
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 04/30/2020
ms.author: mimart
ms.reviewer: dhanyahk
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: f879ebd2f3628b8282342d730a5f3957cf2a615f
ms.sourcegitcommit: fc718cc1078594819e8ed640b6ee4bef39e91f7f
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 05/27/2020
ms.locfileid: "83994996"
---
# <a name="whats-new-in-azure-active-directory"></a>Vad är nytt i Azure Active Directory?

>Bli informerad om när du ska gå tillbaka till den här sidan för uppdateringar genom att kopiera och klistra in den här URL: en `https://docs.microsoft.com/api/search/rss?search=%22Release+notes+-+Azure+Active+Directory%22&locale=en-us` i din ![ RSS feed reader-ikon ](./media/whats-new/feed-icon-16x16.png) feed reader.

Azure AD tar emot förbättringar kontinuerligt. För att hålla dig uppdaterad med den senaste utvecklingen ger den här artikeln information om:

- De senaste versionerna
- Kända problem
- Felkorrigeringar
- Föråldrade funktioner
- Planer för ändringar

Den här sidan uppdateras varje månad, så du kan uppdatera den regelbundet. Om du letar efter objekt som är äldre än sex månader kan du hitta dem i [arkivet för nyheter i Azure Active Directory](whats-new-archive.md).

---

## <a name="april-2020"></a>April 2020

### <a name="combined-security-info-registration-experience-is-now-generally-available"></a>Den kombinerade registreringen av säkerhets information är nu allmänt tillgänglig

**Typ:** Ny funktion

**Tjänste kategori:** Autentiseringar (inloggningar)

**Produkt kapacitet:** & skydd för identitets säkerhet

Den kombinerade registrerings upplevelsen för Multi-Factor Authentication (MFA) och lösen ords återställning via självbetjäning (SSPR) är nu allmänt tillgänglig. Den här nya registreringen gör det möjligt för användare att registrera sig för MFA-och SSPR i en enda steg-för-steg-process. När du distribuerar den nya upplevelsen för din organisation kan användarna registrera sig på kortare tid och med färre krångel. Kolla in blogg inlägget [här](https://bit.ly/3etiRyQ).

---

### <a name="continuous-access-evaluation"></a>Utvärdering av kontinuerlig åtkomst

**Typ:** Ny funktion

**Tjänste kategori:** Autentiseringar (inloggningar)

**Produkt kapacitet:** & skydd för identitets säkerhet

Utvärdering av kontinuerlig åtkomst är en ny säkerhetsfunktion som gör det möjligt att tillämpa principer på förlitande parter i nära real tid när händelser inträffar i Azure AD (t. ex. borttagning av användar konto). Vi rullar ut den här funktionen först för team-och Outlook-klienter. Läs vår [blogg](https://techcommunity.microsoft.com/t5/azure-active-directory-identity/moving-towards-real-time-policy-and-security-enforcement/ba-p/1276933) och [dokumentation](https://docs.microsoft.com/azure/active-directory/fundamentals/concept-fundamentals-continuous-access-evaluation)om du vill ha mer information.

---

### <a name="sms-sign-in-firstline-workers-can-sign-in-to-azure-ad-backed-applications-with-their-phone-number-and-no-password"></a>SMS-inloggning: firstline-anställda kan logga in till Azure AD-program med sina telefonnummer och inget lösen ord

**Typ:** Ny funktion

**Tjänste kategori:** Autentiseringar (inloggningar)

**Produkt kapacitet:** Användarautentisering

Office lanserar en serie mobila appar som går till icke-traditionella organisationer och anställda i stora organisationer som inte använder e-post som primär kommunikations metod. De här apparna riktar Frontline anställda, Deskless-arbetskrafter, fält agenter eller detalj handels anställda som inte får en e-postadress från sin arbetsgivare, har åtkomst till en dator eller till den. Med det här projektet kan de anställda logga in på företags program genom att ange ett telefonnummer och roundtripping en kod. Mer information finns i vår dokumentation om [administratör](https://docs.microsoft.com/azure/active-directory/authentication/howto-authentication-sms-signin) [och slutanvändare](https://docs.microsoft.com/azure/active-directory/user-help/sms-sign-in-explainer).

---

### <a name="invite-internal-users-to-use-b2b-collaboration"></a>Bjud in interna användare att använda B2B-samarbete

**Typ:** Ny funktion

**Tjänste kategori:** Business

**Produkt kapacitet:**

Vi utökar B2B-Inbjudnings funktionen så att befintliga interna konton kan bjudas in att använda B2B-samarbets uppgifter som skickas framåt. Detta görs genom att skicka användarobjektet till inbjudans-API: t Förutom vanliga parametrar som den inbjudna e-postadressen. Användarens objekt-ID, UPN, grupp medlemskap, app-tilldelning etc. förblir intakt, men fortsätter att använda B2B för att autentisera med sina autentiseringsuppgifter för hem klienten i stället för de interna autentiseringsuppgifter som de använde före inbjudan. Mer information finns i [dokumentationen](https://docs.microsoft.com/azure/active-directory/b2b/invite-internal-users).

---

### <a name="report-only-mode-for-conditional-access-is-now-generally-available"></a>Endast rapport läge för villkorlig åtkomst är nu allmänt tillgänglig

**Typ:** Ny funktion

**Tjänste kategori:** Villkorlig åtkomst

**Produkt kapacitet:** & skydd för identitets säkerhet

Med [endast rapport läge för villkorlig åtkomst i Azure AD](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-conditional-access-report-only) kan du utvärdera resultatet av en princip utan att tvinga åtkomst kontroller. Du kan testa endast rapport principer i organisationen och förstå deras inverkan innan du aktiverar dem, vilket gör distributionen säkrare och enklare. Under de senaste månaderna har vi sett ett starkt införande av enbart rapport läge, med över 26M-användare som redan är i omfattning av en rapport princip. Med det här meddelandet skapas nya Azure AD-principer för villkorlig åtkomst i endast rapport läge som standard. Det innebär att du kan övervaka påverkan av dina principer från den tidpunkt då de skapas. Och för de som använder MS Graph API: er kan du även [Hantera rapport principer program mässigt](https://docs.microsoft.com/graph/api/resources/conditionalaccesspolicy?view=graph-rest-beta). 

---

### <a name="conditional-access-insights-and-reporting-workbook-is-generally-available"></a>Arbets boken för villkorlig åtkomst insikter och rapportering är allmänt tillgänglig

**Typ:** Ny funktion

**Tjänste kategori:** Villkorlig åtkomst

**Produkt kapacitet:** & skydd för identitets säkerhet

Arbets boken för villkorlig åtkomst [insikter och rapportering](https://docs.microsoft.com/azure/active-directory/conditional-access/howto-conditional-access-insights-reporting) ger administratörer en översikt över villkorlig åtkomst för Azure AD i klienten. Med möjligheten att välja en enskild princip kan administratörer bättre förstå vad varje princip gör och övervaka ändringar i real tid. Arbets boken strömma data som lagras i Azure Monitor, som du kan konfigurera på några minuter [efter dessa instruktioner](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics). För att instrument panelen ska bli mer synlig har vi flyttat den till fliken nya insikter och rapporter på menyn för villkorlig åtkomst i Azure AD.

---

### <a name="policy-details-blade-for-conditional-access-is-in-public-preview"></a>Bladet princip information för villkorlig åtkomst finns i offentlig för hands version

**Typ:** Ny funktion

**Tjänste kategori:** Villkorlig åtkomst

**Produkt kapacitet:** & skydd för identitets säkerhet

I [bladet ny princip information](https://docs.microsoft.com/azure/active-directory/conditional-access/troubleshoot-conditional-access) visas vilka tilldelningar, villkor och kontroller som var uppfyllda under utvärderingen av principen för villkorlig åtkomst. Du kan komma åt bladet genom att välja en rad i flikarna **villkorlig åtkomst** eller **endast rapporter** i inloggnings informationen.

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---april-2020"></a>Nya federerade appar är tillgängliga i Azure AD App Galleri – april 2020

**Typ:** Ny funktion

**Tjänste kategori:** Företags program

**Produkt kapacitet:** integration från tredje part

I april 2020 har vi lagt till dessa 31 nya appar med stöd för federation i app-galleriet: 

[SincroPool Apps](https://www.sincropool.com/), [SmartDB](https://hibiki.dreamarts.co.jp/smartdb/trial/), [float](https://docs.microsoft.com/azure/active-directory/saas-apps/float-tutorial), [LMS365](https://lms.365.systems/), [IWT anskaffning Suite](https://docs.microsoft.com/azure/active-directory/saas-apps/iwt-procurement-suite-tutorial), [Lunni](https://lunni.fi/), [EasySSO för JIRA](https://docs.microsoft.com/azure/active-directory/saas-apps/easysso-for-jira-tutorial), [Virtual Training Academy](https://vta.c3p.ca/app/en/openid?authenticate_with=microsoft), [Meraki Dashboard](https://docs.microsoft.com/azure/active-directory/saas-apps/meraki-dashboard-tutorial), [Office 365-arbetskrafter](https://app.mover.io/login), [talare engagera](https://speakerengage.com/login.php), [belys](https://docs.microsoft.com/azure/active-directory/saas-apps/honestly-tutorial), Ally, DutyFlow, AlertMedia, gr8, Pendo [, Highground](https://docs.microsoft.com/azure/active-directory/saas-apps/harmony-tutorial), Timetabling, SynchroNet, Fortes [empower](https://www.made-in-office.com/en/), Litmus, [DutyFlow](https://app.dutyflow.nl/) [GroupTalk](https://docs.microsoft.com/azure/active-directory/saas-apps/highground-tutorial), Frontify, MongoDB, TickitLMS, Coco [, Nitro,](https://docs.microsoft.com/azure/active-directory/saas-apps/fortes-change-cloud-tutorial)TMWS, [gr8 People](https://docs.microsoft.com/azure/active-directory/saas-apps/gr8-people-tutorial) [GroupTalk](https://recorder.grouptalk.com/), [AlertMedia](https://docs.microsoft.com/azure/active-directory/saas-apps/alertmedia-tutorial) [Timetabling Solutions](https://docs.microsoft.com/azure/active-directory/saas-apps/timetabling-solutions-tutorial) [Frontify](https://docs.microsoft.com/azure/active-directory/saas-apps/frontify-tutorial),, [TickitLMS Learn](https://docs.microsoft.com/azure/active-directory/saas-apps/tickitlms-learn-tutorial), [,](https://hexaware.com/partnerships-and-alliances/digital-transformation-using-microsoft-azure/) [,](https://docs.microsoft.com/azure/active-directory/saas-apps/litmus-tutorial) [Pendo](https://docs.microsoft.com/azure/active-directory/saas-apps/pendo-tutorial), [Trend Micro Web Security(TMWS)](https://review.docs.microsoft.com/azure/active-directory/saas-apps/trend-micro-tutorial) [,](https://docs.microsoft.com/azure/active-directory/saas-apps/mongodb-cloud-tutorial) [Ally](https://docs.microsoft.com/azure/active-directory/saas-apps/ally-tutorial) [SynchroNet CLICK](https://docs.microsoft.com/azure/active-directory/saas-apps/synchronet-click-tutorial) [Nitro Productivity Suite](https://docs.microsoft.com/azure/active-directory/saas-apps/nitro-productivity-suite-tutorial)

Mer information om apparna finns i [SaaS Application Integration with Azure Active Directory](https://aka.ms/appstutorial). Mer information om hur du visar ditt program i Azure AD App-galleriet finns i [lista ditt program i Azure Active Directory program galleriet](https://aka.ms/azureadapprequest).

---

### <a name="microsoft-graph-delta-query-support-for-oauth2permissiongrant-available-for-public-preview"></a>Stöd för Microsoft Graph delta frågor för oAuth2PermissionGrant tillgängligt för offentlig för hands version

**Typ:** Ny funktion

**Tjänste kategori:** MS Graph

**Produkt kapacitet:** Utvecklings miljö

Delta fråga för oAuth2PermissionGrant är tillgänglig för offentlig för hands version! Nu kan du spåra ändringar utan att behöva söka Microsoft Graph. [Läs mer.](https://docs.microsoft.com/graph/api/oAuth2PermissionGrant-delta?view=graph-rest-beta&tabs=http)

---

### <a name="microsoft-graph-delta-query-support-for-organizational-contact-generally-available"></a>Stöd för Microsoft Graph delta frågor för organisations kontakt är allmänt tillgänglig

**Typ:** Ny funktion

**Tjänste kategori:** MS Graph

**Produkt kapacitet:** Utvecklings miljö

Delta frågor för organisatoriska kontakter är allmänt tillgängliga! Nu kan du spåra ändringar i produktions program utan att behöva avsöka Microsoft Graph kontinuerligt. Ersätt eventuell befintlig kod som kontinuerligt avsöker orgContact data efter delta-fråga för att förbättra prestanda avsevärt. [Läs mer.](https://docs.microsoft.com/graph/api/orgcontact-delta?view=graph-rest-1.0&tabs=http)

---

### <a name="microsoft-graph-delta-query-support-for-application-generally-available"></a>Stöd för Microsoft Graph delta frågor för programmet är allmänt tillgängligt

**Typ:** Ny funktion

**Tjänste kategori:** MS Graph

**Produkt kapacitet:** Utvecklings miljö

Delta fråga för program är allmänt tillgänglig! Nu kan du spåra ändringar i produktions program utan att behöva avsöka Microsoft Graph kontinuerligt. Ersätt eventuell befintlig kod som kontinuerligt avsöker program data efter delta-fråga för att förbättra prestanda avsevärt. [Läs mer.](https://docs.microsoft.com/graph/api/application-delta?view=graph-rest-1.0)

---

### <a name="microsoft-graph-delta-query-support-for-administrative-units-available-for-public-preview"></a>Stöd för Microsoft Graph delta frågor för administrativa enheter som är tillgängliga för offentlig för hands version

**Typ:** Ny funktion

**Tjänste kategori:** MS Graph

**Produkt kapacitet:** Utvecklings upplevelse delta fråga för administrativa enheter är tillgänglig för offentlig för hands version! Nu kan du spåra ändringar utan att behöva söka Microsoft Graph. [Läs mer.](https://docs.microsoft.com/graph/api/administrativeunit-delta?view=graph-rest-beta&tabs=http)

---

### <a name="manage-authentication-phone-numbers-and-more-in-new-microsoft-graph-beta-apis"></a>Hantera telefonnummer för autentisering och mer i nya Microsoft Graph beta-API: er

**Typ:** Ny funktion

**Tjänste kategori:** MS Graph

**Produkt kapacitet:** Utvecklings miljö

Dessa API: er är ett nyckel verktyg för att hantera dina användares autentiseringsmetoder. Nu kan du programmatiskt för att registrera och hantera de autentiserare som används för MFA och lösen ords återställning via självbetjäning (SSPR). Detta har varit en av de mest efterfrågade funktionerna i Azure MFA, SSPR och Microsoft Graph Spaces. De nya API: er som vi har lanserat i den här vågen ger dig möjlighet att:

- Läsa, lägga till, uppdatera och ta bort en användares telefoner för autentisering
- Återställa en användares lösen ord
- Aktivera och inaktivera SMS-inloggning

Mer information finns i [Översikt över Azure AD Authentication Methods API](https://docs.microsoft.com/graph/api/resources/authenticationmethods-overview?view=graph-rest-beta).

---

### <a name="administrative-units-public-preview"></a>Den offentliga för hands versionen av administrativa enheter

**Typ:** Ny funktion

**Tjänste kategori:** RBAC

**Produkt kapacitet:** Access Control

Med administrativa enheter kan du bevilja administratörs behörigheter som är begränsade till en avdelning, region eller något annat segment i din organisation som du definierar. Du kan använda administrativa enheter för att delegera behörigheter till regionala administratörer eller ange en princip på en detaljerad nivå. En användar konto administratör kan till exempel uppdatera profil information, återställa lösen ord och tilldela licenser enbart för användare i sin administrativa enhet.

Med hjälp av administrativa enheter kan en central administratör:

- Skapa en administrativ enhet för decentraliserad hantering av resurser
- Tilldela en roll med administrativ behörighet enbart över Azure AD-användare i en administrativ enhet
- Fyll i de administrativa enheterna med användare och grupper efter behov

Mer information finns i [hantering av administrativa enheter i Azure Active Directory (för hands version)](https://aka.ms/AdminUnitsDocs).

---

### <a name="printer-administrator-and-printer-technician-built-in-roles"></a>Inbyggda roller för skrivar administratör och skrivar tekniker

**Typ:** Ny funktion

**Tjänste kategori:** RBAC

**Produkt kapacitet:** Access Control

**Skrivar administratör**: användare med den här rollen kan registrera skrivare och hantera alla aspekter av alla skrivar konfigurationer i Microsofts Universal Print-lösning, inklusive inställningar för Universal Print Connector. De kan godkänna alla delegerade utskrifts behörighets begär Anden. Skrivar administratörer har även åtkomst till utskrifts rapporter. 

**Skrivar tekniker**: användare med den här rollen kan registrera skrivare och hantera skrivar status i Microsoft Universal Print-lösningen. De kan också läsa all anslutnings information. Viktiga aktiviteter en skrivare tekniker kan inte ange användar behörigheter för skrivare och dela skrivare. [Läs mer.](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles#printer-administrator)

---

### <a name="hybrid-identity-admin-built-in-role"></a>Inbyggd rollen hybrid identitets administratör

**Typ:** Ny funktion

**Tjänste kategori:** RBAC

**Produkt kapacitet:** Access Control

Användare med den här rollen kan aktivera, konfigurera och hantera tjänster och inställningar för att aktivera hybrid identitet i Azure AD. Den här rollen ger möjlighet att konfigurera Azure AD till någon av de tre autentiseringsmetoder som stöds&#8212;(PHS), direktautentisering (PTA) eller Federation (AD FS eller tredjepartsleverantörer) &#8212;och för att distribuera relaterad lokal infrastruktur för att aktivera dem. Lokal infrastruktur innehåller etablerings-och PTA-agenter. Den här rollen ger möjlighet att aktivera sömlös enkel inloggning (S-SSO) för att möjliggöra sömlös autentisering på icke-Windows 10-enheter eller datorer som inte är Windows Server 2016. Dessutom ger den här rollen möjlighet att se inloggnings loggar och för att få åtkomst till hälso-och analys funktioner för övervakning och fel sökning. [Läs mer.](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles#hybrid-identity-administrator)

---

### <a name="network-administrator-built-in-role"></a>Inbyggd nätverks administratörs roll

**Typ:** Ny funktion

**Tjänste kategori:** RBAC

**Produkt kapacitet:** Access Control

Användare med den här rollen kan granska rekommendationer för nätverks perimeter arkitektur från Microsoft som baseras på telemetri från användarens platser. Nätverks prestanda för Office 365 förlitar sig på noggrann nätverks perimeter arkitektur i företags nätverk, vilket vanligt vis är specifika för användare. Med den här rollen kan du redigera identifierade användar platser och konfigurera nätverks parametrar för de platserna för att under lätta förbättrad telemetri och bättre design rekommendationer. [Läs mer.](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles#network-administrator)

---

### <a name="bulk-activity-and-downloads-in-the-azure-ad-admin-portal-experience"></a>Mass aktivitet och nedladdningar i Azure AD Admin Portal-upplevelsen

**Typ:** Ny funktion

**Tjänste kategori:** Användar hantering

**Produkt kapacitet:** Katalogen

Nu kan du utföra Mass aktiviteter för användare och grupper i Azure AD genom att ladda upp en CSV-fil i Azure AD Admin Portal-upplevelsen. Du kan skapa användare, ta bort användare och bjuda in gäst användare. Och du kan lägga till och ta bort medlemmar från en grupp.

Du kan också hämta listor över Azure AD-resurser från Azure AD Admin Portal-upplevelsen. Du kan hämta listan över användare i katalogen, listan över grupper i katalogen och medlemmarna i en viss grupp.

Om du vill ha mer information kan du läsa följande:

- [Skapa användare](https://docs.microsoft.com/azure/active-directory/users-groups-roles/users-bulk-add) eller [Bjud in gäst användare](https://docs.microsoft.com/azure/active-directory/b2b/tutorial-bulk-invite)
- [Ta bort användare](https://docs.microsoft.com/azure/active-directory/users-groups-roles/users-bulk-delete) eller [Återställ borttagna användare](https://docs.microsoft.com/azure/active-directory/users-groups-roles/users-bulk-restore)
- [Hämta lista över användare](https://docs.microsoft.com/azure/active-directory/users-groups-roles/users-bulk-download) eller [Hämta lista över grupper](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-bulk-download)
- [Lägg till (importera) medlemmar](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-bulk-import-members) eller [ta bort medlemmar](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-bulk-remove-members) eller [Hämta en lista över medlemmar](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-bulk-download-members) för en grupp

---

### <a name="my-staff-delegated-user-management"></a>Min personal har delegerat användar hantering

**Typ:** Ny funktion

**Tjänste kategori:** Användar hantering

**Produkt kapacitet:**

Min personal ger firstline-ansvariga, till exempel en butiks chef, för att säkerställa att deras personal medlemmar kan komma åt sina Azure AD-konton. I stället för att förlita dig på en central helpdesk kan organisationer delegera vanliga uppgifter, till exempel att återställa lösen ord eller ändra telefonnummer till en firstline Manager. Med min personal kan en användare som inte har åtkomst till sitt konto återfå åtkomst på bara några klick, utan supportavdelningen eller IT-personal som krävs. Mer information finns i [Hantera dina användare med min personal (för hands version)](https://aka.ms/MyStaffAdminDocs) och [delegera användar hantering med min personal (för hands version)](https://aka.ms/MyStaffUserDocs).

---

### <a name="an-upgraded-end-user-experience-in-access-reviews"></a>En uppgraderad slut användar upplevelse i åtkomst granskningar

**Typ:** Ändrad funktion

**Tjänste kategori:** Åtkomst granskningar

**Produkt kapacitet:** Identitets styrning

Vi har uppdaterat gransknings upplevelsen för åtkomst granskningar av Azure AD i portalen Mina appar. I slutet av april visas en banderoll som gör det möjligt för dina granskare som är inloggade i Azure AD Access granskare att se en banderoll som gör det möjligt för dem att testa den uppdaterade upplevelsen i min åtkomst. Observera att de uppdaterade åtkomst granskningarna har samma funktioner som den aktuella upplevelsen, men med ett förbättrat användar gränssnitt ovanpå nya funktioner som gör att användarna kan vara produktiva. [Du kan lära dig mer om den uppdaterade upplevelsen här](https://docs.microsoft.com/azure/active-directory/governance/perform-access-review). Den här allmänna för hands versionen kommer att gälla till slutet av juli 2020. I slutet av juli kommer granskare som inte har valt förhands gransknings upplevelsen automatiskt att dirigeras till min åtkomst för att utföra åtkomst granskningar. Om du vill att granskarna ska växlas över till för hands versionen i min åtkomst nu, gör du [en begäran här](https://forms.microsoft.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR5dv-S62099HtxdeKIcgO-NUOFJaRDFDWUpHRk8zQ1BWVU1MMTcyQ1FFUi4u).

---

### <a name="workday-inbound-user-provisioning-and-writeback-apps-now-support-the-latest-versions-of-workday-web-services-api"></a>Arbets dag för inkommande användar etablering och tillbakaskrivning av appar stöder nu de senaste versionerna av webb tjänstens API för arbets dagar

**Typ:** Ändrad funktion

**Tjänste kategori:** App-etablering

**Produkt kapacitet:** 

Baserat på kundfeedback har vi nu uppdaterat inetablerings-och tillbakaskrivning av workday-appar i Enterprise App-galleriet för att stödja de senaste versionerna av WWS-API: et för webb tjänster (Workday). Med den här ändringen kan kunderna ange den WWS-API-version som de vill använda i anslutnings strängen. Detta ger kunderna möjlighet att hämta fler HR-attribut som är tillgängliga i alla versioner av arbets dagen. Tillbakaskrivning-appen för Workday använder nu den rekommenderade Change_Work_Contact_Info Workday-webbtjänsten för att lösa Maintain_Contact_Infos begränsningar.

Om ingen version anges i anslutnings strängen fortsätter inkommande Provisioning-appar som standard att använda WWS v-21.1 för att växla till de senaste Workday-API: erna för inkommande användar etablering, kunder måste uppdatera anslutnings strängen som dokumenteras [i självstudien](https://docs.microsoft.com/azure/active-directory/saas-apps/workday-inbound-tutorial#which-workday-apis-does-the-solution-use-to-query-and-update-workday-worker-profiles) och även uppdatera de XPath-attribut som används för Workday-attribut enligt beskrivningen i [referens hand boken för Workday-attributet](https://docs.microsoft.com/azure/active-directory/app-provisioning/workday-attribute-reference#xpath-values-for-workday-web-services-wws-api-v30). 

Om du vill använda det nya API: t för tillbakaskrivning behöver du inte göra några ändringar i programetablerings appen för Workday-tillbakaskrivning. På sidan arbets dag ser du till att användar kontot för ISU-kontot (Workday) har behörighet att anropa Change_Work_Contact affärs process som dokumenteras i avsnittet självstudie, [Konfigurera säkerhets princip för affärs processer](https://docs.microsoft.com/azure/active-directory/saas-apps/workday-inbound-tutorial#configuring-business-process-security-policy-permissions). 

Vi har uppdaterat vår [själv studie guide](https://docs.microsoft.com/azure/active-directory/saas-apps/workday-inbound-tutorial) för att återspegla den nya stöd för API-versionen.

---

### <a name="users-with-default-access-role-are-now-in-scope-for-provisioning"></a>Användare med rollen standard åtkomst är nu inom omfånget för etablering

**Typ:** Ändrad funktion

**Tjänste kategori:** App-etablering

**Produkt kapacitet:** Hantering av identitets livs cykel

Tidigare har användare med standard åtkomst rollen utanför definitions området för etablering. Vi har hört feedback om att kunderna vill att användare med den här rollen ska omfattas av etableringen. Från och med 16 april 2020 låter alla nya etablerings konfigurations användare med standard åtkomst rollen tillhandahållas. Gradvis kommer vi att ändra beteendet för befintliga etablerings konfigurationer som stöder etablering av användare med den här rollen. [Läs mer.](https://docs.microsoft.com/azure/active-directory/app-provisioning/application-provisioning-config-problem-no-users-provisioned)

---

### <a name="updated-provisioning-ui"></a>Uppdaterat etablerings gränssnitt

**Typ:** Ändrad funktion

**Tjänste kategori:** App-etablering

**Produkt kapacitet:** Hantering av identitets livs cykel

Vi har uppdaterat vår etablerings upplevelse för att skapa en mer fokuserad hanterings vy. När du navigerar till etablerings bladet för ett företags program som redan har kon figurer ATS kan du enkelt övervaka förloppet för etablering och hantering av åtgärder som att starta, stoppa och starta om etableringen. [Läs mer.](https://docs.microsoft.com/azure/active-directory/app-provisioning/configure-automatic-user-provisioning-portal)

---

### <a name="dynamic-group-rule-validation-is-now-available-for-public-preview"></a>Verifiering av dynamisk grupp regel är nu tillgängligt för offentlig för hands version

**Typ:** Ändrad funktion

**Tjänste kategori:** Grupp hantering

**Produkt kapacitet:** Samarbete

Azure Active Directory (Azure AD) ger nu möjlighet att verifiera dynamiska grupp regler. På fliken **validera regler** kan du verifiera din dynamiska regel mot exempel grupp medlemmar för att bekräfta att regeln fungerar som förväntat. När du skapar eller uppdaterar dynamiska grupp regler vill administratörer veta om en användare eller enhet kommer att vara medlem i gruppen. Detta hjälper till att utvärdera om en användare eller enhet uppfyller regel villkoren och hjälper till med fel sökning när medlemskap inte förväntas.

Mer information finns i [Validera en regel för dynamisk grupp medlemskap (för hands version)](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-dynamic-rule-validation).

---

### <a name="identity-secure-score---security-defaults-and-mfa-improvement-action-updates"></a>Identifiera säkra Poäng – säkerhets inställningar och uppdatering av MFA-förbättringar

**Typ:** Ändrad funktion

**Tjänste kategori:** EJ TILLÄMPLIGT

**Produkt kapacitet:** & skydd för identitets säkerhet

**Stöd för säkerhets inställningar för Azure AD-förbättringar:** Microsofts säkra poäng kommer att uppdatera förbättrings åtgärder för att ge stöd för [säkerhets inställningar i Azure AD](https://docs.microsoft.com/azure/active-directory/fundamentals/concept-fundamentals-security-defaults), vilket gör det enklare att skydda din organisation med förkonfigurerade säkerhets inställningar för vanliga attacker. Detta kommer att påverka följande förbättrings åtgärder:

- Se till att alla användare kan slutföra Multi-Factor Authentication för säker åtkomst
- Kräv MFA för administrativa roller
- Aktivera princip för att blockera äldre autentisering
 
**Uppdatering av MFA-förbättringar:** För att avspegla behovet av företag för att säkerställa den högsta säkerheten vid användning av principer som fungerar med deras verksamhet har Microsoft Secure score tagit bort tre förbättringar av förbättringarna i mitten av Multi-Factor Authentication och lagt till två.

Borttagna förbättrings åtgärder:

- Registrera alla användare för Multi-Factor Authentication
- Kräv MFA för alla användare
- Kräv MFA för privilegierade Azure AD-roller

Ytterligare förbättrings åtgärder:

- Se till att alla användare kan slutföra Multi-Factor Authentication för säker åtkomst
- Kräv MFA för administrativa roller

Dessa nya förbättringar kräver att du registrerar dina användare eller administratörer för Multi-Factor Authentication (MFA) i din katalog och upprättar rätt uppsättning principer som passar organisationens behov. Huvud målet är att ha flexibilitet och samtidigt se till att alla användare och administratörer kan autentisera med flera faktorer eller riskfyllda identitets verifierings meddelanden. Det kan vara form av att ha flera principer som tillämpar definitions områden, eller ställa in säkerhets inställningar (från och med den 16 mars) som gör att Microsoft bestämmer sig för att utmana användare för MFA. [Läs mer om vad som är nytt i Microsofts säkra Poäng](https://docs.microsoft.com/microsoft-365/security/mtp/microsoft-secure-score?view=o365-worldwide#whats-new).

---

## <a name="march-2020"></a>Mars 2020

### <a name="unmanaged-azure-active-directory-accounts-in-b2b-update-for-march-2021"></a>Ohanterade Azure Active Directory konton i B2B-uppdatering för mars 2021

**Typ:** Planera för ändring  
**Tjänste kategori:** Business  
**Produkt kapacitet:** B2B/B2C
 
**Från och med den 31 mars 2021**kommer Microsoft inte längre att stödja inlösen av inbjudningar genom att skapa ohanterade Azure Active Directory-konton (Azure AD) och klienter för B2B-samarbets scenarier. Vi rekommenderar att du väljer att [e-posta autentisering med eng ång slö sen ord](https://docs.microsoft.com/azure/active-directory/b2b/one-time-passcode).

---

### <a name="users-with-the-default-access-role-will-be-in-scope-for-provisioning"></a>Användare med standard åtkomst rollen är inom omfånget för etablering

**Typ:** Planera för ändring  
**Tjänste kategori:** App-etablering  
**Produkt kapacitet:** Hantering av identitets livs cykel
 
Tidigare har användare med standard åtkomst rollen utanför definitions området för etablering. Vi har hört feedback om att kunderna vill att användare med den här rollen ska omfattas av etableringen. Vi arbetar med att distribuera en ändring så att alla nya etablerings konfigurationer tillåter användare med standard åtkomst rollen som ska tillhandahållas. Gradvis kommer vi att ändra beteendet för befintliga etablerings konfigurationer som stöder etablering av användare med den här rollen. Ingen kund åtgärd krävs. Vi publicerar en uppdatering av vår [dokumentation](https://docs.microsoft.com/azure/active-directory/app-provisioning/application-provisioning-config-problem-no-users-provisioned) när den här ändringen är på plats.

---

### <a name="azure-ad-b2b-collaboration-will-be-available-in-microsoft-azure-operated-by-21vianet-azure-china-21vianet-tenants"></a>Azure AD B2B-samarbete är tillgängligt i Microsoft Azure som drivs av 21Vianet-klienter (Azure Kina 21Vianet)

**Typ:** Planera för ändring  
**Tjänste kategori:** Business  
**Produkt kapacitet:** B2B/B2C
 
Funktionerna i Azure AD B2B-samarbete kommer att göras tillgängliga i Microsoft Azure som drivs av 21Vianet (Azure Kina 21Vianet), vilket gör det möjligt för användare i en Azure Kina 21Vianet-klient att samar beta sömlöst med användare i andra Azure Kina 21Vianet-klienter. [Lär dig mer om Azure AD B2B-samarbete](https://docs.microsoft.com/azure/active-directory/b2b/).

---
 
### <a name="azure-ad-b2b-collaboration-invitation-email-redesign"></a>Inbjudan till Azure AD B2B-samarbete e-postdesign

**Typ:** Planera för ändring  
**Tjänste kategori:** Business  
**Produkt kapacitet:** B2B/B2C
 
[E-postmeddelanden](https://docs.microsoft.com/azure/active-directory/b2b/invitation-email-elements) som skickas av Azure AD B2B Collaboration-Inbjudnings tjänsten för att bjuda in användare till katalogen, kommer att omutformas för att göra Inbjudnings informationen och användarens nästa steg tydligare.

---

### <a name="homerealmdiscovery-policy-changes-will-appear-in-the-audit-logs"></a>HomeRealmDiscovery princip ändringar visas i gransknings loggarna

**Typ:** Fastsatt  
**Tjänste kategori:** Händelse  
**Produkt kapacitet:** Övervaka & rapportering
 
Vi har åtgärdat ett fel där ändringar i [HomeRealmDiscovery-principen](https://docs.microsoft.com/azure/active-directory/manage-apps/configure-authentication-for-federated-users-portal) inte inkluderades i gransknings loggarna. Du kommer nu att kunna se när och hur principen ändrades, och av vem. 

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---march-2020"></a>Nya federerade appar som är tillgängliga i Azure AD App Galleri – mars 2020

**Typ:** Ny funktion  
**Tjänste kategori:** Företags program  
**Produkt kapacitet:** integration från tredje part
 
I mars 2020 har vi lagt till dessa 51 nya appar med stöd för federation i app-galleriet: 

[Cisco AnyConnect](https://docs.microsoft.com/azure/active-directory/saas-apps/cisco-anyconnect), [Zoho, en Kina](https://docs.microsoft.com/azure/active-directory/saas-apps/zoho-one-china-tutorial), [PlusPlus](https://test.plusplus.app/auth/login/azuread-outlook/), [profit.co SAML-App](https://docs.microsoft.com/azure/active-directory/saas-apps/profitco-saml-app-tutorial), [iPoint-tjänsteleverantör](https://docs.microsoft.com/azure/active-directory/saas-apps/ipoint-service-provider-tutorial), [Contexxt.AI-sfär](https://contexxt-sphere.com/login), [ingengörskunskap av Invictus](https://docs.microsoft.com/azure/active-directory/saas-apps/wisdom-by-invictus-tutorial), [överstrålning, digitalt signerat](https://spark-dev.pixelnebula.com/login), [Logz.io – molnets användarvänlighet för ingenjörer](https://docs.microsoft.com/azure/active-directory/saas-apps/logzio-cloud-observability-for-engineers-tutorial), [SPECTRUMU](https://docs.microsoft.com/azure/active-directory/saas-apps/spectrumu-tutorial), [BizzContact](https://bizzcontact.app/), [Elqano SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/elqano-sso-tutorial), [MarketSignShare](http://www.signshare.com/), CrossKnowledge [Learning Suite](https://docs.microsoft.com/azure/active-directory/saas-apps/crossknowledge-learning-suite-tutorial), [Netvision kompositioner](https://docs.microsoft.com/azure/active-directory/saas-apps/netvision-compas-tutorial), [FCM Hub](https://docs.microsoft.com/azure/active-directory/saas-apps/fcm-hub-tutorial), [revben A/S Byggeweb Mobile](https://apps.apple.com/us/app/docia/id529058757), [GoLinks](https://docs.microsoft.com/azure/active-directory/saas-apps/golinks-tutorial), [Datadog](https://docs.microsoft.com/azure/active-directory/saas-apps/datadog-tutorial), [Zscaler B2B User Portal](https://docs.microsoft.com/azure/active-directory/saas-apps/zscaler-b2b-user-portal-tutorial), [Aster](https://demo.asterapp.io/login) [hiss](https://docs.microsoft.com/azure/active-directory/saas-apps/lift-tutorial), [Planview Enterprise One](https://docs.microsoft.com/azure/active-directory/saas-apps/planview-enterprise-one-tutorial) [, WatchTeams och Aster](https://www.devfinition.com/), [kompetens arbets flöde](https://docs.microsoft.com/azure/active-directory/saas-apps/skills-workflow-tutorial) [,](https://admin.nodeinsight.com/AADLogin.aspx) [IP-plattform](https://docs.microsoft.com/azure/active-directory/saas-apps/ip-platform-tutorial), [insikt](https://docs.microsoft.com/azure/active-directory/saas-apps/invision-tutorial), [PipeDrive](https://docs.microsoft.com/azure/active-directory/saas-apps/pipedrive-tutorial), [Showcase-workshop](https://app.showcaseworkshop.com/), [GreenLight integration Platform](https://docs.microsoft.com/azure/active-directory/saas-apps/greenlight-integration-platform-tutorial), [GreenLight-kompatibel åtkomst hantering](https://docs.microsoft.com/azure/active-directory/saas-apps/greenlight-compliant-access-management-tutorial), [grok Learning](https://docs.microsoft.com/azure/active-directory/saas-apps/grok-learning-tutorial), [Miradore online](https://login.online.miradore.com/), [Khoros Care](https://docs.microsoft.com/azure/active-directory/saas-apps/khoros-care-tutorial) [, AskYourTeam, TruNarrative](https://docs.microsoft.com/azure/active-directory/saas-apps/askyourteam-tutorial) [, Smartwaiver](https://docs.microsoft.com/azure/active-directory/saas-apps/trunarrative-tutorial), [Bizagi Studio för digital process Automation](https://docs.microsoft.com/azure/active-directory/saas-apps/bizagi-studio-for-digital-process-automation-tutorial), [insuiteX](https://www.insuite.jp/) [, Sybo](https://www.systexsoftware.com.tw/) [, Britive](https://docs.microsoft.com/azure/active-directory/saas-apps/britive-tutorial) [,](https://docs.microsoft.com/azure/active-directory/saas-apps/whosoffice-tutorial) [WhosOffice,](https://www.smartwaiver.com/m/user/sw_login.php?wms_login) [E-dagar](https://docs.microsoft.com/azure/active-directory/saas-apps/e-days-tutorial), [Kollective SDN](https://portal.kollective.app/login), [Witivio](https://app.witivio.com/), [PlayVox](https://my.playvox.com/login), [korn fartyg 360](https://docs.microsoft.com/azure/active-directory/saas-apps/korn-ferry-360-tutorial), [Campus kafé](https://docs.microsoft.com/azure/active-directory/saas-apps/campus-cafe-tutorial) [, Catchpoint](https://docs.microsoft.com/azure/active-directory/saas-apps/catchpoint-tutorial) [, Code42](https://docs.microsoft.com/azure/active-directory/saas-apps/code42-tutorial)

Mer information om apparna finns i [SaaS Application Integration with Azure Active Directory](https://aka.ms/appstutorial). Mer information om hur du visar ditt program i Azure AD App-galleriet finns i [lista ditt program i Azure Active Directory program galleriet](https://aka.ms/azureadapprequest).

---

### <a name="azure-ad-b2b-collaboration-available-in-azure-government-tenants"></a>Azure AD B2B-samarbete tillgängligt i Azure Government-klienter

**Typ:** Ny funktion  
**Tjänste kategori:** Business  
**Produkt kapacitet:** B2B/B2C
 
Samarbets funktionerna i Azure AD B2B är nu tillgängliga mellan vissa Azure Government klienter.  Om du vill ta reda på om din klient kan använda de här funktionerna följer du anvisningarna på [Hur vet jag om B2B-samarbete är tillgängligt i min Azure amerikanska myndighets klient?](https://docs.microsoft.com/azure/active-directory/b2b/current-limitations#how-can-i-tell-if-b2b-collaboration-is-available-in-my-azure-us-government-tenant).

---

### <a name="azure-monitor-integration-for-azure-logs-is-now-available-in-azure-government"></a>Azure Monitor integration för Azure-loggar är nu tillgänglig i Azure Government

**Typ:** Ny funktion  
**Tjänste kategori:** Uppgiftslämn  
**Produkt kapacitet:** Övervaka & rapportering
 
Azure Monitor integrering med Azure AD-loggar finns nu i Azure Government. Du kan skicka Azure AD-loggar (gransknings-och inloggnings loggar) till ett lagrings konto, Event Hub och Log Analytics. Läs igenom den [detaljerade dokumentationen](https://aka.ms/aadlogsinamd) [och distributions planer för rapportering och övervakning](https://docs.microsoft.com/azure/active-directory/reports-monitoring/plan-monitoring-and-reporting) av Azure AD-scenarier.

---

### <a name="identity-protection-refresh-in-azure-government"></a>Uppdatera identitets skydd i Azure Government

**Typ:** Ny funktion  
**Tjänste kategori:** Identitets skydd  
**Produkt kapacitet:** & skydd för identitets säkerhet

Vi är glada att kunna dela att vi nu har lanserat den uppdaterade [Azure AD Identity Protection](https://aka.ms/IdentityProtectionDocs)   upplevelsen på [Microsoft Azure Government portalen](https://portal.azure.us/). Mer information finns i [blogg inlägget för meddelande](https://techcommunity.microsoft.com/t5/public-sector-blog/identity-protection-refresh-in-microsoft-azure-government/ba-p/1223667).

---

### <a name="disaster-recovery-download-and-store-your-provisioning-configuration"></a>Haveri beredskap: Hämta och lagra etablerings konfigurationen

**Typ:** Ny funktion  
**Tjänste kategori:** App-etablering  
**Produkt kapacitet:** Hantering av identitets livs cykel
 
Azure AD Provisioning-tjänsten innehåller en omfattande uppsättning konfigurations funktioner. Kunderna måste kunna spara sin konfiguration så att de kan referera till den senare eller återställa till en känd version. Vi har lagt till möjligheten att ladda ned din etablerings konfiguration som en JSON-fil och ladda upp den när du behöver den. [Läs mer](https://docs.microsoft.com/azure/active-directory/app-provisioning/export-import-provisioning-configuration).

---
 
### <a name="sspr-self-service-password-reset-now-requires-two-gates-for-admins-in-microsoft-azure-operated-by-21vianet-azure-china-21vianet"></a>SSPR (lösen ords återställning via självbetjäning) kräver nu två portar för administratörer i Microsoft Azure som drivs av 21Vianet (Azure Kina 21Vianet) 

**Typ:** Ändrad funktion  
**Tjänste kategori:** Lösen ords återställning via självbetjäning  
**Produkt kapacitet:** & skydd för identitets säkerhet
 
Tidigare i Microsoft Azure som drivs av 21Vianet (Azure Kina 21Vianet), administratörer som använder självbetjäning för återställning av lösen ord (SSPR) för att återställa sina egna lösen ord behövde bara en "grind" (Challenge) för att bevisa sin identitet. I offentliga och andra nationella moln måste administratörerna i allmänhet använda två portar för att bevisa sin identitet när de använder SSPR. Men eftersom vi inte har stöd för SMS eller telefonsamtal i Azure Kina 21Vianet, tillät vi ett-Gate lösen ords återställning av administratörer.

Vi skapar SSPR funktions paritet mellan Azure Kina 21Vianet och det offentliga molnet. Administratörerna måste använda två portar när de använder SSPR. SMS, telefonsamtal och meddelandeautentisering och-koder kommer att stödjas. [Läs mer](https://docs.microsoft.com/azure/active-directory/authentication/concept-sspr-policy#administrator-reset-policy-differences).

---

### <a name="password-length-is-limited-to-256-characters"></a>Lösen ords längden är begränsad till 256 tecken

**Typ:** Ändrad funktion  
**Tjänste kategori:** Autentiseringar (inloggningar)  
**Produkt kapacitet:** Användarautentisering
 
För att säkerställa tillförlitligheten för Azure AD-tjänsten är användar lösen ord nu begränsade till 256 tecken. Användare med lösen ord som är längre än detta kommer att uppmanas att ändra sina lösen ord vid efterföljande inloggningar, antingen genom att kontakta deras administratör eller genom att använda funktionen för lösen ords återställning via självbetjäning.

Den här ändringen aktiverades den 13 mars 2020, 10 PST (18:00 UTC) och felet är AADSTS 50052, InvalidPasswordExceedsMaxLength. Mer information finns i meddelandet om att [bryta ändring](https://docs.microsoft.com/azure/active-directory/develop/reference-breaking-changes#user-passwords-will-be-restricted-to-256-characters) .

---

### <a name="azure-ad-sign-in-logs-are-now-available-for-all-free-tenants-through-the-azure-portal"></a>Inloggnings loggarna för Azure AD är nu tillgängliga för alla kostnads fria klienter via Azure Portal

**Typ:** Ändrad funktion  
**Tjänste kategori:** Uppgiftslämn  
**Produkt kapacitet:** Övervaka & rapportering
 
Nu kan kunder som har kostnads fria klient organisationer komma åt [inloggnings loggarna för Azure AD från Azure Portal](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-sign-ins) i upp till 7 dagar. Tidigare var inloggnings loggar bara tillgängliga för kunder med Azure Active Directory Premium licenser. Med den här ändringen kan alla klienter komma åt dessa loggar via portalen.

> [!NOTE]
> Kunderna behöver fortfarande en Premium licens (Azure Active Directory Premium P1 eller P2) för att komma åt inloggnings loggarna via Microsoft Graph-API och Azure Monitor.

---

### <a name="deprecation-of-directory-wide-groups-option-from-groups-general-settings-on-azure-portal"></a>Utfasning av alternativ för katalog över hela gruppen från grupper allmänna inställningar på Azure Portal

**Typ:** Föråldrad  
**Tjänste kategori:** Grupp hantering  
**Produkt kapacitet:** Samarbete

För att ge kunderna ett mer flexibelt sätt att skapa katalog grupper som bäst uppfyller deras behov har vi ersatt alternativet för **katalog grupper** **från gruppen**  >  **allmänna** inställningar i Azure Portal med en länk till [dynamisk grupp dokumentation](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-dynamic-membership). Vi har förbättrat vår dokumentation för att inkludera fler instruktioner så att administratörer kan skapa grupper med alla användare som inkluderar eller undantar gäst användare.

---

## <a name="february-2020"></a>Februari 2020

### <a name="upcoming-changes-to-custom-controls"></a>Kommande ändringar i anpassade kontroller

**Typ:** Planera för ändring  
**Tjänste kategori:** MFA  
**Produkt kapacitet:** & skydd för identitets säkerhet
 
Vi planerar att ersätta den aktuella för hands versionen av anpassade kontroller med en metod som gör det möjligt att samar beta med de funktioner som tillhandahålls av partner för att arbeta smidigt med Azure Active Directory administratör och slutanvändarens upplevelse. I dag är partner MFA-lösningarna riktade mot följande begränsningar: de fungerar bara när ett lösen ord har angetts. de fungerar inte som MFA för stegvis autentisering i andra nyckel scenarier. och de integreras inte med funktionerna för hantering av slutanvändare och administration av autentiseringsuppgifter. Den nya implementeringen tillåter att samarbets faktorer som tillhandahålls av partner arbetar tillsammans med inbyggda faktorer för viktiga scenarier, inklusive registrering, användning, MFA-anspråk, stegvis autentisering, rapportering och loggning. 

Anpassade kontroller fortsätter att stödjas i för hands versionen tillsammans med den nya designen tills den når allmän tillgänglighet. Vid det här skedet ger vi kunderna tid att migrera till den nya designen. På grund av begränsningarna i den aktuella metoden kommer vi inte att publicera nya leverantörer förrän den nya designen är tillgänglig. Vi arbetar nära kunder och leverantörer och kommer att kommunicera med tids linjen medan vi får närmare. [Läs mer](https://techcommunity.microsoft.com/t5/azure-active-directory-identity/upcoming-changes-to-custom-controls/ba-p/1144696#).

---

### <a name="identity-secure-score---mfa-improvement-action-updates"></a>Identifiera säkra Poäng – MFA Improvement åtgärd uppdateringar

**Typ:** Planera för ändring  
**Tjänste kategori:** MFA  
**Produkt kapacitet:** & skydd för identitets säkerhet
 
För att avspegla behovet av företag för att säkerställa den högsta säkerheten vid användning av principer som fungerar med deras verksamhet, tar Microsoft Secures score bort tre förbättrings åtgärder som är centrerade kring Multi-Factor Authentication (MFA) och lägger till två.

Följande förbättrings åtgärder kommer att tas bort:

- Registrera alla användare för MFA
- Kräv MFA för alla användare
- Kräv MFA för privilegierade Azure AD-roller

Följande förbättrings åtgärder kommer att läggas till:

- Se till att alla användare kan slutföra MFA för säker åtkomst
- Kräv MFA för administrativa roller

Dessa nya förbättringar kräver att du registrerar användare eller administratörer för MFA i din katalog och hur du skapar rätt uppsättning principer som passar organisationens behov. Huvud målet är att ha flexibilitet och samtidigt se till att alla användare och administratörer kan autentisera med flera faktorer eller riskfyllda identitets verifierings meddelanden. Detta kan vara en form av att ställa in säkerhets inställningar som gör att Microsoft kan avgöra när användare ska kunna använda MFA eller ha flera principer som tillämpar besluts fattandet i definitions området. Som en del av dessa uppdateringar av förbättrings åtgärden kommer principer för bas linje skydd inte längre att ingå i beräknings beräkningar. [Läs mer om vad som kommer i Microsofts säkra Poäng](https://docs.microsoft.com/microsoft-365/security/mtp/microsoft-secure-score-whats-coming?view=o365-worldwide).

---

### <a name="azure-ad-domain-services-sku-selection"></a>Val av Azure AD Domain Services SKU

**Typ:** Ny funktion  
**Tjänste kategori:** Azure AD Domain Services  
**Produkt kapacitet:** Azure AD Domain Services
 
Vi har hört feedback som Azure AD Domain Services kunder vill ha mer flexibilitet när de väljer prestanda nivåer för sina instanser. Från och med den 1 februari 2020 bytte vi från en dynamisk modell (där Azure AD fastställer prestanda-och pris nivån baserat på antal objekt) till en egen markerings modell. Nu kan kunderna välja en prestanda nivå som matchar deras miljö. Den här ändringen gör det också möjligt för oss att aktivera nya scenarier som resurs skogar och Premium funktioner som dagliga säkerhets kopieringar. Antalet objekt är nu obegränsat för alla SKU: er, men vi fortsätter att erbjuda förslag på antal objekt för varje nivå.

**Ingen omedelbar kund åtgärd krävs.** För befintliga kunder fastställer den dynamiska nivån som användes den 1 februari 2020 den nya standard nivån. Det finns inga priser eller prestanda som påverkar resultatet av den här ändringen. Azure AD DS-kunder kommer att behöva utvärdera prestanda krav när deras katalog storlek och arbets belastnings egenskaper ändras. Växling mellan tjänst nivåer fortsätter att vara en åtgärd utan avbrott och vi kommer inte längre att automatiskt flytta kunder till nya nivåer baserat på katalogens tillväxt. Dessutom kommer det inte att finnas några prisökningar och den nya prissättningen kommer att justeras mot vår aktuella fakturerings modell. Mer information finns i dokumentationen för [Azure AD DS SKU](https://docs.microsoft.com/azure/active-directory-domain-services/administration-concepts#azure-ad-ds-skus) och [sidan Azure AD Domain Services prissättning](https://azure.microsoft.com/pricing/details/active-directory-ds/).

---
 
### <a name="new-federated-apps-available-in-azure-ad-app-gallery---february-2020"></a>Nya federerade appar som är tillgängliga i Azure AD App Galleri – februari 2020

**Typ:** Ny funktion  
**Tjänste kategori:** Företags program  
**Produkt kapacitet:** integration från tredje part
 
I februari 2020 har vi lagt till dessa 31 nya appar med stöd för federation i app-galleriet: 

[IamIP patent Platform](https://docs.microsoft.com/azure/active-directory/saas-apps/iamip-patent-platform-tutorial), [Experience Cloud](https://docs.microsoft.com/azure/active-directory/saas-apps/experience-cloud-tutorial), [ns1 SSO för Azure](https://docs.microsoft.com/azure/active-directory/saas-apps/ns1-sso-azure-tutorial), [Barracuda e-mail Security Service](https://ess.barracudanetworks.com/sso/azure), [ABA repor ting](https://myaba.co.uk/client-access/signin/auth/msad), [i händelse av kris – online portal](https://docs.microsoft.com/azure/active-directory/saas-apps/in-case-of-crisis-online-portal-tutorial), [BIC Cloud design](https://docs.microsoft.com/azure/active-directory/saas-apps/bic-cloud-design-tutorial), [Biodlings Azure AD data Connector](https://docs.microsoft.com/azure/active-directory/saas-apps/beekeeper-azure-ad-data-connector-tutorial), [Korns](https://www.kornferry.com/solutions/kf-digital/kf-assess) [Verkada-kommando](https://docs.microsoft.com/azure/active-directory/saas-apps/verkada-command-tutorial), [Splashtop](https://docs.microsoft.com/azure/active-directory/saas-apps/splashtop-tutorial), [Syxsense](https://docs.microsoft.com/azure/active-directory/saas-apps/syxsense-tutorial), [EAB navigera](https://docs.microsoft.com/azure/active-directory/saas-apps/eab-navigate-tutorial), [New Relic (begränsad utgåva)](https://docs.microsoft.com/azure/active-directory/saas-apps/new-relic-limited-release-tutorial), [Thulium](https://admin.thulium.com/login/instance), [Tick Manager](https://docs.microsoft.com/azure/active-directory/saas-apps/ticketmanager-tutorial), [mall väljare för team](https://links.officeatwork.com/templatechooser-download-teams), [bin](https://www.beesy.me/index.php/site/login), [hälso support system](https://docs.microsoft.com/azure/active-directory/saas-apps/health-support-system-tutorial), [MURAL](https://app.mural.co/signup), [Hive](https://docs.microsoft.com/azure/active-directory/saas-apps/hive-tutorial), [LavaDo](https://appsource.microsoft.com/product/web-apps/lavaloon.lavado_standard?tab=Overview), Wakelet, [Wakelet](https://wakelet.com/login)Firmex [vdr](https://docs.microsoft.com/azure/active-directory/saas-apps/firmex-vdr-tutorial), [ThingLink för lärare och skolor](https://www.thinglink.com/) [,](https://docs.microsoft.com/azure/active-directory/saas-apps/teamviewer-tutorial) [CODA](https://docs.microsoft.com/azure/active-directory/saas-apps/coda-tutorial), [NearpodApp](https://nearpod.com/signup/?oc=Microsoft&utm_campaign=Microsoft&utm_medium=site&utm_source=product), [WEDO](https://docs.microsoft.com/azure/active-directory/saas-apps/wedo-tutorial) [, InvitePeople,](https://invitepeople.com/login) [,](https://docs.microsoft.com/azure/active-directory/saas-apps/reprints-desk-article-galaxy-tutorial)

 
Mer information om apparna finns i [SaaS Application Integration with Azure Active Directory](https://aka.ms/appstutorial). Mer information om hur du visar ditt program i Azure AD App-galleriet finns i [lista ditt program i Azure Active Directory program galleriet](https://aka.ms/azureadapprequest).

---
 
### <a name="new-provisioning-connectors-in-the-azure-ad-application-gallery---february-2020"></a>Nya etablerings anslutningar i Azure AD Application Gallery – februari 2020

**Typ:** Ny funktion  
**Tjänste kategori:** Företags program  
**Produkt kapacitet:** integration från tredje part
 
Nu kan du automatisera att skapa, uppdatera och ta bort användar konton för dessa nyligen integrerade appar:

- [Mixpanel](https://docs.microsoft.com/azure/active-directory/saas-apps/mixpanel-provisioning-tutorial)
- [TeamViewer](https://docs.microsoft.com/azure/active-directory/saas-apps/teamviewer-provisioning-tutorial)
- [Azure Databricks](https://docs.microsoft.com/azure/active-directory/saas-apps/azure-databricks-scim-connector-provisioning-tutorial)
- [PureCloud by Genesys](https://docs.microsoft.com/azure/active-directory/saas-apps/purecloud-by-genesys-provisioning-tutorial)
- [Zapier](https://docs.microsoft.com/azure/active-directory/saas-apps/zapier-provisioning-tutorial)

Mer information om hur du bättre skyddar din organisation med hjälp av automatiserad användar konto etablering finns i [Automatisera användar etablering för SaaS-program med Azure AD](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning).

---
 
### <a name="azure-ad-support-for-fido2-security-keys-in-hybrid-environments"></a>Azure AD-stöd för FIDO2 säkerhets nycklar i hybrid miljöer

**Typ:** Ny funktion  
**Tjänste kategori:** Autentiseringar (inloggningar)  
**Produkt kapacitet:** Användarautentisering
 
Vi presenterar den offentliga för hands versionen av Azure AD-stöd för FIDO2-säkerhetsnycklar i hybrid miljöer. Användare kan nu använda FIDO2-säkerhetsnycklar för att logga in på sina hybrid Azure AD-anslutna Windows 10-enheter och få sömlös inloggning till sina lokala och molnbaserade resurser. Stöd för Hybrid miljöer har varit den mest efterfrågade funktionen från våra kunder med lösen ords skydd eftersom vi ursprungligen startade den offentliga för hands versionen för FIDO2-stöd i Azure AD-anslutna enheter. Autentisering utan lösen ord med avancerade tekniker som biometrik och offentlig/privat nyckel kryptering ger bekvämlighet och enkel användning medan de är säkra. Med den här offentliga för hands versionen kan du nu använda modern autentisering som FIDO2-säkerhetsnycklar för att komma åt traditionella Active Directory-resurser. Mer information finns [på SSO till lokala resurser](https://docs.microsoft.com/azure/active-directory/authentication/howto-authentication-passwordless-security-key-on-premises). 

Kom igång genom att gå till [Aktivera FIDO2-säkerhetsnycklar för din klient](https://docs.microsoft.com/azure/active-directory/authentication/howto-authentication-passwordless-security-key) för stegvisa anvisningar. 

---
 
### <a name="the-new-my-account-experience-is-now-generally-available"></a>Den nya mitt konto upplevelsen är nu allmänt tillgänglig

**Typ:** Ändrad funktion  
**Tjänste kategori:** Min profil/konto  
**Produkt kapacitet:** Slut användar upplevelser
 
Mitt konto, det enda steget för att hantera slut användar kontots behov, är nu allmänt tillgängligt! Slutanvändare kan komma åt den här nya webbplatsen via URL eller i rubriken för den nya Mina appar-upplevelsen. Lär dig mer om alla självbetjänings funktioner som den nya upplevelsen erbjuder i [min konto Portal översikt](https://docs.microsoft.com/azure/active-directory/user-help/my-account-portal-overview).

---
 
### <a name="my-account-site-url-updating-to-myaccountmicrosoftcom"></a>Min konto webbplats-URL uppdateras till myaccount.microsoft.com

**Typ:** Ändrad funktion  
**Tjänste kategori:** Min profil/konto  
**Produkt kapacitet:** Slut användar upplevelser
 
Den nya slut användar upplevelsen för mitt konto kommer att uppdatera URL: en till `https://myaccount.microsoft.com` Nästa månad. Hitta mer information om upplevelsen och alla självbetjänings funktioner som det erbjuder för slutanvändare i [mitt konto Portal hjälp](https://docs.microsoft.com/azure/active-directory/user-help/my-account-portal-overview).

---
 
## <a name="january-2020"></a>Januari 2020
 
### <a name="the-new-my-apps-portal-is-now-generally-available"></a>Den nya portalen för Mina appar är nu allmänt tillgänglig

**Typ:** Planera för ändring  
**Tjänste kategori:** Mina appar  
**Produkt kapacitet:** Slut användar upplevelser
 
Uppgradera din organisation till den nya My Apps-portalen som nu är allmänt tillgänglig! Mer information om den nya portalen och samlingar finns i [skapa samlingar på mina apps-portalen](https://docs.microsoft.com/azure/active-directory/manage-apps/access-panel-collections).

---
 
### <a name="workspaces-in-azure-ad-have-been-renamed-to-collections"></a>Arbets ytor i Azure AD har bytt namn till samlingar

**Typ:** Ändrad funktion  
**Tjänste kategori:** Mina appar   
**Produkt kapacitet:** Slut användar upplevelser
 
Arbets ytor, filter administratörer kan konfigurera för att organisera sina användares appar, kommer nu att kallas samlingar. Mer information om hur du konfigurerar dem finns i [skapa samlingar på mina apps-portalen](https://docs.microsoft.com/azure/active-directory/manage-apps/access-panel-collections).

---
 
### <a name="azure-ad-b2c-phone-sign-up-and-sign-in-using-custom-policy-public-preview"></a>Azure AD B2C telefonin loggning och inloggning med anpassad princip (offentlig för hands version)

**Typ:** Ny funktion  
**Tjänste kategori:** B2C – konsument identitets hantering  
**Produkt kapacitet:** B2B/B2C
 
Med telefonnummer för registrering och inloggning kan utvecklare och företag låta sina kunder registrera sig och logga in med ett eng ång slö sen ord som skickas till användarens telefonnummer via SMS. Den här funktionen gör det också möjligt för kunden att ändra sina telefonnummer om de förlorar åtkomsten till sin telefon. Med hjälp av anpassade principer kan du använda telefonin loggning och inloggning för att kommunicera med sitt varumärke genom sidan anpassning. Ta reda på hur du [ställer in telefonin loggning och inloggning med anpassade principer i Azure AD B2C](https://docs.microsoft.com/azure/active-directory-b2c/phone-authentication).
 
---
 
### <a name="new-provisioning-connectors-in-the-azure-ad-application-gallery---january-2020"></a>Nya etablerings anslutningar i Azure AD Application Gallery – januari 2020

**Typ:** Ny funktion  
**Tjänste kategori:** Företags program  
**Produkt kapacitet:** integration från tredje part
 
Nu kan du automatisera att skapa, uppdatera och ta bort användar konton för dessa nyligen integrerade appar:

- [Promapp]( https://docs.microsoft.com/azure/active-directory/saas-apps/promapp-provisioning-tutorial)
- [Zscaler Private Access](https://docs.microsoft.com/azure/active-directory/saas-apps/zscaler-private-access-provisioning-tutorial)

Mer information om hur du bättre skyddar din organisation med hjälp av automatiserad användar konto etablering finns i [Automatisera användar etablering för SaaS-program med Azure AD](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning).

---
 
### <a name="new-federated-apps-available-in-azure-ad-app-gallery---january-2020"></a>Nya federerade appar som är tillgängliga i Azure AD App Galleri – januari 2020

**Typ:** Ny funktion  
**Tjänste kategori:** Företags program  
**Produkt kapacitet:** integration från tredje part
 
I januari 2020 har vi lagt till dessa 33 nya appar med stöd för federation i app-galleriet: 

[JOSA](https://docs.microsoft.com/azure/active-directory/saas-apps/josa-tutorial), [snabbt Edge-moln](https://docs.microsoft.com/azure/active-directory/saas-apps/fastly-edge-cloud-tutorial), [terraform Enterprise](https://docs.microsoft.com/azure/active-directory/saas-apps/terraform-enterprise-tutorial), [Spintr SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/spintr-sso-tutorial), [Abibot Netlogistik](https://azuremarketplace.microsoft.com/marketplace/apps/aad.abibotnetlogistik), [SkyKick](https://login.skykick.com/login?state=g6Fo2SBTd3M5Q0xBT0JMd3luS2JUTGlYN3pYTE1remJQZnR1c6N0aWTZIDhCSkwzYVQxX2ZMZjNUaWxNUHhCSXg2OHJzbllTcmYto2NpZNkgM0h6czk3ZlF6aFNJV1VNVWQzMmpHeFFDbDRIMkx5VEc&client=3Hzs97fQzhSIWUMUd32jGxQCl4H2LyTG&protocol=oauth2&audience=https://papi.skykick.com&response_type=code&redirect_uri=https://portal.skykick.com/callback&scope=openid%20profile%20offline_access), [LeaveBot](https://docs.microsoft.com/azure/active-directory/saas-apps/upshotly-tutorial), [DataCamp](https://docs.microsoft.com/azure/active-directory/saas-apps/datacamp-tutorial), [TripActions, SmartWork](https://docs.microsoft.com/azure/active-directory/saas-apps/tripactions-tutorial), [Dotcom, SSOGEN](https://docs.microsoft.com/azure/active-directory/saas-apps/dotcom-monitor-tutorial), [EBS,](https://leavebot.io/#home),,, [,](https://www.intumit.com/english/SmartWork.html)- [Azure AD SSO Gateway för Oracle E-Business Suite-, JDE](https://docs.microsoft.com/azure/active-directory/saas-apps/ssogen-tutorial), [VÄRDBASERAD MyCirqa SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/hosted-mycirqa-sso-tutorial), [yuhu Property Management Platform](https://docs.microsoft.com/azure/active-directory/saas-apps/yuhu-property-management-platform-tutorial), [LumApps](https://sites.lumapps.com/login), driver- [Enterprise](https://docs.microsoft.com/azure/active-directory/saas-apps/upwork-enterprise-tutorial), [TalentSoft](https://docs.microsoft.com/azure/active-directory/saas-apps/talentsoft-tutorial), [SmartDB för Microsoft Teams](http://teams.smartdb.jp/login/), [PressPage](https://docs.microsoft.com/azure/active-directory/saas-apps/presspage-tutorial), [ContractSafe Saml2 SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/contractsafe-saml2-sso-tutorial), [Maxient Driver Manager-programvara](https://docs.microsoft.com/azure/active-directory/saas-apps/maxient-conduct-manager-software-tutorial), [helpshift](https://docs.microsoft.com/azure/active-directory/saas-apps/helpshift-tutorial), [PortalTalk 365](https://www.portaltalk.com/) [,,](https://portal.coreview.com/) [Squelch Cloud Office365 Connector](https://laxmi.squelch.io/login), [PingFlow Authentication](https://app-staging.pingview.io/), PrinterLogic [SaaS](https://docs.microsoft.com/azure/active-directory/saas-apps/printerlogic-saas-tutorial), [Taskize Connect](https://docs.microsoft.com/azure/active-directory/saas-apps/taskize-connect-tutorial), [Sandwai](https://app.sandwai.com/), [EZRentOut](https://docs.microsoft.com/azure/active-directory/saas-apps/ezrentout-tutorial), [AssetSonar](https://docs.microsoft.com/azure/active-directory/saas-apps/assetsonar-tutorial), Akari [Virtual Assistant](https://akari.io/akari-virtual-assistant/)

Mer information om apparna finns i [SaaS Application Integration with Azure Active Directory](https://aka.ms/appstutorial). Mer information om hur du visar ditt program i Azure AD App-galleriet finns i [lista ditt program i Azure Active Directory program galleriet](https://aka.ms/azureadapprequest).

---

### <a name="two-new-identity-protection-detections"></a>Två nya identifieringar av identitets skydd

**Typ:** Ny funktion  
**Tjänste kategori:** Identitets skydd  
**Produkt kapacitet:** & skydd för identitets säkerhet
 
Vi har lagt till två nya inloggnings typer som är länkade till identitets skydd: misstänkta regler för att ändra Inkorgen och omöjlig resa. De här offline-identifieringarna upptäcks av Microsoft Cloud App Security (MCAS) och påverkar användaren och inloggnings risken i identitets skyddet. Mer information om dessa identifieringar finns i våra [typer av inloggnings risker](https://docs.microsoft.com/azure/active-directory/identity-protection/concept-identity-protection-risks#sign-in-risk).
 
---
 
### <a name="breaking-change-uri-fragments-will-not-be-carried-through-the-login-redirect"></a>Bryta ändring: URI-fragment kommer inte att transporteras via omdirigeringen för inloggning

**Typ:** Ändrad funktion  
**Tjänste kategori:** Autentiseringar (inloggningar)  
**Produkt kapacitet:** Användarautentisering
 
Från och med den 8 februari 2020 lägger tjänsten till ett tomt fragment i begäran när en begäran skickas till login.microsoftonline.com för att logga in en användare.  Detta förhindrar en klass av omdirigerings attacker genom att se till att webbläsaren rensar alla befintliga fragment i begäran. Inget program bör ha ett beroende på detta beteende. Mer information finns i avsnittet om att [dela upp ändringar](https://docs.microsoft.com/azure/active-directory/develop/reference-breaking-changes#february-2020) i dokumentationen för Microsoft Identity Platform.

---

## <a name="december-2019"></a>December 2019

### <a name="integrate-sap-successfactors-provisioning-into-azure-ad-and-on-premises-ad-public-preview"></a>Integrera SAP SuccessFactors-etablering i Azure AD och on-premises AD (offentlig för hands version)

**Typ:** Ny funktion  
**Tjänste kategori:** App-etablering  
**Produkt kapacitet:** Hantering av identitets livs cykel

Nu kan du integrera SAP-SuccessFactors som en auktoritativ identitets källa i Azure AD. Den här integreringen hjälper dig att automatisera livs cykeln för slut punkt till slut punkt, inklusive användning av HR-baserade händelser, t. ex. nya anställningar eller uppsägningar, för att styra etableringen av Azure AD-konton.

Mer information om hur du konfigurerar SAP SuccessFactors inkommande etablering i Azure AD finns i själv studie kursen [Konfigurera SAP-SuccessFactors automatisk etablering](https://aka.ms/SAPSuccessFactorsInboundTutorial) .

---

### <a name="support-for-customized-emails-in-azure-ad-b2c-public-preview"></a>Stöd för anpassade e-postmeddelanden i Azure AD B2C (offentlig för hands version)

**Typ:** Ny funktion  
**Tjänste kategori:** B2C – konsument identitets hantering  
**Produkt kapacitet:** B2B/B2C

Nu kan du använda Azure AD B2C för att skapa anpassade e-postmeddelanden när användarna registrerar sig för att använda dina appar. Genom att använda DisplayControls (för närvarande i för hands version) och en e-postprovider från tredje part (till exempel, [SendGrid](https://sendgrid.com/), [Spark post](https://sparkpost.com/)eller en anpassad REST API), kan du använda din egen e-postmall, **från** adress och ämnes text, samt stöd för lokalisering och anpassad eng ång slö sen ord.

Mer information finns i [anpassad e-postverifiering i Azure Active Directory B2C](https://docs.microsoft.com/azure/active-directory-b2c/custom-email).

---

### <a name="replacement-of-baseline-policies-with-security-defaults"></a>Utbyte av bas linje principer med säkerhets inställningar

**Typ:** Ändrad funktion  
**Tjänste kategori:** Andra  
**Produkt kapacitet:** Identitets säkerhet och skydd

Som en del av en säker modell som är säker för autentisering tar vi bort befintliga principer för bas linje skydd från alla klienter. Den här borttagningen är avsedd för slut för ande i slutet av februari. Ersättningen för de här bas linje skydds principerna är säkerhets inställningar. Om du har använt principer för bas linje skydd måste du planera att flytta till den nya säkerhets standard principen eller villkorlig åtkomst. Om du inte har använt de här principerna finns det ingen åtgärd att vidta.

Mer information om de nya säkerhets standarderna finns i [Vad är säkerhets inställningar?](https://docs.microsoft.com/azure/active-directory/fundamentals/concept-fundamentals-security-defaults) Mer information om principer för villkorlig åtkomst finns i [vanliga principer för villkorlig åtkomst](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-conditional-access-policy-common).

---

## <a name="november-2019"></a>November 2019

### <a name="support-for-the-samesite-attribute-and-chrome-80"></a>Stöd för SameSite-attributet och Chrome 80

**Typ:** Planera för ändring  
**Tjänste kategori:** Autentiseringar (inloggningar)  
**Produkt kapacitet:** Användarautentisering

Som en del av en säker standard modell för cookies, ändrar Chrome 80-webbläsaren hur den behandlar cookies utan `SameSite` attributet. Alla cookies som inte anger `SameSite` attributet behandlas som om det var inställt på `SameSite=Lax` , vilket leder till att Chrome blockerar vissa scenarier mellan domänens cookie-delning som din app kan vara beroende av. Om du vill behålla det äldre Chrome-beteendet kan du använda `SameSite=None` attributet och lägga till ytterligare ett `Secure` attribut, så att cookies från flera platser bara kan nås via HTTPS-anslutningar. Chrome har schemalagts för att slutföra den här ändringen senast den 4 februari 2020.

Vi rekommenderar att alla utvecklare testar sina appar med hjälp av den här vägledningen:

- Ange standardvärdet för inställningen **Använd säker cookie** till **Ja**.

- Ange standardvärdet för attributet **SameSite** till **none**.

- Lägg till ytterligare ett `SameSite` attribut för **Secure**.

Mer information finns i [kommande SameSite cookie-ändringar i ASP.net och ASP.net Core](https://devblogs.microsoft.com/aspnet/upcoming-samesite-cookie-changes-in-asp-net-and-asp-net-core/) och [potentiella avbrott till kund webbplatser och Microsofts produkter och tjänster i Chrome version 79 och senare](https://support.microsoft.com/help/4522904/potential-disruption-to-microsoft-services-in-chrome-beta-version-79).

---

### <a name="new-hotfix-for-microsoft-identity-manager-mim-2016-service-pack-2-sp2"></a>Ny snabb korrigering för Microsoft Identity Manager (MIM) 2016 Service Pack 2 (SP2)

**Typ:** Fastsatt  
**Tjänste kategori:** Microsoft Identity Manager  
**Produkt kapacitet:** Hantering av identitets livs cykel

Det finns ett samlat snabb korrigerings paket (build 4.6.34.0) för Microsoft Identity Manager (MIM) 2016 Service Pack 2 (SP2). Det här samlade paketet löser problem och lägger till förbättringar som beskrivs i avsnittet "problem som åtgärd ATS och förbättringar som lagts till i den här uppdateringen".

Mer information och hämta snabb korrigerings paketet finns [Microsoft Identity Manager 2016 Service Pack 2 (build 4.6.34.0) Samlad uppdatering är tillgänglig](https://support.microsoft.com/help/4512924/microsoft-identity-manager-2016-service-pack-2-build-4-6-34-0-update-r).

---

### <a name="new-ad-fs-app-activity-report-to-help-migrate-apps-to-azure-ad-public-preview"></a>Ny rapport för AD FS app-aktivitet som hjälper till att migrera appar till Azure AD (offentlig för hands version)

**Typ:** Ny funktion  
**Tjänste kategori:** Företags program  
**Produkt kapacitet:** DEFINITION

Använd aktivitets rapporten ny Active Directory Federation Services (AD FS) (AD FS) i Azure Portal för att identifiera vilka av dina appar som kan migreras till Azure AD. Rapporten visar alla AD FS appar för kompatibilitet med Azure AD, söker efter eventuella problem och ger vägledning om att förbereda enskilda appar för migrering.

Mer information finns i [använda rapporten AD FS program aktivitet för att migrera program till Azure AD](https://docs.microsoft.com/azure/active-directory/manage-apps/migrate-adfs-application-activity).

---

### <a name="new-workflow-for-users-to-request-administrator-consent-public-preview"></a>Nytt arbets flöde för användare som begär administratörs medgivande (offentlig för hands version)

**Typ:** Ny funktion  
**Tjänste kategori:** Företags program  
**Produkt kapacitet:** Access Control

Det nya administratörs godkännande arbets flödet ger administratörer ett sätt att bevilja åtkomst till appar som kräver administratörs godkännande. Om en användare försöker få åtkomst till en app, men inte kan ge samtycke, kan de nu skicka en begäran om administratörs godkännande. Begäran skickas via e-post och placeras i en kö som är tillgänglig från Azure Portal, till alla administratörer som har angetts som granskare. När en granskare har åtgärd ATS för en väntande begäran, meddelas de begär ande användarna om åtgärden.

Mer information finns i [Konfigurera arbets flödet för administratörs medgivande (för hands version)](https://docs.microsoft.com/azure/active-directory/manage-apps/configure-admin-consent-workflow).

---

### <a name="new-azure-ad-app-registrations-token-configuration-experience-for-managing-optional-claims-public-preview"></a>Nya Azure AD App registrerar konfigurations upplevelsen för token för hantering av valfria anspråk (offentlig för hands version)

**Typ:** Ny funktion  
**Tjänste kategori:** Andra  
**Produkt kapacitet:** Utvecklings miljö

Bladet ny **Azure AD App registreringar för token-konfiguration** på Azure Portal visar nu Apps-utvecklare en dynamisk lista med valfria anspråk för sina appar. Den här nya upplevelsen hjälper till att effektivisera Azure AD-App-migreringar och minimera valfria felkonfigurationer för anspråk.

Mer information finns i [tillhandahålla valfria anspråk till din Azure AD-App](https://docs.microsoft.com/azure/active-directory/develop/active-directory-optional-claims).

---

### <a name="new-two-stage-approval-workflow-in-azure-ad-entitlement-management-public-preview"></a>Nytt arbets flöde med två stegs godkännande i hantering av Azure AD-rättigheter (offentlig för hands version)

**Typ:** Ny funktion  
**Tjänste kategori:** Andra  
**Produkt kapacitet:** Hantering av rättigheter

Vi har lanserat ett nytt arbets flöde med två steg som gör att du kan kräva att två god kännare godkänner en användares begäran till ett Access-paket. Du kan till exempel ställa in det så att den begär ande användarens chef först måste godkänna, och sedan kan du också kräva att en resurs ägare godkänner detta. Om en av god kännarna inte godkänner beviljas inte åtkomst.

Mer information finns i [Inställningar för ändring av begäran och godkännande för ett Access-paket i hantering av Azure AD-rättigheter](https://docs.microsoft.com/azure/active-directory/governance/entitlement-management-access-package-request-policy).

---

### <a name="updates-to-the-my-apps-page-along-with-new-workspaces-public-preview"></a>Uppdateringar av sidan Mina appar tillsammans med nya arbets ytor (offentlig för hands version)

**Typ:** Ny funktion  
**Tjänste kategori:** Mina appar  
**Produkt kapacitet:** integration från tredje part

Nu kan du anpassa hur din organisations användare visar och får åtkomst till den uppdaterade Mina appar-upplevelsen. Den här nya upplevelsen innehåller även funktionen nya arbets ytor, vilket gör det enklare för användarna att hitta och organisera appar.

Mer information om den nya upplevelsen för Mina appar och hur du skapar arbets ytor finns i [skapa arbets ytor på portalen Mina appar](https://docs.microsoft.com/azure/active-directory/manage-apps/access-panel-workspaces).

---

### <a name="google-social-id-support-for-azure-ad-b2b-collaboration-general-availability"></a>Google social ID-stöd för Azure AD B2B-samarbete (allmän tillgänglighet)

**Typ:** Ny funktion  
**Tjänste kategori:** Business  
**Produkt kapacitet:** Användarautentisering

Nytt stöd för användning av Google social ID (Gmail-konton) i Azure AD hjälper till att förenkla samarbetet för dina användare och partner. Du behöver inte längre använda dina partner för att skapa och hantera ett nytt Microsoft-särskilt konto. Microsoft Teams har nu fullständigt stöd för Google-användare på alla klienter och över de vanliga och klient-relaterade autentiserings slut punkterna.

Mer information finns i [lägga till Google som en identitets leverantör för B2B-gäst användare](https://docs.microsoft.com/azure/active-directory/b2b/google-federation).

---

### <a name="microsoft-edge-mobile-support-for-conditional-access-and-single-sign-on-general-availability"></a>Microsoft Edge Mobile-support för villkorlig åtkomst och enkel inloggning (allmän tillgänglighet)

**Typ:** Ny funktion  
**Tjänste kategori:** Villkorlig åtkomst  
**Produkt kapacitet:** & skydd för identitets säkerhet

Azure AD för Microsoft Edge på iOS och Android har nu stöd för enkel inloggning och villkorlig åtkomst i Azure AD:

- **Enkel inloggning för Microsoft Edge (SSO):** Enkel inloggning är nu tillgängligt över interna klienter (till exempel Microsoft Outlook och Microsoft Edge) för alla Azure AD-anslutna appar.

- **Villkorlig åtkomst för Microsoft Edge:** Via programbaserade villkorliga åtkomst principer måste användarna använda Microsoft Intune-skyddade webbläsare, till exempel Microsoft Edge.

Mer information om villkorlig åtkomst och enkel inloggning med Microsoft Edge finns i [Microsoft Edge Mobile support för villkorlig åtkomst och enkel inloggning nu allmänt tillgängliga](https://techcommunity.microsoft.com/t5/Intune-Customer-Success/Microsoft-Edge-Mobile-Support-for-Conditional-Access-and-Single/ba-p/988179) blogg inlägg. Mer information om hur du konfigurerar dina klient program med hjälp av [app-baserad villkorlig åtkomst](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access) eller [enhets-baserad villkorlig åtkomst](https://docs.microsoft.com/azure/active-directory/conditional-access/require-managed-devices)finns i [Hantera webb åtkomst med hjälp av en Microsoft Intune-princip-skyddad webbläsare](https://docs.microsoft.com/intune/apps/app-configuration-managed-browser).

---

### <a name="azure-ad-entitlement-management-general-availability"></a>Hantering av Azure AD-rättigheter (allmän tillgänglighet)

**Typ:** Ny funktion  
**Tjänste kategori:** Andra  
**Produkt kapacitet:** Hantering av rättigheter

Hantering av Azure AD-berättigande är en ny funktion för identitets styrning, som hjälper organisationer att hantera identitets-och åtkomst livs cykeln i stor skala. Den här nya funktionen hjälper till att automatisera arbets flöden för åtkomstbegäran, åtkomst tilldelningar, recensioner och förfallo datum för grupper, appar och SharePoint Online-webbplatser.

Med hantering av Azure AD-behörighet kan du effektivare hantera åtkomst både för anställda och även för användare utanför organisationen som behöver åtkomst till dessa resurser.

Mer information finns i [Vad är Azure AD-hantering av rättigheter?](https://docs.microsoft.com/azure/active-directory/governance/entitlement-management-overview#license-requirements)

---

### <a name="automate-user-account-provisioning-for-these-newly-supported-saas-apps"></a>Automatisera användar konto etablering för de här nyligen SaaS apparna som stöds

**Typ:** Ny funktion  
**Tjänste kategori:** Företags program  
**Produkt kapacitet:** integration från tredje part  

Nu kan du automatisera att skapa, uppdatera och ta bort användar konton för dessa nyligen integrerade appar:

[SAP Cloud Platform Identity Authentication Service](https://docs.microsoft.com/azure/active-directory/saas-apps/sap-hana-cloud-platform-identity-authentication-tutorial), [RingCentral](https://docs.microsoft.com/azure/active-directory/saas-apps/ringcentral-provisioning-tutorial), [SpaceIQ](https://docs.microsoft.com/azure/active-directory/saas-apps/spaceiq-provisioning-tutorial), [Miro](https://docs.microsoft.com/azure/active-directory/saas-apps/miro-provisioning-tutorial), [Cloudgate](https://docs.microsoft.com/azure/active-directory/saas-apps/soloinsight-cloudgate-sso-provisioning-tutorial), [infor CloudSuite](https://docs.microsoft.com/azure/active-directory/saas-apps/infor-cloudsuite-provisioning-tutorial), [OfficeSpace program vara](https://docs.microsoft.com/azure/active-directory/saas-apps/officespace-software-provisioning-tutorial), [prioritets mat ris](https://docs.microsoft.com/azure/active-directory/saas-apps/priority-matrix-provisioning-tutorial)

Mer information om hur du bättre skyddar din organisation med hjälp av automatiserad användar konto etablering finns i [Automatisera användar etablering för SaaS-program med Azure AD](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning).

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---november-2019"></a>Nya federerade appar som är tillgängliga i Azure AD App Galleri – november 2019

**Typ:** Ny funktion  
**Tjänste kategori:** Företags program  
**Produkt kapacitet:** integration från tredje part

I november 2019 har vi lagt till dessa 21 nya appar med stöd för federation i app-galleriet:

[Flygbord](https://docs.microsoft.com/azure/active-directory/saas-apps/airtable-tutorial), [HootSuite](https://docs.microsoft.com/azure/active-directory/saas-apps/hootsuite-tutorial), [blå åtkomst för medlemmar (BAM)](https://docs.microsoft.com/azure/active-directory/saas-apps/blue-access-for-members-tutorial), [bitly](https://docs.microsoft.com/azure/active-directory/saas-apps/bitly-tutorial), [Riva](https://docs.microsoft.com/azure/active-directory/saas-apps/riva-tutorial), [ResLife Portal](https://app.reslifecloud.com/hub5_signin/microsoft_azuread/?g=44BBB1F90915236A97502FF4BE2952CB&c=5&uid=0&ht=2&ref=), [NegometrixPortal enkel inloggning (SSO)](https://docs.microsoft.com/azure/active-directory/saas-apps/negometrixportal-tutorial), [TeamsChamp](https://login.microsoftonline.com/551f45da-b68e-4498-a7f5-a6e1efaeb41c/adminconsent?client_id=ca9bbfa4-1316-4c0f-a9ee-1248ac27f8ab&redirect_uri=https://admin.teamschamp.com/api/adminconsent&state=6883c143-cb59-42ee-a53a-bdb5faabf279) [, Motus,](https://docs.microsoft.com/azure/active-directory/saas-apps/motus-tutorial)MyAryaka, BlueMail [, Beedle](https://loginself1.bluemail.me/), Visma [,](https://docs.microsoft.com/azure/active-directory/saas-apps/visma-tutorial) [OneDesk](https://docs.microsoft.com/azure/active-directory/saas-apps/onedesk-tutorial), [foko Retail](https://docs.microsoft.com/azure/active-directory/saas-apps/foko-retail-tutorial), [Qmarkets-idé & innovation Management](https://docs.microsoft.com/azure/active-directory/saas-apps/qmarkets-idea-innovation-management-tutorial), [Beedle](https://teams-web.beedle.co/#/) [Netskope User Authentication](https://docs.microsoft.com/azure/active-directory/saas-apps/netskope-user-authentication-tutorial), [uniFLOW online](https://docs.microsoft.com/azure/active-directory/saas-apps/uniflow-online-tutorial), [Claromentis](https://docs.microsoft.com/azure/active-directory/saas-apps/claromentis-tutorial), [JISC student registrering](https://docs.microsoft.com/azure/active-directory/saas-apps/jisc-student-voter-registration-tutorial), [e4enable](https://portal.e4enable.com/) [MyAryaka](https://docs.microsoft.com/azure/active-directory/saas-apps/myaryaka-tutorial)

Mer information om apparna finns i [SaaS Application Integration with Azure Active Directory](https://aka.ms/appstutorial). Mer information om hur du visar ditt program i Azure AD App-galleriet finns i [lista ditt program i Azure Active Directory program galleriet](https://aka.ms/azureadapprequest).

---

### <a name="new-and-improved-azure-ad-application-gallery"></a>Nytt och förbättrat program Galleri för Azure AD

**Typ:** Ändrad funktion  
**Tjänste kategori:** Företags program  
**Produkt kapacitet:** DEFINITION

Vi har uppdaterat Azure AD-programgalleriet för att göra det enklare för dig att hitta förintegrerade appar som stöder etablering, OpenID Connect och SAML på din Azure Active Directory klient.

Mer information finns i [lägga till ett program i Azure Active Directory-klienten](https://docs.microsoft.com/azure/active-directory/manage-apps/add-application-portal).

---

### <a name="increased-app-role-definition-length-limit-from-120-to-240-characters"></a>Utökad längd gräns för program Rolls definition från 120 till 240 tecken

**Typ:** Ändrad funktion  
**Tjänste kategori:** Företags program  
**Produkt kapacitet:** DEFINITION

Vi har hört kunder om att längd gränsen för definition svärdet för app-roll i vissa appar och tjänster är för kort med 120 tecken. Som svar har vi ökat den maximala längden för roll värdes definitionen till 240 tecken.

Mer information om hur du använder programspecifika roll definitioner finns i [lägga till app-roller i programmet och ta emot dem i token](https://docs.microsoft.com/azure/active-directory/develop/howto-add-app-roles-in-azure-ad-apps).

---
