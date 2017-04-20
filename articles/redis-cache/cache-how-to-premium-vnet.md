<properties 
    pageTitle="Het configureren van virtuele netwerk ondersteuning voor een Cache Premium Azure bestand Vgx. | Microsoft Azure" 
    description="Meer informatie over het maken en beheren van virtuele netwerk ondersteuning voor uw sessies Premium tier Azure-Cache bestand Vgx." 
    services="redis-cache" 
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/15/2016" 
    ms.author="sdanie"/>

# <a name="how-to-configure-virtual-network-support-for-a-premium-azure-redis-cache"></a>Ondersteuning voor virtuele netwerken configureren voor een Cache Premium Azure bestand Vgx.
Azure Cache bestand Vgx. levert verschillende cache-oplossingen die zorgen voor flexibiliteit in de keuze van de grootte van de cache en functies, waaronder de nieuwe Premium-laag.

Azure bestand Vgx. Cache premium laag biedt onder meer clustering, persistentie en ondersteuning voor virtuele netwerken (VNet). Een VNet is een particulier netwerk in de cloud. Wanneer u een exemplaar van het bestand Vgx. Azure-Cache is geconfigureerd met een VNet, is niet openbaar toegankelijk en is alleen toegankelijk vanuit virtuele machines en toepassingen binnen de VNet. In dit artikel wordt beschreven hoe virtueel netwerkondersteuning configureren voor een exemplaar van premium Azure bestand Vgx. Cache.

>[AZURE.NOTE] Azure Cache bestand Vgx. ondersteunt zowel klassiek en ARM VNets.

Zie voor informatie over andere functies van de premium-cache, [Inleiding tot de Azure bestand Vgx. Cache Premium laag](cache-premium-tier-intro.md).

## <a name="why-vnet"></a>Waarom VNet?
Implementatie van [Azure Virtual Network (VNet)](https://azure.microsoft.com/services/virtual-network/) biedt verbeterde beveiliging en isolatie voor uw Cache Azure bestand Vgx. alsmede subnetten opstellen en andere functies verder toegang te beperken tot Azure bestand Vgx. Cache.

## <a name="virtual-network-support"></a>Ondersteuning voor virtuele netwerken
Ondersteuning voor virtuele netwerken (VNet) wordt geconfigureerd op het **Nieuwe bestand Vgx. Cache** blade tijdens het maken van de cache. 

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-premium-create.md)]

Zodra u een premium prijzen laag hebt geselecteerd, kunt u VNet van Azure Cache bestand Vgx. integratie door het selecteren van een VNet die zich in dezelfde locatie als de cache en abonnement. Eerst maken door de stappen in [een virtueel netwerk met behulp van de portal Azure maken](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) of [een virtueel netwerk (klassiek) maken met behulp van de portal Azure](../virtual-network/virtual-networks-create-vnet-classic-portal.md) en vervolgens terug naar het **Nieuwe bestand Vgx. Cache** blade maken en configureren van de cache van de premie voor het gebruik van een nieuwe VNet.

Configureren van de VNet voor de nieuwe cache **Virtueel netwerk** klikt u op in het **Nieuwe bestand Vgx. Cache** blade en selecteer de gewenste VNet in de vervolgkeuzelijst.

![Virtueel netwerk][redis-cache-vnet]

Selecteer het gewenste subnet in de vervolgkeuzelijst **Subnet** en het gewenste **statisch IP-adres**opgeven. Als u een klassieke VNet met het **statische IP-adres** veld is optioneel, en als er geen is opgegeven, wordt er een gekozen uit het geselecteerde subnet.

>[AZURE.IMPORTANT] Wanneer u een Cache Azure bestand Vgx. implementeert op een ARM VNet, moet de cache in een specifiek subnet dat geen andere bronnen, met uitzondering van Azure bestand Vgx. Cache exemplaren bevat. Als wordt geprobeerd een Cache Azure bestand Vgx. implementeren op een ARM-VNet met een subnet dat andere bronnen bevat, mislukt de installatie.

![Virtueel netwerk][redis-cache-vnet-ip]

>[AZURE.IMPORTANT] De eerste vier adressen in een subnet zijn gereserveerd en kunnen niet worden gebruikt. Zie voor meer informatie [zijn er beperkingen voor het gebruik van IP-adressen binnen deze subnetten?](../virtual-network/virtual-networks-faq.md#are-there-any-restrictions-on-using-ip-addresses-within-these-subnets)

Nadat de cache is gemaakt, kunt u de configuratie voor de VNet kunt weergeven door te klikken op **Virtueel netwerk** van de bladeserver **Instellingen** .

![Virtueel netwerk][redis-cache-vnet-info]


Opgeven voor de verbinding met uw exemplaar van de cache Azure bestand Vgx. bij gebruik van een VNet wordt de hostnaam van de cache in de verbindingsreeks, zoals in het volgende voorbeeld wordt getoond.

    private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
    {
        return ConnectionMultiplexer.Connect("contoso5premium.redis.cache.windows.net,abortConnect=false,ssl=true,password=password");
    });
    
    public static ConnectionMultiplexer Connection
    {
        get
        {
            return lazyConnection.Value;
        }
    }

## <a name="azure-redis-cache-vnet-faq"></a>Cache VNet Veelgestelde vragen over het bestand Vgx Azure.

De volgende lijst bevat antwoorden op enkele veelgestelde vragen over de schaal van Azure bestand Vgx. Cache.

-   [Wat zijn enkele onjuiste configuratie problemen met Azure bestand Vgx. Cache en VNets?](#what-are-some-common-misconfiguration-issues-with-azure-redis-cache-and-vnets)
-   [Kan ik VNets met een standaard of eenvoudige cache gebruiken?](#can-i-use-vnets-with-a-standard-or-basic-cache)
-   [Waarom maakt een bestand Vgx. cache faalt in sommige subnetten, maar andere niet?](#why-does-creating-a-redis-cache-fail-in-some-subnets-but-not-others)
-   [Werken alle functies van de cache bij het hosten van een cache in een VNET?](#do-all-cache-features-work-when-hosting-a-cache-in-a-vnet)


## <a name="what-are-some-common-misconfiguration-issues-with-azure-redis-cache-and-vnets"></a>Wat zijn enkele onjuiste configuratie problemen met Azure bestand Vgx. Cache en VNets?

Wanneer Azure bestand Vgx. Cache wordt gehost in een VNet, worden de poorten in de volgende tabel worden gebruikt. Als deze poorten zijn geblokkeerd, is het mogelijk dat de cache niet goed werkt. Met een of meer van deze poorten geblokkeerd is het meest voorkomende probleem met onjuiste configuratie als u bestand Vgx. Azure-Cache in een VNet.

| Poort(en)     | Richting        | Transportprotocol | Doel                                                                           | RAS IP                           |
|-------------|------------------|--------------------|-----------------------------------------------------------------------------------|-------------------------------------|
| 80, 443     | Uitgaande         | TCP                | Bestand Vgx afhankelijkheden op Azure opslag/PKI (Internet).                                | *                                   |
| 53          | Uitgaande         | TCP/UDP-            | Bestand Vgx afhankelijk is van de DNS (Internet/VNet).                                         | *                                   |
| 6379, 6380  | Inkomend          | TCP                | Mededeling van de client aan het bestand Vgx. Azure Load Balancing                               | VIRTUAL_NETWORK, AZURE_LOADBALANCER |
| 8443        | Inkomende/uitgaande | TCP                | Details van de uitvoering van het bestand Vgx.                                                   | VIRTUAL_NETWORK                     |
| 8500        | Inkomend          | TCP/UDP-            | Azure Load Balancing                                                              | AZURE_LOADBALANCER                  |
| 10221 10231 | Inkomende/uitgaande | TCP                | Details van de implementatie voor bestand Vgx. (externe endpoint kunt beperken tot VIRTUAL_NETWORK) | VIRTUAL_NETWORK, AZURE_LOADBALANCER |
| 13000 13999 | Inkomend          | TCP                | Clientcommunicatie naar bestand Vgx. Clusters Azure Load Balancing                      | VIRTUAL_NETWORK, AZURE_LOADBALANCER |
| 15000-15999 | Inkomend          | TCP                | Clientcommunicatie naar bestand Vgx. Clusters Azure Load Balancing                      | VIRTUAL_NETWORK, AZURE_LOADBALANCER |
| 16001       | Inkomend          | TCP/UDP-            | Azure Load Balancing                                                              | AZURE_LOADBALANCER                  |
| 20226       | Inkomend + uitgaand | TCP                | Details van de implementatie voor Clusters bestand Vgx.                                          | VIRTUAL_NETWORK                     |


Zijn er vereisten voor connectiviteit voor Azure bestand Vgx. Cache die niet in eerste instantie kan worden voldaan in een virtueel netwerk. Azure bestand Vgx. Cache vereist voor alle volgende items goed werken als in een virtueel netwerk gebruikt.

-  Uitgaande netwerkverbindingen met Azure opslag eindpunten wereldwijd. Dit omvat de eindpunten zich in hetzelfde gebied, als de instantie Azure bestand Vgx. Cache ook opslag eindpunten zich in **andere** gebieden Azure. Azure opslag eindpunten oplossen onder de volgende DNS-domeinen: *table.core.windows.net*, *blob.core.windows.net*, *queue.core.windows.net*en *file.core.windows.net*. 
-  Uitgaande netwerkverbindingen met *ocsp.msocsp.com*, *mscrl.microsoft.com* en *crl.microsoft.com*. Deze verbinding nodig voor ondersteuning van SSL-functionaliteit.
-  De DNS-configuratie voor het virtuele netwerk van het oplossen van alle domeinen die worden vermeld in de vorige punten en eindpunten kunnen worden. Deze DNS-vereisten kunnen worden voldaan door te zorgen voor een geldige DNS-infrastructuur is geconfigureerd en onderhouden voor het virtuele netwerk.



### <a name="can-i-use-vnets-with-a-standard-or-basic-cache"></a>Kan ik VNets met een standaard of eenvoudige cache gebruiken?

VNets kan alleen worden gebruikt met premium-caches.

### <a name="why-does-creating-a-redis-cache-fail-in-some-subnets-but-not-others"></a>Waarom maakt een bestand Vgx. cache faalt in sommige subnetten, maar andere niet?

Als u een Cache Azure bestand Vgx. naar een ARM VNet implementeert, moet de cache in een specifiek subnet met andere brontype. Als wordt geprobeerd een Cache Azure bestand Vgx. implementeren op een subnet ARM VNet met andere bronnen, mislukt de installatie. Voordat u een nieuw bestand Vgx. cache kunt maken, moet u de bestaande bronnen binnen het subnet verwijderen.

U kunt meerdere typen bronnen implementeren op een klassieke VNet, zolang er voldoende beschikbare IP-adressen.

### <a name="do-all-cache-features-work-when-hosting-a-cache-in-a-vnet"></a>Werken alle functies van de cache bij het hosten van een cache in een VNET?

Als de cache is een onderdeel van een VNET, toegang alleen clients in de VNET de cache. Hierdoor werken de volgende functies voor cache niet op dit moment.

-   Bestand Vgx. Console - omdat het bestand Vgx. Console gebruikt het bestand Vgx. cli.exe client gehost op VMs die geen deel uitmaken van de VNET, er kan geen verbinding met uw cache.


## <a name="use-expressroute-with-azure-redis-cache"></a>ExpressRoute gebruiken met Cache bestand Vgx Azure.

Klanten kunnen verbinden met een circuit [Azure ExpressRoute](https://azure.microsoft.com/services/expressroute/) hun infrastructuur virtueel netwerk uitbreiden, dus hun netwerk op ruimten naar Azure. 

Een zojuist gemaakte ExpressRoute circuit wordt standaard een standaardroute waarmee uitgaande Internet-verbinding. Met deze configuratie kunnen client-toepassingen verbinding maken met andere Azure eindpunten, met inbegrip van Azure bestand Vgx. Cache.

Een algemene configuratie voor de klant is echter hun eigen standaardroute (0.0.0.0/0), waardoor de uitgaande Internet-verkeer toe in plaats daarvan op ruimten te definiëren. Connectiviteit met Azure bestand Vgx. Cache verbroken deze verkeersstroom altijd omdat het uitgaande verkeer een geblokkeerde op-lokalen wordt of NAT zou een onherkenbare set adressen die niet meer met verschillende Azure eindpunten werken.

De oplossing is voor het definiëren van een (of meer) door de gebruiker gedefinieerde routes (UDRs) op het subnet waarin de Azure bestand Vgx. Cache. Een UDR definieert de subnet-specifieke routes die in plaats van de standaardroute wordt ingewilligd.

Indien mogelijk verdient het gebruik van de volgende configuratie:

- De configuratie ExpressRoute 0.0.0.0/0 adverteert en standaard tunnels kracht alle uitgaand verkeer op-ruimten.
- De UDR toegepast op het subnet waarin de Azure bestand Vgx. Cache definieert 0.0.0.0/0 van het type volgende hop Internet (een voorbeeld hiervan is verder naar beneden in dit artikel).

Het gecombineerde effect van deze stappen is dat subnetniveau UDR voorrang op de gedwongen ExpressRoute tunneling heeft, uitgaande Internet-toegang via de Azure bestand Vgx. Cache om te kunnen garanderen.

Hoewel verbinding te maken met een exemplaar van het bestand Vgx. Azure-Cache van een plaats op toepassing met behulp van ExpressRoute is niet een typische gebruiksscenario om redenen van prestaties (voor de beste prestaties Azure bestand Vgx. Cache-clients moeten zich in hetzelfde gebied, als de Cache Azure bestand Vgx.) in dit scenario is die het uitgaande netwerkpad kan niet via interne corporate proxy's reizen, noch kracht tunnelverbinding tot op de gebouwen worden. Op die manier verandert het effectieve NAT-adres van het uitgaande netwerkverkeer uit de Cache Azure bestand Vgx.. Het NAT-adres van een Cache Azure bestand Vgx. uitgaand netwerkverkeer van de instantie zorgt ervoor dat storingen tot veel van de hierboven vermelde eindpunten connectiviteit. Dit resulteert in mislukte pogingen voor Azure bestand Vgx. Cache maken.

**Belangrijk:**  De routes die zijn gedefinieerd in een UDR **moet** worden specifieke voorrang op alle routes die zijn aangekondigd door de configuratie ExpressRoute. In het volgende voorbeeld gebruikt het adresbereik brede 0.0.0.0/0 en als zodanig kan mogelijk worden per ongeluk overschreven door route-advertisements met meer specifieke adresbereiken.

**Zeer belangrijk:**  Azure bestand Vgx. Cache is niet ondersteund met ExpressRoute configuraties die **ten onrechte cross - routes adverteren van het openbare peering pad naar de private peering pad**. ExpressRoute configuraties met openbare peering is geconfigureerd, wordt voor een groot aantal Microsoft Azure IP-adresbereiken route-advertisements ontvangen van Microsoft. Als de adresbereiken van deze onjuist cross geadverteerd op de private peering pad, is het eindresultaat dat alle uitgaande pakketten van het subnet van de instantie Azure bestand Vgx. Cache onjuist force getunnelde aan infrastructuur in gebouwen van de klant. Deze overdracht netwerk verbroken Azure bestand Vgx. Cache. De oplossing voor dit probleem is niet meer cross-reclame routes van het openbare peering pad naar de private peering pad.

Achtergrondinformatie over routes door de gebruiker gedefinieerde is beschikbaar in het [Overzicht](../virtual-network/virtual-networks-udr-overview.md). 

Zie voor meer informatie over ExpressRoute, [ExpressRoute technisch overzicht](../expressroute/expressroute-introduction.md)

## <a name="next-steps"></a>Volgende stappen
Leer meer cache premium-functies gebruiken.

-   [Inleiding tot het niveau van de premie van Azure Cache bestand Vgx.](cache-premium-tier-intro.md)





  
<!-- IMAGES -->

[redis-cache-vnet]: ./media/cache-how-to-premium-vnet/redis-cache-vnet.png

[redis-cache-vnet-ip]: ./media/cache-how-to-premium-vnet/redis-cache-vnet-ip.png

[redis-cache-vnet-info]: ./media/cache-how-to-premium-vnet/redis-cache-vnet-info.png

