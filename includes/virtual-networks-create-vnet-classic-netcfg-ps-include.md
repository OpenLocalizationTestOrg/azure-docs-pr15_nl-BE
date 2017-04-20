## <a name="how-to-create-a-vnet-using-a-network-config-file-from-powershell"></a>Het maken van een VNet via een netwerk config-bestand van PowerShell

Azure wordt een XML-bestand gebruikt voor het definiëren van alle beschikbare VNets op een abonnement. U kunt dit bestand downloaden en bewerken om aan te passen of te verwijderen van bestaande VNets en nieuwe maken. In dit document, wordt u informatie over het downloaden van dit bestand netwerk configuratie (of netcgf) bestand genoemd en bewerken om een nieuwe VNet maken. Controleer het [configuratieschema Azure virtueel netwerk](https://msdn.microsoft.com/library/azure/jj157100.aspx) voor meer informatie over het netwerk configuratiebestand.

Volg de onderstaande stappen om het maken van een VNet met een netcfg-bestand met PowerShell.

1. Als u Azure PowerShell nog nooit hebt gebruikt, Zie [installeren en configureren van Azure PowerShell](../articles/powershell-install-configure.md) en volg de instructies helemaal naar het einde ondertekenen in Azure en selecteer uw abonnement.
2. Gebruik de cmdlet **Get-AzureVnetConfig** voor het downloaden van het configuratiebestand van het netwerk door de opdracht hieronder uit te voeren vanuit de console Azure PowerShell. 

        Get-AzureVNetConfig -ExportToFile c:\NetworkConfig.xml

    Verwachte output:

        XMLConfiguration                                                                                                     
        ----------------                                                                                                     
        <?xml version="1.0" encoding="utf-8"?>...  

3. Open het bestand dat u hebt opgeslagen in stap 2 hierboven met een willekeurige toepassing XML of tekst editor en zoek de **<VirtualNetworkSites>** element. Als er geen netwerken al gemaakt, elk netwerk wordt weergegeven als een eigen **<VirtualNetworkSite>** element.
4. Om het virtuele netwerk dat wordt beschreven in dit scenario maakt, net onder de volgende XML toevoegen de **<VirtualNetworkSites>** element:

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

9.  Sla het bestand netwerk configuratie.
10. Gebruik de cmdlet **Set-AzureVnetConfig** voor het uploaden van het configuratiebestand van het netwerk door de opdracht hieronder uit te voeren vanuit de console Azure PowerShell. U ziet de uitvoer onder de opdracht, ziet u **geslaagd** onder **OperationStatus**. Als dat niet het geval is, controleert u het XML-bestand op fouten.

        Set-AzureVNetConfig -ConfigurationPath c:\NetworkConfig.xml

    Dit is de verwachte output van de bovenstaande opdracht:

        OperationDescription OperationId                          OperationStatus
        -------------------- -----------                          ---------------
        Set-AzureVNetConfig  49579cb9-3f49-07c3-ada2-7abd0e28c4e4 Succeeded 
    
11. Gebruik de cmdlet **Get-AzureVnetSite** om te controleren of het nieuwe netwerk is toegevoegd door de opdracht hieronder uit te voeren vanuit de console Azure PowerShell. 

        Get-AzureVNetSite -VNetName TestVNet

    Dit is de verwachte output van de bovenstaande opdracht:

        AddressSpacePrefixes : {192.168.0.0/16}
        Location             : Central US
        AffinityGroup        : 
        DnsServers           : {}
        GatewayProfile       : 
        GatewaySites         : 
        Id                   : b953f47b-fad9-4075-8cfe-73ff9c98278f
        InUse                : False
        Label                : 
        Name                 : TestVNet
        State                : Created
        Subnets              : {FrontEnd, BackEnd}
        OperationDescription : Get-AzureVNetSite
        OperationId          : 3f35d533-1f38-09c0-b286-3d07cd0904d8
        OperationStatus      : Succeeded