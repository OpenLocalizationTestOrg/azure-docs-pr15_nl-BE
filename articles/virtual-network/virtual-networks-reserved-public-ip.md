<properties
   pageTitle="Gereserveerde IP | Microsoft Azure"
   description="Gereserveerde IP-adressen en hoe u ze kunt beheren"
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

# <a name="reserved-ip-overview"></a>Gereserveerde IP-overzicht
IP-adressen in Azure is onderverdeeld in twee categorieën: dynamische en gereserveerd. Openbare IP-adressen die worden beheerd door Azure zijn standaard dynamisch. Dat betekent dat het IP-adres voor een bepaalde cloud service (VIP gebruikt) of toegang te krijgen tot een rol of VM-instantie rechtstreeks (ILPIP) van tijd tot tijd kunt wijzigen wanneer resources afgesloten zijn of vrijgegeven.

Als u wilt voorkomen dat IP-adressen wijzigen, kunt u een IP-adres reserveren. Gereserveerde IP-adressen kan alleen worden gebruikt als een VIP, ervoor te zorgen dat het IP-adres voor de cloud-service zal dezelfde zelfs wanneer resources afgesloten worden of vrijgegeven. Bovendien kunt u als een VIP in een gereserveerd IP-adres gebruikt bestaande dynamische IP-adressen omzetten.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Informatie over het reserveren van een statische openbare IP-adres met behulp van de [bronnenbeheerder implementatiemodel](virtual-network-ip-addresses-overview-arm.md).

Zorg ervoor dat u begrijpt hoe [IP-adressen](virtual-network-ip-addresses-overview-classic.md) werken in Azure.

## <a name="when-do-i-need-a-reserved-ip"></a>Wanneer moet ik een gereserveerd IP-adres
- **U wilt ervoor zorgen dat de IP in uw abonnement is gereserveerd**. Als u reserveren, een IP-adres dat niet uit uw abonnement onder geen beding zal worden vrijgegeven wilt, moet u een gereserveerde openbare IP-adres.  
- **U wilt dat uw IP te blijven met uw cloud-service ook in staat is gestopt of opgeheven (VMs)**. Als u wilt dat de service om te worden benaderd via een IP-adres dat niet zal veranderen zelfs als VMs in de cloud-service stoppen of opgeheven.
- **U wilt ervoor zorgen dat uitgaande verkeer van Azure een voorspelbare IP-adres gebruikt**. Mogelijk hebt u de firewall op ruimten is zo geconfigureerd dat alleen verkeer van bepaalde IP-adressen. Door te reserveren IP, wordt het bron-IP-adres hebt en geen voor het bijwerken van uw firewall-regels als gevolg van een wijziging van het IP.

## <a name="faq"></a>FAQ
1. Kan ik een gereserveerd IP-adres gebruiken voor alle Azure services?  
  - Gereserveerde IP-adressen kan alleen worden gebruikt voor VMs en cloud service-exemplaar functies toegankelijk zijn via een VIP.
1. Het aantal gereserveerde IP-adressen kan ik hebben?  
  - Op dit moment mogen alle Azure abonnementen 20 gereserveerde IP-adressen gebruiken. U kunt echter extra gereserveerde IP-adressen aanvragen. Zie de pagina [abonnement en de grenzen van de Service](../azure-subscription-service-limits.md) voor meer informatie.
1. Is er een toeslag voor het gereserveerde IP-adressen?
  - Zie [Gereserveerde IP-adres prijzen Details](http://go.microsoft.com/fwlink/?LinkID=398482) voor prijsinformatie.
1. Hoe ik een IP-adres reserveren?
  - U kunt PowerShell of de [Azure Management REST API](https://msdn.microsoft.com/library/azure/dn722420.aspx) reserveert een IP-adres in een bepaald gebied. Deze gereserveerde IP-adres is gekoppeld aan uw abonnement. U kunt niet een IP-adres reserveren via de Portal beheren.
1. Kan ik gebruiken dit met affiniteit groep op basis van VNets?
  - Gereserveerde IP-adressen worden alleen ondersteund in de regionale VNets. Het wordt niet ondersteund voor VNets die gekoppeld aan de affiniteit groepen zijn. Zie voor meer informatie over het koppelen van een VNet met een regio of een groep affiniteit [over regionale VNets en affiniteit groepen](virtual-networks-migrate-to-regional-vnet.md).

## <a name="how-to-manage-reserved-vips"></a>Gereserveerde VIP's beheren

Voordat u gereserveerde IP-adressen gebruiken kunt, moet u deze toevoegen aan uw abonnement. Om een gereserveerd IP-adres uit de pool van openbare IP-adressen beschikbaar in de *V.S. centrale* locatie maakt, moet u de volgende PowerShell-opdracht uitvoeren:

    New-AzureReservedIP –ReservedIPName MyReservedIP –Location "Central US"

Merkt echter dat u niet opgeven welke IP wordt gereserveerd. De volgende PowerShell-opdracht uitvoeren om te bekijken welke IP-adressen worden gereserveerd in uw abonnement, en ziet u de waarden voor *ReservedIPName* en *adres*:

    Get-AzureReservedIP

    ReservedIPName       : MyReservedIP
    Address              : 23.101.114.211
    Id                   : d73be9dd-db12-4b5e-98c8-bc62e7c42041
    Label                :
    Location             : Central US
    State                : Created
    InUse                : False
    ServiceName          :
    DeploymentName       :
    OperationDescription : Get-AzureReservedIP
    OperationId          : 55e4f245-82e4-9c66-9bd8-273e815ce30a
    OperationStatus      : Succeeded

Nadat een IP is gereserveerd, blijft deze gekoppeld aan uw abonnement totdat u het verwijdert. Als u wilt verwijderen van de gereserveerde IP hierboven wordt weergegeven, kunt u de volgende PowerShell-opdracht uitvoeren:

    Remove-AzureReservedIP -ReservedIPName "MyReservedIP"

## <a name="how-to-reserve-the-ip-address-of-an-existing-cloud-service"></a>Het reserveren van het IP-adres van een bestaande cloud-service

U kunt het IP-adres van een bestaande cloud service reserveren door de parameter *- Servicenaam* toe te voegen. U kunt het IP-adres van een cloud service *TestService* op de *Amerikaanse centrale* locatie reserveren, kunt u de volgende PowerShell-opdracht uitvoeren:

    New-AzureReservedIP –ReservedIPName MyReservedIP –Location "Central US" -ServiceName TestService


## <a name="how-to-associate-a-reserved-ip-to-a-new-cloud-service"></a>Het koppelen van een gereserveerd IP-adres naar een nieuwe wolk service
Het onderstaande script maakt een nieuwe gereserveerde IP-adres en vervolgens koppelt u deze aan een nieuwe wolk service genaamd *TestService*.

    New-AzureReservedIP –ReservedIPName MyReservedIP –Location "Central US"
    $image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}
    New-AzureVMConfig -Name TestVM -InstanceSize Small -ImageName $image.ImageName `
  	| Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
  	| New-AzureVM -ServiceName TestService -ReservedIPName MyReservedIP -Location "Central US"

>[AZURE.NOTE] Als u een gereserveerd IP-adres met een cloud-service maakt, moet u nog steeds verwijzen naar de VM met behulp van *VIP:&lt;poortnummer >* voor binnenkomende communicatie. Reserveren van een IP betekent niet dat u rechtstreeks kunt aansluiten op de VM. De gereserveerde IP is toegewezen aan de cloud-service die naar de VM is geïmplementeerd. Als u rechtstreeks een VM door IP verbinding wilt maken, hebt u instantieniveau openbare IP configureren. Een openbare IP instantieniveau is een openbare IP (ook wel een ILPIP genoemd) rechtstreeks aan uw VM toegewezen is. Het kan niet worden gereserveerd. Zie [Instantieniveau openbare IP-(ILPIP)](virtual-networks-instance-level-public-ip.md) voor meer informatie.

## <a name="how-to-remove-a-reserved-ip-from-a-running-deployment"></a>Een gereserveerd IP-adres van een actieve implementatie verwijderen
Als u wilt dat de gereserveerde IP toegevoegd aan de nieuwe service in het bovenstaande script hebt gemaakt, kunt u de volgende PowerShell-opdracht uitvoeren:

    Remove-AzureReservedIPAssociation -ReservedIPName MyReservedIP -ServiceName TestService

>[AZURE.NOTE] Een gereserveerd IP-adres worden van een actieve implementatie niet verwijderd de reservering van uw abonnement. Het vrij eenvoudig, het onderzoektijdvak moet worden gebruikt door een andere bron in uw abonnement.

## <a name="how-to-associate-a-reserved-ip-to-a-running-deployment"></a>Het koppelen van een gereserveerd IP-adres aan een actief distributie
Het onderstaande script maakt een nieuwe wolk service met de naam *TestService2* met een nieuwe VM met de naam *TestVM2*en vervolgens koppelt u de bestaande gereserveerde IP- *MyReservedIP* met de naam naar de cloud-service.

    $image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}
    New-AzureVMConfig -Name TestVM2 -InstanceSize Small -ImageName $image.ImageName `
  	| Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
  	| New-AzureVM -ServiceName TestService2 -Location "Central US"
    Set-AzureReservedIPAssociation -ReservedIPName MyReservedIP -ServiceName TestService2

## <a name="how-to-associate-a-reserved-ip-to-a-cloud-service-by-using-a-service-configuration-file"></a>Het koppelen van een gereserveerd IP-adres naar een cloud-service met behulp van een configuratiebestand service
U kunt ook een gereserveerd IP-adres met een cloud service koppelen met behulp van een service-configuratiebestand (CSCFG). De xml voorbeeld hieronder ziet u hoe een cloud-service voor het gebruik van een gereserveerde VIP met de naam *MyReservedIP*configureren:

    <?xml version="1.0" encoding="utf-8"?>
    <ServiceConfiguration serviceName="ReservedIPSample" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="4" osVersion="*" schemaVersion="2014-01.2.3">
      <Role name="WebRole1">
        <Instances count="1" />
        <ConfigurationSettings>
          <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true" />
        </ConfigurationSettings>
      </Role>
      <NetworkConfiguration>
        <AddressAssignments>
          <ReservedIPs>
           <ReservedIP name="MyReservedIP"/>
          </ReservedIPs>
        </AddressAssignments>
      </NetworkConfiguration>
    </ServiceConfiguration>

## <a name="next-steps"></a>Volgende stappen

- Begrijpen hoe de [IP-adressering](virtual-network-ip-addresses-overview-classic.md) werkt in de klassieke implementatiemodel.

- Meer informatie over [gereserveerde particuliere IP-adressen](virtual-networks-reserved-private-ip.md).

- Informatie over [adressen exemplaar niveau openbare IP-(ILPIP)](virtual-networks-instance-level-public-ip.md).
