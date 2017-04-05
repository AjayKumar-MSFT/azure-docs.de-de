---
title: 'Tutorial: Azure Active Directory-Integration mit RightScale | Microsoft Docs'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und RightScale konfigurieren.
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: 3a8d376d-95fb-4dd7-832a-4fdd4dd7c87c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/10/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 07635b0eb4650f0c30898ea1600697dacb33477c
ms.openlocfilehash: adcc4e35f8febe1d0dc1bc093954dee56bf34652
ms.lasthandoff: 03/28/2017


---
# <a name="tutorial-azure-active-directory-integration-with-rightscale"></a>Tutorial: Azure Active Directory-Integration mit RightScale
Dieses Tutorial soll Ihnen zeigen, wie Sie RightScale in Azure Active Directory (Azure AD) integrieren können.

Die Integration von RightScale in Azure AD bietet die folgenden Vorteile:

* Sie können in Azure AD steuern, wer Zugriff auf RightScale hat.
* Sie können Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei RightScale anzumelden (einmaliges Anmelden, SSO).
* Sie können Ihre Konten an einem zentralen Ort verwalten – im klassischen Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen
Um die Azure AD-Integration mit RightScale konfigurieren zu können, benötigen Sie Folgendes:

* Ein Azure AD-Abonnement
* Ein RightScale-Abonnement, für das einmaliges Anmelden (SSO) aktiviert ist

>[!NOTE]
>Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.
>  

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

* Sie sollten keine Produktionsumgebung verwenden, sofern dies nicht erforderlich ist.
* Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
Ziel dieses Tutorials ist es, das einmalige Anmelden (SSO) von Azure AD in einer Testumgebung zu testen. 

Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von RightScale aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung (SSO) von Azure AD

## <a name="add-rightscale-from-the-gallery"></a>Hinzufügen von RightScale aus dem Katalog
Zum Konfigurieren der Integration von RightScale in Azure AD müssen Sie RightScale aus dem Katalog zur Liste der verwalteten SaaS-Apps hinzufügen.

**Um RightScale aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im **klassischen Azure-Portal** im linken Navigationsbereich auf **Active Directory**. 
   
    ![Active Directory][1]

2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
    ![Anwendungen][2]

4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
    ![Anwendungen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
    ![Anwendungen][4]

6. Geben Sie im Suchfeld den Suchbegriff **RightScale**ein.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_01.png)

7. Wählen Sie im Ergebnisbereich **RightScale** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD
In diesem Abschnitt soll anhand eines Testbenutzers namens Britta Simon veranschaulicht werden, wie das einmalige Anmelden von Azure AD in RightScale konfiguriert und getestet werden kann.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in RightScale als Entsprechung zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in RightScale muss eine Linkbeziehung eingerichtet werden.  

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei RightScale müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** – um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines RightScale-Testbenutzers](#creating-a-rightscale-test-user)** , um eine Entsprechung von Britta Simon in RightScale zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** , um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#testing-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD
Das Ziel dieses Abschnitts besteht darin, das einmalige Anmelden (SSO) von Azure AD im klassischen Azure-Portal zu aktivieren und das einmalige Anmelden in Ihrer RightScale-Anwendung zu konfigurieren.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD bei RightScale die folgenden Schritte aus:**

1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **RightScale** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
    ![Einmaliges Anmelden konfigurieren][6] 

2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei RightScale anmelden?** die Option **Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_03.png) 

3. Führen Sie auf der Dialogfeldseite **App-Einstellungen konfigurieren** die folgenden Schritte aus, wenn Sie die Anwendung im **IdP-initiierten Modus** konfigurieren möchten, und klicken Sie dann auf **Weiter**:
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_04.png) 
  1. Geben Sie im Textfeld „Antwort-URL“ die URL nach folgendem Muster ein: `https://login.rightscale.com/login/saml2/consume`
  2. Klicken Sie auf **Weiter**.

1. Wenn die Anwendung im **SP-initiierten Modus** konfiguriert werden soll, klicken Sie auf der Dialogfeldseite **App-Einstellungen konfigurieren** auf **Zeigen Sie die erweiterten Einstellungen an (optional)**, geben Sie die **Anmelde-URL** ein, und klicken Sie dann auf **Weiter**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_06.png) 
  1. Geben Sie im Textfeld „Anmelde-URL“ die URL, die von Ihren Benutzern zur Anmeldung bei der RightScale-Anwendung verwendet wird, nach folgendem Muster ein: `https://login.rightscale.com/`
  2. Klicken Sie auf **Weiter**.

2. Führen Sie auf der Seite **Einmaliges Anmelden konfigurieren für RightScale** die folgenden Schritte aus, und klicken Sie dann auf **Weiter**:
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_05.png) 
  1. Klicken Sie auf **Zertifikat herunterladen**, und speichern Sie das Base64-codierte Zertifikat auf Ihrem Computer.
  2. Klicken Sie auf **Weiter**.

3. Zum Konfigurieren des einmaligen Anmeldens für Ihre Anwendung müssen Sie sich als Administrator bei Ihrem RightScale-Mandanten anmelden.
  1. Klicken Sie im Menü oben auf die Registerkarte **Settings**, und wählen Sie **Single Sign-On**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_001.png) 
  2. Klicken Sie auf die Schaltfläche **new**, um **Your SAML Identity Providers** hinzuzufügen.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_002.png)  
  3. Geben Sie im Textfeld **Display Name**den Namen Ihres Unternehmens ein.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_003.png) 
  4. Wählen Sie in unten stehendem Textfeld **Allow RightScale-initiated SSO using a discovery hint** aus, und geben Sie Ihren **Domänennamen** in das unten stehende Textfeld ein.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_004.png)
  5. Kopieren Sie die SAML-SSO-URL von Azure AD in das Feld **SAML SSO Endpoint** in RightScale.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_005.png)
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_006.png)
  6. Kopieren Sie die Entitäts-ID von Azure AD in das Feld **SAML EntityID** in RightScale.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_007.png)
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_008.png)
  7. Klicken Sie auf die Schaltfläche **Browser** , um das Zertifikat hochzuladen, das Sie in Schritt 4 heruntergeladen haben.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_009.png)
  8. Klicken Sie auf **Speichern**.

4. Wählen Sie im klassischen Portal die Bestätigung zur Konfiguration der einmaligen Anmeldung aus, und klicken Sie dann auf **Weiter**.
   
    ![Azure AD – einmaliges Anmelden][10]

5. Klicken Sie auf der Seite **Bestätigung zur einmaligen Anmeldung** auf **Fertig stellen**.  
   
    ![Azure AD – einmaliges Anmelden][11]

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im klassischen Azure-Portal.

![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im **klassischen Azure-Portal** im linken Navigationsbereich auf **Active Directory**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-rightscale-tutorial/create_aaduser_09.png) 

2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Anzeigen der Liste der Benutzer im Menü oben auf **Benutzer**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-rightscale-tutorial/create_aaduser_03.png) 

4. Um das Dialogfeld **Benutzer hinzufügen** zu öffnen, klicken Sie auf der Symbolleiste unten auf **Benutzer hinzufügen**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-rightscale-tutorial/create_aaduser_04.png) 

5. Führen Sie auf der Dialogfeldseite **Informationen über diesen Benutzer** die folgenden Schritte aus:
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-rightscale-tutorial/create_aaduser_05.png) 
  1. Wählen Sie als **Benutzertyp** die Option **Neuer Benutzer in Ihrer Organisation** aus.
  2. Geben Sie in das Textfeld **Benutzername** den Namen **BrittaSimon** ein.
  3. Klicken Sie auf **Weiter**.

6. Führen Sie auf der Dialogfeldseite **Benutzerprofil** die folgenden Schritte aus:
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-rightscale-tutorial/create_aaduser_06.png) 
  1. Geben Sie in das Textfeld **Vorname** den Namen **Britta** ein.  
  2. Geben Sie in das Textfeld **Nachname** den Namen **Simon** ein.
  3. Geben Sie in das Textfeld **Anzeigename** den Namen **Britta Simon** ein.
  4. Wählen Sie in der Liste **Rolle** die Option **Benutzer** aus.
  5. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** auf **Erstellen**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-rightscale-tutorial/create_aaduser_07.png) 

8. Führen Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** die folgenden Schritte aus:
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-rightscale-tutorial/create_aaduser_08.png)  
  1. Notieren Sie den Wert von **Neues Kennwort**.
  2. Klicken Sie auf **Fertig stellen**.   

### <a name="create-a-rightscale-test-user"></a>Erstellen eines RightScale-Testbenutzers
In diesem Abschnitt erstellen Sie in RightScale einen Benutzer mit dem Namen Britta Simon. Arbeiten Sie über support@rightscale.com mit dem RightScale-Supportteam zusammen, um Benutzer in der RightScale-Plattform hinzuzufügen.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers
Das Ziel dieses Abschnitts besteht darin, Britta Simon die Verwendung des einmaligen Anmeldens (SSO) von Azure zu ermöglichen, indem sie Zugriff auf RightScale erhält.

![Benutzer zuweisen][200] 

**Um Britta Simon zu RightScale zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie zum Öffnen der Anwendungsansicht im klassischen Portal in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste den Eintrag **RightScale**aus.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_50.png) 

3. Klicken Sie im oberen Menü auf **Benutzer**.
   
    ![Benutzer zuweisen][203] 

4. Wählen Sie in der Benutzerliste **Britta Simon**aus.

5. Klicken Sie auf der Symbolleiste unten auf **Zuweisen**.
   
    ![Benutzer zuweisen][205]

### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens
Das Ziel dieses Abschnitts ist das Testen Ihrer Azure AD-Konfiguration für einmaliges Anmelden (SSO) über den Zugriffsbereich.  

Wenn Sie im Zugriffsbereich auf die Kachel "RightScale" klicken, sollten Sie automatisch in Ihrer RightScale-Anwendung angemeldet werden.

## <a name="additional-resources"></a>Weitere Ressourcen
* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_205.png

