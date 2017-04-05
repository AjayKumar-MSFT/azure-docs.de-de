---
title: "Zehn Dinge, die Sie mit der Data Science Virtual Machine machen können | Microsoft Docs"
description: "Führen Sie verschiedene Durchsuchungen von Daten und Modellierungsaufgaben auf der Data Science Virtual Machine aus."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 145dfe3e-2bd2-478f-9b6e-99d97d789c62
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: gokuma;weig;bradsev
translationtype: Human Translation
ms.sourcegitcommit: 503f5151047870aaf87e9bb7ebf2c7e4afa27b83
ms.openlocfilehash: 3b608f341278ceaef9dd112cea38f138be69ee44
ms.lasthandoff: 03/28/2017


---
# <a name="ten-things-you-can-do-on-the-data-science-virtual-machine"></a>Zehn Dinge, die Sie mit der Data Science Virtual Machine machen können
Die Microsoft Data Science Virtual Machine (DSVM) ist eine leistungsfähige Data Science-Entwicklungsumgebung, mit der Sie unterschiedliche Aufgaben zum Durchsuchen und Modellieren von Daten ausführen können. Die Umgebung wird bereits mit mehreren gängigen Datenanalysetools geliefert, sodass Sie schnell mit Ihrer Analyse für lokale Bereitstellungen, Cloud- oder Hybridbereitstellungen beginnen können. Die DSVM arbeitet eng mit zahlreichen Azure-Diensten zusammen und kann Daten lesen und verarbeiten, die bereits in Azure SQL Data Warehouse, Azure Data Lake, Azure Storage oder in DocumentDB gespeichert sind. Sie kann auch andere Analysetools wie Azure Machine Learning und Azure Data Factory nutzen.

In diesem Artikel erfahren Sie, wie Sie Ihre DSVM nutzen können, um verschiedene Data Science-Aufgaben auszuführen und mit anderen Azure-Diensten zu interagieren. Sie können z.B. die folgenden Aufgaben auf der DSVM ausführen:

1. Durchsuchen von Daten und Entwickeln von Modellen lokal auf der DSVM unter Verwendung von Microsoft R Server, Python
2. Verwenden eines Jupyter-Notebooks zum Experimentieren mit Ihren Daten mithilfe von Python 2, Python 3, Microsoft R in einem Browser in einer Enterprise-tauglichen Version von R, entwickelt für Skalierbarkeit und Leistung
3. Operationalisieren von Modellen, die mit R und Python in Azure Machine Learning erstellt wurden, damit Clientanwendungen mithilfe einer einfachen Webdiensteschnittstelle auf Ihre Modelle zugreifen können
4. Verwalten von Azure-Ressourcen über PowerShell oder das Azure-Portal
5. Erweitern Ihres Speicherplatzes und Freigeben von umfangreichen Datasets/Codes für Ihr gesamtes Team durch Erstellen eines Azure-Dateispeichers als bereitstellbares Laufwerk auf Ihrer DSVM
6. Freigeben von Code für Ihr Team über GitHub und Zugriff auf Ihr Repository mit den vorinstallierten Git Clients – Git Bash, Git GUI.
7. Zugriff auf verschiedene Azure Daten und Analysedienste wie Azure-Blobspeicher, Azure Data Lake, Azure, HDInsight (Hadoop), Azure DocumentDB, Azure SQL Data Warehouse und Datenbanken
8. Erstellen von Berichten und Dashboard mit Power BI Desktop – auf der DSVM vorinstalliert – und deren Bereitstellung in der Cloud
9. Dynamische Skalierung Ihrer DSVM gemäß Ihren Projektanforderungen 
10. Installieren zusätzlicher Tools auf Ihrem virtuellen Computer   

> [!NOTE]
> Für viele der in diesem Artikel aufgelisteten zusätzlichen Datenspeicher- und Analysedienste fallen zusätzliche Nutzungsgebühren an. Genauere Angaben zu den Preisen finden Sie auf der Seite [Azure-Preise](https://azure.microsoft.com/pricing/) .
> 
> 

**Voraussetzungen**

* Sie benötigen ein Azure-Abonnement. Sie können sich [hier](https://azure.microsoft.com/free/)für eine kostenlose Testversion registrieren.
* Anleitungen für die Bereitstellung einer Data Science Virtual Machine im Azure-Portal finden Sie unter [Erstellen eines virtuellen Computers](https://portal.azure.com/#create/microsoft-ads.standard-data-science-vmstandard-data-science-vm).

## <a name="1-explore-data-and-develop-models-using-microsoft-r-server-or-python"></a>1. Durchsuchen von Daten und Entwickeln von Modellen mit Microsoft R Server oder Python
Sie können Ihre Datenanalysen mit Sprachen wie R und Python direkt auf der DSVM ausführen.

Für R können Sie eine IDE namens „Revolution R Enterprise 8.0“ verwenden, die Sie im Startmenü oder auf dem Desktop finden. Microsoft hat Bibliotheken zusätzlich zu Open Source/CRAN-R bereitgestellt, um skalierbare Analysen zu ermöglichen sowie das Analysieren von Datenmengen, deren Größe den Arbeitsspeicher überschreitet, in parallelen Datenblöcken. Sie können auch eine R-IDE ihrer Wahl wie [RStudio](https://www.rstudio.com/products/rstudio-desktop/)installieren.

Für Python können Sie eine IDE wie Visual Studio Community Edition verwenden, bei der die „Python Tools for Visual Studio“-Erweiterung (PTVS) vorinstalliert ist. Standardmäßig wird nur eine Basisinstallation von Python 2.7 (ohne Analysebibliothek wie SciKit, Pandas) auf PTVS konfiguriert. Um Anaconda Python 2.7 und 3.5 zu aktivieren, müssen Sie die folgenden Schritte ausführen:

* Erstellen Sie benutzerdefinierte Umgebungen für jede Version durch Navigieren zu **Tools** -> **Python Tools** -> **Python Environments**, und klicken Sie auf „**+ Custom**“ in Visual Studio 2015 Community Edition.
* Geben Sie eine Beschreibung ein, und legen Sie für die Umgebungspräfixpfade *c:\anaconda* für Anaconda Python 2.7 ODER *c:\anaconda\envs\py35* für Anaconda Python 3.5 fest.
* Klicken Sie auf **Auto Detect** und **Apply**, um die Umgebung zu speichern.

So sieht das benutzerdefinierte Umgebungssetup in Visual Studio aus.

![PTVS-Setup](./media/machine-learning-data-science-vm-do-ten-things/PTVSSetup.png)

In der [PTVS-Dokumentation](https://github.com/Microsoft/PTVS/wiki/Selecting-and-Installing-Python-Interpreters#hey-i-already-have-an-interpreter-on-my-machine-but-ptvs-doesnt-seem-to-know-about-it) finden Sie zusätzliche Informationen zum Erstellen der Python-Umgebungen.

Jetzt können Sie ein neues Python-Projekt erstellen. Navigieren Sie zu **File** -> **New** -> **Project** -> **Python**, und wählen Sie die Python-Anwendung aus, die sie erstellen. Sie können die Python-Umgebung für das aktuelle Projekt auf die gewünschte Version (Anaconda 2.7 oder 3.5) festlegen: Klicken Sie mit der rechten Maustaste auf **Python environment**, wählen Sie **Add/Remove Python Environments** aus, und wählen Sie dann die gewünschte Umgebung aus, die Sie dem Projekt zuordnen möchten. Weitere Informationen zum Arbeiten mit PTVS finden Sie auf der [Dokumentationsseite](https://github.com/Microsoft/PTVS/wiki) des Produkts.

## <a name="2-using-a-jupyter-notebook-to-explore-and-model-your-data-with-python-or-r"></a>2. Verwenden eines Jupyter Notebooks zum Durchsuchen und Modellieren Ihrer Daten mit Python oder R
Das Jupyter Notebook ist eine leistungsfähige Umgebung, die eine browserbasierte „IDE“ für das Durchsuchen und Modellieren von Daten bereitstellt. Sie können Python 2, Python 3 oder R (sowohl Microsoft R Open als auch Microsoft R Server) in einem Jupyter Notebook verwenden.

Zum Starten des Jupyter Notebooks klicken Sie auf das Symbol **Jupyter Notebook**im Startmenü/auf dem Desktop. Auf der DSVM können Sie auch zu „https://localhost:9999/“ wechseln, um auf das Jupyter Notebook zuzugreifen. Wenn Sie zur Eingabe eines Kennworts aufgefordert werden, verwenden Sie die Anweisungen im Abschnitt ***Erstellen eines sicheren Kennworts für den Jupyter Notebook-Server*** des Themas [Bereitstellen der Microsoft Data Science Virtual Machine](machine-learning-data-science-provision-vm.md), um ein sicheres Kennwort für den Zugriff auf das Jupyter Notebook zu erstellen. 

Sobald Sie das Notebook geöffnet haben, sehen Sie ein Verzeichnis, das einige Beispielnotebooks enthält, die in der DSVM vorkonfiguriert sind. Sie können jetzt:

* auf das Notebook klicken, um den Code zu sehen
* Jede Zelle über **UMSCHALT+EINGABE-Taste**ausführen.
* Das gesamte Notebook ausführen, indem Sie auf **Cell** -> **Run** klicken.
* Ein neues Notebook erstellen, indem Sie auf das Jupyter-Symbol klicken (linke obere Ecke), dann auf der rechten Seite auf die Schaltfläche **New** klicken und die Notebook-Sprache (auch Kernels genannt) wählen.   

> [!NOTE]
> Derzeit werden Python 2.7, Python 3.5 und R unterstützt. Der R-Kernel unterstützt sowohl das Programmieren in Open Source-R als auch den Enterprise-skalierbaren Microsoft R-Server.   
> 
> 

Sobald Sie das Notebook geöffnet haben, können Sie mit den Bibliotheken Ihrer Wahl Ihre Daten durchsuchen, das Modell erstellen und das Modell testen.

## <a name="3-build-models-using-r-or-python-and-operationalize-them-using-azure-machine-learning"></a>3. Erstellen von Modellen mit R oder Python und Operationalisieren dieser Modelle durch Verwenden von Azure Machine Learning
Sobald Sie Ihr Modell erstellt und überprüft haben, ist der nächste Schritt in der Regel die Bereitstellung für die Produktion. Dies ermöglicht Ihren Clientanwendungen, die Modellvorhersagen auf Echtzeit- oder Batchmodusbasis aufzurufen. Azure Machine Learning bietet einen Mechanismus zum Operationalisieren eines in R oder Python erstellten Modells.

Wenn Sie Ihr Modell in Azure Machine Learning operationalisieren, steht ein Webdienst zur Verfügung, der Clients REST-Aufrufe erlaubt, die Eingabeparameter übergeben und Vorhersagen aus dem Modell als Ausgaben empfangen.   

> [!NOTE]
> Wenn Sie sich noch nicht für Azure Machine Learning registriert haben, erhalten Sie einen kostenlosen Arbeitsbereich oder einen Standardarbeitsbereich, wenn Sie die Startseite von [Azure Machine Learning Studio](https://studio.azureml.net/) besuchen und auf „Sign Up“ (Registrieren) klicken.   
> 
> 

### <a name="build-and-operationalize-python-models"></a>Erstellen und Operationalisieren von Python-Modellen
Hier sehen Sie den Ausschnitt eines in einem Python Jupyter Notebook entwickelten Codes, der durch Verwenden der SciKit-Learn-Bibliothek ein einfaches Modell erstellt.

    #IRIS classification
    from sklearn import datasets
    from sklearn import svm
    clf = svm.SVC()
    iris = datasets.load_iris()
    X, y = iris.data, iris.target
    clf.fit(X, y)

Die Methode, die zum Bereitstellen Ihrer Python-Modelle in Azure Machine Learning verwendet wird, umschließt die Vorhersage des Modells in eine Funktion und stattet sie mit Attributen aus, die von der vorinstallierten Azure Machine Learning-Python-Bibliothek bereitgestellt werden, die Ihre Azure Machine Learning-Arbeitsbereichs-ID, den API-Schlüssel, die Eingabe- und Rückgabeparameter bezeichnet.  

    from azureml import services
    @services.publish(workspaceid, auth_token)
    @services.types(sep_l = float, sep_w = float, pet_l=float, pet_w=float)
    @services.returns(int) #0, or 1, or 2
    def predictIris(sep_l, sep_w, pet_l, pet_w):
     inputArray = [sep_l, sep_w, pet_l, pet_w]
    return clf.predict(inputArray)

Ein Client kann jetzt den Webdienst aufrufen. Es gibt benutzerfreundliche Wrapper, die die REST-API-Anforderungen erstellen. Hier ist ein Beispielcode zur Nutzung des Webdiensts.

    # Consume through web service URL and keys
    from azureml import services
    @services.service(url, api_key)
    @services.types(sep_l = float, sep_w = float, pet_l=float, pet_w=float)
    @services.returns(float)
    def IrisPredictor(sep_l, sep_w, pet_l, pet_w):
    pass

    IrisPredictor(3,2,3,4)


> [!NOTE]
> Die Azure Machine Learning-Bibliothek wird derzeit nur von Python 2.7 unterstützt.   
> 
> 

### <a name="build-and-operationalize-r-models"></a>Erstellen und Operationalisieren von R-Modellen
Sie können R-Modelle, die auf der Data Science Virtual Machine oder anderswo erstellt wurden, auf Azure Machine Learning in einer Weise bereitstellen, die derjenigen für Python ähnelt. Gehen Sie wie folgt vor:

* Erstellen Sie wie unten gezeigt eine Datei „settings.json“, um Ihre Arbeitsbereichs-ID und Ihr Authentifizierungstoken bereitzustellen.
* Schreiben Sie einen Wrapper für die Vorhersagefunktion des Modells.
* Rufen Sie ```publishWebService``` in der Azure Machine Learning-Bibliothek auf, um den Wrapper für die Funktion zu übergeben.  

Hier sehen Sie die Vorgehensweise und Codeausschnitte, die Sie verwenden können, um ein Modell als Webdienst in Azure Machine Learning einzurichten, zu erstellen, zu veröffentlichen und zu nutzen.

#### <a name="setup"></a>Einrichtung
1. Installieren Sie das Machine Learning-R-Paket, indem Sie ```install.packages("AzureML")``` in die IDE „Revolution R Enterprise 8.0“ oder in Ihre R-IDE eingeben.
2. RTools können Sie [hier](https://cran.r-project.org/bin/windows/Rtools/)herunterladen. Das ZIP-Hilfsprogramm muss sich im Pfad befinden (und mit „zip.exe“ benannt sein), um Ihr R-Paket in Machine Learning zu operationalisieren.
3. Erstellen Sie in Ihrem Basisverzeichnis in einem Verzeichnis namens ```.azureml``` eine Datei namens „settings.json“, und geben Sie die Parameter aus Ihrem Azure Machine Learning-Arbeitsbereich ein:

Struktur der Datei „settings.json“:

    {"workspace":{
    "id"                  : "ENTER YOUR AZUREML WORKSPACE ID",
    "authorization_token" : "ENTER YOUR AZUREML AUTH TOKEN"
    }}


#### <a name="build-a-model-in-r-and-publish-it-in-azure-machine-learning"></a>Erstellen eines Modells in R und Veröffentlichen des Modells in Azure Machine Learning
    library(AzureML)
    ws <- workspace(config="~/.azureml/settings.json")

    if(!require("lme4")) install.packages("lme4")
    library(lme4)
    set.seed(1)
    train <- sleepstudy[sample(nrow(sleepstudy), 120),]
    m <- lm(Reaction ~ Days + Subject, data = train)

    # Define a prediction function to publish based on the model:
    sleepyPredict <- function(newdata){
          predict(m, newdata=newdata)
    }

    ep <- publishWebService(ws, fun = sleepyPredict, name="sleepy lm", inputSchema = sleepstudy, data.frame=TRUE)

#### <a name="consume-the-model-deployed-in-azure-machine-learning"></a>Nutzen des bereitgestellten Modells in Azure Machine Learning
Um das Modell aus einer Clientanwendung zu nutzen, verwenden wir die Azure Machine Learning-Bibliothek, um den veröffentlichten Webdienst anhand seines Namens mit dem API-Aufruf `services` zu suchen, und um den Endpunkt zu bestimmen. Dann rufen Sie einfach die Funktion `consume` auf und übergeben den Datenrahmen, der vorhergesagt werden soll.
Der folgende Code wird verwendet, um das als Azure Machine Learning-Webdienst veröffentlichte Modell zu nutzen.

    library(AzureML)
    library(lme4)
    ws <- workspace(config="~/.azureml/settings.json")

    s <-  services(ws, name = "sleepy lm")
    s <- tail(s, 1) # use the last published function, in case of duplicate function names

    ep <- endpoints(ws, s)

    # OK, try this out, and compare with raw data
    ans = consume(ep, sleepstudy)$ans

Weitere Informationen über die Azure Machine Learning-R-Bibliothek finden Sie [hier](https://cran.r-project.org/web/packages/AzureML/AzureML.pdf).

## <a name="4-administer-your-azure-resources-using-azure-portal-or-powershell"></a>4. Verwalten von Azure-Ressourcen über PowerShell oder das Azure-Portal
Die DSVM ermöglicht Ihnen nicht nur, Ihre Analyselösung lokal auf dem virtuellen Computer zu erstellen, sondern auch, auf Dienste der Azure-Cloud von Microsoft zuzugreifen. Azure bietet mehrere Compute-, Speicher-, Datenanalyse- und andere Dienste, die Sie von Ihrer DSVM aus verwalten, und auf die Sie von dort aus zugreifen können.

Um Ihr Azure-Abonnement und Ihre Cloudressourcen zu verwalten, können Sie in Ihrem Browser auf das [Azure-Portal](https://portal.azure.com)zugreifen. Sie können auch Azure PowerShell zur Verwaltung Ihres Azure-Abonnements und Ihrer Ressourcen über ein Skript verwenden.
Sie können Azure PowerShell über eine Verknüpfung auf dem Desktop oder im Startmenü „Microsoft Azure Powershell“ ausführen. In der [Microsoft Azure PowerShell-Dokumentation](../powershell-azure-resource-manager.md) finden Sie weitere Informationen dazu, wie Sie Ihr Azure-Abonnement und Ihre Ressourcen mithilfe von Windows PowerShell-Skripts verwalten können.

## <a name="5-extend-your-storage-space-with-a-shared-file-system"></a>5. Erweitern Ihres Speichers mit einem Shared-Dateisystem
Datenanalysten können große DataSets, Code oder andere Ressourcen innerhalb des Teams freigeben. Die DSVM selbst hat etwa 70 GB verfügbaren Speicherplatz. Um Ihren Speicher zu erweitern, können Sie den Azure-Dateidienst verwenden und auf der DSVM einbinden, oder über eine REST-API darauf zugreifen.   

> [!NOTE]
> Der maximale Speicherplatz der Azure-Dateidienstfreigabe beträgt 5 TB, und die Begrenzung für eine einzelne Datei beträgt 1 TB.   
> 
> 

Azure PowerShell können Sie verwenden, um eine Azure-Dateidienstfreigabe zu erstellen. Dieses Skript können Sie in Azure PowerShell ausführen, um eine Azure-Dateidienstfreigabe zu erstellen.

    # Authenticate to Azure.
    Login-AzureRmAccount
    # Select your subscription
    Get-AzureRmSubscription –SubscriptionName "<your subscription name>" | Select-AzureRmSubscription
    # Create a new resource group.
    New-AzureRmResourceGroup -Name <dsvmdatarg>
    # Create a new storage account. You can reuse existing storage account if you wish.
    New-AzureRmStorageAccount -Name <mydatadisk> -ResourceGroupName <dsvmdatarg> -Location "<Azure Data Center Name For eg. South Central US>" -Type "Standard_LRS"
    # Set your current working storage account
    Set-AzureRmCurrentStorageAccount –ResourceGroupName "<dsvmdatarg>" –StorageAccountName <mydatadisk>

    # Create a Azure File Service Share
    $s = New-AzureStorageShare <<teamsharename>>
    # Create a directory under the FIle share. You can give it any name
    New-AzureStorageDirectory -Share $s -Path <directory name>
    # List the share to confirm that everything worked
    Get-AzureStorageFile -Share $s


Die neu erstellte Azure-Dateidienstfreigabe können Sie nun in jeden virtuellen Computer in Azure einbinden. Die VM sollte sich in dem gleichen Azure-Rechenzentrum wie das Speicherkonto befinden, um Latenz und Datenübertragungsgebühren zu vermeiden. Mit den folgenden Befehlen binden Sie das Laufwerk in die DSVM ein, die Sie unter Azure PowerShell ausführen können.

    # Get storage key of the storage account that has the Azure file share from Azure portal. Store it securely on the VM to avoid prompted in next command.
    cmdkey /add:<<mydatadisk>>.file.core.windows.net /user:<<mydatadisk>> /pass:<storage key>

    # Mount the Azure file share as Z: drive on the VM. You can chose another drive letter if you wish
    net use z:  \\<mydatadisk>.file.core.windows.net\<<teamsharename>>


Jetzt können Sie auf dieses Laufwerk wie auf jedes normale Laufwerk auf der VM zugreifen.

## <a name="6-share-code-with-your-team-using-github"></a>6. Freigeben von Code für Ihr Team mithilfe von GitHub
GitHub ist ein Coderepository, in dem Sie umfangreichen Beispielcode und Quellen für verschiedene Tools finden, die unterschiedliche Technologien einsetzen. Sie werden von der gesamten Entwicklercommunity verwendet. Sie nutzt Git als Technologie zum Nachverfolgen und speichern der Versionen der Codedateien. GitHub ist auch eine Plattform, auf der Sie Ihr eigenes Repository erstellen können, um freigegebenen Code und die Dokumentation Ihres Teams zu speichern. Zudem können Sie die Versionskontrolle implementieren und auch steuern, wer Zugriffsrechte hat, um Code anzuzeigen und bereitzustellen. Auf den [GitHub-Hilfeseiten](https://help.github.com/) finden Sie weitere Informationen zur Verwendung von Git. Sie können GitHub als eine der Möglichkeiten nutzen, mit Ihrem Team zusammenzuarbeiten, von der Community entwickelten Code zu verwenden und wiederum Code für die Community bereitzustellen.

Die DSVM enthält bereits Clienttools, die den Zugriff auf das GitHub-Repository sowohl über die Befehlszeile als auch über die GUI ermöglichen. Das Befehlszeilentool zur Verwendung von Git und GitHub heißt Git Bash. Die auf der DSVM installierte Version von Visual Studio besitzt die Git-Erweiterungen. Sie finden die Startsymbole für diese Tools auf dem Desktop und im Startmenü.

Zum Herunterladen von Code aus einem GitHub-Repository verwenden Sie den Befehl ```git clone```. Um z. B. das von Microsoft veröffentlichte Data Science-Repository in das aktuelle Verzeichnis herunterzuladen, können Sie den folgenden Befehl ausführen, sobald Sie sich in ```git-bash``` befinden.

    git clone https://github.com/Azure/Azure-MachineLearning-DataScience.git

In Visual Studio können Sie den gleichen Klonvorgang ausführen. Im folgenden Screenshot sehen Sie, wie Sie in Visual Studio auf Git- und GitHub-Tools zugreifen können.

![Git in Visual Studio](./media/machine-learning-data-science-vm-do-ten-things/VSGit.PNG)

Weitere Informationen zur Verwendung von Git zur Arbeit mit Ihrem GitHub-Repository finden Sie in verschiedenen Quellen, die unter github.com verfügbar sind. Der [Spickzettel](https://training.github.com/kit/downloads/github-git-cheat-sheet.pdf) ist eine nützliche Referenz.

## <a name="7-access-various-azure-data-and-analytics-services"></a>7. Zugriff auf verschiedene Azure-Daten und Analysedienste
### <a name="azure-blob"></a>Azure Blob
Azure Blob ist ein zuverlässiger, wirtschaftlicher Cloudspeicher für große und kleine Datenmengen. Wir sehen uns nun an, wie Sie Daten in ein Azure-Blob verschieben und auf Daten zugreifen können, die in einem Azure-Blob gespeichert sind.

**Voraussetzung**

* **Erstellen Sie ein Azure-Blobspeicherkonto im [Azure-Portal](https://portal.azure.com).**

![Create_Azure_Blob](./media/machine-learning-data-science-vm-do-ten-things/Create_Azure_Blob.PNG)

* Stellen Sie sicher, dass sich das vorinstallierte Befehlszeilentool AzCopy in ```C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy.exe```befindet. Sie können das Verzeichnis, das „azcopy.exe“ enthält, Ihrer PATH-Umgebungsvariable hinzufügen, damit Sie beim Ausführen des Tools nicht den ganzen Befehlspfad eingeben müssen. Weitere Informationen zu AzCopy finden Sie in der [AzCopy-Dokumentation](../storage/storage-use-azcopy.md)
* Starten Sie das Tool Azure-Speicher-Explorer. Das Tool kann von der Seite [Microsoft Azure Storage Explorer](http://storageexplorer.com/)heruntergeladen werden. 

![AzureStorageExplorer_v4](./media/machine-learning-data-science-vm-do-ten-things/AzureStorageExplorer_v4.png)

**Verschieben von Daten vom virtuellen Computer in den Azure-Blobspeicher: AzCopy**

Zum Verschieben von Daten zwischen Ihren lokalen Dateien und dem Blobspeicher können Sie AzCopy in der Befehlszeile oder PowerShell verwenden:

    AzCopy /Source:C:\myfolder /Dest:https://<mystorageaccount>.blob.core.windows.net/<mycontainer> /DestKey:<storage account key> /Pattern:abc.txt

Ersetzen Sie **C:\myfolder** mit dem Pfad, in dem Ihre Datei gespeichert wird, **mystorageaccount** durch Ihren Blobspeicher-Kontonamen, **mycontainer** durch den Containernamen, **storage account key** durch Ihren Blobspeicher-Zugriffsschlüssel. Sie finden die Anmeldeinformationen für Ihr Speicherkonto im [Azure-Portal](https://portal.azure.com).

![StorageAccountCredential_v2](./media/machine-learning-data-science-vm-do-ten-things/StorageAccountCredential_v2.png)

Führen Sie den AzCopy-Befehl in PowerShell oder im Eingabeaufforderungsfenster aus. Hier sehen Sie ein Beispiel für die Nutzung des AzCopy-Befehls:

    # Copy *.sql from local machine to a Azure Blob
    "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:"c:\Aaqs\Data Science Scripts" /Dest:https://[ENTER STORAGE ACCOUNT].blob.core.windows.net/[ENTER CONTAINER] /DestKey:[ENTER STORAGE KEY] /S /Pattern:*.sql

    # Copy back all files from Azure Blob container to Local machine

    "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Dest:"c:\Aaqs\Data Science Scripts\temp" /Source:https://[ENTER STORAGE ACCOUNT].blob.core.windows.net/[ENTER CONTAINER] /SourceKey:[ENTER STORAGE KEY] /S



Kurz nachdem Sie den AzCopy-Befehl zum Kopieren in einen Azure-Blobspeicher ausgeführt haben, werden Ihre Dateien im Azure Storage-Explorer angezeigt.

![AzCopy_run_finshed_Storage_Explorer_v3](./media/machine-learning-data-science-vm-do-ten-things/AzCopy_run_finshed_Storage_Explorer_v3.png)

**Verschieben von Daten vom virtuellen Computer in den Azure-Blobspeicher: Azure Storage-Explorer**

Außerdem können Sie Daten aus der lokalen Datei auf Ihrem virtuellen Computer mit dem Azure Storage-Explorer hochladen:

* Um Daten in einen Container hochzuladen, wählen Sie den Zielcontainer aus, und klicken Sie auf die Schaltfläche **Hochladen**.![Hochladen in den Speicher-Explorer](./media/machine-learning-data-science-vm-do-ten-things/storage-accounts.png)
* Klicken Sie rechts neben dem Feld **Dateien** auf die Schaltfläche **...**, wählen Sie im Dateisystem eine oder mehrere Dateien zum Hochladen aus, und klicken Sie auf **Hochladen**, um mit dem Hochladen der Dateien zu beginnen.![Hochladen von Dateien in das Blob](./media/machine-learning-data-science-vm-do-ten-things/upload-files-to-blob.png)

**Lesen von Daten aus dem Azure-Blob: Machine Learning-Readermodul**

In Azure Machine Learning Studio können Sie mit einem **„Import Data“-Modul** Daten aus Ihrem Blobspeicher lesen.

![AML_ReaderBlob_Module_v3](./media/machine-learning-data-science-vm-do-ten-things/AML_ReaderBlob_Module_v3.png)

**Lesen von Daten aus dem Azure-Blobspeicher: Python ODBC**

Sie können die **BlobService** -Bibliothek verwenden, um Daten direkt aus dem Blob in einem Jupyter Notebook- oder Python-Programm zu lesen.

Importieren Sie zunächst die erforderlichen Pakete:

    import pandas as pd
    from pandas import Series, DataFrame
    import numpy as np
    import matplotlib.pyplot as plt
    from time import time
    import pyodbc
    import os
    from azure.storage.blob import BlobService
    import tables
    import time
    import zipfile
    import random

Geben Sie dann Ihre Anmeldeinformationen für den Azure-Blobspeicher an, und lesen Sie Daten aus dem Blobspeicher:

    CONTAINERNAME = 'xxx'
    STORAGEACCOUNTNAME = 'xxxx'
    STORAGEACCOUNTKEY = 'xxxxxxxxxxxxxxxx'
    BLOBNAME = 'nyctaxidataset/nyctaxitrip/trip_data_1.csv'
    localfilename = 'trip_data_1.csv'
    LOCALDIRECTORY = os.getcwd()
    LOCALFILE =  os.path.join(LOCALDIRECTORY, localfilename)

    #download from blob
    t1 = time.time()
    blob_service = BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)
    blob_service.get_blob_to_path(CONTAINERNAME,BLOBNAME,LOCALFILE)
    t2 = time.time()
    print(("It takes %s seconds to download "+BLOBNAME) % (t2 - t1))

    #unzipping downloaded files if needed
    #with zipfile.ZipFile(ZIPPEDLOCALFILE, "r") as z:
    #    z.extractall(LOCALDIRECTORY)

    df1 = pd.read_csv(LOCALFILE, header=0)
    df1.columns = ['medallion','hack_license','vendor_id','rate_code','store_and_fwd_flag','pickup_datetime','dropoff_datetime','passenger_count','trip_time_in_secs','trip_distance','pickup_longitude','pickup_latitude','dropoff_longitude','dropoff_latitude']
    print 'the size of the data is: %d rows and  %d columns' % df1.shape

Die Daten werden als Datenrahmen eingelesen:

![IPNB_data_readin](./media/machine-learning-data-science-vm-do-ten-things/IPNB_data_readin.PNG)

### <a name="azure-data-lake"></a>Azure Data Lake
Azure Data Lake Store ist ein Repository mit Hyperskalierung für Big Data-Analyseworkloads und ist mit dem Hadoop Distributed File System (HDFS) kompatibel. Der Speicher funktioniert sowohl mit dem Hadoop-Ökosystem als auch mit Azure Data Lake Analytics. Wir zeigen Ihnen, wie Sie Daten in den Azure Data Lake-Speicher verschieben und Analysen mithilfe von Azure Data Lake Analytics ausführen.

**Voraussetzung**

* Erstellen Sie Azure Data Lake Analytics im [Azure-Portal](https://portal.azure.com).

![Azure_Data_Lake_Create_v2](./media/machine-learning-data-science-vm-do-ten-things/Azure_Data_Lake_Create_v2.png)

* Die **Azure Data Lake-Tools** in **Visual Studio**, die Sie unter diesem [Link](https://www.microsoft.com/download/details.aspx?id=49504) finden, sind bereits in der Visual Studio Community Edition installiert, die sich auf dem virtuellen Computer befindet. Wenn Sie sich nach dem Start von Visual Studio bei Ihrem Azure-Abonnement angemeldet haben, werden Ihr Azure Data Analytics-Konto und der zugehörige Speicher im linken Bereich von Visual Studio angezeigt.

![Azure_Data_Lake_PlugIn_v2](./media/machine-learning-data-science-vm-do-ten-things/Azure_Data_Lake_PlugIn_v2.PNG)

**Verschieben von Daten vom virtuellen Computer in Data Lake: Azure Data Lake Explorer**

Sie können **Azure Data Lake Explorer** zum Hochladen von Daten aus den lokalen Dateien auf Ihrem virtuellen Computer in den Data Lake Store verwenden.

![Azure_Data_Lake_UploadData](./media/machine-learning-data-science-vm-do-ten-things/Azure_Data_Lake_UploadData.PNG)

Sie können auch eine Datenpipeline erstellen, um die Datenverschiebung zu oder von Azure Data Lake mithilfe der [Azure Data Factory (ADF)](https://azure.microsoft.com/services/data-factory/)in die Produktion umzusetzen. In diesem [Artikel](https://azure.microsoft.com/blog/creating-big-data-pipelines-using-azure-data-lake-and-azure-data-factory/) finden Sie eine Anleitung zum Erstellen einer Datenpipeline.

**Lesen von Daten aus dem Azure-Blobspeicher in Data Lake: U-SQL**

Wenn Ihre Daten sich im Azure-Blobspeicher befinden, können Sie die Daten mit einer U-SQL-Abfrage direkt aus dem Azure-Blobspeicher lesen. Stellen Sie vor dem Formulieren der U-SQL-Abfrage sicher, dass Ihr Blobspeicherkonto mit Azure Data Lake verknüpft ist. Rufen Sie das **Azure-Portal** auf, klicken Sie auf Ihrem Azure Data Lake Analytics-Dashboard auf **Datenquelle hinzufügen**, wählen Sie als Speichertyp **Azure Storage** aus, und geben Sie den Namen und den Schlüssel Ihres Azure Storage-Kontos an. Anschließend können Sie auf die auf dem Speicherkonto gespeicherten Daten verweisen.

![Eingeben von Speicherkonto und Schlüssel](./media/machine-learning-data-science-vm-do-ten-things/Link_Blob_to_ADLA_v2.PNG)

In Visual Studio können Sie Daten aus dem Blobspeicher lesen, einige Datenmanipulationen ausführen, Features entwickeln und die resultierenden Daten entweder in Azure Data Lake oder im Azure-Blobspeicher ausgeben. Wenn Sie auf die Daten im Blobspeicher verweisen, verwenden Sie **wasb://**; wenn Sie auf die Daten in Azure Data Lake verweisen, verwenden Sie **swbhdfs://**.

![Datenrahmen](./media/machine-learning-data-science-vm-do-ten-things/USQL_Read_Blob_v2.PNG)

Sie können die folgenden U-SQL-Abfragen in Visual Studio verwenden:

    @a =
        EXTRACT medallion string,
                hack_license string,
                vendor_id string,
                rate_code string,
                store_and_fwd_flag string,
                pickup_datetime string,
                dropoff_datetime string,
                passenger_count int,
                trip_time_in_secs double,
                trip_distance double,
                pickup_longitude string,
                pickup_latitude string,
                dropoff_longitude string,
                dropoff_latitude string

        FROM "wasb://<Container name>@<Azure Blob Storage Account Name>.blob.core.windows.net/<Input Data File Name>"
        USING Extractors.Csv();

    @b =
        SELECT vendor_id,
        COUNT(medallion) AS cnt_medallion,
        SUM(passenger_count) AS cnt_passenger,
        AVG(trip_distance) AS avg_trip_dist,
        MIN(trip_distance) AS min_trip_dist,
        MAX(trip_distance) AS max_trip_dist,
        AVG(trip_time_in_secs) AS avg_trip_time
        FROM @a
        GROUP BY vendor_id;

    OUTPUT @b   
    TO "swebhdfs://<Azure Data Lake Storage Account Name>.azuredatalakestore.net/<Folder Name>/<Output Data File Name>"
    USING Outputters.Csv();

    OUTPUT @b   
    TO "wasb://<Container name>@<Azure Blob Storage Account Name>.blob.core.windows.net/<Output Data File Name>"
    USING Outputters.Csv();



Nachdem Ihre Abfrage an den Server gesendet wurde, wird ein Diagramm mit dem Status des Auftrags angezeigt.

![Auftragsstatusdiagramm](./media/machine-learning-data-science-vm-do-ten-things/USQL_Job_Status.PNG)

**Datenabfragen in Data Lake: U-SQL**

Nachdem das Dataset in Azure Data Lake aufgenommen wurde, können Sie die [U-SQL-Sprache](../data-lake-analytics/data-lake-analytics-u-sql-get-started.md) zum Abfragen und Durchsuchen der Daten verwenden. Die U-SQL-Sprache ähnelt T-SQL, kombiniert aber einige Funktionen von C#, sodass Benutzer angepasste Module, benutzerdefinierte Funktionen usw. schreiben können. Sie können die Skripts aus dem vorherigen Schritt verwenden.

Nachdem die Abfrage an den Server gesendet wurde, finden Sie nach kurzer Zeit die Datei „tripdata_summary.CSV“ in **Azure Data Lake Explorer**. Zur Datenvorschau können Sie mit der rechten Maustaste auf die Datei klicken.

![Datei in Azure Data Lake Explorer](./media/machine-learning-data-science-vm-do-ten-things/USQL_create_summary.png)

So zeigen Sie die Dateiinformationen an:

![Dateizusammenfassung](./media/machine-learning-data-science-vm-do-ten-things/USQL_tripdata_summary.png)

### <a name="hdinsight-hadoop-clusters"></a>HDInsight Hadoop-Cluster
Azure HDInsight ist ein verwalteter Apache Hadoop-, Spark-, HBase- und Storm-Dienst in der Cloud. Sie können problemlos mit Azure HDInsight-Clustern der Data Science Virtual Machine arbeiten.

**Voraussetzung**

* Erstellen Sie ein Azure-Blobspeicherkonto im [Azure-Portal](https://portal.azure.com). Dieses Speicherkonto wird zum Speichern von Daten für HDInsight-Cluster verwendet.

![Erstellen des Azure Blob Storage-Kontos](./media/machine-learning-data-science-vm-do-ten-things/Create_Azure_Blob.PNG)

* Passen Sie Azure HDInsight Hadoop-Cluster im [Azure-Portal](machine-learning-data-science-customize-hadoop-cluster.md)
  
  * Sie müssen das mit dem HDInsight-Cluster erstellte Speicherkonto verknüpfen, sobald es erstellt ist. Mit diesem Speicherkonto wird auf Daten zugegriffen, die innerhalb des Clusters verarbeitet werden können.

![Verknüpfen mit dem Speicherkonto, das mit dem HDInsight-Cluster erstellt wurde](./media/machine-learning-data-science-vm-do-ten-things/Create_HDI_v4.PNG)

* Sie müssen nach dem Erstellen den **Remotezugriff** auf den Hauptknoten des Clusters aktivieren. Merken Sie sich die hier angegebenen RAS-Anmeldeinformationen (die sich von denen beim Erstellen des Clusters unterscheiden). Sie werden diese später benötigen.

![Aktivieren des Remotezugriffs](./media/machine-learning-data-science-vm-do-ten-things/Create_HDI_dashboard_v3.PNG)

* Erstellen Sie einen Azure Machine Learning-Arbeitsbereich. Ihre Machine Learning-Experimente werden in diesem Machine Learning-Arbeitsbereich gespeichert. Wählen Sie die hervorgehobenen Optionen im Portal aus, wie im Screenshot unten dargestellt.

![Erstellen eines Azure Machine Learning-Arbeitsbereichs](./media/machine-learning-data-science-vm-do-ten-things/Create_ML_Space.PNG)

* Geben Sie die Parameter für Ihren Arbeitsbereich ein.

![Eingeben der Parameter für den Machine Learning-Arbeitsbereich](./media/machine-learning-data-science-vm-do-ten-things/Create_ML_Space_step2_v2.PNG)

* Hochladen von Daten mit IPython Notebook. Importieren Sie zunächst die erforderlichen Pakete, geben Sie Ihre Anmeldeinformationen ein, erstellen Sie eine Datenbank in Ihrem Speicherkonto, und laden Sie dann Daten in die HDI-Cluster hoch.

        #Import required Packages
        import pyodbc
        import time as time
        import json
        import os
        import urllib
        import urllib2
        import warnings
        import re
        import pandas as pd
        import matplotlib.pyplot as plt
        from azure.storage.blob import BlobService
        warnings.filterwarnings("ignore", category=UserWarning, module='urllib2')


        #Create the connection to Hive using ODBC
        SERVER_NAME='xxx.azurehdinsight.net'
        DATABASE_NAME='nyctaxidb'
        USERID='xxx'
        PASSWORD='xxxx'
        DB_DRIVER='Microsoft Hive ODBC Driver'
        driver = 'DRIVER={' + DB_DRIVER + '}'
        server = 'Host=' + SERVER_NAME + ';Port=443'
        database = 'Schema=' + DATABASE_NAME
        hiveserv = 'HiveServerType=2'
        auth = 'AuthMech=6'
        uid = 'UID=' + USERID
        pwd = 'PWD=' + PASSWORD
        CONNECTION_STRING = ';'.join([driver,server,database,hiveserv,auth,uid,pwd])
        connection = pyodbc.connect(CONNECTION_STRING, autocommit=True)
        cursor=connection.cursor()


        #Create Hive database and tables
        queryString = "create database if not exists nyctaxidb;"
        cursor.execute(queryString)

        queryString = """
                        create external table if not exists nyctaxidb.trip
                        (
                            medallion string,
                            hack_license string,
                            vendor_id string,
                            rate_code string,
                            store_and_fwd_flag string,
                            pickup_datetime string,
                            dropoff_datetime string,
                            passenger_count int,
                            trip_time_in_secs double,
                            trip_distance double,
                            pickup_longitude double,
                            pickup_latitude double,
                            dropoff_longitude double,
                            dropoff_latitude double)  
                        PARTITIONED BY (month int)
                        ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' lines terminated by '\\n'
                        STORED AS TEXTFILE LOCATION 'wasb:///nyctaxidbdata/trip' TBLPROPERTIES('skip.header.line.count'='1');
                    """
        cursor.execute(queryString)

        queryString = """
                        create external table if not exists nyctaxidb.fare
                        (
                            medallion string,
                            hack_license string,
                            vendor_id string,
                            pickup_datetime string,
                            payment_type string,
                            fare_amount double,
                            surcharge double,
                            mta_tax double,
                            tip_amount double,
                            tolls_amount double,
                            total_amount double)
                        PARTITIONED BY (month int)
                        ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' lines terminated by '\\n'
                        STORED AS TEXTFILE LOCATION 'wasb:///nyctaxidbdata/fare' TBLPROPERTIES('skip.header.line.count'='1');
                    """
        cursor.execute(queryString)


        #Upload data from blob storage to HDI cluster
        for i in range(1,13):
            queryString = "LOAD DATA INPATH 'wasb:///nyctaxitripraw2/trip_data_%d.csv' INTO TABLE nyctaxidb2.trip PARTITION (month=%d);"%(i,i)
            cursor.execute(queryString)
            queryString = "LOAD DATA INPATH 'wasb:///nyctaxifareraw2/trip_fare_%d.csv' INTO TABLE nyctaxidb2.fare PARTITION (month=%d);"%(i,i)  
            cursor.execute(queryString)


* Alternativ dazu können Sie auch dieser [exemplarischen Vorgehensweise](machine-learning-data-science-process-hive-walkthrough.md) folgen, um NYC Taxi-Daten in den HDI-Cluster hochzuladen. Zu den wichtigsten Schritten zählen:
  
  * AzCopy: Herunterladen gezippter CSV-Dateien aus dem öffentlichen Blobspeicher in Ihren lokalen Ordner
  * AzCopy: Hochladen entzippter CSV-Dateien aus dem lokalen Ordner in den HDI-Cluster
  * Anmelden beim Hauptknoten des Hadoop-Clusters und Vorbereitung einer explorativen Datenanalyse

Nachdem die Daten in den HDI-Cluster geladen wurden, können Sie Ihre Daten im Azure Storage-Explorer überprüfen. Und Sie besitzen eine im HDI-Cluster erstellte Datenbank „nyctaxidb“.

**Durchsuchen von Daten: Hive-Abfragen in Python**

Da sich die Daten im Hadoop-Cluster befinden, können Sie das Paket „pyodbc“ verwenden, um Verbindungen mit Hadoop-Clustern herzustellen und die Datenbank mit Hive abzufragen, um Durchsuchungsvorgänge und Funktionsverarbeitungen durchzuführen. Sie können die vorhandenen Tabellen anzeigen, die wir im erforderlichen Schritt erstellt haben.

    queryString = """
        show tables in nyctaxidb2;
        """
    pd.read_sql(queryString,connection)


![Anzeigen vorhandener Tabellen](./media/machine-learning-data-science-vm-do-ten-things/Python_View_Existing_Tables_Hive_v3.PNG)

Betrachten wir nun die Anzahl der Datensätze in jedem Monat, und wie häufig Trinkgeld gegeben wurde, in der Fahrttabelle:

    queryString = """
        select month, count(*) from nyctaxidb.trip group by month;
        """
    results = pd.read_sql(queryString,connection)

    %matplotlib inline

    results.columns = ['month', 'trip_count']
    df = results.copy()
    df.index = df['month']
    df['trip_count'].plot(kind='bar')


![Darstellung der Anzahl von Datensätzen in den einzelnen Monaten](./media/machine-learning-data-science-vm-do-ten-things/Exploration_Number_Records_by_Month_v3.PNG)

    queryString = """
        SELECT tipped, COUNT(*) AS tip_freq
        FROM
        (
            SELECT if(tip_amount > 0, 1, 0) as tipped, tip_amount
            FROM nyctaxidb.fare
        )tc
        GROUP BY tipped;
        """
    results = pd.read_sql(queryString,connection)

    results.columns = ['tipped', 'trip_count']
    df = results.copy()
    df.index = df['tipped']
    df['trip_count'].plot(kind='bar')


![Darstellung der Trinkgeldhäufigkeit](./media/machine-learning-data-science-vm-do-ten-things/Exploration_Frequency_tip_or_not_v3.PNG)

Außerdem können wir die Entfernung zwischen dem Aufnahmestandort und dem Absetzstandort berechnen und sie dann mit der Fahrtstrecke vergleichen.

    queryString = """
                    select pickup_longitude, pickup_latitude, dropoff_longitude, dropoff_latitude, trip_distance, trip_time_in_secs,
                        3959*2*2*atan((1-sqrt(1-pow(sin((dropoff_latitude-pickup_latitude)
                        *radians(180)/180/2),2)-cos(pickup_latitude*radians(180)/180)
                        *cos(dropoff_latitude*radians(180)/180)*pow(sin((dropoff_longitude-pickup_longitude)*radians(180)/180/2),2)))
                        /sqrt(pow(sin((dropoff_latitude-pickup_latitude)*radians(180)/180/2),2)
                        +cos(pickup_latitude*radians(180)/180)*cos(dropoff_latitude*radians(180)/180)*
                        pow(sin((dropoff_longitude-pickup_longitude)*radians(180)/180/2),2))) as direct_distance
                        from nyctaxidb.trip
                        where month=1
                            and pickup_longitude between -90 and -30
                            and pickup_latitude between 30 and 90
                            and dropoff_longitude between -90 and -30
                            and dropoff_latitude between 30 and 90;
                """
    results = pd.read_sql(queryString,connection)
    results.head(5)


![Aufnahme- und Absetztabelle](./media/machine-learning-data-science-vm-do-ten-things/Exploration_compute_pickup_dropoff_distance_v2.PNG)

    results.columns = ['pickup_longitude', 'pickup_latitude', 'dropoff_longitude',
                       'dropoff_latitude', 'trip_distance', 'trip_time_in_secs', 'direct_distance']
    df = results.loc[results['trip_distance']<=100] #remove outliers
    df = df.loc[df['direct_distance']<=100] #remove outliers
    plt.scatter(df['direct_distance'], df['trip_distance'])


![Darstellung mit Aufnahme-/Absetzstrecke und Fahrtstrecke](./media/machine-learning-data-science-vm-do-ten-things/Exploration_direct_distance_trip_distance_v2.PNG)

Nun bereiten wir komprimierte Daten (1 %) für die Modellierung vor. Wir können diese Daten im Machine Learning-Readermodul verwenden.

        queryString = """
        create  table if not exists nyctaxi_downsampled_dataset_testNEW (
        medallion string,
        hack_license string,
        vendor_id string,
        rate_code string,
        store_and_fwd_flag string,
        pickup_datetime string,
        dropoff_datetime string,
        pickup_hour string,
        pickup_week string,
        weekday string,
        passenger_count int,
        trip_time_in_secs double,
        trip_distance double,
        pickup_longitude double,
        pickup_latitude double,
        dropoff_longitude double,
        dropoff_latitude double,
        direct_distance double,
        payment_type string,
        fare_amount double,
        surcharge double,
        mta_tax double,
        tip_amount double,
        tolls_amount double,
        total_amount double,
        tipped string,
        tip_class string
        )
        row format delimited fields terminated by ','
        lines terminated by '\\n'
        stored as textfile;
        """
        cursor.execute(queryString)

        --- now insert contents of the join into the above internal table

        queryString = """
        insert overwrite table nyctaxi_downsampled_dataset_testNEW
        select
        t.medallion,
        t.hack_license,
        t.vendor_id,
        t.rate_code,
        t.store_and_fwd_flag,
        t.pickup_datetime,
        t.dropoff_datetime,
        hour(t.pickup_datetime) as pickup_hour,
        weekofyear(t.pickup_datetime) as pickup_week,
        from_unixtime(unix_timestamp(t.pickup_datetime, 'yyyy-MM-dd HH:mm:ss'),'u') as weekday,
        t.passenger_count,
        t.trip_time_in_secs,
        t.trip_distance,
        t.pickup_longitude,
        t.pickup_latitude,
        t.dropoff_longitude,
        t.dropoff_latitude,
        t.direct_distance,
        f.payment_type,
        f.fare_amount,
        f.surcharge,
        f.mta_tax,
        f.tip_amount,
        f.tolls_amount,
        f.total_amount,
        if(tip_amount>0,1,0) as tipped,
        if(tip_amount=0,0,
        if(tip_amount>0 and tip_amount<=5,1,
        if(tip_amount>5 and tip_amount<=10,2,
        if(tip_amount>10 and tip_amount<=20,3,4)))) as tip_class
        from
        (
        select
        medallion,
        hack_license,
        vendor_id,
        rate_code,
        store_and_fwd_flag,
        pickup_datetime,
        dropoff_datetime,
        passenger_count,
        trip_time_in_secs,
        trip_distance,
        pickup_longitude,
        pickup_latitude,
        dropoff_longitude,
        dropoff_latitude,
        3959*2*2*atan((1-sqrt(1-pow(sin((dropoff_latitude-pickup_latitude)
        radians(180)/180/2),2)-cos(pickup_latitude*radians(180)/180)
        *cos(dropoff_latitude*radians(180)/180)*pow(sin((dropoff_longitude-pickup_longitude)*radians(180)/180/2),2)))
        /sqrt(pow(sin((dropoff_latitude-pickup_latitude)*radians(180)/180/2),2)
        +cos(pickup_latitude*radians(180)/180)*cos(dropoff_latitude*radians(180)/180)*pow(sin((dropoff_longitude-pickup_longitude)*radians(180)/180/2),2))) as direct_distance,
        rand() as sample_key

        from trip
        where pickup_latitude between 30 and 90
            and pickup_longitude between -90 and -30
            and dropoff_latitude between 30 and 90
            and dropoff_longitude between -90 and -30
        )t
        join
        (
        select
        medallion,
        hack_license,
        vendor_id,
        pickup_datetime,
        payment_type,
        fare_amount,
        surcharge,
        mta_tax,
        tip_amount,
        tolls_amount,
        total_amount
        from fare
        )f
        on t.medallion=f.medallion and t.hack_license=f.hack_license and t.pickup_datetime=f.pickup_datetime
        where t.sample_key<=0.01
        """
        cursor.execute(queryString)

Nach einer Weile können Sie sehen, dass die Daten in Hadoop-Cluster geladen wurden:

    queryString = """
        select * from nyctaxi_downsampled_dataset limit 10;
        """
    cursor.execute(queryString)
    pd.read_sql(queryString,connection)


![Datentabelle](./media/machine-learning-data-science-vm-do-ten-things/DownSample_Data_For_Modeling_v2.PNG)

**Lesen von Daten aus HDI mithilfe von Machine Learning: Readermodul**

Sie können auch das **Readermodul** in Azure Machine Learning Studio für den Datenbankzugriff in Hadoop-Clustern verwenden. Geben Sie die Anmeldeinformationen für Ihre HDI-Cluster und Ihr Azure Storage-Konto an, und Sie können die Datenbank in HDI-Clustern verwenden, um Machine Learning-Modelle zu erstellen.

![Eigenschaften des Readermoduls](./media/machine-learning-data-science-vm-do-ten-things/AML_Reader_Hive.PNG)

Das bewertete Dataset kann dann angezeigt werden:

![Anzeigen des bewerteten Datasets](./media/machine-learning-data-science-vm-do-ten-things/AML_Model_Results.PNG)

### <a name="azure-sql-data-warehouse--databases"></a>Azure SQL Data Warehouse und Datenbanken
Azure SQL Data Warehouse ist ein elastisches Data Warehouse-as-a-Service-Angebot mit SQL Server-Funktionalität auf Unternehmensniveau.

Sie können Ihr Azure SQL Data Warehouse bereitstellen, indem Sie die Anweisungen in diesem [Artikel](../sql-data-warehouse/sql-data-warehouse-get-started-provision.md)befolgen. Nachdem Sie Ihr Azure SQL Data Warehouse bereitgestellt haben, können Sie diese [exemplarische Vorgehensweise](machine-learning-data-science-process-sqldw-walkthrough.md) verwenden, um Daten in SQL Data Warehouse hochzuladen, zu durchsuchen und zu modellieren.

#### <a name="azure-documentdb"></a>Azure DocumentDB
Azure DocumentDB ist eine NoSQL-Datenbank in der Cloud. Sie ermöglicht Ihnen, mit Dokumenten wie JSON zu arbeiten und die Dokumente zu speichern und abzufragen.

Sie müssen die folgenden erforderlichen Schritte ausführen, um über die DSVM auf DocumentDB zuzugreifen.

1. Installieren Sie das DocumentDB Python SDK (führen Sie ```pip install pydocumentdb``` an der Befehlszeile aus).
2. Erstellen Sie im [Azure-Portal](https://portal.azure.com)
3. Laden Sie [hier](http://www.microsoft.com/downloads/details.aspx?FamilyID=cda7703a-2774-4c07-adcc-ad02ddc1a44d) das DocumentDB-Migrationstool herunter, und extrahieren Sie es in ein Verzeichnis Ihrer Wahl.
4. Importieren Sie JSON-Daten, die in einem [öffentlichen Blobspeicher](https://cahandson.blob.core.windows.net/samples/volcano.json) gespeichert sind (Daten zu Vulkanen), mit den folgenden Befehlsparametern des Migrationstools („dtui.exe“ in dem Verzeichnis, in dem Sie das DocumentDB-Migrationstool installiert haben) in DocumentDB. Geben Sie die unten stehenden Quell-und Zielpositionsparameter ein.
   
    /s:JsonFile /s.Files:https://cahandson.blob.core.windows.net/samples/volcano.json /t:DocumentDBBulk /t.ConnectionString:AccountEndpoint=https://[DocDBAccountName].documents.azure.com:443/;AccountKey=[[KEY];Database=volcano /t.Collection:volcano1

Sobald Sie die Daten importiert haben, können Sie in Jupyter das Notebook namens *DocumentDBSample* öffnen, das den Python-Code für den Zugriff auf DocumentDB und die Ausführung einiger grundlegender Abfragen enthält. Weitere Informationen zu DocumentDB finden Sie auf der [Dokumentationsseite](https://azure.microsoft.com/documentation/learning-paths/documentdb/)

## <a name="8-build-reports-and-dashboard-using-the-power-bi-desktop"></a>8. Erstellen von Berichten und Dashboard mit Power BI Desktop
Nun visualisieren wir die Volcano JSON-Datei aus dem obigen DocumentDB-Beispiel in Power BI, um visuelle Einblicke in die Daten zu erhalten. Eine ausführliche Anleitung finden Sie im [Power BI-Artikel](../documentdb/documentdb-powerbi-visualize.md). Die allgemeinen Schritte sind:

1. Öffnen Sie Power BI Desktop, und führen Sie „Get Data“ aus. Geben Sie die URL an: https://cahandson.blob.core.windows.net/samples/volcano.json
2. Sie sollten die JSON-Datensätze in einer importierten Liste sehen.
3. Konvertieren Sie die Liste in eine Tabelle, damit Power BI damit arbeiten kann.
4. Erweitern Sie die Spalten durch Klicken auf das Erweiterungssymbol (das Symbol „Pfeil nach links und Pfeil nach rechts“ auf der rechten Seite der Spalte).
5. Beachten Sie, dass bei „Location“ das Feld „Record“ steht. Erweitern Sie den Datensatz und wählen Sie nur „coordinates“. „coordinates“ ist eine Listenspalte.
6. Fügen Sie eine neue Spalte hinzu, um die Listenspalte „coordinates“ in eine kommagetrennte LatLong-Spalte zu konvertieren, wobei die beiden Elemente im Listenfeld „coordinates“ mithilfe der Formel ```Text.From([coordinates]{1})&","&Text.From([coordinates]{0})```verkettet werden.
7. Konvertieren Sie schließlich die Spalte ```Elevation``` zu „Dezimal“, und wählen Sie **Schließen** und **Anwenden**.

Anstelle der oben beschriebenen Schritte können Sie den folgenden Code, in dem die obigen Schritte ausformuliert sind, in den erweiterten Editor in Power BI einfügen, der Ihnen ermöglicht, die Datentransformationen in einer Abfragesprache zu schreiben.

    let
        Source = Json.Document(Web.Contents("https://cahandson.blob.core.windows.net/samples/volcano.json")),
        #"Converted to Table" = Table.FromList(Source, Splitter.SplitByNothing(), null, null, ExtraValues.Error),
        #"Expanded Column1" = Table.ExpandRecordColumn(#"Converted to Table", "Column1", {"Volcano Name", "Country", "Region", "Location", "Elevation", "Type", "Status", "Last Known Eruption", "id"}, {"Volcano Name", "Country", "Region", "Location", "Elevation", "Type", "Status", "Last Known Eruption", "id"}),
        #"Expanded Location" = Table.ExpandRecordColumn(#"Expanded Column1", "Location", {"coordinates"}, {"coordinates"}),
        #"Added Custom" = Table.AddColumn(#"Expanded Location", "LatLong", each Text.From([coordinates]{1})&","&Text.From([coordinates]{0})),
        #"Changed Type" = Table.TransformColumnTypes(#"Added Custom",{{"Elevation", type number}})
    in
        #"Changed Type"



Jetzt befinden sich die Daten in Ihrem Power BI-Datenmodell. Ihr Power BI-Desktop sollte nun wie unten dargestellt aussehen.

![Power BI Desktop](./media/machine-learning-data-science-vm-do-ten-things/PowerBIVolcanoData.png)

Sie können beginnen, Berichte und Visualisierungen mit dem Datenmodell zu erstellen. Sie können die in diesem [Power BI-Artikel](../documentdb/documentdb-powerbi-visualize.md#build-the-reports) beschriebenen Schritte verwenden, um einen Bericht zu erstellen. Das Endergebnis ist ein Bericht, der wie folgt aussieht.

![Power BI Desktop-Berichtsansicht – Power BI-Connector](./media/machine-learning-data-science-vm-do-ten-things/power_bi_connector_pbireportview2.png)

## <a name="9-dynamically-scale-your-dsvm-to-meet-your-project-needs"></a>9. Dynamische Skalierung Ihrer DSVM gemäß Ihren Projektanforderungen 
Sie können die DSVM gemäß Ihren Projektanforderungen herauf- oder herabskalieren. Wenn Sie die VM abends oder am Wochenende nicht benötigen, können Sie sie einfach vom [Azure-Portal](https://portal.azure.com)aus herunterfahren.

> [!NOTE]
> Es fallen Computegebühren an, wenn Sie nur die Betriebssystemschaltfläche zum Herunterfahren auf der VM verwenden.  
> 
> 

Wenn Sie einige umfangreiche Analysen abwickeln müssen und mehr CPU- und/oder Arbeitsspeicher- und/oder Datenträgerkapazität benötigen, finden Sie eine große Auswahl an VM-Größen in Bezug auf CPU-Kerne, Speicherkapazität und Datenträgertypen (einschließlich Solid-State-Laufwerke), die Ihren Compute- und Budgetanforderungen entsprechen. Die vollständige Liste der VMs zusammen mit ihren Computepreisen pro Stunde finden Sie auf der Seite [Azure Virtual Machines – Preise](https://azure.microsoft.com/pricing/details/virtual-machines/) .

Auch wenn Ihr Bedarf an VM-Verarbeitungskapazität sinkt (wenn Sie z.B. eine erhebliche Workload in einen Hadoop- oder Spark-Cluster verschoben haben), können Sie den Cluster im [Azure-Portal](https://portal.azure.com) über die Einstellungen Ihrer VM-Instanz zentral herunterskalieren. Hier sehen Sie einen Screenshot.

![VM-Instanzeinstellungen](./media/machine-learning-data-science-vm-do-ten-things/VMScaling.PNG)

## <a name="10-install-additional-tools-on-your-virtual-machine"></a>10. Installieren zusätzlicher Tools auf Ihrem virtuellen Computer
Wir haben mehrere Tools zu einem Paket zusammengefasst, die unserer Ansicht nach vielen der allgemeinen Datenanalyseanforderungen gerecht werden und Ihnen Zeit sparen sollten, da sie vermeiden, dass Sie Ihre Umgebungen einzeln installieren und konfigurieren müssen. Außerdem sparen Sie Geld, da Sie nur für die Ressourcen bezahlen, die Sie auch wirklich verwenden.

Sie können andere in diesem Artikel dargestellte Azure-Datendienste und -Analysedienste nutzen, um Ihre Analyseumgebung zu verbessern. Wir wissen, dass in manchen Fällen für Ihre Bedürfnisse zusätzliche Tools, einschließlich geschützter Drittanbietertools, erforderlich sein können. Sie haben vollen administrativen Zugriff auf den virtuellen Computer, um neue Tools zu installieren, die Sie benötigen. Sie können auch zusätzliche Pakete in Python und R installieren, die nicht vorinstalliert sind. Für Python können Sie entweder ```conda``` oder ```pip``` verwenden. Für R können Sie ```install.packages()``` in der R-Konsole verwenden, oder Sie verwenden die IDE und wählen „**Packages** -> **Install Packages...**“ aus.

## <a name="summary"></a>Zusammenfassung
Dies sind nur einige Dinge, die mit der Microsoft Data Science Virtual Machine möglich sind. Sie können viele weitere Dinge tun, um sie als wirksame Analyseumgebung zu nutzen.


