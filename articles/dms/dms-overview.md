---
title: Vad är Azure Database Migration Service?
description: Översikt över Azure Database Migration Service, som ger sömlös migrering från många databas källor till Azure-dataplattformar.
services: database-migration
author: pochiraju
ms.author: rajpo
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.topic: article
ms.date: 02/20/2020
ms.openlocfilehash: 8607f1a687f2979c37c67099874288c6c5ead164
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/28/2020
ms.locfileid: "77647487"
---
# <a name="what-is-azure-database-migration-service"></a>Vad är Azure Database Migration Service?

Azure Database Migration Service är en fullständigt hanterad tjänst som är utformad för att möjliggöra sömlös migrering från flera databas källor till Azure-dataplattformar med minimal stillestånds tid (online-migreringar).

## <a name="migrate-databases-to-azure-with-familiar-tools"></a>Migrera databaser till Azure med välbekanta verktyg

Azure Database Migration Service integrerar några av funktionerna hos våra befintliga verktyg och tjänster. Den ger kunderna en omfattande lösning med hög tillgänglighet. Tjänsten använder [Data Migration Assistant](https://aka.ms/dma) för att generera utvärderings rapporter som ger rekommendationer för att vägleda dig genom de ändringar som krävs innan migreringen utförs. Det är upp till dig att utföra alla åtgärder som krävs. När du är redo att påbörja migreringsprocessen utför Azure Database Migration Service alla nödvändiga steg. Du kan tryggt starta och sedan glömma dina migreringsprojekt i vetskap om att processen sker enligt Microsofts bästa praxis.

> [!NOTE]
> Om du använder Azure Database Migration Service för att utföra en online-migrering måste du skapa en instans utifrån pris nivån Premium.

## <a name="regional-availability"></a>Regional tillgänglighet

Uppdaterad information om regional tillgänglighet för Azure Database Migration Service finns i [produkt tillgänglighet per region](https://azure.microsoft.com/global-infrastructure/services/?products=database-migration).

## <a name="pricing"></a>Prissättning

Uppdaterad information om Azure Database Migration Service priser finns i [Azure Database migration service prissättning](https://azure.microsoft.com/pricing/details/database-migration/).

## <a name="next-steps"></a>Nästa steg

* [Status för migrerings scenarier som stöds av Azure Database migration service](resource-scenario-status.md).
* [Skapa en instans av Azure Database migration service med hjälp av Azure Portal](quickstart-create-data-migration-service-portal.md).
* [Migrera SQL Server till Azure SQL Database](tutorial-sql-server-to-azure-sql.md).
* [Översikt över krav för att använda Azure Database migration service](pre-reqs.md).
* [Vanliga frågor och svar om hur du använder Azure Database migration service](faq.md).
* [Tjänster och verktyg som är tillgängliga för data migrations scenarier](dms-tools-matrix.md).
