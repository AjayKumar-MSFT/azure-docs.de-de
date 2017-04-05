---
title: "Häufig gestellte Fragen zu Azure DevTest Labs | Microsoft Docs"
description: "Hier finden Sie Antworten auf häufig gestellte Fragen zu Azure DevTest Labs."
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: afe83109-b89f-4f18-bddd-b8b4a30f11b4
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/22/2017
ms.author: tarcher
translationtype: Human Translation
ms.sourcegitcommit: 503f5151047870aaf87e9bb7ebf2c7e4afa27b83
ms.openlocfilehash: 5c427ddbe408fc42403eb6738d1983c220e899a7
ms.lasthandoff: 03/28/2017


---
# <a name="azure-devtest-labs-faq"></a>Häufig gestellte Fragen zu Azure DevTest Labs
In diesem Artikel werden einige der am häufigsten gestellten Fragen zu Azure DevTest Labs beantwortet.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="general"></a>Allgemein
* [Was kann ich tun, wenn meine Frage hier nicht beantwortet wird?](#what-if-my-question-isnt-answered-here)
* [Warum sollte ich Azure DevTest Labs verwenden?](#why-should-i-use-azure-devtest-labs)
* [Was bedeutet „Self-Service für sorgenfreies Arbeiten“?](#what-does-worry-free-self-service-mean)
* [Wie kann ich Azure DevTest Labs verwenden?](#how-can-i-use-azure-devtest-labs)
* [Wie wird die Nutzung von Azure DevTest Labs abgerechnet?](#how-am-i-billed-for-azure-devtest-labs)

## <a name="security"></a>Sicherheit
* [Wie lauten die verschiedenen Sicherheitsstufen in Azure DevTest Labs?](#what-are-the-different-security-levels-in-azure-devtest-labs)
* [Wie erstelle ich eine bestimmte Rolle, um Benutzern das Ausführen einer einzelnen Aufgabe zu ermöglichen?](#how-do-i-create-a-role-to-allow-users-to-perform-a-specific-task)

## <a name="cicd-integration--automation"></a>CI/CD-Integration und Automatisierung
* [Lässt sich Azure DevTest Labs in meine CI/CD-Toolkette integrieren?](#does-azure-devtest-labs-integrate-with-my-cicd-toolchain)

## <a name="virtual-machines"></a>Virtual Machines
* [Warum werden bestimmte VMs, die in Azure DevTest Labs angezeigt werden, nicht auf dem Blatt „Virtuelle Azure-Computer“ angezeigt?](#why-cant-i-see-certain-vms-in-the-azure-virtual-machines-blade-that-i-see-within-azure-devtest-labs)
* [Was ist der Unterschied zwischen benutzerdefinierten Images und Formeln?](#what-is-the-difference-between-custom-images-and-formulas)
* [Wie kann ich mehrere VMs gleichzeitig anhand derselben Vorlage erstellen?](#how-do-i-create-multiple-vms-from-the-same-template-at-once)
* [Wie verschiebe ich meine vorhandenen Azure-VMs in mein Azure DevTest Labs-Lab?](#how-do-i-move-my-existing-azure-vms-into-my-azure-devtest-labs-lab)
* [Kann ich mehrere Datenträger an meine VMs anfügen?](#can-i-attach-multiple-disks-to-my-vms)
* [Wenn ich ein Windows-Betriebssystemimage für meine Tests verwenden möchte, muss ich dann ein MSDN-Abonnement erwerben?](#if-i-want-to-use-a-windows-os-image-for-my-testing-do-i-have-to-purchase-an-msdn-subscription)
* [Wie automatisiere ich das Hochladen von VHD-Dateien zum Erstellen benutzerdefinierter Images?](#how-do-i-automate-the-process-of-uploading-vhd-files-to-create-custom-images)
* [Wie kann ich den Löschvorgang für alle virtuellen Computer in meinem Lab automatisieren?](#how-can-i-automate-the-process-of-deleting-all-the-vms-in-my-lab)

## <a name="artifacts"></a>Artefakte
* [Was sind Artefakte?](#what-are-artifacts)

## <a name="lab-configuration"></a>Labkonfiguration
* [Wie erstelle ich ein Lab anhand einer Azure Resource Manager-Vorlage?](#how-do-i-create-a-lab-from-an-azure-resource-manager-template)
* [Warum werden meine VMs in verschiedenen Ressourcengruppen mit willkürlichen Namen erstellt? Kann ich diese Ressourcengruppen umbenennen oder ändern?](#why-are-my-vms-created-in-different-resource-groups-with-arbitrary-names-can-i-rename-or-modify-these-resource-groups)
* [Wie viele Labs kann ich unter demselben Abonnement erstellen?](#how-many-labs-can-i-create-under-the-same-subscription)
* [Wie viele VMs kann ich pro Lab erstellen?](#how-many-vms-can-i-create-per-lab)
* [Wie gebe ich einen direkten Link zu meinem Lab frei?](#how-do-i-share-a-direct-link-to-my-lab)
* [Was ist ein Microsoft-Konto?](#what-is-a-microsoft-account)

## <a name="troubleshooting"></a>Problembehandlung
* [Für mein Artefakt ist während der VM-Erstellung ein Fehler aufgetreten. Wie kann ich das Problem beheben?](#my-artifact-failed-during-vm-creation-how-do-i-troubleshoot-it)
* [Warum wird mein vorhandenes virtuelles Netzwerk nicht korrekt gespeichert?](#why-isnt-my-existing-virtual-network-saving-properly)
* [Warum erhalte ich den Fehler „Übergeordnete Ressource wurde nicht gefunden“ bei der Bereitstellung von PowerShell?](#why-do-i-get-a-parent-resource-not-found-error-when-provisioning-a-vm-from-powershell)  
* [Wo finde ich weitere Informationen zu Fehlern bei einer VM-Bereitstellung?](#where-can-i-find-more-error-information-if-a-vm-deployment-fails)  

### <a name="what-if-my-question-isnt-answered-here"></a>Was kann ich tun, wenn meine Frage hier nicht beantwortet wird?
Wenn Ihre Frage hier nicht aufgeführt wird, informieren Sie uns, damit wir Ihnen helfen können, eine Antwort zu finden.

* Stellen Sie eine Frage im [Disqus-Thread](#comments) am Ende dieser FAQ, und diskutieren Sie mit dem Azure Cache-Team und anderen Mitgliedern der Community über diesen Artikel.
* Um eine größere Benutzergruppe zu erreichen, stellen Sie eine Frage im [MSDN-Forum zu Azure DevTest Labs](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureDevTestLabs), und diskutieren mit dem Azure DevTest Labs-Team und anderen Mitgliedern der Community.
* Um ein neues Feature vorzuschlagen, übermitteln Sie Ihre Anfragen und Ideen in [Azure DevTest Labs User Voice](https://feedback.azure.com/forums/320373-azure-devtest-labs).

### <a name="why-should-i-use-azure-devtest-labs"></a>Warum sollte ich Azure DevTest Labs verwenden?
Mit Azure DevTest Labs kann Ihr Team Zeit und Geld sparen. Entwickler können mit verschiedenen Basiskonfigurationen eigene Umgebungen erstellen und Artefakte zum schnellen Bereitstellen und Konfigurieren von Anwendungen verwenden. Mithilfe von benutzerdefinierten Images und Formeln können virtuelle Computer als Vorlagen gespeichert und mühelos reproduziert werden. Darüber hinaus bieten Labs mehrere konfigurierbare Richtlinien, mit denen Labadministratoren die Vergeudung von Ressourcen reduzieren und die Umgebungen eines Teams verwalten können. Diese Richtlinien beinhalten u.a. das automatische Herunterfahren, Kostenschwellenwerte, eine maximale Anzahl von VMs pro Benutzer und maximale VM-Größen. Eine ausführlichere Erläuterung von Azure DevTest Labs finden Sie in der [Übersicht](devtest-lab-overview.md) oder im [Einführungsvideo](/documentation/videos/videos/what-is-azure-devtest-labs).

### <a name="what-does-worry-free-self-service-mean"></a>Was bedeutet „Self-Service für sorgenfreies Arbeiten“?
„Self-Service für sorgenfreies Arbeiten“ bedeutet, dass die Entwickler und Tester nach Bedarf eigene Umgebungen erstellen können. Die Administratoren wissen dabei genau, dass bei Azure DevTest Labs keine Ressourcen vergeudet werden und dass sie die Kosten vollständig kontrollieren können. Administratoren können die zulässigen VM-Größen und die maximale Anzahl von VMs angeben und festlegen, wann VMs gestartet und beendet werden. Azure DevTest Labs erleichtert auch die Überwachung der Kosten und das Festlegen von Warnungen, sodass sie stets wissen, wie Ressourcen im Lab verwendet werden.

### <a name="how-can-i-use-azure-devtest-labs"></a>Wie kann ich Azure DevTest Labs verwenden?
Azure DevTest Labs eignet sich, wenn Sie Entwicklungs- oder Testumgebungen benötigen und diese schnell reproduzieren und/oder mit kostensparenden Richtlinien verwalten möchten.

Unsere Kunden verwenden Azure DevTest Labs beispielsweise für folgende Szenarien:

* Verwaltung von Entwicklungs- und Testumgebungen an einer zentralen Stelle mithilfe von Richtlinien zum Reduzieren der Kosten und benutzerdefinierten Images zum Freigeben von Builds innerhalb des Teams
* Entwicklung von Anwendungen mit benutzerdefinierten Images, um den Datenträgerzustand in den Entwicklungsphasen zu speichern
* Nachverfolgung der Kosten im Verhältnis zum Status
* Erstellung von Massentestumgebungen für Qualitätssicherungstests
* Verwendung von Artefakten und Formeln zum einfachen Konfigurieren und Reproduzieren von Anwendungen für verschiedene Umgebungen
* Verteilung von VMs für Hackathons (gemeinschaftliche Entwicklungs- oder Testarbeiten) und einfache Aufhebung der Bereitstellung nach Beendigung des Ereignisses

### <a name="how-am-i-billed-for-azure-devtest-labs"></a>Wie wird die Nutzung von Azure DevTest Labs abgerechnet?
Azure DevTest Labs ist ein kostenloser Service. Für das Erstellen von Labs sowie das Konfigurieren der Richtlinien, Vorlagen und Artefakte fallen keinerlei Kosten an. Sie bezahlen nur für die Azure-Ressourcen, die Sie in den Labs verwenden, beispielsweise virtuelle Computer, Speicherkonten und virtuelle Netzwerke. Informieren Sie sich über die [Preise von Azure DevTest Labs](https://azure.microsoft.com/pricing/details/devtest-lab/), um mehr über die Kosten von Labressourcen zu erfahren.

### <a name="what-are-the-different-security-levels-in-azure-devtest-labs"></a>Wie lauten die verschiedenen Sicherheitsstufen in Azure DevTest Labs?
Der Sicherheitszugriff wird durch die [rollenbasierte Zugriffssteuerung von Azure (Role-Based Access Control, RBAC)](../active-directory/role-based-access-built-in-roles.md)bestimmt. Die Funktionsweise des Zugriffs ist leichter zu verstehen, wenn Sie die Unterschiede zwischen Berechtigungen, Rollen und Bereichen in RBAC kennen.

* **Berechtigung** : Eine Berechtigung ermöglicht einen definierten Zugriff auf eine bestimmte Aktion. Zum Beispiel kann eine Berechtigung den Lesezugriff auf alle virtuellen Computer ermöglichen.
* **Rolle** : Eine Rolle ist ein Satz von Berechtigungen, die gruppiert und einem Benutzer zugewiesen werden können. Zum Beispiel verfügt die Rolle „Abonnementbesitzer“ über Zugriff auf alle Ressourcen in einem Abonnement.
* **Bereich** : Ein Bereich ist eine Stufe innerhalb der Hierarchie einer Azure-Ressource. Zum Beispiel kann eine Ressourcengruppe oder ein einzelnes Lab oder das gesamte Abonnement ein Bereich sein.

Innerhalb des Bereichs von Azure DevTest Labs gibt es zwei Arten von Rollen, mit denen Benutzerberechtigungen definiert werden: Labbesitzer und Labbenutzer.

* **Labbesitzer** : Ein Labbesitzer verfügt über Zugriff auf alle Ressourcen innerhalb des Labs. Daher kann er Richtlinien und das virtuelle Netzwerk ändern, verfügt über Lese- und Schreibberechtigungen für alle virtuellen Computer usw.
* **Labbenutzer:** Ein Labbenutzer kann alle Labressourcen anzeigen (z.B. VMs, Richtlinien und virtuelle Netzwerke), aber keine Richtlinien oder von anderen Benutzern erstellte virtuelle Computer ändern. Es ist auch möglich, benutzerdefinierte Rollen in Azure DevTest Labs zu erstellen. Informationen dazu finden Sie im Artikel [Gewähren von Benutzerberechtigungen für bestimmte Labrichtlinien](devtest-lab-grant-user-permissions-to-specific-lab-policies.md).

Da Bereiche hierarchisch sind, gelten die Berechtigungen, die einem Benutzer für einen bestimmten Bereich gewährt werden, automatisch für alle darin enthaltenen Bereiche auf niedrigerer Ebene. Ist einem Benutzer beispielsweise die Rolle „Abonnementbesitzer“ zugewiesen, hat er Zugriff auf alle Ressourcen in einem Abonnement. Zu diesen Ressourcen gehören alle virtuellen Computer, alle virtuellen Netzwerke und alle Labs. Der Abonnementbesitzer erbt somit automatisch die Rolle des Labbesitzers. Umgekehrt ist dies jedoch nicht der Fall. Ein Labbesitzer hat Zugriff auf ein Lab, d.h. auf einen Bereich, der sich innerhalb der Hierarchie auf einer niedrigeren Ebene befindet als das Abonnement. Aus diesem Grund kann ein Labbesitzer keine virtuellen Computer, virtuellen Netzwerke oder anderen Ressourcen außerhalb des Labs anzeigen.

### <a name="how-do-i-create-a-role-to-allow-users-to-perform-a-specific-task"></a>Wie erstelle ich eine bestimmte Rolle, um Benutzern das Ausführen einer einzelnen Aufgabe zu ermöglichen?
Einen ausführlichen Artikel zum Erstellen von benutzerdefinierten Rollen und Zuweisen von Berechtigungen zu dieser Rolle finden Sie hier. Das folgende Beispielskript erstellt die Rolle „DevTest Labs Advanced User“, die über die Berechtigung zum Starten und Beenden alle VMs im Lab verfügt:

    $policyRoleDef = Get-AzureRmRoleDefinition "DevTest Labs User"
    $policyRoleDef.Actions.Remove('Microsoft.DevTestLab/Environments/*')
    $policyRoleDef.Id = $null
    $policyRoleDef.Name = "DevTest Labs Advance User"
    $policyRoleDef.IsCustom = $true
    $policyRoleDef.AssignableScopes.Clear()
    $policyRoleDef.AssignableScopes.Add("subscriptions/<subscription Id>")
    $policyRoleDef.Actions.Add("Microsoft.DevTestLab/labs/virtualMachines/Start/action")
    $policyRoleDef.Actions.Add("Microsoft.DevTestLab/labs/virtualMachines/Stop/action")
    $policyRoleDef = New-AzureRmRoleDefinition -Role $policyRoleDef  

### <a name="does-azure-devtest-labs-integrate-with-my-cicd-toolchain"></a>Lässt sich Azure DevTest Labs in meine CI/CD-Toolkette integrieren?
Wenn Sie Visual Studio Team Services (VSTS) verwenden, ist eine [Azure DevTest Labs-Erweiterung für Aufgaben](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks) verfügbar, mit der Sie Ihre Releasepipeline in Azure DevTest Labs automatisieren können. Die Erweiterung kann beispielsweise für folgende Zwecke verwendet werden:

* Automatisches Erstellen und Bereitstellen einer VM und Konfigurieren der VM mit dem aktuellen Build mithilfe des Azure-Dateikopiervorgangs oder von PowerShell-VSTS-Aufgaben
* Automatisches Erfassen des Zustands einer VM nach dem Testen, um zur weiteren Untersuchung einen Fehler auf derselben VM zu reproduzieren
* Löschen der VM am Ende der Releasepipeline, wenn sie nicht mehr benötigt wird

Die folgenden Blogbeiträge enthalten Anleitungen und Informationen zur Verwendung der VSTS-Erweiterung:

* [Azure DevTest Labs – VSTS extension (Azure DevTest Labs – VSTS-Erweiterung)](https://blogs.msdn.microsoft.com/devtestlab/2016/06/15/azure-devtest-labs-vsts-extension/)
* [Deploying a new VM in an existing AzureDevTestLab from VSTS (Bereitstellen einer neuen VM in einem vorhandenen AzureDevTestLab über VSTS)](http://www.visualstudiogeeks.com/blog/DevOps/Deploy-New-VM-To-Existing-AzureDevTestLab-From-VSTS)
* [Using VSTS Release Management for Continuous Deployments to AzureDevTestLabs (Verwenden der VSTS-Releaseverwaltung für kontinuierliche Bereitstellungen in AzureDevTestLabs)](http://www.visualstudiogeeks.com/blog/DevOps/Use-VSTS-ReleaseManagement-to-Deploy-and-Test-in-AzureDevTestLabs)

Für andere CI/CD-Toolketten können alle der oben genannten Szenarien, die mit der Erweiterung für VSTS-Aufgaben möglich sind, auf ähnliche Weise durch die Bereitstellung von [Azure Resource Manager-Vorlagen](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates) mit [Azure PowerShell-Cmdlets](../azure-resource-manager/resource-group-template-deploy.md) und [.NET SDKs](https://www.nuget.org/packages/Microsoft.Azure.Management.DevTestLabs/) umgesetzt werden. Sie können auch [REST-APIs für DevTest Labs](http://aka.ms/dtlrestapis) zur Integration in Ihre Toolkette verwenden.  

### <a name="why-cant-i-see-certain-vms-in-the-azure-virtual-machines-blade-that-i-see-within-azure-devtest-labs"></a>Warum werden bestimmte VMs, die in Azure DevTest Labs angezeigt werden, nicht auf dem Blatt „Virtuelle Azure-Computer“ angezeigt?
Wenn ein virtueller Computer in Azure DevTest Labs erstellt wird, wird eine Berechtigung zum Zugriff auf diesen virtuellen Computer erstellt. Sie können ihn sowohl auf dem Blatt für die Labs als auch auf dem Blatt **Virtuelle Computer** anzeigen. Benutzer mit der DevTest Labs-Rolle können alle im Lab erstellten virtuellen Computer auf dem Blatt **Alle virtuellen Computer** des Labs sehen. Allerdings erhalten Benutzer mit der DevTest Labs-Rolle nicht automatisch Lesezugriff auf die VM-Ressourcen, die andere erstellt haben. Deshalb werden diese virtuellen Computer nicht auf dem Blatt **Virtuelle Computer** angezeigt.

### <a name="what-is-the-difference-between-custom-images-and-formulas"></a>Was ist der Unterschied zwischen benutzerdefinierten Images und Formeln?
Ein benutzerdefiniertes Image ist eine VHD (virtuelle Festplatte), während eine Formel ein Image ist, das Sie mit zusätzlichen Einstellungen konfigurieren sowie speichern und reproduzieren können. Ein benutzerdefiniertes Image kann vorteilhafter sein, wenn Sie schnell mehrere Umgebungen mit demselben grundlegenden, unveränderlichen Image erstellen möchten. Eine Formel eignet sich möglicherweise besser, wenn Sie die Konfiguration Ihrer VM mit den aktuellsten Komponenten, einem virtuellen Netzwerk/Subnetz oder einer bestimmten Größe reproduzieren möchten. Eine ausführlichere Erklärung finden Sie im Artikel [Vergleich zwischen benutzerdefinierten Images und Formeln in DevTest Labs](devtest-lab-comparing-vm-base-image-types.md).

### <a name="how-do-i-create-multiple-vms-from-the-same-template-at-once"></a>Wie kann ich mehrere VMs gleichzeitig anhand derselben Vorlage erstellen?
Sie können die [Erweiterung für VSTS-Aufgaben](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks) verwenden oder beim Erstellen einer VM eine [Azure Resource Manager-Vorlage generieren](devtest-lab-add-vm-with-artifacts.md#save-azure-resource-manager-template) und die [Azure Resource Manager-Vorlage über Windows PowerShell](../azure-resource-manager/resource-group-template-deploy.md) bereitstellen.

### <a name="how-do-i-move-my-existing-azure-vms-into-my-azure-devtest-labs-lab"></a>Wie verschiebe ich meine vorhandenen Azure-VMs in mein Azure DevTest Labs-Lab?
Wir arbeiten an einer Lösung, die das direkte Verschieben von VMs nach Azure DevTest Labs ermöglicht. Zurzeit können Sie Ihre vorhandenen VMs jedoch wie folgt nach Azure DevTest Labs verschieben:

1. Kopieren Sie die VHD-Datei der vorhandenen VM mithilfe des folgenden [Windows PowerShell-Skripts](https://github.com/Azure/azure-devtestlab/blob/master/Scripts/CopyVHDFromVMToLab.ps1)
2. [Erstellen Sie das benutzerdefinierte Image](devtest-lab-create-template.md) in Ihrem Azure DevTest Labs-Lab.
3. Erstellen Sie anhand des benutzerdefinierten Images eine VM im Lab.

### <a name="can-i-attach-multiple-disks-to-my-vms"></a>Kann ich mehrere Datenträger an meine VMs anfügen?
Das Anfügen mehrerer Datenträger an VMs wird unterstützt.  

### <a name="if-i-want-to-use-a-windows-os-image-for-my-testing-do-i-have-to-purchase-an-msdn-subscription"></a>Wenn ich ein Windows-Betriebssystemimage für meine Tests verwenden möchte, muss ich dann ein MSDN-Abonnement erwerben?
Wenn Sie Windows-Clientbetriebssystem-Images (Windows 7 oder höher) für Ihre Entwicklung oder Ihre Tests in Azure benötigen, müssen Sie eine dieser Optionen nutzen:

- [Ein MSDN-Abonnement erwerben](https://www.visualstudio.com/products/how-to-buy-vs).
- Wenn Sie über ein Enterprise Agreement verfügen, erstellen Sie mit dem [Enterprise Dev/Test-Angebot](https://azure.microsoft.com/en-us/offers/ms-azr-0148p) ein Azure-Abonnement.

Weitere Informationen zu den Azure-Gutschriften für die einzelnen MSDN-Angebote finden Sie unter [Monatliche Azure-Gutschrift für Visual Studio-Abonnenten](https://azure.microsoft.com/en-us/pricing/member-offers/msdn-benefits-details/).

### <a name="how-do-i-automate-the-process-of-uploading-vhd-files-to-create-custom-images"></a>Wie automatisiere ich das Hochladen von VHD-Dateien zum Erstellen benutzerdefinierter Images?
Es gibt zwei Optionen:

* [Azure AzCopy](../storage/storage-use-azcopy.md#blob-upload) kann verwendet werden, um VHD-Dateien in das dem Lab zugeordnete Speicherkonto zu kopieren oder hochzuladen.
* [Microsoft Azure-Speicher-Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) ist eine eigenständige App, die unter Windows, OSX und Linux ausgeführt wird.   

Führen Sie folgende Schritte aus, um nach dem Zielspeicherkonto zu suchen, das Ihrem Lab zugeordnet ist:

1. Melden Sie sich auf dem [Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040)an.
2. Wählen Sie im linken Bereich **Ressourcengruppen** aus.
3. Suchen Sie nach der Ressourcengruppe, die Ihrem Lab zugeordnet ist, und wählen Sie sie aus.
4. Wählen Sie auf dem Blatt **Übersicht** eines der Speicherkonten aus.
5. Wählen Sie **Blobs**aus.
6. Suchen Sie in der Liste nach Uploads. Falls kein Upload vorhanden ist, kehren Sie zu Schritt 4 zurück, und versuchen Sie es mit einem anderen Speicherkonto.
7. Verwenden Sie die **URL** im AzCopy-Befehl als Ziel.

### <a name="how-can-i-automate-the-process-of-deleting-all-the-vms-in-my-lab"></a>Wie kann ich den Löschvorgang für alle virtuellen Computer in meinem Lab automatisieren?
Zusätzlich zum Löschen von virtuellen Computern aus Ihrem Lab im Azure-Portal können Sie alle virtuellen Computer in Ihrem Lab mithilfe eines PowerShell-Skripts löschen. Ändern Sie im folgenden Beispiel die Werte der Parameter unter dem Kommentar **Values to change** . Sie können die Werte `subscriptionId`, `labResourceGroup` und `labName` aus dem Labblatt im Azure-Portal abrufen.

    # Delete all the VMs in a lab

    # Values to change
    $subscriptionId = "<Enter Azure subscription ID here>"
    $labResourceGroup = "<Enter lab's resource group here>"
    $labName = "<Enter lab name here>"

    # Login to your Azure account
    Login-AzureRmAccount

    # Select the Azure subscription that contains the lab. This step is optional
    # if you have only one subscription.
    Select-AzureRmSubscription -SubscriptionId $subscriptionId

    # Get the lab that contains the VMs to delete.
    $lab = Get-AzureRmResource -ResourceId ('subscriptions/' + $subscriptionId + '/resourceGroups/' + $labResourceGroup + '/providers/Microsoft.DevTestLab/labs/' + $labName)

    # Get the VMs from that lab.
    $labVMs = Get-AzureRmResource | Where-Object {
              $_.ResourceType -eq 'microsoft.devtestlab/labs/virtualmachines' -and
              $_.ResourceName -like "$($lab.ResourceName)/*"}

    # Delete the VMs.
    foreach($labVM in $labVMs)
    {
        Remove-AzureRmResource -ResourceId $labVM.ResourceId -Force
    }




### <a name="what-are-artifacts"></a>Was sind Artefakte?
Artefakte sind anpassbare Elemente, die verwendet werden können, um die neuesten Komponenten oder Ihre Entwicklungstools auf einer VM bereitzustellen. Sie werden während der VM-Erstellung mit ein paar einfachen Klicks an die VM angefügt. Nach der Bereitstellung der VM wird Ihre VM von den Artefakten bereitgestellt und konfiguriert. In unserem [öffentlichen GitHub-Repository](https://github.com/Azure/azure-devtestlab/tree/master/Artifacts) ist bereits eine Reihe von Artefakten verfügbar, Sie können aber auch ganz einfach [eigene Artefakte erstellen](devtest-lab-artifact-author.md).

### <a name="how-do-i-create-a-lab-from-an-azure-resource-manager-template"></a>Wie erstelle ich ein Lab anhand einer Azure Resource Manager-Vorlage?
Wir haben ein [GitHub-Repository von Azure Resource Manager-Labvorlagen](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates) vorbereitet, die Sie in vorliegender oder geänderter Form zum Erstellen benutzerdefinierter Vorlagen für Ihre Labs bereitstellen können. Jede dieser Vorlagen hat einen Link, auf den Sie klicken können, um das Lab in vorliegender Form unter Ihrem eigenen Azure-Abonnement bereitzustellen, oder Sie können die Vorlage anpassen und [mit PowerShell oder über die Azure-Befehlszeilenschnittstelle bereitstellen](../azure-resource-manager/resource-group-template-deploy.md).

### <a name="why-are-my-vms-created-in-different-resource-groups-with-arbitrary-names-can-i-rename-or-modify-these-resource-groups"></a>Warum werden meine VMs in verschiedenen Ressourcengruppen mit willkürlichen Namen erstellt? Kann ich diese Ressourcengruppen umbenennen oder ändern?
Ressourcengruppen werden auf diese Weise erstellt, damit Azure DevTest Labs die Benutzerberechtigungen und den Zugriff auf virtuelle Computer verwalten kann. Sie können einen virtuellen Computer zwar in eine andere Ressourcengruppe mit dem gewünschten Namen verschieben, dies wird jedoch nicht empfohlen. Wir arbeiten daran, diese Situation zu verbessern, um mehr Flexibilität zu ermöglichen.   

### <a name="how-many-labs-can-i-create-under-the-same-subscription"></a>Wie viele Labs kann ich unter demselben Abonnement erstellen?
Es gibt keine bestimmte Beschränkung für die Anzahl von Labs, die pro Abonnement erstellt werden können. Allerdings sind die pro Abonnement nutzbaren Ressourcen begrenzt. Weitere Informationen finden Sie bei Bedarf in den Artikeln zu den [Einschränkungen für Azure-Abonnements und Dienste, Kontingente und Einschränkungen](../azure-subscription-service-limits.md) und zum [Erhöhen dieser Limits](https://azure.microsoft.com/blog/azure-limits-quotas-increase-requests).

### <a name="how-many-vms-can-i-create-per-lab"></a>Wie viele VMs kann ich pro Lab erstellen?
Es gibt keine bestimmte Beschränkung für die Anzahl von virtuellen Computern (VMs), die pro Abonnement erstellt werden können. Das Lab unterstützt zurzeit aber nur ca. 40 gleichzeitig ausgeführte VMs für Standardspeicher und 25 gleichzeitig ausgeführte VMs für Storage Premium. Wir arbeiten daran, diese Limits zu erhöhen.

### <a name="how-do-i-share-a-direct-link-to-my-lab"></a>Wie gebe ich einen direkten Link zu meinem Lab frei?
Einen direkten Link können Sie mit folgendem Verfahren für Ihre Labbenutzer freigeben:

1. Navigieren Sie im Azure-Portal zum Lab.
2. Kopieren Sie die Lab-URL aus Ihrem Browser, und geben Sie sie für die Laborbenutzer frei.

> [!NOTE]
> Wenn Ihre Labbenutzer externe Benutzer mit einem [Microsoft-Konto](#what-is-a-microsoft-account) sind und nicht zum Active Directory Ihres Unternehmens gehören, erhalten sie möglicherweise eine Fehlermeldung, wenn sie zu dem bereitgestellten Link navigieren. Weisen Sie sie an, im Falle einer Fehlermeldung oben rechts im Azure-Portal auf ihren Namen zu klicken und im Bereich **Verzeichnis** des Menüs das Verzeichnis auszuwählen, in dem sich das Lab befindet.
>
>

### <a name="what-is-a-microsoft-account"></a>Was ist ein Microsoft-Konto?
Ein Microsoft-Konto verwenden Sie für nahezu jede Aktion, die Sie mit Geräten und Diensten von Microsoft ausführen. Es handelt sich um eine E-Mail-Adresse und ein Kennwort zur Anmeldung bei Skype, Outlook.com, OneDrive, Windows Phone und Xbox LIVE – und es bedeutet, dass Ihre Dateien, Fotos, Kontakte und Einstellungen Ihnen zu jedem Gerät folgen können.

> [!NOTE]
> Früher wurde das Microsoft-Konto als „Windows Live ID“ bezeichnet.
>
>

### <a name="my-artifact-failed-during-vm-creation-how-do-i-troubleshoot-it"></a>Für mein Artefakt ist während der VM-Erstellung ein Fehler aufgetreten. Wie kann ich das Problem beheben?
Im Blogbeitrag [How to troubleshoot failing Artifacts in AzureDevTestLabs (Problembehandlung für fehlerhafte Artefakte in AzureDevTestLabs)](http://www.visualstudiogeeks.com/blog/DevOps/How-to-troubleshoot-failing-artifacts-in-AzureDevTestLabs) eines unserer MVPs finden Sie Informationen dazu, wie Sie Protokolle für das fehlgeschlagene Artefakt abrufen können.

### <a name="why-isnt-my-existing-virtual-network-saving-properly"></a>Warum wird mein vorhandenes virtuelles Netzwerk nicht korrekt gespeichert?
Möglicherweise enthält der Name des virtuellen Netzwerks Punkte. Wenn dies der Fall ist, können Sie die Punkte entfernen oder durch Bindestriche ersetzen und dann noch einmal versuchen, das virtuelle Netzwerk zu speichern.

### <a name="why-do-i-get-a-parent-resource-not-found-error-when-provisioning-a-vm-from-powershell"></a>Warum erhalte ich den Fehler „Übergeordnete Ressource wurde nicht gefunden“ bei der Bereitstellung einer VM von PowerShell?
Wenn eine Ressource einer anderen übergeordnet ist, muss die übergeordnete Ressource vor dem Erstellen der untergeordneten Ressource bereits vorhanden sein. Wenn sie nicht vorhanden ist, erhalten Sie den Fehler **ParentResourceNotFound**. Wenn Sie keine Abhängigkeit von der übergeordneten Ressource angeben, wird die untergeordnete Ressource möglicherweise vor der übergeordneten bereitgestellt.

VMs sind untergeordnete Ressourcen unter einem Lab in einer Ressourcengruppe. Wenn Sie Azure-Ressourcenvorlagen zur Bereitstellung über PowerShell verwenden, sollte der im PowerShell-Skript bereitgestellte Name der Ressourcengruppe der Name der Ressourcengruppe des Labs sein. Weitere Informationen finden Sie unter [Beheben verbreiteter Azure-Bereitstellungsfehler](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-manager-common-deployment-errors#parentresourcenotfound).

### <a name="where-can-i-find-more-error-information-if-a-vm-deployment-fails"></a>Wo finde ich weitere Fehlerinformationen zu Fehlern bei einer VM-Bereitstellung?
VM-Bereitstellungsfehler werden in den Aktivitätsprotokollen erfasst. Sie können die Aktivitätsprotokolle von virtuellen Labcomputern über die **Überwachungsprotokolle** oder **VM-Diagnose** auf dem VM-Blatt des Labs im Ressourcenmenü finden (das Blatt wird angezeigt, nachdem Sie den virtuellen Computer in der Liste **Meine virtuellen Computer** ausgewählt haben).

Manchmal tritt der Bereitstellungsfehler vor dem Start der VM-Bereitstellung auf – etwa, wenn das Abonnementlimit für eine mithilfe des virtuellen Computers erstellte Ressource überschritten ist. In diesem Fall werden die Fehlerdetails in den **Aktivitätsprotokollen** der Lab-Ebene erfasst, die Sie unten in den **Konfigurations- und Richtlinieneinstellungen** finden. Weitere Informationen zum Verwenden von Aktivitätsprotokollen in finden Sie unter [Anzeigen von Aktivitätsprotokollen, um Aktionen an Ressourcen zu überwachen](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-audit).

