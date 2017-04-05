---
title: Verwenden von SSH mit HDInsight-Clustern aus PuTTY unter Windows | Microsoft-Dokumentation
description: "Hier erfahren Sie, wie Sie auf Windows-basierten Clients unter Verwendung des PuTTY-SSH-Clients SSH-Schlüssel für die Authentifizierung bei Linux-basierten HDInsight-Clustern erstellen und verwenden."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 639328ca-d800-4fa9-97ed-5664477b88cd
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/04/2017
ms.author: larryfr
ROBOTS: NOINDEX
translationtype: Human Translation
ms.sourcegitcommit: 0d8472cb3b0d891d2b184621d62830d1ccd5e2e7
ms.openlocfilehash: b1806950581e0adbeec52839f12c70599d28100d
ms.lasthandoff: 03/21/2017

---
# <a name="use-ssh-with-hdinsight-hadoop-from-putty-on-windows"></a>Verwenden von SSH mit HDInsight (Hadoop) aus PuTTY unter Windows

> [!div class="op_single_selector"]
> * [PuTTY (Windows)](hdinsight-hadoop-linux-use-ssh-windows.md)
> * [SSH (Windows, Linux, Unix, OS X)](hdinsight-hadoop-linux-use-ssh-unix.md)

[Secure Shell (SSH)](https://en.wikipedia.org/wiki/Secure_Shell) ermöglicht die Remoteausführung von Vorgängen in Linux-basierten HDInsight-Clustern über eine Befehlszeilenschnittstelle. Dieses Dokument enthält Informationen zum Herstellen einer Verbindung von Windows-Clients mit HDInsight über den PuTTY SSH-Client.

> [!NOTE]
> Bei den Schritten in diesem Artikel wird davon ausgegangen, dass Sie ein Windows-basiertes System mit dem PuTTY-SSH-Client verwenden. Falls Sie ein Linux-, Unix-, OS X- oder Windows-System verwenden, das über den Befehl `ssh` verfügt, lesen Sie unter [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Linux, Unix oder OS X](hdinsight-hadoop-linux-use-ssh-unix.md) weiter.

## <a name="prerequisites"></a>Voraussetzungen

* **PuTTY** und **PuTTYGen** für Windows-Clients. Diese Hilfsprogramme stehen unter [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)zur Verfügung.
* Ein zeitgemäßer Webbrowser, der HTML5 unterstützt.

## <a name="what-is-ssh"></a>Was ist SSH?

SSH ist ein Dienstprogramm zur Anmeldung und Remoteausführung von Befehlen auf einem Remoteserver. Bei Linux-basiertem HDInsight stellt SSH eine verschlüsselte Verbindung mit dem Hauptknoten des Clusters her und bietet eine Befehlszeile, über die Sie Befehle eingeben können. Die Befehle werden dann direkt auf dem Server ausgeführt.

In der Vergangenheit wurde von Windows kein SSH-Client bereitgestellt. Bei PuTTY handelt es sich um einen grafischen SSH-Client, der unter Windows installiert werden kann.

### <a name="ssh-user-name"></a>SSH-Benutzername

Ein SSH-Benutzername ist der Name, den Sie für die Authentifizierung beim HDInsight-Cluster verwenden. Wenn Sie während der Erstellung des Clusters einen SSH-Benutzernamen angeben, wird dieser Benutzer in allen Knoten im Cluster erstellt. Nach dem Erstellen des Clusters können Sie diesen Benutzernamen zum Herstellen einer Verbindung mit den Hauptknoten des HDInsight-Clusters verwenden. Ausgehend von den Hauptknoten können Sie dann eine Verbindung mit den einzelnen Workerknoten herstellen.

### <a name="ssh-password-or-public-key"></a>SSH-Kennwort oder öffentlicher Schlüssel

Ein SSH-Benutzer kann entweder ein Kennwort oder einen öffentlichen Schlüssel für die Authentifizierung verwenden. Ein Kennwort ist nur eine von Ihnen erstellte Textzeichenfolge, während ein öffentlicher Schlüssel Teil eines kryptografischen Schlüsselpaars ist, das generiert wurde, um Sie eindeutig zu identifizieren.

Ein Schlüssel ist sicherer als ein Kennwort, es sind jedoch zusätzliche Schritte erforderlich, um den Schlüssel zu generieren, und Sie müssen die Dateien mit dem Schlüssel an einem sicheren Ort verwalten. Wenn jemand Zugriff auf die Schlüsseldateien erhält, hat er Zugriff auf Ihr Konto. Wenn die Schlüsseldateien verloren gehen, können Sie sich nicht mehr bei Ihrem Konto anmelden.

Ein Schlüsselpaar besteht aus einem öffentlichen Schlüssel (der an den HDInsight-Server gesendet wird) und einem privaten Schlüssel (der auf dem Clientcomputer gespeichert ist). Beim Herstellen einer Verbindung mit dem HDInsight-Server über SSH verwendet der SSH-Client den privaten Schlüssel auf Ihrem Computer zur Authentifizierung beim Server.

## <a name="create-an-ssh-key"></a>Erstellen eines SSH-Schlüssels

Verwenden Sie die folgenden Informationen, wenn Sie für Ihren Cluster die Verwendung von SSH-Schlüsseln planen. Wenn Sie ein Kennwort verwenden möchten, können Sie diesen Abschnitt überspringen.

1. Öffnen Sie PuTTYGen.

2. Wählen Sie für **Type of key to generate** die Option **SSH-2 RSA**, und klicken Sie dann auf **Generate**.
   
    ![PuTTYGen-Oberfläche](./media/hdinsight-hadoop-linux-use-ssh-windows/puttygen.png)

3. Bewegen Sie die Maus im Bereich unterhalb der Statusanzeige, bis die Anzeige gefüllt ist. Durch das Bewegen der Maus werden zufällige Daten generiert, die zum Generieren des Schlüssels verwendet werden.
   
    ![Bewegen Sie die Maus](./media/hdinsight-hadoop-linux-use-ssh-windows/movingmouse.png)
   
    Nachdem der Schlüssel generiert wurde, wird der öffentliche Schlüssel angezeigt.

4. Zur Erhöhung der Sicherheit können Sie eine Passphrase in das Feld **Key passphrase** und dann denselben Wert in das Feld **Confirm passphrase** eingeben.
   
    ![Passphrase](./media/hdinsight-hadoop-linux-use-ssh-windows/key.png)
   
   > [!NOTE]
   > Es wird dringend empfohlen, dass Sie für den Schlüssel eine sichere Passphrase verwenden. Wenn Sie die Passphrase vergessen, besteht jedoch keine Möglichkeit, diese wiederherzustellen.

5. Klicken Sie auf **Save private key**, um den Schlüssel in einer **PPK**-Datei zu speichern. Dieser Schlüssel wird zum Authentifizieren für die Linux-basierten HDInsight-Cluster verwendet.
   
   > [!NOTE]
   > Sie sollten diesen Schlüssel an einem sicheren Ort verwahren, da dieser für den Zugriff auf Ihre Linux-basierten HDInsight-Cluster verwendet werden kann.

6. Klicken Sie auf **Save public key**, um den Schlüssel in einer **TXT**-Datei zu speichern. Dadurch können Sie den öffentlichen Schlüssel zukünftig wiederverwenden, wenn Sie zusätzliche Linux-basierte HDInsight-Cluster erstellen.
   
   > [!NOTE]
   > Der öffentliche Schlüssel wird auch am oberen Rand von PuTTYGen angezeigt. Sie können mit der rechten Maustaste auf dieses Feld klicken, den Wert kopieren und ihn dann in ein Formular einfügen, wenn Sie einen Cluster mit dem Azure-Portal erstellen.

## <a name="create-a-linux-based-hdinsight-cluster"></a>Erstellen eines Linux-basierten HDInsight-Clusters

Wenn Sie einen Linux-basierten HDInsight-Cluster erstellen, müssen Sie den zuvor erstellten öffentlichen Schlüssel bereitstellen. Es gibt Möglichkeiten, um über Windows-Clients einen Linux-basierten HDInsight-Cluster zu erstellen:

* **Azure-Portal** : Zum Erstellen des Clusters wird ein webbasiertes Portal verwendet.

* **Azure-CLI für Mac, Linux und Windows** : Zum Erstellen des Clusters werden Befehle über die Befehlszeile eingegeben.

Für jede dieser Methoden ist der öffentliche Schlüssel erforderlich. Vollständige Informationen zum Erstellen eines Linux-basierten HDInsight-Clusters finden Sie unter [Bereitstellen von Linux-basierten HDInsight-Clustern](hdinsight-hadoop-provision-linux-clusters.md).

### <a name="azure-portal"></a>Azure-Portal

Wenn Sie das [Azure-Portal][preview-portal] verwenden, um einen Linux-basierten HDInsight-Cluster zu erstellen, müssen Sie einen **SSH-Benutzernamen** eingeben und auswählen, ob Sie ein **Kennwort** oder einen **öffentlichen SSH-Schlüssel** eingeben.

Wenn Sie **ÖFFENTLICHER SSH-SCHLÜSSEL** auswählen, können Sie den öffentlichen Schlüssel (angezeigt im Feld **Öffentliche Schlüssel für die Datei für\_ autorisierte OpenSSH-Schlüssel** in PuttyGen) in das Feld **Öffentlicher SSH-Schlüssel** einfügen. Oder klicken Sie auf **Datei auswählen**, um die Datei mit dem öffentlichen Schlüssel zu suchen und auszuwählen.

![Abbildung eines Formulars, das den öffentlichen Schlüssel anfordert](./media/hdinsight-hadoop-linux-use-ssh-windows/ssh-key.png)

Dadurch wird für den angegebenen Benutzer ein Anmeldename erstellt und die Kennwort- oder SSH-Schlüsselauthentifizierung ermöglicht.

### <a name="azure-command-line-interface-for-mac-linux-and-windows"></a>Azure-Befehlszeilenschnittstelle (CLI) für Mac, Linux und Microsoft Azure

Sie können über die [Azure-CLI für Mac, Linux und Windows](../cli-install-nodejs.md) einen neuen Cluster mithilfe des Befehls `azure hdinsight cluster create` erstellen.

Weitere Informationen zur Verwendung dieses Befehls finden Sie unter [Benutzerdefinierte Bereitstellung eines Hadoop-Linux-Clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

## <a name="connect-to-a-linux-based-hdinsight-cluster"></a>Verbinden mit einem Linux-basierten HDInsight-Cluster

1. Öffnen Sie PuTTY.
   
    ![PuTTY-Oberfläche](./media/hdinsight-hadoop-linux-use-ssh-windows/putty.png)
2. Sofern Sie beim Erstellen des Benutzerkontos einen SSH-Schlüssel bereitgestellt haben, müssen Sie die folgenden Schritte ausführen, um den privaten Schlüssel auszuwählen, der zum Authentifizieren für den Cluster verwendet wird:
   
    Erweitern Sie in **Kategorie** erst **Verbindung**, dann **SSH**, und wählen Sie anschließend **Authentifizierung** aus. Klicken Sie abschließend auf **Browse** , und wählen Sie die PPK-Datei aus, die Ihren privaten Schlüssel enthält.
   
    ![PuTTY-Oberfläche, privaten Schlüssel auswählen](./media/hdinsight-hadoop-linux-use-ssh-windows/puttykey.png)

3. Wählen Sie in **Category** die Option **Session** aus. Geben Sie auf dem Bildschirm **Basic options for your PuTTY session** die SSH-Adresse Ihres HDInsight-Servers in das Feld **Host name (or IP address)** ein. Es gibt zwei mögliche SSH-Adressen, die Sie beim Herstellen einer Verbindung mit einem Cluster verwenden können:

    ![PuTTY-Oberfläche mit eingegebener SSH-Adresse ](./media/hdinsight-hadoop-linux-use-ssh-windows/puttyaddress.png)

    * **Hauptknotenadresse**: Wenn Sie eine Verbindung mit dem Hauptknoten des Clusters herstellen möchten, verwenden Sie den Clusternamen und **-ssh.azurehdinsight.net**. Beispiel: **mycluster-ssh.azurehdinsight.net**.
   
    * **Edgeknotenadresse**: Wenn Sie eine Verbindung mit einer R Server-Instanz in einem HDInsight-Cluster herstellen möchten, können Sie mithilfe der Adresse **RServer.CLUSTERNAME.ssh.azurehdinsight.net** eine Verbindung mit dem R Server-Edgeknoten herstellen. Dabei steht „CLUSTERNAME“ für den Namen Ihres Clusters. Beispiel: **RServer.mycluster.ssh.azurehdinsight.net**.
     

4. Geben Sie zum Speichern der Verbindungsinformationen für die künftige Verwendung einen Namen für diese Verbindung unter **Saved Sessions** ein, und klicken Sie dann auf **Save**. Die Verbindung wird zur Liste der gespeicherten Sitzungen hinzugefügt.
5. Klicken Sie auf **Open** , um die Verbindung mit dem Cluster herzustellen.
   
   > [!NOTE]
   > Wenn Sie das erste Mal eine Verbindung mit dem Cluster hergestellt haben, wird eine Sicherheitswarnung angezeigt. Dies ist normal. Wählen Sie **Yes** aus, um den RSA2-Schlüssel des Servers zum Fortfahren zwischenzuspeichern.

6. Geben Sie nach der entsprechenden Aufforderung den Benutzer ein, den Sie beim Erstellen des Clusters eingegeben haben. Wenn Sie für den Benutzer ein Kennwort angegeben haben, werden Sie auch zur Eingabe dieses Kennworts aufgefordert.

> [!NOTE]
> In den oben genannten Schritten wird davon ausgegangen, dass Sie Port 22 verwenden. Über diesen Port wird eine Verbindung mit dem primären Hauptknoten auf dem HDInsight-Cluster hergestellt. Wenn Sie Port 23 verwenden, wird eine Verbindung mit dem sekundären Knoten hergestellt. Weitere Informationen zu Hauptknoten finden Sie unter [Verfügbarkeit und Zuverlässigkeit von Hadoop-Clustern in HDInsight](hdinsight-high-availability-linux.md).

### <a name="connect-to-worker-nodes"></a>Herstellen einer Verbindung mit den Workerknoten

Auf die Workerknoten kann von außerhalb des Azure-Datencenters nicht direkt zugegriffen werden. Doch auf dem Hauptknoten des Clusters ist der Zugriff darauf über SSH möglich.

Sofern Sie beim Erstellen des Benutzerkontos einen SSH-Schlüssel bereitgestellt haben, müssen Sie die folgenden Schritte ausführen, um den privaten Schlüssel auszuwählen, der zum Authentifizieren für den Cluster verwendet wird (wenn Sie eine Verbindung mit den Workerknoten herstellen möchten).

1. Laden Sie Pageant von [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)herunter, und installieren Sie dieses Programm. Es dient zum Zwischenspeichern von SSH-Schlüsseln für PuTTY.

2. Führen Sie Pageant aus. Es wird im Statusbereich minimiert als Symbol angezeigt. Klicken Sie mit rechten Maustaste, und wählen Sie **Add Key**aus.
   
    ![Schlüssel hinzufügen](./media/hdinsight-hadoop-linux-use-ssh-windows/addkey.png)

3. Wenn das Dialogfeld "Durchsuchen" angezeigt wird, wählen Sie die PPK-Datei aus, die den Schlüssel enthält, und klicken Sie dann auf **Open**. Dadurch wird den Schlüssel zu Pageant hinzugefügt, das ihn PuTTY beim Verbinden mit dem Cluster bereitstellt.
   
   > [!IMPORTANT]
   > Wenn Sie einen SSH-Schlüssel verwendet haben, um Ihr Konto zu schützen, müssen Sie die vorherigen Schritte abschließen, bevor eine Verbindung mit Workerknoten hergestellt werden kann.

4. Öffnen Sie PuTTY.

5. Bei Verwendung eines SSH-Schlüssels zur Authentifizierung erweitern Sie im Abschnitt **Category** erst **Connection**, dann **SSH**, und wählen Sie anschließend **Auth** aus.
   
    Aktivieren Sie im Abschnitt **Authentication parameters** das Kontrollkästchen **Allow agent forwarding**. Dadurch kann PuTTY die Zertifikatauthentifizierung über die Verbindung automatisch an den Hauptknoten des Clusters übergeben, wenn eine Verbindung mit Workerknoten hergestellt wird.
   
    ![Allow agent forwarding](./media/hdinsight-hadoop-linux-use-ssh-windows/allowforwarding.png)

6. Verbinden Sie sich, wie weiter oben beschrieben, mit dem Cluster. Wenn Sie einen SSH-Schlüssel für die Authentifizierung verwenden, müssen Sie den Schlüssel nicht auswählen. Der Pageant hinzugefügte SSH-Schlüssel wird zur Authentifizierung beim Cluster verwendet.

7. Nachdem die Verbindung hergestellt wurde, verwenden Sie den folgenden Befehl zum Abrufen einer Liste der Knoten im Cluster. Ersetzen Sie *ADMINPASSWORD* durch das Kennwort Ihres Clusteradministratorkontos. Ersetzen Sie *CLUSTERNAME* durch den Namen Ihres Clusters.
   
        curl --user admin:ADMINPASSWORD https://CLUSTERNAME.azurehdinsight.net/api/v1/hosts
   
    Dadurch werden Informationen im JSON-Format zu den Knoten im Cluster zurückgegeben, einschließlich `host_name`, die den vollqualifizierten Domänennamen (FQDN) für jeden Knoten enthalten. Es folgt ein Beispiel eines Eintrags vom Typ `host_name` , der vom Befehl **curl** zurückgegeben wird:
   
        "host_name" : "workernode0.workernode-0-e2f35e63355b4f15a31c460b6d4e1230.j1.internal.cloudapp.net"

8. Sobald Sie eine Liste der Workerknoten haben, mit denen Sie eine Verbindung herstellen möchten, geben Sie in der PuTTY-Sitzung mit dem Server den folgenden Befehl ein, um eine Verbindung mit einem Workerknoten herzustellen:
   
        ssh USERNAME@FQDN
   
    Ersetzen Sie *USERNAME* durch Ihren SSH-Benutzernamen und *FQDN* durch den vollqualifizierten Domänennamen des Workerknotens. Beispiel: `workernode0.workernode-0-e2f35e63355b4f15a31c460b6d4e1230.j1.internal.cloudapp.net`.
    
    > [!NOTE]
    > Wenn Sie ein Kennwort zur Authentifizierung Ihrer SSH-Sitzung verwenden, werden Sie aufgefordert, das Kennwort erneut einzugeben. Wenn Sie einen SSH-Schlüssel verwenden, sollte die Verbindung ohne Aufforderungen fertig gestellt werden.

9. Sobald die Sitzung eingerichtet ist, ändert sich die Eingabeaufforderung für Ihre PuTTY-Sitzung von `username@hn#-clustername` in `username@wn#-clustername`, um anzugeben, dass Sie mit dem Workerknoten verbunden sind. Alle Befehle, die Sie ab diesem Punkt ausführen, werden auf dem Workerknoten ausgeführt.

10. Wenn Sie mit dem Ausführen von Aktionen auf dem Workerknoten fertig sind, geben Sie den Befehl `exit` zum Schließen der Sitzung mit dem Workerknoten ein. Sie kehren zur Eingabeaufforderung `username@hn#-clustername` zurück.

## <a name="add-more-accounts"></a>Hinzufügen weiterer Konten

Wenn Sie weitere Konten zum Cluster hinzufügen möchten, führen Sie die folgenden Schritte aus:

1. Generieren Sie einen neuen öffentlichen Schlüssel und privatem Schlüssel für das neue Benutzerkonto, wie zuvor beschrieben.

2. Fügen Sie in einer SSH-Sitzung mit dem Cluster den neuen Benutzer mithilfe des folgenden Befehls hinzu:
   
        sudo adduser --disabled-password <username>
   
    Dadurch wird ein neues Benutzerkonto erstellt, aber die Kennwortauthentifizierung deaktiviert.

3. Erstellen Sie das Verzeichnis und die Dateien zum Speichern des Schlüssels mit den folgenden Befehlen:
   
        sudo mkdir -p /home/<username>/.ssh
        sudo touch /home/<username>/.ssh/authorized_keys
        sudo nano /home/<username>/.ssh/authorized_keys

4. Wenn der Nano-Editor geöffnet wird, kopieren Sie die Inhalte des öffentlichen Schlüssels für das neue Benutzerkonto und fügen sie dann ein. Drücken Sie schließlich **STRG+X** zum Speichern der Datei, und beenden Sie den Editor.
   
    ![Abbildung von Nano-Editor mit Beispielschlüssel](./media/hdinsight-hadoop-linux-use-ssh-windows/nano.png)

5. Verwenden Sie den folgenden Befehl, um den Ordner ".ssh" und dessen Inhalt dem neuen Benutzerkonto zuzuordnen:
   
        sudo chown -hR <username>:<username> /home/<username>/.ssh

6. Sie sollten jetzt in der Lage sein, sich beim Server mit dem neuen Benutzerkonto und dem privaten Schlüssel zu authentifizieren.

## <a id="tunnel"></a>SSH-Tunnel

SSH kann auch zum Tunneln lokaler Anforderungen wie etwa Webanforderungen zum HDInsight-Cluster verwendet werden. Die Anforderung wird dann zur angeforderten Ressource weitergeleitet, als ob sie vom Stammknoten des HDInsight-Clusters stammen würde.

> [!IMPORTANT]
> Ein SSH-Tunnel ist für manche Hadoop-Dienste eine Voraussetzung für den Zugriff auf die Webbenutzeroberfläche. Auf die Benutzeroberfläche des Auftragsverlaufs und des Ressourcen-Managers kann beispielsweise nur über einen SSH-Tunnel zugegriffen werden.

Weitere Informationen zum Erstellen und Verwenden eines SSH-Tunnels finden Sie unter [Verwenden von SSH-Tunneling zum Zugriff auf die Ambari-Webbenutzeroberfläche, ResourceManager, JobHistory, NameNode, Oozie und andere Webbenutzeroberflächen](hdinsight-linux-ambari-ssh-tunnel.md).

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie jetzt wissen, wie die Authentifizierung mithilfe eines SSH-Schlüssels erfolgt, erfahren Sie, wie Sie MapReduce mit Hadoop für HDInsight verwenden.

* [Verwenden von Hive mit HDInsight](hdinsight-use-hive.md)
* [Verwenden von Pig mit HDInsight](hdinsight-use-pig.md)
* [Verwenden von MapReduce-Aufträgen mit HDInsight](hdinsight-use-mapreduce.md)

[preview-portal]: https://portal.azure.com/

