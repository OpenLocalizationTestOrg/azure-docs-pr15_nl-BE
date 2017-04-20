<properties 
   pageTitle="Instantie-niveau van openbare IP-(ILPIP) | Microsoft Azure"
   description="Wat zijn ILPIP (PIP) en hoe u ze kunt beheren"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/10/2016"
   ms.author="jdial" />

# <a name="instance-level-public-ip-overview"></a>Instantie niveau openbare IP-overzicht
Een exemplaar op openbare IP-(ILPIP) is een openbare IP-adres dat u rechtstreeks met de rol of VM-sessie, in plaats van naar de cloud-service die uw exemplaar VM of rol zich bevinden in kunt toewijzen. Dit wordt niet ter vervanging van de VIP (virtuele IP) die is toegewezen aan de service cloud. In plaats daarvan is het een extra IP-adres kunt u rechtstreeks aansluiten op uw rol of VM-exemplaar.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Meer informatie over hoe u [deze stappen uitvoert met behulp van het model Resource Manager](virtual-network-ip-addresses-overview-arm.md). 

Zorg ervoor dat u begrijpt hoe [IP-adressen](virtual-network-ip-addresses-overview-classic.md) werken in Azure.

>[AZURE.NOTE] In het verleden, is een ILPIP bedoeld als een PIP staat voor openbare IP. 

![Verschil tussen ILPIP en VIP](./media/virtual-networks-instance-level-public-ip/Figure1.png)

Zoals in figuur 1, de service cloud toegankelijk is via een VIP, terwijl de afzonderlijke VMs zijn normaal gesproken toegankelijk met VIP:&lt;poort nummer&gt;. Door het toekennen van een ILPIP voor een specifieke VM dat VM rechtstreeks met dat IP-adres kan worden geopend.

Als u een cloud-service in Azure maakt, worden overeenkomstige A DNS-records automatisch gemaakt voor toegang tot de service via een volledig gekwalificeerde domeinnaam (FQDN) in plaats van de werkelijke VIP. Ditzelfde gebeurt voor ILPIP, waardoor de toegang tot de rol of VM exemplaar door de FQDN-naam in plaats van de ILPIP. Bijvoorbeeld, als u een cloud-service met de naam *contosoadservice*maken en configureren van een web-functie met de naam *contosoweb* met twee exemplaren, Azure registreert de volgende A-records voor de exemplaren:

- contosoweb\_IN_0.contosoadservice.cloudapp.net
- contosoweb\_IN_1.contosoadservice.cloudapp.net 

>[AZURE.NOTE] U kunt slechts één ILPIP voor elk exemplaar VM of rol toewijzen. U kunt maximaal 5 ILPIP van per abonnement. ILPIP is op dit moment niet ondersteund voor meerdere NIC's VMs.

## <a name="why-should-i-request-an-ilpip"></a>Waarom moet ik een ILPIP aanvragen?
Als u verbinding maken met de rol of VM-sessie door een IP-adres dat is toegewezen aan het wilt, in plaats van de cloud service VIP:&lt;poortnummer&gt;, een aanvraag voor een ILPIP voor uw VM of een exemplaar van uw rol.
- **Passieve FTP** - door een ILPIP op de VM, kunt u ontvangen verkeer op elke poort, niet hebt u voor het openen van een eindpunt verkeer ontvangen. Hierdoor kunnen scenario's als passieve FTP-protocol waarbij de poorten dynamisch worden gekozen.
- **Uitgaande IP** - uitgaand verkeer dat afkomstig is uit de VM gaat uit met de ILPIP als de bron- en dit is uniek voor de VM met externe entiteiten.

## <a name="how-to-request-an-ilpip-during-vm-creation"></a>Het aanvragen van een ILPIP tijdens het maken van VM
Het onderstaande PowerShell script maakt een nieuwe wolk service genaamd *FTPService*, en vervolgens een afbeelding opgehaald uit Azure, en een VM met de naam *FTPInstance* met de opgehaalde afbeelding maakt, stelt de VM te gebruiken van een ILPIP en VM toegevoegd aan de nieuwe service:

    New-AzureService -ServiceName FTPService -Location "Central US"
    $image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}
    New-AzureVMConfig -Name FTPInstance -InstanceSize Small -ImageName $image.ImageName `
  	| Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
  	| Set-AzurePublicIP -PublicIPName ftpip | New-AzureVM -ServiceName FTPService -Location "Central US"

## <a name="how-to-retrieve-ilpip-information-for-a-vm"></a>Het ophalen van gegevens voor een VM ILPIP
Als u de informatie van de ILPIP voor de VM gemaakt met het bovenstaande script, de volgende PowerShell-opdracht en houd rekening met de waarden voor *PublicIPAddress* en *PublicIPName*:

    Get-AzureVM -Name FTPInstance -ServiceName FTPService

    DeploymentName              : FTPService
    Name                        : FTPInstance
    Label                       : 
    VM                          : Microsoft.WindowsAzure.Commands.ServiceManagement.Model.PersistentVM
    InstanceStatus              : ReadyRole
    IpAddress                   : 100.74.118.91
    InstanceStateDetails        : 
    PowerState                  : Started
    InstanceErrorCode           : 
    InstanceFaultDomain         : 0
    InstanceName                : FTPInstance
    InstanceUpgradeDomain       : 0
    InstanceSize                : Small
    HostName                    : FTPInstance
    AvailabilitySetName         : 
    DNSName                     : http://ftpservice888.cloudapp.net/
    Status                      : ReadyRole
    GuestAgentStatus            : Microsoft.WindowsAzure.Commands.ServiceManagement.Model.GuestAgentStatus
    ResourceExtensionStatusList : {Microsoft.Compute.BGInfo}
    PublicIPAddress             : 104.43.142.188
    PublicIPName                : ftpip
    NetworkInterfaces           : {}
    ServiceName                 : FTPService
    OperationDescription        : Get-AzureVM
    OperationId                 : 568d88d2be7c98f4bbb875e4d823718e
    OperationStatus             : OK

## <a name="how-to-remove-an-ilpip-from-a-vm"></a>Een ILPIP verwijderen uit een VM
Als u wilt dat de ILPIP toegevoegd aan de VM in het bovenstaande script, moet u de volgende PowerShell-opdracht uitvoeren:
    
    Get-AzureVM -ServiceName FTPService -Name FTPInstance `
  	| Remove-AzurePublicIP `
  	| Update-AzureVM

## <a name="how-to-add-an-ilpip-to-an-existing-vm"></a>Een ILPIP toevoegen aan een bestaande VM
Een ILPIP toevoegen aan de VM gemaakt met behulp van het bovenstaande script, moet u de volgende opdracht uitvoeren:

    Get-AzureVM -ServiceName FTPService -Name FTPInstance `
  	| Set-AzurePublicIP -PublicIPName ftpip2 `
  	| Update-AzureVM

## <a name="how-to-associate-an-ilpip-to-a-vm-by-using-a-service-configuration-file"></a>Het koppelen van een ILPIP voor een VM met behulp van een configuratiebestand service
U kunt ook een ILPIP voor een VM koppelen met behulp van een service-configuratiebestand (CSCFG). De xml voorbeeld hieronder ziet u hoe voor het configureren van een cloud-service voor het gebruik van een ILPIP *MyPublicIP* met de naam voor het exemplaar van een rol: 
    
    <?xml version="1.0" encoding="utf-8"?>
    <ServiceConfiguration serviceName="ReservedIPSample" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="4" osVersion="*" schemaVersion="2014-01.2.3">
      <Role name="WebRole1">
        <Instances count="1" />
        <ConfigurationSettings>
          <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true" />
        </ConfigurationSettings>
      </Role>
      <NetworkConfiguration>
        <VirtualNetworkSite name="VNet"/>
        <AddressAssignments>
          <InstanceAddress roleName="VMRolePersisted">
            <Subnets>
              <Subnet name="Subnet1"/>
              <Subnet name="Subnet2"/>
            </Subnets>
            <PublicIPs>
              <PublicIP name="MyPublicIP" domainNameLabel="MyPublicIP" />
            </PublicIPs>
          </InstanceAddress>
        </AddressAssignments>
      </NetworkConfiguration>
    </ServiceConfiguration>

## <a name="next-steps"></a>Volgende stappen

- Begrijpen hoe de [IP-adressering](virtual-network-ip-addresses-overview-classic.md) werkt in de klassieke implementatiemodel.

- Meer informatie over [gereserveerde IP-adressen](virtual-networks-reserved-public-ip.md).
 