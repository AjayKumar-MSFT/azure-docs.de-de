In diesem Artikel werden verschiedene bewährte Methoden zur Ausführung einer Linux-VM in Azure beschrieben. Dabei geht es vor allem um Skalierbarkeit, Verfügbarkeit, Verwaltbarkeit und Sicherheit. Azure unterstützt die Ausführung verschiedener beliebter Linux-Distributionen, z.B. CentOS, Debian, Red Hat Enterprise, Ubuntu und FreeBSD. Weitere Informationen finden Sie unter [Azure und Linux][azure-linux].

> [!NOTE]
> Azure bietet zwei verschiedene Bereitstellungsmodelle: [Resource Manager][resource-manager-overview] und klassisch. In diesem Artikel wird Resource Manager verwendet, der von Microsoft für neue Bereitstellungen empfohlen wird.
> 
> 

Wir raten davon ab, für unternehmenskritische Workloads nur eine VM zu verwenden, weil so eine einzelne Fehlerquelle („Single Point of Failure“) entsteht. Stellen Sie mehrere VMs in einer [Verfügbarkeitsgruppe][availability-set] bereit, um eine höhere Verfügbarkeit zu erzielen. Weitere Informationen finden Sie unter [Ausführen mehrerer VMs in Azure][multi-vm]. 

## <a name="architecture-diagram"></a>Architekturdiagramm
Die Bereitstellung einer einzelnen VM in Azure umfasst mehr „bewegliche Teile“ als die VM selbst. Es sind Compute-, Netzwerk- und Speicherelemente vorhanden, die berücksichtigt werden müssen.

> Ein Visio-Dokument, in dem dieses Architekturdiagramm enthalten ist, steht im [Microsoft Download Center][visio-download] zum Herunterladen zur Verfügung. Dieses Diagramm befindet sich auf der Seite „Compute – einzelne VM“.
> 
> 

![[0]][0]

* **Ressourcengruppe:** Eine [*Ressourcengruppe*][resource-manager-overview] ist ein Container, der verwandte Ressourcen enthält. Erstellen Sie eine Ressourcengruppe für die Ressourcen dieser VM.
* **VM**. Sie können eine VM über eine Liste mit veröffentlichten Images oder über eine virtuelle Festplattendatei (VHD) bereitstellen, die Sie in Azure Blob Storage hochladen.
* **Betriebssystem-Datenträger:** Der Betriebssystemdatenträger ist eine virtuelle Festplatte (Virtual Hard Disk, VHD), die in [Azure Storage][azure-storage] gespeichert ist. Dies bedeutet, dass er auch dann noch vorhanden ist, wenn der Hostcomputer ausfällt. Der Betriebssystemdatenträger ist `/dev/sda1`.
* **Temporärer Datenträger** Die VM wird mit einem temporären Datenträger erstellt. Dieser Datenträger wird auf dem Hostcomputer auf einem physischen Laufwerk gespeichert. Er wird *nicht* in Azure Storage gespeichert und kann bei Neustarts und anderen Ereignissen während des VM-Lebenszyklus gelöscht werden. Verwenden Sie diesen Datenträger nur für temporäre Daten, z. B. Auslagerungsdateien. Der temporäre Datenträger ist `/dev/sdb1` und wird in `/mnt/resource` oder `/mnt` bereitgestellt.
* **Datenträger** Bei einem [Datenträger für Daten][data-disk] handelt es sich um eine persistente VHD, die für Anwendungsdaten verwendet wird. Datenträger werden in Azure Storage gespeichert, z.B. auf dem Betriebssystem-Datenträger.
* **Virtuelles Netzwerk (VNet) und Subnetz_** Jede VM in Azure wird in einem VNet bereitgestellt, das weiter in Subnetze unterteilt wird.
* **Öffentliche IP-Adresse** Eine öffentliche IP-Adresse wird für die Kommunikation mit der VM benötigt, z.B. über SSH.
* **Netzwerkschnittstelle (NIC)** Die NIC ermöglicht der VM die Kommunikation mit dem virtuellen Netzwerk.
* **Netzwerksicherheitsgruppen (NSG)** Die [Netzwerksicherheitsgruppe][nsg] wird verwendet, um den Netzwerkdatenverkehr in das Subnetz zuzulassen oder zu verweigern. Sie können eine NSG einer einzelnen NIC oder einem Subnetz zuordnen. Wenn Sie sie einem Subnetz zuordnen, gelten die NSG-Regeln für alle VMs in diesem Subnetz.
* **Diagnose:** Diagnoseprotokolle sind für die Verwaltung und Problembehandlung des virtuellen Computers von entscheidender Bedeutung.

## <a name="recommendations"></a>Recommendations

Die folgenden Empfehlungen gelten für die meisten Szenarios. Sofern Sie keine besonderen Anforderungen haben, die Vorrang haben, sollten Sie diese Empfehlungen befolgen. 

### <a name="vm-recommendations"></a>Empfehlungen für virtuelle Computer

In Azure werden viele verschiedene VM-Größen angeboten. Wir raten aber zur DS- und GS-Serie, da für diese Computergrößen [Storage Premium][premium-storage] unterstützt wird. Wählen Sie eine dieser Computergrößen, falls bei Ihnen nicht eine spezielle Workload erforderlich ist, z.B. High Performance Computing. Ausführliche Informationen finden Sie unter [Größen virtueller Computer][virtual-machine-sizes].

Starten Sie beim Verschieben einer vorhandenen Workload in Azure mit der VM-Größe, die Ihren lokalen Servern am ehesten entspricht. Messen Sie dann die Leistung Ihres tatsächlichen Workloads hinsichtlich CPU, Arbeitsspeicher und Datenträger-IOPS (E/A-Vorgänge pro Sekunde), und passen Sie die Größe bei Bedarf an. Wenn Sie mehrere Netzwerkschnittstellenkarten für Ihre VM benötigen, sollten Sie sich darüber im Klaren sein, dass die maximale Anzahl von Netzwerkkarten eine Funktion der [VM-Größe][vm-size-tables] ist.

Wenn Sie die VM und anderen Ressourcen bereitstellen, müssen Sie eine Region angeben. Es ist im Allgemeinen ratsam, eine Region zu wählen, der sich in der Nähe Ihrer internen Benutzer oder Ihrer Kunden befindet. Es sind aber nicht alle VM-Größen in allen Regionen verfügbar. Weitere Informationen finden Sie unter [Dienste nach Region][services-by-region]. Führen Sie den folgenden Befehl der Azure-Befehlszeilenschnittstelle (CLI) aus, um die in einer bestimmten Region verfügbaren VM-Größen aufzulisten:

```
azure vm sizes --location <location>
```

Informationen zum Auswählen eines veröffentlichten VM-Image finden Sie unter [Auswählen von Linux-VM-Images mit der Azure-CLI][select-vm-image].

### <a name="disk-and-storage-recommendations"></a>Empfehlungen für Datenträger und Speicher

Zum Erzielen einer optimalen E/A-Leistung empfehlen wir [Storage Premium][premium-storage] zum Speichern von Daten auf SSDs (Solid State Drives). Die Kosten basieren auf der Größe des bereitgestellten Datenträgers. IOPS und Durchsatz (also die Datenübertragungsrate) richten sich ebenfalls nach der Datenträgergröße. Berücksichtigen Sie beim Bereitstellen eines Datenträgers also alle drei Faktoren (Kapazität, IOPS und Durchsatz). 

Erstellen Sie separate Azure Storage-Konten für jeden virtuellen Computer, auf denen die virtuellen Festplatten (VHDs) gespeichert werden, um die IOPS-Grenzwerte für Storage-Konten zu vermeiden. 

Fügen Sie einen oder mehrere Datenträger hinzu. Wenn Sie eine virtuelle Festplatte (VHD) erstellen, ist sie nicht formatiert. Melden Sie sich an der VM an, um den Datenträger zu formatieren. In der Linux-Shell werden Datenträger für Daten als `/dev/sdc`, `/dev/sdd` usw. angezeigt. Sie können `lsblk` ausführen, um die Blockgeräte einschließlich der Datenträger aufzulisten. Um einen Datenträger für Daten zu verwenden, erstellen Sie eine Partition und ein Dateisystem und binden den Datenträger ein. Beispiel:

```bat
# Create a partition.
sudo fdisk /dev/sdc     # Enter 'n' to partition, 'w' to write the change.     

# Create a file system.
sudo mkfs -t ext3 /dev/sdc1

# Mount the drive.
sudo mkdir /data1
sudo mount /dev/sdc1 /data1
```

Wenn Sie über eine große Zahl von Datenträgern verfügen, sollten Sie sich über die E/A-Grenzwerte des Speicherkontos bewusst sein. Weitere Informationen finden Sie unter [Grenzwerte für Datenträger virtueller Computer][vm-disk-limits].

Wenn Sie einen Datenträger für Daten hinzufügen, wird dem Datenträger eine logische Einheitennummer (Logical Unit Number, LUN) zugewiesen. Optional können Sie die LUN-ID angeben, z.B. wenn Sie einen Datenträger austauschen und dieselbe LUN-ID beibehalten möchten oder über eine Anwendung verfügen, die nach einer bestimmten LUN-ID sucht. Beachten Sie jedoch, dass die LUN-IDs für jeden Datenträger eindeutig sein muss.

Es kann ratsam sein, den E/A-Scheduler zu ändern, um die Leistung auf SSDs zu optimieren. Bei den Datenträgern für VMs mit Storage Premium-Konten handelt es sich nämlich um SSDs. Eine übliche Empfehlung ist die Verwendung des NOOP-Schedulers für SSDs. Sie sollten aber ein Tool wie [iostat] zur Überwachung der E/A-Datenträgerleistung für Ihre spezifische Workload einsetzen.

Erstellen Sie ein separates Speicherkonto zum Speichern der Diagnoseprotokolle, um eine optimale Leistung zu erzielen. Ein standardmäßiger lokal redundanter Speicher (LRS) reicht für Diagnoseprotokolle aus.

### <a name="network-recommendations"></a>Netzwerkempfehlungen

Die öffentliche IP-Adresse kann dynamisch oder statisch sein. Die Standardeinstellung ist „Dynamisch“.

* Reservieren Sie eine [statische IP-Adresse][static-ip], falls Sie eine statische IP-Adresse benötigen, die sich nicht ändert – z.B. wenn Sie einen A-Eintrag in DNS erstellen oder die IP-Adresse auf eine Liste mit sicheren Adressen setzen müssen.
* Sie können auch einen vollständig qualifizierten Domänennamen (FQDN) für die IP-Adresse erstellen. Sie können anschließend einen [CNAME-Eintrag][cname-record] in DNS registrieren, der auf den FQDN verweist. Weitere Informationen finden Sie unter [Erstellen eines vollqualifizierten Domänennamens im Azure-Portal][fqdn].

Alle Netzwerksicherheitsgruppen enthalten eine Reihe von [Standardregeln][nsg-default-rules], einschließlich einer Regel, die den gesamten eingehenden Internetverkehr blockiert. Die Standardregeln können nicht gelöscht, aber von anderen Regeln überschrieben werden. Um Internetdatenverkehr zu ermöglichen, erstellen Sie Regeln, die eingehenden Datenverkehr an bestimmten Ports zulassen, z.B. Port 80 für HTTP.  

Fügen Sie zum Aktivieren von SSH der NSG eine Regel hinzu, die den eingehenden Datenverkehr am TCP-Port 22 zulässt.

## <a name="scalability-considerations"></a>Überlegungen zur Skalierbarkeit

[Ändern Sie die VM-Größe][vm-resize], wenn Sie zentral hoch- oder herunterskalieren. 

Um horizontal zu skalieren, platzieren Sie zwei oder mehr VMs in einer Verfügbarkeitsgruppe hinter einem Load Balancer. Weitere Informationen finden Sie unter [Ausführen mehrerer virtueller Computer in Azure][multi-vm].

## <a name="availability-considerations"></a>Überlegungen zur Verfügbarkeit

Stellen Sie mehrere VMs in einer Verfügbarkeitsgruppe bereit, um eine höhere Verfügbarkeit zu erzielen. Dies führt auch zu einer Erhöhung der [Vereinbarung zum Servicelevel][vm-sla] (SLA). 

Ihr virtueller Computer kann von einer [geplanten Wartung][planned-maintenance] oder [ungeplanten Wartung][manage-vm-availability] betroffen sein. Sie können [VM-Neustartprotokolle][reboot-logs] verwenden, um zu ermitteln, ob ein VM-Neustart durch einen geplanten Wartungsvorgang verursacht wurde.

VHDs werden in [Azure-Speicher][azure-storage] gespeichert, und der Azure-Speicher wird repliziert, um Dauerhaftigkeit und Verfügbarkeit sicherzustellen.

Als Schutz vor versehentlichen Datenverlusten während des normalen Betriebs (z.B. aufgrund eines Benutzerfehlers) sollten Sie auch Point-in-Time-Sicherungen implementieren, indem Sie [Blobmomentaufnahmen][blob-snapshot] oder ein anderes Tool verwenden.

## <a name="manageability-considerations"></a>Überlegungen zur Verwaltbarkeit

**Ressourcengruppen.** Legen Sie eng miteinander verknüpfte Ressourcen mit demselben Lebenszyklus in derselben [Ressourcengruppe][resource-manager-overview] ab. Ressourcengruppen ermöglichen Ihnen das Bereitstellen und Überwachen von Ressourcen und das Zusammenfassen von Abrechnungskosten nach Ressourcengruppe. Sie können auch Ressourcen als Gruppe löschen, was für Testbereitstellungen sehr nützlich ist. Versehen Sie Ressourcen mit aussagekräftigen Namen. Dies vereinfacht das Auffinden einer bestimmten Ressource und Verstehen ihrer Rolle. Siehe [Empfohlene Benennungskonventionen für Azure-Ressourcen][naming conventions].

**SSH**. Bevor Sie eine Linux-VM erstellen, wird ein 2048-Bit-RSA-Paar aus privatem und öffentlichem Schlüssel generiert. Verwenden Sie die öffentliche Schlüsseldatei bei der Erstellung der VM. Weitere Informationen finden Sie unter [Verwenden von SSH mit Linux und Mac in Azure][ssh-linux].

**VM-Diagnose.** Aktivieren Sie die Überwachung und Diagnose, einschließlich grundlegender Integritätsmetriken, Infrastrukturprotokolle zur Diagnose sowie [Startdiagnose][boot-diagnostics]. Startdiagnosen dienen dazu, einen Fehler beim Startvorgang zu untersuchen, wenn sich Ihre VM in einem nicht startfähigen Zustand befindet. Weitere Informationen finden Sie unter [Aktivieren von Überwachung und Diagnose][enable-monitoring].  

Der folgende CLI-Befehl aktiviert die Diagnose:

```
azure vm enable-diag <resource-group> <vm-name>
```

**Beenden einer VM.** Unter Azure wird zwischen den Zuständen „Stopped“ (Beendet) und „Deallocated“ (Zuordnung aufgehoben) unterschieden. Ihnen werden nur Gebühren berechnet, wenn der VM-Status angehalten wird, aber nicht, wenn die Zuordnung für den virtuellen Computer aufgehoben wurde.

Verwenden Sie den folgenden CLI-Befehl, um die Zuordnung für eine VM aufzuheben:

```
azure vm deallocate <resource-group> <vm-name>
```

Sie können die Zuordnung des virtuellen Computers auch mit der Schaltfläche **Beenden** im Azure-Portal aufheben. Wenn das Herunterfahren über das Betriebssystem erfolgt, während Sie angemeldet sind, wird der virtuelle Computer zwar beendet, aber die Zuordnung wird *nicht* aufgehoben. Es fallen also weiter Kosten an.

**Löschen einer VM.** Wenn Sie eine VM löschen, werden die VHDs nicht gelöscht. Dies bedeutet, dass Sie die VM problemlos löschen können, ohne dass Daten verloren gehen. Allerdings wird Ihnen der Speicherplatz weiter in Rechnung gestellt. Um die VHD zu löschen, löschen Sie die Datei aus dem [Blobspeicher][blob-storage].

Zur Verhinderung des versehentlichen Löschens verwenden Sie eine [Ressourcensperre][resource-lock], um die gesamte Ressourcengruppe oder einzelne Ressourcen, z.B. den virtuellen Computer, zu sperren. 

## <a name="security-considerations"></a>Sicherheitshinweise

Automatisieren Sie Betriebssystemupdates mithilfe der VM-Erweiterung [OSPatching] . Installieren Sie diese Erweiterung, wenn Sie die VM bereitstellen. Sie können angeben, wie oft Patches installiert werden sollen und ob im Anschluss daran ein Neustart erfolgen soll.

Arbeiten Sie mit der [rollenbasierten Zugriffssteuerung][rbac] (Role-Based Access Control, RBAC) um den Zugriff auf die von Ihnen bereitgestellten Azure-Ressourcen zu steuern. Mithilfe der RBAC können Sie Mitglieder Ihres DevOps-Teams Autorisierungsrollen zuweisen. Die Rolle „Leser“ kann beispielsweise Azure-Ressourcen anzeigen, diese aber nicht erstellen, verwalten oder löschen. Einige Rollen sind für bestimmte Azure-Ressourcentypen spezifisch. Die VM-Rolle „Mitwirkender“ kann z.B. eine VM neu starten oder ihre Zuordnung aufheben, das Administratorkennwort zurücksetzen, eine VM erstellen usw. Andere [integrierte RBAC-Rollen][rbac-roles], die für diese Referenzarchitektur nützlich sein können, sind u.a. [DevTest Labs-Benutzer][rbac-devtest] und [Netzwerkmitwirkender][rbac-network]. 

Ein Benutzer kann mehreren Rollen zugewiesen werden. Außerdem können Sie für noch präzisere Berechtigungen benutzerdefinierte Rollen erstellen.

d> [!NOTE]
> Die RBAC schränkt nicht die Aktionen eines Benutzers ein, der bei einer VM angemeldet ist. Diese Berechtigungen werden vom Kontotyp im Gastbetriebssystem bestimmt.   
> 
> 

Verwenden Sie [Überwachungsprotokolle][audit-logs], um Bereitstellungsaktionen und andere VM-Ereignisse anzuzeigen.

Ziehen Sie [Azure Disk Encryption][disk-encryption] in Betracht, wenn Sie die Datenträger für Betriebssystem und Daten verschlüsseln müssen. 

## <a name="solution-deployment"></a>Bereitstellung von Lösungen
Eine Bereitstellung für diese Referenzarchitektur ist auf [GitHub][github-folder] verfügbar. Sie enthält ein VNet, eine NSG und einen einzelnen virtuellen Computer. Um die Architektur bereitzustellen, gehen Sie folgendermaßen vor: 

1. Klicken Sie mit der rechten Maustaste auf die Schaltfläche unten, und wählen Sie entweder „Link in neuer Registerkarte öffnen“ oder „Link in neuem Fenster öffnen“.
   [![Bereitstellen in Azure](../articles/guidance/media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-compute-single-vm%2Fazuredeploy.json)
2. Nachdem der Link im Azure-Portal geöffnet wurde, müssen Sie Werte für einige Einstellungen eingeben: 
   
   * Der Name der **Ressourcengruppe** ist bereits in der Parameterdatei definiert. Wählen Sie also **Neu erstellen**, und geben Sie im Textfeld `ra-single-vm-rg` ein.
   * Wählen Sie im Dropdownfeld **Standort** die Region aus.
   * Lassen Sie die Textfelder für den **Vorlagenstamm-URI** bzw. **Parameterstamm-URI** unverändert.
verfügbar.   * Wählen Sie im Dropdownfeld **Betriebssystemtyp** die Option **Linux**.
   * Überprüfen Sie die allgemeinen Geschäftsbedingungen, und aktivieren Sie dann das Kontrollkästchen **Ich stimme den oben genannten Geschäftsbedingungen zu**.
   * Klicken Sie auf die Schaltfläche **Kaufen**.
3. Warten Sie, bis die Bereitstellung abgeschlossen ist.
4. Die Parameterdateien enthalten einen hartcodierten Administratorbenutzernamen und das dazugehörige Kennwort, und es wird dringend empfohlen, beides sofort zu ändern. Klicken Sie im Azure-Portal auf den virtuellen Computer mit dem Namen `ra-single-vm0 `. Klicken Sie im Abschnitt **Support + Problembehandlung** dann auf **Kennwort zurücksetzen**. Wählen Sie im Dropdownfeld **Modus** die Option **Kennwort zurücksetzen**, und wählen Sie dann einen neuen **Benutzernamen** und ein **Kennwort** aus. Klicken Sie auf die Schaltfläche **Aktualisieren**, um den neuen Benutzernamen und das Kennwort dauerhaft zu übernehmen.

## <a name="next-steps"></a>Nächste Schritte
Stellen Sie zwei oder mehr VMs hinter einem Lastenausgleichsmodul bereit, um eine höhere Verfügbarkeit zu erzielen. Weitere Informationen finden Sie unter [Ausführen mehrerer VMs in Azure][multi-vm].

<!-- links -->

[audit-logs]: https://azure.microsoft.com/en-us/blog/analyze-azure-audit-logs-in-powerbi-more/
[availability-set]:../articles/virtual-machines/windows/create-availability-set.md
[azure-cli]: /cli/azure/get-started-with-az-cli2
[azure-linux]:../articles/virtual-machines/linux/overview.md
[azure-storage]: ../articles/storage/storage-introduction.md
[blob-snapshot]: ../articles/storage/storage-blob-snapshots.md
[blob-storage]: ../articles/storage/storage-introduction.md
[boot-diagnostics]: https://azure.microsoft.com/en-us/blog/boot-diagnostics-for-virtual-machines-v2/
[cname-record]: https://en.wikipedia.org/wiki/CNAME_record
[data-disk]: ../articles/storage/storage-about-disks-and-vhds-linux.md
[disk-encryption]: ../articles/security/azure-security-disk-encryption.md
[enable-monitoring]: ../articles/monitoring-and-diagnostics/insights-how-to-use-diagnostics.md
[fqdn]:../articles/virtual-machines/linux/portal-create-fqdn.md
[github-folder]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-single-vm/
[iostat]: https://en.wikipedia.org/wiki/Iostat
[manage-vm-availability]:../articles/virtual-machines/linux/manage-availability.md
[multi-vm]: ../articles/guidance/guidance-compute-multi-vm.md
[naming conventions]: ../articles/guidance/guidance-naming-conventions.md
[nsg]: ../articles/virtual-network/virtual-networks-nsg.md
[nsg-default-rules]: ../articles/virtual-network/virtual-networks-nsg.md#default-rules
[OSPatching]: https://github.com/Azure/azure-linux-extensions/tree/master/OSPatching
[planned-maintenance]:../articles/virtual-machines/linux/planned-maintenance.md
[premium-storage]: ../articles/storage/storage-premium-storage.md
[rbac]: ../articles/active-directory/role-based-access-control-what-is.md
[rbac-roles]: ../articles/active-directory/role-based-access-built-in-roles.md
[rbac-devtest]: ../articles/active-directory/role-based-access-built-in-roles.md#devtest-labs-user
[rbac-network]: ../articles/active-directory/role-based-access-built-in-roles.md#network-contributor
[reboot-logs]: https://azure.microsoft.com/en-us/blog/viewing-vm-reboot-logs/
[Resize-VHD]: https://technet.microsoft.com/en-us/library/hh848535.aspx
[Resize virtual machines]: https://azure.microsoft.com/en-us/blog/resize-virtual-machines/
[resource-lock]: ../articles/resource-group-lock-resources.md
[resource-manager-overview]: ../articles/azure-resource-manager/resource-group-overview.md
[select-vm-image]:../articles/virtual-machines/linux/cli-ps-findimage.md
[services-by-region]: https://azure.microsoft.com/en-us/regions/#services
[ssh-linux]:../articles/virtual-machines/linux/mac-create-ssh-keys.md
[static-ip]: ../articles/virtual-network/virtual-networks-reserved-public-ip.md
[storage-account-limits]: ../articles/azure-subscription-service-limits.md#storage-limits
[storage-price]: https://azure.microsoft.com/pricing/details/storage/
[virtual-machine-sizes]:../articles/virtual-machines/linux/sizes.md
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[vm-disk-limits]: ../articles/azure-subscription-service-limits.md#virtual-machine-disk-limits
[vm-resize]:../articles/virtual-machines/linux/change-vm-size.md
[vm-size-tables]:../articles/virtual-machines/windows/sizes.md#size-tables
[vm-sla]: https://azure.microsoft.com/support/legal/sla/virtual-machines
[readme]: https://github.com/mspnp/reference-architectures/blob/master/guidance-compute-single-vm
[components]: #Solution-components
[blocks]: https://github.com/mspnp/template-building-blocks
[0]: ./media/guidance-blueprints/compute-single-vm.png "Architektur einer einzelnen Linux-VM in Azure"

