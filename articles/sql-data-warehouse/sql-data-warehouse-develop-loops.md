---
title: Nutzen von T-SQL-Schleifen in Azure SQL Data Warehouse | Microsoft-Dokumentation
description: "Tipps zu Transact-SQL-Schleifen und zum Ersetzen von Cursorn in Azure SQL Data Warehouse für die Entwicklung von Lösungen"
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: f3384b81-b943-431b-bc73-90e47e4c195f
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: t-sql
ms.date: 10/31/2016
ms.author: jrj;barbkess
translationtype: Human Translation
ms.sourcegitcommit: 43ab6a2f71ab51c50847b1ba5249f51c48e03fea
ms.openlocfilehash: d6409f1eb87787e5e023aa53b7b264116c9d8026
ms.lasthandoff: 01/24/2017


---
# <a name="loops-in-sql-data-warehouse"></a>Schleifen in SQL Data Warehouse
SQL Data Warehouse unterstützt die [WHILE][WHILE]-Schleife für die wiederholte Ausführung von Anweisungsblöcken. Die Schleife wird so lange ausgeführt, wie die angegebenen Bedingungen wahr sind oder bis die Schleife im Code mit dem Schlüsselwort `BREAK` gezielt beendet wird. Schleifen sind besonders nützlich, um im SQL-Code definierte Cursor zu ersetzen. Glücklicherweise sind fast alle Cursor, die per SQL-Code geschrieben werden, Nur-Lese-Cursor für den schnellen Vorlauf. Daher sind [WHILE] -Schleifen eine hervorragende Alternative, wenn Sie einen Cursor ersetzen müssen.

## <a name="leveraging-loops-and-replacing-cursors-in-sql-data-warehouse"></a>Nutzen von Schleifen und Ersetzen von Cursorn in SQL Data Warehouse
Stellen Sie sich vorher aber unbedingt die folgende Frage: „Kann dieser Cursor so umgeschrieben werden, dass satzbasierte Vorgänge verwendet werden?“ In vielen Fällen lautet die Antwort "Ja", daher ist dies häufig der beste Ansatz. Ein satzbasierter Vorgang wird oft erheblich schneller als ein Durchlauf Zeile für Zeile durchgeführt.

Nur-Lese-Cursor für den schnellen Vorlauf können leicht durch ein Schleifenkonstrukt ersetzt werden. Im Folgenden finden Sie ein einfaches Beispiel: In diesem Codebeispiel wird die Statistik für jede Tabelle der Datenbank aktualisiert. Indem die Tabellen mit der Schleife durchlaufen werden, kann jeder Befehl der Reihe nach ausgeführt werden.

Erstellen Sie zuerst eine temporäre Tabelle mit einer eindeutigen Zeilenzahl zum Identifizieren der einzelnen Anweisungen:

```
CREATE TABLE #tbl
WITH
( DISTRIBUTION = ROUND_ROBIN
)
AS
SELECT  ROW_NUMBER() OVER(ORDER BY (SELECT NULL)) AS Sequence
,       [name]
,       'UPDATE STATISTICS '+QUOTENAME([name]) AS sql_code
FROM    sys.tables
;
```

Initialisieren Sie als Nächstes die Variablen, die zum Durchführen des Schleifenvorgangs erforderlich sind:

```
DECLARE @nbr_statements INT = (SELECT COUNT(*) FROM #tbl)
,       @i INT = 1
;
```

Jetzt können Sie die Anweisungen per Schleife durchlaufen und einzeln ausführen:

```
WHILE   @i <= @nbr_statements
BEGIN
    DECLARE @sql_code NVARCHAR(4000) = (SELECT sql_code FROM #tbl WHERE Sequence = @i);
    EXEC    sp_executesql @sql_code;
    SET     @i +=1;
END
```

Abschließend können Sie die temporäre Tabelle verwerfen, die Sie im ersten Schritt erstellt haben.

```
DROP TABLE #tbl;
```


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->

## <a name="next-steps"></a>Nächste Schritte
Weitere Hinweise zur Entwicklung finden Sie in der [Entwicklungsübersicht][development overview].

<!--Image references-->

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[WHILE]: https://msdn.microsoft.com/library/ms178642.aspx


<!--Other Web references-->

