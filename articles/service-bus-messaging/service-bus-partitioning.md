---
title: Erstellen von Azure Service Bus-Warteschlangen und -Themen | Microsoft-Dokumentation
description: Es wird beschrieben, wie Service Bus-Warteschlangen und -Themen mit mehreren Nachrichtenbrokern partitioniert werden.
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: a0c7d5a2-4876-42cb-8344-a1fc988746e7
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/28/2017
ms.author: sethm;hillaryc
translationtype: Human Translation
ms.sourcegitcommit: b4802009a8512cb4dcb49602545c7a31969e0a25
ms.openlocfilehash: 1952adece7e874ade97c2a8480455e1236bb74f1
ms.lasthandoff: 03/29/2017


---
# <a name="partitioned-queues-and-topics"></a>Partitionierte Warteschlangen und Themen
Für Azure Service Bus werden mehrere Nachrichtenbroker verwendet, um Nachrichten zu verarbeiten, sowie mehrere Nachrichtenspeicher, um Nachrichten zu speichern. Eine herkömmliche Warteschlange oder ein Thema werden von einem einzelnen Nachrichtenbroker verarbeitet und in einem Nachrichtenspeicher gespeichert. Service Bus-*Partitionen* ermöglichen das Partitionieren von Warteschlangen oder Themen über mehrere Nachrichtenbroker und -speicher. Dies bedeutet, dass der Gesamtdurchsatz einer partitionierten Warteschlange oder eines Themas nicht mehr durch die Leistung eines einzelnen Nachrichtenbrokers oder Nachrichtenspeichers beschränkt wird. Außerdem führt ein vorübergehender Ausfall eines Nachrichtenspeichers nicht dazu, dass eine partitionierte Warteschlange oder ein Thema nicht verfügbar ist. Partitionierte Warteschlangen und Themen können alle erweiterten Service Bus-Features enthalten, z. B. die Unterstützung von Transaktionen und Sitzungen.

Weitere Informationen zum inneren Aufbau von Service Bus finden Sie im Artikel [Service Bus-Architektur][Service Bus architecture].

## <a name="how-it-works"></a>So funktioniert's
Jede partitionierte Warteschlange bzw. jedes Thema besteht aus mehreren Fragmenten. Jedes Fragment wird in einem anderen Nachrichtenspeicher gespeichert und von einem anderen Nachrichtenbroker verarbeitet. Wenn eine Nachricht an eine partitionierte Warteschlange bzw. ein Thema gesendet wird, weist Service Bus die Nachricht einem der Fragmente zu. Service Bus nimmt die Auswahl willkürlich oder mithilfe eines Partitionsschlüssels vor, den der Absender angeben kann.

Wenn ein Client eine Nachricht von einer partitionierten Warteschlange oder von einem Abonnement eines partitionierten Themas empfangen möchte, fragt Service Bus alle Fragmente auf Nachrichten ab. Anschließend wird die erste Nachricht zurückgegeben, die von einem der Nachrichtenspeicher an den Empfänger gesendet wird. Service Bus speichert die anderen Nachrichten zwischen und gibt sie zurück, wenn zusätzliche Empfangsanforderungen eingehen. Ein empfangender Client ist sich der Partitionierung nicht bewusst. Das Verhalten einer partitionierten Warteschlange oder eines Themas (z.B. „read“, „complete“, „defer“, „deadletter“, „prefetching“) dem Client gegenüber ist mit dem Verhalten einer normalen Entität identisch.

Es fallen keine zusätzlichen Kosten an, wenn eine Nachricht an eine partitionierte Warteschlange oder ein Thema gesendet oder von dort empfangen wird.

## <a name="enable-partitioning"></a>Aktivieren der Partitionierung
Setzen Sie zum Verwenden von partitionierten Warteschlangen und Themen mit Azure Service Bus die Azure SDK-Version 2.2 oder höher ein, oder geben Sie `api-version=2013-10` in Ihren HTTP-Anforderungen an.

Sie können Service Bus-Warteschlangen und -Themen in Größen von 1, 2, 3, 4 oder 5GB erstellen (die Standardgröße ist 1GB). Bei aktivierter Partitionierung erstellt Service Bus 16 Partitionen für jedes angegebene GB. Wenn Sie also eine Warteschlange mit einer Größe von 5 GB erstellen, beträgt die maximale Warteschlangengröße bei 16 Partitionen 5 \* 16 = 80 GB. Die maximale Größe der partitionierten Warteschlange oder des Themas wird im zugehörigen Eintrag im [Azure-Portal][Azure portal] angezeigt.

Es gibt mehrere Möglichkeiten, eine partitionierte Warteschlange oder ein partitioniertes Thema zu erstellen. Wenn Sie die Warteschlange oder das Thema über Ihre Anwendung erstellen, können Sie die Partitionierung für die Warteschlange oder das Thema aktivieren, indem Sie die [QueueDescription.EnablePartitioning][QueueDescription.EnablePartitioning]- bzw. [TopicDescription.EnablePartitioning-Eigenschaft][TopicDescription.EnablePartitioning] auf **TRUE** festlegen. Diese Eigenschaften müssen bei der Erstellung der Warteschlange oder des Themas festgelegt werden. Es ist nicht möglich, diese Eigenschaften für eine vorhandene Warteschlange oder ein Thema zu ändern. Beispiel:

```csharp
// Create partitioned topic
NamespaceManager ns = NamespaceManager.CreateFromConnectionString(myConnectionString);
TopicDescription td = new TopicDescription(TopicName);
td.EnablePartitioning = true;
ns.CreateTopic(td);
```

Alternativ können Sie eine partitionierte Warteschlange oder ein Thema in Visual Studio oder im [Azure-Portal][Azure portal] erstellen. Wenn Sie eine Warteschlange oder ein Thema im Portal erstellen, legen Sie die Option **Partitionierung aktivieren** auf dem Blatt **Allgemeine Einstellungen** des Warteschlangen- oder Themenfensters **Einstellungen** auf **TRUE** fest. Klicken Sie in Visual Studio im Dialogfeld **Neue Warteschlange** bzw. **Neues Thema** auf das Kontrollkästchen **Partitionierung aktivieren**.

## <a name="use-of-partition-keys"></a>Verwenden von Partitionsschlüsseln
Wenn eine Nachricht in eine partitionierte Warteschlange oder ein Thema eingereiht wird, wird von Service Bus das Vorhandensein eines Partitionsschlüssels überprüft. Wenn ein Schlüssel gefunden wird, wird das Fragment basierend auf diesem Schlüssel ausgewählt. Falls kein Partitionsschlüssel gefunden wird, wird das Fragment basierend auf einem internen Algorithmus ausgewählt.

### <a name="using-a-partition-key"></a>Verwendung eines Partitionsschlüssels
Einige Szenarien, z. B. Sitzungen oder Transaktionen, erfordern es, dass Nachrichten in einem bestimmten Fragment gespeichert werden. Für all diese Szenarien ist die Verwendung eines Partitionsschlüssels erforderlich. Alle Nachrichten, für die der gleiche Partitionsschlüssel verwendet wird, sind demselben Fragment zugewiesen. Wenn das Fragment vorübergehend nicht verfügbar ist, wird von Service Bus ein Fehler zurückgegeben.

Je nach Szenario werden verschiedene Nachrichteneigenschaften als Partitionsschlüssel verwendet:

**SessionId:** Wenn für eine Nachricht die [BrokeredMessage.SessionId][BrokeredMessage.SessionId]-Eigenschaft festgelegt ist, verwendet Service Bus diese Eigenschaft als Partitionsschlüssel. Auf diese Weise werden alle Nachrichten, die zu derselben Sitzung gehören, von demselben Nachrichtenbroker verarbeitet. So kann Service Bus sowohl die Nachrichtensortierung als auch die Einheitlichkeit von Sitzungszuständen garantieren.

**PartitionKey:** Wenn für eine Nachricht die [BrokeredMessage.PartitionKey][BrokeredMessage.PartitionKey]-Eigenschaft festgelegt ist, aber nicht die [BrokeredMessage.SessionId][BrokeredMessage.SessionId]-Eigenschaft, verwendet Service Bus die [PartitionKey][PartitionKey]-Eigenschaft als Partitionsschlüssel. Wenn für die Nachricht sowohl die [SessionId][SessionId]- als auch die [PartitionKey][PartitionKey]-Eigenschaft festgelegt ist, müssen beide Eigenschaften identisch sein. Falls die [PartitionKey][PartitionKey]-Eigenschaft auf einen anderen Wert als die [SessionId][SessionId]-Eigenschaft festgelegt ist, gibt Service Bus eine Ausnahme für einen ungültigen Vorgang zurück. Die [PartitionKey][PartitionKey]-Eigenschaft sollte verwendet werden, wenn ein Absender nicht sitzungsorientierte Transaktionsnachrichten sendet. Mit dem Partitionsschlüssel wird sichergestellt, dass alle Nachrichten, die innerhalb einer Transaktion gesendet werden, von demselben Nachrichtenbroker verarbeitet werden.

**MessageId:** Wenn für die Warteschlange bzw. das Thema die [QueueDescription.RequiresDuplicateDetection][QueueDescription.RequiresDuplicateDetection]-Eigenschaft auf **TRUE** festgelegt ist und die [BrokeredMessage.SessionId][BrokeredMessage.SessionId]- oder die [BrokeredMessage.PartitionKey][BrokeredMessage.PartitionKey]-Eigenschaft nicht festgelegt wurde, dient die [BrokeredMessage.MessageId][BrokeredMessage.MessageId]-Eigenschaft als Partitionsschlüssel. (Beachten Sie, dass die Microsoft .NET- und AMQP-Bibliotheken automatisch eine Nachrichten-ID zuweisen, wenn dies von der sendenden Anwendung nicht übernommen wird.) In diesem Fall werden alle Kopien einer Nachricht von demselben Nachrichtenbroker verarbeitet. So kann Service Bus doppelte Nachrichten erkennen und verwerfen. Wenn die [QueueDescription.RequiresDuplicateDetection][QueueDescription.RequiresDuplicateDetection]-Eigenschaft nicht auf **TRUE** festgelegt ist, sieht Service Bus die [MessageId][MessageId]-Eigenschaft nicht als Partitionsschlüssel an.

### <a name="not-using-a-partition-key"></a>Keine Verwendung eines Partitionsschlüssels
Wenn kein Partitionsschlüssel vorhanden ist, verteilt Service Bus Nachrichten gleichmäßig nacheinander auf alle Fragmente der partitionierten Warteschlange bzw. des Themas. Wenn das ausgewählte Fragment nicht verfügbar ist, weist Service Bus die Nachricht einem anderen Fragment zu. Auf diese Weise kann der Sendevorgang erfolgreich durchgeführt werden, obwohl ein Nachrichtenspeicher vorübergehend nicht verfügbar ist.

Um Service Bus ausreichend Zeit zum Einreihen der Nachricht in ein anderes Fragment zu geben, muss der Wert [MessagingFactorySettings.OperationTimeout][MessagingFactorySettings.OperationTimeout] des Clients, der die Nachricht sendet, größer als 15 Sekunden sein. Es wird empfohlen, die [OperationTimeout][OperationTimeout]-Eigenschaft auf den Standardwert von 60 Sekunden festzulegen.

Beachten Sie, dass ein Partitionsschlüssel eine Nachricht an ein bestimmtes Fragment „anheftet“. Wenn der Nachrichtenspeicher, der dieses Fragment enthält, nicht verfügbar ist, wird von Service Bus ein Fehler zurückgegeben. Wenn kein Partitionsschlüssel vorhanden ist, kann Service Bus ein anderes Fragment auswählen, damit der Vorgang erfolgreich ist. Aus diesem Grund wird empfohlen, nur dann einen Partitionsschlüssel anzugeben, wenn dies erforderlich ist.

## <a name="advanced-topics-use-transactions-with-partitioned-entities"></a>Weiterführende Themen: Verwenden von Transaktionen mit partitionierten Entitäten
Nachrichten, die als Teil einer Transaktion gesendet werden, müssen einen Partitionsschlüssel angeben. Dies kann eine der folgenden Eigenschaften sein: [BrokeredMessage.SessionId][BrokeredMessage.SessionId], [BrokeredMessage.PartitionKey][BrokeredMessage.PartitionKey] oder [BrokeredMessage.MessageId][BrokeredMessage.MessageId]. Für alle Nachrichten, die als Teil derselben Transaktion gesendet werden, muss derselbe Partitionsschlüssel angegeben werden. Wenn Sie versuchen, eine Nachricht innerhalb einer Transaktion ohne Partitionsschlüssel zu senden, wird von Service Bus eine Ausnahme für einen ungültigen Vorgang zurückgegeben. Wenn Sie versuchen, mehrere Nachrichten mit verschiedenen Partitionsschlüsseln innerhalb derselben Transaktion zu senden, gibt Service Bus eine Ausnahme für einen ungültigen Vorgang zurück. Beispiel:

```csharp
CommittableTransaction committableTransaction = new CommittableTransaction();
using (TransactionScope ts = new TransactionScope(committableTransaction))
{
    BrokeredMessage msg = new BrokeredMessage("This is a message");
    msg.PartitionKey = "myPartitionKey";
    messageSender.Send(msg); 
    ts.Complete();
}
committableTransaction.Commit();
```

Wenn Eigenschaften, die als Partitionsschlüssel dienen, festgelegt werden, heftet Service Bus die Nachricht an ein bestimmtes Fragment an. Dieses Verhalten tritt unabhängig davon auf, ob eine Transaktion verwendet wird. Es wird empfohlen, keinen Partitionsschlüssel anzugeben, wenn dies nicht erforderlich ist.

## <a name="using-sessions-with-partitioned-entities"></a>Verwenden von Sitzungen mit partitionierten Entitäten
Um eine Transaktionsnachricht an ein sitzungsorientiertes Thema bzw. eine Warteschlange zu senden, muss für die Nachricht die [BrokeredMessage.SessionId][BrokeredMessage.SessionId]-Eigenschaft festgelegt sein. Wenn die [BrokeredMessage.PartitionKey][BrokeredMessage.PartitionKey]-Eigenschaft ebenfalls angegeben wird, muss diese mit der [SessionId][SessionId]-Eigenschaft identisch sein. Falls sie sich unterscheiden, gibt Service Bus eine Ausnahme für einen ungültigen Vorgang aus.

Im Gegensatz zu normalen (nicht partitionierten) Warteschlangen oder Themen ist es nicht möglich, eine einzelne Transaktion zu verwenden, um mehrere Nachrichten an verschiedene Sitzungen zu senden. Bei einem entsprechenden Versuch gibt Service Bus eine Ausnahme für einen ungültigen Vorgang aus. Beispiel:

```csharp
CommittableTransaction committableTransaction = new CommittableTransaction();
using (TransactionScope ts = new TransactionScope(committableTransaction))
{
    BrokeredMessage msg = new BrokeredMessage("This is a message");
    msg.SessionId = "mySession";
    messageSender.Send(msg); 
    ts.Complete();
}
committableTransaction.Commit();
```

## <a name="automatic-message-forwarding-with-partitioned-entities"></a>Automatische Nachrichtenweiterleitung mit partitionierten Entitäten
Service Bus unterstützt die automatische Nachrichtenweiterleitung von und zwischen partitionierten Entitäten sowie an diese. Legen Sie zum Aktivieren der automatischen Nachrichtenweiterleitung die [QueueDescription.ForwardTo][QueueDescription.ForwardTo]-Eigenschaft für die Quellwarteschlange oder das Abonnement fest. Wenn für die Nachricht ein Partitionsschlüssel ([SessionId][SessionId], [PartitionKey][PartitionKey] oder [MessageId][MessageId]) angegeben ist, wird er für die Zielentität verwendet.

## <a name="considerations-and-guidelines"></a>Aspekte und Richtlinien
* **Features für hohe Konsistenz**: Wenn eine Entität Features wie Sitzungen, Duplikaterkennung oder die explizite Steuerung eines Partitionierungsschlüssels verwendet, werden die Messagingvorgänge immer an bestimmte Fragmente geleitet. Wenn für ein Fragment hoher Datenverkehr auftritt oder der zugrunde liegende Speicher fehlerhaft ist, schlagen diese Vorgänge fehl, und die Verfügbarkeit wird reduziert. Insgesamt ist die Konsistenz trotzdem deutlich höher als bei nicht partitionierten Entitäten: Nur für eine Teilmenge des Datenverkehrs treten Probleme auf, und nicht für den gesamten Datenverkehr.
* **Verwaltung**: Vorgänge wie das Erstellen, Aktualisieren und Löschen müssen für alle Fragmente der Entität durchgeführt werden. Wenn ein Fragment fehlerhaft ist, kann dies zu Fehlern bei diesen Vorgängen führen. Für den Get-Vorgang müssen Informationen aus allen Fragmenten aggregiert werden, z.B. die Nachrichtenanzahl. Wenn ein Fragment einen Fehler aufweist, wird der Verfügbarkeitsstatus der Entität als „Eingeschränkt“ gemeldet.
* **Nachrichtenszenarios mit geringem Volumen**: Für diese Szenarios, vor allem bei Verwendung des HTTP-Protokolls, müssen Sie ggf. mehrere Empfangsvorgänge durchführen, um alle Nachrichten zu erhalten. Für Empfangsanforderungen führt das Front-End den Empfangsvorgang auf allen Fragmenten durch und speichert alle empfangenen Antworten zwischen. Eine nachfolgende Empfangsanforderung derselben Verbindung profitiert von dieser Zwischenspeicherung, und die Latenzen für den Empfang sind niedriger. Wenn Sie über mehrere Verbindungen verfügen oder HTTP verwenden, wird für jede Anforderung eine neue Verbindung hergestellt. Es besteht also keine Garantie, dass sie auf demselben Knoten eintrifft. Wenn alle vorhandenen Nachrichten gesperrt sind und auf einem anderen Front-End zwischengespeichert werden, gibt der Empfangsvorgang **NULL** zurück. Nachrichten laufen nach einer gewissen Zeit ab und können von Ihnen dann erneut empfangen werden. Hierfür wird HTTP-Keep-Alive empfohlen.
* **Durchsuchen/Einsehen von Nachrichten:** [PeekBatch](/dotnet/api/microsoft.servicebus.messaging.queueclient#Microsoft_ServiceBus_Messaging_QueueClient_PeekBatch_System_Int32_) gibt nicht immer die Anzahl von Nachrichten zurück, die in der [MessageCount](/dotnet/api/microsoft.servicebus.messaging.queuedescription#Microsoft_ServiceBus_Messaging_QueueDescription_MessageCount)-Eigenschaft angegeben sind. Hierfür gibt es zwei häufige Ursachen. Ein Grund ist, dass die aggregierte Größe der Sammlung mit Nachrichten die maximale Größe von 256 KB übersteigt. Ein weiterer Grund ist: Wenn für die Warteschlange oder das Thema die [EnablePartitioning-Eigenschaft](/dotnet/api/microsoft.servicebus.messaging.queuedescription#Microsoft_ServiceBus_Messaging_QueueDescription_EnablePartitioning) auf **TRUE** festgelegt ist, verfügt eine Partition ggf. nicht über genügend Nachrichten, um die angeforderte Anzahl von Nachrichten abzuarbeiten. Wenn eine Anwendung eine bestimmte Anzahl von Nachrichten empfangen möchte, sollte sie im Allgemeinen mehrfach [PeekBatch](/dotnet/api/microsoft.servicebus.messaging.queueclient#Microsoft_ServiceBus_Messaging_QueueClient_PeekBatch_System_Int32_) aufrufen, bis die benötigte Anzahl von Nachrichten erreicht ist oder keine einzusehenden Nachrichten mehr vorhanden sind. Weitere Informationen, z.B. Codebeispiele, finden Sie unter [QueueClient.PeekBatch](/dotnet/api/microsoft.servicebus.messaging.queueclient#Microsoft_ServiceBus_Messaging_QueueClient_PeekBatch_System_Int32_) oder [SubscriptionClient.PeekBatch](/dotnet/api/microsoft.servicebus.messaging.subscriptionclient#Microsoft_ServiceBus_Messaging_SubscriptionClient_PeekBatch_System_Int32_).

## <a name="latest-added-features"></a>Neueste Funktionen
* Das Hinzufügen und Entfernen von Regeln wird jetzt für partitionierte Entitäten unterstützt. Diese Vorgänge werden nicht unter Transaktionen unterstützt, was einen Unterschied zu nicht partitionierten Entitäten darstellt. 
* Für AMQP wird jetzt das Senden und Empfangen von Nachrichten an bzw. von einer partitionierten Entität unterstützt.
* AMQP wird jetzt für die folgenden Vorgänge unterstützt: [Batchsendevorgang](/dotnet/api/microsoft.servicebus.messaging.queueclient#Microsoft_ServiceBus_Messaging_QueueClient_SendBatch_System_Collections_Generic_IEnumerable_Microsoft_ServiceBus_Messaging_BrokeredMessage__), [Batchempfangsvorgang](/dotnet/api/microsoft.servicebus.messaging.queueclient#Microsoft_ServiceBus_Messaging_QueueClient_ReceiveBatch_System_Int32_), [Nach Sequenznummer empfangen](/dotnet/api/microsoft.servicebus.messaging.queueclient#Microsoft_ServiceBus_Messaging_QueueClient_Receive_System_Int64_), [Einsehen](/dotnet/api/microsoft.servicebus.messaging.queueclient#Microsoft_ServiceBus_Messaging_QueueClient_Peek), [Sperre erneuern](/dotnet/api/microsoft.servicebus.messaging.queueclient#Microsoft_ServiceBus_Messaging_QueueClient_RenewMessageLock_System_Guid_), [Nachricht planen](/dotnet/api/microsoft.servicebus.messaging.queueclient#Microsoft_ServiceBus_Messaging_QueueClient_ScheduleMessageAsync_Microsoft_ServiceBus_Messaging_BrokeredMessage_System_DateTimeOffset_), [Geplante Nachricht abbrechen](/dotnet/api/microsoft.servicebus.messaging.queueclient#Microsoft_ServiceBus_Messaging_QueueClient_CancelScheduledMessageAsync_System_Int64_), [Regel hinzufügen](/dotnet/api/microsoft.servicebus.messaging.ruledescription), [Regel entfernen](/dotnet/api/microsoft.servicebus.messaging.ruledescription), [Sitzungssperre erneuern](/dotnet/api/microsoft.servicebus.messaging.messagesession#Microsoft_ServiceBus_Messaging_MessageSession_RenewLock), [Sitzungsstatus festlegen](/dotnet/api/microsoft.servicebus.messaging.messagesession#Microsoft_ServiceBus_Messaging_MessageSession_SetState_System_IO_Stream_), [Sitzungsstatus](/dotnet/api/microsoft.servicebus.messaging.messagesession#Microsoft_ServiceBus_Messaging_MessageSession_GetState) abrufen und [Sitzungen aufzählen](/dotnet/api/microsoft.servicebus.messaging.queueclient#Microsoft_ServiceBus_Messaging_QueueClient_GetMessageSessionsAsync).

## <a name="partitioned-entities-limitations"></a>Einschränkungen partitionierter Entitäten
Derzeit gelten bei Service Bus die folgenden Einschränkungen für partitionierte Warteschlangen und Themen:

* Für partitionierte Warteschlangen und Themen wird nicht das Senden von Nachrichten unterstützt, die unterschiedlichen Sitzungen einer einzelnen Transaktion angehören.
* Service Bus erlaubt derzeit bis zu 100 partitionierte Warteschlangen oder Themen pro Namespace. Jede partitionierte Warteschlange bzw. jedes partitionierte Thema wird in das zulässige Kontingent von 10.000 Entitäten pro Namespace eingerechnet (gilt nicht für Premium-Tarif).

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zur Partitionierung von Nachrichtenentitäten finden Sie unter [AMQP 1.0-Unterstützung für partitionierte Warteschlangen und Themen von Service Bus][AMQP 1.0 support for Service Bus partitioned queues and topics]. 

[Service Bus architecture]: service-bus-architecture.md
[Azure portal]: https://portal.azure.com
[QueueDescription.EnablePartitioning]: /dotnet/api/microsoft.servicebus.messaging.queuedescription#Microsoft_ServiceBus_Messaging_QueueDescription_EnablePartitioning
[TopicDescription.EnablePartitioning]: /dotnet/api/microsoft.servicebus.messaging.topicdescription#Microsoft_ServiceBus_Messaging_TopicDescription_EnablePartitioning
[BrokeredMessage.SessionId]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_SessionId
[BrokeredMessage.PartitionKey]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_PartitionKey
[SessionId]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_SessionId
[PartitionKey]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_PartitionKey
[QueueDescription.RequiresDuplicateDetection]: /dotnet/api/microsoft.servicebus.messaging.queuedescription#Microsoft_ServiceBus_Messaging_QueueDescription_RequiresDuplicateDetection
[BrokeredMessage.MessageId]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_MessageId
[MessageId]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_MessageId
[MessagingFactorySettings.OperationTimeout]: /dotnet/api/microsoft.servicebus.messaging.messagingfactorysettings#Microsoft_ServiceBus_Messaging_MessagingFactorySettings_OperationTimeout
[OperationTimeout]: /dotnet/api/microsoft.servicebus.messaging.messagingfactorysettings#Microsoft_ServiceBus_Messaging_MessagingFactorySettings_OperationTimeout
[QueueDescription.ForwardTo]: /dotnet/api/microsoft.servicebus.messaging.queuedescription#Microsoft_ServiceBus_Messaging_QueueDescription_ForwardTo
[AMQP 1.0 support for Service Bus partitioned queues and topics]: service-bus-partitioned-queues-and-topics-amqp-overview.md

