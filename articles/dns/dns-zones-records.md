<properties 
   pageTitle="DNS-Zones en Records | Microsoft Azure" 
   description="Overzicht van de ondersteuning voor DNS-zones en records in Microsoft Azure DNS hosting." 
   services="dns" 
   documentationCenter="na" 
   authors="jtuliani" 
   manager="carmonm" 
   editor=""/>

<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services" 
   ms.date="10/17/2016"
   ms.author="jtuliani"/>

# <a name="dns-zones-and-records"></a>DNS-zones en records

Deze pagina wordt uitgelegd dat de belangrijkste concepten van domeinen, DNS-zones, DNS-records en recordsets en hoe ze in Azure DNS worden ondersteund.

## <a name="domain-names"></a>Domeinnamen
 
Het Domain Name System is een hiërarchie van domeinen. De hiërarchie wordt gestart vanaf het domein 'wortel', waarvan de naam gewoon is**.**.  Daaronder komen domeinen op hoogste niveau, bijvoorbeeld com, "netto", "organisatie", "uk" of "jp".  Zijn onder de tweede niveau domeinen, zoals 'org.uk' of 'co.jp'. De domeinen in de DNS-hiërarchie worden wereldwijd gedistribueerd gehost door een DNS-naamservers over de hele wereld.

Een domeinnamenregister is een organisatie die het mogelijk maakt om te kopen van een domeinnaam, bijvoorbeeld 'contoso.com'.  Een domeinnaam kopen, hebt u het recht om te bepalen van de DNS-hiërarchie onder die naam, zodat u direct de naam 'www.contoso.com' op de website van uw bedrijf bijvoorbeeld. De registrar kan plaats bieden aan het domein in eigen naamservers namens u of u kunt ook alternatieve naamservers opgeven.

Azure DNS biedt een globaal verdeelde, hoge beschikbaarheid naam serverinfrastructuur die u gebruiken kunt als host voor uw domein. Host uw Azure DNS-domeinen, kunt u uw DNS-records met dezelfde referenties, API's, hulpprogramma's, facturering en ondersteuning als uw Azure services beheren.

Azure DNS ondersteunt momenteel geen aanschaf van domeinnamen. Als u een domeinnaam kopen, moet u een derde domeinnamenregister gebruiken. De registrar brengt meestal een kleine jaarlijkse vergoeding. De domeinen kunnen vervolgens in Azure DNS voor het beheer van de DNS-records worden gehost. Zie [een Azure-DNS-domein delegeren](dns-domain-delegation.md) voor meer informatie.

## <a name="dns-zones"></a>DNS-zones 

Een DNS-zone wordt gebruikt voor het hosten van de DNS-records voor een bepaald domein. Als u wilt uw domein in Azure DNS hosting, moet u voor het maken van een DNS-zone voor die domeinnaam. Elke DNS-record voor uw domein is wordt binnen deze DNS-zone gemaakt. 

Het domein 'contoso.com' kan bijvoorbeeld verschillende DNS-records, zoals 'mail.contoso.com' (voor een e-mailserver) en 'www.contoso.com' (voor een website) bevatten.

Bij het maken van een DNS-zone in Azure DNS, is de naam van de zone moet uniek zijn binnen de resourcegroep. De naam van dezelfde zone kan in een resourcegroep of een ander abonnement Azure worden gebruikt. Wanneer meerdere zones dezelfde naam hebben, wordt elke instantie verschillende adressen van naamservers toegewezen. Slechts één reeks adressen kan worden geconfigureerd met de naam domeinregistratieservice.

>[AZURE.NOTE] U hoeft niet de eigenaar van een domeinnaam aan een DNS-zone gemaakt met in Azure DNS-domeinnaam. U hoeft echter de eigenaar van het domein de Azure DNS-naamservers configureren als de servers van de juiste naam voor de naam van het domein met de naam domeinregistratieservice.

Zie [een Azure-DNS-domein delegeren](dns-domain-delegation.md)voor meer informatie.

## <a name="dns-records"></a>DNS-records

### <a name="record-types"></a>Recordtypen

Elke DNS-record heeft een naam en een type. Records worden ingedeeld in verschillende typen op basis van de gegevens die ze bevatten. Het meest voorkomende type is een 'A'-record, dat wordt een naam aan een IPv4-adres toegewezen. Een andere veelvoorkomende soort is een 'MX-record, die een naam aan een e-mailserver koppelt.

Azure DNS ondersteunt alle gangbare DNS-recordtypen: A, AAAA, CNAME, MX, NS, PTR-, SOA, SRV- en TXT.

### <a name="record-names"></a>Namen registreren

In Azure DNS records opgegeven met behulp van relatieve namen. Een *volledig gekwalificeerde* domeinnaam (FQDN) bevat de zonenaam van de dat niet het *relatieve* naam. De relatieve recordnaam 'www' in de zone 'contoso.com' geeft bijvoorbeeld de volledig gekwalificeerde naam van de 'www.contoso.com'.

Een record *apex* is een DNS-record op de hoofdmap (of *apex*) van een DNS-zone. Bijvoorbeeld in de DNS-zone 'contoso.com' heeft een record apex ook de volledig gekwalificeerde naam contoso.com (dit wordt soms een *Open* domein genoemd).  Door het Verdrag, de naam van de relatieve '@' wordt gebruikt om apex records te maken.

### <a name="record-sets"></a>Recordsets
Soms moet u meer dan één DNS-record maken met een opgegeven naam en type. Stel bijvoorbeeld dat de 'www.contoso.com' website wordt gehost op twee verschillende IP-adressen. De website van twee verschillende A-records, één voor elk IP-adres is vereist. Dit is een voorbeeld van een verzameling:

    www.contoso.com.        3600    IN  A   134.170.185.46
    www.contoso.com.        3600    IN  A   134.170.188.221

Azure DNS beheert DNS-records met *record ingesteld*. Een record set (ook wel bekend als een *resource* record set) is de verzameling van DNS-records in een zone die dezelfde naam en van hetzelfde type zijn. De meeste recordsets bevat één record maar voorbeelden zoals deze, waarin een verzameling meer dan één record bevat zijn niet ongebruikelijk.

Bijvoorbeeld, Stel dat u al een A-record 'www' in de zone 'contoso.com' hebt gemaakt, '134.170.185.46' (de eerste record hierboven) aan te wijzen voor het IP-adres.  Om de tweede record te maken zou die record toevoegen aan de bestaande record set, plaats u een nieuwe verzameling maken.

De SOA- en CNAME-record zijn uitzonderingen. De DNS-standaarden niet toestaan dat meerdere records met dezelfde naam voor deze typen, dus deze sets record slechts één record bevatten.

### <a name="time-to-live"></a>Time to live

De TTL of TTL geeft aan hoe lang elke record in de cache geplaatst door clients voordat de query opnieuw wordt uitgevoerd. In het bovenstaande voorbeeld is de TTL-waarde 3600 seconden of 1 uur.

De TTL-waarde is opgegeven voor de record is ingesteld, niet voor elke record, in Azure DNS, zodat dezelfde waarde voor alle records in deze record set wordt gebruikt.  Kunt u de TTL-waarde tussen 1 en 2.147.483.647 seconden.

### <a name="wildcard-records"></a>Records met jokertekens

Azure DNS ondersteunt [records met jokertekens](https://en.wikipedia.org/wiki/Wildcard_DNS_record). Jokertekens records worden geretourneerd in antwoord op een query met een overeenkomende naam (tenzij een geschikter is vanuit een niet-jokerteken verzameling). Joker-recordsets worden ondersteund voor alle typen behalve NS- en SOA-records.  

Maakt u een jokerteken verzameling met de naam van de record '\*'. Ook kun je een naam met '\*'als de meest linkse label, bijvoorbeeld'\*.foo'.

### <a name="cname-records"></a>CNAME-records

CNAME-record instellen kunnen niet worden gecombineerd met andere records sets met dezelfde naam. U kan bijvoorbeeld een CNAME-record instellen met de relatieve naam 'www' en een A-record maken met de relatieve naam 'www' op hetzelfde moment.

Omdat de top van de zone (naam = ‘@’) bevat altijd de NS- en SOA-recordsets die zijn gemaakt als de zone is gemaakt, kunt u een CNAME-record instellen op de top van de zone niet kan maken.

Deze beperkingen voortvloeien uit de DNS-standaarden en niet de beperkingen van DNS Azure.

### <a name="ns-records"></a>NS-records

Een set van NS-record wordt automatisch gemaakt op de top van elke zone (naam = ‘@’), en wordt automatisch verwijderd wanneer de zone wordt verwijderd (dit kan niet worden verwijderd afzonderlijk).  Kunt u de TTL-waarde van deze record, maar u kunt de records die zijn vooraf geconfigureerd om te verwijzen naar de Azure DNS-naamservers toegewezen aan de zone niet wijzigen.

U kunt maken en verwijderen van andere NS-records in de zone, niet op de top van de zone.  Hiermee kunt u het configureren van de onderliggende zones (Zie [de subdomeinen overdragen in Azure DNS](dns-domain-delegation.md#delegating-sub-domains-in-azure-dns)).

### <a name="soa-records"></a>SOA-records

Een SOA-record set wordt automatisch gemaakt op de top van elke zone (naam = ‘@’), en wordt automatisch verwijderd wanneer de zone wordt verwijderd.  SOA-records kunnen niet worden gemaakt of afzonderlijk worden verwijderd. 

U kunt alle eigenschappen van de SOA-record, met uitzondering van de eigenschap 'host', die is vooraf geconfigureerd om te verwijzen naar de naam van de primaire naam geleverd door Azure DNS wijzigen.

### <a name="spf-records"></a>SPF-records

Afzender Policy Framework (SPF)-records worden gebruikt om aan te geven welke e-mailservers zijn toegestaan voor het verzenden van e-mail voor een bepaalde domeinnaam.  Juiste configuratie van de SPF-records is het belangrijk om te voorkomen dat geadresseerden uw e-mailadres als 'ongewenste e-mail' markeren.

Een nieuw type van 'SPF-record voor dit scenario wordt oorspronkelijk geïntroduceerd door de DNS RFC's. Ter ondersteuning van oudere naamservers, mogen zij ook het gebruik van het recordtype TXT SPF records opgeven.  Deze dubbelzinnigheid leidde tot verwarring kan door [RFC 7208](http://tools.ietf.org/html/rfc7208#section-3.1)is opgelost.  Dit vermeld SPF records moeten alleen worden gemaakt met het recordtype TXT en het recordtype SPF afgeschaft. 

**SPF-records worden ondersteund door DNS Azure en moeten worden gemaakt met behulp van het recordtype TXT.** De verouderde SPF record wordt niet ondersteund. Wanneer [bestand importeren van een DNS-zone](dns-import-export.md), de SPF-records met behulp van de SPF recordtype worden geconverteerd naar het recordtype TXT. 

### <a name="srv-records"></a>SRV-records

[SRV-records](https://en.wikipedia.org/wiki/SRV_record) worden gebruikt door diverse services serverlocaties op te geven. Bij het opgeven van een SRV-record in DNS Azure:

- De *service* en het *protocol* moeten worden opgegeven als onderdeel van de naam van de recordset, voorafgegaan door een onderstrepingsteken.  Bijvoorbeeld, '\_sip. \_tcp.name'.  Is niet nodig om op te geven voor een record op de top van de zone, '@' in de recordnaam van de, gebruikt de service en het protocol, bijvoorbeeld '\_sip. \_tcp'.
- De *prioriteit*, *gewicht*, *poort*en *doel* zijn opgegeven als parameters van elke record in de recordreeks. 

## <a name="tags-and-metadata"></a>Labels en metagegevens

### <a name="tags"></a>Tags
Tags zijn een lijst met de naam / waarde-paren en door Azure Resource Manager label bronnen worden gebruikt.  Azure Resource Manager worden codes gebruikt om gefilterde weergaven van uw factuur Azure en bovendien kunt u voor het instellen van een beleid dat labels vereist zijn. Zie voor meer informatie over tags [labels gebruiken voor het ordenen van uw resources Azure](../resource-group-using-tags.md).

Azure DNS ondersteunt het gebruik van codes Azure Resource Manager bronnen voor DNS-zone.  Labels worden niet ondersteund op de DNS-record ingesteld.

### <a name="metadata"></a>Metagegevens

Als alternatief voor verzameling codes Azure DNS ondersteunt recordsets met 'metagegevens' Aantekeningen maken.  Metagegevens, net als labels, kunt u naam-waardeparen koppelen aan elke record set.  Dit kan handig zijn, bijvoorbeeld naar het doel van elke set record record zijn.  In tegenstelling tot tags, metagegevens kan niet worden gebruikt om een gefilterde weergave van uw Azure-rekening en kan niet worden opgegeven in het beleid van een Azure Resource Manager.

## <a name="limits"></a>Limieten

De volgende standaard-beperkingen gelden wanneer Azure DNS:

[AZURE.INCLUDE [dns-limits](../../includes/dns-limits.md)] 

## <a name="next-steps"></a>Volgende stappen

- Om te starten met behulp van DNS in Azure, informatie over het [maken van een DNS-zone](dns-getstarted-create-dnszone-portal.md) en [DNS-records maken](dns-getstarted-create-recordset-portal.md).
- Om te migreren van een bestaande DNS-zone, meer informatie over het [importeren en het DNS-zonebestand](dns-import-export.md).
