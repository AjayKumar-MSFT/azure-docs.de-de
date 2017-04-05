---
title: 'Schritt 2: Hochladen von Daten in ein Machine Learning-Experiment | Microsoft Docs'
description: "Exemplarische Vorgehensweise zur Entwicklung einer Vorhersagelösung – Schritt 2: Hochladen gespeicherter öffentlicher Daten in Azure Machine Learning Studio."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 9f4bc52e-9919-4dea-90ea-5cf7cc506d85
ms.service: machine-learning
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/23/2017
ms.author: garye
translationtype: Human Translation
ms.sourcegitcommit: 4f2230ea0cc5b3e258a1a26a39e99433b04ffe18
ms.openlocfilehash: c2ab5f5252e1ea1ec51f6c3bd489826c70ff011c
ms.lasthandoff: 03/25/2017


---
# <a name="walkthrough-step-2-upload-existing-data-into-an-azure-machine-learning-experiment"></a>Anleitung Schritt 2: Hochladen vorhandener Daten in ein Azure Machine Learning-Experiment
Dies ist der zweite Schritt der exemplarischen Vorgehensweise zum [Entwickeln einer Predictive Analytics-Lösung mit Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md)

1. [Erstellen eines Machine Learning-Arbeitsbereichs](machine-learning-walkthrough-1-create-ml-workspace.md)
2. **Hochladen vorhandener Daten**
3. [Erstellen eines neuen Experiments](machine-learning-walkthrough-3-create-new-experiment.md)
4. [Trainieren und Bewerten der Modelle](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5. [Bereitstellen des Webdiensts](machine-learning-walkthrough-5-publish-web-service.md)
6. [Zugreifen auf den Webdienst](machine-learning-walkthrough-6-access-web-service.md)

- - -
Um ein Vorhersagemodell für Kreditrisiken zu entwickeln, benötigen wir Daten, die wir zum Trainieren und anschließenden Testen des Modells verwenden können. In dieser exemplarischen Vorgehensweise verwenden wir den Datensatz „UCI Statlog (German Credit Data)“ aus dem UC Irvine Machine Learning-Repository. Sie finden den Datensatz unter folgender URL:   
<a href="http://archive.ics.uci.edu/ml/datasets/Statlog+(German+Credit+Data)">http://archive.ics.uci.edu/ml/datasets/Statlog+(German+Credit+Data)</a>

Wir werden die Datei mit dem Namen **german.data**verwenden. Laden Sie die Datei auf Ihre lokale Festplatte herunter.  

Das Dataset **german.data** enthält Zeilen mit 20 Variablen für 1.000 Kreditantragsteller aus der Vergangenheit. Diese 20 Variablen stellen den Featuresatz (*Featurevektor*) des Datasets dar, der Identifikationseigenschaften für die einzelnen Kreditantragsteller enthält. Eine zusätzliche Spalte in jeder Zeile enthält das berechnete Kreditrisiko der Antragsteller. 700 der Antragsteller wurden mit niedrigem Risiko klassifiziert und 300 mit hohem Risiko.

Auf der UCI-Website finden Sie eine Beschreibung der Attribute des Funktionsvektors für diese Daten. Enthalten sind z.B. finanzielle Informationen, Bonitätsgeschichte, Beschäftigungsstatus und persönliche Daten. Für jeden Antragsteller wurde eine binäre Bewertung vergeben, um zwischen niedrigem und hohem Kreditrisiko zu unterscheiden. 

Wir werden unser Vorhersageanalytikmodell anhand dieser Daten trainieren. Anschließend sollte unser Modell einen Funktionsvektor für neue Personen akzeptieren und vorhersagen können, ob diese ein niedriges oder hohes Kreditrisiko haben.  

Hier ein interessantes Extra. In der Beschreibung des Datasets auf der UCI-Website wird auf die Kosten einer Fehlklassifizierung des Kreditrisikos einer Person hingewiesen.
Wenn mit dem Modell ein hohes Kreditrisiko für eine Person vorhergesagt wird, die eigentlich ein niedriges Kreditrisiko aufweist, liegt eine Fehlklassifizierung des Modells vor.
Die umgekehrte Fehlklassifizierung, d.h. die Vorhersage eines niedrigen Kreditrisikos für eine Person, die eigentlich ein hohes Kreditrisiko aufweist, ist jedoch fünfmal so teuer für die Finanzinstitution.

Unser Modell soll daher so trainiert werden, dass die Kosten dieser letzteren Fehlklassifizierung fünfmal höher als die der Fehlklassifizierung in der anderen Richtung ausfallen.
Dies kann beim Trainieren des Modells in unserem Experiment auf einfache Weise berücksichtigt werden, indem die entsprechenden Einträge von Personen mit einem hohen Kreditrisiko (fünfmal) dupliziert werden. Wenn mit dem Modell eine Person dann fälschlicherweise als Person mit einem niedrigen Kreditrisiko klassifiziert wird, wird diese Fehlklassifizierung im Modell fünfmal ausgeführt, d.h. einmal pro Duplikat. Auf diese Weise werden die Kosten für diesen Fehler in den Trainingsergebnissen erhöht.


## <a name="convert-the-dataset-format"></a>Konvertieren des Datensatzformats
Der Originaldatensatz verwendet ein Format mit Trennung durch Leerzeichen. Machine Learning Studio funktioniert besser mit durch Trennzeichen getrennten Dateien (CSV). Daher werden wir den Datensatz konvertieren, indem wir die Leerzeichen durch Kommas ersetzen.  

Es gibt viele Möglichkeiten zum Konvertieren dieser Daten. Eine ist die Verwendung des folgenden Windows PowerShell-Befehls:   

    cat german.data | %{$_ -replace " ",","} | sc german.csv  

Eine andere ist die Verwendung des sed-Befehls unter Unix:  

    sed 's/ /,/g' german.data > german.csv  

In beiden Fällen haben wir eine durch Kommas getrennte Version der Daten in der Datei **german.csv** erstellt, die wir in unserem Experiment verwenden können.

## <a name="upload-the-dataset-to-machine-learning-studio"></a>Hochladen des DataSets in Machine Learning Studio
Nach dem Konvertieren der Daten in das CSV-Format müssen Sie sie in Machine Learning Studio hochladen. 

1. Öffnen Sie die Startseite von Machine Learning Studio ([https://studio.azureml.net/Home](https://studio.azureml.net)). 

2. Klicken Sie im Fenster oben links auf das ![Menü][1], klicken Sie auf **Azure Machine Learning**, wählen Sie **Studio** aus, und melden Sie sich an.

3. Klicken Sie im unteren Seitenbereich auf **+NEU** .

4. Wählen Sie **DATASET**.

5. Klicken Sie auf **AUS LOKALER DATEI**.

    ![Hinzufügen eines Datasets aus einer lokalen Datei][2]

6. Klicken Sie im Dialogfeld **Neuen Datensatz hochladen** auf **Durchsuchen**, und suchen Sie nach der zuvor erstellten Datei **german.csv**.

7. Geben Sie einen Namen für das Dataset ein. Nennen Sie es in dieser exemplarischen Vorgehensweise „UCI German Credit Card Data“.

8. Wählen Sie den Datentyp **Generic CSV File With no header (.nh.csv)**aus.

9. Fügen Sie bei Bedarf eine Beschreibung hinzu.

10. Klicken Sie auf das Häkchen **OK**.  

    ![Hochladen des DataSets][3]

Daraufhin werden die Daten in ein DataSet-Modul hochgeladen, das wir in einem Experiment verwenden können.

Zum Verwalten von Datasets, die Sie in Studio hochgeladen haben, klicken Sie auf der linken Seite des Studio-Fensters auf die Registerkarte **DATASETS**.

![Verwalten von Datasets][4]

Weitere Informationen zum Importieren anderer Datentypen in einem Experiment finden Sie unter [Importieren von Trainingsdaten in Azure Machine Learning Studio](machine-learning-data-science-import-data.md).

**Als Nächstes: [Erstellen eines neuen Experiments](machine-learning-walkthrough-3-create-new-experiment.md)**

[1]: media/machine-learning-walkthrough-2-upload-data/menu.png
[2]: media/machine-learning-walkthrough-2-upload-data/add-dataset.png
[3]: media/machine-learning-walkthrough-2-upload-data/upload-dataset.png
[4]: media/machine-learning-walkthrough-2-upload-data/dataset-list.png

