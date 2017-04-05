---
title: "Ressourceneinschränkungen für Azure SQL-Datenbanken | Microsoft Docs"
description: "Diese Seite beschreibt einige allgemeine Ressourceneinschränkungen für Azure SQL-Datenbanken."
services: sql-database
documentationcenter: na
author: janeng
manager: jhubbard
editor: 
ms.assetid: 884e519f-23bb-4b73-a718-00658629646a
ms.service: sql-database
ms.custom: overview
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 03/06/2017
ms.author: janeng
translationtype: Human Translation
ms.sourcegitcommit: 503f5151047870aaf87e9bb7ebf2c7e4afa27b83
ms.openlocfilehash: 61eac09668b14a98a42b1907a54577d80eb933a6
ms.lasthandoff: 03/28/2017


---
# <a name="azure-sql-database-resource-limits"></a>Ressourceneinschränkungen für Azure SQL-Datenbank
## <a name="overview"></a>Übersicht
Azure SQL-Datenbank verwaltet die für eine Datenbank verfügbaren Ressourcen mithilfe zweier verschiedener Mechanismen: **Ressourcenkontrolle** und **Durchsetzung von Grenzen**. In diesem Thema werden diese beiden Hauptbereiche der Ressourcenverwaltung behandelt.

## <a name="resource-governance"></a>Ressourcenkontrolle
Eines der Entwurfsziele der Tarife Basic, Standard und Premium ist es, dass sich die Azure SQL-Datenbank so verhält, als ob sie auf einem eigenen Computer isoliert von anderen Datenbanken ausgeführt wird. Die Ressourcenkontrolle emuliert dieses Verhalten. Wenn die aggregierte Ressourcenverwendung die maximal verfügbaren Ressourcen an CPU, Arbeitsspeicher, Protokoll-E/A und Daten-E/A erreicht, die der Datenbank zugeordnet sind, reiht die Ressourcenkontrolle die Ausführung von Abfragen in eine Warteschlange ein und ordnet den Abfragen in der Warteschlange Ressourcen zu, während sie nach und nach freigegeben werden.

Wie bei einem dedizierten Computer führt das Nutzen aller verfügbarer Ressourcen zu einer längeren Ausführung der derzeit ausgeführten Abfragen, was Befehlstimeouts auf dem Client zur Folge haben kann. Anwendungen mit aggressiver Wiederholungslogik und solche, die Abfragen für die Datenbank mit hoher Frequenz ausführen, können bei dem Versuch auf Fehlermeldungen stoßen, neue Abfragen auszuführen, wenn das Limit gleichzeitiger Anforderungen bereits erreicht wurde.

### <a name="recommendations"></a>Empfehlungen:
Überwachen Sie die Ressourcenverwendung und die durchschnittlichen Antwortzeiten für Abfragen, wenn die maximale Auslastung einer Datenbank fast erreicht wurde. Falls längere Abfragewartezeiten auftreten, haben Sie in der Regel drei Optionen:

1. Reduzieren Sie die Anzahl der eingehenden Anfragen in der Datenbank, um ein Timeout und das Anhäufen von Anfragen zu verhindern.
2. Weisen Sie der Datenbank eine höhere Leistungsstufe zu.
3. Optimieren Sie Abfragen, um die Ressourcenverwendung für jede Abfrage zu reduzieren. Weitere Informationen finden Sie im Abschnitt "Abfrageoptimierung/Abfragehinweise" im Artikel "Leitfaden zur Azure SQL-Datenbankleistung".

## <a name="enforcement-of-limits"></a>Durchsetzung von Grenzen
Durch das Verweigern neuer Anforderungen bei Erreichen der Limits werden andere Ressourcen als CPU, Arbeitsspeicher, Protokoll-E/A und Daten-E/A durchgesetzt. Wenn eine Datenbank das konfigurierte maximale Größenlimit erreicht, tritt bei Einfüge- und Aktualisierungsvorgängen ein Fehler auf. Auswahl- und Löschvorgänge funktionieren hingegen weiterhin. Clients erhalten abhängig vom erreichten Limit eine [Fehlermeldung](sql-database-develop-error-messages.md).

Beispielsweise werden die Anzahl der Verbindungen mit einer SQL-Datenbank und die Anzahl der gleichzeitigen Anforderungen, die verarbeitet werden können, beschränkt. Mit einer SQL-Datenbank kann die Anzahl der Verbindungen mit der Datenbank größer als die Anzahl der gleichzeitigen Anforderungen sein, um Verbindungspooling zu unterstützen. Während die Anzahl der verfügbaren Verbindungen einfach von der Anwendung gesteuert werden kann, ist die Anzahl paralleler Anforderungen oft schwieriger zu schätzen und zu steuern. Insbesondere bei Spitzenbelastungen können Fehler auftreten, wenn die Anwendung entweder zu viele Anforderungen sendet oder die Datenbank seine Ressourcengrenzen erreicht und anfängt, Workerthreads aufgrund einer längeren Ausführungszeit von Abfragen anzuhäufen.

## <a name="service-tiers-and-performance-levels"></a>Tarife und Leistungsebenen
Es gibt Tarife und Leistungsstufen sowohl für Einzeldatenbanken als auch für Pools für elastische Datenbanken.

### <a name="single-databases"></a>Einzeldatenbanken
Für eine einzelne Datenbank sind deren Einschränkungen durch die Dienstebene und Leistungsstufe der Datenbank definiert. In der folgenden Tabelle sind die Merkmale von Basic-, Standard- und Premium-Datenbanken für unterschiedliche Leistungsstufen beschrieben.

[!INCLUDE [SQL DB service tiers table](../../includes/sql-database-service-tiers-table.md)]

> [!IMPORTANT]
> Kunden mit den Leistungsstufen P11 und P15 können bis zu 4TB Speicher ohne Aufpreis nutzen. Diese 4-TB-Option ist derzeit in folgenden Regionen in der Public Preview verfügbar: USA, Osten 2; USA, Westen; Europa, Westen; Asien, Südosten; Japan, Osten; Australien, Osten; Kanada, Mitte und Kanada, Osten.
>

### <a name="elastic-pools"></a>Elastische Pools
[Elastische Pools](sql-database-elastic-pool.md) nutzen Ressourcen in verschiedenen Datenbanken im Pool gemeinsam. In der folgenden Tabelle sind die Merkmale von elastischen Basic-, Standard- und Premium-Pools beschrieben.

[!INCLUDE [SQL DB service tiers table for elastic databases](../../includes/sql-database-service-tiers-table-elastic-pools.md)]

Eine erweiterte Definition der einzelnen Ressourcen, die in den vorangehenden Tabellen aufgeführt werden, können Sie der Beschreibungen in [Funktionen und Grenzen der Serviceebenen](sql-database-performance-guidance.md#service-tier-capabilities-and-limits)entnehmen. Eine Übersicht über die Tarife finden Sie unter [Tarife und Leistungsstufen für Azure SQL-Datenbank](sql-database-service-tiers.md).

## <a name="other-sql-database-limits"></a>Weitere Einschränkungen für SQL-Datenbanken
| Bereich | Begrenzung | Beschreibung |
| --- | --- | --- |
| Datenbanken mit automatisiertem Export pro Abonnement |10 |Automatisierter Export ermöglicht es Ihnen, einen benutzerdefinierten Zeitplan für die Sicherung Ihrer SQL-Datenbanken zu erstellen. Die Vorschau für dieses Feature endet am 1. März 2017.  |
| Datenbanken pro Server |Bis zu 5.000 |Bis zu 5.000 Datenbanken pro Server sind auf V12-Servern zulässig. |
| DTUs pro Server. |45000 |45.000 DTUs pro Server sind auf V12-Servern für die Bereitstellung von eigenständigen Datenbanken und Pools für elastische Datenbanken zugelassen. Die maximale Anzahl von eigenständigen Datenbanken und Pools pro Server wird nur durch die Anzahl der DTUs pro Server begrenzt.  

> [!IMPORTANT]
> Der automatisierte Export von Azure SQL-Datenbanken ist jetzt als Vorschau verfügbar und wird am 1. März 2017 eingestellt. Seit dem 1. Dezember 2016 ist das Konfigurieren des automatisierten Exports für SQL-Datenbanken nicht mehr möglich. Alle vorhandenen Aufträge für automatisierten Export werden bis zum 1. März 2017 weiterhin ausgeführt. Seit dem 1. Dezember 2016 können Sie die [langfristige Sicherungsaufbewahrung](sql-database-long-term-retention.md) oder [Azure Automation](../automation/automation-intro.md) zum regelmäßigen Archivieren von SQL-Datenbanken mithilfe von PowerShell nach einem Zeitplan Ihrer Wahl verwenden. Als Beispielskript können Sie das [Beispielskript von GitHub](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-automation-automated-export) herunterladen.
>


## <a name="resources"></a>Ressourcen
[Einschränkungen für Azure-Abonnements und Dienste, Kontingente und Einschränkungen](../azure-subscription-service-limits.md)

[Dienst- und Leistungsebenen für Azure SQL-Datenbanken](sql-database-service-tiers.md)

[Fehlermeldungen für Clientprogramme der SQL-Datenbank](sql-database-develop-error-messages.md)

