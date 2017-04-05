---
title: "Zurücksetzen des Kennworts oder der Remotedesktopkonfiguration auf einem virtuellen Windows-Computer | Microsoft Docs"
description: "Erfahren Sie, wie Sie das Kennwort für ein Konto oder Remotedesktopdienste auf einem virtuellen Windows-Computer mit dem Azure-Portal oder Azure PowerShell zurücksetzen."
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 45c69812-d3e4-48de-a98d-39a0f5675777
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: iainfou
translationtype: Human Translation
ms.sourcegitcommit: 356de369ec5409e8e6e51a286a20af70a9420193
ms.openlocfilehash: 2673c95d2e312d427454585d46ac790cb126fea6
ms.lasthandoff: 03/27/2017


---
# <a name="how-to-reset-the-remote-desktop-service-or-its-login-password-in-a-windows-vm"></a>Zurücksetzen des Remotedesktopdiensts oder seines Anmeldekennworts in einer Windows-VM
Wenn Sie keine Verbindung mit einem virtuellen Windows-Computer herstellen können, können Sie das lokale Administratorkennwort oder die Konfiguration des Remotedesktopdiensts zurücksetzen. Das Kennwort kann entweder über das Azure-Portal oder über die VM-Zugriffserweiterung in Azure PowerShell zurückgesetzt werden. Wenn Sie PowerShell verwenden, stellen Sie sicher, dass das [neueste PowerShell-Modul installiert und konfiguriert](/powershell/azureps-cmdlets-docs) ist und Sie bei Ihrem Azure-Abonnement angemeldet sind. Sie können diese Schritte auch für virtuelle Computer durchführen, die mit dem [klassischen Bereitstellungsmodell](windows/classic/reset-rdp.md) erstellt wurden.

## <a name="ways-to-reset-configuration-or-credentials"></a>Methoden zum Zurücksetzen der Konfiguration oder der Anmeldeinformationen
Sie können Remotedesktopdienste und -Anmeldeinformationen auf verschiedene Weise zurücksetzen, je nach Ihren Anforderungen:

- [Zurücksetzen mithilfe des Azure-Portals](#azure-portal)
- [Zurücksetzen mithilfe von Azure PowerShell](#vmaccess-extension-and-powershell)

## <a name="azure-portal"></a>Azure-Portal
Klicken Sie zum Erweitern des Portalmenüs oben links auf die drei Balken und dann auf **Virtuelle Computer**:

![Navigieren Sie zu Ihrem virtuellen Azure-Computer](./media/virtual-machines-windows-reset-rdp/Portal-Select-VM.png)

### <a name="reset-the-local-administrator-account-password"></a>**Zurücksetzen des Kennworts eines lokalen Administratorkontos**

Wählen Sie Ihren virtuellen Windows-Computer aus, und klicken Sie dann auf **Support und Problembehandlung** > **Zurücksetzen des Kennworts**. Das Blatt zum Zurücksetzen des Kennworts wird angezeigt:

![Seite „Zurücksetzen des Kennworts“](./media/virtual-machines-windows-reset-rdp/Portal-RM-PW-Reset-Windows.png)

Geben Sie den Benutzernamen und ein neues Kennwort ein, und klicken Sie anschließend auf **Aktualisieren**. Versuchen Sie erneut, eine Verbindung mit Ihrem virtuellen Computer herzustellen.

> [!NOTE] 
> - Nachdem Sie das Kennwort geändert haben und der Vorgang im Portal abgeschlossen wurde, kann es 3 bis 5 Minuten dauern, bis diese Änderung auf dem virtuellen Computer wirksam wird. Wenn die Änderung dann jedoch nicht wirksam wird, starten Sie den virtuellen Computer neu.
> - Die VMAccess-Erweiterung funktioniert nur mit dem integrierten lokalen Administratorkonto und wirkt sich auf keine andere lokale ID oder Domänen-ID aus.
> - Wenn der Zielcomputer ein Domänencontroller ist, wird die Erweiterung zurückgesetzt oder das Domänenadministratorkonto umbenannt.


### <a name="reset-the-remote-desktop-service-configuration"></a>**Zurücksetzen der Konfiguration des Remotedesktopdiensts**

Wählen Sie Ihren virtuellen Windows-Computer aus, und klicken Sie dann auf **Support und Problembehandlung** > **Zurücksetzen des Kennworts**. Das Blatt zum Zurücksetzen des Kennworts wird wie folgt angezeigt:

![Zurücksetzen der RDP-Konfiguration](./media/virtual-machines-windows-reset-rdp/Portal-RM-RDP-Reset.png)

Wählen Sie im Dropdownmenü **Nur Konfiguration zurücksetzen** aus, und klicken Sie dann auf **Aktualisieren**. Versuchen Sie erneut, eine Verbindung mit Ihrem virtuellen Computer herzustellen.


## <a name="vmaccess-extension-and-powershell"></a>VMAccess-Erweiterung und PowerShell
Stellen Sie mit dem Cmdlet `Login-AzureRmAccount` sicher, dass das [neueste PowerShell-Modul installiert und konfiguriert](/powershell/azureps-cmdlets-docs) ist und Sie bei Ihrem Azure-Abonnement angemeldet sind.

### <a name="reset-the-local-administrator-account-password"></a>**Zurücksetzen des Kennworts eines lokalen Administratorkontos**
Sie setzen das Administratorkennwort oder den Benutzernamen mithilfe des PowerShell-Cmdlets [Set-AzureRmVMAccessExtension](https://msdn.microsoft.com/library/mt619447.aspx) zurück. Erstellen Sie die Anmeldeinformationen für Ihr Konto wie folgt:

```powershell
$cred=Get-Credential
```

> [!NOTE] 
> Wenn Sie einen anderen Namen als das aktuelle lokale Administratorkonto auf dem virtuellen Computer eingeben, benennt die VMAccess-Erweiterung das lokale Administratorkonto um, weist diesem Konto das von Ihnen angegebene Kennwort zu und gibt ein Ereignis zum Abmelden von Remotedesktop aus. Wenn das lokale Administratorkonto auf dem virtuellen Computer deaktiviert ist, wird es durch die VMAccess-Erweiterung aktiviert.

Im folgenden Beispiel wird die VM `myVM` in der Ressourcengruppe `myResourceGroup` mit den angegebenen Anmeldeinformationen aktualisiert.

```powershell
Set-AzureRmVMAccessExtension -ResourceGroupName "myResourceGroup" -VMName "myVM" `
    -Name "myVMAccess" -Location WestUS -UserName $cred.GetNetworkCredential().Username `
    -Password $cred.GetNetworkCredential().Password -typeHandlerVersion "2.0"
```

### <a name="reset-the-remote-desktop-service-configuration"></a>**Zurücksetzen der Konfiguration des Remotedesktopdiensts**
Sie setzen den Remotezugriff auf Ihren virtuellen Computer mit dem PowerShell-Cmdlet [Set-AzureRmVMAccessExtension](https://msdn.microsoft.com/library/mt619447.aspx) zurück. Im folgenden Beispiel wird die Zugriffserweiterung `myVMAccess` in der VM `myVM` in der Ressourcengruppe `myResourceGroup` zurückgesetzt:

```powershell
Set-AzureRmVMAccessExtension -ResourceGroupName "myResoureGroup" -VMName "myVM" `
    -Name "myVMAccess" -Location WestUS -typeHandlerVersion "2.0 -ForceRerun
```

> [!TIP]
> Zu jedem Zeitpunkt kann eine VM nur einen einzelnen VM-Zugriffs-Agent haben. Sie können für das Festlegen der Eigenschaften des VM-Zugriffs-Agents die Option `-ForceRerun` verwenden. Verwenden Sie bei Verwendung von `-ForceRerun` unbedingt denselben Namen für den VM-Zugriffs-Agent, den Sie in vorherigen Befehlen verwendet haben.

Wenn Sie weiterhin nicht remote auf den virtuellen Computer zugreifen können, finden Sie weitere Schritte unter [Problembehandlung bei Remotedesktopverbindungen mit einem Windows-basierten virtuellen Azure-Computer](virtual-machines-windows-troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).


## <a name="next-steps"></a>Nächste Schritte
Falls die Erweiterung für den Zugriff auf virtuelle Computer nicht reagiert und Sie das Kennwort nicht zurücksetzen können, können Sie [das lokale Windows-Kennwort offline zurücksetzen](virtual-machines-windows-reset-local-password-without-agent.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Bei dieser Methode handelt es sich um einen fortgeschritteneren Prozess, bei dem Sie die virtuelle Festplatte des problematischen virtuellen Computers mit einem anderen virtuellen Computer verbinden müssen. Führen Sie zunächst die in diesem Artikel dokumentierten Schritte aus. Die Methode zur Offlinezurücksetzung des Kennworts sollte nur als letzter Ausweg verwendet werden.

[Azure-VM-Erweiterungen und Features](virtual-machines-windows-extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

[Herstellen einer Verbindung mit einem virtuellen Azure-Computer über RDP oder SSH](http://msdn.microsoft.com/library/azure/dn535788.aspx)

[Problembehandlung bei Remotedesktopverbindungen mit einem Windows-basierten virtuellen Azure-Computer](virtual-machines-windows-troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)


