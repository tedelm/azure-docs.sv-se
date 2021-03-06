---
title: Stöd för Hyper-V-migrering i Azure Migrate
description: Läs mer om stöd för Hyper-V-migrering med Azure Migrate.
ms.topic: conceptual
ms.date: 04/15/2020
ms.openlocfilehash: 8ec0b72cac75518ac938faa202b28d055409e8dc
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/28/2020
ms.locfileid: "81538196"
---
# <a name="support-matrix-for-hyper-v-migration"></a>Support mat ris för Hyper-V-migrering

I den här artikeln sammanfattas support inställningar och begränsningar för migrering av virtuella Hyper-V-datorer med [Azure Migrate: Server-migrering](migrate-services-overview.md#azure-migrate-server-migration-tool) . Om du vill ha information om hur du bedömer virtuella Hyper-V-datorer för migrering till Azure läser du avsnittet om [utvärderings support](migrate-support-matrix-hyper-v.md).

## <a name="migration-limitations"></a>Migreringsbegränsningar

Du kan välja upp till 10 virtuella datorer på en gång för replikering. Om du vill migrera fler datorer replikerar du i grupper om 10.


## <a name="hyper-v-hosts"></a>Hyper-V-värdar

| **Support**                | **Information**               
| :-------------------       | :------------------- |
| **Distribution**       | Hyper-V-värden kan vara fristående eller distribuerad i ett kluster. <br/>Azure Migrate Replication-programvara (Hyper-V-Replikeringsprovider) är installerad på Hyper-V-värdarna.|
| **Åtkomst**           | Du behöver administratörs behörighet på Hyper-V-värden. |
| **Värd operativ system** | Windows Server 2019, Windows Server 2016 eller Windows Server 2012 R2. |
| **Port åtkomst** |  Utgående anslutningar på HTTPS-port 443 för att skicka data för VM-replikering.

### <a name="url-access-public-cloud"></a>URL-åtkomst (offentligt moln)

Providerns program vara på Hyper-V-värdarna måste ha åtkomst till dessa URL: er.

**ADRESSER** | **Information**
--- | ---
login.microsoftonline.com | Åtkomst kontroll och identitets hantering med hjälp av Active Directory.
backup.windowsazure.com | Överföring och samordning av replikeringsdata.
*.hypervrecoverymanager.windowsazure.com | Används för migrering.
*.blob.core.windows.net | Ladda upp data till lagrings konton. 
dc.services.visualstudio.com | Ladda upp program loggar som används för intern övervakning.
time.windows.com | Verifierar tidssynkronisering mellan system och global tid.

### <a name="url-access-azure-government"></a>URL-åtkomst (Azure Government)

Providerns program vara på Hyper-V-värdarna måste ha åtkomst till dessa URL: er.

**ADRESSER** | **Information**
--- | ---
login.microsoftonline.us | Åtkomst kontroll och identitets hantering med hjälp av Active Directory.
backup.windowsazure.us | Överföring och samordning av replikeringsdata.
*. hypervrecoverymanager.windowsazure.us | Används för migrering.
*. blob.core.usgovcloudapi.net | Ladda upp data till lagrings konton.
dc.services.visualstudio.com | Ladda upp program loggar som används för intern övervakning.
time.nist.gov | Verifierar tidssynkronisering mellan system och global tid.


## <a name="hyper-v-vms"></a>Hyper-V:s virtuella datorer

| **Support**                  | **Information**               
| :----------------------------- | :------------------- |
| **Operativsystem** | Alla [Windows](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines) -och [Linux](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros) -operativsystem som stöds av Azure. |
| **Nödvändiga ändringar för Azure** | Vissa virtuella datorer kan kräva ändringar så att de kan köras i Azure. Gör justeringar manuellt innan migreringen. Relevanta artiklar innehåller instruktioner om hur du gör detta. |
| **Linux-start**                 | Om/boot finns på en dedikerad partition bör den finnas på OS-disken och inte spridas över flera diskar.<br/> Om/Boot är en del av rot-partitionen (/) bör partitionen/-partitionen finnas på OS-disken och inte omfatta andra diskar. |
| **UEFI-start**                  | Den migrerade virtuella datorn i Azure kommer automatiskt att konverteras till en virtuell dator med BIOS-start. Den virtuella datorn ska endast köra Windows Server 2012 och senare. OS-disken bör ha upp till fem partitioner eller färre och storleken på OS-disken måste vara mindre än 300 GB.
  |
| **Diskstorlek**                  | 2 TB för OS-disken, 4 TB för data diskar.
| **Disk nummer** | Högst 16 diskar per virtuell dator.
| **Krypterade diskar/volymer**    | Stöds inte för migrering. |
| **RDM/passthrough-diskar**      | Stöds inte för migrering. |
| **Delad disk** | Virtuella datorer som använder delade diskar stöds inte för migrering.
| **NFS**                        | NFS-volymer som monterats som volymer på de virtuella datorerna replikeras inte. |
| **-**                      | Virtuella datorer med iSCSI-mål stöds inte för migrering.
| **Mål disk**                | Du kan bara migrera till virtuella Azure-datorer med Managed disks. |
| **IPv6** | Stöds inte.
| **NIC-teamning** | Stöds inte.
| **Azure Site Recovery** | Du kan inte replikera med Azure Migrate Server-migrering om den virtuella datorn är aktive rad för replikering med Azure Site Recovery.
| **Portar** | Utgående anslutningar på HTTPS-port 443 för att skicka data för VM-replikering.

## <a name="azure-vm-requirements"></a>Virtuella Azure VMware-datorer

Alla lokala virtuella datorer som replikeras till Azure måste uppfylla de krav för virtuella Azure-datorer som sammanfattas i den här tabellen.

**Komponent** | **Krav** | **Information**
--- | --- | ---
Storlek på operativsystemdisk | Upp till 2 048 GB. | Kontrollen Miss lyckas om den inte stöds.
Antal operativsystemdiskar | 1 | Kontrollen Miss lyckas om den inte stöds.
Antal datadiskar | 16 eller mindre. | Kontrollen Miss lyckas om den inte stöds.
Data disk storlek | Upp till 4 095 GB | Kontrollen Miss lyckas om den inte stöds.
Nätverkskort | Flera nätverkskort stöds. |
Delad VHD | Stöds inte. | Kontrollen Miss lyckas om den inte stöds.
FC-disk | Stöds inte. | Kontrollen Miss lyckas om den inte stöds.
BitLocker | Stöds inte. | BitLocker måste inaktive ras innan du aktiverar replikering för en dator.
VM-namn | Mellan 1 och 63 tecken.<br/> Begränsat till bokstäver, siffror och bindestreck.<br/><br/> Dator namnet måste börja och sluta med en bokstav eller en siffra. |  Uppdatera värdet i dator egenskaperna i Site Recovery.
Anslut efter migreringen – Windows | Ansluta till virtuella Azure-datorer som kör Windows efter migrering:<br/> -Innan migreringen aktiverar RDP på den lokala virtuella datorn. Kontrollera att TCP- och UDP-regler har lagts till för den **offentliga** profilen och att RDP tillåts i **Windows-brandväggen** > **Tillåtna appar** för alla profiler.<br/> För plats-till-plats-VPN-åtkomst aktiverar du RDP och tillåter RDP i **Windows-brandväggen** -> **tillåtna appar och funktioner** för **domän nätverk och privata** nätverk. Dessutom kontrollerar du att operativ systemets SAN-princip är inställd på **OnlineAll**. [Läs mer](prepare-for-migration.md). |
Anslut efter migreringen – Linux | Ansluta till virtuella Azure-datorer efter migrering med SSH:<br/> Innan migreringen går du till den lokala datorn, kontrollerar att tjänsten Secure Shell är inställt på Start och att brand Väggs reglerna tillåter en SSH-anslutning.<br/> Efter redundansväxlingen på den virtuella Azure-datorn tillåter du inkommande anslutningar till SSH-porten för reglerna för nätverks säkerhets grupper på den misslyckade virtuella datorn och för det Azure-undernät som den är ansluten till. Lägg också till en offentlig IP-adress för den virtuella datorn. |  

## <a name="next-steps"></a>Nästa steg

[Migrera virtuella Hyper-V-datorer](tutorial-migrate-hyper-v.md) för migrering.
