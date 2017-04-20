## <a name="how-to-create-a-classic-vnet-in-the-azure-portal"></a>Het maken van een klassieke VNet in Azure portal

U maakt een klassieke VNet op basis van het bovenstaande scenario, de volgende stappen uit te voeren.

1. Vanuit een browser, Ga naar http://portal.azure.com en meld u zonodig met uw Azure-account.
2. Klik op **Nieuw** > **netwerk** > **virtueel netwerk**, de aankondiging van de **klassieke**al ziet u de lijst **Selecteer een implementatiemodel** en klik vervolgens op **maken**, zoals in de onderstaande afbeelding.

    ![VNet maken in Azure portal](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure1.gif)

3. Typ de **naam** van de VNet op de blade **virtuele netwerk** en klik op **adresruimte**. Uw adres space-instellingen voor de VNet en de eerste subnet configureren en klik vervolgens op **OK**. In de volgende afbeelding worden de CIDR blokkeren voor ons scenario.

    ![Adres ruimte blade](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure2.png)

4. Klik op **Resourcegroep** en een groep toe te voegen van de VNet te selecteren of klik op **nieuwe resourcegroep maken** om de VNet toevoegen aan een nieuwe bronnengroep. In de volgende afbeelding wordt de resource groepsinstellingen voor een nieuwe resourcegroep **TestRG**genoemd. Ga naar [Azure Resource Manager-overzicht](../articles/virtual-network/resource-group-overview.md#resource-groups)voor meer informatie over resourcegroepen.

    ![Resource group blade maken](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure3.png)

5. Wijzig indien nodig, de instellingen van het **abonnement** en de **locatie** voor uw VNet. 

6. Als u niet zien van de VNet als een tegel in de **Startboard wilt**, **pincode te Startboard**uitschakelen. 

7. Klik op **maken** en de tegel met de naam **maken virtueel netwerk** zoals wordt weergegeven in de onderstaande afbeelding ziet.

    ![VNet maken in de portal](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure4.png)

8. Wachten op de VNet moet worden gemaakt en wanneer u de tegel hieronder ziet, klikt u op om toe te voegen meer subnetten.

    ![VNet maken in de portal](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure5.png)

9. Zoals hieronder wordt weergegeven, moet u de **configuratie** voor uw VNet zien. 

    ![VNet maken in de portal](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure6.png)

10. Klik op **subnetten** > **toevoegen**, typt u een **naam** opgeven van een **adresbereik (Classless block)** voor het subnet en klik vervolgens op **OK**. In de volgende afbeelding worden de instellingen voor onze huidige scenario.

    ![VNet maken in Azure portal](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure7.gif)