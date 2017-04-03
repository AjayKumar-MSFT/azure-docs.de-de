---
title: "Überprüfen des Datenverkehrs mit der IP-Datenflussüberprüfung in Azure Network Watcher – PowerShell | Microsoft-Dokumentation"
description: "Dieser Artikel beschreibt, wie Sie mithilfe von PowerShell überprüfen, ob bei einem virtuellen Computer eingehender und ausgehender Datenverkehr zugelassen oder verweigert wird."
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: e1dad757-8c5d-467f-812e-7cc751143207
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: 2f03ba60d81e97c7da9a9fe61ecd419096248763
ms.openlocfilehash: 68860006266a60bf8e87f72d8669fb26ed3a5486
ms.lasthandoff: 03/04/2017


---
# <a name="check-if-traffic-is-allowed-or-denied-to-or-from-a-vm-with-ip-flow-verify-a-component-of-azure-network-watcher"></a>Überprüfen mit der IP-Datenflussüberprüfung (einer Komponente von Azure Network Watcher), ob bei einem virtuellen Computer eingehender und ausgehender Datenverkehr zugelassen oder verweigert wird

> [!div class="op_single_selector"]
> - [Azure-Portal](network-watcher-check-ip-flow-verify-portal.md)
> - [PowerShell](network-watcher-check-ip-flow-verify-powershell.md)
> - [BEFEHLSZEILENSCHNITTSTELLE (CLI)](network-watcher-check-ip-flow-verify-cli.md)
> - [Azure-REST-API](network-watcher-check-ip-flow-verify-rest.md)

Die IP-Datenflussüberprüfung ist ein Feature von Network Watcher, mit dem Sie überprüfen können, ob bei einem virtuellen Computer eingehender oder ausgehender Datenverkehr zugelassen wird. Dieses Szenario eignet sich zum Abrufen des aktuellen Status der Kommunikation eines virtuellen Computers mit einer externen Ressource oder einem Back-End. Anhand der IP-Datenflussüberprüfung können Sie überprüfen, ob die NSG-Regeln (Netzwerksicherheitsgruppe) ordnungsgemäß konfiguriert sind, und eine Problembehandlung für Datenflüsse durchführen, die durch die NSG-Regeln blockiert werden. Ein weiterer Grund für die Verwendung der IP-Datenflussüberprüfung besteht darin sicherzustellen, dass Datenverkehr, der blockiert werden soll, durch die NSG ordnungsgemäß blockiert wird.

## <a name="before-you-begin"></a>Voraussetzungen

Dieses Szenario setzt voraus, dass Sie die Schritte unter [Erstellen einer Network Watcher-Instanz](network-watcher-create.md) bereits durchgeführt haben, um eine Network Watcher-Instanz zu erstellen, bzw. dass Sie eine vorhandene Instanz von Network Watcher verwenden. Ferner wird davon ausgegangen, dass eine Ressourcengruppe mit einem gültigen virtuellen Computer vorhanden ist und verwendet werden kann.

## <a name="scenario"></a>Szenario

Dieses Szenario verwendet die IP-Datenflussüberprüfung, um zu überprüfen, ob ein virtueller Computer mit einer bekannten Bing-IP-Adresse kommunizieren kann. Wenn der Datenverkehr abgelehnt wird, wird die Sicherheitsregel zurückgegeben, die den jeweiligen Datenverkehr verweigert. Weitere Informationen zur IP-Datenflussüberprüfung finden Sie in der [Übersicht zur IP-Datenflussüberprüfung](network-watcher-ip-flow-verify-overview.md).

## <a name="retrieve-network-watcher"></a>Abrufen von Network Watcher

Der erste Schritt besteht im Abrufen der Network Watcher-Instanz. Die `$networkWatcher`-Variable wird an das Cmdlet zur IP-Datenflussüberprüfung übergeben.

```powershell
$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq "WestCentralUS" } 
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName 
```

## <a name="get-a-vm"></a>Abrufen eines virtuellen Computers

Die IP-Datenflussüberprüfung testet den bei einer IP-Adresse eines virtuellen Computers eingehenden oder ausgehenden Datenverkehr von einem bzw. an ein Remoteziel. Eine ID eines virtuellen Computers ist zum Ausführen des Cmdlets erforderlich. Wenn Sie die ID des virtuellen Computers bereits kennen, können Sie diesen Schritt überspringen.

```powershell
$VM = Get-AzurermVM -ResourceGroupName "testrg" -Name "testvm1"
```

## <a name="get-the-nics"></a>Abrufen der NICs

Die IP-Adresse einer NIC auf dem virtuellen Computer wird benötigt. In diesem Beispiel rufen wir die NICs auf einem virtuellen Computer ab. Wenn Sie die IP-Adresse bereits kennen, die Sie auf dem virtuellen Computer testen möchten, können Sie diesen Schritt überspringen.

```powershell
$Nics = Get-AzureRmNetworkInterface | Where {$_.Id -eq $vm.NetworkInterfaceIDs.ForEach({$_})}
```

## <a name="run-ip-flow-verify"></a>Ausführen der IP-Datenflussüberprüfung

Da wir jetzt die zum Ausführen des Cmdlets benötigten Informationen besitzen, führen wir das Cmdlet `Test-AzureRmNetworkWatcherIPFlow` aus, um den Datenverkehr zu testen. In diesem Beispiel verwenden wir die erste IP-Adresse der ersten NIC.

```powershell
Test-AzureRmNetworkWatcherIPFlow -NetworkWatcher $networkWatcher -TargetVirtualMachineId $VM.Id `
-Direction Outbound -Protocol TCP `
-LocalIPAddress $nics[0].IpConfigurations[0].PrivateIpAddress -LocalPort 6895 -RemoteIPAddress 204.79.197.200 -RemotePort 80
```

> [!NOTE]
> Für die IP-Datenflussüberprüfung ist es erforderlich, dass die VM-Ressource für die Ausführung zugeordnet wird.

## <a name="review-results"></a>Überprüfen der Ergebnisse

Nach der Ausführung von `Test-AzureRmNetworkWatcherIPFlow` werden die Ergebnisse zurückgegeben. Das folgende Beispiel zeigt die im vorhergehenden Schritt zurückgegebenen Ergebnisse.

```
Access RuleName                                  
------ --------                                  
Allow  defaultSecurityRules/AllowInternetOutBound
```

## <a name="next-steps"></a>Nächste Schritte

Wenn Datenverkehr fälschlicherweise blockiert wird, finden Sie unter [Verwalten von Netzwerksicherheitsgruppen](../virtual-network/virtual-network-manage-nsg-arm-portal.md) Informationen zum Ermitteln der Netzwerksicherheitsgruppe und der definierten Sicherheitsregeln.

[1]: ./media/network-watcher-check-ip-flow-verify-portal/figure1.png
[2]: ./media/network-watcher-check-ip-flow-verify-portal/figure2.png














