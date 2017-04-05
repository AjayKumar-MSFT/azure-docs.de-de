---
title: Aktivieren von Remotedesktop in Azure Cloud Services | Microsoft Docs
description: "Konfigurieren einer Azure-Clouddienstanwendung für Remotedesktopverbindungen."
services: cloud-services
documentationcenter: 
author: thraka
manager: timlt
editor: 
ms.assetid: d3110ee8-6526-4585-aba5-d0bc9a713e9b
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/22/2016
ms.author: adegeo
translationtype: Human Translation
ms.sourcegitcommit: 4f2230ea0cc5b3e258a1a26a39e99433b04ffe18
ms.openlocfilehash: f64c41733f8fa7e34a0b0dfbbff2b565af7cf7db
ms.lasthandoff: 03/25/2017

---

# <a name="enable-remote-desktop-connection-for-a-role-in-azure-cloud-services"></a>Aktivieren einer Remotedesktopverbindung für eine Rolle in Azure Cloud Services

> [!div class="op_single_selector"]
> * [Azure-Portal](cloud-services-role-enable-remote-desktop-new-portal.md)
> * [Klassisches Azure-Portal](cloud-services-role-enable-remote-desktop.md)
> * [PowerShell](cloud-services-role-enable-remote-desktop-powershell.md)
> * [Visual Studio](../vs-azure-tools-remote-desktop-roles.md)

Sie können eine Remotedesktopverbindung in Ihrer Rolle während der Entwicklung aktivieren, indem Sie die Remotedesktopmodule in ihre Dienstdefinition aufnehmen, oder Sie können Remotedesktop über die Remotedesktoperweiterung aktivieren. Der bevorzugte Ansatz ist die Verwendung der Remotedesktoperweiterung, da sie Remotedesktop damit auch nach der Bereitstellung der Anwendung aktivieren können, ohne die Anwendung erneut bereitzustellen.

## <a name="configure-remote-desktop-from-the-azure-classic-portal"></a>Konfigurieren von Remotedesktop über das klassische Azure-Portal
Das klassische Azure-Portal ermöglicht die Remotedesktoperweiterung, sodass Sie Remotedesktop auch nach Bereitstellung der Anwendung aktivieren können. Auf der Seite **Konfigurieren** des Clouddiensts können Sie Remotedesktop aktivieren, das lokale Administratorkonto, das zum Herstellen einer Verbindung mit den virtuellen Computern verwendet wird, und das bei der Authentifizierung verwendete Zertifikat ändern und das Ablaufdatum festlegen.

1. Klicken Sie auf **Cloud Services** und dann auf den Namen des Clouddiensts, und klicken Sie dann auf **Konfigurieren**.
2. Klicken Sie unten auf die Schaltfläche **Remote**.

    ![Clouddienste remote](./media/cloud-services-role-enable-remote-desktop/CloudServices_Remote.png)

   > [!WARNING]
   > Alle Rolleninstanzen werden neu gestartet, wenn Sie Remotedesktop erstmals aktivieren und auf OK (Häkchen) klicken. Um einen Neustart zu verhindern, muss in der Rolle das Zertifikat installiert sein, mit dem das Kennwort verschlüsselt wird. Zum Verhindern eines Neustarts [laden Sie ein Zertifikat für den Clouddienst hoch](cloud-services-configure-ssl-certificate.md#step-3-upload-a-certificate) und kehren dann zu diesem Dialogfeld zurück.

3. Wählen Sie unter **Rollen** die Rolle aus, die aktualisiert werden soll, oder wählen Sie **Alle** für alle Rollen.
4. Nehmen Sie die folgenden Änderungen vor:

   * Um Remotedesktop zu aktivieren, aktivieren Sie das Kontrollkästchen **Remotedesktop aktivieren** . Um Remotedesktop zu deaktivieren, deaktivieren Sie das Kontrollkästchen.
   * Erstellen Sie ein Konto, das für Remotedesktopverbindungen mit den Rolleninstanzen verwendet wird.
   * Aktualisieren Sie das Kennwort für das bestehende Konto.
   * Wählen Sie ein hochgeladenes Zertifikat für die Authentifizierung aus (laden Sie das Zertifikat mit **Hochladen** auf der Seite **Zertifikate** hoch), oder erstellen Sie ein neues Zertifikat.
   * Ändern Sie das Ablaufdatum für die Remotedesktopkonfiguration.

5. Wenn Sie die Konfigurationsupdates beendet haben, klicken Sie auf **OK** (Häkchen).

## <a name="remote-into-role-instances"></a>Remotezugriff auf Rolleninstanzen
Nach der Aktivierung von Remotedesktop in den Rollen können Sie mit verschiedenen Tools remote auf eine Rolleninstanz zugreifen.

So verbinden Sie eine Rolleninstanz über das klassische Azure-Portal:

1. Klicken Sie auf **Instanzen**, um die Seite **Instanzen** zu öffnen.
2. Wählen Sie eine Rolleninstanz aus, in der Remotedesktop konfiguriert ist.
3. Klicken Sie auf **Verbinden**, und folgen Sie den Anweisungen, um den Desktop zu öffnen.
4. Klicken Sie auf **Öffnen** und dann auf **Verbinden**, um die Remotedesktopverbindung zu starten.

### <a name="use-visual-studio-to-remote-into-a-role-instance"></a>Remotezugriff auf eine Rolleninstanz mithilfe von Visual Studio
In Server-Explorer von Visual Studio:

1. Erweitern Sie den Knoten **Azure** > **Cloud Services** > **[Name des Clouddiensts]**.
2. Erweitern Sie entweder **Staging** oder **Produktion**.
3. Erweitern Sie die jeweilige Rolle.
4. Klicken Sie mit der rechten Maustaste auf eine der Rolleninstanzen, klicken Sie auf **Mithilfe von Remotedesktop verbinden...**, und geben Sie dann den Benutzernamen und das Kennwort ein.

![Server-Explorer von Remotedesktop](./media/cloud-services-role-enable-remote-desktop/ServerExplorer_RemoteDesktop.png)

### <a name="use-powershell-to-get-the-rdp-file"></a>Abrufen der RDP-Datei mithilfe von PowerShell
Sie können die RDP-Datei mit dem Cmdlet [Get-AzureRemoteDesktopFile](https://msdn.microsoft.com/library/azure/dn495261.aspx) abrufen. Sie können dann die RDP-Datei mit der Remotedesktopverbindung verwenden, um auf den Clouddienst zuzugreifen.

### <a name="programmatically-download-the-rdp-file-through-the-service-management-rest-api"></a>Programmgesteuertes Herunterladen der RDP-Datei über die Dienstverwaltungs-REST-API
Sie können den REST-Vorgang [Download RDP File](https://msdn.microsoft.com/library/jj157183.aspx) verwenden, um die RDP-Datei herunterzuladen.

## <a name="to-configure-remote-desktop-in-the-service-definition-file"></a>Konfigurieren von Remotedesktop in der Dienstdefinitionsdatei
Diese Methode ermöglicht das Aktivieren von Remotedesktop für die Anwendung während der Entwicklung. Dieser Ansatz erfordert die Speicherung verschlüsselter Kennwörter in Ihrer Dienstkonfigurationsdatei, und alle Aktualisierungen der Remotedesktopkonfiguration würden eine erneute Bereitstellung der Anwendung erfordern. Wenn Sie diese Nachteile umgehen möchten, sollten Sie den oben beschriebenen Ansatz mit der Remotedesktoperweiterung verwenden.  

Sie können Visual Studio verwenden, um mithilfe Ansatzes mit der Dienstdefinitionsdatei eine [Remotedesktopverbindung zu aktivieren](../vs-azure-tools-remote-desktop-roles.md) .  
Die folgenden Schritte beschreiben die erforderlichen Änderungen an den Dienstmodelldateien, um Remotedesktop zu aktivieren. Visual Studio nimmt diese Änderungen bei der Veröffentlichung automatisch vor.

### <a name="set-up-the-connection-in-the-service-model"></a>Einrichten der Verbindung im Dienstmodell
Verwenden Sie das **Imports**-Element zum Importieren der **RemoteAccess**-und **RemoteForwarder**-Module in die Datei [ServiceDefinition.csdef](cloud-services-model-and-package.md#csdef).

Die Dienstdefinitionsdatei sollte dem folgenden Beispiel mit hinzugefügtem `<Imports>` -Element ähneln.

```xml
<ServiceDefinition name="<name-of-cloud-service>" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition" schemaVersion="2013-03.2.0">
    <WebRole name="WebRole1" vmsize="Small">
        <Sites>
            <Site name="Web">
                <Bindings>
                    <Binding name="Endpoint1" endpointName="Endpoint1" />
                </Bindings>
            </Site>
        </Sites>
        <Endpoints>
            <InputEndpoint name="Endpoint1" protocol="http" port="80" />
        </Endpoints>
        <Imports>
            <Import moduleName="Diagnostics" />
            <Import moduleName="RemoteAccess" />
            <Import moduleName="RemoteForwarder" />
        </Imports>
    </WebRole>
</ServiceDefinition>
```
Die Datei [ServiceConfiguration.cscfg](cloud-services-model-and-package.md#cscfg) sollte dem folgenden Beispiel ähneln. Beachten Sie die Elemente `<ConfigurationSettings>` und `<Certificates>`. Das angegebene Zertifikat muss [in den Clouddienst hochgeladen werden](cloud-services-how-to-create-deploy.md#how-to-upload-a-certificate-for-a-cloud-service).

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceConfiguration serviceName="<name-of-cloud-service>" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="3" osVersion="*" schemaVersion="2013-03.2.0">
    <Role name="WebRole1">
        <Instances count="2" />
        <ConfigurationSettings>
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.Enabled" value="true" />
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.AccountUsername" value="[name-of-user-account]" />
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.AccountEncryptedPassword" value="[base-64-encrypted-user-password]" />
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.AccountExpiration" value="[certificate-expiration]" />
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteForwarder.Enabled" value="true" />
        </ConfigurationSettings>
        <Certificates>
            <Certificate name="Microsoft.WindowsAzure.Plugins.RemoteAccess.PasswordEncryption" thumbprint="[certificate-thumbprint]" thumbprintAlgorithm="sha1" />
        </Certificates>
    </Role>
</ServiceConfiguration>
```


## <a name="additional-resources"></a>Weitere Ressourcen
[Konfigurieren von Clouddiensten](cloud-services-how-to-configure.md)
[Häufig gestellte Fragen zu Clouddiensten– Remotedesktop](cloud-services-faq.md#remote-desktop)

