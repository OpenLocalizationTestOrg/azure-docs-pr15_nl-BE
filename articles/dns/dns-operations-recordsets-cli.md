<properties
   pageTitle="Beheren van DNS-record sets en records op Azure DNS met behulp van de CLI Azure | Microsoft Azure"
   description="Records op Azure DNS en DNS-record sets beheren wanneer uw Azure DNS-domein hosting. Alle CLI-opdrachten voor bewerkingen op records en recordsets."
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
   ms.date="09/22/2016"
   ms.author="jtuliani"/>

# <a name="manage-dns-records-and-record-sets-by-using-cli"></a>DNS-records beheren en record wordt ingesteld met behulp van CLI


> [AZURE.SELECTOR]
- [Azure Portal](dns-operations-recordsets-portal.md)
- [Azure CLI](dns-operations-recordsets-cli.md)
- [PowerShell](dns-operations-recordsets.md)


In dit artikel wordt beschreven hoe u records voor uw DNS-zone en recordsets beheren via meerdere platforms Azure opdrachtregelinterface (CLI).

Het is belangrijk te begrijpen het verschil tussen de DNS-record sets en afzonderlijke DNS-records. Een verzameling is een verzameling van records in een zone met dezelfde naam en hetzelfde type. Zie voor meer informatie, [wat recordsets en records](dns-getstarted-create-recordset-cli.md).


## <a name="configure-the-cross-platform-azure-cli"></a>De cross-platform Azure CLI configureren

Azure DNS is een Azure Resource Manager-service. Het hoeft niet een Azure Service Management API. Zorg ervoor dat de CLI Azure is geconfigureerd voor het gebruik van Resource Manager-modus met behulp van de `azure config mode arm` opdracht.

Als er **Fout: 'dns' is niet een azure-opdracht**, is waarschijnlijk omdat u Azure CLI in Azure Service Management-modus, niet in de modus Resource Manager.

## <a name="create-a-new-record-set-and-record"></a>Maak een nieuwe record en record

Zie maken van een nieuwe record is ingesteld in de portal Azure [een record en records te maken](dns-getstarted-create-recordset-cli.md).


## <a name="retrieve-a-record-set"></a>Een verzameling ophalen

U haalt een bestaande record set met `azure network dns record-set show`. Geef de resourcegroep, de naam van de zone, record relatieve naam en type record ingesteld. Het volgende voorbeeld wordt de waarden te vervangen door uw eigen gebruik.

    azure network dns record-set show myresourcegroup contoso.com www A


## <a name="list-record-sets"></a>Lijst met recordsets

U kunt alle records in een DNS-zone weergeven met behulp van de `azure network dns record-set list` opdracht. U moet de naam van de resource en de zonenaam opgeven.

### <a name="to-list-all-record-sets"></a>Overzicht van alle recordsets

In het volgende voorbeeld wordt alle recordsets, ongeacht de naam of het type record:

    azure network dns record-set list myresourcegroup contoso.com

### <a name="to-list-record-sets-of-a-given-type"></a>Aan recordsets van een bepaald type lijst

In het volgende voorbeeld wordt alle recordsets die overeenkomen met het opgegeven type record (in dit geval 'A'-records):

    azure network dns record-set list myresourcegroup contoso.com A


## <a name="add-a-record-to-a-record-set"></a>Een record toevoegen aan een verzameling

U records toevoegen aan een record wordt ingesteld met behulp van de `azure network dns record-set add-record`opdracht. De parameters voor het toevoegen van records aan een verzameling variëren afhankelijk van het type van de record die u instelt. Bijvoorbeeld wanneer u een record gebruiken van het type "A", kunt u alleen opgeven records met de parameter `-a <IPv4 address>`.

U kunt een verzameling maken de `azure network dns record-set create`opdracht. Geef de resourcegroep, de naam van de zone, record relatieve naam, het recordtype en tijd live (TTL) ingesteld. Als de `--ttl` parameter is niet gedefinieerd, wordt de standaardwaarde (in seconden) 4.

    azure network dns record-set create myresourcegroup  contoso.com "test-a"  A --ttl 300


Nadat u de record "A" set gemaakt, het IPv4-adres toevoegen met behulp van de `azure network dns record-set add-record`opdracht.

    azure network dns record-set add-record myresourcegroup contoso.com "test-a" A -a 192.168.1.1


De volgende voorbeelden laten zien hoe een recordset maken van elk type record. Elke record bevat één record.

[AZURE.INCLUDE [dns-add-record-cli-include](../../includes/dns-add-record-cli-include.md)]


## <a name="update-a-record-in-a-record-set"></a>Een record in een recordset bij te werken

### <a name="to-add-another-ip-address-1234-to-an-existing-a-record-set-www"></a>Als u wilt een ander IP-adres (1.2.3.4) toevoegen aan een bestaande A-record ('www') in te stellen:

    azure network dns record-set add-record  myresourcegroup contoso.com  A
    -a 1.2.3.4
    info:    Executing command network dns record-set add-record
    Record set name: www
    + Looking up the dns zone "contoso.com"
    + Looking up the DNS record set "www"
    + Updating DNS record set "www"
    data:    Id                              : /subscriptions/################################/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/a/www
    data:    Name                            : www
    data:    Type                            : Microsoft.Network/dnszones/a
    data:    Location                        : global
    data:    TTL                             : 4
    data:    A records:
    data:        IPv4 address                : 192.168.1.1
    data:        IPv4 address                : 1.2.3.4
    data:
    info:    network dns record-set add-record command OK

### <a name="to-remove-an-existing-value-from-a-record-set"></a>Een bestaande waarde uit een verzameling verwijderen
Gebruik `azure network dns record-set delete-record`.

    azure network dns record-set delete-record myresourcegroup contoso.com www A -a 1.2.3.4
    info:    Executing command network dns record-set delete-record
    + Looking up the DNS record set "www"
    Delete DNS record? [y/n] y
    + Updating DNS record set "www"
    data:    Id                              : /subscriptions/################################/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/A/www
    data:    Name                            : www
    data:    Type                            : Microsoft.Network/dnszones/A
    data:    Location                        : global
    data:    TTL                             : 4
    data:    A records:
    data:    IPv4 address                    : 192.168.1.1
    data:
    info:    network dns record-set delete-record command OK



## <a name="remove-a-record-from-a-record-set"></a>Een record verwijderen uit een verzameling

Records kunnen worden verwijderd uit een record instellen met behulp van `azure network dns record-set delete-record`. De record die u wilt verwijderen moet exact overeenkomen met een bestaande record over alle parameters.

De verzameling worden niet verwijderd als u de laatste record verwijderen uit een verzameling. Zie de sectie van dit artikel [een set record verwijderen](#delete)voor meer informatie.

    azure network dns record-set delete-record myresourcegroup contoso.com www A -a 192.168.1.1

    azure network dns record-set delete myresourcegroup contoso.com www A

### <a name="remove-an-aaaa-record-from-a-record-set"></a>Een AAAA-record verwijderen uit een verzameling

    azure network dns record-set delete-record myresourcegroup contoso.com test-aaaa  AAAA -b "2607:f8b0:4009:1803::1005"

### <a name="remove-a-cname-record-from-a-record-set"></a>Een CNAME-record verwijderen uit een verzameling

    azure network dns record-set delete-record myresourcegroup contoso.com test-cname CNAME -c www.contoso.com


### <a name="remove-an-mx-record-from-a-record-set"></a>Een MX-record verwijderen uit een verzameling

    azure network dns record-set delete-record myresourcegroup contoso.com "@" MX -e "mail.contoso.com" -f 5

### <a name="remove-an-ns-record-from-record-set"></a>Een NS-record verwijderen uit een Recordset

    azure network dns record-set delete-record myresourcegroup contoso.com  "test-ns" NS -d "ns1.contoso.com"

### <a name="remove-a-ptr-record-from-a-record-set"></a>Een PTR-record verwijderen uit een verzameling
In dit geval "Mijn-arpa-zone.com' de ARPA zone waarmee het IP-bereik vertegenwoordigt.  Elke PTR-record instellen in deze zone komt overeen met een IP-adres binnen het bereik van deze IP.

    azure network dns record-set delete-record myresourcegroup my-arpa-zone.com "10" PTR -P "myservice.contoso.com"

### <a name="remove-an-srv-record-from-a-record-set"></a>Een SRV-record verwijderen uit een verzameling

    azure network dns record-set delete-record myresourcegroup contoso.com  "_sip._tls" SRV -p 0 -w 5 -o 8080 -u "sip.contoso.com"

### <a name="remove-a-txt-record-from-a-record-set"></a>Een TXT-record verwijderen uit een verzameling

    azure network dns record-set delete-record myresourcegroup contoso.com  "test-TXT" TXT -x "this is a TXT record"

## <a name="delete"></a>Een verzameling verwijderen

Recordsets kunnen worden verwijderd met behulp van de `Remove-AzureRmDnsRecordSet` cmdlet. U kunt het SOA niet verwijderen en de NS-record ingesteld op de top van de zone (naam = "@") die automatisch zijn gemaakt als de zone is gemaakt. Ze worden automatisch verwijderd als de zone wordt verwijderd.

In het volgende voorbeeld wordt de A-record instellen 'test-a' verwijderd uit de 'contoso.com' DNS-zone:

    azure network dns record-set delete myresourcegroup contoso.com  "test-a" A

De optionele *q -* switch kan worden gebruikt voor de bevestigingsprompt.


## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over DNS Azure [Azure DNS-overzicht](dns-overview.md). Zie [maken van DNS-zones en record wilt instellen met de .NET SDK](dns-sdk.md)voor meer informatie over het automatiseren van DNS.

Als u werken met reverse DNS-records wilt, Zie [beheren reverse DNS-records voor uw services met behulp van de CLI Azure](dns-reverse-dns-record-operations-cli.md).
