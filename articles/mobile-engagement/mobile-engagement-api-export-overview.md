<properties
    pageTitle="Betrokkenheid bij de mobiele Export API-overzicht"
    description="Leer de grondbeginselen van het exporteren van de ruwe gegevens die door apparaten gebruikmaken van deze in uw eigen hulpprogramma's van de gebruiker worden gegenereerd"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="kpiteira"
    manager="erikre"
    editor=""/>

<tags
    ms.service="mobile-engagement"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="mobile-multiple"
    ms.workload="mobile"
    ms.date="04/26/2016"
    ms.author="kpiteira"/>

# <a name="mobile-engagement-export-api-overview"></a>Betrokkenheid bij de mobiele Export API-overzicht

## <a name="introduction"></a>Inleiding

In dit document leert u de basisbegrippen over het exporteren van de ruwe gegevens die door apparaten gebruikmaken van deze in uw eigen hulpprogramma's van de gebruiker worden gegenereerd.

## <a name="pre-requisites"></a>Minimumvereisten

Alleen de onbewerkte gegevens exporteren van Mobile Engagement:

- API authentication setup kunnen gebruikmaken van de API's (Zie [handmatige verificatie-instellingen](mobile-engagement-api-authentication-manual.md)),
- De REST API's of [SDK voor .net](mobile-engagement-dotnet-sdk-service-api.md)gebruiken
- Een account Azure opslag.

>[AZURE.NOTE] Ook wordt geadviseerd de uitstekende [Microsoft Azure Opslagverkenner](http://storageexplorer.com/)ten minste tijdens de ontwikkelingsfase aangezien deze een gebruiksvriendelijke gebruikersinterface biedt voor interactie met Azure opslag.

## <a name="what-can-be-exported"></a>Wat kunnen worden geëxporteerd?

Mobile Engagement kunnen de gebruikers veel soorten gegevens verzamelen en heeft daarom verschillende soorten uitvoer die geschikt is voor deze verschillende gegevenstypen.
Er zijn 2 belangrijke soorten uitvoer:

- Momentopname: gebruikt om gegevens te exporteren vertegenwoordigt die meestal een staat en waarvoor Mobile Engagement heeft geen geschiedenis. Dit bevat bijvoorbeeld Tags (app-info)-tokens of push campagne feedback. Als gevolg hiervan dit soort export niet gerelateerd zijn aan een datum.
- historische: dit type uitvoer wordt gebruikt voor gegevens die zich bijvoorbeeld na verloop van tijd, zoals gebeurtenissen of activiteiten.

De volgende tabel bevat een uitgebreid de mogelijke uitvoer:

| Type exporteren | Gegevenstype | Beschrijving                                                                                                                                 |
|-------------|-----------|---------------------------------------------------------------------------------------------------------------------------------------------|
| Momentopname    | Push      | Uitvoer van feedback van de Push-campagnes op basis van per deviceid/gebruikers-id wordt gegenereerd                                                              |
| Momentopname    | Label       | Genereert een export van de tags (app info) die is gekoppeld aan elke apparaten                                                                       |
| Momentopname    | Apparaat    | Genereert een uitvoer van de gegevens over apparaten zoals de technicals (model, landinstellingen, tijdzone,...), de codes, eerst gezien, de meeste... |
| Momentopname    | Token     | Uitvoer van de geldige tokens genereert                                                                                                 |
| Historische  | Activiteit  | Genereert een export van de activiteiten voor alle apparaten in een bepaalde periode                                                           |
| Historische  | Gebeurtenis     | Genereert een export van de activiteiten voor alle apparaten in een bepaalde periode                                                           |
| Historische  | Taak       | Genereert een export van de taken voor alle apparaten in een bepaalde periode                                                                 |
| Historische  | Fout     | Genereert een export van de fouten voor alle apparaten in een bepaalde periode                                                               |

## <a name="how-does-it-work"></a>Hoe werkt het?

Uitvoer lang worden taken uitgevoerd die kan leiden tot grote gegevensbestanden. Om die reden wordt ze niet aanroepen om terug te keren direct een bestand te downloaden.
Om gegevens te exporteren uit Mobile Engagement, hebt u een **Taak exporteren** via API wilt maken waar u in het algemeen:

- Het type van uitvoer (momentopname of historische)
- Het gegevenstype
- **Azure opslag Container** (met inbegrip van een geldige SAS met schrijftoegang) waarbij het resultaat van de uitvoer wordt geschreven.

Houd er rekening mee dat het enkele minuten duren voordat de taak duurt moet worden gestart en het u enkele seconden voor kleine toepassingen tot enkele uren voor toepassingen met veel gebruikers of activiteit voert mogelijk.

Zodra de taak is gemaakt, is het mogelijk om de status weer te geven als deze nog wordt uitgevoerd of als deze is voltooid.

Zodra de taak is voltooid, is het resulterende bestand is beschikbaar op de container voor opslag opgegeven.
