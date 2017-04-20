<properties 
   pageTitle="DNS-opties voor naamomzetting voor Linux VMs in Azure"
   description="Naam scenario's voor Linux VMs in Azure IaaS, met inbegrip van de opgegeven DNS-naamomzettingsservices, hybride externe DNS en breng uw eigen DNS-server."
   services="virtual-machines"
   documentationCenter="na"
   authors="RicksterCDN"
   manager="timlt"
   editor="tysonn" />
<tags 
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/19/2016"
   ms.author="rclaus" />

# <a name="dns-name-resolution-options-for-linux-vms-in-azure"></a>DNS-opties voor naamomzetting voor Linux VMs in Azure

Azure biedt DNS-naamomzetting standaard voor alle VMs zich in één virtueel netwerk. U zijn kunt uw eigen DNS-naam resolutie oplossing implementeren door het configureren van uw eigen DNS-services op uw Azure gehoste VMs. De volgende scenario's kunt u zelf kiezen welke beter voor uw specifieke situatie.

- [Resolutie van de naam van Azure](#azure-provided-name-resolution)

- [Naamomzetting met DNS-server](#name-resolution-using-your-own-dns-server) 

Het type naamomzetting dat u gebruikt is afhankelijk van hoe uw VMs en rol exemplaren moeten met elkaar communiceren.

**De volgende tabel ziet u scenario's en de bijbehorende naam resolutie oplossingen:**

| **Scenario** | **Oplossing** | **Achtervoegsel** |
|--------------|--------------|----------|
| Naamomzetting tussen exemplaren van de rol of VMs zich in hetzelfde virtuele netwerk | [Resolutie van de naam van Azure](#azure-provided-name-resolution)| hostnaam of FQDN-naam |
| Naamomzetting tussen exemplaren van de rol of VMs zich in verschillende virtuele netwerken | Klant beheerde DNS-servers doorsturen van query's tussen de vnets voor de omzetting door Azure (DNS-proxy).  Zie [naamomzetting met DNS-server](#name-resolution-using-your-own-dns-server)| FQDN-naam alleen |
| Omzetting van de computer en service namen voor gebouwen van exemplaren van de rol of VMs in Azure | Klant beheerde DNS-servers (bijvoorbeeld op de lokale domeincontroller, lokale alleen-lezen domeincontroller of een secundaire DNS gesynchroniseerd met behulp van zoneoverdrachten).  Zie [naamomzetting met DNS-server](#name-resolution-using-your-own-dns-server)|FQDN-naam alleen |
| Resolutie van Azure hostnamen die gebruikmaken van lokale computers | Doorsturen van aanvragen naar een klant beheerde DNS proxy-server in het bijbehorende vnet, de proxy-server stuurt query's naar Azure voor resolutie. Zie [naamomzetting met DNS-server](#name-resolution-using-your-own-dns-server)| FQDN-naam alleen |
| Reverse DNS voor interne IP-adressen | [Naamomzetting met DNS-server](#name-resolution-using-your-own-dns-server) | n.v.t. |

## <a name="azure-provided-name-resolution"></a>Resolutie van de naam van Azure

Samen met de resolutie van de openbare DNS-namen voorziet Azure interne naamomzetting VMs en rol van instanties die zich in hetzelfde virtuele netwerk bevinden.  Het DNS-achtervoegsel is consistent in het virtuele netwerk (dus de FQDN-naam is niet nodig) en DNS-namen kunnen worden toegewezen aan zowel NIC's en VMs in ARM gebaseerde virtuele netwerken. Hoewel geleverde Azure naamomzetting niet nodig elke configuratie, is het niet de juiste keuze voor alle implementatiescenario's, zoals weergegeven in de voorgaande tabel.

### <a name="features-and-considerations"></a>Functies en overwegingen

**Functies:**

- Eenvoudig in gebruik: Er is geen configuratie vereist is voor naam van Azure resolutie.

- Azure geleverde service voor naamomzetting is uiterst beschikbaar, zodat u de noodzaak om te maken en beheren van clusters van uw eigen DNS-servers.

- Kan met uw eigen DNS-servers worden gebruikt zowel lokale als Azure hostnamen omzetten.

- Naamomzetting wordt tussen VMs in virtuele netwerken zonder de noodzaak voor de FQDN-naam geleverd. 

- Hostnamen die het beste uw implementaties beschrijven, kunt u in plaats van het werken met de namen automatisch gegenereerd.

**Overwegingen met betrekking tot:**

- Het Azure gemaakt van DNS-achtervoegsel kan niet worden gewijzigd.

- U kunt uw eigen administratie niet handmatig registreren.

- WINS- en NetBIOS-worden niet ondersteund.

- Hostnamen moet DNS-compatibele (alleen 0-9, a-z moet worden gebruikt en '-', en kan niet beginnen of eindigen met een '-'. Zie RFC 3696 sectie 2.)

- DNS-queryverkeer verminderd voor elke VM. Dit moet niet van invloed op de meeste toepassingen.  Als aanvraag bandbreedtebeperking wordt waargenomen, controleert u caching aan clientzijde is ingeschakeld.  Zie [het beste geleverde Azure naamomzetting](#Getting-the-most-from-Azure-provided-name-resolution)voor meer informatie.


### <a name="getting-the-most-from-azure-provided-name-resolution"></a>Ophalen van de meest uit naam van Azure resolutie
**Caching aan clientzijde:**

Niet alle DNS-query wordt verzonden via het netwerk.  Caching aan clientzijde kan verminderen latentie en veerkracht te blips netwerk verbeteren door terugkerende DNS-query's in een lokale cache.  DNS-records bevatten een Time-To-Live (TTL) die het mogelijk de cache maakt voor het opslaan van de record voor zo lang mogelijk zonder invloed op record versheid.  Om deze reden is caching aan clientzijde geschikt voor de meeste situaties.

Sommige Linux-distros bevatten geen cache standaard.  Het is raadzaam dat u toevoegen aan elk Linux VM (na controle dat er een lokale cache nog niet).

Er zijn diverse verschillende DNS-caching pakketten beschikbaar, bijvoorbeeld dnsmasq, dnsmasq op de meest voorkomende distros installeren de volgende stappen uitvoeren:

- **Ubuntu (resolvconf gebruikt)**:
    - Installeer het pakket dnsmasq ("sudo apt-get install dnsmasq").
- **SUSE (netconf gebruikt)**:
    - Installeer het pakket dnsmasq ("sudo zypper installeren dnsmasq") 
    - Schakel de service dnsmasq ("systemctl inschakelen dnsmasq.service") 
    - Start de service dnsmasq ("systemctl start dnsmasq.service") 
    - bewerken "/ etc/sysconfig/netwerk/config" en wijzig NETCONFIG_DNS_FORWARDER = "" naar "dnsmasq"
    - resolv.conf ("netconfig-update') in te stellen van de cache als de lokale DNS-resolver bijwerken
- **OpenLogic (NetworkManager gebruikt)**:
    - Installeer het pakket dnsmasq ("sudo yum install dnsmasq")
    - Schakel de service dnsmasq ("systemctl inschakelen dnsmasq.service")
    - Start de service dnsmasq ("systemctl start dnsmasq.service")
    - "Voeg domain name servers 127.0.0.1;" toevoegen aan "/etc/dhclient-eth0.conf"
    - Start de NetworkService ("service netwerk opnieuw opstarten') als u de cache de lokale DNS-resolver

> [AZURE.NOTE]: Het pakket "dnsmasq" is slechts een van de vele DNS-cache beschikbaar voor Linux.  Controleer voordat u deze gebruikt, geschikt is voor uw specifieke behoeften en geen andere cache is geïnstalleerd.

**Client-side pogingen:**

DNS is voornamelijk een UDP-protocol.  Als het UDP-protocol garandeert aflevering van berichten niet, wordt in het DNS-protocol zelf logica opnieuw verwerkt.  Elke DNS-client (besturingssysteem) kan vertonen verschillende Pogingslogica afhankelijk van de makers van voorkeur:

 - Windows-besturingssystemen opnieuw proberen na een tweede en vervolgens opnieuw na een andere twee, vier en andere vier seconden. 
 - De standaard Linux installatie opnieuw proberen na vijf seconden.  Moet u dit opnieuw vijf keer tussenpozen van één seconde.  

Om de huidige instellingen op een Linux VM, 'kat /etc/resolv.conf' en kijk naar de regel 'opties', bijvoorbeeld:

    options timeout:1 attempts:5

Het bestand resolv.conf automatisch wordt gegenereerd en niet worden bewerkt.  De specifieke stappen voor het toevoegen van de regel '-opties verschillen per distro:

- **Ubuntu** (maakt gebruik van resolvconf):
    - de opties voor regel toe te voegen ' / etc/resolveconf/resolv.conf.d/head' 
    - resolvconf -u uitvoeren om te werken
- **SUSE** (maakt gebruik van netconf):
    - 'timeout:1 pogingen: 5' toevoegen aan de NETCONFIG_DNS_RESOLVER_OPTIONS = "" parameter in '/ etc/sysconfig/netwerk/config' 
    - netconfig-update uitvoeren om te werken
- **OpenLogic** (maakt gebruik van NetworkManager):
    - 'echo "opties timeout:1 pogingen: 5" ' toevoegen aan ' / etc/NetworkManager/dispatcher.d/11-dhclient' 
    - 'service netwerk opnieuw opstarten' uitvoeren om te werken

## <a name="name-resolution-using-your-own-dns-server"></a>Naamomzetting met DNS-server
Er zijn verschillende situaties waar uw naam resolutie behoeften dan de functies reiken kunnen voorwaarde Azure, bijvoorbeeld wanneer u wenst DNS-omzetting tussen virtuele netwerken (vnets dat).  Voor dit scenario kunt Azure u uw eigen DNS-servers gebruiken.  

DNS-servers in een virtueel netwerk kunnen DNS-query's doorsturen naar Azure van recursieve resolvers omzetten van hostnamen in het virtuele netwerk.  Bijvoorbeeld een DNS-server waarop wordt uitgevoerd in Azure reageren op DNS-query's voor de eigen DNS-zonebestanden en andere query's doorsturen naar Azure.  Hierdoor kan de VMs zowel de ingaven in zonebestanden en geleverde Azure hostnamen (via de doorstuurserver).  Toegang tot de Azure recursieve resolvers is beschikbaar via het virtuele IP-168.63.129.16.

DNS-doorsturen inter-vnet DNS-omzetting kan ook en kunt uw lokale machines geleverde Azure hostnamen omzetten.  U kunt oplossen door een VM de hostnaam, de DNS-server VM moet zich bevinden in hetzelfde virtuele netwerk en voor hostname forward query's naar Azure worden geconfigureerd.  Als u het DNS-achtervoegsel is anders in elk vnet, kunt u regels voor voorwaardelijk doorsturen DNS-query's verzenden naar de juiste vnet voor resolutie.  In de volgende afbeelding ziet u twee vnets en een lokaal netwerk doen inter-vnet DNS-omzetting met behulp van deze methode:

![Inter-vnet DNS](./media/virtual-machines-linux-azure-dns/inter-vnet-dns.png)

Wanneer u de naam van Azure resolutie, is de interne DNS-achtervoegsel beschikbaar voor elke VM met behulp van DHCP.  Wanneer u uw eigen naam resolutie oplossing, is dit achtervoegsel verstrekt aan VMs omdat dit problemen met andere DNS-architecturen oplevert.  Verwijzen naar computers met FQDN of het achtervoegsel configureren op uw VMs, kan het achtervoegsel worden bepaald met behulp van PowerShell of de API:

-  Voor Azure Resource Management vnets beheerde, het achtervoegsel beschikbaar via de bron van de [netwerk-interfacekaart is](https://msdn.microsoft.com/library/azure/mt163668.aspx) of u kunt de opdracht uitvoeren `azure network public-ip show <resource group> <pip name>` om de details van uw openbare IP, met inbegrip van de FQDN-naam van de netwerkadapterkaart.    


Als uw niet op query's doorsturen naar Azure behoeften, moet u uw eigen DNS-oplossing te bieden.  De DNS-oplossing moet:

-  Juiste omzetten, bijvoorbeeld via [DDNS](../virtual-network/virtual-networks-name-resolution-ddns.md)bieden.  Opmerking Als de DDNS, moet u de DNS-record opruiming Azure van DHCP-leases zijn erg lang en opruiming uitgeschakeld verwijderen DNS-records voortijdig beëindigd. 
-  Juiste naamomzetting om omzetting van namen voor externe domein bevatten.
-  Toegankelijk (TCP en UDP op poort 53) van de clients die het dient te worden en kunnen toegang krijgen tot het internet.
-  Worden beveiligd tegen toegang vanaf internet te beperken door agents op externe dreigingen.

> [AZURE.NOTE] Wanneer u Azure VMs als DNS-servers, IPv6 moet worden uitgeschakeld voor de beste prestaties en een [Openbare IP instantieniveau](../virtual-network/virtual-networks-instance-level-public-ip.md) moet worden toegewezen aan elke DNS-server VM.  

