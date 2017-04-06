---
title: Migrieren zu Resource Manager mit PowerShell | Microsoft Docs
description: "In diesem Artikel wird die plattformgestützte Migration von IaaS-Ressourcen wie virtuellen Computern (VMs), virtuellen Netzwerken (VNETs) und Speicherkonten vom klassischen Bereitstellungsmodell zu Azure Resource Manager (ARM) mithilfe von Azure PowerShell-Befehlen erläutert."
services: virtual-machines-windows
documentationcenter: 
author: singhkays
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 2b3dff9b-2e99-4556-acc5-d75ef234af9c
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: kasing
translationtype: Human Translation
ms.sourcegitcommit: 1429bf0d06843da4743bd299e65ed2e818be199d
ms.openlocfilehash: 3f7a33f947913bf4b5ce9db20cacf746e4f7f169
ms.lasthandoff: 03/22/2017


---
# <a name="migrate-iaas-resources-from-classic-to-azure-resource-manager-by-using-azure-powershell"></a>Migrieren von IaaS-Ressourcen aus dem klassischen Bereitstellungsmodell zu Azure Resource Manager mithilfe von Azure PowerShell
Diese Schritte zeigen, wie Sie Azure PowerShell-Befehle zum Migrieren von IaaS-Ressourcen (Infrastructure as a Service) aus dem klassischen Bereitstellungsmodell in das Azure Resource Manager-Bereitstellungsmodell verwenden. 

Wenn Sie möchten, können Sie Ressourcen auch mithilfe der [Azure-Befehlszeilenschnittstelle](virtual-machines-linux-cli-migration-classic-resource-manager.md)migrieren.

* Hintergrundinformationen zu unterstützten Migrationsszenarien finden Sie unter [Plattformgestützte Migration von IaaS-Ressourcen vom klassischen Bereitstellungsmodell zu Azure Resource Manager](virtual-machines-windows-migration-classic-resource-manager.md). 
* Detaillierte Anleitungen und eine exemplarische Vorgehensweise zur Migration finden Sie unter [Ausführliche technische Informationen zur plattformgestützten Migration vom klassischen Bereitstellungsmodell zu Azure Resource Manager](virtual-machines-windows-migration-classic-resource-manager-deep-dive.md).
* [Überprüfen der häufigsten Fehler bei der Migration](virtual-machines-migration-errors.md)

<br>
Hier sehen Sie ein Flussdiagramm, das die Reihenfolge veranschaulicht, in der Schritte während einer Migration ausgeführt werden müssen.

![Screenshot that shows the migration steps](./media/virtual-machines-windows-migration-classic-resource-manager/migration-flow.png)

## <a name="step-1-plan-for-migration"></a>Schritt 1: Planen der Migration
Hier finden Sie einige bewährte Methoden, die wir empfehlen, wenn Sie eine Migration von IaaS-Ressourcen aus dem klassischen Bereitstellungsmodell zu Resource Manager in Erwägung ziehen:

* Lesen Sie die Informationen zu [unterstützten und nicht unterstützten Funktionen und Konfigurationen](virtual-machines-windows-migration-classic-resource-manager.md). Sollten Sie über virtuelle Computer mit nicht unterstützten Konfigurationen oder Features verfügen, empfiehlt es sich, auf die Ankündigung der Unterstützung der entsprechenden Features/Konfigurationen zu warten. Entfernen Sie alternativ ggf. die betroffenen Features, oder verwenden Sie eine andere Konfiguration, um die Migration zu ermöglichen.
* Wenn Sie derzeit über automatisierte Skripts zum Bereitstellen Ihrer Infrastruktur und Anwendungen verfügen, versuchen Sie, mithilfe dieser Skripts für die Migration eine ähnliche Testeinrichtung zu erstellen. Alternativ dazu können Sie über das Azure-Portal Beispielumgebungen einrichten.

> [!IMPORTANT]
> Application Gateways werden für die Migration vom klassischen zum Resource Manager-Modell derzeit nicht unterstützt. Entfernen Sie zum Migrieren eines klassischen virtuellen Netzwerks mit einem Application Gateway das Gateway, bevor Sie einen Commitvorgang zum Verschieben des Netzwerks durchführen. (Sie können den Vorbereitungsschritt ausführen, ohne das Application Gateway zu löschen.) Stellen Sie nach Abschluss der Migration die Verbindung zum Gateway im Azure Resource Manager wieder her. Sie müssen, wenn Sie ExpressRoute-Gateways migrieren möchten, den Support bei Fällen kontaktieren, bei denen sich das Gateway und die ExpressRoute-Verbindung im selben Abonnement befinden. ExpressRoute-Gateways, die sich mit ExpressRoute-Verbindungen in einem anderen Abonnement verbinden, können nicht migriert werden. Entfernen Sie in solchen Fällen das ExpressRoute-Gateway, migrieren Sie das virtuelle Netzwerk, und erstellen Sie das Gateway neu.
> 
> 

## <a name="step-2-install-the-latest-version-of-azure-powershell"></a>Schritt 2: Installieren der neuesten Version von Azure PowerShell
Es gibt zwei Hauptoptionen für die Installation von Azure PowerShell: [PowerShell-Katalog](https://www.powershellgallery.com/profiles/azure-sdk/) und [Webplattform-Installer (WebPI)](http://aka.ms/webpi-azps). WebPI empfängt monatliche Updates. Der PowerShell-Katalog wird fortlaufend aktualisiert. Dieser Artikel basiert auf Azure PowerShell Version 2.1.0.

Installationsanweisungen finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azureps-cmdlets-docs).

<br>

## <a name="step-3-ensure-that-you-are-co-administrator-for-the-subscription-in-azure-classic-portal"></a>Schritt 3: Stellen Sie sicher, dass Sie Co-Administrator für das Abonnement im klassischen Azure-Portal sind
Um diese Migration durchführen zu können, müssen Sie im [klassischen Azure-Portal](https://manage.windowsazure.com/) als Co-Administrator für das Abonnement hinzugefügt werden. Dies ist auch erforderlich, wenn Sie im [Azure-Portal](https://portal.azure.com) bereits als Besitzer hinzugefügt wurden. Versuchen Sie, [einen Co-Administrator für das Abonnement im klassischen Azure-Portal hinzuzufügen](../billing/billing-add-change-azure-subscription-administrator.md), um herauszufinden, ob Sie Co-Administrator für das Abonnement sind. Wenn Sie keinen Co-Administrator hinzufügen können, wenden Sie sich bitte an einen Dienstadministrator oder Co-Administrator für das Abonnement, um sich hinzufügen zu lassen.   

## <a name="step-4-set-your-subscription-and-sign-up-for-migration"></a>Schritt 4: Festlegen Ihres Abonnements und Registrieren für die Migration
Starten Sie zunächst eine PowerShell-Eingabeaufforderung. Für die Migration müssen Sie Ihre Umgebung sowohl für das klassische Bereitstellungsmodell als auch für Resource Manager einrichten.

Melden Sie sich an Ihrem Konto für das Resource Manager-Modell an.

```powershell
    Login-AzureRmAccount
```

Rufen Sie die verfügbaren Abonnements mit dem folgenden Befehl ab:

```powershell
    Get-AzureRMSubscription | Sort SubscriptionName | Select SubscriptionName
```

Legen Sie Ihr Azure-Abonnement für die aktuelle Sitzung fest. In diesem Beispiel wird der Abonnementname standardmäßig auf **My Azure Subscription** festgelegt. Ersetzen Sie den Abonnementnamen im Beispiel durch den Namen Ihres eigenen Abonnements. 

```powershell
    Select-AzureRmSubscription –SubscriptionName "My Azure Subscription"
```

> [!NOTE]
> Die Registrierung ist ein einmaliger Schritt, der jedoch einmal ausgeführt werden muss, bevor Sie versuchen, die Migration auszuführen. Ohne Registrierung wird die folgende Fehlermeldung angezeigt: 
> 
> *BadRequest: Subscription is not registered for migration.* (BadRequest: Das Abonnement ist nicht für die Migration registriert.) 
> 
> 

Registrieren Sie sich mithilfe des folgenden Befehls beim Migrationsressourcenanbieter:

```powershell
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.ClassicInfrastructureMigrate
```

Der Abschluss der Registrierung kann bis zu fünf Minuten dauern. Der Genehmigungsstatus kann mithilfe des folgenden Befehls geprüft werden:

```powershell
    Get-AzureRmResourceProvider -ProviderNamespace Microsoft.ClassicInfrastructureMigrate
```

Stellen Sie sicher, dass der RegistrationState-Wert `Registered` lautet, bevor Sie fortfahren. 

Melden Sie sich jetzt an Ihrem Konto für das klassische Modell an.

```powershell
    Add-AzureAccount
```

Rufen Sie die verfügbaren Abonnements mit dem folgenden Befehl ab:

```powershell
    Get-AzureSubscription | Sort SubscriptionName | Select SubscriptionName
```

Legen Sie Ihr Azure-Abonnement für die aktuelle Sitzung fest. In diesem Beispiel wird das Abonnement standardmäßig auf **My Azure Subscription** festgelegt. Ersetzen Sie den Abonnementnamen im Beispiel durch den Namen Ihres eigenen Abonnements. 

```powershell
    Select-AzureSubscription –SubscriptionName "My Azure Subscription"
```

<br>

## <a name="step-5-make-sure-you-have-enough-azure-resource-manager-virtual-machine-cores-in-the-azure-region-of-your-current-deployment-or-vnet"></a>Schritt 5: Sicherstellen, dass Sie über genügend Kerne in virtuellen Azure Resource Manager-Computern in der Azure-Region Ihrer aktuellen Bereitstellung oder Ihres VNET verfügen
Mit dem folgenden PowerShell-Befehl können Sie die aktuelle Anzahl von Kernen in Azure Resource Manager überprüfen. Weitere Informationen zu Kernkontingenten finden Sie unter [Grenzwerte und Azure Resource Manager](../azure-subscription-service-limits.md#limits-and-the-azure-resource-manager). 

In diesem Beispiel wird die Verfügbarkeit in der Region **USA, Westen** geprüft. Ersetzen Sie den Regionsnamen im Beispiel durch den Namen Ihrer eigenen Region. 

```powershell
Get-AzureRmVMUsage -Location "West US"
```

## <a name="step-6-run-commands-to-migrate-your-iaas-resources"></a>Schritt 6: Ausführen von Befehlen zum Migrieren Ihrer IaaS-Ressourcen
> [!NOTE]
> Alle hier beschriebenen Vorgänge sind idempotent. Sollte ein Problem auftreten, das nicht auf ein nicht unterstütztes Feature oder auf einen Konfigurationsfehler zurückzuführen ist, wiederholen Sie den Vorbereitungs-, Abbruch- oder Commitvorgang. Die Plattform versucht dann erneut, die Aktion auszuführen.
> 
> 

## <a name="step-61-migrate-virtual-machines-in-a-cloud-service-not-in-a-virtual-network"></a>Schritt 6.1: Migrieren virtueller Computer in einem Clouddienst (nicht in einem virtuellen Netzwerk)
Rufen Sie mithilfe des folgenden Befehls die Liste mit den Clouddiensten auf, und wählen Sie anschließend den zu migrierenden Clouddienst aus. Falls sich die virtuellen Computer im Clouddienst in einem virtuellen Netzwerk befinden oder über Web- oder Workerrollen verfügen, gibt der Befehl eine Fehlermeldung zurück.

```powershell
    Get-AzureService | ft Servicename
```

Rufen Sie den Bereitstellungsnamen für den Clouddienst ab. In diesem Beispiel lautet der Dienstname **My Service**. Ersetzen Sie den Dienstnamen im Beispiel durch den Namen Ihres eigenen Diensts. 

```powershell
    $serviceName = "My Service"
    $deployment = Get-AzureDeployment -ServiceName $serviceName
    $deploymentName = $deployment.DeploymentName
```

Bereiten Sie die virtuellen Computer des Clouddiensts auf die Migration vor. Dabei stehen Ihnen zwei Optionen zur Auswahl.

* **Option 1. Migrieren der virtuellen Computer in ein von der Plattform erstelltes virtuelles Netzwerk**
  
    Überprüfen Sie zuerst, ob Sie den Clouddienst mithilfe des folgenden Befehls migrieren können:
  
    ```powershell
    $validate = Move-AzureService -Validate -ServiceName $serviceName `
        -DeploymentName $deploymentName -CreateNewVirtualNetwork
    $validate.ValidationMessages
    ```
  
    Der vorhergehende Befehl zeigt ggf. Warnungen und Fehler an, die die Migration blockieren. Wenn die Überprüfung erfolgreich ist, können Sie mit dem Schritt **Vorbereiten** fortfahren:
  
    ```powershell
    Move-AzureService -Prepare -ServiceName $serviceName `
        -DeploymentName $deploymentName -CreateNewVirtualNetwork
    ```
* **Option 2. Migrieren zu einem vorhandenen virtuellen Netzwerk im Resource Manager-Bereitstellungsmodell**
  
    In diesem Beispiel wird der Name der Ressourcengruppe auf **myResourceGroup**, der Name des virtuellen Netzwerks auf **myVirtualNetwork** und der Subnetzname auf **mySubNet** festgelegt. Ersetzen Sie die Namen im Beispiel durch die Namen Ihrer eigenen Ressourcen.
  
    ```powershell
    $existingVnetRGName = "myResourceGroup"
    $vnetName = "myVirtualNetwork"
    $subnetName = "mySubNet"
    ```
  
    Überprüfen Sie zuerst, ob Sie den Clouddienst mithilfe des folgenden Befehls migrieren können:
  
    ```powershell
    $validate = Move-AzureService -Validate -ServiceName $serviceName `
        -DeploymentName $deploymentName -UseExistingVirtualNetwork -VirtualNetworkResourceGroupName $existingVnetRGName -VirtualNetworkName $vnetName -SubnetName $subnetName
    $validate.ValidationMessages
    ```
  
    Der vorhergehende Befehl zeigt ggf. Warnungen und Fehler an, die die Migration blockieren. Wenn die Überprüfung erfolgreich ist, können Sie mit dem folgenden Vorbereitungsschritt fortfahren:
  
    ```powershell
    Move-AzureService -Prepare -ServiceName $serviceName -DeploymentName $deploymentName `
        -UseExistingVirtualNetwork -VirtualNetworkResourceGroupName $existingVnetRGName `
        -VirtualNetworkName $vnetName -SubnetName $subnetName
    ```

Nachdem der Vorbereitungsvorgang mit einer der oben beschriebenen Optionen erfolgreich abgeschlossen wurde, fragen Sie den Migrationsstatus der virtuellen Computer ab. Stellen Sie sicher, dass sie sich im Status `Prepared` befinden.

In diesem Beispiel wird der Name des virtuellen Computers auf **myVM** festgelegt. Ersetzen Sie den Namen im Beispiel durch den Namen Ihrer eigenen VM.

    ```powershell
    $vmName = "myVM"
    $vm = Get-AzureVM -ServiceName $serviceName -Name $vmName
    $vm.VM.MigrationState
    ```

Überprüfen Sie die Konfiguration der vorbereiteten Ressourcen mithilfe von PowerShell oder des Azure-Portals. Wenn Sie noch nicht für die Migration bereit sind und zum alten Zustand zurückkehren möchten, verwenden Sie den folgenden Befehl:

```powershell
    Move-AzureService -Abort -ServiceName $serviceName -DeploymentName $deploymentName
```

Wenn die vorbereitete Konfiguration in Ordnung ist, können Sie den Vorgang fortsetzen und mithilfe des folgenden Befehls ein Commit für die Ressourcen ausführen:

```powershell
    Move-AzureService -Commit -ServiceName $serviceName -DeploymentName $deploymentName
```

## <a name="step-62-migrate-virtual-machines-in-a-virtual-network"></a>Schritt 6.2: Migrieren virtueller Computer in einem virtuellen Netzwerk
Um virtuelle Computer in einem virtuellen Netzwerk zu migrieren, migrieren Sie das virtuelle Netzwerk. Die virtuellen Computer werden automatisch zusammen mit dem virtuellen Netzwerk migriert. Wählen Sie das virtuelle Netzwerk aus, das Sie migrieren möchten. 
> [!NOTE]
> [Migrieren Sie einzelne klassische virtuelle Computer](./virtual-machines-windows-migrate-single-classic-to-resource-manager.md) durch Erstellen einer neuen Resource Manager-VM mit Managed Disks unter Verwendung der VHD-Dateien (Betriebssystem und Daten) des virtuellen Computers. 

In diesem Beispiel wird der Name des virtuellen Netzwerks auf **myVnet** festgelegt. Ersetzen Sie den Namen des virtuellen Netzwerks im Beispiel durch den Namen Ihres eigenen virtuellen Netzwerks. 

```powershell
    $vnetName = "myVnet"
```

> [!NOTE]
> Wenn das virtuelle Netzwerk Web- oder Workerrollen oder virtuelle Computer mit nicht unterstützten Konfigurationen enthält, tritt ein Überprüfungsfehler auf.
> 
> 

Überprüfen Sie zuerst mithilfe des folgenden Befehls, ob Sie das virtuelle Netzwerk migrieren können:

```powershell
    Move-AzureVirtualNetwork -Validate -VirtualNetworkName $vnetName
```

Der vorhergehende Befehl zeigt ggf. Warnungen und Fehler an, die die Migration blockieren. Wenn die Überprüfung erfolgreich ist, können Sie mit dem folgenden Vorbereitungsschritt fortfahren:

```powershell
    Move-AzureVirtualNetwork -Prepare -VirtualNetworkName $vnetName
```

Überprüfen Sie die Konfiguration der vorbereiteten virtuellen Computer mithilfe von Azure PowerShell oder über das Azure-Portal. Wenn Sie noch nicht für die Migration bereit sind und zum alten Zustand zurückkehren möchten, verwenden Sie den folgenden Befehl:

```powershell
    Move-AzureVirtualNetwork -Abort -VirtualNetworkName $vnetName
```

Wenn die vorbereitete Konfiguration in Ordnung ist, können Sie den Vorgang fortsetzen und mithilfe des folgenden Befehls ein Commit für die Ressourcen ausführen:

```powershell
    Move-AzureVirtualNetwork -Commit -VirtualNetworkName $vnetName
```

## <a name="step-63-migrate-a-storage-account"></a>Schritt 6.3: Migrieren eines Speicherkontos
Sobald Sie mit der Migration der virtuellen Computer fertig sind, sollten Sie die Speicherkonten migrieren.

Bevor Sie das Speicherkonto migrieren, führen Sie Voraussetzungsprüfungen durch:

* **Migrieren klassischer virtueller Computer, deren Datenträger im Speicherkonto gespeichert sind**

    Der obige Befehl gibt die Eigenschaften RoleName und DiskName aller klassischen VM-Datenträger im Speicherkonto zurück. RoleName ist der Name des virtuellen Computers, dem ein Datenträger angefügt ist. Wenn der obige Befehl Datenträger zurückgibt, stellen Sie sicher, dass virtuelle Computer, denen diese Datenträger angefügt sind, vor der Migration des Speicherkontos migriert werden.
    ```powershell
     $storageAccountName = 'yourStorageAccountName'
      Get-AzureDisk | where-Object {$_.MediaLink.Host.Contains($storageAccountName)} | Select-Object -ExpandProperty AttachedTo -Property `
      DiskName | Format-List -Property RoleName, DiskName 

    ```
* **Löschen nicht angefügter klassischer, im Speicherkonto gespeicherter VM-Datenträger**
 
    Mit folgendem Befehl finden Sie nicht angefügte klassische, im Speicherkonto gespeicherte VM-Datenträger: 

    ```powershell
        $storageAccountName = 'yourStorageAccountName'
        Get-AzureDisk | where-Object {$_.MediaLink.Host.Contains($storageAccountName)} | Format-List -Property DiskName  

    ```
    Wenn der obige Befehl Datenträger zurückgibt, dann löschen Sie diese Datenträger mit folgendem Befehl:

    ```powershell
       Remove-AzureDisk -DiskName 'yourDiskName'
    ```
* **Löschen von VM-Images, die im Speicherkonto gespeichert sind**

    Der obige Befehl gibt alle VM-Images mit Betriebssystemdatenträgern zurück, die im Speicherkonto gespeichert sind.
     ```powershell
        Get-AzureVmImage | Where-Object { $_.OSDiskConfiguration.MediaLink -ne $null -and $_.OSDiskConfiguration.MediaLink.Host.Contains($storageAccountName)`
                                } | Select-Object -Property ImageName, ImageLabel
     ```
     Der obige Befehl gibt alle VM-Images mit Datenträgern für Daten zurück, die im Speicherkonto gespeichert sind.
     ```powershell

        Get-AzureVmImage | Where-Object {$_.DataDiskConfigurations -ne $null `
                                         -and ($_.DataDiskConfigurations | Where-Object {$_.MediaLink -ne $null -and $_.MediaLink.Host.Contains($storageAccountName)}).Count -gt 0 `
                                        } | Select-Object -Property ImageName, ImageLabel
     ```
    Löschen Sie alle VM-Images, die von den vorherigen Befehlen zurückgegeben wurden, mit folgendem Befehl:
    ```powershell
    Remove-AzureVMImage -ImageName 'yourImageName'
    ```
    
Bereiten Sie jedes Speicherkonto mithilfe des folgenden Befehls für die Migration vor. In diesem Beispiel lautet der Name des Speicherkontos **MyStorageAccount**. Ersetzen Sie den Namen im Beispiel durch den Namen Ihres eigenen Speicherkontos. 

```powershell
    $storageAccountName = "myStorageAccount"
    Move-AzureStorageAccount -Prepare -StorageAccountName $storageAccountName
```

Überprüfen Sie die Konfiguration des vorbereiteten Speicherkontos entweder mit Azure PowerShell oder im Azure-Portal. Wenn Sie noch nicht für die Migration bereit sind und zum alten Zustand zurückkehren möchten, verwenden Sie den folgenden Befehl:

```powershell
    Move-AzureStorageAccount -Abort -StorageAccountName $storageAccountName
```

Wenn die vorbereitete Konfiguration in Ordnung ist, können Sie den Vorgang fortsetzen und mithilfe des folgenden Befehls ein Commit für die Ressourcen ausführen:

```powershell
    Move-AzureStorageAccount -Commit -StorageAccountName $storageAccountName
```

## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zur Migration finden Sie unter [Plattformgestützte Migration von IaaS-Ressourcen vom klassischen Bereitstellungsmodell zu Azure Resource Manager](virtual-machines-windows-migration-classic-resource-manager.md).
* Um weitere Netzwerkressourcen mithilfe von Azure PowerShell zu Resource Manager zu migrieren, verwenden Sie ähnliche Schritte mit [Move-AzureNetworkSecurityGroup](https://msdn.microsoft.com/library/mt786729.aspx), [Move-AzureReservedIP](https://msdn.microsoft.com/library/mt786752.aspx) und [Move-AzureRouteTable](https://msdn.microsoft.com/library/mt786718.aspx).
* Open Source-Skripts für die Migration von Azure-Ressourcen vom klassischen Bereitstellungsmodell zu Resource Manager finden Sie unter [Communitytools für die Migration zu Azure Resource Manager](virtual-machines-windows-migration-scripts.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)


