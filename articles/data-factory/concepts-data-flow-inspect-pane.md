---
title: Azure Data Factory Mapping Data Flow – Überprüfen und Datenvorschau
description: Azure Data Factory Mapping Data Flow verfügt über einen Bereich, der Datenflussmetadaten („Überprüfen“) und eine Datenvorschau im Debugmodus („Datenvorschau“) anzeigt.
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/01/2019
ms.openlocfilehash: e7d03ecd8ea4aecf1e0cfdd02d3b9bc5300abe6b
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2019
ms.locfileid: "56212539"
---
# <a name="azure-data-factory-mapping-data-flow-transformation-inspect-tab"></a>Azure Data Factory Mapping Data Flow – Registerkarte zum Überprüfen der Transformation

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

![Bereich „Überprüfen“](media/data-flow/agg3.png "Bereich „Überprüfen“")

Der Bereich „Überprüfen“ bietet einen Einblick in die Metadaten des Datenstroms, den Sie transformieren. Sie können die Spaltenanzahl, geänderte Spalten, hinzugefügte Spalten, Datentypen, die Spaltensortierung und Spaltenverweise sehen. „Überprüfen“ ist eine schreibgeschützte Ansicht Ihrer Metadaten. Der Debugmodus muss nicht aktiviert sein, um die Metadaten im Bereich „Überprüfen“ anzeigen zu können.

Wenn Sie die Form Ihrer Daten durch Transformationen ändern, wird der Fluss der Metadatenänderungen im Bereich „Überprüfen“ angezeigt. Wenn in Ihrer Quelltransformation kein definiertes Schema vorhanden ist, werden im Bereich „Überprüfen“ keine Metadaten angezeigt. Fehlende Metadaten sind in Schemaabweichungsszenarien üblich.

![Datenvorschau](media/data-flow/datapreview.png "Datenvorschau")

Die „Datenvorschau“ ist ein Bereich, der eine schreibgeschützte Ansicht Ihrer Daten während der Transformation bietet. Sie können das Ergebnis Ihrer Transformation und Ausdrücke zum Überprüfen Ihres Datenflusses anzeigen. Der Debugmodus muss aktiviert sein, um Datenvorschauen anzeigen zu können. Wenn Sie auf Spalten im Datenvorschauraster klicken, wird auf der rechten Seite ein weiterer Bereich angezeigt. Im Popout-Bereich werden die Profilstatistiken zu den einzelnen Spalten angezeigt, die Sie auswählen.

![Datenvorschaudiagramm](media/data-flow/chart.png "Datenvorschaudiagramm")

