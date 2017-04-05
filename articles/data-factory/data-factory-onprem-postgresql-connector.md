---
title: Verschieben von Daten aus PostgreSQL mithilfe von Azure Data Factory | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Daten aus der PostgreSQL-Datenbank mithilfe von Azure Data Factory verschieben.
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 888d9ebc-2500-4071-b6d1-0f6bd1b5997c
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: jingwang
translationtype: Human Translation
ms.sourcegitcommit: b4802009a8512cb4dcb49602545c7a31969e0a25
ms.openlocfilehash: 46cee1099e487799ad6cf657253cc7f3c9b194e2
ms.lasthandoff: 03/29/2017


---
# <a name="move-data-from-postgresql-using-azure-data-factory"></a>Verschieben von Daten aus PostgreSQL mithilfe von Azure Data Factory 
Dieser Artikel beschreibt, wie Sie die Kopieraktivität in Azure Data Factory verwenden, um Daten aus einer lokalen PostgreSQL-Datenbank zu verschieben. Dieser Artikel baut auf dem Artikel zu [Datenverschiebungsaktivitäten](data-factory-data-movement-activities.md) auf, der eine allgemeine Übersicht zur Datenverschiebung mit der Kopieraktivität bietet.

Sie können Daten aus einer lokalen PostgreSQL-Datenbank in beliebige unterstützte Senkendatenspeicher kopieren. Eine Liste der Datenspeicher, die als Senken für die Kopieraktivität unterstützt werden, finden Sie in der Tabelle [Unterstützte Datenspeicher](data-factory-data-movement-activities.md#supported-data-stores-and-formats). Data Factory unterstützt derzeit nur das Verschieben von Daten aus einem PostgreSQL-Datenspeicher in andere Datenspeicher, aber nicht das Verschieben von Daten aus anderen Datenspeichern in einen PostgreSQL-Datenspeicher. 

## <a name="prerequisites"></a>Voraussetzungen

Der Data Factory-Dienst unterstützt das Herstellen einer Verbindung mit lokalen PostgreSQL-Datenquellen mithilfe des Datenverwaltungsgateways. Im Artikel [Verschieben von Daten zwischen lokalen Standorten und Cloud](data-factory-move-data-between-onprem-and-cloud.md) erfahren mehr zum Datenverwaltungsgateway und erhalten eine schrittweise Anleitung zum Einrichten des Gateways.

Das Gateway ist auch erforderlich, wenn die PostgreSQL-Datenbank auf einem virtuellen Azure IaaS-Computer gehostet wird. Sie können das Gateway auf dem gleichen virtuellen IaaS-Computer installieren wie den Datenspeicher oder auch auf einem anderen virtuellen Computer, solange das Gateway eine Verbindung mit der Datenbank herstellen kann.

> [!NOTE]
> Unter [Problembehandlung bei Gateways](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) finden Sie Tipps zur Behandlung von Verbindungs- bzw. Gatewayproblemen.

## <a name="supported-versions-and-installation"></a>Unterstützte Versionen und Installation
Damit sich das Datenverwaltungsgateway mit der PostgreSQL-Datenbank verbindet, müssen Sie den [Ngpsql-Datenanbieter für PostgreSQL](http://go.microsoft.com/fwlink/?linkid=282716) 2.0.12 oder höher auf dem System mit dem Datenverwaltungsgateway installieren. PostgreSQL-Version 7.4 oder höher wird unterstützt.

## <a name="getting-started"></a>Erste Schritte
Sie können eine Pipeline mit einer Kopieraktivität erstellen, die Daten mithilfe verschiedener Tools/APIs aus einem lokalen PostgreSQL-Datenspeicher verschiebt. 

- Am einfachsten erstellen Sie eine Pipeline mit dem **Kopier-Assistenten**. Unter [Tutorial: Erstellen einer Pipeline mit dem Assistenten zum Kopieren](data-factory-copy-data-wizard-tutorial.md) finden Sie eine kurze exemplarische Vorgehensweise zum Erstellen einer Pipeline mithilfe des Assistenten zum Kopieren von Daten. 
- Sie können auch die folgenden Tools für das Erstellen einer Pipeline verwenden: **Azure-Portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-Vorlagen**, **.NET-API** und **REST-API**. Im [Tutorial zur Kopieraktivität](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) finden Sie detaillierte Anweisungen, wie Sie eine Pipeline mit einer Kopieraktivität erstellen können. 

Unabhängig davon, ob Sie Tools oder APIs verwenden, führen Sie die folgenden Schritte aus, um eine Pipeline zu erstellen, die Daten aus einem Quelldatenspeicher in einen Senkendatenspeicher verschiebt:

1. Erstellen **verknüpfter Dienste** zum Verknüpfen von Eingabe- und Ausgabedatenspeichern mit Ihrer Data Factory
2. Erstellen von **Datasets** zur Darstellung von Eingabe- und Ausgabedaten für den Kopiervorgang 
3. Erstellen einer **Pipeline** mit einer Kopieraktivität, die ein Dataset als Eingabe und ein Dataset als Ausgabe akzeptiert 

Wenn Sie den Assistenten verwenden, werden automatisch JSON-Definitionen für diese Data Factory-Entitäten (verknüpfte Diensten, Datasets und die Pipeline) erstellt. Bei Verwendung von Tools und APIs (mit Ausnahme der .NET-API) definieren Sie diese Data Factory-Entitäten im JSON-Format.  Ein Beispiel mit JSON-Definitionen für Data Factory-Entitäten, die zum Kopieren von Daten aus einem lokalen PostgreSQL-Datenspeicher verwendet werden, finden Sie in diesem Artikel im Abschnitt [JSON-Beispiel: Kopieren von Daten aus PostgreSQL in ein Azure-Blob](#json-example-copy-data-from-postgresql-to-azure-blob). 

Die folgenden Abschnitte enthalten Details zu JSON-Eigenschaften, die zum Definieren von Data Factory-Entitäten speziell für PostgreSQL-Datenspeicher verwendet werden:

## <a name="linked-service-properties"></a>Eigenschaften des verknüpften Diensts
Die folgende Tabelle enthält eine Beschreibung der JSON-Elemente, die für den mit PostgreSQL verknüpften Dienst spezifisch sind.

| Eigenschaft | Beschreibung | Erforderlich |
| --- | --- | --- |
| Typ |Die "type"-Eigenschaft muss auf **OnPremisesPostgreSql** |Ja |
| server |Name des PostgreSQL-Servers. |Ja |
| database |Name der PostgreSQL-Datenbank. |Ja |
| schema |Name des Schemas in der Datenbank. Beim Schemanamen wird die Groß- und Kleinschreibung beachtet. |Nein |
| authenticationType |Typ der Authentifizierung für die Verbindung mit der PostgreSQL-Datenbank. Mögliche Werte: Anonymous, Basic und Windows. |Ja |
| username |Geben Sie den Benutzernamen an, wenn Sie die Standard- oder Windows-Authentifizierung verwenden. |Nein |
| password |Geben Sie das Kennwort für das Benutzerkonto an, das Sie für den Benutzernamen angegeben haben. |Nein |
| gatewayName |Name des Gateways, das der Data Factory-Dienst zum Herstellen einer Verbindung mit der lokalen PostgreSQL-Datenbank verwenden soll. |Ja |

## <a name="dataset-properties"></a>Dataset-Eigenschaften
Eine vollständige Liste der Abschnitte und Eigenschaften, die zum Definieren von Datasets zur Verfügung stehen, finden Sie im Artikel [Erstellen von Datasets](data-factory-create-datasets.md). Abschnitte wie „structure“, „availability“ und „policy“ des JSON-Codes eines Datasets sind bei allen Dataset-Typen (Azure SQL, Azure-Blob, Azure-Tabelle usw.) ähnlich.

Der Abschnitt "typeProperties" unterscheidet sich bei jedem Typ von Dataset und bietet Informationen zum Speicherort der Daten im Datenspeicher. Der Abschnitt „typeProperties“ für ein Dataset vom Typ **RelationalTable** (wozu das PostgreSQL-Dataset gehört) hat folgende Eigenschaften:

| Eigenschaft | Beschreibung | Erforderlich |
| --- | --- | --- |
| tableName |Name der Tabelle in der PostgreSQL-Datenbankinstanz, auf die der verknüpfte Dienst verweist. Beim Tabellennamen wird die Groß- und Kleinschreibung beachtet. |Nein (wenn **query** von **RelationalSource** angegeben ist) |

## <a name="copy-activity-properties"></a>Eigenschaften der Kopieraktivität
Eine vollständige Liste der Abschnitte und Eigenschaften zum Definieren von Aktivitäten finden Sie im Artikel [Erstellen von Pipelines](data-factory-create-pipelines.md). Eigenschaften wie Name, Beschreibung, Eingabe- und Ausgabetabellen und Richtlinie sind für alle Arten von Aktivitäten verfügbar.

Eigenschaften im Abschnitt typeProperties der Aktivität können dagegen je nach Aktivitätstyp variieren. Für die Kopieraktivität variieren die Eigenschaften je nach Art der Quellen und Senken.

Wenn eine Quelle vom Typ **RelationalSource** (wozu PostgreSQL gehört) verwendet wird, sind im Abschnitt typeProperties folgende Eigenschaften verfügbar:

| Eigenschaft | Beschreibung | Zulässige Werte | Erforderlich |
| --- | --- | --- | --- |
| query |Verwendet die benutzerdefinierte Abfrage zum Lesen von Daten. |SQL-Abfragezeichenfolge. Beispiel: "query": "select * from \"MySchema\".\"MyTable\"". |Nein (wenn **tableName** von **Dataset** angegeben ist) |

> [!NOTE]
> Bei Schema- und Tabellennamen muss die Groß- und Kleinschreibung beachtet werden, und sie müssen in der Abfrage in doppelte Anführungszeichen (`""`) eingeschlossen werden.  

**Beispiel:**

 "query": "select * from \"MySchema\".\"MyTable\""

## <a name="json-example-copy-data-from-postgresql-to-azure-blob"></a>JSON-Beispiel: Kopieren von Daten aus PostgreSQL in ein Azure-Blob
Dieses Beispiel stellt JSON-Beispieldefinitionen bereit, die Sie zum Erstellen einer Pipeline mit dem [Azure-Portal](data-factory-copy-activity-tutorial-using-azure-portal.md), mit [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) oder mit [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md) verwenden können. Darin wird veranschaulicht, wie Sie Daten aus einer PostgreSQL-Datenbank in Azure Blob Storage kopieren. Daten können jedoch auch mithilfe der Kopieraktivität in Azure Data Factory in eine beliebige der [hier](data-factory-data-movement-activities.md#supported-data-stores-and-formats) aufgeführten Senken kopiert werden.   

> [!IMPORTANT]
> Dieses Beispiel enthält JSON-Codeausschnitte. Eine schrittweise Anleitung zum Erstellen der Data Factory ist nicht enthalten. Einen Artikel mit schrittweisen Anleitungen finden Sie unter [Verschieben von Daten zwischen lokalen Standorten und Cloud](data-factory-move-data-between-onprem-and-cloud.md) .

Das Beispiel enthält die folgenden Data Factory-Entitäten:

1. Einen verknüpften Dienst des Typs [OnPremisesPostgreSql](data-factory-onprem-postgresql-connector.md#linked-service-properties)
2. Einen verknüpften Dienst des Typs [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties)
3. Ein [Eingabedataset](data-factory-create-datasets.md) des Typs [RelationalTable](data-factory-onprem-postgresql-connector.md#dataset-properties)
4. Ein [Ausgabedataset](data-factory-create-datasets.md) des Typs [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)
5. Die [Pipeline](data-factory-create-pipelines.md) mit Kopieraktivität, die [RelationalSource](data-factory-onprem-postgresql-connector.md#copy-activity-properties) und [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties) verwendet.

Das Beispiel kopiert Daten stündlich aus einem Abfrageergebnis in einer PostgreSQL-Datenbank in ein Blob. Die bei diesen Beispielen verwendeten JSON-Eigenschaften werden in den Abschnitten beschrieben, die auf die Beispiele folgen.

Als Erstes richten Sie das Datenverwaltungsgateway ein. Anweisungen dazu finden Sie im Artikel [Verschieben von Daten zwischen lokalen Standorten und Cloud](data-factory-move-data-between-onprem-and-cloud.md) .

**Mit PostgreSQL verknüpfter Dienst:**

    {
        "name": "OnPremPostgreSqlLinkedService",
        "properties": {
            "type": "OnPremisesPostgreSql",
            "typeProperties": {
                "server": "<server>",
                "database": "<database>",
                "schema": "<schema>",
                "authenticationType": "<authentication type>",
                "username": "<username>",
                "password": "<password>",
                "gatewayName": "<gatewayName>"
            }
        }
    }

**Mit Azure-Blobspeicher verknüpfter Dienst:**

    {
      "name": "AzureStorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<AccountName>;AccountKey=<AccountKey>"
        }
      }
    }

**PostgreSQL-Eingabedataset:**

Im Beispiel wird vorausgesetzt, dass Sie die Tabelle "MyTable" in PostgreSQL erstellt haben, die für Zeitreihendaten eine Spalte namens "timestamp" enthält.

Durch Festlegen von „external“ auf „true“ wird dem Data Factory-Dienst mitgeteilt, dass das Dataset für die Data Factory extern ist und nicht durch eine Aktivität in der Data Factory erzeugt wird.

    {
        "name": "PostgreSqlDataSet",
        "properties": {
            "type": "RelationalTable",
            "linkedServiceName": "OnPremPostgreSqlLinkedService",
            "typeProperties": {},
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true,
            "policy": {
                "externalData": {
                    "retryInterval": "00:01:00",
                    "retryTimeout": "00:10:00",
                    "maximumRetry": 3
                }
            }
        }
    }


**Azure-Blob-Ausgabedataset:**

Daten werden stündlich in ein neues Blob geschrieben ("frequency": "hour", "interval": 1). Ordnerpfad und Dateiname des Blobs werden basierend auf der Startzeit des Slices, der verarbeitet wird, dynamisch ausgewertet. Im Ordnerpfad werden Jahr, Monat, Tag und die Stundenteile der Startzeit verwendet.

    {
        "name": "AzureBlobPostgreSqlDataSet",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/postgresql/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
                "format": {
                    "type": "TextFormat",
                    "rowDelimiter": "\n",
                    "columnDelimiter": "\t"
                },
                "partitionedBy": [
                    {
                        "name": "Year",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "yyyy"
                        }
                    },
                    {
                        "name": "Month",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "MM"
                        }
                    },
                    {
                        "name": "Day",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "dd"
                        }
                    },
                    {
                        "name": "Hour",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "HH"
                        }
                    }
                ]
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }


**Pipeline mit Kopieraktivität:**

Die Pipeline enthält eine Kopieraktivität, die für die Verwendung der Ein- und Ausgabedatasets und für eine stündliche Ausführung konfiguriert ist. In der JSON-Definition der Pipeline ist der Typ **source** auf **RelationalSource** und der Typ **sink** auf **BlobSink** festgelegt. Die für die **query** -Eigenschaft angegebene SQL-Abfrage wählt Daten in der Tabelle "public.usstates" in der PostgreSQL-Datenbank aus.

    {
        "name": "CopyPostgreSqlToBlob",
        "properties": {
            "description": "pipeline for copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "RelationalSource",
                            "query": "select * from \"public\".\"usstates\""
                        },
                        "sink": {
                            "type": "BlobSink"
                        }
                    },
                    "inputs": [
                        {
                            "name": "PostgreSqlDataSet"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobPostgreSqlDataSet"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00",
                        "concurrency": 1
                    },
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                    },
                    "name": "PostgreSqlToBlob"
                }
            ],
            "start": "2014-06-01T18:00:00Z",
            "end": "2014-06-01T19:00:00Z"
        }
    }

## <a name="type-mapping-for-postgresql"></a>Typzuordnung für PostgreSQL
Wie im Artikel [Datenverschiebungsaktivitäten](data-factory-data-movement-activities.md) beschrieben, führt die Kopieraktivität mithilfe der folgenden beiden Schritte automatische Typkonvertierungen von Quelltypen in Senkentypen durch:

1. Konvertieren von systemeigenen Quelltypen in den .NET-Typ
2. Konvertieren vom .NET-Typ in systemeigenen Senkentyp

Beim Verschieben von Daten in PostgreSQL werden die folgenden Zuordnungen zwischen PostgreSQL-Typ und .NET-Typ verwendet:

| Typ "PostgreSQL-Datenbank" | PostgreSQL-Aliase | Typ ".NET Framework" |
| --- | --- | --- |
| abstime | |Datetime |
| bigint |int8 |Int64 |
| bigserial |serial8 |Int64 |
| bit [ (n) ] | |Byte[], String |
| bit varying [ (n) ] |varbit |Byte[], String |
| Boolescher Wert |bool |Boolescher Wert |
| box | |Byte[], String |
| bytea | |Byte[], String |
| character [ (n) ] |char [ (n) ] |String |
| character varying [ (n) ] |varchar [ (n) ] |String |
| cid | |String |
| cidr | |String |
| circle | |Byte[], String |
| date | |Datetime |
| daterange | |String |
| double precision |float8 |Double |
| inet | |Byte[], String |
| intarry | |String |
| int4range | |String |
| int8range | |String |
| integer |int, int4 |Int32 |
| interval [ fields ] [ (p) ] | |Timespan |
| json | |String |
| jsonb | |Byte[] |
| line | |Byte[], String |
| lseg | |Byte[], String |
| macaddr | |Byte[], String |
| money | |Decimal |
| numeric [ (p, s) ] |decimal [ (p, s) ] |Decimal |
| numrange | |String |
| oid | |Int32 |
| path | |Byte[], String |
| pg_lsn | |Int64 |
| point | |Byte[], String |
| polygon | |Byte[], String |
| real |float4 |Single |
| smallint |int2 |Int16 |
| smallserial |serial2 |Int16 |
| serial |serial4 |Int32 |
| Text | |String |

## <a name="map-source-to-sink-columns"></a>Zuordnen von Quell- zur Senkenspalten
Weitere Informationen zum Zuordnen von Spalten im Quelldataset zu Spalten im Senkendataset finden Sie unter [Zuordnen von Datasetspalten in Azure Data Factory](data-factory-map-columns.md).

## <a name="repeatable-read-from-relational-sources"></a>Wiederholbare Lesevorgänge aus relationalen Quellen
Beim Kopieren von Daten aus relationalen Datenspeichern müssen Sie die Wiederholbarkeit berücksichtigen, um unbeabsichtigte Ergebnisse zu vermeiden. Sie können einen Slice in Azure Data Factory manuell erneut ausführen. Sie können auch eine Wiederholungsrichtlinie für ein Dataset konfigurieren, sodass ein Slice erneut ausgeführt wird, wenn ein Fehler auftritt. Wenn ein Slice erneut ausgeführt wird, müssen Sie sicherstellen, dass dieselben Daten gelesen werden – egal wie oft ein Slice ausgeführt wird. Weitere Informationen finden Sie unter [Wiederholbare Lesevorgänge aus relationalen Quellen](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).

## <a name="performance-and-tuning"></a>Leistung und Optimierung
Der Artikel [Handbuch zur Leistung und Optimierung der Kopieraktivität](data-factory-copy-activity-performance.md) beschreibt wichtige Faktoren, die sich auf die Leistung der Datenverschiebung (Kopieraktivität) in Azure Data Factory auswirken, sowie verschiedene Möglichkeiten zur Leistungsoptimierung.

