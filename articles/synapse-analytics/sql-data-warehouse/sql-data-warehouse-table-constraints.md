---
title: Primära, externa och unika nycklar
description: Tabell begränsningar stöder i SQL-poolen Synapse i Azure Synapse Analytics
services: synapse-analytics
author: XiaoyuMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 09/05/2019
ms.author: xiaoyul
ms.reviewer: nibruno; jrasnick
ms.custom: seo-lt-2019, azure-synapse
ms.openlocfilehash: f97163d02836442430037e18439bcf0724046332
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/28/2020
ms.locfileid: "80990777"
---
# <a name="primary-key-foreign-key-and-unique-key-in-synapse-sql-pool"></a>Primär nyckel, sekundär nyckel och unik nyckel i Synapse SQL-pool

Lär dig mer om tabell begränsningar i Synapse SQL-pool, inklusive primär nyckel, sekundär nyckel och unik nyckel.

## <a name="table-constraints"></a>Tabellbegränsningar

Synapse SQL-pool stöder följande tabell begränsningar: 
- PRIMÄR nyckel stöds bara om både icke-KLUSTRad och inte framtvingad används.    
- En unik begränsning stöds endast med inte TVINGAd användning.

För syntax, kontrol lera [Alter Table](https://docs.microsoft.com/sql/t-sql/statements/alter-table-transact-sql) och [CREATE TABLE](https://docs.microsoft.com/sql/t-sql/statements/create-table-azure-sql-data-warehouse). 

Sekundär nyckel begränsning stöds inte i Synapse SQL-pool.  


## <a name="remarks"></a>Anmärkningar

Med primär nyckel och/eller unik nyckel kan Synapse SQL-pool generera en optimal körnings plan för en fråga.  Alla värden i en primär nyckel kolumn eller en unik begränsnings kolumn måste vara unika.

När du har skapat en tabell med primär nyckel eller unik begränsning i Synapse SQL-pool måste användarna se till att alla värden i dessa kolumner är unika.  En överträdelse av som kan orsaka att frågan returnerar felaktigt resultat.  Det här exemplet visar hur en fråga kan returnera felaktigt resultat om kolumnen primär nyckel eller unik begränsning innehåller dubblettvärden.  

```sql
 -- Create table t1
CREATE TABLE t1 (a1 INT NOT NULL, b1 INT) WITH (DISTRIBUTION = ROUND_ROBIN)

-- Insert values to table t1 with duplicate values in column a1.
INSERT INTO t1 VALUES (1, 100)
INSERT INTO t1 VALUES (1, 1000)
INSERT INTO t1 VALUES (2, 200)
INSERT INTO t1 VALUES (3, 300)
INSERT INTO t1 VALUES (4, 400)

-- Run this query.  No primary key or unique constraint.  4 rows returned. Correct result.
SELECT a1, COUNT(*) AS total FROM t1 GROUP BY a1

/*
a1          total
----------- -----------
1           2
2           1
3           1
4           1

(4 rows affected)
*/

-- Add unique constraint
ALTER TABLE t1 ADD CONSTRAINT unique_t1_a1 unique (a1) NOT ENFORCED

-- Re-run this query.  5 rows returned.  Incorrect result.
SELECT a1, count(*) AS total FROM t1 GROUP BY a1

/*
a1          total
----------- -----------
2           1
4           1
1           1
3           1
1           1

(5 rows affected)
*/

-- Drop unique constraint.
ALTER TABLE t1 DROP CONSTRAINT unique_t1_a1

-- Add primary key constraint
ALTER TABLE t1 add CONSTRAINT PK_t1_a1 PRIMARY KEY NONCLUSTERED (a1) NOT ENFORCED

-- Re-run this query.  5 rows returned.  Incorrect result.
SELECT a1, COUNT(*) AS total FROM t1 GROUP BY a1

/*
a1          total
----------- -----------
2           1
4           1
1           1
3           1
1           1

(5 rows affected)
*/

-- Manually fix the duplicate values in a1
UPDATE t1 SET a1 = 0 WHERE b1 = 1000

-- Verify no duplicate values in column a1 
SELECT * FROM t1

/*
a1          b1
----------- -----------
2           200
3           300
4           400
0           1000
1           100

(5 rows affected)
*/

-- Add unique constraint
ALTER TABLE t1 add CONSTRAINT unique_t1_a1 UNIQUE (a1) NOT ENFORCED  

-- Re-run this query.  5 rows returned.  Correct result.
SELECT a1, COUNT(*) as total FROM t1 GROUP BY a1

/*
a1          total
----------- -----------
2           1
3           1
4           1
0           1
1           1

(5 rows affected)
*/

-- Drop unique constraint.
ALTER TABLE t1 DROP CONSTRAINT unique_t1_a1

-- Add primary key contraint
ALTER TABLE t1 ADD CONSTRAINT PK_t1_a1 PRIMARY KEY NONCLUSTERED (a1) NOT ENFORCED

-- Re-run this query.  5 rows returned.  Correct result.
SELECT a1, COUNT(*) AS total FROM t1 GROUP BY a1

/*
a1          total
----------- -----------
2           1
3           1
4           1
0           1
1           1

(5 rows affected)
*/

```

## <a name="examples"></a>Exempel

Skapa en Synapse för SQL-pool med en primär nyckel: 

```sql 
CREATE TABLE mytable (c1 INT PRIMARY KEY NONCLUSTERED NOT ENFORCED, c2 INT);
```
Skapa en Synapse för SQL-pool med en unik begränsning:

```sql
CREATE TABLE t6 (c1 INT UNIQUE NOT ENFORCED, c2 INT);
```

## <a name="next-steps"></a>Nästa steg

När du har skapat tabellerna för din Synapse SQL-pool är nästa steg att läsa in data i tabellen. En inläsnings kurs finns i [inläsning av data till SYNAPSE SQL-pool](load-data-wideworldimportersdw.md).
