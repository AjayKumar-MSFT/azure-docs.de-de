---
title: "Bereitstellen einer ausführbaren Gastanwendungsdatei in Azure Service Fabric | Microsoft-Dokumentation"
description: "Exemplarische Vorgehensweise beim Packen einer vorhandenen Anwendung als ausführbare Gastanwendungsdatei, um diese in einem Service Fabric-Cluster bereitzustellen"
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: 
ms.assetid: d799c1c6-75eb-4b8a-9f94-bf4f3dadf4c3
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 02/17/2016
ms.author: mfussell;mikhegn
translationtype: Human Translation
ms.sourcegitcommit: 503f5151047870aaf87e9bb7ebf2c7e4afa27b83
ms.openlocfilehash: e9c53dc601406961ee7aeca2e350ba14e691cb9b
ms.lasthandoff: 03/28/2017


---
# <a name="deploy-a-guest-executable-to-service-fabric"></a>Bereitstellen einer ausführbaren Gastanwendungsdatei in Service Fabric
Sie können beliebige Anwendungen, z. B. Node.js-, Java- oder native Anwendungen, in Azure Service Fabric ausführen. Im Zusammenhang mit Service Fabric werden diese Anwendungen als ausführbare Gastanwendungsdateien bezeichnet.
Ausführbare Gastanwendungsdateien werden von Service Fabric wie zustandslose Dienste behandelt. Folglich werden sie basierend auf der Verfügbarkeit und anderen Metriken auf Knoten innerhalb eines Clusters platziert. In diesem Artikel wird beschrieben, wie Sie eine ausführbare Gastanwendungsdatei packen und in einem Service Fabric-Cluster bereitstellen, indem Sie Visual Studio oder ein Befehlszeilenprogramm verwenden.

In diesem Artikel werden die Schritte zum Packen einer ausführbaren Gastanwendungsdatei sowie ihre Bereitstellung in Service Fabric beschrieben.  

## <a name="benefits-of-running-a-guest-executable-in-service-fabric"></a>Vorteile der Ausführung einer ausführbaren Gastanwendungsdatei in Service Fabric
Das Ausführen einer ausführbaren Gastanwendungsdatei in einem Service Fabric-Cluster bietet mehrere Vorteile:

* Hohe Verfügbarkeit. Anwendungen, die in Service Fabric ausgeführt werden, weisen eine hohe Verfügbarkeit auf. In Service Fabric wird sichergestellt, dass die Instanzen einer Anwendung ausgeführt werden.
* Systemüberwachung. Die Service Fabric-Systemüberwachung erkennt, ob eine Anwendung ausgeführt wird, und stellt bei Fehlern Diagnoseinformationen bereit.   
* Application Lifecycle Management. Zusätzlich zur Bereitstellung von Upgrades ohne Ausfallzeiten ermöglicht Service Fabric das automatische Zurücksetzen auf die vorherige Version (Rollback), wenn während eines Upgrades ein Fehler in Bezug auf die Integrität gemeldet wird.    
* Dichte. Sie können mehrere Anwendungen in einem Cluster ausführen, sodass nicht mehr jede Anwendung auf eigener Hardware ausgeführt werden muss.

## <a name="samples"></a>Beispiele
* [Beispiel für das Packen und Bereitstellen einer ausführbaren Gastdatei](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/master/GuestExe/SimpleApplication)
* [Beispiel für zwei ausführbare Gastdateien (C# und Node.js), die über den Naming Service mithilfe von REST kommunizieren](https://github.com/Azure-Samples/service-fabric-dotnet-containers)

## <a name="overview-of-application-and-service-manifest-files"></a>Übersicht über die Anwendungs- und Dienstmanifestdateien
Im Rahmen der Bereitstellung einer ausführbaren Gastanwendungsdatei sollten Sie das Service Fabric-Modell für das Packen und Bereitstellen von Anwendungen kennen. Eine Beschreibung finden Sie unter [Anwendungsmodell](service-fabric-application-model.md). Das Packmodell von Service Fabric basiert auf zwei XML-Dateien: dem Anwendungs- und dem Dienstmanifest. Die Schemadefinition für die Dateien „ApplicationManifest.xml“ und „ServiceManifest.xml“ wird mit dem Service Fabric SDK unter *C:\Programme\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd* installiert.

* **Anwendungsmanifest**: Das Anwendungsmanifest wird verwendet, um die Anwendung zu beschreiben. Es listet neben den Diensten, aus denen die Anwendung besteht, noch weitere Parameter auf, mit denen definiert wird, wie Dienste bereitgestellt werden sollen, z.B. die Anzahl von Instanzen.
  
  Unter Service Fabric ist eine Anwendung eine Bereitstellungs- und Upgradeeinheit. Eine Anwendung kann als eine Einheit aktualisiert werden, wobei potenzielle Fehler und mögliche Zurücksetzungen verwaltet werden. Mit Service Fabric wird sichergestellt, dass der Upgradeprozess entweder erfolgreich ist oder – falls beim Upgrade ein Fehler auftritt – die Anwendung nicht in einem unbekannten oder instabilen Zustand verbleibt.
* **Dienstmanifest**: Das Dienstmanifest beschreibt die Komponenten eines Diensts. Es enthält Daten, z.B. den Namen und Typ des Diensts, und den Code sowie die Konfiguration. Das Dienstmanifest enthält auch einige zusätzliche Parameter, die verwendet werden können, um den Dienst zu konfigurieren, nachdem er bereitgestellt wurde.

## <a name="application-package-file-structure"></a>Dateistruktur des Anwendungspakets
Damit eine Anwendung in Service Fabric bereitgestellt werden kann, sollte die Anwendung einer vordefinierten Verzeichnisstruktur folgen. Unten ist ein Beispiel für diese Struktur angegeben.

```
|-- ApplicationPackageRoot
    |-- GuestService1Pkg
        |-- Code
            |-- existingapp.exe
        |-- Config
            |-- Settings.xml
        |-- Data
        |-- ServiceManifest.xml
    |-- ApplicationManifest.xml
```

ApplicationPackageRoot enthält die Datei „ApplicationManifest.xml“, mit der die Anwendung definiert wird. Für jeden Dienst, der in der Anwendung enthalten ist, gibt es ein Unterverzeichnis, das alle für den Dienst erforderlichen Artefakte enthält. Diese Unterverzeichnisse umfassen „Servicemanifest.xml“ und normalerweise Folgendes:

* *Code*. Dieses Verzeichnis enthält den Code des Diensts.
* *Config*. Dieses Verzeichnis enthält die Datei „Settings.xml“ (sowie andere Dateien, falls erforderlich), auf die der Dienst zur Laufzeit zugreifen kann, um bestimmte Konfigurationseinstellungen abzurufen.
* *Daten*. Dies ist ein zusätzliches Verzeichnis zum Speichern zusätzlicher lokaler Daten, die der Dienst möglicherweise benötigt. Das Verzeichnis „data“ sollte nur verwendet werden, um kurzlebige Daten zu speichern. Service Fabric kopiert oder repliziert keine Änderungen in das Verzeichnis „data“, wenn der Dienst (beispielsweise bei einem Failover) verschoben werden muss.

> [!NOTE]
> Sie müssen die Verzeichnisse `config` und `data` nur erstellen, wenn Sie sie benötigen.
> 
> 

## <a name="package-an-existing-executable"></a>Packen einer vorhandenen ausführbaren Datei
Beim Packen einer ausführbaren Gastanwendungsdatei können Sie wählen, ob Sie eine Visual Studio-Projektvorlage verwenden oder das [Anwendungspaket manuell erstellen](#manually). Mit Visual Studio werden die Anwendungspaketstruktur und Manifestdateien mit der neuen Projektvorlage für Sie erstellt.

> [!TIP]
> Der einfachste Weg zum Packen einer vorhandenen ausführbaren Windows-Datei in einen Dienst ist die Verwendung von Visual Studio.
> 
> 

## <a name="use-visual-studio-to-package-an-existing-executable"></a>Verwenden von Visual Studio zum Packen einer vorhandenen ausführbaren Datei
In Visual Studio wird eine Service Fabric-Dienstvorlage bereitgestellt, um Sie beim Bereitstellen einer ausführbaren Gastanwendung für einen Service Fabric-Cluster zu unterstützen.

1. Wählen Sie **Datei** > **Neues Projekt**, und erstellen Sie eine Service Fabric-Anwendung.
2. Wählen Sie als Dienstvorlage die Option **Guest Executable** (Ausführbare Gastdatei).
3. Klicken Sie auf **Durchsuchen**, um den Ordner mit der ausführbaren Datei auszuwählen, und geben Sie die restlichen Parameter an, um den Dienst zu erstellen.
   * *Codepaketverhalten*: Sie können das Codepaketverhalten so festlegen, dass der gesamte Inhalt Ihres Ordners in das Visual Studio-Projekt kopiert wird. Dies ist hilfreich, wenn sich die ausführbare Datei nicht ändert. Wenn Sie erwarten, dass sich die ausführbare Datei ändert, und neue Builds dynamisch übernehmen möchten, können Sie stattdessen auch einen Link zum Ordner angeben. Beachten Sie, dass Sie verknüpfte Ordner verwenden können, wenn Sie das Anwendungsprojekt in Visual Studio erstellen. Im Projekt wird ein Link zum Quellspeicherort erstellt, damit Sie die ausführbare Gastanwendungsdatei an der Quelle aktualisieren können. Diese Updates werden beim Buildvorgang Teil des Anwendungspakets.
   * *Program*: Gibt die ausführbare Datei an, die zum Starten des Diensts ausgeführt werden soll.
   * *Arguments*: Gibt die Argumente an, die an die ausführbare Datei übergeben werden sollen. Dies kann eine Liste von Parametern mit Argumenten sein.
   * *WorkingFolder*: Gibt das Arbeitsverzeichnis für den Prozess an, der gestartet werden soll. Sie können drei Werte angeben:
     * `CodeBase` gibt an, dass das Arbeitsverzeichnis auf das Verzeichnis „code“ im Anwendungspaket festgelegt wird (das Verzeichnis `Code` in der obigen Dateistruktur).
     * `CodePackage` gibt an, dass das Arbeitsverzeichnis auf das Stammverzeichnis des Anwendungspakets festgelegt wird (`GuestService1Pkg` in der obigen Dateistruktur).
     * `Work` gibt an, dass die Dateien in einem Unterverzeichnis mit dem Namen „work“ angeordnet werden.
4. Geben Sie dem Dienst einen Namen, und klicken Sie auf **OK**.
5. Wenn der Dienst einen Endpunkt für die Kommunikation benötigt, können Sie das Protokoll, den Port und den Typ der Datei „ServiceManifest.xml“ hinzufügen. Beispiel: `<Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000" UriScheme="http" PathSuffix="myapp/" Type="Input" />`.
6. Sie können die Aktion zum Packen und Veröffentlichen jetzt für Ihren lokalen Cluster verwenden, indem Sie die Projektmappe in Visual Studio debuggen. Wenn Sie bereit sind, können Sie die Anwendung in einem Remotecluster veröffentlichen oder die Projektmappe in die Quellcodeverwaltung einchecken.
7. Am Ende dieses Artikels wird beschrieben, wie Sie die ausführbare Gastdatei für den Dienst anzeigen, der im Service Fabric Explorer ausgeführt wird.

<a id="manually"></a>

## <a name="manually-package-and-deploy-an-existing-executable"></a>Manuelles Packen und Bereitstellen einer vorhandenen ausführbaren Datei
Der Vorgang zum manuellen Packen einer ausführbaren Gastanwendungsdatei basiert auf den folgenden allgemeinen Schritten:

1. Erstellen der Verzeichnisstruktur des Pakets.
2. Hinzufügen von Anwendungscode und Konfigurationsdateien.
3. Bearbeiten der Dienstmanifestdatei.
4. Bearbeiten der Anwendungsmanifestdatei.

<!--
>[AZURE.NOTE] We do provide a packaging tool that allows you to create the ApplicationPackage automatically. The tool is currently in preview. You can download it from [here](http://aka.ms/servicefabricpacktool).
-->

### <a name="create-the-package-directory-structure"></a>Erstellen der Verzeichnisstruktur des Pakets
Sie können beginnen, indem Sie die Verzeichnisstruktur wie im vorherigen Abschnitt „Dateistruktur des Anwendungspakets“ beschrieben erstellen.

### <a name="add-the-applications-code-and-configuration-files"></a>Hinzufügen von Anwendungscode und Konfigurationsdateien
Nachdem Sie die Verzeichnisstruktur erstellt haben, können Sie den Anwendungscode und die Konfigurationsdateien den Verzeichnissen „code“ und „config“ hinzufügen. In den Verzeichnissen „code“ und „config“ können Sie auch weitere Verzeichnisse bzw. Unterverzeichnisse erstellen.

Service Fabric führt einen `xcopy`-Vorgang für den Inhalt des Stammverzeichnisses der Anwendung aus. Mit Ausnahme der Erstellung der beiden übergeordneten Verzeichnisse „code“ und „settings“ gibt es also keine vordefinierte Struktur, die verwendet werden muss. (Sie können auch andere Namen verwenden. Weitere Informationen finden Sie im nächsten Abschnitt.)

> [!NOTE]
> Stellen Sie sicher, dass Sie alle Dateien und Abhängigkeiten einbeziehen, die für die Anwendung erforderlich sind. Service Fabric kopiert den Inhalt des Anwendungspakets auf alle Knoten im Cluster, auf denen die Anwendungsdienste bereitgestellt werden sollen. Das Paket muss den gesamten Code enthalten, den die Anwendung zur Ausführung benötigt. Gehen Sie nicht davon aus, dass die Abhängigkeiten bereits installiert sind.
> 
> 

### <a name="edit-the-service-manifest-file"></a>Bearbeiten der Dienstmanifestdatei
Im nächsten Schritt wird die Dienstmanifestdatei so bearbeitet, dass sie folgende Informationen enthält:

* Den Namen des Diensttyps. Dies ist eine ID, die von Service Fabric zum Identifizieren eines Diensts verwendet wird.
* Den Befehl zum Starten der Anwendung (ExeHost).
* Alle Skripts, die ausgeführt werden müssen, um die Anwendung einzurichten (SetupEntryPoint).

Es folgt ein Beispiel für die Datei `ServiceManifest.xml` :

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Name="NodeApp" Version="1.0.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceTypes>
      <StatelessServiceType ServiceTypeName="NodeApp" UseImplicitHost="true"/>
   </ServiceTypes>
   <CodePackage Name="code" Version="1.0.0.0">
      <SetupEntryPoint>
         <ExeHost>
             <Program>scripts\launchConfig.cmd</Program>
         </ExeHost>
      </SetupEntryPoint>
      <EntryPoint>
         <ExeHost>
            <Program>node.exe</Program>
            <Arguments>bin/www</Arguments>
            <WorkingFolder>CodePackage</WorkingFolder>
         </ExeHost>
      </EntryPoint>
   </CodePackage>
   <Resources>
      <Endpoints>
         <Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000" Type="Input" />
      </Endpoints>
   </Resources>
</ServiceManifest>
```

In den folgenden Abschnitten geht es um die anderen Teile der Datei, die Sie aktualisieren müssen.

#### <a name="update-servicetypes"></a>Aktualisieren von ServiceTypes
```xml
<ServiceTypes>
  <StatelessServiceType ServiceTypeName="NodeApp" UseImplicitHost="true" />
</ServiceTypes>
```

* Sie können für `ServiceTypeName`einen beliebigen Namen auswählen. Der Wert wird in der Datei `ApplicationManifest.xml` zum Identifizieren des Diensts verwendet.
* Geben Sie `UseImplicitHost="true"` an. Dieses Attribut informiert Service Fabric, dass der Dienst auf einer eigenständigen Anwendung beruht. Service Fabric muss ihn also lediglich als Prozess starten und seine Integrität überwachen.

#### <a name="update-codepackage"></a>Aktualisieren von CodePackage
Das Element „CodePackage“ gibt den Speicherort (und die Version) des Dienstcodes an.

```xml
<CodePackage Name="Code" Version="1.0.0.0">
```

Das Element `Name` wird verwendet, um den Namen des Verzeichnisses im Anwendungspaket anzugeben, das den Dienstcode enthält. `CodePackage` weist auch das `version`-Attribut auf. Es kann verwendet werden, um die Version des Codes anzugeben. Außerdem kann es dazu genutzt werden, den Dienstcode mithilfe der Infrastruktur für die Anwendungslebenszyklusverwaltung in Service Fabric zu aktualisieren.

#### <a name="optional-update-setupentrypoint"></a>Optional: Aktualisieren von SetupEntryPoint
```xml
<SetupEntryPoint>
   <ExeHost>
       <Program>scripts\launchConfig.cmd</Program>
   </ExeHost>
</SetupEntryPoint>
```
Der Element „SetupEntryPoint“ wird verwendet, um eine ausführbare Datei oder eine Batchdatei anzugeben, die vor dem Starten des Dienstcodes ausgeführt werden soll. Dies ist ein optionaler Schritt, der nicht angegeben werden muss, wenn keine Initialisierung erforderlich ist. Der „SetupEntryPoint“ wird bei jedem Neustart des Diensts ausgeführt.

Es gibt nur ein Element „SetupEntryPoint“. Daher müssen Setupskripts in einer Batchdatei gruppiert werden, wenn für das Setup der Anwendung mehrere Skripts erforderlich sind. Das Element „SetupEntryPoint“ kann jeden beliebigen Dateityp ausführen: ausführbare Dateien, Batchdateien und PowerShell-Cmdlets. Weitere Informationen finden Sie unter [Konfigurieren von SetupEntryPoint](service-fabric-application-runas-security.md).

Im obigen Beispiel führt „SetupEntryPoint“ eine Batchdatei mit dem Namen `LaunchConfig.cmd` aus, die sich im Unterverzeichnis `scripts` des Codeverzeichnisses befindet (vorausgesetzt, das WorkingFolder-Element ist auf „CodeBase“ festgelegt).

#### <a name="update-entrypoint"></a>Aktualisieren von EntryPoint
```xml
<EntryPoint>
  <ExeHost>
    <Program>node.exe</Program>
    <Arguments>bin/www</Arguments>
    <WorkingFolder>CodeBase</WorkingFolder>
  </ExeHost>
</EntryPoint>
```

Mit dem Element `EntryPoint` in der Dienstmanifestdatei wird angegeben, wie der Dienst gestartet werden soll. Das Element `ExeHost` gibt die ausführbare Datei (und die Argumente) an, die zum Starten des Diensts verwendet werden soll.

* `Program` gibt den Namen der ausführbaren Datei an, die zum Starten des Diensts verwendet werden soll.
* `Arguments` gibt die Argumente an, die an die ausführbare Datei übergeben werden sollen. Dies kann eine Liste von Parametern mit Argumenten sein.
* `WorkingFolder` gibt das Arbeitsverzeichnis für den Prozess an, der gestartet werden soll. Sie können drei Werte angeben:
  * `CodeBase` gibt an, dass das Arbeitsverzeichnis auf das Verzeichnis „code“ im Anwendungspaket festgelegt wird (das Verzeichnis `Code` in der obigen Dateistruktur).
  * `CodePackage` gibt an, dass das Arbeitsverzeichnis auf das Stammverzeichnis des Anwendungspakets festgelegt wird (`GuestService1Pkg` in der obigen Dateistruktur).
    * `Work` gibt an, dass die Dateien in einem Unterverzeichnis mit dem Namen „work“ angeordnet werden.

WorkingFolder ist nützlich, um das richtige Arbeitsverzeichnis festzulegen, damit von der Anwendung oder von Initialisierungsskripts relative Pfade verwendet werden können.

#### <a name="update-endpoints-and-register-with-naming-service-for-communication"></a>Aktualisieren der Endpunkte und Registrieren beim Naming Service für die Kommunikation
```xml
<Endpoints>
   <Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000" Type="Input" />
</Endpoints>

```
Im vorherigen Beispiel werden mit dem `Endpoint`-Element die Endpunkte angegeben, über die die Anwendung lauschen kann. In diesem Beispiel lauscht die Node.js-Anwendung über Port 3000 (HTTP).

Außerdem können Sie angeben, dass Service Fabric diesen Endpunkt für den Naming Service veröffentlicht, damit andere Dienste die Endpunktadresse für diesen Dienst ermitteln können. Dies ermöglicht Ihnen die Kommunikation zwischen Diensten, bei denen es sich um ausführbare Gastanwendungsdateien handelt.
Die veröffentlichte Endpunktadresse hat das Format `UriScheme://IPAddressOrFQDN:Port/PathSuffix`. `UriScheme` und `PathSuffix` sind optionale Attribute. `IPAddressOrFQDN` ist die IP-Adresse oder der vollqualifizierte Domänenname des Knotens, auf dem die ausführbare Datei angeordnet wird. Dieser Wert wird für Sie berechnet.

Im folgenden Beispiel sehen Sie nach der Bereitstellung des Diensts im Service Fabric Explorer einen Endpunkt, der in etwa `http://10.1.4.92:3000/myapp/` lautet und für die Dienstinstanz veröffentlicht wird. Falls es sich um einen lokalen Computer handelt, wird `http://localhost:3000/myapp/` angezeigt.

```xml
<Endpoints>
   <Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000"  UriScheme="http" PathSuffix="myapp/" Type="Input" />
</Endpoints>
```
Sie können diese Adressen mit dem [Reverseproxy](service-fabric-reverseproxy.md) verwenden, um zwischen den Diensten zu kommunizieren.

### <a name="edit-the-application-manifest-file"></a>Bearbeiten der Anwendungsmanifestdatei
Nach der Konfiguration der Datei `Servicemanifest.xml` müssen Sie einige Änderungen an der Datei `ApplicationManifest.xml` vornehmen, um sicherzustellen, dass der richtige Diensttyp und der richtige Name verwendet werden.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="NodeAppType" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="NodeApp" ServiceManifestVersion="1.0.0.0" />
   </ServiceManifestImport>
</ApplicationManifest>
```

#### <a name="servicemanifestimport"></a>ServiceManifestImport
Im Element `ServiceManifestImport` können Sie einen oder mehrere Dienste angeben, die in der Anwendung enthalten sein sollen. `ServiceManifestName` verweist auf die Dienste. Dieses Element gibt den Namen des Verzeichnisses an, in dem sich die Datei `ServiceManifest.xml` befindet.

```xml
<ServiceManifestImport>
  <ServiceManifestRef ServiceManifestName="NodeApp" ServiceManifestVersion="1.0.0.0" />
</ServiceManifestImport>
```

## <a name="set-up-logging"></a>Einrichten der Protokollierung
Bei ausführbaren Gastanwendungsdateien ist es nützlich, Konsolenprotokolle anzeigen zu können, um festzustellen, ob die Anwendungs- und Konfigurationsskripts Fehler aufweisen.
In der Datei `ServiceManifest.xml` kann mit dem Element `ConsoleRedirection` eine Konsolenumleitung konfiguriert werden.

> [!WARNING]
> Verwenden Sie die Richtlinie zur Konsolenumleitung nie in einer Anwendung, die in der Produktionsumgebung bereitgestellt wurde, da sich dies auf das Anwendungsfailover auswirken kann. Verwenden Sie diese *nur* für die lokale Entwicklung und das Debuggen.  
> 
> 

```xml
<EntryPoint>
  <ExeHost>
    <Program>node.exe</Program>
    <Arguments>bin/www</Arguments>
    <WorkingFolder>CodeBase</WorkingFolder>
    <ConsoleRedirection FileRetentionCount="5" FileMaxSizeInKb="2048"/>
  </ExeHost>
</EntryPoint>
```

`ConsoleRedirection` kann verwendet werden, um die Konsolenausgabe (sowohl stdout als auch stderr) in ein Arbeitsverzeichnis umzuleiten. So kann per Überprüfung sichergestellt werden, dass während des Setups oder der Ausführung der Anwendung im Service Fabric-Cluster keine Fehler auftreten.

`FileRetentionCount` legt fest, wie viele Dateien im Arbeitsverzeichnis gespeichert werden. Der Wert 5 bedeutet beispielsweise, dass die Protokolldateien für die letzten fünf Ausführungsvorgänge im Arbeitsverzeichnis gespeichert werden.

`FileMaxSizeInKb` gibt die maximale Größe der Protokolldateien an.

Protokolldateien werden in einem der Arbeitsverzeichnisse des Diensts gespeichert. Verwenden Sie den Service Fabric Explorer, um zu bestimmen, wo sich die Dateien befinden. Damit können Sie ermitteln, auf welchem Knoten der Dienst ausgeführt wird und welches Arbeitsverzeichnis verwendet wird. Dieser Vorgang wird weiter unten in diesem Artikel erläutert.

## <a name="deployment"></a>Bereitstellung
Der letzte Schritt ist das [Bereitstellen der Anwendung](service-fabric-deploy-remove-applications.md). Das folgende PowerShell-Skript veranschaulicht, wie Sie die Anwendung im lokalen Entwicklungscluster bereitstellen und einen neuen Service Fabric-Dienst starten.

```PowerShell

Connect-ServiceFabricCluster localhost:19000

Write-Host 'Copying application package...'
Copy-ServiceFabricApplicationPackage -ApplicationPackagePath 'C:\Dev\MultipleApplications' -ImageStoreConnectionString 'file:C:\SfDevCluster\Data\ImageStoreShare' -ApplicationPackagePathInImageStore 'nodeapp'

Write-Host 'Registering application type...'
Register-ServiceFabricApplicationType -ApplicationPathInImageStore 'nodeapp'

New-ServiceFabricApplication -ApplicationName 'fabric:/nodeapp' -ApplicationTypeName 'NodeAppType' -ApplicationTypeVersion 1.0

New-ServiceFabricService -ApplicationName 'fabric:/nodeapp' -ServiceName 'fabric:/nodeapp/nodeappservice' -ServiceTypeName 'NodeApp' -Stateless -PartitionSchemeSingleton -InstanceCount 1

```

>[!TIP]
> [Komprimieren Sie das Paket](service-fabric-package-apps.md#compress-a-package) vor dem Kopieren in den Imagespeicher, wenn das Paket sehr groß ist oder viele Dateien enthält. Weitere Informationen finden Sie [hier](service-fabric-deploy-remove-applications.md#upload-the-application-package).
>

Ein Service Fabric-Dienst kann mit verschiedenen „Konfigurationen“ bereitgestellt werden. Als eine Instanz, als mehrere Instanzen oder als eine Instanz des Diensts auf jedem Knoten des Service Fabric-Clusters, um nur einige Beispiele zu nennen.

Mit dem `InstanceCount`-Parameter des Cmdlets `New-ServiceFabricService` wird angegeben, wie viele Instanzen des Diensts im Service Fabric-Cluster gestartet werden sollen. Sie können den Wert `InstanceCount` abhängig vom Typ der bereitzustellenden Anwendung festlegen. Die beiden häufigsten Szenarien:

* `InstanceCount = "1"`. In diesem Fall wird nur eine Instanz des Diensts im Cluster bereitgestellt. Der Service Fabric-Scheduler bestimmt den Knoten, auf dem der Dienst bereitgestellt werden soll.
* `InstanceCount ="-1"`. In diesem Fall wird eine Instanz des Diensts auf jedem Knoten im Service Fabric-Cluster bereitgestellt. Das Ergebnis ist eine (und nur eine) Instanz des Diensts für jeden Knoten im Cluster.

Dies ist eine praktische Konfiguration für Front-End-Anwendungen (z.B. REST-Endpunkte), da Clientanwendungen eine Verbindung mit einem Knoten im Cluster herstellen müssen, um den Endpunkt zu verwenden. Diese Konfiguration kann auch verwendet werden, wenn beispielsweise alle Knoten des Service Fabric-Clusters mit einem Lastenausgleichsmodul verbunden sind. Der Clientdatenverkehr kann dann über den Dienst verteilt werden, der auf allen Knoten im Cluster ausgeführt wird.

## <a name="check-your-running-application"></a>Überprüfen der ausgeführten Anwendung
Bestimmen Sie im Service Fabric Explorer den Knoten, auf dem der Dienst ausgeführt wird. In diesem Beispiel ist es „Node1“:

![Knoten, auf dem der Dienst ausgeführt wird](./media/service-fabric-deploy-existing-app/nodeappinsfx.png)

Wenn Sie zum Knoten navigieren und zur Anwendung wechseln, sehen Sie die wesentlichen Knoteninformationen, einschließlich des Speicherorts auf dem Datenträger.

![Speicherort auf dem Datenträger](./media/service-fabric-deploy-existing-app/locationondisk2.png)

Wenn Sie in Server-Explorer zum jeweiligen Verzeichnis wechseln, sehen Sie das Arbeitsverzeichnis und den Protokollordner des Diensts, wie im folgenden Screenshot dargestellt.

![Speicherort des Protokolls](./media/service-fabric-deploy-existing-app/loglocation.png)

## <a name="creating-a-guest-executable-using-yeoman-for-service-fabric-on-linux"></a>Erstellen einer ausführbaren Gastanwendungsdatei mithilfe von Yeoman für Service Fabric unter Linux

Zum Erstellen und Bereitstellen einer ausführbaren Gastanwendungsdatei unter Linux wird das gleiche Verfahren verwendet wie beim Bereitstellen einer CSharp- oder Java-Anwendung. 

1. Geben Sie in einem Terminal `yo azuresfguest` ein.
2. Benennen Sie Ihre Anwendung.
3. Wählen Sie die Art Ihres ersten Diensts aus, und benennen Sie ihn. Wählen Sie **Guest Binary** (Binäre Gastdatei) für eine ausführbare Gastanwendungsdatei (und **Gastcontainer** für einen Container) aus, und geben Sie Details wie etwa den Pfad der ausführbaren Datei und die Parameter an, mit denen sie aufgerufen werden muss.

Yeoman hätte ein Anwendungspaket mit der entsprechenden Anwendung und den Manifestdateien sowie ein Installations- und ein Deinstallationsskript erstellt.

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel wurden das Packen einer ausführbaren Gastanwendungsdatei sowie ihre Bereitstellung in Service Fabric beschrieben. Entsprechende Informationen und Aufgaben finden Sie in den folgenden Artikeln.

* [Beispiel für das Packen und Bereitstellen einer ausführbaren Gastdatei](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/master/GuestExe/SimpleApplication), einschließlich eines Links zur Vorabversion des Packtools
* [Beispiel für zwei ausführbare Gastdateien (C# und Node.js), die über den Naming Service mithilfe von REST kommunizieren](https://github.com/Azure-Samples/service-fabric-dotnet-containers)
* [Bereitstellen mehrerer ausführbarer Gastanwendungsdateien](service-fabric-deploy-multiple-apps.md)
* [Erstellen Ihrer ersten Service Fabric-Anwendung in Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md)


