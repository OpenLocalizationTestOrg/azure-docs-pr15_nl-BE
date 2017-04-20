<properties 
   pageTitle="Resolutie voor VMs en exemplaren van de rol"
   description="Naam resolutie scenario's voor Azure IaaS, hybride oplossingen, tussen verschillende cloud-services, Active Directory en DNS-server "
   services="virtual-network"
   documentationCenter="na"
   authors="GarethBradshawMSFT"
   manager="carmonm"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/31/2016"
   ms.author="telmos" />

# <a name="name-resolution-for-vms-and-role-instances"></a>Naamomzetting voor VMs en exemplaren van de rol

Afhankelijk van hoe u Azure IaaS, PaaS en hybride oplossingen beschikken, moet u de VMs en rol exemplaren die u maakt om te communiceren met elkaar. Hoewel deze mededeling kan worden uitgevoerd met behulp van IP-adressen, is het veel eenvoudiger te gebruiken die gemakkelijk kunnen worden onthouden en niet wijzigen. 

Exemplaren van de rol en gehost in Azure VMs domeinnamen omzetten in interne IP-adressen moeten, u die een van twee methoden:

- [Resolutie van de naam van Azure](#azure-provided-name-resolution)

- [Naamomzetting met DNS-server](#name-resolution-using-your-own-dns-server) (die kan query's doorgestuurd naar de Azure opgegeven DNS-servers) 

Het type naamomzetting dat u gebruikt is afhankelijk van hoe uw VMs en rol exemplaren moeten met elkaar communiceren.

**De volgende tabel ziet u scenario's en de bijbehorende naam resolutie oplossingen:**

| **Scenario** | **Oplossing** | **Achtervoegsel** |
|--------------|--------------|----------|
| Naamomzetting tussen exemplaren van de rol of VMs zich in dezelfde cloud service- of virtuele netwerk | [Resolutie van de naam van Azure](#azure-provided-name-resolution)| hostnaam of FQDN-naam |
| Naamomzetting tussen exemplaren van de rol of VMs zich in verschillende virtuele netwerken | Klant beheerde DNS-servers doorsturen van query's tussen de vnets voor de omzetting door Azure (DNS-proxy).  Zie [naamomzetting met DNS-server](#name-resolution-using-your-own-dns-server)| FQDN-naam alleen |
| Omzetting van de computer en service namen voor gebouwen van exemplaren van de rol of VMs in Azure | Klant beheerde DNS-servers (bv. lokale domeincontroller, lokale alleen-lezen domeincontroller of een secundaire DNS gesynchroniseerd met behulp van zoneoverdrachten).  Zie [naamomzetting met DNS-server](#name-resolution-using-your-own-dns-server)|FQDN-naam alleen |
| Resolutie van Azure hostnamen die gebruikmaken van lokale computers | Doorsturen van aanvragen naar een klant beheerde DNS proxy-server in het bijbehorende vnet, de proxy-server stuurt query's naar Azure voor resolutie. Zie [naamomzetting met DNS-server](#name-resolution-using-your-own-dns-server)| FQDN-naam alleen |
| Reverse DNS voor interne IP-adressen | [Naamomzetting met DNS-server](#name-resolution-using-your-own-dns-server) | n.v.t. |
| Naamomzetting tussen VMs of rol-exemplaren zich in verschillende cloud-diensten, niet in een virtueel netwerk| Niet van toepassing. Verbindingen tussen VMs en rol exemplaren in verschillende cloud-services buiten een virtueel netwerk niet wordt ondersteund.| n.v.t. |



## <a name="azure-provided-name-resolution"></a>Resolutie van de naam van Azure

Samen met de resolutie van de openbare DNS-namen voorziet Azure interne naamomzetting VMs en rol van instanties die zich in het hetzelfde virtuele netwerk of cloud-service bevinden.  VMs/exemplaren in een cloud-service delen het dezelfde DNS-achtervoegsel (zodat de hostnaam alleen al voldoende is), maar in de klassieke virtuele netwerken verschillende cloud services hebben verschillende DNS-achtervoegsels de FQDN-naam is vereist voor het herleiden van namen tussen verschillende cloud-services.  Het DNS-achtervoegsel is consistent in het virtuele netwerk (dus de FQDN-naam is niet nodig) en DNS-namen kunnen worden toegewezen aan zowel NIC's en VMs in virtuele netwerken in het implementatiemodel Resource Manager. Hoewel geleverde Azure naamomzetting niet nodig elke configuratie, is het niet de juiste keuze voor alle implementatiescenario's, zoals weergegeven in de bovenstaande tabel.

> [AZURE.NOTE] In het geval van web- en werknemer rollen, kunt u het interne IP-adressen van de rol-exemplaren op basis van de rol en het exemplaar nummer Azure Service Management REST API gebruiken openen. Zie [Service Management REST API Reference](https://msdn.microsoft.com/library/azure/ee460799.aspx)voor meer informatie.

### <a name="features-and-considerations"></a>Functies en overwegingen

**Functies:**

- Eenvoudig in gebruik: Er is geen configuratie is vereist voor de geleverde Azure naamomzetting.

- Azure geleverde service voor naamomzetting is uiterst beschikbaar, zodat u de noodzaak om te maken en beheren van clusters van uw eigen DNS-servers.

- Kan worden gebruikt in combinatie met uw eigen DNS-servers op te lossen, zowel lokaal als Azure hostnamen.

- Naamomzetting wordt tussen exemplaren rol/VMs binnen dezelfde cloud service zonder noodzaak van een FQDN-naam geleverd.

- Naamomzetting wordt tussen VMs in virtuele netwerken die gebruikmaken van het implementatiemodel Resource Manager, zonder de noodzaak voor de FQDN-naam geleverd. Virtuele netwerken in het implementatiemodel Klassiek moeten de FQDN-naam bij het omzetten van namen in verschillende cloud-services. 

- Hostnamen die het beste uw implementaties beschrijven, kunt u in plaats van het werken met de namen automatisch gegenereerd.

**Overwegingen met betrekking tot:**

- Het Azure gemaakt van DNS-achtervoegsel kan niet worden gewijzigd.

- U kunt uw eigen administratie niet handmatig registreren.

- WINS- en NetBIOS-worden niet ondersteund. (U kunt uw VMs in Windows Verkenner niet zien.)

- Hostnamen moet DNS-compatibele (alleen 0-9, a-z moet worden gebruikt en '-', en kan niet beginnen of eindigen met een '-'. Zie RFC 3696 sectie 2.)

- DNS-queryverkeer verminderd voor elke VM. Dit moet niet van invloed op de meeste toepassingen.  Als aanvraag bandbreedtebeperking wordt waargenomen, controleert u caching aan clientzijde is ingeschakeld.  Zie [het beste geleverde Azure naamomzetting](#Getting-the-most-from-Azure-provided-name-resolution)voor meer informatie.

- Alleen VMs in de eerste 180 cloud-services zijn geregistreerd voor elk virtueel netwerk in een klassieke implementatiemodel. Dit geldt niet voor virtuele netwerken in implementatiemodellen Resource Manager.


### <a name="getting-the-most-from-azure-provided-name-resolution"></a>Ophalen van de meest uit naam van Azure resolutie
**Caching aan clientzijde:**

Niet alle DNS-query moet via het netwerk worden verzonden.  Caching aan clientzijde kan verminderen latentie en veerkracht te blips netwerk verbeteren door terugkerende DNS-query's in een lokale cache.  DNS-records bevatten een Time-To-Live (TTL) die het mogelijk de cache maakt voor het opslaan van de record voor zo lang mogelijk zonder invloed op record versheid, caching aan clientzijde is geschikt voor de meeste situaties.

De standaard Windows DNS-Client heeft een ingebouwd DNS-cache.  Sommige Linux distros cache standaard niet opnemen, is het raadzaam dat een worden toegevoegd aan elke Linux VM (na controle dat er een lokale cache nog niet).

Er zijn een aantal verschillende DNS-caching pakketten beschikbaar, bijvoorbeeld dnsmasq, dnsmasq op de meest voorkomende distros installeren de volgende stappen uitvoeren:

- **Ubuntu (resolvconf gebruikt)**:
    - Installeer alleen het pakket dnsmasq ("sudo apt-get install dnsmasq").
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

> [AZURE.NOTE] Het pakket "dnsmasq" is slechts een van de vele DNS-cache beschikbaar voor Linux.  Controleer voordat u deze gebruikt, geschikt is voor uw specifieke behoeften en geen andere cache is geïnstalleerd.

**Client-side pogingen:**

DNS is voornamelijk een UDP-protocol.  Als het UDP-protocol garandeert aflevering van berichten niet, wordt in het DNS-protocol zelf logica opnieuw verwerkt.  Elke DNS-client (besturingssysteem) kan vertonen verschillende Pogingslogica afhankelijk van de makers van voorkeur:

 - Windows-besturingssystemen opnieuw proberen na 1 tweede en vervolgens opnieuw na een ander 2, 4 en een 4 seconden. 
 - De standaard Linux installatie opnieuw proberen na vijf seconden.  Het is raadzaam om dit opnieuw 5 keer met 1 tweede intervallen te wijzigen.  

Om de huidige instellingen op een Linux VM, 'kat /etc/resolv.conf' en kijk naar de regel 'opties' bijv.:

    options timeout:1 attempts:5

Het bestand resolv.conf is meestal automatisch gegenereerd en niet worden bewerkt.  De specifieke stappen voor het toevoegen van de regel '-opties verschillen per distro:

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
Er zijn een aantal situaties waar uw naam resolutie behoeften kunnen gaan dan de functies van Azure, bijvoorbeeld wanneer met behulp van Active Directory-domeinen of wanneer u de DNS-omzetting tussen virtuele netwerken (vnets) vereist.  Ter dekking van deze scenario's kunt Azure u uw eigen DNS-servers gebruiken.  

DNS-servers in een virtueel netwerk kunnen DNS-query's doorsturen naar Azure van recursieve resolvers omzetten van hostnamen in het virtuele netwerk.  Bijvoorbeeld: een domein Controller (DC) uitgevoerd in Azure reageren op DNS-query's voor de domeinen en andere query's doorsturen naar Azure.  Hierdoor kan de VMs uw lokale bronnen (via de DC) en de geleverde Azure hostnamen (via de doorstuurserver) te zien.  Toegang tot de Azure recursieve resolvers is beschikbaar via het virtuele IP-168.63.129.16.

DNS-doorsturen inter-vnet DNS-omzetting kan ook en kunt uw lokale machines geleverde Azure hostnamen omzetten.  Om het oplossen van een VM-hostnaam, de DNS-server VM moet zich bevinden in hetzelfde virtuele netwerk en voor hostname forward query's naar Azure worden geconfigureerd.  Als u het DNS-achtervoegsel is anders in elk vnet, kunt u regels voor voorwaardelijk doorsturen DNS-query's verzenden naar de juiste vnet voor resolutie.  De volgende afbeelding ziet u twee vnets en een lokaal netwerk inter-vnet DNS-omzetting met behulp van deze methode te doen.  Een voorbeeld van de DNS-doorstuurserver is beschikbaar in de [galerie met Azure Quickstart sjablonen](https://azure.microsoft.com/documentation/templates/301-dns-forwarder/) en [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/301-dns-forwarder).

![Inter-vnet DNS](./media/virtual-networks-name-resolution-for-vms-and-role-instances/inter-vnet-dns.png)

Wanneer u de naam van Azure resolutie, een interne DNS-achtervoegsel (*. internal.cloudapp.net) is beschikbaar voor elke VM met behulp van DHCP.  Zo kunnen omzetten als de hostname-records in de internal.cloudapp.net zone zijn.  Wanneer u uw eigen naam resolutie oplossing, is het achtervoegsel IDN verstrekt aan VMs omdat dit problemen met andere DNS-architecturen (zoals in scenario's voor het domein behoren oplevert).  In plaats daarvan bieden we een niet-functionerende tijdelijke aanduiding (reddog.microsoft.com).  

Indien nodig, kan de interne DNS-achtervoegsel worden bepaald met behulp van PowerShell of de API:

-  Het achtervoegsel is beschikbaar via de bron van de [netwerk-interfacekaart](https://msdn.microsoft.com/library/azure/mt163668.aspx) of via de cmdlet [Get-AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt619434.aspx) voor virtuele netwerken in implementatiemodellen Resource Manager.    
-  Het achtervoegsel in klassieke implementatiemodellen, is beschikbaar via de [Get implementatie API](https://msdn.microsoft.com/library/azure/ee460804.aspx) -aanroep of via de [Get-AzureVM-Debug](https://msdn.microsoft.com/library/azure/dn495236.aspx) cmdlet.


Als uw niet op query's doorsturen naar Azure behoeften, moet u uw eigen DNS-oplossing te bieden.  De DNS-oplossing moet:

-  Juiste hostnaamherleiding, bv. via [DDNS](virtual-networks-name-resolution-ddns.md)bieden.  Opmerking Als de DDNS, moet u de DNS-record opruiming Azure van DHCP-leases zijn erg lang en opruiming uitgeschakeld verwijderen DNS-records voortijdig beëindigd. 
-  Juiste naamomzetting om omzetting van namen voor externe domein bevatten.
-  Toegankelijk (TCP en UDP op poort 53) van de clients die het dient te worden en kunnen toegang krijgen tot het internet.
-  Worden beveiligd tegen toegang vanaf internet te beperken door agents op externe dreigingen.

> [AZURE.NOTE] Wanneer u Azure VMs als DNS-servers, IPv6 moet worden uitgeschakeld voor de beste prestaties en een [Openbare IP instantieniveau](virtual-networks-instance-level-public-ip.md) moet worden toegewezen aan elke DNS-server VM.  Als u Windows Server als uw DNS-server wordt gebruikt, wordt [dit artikel](http://blogs.technet.com/b/networking/archive/2015/08/19/name-resolution-performance-of-a-recursive-windows-dns-server-2012-r2.aspx) bevat aanvullende prestatie-analyse en optimalisatie.


### <a name="specifying-dns-servers"></a>DNS-servers opgeven

Wanneer u uw eigen DNS-servers, kunt Azure u meerdere DNS-servers opgeven per virtueel netwerk of netwerk-interface (Resource Manager) / cloud service (classic).  DNS-servers die zijn opgegeven voor een cloud service netwerk interface krijgt voorrang boven die welke voor het virtuele netwerk.

> [AZURE.NOTE] Eigenschappen van het netwerk, zoals DNS-server IP-adressen, mag niet rechtstreeks vanuit Windows VMs worden bewerkt als ze tijdens de service kunnen ophalen gewist retoucheren wanneer de virtuele netwerkadapter wordt vervangen. 


Wanneer u het implementatiemodel Resource Manager, kunnen DNS-servers in de Portal, API/Templates ([vnet](https://msdn.microsoft.com/library/azure/mt163661.aspx), [nic](https://msdn.microsoft.com/library/azure/mt163668.aspx)) of PowerShell ([vnet](https://msdn.microsoft.com/library/mt603657.aspx), [nic](https://msdn.microsoft.com/library/mt619370.aspx)) worden opgegeven.

Als u het implementatiemodel klassiek gebruikt, kunnen DNS-servers voor het virtuele netwerk in de Portal of [het *Netwerk* configuratiebestand](https://msdn.microsoft.com/library/azure/jj157100)worden opgegeven.  Voor cloud services, worden de DNS-servers opgegeven via [de *Service* configuratiebestand](https://msdn.microsoft.com/library/azure/ee758710) of in PowerShell ([Nieuw AzureVM](https://msdn.microsoft.com/library/azure/dn495254.aspx)).

> [AZURE.NOTE] Als u de DNS-instellingen voor een virtuele netwerk/virtuele machine die al is geïmplementeerd wijzigen, moet u elke getroffen VM om de wijzigingen van kracht te laten starten.


## <a name="next-steps"></a>Volgende stappen

Resource Manager implementatiemodel:

- [Een virtueel netwerk maken of bijwerken](https://msdn.microsoft.com/library/azure/mt163661.aspx)
- [Maken of bijwerken van een netwerkkaart](https://msdn.microsoft.com/library/azure/mt163668.aspx)
- [Nieuwe AzureRmVirtualNetwork](https://msdn.microsoft.com/library/mt603657.aspx)
- [Nieuwe AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt619370.aspx)

 
Klassieke implementatiemodel:

- [Azure Service configuratieschema](https://msdn.microsoft.com/library/azure/ee758710)
- [Virtueel netwerk configuratieschema](https://msdn.microsoft.com/library/azure/jj157100)
- [Een virtueel netwerk configureren met behulp van een configuratiebestand netwerk](virtual-networks-using-network-configuration-file.md) 

