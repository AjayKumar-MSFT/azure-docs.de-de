---
title: Übersicht über mehrinstanzenfähige Back-Ends mit Azure Application Gateway
description: Diese Seite bietet eine Übersicht über die Application Gateway-Unterstützung für mehrinstanzenfähige Back-Ends.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 8/1/2018
ms.author: victorh
ms.openlocfilehash: 8e5f48e42a2a677622dae0b733f9d5af484d1c51
ms.sourcegitcommit: 4eeeb520acf8b2419bcc73d8fcc81a075b81663a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/19/2018
ms.locfileid: "53605805"
---
# <a name="application-gateway-support-for-multi-tenant-back-ends"></a>Application Gateway-Unterstützung für mehrinstanzenfähige Back-Ends

Azure Application Gateway unterstützt VM-Skalierungsgruppen, Netzwerkschnittstellen, öffentliche/private IP-Adressen oder vollqualifizierte Domänennamen (Fully Qualified Domain Names, FQDNs) im Rahmen von Back-End-Pools. Der vom Client eingehende HTTP-Hostheader wird von Application Gateway standardmäßig nicht geändert und in der vorliegenden Form an das Back-End gesendet. Viele Dienste wie [Azure App Service](../app-service/overview.md) sind mehrinstanzenfähig und benötigen für die Auflösung zum korrekten Endpunkt einen bestimmten Hostheader oder eine SNI-Erweiterung. Application Gateway ermöglicht Benutzern nun die Überschreibung des eingehenden HTTP-Hostheaders auf der Grundlage der Back-End-HTTP-Einstellungen. Dies ermöglicht die Unterstützung mehrinstanzenfähiger Back-Ends für Azure Web Apps und API Management. Die Funktion ist sowohl für die Standard- als auch für die WAF-SKU verfügbar. Die Unterstützung mehrinstanzenfähiger Back-Ends kann auch in SSL-Beendigungs- und End-to-End-SSL-Szenarien verwendet werden.

> [!NOTE]
> Die Einrichtung des Authentifizierungszertifikats ist für vertrauenswürdige Azure-Dienste, z.B. Azure-Web-Apps, nicht erforderlich.

![Web-App-Szenario](./media/application-gateway-web-app-overview/scenario.png)

Die Möglichkeit zum Angeben einer Hostüberschreibung wird in den HTTP-Einstellungen definiert und kann bei der Regelerstellung für einen beliebigen Back-End-Pool angewendet werden. Hostheader und SNI-Erweiterung können bei mehrinstanzenfähigen Back-Ends auf zwei Arten überschrieben werden.

1. Festlegen des Hostnamens auf einen festen Wert in den HTTP-Einstellungen: Hierdurch wird sichergestellt, dass der Hostheader für sämtlichen eingehenden Datenverkehr des Back-End-Pools, auf den die HTTP-Einstellungen angewendet werden, mit diesem Wert überschrieben wird. Bei Verwendung von End-to-End-SSL wird der überschriebene Hostname in der SNI-Erweiterung verwendet. Diese Funktion ermöglicht Szenarien, bei denen eine Back-End-Poolfarm einen Hostheader erwartet, der sich vom eingehenden Hostheader des Kunden unterscheidet.

2. Ableiten des Hostnamens von der IP-Adresse oder dem FQDN der Back-End-Poolmitglieder: Die HTTP-Einstellungen enthalten auch eine Option zur Ableitung des Hostnamens vom FQDN eines Back-End-Poolmitglieds, sofern die Option zur Ableitung des Hostnamens von einem einzelnen Back-End-Pool-Mitglied konfiguriert ist. Bei Verwendung von End-to-End-SSL wird dieser Hostname vom FQDN abgeleitet und in der SNI-Erweiterung verwendet. Diese Funktion ermöglicht Szenarien, in denen ein Back-End-Pool mehrere mehrinstanzenfähige PaaS-Dienste (z.B. Azure Web Apps) enthalten kann und der Hostheader der Anforderung für die einzelnen Mitglieder den vom entsprechenden FQDN abgeleiteten Hostnamen enthält.

> [!NOTE]
> In beiden Fällen gelten die Einstellungen nur für das Verhalten des Livedatenverkehrs und nicht für das Integritätstestverhalten. Benutzerdefinierte Tests unterstützen bereits das Angeben eines Hostheaders in der Testkonfiguration. Benutzerdefinierte Tests unterstützen nun auch die Ableitung des Hostheaderverhaltens von den aktuell konfigurierten HTTP-Einstellungen. Diese Konfiguration kann mithilfe des Parameters `PickHostNameFromBackendHttpSettings` in der Testkonfiguration angegeben werden. Für die Verwendung von End-to-End-Funktionen müssen sowohl die Test- als auch die HTTP-Einstellungen an die korrekte Konfiguration angepasst werden.

Mit dieser Funktion können Kunden die Optionen in den HTTP-Einstellungen und benutzerdefinierten Tests auf die entsprechende Konfiguration festlegen. Diese Einstellung wird dann mithilfe einer Regel an einen Listener und an einen Back-End-Pool gebunden.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie unter [Configure App Service Web Apps with Application Gateway](application-gateway-web-app-powershell.md) (Konfigurieren von App Service-Web-Apps mit Application Gateway), wie Sie ein Anwendungsgateway mit einer Web-App als Back-End-Poolmitglied einrichten. 
