---
title: "Häufig gestellte Fragen zu Azure SQL Data Warehouse | Microsoft-Dokumentation"
description: "Dieser Artikel enthält häufig gestellte Fragen von Kunden und Entwicklern zu Azure SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: johnmac
editor: 
ms.assetid: 812CA525-3BF3-49DF-8DF3-FB4342464F4F
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 3/1/2017
ms.author: elbutter
translationtype: Human Translation
ms.sourcegitcommit: 2f03ba60d81e97c7da9a9fe61ecd419096248763
ms.openlocfilehash: 7a752bfb349d2730537538f6856fe431204d3329
ms.lasthandoff: 03/04/2017


---

# <a name="sql-data-warehouse-frequently-asked-questions"></a>Häufig gestellte Fragen zu SQL Data Warehouse

## <a name="general"></a>Allgemein

F: Welche Datensicherheit bietet SQL DW?

A: SQL DW bietet verschiedene Lösungen für den Schutz von Daten, etwa transparente Datenverschlüsselung (Transparent Data Encryption, TDE) und Überwachung. Weitere Informationen finden Sie unter [Sicherheit].

F: Wo finde ich Informationen dazu, welche rechtlichen oder geschäftsspezifischen Standards SQL DW erfüllt?

A: Auf der Seite [Microsoft-Compliance] finden Sie verschiedene nach Produkt sortierte Compliance-Angebote (beispielsweise für SOC und ISO). Wählen Sie zunächst einen Compliancetitel aus. Erweitern Sie anschließend rechts auf der Seite im Abschnitt mit den konformen Microsoft-Clouddiensten den Knoten „Azure“, um die richtlinienkonformen Azure-Dienste anzuzeigen.
 
F: Kann ich eine Verbindung mit PowerBI herstellen?

A: Ja. PowerBI unterstützt zwar direkte Abfragen mit SQL DW, ist jedoch nicht für eine große Anzahl von Benutzern oder eine große Menge von Echtzeitdaten vorgesehen. Für die Verwendung von PowerBI in der Produktion wird die Nutzung von PowerBI zusätzlich zu Azure Analysis Services oder Analysis Service IaaS empfohlen. 

F: Welche Kapazitätsgrenzen hat SQL Data Warehouse?

A: Informationen finden Sie auf der Seite mit den [Kapazitätsgrenzen]. 

## <a name="user-support"></a>Unterstützung für Benutzer

F: Ich habe einen Funktionswunsch. Wie kann ich diesen übermitteln?

A: Wenn Sie einen Funktionswunsch haben, können Sie ihn über die Seite [UserVoice] einsenden.

F: Wo finde ich Informationen zu x?

A: Wenn Sie Hilfe bei der Entwicklung mit SQL Data Warehouse benötigen, können Sie Fragen auf der Seite [Stack Overflow] stellen. 

F: Wie übermittle ich ein Supportticket?

A: [Supporttickets] können über das Azure-Portal erstellt werden.

## <a name="sql-languagefeature-support"></a>Unterstützung von SQL-Sprache/-Features 

F: Welche Datentypen werden von SQL Data Warehouse unterstützt?

A: Informationen finden Sie im Thema zu SQL Data Warehouse-[Datentypen].

F: Welche Tabellenfunktionen werden unterstützt?

A: SQL Data Warehouse unterstützt zahlreiche Funktionen. Die nicht unterstützten Funktionen sind unter [Nicht unterstützte Tabellenfunktionen] dokumentiert.

## <a name="tooling-and-administration"></a>Tools und Verwaltung

F: Werden Datenbankprojekte in Visual Studio unterstützt?

A: Derzeit werden keine Datenbankprojekte in Visual Studio für SQL Data Warehouse unterstützt. Wenn Sie für die Einführung dieses Features stimmen möchten, besuchen Sie unsere User Voice-Seite für die [Anforderung von Features für Datenbankprojekte].

F: Unterstützt SQL Data Warehouse REST-APIs?

A: Ja. Die meisten REST-Funktionen, die mit SQL-Datenbank verwendet werden können, sind auch für SQL Data Warehouse verfügbar. API-Informationen finden Sie auf den REST-Dokumentationsseiten oder auf [MSDN].


## <a name="loading"></a>Laden

F: Welche Clienttreiber werden unterstützt?

A: Treiberunterstützung für DW finden Sie auf der Seite [Verbindungszeichenfolgen].

F: Welche Dateiformate werden von PolyBase und SQL Data Warehouse unterstützt?

A: Orc, RC, Parquet und flacher, durch Trennzeichen getrennter Text

F: Mit welchen Komponenten kann ich in SQL DW mithilfe von PolyBase Verbindungen herstellen? 

A: [Azure Data Lake Store] und [Azure Storage Blobs]

F: Ist ein Berechnungs-Pushdown möglich, wenn ich eine Verbindung mit Azure Storage Blobs oder ADLS herstelle? 

A: Nein. PolyBase in SQL DW interagiert nur mit den Speicherkomponenten. 

F: Kann ich eine Verbindung mit HDI herstellen?

A: HDI kann entweder ADLS oder WASB als HDFS-Ebene verwenden. Wenn Sie eine der beiden Komponenten als HDFS-Ebene verwenden, können Sie diese Daten in SQL DW laden. Sie können jedoch keine Pushdown-Berechnung für die HDI-Instanz ausführen. 

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu SQL Data Warehouse im Allgemeinen finden Sie auf der Seite mit der [Übersicht].


<!-- Article references -->
[UserVoice]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[Verbindungszeichenfolgen]: ./sql-data-warehouse-connection-strings.md
[Stack Overflow]: http://stackoverflow.com/questions/tagged/azure-sqldw
[Supporttickets]: ./sql-data-warehouse-get-started-create-support-ticket.md
[Sicherheit]: ./sql-data-warehouse-overview-manage-security.md
[Microsoft-Compliance]: https://www.microsoft.com/en-us/trustcenter/compliance/complianceofferings
[Kapazitätsgrenzen]: ./sql-data-warehouse-service-capacity-limits.md
[Datentypen]: ./sql-data-warehouse-tables-data-types.md
[Nicht unterstützte Tabellenfunktionen]: ./sql-data-warehouse-tables-overview.md#unsupported-table-features
[Azure Data Lake Store]: ./sql-data-warehouse-load-from-azure-data-lake-store.md 
[Azure Storage Blobs]: ./sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md
[Anforderung von Features für Datenbankprojekte]: https://feedback.azure.com/forums/307516-sql-data-warehouse/suggestions/13313247-database-project-from-visual-studio-to-support-azu
[MSDN]: https://msdn.microsoft.com/en-us/library/azure/mt163685.aspx
[Übersicht]: ./sql-data-warehouse-overview-faq.md
