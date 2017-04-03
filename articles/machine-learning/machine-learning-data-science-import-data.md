---
title: Importieren von Daten in Machine Learning Studio | Microsoft Docs
description: "Erfahren Sie, wie Sie Trainingsdaten aus verschiedenen Datenquellen in Azure Machine Learning Studio importieren. Erfahren Sie, welche Datentypen und Datenformate unterstützt werden."
keywords: Importieren von Daten,Datenformat,Datentypen,Datenquellen,Trainingsdaten
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: c194ee3b-838c-4efe-bb2a-c1d052326216
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: garye;bradsev
translationtype: Human Translation
ms.sourcegitcommit: a6bc79b2cb5b73109cddd6cf57caeba754b52e2e
ms.openlocfilehash: a35bc89044ebe8ea8e4a0e4a883c30fb8e8d879a
ms.lasthandoff: 02/11/2017


---
# <a name="import-your-training-data-into-azure-machine-learning-studio-from-various-data-sources"></a>Importieren von Trainingsdaten aus verschiedenen Datenquellen in Azure Machine Learning Studio
Um Ihre eigenen Daten in Machine Learning Studio zum Entwickeln und Trainieren einer Predictive Analytics-Lösung zu verwenden, können Sie folgende Aktionen ausführen: 

* Hochladen von Daten aus einer **lokalen Datei** von der Festplatte im Voraus, um damit ein Dataset-Modul in Ihrem Arbeitsbereich zu erstellen
* Zugreifen auf Daten aus einer von mehreren **Onlinedatenquellen**, während Ihr Experiment mit dem [Import Data][import-data]-Modul ausgeführt wird 
* Verwenden von Daten aus einem anderen Azure Machine Learning-**Experiment**, die als Dataset gespeichert sind
* Verwenden von Daten aus einer lokalen **SQL Server-Datenbank**

Diese Optionen werden in den Themen im folgenden Menü beschrieben. In diesen Themen wird das Importieren von Daten aus verschiedenen Datenquellen für die Verwendung in Machine Learning Studio veranschaulicht. 

[!INCLUDE [import-data-into-aml-studio-selector](../../includes/machine-learning-import-data-into-aml-studio.md)]

> [!NOTE]
> Es gibt eine Reihe von Beispiel-Datasets in Machine Learning Studio, die Sie als Trainingsdaten verwenden können. Weitere Informationen finden Sie unter [Verwenden von Beispiel-DataSets in Azure Machine Learning Studio](machine-learning-use-sample-datasets.md)).
> 
> 

In diesem Einführungsthema wird auch erläutert, wie Sie Daten für die Verwendung in Machine Learning Studio vorbereiten, und es wird beschrieben, welche Datenformate und Datentypen unterstützt werden. 

> [!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]
> 
> 

## <a name="get-data-ready-for-use-in-azure-machine-learning-studio"></a>Vorbereiten von Daten für die Verwendung in Azure Machine Learning Studio
Machine Learning Studio ist für die Nutzung von rechteckigen oder tabellarischen Daten vorgesehen, z. B. Textdaten, bei denen es sich um mit Trennzeichen versehene oder strukturierte Daten aus einer Datenbank handelt. Unter Umständen können nicht rechteckige Daten verwendet werden.

Möglichst sollten relativ saubere Daten verwendet werden. Sie sollten also Probleme wie Zeichenfolgen ohne Anführungszeichen beheben, bevor Sie die Daten in das Experiment hochladen.

In Machine Learning Studio stehen jedoch Module zur Verfügung, mit denen Sie einige Änderungen an Daten im Experiment vornehmen können. Abhängig vom verwendeten Machine Learning-Algorithmen müssen Sie möglicherweise entscheiden, wie Sie mit Datenstrukturproblemen wie fehlenden Werten und geringen Datenmengen umgehen möchten. Es gibt Module, die dabei helfen können. Suchen Sie im Abschnitt **Data Transformation** der Modulpalette nach Modulen, die diese Funktionen ausführen.

Zu jedem Zeitpunkt des Experiments können Sie die Daten anzeigen oder herunterladen, die von einem Modul erstellt werden, indem Sie auf den Ausgabeport klicken. Je nach Modul können verschiedene Downloadoptionen verfügbar sein, oder Sie können die Daten in Ihrem Webbrowser in Machine Learning Studio anzeigen.

## <a name="data-formats-and-data-types-supported"></a>Unterstützte Datenformate und Datentypen
Sie können eine Reihe von Datentypen in das Experiment importieren, je nachdem, welchen Mechanismus Sie zum Importieren von Daten verwenden und woher die Daten stammen:

* Nur-Text (.txt)
* Durch Trennzeichen getrennte Werte (CSV) mit einer Kopfzeile (.csv) oder ohne (.nh.csv)
* Durch Tabulator getrennte Werte (TSV) mit einer Kopfzeile (.tsv) oder ohne (.nh.tsv)
* Excel-Datei
* Azure-Tabelle
* Hive-Tabelle
* SQL-Datenbanktabelle
* OData-Werte
* SVMLight-Daten (.svmlight) (Formatinformationen finden Sie in der [SVMLight-Definition](http://svmlight.joachims.org/) (in englischer Sprache))
* ARFF-Daten (Attribute Relation File Format) (.arff) (Formatinformationen finden Sie in der [ARFF-Definition](http://weka.wikispaces.com/ARFF) (in englischer Sprache))
* ZIP-Datei (.zip)
* R-Objektdatei oder -Arbeitsbereichsdatei (.RData)

Wenn Sie Daten in einem Format wie z. B. ARFF importieren, die Metadaten enthalten, verwendet Machine Learning Studio diese Metadaten, um die Kopfzeile und den Datentyp der einzelnen Spalten zu definieren.

Wenn Sie z. B. Daten im TSV- oder CSV-Format importieren, das diese Metadaten nicht enthält, leitet Machine Learning Studio den Datentyp für jede Spalte ab, indem Stichproben der Daten entnommen werden. Wenn die Daten zudem keine Spaltenüberschriften aufweisen, gibt Machine Learning Studio Standardnamen an.

Sie können die Überschriften und Datentypen für Spalten mit [Edit Metadata][edit-metadata] (Metadaten bearbeiten) explizit angeben oder ändern.

Die folgenden **Datentypen** werden von Machine Learning Studio erkannt:

* String
* Integer
* Double
* Boolean
* DateTime
* TimeSpan

Machine Learning Studio verwendet einen internen Datentyp mit dem Namen ***Data Table*** zum Übergeben von Daten zwischen Modulen. Mit dem Modul [Convert to Dataset][convert-to-dataset] können Sie Daten explizit in das Data Table-Format konvertieren.

Jedes Modul, das andere Formate als Data Table akzeptiert, konvertiert die Daten im Hintergrund vor der Übergabe an das nächste Modul in das Data Table-Format.

Bei Bedarf können Sie das Data Table-Format mit anderen Konvertierungsmodulen wieder in die Formate CSV, TSV, ARFF oder SVMLight konvertieren.
Suchen Sie im Abschnitt **Data Format Conversions** der Modulpalette nach Modulen, die diese Funktionen ausführen.

<!-- Module References -->
[convert-to-dataset]: https://msdn.microsoft.com/library/azure/72bf58e0-fc87-4bb1-9704-f1805003b975/
[edit-metadata]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/

