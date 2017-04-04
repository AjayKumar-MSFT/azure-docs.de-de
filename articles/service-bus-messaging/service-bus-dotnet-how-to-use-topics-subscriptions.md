---
title: Verwenden von Azure Service Bus-Themen mit .NET | Microsoft-Dokumentation
description: "Erfahren Sie mehr zur Verwendung von Service Bus-Themen und -Abonnements mit .NET in Azure. Die Codebeispiele wurden für .NET-Anwendungen geschrieben."
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 31d0bc29-6524-4b1b-9c7f-aa15d5a9d3b4
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: get-started-article
ms.date: 03/23/2017
ms.author: sethm
translationtype: Human Translation
ms.sourcegitcommit: 0bec803e4b49f3ae53f2cc3be6b9cb2d256fe5ea
ms.openlocfilehash: bec18e91ef8798a791d4b1fe93bd529593197e01
ms.lasthandoff: 03/24/2017


---
# <a name="how-to-use-service-bus-topics-and-subscriptions"></a>Verwenden von Service Bus-Themen und -Abonnements
[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

In diesem Artikel erfahren Sie, wie Sie Service Bus-Themen und -Abonnements verwenden. Die Beispiele sind in C# geschrieben und verwenden die .NET APIs. Die behandelten Szenarien umfassen das Erstellen von Themen und Abonnements, das Erstellen von Abonnementfiltern, das Senden von Nachrichten an ein Thema, das Empfangen von Nachrichten von einem Abonnement und das Löschen von Themen und Abonnements. Weitere Informationen zu Themen und Abonnements finden Sie im Abschnitt [Nächste Schritte](#next-steps).

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

[!INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

## <a name="configure-the-application-to-use-service-bus"></a>Konfigurieren Ihrer Anwendung für die Verwendung von Service Bus
Wenn Sie eine Anwendung erstellen, die den Servicebus verwendet, müssen Sie zu der Servicebus-Assembly eine Referenz hinzufügen und die zugehörigen Namespaces einbinden. Die einfachste Möglichkeit besteht hierbei darin, das entsprechende [NuGet](https://www.nuget.org)-Paket herunterzuladen.

## <a name="get-the-service-bus-nuget-package"></a>Abrufen des NuGet-Pakets "Service Bus"
Das [NuGet-Paket „Service Bus “](https://www.nuget.org/packages/WindowsAzure.ServiceBus) stellt die einfachste Möglichkeit zum Konfigurieren der Anwendung mit allen erforderlichen Service Bus-Abhängigkeiten dar. Gehen Sie wie folgt vor, um das NuGet-Paket „Service Bus“ in Ihrem Projekt zu installieren:

1. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf **Verweise**, und klicken Sie anschließend auf **NuGet-Pakete verwalten**.
2. Klicken Sie auf **Durchsuchen**, suchen Sie nach „Azure Service Bus“, und wählen Sie dann den Eintrag **Microsoft Azure Service Bus**. Klicken Sie auf **Installieren**, um die Installation abzuschließen. Schließen Sie anschließend das Dialogfeld:
   
   ![][7]

Sie können nun Code für Service Bus erstellen.

## <a name="create-a-service-bus-connection-string"></a>Erstellen einer Service Bus-Verbindungszeichenfolge
Service Bus verwendet eine Verbindungszeichenfolge zum Speichern von Endpunkten und Anmeldeinformationen. Sie können die Verbindungszeichenfolge in einer Konfigurationsdatei ablegen, statt sie fest zu programmieren:

* Bei der Verwendung von Azure-Diensten empfiehlt es sich, die Verbindungszeichenfolge mithilfe des Azure-Dienstkonfigurationssystems (CSDEF- und CSCFG-Dateien) zu speichern.
* Bei der Verwendung von Azure-Websites oder Azure Virtual Machines sollten Sie Ihre Verbindungszeichenfolge mithilfe des .NET-Konfigurationssystems speichern (z. B. die Web.config-Datei).

In beiden Fällen können Sie Ihre Verbindungszeichenfolge über die `CloudConfigurationManager.GetSetting`-Methode abrufen, wie später in diesem Artikel beschrieben.

### <a name="configure-your-connection-string"></a>Konfigurieren der Verbindungszeichenfolge
Der Dienstkonfigurationsmechanismus ermöglicht das dynamische Ändern der Konfigurationseinstellungen im [Azure-Portal][Azure portal], ohne dass die Anwendung neu bereitgestellt werden muss. Fügen Sie beispielsweise der Dienstdefinitionsdatei (**.csdef**) eine `Setting`-Bezeichnung hinzu. Dies ist im folgenden Beispiel dargestellt:

```xml
<ServiceDefinition name="Azure1">
...
    <WebRole name="MyRole" vmsize="Small">
        <ConfigurationSettings>
            <Setting name="Microsoft.ServiceBus.ConnectionString" />
        </ConfigurationSettings>
    </WebRole>
...
</ServiceDefinition>
```

Anschließend geben Sie Werte in der Dienstkonfigurationsdatei (.cscfg) an:

```xml
<ServiceConfiguration serviceName="Azure1">
...
    <Role name="MyRole">
        <ConfigurationSettings>
            <Setting name="Microsoft.ServiceBus.ConnectionString"
                     value="Endpoint=sb://yourServiceNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey" />
        </ConfigurationSettings>
    </Role>
...
</ServiceConfiguration>
```

Verwenden Sie die aus dem Portal abgerufenen SAS-Schlüsselwerte und -Schlüsselnamen (Shared Access Signature). Dieses Verfahren wurde weiter oben beschrieben.

### <a name="configure-your-connection-string-when-using-azure-websites-or-azure-virtual-machines"></a>Konfigurieren der Verbindungszeichenfolge bei Verwendung von Azure-Websites oder virtuellen Azure-Computern
Bei der Verwendung von Websites oder virtuellen Computern wird empfohlen, das .NET-Konfigurationssystem zu verwenden (z. B. Web.config). Sie speichern die Verbindungszeichenfolge mit dem `<appSettings>`-Element.

```xml
<configuration>
    <appSettings>
        <add key="Microsoft.ServiceBus.ConnectionString"
             value="Endpoint=sb://yourServiceNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey" />
    </appSettings>
</configuration>
```

Verwenden Sie die aus dem [Azure-Portal][Azure portal] abgerufenen SAS-Namenswerte und -Schlüsselwerte. Dieses Verfahren wurde weiter oben beschrieben.

## <a name="create-a-topic"></a>Erstellen eines Themas
Verwaltungsvorgänge für Service Bus-Themen und -Abonnements können über die [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager)-Klasse durchgeführt werden. Diese Klasse stellt Methoden zum Erstellen, Aufzählen und Löschen von Themen zur Verfügung.

Im folgenden Beispiel wird ein `NamespaceManager`-Objekt mit der Azure-Klasse `CloudConfigurationManager` mit einer Verbindungszeichenfolge erstellt, die aus der Basisadresse eines Service Bus-Namespace und den entsprechenden SAS-Anmeldeinformationen mit Berechtigungen für dessen Verwaltung besteht. Diese Verbindungszeichenfolge weist die folgende Form auf:

```xml
Endpoint=sb://<yourNamespace>.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=<yourKey>
```

Verwenden Sie das folgende Beispiel mit den Konfigurationseinstellungen aus dem vorherigen Abschnitt.

```csharp
// Create the topic if it does not exist already.
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

var namespaceManager =
    NamespaceManager.CreateFromConnectionString(connectionString);

if (!namespaceManager.TopicExists("TestTopic"))
{
    namespaceManager.CreateTopic("TestTopic");
}
```

Es gibt Überladungen der [CreateTopic](/dotnet/api/microsoft.servicebus.namespacemanager)-Methode, die es Ihnen ermöglichen, die Eigenschaften des Themas zu optimieren. So können Sie beispielsweise den Standardwert für die TTL (Time-to-live, Lebensdauer) festlegen, der auf an das Thema gesendete Nachrichten angewendet wird. Diese Einstellungen werden mithilfe der [TopicDescription](/dotnet/api/microsoft.servicebus.messaging.topicdescription)-Klasse angewendet. Das folgende Beispiel zeigt, wie ein Thema namens **TestTopic** mit einer Maximalgröße von 5 GB und einer Standard-Nachrichten-TTL von 1 Minute erstellt wird.

```csharp
// Configure Topic Settings.
TopicDescription td = new TopicDescription("TestTopic");
td.MaxSizeInMegabytes = 5120;
td.DefaultMessageTimeToLive = new TimeSpan(0, 1, 0);

// Create a new Topic with custom settings.
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

var namespaceManager =
    NamespaceManager.CreateFromConnectionString(connectionString);

if (!namespaceManager.TopicExists("TestTopic"))
{
    namespaceManager.CreateTopic(td);
}
```

> [!NOTE]
> Sie können die [TopicExists](/dotnet/api/microsoft.servicebus.namespacemanager#Microsoft_ServiceBus_NamespaceManager_TopicExists_System_String_)-Methode in [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager)-Objekten verwenden, um zu prüfen, ob ein Thema mit einem bestimmten Namen innerhalb eines Namespace bereits vorhanden ist.
> 
> 

## <a name="create-a-subscription"></a>Erstellen eines Abonnements
Sie können auch Themenabonnements mithilfe der [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager)-Klasse erstellen. Abonnements werden benannt und können einen optionalen Filter aufweisen, der die Nachrichten einschränkt, die an die virtuelle Warteschlange des Abonnements übergeben werden.

> [!IMPORTANT]
> Damit Nachrichten von einem Abonnement empfangen werden können, müssen Sie das Abonnement erstellen, bevor Sie Nachrichten an das Thema senden. Sind für ein Thema keine Abonnements vorhanden, verwirft das Thema die Nachrichten.
> 
> 

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Erstellen eines Abonnements mit dem Standardfilter (MatchAll)
**MatchAll** ist der Standardfilter, der verwendet wird, wenn beim Erstellen eines neuen Abonnements kein Filter angegeben wird. Wenn Sie den Filter **MatchAll** verwenden, werden alle für das Thema veröffentlichten Nachrichten in die virtuelle Warteschlange des Abonnements eingereiht. Mit dem folgenden Beispiel wird ein Abonnement namens „AllMessages“ erstellt, für das der Standardfilter **MatchAll** verwendet wird.

```csharp
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

var namespaceManager =
    NamespaceManager.CreateFromConnectionString(connectionString);

if (!namespaceManager.SubscriptionExists("TestTopic", "AllMessages"))
{
    namespaceManager.CreateSubscription("TestTopic", "AllMessages");
}
```

### <a name="create-subscriptions-with-filters"></a>Erstellen von Abonnements mit Filtern
Sie können auch Filter einrichten, durch die Sie angeben können, welche an ein Thema gesendeten Nachrichten in einem bestimmten Themenabonnement angezeigt werden sollen.

Der von Abonnements unterstützte flexibelste Filtertyp ist die [SqlFilter][SqlFilter]-Klasse, die eine Teilmenge von SQL92 implementiert. SQL-Filter werden auf die Eigenschaften der Nachrichten angewendet, die für das Thema veröffentlicht werden. Weitere Informationen zu den Ausdrücken, die mit einem SQL-Filter verwendet werden können, finden Sie in der Syntax [SqlFilter.SqlExpression][SqlFilter.SqlExpression].

Mit dem folgenden Beispiel wird ein Abonnement namens **HighMessages** mit einem [SqlFilter][SqlFilter]-Objekt erstellt, bei dem nur Nachrichten ausgewählt werden, deren benutzerdefinierte **MessageNumber**-Eigenschaft größer als 3 ist.

```csharp
// Create a "HighMessages" filtered subscription.
SqlFilter highMessagesFilter =
   new SqlFilter("MessageNumber > 3");

namespaceManager.CreateSubscription("TestTopic",
   "HighMessages",
   highMessagesFilter);
```

Im folgenden Beispiel wird in ähnlicher Weise ein Abonnement namens **LowMessages** mit einem [SqlFilter][SqlFilter]-Objekt erstellt, bei dem nur Nachrichten ausgewählt werden, deren benutzerdefinierte **MessageNumber**-Eigenschaft kleiner oder gleich 3 ist:

```csharp
// Create a "LowMessages" filtered subscription.
SqlFilter lowMessagesFilter =
   new SqlFilter("MessageNumber <= 3");

namespaceManager.CreateSubscription("TestTopic",
   "LowMessages",
   lowMessagesFilter);
```

Wenn eine Nachricht an `TestTopic` gesendet wird, wird diese nun stets an die Empfänger des Themenabonnements **AllMessages** zugestellt. Sie wird selektiv an die Empfänger der Themenabonnements **HighMessages** und **LowMessages** zugestellt (je nach Inhalt der Nachricht).

## <a name="send-messages-to-a-topic"></a>Senden von Nachrichten an ein Thema
Um eine Nachricht an ein Service Bus-Thema zu senden, erstellt die Anwendung ein [TopicClient](/dotnet/api/microsoft.servicebus.messaging.topicclient)-Objekt mithilfe der Verbindungszeichenfolge.

Der folgende Code veranschaulicht, wie ein [TopicClient](/dotnet/api/microsoft.servicebus.messaging.topicclient)-Objekt für das **TestTopic**-Thema erstellt wird, das Sie gerade mit dem Aufruf der [CreateFromConnectionString](/dotnet/api/microsoft.servicebus.messaging.topicclient#Microsoft_ServiceBus_Messaging_TopicClient_CreateFromConnectionString_System_String_System_String_)-API erstellt haben.

```csharp
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

TopicClient Client =
    TopicClient.CreateFromConnectionString(connectionString, "TestTopic");

Client.Send(new BrokeredMessage());
```

An Service Bus-Themen gesendete Nachrichten sind Instanzen der [BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage)-Klasse. **BrokeredMessage**-Objekte verfügen über einen Satz von Standardeigenschaften (z.B. [Label](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Label) und [TimeToLive](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_TimeToLive)), ein Wörterbuch, in dem benutzerdefinierte anwendungsspezifische Eigenschaften enthalten sind, sowie einen Bestand beliebiger Anwendungsdaten. Eine Anwendung kann den Textkörper der Nachricht festlegen, indem ein beliebiges serialisierbares Objekt an den Konstruktor des **BrokeredMessage**-Objekts übergeben wird. Der entsprechende **DataContractSerializer** wird dann zum Serialisieren des Objekts verwendet. Alternativ kann ein **System.IO.Stream**-Objekt zur Verfügung gestellt werden.

Das folgende Beispiel zeigt, wie fünf Testnachrichten an das **TestTopic**-/[TopicClient](/dotnet/api/microsoft.servicebus.messaging.topicclient)-Objekt gesendet werden, das im vorherigen Codebeispiel abgerufen wurde. Beachten Sie, dass der Wert der [MessageId](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_MessageId)-Eigenschaft jeder Nachricht gemäß der Iteration der Schleife variiert (auf diese Weise wird bestimmt, welche Abonnements die Nachricht erhalten).

```csharp
for (int i=0; i<5; i++)
{
  // Create message, passing a string message for the body.
  BrokeredMessage message = new BrokeredMessage("Test message " + i);

  // Set additional custom app-specific property.
  message.Properties["MessageId"] = i;

  // Send message to the topic.
  Client.Send(message);
}
```

Service Bus-Themen unterstützen eine maximale Nachrichtengröße von 256 KB im [Standard-Tarif](service-bus-premium-messaging.md) und 1 MB im [Premium-Tarif](service-bus-premium-messaging.md). Der Header, der die standardmäßigen und benutzerdefinierten Anwendungseigenschaften enthält, kann eine maximale Größe von 64 KB haben. Es gibt keine Beschränkung für die Anzahl der Nachrichten, die ein Thema enthält. Es gibt jedoch eine Obergrenze für die Gesamtgröße der Nachrichten eines Themas. Die Themengröße wird bei der Erstellung definiert. Die Obergrenze beträgt 5 GB. Wenn Partitionierung aktiviert ist, ist die Obergrenze höher. Weitere Informationen finden Sie unter [Partitionierte Nachrichtenentitäten](service-bus-partitioning.md).

## <a name="how-to-receive-messages-from-a-subscription"></a>Empfangen von Nachrichten aus einem Abonnement
Die empfohlene Möglichkeit, Nachrichten aus einem Abonnement zu empfangen, besteht in der Verwendung eines [SubscriptionClient](/dotnet/api/microsoft.servicebus.messaging.subscriptionclient)-Objekts. **SubscriptionClient**-Objekte können in zwei unterschiedlichen Modi verwendet werden: [*ReceiveAndDelete* und *PeekLock*](/dotnet/api/microsoft.servicebus.messaging.receivemode). **PeekLock** ist die Standardeinstellung.

Bei Verwendung des **ReceiveAndDelete**-Modus ist der Nachrichtenempfang ein Single-Shot-Vorgang. Dies bedeutet: Wenn Service Bus eine Leseanforderung für eine Nachricht in einem Abonnement erhält, wird die Nachricht als verarbeitet gekennzeichnet und an die Anwendung zurückgesendet. Der **ReceiveAndDelete**-Modus ist das einfachste Modell. Es wird am besten für Szenarios eingesetzt, bei denen es eine Anwendung tolerieren kann, wenn eine Nachricht bei Auftreten eines Fehlers nicht verarbeitet wird. Um dieses Verfahren zu verstehen, stellen Sie sich ein Szenario vor, in dem der Consumer die Empfangsanforderung ausstellt und dann abstürzt, bevor diese verarbeitet wird. Da die Nachricht von Service Bus als verarbeitet gekennzeichnet wurde, wird die vor dem Absturz verarbeitete Nachricht nicht berücksichtigt, wenn die Anwendung neu gestartet und die Verarbeitung von Nachrichten wieder aufgenommen wird.

Im **PeekLock**-Modus (dem Standardmodus) ist der Empfangsprozess ein Vorgang mit zwei Phasen, der die Unterstützung von Anwendungen ermöglicht, die fehlende Nachrichten nicht tolerieren können. Wenn Service Bus eine Anfrage erhält, ermittelt der Dienst die nächste zu verarbeitende Nachricht, sperrt diese, um zu verhindern, dass andere Consumer sie erhalten, und sendet sie dann zurück an die Anwendung. Nachdem die Anwendung die Verarbeitung der Nachricht abgeschlossen hat (oder sie zwecks zukünftiger Verarbeitung zuverlässig gespeichert hat), führt sie die zweite Phase des Empfangsprozesses per Aufruf von [Complete](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Complete) für die empfangene Nachricht durch. Wenn der Service Bus den **Complete**-Aufruf erkennt, markiert er die Nachricht als verwendet und entfernt sie aus dem Abonnement.

Das folgende Beispiel zeigt, wie Nachrichten mit dem Standardmodus **PeekLock** empfangen und verarbeitet werden können. Zum Angeben eines anderen [ReceiveMode](/dotnet/api/microsoft.servicebus.messaging.receivemode)-Werts können Sie eine andere Überladung für [CreateFromConnectionString](/dotnet/api/microsoft.servicebus.messaging.subscriptionclient#Microsoft_ServiceBus_Messaging_SubscriptionClient_CreateFromConnectionString_System_String_System_String_System_String_Microsoft_ServiceBus_Messaging_ReceiveMode_) verwenden. Dieses Beispiel verwendet den [OnMessage](/dotnet/api/microsoft.servicebus.messaging.subscriptionclient#Microsoft_ServiceBus_Messaging_SubscriptionClient_OnMessage_System_Action_Microsoft_ServiceBus_Messaging_BrokeredMessage__Microsoft_ServiceBus_Messaging_OnMessageOptions_)-Rückruf zum Verarbeiten von Nachrichten, während diese im **HighMessages**-Abonnement eintreffen.

```csharp
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

SubscriptionClient Client =
    SubscriptionClient.CreateFromConnectionString
            (connectionString, "TestTopic", "HighMessages");

// Configure the callback options.
OnMessageOptions options = new OnMessageOptions();
options.AutoComplete = false;
options.AutoRenewTimeout = TimeSpan.FromMinutes(1);

Client.OnMessage((message) =>
{
    try
    {
        // Process message from subscription.
        Console.WriteLine("\n**High Messages**");
        Console.WriteLine("Body: " + message.GetBody<string>());
        Console.WriteLine("MessageID: " + message.MessageId);
        Console.WriteLine("Message Number: " +
            message.Properties["MessageNumber"]);

        // Remove message from subscription.
        message.Complete();
    }
    catch (Exception)
    {
        // Indicates a problem, unlock message in subscription.
        message.Abandon();
    }
}, options);
```

Das folgende Beispiel konfiguriert den [OnMessage](/dotnet/api/microsoft.servicebus.messaging.subscriptionclient#Microsoft_ServiceBus_Messaging_SubscriptionClient_OnMessage_System_Action_Microsoft_ServiceBus_Messaging_BrokeredMessage__Microsoft_ServiceBus_Messaging_OnMessageOptions_)-Rückruf mithilfe eines [OnMessageOptions](/dotnet/api/microsoft.servicebus.messaging.onmessageoptions)-Objekts. [AutoComplete](/dotnet/api/microsoft.servicebus.messaging.onmessageoptions#Microsoft_ServiceBus_Messaging_OnMessageOptions_AutoComplete) ist auf **false** festgelegt, um die manuelle Steuerung für den Aufruf [Complete](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Complete) für die empfangene Nachricht zu aktivieren. [AutoRenewTimeout](/dotnet/api/microsoft.servicebus.messaging.onmessageoptions#Microsoft_ServiceBus_Messaging_OnMessageOptions_AutoRenewTimeout) ist auf „1 Minute“ festgelegt. Der Client wartet also bis zu eine Minute lang, bevor das Feature für die automatische Verlängerung beendet wird und der Client einen neuen Aufruf durchführt, um das Vorhandensein von Nachrichten zu prüfen. Dieser Eigenschaftswert verringert die Anzahl der kostenpflichtigen Aufrufe des Clients, bei denen keine Nachrichten abgerufen werden.

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Behandeln von Anwendungsabstürzen und nicht lesbaren Nachrichten
Service Bus stellt Funktionen zur Verfügung, die Sie bei der ordnungsgemäßen Behandlung von Fehlern in der Anwendung oder bei Problemen beim Verarbeiten einer Nachricht unterstützen. Wenn eine empfangene Anwendung eine Nachricht aus einem beliebigen Grund nicht verarbeiten kann, kann sie die [Abandon](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Abandon_System_Collections_Generic_IDictionary_System_String_System_Object__)-Methode für die empfangene Nachricht aufrufen (anstatt der [Complete](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Complete)-Methode). Dies führt dazu, dass Service Bus die Nachricht innerhalb des Abonnements entsperrt und verfügbar macht, damit sie erneut empfangen werden kann, und zwar entweder durch dieselbe verarbeitende Anwendung oder durch eine andere verarbeitende Anwendung.

Zudem wird einer im Abonnement gesperrten Anwendung ein Zeitlimit zugeordnet. Wenn die Anwendung die Nachricht vor Ablauf des Sperrzeitlimits nicht verarbeiten kann (zum Beispiel wenn die Anwendung abstürzt) entsperrt Service Bus die Nachricht automatisch und macht sie verfügbar, um erneut empfangen zu werden.

Falls die Anwendung nach der Verarbeitung der Nachricht – aber vor Ausgabe der [Complete](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Complete)-Anforderung – abstürzt, wird die Nachricht wieder an die Anwendung zugestellt, wenn diese neu gestartet wird. Dies wird häufig als *At Least Once Processing* (Verarbeitung mindestens einmal) bezeichnet und bedeutet, dass jede Nachricht mindestens einmal verarbeitet wird, wobei dieselbe Nachricht in bestimmten Situationen unter Umständen erneut zugestellt wird. Wenn eine doppelte Verarbeitung im betreffenden Szenario nicht geeignet ist, sollten Anwendungsentwickler ihrer Anwendung zusätzliche Logik für den Umgang mit der Übermittlung doppelter Nachrichten hinzufügen. Dies wird häufig durch die Verwendung der [MessageId](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_MessageId) -Eigenschaft der Nachricht erzielt, die über mehrere Zustellungsversuche hinweg konstant bleibt.

## <a name="delete-topics-and-subscriptions"></a>Löschen von Themen und Abonnements
Das folgende Beispiel zeigt, wie das Thema **TestTopic** aus dem **HowToSample**-Dienstnamespace gelöscht wird.

```csharp
// Delete Topic.
namespaceManager.DeleteTopic("TestTopic");
```

Durch das Löschen eines Themas werden auch alle Abonnements gelöscht, die mit dem Thema registriert sind. Abonnements können auch unabhängig gelöscht werden. Der folgende Code zeigt, wie ein Abonnement namens **HighMessages** aus dem Thema **TestTopic** gelöscht wird:

```csharp
namespaceManager.DeleteSubscription("TestTopic", "HighMessages");
```

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie nun mit den Grundlagen der Service Bus-Themen und -Abonnements vertraut sind, finden Sie unter diesen Links weitere Informationen:

* [Warteschlangen, Themen und Abonnements][Queues, topics, and subscriptions]
* [Themenfilter – Beispiel][Topic filters sample]
* API-Referenz für [SqlFilter][SqlFilter]
* Erstellen Sie eine Anwendung, die Nachrichten an eine Service Bus-Warteschlange sendet und von dort empfängt: [.NET-Tutorial zu Service Bus-Brokermessaging][Service Bus brokered messaging .NET tutorial].
* Service Bus-Beispiele: Laden Sie diese aus den [Azure-Beispielen][Azure samples] herunter, oder sehen Sie sich die [Übersicht](service-bus-samples.md) an.

[Azure portal]: https://portal.azure.com

[7]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/getting-started-multi-tier-13.png

[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[Topic filters sample]: https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/TopicFilters
[SqlFilter]: /dotnet/api/microsoft.servicebus.messaging.sqlfilter
[SqlFilter.SqlExpression]: /dotnet/api/microsoft.servicebus.messaging.sqlfilter#Microsoft_ServiceBus_Messaging_SqlFilter_SqlExpression
[Service Bus brokered messaging .NET tutorial]: service-bus-brokered-tutorial-dotnet.md
[Azure samples]: https://code.msdn.microsoft.com/site/search?query=service%20bus&f%5B0%5D.Value=service%20bus&f%5B0%5D.Type=SearchText&ac=2

