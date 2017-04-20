<properties
   pageTitle="Importeren en exporteren van het bestand met een zone naar Azure DNS met behulp van de CLI | Microsoft Azure"
   description="Meer informatie over het importeren en exporteren van een DNS-zonebestand Azure DNS met behulp van de CLI Azure"
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

# <a name="import-and-export-a-dns-zone-file-using-the-azure-cli"></a>Importeren en exporteren van een bestand voor DNS-zone met behulp van de CLI Azure


Dit artikel helpt u bij het importeren en exporteren van DNS-zonebestanden voor Azure DNS met behulp van de CLI Azure.

## <a name="introduction-to-dns-zone-migration"></a>Inleiding tot de DNS-zone migratie

Een DNS-zone-bestand is een tekstbestand met de details van elke record Domain Name System (DNS) in de zone. Volgt een vaste indeling, zodat het geschikt is voor de DNS-records overbrengen tussen DNS-systemen. Met behulp van een zonebestand is een snelle, betrouwbare en handige manier overbrengen van een DNS-zone in- of DNS-Azure.

Azure DNS ondersteunt het importeren en exporteren van zonebestanden via Azure opdrachtregelinterface (CLI). De CLI Azure is een opdrachtregelprogramma van cross-platform gebruikt voor het beheren van services op Azure. Het is beschikbaar voor Windows, Mac en Linux-platforms van de [pagina downloads Azure](https://azure.microsoft.com/downloads/). Cross-platform ondersteuning is vooral belangrijk voor het importeren en exporteren van zonebestanden, aangezien de meest gangbare naam server-software, [BINDEN](https://www.isc.org/downloads/bind/), meestal op Linux wordt uitgevoerd.

## <a name="obtain-your-existing-dns-zone-file"></a>Uw bestaande DNS-zonebestand verkrijgen

Voordat u een DNS-zonebestand in Azure DNS importeren, moet u een exemplaar van het zonebestand. De bron van het bestand afhankelijk van waar de DNS-zone die momenteel wordt gehost.

- Als de DNS-zone die wordt gehost door een partner-service (zoals een domeinregistratieservice specifieke DNS-hosting provider of andere cloud provider), dient deze dienst de mogelijkheid voor het downloaden van het DNS-zonebestand.

- Als de DNS-zone is ondergebracht op Windows DNS, is de standaardmap voor de zonebestanden **%systemroot%\system32\dns**. Het volledige pad naar het bestand voor elke zone wordt ook weergegeven op het tabblad **Algemeen** van de DNS service management-console.

- Als de DNS-zone met behulp van BIND wordt gehost, moet de locatie van het bestand voor zone voor elke zone die is opgegeven in de BIND configuratie bestand **named.conf**.

**Werken met zonebestanden in de van GoDaddy**<BR>
Zonebestanden die zijn gedownload van GoDaddy hebben een enigszins niet-standaard indeling. Moet u deze corrigeren voordat u deze zonebestanden in Azure DNS importeren. DNS-namen in de RData van elke DNS-record worden opgegeven als volledig gekwalificeerde namen, maar hebben niet tot een beëindiging "." Dit betekent dat ze door andere DNS-systemen als relatieve namen worden geïnterpreteerd. U moet het zone bestand toevoegen de afsluitende '. ' naar hun namen voordat u deze in Azure DNS importeert.

## <a name="import-a-dns-zone-file-into-azure-dns"></a>Een DNS-zonebestand importeren in Azure DNS


Een zonebestand importeren maakt een nieuwe zone in Azure DNS als dit nog niet bestaat. Als de zone al bestaat, moeten de record sets in het zonebestand worden samengevoegd met de bestaande record groepen.

### <a name="merge-behavior"></a>Gedrag samenvoegen

- Standaard worden bestaande en nieuwe record wordt samengevoegd. Identieke records binnen een samengevoegde verzameling zijn opgeheven gedupliceerd.

- U kunt ook door te geven de `--force` optie tijdens het importeren worden bestaande vervangen recordsets met nieuwe recordsets. Bestaande record groepen die u hebt ingesteld in het zonebestand geïmporteerd een record wordt verwijderd.

- Als recordsets worden samengevoegd, wordt de tijd voor het live (TTL) van bestaande recordsets gebruikt. Wanneer `--force` wordt gebruikt, de TTL-waarde van de nieuwe recordset wordt gebruikt.

- Begin van autoriteit (SOA) parameters (behalve `host`) komt altijd van het geïmporteerde bestand, ongeacht of `--force` wordt gebruikt. Ook voor de server-record ingesteld op de top van de zone, is de TTL-waarde altijd afkomstig van de geïmporteerde zonebestand.

- Een geïmporteerde CNAME-record niet een bestaande CNAME-record met dezelfde naam vervangen tenzij de `--force` -parameter is opgegeven.

- Als een conflict optreedt tussen een CNAME-record en een andere record met dezelfde naam maar met een ander type (ongeacht welke is bestaande of nieuwe), blijft de bestaande record. Dit is afhankelijk van het gebruik van `--force`.

### <a name="additional-information-about-importing"></a>Als u meer informatie over het importeren van

De volgende opmerkingen bevatten aanvullende technische informatie over de zone importeren.

- De `$TTL` richtlijn is optioneel en wordt ondersteund. Als er geen `$TTL` richtlijn wordt gegeven, worden records zonder een expliciete TTL geïmporteerde is ingesteld op een standaardwaarde voor TTL van 3600 seconden. Wanneer twee records in dezelfde record set verschillende TTLs opgeeft, wordt de laagste waarde gebruikt.

- De `$ORIGIN` richtlijn is optioneel en wordt ondersteund. Als er geen `$ORIGIN` is ingesteld, de standaardwaarde gebruikt, wordt de naam van de zone die is opgegeven op de opdrachtregel (plus de afsluitende ".").

- De `$INCLUDE` en `$GENERATE` richtlijnen worden niet ondersteund.

- Deze typen worden ondersteund: A, AAAA, CNAME, MX, NS, SOA, SRV- en TXT.

- De SOA-record wordt automatisch gemaakt door Azure DNS als een zone is gemaakt. Als u een zonebestand importeert, alle SOA-parameters zijn afkomstig uit de zone-bestand *, met uitzondering van* de `host` parameter. Deze parameter wordt de waarde die is opgegeven door Azure DNS gebruikt. Dit is omdat deze parameter naar de van de primaire naamserver van Azure DNS verwijzen moet.

- De server-record ingesteld op de top van de zone wordt ook automatisch gemaakt door Azure DNS wanneer de zone is gemaakt. Alleen de TTL van deze record wordt geïmporteerd. Deze records bevatten de naam server-namen die door DNS Azure. De gegevens in de record niet wordt overschreven door de waarden in het geïmporteerde bestand.

- Azure DNS ondersteunt tijdens Public Preview alleen één tekenreeks TXT-records. Multistring TXT-records worden samengevoegd en afgekapt op 255 tekens.

### <a name="cli-format-and-values"></a>CLI-indeling en waarden


De indeling van de Azure CLI-opdracht voor het importeren van een DNS-zone is:<BR>`azure network dns zone import [options] <resource group> <zone name> <zone file name>`

Waarden:

- `<resource group>`is de naam van de resourcegroep voor de zone in DNS Azure.
- `<zone name>`is de naam van de zone.
- `<zone file name>`is de padnaam van het zonebestand te importeren.

Als een zone met deze naam in de resourcegroep niet bestaat, wordt deze voor u gemaakt. Als de zone al bestaat, wordt het geïmporteerde record wordt samengevoegd met bestaande recordsets. U overschrijft de bestaande record groepen, gebruikt u de `--force` optie.

Als u wilt controleren of de indeling van een zonebestand zonder deze daadwerkelijk te importeren, gebruikt u de `--parse-only` optie.

### <a name="step-1-import-a-zone-file"></a>Stap 1. Een zonebestand importeren

Importeren van een zonebestand voor de zone **contoso.com**.

1. Aanmelden bij uw abonnement Azure met behulp van de CLI Azure.

        azure login

2. Selecteer het abonnement waar u uw nieuwe DNS-zone te maken.

        azure account set <subscription name>

3. Azure DNS is een Azure Resource Manager-service, zodat de CLI Azure moet worden overgeschakeld naar de modus Resource Manager.

        azure config mode arm

4. Voordat u de Azure DNS-service gebruiken, moet u uw abonnement voor het gebruik van de resource Microsoft.Network provider registreren. (Dit is een eenmalige bewerking voor elk abonnement).

        azure provider register Microsoft.Network

5. Als u niet al een hebt, moet u ook voor het maken van een resourcegroep Resource Manager.

        azure group create myresourcegroup westeurope

6. De opdracht uitvoeren om de zone **contoso.com** importeren uit het bestand **contoso.com.txt** in een nieuwe DNS-zone in de resource groep **myresourcegroup**, `azure network dns zone import`.<BR>Deze opdracht wordt het zonebestand laden en parseren op. De opdracht wordt een reeks opdrachten uitvoeren op de Azure DNS-service voor het maken van de zone en alle van de record in de zone ingesteld. De opdracht wordt uitgevoerd in het consolevenster, samen met eventuele fouten of waarschuwingen te melden. Omdat de recordsets worden gemaakt in de reeks, duurt een paar minuten om een grote zone-bestand te importeren.

        azure network dns zone import myresourcegroup contoso.com contoso.com.txt



### <a name="step-2-verify-the-zone"></a>Stap 2. Controleer of de zone

Als u wilt controleren of de DNS-zone nadat u het bestand hebt geïmporteerd, kunt u een van de volgende methoden gebruiken:

- Met behulp van de volgende Azure CLI-opdracht kunt u de records weergeven.

        azure network dns record-set list myresourcegroup contoso.com

- U kunt de records weergeven met behulp van de PowerShell-cmdlet `Get-AzureRmDnsRecordSet`.

- U kunt `nslookup` om te controleren of naamomzetting voor de records. Omdat de zone nog niet is overgedragen, moet u de juiste Azure DNS-naamservers expliciet opgeven. Het onderstaande voorbeeld ziet u hoe met de naam server namen toegewezen aan de zone. IT ook ziet u hoe de 'www' record zoeken met behulp van `nslookup`.

        C:\>azure network dns record-set show myresourcegroup contoso.com @ NS
        info:Executing command network dns record-set show
        + Looking up the DNS Record Set "@" of type "NS"
        data:Id: /subscriptions/…/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/NS/@
        data:Name: @
        data:Type: Microsoft.Network/dnszones/NS
        data:Location: global
        data:TTL : 3600
        data:NS records
        data:Name server domain name : ns1-01.azure-dns.com
        data:Name server domain name : ns2-01.azure-dns.net
        data:Name server domain name : ns3-01.azure-dns.org
        data:Name server domain name : ns4-01.azure-dns.info
        data:
        info:network dns record-set show command OK

        C:\> nslookup www.contoso.com ns1-01.azure-dns.com

        Server: ns1-01.azure-dns.com
        Address:  40.90.4.1

        Name:www.contoso.com
        Addresses:  134.170.185.46
        134.170.188.221

### <a name="step-3-update-dns-delegation"></a>Stap 3. DNS-overdracht bijwerken

Nadat u hebt gecontroleerd dat de zone correct zijn geïmporteerd, moet u voor het bijwerken van de DNS-overdracht naar de Azure DNS-naamservers. Zie het artikel [de DNS-overdracht bijwerken](dns-domain-delegation.md)voor meer informatie.

## <a name="export-a-dns-zone-file-from-azure-dns"></a>Een DNS-zonebestand van Azure DNS exporteren

De indeling van de Azure CLI-opdracht voor het importeren van een DNS-zone is:<BR>`azure network dns zone export [options] <resource group> <zone name> <zone file name>`

Waarden:

- `<resource group>`is de naam van de resourcegroep voor de zone in DNS Azure.
- `<zone name>`is de naam van de zone.
- `<zone file name>`is de padnaam van het zonebestand te exporteren.

Als het importeren zone moet u eerst aanmelden, kies je abonnement en de Azure CLI modus Resource Manager configureren.

### <a name="to-export-a-zone-file"></a>Een zonebestand exporteren


1. Aanmelden bij uw abonnement Azure met behulp van de CLI Azure.

        azure login

2. Selecteer het abonnement waar u uw nieuwe DNS-zone te maken.

        azure account set <subscription name>

3. Azure DNS is een Azure Resource Manager-service. De Azure CLI moet worden overgeschakeld naar de modus Resource Manager.

        azure config mode arm

4. Uitvoeren om de bestaande Azure DNS-zone **contoso.com** in resource groep **myresourcegroup** exporteren naar het bestand **contoso.com.txt** (in de huidige map), `azure network dns zone export`. Met deze opdracht belt de Azure DNS-service inventariseren recordsets in de zone en de resultaten exporteren naar een bestand compatibel is met BIND zone.

        azure network dns zone export myresourcegroup contoso.com contoso.com.txt

