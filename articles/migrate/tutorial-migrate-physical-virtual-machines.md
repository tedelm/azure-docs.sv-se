---
title: Migrera datorer som fysiska servrar till Azure med Azure Migrate.
description: I den här artikeln beskrivs hur du migrerar fysiska datorer till Azure med Azure Migrate.
ms.topic: tutorial
ms.date: 04/15/2020
ms.custom: MVC
ms.openlocfilehash: 1824fc6c7cbc0fd0390770027f4a15d9130139de
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/29/2020
ms.locfileid: "81535391"
---
# <a name="migrate-machines-as-physical-servers-to-azure"></a>Migrera datorer som fysiska servrar till Azure

Den här artikeln visar hur du migrerar datorer som fysiska servrar till Azure med hjälp av verktyget Azure Migrate: Migreringsverktyg för Server. Att migrera datorer genom att behandla dem som fysiska servrar är användbara i ett antal scenarier:

- Migrera lokala fysiska servrar.
- Migrera virtuella datorer som är virtualiserade med plattformar som xen, KVM.
- Migrera virtuella Hyper-V-eller VMware-datorer, om det av någon anledning inte går att använda standardmigreringen för [Hyper-v](tutorial-migrate-hyper-v.md)eller [VMware](server-migrate-overview.md) -migrering.
- Migrera virtuella datorer som körs i privata moln.
- Migrera virtuella datorer som körs i offentliga moln, till exempel Amazon Web Services (AWS) eller Google Cloud Platform (GCP).


Den här självstudien är den tredje i en serie som visar hur du bedömer och migrerar fysiska servrar till Azure. I den här guiden får du lära dig att:

> [!div class="checklist"]
> * Förbered för att använda Azure med Azure Migrate: Server-migrering.
> * Kontrol lera kraven för datorer som du vill migrera och Förbered en dator för Azure Migrate Replication-enheten som används för att identifiera och migrera datorer till Azure.
> * Lägg till Migreringsverktyg för Azure Migrate server i Azure Migrate Hub.
> * Konfigurera replikerings enheten.
> * Installera mobilitets tjänsten på datorer som du vill migrera.
> * Aktivera replikering.
> * Kör en testmigrering för att se till att allt fungerar som förväntat.
> * Kör en fullständig migrering till Azure.

> [!NOTE]
> Självstudier visar dig den enklaste distributions Sök vägen för ett scenario så att du snabbt kan konfigurera ett koncept för koncept bevis. Självstudier använder standard alternativ där det är möjligt, och visar inte alla möjliga inställningar och sökvägar. Detaljerade anvisningar finns i instruktionen how-TOS för Azure Migrate.

Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt](https://azure.microsoft.com/pricing/free-trial/) konto innan du börjar.


## <a name="prerequisites"></a>Krav

Innan du börjar de här självstudierna bör du:

[Granska](migrate-architecture.md) migreringens arkitektur.




## <a name="prepare-azure"></a>Förbereda Azure

Förbered Azure för migrering med Server migrering.

**Uppgift** | **Information**
--- | ---
**Skapa ett Azure Migrate-projekt** | Ditt Azure-konto måste ha Contributes eller ägar behörigheter för att skapa ett projekt.
**Verifiera behörigheter för ditt Azure-konto** | Ditt Azure-konto måste ha behörighet att skapa en virtuell dator och skriva till en Azure-hanterad disk.


### <a name="assign-permissions-to-create-project"></a>Tilldela behörigheter för att skapa projekt

1. Öppna prenumerationen i Azure Portal och välj **åtkomst kontroll (IAM)**.
2. Leta upp det relevanta kontot i **kontrol lera åtkomst**och klicka på det för att visa behörigheter.
3. Du bör ha behörighet som **deltagare** eller **ägare** .
    - Om du precis har skapat ett kostnads fritt Azure-konto är du ägare till din prenumeration.
    - Om du inte är prenumerations ägare kan du samar beta med ägaren för att tilldela rollen.


### <a name="assign-azure-account-permissions"></a>Tilldela behörigheter för Azure-konto

Tilldela Azure-kontot rollen virtuell dator deltagare. Detta ger behörighet att:

    - Skapa en virtuell dator i den valda resursgruppen.
    - Skapa en virtuell dator i det valda virtuella nätverket.
    - Skriv till en Azure-hanterad disk. 

### <a name="create-an-azure-network"></a>Skapa ett Azure-nätverk

[Konfigurera](../virtual-network/manage-virtual-network.md#create-a-virtual-network) ett virtuellt Azure-nätverk (VNet). När du replikerar till Azure skapas och ansluts virtuella Azure-datorer till det virtuella Azure-nätverk som du anger när du konfigurerar migrering.

## <a name="prepare-for-migration"></a>Förbereda för migrering

För att förbereda för migrering av fysiska servrar måste du verifiera inställningarna för den fysiska servern och förbereda distributionen av en replikeringsfil.

### <a name="check-machine-requirements-for-migration"></a>Kontrol lera dator kraven för migrering

Kontrol lera att datorerna uppfyller kraven för migrering till Azure. 

> [!NOTE]
> När du migrerar fysiska datorer Azure Migrate: Server migrering använder samma replikeringsprincip som agentbaserade haveri beredskap i Azure Site Recovery-tjänsten, och vissa komponenter delar samma kodbas. En del innehåll kan länkas till Site Recovery-dokumentationen.

1. [Kontrol lera](migrate-support-matrix-physical-migration.md#physical-server-requirements) krav för fysisk server.
2. Kontrol lera att lokala datorer som du replikerar till Azure uppfyller [kraven för virtuella Azure](migrate-support-matrix-physical-migration.md#azure-vm-requirements)-datorer.


### <a name="prepare-a-machine-for-the-replication-appliance"></a>Förbereda en dator för replikerings enheten

Azure Migrate: Server-migreringen använder en replikeringsfil för att replikera datorer till Azure. Replikeringssystemet kör följande komponenter.

- **Konfigurations**Server: konfigurations servern samordnar kommunikationen mellan den lokala miljön och Azure och hanterar datareplikering.
- **Processerver**: processervern fungerar som en gateway för replikering. Den tar emot replikeringsdata; optimerar den med cachelagring, komprimering och kryptering och skickar den till ett cache Storage-konto i Azure. 

Förbered distribution av installationer enligt följande:

- Du förbereder en dator för att vara värd för replikerings enheten. [Granska](migrate-replication-appliance.md#appliance-requirements) maskin varu kraven. Enheten bör inte installeras på en käll dator som du vill replikera.
- Replikerings enheten använder MySQL. Granska [alternativen](migrate-replication-appliance.md#mysql-installation) för att installera MySQL på-enheten.
- Granska de Azure-URL: er som krävs för att replikeringssystemet ska kunna komma åt [offentliga](migrate-replication-appliance.md#url-access) och [offentliga](migrate-replication-appliance.md#azure-government-url-access) moln.
- Granska [port] (Migrate-Replication-installation. MD # port-Access) åtkomst krav för replikerings enheten.

## <a name="add-the-server-migration-tool"></a>Lägg till Migreringsverktyg för Server

Konfigurera ett Azure Migrate projekt och Lägg sedan till Migreringsverktyg för server i det.

1. I Azure-portalen > **Alla tjänster** söker du efter **Azure Migrate**.
2. Under **Tjänster** väljer du **Azure Migrate**.
3. I **översikten** klickar du på **Utvärdera och migrera servrar**.
4. Under **identifiera, utvärdera och migrera servrar**klickar du på **utvärdera och migrera servrar**.

    ![Identifiera och utvärdera servrar](./media/tutorial-migrate-physical-virtual-machines/assess-migrate.png)

5. I **Discover, assess and migrate servers** (Identifiera, utvärdera och migrera servrar) klickar du på **Lägg till verktyg**.
6. I **Migrera projekt** väljer du din Azure-prenumeration och skapar en resursgrupp om du inte har någon.
7. I **Projektinformation** anger du projektnamnet och geografin där du vill skapa projektet och klickar på **Nästa**. Granska stödda geografiska områden för [offentliga](migrate-support-matrix.md#supported-geographies-public-cloud) och [offentliga moln](migrate-support-matrix.md#supported-geographies-azure-government).

    ![Skapa ett Azure Migrate-projekt](./media/tutorial-migrate-physical-virtual-machines/migrate-project.png)

8. I **Välj bedömnings verktyg**väljer du **hoppa över Lägg till ett bedömnings verktyg för** > **Nästa**gång.
9. I **Välj Migreringsverktyg**väljer du **Azure Migrate: Server migrering** > **Nästa**.
10. I **Review + add tools** (Granska + lägg till verktyg)
granskar du inställningarna och klickar på **Lägg till verktyg**
11. När du har lagt till verktyget visas det i Azure Migrate för Project > **Server** > -**Migreringsverktyg**.

## <a name="set-up-the-replication-appliance"></a>Konfigurera replikerings enheten

Det första steget i migreringen är att konfigurera replikerings enheten. Om du vill konfigurera installations programmet för fysisk server-migrering, laddar du ned installations filen för installationen och kör den sedan på den [dator som du för beredde](#prepare-a-machine-for-the-replication-appliance). När du har installerat installationen registrerar du den med Azure Migrate Server-migrering.


### <a name="download-the-replication-appliance-installer"></a>Ladda ned installations programmet för replikerings enheten

1. I **Azure Migrate: Server-migrering**i Azure Migrate Project >- **servrar**klickar du på **identifiera**.

    ![Identifiera virtuella datorer](./media/tutorial-migrate-physical-virtual-machines/migrate-discover.png)

3. I **identifiera datorer** > **är dina datorer virtualiserade?**, klicka på **inte virtualiserad/annan**.
4. I **mål region**väljer du den Azure-region som du vill migrera datorerna till.
5. Välj **Bekräfta att mål regionen för migrering är regions namn**.
6. Klicka på **Skapa resurser**. Detta skapar ett Azure Site Recovery valv i bakgrunden.
    - Om du redan har konfigurerat migrering med Azure Migrate Server-migreringen kan du inte konfigurera mål alternativet eftersom resurserna tidigare har kon figurer ATS.
    - Du kan inte ändra mål region för projektet när du har klickat på den här knappen.
    - Alla efterföljande migreringar är till den här regionen.

7. I vill **du installera en ny replikeringsprincip? väljer du** **installera en replikeringsprincip**.
9. I **Hämta och installera installations programmet för replikering**laddar du ned installations programmet för installationen och registrerings nyckeln. Du behöver nyckeln för att kunna registrera installationen. Nyckeln är giltig i fem dagar efter att den har laddats ned.

    ![Hämta Provider](media/tutorial-migrate-physical-virtual-machines/download-provider.png)

10. Kopiera installations filen för installationen och nyckel filen till Windows Server 2016-datorn som du skapade för installationen.
11. Kör installations filen för replikeringstjänsten enligt beskrivningen i nästa procedur. När installationen är klar startas konfigurations guiden för enheten automatiskt (du kan också starta guiden manuellt genom att använda cspsconfigtool-genvägen som skapas på Skriv bordet på enheten). Använd fliken Hantera konton i guiden för att lägga till konto information som ska användas för push-installation av mobilitets tjänsten. I den här självstudien kommer vi att installera mobilitets tjänsten manuellt på datorer som ska replikeras, så skapa ett dummy-konto i det här steget och gå vidare.

12. När installationen har startats om efter installationen går du till **identifiera datorer**, väljer den nya installationen i **Välj konfigurations Server**och klickar på **Slutför registrering**. Genom att slutföra registreringen utförs några slutliga uppgifter för att förbereda replikeringen.

    ![Slutför registrering](./media/tutorial-migrate-physical-virtual-machines/finalize-registration.png)

Det kan ta lite tid efter att registreringen har slutförts tills identifierade datorer visas i Azure Migrate Server-migrering. När virtuella datorer identifieras ökar antalet **identifierade servrar** .

![Identifierade servrar](./media/tutorial-migrate-physical-virtual-machines/discovered-servers.png)


## <a name="install-the-mobility-service"></a>Installera mobilitetstjänsten

På datorer som du vill migrera måste du installera mobilitets tjänst agenten. Agent installationerna är tillgängliga på replikerings enheten. Du hittar rätt installations program och installerar agenten på varje dator som du vill migrera. Gör på följande sätt:

1. Logga in på replikerings enheten.
2. Navigera till **%programdata%\ASR\home\svsystems\pushinstallsvc\repository**.
3. Hitta installations programmet för datorns operativ system och version. Granska [operativ system som stöds](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#replicated-machines). 
4. Kopiera installations filen till den dator som du vill migrera.
5. Kontrol lera att du har den lösen fras som genererades när du distribuerade enheten.
    - Lagra filen i en tillfällig textfil på datorn.
    - Du kan hämta lösen frasen på replikerings enheten. Från kommando raden kör du **C:\ProgramData\ASR\home\svsystems\bin\genpassphrase.exe-v** för att visa den aktuella lösen frasen.
    - Återskapa inte lösen frasen. Detta bryter anslutningen och du måste registrera om replikerings enheten.


### <a name="install-on-windows"></a>Installera i Windows

1. Extrahera innehållet i installations filen till en lokal mapp (till exempel C:\Temp) på datorn enligt följande:

    ```
    ren Microsoft-ASR_UA*Windows*release.exe MobilityServiceInstaller.exe
    MobilityServiceInstaller.exe /q /x:C:\Temp\Extracted
    cd C:\Temp\Extracted
    ```
2. Kör installations programmet för mobilitets tjänsten:
    ```
   UnifiedAgent.exe /Role "MS" /Silent
    ```
3. Registrera agenten med replikerings enheten:
    ```
    cd C:\Program Files (x86)\Microsoft Azure Site Recovery\agent
    UnifiedAgentConfigurator.exe  /CSEndPoint <replication appliance IP address> /PassphraseFilePath <Passphrase File Path>
    ```

### <a name="install-on-linux"></a>Installera på Linux

1. Extrahera innehållet i installations tarball till en lokal mapp (till exempel/tmp/MobSvcInstaller) på datorn enligt följande:
    ```
    mkdir /tmp/MobSvcInstaller
    tar -C /tmp/MobSvcInstaller -xvf <Installer tarball>
    cd /tmp/MobSvcInstaller
    ```
2. Kör installations skriptet:
    ```
    sudo ./install -r MS -q
    ```
3. Registrera agenten med replikerings enheten:
    ```
    /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i <replication appliance IP address> -P <Passphrase File Path>
    ```

## <a name="replicate-machines"></a>Replikera datorer

Välj datorer för migrering nu. 

> [!NOTE]
> Du kan replikera upp till 10 datorer tillsammans. Om du behöver replikera mer replikerar du dem samtidigt i batchar med 10.

1. I Azure Migrate Project >- **servrar** **Azure Migrate: Server-migrering**klickar du på **Replikera**.

    ![Replikera virtuella datorer](./media/tutorial-migrate-physical-virtual-machines/select-replicate.png)

2. I **Replikera**, > **käll inställningar** > **att datorerna har virtualiserats?** väljer du **inte virtualiserad/övrigt**.
3. I **lokal**installation väljer du namnet på Azure Migrate-installationen som du konfigurerar.
4. I **processerver**väljer du namnet på replikerings enheten.
6. I **autentiseringsuppgifter för gäst**anger du ett dummy-konto som ska användas för att installera mobilitets tjänsten manuellt (push-installation stöds inte i fysiska). Klicka sedan på **Nästa: virtuella datorer**.

    ![Replikera virtuella datorer](./media/tutorial-migrate-physical-virtual-machines/source-settings.png)

7. I **Virtual Machines**, i **Importera migreringsjobb från en utvärdering?**, lämnar du standardinställningen **Nej, jag anger inställningarna för migrering manuellt**.
8. Markera varje virtuell dator som du vill migrera. Klicka sedan på **Nästa: mål inställningar**.

    ![Välj virtuella datorer](./media/tutorial-migrate-physical-virtual-machines/select-vms.png)


9. I **Målinställningar** väljer du prenumeration och den målregion som du vill migrera till. Ange sedan den resursgrupp där du vill att de virtuella Azure-datorerna ska finnas efter migreringen.
10. I **Virtuellt nätverk** väljer du det Azure VNet/undernät som de virtuella Azure-datorerna ska anslutas till efter migreringen.
11. I **Azure Hybrid-förmån**:

    - Välj **Nej** om du inte vill använda Azure Hybrid-förmånen. Klicka sedan på **Nästa**.
    - Välj **Ja** om du har Windows Server-datorer som omfattas av aktiva Software Assurance- eller Windows Server-prenumerationer och du vill tillämpa förmånen på de datorer som du migrerar. Klicka sedan på **Nästa**.

    ![Mål inställningar](./media/tutorial-migrate-physical-virtual-machines/target-settings.png)

12. I **Compute** granskar du namnet på den virtuella datorn, storlek, disktyp för operativsystemet och tillgänglighetsuppsättningen. De virtuella datorerna måste följa [Azures krav](migrate-support-matrix-physical-migration.md#azure-vm-requirements).

    - **VM-storlek**: Azure Migrate Server-migreringen väljer som standard en storlek baserat på den närmaste matchningen i Azure-prenumerationen. Du kan också välja en storlek manuellt i **Storlek på virtuell Azure-dator**. 
    - **OS-disk**: Ange OS-disken (start) för den virtuella datorn. Operativsystemdisken är den disk där operativsystemets bootloader och installationsprogram finns. 
    - **Tillgänglighets uppsättning**: om den virtuella datorn ska finnas i en Azure-tillgänglighets uppsättning efter migreringen anger du uppsättningen. Uppsättningen måste finnas i målets resursgrupp som du anger för migreringen.

    ![Beräknings inställningar](./media/tutorial-migrate-physical-virtual-machines/compute-settings.png)

13. I **diskar**anger du om de virtuella dator diskarna ska replikeras till Azure och väljer disk typ (standard SSD/HDD eller Premium Managed Disks) i Azure. Klicka sedan på **Nästa**.
    - Du kan undanta diskar från replikering.
    - Om du undantar diskar kommer de inte att synas i den virtuella Azure-datorn efter migreringen. 

    ![Disk inställningar](./media/tutorial-migrate-physical-virtual-machines/disks.png)


14. I **Granska och starta replikering** kontrollerar du inställningarna och klickar på **Replikera** för att påbörja den första replikeringen för servrarna.

> [!NOTE]
> Du kan uppdatera replikeringsinställningar varje tid innan replikeringen startar, **Hantera** > **replikering av datorer**. Det går inte att ändra inställningarna efter att replikeringen har startat.



## <a name="track-and-monitor"></a>Spåra och övervaka

- När du klickar på **Replikera** startar du jobbet starta replikering. 
- När jobbet starta replikeringen har slutförts påbörjar datorerna sin inledande replikering till Azure.
- När den inledande replikeringen har slutförts börjar delta-replikeringen. Stegvisa ändringar av lokala diskar replikeras regelbundet till replik diskarna i Azure.


Du kan spåra jobb status i Portal meddelanden.

Du kan övervaka replikeringsstatus genom att klicka på **Replikera servrar** i **Azure Migrate: Server-migrering**.
![Övervaka replikering](./media/tutorial-migrate-physical-virtual-machines/replicating-servers.png)

## <a name="run-a-test-migration"></a>Kör en testmigrering


När delta-replikering börjar kan du köra en testmigrering för de virtuella datorerna innan du kör en fullständig migrering till Azure. Vi rekommenderar starkt att du gör detta minst en gång för varje dator innan du migrerar den.

- Genom att köra en test-migrering kontrollerar du att migreringen fungerar som förväntat, utan att det påverkar de lokala datorerna, som fortsätter att fungera och fortsätter att replikeras. 
- Testmigrering simulerar migreringen genom att skapa en virtuell Azure-dator med replikerade data (vanligt vis migrera till ett virtuellt nätverk som inte är i Azure-prenumerationen).
- Du kan använda den replikerade virtuella Azure-datorn för att verifiera migreringen, utföra app-testning och åtgärda eventuella problem före fullständig migrering.

Gör en testmigrering enligt följande:


1. I**Server för** >  **migrerings mål** > **Azure Migrate: Server migrering**klickar du på **test migrerade servrar**.

     ![Testmigrerade servrar](./media/tutorial-migrate-physical-virtual-machines/test-migrated-servers.png)

2. Högerklicka på den virtuella dator som ska testas och klicka på **Testmigrera**.

    ![Testmigrera](./media/tutorial-migrate-physical-virtual-machines/test-migrate.png)

3. I **Testmigrera** väljer du det Azure VNet där den virtuella Azure-datorn kommer att finnas efter migreringen. Vi rekommenderar att du använder ett VNet för icke-produktion.
4. **Testmigreringen** startas. Övervaka jobbet i portalmeddelanden.
5. När migreringen är klar kan du se den migrerade virtuella Azure-datorn i **Virtual Machines** i Azure Portal. Datornamnet har suffixet **-Test**.
6. När testet är klart högerklickar du på den virtuella Azure-datorn i **Replikera datorer** och klickar på **Rensa upp i testmigreringen**.

    ![Rensa migrering](./media/tutorial-migrate-physical-virtual-machines/clean-up.png)


## <a name="migrate-vms"></a>Migrera virtuella datorer

När du har kontrollerat att testmigreringen fungerar som förväntat kan du migrera de lokala datorerna.

1. I Azure Migrate Project >- **servrar** > **Azure Migrate: Server-migrering**klickar du på **Replikera servrar**.

    ![Servrarna replikeras](./media/tutorial-migrate-physical-virtual-machines/replicate-servers.png)

2. I **Replikera datorer** högerklickar du på den virtuella datorn > **Migrera**.
3. I **migrera** > **Stäng virtuella datorer och utför en planerad migrering utan data förlust**väljer du **Ja** > **OK**.
    - Om du inte vill stänga av den virtuella datorn väljer du **Nej**

    Obs! för migrering av fysiska servrar är rekommendationen att sätta igång programmet som en del av migreringstabellen (låt inte programmen acceptera några anslutningar) och initiera sedan migreringen (servern måste vara igång, så återstående ändringar kan synkroniseras) innan migreringen har slutförts.

4. Ett migreringsjobb startas för den virtuella datorn. Spåra jobbet i Azure-meddelanden.
5. När jobbet är klart kan du se och hantera den virtuella datorn på sidan **Virtual Machines**.

## <a name="complete-the-migration"></a>Slutföra migreringen

1. När migreringen är färdig högerklickar du på den virtuella datorn > **stoppa migreringen**. Det här gör följande:
    - Stoppar replikering för den lokala datorn.
    - Tar bort datorn från antalet **replikerade servrar** i Azure Migrate: Server-migrering.
    - Rensar information om replikeringstillståndet för datorn.
2. Installera [Azure VM-](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-windows) eller [Linux](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-linux) -agenten på de migrerade datorerna.
3. Utför alla finjusteringar av appen efter migreringen som att uppdatera databasanslutningssträngar och webbserverkonfigurationer.
4. Utför slutlig program- och migreringsacceptanstestning på det migrerade programmet som nu körs i Azure.
5. Klipp ut över trafik till den migrerade virtuella Azure-instansen.
6. Ta bort de lokala virtuella datorerna från ditt lokala VM-inventarie.
7. Ta bort de lokala virtuella datorerna från lokala säkerhetskopior.
8. Uppdatera eventuell intern dokumentation för att ange den nya platsen och IP-adressen för de virtuella Azure-datorerna. 

## <a name="post-migration-best-practices"></a>Metod tips för efter migreringen

- För ökat skydd:
    - Skydda data genom att säkerhetskopiera virtuella Azure-datorer med Azure Backup-tjänsten. [Läs mer](../backup/quick-backup-vm-portal.md).
    - Håll arbetsbelastningar i körning och kontinuerligt tillgängliga genom att replikera virtuella Azure-datorer till en sekundär region med Site Recovery. [Läs mer](../site-recovery/azure-to-azure-tutorial-enable-replication.md).
- För ökad säkerhet:
    - Lås och begränsa inkommande trafik åtkomst med [Azure Security Center – just-in-Time-administration](https://docs.microsoft.com/azure/security-center/security-center-just-in-time).
    - Begränsa nätverkstrafik till hanteringsslutpunkter med [nätverkssäkerhetsgrupper](https://docs.microsoft.com/azure/virtual-network/security-overview).
    - Distribuera [Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption-overview) för att säkra diskar och skydda data från stöld och obehörig åtkomst.
    - Läs mer om [ att skydda IaaS-resurser](https://azure.microsoft.com/services/virtual-machines/secure-well-managed-iaas/) och besök [Azure Security Center](https://azure.microsoft.com/services/security-center/).
- För övervakning och hantering:
    - Överväg att distribuera [Azure Cost Management](https://docs.microsoft.com/azure/cost-management/overview) för att övervaka användning och utgifter.


## <a name="next-steps"></a>Nästa steg

Undersök [resan för migrering i molnet](https://docs.microsoft.com/azure/architecture/cloud-adoption/getting-started/migrate) i Azure Cloud adoption Framework.
