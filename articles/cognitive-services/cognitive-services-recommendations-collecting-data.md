<properties
    pageTitle="Het verzamelen van gegevens om te trainen uw Model: Machine Learning aanbevelingen API | Microsoft Azure"
    description="Azure Machine Learning aanbevelingen - gegevens verzamelt om te trainen uw Model"
    services="cognitive-services"
    documentationCenter=""
    authors="luiscabrer"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="cognitive-services"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="luisca"/>

#  <a name="collecting-data-to-train-your-model"></a>Het verzamelen van gegevens naar het Model van de trein #

De API aanbevelingen hoort van je laatste transacties te vinden welke items moeten worden aanbevolen voor een bepaalde gebruiker.

Nadat u een model hebt gemaakt, moet u twee stukje informatie opgeven voordat u een training kunt doen: een catalogusbestand en gebruiksgegevens.

>   Als u nog niet gedaan hebt, raden we u aan het voltooien van de [handleiding Snel starten](cognitive-services-recommendations-quick-start.md).


## <a name="catalog-data"></a>Gegevens in de catalogus ##

### <a name="catalog-file-format"></a>Catalogus-bestandsindeling ###

Het catalogusbestand bevat informatie over de items die u aan uw klant aanbiedt.
De gegevens in de catalogus moet de volgende notatie volgen:

- Zonder functies:`<Item Id>,<Item Name>,<Item Category>[,<Description>]`

- Met de functies:`<Item Id>,<Item Name>,<Item Category>,[<Description>],<Features list>`

#### <a name="sample-rows-in-a-catalog-file"></a>Rijen in een catalogusbestand monster

Zonder de functies:

    AAA04294,Office Language Pack Online DwnLd,Office
    AAA04303,Minecraft Download Game,Games
    C9F00168,Kiruna Flip Cover,Accessories

Met de functies:

    AAA04294,Office Language Pack Online DwnLd,Office,, softwaretype=productivity, compatibility=Windows
    BAB04303,Minecraft DwnLd,Games, softwaretype=gaming,, compatibility=iOS, agegroup=all
    C9F00168,Kiruna Flip Cover,Accessories, compatibility=lumia,, hardwaretype=mobile

#### <a name="format-details"></a>Opmaak details

| Naam | Verplicht | Type |  Beschrijving |
|:---|:---|:---|:---|
| Item-Id |Ja | [A-z], [a-z], [0-9] [_] & #40; Onderstrepingsteken & #41; [-] & #40; streepje & #41;<br> Maximale lengte: 50 | De unieke id van een item. |
| Naam van item | Ja | Alle alfanumerieke tekens<br> Maximale lengte: 255 | De naam van het item. |
| Artikelcategorie | Ja | Alle alfanumerieke tekens <br> Maximale lengte: 255 | Categorie waartoe dit artikel behoort (bijvoorbeeld koken boeken, Drama...); kan niet leeg zijn. |
| Beschrijving | Nee, tenzij er voorzieningen zijn aanwezig (maar kan niet leeg zijn) | Alle alfanumerieke tekens <br> Maximale lengte: 4000 | Omschrijving van dit artikel. |
| Lijst met functies | Nee | Alle alfanumerieke tekens <br> Maximale lengte: 4000; Het maximumaantal functies: 20 | Door komma's gescheiden lijst met de functienaam = functie waarde die kan worden gebruikt voor het verbeteren van de aanbeveling model.|

#### <a name="uploading-a-catalog-file"></a>Een catalogusbestand uploaden

De [API-naslaggids](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f316efeda5650db055a3e1) voor het uploaden van een bestand bekijken.  

Houd er rekening mee dat de inhoud van het catalogusbestand moet worden doorgegeven als het hoofdgedeelte van de aanvraag.

Als u verschillende bestanden naar hetzelfde model met verschillende aanroepen uploaden, zullen we de nieuwe catalogusitems invoegen. Bestaande items blijft met de oorspronkelijke waarden. U kunt gegevens in de catalogus niet bijwerken met behulp van deze methode.

>   Opmerking: De maximale bestandsgrootte is 200MB.
>   Het maximum aantal items in de catalogus die wordt ondersteund is 100.000 artikelen.


## <a name="why-add-features-to-the-catalog"></a>Waarom functies toevoegen aan de catalogus?

De aanbevelingen-engine maakt een statistisch model die leest u welke items kunnen worden gekocht door een klant of interessant vond. Wanneer u is een nieuw product dat is nooit zijn interactief gecommuniceerd met het niet alleen een model maken op collega exemplaren. Stel dat u aanbiedt, een nieuwe start "van kinderen violin" in uw winkel, omdat u die violin nooit verkocht hebt voordat u niet welke andere artikelen weet aan te bevelen met die violin.

Dat gezegd, als de motor informatie over die violin kent (dat wil zeggen een musical instrument is, is het voor kinderen de leeftijd 7-10, is het niet een dure violin, enz.) en vervolgens de engine van andere producten met vergelijkbare functies leren kunt. Bijvoorbeeld, u hebt de violin in het verleden verkocht en meestal mensen die violen kopen vaak klassieke muziek-cd's en Bladmuziek statieven kopen.  Het systeem kan deze verbindingen tussen de functies zoeken en aanbevelingen op basis van de functies, terwijl de nieuwe violin weinig gebruik heeft opgeven.

Functies als onderdeel van de gegevens in de catalogus worden geïmporteerd, en vervolgens hun rang (of het belang van de functie in het model) is gekoppeld als een absolute build voltooid is. Functie rang kunt volgens het patroon van gegevens over het gebruik en het type items wijzigen. Maar voor het consistente gebruik/artikelen de rang moet slechts kleine schommelingen. De rang van functies is een niet-negatief getal. Het getal 0 houdt in dat de functie niet eindigde (als u deze API voor de voltooiing van de eerste rang build gebeurt). De datum waarop de rang te wijten is, wordt de score versheid genoemd.


###<a name="features-are-categorical"></a>Functies zijn Categorical

Dit betekent dat u functies die lijken op een categorie te maken. Bijvoorbeeld prijs = 9.34 wordt niet bestaan. Aan de andere kant, een functie zoals priceRange = Under5Dollars is een functie bestaan. Een andere veelvoorkomende fout is de naam van het item als een functie te gebruiken. Hierdoor zou de naam van een item uniek te zijn zodat het zou niet een categorie beschrijven. Zorg ervoor dat de functies zijn categorieën met items.


###<a name="how-manywhich-features-should-i-use"></a>Hoe veel of waarvan onderdelen moet ik gebruiken?


Uiteindelijk maken de aanbevelingen voor het bouwen van een model met maximaal 20 functies ondersteunt. Kan u meer dan 20 functies toewijzen aan de artikelen in de catalogus, maar u moet een rangorde build doen en kies alleen de functies die hoge rang. (Een onderdeel met de rang van 2.0 of meer is een goede functie om te gebruiken!). 


###<a name="when-are-features-actually-used"></a>Wanneer functies daadwerkelijk worden toegepast?

Functies worden gebruikt door het model als er niet voldoende gegevens voor aanbevelingen op transactiegegevens alleen. Functies worden dus de grootste invloed hebben op 'koude items' – artikelen met minder transacties. Als alle items voldoende transactie-informatie niet wilt verrijken uw model met functies zijn.


###<a name="using-product-features"></a>Met behulp van functies

Functies als onderdeel van uw build u moet gebruiken:

1. Zorg ervoor dat uw catalogus functies wanneer u het uploadt.

2. Een rangschikking build-trigger. Dit doet de analyses van de urgentie/rang van de functies.

3. Een build aanbevelingen activeren, het instellen van de volgende parameters maken: useFeaturesInModel ingesteld op true, allowColdItemPlacement op true, en modelingFeatureList moet worden ingesteld op de door komma's gescheiden lijst met functies die u gebruiken wilt voor het verbeteren van uw model. Zie [aanbevelingen bouwen typeparameters](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f30d77eda5650db055a3d0) voor meer informatie.





## <a name="usage-data"></a>Gebruiksgegevens ##
Een gebruik-bestand bevat informatie over hoe deze items worden gebruikt, of de transacties van uw bedrijf.

#### <a name="usage-format-details"></a>Details van gebruik-indeling
Een gebruik bestand is een CSV (door komma's gescheiden waarde) waarbij elke rij in een bestand met gebruik van een interactie tussen een gebruiker en een item vertegenwoordigt. Elke rij wordt als volgt ingedeeld:<br>
`<User Id>,<Item Id>,<Time>,[<Event>]`



| Naam  | Verplicht | Type | Beschrijving
|-------|------------|------|---------------
|Gebruikers-Id|         Ja|[A-z], [a-z], [0-9] [_] & #40; Onderstrepingsteken & #41; [-] & #40; streepje & #41;<br> Maximale lengte: 255 |De unieke id van een gebruiker.
|Item-Id|Ja|[A-z], [a-z], [0-9] [& #95;] & #40; Onderstrepingsteken & #41; [-] & #40; streepje & #41;<br> Maximale lengte: 50|De unieke id van een item.
|Tijd|Ja|Datum in de notatie: jjjj/MM/ddTUU (b.v. 2013/06/20T10:00:00)|Tijd van de gegevens.
|Gebeurtenis|Nee | Een van de volgende handelingen uit:<br>• Klik op<br>• RecommendationClick<br>• AddShopCart<br>• RemoveShopCart<br>• Inkoop| Het type transactie. |

#### <a name="sample-rows-in-a-usage-file"></a>Rijen in een bestand gebruik monster

    00037FFEA61FCA16,288186200,2015/08/04T11:02:52,Purchase
    0003BFFDD4C2148C,297833400,2015/08/04T11:02:50,Purchase
    0003BFFDD4C2118D,297833300,2015/08/04T11:02:40,Purchase
    00030000D16C4237,297833300,2015/08/04T11:02:37,Purchase
    0003BFFDD4C20B63,297833400,2015/08/04T11:02:12,Purchase
    00037FFEC8567FB8,297833400,2015/08/04T11:02:04,Purchase

#### <a name="uploading-a-usage-file"></a>Uploaden van een bestand gebruik

Kijk naar de [API-naslaggids](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f316efeda5650db055a3e2) voor het uploaden van bestanden in gebruik.
Opmerking u moet de inhoud van het bestand voor gebruik als het hoofdgedeelte van de HTTP-oproep doorgeven.

>  Opmerking:

>  Maximale bestandsgrootte: 200MB. U kunt verschillende gebruik bestanden kan uploaden.

>  U moet een catalogusbestand uploaden voordat u de gegevens toe te voegen aan uw model. Alleen items in het catalogusbestand wordt gebruikt tijdens de fase van de opleiding. Alle andere items worden genegeerd.

## <a name="how-much-data-do-you-need"></a>Hoeveel gegevens hebt u nodig?

De kwaliteit van het model is sterk afhankelijk van de kwaliteit en kwantiteit van de gegevens.
Leert het systeem wanneer gebruikers verschillende artikelen kopen (We noemen dit CO exemplaren). FBT opbouw is het ook belangrijk om te weten welke artikelen worden ingekocht in dezelfde transacties. 

Een goede vuistregel is dat de meeste items in de transacties van 20 of meer, dus als u 10.000 items in uw catalogus had, zou raden u ten minste 20-maal het aantal transacties of transacties van ongeveer 200.000. Nogmaals, is dit een vuistregel. U wilt experimenteren met uw gegevens.

Als u een model hebt gemaakt, kunt u uitvoeren om te controleren hoe goed uw model dreigt uit te voeren [evaluatie off line](cognitive-services-recommendations-buildtypes.md) .
