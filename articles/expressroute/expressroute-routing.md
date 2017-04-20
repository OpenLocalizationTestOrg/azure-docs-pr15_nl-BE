<properties
   pageTitle="Vereisten voor routering voor ExpressRoute | Microsoft Azure"
   description="Deze pagina bevat gedetailleerde vereisten voor het configureren en beheren van routering voor ExpressRoute circuits."
   documentationCenter="na"
   services="expressroute"
   authors="osamazia"
   manager="ganesr"
   editor=""/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/19/2016"
   ms.author="osamazia"/>


# <a name="expressroute-routing-requirements"></a>ExpressRoute routering eisen  

Als u wilt verbinding maken met Microsoft cloud services met behulp van ExpressRoute, moet u instellen en beheren van routering. Sommige providers connectiviteit aanbieden instellen en beheren als een beheerde service Routering. Neem contact op met uw provider verbinding te zien als ze deze service aanbieden. Zo niet, moet u voldoen aan de volgende vereisten. 

Raadpleeg het artikel [Circuits en routering domeinen](expressroute-circuit-peerings.md) voor een beschrijving van de routering sessies die vergemakkelijken connectiviteit worden ingesteld moeten in.

>[AZURE.NOTE] Microsoft biedt geen ondersteuning voor elke router redundancy protocollen (bijvoorbeeld HSRP, VRRP) voor configuraties met hoge beschikbaarheid. We vertrouwen op een paar redundante BGP sessies per peering voor hoge beschikbaarheid.

## <a name="ip-addresses-used-for-peerings"></a>IP-adressen gebruikt voor peerings

U moet een paar blokken van IP-adressen voor het configureren van routeren tussen uw netwerk en routers van Microsofts Enterprise edge (MSEEs) te reserveren. Deze sectie bevat een lijst met vereisten en beschrijft de regels met betrekking tot hoe deze IP-adressen moeten worden verkregen en gebruikt.

### <a name="ip-addresses-used-for-azure-private-peering"></a>IP-adressen gebruikt voor Azure private peering

U kunt persoonlijke IP-adressen of openbare IP-adressen configureren de peerings. Het adresbereik dat wordt gebruikt voor het configureren van routes moet niet overlappen met adresbereiken die zijn gebruikt voor het maken van virtuele netwerken in Azure. 

 - U moet een /29 reserveren subnet of subnetten voor routeringsinterfaces twee /30.
 - De subnetten die wordt gebruikt voor routering kunnen worden particuliere IP-adressen of openbare IP-adressen.
 - De subnetten moeten niet in strijd met het door de klant voor gebruik in de Microsoft cloud gereserveerd bereik.
 - Als een /29 subnet wordt gebruikt, wordt deze gesplitst in twee /30 subnetten. 
     - De eerste /30 subnet worden gebruikt voor de primaire koppeling en de tweede/30 subnet wordt gebruikt voor de tweede koppeling.
     - Voor elk van de /30 subnetten, moet u het eerste IP-adres van de /30 subnet van uw router. Microsoft gebruikt het tweede IP-adres van de /30 subnet een BGP sessie instellen.
     - U moet beide BGP sessies voor onze [beschikbaarheid SLA](https://azure.microsoft.com/support/legal/sla/) om geldig te zijn ingesteld.  

#### <a name="example-for-private-peering"></a>Voorbeeld voor private peering

Als u de a.b.c.d/29 gebruiken voor het instellen van de peering, wordt deze gesplitst in twee /30 subnetten. In het volgende voorbeeld zullen we bekijken hoe het subnet van de a.b.c.d/29 wordt gebruikt. 

a.b.c.d/29 worden opgesplitst in a.b.c.d/30 en a.b.c.d+4/30 en doorgegeven aan Microsoft via de provisioning API's. U gebruikt a.b.c.d+1 als de VRF IP voor de primaire PE en Microsoft a.b.c.d+2 verbruiken als de VRF IP voor de primaire MSEE. U gebruikt a.b.c.d+5 als de VRF IP voor de secundaire PE en Microsoft wordt a.b.c.d+6 gebruikt als de IP VRF voor de secundaire MSEE.

U kunt een geval waarin u 192.168.100.128/29 voor het instellen van persoonlijke peering selecteren. 192.168.100.128/29 bevat de adressen van 192.168.100.128 aan 192.168.100.135, waaronder:

- 192.168.100.128/30 wordt toegewezen aan link1, met de provider met behulp van 192.168.100.129 en 192.168.100.130 met Microsoft.
- 192.168.100.132/30 wordt toegewezen aan link2, met de provider met behulp van 192.168.100.133 en 192.168.100.134 met Microsoft.

### <a name="ip-addresses-used-for-azure-public-and-microsoft-peering"></a>IP-adressen voor openbare Azure en Microsoft peering

Voor het instellen van de BGP sessies moet u openbare IP-adressen die u bezit. Microsoft moet kunnen controleren of de eigendom van de IP-adressen via Internet-registers-Routering en routering Internet-registers. 

- Hebt u een unieke/29 subnet of /30 twee subnetten voor het instellen van de peering voor elke peering per ExpressRoute BGP circuit (als u meer dan één). 
- Als een /29 subnet wordt gebruikt, wordt deze gesplitst in twee /30 subnetten. 
    - De eerste /30 subnet worden gebruikt voor de primaire koppeling en de tweede/30 subnet wordt gebruikt voor de tweede koppeling.
    - Voor elk van de /30 subnetten, moet u het eerste IP-adres van de /30 subnet van uw router. Microsoft gebruikt het tweede IP-adres van de /30 subnet een BGP sessie instellen.
    - U moet beide BGP sessies voor onze [beschikbaarheid SLA](https://azure.microsoft.com/support/legal/sla/) om geldig te zijn ingesteld.

## <a name="public-ip-address-requirement"></a>Openbare IP-adres vereist 

### <a name="private-peering"></a>Private Peering 

U kunt IPv4-adressen voor openbare of particuliere voor private peering gebruiken. Wij bieden end-to-end-isolatie van uw verkeer overlappende adressen met andere klanten en is dus niet mogelijk bij een private peering. Deze adressen worden niet geadverteerd naar Internet. 

### <a name="public-peering"></a>Openbare Peering

De Azure openbare peering pad kunt u verbinding maken met alle services die worden gehost in Azure via de openbare IP-adressen. Zoals vermeld in de [Veelgestelde vragen over ExpessRoute](expressroute-faqs.md) services en eventuele services die worden gehost door onafhankelijke softwareleveranciers op Microsoft Azure. Connectiviteit met Microsoft Azure services op openbare peering is altijd afkomstig van het netwerk in het Microsoft-netwerk. U moet de openbare IP-adressen gebruiken voor het verkeer dat bestemd is voor Microsoft-netwerk.

### <a name="microsoft-peering"></a>Microsoft Peering

Het Microsoft peering pad kunt u verbinding maken met Microsoft cloud services die niet worden ondersteund door de Azure openbare peering pad. De lijst met services bevat Office 365-services, zoals Exchange Online, SharePoint Online, Skype voor Business en CRM Online. Microsoft ondersteunt bidirectionele verbinding op de peering van Microsoft. Verkeer dat bestemd is voor Microsoft cloud services moet geldige openbare IPv4-adressen gebruiken voordat ze het Microsoft-netwerk invoeren.

Zorg ervoor dat uw IP-adres en als een nummer aan u in een van de hieronder vermelde registers zijn ingeschreven.

- [ARIN](https://www.arin.net/)
- [APNIC](https://www.apnic.net/)
- [AFRINIC](https://www.afrinic.net/)
- [LACNIC](http://www.lacnic.net/)
- [RIPENCC](https://www.ripe.net/)
- [RADB](http://www.radb.net/)
- [ALTDB](http://altdb.net/)

>[AZURE.IMPORTANT] Openbare IP-adressen die aan Microsoft is aangekondigd via ExpressRoute moeten niet aangekondigd met het Internet. Deze worden verbindingen met andere Microsoft-diensten verbroken. Openbare IP-adressen die worden gebruikt door servers in uw netwerk die met O365 eindpunten binnen Microsoft communiceren kunnen echter worden geadverteerd via ExpressRoute. 

## <a name="dynamic-route-exchange"></a>Dynamische route exchange

Exchange Routing worden via het eBGP-protocol. EBGP-sessies worden vastgesteld tussen de MSEEs en de routers. Verificatie van BGP sessies is niet vereist. Indien nodig, kan een MD5-hash worden geconfigureerd. Zie [routering configureren](expressroute-howto-routing-classic.md) en [Circuit inrichten van workflows en Staten circuit](expressroute-workflows.md) voor meer informatie over het configureren van BGP sessies.

## <a name="autonomous-system-numbers"></a>Autonoom systeem nummers

Microsoft gebruikt als 12076 voor Azure openbare, persoonlijke Azure en peering van Microsoft. We hebben ASN's gereserveerd in 65515 met 65520 voor intern gebruik. 16 en 32-bits als getallen worden ondersteund.

Er zijn geen vereisten om gegevens overdracht symmetrie. De vooruit- en paden kunnen paren van verschillende router passeren. Identieke routes moeten over meerdere circuit paren die u aan beide zijden worden aangekondigd. Route-metrics hoeven niet identiek te zijn.

## <a name="route-aggregation-and-prefix-limits"></a>Aggregatie-en voorvoegsel route

Ondersteuning van maximaal 4.000 prefixen aan ons via de Azure private peering. Dit kan worden verhoogd tot 10.000 prefixen als de ExpressRoute premium-invoegtoepassing is ingeschakeld. We accepteren maximaal 200 voorvoegsels per sessie voor openbare Azure BGP en peering van Microsoft. 

Het BGP-sessie wordt verwijderd als het aantal voorvoegsels overschrijdt. We accepteert standaardroutes op alleen de persoonlijke peering verbinding. Provider moet een filter standaardroute en particuliere IP-adressen (RFC 1918) uit de openbare Azure en Microsoft peering paden. 

## <a name="transit-routing-and-cross-region-routing"></a>Transit-Routering en routering van cross-regio

ExpressRoute kan niet worden geconfigureerd als routers voor communautair douanevervoer. U moet zijn afhankelijk van uw provider verbinding voor doorvoer routeringsservices.

## <a name="advertising-default-routes"></a>Standaardroutes reclame

Standaardroutes zijn alleen op Azure private peering sessies toegestaan. In dat geval zullen we al het verkeer van de bijbehorende virtuele netwerken op het netwerk route. Standaardroutes reclame in private peering resulteert in het pad van het internet van Azure wordt geblokkeerd. U moet vertrouwen op uw corporate rand voor het routeren van verkeer van en naar het internet voor services die worden gehost in Azure. 

 Als u verbindingen met andere Azure services en infrastructuurservices, moet u ervoor zorgen dat een van de volgende items aanwezig is:

 - Azure openbare peering is ingeschakeld voor het routeren van netwerkverkeer aan openbare eindpunten
 - Met door de gebruiker gedefinieerde routering kunt u internet-verbindingen voor elk subnet waarvoor Internet-verbinding toestaan.
 
>[AZURE.NOTE] Standaardroutes reclame verbreekt Windows en andere VM licentie activeren. Volg de instructies [hier](http://blogs.msdn.com/b/mast/archive/2015/05/20/use-azure-custom-routes-to-enable-kms-activation-with-forced-tunneling.aspx) om dit te omzeilen.

## <a name="support-for-bgp-communities-preview"></a>Ondersteuning voor BGP gemeenschappen (voorbeeld)


Deze sectie bevat een overzicht van hoe de BGP gemeenschappen met ExpressRoute worden gebruikt. Microsoft zal de routes in de openbare en Microsoft peering paden met routes die zijn gelabeld met de toepasselijke communautaire waarden gebruiken. Waarom doet en de details van de waarden van de Gemeenschap worden hieronder beschreven. Microsoft, echter niet van toepassing communautaire waarden tagged naar naar Microsoft aangekondigde routes.

Als u verbinding met Microsoft via ExpressRoute op één peering vestigingen binnen een geopolitieke regio maakt, hebt u toegang tot alle Microsoft cloud in alle regio's binnen de grenzen van de geopolitieke. 

Als u verbinding met Microsoft in Amsterdam via ExpressRoute, hebt u bijvoorbeeld toegang tot alle Microsoft cloud gehost in Noord-Europa en West-Europa. 

Ga naar de pagina [ExpressRoute partners en peering locaties](expressroute-locations.md) voor een gedetailleerde lijst van geopolitieke regio's, bijbehorende Azure gebieden en bijbehorende ExpressRoute peering locaties.

U kunt meer dan één ExpressRoute circuit per regio geopolitieke aanschaffen. Met meerdere verbindingen biedt aanzienlijke voordelen van hoge beschikbaarheid door geo redundantie. In gevallen waarin er meerdere ExpressRoute circuits, ontvangt u dezelfde reeks prefixen van Microsoft op de openbare peering en Microsoft peering paden. Dit betekent dat u meerdere paden hebt van uw netwerk in Microsoft. Hierdoor mogelijk sub optimale routebeslissingen worden gemaakt in uw netwerk. Daardoor kunnen er ervaringen sub optimale verbindingen met andere services. 

Microsoft zal via openbare peering prefixen labelen en Microsoft peering met de juiste BGP communautaire waarden die het gebied aangeeft de prefixen worden gehost in. U kunt vertrouwen op de waarden van de Gemeenschap om beslissingen te nemen passende routering bieden [optimale routering naar klanten](expressroute-optimize-routing.md).

| **Geopolitieke regio** | **Microsoft Azure regio** | **Waarde van de Gemeenschap BGP** |
|---|---|---|
| **Noord-Amerika** |    |  |
|    | Oost-Verenigde Staten | 12076:51004 |
|    | Oost-Verenigde Staten 2 | 12076:51005 |
|    | West-Verenigde Staten | 12076:51006 |
|    | West VS 2 | 12076:51026 |
|    | West Central US | 12076:51027 |
|    | Centrale Noord-Amerikaanse | 12076:51007 |
|    | Zuid-centraal-Verenigde Staten | 12076:51008 |
|    | Centrale VS | 12076:51009 |
|    | Canada Central | 12076:51020 |
|    | Canada Oost | 12076:51021 |
| **Zuid-Amerika** |  |  |
|    | Brazilië-Zuid | 12076:51014 |
| **Europa** |    |  |
|    | Noord-Europa | 12076:51003 |
|    | West-Europa | 12076:51002 |
| **Azië/Stille Oceaangebied** |    |   |
|    | Oost-Azië | 12076:51010 |
|    | Zuidoost-Azië | 12076:51011 |
| **Japan** |     |   |
|    | East Japan | 12076:51012 |
|    | West Japan | 12076:51013 |
| **Australië** |    |   | 
|    | Australië, Oost | 12076:51015 |
|    | Zuidoost Australië | 12076:51016 |
| **India** |    |   |
|    | Zuid India | 12076:51019 |
|    | West India | 12076:51018 |
|    | Centraal India | 12076:51017 |

Alle routes van Microsoft aangekondigd zal worden gelabeld met de toepasselijke communautaire waarde. 

>[AZURE.IMPORTANT] Globale prefixen gemarkeerd met een waarde van de desbetreffende communautaire en wordt aangekondigd als ExpressRoute premium-invoegtoepassing is ingeschakeld.


Naast de bovenstaande prefixen op basis van de service die ze naar behoren wordt ook labelen door Microsoft. Dit geldt alleen voor de peering van Microsoft. De volgende tabel vindt een toewijzing van de waarde van de Gemeenschap BGP-service.

| **Service** | **Waarde van de Gemeenschap BGP** |
|---|---|
| **Exchange** | 12076:5010 |
| **SharePoint** | 12076:5020 |
| **Skype voor bedrijven** | 12076:5030 |
| **CRM Online** | 12076:5040 |
| **Andere Office 365 diensten** | 12076:5100 |

>[AZURE.NOTE] Microsoft heeft niet inwilligen BGP communautaire waarden die u op de routes naar Microsoft aangekondigd instelt.

## <a name="next-steps"></a>Volgende stappen

- Configureer uw verbinding ExpressRoute.

    - [Een circuit voor de klassieke implementatiemodel voor ExpressRoute maken](expressroute-howto-circuit-classic.md) of [maken en wijzigen van een ExpressRoute circuit met Azure Resource Manager](expressroute-howto-circuit-arm.md)
    - [Routering voor het implementatiemodel klassiek configureren](expressroute-howto-routing-classic.md) of het [bewerkingsplan voor het implementatiemodel Resource Manager configureren](expressroute-howto-routing-arm.md)
    - [Koppeling een klassieke VNet aan een ExpressRoute circuit](expressroute-howto-linkvnet-classic.md) of [een Resource Manager VNet op een circuit ExpressRoute koppeling](expressroute-howto-linkvnet-arm.md)


