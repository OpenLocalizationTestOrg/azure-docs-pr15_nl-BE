<properties
    pageTitle="Inleiding tot opslag | Microsoft Azure"
    description="Een overzicht van opslag in Azure, van Microsoft on line gegevens worden opgeslagen in de cloud. Informatie over het gebruik van de beste beschikbare cloud storage oplossing in uw toepassingen."
    services="storage"
    documentationCenter=""
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/25/2016"
    ms.author="tamram"/>

# <a name="introduction-to-microsoft-azure-storage"></a>Inleiding tot Microsoft Azure opslag

## <a name="overview"></a>Overzicht

Azure opslag is de cloud storage oplossing voor moderne toepassingen die afhankelijk zijn van duurzaamheid, beschikbaarheid en schaalbaarheid om te voldoen aan de behoeften van hun klanten. Door het lezen van dit artikel, ontwikkelaars, IT-professionals en zakelijke beslissing makers meer informatie over:

- Wat opslag Azure is en hoe u gebruik kunt maken in de cloud, mobiel, server- en desktop toepassingen kunt nemen
- Welke typen gegevens u kunt opslaan met de opslag van Azure services: blob-gegevens (object), NoSQL tabelgegevens, berichten en bestandsshares.
- Hoe toegang tot uw gegevens in Azure opslag wordt beheerd
- Hoe uw gegevens opslag Azure geschiedt duurzame via redundantie en replicatie
- Waar gaat u vervolgens naar uw eerste Azure opslag-toepassing maken

Slag met Azure opslag snel Zie [aan de slag met Azure opslag in vijf minuten](storage-getting-started-guide.md).

Zie [Volgende stappen](#next-steps) hieronder voor meer informatie over hulpprogramma's, bibliotheken en andere bronnen voor het werken met Azure-opslag.

## <a name="what-is-azure-storage"></a>Wat is Azure opslag?

Cloud computing maakt nieuwe scenario's voor toepassingen waarvoor schaalbare, duurzame en hoge beschikbaarheid opslag voor hun gegevens – dat is precies waarom Microsoft Azure opslag ontwikkeld. Maken voor ontwikkelaars op grootschalige toepassingen ter ondersteuning van nieuwe scenario's mogelijk Azure opslag beschikt bovendien over de Stichting opslag voor Azure virtuele Machines, een nieuwe testament aan de robuustheid.

Azure opslag is sterk schaalbare, zodat u kunt opslaan en proces honderden terabytes aan gegevens ter ondersteuning van scenario's voor de grote gegevens voor het wetenschappelijke, financiële analyse en mediatoepassingen vereist. Of u kunt de kleine hoeveelheden gegevens die nodig zijn voor een klein bedrijf website opslaan. Waar uw behoeften liggen, betaalt u alleen de gegevens die u wilt opslaan. Azure opslag momenteel tientallen trillions unieke klant-objecten worden opgeslagen en verwerkt aanvragen per seconde miljoenen gemiddeld.

Azure opslag is elastisch, zodat toepassingen voor een groot publiek globale ontwerpen en die toepassingen schalen zo nodig - zowel wat betreft de hoeveelheid opgeslagen gegevens en het aantal aanvragen die zijn ingediend tegen. U betaalt alleen voor wat u gebruikt en alleen wanneer u deze gebruikt.

Azure opslag gebruikt een automatisch partitioneren dat systeem automatisch laden-saldi uw gegevens op basis van het verkeer. Dit betekent dat als de vereisten van uw toepassing vergroten, Azure opslag automatisch toegewezen de juiste middelen om daaraan te voldoen.

Azure opslag is toegankelijk vanaf overal ter wereld van elk type toepassing, of het wordt uitgevoerd in de cloud, op het bureaublad, op een server op locatie of op een mobiel of Tablet PC-apparaat. U kunt opslag Azure in mobiele scenario's waarbij de toepassing bevat een subset van de gegevens op het apparaat en worden deze gesynchroniseerd met een volledige set gegevens die zijn opgeslagen in de cloud.

Azure opslag ondersteunt met behulp van een groot aantal verschillende besturingssystemen (waaronder Windows en Linux) en een aantal verschillende programmeertalen (inclusief .NET, Java, Node.js, Python, Ruby, PHP en C++ en mobiele programmeertalen) voor de ontwikkeling van geschikte clients. Azure opslag stelt tevens resources via eenvoudige REST API's die beschikbaar zijn voor elke client kunnen verzenden en ontvangen van gegevens via HTTP/HTTPS-gegevens.

Azure Premium opslag biedt schijfondersteuning voor i/o-intensieve belasting Azure virtuele Machines met hoge prestaties, lage latentie. Met Azure Premium opslag, kunt u meerdere schijven van permanente gegevens koppelen aan een virtuele machine en configureren om te voldoen aan de prestatie-eisen. Elke gegevensschijf wordt ondersteund door een SSD-schijf in Azure premie opslag voor maximale i/o-prestaties. Zie [Premium-opslag: krachtige opslag voor Azure VM werklasten](storage-premium-storage.md) voor meer informatie.

## <a name="introducing-the-azure-storage-services"></a>Introductie van de van Azure opslagservices

Azure opslag biedt de volgende vier diensten: Blob-opslag, opslag tabel Queue storage en opslag van bestanden.

- BLOB-opslag ongestructureerde objectgegevens worden opgeslagen. Een blob kan tekst of binaire gegevens, zoals een document, bestand of installatieprogramma voor elk type zijn. BLOB-opslag is ook opslag van objecten genoemd.
- Tabelopslag gestructureerde datasets worden opgeslagen. Tabelopslag is een NoSQL sleutel kenmerk gegevensopslag, waardoor snelle ontwikkeling en snelle toegang tot grote hoeveelheden gegevens.
- Queue Storage biedt betrouwbare berichten voor verwerking van de workflow voor communicatie tussen onderdelen van cloud-services.
- Opslag van bestanden biedt gedeelde opslag van oudere toepassingen met behulp van de standaard SMB-protocol. Azure virtuele machines en cloud services gegevens in een bestand kunnen delen via via aandelen gekoppelde onderdelen en toepassingen in gebouwen toegang tot gegevens in een bestand op een share via de File service REST API.

Een account Azure opslag is een veilige account waarmee u toegang krijgt tot services in Azure opslag. Uw account opslag biedt een unieke naamruimte voor uw opslagbronnen. De onderstaande afbeelding ziet u de relaties tussen de Azure opslagbronnen op een rekening van de opslag:

![Azure opslagbronnen](./media/storage-introduction/storage-concepts.png)

[AZURE.INCLUDE [storage-account-types-include](../../includes/storage-account-types-include.md)]

[AZURE.INCLUDE [storage-versions-include](../../includes/storage-versions-include.md)]

## <a name="blob-storage"></a>BLOB-opslag

Blob-opslag biedt een kosteneffectieve en schaalbare oplossing voor gebruikers met grote hoeveelheden ongestructureerde objectgegevens op te slaan in de cloud. Blob-opslag kunt u inhoud opslaan, zoals:

- Documenten
- Sociale gegevens zoals foto's, video's, muziek en blogs
- Back-ups van bestanden, computers, databases en apparaten
- Afbeeldingen en tekst voor webtoepassingen
- Configuratiegegevens voor cloud-toepassingen
- Grote gegevens, zoals Logboeken en andere grote gegevenssets

Elke blob is ingedeeld in een container. Containers bieden ook een handige manier om beveiligingsbeleid toewijzen aan groepen van objecten. Een opslag-account kan een willekeurig aantal containers bevatten en een container kan een willekeurig aantal BLOB's tot aan de limiet van 500 TB capaciteit van de opslag account bevatten.  

BLOB-opslag biedt drie soorten BLOB's, BLOB's blokkeren, append BLOB's en pagina BLOB's (schijven).

- Blok BLOB's zijn geoptimaliseerd voor streaming en cloud-objecten op te slaan en zijn een goede keuze voor het opslaan van documenten, media-bestanden, back-ups enz.
- Append BLOB's zijn vergelijkbaar met de BLOB's blokkeren, maar zijn geoptimaliseerd voor bewerkingen toevoegen. Een toevoegquery blob kan alleen worden bijgewerkt met een nieuw blok toe te voegen aan het einde. Append BLOB's zijn een goede keuze voor scenario's zoals registratie, waarin nieuwe gegevens moet worden geschreven alleen aan het einde van de blob.
- Pagina BLOB's zijn geoptimaliseerd voor die schijven IaaS en ondersteunende willekeurige schrijft, en mogelijk tot 1 TB in grootte. Een netwerk voor Azure VM IaaS schijf een VHD opgeslagen als een blob pagina is gekoppeld.

Voor zeer grote gegevenssets waarin netwerk beperkingen kunnen uploaden of downloaden van gegevens naar een Blob-opslag via het netwerk onrealistische, kunt u een vaste schijf met Microsoft om te importeren of exporteren van gegevens rechtstreeks vanuit het midden van de gegevens verzenden. Zie [werken met de Service Microsoft Azure importeren/exporteren om gegevens als Blob-opslag te brengen](storage-import-export-service.md).

## <a name="table-storage"></a>Tabelopslag

Moderne toepassingen vereisen vaak opgeslagen gegevens met een grotere schaalbaarheid en flexibiliteit dan vorige generaties van de software die nodig is. Tabelopslag biedt een hoge beschikbaarheid, sterk schaalbare opslag, zodat uw toepassing schaal automatisch aanpassen kunt om te voldoen aan de eisen van de gebruiker. Tabelopslag van Microsoft NoSQL sleutel/kenmerk Archief is – heeft een schemaless ontwerp verschilt van de traditionele relationele databases. Met een schemaless gegevensarchief is het eenvoudig om uw gegevens aan te passen als de behoeften van uw toepassing evolve. Tabelopslag is eenvoudig te gebruiken, zodat ontwikkelaars toepassingen snel kunnen maken. Toegang tot gegevens is snel en rendabel voor allerlei soorten toepassingen.  Tabelopslag is meestal in de kosten aanzienlijk lager is dan traditionele SQL voor dergelijke volumes van gegevens.

Tabelopslag is een sleutel kenmerk Archief, wat betekent dat elke waarde in een tabel is opgeslagen met een getypte naam. Naam van de eigenschap kan worden gebruikt voor het filteren en selectiecriteria op te geven. Een verzameling eigenschappen en bijbehorende waarden bestaan uit een entiteit. Aangezien opslag tabel schemaless, twee entiteiten in dezelfde tabel kunnen bevatten verschillende verzamelingen van eigenschappen en deze eigenschappen zijn verschillende soorten.

U kunt opslag tabel opslaan flexibele datasets, zoals gegevens van de gebruiker voor webtoepassingen, adresboeken, gegevens van een apparaat en een ander type metagegevens die de service nodig heeft.  U kunt een willekeurig aantal entiteiten opslaan in een tabel en een opslag-account kan een willekeurig aantal tabellen tot de capaciteit van de opslag account bevatten.

Net als BLOB's en wachtrijen, ontwikkelaars kunnen beheren en openen tabel opslag met behulp van standaard overige protocollen, maar tabelopslag biedt ook ondersteuning voor een subset van de OData-protocol vereenvoudigen geavanceerde querymogelijkheden en JSON- en AtomPub (XML gebaseerd) inschakelen indelingen.

NoSQL databases zoals tabelopslag bieden voor hedendaagse Internet-toepassingen, een populair alternatief voor de traditionele relationele databases.

## <a name="queue-storage"></a>Queue Storage

Bij het ontwerpen van toepassingen voor schaal, zijn componenten van toepassing vaak ontkoppeld, zodat ze kunnen onafhankelijk van elkaar worden geschaald. Wachtrij opslag biedt een betrouwbare oplossing voor expresberichten voor asynchrone communicatie tussen toepassingscomponenten van, of ze worden uitgevoerd in de cloud, op het bureaublad, op een server op locatie of op een mobiel apparaat. Wachtrij opslag biedt ook ondersteuning voor asynchrone taken beheren en proces werkstromen te bouwen.

Een opslag-account kan een willekeurig aantal wachtrijen bevatten. Een wachtrij kan een willekeurig aantal berichten tot aan de capaciteit van de opslag account bevatten. Afzonderlijke berichten kunnen tot 64 KB groot zijn.

## <a name="file-storage"></a>Opslag van bestanden

Azure bestandsopslag biedt cloud-gebaseerde SMB-bestandsshares, zodat u kunt oudere toepassingen die afhankelijk van bestandsshares naar Azure snel en zonder dure herschreven zijn migreren. Met Azure, opslag kunnen toepassingen in Azure virtual machines of cloud-services zoals een bureaubladtoepassing een typische SMB-share koppelt gedeelde bestanden in de cloud, koppelen. Een aantal toepassingsonderdelen kan vervolgens koppelen en gelijktijdig toegang krijgen tot de bestandsshare voor de opslag.

Aangezien een gedeeld bestand opslag een standaard SMB-bestandsshare is, hebben toepassingen die worden uitgevoerd in Azure toegang tot gegevens in de share via bestand systeem I/O APIs. Ontwikkelaars kunnen daarom gebruikmaken van hun bestaande code en vaardigheden voor het migreren van bestaande toepassingen. IT-professionals kunnen PowerShell-cmdlets gebruiken maken, koppelen en opslag van bestandsshares beheren als onderdeel van het beheer van toepassingen op Azure.

Net als de andere Azure opslagservices beschrijft bestandsopslag REST-API voor toegang tot gegevens in een share. Lokale toepassingen kunnen de REST API voor toegang tot gegevens in een bestandsshare voor bestandsopslag aanroepen. Op deze manier een onderneming kunt migreren van sommige oudere toepassingen naar Azure en doorgaan met anderen binnen hun eigen organisatie. Montage van een gedeeld bestand is alleen mogelijk voor toepassingen die worden uitgevoerd in Azure; een toepassing op ruimten kan alleen toegang tot de bestandsshare via de REST API.

Gedistribueerde toepassingen ook kunt opslaan van bestanden opslaan en delen van toepassingsgegevens voor nuttige en de ontwikkeling en testen hulpmiddelen. Bijvoorbeeld een toepassing configuratiebestanden kan opslaan en diagnostische gegevens zoals Logboeken, statistieken en crash dumpen in een bestand opslag delen, zodat ze beschikbaar zijn voor meerdere virtuele machines of rollen. Ontwikkelaars en beheerders hulpprogramma's die ze nodig hebben om te bouwen of beheren van een toepassing in een bestandsshare opslagruimte die beschikbaar is voor alle onderdelen kunnen worden opgeslagen in plaats van ze te installeren op elke virtuele machine of het exemplaar van de rol.

## <a name="access-to-blob-table-queue-and-file-resources"></a>Toegang tot de Blob, tabel, wachtrij en bestandsbronnen

Standaard alleen de eigenaar van de opslag-account toegang tot bronnen in de opslag. Elke aanvraag ten opzichte van de resources in uw account moet worden geverifieerd voor de beveiliging van uw gegevens. Wordt gebruikgemaakt van een gedeelde sleutel model. BLOB's kunnen ook worden geconfigureerd voor ondersteuning van anonieme verificatie.

Uw account voor opslag is toegewezen twee particuliere toegangstoetsen gemaakt die worden gebruikt voor verificatie. Met twee sleutels zorgt ervoor dat uw toepassing beschikbaar blijft wanneer u regelmatig de sleutels algemene Sleutelbeheer veiligheidsoverwegingen genereren.

Als u gebruikers gecontroleerde toegang kunnen krijgen tot uw opslagbronnen wilt kunt u een gedeelde access-handtekening maken. Een gedeelde access-handtekening (SAS) is een token dat kan worden toegevoegd aan een URL waarmee gedelegeerde toegang tot een resource opslag. Iedereen die beschikt over het token toegang tot de bron verwijst naar de machtigingen die zijn opgegeven, voor de periode dat deze geldig is. Vanaf versie 2015-04-05, Azure opslag ondersteunt twee soorten gedeelde toegang handtekeningen: SAS-service en de account SA's.

De SAS-service delegeert toegang tot een bron in slechts één van de storage-services: de service Blob, wachtrij, tabel of bestand.

Een account SAS delegeert toegang tot bronnen in een of meer van de storage-services. U kunt de toegang tot de service level-bewerkingen die niet beschikbaar met een SAS-service zijn overdragen. U kunt ook toegang tot het lezen, schrijven en verwijderen van containers blob, tabellen, wachtrijen en bestandsshares die niet zijn toegestaan met een SAS-service overdragen.

Ten slotte kunt u opgeven dat een container en de BLOB's of een specifieke blob voor toegang van het publiek beschikbaar zijn. Wanneer u aangeeft dat een container of blob openbaar is, iedereen kan worden gelezen op anoniem; Er is geen verificatie vereist.  Openbare containers en BLOB's zijn nuttig voor de weergave van bronnen, zoals media en documenten die worden gehost op websites.  U kunt als netwerkvertraging verlagen voor een globale doelgroep, die wordt gebruikt door websites met de CDN Azure blob-gegevens cache.

[Met behulp van gedeelde toegang handtekeningen (SAS)](storage-dotnet-shared-access-signature-part-1.md) Zie voor meer informatie over handtekeningen voor gedeeld gebruik. Zie [verificatie voor de opslag Azure Services](https://msdn.microsoft.com/library/azure/dd179428.aspx) en [anonieme leestoegang tot containers en BLOB's beheren](storage-manage-access-to-resources.md) voor meer informatie over veilige toegang tot uw account opslag.

## <a name="replication-for-durability-and-high-availability"></a>Replicatie voor duurzaamheid en hoge beschikbaarheid

De gegevens in uw account Microsoft Azure opslag wordt altijd duurzaamheid en hoge beschikbaarheid te garanderen gerepliceerd. Replicatie kopieert uw gegevens binnen dezelfde Datacenter of tot een tweede Datacenter, afhankelijk van welke replicatieoptie u kiest. Replicatie beschermt uw gegevens en uw toepassing gebruikstijd bij hardwarestoringen tijdelijk behoudt. Als uw gegevens worden gerepliceerd naar een tweede Datacenter, beveiligt die ook u uw gegevens tegen een onherstelbare fout in de primaire locatie.

Replicatie zorgt ervoor dat uw account opslag voldoet aan de [Serviceniveau overeenkomst (SLA) voor opslag](https://azure.microsoft.com/support/legal/sla/storage/) , zelfs tegen storingen. Zie dat de SLA voor informatie over Azure opslag voor duurzaamheid en beschikbaarheid garandeert. 

Wanneer u een opslag-account maakt, kunt u een van de volgende replicatieopties:  

- **Lokaal redundante opslag (LRS).** Lokaal redundante opslag houdt drie exemplaren van uw gegevens. LRS wordt drie keer binnen één datacenter in één regio gerepliceerd. LRS beveiligt u uw gegevens van normale hardwarefouten, maar niet van het uitvallen van één datacenter.  

    LRS wordt aangeboden met een korting. Voor een maximale duurzaamheid, is het raadzaam gebruik van geo-redundante opslag, zoals hieronder beschreven.


- **Zone redundante opslag (ZRS).** Zone redundante opslag houdt drie exemplaren van uw gegevens. ZRS gerepliceerd driemaal in twee of drie faciliteiten, binnen een bepaalde regio of tussen de twee regio's, die met een hogere duurzaamheid dan LRS. ZRS zorgt ervoor dat de gegevens duurzaam binnen een bepaalde regio.  

    ZRS biedt een hogere mate van duurzaamheid dan LRS; we raden echter voor maximale duurzaamheid aan gebruik van geo-redundante opslag, zoals hieronder beschreven.  

    > [AZURE.NOTE] ZRS is momenteel alleen beschikbaar voor BLOB's blokkeren en wordt alleen ondersteund voor versies 2014-02-14 en hoger.
    >
    > Zodra u hebt gemaakt van uw account voor de opslag en ZRS hebt geselecteerd, kunt u deze met behulp van een ander type replicatie niet converteren of vice versa.

- **Geo-redundante opslag (GRS)**. GRS houdt zes exemplaren van uw gegevens. GRS, uw gegevens drie keer wordt gerepliceerd binnen het primaire gebied en tevens drie keer worden gerepliceerd in een secundaire gebied honderden van mijl van de primaire regio, biedt het hoogste niveau van duurzaamheid. Azure opslag wordt bij een storing op de primaire regio failover naar de secundaire regio. GRS zorgt ervoor dat de gegevens duurzaam in twee afzonderlijke regio's.

    Zie voor meer informatie over primaire en secundaire koppelingen per regio [Azure regio's](https://azure.microsoft.com/regions/).

- **Geo-redundante opslag lezen-toegang (RA-GRS)**. Geo-redundante opslag leestoegang tot de gegevens worden gerepliceerd naar een secundaire locatie en biedt ook leestoegang hebben tot de gegevens in de secundaire locatie. Leestoegang tot geo-redundante opslag kunt u toegang tot uw gegevens op de primaire of de secundaire locatie, in het geval dat er niet één locatie beschikbaar is. Leestoegang tot geo-redundante opslag is de standaardoptie voor de standaard opslag-account wanneer u deze maakt. 

    > [AZURE.IMPORTANT] U kunt wijzigen hoe uw gegevens worden gerepliceerd nadat uw account opslag is gemaakt, tenzij u ZRS hebt opgegeven toen u de account hebt gemaakt. Bedenk wel dat u een extra eenmalige gegevensoverdracht als u vanuit een LRS op GRS of RA GRS overschakelt kosten kan oplopen.

Zie [replicatie Azure opslag](storage-redundancy.md) voor meer informatie over opslagopties voor replicatie.

Voor prijsinformatie voor opslag account replicatie, Zie [Azure opslag prijzen](https://azure.microsoft.com/pricing/details/storage/). Zie [Azure regio's](https://azure.microsoft.com/regions/#services) voor meer informatie over welke services beschikbaar in elke regio zijn.

Zie voor details over duurzaamheid met Azure opslag architecturale [SOSP papier - Azure opslag: een zeer beschikbare Cloud Storage-Service met een sterke samenhang](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx).


## <a name="transferring-data-to-and-from-azure-storage"></a>Gegevens overbrengen van en naar Azure opslag

U kunt het opdrachtregelprogramma AzCopy blob-bestand en gegevens in een tabel kopiëren binnen uw account opslag of opslag rekeningen. Zie de [overdracht van gegevens met het hulpprogramma AzCopy voor Command-Line](storage-use-azcopy.md) voor meer informatie.

AzCopy is gebaseerd op de [Azure Data verkeer bibliotheek](https://www.nuget.org/packages/Microsoft.Azure.Storage.DataMovement/)is momenteel beschikbaar in het afdrukvoorbeeld.

De service Azure importeren/exporteren kunt u in blob-gegevens importeren of exporteren van blob-gegevens van uw account opslag via een harde schijf schijf opgestuurd naar het datacenter Azure. Zie voor meer informatie over de Import/Export-service [gebruik van de Service Microsoft Azure importeren/exporteren op gegevens overbrengen naar een Blob-opslag](storage-import-export-service.md).

## <a name="pricing"></a>Prijzen

[AZURE.INCLUDE [storage-account-billing-include](../../includes/storage-account-billing-include.md)]

## <a name="storage-apis-libraries-and-tools"></a>Storage API's, bibliotheken en Tools

Azure opslagbronnen is toegankelijk via elke taal die HTTP/HTTPS-verzoeken kan indienen. Daarnaast biedt Azure opslag programming bibliotheken voor diverse populaire talen. Deze bibliotheken vereenvoudigen veel aspecten van het werken met Azure opslag door details zoals synchrone en asynchrone aanroep te verwerken, batchen van bewerkingen, uitzonderingen beheren, automatische pogingen, systeemgedrag enzovoort. Bibliotheken zijn beschikbaar voor de volgende talen en platforms met anderen in de pijpleiding:

### <a name="azure-storage-data-services"></a>Azure opslag van gegevens

- [Opslagservices REST API](http://msdn.microsoft.com/library/azure/dd179355.aspx)
- [Client-bibliotheek van opslag voor .NET, Windows Phone en Windows Runtime](https://www.nuget.org/packages/WindowsAzure.Storage/)
- [Opslag Client Library for C++](https://github.com/Azure/azure-storage-cpp)
- [Client-bibliotheek van opslag voor Java/Android](/develop/java/)
- [Client-bibliotheek van opslag voor Node.js](http://dl.windowsazure.com/nodestoragedocs/index.html)
- [Opslag Client Library voor PHP](/develop/php/)
- [Client-bibliotheek van opslag voor Ruby](/develop/ruby/)
- [Client-bibliotheek van opslag voor Python](/develop/python/)
- [Opslag-Cmdlets voor PowerShell 1.0](https://msdn.microsoft.com/library/azure/mt269418.aspx)

### <a name="azure-storage-management-services"></a>Azure Storage Management Services

- [Opslag Resource Provider REST API: naslag](https://msdn.microsoft.com/library/azure/mt163683.aspx)
- [Storage Provider Client Bronbibliotheek voor .NET](https://msdn.microsoft.com/library/azure/mt131037.aspx)
- [Opslag Resource Provider Cmdlets voor PowerShell 1.0](https://msdn.microsoft.com/library/azure/mt607151.aspx)
- [Opslag Service Management REST API (klassiek)](https://msdn.microsoft.com/library/azure/ee460790.aspx)

### <a name="azure-storage-data-movement-services"></a>Azure opslag verplaatsing van gegevens

- [Opslag Import/Export Service REST API](https://msdn.microsoft.com/library/azure/dn529096.aspx)
- [Opslag gegevensverplaatsing Client Library for .NET](https://www.nuget.org/packages/Microsoft.Azure.Storage.DataMovement/)

### <a name="tools-and-utilities"></a>Hulpmiddelen en hulpprogramma 's

- [Azure Opslagverkenner](http://go.microsoft.com/fwlink/?LinkID=822673&clcid=0x409)
- [Azure opslag clienthulpprogramma 's](storage-explorers.md)
- [Azure-SDK's en hulpprogramma 's](https://azure.microsoft.com/tools/)
- [Azure opslag Emulator](http://www.microsoft.com/download/details.aspx?id=43709)
- [Azure PowerShell](../powershell-install-configure.md)
- [AzCopy op de opdrachtregel](http://aka.ms/downloadazcopy)

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over opslag in Azure, deze bronnen te verkennen:

### <a name="documentation"></a>Documentatie

- [Azure opslag documentatie](https://azure.microsoft.com/documentation/services/storage/)

### <a name="for-administrators"></a>Voor beheerders

- [Met Azure PowerShell en Azure opslag](storage-powershell-guide-full.md)
- [Azure CLI gebruiken met Azure opslag](storage-azure-cli.md)

### <a name="for-net-developers"></a>Voor .NET ontwikkelaars

- [Aan de slag met Azure Blob-opslag met behulp van .NET](storage-dotnet-how-to-use-blobs.md)
- [Aan de slag met Azure tabel opslag met behulp van .NET](storage-dotnet-how-to-use-tables.md)
- [Aan de slag met Azure wachtrij opslag met behulp van .NET](storage-dotnet-how-to-use-queues.md)
- [Aan de slag met Azure opslaan van bestanden in Windows](storage-dotnet-how-to-use-files.md)

### <a name="for-javaandroid-developers"></a>Ontwikkelaars Java/Android

- [Het gebruik van Java Blob-opslag](storage-java-how-to-use-blob-storage.md)
- [Het gebruik van tabelopslag van Java](storage-java-how-to-use-table-storage.md)
- [Queue storage van Java gebruiken](storage-java-how-to-use-queue-storage.md)
- [Het gebruik van opslag van Java](storage-java-how-to-use-file-storage.md)

### <a name="for-nodejs-developers"></a>Voor ontwikkelaars van Node.js

- [Het gebruik van Node.js Blob-opslag](storage-nodejs-how-to-use-blob-storage.md)
- [Het gebruik van opslag van Node.js tabel](storage-nodejs-how-to-use-table-storage.md)
- [Het gebruik van opslag van Node.js wachtrij](storage-nodejs-how-to-use-queues.md)

### <a name="for-php-developers"></a>Voor PHP-ontwikkelaars

- [Het gebruik van PHP Blob-opslag](storage-php-how-to-use-blobs.md)
- [Het gebruik van opslag van PHP tabel](storage-php-how-to-use-table-storage.md)
- [Het gebruik van opslag van PHP wachtrij](storage-php-how-to-use-queues.md)

### <a name="for-ruby-developers"></a>Ruby-ontwikkelaars

- [Het gebruik van Ruby Blob-opslag](storage-ruby-how-to-use-blob-storage.md)
- [Het gebruik van opslag van Ruby tabel](storage-ruby-how-to-use-table-storage.md)
- [Het gebruik van opslag van Ruby wachtrij](storage-ruby-how-to-use-queue-storage.md)

### <a name="for-python-developers"></a>Python-ontwikkelaars

- [Het gebruik van Python Blob-opslag](storage-python-how-to-use-blob-storage.md)
- [Het gebruik van opslag van Python tabel](storage-python-how-to-use-table-storage.md)
- [Het gebruik van opslag van Python wachtrij](storage-python-how-to-use-queue-storage.md)
- [Het gebruik van opslag van Python](storage-python-how-to-use-file-storage.md)
