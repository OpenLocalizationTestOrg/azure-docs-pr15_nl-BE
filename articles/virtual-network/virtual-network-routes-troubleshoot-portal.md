<properties 
   pageTitle="Problemen oplossen met routes - Portal | Microsoft Azure"
   description="Informatie over het oplossen van problemen met routes in het implementatiemodel voor Azure Resource Manager met behulp van de Portal Azure."
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

# <a name="troubleshoot-routes-using-the-azure-portal"></a>Problemen oplossen met behulp van de Portal Azure routes

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

1. Aanmelden bij de Azure portal op https://portal.azure.com.
2. Klik op **meer services**en klik vervolgens op **virtuele machines** in de lijst die wordt weergegeven.
3. Selecteer een VM oplossen in de lijst die wordt weergegeven en een VM-blade met opties wordt weergegeven.
4. Klik op **Diagnose & oplossen van problemen** en selecteer vervolgens een veelvoorkomend probleem. Bijvoorbeeld: **ik kan geen verbinding maken met mijn Windows VM** is geselecteerd. 

    ![](./media/virtual-network-routes-troubleshoot-portal/image1.png)

5. Stappen worden weergegeven onder het probleem, zoals in de volgende afbeelding: 

    ![](./media/virtual-network-routes-troubleshoot-portal/image2.png)

    Klik op de *effectieve routes* in de lijst met aanbevolen stappen.

6. De **effectieve routes** blade wordt weergegeven, zoals in de volgende afbeelding:

    ![](./media/virtual-network-routes-troubleshoot-portal/image3.png)

    Als uw VM slechts één NIC heeft, is standaard ingeschakeld. Als u meer dan één Netwerkkaart hebt, selecteert u de Netwerkkaart die u wilt weergeven van de effectieve routes.

    >[AZURE.NOTE] Als de VM die is gekoppeld aan de Netwerkkaart niet actief is, effectieve routes niet weergegeven. Alleen de eerste 200 effectieve routes worden weergegeven in de portal. Voor de volledige lijst, klik op **downloaden**. Verder kunt u filteren op de resultaten van het gedownloade CSV-bestand.

    U ziet in de uitvoer als volgt:
    - **Bron**: geeft het type van de route. Systeem-routes worden weergegeven als *standaard*, UDRs worden weergegeven als de *gebruiker* en de gateway (statische of BGP) worden weergegeven als *VPNGateway*.
    - **Status**: geeft de status van de effectieve route. Mogelijke waarden zijn *actief* of is *ongeldig*.
    - **AddressPrefixes**: Hiermee geeft u het adresvoorvoegsel van de effectieve route in de CIDR-notatie. 
    - **nextHopType**: geeft aan dat de volgende hop voor de opgegeven route. Mogelijke waarden zijn *VirtualAppliance*, *Internet*, *VNetLocal*, *VNetPeering*of *Null*. De waarde *Null* voor **nextHopType** in een UDR mogelijk ongeldig. Bijvoorbeeld als **nextHopType** *VirtualAppliance* en het virtuele netwerk is toestel VM niet in staat ingericht/uitgevoerd. Als er geen gateway ingericht/uitgevoerd in de opgegeven VNet is **nextHopType** *VPNGateway* is kan de route ongeldig worden.
    
7. Er is geen route naar de *WestUS VNET3* VNet (voorvoegsel 10.10.0.0/16) weergegeven van de *WestUS VNet1* (voorvoegsel 10.9.0.0/16) in de afbeelding in de vorige stap. In de volgende afbeelding is de koppeling peering in de status *verbroken* :
    
    ![](./media/virtual-network-routes-troubleshoot-portal/image4.png)

    De bi-directionele koppeling voor de peering verbroken is, waarin wordt uitgelegd waarom VM1 kan geen verbinding maken met VM3 in de VNet *WestUS VNet3* .

8. De volgende afbeelding ziet de routes na de bi-directionele peering koppeling tot stand te brengen:

    ![](./media/virtual-network-routes-troubleshoot-portal/image5.png)

Lees de sectie [overwegingen](virtual-network-routes-troubleshoot-portal.md#Considerations) van dit artikel voor meer probleemoplossing scenario's voor gedwongen tunneling en de evaluatie van de route.

### <a name="view-effective-routes-for-a-network-interface"></a>De effectieve routes voor een netwerkinterface weergeven

Als het netwerkverkeer wordt gebracht voor een bepaalde netwerkinterface (NIC), kunt u een volledige lijst van de effectieve routes rechtstreeks op een NIC weergeven. Overzicht van de statistische routes die worden toegepast op een Netwerkkaart, moet u de volgende stappen uitvoeren:

1. Aanmelden bij de Azure portal op https://portal.azure.com.
2. Klik op **meer services**en klik op **Netwerkinterfaces**
3. Zoeken in de lijst voor de naam van een Netwerkkaart of Selecteer deze in de lijst die verschijnt. In dit voorbeeld wordt de **VM1-NIC1** is geselecteerd.
4. Selecteer in de blade **netwerkinterface** **effectieve routes** zoals in de volgende afbeelding:
   
    ![](./media/virtual-network-routes-troubleshoot-portal/image6.png)

    Het **bereik** standaard ingesteld op de netwerkinterface die is geselecteerd.

    ![](./media/virtual-network-routes-troubleshoot-portal/image7.png)


### <a name="view-effective-routes-for-a-route-table"></a>Effectieve routes voor een routetabel weergeven

Wanneer door de gebruiker gedefinieerde routes (UDRs) in een tabel te wijzigen, kunt u bekijken de impact van de routes die op een bepaalde VM wordt toegevoegd. Een tabel kan worden gekoppeld aan een willekeurig aantal subnetten. U kunt nu de effectieve routes weergeven voor alle NIC's die een bepaalde routetabel is toegepast, zonder over te schakelen van de context van het blad bepaalde route tabel.

In dit voorbeeld wordt een UDR (*UDRoute*) worden opgegeven in een routetabel (*UDRouteTable*). Deze route verzendt alle Internet-verkeer van *Subnet1* in de VNet *WestUS-VNet1* via een netwerk virtueel toestel (NVA), in *Subnet2* van de dezelfde VNet. De route wordt weergegeven in de volgende afbeelding:

![](./media/virtual-network-routes-troubleshoot-portal/image8.png)

Overzicht van de statistische routes voor een tabel met de volgende stappen:

1. Aanmelden bij de Azure portal op https://portal.azure.com.
2. **Meer services**, klik op **routetabellen**
3. Zoek in de lijst voor de statistische routes voor zien en selecteert u het gewenste routetabel. In dit voorbeeld is **UDRouteTable** geselecteerd. Een blade voor de geselecteerde tabel wordt weergegeven, zoals in de volgende afbeelding:

    ![](./media/virtual-network-routes-troubleshoot-portal/image9.png)

4. Selecteer de **Effectieve Routes** in de **routetabel** blade. Het **bereik** is ingesteld op de geselecteerde tabel.
5. Een tabel kan worden toegepast op meerdere subnetten. Selecteer het **Subnet** dat u wilt bekijken in de lijst. In dit voorbeeld is **Subnet1** geselecteerd.
6. Selecteer een **netwerk-Interface**. Alle NIC's die zijn verbonden met het geselecteerde subnet weergegeven. In dit voorbeeld wordt de **VM1-NIC1** is geselecteerd.

    ![](./media/virtual-network-routes-troubleshoot-portal/image10.png)

    >[AZURE.NOTE] Als de Netwerkkaart niet gekoppeld aan een actieve VM is, worden geen effectieve routes weergegeven.

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
    - Network Security Group (NSG) regels kunnen worden de verkeersstromen beïnvloeden. Zie het artikel [Problemen met netwerk-beveiligingsgroepen](virtual-network-nsg-troubleshoot-portal.md) voor meer informatie.
