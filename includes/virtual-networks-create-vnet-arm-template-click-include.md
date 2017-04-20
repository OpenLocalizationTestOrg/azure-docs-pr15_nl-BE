## <a name="deploy-the-arm-template-by-using-click-to-deploy"></a>De sjabloon ARM implementeren met behulp van klik te implementeren

U kunt hergebruiken van vooraf gedefinieerde sjablonen uploaden van ARM naar een github opslagplaats die door Microsoft wordt bijgehouden en open naar de Gemeenschap. Deze sjablonen kunnen worden geïmplementeerd recht uit github, of gedownload en aangepast aan uw behoeften aanpassen. Volg de onderstaande stappen voor de implementatie van een sjabloon die wordt gemaakt van een VNet met twee subnetten.

1. Vanuit een browser, Ga naar [https://github.com/Azure/azure-quickstart-templates](https://github.com/Azure/azure-quickstart-templates).
2. Schuif naar beneden in de lijst met sjablonen en op **101 vnet-twee subnetten**. Controleer het bestand **README.md** , zoals hieronder wordt weergegeven.

    ![READEME.md bestand in github](./media/virtual-networks-create-vnet-arm-template-click-include/figure1.png)

3. Klik **op Azure implementeren**. Voer indien nodig uw inloggegevens Azure. 
4. Voer de waarden die u gebruiken wilt voor het maken van uw nieuwe VNet in het blad **Parameters** en klik op **OK**. In de volgende afbeelding ziet u de waarden in ons scenario.

    ![Sjabloonparameters ARM](./media/virtual-networks-create-vnet-arm-template-click-include/figure2.png)

4. Klik op **resourcegroep** en een groep toe te voegen van de VNet te selecteren of klik op **Nieuw** om de VNet toevoegen aan een nieuwe bronnengroep. In de volgende afbeelding wordt de resource groepsinstellingen voor een nieuwe resourcegroep **TestRG**genoemd.

    ![Resourcegroep](./media/virtual-networks-create-vnet-arm-template-click-include/figure3.png)

5. Wijzig indien nodig, de instellingen van het **abonnement** en de **locatie** voor uw VNet.
6. Als u niet zien van de VNet als een tegel in de **Startboard wilt**, **pincode te Startboard**uitschakelen.
5. Op **Leagl voorwaarden**, lees de voorwaarden en klik op **kopen** te accepteren. 
6. Klik op **maken** om de VNet te maken.

    ![Indienende implementatie tegel in preview portal](./media/virtual-networks-create-vnet-arm-template-click-include/figure4.png)

7. Zodra de installatie is voltooid, klikt u op **TestVNet** > **alle instellingen** > **subnetten** voor een overzicht van de eigenschappen voor subnet, zoals hieronder wordt weergegeven.

    ![VNet maken in preview portal](./media/virtual-networks-create-vnet-arm-template-click-include/figure5.gif)