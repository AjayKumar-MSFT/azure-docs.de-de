---
title: Neustart eines Azure Database for PostgreSQL-Servers über das Azure-Portal
description: In diesem Artikel wird beschrieben, wie Sie über das Azure-Portal einen Azure Database for PostgreSQL-Server neu starten können.
author: ajlam
ms.author: andrela
ms.service: postgresql
ms.topic: conceptual
ms.date: 2/7/2019
ms.openlocfilehash: 28e99f64fdee414549c55f9666bfd53f07fb3efb
ms.sourcegitcommit: e51e940e1a0d4f6c3439ebe6674a7d0e92cdc152
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/08/2019
ms.locfileid: "55892693"
---
# <a name="restart-azure-database-for-postgresql-server-using-azure-portal"></a>Neustart eines Azure Database for PostgreSQL-Servers über das Azure-Portal
In diesem Thema wird erläutert, wie Sie einen Azure Database for PostgreSQL-Server neu starten können. Es kann vorkommen, dass Sie Ihren Server zu Wartungszwecken neu starten müssen. In diesem Fall kommt es zu einem kurzen Ausfall, weil der Vorgang vom Server ausgeführt wird.

Der Neustart des Servers wird blockiert, wenn der Dienst ausgelastet ist. Zum Beispiel kann der Dienst einen zuvor angeforderten Vorgang (z. B. das Skalieren von virtuellen Kernen) verarbeiten.
 
Die für einen Neustart benötigte Zeit hängt von dem PostgreSQL-Wiederherstellungsprozess ab. Um die Neustartzeit zu verkürzen, empfehlen wir, die Aktivitäten auf dem Server vor dem Neustart auf ein Minimum zu beschränken.

## <a name="prerequisites"></a>Voraussetzungen
Zum Durcharbeiten dieses Leitfadens benötigen Sie Folgendes:
- [Einen Server und eine Datenbank für Azure-Datenbank für PostgreSQL](quickstart-create-server-database-portal.md)

## <a name="perform-server-restart"></a>Ausführen des Serverneustarts

Mit den folgenden Schritten wird der PostgreSQL-Server neu gestartet:

1. Wählen Sie im Azure-Portal Ihren Azure Database for PostgreSQL-Server aus.

2. Klicken Sie auf der Seite **Übersicht** des Servers auf der Symbolleiste auf **Neu starten**.

   ![Azure Database for PostgreSQL – Übersicht – Schaltfläche „Neu starten“](./media/howto-restart-server-portal/2-server.png)

3. Klicken Sie auf **Ja**, um den Neustart des Servers zu bestätigen.

   ![Azure Database for PostgreSQL – Neustart bestätigen ](./media/howto-restart-server-portal/3-restart-confirm.png)

4. Beachten Sie, dass der Serverstatus zu „Wird neu gestartet“ wechselt.

   ![Azure Database for PostgreSQL – Status „Wird neu gestartet“ ](./media/howto-restart-server-portal/4-restarting-status.png)

5. Vergewissern Sie sich, das der Neustart des Servers erfolgreich ist.

   ![Azure Database for PostgreSQL – Erfolgreicher Neustart ](./media/howto-restart-server-portal/5-restart-success.png)

## <a name="next-steps"></a>Nächste Schritte

[Schnellstart: Erstellen eines Azure Database for PostgreSQL-Servers im Azure-Portal](./quickstart-create-server-database-portal.md)