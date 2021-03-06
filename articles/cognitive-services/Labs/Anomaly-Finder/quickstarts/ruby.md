---
title: Verwenden der API für die Suche nach Anomalien mit Ruby – Microsoft Cognitive Services | Microsoft-Dokumentation
description: Informationen und Codebeispiele für den schnellen Einstieg in die Verwendung von Ruby und der API für die Suche nach Anomalien in Cognitive Services
services: cognitive-services
author: chliang
manager: bix
ms.service: cognitive-services
ms.subservice: anomaly-detection
ms.topic: article
ms.date: 05/01/2018
ms.author: chliang
ms.openlocfilehash: a229011617239b829ce2622a0482fd9b4908aca6
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55224206"
---
# <a name="use-the-anomaly-finder-api-with-ruby"></a>Verwenden der API für die Suche nach Anomalien mit Ruby

[!INCLUDE [PrivatePreviewNote](../../../../../includes/cognitive-services-anomaly-finder-private-preview-note.md)]

In diesem Artikel finden Sie Informationen und Codebeispiele, die Ihnen bei der Nutzung der API für die Suche nach Anomalien mit Ruby helfen, damit Sie Ergebnisse der Anomalieerkennung bei Zeitreihendaten erfassen können.

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [GetSubscriptionKey](../includes/get-subscription-key.md)]

## <a name="getting-anomaly-points-with-anomaly-finder-api-using-ruby"></a>Abrufen von Anomaliepunkten mithilfe der API für die Suche nach Anomalien mit Ruby 
[!INCLUDE [DataContract](../includes/datacontract.md)]

### <a name="example-of-time-series-data"></a>Beispiel für Zeitreihendaten
So sieht das Beispiel für die Datenpunkte der Zeitreihen aus:

[!INCLUDE [Request](../includes/request.md)]

### <a name="analyze-data-and-get-anomaly-points-ruby-example"></a>Beispiel zum Analysieren von Daten und Abrufen von Anomaliepunkten mit Ruby

Sie können das Beispiel folgendermaßen anwenden:

1. Installieren Sie den [REST-Client](https://github.com/rest-client/rest-client), indem Sie „gem install rest-client“ ausführen.
2. Speichern Sie den Code unten als RB-Datei.
3. Ersetzen Sie den Wert `[YOUR_SUBSCRIPTION_KEY]` durch Ihren gültigen Abonnementschlüssel.
4. Ersetzen Sie `[REPLACE_WITH_THE_EXAMPLE_OR_YOUR_OWN_DATA_POINTS]` durch das Beispiel oder Ihre eigenen Datenpunkte.
5. Führen Sie die Anforderung aus, und überprüfen Sie die Antwort.

```ruby
# https://github.com/rest-client/rest-client
require 'rest_client'

# **********************************************
# *** Update or verify the following values. ***
# **********************************************

# Replace the subscriptionKey string value with your valid subscription key.
subscription_key = '[YOUR_SUBSCRIPTION_KEY]';

endpoint = "https://api.labs.cognitive.microsoft.com/anomalyfinder/v1.0/anomalydetection";

# Replace the request data with your real data.
requestData = '[REPLACE_WITH_THE_EXAMPLE_OR_YOUR_OWN_DATA_POINTS]';

header = {
    :content_type => 'application/json',
    :'Ocp-Apim-Subscription-Key' => subscription_key
}

response = RestClient::Request.execute(
    :url => endpoint,
    :method => :post,
    :verify_ssl => true,
    :payload => requestData,
    :header => header)

# You will see the response with anomaly results
puts response.body
```

### <a name="example-response"></a>Beispielantwort

Eine erfolgreiche Antwort wird im JSON-Format zurückgegeben. Hier sehen Sie eine Beispielantwort:
[!INCLUDE [Response](../includes/response.md)]

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [REST-API-Referenz](https://dev.labs.cognitive.microsoft.com/docs/services/anomaly-detection/operations/post-anomalydetection)
