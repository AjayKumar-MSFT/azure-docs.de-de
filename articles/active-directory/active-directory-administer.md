---
title: Verwalten Ihres Azure AD-Verzeichnisses | Microsoft Docs
description: "Hier wird erklärt, was ein Azure AD-Mandant ist und wie Azure über Azure Active Directory verwaltet wird."
services: active-directory
documentationcenter: 
author: curtand
writer: markvi
manager: femila
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/06/2017
ms.author: curtand
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 001ffc0f9c7465552392a9848ef1487a4c0eafce
ms.lasthandoff: 12/08/2016


---
# <a name="administer-your-azure-ad-directory"></a>Verwalten Ihres Azure AD-Verzeichnisses
## <a name="what-is-an-azure-ad-tenant"></a>Was ist Azure AD-Mandant?
In der physischen Arbeitswelt kann der Wort-Mandant als Gruppe oder Firma definiert werden, die in einem Gebäude angesiedelt sind. Ihre Organisation kann z. B. Büroräume in einem Gebäude besitzen. Dieses Gebäude kann sich auf einer Straße mit mehreren anderen Organisationen befinden. Ihr Unternehmen wird somit als Mandant dieses Gebäudes betrachtet. Dieses Gebäude ist ein Anlagegut Ihrer Organisation, bietet Sicherheit und sorgt dafür, dass Sie Ihre Geschäfte zuverlässig durchführen können. Es ist auch von anderen Unternehmen in Ihrer Straße getrennt. Dadurch wird sichergestellt, dass Ihre Organisation und die darin befindlichen Bestände von anderen Organisationen isoliert sind.

An einem cloudaktivierten Arbeitsplatz kann ein Mandant als ein Client oder eine Organisation definiert werden, die eine bestimmte Instanz dieses Clouddienstes besitzt und verwaltet. Mit der von Microsoft Azure bereitgestellten Identitätsplattform ist ein Mandant einfach eine dedizierte Instanz von Azure Active Directory (Azure AD), die Ihre Organisation erhält und besitzt, wenn sie sich für einen Microsoft-Clouddienst wie Azure oder Office 365 registriert.

Jedes Azure AD-Verzeichnis ist eindeutig und von anderen Azure AD-Verzeichnissen getrennt. Genau so wie ein Bürogebäude, das ein sicherer Ort nur für Ihre Organisation ist, soll auch ein Azure AD-Verzeichnis ein sicheres Anlagegut nur für Ihre Organisation darstellen. Die Azure AD-Architektur isoliert Kundendaten und Identitätsinformationen und verhindert deren Vermischung. Dies bedeutet, dass Benutzer und Administratoren eines Azure AD-Verzeichnisses nicht versehentlich oder böswillig auf Daten in einem anderen Verzeichnis zugreifen können.

![Verwalten von Azure Active Directory][1]

## <a name="how-can-i-get-an-azure-ad-directory"></a>Wie bekomme ich ein Azure AD-Verzeichnis?
Azure AD bietet die wichtigsten Unternehmensverzeichnis- und Verwaltungsfunktionen für die meisten Clouddienste von Microsoft, einschließlich:

* Azure
* Microsoft Office 365
* Microsoft Dynamics CRM Online
* Microsoft Intune

Sie erhalten ein Azure AD-Verzeichnis, wenn Sie sich für einen dieser Clouddienste von Microsoft registrieren. Sie können je nach Bedarf weitere Verzeichnisse erstellen. Beispielsweise können Sie Ihr erstes Verzeichnis als Produktionsverzeichnis verwalten und dann ein anderes Verzeichnis für Testzwecke oder Staging erstellen.

> [!NOTE]
> Nach der Anmeldung für den ersten Dienst wird empfohlen, das gleiche Ihrer Organisation zugeordnete Administratorkonto zu verwenden, wenn Sie sich für andere Microsoft-Clouddienste registrieren.
> 
> 

Bei der ersten Registrierung für einen Microsoft-Clouddienst werden Sie aufgefordert, Details zu Ihrer Organisation und der Internet-Domänennamenregistrierung Ihrer Organisation bereitzustellen. Diese Informationen werden dann zum Erstellen einer neuen Azure AD-Verzeichnisinstanz für Ihre Organisation verwendet. Das gleiche Verzeichnis wird zum Authentifizieren von Anmeldeversuchen verwendet, wenn Sie mehrere Clouddienste von Microsoft abonnieren.

Die zusätzlichen Dienste nutzen vollständig alle vorhandenen Benutzerkonten, Richtlinien, Einstellungen oder die lokale Verzeichnisintegration, die Sie zur Verbesserung der Effizienz zwischen der lokalen Identitätsinfrastruktur Ihrer Organisation und Azure AD konfigurieren.

Wenn Sie sich beispielsweise ursprünglich für ein Microsoft Intune-Abonnement registriert und die Schritte ausgeführt haben, die zur weiter gehenden Integration Ihrer lokalen Active Directory-Instanz in Azure AD Directory erforderlich sind (Bereitstellung der Verzeichnissynchronisierung bzw. von Servern für einmaliges Anmelden), können Sie sich für einen anderen Microsoft-Clouddienst registrieren, z.B. Office 365. So kommen Sie in den Genuss der gleichen Vorteile der Verzeichnisintegration, von denen Sie jetzt bereits mit Microsoft Intune profitieren.

Weitere Informationen zum Integrieren Ihres lokalen Verzeichnisses in Azure AD finden Sie unter [Verzeichnisintegration](active-directory-aadconnect.md).

### <a name="associate-an-azure-ad-directory-with-a-new-azure-subscription"></a>Ordnen Sie ein Azure AD-Verzeichnis einem neuen Azure-Abonnement zu
Sie können ein neues Azure-Abonnement dem gleichen Verzeichnis zuordnen, das die Anmeldung für ein vorhandenes Office 365- oder Microsoft Intune-Abonnement authentifiziert. Melden Sie sich mit Ihrem Geschäfts- oder Schulkonto beim Azure-Verwaltungsportal an. Das Verwaltungsportal gibt eine Meldung zurück, dass keine Abonnements für dieses Konto gefunden werden konnten. Wählen Sie **Bei Azure registrieren**, und Ihr Verzeichnis ist für die Verwaltung innerhalb des Portals verfügbar. Erfahren Sie hier mehr über das [Verwalten des Verzeichnisses für Ihr Office 365-Abonnement in Azure](active-directory-how-subscriptions-associated-directory.md#manage-the-directory-for-your-office-365-subscription-in-azure).

Ein Video über allgemeine Fragen zur Verwendung von Azure AD finden Sie unter [Azure Active Directory – Allgemeines, Registrierung, Anmeldung und Verwendung](http://channel9.msdn.com/Series/Windows-Azure-Active-Directory/WAADCommonSignupsigninquestions).

### <a name="create-an-azure-ad-directory-by-signing-up-for-a-microsoft-cloud-service-as-an-organization"></a>Erstellen Sie ein Azure AD-Verzeichnis, indem Sie sich für einen Microsoft Clouddienst als Organisation registrieren
Wenn Sie noch nicht über ein Abonnement für einen Microsoft-Clouddienst verfügen, gehen Sie auf einen der untenstehenden Links, um sich zu registrieren. Wenn Sie sich für Ihren ersten Dienst registrieren, wird automatisch ein Azure AD-Verzeichnis erstellt.

* [Microsoft Azure](https://account.windowsazure.com/organization)
* [Office 365](http://products.office.com/business/compare-office-365-for-business-plans/)
* [Microsoft Intune](https://account.manage.microsoft.com/Signup/MainSignUp.aspx?OfferId=40BE278A-DFD1-470a-9EF7-9F2596EA7FF9&ali=1)

### <a name="manage-an-azure-provisioned-default-directory"></a>Ein Standardverzeichnis mit Azure-Bereitstellung verwalten
Heute wird ein Verzeichnis automatisch erstellt, wenn Sie sich für Azure registrieren und Ihr Abonnement diesem Verzeichnis zugeordnet ist. Wenn Sie sich ursprünglich vor Oktober 2013 für Azure registriert haben, wurde ein Verzeichnis jedoch nicht automatisch erstellt. In diesem Fall hat Azure Ihr Konto möglicherweise "abgeglichen", indem ein Standardverzeichnis bereitgestellt wurde. Ihr Abonnement wurde dann diesem Standardverzeichnis zugeordnet.

Ein Abgleich der Verzeichnisse wurde im Oktober 2013 als Teil der generellen Verbesserung des Sicherheitsmodells für Azure durchgeführt. Dieser hilft, allen Azure-Kunden Identitätsfunktionen für die Organisation anzubieten und sicherzustellen, dass auf alle Azure-Ressourcen im Kontext eines Benutzers im Verzeichnis zugegriffen wird. Sie können Azure nicht ohne ein Verzeichnis verwenden. Um dies zu erreichen, musste für jeden Benutzer, der vor dem 7. Juli 2013 registriert wurde und über kein Verzeichnis verfügte, eines erstellt werden. Wenn Sie bereits ein Verzeichnis erstellt haben, wurde Ihr Abonnement diesem Verzeichnis zugeordnet.

Die Verwendung von Azure AD ist kostenlos. Das Verzeichnis ist eine kostenlose (free) Ressource. Zusätzlich gibt es Azure Active Directory Premium, das separat lizenziert ist und zusätzliche Funktionen wie Unternehmensbranding und Self-Service-Kennwortzurücksetzung bietet.

Um den Anzeigenamen des Verzeichnisses zu ändern, klicken Sie im Portal auf das Verzeichnis und anschließend auf **Konfigurieren**. Wie weiter unten in diesem Thema beschrieben, können Sie ein neues Verzeichnis hinzufügen oder ein Verzeichnis löschen, das Sie nicht mehr benötigen. Um Ihrem Abonnement ein anderes Verzeichnis zuzuordnen, klicken Sie im linken Navigationsbereich auf die Erweiterung **Einstellungen** und unten auf der Seite **Abonnements** auf **Verzeichnis bearbeiten**. Sie können auch eine benutzerdefinierte Domäne mit einem DNS-Namen erstellen, die Sie anstelle der standardmäßigen *.onmicrosoft.com-Domäne registriert haben. Dies ist mit einem Dienst wie SharePoint Online möglicherweise empfehlenswert.

## <a name="how-can-i-manage-directory-data"></a>Wie kann ich Verzeichnisdaten verwalten?
Als Administrator eines oder mehrerer Clouddienst-Abonnements von Microsoft können Sie entweder das Azure-Verwaltungsportal, das Microsoft Intune-Kontoportal oder das Office 365 Admin Center verwenden, um Verzeichnisdaten Ihrer Organisation zu verwalten. Sie können auch Cmdlets vom [Microsoft Azure Active Directory-Modul für Windows PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx) herunterladen und ausführen, um in Azure AD gespeicherte Daten einfach zu verwalten.

Von einem dieser Portale (oder Cmdlets) können Sie folgende Aktionen ausführen:

* Erstellen und Verwalten von Benutzer- und Gruppenkonten
* Verwalten des entsprechenden Clouddienstes oder der Clouddienste, die Ihre Organisation abonniert
* Einrichten einer lokale Integration mit Ihrem Verzeichnisdienst

Das Azure-Verwaltungsportal, das Office 365 Admin Center, das Microsoft Intune-Kontoportal und Azure AD-Cmdlets lesen aus und schreiben in eine einzelne freigegebene (shared) Instanz von Azure AD, die dem Verzeichnis Ihrer Organisation zugeordnet ist, wie in der folgenden Abbildung dargestellt. Auf diese Weise fungieren Portale (oder Cmdlets) als Front-End-Schnittstelle, die Ihre Verzeichnisdaten einholen und/oder ändern.

![][2]

Diese Kontoportale und die zugehörigen Azure AD PowerShell-Cmdlets zur Verwaltung von Benutzern und Gruppen setzen auf der Azure AD-Plattform auf.

Beim Vornehmen von Änderungen an den Daten Ihrer Organisation mithilfe der Portale (oder Cmdlets) während der Anmeldung im Kontext mit einem dieser Dienste wird die Änderung bei der nächsten Anmeldung im Kontext mit diesem Dienst auch in den anderen Portalen angezeigt, da die Daten von allen abonnierten Microsoft-Clouddiensten gemeinsam verwendet werden (shared data).
Wenn Sie z. B. das Office 365 Admin Center benutzt haben, um einen Benutzer von der Anmeldung zu blockieren, dann blockiert diese Aktion die Anmeldung bei jedem Dienst, den Ihre Organisation zurzeit abonniert hat. Wenn Sie das gleiche Benutzerkonto im Kontext des Microsoft Intune-Kontoportals ziehen, können Sie sehen, dass der Benutzer gesperrt ist.

## <a name="how-can-i-add-and-manage-multiple-directories"></a>Wie kann ich mehrere Verzeichnisse hinzufügen und verwalten?
Sie können ein Azure AD-Verzeichnis im Azure-Verwaltungsportal hinzufügen. Wählen Sie die Erweiterung **Active Directory** auf der linken Seite, und klicken Sie auf **Hinzufügen**.

Sie können jedes Verzeichnis als vollständig unabhängige Ressource verwalten: Jedes Verzeichnis ist gleichberechtigt, voll funktionsfähig und logisch unabhängig von anderen Verzeichnissen, die Sie verwalten. Es ist keine unter- und übergeordnete Beziehung zwischen den Verzeichnissen vorhanden. Diese Unabhängigkeit zwischen den Verzeichnissen beinhaltet Ressourcen-, Verwaltungs- und Synchronisierungsunabhängigkeit.

* **Ressourcenunabhängigkeit**. Wenn Sie eine Ressource in einem Verzeichnis erstellen oder löschen, hat dies keine Auswirkungen auf Ressourcen in einem anderen Verzeichnis. Eine teilweise geltende Ausnahme bilden externe Benutzer, wie unten beschrieben. Wenn Sie eine benutzerdefinierte Domäne "contoso.com" in einem Verzeichnis verwenden, kann sie in keinem anderen Verzeichnis verwendet werden.
* **Verwaltungsunabhängigkeit**.  Wenn ein Benutzer ohne Administratorrechte aus dem Verzeichnis "Contoso" ein Testverzeichnis "Test" erstellt, dann gibt es/geschieht Folgendes:
  
  * Verzeichnissynchronisierungstool zum Synchronisieren von Daten mit einer AD-Gesamtstruktur.
  * Die Administratoren für das Verzeichnis „Contoso“ haben keine direkten Administratorberechtigungen für das Verzeichnis „Test“, sofern ihnen nicht ein Administrator dieses Verzeichnisses die Berechtigungen explizit erteilt. Die Administratoren von "Contoso" steuern den Zugriff auf das Verzeichnis "Test", da sie das Benutzerkonto steuern, mit dem dieses Verzeichnis erstellt wurde.
    
    Und wenn Sie eine Administratorrolle für einen Benutzer in einem Verzeichnis ändern (hinzufügen oder entfernen), hat die Änderung keine Auswirkungen auf Administratorrollen, die der Benutzer möglicherweise in einem anderen Verzeichnis besitzt.
* **Synchronisierungsunabhängigkeit**. Sie können jedes Azure AD unabhängig voneinander konfigurieren, damit Sie synchronisierte Daten einer einzelnen Instanz erhalten, mit folgenden Möglichkeiten:
  
  * Dem Verzeichnissynchronisierungstool zum Synchronisieren von Daten mit einer AD-Gesamtstruktur.
  * Dem Azure Active Directory-Connector für Forefront Identity Manager zum Synchronisieren von Daten mit einer oder mehreren lokalen Gesamtstrukturen und/oder nicht-AD-Datenquellen.

Beachten Sie außerdem, dass Ihre Verzeichnisse im Gegensatz zu anderen Azure-Ressourcen keine untergeordneten Ressourcen eines Azure-Abonnements sind. Wenn Sie also kündigen oder Ihr Azure-Abonnement ablaufen lassen, können Sie weiterhin auf Ihre Verzeichnisdaten mithilfe von Azure AD PowerShell, der Azure Graph-API oder anderen Schnittstellen wie Office 365 Admin Center zugreifen. Sie können dem Verzeichnis auch ein anderes Abonnement zuordnen.

## <a name="how-can-i-delete-an-azure-ad-directory"></a>Wie lösche ich ein Azure AD-Verzeichnis?
Ein globaler Administrator kann ein Azure AD-Verzeichnis über das Portal löschen. Wenn ein Verzeichnis gelöscht wird, werden alle im Verzeichnis enthaltenen Ressourcen ebenfalls gelöscht. Sie sollten also vor dem Löschen sicher sein, dass Sie das Verzeichnis nicht mehr benötigen.

> [!NOTE]
> Wenn der Benutzer mit einem Geschäfts- oder Schulkonto angemeldet ist, darf er das Basisverzeichnis nicht löschen. Wenn der Benutzer z.B. als joe@contoso.onmicrosoft.com, angemeldet ist, kann dieser Benutzer nicht das Verzeichnis mit der Standarddomäne „contoso.onmicrosoft.com“ löschen.
> 
> 

### <a name="conditions-that-must-be-met-to-delete-an-azure-ad-directory"></a>Bedingungen, für das Löschen eines Azure AD-Verzeichnisses erfüllt sein müssen
Bei Azure AD müssen bestimmte Bedingungen erfüllt sein, um ein Verzeichnis zu löschen. Dies reduziert das Risiko, dass das Löschen eines Verzeichnisses Benutzer oder Anwendungen beeinträchtigt, wie z. B. die Möglichkeit von Benutzern, sich bei Office 365 anzumelden oder auf Ressourcen in Azure zuzugreifen. Wenn ein Verzeichnis für ein Abonnement beispielsweise versehentlich gelöscht wurde, konnten Benutzer nicht auf die Azure-Ressourcen dieses Abonnements zugreifen.

Die folgenden Bedingungen werden überprüft:

* Der einzige Benutzer im Verzeichnis ist der globale Administrator, der das Verzeichnis löschen wird. Andere Benutzer müssen gelöscht werden, bevor das Verzeichnis gelöscht werden kann. Wenn Benutzer lokal synchronisiert werden, dann muss die Synchronisation deaktiviert werden und die Benutzer müssen über das Verwaltungsportal oder das Azure-Modul für Windows PowerShell im Cloudverzeichnis gelöscht werden. Es gibt keine Anforderung zum Löschen von Gruppen oder Kontakten, z. B. aus Office 365 Admin Center hinzugefügten Kontakten.
* Es können keine Anwendungen im Verzeichnis vorhanden sein. Alle Anwendungen müssen gelöscht werden, bevor das Verzeichnis gelöscht werden kann.
* Dem Verzeichnis können keine Abonnements für Microsoft Online Services, z.B. Microsoft Azure, Office 365 oder Azure AD Premium, zugeordnet werden. Wenn für Sie in Azure beispielsweise ein Standardverzeichnis erstellt wurde, können Sie dieses Verzeichnis nicht löschen, wenn es vom Azure-Abonnement noch für die Authentifizierung benötigt wird. Sie können ein Verzeichnis auch nicht löschen, wenn ein anderer Benutzer dem Verzeichnis ein Abonnement zugeordnet hat. Um Ihrem Abonnement ein anderes Verzeichnis zuzuordnen, melden Sie sich am Azure-Verwaltungsportal an und klicken links im Navigationsbereich auf **Einstellungen**. Klicken Sie dann unten auf der Seite **Abonnements** auf **Verzeichnis bearbeiten**. Weitere Informationen zu Azure-Abonnements finden Sie unter [Verknüpfung von Azure-Abonnements mit Azure AD](active-directory-how-subscriptions-associated-directory.md).

> [!NOTE]
> Wenn der Benutzer mit einem Geschäfts- oder Schulkonto angemeldet ist, darf er das Basisverzeichnis nicht löschen. Wenn der Benutzer z.B. als joe@contoso.onmicrosoft.com, angemeldet ist, kann dieser Benutzer nicht das Verzeichnis mit der Standarddomäne „contoso.onmicrosoft.com“ löschen.
> 
> 

* Kein Multi-Factor Authentication-Anbieter kann mit dem Verzeichnis verknüpft werden.

## <a name="additional-resources"></a>Zusätzliche Ressourcen
* [Azure AD-Forum](https://social.msdn.microsoft.com/Forums/home?forum=WindowsAzureAD)
* [Azure Multi-Factor Authentication-Forum](https://social.msdn.microsoft.com/Forums/home?forum=windowsazureactiveauthentication)
* [Stackoverflow](http://stackoverflow.com/questions/tagged/azure)
* [Als Unternehmen für Azure registrieren](sign-up-organization.md)
* [Verwalten von Azure AD mithilfe von Windows PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx)
* [Zuweisen von Administratorrollen in Azure AD](active-directory-assign-admin-roles.md)

<!--Image references-->
[1]: ./media/active-directory-administer/aad_portals.png
[2]: ./media/active-directory-administer/azure_tenants.png

