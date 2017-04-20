## <a name="how-to-create-a-vnet-in-the-azure-portal"></a>Een VNet maken in de portal Azure

Volg de onderstaande stappen om een VNet op basis van het scenario hierboven, via de portal Azure voorbeeld maken.

1. Vanuit een browser, Ga naar http://portal.azure.com en meld u zonodig met uw Azure-account.
2. Klik op **Nieuw** > **netwerk** > **virtueel netwerk**, klikt u vervolgens in de lijst **Selecteer een implementatiemodel** op **Resource Manager** en klik op **maken**, zoals in de onderstaande afbeelding.

    ![VNet maken in Azure portal](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure1.gif)

3. Op het blad **virtueel netwerk maken** de VNet instellingen configureren die in de onderstaande figuur.

    ![Blade virtueel netwerk maken](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure2.png)

4. Klik op **resourcegroep** en een groep toe te voegen van de VNet te selecteren of klik op **Nieuw** om de VNet toevoegen aan een nieuwe bronnengroep. In de volgende afbeelding wordt de resource groepsinstellingen voor een nieuwe resourcegroep **TestRG**genoemd. Ga naar [Azure Resource Manager-overzicht](../articles/resource-group-overview.md#resource-groups)voor meer informatie over resourcegroepen.

    ![Resourcegroep](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure3.png)

5. Wijzig indien nodig, de instellingen van het **abonnement** en de **locatie** voor uw VNet. 

6. Als u niet zien van de VNet als een tegel in de **Startboard wilt**, **pincode te Startboard**uitschakelen. 

7. Klik op **maken** en de tegel met de naam **maken virtueel netwerk** zoals wordt weergegeven in de onderstaande afbeelding ziet.

    ![Maken virtueel netwerk naast elkaar](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure4.png)

8. Wachten op de VNet moet worden gemaakt en klik vervolgens in de blade **virtuele netwerk** op **alle instellingen** > **subnetten** > **toevoegen** zoals hieronder wordt weergegeven.

    ![Subnet in de Azure portal toe te voegen.](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure5.gif)

9. De subnetinstellingen voor de *back-end* -subnet opgeven zoals hieronder wordt weergegeven en klik vervolgens op **OK**. 

    ![Subnetinstellingen](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure6.png)

10. U ziet de lijst van subnetten, zoals in de onderstaande afbeelding.

    ![Lijst van subnetten in VNet](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure7.png)
