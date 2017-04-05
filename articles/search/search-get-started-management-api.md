---
title: "Erste Schritte mit der REST-API für die Verwaltung von Azure Search | Microsoft Docs"
description: Verwalten Ihres in der Cloud gehosteten Azure Search-Diensts mithilfe einer Verwaltungs-REST-API
services: search
documentationcenter: 
author: HeidiSteen
manager: jhubbard
editor: 
ms.assetid: cd4c41d8-81bd-4609-9a37-e112ddf1f21f
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 10/17/2016
ms.author: heidist
translationtype: Human Translation
ms.sourcegitcommit: 503f5151047870aaf87e9bb7ebf2c7e4afa27b83
ms.openlocfilehash: 4fe0db2a5fb9b7798d8583721ac35b2e02435d7d
ms.lasthandoff: 03/28/2017


---
# <a name="get-started-with-azure-search-management-rest-api"></a>Erste Schritte mit der Azure Search Management-REST-API
> [!div class="op_single_selector"]
> * [Portal](search-manage.md)
> * [PowerShell](search-manage-powershell.md)
> * [REST-API](search-get-started-management-api.md)
>
>

Die Azure Search Management-REST-API ist eine programmatische Alternative zum Ausführen von Verwaltungsaufgaben im Verwaltungsportal. Dienstverwaltungsvorgänge umfassen Erstellen oder Löschen des Dienstes, Skalieren des Dienstes und Verwalten von Schlüsseln. Dieses Lernprogramm enthält eine Beispielclientanwendung, die die Dienstverwaltungs-API veranschaulicht. Darüber hinaus sind die erforderlichen Konfigurationsschritte zum Ausführen des Beispiels in der lokalen Entwicklungsumgebung enthalten.

Um dieses Lernprogramm abzuschließen, benötigen Sie:

* Visual Studio 2012 oder 2013
* Beispielclientanwendung zum Herunterladen

Im Verlauf des Lernprogramms werden zwei Dienste bereitgestellt: Azure Search und Active Directory (AD). Darüber hinaus erstellen Sie eine AD-Anwendung, die eine Vertrauensstellung zwischen Ihrer Clientanwendung und dem Ressourcenmanagerendpunkt in Azure herstellt.

Sie benötigen ein Azure-Konto, um dieses Lernprogramm durchführen zu können:

## <a name="download-the-sample-application"></a>Herunterladen der Beispielanwendung
Dieses Lernprogramm basiert auf einer Windows-Konsolenanwendung, geschrieben in C#, die Sie bearbeiten und in Visual Studio 2012 oder 2013 ausführen können

Die Clientanwendung finden Sie auf GitHub unter [Azure Search .NET Management API Demo](https://github.com/Azure-Samples/search-dotnet-management-api/) (Demo der .NET-Verwaltungs-API für Azure Search).

## <a name="configure-the-application"></a>Konfigurieren der Anwendung
Bevor Sie die Beispielanwendung ausführen können, müssen Sie Authentifizierung aktivieren, damit von der Clientanwendung an den Ressourcenmanagerendpunkt gesendete Anforderungen akzeptiert werden können. Die Authentifizierungsanforderung stammt aus dem [Azure-Ressourcen-Manager](https://msdn.microsoft.com/library/azure/dn790568.aspx), der die Grundlage für alle über eine API angeforderten Portalvorgänge bildet, einschließlich jener im Zusammenhang mit der Search-Dienstverwaltung. Die Dienstverwaltungs-API für Azure Search ist einfach eine Erweiterung des Azure Resource Manager und erbt daher seine Abhängigkeiten.  

Azure Resource Manager benötigt den Azure Active Directory-Dienst als Identitätsanbieter.

Um ein Zugriffstoken zu erhalten, mit dem Anforderungen den Ressourcenmanager erreichen können, enthält die Clientanwendung ein Codesegment, das Active Directory aufruft. Das Codesegment sowie die erforderlichen Schritte zur Verwendung des Codesegments wurden aus diesem Artikel übernommen: [Authentifizieren von Anforderungen des Azure-Ressourcen-Managers](http://msdn.microsoft.com/library/azure/dn790557.aspx).

Sie können die Anweisungen aus dem oben aufgeführten Link oder die Schritte in diesem Dokument verwenden, wenn Sie es vorziehen, das Lernprogramm Schritt für Schritt zu durchlaufen.

In diesem Abschnitt führen Sie die folgenden Aufgaben aus:

1. Erstellen eines AD-Diensts
2. Erstellen einer AD-Anwendung
3. Konfigurieren Sie die AD-Anwendung durch die Registrierung von Details über die Beispielclientanwendung, die Sie heruntergeladen haben.
4. Laden Sie die Beispielclientanwendung mit Werten, die dazu verwendet werden, Autorisierungen für ihre Anforderungen zu erhalten.

> [!NOTE]
> Diese Links bieten Hintergrundinformationen zur Verwendung von Azure Active Directory für die Authentifizierung von Clientanforderungen an den Ressourcen-Manager: [Azure Resource Manager](http://msdn.microsoft.com/library/azure/dn790568.aspx), [Authentifizieren von Azure Resource Manager-Anforderungen](http://msdn.microsoft.com/library/azure/dn790557.aspx) und [Azure Active Directory](http://msdn.microsoft.com/library/azure/jj673460.aspx).
>
>

### <a name="create-an-active-directory-service"></a>Erstellen eines Active Directory-Diensts
1. Melden Sie sich beim [Azure-Portal](https://manage.windowsazure.com)an.
2. Führen Sie im linken Navigationsbereich  einen Bildlauf nach unten aus, und klicken Sie auf **Active Directory**.
3. Klicken Sie auf **NEU**, um **App-Dienste** | **Active Directory** zu öffnen. In diesem Schritt erstellen Sie einen neuen Active Directory-Dienst. Dieser Dienst hostet die AD-Anwendung, die Sie einige Schritte weiter definieren. Das Erstellen eines neuen Diensts hilft dabei, das Lernprogramm von anderen Anwendungen zu isolieren, die Sie bereits in Azure hosten.
4. Klicken Sie auf **Verzeichnis** | **Benutzerdefiniert erstellen**.
5. Geben Sie einen Dienstnamen, eine Domäne und den geografischen Standort ein. Die Domäne muss eindeutig sein. Aktivieren Sie das Kontrollkästchen, um die Warteschlange zu erstellen.

### <a name="create-a-new-ad-application-for-this-service"></a>Erstellen einer neuen AD-Anwendung für diesen Dienst
1. Wählen Sie den Active Directory-Dienst "SearchTutorial", den Sie gerade erstellt haben.
2. Klicken Sie im oberen Menü auf **Anwendungen**.
3. Klicken Sie auf **Eine Anwendung hinzufügen**. Eine AD-Anwendung speichert Informationen über die Clientanwendungen, die sie als Identitätsanbieter verwenden.  
4. Wählen Sie **Eine von meinem Unternehmen entwickelte Anwendung hinzufügen**aus. Diese Option bietet die Registrierungseinstellungen für Anwendungen, die nicht in der Anwendungsgalerie veröffentlicht werden. Da die Client-Anwendung nicht Teil der Galerie ist, ist dies die richtige Wahl für dieses Lernprogramm.
5. Geben Sie einen Namen ein, z. B. "Azure-Search-Manager".
6. Wählen Sie als Anwendungstyp **Systemeigene Clientanwendung** aus. Dies ist korrekt für die Beispielanwendung. Es ist eine Windows-Clientnwendung (Konsole), keine Webanwendung.
7. Geben Sie als Umleitungs-URI "http://localhost/Azure-Search-Manager-App" an. Dies ist ein URI, zu dem Azure Active Directory den Benutzer-Agent als Antwort auf eine OAuth 2.0-Autorisierungsanforderung umleitet. Der Wert muss kein physischer Endpunkt, aber unbedingt ein gültiger URI sein.

    Für die Zwecke dieses Lernprogramms ist der Wert beliebig. Was Sie eingeben wird jedoch eine erforderliche Eingabe für die administrative Verbindung in der Beispielanwendung.
8. Klicken Sie auf das Häkchen, um die Active Directory-Anwendung zu erstellen. Im linken Navigationsbereich sollte "Azure-Search-Manager-App" angezeigt werden.

### <a name="configure-the-ad-application"></a>Konfigurieren der AD-Anwendung
1. Klicken Sie auf die AD-Anwendung "Azure-Search-Manager-App", die Sie gerade erstellt haben. Sie sollte im linken Navigationsbereich angezeigt werden.
2. Klicken Sie im oberen Menü auf **Konfigurieren** .
3. Führen Sie einen Bildlauf nach unten zu den Berechtigungen durch, und wählen Sie **Azure Management-API**. In diesem Schritt geben Sie die API an (in diesem Fall die Azure Resource Manager-API), auf welche die Clientanwendung Zugriff benötigt, sowie die erforderliche Ebene des Zugriffs.
4. Klicken Sie in den delegierten Berechtigungen auf die Dropdownliste, und wählen Sie **Access Azure Service Management (Preview)**aus.
5. Speichern Sie die Änderungen.

Lassen Sie die Konfigurationsseite geöffnet. Im nächsten Schritt kopieren Sie die Werte auf dieser Seite und geben sie in der Beispielanwendung ein.

### <a name="load-the-sample-application-program-with-registration-and-subscription-values"></a>Laden der Beispielanwendung mit Registrierungs- und Abonnementwerten
In diesem Abschnitt bearbeiten Sie die Projektmappe in Visual Studio und setzen gültige Werte ein, die Sie aus dem Verwaltungsportal abgerufen haben.
Die Werte, die Sie hinzufügen sollen, werden am Anfang der Datei "Program.cs" angezeigt:

        private const string TenantId = "<your tenant id>";
        private const string ClientId = "<your client id>";
        private const string SubscriptionId = "<your subscription id>";
        private static readonly Uri RedirectUrl = new Uri("<your redirect url>");

Wenn Sie die Beispielanwendung noch nicht [von GitHub heruntergeladen](https://github.com/Azure-Samples/search-dotnet-management-api/) haben, ist dies für diesen Schritt jetzt erforderlich.

1. Öffnen Sie die Datei **ManagementAPI.sln** in Visual Studio.
2. Öffnen Sie die Datei Program.cs.
3. Geben Sie `ClientId`an. Kopieren Sie von der AD-Konfigurationsseite, die noch vom vorherigen Schritt geöffnet ist, die Client-ID der Konfigurationsseite der AD-Anwendung im Verwaltungsportal, und fügen Sie diese in die Datei "Program.cs" ein.
4. Geben Sie `RedirectUrl`an. Kopieren Sie den Umleitungs-URI von der gleichen Portalseite, und fügen Sie ihn in die Datei "Program.cs" ein.
5. Angabe von `TenantID.`

   * Gehen Sie zurück zu Active Directory | SearchTutorial (Dienst).
   * Klicken Sie in der oberen Leiste auf **Anwendungen** .
   * Klicken Sie unten auf der Seite auf **Endpunkte anzeigen** .
   * Kopieren Sie den OAUTH 2.0-Autorisierungsendpunkt unten auf der Liste.
   * Fügen Sie den Endpunkt unter "TenantID" ein, und verkürzen Sie dabei den Wert aller URI-Parameter, mit Ausnahme der Mandanten-ID.

     Wenn "https://login.windows.net/55e324c7-1656-4afe-8dc3-43efcd4ffa50/oauth2/authorize?api-version=1.0" ist, löschen Sie alles mit Ausnahme von "55e324c7-1656-4afe-8dc3-43efcd4ffa50".

6. Geben Sie `SubscriptionID`an.

   * Wechseln Sie zur Hauptseite des Portals.
   * Klicken Sie unten im linken Navigationsbereich auf **Einstellungen** .
   * Kopieren Sie die Abonnement-ID aus der Registerkarte "Abonnements", und fügen Sie diese in die Datei "Program.cs" ein.
7. Speichern und erstellen Sie die Projektmappe.

## <a name="explore-the-application"></a>Erkunden der Anwendung
Fügen Sie einen Haltepunkt am ersten Methodenaufruf ein, sodass Sie das Programm schrittweise durchgehen können. Drücken Sie **F5**, um die Anwendung auszuführen, und drücken Sie **F11**, um den Code schrittweise zu durchlaufen.

Die Beispielanwendung erstellt einen kostenlosen Azure Search-Dienst für ein vorhandenes Azure-Abonnement. Wenn ein kostenloser Dienst für Ihr Abonnement bereits vorhanden ist, schlägt die Beispielanwendung fehl. Nur ein kostenloser Search-Dienst pro Abonnement ist zulässig.

1. Öffnen Sie im Projektmappen-Explorer „Program.cs“, und wechseln Sie zur Main(string[] void)-Funktion.
2. Beachten Sie, dass **ExecuteArmRequest** verwendet wird, um Anforderungen für den Azure Resource Manager-Endpunkt `https://management.azure.com/subscriptions` für eine angegebene `subscriptionID` auszuführen. Diese Methode wird in der gesamten Anwendung für Operationen mit der Azure Resource Manager-API oder der Search Management-API verwendet.
3. Anforderungen an den Azure Resource Manager müssen authentifiziert und autorisiert werden. Dies geschieht mithilfe der **GetAuthorizationHeader**-Methode, die von der **ExecuteArmRequest**-Methode aufgerufen wird, die aus [Authentifizieren von Anforderungen von Azure Resource Manager](http://msdn.microsoft.com/library/azure/dn790557.aspx) übernommen wurde. Beachten Sie, dass **GetAuthorizationHeader** `https://management.core.windows.net` aufruft, um ein Zugriffstoken zu erhalten.
4. Sie werden aufgefordert, Sie sich mit einem Benutzernamen und Kennwort anzumelden, das für Ihr Abonnement gültig ist.
5. Als Nächstes wird ein neuer Azure Search-Dienst beim Azure Resource Manager-Anbieter registriert. Dies ist wiederum die **ExecuteArmRequest**-Methode. Dieses Mal wird sie verwendet, um den Search-Dienst in Azure über `providers/Microsoft.Search/register` für Ihr Abonnement zu erstellen.
6. Der Rest des Programms verwendet die [Azure Search Management-REST-API](http://msdn.microsoft.com/library/dn832684.aspx). Beachten Sie, dass sich die `api-version` für diese API von der API-Version des Azure-Ressourcen-Managers unterscheidet. Beispielsweise enthält `/listAdminKeys?api-version=2014-07-31-Preview` die `api-version` der Azure Search Management-REST-API.

    Die nächste Folge von Vorgängen ruft die Dienstdefinition ab, die Sie gerade erstellt haben, sowie die Admin-API-Schlüssel, regeneriert und ruft Schlüssel ab, ändert das Replikat und die Partition und löscht schließlich den Dienst.

    Wenn Sie die Anzahl der Dienstreplikate oder Partitionen ändern, wird davon ausgegangen, dass diese Aktion fehlschlägt, wenn Sie die kostenlose Edition verwenden. Sie können nur mit der Standard-Edition zusätzliche Partitionen und Replikate verwenden.

    Das Löschen des Diensts ist der letzte Vorgang.

## <a name="next-steps"></a>Nächste Schritte
Nach Abschluss dieses Lernprogramms empfiehlt es sich, mehr über die Dienstverwaltung oder die Authentifizierung mit dem Active Directory-Dienst zu erfahren:

* Erfahren Sie mehr über die Integration einer Clientanwendung mit Active Directory. Weitere Informationen finden Sie unter [Integrieren von Anwendungen in Azure Active Directory](http://msdn.microsoft.com/library/azure/dn151122.aspx).
* Erfahren Sie mehr zu anderen Dienstverwaltungsvorgängen in Azure. Weitere Informationen finden Sie unter [Verwalten von Diensten](http://msdn.microsoft.com/library/azure/dn578292.aspx).

<!--Anchors-->
[Download the sample application]: #Download
[Configure the application]: #config
[Explore the application]: #explore
[Next Steps]: #next-steps

<!--Image references-->

<!--Link references-->
[Manage your search solution in Microsoft Azure]: search-manage.md
[Azure Search development workflow]: search-workflow.md
[Create your first azure search solution]: search-create-first-solution.md
[Create a geospatial search app using Azure Search]: search-create-geospatial.md

