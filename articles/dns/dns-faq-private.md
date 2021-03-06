---
title: Vanliga frågor och svar om Azure Private DNS
description: I den här artikeln lär du dig vanliga frågor om Azure Privat DNS
services: dns
author: rohinkoul
ms.service: dns
ms.topic: article
ms.date: 10/05/2019
ms.author: rohink
ms.openlocfilehash: 4de585a965cfefa6399b0c0929a8f732d0712617
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/28/2020
ms.locfileid: "76939428"
---
# <a name="azure-private-dns-faq"></a>Vanliga frågor och svar om Azure Private DNS

Här är några vanliga frågor om Azures privata DNS.

## <a name="does-azure-dns-support-private-domains"></a>Stöder Azure DNS privata domäner?

Privata domäner stöds med Azure Privat DNS Zones-funktionen. Privat DNS zoner matchas bara från angivna virtuella nätverk. Mer information finns i [översikten](private-dns-overview.md).

Information om andra interna DNS-alternativ i Azure finns i [namn matchning för virtuella datorer och roll instanser](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).

## <a name="will-azure-private-dns-zones-work-across-azure-regions"></a>Kommer Azure Privat DNS zoner att fungera i Azure-regioner?

Ja. Privata zoner stöds för DNS-matchning mellan virtuella nätverk i Azure-regioner. Privata zoner fungerar även utan att uttryckligen peer-koppla de virtuella nätverken. Alla virtuella nätverk måste vara länkade till den privata DNS-zonen.

## <a name="is-connectivity-to-the-internet-from-virtual-networks-required-for-private-zones"></a>Är anslutningen till Internet från virtuella nätverk som krävs för privata zoner?

Nej. Privata zoner fungerar tillsammans med virtuella nätverk. Du kan använda dem för att hantera domäner för virtuella datorer eller andra resurser i och mellan virtuella nätverk. Ingen Internet anslutning krävs för namn matchning.

## <a name="can-the-same-private-zone-be-used-for-several-virtual-networks-for-resolution"></a>Kan samma privata zon användas för flera virtuella nätverk för matchning?

Ja. Du kan länka en privat DNS-zon med tusentals virtuella nätverk. Mer information finns i [Azure DNS gränser](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#azure-dns-limits)

## <a name="can-a-virtual-network-that-belongs-to-a-different-subscription-be-linked-to-a-private-zone"></a>Kan ett virtuellt nätverk som tillhör en annan prenumeration länkas till en privat zon?

Ja. Du måste ha behörigheten Skriv åtgärd för de virtuella nätverken och den privata DNS-zonen. Skriv behörigheten kan beviljas till flera RBAC-roller. Till exempel har den klassiska nätverks deltagaren RBAC-rollen Skriv behörigheter till virtuella nätverk och Privat DNS zoner deltagar rollen har Skriv behörighet för privata DNS-zoner. Mer information om RBAC-roller finns i [rollbaserad åtkomst kontroll](../role-based-access-control/overview.md).

## <a name="will-the-automatically-registered-virtual-machine-dns-records-in-a-private-zone-be-automatically-deleted-when-you-delete-the-virtual-machine"></a>Kommer de automatiskt registrerade DNS-posterna för virtuella datorer i en privat zon automatiskt att tas bort när du tar bort den virtuella datorn?

Ja. Om du tar bort en virtuell dator i ett länkat virtuellt nätverk med automatisk registrering aktive rad tas registrerade poster bort automatiskt.

## <a name="can-an-automatically-registered-virtual-machine-record-in-a-private-zone-from-a-linked-virtual-network-be-deleted-manually"></a>Kan en automatiskt registrerad virtuell dator post i en privat zon från ett länkat virtuellt nätverk tas bort manuellt?

Ja. Du kan skriva över de automatiskt registrerade DNS-posterna med en manuellt skapad DNS-post i zonen. Följande fråga och svar riktar sig till det här avsnittet.

## <a name="what-happens-when-i-try-to-manually-create-a-new-dns-record-into-a-private-zone-that-has-the-same-hostname-as-an-automatically-registered-existing-virtual-machine-in-a-linked-virtual-network"></a>Vad händer när jag försöker skapa en ny DNS-post manuellt i en privat zon som har samma värdnamn som en automatiskt registrerad befintlig virtuell dator i ett länkat virtuellt nätverk?

Du försöker skapa en ny DNS-post manuellt i en privat zon som har samma värdnamn som en befintlig, automatiskt registrerad virtuell dator i ett länkat virtuellt nätverk. När du gör det skriver den nya DNS-posten över den automatiskt registrerade posten för den virtuella datorn. Om du försöker ta bort den här manuellt skapade DNS-posten från zonen igen lyckas borttagningen. Den automatiska registreringen sker igen så länge den virtuella datorn fortfarande finns och har en privat ansluten IP-adress. DNS-posten skapas igen automatiskt i zonen.

## <a name="what-happens-when-we-unlink-a-linked-virtual-network-from-a-private-zone-will-the-automatically-registered-virtual-machine-records-from-the-virtual-network-be-removed-from-the-zone-too"></a>Vad händer när vi tar bort länkar till ett länkat virtuellt nätverk från en privat zon? Kommer de automatiskt registrerade poster för virtuella datorer från det virtuella nätverket att tas bort från zonen?

Ja. Om du vill ta bort länken mellan ett länkat virtuellt nätverk från en privat zon uppdaterar du DNS-zonen för att ta bort den associerade virtuella nätverks länken. I den här processen tas poster för virtuella datorer som registrerats automatiskt bort från zonen.

## <a name="what-happens-when-we-delete-a-linked-virtual-network-thats-linked-to-a-private-zone-do-we-have-to-manually-update-the-private-zone-to-unlink-the-virtual-network-as-a-linked-virtual-network-from-the-zone"></a>Vad händer när vi tar bort ett länkat virtuellt nätverk som är länkat till en privat zon? Måste vi uppdatera den privata zonen manuellt för att ta bort länkar till det virtuella nätverket som ett länkat virtuellt nätverk från zonen?

Nej. När du tar bort ett länkat virtuellt nätverk utan att ta bort länken från en privat zon måste du först ta bort åtgärden och länkarna till DNS-zonen rensas automatiskt.

## <a name="will-dns-resolution-by-using-the-default-fqdn-internalcloudappnet-still-work-even-when-a-private-zone-for-example-privatecontosocom-is-linked-to-a-virtual-network"></a>Kommer DNS-matchning med hjälp av standard-FQDN (internal.cloudapp.net) fortfarande fungera även om en privat zon (till exempel private.contoso.com) är länkad till ett virtuellt nätverk?

Ja. Privata zoner ersätter inte standard Azure-tillhandahållen internal.cloudapp.net-zon. Oavsett om du förlitar dig på den Azure-tillhandahållna internal.cloudapp.net eller på en egen privat zon, använder du FQDN för den zon som du vill matcha mot.

## <a name="will-the-dns-suffix-on-virtual-machines-within-a-linked-virtual-network-be-changed-to-that-of-the-private-zone"></a>Kommer DNS-suffixet på virtuella datorer i ett länkat virtuellt nätverk att ändras till den privata zonen?

Nej. DNS-suffixet på de virtuella datorerna i det länkade virtuella nätverket är kvar som standard Azure-tillhandahållet suffix ("*. internal.cloudapp.net"). Du kan manuellt ändra det här DNS-suffixet på dina virtuella datorer till den privata zonen.
Information om hur du ändrar det här suffixet hittar du i [använda dynamisk DNS för att registrera värdnamn på din egen DNS-Server](https://docs.microsoft.com/azure/virtual-network/virtual-networks-name-resolution-ddns#windows-clients)

## <a name="what-are-the-usage-limits-for-azure-dns-private-zones"></a>Vilka är användnings gränser för Azure DNS privata zoner?

Se [Azure DNS gränser](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#azure-dns-limits) för information om användnings gränserna för Azure DNS privata zoner.

## <a name="why-dont-my-existing-private-dns-zones-show-up-in-new-portal-experience"></a>Varför visas inte mina befintliga privata DNS-zoner i den nya Portal upplevelsen?

Om din befintliga privata DNS-zon har skapats med hjälp av för hands versionen av API måste du migrera zonerna till en ny resurs modell. Privat DNS zoner som skapats med hjälp av för hands versions-API visas inte i den nya Portal upplevelsen. Se nedan för instruktioner om hur du migrerar till en ny resurs modell.

## <a name="how-do-i-migrate-my-existing-private-dns-zones-to-the-new-model"></a>Hur gör jag för att migrera mina befintliga privata DNS-zoner till den nya modellen?

Vi rekommenderar starkt att du migrerar till den nya resurs modellen så snart som möjligt. Äldre resurs modell kommer att stödjas men ytterligare funktioner kommer inte att utvecklas ovanpå den här modellen. I framtiden planerar vi att bli av med den nya resurs modellens fördel. Vägledning om hur du migrerar befintliga privata DNS-zoner till en ny resurs modell finns i[migreringsguiden för Azure DNS privata zoner](private-dns-migration-guide.md).

## <a name="next-steps"></a>Nästa steg

- [Läs mer om Azure Privat DNS](private-dns-overview.md)
