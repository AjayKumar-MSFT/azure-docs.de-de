---
title: Automatisieren der Azure Stack-Überprüfung mit PowerShell | Microsoft-Dokumentation
description: Sie können die Azure Stack-Überprüfung mit PowerShell automatisieren.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/17/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.lastreviewed: 11/26/2018
ROBOTS: NOINDEX
ms.openlocfilehash: 99a1b218919380dcdb0c1dba4540d9f8219d2df3
ms.sourcegitcommit: a8948ddcbaaa22bccbb6f187b20720eba7a17edc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/21/2019
ms.locfileid: "56593640"
---
# <a name="automate-azure-stack-validation-with-powershell"></a>Automatisieren der Azure Stack-Überprüfung mit PowerShell

Validation-as-a-Service (VaaS) bietet die Möglichkeit, den Start von Tests mithilfe des Skripts **LaunchVaaSTests.ps1** zu automatisieren.

> [!NOTE]
> Automation ist nur für den Testdurchlauf-Workflow verfügbar. Die Workflows für Paket- und Lösungsüberprüfung werden nur über das VaaS-Portal unterstützt.

In diesem Tutorial erfahren Sie, wie Sie ein Skript erstellen, das folgende Aktionen ausführt:

> [!div class="checklist"]
> * Installieren der erforderlichen Komponenten
> * Installieren und Starten des lokalen Agents
> * Starten einer Testkategorie (beispielsweise Integrations-, Funktions- oder Zuverlässigkeitstests)
> * Melden der Testergebnisse

## <a name="launch-the-test-pass-workflow"></a>Starten des Workflows für den Testdurchlauf

1. Öffnen Sie eine PowerShell-Eingabeaufforderung mit erhöhten Rechten.

2. Führen Sie das folgende Skript aus, um das Automatisierungsskript herunterzuladen:

    ```PowerShell
    New-Item -ItemType Directory -Path <VaaSLaunchDirectory>
    Set-Location <VaaSLaunchDirectory>
    Invoke-WebRequest -Uri https://storage.azurestackvalidation.com/packages/Microsoft.VaaS.Scripts.latest.nupkg -OutFile "LaunchVaaS.zip"
    Expand-Archive -Path ".\LaunchVaaS.zip" -DestinationPath .\ -Force
    ```

3. Führen Sie das folgende Skript mit den entsprechenden Parameterwerten aus:

    ```PowerShell
    $VaaSAccountCreds = New-Object System.Management.Automation.PSCredential "<VaaSUserId>", (ConvertTo-SecureString "<VaaSUserPassword>" -AsPlainText -Force)
    .\LaunchVaaSTests.ps1 -VaaSAccountCreds $VaaSAccountCreds `
                          -VaaSAccountTenantId <VaaSAccountTenantId> `
                          -VaaSSolutionName <VaaSSolutionName> `
                          -VaaSTestPassName <VaaSTestPassName> `
                          -VaaSTestCategories Integration,Functional `
                          -MaxScriptWaitTimeInHours 12 `
                          -ServiceAdminUserName <AzSServiceAdminUser> `
                          -ServiceAdminUserPassword <AzSServiceAdminPassword> `
                          -TenantAdminUserName <AzSTenantAdminUser> `
                          -TenantAdminUserPassword <AzSTenantAdminPassword> `
                          -CloudAdminUserName <AzSCloudAdminUser> `
                          -CloudAdminUserPassword <AzSCloudAdminPassword>
    ```

    **Parameter**

    | Parameter | BESCHREIBUNG |
    | --- | --- |
    | VaaSUserId | Ihre VaaS-Benutzer-ID |
    | VaaSUserPassword | Ihr VaaS-Kennwort |
    | VaaSAccountTenantId | Die GUID Ihres VaaS-Mandanten. |
    | VaaSSolutionName | Der Name der VaaS-Lösung, unter der der Testdurchlauf ausgeführt wird. |
    | VaaSTestPassName | Der Name des zu erstellenden VaaS-Testdurchlaufworkflows. |
    | VaaSTestCategories | `Integration`, `Functional` oder `Reliability`. Wenn Sie mehrere Werte verwenden, trennen Sie diese durch ein Komma.  |
    | ServiceAdminUserName | Ihr Azure Stack-Dienstadministratorkonto  |
    | ServiceAdminPassword | Ihr Kennwort für den Azure Stack-Dienst  |
    | TenantAdminUserName | Der Administrator für den primären Mandanten  |
    | TenantAdminPassword | Das Kennwort für den primären Mandanten  |
    | CloudAdminUserName | Der Benutzername des Cloudadministrators.  |
    | CloudAdminPassword | Das Kennwort für den Cloudadministrator.  |

4. Sehen Sie sich die Ergebnisse des Tests an. Weitere Optionen finden Sie unter [Überwachen und Verwalten von Tests im VaaS-Portal](azure-stack-vaas-monitor-test.md).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu PowerShell in Azure Stack finden Sie in den neuesten Modulen.

> [!div class="nextstepaction"]
> [Azure Stack-Modul](https://docs.microsoft.com/powershell/azure/azure-stack/overview?view=azurestackps-1.6.0)
