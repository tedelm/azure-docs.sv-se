---
title: Använda privata slutpunkter
titleSuffix: Azure Storage
description: Översikt över privata slut punkter för säker åtkomst till lagrings konton från virtuella nätverk.
services: storage
author: santoshc
ms.service: storage
ms.topic: article
ms.date: 03/12/2020
ms.author: santoshc
ms.reviewer: santoshc
ms.subservice: common
ms.openlocfilehash: c51f2db698f30368c9d4090d3d571fa0c131178a
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/28/2020
ms.locfileid: "79299064"
---
# <a name="use-private-endpoints-for-azure-storage"></a>Använd privata slut punkter för Azure Storage

Du kan använda [privata slut punkter](../../private-link/private-endpoint-overview.md) för dina Azure Storage-konton om du vill tillåta klienter i ett virtuellt nätverk (VNet) att säkert få åtkomst till data via en [privat länk](../../private-link/private-link-overview.md). Den privata slut punkten använder en IP-adress från VNet-adressutrymmet för din lagrings konto tjänst. Nätverks trafik mellan klienterna i VNet och lagrings kontot passerar över VNet och en privat länk i Microsoft stamnät nätverket, vilket eliminerar exponering från det offentliga Internet.

Med hjälp av privata slut punkter för ditt lagrings konto kan du:

- Skydda ditt lagrings konto genom att konfigurera lagrings brand väggen för att blockera alla anslutningar på den offentliga slut punkten för lagrings tjänsten.
- Öka säkerheten för det virtuella nätverket (VNet) genom att göra det möjligt att blockera exfiltrering av data från VNet.
- Anslut säkert till lagrings konton från lokala nätverk som ansluter till VNet med [VPN](../../vpn-gateway/vpn-gateway-about-vpngateways.md) eller [ExpressRoute](../../expressroute/expressroute-locations.md) med privat peering.

## <a name="conceptual-overview"></a>Konceptuell översikt

![Översikt över privata slut punkter för Azure Storage](media/storage-private-endpoints/storage-private-endpoints-overview.jpg)

En privat slut punkt är ett särskilt nätverks gränssnitt för en Azure-tjänst i din [Virtual Network](../../virtual-network/virtual-networks-overview.md) (VNet). När du skapar en privat slut punkt för ditt lagrings konto ger den säker anslutning mellan klienter i ditt VNet och ditt lagrings utrymme. Den privata slut punkten tilldelas en IP-adress från det virtuella nätverkets IP-adressintervall. Anslutningen mellan den privata slut punkten och lagrings tjänsten använder en säker privat länk.

Program i det virtuella nätverket kan ansluta till lagrings tjänsten via den privata slut punkten sömlöst **med samma anslutnings strängar och auktoriseringsbeslut som de skulle använda i övrigt**. Privata slut punkter kan användas med alla protokoll som stöds av lagrings kontot, inklusive REST och SMB.

Privata slut punkter kan skapas i undernät som använder [tjänst slut punkter](../../virtual-network/virtual-network-service-endpoints-overview.md). Klienter i ett undernät kan därmed ansluta till ett lagrings konto med hjälp av privat slut punkt, samtidigt som tjänstens slut punkter används för att komma åt andra.

När du skapar en privat slutpunkt för en lagringstjänst i ditt VNet skickas en begäran om godkännande till lagringskontots ägare. Om användaren som begär att den privata slut punkten ska skapas även är ägare till lagrings kontot, godkänns den här medgivande förfrågningen automatiskt.

Lagrings kontots ägare kan hantera medgivande förfrågningar och privata slut punkter via fliken "*privata slut punkter*" för lagrings kontot i [Azure Portal](https://portal.azure.com).

> [!TIP]
> Om du vill begränsa åtkomsten till ditt lagrings konto via enbart den privata slut punkten konfigurerar du lagrings brand väggen för att neka eller kontrol lera åtkomst via den offentliga slut punkten.

Du kan skydda ditt lagrings konto så att det bara accepterar anslutningar från ditt VNet genom att [Konfigurera lagrings brand väggen](storage-network-security.md#change-the-default-network-access-rule) för att neka åtkomst via dess offentliga slut punkt som standard. Du behöver inte ha någon brand Väggs regel för att tillåta trafik från ett VNet som har en privat slut punkt, eftersom lagrings brand väggen bara kontrollerar åtkomst via den offentliga slut punkten. Privata slut punkter förlitar sig i stället på godkännande flödet för att ge undernät åtkomst till lagrings tjänsten.

### <a name="private-endpoints-for-azure-storage"></a>Privata slut punkter för Azure Storage

När du skapar den privata slut punkten måste du ange det lagrings konto och den lagrings tjänst som den ansluter till. Du behöver en separat privat slut punkt för varje lagrings tjänst i ett lagrings konto som du behöver komma åt, nämligen [blobbar](../blobs/storage-blobs-overview.md), [data Lake Storage Gen2](../blobs/data-lake-storage-introduction.md), [filer](../files/storage-files-introduction.md), [köer](../queues/storage-queues-introduction.md), [tabeller](../tables/table-storage-overview.md)eller [statiska webbplatser](../blobs/storage-blob-static-website.md).

> [!TIP]
> Skapa en separat privat slut punkt för den sekundära instansen av lagrings tjänsten för bättre Läs prestanda på RA-GRS-konton.

Om du vill ha Läs behörighet till den sekundära regionen med ett lagrings konto som kon figurer ATS för Geo-redundant lagring, behöver du separata privata slut punkter för både den primära och sekundära tjänstens instanser av tjänsten. Du behöver inte skapa en privat slut punkt för den sekundära instansen för **redundansväxling**. Den privata slut punkten ansluts automatiskt till den nya primära instansen efter redundansväxlingen. Mer information om alternativ för redundans finns [Azure Storage redundans](storage-redundancy.md).

Mer detaljerad information om hur du skapar en privat slut punkt för ditt lagrings konto finns i följande artiklar:

- [Anslut privat till ett lagrings konto från lagrings konto upplevelsen i Azure Portal](../../private-link/create-private-endpoint-storage-portal.md)
- [Skapa en privat slut punkt med hjälp av det privata länk centret i Azure Portal](../../private-link/create-private-endpoint-portal.md)
- [Skapa en privat slut punkt med Azure CLI](../../private-link/create-private-endpoint-cli.md)
- [Skapa en privat slut punkt med hjälp av Azure PowerShell](../../private-link/create-private-endpoint-powershell.md)

### <a name="connecting-to-private-endpoints"></a>Ansluter till privata slut punkter

Klienter i ett VNet som använder den privata slut punkten bör använda samma anslutnings sträng för lagrings kontot, som klienter som ansluter till den offentliga slut punkten. Vi förlitar sig på DNS-matchning för att automatiskt dirigera anslutningarna från VNet till lagrings kontot via en privat länk.

> [!IMPORTANT]
> Använd samma anslutnings sträng för att ansluta till lagrings kontot med privata slut punkter, som du annars skulle använda. Anslut inte till lagrings kontot med hjälp av URL: en för*privatelink*-underdomänen.

Vi skapar en [privat DNS-zon](../../dns/private-dns-overview.md) som är kopplad till det virtuella nätverket med nödvändiga uppdateringar för privata slut punkter som standard. Men om du använder en egen DNS-server kan du behöva göra ytterligare ändringar i DNS-konfigurationen. Avsnittet om [DNS-ändringar](#dns-changes-for-private-endpoints) nedan beskriver de uppdateringar som krävs för privata slut punkter.

## <a name="dns-changes-for-private-endpoints"></a>DNS-ändringar för privata slut punkter

När du skapar en privat slut punkt uppdateras DNS CNAME-resursposten för lagrings kontot till ett alias i en under domän med prefixet "*privatelink*". Som standard skapar vi också en [privat DNS-zon](../../dns/private-dns-overview.md)som motsvarar under domänen "*privatelink*" med DNS a-resursposter för de privata slut punkterna.

När du löser lagrings slut punktens URL från utanför det virtuella nätverket med den privata slut punkten matchas den offentliga slut punkten för lagrings tjänsten. Vid matchning från det VNet som är värd för den privata slut punkten matchas slut punktens URL-adress till den privata slut punktens IP-adress.

För det illustrerat exemplet ovan blir DNS-resursposterna för lagrings kontot StorageAccountA, när de löses från utanför det virtuella nätverket som är värd för den privata slut punkten,:

| Name                                                  | Typ  | Värde                                                 |
| :---------------------------------------------------- | :---: | :---------------------------------------------------- |
| ``StorageAccountA.blob.core.windows.net``             | CNAME | ``StorageAccountA.privatelink.blob.core.windows.net`` |
| ``StorageAccountA.privatelink.blob.core.windows.net`` | CNAME | \<offentlig slut punkt för lagrings tjänst\>                   |
| \<offentlig slut punkt för lagrings tjänst\>                   | A     | \<offentlig IP-adress för lagrings tjänst\>                 |

Som tidigare nämnts kan du neka eller kontrol lera åtkomsten för klienter utanför VNet via den offentliga slut punkten med hjälp av lagrings brand väggen.

DNS-resursposterna för StorageAccountA, när de löses av en klient i det VNet som är värd för den privata slut punkten, blir:

| Name                                                  | Typ  | Värde                                                 |
| :---------------------------------------------------- | :---: | :---------------------------------------------------- |
| ``StorageAccountA.blob.core.windows.net``             | CNAME | ``StorageAccountA.privatelink.blob.core.windows.net`` |
| ``StorageAccountA.privatelink.blob.core.windows.net`` | A     | 10.1.1.5                                              |

Den här metoden ger åtkomst till lagrings kontot **med samma anslutnings sträng** för klienter på det virtuella nätverk som är värd för privata slut punkter, samt klienter utanför VNet.

Om du använder en anpassad DNS-server i ditt nätverk måste klienterna kunna matcha FQDN för lagrings kontots slut punkt till den privata slut punktens IP-adress. Du bör konfigurera DNS-servern för att delegera din privata länk under domän till den privata DNS-zonen för det virtuella nätverket eller konfigurera A-posterna för "*StorageAccountA.privatelink.blob.Core.Windows.net*" med den privata slut punkten IP-adress.

> [!TIP]
> När du använder en anpassad eller lokal DNS-server bör du konfigurera DNS-servern för att matcha lagrings kontots namn i under domänen "privatelink" till IP-adressen för den privata slut punkten. Du kan göra detta genom att delegera privatelink-underdomänen till det virtuella nätverkets privata DNS-zon eller konfigurera DNS-zonen på DNS-servern och lägga till DNS-posterna.

De rekommenderade DNS-zonnamn för privata slut punkter för lagrings tjänster är:

| Lagrings tjänst        | Zonnamn                            |
| :--------------------- | :----------------------------------- |
| Blob Service           | `privatelink.blob.core.windows.net`  |
| Data Lake Storage Gen2 | `privatelink.dfs.core.windows.net`   |
| Fil tjänst           | `privatelink.file.core.windows.net`  |
| Kötjänst          | `privatelink.queue.core.windows.net` |
| Table service          | `privatelink.table.core.windows.net` |
| Statiska webbplatser        | `privatelink.web.core.windows.net`   |

Mer information om hur du konfigurerar en egen DNS-server för att stödja privata slut punkter finns i följande artiklar:

- [Namnmatchning för resurser i virtuella nätverk i Azure](/azure/virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances#name-resolution-that-uses-your-own-dns-server)
- [DNS-konfiguration för privata slut punkter](/azure/private-link/private-endpoint-overview#dns-configuration)

## <a name="pricing"></a>Prissättning

Pris information finns i [priser för privata Azure-länkar](https://azure.microsoft.com/pricing/details/private-link).

## <a name="known-issues"></a>Kända problem

Tänk på följande kända problem med privata slut punkter för Azure Storage.

### <a name="copy-blob-support"></a>Kopiera BLOB-stöd

Om lagrings kontot skyddas av en brand vägg och kontot nås via privata slut punkter, kan det kontot inte fungera som källa för en [kopierings-BLOB](/rest/api/storageservices/copy-blob) -åtgärd.

### <a name="storage-access-constraints-for-clients-in-vnets-with-private-endpoints"></a>Begränsningar för lagrings åtkomst för klienter i virtuella nätverk med privata slut punkter

Klienter i virtuella nätverk med befintliga privata slut punkter möter begränsningar vid åtkomst till andra lagrings konton som har privata slut punkter. Anta till exempel att ett VNet N1 har en privat slut punkt för ett lagrings konto a1 för Blob Storage. Om lagrings konto a2 har en privat slut punkt i en VNet-N2 för Blob Storage, måste klienter i VNet N1 också komma åt Blob Storage i konto a2 med en privat slut punkt. Om lagrings konto a2 inte har några privata slut punkter för Blob Storage, kan klienter i VNet N1 komma åt Blob Storage i det kontot utan en privat slut punkt.

Den här begränsningen beror på DNS-ändringar som gjorts när konto a2 skapar en privat slut punkt.

### <a name="network-security-group-rules-for-subnets-with-private-endpoints"></a>Regler för nätverkssäkerhetsgrupper för undernät med privata slutpunkter

För närvarande kan du inte konfigurera regler för [nätverks säkerhets grupper](../../virtual-network/security-overview.md) (NSG) och användardefinierade vägar för privata slut punkter. NSG-regler som tillämpas på det undernät som är värd för den privata slut punkten tillämpas på den privata slut punkten. En begränsad lösning för det här problemet är att implementera åtkomst regler för privata slut punkter på käll under näten, även om den här metoden kan kräva en högre hanterings kostnad.

## <a name="next-steps"></a>Nästa steg

- [Konfigurera Azure Storage brand väggar och virtuella nätverk](storage-network-security.md)
- [Säkerhets rekommendationer för Blob Storage](../blobs/security-recommendations.md)
