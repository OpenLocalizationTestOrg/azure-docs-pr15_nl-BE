<properties 
    pageTitle="Overzicht Media Services REST API | Microsoft Azure" 
    description="Overzicht Media Services REST API" 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="10/12/2016"
    ms.author="juliako"/>


# <a name="media-services-rest-api-overview"></a>Overzicht Media Services REST API 

[AZURE.INCLUDE [media-services-selector-setup](../../includes/media-services-selector-setup.md)]

Microsoft Azure Media Services is een service die op basis van OData HTTP-aanvragen accepteert en terug in de uitgebreide JSON of atom + pub kunt reageren. Omdat Media Services aan de richtlijnen voor het ontwerpen van Azure voldoen, is er een reeks vereiste HTTP-headers die elke client gebruiken moet wanneer u verbinding maakt met Media Services, alsook een aantal optionele headers die kunnen worden gebruikt. De volgende secties worden de kopteksten en HTTP-woorden die u kunt gebruiken bij maken van verzoeken en antwoorden ontvangen van Media Services.

##<a name="considerations"></a>Overwegingen met betrekking tot 

De volgende overwegingen zijn van toepassing bij het gebruik van de REST.

- Bij het opvragen van entiteiten, is er een limiet van 1000 entiteiten in één keer worden geretourneerd omdat openbare REST v2 queryresultaten 1000 resultaten beperkt. U moet gebruiken, **overslaan** en **nemen** (.NET) / **top** (REST) als beschreven in [dit voorbeeld .NET](media-services-dotnet-manage-entities.md#enumerating-through-large-collections-of-entities) en [REST API voorbeeld](media-services-rest-manage-entities.md#enumerating-through-large-collections-of-entities). 

- Als JSON en voor het gebruik van het trefwoord **__metadata** in de aanvraag te geven (bijvoorbeeld naar verwijst naar een gekoppeld object) moet u de **Accept** -header ingesteld op [uitgebreide JSON-indeling](http://www.odata.org/documentation/odata-version-3-0/json-verbose-format/) (Zie het volgende voorbeeld). OData begrijpt niet de eigenschap **__metadata** in het verzoek, tenzij u deze op uitgebreid instelt.  

        POST https://media.windows.net/API/Jobs HTTP/1.1
        Content-Type: application/json;odata=verbose
        Accept: application/json;odata=verbose
        DataServiceVersion: 3.0
        MaxDataServiceVersion: 3.0
        x-ms-version: 2.11
        Authorization: Bearer <token> 
        Host: media.windows.net
        
        {
            "Name" : "NewTestJob", 
            "InputMediaAssets" : 
                [{"__metadata" : {"uri" : "https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3Aba5356eb-30ff-4dc6-9e5a-41e4223540e7')"}}]
        . . . 
        

## <a name="standard-http-request-headers-supported-by-media-services"></a>Standaard HTTP-headers voor de aanvraag wordt ondersteund door Media Services

Er is een reeks vereiste headers die u in uw aanvraag opnemen moet voor elk gesprek dat u Media Services maken, en ook een aantal optionele headers u wilt opnemen. De volgende tabel bevat de vereiste headers:


Koptekst|Type|Waarde
---|---|---
Autorisatie|Aan toonder|Aan toonder is het enige geaccepteerde autorisatie mechanisme. De waarde moet ook het toegangstoken verstrekt door de ACS.
x-ms-versie|Decimaal|2.11
DataServiceVersion|Decimaal|3.0
MaxDataServiceVersion|Decimaal|3.0



>[AZURE.NOTE] Omdat OData Media Services gebruikt om de onderliggende activa metagegevens opslagplaats via REST API's beschikbaar te maken, moeten de koppen van de DataServiceVersion en MaxDataServiceVersion worden opgenomen in elk verzoek; echter, als dat niet het geval is, vervolgens momenteel Media Services wordt verondersteld de waarde DataServiceVersion in gebruik 3.0.

Hier volgt een aantal optionele headers:

Koptekst|Type|Waarde
---|---|---
Datum|RFC 1123 datum|Tijdstempel van de aanvraag
Accepteren|Type inhoud|Het aangevraagde type inhoud voor het antwoord op de volgende:<p> -application/json; odata = verbose<p> -atom-toepassing + xml<p> Antwoorden wellicht een ander type inhoud, zoals een blob-fetch waarbij een positief antwoord bevat de blob-stroom als de nettolading.
Accepteren-codering|Gzip, verkleinen|GZIP en DEFLATE codering, indien van toepassing. Opmerking: Voor grote bronnen, Media Services kan deze header te negeren en niet-gecomprimeerde gegevens.
Accepteren-taal|"nl", "es", enzovoort.|Hiermee geeft u de gewenste taal voor het antwoord.
Accepteer Charset|Charset type zoals "UTF-8"|Standaard wordt UTF-8.
X-HTTP-methode|HTTP-methode|Hiermee kunt clients of firewalls die geen ondersteuning voor HTTP-methoden, zoals het plaatsen of verwijderen bieden Gebruik deze methoden tunnelverbinding via een aanroep ophalen.
Content-Type|Type inhoud|Type inhoud van het hoofdgedeelte van de aanvraag in de PUT- of POST-aanvragen.
client-aanvraag-id|Tekenreeks|Een beller gedefinieerde waarde waarmee de aanvraag. Als u opgeeft, wordt deze waarde in het antwoordbericht opgenomen als een manier om de aanvraag toewijzen. <p><p>**Belangrijk**<p>Waarden moeten worden beperkt tot 2096b (2k).

## <a name="standard-http-response-headers-supported-by-media-services"></a>Standaard-HTTP-antwoordheaders worden ondersteund door Media Services

Het volgende is een set van koppen die kunnen worden geretourneerd aan u, afhankelijk van de bron die u zijn aangevraagd en de actie die u wilde uitvoeren.


Koptekst|Type|Waarde
---|---|---
aanvraag-id|Tekenreeks|Een unieke id voor de huidige bewerking, service gegenereerd.
client-aanvraag-id|Tekenreeks|Een id die is opgegeven door de aanvrager in de oorspronkelijke aanvraag, indien aanwezig.
Datum|RFC 1123 datum|De datum waarop de aanvraag is verwerkt.
Content-Type|Varieert|Het type inhoud van het hoofdgedeelte van de response.
Content-Encoding|Varieert|Gzip of deflate, indien nodig.


## <a name="standard-http-verbs-supported-by-media-services"></a>Standaard HTTP-woorden die worden ondersteund door Media Services

Hieronder volgt een volledige lijst met HTTP-woorden die kunnen worden gebruikt bij het maken van HTTP-aanvragen:


Werkwoord|Beschrijving
---|---
Toevoegen|Deze eigenschap retourneert de huidige waarde van een object.
Verzenden|Hiermee maakt u een object op basis van de verstrekte gegevens of een opdracht verzendt.
PLAATSEN|Een object vervangt of maakt u een benoemd object (indien van toepassing).
VERWIJDEREN|Deze methode verwijdert een object.
SAMENVOEGEN|Een bestaand object bijgewerkt met wijzigingen in de benoemde eigenschappen.
HOOFD|Geeft als resultaat de metagegevens van een object voor een antwoord krijgen.

##<a name="limitation"></a>Beperking

Bij het opvragen van entiteiten, is er een limiet van 1000 entiteiten in één keer worden geretourneerd omdat openbare REST v2 queryresultaten 1000 resultaten beperkt. U moet gebruiken, **overslaan** en **nemen** (.NET) / **top** (REST) als beschreven in [dit voorbeeld .NET](media-services-dotnet-manage-entities.md#enumerating-through-large-collections-of-entities) en [REST API voorbeeld](media-services-rest-manage-entities.md#enumerating-through-large-collections-of-entities). 


## <a name="discovering-media-services-model"></a>Ontdekking van een model van Media Services

Als u entiteiten Media Services op te sporen, kan de $metadata-bewerking worden gebruikt. U kunt ophalen voor alle Entiteitstypen geldig, entiteitseigenschappen, koppelingen, functies, acties, enzovoort. In het volgende voorbeeld ziet u hoe de URI samenstellen: https://media.windows.net/API/$ metagegevens.

U moet toevoegen "? api version=2.x" aan het einde van de URI als u wilt de metagegevens in een browser bekijkt of de header x-ms-versie niet in uw aanvraag opnemen.



##<a name="media-services-learning-paths"></a>Leerfasen Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]





 
