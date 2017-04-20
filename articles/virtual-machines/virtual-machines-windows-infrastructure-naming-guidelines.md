<properties
    pageTitle="Naamgevingsregels infrastructuur | Microsoft Azure"
    description="Meer informatie over de belangrijke ontwerp en de implementatie van de richtlijnen voor de naamgeving in Azure infrastructuurservices."
    documentationCenter=""
    services="virtual-machines-windows"
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/08/2016"
    ms.author="iainfou"/>

# <a name="infrastructure-naming-guidelines"></a>Richtlijnen voor naamgeving van infrastructuur

[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-intro](../../includes/virtual-machines-windows-infrastructure-guidelines-intro.md)] 

Dit artikel is gericht op het inzicht van de benadering van de naamgevingsconventies voor alle verschillende Azure resources maken een logische en gemakkelijk identificeerbaar set resources in uw omgeving.

## <a name="implementation-guidelines-for-naming-conventions"></a>Implementatie van richtlijnen voor naamgeving

Besluiten:

- Wat zijn de naamgevingsconventies voor Azure resources?

Taken:

- De voor-en achtervoegsel in uw resources gebruiken om de consistentie te definiëren.
- Gezien de eis voor ze de globaal unieke namen van opslag definiëren.
- Documenteer de naamgevingsconventie gebruiken en verspreiden onder alle betrokkenen om ervoor te zorgen dat alle implementaties.

## <a name="naming-conventions"></a>Naamgevingsconventies

U moet beschikken over een goede naamgevingsconventie voordat u iets in Azure. Een naamgevingsconventie zorgt ervoor dat alle bronnen een voorspelbare naam hebben, waardoor de administratieve last die is gekoppeld aan het beheer van deze middelen verlagen.

U kunt als volgt een specifieke reeks gedefinieerd voor uw gehele organisatie of voor een bepaald Azure-abonnement of een account voor naamgeving. Gemakkelijk personen binnen organisaties impliciete regels vast wanneer u werkt met resources Azure, wanneer een team nodig heeft om te werken aan een project op Azure, maar dat model is niet goed op schaal.

Een set vooraf naamgevingsconventies afspreken. Er zijn enkele overwegingen met betrekking tot naamgeving die meerdere die ingesteld van regels.

## <a name="affixes"></a>Voor-en achtervoegsel

Als u kijkt om te definiëren voor naamgeving, komt een besluit mee of het voorvoegsel op:

- Het begin van de naam (voorvoegsel)
- Het einde van de naam (achtervoegsel)

Bijvoorbeeld hier zijn twee verschillende namen voor een bronnengroep met de `rg` brengt:

- Rg-WebApp (prefix)
- WebApp-Rg (achtervoegsel)

Voor-en achtervoegsel kunnen verwijzen naar verschillende aspecten die bepaalde resources te beschrijven. De volgende tabel ziet u enkele voorbeelden die meestal wordt gebruikt.

| Aspect                               | Voorbeelden                                                               | Notities                                                                                                      |
|:-------------------------------------|:-----------------------------------------------------------------------|:-----------------------------------------------------------------------------------------------------------|
| Omgeving                          | PO-dev, stg,                                                         | Afhankelijk van het doel en de naam van elke omgeving.                                                     |
| Locatie                             | usw (West VS) gebruiken (Oost-VS-2)                                         | Afhankelijk van het gebied van het datacenter of de regio van de organisatie.                               |
| Azure component, service of product | Rg voor resourcegroep, VNet voor een virtueel netwerk                        | Afhankelijk van het product die de bron wordt ondersteund.                                          |
| Rol                                 | SQL, ora, sp, iis                                                      | Afhankelijk van de functie van de virtuele machine.                                                              |
| Exemplaar                             | 01, 02, 03, enz.                                                       | Voor bronnen die meer dan één exemplaar. Bijvoorbeeld, evenwichtige taakverdeling webservers in een cloud-service. |


Bij de vaststelling van de regels voor naamgeving, zorg dat ze duidelijk welke voor-en achtervoegsel vermelden voor elk type van de bron en op welke positie (voorvoegsel en achtervoegsel).

## <a name="dates"></a>Datums

Vaak is het belangrijk om te bepalen van de datum van het maken van de naam van een bron. Het is raadzaam de notatie JJJJMMDD. Deze indeling zorgt ervoor dat niet alleen de volledige datum wordt vastgelegd, maar ook twee resources waarvan de namen op de datum verschillen alfabetisch en chronologisch gesorteerd op hetzelfde moment.

## <a name="naming-resources"></a>Namen van resources

Definiëren van elk type van de resource in de naamgevingsconventie, moet die regels die bepalen hoe namen toewijzen aan elke resource die wordt gemaakt. Deze regels gelden voor alle soorten bronnen, bijvoorbeeld:

- Abonnementen
- Rekeningen
- Opslag-accounts
- Virtuele netwerken
- Subnetten
- Beschikbaarheid van sets
- Resourcegroepen
- Virtuele machines
- Eindpunten
- Netwerk-beveiligingsgroepen
- Rollen

Om ervoor te zorgen dat de naam biedt voldoende informatie om te bepalen tot welke bron verwijst, moet u beschrijvende namen.

## <a name="computer-names"></a>Computernamen

Bij het maken van een virtuele machine (VM), moet Microsoft Azure een VM naam van maximaal 15 tekens dat wordt gebruikt voor de naam van de resource. Azure gebruikt dezelfde naam voor het besturingssysteem is geïnstalleerd in de VM. Echter, deze namen kunnen niet altijd hetzelfde zijn.

In het geval een VM is gemaakt van de VHD image-bestand dat al een besturingssysteem bevat, worden de naam VM in Azure kan afwijken van de computernaam van de VM-besturingssysteem. Deze situatie kan een graad van moeilijkheid toevoegen aan beheer van VM daarom niet aangeraden. Dezelfde naam als de naam van de computer die u aan het besturingssysteem van deze VM toewijst voor Azure VM resource toewijzen.

Het is raadzaam de naam Azure VM is hetzelfde als de naam van het onderliggende besturingssysteem van computer.

## <a name="storage-account-names"></a>Namen van opslag

Opslag zijn bijzondere regels inzake hun namen. U kunt alleen kleine letters en cijfers gebruiken. Zie [een opslag-account maken](../storage/storage-create-storage-account.md#create-a-storage-account) voor meer informatie. Bovendien moet de naam van de opslag, samen met core.windows.net, een globaal geldig, uniek DNS-naam. Bijvoorbeeld als de account voor de opslag wordt mystorageaccount genoemd, moeten resulterende DNS-namen uniek zijn:

- mystorageaccount.BLOB.Core.Windows.NET
- mystorageaccount.Table.Core.Windows.NET
- mystorageaccount.Queue.Core.Windows.NET


## <a name="next-steps"></a>Volgende stappen
[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-next-steps](../../includes/virtual-machines-windows-infrastructure-guidelines-next-steps.md)] 