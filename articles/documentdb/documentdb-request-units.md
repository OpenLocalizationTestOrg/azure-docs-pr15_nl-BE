<properties 
    pageTitle="Eenheden in DocumentDB aanvragen | Microsoft Azure" 
    description="Informatie over het te begrijpen, opgeven en aanvraag eenheid voorschriften in DocumentDB te schatten." 
    services="documentdb" 
    authors="syamkmsft" 
    manager="jhubbard" 
    editor="mimig" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="06/29/2016" 
    ms.author="syamk"/>

#<a name="request-units-in-documentdb"></a>Eenheden in DocumentDB aanvragen
Nu beschikbaar: DocumentDB [aanvraag eenheid Rekenmachine](https://www.documentdb.com/capacityplanner). Meer informatie in [de doorvoer moet Estimating](documentdb-request-units.md#estimating-throughput-needs).

![Doorvoer van Rekenmachine][5]

##<a name="introduction"></a>Inleiding
Dit artikel biedt een overzicht van eenheden in [Microsoft Azure DocumentDB](https://azure.microsoft.com/services/documentdb/)verzoek. 

Na het lezen van dit artikel, zult u de volgende vragen beantwoorden:  

-   Wat zijn eenheden aanvragen en aanvragen toeslagen?
-   Hoe geef ik verzoek eenheid capaciteit voor een collectie
-   Hoe u schatten dat van mijn toepassing aanvraag heeft?
-   Wat gebeurt er als ik meer dan verzoek eenheid capaciteit voor een collectie?


##<a name="request-units-and-request-charges"></a>Eenheden aanvragen en aanvragen toeslagen
DocumentDB biedt snelle, voorspelbare prestaties door resources *reserveren* om te voldoen aan de behoeften van uw toepassing doorvoer.  Omdat de toepassing geladen en toegang tot wijziging van de patronen in de tijd, DocumentDB Hiermee kunt u eenvoudig vergroten of verkleinen van de hoeveelheid gereserveerde doorvoer die beschikbaar is voor uw toepassing.

Met DocumentDB, gereserveerde doorvoer gespecificeerd voor aanvraag eenheden per seconde verwerkt.  U kunt eenheden aanvraag beschouwen als doorvoer valuta, waarbij *reserveren* bedrag van aanvraag eenheden beschikbaar is voor uw toepassing op basis van per seconde gegarandeerde.  Elke bewerking in DocumentDB - schrijven, uitvoeren van een query, een document bijwerken - verbruikt CPU, geheugen en IOP's.  Dat wil zeggen, resulteert elke bewerking in een *aanvraag kosten*, uitgedrukt in *eenheden van de aanvraag*.  Wat zijn de factoren die van invloed zijn op aanvraag eenheid kosten, samen met de vereisten van uw toepassing doorvoer, kunt u uw toepassing als kosten zo efficiënt mogelijk uitvoeren. 

##<a name="specifying-request-unit-capacity"></a>Aanvraag eenheid capaciteit op te geven
Bij het maken van een DocumentDB-collectie, kunt u het aantal eenheden van de aanvraag per seconde (RUs) gereserveerd is voor de collectie dat opgeven.  Als de collectie is gemaakt, is de volledige toewijzing van de RUs opgegeven gereserveerd voor gebruik van de collectie.  Elke collectie is gegarandeerd hebt toegewezen en alleenstaande doorvoer kenmerken.  

Het is belangrijk te weten dat DocumentDB op een reservering model werkt; dat wil zeggen, worden u gefactureerd voor de doorvoer *gereserveerd* voor de collectie, ongeacht hoeveel waarop doorvoer actief *gebruikt is*.  Houd er echter rekening mee, dat als een van uw toepassing laden gegevens en gebruik patronen wijzigen, die kunt u gemakkelijk de schaal en het bedrag van de RUs via DocumentDB SDK's of met behulp van de [Portal Azure](https://portal.azure.com)gereserveerd.  Zie voor meer informatie over de doorvoer schaal omhoog en omlaag, [DocumentDB prestaties](documentdb-performance-levels.md).

##<a name="request-unit-considerations"></a>Overwegingen bij het verzoek eenheid
Bij het bepalen van het aantal eenheden van de aanvraag te reserveren voor de DocumentDB-collectie, is het belangrijk de volgende variabelen in aanmerking te nemen:

- **De grootte van het document**. Documentgrootten vergroten de eenheden als u wilt lezen of schrijven van dat gegevens meer verbruikt.
- **De eigenschap count van document**. Uitgaande standaard het indexeren van alle eigenschappen van de eenheden die zijn gebruikt om een document te schrijven zal toenemen als de eigenschap count wordt verhoogd.
- **Consistentie van de gegevens**. Wanneer u gegevens consistent niveaus van sterke- of Staleness wordt begrensd, wordt extra eenheden verbruikt om documenten te lezen.
- **Geïndexeerde eigenschappen**. Een index-beleid voor elke collectie bepaalt welke eigenschappen worden standaard geïndexeerd. Door het beperken van het aantal geïndexeerde eigenschappen of doordat de lazy indexeren, kunt u uw aanvraag eenheid consumptie verminderen.
- Het **indexeren van het document**. Elk document wordt automatisch geïndexeerd standaard verbruiken u minder eenheden van de aanvraag als u niet wilt indexeren van uw documenten.
- **Query patronen**. De complexiteit van een query heeft gevolgen voor het aantal eenheden van de aanvraag voor een bewerking worden verbruikt. Het nummer van de predikaten, aard van de predikaten, projecties, aantal UDF's en de grootte van de bron-gegevensset die alle van invloed zijn op de kosten van de query.
- **Gebruik van script**.  Net als bij query's, opgeslagen procedures en triggers aanvraag eenheden op basis van de complexiteit van de bewerkingen worden verbruikt. Inspecteer de aanvraagheader kosten beter wilt begrijpen hoe elke bewerking aanvraag eenheid capaciteit verbruikt tijdens het ontwikkelen van uw toepassing.

##<a name="estimating-throughput-needs"></a>Schatten van doorvoer moet
Een eenheid voor de aanvraag is een gestandaardiseerde meting van kosten verwerking van aanvragen. Een aanvraag voor één eenheid vertegenwoordigt de verwerkingscapaciteit nodig is om te lezen (via de self-koppeling of -id) van één 1KB JSON-document dat bestaat uit 10 unieke waarden (met uitzondering van het dialoogvenster Systeemeigenschappen). Een aanvraag maken (insert), vervangen of verwijderen van hetzelfde document verbruiken meer verwerking van de service en daardoor meer eenheden van de aanvraag.   

> [AZURE.NOTE] De basislijn van de eenheid voor een document van 1KB 1 verzoek komt overeen met een eenvoudige GET door zelf koppeling of de id van het document.

###<a name="use-the-request-unit-calculator"></a>De aanvraag eenheid Rekenmachine gebruiken
Om te helpen klanten fijn afstemmen van hun schattingen doorvoer, is er een web gebaseerd [verzoek eenheid Rekenmachine](https://www.documentdb.com/capacityplanner) bij het schatten van de aanvraag eenheid eisen voor normale bewerkingen, met inbegrip van:

- Document wordt gemaakt (schrijft)
- Document wordt gelezen
- Document wordt verwijderd
- Updates van documenten

Het hulpprogramma biedt ook ondersteuning voor het schatten van de opslagbehoeften van gegevens op basis van de voorbeelddocumenten die u opgeeft.

Met het gereedschap is eenvoudig:

1. Een of meer representatieve JSON-documenten uploaden.

    ![Documenten uploaden naar de aanvraag eenheid Rekenmachine][2]

2. Als u wilt schatten voor gegevensopslag, voer het totale aantal documenten dat u wilt opslaan.

3. Voer het nummer van het document maken, lezen, bijwerken en verwijderen van de bewerkingen die u nodig (op basis van per seconde hebt). Uploaden als u wilt inschatten van de heffingen verzoek eenheid updatebewerkingen document, een kopie van het voorbeelddocument uit stap 1 hierboven dat typische veld updates bevat.  Bijvoorbeeld, updates van documenten wijzigen meestal twee eigenschappen met de naam lastLogin en userVisits, vervolgens gewoon het voorbeelddocument kopiëren, de waarden voor deze twee eigenschappen werken als het gekopieerde document uploaden.

    ![Doorvoer vereisten in de Rekenmachine aanvraag eenheid invoeren][3]

4. Klik op berekenen en bekijk de resultaten.

    ![Eenheid Rekenmachine resultaten aanvragen][4]

>[AZURE.NOTE]Als u de documenttypen die aanzienlijk qua grootte en het aantal geïndexeerde eigenschappen verschillen, een voorbeeld van elk *type* van typische document vervolgens uploaden naar het gereedschap en vervolgens de resultaten berekenen.

###<a name="use-the-documentdb-request-charge-response-header"></a>Gebruik de DocumentDB verzoek gratis response-header
Elke reactie van de DocumentDB-service bevat een aangepaste header (x-ms-aanvraag-kosten) met de aanvraag eenheden verbruikt voor de aanvraag. Deze header is ook toegankelijk via de DocumentDB SDK's. In de SDK voor .NET is RequestCharge het een eigenschap van het object ResourceResponse.  Voor query's biedt de Explorer DocumentDB Query in de portal voor Azure gratis informatie aanvragen voor uitgevoerde query's.

![RU toeslagen in de Explorer Query controleren][1]

Met dit in gedachten, een methode voor het schatten van de hoeveelheid gereserveerde doorvoer door uw toepassing vereist is voor het vastleggen van de aanvraag eenheid kosten die worden gemaakt met de gebruikelijke bewerkingen uitgevoerd op een representatief document gebruikt door de toepassing en vervolgens het schatten van het aantal bewerkingen wilt u uitvoeren per seconde.  Zorg ervoor dat meten en gangbare query's en ook DocumentDB script gebruik.

>[AZURE.NOTE]Als u de documenttypen die aanzienlijk qua grootte en het aantal geïndexeerde eigenschappen verschillen, vervolgens de toepasselijke bewerking aanvraag eenheidstoeslag die is gekoppeld aan elk *type* van typische document opnemen.

Bijvoorbeeld:

1. De kostprijs van de eenheid aanvraag maken (invoegen) registreren een normaal document. 
2. De kosten van de aanvraag eenheid van een normaal document opnemen.
3. De kosten van de aanvraag eenheid van het bijwerken van een normaal document opnemen.
3. De kosten van de aanvraag eenheid van document gewoon, algemene query's opnemen.
4. De aanvraag eenheidstoeslag registreren van alle aangepaste scripts (opgeslagen procedures, triggers, door de gebruiker gedefinieerde functies) overgenomen door de toepassing
5. Berekenen van de aanvraag vereiste eenheden gezien het geschatte aantal bewerkingen die u verwacht dat per seconde wordt uitgevoerd.

##<a name="a-request-unit-estimation-example"></a>Een voorbeeld van een schatting aanvragen
Houd rekening met het volgende ~ 1KB-document:

    {
     "id": "08259",
    "description": "Cereals ready-to-eat, KELLOGG, KELLOGG'S CRISPIX",
    "tags": [
        {
        "name": "cereals ready-to-eat"
        },
        {
        "name": "kellogg"
        },
        {
        "name": "kellogg's crispix"
        }
    ],
    "version": 1,
    "commonName": "Includes USDA Commodity B855",
    "manufacturerName": "Kellogg, Co.",
    "isFromSurvey": false,
    "foodGroup": "Breakfast Cereals",
    "nutrients": [
        {
        "id": "262",
        "description": "Caffeine",
        "nutritionValue": 0,
        "units": "mg"
        },
        {
        "id": "307",
        "description": "Sodium, Na",
        "nutritionValue": 611,
        "units": "mg"
        },
        {
        "id": "309",
        "description": "Zinc, Zn",
        "nutritionValue": 5.2,
        "units": "mg"
        }
    ],
    "servings": [
        {
        "amount": 1,
        "description": "cup (1 NLEA serving)",
        "weightInGrams": 29
        }
    ]
    }

>[AZURE.NOTE]Documenten zijn in DocumentDB, minified, zodat het systeem berekend grootte van het bovengenoemde document is iets minder dan 1KB.


De volgende tabel ziet bij benadering aanvraag eenheid toeslagen voor normale bewerkingen op dit document (de toeslag bij benadering aanvraag eenheid wordt ervan uitgegaan dat de consistentie niveau is ingesteld op "Sessie" en alle documenten worden automatisch geïndexeerd):

Bewerking|Aanvraag eenheidstoeslag 
---|---
Document maken|~ 15 RU 
Document lezen|~ 1 RU
Query-document met id|~2.5 RU

In deze tabel ziet bij benadering aanvraag bovendien eenheid toeslagen voor gangbare query's gebruikt in de toepassing:

Query|Aanvraag eenheidstoeslag|# Geretourneerde documenten
---|---|--- 
Selecteer voedsel op id|~2.5 RU|1 
Selecteer levensmiddelen door fabrikant|~ 7 RU|7
Selecteren door de groep levensmiddelen en de volgorde van het gewicht|~ 70 RU|100
Top 10 voedingsmiddelen in een levensmiddel groep selecteren|~ 10 RU|10

>[AZURE.NOTE]RU toeslagen afhankelijk van het aantal geretourneerde documenten.

Met deze informatie kunnen we de RU-eisen voor deze toepassing is gezien het aantal activiteiten en query's dat per seconde verwachten we schatten:

Bewerking/Query|Het geschatte aantal per seconde|Vereiste RUs 
---|---|--- 
Document maken|10|150 
Document lezen|100|100 
Selecteer levensmiddelen door fabrikant|25|175 
Door voedsel groep selecteren|10|700 
Selecteer top 10|15|Totaal 150|155|1275

In dat geval verwachten we een vereiste gemiddelde doorvoer van 1,275 RU/s.  Afronden op de dichtstbijzijnde 100 zouden we 1300 RU/s voor de verzameling van de toepassing inrichten.

##<a id="RequestRateTooLarge"></a>Meer dan gereserveerde doorvoer limieten
Intrekken dat verzoek eenheid consumptie wordt geëvalueerd als een tarief per seconde. Voor toepassingen die hoger zijn dan het verzoek ingerichte eenheid voor een verzameling, aanvragen voor die collectie wordt de snelheid totdat de snelheid lager is dan het niveau van de gereserveerde. Als er een vertraging optreedt, wordt de server preemptively beëindigen van de aanvraag met RequestRateTooLargeException (HTTP-statuscode 429) en geven de x-ms-opnieuw-na-ms-header die de tijdsduur in milliseconden, die de gebruiker wachten moet alvorens nogmaals te proberen het verzoek aangeeft.

    HTTP Status 429
    Status Line: RequestRateTooLarge
    x-ms-retry-after-ms :100

Als u de .NET Client SDK en LINQ-query's en vervolgens de meeste van de tijd die u nooit omgaan met deze uitzondering, zoals de huidige versie van de SDK voor .NET Client impliciet dit antwoord van de vangsten, rekening wordt gehouden met de server opgegeven opnieuw na de header en pogingen van de aanvraag. Tenzij uw account is gelijktijdig door meerdere clients wordt geopend, wordt de volgende poging mislukt.

Als er meer dan één client cumulatief werken boven de aanvraagsnelheid, het standaardgedrag opnieuw niet toereikend zijn en de client genereert een DocumentClientException met statuscode 429 aan de toepassing. In dergelijke gevallen kunt u overwegen verwerking opnieuw gedrag en de logica in uw toepassing van fout routines voor het afhandelen of waardoor de gereserveerde doorvoer voor de collectie.

##<a name="next-steps"></a>Volgende stappen

Meer informatie over gereserveerde doorvoer met Azure DocumentDB databases, deze bronnen te verkennen:
 
- [DocumentDB prijzen](https://azure.microsoft.com/pricing/details/documentdb/)
- [DocumentDB capaciteit beheren](documentdb-manage.md) 
- [Modellering van gegevens in DocumentDB](documentdb-modeling-data.md)
- [Prestaties van DocumentDB](documentdb-partition-data.md)

Zie voor meer informatie over DocumentDB, de DocumentDB Azure- [documentatie](https://azure.microsoft.com/documentation/services/documentdb/). 

Zie [prestaties en schaal met Azure DocumentDB testen](documentdb-performance-testing.md)aan de slag met schaal en prestaties testen met DocumentDB.


[1]: ./media/documentdb-request-units/queryexplorer.png 
[2]: ./media/documentdb-request-units/RUEstimatorUpload.png
[3]: ./media/documentdb-request-units/RUEstimatorDocuments.png
[4]: ./media/documentdb-request-units/RUEstimatorResults.png
[5]: ./media/documentdb-request-units/RUCalculator2.png
