---
title: Felsök anslutning för haveri beredskap i Azure till Azure med Azure Site Recovery
description: Felsök anslutnings problem i Azure VM haveri beredskap
author: sideeksh
manager: rochakm
ms.topic: how-to
ms.date: 04/06/2020
ms.openlocfilehash: d2cc4133e52e7cab812413d23948da6ac2660e77
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/28/2020
ms.locfileid: "80884876"
---
# <a name="troubleshoot-azure-to-azure-vm-network-connectivity-issues"></a>Felsök problem med Azure-till-Azure VM-nätverksanslutningar

I den här artikeln beskrivs vanliga problem som rör nätverks anslutning när du replikerar och återställer virtuella Azure-datorer från en region till en annan region. Mer information om nätverks krav finns i [anslutnings kraven för replikering av virtuella Azure-datorer](azure-to-azure-about-networking.md).

För att Site Recovery replikering ska fungera krävs utgående anslutning till vissa URL-adresser eller IP-intervall från den virtuella datorn. Om den virtuella datorn ligger bakom en brand vägg eller använder regler för nätverks säkerhets grupper (NSG) för att kontrol lera utgående anslutningar kan du stöta på något av dessa problem.

| URL | Information |
|---|---|
| `*.blob.core.windows.net` | Krävs så att data kan skrivas till cache-lagrings kontot i käll regionen från den virtuella datorn. Om du känner till alla cache-lagrings konton för dina virtuella datorer kan du använda en lista över tillåtna för de angivna URL: erna för lagrings kontot. Till exempel `cache1.blob.core.windows.net` och `cache2.blob.core.windows.net` i stället för `*.blob.core.windows.net`. |
| `login.microsoftonline.com` | Krävs för auktorisering och autentisering till Site Recovery tjänst-URL: er. |
| `*.hypervrecoverymanager.windowsazure.com` | Krävs så att kommunikationen mellan Site Recoverys tjänsten kan ske från den virtuella datorn. Du kan använda motsvarande _Site Recovery-IP_ om brand Väggs-proxyn har stöd för IP-adresser. |
| `*.servicebus.windows.net` | Krävs så att Site Recovery övervakning och diagnostikdata kan skrivas från den virtuella datorn. Du kan använda motsvarande _Site Recovery övervakning av IP_ om din brand vägg stöder IP-adresser. |

## <a name="outbound-connectivity-for-site-recovery-urls-or-ip-ranges-error-code-151037-or-151072"></a>Utgående anslutning för Site Recovery-URL: er eller IP-intervall (felkod 151037 eller 151072)

### <a name="issue-1-failed-to-register-azure-virtual-machine-with-site-recovery-151195"></a>Problem 1: det gick inte att registrera den virtuella Azure-datorn med Site Recovery (151195)

#### <a name="possible-cause"></a>Möjlig orsak

Det går inte att upprätta en anslutning till Site Recovery slut punkter på grund av ett matchnings problem i Domain Name System (DNS). Det här problemet är vanligare när du har redundansväxlats på den virtuella datorn men det går inte att komma åt DNS-servern från katastrof återställnings området (DR).

#### <a name="resolution"></a>Lösning

Om du använder anpassad DNS måste du kontrol lera att DNS-servern är tillgänglig från Disaster Recovery-regionen.

Kontrol lera om den virtuella datorn använder en anpassad DNS-inställning:

1. Öppna **virtuella datorer** och välj den virtuella datorn.
1. Navigera till **inställningarna** för virtuella datorer och välj **nätverk**.
1. I **virtuellt nätverk/undernät**väljer du länken för att öppna det virtuella nätverkets resurs sida.
1. Gå till **Inställningar** och välj **DNS-servrar**.

Försök att komma åt DNS-servern från den virtuella datorn. Om DNS-servern inte är tillgänglig kan du göra den tillgänglig genom att antingen redundansväxla DNS-servern eller skapa en plats mellan DR-nätverket och DNS.

  :::image type="content" source="./media/azure-to-azure-troubleshoot-errors/custom_dns.png" alt-text="com-fel":::

### <a name="issue-2-site-recovery-configuration-failed-151196"></a>Problem 2: Site Recovery konfiguration misslyckades (151196)

> [!NOTE]
> Om de virtuella datorerna finns **bakom en intern belastningsutjämnare som standard har** den inte åtkomst till Office 365-IP-adresser som `login.microsoftonline.com`. Ändra den till en **grundläggande** typ av intern belastningsutjämnare eller skapa utgående åtkomst som anges i artikeln [Konfigurera belastnings utjämning och utgående regler i standard load BALANCER med Azure CLI](/azure/load-balancer/configure-load-balancer-outbound-cli).

#### <a name="possible-cause"></a>Möjlig orsak

Det går inte att upprätta en anslutning till Office 365-autentisering och Identity IP4-slutpunkter.

#### <a name="resolution"></a>Lösning

- Azure Site Recovery kräver åtkomst till Office 365 IP-intervall för autentisering.
- Om du använder Azure nätverks säkerhets grupp (NSG) regler/brand Väggs-proxy för att kontrol lera utgående nätverks anslutning på den virtuella datorn, så se till att du tillåter kommunikation till IP-intervallen för Office 365. Skapa en [Azure Active Directory (Azure AD) service tag-](/azure/virtual-network/security-overview#service-tags) NSG regel som ger åtkomst till alla IP-adresser som motsvarar Azure AD.
- Om nya adresser läggs till i Azure AD i framtiden måste du skapa nya NSG-regler.

### <a name="example-nsg-configuration"></a>Exempel på NSG-konfiguration

Det här exemplet visar hur du konfigurerar NSG-regler för en virtuell dator att replikera.

- Om du använder NSG-regler för att kontrol lera utgående anslutningar använder du **Tillåt https utgående** regler till port 443 för alla IP-adressintervall som krävs.
- Exemplet förutsätter att käll platsen för den virtuella datorn är **östra USA** och att mål platsen är **Central USA**.

#### <a name="nsg-rules---east-us"></a>NSG-regler – USA, östra

1. Skapa en HTTPS utgående säkerhets regel för NSG så som visas på följande skärm bild. I det här exemplet används **destinations tjänst tag gen**: _Storage. öster_ och **mål Port intervall**: _443_.

     :::image type="content" source="./media/azure-to-azure-about-networking/storage-tag.png" alt-text="lagrings tag":::

1. Skapa en HTTPS utgående säkerhets regel för NSG så som visas på följande skärm bild. I det här exemplet används **destinations tjänst tag gen**: _AzureActiveDirectory_ -och **mål Port intervall**: _443_.

     :::image type="content" source="./media/azure-to-azure-about-networking/aad-tag.png" alt-text="AAD-tagg":::

1. Skapa HTTPS-port 443 utgående regler för de Site Recovery IP-adresser som motsvarar mål platsen:

   | Plats | Site Recovery IP-adress | Site Recovery övervakning av IP-adress |
   | --- | --- | --- |
   | USA, centrala | 40.69.144.231 | 52.165.34.144 |

#### <a name="nsg-rules---central-us"></a>NSG-regler – centrala USA

I det här exemplet krävs dessa NSG-regler för att replikeringen ska kunna aktive ras från mål regionen till käll regionen efter redundans:

1. Skapa en HTTPS utgående säkerhets regel för _Storage. centralal_:

   - **Mål tjänst tag gen**: _Storage. centraliserat_
   - **Mål Port intervall**: _443_

1. Skapa en HTTPS utgående säkerhets regel för _AzureActiveDirectory_.

   - **Mål tjänst tag gen**: _AzureActiveDirectory_
   - **Mål Port intervall**: _443_

1. Skapa HTTPS-port 443 utgående regler för de Site Recovery IP-adresser som motsvarar käll platsen:

   | Plats | Site Recovery IP-adress | Site Recovery övervakning av IP-adress |
   | --- | --- | --- |
   | USA, östra | 13.82.88.226 | 104.45.147.24 |

### <a name="issue-3-site-recovery-configuration-failed-151197"></a>Problem 3: Site Recovery konfiguration misslyckades (151197)

#### <a name="possible-cause"></a>Möjlig orsak

Det går inte att upprätta en anslutning till Azure Site Recovery tjänstens slut punkter.

#### <a name="resolution"></a>Lösning

Azure Site Recovery krävde åtkomst till [IP-intervall för Site Recovery](azure-to-azure-about-networking.md#outbound-connectivity-using-service-tags) beroende på region. Kontrol lera att de nödvändiga IP-intervallen är tillgängliga från den virtuella datorn.

### <a name="issue-4-azure-to-azure-replication-failed-when-the-network-traffic-goes-through-on-premises-proxy-server-151072"></a>Problem 4: Azure-till-Azure-replikeringen misslyckades när nätverks trafiken går via en lokal proxyserver (151072)

#### <a name="possible-cause"></a>Möjlig orsak

Inställningarna för den anpassade proxyn är ogiltiga och Azure Site Recovery mobilitets tjänst agenten identifierade inte proxyinställningarna automatiskt från Internet Explorer (IE).

#### <a name="resolution"></a>Lösning

1. Mobilitets tjänst agenten identifierar proxyinställningarna från IE i Windows och `/etc/environment` på Linux.
1. Om du föredrar att bara ange proxy för Azure Site Recovery mobilitets tjänst kan du ange proxyinformation i _ProxyInfo. conf_ på:

   - **Linux**:`/usr/local/InMage/config/`
   - **Windows**:`C:\ProgramData\Microsoft Azure Site Recovery\Config`

1. _ProxyInfo. conf_ ska ha proxyinställningarna i följande _ini_ -format:

   ```plaintext
   [proxy]
   Address=http://1.2.3.4
   Port=567
   ```

> [!NOTE]
> Azure Site Recovery Mobility Service Agent stöder bara **oautentiserade proxyservrar**.

### <a name="fix-the-problem"></a>Åtgärda problemet

Om du vill tillåta [nödvändiga URL: er](azure-to-azure-about-networking.md#outbound-connectivity-for-urls) eller de [IP-adressintervall som krävs](azure-to-azure-about-networking.md#outbound-connectivity-using-service-tags)följer du stegen i [dokumentet om nätverks vägledning](site-recovery-azure-to-azure-networking-guidance.md).

## <a name="next-steps"></a>Nästa steg

[Replikera virtuella Azure-datorer till en annan Azure-region](azure-to-azure-how-to-enable-replication.md)
