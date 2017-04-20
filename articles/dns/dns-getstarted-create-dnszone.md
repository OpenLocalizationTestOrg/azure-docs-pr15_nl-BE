<properties
   pageTitle="Aan de slag met Azure DNS | Microsoft Azure"
   description="Informatie over het maken van DNS-zones voor DNS Azure. Dit is een stap door stap om uw eerste DNS-zone gemaakt voor het hosten van uw DNS-domein met PowerShell."
   services="dns"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/16/2016"
   ms.author="sewhee"/>

# <a name="create-a-dns-zone-using-powershell"></a>Maak een DNS-zone met Powershell

> [AZURE.SELECTOR]
- [Azure Portal](dns-getstarted-create-dnszone-portal.md)
- [PowerShell](dns-getstarted-create-dnszone.md)
- [Azure CLI](dns-getstarted-create-dnszone-cli.md)

Dit artikel begeleidt u door de stappen voor het maken van een DNS-zone met PowerShell. Ook kunt u een DNS-zone met behulp van de CLI of de Azure portal.

[AZURE.INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]

## <a name="tagetag"></a>Over Etags en labels

### <a name="etags"></a>Etags

Stel dat twee personen of twee processen wilt wijzigen van een DNS-record op hetzelfde moment. Welke wins? En de winnaar weet dat ze net gemaakt door iemand anders wijzigingen hebt overschreven?

Etags Azure DNS gebruikt voor het verwerken van gelijktijdige wijzigingen in dezelfde resource veilig. Elke DNS-bronrecords (zone of recordset) heeft een Etag gekoppeld. Wanneer een resource wordt opgehaald, wordt ook de Etag opgehaald. U hebt doorgeven terug de Etag Azure DNS kunt verifiëren dat de Etag op de server komt overeen met de optie bij het bijwerken van een resource. Aangezien elke update naar een resource in de Etag opnieuw worden gegenereerd resulteert, een incompatibel Etag geeft aan dat een gelijktijdige wijziging is doorgevoerd. Etags worden ook gebruikt bij het maken van een nieuw middel om ervoor te zorgen dat de bron niet al bestaat.

Azure DNS PowerShell standaard Etags blokkeren gelijktijdige wijzigingen in zones en recordsets. De optionele *-overschrijven* switch onderdrukken Etag controles kunnen worden gebruikt, in welk geval gelijktijdige wijzigingen die zich hebben voorgedaan, worden overschreven.

Etags worden opgegeven met behulp van HTTP-headers op het niveau van de Azure DNS REST API.  Hun gedrag wordt gegeven in de volgende tabel:

|Koptekst|Gedrag|
|------|--------|
|Geen|PUT slaagt altijd (geen cheques Etag)|
|If-match<etag>|PUT slaagt alleen als de bron bestaat en Etag overeenkomt met|
|If-match *     | PUT slaagt alleen als de resource bestaat|
|If-none-match * |  PUT slaagt alleen als de resource bestaat niet|

### <a name="tags"></a>Tags

Tags zijn anders dan Etags. Tags zijn een lijst met de naam / waarde-paren en door Azure Resource Manager label bronnen voor facturering of groepeerniveau doeleinden worden gebruikt. Zie voor meer informatie over tags [labels gebruiken voor het ordenen van uw resources Azure](../resource-group-using-tags.md).

Azure PowerShell van DNS ondersteunt de Tags op zones en recordsets die zijn opgegeven met de opties `-Tag` parameter.


## <a name="before-you-begin"></a>Voordat u begint

Controleer of u de volgende items voordat u begint met uw configuratie.

- Een abonnement op Azure. Als u niet al een Azure-abonnement hebt, kunt u uw [MSDN-abonnee vergoedingen](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) of teken omhoog voor een [gratis account](https://azure.microsoft.com/pricing/free-trial/).

- U moet de meest recente versie van de Azure Resource Manager PowerShell-cmdlets (1.0 of hoger) installeren. [Het installeren en configureren van Azure PowerShell](../powershell-install-configure.md) Zie voor meer informatie over het installeren van de PowerShell-cmdlets.

## <a name="step-1---sign-in"></a>Stap 1 - aanmelden

De PowerShell-console openen en verbinding maken met uw account. Zie [Windows PowerShell gebruiken met Resource Manager](../powershell-azure-resource-manager.md)voor meer informatie.

Met het volgende voorbeeld kunt u verbinding maken:

    Login-AzureRmAccount

Controleer de abonnementen voor de account.

    Get-AzureRmSubscription

Geef het abonnement dat u wilt gebruiken.

    Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"

## <a name="step-2---create-a-resource-group"></a>Stap 2: een groep maken

Azure Resource Manager vereist dat alle bronnengroepen van een locatie opgeven. Dit wordt gebruikt als de standaardlocatie voor resources in die groep. Omdat alle DNS-bronnen algemene, niet-regionale zijn, heeft de keuze van de locatie van de resource echter geen invloed op Azure DNS.

Als u een bestaande resourcegroep gebruikt, kunt u deze stap overslaan.

    New-AzureRmResourceGroup -Name MyAzureResourceGroup -location "West US"


## <a name="step-3---register"></a>Stap 3 - registreren

De Azure DNS-service wordt beheerd door de provider van de bron Microsoft.Network. Uw abonnement op Azure dient te worden geregistreerd voor het gebruik van deze resource provider voordat u Azure DNS kunt gebruiken. Dit is een eenmalige bewerking voor elk abonnement.

    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network


## <a name="step-4----create-a-dns-zone"></a>Stap 4: een DNS-zone maken

Een DNS-zone gemaakt met de `New-AzureRmDnsZone` cmdlet. Er zijn voorbeelden hieronder voor het maken van een DNS-zone met of zonder labels. Zie de sectie van de [codes](#tags) in dit artikel voor meer informatie over tags.

>[AZURE.NOTE] In Azure DNS zonenamen moeten worden opgegeven zonder een beëindiging **'.'**. Bijvoorbeeld als '**contoso.com**' plaats '**contoso.com.**'.

### <a name="to-create-a-dns-zone"></a>Een DNS-zone maken

In het volgende voorbeeld wordt een DNS-zone *contoso.com* genoemd in de bronnengroep met de naam *MyResourceGroup*gemaakt. In het voorbeeld voor het maken van een DNS-zone, vervangen door de waarden voor eigen gebruik.

    New-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup

### <a name="to-create-a-dns-zone-with-tags"></a>Een DNS-zone met tags maken

In het volgende voorbeeld ziet u hoe een DNS-zone gemaakt met twee codes *project demo =* en *env = test*. In het voorbeeld voor het maken van een DNS-zone, vervangen door de waarden voor eigen gebruik.

    New-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup -Tag @( @{ Name="project"; Value="demo" }, @{ Name="env"; Value="test" } )

## <a name="view-records"></a>Records weergeven

Een DNS-zone maakt, maakt ook de volgende DNS-records:

- De record *Start of Authority* (SOA). Dit is aanwezig in de hoofdmap van elke DNS-zone.

- De gemachtigde naamserver (NS) records. Deze statistieken tonen welke naamservers host fungeert voor de zone. Azure DNS naamservers van een groep wordt gebruikt, en dus verschillende naamservers kunnen worden toegewezen aan de verschillende zones in Azure DNS. Zie [een Azure-DNS-domein delegeren](dns-domain-delegation.md) voor meer informatie.

Deze records wilt weergeven, gebruikt u `Get-AzureRmDnsRecordSet`:

    Get-AzureRmDnsRecordSet -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup

    Name              : @
    ZoneName          : contoso.com
    ResourceGroupName : MyResourceGroup
    Ttl               : 3600
    Etag              : 2b855de1-5c7e-4038-bfff-3a9e55b49caf
    RecordType        : SOA
    Records           : {[ns1-01.azure-dns.com,msnhst.microsoft.com,900,300,604800,300]}
    Tags              : {}

    Name              : @
    ZoneName          : contoso.com
    ResourceGroupName : MyResourceGroup
    Ttl               : 3600
    Etag              : 5fe92e48-cc76-4912-a78c-7652d362ca18
    RecordType        : NS
    Records           : {ns1-01.azure-dns.com, ns2-01.azure-dns.net, ns3-01.azure-dns.org,
                  ns4-01.azure-dns.info}
    Tags              : {}


Record ingesteld op de hoofdmap (of *apex*) van het gebruik van een DNS-Zone **@** als de naam van de record instellen.


## <a name="test"></a>Test

U kunt de DNS-zone met behulp van DNS-hulpprogramma's, zoals nslookup, graven of de [DNS-omzetting-naam PowerShell-cmdlet](https://technet.microsoft.com/library/jj590781.aspx)testen.

Als u uw gebruik van de nieuwe zone in Azure DNS-domein nog niet hebt overgedragen, moet u direct de DNS-query naar een van de naamservers voor de zone. De naamservers voor de zone worden gegeven in de NS-records, zoals vermeld door `Get-AzureRmDnsRecordSet` hierboven. Zorg ervoor dat het vervangende product de juiste waarden voor uw zone in de opdracht hieronder.

    nslookup
    > set type=SOA
    > server ns1-01.azure-dns.com
    > contoso.com

    Server: ns1-01.azure-dns.com
    Address:  208.76.47.1

    contoso.com
            primary name server = ns1-01.azure-dns.com
            responsible mail addr = msnhst.microsoft.com
            serial  = 1
            refresh = 900 (15 mins)
            retry   = 300 (5 mins)
            expire  = 604800 (7 days)
            default TTL = 300 (5 mins)


## <a name="next-steps"></a>Volgende stappen

Na het maken van een DNS-zone maken [Recordsets en records](dns-getstarted-create-recordset.md) als u wilt beginnen met het omzetten van namen voor uw Internet-domein.

