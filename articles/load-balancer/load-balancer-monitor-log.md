---
title: "Überwachen von Vorgängen, Ereignissen und Leistungsindikatoren für Load Balancer | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie die Protokollierung für Warnereignisse und Integritätsteststatus für den Azure-Lastenausgleich aktivieren."
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
tags: azure-resource-manager
ms.assetid: 56656d74-0241-4096-88c8-aa88515d676d
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/24/2016
ms.author: kumud
translationtype: Human Translation
ms.sourcegitcommit: 503f5151047870aaf87e9bb7ebf2c7e4afa27b83
ms.openlocfilehash: 638ecd5e02889bd8cb6e7429dfcec335feaac4a3
ms.lasthandoff: 03/28/2017

---

# <a name="log-analytics-for-azure-load-balancer"></a>Protokollanalysen für den Azure-Lastenausgleich

Sie können in Azure verschiedene Protokolltypen verwenden, um den Lastenausgleich zu verwalten und eventuelle Fehler zu beheben. Auf einige dieser Protokolle kann über das Portal zugegriffen werden. Alle Protokolle können aus Azure Blob Storage extrahiert und in anderen Tools wie Excel und PowerBI angezeigt werden. In der unten stehenden Liste finden Sie weitere Informationen über die verschiedenen Typen von Protokollen.

* **Überwachungsprotokolle:** Sie können [Azure-Überwachungsprotokolle](../monitoring-and-diagnostics/insights-debugging-with-events.md) (ehemals Betriebsprotokolle) verwenden, um alle Vorgänge, die an Ihre Azure-Abonnements übermittelt werden, sowie deren Status anzuzeigen. Überwachungsprotokolle sind standardmäßig aktiviert und können im Azure-Portal angezeigt werden.
* **Warnereignisprotokolle** : Verwenden Sie dieses Protokoll, um Warnungen anzuzeigen, die vom Lastenausgleich ausgelöst wurden. Der Status des Lastenausgleichs wird alle fünf Minuten erfasst. Dieses Protokoll wird nur geschrieben, wenn ein Warnereignis für den Lastenausgleich ausgelöst wird.
* **Integritätstestprotokolle**: Verwenden Sie dieses Protokoll zum Anzeigen von Problemen, die vom Integritätstest erkannt wurden, z.B. die Anzahl der Instanzen in Ihrem Back-End-Pool, die aufgrund von Integritätstestfehlern keine Anforderungen vom Lastenausgleich empfangen. In dieses Protokoll wird geschrieben, wenn sich der Integritätsteststatus ändert.

> [!IMPORTANT]
> Die Protokollanalyse funktioniert derzeit nur für Lastenausgleichsmodule mit Internetzugriff. Protokolle sind nur für Ressourcen verfügbar, die im Ressourcen-Manager-Bereitstellungsmodell bereitgestellt werden. Sie können Protokolle nicht für Ressourcen im klassischen Bereitstellungsmodell verwenden. Weitere Informationen zu diesen Bereitstellungsmodellen finden Sie unter [Grundlegendes zur Bereitstellung über Resource Manager im Vergleich zur klassischen Bereitstellung](../azure-resource-manager/resource-manager-deployment-model.md).

## <a name="enable-logging"></a>Aktivieren der Protokollierung

Die Überwachungsprotokollierung ist automatisch für alle Ressourcen-Manager-Ressourcen aktiviert. Sie müssen die Ereignis- und Integritätstestprotokollierung aktivieren, um mit der Erfassung von Daten aus diesen Protokollen zu beginnen. Führen Sie die folgenden Schritte aus, um die Protokollierung zu aktivieren:

Melden Sie sich beim [Azure-Portal](http://portal.azure.com)an. Wenn Sie noch nicht über einen Load Balancer verfügen, [erstellen Sie einen Load Balancer](load-balancer-get-started-internet-arm-ps.md) , bevor Sie fortfahren.

1. Klicken Sie im Portal auf **Durchsuchen**.
2. Wählen Sie **Lastenausgleichsmodule** aus.

    ![Portal – Lastenausgleich](./media/load-balancer-monitor-log/load-balancer-browse.png)

3. Wählen Sie ein vorhandenes Lastenausgleichsmodul und anschließend **Alle Einstellungen** aus.
4. Scrollen Sie auf der rechten Seite des Dialogfelds unter dem Namen des Lastenausgleichsmoduls zu **Überwachung**, und klicken Sie anschließend auf **Diagnose**.

    ![Portal – Einstellungen für den Lastenausgleich](./media/load-balancer-monitor-log/load-balancer-settings.png)

5. Wählen Sie im Bereich **Diagnose** unter **Status** die Option **Ein** aus.
6. Klicken Sie auf **Speicherkonto**.
7. Wählen Sie unter **PROTOKOLLE** ein vorhandenes Speicherkonto aus, oder erstellen Sie ein neues. Legen Sie mithilfe des Schiebereglers in Tagen fest, wie lange Ereignisdaten im Ereignisprotokoll gespeichert werden sollen. 
8. Klicken Sie auf **Speichern**.

    ![Portal – Diagnoseprotokolle](./media/load-balancer-monitor-log/load-balancer-diagnostics.png)

> [!NOTE]
> Für Überwachungsprotokolle ist kein separates Speicherkonto erforderlich. Bei der Nutzung von Speicher zur Ereignis- und Integritätstestprotokollierung fallen Gebühren an.

## <a name="audit-log"></a>Überwachungsprotokoll

Das Überwachungsprotokoll wird standardmäßig generiert. Die Protokolle werden 90 Tage lang im Azure-Ereignisprotokollspeicher aufbewahrt. Weitere Informationen zu diesen Protokollen finden Sie im Artikel [Anzeigen von Ereignis- und Überwachungsprotokollen](../monitoring-and-diagnostics/insights-debugging-with-events.md) .

## <a name="alert-event-log"></a>Warnereignisprotokoll

Dieses Protokoll wird nur generiert, wenn Sie es für das jeweilige Lastenausgleichsmodul aktiviert haben. Die Ereignisse werden im JSON-Format protokolliert und in dem Speicherkonto gespeichert, das Sie beim Aktivieren der Protokollierung angegeben haben. Im Anschluss finden Sie ein Beispiel für ein Ereignis.

```json
{
    "time": "2016-01-26T10:37:46.6024215Z",
    "systemId": "32077926-b9c4-42fb-94c1-762e528b5b27",
    "category": "LoadBalancerAlertEvent",
    "resourceId": "/SUBSCRIPTIONS/XXXXXXXXXXXXXXXXX-XXXX-XXXX-XXXXXXXXX/RESOURCEGROUPS/RG7/PROVIDERS/MICROSOFT.NETWORK/LOADBALANCERS/WWEBLB",
    "operationName": "LoadBalancerProbeHealthStatus",
    "properties": {
        "eventName": "Resource Limits Hit",
        "eventDescription": "Ports exhausted",
        "eventProperties": {
            "public ip address": "40.117.227.32"
        }
    }
}
```

Die JSON-Ausgabe zeigt die *eventname* -Eigenschaft, die den Grund beschreibt, aus dem der Lastenausgleich eine Warnung generiert hat. In diesem Fall wurde die Warnung generiert, weil Quell-IP-NAT-Grenzwerte (SNAT) eine TCP-Portauslastung verursacht haben.

## <a name="health-probe-log"></a>Integritätstestprotokoll

Dieses Protokoll wird nur generiert, wenn Sie es wie oben beschrieben für das jeweilige Lastenausgleichsmodul aktiviert haben. Die Daten werden im Speicherkonto gespeichert, das Sie beim Aktivieren der Protokollierung angegeben haben. Ein Container namens „insights-logs-loadbalancerprobehealthstatus“ wird erstellt, und die folgenden Daten werden protokolliert:

```json
{
    "records":[
    {
        "time": "2016-01-26T10:37:46.6024215Z",
        "systemId": "32077926-b9c4-42fb-94c1-762e528b5b27",
        "category": "LoadBalancerProbeHealthStatus",
        "resourceId": "/SUBSCRIPTIONS/XXXXXXXXXXXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXX/RESOURCEGROUPS/RG7/PROVIDERS/MICROSOFT.NETWORK/LOADBALANCERS/WWEBLB",
        "operationName": "LoadBalancerProbeHealthStatus",
        "properties": {
            "publicIpAddress": "40.83.190.158",
            "port": "81",
            "totalDipCount": 2,
            "dipDownCount": 1,
            "healthPercentage": 50.000000
        }
    },
    {
        "time": "2016-01-26T10:37:46.6024215Z",
        "systemId": "32077926-b9c4-42fb-94c1-762e528b5b27",
        "category": "LoadBalancerProbeHealthStatus",
        "resourceId": "/SUBSCRIPTIONS/XXXXXXXXXXXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXX/RESOURCEGROUPS/RG7/PROVIDERS/MICROSOFT.NETWORK/LOADBALANCERS/WWEBLB",
        "operationName": "LoadBalancerProbeHealthStatus",
        "properties": {
            "publicIpAddress": "40.83.190.158",
            "port": "81",
            "totalDipCount": 2,
            "dipDownCount": 0,
            "healthPercentage": 100.000000
        }
    }]
}
```

Die JSON-Ausgabe zeigt im Eigenschaftenfeld die grundlegenden Informationen zum Status des Integritätstests. Die *dipDownCount* -Eigenschaft zeigt die Gesamtzahl von Instanzen im Back-End, die aufgrund fehlerhafter Testantworten keinen Netzwerkdatenverkehr empfangen.

## <a name="view-and-analyze-the-audit-log"></a>Anzeigen und Analysieren des Überwachungsprotokolls

Mit einer der folgenden Methoden können Sie die Überwachungsprotokolldaten anzeigen und analysieren:

* **Azure-Tools:** Rufen Sie Informationen aus den Überwachungsprotokollen über Azure PowerShell, die Azure-Befehlszeilenschnittstelle, die Azure REST-API oder über das Azure-Vorschauportal ab. Schrittweise Anleitungen für die einzelnen Methoden finden Sie im Artikel [Überwachen von Vorgängen mit dem Ressourcen-Manager](../azure-resource-manager/resource-group-audit.md) .
* **Power BI:** Wenn Sie noch kein [Power BI](https://powerbi.microsoft.com/pricing)-Konto besitzen, können Sie es kostenlos testen. Mithilfe des [Azure Audit Logs Content Pack for Power BI](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-audit-logs) können Sie Ihre Daten mit vorkonfigurierten Dashboards analysieren oder Ansichten an Ihre Anforderungen anpassen.

## <a name="view-and-analyze-the-health-probe-and-event-log"></a>Anzeigen und Analysieren des Integritätstest- und Ereignisprotokolls

Sie müssen eine Verbindung mit Ihrem Speicherkonto herstellen und die JSON-Protokolleinträge für das Integritätstest- und das Ereignisprotokoll abrufen. Sobald Sie die JSON-Dateien heruntergeladen haben, können Sie diese in das CSV-Format konvertieren oder in Excel, PowerBI oder einem anderen Datenvisualisierungstool anzeigen.

> [!TIP]
> Wenn Sie mit Visual Studio und den grundlegenden Konzepten zum Ändern der Werte für Konstanten und Variablen in C# vertraut sind, können Sie die [Protokollkonvertierungstools](https://github.com/Azure-Samples/networking-dotnet-log-converter) von GitHub verwenden.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Visualize your Azure Audit Logs with Power BI](http://blogs.msdn.com/b/powerbi/archive/2015/09/30/monitor-azure-audit-logs-with-power-bi.aspx) .
* [View and analyze Azure Audit Logs in Power BI and more](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/) .

## <a name="next-steps"></a>Nächste Schritte

[Grundlegendes zu Load Balancer-Tests](load-balancer-custom-probe-overview.md)

