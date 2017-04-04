---
title: Verbessern der Columnstore-Indexleistung in Azure SQL | Microsoft-Dokumentation
description: "Reduzieren Sie Arbeitsspeicheranforderungen, oder erhöhen Sie den verfügbaren Arbeitsspeicher, um die Anzahl von Zeilen zu maximieren, die ein Columnstore-Index in jede Zeilengruppe komprimieren kann."
services: sql-data-warehouse
documentationcenter: NA
author: shivaniguptamsft
manager: jhubbard
editor: 
ms.assetid: ef170f39-ae24-4b04-af76-53bb4c4d16d3
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: performance
ms.date: 11/18/2016
ms.author: shigu;barbkess
translationtype: Human Translation
ms.sourcegitcommit: b4802009a8512cb4dcb49602545c7a31969e0a25
ms.openlocfilehash: 8d189256ed4c876859203406cda95ce0be36c96c
ms.lasthandoff: 03/29/2017


---

# <a name="memory-optimizations-for-columnstore-compression"></a>Arbeitsspeicheroptimierung für Columnstore-Komprimierung

Reduzieren Sie Arbeitsspeicheranforderungen, oder erhöhen Sie den verfügbaren Arbeitsspeicher, um die Anzahl von Zeilen zu maximieren, die ein Columnstore-Index in jede Zeilengruppe komprimieren kann.  Verwenden Sie diese Methoden, um die Komprimierungsrate und Abfrageleistung für Columnstore-Indizes zu verbessern.

## <a name="why-the-rowgroup-size-matters"></a>Bedeutung der Größe der Zeilengruppe
Da ein Columnstore-Index eine Tabelle durch die Überprüfung der einzelnen Spaltensegmente von einzelnen Zeilengruppen überprüft, verbessert das Maximieren der Zeilenanzahl in jeder Zeilengruppe die Abfrageleistung. Wenn Zeilengruppen über eine hohe Anzahl von Reihen verfügen, verbessert sich die Datenkomprimierung, d.h., es werden weniger Daten vom Datenträger gelesen.

Weitere Informationen über Zeilengruppen, finden Sie unter [Beschreibung von Columnstore-Indizez](https://msdn.microsoft.com/library/gg492088.aspx).

## <a name="target-size-for-rowgroups"></a>Zielgröße für Zeilengruppen
Für eine optimale Abfrageleistung besteht das Ziel darin, die Anzahl der Zeilen pro Zeilengruppe in einem Columnstore-Index zu maximieren. Eine Zeilengruppe kann bis zu 1.048.576 Zeilen enthalten. Es ist in Ordnung, nicht die maximale Anzahl von Zeilen pro Zeilengruppe zu verwenden. Columnstore-Indizes erzielen gute Leistung, wenn Zeilengruppen mindestens 100.000 Zeilen aufweisen.

## <a name="rowgroups-can-get-trimmed-during-compression"></a>Zeilengruppen können während der Komprimierung gekürzt werden

Während des Massenladens oder einer Neuerstellung des Columnstore-Indizes ist manchmal nicht genug Arbeitsspeicher verfügbar, um alle Zeilen in die vorgesehenen Zeilengruppen zu komprimieren. Im Fall einer Arbeitsspeicherauslastung kürzen Columnstore-Indizes die Zeilengruppengröße, sodass die Komprimierung im Columnstore erfolgen kann.

Reicht der Arbeitsspeicher nicht aus, um mindestens 10.000 Reihen in jede Zeilengruppe zu komprimieren, erzeugt SQL Data Warehouse einen Fehler.

Weitere Informationen zum Massenladen, finden Sie unter [Bulk load into a clustered columnstore index (Massenladen in einen gruppierten Columnstore-Index)](https://msdn.microsoft.com/en-us/library/dn935008.aspx#Bulk load into a clustered columnstore index).

## <a name="how-to-estimate-memory-requirements"></a>Einschätzen der Arbeitsspeicheranforderungen

<!--
To view an estimate of the memory requirements to compress a rowgroup of maximum size into a columnstore index, download and run the view [dbo.vCS_mon_mem_grant](). This view shows the size of the memory grant that a rowgroup requires for compression in to the columnstore.
-->

Der maximale erforderliche Arbeitsspeicher zum Komprimieren einer Zeilengruppe beträgt ungefähr

- 72 MB +
- \#Zeilen \* \#Spalten \* 8 Byte +
- \#Zeilen \* \#kurze Zeichenfolgenspalten \* 32 Bytes +
- \#lange Zeichenfolgenspalten \* 16 MB für das Komprimierungswörterbuch

wo kurze Zeichenfolgenspalten Zeichenfolgendatentypen mit <= 32 Bytes und lange Zeichenfolgenspalten Zeichenfolgendatentypen mit > 32 Bytes verwenden.

Lange Zeichenfolgen werden mit einer Komprimierungsmethode komprimiert, die für das Komprimieren von Texten entwickelt wurde. Diese Komprimierungsmethode verwendet ein *Wörterbuch* zum Speichern von Textmustern. Die maximale Größe eines Wörterbuchs beträgt 16 MB. Es gibt nur ein Wörterbuch für jede lange Zeichenfolgenspalte in der Zeilengruppe.

Eine ausführliche Diskussion der Columnstore-Arbeitsspeicheranforderungen, finden Sie im Video [Azure SQL Data Warehouse scaling: configuration and guidance (Skalieren von Azure SQL Data Warehouse: Konfiguration und Anleitungen, in englischer Sprache)](https://myignite.microsoft.com/videos/14822).

## <a name="ways-to-reduce-memory-requirements"></a>Verfahren zum Verringern der Arbeitsspeicheranforderungen

Befolgen Sie die folgenden Vorgehensweisen, um die Arbeitsspeicheranforderungen für die Komprimierung von Zeilengruppen in den Columnstore-Indizes zu reduzieren.

### <a name="use-fewer-columns"></a>Verwenden von weniger Spalten
Entwerfen Sie, wenn möglich, die Tabelle mit weniger Spalten. Wenn eine Zeilengruppe in den Columnstore komprimiert wird, komprimiert der Columnstore-Index jedes Spaltensegment getrennt. Deshalb erhöhen sich die Arbeitsspeicheranforderungen zur Erhöhung einer Zeilengruppe mit der Anzahl von Spalten.


### <a name="use-fewer-string-columns"></a>Verwenden von weniger Zeichenfolgenspalten
Spalten von Zeichenfolgendatentypen benötigen mehr Arbeitsspeicher als nummerische und Datumsdatentypen. Erwägen Sie, Zeichenfolgenspalten aus Faktentabellen zu entfernen und in kleinere Dimensionstabellen zu setzen, um Arbeitsspeicheranforderungen zu reduzieren.

Zusätzliche Arbeitsspeicheranforderungen für die Komprimierung von Zeichenfolgen:

- Zeichenfolgendatentypen von bis zu 32 Zeichen können 32 zusätzliche Bytes pro Wert voraussetzen.
- Zeichenfolgendatentypen mit mehr als 32 Zeichen werden mithilfe von Wörterbuchmethoden komprimiert.  Jede Spalte in der Zeilengruppe kann bis zu zusätzlich 16 MB benötigen, um das Wörterbuch zu erstellen.

### <a name="avoid-over-partitioning"></a>Vermeiden von übermäßige Partitionierung

Columnstore-Indizes erstellen eine oder mehrere Zeilengruppen pro Partition. In SQL Data Warehouse steigt die Anzahl von Partitionen schnell, da die Daten verteilt werden und jede Verteilung partitioniert ist. Wenn die Tabelle zu viele Partitionen besitzt, gibt es möglicherweise nicht genügend Zeilen, um die Zeilengruppen zu füllen. Das Fehlen von Zeilen erzeugt keine Arbeitsspeicherauslastung während der Komprimierung, führt aber zu Zeilengruppen, die nicht die beste Columnstore-Abfrageleistung erreichen.

Ein weiterer Grund, übermäßige Partitionierung zu vermeiden, ist, dass es eine Arbeitsspeicherauslastung beim Laden von Zeilen in den Columnstore-Index in einer partitionierten Tabelle gibt. Während einer Auslastung können viele Partitionen die eingehenden Zeilen aufnehmen, die im Arbeitsspeicher behalten werden, bis jede Partition genug Zeilen komprimiert hat. Zu viele Partitionen erzeugen zusätzliche Arbeitsspeicherauslastung.

### <a name="simplify-the-load-query"></a>Vereinfachen der Ladeabfrage

Die Datenbank teilt die Arbeitsspeicherzuweisung mit allen Operatoren in der Abfrage. Wenn eine Ladeanfrage komplexe Sortierungen und Verknüpfungen hat, reduziert sich dadurch der Arbeitsspeicher, der für die Komprimierung verfügbar ist.

Entwickeln Sie die Ladeabfrage nur zum Laden von Abfragen Wenn Sie Transformationen an der Datei ausführen müssen, führen Sie sie getrennt von der Ladeabfrage aus. Staffeln Sie die Datei, z.B. in einem Heap, führen sie die Transformationen aus, und laden Sie anschließend die Stagingtabelle in den Columnstore-Index. Sie können die Datei auch zuerst laden und dann das MPP-System zum Transformieren der Datei verwenden.

### <a name="adjust-maxdop"></a>Anpassen von MAXDOP

Jede Verteilung komprimiert Zeilengruppen parallel in den Columnstore, sobald ein oder mehrere CPU-Kerne pro Verteilung verfügbar sind. Die Parallelität erfordert zusätzliche Arbeitsspeicherressourcen, was zur Arbeitsspeicherauslastung und Kürzung der Zeilengruppe führen kann.

Sie können den Tipp für MAXDOP-Anfrage anwenden, damit der Ladevorgang gezwungen wird, innerhalb jeder Verteilung im Serienmodus zu arbeiten, um Arbeitsspeicherauslastung zu verringern.

```
CREATE TABLE MyFactSalesQuota
WITH (DISTRIBUTION = ROUND_ROBIN)
AS SELECT * FROM FactSalesQUota
OPTION (MAXDOP 1);
```

## <a name="ways-to-allocate-more-memory"></a>Möglichkeiten, um mehr Arbeitsspeicher zu belegen

Die DWU-Größe und die Benutzer-Ressourcenklasse bestimmen zusammen, wie viel Arbeitsspeicher für einen Benutzer zur Verfügung steht. Um die Arbeitsspeicherzuweisung für eine Ladeanfrage zu erhöhen, können Sie entweder die Anzahl der DWUs oder die Ressourcenklasse erhöhen.

- Wie Sie die DWUs erhöhen, erfahren Sie unter [Wie skaliere ich Leistung?](sql-data-warehouse-manage-compute-overview.md#scale-compute)
- Wie Sie die Ressourcenklasse für eine Abfrage ändern, erfahren Sie unter [Beispiel: Ändern der Ressourcenklasse eines Benutzers](sql-data-warehouse-develop-concurrency.md#change-a-user-resource-class-example).

Beispielsweise kann ein Benutzer in der Ressourcenklasse „smallrc“ auf DWU-100 100 MB Arbeitsspeicher für jede Verteilung verwenden. Details finden Sie unter [Parallelität in SQL Data Warehouse](sql-data-warehouse-develop-concurrency.md).

Angenommen, Sie stellen fest, dass Sie 700 MB Arbeitsspeicher brauchen, um Zeilengruppengrößen von hoher Qualität zu bekommen. Anhand dieser Beispiele wird gezeigt, wie Sie die Ladeabfrage mit genug Arbeitsspeicher ausführen.

- Indem DWU 1000 und „mediumrc“ verwendet wird, beträgt die Arbeitsspeicherzuweisung 800 MB
- Indem DWU 600 und „largerc“ verwendet wird, beträgt die Arbeitsspeicherzuweisung 800 MB.


## <a name="next-steps"></a>Nächste Schritte

Weitere Möglichkeiten zum Verbessern der Leistung in SQL Data Warehouse finden Sie unter der [Übersicht über die Leistung](sql-data-warehouse-overview-manage-user-queries.md).

<!--Image references-->

<!--Article references-->


<!--MSDN references-->

<!--Other Web references-->

