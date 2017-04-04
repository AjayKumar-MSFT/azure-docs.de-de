---
title: Anmerkungen zu dieser Version des Datenverwaltungsgateways | Microsoft Docs
description: "Versionshinweise für Datenverwaltungsgateway"
services: data-factory
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 14762e82-76d9-41c4-ba9f-14a54da29c36
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/03/2017
ms.author: spelluru
published: true
translationtype: Human Translation
ms.sourcegitcommit: db0face48d84680eabd82245bd38bd49b204f9ae
ms.openlocfilehash: a88b791bad9f71f16700ccc7efdee8ef493478a9
ms.lasthandoff: 02/13/2017

---
# <a name="release-notes-for-data-management-gateway"></a>Versionshinweise für Datenverwaltungsgateway
Eine Herausforderung für die moderne Datenintegration ist das nahtlose Verschieben von Daten zwischen lokalen Speicherorten und der Cloud. Dank Data Factory verläuft diese Integration mit dem Datenverwaltungsgateway nahtlos. Es handelt sich um einen Agent, den Sie lokal installieren können, um eine hybride Datenverschiebung zu ermöglichen.

In den folgenden Artikeln finden Sie ausführliche Informationen zum Datenverwaltungsgateway und zu dessen Verwendung: 

*  [Gateway zur Datenverwaltung](data-factory-data-management-gateway.md)
*  [Verschieben von Daten zwischen lokalen Quellen und der Cloud mit Azure Data Factory](data-factory-move-data-between-onprem-and-cloud.md) 


## <a name="current-version-2762192"></a>AKTUELLE VERSION (2.7.6219.2)

### <a name="whats-new"></a>Neuigkeiten
- Sie können sich jetzt über den Dienstprinzipal bei Ihrem Azure Data Lake Store authentifizieren. Zuvor wurde nur OAuth unterstützt.
- Wir haben einen neuen Treiber für das Auslesen von Daten aus Oracle in lokalen Datenspeicher im Gateway in das Paket integriert.

### <a name="enhancements-"></a>Verbesserungen
- Verbesserte Leistung beim Lesen von Daten aus Oracle-Datenquellen.
- Behoben: Problem beim Ablauf des OAuth-Tokens der OData-Quelle.
- Behoben: Problem beim Lesen von Oracle-Dezimalzahl von mehr als 28 Bits.


## <a name="earlier-versions"></a>Frühere Versionen

## <a name="2661922"></a>2.6.6192.2
### <a name="whats-new"></a>Neuigkeiten
- Kunden können Feedback zur Gatewayregistrierung bereitstellen.
- Ein neues Komprimierungsformat wird unterstützt: ZIP (verkleinern)

### <a name="enhancements-"></a>Verbesserungen
- Leistungsverbesserung für Oracle Sink, HDFS-Quelle.
- Fehlerbehebung für automatische Gatewayaktualisierung, Kapazität für parallele Gatewayverarbeitung.


## <a name="2561641"></a>2.5.6164.1
### <a name="enhancements"></a>Verbesserungen
- Verbesserte und stabilere Gatewayregistrierung: Während der Gatewayregistrierung kann nun der Fortschrittsstatus nachverfolgt werden. Die Registrierung wird dadurch benutzerfreundlicher.
- Optimierter Vorgang zur Gatewaywiederherstellung: Sie können das Gateway auch ohne die Gatewaysicherungsdatei mit diesem Update wiederherstellen. Dazu müssen Sie die Anmeldeinformationen für den verknüpften Dienst im Portal zurücksetzen.
- Fehlerbehebung

## <a name="2461511"></a>2.4.6151.1

### <a name="whats-new"></a>Neuigkeiten

- Sie können jetzt die Anmeldeinformationen für Datenquellen lokal speichern. Die Anmeldeinformationen werden verschlüsselt. Die Anmeldeinformationen für Datenquellen können mit einer Sicherungsdatei, die über das vorhandene Gateway exportiert werden kann, wiederhergestellt werden (alles lokal). 

### <a name="enhancements-"></a>Verbesserungen

- Bessere und stabilere Gatewayregistrierung.
- Unterstützt die automatische Erkennung der QuoteChar-Konfiguration für das Textformat im Kopier-Assistenten. Zudem wurde die Gesamtgenauigkeit bei der Formaterkennung verbessert.

## <a name="2361002"></a>2.3.6100.2

- Unterstützt die automatische firstRowAsHeader- und SkipLineCount-Erkennung im Kopier-Assistenten für Textdateien im lokalen Dateisystem und HDFS.
- Höhere Stabilität der Netzwerkverbindung zwischen Gateway und Service Bus
- Einige Fehlerbehebungen


## <a name="2260721"></a>2.2.6072.1

*  Unterstützt das Festlegen eines HTTP-Proxys für das Gateway mithilfe des Gatewaykonfigurations-Managers. Wenn der HTTP-Proxy konfiguriert ist, erfolgt der Zugriff auf Azure-Blobs, Azure-Tabellen, Azure Data Lake und DocumentDB über diesen Proxy.
*  Unterstützt die Verarbeitung von Kopfzeilen für TextFormat beim Kopieren von Daten aus und nach Azure-Blobs, Azure Data Lake Store, lokalem Dateisystem und lokalem HDFS.
*  Unterstützt das Kopieren von Daten aus Anfüge- und Seitenblobs zusätzlich zu den bereits unterstützten Blockblobs.
*  Führt den neuen Gatewaystatus **Online (begrenzt)**ein, der darauf hinweist, dass die Hauptfunktion des Gateways funktioniert, nicht jedoch die Unterstützung des interaktiven Betriebs für den Kopier-Assistenten.
*  Erweitert die Stabilität der Gatewayregistrierung mithilfe von Registrierungsschlüsseln.

## <a name="216040"></a>2.1.6040.

*  Der DB2-Treiber ist jetzt im Gatewayinstallationspaket enthalten. Sie müssen ihn nicht separat installieren. 
*  Der DB2-Treiber unterstützt nun z/OS und DB2 für i (AS/400) sowie die bereits unterstützten Plattformen (Linux, Unix und Windows). 
*  Unterstützt die Verwendung von DocumentDB als Quelle oder Ziel für lokale Datenspeicher.
*  Unterstützt das Kopieren von Daten in kalte bzw. heiße Blobspeicher (und umgekehrt) sowie das bereits unterstützte allgemeine Speicherkonto. 
*  Ermöglicht die Verbindung mit einer lokalen SQL Server-Instanz über ein Gateway mit Remoteanmeldeberechtigungen.  

## <a name="2060131"></a>2.0.6013.1

*  Sie können die Sprache/Kultur auswählen, die von einem Gateway während der manuellen Installation verwendet werden soll.

*  Wenn das Gateway nicht wie erwartet funktioniert, können Sie Gatewayprotokolle der letzten sieben Tage an Microsoft senden, um die Behebung des Problems zu erleichtern. Wenn das Gateway nicht mit dem Clouddienst verbunden ist, können Sie Gatewayprotokolle speichern und archivieren.  

*  Verbesserte Benutzeroberfläche für Gatewaykonfigurations-Manager:

    *  Der Gatewaystatus ist auf der Registerkarte „Start“ besser sichtbar.

    *  Neu organisierte und vereinfachte Steuerelemente.

    *  Sie können Daten aus einem Speicher mithilfe des [in der Vorschau befindlichen codefreien Kopiertools](data-factory-copy-data-wizard-tutorial.md)kopieren. Unter [Gestaffeltes Kopieren](data-factory-copy-activity-performance.md#staged-copy) finden Sie allgemeine Details zu diesem Feature. 
*  Sie können das Datenverwaltungsgateway nutzen, um Daten direkt aus einer lokalen SQL Server-Datenbank in Azure Machine Learning zu erfassen.

*  Leistungsverbesserungen

    * Verbessern der Leistung beim Anzeigen des Schemas bzw. einer Vorschau im Vergleich zu SQL Server im in der Vorschauphase befindlichen codelosen Kopiertool.

## <a name="11259531"></a>1.12.5953.1

*  Fehlerbehebungen

## <a name="11159181"></a>1.11.5918.1

*  Die maximale Größe des Gatewayereignisprotokolls wurde von 1 MB auf 40 MB erhöht.

*  Wenn während der automatischen Aktualisierung des Gateways ein Neustart erforderlich ist, wird ein Warnungsdialogfeld angezeigt. Sie können auswählen, ob das Gateway jetzt oder zu einem späteren Zeitpunkt neugestartet werden soll. 

*  Wenn bei der automatischen Aktualisierung ein Fehler auftritt, versucht das Gatewayinstallationsprogramm höchstens dreimal, die automatische Aktualisierung zu wiederholen.

*  Leistungsverbesserungen

    * Verbessern Sie die Leistung beim Laden großer Tabellen von lokalen Servern in Kopierszenarieren ohne Code.

*  Fehlerbehebungen

## <a name="11058921"></a>1.10.5892.1

*  Leistungsverbesserungen

*  Fehlerbehebungen

## <a name="1958652"></a>1.9.5865.2

*  Automatische Aktualisierung ohne Benutzereingriff
*  Neues Taskleistensymbol mit Gatewaystatusanzeige
*  „Jetzt aktualisieren“ auf dem Client
*  Festlegen der geplanten Updatezeit
*  PowerShell-Skript für Aktivierung/Deaktivierung der automatischen Aktualisierung
*  Unterstützung für JSON-Format  
*  Leistungsverbesserungen
*  Fehlerbehebungen

## <a name="1858221"></a>1.8.5822.1

*  Verbesserung der Problembehandlung
*  Leistungsverbesserungen
*  Fehlerbehebungen

### <a name="1757951"></a>1.7.5795.1

*  Leistungsverbesserungen
*  Fehlerbehebungen

### <a name="1757641"></a>1.7.5764.1

*  Leistungsverbesserungen
*  Fehlerbehebungen

### <a name="1657351"></a>1.6.5735.1

*  Unterstützung für lokale HDFS-Quelle/Senke
*  Leistungsverbesserungen
*  Fehlerbehebungen

### <a name="1656961"></a>1.6.5696.1

*  Leistungsverbesserungen
*  Fehlerbehebungen

### <a name="1656761"></a>1.6.5676.1

*  Unterstützung für Diagnosetools im Konfigurations-Manager
*  Unterstützung für Tabellenspalten für tabellarische Datenquellen für Azure Data Factory
*  SQL-DW-Unterstützung für Azure Data Factory
*  Reclusive-Unterstützung in BlobSource und FileSource für Azure Data Factory
*  Unterstützung von CopyBehavior – MergeFiles, PreserveHierarchy und FlattenHierarchy in BlobSink und FileSink mit binärer Kopie für Azure Data Factory
*  Unterstützung für Kopieraktivität mit Fortschrittsbericht für Azure Data Factory
*  Unterstützung für Überprüfung der Datenquellenverbindung für Azure Data Factory
*  Fehlerbehebungen

### <a name="1656721"></a>1.6.5672.1

*  Unterstützung des Tabellennamens für ODBC-Datenquelle für Azure Data Factory
*  Leistungsverbesserungen
*  Fehlerbehebungen

### <a name="1656581"></a>1.6.5658.1

*  Unterstützung der Dateisenke für Azure Data Factory
*  Unterstützung für Beibehaltung der Hierarchie in binärer Kopie für Azure Data Factory
*  Unterstützung der Kopieraktivität-Idempotenz für Azure Data Factory
*  Fehlerbehebungen

### <a name="1656401"></a>1.6.5640.1

*  Unterstützung von drei weiteren Datenquellen für Azure Data Factory (ODBC, OData, HDFS)
*  Unterstützung für Anführungszeichen im CSV-Parser für Azure Data Factory
*  Unterstützung der Komprimierung (BZip2)
*  Fehlerbehebungen

### <a name="1556121"></a>1.5.5612.1

*  Unterstützung von fünf relationalen Datenbanken für Azure Data Factory (MySQL, PostgreSQL, DB2, Teradata und Sybase)
*  Unterstützung der Komprimierung (Gzip und Deflate)
*  Leistungsverbesserungen
*  Fehlerbehebungen

### <a name="1455491"></a>1.4.5549.1

*  Unterstützung von Oracle-Datenquellen für Azure Data Factory hinzugefügt
*  Leistungsverbesserungen
*  Fehlerbehebungen

### <a name="1454921"></a>1.4.5492.1

*  Einheitliche Binärdatei, die sowohl Microsoft Azure Data Factory als auch Office 365 Power BI-Dienste unterstützt
*  Optimierung von Konfigurationsbenutzeroberfläche und Registrierungsprozess
*  Azure Data Factory – Azure Ingress- und Egress-Unterstützung für SQL Server-Datenquelle

### <a name="1253031"></a>1.2.5303.1

*  Beheben von Problemen mit der Zeitüberschreitung, um zeitaufwändigere Datenquellenverbindungen zu unterstützen 

### <a name="1155268"></a>1.1.5526.8

*  Erfordert .NET Framework 4.5.1 als Voraussetzung während des Setups.

### <a name="1051442"></a>1.0.5144.2

*  Keine Änderungen, die Azure Data Factory-Szenarien betreffen.

