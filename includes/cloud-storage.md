#<a name="data-management-and-business-analytics"></a>Data Management en Business Analytics

Beheren en analyseren van gegevens in de cloud is net zo belangrijk als het ergens anders is. Als u dit doet, biedt Azure een assortiment van technologieën voor het werken met relationele en niet-relationele gegevens. Dit artikel bevat de opties. 

##<a name="table-of-contents"></a>Inhoudsopgave      

- [BLOB-opslag](#blob)
- [Een DBMS wordt uitgevoerd op een virtuele Machine](#dbinvm)
- [SQL-Database](#sqldb)
    - [SQL-gegevens synchroniseren](#datasync)
    - [Rapporteren van SQL-gegevens met behulp van virtuele Machines](#datarpt)
- [Tabelopslag](#tblstor)
- [Hadoop](#hadoop)

## <a name="blob"></a>BLOB-opslag

Het woord "blob" is de afkorting voor "Binary Large OBject' en beschrijft precies wat een blob is: een verzameling binaire gegevens. BLOB's zijn nog een nuttig, hoewel ze eenvoudig zijn. [Figuur 1](#Fig1) illustreert de basisbeginselen van Azure Blob-opslag.

<a name="Fig1"></a>![Diagram van BLOB 's][blobs]
 
**Figuur 1: Azure Blob-opslag binaire gegevens opgeslagen - BLOB's - in containers.**

BLOB's gebruikt, moet u eerst een Azure *opslag account*maken. Als onderdeel van dit, moet u de Azure datacenter waarin de objecten die u maakt met behulp van deze account opgeven. Waar zij woont, behoort elk blob u maakt een container in uw account voor de opslag. Voor toegang tot een blob, levert een aanvraag een URL met het formulier:

http://&lt;*StorageAccount*&gt;.blob.core.windows.net/&lt;*Container*&gt;/&lt;*BlobName*&gt;

&lt;*StorageAccount* &gt; is een unieke id toegewezen wanneer een nieuwe opslag-account is gemaakt, terwijl &lt; *Container* &gt; en &lt; *BlobName* &gt; zijn de namen van een specifieke container en een blob in die container. 

Azure biedt twee soorten BLOB's. De opties zijn:

- *Blok* blobs die elk maximaal 200 GB aan gegevens kan bevatten. Zoals de naam al aangeeft, wordt een blok blob onderverdeeld in een aantal blokken. Als een fout optreedt tijdens het overzetten van een blob blok, kunt retransmission hervatten met de meest recente blokkeren in plaats van de gehele blob opnieuw te verzenden. Blok BLOB's zijn een vrij algemene aanpak voor opslag en ze vandaag de meest gebruikte blob-type zijn.

- *Pagina* BLOB's, die op één terabyte groot kunnen zijn. Pagina BLOB's zijn ontworpen voor RAM en dus elke is onderverdeeld in een aantal pagina's. Een toepassing is voor lezen en schrijven op afzonderlijke pagina's in willekeurige volgorde in de blob gratis. In Azure virtuele Machines bijvoorbeeld gebruik VMs u maakt pagina BLOB's als permanente opslag voor zowel OS schijven en gegevensschijven.

Of u BLOB's blokkeren of BLOB van pagina's kiest, toegankelijk toepassingen blob-gegevens op verschillende manieren. De opties omvatten het volgende:

- Rechtstreeks via een RESTful (dat wil zeggen, op basis van HTTP) toegang tot protocol. Azure toepassingen en externe toepassingen, waaronder apps op gebouwen, kunnen deze optie gebruiken.
- Gebruikt de clientbibliotheek Azure opslag, waarmee een ontwikkelaar vriendelijke interface op het ruwe RESTful blob-protocol. Nogmaals, Azure toepassingen en toepassingen voor externe toegang tot BLOB's met behulp van deze bibliotheek.
- Azure stations, een optie waarmee een Azure toepassing een pagina blob behandelen als een lokaal station met een NTFS-bestandssysteem gebruiken. Voor de toepassing eruitziet de blob pagina als een gewone Windows-bestandssysteem benaderd met standaard bestand I/O. In feite leest en schrijft naar de onderliggende pagina blob waarmee het station Azure worden verzonden. 

Als u wilt beveiligen tegen hardwarestoringen en verbetering van de beschikbaarheid, is elke blob gerepliceerd in drie computers in een datacenter Azure. Schrijven naar een blob bijgewerkt alle drie kopieën, zodat later leest inconsistente resultaten niet zien. U kunt ook opgeven dat van een blob-gegevens moeten worden gekopieerd naar een andere Azure datacenter in de dezelfde geo maar ten minste 500 mijl weg. Dit kopiëren, *geo-replicatie*, genoemd gebeurt binnen een paar minuten van een update met de blob en is handig voor noodherstel.

Gegevens in een BLOB's kunnen ook via de Azure *Content Delivery Network (CDN)*beschikbaar worden gesteld. Door caching kopieën van blob-gegevens op een groot aantal servers over de hele wereld, kunt de CDN snellere toegang tot gegevens die herhaaldelijk wordt geopend. 

Eenvoudig omdat zij, BLOB's zijn de juiste keuze in veel situaties. Opslag en streaming video en audio zijn duidelijke voorbeelden zijn back-ups en andere soorten gegevens archiveren. Ontwikkelaars kunnen ook BLOB's voor elk soort ongestructureerde gegevens die ze willen gebruiken. Met een eenvoudige manier om te slaan en toegang tot binaire gegevens kan verrassend handig zijn.


## <a name="dbinvm"></a>Een DBMS wordt uitgevoerd op een virtuele Machine

Veel toepassingen maken tegenwoordig gebruik van een soort databasebeheersysteem (DBMS). Relationele systemen, zoals SQL Server, zijn de meestgebruikte keuze, maar niet-relationele benaderingen, ook wel bekend als *NoSQL* technologieën, elke dag meer populaire ontvangt. Zodat de cloud-toepassingen gebruiken deze Gegevensopties Azure virtuele Machines kunt u uitvoeren van een DBMS (relationele of NoSQL) in een VM. [In afbeelding 2](#Fig2) ziet u hoe dit eruitziet met SQL Server.

<a name="Fig2"></a>![Diagram van SQL Server op een virtuele Machine][SQLSvr-vm]
 
**Afbeelding 2: Azure virtuele Machines kunnen met een DBMS in een VM, en persistentie door BLOB's geleverd.**

Ontwikkelaars en databasebeheerders ziet dit scenario er veel dezelfde software uitgevoerd in hun eigen datacenter. In het voorbeeld bijvoorbeeld bijna alle mogelijkheden van SQL Server kan worden gebruikt en u hebt volledige beheerrechten voor het systeem. U hebt ook de verantwoordelijkheid voor het beheer van de databaseserver, natuurlijk, net alsof deze lokaal zijn uitgevoerd.

Zoals [in figuur 2](#Fig2) ziet, verschijnen uw databases kunnen worden opgeslagen op de lokale schijf van de server wordt uitgevoerd in VM. Achter de echter wordt elk van deze schijven geschreven naar een Azure blob. (Dit is vergelijkbaar met het gebruik van een SAN in uw eigen datacenter, met een blob die veel fungeert als een LUN). Met een Azure blob gegevens die het bevat drie keer in een datacenter wordt gerepliceerd en, als u daarom, geo-gerepliceerd naar een ander datacenter in hetzelfde gebied vraagt. Het is ook mogelijk om met de opties, zoals een SQL Server-database-mirroring voor verbeterde betrouwbaarheid. 

Een andere manier gebruik van SQL Server in een VM is een hybride-toepassing maken waar de gegevens zich op Azure terwijl de toepassingslogica in ruimten wordt uitgevoerd. Bijvoorbeeld wellicht dit zinvol als toepassingen die worden uitgevoerd op meerdere locaties of op verschillende mobiele apparaten moeten dezelfde gegevens delen. Om de communicatie tussen de cloud database en op ruimten logica eenvoudiger kunt een organisatie Azure Virtual Network een virtueel particulier netwerk (VPN) verbinding maken tussen een Azure datacenter en een eigen datacenter in gebouwen.


## <a name="sqldb"></a>SQL-Database

Voor veel mensen is een DBMS in een VM wordt uitgevoerd de eerste optie die u bedenken kunt voor het beheren van gestructureerde gegevens in de cloud. Het is niet de enige keuze, noch is het altijd de beste keuze. In sommige gevallen beheren van gegevens met behulp van een Platform als een Service (PaaS) aanpak zinvol meer. Azure biedt een PaaS technologie met de naam van de SQL-Database, kunt u dit doen voor relationele gegevens. [In figuur 3](#Fig3) ziet u deze optie. 

<a name="Fig3"></a>![Diagram van een SQL-Database][SQL-db]
 
**Afbeelding 3: SQL-Database biedt gedeelde PaaS relationele opslag.**

SQL-Database krijgt elke klant een eigen fysiek exemplaar van SQL Server. In plaats daarvan biedt het een service met meerdere huurder een logische Database van SQL server voor elke klant. Alle klanten delen de compute en opslag capaciteit die de service levert. En net als bij de Blob-opslag, alle gegevens in een SQL-Database is opgeslagen op drie afzonderlijke computers binnen een Azure datacenter, zodat uw databases ingebouwde hoge beschikbaarheid (HA).

Aan een toepassing lijkt de SQL-Database veel op SQL Server. Toepassingen kunnen uitgeven van SQL-query's op relationele tabellen, T-SQL opgeslagen procedures gebruiken en transacties uitvoeren via meerdere tabellen. En aangezien access-toepassingen met behulp van het protocol in tabelvorm Data Stream (TDS), hetzelfde protocol gebruikt voor toegang tot SQL Server, SQL-Database met gegevens met behulp van de entiteit Framework, ADO.NET, JDBC en andere bekende data access-interfaces kunt werken. 

Maar omdat de SQL-Database is een cloud-service die wordt uitgevoerd in Azure datacenters, u hoeft niet te beheren op een van de fysieke aspecten van het systeem, zoals het gebruik van de schijf. Moet u ook geen zorgen te maken over het bijwerken van de software of andere beheertaken op laag niveau te verwerken. Elke klantorganisatie bepaalt nog steeds zijn eigen databases, natuurlijk met inbegrip van hun schema's en -aanmeldingen, maar veel van de beheertaken van routinetaken voor u klaar bent. 

Terwijl de SQL-Database ziet er veel SQL Server voor toepassingen, werken niet het precies hetzelfde als een DBMS uitvoeren op een fysieke of virtuele machine. Omdat deze op gedeelde hardware wordt uitgevoerd, verschillen de prestaties naargelang de belasting van deze hardware door al haar klanten. Dit betekent dat de prestaties van bijvoorbeeld een opgeslagen procedure in de SQL-Database van één dag naar het volgende variëren kan. 

Vandaag, kunt SQL-Database u een database met maximaal 150 GB maken. Als u werken met databases die groter zijn wilt, is de service bevat een optie *Federation*. Hiervoor maakt de beheerder van een database twee of meer *leden van de Federatie*, die elk een afzonderlijke database met een eigen schema is. Gegevens worden over deze leden, iets dat wordt vaak aangeduid als *sharding*, met elk een unieke *sleutel federation*toegewezen lid verspreid. Een toepassing problemen met SQL-query's op deze gegevens door te geven van de federation-sleutel waarmee u het onderdeel federation de query moeten het doel zijn. Dit kan met behulp van een traditionele relationele benadering met grote hoeveelheden gegevens. Er zijn zoals altijd compromis; query's noch transacties kunnen leden van de Federatie, bijvoorbeeld beslaan. Maar wanneer een relationele PaaS-service de beste keuze is en deze compromis aanvaardbaar zijn, met behulp van SQL-Federatie kan een goede oplossing.

SQL-Database kan worden gebruikt door toepassingen op Azure of elders, zoals in uw datacenter op gebouwen. Dit is het nuttig voor cloud-toepassingen die relationele gegevens moeten, alsmede toepassingen die profiteren kunnen van de gegevens opslaan in de cloud ruimten. Een mobiele toepassing mogelijk afhankelijk zijn van de SQL-Database voor het beheren van gedeelde relationele gegevens, bijvoorbeeld als mogelijk een voorraad-toepassing die wordt uitgevoerd op meerdere dealers over de hele wereld.

Na te denken over de SQL-Database genereert een probleem voor de hand liggende (en belangrijk): wanneer moet u SQL Server uitvoert in een VM en wanneer SQL-Database is een betere keuze? Zoals gebruikelijk zijn er e-mailverkeer en dus welke aanpak beter is afhankelijk van uw vereisten. 

Een eenvoudige manier om na te denken over het is weergeven van SQL-Database als voor nieuwe toepassingen, terwijl SQL Server in een VM een betere keuze is wanneer u een bestaande op-premises toepassing naar de cloud verplaatst. Het kan ook handig zijn te kijken dit besluit in een meer verfijnde manier echter. Bijvoorbeeld, SQL-Database is gemakkelijker te gebruiken, omdat er minimale installatie en beheer. Maar meer voorspelbare prestaties met SQL Server in een VM kan hebben - het is niet een gedeelde service - en biedt ook ondersteuning voor niet-federatieve database groter dan de SQL-Database. SQL-Database biedt nog steeds, ingebouwde replicatie van gegevens en de verwerking, effectief waardoor u met een DBMS met hoge beschikbaarheid met zeer weinig werk. Hoewel SQL Server u meer controle en een iets groter aantal opties hebt, is SQL-Database eenvoudiger instellen en aanzienlijk minder werk te beheren.

Ten slotte is het belangrijk te weten dat SQL-Database is niet de enige PaaS gegevensservice beschikbaar op Azure. Microsoft-partners bieden ook andere opties. ClearDB biedt bijvoorbeeld een MySQL PaaS aanbieden, terwijl Cloudant een optie NoSQL verkoopt. PaaS van gegevens de juiste oplossing in veel gevallen zijn, en dus deze aanpak voor het beheer van een belangrijk onderdeel van Azure.


### <a name="datasync"></a>SQL-gegevens synchroniseren

Terwijl de SQL-Database drie exemplaren van elke database binnen een enkele Azure datacenter onderhouden, repliceren niet het automatisch gegevens tussen Azure datacenters. In plaats daarvan biedt SQL gegevens synchroniseren, een service die u gebruiken kunt om dit te doen. [In figuur 4](#Fig4) ziet u hoe dit eruitziet.

<a name="Fig4"></a>![Diagram van een SQL-gegevens synchroniseren][SQL-datasync]
 
**Figuur 4: SQL-gegevens Sync synchroniseert gegevens in een SQL-Database met gegevens in andere Azure en datacenters ruimten.**

Zoals blijkt uit het diagram, kunt SQL-gegevens synchroniseren gegevens synchroniseren over verschillende locaties. Stel dat u werkt een toepassing in meerdere Azure datacentra, bijvoorbeeld met gegevens die zijn opgeslagen in een SQL-Database. U kunt SQL-gegevens synchroniseren die gegevens gesynchroniseerd houden. SQL-gegevens synchroniseren kunt ook synchroniseren van gegevens tussen een Azure datacenter en een exemplaar van SQL Server wordt uitgevoerd in een datacenter op gebouwen. Dit kan handig zijn voor zowel een lokale kopie van de gegevens die worden gebruikt door toepassingen voor bedrijfsruimten en een wolk kopie die wordt gebruikt door toepassingen die worden uitgevoerd op Azure. En hoewel het niet wordt weergegeven in de afbeelding, SQL Data synchronisatie kan worden gebruikt voor het synchroniseren van gegevens tussen SQL-Database en SQL Server wordt uitgevoerd in een VM in Azure of elders.

Synchronisatie kan bi-directionele en bepaalt u precies welke gegevens gesynchroniseerd en hoe vaak dit gebeurt. (Synchronisatie tussen databases niet atomisch, - er is echter altijd ten minste enigszins vertraagd.) En maar wordt gebruikt, het instellen van synchronisatie met SQL-gegevens synchroniseren is volledig configuratie aangedreven; Er is geen code te schrijven.


### <a name="datarpt"></a>Rapporteren van SQL-gegevens met behulp van virtuele Machines

Zodra een database gegevens bevat, wilt iemand waarschijnlijk rapporten maken met gegevens. Azure kunt uitvoeren SQL Server Reporting Services (SSRS) in Azure virtuele Machines die is functioneel gelijk aan het uitvoeren van SQL Server Reporting Services op locatie. Vervolgens kunt u SSRS rapporten uitvoeren op gegevens die zijn opgeslagen in een SQL-Database Azure.  [In figuur 5](#Fig5) ziet hoe dit werkt.

<a name="Fig5"></a>![Diagram van SQL reporting][SQL-report]
 
**Figuur 5: SQL Server Reporting Services uitvoert in een Azure virtuele Machines biedt reporting services voor gegevens in een SQL-Database. .**

Voordat een gebruiker een rapport ziet, iemand wordt gedefinieerd wat dit verslag moet er als volgt uitzien (stap 1). Met SQL Server Reporting Services op een VM, u kunt dit doen met behulp van twee methoden: SQL Server Data Tools, onderdeel van SQL Server 2012 of de voorafgaande taak, Business Intelligence (BI) Development Studio. Deze rapportdefinities worden net als bij SQL Server Reporting Services, Report Definition Language (RDL) worden uitgedrukt. Nadat de RDL-bestanden voor een rapport hebt gemaakt, worden ze geüpload naar een VM in de cloud (stap 2). De rapportdefinitie is nu klaar voor gebruik.

Een gebruiker van de toepassing wordt vervolgens het rapport (stap 3). De toepassing deze aanvraag doorgegeven aan de VM SSRS (stap 4), die een SQL-Database of andere gegevensbronnen om de benodigde gegevens van contactpersonen (stap 5). SQL Server Reporting Services gebruikt deze gegevens en de relevante RDL-bestanden weer te geven van het rapport (stap 6), vervolgens het rapport naar de toepassing (stap 7), die wordt weergegeven voor de gebruiker (stap 8) retourneert.

Het scenario dat hier is afgebeeld, is het insluiten van een rapport in een toepassing, niet de enige optie. Het is ook mogelijk om rapporten in Report Manager van SharePoint op de VM VM of op andere manieren weergeven. Rapporten kunnen ook worden gecombineerd, met een rapport met een koppeling naar een andere.

SQL Server Reporting Services op een Azure VM biedt u volledige functionaliteit als een oplossing voor de rapportage in de cloud. Rapporten kunnen een willekeurige gegevensbron ondersteund door SQL Server Reporting Services gebruiken. Toepassingen en rapporten kunnen ingesloten code of assembly's voor de ondersteuning van aangepaste gedragingen omvatten. Rapport kan worden uitgevoerd en weergave zijn snel omdat rapportinhoud en de motor samen op dezelfde virtuele server worden uitgevoerd.



## <a name="tblstor"></a>Tabelopslag

Relationele gegevens is in veel gevallen handig, maar het is niet altijd de juiste keuze. Als uw toepassing snel en eenvoudig toegang tot grote hoeveelheden losjes gestructureerde gegevens moet, bijvoorbeeld een relationele database werkt mogelijk niet goed. Een technologie voor NoSQL is waarschijnlijk een betere optie.

Opslag van Azure tabel is een voorbeeld van dit soort NoSQL aanpak. Ondanks de naam ondersteunt geen opslag van de tabel standaard relationele tabellen. In plaats daarvan biedt wat wordt genoemd een *sleutelwaarde opslaan*, een set gegevens koppelen aan een bepaalde sleutel en vervolgens laat de toegang tot een toepassing die gegevens door middel van de sleutel. [Figuur 6](#Fig6) ziet u de basisprincipes.

<a name="Fig6"></a>![Diagram van de tabelopslag][SQL-tblstor]
 
**Figuur 6: Azure tabelopslag is een sleutel/waarde-archief dat snelle, eenvoudige toegang tot grote hoeveelheden gegevens biedt.**

Elke tabel is net als BLOB's, gekoppeld aan een account Azure opslag. Tabellen zijn ook veel genoemd als BLOB's met een URL van het formulier

http://&lt;*StorageAccount*&gt;.table.core.windows.net/&lt;*tabelnaam*&gt;

Als de afbeelding wordt weergegeven, wordt elke tabel is onderverdeeld in een aantal partities, die elk op een afzonderlijke computer kan worden opgeslagen. (Dit is een vorm van sharding, net als bij SQL Federation.) Een tabel met de RESTful OData-protocol of de clientbibliotheek Azure opslag toegang tot Azure toepassingen en toepassingen die elders worden uitgevoerd.

Elke partitie in een tabel bevat een aantal *entiteiten*, elk met maximaal 255 *Eigenschappen*. Elke eigenschap heeft een naam, een type (bijvoorbeeld binair, Bool, DateTime, Int of tekenreeks) en een waarde. In tegenstelling tot relationele opslag, deze tabellen geen vaste schema hebben en dus verschillende entiteiten in dezelfde tabel kunnen bevatten eigenschappen met verschillende typen. Een entiteit kan gewoon een tekenreekseigenschap met een naam, bijvoorbeeld, terwijl een andere entiteit in dezelfde tabel heeft twee Int eigenschappen met een klant-id en een credit rating hebben.

Ter identificatie van een bepaalde entiteit binnen een tabel biedt een toepassing de sleutel van de entiteit. De sleutel bestaat uit twee delen: een *partitiesleutel* waarmee een specifieke partitie en een *rijsleutel* waarmee een entiteit in die partitie. In [figuur 6](#Fig6), bijvoorbeeld, vraagt de client de entiteit met de partitie A en rijsleutel 3 en opslag van de tabel als resultaat gegeven die entiteit, met inbegrip van alle eigenschappen bevat.

Met deze structuur kunt tabellen groot - één tabel kan bevatten maximaal 100 terabytes aan gegevens - en maakt snelle toegang tot de gegevens die ze bevatten. Het brengt ook beperkingen, echter. Er is bijvoorbeeld geen ondersteuning voor transactionele updates die gelden voor tabellen of zelfs partities in één tabel. Een reeks updates aan een tabel kan alleen worden gegroepeerd in een atomic-transacties als de entiteiten die betrokken zijn allemaal op dezelfde partitie. Er ook een tabel op geen enkele manier gebaseerd op de waarde van de eigenschappen, noch is er ondersteuning voor joins voor meerdere tabellen. En in tegenstelling tot relationele databases, tabellen bieden geen ondersteuning voor opgeslagen procedures.

Azure tabelopslag is een goede keuze voor toepassingen die snelle en goedkope toegang tot grote hoeveelheden losjes gestructureerde gegevens. Bijvoorbeeld, een Internet-toepassing waarin gegevens voor een groot aantal gebruikers profiel opgeslagen tabellen gebruiken. Snelle toegang is belangrijk in deze situatie en de toepassing waarschijnlijk hoeft niet de volledige kracht van SQL. Geven om deze functionaliteit te krijgen van de snelheid en omvang kan soms zinvol en tabelopslag is dus gewoon de juiste oplossing voor bepaalde problemen.


## <a name="hadoop"></a>Hadoop

Organisaties hebben datawarehouses bouwen voor decennia. Deze verzamelingen van gegevens, meestal in relationele tabellen kunnen gebruikers werken met en leren van de gegevens op verschillende manieren. Met SQL Server, bijvoorbeeld, is het gemeenschappelijk gebruik van hulpprogramma's zoals SQL Server Analysis Services om dit te doen.

Maar Stel dat u wilt doen van de analyse van niet-relationele gegevens. Uw gegevens kunnen vele vormen aannemen: informatie van sensoren of RFID-tags, logboekbestanden in server-farms, clickstream data geproduceerd door webtoepassingen van afbeeldingen van medische diagnostische hulpmiddelen en nog veel meer. Deze gegevens kan ook zijn hele grote, te groot om met een traditionele datawarehouse effectief worden gebruikt. Gegevens voor grote problemen, zoals zeldzame maar een paar jaar geleden, nu wel heel gebruikelijk.

Om dit soort grote gegevens analyseren, onze bedrijfstak grotendeels is geconvergeerd in één oplossing: de open source-technologie Hadoop. Hadoop wordt uitgevoerd op een cluster van fysieke of virtuele machines, verspreiding van de gegevens in deze machines werkt en gelijktijdig wordt verwerkt. Meer machines Hadoop heeft gebruikt, des te sneller kunt uitvoeren ongeacht het werk doet.

Dit soort problemen is een natuurlijke past bij de openbare cloud. In plaats van handhaving van een VMs leger van lokale servers die mogelijk inactief veel van de tijd, Hadoop uitgevoerd in de cloud kunt u maken (en betaalt) alleen wanneer u ze nodig hebt. Beter zelfs, meer en meer van de grote gegevens die u wilt analyseren met Hadoop wordt gemaakt in de cloud, zodat u de problemen van deze verplaatsen. Als u deze synergie-effecten, biedt Microsoft Hadoop op Azure. [Figuur 7](#Fig7) ziet u de belangrijkste onderdelen van deze service.

<a name="Fig7"></a>![Diagram van hadoop][hadoop]

**Figuur 7: Hadoop op Azure voert MapReduce taken uit die gegevens gelijktijdig met meerdere virtuele machines.**

Voor het gebruik van Hadoop op Azure, vragen u eerst dit cloud platform voor het maken van een cluster Hadoop waarmee het aantal VMs die u nodig hebt. Het instellen van een cluster Hadoop zelf is een niet-triviale taak en dus laten doen Azure zinvol is. Wanneer u klaar bent met het cluster, u deze afsluiten. Er is niet nodig om te betalen voor het berekenen van bronnen die u niet gebruikt.

Een Hadoop-toepassing gewoonlijk een *taak*genoemd bekend als *MapReduce*-een programmeermodel gebruikt. Zoals in de afbeelding kunt zien, de logica voor een project MapReduce wordt uitgevoerd gelijktijdig over veel VMs. Gegevens in parallelle verwerking, kunt Hadoop analyseren gegevens sneller dan één computer oplossingen.

Op Azure, de gegevens een taak werkt op MapReduce wordt meestal opgeslagen in blob-opslag. In Hadoop verwacht MapReduce taken echter gegevens worden opgeslagen in het *Hadoop Distributed bestand System (HDFS)*. HDFS is vergelijkbaar met de Blob-opslag in sommige opzichten; Deze gegevens worden gerepliceerd naar meerdere fysieke servers, bijvoorbeeld. In plaats van deze functionaliteit dupliceert, stelt Hadoop op Azure Blob-opslag via de API HDFS in plaats daarvan als de figuur wordt weergegeven. Terwijl de logica in een taak MapReduce dat het gewone HDFS bestanden opent denkt, wordt de taak in feite werken met gegevens stroomsgewijs verzonden naar deze van BLOB's. En ter ondersteuning van het geval waarin meerdere taken worden uitgevoerd via dezelfde gegevens, Hadoop op Azure ook kopiëren van gegevens van BLOB's in volledige HDFS in het VMs uitgevoerd. 

MapReduce taken worden meestal geschreven in Java tegenwoordig een benadering die Hadoop op Azure ondersteunt. Microsoft heeft ook ondersteuning toegevoegd voor MapReduce projecten maken in andere talen, zoals C#, F # en JavaScript. Het doel is om deze technologie grote gegevens gemakkelijker toegankelijk maken voor een grotere groep van ontwikkelaars.

Hadoop bevat HDFS en MapReduce, andere technologieën waarmee gebruikers gegevens analyseren zonder het schrijven van een taak MapReduce zelf. Varken is bijvoorbeeld een high-level taal is ontworpen voor het analyseren van grote gegevens terwijl de component biedt een SQL-achtige taal HiveQL genoemd. Zowel varkens als component in werkelijkheid genereert MapReduce taken die HDFS gegevens verwerken, maar ze deze complexiteit van hun gebruikers verbergen. Beide zijn voorzien van Hadoop op Azure.

Microsoft biedt ook een HiveQL stuurprogramma voor Excel. Met behulp van een Excel-invoegtoepassing, bedrijfsanalisten maakt HiveQL-query's (en dus MapReduce taken) rechtstreeks vanuit Excel, en vervolgens verwerken en visualiseren van de resultaten met behulp van PowerPivot en andere hulpprogramma's voor Excel. Hadoop op Azure bevat andere technologieën, zoals de machine learning bibliotheken Mahout en de grafiek mijnbouw systeem Pegasus.

BIG data-analyse is belangrijk en dus Hadoop is ook van belang. Dankzij Hadoop als een beheerde service op Azure, en koppelingen naar vertrouwde programma's zoals Excel, beoogt Microsoft bij het maken van deze technologie voor een groter aantal gebruikers toegankelijk.

Gegevens van alle soorten is meer in het algemeen belang. Dit is waarom Azure een reeks opties voor data management en business analytics omvat. Ongeacht welke toepassing u probeert te maken, is het waarschijnlijk dat u vindt iets in deze cloud platform dat geschikt is voor u.

[blobs]: ./media/cloud-storage/Data_01_Blobs.png
[SQLSvr-vm]: ./media/cloud-storage/Data_02_SQLSvrVM.png
[SQL-db]: ./media/cloud-storage/Data_03_SQLdb.png
[SQL-datasync]: ./media/cloud-storage/Data_04_SQLDataSync.png
[SQL-report]: ./media/cloud-storage/Data_05_SQLReporting.png
[SQL-tblstor]: ./media/cloud-storage/Data_06_TblStorage.png
[hadoop]: ./media/cloud-storage/Data_07_Hadoop.png
