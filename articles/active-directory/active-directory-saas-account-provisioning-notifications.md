---
title: Kontobereitstellungsbenachrichtigungen | Microsoft Docs
description: "Erfahren Sie, wie Sie durch das Aktivieren von Benachrichtigungen zur Kontobereitstellung sicherstellen können, dass Sie zu Problemen benachrichtigt werden, die sich auf die Benutzerbereitstellung beziehen und Ihr Eingreifen erfordern."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: a637aac7-f06b-48ef-a66d-639835a8edec
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/06/2017
ms.author: markvi
translationtype: Human Translation
ms.sourcegitcommit: 6adb1dd25c24b18b834dd921c2586ef29d56dc81
ms.openlocfilehash: e15f3740fbf7cd97002b0b7efda34e968db4fe3c
ms.lasthandoff: 12/28/2016


---
# <a name="account-provisioning-notifications"></a>Kontobereitstellungsbenachrichtigungen
Mithilfe der Benutzerbereitstellung können Sie den Vorgang der Verwaltung von Benutzern in SaaS-Anwendungen von Drittanbietern automatisieren. <br>
Auch wenn es sich um einen automatisierten Vorgang handelt, ist Ihre Interaktion mit diesem Vorgang von Zeit zu Zeit erforderlich. <br>
Dies ist beispielsweise dann der Fall, wenn das Kennwort des Kontos abgelaufen ist, das Sie für den Austausch von Daten mit einer SaaS-Anwendung eines Drittanbieters konfiguriert haben. 

Indem Sie Benachrichtigungen zur Kontobereitstellung aktivieren, können Sie sicherstellen, dass Sie zu Problemen benachrichtigt werden, die sich auf die Benutzerbereitstellung beziehen und Ihr Eingreifen erfordern.

Sie aktivieren oder deaktivieren Benachrichtigungen zur Kontobereitstellung als Teil der Bereitstellungskonfiguration für eine SaaS-Anwendung eines Drittanbieters.

![Benutzerbereitstellung][1] 

Wenn Sie Benachrichtigungen zur Kontobereitstellung aktivieren möchten, aktivieren Sie das entsprechende Kontrollkästchen auf der Dialogfeldseite **Bestätigung** , und geben Sie dann den E-Mail-Alias des Empfängers ein.

![Kontobereitstellungsbenachrichtigungen][2]

Sie können eine Verteilerliste als Empfänger eingeben. Beachten Sie jedoch unbedingt, dass die Benachrichtigungs-E-Mail Links zu Berichten enthält, die nur für die Azure AD-Administratoren zugänglich sind.

Wenn Sie Benachrichtigungen zur Kontobereitstellung aktiviert haben, erhalten Sie E-Mail-Nachrichten zu wichtigen Problemen, die sich auf die Benutzerbereitstellung beziehen. Damit eine zu große Anzahl von E-Mails vermieden wird, erhalten Sie jedoch nur eine Benachrichtigungs-E-Mail pro Tag für jede SaaS-Anwendung, für die Benachrichtigungs-E-Mails aktiviert sind.

## <a name="related-articles"></a>Verwandte Artikel
* [Artikelindex für die Anwendungsverwaltung in Azure Active Directory](active-directory-apps-index.md)
* [Automatisieren der Bereitstellung/Bereitstellungsaufhebung von Benutzern für SaaS-Apps](active-directory-saas-app-provisioning.md)
* [Anpassen von Attributzuordnungen für die Benutzerbereitstellung](active-directory-saas-customizing-attribute-mappings.md)
* [Schreiben von Ausdrücken für Attributzuordnungen](active-directory-saas-writing-expressions-for-attribute-mappings.md)
* [Bereichsfilter für die Benutzerbereitstellung](active-directory-saas-scoping-filters.md)
* [Verwenden von SCIM für die automatische Bereitstellung von Benutzern und Gruppen aus Azure Active Directory für Anwendungen](active-directory-scim-provisioning.md)
* [Liste der Tutorials zur Integration von SaaS-Apps](active-directory-saas-tutorial-list.md)

<!--Image references-->
[1]: ./media/active-directory-saas-account-provisioning-notifications/ic766307.png
[2]: ./media/active-directory-saas-account-provisioning-notifications/ic766308.png

