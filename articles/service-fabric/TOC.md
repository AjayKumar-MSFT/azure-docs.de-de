
# Übersicht
## [Was ist Service Fabric?](service-fabric-overview.md)
## [Grundlegendes zu Microservices](service-fabric-overview-microservices.md)
## [Anwendungsszenarien](service-fabric-application-scenarios.md)
## [Architektur](service-fabric-architecture.md)
## [Terminologie](service-fabric-technical-overview.md)
## [Inhaltsroadmap](service-fabric-content-roadmap.md)

# Erste Schritte
## Einrichten der Entwicklungsumgebung
### [Windows](service-fabric-get-started.md)
### [Linux](service-fabric-get-started-linux.md)
### [Mac OS](service-fabric-get-started-mac.md)
## Erstellen Ihrer ersten Anwendung
### [C# unter Windows](service-fabric-create-your-first-application-in-visual-studio.md)
### [Java unter Linux](service-fabric-create-your-first-linux-application-with-java.md)
### [C# unter Linux](service-fabric-create-your-first-linux-application-with-csharp.md)
## [Bereitstellen von Apps in einem lokalen Cluster](service-fabric-get-started-with-a-local-cluster.md)

# Anleitung
## Erstellen einer Anwendung
### [Muster und Szenarien](service-fabric-patterns-and-scenarios.md)
### Grundlagen
#### [Anwendungsmodell](service-fabric-application-model.md)
#### [Packen einer Anwendung](service-fabric-package-apps.md)
#### [Unterstütztes Programmiermodell](service-fabric-choose-framework.md)
#### [Dienststatus](service-fabric-concepts-state.md)
#### [Dienstkommunikation](service-fabric-connect-and-communicate-with-services.md)
#### [Hinzufügen eines Web-Front-Ends](service-fabric-add-a-web-frontend.md)
#### [Dienstmanifestressourcen](service-fabric-service-manifest-resources.md)
#### [Erste Schritte mit dem Eclipse-Plug-In für die Entwicklung mit Java](service-fabric-get-started-eclipse.md)
#### [Verwalten von Apps in Visual Studio](service-fabric-manage-application-in-visual-studio.md)
#### [Konfigurieren sicherer Verbindungen in Visual Studio](service-fabric-visualstudio-configure-secure-connections.md)
#### Debuggen
##### [Debuggen eines C#-Diensts in VS](service-fabric-debugging-your-application.md)
##### [Debuggen eines Java-Diensts in Eclipse](service-fabric-debugging-your-application-java.md)
#### Überwachen und Diagnostizieren
##### [Windows](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)
##### [Linux](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md)
#### [Verwalten von Anwendungsgeheimnissen](service-fabric-application-secret-management.md)  
#### [Konfigurieren von Sicherheitsrichtlinien für Ihre Anwendung](service-fabric-application-runas-security.md)  
#### [Konfigurieren Ihrer Anwendung für mehrere Umgebungen](service-fabric-manage-multiple-environment-app-configuration.md)  
#### [Allgemeine Fehler und Ausnahmen](service-fabric-errors-and-exceptions.md) 

### Ausführbare Gastanwendungsdatei
#### [Bereitstellen einer ausführbaren Gastanwendungsdatei](service-fabric-deploy-existing-app.md)
#### [Bereitstellen mehrerer ausführbarer Gastanwendungsdateien](service-fabric-deploy-multiple-apps.md)

### Containeranwendung
#### [Übersicht](service-fabric-containers-overview.md)
#### [Bereitstellen von Windows Containern](service-fabric-deploy-container.md)
#### [Bereitstellen von Docker-Containern](service-fabric-deploy-container-linux.md)

### Reliable Services-Anwendung
#### [Übersicht](service-fabric-reliable-services-introduction.md)
#### Erste Schritte
##### [C# unter Windows](service-fabric-reliable-services-quick-start.md)
##### [Java unter Linux](service-fabric-reliable-services-quick-start-java.md)
#### [Reliable Services-Lebenszyklus – C#](service-fabric-reliable-services-lifecycle.md)
#### [Reliable Services-Lebenszyklus – Java](service-fabric-reliable-services-lifecycle-java.md)
#### [Zuverlässige Auflistungen](service-fabric-reliable-services-reliable-collections.md)
#### [Verwenden zuverlässiger Auflistungen](service-fabric-work-with-reliable-collections.md)
#### [Konfigurieren](service-fabric-reliable-services-configuration.md)
#### [Notifications](service-fabric-reliable-services-notifications.md)
#### [Sichern und Wiederherstellen](service-fabric-reliable-services-backup-restore.md)
#### [Kommunizieren mit Reliable Services](service-fabric-reliable-services-communication.md)
#### [Sichere Kommunikation mit Reliable Services – C#](service-fabric-reliable-services-secure-communication.md)
#### [Sichere Kommunikation mit Reliable Services – Java](service-fabric-reliable-services-secure-communication-java.md)
##### [ASP.NET Core](service-fabric-reliable-services-communication-aspnetcore.md)
##### [Dienstremoting – C#](service-fabric-reliable-services-communication-remoting.md)
##### [Dienstremoting – Java](service-fabric-reliable-services-communication-remoting-java.md)
##### [WCF](service-fabric-reliable-services-communication-wcf.md)
##### [Reverseproxy](service-fabric-reverseproxy.md)
#### [Erweiterte Verwendung](service-fabric-reliable-services-advanced-usage.md)

### Reliable Actor-Anwendung
#### [Übersicht](service-fabric-reliable-actors-introduction.md)
#### Erste Schritte
##### [C# unter Windows](service-fabric-reliable-actors-get-started.md)
##### [Java unter Linux](service-fabric-reliable-actors-get-started-java.md)
#### [Architektur](service-fabric-reliable-actors-platform.md)
#### [Lebenszyklus und Garbage Collection](service-fabric-reliable-actors-lifecycle.md)
#### [Polymorphie](service-fabric-reliable-actors-polymorphism.md)
#### [Eintrittsinvarianz](service-fabric-reliable-actors-reentrancy.md)
#### [Ereignisse](service-fabric-reliable-actors-events.md) 
#### [Timer und Erinnerungen](service-fabric-reliable-actors-timers-reminders.md)
#### [Zustandsverwaltung](service-fabric-reliable-actors-state-management.md)
#### [Konfigurieren von „KvsActorStateProvider“](service-fabric-reliable-actors-kvsactorstateprovider-configuration.md)
#### [Typserialisierung](service-fabric-reliable-actors-notes-on-actor-type-serialization.md)
#### [Konfigurieren von Kommunikationseinstellungen](service-fabric-reliable-actors-fabrictransportsettings.md) 
#### [Konfigurieren von „ReliableDictionaryActorStateProvider“](service-fabric-reliable-actors-reliabledictionarystateprovider-configuration.md)

## Migrieren aus Cloud Services
### [Unterschiede zwischen Cloud Services und Service Fabric](service-fabric-cloud-services-migration-differences.md)
### [Migrieren zu Service Fabric](service-fabric-cloud-services-migration-worker-role-stateless-service.md)
### [Empfohlene Vorgehensweisen](/azure/architecture/service-fabric/migrate-from-cloud-services)

## Erstellen und Verwalten von Clustern

### Grundlagen
#### [Übersicht](service-fabric-deploy-anywhere.md)
#### [Beschreiben eines Clusters](service-fabric-cluster-resource-manager-cluster-description.md)
#### [Kapazitätsplanung](service-fabric-cluster-capacity.md)
#### [Visualisieren eines Clusters](service-fabric-visualizing-your-cluster.md)
#### [Herstellen einer Verbindung mit einem sicheren Cluster](service-fabric-connect-to-secure-cluster.md)
#### [Verwalten eines Clusters mithilfe der Azure-CLI](service-fabric-azure-cli.md) 
#### [Schützen eines Clusters](service-fabric-cluster-security.md)
#### [Notfallwiederherstellung](service-fabric-disaster-recovery.md)
#### [Funktionsunterschiede zwischen Linux und Windows](service-fabric-linux-windows-differences.md)

### Cluster in Azure
#### Erstellen eines Clusters in Azure
##### [Azure-Portal](service-fabric-cluster-creation-via-portal.md)
##### [Azure Resource Manager](service-fabric-cluster-creation-via-arm.md)
##### [Visual Studio und Azure Resource Manager](service-fabric-cluster-creation-via-visual-studio.md)
#### [Clusternetzwerkmuster](service-fabric-patterns-networking.md)
#### [Knotentypen und VM-Skalierungsgruppen](service-fabric-cluster-nodetypes.md)
#### [Skalieren eines Clusters](service-fabric-cluster-scale-up-down.md)
#### [Programmgesteuertes Skalieren eines Clusters](service-fabric-cluster-programmatic-scaling.md)
#### [Upgraden eines Clusters](service-fabric-cluster-upgrade.md)
#### [Löschen eines Clusters](service-fabric-cluster-delete.md)
#### [Zugriffssteuerung](service-fabric-cluster-security-roles.md)
#### [Konfigurieren eines Clusters](service-fabric-cluster-fabric-settings.md)
#### [Hinzufügen oder Ausführen eines Rollovers für Clusterzertifikate](service-fabric-cluster-security-update-certs-azure.md) 
#### [Kostenloses Testen eines Partyclusters](http://aka.ms/tryservicefabric)

### Eigenständige Cluster
#### [Planen und Vorbereiten der Bereitstellung](service-fabric-cluster-standalone-deployment-preparation.md)
#### [Inhalt des eigenständigen Pakets für Service Fabric](service-fabric-cluster-standalone-package-contents.md)
#### [Erstellen eines eigenständigen Clusters](service-fabric-cluster-creation-for-windows-server.md)
#### [Erstellen eines eigenständigen Clusters in Azure Virtual Machines](service-fabric-cluster-creation-with-windows-azure-vms.md)
#### [Skalieren eines Clusters](service-fabric-cluster-windows-server-add-remove-nodes.md)
#### [Upgraden eines Clusters](service-fabric-cluster-upgrade-windows-server.md)
#### [Zugriffssteuerung](service-fabric-cluster-security-roles.md)
#### [Konfigurieren eines Clusters](service-fabric-cluster-manifest.md)
#### [Sichern eines Clusters mithilfe von Zertifikaten](service-fabric-windows-cluster-x509-security.md)  
#### [Sichern eines Clusters mit Windows-Sicherheit](service-fabric-windows-cluster-windows-security.md) 

## Verwalten des Anwendungslebenszyklus
### [Übersicht](service-fabric-application-lifecycle.md)
### [Einrichten von Continuous Integration mit VSTS](service-fabric-set-up-continuous-integration.md)
### [Bereitstellen Ihrer Linux-Java-App mithilfe von Jenkins](service-fabric-cicd-your-linux-java-application-with-jenkins.md)
### [Grundlegendes zur ImageStoreConnectionString-Einstellung](service-fabric-image-store-connection-string.md)
### Bereitstellen oder Entfernen von Anwendungen
#### [PowerShell](service-fabric-deploy-remove-applications.md)
#### [Visual Studio](service-fabric-publish-app-remote-cluster.md)
### [Übersicht über Anwendungsupgrades](service-fabric-application-upgrade.md)
### [Konfigurieren des Anwendungsupgrades](service-fabric-visualstudio-configure-upgrade.md)
### [Parameter für Anwendungsupgrades](service-fabric-application-upgrade-parameters.md)
### Upgraden einer Anwendung
#### [PowerShell](service-fabric-application-upgrade-tutorial-powershell.md)
#### [Visual Studio](service-fabric-application-upgrade-tutorial.md)
### [Problembehandlung bei Anwendungsupgrades](service-fabric-application-upgrade-troubleshooting.md)
### [Datenserialisierung bei Anwendungsupgrades](service-fabric-application-upgrade-data-serialization.md)
### [Weiterführende Themen zu Anwendungsupgrades](service-fabric-application-upgrade-advanced.md)

## Untersuchen der Anwendungs- und Clusterintegrität
### [Überwachen der Service Fabric-Integrität](service-fabric-health-introduction.md)
### [Melden und Überprüfen der Dienstintegrität](service-fabric-diagnostics-how-to-report-and-check-service-health.md)
### [Hinzufügen von benutzerdefinierten Berichten zur Integrität](service-fabric-report-health.md)
### [Behandeln von Problemen anhand von Berichten zur Systemintegrität](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)
### [Anzeigen von Integritätsberichten](service-fabric-view-entities-aggregated-health.md)

## Überwachen und Diagnostizieren
### [Überwachen und Diagnostizieren von Anwendungen](service-fabric-diagnostics-overview.md)
### Lokales Überwachen und Diagnostizieren von Diensten
#### [Windows](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)
#### [Linux](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md)
### Azure-Diagnoseprotokolle
#### [Windows](service-fabric-diagnostics-how-to-setup-wad.md)
#### [Linux](service-fabric-diagnostics-how-to-setup-lad.md)
### [Sammeln von Protokollen in einem Dienstprozess](service-fabric-diagnostic-collect-logs-without-an-agent.md)
### [Diagnose für zustandsbehaftete Reliable Services](service-fabric-reliable-services-diagnostics.md)
### [Diagnose in Reliable Actors](service-fabric-reliable-actors-diagnostics.md)
### [Behandeln von Problemen mit dem lokalen Cluster](service-fabric-troubleshoot-local-cluster-setup.md)

## Skalieren von Anwendungen
### [Partitionieren von Reliable Services](service-fabric-concepts-partitioning.md)
### [Verfügbarkeit von Diensten](service-fabric-availability-services.md)
### [Skalieren von Anwendungen](service-fabric-concepts-scalability.md)
### [Planen der Kapazität von Anwendungen](service-fabric-capacity-planning.md)

## Testen von Anwendungen und Diensten
### [Übersicht über die Fehleranalyse](service-fabric-testability-overview.md)
### [Testen der Kommunikation zwischen Diensten](service-fabric-testability-scenarios-service-communication.md)
### Simulieren von Ausfällen
#### [Verwenden des kontrollierten Chaos](service-fabric-controlled-chaos.md)
#### [Verwenden von Testaktionen](service-fabric-testability-actions.md)
#### [Bei Ausführung von Workloads](service-fabric-testability-workload-tests.md)
#### [Verwenden von Testszenarien](service-fabric-testability-scenarios.md)
#### [Verwenden der Knotenübergang-APIs](service-fabric-node-transition-apis.md)
### [Durchführen eines Auslastungstests für Ihre Anwendung](service-fabric-vso-load-test.md)

## Verwalten und Orchestrieren von Clusterressourcen
### [Übersicht über den Clusterressourcen-Manager](service-fabric-cluster-resource-manager-introduction.md)
### [Architektur des Clusterressourcen-Managers](service-fabric-cluster-resource-manager-architecture.md)
### [Beschreiben eines Clusters](service-fabric-cluster-resource-manager-cluster-description.md)
### [Übersicht über Anwendungsgruppen](service-fabric-cluster-resource-manager-application-groups.md)
### [Konfigurieren der Einstellungen des Clusterressourcen-Managers](service-fabric-cluster-resource-manager-configure-services.md)
### [Metriken zum Ressourcenverbrauch](service-fabric-cluster-resource-manager-metrics.md)
### [Nutzen der Dienstaffinität](service-fabric-cluster-resource-manager-advanced-placement-rules-affinity.md)
### [Richtlinien zur Dienstplatzierung](service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies.md)
### [Verwalten eines Clusters](service-fabric-cluster-resource-manager-management-integration.md)
### [Clusterdefragmentierung](service-fabric-cluster-resource-manager-defragmentation-metrics.md)
### [Ausbalancieren eines Clusters](service-fabric-cluster-resource-manager-balancing.md)
### [Drosselung](service-fabric-cluster-resource-manager-advanced-throttling.md)
### [Dienstverschiebung](service-fabric-cluster-resource-manager-movement-cost.md)

# Referenz
## [PowerShell](//powershell/servicefabric/vlatest/servicefabric)
## [Java-API](/java/api/microsoft.servicefabric.services)
## [.NET](/dotnet/api/microsoft.servicefabric.services)
## [REST](/rest/api/servicefabric)

# Ressourcen
## [Allgemeine Fragen zu Service Fabric](service-fabric-common-questions.md)
## [Supportoptionen für Service Fabric](service-fabric-support.md)
## [Beispielcode](http://aka.ms/servicefabricsamples)
## [Lernpfad](https://azure.microsoft.com/documentation/learning-paths/service-fabric/)
## [Preise](https://azure.microsoft.com/pricing/details/service-fabric/)
## [Dienstupdates](https://azure.microsoft.com/updates/?product=service-fabric)
## [MSDN-Forum](https://social.msdn.microsoft.com/Forums/home?forum=AzureServiceFabric)
## [Videos](https://azure.microsoft.com/documentation/videos/index/?services=service-fabric)
