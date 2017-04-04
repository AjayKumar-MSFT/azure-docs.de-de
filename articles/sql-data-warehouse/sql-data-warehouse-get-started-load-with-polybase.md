---
title: Tutorial zur Verwendung von PolyBase in SQL Data Warehouse | Microsoft Docs
description: Informationen zu PolyBase und Hinweise zur Verwendung in Data Warehouse-Szenarios.
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: jhubbard
editor: 
ms.assetid: 0a0103b4-ddd6-4d1e-87be-4965d6e99f3f
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: loading
ms.date: 03/01/2017
ms.author: cakarst
translationtype: Human Translation
ms.sourcegitcommit: cea53acc33347b9e6178645f225770936788f807
ms.openlocfilehash: 1e3cd48aebf6f00802863c1a1b20fcb625e03c25
ms.lasthandoff: 03/03/2017



---
# <a name="load-data-with-polybase-in-sql-data-warehouse"></a>Laden von Daten mit PolyBase in SQL Data Warehouse
> [!div class="op_single_selector"]
> * [Redgate](sql-data-warehouse-load-with-redgate.md)  
> * [Data Factory](sql-data-warehouse-get-started-load-with-azure-data-factory.md)  
> * [PolyBase](sql-data-warehouse-get-started-load-with-polybase.md)  
> * [BCP](sql-data-warehouse-load-with-bcp.md)
> 
> 

Dieses Tutorial zeigt, wie Daten mit AzCopy und PolyBase in SQL Data Warehouse geladen werden. Wenn Sie es abgeschlossen haben, können Sie folgende Aufgaben ausführen:

* Verwenden von AzCopy zum Kopieren von Daten in Azure-Blobspeicher
* Erstellen von Datenbankobjekten zum Definieren der Daten
* Ausführen einer T-SQL-Abfrage zum Laden der Daten

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Loading-data-with-PolyBase-in-Azure-SQL-Data-Warehouse/player]
> 
> 

## <a name="prerequisites"></a>Voraussetzungen
Für dieses Tutorial ist Folgendes erforderlich:

* Eine SQL Data Warehouse-Datenbank.
* Ein Azure-Speicherkonto des Typs „Lokal redundanter Standardspeicher“ (Standard-LRS), „Georedundanter Standardspeicher“ (Standard-GRS) oder „Georedundanter Standardspeicher mit Lesezugriff“ (Standard-RAGRS).
* Befehlszeilenprogramm AzCopy. Laden Sie die [neueste Version von AzCopy][latest version of AzCopy] herunter, und installieren Sie sie. AzCopy wird zusammen mit den Microsoft Azure Storage-Tools installiert.
  
    ![Azure-Speichertools](./media/sql-data-warehouse-get-started-load-with-polybase/install-azcopy.png)

## <a name="step-1-add-sample-data-to-azure-blob-storage"></a>Schritt 1: Hinzufügen von Beispieldaten zum Azure-Blobspeicher
Um Daten laden zu können, müssen wir einige Beispieldaten in einen Azure-Blobspeicher einfügen. In diesem Schritt füllen wir einen Azure-Blobspeicher mit Beispieldaten. Wir werden diese Beispieldaten später mit PolyBase in die SQL Data Warehouse-Datenbank laden.

### <a name="a-prepare-a-sample-text-file"></a>A: Vorbereiten einer Beispieltextdatei
So bereiten Sie eine Beispieltextdatei vor

1. Öffnen Sie Editor, und kopieren Sie die folgenden Datenzeilen in eine neue Datei. Speichern Sie diese im lokalen temporären Verzeichnis als „%temp%\DimDate2.txt“.

```
20150301,1,3
20150501,2,4
20151001,4,2
20150201,1,3
20151201,4,2
20150801,3,1
20150601,2,4
20151101,4,2
20150401,2,4
20150701,3,1
20150901,3,1
20150101,1,3
```

### <a name="b-find-your-blob-service-endpoint"></a>B. Suchen des Endpunkts Ihres Blobdiensts
So suchen Sie den Endpunkt Ihres Blobdiensts

1. Wählen Sie im Azure-Portal **Durchsuchen** > **Speicherkonten**.
2. Klicken Sie auf das Speicherkonto, das Sie verwenden möchten.
3. Klicken Sie auf dem Blatt des Speicherkontos auf „Blobs“.
   
    ![Auf „Blobs“ klicken](./media/sql-data-warehouse-get-started-load-with-polybase/click-blobs.png)
4. Bewahren Sie die Endpunkt-URL Ihres Blobdiensts für später auf.
   
    ![Endpunkt des Blobdiensts](./media/sql-data-warehouse-get-started-load-with-polybase/blob-service.png)

### <a name="c-find-your-azure-storage-key"></a>C. Suchen des Azure-Speicherschlüssels
So suchen Sie den Azure-Speicherschlüssel

1. Wählen Sie im Azure-Portal **Durchsuchen** > **Speicherkonten**.
2. Klicken Sie auf das Speicherkonto, das Sie verwenden möchten.
3. Wählen Sie **Alle Einstellungen** > **Zugriffsschlüssel** aus.
4. Klicken Sie auf das Kopierfeld, um einen der Zugriffsschlüssel in die Zwischenablage zu kopieren.
   
    ![Azure-Speicherschlüssel kopieren](./media/sql-data-warehouse-get-started-load-with-polybase/access-key.png)

### <a name="d-copy-the-sample-file-to-azure-blob-storage"></a>D. Kopieren der Beispieldatei in den Azure-Blobspeicher
So kopieren Sie Ihre Daten in den Azure-Blobspeicher

1. Öffnen Sie eine Eingabeaufforderung, und ändern Sie die Verzeichnisse in das AzCopy-Installationsverzeichnis. Mit diesem Befehl wird das Standardverzeichnis für die Installation auf einem 64-Bit-Windows-Client festgelegt.
   
    ```
    cd /d "%ProgramFiles(x86)%\Microsoft SDKs\Azure\AzCopy"
    ```
2. Führen Sie den folgenden Befehl aus, um die Datei hochzuladen. Geben Sie Ihre Blobdienstendpunkt-URL für <blob service endpoint URL> und Ihren Azure-Speicherkontoschlüssel für <azure_storage_account_key> ein.
   
    ```
    .\AzCopy.exe /Source:C:\Temp\ /Dest:<blob service endpoint URL> /datacontainer/datedimension/ /DestKey:<azure_storage_account_key> /Pattern:DimDate2.txt
    ```

Weitere Informationen finden Sie unter [Übertragen von Daten mit dem Befehlszeilenprogramm AzCopy][Getting Started with the AzCopy Command-Line Utility].

### <a name="e-explore-your-blob-storage-container"></a>E. Untersuchen des Blobspeichercontainers
So zeigen Sie die Datei an, die Sie in den Blobspeicher hochgeladen haben

1. Wechseln Sie zurück zum Blatt für Ihren Blobdienst.
2. Doppelklicken Sie unter „Container“ auf **datacontainer**.
3. Um den Pfad zu Ihren Daten zu untersuchen, klicken Sie auf den Ordner **datedimension**. Die hochgeladene Datei **DimDate2.txt** wird angezeigt.
4. Um Eigenschaften anzuzeigen, klicken Sie auf **DimDate2.txt**.
5. Beachten Sie, dass Sie die Datei auf dem Blatt mit den Blobeigenschaften herunterladen oder löschen können.
   
    ![Azure-Speicherblob anzeigen](./media/sql-data-warehouse-get-started-load-with-polybase/view-blob.png)

## <a name="step-2-create-an-external-table-for-the-sample-data"></a>Schritt 2: Erstellen einer externen Tabelle für die Beispieldaten
In diesem Abschnitt erstellen wir eine externe Tabelle, die die Beispieldaten definiert.

PolyBase verwendet externe Tabellen, um auf Daten in einem Azure-Blobspeicher zuzugreifen. Da die Daten nicht in SQL Data Warehouse gespeichert sind, verarbeitet PolyBase die Authentifizierung für die externen Daten anhand von datenbankbezogenen Anmeldeinformationen.

Für das Beispiel in diesem Schritt werden die folgenden Transact-SQL-Anweisungen verwendet, um eine externe Tabelle zu erstellen.

* [Create Master Key (Transact-SQL):][Create Master Key (Transact-SQL)] Dient zum Verschlüsseln des Geheimnisses Ihrer datenbankbezogenen Anmeldeinformationen.
* [Create Database Scoped Credential (Transact-SQL):][Create Database Scoped Credential (Transact-SQL)] Dient zum Angeben der Authentifizierungsinformationen für Ihr Azure-Speicherkonto.
* [Create External Data Source (Transact-SQL):][Create External Data Source (Transact-SQL)] Dient zum Angeben des Speicherorts Ihres Azure-Blobspeichers.
* [Create External File Format (Transact-SQL):][Create External File Format (Transact-SQL)] Dient zum Angeben des Formats Ihrer Daten.
* [Create External Table (Transact-SQL):][Create External Table (Transact-SQL)] Dient zum Angeben der Tabellendefinition und des Speicherorts der Daten.

Führen Sie diese Abfrage für die SQL Data Warehouse-Datenbank aus. Sie erstellt eine externe Tabelle mit dem Namen „DimDate2External“ im dbo-Schema, die auf die Beispieldaten „DimDate2.txt“ im Azure-Blobspeicher verweist.

```sql
-- A: Create a master key.
-- Only necessary if one does not already exist.
-- Required to encrypt the credential secret in the next step.

CREATE MASTER KEY;


-- B: Create a database scoped credential
-- IDENTITY: Provide any string, it is not used for authentication to Azure storage.
-- SECRET: Provide your Azure storage account key.


CREATE DATABASE SCOPED CREDENTIAL AzureStorageCredential
WITH
    IDENTITY = 'user',
    SECRET = '<azure_storage_account_key>'
;


-- C: Create an external data source
-- TYPE: HADOOP - PolyBase uses Hadoop APIs to access data in Azure blob storage.
-- LOCATION: Provide Azure storage account name and blob container name.
-- CREDENTIAL: Provide the credential created in the previous step.

CREATE EXTERNAL DATA SOURCE AzureStorage
WITH (
    TYPE = HADOOP,
    LOCATION = 'wasbs://<blob_container_name>@<azure_storage_account_name>.blob.core.windows.net',
    CREDENTIAL = AzureStorageCredential
);


-- D: Create an external file format
-- FORMAT_TYPE: Type of file format in Azure storage (supported: DELIMITEDTEXT, RCFILE, ORC, PARQUET).
-- FORMAT_OPTIONS: Specify field terminator, string delimiter, date format etc. for delimited text files.
-- Specify DATA_COMPRESSION method if data is compressed.

CREATE EXTERNAL FILE FORMAT TextFile
WITH (
    FORMAT_TYPE = DelimitedText,
    FORMAT_OPTIONS (FIELD_TERMINATOR = ',')
);


-- E: Create the external table
-- Specify column names and data types. This needs to match the data in the sample file.
-- LOCATION: Specify path to file or directory that contains the data (relative to the blob container).
-- To point to all files under the blob container, use LOCATION='.'

CREATE EXTERNAL TABLE dbo.DimDate2External (
    DateId INT NOT NULL,
    CalendarQuarter TINYINT NOT NULL,
    FiscalQuarter TINYINT NOT NULL
)
WITH (
    LOCATION='/datedimension/',
    DATA_SOURCE=AzureStorage,
    FILE_FORMAT=TextFile
);


-- Run a query on the external table

SELECT count(*) FROM dbo.DimDate2External;

```


Im SQL Server-Objekt-Explorer in Visual Studio sehen Sie das externe Dateiformat, die externe Datenquelle und die Tabelle „DimDate2External“.

![Externe Tabelle anzeigen](./media/sql-data-warehouse-get-started-load-with-polybase/external-table.png)

## <a name="step-3-load-data-into-sql-data-warehouse"></a>Schritt 3: Laden von Daten in SQL Data Warehouse
Sobald die externe Tabelle erstellt wurde, können Sie die Daten in eine neue Tabelle laden oder in eine vorhandene Tabelle einfügen.

* Führen Sie die Anweisung [CREATE TABLE AS SELECT (Transact-SQL)][CREATE TABLE AS SELECT (Transact-SQL)] aus, um die Daten in eine neue Tabelle zu laden. Die neue Tabelle enthält die in der Abfrage benannten Spalten. Die Datentypen der Spalten entsprechen den Datentypen in der Definition der externen Tabelle.
* Verwenden Sie die Anweisung [INSERT...SELECT (Transact-SQL)][INSERT...SELECT (Transact-SQL)], um die Daten in eine vorhandene Tabelle zu laden.

```sql
-- Load the data from Azure blob storage to SQL Data Warehouse

CREATE TABLE dbo.DimDate2
WITH
(   
    CLUSTERED COLUMNSTORE INDEX,
    DISTRIBUTION = ROUND_ROBIN
)
AS
SELECT * FROM [dbo].[DimDate2External];
```

## <a name="step-4-create-statistics-on-your-newly-loaded-data"></a>Schritt 4: Erstellen von Statistiken für die neu geladenen Daten
SQL Data Warehouse ermöglicht keine automatische Erstellung oder automatische Aktualisierung von Statistiken. Um eine hohe Abfrageleistung zu erzielen, ist es daher wichtig, nach dem ersten Laden Statistiken für jede Spalte der einzelnen Tabelle zu erstellen. Es ist auch wichtig, Statistiken nach wesentlichen Änderungen an den Daten zu aktualisieren.

Dieses Beispiel erstellt Statistiken für einzelne Spalten der neuen Tabelle „DimDate2“.

```sql
CREATE STATISTICS [DateId] on [DimDate2] ([DateId]);
CREATE STATISTICS [CalendarQuarter] on [DimDate2] ([CalendarQuarter]);
CREATE STATISTICS [FiscalQuarter] on [DimDate2] ([FiscalQuarter]);
```

Weitere Informationen finden Sie unter [Verwalten von Statistiken für Tabellen in SQL Data Warehouse][Statistics].  

## <a name="next-steps"></a>Nächste Schritte
Im [PolyBase-Handbuch][PolyBase guide] finden Sie weitere Informationen, die Sie beim Entwickeln einer Lösung mit PolyBase kennen sollten.

<!--Image references-->


<!--Article references-->
[PolyBase in SQL Data Warehouse Tutorial]: ./sql-data-warehouse-get-started-load-with-polybase.md
[Load data with bcp]: ./sql-data-warehouse-load-with-bcp.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md
[PolyBase guide]: ./sql-data-warehouse-load-polybase-guide.md
[Getting Started with the AzCopy Command-Line Utility]: ../storage/storage-use-azcopy.md
[latest version of AzCopy]: ../storage/storage-use-azcopy.md

<!--External references-->
[supported source/sink]: https://msdn.microsoft.com/library/dn894007.aspx
[copy activity]: https://msdn.microsoft.com/library/dn835035.aspx
[SQL Server destination adapter]: https://msdn.microsoft.com/library/ms141095.aspx
[SSIS]: https://msdn.microsoft.com/library/ms141026.aspx


[CREATE EXTERNAL DATA SOURCE (Transact-SQL)]:https://msdn.microsoft.com/library/dn935022.aspx
[CREATE EXTERNAL FILE FORMAT (Transact-SQL)]:https://msdn.microsoft.com/library/dn935026.aspx
[CREATE EXTERNAL TABLE (Transact-SQL)]:https://msdn.microsoft.com/library/dn935021.aspx

[DROP EXTERNAL DATA SOURCE (Transact-SQL)]:https://msdn.microsoft.com/library/mt146367.aspx
[DROP EXTERNAL FILE FORMAT (Transact-SQL)]:https://msdn.microsoft.com/library/mt146379.aspx
[DROP EXTERNAL TABLE (Transact-SQL)]:https://msdn.microsoft.com/library/mt130698.aspx

[CREATE TABLE AS SELECT (Transact-SQL)]:https://msdn.microsoft.com/library/mt204041.aspx
[INSERT...SELECT (Transact-SQL)]:https://msdn.microsoft.com/library/ms174335.aspx
[CREATE MASTER KEY (Transact-SQL)]:https://msdn.microsoft.com/library/ms174382.aspx
[CREATE CREDENTIAL (Transact-SQL)]:https://msdn.microsoft.com/library/ms189522.aspx
[CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)]:https://msdn.microsoft.com/library/mt270260.aspx
[DROP CREDENTIAL (Transact-SQL)]:https://msdn.microsoft.com/library/ms189450.aspx

