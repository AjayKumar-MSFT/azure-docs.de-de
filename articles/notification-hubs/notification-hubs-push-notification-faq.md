---
title: "Azure Notification Hubs - häufig gestellte Fragen (FAQs)"
description: "Häufig gestellte Fragen zum Entwerfen/Implementieren von Lösungen in Notification Hubs"
services: notification-hubs
documentationcenter: mobile
author: ysxu
manager: erikre
keywords: Pushbenachrichtigung, Pushbenachrichtigungen, iOS-Pushbenachrichtigungen, Android-Pushbenachrichtigungen, iOS-Push, Android-Push
editor: 
ms.assetid: 7b385713-ef3b-4f01-8b1f-ffe3690bbd40
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: multiple
ms.topic: article
ms.date: 01/19/2017
ms.author: yuaxu
translationtype: Human Translation
ms.sourcegitcommit: 4f2230ea0cc5b3e258a1a26a39e99433b04ffe18
ms.openlocfilehash: 9f0e7f071f1aa8fdd95c4959eae884afc60644e9
ms.lasthandoff: 03/25/2017


---
# <a name="push-notifications-with-azure-notification-hubs---frequently-asked-questions"></a>Pushbenachrichtigungen bei Azure Notification Hubs – häufig gestellte Fragen (FAQs)
## <a name="general"></a>Allgemein
### <a name="0--what-is-the-resource-structure-of-notification-hubs"></a>0.  Wie ist die Ressourcenstruktur von Notification Hubs aufgebaut?

Notification Hubs umfasst zwei Ressourcenebenen: Hubs und Namespaces. Ein Hub ist eine zentrale Ressource für Pushbenachrichtigungen, die plattformübergreifende Push-Informationen einer App enthalten kann. Ein Namespace ist eine Sammlung von Hubs in einem Bereich.

Allgemein wird empfohlen, einem Namespace eine App zuzuordnen. Ein Namespace kann einen Produktionshub enthalten, der mit Ihrer Produktions-App zusammenarbeitet, einen Test-Hub, der mit Ihrer Test-App zusammenarbeitet etc.

### <a name="1--what-is-the-price-model-for-notification-hubs"></a>1.  Welches Preismodell gilt für Notification Hubs?
Die neuesten Informationen finden Sie auf der Seite [Notification Hubs – Preise]. In Notification Hubs erfolgt die Abrechnung auf Namespace-Ebene (Informationen zur Definition eines Namespace finden Sie in der Ressourcenstruktur oben). Hierfür werden drei Tarife angeboten:

* **Free** – Dieser Tarif ist ein guter Ausgangspunkt, um Pushfunktionen zu testen, die nicht für Produktions-Apps empfohlen werden. Pro Namespace sind monatlich 500 Geräte und 1 Mio. Pushvorgänge ohne SLA-Garantie inbegriffen.
* **Basic** – Für kleinere Produktions-Apps wird dieser Tarif oder der Tarif „Standard“ empfohlen. Pro Namespace sind monatlich 200.000 Geräte und 10 Mio. Pushvorgänge Baseline, inklusive Optionen zur Steigerung des Kontingents, inbegriffen.
* **Standard** – Dieser Tarif wird für mittelgroße bis große Produktions-Apps empfohlen. Pro Namespace sind monatlich 10 Mio. Geräte und 10 Mio. Pushvorgänge Baseline, inklusive Optionen zur Steigerung des Kontingents, sowie umfangreiche Telemetriefunktionen inbegriffen.

Im Folgenden werden einige vorteilhafte Funktionen des Standard-Tarifs vorgestellt:
* *Umfangreiche Telemetrie* - Notification Hubs bietet Telemetrie auf Nachrichtenbasis, um Pushanforderungen und Feedback zum Plattformbenachrichtigungssystem zwecks Debugging nachzuverfolgen.
* *Mehrmandantenfähigkeit* – Sie können mit PNS-Anmeldeinformationen (Platform Notification System) auf Namespace-Ebene arbeiten. Hierdurch können Sie Mandanten im selben Namespace mühelos in Hubs unterteilen.
* *Geplanter Push* – Sie können planen, dass Benachrichtigungen zu einem beliebigen Zeitpunkt gesendet werden.

### <a name="2--what-is-the-notification-hubs-sla"></a>2.  Was ist die Notification Hubs-SLA?
Für die Notification Hubs-Tarife **Basic** und **Standard** wird garantiert, dass von ordnungsgemäß konfigurierten Anwendungen zu mindestens 99,9 % der Zeit Pushbenachrichtigungen gesendet oder Registrierungsverwaltungsvorgänge für ein Notification Hub in einem unterstützten Tarif durchgeführt werden können. Weitere Informationen über die Vereinbarung zum Servicelevel finden Sie auf der Seite [SLA für Notification Hubs](https://azure.microsoft.com/support/legal/sla/notification-hubs/) .

> [!NOTE]
> Da Pushbenachrichtigungen von Plattformbenachrichtigungssystemen von Drittanbietern (APNS von Apple, FCM von Google etc.) abhängig sind, wird keine SLA-Garantie für die Übermittlung derartiger Nachrichten gewährt. Nachdem Notification Hubs die Sendevorgänge batchweise an die PNS geleitet hat (mit SLA-Garantie), obliegt die Übermittlung der Pushvorgänge den PNS (ohne SLA-Garantie).

### <a name="3--which-customers-are-using-notification-hubs"></a>3.  Welche Kunden verwenden Notification Hubs?
Es gibt zahlreiche Kunden, die Notification Hubs verwenden, darunter beispielsweise:

* Sochi 2014 – Hunderte von Interessengruppen, über 3 Millionen Geräte, mehr als 150 Millionen Benachrichtigungen in 2 Wochen gesendet. [Fallstudie - Sochi]
* Skanska – [Fallstudie – Skanska]
* Seattle Times – [Fallstudie – Seattle Times]
* Mural.ly – [Fallstudie – Mural.ly]
* 7Digital – [Fallstudie – 7Digital]
* Bing Apps – 10 Millionen Geräte, 3 Millionen Benachrichtigungen pro Tag gesendet

### <a name="4-how-do-i-upgrade-or-downgrade-my-hub-or-namespace-to-a-different-tier"></a>4. Wie stufe ich meinen Hub oder meinen Namespace auf einen anderen Tarif hoch oder herab?
Navigieren Sie zum [Azure-Portal] und dann zu „Notification Hubs-Namespaces“ oder „Notification Hubs“. Klicken Sie auf die Ressource, die aktualisiert werden soll, und navigieren Sie in der Navigation zu „Tarif“. Sie können eine Aktualisierung auf einen beliebigen Tarif durchführen. Dabei sind einige Hinweise zu beachten:
* Der aktualisierte Tarif gilt für *alle* Hubs in dem Namespace, mit dem Sie arbeiten.
* Wenn Sie eine Herabstufung durchführen und Ihre Geräteanzahl das Limit des Tarifs, auf den Sie eine Herabstufung durchführen möchten, überschreiten, müssen Sie vor der Herabstufung Geräte löschen, um das Limit einzuhalten.


## <a name="design--development"></a>Entwurf und Entwicklung
### <a name="1--which-server-side-platforms-do-you-support"></a>1.  Welche dienstseitigen Plattformen werden unterstützt?
Wir stellen Server-SDKs für .NET, Java, Node.js, PHP und Python bereit. Darüber hinaus basieren die APIs von Notification Hubs auf REST-Schnittstellen, sodass Sie direkt mit REST-APIs arbeiten können, wenn Sie andere Plattformen verwenden oder weitere Abhängigkeiten vermeiden möchten. Weitere Einzelheiten finden Sie auf der Seite [NH – REST-APIs].

### <a name="2--which-client-platforms-do-you-support"></a>2.  Welche Clientplattformen werden unterstützt?
Pushbenachrichtigungen können an [iOS](notification-hubs-ios-apple-push-notification-apns-get-started.md), [Android](notification-hubs-android-push-notification-google-gcm-get-started.md), [Windows Universal](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md), [Windows Phone](notification-hubs-windows-mobile-push-notifications-mpns.md), [Kindle](notification-hubs-kindle-amazon-adm-push-notification.md), [Android China (via Baidu)](notification-hubs-baidu-china-android-notifications-get-started.md), Xamarin ([iOS](xamarin-notification-hubs-ios-push-notification-apns-get-started.md) & [Android](xamarin-notification-hubs-push-notifications-android-gcm.md)), [Chrome Apps](notification-hubs-chrome-push-notifications-get-started.md) und [Safari](https://github.com/Azure/azure-notificationhubs-samples/tree/master/PushToSafari) gesendet werden. Weitere Informationen finden Sie auf unserer Seite [Einführungen].

### <a name="3--do-you-support-smsemailweb-notifications"></a>3.  Werden Benachrichtigungen per SMS/E-Mail/Web unterstützt?
Notification Hubs wurde primär zum Senden von Benachrichtigungen an mobile Apps entwickelt und stellt keine Funktionen zum Senden von E-Mails oder SMS bereit. Es können jedoch zusätzlich zu Notification Hubs Drittanbieterplattformen mit dieser Funktionalität integriert werden, um systemeigene Pushbenachrichtigungen unter Verwendung von [Azure Mobile Apps] zu senden.

Notification Hubs stellt auch kein sofort einsatzfähiges Zustellverfahren für Browserpushbenachrichtigungen bereit. Kunden können dies implementieren, indem Sie zusätzlich zu den unterstützten serverseitigen Plattformen SignalR verwenden. Wenn Sie Benachrichtigungen an Browser-Apps in der Chrome Sandbox senden möchten, sehen Sie sich das Tutorial [Erste Schritte mit Notification Hubs für Chrome-Apps]an.

### <a name="4----what-is-the-relation-between-azure-mobile-apps-and-azure-notification-hubs-and-when-do-i-use-what"></a>4.    Welche Beziehung besteht zwischen Azure Mobile Apps und Azure Notification Hubs, und wann verwende ich was?
Wenn Sie über ein vorhandenes Mobile Apps-Back-End verfügen und nur die Funktionalität zum Senden von Pushbenachrichtigungen hinzufügen möchten, können Sie Azure Notification Hubs verwenden. Wenn Sie ein Mobile Apps-Back-End von Grund auf erstellen möchten, sollten Sie die Verwendung von Azure Mobile Apps erwägen. Azure Mobile Apps stellt automatisch einen Notification Hub für Sie bereit, damit Sie über das Mobile Apps-Back-End problemlos Pushbenachrichtigungen senden können. Die Preise für Azure Mobile Apps schließen die Grundgebühren für Notification Hubs ein, und Kosten fallen nur dann an, wenn Sie die eingeschlossene Zahl an Pushvorgängen überschreiten. Weitere Informationen zu den Kosten finden Sie auf der Seite [App Service – Preise] .

### <a name="5----how-many-devices-can-i-support-if-i-send-push-notifications-via-notification-hubs"></a>5.    Wie viele Geräte kann ich unterstützen, wenn ich Pushbenachrichtigungen über Notification Hubs sende?
Auf der Seite [Notification Hubs – Preise] finden Sie ausführliche Informationen zur Anzahl der unterstützten Geräte.

Wenn Sie für bestimmte Szenarios mehr als 10.000.000 registrierte Geräte benötigen, nehmen Sie mit uns [Kontakt](https://azure.microsoft.com/overview/contact-us/) auf, damit wir Ihnen beim Skalieren Ihrer Lösung helfen können.

### <a name="6----how-many-push-notifications-can-i-send-out"></a>6.    Wie viele Pushbenachrichtigungen kann ich senden?
Je nach dem ausgewählten Tarif erfolgt die Skalierung automatisch basierend auf der Anzahl von Benachrichtigungen, die über das System verarbeitet werden.

> [!NOTE]
> Die Gesamtkosten können je nach der Anzahl der verarbeiteten Pushbenachrichtigungen steigen. Informieren Sie sich deshalb unbedingt auf der Seite [Notification Hubs – Preise] über die bestehenden Limits für die einzelnen Tarife.
> 
> 

Unsere Kunden senden mit Notification Hubs Millionen von Pushbenachrichtigungen pro Tag. Wenn Sie Azure Notification Hubs verwenden, müssen Sie sich nicht um die Skalierung der Reichweite Ihrer Pushbenachrichtigungen kümmern.

### <a name="7----how-long-does-it-take-for-sent-push-notifications-to-reach-my-device"></a>7.    Wie lange dauert das Senden der Pushbenachrichtigungen auf mein Gerät?
Azure Notification Hubs kann in einem normalen Benutzerszenario mit konstanter Eingangslast ohne nennenswerte Spitzen mindestens **1 Millionen Sendevorgänge pro Minute** verarbeiten. Diese Rate kann abhängig von der Anzahl von Tags, der Art der eingehenden Sendevorgänge und anderer externer Faktoren variieren.

Während der geschützten Zustellzeit kann der Dienst die Ziele pro Plattform und die Weiterleitung von Nachrichten an die entsprechenden PNS (Push Notification Services) basierend auf den registrierten Tags/Tag-Ausdrücken berechnen. Ab dort liegt die Verantwortung zur Übermittlung der Benachrichtigung an das Gerät beim PNS (Push Notification Service).

Ein PNS garantiert keine SLA für die Übermittlung von Benachrichtigungen. Der größte Teil der Pushbenachrichtigungen wird jedoch innerhalb von wenigen Minuten (meist unter 10 Minuten) nach dem Senden an die Plattform zugestellt. Es kann wenige Ausnahmen geben, bei denen die Übermittlung mehr Zeit in Anspruch nimmt.

> [!NOTE]
> Azure Notification Hubs verfügt über eine Richtlinie, nach der Pushbenachrichtigungen verworfen werden, wenn sie nicht innerhalb von 30 Minuten an den PNS übermittelt werden können. Diese Verzögerung kann aus verschiedenen Gründen auftreten, meistens jedoch, weil der PNS Ihre Anwendung drosselt.
> 
> 

### <a name="8----is-there-any-latency-guarantee"></a>8.    Gibt es garantierte Latenzzeiten?
Aufgrund der Tatsache, dass Pushbenachrichtigungen von einem externen, plattformspezifischen PNS übermittelt werden, können keine Latenzzeiten garantiert werden. Die Mehrzahl der Pushbenachrichtigungen wird jedoch innerhalb weniger Minuten zugestellt.

### <a name="9----what-are-the-considerations-i-need-to-take-into-account-when-designing-a-solution-with-namespaces-and-notification-hubs"></a>9.    Welche Aspekte müssen beim Entwurf einer Lösung mit Namespaces und Notification Hubs berücksichtigt werden?
#### <a name="mobile-appenvironment"></a>Mobile Apps/Umgebung
* Es sollte ein Notification Hub pro mobiler App und Umgebung verfügbar sein.
* In einem mehrinstanzenfähigen Szenario sollte jeder Mandant über einen separaten Hub verfügen.
* Ein Notification Hub darf niemals von einer Test- und einer Produktionsumgebung gemeinsam genutzt werden, da dies zu Problemen beim Senden von Benachrichtigungen führen kann. Apple bietet beispielsweise Pushendpunkte für Sandbox und Produktion an, die jeweils über separate Anmeldeinformationen verfügen.
* Standardmäßig können Sie über das Azure-Portal oder die integrierte Azure-Komponente in Visual Studio Testbenachrichtigungen an registrierte Geräte senden. Der Schwellenwert wird auf 10 Geräte festgelegt, die nach dem Zufallsprinzip aus dem Registrierungspool ausgewählt werden.

> [!NOTE]
> Wenn der Hub ursprünglich mit einem Apple-Sandbox-Zertifikat konfiguriert wurde und anschließend zur Verwendung eines Apple-Produktionszertifikats neu konfiguriert wird, verfällt das vorherige Gerätetoken durch das neue Zertifikat, und es kommt zu Fehlern bei Pushvorgängen. Sie sollten Ihre Produktions- und Testumgebung immer getrennt voneinander verwalten und unterschiedliche Hubs für unterschiedliche Umgebungen verwenden.
> 
> 

#### <a name="pns-credentials"></a>PNS-Anmeldeinformationen
Wenn eine mobile App für das Entwicklerportal einer Plattform registriert wird (z. B. Apple oder Google usw.), erhalten Sie eine App-ID und Sicherheitstoken, die ein App-Back-End für den PNS der Plattform benötigen, um Pushbenachrichtigungen an die Geräte zu senden. Diese Sicherheitstoken, die in Form von Zertifikaten (beispielsweise für Apple iOS oder Windows Phone) oder als Sicherheitsschlüssel (Google Android, Windows usw.) ausgestellt werden, müssen in Notification Hubs konfiguriert werden. Die Konfiguration erfolgt typischerweise auf Notification Hub-Ebene, kann jedoch in einem Szenario mit Mehrinstanzenfähigkeit auch auf Namespaceebene durchgeführt werden.

#### <a name="namespaces"></a>Namespaces
Namespaces können für eine Bereitstellungsgruppierung eingesetzt werden.  Sie können auch alle Notification Hubs für alle Mandanten derselben App in einem Szenario mit Mehrinstanzenfähigkeit repräsentieren.

#### <a name="geo-distribution"></a>Geografische Verteilung
Die geografische Verteilung ist bei Szenarios mit Pushbenachrichtigungen nicht immer ein wichtiger Aspekt. Die verschiedenen Push Notification Services (beispielsweise APNS, GCM usw.), die für die Zustellung der Pushbenachrichtigungen an die Geräte sorgen, sind häufig selbst nicht gleichmäßig verteilt.

Wenn Sie über eine Anwendung verfügen, die weltweit eingesetzt wird, können Sie mehrere Hubs in verschiedenen Namespaces erstellen, um die Verfügbarkeit des Notification Hubs-Diensts in verschiedenen Azure-Regionen rund um den Globus zu nutzen.

> [!NOTE]
> Dies erhöht die Verwaltungskosten, insbesondere im Hinblick auf die Registrierungen. Deshalb wird diese Konfiguration nicht ausdrücklich empfohlen und sollte nur verwendet werden, wenn dies unbedingt erforderlich ist.
> 
> 

### <a name="10----should-i-do-registrations-from-the-app-backend-or-directly-through-client-devices"></a>10.    Sollten Registrierungen vom App-Back-End oder direkt über Clientgeräte erfolgen?
Registrierungen über das App-Back-End sind sinnvoll, wenn Sie eine Clientauthentifizierung durchführen müssen, bevor Sie die Registrierung erstellen, oder wenn Sie über Tags verfügen, die vom App-Back-End basierend auf einer bestimmten Anwendungslogik durch die App erstellt oder geändert werden müssen. Weitere Informationen finden Sie im [Leitfaden zur Back-End-Registrierung] und im [Leitfaden zur Back-End-Registrierung – 2].

### <a name="11----what-is-the-push-notification-delivery-security-model"></a>11.    Welches Sicherheitsmodell gilt für die Übermittlung von Pushbenachrichtigungen?
Azure Notification Hubs verwendet ein [SAS-basiertes (Shared Access Signature)](../storage/storage-dotnet-shared-access-signature-part-1.md) Sicherheitsmodell. Sie können die SAS-Token auf Namespace-Stammebene oder auf der granularen Notification Hubs-Ebene verwenden. Diese SAS-Token können mit verschiedenen Authentifizierungsregeln festgelegt werden, z. B. Berechtigungen zum Senden von Nachrichten oder Berechtigungen zum Überwachen auf Benachrichtigungen usw. Weitere Einzelheiten finden Sie im Dokument zum [Notification Hubs – Sicherheitsmodell].

### <a name="12----how-should-i-handle-sensitive-payload-in-the-push-notifications"></a>12.    Wie sollte ich sensible Daten in Pushbenachrichtigungen behandeln?
Alle Benachrichtigungen werden über den Push Notification Service (PNS) der Plattform an die Zielgeräte übermittelt. Wenn ein Absender eine Benachrichtigung an Azure Notification Hubs sendet, werden diese Benachrichtigungen verarbeitet und an den jeweiligen PNS weitergeleitet.

Alle Verbindungen zwischen Absender und Azure Notification Hubs und zum PNS verwenden HTTPS.

> [!NOTE]
> Azure Notification Hubs führt keine Protokollierung der Nachrichteninhalte durch.
> 
> 

Für das Senden vertraulicher Daten wird jedoch der Einsatz eines sicheren Pushmusters empfohlen, bei dem der Absender eine „Ping“-Benachrichtigung ohne die vertraulichen Daten mit einer Nachrichten-ID an das Gerät sendet. Wenn die App auf dem Gerät diese Daten empfängt, erfolgt ein direkter Aufruf einer sicheren API, mit der die Nachrichtendetails angefordert werden. Eine Anleitung zum Implementieren des oben beschriebenen Musters finden Sie auf der Seite [Azure Notification Hubs – Sichere Pushbenachrichtigungen].

## <a name="operations"></a>Vorgänge
### <a name="1----what-is-the-disaster-recovery-dr-story"></a>1.    Wie wird eine Notfallwiederherstellung gewährleistet?
Wir bieten eine Notfallwiederherstellung der Metadaten (Notification Hub-Name, Verbindungszeichenfolge usw.). Wenn eine Notfallwiederherstellung ausgelöst wird, gehen **nur** die Registrierungsdaten der Notification Hubs-Infrastruktur verloren. Sie müssen eine Lösung implementieren, um Ihren neuen Hub nach der Wiederherstellung wieder mit diesen Daten aufzufüllen.

* *Schritt 1:* Erstellen Sie einen sekundären Notification Hub in einem anderen Rechenzentrum. Sie können den sekundären Notification Hub im Falle einer Notfallwiederherstellung oder bereits im Vorfeld erstellen. Welche Option Sie auswählen, macht keinen großen Unterschied, da die Bereitstellung von Notification Hubs relativ schnell abläuft und meist nur Sekunden dauert. Es wird jedoch dringend empfohlen, bereits im Vorfeld einen sekundären Notification Hub zu erstellen, da so Auswirkungen der Notfallwiederherstellung auf Verwaltungsfunktionen ausgeschlossen werden.
* *Schritt 2* – Stellen Sie die Registrierungen aus dem primären Notification Hub im sekundären Notification Hub bereit. Es wird nicht empfohlen, Registrierungen auf beiden Hubs zu verwalten und diese zu synchronisieren, wenn Registrierungen eingehen. Dies funktioniert in der Regel nicht, da Registrierungen auf PNS-Seite häufig mit einem Ablaufdatum versehen werden. Notification Hubs bereinigt Registrierungen, wenn PNS-Informationen zu abgelaufenen oder ungültigen Registrierungen empfangen werden.  

Es wird empfohlen, ein App-Back-End zu verwenden, das folgende Aufgaben erfüllt:

* Verwaltung der Registrierungen, sodass bei einer Notfallwiederherstellung ein Masseneinfügevorgang in den sekundären Notification Hub durchgeführt werden kann,

**OR**

* Regelmäßiger Abruf einer Sicherungskopie der Registrierungen aus dem primären Hub als Datensicherung und anschließendes Einfügen der Registrierungen auf dem sekundären Notification Hub (per Massenvorgang).

> [!NOTE]
> Informationen zur Funktion für den Export/Import von Registrierungen im Tarif „Standard“ finden Sie unter [Vorgehensweise: Massenhaftes Exportieren und Ändern von Registrierungen] .
> 
> 

Wenn Sie kein Back-End haben, führt die App beim Start auf einem Zielgerät eine neue Registrierung im sekundären Notification Hub durch. Der sekundäre Notification Hub verfügt dann über Registrierungen aller aktiven Geräte.

Der Nachteil hiervon ist, dass es vorkommen kann, dass Geräte, auf denen keine Apps geöffnet wurden, keine Benachrichtigungen erhalten.

### <a name="2----is-there-any-audit-log-capability"></a>2.    Steht ein Überwachungsprotokoll zur Verfügung?
Alle Notification Hubs-Verwaltungsvorgänge werden in Vorgangsprotokollen aufgezeichnet, die im [klassischen Azure-Portal]zur Verfügung stehen.

## <a name="monitoring--troubleshooting"></a>Überwachung und Problembehandlung
### <a name="1----what-troubleshooting-capabilities-are-available"></a>1.    Welche Funktionen für die Problembehandlung stehen zur Verfügung?
Azure Notification Hubs bietet verschiedene Funktionen für die gängige Problembehandlung, insbesondere für die häufigsten Szenarios im Zusammenhang mit verworfenen Benachrichtigungen. Weitere Einzelheiten finden Sie im Whitepaper [Azure Notification Hubs – Diagnoserichtlinien] .

### <a name="2----what-telemetry-features-are-available"></a>2.    Welche Telemetriefunktionen stehen zur Verfügung?
Azure Notification Hubs ermöglicht die Anzeige von Telemetriedaten im [klassischen Azure-Portal]. Detailinformationen zu den verfügbaren Metriken finden Sie auf der Seite [Metriken] .

> [!NOTE]
> Erfolgreiche Benachrichtigungen bedeuten lediglich, dass Pushbenachrichtigungen an den externen Pushbenachrichtigungsdienst (z. B. APNS für Apple, GCM für Google usw.) übermittelt wurden. Der PNS ist dafür verantwortlich, die Benachrichtigung an die Zielgeräte zu übermitteln. In der Regel veröffentlicht der PNS Übermittlungsmetriken nicht für Dritte.  
> 
> 

Wir bieten außerdem die Möglichkeit, Telemetriedaten programmgesteuert zu exportieren (im Tarif **Standard** ). Einzelheiten finden Sie im Beispiel unter [NH – Metrics sample] .

[klassischen Azure-Portal]: https://manage.windowsazure.com
[Notification Hubs – Preise]: http://azure.microsoft.com/pricing/details/notification-hubs/
[Notification Hubs SLA]: http://azure.microsoft.com/support/legal/sla/
[Fallstudie - Sochi]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=7942
[Fallstudie – Skanska]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=5847
[Fallstudie – Seattle Times]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=8354
[Fallstudie – Mural.ly]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=11592
[Fallstudie – 7Digital]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=3684
[NH – REST-APIs]: https://msdn.microsoft.com/library/azure/dn530746.aspx
[NH - Getting Started Tutorials]: http://azure.microsoft.com/documentation/articles/notification-hubs-ios-get-started/
[Erste Schritte mit Notification Hubs für Chrome-Apps]: http://azure.microsoft.com/documentation/articles/notification-hubs-chrome-get-started/
[Mobile Services Pricing]: http://azure.microsoft.com/pricing/details/mobile-services/
[Leitfaden zur Back-End-Registrierung]: https://msdn.microsoft.com/library/azure/dn743807.aspx
[Leitfaden zur Back-End-Registrierung – 2]: https://msdn.microsoft.com/library/azure/dn530747.aspx
[Notification Hubs – Sicherheitsmodell]: https://msdn.microsoft.com/library/azure/dn495373.aspx
[Azure Notification Hubs – Sichere Pushbenachrichtigungen]: http://azure.microsoft.com/documentation/articles/notification-hubs-aspnet-backend-ios-secure-push/
[Azure Notification Hubs – Diagnoserichtlinien]: http://azure.microsoft.com/documentation/articles/notification-hubs-diagnosing/
[Metriken]: https://msdn.microsoft.com/library/dn458822.aspx
[NH – Metrics sample]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/FetchNHTelemetryInExcel
[Vorgehensweise: Massenhaftes Exportieren und Ändern von Registrierungen]: https://msdn.microsoft.com/library/dn790624.aspx
[Azure-Portal]: https://portal.azure.com
[complete samples]: https://github.com/Azure/azure-notificationhubs-samples
[Azure Mobile Apps]: https://azure.microsoft.com/services/app-service/mobile/
[App Service – Preise]: https://azure.microsoft.com/pricing/details/app-service/

