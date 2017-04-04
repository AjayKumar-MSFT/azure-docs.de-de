---
title: "Datentypen für Tabellen in SQL Data Warehouse | Microsoft Docs"
description: "Es werden die ersten Schritte für die Verwendung von Datentypen für Azure SQL Data Warehouse-Tabellen beschrieben."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: barbkess
editor: 
ms.assetid: d4a1f0a3-ba9f-44b9-95f6-16a4f30746d6
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: tables
ms.date: 10/31/2016
ms.author: jrj;barbkess
translationtype: Human Translation
ms.sourcegitcommit: 424d8654a047a28ef6e32b73952cf98d28547f4f
ms.openlocfilehash: 7757de96601c426ff07e94cfa0c12d4dec8f06f5
ms.lasthandoff: 03/22/2017


---
# <a name="data-types-for-tables-in-sql-data-warehouse"></a>Datentypen für Tabellen in SQL Data Warehouse
> [!div class="op_single_selector"]
> * [Übersicht][Overview]
> * [Datentypen][Data Types]
> * [Verteilen][Distribute]
> * [Index][Index]
> * [Partition][Partition]
> * [Statistiken][Statistics]
> * [Temporär][Temporary]
> 
> 

SQL Data Warehouse unterstützt die gängigsten Datentypen.  Unten ist eine Liste mit den Datentypen angegeben, die von SQL Data Warehouse unterstützt werden.  Weitere Informationen zur Unterstützung von Datentypen finden Sie unter [CREATE TABLE][create table].

| **Unterstützte Datentypen** |  |  |
| --- | --- | --- |
| [bigint][bigint] |[decimal][decimal] |[smallint][smallint] |
| [binary][binary] |[float][float] |[smallmoney][smallmoney] |
| [bit][bit] |[int][int] |[sysname][sysname] |
| [char][char] |[money][money] |[time][time] |
| [date][date] |[nchar][nchar] |[tinyint][tinyint] |
| [datetime][datetime] |[nvarchar][nvarchar] |[uniqueidentifier][uniqueidentifier] |
| [datetime2][datetime2] |[real][real] |[varbinary][varbinary] |
| [datetimeoffset][datetimeoffset] |[smalldatetime][smalldatetime] |[varchar][varchar] |

## <a name="data-type-best-practices"></a>Bewährte Methoden für Datentypen
 Beim Definieren Ihrer Spaltentypen verbessert sich die Abfrageleistung, wenn Sie den kleinsten Datentyp verwenden, der für Ihre Daten unterstützt wird. Dies ist besonders für CHAR- und VARCHAR-Spalten wichtig. Wenn der längste Wert in einer Spalte 25 Zeichen lang ist, sollten Sie die Spalte VARCHAR(25) definieren. Vermeiden Sie es, für alle Zeichenspalten eine hohe Standardlänge zu definieren. Definieren Sie darüber hinaus Spalten als VARCHAR, sofern dies ausreicht, anstatt [NVARCHAR][NVARCHAR] zu verwenden.  Verwenden Sie anstelle von NVARCHAR(MAX) oder VARCHAR(MAX) nach Möglichkeit NVARCHAR(4000) oder VARCHAR(8000).

## <a name="polybase-limitation"></a>PolyBase-Beschränkung
Wenn Sie die Tabellen mit Polybase laden möchten, stellen Sie sicher, dass die Länge der Daten 1 MB nicht überschreitet.  Sie können zwar eine Zeile mit Daten variabler Länge definieren, bei der diese Breite überschritten wird und Zeilen mit BCP geladen werden können, aber Sie können PolyBase nicht zum Laden dieser Daten verwenden.  

## <a name="unsupported-data-types"></a>Nicht unterstützte Datentypen
Wenn Sie die Datenbank von einer anderen SQL-Plattform wie Azure SQL-Datenbank migrieren, begegnen Ihnen während der Migration unter Umständen einige Datentypen, die in SQL Data Warehouse nicht unterstützt werden.  Unten sind nicht unterstützte Datentypen und einige Alternativen angegeben, die Sie anstelle von nicht unterstützten Datentypen verwenden können.

| Datentyp | Problemumgehung |
| --- | --- |
| [geometry][geometry] |[varbinary][varbinary] |
| [geography][geography] |[varbinary][varbinary] |
| [hierarchyid][hierarchyid] |[nvarchar][nvarchar](4000) |
| [image][ntext,text,image] |[varbinary][varbinary] |
| [text][ntext,text,image] |[varchar][varchar] |
| [ntext][ntext,text,image] |[nvarchar][nvarchar] |
| [sql_variant][sql_variant] |Spalte in mehrere Spalten mit starker Typisierung unterteilen. |
| [table][table] |In temporäre Tabellen konvertieren. |
| [timestamp][timestamp] |Passen Sie den Code an, damit [datetime2][datetime2] und die `CURRENT_TIMESTAMP`-Funktion verwendet werden.  Nur Konstanten werden als Standardwerte unterstützt. Daher kann „current_timestamp“ nicht als Standardeinschränkung definiert werden. Wenn Sie Zeilenversionswerte aus einer Spalte vom Typ timestamp migrieren müssen, sollten Sie [BINARY][BINARY](8) oder [VARBINARY][BINARY](8) für NOT NULL- oder NULL-Zeilenversionswerte verwenden. |
| [xml][xml] |[varchar][varchar] |
| [Benutzerdefinierte Typen][user defined types] |Zurück in native Typen konvertieren, falls möglich |
| Standardwerte |Standardwerte unterstützen nur Literale und Konstanten.  Nicht deterministische Ausdrücke oder Funktionen, z. B. `GETDATE()` oder `CURRENT_TIMESTAMP`, werden nicht unterstützt. |

Der unten angegebene SQL-Code kann in Ihrer aktuellen SQL-Datenbank ausgeführt werden, um Spalten zu identifizieren, die von Azure SQL Data Warehouse nicht unterstützt werden:

```sql
SELECT  t.[name], c.[name], c.[system_type_id], c.[user_type_id], y.[is_user_defined], y.[name]
FROM sys.tables  t
JOIN sys.columns c on t.[object_id]    = c.[object_id]
JOIN sys.types   y on c.[user_type_id] = y.[user_type_id]
WHERE y.[name] IN ('geography','geometry','hierarchyid','image','text','ntext','sql_variant','timestamp','xml')
 AND  y.[is_user_defined] = 1;
```

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie in den Artikeln [Übersicht über Tabellen][Overview], [Verteilen einer Tabelle][Distribute], [Indizieren einer Tabelle][Index], [Partitionieren einer Tabelle][Partition], [Verwalten von Statistiken für Tabellen][Statistics] und [Temporäre Tabellen][Temporary].  Weitere Informationen zu Best Practices finden Sie unter [SQL Data Warehouse – Best Practices][SQL Data Warehouse Best Practices].

<!--Image references-->

<!--Article references-->
[Overview]: ./sql-data-warehouse-tables-overview.md
[Data Types]: ./sql-data-warehouse-tables-data-types.md
[Distribute]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md
[Temporary]: ./sql-data-warehouse-tables-temporary.md
[SQL Data Warehouse Best Practices]: ./sql-data-warehouse-best-practices.md

<!--MSDN references-->

<!--Other Web references-->
[create table]: https://msdn.microsoft.com/library/mt203953.aspx
[bigint]: https://msdn.microsoft.com/library/ms187745.aspx
[binary]: https://msdn.microsoft.com/library/ms188362.aspx
[bit]: https://msdn.microsoft.com/library/ms177603.aspx
[char]: https://msdn.microsoft.com/library/ms176089.aspx
[date]: https://msdn.microsoft.com/library/bb630352.aspx
[datetime]: https://msdn.microsoft.com/library/ms187819.aspx
[datetime2]: https://msdn.microsoft.com/library/bb677335.aspx
[datetimeoffset]: https://msdn.microsoft.com/library/bb630289.aspx
[decimal]: https://msdn.microsoft.com/library/ms187746.aspx
[float]: https://msdn.microsoft.com/library/ms173773.aspx
[geometry]: https://msdn.microsoft.com/library/cc280487.aspx
[geography]: https://msdn.microsoft.com/library/cc280766.aspx
[hierarchyid]: https://msdn.microsoft.com/library/bb677290.aspx
[int]: https://msdn.microsoft.com/library/ms187745.aspx
[money]: https://msdn.microsoft.com/library/ms179882.aspx
[nchar]: https://msdn.microsoft.com/library/ms186939.aspx
[nvarchar]: https://msdn.microsoft.com/library/ms186939.aspx
[ntext,text,image]: https://msdn.microsoft.com/library/ms187993.aspx
[real]: https://msdn.microsoft.com/library/ms173773.aspx
[smalldatetime]: https://msdn.microsoft.com/library/ms182418.aspx
[smallint]: https://msdn.microsoft.com/library/ms187745.aspx
[smallmoney]: https://msdn.microsoft.com/library/ms179882.aspx
[sql_variant]: https://msdn.microsoft.com/library/ms173829.aspx
[sysname]: https://msdn.microsoft.com/library/ms186939.aspx
[table]: https://msdn.microsoft.com/library/ms175010.aspx
[time]: https://msdn.microsoft.com/library/bb677243.aspx
[timestamp]: https://msdn.microsoft.com/library/ms182776.aspx
[tinyint]: https://msdn.microsoft.com/library/ms187745.aspx
[uniqueidentifier]: https://msdn.microsoft.com/library/ms187942.aspx
[varbinary]: https://msdn.microsoft.com/library/ms188362.aspx
[varchar]: https://msdn.microsoft.com/library/ms186939.aspx
[xml]: https://msdn.microsoft.com/library/ms187339.aspx
[user defined types]: https://msdn.microsoft.com/library/ms131694.aspx

