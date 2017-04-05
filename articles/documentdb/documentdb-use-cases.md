---
title: "Allgemeine Anwendungsfälle und NoSQL-Szenarien für Azure DocumentDB | Microsoft-Dokumentation"
description: "Erfahren Sie mehr über die fünf wichtigsten Anwendungsfälle für DocumentDB: benutzergenerierte Inhalte, Ereignisprotokollierung, Katalogdaten, Benutzereinstellungsdaten und das Internet der Dinge (Internet of Things, IoT)."
services: documentdb
author: h0n
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: eca68a58-1a8c-4851-8cf8-6e4d2b889905
ms.service: documentdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/23/2017
ms.author: hawong
translationtype: Human Translation
ms.sourcegitcommit: 0bec803e4b49f3ae53f2cc3be6b9cb2d256fe5ea
ms.openlocfilehash: 9a0ba106b4c2aded8aaac673dfba3610bd4ca0fb
ms.lasthandoff: 03/24/2017


---

# <a name="common-documentdb-use-cases"></a>Häufige Anwendungsfälle für DocumentDB
Dieser Artikel bietet eine Übersicht über verschiedene häufige Anwendungsfälle für DocumentDB.  Die Empfehlungen in diesem Artikel dienen als Ausgangspunkt für die Entwicklung Ihrer Anwendung mit DocumentDB.   

Nach Lesen dieses Artikels können Sie die folgenden Fragen beantworten: 

* Was sind häufige Anwendungsfälle für DocumentDB?
* Welche Vorteile bietet die Verwendung von DocumentDB für Verkaufsanwendungen?
* Welche Vorteile bietet die Verwendung von DocumentDB als Datenspeicher für Systeme für das Internet der Dinge (Internet of Things, IoT)?
* Welche Vorteile bietet die Verwendung von DocumentDB für Web- und mobile Anwendungen?

## <a name="common-use-cases-for-documentdb"></a>Häufige Anwendungsfälle für DocumentDB
Azure DocumentDB ist eine allgemeine NoSQL-Datenbank, die in einer Vielzahl von Anwendungen und Anwendungsfällen verwendet wird. Sie eignet sich für alle Anwendungen, die kurze Antwortzeiten im Millisekundenbereich benötigen und schnell skalieren müssen. Aufgrund der folgenden Attribute eignet sich DocumentDB besonders gut für Hochleistungsanwendungen.

* DocumentDB partitioniert Ihre Daten systemintern, um eine höhere Verfügbarkeit und Skalierbarkeit zu gewährleisten.
* DocumentDB verfügt über SSD-gestützten Speicher mit niedriger Latenz und kurzen Antwortzeiten im Millisekundenbereich.
* Da DocumentDB Konsistenzebenen wie „eventual“, „session“ und „bounded-staleness“ unterstützt, wird ein optimales Preis-Leistungs-Verhältnis ermöglicht. 
* DocumentDB verfügt über ein flexibles, datenfreundliches Preismodell, das Speicher und Durchsatz unabhängig voneinander misst.
* Mit dem reservierten Durchsatzmodell in DocumentDB können Sie mit der Anzahl von Lese-und Schreibvorgängen anstelle von CPU-Leistung, Arbeitsspeicher oder IOPS-Wert der verwendeten Hardware arbeiten.
* Das Design von DocumentDB ermöglicht Ihnen auch die Anpassung an sehr hohe Abfragevolumen (mehr als 1 Milliarde Abfragen pro Tag).

Diese Attribute sind vorteilhaft im Hinblick auf Web-, Mobil-, Gaming- und IoT-Anwendungen, die niedrige Antwortzeiten benötigen und große Mengen von Lese- und Schreibvorgängen bewältigen müssen. 

## <a name="iot-and-telematics"></a>IoT und Telematik
IoT-Anwendungsfälle weisen einige gemeinsame Muster hinsichtlich der Erfassung, Verarbeitung und Speicherung von Daten auf.  Erstens müssen diese Systeme große Mengen von Daten aus Gerätesensoren verschiedenster Gebietsschemata erfassen. Zum Zweiten verarbeiten und analysieren diese Systeme Daten, um in Echtzeit Erkenntnisse aus diesen Daten zu gewinnen. Die Daten werden dann für die Batch-Analyse im Cold Storage archiviert. Microsoft Azure bietet umfassende Dienste, die für IoT-Anwendungsfälle eingesetzt werden können. Zu diesen Diensten gehören Azure Event Hubs, Azure DocumentDB, Azure Stream Analytics, Azure Notification Hub, Azure Machine Learning, Azure HDInsight und PowerBI. 

![IoT-Referenzarchitektur für Azure DocumentDB](./media/documentdb-use-cases/documentdb-iot.png)

Azure Event Hubs bietet einen hohen Durchsatz bei geringer Latenz und kann daher große Datenmengen erfassen und verarbeiten. Erfasste Daten, die für Erkenntnisse in Echtzeit verarbeitet werden müssen, können zur Echtzeitanalyse an Azure Stream Analytics weitergeleitet werden. Daten können für Ad-hoc-Abfragen in DocumentDB geladen werden. Sobald die Daten in DocumentDB geladen sind, können sie abgefragt werden.  Die Daten in DocumentDB können im Rahmen von Echtzeitanalysen als Referenzdaten verwendet werden. Darüber hinaus können Daten durch Verknüpfen von DocumentDB-Daten mit HDInsight für Pig-, Hive- oder Map/Reduce-Aufträge weiter optimiert und verarbeitet werden.  Die optimierten Daten werden anschließend zur Berichterstellung wieder in DocumentDB geladen.   

Eine Beispiellösung für das Internet der Dinge unter Verwendung von DocumentDB, EventHubs und Storm finden Sie im [Beispielrepository "hdinsight-storm" in GitHub](https://github.com/hdinsight/hdinsight-storm-examples/).

Weitere Informationen zu den Azure-Angeboten für das Internet der Dinge finden Sie unter [Ihr eigenes Internet der Dinge](http://www.microsoft.com/en-us/server-cloud/internet-of-things.aspx). 

## <a name="retail-and-marketing"></a>Einzelhandel und Marketing
DocumentDB wird im Einzelhandel häufig zum Speichern von Katalogdaten verwendet. Zu den Anwendungsfällen für Katalogdaten gehören das Speichern und Abfragen eines Satzes von Attributen für Entitäten wie beispielsweise Personen, Orte und Produkte.  Einige Beispiele für Katalogdaten sind Benutzerkonten, Produktkataloge, Geräteregistrierungen für das Internet der Dinge und Stücklistensysteme.  Attribute für diese Daten können variieren und sich im Laufe der Zeit ändern, um geänderte Anwendungsanforderungen zu erfüllen.  

Betrachten Sie als Beispiel einen Produktkatalog für einen Automobilzulieferer. Jedes Teil verfügt zusätzlich zu den Attributen, die allen Teilen gemeinsam sind, über eigene Attribute.  Darüber hinaus können sich Attribute für ein bestimmtes Teil ändern, wenn ein neues Modell auf den Markt kommt.  Als JSON-Dokumentspeicher unterstützt DocumentDB flexible Schemata sowie hierarchische Daten und eignet sich daher gut zum Speichern von Produktkatalogdaten.

![Einzelhandelskatalog-Referenzarchitektur für Azure DocumentDB](./media/documentdb-use-cases/documentdb-retail-catalog.png)

 Darüber hinaus können Daten in DocumentDB für Big Data-Analysen über Pig, Hive oder Map/Reduce-Aufträge in HDInsight integriert werden. Ausführliche Informationen zum Hadoop-Connector für DocumentDB finden Sie unter [Ausführen ein Hadoop-Auftrags mit DocumentDB und HDInsight](documentdb-run-hadoop-with-hdinsight.md).

 ![Einzelhandelsbestellungs-Referenzarchitektur für Azure DocumentDB](./media/documentdb-use-cases/documentdb-retail-orders.png)
## <a name="gaming"></a>Spiele
Die Datenbankebene ist eine wesentliche Komponente von Gaming-Anwendungen. Moderne Spiele führen Grafikberechnungen auf Clients (mobile Endgeräte/Konsolen) durch, verlassen sich aber auf die Cloud, die benutzerdefinierten und personalisierten Inhalte wie In-Game Statistiken, Integration von sozialen Medien sowie Highscore-Listen zur Verfügung stellt. Spiele erfordern häufig Wartezeiten von einzelnen Millisekunden für Lese- und Schreibvorgänge, um ein ansprechendes Spielerlebnis bereitzustellen. Eine Spieldatenbank muss schnell sein und enorme Spitzen bei Anforderungsraten während der Einführung neuer Spiele und Featureupdates verarbeiten können.

DocumentDB wird von Spielen wie z.B. [The Walking Dead: No Man's Land](https://azure.microsoft.com//blog/the-walking-dead-no-mans-land-game-soars-to-1-with-azure-documentdb/) von [Next Games](http://www.nextgames.com/) und [Halo 5: Guardians](https://azure.microsoft.com/blog/how-halo-5-guardians-implemented-social-gameplay-using-azure-documentdb/) verwendet. DocumentDB bietet Spielentwicklern die folgenden Vorteile:

* DocumentDB ermöglicht eine flexible zentrale Hoch- oder Herunterskalierung der Leistung. So können Spiele die Profile und Statistiken von gleichzeitig aktiven Spielern mit nur einem API-Aufruf aktualisieren – ganz egal, ob es sich um mehrere Dutzend oder mehrere Millionen Spieler handelt.
* DocumentDB unterstützt Lese- und Schreibvorgänge in Millisekunden, um jede Verzögerung während des Spiels zu vermeiden.
* Die automatische Indizierung von DocumentDB ermöglicht die Filterung mehrerer verschiedener Eigenschaften in Echtzeit, also z.B. Spieler durch ihre internen Player, GameCenter-, Facebook- und Google-IDs zu lokalisieren oder sie auf Grundlage der Mitgliedschaft der Spielcharaktere in einer Gilde abzufragen. Dies ist möglich, ohne eine komplexe Indizierung oder Sharding-Infrastruktur erstellen zu müssen.
* Soziale Funktionen, einschließlich Chat-Nachrichten im Spiel, Mitgliedschaften in Gilden, bewältigte Aufgaben, Highscore-Listen und soziale Graphen, sind einfacher mit einem flexiblen Schema zu implementieren.
* DocumentDB benötigt als verwaltete Platform-as-a-Service (PaaS) minimalen Einrichtungs- und Verwaltungsaufwand, um schnelle Iteration zu ermöglichen und die Markteinführungszeit zu verkürzen.

![Gaming-Referenzarchitektur für Azure DocumentDB](./media/documentdb-use-cases/documentdb-gaming-architecture.png)

## <a name="web-and-mobile-applications"></a>Webanwendungen und mobile Anwendungen
DocumentDB wird häufig in Web- und mobilen Anwendungen verwendet und eignet sich besonders gut für die Modellierung sozialer Interaktionen, Integration in Drittanbieterdienste sowie zum Erstellen umfangreicher Personalisierungen. Mithilfe der DocumentDB-SDKs können umfangreiche iOS- und Android-Anwendungen mit dem beliebten [Xamarin-Framework](documentdb-mobile-apps-with-xamarin.md) erstellt werden.  

### <a name="social-applications"></a>Soziale Anwendungen
Ein häufiger Anwendungsfall für DocumentDB ist die Speicherung und Abfrage von benutzergenerierten Inhalten für Web- und mobile Anwendungen, insbesondere Anwendungen für soziale Medien. Einige Beispiele für benutzergenerierte Inhalte sind Chatsitzungen, Tweets, Blogbeiträge, Bewertungen und Kommentare. Benutzergenerierte Inhalte in Anwendungen für soziale Medien sind eine Mischung aus Freitext, Eigenschaften, Tags und Beziehungen, die nicht durch eine starre Struktur begrenzt sind. Inhalte wie Chats, Kommentare und Beiträge können in DocumentDB gespeichert werden, ohne dass hierzu Transformationen oder komplexe ORM-Ebenen (Object Relational Mapping) erforderlich sind.  Dateneigenschaften können beim Durchlaufen des Anwendungscodes ganz einfach hinzugefügt oder geändert werden, um die jeweiligen Anforderungen zu erfüllen. Auf diese Weise wird die Entwicklung erheblich beschleunigt.  

Anwendungen, die in soziale Drittanbieter-Netzwerke integriert werden, müssen auf Schemaänderungen dieser Netzwerke reagieren. Da Daten in DocumentDB standardmäßig und automatisch indiziert werden, können Daten jederzeit abgefragt werden. Daher verfügen diese Anwendungen über die Flexibilität, Projektionen entsprechend ihren jeweiligen Anforderungen abzurufen.

Viele soziale Anwendungen werden auf globaler Ebene ausgeführt und können nicht vorhersagbare Nutzungsmuster aufweisen. Flexibilität bei der Skalierung des Datenspeichers ist von grundlegender Bedeutung, da die Anwendungsschicht in Abhängigkeit von der Nutzung skaliert werden muss.  Sie können den Speicher horizontal hochskalieren, indem Sie einem DocumentDB-Konto weitere Datenpartitionen hinzufügen.  Darüber hinaus können Sie auch weitere DocumentDB-Konten über mehrere Regionen hinweg erstellen. Informationen zur regionalen Verfügbarkeit von DocumentDB-Diensten finden Sie unter [Azure-Region](https://azure.microsoft.com/regions/#services).

![Web-App-Referenzarchitektur für Azure DocumentDB](./media/documentdb-use-cases/documentdb-web.png)

### <a name="personalization"></a>Personalisierung
Heute sind die modernen Anwendungen mit komplexen Ansichten und Funktionalitäten verbunden. Diese sind üblicherweise dynamisch und auf die Einstellungen bzw. die Stimmung des Benutzers oder auf Brandinganforderungen ausgerichtet. Daher müssen Anwendungen personalisierte Einstellungen effektiv abrufen können, um Benutzeroberflächenelemente und Funktionalitäten schnell darzustellen. 

JSON ist ein effektives Format zur Darstellung von Daten für das Benutzeroberflächenlayout. Es ist nicht nur unkompliziert, sondern kann auch problemlos von JavaScript interpretiert werden. DocumentDB bietet optimierbare Konsistenzebenen, die schnelle Lesevorgänge sowie Schreibvorgänge mit geringer Latenz ermöglichen. Daher ist die Speicherung von Daten für das Benutzeroberflächenlayout einschließlich personalisierter Einstellungen als JSON-Dokument in DocumentDB eine effektive Möglichkeit zur Übertragung dieser Daten.

## <a name="next-steps"></a>Nächste Schritte
Um mit DocumentDB zu beginnen, können Sie ein [Konto](https://azure.microsoft.com/pricing/free-trial/) erstellen und dann unserem [Lernpfad](https://azure.microsoft.com/documentation/learning-paths/documentdb/) folgen, um die Informationen zu DocumentDB zu erhalten, die Sie benötigen. 

Wenn Sie mehr zu Kunden erfahren möchten, die DocumentDB verwenden, stehen Ihnen folgende Kundenberichte zur Verfügung:

* [Sulekha.com](https://customers.microsoft.com/en-US/story/sulekha-uses-azure-documentdb-to-connect-customers-and-businesses-across-india). Sulekha verwendet Azure DocumentDB, um Kunden und Unternehmen in ganz Indien zu vernetzen.
* [NewOrbit](https://customers.microsoft.com/en-US/story/neworbit-takes-flight-with-azure-documentdb). NewOrbit ist mit Azure DocumentDB erfolgreich.
* [Affinio](https://customers.microsoft.com/en-US/doclink/affinio-switches-from-aws-to-azure-documentdb-to-harness-social-data-at-scale). Affinio wechselt von AWS zu Azure DocumentDB, um soziale Daten in großem Umfang zu nutzen.
* [Next Games](https://azure.microsoft.com//blog/the-walking-dead-no-mans-land-game-soars-to-1-with-azure-documentdb/). Das Spiel „The Walking Dead: No Man‘s Land“ steigt durch Unterstützung von Azure DocumentDB auf Platz 1.
* [Halo](https://azure.microsoft.com/blog/how-halo-5-guardians-implemented-social-gameplay-using-azure-documentdb/). So hat „Halo 5“ mit Azure DocumentDB Social Gaming implementiert.
* [Cortana Analytics-Katalog](https://azure.microsoft.com/blog/cortana-analytics-gallery-a-scalable-community-site-built-on-azure-documentdb/). Cortana Analytics-Katalog – eine skalierbare Community-Website in Azure DocumentDB.
* [Breeze](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18602). Führender Integrator bietet multinationalen Unternehmen innerhalb weniger Minuten globale Einblicke dank flexibler Cloudtechnologien.
* [News Republic](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18639). Mehr Personalisierung und Interaktivität in den Nachrichten, um engagierten Bürgern zielgerichtete Informationen zu bieten. 
* [SGS International](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18653). Große Marken verlassen sich auf SGS, um eine konsistente Farbdarstellung auf der ganzen Welt zu erzielen. Und SGS verlässt sich auf Azure.
* [Telenor](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18608). Telenor, einer der weltweit größten Mobilfunkanbieter, nutzt die Cloud, um so schnell reagieren zu können wie ein Startupunternehmen. 
* [XOMNI](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18667). Der Speicher der Zukunft basiert auf schnellen Suchvorgängen und problemlosem Datenfluss.
* [Nucleo](https://customers.microsoft.com/story/azure-based-software-platform-breaks-down-barriers-bet). Azure-basierte Softwareplattform reißt Barrieren zwischen Unternehmen und Kunden ein.
* [Weka](https://customers.microsoft.com/story/weka-smart-fridge-improves-vaccine-management-so-more-people-can-be-protected-against-diseases). Der Smart-Fridge von Weka verbessert die Verwaltung von Impfstoffen, damit noch mehr Menschen vor Krankheiten geschützt werden können.
* [Orange Tribes](https://customers.microsoft.com/en-US/story/theres-more-to-that-food-app-than-meets-the-eye-or-the-mouth). Eine Lebensmittel-App, nicht nur für Auge und Mund.
* [Real Madrid](https://customers.microsoft.com/en-US/story/real-madrid-brings-the-stadium-closer-to-450-million-f). Real Madrid bringt den weltweit 450 Millionen Fans mit der Microsoft Cloud ihr Fußballstadion näher.
* [Tuku](https://customers.microsoft.com/en-US/story/tuku-makes-car-buying-fun-with-help-from-azure-services). TUKU macht den Autokauf mithilfe der Azure-Dienste zum Vergnügen. 

