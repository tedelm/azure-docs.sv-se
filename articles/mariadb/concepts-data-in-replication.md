---
title: Data-i replikering – Azure Database for MariaDB
description: Lär dig mer om att använda datareplikering för att synkronisera från en extern server till tjänsten Azure Database for MariaDB.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 3/18/2020
ms.openlocfilehash: 1fbcc1fb27d5e6df4641f79c0d634580f74000b8
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/28/2020
ms.locfileid: "79532068"
---
# <a name="replicate-data-into-azure-database-for-mariadb"></a>Replikera data till Azure Database for MariaDB

Med datareplikering kan du synkronisera data från en MariaDB-server som körs lokalt, på virtuella datorer eller i databastjänster som hanteras av andra molnleverantörer i tjänsten Azure Database for MariaDB-tjänsten. Datareplikering baseras på positionsbaserad replikering med en binär loggfil (binlog) som är inbyggd i MariaDB. Mer information om BinLog-replikering finns i [Översikt över BinLog-replikering](https://mariadb.com/kb/en/library/replication-overview/).

## <a name="when-to-use-data-in-replication"></a>När du ska använda Datareplikering
Huvud scenarierna för att överväga att använda Datareplikering är:

- **Synkronisering av hybrid data:** Med Datareplikering kan du synkronisera data mellan dina lokala servrar och Azure Database for MariaDB. Den här synkroniseringen är användbar för att skapa hybrid program. Den här metoden kan överklaga när du har en befintlig lokal databas server, men vill flytta data till en region närmare slutanvändarna.
- **Synkronisering med flera moln:** För komplexa moln lösningar använder du Datareplikering för att synkronisera data mellan Azure Database for MariaDB och olika moln leverantörer, inklusive virtuella datorer och databas tjänster som finns i molnet.

## <a name="limitations-and-considerations"></a>Begränsningar och överväganden

### <a name="data-not-replicated"></a>Data har inte repliker ATS
[*MySQL system-databasen*](https://mariadb.com/kb/en/library/the-mysql-database-tables/) på huvud servern replikeras inte. Ändringar av konton och behörigheter på huvud servern replikeras inte. Om du skapar ett konto på huvud servern och det här kontot behöver åtkomst till replik servern, skapar du samma konto manuellt på replik Server sidan. Information om vilka tabeller som finns i system-databasen finns i [MariaDB-dokumentationen](https://mariadb.com/kb/en/library/the-mysql-database-tables/).

### <a name="requirements"></a>Krav
- Huvud Server versionen måste vara minst MariaDB version 10,2.
- Huvud-och replik Server versionerna måste vara desamma. Till exempel måste båda vara MariaDB version 10,2.
- Varje tabell måste ha en primär nyckel.
- Huvud servern bör använda InnoDB-motorn.
- Användaren måste ha behörighet att konfigurera binär loggning och skapa nya användare på huvud servern.
- Om huvud servern har SSL aktiverat kontrollerar du att det SSL-CA-certifikat som har angetts för domänen har inkluderats i den `mariadb.az_replication_change_master` lagrade proceduren. Se följande [exempel](https://docs.microsoft.com/azure/mariadb/howto-data-in-replication#link-the-master-and-replica-servers-to-start-data-in-replication) och `master_ssl_ca` parametern.
- Se till att huvudserverns IP-adress har lagts till i Azure Database for MariaDB-replikserverns brandväggsregler. Uppdatera brandväggsregler med hjälp av [Azure-portalen](https://docs.microsoft.com/azure/mariadb/howto-manage-firewall-portal) eller [Azure CLI](https://docs.microsoft.com/azure/mariadb/howto-manage-firewall-cli).
- Se till att datorn som är värd för huvudservern tillåter både inkommande och utgående trafik på port 3306.
- Se till att huvud servern har en **offentlig IP-adress**, att DNS är offentligt tillgängligt eller har ett fullständigt kvalificerat domän namn (FQDN).

### <a name="other"></a>Annat
- Datareplikering stöds bara i Generell användning och minnesoptimerade pris nivåer.

## <a name="next-steps"></a>Nästa steg
- Lär dig hur du [konfigurerar replikering av data](howto-data-in-replication.md).
