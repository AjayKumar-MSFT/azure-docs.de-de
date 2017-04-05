---
title: "Aktivieren einer Remotedesktopverbindung für eine Rolle in Azure Cloud Services mit PowerShell"
description: "Konfigurieren einer Azure-Clouddienstanwendung für Remotedesktopverbindungen mithilfe der PowerShell"
services: cloud-services
documentationcenter: 
author: thraka
manager: timlt
editor: 
ms.assetid: bf2f70a4-20dc-4302-a91a-38cd7a2baa62
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/22/2016
ms.author: adegeo
translationtype: Human Translation
ms.sourcegitcommit: 4f2230ea0cc5b3e258a1a26a39e99433b04ffe18
ms.openlocfilehash: a78fc57d264e9f8074c94b334b24bbf1b7871d08
ms.lasthandoff: 03/25/2017


---
# <a name="enable-remote-desktop-connection-for-a-role-in-azure-cloud-services-using-powershell"></a>Aktivieren einer Remotedesktopverbindung für eine Rolle in Azure Cloud Services mit PowerShell
> [!div class="op_single_selector"]
> * [Azure-Portal](cloud-services-role-enable-remote-desktop-new-portal.md)
> * [Klassisches Azure-Portal](cloud-services-role-enable-remote-desktop.md)
> * [PowerShell](cloud-services-role-enable-remote-desktop-powershell.md)
> * [Visual Studio](../vs-azure-tools-remote-desktop-roles.md)
>
>

Mit Remotedesktop können Sie auf den Desktop einer Rolle zugreifen, die in Azure ausgeführt wird. Mithilfe einer Remotedesktopverbindung können Sie Probleme mit Ihrer Anwendung diagnostizieren und beheben, während diese ausgeführt wird.

In diesem Artikel wird beschrieben, wie Sie Remotedesktop mithilfe von PowerShell für Cloud Services-Rollen aktivieren. Informationen zu den erforderlichen Komponenten für diesen Artikel finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azureps-cmdlets-docs). PowerShell ermöglicht die Remotedesktoperweiterung, sodass Sie Remotedesktop auch nach der Bereitstellung der Anwendung aktivieren können.

## <a name="configure-remote-desktop-from-powershell"></a>Konfigurieren von Remotedesktop über PowerShell
Mit dem Cmdlet [Set-AzureServiceRemoteDesktopExtension](https://msdn.microsoft.com/library/azure/dn495117.aspx) können Sie Remotedesktop für bestimmte Rollen oder für alle Rollen Ihrer Clouddienstbereitstellung aktivieren. Das Cmdlet ermöglicht die Angabe des Benutzernamens und Kennworts für Remotedesktopbenutzer über den *Credential* -Parameter, der ein PSCredential-Objekt annimmt.

Wenn Sie PowerShell interaktiv verwenden, können Sie das PSCredential-Objekt durch Aufrufen des Cmdlets [Get-Credentials](https://technet.microsoft.com/library/hh849815.aspx) auf einfache Weise festlegen.

```
$remoteusercredentials = Get-Credential
```

Dieser Befehl zeigt ein Dialogfeld an, in dem Sie den Benutzernamen und das Kennwort für einen Benutzer auf sichere Weise eingeben können.

Da PowerShell Automatisierungsszenarien unterstützt, können Sie das **PSCredential** -Objekt auch so einrichten, dass keine Benutzerinteraktion erforderlich ist. Zunächst müssen Sie ein sicheres Kennwort einrichten. Sie geben zunächst ein Klartextkennwort an und konvertieren es dann mit [ConvertTo-SecureString](https://technet.microsoft.com/library/hh849818.aspx)in eine sichere Zeichenfolge. Als Nächstes müssen Sie diese sichere Zeichenfolge mit [ConvertFrom-SecureString](https://technet.microsoft.com/library/hh849814.aspx)in eine verschlüsselte Standardzeichenfolge konvertieren. Anschließend können Sie diese verschlüsselte Standardzeichenfolge mit [Set-Content](https://technet.microsoft.com/library/ee176959.aspx)in einer Datei speichern.

Sie können auch eine sichere Kennwortdatei erstellen, damit Sie nicht jedes Mal das Kennwort eingeben müssen. Darüber hinaus ist eine sichere Kennwortdatei besser als eine Nur-Text-Datei. Verwenden Sie das folgende PowerShell-Skript zum Erstellen einer sicheren Kennwortdatei:

```
ConvertTo-SecureString -String "Password123" -AsPlainText -Force | ConvertFrom-SecureString | Set-Content "password.txt"
```

> [!IMPORTANT]
> Stellen Sie beim Festlegen des Kennworts sicher, dass die [Komplexitätsvoraussetzungen](https://technet.microsoft.com/library/cc786468.aspx)erfüllt sind.
>
>

Zum Erstellen des Anmeldeinformationsobjekts aus der sicheren Kennwortdatei müssen Sie den Inhalt der Datei auslesen und mit [ConvertTo-SecureString](https://technet.microsoft.com/library/hh849818.aspx)in eine sichere Zeichenfolge zurückkonvertieren.

Das Cmdlet [Set-AzureServiceRemoteDesktopExtension](https://msdn.microsoft.com/library/azure/dn495117.aspx) nimmt auch den *Expiration* -Parameter an, der einen **DateTime** -Wert für den Zeitpunkt angibt, an dem das Benutzerkonto abläuft. Beispielsweise könnten Sie das Konto so einrichten, dass es ein paar Tage ab dem aktuellen Datum und der aktuellen Uhrzeit abläuft.

Im folgenden PowerShell-Beispiel wird die Remotedesktoperweiterung für einen Clouddienst festgelegt:

```
$servicename = "cloudservice"
$username = "RemoteDesktopUser"
$securepassword = Get-Content -Path "password.txt" | ConvertTo-SecureString
$expiry = $(Get-Date).AddDays(1)
$credential = New-Object System.Management.Automation.PSCredential $username,$securepassword
Set-AzureServiceRemoteDesktopExtension -ServiceName $servicename -Credential $credential -Expiration $expiry
```
Sie können optional auch den Bereitstellungsslot und die Bereitstellungsrollen angeben, für die Sie Remotedesktop aktivieren möchten. Wenn diese Parameter nicht angegeben werden, aktiviert das Cmdlet standardmäßig Remotedesktop für alle Rollen im Slot zur Bereitstellung der **Produktion**.

Die Remotedesktoperweiterung ist immer einer Bereitstellung zugeordnet. Wenn Sie eine neue Bereitstellung für den Dienst erstellen, müssen Sie Remotedesktop für diese Bereitstellung aktivieren. Wenn Remotedesktop immer aktiviert sein soll, empfiehlt es sich, die PowerShell-Skripts in Ihren Bereitstellungsworkflow zu integrieren.

## <a name="remote-desktop-into-a-role-instance"></a>Remotedesktop in einer Rolleninstanz
Mit dem Cmdlet [Get-AzureRemoteDesktopFile](https://msdn.microsoft.com/library/azure/dn495261.aspx) stellen Sie eine Remotedesktopverbindung mit einer bestimmten Rolleninstanz Ihres Clouddiensts her. Sie können den *LocalPath* -Parameter zum lokalen Herunterladen der RDP-Datei verwenden. Sie können alternativ den *Launch* -Parameter verwenden, um das Dialogfeld „Remotedesktopverbindung“ für den Zugriff auf die Rolleninstanz des Clouddiensts direkt zu starten.

```
Get-AzureRemoteDesktopFile -ServiceName $servicename -Name "WorkerRole1_IN_0" -Launch
```


## <a name="check-if-remote-desktop-extension-is-enabled-on-a-service"></a>Überprüfen, ob die Remotedesktoperweiterung für einen Dienst aktiviert ist
Das Cmdlet [Get-AzureServiceRemoteDesktopExtension](https://msdn.microsoft.com/library/azure/dn495261.aspx) zeigt an, ob Remotedesktop für eine Dienstbereitstellung aktiviert oder deaktiviert ist. Das Cmdlet gibt den Benutzernamen des Remotedesktopbenutzers und die Rollen zurück, für die die Remotedesktoperweiterung aktiviert ist. Standardmäßig geschieht dies auf dem Bereitstellungsslot, und Sie können stattdessen den Stagingslot wählen.

```
Get-AzureServiceRemoteDesktopExtension -ServiceName $servicename
```

## <a name="remove-remote-desktop-extension-from-a-service"></a>Entfernen der Remotedesktoperweiterung für einen Dienst
Wenn Sie die Remotedesktoperweiterung bereits für eine Bereitstellung aktiviert haben und die Remotedesktopeinstellungen aktualisieren müssen, entfernen Sie zunächst die Erweiterung. Aktivieren Sie sie erneut mit den neuen Einstellungen. Wenn Sie beispielsweise ein neues Kennwort für das Remotebenutzerkonto festlegen möchten, oder das Konto abgelaufen ist. Dies ist nur bei vorhandenen Bereitstellungen erforderlich, für die die Remotedesktoperweiterung aktiviert ist. Bei neuen Bereitstellungen können Sie dagegen die Erweiterung einfach direkt anwenden.

Zum Entfernen der Remotedesktoperweiterung aus einer Bereitstellung können Sie das Cmdlet [Remove-AzureServiceRemoteDesktopExtension](https://msdn.microsoft.com/library/azure/dn495280.aspx) verwenden. Sie können optional auch den Bereitstellungsslot und die Bereitstellungsrollen angeben, für die Sie die Remotedesktoperweiterung entfernen möchten.

```
Remove-AzureServiceRemoteDesktopExtension -ServiceName $servicename -UninstallConfiguration
```

> [!NOTE]
> Um die Erweiterungskonfiguration vollständig zu entfernen, sollten Sie das Cmdlet *remove* mit dem **UninstallConfiguration** -Parameter aufrufen.
>
> Mit dem **UninstallConfiguration** -Parameter wird die gesamte Erweiterungskonfiguration deinstalliert, die auf den Dienst angewendet wurde. Jede Erweiterungskonfiguration ist mit der Dienstkonfiguration verbunden. Durch Aufrufen des Cmdlets *remove* ohne den **UninstallConfiguration**-Parameter wird die Zuordnung der <mark>Bereitstellung</mark> zur Erweiterungskonfiguration aufgehoben und die Erweiterung faktisch entfernt. Die Erweiterungskonfiguration ist jedoch weiterhin dem Dienst zugeordnet.
>
>

## <a name="additional-resources"></a>Zusätzliche Ressourcen

[Konfigurieren von Clouddiensten](cloud-services-how-to-configure.md)
[Häufig gestellte Fragen zu Clouddiensten– Remotedesktop](cloud-services-faq.md#remote-desktop)

