---
title: "Herstellen einer Verbindung für ein virtuelles Azure-Netzwerk mit einem anderen VNet: Portal | Microsoft-Dokumentation"
description: Erstellen Sie eine VPN Gateway-Verbindung zwischen VNets unter Verwendung von Resource Manager und Azure-Portal.
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: a7015cfc-764b-46a1-bfac-043d30a275df
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/27/2017
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: 197ebd6e37066cb4463d540284ec3f3b074d95e1
ms.openlocfilehash: c80ddbaf8c2c84735564e514ddaf4308c4aff303
ms.lasthandoff: 03/31/2017


---
# <a name="configure-a-vnet-to-vnet-connection-using-the-azure-portal"></a>Konfigurieren einer VNet-zu-VNet-Verbindung über das Azure-Portal
> [!div class="op_single_selector"]
> * [Resource Manager – Azure-Portal](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
> * [Resource Manager – PowerShell](vpn-gateway-vnet-vnet-rm-ps.md)
> * [Klassisch – Azure-Portal](vpn-gateway-howto-vnet-vnet-portal-classic.md)
> * [Klassisch – Klassisches Portal](virtual-networks-configure-vnet-to-vnet-connection.md)
> 
> 

In diesem Artikel erfahren Sie Schritt für Schritt, wie Sie im Rahmen des Resource Manager-Bereitstellungsmodells mithilfe von VPN Gateway und Azure-Portal eine Verbindung zwischen VNets erstellen.

Wenn Sie virtuelle Netzwerke mithilfe des Azure-Portals verbinden möchten, müssen sich die VNets im gleichen Abonnement befinden. Falls sich Ihre virtuellen Netzwerke in unterschiedlichen Abonnements befinden, können Sie zur Verbindungsherstellung die [PowerShell-Schritte](vpn-gateway-vnet-vnet-rm-ps.md) verwenden.

![v2v-Diagramm](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/v2vrmps.png)

### <a name="deployment-models-and-methods-for-vnet-to-vnet-connections"></a>Bereitstellungsmodelle und -methoden für VNet-zu-VNet-Verbindungen
[!INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)]

Die folgende Tabelle enthält die derzeit verfügbaren Bereitstellungsmodelle und -methoden für VNet-zu-VNet-Konfigurationen. Falls ein Artikel mit Konfigurationsschritten verfügbar ist, steht in der Tabelle ein direkter Link zur Verfügung.

[!INCLUDE [vpn-gateway-table-vnet-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)]

**VNet-Peering**

[!INCLUDE [vpn-gateway-vnetpeeringlink](../../includes/vpn-gateway-vnetpeeringlink-include.md)]

## <a name="about-vnet-to-vnet-connections"></a>Über VNet-zu-VNet-Verbindungen
Das Verbinden eines virtuellen Netzwerks mit einem anderen virtuellen Netzwerk (VNet-zu-VNet) ähnelt dem Verbinden eines VNet mit einem lokalen Standort. Beide Verbindungstypen verwenden ein Azure VPN Gateway, um einen sicheren Tunnel mit IPSec/IKE bereitzustellen. Die VNets, die Sie verbinden, können sich in verschiedenen Regionen oder unter verschiedenen Abonnements befinden.

Sie können sogar VNet-zu-VNet-Kommunikation mit Konfigurationen für mehrere Standorte kombinieren. Auf diese Weise können Sie Netzwerktopologien einrichten, die wie in der folgenden Abbildung dargestellt standortübergreifende Konnektivität mit Konnektivität zwischen virtuellen Netzwerken kombinieren:

![Informationen zu Verbindungen](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/aboutconnections.png "Informationen zu Verbindungen")

### <a name="why-connect-virtual-networks"></a>Gründe für Verbindungen zwischen virtuellen Netzwerken
Aus den folgenden Gründen empfiehlt sich das Herstellen von Verbindungen zwischen virtuellen Netzwerken:

* **Regionsübergreifende Georedundanz und Geopräsenz**
  
  * Sie können Ihre eigene Georeplikation oder -synchronisierung mit sicheren Verbindungen einrichten, ohne Endpunkte mit Internetzugriff verwenden zu müssen.
  * Mit Azure Traffic Manager und Load Balancer können Sie eine hoch verfügbare Workload mit Georedundanz über mehrere Azure-Regionen hinweg einrichten. Ein wichtiges Beispiel ist die Einrichtung von SQL Always On mit Verfügbarkeitsgruppen, die sich über mehrere Azure-Regionen erstrecken.
* **Regionale Anwendungen mit mehreren Ebenen und Isolation oder Verwaltungsgrenze**
  
  * In derselben Region können Sie Anwendungen mit mehreren Ebenen und mehreren virtuellen Netzwerken einrichten, die aufgrund von Isolations- oder Verwaltungsanforderungen miteinander verbunden sind.

Weitere Informationen zu VNet-zu-VNet-Verbindungen finden Sie am Ende dieses Artikels unter [Informationen zu VNet-zu-VNet-Verbindungen](#faq).

### <a name="values"></a>Beispieleinstellungen
Sie können die Beispielkonfigurationswerte nutzen, wenn Sie diese Schritte als Übung verwenden. Als Beispiel verwenden wir mehrere Adressräume für die einzelnen VNets. Bei VNet-zu-VNet-Konfigurationen werden jedoch nicht mehrere Adressräume benötigt.

**Werte für TestVNet1:**

* VNet-Name: TestVNet1
* Adressraum: 10.11.0.0/16
  * Subnetzname: FrontEnd
  * Subnetzadressbereich: 10.11.0.0/24
* Ressourcengruppe: TestRG1
* Standort: USA, Osten
* Adressraum: 10.12.0.0/16
  * Subnetzname: BackEnd
  * Subnetzadressbereich: 10.12.0.0/24
* Name des Gatewaysubnetzes: GatewaySubnet (wird im Portal automatisch ausgefüllt)
  * Adressbereich des Gatewaysubnetzes: 10.11.255.0/27
* DNS-Server: Verwenden Sie die IP-Adresse Ihres DNS-Servers.
* Name des virtuellen Netzwerkgateways: TestVNet1GW
* Gatewaytyp: VPN
* VPN-Typ: Routenbasiert
* SKU: Wählen Sie die gewünschte Gateway-SKU aus.
* Name der öffentlichen IP-Adresse: TestVNet1GWIP
* Verbindungswerte:
  * Name: TestVNet1toTestVNet4
  * Gemeinsam verwendeter Schlüssel: Sie können den gemeinsam verwendeten Schlüssel selbst erstellen. In diesem Beispiel verwenden wir „abc123“. Wichtig: Der Wert muss beim Erstellen der Verbindung zwischen den VNets übereinstimmen.

**Werte für TestVNet4:**

* VNet-Name: TestVNet4
* Adressraum: 10.41.0.0/16
  * Subnetzname: FrontEnd
  * Subnetzadressbereich: 10.41.0.0/24
* Ressourcengruppe: TestRG1
* Standort: USA, Westen
* Adressraum: 10.42.0.0/16
  * Subnetzname: BackEnd
  * Subnetzadressbereich: 10.42.0.0/24
* Name des Gatewaysubnetzes: GatewaySubnet (wird im Portal automatisch ausgefüllt)
  * Adressbereich des Gatewaysubnetzes: 10.41.255.0/27
* DNS-Server: Verwenden Sie die IP-Adresse Ihres DNS-Servers.
* Name des virtuellen Netzwerkgateways: TestVNet4GW
* Gatewaytyp: VPN
* VPN-Typ: Routenbasiert
* SKU: Wählen Sie die gewünschte Gateway-SKU aus.
* Name der öffentlichen IP-Adresse: TestVNet4GWIP
* Verbindungswerte:
  * Name: TestVNet4toTestVNet1
  * Gemeinsam verwendeter Schlüssel: Sie können den gemeinsam verwendeten Schlüssel selbst erstellen. In diesem Beispiel verwenden wir „abc123“. Wichtig: Der Wert muss beim Erstellen der Verbindung zwischen den VNets übereinstimmen.

## <a name="CreatVNet"></a>1. Erstellen und Konfigurieren von „TestVNet1“
Wenn Sie bereits über ein VNet verfügen, sollten Sie überprüfen, ob die Einstellungen mit Ihrem Entwurf des VPN Gateways kompatibel sind. Achten Sie besonders auf Subnetze, die sich unter Umständen mit anderen Netzwerken überlappen. Bei überlappenden Subnetzen funktioniert die Verbindung nicht einwandfrei. Wenn Ihr VNet mit den richtigen Einstellungen konfiguriert wurde, können Sie mit den Schritten im Abschnitt [Angeben eines DNS-Servers](#dns) beginnen.

### <a name="to-create-a-virtual-network"></a>So erstellen Sie ein virtuelles Netzwerk
[!INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-basic-vnet-rm-portal-include.md)]

## <a name="subnets"></a>2. Hinzufügen weiterer Adressräume und Erstellen von Subnetzen
Sie können dem VNet nach dem Erstellen weitere Adressräume hinzufügen und Subnetze erstellen.

[!INCLUDE [vpn-gateway-additional-address-space](../../includes/vpn-gateway-additional-address-space-include.md)]

## <a name="gatewaysubnet"></a>3. Erstellen eines Gatewaysubnetzes
Bevor Sie das virtuelle Netzwerk mit einem Gateway verbinden, müssen Sie das Gatewaysubnetz für das virtuelle Netzwerk erstellen, mit dem Sie eine Verbindung herstellen möchten. Erstellen Sie nach Möglichkeit ein Gatewaysubnetz mit einem CIDR-Block vom Typ „/28“ oder „/27“, damit genügend IP-Adressen für zukünftige zusätzliche Konfigurationsanforderungen zur Verfügung stehen.

Wenn Sie diese Konfiguration als Übung erstellen, können Sie beim Erstellen des Gatewaysubnetzes [diese Beispieleinstellungen](#values) verwenden.

[!INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]

### <a name="to-create-a-gateway-subnet"></a>So erstellen Sie ein Gatewaysubnetz
[!INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-rm-portal-include.md)]

## <a name="DNSServer"></a>4. Angeben eines DNS-Servers (optional)
DNS ist für VNet-zu-VNet-Verbindungen nicht erforderlich. Wenn Sie für Ressourcen, die in Ihren virtuellen Netzwerken bereitgestellt werden, die Namensauflösung verwenden möchten, müssen Sie aber einen DNS-Server angeben. Diese Einstellung bietet die Möglichkeit, den DNS-Server anzugeben, den Sie zur Namensauflösung für dieses virtuelle Netzwerk verwenden möchten. Mit dieser Einstellung wird kein DNS-Server erstellt.

[!INCLUDE [vpn-gateway-add-dns-rm-portal](../../includes/vpn-gateway-add-dns-rm-portal-include.md)]

## <a name="VNetGateway"></a>5. Erstellen eines Gateways für das virtuelle Netzwerk
In diesem Schritt erstellen Sie das virtuelle Netzwerkgateway für Ihr VNet. Dieser Schritt kann bis zu 45 Minuten dauern. Falls Sie diese Konfiguration zu Übungszwecken erstellen, können Sie die [Beispieleinstellungen](#values) verwenden.

### <a name="to-create-a-virtual-network-gateway"></a>So erstellen Sie ein Gateway für das virtuelle Netzwerk
[!INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]

## <a name="CreateTestVNet4"></a>6. Erstellen und Konfigurieren von „TestVNet4“
Nachdem Sie „TestVNet1“ konfiguriert haben, erstellen Sie „TestVNet4“. Wiederholen Sie hierzu die vorherigen Schritte mit den Werten von „TestVNet4“. Sie können mit dem Konfigurieren von „TestVNet4“ beginnen, auch wenn die Erstellung des virtuellen Netzwerkgateways für „TestVNet1“ noch nicht abgeschlossen ist. Achten Sie bei Verwendung eigener Werte darauf, dass sich die Adressräume nicht mit anderen VNets überschneiden, mit denen Sie eine Verbindung herstellen möchten.

## <a name="TestVNet1Connection"></a>7. Konfigurieren der TestVNet1-Verbindung
Nach Abschluss der Vorgänge für die virtuellen Netzwerkgateways für „TestVNet1“ und „TestVNet4“ können Sie die Verbindungen für das virtuelle Netzwerkgateway erstellen. In diesem Abschnitt erstellen Sie eine Verbindung von „VNet1“ zu „VNet4“.

1. Navigieren Sie unter **Alle Ressourcen** zum virtuellen Netzwerkgateway für Ihr VNet. Beispiel: **TestVNet1GW**. Klicken Sie auf **TestVNet1GW**, um das Blatt für das virtuelle Netzwerkgateway zu öffnen.
   
    ![Blatt „Verbindungen“](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/settings_connection.png "Blatt „Verbindungen“")
2. Klicken Sie auf **+ Hinzufügen**, um das Blatt **Verbindung hinzufügen** zu öffnen.
3. Geben Sie auf dem Blatt **Verbindung hinzufügen** im Feld „Name“ einen Namen für Ihre Verbindung ein. Beispiel: **TestVNet1toTestVNet4**.
   
    ![Verbindungsname](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/v1tov4.png "Verbindungsname")
4. Wählen Sie unter **Verbindungstyp** in der Dropdownliste die Option **VNet-zu-VNet** aus.
5. Der Wert des Felds **Erstes virtuelles Netzwerkgateway** wird automatisch ausgefüllt, da Sie diese Verbindung über das angegebene virtuelle Netzwerkgateway erstellen.
6. Das Feld **Zweites virtuelles Netzwerkgateway** ist das virtuelle Netzwerkgateway des VNets, mit dem Sie eine Verbindung herstellen möchten. Klicken Sie auf **Ein weiteres virtuelles Netzwerkgateway auswählen**, um das Blatt **Virtuelles Netzwerkgateway auswählen** zu öffnen.
   
    ![Hinzufügen der Verbindung](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/add_connection.png "Hinzufügen der Verbindung")
7. Sehen Sie sich die auf dem Blatt aufgeführten virtuellen Netzwerkgateways an. Beachten Sie, dass nur virtuelle Netzwerkgateways aus Ihrem Abonnement aufgeführt werden. Falls Sie eine Verbindung mit einem virtuellen Netzwerkgateway herstellen möchten, das nicht in Ihrem Abonnement enthalten ist, lesen Sie den [PowerShell-Artikel](vpn-gateway-vnet-vnet-rm-ps.md). 
8. Klicken Sie auf das virtuelle Netzwerkgateway, mit dem Sie eine Verbindung herstellen möchten.
9. Geben Sie im Feld **Gemeinsam verwendeter Schlüssel** einen gemeinsam verwendeten Schlüssel für Ihre Verbindung ein. Diesen Schlüssel können Sie selbst generieren oder erstellen. Bei einer Site-to-Site-Verbindung wird für Ihr lokales Gerät und für Ihre Verbindung mit dem virtuellen Netzwerkgateway exakt der gleiche Schlüssel verwendet. Das Konzept ist hier ähnlich, nur dass diesmal keine Verbindung mit einem VPN-Gerät hergestellt wird, sondern mit einem anderen virtuellen Netzwerkgateway.
   
    ![Gemeinsam verwendeter Schlüssel](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/sharedkey.png "Gemeinsam verwendeter Schlüssel")
10. Klicken Sie am unteren Rand des Blatts auf **OK**, um die vorgenommenen Änderungen zu speichern.

## <a name="TestVNet4Connection"></a>8. Konfigurieren der TestVNet4-Verbindung
Als Nächstes erstellen Sie eine Verbindung von „TestVNet4“ zu „TestVNet1“. Verwenden Sie die gleiche Methode wie beim Erstellen der Verbindung von „TestVNet1“ zu „TestVNet4“. Verwenden Sie den gleichen gemeinsam verwendeten Schlüssel.

## <a name="VerifyConnection"></a>9. Überprüfen der Verbindung
Überprüfen Sie die Verbindung. Führen Sie für jedes virtuelle Netzwerkgateway folgende Schritte aus:

1. Navigieren Sie zum Blatt für das virtuelle Netzwerkgateway. Beispiel: **TestVNet4GW**. 
2. Klicken Sie auf dem Blatt des virtuellen Netzwerkgateways auf **Verbindungen**, um das Verbindungsblatt für das virtuelle Netzwerkgateway anzuzeigen.

Sehen Sie sich die Verbindungen an, und überprüfen Sie den Status. Sobald die Verbindung erstellt wurde, werden die Statuswerte **Erfolgreich** und **Verbunden** angezeigt.

![Erfolgreich](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/connected.png "Erfolgreich")

Wenn Sie auf eine der Verbindungen doppelklicken, erhalten Sie weitere Informationen zur jeweiligen Verbindung.

![Zusammenfassung](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/essentials.png "Zusammenfassung")

## <a name="faq"></a>Informationen zu VNet-zu-VNet-Verbindungen
Zeigen Sie die Details zu den häufig gestellten Fragen an, um zusätzliche Informationen zu VNet-zu-VNet-Verbindungen zu erhalten.

[!INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-vnet-vnet-faq-include.md)]

## <a name="next-steps"></a>Nächste Schritte
Sobald die Verbindung hergestellt ist, können Sie Ihren virtuellen Netzwerken virtuelle Computer hinzufügen. Weitere Informationen finden Sie unter [Dokumentation zu Virtual Machines](https://docs.microsoft.com/azure/#pivot=services&panel=Compute) .

