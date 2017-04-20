<properties
    pageTitle="De Azure blobopslag Connector toevoegen in uw logica Apps | Microsoft Azure"
    description="Overzicht van Azure blob-opslag Connector met REST API parameters"
    services=""
    documentationCenter="" 
    authors="MandiOhlinger"
    manager="anneta"
    editor=""
    tags="connectors"/>

<tags
   ms.service="logic-apps"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="10/18/2016"
   ms.author="mandia"/>

# <a name="get-started-with-the-azure-blob-storage-connector"></a>Aan de slag met Azure blob storage-connector
Azure Blob storage is een service voor het opslaan van grote hoeveelheden ongestructureerde gegevens. Diverse acties uitvoeren zoals het uploaden, bijwerken, ophalen en verwijderen van BLOB's in Azure blob-opslag. 

Met Azure blobopslag u:

- Bouw uw werkstroom door nieuwe projecten uploaden of ophalen van bestanden die onlangs zijn bijgewerkt.
- Acties voor bestand metagegevens ophalen, verwijderen van een bestand en bestanden kopiëren gebruiken. Bijvoorbeeld wanneer een programma wordt bijgewerkt in een Azure-website (trigger), vervolgens een bestand bijwerken in blob-opslag (actie). 

In dit onderwerp wordt beschreven hoe u blob storage connector gebruiken in een logica app u en worden ook de acties.

>[AZURE.NOTE] Deze versie van het artikel is van toepassing op logica Apps algemene beschikbaarheid (GA). 

Zie voor meer informatie over Logic Apps, [Wat zijn apps logica](../app-service-logic/app-service-logic-what-are-logic-apps.md) en [maken van een app logica](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="connect-to-azure-blob-storage"></a>Verbinding maken met Azure blob-opslag

Voordat uw logica app toegang een service tot, maakt u een *verbinding* met de service. Een verbinding biedt connectiviteit tussen een app logica en een andere service. Bijvoorbeeld voor verbinding met een account van de opslag, u eerst een blob opslag maken *verbinding*. Een verbinding wilt maken, voert u de referenties gebruikt voor toegang tot de service die u gaat aansluiten op. Dus met Azure opslag, geeft u de referenties op uw account opslag om de verbinding te maken. 

#### <a name="create-the-connection"></a>Maak de verbinding

>[AZURE.INCLUDE [Create a connection to Azure blob storage](../../includes/connectors-create-api-azureblobstorage.md)]
 
## <a name="use-a-trigger"></a>Een trigger gebruiken

Deze connector hoeft niet alle triggers. Met andere triggers kunt u start de app logica, zoals een trigger terugkeerpatroon, een HTTP-Webhook-trigger, triggers met andere connectors en meer beschikbaar. [Maken van een app logica](../app-service-logic/app-service-logic-create-a-logic-app.md) wordt een voorbeeld gegeven.

## <a name="use-an-action"></a>Actie
    
Een actie is een bewerking die wordt uitgevoerd door de werkstroom in een app logica gedefinieerd.

1. Selecteer het plus-teken. Ziet u verschillende mogelijkheden: **een actie toevoegen**, **toevoegen van een voorwaarde**of een van de **meer** opties.

    ![](./media/connectors-create-api-azureblobstorage/add-action.png)

2. Kies **een actie toevoegen**.

3. Typ in het tekstvak "blob" voor een overzicht van alle beschikbare acties.

    ![](./media/connectors-create-api-azureblobstorage/actions.png) 

4. In ons voorbeeld kiezen **AzureBlob - metagegevens van het bestand met behulp van pad ophalen**. Als er al een verbinding bestaat, selecteert u de **...** Knop (datumkiezer weergeven) om een bestand te selecteren.

    ![](./media/connectors-create-api-azureblobstorage/sample-file.png)

    Als u wordt gevraagd om de verbindingsgegevens, voert u de details van de verbinding te maken. Deze eigenschappen van [de verbinding maken](connectors-create-api-azureblobstorage.md#create-the-connection) in dit onderwerp worden beschreven. 

    > [AZURE.NOTE] In dit voorbeeld krijgen we de metagegevens van een bestand. De metagegevens, toe te voegen die u maakt een nieuw bestand met een andere connector. Bijvoorbeeld een OneDrive-actie die u een nieuw bestand maakt "test" op basis van de metagegevens toevoegen. 

5. **Sla** uw wijzigingen (linksboven aan de werkbalk). Uw app logica wordt opgeslagen en automatisch kan worden ingeschakeld.

> [AZURE.TIP] [Opslagverkenner](http://storageexplorer.com/) is een geweldig hulpmiddel voor het beheren van meerdere accounts voor opslag.

## <a name="technical-details"></a>Technische Details

## <a name="storage-blob-actions"></a>Opslag Blob acties

|Actie|Beschrijving|
|--- | ---|
|[Metagegevens van bestanden ophalen](connectors-create-api-azureblobstorage.md#get-file-metadata)|Deze bewerking wordt de metagegevens van het bestand met de bestands-id.|
|[Update-bestand](connectors-create-api-azureblobstorage.md#update-file)|Deze bewerking een bestand wordt bijgewerkt.|
|[Bestand verwijderen](connectors-create-api-azureblobstorage.md#delete-file)|Deze bewerking verwijdert een bestand.|
|[Ophalen van metagegevens van het bestand met behulp van pad](connectors-create-api-azureblobstorage.md#get-file-metadata-using-path)|Deze bewerking wordt de metagegevens van het bestand met het pad.|
|[Inhoud van bestand met behulp van pad](connectors-create-api-azureblobstorage.md#get-file-content-using-path)|Deze bewerking wordt de inhoud van het bestand met het pad.|
|[Inhoud van bestand ophalen](connectors-create-api-azureblobstorage.md#get-file-content)|Deze bewerking wordt de bestandsinhoud met id.|
|[Bestand maken](connectors-create-api-azureblobstorage.md#create-file)|Deze bewerking is een bestand geüpload.|
|[Bestand kopiëren](connectors-create-api-azureblobstorage.md#copy-file)|Deze bewerking kopieert een bestand naar Azure Blob-opslag.|
|[Archief map uitpakken](connectors-create-api-azureblobstorage.md#extract-archive-to-folder)|Deze bewerking een archiefbestand uitpakken naar een map (voorbeeld: .zip).|

### <a name="action-details"></a>Actiedetails

In dit gedeelte ziet u de specifieke details over elke actie, met inbegrip van eventuele verplichte of optionele eigenschappen voor de invoer en de bijbehorende uitvoer die is gekoppeld aan de verbindingslijn.

#### <a name="get-file-metadata"></a>Metagegevens van bestanden ophalen
Deze bewerking wordt de metagegevens van het bestand met de bestands-id.  

|Naam van eigenschap| Weergavenaam|Beschrijving|
| ---|---|---|
|ID *|Bestand|Selecteer een bestand|

Een sterretje (*) betekent dat de eigenschap is vereist.

##### <a name="output-details"></a>Uitvoerdetails
BlobMetadata

| Naam van eigenschap | Gegevenstype |
|---|---|
|ID|tekenreeks|
|Naam|tekenreeks|
|DisplayName|tekenreeks|
|Pad|tekenreeks|
|LastModified|tekenreeks|
|Grootte|geheel getal|
|MediaType|tekenreeks|
|IsFolder|Boole-waarde|
|ETag|tekenreeks|
|FileLocator|tekenreeks|


#### <a name="update-file"></a>Update-bestand
Deze bewerking een bestand wordt bijgewerkt.  

|Naam van eigenschap| Weergavenaam|Beschrijving|
| ---|---|---|
|ID *|Bestand|Selecteer een bestand|
|instantie *|Inhoud van bestand|Inhoud van het bestand bij te werken|

Een sterretje (*) betekent dat de eigenschap is vereist.

##### <a name="output-details"></a>Uitvoerdetails
BlobMetadata

| Naam van eigenschap | Gegevenstype |
|---|---|
|ID|tekenreeks|
|Naam|tekenreeks|
|DisplayName|tekenreeks|
|Pad|tekenreeks|
|LastModified|tekenreeks|
|Grootte|geheel getal|
|MediaType|tekenreeks|
|IsFolder|Boole-waarde|
|ETag|tekenreeks|
|FileLocator|tekenreeks|


#### <a name="delete-file"></a>Bestand verwijderen
Deze bewerking verwijdert een bestand.  

|Naam van eigenschap| Weergavenaam|Beschrijving|
| ---|---|---|
|ID *|Bestand|Selecteer een bestand|

Een sterretje (*) betekent dat de eigenschap is vereist.

##### <a name="output-details"></a>Uitvoerdetails
Geen.


#### <a name="get-file-metadata-using-path"></a>Ophalen van metagegevens van het bestand met behulp van pad
Deze bewerking wordt de metagegevens van het bestand met het pad.  

|Naam van eigenschap| Weergavenaam|Beschrijving|
| ---|---|---|
|pad *|Pad naar bestand|Selecteer een bestand|

Een sterretje (*) betekent dat de eigenschap is vereist.

##### <a name="output-details"></a>Uitvoerdetails
BlobMetadata

| Naam van eigenschap | Gegevenstype |
|---|---|
|ID|tekenreeks|
|Naam|tekenreeks|
|DisplayName|tekenreeks|
|Pad|tekenreeks|
|LastModified|tekenreeks|
|Grootte|geheel getal|
|MediaType|tekenreeks|
|IsFolder|Boole-waarde|
|ETag|tekenreeks|
|FileLocator|tekenreeks|


#### <a name="get-file-content-using-path"></a>Inhoud van bestand met behulp van pad
Deze bewerking wordt de inhoud van het bestand met het pad.  

|Naam van eigenschap| Weergavenaam|Beschrijving|
| ---|---|---|
|pad *|Pad naar bestand|Selecteer een bestand|

Een sterretje (*) betekent dat de eigenschap is vereist.

##### <a name="output-details"></a>Uitvoerdetails
Geen.


#### <a name="get-file-content"></a>Inhoud van bestand ophalen
Deze bewerking wordt de bestandsinhoud met id.  

|Naam van eigenschap| Gegevenstype|Beschrijving|
| ---|---|---|
|ID *|tekenreeks|Selecteer een bestand|

Een sterretje (*) betekent dat de eigenschap is vereist.

##### <a name="output-details"></a>Uitvoerdetails
Geen.


#### <a name="create-file"></a>Bestand maken
Deze bewerking is een bestand geüpload.  

|Naam van eigenschap| Weergavenaam|Beschrijving|
| ---|---|---|
|mappad: *|Pad naar map|Selecteer een map|
|naam *|Bestandsnaam|Naam van het bestand te uploaden|
|instantie *|Inhoud van bestand|Inhoud van het bestand te uploaden|

Een sterretje (*) betekent dat de eigenschap is vereist.

##### <a name="output-details"></a>Uitvoerdetails
BlobMetadata

| Naam van eigenschap | Gegevenstype | 
|---|---|
|ID|tekenreeks|
|Naam|tekenreeks|
|DisplayName|tekenreeks|
|Pad|tekenreeks|
|LastModified|tekenreeks|
|Grootte|geheel getal|
|MediaType|tekenreeks|
|IsFolder|Boole-waarde|
|ETag|tekenreeks|
|FileLocator|tekenreeks|


#### <a name="copy-file"></a>Bestand kopiëren
Deze bewerking kopieert een bestand naar Azure Blob-opslag.  

|Naam van eigenschap| Weergavenaam|Beschrijving|
| ---|---|---|
|bron *|De bron-url|Geef de Url naar het bronbestand|
|bestemming *|Bestemmingspad|Geef het bestandspad bestemming, met inbegrip van de doelbestandsnaam|
|overschrijven|Overschrijven?|Moet een bestaand doelbestand overschreven (true/false)?  |

Een sterretje (*) betekent dat de eigenschap is vereist.

##### <a name="output-details"></a>Uitvoerdetails
BlobMetadata

| Naam van eigenschap | Gegevenstype |
|---|---|
|ID|tekenreeks|
|Naam|tekenreeks|
|DisplayName|tekenreeks|
|Pad|tekenreeks|
|LastModified|tekenreeks|
|Grootte|geheel getal|
|MediaType|tekenreeks|
|IsFolder|Boole-waarde|
|ETag|tekenreeks|
|FileLocator|tekenreeks|

#### <a name="extract-archive-to-folder"></a>Archief map uitpakken
Deze bewerking een archiefbestand uitpakken naar een map (voorbeeld: .zip).  

|Naam van eigenschap| Weergavenaam|Beschrijving|
| ---|---|---|
|bron *|Pad naar bron archief|Selecteer een archiefbestand|
|bestemming *|Bestemmingspad|Selecteer de inhoud uitpakken|
|overschrijven|Overschrijven?|Moet een bestaand doelbestand overschreven (true/false)?|

Een sterretje (*) betekent dat de eigenschap is vereist.

##### <a name="output-details"></a>Uitvoerdetails
BlobMetadata

| Naam van eigenschap | Gegevenstype |
|---|---|
|ID|tekenreeks|
|Naam|tekenreeks|
|DisplayName|tekenreeks|
|Pad|tekenreeks|
|LastModified|tekenreeks|
|Grootte|geheel getal|
|MediaType|tekenreeks|
|IsFolder|Boole-waarde|
|ETag|tekenreeks|
|FileLocator|tekenreeks|


## <a name="http-responses"></a>HTTP-antwoorden

Als u de verschillende acties, krijgt u mogelijk bepaalde antwoorden. De volgende tabel geeft een overzicht van de antwoorden en de bijbehorende beschrijvingen:  

|Naam|Beschrijving|
|---|---|
|200|OK|
|202|Geaccepteerd|
|400|Ongeldig verzoek|
|401|Niet-geautoriseerde|
|403|Verboden|
|404|Niet gevonden|
|500|Interne serverfout. Er is een onbekende fout opgetreden|
|Standaard|De bewerking is mislukt.|

## <a name="next-steps"></a>Volgende stappen

[Maken van een app logica](../app-service-logic/app-service-logic-create-a-logic-app.md). Bekijk de beschikbare connectors in Apps logica op onze [lijst van API's](apis-list.md).



