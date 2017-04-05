



Weitere Informationen zu den VM-Agents und deren Unterstützung von VM-Erweiterungen finden Sie unter [Übersicht über VM-Agents und VM-Erweiterungen](../articles/virtual-machines/windows/classic/manage-extensions.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

## <a name="azure-vm-extensions"></a>Azure-VM-Erweiterungen
VM-Erweiterungen implementieren die meisten der wichtigsten Funktionen, die Sie für Ihre virtuellen Computer verwenden möchten, darunter Grundfunktionen wie das Zurücksetzen von Kennwörtern, das Konfigurieren von RDP und viele andere. Da ständig neue Erweiterungen hinzugefügt werden, nimmt die Anzahl der möglichen Features, die Ihre virtuellen Computer in Azure unterstützen, weiter zu. Standardmäßig werden mehrere grundlegende VM-Erweiterungen installiert, wenn Sie Ihren virtuellen Computer über den Imagekatalog erstellen, einschließlich **IaaSDiagnostics** (derzeit nur virtuelle Windows-Computer), **VMAccess** und **BGInfo** (derzeit nur für Windows). Jedoch werden nicht alle Erweiterungen zu einem bestimmten Zeitpunkt unter Windows und Linux implementiert, da es ständig Featureupdates und neue Erweiterungen gibt.

## <a name="connectivity-and-basic-management"></a>Konnektivität und grundlegende Verwaltung
Die folgenden Erweiterungen sind entscheidend zum Aktivieren, Reaktivieren oder Deaktivieren der Konnektivität Ihrer virtuellen Computer, nachdem sie erstellt wurden und ausgeführt werden.

| Name der VM-Erweiterung | Featurebeschreibung | Weitere Informationen |
| --- | --- | --- |
| VMAccessAgent (Windows) |Erstellen, Aktualisieren und Zurücksetzen von Benutzerinformationen und RDP-Verbindungskonfigurationen. |[Windows](../articles/virtual-machines/windows/classic/extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) |
| VMAccessForLinux (Linux) |Erstellen, Aktualisieren und Zurücksetzen von Benutzerinformationen und SSH-Verbindungskonfigurationen. |[Linux](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess) |

## <a name="deployment-and-configuration-management"></a>Bereitstellungs- und Konfigurationsverwaltung
Die folgenden Erweiterungen unterstützen verschiedene Arten von Szenarien und Features für die Bereitstellungs- und Konfigurationsverwaltung. Weitere Informationen finden Sie auch weiter unten im Abschnitt über Betrieb und Verwaltung virtueller Computer.

| Name der VM-Erweiterung | Featurebeschreibung | Weitere Informationen |
| --- | --- | --- |
| **MSEnterpriseApplication** |Implementiert Features für die Unterstützung durch Windows System Center. |[System Center 2012 R2 – Rollen virtueller Computer](http://social.technet.microsoft.com/wiki/contents/articles/18274.system-center-2012-r2-virtual-machine-role-authoring-guide-resource-extension-package.aspx) |
| **Octopus Deploy** (basiert auf DSC-Erweiterung) |Unterstützt die automatisierte Bereitstellung von ASP.NET-Webanwendungen und Windows-Diensten in Entwicklungs-, Test- und Produktionsumgebungen. |[Getting Started with Octopus Deploy (in englischer Sprache)](http://docs.octopusdeploy.com/display/OD/Getting%20started) |
| **Visual Studio Release Manager** (basiert auf DSC-Erweiterung) |Unterstützt die kontinuierliche Bereitstellung mit Visual Studio. |[Automatisieren von Bereitstellungen mit Release Management](https://msdn.microsoft.com/Library/vs/alm/Release/overview) |
| **CentosChefClient** | | |
| **ChefClient** |Erstellt einen Chef-Client unter Windows. (Kann auch die weiter unten aufgeführte DSC-Erweiterung verwenden.) |[Chef und Microsoft Azure](https://www.getchef.com/solutions/azure/) |
| **LinuxChefClient** | | |
| **DockerExtension** |Installiert den Docker-Daemon zur Unterstützung von Docker-Remotebefehlen. |[Verwenden der Docker-Erweiterung für virtuelle Computer](../articles/virtual-machines/linux/dockerextension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) Ausführlichere Informationen finden Sie im [Benutzerhandbuch für die Docker-VM-Erweiterung](https://github.com/Azure/azure-docker-extension/blob/master/README.md). |
| **DSC** |PowerShell-DSC-Erweiterung (Desired State Configuration, Konfigurieren des gewünschten Zustands). |[Azure PowerShell DSC (Desired State Configuration) extension (in englischer Sprache)](http://blogs.msdn.com/b/powershell/archive/2014/08/07/introducing-the-azure-powershell-dsc-desired-state-configuration-extension.aspx) |
| **PuppetEnterpriseAgent** |Implementiert die Features von Puppet Enterprise. |[Puppet on Azure (in englischer Sprache)](http://puppetlabs.com/solutions/microsoft) |
| **CustomScriptExtension** (Windows)**CustomScriptForLinux** (Linux) |Ruft benutzerdefinierte Skripts auf dem virtuellen Computer zu einem beliebigen Zeitpunkt auf: beim Start oder während der Lebensdauer. |[Benutzerdefinierte Skripterweiterung](../articles/virtual-machines/windows/classic/extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) |
| **AzureCATExtensionHandler** |Nutzt die von **IaaSDiagnostics** und einigen anderen Datenquellen wie [Metriken der Azure-Speicheranalyse](https://msdn.microsoft.com/library/azure/hh343270.aspx) erfassten Diagnosedaten und wandelt diese in einen aggregierten Datensatz um, der vom SAP-Hoststeuerungprozess verwendet werden kann. |[Azure Enhanced Monitoring for SAP (in englischer Sprache)](https://azure.microsoft.com/blog/2014/06/04/azure-enhanced-monitoring-for-sap/) |

## <a name="security-and-protection"></a>Sicherheit und Schutz
Die Erweiterungen in diesem Abschnitt bieten wichtige Sicherheitsfeatures für Ihre Azure-VMs.

| Name der VM-Erweiterung | Featurebeschreibung | Weitere Informationen |
| --- | --- | --- |
| **CloudLinkSecureVMWindowsAgent** |Bietet Microsoft Azure-Kunden die Möglichkeit, die Daten der virtuellen Computer in einer freigegebenen Infrastruktur mit mehreren Mandanten zu verschlüsseln und die Verschlüsselungsschlüssel für die verschlüsselten Daten in der Azure-Speicherinfrastruktur vollständig zu kontrollieren. |[Securing Microsoft Azure Virtual Machines leveraging BitLocker and Native OS encryption (in englischer Sprache)](http://www.cloudlinktech.com/azure) |
| **McAfeeEndpointSecurity** |Schützt Ihren virtueller Computer vor Schadsoftware. |[McAfee](https://www.mcafeeasap.com/MarketingContent/default.aspx) |
| **TrendMicroDSA** |Ermöglicht die Unterstützung der Deep Security-Plattform von TrendMicro, um Angriffserkennung und -verhinderung, Firewall, Antischadsoftware, Web-Reputation, Protokollprüfug und Überwachung der Integrität zu bieten. |[Installieren und Konfigurieren von Trend Micro Deep Security als Dienst auf einem virtuellen Azure-Computer](../articles/virtual-machines/windows/classic/install-trend.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) |
| **PortalProtectExtension** |Schützt vor Bedrohungen der Microsoft SharePoint-Umgebung. |[Securing Your SharePoint Deployment on Azure (in englischer Sprache)](http://blog.trendmicro.com/securing-sharepoint-deployment-azure/) |
| **IaaSAntimalware** |Antischadsoftware für Azure-Clouddienste und virtuelle Computer von Microsoft ist eine Funktion für Echtzeitschutz, die beim Erkennen und Entfernen von Viren, Spyware und anderer Schadsoftware hilft. Sie bietet konfigurierbare Warnungen, wenn bekannte Schadsoftware oder unerwünschte Software versucht, sich selbst zu installieren oder auf Ihrem System zu starten. |[Antimalware für Azure Cloud Services und Virtual Machines](../articles/security/azure-security-antimalware.md) |
| **SymantecEndpointProtection** |Symantec Endpoint Protection 12.1.4 ermöglicht Sicherheit und Leistung für physische und virtuelle Systeme. |[Gewusst wie: Installieren und Konfigurieren von Symantec Endpoint Protection auf einem virtuellen Azure-Computer](../articles/virtual-machines/windows/classic/install-symantec.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) |

## <a name="vm-operations-and-management"></a>Betrieb und Verwaltung virtueller Computer
Unterstützt allgemeine Features und Verhaltensweisen bei der Verwaltung des Betriebs. Weitere Informationen finden Sie weiter oben im Abschnitt zur Bereitstellungs- und Konfigurationsverwaltung.

| **Name der VM-Erweiterung** | Featurebeschreibung | Weitere Informationen |
| --- | --- | --- |
| **AzureVmLogCollector** |Sie können die Erweiterung **AzureVMLogCollector** bedarfsgesteuert verwenden, um eine einmalige Sammlung von Protokollen von einer oder mehreren Clouddienst-VMs (von Web- und Workerrollen) durchzuführen und die gesammelten Dateien an ein Azure-Speicherkonto zu übertragen – alles ohne Remoteanmeldung auf den virtuellen Computern. |[AzureLogCollector-Erweiterung](../articles/virtual-machines/windows/log-collector-extension.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) |
| **IaaSDiagnostics** |Aktiviert, deaktiviert und konfiguriert die Azure-Diagnose und wird auch von **AzureCATExtensionHandler** zur Unterstützung der SAP-Überwachung verwendet. |[Microsoft Azure Virtual Machine Monitoring with Azure Diagnostics Extension (in englischer Sprache)](https://azure.microsoft.com/blog/2014/09/02/windows-azure-virtual-machine-monitoring-with-wad-extension/) |
| **OSPatchingForLinux** |Ermöglicht Administratoren von Azure-VMs das Automatisieren von Betriebssystemupdates für virtuelle Computer mit benutzerdefinierten Konfigurationen. Sie können die Erweiterung OSPatching verwenden, um Betriebssystemupdates für Ihre virtuellen Computer zu konfigurieren, einschließlich der Angabe, wie oft und wann Betriebssystempatches installiert werden sollen, welche Patches installiert werden sollen und der Konfiguration des Verhaltens beim Neustart nach Updates. |[Blogbeitrag zur OSPatching-Erweiterung](https://azure.microsoft.com/blog/2014/10/23/automate-linux-vm-os-updates-using-ospatching-extension/). Weitere Informationen finden Sie in der Infodatei und der Quelle auf GitHub unter [OS Patching Extension](https://github.com/Azure/azure-linux-extensions)(in englischer Sprache). |

## <a name="developing-and-debugging"></a>Entwickeln und Debuggen
Diese VM-Erweiterungen werden der Vollständigkeit halber hier aufgeführt, da sie Unterstützung für Visual Studio-bezogene Features bieten und nicht direkt verwendet werden sollen.

| Name der VM-Erweiterung | Featurebeschreibung | Weitere Informationen |
| --- | --- | --- |
| **VS14CTPDebugger** |Unterstützt das Remotedebuggen von Visual Studio mit dem Azure SDK 2.4 |[Remotedebuggen in Visual Studio](https://msdn.microsoft.com/library/y7f5zaaa.aspx) |
| **VS2013Debugger** |Unterstützt das Remotedebuggen von Visual Studio mit dem Azure SDK 2.4 | |
| **VS2012Debugger** |Unterstützt das Remotedebuggen von Visual Studio mit dem Azure SDK 2.4 | |
| **RemoteDebugVS14CTP** |Unterstützt das Remotedebuggen von Visual Studio mit dem Azure SDK 2.3 | |
| **RemoteDebugVS2013** |Unterstützt das Remotedebuggen von Visual Studio mit dem Azure SDK 2.3 | |
| **RemoteDebugVS2012** |Unterstützt das Remotedebuggen von Visual Studio mit dem Azure SDK 2.3 | |
| **WebDeployForVSDevTest** |Installiert und konfiguriert IIS und Web Deploy unter Windows Server. Ein Entfernen oder Deaktivieren wird nicht unterstützt. | |

## <a name="miscellaneous-features"></a>Verschiedene Features
Diese Erweiterungen bieten Unterstützung für andere Features von virtuellen Computern, die hilfreich sein können.

| Name der VM-Erweiterung | Featurebeschreibung | Weitere Informationen |
| --- | --- | --- |
| **BGInfo** |Stellt eine konsolidierte Ansicht nützlicher Serverinformationen auf dem Desktop dar, wenn RDP verwendet wird. |[Erweiterung "BGInfo"](https://msdn.microsoft.com/library/mt589195.aspx) |
| **HpcVmDrivers** |Installiert, konfiguriert und verwaltet die Netzwerk-Gerätetreiber für das RDMA-Netzwerk (Remote Direct Memory Access) auf virtuellen Computern der Größe A8 oder A9, auf denen Windows Server 2012 R2 oder Windows Server 2012 ausgeführt wird. Ermöglicht virtuellen A8- oder A9-Computern in einem Cluster die Verwendung des RDMA-Netzwerks bei der Ausführung paralleler MPI-Anwendung. |[Informationen zu den rechenintensiven A8-, A9-, A10- und A11-Instanzen](../articles/virtual-machines/windows/a8-a9-a10-a11-specs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) |

