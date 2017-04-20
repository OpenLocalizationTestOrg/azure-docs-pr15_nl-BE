## <a name="how-to-create-a-vnet-using-a-network-config-file-in-the-azure-portal"></a>Het maken van een VNet met een netwerk config-bestand in de portal voor Azure

Azure wordt een XML-bestand gebruikt voor het definiëren van alle beschikbare VNets op een abonnement. U kunt dit bestand downloaden en bewerken om aan te passen of te verwijderen van bestaande VNets en nieuwe maken. In dit document, wordt u informatie over het downloaden van dit bestand netwerk configuratie (of netcgf) bestand genoemd en bewerken om een nieuwe VNet maken. Controleer het [configuratieschema Azure virtueel netwerk](https://msdn.microsoft.com/library/azure/jj157100.aspx) voor meer informatie over het netwerk configuratiebestand.

U maakt een VNet met een netcfg-bestand via de portal Azure, volg de onderstaande stappen.

1. Vanuit een browser, Ga naar http://manage.windowsazure.com en meld u zonodig met uw Azure-account.
2. Blader omlaag in de lijst met services en klik op **netwerken** , zoals hieronder wordt weergegeven.

    ![Azure virtuele netwerken](./media/virtual-networks-create-vnet-classic-portal-xml-include/vnet-create-portal-netcfg-figure1.gif)

3. Aan de onderkant van de pagina, klikt u op de knop **EXPORTEREN** zoals hieronder wordt weergegeven.

    ![Knop exporteren](./media/virtual-networks-create-vnet-classic-portal-xml-include/vnet-create-portal-netcfg-figure2.png)

4. Selecteer het abonnement dat u wilt exporteren, de configuratie van het virtuele netwerk uit op de pagina **netwerkconfiguratie exporteren** en klik vervolgens op de knop check mark op de linker benedenhoek van de pagina.
5. Volg de instructies van uw browser het bestand **NetworkConfig.xml** op te slaan. Zorg ervoor dat u ziet wanneer u het bestand wilt opslaan.
6. Open het bestand dat u hebt opgeslagen in stap 5 hierboven met een willekeurige toepassing XML of tekst editor en zoek de **<VirtualNetworkSites>** element. Als er geen netwerken al gemaakt, elk netwerk wordt weergegeven als een eigen **<VirtualNetworkSite>** element.
7. Om het virtuele netwerk dat wordt beschreven in dit scenario maakt, net onder de volgende XML toevoegen de **<VirtualNetworkSites>** element:

        <VirtualNetworkSite name="TestVNet" Location="Central US">
          <AddressSpace>
            <AddressPrefix>192.168.0.0/16</AddressPrefix>
          </AddressSpace>
          <Subnets>
            <Subnet name="FrontEnd">
              <AddressPrefix>192.168.1.0/24</AddressPrefix>
            </Subnet>
            <Subnet name="BackEnd">
              <AddressPrefix>192.168.2.0/24</AddressPrefix>
            </Subnet>
          </Subnets>
        </VirtualNetworkSite>

8.  Sla het bestand netwerk configuratie.
9.  In Azure portal in de linker benedenhoek van de pagina, klik op **Nieuw**en vervolgens **NETWORK SERVICES**, klikt u op en vervolgens klik op **VIRTUAL NETWORK**en klik op **Configuratie importeren** zoals in de onderstaande afbeelding.

    ![Configuratie importeren](./media/virtual-networks-create-vnet-classic-portal-xml-include/vnet-create-portal-netcfg-figure3.gif)

10.  Op de pagina **importeren van het configuratiebestand netwerk** klikt u op **Bladeren voor bestand...**, en vervolgens Ga naar de map die u in stap 8 hierboven uw bestand hebt opgeslagen, selecteert u het bestand en klik op **openen**. De webpagina moet uitzien in de volgende afbeelding. Klik op de rechterbenedenhoek van de pagina, op de pijl om te verplaatsen naar de volgende stap.

    ![Netwerk configuratiepagina importeren](./media/virtual-networks-create-vnet-classic-portal-xml-include/vnet-create-portal-netcfg-figure4.png)

11.   Op de pagina **maken van het netwerk** ziet u de vermelding voor uw nieuwe VNet zoals in de onderstaande afbeelding.

    ![Uw netwerk pagina maken](./media/virtual-networks-create-vnet-classic-portal-xml-include/vnet-create-portal-netcfg-figure5.png)

12.   Klik op het vinkje in de rechterbenedenhoek van de pagina voor het maken van de VNet. Na een paar seconden worden uw VNet weergegeven in de lijst met beschikbare VNets, zoals in de onderstaande afbeelding.

    ![Nieuw virtueel netwerk](./media/virtual-networks-create-vnet-classic-portal-xml-include/vnet-create-portal-netcfg-figure6.png)