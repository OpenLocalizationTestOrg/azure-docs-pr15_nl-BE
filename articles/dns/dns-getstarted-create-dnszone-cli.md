<properties
   pageTitle="Een DNS-zone met behulp van de CLI maken | Microsoft Azure"
   description="Informatie over het maken van DNS-zones voor DNS Azure stapsgewijze hosting van uw DNS-domein met behulp van de CLI starten"
   services="dns"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/16/2016"
   ms.author="sewhee"/>

# <a name="create-an-azure-dns-zone-using-cli"></a>Maak een Azure DNS-zone met behulp van de CLI


> [AZURE.SELECTOR]
- [Azure Portal](dns-getstarted-create-dnszone-portal.md)
- [PowerShell](dns-getstarted-create-dnszone.md)
- [Azure CLI](dns-getstarted-create-dnszone-cli.md)


Dit artikel begeleidt u door de stappen voor het maken van een DNS-zone met behulp van de CLI. Ook kunt u een DNS-zone met PowerShell of de Azure portal.

[AZURE.INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]


## <a name="before-you-begin"></a>Voordat u begint

Deze instructies gebruiken Microsoft Azure CLI. Controleer of bij te werken naar de meest recente Azure CLI (0.9.8 of hoger) gebruik van Azure DNS-opdrachten. Type `azure -v` om te controleren welke Azure CLI-versie op uw computer is geïnstalleerd.

## <a name="step-1---set-up-azure-cli"></a>Stap 1 - Azure CLI instellen

### <a name="1-install-azure-cli"></a>1. Installeer Azure CLI

U kunt installeren Azure CLI voor Windows, Linux en MAC. De volgende stappen moeten worden voltooid voordat u kunt beheren met behulp van de CLI Azure Azure-DNS. Meer informatie is beschikbaar op [de Azure CLI installeren](../xplat-cli-install.md). De DNS-opdrachten vereisen Azure CLI versie 0.9.8 of hoger.

Alle opdrachten voor netwerk provider op de CLI kunnen worden gevonden met de volgende opdracht:

    azure network

### <a name="2-switch-cli-mode"></a>2. switch CLI-modus

Azure DNS Azure Resource Manager gebruikt. Zorg ervoor dat u overschakelt CLI modus als ARM-opdrachten wilt gebruiken.

    azure config mode arm

### <a name="3-sign-in-to-your-azure-account"></a>3. log in op uw account Azure

U wordt gevraagd naar uw referenties worden geverifieerd. Houd er rekening mee dat u alleen ORGID accounts kunt gebruiken.

    azure login -u "username"

### <a name="4-select-the-subscription"></a>4. Selecteer het abonnement

Kies welke van uw Azure-abonnementen te gebruiken.

    azure account set "subscription name"

### <a name="5-create-a-resource-group"></a>5. Maak een resourcegroep

Azure Resource Manager vereist dat alle bronnengroepen van een locatie opgeven. Dit wordt gebruikt als de standaardlocatie voor resources in die groep. Omdat alle DNS-bronnen algemene, niet-regionale zijn, heeft de keuze van de locatie van de resource echter geen invloed op Azure DNS.

Als u een bestaande resourcegroep gebruikt, kunt u deze stap overslaan.

    azure group create -n myresourcegroup --location "West US"


### <a name="6-register"></a>6. kassa

De Azure DNS-service wordt beheerd door de provider van de bron Microsoft.Network. Uw abonnement op Azure dient te worden geregistreerd voor het gebruik van deze resource provider voordat u Azure DNS kunt gebruiken. Dit is een eenmalige bewerking voor elk abonnement.

    azure provider register --namespace Microsoft.Network


## <a name="step-2---create-a-dns-zone"></a>Stap 2: een DNS-zone maken

Een DNS-zone gemaakt met behulp van de `azure network dns zone create` opdracht. Desgewenst kunt u een DNS-zone met codes. Tags zijn een lijst met de naam / waarde-paren en door Azure Resource Manager label bronnen voor facturering of groepeerniveau doeleinden worden gebruikt. Zie voor meer informatie over tags [labels gebruiken voor het ordenen van uw resources Azure](../resource-group-using-tags.md).

In Azure DNS zonenamen moeten worden opgegeven zonder een beëindiging **'.'**. Bijvoorbeeld als '**contoso.com**' plaats '**contoso.com.**'.


### <a name="to-create-a-dns-zone"></a>Een DNS-zone maken

In het volgende voorbeeld wordt een DNS-zone *contoso.com* genoemd in de bronnengroep met de naam *MyResourceGroup*gemaakt.

In het voorbeeld voor het maken van de DNS-zone, vervangen door de waarden voor uw eigen gebruik.

    azure network dns zone create myresourcegroup contoso.com

### <a name="to-create-a-dns-zone-and-tags"></a>Een DNS-zone en labels maken.

Azure DNS CLI ondersteunt tags van DNS-zones die zijn opgegeven met de optionele *-Tag* parameter. In het volgende voorbeeld ziet u hoe u een DNS-zone gemaakt met twee codes, project = demo en env = test.

In het volgende voorbeeld gebruiken om een DNS-zone en tags, vervangen door de waarden voor uw eigen te maken.

    azure network dns zone create myresourcegroup contoso.com -t "project=demo";"env=test"

## <a name="view-records"></a>Records weergeven

Een DNS-zone maakt, maakt ook de volgende DNS-records:

- De record 'Start of Authority' (SOA). Dit is aanwezig in de hoofdmap van elke DNS-zone.

- De gemachtigde naamserver (NS) records. Deze statistieken tonen welke naamservers host fungeert voor de zone. Azure DNS naamservers van een groep wordt gebruikt, en dus verschillende naamservers kunnen worden toegewezen aan de verschillende zones in Azure DNS. Zie [een Azure-DNS-domein delegeren](dns-domain-delegation.md) voor meer informatie.

Deze records wilt weergeven, gebruikt u `azure network dns-record-set show`.<BR>
*Syntaxis: netwerk dns record set weergeven < resourcegroep >< dns-zone-naam > <name><type>*


In het volgende voorbeeld als u de opdracht met de resource groep *myresourcegroup uitvoeren*record instellen de naam *"@"* (voor een record root) en type van *SOA*, resulteert dit in de volgende uitvoer:


    azure network dns record-set show myresourcegroup "contoso.com" "@" SOA
    info:    Executing command network dns-record-set show
    + Looking up the DNS record set "@"
    data:    Id                              : /subscriptions/#######################/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/SOA/@
    data:    Name                            : @
    data:    Type                            : Microsoft.Network/dnszones/SOA
    data:    Location                        : global
    data:    TTL                             : 3600
    data:    SOA record:
    data:      Email                         : msnhst.microsoft.com
    data:      Expire time                   : 604800
    data:      Host                          : edge1.azuredns-cloud.net
    data:      Minimum TTL                   : 300
    data:      Refresh time                  : 900
    data:      Retry time                    : 300
    data:                                    :
<BR>
De NS-records gemaakt met de zone wilt weergeven, gebruikt u de volgende opdracht:

    azure network dns record-set show myresourcegroup "contoso.com" "@" NS
    info:    Executing command network dns-record-set show
    + Looking up the DNS record set "@"
    data:    Id                              : /subscriptions/#######################/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/NS/@
    data:    Name                            : @
    data:    Type                            : Microsoft.Network/dnszones/NS
    data:    Location                        : global
    data:    TTL                             : 3600
    data:    NS records
    data:        Name server domain name     : ns1-05.azure-dns.com
    data:        Name server domain name     : ns2-05.azure-dns.net
    data:        Name server domain name     : ns3-05.azure-dns.org
    data:        Name server domain name     : ns4-05.azure-dns.info
    data:
    info:    network dns-record-set show command OK

>[AZURE.NOTE] Record ingesteld op de hoofdmap (of *apex*) van het gebruik van een DNS-Zone **@** als de naam van de record instellen.

## <a name="test"></a>Test

U kunt de DNS-zone met behulp van DNS-hulpprogramma's, zoals nslookup, GRAVEN, testen of de `Resolve-DnsName` PowerShell-cmdlet.

Als u uw gebruik van de nieuwe zone in Azure DNS-domein nog niet hebt overgedragen, moet u direct de DNS-query naar een van de naamservers voor de zone. De naamservers voor de zone zijn vermeld in de NS-records zoals bovenstaande door 'show azure netwerk DNS-record instellen'. Zorg ervoor dat het vervangende product de juiste waarden voor de tijdzone in de opdracht hieronder.

GRAVEN wordt in het volgende voorbeeld query het domein contoso.com met behulp van de naamservers die is toegewezen voor de DNS-zone. De query heeft om te verwijzen naar een naamserver die we gebruikt * @ * en GRAVEN met zonenaam.

     <<>> DiG 9.10.2-P2 <<>> @ns1-05.azure-dns.com contoso.com
    (1 server found)
    global options: +cmd
    Got answer:
    ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 60963
    flags: qr aa rd; QUERY: 1, ANSWER: 0, AUTHORITY: 1, ADDITIONAL: 1
    WARNING: recursion requested but not available

    OPT PSEUDOSECTION:
    EDNS: version: 0, flags:; udp: 4000
    QUESTION SECTION:
    contoso.com.                        IN      A

    AUTHORITY SECTION:
    contoso.com.         300     IN      SOA     edge1.azuredns-cloud.net.
    msnhst.microsoft.com. 6 900 300 604800 300

    Query time: 93 msec
    SERVER: 208.76.47.5#53(208.76.47.5)
    WHEN: Tue Jul 21 16:04:51 Pacific Daylight Time 2015
    MSG SIZE  rcvd: 120

## <a name="next-steps"></a>Volgende stappen

Na het maken van een DNS-zone maken [Recordsets en records](dns-getstarted-create-recordset-cli.md) als u wilt beginnen met het omzetten van namen voor uw Internet-domein.
