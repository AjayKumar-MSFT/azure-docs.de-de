---
title: Einrichten Ihrer Entwicklungsumgebung unter Linux | Microsoft Docs
description: "Installieren Sie die Laufzeit und das SDK, und erstellen Sie einen lokalen Entwicklungscluster unter Linux. Nach Abschluss des Setups können Sie mit der Erstellung von Clientanwendungen beginnen."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: d552c8cd-67d1-45e8-91dc-871853f44fc6
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/23/2017
ms.author: subramar
translationtype: Human Translation
ms.sourcegitcommit: 503f5151047870aaf87e9bb7ebf2c7e4afa27b83
ms.openlocfilehash: 516b8e517a16dd0d87e02189260166696225fbab
ms.lasthandoff: 03/28/2017


---
# <a name="prepare-your-development-environment-on-linux"></a>Vorbereiten Ihrer Entwicklungsumgebung unter Linux
> [!div class="op_single_selector"]
> * [Windows](service-fabric-get-started.md)
> * [Linux](service-fabric-get-started-linux.md)
> * [OSX](service-fabric-get-started-mac.md)
>
>  

 Zur Bereitstellung und Ausführung von [Azure Service Fabric-Anwendungen](service-fabric-application-model.md) auf Ihrem Linux-Entwicklungscomputer müssen Sie die Laufzeit und das allgemeine SDK installieren. Darüber hinaus können Sie auch optionale SDKs für Java und .NET Core installieren.

## <a name="prerequisites"></a>Voraussetzungen

### <a name="supported-operating-system-versions"></a>Unterstützte Betriebssystemversionen
Die folgenden Betriebssystemversionen werden bei der Entwicklung unterstützt:

* Ubuntu 16.04 (**„Xenial Xerus“**)

## <a name="update-your-apt-sources"></a>Aktualisieren Ihrer apt-Quellen
Um das SDK und das dazugehörige Laufzeitpaket über „apt-get“ installieren zu können, müssen Sie zunächst Ihre apt-Datenquellen aktualisieren.

1. Öffnen Sie ein Terminal.
2. Fügen Sie der Quellenliste das Service Fabric-Repository hinzu.

    ```bash
    sudo sh -c 'echo "deb [arch=amd64] http://apt-mo.trafficmanager.net/repos/servicefabric/ trusty main" > /etc/apt/sources.list.d/servicefabric.list'
    ```
3. Fügen Sie der Quellenliste das **DotNet**-Repository hinzu.

    ```bash
    sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/dotnet-release/ xenial main" > /etc/apt/sources.list.d/dotnetdev.list'
    ```
4. Fügen Sie Ihrem apt-Schlüsselbund den neuen GPG-Schlüssel hinzu.

    ```bash
    sudo apt-key adv --keyserver apt-mo.trafficmanager.net --recv-keys 417A0893
    ```
    ```bash
    sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 417A0893
    ```

5. Aktualisieren Sie Ihre Paketlisten auf der Grundlage der neu hinzugefügten Repositorys.

    ```bash
    sudo apt-get update
    ```
## <a name="install-and-set-up-the-sdk-for-containers-and-guest-executables"></a>Installieren und Einrichten des SDK für Container und ausführbare Gastanwendungsdateien
Nach der Aktualisierung Ihrer Datenquellen können Sie das SDK installieren.

1. Installieren Sie das Service Fabric-SDK-Paket. Sie werden aufgefordert, die Installation zu bestätigen und einem Lizenzvertrag zuzustimmen.

    ```bash
    sudo apt-get install servicefabricsdkcommon
    ```
2. Führen Sie das SDK-Setupskript aus.

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/common/sdkcommonsetup.sh
    ```

Nach dem Ausführen der Schritte zum Installieren des allgemeinen SDK-Pakets sollte die Erstellung von Apps mit ausführbaren Gastdateien oder Containerdiensten möglich sein, indem `yo azuresfguest` ausgeführt wird. Unter Umständen müssen Sie die Umgebungsvariable **$NODE_PATH** auf den Speicherort der Knotenmodule festlegen. 

    ```bash
    export NODE_PATH=$NODE_PATH:$HOME/.node/lib/node_modules 
    ```

Bei Verwendung der Umgebung als Stammumgebung (Root) müssen Sie die Variable ggf. mit dem folgenden Befehl festlegen:

    ```bash
    export NODE_PATH=$NODE_PATH:/root/.node/lib/node_modules 
    ```

> [!TIP]
> Es kann hilfreich sein, diese Befehle der Datei „~/.bashrc“ hinzuzufügen, damit Sie die Umgebungsvariable nicht bei jeder Anmeldung festlegen müssen.
>

## <a name="set-up-the-azure-cross-platform-cli"></a>Einrichten der plattformübergreifenden Azure-Befehlszeilenschnittstelle
Die [plattformübergreifende Azure-Befehlszeilenschnittstelle][azure-xplat-cli-github] enthält Befehle für die Interaktion mit Service Fabric-Entitäten (wie etwa Cluster und Anwendungen). Sie basiert auf Node.js. [Vergewissern Sie sich daher, dass Sie Node installiert haben][install-node], bevor Sie mit den folgenden Anweisungen fortfahren:

1. Klonen Sie das GitHub-Repository auf Ihren Entwicklungscomputer.

    ```bash
    git clone https://github.com/Azure/azure-xplat-cli.git
    ```
2. Wechseln Sie in das geklonte Repository, und installieren Sie die Abhängigkeiten der Befehlszeilenschnittstelle mithilfe des Knotenpaket-Managers (Node Package Manager, npm).

    ```bash
    cd azure-xplat-cli
    npm install
    ```
3. Erstellen Sie einen Symlink zwischen dem Ordner „bin/azure“ des geklonten Repositorys und „/usr/bin/azure“, damit er Ihrem Pfad hinzugefügt wird und Befehle in jedem Verzeichnis zur Verfügung stehen.

    ```bash
    sudo ln -s $(pwd)/bin/azure /usr/bin/azure
    ```
4. Aktivieren Sie schließlich die automatische Vervollständigung von Service Fabric-Befehlen.

    ```bash
    azure --completion >> ~/azure.completion.sh
    echo 'source ~/azure.completion.sh' >> ~/.bash_profile
    source ~/azure.completion.sh
    ```

> [!NOTE]
> Service Fabric-Befehle sind in Azure CLI 2.0 noch nicht verfügbar.


## <a name="set-up-a-local-cluster"></a>Einrichten eines lokalen Clusters
Wenn alles erfolgreich installiert wurde, können Sie einen lokalen Cluster starten.

1. Führen Sie das Clustersetupskript aus.

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/common/clustersetup/devclustersetup.sh
    ```
2. Öffnen Sie einen Webbrowser, und navigieren Sie zu http://localhost:19080/Explorer. Der gestartete Cluster wird auf dem Dashboard von Service Fabric Explorer angezeigt.

    ![Service Fabric Explorer unter Linux][sfx-linux]

Nun können Sie vorgefertigte Service Fabric-Anwendungspakete oder neue, auf Gastcontainern oder ausführbaren Gastdateien basierende Anwendungspakete bereitstellen. Wenn Sie neue Dienste mit den Java- oder .NET Core-SDKs erstellen möchten, führen Sie die folgenden optionalen Einrichtungsschritte in den nachfolgenden Abschnitten aus.


> [!NOTE]
> Eigenständige Cluster werden unter Linux nicht unterstützt. In der Vorschau werden lediglich One-Box-Cluster und Azure Linux-Cluster mit mehreren Computern unterstützt.
>

## <a name="install-the-java-sdk-optional-if-you-wish-to-use-the-java-programming-models"></a>Installieren des Java SDK (optional, falls Sie Java-Programmiermodelle nutzen möchten)
Das Java SDK stellt die Bibliotheken und Vorlagen bereit, die zum Erstellen von Service Fabric-Diensten mit Java benötigt werden.

1. Installieren Sie das Java SDK-Paket.

    ```bash
    sudo apt-get install servicefabricsdkjava
    ```
2. Führen Sie das SDK-Setupskript aus.

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/java/sdkjavasetup.sh
    ```
## <a name="install-the-eclipse-neon-plugin-optional"></a>Installieren des Eclipse Neon-Plug-Ins (optional)

Das Eclipse-Plug-In für Service Fabric können Sie über die **Eclipse-IDE für Java-Entwickler** installieren. Sie können Eclipse verwenden, um zusätzlich zu Service Fabric-Java-Anwendungen Anwendungen mit ausführbarer Gastanwendungsdatei und Containeranwendungen für Service Fabric zu erstellen.

> [!NOTE]
> Die Installation des Java SDK ist eine Voraussetzung für die Verwendung des Eclipse-Plug-Ins. Dies gilt auch, wenn Sie es nur zum Erstellen und Bereitstellen von ausführbaren Gastanwendungsdateien und Containeranwendungen verwenden.
>

1. Stellen Sie in Eclipse sicher, dass Sie die aktuellen Versionen von Eclipse **Neon** und Buildship (1.0.17 oder höher) installiert haben. Die Version der installierten Komponenten können Sie unter **Hilfe > Installationsdetails** ermitteln. Eine Aktualisierungsanleitung für Buildship finden Sie [hier][buildship-update].
2. Wählen Sie zum Installieren des Service Fabric-Plug-Ins **Hilfe > Neue Software installieren...** aus.
3. Geben Sie in das Textfeld „Arbeiten mit“ ein: http://dl.windowsazure.com/eclipse/servicefabric
4. Klicken Sie auf "Hinzufügen".
    ![Eclipse-Plug-In][sf-eclipse-plugin]
5. Wählen Sie das Service Fabric-Plug-In aus, und klicken Sie auf „Weiter“.
6. Fahren Sie mit der Installation fort, und akzeptieren Sie den Endbenutzer-Lizenzvertrag.

Falls Sie das Service Fabric-Eclipse-Plug-In bereits installiert haben, sollten Sie sicherstellen, dass es sich um die aktuelle Version handelt. Sie können dies überprüfen, indem Sie ``Help => Installation Details`` auswählen und in der Liste mit den installierten Plug-Ins nach Service Fabric suchen. Wählen Sie die Option zum Aktualisieren, falls eine neuere Version verfügbar ist. 

Weitere Informationen finden Sie unter [Erste Schritte mit Eclipse für Service Fabric](service-fabric-get-started-eclipse.md).


## <a name="install-the-net-core-sdk-optional-if-you-wish-to-use-the-net-core-programming-models"></a>Installieren des .NET Core SDK (optional, falls Sie die .NET Core-Programmiermodelle verwenden möchten)
Das .NET Core SDK stellt die Bibliotheken und Vorlagen bereit, die zum Erstellen von Service Fabric-Diensten mit plattformübergreifendem .NET Core benötigt werden.

1. Installieren Sie das .NET Core SDK-Paket.

   ```bash
   sudo apt-get install servicefabricsdkcsharp
   ```

2. Führen Sie das SDK-Setupskript aus.

   ```bash
   sudo /opt/microsoft/sdk/servicefabric/csharp/sdkcsharpsetup.sh
   ```

## <a name="updating-the-sdk-and-runtime"></a>Aktualisieren des SDK und der Laufzeit

Führen Sie zum Aktualisieren auf die aktuelle SDK- und Laufzeitversion die folgenden Schritte aus (entfernen Sie aus der Liste die SDKs, die Sie nicht aktualisieren oder installieren möchten.):

   ```bash
   sudo apt-get update
   sudo apt-get install servicefabric servicefabricsdkcommon servicefabricsdkcsharp servicefabricsdkjava
   ```

Navigieren Sie zum Aktualisieren der CLI zum Verzeichnis, in dem Sie die CLI geklont haben, und führen Sie zum Aktualisieren `git pull` aus.  Falls für die Aktualisierung weitere Schritte erforderlich sind, sind sie in den Versionsinformationen angegeben. 


## <a name="next-steps"></a>Nächste Schritte
* [Erstellen und Bereitstellen Ihrer ersten Service Fabric-Java-Anwendung unter Linux mithilfe von Yeoman](service-fabric-create-your-first-linux-application-with-java.md)
* [Erstellen und Bereitstellen Ihrer ersten Service Fabric-Java-Anwendung unter Linux mithilfe des Service Fabric-Plug-Ins für Eclipse](service-fabric-get-started-eclipse.md)
* [Erstellen der ersten Java-Anwendung unter Linux](service-fabric-create-your-first-linux-application-with-csharp.md)
* [Prepare your development environment on OSX (Vorbereiten Ihrer Entwicklungsumgebung unter OSX)](service-fabric-get-started-mac.md)
* [Verwalten von Service Fabric-Anwendungen mit der Azure-CLI](service-fabric-azure-cli.md)
* [Unterschiede zwischen Service Fabric unter Linux (Vorschau) und Windows (allgemein verfügbar)](service-fabric-linux-windows-differences.md)

<!-- Links -->

[azure-xplat-cli-github]: https://github.com/Azure/azure-xplat-cli
[install-node]: https://nodejs.org/en/download/package-manager/#installing-node-js-via-package-manager
[buildship-update]: https://projects.eclipse.org/projects/tools.buildship

<!--Images -->

[sf-eclipse-plugin]: ./media/service-fabric-get-started-linux/service-fabric-eclipse-plugin.png
[sfx-linux]: ./media/service-fabric-get-started-linux/sfx-linux.png

