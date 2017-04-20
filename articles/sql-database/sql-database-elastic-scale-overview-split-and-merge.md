<properties 
    pageTitle="Verplaatsen van gegevens tussen databases schaalvergroting cloud | Microsoft Azure" 
    description="Hoe shards bewerken en verplaatsen van gegevens via een zelf gehoste service elastische database API's gebruikt." 
    services="sql-database" 
    documentationCenter="" 
    manager="jhubbard" 
    authors="ddove"/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/27/2016" 
    ms.author="ddove" />

# <a name="moving-data-between-scaled-out-cloud-databases"></a>Verplaatsen van gegevens tussen databases schaalvergroting cloud

Als u een Software als een Service-ontwikkelaar bent en plotseling uw app enorme vraag ondergaat, moet u ten behoeve van de groei. Zo kunt u meer databases (shards) toevoegen. Hoe u de gegevens naar de nieuwe databases zonder te verstoren, de integriteit van gegevens verspreiden? Gebruik het **hulpprogramma gesplitste samenvoegen** om gegevens te verplaatsen van beperkte databases met nieuwe databases.  

Het hulpprogramma gesplitste samenvoegbewerking uitgevoerd als een webservice Azure. Een beheerder of ontwikkelaar maakt gebruik van het hulpprogramma shardlets (gegevens uit een shard) verplaatsen tussen verschillende databases (shards). Het hulpprogramma gebruikt shard kaart management de metadata-database beheren en consistente toewijzingen te waarborgen.

![Overzicht][1]

## <a name="download"></a>Downloaden
[Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge/)


## <a name="documentation"></a>Documentatie
1. [Elastische gesplitste samenvoegen tool zelfstudie](sql-database-elastic-scale-configure-deploy-split-and-merge.md)
* [Gesplitste samenvoegen Beveiligingsconfiguratie](sql-database-elastic-scale-split-merge-security-configuration.md)
* [Beveiligingsoverwegingen voor gesplitste samenvoegen](sql-database-elastic-scale-split-merge-security-configuration.md)
* [Shard kaart management](sql-database-elastic-scale-shard-map-management.md)
* [Migreren van bestaande databases te schalen](sql-database-elastic-convert-to-use-elastic-tools.md)
* [Elastische Databasehulpmiddelen](sql-database-elastic-scale-introduction.md)
* [Elastische Database tools-verklarende woordenlijst](sql-database-elastic-scale-glossary.md)

## <a name="why-use-the-split-merge-tool"></a>Waarom gebruikt u het hulpprogramma gesplitste samenvoegen?

**Flexibiliteit**

Toepassingen moeten flexibel rekken de grenzen van één database Azure SQL DB. Het hulpprogramma gebruiken om gegevens te verplaatsen wanneer dat nodig is voor nieuwe databases integriteit behouden.

**Splitsen om te groeien** 

U moet verhogen de totale capaciteit voor de verwerking van de explosieve groei. Hiervoor extra capaciteit door de gegevens sharding en maken door te distribueren over stapsgewijs meer databases, totdat de capaciteitsbehoeften wordt voldaan. Dit is een goed voorbeeld van de functie 'splitsen'. 

**Samenvoegen naar verkleinen**

Capaciteit moet inkrimpen vanwege het seizoensgebonden karakter van een bedrijf. Het hulpprogramma kunt u minder eenheden van de schaal verkleinen bij het bedrijf wordt vertraagd. Deze eis heeft betrekking op de functie 'samenvoegen' in de elastische schaal gesplitst samenvoegen-Service. 

**Hotspots beheren door te verplaatsen shardlets**

De toewijzing van shardlets naar shards kan met meerdere huurders per database leiden tot capaciteit knelpunten op sommige shards. Hiervoor moet bij het opnieuw toewijzen van shardlets of bezet shardlets verplaatsen naar nieuwe of minder gebruikte shards. 

## <a name="concepts--key-features"></a>Concepten & belangrijke functies

**Klant-hosted services**

Samenvoegen van de splitsing wordt geleverd als een gehoste customer service. U moet implementeren en de service host van uw abonnement op Microsoft Azure. Het pakket dat u van NuGet downloadt bevat een configuratiesjabloon te voltooien met de informatie voor uw specifieke implementatie. Zie de [zelfstudie gesplitste samenvoegen](sql-database-elastic-scale-configure-deploy-split-and-merge.md) voor meer informatie. Omdat de service wordt uitgevoerd op uw abonnement Azure, kunt u instellen en configureren van de meeste beveiligingsaspecten van de service. De standaardsjabloon bevat de opties voor het configureren van SSL, op basis van certificaten voor clientverificatie, codering opgeslagen referenties, DoS, beveiligen en IP-beperkingen. U vindt meer informatie over de beveiligingsaspecten in het volgende document [splitsen samenvoegen beveiligingsconfiguratie](sql-database-elastic-scale-split-merge-security-configuration.md).

De standaard geïmplementeerd met één werknemer en een web-functie de service wordt uitgevoerd. Gebruik het formaat A1 VM in Azure Cloud Services. Terwijl u kunt deze instellingen niet wijzigen bij het implementeren van het pakket, kan u ze na een succesvolle implementatie in de actieve cloud-service (via de portal Azure). Houd er rekening mee dat de rol van de werknemer moet niet worden geconfigureerd voor meer dan één exemplaar om technische redenen. 

**Shard kaart integratie**

De gesplitste samenvoegen service communiceert met de shard plattegrond van de toepassing. Als u de service gesplitste samenvoegen voor splitsen of samenvoegen van bereiken of shardlets tussen shards, blijft de service automatisch de map shard up-to-date. Hiervoor is de service verbinding maakt met de shard kaart manager-database van de toepassing en bereiken en toewijzingen als voortgang splitsen/samenvoegen/move-aanvragen worden bijgehouden. Dit zorgt ervoor dat de kaart shard altijd een actuele weergave presenteert bij het samenvoegen van gesplitste bewerkingen gaat. Splitsen, worden samenvoegen en shardlet verkeer bewerkingen geïmplementeerd door een partij van shardlets uit de bron shard verplaatsen naar de doel-shard. Tijdens de shardlet verplaatsing van de shardlets onder de huidige batch zijn gemarkeerd als off line in de map shard en zijn niet beschikbaar voor gegevens-afhankelijke **OpenConnectionForKey** API gebruiken. 

**Consistente shardlet verbindingen**

Bij verplaatsing van gegevens voor een nieuwe batch shardlets starten, geen shard-overzicht gegevensafhankelijke routering verbindingen met het opslaan van de shardlet shard worden gedood en de daaropvolgende verbindingen uit de map shard API's met deze shardlets worden geblokkeerd tijdens het verplaatsen van gegevens om te voorkomen dat de inconsistente. Verbindingen met andere shardlets op de dezelfde shard ook zal krijgen gedood, maar opnieuw worden uitgevoerd onmiddellijk op probeer het opnieuw. Nadat de batchverwerking is verplaatst, de shardlets opnieuw voor het doel shard online zijn gemarkeerd en de brongegevens wordt verwijderd uit de bron shard. De service wordt via deze stappen voor elke batch tot alle shardlets zijn verplaatst. Dit zal leiden tot verschillende verbinding kill bewerkingen in de loop van het voltooien van de bewerking splitsen/samenvoegen/verplaatsen.  

**Beheren van de beschikbaarheid van shardlet**

De doden aan de huidige batch van shardlets zoals hierboven besproken verbinding beperken beperkt het toepassingsgebied van onbeschikbaarheid tot één partij van shardlets tegelijk. Dit heeft de voorkeur boven een aanpak waarbij de volledige shard blijft offline voor alle shardlets zijn in de loop van een bewerking splitsen of samenvoegen. De grootte van een partij, gedefinieerd als het aantal verschillende shardlets tegelijk verplaatsen is een configuratieparameter. Deze kan worden gedefinieerd voor elke bewerking samenvoegen en splitsen afhankelijk van de beschikbaarheid en prestaties wensen van de toepassing. Houd er rekening mee dat het bereik dat in de map shard vergrendeld mogelijk groter is dan de opgegeven grootte van de partij. Dit komt omdat de service de grootte van het bereik neemt dat het werkelijke aantal sharding sleutelwaarden in de gegevens komt ongeveer overeen met de batchgrootte. Dit is belangrijk om te onthouden in het bijzonder voor zeer gevulde sharding sleutels. 

**Opslag van metagegevens**

De gesplitste samenvoegen-service gebruikt een database om de status ervan bijhouden en logboeken tijdens de verwerking van de aanvraag. De gebruiker deze database gemaakt in hun abonnement en voorziet de verbindingsreeks in het configuratiebestand voor de implementatie van de service. Beheerders van de organisatie van de gebruiker kunnen ook verbinding maken met deze database verzoek voortgang te controleren en te onderzoeken, gedetailleerde informatie over mogelijke fouten.

**Sharding bewustzijn**

De gesplitste samenvoegen service onderscheidt (1) een laptopgeheugen tabellen en tabellen referentie (2) (3) normale tabellen. De semantiek van een splitsen/samenvoegen/verplaatsing is afhankelijk van het type van de tabel die wordt gebruikt en worden als volgt gedefinieerd: 

* **Een laptopgeheugen tabellen**: splitsen en samenvoegen verplaatst van het ene shardlets bron naar doel shard. Na de succesvolle afronding van de algemene aanvraag zijn die shardlets niet meer aanwezig op de bron. Overigens moeten aanwezig zijn op het doel shard de doeltabellen en mag geen gegevens in het doelbereik voor de verwerking van de bewerking. 

* **Tabellen**: voor tabellen, splitsen, samenvoegen en bewerkingen kunt u de gegevens uit de bron kopiëren naar de doel-shard verplaatsen. Bedenk echter dat er geen wijzigingen optreden op de shard doel voor een bepaalde tabel een rij is al aanwezig in deze tabel op het doel. De tabel is leeg voor elke bewerking verwijzing tabel kopiëren naar verwerkt.

* **Andere tabellen**: andere tabellen kunnen aanwezig zijn op de bron of het doel van een bewerking samenvoegen en splitsen. De gesplitste samenvoegen-service negeert deze tabellen voor kopiëren of verplaatsen van gegevens. Bedenk echter dat zij deze bewerkingen in geval van beperkingen kunnen verstoren.

De informatie op de verwijzing versus een laptopgeheugen tabellen wordt geleverd door de **SchemaInfo** API's op de kaart shard. In het volgende voorbeeld illustreert het gebruik van deze API's op een bepaalde shard kaart manager object smm: 

    // Create the schema annotations 
    SchemaInfo schemaInfo = new SchemaInfo(); 

    // Reference tables 
    schemaInfo.Add(new ReferenceTableInfo("dbo", "region")); 
    schemaInfo.Add(new ReferenceTableInfo("dbo", "nation")); 

    // Sharded tables 
    schemaInfo.Add(new ShardedTableInfo("dbo", "customer", "C_CUSTKEY")); 
    schemaInfo.Add(new ShardedTableInfo("dbo", "orders", "O_CUSTKEY")); 

    // Publish 
    smm.GetSchemaInfoCollection().Add(Configuration.ShardMapName, schemaInfo); 

De tabellen 'regio' en 'natie' als de tabellen zijn gedefinieerd en worden gekopieerd met splitsen/samenvoegen/verplaatst. 'klant' en 'orders' worden op hun beurt gedefinieerd als een laptopgeheugen tabellen. C_CUSTKEY en O_CUSTKEY dienen als de sleutel sharding. 

**Referentiële integriteit**

De gesplitste samenvoegen service afhankelijkheden tussen tabellen analyseert en primaire sleutel refererende sleutels gebruikt voor het stadium van de bewerkingen voor het verplaatsen van tabellen en shardlets. In het algemeen wordt referentietabellen eerst gekopieerd op volgorde van afhankelijkheid, worden gekopieerd op volgorde van hun afhankelijkheden binnen elke batch shardlets. Dit is nodig zodat de beperkingen op het doel shard FK PK worden gehonoreerd, ontvangt de nieuwe gegevens. 

**Shard kaart samenhang en de uiteindelijke voltooiing**

Bij storingen, de gesplitste samenvoegen service operations hervat na een stroomstoring en beoogt te voltooien in de voortgang van aanvragen. Echter kan er onherstelbare situaties, bijvoorbeeld wanneer het doel shard verloren is gegaan of onherstelbaar worden beschadigd. In deze omstandigheden blijven sommige shardlets die moeten worden verplaatst zijn bevinden zich op de bron shard. De service zorgt ervoor dat shardlet toewijzingen alleen worden bijgewerkt nadat u de benodigde gegevens heeft gekopieerd naar het doel. Shardlets worden alleen op de broncomputer verwijderd nadat alle hun gegevens zijn gekopieerd naar het doel en de bijbehorende toewijzingen zijn bijgewerkt. De verwijderingsbewerking gebeurt op de achtergrond, terwijl het bereik op de doel-shard al online is. De gesplitste samenvoegen service garandeert altijd de juistheid van de toewijzingen die zijn opgeslagen in de map shard.


## <a name="the-split-merge-user-interface"></a>De gebruikersinterface van gesplitste samenvoegen

Gesplitste samenvoegen-servicepakket omvat de rol van een werknemer en de Webrol van een. De rol van de webpagina wordt gebruikt om gesplitste samenvoegen aanvragen indienen op een interactieve manier. De belangrijkste onderdelen van de gebruikersinterface zijn als volgt:

-    Bewerkingstype: Het bewerkingstype is een keuzerondje waarmee de aard van de bewerking die wordt uitgevoerd door de service voor dit verzoek. U kunt kiezen tussen de splitsing samenvoegen en scenario's te verplaatsen. U kunt ook een eerder ingediende bewerking annuleren. U kunt gebruik splitsen, samenvoegen en aanvragen voor bereik shard kaarten te verplaatsen. Lijst shard wijst alleen ondersteuning voor migratiebewerkingen.

-    Shard kaart: Informatie over de shard-kaart en de database voor het hosten van uw kaart shard betrekking hebben op de volgende sectie van de parameters van de aanvraag. In het bijzonder moet u de naam opgeven van de Azure SQL-databaseserver en de database de shardmap, referenties om verbinding met de shard database en ten slotte op de naam van de map shard te hosten. Op dit moment de bewerking kan slechts één enkele reeks referenties. Deze referenties moeten voldoende rechten hebt voor het uitvoeren van wijzigingen in de toewijzing shard ook dat de gegevens van de gebruiker op de shards.

-    Bronbereik (splitsen en samenvoegen): een bewerking samenvoegen en splitsen een bereik met de hoge en lage sleutel verwerkt. Als u een bewerking met een niet-gebonden hoge waarde, het selectievakje 'hoge sleutel is max' en het hoge veld leeg laten. De belangrijkste waarden die u opgeeft, hoeft niet precies overeenkomt met een toewijzing en de grenzen in de kaart shard. Als u grenzen voor het bereik op alle opgeeft wordt de service afgeleid het dichtst bereik voor u automatisch. Het GetMappings.ps1 PowerShell-script kunt u de huidige toewijzingen in een bepaalde shard kaart ophalen.

-    Gedrag van de bron splitsing (scheiding): definiëren voor gesplitste bewerkingen, het punt om het bronbereik te splitsen. Hiertoe verstrekken de sharding sleutel waar u de splitsing. Gebruik het keuzerondje opgeven of u het onderste gedeelte van het bereik (met uitzondering van de sleutel splitsen) wilt verplaatsen of wilt u het bovenste gedeelte te verplaatsen (inclusief de sleutel splitsen).

-    Shardlet (verplaatsen) bron: verplaatsen bewerkingen anders zijn dan bewerkingen splitsen of samenvoegen als u een bereik voor het beschrijven van de bron is niet vereist. Een bron voor het verplaatsen wordt gewoon aangeduid met sharding waarde van de sleutel die u wilt verplaatsen.

-    Shard (split) doel: zodra u de informatie hebt opgegeven op de bron van de gesplitste bewerking, moet u opgeven waar u de gegevens wilt kopiëren naar doordat de Azure SQL server en database databasenaam voor het doel.

-    Het doelbereik (samenvoegen): samenvoegbewerkingen shardlets verplaatsen naar een bestaande shard. U identificeren de bestaande shard door middel van het bestaande bereik dat u samenvoegen wilt met de grenzen van het bereik.

-    Omvang van de partij: De batchgrootte bepaalt het aantal shardlets dat op een moment tijdens het verplaatsen van gegevens off line gaan. Dit is een geheel getal waar u kleinere waarden kunt gebruiken wanneer u gevoelig bent voor lange perioden van uitval van de shardlets. Hogere waarden verhoogt de tijd die een bepaalde shardlet is off line, maar de prestaties mogelijk verbeterd.

-    Bewerking-Id (annuleren): Als u een bewerking die niet meer nodig hebt, kunt u de bewerking annuleren door de bewerking-ID in dit veld. Kunt u de bewerking-ID ophalen uit de tabel met de status (Zie punt 8.1) of van de uitvoer in de webbrowser wanneer u de aanvraag hebt ingediend.


## <a name="requirements-and-limitations"></a>Vereisten en beperkingen 

De huidige implementatie van de gesplitste samenvoegen service is onderworpen aan de volgende vereisten en beperkingen: 

* De shards moeten bestaan en in de map shard worden geregistreerd voordat een gesplitste samenvoegbewerking op deze shards kan worden uitgevoerd. 

* De service maakt geen tabellen of alle andere objecten in de database automatisch als onderdeel van haar activiteiten. Dit betekent dat het schema voor alle een laptopgeheugen tabellen en tabellen moet aanwezig zijn op het doel shard voorafgaand aan elke bewerking splitsen/samenvoegen/verplaatsen. Een laptopgeheugen tabellen in het bijzonder moeten in het bereik waar de nieuwe shardlets zullen worden toegevoegd door een splitsen/samenvoegen/verplaatsing niet leeg zijn. Anders mislukt de bewerking de eerste consistentiecontrole van de doel-shard. Rekening mee dat gegevens alleen worden gekopieerd als de verwijzing tabel leeg is en er zijn geen garanties voor consistentie met betrekking tot andere gelijktijdige op de tabellen met de schrijfbewerkingen verwijzing. Het is raadzaam dit: bij het uitvoeren van bewerkingen splitsen/samenvoegen, geen schrijfbewerkingen wijzigingen aanbrengen in de tabellen.

* De service is gebaseerd op rij-id die door een unieke index of sleutel waarin de toets sharding ter verbetering van de prestaties en betrouwbaarheid voor grote shardlets. Hierdoor kan de service om gegevens te verplaatsen op een zelfs weer specifieker dan alleen sharding waarde van de sleutel. Hierdoor verminderen de maximale hoeveelheid ruimte en vergrendelingen die nodig tijdens de bewerking zijn. Kunt u een unieke index of een primaire sleutel, met inbegrip van de sleutel sharding op een bepaalde tabel als u wilt gebruikt u die tabel met splitsen/samenvoegen/move-aanvragen te maken. Voor betere prestaties, moet het sharding-sleutel bestaan uit de eerste kolom in de sleutel of de index.

* Tijdens de verwerking van bepaalde gegevens van de shardlet mogelijk zowel op de bron en de doel-shard. Dit is nodig om u te beschermen tegen storingen tijdens de verplaatsing van de shardlet. De integratie van de gesplitste samenvoegen met de shard kaart zorgt ervoor dat verbindingen via de gegevens afhankelijke API's met behulp van de methode **OpenConnectionForKey** op de kaart shard routering een inconsistente tussenliggende statussen niet ziet. Wanneer u verbinding maakt met de bron of het doel shards zonder de methode **OpenConnectionForKey** , kunnen inconsistente tussenliggende statussen echter worden zichtbaar wanneer u gaat splitsen/samenvoegen/move-aanvragen. Deze verbindingen kunnen geheel of gedeeltelijk dubbele resultaten afhankelijk van het tijdstip of de shard onderliggende de verbinding weer. Deze beperking omvat momenteel de verbindingen te elastisch schaal meerdere Shard-query.

* De metagegevensdatabase voor de splitsing samenvoegen-service moet niet worden gedeeld tussen verschillende rollen. De rol van de gesplitste samenvoegen-service wordt uitgevoerd in een gefaseerde installatie moet verwijzen naar een andere metagegevensdatabase dan de rol van de productie.
 

## <a name="billing"></a>Facturering 

De gesplitste samenvoegen-service wordt uitgevoerd als een service cloud in uw abonnement op Microsoft Azure. Toeslagen voor cloud services gelden daarom uw exemplaar van de service. Tenzij u veel splitsen/samenvoegen/verplaatsen bewerkingen, wordt u aangeraden dat u uw split samenvoegen cloud-service verwijderen. Dat bespaart u kosten voor het uitvoeren of exemplaren van de cloud-service geïmplementeerd. U kunt opnieuw te implementeren en de configuratie van uw gemakkelijk runnable starten wanneer u wilt splitsen of samenvoegen-bewerkingen uitvoeren. 
 
## <a name="monitoring"></a>Monitoring 
### <a name="status-tables"></a>Status van tabellen 

De gesplitste samenvoegen-Service biedt de tabel **RequestStatus** in de metagegevens opslaan in database voor het controleren van voltooide en lopende aanvragen. De tabel bevat een rij voor elke splitsing samenvoegen aanvraag die is ingediend voor dit exemplaar van de service gesplitste samenvoegen. U kunt voor elke aanvraag de volgende informatie:

* **Timestamp**: de tijd en datum waarop de aanvraag is gestart.

* **Bewerkingsnummer**: een GUID die een unieke aanduiding voor de aanvraag. Deze aanvraag kan ook worden gebruikt om de bewerking te annuleren als deze nog steeds voortdurende.

* **Status**: de huidige status van de aanvraag. Voor lopende aanvragen, ziet u hier ook de huidige fase waarin de aanvraag is.

* **CancelRequest**: een vlag die aangeeft of de aanvraag is geannuleerd.

* **Voortgang**: een schatting van het percentage voltooid werk voor de bewerking. Een waarde van 50 geeft aan dat de bewerking ongeveer 50% voltooid is.

* **Details**: een XML-waarde die een meer gedetailleerde verslag. Het voortgangsverslag wordt periodiek bijgewerkt als sets van rijen worden gekopieerd van de bron naar het doel. Deze kolom bevat tevens meer gedetailleerde informatie over de fout in het geval van fouten of uitzonderingen.


### <a name="azure-diagnostics"></a>Azure diagnostische gegevens

De service gesplitste samenvoegen gebruikt Azure diagnostische gegevens op basis van Azure SDK 2.5 voor bewaking en diagnose. Besturing van de configuratie van diagnostische zoals hier uitgelegd: [Diagnostics inschakelen in Azure Cloud Services en virtuele Machines](../cloud-services/cloud-services-dotnet-diagnostics.md). Het downloadpakket bevat twee diagnostics configuraties: een voor de Webrol en een voor de rol van de werknemer. Deze configuraties diagnostische gegevens voor de service Volg de richtlijnen van de [Cloud Service Fundamentals in Microsoft Azure](https://code.msdn.microsoft.com/windowsazure/Cloud-Service-Fundamentals-4ca72649). Bevat de definities Meld prestatiemeteritems, IIS logboeken Windows-Logboeken en gebeurtenislogboeken toepassing gesplitste samenvoegen. 

## <a name="deploy-diagnostics"></a>Diagnostische gegevens implementeren 

Als u controle en diagnose met behulp van de diagnostische configuratie voor het web en werknemer rollen die door het pakket NuGet, voert u de volgende opdrachten met Azure PowerShell: 

    $storage_name = "<YourAzureStorageAccount>" 
    
    $key = "<YourAzureStorageAccountKey" 
    
    $storageContext = New-AzureStorageContext -StorageAccountName $storage_name -StorageAccountKey $key  
    
    
    $config_path = "<YourFilePath>\SplitMergeWebContent.diagnostics.xml" 
    
    $service_name = "<YourCloudServiceName>" 
    
    Set-AzureServiceDiagnosticsExtension -StorageContext $storageContext -DiagnosticsConfigurationPath $config_path -ServiceName $service_name -Slot Production -Role "SplitMergeWeb" 
    
    
    $config_path = "<YourFilePath>\SplitMergeWorkerContent.diagnostics.xml" 
    
    $service_name = "<YourCloudServiceName>" 
    
    Set-AzureServiceDiagnosticsExtension -StorageContext $storageContext -DiagnosticsConfigurationPath $config_path -ServiceName $service_name -Slot Production -Role "SplitMergeWorker" 

U vindt meer informatie over het configureren en implementeren van diagnostische instellingen: [Diagnostische gegevens inschakelen in Azure Cloud Services en virtuele Machines](../cloud-services/cloud-services-dotnet-diagnostics.md). 

## <a name="retrieve-diagnostics"></a>Diagnostische gegevens ophalen 

Gemakkelijk toegankelijk de diagnostische gegevens van Visual Studio Server Explorer in de Azure deel van de structuur van de Server Explorer. Een exemplaar van Visual Studio openen, en klik in de menubalk op Beeld en Server Explorer. Klik op het pictogram verbinding maken met uw abonnement Azure van Azure. Ga naar Azure -> opslag -> <your storage account> -> tabellen -> WADLogsTable. Zie [Opslagbronnen bladeren met Explorer Server](http://msdn.microsoft.com/library/azure/ff683677.aspx)voor meer informatie. 

![WADLogsTable][2]

Het WADLogsTable is gemarkeerd in de bovenstaande afbeelding bevat de gedetailleerde gegevens van gebeurtenissen in het toepassingslogboek van de gesplitste samenvoegen service. Houd er rekening mee dat de standaardconfiguratie van het gedownloade pakket op een productie-implementatie gericht is. Het interval waarmee logboeken en items worden opgehaald uit de exemplaren van de service is dus groot (5 minuten). Voor tests en ontwikkeling, het interval te verlagen door de instellingen van de diagnostische gegevens van het web of de rol van de werknemer aan uw behoeften aan te passen. Klik met de rechtermuisknop op de rol in Visual Studio Server Explorer (Zie boven) en pas vervolgens de Transfer periode in het dialoogvenster voor de configuratie-instellingen voor diagnostische gegevens: 

![Configuratie][3]


## <a name="performance"></a>Prestaties

In het algemeen kan betere prestaties worden verwacht van een hogere, meer zodat service tiers in Azure SQL-Database. Hogere i/o, CPU en geheugen toewijzingen voor de hogere niveaus van service profiteren van het massaal kopiëren en verwijderen van bewerkingen die de service gesplitste samenvoegen gebruikt. Daarom verhogen het niveau van de service alleen voor deze databases gedurende een bepaalde, beperkte tijd.

De service voert ook validatie query's als onderdeel van haar normale werkzaamheden. Deze validatie van query's controleren op onverwachte aanwezigheid van gegevens in het doelbereik en ervoor te zorgen dat elke bewerking splitsen/samenvoegen/verplaatsen van een consistente status wordt gestart. Deze alle query's werken via sharding belangrijkste bereiken gedefinieerd door het bereik van de bewerking en de grootte van de partij die deel uitmaken van de definitie van de aanvraag. Deze query's uitvoeren best als een index is aanwezig met de sleutel sharding als de eerste kolom. 

Een unieke eigenschap met de sleutel sharding als de eerste kolom wordt bovendien dat de service een geoptimaliseerde aanpak die verbruik van ruimte en geheugen beperkt gebruiken. Deze unieke eigenschap is vereist voor het verplaatsen van grote formaten (meestal meer dan 1GB). 

## <a name="how-to-upgrade"></a>Het upgraden

1. Volg de stappen in [een gesplitste samenvoegen service implementeren](sql-database-elastic-scale-configure-deploy-split-and-merge.md).
2. Het configuratiebestand voor cloud service voor de implementatie van de gesplitste samenvoegen aangepast aan de nieuwe configuratieparameters wijzigen. Een nieuwe verplichte parameter is de informatie over het certificaat dat wordt gebruikt voor codering. Een eenvoudige manier om dit te doen is de nieuwe sjabloonbestand van de download ten opzichte van de bestaande configuratie te vergelijken. Zorg ervoor dat u de instellingen voor "DataEncryptionPrimaryCertificateThumbprint" en "DataEncryptionPrimary" voor het web en de rol van werknemer toevoegen.
3. Voordat u de update implementeert op Azure, ervoor te zorgen dat alle lopende gesplitste samenvoegen bewerkingen zijn voltooid. U kunt dit eenvoudig doen door het uitvoeren van een query de tabellen RequestStatus en PendingWorkflows in de metagegevensdatabase splitsen samenvoegen voor lopende aanvragen.
4. Werk uw bestaande cloud service implementeren voor het samenvoegen van uw abonnement op Azure splitsen met het nieuwe pakket en het bestand met de bijgewerkte service.

U hoeft niet inrichten van een nieuwe metagegevensdatabase voor split-samenvoegen om te upgraden. De nieuwe versie zal uw bestaande metagegevensdatabase automatisch bijgewerkt naar de nieuwe versie. 

## <a name="best-practices--troubleshooting"></a>Beste praktijken en het oplossen van problemen
 
-    Een huurder test definiëren en uitoefenen van de belangrijkste verrichtingen splitsen/samenvoegen/verplaatsen met de huurder test via verschillende shards. Zorg ervoor dat alle metagegevens correct op uw kaart shard is gedefinieerd en dat de bewerkingen niet in strijd beperkingen of refererende sleutels zijn.
-    Houd de huurder test vraagstukken in verband met gegevensgrootte boven de maximale grootte van uw grootste huurder om ervoor te zorgen u gegevensgrootte niet doe. Zo kunt u een bovengrens van de tijd die nodig is voor het verplaatsen van een enkele huurder beoordelen. 
-    Zorg ervoor dat uw schema verwijderen kunt. De gesplitste samenvoegen service vereist de mogelijkheid om gegevens te verwijderen uit de bron shard eenmaal aan het doel is de gegevens gekopieerd. Bijvoorbeeld **triggers verwijderen** kunt voorkomen dat de service de gegevens op de broncomputer te verwijderen en kunnen er bewerkingen mislukken.
-    De sleutel sharding moet de eerste kolom in de primaire sleutel of unieke indexdefinitie. Dat zorgt voor de beste prestaties voor splitsen of samenvoegen validatie query's en de werkelijke gegevens beweging verwijdering bewerkingen en die altijd op de belangrijkste bereiken sharding werken.
-    Collocate uw service gesplitste samenvoegen in de regio en Datacenter waarin uw databases zijn opgeslagen. 

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]



<!--Anchors-->
<!--Image references-->
[1]:./media/sql-database-elastic-scale-overview-split-and-merge/split-merge-overview.png
[2]:./media/sql-database-elastic-scale-overview-split-and-merge/diagnostics.png
[3]:./media/sql-database-elastic-scale-overview-split-and-merge/diagnostics-config.png
 
