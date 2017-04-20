<properties 
   pageTitle="Problemen oplossen met routes - PowerShell | Microsoft Azure"
   description="Informatie over het oplossen van problemen met de routes in de bronnenbeheerder Azure implementatiemodel met Azure PowerShell."
   services="virtual-network"
   documentationCenter="na"
   authors="AnithaAdusumilli"
   manager="narayan"
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
   ms.author="anithaa" />

# <a name="troubleshoot-routes-using-azure-powershell"></a>Problemen oplossen met Azure PowerShell routes

> [AZURE.SELECTOR]
- [Azure Portal](virtual-network-routes-troubleshoot-portal.md)
- [PowerShell](virtual-network-routes-troubleshoot-powershell.md)

Als u ondervindt problemen met de netwerkverbinding aan of uit uw Azure Virtual Machine (VM), kunnen routes de verkeersstromen VM beïnvloeden. Dit artikel biedt een overzicht van de mogelijkheden voor routes om verdere diagnose.

Routetabellen zijn gekoppeld aan de subnetten en geldig zijn op alle netwerkinterfaces (NIC) in dat subnet. De volgende typen routes kunnen worden toegepast op elke netwerkinterface:

- **Systeem routes:** Elk subnet dat is gemaakt in een virtueel netwerk van Azure (VNet), heeft standaard route systeemtabellen voor lokale VNet-verkeer, verkeer via de VPN-gateways op gebouwen en Internet-verkeer toestaan. Er zijn ook routes systeem voor peered VNets.
- **BGP routes:** Doorgegeven aan netwerkinterfaces via ExpressRoute of VPN-verbindingen van site naar site. Meer informatie over het BGP door het lezen van de artikelen [ExpressRoute overzicht](../expressroute/expressroute-introduction.md) en [BGP met gateways voor VPN-](../vpn-gateway/vpn-gateway-bgp-overview.md) routering.
- **Gebruiker gedefinieerde routes (UDR):** Als u toestellen voor virtuele netwerken of zijn gedwongen tunneling-verkeer naar een op gebouwen-netwerk via een VPN website, wellicht door de gebruiker gedefinieerde routes (UDRs) die is gekoppeld aan de routetabel van uw subnet. Als u niet vertrouwd met UDRs bent, lees het artikel [door de gebruiker gedefinieerde routes](virtual-networks-udr-overview.md#user-defined-routes) .

Met de verschillende routes die kunnen worden toegepast op een netwerk-interface, kan het lastig zijn om te bepalen welke statistische routes zijn van kracht. Om te helpen oplossen van problemen met netwerkverbindingen VM, kunt u de effectieve routes voor een netwerkinterface weergeven in het implementatiemodel Azure Resource Manager.

## <a name="using-effective-routes-to-troubleshoot-vm-traffic-flow"></a>Problemen met netwerkverkeer VM met effectieve Routes

In dit artikel wordt het volgende scenario als voorbeeld om te illustreren hoe u problemen met de effectieve routes voor een netwerkinterface:

Een VM (*VM1*) verbonden met de VNet (*VNet1*, voorvoegsel: 10.9.0.0/16) geen verbinding met een VM(VM3) in een nieuw peered VNet (*VNet3*, voorvoegsel 10.10.0.0/16). Er zijn geen UDRs of BGP routeert toegepast op VM1 NIC1 netwerkinterface is aangesloten op de VM, alleen de routes van het systeem worden toegepast.

In dit artikel wordt uitgelegd hoe u de oorzaak van de mislukte verbinding met de mogelijkheid effectieve routes in Azure Resource Management implementatiemodel.
Terwijl in het voorbeeld alleen de routes van het systeem wordt, kunnen dezelfde stappen inkomende en uitgaande verbindingsfouten vaststellen over elk routetype worden gebruikt.

>[AZURE.NOTE] Als uw VM meer dan één Netwerkkaart is aangesloten heeft, controleert u de effectieve routes voor elk van de NIC's voor het vaststellen van problemen met de netwerkverbinding en naar een VM.

### <a name="view-effective-routes-for-a-virtual-machine"></a>De effectieve routes weergeven voor een virtuele machine

Overzicht van de statistische routes die worden toegepast op een VM de volgende stappen:

### <a name="view-effective-routes-for-a-network-interface"></a>De effectieve routes voor een netwerkinterface weergeven

Overzicht van de statistische routes die worden toegepast op een netwerk-interface, moet u de volgende stappen uitvoeren:

1.  Start een sessie Azure PowerShell en Azure-aanmelding. Als u niet vertrouwd met Azure PowerShell bent, lees het artikel voor [het installeren en configureren van Azure PowerShell](../powershell-install-configure.md) .

2.  De volgende opdracht retourneert alle routes die op een netwerk-interface met de naam *VM1 NIC1* in de resourcegroep *RG1*toegepast.

        Get-AzureRmEffectiveRouteTable -NetworkInterfaceName VM1-NIC1 -ResourceGroupName RG1

    >[AZURE.TIP] Als u de naam van een netwerkinterface niet weet, typt u de volgende opdracht om de namen van alle netwerkinterfaces in een resource-group.*

        Get-AzureRmNetworkInterface -ResourceGroupName RG1 | Format-Table Name

    De volgende uitvoer lijkt op de uitvoer voor elke route die wordt toegepast op het subnet dat de Netwerkkaart is verbonden met:

        Name :
        State : Active
        AddressPrefix : {10.9.0.0/16}
        NextHopType : VNetLocal
        NextHopIpAddress : {}

        Name :
        State : Active
        AddressPrefix : {0.0.0.0/16}
        NextHopType : Internet
        NextHopIpAddress : {}

    U ziet in de uitvoer als volgt:
    - **Naam**: naam van de effectieve route mogelijk leeg, tenzij expliciet vermeld, voor door de gebruiker gedefinieerde routes. 
    - **Status**: geeft de status van de effectieve route. Mogelijke waarden zijn 'Actief' of 'Ongeldig'
    - **AddressPrefixes**: Hiermee geeft u het adresvoorvoegsel van de effectieve route in de CIDR-notatie. 
    - **nextHopType**: geeft aan dat de volgende hop voor de opgegeven route. Mogelijke waarden zijn *VirtualAppliance*, *Internet*, *VNetLocal*, *VNetPeering*of *Null*. De waarde *Null* voor **nextHopType** in een UDR mogelijk ongeldig. Bijvoorbeeld als **nextHopType** *VirtualAppliance* en het virtuele netwerk is toestel VM niet in staat ingericht/uitgevoerd. Als er geen gateway ingericht/uitgevoerd in de opgegeven VNet is **nextHopType** *VPNGateway* is kan de route ongeldig worden.
    - **NextHopIpAddress**: het IP-adres van de volgende hop van de effectieve route.
    
    De routes resulteert met de volgende opdracht in een gemakkelijker tabel weergeven:

        Get-AzureRmEffectiveRouteTable -NetworkInterfaceName VM1-NIC1 -ResourceGroupName RG1 | Format-Table

    De volgende uitvoer is deel van de uitvoer ontvangen voor het eerder beschreven scenario:

        Name State AddressPrefix NextHopType NextHopIpAddress
        ---- ----- ------------- ----------- ----------------
        Active {10.9.0.0/16} VnetLocal {}
        Active {0.0.0.0/0} Internet {}
    

3. Er is geen route vermeld op de *WestUS VNet3* VNet (voorvoegsel 10.10.0.0/16)* * van *WestUS VNet1* (voorvoegsel 10.9.0.0/16) in de uitvoer uit de vorige stap. Zoals in de volgende afbeelding, is de peering VNet-koppeling met de VNet *WestUS VNet3* in de status *verbroken* .
    
    ![](./media/virtual-network-routes-troubleshoot-portal/image4.png)

    De bi-directionele koppeling voor de peering verbroken is, waarin wordt uitgelegd waarom VM1 kan geen verbinding maken met VM3 in de VNet *WestUS VNet3* . Een bi-directionele VNet peering koppeling opnieuw instellen voor *WestUS VNet1* en *WestUS VNet3* VNets. Volgt u de uitvoer die wordt geretourneerd nadat de VNet peering koppeling correct is ingesteld:

        Name State AddressPrefix NextHopType NextHopIpAddress
        ---- ----- ------------- ----------- ----------------
        Active {10.9.0.0/16} VnetLocal {}
        Active {10.10.0.0/16} VNetPeering {}
        Active {0.0.0.0/0} Internet {}
        
    Zodra u hebt vastgesteld dat het probleem, u kunt toevoegen, verwijderen, of wijzigen van routes en routeren van tabellen. Typ de volgende opdracht om een overzicht van de opdrachten te doen:

        Get-Help *-AzureRmRouteConfig

## <a name="considerations"></a>Overwegingen met betrekking tot

Een aantal zaken rekening moet houden bij het controleren van de lijst met routes die worden geretourneerd:

- Routering is gebaseerd op langste voorvoegsel Match (LPM) tussen de UDRs, systeem en BGP routes. Als er meer dan één route met de overeenkomst in dezelfde LPM, is vervolgens een route geselecteerd op basis van hun oorsprong vinden in de volgende volgorde:
    - Door de gebruiker gedefinieerde route
    - BGP route
    - Route systeem (standaard)

    Met de effectieve routes ziet u alleen effectieve routes LPM vergelijken op basis van alle routes in de hoeveelheid. Door aan te geven hoe de routes daadwerkelijk worden geëvalueerd voor een bepaalde NIC, hierdoor veel eenvoudiger oplossen met specifieke routes die kunnen worden invloed op verbindingen tussen uw VM.

- Als u verkeer naar een netwerk virtueel toestel (NVA), met *VirtualAppliance* als **nextHopType verzendt**en UDRs hebt, moet u controleren of IP-doorsturen is ingeschakeld op de NVA ontvangen van het verkeer of pakketten verloren gaan. 
- Als geforceerde tunneling is ingeschakeld, worden alle uitgaande Internet-verkeer naar op locatie worden gerouteerd. RDP/SSH van Internet voor uw VM werkt niet met deze instelling is afhankelijk van hoe de gebouwen op dit verkeer verwerkt. 
  Gedwongen tunneling kan worden ingeschakeld:
    - Als u met behulp van site naar site VPN, door een gebruiker gedefinieerde route (UDR) met nextHopType als VPN-Gateway
    - Als een standaardroute wordt geadverteerd via BGP
- Voor peering verkeer VNet correct te laten werken, moet de route van een systeem met **nextHopType** *VNetPeering* voor de peered VNet voorvoegsel bereik bestaan. Als een dergelijke route bestaat niet en de VNet peering koppeling ziet er OK:
    - Wacht een paar seconden en probeer het opnieuw als het een nieuw opgezette peering koppeling. Soms duurt het langer routes naar alle netwerkinterfaces in een subnet doorgeven.
    - Network Security Group (NSG) regels kunnen worden de verkeersstromen beïnvloeden. Zie het artikel [Problemen met netwerk-beveiligingsgroepen](virtual-network-nsg-troubleshoot-powershell.md) voor meer informatie.
