---
title: Installieren und Verwenden von Giraph auf Linux-basiertem HDInsight (Hadoop) | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie Giraph auf Linux-basierten HDInsight-Clustern mit Skriptaktionen installieren. Mit Skriptaktionen können Sie den Cluster während der Erstellung anpassen, indem Sie die Clusterkonfiguration ändern oder Dienste und Hilfsprogramme installieren."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 9fcac906-8f06-4002-9fe8-473e42f8fd0f
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/08/2017
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: 4f2230ea0cc5b3e258a1a26a39e99433b04ffe18
ms.openlocfilehash: ff27749800319517a8f635530f0f16b928692575
ms.lasthandoff: 03/25/2017


---
# <a name="install-giraph-on-hdinsight-hadoop-clusters-and-use-giraph-to-process-large-scale-graphs"></a>Installieren von Giraph in HDInsight Hadoop-Clustern und Verwenden von Giraph zur Verarbeitung großer Diagramme

Sie können Giraph in einem beliebigen Clustertyp in Hadoop in Azure HDInsight mithilfe der **Skriptaktion** zum Anpassen eines Clusters installieren.

In diesem Thema wird beschrieben, wie Sie Giraph mithilfe der Funktion „Skriptaktion“ installieren. Nach der Installation von Giraph erfahren Sie auch, wie Sie Giraph bei den typischsten Anwendungen verwenden können, d. h. bei der Verarbeitung großer Graphen.

> [!IMPORTANT]
> Die Schritte in diesem Dokument erfordern einen HDInsight-Cluster mit Linux. Linux ist das einzige Betriebssystem, das unter HDInsight Version 3.4 oder höher verwendet wird. Weitere Informationen finden Sie unter [Ende des Lebenszyklus von HDInsight unter Windows](hdinsight-component-versioning.md#hdi-version-32-and-33-nearing-deprecation-date).

## <a name="whatis"></a>Was ist Giraph?

[Apache Giraph](http://giraph.apache.org/) ermöglicht die Graphverarbeitung mit Hadoop und lässt sich mit Azure HDInsight nutzen. Graphen bilden Beziehungen zwischen Objekten ab, wie z. B. Verbindungen zwischen Routern in einem großen Netzwerk wie dem Internet oder Beziehungen zwischen Menschen in sozialen Netzwerken (mitunter als "Social Graph" bezeichnet). Mit der Graphverarbeitung können Sie sich Gedanken über die Beziehungen zwischen den Objekten im Diagramm machen wie etwa:

* Ermitteln potenzieller Freunde aufgrund Ihrer aktuellen Beziehungen.

* Ermitteln der kürzesten Route zwischen zwei Computern in einem Netzwerk.

* Berechnen des Seitenrangs von Webseiten.

> [!WARNING]
> Komponenten, die mit dem HDInsight-Cluster bereitgestellt werden, werden vollständig unterstützt, und Microsoft Support hilft Ihnen, Probleme im Zusammenhang mit diesen Komponenten zu isolieren und zu beheben.
> 
> Für benutzerdefinierte Komponenten wie Giraph steht in wirtschaftlich angemessenem Rahmen Support für eine weiterführende Behebung des Problems zur Verfügung. Auf diese Weise kann das Problem behoben werden, ODER Sie werden aufgefordert, verfügbare Kanäle für Open-Source-Technologien in Anspruch zu nehmen, die über umfassende Kenntnisse für diese Technologien verfügen. So können z. B. viele Communitywebsites verwendet werden, wie: das [MSDN-Forum für HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Für Apache-Projekte gibt es auch Projektwebsites auf [http://apache.org](http://apache.org), [z. B. Hadoop](http://hadoop.apache.org/).


## <a name="what-the-script-does"></a>Funktion des Skripts

Dieses Skript führt folgende Aktionen aus:

* Installiert Giraph in `/usr/hdp/current/giraph`

* Kopiert die `giraph-examples.jar`-Datei in den Standardspeicher (WASB) für den Cluster: `/example/jars/giraph-examples.jar`

## <a name="install"></a>Installation von Giraph mithilfe von Skriptaktionen

Ein Beispielskript zum Installieren von Giraph in einem HDInsight-Cluster ist an folgendem Ort verfügbar:

    https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh

Dieser Abschnitt enthält Anweisungen zur Verwendung des Beispielskripts während der Erstellung des Clusters mithilfe des Azure-Portals. 

> [!NOTE]
> Zum Anwenden von Skriptaktionen können auch Azure PowerShell, die Azure-Befehlszeilenschnittstelle, das HDInsight .NET SDK oder Azure Resource Manager-Vorlagen verwendet werden. Sie können auch Skriptaktionen auf bereits ausgeführte Cluster anwenden. Weitere Informationen finden Sie unter [Anpassen von HDInsight-Clustern mithilfe von Skriptaktionen](hdinsight-hadoop-customize-cluster-linux.md).

1. Beginnen Sie die Erstellung eines Clusters anhand der Schritte in [Erstellen Linux-basierter HDInsight-Cluster](hdinsight-hadoop-create-linux-clusters-portal.md), schließen Sie sie jedoch nicht ab.

2. Wählen Sie auf dem Blatt **Optionale Konfiguration** die Option **Skriptaktionen**, und geben Sie die folgenden Informationen an:
   
   * **NAME**: Geben Sie einen Anzeigenamen für die Skriptaktion ein.

   * **SCRIPT URI**: https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh

   * **HEAD**: Aktivieren Sie diese Option.

   * **WORKER**: Lassen Sie dieses Kontrollkästchen deaktiviert.

   * **ZOOKEEPER**: Lassen Sie dieses Kontrollkästchen deaktiviert.

   * **PARAMETERS**: Lassen Sie dieses Feld leer.

3. Verwenden Sie am unteren Rand der **Skriptaktionen** die Schaltfläche **Auswählen**, um die Konfiguration zu speichern. Verwenden Sie schließlich die Schaltfläche **Auswählen** am unteren Rand des Blatts **Optionale Konfiguration**, um die optionalen Konfigurationsinformationen zu speichern.

4. Setzen Sie die Erstellung des Clusters wie unter [Erstellen Linux-basierter HDInsight-Cluster](hdinsight-hadoop-create-linux-clusters-portal.md)beschrieben fort.

## <a name="usegiraph"></a>Wie verwende ich Giraph in HDInsight?

Sobald die Clustererstellung abgeschlossen ist, gehen Sie folgendermaßen vor, um das in Giraph enthaltene Beispiel "SimpleShortestPathsComputation" auszuführen.  Dieses verwendet die grundlegende [Pregel](http://people.apache.org/~edwardyoon/documents/pregel.pdf)-Implementierung zum Ermitteln des kürzesten Pfads zwischen Objekten in einem Graph.

1. Stellen Sie mithilfe von SSH eine Verbindung mit dem HDInsight-Cluster her:
   
        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
   
    Informationen hierzu finden Sie unter [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Linux, Unix oder OS X](hdinsight-hadoop-linux-use-ssh-unix.md).

2. Verwenden Sie den folgenden Befehl, um eine neue Datei namens **tiny_graph.txt** zu erstellen:
   
    ```
    nano tiny_graph.txt
    ```
   
    Fügen Sie Folgendes als Inhalt der Datei hinzu:
   
    ```
    [0,0,[[1,1],[3,3]]]
    [1,0,[[0,1],[2,2],[3,1]]]
    [2,0,[[1,2],[4,4]]]
    [3,0,[[0,3],[1,1],[4,4]]]
    [4,0,[[3,4],[2,4]]]
    ```
   
    Diese Daten beschreiben die Beziehung zwischen Objekten in einem gerichteten Graphen unter Verwendung des Formats `[source_id, source_value,[[dest_id], [edge_value],...]]`. Jede Zeile repräsentiert eine Beziehung zwischen einem `source_id`-Objekt und mindestens einem `dest_id`-Objekt. Der `edge_value` (oder die Gewichtung) kann man sich als die Stärke oder Distanz der Verbindung zwischen `source_id` und `dest\_id` vorstellen.
   
    Wenn die obigen Daten auseinandergezogen und der Wert (die Gewichtung) als Abstand zwischen den Objekten verwendet werden, dann könnte das so aussehen:
   
    ![tiny_graph.txt als Kreise dargestellt mit Linien unterschiedlicher Länge dazwischen](./media/hdinsight-hadoop-giraph-install-linux/giraph-graph.png)

3. Drücken Sie zum Speichern der Datei **STRG + X**, dann **Y**, und schließlich die **Eingabetaste**, um den Dateinamen zu akzeptieren.

4. Verwenden Sie zum Speichern der Daten im primären Speicher für den HDInsight-Cluster den folgenden Befehl:
   
    ```
    hdfs dfs -put tiny_graph.txt /example/data/tiny_graph.txt
    ```

5. Führen Sie das Beispiel "SimpleShortstPathsComputation" mit dem folgenden Befehl aus.
   
    ```
    yarn jar /usr/hdp/current/giraph/giraph-examples.jar org.apache.giraph.GiraphRunner org.apache.giraph.examples.SimpleShortestPathsComputation -ca mapred.job.tracker=headnodehost:9010 -vif org.apache.giraph.io.formats.JsonLongDoubleFloatDoubleVertexInputFormat -vip /example/data/tiny_graph.txt -vof org.apache.giraph.io.formats.IdWithValueTextOutputFormat -op /example/output/shortestpaths -w 2
    ```
   
    Die mit diesem Befehl verwendeten Parameter werden in der folgenden Tabelle beschrieben.
   
   | Parameter | Funktionsbeschreibung |
   | --- | --- |
   | `jar /usr/hdp/current/giraph/giraph-examples.jar` |Die JAR-Datei, die die Beispiele enthält |
   | `org.apache.giraph.GiraphRunner` |Die Klasse, die zum Starten der Beispiele verwendet wird. |
   | `org.apache.giraph.examples.SimpleShortestPathsCoputation` |Das verwendete Beispiel. In diesem Fall wird der kürzeste Pfad zwischen ID 1 und allen anderen IDs im Graphen berechnet. |
   | `-ca mapred.job.tracker=headnodehost:9010` |Der Hauptknoten für den Cluster. |
   | `-vif org.apache.giraph.io.formats.JsonLongDoubleFloatDoubleVertexInputFromat` |Das Eingabeformat, das für die Eingabedaten verwendet wird. |
   | `-vip /example/data/tiny_graph.txt` |Die Eingabedatendatei. |
   | `-vof org.apache.giraph.io.formats.IdWithValueTextOutputFormat` |Das Ausgabeformat. In diesem Fall die ID und der Wert als Nur-Text. |
   | `-op /example/output/shortestpaths` |Der Ausgabespeicherort. |
   | `-w 2` |Die Anzahl der zu verwendenden Worker. In diesem Fall 2. |
   
    Weitere Informationen zu diesen und anderen mit Giraph-Beispielen verwendeten Parametern finden Sie unter [Giraph Schnellstart](http://giraph.apache.org/quick_start.html).

6. Nach Abschluss des Auftrags werden die Ergebnisse im Verzeichnis **/example/out/shortestpaths** gespeichert. Die Ausgabedateinamen beginnen mit **part-m-** und enden mit einer Zahl, die die erste, zweite, dritte Datei usw. anzeigt. Verwenden Sie den folgenden Befehl, um die Ausgabe anzuzeigen:
   
    ```
    hdfs dfs -text /example/output/shortestpaths/*
    ```
   
    Die Ausgabe sollte in etwa folgendermaßen aussehen:
   
        0    1.0
        4    5.0
        2    2.0
        1    0.0
        3    1.0
   
    Das Beispiel "SimpleShortestPathComputation" wurde hartcodiert und beginnt mit der Objekt-ID 1. Es findet den kürzesten Pfad zu anderen Objekten. Die Ausgabe sollte also `destination_id distance` lauten, wobei der Abstand der Wert (oder das Gewicht) der Kanten ist, der zwischen Objekt-ID 1 und der Ziel-ID zurückgelegt wird.
   
    Wenn Sie dies visualisieren, können Sie die Ergebnisse überprüfen, indem Sie den kürzesten Weg zwischen ID 1 und allen anderen Objekten zurücklegen. Beachten Sie, dass 5 der kürzeste Pfad zwischen ID 1 und ID 4 ist. Dies ist die gesamte Entfernung zwischen <span style="color:orange">ID 1 und 3</span> und dann zwischen <span style="color:red">ID 3 und 4</span>.
   
    ![Zeichnen von Objekten als Kreise mit dem kürzesten Pfad dazwischen](./media/hdinsight-hadoop-giraph-install-linux/giraph-graph-out.png)

## <a name="next-steps"></a>Nächste Schritte

* [Installieren und Verwenden von Hue in HDInsight-Clustern](hdinsight-hadoop-hue-linux.md). Hue ist eine Webbenutzeroberfläche, die das Erstellen, Ausführen und Speichern von Pig- und Hive-Aufträgen sowie das Durchsuchen des Standardspeichers für Ihre HDInsight-Cluster vereinfacht.

* Unter [Installieren und Verwenden von R für HDInsight-Cluster](hdinsight-hadoop-r-scripts-linux.md) finden Sie Anweisungen bezüglich der Clusteranpassung zum Installieren und Verwenden von R in HDInsight Hadoop-Clustern. R ist eine Open-Source-Sprache und -Umgebung für statistische Berechnungen. Sie bietet Hunderte integrierter Statistikfunktionen und eine eigene Programmiersprache, die Aspekte der funktionalen und objektorientierten Programmierung kombiniert. Darüber hinaus werden umfangreiche Grafikfunktionen geboten.

* [Installieren von Solr in HDInsight-Clustern](hdinsight-hadoop-solr-install-linux.md). Verwenden Sie die Clusteranpassung, um Solr in HDInsight Hadoop-Clustern zu installieren. Solr ermöglicht es Ihnen, leistungsstarke Suchvorgänge für gespeicherte Daten durchzuführen.


