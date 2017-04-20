<properties
  pageTitle="Een Cloud-Service verbinding te maken met een aangepaste domeincontroller | Microsoft Azure"
  description="Informatie over het aansluiten van uw web/werknemer rollen aan een aangepaste AD-domein met behulp van PowerShell en uitbreiding van de AD-domein"
  services="cloud-services"
  documentationCenter=""
  authors="Thraka"
  manager="timlt"
  editor=""/>

  <tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/21/2016"
    ms.author="adegeo"/>

# <a name="connecting-azure-cloud-services-roles-to-a-custom-ad-domain-controller-hosted-in-azure"></a>Azure Cloud Services-rollen verbinding te maken met een aangepaste AD-domeincontroller als host in Azure

We zullen een virtueel netwerk (VNet) eerst instellen in Azure. We wordt een Active Directory-domeincontroller (die worden gehost op een Azure Virtual Machine) toegevoegd aan de VNet. Vervolgens zullen we bestaande functies van de cloud-service toevoegen aan de vooraf gemaakte VNet en ze vervolgens verbinding maken met de domeincontroller.

Voordat we aan de slag, paar dingen bedenken:

1.  In deze zelfstudie wordt PowerShell, dus neem controleert u of dat u hebt Azure PowerShell is geïnstalleerd en klaar om te gaan. Zie voor hulp bij het instellen van Azure PowerShell, [installeren en configureren van Azure PowerShell](../powershell-install-configure.md).

2.  Uw domeincontroller AD en Web/werknemer rol sessies moeten worden in de VNet.

Volg deze stapsgewijze handleiding en als u eventuele problemen, laat ons een reactie hieronder. Iemand zal je terug (Ja, we opmerkingen lezen).

3. Het netwerk waarnaar wordt verwezen door de cloud-service <mark>moet</mark> een **klassieke virtueel netwerk**.

## <a name="create-a-virtual-network"></a>Een virtueel netwerk maken

U kunt een virtueel netwerk maken in Azure via de Azure klassieke portal of PowerShell. Voor deze zelfstudie gebruiken we PowerShell. Zie [Virtuele netwerken maken](../virtual-network/virtual-networks-create-vnet-arm-pportal.md)maken van een virtueel netwerk met behulp van de klassieke Azure portal.

```powershell
#Create Virtual Network

$vnetStr =
@"<?xml version="1.0" encoding="utf-8"?>
<NetworkConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
    <VirtualNetworkConfiguration>
    <VirtualNetworkSites>
        <VirtualNetworkSite name="[your-vnet-name]" Location="West US">
        <AddressSpace>
            <AddressPrefix>[your-address-prefix]</AddressPrefix>
        </AddressSpace>
        <Subnets>
            <Subnet name="[your-subnet-name]">
            <AddressPrefix>[your-subnet-range]</AddressPrefix>
            </Subnet>
        </Subnets>
        </VirtualNetworkSite>
    </VirtualNetworkSites>
    </VirtualNetworkConfiguration>
</NetworkConfiguration>
"@;

$vnetConfigPath = "<path-to-vnet-config>"
Set-AzureVNetConfig -ConfigurationPath $vnetConfigPath
```

## <a name="create-a-virtual-machine"></a>Een virtuele Machine maken

Als u klaar bent met het instellen van het virtuele netwerk, moet u een AD-domeincontroller maken. Voor deze zelfstudie instellen we van een AD-domeincontroller op een Azure Virtual Machine.

Hiertoe maakt u een virtuele machine via PowerShell met de volgende opdrachten:

```powershell
# Initialize variables
# VNet and subnet must be classic virtual network resources, not Azure Resource Manager resources.

$vnetname = '<your-vnet-name>'
$subnetname = '<your-subnet-name>'
$vmsvc1 = '<your-hosted-service>'
$vm1 = '<your-vm-name>'
$username = '<your-username>'
$password = '<your-password>'
$affgrp = '<your- affgrp>'

# Create a VM and add it to the Virtual Network

New-AzureQuickVM -Windows -ServiceName $vmsvc1 -Name $vm1 -ImageName $imgname -AdminUsername $username -Password $password -AffinityGroup $affgrp -SubnetNames $subnetname -VNetName $vnetname
```

## <a name="promote-your-virtual-machine-to-a-domain-controller"></a>Bevordering van de virtuele Machine naar een domeincontroller
Als de virtuele Machine is geconfigureerd als een domeincontroller met AD, moet u zich aanmelden bij de VM en configureer deze.

Voor het aanmelden bij de VM, kunt u het RDP-bestand ophalen via PowerShell, gebruikt u de volgende opdrachten.

```powershell
# Get RDP file
Get-AzureRemoteDesktopFile -ServiceName $vmsvc1 -Name $vm1 -LocalPath <rdp-file-path>
```

Zodra u bent aangemeld bij de VM, setup uw virtuele Machine als een domeincontroller met AD door de stapsgewijze handleiding voor [het instellen van uw klant AD-domeincontroller](http://social.technet.microsoft.com/wiki/contents/articles/12370.windows-server-2012-set-up-your-first-domain-controller-step-by-step.aspx).

## <a name="add-your-cloud-service-to-the-virtual-network"></a>De Cloud-Service toevoegen aan het virtuele netwerk

Vervolgens moet u uw implementatie van cloud-service toevoegen aan de VNet die u zojuist hebt gemaakt. Hiertoe uw cloud service cscfg te wijzigen door de relevante secties toe te voegen aan uw cscfg met behulp van Visual Studio of de editor van uw keuze.

```xml
<ServiceConfiguration serviceName="[hosted-service-name]" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="[os-family]" osVersion="*">
    <Role name="[role-name]">
    <Instances count="[number-of-instances]" />
    </Role>
    <NetworkConfiguration>

    <!--optional-->
    <Dns>
        <DnsServers><DnsServer name="[dns-server-name]" IPAddress="[ip-address]" /></DnsServers>
    </Dns>
    <!--optional-->

    <!--VNet settings
        VNet and subnet must be classic virtual network resources, not Azure Resource Manager resources.-->
    <VirtualNetworkSite name="[virtual-network-name]" />
    <AddressAssignments>
        <InstanceAddress roleName="[role-name]">
        <Subnets>
            <Subnet name="[subnet-name]" />
        </Subnets>
        </InstanceAddress>
    </AddressAssignments>
    <!--VNet settings-->

    </NetworkConfiguration>
</ServiceConfiguration>
```

Naast uw cloud services-project maken en deze implementeren in Azure. Zie voor hulp bij het implementeren van uw pakket cloud services op Azure, [het maken en implementeren van een Cloud-Service](cloud-services-how-to-create-deploy.md#deploy)

## <a name="connect-your-webworker-roles-to-the-domain"></a>De rol(len) web/werknemer verbinding met het domein

Zodra uw project cloud-service is geïmplementeerd op Azure, verbinding maken met uw rol exemplaren aan de aangepaste AD-domein met de extensie AD-domein. Voor de uitbreiding van de AD-domein toevoegen aan uw bestaande cloud services-implementatie en lid van het aangepaste domein, de volgende opdrachten in PowerShell worden uitgevoerd:

```powershell
# Initialize domain variables

$domain = '<your-domain-name>'
$dmuser = '$domain\<your-username>'
$dmpswd = '<your-domain-password>'
$dmspwd = ConvertTo-SecureString $dmpswd -AsPlainText -Force
$dmcred = New-Object System.Management.Automation.PSCredential ($dmuser, $dmspwd)

# Add AD Domain Extension to the cloud service roles

Set-AzureServiceADDomainExtension -Service <your-cloud-service-hosted-service-name> -Role <your-role-name> -Slot <staging-or-production> -DomainName $domain -Credential $dmcred -JoinOption 35
```

En dat is alles.

U cloud services moeten nu worden gekoppeld aan uw aangepaste domeincontroller. Als u meer informatie over de verschillende opties voor het configureren van AD-domein extensie wilt, de help PowerShell gebruiken zoals hieronder wordt weergegeven.

```powershell
help Set-AzureServiceADDomainExtension
help New-AzureServiceADDomainExtensionConfig
```
