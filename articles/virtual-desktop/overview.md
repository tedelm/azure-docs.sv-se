---
title: Vad är Windows Virtual Desktop? – Azure
description: En översikt över virtuella Windows-datorer.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: overview
ms.date: 05/07/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: ab1d0318464f6b44e1f46bd30dc76272584fde64
ms.sourcegitcommit: a6d477eb3cb9faebb15ed1bf7334ed0611c72053
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 05/08/2020
ms.locfileid: "82929833"
---
# <a name="what-is-windows-virtual-desktop"></a>Vad är Windows Virtual Desktop? 

Windows Virtual Desktop är en Skriv bords-och app Virtualization-tjänst som körs i molnet.

Det här kan du göra när du kör Windows Virtual Desktop på Azure:

* Konfigurera en Windows 10-distribution med flera sessioner som ger en fullständig Windows 10 med skalbarhet
* Virtualisera Office 365 ProPlus och optimera det för att köras i virtuella scenarier med flera användare
* Tillhandahålla virtuella Windows 7-datorer med kostnads fria utökade säkerhets uppdateringar
* Ta med dina befintliga Fjärrskrivbordstjänster (RDS) och Windows Server-datorer och appar till valfri dator
* Virtualisera både Station ära datorer och appar
* Hantera Windows 10-, Windows Server-och Windows 7-datorer och-appar med en enhetlig hanterings upplevelse

## <a name="introductory-video"></a>Introduktions video

Lär dig om virtuella Windows-datorer, varför det är unikt och vad som är nytt i den här videon:

<br></br><iframe src="https://www.youtube.com/embed/NQFtI3JLtaU" width="640" height="320" allowFullScreen="true" frameBorder="0"></iframe>

Mer information om virtuella Windows-datorer finns i [vår spelnings lista](https://www.youtube.com/watch?v=NQFtI3JLtaU&list=PLXtHYVsvn_b8KAKw44YUpghpD6lg-EHev).

## <a name="key-capabilities"></a>De viktigaste funktionerna

Med Windows Virtual Desktop kan du konfigurera en skalbar och flexibel miljö:

* Skapa en fullständig Virtualization-miljö för skriv bord i din Azure-prenumeration utan att behöva köra några ytterligare Gateway-servrar.
* Publicera så många värdbaserade pooler som du behöver för att hantera dina olika arbets belastningar.
* Ta med din egen bild för produktions arbets belastningar eller test från Azure-galleriet.
* Minska kostnaderna med poolbaserade resurser för flera sessioner. Med den nya kapaciteten för Windows 10 Enterprise multi-session exklusiv till Windows Virtual Desktop och värd för fjärrskrivbordssession (RDSH) i Windows Server kan du minska antalet virtuella datorer och operativ system (OS) samtidigt som du fortfarande tillhandahåller samma resurser för dina användare.
* Ge individuella ägarskap via personliga (beständiga) Skriv bord.

Du kan distribuera och hantera virtuella skriv bord:

* Använd Windows Virtual Desktop PowerShell och REST Interfaces för att konfigurera värd grupper, skapa app-grupper, tilldela användare och publicera resurser.
* Publicera hela Skriv bordet eller enskilda fjärrappar från en enda adresspool, skapa enskilda grupp grupper för olika uppsättningar med användare eller till och med tilldela användare till flera app-grupper för att minska antalet avbildningar.
* När du hanterar din miljö kan du använda inbyggd delegerad åtkomst för att tilldela roller och samla in diagnostik för att förstå olika konfigurations-eller användar fel.
* Använd den nya diagnostik tjänsten för att felsöka fel.
* Hantera bara avbildningen och virtuella datorer, inte infrastrukturen. Du behöver inte personligen hantera de fjärr skrivbords roller som du gör med Fjärrskrivbordstjänster, bara de virtuella datorerna i din Azure-prenumeration.

Du kan också tilldela och ansluta användare till dina virtuella skriv bord:

* När de har tilldelats kan användarna starta en virtuell Windows-klient för att ansluta användare till publicerade Windows-datorer och program. Anslut från valfri enhet via antingen ett inbyggt program på din enhet eller på Windows Virtual Desktop HTML5 webb klient.
* Upprätta en säker anslutning till tjänsten på ett säkert sätt, så att du aldrig behöver lämna några öppna inkommande portar.

## <a name="requirements"></a>Krav

Det finns några saker du behöver för att konfigurera virtuella Windows-datorer och ansluta dina användare till sina Windows-datorer och program.

Vi planerar att lägga till stöd för följande operativ system, så se till att du har [lämpliga licenser](https://azure.microsoft.com/pricing/details/virtual-desktop/) för dina användare baserat på Skriv bordet och appar som du planerar att distribuera:

|Operativsystem|Nödvändig licens|
|---|---|
|Windows 10 Enterprise multi-session eller Windows 10 Enterprise|Microsoft 365 E3, E5, A3, A5, F3, Business Premium<br>Windows E3, E5, A3, A5|
|Windows 7 Enterprise |Microsoft 365 E3, E5, A3, A5, F3, Business Premium<br>Windows E3, E5, A3, A5|
|Windows Server 2012 R2, 2016, 2019|Klient åtkomst licens för fjärr skrivbords tjänster med Software Assurance|

Infrastrukturen behöver följande saker för att stödja Windows Virtual Desktop:

* En [Azure Active Directory](/azure/active-directory/)
* En Windows Server-Active Directory som synkroniseras med Azure Active Directory. Du kan konfigurera detta med något av följande:
  * Azure AD Connect (för Hybrid organisationer)
  * Azure AD Domain Services (för Hybrid-eller moln organisationer)
* En Azure-prenumeration som innehåller ett virtuellt nätverk som antingen innehåller eller är ansluten till Windows Server-Active Directory
  
De virtuella Azure-datorer som du skapar för virtuella Windows-datorer måste vara:

* [Standard domän ansluten](../active-directory-domain-services/active-directory-ds-comparison.md) eller [hybrid AD-ansluten](../active-directory/devices/hybrid-azuread-join-plan.md). Virtuella datorer kan inte vara Azure AD-anslutna.
* Köra en av följande [OS-avbildningar som stöds](#supported-virtual-machine-os-images).

>[!NOTE]
>Om du behöver en Azure-prenumeration kan du [Registrera dig för en kostnads fri utvärderings version av en månad](https://azure.microsoft.com/free/). Om du använder den kostnads fria utvärderings versionen av Azure bör du använda Azure AD Domain Services för att hålla Windows Server-Active Directory synkroniserad med Azure Active Directory.

De virtuella Azure-datorer som du skapar för virtuella Windows-datorer måste ha åtkomst till följande URL: er:

|Adress|Utgående TCP-port|Syfte|Service tag|
|---|---|---|---|
|*. wvd.microsoft.com|443|Tjänst trafik|WindowsVirtualDesktop|
|mrsglobalsteus2prod.blob.core.windows.net|443|Uppdateringar av agent-och SXS-stack|AzureCloud|
|*.core.windows.net|443|Agent trafik|AzureCloud|
|*.servicebus.windows.net|443|Agent trafik|AzureCloud|
|prod.warmpath.msftcloudes.com|443|Agent trafik|AzureCloud|
|catalogartifact.azureedge.net|443|Azure Marketplace|AzureCloud|
|kms.core.windows.net|1688|Windows-aktivering|Internet|
|wvdportalstorageblob.blob.core.windows.net|443|Azure Portal support|AzureCloud|

>[!IMPORTANT]
>Windows Virtual Desktop stöder nu FQDN-taggen. Mer information finns i [använda Azure Firewall för att skydda fönster distributioner av virtuella skriv bord](../firewall/protect-windows-virtual-desktop.md).
>
>Vi rekommenderar att du använder FQDN-taggar eller tjänst Taggar i stället för URL: er för att förhindra tjänst problem. URL: er och taggar i listan motsvarar endast Windows virtuella Skriv bords webbplatser och resurser. De omfattar inte URL: er för andra tjänster som Azure Active Directory.

I följande tabell visas valfria URL: er som dina virtuella Azure-datorer kan ha åtkomst till:

|Adress|Utgående TCP-port|Syfte|Service tag|
|---|---|---|---|
|*.microsoftonline.com|443|Autentisering till MS Online Services|Inga|
|*. events.data.microsoft.com|443|Telemetri-tjänst|Inga|
|www.msftconnecttest.com|443|Identifierar om operativ systemet är anslutet till Internet|Inga|
|*. prod.do.dsp.mp.microsoft.com|443|Windows Update|Inga|
|login.windows.net|443|Logga in på MS Online Services, Office 365|Inga|
|*. sfx.ms|443|Uppdateringar för OneDrive-klientprogramvara|Inga|
|*. digicert.com|443|Återkallnings kontroll av certifikat|Inga|


>[!NOTE]
>Det finns för närvarande ingen lista över IP-adressintervall som du kan vitlista för att tillåta nätverks trafik i det virtuella Windows-skrivbordet. Vi stöder bara vit listning-angivna URL: er just nu.
>
>En lista över Office-relaterade URL: er, inklusive nödvändiga Azure Active Directory-relaterade URL: er, finns i [Office 365-URL: er och IP-adressintervall](/office365/enterprise/urls-and-ip-address-ranges).
>
>Du måste använda jokertecknet (*) för URL: er som involverar tjänst trafiken. Om du inte vill använda * för agent-relaterad trafik så här hittar du URL: erna utan jokertecken:
>
>1. Registrera dina virtuella datorer på Windows-poolen för virtuella skriv bord.
>2. Öppna **logg boken** och navigera till **Windows-loggar** > **Application** > **WVD-agent** och leta efter händelse-ID 3702.
>3. Vitlista de URL: er som du hittar under händelse-ID 3702. URL: erna under händelse-ID 3702 är landsspecifika. Du måste upprepa vit listning-processen med relevanta URL: er för varje region som du vill distribuera dina virtuella datorer i.

Windows Virtual Desktop består av Windows-datorer och appar som du levererar till användare och hanterings lösningen, som är värdbaserad som en tjänst på Azure av Microsoft. Skriv bord och appar kan distribueras på virtuella datorer i valfri Azure-region och hanterings lösningen och data för dessa virtuella datorer finns i USA. Detta kan leda till att data överförs till USA.

För bästa prestanda bör du kontrol lera att nätverket uppfyller följande krav:

* Svars tid för tur och retur från klientens nätverk till den Azure-region där värdbaserade pooler har distribuerats måste vara mindre än 150 ms.
* Nätverks trafiken kan flöda utanför lands-/region gränser när virtuella datorer som är värdar för Station ära datorer och appar ansluter till hanterings tjänsten.
* För att optimera för nätverks prestanda rekommenderar vi att de virtuella datorerna i samordnad i samma Azure-region som hanterings tjänsten.

## <a name="supported-remote-desktop-clients"></a>Fjärr skrivbords klienter som stöds

Följande fjärr skrivbords klienter stöder virtuellt skriv bord i Windows:

* [Windows-skrivbordet](connect-windows-7-and-10.md)
* [Webb](connect-web.md)
* [macOS](connect-macos.md)
* [iOS](connect-ios.md)
* [Android (för hands version)](connect-android.md)

> [!IMPORTANT]
> Windows Virtual Desktop stöder inte RADC-klienten (RemoteApp-och Desktop Connections) eller Anslutning till fjärrskrivbord-klienten (MSTSC).

> [!IMPORTANT]
> Windows Virtual Desktop stöder för närvarande inte fjärr skrivbords klienten från Windows Store. Support för den här klienten kommer att läggas till i en framtida version.

Fjärr skrivbords klienterna måste ha åtkomst till följande URL: er:

|Adress|Utgående TCP-port|Syfte|Klient (er)|
|---|---|---|---|
|*. wvd.microsoft.com|443|Tjänst trafik|Alla|
|*.servicebus.windows.net|443|Felsöka data|Alla|
|go.microsoft.com|443|Microsoft-FWLinks|Alla|
|aka.ms|443|Microsoft URL-kortare|Alla|
|docs.microsoft.com|443|Dokumentation|Alla|
|privacy.microsoft.com|443|Sekretesspolicy|Alla|
|query.prod.cms.rt.microsoft.com|443|Klient uppdateringar|Windows-skrivbordet|

>[!IMPORTANT]
>Att öppna dessa URL: er är viktigt för en tillförlitlig klient upplevelse. Det finns inte stöd för att blockera åtkomst till dessa URL: er och det påverkar service funktionerna. Dessa URL: er motsvarar bara klientens platser och resurser och inkluderar inte URL: er för andra tjänster som Azure Active Directory.

## <a name="supported-virtual-machine-os-images"></a>OS-avbildningar för virtuella datorer som stöds

Windows Virtual Desktop stöder följande x64-operativ system avbildningar:

* Windows 10 Enterprise multi-session, version 1809 eller senare
* Windows 10 Enterprise, version 1809 eller senare
* Windows 7 Enterprise
* Windows Server 2019
* Windows Server 2016
* Windows Server 2012 R2

Windows Virtual Desktop stöder inte x86 (32-bitars), Windows 10 Enterprise N eller Windows 10 Enterprise KN-operativsystem avbildningar. Windows 7 stöder inte heller några VHD-eller VHDX-baserade profil lösningar som finns på hanterade Azure Storage på grund av en begränsning för sektor storlek.

Tillgängliga alternativ för Automation och distribution beror på vilket operativ system och vilken version du väljer, som du ser i följande tabell: 

|Operativsystem|Azures avbildnings Galleri|Manuell distribution av virtuella datorer|Azure Resource Manager mall-integrering|Etablera värdbaserade pooler på Azure Marketplace|
|--------------------------------------|:------:|:------:|:------:|:------:|
|Windows 10 multi-session, version 1903|Ja|Ja|Ja|Ja|
|Windows 10 multi-session, version 1809|Ja|Ja|Inga|Inga|
|Windows 10 Enterprise, version 1903|Ja|Ja|Ja|Ja|
|Windows 10 Enterprise, version 1809|Ja|Ja|Inga|Inga|
|Windows 7 Enterprise|Ja|Ja|Inga|Inga|
|Windows Server 2019|Ja|Ja|Inga|Inga|
|Windows Server 2016|Ja|Ja|Ja|Ja|
|Windows Server 2012 R2|Ja|Ja|Inga|Inga|

## <a name="next-steps"></a>Nästa steg

Om du använder Windows Virtual Desktop hösten 2019-versionen kan du komma igång med vår självstudie i [skapa en klient i Windows Virtual Desktop](./virtual-desktop-fall-2019/tenant-setup-azure-active-directory.md).

Om du använder Windows Virtual Desktop våren 2020-versionen måste du skapa en adresspool i stället. Gå till följande självstudie för att komma igång.

> [!div class="nextstepaction"]
> [Skapa en värdpool med Azure-portalen](create-host-pools-azure-marketplace.md)
