---
title: Migrera TDE-certifikat – hanterad instans
description: Migrera certifikat skydda databas krypterings nyckeln för en databas med transparent data kryptering till Azure SQL Managed instance
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: conceptual
author: MladjoA
ms.author: mlandzic
ms.reviewer: carlrab, jovanpop
ms.date: 04/25/2019
ms.openlocfilehash: eb8c794f4817c11d30112fbdf7d754081cc29859
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 05/27/2020
ms.locfileid: "84045790"
---
# <a name="migrate-certificate-of-tde-protected-database-to-azure-sql-managed-instance"></a>Migrera certifikat för TDE-skyddad databas till Azure SQL-hanterad instans
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

När du migrerar en databas som skyddas av [Transparent datakryptering](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption) till en Azure SQL-hanterad instans med hjälp av intern återställnings alternativ måste motsvarande certifikat från SQL Server-instansen migreras innan databasen återställs. Den här artikeln vägleder dig genom processen för manuell migrering av certifikatet till den Azure SQL-hanterade instansen:

> [!div class="checklist"]
>
> * Exportera certifikatet till en Personal Information Exchange-fil (.pfx)
> * Extrahera certifikatet från fil till base-64-sträng
> * Ladda upp den med hjälp av PowerShell-cmdlet

Ett alternativt alternativ som använder en fullständigt hanterad tjänst för sömlös migrering av både TDE-skyddade databaser och motsvarande certifikat finns i [så här migrerar du din lokala databas till Azure SQL-hanterad instans med hjälp av Azure Database migration service](../../dms/tutorial-sql-server-to-managed-instance.md).

> [!IMPORTANT]
> Migrerade certifikat används endast för återställning av den TDE-skyddade databasen. Snart när återställningen är färdig ersätts det migrerade certifikatet med ett annat skydd, antingen tjänst-hanterat certifikat eller asymmetrisk nyckel från nyckel valvet, beroende på vilken typ av transparent data kryptering som du har angett på instansen.

## <a name="prerequisites"></a>Förutsättningar

Du behöver följande för att slutföra stegen i den här artikeln:

* [Pvk2Pfx](https://docs.microsoft.com/windows-hardware/drivers/devtest/pvk2pfx)-kommandoradsverktyget installerat på den lokala servern eller en annan dator med åtkomst till det certifikat som exporterats som en fil. Pvk2Pfx-verktyget är en del av [Enterprise Windows Driver Kit](https://docs.microsoft.com/windows-hardware/drivers/download-the-wdk), en fristående, självständig kommandoradsmiljö.
* [Windows PowerShell](/powershell/scripting/install/installing-windows-powershell) version 5.0 eller senare installerat.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Se till att du har följande:

* Azure PowerShell-modulen [installeras och uppdateras](https://docs.microsoft.com/powershell/azure/install-az-ps).
* [AZ. SQL-modul](https://www.powershellgallery.com/packages/Az.Sql).

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

> [!IMPORTANT]
> PowerShell Azure Resource Manager-modulen stöds fortfarande av Azure SQL-hanterad instans, men all framtida utveckling gäller AZ. SQL-modulen. De här cmdletarna finns i [AzureRM. SQL](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Argumenten för kommandona i AZ-modulen och i AzureRm-modulerna är i stort sett identiska.

Kör följande kommandon i PowerShell för att installera/uppdatera modulen:

```azurepowershell
Install-Module -Name Az.Sql
Update-Module -Name Az.Sql
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Om du behöver installera eller uppgradera kan du läsa informationen i [Installera Azure CLI](/cli/azure/install-azure-cli).

* * *

## <a name="export-tde-certificate-to-a-personal-information-exchange-pfx-file"></a>Exportera TDE-certifikatet till en Personal Information Exchange-fil (.pfx)

Certifikatet kan exporteras direkt från SQL Server-källan eller från certifikatarkivet om det förvaras där.

### <a name="export-certificate-from-the-source-sql-server"></a>Exportera certifikat från SQL Server-källan

Använd följande steg för att exportera certifikat med SQL Server Management Studio och omvandla det till pfx-format. De allmänna namnen *TDE_Cert* och *full_path* används för certifikat- och filnamn samt sökvägar i stegen. De ska ersättas med de faktiska namnen.

1. I SSMS öppnar du ett nytt frågefönster och ansluter till SQL Server-källan.

1. Använd följande skript för att lista TDE-skyddade databaser och hämta namnet på det certifikat som skyddar kryptering på den databas som ska migreras:

   ```sql
   USE master
   GO
   SELECT db.name as [database_name], cer.name as [certificate_name]
   FROM sys.dm_database_encryption_keys dek
   LEFT JOIN sys.certificates cer
   ON dek.encryptor_thumbprint = cer.thumbprint
   INNER JOIN sys.databases db
   ON dek.database_id = db.database_id
   WHERE dek.encryption_state = 3
   ```

   ![lista över TDE-certifikat](./media/tde-certificate-migrate/onprem-certificate-list.png)

1. Kör följande skript för att exportera certifikatet till ett par filer (.cer och .pvk) med informationen för den offentliga nyckeln och den privata nyckeln:

   ```sql
   USE master
   GO
   BACKUP CERTIFICATE TDE_Cert
   TO FILE = 'c:\full_path\TDE_Cert.cer'
   WITH PRIVATE KEY (
     FILE = 'c:\full_path\TDE_Cert.pvk',
     ENCRYPTION BY PASSWORD = '<SomeStrongPassword>'
   )
   ```

   ![säkerhetskopiera TDE-certifikat](./media/tde-certificate-migrate/backup-onprem-certificate.png)

1. Använd PowerShell-konsolen om du vill kopiera certifikatinformationen från ett par nyligen skapade filer till en Personal Information Exchange-fil (.pfx) med Pvk2Pfx-verktyget:

   ```cmd
   .\pvk2pfx -pvk c:/full_path/TDE_Cert.pvk  -pi "<SomeStrongPassword>" -spc c:/full_path/TDE_Cert.cer -pfx c:/full_path/TDE_Cert.pfx
   ```

### <a name="export-certificate-from-certificate-store"></a>Exportera certifikat från certifikatarkiv

Om certifikatet finns i SQL-serverns certifikatarkiv i lokal dator kan det exporteras med följande steg:

1. Öppna PowerShell-konsolen och kör följande kommando för att öppna snapin-modulen för certifikat för Microsoft Management Console:

   ```cmd
   certlm
   ```

2. I MMC-snapin-modulen för certifikat expanderar du sökvägen Personligt -> Certifikat för att se listan över certifikat

3. Högerklicka på certifikatet och klicka på Exportera…

4. Följ guiden för att exportera certifikatet och den privata nyckeln till ett Personal Information Exchange-format

## <a name="upload-certificate-to-azure-sql-managed-instance-using-azure-powershell-cmdlet"></a>Ladda upp certifikat till Azure SQL-hanterad instans med hjälp av Azure PowerShell-cmdlet

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

1. Börja med förberedelsestegen i PowerShell:

   ```azurepowershell
   # import the module into the PowerShell session
   Import-Module Az
   # connect to Azure with an interactive dialog for sign-in
   Connect-AzAccount
   # list subscriptions available and copy id of the subscription target the managed instance belongs to
   Get-AzSubscription
   # set subscription for the session
   Select-AzSubscription <subscriptionId>
   ```

2. När alla förberedelse steg har utförts kör du följande kommandon för att ladda upp det Base-64-kodade certifikatet till den hanterade mål instansen:

   ```azurepowershell
   $fileContentBytes = Get-Content 'C:/full_path/TDE_Cert.pfx' -Encoding Byte
   $base64EncodedCert = [System.Convert]::ToBase64String($fileContentBytes)
   $securePrivateBlob = $base64EncodedCert  | ConvertTo-SecureString -AsPlainText -Force
   $password = "<password>"
   $securePassword = $password | ConvertTo-SecureString -AsPlainText -Force
   Add-AzSqlManagedInstanceTransparentDataEncryptionCertificate -ResourceGroupName "<resourceGroupName>" `
       -ManagedInstanceName "<managedInstanceName>" -PrivateBlob $securePrivateBlob -Password $securePassword
   ```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Du måste först [Konfigurera en Azure Key Vault](/azure/key-vault/key-vault-manage-with-cli2) med *. pfx* -filen.

1. Börja med förberedelsestegen i PowerShell:

   ```azurecli
   # connect to Azure with an interactive dialog for sign-in
   az login

   # list subscriptions available and copy id of the subscription target the managed instance belongs to
   az account list

   # set subscription for the session
   az account set --subscription <subscriptionId>
   ```

1. När alla förberedelse steg har utförts kör du följande kommandon för att ladda upp det Base-64-kodade certifikatet till den hanterade mål instansen:

   ```azurecli
   az sql mi tde-key set --server-key-type AzureKeyVault --kid "<keyVaultId>" `
       --managed-instance "<managedInstanceName>" --resource-group "<resourceGroupName>"
   ```

* * *

Certifikatet är nu tillgängligt för den angivna hanterade instansen och säkerhets kopiering av motsvarande TDE-skyddade databas kan återställas.

## <a name="next-steps"></a>Nästa steg

I den här artikeln lärde du dig att migrera certifikat som skyddar krypteringsnyckeln för databasen med transparent datakryptering, från lokal server eller IaaS SQL-server till Azure SQL-hanterad instans.

Se [återställa en databas säkerhets kopia till en hanterad Azure SQL-instans](restore-sample-database-quickstart.md) om du vill lära dig hur du återställer en databas säkerhets kopia till en hanterad Azure SQL-instans.
