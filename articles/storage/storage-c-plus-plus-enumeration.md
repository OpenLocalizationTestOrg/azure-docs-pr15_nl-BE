<properties
    pageTitle="Lijst met Azure opslagbronnen met de Client-bibliotheek van Microsoft Azure opslag voor C++ | Microsoft Azure"
    description="Leren de aanbieding API's gebruiken in Microsoft Azure opslag Client Library for C++ BLOB's, containers, wachtrijen, tabellen en entiteiten te inventariseren."
    documentationCenter=".net"
    services="storage"
    authors="dineshmurthy"
    manager="jahogg"
    editor="tysonn"/>
<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="dineshm"/>

# <a name="list-azure-storage-resources-in-c"></a>Lijst Azure opslagbronnen in C++

Aanbieding bewerkingen vormen de sleutel tot veel ontwikkeling-scenario's met Azure opslag. In dit artikel wordt beschreven hoe objecten in Azure opslag met behulp van de aanbieding geleverd in Microsoft Azure opslag Client Library for C++ API's zo efficiënt mogelijk te inventariseren.

>[AZURE.NOTE] Deze handleiding is bedoeld voor de Azure opslag Client-bibliotheek voor C++ versie 2.x die beschikbaar is via [NuGet](http://www.nuget.org/packages/wastorage) of [GitHub](https://github.com/Azure/azure-storage-cpp).

De Client-bibliotheek van opslag biedt tal van methoden om de lijst of query objecten in Azure opslag. In dit artikel komen de volgende scenario's:

-   Lijst van containers in een account
-   Lijst BLOB's in een container of blob virtuele map
-   Lijst wachtrijen op een rekening
-   Lijst met tabellen in een account
-   Query-entiteiten in een tabel

Elk van deze methoden wordt weergegeven met behulp van verschillende overbelastingen voor verschillende scenario's.

## <a name="asynchronous-versus-synchronous"></a>Asynchrone en synchrone

Omdat de opslag Client Library for C++ is gebouwd op de [REST van de C++-bibliotheek](https://github.com/Microsoft/cpprestsdk), ondersteunen we per definitie asynchrone bewerkingen met behulp van [pplx::task](http://microsoft.github.io/cpprestsdk/classpplx_1_1task.html). Bijvoorbeeld:

    pplx::task<list_blob_item_segment> list_blobs_segmented_async(continuation_token& token) const;

Synchrone bewerkingen doorloopt de corresponderende asynchrone bewerkingen:

    list_blob_item_segment list_blobs_segmented(const continuation_token& token) const
    {
        return list_blobs_segmented_async(token).get();
    }

Als u met meerdere threads toepassingen of services werkt, wordt u aangeraden de async-API's te gebruiken direct in plaats van het maken van een thread aan te roepen van de synchronisatie van API's die sterk van invloed op de prestaties van uw.

## <a name="segmented-listing"></a>Gesegmenteerde aanbieding

De schaal van cloud opslag vereist gesegmenteerde aanbieding. U kunt bijvoorbeeld via een miljoen BLOB's in een container Azure blob of entiteiten in een tabel Azure een miljard. Dit zijn geen theoretische cijfers, maar echte klant gebruik gevallen.

Daarom is het niet praktisch om alle objecten in één antwoord. In plaats daarvan kunt u met behulp van wisselbestanden objecten aanbieden. Elk van de API's van de aanbieding heeft een *gesegmenteerd* overbelasten.

Het antwoord voor een gesegmenteerde aanbieding omvat:

-   <i>_segment</i>, die de verzameling resultaten geretourneerd voor één aanroep van de API aanbieding bevat.
-   *continuation_token*, dat wordt doorgegeven aan de volgende oproep om de volgende pagina van de resultaten. Als er niet meer resultaten om terug te keren, wordt het token voortzetting is leeg.

Een typische aanroep naar de lijst van alle BLOB's in een container kan bijvoorbeeld het volgende codefragment eruit. De code is beschikbaar in onze [voorbeelden](https://github.com/Azure/azure-storage-cpp/blob/master/Microsoft.WindowsAzure.Storage/samples/BlobsGettingStarted/Application.cpp):

    // List blobs in the blob container
    azure::storage::continuation_token token;
    do
    {
        azure::storage::list_blob_item_segment segment = container.list_blobs_segmented(token);
        for (auto it = segment.results().cbegin(); it != segment.results().cend(); ++it)
    {
        if (it->is_blob())
        {
            process_blob(it->as_blob());
        }
        else
        {
            process_diretory(it->as_directory());
        }
    }

        token = segment.continuation_token();
    }
    while (!token.empty());

Houd er rekening mee dat het aantal resultaten dat wordt geretourneerd in een pagina kan worden beheerd door de parameter *max_results* in de overbelasting van de API, bijvoorbeeld:

    list_blob_item_segment list_blobs_segmented(const utility::string_t& prefix, bool use_flat_blob_listing,
        blob_listing_details::values includes, int max_results, const continuation_token& token,
        const blob_request_options& options, operation_context context)

Als u geen waarde voor de parameter *max_results opgeeft* , wordt de standaard maximale waarde van maximaal 5000 resultaten geretourneerd in een enkele pagina.

Vergeet niet dat een query op tabel Azure opslag als resultaat mogelijk geen records of minder records zijn dan de waarde van de parameter *max_results* die u hebt opgegeven, zelfs als het token voortzetting niet leeg is. Een reden kan zijn dat de query kan niet worden voltooid in vijf seconden. De query moet blijven zolang het token voortzetting niet leeg is, en uw code de grootte van het resultaat van een segment niet uitgaan.

De aanbevolen coding patroon voor de meeste scenario's is onderverdeeld op te nemen, waar u expliciete voortgang van de aanbieding of op te zoeken en hoe de service reageert op elke aanvraag. Met name voor C++-toepassingen of services kunt lager niveau controle van de voortgang van de aanbieding geheugen beheer en de prestaties.

## <a name="greedy-listing"></a>Hebzuchtig aanbieding

Eerdere versies van de opslag Client Library for C++ (versie 0.5.0 bekijken en eerder) opgenomen, niet gesegmenteerd aanbieding API's voor tabellen en wachtrijen, zoals in het volgende voorbeeld:

    std::vector<cloud_table> list_tables(const utility::string_t& prefix) const;
    std::vector<table_entity> execute_query(const table_query& query) const;
    std::vector<cloud_queue> list_queues() const;

Deze methoden zijn geïmplementeerd als wrappers gesegmenteerde API's. Voor elk antwoord van gesegmenteerde aanbiedingen, de code de resultaten toegevoegd aan een vector en alle resultaten geretourneerd nadat de volledige containers zijn gescand.

Deze benadering kan werken wanneer de opslag- of tabel een klein aantal objecten bevat. Echter, met een toename van het aantal objecten, het geheugen dat nodig kan vergroten zonder beperking, omdat alle resultaten in het geheugen bleven. Één aanbieding bewerking kan zeer lang duren, gedurende welke de aanroeper geen informatie over de voortgang heeft.

Deze API's in de SDK van hebzuchtig aanbieding bestaan niet in C#, Java of JavaScript Node.js milieu. Als u wilt voorkomen dat de potentiële problemen van het gebruik van deze API hebzuchtig, we deze verwijderd in versie 0.6.0 voorbeeld.

Als uw code deze hebzuchtig API's aanroepen:

    std::vector<azure::storage::table_entity> entities = table.execute_query(query);
    for (auto it = entities.cbegin(); it != entities.cend(); ++it)
    {
        process_entity(*it);
    }

Vervolgens moet u de code voor het gebruik van de gesegmenteerde aanbieding API's wijzigen:

    azure::storage::continuation_token token;
    do
    {
        azure::storage::table_query_segment segment = table.execute_query_segmented(query, token);
        for (auto it = segment.results().cbegin(); it != segment.results().cend(); ++it)
        {
            process_entity(*it);
        }

        token = segment.continuation_token();
    } while (!token.empty());

Door de parameter *max_results* van het segment op te geven, kunt u in evenwicht tussen de nummers van de aanvragen en geheugengebruik aan overwegingen bij prestaties van uw toepassing.

Ook als u een gesegmenteerde aanbieding API's gebruikt, maar de gegevens worden opgeslagen in een lokale collectie in een 'greedy' stijl, ook wordt aangeraden dat u refactoring van de code voor het verwerken van de gegevens opslaan in een lokale collectie zorgvuldig op schaal.

## <a name="lazy-listing"></a>Lazy aanbieding

Hoewel hebzuchtig aanbieding die potentiële problemen optreedt, is het handig als er te veel objecten in de container zijn.

Als u ook C# of Oracle Java SDK's gebruikt, moet u vertrouwd zijn met het overzicht programmeermodel dat een fikse stijl op te nemen biedt, waarbij de gegevens op een bepaalde offset alleen opgehaald als dat nodig is. In C++ biedt de sjabloon op basis van een iterator ook een soortgelijke benadering.

Een typische luie aanbieding API, met **list_blobs** als voorbeeld, ziet er zo uit:

    list_blob_item_iterator list_blobs() const;

Een typische codefragment dat gebruikmaakt van het patroon lazy aanbieding uitzien als volgt:

    // List blobs in the blob container
    azure::storage::list_blob_item_iterator end_of_results;
    for (auto it = container.list_blobs(); it != end_of_results; ++it)
    {
        if (it->is_blob())
        {
            process_blob(it->as_blob());
        }
        else
        {
            process_directory(it->as_directory());
        }
    }

Opmerking lazy aanbieding is alleen beschikbaar in de synchrone modus.

Vergeleken met hebzuchtig aanbieding, haalt lazy aanbieding gegevens alleen indien nodig. Achter de ophaalt het gegevens uit de opslag van Azure alleen wanneer de volgende iterator worden verplaatst naar het volgende segment. Daarom geheugengebruik wordt geregeld met een begrensde grootte en de werking is snel.

Lazy aanbieding API's zijn opgenomen in de opslag Client-bibliotheek voor C++ in versie 2.2.0.

## <a name="conclusion"></a>Conclusie

In dit artikel hebben besproken we verschillende overbelastingen voor het aanbieden van API's voor diverse objecten in de opslag Client Library for C++. Samenvatting:

-   Asynchrone APIs worden onder meerdere threads scenario's aanbevolen.
-   Gesegmenteerde aanbieding wordt aanbevolen voor de meeste scenario's.
-   Lazy aanbieding is beschikbaar in de bibliotheek als een handige wrapper in synchrone scenario's.
-   Hebzuchtig aanbieding wordt niet aanbevolen en is verwijderd uit de bibliotheek.

##<a name="next-steps"></a>Volgende stappen

Zie de volgende bronnen voor meer informatie over Azure opslag en Client-bibliotheek voor C++.

-   [Het gebruik van C++ Blob-opslag](storage-c-plus-plus-how-to-use-blobs.md)
-   [Het gebruik van opslag van C++ tabel](storage-c-plus-plus-how-to-use-tables.md)
-   [Het gebruik van opslag van C++ wachtrij](storage-c-plus-plus-how-to-use-queues.md)
-   [Azure opslag clientbibliotheek voor C++ API-documentatie.](http://azure.github.io/azure-storage-cpp/)
-   [Azure opslag-teamblog](http://blogs.msdn.com/b/windowsazurestorage/)
-   [Azure opslag documentatie](https://azure.microsoft.com/documentation/services/storage/)
