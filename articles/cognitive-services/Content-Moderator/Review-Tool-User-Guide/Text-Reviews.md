---
title: Überprüfen von Text in Azure Content Moderator – Content Moderator
description: Erfahren Sie, wie Sie Text im Content Moderator überprüfen können, um seine Bewertung und die erkannten Tags anzuzeigen. Verwenden Sie die Informationen, um zu entscheiden, ob der Inhalt angemessen ist.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: article
ms.date: 01/10/2019
ms.author: sajagtap
ms.openlocfilehash: 896956653ed924b7e30e63c1ae5cfa90dd0cc94d
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55219786"
---
# <a name="let-human-reviewers-review-text"></a>Lassen Sie menschliche Prüfer Texte prüfen

Sie können Azure Content Moderator verwenden, um Text anhand von Bewertungen und erkannten Tags zu überprüfen. Verwenden Sie die Informationen, um zu entscheiden, ob der Inhalt angemessen ist. 

## <a name="select-or-enter-the-text-to-review"></a>Auswählen oder Eingeben des zu überprüfenden Textes

Wählen Sie in Content Moderator die Registerkarte **Ausprobieren**. Wählen Sie dann die **Text**-Option, um den Startbildschirm für die Textmoderation aufzurufen. Geben Sie einen beliebigen Text ein oder übermitteln Sie den Standardtext für die automatische Textmoderation. Sie können maximal 1.024 Zeichen eingeben.

## <a name="get-ready-to-review-results"></a>Vorbereiten der Ergebnisüberprüfung

Das Prüfungstool ruft zunächst die Textmoderations-API auf. Anschließend werden mithilfe der erkannten Tags Textüberprüfungen generiert. Das Prüfungstool gleicht die Ergebnisse für Ihr Team ab.

## <a name="review-text-results"></a>Überprüfen der Textergebnisse

Die ausführlichen Ergebnisse werden in den Fenstern angezeigt. Die Ergebnisse enthalten erkannten Tags und Ausdrücke, die von der Textmoderations-API zurückgegeben wurden. Um den Auswahlstatus eines Tags zu wechseln, wählen Sie den Tag aus. Sie können auch mit beliebigen benutzerdefinierten Tags arbeiten, die Sie möglicherweise erstellt haben.

![Screenshot des Überprüfungstools mit markiertem Text in einem Chrome-Browserfenster](../images/reviewresults_text.png)
