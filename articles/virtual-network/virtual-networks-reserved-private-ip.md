<properties 
   pageTitle="Het instellen van een statisch intern particuliere IP-adres"
   description="Wat zijn statische interne IP-adressen (Spanningsdips) en hoe u ze kunt beheren"
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
   ms.date="03/22/2016"
   ms.author="jdial" />

# <a name="how-to-set-a-static-internal-private-ip-address-using-powershell-classic"></a>Het instellen van een statisch intern particuliere IP-adres met PowerShell (klassiek)
In de meeste gevallen hoeft u niet opgeven van een statisch interne IP-adres voor uw virtuele machine. VMs in een virtueel netwerk ontvangen automatisch een intern IP-adres van een bereik dat u opgeeft. Maar in bepaalde gevallen een statisch IP-adres op te geven voor een bepaalde VM zinvol is. Als bijvoorbeeld uw VM DNS uitvoert of wordt een domeincontroller. Een statische interne IP-adres blijft bij de VM zelfs door middel van een stop/identiteitsgegevens staat. 

> [AZURE.IMPORTANT] Azure heeft twee verschillende modellen voor het maken en werken met resources: [Resource Manager en klassiek](../resource-manager-deployment-model.md). In dit artikel beschreven hoe u met de klassieke implementatiemodel. Microsoft raadt aan dat de meeste nieuwe implementaties het [implementatiemodel Resource Manager](virtual-networks-static-private-ip-arm-ps.md)gebruiken.

## <a name="how-to-verify-if-a-specific-ip-address-is-available"></a>Hoe u kunt controleren of een specifiek IP-adres beschikbaar is
De volgende PowerShell-opdracht uitvoeren om te controleren of het IP-adres *10.0.0.7* beschikbaar in een vnet met de naam *TestVnet is*, en controleer of de waarde voor *IsAvailable*:

    Test-AzureStaticVNetIP –VNetName TestVNet –IPAddress 10.0.0.7 

    IsAvailable          : True
    AvailableAddresses   : {}
    OperationDescription : Test-AzureStaticVNetIP
    OperationId          : fd3097e1-5f4b-9cac-8afa-bba1e3492609
    OperationStatus      : Succeeded

>[AZURE.NOTE] Als u testen van de opdracht boven in een veilige omgeving volgen de richtlijnen in [een virtueel netwerk maken wilt](virtual-networks-create-vnet-classic-portal.md) voor het maken van een vnet met de naam *TestVnet* en ervoor te zorgen dat de adresruimte *10.0.0.0/8* wordt gebruikt.

## <a name="how-to-specify-a-static-internal-ip-when-creating-a-vm"></a>Een statische interne IP-adres opgeven bij het maken van een VM
De onderstaande PowerShell script maakt een nieuwe wolk service genaamd *TestService*, vervolgens een afbeelding opgehaald uit Azure, en vervolgens maakt een VM met de naam *TestVM* in de nieuwe wolk service met behulp van de opgehaalde afbeelding stelt de VM in een subnet met *Subnet 1*de naam en *10.0.0.7* als een statische interne IP-adres ingesteld voor de VM:

    New-AzureService -ServiceName TestService -Location "Central US"
    $image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}
    New-AzureVMConfig -Name TestVM -InstanceSize Small -ImageName $image.ImageName `
  	| Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
  	| Set-AzureSubnet –SubnetNames Subnet-1 `
  	| Set-AzureStaticVNetIP -IPAddress 10.0.0.7 `
  	| New-AzureVM -ServiceName "TestService" –VNetName TestVnet

## <a name="how-to-retrieve-static-internal-ip-information-for-a-vm"></a>Het statische interne IP-gegevens ophalen voor een VM
Als u de statische interne IP-informatie voor VM gemaakt met het bovenstaande script, voert u de opdracht PowerShell en houd rekening met de waarden voor het *IP-adres*:

    Get-AzureVM -Name TestVM -ServiceName TestService

    DeploymentName              : TestService
    Name                        : TestVM
    Label                       : 
    VM                          : Microsoft.WindowsAzure.Commands.ServiceManagement.Model.PersistentVM
    InstanceStatus              : Provisioning
    IpAddress                   : 10.0.0.7
    InstanceStateDetails        : Windows is preparing your computer for first use...
    PowerState                  : Started
    InstanceErrorCode           : 
    InstanceFaultDomain         : 0
    InstanceName                : TestVM
    InstanceUpgradeDomain       : 0
    InstanceSize                : Small
    HostName                    : rsR2-797
    AvailabilitySetName         : 
    DNSName                     : http://testservice000.cloudapp.net/
    Status                      : Provisioning
    GuestAgentStatus            : Microsoft.WindowsAzure.Commands.ServiceManagement.Model.GuestAgentStatus
    ResourceExtensionStatusList : {Microsoft.Compute.BGInfo}
    PublicIPAddress             : 
    PublicIPName                : 
    NetworkInterfaces           : {}
    ServiceName                 : TestService
    OperationDescription        : Get-AzureVM
    OperationId                 : 34c1560a62f0901ab75cde4fed8e8bd1
    OperationStatus             : OK

## <a name="how-to-remove-a-static-internal-ip-from-a-vm"></a>Een statische interne IP-adres van een VM verwijderen
Als u wilt dat het statisch interne IP-adres toegevoegd aan de VM in het bovenstaande script, moet u de volgende PowerShell-opdracht uitvoeren:
    
    Get-AzureVM -ServiceName TestService -Name TestVM `
  	| Remove-AzureStaticVNetIP `
  	| Update-AzureVM

## <a name="how-to-add-a-static-internal-ip-to-an-existing-vm"></a>Een statische interne IP-adres toevoegen aan een bestaande VM
Als u wilt toevoegen van een interne statische IP voor VM gemaakt met het script hierboven runt hij de volgende opdracht:

    Get-AzureVM -ServiceName TestService000 -Name TestVM `
  	| Set-AzureStaticVNetIP -IPAddress 10.10.0.7 `
  	| Update-AzureVM

## <a name="next-steps"></a>Volgende stappen

[Gereserveerde IP](virtual-networks-reserved-public-ip.md)

[Instantieniveau openbare IP (ILPIP)](virtual-networks-instance-level-public-ip.md)

[Gereserveerde IP-REST API 's](https://msdn.microsoft.com/library/azure/dn722420.aspx)
 
