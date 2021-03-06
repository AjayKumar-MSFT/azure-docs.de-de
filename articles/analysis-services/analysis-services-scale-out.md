---
title: Horizontales Hochskalieren von Azure Analysis Services | Microsoft-Dokumentation
description: Replizieren von Azure Analysis Services-Servern mittels horizontalem Hochskalieren
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 01/18/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 213a695d99c50cea5962237c6210e6efcdbc5f6a
ms.sourcegitcommit: 82cdc26615829df3c57ee230d99eecfa1c4ba459
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/19/2019
ms.locfileid: "54411678"
---
# <a name="azure-analysis-services-scale-out"></a>Horizontales Hochskalieren von Azure Analysis Services

Durch horizontales Hochskalieren können Clientabfragen auf mehrere *Abfragereplikate* in einem Abfragepool verteilt werden, was bei einem hohen Aufkommen von Abfrageworkloads zu kürzeren Antwortzeiten führt. Darüber hinaus kann die Verarbeitung vom Abfragepool getrennt werden, um sicherzustellen, dass Clientabfragen nicht durch Verarbeitungsvorgänge beeinträchtigt werden. Horizontales Hochskalieren kann im Azure-Portal oder mithilfe der Analysis Services-REST-API konfiguriert werden.

## <a name="how-it-works"></a>So funktioniert's

In einer typischen Serverbereitstellung fungiert ein einzelner Server sowohl als Verarbeitungs- als auch als Abfrageserver. Wenn die Anzahl von Clientabfragen für Modelle auf dem Server die QPUs (Query Processing Units) für den Tarif Ihres Servers übersteigt oder die Modellverarbeitung mit einem hohen Aufkommen von Abfrageworkloads zusammenfällt, kann sich dies negativ auf die Leistung auswirken. 

Mit der horizontalen Hochskalierung können Sie einen Abfragepool mit bis zu sieben zusätzlichen Abfragereplikatressourcen erstellen, sodass Sie zusammen mit Ihrem Server über insgesamt acht verfügen. Sie können die Anzahl von Abfragereplikaten jederzeit skalieren, um die QPU-Anforderungen in kritischen Zeiten zu erfüllen, und Sie können einen Verarbeitungsserver aus dem Abfragepool herauslösen. Alle Abfragereplikate werden in der gleichen Region wie Ihr Server erstellt.

Verarbeitungsworkloads werden unabhängig von der Anzahl von Abfragereplikaten in einem Abfragepool nicht auf Abfragereplikate verteilt. Ein einzelner Server fungiert als Verarbeitungsserver. Abfragereplikate bedienen ausschließlich Abfragen für die Modelle, die zwischen den einzelnen Abfragereplikaten im Abfragepool synchronisiert werden. 

Beim horizontalen Hochskalieren werden dem Abfragepool inkrementell neue Abfragereplikate hinzugefügt. Es kann bis zu fünf Minuten dauern, bis neue Abfragereplikatressourcen in den Abfragepool aufgenommen werden. Wenn alle neuen Abfragereplikate einsatzbereit sind, kommen für neue Clientverbindungen alle Ressourcen im Abfragepool für den Lastenausgleich zum Einsatz. Bestehende Clientverbindungen werden nicht geändert; die Verbindung mit der bisherigen Ressource bleibt bestehen.  Beim horizontalen Herunterskalieren werden alle bestehenden Clientverbindungen mit einer Abfragepoolressource, die aus dem Abfragepool entfernt wird, beendet. Sie werden nach Abschluss des horizontalen Herunterskalierens mit einer der übrigen Abfragepoolressourcen neu verbunden, was bis zu fünf Minuten dauern kann.

Bei der Verarbeitung von Modellen ist nach dem Abschluss der Verarbeitungsvorgänge eine Synchronisierung zwischen dem Verarbeitungsserver und den Abfragereplikaten erforderlich. Wenn Verarbeitungsvorgänge automatisiert werden, muss auch ein Synchronisierungsvorgang konfiguriert werden, der nach erfolgreicher Durchführung von Verarbeitungsvorgängen ausgeführt wird. Die Synchronisierung kann manuell im Portal oder mithilfe von PowerShell oder der REST-API ausgeführt werden. 

### <a name="separate-processing-from-query-pool"></a>Getrennte Verarbeitung vom Abfragepool

Zur Optimierung der Leistung bei Verarbeitungs- und Abfragevorgängen können Sie optional Ihren Verarbeitungsserver vom Abfragepool trennen. Nach der Trennung werden vorhandene und neue Clientverbindungen nur den Abfragereplikaten im Abfragepool zugewiesen. Wenn Verarbeitungsvorgänge nur eine kurze Zeit dauern, können Sie Ihren Verarbeitungsserver auch nur für den Zeitraum vom Abfragepool trennen, der zur Durchführung der Verarbeitungs- und Synchronisierungsvorgänge erforderlich ist, und ihn dann wieder in den Abfragepool aufnehmen. 

> [!NOTE]
> Horizontales Hochskalieren ist für Server im Standardtarif verfügbar. Jedes Abfragereplikat wird mit dem gleichen Tarif abgerechnet wie Ihr Server.

> [!NOTE]
> Durch horizontales Hochskalieren erhöht sich nicht die Menge an verfügbarem Arbeitsspeicher für Ihren Server. Zur Erhöhung des Arbeitsspeichers müssen Sie Ihren Plan upgraden.

## <a name="region-limits"></a>Regionale Einschränkungen

Die Anzahl der von Ihnen konfigurierbaren Abfragereplikate ist durch die Region, in der sich Ihr Server befindet, eingeschränkt. Weitere Informationen finden Sie unter [Verfügbarkeit nach Region](analysis-services-overview.md#availability-by-region).

## <a name="monitor-qpu-usage"></a>Überwachen der QPU-Nutzung

 Überwachen Sie Ihren Server im Azure-Portal mithilfe von Metriken, um zu ermitteln, ob horizontales Hochskalieren für Ihren Server erforderlich ist. Wenn regelmäßig die QPU-Obergrenze erreicht wird, übersteigt die Anzahl von Abfragen für Ihre Modelle das QPU-Limit für Ihren Tarif. Die Metrik „Warteschlangenlänge für Abfragepoolaufträge“ erhöht sich auch, wenn die Anzahl von Abfragen in der Warteschlange des Abfragethreadpools die verfügbaren QPUs übersteigt. Weitere Informationen finden Sie unter [Überwachen von Servermetriken](analysis-services-monitor.md).

## <a name="configure-scale-out"></a>Konfigurieren des horizontalen Hochskalierens

### <a name="in-azure-portal"></a>Im Azure-Portal

1. Klicken Sie im Portal auf **Horizontale Skalierung**. Wählen Sie mithilfe des Schiebereglers die Anzahl von Abfragereplikatservern aus. Die gewählte Anzahl von Replikaten kommt zu Ihrem bereits vorhandenen Server hinzu.

2. Klicken Sie unter **Verarbeitungsserver vom Abfragepool trennen** auf „Ja“, um Ihren Verarbeitungsserver von Abfrageservern auszuschließen. Clientverbindungen, die die Standardverbindungszeichenfolge (ohne: rw) verwenden, werden an Replikate im Abfragepool umgeleitet. 

   ![Schieberegler für horizontales Hochskalieren](media/analysis-services-scale-out/aas-scale-out-slider.png)

3. Klicken Sie auf **Speichern**, um Ihre neuen Abfragereplikatserver bereitzustellen. 

Tabellarische Modelle auf Ihrem primären Server werden mit den Replikatservern synchronisiert. Nach Abschluss der Synchronisierung beginnt der Abfragepool mit der Verteilung eingehender Abfragen auf die Replikatserver. 

## <a name="synchronization"></a>Synchronisierung 

Wenn Sie neue Abfragereplikate bereitstellen, repliziert Azure Analysis Services Ihre Modelle automatisch in allen Replikaten. Sie können auch eine manuelle Synchronisierung mithilfe des Portals oder der REST-API ausführen. Wenn Sie Ihre Modelle verarbeiten, sollten Sie eine Synchronisierung durchführen, damit Aktualisierungen zwischen Ihren Abfragereplikaten synchronisiert werden.

### <a name="in-azure-portal"></a>Im Azure-Portal

Klicken Sie in der **Übersicht** für das Modell auf das Symbol **Modell synchronisieren**.

![Schieberegler für horizontales Hochskalieren](media/analysis-services-scale-out/aas-scale-out-sync.png)

### <a name="rest-api"></a>REST-API

Verwenden Sie die **sync**-Operation.

#### <a name="synchronize-a-model"></a>Synchronisieren eines Modells   

`POST https://<region>.asazure.windows.net/servers/<servername>:rw/models/<modelname>/sync`

#### <a name="get-sync-status"></a>Abrufen des Synchronisierungsstatus  

`GET https://<region>.asazure.windows.net/servers/<servername>/models/<modelname>/sync`

### <a name="powershell"></a>PowerShell

Vor der Verwendung von PowerShell müssen Sie [das neueste AzureRM-Modul installieren oder aktualisieren](https://github.com/Azure/azure-powershell/releases). 

Verwenden Sie [Set-AzureRmAnalysisServicesServer](https://docs.microsoft.com/powershell/module/azurerm.analysisservices/set-azurermanalysisservicesserver), um die Anzahl der Abfragereplikate festzulegen. Geben Sie den optionalen Parameter `-ReadonlyReplicaCount` an.

Verwenden Sie [Sync-AzureAnalysisServicesInstance](https://docs.microsoft.com/powershell/module/azurerm.analysisservices/sync-azureanalysisservicesinstance), um die Synchronisierung auszuführen.

## <a name="connections"></a>Verbindungen

Auf der Übersichtsseite des Servers werden zwei Servernamen angezeigt. Wenn Sie noch keine horizontale Hochskalierung für einen Server konfiguriert haben, funktionieren beide Servernamen gleich. Nach dem Konfigurieren der horizontalen Hochskalierung für einen Server müssen Sie den passenden Servernamen für den jeweiligen Verbindungstyp angeben. 

Für Endbenutzer-Clientverbindungen wie Power BI Desktop, Excel und benutzerdefinierte Apps muss der **Servername** verwendet werden. 

Für SSMS, SSDT und Verbindungszeichenfolgen in PowerShell, Azure Functions-Apps und AMO muss der **Name des Verwaltungsservers** verwendet werden. Der Name des Verwaltungsservers enthält einen speziellen `:rw`-Qualifizierer (Lesen/Schreiben). Sämtliche Verarbeitungsvorgänge finden auf dem Verwaltungsserver statt.

![Servernamen](media/analysis-services-scale-out/aas-scale-out-name.png)

## <a name="troubleshoot"></a>Problembehandlung

**Problem:** Benutzer erhalten die Fehlermeldung **Serverinstanz „\<Name des Servers>“ im Verbindungsmodus „ReadOnly“ nicht gefunden.**

**Lösung:** Beim Auswählen der **Verarbeitungsserver vom Abfragepool trennen** werden Clientverbindungen, die die Standardverbindungszeichenfolge (ohne „:rw“) verwenden, an Abfragepoolreplikate umgeleitet. Wenn Replikate im Abfragepool noch nicht online sind, weil die Synchronisierung noch nicht abgeschlossen ist, können umgeleitete Clientverbindungen fehlschlagen. Um Verbindungsfehler zu verhindern, müssen beim Durchführen einer Synchronisierung mindestens zwei Server im Abfragepool enthalten sein. Jeder Server wird einzeln synchronisiert, während andere Server online bleiben. Wenn der Verarbeitungsserver während der Verarbeitung nicht im Abfragepool enthalten sein soll, können Sie ihn für die Verarbeitung aus dem Pool entfernen und dann nach Abschluss der Verarbeitung, jedoch vor der Synchronisierung, wieder zum Pool hinzufügen. Verwenden Sie die Metriken „Arbeitsspeicher“ und „QPU“, um den Synchronisierungsstatus zu überwachen.

## <a name="related-information"></a>Verwandte Informationen

[Überwachen von Servermetriken](analysis-services-monitor.md)   
[Verwalten von Azure Analysis Services](analysis-services-manage.md) 

