<properties 
    pageTitle="Gesimuleerde hybride cloud testomgeving | Microsoft Azure" 
    description="Een gesimuleerde hybride cloud omgeving creëren voor IT pro of ontwikkeling testen met behulp van twee Azure virtuele netwerken en een VNet-VNet-verbinding." 
    services="virtual-machines-windows" 
    documentationCenter="" 
    authors="JoeDavies-MSFT" 
    manager="timlt" 
    editor=""
    tags="azure-resource-manager"/>

<tags 
    ms.service="virtual-machines-windows" 
    ms.workload="infrastructure-services" 
    ms.tgt_pltfrm="vm-windows" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/30/2016" 
    ms.author="josephd"/>

# <a name="set-up-a-simulated-hybrid-cloud-environment-for-testing"></a>Instellen van een gesimuleerde hybride cloud omgeving voor het testen

Dit artikel begeleidt u door het maken van een gesimuleerde hybride cloud-omgeving met Microsoft Azure met twee Azure virtuele netwerken. Hier is de resulterende configuratie.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph4.png)

Dit simuleert een hybride cloud productie-omgeving en bestaat uit:

- Een gesimuleerde en vereenvoudigde op-premises netwerk gehost in Azure virtueel netwerk (het TestLab virtueel netwerk).
- Een gesimuleerde cross-premises virtueel netwerk ondergebracht in Azure (TestVNET).
- Een VNet-VNet-verbinding tussen de twee virtuele netwerken.
- Een tweede domeincontroller in het virtuele netwerk TestVNET.

Dit biedt dat een basis en een gemeenschappelijke starten wijst uit dat u kunt:

- Toepassingen ontwikkelen en testen in een gesimuleerde hybride cloud-omgeving.
- Configuraties van computers, sommige binnen het virtuele netwerk TestLab en sommige binnen het virtuele netwerk van TestVNET voor het simuleren van hybride cloud-gebaseerde IT-werklasten test maken.

Er zijn vier belangrijke fasen om deze hybride cloud-testomgeving in te stellen:

1.  Configureer de virtuele netwerk TestLab.
2.  De cross-premises virtueel netwerk maken.
3.  De VNet-VNet-VPN-verbinding maken.
4.  DC2 configureren. 

Deze configuratie is vereist voor een abonnement op Azure. Als u een abonnement op MSDN of Visual Studio, Zie [maandelijkse Azure krediet voor abonnees van Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).

>[AZURE.NOTE] Virtuele machines en virtueel netwerkgateways in Azure kosten in rekening gebracht een voortdurende monetaire wanneer ze worden uitgevoerd. Deze kosten in rekening gebracht tegen de MSDN of betaald abonnement. Een Azure VPN-gateway wordt geïmplementeerd als een set van twee Azure virtuele machines. Om de kosten te minimaliseren, maken de testomgeving en zo snel mogelijk de benodigde test- en demonstratie uit te voeren.

## <a name="phase-1-configure-the-testlab-virtual-network"></a>Fase 1: Het TestLab virtuele netwerk configureren

Volg de instructies in het onderwerp [Base configuratie testomgeving](https://technet.microsoft.com/library/mt771177.aspx) de DC1 en 1 CLIENT1 om computers te configureren in de Azure virtueel netwerk met de naam TestLab. 

Start vervolgens een Azure PowerShell-prompt.

> [AZURE.NOTE] De volgende opdracht stelt gebruik Azure PowerShell 1.0 en hoger.

Aanmelden bij uw account.

    Login-AzureRMAccount

De abonnementsnaam van uw met de volgende opdracht krijgt.

    Get-AzureRMSubscription | Sort SubscriptionName | Select SubscriptionName

Stel uw abonnement Azure. Gebruik het hetzelfde abonnement waarmee u de basisconfiguratie in fase 1. Vervang alles binnen de aanhalingstekens, met inbegrip van de < en > tekens met de juiste naam.

    $subscr="<subscription name>"
    Get-AzureRmSubscription –SubscriptionName $subscr | Select-AzureRmSubscription

Voeg vervolgens een gateway subnet met het virtuele netwerk TestLab van de basisconfiguratie, worden gebruikt als host voor de gateway Azure.

    $rgName="<name of your resource group that you used for your TestLab virtual network>"
    $locName="<Azure location name where you placed the TestLab virtual network, such as West US>"
    $vnet=Get-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name TestLab
    Add-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix 10.255.255.248/29 -VirtualNetwork $vnet
    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

Vervolgens vraagt u een openbaar IP-adres toe te wijzen aan de gateway voor het virtuele netwerk TestLab.

    $gwpip=New-AzureRmPublicIpAddress -Name TestLab_pip -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic

Maak uw gateway.

    $vnet=Get-AzureRmVirtualNetwork -Name TestLab -ResourceGroupName $rgName
    $subnet=Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
    $gwipconfig=New-AzureRmVirtualNetworkGatewayIpConfig -Name TestLab_GWConfig -SubnetId $subnet.Id -PublicIpAddressId $gwpip.Id 
    New-AzureRmVirtualNetworkGateway -Name TestLab_GW -ResourceGroupName $rgName -Location $locName -IpConfigurations $gwipconfig -GatewayType Vpn -VpnType RouteBased

Houd er rekening mee dat nieuwe gateways duurt 20 minuten of langer te maken.

Vanaf de Azure portal op de lokale computer verbinding met DC1 met de referenties van de CORP\User1. Als het domein CORP zodanig configureren dat computers en gebruikers hun lokale domeincontroller voor verificatie gebruiken, moet u deze opdrachten uitvoeren vanaf een opdrachtprompt beheerdersniveau Windows PowerShell op DC1.

    New-ADReplicationSite -Name "TestLab" 
    New-ADReplicationSite -Name "TestVNET"
    New-ADReplicationSubnet -Name "10.0.0.0/8" -Site "TestLab"
    New-ADReplicationSubnet -Name "192.168.0.0/16" -Site "TestVNET"

Dit is uw huidige configuratie.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph1.png)
 
## <a name="phase-2-create-the-testvnet-virtual-network"></a>Fase 2: De TestVNET virtueel netwerk maken

Eerst maakt u het virtuele netwerk TestVNET en beveiligen met een beveiligingsgroep netwerk.

    $rgName="<name of the resource group that you used for your TestLab virtual network>"
    $locName="<Azure location name where you placed the TestLab virtual network, such as West US>"
    $locShortName="<Azure location name from $locName in all lowercase letters with spaces removed. Example:  westus>"
    $testSubnet=New-AzureRMVirtualNetworkSubnetConfig -Name "TestSubnet" -AddressPrefix 192.168.0.0/24
    $gatewaySubnet=New-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix 192.168.255.248/29
    New-AzureRMVirtualNetwork -Name "TestVNET" -ResourceGroupName $rgName -Location $locName -AddressPrefix 192.168.0.0/16 -Subnet $testSubnet,$gatewaySubnet –DNSServer 10.0.0.4
    $rule1=New-AzureRMNetworkSecurityRuleConfig -Name "RDPTraffic" -Description "Allow RDP to all VMs on the subnet" -Access Allow -Protocol Tcp -Direction Inbound -Priority 100 -SourceAddressPrefix Internet -SourcePortRange * -DestinationAddressPrefix * -DestinationPortRange 3389
    New-AzureRMNetworkSecurityGroup -Name "TestSubnet" -ResourceGroupName $rgName -Location $locShortName -SecurityRules $rule1
    $vnet=Get-AzureRMVirtualNetwork -ResourceGroupName $rgName -Name TestVNET
    $nsg=Get-AzureRMNetworkSecurityGroup -Name "TestSubnet" -ResourceGroupName $rgName
    Set-AzureRMVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name "TestSubnet" -AddressPrefix 192.168.0.0/24 -NetworkSecurityGroup $nsg

Vervolgens vraagt u een openbaar IP-adres worden toegewezen aan de gateway voor het virtuele netwerk van TestVNET en het maken van uw gateway.

    $gwpip=New-AzureRmPublicIpAddress -Name TestVNET_pip -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
    $vnet=Get-AzureRmVirtualNetwork -Name TestVNET -ResourceGroupName $rgName
    $subnet=Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
    $gwipconfig=New-AzureRmVirtualNetworkGatewayIpConfig -Name "TestVNET_GWConfig" -SubnetId $subnet.Id -PublicIpAddressId $gwpip.Id
    New-AzureRmVirtualNetworkGateway -Name "TestVNET_GW" -ResourceGroupName $rgName -Location $locName -IpConfigurations $gwipconfig -GatewayType Vpn -VpnType RouteBased

Dit is uw huidige configuratie.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph2.png)
 
##<a name="phase-3-create-the-vnet-to-vnet-connection"></a>Fase 3: De VNet-VNet-verbinding maken

Eerst moet u een vooraf-gedeelde sleutel willekeurige cryptografisch sterke, 32 tekens van de netwerk- of systeembeheerder. De informatie op [een willekeurige tekenreeks voor een vooraf gedeelde sleutel van IPsec maken](http://social.technet.microsoft.com/wiki/contents/articles/32330.create-a-random-string-for-an-ipsec-preshared-key.aspx) ook gebruiken voor een vooraf-gedeelde sleutel.

Vervolgens moet u deze opdrachten gebruiken voor het maken van de VNet-VNet-VPN-verbinding, die kan enige tijd duren.

    $sharedKey="<pre-shared key value>"
    $gwTestLab=Get-AzureRmVirtualNetworkGateway -Name TestLab_GW -ResourceGroupName $rgName
    $gwTestVNET=Get-AzureRmVirtualNetworkGateway -Name TestVNET_GW -ResourceGroupName $rgName
    New-AzureRmVirtualNetworkGatewayConnection -Name TestLab_to_TestVNET -ResourceGroupName $rgName -VirtualNetworkGateway1 $gwTestLab -VirtualNetworkGateway2 $gwTestVNET -Location $locName -ConnectionType Vnet2Vnet -SharedKey $sharedKey
    New-AzureRmVirtualNetworkGatewayConnection -Name TestVNET_to_TestLab -ResourceGroupName $rgName -VirtualNetworkGateway1 $gwTestVNET -VirtualNetworkGateway2 $gwTestLab -Location $locName -ConnectionType Vnet2Vnet -SharedKey $sharedKey

Na een paar minuten moet de verbinding worden gemaakt.

Dit is uw huidige configuratie.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph3.png)
 
## <a name="phase-4-configure-dc2"></a>Fase 4: Configureer DC2

Maak eerst een virtuele machine voor DC2. Deze opdrachten uitvoeren vanaf de opdrachtprompt Azure PowerShell op uw lokale computer.

    $rgName="<your resource group name>"
    $locName="<your Azure location, such as West US>"
    $saName="<the storage account name for the base configuration>"
    $vnet=Get-AzureRMVirtualNetwork -Name TestVNET -ResourceGroupName $rgName
    $pip=New-AzureRMPublicIpAddress -Name DC2-NIC -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
    $nic=New-AzureRMNetworkInterface -Name DC2-NIC -ResourceGroupName $rgName -Location $locName -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id -PrivateIpAddress 192.168.0.4
    $vm=New-AzureRMVMConfig -VMName DC2 -VMSize Standard_A1
    $storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
    $vhdURI=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/DC2-TestVNET-ADDSDisk.vhd"
    Add-AzureRMVMDataDisk -VM $vm -Name ADDS-Data -DiskSizeInGB 20 -VhdUri $vhdURI  -CreateOption empty
    $cred=Get-Credential -Message "Type the name and password of the local administrator account for DC2."
    $vm=Set-AzureRMVMOperatingSystem -VM $vm -Windows -ComputerName DC2 -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm=Set-AzureRMVMSourceImage -VM $vm -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
    $vm=Add-AzureRMVMNetworkInterface -VM $vm -Id $nic.Id
    $osDiskUri=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/DC2-TestLab-OSDisk.vhd"
    $vm=Set-AzureRMVMOSDisk -VM $vm -Name DC2-TestVNET-OSDisk -VhdUri $osDiskUri -CreateOption fromImage
    New-AzureRMVM -ResourceGroupName $rgName -Location $locName -VM $vm

Vervolgens verbinding maken met de nieuwe virtuele machine voor DC2 vanaf de portal Azure.

Configureer een Windows Firewall-regel voor het toestaan van verkeer voor eenvoudige connectiviteit testen. Uit de beheerdersniveau Windows PowerShell-prompt op DC2, deze opdrachten worden uitgevoerd.

    Set-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" -enabled True
    ping dc1.corp.contoso.com

De opdracht ping moet leiden tot vier geslaagde antwoorden van IP-adres, adres 10.0.0.4. Dit is een test van het verkeer via de verbinding VNet naar VNet.

Voeg vervolgens de extra gegevensschijf op DC2 als een nieuw volume met de stationsletter F:.

1.  Klik op **Bestands- en opslagservices**in het linkerdeelvenster van Serverbeheer en klik vervolgens op **schijven**.
2.  In het deelvenster inhoud in de groep **schijven** , klikt u op **schijf 2** (met de **partitie** ingesteld op **Onbekend**).
3.  Klik op **taken**en klik vervolgens op **NieuwVolume**.
4.  Op de Before begin pagina van de Wizard Nieuw Volume en klik op **volgende**.
5.  Selecteer op de pagina van de server en de schijf, klikt u op **schijf 2**en klik op **volgende**. Klik op **OK**wanneer daarom wordt gevraagd.
6.  Geef de grootte van het volume, klik op **volgende**.
7.  Bij het toewijzen aan een station of een map pagina, klik op **volgende**.
8.  Klik op **volgende**op de pagina Selecteer bestand systeem instellingen.
9.  Klik op de pagina bevestigen selecties **maken**.
10. Als alles klaar is, klikt u op **sluiten**.

Configureer DC2 als een replica-domeincontroller voor het domein corp.contoso.com. Deze opdrachten uitvoeren vanaf de opdrachtprompt van Windows PowerShell op DC2.

    Install-WindowsFeature AD-Domain-Services -IncludeManagementTools
    Install-ADDSDomainController -Credential (Get-Credential CORP\User1) -DomainName "corp.contoso.com" -InstallDns:$true -DatabasePath "F:\NTDS" -LogPath "F:\Logs" -SysvolPath "F:\SYSVOL"

Houd er rekening mee dat u wordt gevraagd om het wachtwoord van de CORP\User1 en een wachtwoord voor de DSRM Directory Services Restore Mode (Mode) en DC2 opnieuw opstarten.

Nu dat het virtuele netwerk TestVNET heeft een eigen DNS-server (DC2), moet u het virtuele netwerk TestVNET om deze DNS-server configureren.

1.  Klik op het pictogram van de virtuele netwerken en klik op **TestVNET**in het linkerdeelvenster van de portal voor Azure.
2.  Klik op **DNS-servers**op het tabblad **Instellingen** .
3.  Typ in het vak **primaire DNS-server** **192.168.0.4** om het adres 10.0.0.4 vervangen.
4.  Klik op **Opslaan**.

Dit is uw huidige configuratie. 

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph4.png)
 
De gesimuleerde hybride cloud-omgeving is nu klaar om te testen.

## <a name="next-step"></a>Volgende stap

- Stel een [regel op het web, zakelijke toepassing](virtual-machines-windows-ps-hybrid-cloud-test-env-lob.md) in deze omgeving.
