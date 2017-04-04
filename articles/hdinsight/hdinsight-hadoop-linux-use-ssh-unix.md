---
title: Verwenden von SSH mit HDInsight (Hadoop) unter Windows, Linux, Unix oder OS X | Microsoft-Dokumentation
description: " Sie können auf HDInsight über Secure Shell (SSH) zugreifen. Dieses Dokument enthält Informationen zur Verwendung von SSH zum Herstellen einer Verbindung mit HDInsight auf Windows-, Linux-, Unix- oder OS X-Clients."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: a6a16405-a4a7-4151-9bbf-ab26972216c5
ms.service: hdinsight
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 03/16/2017
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive
translationtype: Human Translation
ms.sourcegitcommit: 4f2230ea0cc5b3e258a1a26a39e99433b04ffe18
ms.openlocfilehash: 89d44476e9de8ac32195efaf66535cdd9fb4260e
ms.lasthandoff: 03/25/2017

---
# <a name="connect-to-hdinsight-hadoop-using-ssh"></a>Herstellen einer Verbindung mit HDInsight (Hadoop) per SSH

Informieren Sie sich darüber, wie Sie [Secure Shell (SSH)](https://en.wikipedia.org/wiki/Secure_Shell) verwenden, um eine sichere Verbindung mit HDInsight herzustellen. HDInsight kann Linux (Ubuntu) als Betriebssystem für Knoten im Cluster verwenden. SSH kann genutzt werden, um eine Verbindung mit den Haupt- und Edgeknoten eines Linux-basierten Clusters herzustellen und Befehle direkt auf diesen Knoten auszuführen.

Die folgende Tabelle enthält die Adress- und Portinformationen, die zum Herstellen der Verbindung mit HDInsight per SSH benötigt werden:

| Adresse | Port | Verbindungsherstellung mit... |
| ----- | ----- | ----- |
| `<edgenodename>.<clustername>-ssh.azurehdinsight.net` | 22 | Edgeknoten (falls vorhanden) |
| `<clustername>-ssh.azurehdinsight.net` | 22 | Primärer Hauptknoten |
| `<clustername>-ssh.azurehdinsight.net` | 23 | Sekundärer Hauptknoten |

> [!NOTE]
> Ersetzen Sie `<edgenodename>` durch den Namen des Edgeknotens. Weitere Informationen zur Verwendung von Edgeknoten finden Sie unter [Verwenden leerer Edgeknoten in HDInsight](hdinsight-apps-use-edge-node.md#access-an-edge-node).
>
> Ersetzen Sie `<clustername>` durch den Namen Ihres HDInsight-Clusters.
>
> Wir empfehlen Ihnen, __immer eine Verbindung mit dem Edgeknoten herzustellen__, falls ein Knoten dieser Art vorhanden ist. Auf den Hauptknoten werden Dienste gehostet, die für die Integrität des Clusters wichtig sind. Auf dem Edgeknoten werden nur die Komponenten ausgeführt, die Sie darauf anordnen.

## <a name="ssh-clients"></a>SSH-Clients

Die meisten Betriebssysteme verfügen über den `ssh`-Client. In Microsoft Windows wird standardmäßig kein SSH-Client bereitgestellt. Ein SSH-Client für Windows ist in den folgenden Paketen verfügbar:

* [Bash unter Ubuntu unter Windows 10](https://msdn.microsoft.com/commandline/wsl/about): Der Befehl `ssh` wird per Bash über die Windows-Befehlszeile bereitgestellt.

* [Git (https://git-scm.com/)](https://git-scm.com/): Der Befehl `ssh` wird über die GitBash-Befehlszeile bereitgestellt.

* [GitHub Desktop (https://desktop.github.com/)](https://desktop.github.com/): Der Befehl `ssh` wird über die Git-Shell-Befehlszeile bereitgestellt. GitHub Desktop kann für die Verwendung von Bash, die Windows-Eingabeaufforderung oder PowerShell als Befehlszeile für die Git-Shell konfiguriert werden.

* [OpenSSH (https://github.com/PowerShell/Win32-OpenSSH/wiki/Install-Win32-OpenSSH)](https://github.com/PowerShell/Win32-OpenSSH/wiki/Install-Win32-OpenSSH): Das PowerShell-Team portiert OpenSSH zu Windows und stellt Testversionen bereit.

    > [!WARNING]
    > Das OpenSSH-Paket enthält die SSH-Serverkomponente `sshd`. Mit dieser Komponente wird ein SSH-Server auf Ihrem System gestartet, sodass andere Benutzer eine Verbindung damit herstellen können. Sie sollten diese Komponente nur konfigurieren und Port 22 nur öffnen, wenn Sie auf Ihrem System einen SSH-Server hosten möchten. Für die Kommunikation mit HDInsight ist dies nicht erforderlich.

Es gibt auch mehrere grafische SSH-Clients, z.B. [PuTTY (http://www.chiark.greenend.org.uk/~sgtatham/putty/)](http://www.chiark.greenend.org.uk/~sgtatham/putty/) und [MobaXterm (http://mobaxterm.mobatek.net/)](http://mobaxterm.mobatek.net/). Diese Clients können zum Herstellen einer Verbindung mit HDInsight verwendet werden, aber der Prozess der Verbindungsherstellung mit einem Server ist anders als bei der Nutzung des Hilfsprogramms `ssh`. Weitere Informationen finden Sie in der Dokumentation des grafischen Clients, den Sie verwenden.

## <a id="sshkey"></a>Authentifizierung: SSH-Schlüssel

Für SSH-Schlüssel wird [Public-key cryptography](https://en.wikipedia.org/wiki/Public-key_cryptography) (Verschlüsselung mit öffentlichem Schlüssel) zum Schützen des Clusters verwendet. SSH-Schlüssel sind sicherer als Kennwörter und stellen einen einfachen Weg zum Schützen des HDInsight-Clusters dar.

Wenn Ihr SSH-Konto mit einem Schlüssel geschützt ist, muss der Client beim Herstellen der Verbindung den passenden privaten Schlüssel bereitstellen:

* Die meisten Clients können für die Verwendung eines __Standardschlüssels__ konfiguriert werden. Beispielsweise sucht der `ssh`-Client in Linux- und Unix-Umgebungen unter `~/.ssh/id_rsa` nach einem privaten Schlüssel.

* Sie können den __Pfad zu einem privaten Schlüssel__ angeben. Beim `ssh`-Client wird der Parameter `-i` verwendet, um den Pfad zum privaten Schlüssel anzugeben. Beispiel: `ssh -i ~/.ssh/hdinsight sshuser@myedge.mycluster-ssh.azurehdinsight.net`.

* Wenn Sie __mehrere private Schlüssel__ zur Verwendung mit verschiedenen Servern nutzen, können Hilfsprogramme wie [ssh-agent (https://en.wikipedia.org/wiki/Ssh-agent)](https://en.wikipedia.org/wiki/Ssh-agent) zum automatischen Auswählen des richtigen Schlüssels eingesetzt werden.

> [!IMPORTANT]
>
> Falls Sie Ihren privaten Schlüssel mit einer Passphrase schützen, müssen Sie die Passphrase beim Verwenden des Schlüssels eingeben. Mit Hilfsprogrammen wie `ssh-agent` kann das Kennwort zwischengespeichert werden, um Ihnen den Vorgang zu erleichtern.

### <a name="create-an-ssh-key-pair"></a>Erstellen eines SSH-Schlüsselpaars

Verwenden Sie den Befehl `ssh-keygen`, um Dateien mit öffentlichen und privaten Schlüsseln zu erstellen. Mit dem folgenden Befehl wird ein RSA-Schlüsselpaar (2048 Bit) generiert, das mit HDInsight verwendet werden kann:

    ssh-keygen -t rsa -b 2048

Beim Erstellen des Schlüssels werden Sie zum Eingeben von Informationen aufgefordert. Dies können beispielsweise Informationen dazu sein, wo die Schlüssel gespeichert sind oder ob eine Passphrase verwendet werden soll. Nach Abschluss des Vorgangs werden zwei Dateien erstellt: ein öffentlicher Schlüssel und ein privater Schlüssel.

* Der __öffentliche Schlüssel__ wird zum Erstellen eines HDInsight-Clusters verwendet. Der öffentliche Schlüssel hat die Erweiterung `.pub`.

* Der __private Schlüssel__ wird verwendet, um Ihren Client im HDInsight-Cluster zu authentifizieren.

> [!IMPORTANT]
> Sie können Ihre Schlüssel mit einer Passphrase schützen. Dies ist praktisch ein Kennwort für Ihren privaten Schlüssel. Wenn Ihr privater Schlüssel in den Besitz einer anderen Person gelangt, muss diese Person für die Nutzung des Schlüssels dann auch über die Passphrase verfügen.

### <a name="create-hdinsight-using-the-public-key"></a>Erstellen von HDInsight-Clustern mit dem öffentlichen Schlüssel

| Erstellungsmethode | Verwenden des öffentlichen Schlüssels |
| ------- | ------- |
| **Azure-Portal** | Deaktivieren Sie das Kontrollkästchen __Dasselbe Kennwort wie für die Clusteranmeldung verwenden__, und wählen Sie dann __Öffentlicher Schlüssel__ als SSH-Authentifizierungstyp. Wählen Sie abschließend die Datei mit dem öffentlichen Schlüssel aus, oder fügen Sie den Textinhalt der Datei im Feld __Öffentlicher SSH-Schlüssel__ ein.</br>![Dialogfeld „Öffentlicher SSH-Schlüssel“ bei der Erstellung des HDInsight-Clusters](./media/hdinsight-hadoop-linux-use-ssh-unix/create-hdinsight-ssh-public-key.png) |
| **Azure PowerShell** | Verwenden Sie den Parameter `-SshPublicKey` des `New-AzureRmHdinsightCluster`-Cmdlets, und übergeben Sie den Inhalt des öffentlichen Schlüssels als Zeichenfolge.|
| **Azure CLI 1.0** | Verwenden Sie den Parameter `--sshPublicKey` des Befehls `azure hdinsight cluster create`, und übergeben Sie den Inhalt des öffentlichen Schlüssels als Zeichenfolge. |
| **Resource Manager-Vorlage** | Ein Beispiel für die Verwendung von SSH-Schlüsseln mit einer Vorlage finden Sie unter [Deploy HDInsight on Linux (w/ Azure Storage, SSH key)](https://azure.microsoft.com/resources/templates/101-hdinsight-linux-ssh-publickey/) (Bereitstellen von HDInsight unter Linux (mit Azure Storage, SSH-Schlüssel)). Das `publicKeys`-Element in der Datei [azuredeploy.json](https://github.com/Azure/azure-quickstart-templates/blob/master/101-hdinsight-linux-ssh-publickey/azuredeploy.json) wird zum Übergeben der Schlüssel an Azure während der Erstellung des Clusters verwendet. |

## <a id="sshpassword"></a>Authentifizierung: Kennwort

SSH-Konten können mit einem Kennwort geschützt werden. Beim Herstellen der Verbindung mit HDInsight per SSH werden Sie zum Eingeben des Kennworts aufgefordert.

> [!WARNING]
> Es ist nicht ratsam, für SSH die Kennwortauthentifizierung zu verwenden. Kennwörter können erraten werden und sind anfällig für Brute-Force-Angriffe. Stattdessen empfehlen wir die Verwendung von [SSH-Schlüsseln für die Authentifizierung](#sshkey).

### <a name="create-hdinsight-using-a-password"></a>Erstellen eines HDInsight-Clusters mit einem Kennwort

| Erstellungsmethode | Angeben des Kennworts |
| --------------- | ---------------- |
| **Azure-Portal** | Standardmäßig gilt für das SSH-Benutzerkonto dasselbe Kennwort wie für das Anmeldekonto für den Cluster. Sie können ein anderes Kennwort verwenden, indem Sie die Option __Dasselbe Kennwort wie für die Clusteranmeldung verwenden__ deaktivieren und das Kennwort im Feld __SSH-Kennwort__ eingeben.</br>![Dialogfeld „SSH-Kennwort“ bei der Erstellung des HDInsight-Clusters](./media/hdinsight-hadoop-linux-use-ssh-unix/create-hdinsight-ssh-password.png)|
| **Azure PowerShell** | Verwenden Sie den Parameter `--SshCredential` des `New-AzureRmHdinsightCluster`-Cmdlets, und übergeben Sie ein `PSCredential`-Objekt, in dem der Name und das Kennwort des SSH-Benutzerkontos enthalten sind. |
| **Azure CLI 1.0** | Verwenden Sie den Parameter `--sshPassword` des Befehls `azure hdinsight cluster create`, und geben Sie den Kennwortwert an. |
| **Resource Manager-Vorlage** | Ein Beispiel für die Verwendung eines Kennworts mit einer Vorlage finden Sie unter [Deploy HDInsight cluster with Storage and SSH password](https://azure.microsoft.com/resources/templates/101-hdinsight-linux-ssh-password/) (Bereitstellen eines HDInsight-Clusters mit Storage und SSH-Kennwort). Das `linuxOperatingSystemProfile`-Element in der Datei [azuredeploy.json](https://github.com/Azure/azure-quickstart-templates/blob/master/101-hdinsight-linux-ssh-password/azuredeploy.json) wird zum Übergeben des Namens und Kennworts für das SSH-Konto an Azure während der Erstellung des Clusters verwendet.|

### <a name="change-the-ssh-password"></a>Ändern des SSH-Kennworts

Informationen zum Ändern des Kennworts für das SSH-Benutzerkonto finden Sie im Abschnitt __Ändern von Kennwörtern__ des Dokuments [Verwalten von Hadoop-Clustern in HDInsight mit dem Azure-Portal](hdinsight-administer-use-portal-linux.md#change-passwords).

## <a id="domainjoined"></a>Authentifizierung: In die Domäne eingebundener HDInsight-Cluster

Wenn Sie einen __in die Domäne eingebundenen HDInsight-Cluster__ verwenden, müssen Sie nach dem Herstellen der Verbindung per SSH den Befehl `kinit` verwenden. Bei diesem Befehl werden Sie zum Eingeben eines Benutzers und Kennworts für die Domäne aufgefordert, und Ihre Sitzung wird anhand der Azure Active Directory-Domäne authentifiziert, die dem Cluster zugeordnet ist.

Weitere Informationen finden Sie unter [Configure Domain-joined HDInsight clusters (Preview)](hdinsight-domain-joined-configure.md) (Konfigurieren von in die Domäne eingebundenen HDInsight-Clustern (Vorschau)).

## <a name="connect-to-worker-and-zookeeper-nodes"></a>Herstellen einer Verbindung mit Worker- und Zookeeper-Knoten

Auf die Workerknoten und die Zookeeper-Knoten kann über das Internet nicht direkt zugegriffen werden. Der Zugriff ist aber über die Haupt- oder Edgeknoten des Clusters möglich. Im Anschluss finden Sie die allgemeinen Schritte zum Verbinden mit anderen Knoten:

1. Herstellen einer SSH-Verbindung mit einem Haupt- oder Edgeknoten:

        ssh sshuser@myedge.mycluster-ssh.azurehdinsight.net

2. Verwenden Sie für die SSH-Verbindung mit dem Haupt- oder Edgeknoten den Befehl `ssh`, um eine Verbindung mit einem Workerknoten im Cluster herzustellen:

        ssh sshuser@wn0-myhdi

    Informationen zum Abrufen einer Liste mit den Domänennamen der Knoten im Cluster finden Sie unter den Beispielen im Dokument [Verwalten von HDInsight-Clustern mithilfe der Ambari-REST-API](hdinsight-hadoop-manage-ambari-rest-api.md#example-get-the-fqdn-of-cluster-nodes).

Falls das SSH-Konto durch ein __Kennwort__ geschützt ist, werden Sie zur Eingabe dieses Kennworts aufgefordert, und die Verbindung wird hergestellt.

Wenn das SSH-Konto mit __SSH-Schlüsseln__ geschützt ist, müssen Sie sicherstellen, dass Ihre lokale Umgebung für die SSH-Agent-Weiterleitung konfiguriert ist.

> [!NOTE]
> Eine weitere Möglichkeit zum direkten Zugreifen auf alle Knoten im Cluster ist die Installation von HDInsight in einem Azure Virtual Network. Anschließend können Sie Ihren Remotecomputer mit demselben virtuellen Netzwerk verknüpfen und auf alle Knoten im Cluster direkt zugreifen.
>
> Weitere Informationen finden Sie unter [Erweitern der HDInsight-Funktionen mit Azure Virtual Network](hdinsight-extend-hadoop-virtual-network.md).

### <a name="configure-ssh-agent-forwarding"></a>Konfigurieren der SSH-Agent-Weiterleitung

> [!IMPORTANT]
> In den folgenden Schritten wird davon ausgegangen, dass Sie ein Linux-/UNIX-basiertes System und Bash unter Windows 10 verwenden. Falls diese Schritte für Ihr System nicht geeignet sind, ziehen Sie die Dokumentation für Ihren SSH-Client zurate.

1. Öffnen Sie `~/.ssh/config`in einem Text-Editor. Sollte die Datei nicht vorhanden sein, können Sie sie durch Eingabe von `touch ~/.ssh/config` an einer Befehlszeile erstellen.

2. Fügen Sie der Datei `config` den folgenden Text hinzu:

        Host <edgenodename>.<clustername>-ssh.azurehdinsight.net
          ForwardAgent yes

    Ersetzen Sie die __Host__-Informationen durch die Adresse des Knotens, mit dem Sie per SSH eine Verbindung herstellen. Im vorherigen Beispiel wird der Edgeknoten verwendet. Mit diesem Eintrag wird die SSH-Agent-Weiterleitung für den angegebenen Knoten konfiguriert.

3. Testen Sie die SSH-Agent-Weiterleitung über die Eingabe des folgenden Befehls in das Terminal:

        echo "$SSH_AUTH_SOCK"

    Die Ausgabe dieses Befehls sieht in etwa wie folgt aus:

        /tmp/ssh-rfSUL1ldCldQ/agent.1792

    Sollte nichts zurückgegeben werden, wird `ssh-agent` nicht ausgeführt. Sehen Sie sich unter [Using ssh-agent with ssh (http://mah.everybody.org/docs/ssh)](http://mah.everybody.org/docs/ssh) (Verwenden von „ssh-agent“ mit SSH) die Informationen zu den Agent-Startskripts an, oder informieren Sie sich in der Dokumentation Ihres SSH-Clients über spezifische Installations- und Konfigurationsschritte für `ssh-agent`.

4. Nachdem Sie sichergestellt haben, dass **ssh-Agent** ausgeführt wird, verwenden Sie folgenden Befehl, um Ihren privaten SSH-Schlüssel dem Agent hinzuzufügen:

        ssh-add ~/.ssh/id_rsa

    Wenn Ihr privater Schlüssel in einer anderen Datei gespeichert ist, ersetzen Sie `~/.ssh/id_rsa` durch den Pfad zur Datei.

5. Stellen Sie per SSH eine Verbindung mit dem Clusteredgeknoten oder -hauptknoten her. Verwenden Sie anschließend den SSH-Befehl, um eine Verbindung mit einem Worker- oder Zookeeper-Knoten herzustellen. Die Verbindung wird mit dem weitergeleiteten Schlüssel hergestellt.

## <a name="next-steps"></a>Nächste Schritte

* [Verwenden von SSH-Tunneln mit HDInsight](hdinsight-linux-ambari-ssh-tunnel.md)
* [Verwenden eines virtuellen Netzwerks mit HDInsight](hdinsight-extend-hadoop-virtual-network.md)
* [Verwenden von Edgeknoten in HDInsight](hdinsight-apps-use-edge-node.md#access-an-edge-node)
