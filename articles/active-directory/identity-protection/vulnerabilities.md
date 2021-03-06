---
title: Von Azure Active Directory Identity Protection erkannte Sicherheitslücken | Microsoft Docs
description: Übersicht über die von Azure Active Directory Identity Protection erkannten Sicherheitslücken
services: active-directory
keywords: Azure Active Directory Identity Protection, Cloud Discovery, Verwalten von Anwendungen, Sicherheit, Risiko, Risikostufe, Sicherheitsrisiko, Sicherheitsrichtlinie
documentationcenter: ''
author: MarkusVi
manager: daveba
ms.assetid: 92233a5b-cb34-4d28-88cc-d5d29c0f3256
ms.service: active-directory
ms.subservice: identity-protection
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2018
ms.author: markvi
ms.reviewer: nigu
ms.collection: M365-identity-device-management
ms.openlocfilehash: b5968731981e9ba602a27b6b66a53e7b67e8fa7f
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2019
ms.locfileid: "56187320"
---
# <a name="vulnerabilities-detected-by-azure-active-directory-identity-protection"></a>Von Azure Active Directory Identity Protection erkannte Sicherheitsrisiken
Bei Sicherheitsrisiken handelt es sich um Schwachstellen in Ihrer Umgebung, die von einem Angreifer ausgenutzt werden können. Es wird empfohlen, dass Sie diese Sicherheitsrisiken entschärfen, um den Sicherheitsstatus Ihrer Organisation zu verbessern und Angreifer daran zu hindern, diese Schwachstellen auszunutzen.


![Sicherheitsrisiken](./media/vulnerabilities/101.png "Sicherheitsrisiken")



Die folgenden Abschnitte bieten Ihnen eine Übersicht der Sicherheitsrisiken, die von Identity Protection gemeldet werden.

## <a name="multi-factor-authentication-registration-not-configured"></a>Multi-Factor Authentication-Registrierung nicht konfiguriert
Mit diesem Sicherheitsrisiko können Sie die Bereitstellung der Azure Multi-Factor Authentication in Ihrer Organisation besser steuern. 

Azure Multi-Factor Authentication dient als zweite Sicherheitsebene für die Benutzerauthentifizierung. Sie hilft beim Schutz des Zugriffs auf Daten und Anwendungen und erfüllt gleichzeitig die Anforderungen von Benutzern an ein einfaches Anmeldeverfahren. Sie bietet leistungsfähige Authentifizierung mittels einiger einfacher Überprüfungsoptionen – Telefonanruf, SMS oder per Benachrichtigung bzw. Überprüfungscode in einer mobilen Anwendung sowie OATH-Tokens von Drittanbietern.

Es wird empfohlen, dass Sie die Azure Multi-Factor Authentication für Anmeldungen von Benutzern obligatorisch machen. Die mehrstufige Authentifizierung spielt eine wichtige Rolle in Bezug auf risikobasierte Richtlinien für bedingten Zugriff, die unter Identity Protection verfügbar sind.

Weitere Informationen finden Sie unter [Was ist Azure Multi-Factor Authentication?](../authentication/multi-factor-authentication.md).

## <a name="unmanaged-cloud-apps"></a>Nicht verwaltete Cloud-Apps
Mit diesem Sicherheitsrisiko können Sie in Ihrer Organisation nicht verwaltete Cloud-Apps identifizieren.

In modernen Unternehmen sind den IT-Abteilungen häufig nicht alle Cloudanwendungen bekannt, die die Benutzer der Organisation für ihre Arbeit verwenden. Daher ist es nicht verwunderlich, dass sich Administratoren Sorgen um den unberechtigten Zugriff auf Unternehmensdaten, mögliche Datenlecks und andere Sicherheitsrisiken machen. 

Wir empfehlen, Cloud Discovery bereitzustellen, um nicht verwaltete Cloudanwendungen zu ermitteln und diese Anwendungen mit Azure Active Directory zu verwalten.

Weitere Informationen finden Sie unter [Cloud Discovery](/cloud-app-security/set-up-cloud-discovery).

## <a name="security-alerts-from-privileged-identity-management"></a>Sicherheitswarnungen von Privileged Identity Management
Dieses Sicherheitsrisiko hilft Ihnen beim Ermitteln und Bereinigen von Warnungen zu privilegierten Identitäten in Ihrer Organisation.  

Damit Benutzer privilegierte Vorgänge ausführen können, müssen Organisationen ihren Benutzern einen vorübergehenden oder dauerhaften privilegierten Zugriff auf Azure AD, Azure- oder Office 365-Ressourcen oder andere SaaS-Apps gewähren. Mit jedem dieser privilegierten Benutzer vergrößert sich die Angriffsfläche Ihrer Organisation. Dieses Sicherheitsrisiko dient Ihnen als Unterstützung beim Identifizieren von Benutzern mit nicht erforderlichem privilegiertem Zugriff und beim Treffen geeigneter Maßnahmen, um das damit verbundene Risiko zu mindern oder zu beseitigen. 

Wir empfehlen Ihrer Organisation die Verwendung von Azure AD Privileged Identity Management, um privilegierte Identitäten und deren Zugriff auf Ressourcen in Azure AD und anderen Microsoft-Onlinediensten wie Office 365 oder Microsoft Intune verwalten, steuern und überwachen zu können.

Weitere Informationen finden Sie unter [Was ist Azure AD Privileged Identity Management?](../privileged-identity-management/pim-configure.md). 

## <a name="see-also"></a>Weitere Informationen

[Azure Active Directory Identity Protection](../active-directory-identityprotection.md)

