<properties 
   pageTitle="Versnelde toegang voor een virtuele machine - PowerShell | Microsoft Azure"
   description="Informatie over het versnelde netwerken configureren voor een Azure virtuele machine met PowerShell."
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/23/2016"
   ms.author="jdial" />

# <a name="accelerated-networking-for-a-virtual-machine"></a>Versnelde netwerken voor een virtuele machine

> [AZURE.SELECTOR]
- [Azure Portal](virtual-network-accelerated-networking-portal.md)
- [PowerShell](virtual-network-accelerated-networking-powershell.md)

Versnelde netwerken kan één basis-i/o-virtualisatie (SR-IOV) met een virtuele machine (VM), de netwerkprestaties aanzienlijk verbeteren. Dit pad krachtige omzeilt de host van het gegevenspad latentie en jitter voor gebruik met de meest veeleisende werklast netwerk op ondersteunde VM typen CPU-gebruik verminderen. In dit artikel wordt uitgelegd hoe u met Azure PowerShell versnelde netwerken in het implementatiemodel Azure Resource Manager configureren. U kunt ook een VM maken met versnelde toegang met behulp van de Portal Azure. Voor meer informatie over hoe, klik in het vak Azure Portal aan het begin van dit artikel.

In de volgende afbeelding ziet u de communicatie tussen de twee virtuele machines (VM) met en zonder versnelde toegang:

![Vergelijking](./media/virtual-network-accelerated-networking-powershell/image1.png)

Alle netwerkverkeer van en naar de VM moet zonder versnelde netwerken via de host en de virtual switch. De virtual switch biedt alle afdwingen, zoals netwerk-beveiligingsgroepen, access control lists, isolatie en andere gevirtualiseerde services voor het netwerkverkeer. Meer informatie leest u het artikel [Hyper-V-netwerk virtualisatie en Virtual Switch](https://technet.microsoft.com/library/jj945275.aspx) .

Versnelde netwerken, netwerkverkeer aankomt bij de netwerkkaart (NIC) en wordt vervolgens doorgestuurd naar de VM. Alle netwerkbeleid dat de virtual switch wordt toegepast zonder versnelde netwerken zijn overgenomen en toegepast in de hardware. Toepassen van beleid in hardware, kunt de NIC voor doorsturen van netwerkverkeer rechtstreeks naar de VM, overslaan van de host en de virtual switch, terwijl het beleid die het toegepast in de host.

De voordelen van versnelde toegang zijn alleen van toepassing op de VM die is ingeschakeld. Voor de beste resultaten is het ideaal om deze functie inschakelen voor ten minste twee VMs verbonden met de dezelfde VNet.  Tijdens het communiceren via VNets of aansluitende op gebouwen, heeft deze functie een minimale invloed op de algehele latentie.

[AZURE.INCLUDE [virtual-network-preview](../../includes/virtual-network-preview.md)]

##<a name="benefits"></a>Voordelen

- **Lagere latentie / hogere pakketten per seconde (pps):** De virtual switch van het gegevenspad verwijderen verwijdert de tijd pakketten besteden in de host voor de verwerking van beleid en neemt het aantal pakketten dat kan worden verwerkt binnen de VM.
- **Verlaagd jitter:** Virtual switch verwerking, is afhankelijk van het bedrag van het beleid dat moet worden toegepast en de belasting van de CPU die bezig is met de verwerking. Het afdwingen van beleid aan de hardware-offloading verwijdert die variabiliteit door het afleveren van pakketten rechtstreeks naar de VM, verwijderen van de host de VM-communicatie- en alle software-interrupts en Contextomschakelingen.
- **Verminderd CPU-gebruik:** De virtual switch in de host overslaat leidt tot minder CPU-gebruik voor het verwerken van netwerkverkeer.

## <a name="limitations"></a>Beperkingen

De volgende beperkingen bestaan bij het gebruik van deze mogelijkheid:
 
- **Network interface maken:** Versnelde netwerken kan alleen worden ingeschakeld voor een nieuwe netwerkinterface.  Deze kan niet worden ingeschakeld op een bestaand netwerkinterface.
- **VM maken:** Een netwerkinterface met versnelde netwerken ingeschakeld kan alleen worden gekoppeld aan een VM wanneer de VM wordt gemaakt. De netwerk-interface kan niet worden gekoppeld aan een bestaande VM.
- **Regio's:** Aangeboden in alleen de regio West Centraal US en West-Europa Azure. De set van de regio's wordt in de toekomst uitgebreid.
- **Ondersteund besturingssysteem:** Microsoft Windows Server 2012 R2 en Windows Server 2016 technische voorbeeld 5. Ondersteuning voor Linux en Windows Server 2012 zal spoedig worden toegevoegd.
- **VM-grootte:** Standard_D15_v2 en Standard_DS15_v2 zijn dat de enige ondersteunde formaten voor VM-exemplaar. Zie het artikel [Windows VM formaten](../virtual-machines/virtual-machines-windows-sizes.md) voor meer informatie. Het aantal ondersteunde formaten voor VM-exemplaar wordt in de toekomst uitgebreid.

Wijzigingen in deze beperkingen wordt aangekondigd via de pagina [updates Azure virtuele netwerken](https://azure.microsoft.com/updates/accelerated-networking-in-preview) .

## <a name="create-a-windows-vm-with-accelerated-networking"></a>Maak een Windows VM met versnelde netwerken

1. Open een PowerShell-opdrachtprompt en voer de overige stappen in deze sectie binnen een PowerShell-sessie. Als u nog geen PowerShell is geïnstalleerd en geconfigureerd, de stappen in het artikel voor [het installeren en configureren van Azure PowerShell](../powershell-install-configure.md) .
2. Om te registreren voor de voorvertoning, kunt u een e-mail sturen naar [Abonnementen voor versnelde toegang](mailto:axnpreview@microsoft.com?subject=Request%20to%20enable%20subscription%20%3csubscription%20id%3e) met uw abonnement-ID en gebruik bedoeld. Voer de overige stappen tot niet nadat u een e-mailbericht de mededeling dat u hebt geaccepteerd in de voorvertoning hebt ontvangen.
3. De mogelijkheid uw abonnement registreren door de volgende opdrachten:

        Register-AzureRmProviderFeature -FeatureName AllowAcceleratedNetworkingFeature -ProviderNamespace Microsoft.Network
        Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network

4. *Westcentralus* vervangen door de naam van een andere locatie wordt ondersteund door deze mogelijkheid vermeld in de sectie [beperkingen](#limitations) van dit artikel (indien gewenst). Voer de volgende opdracht om een variabele voor de locatie instellen:

        $locName = "westcentralus"

5. *RG1* vervangen door een naam voor de resourcegroep die bevatten de nieuwe netwerkinterface van het en voer de volgende opdrachten om deze te maken:

        $rgName = "RG1"
        New-AzureRmResourceGroup -Name $rgName -Location $locName

6. *VM1 NIC1* wijzigen op de gewenste naam voor de netwerkinterface en voer vervolgens de volgende opdracht:

        $NICName = "VM1-NIC1"

7. De netwerkinterface moet worden verbonden met een subnet binnen een bestaande Azure Virtual Network (VNet) in dezelfde locatie en [abonnement](../azure-glossary-cloud-terminology.md#subscription) als de netwerkinterface. Meer informatie over VNets door het [virtuele netwerk overzicht](virtual-networks-overview.md) artikel lezen als u niet bekend mee bent. Een VNet maken, voltooi de stappen in het artikel [een VNet maken](virtual-networks-create-vnet-arm-ps.md) . 'Waarden' van de volgende $Variables op de naam van de VNet en het subnet dat u wilt verbinding maken met de netwerkinterface te wijzigen.

        $VNetName   = "VNet1"
        $SubnetName = "Subnet1"

    Als u niet de naam van een bestaande VNet op de locatie die u in stap 3 hebt gekozen weet, voert u de volgende opdrachten:
        
        $VirtualNetworks = Get-AzureRmVirtualNetwork
        $VirtualNetworks | Where-Object {$_.Location -eq $locName} | Format-Table Name, Location
        
    Als de lijst die wordt teruggezonden leeg is, moet u een VNet maken in de locatie. Een VNet maken, voltooi de stappen in het artikel [een virtueel netwerk maken](virtual-networks-create-vnet-arm-ps.md) .

    Typ de volgende opdrachten als u de naam van de subnetten binnen de VNet, en *Subnet1* hierboven vervangen door de naam van een subnet:
        
        $VNet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $rgName
        $VNet.Subnets | Format-Table Name, AddressPrefix

8. Voer de volgende opdrachten voor het ophalen van de VNet en het subnetmasker en deze toewijzen aan variabelen.

        $VNet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $rgName
        $Subnet = $VNet.Subnets | Where-Object { $_.Name -eq $SubnetName }

9. Identificeren van een bestaande openbare IP-adresbron die zijn gekoppeld aan de netwerkinterface kunt u deze via het Internet. Als u niet dat de VM met de netwerkinterface toegang via het Internet wilt, kunt u deze stap overslaan. Zonder een openbaar IP-adres, moet u de VM uit een andere VM verbonden met de dezelfde VNet. 

    *PIP1* op de naam van een bestaande openbare IP-adresbron die bestaat op de locatie als u de netwerkinterface in en dat is gekoppeld aan een ander netwerk-interface niet wijzigen. Wijzig zo nodig $rgName op de naam van de resourcegroep de openbare IP-adresbron bestaat en voer de volgende opdracht:

        $PIP1 = Get-AzureRmPublicIPAddress -Name "PIP1" -ResourceGroupName $rgName

    Als u de naam van een bestaande bron van openbare IP-adres niet weet, voert u de volgende opdrachten:

        $PublicIPAddresses = Get-AzureRmPublicIPAddress
        $PublicIPAddresses | Where-Object { $_.Location -eq $locName } |Format-Table Name, Location, IPAddress, IpConfiguration

    Als de kolom **IPConfiguration** geen waarde in de uitvoer die wordt geretourneerd heeft, wordt de openbare IP-adresbron niet is gekoppeld aan een bestaand netwerkinterface en kan worden gebruikt. Als de lijst leeg is, of er geen bronnen beschikbaar openbaar IP-adres zijn, kunt u maken met de opdracht Nieuw AzureRmPublicIPAddress.

    >[AZURE.NOTE] Openbare IP-adressen hebben een nominale vergoeding. Meer informatie over het [IP-adres prijzen](https://azure.microsoft.com/pricing/details/ip-addresses) de pagina prijzen.
10. Als u ervoor niet de bron van een openbaar IP-adres toevoegen aan de interface kiest, verwijdert u *- PublicIPAddress $PIP1* aan het einde van de opdracht die volgt. Maak de netwerkinterface met versnelde netwerken door te voeren met de volgende opdracht:

        $nic = New-AzureRmNetworkInterface -Location $locName -Name $NICName -ResourceGroupName $rgName -Subnet $Subnet -EnableAcceleratedNetworking -PublicIpAddress $PIP1 

11. De netwerkinterface aan een VM toewijzen bij het maken van de VM door de instructies in stap 3 en 6 van het artikel [een VM maken](../virtual-machines/virtual-machines-windows-ps-create.md) . Vervang *Standard_A1* met een van de VM-formaten die in de sectie [beperkingen](#limitations) van dit artikel worden vermeld in stap 6-2.

    >[AZURE.NOTE] Als u de *naam* van de $locName, $rgName of $nic variabelen in dit artikel hebt gewijzigd mislukt in stap 6 in het maken van een artikel VM. U kunt echter de *waarden* van variabelen wijzigen.

12. Zodra de VM is gemaakt, downloadt u het [stuurprogramma voor versnelde toegang](https://gallery.technet.microsoft.com/Azure-Accelerated-471b5d84)verbinding maken met de VM en voert u het installatieprogramma van het stuurprogramma in de VM.

13. Klik met de rechtermuisknop op de Windows-knop en klik op **Apparaatbeheer**. Controleer dat de **Mellanox ConnectX 3 virtuele functie Ethernet-Adapter** wordt weergegeven onder de optie **netwerk** als het is uitgevouwen, zoals in de volgende afbeelding:

    ![Apparaatbeheer](./media/virtual-network-accelerated-networking-powershell/image2.png)