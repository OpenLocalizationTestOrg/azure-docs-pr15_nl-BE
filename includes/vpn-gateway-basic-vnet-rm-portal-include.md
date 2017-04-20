Volg de onderstaande stappen om een VNet in het implementatiemodel Resource Manager maakt met behulp van de portal Azure. De screenshots zijn beschikbaar als voorbeelden. Zorg ervoor dat de waarden vervangen door uw eigen. Zie [Virtuele netwerk-overzicht](../articles/virtual-network/virtual-networks-overview.md)voor meer informatie over het werken met virtuele netwerken.

1. Vanuit een browser, Ga naar de [Azure portal](http://portal.azure.com) en meld u zonodig met uw Azure-account.

2. Klik op **Nieuw**. Typ in het veld **Zoeken de marketplace** "Virtuele netwerk". **Virtueel netwerk** vinden in de resulterende lijst en klik op om het **Virtuele netwerk** blade.

    ![Zoek Virtual Network resource blade] (./media/vpn-gateway-basic-vnet-rm-portal-include/newvnetportal700.png "Zoek virtueel netwerk bron blade")

3. Aan de onderkant van de blade virtuele netwerk in de lijst **Selecteer een implementatiemodel** **Resource Manager**te selecteren en klik op **maken**.


    ![Selecteer de Resource Manager] (./media/vpn-gateway-basic-vnet-rm-portal-include/resourcemanager250.png "Selecteer de Resource Manager")

4. Op het blad **virtueel netwerk maken** door de VNet-instellingen te configureren. Wanneer u de velden hebt ingevuld, worden het rode uitroepteken een groen vinkje als de tekens ingevoerd in het veld geldig zijn.

    ![Validatie] (./media/vpn-gateway-basic-vnet-rm-portal-include/checkmark300.png "Validatie")

5. De blade **virtueel netwerk maken** lijkt op het volgende voorbeeld. Er zijn waarden die automatisch ingevuld worden. Als dat zo is, vervangen door de waarden zelf.

    ![Maken virtueel netwerk blade] (./media/vpn-gateway-basic-vnet-rm-portal-include/createvnet300.png "Maken virtueel netwerk blade")

6. **Naam**: Voer de naam van het virtuele netwerk.

7. **Adresruimte**: de adresruimte invoeren. Als er meerdere adresruimten toe te voegen, Voeg uw eerste adresruimte. U kunt later extra spaties toevoegen na het maken van de VNet.
 
8. **Subnetnaam**: Voeg de subnet- en subnet-adresbereik. U kunt later extra subnetten toevoegen na het maken van de VNet.

10. **Abonnement**: Controleer of u het abonnement vermeld de juiste. U kunt abonnementen wijzigen met behulp van de vervolgkeuzelijst.

11. **Resourcegroep**: Selecteer een bestaande resourcegroep of een nieuwe maken door een naam voor de nieuwe resourcegroep te typen. Als u een nieuwe groep maken wilt, benoemen de resourcegroep, afhankelijk van uw van geplande configuratiewaarden. Ga naar [Azure Resource Manager-overzicht](resource-group-overview.md#resource-groups)voor meer informatie over resourcegroepen.

12. **Locatie**: Selecteer de locatie voor uw VNet. De locatie bepaalt waar de resources die u op deze VNet implementeert wordt geplaatst.

13. **Pincode voor dashboard** selecteren als u uw VNet gemakkelijk terugvinden op het dashboard en klik vervolgens op **maken**.
    
    ![Pincode voor dashboard] (./media/vpn-gateway-basic-vnet-rm-portal-include/pintodashboard150.png "pincode voor dashboard")

14. Nadat u op **maken**klikt, ziet u een tegel op uw dashboard dat de vorderingen van uw VNet weerspiegelt. De tegel wordt gewijzigd wanneer de VNet wordt gemaakt.

    ![Maken virtueel netwerk naast elkaar] (./media/vpn-gateway-basic-vnet-rm-portal-include/deploying150.png "Maken virtueel netwerk naast elkaar")