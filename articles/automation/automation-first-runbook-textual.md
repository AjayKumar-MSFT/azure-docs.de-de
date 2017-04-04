---
title: Mein erstes PowerShell-Workflow-Runbook in Azure Automation | Microsoft Docs
description: "Tutorial, in dem Sie sich mit dem Erstellen, Testen und Veröffentlichen eines einfachen Textrunbooks unter Verwendung eines PowerShell-Workflows vertraut machen können."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: 
keywords: "PowerShell-Workflow, Beispiele für Powershell-Workflows, Workflow PowerShell"
ms.assetid: 0002d7f7-e2b5-46e3-b5eb-4596b84fd526
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/26/2017
ms.author: magoedte;bwren
translationtype: Human Translation
ms.sourcegitcommit: 356de369ec5409e8e6e51a286a20af70a9420193
ms.openlocfilehash: 739f7c0fe0cca03d80fa8b4bdadbf93b5da72a73
ms.lasthandoff: 03/27/2017


---
# <a name="my-first-powershell-workflow-runbook"></a>Mein erstes PowerShell-Workflow-Runbook

> [!div class="op_single_selector"]
> * [Grafisch](automation-first-runbook-graphical.md)
> * [PowerShell](automation-first-runbook-textual-powershell.md)
> * [PowerShell-Workflow](automation-first-runbook-textual.md)
> 
> 

Dieses Tutorial führt Sie durch die Erstellung eines [PowerShell-Workflow-Runbooks](automation-runbook-types.md#powershell-workflow-runbooks) in Azure Automation. Wir beginnen mit einem einfachen Runbook, das wir testen und veröffentlichen. Dabei erläutern wir, wie Sie den Status des Runbookauftrags nachverfolgen. Anschließend ändern wir das Runbook, um damit tatsächlich Azure-Ressourcen zu verwalten. Im vorliegenden Fall soll ein virtueller Azure-Computer gestartet werden. Abschließend fügen wir Runbookparameter hinzu, um die Stabilität des Runbooks zu erhöhen.

## <a name="prerequisites"></a>Voraussetzungen
Für dieses Tutorial benötigen Sie Folgendes:

* Azure-Abonnement. Wenn Sie noch kein Abonnement haben, können Sie Ihre [MSDN-Abonnentenvorteile aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) oder sich für ein <a href="/pricing/free-account/" target="_blank">[kostenloses Konto registrieren](https://azure.microsoft.com/free/).
* [Automation-Konto](automation-sec-configure-azure-runas-account.md) dient zur Aufbewahrung des Runbooks und zur Authentifizierung gegenüber Azure-Ressourcen.  Dieses Konto muss über die Berechtigung zum Starten und Beenden des virtuellen Computers verfügen.
* Einen virtuellen Azure-Computer. Da dieser Computer gestartet und beendet wird, sollte es sich nicht um einen virtuellen Computer in der Produktionsumgebung handeln.

## <a name="step-1---create-new-runbook"></a>Schritt 1: Erstellen eines neuen Runbooks
Wir erstellen zunächst ein einfaches Runbook, das den Text *Hello World*ausgibt.

1. Öffnen Sie im Azure-Portal Ihr Automation-Konto.  
   Die Seite des Automation-Kontos bietet einen kurzen Überblick über die Ressourcen des Kontos. Sie sollten bereits über einige Objekte verfügen. Bei den meisten handelt es sich um die Module, die automatisch in einem neuen Automation-Konto enthalten sind. Darunter müsste sich auch das in den [Voraussetzungen](#prerequisites)erwähnte Anmeldeinformationsobjekt befinden.
2. Klicken Sie auf die Kachel **Runbooks** , um die Liste mit den Runbooks zu öffnen.<br><br> ![Steuerelement für Runbooks](media/automation-first-runbook-textual/runbooks-control-tiles.png)
3. Erstellen Sie ein neues Runbook, indem Sie auf die Schaltfläche **Runbook hinzufügen** und anschließend auf **Neues Runbook erstellen** klicken.
4. Nennen Sie das Runbook *MyFirstRunbook-Workflow*.
5. Da wir hier ein [PowerShell Workflow-Runbook](automation-runbook-types.md#powershell-workflow-runbooks) erstellen, wählen Sie als **Runbooktyp** die Option **PowerShell-Workflow** aus.<br><br> ![Neues Runbook](media/automation-first-runbook-textual/new-runbook-properties.png)
6. Klicken Sie auf **Erstellen** , um das Runbook zu erstellen und den Text-Editor zu öffnen.

## <a name="step-2---add-code-to-the-runbook"></a>Schritt 2 – Hinzufügen von Code zum Runbook
Sie können entweder direkt Code in das Runbook eingeben, oder Sie wählen Cmdlets, Runbooks und Objekte aus dem Bibliotheksteuerelement aus und fügen diese mit entsprechenden Parametern zum Runbook hinzu. In dieser exemplarischen Vorgehensweise erfolgt eine direkte Eingabe in das Runbook.

1. Unser Runbook ist zurzeit leer. Es enthält lediglich das erforderliche Schlüsselwort *workflow*, den Namen des Runbooks und die geschweiften Klammern, die den gesamten Workflow umschließen.

   ```
   Workflow MyFirstRunbook-Workflow
   {
   }
   ```
2. Geben Sie zwischen den geschweiften Klammern *Write-Output "Hello World."* ein.

   ```
   Workflow MyFirstRunbook-Workflow
   {
   Write-Output "Hello World"
   }
   ```
3. Klicken Sie auf **Speichern**, um das Runbook zu speichern.<br><br> ![Runbook speichern](media/automation-first-runbook-textual/automation-runbook-edit-controls-save.png)

## <a name="step-3---test-the-runbook"></a>Schritt 3: Testen des Runbooks
Bevor wir das Runbook für die Verwendung in der Produktionsumgebung veröffentlichen, möchten wir uns vergewissern, dass es ordnungsgemäß funktioniert. Beim Testen eines Runbooks führen Sie die **Entwurfsversion** des Runbooks aus und sehen sich interaktiv die Ausgabe an.

1. Klicken Sie auf **Testbereich** , um den Testbereich zu öffnen.<br><br> ![Testbereich](media/automation-first-runbook-textual/automation-runbook-edit-controls-test.png)
2. Klicken Sie auf **Starten** , um den Test zu starten. Andere Optionen dürften nicht zur Verfügung stehen.
3. Ein [Runbookauftrag](automation-runbook-execution.md) wird erstellt, und der dazugehörige Status wird angezeigt.  
   Der Auftrag besitzt zunächst den Status *In der Warteschlange* , um anzugeben, dass der Auftrag darauf wartet, dass in der Cloud ein Runbook Worker verfügbar wird. Wird der Auftrag von einem Worker übernommen, wechselt der Status zu *Wird gestartet...* und anschließend zu *Wird ausgeführt...*, wenn die Ausführung des Runbooks tatsächlich gestartet wurde.  
4. Nach Abschluss des Runbookauftrags wird die Ausgabe angezeigt. In unserem Fall: *Hello World*.<br><br> ![Hello World](media/automation-first-runbook-textual/test-output-hello-world.png)
5. Schließen Sie den Testbereich, um zum Zeichenbereich zurückzukehren.

## <a name="step-4---publish-and-start-the-runbook"></a>Schritt 4: Veröffentlichen und Starten des Runbooks
Das soeben erstellte Runbook befindet sich immer noch im Entwurfsmodus. Wir müssen das Runbook veröffentlichen, um es in der Produktionsumgebung ausführen zu können. Beim Veröffentlichen eines Runbooks wird die vorhandene veröffentlichte Version durch die Entwurfsversion überschrieben. In unserem Fall ist noch keine veröffentlichte Version vorhanden, da wir das Runbook gerade erst erstellt haben.

1. Klicken Sie auf **Veröffentlichen**, um das Runbook zu veröffentlichen, und bestätigen Sie den Vorgang mit **Ja**.<br><br> ![Veröffentlichen](media/automation-first-runbook-textual/automation-runbook-edit-controls-publish.png)
2. Wenn Sie jetzt einen Bildlauf nach links durchführen, um das Runbook im Bereich **Runbooks** anzuzeigen, zeigt das Runbook **Veröffentlicht** als **Erstellungsstatus** an.
3. Führen Sie wieder einen Bildlauf nach rechts durch, um den Bereich für **MyFirstRunbook-Workflow**anzuzeigen.  
   Mit den Optionen am oberen Rand können wir das Runbook starten, den Start für einen späteren Zeitpunkt planen oder einen [Webhook](automation-webhooks.md) erstellen, um den Start über einen HTTP-Aufruf zu ermöglichen.
4. Wir möchten das Runbook starten. Klicken Sie daher auf **Starten** und anschließend auf **Ja**.<br><br> ![Runbook starten](media/automation-first-runbook-textual/automation-runbook-controls-start.png)
5. Ein Auftragsbereich für den soeben erstellten Runbookauftrag erscheint. Dieser Bereich kann zwar geschlossen werden, in diesem Fall lassen wir ihn jedoch geöffnet, um den Status des Auftrags verfolgen zu können.
6. Der Auftragsstatus wird unter **Auftragszusammenfassung** angezeigt und entspricht den Statusoptionen, die wir bereits beim Testen des Runbooks gesehen haben.<br><br> ![Auftragszusammenfassung](media/automation-first-runbook-textual/job-pane-status-blade-jobsummary.png)
7. Wenn der Runbookstatus *Abgeschlossen*lautet, klicken Sie auf **Ausgabe**. Der Bereich "Ausgabe" wird geöffnet, und der Text *Hello World*.<br><br> ![API-Zusammenfassung](media/automation-first-runbook-textual/job-pane-status-blade-outputtile.png)  
8. Schließen Sie den Ausgabebereich.
9. Klicken Sie auf **Alle Protokolle**, um den Bereich „Datenströme“ für den Runbookauftrag zu öffnen. Im Ausgabestream sollte nur *Hello World* angezeigt werden. Hier können aber auch andere Datenströme für einen Runbookauftrag (wie etwa „Ausführlich“ und „Fehler“) angezeigt werden, sofern das Runbook in diese schreibt.<br><br> ![API-Zusammenfassung](media/automation-first-runbook-textual/job-pane-status-blade-alllogstile.png)
10. Schließen Sie den Datenstrom- und den Auftragsbereich, um zum Bereich „MyFirstRunbook“ zurückzukehren.
11. Klicken Sie auf **Aufträge** , um den Auftragsbereich für dieses Runbook zu öffnen. Dadurch werden alle von diesem Runbook erstellten Aufträge aufgeführt. Hier wird nur ein einzelner Auftrag aufgeführt, da wir den Auftrag bislang erst einmal ausgeführt haben.<br><br> ![Aufträge](media/automation-first-runbook-textual/runbook-control-job-tile.png)
12. Wenn Sie auf diesen Auftrag klicken, wird wieder der Auftragsbereich geöffnet, den wir uns beim Starten des Runbooks angesehen haben. So können Sie bereits ausgeführte Aufträge öffnen und Details zu jedem Auftrag anzeigen, der für ein bestimmtes Runbook erstellt wurde.

## <a name="step-5---add-authentication-to-manage-azure-resources"></a>Schritt 5: Hinzufügen von Authentifizierungsfunktionen für die Verwaltung von Azure-Ressourcen
Wir haben unser Runbook inzwischen zwar getestet und veröffentlicht, bislang ist es aber noch nicht sonderlich hilfreich. Wir möchten damit Azure-Ressourcen verwalten. Dazu ist jedoch eine Authentifizierung mit den Anmeldeinformationen erforderlich, die in den [Voraussetzungen](#prerequisites)genannt sind. Wir verwenden zu diesem Zweck das Cmdlet **Add-AzureRMAccount** .

1. Öffnen Sie den Text-Editor, indem Sie im Bereich „MyFirstRunbook-Workflow“ auf **Bearbeiten** klicken.<br><br> ![Runbook bearbeiten](media/automation-first-runbook-textual/automation-runbook-controls-edit.png)
2. Die Zeile **Write-Output** wird nicht mehr benötigt, also löschen Sie sie.
3. Positionieren Sie den Cursor in einer leeren Zeile zwischen den geschweiften Klammern.
4. Geben Sie den folgenden Code ein (oder fügen Sie ihn ein). Dieser Code ist für die Authentifizierung bei Ihrem ausführenden Konto für Automation zuständig:

   ```
   $Conn = Get-AutomationConnection -Name AzureRunAsConnection
   Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID `
   -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
   ```
5. Klicken Sie auf den **Testbereich** , um das Runbook zu testen.
6. Klicken Sie auf **Starten** , um den Test zu starten. Nach Abschluss des Tests erhalten Sie in der Regel eine Ausgabe wie in der Abbildung unten mit allgemeinen Informationen aus Ihrem Konto. Auf diese Weise wird bestätigt, dass die Anmeldeinformationen gültig sind.<br><br> ![Authentifizieren](media/automation-first-runbook-textual/runbook-auth-output.png)

## <a name="step-6---add-code-to-start-a-virtual-machine"></a>Schritt 6: Hinzufügen von Code zum Starten eines virtuellen Computers
Nachdem das Runbook jetzt in unserem Azure-Abonnement authentifiziert ist, können wir Ressourcen verwalten. Wir fügen einen Befehl zum Starten eines virtuellen Computers hinzu. Sie können einen beliebigen virtuellen Computer in Ihrem Azure-Abonnement auswählen. Wir werden den Namen vorerst im Runbook hartcodieren.

1. Geben Sie nach *Add-AzureRmAccount* den Code *Start-AzureRmVM -Name 'VMName' -ResourceGroupName 'NameofResourceGroup'* ein, und geben Sie den Namen und den Ressourcengruppennamen des virtuellen Computers an, der gestartet werden soll.  

   ```
   workflow MyFirstRunbook-Workflow
   {
   $Conn = Get-AutomationConnection -Name AzureRunAsConnection
   Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
   Start-AzureRmVM -Name 'VMName' -ResourceGroupName 'ResourceGroupName'
   }
   ```
2. Speichern Sie das Runbook, und klicken Sie dann auf den **Testbereich** , um das Runbook zu testen.
3. Klicken Sie auf **Starten** , um den Test zu starten. Vergewissern Sie sich nach Abschluss des Tests, dass der virtuelle Computer gestartet wurde.

## <a name="step-7---add-an-input-parameter-to-the-runbook"></a>Schritt 7: Hinzufügen eines Eingabeparameters zum Runbook
Unser Runbook startet zwar nun den virtuellen Computer, den wir im Runbook hartcodiert haben, es wäre aber praktischer, wenn wir den virtuellen Computer beim Start des Runbooks angeben könnten. Zu diesem Zweck fügen wir dem Runbook nun Eingabeparameter hinzu.

1. Fügen Sie dem Runbook Parameter für *VMName* und *ResourceGroupName* hinzu, und verwenden Sie diese Variablen mit dem Cmdlet **Start-AzureRmVM**, wie im folgenden Beispiel gezeigt.

   ```
   workflow MyFirstRunbook-Workflow
   {
    Param(
     [string]$VMName,
     [string]$ResourceGroupName
    )  
   $Conn = Get-AutomationConnection -Name AzureRunAsConnection
   Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
   Start-AzureRmVM -Name $VMName -ResourceGroupName $ResourceGroupName
   }
   ```
2. Speichern Sie das Runbook, und öffnen Sie den Testbereich. Beachten Sie, dass Sie nun Werte für die beiden Eingabevariablen angeben können, die im Test verwendet werden.
3. Schließen Sie den Testbereich.
4. Klicken Sie auf **Veröffentlichen** , um die neue Version des Runbooks zu veröffentlichen.
5. Beenden Sie den virtuellen Computer, den Sie im vorherigen Schritt gestartet haben.
6. Klicken Sie auf **Starten** , um das Runbook zu starten. Geben Sie **VMName** und **ResourceGroupName** für den virtuellen Computer ein, den Sie starten möchten.<br><br> ![Start Runbook](media/automation-first-runbook-textual/automation-pass-params.png)<br>  
7. Vergewissern Sie sich nach Abschluss des Runbooks, dass der virtuelle Computer gestartet wurde.  

## <a name="next-steps"></a>Nächste Schritte
* Informationen über die ersten Schritte mit grafischen Runbooks finden Sie unter [Mein erstes grafisches Runbook](automation-first-runbook-graphical.md)
* Erste Schritte mit PowerShell-Runbooks werden in [Mein erstes PowerShell-Runbook](automation-first-runbook-textual-powershell.md)
* Informationen über die verschiedenen Runbooktypen, ihre Vorteile und Einschränkungen finden Sie unter [Azure Automation-Runbooktypen](automation-runbook-types.md)
* Weitere Informationen zur PowerShell-Skriptunterstützung finden Sie unter [Native PowerShell Script Support in Azure Automation](https://azure.microsoft.com/blog/announcing-powershell-script-support-azure-automation-2/)

