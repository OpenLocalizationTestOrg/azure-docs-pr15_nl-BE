<properties
   pageTitle="Een VM maken met meerdere NIC 's"
   description="Meer informatie over het maken en configureren van vms met meerdere NIC 's"
   services="virtual-network, virtual-machines"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-service-management,azure-resource-manager"
/>
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/02/2016"
   ms.author="jdial" />

# <a name="create-a-vm-with-multiple-nics"></a>Een VM maken met meerdere NIC 's

U kunt het maken van virtuele machines (VMs) in Azure en meerdere netwerkinterfaces (NIC's) koppelen aan elk van uw VMs. Multi NIC is een vereiste voor een groot aantal virtuele netwerkapparaten, zoals application delivery en optimalisatie van WAN-oplossingen. Ook biedt meer functionaliteit voor het netwerkverkeer beheer Multi NIC, met inbegrip van isolatie van het verkeer tussen een front end NIC en back-end bekabelen of scheiding van gegevensverkeer vlak van het beheer van verkeer vlak.

![Multi-NIC voor VM](./media/virtual-networks-multiple-nics/IC757773.png)

De bovenstaande figuur toont een VM met drie netwerkinterfacekaarten, dat elk zijn aangesloten op een ander subnet.

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]

- VIP internetverbinding (klassieke implementaties) wordt alleen ondersteund op de 'standaard' NIC. Er is slechts één VIP op het IP-adres van de standaard-NIC.
- Adressen exemplaar niveau openbare IP-(LPIP) (klassieke implementaties) worden op dit moment niet ondersteund voor meerdere NIC VMs.
- De volgorde van de NIC's van binnen de VM is willekeurig en ook via Azure infrastructuur updates kan wijzigen. De IP-adressen en de bijbehorende Ethernet MAC-adressen echter zal hetzelfde blijven. Stel **dat eth1** is IP-adres 10.1.0.100 en MAC-adres 00-0D-3A-B0-39-0D; na een Azure infrastructuur bijwerken en opnieuw opstarten, kan deze worden gewijzigd naar **Eth2**, maar de IP- en MAC koppelen blijven hetzelfde. Wanneer een opnieuw moet gestart met een klant worden, blijft de volgorde van NIC hetzelfde.
- Het adres voor elke Netwerkadapter op elk VM moet zich bevinden in een subnet, meerdere netwerkadapters op een enkele VM kunnen elk toegewezen adressen die zich in hetzelfde subnet.
- De VM-grootte bepaalt het aantal Netwerkkaarten die u voor een VM maken kunt. Verwijzing naar de [Windows Server](../virtual-machines/virtual-machines-windows-sizes.md) en [Linux](../virtual-machines/virtual-machines-linux-sizes.md) VM formaten artikelen om te bepalen hoeveel NIC's ondersteunt de grootte van elke VM. 

## <a name="network-security-groups-nsgs"></a>Netwerk-beveiligingsgroepen (NSGs)
In een implementatie Resource Manager kan een willekeurige Netwerkadapter op een VM worden gekoppeld met een netwerk Security Group (NSG), met inbegrip van alle netwerkadapters op een VM met meerdere NIC's ingeschakeld. Als u een adres in een subnet waar het subnet gekoppeld aan een NSG is is toegewezen aan een NIC, vervolgens toepassen de regels in het subnet van de NSG ook op die NIC. Naast het koppelen van subnetten aan NSGs, kunt u ook een NIC koppelen aan een NSG.

Als een subnet is gekoppeld aan een NSG en een NIC binnen dat subnet afzonderlijk gekoppeld aan een NSG, de bijbehorende NSG-regels worden toegepast in **stroom volgorde** volgens de richting van het verkeer van of naar de NIC worden doorgegeven:

- **Binnenkomend gegevensverkeer** waarvan de bestemming de betrokken NIC is doorloopt eerst het subnet van het subnet NSG regels voordat passeren de NIC en vervolgens op de NIC's NSG regels activeren activeren.
- **Uitgaand verkeer** , waarvan de bron de betrokken NIC is stroomt eerst uit de NIC, waardoor de NIC's NSG regels voordat die via het subnet en vervolgens van het subnet NSG regels activeren.

Meer informatie over [Beveiligingsgroepen netwerk](virtual-networks-nsg.md) en hoe deze worden toegepast op basis van koppelingen met subnetten, VMs en NIC's...

## <a name="how-to-configure-a-multi-nic-vm-in-a-classic-deployment"></a>Het configureren van een multi-NIC VM in een klassieke implementatie

De volgende instructies kunt u een multi-NIC VM met 3 NIC's maken: een standaard NIC en twee extra netwerkadapters. De configuratiestappen maakt een VM die wordt geconfigureerd volgens de onderstaande van service-configuratie bestandsfragment:

    <VirtualNetworkSite name="MultiNIC-VNet" Location="North Europe">
    <AddressSpace>
      <AddressPrefix>10.1.0.0/16</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="Frontend">
            <AddressPrefix>10.1.0.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Midtier">
            <AddressPrefix>10.1.1.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Backend">
            <AddressPrefix>10.1.2.0/23</AddressPrefix>
          </Subnet>
          <Subnet name="GatewaySubnet">
            <AddressPrefix>10.1.200.0/28</AddressPrefix>
          </Subnet>
        </Subnets>
    … Skip over the remainder section …
    </VirtualNetworkSite>


Voordat u probeert de PowerShell-opdrachten uitvoeren in het voorbeeld moet u de volgende vereisten.

- Een abonnement op Azure.
- Een virtueel netwerk geconfigureerd. Zie [Virtuele netwerk-overzicht](virtual-networks-overview.md) voor meer informatie over VNets.
- De nieuwste versie van Azure PowerShell gedownload en geïnstalleerd. Zie [het installeren en configureren van Azure PowerShell](../powershell-install-configure.md).

Als u wilt een VM maken met meerdere netwerkkaarten, volg de onderstaande stappen:

1. Selecteer de afbeelding van een VM in Azure VM image gallery. Houd er rekening mee dat afbeeldingen vaak worden gewijzigd en beschikbaar per regio zijn. De afbeelding die is opgegeven in het volgende voorbeeld kan wijzigen of mogelijk niet in uw regio, dus zorg ervoor dat de afbeelding die u moet opgeven.

        $image = Get-AzureVMImage `
            -ImageName "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-201410.01-en.us-127GB.vhd"

1. Maak een VM-configuratie.

        $vm = New-AzureVMConfig -Name "MultiNicVM" -InstanceSize "ExtraLarge" `
            -Image $image.ImageName –AvailabilitySetName "MyAVSet"

1. De standaard administrator-aanmelding maken.

        Add-AzureProvisioningConfig –VM $vm -Windows -AdminUserName "<YourAdminUID>" `
            -Password "<YourAdminPassword>"

1. Extra NIC's toevoegen aan de configuratie van de VM.

        Add-AzureNetworkInterfaceConfig -Name "Ethernet1" `
            -SubnetName "Midtier" -StaticVNetIPAddress "10.1.1.111" -VM $vm
        Add-AzureNetworkInterfaceConfig -Name "Ethernet2" `
            -SubnetName "Backend" -StaticVNetIPAddress "10.1.2.222" -VM $vm

1. Het IP-adres en subnetmasker opgeven voor de standaard-NIC.

        Set-AzureSubnet -SubnetNames "Frontend" -VM $vm
        Set-AzureStaticVNetIP -IPAddress "10.1.0.100" -VM $vm

1. De VM maken in het virtuele netwerk.

        New-AzureVM -ServiceName "MultiNIC-CS" –VNetName "MultiNIC-VNet" –VMs $vm

>[AZURE.NOTE] De VNet die u hier opgeeft, moet al bestaan (zoals vermeld in de voorwaarden). In het volgende voorbeeld geeft een virtueel netwerk met de naam **MultiNIC VNet**.

## <a name="limitations"></a>Beperkingen

De volgende beperkingen zijn van toepassing wanneer de functie meerdere NIC:

- Meerdere NIC VMs moeten worden gemaakt in Azure virtuele netwerken (VNets). VMs niet VNet kan niet worden geconfigureerd met meerdere NIC's.
- Alle VMs in een set beschikbaarheid moeten NIC met één of meerdere NIC gebruiken Er kan een mengsel van meerdere NIC VMs en één NIC VMs binnen een set beschikbaarheid niet. Er gelden dezelfde regels voor VMs in een cloud-service.
- Een VM met één NIC met meerdere NIC's (en vice versa) kan niet worden geconfigureerd nadat deze is geïmplementeerd, zonder te verwijderen en opnieuw te maken.


## <a name="secondary-nics-access-to-other-subnets"></a>Secundaire NIC's toegang tot andere subnetten

Standaard worden secundaire NIC's niet geconfigureerd met een standaard-gateway, als gevolg waarvan de verkeersstroom op de secundaire NIC's beperkt worden in hetzelfde subnet. Als de gebruikers inschakelen, secundaire NIC's wilt praten buiten hun eigen subnet, moet zij een vermelding toevoegen aan de routeringstabel voor de gateway configureren zoals hieronder beschreven.

>[AZURE.NOTE] VMs voordat juli 2015 kan een standaard-gateway geconfigureerd voor alle NIC's hebt gemaakt. De standaard-gateway voor secundaire NIC's niet verwijderd als deze VMs opnieuw worden gestart. Internet-verbinding kunt in besturingssystemen die gebruikmaken van de zwakke routering hostmodel, zoals Linux, verbreken als de ingress- en egress-verkeer verschillende netwerkkaarten gebruikt.

### <a name="configure-windows-vms"></a>Configureer Windows VMs

Stel dat u een Windows VM met twee NIC's als volgt:

- Primaire NIC IP-adres: 192.168.1.4
- Secundaire NIC IP-adres: 192.168.2.5

De IPv4-routeringstabel voor deze VM eruit als volgt:

    IPv4 Route Table
    ===========================================================================
    Active Routes:
    Network Destination        Netmask          Gateway       Interface  Metric
              0.0.0.0          0.0.0.0      192.168.1.1      192.168.1.4      5
            127.0.0.0        255.0.0.0         On-link         127.0.0.1    306
            127.0.0.1  255.255.255.255         On-link         127.0.0.1    306
      127.255.255.255  255.255.255.255         On-link         127.0.0.1    306
        168.63.129.16  255.255.255.255      192.168.1.1      192.168.1.4      6
          192.168.1.0    255.255.255.0         On-link       192.168.1.4    261
          192.168.1.4  255.255.255.255         On-link       192.168.1.4    261
        192.168.1.255  255.255.255.255         On-link       192.168.1.4    261
          192.168.2.0    255.255.255.0         On-link       192.168.2.5    261
          192.168.2.5  255.255.255.255         On-link       192.168.2.5    261
        192.168.2.255  255.255.255.255         On-link       192.168.2.5    261
            224.0.0.0        240.0.0.0         On-link         127.0.0.1    306
            224.0.0.0        240.0.0.0         On-link       192.168.1.4    261
            224.0.0.0        240.0.0.0         On-link       192.168.2.5    261
      255.255.255.255  255.255.255.255         On-link         127.0.0.1    306
      255.255.255.255  255.255.255.255         On-link       192.168.1.4    261
      255.255.255.255  255.255.255.255         On-link       192.168.2.5    261
    ===========================================================================

U ziet de standaardroute (0.0.0.0) is alleen beschikbaar voor de primaire NIC. Niet worden toegang tot bronnen buiten het subnet voor de secundaire NIC, zoals hieronder wordt weergegeven:

    C:\Users\Administrator>ping 192.168.1.7 -S 192.165.2.5

    Pinging 192.168.1.7 from 192.165.2.5 with 32 bytes of data:
    PING: transmit failed. General failure.
    PING: transmit failed. General failure.
    PING: transmit failed. General failure.
    PING: transmit failed. General failure.

Als u wilt toevoegen een standaardroute op de secundaire NIC, volg de onderstaande stappen:

1. Uitvoeren vanaf een opdrachtprompt de opdracht hieronder om het indexnummer voor de tweede Netwerkkaart:

        C:\Users\Administrator>route print
        ===========================================================================
        Interface List
         29...00 15 17 d9 b1 6d ......Microsoft Virtual Machine Bus Network Adapter #16
         27...00 15 17 d9 b1 41 ......Microsoft Virtual Machine Bus Network Adapter #14
          1...........................Software Loopback Interface 1
         14...00 00 00 00 00 00 00 e0 Teredo Tunneling Pseudo-Interface
         20...00 00 00 00 00 00 00 e0 Microsoft ISATAP Adapter #2
        ===========================================================================

2. U ziet het tweede item in de tabel, met een index van 27 (in dit voorbeeld).
3. Uitvoeren vanaf de opdrachtprompt de opdracht **route add** zoals hieronder wordt weergegeven. In dit voorbeeld geeft u 192.168.2.1 als de standaardgateway voor de tweede Netwerkkaart:

        route ADD -p 0.0.0.0 MASK 0.0.0.0 192.168.2.1 METRIC 5000 IF 27

4. Connectiviteit testen terug te keren naar de MS-DOS-prompt en probeert u te pingen een ander subnet bevindt dan de secundaire NIC als weergegeven int eh voorbeeld hieronder:

        C:\Users\Administrator>ping 192.168.1.7 -S 192.165.2.5

        Reply from 192.168.1.7: bytes=32 time<1ms TTL=128
        Reply from 192.168.1.7: bytes=32 time<1ms TTL=128
        Reply from 192.168.1.7: bytes=32 time=2ms TTL=128
        Reply from 192.168.1.7: bytes=32 time<1ms TTL=128

5. U kunt ook de routetabel controleren de toegevoegde route controleren zoals hieronder wordt weergegeven:

        C:\Users\Administrator>route print

        ...

        IPv4 Route Table
        ===========================================================================
        Active Routes:
        Network Destination        Netmask          Gateway       Interface  Metric
                  0.0.0.0          0.0.0.0      192.168.1.1      192.168.1.4      5
                  0.0.0.0          0.0.0.0      192.168.2.1      192.168.2.5   5005
                127.0.0.0        255.0.0.0         On-link         127.0.0.1    306

### <a name="configure-linux-vms"></a>Linux VMs configureren

Voor Linux VMs, omdat het standaardgedrag zwak host routering gebruikt, raden wij aan dat de secundaire NIC's beperkt tot verkeersstromen alleen binnen hetzelfde subnet zijn. Echter, als bepaalde scenario's connectiviteit buiten het subnet gebruikers moeten inschakelen gebaseerd beleid om ervoor te zorgen dat de dezelfde NIC. wordt gebruikt door de ingress- en egress-verkeer routeren

## <a name="next-steps"></a>Volgende stappen

- [VMs in een scenario 2-tier-toepassing in een implementatie Resource Manager MultiNIC](virtual-network-deploy-multinic-arm-template.md)implementeren.
- [VMs in een scenario 2-tier-toepassing in een klassieke implementatie MultiNIC](virtual-network-deploy-multinic-classic-ps.md)implementeren.
