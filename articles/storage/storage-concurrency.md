<properties
    pageTitle="Gelijktijdigheid in Microsoft Azure opslag beheren"
    description="Het beheren van gelijktijdige uitvoering van de diensten van de Blob, wachtrij, tabel en bestand"
    services="storage"
    documentationCenter=""
    authors="jasonnewyork"
    manager="tadb"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="jahogg"/>

# <a name="managing-concurrency-in-microsoft-azure-storage"></a>Gelijktijdigheid in Microsoft Azure opslag beheren

## <a name="overview"></a>Overzicht

Moderne Internet gebaseerde toepassingen hebben meestal meerdere gebruikers weergeven en bijwerken van gegevens tegelijk. Hiervoor moeten toepassingsontwikkelaars zorgvuldig nadenken over hoe u een voorspelbare ervaring aan de eindgebruikers, met name voor scenario's waarin meerdere gebruikers dezelfde gegevens kunnen bijwerken. Er zijn drie belangrijke gegevens gelijktijdigheidsstrategieën ontwikkelaars worden meestal beschouwd als:  


1.  Optimistische gelijktijdigheid: een toepassing uitvoeren van die een update als onderdeel van de update verifiëren wordt of de gegevens is gewijzigd sinds de toepassing die gegevens voor het laatst gelezen. Bijvoorbeeld, als twee gebruikers een wiki-pagina een update naar dezelfde pagina vervolgens het wiki-platform moet ervoor zorgen dat de tweede update de eerste update – niet overschreven en dat beide gebruikers weten of de update is gelukt. Deze strategie wordt meestal gebruikt in webtoepassingen.
2.  Volledige vergrendeling: een toepassing die op zoek zijn voor het uitvoeren van een update duurt een vergrendeling voor een object dat wordt voorkomen dat andere gebruikers de gegevens bij te werken totdat de vergrendeling wordt opgeheven. Bijvoorbeeld in een master/slave data replicatiescenario waar alleen het model updates uitvoeren wordt de kapitein meestal houdt een exclusieve vergrendeling voor een langere tijd op de gegevens zodat niemand anders deze kan bijwerken.
3.  Laatste schrijver wins – een aanpak waarmee bewerkingen om verder te gaan zonder te controleren of een andere toepassing heeft bijgewerkt de gegevens sinds de toepassing eerst de gegevens lezen. Deze strategie (of het ontbreken van een formele strategie) wordt gewoonlijk gebruikt wanneer gegevens in een zodanige wijze dat er geen kans dat meerdere gebruikers toegang hebben tot dezelfde gegevens is gepartitioneerd. Het kan ook nuttig zijn indien tijdelijk gegevensstromen worden verwerkt.  

Dit artikel bevat een overzicht van hoe de opslag van Azure platform vereenvoudigt de ontwikkeling eerste-klas door ondersteuning te bieden voor alle drie van deze gelijktijdigheidsstrategieën.  

## <a name="azure-storage--simplifies-cloud-development"></a>Azure opslag – vereenvoudigt de ontwikkeling van de Cloud
De Azure storage-service ondersteunt alle drie strategieën, hoewel het onderscheidend in de mogelijkheid te bieden volledige ondersteuning voor de optimistische en Pessimistische gelijktijdigheid omdat het is ontworpen om in te spelen, een sterke samenhang model waarborgt dat wanneer de Storage-service een gegevens invoegen voert of updatebewerking alle verdere toegang heeft tot aan gegevens ziet de meest recente update. Opslagplatforms die gebruikmaken van een uiteindelijke algehele consistentie model hebben een vertraging tussen wanneer een schrijfbewerking wordt uitgevoerd door één gebruiker en wanneer de bijgewerkte gegevens zijn zichtbaar voor andere gebruikers dus complicerende ontwikkeling van toepassingen om te voorkomen dat de inconsistente eindgebruikers kunnen beïnvloeden.  

Naast het selecteren van een geschikte gelijktijdigheidsstrategie moeten ontwikkelaars zich bewust zijn van hoe een platform opslag wijzigingen – met name wijzigingen aan hetzelfde object over transacties geïsoleerd. Snapshot-isolatie de Azure storage-service gebruikt om leesbewerkingen plaatsvinden als de schrijfbewerkingen binnen een enkele partitie. In tegenstelling tot andere isolatieniveaus garandeert snapshot-isolatie dat alle leesbewerkingen een momentopname van de gegevens van een consistente zien terwijl updates optreden – in feite door het retourneren van het laatste vastgelegde waarden, terwijl een update transactie wordt verwerkt.  

## <a name="managing-concurrency-in-blob-storage"></a>Beheren van gelijktijdige uitvoering in Blob-opslag
U kunt kiezen voor het gebruik van beide modellen Optimistisch of Pessimistisch gelijktijdige toegang tot BLOB's en containers in de blob-service beheren. Als u een strategie laatste schrijft wins niet expliciet opgegeven is de standaardwaarde.  

### <a name="optimistic-concurrency-for-blobs-and-containers"></a>Optimistische gelijktijdigheid voor BLOB's en containers  

Een id wijst de Storage-service aan elk object opgeslagen. Deze id wordt bijgewerkt telkens wanneer een updatebewerking wordt uitgevoerd op een object. De id wordt geretourneerd naar de client als onderdeel van een HTTP GET-antwoord de header van ETag (eenheidtag) die is gedefinieerd in het HTTP-protocol gebruiken. Een gebruiker een update uitvoeren op een dergelijk object verzenden in het oorspronkelijke ETag samen met een voorwaardelijke kop om ervoor te zorgen dat een update vindt alleen plaats als een bepaalde voorwaarde is voldaan – in dit geval de voorwaarde is voor een 'If-Match'-header waarvoor de Storage-Service om de waarde van de ETag is opgegeven in de aanvraag voor een update die worden opgeslagen in de Storage-Service.  

Het overzicht van dit proces is als volgt:  

1.  Een blob ophalen uit de storage-service, het antwoord van een HTTP-ETag Header-waarde die aangeeft in de huidige versie van het object in de storage-service bestaat.
2.  Tijdens het bijwerken van de blob bevatten de ETag-waarde die u in stap 1 van de **If-Match** voorwaardelijke kop van de aanvraag die u naar de service verzendt hebt ontvangen.
3.  De service vergelijkt de ETag-waarde in de aanvraag met de huidige ETag-waarde van de blob.
4.  Als de huidige ETag-waarde van de blob een andere versie dan de ETag in de voorwaardelijke header **If-Match** in de aanvraag is, de service een fout 412 geretourneerd naar de client. Hiermee geeft u aan de client dat een ander proces de blob is bijgewerkt nadat deze door de client worden opgehaald.
5.  Als de huidige ETag-waarde van de blob dezelfde versie als de ETag in de voorwaardelijke header **If-Match** in de aanvraag is, kan de service de aangevraagde bewerking wordt uitgevoerd en werkt de huidige ETag-waarde van de label weer te geven dat deze een nieuwe versie gemaakt.  

De volgende C#-fragment (met behulp van de opslag-clientbibliotheek 4.2.0) geeft een eenvoudig voorbeeld van het maken van een **If-Match-AccessCondition** op basis van de ETag-waarde die wordt benaderd vanuit de eigenschappen van een blob die is eerder opgehaald of ingevoegd. Vervolgens wordt het **AccessCondition** -object wanneer het bijwerken van de blob: het **AccessCondition** -object wordt de header **If-Match** toegevoegd aan de aanvraag. Als een ander proces heeft het blob wordt bijgewerkt, wordt het blob een statusbericht HTTP 412 (voorwaarde mislukt). U kunt hier het volledige monster downloaden: [Gelijktijdige uitvoering met Azure opslag beheren](http://code.msdn.microsoft.com/Managing-Concurrency-using-56018114).  

    // Retrieve the ETag from the newly created blob
    // Etag is already populated as UploadText should cause a PUT Blob call
    // to storage blob service which returns the etag in response.
    string orignalETag = blockBlob.Properties.ETag;

    // This code simulates an update by a third party.
    string helloText = "Blob updated by a third party.";

    // No etag, provided so orignal blob is overwritten (thus generating a new etag)
    blockBlob.UploadText(helloText);
    Console.WriteLine("Blob updated. Updated ETag = {0}",
    blockBlob.Properties.ETag);

    // Now try to update the blob using the orignal ETag provided when the blob was created
    try
    {
        Console.WriteLine("Trying to update blob using orignal etag to generate if-match access condition");
        blockBlob.UploadText(helloText,accessCondition:
        AccessCondition.GenerateIfMatchCondition(orignalETag));
    }
    catch (StorageException ex)
    {
        if (ex.RequestInformation.HttpStatusCode == (int)HttpStatusCode.PreconditionFailed)
        {
            Console.WriteLine("Precondition failure as expected. Blob's orignal etag no longer matches");
            // TODO: client can decide on how it wants to handle the 3rd party updated content.
        }
        else
            throw;
    }  

De Storage-Service biedt ook ondersteuning voor aanvullende voorwaardelijke kopteksten, zoals **If-Modified-Since**, **Als ongewijzigd sinds** en **If-None-Match** als combinaties daarvan. Zie [Voorwaardelijke Headers opgeven voor Blob servicebewerkingen](http://msdn.microsoft.com/library/azure/dd179371.aspx) op MSDN voor meer informatie.  

In de volgende tabel bevat een overzicht van de bewerkingen van de container die voorwaardelijke headers zoals **If-Match** in het verzoek accepteren en die in het antwoord een ETag-waarde als resultaat.  

| Bewerking                | Geeft als resultaat de Container ETag-waarde | Voorwaardelijke-headers accepteert |
|:-------------------------|:-----------------------------|:----------------------------|
| Maken van Container         | Ja                          | Nee                          |
| Ophalen van de containereigenschappen | Ja                          | Nee                          |
| Container metagegevens ophalen   | Ja                          | Nee                          |
| Metagegevens van Container instellen   | Ja                          | Ja                         |
| Ophalen van de Container ACL        | Ja                          | Nee                          |
| ACL-Container instellen        | Ja                          | Ja (*)                     |
| Container verwijderen         | Nee                           | Ja                         |
| Container lease          | Ja                          | Ja                         |
| Lijst BLOB 's               | Nee                           | Nee                          |

(*) De machtigingen die zijn gedefinieerd door de SetContainerACL in de cache opgeslagen en updates van deze machtigingen 30 seconden duren doorgeven tijdens de periode waarin updates zijn niet gegarandeerd consistent.  

In de volgende tabel bevat een overzicht van de bewerkingen blob die voorwaardelijke headers zoals **If-Match** in het verzoek accepteren en die in het antwoord een ETag-waarde als resultaat.

| Bewerking           | Deze eigenschap retourneert een waarde van ETag | Voorwaardelijke-headers accepteert           |
|:--------------------|:-------------------|:--------------------------------------|
| Blob plaatsen            | Ja                | Ja                                   |
| Blob ophalen            | Ja                | Ja                                   |
| Blob-eigenschappen ophalen | Ja                | Ja                                   |
| Blob-eigenschappen instellen | Ja                | Ja                                   |
| Blob metagegevens ophalen   | Ja                | Ja                                   |
| Blob metagegevens instellen   | Ja                | Ja                                   |
| Lease Blob (*)      | Ja                | Ja                                   |
| Momentopname Blob       | Ja                | Ja                                   |
| Blob kopiëren           | Ja                | Ja (voor bron- en doeladressen blob) |
| Kopie Blob afbreken     | Nee                 | Nee                                    |
| Blob verwijderen         | Nee                 | Ja                                   |
| Blok plaatsen           | Nee                 | Nee                                    |
| Block lijst      | Ja                | Ja                                   |
| Block lijst      | Ja                | Nee                                    |
| Pagina plaatsen            | Ja                | Ja                                   |
| Get paginabereiken     | Ja                | Ja                                   |


(*) De ETag op een blob Blob lease niet gewijzigd.  

### <a name="pessimistic-concurrency-for-blobs"></a>Volledige vergrendeling voor BLOB 's
Als u wilt vergrendelen een blob voor exclusief gebruik, kunt u een [lease](http://msdn.microsoft.com/library/azure/ee691972.aspx) bij het aanschaffen. Wanneer u een lease-overeenkomst, opgeven hoe lang moet u de lease: dit is voor tussen 15 tot 60 seconden of oneindige welke bedragen naar een exclusieve vergrendeling. U kunt een eindige lease te laten verlengen en kunt u een lease vrijgeven wanneer u klaar met het bent. Eindige leases vrijgegeven de blob-service automatisch wanneer ze verlopen.  

Leases inschakelen andere synchronisatie-strategieën moet worden ondersteund, met inbegrip van de exclusieve schrijven / lezen, exclusieve schrijven gedeeld / exclusieve lezen en schrijven gedeeld / exclusive lezen. Wanneer een lease-overeenkomst bestaat de storage-service wordt afgedwongen exclusieve weggeschreven (put, instellen en verwijderen) exclusiviteit voor leesbewerkingen te waarborgen moet echter de ontwikkelaar om ervoor te zorgen dat alle clienttoepassingen een lease-ID en die slechts één client tegelijk gebruiken heeft een geldige lease-ID. Leesbewerkingen dat een resultaat van lease-ID niet in gedeelde leest opnemen.  

De volgende C#-fragment toont een voorbeeld van het verkrijgen van een exclusieve lease gedurende 30 seconden op een blob, bijwerken van de inhoud van de blob en vervolgens los te laten de lease-overeenkomst. Als er al een geldige lease op de blob wanneer u probeert een nieuwe lease te verkrijgen, de service blob resultaat wordt een "HTTP (409) Conflict" status. In het volgende fragment wordt een **AccessCondition** -object voor het inkapselen van de lease-informatie bij een aanvraag voor het bijwerken van de blob in de storage-service.  U kunt hier het volledige monster downloaden: [Gelijktijdige uitvoering met Azure opslag beheren](http://code.msdn.microsoft.com/Managing-Concurrency-using-56018114).

    // Acquire lease for 15 seconds
    string lease = blockBlob.AcquireLease(TimeSpan.FromSeconds(15), null);
    Console.WriteLine("Blob lease acquired. Lease = {0}", lease);

    // Update blob using lease. This operation will succeed
    const string helloText = "Blob updated";
    var accessCondition = AccessCondition.GenerateLeaseCondition(lease);
    blockBlob.UploadText(helloText, accessCondition: accessCondition);
    Console.WriteLine("Blob updated using an exclusive lease");

    //Simulate third party update to blob without lease
    try
    {
        // Below operation will fail as no valid lease provided
        Console.WriteLine("Trying to update blob without valid lease");
        blockBlob.UploadText("Update without lease, will fail");
    }
    catch (StorageException ex)
    {
        if (ex.RequestInformation.HttpStatusCode == (int)HttpStatusCode.PreconditionFailed)
            Console.WriteLine("Precondition failure as expected. Blob's lease does not match");
        else
            throw;
    }  

Als u een schrijfbewerking op een gehuurde blob probeert zonder de lease-ID wordt doorgegeven, mislukt de aanvraag met een fout 412. Houd er rekening mee dat als de lease verloopt voordat de methode **UploadText** wordt aangeroepen, maar u nog steeds de lease-ID doorgeeft, de aanvraag ook met een fout **412 mislukt** . Raadpleeg de documentatie van de REST [Lease Blob](http://msdn.microsoft.com/library/azure/ee691972.aspx) voor meer informatie over het beheren van de leaseduur verstrijken en lease-id.  

De volgende blob bewerkingen kunt leases beheren volledige vergrendeling:  


-   Blob plaatsen
-   Blob ophalen
-   Blob-eigenschappen ophalen
-   Blob-eigenschappen instellen
-   Blob metagegevens ophalen
-   Blob metagegevens instellen
-   Blob verwijderen
-   Blok plaatsen
-   Block lijst
-   Block lijst
-   Pagina plaatsen
-   Get paginabereiken
-   Momentopname Blob - lease-ID is optioneel als een lease-overeenkomst bestaat
-   Blob - ID vereist als een lease-overeenkomst op de bestemming blob bestaat lease kopiëren
-   Afbreken kopie Blob - lease-ID nodig als een oneindige lease op de bestemming blob bestaat
-   Lease Blob  

### <a name="pessimistic-concurrency-for-containers"></a>Volledige vergrendeling voor containers
Leases op containers inschakelen de dezelfde synchronisatie-strategieën op BLOB's worden ondersteund (exclusief schrijven / lezen, exclusieve schrijven gedeeld / exclusive lezen en schrijven gedeeld / exclusive lezen) echter in tegenstelling tot de BLOB's storage-service alleen afgedwongen exclusiviteit op de delete-bewerkingen. Als u wilt verwijderen van een container met een actieve lease, omvat een client de actieve lease-ID en het delete-verzoek. Alle andere container bewerkingen uitgevoerd op een gehuurde container zonder de lease-ID bewerkingen in welk geval ze worden gedeeld. Als de exclusiviteit van de update (put of set) of leesbewerkingen vereist is vervolgens ontwikkelaars moeten ervoor zorgen dat alle clients gebruiken een lease-ID en die slechts één client tegelijk een geldige lease heeft.  

De volgende container bewerkingen kunt leases beheren volledige vergrendeling:  

-   Container verwijderen
-   Ophalen van de containereigenschappen
-   Container metagegevens ophalen
-   Metagegevens van Container instellen
-   Ophalen van de Container ACL
-   ACL-Container instellen
-   Container lease  

Zie voor meer informatie:  

- [Voorwaardelijke Headers opgeven voor servicebewerkingen Blob](http://msdn.microsoft.com/library/azure/dd179371.aspx)
- [Container lease](http://msdn.microsoft.com/library/azure/jj159103.aspx)
- [Lease Blob](http://msdn.microsoft.com/library/azure/ee691972.aspx)

## <a name="managing-concurrency-in-the-table-service"></a>Gelijktijdigheid in de tabel-Service beheren
De tabel service gebruikt optimistische gelijktijdigheid wordt gecontroleerd als het standaardgedrag wanneer u werkt met entiteiten, in tegenstelling tot de blob-service waarbij u expliciet kiezen moet optimistische gelijktijdigheid controles uit te voeren. Het andere verschil tussen de diensten als de blob is u kunt alleen de gelijktijdige werking van entiteiten beheren dat met de blob-service kunt u de gelijktijdigheid van de recipiënten en de BLOB's beheren.  

Optimistische gelijktijdigheid en om te controleren als een ander proces een entiteit is gewijzigd sinds u het opgehaald uit de tabel storage-service, kunt u de ETag-waarde wordt weergegeven wanneer de tabel een entiteit wordt. Het overzicht van dit proces is als volgt:  

1.  Een entiteit worden opgehaald uit de tabel storage-service, het antwoord bevat een ETag-waarde die de huidige id die is gekoppeld aan die entiteit in de storage-service aangeeft.
2.  Tijdens het bijwerken van de entiteit bevatten de ETag-waarde die u in stap 1 van de verplichte header **If-Match** van de aanvraag die u naar de service verzendt hebt ontvangen.
3.  De service vergelijkt de ETag-waarde in de aanvraag met de huidige ETag-waarde van de entiteit.
4.  Als de huidige waarde van ETag van de entiteit anders dan de ETag in de verplichte header **If-Match** in de aanvraag is, de service een fout 412 geretourneerd naar de client. Hiermee geeft u aan de client dat een ander proces de entiteit heeft bijgewerkt nadat deze door de client worden opgehaald.
5.  Als de huidige waarde van ETag van de entiteit hetzelfde als de ETag in de verplichte header **If-Match** in de aanvraag is of de koptekst **Als resultaat** het jokerteken (*) bevat, wordt de service de aangevraagde bewerking wordt uitgevoerd en de huidige ETag-waarde van de entiteit weer te geven dat deze is bijgewerkt.  

Houd er rekening mee dat in tegenstelling tot de blob-service, de tabel service vereist is de client in te nemen een header **If-Match** updateaanvragen. Het is echter mogelijk om te dwingen een onvoorwaardelijke bijwerken (laatste schrijver WINS-strategie) en gelijktijdige controles overslaan als de client de header **If-Match** op het jokerteken (*) in de aanvraag wordt.  

De volgende C#-fragment toont klantentiteit die u eerder hebt gemaakt of opgehaald met hun e-mailadres bijgewerkt. De eerste plaats of bewerking winkels ETag-waarde in de klant-object ophalen omdat in dit voorbeeld dezelfde objectinstantie worden bij het uitvoeren van de vervangbewerking, verzonden en wordt automatisch de ETag-waarde terug naar de tabel service, het inschakelen van de service controleren op schendingen van gelijktijdigheid. Als een ander proces is de entiteit in de tabelopslag wordt bijgewerkt, wordt het een statusbericht HTTP 412 (voorwaarde mislukt).  U kunt hier het volledige monster downloaden: [Gelijktijdige uitvoering met Azure opslag beheren](http://code.msdn.microsoft.com/Managing-Concurrency-using-56018114).

    try
    {
        customer.Email = "updatedEmail@contoso.org";
        TableOperation replaceCustomer = TableOperation.Replace(customer);
        customerTable.Execute(replaceCustomer);
        Console.WriteLine("Replace operation succeeded.");
    }
    catch (StorageException ex)
    {
        if (ex.RequestInformation.HttpStatusCode == 412)
            Console.WriteLine("Optimistic concurrency violation – entity has changed since it was retrieved.");
        else
            throw;
    }  

Als het selectievakje gelijktijdigheid, expliciet uitschakelen, moet u de **ETag** -eigenschap van het object **werknemer** instellen ' * ' voordat u de vervangbewerking uitgevoerd.  

    customer.ETag = "*";  

De volgende tabel ziet hoe de bewerkingen van entiteit ETag waarden gebruiken:

| Bewerking                | Deze eigenschap retourneert een waarde van ETag | Verzoek-header If-Match is vereist |
|:-------------------------|:-------------------|:---------------------------------|
| Query-entiteiten           | Ja                | Nee                               |
| Entiteit invoegen            | Ja                | Nee                               |
| Entiteit bijwerken            | Ja                | Ja                              |
| Samenvoegen van entiteit             | Ja                | Ja                              |
| Entiteit verwijderen            | Nee                 | Ja                              |
| Invoegen of vervangen van entiteit | Ja                | Nee                               |
| Invoegen of entiteit samenvoegen   | Ja                | Nee                               |

Merk op dat de **entiteit vervangen of invoegen** en **invoegen of entiteit samenvoegen** bewerkingen uitvoeren *niet* gelijktijdige controles uitvoeren omdat ze een ETag-waarde niet aan de tabel service verzendt.  

In het algemeen moeten ontwikkelaars met behulp van tabellen vertrouwen op optimistische gelijktijdigheid bij het ontwikkelen van schaalbare toepassingen. Als volledige vergrendeling nodig is, een benadering ontwikkelaars worden wanneer toegang krijgen tot tabellen is het toewijzen van een aangewezen blob voor elke tabel en probeert uit te voeren van een lease-overeenkomst op de blob voordat die op de tabel. Deze benadering is de toepassing te waarborgen van de paden van de toegang tot alle gegevens verkrijgen van de lease-overeenkomst voor het besturingssysteem in de tabel vereist. Vergeet niet dat de leasetijd van ten minste 15 seconden, die moet zorgvuldig gebeuren voor schaalbaarheid is.  

Zie voor meer informatie:  

- [Bewerkingen op entiteiten](http://msdn.microsoft.com/library/azure/dd179375.aspx)  

## <a name="managing-concurrency-in-the-queue-service"></a>Gelijktijdigheid in de wachtrijservice beheren
Een scenario een probleem in de queueing-service welke gelijktijdigheid is is waar meerdere clients berichten uit een wachtrij worden opgehaald. Wanneer een bericht wordt opgehaald uit de wachtrij, wordt het antwoord bevat het bericht en de ontvangstwaarde van een pop-, die vereist is om het bericht te verwijderen. Het bericht wordt niet automatisch verwijderd uit de wachtrij, maar nadat deze zijn opgehaald, het is niet zichtbaar voor andere clients voor het interval dat is opgegeven met de parameter visibilitytimeout. De client die het bericht wordt opgehaald, wordt het bericht wilt verwijderen nadat deze is verwerkt en voordat de tijd die is opgegeven door de TimeNextVisible-element van de reactie die wordt berekend op basis van de waarde van de parameter visibilitytimeout verwacht. De waarde van visibilitytimeout wordt toegevoegd aan de tijd waarop het bericht wordt opgehaald om te bepalen van de waarde van TimeNextVisible.  

Queue-service biedt geen ondersteuning voor gelijktijdige Optimistisch of Pessimistisch en voor deze reden clients verwerken berichten uit een wachtrij opgehaald moeten ervoor zorgen de berichten worden verwerkt op een wijze die idempotency is ingeschakeld. Een laatste schrijver WINS-strategie wordt gebruikt voor bewerkingen zoals SetQueueServiceProperties, SetQueueMetaData, SetQueueACL en UpdateMessage.  

Zie voor meer informatie:  

- [REST API-wachtrij Service](http://msdn.microsoft.com/library/azure/dd179363.aspx)
- [Berichten ophalen](http://msdn.microsoft.com/library/azure/dd179474.aspx)  

## <a name="managing-concurrency-in-the-file-service"></a>Gelijktijdigheid in het bestand-Service beheren
De file service toegankelijk met twee verschillende eindpunten – SMB en de REST. De REST-service biedt geen ondersteuning voor beperkte vergrendeling of pessimistische vergrendeling en alle updates volgen nog een laatste schrijver WINS-strategie. Vergrendelingsmechanismen voor bestand-systeem voor het beheren van toegang tot gedeelde bestanden – met inbegrip van de mogelijkheid voor het uitvoeren van volledige vergrendeling kunnen gebruikmaken van SMB-clients die gedeelde bestanden koppelen. Als een SMB-client een bestand wordt geopend, geeft de toegang tot bestanden en de gedeelde modus. Een toegang tot het bestand van "Write" of "Lezen/schrijven" en een bestandsshare-modus, "Geen" optie resulteert in het bestand wordt vergrendeld door een SMB-client totdat het bestand wordt gesloten. Als REST-bewerking wordt uitgevoerd voor een bestand waarin een SMB-client het bestand vergrendeld is wordt de REST service statuscode 409 (Conflict) met de foutcode SharingViolation geretourneerd.  

Wanneer een SMB-client een bestand voor verwijderen opent, markeert het bestand als in behandeling verwijderen totdat de SMB-client open ingangen op dat bestand gesloten. Terwijl een bestand is gemarkeerd als in afwachting van verwijderen, wordt REST bewerking op dat bestand statuscode 409 (Conflict) met de foutcode SMBDeletePending geretourneerd. Statuscode 404 (niet gevonden) niet geretourneerd omdat de SMB-client om de markering in afwachting van verwijdering voor het afsluiten van het bestand te verwijderen. Statuscode 404 (niet gevonden) zal met andere woorden, alleen wanneer het bestand is verwijderd. Houd er rekening mee dat terwijl een bestand in een SMB in afwachting van frame verwijderen is, het zal niet worden opgenomen in de bestanden van de lijst met resultaten. Rekening mee dat de REST verwijderen van bestands- en REST verwijderen bewerkingen atomically doorgevoerd worden en niet tot gevolg hebben in de behandeling van status verwijderen.  

Zie voor meer informatie:  

- [Vergrendeld bestand beheren](http://msdn.microsoft.com/library/azure/dn194265.aspx)  

## <a name="summary-and-next-steps"></a>Samenvatting en volgende stappen
De Microsoft Azure Storage-service is ontworpen om te voldoen aan de behoeften van de meest complexe online toepassingen zonder ontwikkelaars kunnen binnendringen of denken over belangrijke hypothesen zoals consistentie gelijktijdigheid en de gegevens die ze nemen voor verleend.  

Voor de volledige voorbeeldtoepassing waarnaar wordt verwezen in deze blog:  

- [Beheren opslag Azure - voorbeeldtoepassing gelijktijdigheid](http://code.msdn.microsoft.com/Managing-Concurrency-using-56018114)  

Voor meer informatie over Azure opslag Zie:  

- [Introductiepagina voor Microsoft Azure opslag](https://azure.microsoft.com/services/storage/)
- [Inleiding tot Azure opslag](storage-introduction.md)
- Opslag voor [Blob](storage-dotnet-how-to-use-blobs.md), [tabel](storage-dotnet-how-to-use-tables.md), [wachtrijen](storage-dotnet-how-to-use-queues.md)en [bestanden](storage-dotnet-how-to-use-files.md) aan de slag
- Opslagarchitectuur – [Azure opslag: een hoge beschikbaarheid Cloud Storage-Service met een sterke samenhang](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx)
