---
title: API-Referenz – Content Moderator
titlesuffix: Azure Cognitive Services
description: Erfahren Sie mehr über die verschiedenen Inhaltsmoderations- und die Überprüfungs-APIs für Content Moderator.
services: cognitive-services
author: sanjeev3
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: reference
ms.date: 06/25/2017
ms.author: sajagtap
ms.openlocfilehash: a6bd92a675885cb343b07ef981c38571af0a931c
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/07/2019
ms.locfileid: "55862461"
---
# <a name="content-moderator-api-reference"></a>Content Moderator-API-Referenz

Sie können die ersten Schritte mit Azure Content Moderator-APIs folgendermaßen unternehmen: (Siehe auch [Verwalten von Anmeldeinformationen](review-tool-user-guide/credentials.md).)

- [Abonnieren Sie die Content Moderator-APIs](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesContentModerator) im Azure-Portal.
- Registrieren Sie sich für das [Content Moderator-Prüfungstool](https://contentmoderator.cognitive.microsoft.com/). Siehe [Testen von Content Moderator im Web](quick-start.md).

## <a name="moderation-apis"></a>Moderations-APIs

Sie können die folgenden Content Moderator-APIs zum Einrichten Ihrer Workflows für die Beitragsmoderation verwenden.

| BESCHREIBUNG | Verweis |
| -------------------- |-------------|
| **Bildmoderations-API**<br /><br />Durchsuchen Sie Bilder und erkennen Sie anzügliche sowie nur für Erwachsene geeignete Inhalte mithilfe von Tags, Zuverlässigkeitsbewertungen und anderen extrahierten Informationen. <br /><br />Anhand dieser Informationen können Sie den Inhalt in Ihrem Workflow für die Beitragsmoderation veröffentlichen, ablehnen oder überprüfen. <br /><br />| [Bildmoderations-API-Referenz](https://westus.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66c "Bildmoderations-API-Referenz")   |
| **Textmoderations-API**<br /><br />Durchsuchen Sie Textinhalt. Es werden anstößige Ausdrücke und persönlich identifizierbare Informationen (PII) zurückgegeben. <br /><br />Anhand dieser Informationen können Sie den Inhalt in Ihrem Workflow für die Beitragsmoderation veröffentlichen, ablehnen oder überprüfen.<br /><br /> | [Textmoderations-API-Referenz](https://westus.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66f "Textmoderations-API-Referenz")   |
| **Videomoderations-API**<br /><br />Durchsuchen Sie Videos und erkennen Sie anzügliche sowie nur für Erwachsene geeignete Inhalte. <br /><br />Anhand dieser Informationen können Sie den Inhalt in Ihrem Workflow für die Beitragsmoderation veröffentlichen, ablehnen oder überprüfen.<br /><br /> | [Videomoderations-API – Überblick](video-moderation-api.md "Videomoderations-API – Überblick")   |
| **Listenverwaltungs-API**<br /><br />Erstellen und verwalten Sie benutzerdefinierte Ausschluss- und Aufnahmeliste von Bildern und Text. Nach der Aktivierung überprüfen die Vorgänge **Image - Match** und **Text - Screen**, ob eine Fuzzyübereinstimmung zwischen dem übermittelten Inhalt und Ihren benutzerdefinierten Listen besteht. <br /><br />Aus Effizienzgründen können Sie den auf maschinellen Lernen basierenden Moderationsschritt überspringen.<br /><br /> | [Listenverwaltungs-API-Referenz](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f675 "Listenverwaltungs-API-Referenz")   |

## <a name="review-api"></a>Überprüfungs-API

Die Überprüfungs-API enthält die folgenden Komponenten:

| BESCHREIBUNG | Verweis |
| -------------------- |-------------|
| **Aufträge**<br /><br /> Starten Sie Moderationsworkflows zum Durchsuchen und Überprüfen von Bild- und Textinhalten. Ein Moderationsauftrag durchsucht Ihre Inhalte mithilfe der Bildmoderations-API und der Textmoderations-API. Moderationsaufträge verwenden definierte und Standardworkflows, um Überprüfungen zu generieren. <br /><br />Nachdem ein menschlicher Moderator die automatisch zugewiesenen Tags und Vorhersagedaten überprüft und eine Entscheidung zur Inhaltsmoderation getroffen hat, sendet die Überprüfungs-API alle Informationen an Ihren API-Endpunkt.<br /><br /> | [Auftragsreferenz](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c5 "Auftragsreferenz")   |
| **Überprüfungen**<br /><br />Verwenden Sie das Prüfungstool, um direkt Bild- oder Textüberprüfungen für menschliche Moderatoren zu erstellen.<br /><br /> Nachdem ein menschlicher Moderator die automatisch zugewiesenen Tags und Vorhersagedaten überprüft und eine Entscheidung zur Inhaltsmoderation getroffen hat, sendet die Überprüfungs-API alle Informationen an Ihren API-Endpunkt.<br /><br /> | [Überprüfungsreferenz](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c4 "Überprüfungsreferenz")   |
| **Workflows**<br /><br />Erstellen, aktualisieren und erhalten Sie Details zu den benutzerdefinierten Workflows, die Ihr Team erstellt. Mithilfe des Prüfungstools können Sie Workflows definieren. <br /> <br />Workflows verwenden normalerweise Content Moderator, können aber auch bestimmte andere APIs verwenden, die als Konnektoren im Prüfungstool verfügbar sind.<br /><br /> | [Workflowreferenz](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/5813b46b3f9b0711b43c4c59 "Workflowreferenz")   |


