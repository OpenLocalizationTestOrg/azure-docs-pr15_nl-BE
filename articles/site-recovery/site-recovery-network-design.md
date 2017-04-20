<properties
    pageTitle="Ontwerpen van uw netwerkinfrastructuur voor noodherstel | Microsoft Azure"
    description="In dit artikel wordt beschreven netwerk ontwerpoverwegingen voor Azure Site herstellen"
    services="site-recovery"
    documentationCenter=""
    authors="prateek9us"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="09/19/2016"
    ms.author="pratshar"/>

#  <a name="designing-your-network-infrastructure-for-disaster-recovery"></a>Ontwerpen van uw netwerkinfrastructuur voor noodherstel

Dit artikel is gericht op IT-professionals die verantwoordelijk zijn voor het goede toepassingsarchitectuur te ontwerpen, implementeren, en ondersteunende bedrijfscontinuïteit en disaster recovery (BCDR)-infrastructuur en die willen profiteren van de Microsoft Azure Site voor automatisch systeemherstel (ASR) te ondersteunen en hun BCDR diensten te verbeteren. Dit witboek bespreekt praktische overwegingen voor implementatie van System Center Virtual Machine Manager-server, de voor- en nadelen van uitgerekte subnetten versus subnet failover en noodherstel voor virtuele sites in Microsoft Azure structuur.

## <a name="overview"></a>Overzicht

[Azure Site voor automatisch systeemherstel (ASR)](https://azure.microsoft.com/services/site-recovery/) is een service Microsoft Azure orchestrates de bescherming en het herstel van uw gevirtualiseerde toepassingen voor business continuity (BCDR) noodherstel. Dit document is bedoeld om u te helpen de lezer door het proces van het ontwerpen van de netwerken, dat zich richt op IP-bereiken en subnetten op de disaster recovery-site aanpassen bij het repliceren van virtuele machines (VMs) herstel van de Site met.

In dit artikel ziet u bovendien hoe Site-Recovery kunt aanpassen en implementeren van een multisite virtual datacenter ter ondersteuning van BCDR tijdens test of ramp.

In een wereld waar iedereen connectiviteit voor 24/7 verwacht, is het belangrijker dan ooit om uw infrastructuur en toepassingen van en actief. Het doel van Business Continuity en Disaster Recovery (BCDR) is defecte onderdelen te herstellen zodat de organisatie snel opnieuw kan voor normale bewerkingen. Ontwikkeling van strategieën voor noodherstel omgaan met gebeurtenissen onwaarschijnlijk, vernietigende is zeer moeilijk. Dit komt door de inherente moeilijkheid van het voorspellen van de toekomst, met name met betrekking tot het onwaarschijnlijke gebeurtenissen en de hoge kosten om adequate maatregelen ter bescherming tegen een groot catastrophes. 

Cruciaal voor het plannen van BCDR, herstel tijd doelstelling (RTO) en herstel punt doelstelling (vrijgegeven Productieorder) moeten worden gedefinieerd als onderdeel van een calamiteitenplan. Wanneer een ramp van de klant datacenter met Azure Site herstel, klanten kan snel (lage RTO) biedt on line brengen hun gerepliceerde virtuele machines die zich in de secundaire Datacenter of het Microsoft Azure met minimale gegevens verloren gaan (lage vrijgegeven Productieorder). 

Failover wordt mogelijk gemaakt door ASR die in eerste instantie aangewezen virtuele machines vanuit het midden van de primaire gegevens naar het secundaire gegevenscentrum of gekopieerd Azure (afhankelijk van het scenario) en de replica's vervolgens periodiek vernieuwt. Tijdens de planning van infrastructuur netwerkontwerp beschouwd als potentieel knelpunt kan worden voorkomen dat u van vergadering bedrijf RTO en doelstellingen van de vrijgegeven Productieorder.  

Wanneer beheerders van plan bent een disaster recovery-oplossing te implementeren, is een van de belangrijkste vragen in hun geesten hoe de virtuele machine is bereikbaar nadat de failover is uitgevoerd. Automatisch Systeemherstel kan de beheerder het netwerk waaraan een virtuele machine zou worden verbonden na een failover kiezen. Als de primaire site wordt beheerd door een server VMM wordt vervolgens bereikt via het netwerk toewijzen. Zie [voorbereiden voor het netwerk toewijzen](site-recovery-network-mapping.md) voor meer informatie.

Tijdens het ontwerpen van het netwerk voor de site van het herstel, heeft de beheerder twee mogelijkheden:

- Gebruik een ander IP-adresbereik voor het netwerk op de site van herstel. In dit scenario krijgt de virtuele machine na een failover een nieuw IP-adres en de beheerder moet een DNS-update doen. Lees meer over hoe u de DNS-server doet [hier](site-recovery-vmm-to-vmm.md#test-your-deployment) 
- Gebruik dezelfde IP-adresbereik voor het netwerk op de site van herstel. In bepaalde scenario's liever beheerders behouden de IP-adressen die zelfs na de overname van de primaire site nodig hebben. Beheerder moet in een normale scenario voor het bijwerken van de routes om aan te geven van de nieuwe locatie van de IP-adressen. Maar in het geval waarin een uitgerekte VLAN tussen de primaire en de sites herstel is ingezet, behoud van de IP-adressen voor de virtuele machines wordt een aantrekkelijke optie. Houden de dezelfde IP adressen vereenvoudigt het herstel door een netwerk weg te nemen gerelateerde post failover stappen.


Wanneer beheerders van plan bent een disaster recovery-oplossing te implementeren, is een van de belangrijkste vragen in hun gedachten hoe de toepassingen zijn bereikbaar nadat de failover is uitgevoerd. Moderne toepassingen zijn bijna altijd afhankelijk van de toegang tot op zekere hoogte, dus fysiek verplaatsen dat van de ene site naar de andere service vertegenwoordigt een uitdaging voor netwerken. Er zijn twee manieren waarmee dit probleem wordt behandeld in disaster recovery oplossingen. De eerste benadering is het vaste IP-adressen te onderhouden. Ondanks de services verplaatsen en de hosting-servers in verschillende fysieke locaties, nemen toepassingen de IP-adresconfiguratie met hen naar de nieuwe locatie. De tweede aanpak omvat het compleet wijzigen van het IP-adres tijdens de overgang in de herstelde site. Elke benadering heeft verschillende varianten van uitvoering die hieronder worden samengevat.

Tijdens het ontwerpen van het netwerk voor de site van het herstel, heeft de beheerder twee mogelijkheden:

## <a name="option-1-retain-ip-addresses"></a>Optie 1: IP-adressen behouden 

Vanuit een perspectief van disaster recovery proces, met vaste IP-adressen is waarschijnlijk de eenvoudigste methode voor het implementeren, maar heeft een aantal potentiële uitdagingen die in de praktijk de minst populaire aanpak maken. Azure-Site-Recovery biedt de mogelijkheid te behouden van de IP-adressen in alle scenario's. Voordat een besluit te behouden, IP, moet passende aandacht worden besteed aan de beperkingen opgelegd, de failover-mogelijkheden. Laat ons de factoren waarmee u een beslissing te behouden van IP-adressen, of niet kunnen bekijken. Dit kan op twee manieren bereikt met behulp van een subnet uitgerekt of als u een volledige subnet failover uitvoert.

### <a name="stretched-subnet"></a>Uitgerekte subnet

Hier wordt het subnet tegelijkertijd met zowel primaire DR locaties gemaakt. Dit betekent in eenvoudige termen kunt u een server en de configuratie van IP-(laag 3) verplaatsen naar de tweede site en het netwerk wordt het verkeer doorstuurt naar de nieuwe locatie automatisch. Dit vrij eenvoudig te behandelen vanuit het perspectief van een server is, maar heeft een aantal uitdagingen:

- Vanuit een perspectief laag 2 (data link-laag), moet de netwerkapparatuur die een uitgerekte VLAN kunt beheren, maar dit is minder een probleem omdat het nu algemeen beschikbaar is geworden. Tweede en moeilijker is het probleem dat door de onderdelen van het VLAN het mogelijke probleem domein wordt uitgebreid tot beide sites, in wezen steeds een potentieel risico. Dit is waarschijnlijk niet voorkomt, is er een broadcast-storm gestart, maar kan niet worden geïsoleerd gebeurd. Gemengde meningen over dit laatste probleem hebt gevonden en we zien veel geslaagde implementaties ook 'we zullen nooit hier deze technologie implementeren'.
- Uitgerekte subnet is niet mogelijk als u met Microsoft Azure als de site DR.


### <a name="subnet-failover"></a>Subnet failover

Het is mogelijk voor het subnet failover voor de voordelen van de hierboven beschreven zonder het subnet rekken op meerdere sites uitgerekte subnet-oplossing implementeren. Hier zou een bepaald subnet aanwezig zijn op locatie 1 of 2, maar niet op beide sites tegelijk. Om de IP-adresruimte in het geval van een failover, is het mogelijk via programmering rangschikken voor de infrastructuur van de router naar de subnetten van de ene site naar de andere verplaatsen. Beveiligd in een failover-scenario die de subnetten zou worden verplaatst met de bijbehorende VMs. Het belangrijkste nadeel van deze aanpak is bij een storing die u hebt de hele subnet, kan worden verplaatst, maar kan invloed hebben op de granulatie failover-overwegingen. 

We bekijken hoe een fictieve onderneming met de naam Contoso kan de VMs repliceren naar een locatie voor herstel bij een storing over het gehele subnet. Laten we eerst zien hoe Contoso de subnetten beheren is terwijl VMs repliceren tussen twee-lokale vestigingen, en vervolgens bespreken we hoe failover subnet werkt wanneer [dat Azure wordt gebruikt als de disaster recovery-site](#failover-to-azure).

#### <a name="failover-to-a-secondary-on-premises-site"></a>Failover naar een secundaire op locatie site

Laat ons kijken naar een scenario waar we willen behouden het OT van elk van de VMs en failover-het volledige subnet samen. De primaire site heeft toepassingen in een subnet 192.168.1.0/24. Als failover gebeurt, worden alle virtuele machines die deel van dit subnet uitmaken wordt failover uitgevoerd naar de site van herstel en behouden hun IP-adressen. Routes moeten op de juiste wijze aangepast aan het feit dat alle virtuele machines die deel uitmaken van het subnet 192.168.1.0/24 nu naar de recovery site hebt verplaatst. 

In de volgende afbeelding moet de routes tussen primaire en recovery site, derde site en primaire site en derde site en recovery site op de juiste wijze worden gewijzigd. 

De volgende afbeeldingen ziet u de subnetten die vóór de failover-functie. Subnet 192.168.0.1/24 op de primaire Site voordat u de failover-functie actief is en actief van de Site voor herstel na een failover 

![Voordat failover wordt uitgevoerd](./media/site-recovery-network-design/network-design2.png)

Voordat failover wordt uitgevoerd


De volgende afbeelding ziet netwerken en subnetten na een failover.
    
![Na een Failover](./media/site-recovery-network-design/network-design3.png)

Na een failover

In de secundaire site is in de lokalen en u een VMM-server te beheren is om de beveiliging voor een specifieke virtuele machine, ASR netwerken de volgende workflow de resources worden toegewezen:

- ASR wijst een IP-adres voor elke netwerkinterface op de virtuele machine van de statische IP-adresgroep die is gedefinieerd op het desbetreffende netwerk voor elk exemplaar van System Center VMM.
- Als de beheerder de dezelfde IP-adresgroep voor het netwerk op de site herstellen als die van de IP-adresgroep van het netwerk op de primaire site definieert tijdens het toewijzen van het IP-adres op de virtuele machine van replica ASR hetzelfde IP-adres als die van de primaire virtuele machine wilt toewijzen.  Het onderzoektijdvak in VMM is gereserveerd maar niet ingesteld als failover-IP. Failover-IP is ingesteld voordat de failover-functie.

![Bewaren IP-adres](./media/site-recovery-network-design/network-design4.png)
    
Figuur 5

In figuur 5 ziet de failover-TCP/IP-instellingen voor de virtuele machine van replica (op de console van Hyper-V). Deze instellingen zou worden gevuld voordat de virtuele machine wordt gestart na een failover

Als de dezelfde IP niet beschikbaar is, zou ASR sommige andere beschikbare IP-adres toewijzen uit de gedefinieerde IP-adresgroep. 

Nadat de VM voor beveiliging die kunt u volgende voorbeeldscript controleren of de IP dat is toegewezen aan de virtuele machine is ingeschakeld. Het hetzelfde IP zou instellen als Failover IP en toegewezen aan de VM op het moment van failover:

        $vm = Get-SCVirtualMachine -Name <VM_NAME>
        $na = $vm[0].VirtualNetworkAdapters>
        $ip = Get-SCIPAddress -GrantToObjectID $na[0].id
        $ip.address  

>[AZURE.NOTE] In het scenario waarbij virtuele machines gebruik van DHCP, is het beheer van IP-adressen volledig buiten de controle van automatisch Systeemherstel. Een beheerder heeft om ervoor te zorgen dat de DHCP-server die de IP-adressen op de site van het herstel van hetzelfde bereik als die van de primaire site dienen kan.

#### <a name="failover-to-azure"></a>Failover naar Azure

Azure Site voor automatisch systeemherstel (ASR) kunt Microsoft Azure worden gebruikt als een disaster recovery-site voor uw virtuele machines. In dit geval moet u omgaan met een beperking meer. 

We bekijken een scenario waarbij een fictief bedrijf met de naam Woodgrove Bank hun aantal zakelijke toepassingen die als host fungeert voor gebouwen-infrastructuur heeft en zij hun mobiele toepassingen op Azure hosten. Verbinding tussen Woodgrove Bank VMs in Azure en op gebouwen-servers wordt geleverd door een site naar site (S2S) virtueel particulier netwerk (VPN). Virtueel netwerk van Woodgrove Bank kunt S2S VPN in Azure worden gezien als een uitbreiding van de Woodgrove Bank op ruimten netwerk. Deze mededeling wordt ingeschakeld door de S2S VPN tussen Woodgrove Bank en Azure virtueel netwerk. Woodgrove wil nu automatisch Systeemherstel gebruiken voor het repliceren van de werkbelasting in het datacenter uitvoeren naar Azure. Deze optie komt tegemoet aan Woodgrove die wil een goedkope optie voor DR en kunnen worden gegevens opgeslagen in openbare cloud-omgevingen. Woodgrove omgaan met toepassingen en configuraties die afhankelijk van vaste IP-adressen zijn zijn, dus ze hebben een vereiste IP-adressen voor hun toepassingen behouden na storing worden overgenomen op Azure.

Woodgrove heeft besloten om de IP-adressen toewijzen van IP-adresbereik (172.16.1.0/24, 172.16.2.0/24) tot de bronnen in Azure uitgevoerd.

Voor Woodgrove kunnen worden gerepliceerd van de virtuele machines naar Azure behoud de IP-adressen, moet een virtueel netwerk Azure worden gemaakt. Een uitbreiding van het netwerk op ruimten moet zijn dat toepassingen failover van de site op ruimten naar Azure naadloos kunnen. Azure kunt u VPN-verbindingen van site naar site zoals punt-naar-site toevoegen aan de virtuele netwerken in Azure gemaakt. Bij het instellen van de verbinding van site naar site, heeft Azure netwerk kunt u verkeer doorstuurt naar de locatie van de op de gebouwen (Azure roept het LAN-netwerk) alleen als het IP-adresbereik anders dan in lokalen IP-adresbereik is omdat Azure rekken subnetten wordt niet ondersteund.  Dit betekent dat als u een subnet 192.168.1.0/24 op lokalen, u kunt niet een LAN-netwerk 192.168.1.0/24 toevoegen in het netwerk Azure. Dit is normaal omdat Azure niet weten dat er geen actieve VMs in het subnet en het subnet wordt gemaakt alleen voor doeleinden van DR. Om het netwerkverkeer niet op een netwerk Azure correct routeren de subnetten in het netwerk en het LAN-netwerk te kunnen niet in strijd zijn. 

![Voordat de Subnet-Failover](./media/site-recovery-network-design/network-design7.png)

Voordat failover wordt uitgevoerd

Om te helpen te voldoen aan de behoeften van hun zakelijke Woodgrove, moeten we de volgende workflows implementeren:

- Een extra netwerk maken, bellen we Recovery-netwerk, waar het kan niet via virtuele machines zouden worden gemaakt.
- De dezelfde IP dat de VM op gebouwen heeft om ervoor te zorgen dat de IP voor een VM nadat failover is uitgevoerd behouden blijft, gaat u naar het tabblad configureren onder VM-eigenschappen opgeven en klik vervolgens op opslaan. Wanneer de VM storing is opgetreden, wordt Azure Site herstel het opgegeven IP toewijzen aan de virtuele machine. 

![Eigenschappen van netwerk](./media/site-recovery-network-design/network-design8.png)

Nadat de failover-functie wordt geactiveerd en de virtuele machines zijn gemaakt in het netwerk herstellen met het gewenste IP-kan verbinding met dit netwerk worden gemaakt met behulp van een [Vnet met Vnet-verbinding](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md). Als deze actie kan worden vastgelegd in een script nodig.  Zoals we in de vorige sectie over failover subnet, zelfs wanneer een failover naar Azure besproken, zou routes moeten op passende wijze worden aangepast aan dat die 192.168.1.0/24 is nu verplaatst naar Azure. 

![Na een Failover Subnet](./media/site-recovery-network-design/network-design9.png)

Na een failover

Als u niet een Azure netwerk zoals in de bovenstaande afbeelding. Nadat de failover-functie kunt u een site naar site VPN-verbinding tussen uw 'Primary Site' en 'Netwerk herstellen'.  


## <a name="option-2-changing-ip-addresses"></a>Optie 2: IP-adressen wijzigen

Deze aanpak lijkt te zijn het meest gangbare op basis van wat we hebben gezien. Het heeft de vorm van het IP-adres van elke VM die bij de overname betrokken is. Een nadeel van deze aanpak is het binnenkomende netwerk 'weten' dat de toepassing die op IPx bevindt zich nu op IPy vereist. Zelfs als IPx en IPy logische namen, hebben meestal DNS-vermeldingen worden gewijzigd of verwijderd in het gehele netwerk, en de vermeldingen in cache in netwerk-tabellen worden bijgewerkt of verwijderd, daarom een uitvaltijd kan worden gezien, afhankelijk van hoe de DNS-infrastructuur ingesteld is. Deze problemen kunnen worden beperkt door lage TTL-waarden voor intranettoepassingen en [Azure verkeer Manager met automatisch Systeemherstel](http://azure.microsoft.com/blog/2015/03/03/reduce-rto-by-using-azure-traffic-manager-with-azure-site-recovery/) gebruiken voor op internet gebaseerde toepassingen

### <a name="changing-the-ip-addresses---illustration"></a>De IP-adressen - wijzigen afbeelding

Laat ons kijken naar het scenario waar u van plan bent naar verschillende IP-adressen gebruiken in de primaire en de herstel-sites. In het volgende voorbeeld dat we ook hebben een andere site uit waar de toepassingen die worden gehost op de primaire of herstel site toegankelijk is.

![Verschillende IP - voordat failover wordt uitgevoerd](./media/site-recovery-network-design/network-design10.png)

Figuur 11

In figuur 11 zijn sommige toepassingen in het subnet van subnet 192.168.1.0/24 op de primaire site gehost en zijn geconfigureerd om te komen op de site herstellen in het subnet 172.16.1.0/24 nadat failover is uitgevoerd. VPN-verbindingen/netwerk routes zijn op de juiste wijze geconfigureerd zodat alle drie sites toegang elkaar tot.
 
Als de afbeelding 12 laat zien, na een storing worden overgenomen van een of meer toepassingen, worden ze hersteld in het subnetwerk voor herstel. In dit geval zijn we niet beperkt tot het gehele subnet failover op hetzelfde moment. Geen wijzigingen zijn vereist voor VPN- of netwerk-routes te configureren. Een failover en bepaalde DNS-updates zorgt ervoor dat toepassingen niet toegankelijk zijn. Als de DNS-server is geconfigureerd voor het toestaan van dynamische updates zou vervolgens de virtuele machines registreren zichzelf met behulp van het nieuwe onderzoektijdvak zodra ze nadat failover is uitgevoerd beginnen. 

![Verschillende IP - na een Failover](./media/site-recovery-network-design/network-design11.png)

Figuur 12

De replica virtuele machine mogelijk na mislukte via een IP-adres dat is niet hetzelfde als het IP-adres van de primaire virtuele machine. Virtuele machines werkt u de DNS-server die ze gebruiken wanneer ze worden gestart. DNS-vermeldingen hebben meestal worden gewijzigd of verwijderd in het gehele netwerk en de vermeldingen in cache in netwerk tabellen moeten worden bijgewerkt of verwijderd, dus het is niet ongebruikelijk om te worden geconfronteerd met uitvaltijd, terwijl deze staat wijzigingen plaatsvinden. Dit probleem kan worden omzeild door:

- Met behulp van lage TTL-waarden voor intranettoepassingen.
- Met behulp van Azure verkeer Manager met automatisch Systeemherstel voor internet gebaseerde toepassingen.
- Met behulp van het volgende script in een herstelplan voor het bijwerken van de DNS-Server om ervoor te zorgen een tijdige update (het script is niet vereist als u de dynamische DNS-registratie is geconfigureerd)

        string]$Zone,
        [string]$name,
        [string]$IP
        )
        $Record = Get-DnsServerResourceRecord -ZoneName $zone -Name $name
        $newrecord = $record.clone()
        $newrecord.RecordData[0].IPv4Address  =  $IP
        Set-DnsServerResourceRecord -zonename $zone -OldInputObject $record -NewInputObject $Newrecord


### <a name="changing-the-ip-addresses--dr-to-azure"></a>De IP-adressen: DR naar Azure wijzigen

Het weblogbericht [Networking Infrastructure Setup voor Microsoft Azure een Disaster Recovery site](http://azure.microsoft.com/blog/2014/09/04/networking-infrastructure-setup-for-microsoft-azure-as-a-disaster-recovery-site/) wordt uitgelegd hoe u setup de vereiste Azure netwerkinfrastructuur bij behoud van IP-adressen is niet een vereiste. Het begint met een beschrijving van de toepassing en vervolgens kijken bij het instellen van netwerken op gebouwen en Azure en vervolgens concluderen hoe een failover test en een geplande failover.



## <a name="next-steps"></a>Volgende stappen

[Leren](site-recovery-network-mapping.md) hoe herstel Site kaarten bron en doel netwerken wanneer een VMM-server wordt gebruikt voor het beheren van de primaire site. 
