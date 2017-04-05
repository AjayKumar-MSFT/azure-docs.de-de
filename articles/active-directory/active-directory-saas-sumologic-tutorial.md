---
title: 'Tutorial: Azure Active Directory-Integration mit SumoLogic | Microsoft Docs'
description: "Erfahren Sie, wie Sie SumoLogic mit Azure Active Directory verwenden können, um einmaliges Anmelden, automatisierte Bereitstellung und vieles mehr zu ermöglichen."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: fbb76765-92d7-4801-9833-573b11b4d910
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 3/07/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 07635b0eb4650f0c30898ea1600697dacb33477c
ms.openlocfilehash: 3fbf55ddc13e6489e81eb04c962e833600bfc846
ms.lasthandoff: 03/28/2017


---
# <a name="tutorial-azure-active-directory-integration-with-sumologic"></a>Tutorial: Azure Active Directory-Integration mit SumoLogic
In diesem Tutorial wird die Integration von Azure und SumoLogic erläutert.  

Das in diesem Lernprogramm verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

* Ein gültiges Azure-Abonnement
* Ein SumoLogic-Mandant

Nach Abschluss dieses Tutorials können sich die SumoLogic zugewiesenen Azure AD-Benutzer mittels einmaliger Anmeldung auf Ihrer SumoLogic-Unternehmenswebsite bei der Anwendung anmelden (durch den Dienstanbieter initiierte Anmeldung). Alternativ können sie den Zugriffsbereich nutzen (siehe [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md)).

Das in diesem Lernprogramm beschriebene Szenario besteht aus den folgenden Bausteinen:

1. Aktivieren der Anwendungsintegration für SumoLogic
2. Konfigurieren des einmaligen Anmeldens (SSO)
3. Konfigurieren der Benutzerbereitstellung
4. Zuweisen von Benutzern

![Szenario](./media/active-directory-saas-sumologic-tutorial/IC778549.png "Szenario")

## <a name="enable-the-application-integration-for-sumologic"></a>Aktivieren der Anwendungsintegration für SumoLogic
In diesem Abschnitt wird beschrieben, wie Sie die Anwendungsintegration für SumoLogic aktivieren.

**So aktivieren Sie die Anwendungsintegration für SumoLogic**

1. Klicken Sie im klassischen Azure-Portal im linken Navigationsbereich auf **Active Directory**.
   
    ![Active Directory](./media/active-directory-saas-sumologic-tutorial/IC700993.png "Active Directory")

2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
    ![Anwendungen](./media/active-directory-saas-sumologic-tutorial/IC700994.png "Anwendungen")

4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
    ![Anwendung hinzufügen](./media/active-directory-saas-sumologic-tutorial/IC749321.png "Anwendung hinzufügen")

5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
    ![Anwendung aus dem Katalog hinzufügen](./media/active-directory-saas-sumologic-tutorial/IC749322.png "Anwendung aus dem Katalog hinzufügen")

6. Geben Sie in das **Suchfeld** den Suchbegriff **sumologic** ein.
   
    ![Anwendungskatalog](./media/active-directory-saas-sumologic-tutorial/IC778550.png "Anwendungskatalog")

7. Wählen Sie im Ergebnisbereich **SumoLogic** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
    ![SumoLogic](./media/active-directory-saas-sumologic-tutorial/IC778551.png "SumoLogic")

## <a name="configure-single-sign-on"></a>Einmaliges Anmelden konfigurieren
In diesem Abschnitt wird erläutert, wie Sie es Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Azure AD-Konto bei SumoLogic zu authentifizieren.  

Im Rahmen dieses Verfahrens müssen Sie eine Base64-codierte Zertifikatsdatei in Ihren SumoLogic-Mandanten hochladen.  

Falls Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [How to convert a binary certificate into a text file](http://youtu.be/PlgrzUZ-Y1o)

**So konfigurieren Sie einmaliges Anmelden**

1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **SumoLogic** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sumologic-tutorial/IC778552.png "Einmaliges Anmelden konfigurieren")

2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei SumoLogic anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sumologic-tutorial/IC778553.png "Einmaliges Anmelden konfigurieren")

3. Geben Sie auf der Seite **App-URL konfigurieren** im Textfeld für die **SumoLogic-Anmelde-URL** die URL im Format „*https://\<Mandantenname\>.SumoLogic.com*“ ein, und klicken Sie dann auf **Weiter**.
   
    ![AOO-URL konfigurieren](./media/active-directory-saas-sumologic-tutorial/IC778554.png "AOO-URL konfigurieren")

4. Klicken Sie zum Herunterladen des Zertifikats auf der Seite **Einmaliges Anmelden konfigurieren für SumoLogic** auf **Zertifikat herunterladen**, und speichern Sie das Zertifikat auf Ihrem Computer.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sumologic-tutorial/IC778555.png "Einmaliges Anmelden konfigurieren")

5. Melden Sie sich in einem anderen Webbrowserfenster auf der SumoLogic-Unternehmenswebsite als Administrator an.

6. Navigieren Sie zu **Verwalten \> Sicherheit**.
   
    ![Verwalten](./media/active-directory-saas-sumologic-tutorial/IC778556.png "Verwalten")

7. Klicken Sie auf **SAML**.
   
    ![Globale Sicherheitseinstellungen](./media/active-directory-saas-sumologic-tutorial/IC778557.png "Globale Sicherheitseinstellungen")

8. Wählen Sie aus der Liste **Eine Konfiguration auswählen oder eine neue erstellen** die Option **Azure AD** aus, und klicken Sie dann auf **Konfigurieren**.
   
    ![SAML 2.0 konfigurieren](./media/active-directory-saas-sumologic-tutorial/IC778558.png "SAML 2.0 konfigurieren")

9. Führen Sie auf der Seite **SAML 2.0 konfigurieren** die folgenden Schritte aus:
   
    ![SAML 2.0 konfigurieren](./media/active-directory-saas-sumologic-tutorial/IC778559.png "SAML 2.0 konfigurieren")   
  1. Geben Sie in das Textfeld **Konfigurationsnamen** die Option **Azure AD** ein. 
  2. Wählen Sie **Debug-Modus**aus.
  3. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für SumoLogic** den Wert der **Aussteller-URL**, und fügen Sie ihn in das Textfeld **Aussteller** ein.
  4. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für SumoLogic** den Wert der **Authentifizierungsanforderungs-URL**, und fügen Sie ihn in das Textfeld **Authentifizierungsanforderungs-URL** ein.
  5. Erstellen Sie eine **Base-64-codierte** Datei aus dem heruntergeladenen Zertifikat.  
      
     >[!TIP]
     >Weitere Informationen finden Sie unter [How to convert a binary certificate into a text file](http://youtu.be/PlgrzUZ-Y1o)(in englischer Sprache).
     >  

  6. Öffnen Sie das Base-64-codierte Zertifikat im Editor, kopieren Sie den Inhalt des Zertifikats in die Zwischenablage, und fügen Sie anschließend das gesamte Zertifikat in das Textfeld **X.509-Zertifikat** ein.
  7. Wählen Sie als **E-Mail-Attribut** die Option **SAML-Betreff verwenden**.  
  8. Wählen Sie **SP-initiierte Anmeldungskonfiguration**.
  9. Geben Sie im Textfeld **Anmeldepfad** den Wert **Azure** ein, und klicken Sie auf **Speichern**.

10. Wählen Sie im klassischen Azure-Portal auf der Dialogseite **Einmaliges Anmelden konfigurieren für SumoLogic** die Bestätigung zur Konfiguration des einmaligen Anmeldens aus, und klicken Sie dann auf **Abschließen**.
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sumologic-tutorial/IC778560.png "Einmaliges Anmelden konfigurieren")

## <a name="configure-user-provisioning"></a>Benutzerbereitstellung konfigurieren
Damit sich Azure AD-Benutzer bei SumoLogic anmelden können, müssen sie in SumoLogic bereitgestellt werden.  

* Im Fall von SumoLogic ist die Bereitstellung eine manuelle Aufgabe.

**Führen Sie zum Bereitstellen von Benutzerkonten die folgenden Schritte aus:**

1. Melden Sie sich bei Ihrem **SumoLogic** -Mandanten an.

2. Navigieren Sie zu **Benutzer \> verwalten**.
   
    ![Benutzer](./media/active-directory-saas-sumologic-tutorial/IC778561.png "Benutzer")

3. Klicken Sie auf **Hinzufügen**.
   
    ![Benutzer](./media/active-directory-saas-sumologic-tutorial/IC778562.png "Benutzer")

4. Führen Sie im Dialogfeld **Neuer Benutzer** die folgenden Schritte aus:
   
    ![Neuer Benutzer](./media/active-directory-saas-sumologic-tutorial/IC778563.png "Neuer Benutzer")  
  1. Geben Sie die entsprechenden Informationen in die Textfelder **Vornamen**, **Nachnamen**, **Email-Adresse** eines gültigen AAD-Benutzerkontos ein, das Sie bereitstellen möchten.
  2. Wählen Sie eine Rolle aus.
  3. Wählen Sie als **Status** **Aktiv** aus.
  4. Klicken Sie auf **Speichern**.

>[!NOTE]
>Sie können AAD-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von SumoLogic-Benutzerkonten oder mithilfe der von SumoLogic bereitgestellten APIs erstellen. 
> 

## <a name="assign-users"></a>Benutzer zuweisen
Um Ihre Konfiguration zu testen, müssen Sie den Azure AD-Benutzern, denen Sie die Verwendung Ihrer Anwendung ermöglichen möchten, Zugriff auf die Anwendung gewähren. Weisen Sie dazu der Anwendung Benutzer zu.

**So weisen Sie SumoLogic Benutzer zu**

1. Erstellen Sie im klassischen Azure-Portal ein Testkonto.

2. Klicken Sie auf der Anwendungsintegrationsseite für **SumoLogic** auf **Benutzer zuweisen**.
   
    ![Zuweisen von Benutzern](./media/active-directory-saas-sumologic-tutorial/IC778564.png "Zuweisen von Benutzern")

3. Wählen Sie den Testbenutzer aus, klicken Sie auf **Zuweisen** und anschließend auf **Ja**, um die Zuweisung zu bestätigen.
   
    ![Ja](./media/active-directory-saas-sumologic-tutorial/IC767830.png "Ja")

Wenn Sie die SSO-Einstellungen testen möchten, öffnen Sie den Zugriffsbereich. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).


