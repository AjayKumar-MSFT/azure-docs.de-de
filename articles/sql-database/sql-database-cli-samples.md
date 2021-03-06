---
title: Azure CLI-Skriptbeispiele für SQL-Datenbank | Microsoft-Dokumentation
description: Azure CLI-Skriptbeispiele zum Erstellen und Verwalten von Azure SQL-Datenbank-Servern, Pools für elastische Datenbanken, Datenbanken und Firewalls.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: overview-samples, mvc
ms.devlang: azurecli
ms.topic: sample
author: CarlRabeler
ms.author: carlrab
ms.reviewer: ''
manager: craigg
ms.date: 02/03/2019
ms.openlocfilehash: 3b98214da259e9e429c938f8ca7963c9f525e862
ms.sourcegitcommit: ba035bfe9fab85dd1e6134a98af1ad7cf6891033
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2019
ms.locfileid: "55560820"
---
# <a name="azure-cli-samples-for-azure-sql-database"></a>Azure CLI-Beispiele für Azure SQL-Datenbank

Azure SQL-Datenbank kann mithilfe der <a href="/cli/azure">Azure CLI</a> konfiguriert werden.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Wenn Sie die Befehlszeilenschnittstelle lokal installieren und verwenden möchten, müssen Sie für dieses Thema die Azure CLI in Version 2.0 oder höher ausführen. Führen Sie `az --version` aus, um die Version zu finden. Installations- und Upgradeinformationen finden Sie bei Bedarf unter [Installieren von Azure CLI]( /cli/azure/install-azure-cli). 

## <a name="single-database--elastic-pools"></a>Einzeldatenbank und Pools für elastische Datenbanken

Die folgende Tabelle enthält Links zu Azure CLI-Skriptbeispielen für Azure SQL-Datenbank.

| |  |
|---|---|
|**Erstellen einer einzelnen Datenbank und eines Pools für elastische Datenbanken**||
| [Create a single SQL database and configure a firewall rule using PowerShell](scripts/sql-database-create-and-configure-database-cli.md?toc=%2fcli%2fazure%2ftoc.json) (Erstellen einer einzelnen SQL-Datenbank und Konfigurieren einer Firewallregel mit PowerShell) | In diesem CLI-Skriptbeispiel wird eine einzelne Azure SQL-Datenbank erstellt und eine Firewallregel auf Serverebene konfiguriert. |
| [Create elastic pools and move databases between pools and out of a pool using PowerShell](scripts/sql-database-move-database-between-pools-cli.md?toc=%2fcli%2fazure%2ftoc.json) (Erstellen von Pools für elastische Datenbanken und Verschieben von Datenbanken zwischen Pools und aus einem Pool mit PowerShell) | In diesem CLI-Skriptbeispiel werden Pools für elastische SQL-Datenbanken erstellt, in einem Pool zusammengefasste Azure SQL-Datenbanken verschoben und Computegrößen geändert.|
|**Skalieren einer einzelnen Datenbank und eines Pools für elastische Datenbanken**||
| [Monitor and scale a single SQL database using the Azure CLI](scripts/sql-database-monitor-and-scale-database-cli.md?toc=%2fcli%2fazure%2ftoc.json) (Überwachen und Skalieren einer einzelnen SQL-Datenbank mit Azure CLI) | In diesem CLI-Skriptbeispiel wird eine einzelne Azure SQL-Datenbank nach Abfrage der Größeninformationen für die Datenbank auf eine andere Computegröße skaliert. |
| [Scale an elastic pool in Azure SQL Database using the Azure CLI](scripts/sql-database-scale-pool-cli.md?toc=%2fcli%2fazure%2ftoc.json) (Skalieren eines Pools für elastische Datenbanken in Azure SQL-Datenbank mit Azure CLI) | In diesem CLI-Skriptbeispiel wird ein Pool für elastische SQL-Datenbanken auf eine andere Computegröße skaliert.  |
|||

Erfahren Sie mehr über die [Azure CLI-API für Einzeldatenbanken](sql-database-single-databases-manage.md#azure-cli-manage-sql-database-servers-and-single-databases).

## <a name="managed-instance"></a>Verwaltete Instanz

Die folgende Tabelle enthält Links zu Azure CLI-Skriptbeispielen für verwaltete Azure SQL-Datenbank-Instanzen.

| |  |
|---|---|
| [Erstellen einer verwalteten Instanz](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2018/11/14/create-azure-sql-managed-instance-using-azure-cli/) | Dieses CLI-Skript veranschaulicht die Erstellung einer verwalteten Instanz. |
| [Aktualisieren einer verwalteten Instanz](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2018/11/14/modify-azure-sql-database-managed-instance-using-azure-cli/) | Dieses CLI-Skript veranschaulicht die Aktualisierung einer verwalteten Instanz. |
| [Verschieben einer Datenbank in eine andere verwaltete Instanz](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2018/06/07/cross-instance-point-in-time-restore-in-azure-sql-database-managed-instance/) | Dieses CLI-Skript veranschaulicht die Wiederherstellung einer Datenbanksicherung aus einer Instanz in einer anderen. |
|||

Erfahren Sie mehr über die [Azure CLI-API für verwaltete Instanzen](sql-database-managed-instance-create-manage.md#azure-cli-create-and-manage-managed-instances), und finden Sie [hier weitere Beispiele](https://medium.com/azure-sqldb-managed-instance/working-with-sql-managed-instance-using-azure-cli-611795fe0b44).
