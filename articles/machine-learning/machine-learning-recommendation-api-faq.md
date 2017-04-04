---
title: Einrichten und Verwenden der Machine Learning-Empfehlungen-API | Microsoft Docs
description: FAQ zur mit Azure Machine Learning erstellten Microsoft RECOMMENDATIONS API
services: machine-learning
documentationcenter: 
author: LuisCabrer
manager: jhubbard
editor: cgronlun
ms.assetid: 1ffc3c16-e040-4225-9d72-105129938dfa
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: luisca
ROBOTS: NOINDEX, NOFOLLOW
redirect_url: machine-learning-datamarket-deprecation
translationtype: Human Translation
ms.sourcegitcommit: 9ac2a1bf5987fc6fc30e20a1b4a10340eeed3825
ms.openlocfilehash: 15cf891b9c31e1f2e2b108e631f32883ce2f36a7
ms.lasthandoff: 02/17/2017


---
# <a name="setting-up-and-using-machine-learning-recommendations-api-faq"></a>Häufig gestellte Fragen zur Einrichtung und Verwendung der Machine Learning RECOMMENDATIONS-API
**Was ist RECOMMENDATIONS?**

> [!NOTE]
> Beginnen Sie mit der Nutzung der Empfehlungs-API des Cognitive Service anstatt mit dieser Version. Der Recommendations Cognitive Service wird diesen Dienst ersetzen, weshalb alle neuen Features dafür entwickelt werden. Der Dienst bietet neue Funktionen wie Unterstützung der Batchverarbeitung, einen besseren API-Explorer, eine übersichtlichere API-Oberfläche, eine einheitlicherere Registrierungs-/Abrechnungsumgebung usw.
> Erfahren Sie mehr zur [Migration zum neuen Cognitive Service](http://aka.ms/recomigrate)
> 
> 

Für Organisationen und Unternehmen, die für Cross-Selling und Up-Selling auf Empfehlungen bauen, ist RECOMMENDATIONS von Azure Machine Learning ein Self-Service-Empfehlungsmodul. Es handelt sich um eine Implementierung von kombinierten Filtern, die als Kernalgorithmus eine Matrixfaktorisierung einsetzen. Anwendungsentwickler können mithilfe von REST-APIs auf RECOMMENDATIONS zugreifen. 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

**Was kann ich mit RECOMMENDATIONS tun?**

RECOMMENDATIONS akzeptiert als Eingabe ein Element oder einen Satz von Elementen und gibt eine Liste der relevanten Empfehlungen. Beispiel: Ein Kunde eines Online-Händlers klickt auf ein Produkt. Der Online-Händler sendet dieses Produkt als Eingabe für RECOMMENDATIONS, ruft eine Liste der Produkte ab und entscheidet, welches dieser Produkte dem Kunden angezeigt wird. Sie wollen RECOMMENDATIONS nutzen, um Ihren Onlinestore zu optimieren oder um Ihre Verkaufsabteilung oder Ihr Call Center zu informieren.

**Gibt es Nutzungsbeschränkungen?**

Azure Machine Learning-Empfehlungen hat die folgenden Nutzungsbeschränkungen:

* Maximale Anzahl von Modellen pro Abonnement: 10
* Maximale Anzahl von Elementen, die ein Katalog aufnehmen kann: 100.000
* Die maximale Menge der Nutzungspunkte, die aufbewahrt werden, beträgt etwa&5;.000.000. Die ältesten werden gelöscht, wenn neue hochgeladen oder gemeldet werden.
* Die maximale Größe der Daten, die per E-Mail gesendet werden können (z. B. Importieren von Katalog- oder Nutzungsdaten), beträgt 200 MB.
* Die Anzahl der Transaktionen pro Sekunde bei Empfehlungsmodellbuilds, die nicht aktiv sind, beträgt etwa&2; T/s. Ein aktives Empfehlungsmodellbuild kann bis zu 20 TPS aufnehmen.

## <a name="purchase-and-billing"></a>Einkauf und Abrechnung
**Wie hoch ist der Preis für RECOMMENDATIONS während der Startphase?**

RECOMMENDATIONS ist ein abonnementbasierter Dienst. Das Aufladen erfolgt anhand der Anzahl von Transaktionen pro Monat. Auf der [Angebotsseite](https://datamarket.azure.com/dataset/amla/recommendations) des Microsoft Azure Marketplace finden Sie Preisinformationen.

**Entstehen Kosten, wenn RECOMMENDATIONS das Nachverfolgen und/oder Speichern der Benutzeraktivitäten übernimmt?**

Nicht im Moment.

**Gibt es eine kostenlose Testversion von RECOMMENDATIONS?**

Es gibt eine kostenlose Testversion, die auf 10.000 Transaktionen pro Monat beschränkt ist.

**Wann erhalte ich eine Rechnung für RECOMMENDATIONS?**

Ein bezahltes Abonnement ist jedes Abonnement, für das eine monatliche Gebühr fällig wird. Beim Kauf eines kostenpflichtigen Abonnements wird Ihnen sofort der erste Monat in Rechnung gestellt. Sie bezahlen den Betrag entsprechend den Angaben im Angebot auf der Abonnementseite (zzgl. Steuern). Diese Gebühr ist monatlich fällig, immer zu den Tag, an dem Sie Ihr Abonnement bestellten, und zwar solange bis Sie das Abonnement kündigen. 

**Wie aktualisiere ich auf einen Dienst höherer Ebene?**

Sie können Ihr Abonnement auf der [Angebotsseite](https://datamarket.azure.com/dataset/amla/recommendations) des Microsoft Azure Marketplace erwerben oder aktualisieren.

Wenn Sie ein Abonnement aktualisieren:

* Ihre im alten Abonnement verbleibenden Transaktionen werden nicht auf Ihr neues Abonnement übertragen. 
* Sie bezahlen den Preis für das neue Abonnement, auch wenn Sie nicht verwendete Transaktionen auf Ihrem alten Abonnement haben

Vorgehensweise zum Aktualisieren eines Abonnements:

* Navigieren Sie zur [Angebotsseite](https://datamarket.azure.com/dataset/amla/recommendations).
* Melden Sie sich ggf. beim Marketplace an.
* Im rechten Bereich werden alle verfügbaren Pläne aufgeführt. Klicken Sie auf das Optionsfeld für den zu aktualisierenden Plan.
* Klicken Sie auf **OK**, wenn Sie aktualisieren möchten. Wenn Sie nicht aktualisieren möchten, klicken Sie auf **Abbrechen**.

**Wichtig:** Lesen Sie den Text im Dialogfeld vor dem Update genau durch, da Auswirkungen auf die Abrechnung und die Nutzung entstehen können.

**Wann endet mein RECOMMENDATIONS-Abonnement?**

Ihr Abonnement wird beendet, wenn Sie das Abonnement kündigen. Wenn Sie Ihre Abonnements kündigen möchten, beachten Sie die folgenden Anweisungen.

**Wie kündige ich mein RECOMMENDATIONS-Abonnement?**

Gehen Sie folgendermaßen vor, um Ihr Abonnement zu kündigen. Wenn Ihr aktuelles Abonnement ein kostenpflichtiges Abonnement ist, bleibt Ihr Abonnement bis zum Ende des aktuellen Abrechnungszeitraums gültig. Wenn die Kündigung sofort wirksam sein soll, kontaktieren Sie den [Microsoft Support](https://support.microsoft.com/oas/default.aspx?gprid=17024&st=1&wfxredirect=1&sd=gn).

**Hinweis:** Es erfolgt keine Erstattung, wenn Sie vor dem Ende eines Abrechnungszeitraums kündigen oder über nicht verwendete Transaktionen verfügen.

* Navigieren Sie zur [Angebotsseite](https://datamarket.azure.com/dataset/amla/recommendations).
* Melden Sie sich ggf. beim Marketplace an.
* Klicken Sie auf rechts neben dem DataSet-Namen und -Status auf **Abbrechen** . Sie können dieses Abonnement bis an das Ende des aktuellen Abrechnungszeitraums oder bis zum Erreichen der Transaktionsgrenze nutzen (je nachdem, was zuerst eintritt).

Wenn Sie Ihr Abonnement sofort kündigen möchten, damit Sie ein neues Abonnement erwerben können, können Sie hier beim [Microsoft Support](https://support.microsoft.com/oas/default.aspx?gprid=17024&st=1&wfxredirect=1&sd=gn)ein Ticket beantragen.

## <a name="getting-started-with-recommendations"></a>Erste Schritte mit RECOMMENDATIONS
**Lohnt sich RECOMMENDATIONS für mich?** 

RECOMMENDATIONS in Machine Learning eignet sich für Organisationen und Unternehmen, die beim Cross- und Upselling von Produkten und Dienstleistungen an ihre Kunden auf Empfehlungen bauen. Wenn Sie über eine kundenorientierte Website, Verkaufspersonal, Vertriebsmitarbeiter oder ein Callcenter ebenso verfügen wie über einen Katalog mit mehr als nur einigen Dutzend Produkten oder Dienstleistungen, kann Ihr Betriebsergebnis von RECOMMENDATIONS profitieren. 

Das Experimentieren mit RECOMMENDATIONS gestaltet sich in der Regel recht einfach. Die aktuelle API-basierte Version erfordert grundlegende Programmierkenntnisse. Wenn Sie Unterstützung benötigen, wenden Sie sich an den Anbieter, der Ihre Website entwickelt hat. Wenn Sie über eine interne IT-Abteilung oder einen internen Entwickler verfügen, sollte sich RECOMMENDATIONS für Sie eignen. 

**Was sind die Voraussetzungen für das Einrichten von RECOMMENDATIONS?**

RECOMMENDATIONS erfordert ein Protokoll der Benutzerauswahl in Bezug auf den Katalog. Falls Sie nicht über ein solches Protokoll, jedoch über eine Kundenwebsite verfügen, kann RECOMMENDATIONS möglicherweise Benutzeraktivitäten für Sie erfassen. 

RECOMMENDATIONS erfordert außerdem einen Katalog mit Produkten und Diensten. Für den Fall, dass Sie keinen Katalog haben, kann RECOMMENDATIONS die tatsächliche Nutzung der Kundendaten verwenden und einen Katalog destillieren. Ein impliziter Katalog berücksichtigt keine Elemente, die nicht als Teil von Benutzertransaktionen gemeldet wurden.

**Wie richte ich RECOMMENDATIONS erstmalig ein?**

Nach dem [Abonnieren](https://datamarket.azure.com/dataset/amla/recommendations) von RECOMMENDATIONS sollten Sie den Dienst mithilfe der API-Dokumentation in der Kurzanleitung [Azure Machine Learning Recommendations – Schnellstartanleitung](machine-learning-recommendation-api-quick-start-guide.md) einrichten.

**Wo finde ich die API-Dokumentation?** 

Die API-Dokumentation ist [Azure Machine Learning Recommendations – Schnellstartanleitung](machine-learning-recommendation-api-quick-start-guide.md).

**Welche Optionen habe ich, um Katalog- und Nutzungsdaten auf RECOMMENDATIONS hochzuladen?**

Zum Hochladen der Katalog- und Nutzungsdaten verfügen Sie über zwei Optionen. Entweder exportieren Sie diese Daten aus Ihrem CRM-System oder anderen Protokollen und laden sie in RECOMMENDATIONS hoch, oder Sie fügen Ihrer Website Markierungen hinzu, um die Benutzeraktivitäten zu verfolgen. Wenn Sie die zweite Methode verwenden, werden die Daten in Azure gespeichert.

## <a name="maintenance-and-support"></a>Wartung und Support
**Wie groß dürfen meine Datensätze sein?**

Jedes Dataset können bis zu 100.000 Katalogelemente enthalten und bis zu 2048 MB mit Nutzungsdaten.
Darüber hinaus kann ein Abonnement bis zu 10 Datensätze (Modelle) enthalten.

**Wo erhalte ich technischen Support für RECOMMENDATIONS?**

Der technische Support ist auf der Website des [Microsoft Azure-Supports](https://social.msdn.microsoft.com/forums/azure/home?forum=MachineLearning) verfügbar.

**Wo finde ich die Nutzungsbedingungen?**

[Microsoft Azure Machine Learning Recommendations API Terms of Service](https://datamarket.azure.com/dataset/amla/recommendations#terms)(in englischer Sprache).


