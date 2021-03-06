---
title: Publicera en rot-eller Apex-domän till en befintlig front dörr – Azure Portal
description: Lär dig att publicera en rot-eller Apex-domän till en befintlig front dörr med hjälp av Azure Portal.
services: front-door
author: sharad4u
ms.service: frontdoor
ms.topic: article
ms.date: 5/21/2019
ms.author: sharadag
ms.openlocfilehash: 4b74338f22a82d76ef13126ee0862b841bd89a99
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/28/2020
ms.locfileid: "80878892"
---
# <a name="onboard-a-root-or-apex-domain-on-your-front-door"></a>Publicera en rot-eller Apex-domän på din front dörr
Azures front dörr använder CNAME-poster för att verifiera domän ägarskap för onboarding av anpassade domäner. Dessutom exponeras inte klient delens IP-adress som är kopplad till din profil för klient delen och du kan därför inte mappa din Apex-domän till en IP-adress, om avsikten är att publicera den till Azures front dörr.

DNS-protokollet förhindrar att CNAME-poster tilldelas i zonens Apex. Till exempel om din domän är `contoso.com`. Du kan skapa CNAME-poster `somelabel.contoso.com`för; men du kan inte skapa CNAME `contoso.com` för sig själv. Den här begränsningen utgör ett problem för program ägare som har belastningsutjämnade program bakom Azures front dörr. Eftersom du måste skapa en CNAME-post när du använder en profil för en frontend-dörr är det inte möjligt att peka på profilen för den främre dörren från zonens Apex.

Det här problemet kan lösas med hjälp av Ali Aset-poster på Azure DNS. Till skillnad från CNAME-poster skapas Alian slut poster i zonens Apex och program ägare kan använda den för att peka sin zon spetsig-post till en profil för en front dörr som har offentliga slut punkter. Program ägare pekar på samma profil för front dörren som används för alla andra domäner i DNS-zonen. Till exempel `contoso.com` och `www.contoso.com` kan peka på samma profil för front dörren. 

Att mappa din Apex-eller root-domän till din profil för front dörren kräver i princip CNAME-förenkling eller DNS-jaga, vilket är en mekanism där i DNS-providern rekursivt löser CNAME-posten tills den har en IP-adress. Den här funktionen stöds av Azure DNS för slut punkter i front dörren. 

> [!NOTE]
> Det finns andra DNS-leverantörer som stöder CNAME-förenkling eller DNS-jaga, men Azures front dörr rekommenderar att du använder Azure DNS för sina kunder för att vara värd för sina domäner.

Du kan använda Azure Portal för att publicera en Apex-domän på din front dörr och Aktivera HTTPS på den genom att associera den med ett certifikat för TLS-avslutning. Apex-domäner kallas även rot-eller blott-domäner.

I den här artikeln kan du se hur du:

> [!div class="checklist"]
> * Skapa en aliasresurspost som pekar på din profil för din front dörr
> * Lägg till rot domänen i front dörren
> * Konfigurera HTTPS på rot domänen

> [!NOTE]
> I den här självstudien krävs att du redan har skapat en profil för en front dörr. Se andra själv studie kurser som [snabb start: skapa en frontend](./quickstart-create-front-door.md) -dörr eller [skapa en frontend-dörr med HTTP till https-omdirigering](./front-door-how-to-redirect-https.md) för att komma igång.

## <a name="create-an-alias-record-for-zone-apex"></a>Skapa en aliasresurspost för Zone Apex

1. Öppna **Azure DNS** konfiguration för den domän som ska registreras.
2. Skapa eller redigera posten för Zone Apex.
3. Välj post **typen** som _en_ post och välj sedan _Ja_ för **post uppsättning för alias**. **Aliasuppsättningen** måste anges till _Azure Resource_.
4. Välj den Azure-prenumeration där din profil för din klient organisation finns och välj sedan den främre dörren från List rutan **Azure-resurs** .
5. Klicka på **OK** för att skicka ändringarna.

    ![Aliasresurspost för Zone Apex](./media/front-door-apex-domain/front-door-apex-alias-record.png)

6. Steget ovan skapar en zon Apex-post som pekar på din frontend-resurs och även en CNAME-Postmappning "afdverify" (exempel `afdverify.contosonews.com`-) `afdverify.<name>.azurefd.net` som ska användas för att registrera domänen på din profil för din front dörr.

## <a name="onboard-the-custom-domain-on-your-front-door"></a>Publicera den anpassade domänen på din front dörr

1. På fliken front dörr designer klickar du på ikonen "+" i avsnittet klient dels värdar för att lägga till en ny anpassad domän.
2. Ange rot-eller Apex-domän namnet i fältet namn på anpassad värd, `contosonews.com`exempel.
3. När CNAME-mappningen från domänen till din front dörr har verifierats klickar du på **Lägg till** för att lägga till den anpassade domänen.
4. Klicka på **Spara** för att skicka ändringarna.

![Meny för anpassad domän](./media/front-door-apex-domain/front-door-onboard-apex-domain.png)

## <a name="enable-https-on-your-custom-domain"></a>Aktivera HTTPS på din anpassade domän

1. Klicka på den anpassade domänen som lades till och under avsnittet **anpassad https för domän**ändrar du status till **aktive rad**.
2. Välj **certifikat hanterings typ** att _"Använd mitt eget certifikat"_.

> [!WARNING]
> Hanterings typen för hanterade certifikat från Front dörren stöds för närvarande inte för spets-eller rot domäner. Det enda alternativet som är tillgängligt för att aktivera HTTPS på en Apex eller rotdomän för front dörr använder ditt eget anpassade TLS/SSL-certifikat på Azure Key Vault.

3. Se till att du har konfigurerat rätt behörigheter för front dörren för att få åtkomst till ditt nyckel valv som anges i användar gränssnittet, innan du fortsätter till nästa steg.
4. Välj ett **Key Vault konto** från den aktuella prenumerationen och välj sedan rätt **hemlighet** och **hemlig version** som ska mappas till rätt certifikat.
5. Klicka på **Uppdatera** för att spara valet och klicka sedan på **Spara**.
6. Klicka på **Uppdatera** efter några minuter och klicka sedan på den anpassade domänen igen för att se förloppet för certifikat etableringen. 

> [!WARNING]
> Se till att du har skapat lämpliga routningsregler för din Apex-domän eller lagt till domänen i befintliga regler för routning.

## <a name="next-steps"></a>Nästa steg

- Läs hur du [skapar en Front Door](quickstart-create-front-door.md).
- Läs [hur Front Door fungerar](front-door-routing-architecture.md).