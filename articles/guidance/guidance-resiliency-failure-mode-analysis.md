<properties
   pageTitle="Modus analyse | Microsoft Azure"
   description="Richtlijnen voor het uitvoeren van analyse-modus voor cloud-oplossingen op basis van Azure."
   services=""
   documentationCenter="na"
   authors="MikeWasson"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="mwasson"/>

# <a name="azure-resiliency-guidance-failure-mode-analysis"></a>Azure resiliency richtlijnen: modus Foutanalyse

Fout is-modus (FMA) een proces voor het bouwen van tolerantie in een systeem, door het identificeren van mogelijke fouten in het systeem. De volledige Postbustoegang moeten deel uitmaken van de fasen architectuur en ontwerp zodat u foutherstel in het systeem vanaf het begin maken kunt.

Dit is het algemene proces uit te voeren van een volledige Postbustoegang: 

1. Alle onderdelen in het systeem identificeren. Externe afhankelijkheden, zoals als id-providers en services van andere leveranciers bevatten.   

2. Geef voor elk onderdeel, mogelijke storingen die kunnen optreden. Een enkelvoudige component mogelijk meer dan één storing modus. U moet bijvoorbeeld rekening houden met leesproblemen optreden en schrijffouten apart, omdat de impact en eventuele beperkende maatregelen verschillen.

3. Elke storingsconditie volgens de algemene risico beoordeling. Deze factoren rekening houden:  

    - Wat is de waarschijnlijkheid van de storing. Is het relatief veelvoorkomende? Zeldzame Extrememly? U hoeft geen exacte getallen; het doel is om de prioriteit te rangschikken.

    - Wat is de impact op de beschikbaarheid, verlies van gegevens, kosten en verstoring van de zakelijke toepassing? 

4. Voor elke storingsconditie bepalen hoe de toepassing reageert en herstellen. U kunt optimalisatie in kosten- en complexiteit.   

Als uitgangspunt voor uw volledige Postbustoegang proces bevat dit artikel een catalogus van potentiële storingen op te sporen en hun beperkingen. De catalogus is ingedeeld op technologie of Azure service, plus een algemene categorie voor het ontwerpen van op toepassingsniveau. De catalogus is niet volledig, maar omvat veel van de core Azure services. 


## <a name="app-service"></a>App-Service

### <a name="app-service-app-shuts-down"></a>App App-Service wordt afgesloten.

**Detectie**. Mogelijke oorzaken:

- Verwachte afsluiting
    
    - Een operator is afgesloten van de toepassing. bijvoorbeeld met behulp van de portal Azure.

    - De app is verwijderd omdat het niet actief is. (Alleen als de `Always On` is uitgeschakeld.)

- Onverwacht afsluiten

    - De toepassing loopt vast.

    - Een exemplaar van de VM voor App-Service niet meer beschikbaar.


Logboekregistratie van Application_End wordt de app domein afgesloten (crasht zachte proces) catch en is de enige manier om het domein toepassing afsluiten.

**Herstel**

- Als het afsluiten wordt verwacht, gebruikt u de gebeurtenis van de toepassing afsluiten. Gebruik bijvoorbeeld in ASP.NET, de `Application_End` methode.

- Als de toepassing verwijderd, terwijl niet-actief is, wordt deze automatisch opnieuw gestart bij de volgende aanvraag. U wordt echter de "koude start" kosten oplopen. 

- Inschakelen als u wilt voorkomen dat de toepassing uit het geheugen wordt verwijderd terwijl actief, de `Always On` instellen in de web app. [Configure web apps in Azure App-Service]Zie[app-service-configure].

- Instellen als u wilt voorkomen dat een exploitant de app afgesloten, een resource vergrendeling met `ReadOnly` niveau. Zie [bronnen vergrendelen met Azure Resource Manager][rm-locks].

- Als de toepassing vastloopt of een VM voor App-Service niet beschikbaar is, App-Service de toepassing opnieuw wordt gestart. 


**Diagnostische gegevens**. Logboeken voor toepassingen en web server logs. Zie [Diagnostische gegevens vastleggen voor web apps in Azure App-Service inschakelen][app-service-logging].


### <a name="a-particular-user-repeatedly-makes-bad-requests-or-overloads-the-system"></a>Een bepaalde gebruiker herhaaldelijk maakt ongeldige aanvragen of het systeem overloads. 

**Detectie**. Verificatie van gebruikers en gebruikers-ID in de toepassingslogboeken opnemen.

**Herstel**

- Beheer van [Azure API] [ api-management] gashendel aanvragen van de gebruiker. Zie [Geavanceerde aanvraag met Azure API beheer beperken][api-management-throttling]

- De gebruiker blokkeren.

**Diagnostische gegevens**. Alle verificatieaanvragen in logboek registreren.


### <a name="a-bad-update-was-deployed"></a>Een slechte update is geïmplementeerd.

**Detectie**. De toepassingsstatus via de Portal Azure controleren (Zie [Monitor Azure web app prestaties][app-insights-web-apps]) of de [gezondheid eindpunt monitoring patroon]implementeert[health-endpoint-monitoring-pattern].

**Herstel**. Gebruik meerdere [sleuven implementatie] [ app-service-slots] en terugkeren naar de laatst bekende juiste implementatie. Zie voor meer informatie, [Basic web application referentiearchitectuur][ra-web-apps-basic].


## <a name="azure-active-directory"></a>Azure Active Directory

### <a name="openid-connect-oidc-authentication-fails"></a>OpenID verbinden (OIDC)-verificatie mislukt.

**Detectie**. Mogelijke storingen op te sporen zijn:

1. Azure AD is niet beschikbaar of kan niet worden bereikt vanwege een netwerkprobleem. Omleiding naar het eindpunt van de verificatie mislukt en de middleware OIDC genereert een uitzondering.
2. Azure AD huurder bestaat niet. Omleiding naar het eindpunt van de verificatie wordt een HTTP-foutcode en de middleware OIDC genereert een uitzondering.
3. Gebruiker verifiëren niet. Geen detectie strategie is noodzakelijk; Azure AD mislukte aanmelding worden verwerkt.

**Herstel**

1. Niet-verwerkte uitzonderingen van de middleware variabel.
2. Verwerken `AuthenticationFailed` gebeurtenissen.
3. De gebruiker omleiden naar een foutpagina.
4. Pogingen van de gebruiker.


## <a name="azure-search"></a>Azure zoeken

### <a name="writing-data-to-azure-search-fails"></a>Gegevens schrijven naar Azure Search is mislukt.

**Detectie**. Catch `Microsoft.Rest.Azure.CloudException` fouten.

**Herstel**

De [SDK voor .NET zoeken] [ search-sdk] automatisch na tijdelijke mislukte pogingen. Eventuele uitzonderingen die door de client-SDK moeten worden behandeld als tijdelijke fouten.

Het standaardbeleid opnieuw gebruikt exponentiële terug uit. Voor het gebruik van een ander opnieuw beleid roept `SetRetryPolicy` op de `SearchIndexClient` of `SearchServiceClient` klasse. Voor meer informatie Zie [Automatische pogingen][auto-rest-client-retry].

**Diagnostische gegevens**. Gebruik [Zoeken verkeer Analytics][search-analytics].


### <a name="reading-data-from-azure-search-fails"></a>Gegevens lezen van Azure zoeken mislukt.

**Detectie**. Catch `Microsoft.Rest.Azure.CloudException` fouten.

**Herstel**

De [SDK voor .NET zoeken] [ search-sdk] automatisch na tijdelijke mislukte pogingen. Eventuele uitzonderingen die door de client-SDK moeten worden behandeld als tijdelijke fouten.

Het standaardbeleid opnieuw gebruikt exponentiële terug uit. Voor het gebruik van een ander opnieuw beleid roept `SetRetryPolicy` op de `SearchIndexClient` of `SearchServiceClient` klasse. Voor meer informatie Zie [Automatische pogingen][auto-rest-client-retry].

**Diagnostische gegevens**. Gebruik [Zoeken verkeer Analytics][search-analytics].



## <a name="cassandra"></a>Cassandra


### <a name="reading-or-writing-to-a-node-fails"></a>Lezen van of schrijven naar een knooppunt uitvalt.

**Detectie**. Werkelijk de uitzondering. .NET-clients, dit wordt doorgaans `System.Web.HttpException`. Andere client wellicht andere typen uitzonderingen.  Zie [foutafhandeling Cassandra gedaan rechts](http://www.datastax.com/dev/blog/cassandra-error-handling-done-right)voor meer informatie.

**Herstel**

- Elke [client Cassandra](https://wiki.apache.org/cassandra/ClientOptions) heeft een eigen beleid opnieuw en mogelijkheden. Zie voor meer informatie, [Cassandra foutafhandeling goed uitgevoerde][cassandra-error-handling].
- Een rack-aware implementatie met gegevensknooppunten verdeeld over de fout met betrekking tot domeinen gebruiken.
- Implementeren op meerdere gebieden met Lokaal quorum consistentie. Als een tijdelijke storing voordoet, niet via een andere regio.

**Diagnostische gegevens**. Toepassingslogboeken


## <a name="cloud-service"></a>Cloud-Service

### <a name="web-or-worker-roles-are-unexpectedlybeing-shut-down"></a>Web- of werknemer rollen worden onverwacht afgesloten.

**Detectie**. De [RoleEnvironment.Stopping] [ RoleEnvironment.Stopping] gebeurtenis wordt gestart.

**Herstel**. Opheffen van de [RoleEntryPoint.OnStop] [ RoleEntryPoint.OnStop] methode om op te schonen zonder problemen. Zie [De rechts kunt u Azure OnStop-gebeurtenissen afhandelen] voor meer informatie,[ onstop-events] (blog).


## <a name="documentdb"></a>DocumentDB

### <a name="reading-data-from-documentdb-fails"></a>Gegevens lezen van DocumentDB mislukt.

**Detectie**. Catch `System.Net.Http.HttpRequestException` of `Microsoft.Azure.Documents.DocumentClientException`. 

**Herstel**

- De SDK pogingen automatisch mislukte pogingen. Het aantal pogingen en de maximale wachttijd instellen, configureren `ConnectionPolicy.RetryOptions`. Uitzonderingen op dat de client bevinden zich buiten het beleid opnieuw of fouten van voorbijgaande aard zijn. 

- Als DocumentDB bandbreedte regelen voor de client, wordt er een fout 429 HTTP. Controleer de statuscode de `DocumentClientException`. Als uw foutbericht 429 consistent, kunt u overwegen verhoging van de doorvoercapaciteit van de DocumentDB-collectie.

- De DocumentDB-database te repliceren tussen twee of meer regio's. Alle replica's kunnen worden gelezen. Met de client-SDK's, geeft u de `PreferredLocations` parameter. Dit is een geordende lijst van Azure regio's. Alle leesbewerkingen wordt verzonden naar de eerste beschikbare regio in de lijst. Als de aanvraag is mislukt, probeert de client de andere regio's in de lijst in volgorde. Voor meer informatie Zie [Developing with accounts voor meerdere regio DocumentDB][docdb-multi-region].

**Diagnostische gegevens**. Meld alle fouten op de client.


### <a name="writing-data-to-documentdb-fails"></a>Gegevens schrijven naar DocumentDB mislukt.

**Detectie**. Catch `System.Net.Http.HttpRequestException` of `Microsoft.Azure.Documents.DocumentClientException`. 

**Herstel**

- De SDK pogingen automatisch mislukte pogingen. Het aantal pogingen en de maximale wachttijd instellen, configureren `ConnectionPolicy.RetryOptions`. Uitzonderingen op dat de client bevinden zich buiten het beleid opnieuw of fouten van voorbijgaande aard zijn. 

- Als DocumentDB bandbreedte regelen voor de client, wordt er een fout 429 HTTP. Controleer de statuscode de `DocumentClientException`. Als uw foutbericht 429 consistent, kunt u overwegen verhoging van de doorvoercapaciteit van de DocumentDB-collectie.

- De DocumentDB-database te repliceren tussen twee of meer regio's. Als de primaire regio mislukt, een andere regio zullen worden gepromoveerd tot schrijven. U kunt ook handmatig een failover activeren. De SDK bevat automatische detectie en routering, zodat de toepassingscode blijft werken nadat failover is uitgevoerd. Tijdens de failover-periode (meestal minuten) hebben schrijfbewerkingen van hogere latentie, zoals de SDK vindt u het nieuwe gebied van schrijven. Voor meer informatie Zie [Developing with accounts voor meerdere regio DocumentDB][docdb-multi-region].

- Als een terugval, behouden het document naar een wachtrij voor back-up en de wachtrij later verwerken.

**Diagnostische gegevens**. Meld alle fouten op de client.


## <a name="elasticsearch"></a>Elasticsearch

### <a name="reading-data-from-elasticsearch-fails"></a>Gegevens lezen van Elasticsearch mislukt.

**Detectie**. De toepasselijke uitzondering voor de desbetreffende [client Elasticsearch] catch[ elasticsearch-client] wordt gebruikt. 

**Herstel**

- Een mechanisme opnieuw gebruiken. Elke client heeft een eigen beleid opnieuw. 

- Meerdere Elasticsearch knooppunten implementeren en gebruiken van replicatie voor hoge beschikbaarheid.

Zie voor meer informatie [Elasticsearch uitgevoerd op Azure][elasticsearch-azure].

**Diagnostische gegevens**. U kunt de controleprogramma's gebruiken voor Elasticsearch of meld alle fouten op de client met de nettolading. Zie de sectie 'Controleren' in [Elasticsearch uitgevoerd op Azure][elasticsearch-azure].


### <a name="writing-data-to-elasticsearch-fails"></a>Gegevens schrijven naar Elasticsearch mislukt.

**Detectie**. De toepasselijke uitzondering voor de desbetreffende [client Elasticsearch] catch[ elasticsearch-client] wordt gebruikt.  

**Herstel**

- Een mechanisme opnieuw gebruiken. Elke client heeft een eigen beleid opnieuw. 
 
- Als de toepassing een verminderde consistentie niveau tolereren kan, kunt u het schrijven met `write_consistency` van `quorum`.

Zie voor meer informatie [Elasticsearch uitgevoerd op Azure][elasticsearch-azure].

**Diagnostische gegevens**. U kunt de controleprogramma's gebruiken voor Elasticsearch of meld alle fouten op de client met de nettolading. Zie de sectie 'Controleren' in [Elasticsearch uitgevoerd op Azure][elasticsearch-azure].


## <a name="queue-storage"></a>Wachtrij-opslag

### <a name="writing-a-message-to-azure-queue-storage-fails-consistently"></a>Schrijven van een bericht naar Azure wachtrij opslag mislukt voortdurend.

**Detectie**. Na *N* aantal pogingen, mislukt het schrijven nog steeds.

**Herstel**

- De gegevens worden opgeslagen in een lokale cache en schrijft naar opslag later doorsturen zodra de service beschikbaar.
- Een secundaire wachtrij maken en schrijven naar die wachtrij als de primaire wachtrij niet beschikbaar is.

**Diagnostische gegevens**. [Maatstelsel van opslag]gebruiken voor[storage-metrics].


### <a name="the-application-cannot-process-a-particular-message-from-the-queue"></a>De toepassing kan een bepaald bericht in de wachtrij niet verwerken. 

**Detectie**. Specifieke toepassing. Bijvoorbeeld: het bericht bevat ongeldige gegevens of de bedrijfslogica voor een of andere reden mislukt. 

**Herstel**

Het bericht verplaatsen naar een afzonderlijke wachtrij. Uitgevoerd in een afzonderlijk proces te onderzoeken van de berichten in die wachtrij.

Overweeg het gebruik van wachtrijen Azure Service Bus berichten waar u een [wachtrij voor onbestelbare berichten] [ sb-dead-letter-queue] functionaliteit voor dit doel.

Opmerking: Als u opslag wachtrijen met WebJobs, de WebJobs SDK biedt ingebouwde verontreinigd bericht verwerken. Zie [How to use Azure wachtrij opslag met de SDK WebJobs][sb-poison-message].

**Diagnostische gegevens**. Gebruik de logboekregistratie van toepassing.


## <a name="redis-cache"></a>Cache bestand Vgx.

### <a name="reading-from-the-cache-fails"></a>Het lezen van de cache mislukt.

**Detectie**. Catch `StackExchange.Redis.RedisConnectionException`.

**Herstel**

1. Probeer opnieuw op fouten van voorbijgaande aard. Azure cache bestand Vgx. ondersteunt ingebouwde opnieuw via Zie [bestand Vgx. Cache opnieuw richtlijnen][redis-retry].
2. Tijdelijke fouten behandelen als een cache-missers en terugschakelen naar de oorspronkelijke gegevensbron.

**Diagnostische gegevens**. Gebruik van [Cache bestand Vgx. diagnostische gegevens][redis-monitor].


### <a name="writing-to-the-cache-fails"></a>Schrijven naar de cache mislukt.

**Detectie**. Catch `StackExchange.Redis.RedisConnectionException`.

**Herstel**

1. Probeer opnieuw op fouten van voorbijgaande aard. Azure cache bestand Vgx. ondersteunt ingebouwde opnieuw via Zie [bestand Vgx. Cache opnieuw richtlijnen][redis-retry].
2. Als de fout is een tijdelijke, negeren en andere transacties die later naar de cache geschreven.

**Diagnostische gegevens**. Gebruik van [Cache bestand Vgx. diagnostische gegevens][redis-monitor].


## <a name="sql-database"></a>SQL-Database

### <a name="cannot-connect-to-the-database-in-the-primary-region"></a>Kan geen verbinding maken met de database in de primaire regio.

**Detectie**. Verbinding mislukt.

**Herstel**

Let op: De database moet worden geconfigureerd voor actieve geo-replicatie. [SQL-Database actief Geo-replicatie]Zie[sql-db-replication].

- Lezen van een secundaire replica voor query's. 
- Voor INSERT en update handmatig failover uitvoeren naar een secundaire replica. Zie [een failover- of niet gepland voor Azure SQL-Database][sql-db-failover].

De replica gebruikt een andere verbindingsreeks, dus moet u de verbindingsreeks in de toepassing bijwerken.


### <a name="client-runs-out-of-connections-in-the-connection-pool"></a>Client wordt uitgevoerd bij verbindingen in de groep.

**Detectie**. Catch `System.InvalidOperationException` fouten. 

**Herstel**

- Probeer de bewerking opnieuw.
- Als een plan voor risicobeperking, isoleren, de verbinding van toepassingen voor elke use-case, zodat een use-case kan niet alle verbindingen domineren.
- Verhoog de maximale verbindingssnelheid van toepassingen.

**Diagnostische gegevens**. Logboeken voor toepassingen.


### <a name="database-connection-limit-is-reached"></a>Database is verbinding bereikt. 

**Detectie**. Azure SQL-Database beperkt het aantal gelijktijdige werknemers, aanmeldingen en sessies. De grenzen zijn afhankelijk van het niveau van de service. Zie voor meer informatie, [Azure SQL Database resource limieten][sql-db-limits].

Deze fouten worden opgespoord, catch `System.Data.SqlClient.SqlException` en de waarde van `SqlException.Number` voor de SQL-foutcode. Zie voor een lijst van relevante foutcodes [foutcodes van de SQL voor SQL-Database-clienttoepassingen: verbindingsfout en andere problemen][sql-db-errors].

**Herstel**. Deze fouten worden beschouwd als tijdelijk, zodat opnieuw wordt geprobeerd het probleem mogelijk opgelost. Als u deze fouten voortdurend raakt, kunt u het schalen van de database.

**Diagnostische gegevens**. -De [sys.event_log] [ sys.event_log] query resulteert in succesvolle databaseverbindingen verbindingsfouten en impasses.

- Maak een [waarschuwingsregel] [ azure-alerts] mislukt voor verbindingen.

- Inschakelen van [controle voor SQL-Database] [ sql-db-audit] en controle op mislukte aanmeldingen.


## <a name="service-bus-messaging"></a>Bus Messaging service

### <a name="reading-a-message-from-a-service-bus-queue-fails"></a>Lezen van een bericht uit de wachtrij van een Bus-Service mislukt.

**Detectie**. Uitzonderingen op de client-SDK variabel. De basisklasse voor Service Bus uitzonderingen is [MessagingException][sb-messagingexception-class]. Als de fout van voorbijgaande aard is, is de `IsTransient` de eigenschap is ingesteld op true. 

Voor meer informatie Zie [Service Bus berichten uitzonderingen][sb-messaging-exceptions].

**Herstel**

1. Probeer opnieuw op fouten van voorbijgaande aard. Zie [Service Bus opnieuw richtlijnen][sb-retry].

2. Berichten die aan de ontvanger kunnen worden afgeleverd in een *wachtrij voor onbestelbare berichten*geplaatst. Gebruik deze wachtrij om te zien welke berichten worden ontvangen. Er is geen automatische opschoning van de wachtrij voor onbestelbare berichten. Berichten blijven daar totdat u ze expliciet ophaalt. Zie [overzicht van Service Bus onbestelbare transactieberichten][sb-dead-letter-queue].


### <a name="writing-a-message-to-a-service-bus-queue-fails"></a>Een bericht schrijven aan een Service Bus mislukt wachtrij.

**Detectie**. Uitzonderingen op de client-SDK variabel. De basisklasse voor Service Bus uitzonderingen is [MessagingException][sb-messagingexception-class]. Als de fout van voorbijgaande aard is, is de `IsTransient` de eigenschap is ingesteld op true. 

Voor meer informatie Zie [Service Bus berichten uitzonderingen][sb-messaging-exceptions].

**Herstel**

1. De client Service Bus pogingen automatisch na fouten van voorbijgaande aard. Standaard gebruikt het exponentiële terug uit. Na het maximale aantal nieuwe pogingen of de maximale time-out genereert de client een uitzondering. Zie voor meer informatie, [Service Bus opnieuw richtlijnen][sb-retry].

2. Als het wachtrijquotum wordt overschreden, de client genereert [QuotaExceededException][QuotaExceededException]. Het uitzonderingsbericht bevat meer details. Sommige berichten uit de wachtrij te corrigeren voordat opnieuw wordt geprobeerd en het patroon stroomonderbreker overwegen om te voorkomen dat verdere pogingen terwijl het quotum wordt overschreden. Zorg ervoor dat de eigenschap [BrokeredMessage.TimeToLive] niet te hoog is ingesteld. 

3. In een gebied, tolerantie kan worden verbeterd met behulp van [gepartitioneerde wachtrijen of onderwerpen][sb-partition]. Een wachtrij voor niet-gepartitioneerde of onderwerp is toegewezen aan een berichtenarchief. Als dit berichtenarchief niet beschikbaar is, niet alle bewerkingen op de desbetreffende wachtrij of onderwerp. Een gepartitioneerde wachtrij of onderwerp is gepartitioneerd in meerdere messaging winkels. 

4. Voor meer tolerantie, twee Service Bus naamruimten maken in verschillende regio's en de berichten worden gerepliceerd. U kunt replicatie van actieve of passieve replicatie.

    - Active-replicatie: de client stuurt elk bericht dat naar beide wachtrijen. De ontvanger luistert naar beide wachtrijen. Berichten met een unieke id-code zodat de client dubbele berichten kunt spelen.

    - Passieve replicatie: de client verzendt het bericht naar één wachtrij. Als er een fout optreedt, schakelt de client terug naar de andere wachtrij. De ontvanger luistert naar beide wachtrijen. Deze benadering vermindert het aantal dubbele berichten die worden verzonden. De ontvanger moet echter nog steeds dubbele berichten verwerken.

    Zie voor meer informatie [GeoReplication monster] [ sb-georeplication-sample] en [Best practices voor isolerende toepassingen tegen Bus Service storingen en calamiteiten][sb-outages].


### <a name="duplicate-message"></a>Dubbel bericht.

**Detectie**. Bekijk de `MessageId` en `DeliveryCount` eigenschappen van het bericht.

**Herstel**

- Ontwerp, indien mogelijk, uw bericht verwerkingen idempotency is ingeschakeld. Anders slaat message-id's van berichten die al zijn verwerkt en de ID controleren voordat een bericht wordt verwerkt.

- Detectie van dubbele inschakelen door het maken van de wachtrij met de `RequiresDuplicateDetection` is ingesteld op true. Met deze instelling verwijdert Service Bus automatisch elk bericht dat wordt verzonden met dezelfde `MessageId` als een vorige bericht.  Let op het volgende:

    -  Deze instelling voorkomt dat er dubbele berichten worden opgeslagen in de wachtrij. Het voorkomen niet dat een ontvanger met het verwerken van hetzelfde bericht meerdere keren.

    - Controle op dubbele heeft een tijdvenster. Als een duplicaat wordt verzonden na dit venster, won't worden gedetecteerd.

**Diagnostische gegevens**. Gedupliceerde berichten vastleggen in logboek.


### <a name="the-application-cannot-process-a-particular-message-from-the-queue"></a>De toepassing kan een bepaald bericht in de wachtrij niet verwerken. 

**Detectie**. Specifieke toepassing. Bijvoorbeeld: het bericht bevat ongeldige gegevens of de bedrijfslogica voor een of andere reden mislukt. 

**Herstel**

Er zijn twee storingsmodi te overwegen. 

- De ontvanger detecteert de storing. In dit geval wordt het bericht verplaatsen naar de wachtrij voor onbestelbare berichten. Een afzonderlijk proces te onderzoeken van de berichten in de wachtrij voor onbestelbare berichten later uitvoeren.

- De ontvanger-fout tijdens het verwerken van bericht &mdash; bijvoorbeeld als gevolg van een niet-afgehandelde uitzondering. Om in dit geval gebruikt u `PeekLock` modus. In deze modus als de vergrendeling is verlopen, beschikbaar het bericht voor andere ontvangers. Als het bericht de maximale levering telling of de time-to-live overschrijdt, wordt het bericht automatisch verplaatst naar de wachtrij voor onbestelbare berichten.

Zie [overzicht van Service Bus onbestelbare berichten]voor meer informatie[sb-dead-letter-queue].

**Diagnostische gegevens**. Wanneer de toepassing een bericht naar de wachtrij voor onbestelbare berichten wordt verplaatst, wordt een gebeurtenis schrijven in de toepassingslogboeken.


## <a name="service-fabric"></a>Service-Fabric

### <a name="a-request-to-a-service-fails"></a>Een aanvraag naar een service mislukt.

**Detectie**. De service heeft een fout geretourneerd.

**Herstel**

- Ga opnieuw naar een proxy (`ServiceProxy` of `ActorProxy`) en de service/actor-methode opnieuw aanroept. 

- **Stateful-service**. Bewerkingen op betrouwbare collecties in een transactie laten teruglopen. Als er een fout optreedt, wordt ook de transactie teruggedraaid. De aanvraag, zal als uit een wachtrij opgehaald opnieuw worden verwerkt. 
 
- **Stateless service**. Als de service gegevens naar een externe opslag aanhoudt, moeten alle bewerkingen idempotency is ingeschakeld.


**Diagnostische gegevens**. Toepassingslogboek

### <a name="service-fabric-node-is-shut-down"></a>Service Fabric knooppunt is afgesloten.

**Detectie**. Een token annulering wordt doorgegeven aan de service `RunAsync` methode. Fabric-service, wordt de taak geannuleerd voordat u het knooppunt afsluit.

**Herstel**. De annulering token gebruiken voor het detecteren van afsluiten. Als Service Fabric annulering wordt aangevraagd, geen werk te voltooien en af te sluiten `RunAsync` zo snel mogelijk.

**Diagnostische gegevens**. Toepassingslogboeken


## <a name="storage"></a>Opslag

### <a name="writing-data-to-azure-storage-fails"></a>Gegevens schrijven naar Azure opslag niet

**Detectie**. De client ontvangt de fouten bij het schrijven.

**Herstel**

1. Probeer het opnieuw, tijdelijke foutcorrectie. Het [beleid opnieuw] [ Storage.RetryPolicies] in de client SDK doet dit automatisch.
2. De stroomonderbreker patroon om te voorkomen dat overweldigend opslag implementeren.
3. Als N nieuwe pogingen mislukken, voert u een correcte terugval. Bijvoorbeeld:

    - De gegevens worden opgeslagen in een lokale cache en schrijft naar opslag later doorsturen zodra de service beschikbaar.
    - Als de actie schrijven in een transactionele bereik is, compenseren de transactie.

**Diagnostische gegevens**. [Maatstelsel van opslag]gebruiken voor[storage-metrics].


### <a name="reading-data-from-azure-storage-fails"></a>Het lezen van gegevens uit de opslag van Azure is mislukt.

**Detectie**. De client ontvangt de fouten bij het lezen.

**Herstel**

1. Probeer het opnieuw, tijdelijke foutcorrectie. Het [beleid opnieuw] [ Storage.RetryPolicies] in de client SDK doet dit automatisch.
2. Probeer het lezen van het secundaire eindpunt voor RA GRS opslag, als het lezen van het primaire eindpunt mislukt. De client-SDK kan dit automatisch worden verwerkt. Zie [replicatie Azure opslag][storage-replication].
3. Als *N* nieuwe pogingen mislukken, doen een terugval verslechteren zonder problemen. Als u een productafbeelding kan niet worden opgehaald uit de opslag, bijvoorbeeld een algemene aanduiding voor de afbeelding weergegeven.

**Diagnostische gegevens**. [Maatstelsel van opslag]gebruiken voor[storage-metrics].


## <a name="virtual-machine"></a>Virtuele Machine

### <a name="connection-to-a-backend-vm-fails"></a>Verbinding met een back-end VM werkt niet.

**Detectie**. Fouten.

**Herstel**

- Ten minste twee back-end VMs in een set van beschikbaarheid, achter een load balancer implementeren.

- Als de verbindingsfout is een tijdelijke, soms TCP wordt met succes opnieuw geprobeerd het bericht te verzenden. 

- Implementeer een beleid opnieuw in de toepassing. 

- De permanente of tijdelijke fouten, implementeren van de [stroomonderbreker] [ circuit-breaker] patroon.

- Als de aanroepende VM de egress netwerklimiet overschrijdt, wordt de wachtrij voor uitgaande items omhoog doorvoeren. Als de uitgaande wachtrij consequent vol is, kunt u horizontaal schalen. 

**Diagnostische gegevens**. De gebeurtenissen op de grenzen van services.

### <a name="vm-instance-becomes-unavailable-or-unhealthy"></a>VM-exemplaar wordt niet beschikbaar of is beschadigd.

**Detectie**. Configureren van een Load Balancer [gezondheid sonde] [ lb-probe] die geeft aan of de VM-exemplaar in orde is. De sonde moet worden gecontroleerd of essentiële functies correct reageert. 

**Herstel**. Voor elke toepassingslaag van de meerdere exemplaren van de VM plaatsen in dezelfde set beschikbaarheid en een load balancer voor de VMs plaats. Als de sonde gezondheid mislukt, reageert de Load Balancer verzenden van nieuwe verbindingen naar het beschadigde exemplaar.

**Diagnostische gegevens**. -Gebruik Load Balancer [logboek analytics][lb-monitor].
- Configureer uw systeem van toezicht voor het controleren van alle van de controle van de eindpunten van de gezondheid.


### <a name="operator-accidentally-shuts-down-a-vm"></a>Operator afgesloten per ongeluk een VM.

**Detectie**. N.V.T.

**Herstel**. Instellen van een resource vergrendeling met `ReadOnly` niveau. Zie [bronnen vergrendelen met Azure Resource Manager][rm-locks].

**Diagnostische gegevens**. Gebruik [Logboeken Azure activiteit][azure-activity-logs].


## <a name="webjobs"></a>WebJobs

### <a name="continuous-job-stops-running-when-the-scm-host-is-idle"></a>Doorlopende taak beëindigd wanneer de SCM-host niet actief is.

**Detectie**. Een token annulering doorgeven aan de functie WebJob. Zie voor meer informatie de [correcte afsluiten][web-jobs-shutdown]. 

**Herstel**. Schakel de `Always On` instellen in de web app. Zie voor meer informatie de [achtergrondtaken uitvoeren met WebJobs][web-jobs].


## <a name="application-design"></a>Ontwerp van toepassingen

### <a name="application-cant-handle-a-spike-in-incoming-requests"></a>Toepassing niet een knelpunt in het binnenkomende aanvragen te verwerken.

**Detectie**. Afhankelijk van de toepassing. Typische symptomen:

- De website wordt HTTP-5xx-foutcodes te retourneren.
- Afhankelijke services, zoals een database of opslag, start aanvragen te beperken. Kijk voor HTTP-fouten, zoals HTTP-429 (te veel aanvragen), afhankelijk van de service.
- Lengte van wachtrij voor HTTP groeit.

**Herstel**

- Schaalt naar grotere belasting. 

- Het risico van fouten niet hoeft trapsgewijze fouten verstoren de hele toepassing. Risicobeperkende strategieën omvatten:

    - Implementatie van het [Patroon beperken] [ throttling-pattern] om te voorkomen dat overweldigend back-end systemen.
    - Gebruik [op basis van wachtrijen laden herverdeling] [ queue-based-load-leveling] buffer aanvragen en deze verwerken in een geschikt tempo.
    - Prioriteit gegeven aan bepaalde clients. Als de toepassing gratis en betaalde niveaus is, beperken bijvoorbeeld klanten op de vrij laag, maar niet betaalde klanten. Zie [prioriteit wachtrij patroon][priority-queue-pattern].

**Diagnostische gegevens**. [App Service Diagnostische logboekregistratie]gebruiken[app-service-logging]. Een service zoals [Azure logboek Analytics]gebruiken[azure-log-analytics], [Inzichten toepassing][app-insights], of [Nieuwe Relic] [ new-relic] om te begrijpen wat de diagnostische logboeken.


### <a name="one-of-the-operations-in-a-workflow-or-distributed-transaction-fails"></a>Een van de bewerkingen in een werkstroom of een gedistribueerde transactie mislukt.

**Detectie**. Na *N* aantal pogingen, het nog steeds niet.

**Herstel**

- Als een plan voor risicobeperking implementeren de [Scheduler Agent Supervisor] [ scheduler-agent-supervisor] patroon voor het beheren van de gehele werkstroom. 
- Niet opnieuw op de time-outs. Er is een laag succespercentage voor deze fout. 
- Wachtrij werk om het later opnieuw proberen.

**Diagnostische gegevens**. Alle bewerkingen (geslaagd of mislukt), met inbegrip van de veredelingsprodukten acties vastleggen in logboek. Correlatie-id's gebruiken zodat u alle bewerkingen in dezelfde transactie kunt bijhouden.


### <a name="a-call-to-a-remote-service-fails"></a>Een aanroep naar een externe service mislukt.

**Detectie**. HTTP-foutcode.

**Herstel**

1. Probeer opnieuw op fouten van voorbijgaande aard. 
2. Als de aanroep mislukt na *N* probeert, een terugval actie ondernemen. (Toepassing specifieke).
3. Het [patroon van de stroomonderbreker] implementeren[ circuit-breaker] trapsgewijze fouten voorkomen. 

**Diagnostische gegevens**. Alle externe aanroep fouten melden.


## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het proces van volledige Postbustoegang [veerkracht voor cloud services] [ resilience-by-design-pdf] (PDF-download).

<!-- links -->

[api-management]: https://azure.microsoft.com/documentation/services/api-management/
[api-management-throttling]: ../api-management/api-management-sample-flexible-throttling.md
[app-insights]: ../application-insights/app-insights-overview.md
[app-insights-web-apps]: ../application-insights/app-insights-azure-web-apps.md
[app-service-configure]: ../app-service-web/web-sites-configure.md
[app-service-logging]: ../app-service-web/web-sites-enable-diagnostic-log.md
[app-service-slots]: ../app-service-web/web-sites-staged-publishing.md
[auto-rest-client-retry]: https://github.com/Azure/autorest/blob/master/Documentation/clients-retry.md
[azure-activity-logs]: ../monitoring-and-diagnostics/monitoring-overview-activity-logs.md
[azure-alerts]: ../monitoring-and-diagnostics/insights-alerts-portal.md
[azure-log-analytics]: ../log-analytics/log-analytics-overview.md
[BrokeredMessage.TimeToLive]: https://msdn.microsoft.com/library/microsoft.servicebus.messaging.brokeredmessage.timetolive.aspx
[cassandra-error-handling]: http://www.datastax.com/dev/blog/cassandra-error-handling-done-right
[circuit-breaker]: https://msdn.microsoft.com/library/dn589784.aspx
[docdb-multi-region]: ../documentdb/documentdb-developing-with-multiple-regions.md
[elasticsearch-azure]: guidance-elasticsearch-running-on-azure.md
[elasticsearch-client]: https://www.elastic.co/guide/en/elasticsearch/client/index.html
[health-endpoint-monitoring-pattern]: https://msdn.microsoft.com/library/dn589789.aspx
[onstop-events]: https://azure.microsoft.com/blog/the-right-way-to-handle-azure-onstop-events/
[lb-monitor]: ../load-balancer/load-balancer-monitor-log.md
[lb-probe]: ../load-balancer/load-balancer-custom-probe-overview.md#learn-about-the-types-of-probes
[new-relic]: https://newrelic.com/
[priority-queue-pattern]: https://msdn.microsoft.com/library/dn589794.aspx
[queue-based-load-leveling]: https://msdn.microsoft.com/library/dn589783.aspx
[QuotaExceededException]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.quotaexceededexception.aspx
[ra-web-apps-basic]: guidance-web-apps-basic.md
[redis-monitor]: ../redis-cache/cache-how-to-monitor.md
[redis-retry]: ../best-practices-retry-service-specific.md#cache-redis-retry-guidelines
[resilience-by-design-pdf]: http://download.microsoft.com/download/D/8/C/D8C599A4-4E8A-49BF-80EE-FE35F49B914D/Resilience_by_Design_for_Cloud_Services_White_Paper.pdf
[RoleEntryPoint.OnStop]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstop.aspx
[RoleEnvironment.Stopping]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.stopping.aspx
[rm-locks]: ../resource-group-lock-resources.md
[sb-dead-letter-queue]: ../service-bus-messaging/service-bus-dead-letter-queues.md
[sb-georeplication-sample]: https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/GeoReplication
[sb-messagingexception-class]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingexception.aspx
[sb-messaging-exceptions]: ../service-bus-messaging/service-bus-messaging-exceptions.md
[sb-outages]: ../service-bus/service-bus-outages-disasters.md#protecting-queues-and-topics-against-datacenter-outages-or-disasters
[sb-partition]: ../service-bus-messaging/service-bus-partitioning.md
[sb-poison-message]: ../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md#poison
[sb-retry]: ../best-practices-retry-service-specific.md#service-bus-retry-guidelines
[search-sdk]: https://msdn.microsoft.com/library/dn951165.aspx
[scheduler-agent-supervisor]: https://msdn.microsoft.com/library/dn589780.aspx
[search-analytics]: ../search/search-traffic-analytics.md
[sql-db-audit]: ../sql-database/sql-database-auditing-get-started.md
[sql-db-errors]: ../sql-database/sql-database-develop-error-messages.md#resource-governanance-errors
[sql-db-failover]: ../sql-database/sql-database-geo-replication-failover-portal.md
[sql-db-limits]: ../sql-database/sql-database-resource-limits.md
[sql-db-replication]: ../sql-database/sql-database-geo-replication-overview.md
[storage-metrics]: https://msdn.microsoft.com/library/dn782843.aspx
[storage-replication]: ../storage/storage-redundancy.md
[Storage.RetryPolicies]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.retrypolicies.aspx
[sys.event_log]: https://msdn.microsoft.com/library/dn270018.aspx
[throttling-pattern]: https://msdn.microsoft.com/library/dn589798.aspx
[web-jobs]: ../app-service-web/web-sites-create-web-jobs.md
[web-jobs-shutdown]: ../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md#graceful

