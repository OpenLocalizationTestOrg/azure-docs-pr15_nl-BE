<properties
   pageTitle="Meerdere IP-adressen voor virtuele machines - PowerShell | Microsoft Azure"
   description="Informatie over meerdere IP-adressen toewijzen aan een virtuele machine met Azure PowerShell."
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
   ms.date="10/05/2016"
   ms.author="jdial;annahar" />


# <a name="assign-multiple-ip-addresses-to-virtual-machines"></a>Meerdere IP-adressen toewijzen aan virtuele machines

Een Azure Virtual Machine (VM) hebben een of meer netwerkinterfaces (NIC) is gekoppeld. De NIC hebben een of meer openbare of particuliere IP-adressen zijn toegewezen. Als u niet vertrouwd met IP-adressen in Azure bent, Lees artikel voor meer informatie over deze [IP-adressen in Azure](virtual-network-ip-addresses-overview-arm.md) . In dit artikel wordt uitgelegd hoe Azure PowerShell gebruiken meerdere IP-adressen toewijzen aan een VM in Azure Resource Manager-implementatiemodel.

Meerdere IP-adressen toewijzen aan een VM, kunt de volgende mogelijkheden:

- Host meerdere websites of services met verschillende IP-adressen en SSL-certificaten op één server.
- Fungeren als een virtueel netwerkapparaat, zoals een firewall of de belasting voor documentconversies.
- De mogelijkheid om een van de particuliere IP-adressen voor een van de NIC's toevoegen aan een Azure Load Balancer back-end-toepassingen. In het verleden, kan alleen het primaire IP-adres voor de primaire Netwerkkaart worden toegevoegd aan een groep met back-end.

[AZURE.INCLUDE [virtual-network-preview](../../includes/virtual-network-preview.md)]

Om te registreren voor de voorvertoning, kunt u een e-mail sturen naar [Meerdere IP -adressen](mailto:MultipleIPsPreview@microsoft.com?subject=Request%20to%20enable%20subscription%20%3csubscription%20id%3e) met uw abonnement-ID en gebruik bedoeld.

## <a name="scenario"></a>Scenario

In dit artikel koppelt u drie IP-configuraties voor een netwerkinterface.
Het volgende voorbeeldconfiguraties worden gemaakt en toegewezen aan een Netwerkkaart die drie particuliere IP-adressen één openbaar IP-adres toegewezen en:

- IPConfig-1: Een dynamische particuliere IP-adres (standaard) en een openbaar IP-adres uit de openbare IP-adresbron met de naam PIP1.
- IPConfig-2: Een statische particulier IP-adres en geen openbare IP-adres.
- IPConfig-3: Een dynamische particuliere IP-adres en geen openbare IP-adres.

    ![Met ALT-afbeeldingstekst](./media/virtual-network-multiple-ip-addresses-powershell/OneNIC-3IP.png)

In dit scenario wordt ervan uitgegaan dat er een resourcegroep *RG1* waarbinnen er is een VNet genaamd *VNet1* en een subnet genaamd *Subnet1*genoemd. Verder wordt verondersteld u hebt een VM *VM1*genoemd en een netwerkinterface *VM1 NIC1* aangeroepen die is gekoppeld aan een openbaar IP-adres *PIP1*genoemd.

[Dit artikel](./virtual-machines/virtual-machines-windows-ps-create.md ) helpt bij het maken van de middelen die hiervoor zijn beschreven in het geval u deze hebt gemaakt.

## <a name = "create"></a>Een VM maken met meerdere IP-adressen

1. Open een PowerShell-opdrachtprompt en voer de overige stappen in deze sectie binnen een PowerShell-sessie. Als u nog geen PowerShell is geïnstalleerd en geconfigureerd, de stappen in het artikel voor [het installeren en configureren van Azure PowerShell](../powershell-install-configure.md) .

2. Wijzigen in 'waarden' van de volgende $Variables de Azure [locatie](https://azure.microsoft.com/regions) die het virtuele netwerk is in de naam van de [resourcegroep](../azure-resource-manager/resource-group-overview.md#resource-groups), de VNet in de resourcegroep, het subnet dat u wilt dat de Netwerkkaart om verbinding te maken en de naam van de netwerkadapterkaart. Voltooi de stappen voor het toevoegen van meerdere IP-adressen voor elk gekoppeld aan een VM, NIC als nodig is.

        $Location = "westcentralus"
        $RgName   = "RG1"
        $VNetName   = "VNet1"
        $SubnetName = "Subnet1"
        $NicName     = "VM1-NIC1"
        $PIP = "PIP"

    Als u de naam van een bestaande Azure locatie of groep niet weet, typt u de volgende opdrachten:

        Get-AzureRmLocation      | Format-Table Location
        Get-AzureRmResourceGroup | Format-Table ResourceGroupName

    <a name="subnet"></a>De Netwerkkaart moet worden verbonden met een subnet binnen een bestaande Azure Virtual Network (VNet). De drie onderdelen: NIC-, subnet- en VNet moet al aanwezig zijn in dezelfde regio en [abonnement](../azure-glossary-cloud-terminology.md#subscription).  Als u niet vertrouwd met VNets bent, Lees artikel voor meer informatie over hen of lees het artikel [een VNet maken](virtual-networks-create-vnet-arm-ps.md) voor informatie over het maken van een [virtueel netwerk-overzicht](virtual-networks-overview.md) .

    Als u de naam van een bestaande VNet niet weet, voert u de volgende opdracht en *VNet1* in de vorige variabele vervangen door de naam van een VNet:

        Get-AzureRmVirtualNetwork | Format-Table Name

    Als de lijst die wordt teruggezonden leeg is, moet u een VNet maken. Voor meer informatie over hoe, lees het artikel [een virtueel netwerk maken](virtual-networks-create-vnet-arm-ps.md) .

    Typ de volgende opdrachten op de naam van de bestaande subnetten binnen de VNet en *Subnet1* hierboven vervangen door de naam van een subnet:

        $VNet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RgName
        $VNet.Subnets | Format-Table Name, AddressPrefix

4. Voer de volgende opdracht voor het ophalen van het subnet en deze toewijzen aan een variabele.

        $Subnet = $VNet.Subnets | Where-Object { $_.Name -eq $SubnetName }

5. <a name="ipconfigs"></a>Definieer de IP-configuraties die u wilt toewijzen aan de NIC. Elke configuratie kan een statische of dynamische particuliere IP-adres en een openbaar IP-adresbron die is gekoppeld aan een adres statisch of dynamisch.

    Toevoegen of verwijderen van een aantal van de configuraties die volgen hoeveel IP-adressen die u wilt koppelen aan de Netwerkkaart en de instellingen die u wilt configureren.

    **IPConfig-1**

    Wijzig de waarde *PIP1* in de naam van een bestaande openbare IP-adresbron die bestaat op de locatie die als u de Netwerkkaart in en die niet is gekoppeld aan een andere netwerkadapterkaart. Wijzigen, *RG1* op de naam van de resourcegroep de openbare IP-adresbron aanwezig in. *IPConfig 1* wijzigen in de naam die u wilt toekennen aan de eerste IP-configuratie. Voer de volgende opdrachten:

        $PIP1 = Get-AzureRmPublicIPAddress -Name "PIP1" -ResourceGroupName $RgName

        $IpConfigName1 = "IPConfig-1"
        $IPConfig1     = New-AzureRmNetworkInterfaceIpConfig -Name $IPConfigName1 -Subnet $Subnet -PublicIpAddress $PIP1 -Primary

    Opmerking de *-primaire* overschakelen. Als u meerdere IP-configuraties aan een Netwerkkaart toewijst, moet een configuratie worden toegewezen als de *primaire*. Als u de naam van een bestaande bron van openbare IP-adres niet weet, voert u de volgende opdracht: Get-AzureRMPublicIPAddress | Tabel opmaken, naam, locatie, IP-adres, IpConfiguration

    Als de kolom **IPConfiguration** geen waarde in de uitvoer die wordt geretourneerd heeft, wordt de openbare IP-adresbron niet is gekoppeld aan een bestaande NIC en kan worden gebruikt. Als de lijst leeg is, of er geen bronnen beschikbaar openbaar IP-adres zijn, kunt u maken met de opdracht **Nieuw AzureRmPublicIPAddress** .

    >[AZURE.NOTE] Openbare IP-adressen hebben een nominale vergoeding. Lees voor meer informatie over prijzen van IP-adres, het [IP-adres prijzen](https://azure.microsoft.com/pricing/details/ip-addresses) pagina.

    **IPConfig-2**

    *IPConfig-2* op de naam die u wilt toekennen aan de tweede IP-configuratie en *10.0.0.5* wijzigen in een ongebruikte geldig IP-adres voor het subnet dat u bij het toewijzen van de NIC te wijzigen. Voer de volgende opdrachten:

        $IPConfigName2 = "IPConfig-2"
        $IPAddress = 10.0.0.5

        $IPConfig2 = New-AzureRmNetworkInterfaceIpConfig -Name $IPConfigName2 -Subnet $Subnet -PrivateIpAddress $IPAddress

    Voer de volgende opdracht als u niet weet het IP wat adres toegewezen aan het subnet:

        $VNet.Subnets | Format-Table Name, AddressPrefix

    **IPConfig-3**

    *IPConfig 3* wijzigen op de naam die u wilt toekennen aan de derde IP-configuratie en voer de volgende opdrachten:

        $IPConfigName3 = "IPConfig-3"
        $IPConfig3 = New-AzureRmNetworkInterfaceIpConfig -Name $IPConfigName3 -Subnet $Subnet

    >[AZURE.NOTE] U kunt maximaal 250 particulier IP-adres toewijzen aan een netwerkadapterkaart. Er is een limiet aan het aantal openbare IP-adressen die kunnen worden gebruikt binnen een abonnement. Lees voor meer informatie het artikel [Azure beperkt](../azure-subscription-service-limits.md#networking-limits---azure-resource-manager) .

6. Maak de NIC met behulp van de IP-configuraties die zijn gedefinieerd in de vorige stap.

        $nic = New-AzureRmNetworkInterface -Name $NicName -ResourceGroupName $RgName -Location $Location -IpConfiguration $IpConfig1,$IpConfig2,$IpConfig3

7. De NIC toevoegen wanneer een VM maken volgens de stappen in het artikel [een VM maken](../virtual-machines/virtual-machines-windows-ps-create.md) . Hoewel het artikel een VM waarop Windows Server wordt uitgevoerd maakt, zijn de stappen hetzelfde voor een Linux VM, selecteren voor een ander besturingssysteem. Stappen 1-3 van het artikel. Stap 4 en 5 en vervolgens volledige stap 6 in het maken van een VM-artikel overslaan.

    >[AZURE.WARNING] Stap 6 in het maken van een artikel VM mislukt als u de variabele met de naam $nic in iets anders in stap 6 van dit artikel wordt gewijzigd of de vorige stappen in dit artikel niet zijn voltooid.

8. Weergave van het persoonlijke IP adressen die Azure DHCP toegewezen aan de Netwerkkaart en het openbare IP-adresbron die is toegewezen aan de Netwerkadapter door te voeren met de volgende opdracht:

        $nic.IpConfigurations | Format-Table Name, PrivateIPAddress, PublicIPAddress, Primary

9. <a name="os"></a>Alle particuliere IP-adressen (met inbegrip van de primaire) handmatig toevoegen aan de TCP/IP-configuratie in het besturingssysteem. 

**Windows**

1. Vanaf een opdrachtprompt, typ *ipconfig/all*.  U ziet alleen de *primaire* particuliere IP-adres (via DHCP).
2. Vervolgens typt u *ncpa.cpl* in het opdrachtpromptvenster. Een nieuw venster wordt geopend.
3. Open de eigenschappen voor **LAN-verbinding**.
4. Dubbelklik op Internet Protocol versie 4 (IPv4)
5. Selecteer **het volgende IP-adres gebruiken** en voer de volgende waarden:
    - **IP-adres**: Geef het *primaire* particuliere IP-adres
    - **Subnetmasker**: instellen op basis van uw subnet. Als het subnet is een /24 bijvoorbeeld subnet vervolgens het subnetmasker is 255.255.255.0.
    - **Standaard-gateway**: het eerste IP-adres in het subnet. Als uw subnet 10.0.0.0/24 is, is het gateway-IP-adres 10.0.0.1.
    - Klik op **de volgende DNS-serveradressen gebruiken** en voer de volgende waarden:
        - **Voorkeurs-DNS-server:** 168.63.129.16 invoeren als u uw eigen DNS-server niet gebruikt.  Als u bent, voert u het IP-adres voor uw DNS-server.
    - Klik op de knop **Geavanceerd** en extra IP-adressen toe te voegen. Elk van de secundaire particuliere IP-adressen die worden vermeld in stap 8 op de Netwerkinterfacekaart met hetzelfde subnetmasker opgegeven voor het primaire IP-adres toevoegen.
    - Klik op **OK** om af te sluiten van de TCP/IP-instellingen en klik vervolgens op **OK** om te sluiten van de instellingen voor de adapter. Vervolgens wordt dit door de RDP-verbinding hersteld.

6. Vanaf een opdrachtprompt, typ *ipconfig/all*. Alle IP-adressen die u hebt toegevoegd, worden weergegeven en DHCP is uitgeschakeld.


**Linux (Ubuntu)**

1. Open een terminal-venster.
2. Zorg ervoor dat u de hoofdgebruiker bent. Als u niet bent, kunt u dit doen door de volgende opdracht:

            sudo -i
3. Het configuratiebestand van de netwerkinterface (ervan uitgaande dat 'eth0') bijwerken.
    - Houd het bestaande artikel voor dhcp. Hiermee wordt het primaire IP-adres configureren zoals het moet eerder zijn gebruikt.
    - Voeg een configuratie voor een meer statische IP-adres met de volgende opdrachten:

                cd /etc/network/interfaces.d/
                ls

        U ziet een CFG-bestand.
4. Open het bestand: vi- *bestandsnaam*.

    Hier ziet u de volgende regels aan het einde van het bestand:

            auto eth0
            iface eth0 inet dhcp
5. Voeg de volgende regels na de regels die in dit bestand bestaat:

            iface eth0 inet static
            address <your private IP address here>
6. Sla het bestand met de volgende opdracht:

            :wq
7.  Opnieuw instellen van de netwerkinterface met de volgende opdracht:

            sudo ifdown eth0 && sudo ifup eth0

    >[AZURE.IMPORTANT] Uitgevoerd zowel ifdown als ifup op dezelfde regel als een externe verbinding.
8. Controleer of dat het IP-adres is toegevoegd aan de network interface met de volgende opdracht:

            ip addr list eth0

    Hier ziet u het IP-adres dat u als onderdeel van de lijst toegevoegd.

**Linux (Redhat, CentOS en anderen)**

1. Open een terminal-venster.
2. Zorg ervoor dat u de hoofdgebruiker bent. Als u niet bent, kunt u dit doen door de volgende opdracht:

            sudo -i
3. Typ uw wachtwoord en volg de instructies als u wordt gevraagd. Als u de hoofdgebruiker bent, Ga naar de scriptmap met de volgende opdracht:

            cd /etc/sysconfig/network-scripts
4. Een overzicht van de bijbehorende ifcfg-bestanden met de volgende opdracht:

            ls ifcfg-*

    *Ifcfg-eth0* te zien als een van de bestanden.
5. Kopieer het bestand *ifcfg-eth0* en geef deze de naam *ifcfg-eth0:0* met de volgende opdracht:

            cp ifcfg-eth0 ifcfg-eth0:0
6. Bewerkt de *ifcfg-eth0:0* -bestand met de volgende opdracht:

            vi ifcfg-eth1
7. Het apparaat wijzigen in de gewenste naam in het bestand. *eth0:0* in dit geval, met de volgende opdracht:

            DEVICE=eth0:0
8. Wijziging de *IPADDR = YourPrivateIPAddress* lijn om het IP-adres weer te geven.
9. Sla het bestand met de volgende opdracht:

            :wq
10. De netwerkservices opnieuw starten en controleer of dat de wijzigingen zijn uitgevoerd door de volgende opdrachten:

            /etc/init.d/network restart
            Ipconfig

    Hier ziet u het IP-adres dat u toegevoegd, *eth0:0*in de lijst die wordt teruggezonden.

## <a name="add"></a>IP-adressen toevoegen aan een bestaande VM

Voer de volgende stappen uit om IP-adressen toevoegen voor een bestaande NIC:

1. Open een PowerShell-opdrachtprompt en voer de overige stappen in deze sectie binnen een PowerShell-sessie. Als u nog geen PowerShell is geïnstalleerd en geconfigureerd, de stappen in het artikel voor [het installeren en configureren van Azure PowerShell](../powershell-install-configure.md) .

2. De naam van de Netwerkkaart toe te voegen IP-adressen en de resourcegroep en locatie komt voor in de NIC wijzigen in 'waarden' van de volgende $Variables:

        $NicName     = "RG1-VM1-NIC1"
        $RgName   = "RG1"
        $NicLocation = "westcentralus"

    Als u de naam van de Netwerkadapter die u wilt wijzigen, voert u de volgende opdrachten niet weet, wijzig de waarden van de vorige variabelen:

        Get-AzureRmNetworkInterface | Format-Table Name, ResourceGroupName, Location

3. Een variabele maken en stel deze in op de NIC bestaande door de volgende opdracht te typen:

        $nic = Get-AzureRmNetworkInterface -Name $NicName -ResourceGroupName $RgName

4. De subnet-ID op de Netwerkkaart is aangesloten door het uitvoeren van [stap 3](#subnet) van het maken van een VM met meerdere IP-adressen sectie van dit artikel worden opgehaald.

5. Maak de IP-configuraties die u toevoegen aan het netwerk wilt door de instructies in [stap 5](#ipconfigs) van de maken een VM met meerdere IP-adressen sectie van dit artikel.

6. *$IPConfigName4* wijzigen in de naam van de IP-configuratie die u in de vorige stap hebt gemaakt. Als u de configuratie wilt toevoegen, typ de volgende opdracht:

        Add-AzureRmNetworkInterfaceIpConfig -Name $IPConfigName4 -NetworkInterface $nic -Subnet $Subnet1

7. Als u wilt dat de Netwerkkaart met de IP-configuratie, voer de volgende opdracht:

        Set-AzureRmNetworkInterface -NetworkInterface $nic

8. Voeg het IP-adressen die u hebt toegevoegd aan de Netwerkkaart voor het besturingssysteem VM door de instructies in [stap 9](#os) van het maken van een VM met meerdere IP-adressen sectie van dit artikel.
