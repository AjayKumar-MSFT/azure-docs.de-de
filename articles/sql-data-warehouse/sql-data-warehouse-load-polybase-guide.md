---
title: "Anleitung für die Verwendung von PolyBase in SQL Data Warehouse | Microsoft Docs"
description: Richtlinien und Empfehlungen zur Verwendung von PolyBase in SQL Data Warehouse-Szenarios.
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: barbkess
editor: 
ms.assetid: 4757fce1-96b3-48ea-8a51-be1385705f9f
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 10/31/2016
ms.custom: loading
ms.author: cakarst;barbkess
translationtype: Human Translation
ms.sourcegitcommit: 1a82f9f1de27c9197bf61d63dd27c5191fec1544
ms.openlocfilehash: 3e1bf2372762de474310c78d512a6a073c7a01b6
ms.lasthandoff: 01/27/2017



---
# <a name="guide-for-using-polybase-in-sql-data-warehouse"></a>Anleitung für die Verwendung von PolyBase in SQL Data Warehouse
Diese Anleitung bietet praktische Informationen zur Verwendung von PolyBase in SQL Data Warehouse.

Führen Sie zum Einstieg das Tutorial [Laden von Daten mit PolyBase][Load data with PolyBase] durch.

## <a name="rotating-storage-keys"></a>Wechseln von Speicherschlüsseln
Von Zeit zu Zeit sollten Sie den Zugriffsschlüssel für den Blob-Speicher aus Sicherheitsgründen ändern.

Die eleganteste Möglichkeit zum Ausführen dieser Aufgabe bietet ein Prozess, der als "Wechseln der Schlüssel" bezeichnet wird. Sie haben wahrscheinlich bemerkt, dass Sie über zwei Speicherschlüssel für Ihr Blob-Speicherkonto verfügen, sodass Sie wechseln können.

Das Wechseln Ihrer Azure-Speicherkontoschlüssel erfolgt in drei einfachen Schritten:

1. Erstellen Sie zweite datenbankbezogene Anmeldeinformationen basierend auf dem sekundären Speicherzugriffsschlüssel.
2. Erstellen Sie eine zweite externe Datenquelle basierend auf diesen neuen Anmeldeinformationen.
3. Löschen und erstellen Sie die externen Tabellen, die auf die neue externe Datenquelle verweisen.

Wenn Sie alle Ihre externen Tabellen zur neuen externen Datenquelle migriert haben, können Sie die Bereinigungsaufgaben ausführen:

1. Löschen Sie die erste externe Datenquelle.
2. Löschen Sie die ersten datenbankbezogenen Anmeldeinformationen basierend auf dem primären Speicherzugriffsschlüssel.
3. Melden Sie sich bei Azure an, und generieren Sie den primären Zugriffsschlüssel neu, sodass er für das nächste Mal bereitsteht.

## <a name="query-azure-blob-storage-data"></a>Abfragen von Daten im Azure-Blob-Speicher
Bei Abfragen für externe Tabellen wird einfach der Tabellenname verwendet, als handle es sich um relationale Tabellen.

```sql
-- Query Azure storage resident data via external table.
SELECT * FROM [ext].[CarSensor_Data]
;
```

> [!NOTE]
> Eine Abfrage für eine externe Tabelle kann mit folgendem Fehler fehlschlagen: *Abfrage abgebrochen – der maximale Ablehnungsgrenzwert wurde beim Lesen aus einer externen Quelle erreicht*. Dies weist darauf hin, dass die externen Daten *fehlerhafte* Datensätze enthalten. Ein Datensatz gilt als fehlerhaft, wenn die tatsächlichen Datentypen/Anzahl der Spalten nicht den Spaltendefinitionen der externen Tabelle entsprechen, oder wenn die Daten nicht im angegebenen Format der externen Datei vorliegen. Um dieses Problem zu beheben, stellen Sie sicher, dass die Formatdefinitionen der externen Tabelle und Datei richtig und Ihre externen Daten diesen Definitionen entsprechen. Für den Fall, dass eine Teilmenge der Datensätze für externe Daten fehlerhaft sind, können Sie diese Datensätze für Ihre Abfragen mit den Ablehnungsoptionen in „CREATE EXTERNAL TABLE DDL“ ablehnen.
> 
> 

## <a name="load-data-from-azure-blob-storage"></a>Laden von Daten aus dem Azure-Blob-Speicher
In diesem Beispiel werden Daten aus dem Azure-Blob-Speicher in die SQL Data Warehouse-Datenbank geladen.

Durch direktes Speichern der Daten entfällt bei Abfragen die für die Datenübertragung verwendete Zeit. Durch Speichern der Daten mit einem Columnstore-Index verbessert sich die Abfrageleistung bei Analyseabfragen um das maximal 10-Fache.

In diesem Beispiel werden die Daten mit der CREATE TABLE AS SELECT-Anweisung geladen. Die neue Tabelle erbt die in der Abfrage benannten Spalten. Sie erbt die Datentypen dieser Spalten aus der Definition der externen Tabelle.

CREATE TABLE AS SELECT ist eine sehr leistungsfähige Transact-SQL-Anweisung, mit der die Daten parallel auf alle Compute-Knoten Ihres SQL Data Warehouse geladen werden.  Sie wurde ursprünglich für das MPP-Modul (Massively Parallel Processing) in Analytics Platform System entwickelt und wird jetzt in SQL Data Warehouse verwendet.

```sql
-- Load data from Azure blob storage to SQL Data Warehouse

CREATE TABLE [dbo].[Customer_Speed]
WITH
(   
    CLUSTERED COLUMNSTORE INDEX
,    DISTRIBUTION = HASH([CarSensor_Data].[CustomerKey])
)
AS
SELECT *
FROM   [ext].[CarSensor_Data]
;
```

Informationen hierzu finden Sie unter [CREATE TABLE AS SELECT (Transact-SQL)][CREATE TABLE AS SELECT (Transact-SQL)].

## <a name="create-statistics-on-newly-loaded-data"></a>Erstellen von Statistiken für die neu geladenen Daten
Azure SQL Data Warehouse bietet noch keine Unterstützung für die automatische Erstellung oder die automatische Aktualisierung von Statistiken.  Um die beste Leistung bei Abfragen zu erhalten, ist es wichtig, dass die Statistiken für alle Spalten aller Tabellen nach dem ersten Laden oder nach allen wesentlichen Datenänderungen erstellt werden.  Eine ausführliche Erläuterung der Statistik finden Sie im Thema [Statistiken][Statistics] in der Entwicklungsgruppe der Themen.  Es folgt ein kurzes Beispiel, wie Sie Statistiken für die in diesem Beispiel geladene Tabelle erstellen können.

```sql
create statistics [SensorKey] on [Customer_Speed] ([SensorKey]);
create statistics [CustomerKey] on [Customer_Speed] ([CustomerKey]);
create statistics [GeographyKey] on [Customer_Speed] ([GeographyKey]);
create statistics [Speed] on [Customer_Speed] ([Speed]);
create statistics [YearMeasured] on [Customer_Speed] ([YearMeasured]);
```

## <a name="export-data-to-azure-blob-storage"></a>Exportieren von Daten in Azure Blob Storage
In diesem Abschnitt wird veranschaulicht, wie Sie Daten aus SQL Data Warehouse in Azure-BLOB-Speicher exportieren. In diesem Beispiel wird CREATE EXTERNAL TABLE AS SELECT verwendet. Dies ist eine sehr leistungsfähige Transact-SQL-Anweisung, mit der die Daten parallel von allen Compute-Knoten exportiert werden.

Im folgenden Beispiel wird die externe Tabelle „Weblogs2014“ erstellt, indem Spaltendefinitionen und Daten aus der Tabelle „dbo.Weblogs“ verwendet werden. Die Definition der externen Tabelle wird in SQL Data Warehouse gespeichert, und die Ergebnisse der SELECT-Anweisung werden in das Verzeichnis „/archive/log2014/“ unter dem BLOB-Container exportiert, der von der Datenquelle angegeben wird. Die Daten werden im angegebenen Textdateiformat exportiert.

```sql
CREATE EXTERNAL TABLE Weblogs2014 WITH
(
    LOCATION='/archive/log2014/',
    DATA_SOURCE=azure_storage,
    FILE_FORMAT=text_file_format
)
AS
SELECT
    Uri,
    DateRequested
FROM
    dbo.Weblogs
WHERE
    1=1
    AND DateRequested > '12/31/2013'
    AND DateRequested < '01/01/2015';
```


## <a name="working-around-the-polybase-utf-8-requirement"></a>Umgehen der UTF-8-Anforderung von PolyBase
Derzeit unterstützt PolyBase das Laden von Datendateien mit UTF-8-Codierung. Da UTF-8 die gleiche Zeichencodierung wie ASCII-verwendet, unterstützt PolyBase auch das Laden ASCII-codierter Daten. PolyBase unterstützt jedoch keine andere Zeichencodierung, wie z. B. UTF-16/Unicode- oder erweiterte ASCII-Zeichen. Erweiterte ASCII-Zeichen umfassen auch Zeichen mit Akzenten, z. B. die im Deutschen häufig verwendeten Umlaute.

Die beste Möglichkeit zum Umgehen dieser Anforderung ist das erneute Schreiben mit UTF-8-Codierung.

Dazu gibt es verschiedene Möglichkeiten. Nachfolgend sind zwei Methoden angegeben, bei denen Powershell verwendet wird:

### <a name="simple-example-for-small-files"></a>Einfaches Beispiel für kleine Dateien
Nachfolgend sehen Sie ein einfaches einzeiliges Powershell-Skript, das die Datei erstellt.

```PowerShell
Get-Content <input_file_name> -Encoding Unicode | Set-Content <output_file_name> -Encoding utf8
```

Obwohl dies eine einfache Methode zum erneuten Codieren der Daten darstellt, ist sie keinesfalls die effizienteste. Das folgende  Beispiel für E/A-Streaming ist sehr viel schneller und erzielt dasselbe Ergebnis.

### <a name="io-streaming-example-for-larger-files"></a>E/A-Streaming-Beispiel für größere Dateien
Das folgende Codebeispiel ist komplexer, doch da die Datenzeilen aus der Quelle in das Ziel gestreamt werden, ist es wesentlich effizienter. Verwenden Sie diese Methode für größere Dateien.

```PowerShell
#Static variables
$ascii = [System.Text.Encoding]::ASCII
$utf16le = [System.Text.Encoding]::Unicode
$utf8 = [System.Text.Encoding]::UTF8
$ansi = [System.Text.Encoding]::Default
$append = $False

#Set source file path and file name
$src = [System.IO.Path]::Combine("C:\input_file_path\","input_file_name.txt")

#Set source file encoding (using list above)
$src_enc = $ansi

#Set target file path and file name
$tgt = [System.IO.Path]::Combine("C:\output_file_path\","output_file_name.txt")

#Set target file encoding (using list above)
$tgt_enc = $utf8

$read = New-Object System.IO.StreamReader($src,$src_enc)
$write = New-Object System.IO.StreamWriter($tgt,$append,$tgt_enc)

while ($read.Peek() -ne -1)
{
    $line = $read.ReadLine();
    $write.WriteLine($line);
}
$read.Close()
$read.Dispose()
$write.Close()
$write.Dispose()
```

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum Verschieben von Daten nach SQL Data Warehouse finden Sie unter der [Übersicht über die Datenmigration][data migration overview].

<!--Image references-->

<!--Article references-->
[Load data with bcp]: ./sql-data-warehouse-load-with-bcp.md
[Load data with PolyBase]: ./sql-data-warehouse-get-started-load-with-polybase.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md
[data migration overview]: ./sql-data-warehouse-overview-migrate.md

<!--MSDN references-->
[supported source/sink]: https://msdn.microsoft.com/library/dn894007.aspx
[copy activity]: https://msdn.microsoft.com/library/dn835035.aspx
[SQL Server destination adapter]: https://msdn.microsoft.com/library/ms141095.aspx
[SSIS]: https://msdn.microsoft.com/library/ms141026.aspx

[CREATE EXTERNAL DATA SOURCE (Transact-SQL)]: https://msdn.microsoft.com/library/dn935022.aspx
[CREATE EXTERNAL FILE FORMAT (Transact-SQL)]: https://msdn.microsoft.com/library/dn935026.aspx
[CREATE EXTERNAL TABLE (Transact-SQL)]: https://msdn.microsoft.com/library/dn935021.aspx

[DROP EXTERNAL DATA SOURCE (Transact-SQL)]: https://msdn.microsoft.com/library/mt146367.aspx
[DROP EXTERNAL FILE FORMAT (Transact-SQL)]: https://msdn.microsoft.com/library/mt146379.aspx
[DROP EXTERNAL TABLE (Transact-SQL)]: https://msdn.microsoft.com/library/mt130698.aspx

[CREATE TABLE AS SELECT (Transact-SQL)]: https://msdn.microsoft.com/library/mt204041.aspx
[INSERT...SELECT (Transact-SQL)]: https://msdn.microsoft.com/library/ms174335.aspx
[CREATE MASTER KEY (Transact-SQL)]: https://msdn.microsoft.com/library/ms174382.aspx
[CREATE CREDENTIAL (Transact-SQL)]: https://msdn.microsoft.com/library/ms189522.aspx
[CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)]: https://msdn.microsoft.com/library/mt270260.aspx
[DROP CREDENTIAL (Transact-SQL)]: https://msdn.microsoft.com/library/ms189450.aspx

<!-- External Links -->

