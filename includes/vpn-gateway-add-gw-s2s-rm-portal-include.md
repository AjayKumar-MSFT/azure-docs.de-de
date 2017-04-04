1. Klicken Sie im Portal auf der linken Seite auf **+**, und geben Sie „Gateway für virtuelle Netzwerke“ für die Suche ein. Suchen Sie unter **Ergebnisse** nach **Gateway für virtuelle Netzwerke**, und klicken Sie auf den Eintrag. Klicken Sie am unteren Rand des Blatts **Gateway für virtuelle Netzwerke** auf **Erstellen**. Das Blatt **Virtuelles Netzwerkgateway erstellen** wird geöffnet.
2. Geben Sie auf dem Blatt **Virtuelles Netzwerkgateway erstellen** die Werte für das virtuelle Netzwerkgateway an.

    ![Felder des Blatts „Gateway für virtuelle Netzwerke erstellen“](./media/vpn-gateway-add-gw-s2s-rm-portal-include/newgw.png "Neues Gateway")
3. **Name**: Benennen Sie Ihr Gateway. Dies ist nicht das Gleiche wie das Benennen eines Gatewaysubnetzes. Hierbei handelt es sich um den Namen des Gatewayobjekts, das Sie erstellen.
4. **Gatewaytyp**: Wählen Sie **VPN** aus. Bei VPN-Gateways wird ein virtuelles Netzwerkgateway vom Typ **VPN** verwendet. 
5. **VPN-Typ**: Wählen Sie den für Ihre Konfiguration angegebenen VPN-Typ aus. Bei den meisten Konfigurationen wird ein routenbasierter VPN-Typ benötigt.
6. **SKU**: Wählen Sie in der Dropdownliste die Gateway-SKU aus. Welche SKUs in der Dropdownliste aufgeführt werden, hängt vom ausgewählten VPN-Typ ab.
7. **Standort**: Unter Umständen müssen Sie einen Bildlauf durchführen, um diese Option anzuzeigen. Passen Sie das Feld **Standort** an, um auf den Standort zu verweisen, an dem sich das virtuelle Netzwerk befindet. Wenn der Standort nicht auf die Region verweist, in der sich Ihr virtuelles Netzwerk befindet, wird das virtuelle Netzwerk im nächsten Schritt nicht in der Dropdownliste „Virtuelles Netzwerk auswählen“ angezeigt.
8. **Virtuelles Netzwerk**: Wählen Sie das virtuelle Netzwerk aus, dem Sie dieses Gateway hinzufügen möchten. Klicken Sie auf **Virtuelles Netzwerk**, um das Blatt **Virtuelles Netzwerk auswählen** zu öffnen. Wählen Sie das VNet aus. Sollte Ihr VNet nicht angezeigt werden, vergewissern Sie sich, dass das Feld **Speicherort** auf die Region verweist, in dem sich Ihr virtuelles Netzwerk befindet.
9. **Öffentliche IP-Adresse**: Auf diesem Blatt wird ein Objekt für eine öffentliche IP-Adresse erstellt, dem eine öffentliche IP-Adresse dynamisch zugewiesen wird. Klicken Sie auf **Öffentliche IP-Adresse**, um das Blatt **Öffentliche IP-Adresse wählen** zu öffnen. Klicken Sie auf **+ Neu erstellen**, um das Blatt **Öffentliche IP-Adresse erstellen** zu öffnen. Geben Sie einen Namen für die öffentliche IP-Adresse ein. Klicken Sie zum Speichern der Änderungen an diesem Blatt auf **OK**.

    ![Erstellen der öffentlichen IP-Adresse](./media/vpn-gateway-add-gw-s2s-rm-portal-include/createpip.png "Erstellen der PIP")
10. **Abonnement**: Vergewissern Sie sich, dass das richtige Abonnement ausgewählt ist.
11. **Ressourcengruppe**: Diese Einstellung wird auf der Grundlage des ausgewählten virtuellen Netzwerks bestimmt. 
12. Passen Sie den **Standort** nach Angabe der vorherigen Einstellungen nicht mehr an.
13. Überprüfen Sie die Einstellungen. Sie können die Option **An Dashboard anheften** unten auf dem Blatt auswählen, wenn das Gateway auf dem Dashboard angezeigt werden soll.
14. Klicken Sie auf **Erstellen** , um das Gateway zu erstellen. Die Einstellungen werden überprüft, und auf dem Dashboard wird die Kachel mit dem Hinweis angezeigt, dass das Gateway des virtuellen Netzwerks bereitgestellt wird. Die Erstellung eines Gateways kann bis zu 45 Minuten dauern. Unter Umständen müssen Sie die Portalseite aktualisieren, um den Status „Abgeschlossen“ anzuzeigen.
    
    ![Erstellen des Gateways](./media/vpn-gateway-add-gw-s2s-rm-portal-include/creategw.png "Erstellen des Gateways")
15. Zeigen Sie nach der Erstellung des Gateways unter dem virtuellen Netzwerk im Portal die zugewiesene IP-Adresse an. Das Gateway wird als verbundenes Gerät angezeigt. Sie können auf das verbundene Gerät (Ihr virtuelles Netzwerkgateway) klicken, um weitere Informationen anzuzeigen.