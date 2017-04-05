---
title: "Hadoop-Komponenten und -Versionen – Azure HDInsight | Microsoft-Dokumente"
description: "Erfahren Sie mehr zu den Hadoop-Komponenten und -Versionen in HDInsight und die in dieser Cloud-Distribution von HortonWorks Data Platform verfügbaren Servicelevels."
services: hdinsight
editor: cgronlun
manager: asadk
author: bprakash
tags: azure-portal
documentationcenter: 
ms.assetid: 367b3f4a-f7d3-4e59-abd0-5dc59576f1ff
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2017
ms.author: bprakash
translationtype: Human Translation
ms.sourcegitcommit: 6e0ad6b5bec11c5197dd7bded64168a1b8cc2fdd
ms.openlocfilehash: c753eac8caed139250a0db85b639fca57c1e9ea6
ms.lasthandoff: 03/28/2017


---
# <a name="what-are-the-different-hadoop-components-and-versions-available-with-hdinsight"></a>Was sind die verschiedenen Hadoop-Komponenten und -Versionen, die in HDInsight verfügbar sind?

Erfahren Sie mehr zu den für Azure HDInsight angebotenen Servicelevels sowie zu den Komponenten des Hadoop-Ökosystems und den enthaltenen Versionen. Jede HDInsight-Version ist eine Cloud-Distribution von HortonWorks Data Platform (HDP).

## <a name="hdinsight-standard-and-hdinsight-premium"></a>HDInsight Standard und HDInsight Premium

Azure HDInsight bietet die Cloudlösungen für Big Data in zwei Kategorien an: **Standard** und **Premium**. Die Tabelle im folgenden Abschnitt listet die Funktionen auf, die **nur** als Teil der Kategorie „Premium“ verfügbar sind. Funktionen, die nicht explizit in dieser Tabelle genannt werden, sind Teil der Kategorie „Standard“.

> [!NOTE]
> Das HDInsight Premium-Angebot befindet sich derzeit in der Vorschauphase und ist nur für Linux-Cluster verfügbar.
>
>

| HDInsight Premium-Funktion | Beschreibung |
| --- | --- |
| In die Domäne eingebundene HDInsight-Cluster |Binden Sie HDInsight-Cluster in AAD-Domänen (Azure Active Directory) ein, um eine unternehmensweite Sicherheit zu erzielen. Sie können jetzt eine Liste mit Mitarbeitern Ihres Unternehmens konfigurieren, die eine Authentifizierung per Azure Active Directory für die Anmeldung am HDInsight-Cluster durchführen können. Der Unternehmensadministrator kann auch mithilfe von [Apache Ranger](http://hortonworks.com/apache/ranger/) die rollenbasierte Zugriffskontrolle für Hive-Sicherheit konfigurieren und so den Datenzugriff optimal beschränken. Darüber hinaus kann der Administrator den mitarbeiterspezifischen Datenzugriff sowie jegliche Änderung der Zugriffssteuerungsrichtlinien überwachen und so ein hohes Maß an Kontrolle über Unternehmensressourcen erreichen. Weitere Informationen finden Sie unter [Configure domain-joined HDInsight clusters](hdinsight-domain-joined-configure.md) (Konfigurieren von in die Domäne eingebundenen HDInsight-Clustern). |

### <a name="cluster-types-supported-for-hdinsight-premium"></a>Für HDInsight Premium unterstützte Clustertypen
Die folgende Tabelle listet den HDInsight-Clustertyp und die Supportmatrix für Premium auf.

| Clustertyp | Standard | Premium |
| --- | --- | --- |
| Hadoop |Ja |Ja (nur HDInsight 3.5) |
| Spark |Ja |Nein |
| HBase |Ja |Nein |
| Storm |Ja |Nein |
| R Server  |Ja |Nein |
| Interactive Hive (Vorschau) |Ja |Nein |
| Kafka (Vorschau)|Ja|Nein| 

Diese Tabelle wird aktualisiert, sobald weitere Clustertypen in HDInsight Premium hinzugefügt werden.

### <a name="features-not-supported-for-hdinsight-premium"></a>Die Funktionen werden nicht für HDInsight Premium unterstützt

Die folgenden Funktionen werden derzeit nicht für HDInsight Premium-Cluster unterstützt:

* **Keine Unterstützung für Azure Data Lake Store als primären Speicher**. Sie können weiterhin Azure Data Lake Store als Add-On-Speicher mit HDInsight Premium-Clustern verwenden.

### <a name="pricing-and-sla"></a>Preise und SLA
Informationen zu Preisen und SLA für HDInsight Premium finden Sie unter [HDInsight-Preise](https://azure.microsoft.com/pricing/details/hdinsight/).

## <a name="hadoop-components-available-with-different-hdinsight-versions"></a>Verfügbare Hadoop-Komponenten in verschiedenen Versionen von HDInsight
Azure HDInsight unterstützt mehrere Hadoop-Clusterversionen, die jederzeit bereitgestellt werden können. Jede ausgewählte Version erstellt eine bestimmte Version der HDP-Distribution (Hortonworks Data Platform) und eine Reihe von Komponenten innerhalb dieser Distribution. Die den HDInsight-Clusterversionen zugeordneten Komponentenversionen sind in der folgenden Tabelle aufgeführt. Als Standardclusterversion wird in Azure HDInsight derzeit Version 3.5 auf Basis von HDP 2.5 (ab 17.02.2017) verwendet.

> [!NOTE]
> Die Standardversion aus dem Dienst kann ohne vorherige Ankündigung geändert werden. Es wird empfohlen, dass Sie die Version angeben, wenn Sie Cluster mit .NET SDK/Azure PowerShell und Azure-Befehlszeilenschnittstelle erstellen, wenn eine Versionsabhängigkeit vorliegt.
>
>


| Komponente | HDInsight, Version 3.5 (Standard) | HDInsight-Version 3.4 | HDInsight-Version 3.3 | HDInsight-Version 3.2 | HDInsight-Version 3.1 | HDInsight Version 3,0 |
| --- | --- | --- | --- | --- | --- | --- |
| Hortonworks Data Platform |2.5 |2.4 |2.3 |2.2 |2.1.7 |2.0 |
| Apache Hadoop und YARN |2.7.3 |2.7.1 |2.7.1 |2.6.0 |2.4.0 |2.2.0 |
| Apache Tez |0.7.0 |0.7.0 |0.7.0 |0.5.2 |0.4.0 |-|
| Apache Pig |0.16.0 |0.15.0 |0.15.0 |0.14.0 |0.12.1 |0.12.0 |
| Apache Hive und HCatalog |1.2.1.2.5 |1.2.1 |1.2.1 |0.14.0 |0.13.1 |0.12.0 |
| Apache HBase |1.1.2 |1.1.2 |1.1.1 |0.98.4 |0.98.0 |-|
| Apache Sqoop |1.4.6 |1.4.6 |1.4.6 |1.4.5 |1.4.4 |1.4.4 |
| Apache Oozie |4.2.0 |4.2.0 |4.2.0 |4.1.0 |4.0.0 |4.0.0 |
| Apache Zookeeper |3.4.6 |3.4.6 |3.4.6 |3.4.6 |3.4.5 |3.4.5 |
| Apache Storm |1.0.1 |0.10.0 |0.10.0 |0.9.3 |0.9.1 |-|
| Apache Mahout |0.9.0+ |0.9.0+ |0.9.0+ |0.9.0 |0.9.0 |-|
| Apache Phoenix |4.7.0 |4.4.0 |4.4.0 |4.2.0 |4.0.0.2.1.7.0-2162 |-|
| Apache Spark |1.6.2 und 2.0 (nur Linux) |1.6.0 (nur Linux) |1.5.2 (nur für Linux/experimenteller Build) |1.3.1 (nur für Windows) |-|-|
| Apache Kafka | 0.10.0 | 0.9.0 |-|-|-|-|
| Mono |4.2.1 |3.2.8 |-|-|-|-|

**Abrufen von Informationen zur aktuellen Komponentenversion**

Die mit den HDInsight-Clusterversionen verknüpften Komponentenversionen können sich bei künftigen Updates für HDInsight ändern. Eine Möglichkeit, die verfügbaren Komponenten zu bestimmen und zu prüfen, welche Versionen für einen Cluster verwendet werden, ist die Ambari REST-API. Über den **GetComponentInformation** -Befehl können Informationen zu einer Dienstkomponente abgerufen werden. Ausführliche Informationen finden Sie in der [Ambari-Dokumentation][ambari-docs]. Sie können sich auch per Remotedesktop bei einem Cluster anmelden und den Inhalt des Verzeichnisses „C:\apps\dist\"“ direkt prüfen, um diese Informationen abzurufen.

**Versionshinweise**

In den [HDInsight-Versionshinweisen](hdinsight-release-notes.md) finden Sie zusätzliche Anmerkungen zu den aktuellen Versionen von HDInsight.

## <a name="supported-hdinsight-versions"></a>Unterstützte HDInsight-Versionen
In der folgenden Tabelle werden die derzeit verfügbaren Versionen von HDInsight, die entsprechenden verwendeten Versionen von Hortonworks Data Platform und die zugehörigen Relesasedaten aufgeführt. Sofern bekannt sind auch die jeweiligen Termine angegeben, ab denen der Support abläuft und sie als veraltet gelten. Beachten Sie Folgendes:

* Hochverfügbare Cluster mit zwei Hauptknoten werden standardmäßig für HDInsight 2.1 und höher bereitgestellt. Für HDInsight 1.6-Cluster sind sie nicht verfügbar.
* Nachdem der Support für eine bestimmte Version abgelaufen ist, steht sie möglicherweise nicht mehr im Azure-Portal zur Verfügung. Die folgende Tabelle zeigt, welche Versionen im klassischen Azure-Portal verfügbar sind. Clusterversionen stehen bis zum Zeitpunkt ihrer „Veraltung“ weiterhin über den Parameter `Version` im Windows PowerShell-Befehl [New-AzureRmHDInsightCluster](https://msdn.microsoft.com/library/mt619331.aspx) und das .NET SDK zur Verfügung.

| HDInsight-Version | HDP-Version | Betriebssystem des virtuellen Computers | Hohe Verfügbarkeit | Herausgabedatum | Im Azure-Portal verfügbar | Supportablaufdatum | Veraltet ab |
| --- | --- | --- | --- | --- | --- | --- | --- |
| HDI 3.6 (Vorschau) |HDP 2.6 |Ubuntu 16 |Ja |02/28/2017 |Ja | | |
| HDI 3.5 |HDP 2.5 |Ubuntu 16 |Ja |9/30/2016 |Ja | | |
| HDI 3.4 |HDP 2.4 |Ubuntu 14.0.4 LTS |Ja |03/29/2016 |Ja |12/29/2016 |1/9/2018 |
| HDI 3.3 |HDP 2.3 |Ubuntu 14.0.4 LTS oder Windows Server 2012 R2 |Ja |02.12.2015 |Ja |27.06.2016 |31.07.2017 |
| HDI 3.2 |HDP 2.2 |Ubuntu 12.04 LTS oder Windows Server 2012 R2 |Ja |18.02.2015 |Ja |01.03.2016 |01.04.2017 |
| HDI 3,1 |HDP 2,1 |Windows Server 2012 R2 |Ja |24.06.2014 |Nein |18.05.2015 |30.06.2016 |
| HDI 3,0 |HDP 2,0 |Windows Server 2012 R2 |Ja |11.02.2014 |Nein |17.09.2014 |30.06.2015 |
| HDI 2,1 |HDP 1,3 |Windows Server 2012 R2 |Ja |28.10.2013 |Nein |12.05.2014 |31.05.2015 |
| HDI 1.6 |HDP 1.1 | |Nein |28.10.2013 |Nein |26.04.2014 |31.05.2015 |

##<a name="hdi-36-preview-with-apache-spark-21"></a>HDI 3.6 (Vorschau) mit Apache Spark 2.1
Die Vorschau von HDI 3.6 wird am 28.2.2017 veröffentlicht. Dies ist in der Vorschau für Apache Spark 2.1 verfügbar. Weitere Informationen finden Sie unter [HDI 3.6 (Vorschau) mit Apache Spark 2.1](https://azure.microsoft.com/en-us/blog/announcing-preview-of-azure-hdinsight-3-6-with-apache-spark-2-1/).

##<a name="hdi-version-32-and-33-nearing-deprecation-date"></a>Die HDI-Versionen 3.2 und 3.3 sind bald veraltet.
Der Support für den HDI 3.2-Cluster ist am 1.3.2016 abgelaufen und wird am 1.4.2017 eingestellt. Der Support für den HDI 3.3-Cluster ist am 27.6.2016 abgelaufen und wird am 31.7.2017 eingestellt. Wenn Sie einen HDI 3.2- oder HDI 3.3-Cluster besitzen, aktualisieren Sie Ihren Cluster demnächst auf HDI 3.5 (neueste Version).

### <a name="the-service-level-agreement-for-hdinsight-cluster-versions"></a>Die Servicelevel-Vereinbarung (SLA) für HDInsight-Clusterversionen
Die SLA ist in Form eines "Supportfensters" definiert. Ein Supportfenster bezieht sich auf den Zeitraum, für den eine HDInsight-Clusterversion vom Microsoft-Kundendienst und -Support unterstützt wird. Ein HDInsight-Cluster gilt als außerhalb des Supportfensters, wenn das **Supportablaufdatum** am aktuellen Datum bereits verstrichen ist. In der Tabelle oben finden Sie eine Liste der unterstützten HDInsight-Clusterversionen. Das Supportablaufdatum für eine bestimmte HDInsight-Version X (sobald eine neuere X+1-Version verfügbar ist) wird berechnet als das spätere Datum von:  

* Formel 1: Addieren Sie 180 Tage zum Releasedatum von HDInsight-Clusterversion X.
* Formel 2: Addieren Sie 90 Tage zu dem Datum, an dem HDInsight-Clusterversion X+1 (die Folgeversion von X) im Portal verfügbar gemacht wurde.

Das Datum **Veraltet ab** ist das Datum, nach dem eine Clusterversion nicht mehr in HDInsight erstellt werden kann. Ab dem 31. Juli. 2017 kann die Größe eines Cluster nach dem Datum „Veraltet ab“ nicht mehr geändert werden.

> [!NOTE]
> Windows-basierte HDInsight-Cluster (einschließlich Version 2.1, 3.0, 3.1, 3.2 und 3.3) laufen unter der Azure-Gastbetriebssystem-Familie 4, die die 64-Bit-Version von Windows Server 2012 R2 verwendet und .NET Framework 4.0, 4.5, 4.5.1 und 4.5.2 unterstützt.
>
>

## <a name="hortonworks-release-notes-associated-with-hdinsight-versions"></a>Hortonworks-Versionshinweise im Zusammenhang mit HDInsight-Versionen
* HDInsight-Clusterversion 3.4 verwendet eine Hadoop-Distribution, die auf [Hortonworks Data Platform 2.4](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.4.0/bk_HDP_RelNotes/content/ch_relnotes_v240.html)basiert. Dies ist der **standardmäßige** Hadoop-Cluster, der bei Verwendung des Portals erstellt wird.
* HDInsight-Clusterversion 3.3 verwendet eine Hadoop-Distribution, die auf [Hortonworks Data Platform 2.3](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.3.0/bk_HDP_RelNotes/content/ch_relnotes_v230.html)basiert.

  * Apache Storm-Versionsanmerkungen finden Sie [hier](https://storm.apache.org/2015/11/05/storm0100-released.html).
  * Apache Hive-Versionsanmerkungen finden Sie [hier](https://issues.apache.org/jira/secure/ReleaseNote.jspa?version=12332384&styleName=Text&projectId=12310843).
* HDInsight-Clusterversion 3.2 verwendet eine Hadoop-Distribution, die auf [Hortonworks Data Platform 2.2][hdp-2-2] basiert.  

  * Versionshinweise für bestimmte Komponenten von Apache: [Hive 0.14](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310843&version=12326450), [Pig 0.14](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310730&version=12326954), [HBase 0.98.4](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310753&version=12326810), [Phoenix 4.2.0](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12315120&version=12327581), [M/R 2.6](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310941&version=12327180), [HDFS 2.6](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310942&version=12327181), [YARN 2.6](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12313722&version=12327197), [Common](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310240&version=12327179), [Tez 0.5.2](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12314426&version=12328742), [Ambari 2.0](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12312020&version=12327486), [Storm 0.9.3](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12314820&version=12327112), [Oozie 4.1.0](https://issues.apache.org/jira/secure/ReleaseNote.jspa?version=12324960&projectId=12311620).
* HDInsight-Clusterversion 3.1 verwendet eine Hadoop-Distribution, die auf [Hortonworks Data Platform 2.1.7][hdp-2-1-7] basiert. Vor dem 7.11.2014 erstellte HDInsight 3.1-Cluster basierten auf [Hortonworks Data Platform 2.1.1][hdp-2-1-1].
* HDInsight-Clusterversion 3.0 verwendet eine Hadoop-Distribution, die auf [Hortonworks Data Platform 2.0][hdp-2-0-8] basiert.
* HDInsight-Clusterversion 2.1 verwendet eine Hadoop-Distribution, die auf [Hortonworks Data Platform 1.3][hdp-1-3-0] basiert.
* HDInsight-Clusterversion 1.6 verwendet eine Hadoop-Distribution, die auf [Hortonworks Data Platform 1.1][hdp-1-1-0] basiert.

[image-hdi-versioning-versionscreen]: ./media/hdinsight-component-versioning/hdi-versioning-version-screen.png

[wa-forums]: http://azure.microsoft.com/support/forums/

[connect-excel-with-hive-ODBC]: hdinsight-connect-excel-hive-ODBC-driver.md

[hdp-2-2]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.2.0/HDP_2.2.0_Release_Notes_20141202_version/index.html

[hdp-2-1-7]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.7-Win/bk_releasenotes_HDP-Win/content/ch_relnotes-HDP-2.1.7.html

[hdp-2-1-1]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.1/bk_releasenotes_hdp_2.1/content/ch_relnotes-hdp-2.1.1.html

[hdp-2-0-8]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.8.0/bk_releasenotes_hdp_2.0/content/ch_relnotes-hdp2.0.8.0.html

[hdp-1-3-0]: http://docs.hortonworks.com/HDPDocuments/HDP1/HDP-1.3.0/bk_releasenotes_hdp_1.x/content/ch_relnotes-hdp1.3.0_1.html

[hdp-1-1-0]: http://docs.hortonworks.com/HDPDocuments/HDP1/HDP-Win-1.1/bk_releasenotes_HDP-Win/content/ch_relnotes-hdp-win-1.1.0_1.html

[ambari-docs]: https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md

[zookeeper]: http://zookeeper.apache.org/

