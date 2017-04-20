## <a name="download-and-understand-the-arm-template"></a>Download en begrijpen van de ARM-sjabloon

U kunt downloaden van de bestaande ARM sjabloon voor het maken van een VNet en twee subnetten van github, breng de gewenste wijzigingen kunt u wilt en deze opnieuw gebruiken. Volg hiervoor de onderstaande stappen.

1. Ga naar [de voorbeeldpagina van de sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vnet-two-subnets).
2. Klik op **azuredeploy.json**en klik vervolgens op **RAW**.
3. Sla het bestand op op een een lokale map op uw computer.
4. Als u bekend met ARM sjablonen bent, gaat u verder met stap 7.
5. Open het bestand dat u zojuist hebt opgeslagen en bekijk de inhoud onder **parameters** in regel 5. Sjabloonparameters ARM bieden een tijdelijke aanduiding voor waarden die kunnen worden ingevuld tijdens de implementatie.

    | Parameter | Beschrijving |
    |---|---|
    | **locatie** | Azure regio waar de VNet wordt gemaakt |
    | **vnetName** | Naam voor de nieuwe VNet |
    | **addressPrefix** | Adresruimte voor de VNet, in CIDR-indeling |
    | **subnet1Name** | Naam voor de eerste VNet |
    | **subnet1Prefix** | Voor het eerste subnet CIDR-blok |
    | **subnet2Name** | Naam voor de tweede VNet |
    | **subnet2Prefix** | Voor het tweede subnet CIDR-blok |

    >[AZURE.IMPORTANT] ARM sjablonen bijgehouden in de github kunnen wijzigen. Zorg ervoor dat u de sjabloon controleren voordat u deze gebruikt.
    
6. Controleer de inhoud van de **bronnen** en u ziet het volgende:

    - **type**. Het soort resource wordt gemaakt door de sjabloon. In dit geval **Microsoft.Network/virtualNetworks**, die staan voor een VNet.
    - **naam**. De naam van de resource. Let op het gebruik van **[parameters('vnetName')]**, wat betekent dat de naam wordt opgegeven als invoer door de gebruiker of een parameterbestand tijdens de implementatie.
    - **Eigenschappen**. Lijst met eigenschappen voor de resource. Deze sjabloon worden de eigenschappen van het adres en het subnetmasker gebruikt tijdens het maken van VNet.

7. Ga terug naar [de voorbeeldpagina van de sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vnet-two-subnets).
8. Klik op **azuredeploy-paremeters.json**en klik vervolgens op **RAW**.
9. Sla het bestand op op een een lokale map op uw computer.
10. Open het bestand dat u zojuist hebt opgeslagen en bewerkt u de waarden voor de parameters. De onderstaande waarden gebruiken voor de implementatie van de VNet in ons scenario beschreven.

        {
          "location": {
            "value": "Central US"
          },
          "vnetName": {
              "value": "TestVNet"
          },
          "addressPrefix": {
              "value": "192.168.0.0/16"
          },
          "subnet1Name": {
              "value": "FrontEnd"
          },
          "subnet1Prefix": {
            "value": "192.168.1.0/24"
          },
          "subnet2Name": {
              "value": "BackEnd"
          },
          "subnet2Prefix": {
              "value": "192.168.2.0/24"
          }
        }

11. Sla het bestand.
  