---
title: "Azure SQL Data Warehouse – Tutorial zu den ersten Schritten | Microsoft-Dokumentation"
description: "In diesem Tutorial erfahren Sie, wie Sie Azure SQL Data Warehouse bereitstellen und Daten laden. Außerdem werden grundlegende Informationen zum Skalieren, Anhalten und Optimieren vermittelt."
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: johnmac
editor: barbkess
ms.assetid: 52DFC191-E094-4B04-893F-B64D5828A900
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: quickstart
ms.date: 01/26/2017
ms.author: elbutter;barbkess
translationtype: Human Translation
ms.sourcegitcommit: 0d8472cb3b0d891d2b184621d62830d1ccd5e2e7
ms.openlocfilehash: 9d3029817cae6570ff8871fbcb068250544595d7
ms.lasthandoff: 03/21/2017


---
# <a name="get-started-with-sql-data-warehouse"></a>Erste Schritte mit SQL Data Warehouse

In diesem Tutorial wird gezeigt, wie Sie Azure SQL Data Warehouse bereitstellen und Daten laden. Außerdem werden grundlegende Informationen zum Skalieren, Anhalten und Optimieren vermittelt. Nach Abschluss dieses Tutorials können Sie Ihr Data Warehouse abfragen und erkunden.

**Voraussichtliche Dauer:** Dieses End-to-End-Tutorial mit Beispielcode dauert ca. 30 Minuten (ohne ggf. erforderliche Vorbereitung). 

## <a name="prerequisites"></a>Voraussetzungen

In diesem Tutorial wird vorausgesetzt, dass Sie mit den grundlegenden Konzepten von SQL Data Warehouse vertraut sind. Eine Einführung finden Sie unter [Was ist SQL Data Warehouse?](sql-data-warehouse-overview-what-is.md). 

### <a name="sign-up-for-microsoft-azure"></a>Für Microsoft Azure registrieren
Falls Sie noch nicht über ein Microsoft Azure-Konto verfügen, müssen Sie sich für ein Konto registrieren, um diesen Dienst nutzen zu können. Sie können diesen Schritt überspringen, wenn Sie schon im Besitz eines Kontos sind. 

1. Navigieren Sie zu den Kontoseiten unter [https://azure.microsoft.com/account/](https://azure.microsoft.com/account/).
2. Erstellen Sie ein kostenloses Azure-Konto, oder erwerben Sie ein kostenpflichtiges Konto.
3. Befolgen Sie die Anweisungen.

### <a name="install-appropriate-sql-client-drivers-and-tools"></a>Installieren der passenden SQL-Clienttreiber und -tools

Von den meisten SQL-Clienttools kann per JDBC, ODBC oder ADO.NET eine Verbindung mit SQL Data Warehouse hergestellt werden. Aufgrund der hohen Anzahl von T-SQL-Features, die von SQL Data Warehouse unterstützt werden, sind einige Clientanwendungen unter Umständen nicht vollständig mit SQL Data Warehouse kompatibel.

Bei Verwendung eines Windows-Betriebssystems empfehlen wir entweder [Visual Studio] oder [SQL Server Management Studio].

[!INCLUDE [Create a new logical server](../../includes/sql-data-warehouse-create-logical-server.md)] 

[!INCLUDE [SQL Database create server](../../includes/sql-database-create-new-server-firewall-portal.md)]

## <a name="create-a-sql-data-warehouse"></a>Erstellen eines SQL Data Warehouse

Eine SQL Data Warehouse-Instanz ist eine besondere Art von Datenbank und für eine intensive parallele Verarbeitung konzipiert. Die Datenbank ist auf mehrere Knoten verteilt und verarbeitet Abfragen parallel. SQL Data Warehouse verfügt über einen Steuerknoten, der die Aktivitäten aller Knoten koordiniert. Die Knoten selbst verwalten Ihre Daten mithilfe von SQL-Datenbank.  

> [!NOTE]
> Wenn Sie eine SQL Data Warehouse-Instanz erstellen, wird dadurch unter Umständen auch ein neuer abrechenbarer Dienst erstellt.  Weitere Informationen finden Sie unter [SQL Data Warehouse – Preise](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).
>

### <a name="create-a-data-warehouse"></a>Erstellen eines Data Warehouse

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Klicken Sie auf **Neu** > **Datenbanken** > **SQL Data Warehouse**.

    ![NewBlade](../../includes/media/sql-data-warehouse-create-dw/blade-click-new.png)
    ![SelectDW](../../includes/media/sql-data-warehouse-create-dw/blade-select-dw.png)

3. Angeben der Bereitstellungsdetails

    **Datenbankname**: Wählen Sie einen beliebigen Namen. Wenn Sie über mehrere Data Warehouses verfügen, sollte der Name Angaben wie Region, Umgebung und Ähnliches enthalten (Beispiel: *mydw-westus-1-test*).

    **Abonnement**: Ihr Azure-Abonnement.

    **Ressourcengruppe**: Erstellen Sie eine Ressourcengruppe, oder verwenden Sie eine vorhandene Ressourcengruppe.
    > [!NOTE]
    > Ressourcengruppen sind für die Ressourcenverwaltung nützlich, z.B. das Einrichten der Zugriffssteuerung und die Bereitstellung über Vorlagen. Weitere Informationen zu Azure-Ressourcengruppen und den bewährten Methoden finden Sie [hier](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups).

    **Quelle**: Leere Datenbank

    **Server**: Wählen Sie den Server, den Sie unter [Voraussetzungen] erstellt haben.

    **Sortierung**: Behalten Sie die Standardsortierung „SQL_Latin1_General_CP1_CI_AS“ bei.

    **Leistung auswählen**: Wir empfehlen, mit der Standardeinstellung von 400 DWUs zu beginnen.

4. Wählen Sie **An Dashboard anheften**
    ![An Dashboard anheften](./media/sql-data-warehouse-get-started-tutorial/pin-to-dashboard.png)

5. Warten Sie, bis die Bereitstellung Ihrer Azure SQL Data Warehouse-Instanz abgeschlossen ist. Dieser Prozess dauert normalerweise mehrere Minuten. Wenn Ihr Data Warehouse verwendungsbereit ist, erhalten Sie im Portal eine entsprechende Benachrichtigung. 

## <a name="connect-to-sql-data-warehouse"></a>Herstellen einer Verbindung mit SQL Data Warehouse

In diesem Tutorial wird SQL Server Management Studio (SSMS) verwendet, um eine Verbindung mit dem Data Warehouse herzustellen. Die Verbindung mit SQL Data Warehouse kann über folgende unterstützte Connectors hergestellt werden: ADO.NET, JDBC, ODBC und PHP. Bei Tools, die nicht von Microsoft unterstützt werden, ist der Funktionsumfang unter Umständen eingeschränkt.


### <a name="get-connection-information"></a>Abrufen von Verbindungsinformationen

Die Verbindung mit Ihrer SQL Data Warehouse-Instanz muss über die logische SQL Server-Instanz hergestellt werden, die Sie unter [Voraussetzungen] erstellt haben.

1. Wählen Sie Ihr Data Warehouse im Dashboard aus, oder durchsuchen Sie Ihre Ressourcen danach.

    ![SQL Data Warehouse-Dashboard](./media/sql-data-warehouse-get-started-tutorial/sql-dw-dashboard.png)

2. Suchen Sie nach dem vollständigen Namen für die logische SQL Server-Instanz.

    ![Auswählen des Servernamens](./media/sql-data-warehouse-get-started-tutorial/select-server.png)

3. Öffnen Sie SSMS, und stellen Sie über den Objekt-Explorer unter Verwendung der Serveradministrator-Anmeldeinformationen, die Sie unter [Voraussetzungen] erstellt haben, eine Verbindung mit diesem Server her.

    ![Verbinden mit SSMS](./media/sql-data-warehouse-get-started-tutorial/ssms-connect.png)

Sofern keine Probleme aufgetreten sind, sollte nun eine Verbindung mit Ihrer logischen SQL Server-Instanz bestehen. Da Sie sich als Serveradministrator angemeldet haben, können Sie eine Verbindung mit einer beliebigen Datenbank herstellen, die von dem Server gehostet wird. Hierzu zählt auch die Masterdatenbank. 

Es gibt nur ein einziges Serveradministratorkonto, und dieses verfügt über mehr Berechtigungen als jeder andere Benutzer. Geben Sie das Administratorkennwort nur an wenige ausgewählte Mitarbeiter in Ihrer Organisation weiter. 

Sie können auch ein Azure Active Directory-Administratorkonto verwenden. Darauf gehen wir hier allerdings nicht näher ein. Weitere Informationen zur Verwendung der Authentifizierung über Azure Active Directory finden Sie unter [Herstellen einer Verbindung mit SQL-Datenbank oder SQL Data Warehouse unter Verwendung der Azure Active Directory-Authentifizierung](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication).

Als Nächstes beschäftigen wir uns mit der Erstellung zusätzlicher Anmeldungen und Benutzer.


## <a name="create-a-database-user"></a>Erstellen eines Datenbankbenutzers

In diesem Schritt erstellen Sie ein Benutzerkonto für den Zugriff auf Ihr Data Warehouse. Außerdem erfahren Sie, wie Sie dem Benutzer das Ausführen von Abfragen mit hohem Arbeitsspeicher- und CPU-Ressourcenbedarf ermöglichen.

### <a name="notes-about-resource-classes-for-allocating-resources-to-queries"></a>Hinweise zu Ressourcenklassen für die Zuordnung von Ressourcen zu Abfragen

- Zum Schutz Ihrer Daten sollten Sie Abfragen für Ihre Produktionsdatenbanken nicht unter Verwendung des Serveradministrators ausführen. Er verfügt über mehr Berechtigungen als jeder andere Benutzer, und seine Verwendung bei Vorgängen für Benutzerdaten stellt eine Gefahr für Ihre Daten dar. Da der Serveradministrator außerdem für die Ausführung von Verwaltungsvorgängen vorgesehen ist, werden für die Vorgänge nur wenig Arbeitsspeicher und CPU-Ressourcen zugewiesen. 

- SQL Data Warehouse verwendet vordefinierte Datenbankrollen (so genannte Ressourcenklassen), um Benutzern unterschiedliche Mengen an Arbeitsspeicher, CPU-Ressourcen und Parallelitätsslots zuzuordnen. Jeder Benutzer kann einer kleinen, mittleren, großen oder sehr großen Ressourcenklasse angehören. Die Ressourcenklasse des Benutzers bestimmt die Ressourcen, die dem Benutzer für Abfragen und Ladevorgänge zur Verfügung stehen.

- Für eine optimale Datenkomprimierung muss der Benutzer beim Laden unter Umständen eine große oder sehr große Ressourcenzuordnung verwenden. Weitere Informationen zu Ressourcenklassen finden Sie [hier](./sql-data-warehouse-develop-concurrency.md#resource-classes):

### <a name="create-an-account-that-can-control-a-database"></a>Erstellen eines Kontos zum Steuern einer Datenbank

Da Sie momentan als Serveradministrator angemeldet sind, sind Sie zum Erstellen von Anmeldungen und Benutzern berechtigt.

1. Öffnen Sie mit SSMS oder einem anderen Abfrageclient eine neue Abfrage für **master**.

    ![Neue Abfrage für „Master“](./media/sql-data-warehouse-get-started-tutorial/query-on-server.png)

    ![Neue Abfrage für „Master1“](./media/sql-data-warehouse-get-started-tutorial/query-on-master.png)

2. Führen Sie im Abfragefenster den folgenden T-SQL-Befehl aus, um eine Anmeldung namens „MedRCLogin“ und einen Benutzer namens „LoadingUser“ zu erstellen. Diese Anmeldung kann eine Verbindung mit der logischen SQL Server-Instanz herstellen.

    ```sql
    CREATE LOGIN MedRCLogin WITH PASSWORD = 'a123reallySTRONGpassword!';
    CREATE USER LoadingUser FOR LOGIN MedRCLogin;
    ```

3. Fragen Sie nun die *SQL Data Warehouse-Datenbank* ab, und erstellen Sie auf der Grundlage der Anmeldung, die Sie erstellt haben, einen Datenbankbenutzer für den Datenbankzugriff und die Ausführung von Datenbankvorgängen.

    ```sql
    CREATE USER LoadingUser FOR LOGIN MedRCLogin;
    ```

4. Erteilen Sie dem Datenbankbenutzer Zugriffsberechtigungen für die Datenbank namens „NYT“. 

    ```sql
    GRANT CONTROL ON DATABASE::[NYT] to LoadingUser;
    ```
    > [!NOTE]
    > Falls der Datenbankname Bindestriche enthält, müssen Sie ihn in Klammern setzen. 
    >

### <a name="give-the-user-medium-resource-allocations"></a>Zuweisen mittelgroßer Ressourcenzuweisungen für den Benutzer

1. Führen Sie den folgenden T-SQL-Befehl aus, um eine Zuweisung zur mittelgroßen Ressourcenklasse (mediumrc) durchzuführen. 

    ```sql
    EXEC sp_addrolemember 'mediumrc', 'LoadingUser';
    ```
    > [!NOTE]
    > Weitere Informationen zu Parallelität und Ressourcenklassen finden Sie [hier](sql-data-warehouse-develop-concurrency.md#resource-classes). 
    >

2. Stellen Sie unter Verwendung der neuen Anmeldeinformationen eine Verbindung mit dem logischen Server her.

    ![Anmelden mit neuer Anmeldung](./media/sql-data-warehouse-get-started-tutorial/new-login.png)


## <a name="load-data-from-azure-blob-storage"></a>Laden von Daten aus dem Azure-Blob-Speicher

Nun können Sie Daten in Ihr Data Warehouse laden. In diesem Schritt wird gezeigt, wie Sie Daten zu New Yorker Taxis aus einem öffentlichen Azure-Speicherblob laden. 

- Eine gängige Methode zum Laden von Daten in SQL Data Warehouse besteht darin, die Daten zunächst in Azure Blob Storage zu verschieben und sie anschließend in das Data Warehouse zu laden. Zum einfacheren Verständnis des Ladevorgangs haben wir die Taxidaten für New York bereits in einem öffentlichen Azure-Speicherblob gehostet. 

- Informationen zum Übertragen Ihrer Daten in Azure Blob Storage oder zum direkten Laden der Daten aus Ihrer Quelle in SQL Data Warehouse finden Sie in der [Ladeübersicht](sql-data-warehouse-overview-load.md).


### <a name="define-external-data"></a>Definieren externer Daten

1. Erstellen Sie einen Hautschlüssel. Der Hauptschlüssel muss nur jeweils einmal pro Datenbank erstellt werden. 

    ```sql
    CREATE MASTER KEY;
    ```

2. Definieren Sie den Speicherort des Azure-Blobs mit den Taxidaten.  

    ```sql
    CREATE EXTERNAL DATA SOURCE NYTPublic
    WITH
    (
        TYPE = Hadoop,
        LOCATION = 'wasbs://2013@nytpublic.blob.core.windows.net/'
    );
    ```

3. Definieren Sie die externen Dateiformate.

    Der Befehl ```CREATE EXTERNAL FILE FORMAT``` dient zum Angeben des Formats von Dateien mit den externen Daten. Sie enthalten durch Trennzeichen getrennten Text. Zur Veranschaulichung sind die Taxidaten sowohl als unkomprimierte Daten als auch als komprimierte GZip-Daten gespeichert.

    Führen Sie die folgenden T-SQL-Befehle aus, um zwei verschiedene Formate zu definieren: unkomprimiert und komprimiert.

    ```sql
    CREATE EXTERNAL FILE FORMAT uncompressedcsv
    WITH (
        FORMAT_TYPE = DELIMITEDTEXT,
        FORMAT_OPTIONS ( 
            FIELD_TERMINATOR = ',',
            STRING_DELIMITER = '',
            DATE_FORMAT = '',
            USE_TYPE_DEFAULT = False
        )
    );

    CREATE EXTERNAL FILE FORMAT compressedcsv
    WITH ( 
        FORMAT_TYPE = DELIMITEDTEXT,
        FORMAT_OPTIONS ( FIELD_TERMINATOR = '|',
            STRING_DELIMITER = '',
        DATE_FORMAT = '',
            USE_TYPE_DEFAULT = False
        ),
        DATA_COMPRESSION = 'org.apache.hadoop.io.compress.GzipCodec'
    );
    ```

4.  Erstellen Sie ein Schema für das Format Ihrer externen Dateien. 

    ```sql
    CREATE SCHEMA ext;
    ```
5. Erstellen Sie die externen Tabellen. In diesen Tabellen wird auf Daten verwiesen, die in Azure Blob Storage gespeichert sind. Führen Sie die folgenden T-SQL-Befehle aus, um mehrere externe Tabellen zu erstellen, die alle auf das Azure-Blob verweisen, das wir zuvor in der externen Datenquelle definiert haben.

```sql
    CREATE EXTERNAL TABLE [ext].[Date] 
    (
        [DateID] int NOT NULL,
        [Date] datetime NULL,
        [DateBKey] char(10) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DayOfMonth] varchar(2) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DaySuffix] varchar(4) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DayName] varchar(9) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DayOfWeek] char(1) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DayOfWeekInMonth] varchar(2) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DayOfWeekInYear] varchar(2) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DayOfQuarter] varchar(3) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DayOfYear] varchar(3) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [WeekOfMonth] varchar(1) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [WeekOfQuarter] varchar(2) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [WeekOfYear] varchar(2) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [Month] varchar(2) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [MonthName] varchar(9) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [MonthOfQuarter] varchar(2) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [Quarter] char(1) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [QuarterName] varchar(9) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [Year] char(4) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [YearName] char(7) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [MonthYear] char(10) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [MMYYYY] char(6) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [FirstDayOfMonth] date NULL,
        [LastDayOfMonth] date NULL,
        [FirstDayOfQuarter] date NULL,
        [LastDayOfQuarter] date NULL,
        [FirstDayOfYear] date NULL,
        [LastDayOfYear] date NULL,
        [IsHolidayUSA] bit NULL,
        [IsWeekday] bit NULL,
        [HolidayUSA] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL
    )
    WITH
    (
        LOCATION = 'Date',
        DATA_SOURCE = NYTPublic,
        FILE_FORMAT = uncompressedcsv,
        REJECT_TYPE = value,
        REJECT_VALUE = 0
    );
    
    CREATE EXTERNAL TABLE [ext].[Geography]
    (
        [GeographyID] int NOT NULL,
        [ZipCodeBKey] varchar(10) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL,
        [County] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [City] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [State] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [Country] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [ZipCode] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL
    )
    WITH
    (
        LOCATION = 'Geography',
        DATA_SOURCE = NYTPublic,
        FILE_FORMAT = uncompressedcsv,
        REJECT_TYPE = value,
        REJECT_VALUE = 0 
    );
        
    
    CREATE EXTERNAL TABLE [ext].[HackneyLicense]
    (
        [HackneyLicenseID] int NOT NULL,
        [HackneyLicenseBKey] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL,
        [HackneyLicenseCode] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL
    )
    WITH
    (
        LOCATION = 'HackneyLicense',
        DATA_SOURCE = NYTPublic,
        FILE_FORMAT = uncompressedcsv,
        REJECT_TYPE = value,
        REJECT_VALUE = 0
    )
    ;
        
    
    CREATE EXTERNAL TABLE [ext].[Medallion]
    (
        [MedallionID] int NOT NULL,
        [MedallionBKey] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL,
        [MedallionCode] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL
    )
    WITH
    (
        LOCATION = 'Medallion',
        DATA_SOURCE = NYTPublic,
        FILE_FORMAT = uncompressedcsv,
        REJECT_TYPE = value,
        REJECT_VALUE = 0
    )
    ;
        
    CREATE EXTERNAL TABLE [ext].[Time]
    (
        [TimeID] int NOT NULL,
        [TimeBKey] varchar(8) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL,
        [HourNumber] tinyint NOT NULL,
        [MinuteNumber] tinyint NOT NULL,
        [SecondNumber] tinyint NOT NULL,
        [TimeInSecond] int NOT NULL,
        [HourlyBucket] varchar(15) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL,
        [DayTimeBucketGroupKey] int NOT NULL,
        [DayTimeBucket] varchar(100) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL
    )
    WITH
    (
        LOCATION = 'Time',
        DATA_SOURCE = NYTPublic,
        FILE_FORMAT = uncompressedcsv,
        REJECT_TYPE = value,
        REJECT_VALUE = 0
    )
    ;
    
    
    CREATE EXTERNAL TABLE [ext].[Trip]
    (
        [DateID] int NOT NULL,
        [MedallionID] int NOT NULL,
        [HackneyLicenseID] int NOT NULL,
        [PickupTimeID] int NOT NULL,
        [DropoffTimeID] int NOT NULL,
        [PickupGeographyID] int NULL,
        [DropoffGeographyID] int NULL,
        [PickupLatitude] float NULL,
        [PickupLongitude] float NULL,
        [PickupLatLong] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DropoffLatitude] float NULL,
        [DropoffLongitude] float NULL,
        [DropoffLatLong] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [PassengerCount] int NULL,
        [TripDurationSeconds] int NULL,
        [TripDistanceMiles] float NULL,
        [PaymentType] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [FareAmount] money NULL,
        [SurchargeAmount] money NULL,
        [TaxAmount] money NULL,
        [TipAmount] money NULL,
        [TollsAmount] money NULL,
        [TotalAmount] money NULL
    )
    WITH
    (
        LOCATION = 'Trip2013',
        DATA_SOURCE = NYTPublic,
        FILE_FORMAT = compressedcsv,
        REJECT_TYPE = value,
        REJECT_VALUE = 0
    )
    ;
    
    CREATE EXTERNAL TABLE [ext].[Weather]
    (
        [DateID] int NOT NULL,
        [GeographyID] int NOT NULL,
        [PrecipitationInches] float NOT NULL,
        [AvgTemperatureFahrenheit] float NOT NULL
    )
    WITH
    (
        LOCATION = 'Weather2013',
        DATA_SOURCE = NYTPublic,
        FILE_FORMAT = uncompressedcsv,
        REJECT_TYPE = value,
        REJECT_VALUE = 0
    )
    ;
```

### <a name="import-the-data-from-azure-blob-storage"></a>Importieren Sie die Daten aus Azure Blob Storage.

SQL Data Warehouse unterstützt eine Schlüsselanweisung mit dem Namen „CREATE TABLE AS SELECT“ (CTAS). Diese Anweisung erstellt eine neue Tabelle basierend auf den Ergebnissen einer SELECT-Anweisung. Die neue Tabelle weist die gleichen Spalten und Datentypen wie die Ergebnisse der SELECT-Anweisung auf.  Dies ist eine elegante Methode zum Importieren von Daten aus Azure Blob Storage in SQL Data Warehouse.

1. Führen Sie dieses Skript aus, um Ihre Daten zu importieren.

    ```sql
    CREATE TABLE [dbo].[Date]
    WITH
    ( 
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    )
    AS SELECT * FROM [ext].[Date]
    OPTION (LABEL = 'CTAS : Load [dbo].[Date]')
    ;
    
    CREATE TABLE [dbo].[Geography]
    WITH
    ( 
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    )
    AS
    SELECT * FROM [ext].[Geography]
    OPTION (LABEL = 'CTAS : Load [dbo].[Geography]')
    ;
    
    CREATE TABLE [dbo].[HackneyLicense]
    WITH
    ( 
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    )
    AS SELECT * FROM [ext].[HackneyLicense]
    OPTION (LABEL = 'CTAS : Load [dbo].[HackneyLicense]')
    ;
    
    CREATE TABLE [dbo].[Medallion]
    WITH
    (
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    )
    AS SELECT * FROM [ext].[Medallion]
    OPTION (LABEL = 'CTAS : Load [dbo].[Medallion]')
    ;
    
    CREATE TABLE [dbo].[Time]
    WITH
    (
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    )
    AS SELECT * FROM [ext].[Time]
    OPTION (LABEL = 'CTAS : Load [dbo].[Time]')
    ;
    
    CREATE TABLE [dbo].[Weather]
    WITH
    ( 
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    )
    AS SELECT * FROM [ext].[Weather]
    OPTION (LABEL = 'CTAS : Load [dbo].[Weather]')
    ;
    
    CREATE TABLE [dbo].[Trip]
    WITH
    (
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    )
    AS SELECT * FROM [ext].[Trip]
    OPTION (LABEL = 'CTAS : Load [dbo].[Trip]')
    ;
    ```

2. Sehen Sie Ihren Daten beim Laden zu.

   Sie laden mehrere GB an Daten und komprimieren diese in hoch performante gruppierte Columnstore-Indizes. Führen Sie die folgende Abfrage mit dynamischen Verwaltungssichten (DMVs) aus, um den Status des Ladevorgangs anzuzeigen. Nach dem Starten der Abfrage können Sie sich einen Kaffee und einen Imbiss holen, während SQL Data Warehouse einige anstrengende Arbeit erledigt.
    
    ```sql
    SELECT
        r.command,
        s.request_id,
        r.status,
        count(distinct input_name) as nbr_files,
        sum(s.bytes_processed)/1024/1024 as gb_processed
    FROM 
        sys.dm_pdw_exec_requests r
        INNER JOIN sys.dm_pdw_dms_external_work s
        ON r.request_id = s.request_id
    WHERE
        r.[label] = 'CTAS : Load [dbo].[Date]' OR
        r.[label] = 'CTAS : Load [dbo].[Geography]' OR
        r.[label] = 'CTAS : Load [dbo].[HackneyLicense]' OR
        r.[label] = 'CTAS : Load [dbo].[Medallion]' OR
        r.[label] = 'CTAS : Load [dbo].[Time]' OR
        r.[label] = 'CTAS : Load [dbo].[Weather]' OR
        r.[label] = 'CTAS : Load [dbo].[Trip]'
    GROUP BY
        r.command,
        s.request_id,
        r.status
    ORDER BY
        nbr_files desc, 
        gb_processed desc;
    ```

3. Zeigen Sie alle Systemabfragen an.

    ```sql
    SELECT * FROM sys.dm_pdw_exec_requests;
    ```

4. Freuen Sie sich darüber, dass Ihre Daten sauber in die Azure SQL Data Warehouse-Instanz geladen werden.

    ![Laden der Daten](./media/sql-data-warehouse-get-started-tutorial/see-data-loaded.png)


## <a name="improve-query-performance"></a>Verbessern der Abfrageleistung

Es gibt mehrere Möglichkeiten, um die Abfrageleistung zu verbessern und die Geschwindigkeit zu erzielen, für die SQL Data Warehouse konzipiert ist.  

### <a name="see-the-effect-of-scaling-on-query-performance"></a>Anzeigen der Auswirkung der Skalierung auf die Abfrageleistung 

Eine Möglichkeit zum Verbessern der Abfrageleistung ist das Skalieren von Ressourcen durch Ändern des DWU-Servicelevels für Ihr Data Warehouse. Jedes Servicelevel ist teurer, aber Sie können Ressourcen jederzeit zurückskalieren oder anhalten. 

In diesem Schritt vergleichen Sie die Leistung bei zwei unterschiedlichen DWU-Einstellungen.

Zunächst skalieren wir den Vorgang zentral auf 100 DWUs herunter, damit wir einschätzen können, über welche Leistung ein Computeknoten allein verfügt.

1. Navigieren Sie zum Portal, und wählen Sie Ihre SQL Data Warehouse-Instanz aus.

2. Wählen Sie auf dem Blatt „SQL Data Warehouse“ die Option „Skalieren“. 

    ![DW-Skalierung im Portal](./media/sql-data-warehouse-get-started-tutorial/scale-dw.png)

3. Skalieren Sie die Leistung zentral auf 100 DWUs herunter, und klicken Sie auf „Speichern“.

    ![Skalieren und Speichern](./media/sql-data-warehouse-get-started-tutorial/scale-and-save.png)

4. Warten Sie, bis der Skalierungsvorgang abgeschlossen ist.

    > [!NOTE]
    > Abfragen können nicht ausgeführt werden, während die Skalierung geändert wird. Eine Skalierung **beendet** aktuell ausgeführte Abfragen. Sie können diese neu starten, wenn der Vorgang abgeschlossen ist.
    >
    
5. Führen Sie einen Scanvorgang für die Taxi-Verkehrsdaten durch, indem Sie die erste Million der Einträge für alle Spalten auswählen. Falls Sie sich nicht so lange aufhalten möchten, können Sie auch weniger Zeilen wählen. Notieren Sie sich, wie lange dieser Vorgang dauert.

    ```sql
    SELECT TOP(1000000) * FROM dbo.[Trip]
    ```
6. Skalieren Sie Ihr Data Warehouse zurück auf 400 DWUs. Beachten Sie, dass für 100 DWUs Ihrer Azure SQL Data Warehouse-Instanz jeweils ein weiterer Computeknoten hinzugefügt wird.

7. Führen Sie die Abfrage erneut aus! Es sollte ein deutlicher Unterschied zu sehen sein. 

> [!NOTE]
> SQL Data Warehouse verwendet einen MPP-Ansatz (Massively Parallel Processing). Bei Abfragen, die Millionen von Zeilen durchsuchen oder Analysefunktionen dafür ausführen, kommt die Leistungsstärke von Azure SQL Data Warehouse voll zur Geltung.
>

### <a name="see-the-effect-of-statistics-on-query-performance"></a>Anzeigen der Auswirkung von Statistiken auf die Abfrageleistung

1. Führen Sie eine Abfrage aus, bei der die Datumstabelle mit der Fahrttabelle verknüpft wird.

    ```sql
    SELECT TOP (1000000) 
        dt.[DayOfWeek],
        tr.[MedallionID],
        tr.[HackneyLicenseID],
        tr.[PickupTimeID],
        tr.[DropoffTimeID],
        tr.[PickupGeographyID],
        tr.[DropoffGeographyID],
        tr.[PickupLatitude],
        tr.[PickupLongitude],
        tr.[PickupLatLong],
        tr.[DropoffLatitude],
        tr.[DropoffLongitude],
        tr.[DropoffLatLong],
        tr.[PassengerCount],
        tr.[TripDurationSeconds],
        tr.[TripDistanceMiles],
        tr.[PaymentType],
        tr.[FareAmount],
        tr.[SurchargeAmount],
        tr.[TaxAmount],
        tr.[TipAmount],
        tr.[TollsAmount],
        tr.[TotalAmount]
    FROM [dbo].[Trip] as tr
        JOIN dbo.[Date] as dt
        ON  tr.DateID = dt.DateID
    ```

    Diese Abfrage dauert eine Weile, da SQL Data Warehouse die Daten zu mischen muss, bevor der Join ausgeführt werden. Bei Joins müssen keine Daten gemischt werden, wenn Daten auf die gleiche Weise verknüpft werden sollen, wie sie verteilt werden. Dies ist ein ausführlicheres Thema. 

2. Statistiken machen einen Unterschied. 
3. Führen Sie diese Anweisung aus, um Statistiken für die verknüpften Spalten zu erstellen.

    ```sql
    CREATE STATISTICS [dbo.Date DateID stats] ON dbo.Date (DateID);
    CREATE STATISTICS [dbo.Trip DateID stats] ON dbo.Trip (DateID);
    ```

    > [!NOTE]
    > In SQL DW wird normalerweise keine automatische Verwaltung von Statistiken für Sie durchgeführt. Statistiken sind wichtig für die Abfrageleistung, und es wird dringend empfohlen, Statistiken zu erstellen und zu aktualisieren.
    > 
    > **Die größten Vorteile ergeben sich, wenn Sie Statistiken für Spalten in Verknüpfungen, in der WHERE-Klausel verwendete Spalten und Spalten in GROUP BY nutzen.**
    >

3. Führen Sie die Abfrage aus „Voraussetzungen“ erneut aus, und achten Sie auf Leistungsunterschiede. Die Unterschiede der Abfrageleistung sind nicht so drastisch wie beim zentralen Hochskalieren, aber es sollte eine Beschleunigung zu beobachten sein. 

## <a name="next-steps"></a>Nächste Schritte

Sie haben jetzt alles vorbereitet, um Abfragen und Untersuchungen durchzuführen. Sehen Sie sich unsere bewährten Methoden und Tipps an.

Denken Sie daran, Ihre Instanz zu pausieren, wenn Sie mit der Arbeit eines Tages fertig sind! In der Produktion können Sie enorme Einsparungen erzielen, wenn Sie pausieren und die Skalierung jeweils an Ihre Geschäftsanforderungen anpassen.

![Pause](./media/sql-data-warehouse-get-started-tutorial/pause.png)

## <a name="useful-readings"></a>Nützliche Messdaten

[Parallelitäts- und Workloadverwaltung][]

[Bewährte Methoden für Azure SQL Data Warehouse][]

[Abfrageüberwachung][]

[Die besten zehn bewährten Methoden zum Erstellen eines großen relationalen Data Warehouse][]

[Migrieren von Daten zu Azure SQL Data Warehouse][]

[Parallelitäts- und Workloadverwaltung]: sql-data-warehouse-develop-concurrency.md#change-a-user-resource-class-example
[Bewährte Methoden für Azure SQL Data Warehouse]: sql-data-warehouse-best-practices.md#hash-distribute-large-tables
[Abfrageüberwachung]: sql-data-warehouse-manage-monitor.md
[Die besten zehn bewährten Methoden zum Erstellen eines großen relationalen Data Warehouse]: https://blogs.msdn.microsoft.com/sqlcat/2013/09/16/top-10-best-practices-for-building-a-large-scale-relational-data-warehouse/
[Migrieren von Daten zu Azure SQL Data Warehouse]: https://blogs.msdn.microsoft.com/sqlcat/2016/08/18/migrating-data-to-azure-sql-data-warehouse-in-practice/



[!INCLUDE [Additional Resources](../../includes/sql-data-warehouse-article-footer.md)]

<!-- Internal Links -->
[Voraussetzungen]: sql-data-warehouse-get-started-tutorial.md#prerequisites

<!--Other Web references-->
[Visual Studio]: https://www.visualstudio.com/
[SQL Server Management Studio]: https://msdn.microsoft.com/en-us/library/mt238290.aspx

