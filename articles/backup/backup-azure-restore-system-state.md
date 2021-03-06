---
title: Återställa system tillstånd till en Windows-Server
description: Steg för steg-förklaringar för återställning av Windows Server-systemtillstånd från en säkerhets kopia i Azure.
ms.reviewer: saurse
ms.topic: conceptual
ms.date: 08/18/2017
ms.openlocfilehash: 39cac84c4a33c1da209d0a0cc7b0f8ac8ee390a0
ms.sourcegitcommit: 50ef5c2798da04cf746181fbfa3253fca366feaa
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/30/2020
ms.locfileid: "82610793"
---
# <a name="restore-system-state-to-windows-server"></a>Återställa system tillstånd till Windows Server

Den här artikeln förklarar hur du återställer säkerhets kopior av Windows Server-systemtillstånd från ett Azure Recovery Services-valv. Om du vill återställa system tillstånd måste du ha en säkerhets kopia av system tillstånd (skapas med instruktionerna i [säkerhetskopiera system tillstånd](backup-azure-system-state.md#back-up-windows-server-system-state)och kontrol lera att du har installerat den [senaste versionen av Microsoft Azure Recovery Services (mars)-agenten](https://aka.ms/azurebackup_agent). Att hämta Windows Server System tillstånds data från ett Azure Recovery Services-valv är en process i två steg:

1. Återställ system tillstånd som filer från Azure Backup. När du återställer system tillstånd som filer från Azure Backup kan du antingen:
   * Återställa system tillstånd till samma server där säkerhets kopiorna gjordes, eller
   * Återställ system tillstånds filen till en annan server.

2. Tillämpa återställda system tillstånds filer på en Windows-Server.

## <a name="recover-system-state-files-to-the-same-server"></a>Återställa systemtillståndsfiler till samma server

Följande steg beskriver hur du återställer din Windows Server-konfiguration till ett tidigare tillstånd. Att återställa Server konfigurationen till ett känt, stabilt tillstånd, kan vara mycket värdefull. Följande steg återställer serverns system tillstånd från ett Recovery Services-valv.

1. Öppna snapin-modulen **Microsoft Azure Backup**. Om du inte vet var snapin-modulen har installerats söker du efter **Microsoft Azure Backup**i datorn eller servern.

    Skriv bords appen bör visas i Sök resultaten.

2. Klicka på **Återställ data** för att starta guiden.

    ![Återställa data](./media/backup-azure-restore-windows-server/recover.png)

3. I rutan **komma igång** , för att återställa data till samma server eller dator, väljer du **den här servern (`<server name>`)** och klickar på **Nästa**.

    ![Välj detta Server alternativ för att återställa data till samma dator](./media/backup-azure-restore-system-state/samemachine.png)

4. I fönstret **Välj återställnings läge** väljer du **system tillstånd** och klickar sedan på **Nästa**.

    ![Bläddra bland filer](./media/backup-azure-restore-system-state/recover-type-selection.png)

5. Välj en återställnings punkt i kalendern i fönstret **Välj volym och datum** .

    Du kan återställa från vilken återställnings punkt som helst. Datum i **fetstil** anger tillgänglighet för minst en återställnings punkt. När du har valt ett datum, om det finns flera återställnings punkter, väljer du den aktuella återställnings punkten från den nedrullningsbara menyn **tid** .

    ![Volym och datum](./media/backup-azure-restore-system-state/select-date.png)

6. När du har valt återställnings punkt att återställa klickar du på **Nästa**.

    Azure Backup monterar den lokala återställnings punkten och använder den som återställnings volym.

7. I nästa fönster anger du målet för de återställda system tillstånds filerna och klickar på **Bläddra** för att öppna Utforskaren och hitta de filer och mappar som du vill använda. Alternativet **skapa kopior så att du har båda versionerna**, skapar kopior av enskilda filer i en befintlig fil Arkiv i system tillstånd i stället för att skapa en kopia av hela system tillstånds arkivet.

    ![Återställnings alternativ](./media/backup-azure-restore-system-state/recover-as-files.png)

8. Kontrol lera informationen om återställningen i **bekräftelse** fönstret och klicka på **Återställ**.

   ![Klicka på Återställ för att bekräfta åtgärden för att återställa](./media/backup-azure-restore-system-state/confirm-recovery.png)

9. Kopiera *WindowsImageBackup* -katalogen på återställnings målet till en icke-kritisk volym på servern. Normalt är Windows OS-volymen den kritiska volymen.

10. När återställningen har slutförts följer du stegen i avsnittet, [tillämpar återställda system tillstånds filer på Windows Server](backup-azure-restore-system-state.md), för att slutföra återställnings processen för system tillstånd.

## <a name="recover-system-state-files-to-an-alternate-server"></a>Återställa system tillstånds filer till en annan server

Om Windows-servern är skadad eller otillgänglig, och du vill återställa den till ett stabilt tillstånd genom att återställa Windows Server System tillstånd, kan du återställa den skadade serverns system tillstånd från en annan server. Använd följande steg för att återställa system tillstånd på en separat server.  

Den terminologi som används i dessa steg omfattar:

* *Käll dator* – den ursprungliga dator som säkerhets kopian skapades från och som för tillfället inte är tillgänglig.
* *Måldator* – den dator som data återställs till.
* *Exempel valv* – det Recovery Services valv som *käll datorn* och *mål datorn* är registrerade på. <br/>

> [!NOTE]
> Säkerhets kopieringar som tas från en dator kan inte återställas till en dator som kör en tidigare version av operativ systemet. Säkerhets kopieringar som tagits från en Windows Server 2016-dator kan till exempel inte återställas till Windows Server 2012 R2. Inversen är dock möjlig. Du kan använda säkerhets kopior från Windows Server 2012 R2 för att återställa Windows Server 2016.
>

1. Öppna snapin-modulen **Microsoft Azure Backup** på *mål datorn*.
2. Se till att *mål datorn* och *käll datorn* är registrerade på samma Recovery Services-valv.
3. Klicka på **Återställ data** för att starta arbets flödet.
4. Välj **en annan server**

    ![En annan server](./media/backup-azure-restore-system-state/anotherserver.png)

5. Ange den loggfil för valvet som motsvarar *exempel valvet*. Om valvet för valvet är ogiltigt (eller har gått ut) laddar du ned en ny fil för valvet från *exempel valvet* i Azure Portal. När du har angett valv filen för valvet visas det Recovery Services valv som är associerat med filen med valvets autentiseringsuppgifter.

6. I fönstret Välj säkerhets kopierings server väljer du *käll datorn* i listan med datorer som visas.
7. I fönstret Välj återställnings läge väljer du **system tillstånd** och klickar på **Nästa**.

    ![Sök](./media/backup-azure-restore-system-state/recover-type-selection.png)

8. I kalendern i fönstret **Välj volym och datum** väljer du en återställnings punkt. Du kan återställa från vilken återställnings punkt som helst. Datum i **fetstil** anger tillgänglighet för minst en återställnings punkt. När du har valt ett datum, om det finns flera återställnings punkter, väljer du den aktuella återställnings punkten från den nedrullningsbara menyn **tid** .

    ![Sök efter objekt](./media/backup-azure-restore-system-state/select-date.png)

9. När du har valt återställnings punkt att återställa klickar du på **Nästa**.

10. I fönstret **Välj återställnings läge för system tillstånd** anger du den plats där du vill att system tillstånds filerna ska återställas och klickar sedan på **Nästa**.

    ![Kryptering](./media/backup-azure-restore-system-state/recover-as-files.png)

    Alternativet **skapa kopior så att du har båda versionerna**, skapar kopior av enskilda filer i en befintlig fil Arkiv i system tillstånd i stället för att skapa en kopia av hela system tillstånds arkivet.

11. Kontrol lera informationen om återställningen i bekräftelse fönstret och klicka på **Återställ**.

    ![Klicka på knappen Återställ för att bekräfta återställnings processen](./media/backup-azure-restore-system-state/confirm-recovery.png)

12. Kopiera katalogen *WindowsImageBackup* till en icke-kritisk volym på servern (till exempel D:\). Vanligt vis är Windows OS-volymen den kritiska volymen.

13. Slutför återställnings processen genom att använda följande avsnitt för att [tillämpa de återställda filerna för system tillstånd på en Windows-Server](#apply-restored-system-state-on-a-windows-server).

## <a name="apply-restored-system-state-on-a-windows-server"></a>Tillämpa återställt systemtillstånd på en Windows Server

När du har återställt system tillstånd som filer med Azure Recovery Services Agent använder du verktyget Windows Server Backup för att tillämpa det återställda system läget på Windows Server. Verktyget Windows Server Backup finns redan på servern. Följande steg beskriver hur du tillämpar det återställda system tillstånd.

1. Använd följande kommandon för att starta om servern i *reparations läge för katalog tjänster*. I en upphöjd kommando tolk:

    ```cmd
    Bcdedit /set safeboot dsrepair
    Shutdown /r /t 0
    ```

2. Starta snapin-modulen Windows Server Backup efter omstart. Om du inte vet var snapin-modulen har installerats söker du efter **Windows Server Backup**i datorn eller servern.

    Skriv bords appen visas i Sök resultaten. Om den inte visas, eller om du stöter på fel när du öppnar programmet, måste du installera **Windows Server Backup funktioner**, och beroende komponenter under den, som är tillgängliga i **guiden Lägg till funktioner** i **Serverhanteraren**.

3. I snapin-modulen väljer du **lokal säkerhets kopiering**.

    ![Välj lokal säkerhets kopia att återställa därifrån](./media/backup-azure-restore-system-state/win-server-backup-local-backup.png)

4. Klicka på **Återställ** i rutan **åtgärder**i den lokala säkerhets kopierings konsolen för att öppna återställnings guiden.

5. Välj alternativet, **en säkerhets kopia som lagrats på en annan plats**och klicka på **Nästa**.

   ![Välj att återställa till en annan server](./media/backup-azure-restore-system-state/backup-stored-in-diff-location.png)

6. När du anger plats typen väljer du **delad fjärrmapp** om säkerhets kopian av system tillstånd återställdes till en annan server. Om system tillstånd har återställts lokalt väljer du **lokala enheter**.

    ![Välj om du vill återställa från en lokal server eller en annan](./media/backup-azure-restore-system-state/ss-recovery-remote-shared-folder.png)

7. Ange sökvägen till katalogen *WindowsImageBackup* eller Välj den lokala enhet som innehåller katalogen (till exempel D:\WindowsImageBackup), återställt som en del av återställningen av system tillstånds filer med Azure Recovery Services-agenten och klicka på **Nästa**.

    ![sökväg till den delade filen](./media/backup-azure-restore-system-state/ss-recovery-remote-folder.png)

8. Välj den version av system tillstånd som du vill återställa och klicka på **Nästa**.

9. I fönstret Välj återställnings typ väljer du **system tillstånd** och klickar på **Nästa**.

10. För system tillstånds återställningens plats väljer du **ursprunglig plats**och klickar på **Nästa**.

11. Läs igenom bekräftelse informationen, verifiera inställningarna för omstart och klicka på **Återställ** för att tillämpa de återställda system tillstånds filerna.

    ![starta återställningen av filer för system tillstånd](./media/backup-azure-restore-system-state/launch-ss-recovery.png)

## <a name="special-considerations-for-system-state-recovery-on-active-directory-server"></a>Särskilda överväganden för återställning av system tillstånd på Active Directory Server

Säkerhets kopiering av system tillstånd omfattar Active Directory data. Använd följande steg för att återställa Active Directory-domän tjänsten (AD DS) från det aktuella läget till ett tidigare tillstånd.

1. Starta om domänkontrollanten i återställnings läge för katalog tjänster (DSRM).
2. Följ stegen [här](https://docs.microsoft.com/windows-server/identity/ad-ds/manage/ad-forest-recovery-nonauthoritative-restore) för att använda Windows Server Backup cmdlets för att återställa AD DS.

## <a name="troubleshoot-failed-system-state-restore"></a>Felsökning av misslyckad återställning av systemtillstånd

Om den tidigare processen att tillämpa system tillstånd inte slutförs korrekt kan du använda Windows återställnings miljö (Win) för att återställa Windows Server. Följande steg beskriver hur du återställer med hjälp av Win RE. Använd bara det här alternativet om Windows Server inte startar normalt efter en återställning av system tillstånd. Följande process raderar icke-systemdata och använder försiktighet.

1. Starta Windows Server i Windows återställnings miljö (Win RE).

2. Välj Felsök från de tre tillgängliga alternativen.

    ![öppna meny](./media/backup-azure-restore-system-state/winre-1.png)

3. Välj **kommando tolk** på skärmen **Avancerade alternativ** och ange användar namn och lösen ord för Server administratör.

   ![öppna meny](./media/backup-azure-restore-system-state/winre-2.png)

4. Ange användar namn och lösen ord för Server administratören.

    ![öppna meny](./media/backup-azure-restore-system-state/winre-3.png)

5. När du öppnar kommando tolken i administratörs läge kör du följande kommando för att hämta system tillståndets säkerhets kopierings versioner.

    ```cmd
    Wbadmin get versions -backuptarget:<Volume where WindowsImageBackup folder is copied>:
    ```

    ![Hämta säkerhets kopie versioner för system tillstånd](./media/backup-azure-restore-system-state/winre-4.png)

6. Kör följande kommando för att hämta alla volymer som är tillgängliga i säkerhets kopian.

    ```cmd
    Wbadmin get items -version:<copy version from above step> -backuptarget:<Backup volume>
    ```

    ![Hämta säkerhets kopie versioner för system tillstånd](./media/backup-azure-restore-system-state/winre-5.png)

7. Följande kommando återställer alla volymer som ingår i säkerhets kopieringen av system tillstånd. Observera att det här steget bara återställer de kritiska volymer som ingår i systemets tillstånd. Alla data som inte är system raderas.

    ```cmd
    Wbadmin start recovery -items:C: -itemtype:Volume -version:<Backupversion> -backuptarget:<backup target volume>
    ```

     ![Hämta säkerhets kopie versioner för system tillstånd](./media/backup-azure-restore-system-state/winre-6.png)

## <a name="next-steps"></a>Nästa steg

* Nu när du har återställt dina filer och mappar kan du [Hantera dina säkerhets kopior](backup-azure-manage-windows-server.md).
