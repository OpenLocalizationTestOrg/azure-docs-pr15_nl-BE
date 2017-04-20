1. Ga naar **Nieuw**in de portal. Typ 'Virtuele netwerkgateway' in zoeken. Zoek **virtueel netwerkgateway** in de zoekopdracht retourneren en klik op het item. Hiermee opent u het blad **maken virtueel netwerkgateway** .
2. Klik op **maken** onder aan het **virtuele netwerkgateway** blade. De blade **virtuele netwerk-gateway maken** wordt geopend. Vul de waarden voor de gateway virtueel netwerk.

    ![Maken virtueel netwerk gateway blade velden] (./media/vpn-gateway-add-gw-rm-portal-include/createvnetgw300.png "Maken virtueel netwerk gateway blade velden")

3. **Naam**: de naam van de gateway. Dit is niet hetzelfde als een subnet gateway benoemen. Dit is de naam van de gateway-object dat u wilt maken.

4. **Gateway-type**: Selecteer **VPN**. VPN-gateways gebruiken virtueel netwerk gateway **VPN**. 

5. **VPN-type**: Selecteer het VPN-type dat is opgegeven voor uw configuratie. De meeste configuraties vereisen een Route gebaseerde VPN-type.

6. **SKU**: Selecteer de SKU-gateway in de vervolgkeuzelijst. De SKU's die worden vermeld in de vervolgkeuzelijst zijn afhankelijk van het VPN-type dat u selecteert.

7. **Locatie**: het veld **vestiging** om te verwijzen naar de locatie waar het virtuele netwerk wijzigen.
 
8. Kies het virtuele netwerk waarmee u wilt deze gateway toevoegen. Klik op **virtuele netwerk** om te openen, het **kiezen van een virtueel netwerk** blade. Selecteer de VNet. Als uw VNet niet wordt weergegeven, moet u dat het veld **locatie** verwijst naar het gebied waar het virtuele netwerk bevindt.

9. Kies een openbaar IP-adres. Klik op het **openbare IP-adres** om te openen de blade **kiezen openbare IP-adres** . Klik op **+ nieuwe maken** om te openen de **blade van openbare IP-adres maken**. Geef een naam op voor uw openbare IP-adres. Deze blade maakt een openbaar IP-adres waaraan een openbaar IP-adres dynamisch worden toegewezen.<br>Klik op **OK** om de wijzigingen in deze blade opslaan.

10. **Abonnement**: Controleer of het juiste abonnement is geselecteerd.

11. **Resourcegroep**: deze instelling wordt bepaald door het virtuele netwerk die u selecteert. 

12. Niet de **locatie** niet aanpassen nadat u de vorige instellingen hebt opgegeven.

13. Controleer de instellingen. Als u wilt dat uw gateway op het dashboard wordt weergegeven, kunt u **aan het dashboard vastmaken** onder aan het blad selecteren.

14. Klik op **maken** om te beginnen met het maken van de gateway. De instellingen zullen worden gevalideerd en ziet u de 'Deploying virtueel netwerkgateway' naast elkaar op het dashboard. Een gateway maken kan maximaal 45 minuten duren. Wellicht moet u uw portal pagina vernieuwen om te zien de status voltooid.

    ![Implementeren van virtuele netwerkgateway] (./media/vpn-gateway-add-gw-rm-portal-include/deployvnetgw150.png "Implementeren van virtuele netwerkgateway")

11. Nadat u de gateway hebt gemaakt, kunt u het IP-adres dat is toegewezen aan het door het virtuele netwerk in de portal weergeven. De gateway wordt weergegeven als een aangesloten apparaat. U kunt klikken op het aangesloten apparaat (de gateway virtueel netwerk) voor meer informatie.



