---
title: 'Tutorial: Azure Active Directory-Integration mit Kronos | Microsoft Docs'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Kronos konfigurieren.
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: e28d6191-c375-43c6-b2df-22daa88d9939
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/10/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 07635b0eb4650f0c30898ea1600697dacb33477c
ms.openlocfilehash: 7563c1a70f6f956b28a6d2b78f7c0d373620c0cd
ms.lasthandoff: 03/28/2017


---
# <a name="tutorial-azure-active-directory-integration-with-kronos"></a>Tutorial: Azure Active Directory-Integration mit Kronos
In diesem Tutorial erfahren Sie, wie Sie Kronos in Azure Active Directory (Azure AD) integrieren.

Die Integration von Kronos in Azure AD bietet die folgenden Vorteile:

* Sie können in Azure AD steuern, wer Zugriff auf Kronos hat.
* Sie können Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Kronos anzumelden (einmaliges Anmelden, SSO).
* Sie können Ihre Konten an einem zentralen Ort verwalten – im klassischen Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen
Um die Azure AD-Integration mit Kronos konfigurieren zu können, benötigen Sie Folgendes:

* Ein Azure AD-Abonnement
* Ein **Kronos Workforce Central**-Abonnement, für das einmaliges Anmelden aktiviert ist

>[!NOTE]
>Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.
>  

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

* Sie sollten keine Produktionsumgebung verwenden, sofern dies nicht erforderlich ist.
* Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden (SSO) von Azure AD in einer Testumgebung. 

Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von Kronos aus dem Katalog
2. Konfigurieren und Testen des einmaligen Anmeldens (SSO) von Azure AD

## <a name="add-kronos-from-the-gallery"></a>Hinzufügen von Kronos aus dem Katalog
Zum Konfigurieren der Integration von Kronos in Azure AD müssen Sie Kronos aus dem Katalog zur Liste der verwalteten SaaS-Apps hinzufügen.

**Um Kronos aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**. 
   
    ![Active Directory][1]

2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
    ![Anwendungen][2]

4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
    ![Anwendungen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
    ![Anwendungen][4]

6. Geben Sie im Suchfeld als Suchbegriff **Kronos**ein.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-kronos-tutorial/tutorial_kronos_01.png)

7. Wählen Sie im Ergebnisbereich **Kronos** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-kronos-tutorial/tutorial_kronos_06.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden (SSO) von Azure AD mit Kronos basierend auf einem Testbenutzer mit dem Namen Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Kronos als Entsprechung zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Kronos muss eine Linkbeziehung eingerichtet werden.

Diese Linkbeziehung wird hergestellt, indem Sie den Wert des **Benutzernamens** in Azure AD als Wert des **Benutzernamens** in Kronos zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD bei Kronos müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** – um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Kronos-Testbenutzers](#creating-an-kronos-test-user)** , um ein Gegenstück zu Britta Simon in Kronos zu erhalten, das mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** , um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#testing-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD
In diesem Abschnitt aktivieren Sie das einmalige Anmelden (SSO) von Azure AD im klassischen Portal und konfigurieren das einmalige Anmelden in Ihrer Kronos-Anwendung.

Ihre Kronos-Anwendung erwartet die SAML-Assertions in einem bestimmten Format. Wenden Sie sich zunächst an das Kronos-Team, um die richtige Benutzer-ID zu ermitteln, die in der Anwendung zugeordnet wird. 

Halten Sie sich zudem in Bezug auf das Attribut, das für diese Zuordnung verwendet werden soll, an die Anweisungen des Kronos-Teams. Microsoft empfiehlt die Verwendung des Attributs **NameIdentifier** als Benutzer-ID. Sie können den Wert dieses Attributs auf der Registerkarte **Attribut** der Anwendung verwalten. 

Der folgende Screenshot zeigt ein Beispiel für diese Attributzuordnungen: Hier haben wir dem Anspruch „nameidentifier“ das Attribut **userprincipalname** zusammen mit der Funktion **ExtractMailPrefix** zugeordnet, das eine eindeutige Benutzer-ID bereitstellt. Diese wird in jeder erfolgreichen SAML-Antwort an die Kronos-Anwendung gesendet.

![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-kronos-tutorial/tutorial_kronos_07.png) 

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD bei Kronos die folgenden Schritte aus:**

1. Klicken Sie im klassischen Portal auf der Anwendungsintegrationsseite für **Kronos** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
     ![Einmaliges Anmelden konfigurieren][6] 

2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei Kronos anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-kronos-tutorial/tutorial_kronos_03.png) 

3. Führen Sie auf der Dialogseite **App-Einstellungen konfigurieren** die folgenden Schritte aus:
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-kronos-tutorial/tutorial_kronos_04.png) 
  1. Geben Sie im Textfeld „Bezeichner“ die URL, die von Ihren Benutzern zur Anmeldung bei der Kronos-Anwendung genutzt wird, nach folgendem Muster ein: `https://<company name>.kronos.net/`
  2. Geben Sie im Textfeld „Antwort-URL“ die URL nach folgendem Muster ein: `https://<company name>.kronos.net/wfc/navigator/logonWithUID`

1. Führen Sie auf der Seite **Einmaliges Anmelden konfigurieren für Kronos** die folgenden Schritte aus:
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-kronos-tutorial/tutorial_kronos_05.png) 
  1. Klicken Sie auf **Metadaten herunterladen**und speichern Sie die Datei auf Ihrem Computer. 
  2. Klicken Sie auf **Weiter**.

2. Kontaktieren Sie Ihren Kronos-Account Manager, um Informationen zum richtigen Kanal für die SSO-Konfiguration für Ihre Anwendung zu erhalten. Bitte beachten Sie, dass Sie eine E-Mail senden und die heruntergeladene Metadatendatei anfügen müssen.

3. Wählen Sie im klassischen Portal die Bestätigung zur Konfiguration der einmaligen Anmeldung aus, und klicken Sie dann auf **Weiter**.
   
    ![Azure AD – einmaliges Anmelden][10]

4. Klicken Sie auf der Seite **Bestätigung zur einmaligen Anmeldung** auf **Fertig stellen**.  
   
    ![Azure AD – einmaliges Anmelden][11]

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
In diesem Abschnitt erstellen Sie im klassischen Portal einen Testbenutzer mit dem Namen Britta Simon.

![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-kronos-tutorial/create_aaduser_09.png) 

2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Anzeigen der Liste der Benutzer im Menü oben auf **Benutzer**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-kronos-tutorial/create_aaduser_03.png) 

4. Um das Dialogfeld **Benutzer hinzufügen** zu öffnen, klicken Sie auf der Symbolleiste unten auf **Benutzer hinzufügen**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-kronos-tutorial/create_aaduser_04.png) 

5. Führen Sie auf der Dialogfeldseite **Informationen über diesen Benutzer** die folgenden Schritte aus:
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-kronos-tutorial/create_aaduser_05.png) 
  1. Wählen Sie als „Benutzertyp“ die Option „Neuer Benutzer in Ihrer Organisation“ aus. 
  2. Geben Sie in das Textfeld **Benutzername** den Namen **BrittaSimon** ein.
  3. Klicken Sie auf **Weiter**.

6. Führen Sie auf der Dialogfeldseite **Benutzerprofil** die folgenden Schritte aus:
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-kronos-tutorial/create_aaduser_06.png) 
  1. Geben Sie in das Textfeld **Vorname** den Namen **Britta** ein.  
  2. Geben Sie in das Textfeld **Nachname** den Namen **Simon** ein. 
  3. Geben Sie in das Textfeld **Anzeigename** den Namen **Britta Simon** ein.
  4. Wählen Sie in der Liste **Rolle** die Option **Benutzer** aus.
  5. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** auf **Erstellen**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-kronos-tutorial/create_aaduser_07.png) 

8. Führen Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** die folgenden Schritte aus:
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-kronos-tutorial/create_aaduser_08.png) 
  1. Notieren Sie den Wert von **Neues Kennwort**.
  2. Klicken Sie auf **Fertig stellen**.   

### <a name="create-an-kronos-test-user"></a>Erstellen eines Kronos-Testbenutzers
In diesem Abschnitt erstellen Sie in Kronos einen Benutzer mit dem Namen Britta Simon. In der Kronos-Anwendung müssen alle Benutzer bereitgestellt werden, bevor das einmalige Anmelden verwendet werden kann. 

Wenden Sie sich an den Kronos-Kundensupport, um sämtliche Benutzer in der Anwendung bereitzustellen. 

>[!NOTE]
>Wenn Sie einen Benutzer manuell oder eine Gruppe von Benutzern erstellen müssen, wenden Sie sich an das Kronos-Supportteam. 
> 

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers
In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Kronos gewähren.

![Benutzer zuweisen][200] 

**Um Britta Simon zu Kronos zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie zum Öffnen der Anwendungsansicht im klassischen Portal im oberen Menü der Verzeichnisansicht auf **Anwendungen** .
   
    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste den Eintrag **Kronos**aus.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-kronos-tutorial/tutorial_kronos_50.png) 

3. Klicken Sie im oberen Menü auf **Benutzer**.
   
    ![Benutzer zuweisen][203] 

4. Wählen Sie in der Benutzerliste **Britta Simon**aus.

5. Klicken Sie auf der Symbolleiste unten auf **Zuweisen**.
   
    ![Benutzer zuweisen][205]

### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens
In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden (SSO) über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Kronos“ klicken, sollten Sie automatisch bei Ihrer Kronos-Anwendung angemeldet werden.

## <a name="additional-resources"></a>Zusätzliche Ressourcen
* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_205.png

