---
title: Kategorisieren von Bildern – Maschinelles Sehen
titleSuffix: Azure Cognitive Services
description: Machen Sie sich mit Konzepten des Bildkategorisierungsfeatures der Maschinelles Sehen-API vertraut.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 02/11/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: a85fb65ad971bfc70bdab0689e99921e616b56ac
ms.sourcegitcommit: f7be3cff2cca149e57aa967e5310eeb0b51f7c77
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/15/2019
ms.locfileid: "56308858"
---
# <a name="categorize-images"></a>Kategorisieren von Bildern

Zusätzlich zu Tags und Beschreibungen gibt maschinelles Sehen auf der Taxonomie basierende Kategorien zurück, die in einem Bild erkannt wurden. Im Gegensatz zu Tags sind Kategorien in einer Vererbungshierarchie mit über- und untergeordneten Elementen organisiert. Außerdem gibt es weniger davon (86, im Gegensatz zu Tausenden von Tags). Alle Kategorienamen sind in Englisch. Die Kategorisierung kann eigenständig oder parallel zum neueren Tagmodell erfolgen.

## <a name="the-86-category-concept"></a>Das 86-Kategorien-Konzept

Maschinelles Sehen kann ein Bild mithilfe der Liste mit 86 Kategorien in der folgenden Abbildung allgemein oder spezifisch kategorisieren. Die vollständige Taxonomie im Textformat finden Sie unter [Kategorietaxonomie](category-taxonomy.md).

![Gruppierte Listen aller Kategorien in der Kategorietaxonomie](./Images/analyze_categories-v2.png)

## <a name="image-categorization-examples"></a>Beispiele für die Kategorisierung von Bildern

Die folgende JSON-Antwort veranschaulicht, was das maschinelle Sehen bei der Kategorisierung des Beispielbilds anhand seiner visuellen Merkmale zurückgibt.

![Eine Frau auf dem Dach eines Wohnblocks](./Images/woman_roof.png)

```json
{
    "categories": [
        {
            "name": "people_",
            "score": 0.81640625
        }
    ],
    "requestId": "bae7f76a-1cc7-4479-8d29-48a694974705",
    "metadata": {
        "height": 200,
        "width": 300,
        "format": "Jpeg"
    }
}
```

Die folgende Tabelle zeigt einen typischen Satz von Bildern und die vom maschinellen Sehen zurückgegebene Kategorie für jedes Bild.

| Image | Category (Kategorie) |
|-------|----------|
| ![Vier Personen, die zusammen als Familie posieren](./Images/family_photo.png) | people_group |
| ![Ein Welpe, der in einem Feld sitzt](./Images/cute_dog.png) | animal_dog |
| ![Eine Person, die bei Sonnenuntergang auf einem Gebirgsfelsen steht](./Images/mountain_vista.png) | outdoor_mountain |
| ![Mehrere Brötchen auf einem Tisch](./Images/bread.png) | food_bread |

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über Konzepte zum [Taggen von Bildern](concept-tagging-images.md) und [Beschreiben von Bildern](concept-describing-images.md).
