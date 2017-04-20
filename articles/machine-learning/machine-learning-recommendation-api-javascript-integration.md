<properties 
    pageTitle="Machine Learning aanbevelingen: Integratie JavaScript | Microsoft Azure" 
    description="Azure Machine Learning aanbevelingen - integratie JavaScript - documentatie" 
    services="machine-learning" 
    documentationCenter="" 
    authors="LuisCabrer" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="javascript" 
    ms.topic="article" 
    ms.date="09/08/2016" 
    ms.author="luisca"/>

# <a name="azure-machine-learning-recommendations---javascript-integration"></a>Azure Machine leren aanbevelingen - JavaScript-integratie

>[AZURE.NOTE]U moet beginnen met de cognitieve aanbevelingen API-Service in plaats van deze versie. De cognitieve Service aanbevelingen zal deze service worden vervangen en de nieuwe functies zal er worden ontwikkeld. Het heeft nieuwe mogelijkheden, zoals batchen ondersteuning, een betere API Explorer, een duidelijkere API oppervlak, consistenter aanmelden/facturering ervaring, enz.
> Meer informatie over het [migreren naar de nieuwe cognitieve Service](http://aka.ms/recomigrate)


Dit document weer het integreren van uw website met JavaScript. De JavaScript kunt u gegevens oplevert gebeurtenissen verzenden en aanbevelingen te gebruiken als u een aanbeveling model maakt. Alle bewerkingen gedaan via JS kunnen ook van de serverkant worden uitgevoerd.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

##<a name="1-general-overview"></a>1. algemene overzicht
Integratie van uw site met Azure ML aanbevelingen bestaan in 2 fasen:

1.  Gebeurtenissen verzenden in Azure ML aanbevelingen. Hierdoor kan een aanbeveling model bouwen.
2.  De aanbevelingen in beslag nemen. Nadat het model is opgebouwd, kunt u de aanbevelingen verbruiken. (Dit document wordt niet uitgelegd hoe u een model, de introductiehandleiding voor meer informatie over het lezen).


<ins>Fase I</ins>

In de eerste fase invoegen u in uw HTML-pagina's een klein JavaScript-bibliotheek waarmee u de pagina gebeurtenissen verzenden terwijl ze in de HTML-pagina in Azure ML aanbevelingen van de servers (via de markt gegevens optreden):

![Tekening1][1]

<ins>Fase II</ins>

In de tweede fase, wanneer u wilt weergeven van de aanbevelingen op de pagina selecteert u een van de volgende opties:

1. uw server (op de fase van de paginaweergave) roept Azure ML aanbevelingen Server (via de markt gegevens) om aanbevelingen krijgen. De resultaten bevatten een lijst met items-id. De server moet de resultaten met de Meta-gegevens (bijvoorbeeld afbeeldingen, beschrijving) van de artikelen te verrijken en de gemaakte pagina naar de browser verzenden.

![Drawing2][2]

2. de andere optie is het JavaScript-bestand van fase één kleine met een eenvoudige lijst met aanbevolen items opgehaald. De hier ontvangen gegevens is slanker dan de eerste optie.

![Drawing3][3]

##<a name="2-prerequisites"></a>2. voorwaarden

1. Maak een nieuw model met behulp van de API's. Zie de introductiehandleiding voor het doen.
2. Codeert de &lt;dataMarketUser&gt;:&lt;dataMarketKey&gt; met base64. (Dit wordt gebruikt voor de basisverificatie inschakelen de JS-code aan te roepen, de API's).


##<a name="3-send-data-acquisition-events-using-javascript"></a>3. verzenden gegevens oplevert gebeurtenissen met JavaScript
Gebeurtenissen verzenden vergemakkelijking van de volgende stappen uit:

1.  De bibliotheek JQuery in de code opnemen. U kunt het downloaden van nuget in de volgende URL.

        http://www.nuget.org/packages/jQuery/1.8.2
2.  Zijn de aanbevelingen Java Script bibliotheek via de volgende URL: http://aka.ms/RecoJSLib1

3.  Bibliotheek met de juiste parameters Azure ML aanbevelingen initialiseren.

        <script>
            AzureMLRecommendationsStart("<base64encoding of username:key>",
            "<model_id>");
        </script>

4.  De gebeurtenis wordt verzonden. Zie de gedetailleerde sectie hieronder op alle typen gebeurtenissen (gebeurtenis Klik op Voorbeeld van)

        <script>
            if (typeof AzureMLRecommendationsEvent=="undefined") {      
                        AzureMLRecommendationsEvent = [];
                    }
            AzureMLRecommendationsEvent.push({ event: "click", item: "18321116" });
        </script>


###<a name="31-limitations-and-browser-support"></a>3.1. Beperkingen en browserondersteuning
Dit is een referentie-implementatie en krijgt deze zoals is. Het moet alle belangrijke browsers ondersteunen.

###<a name="32-type-of-events"></a>3.2. Type gebeurtenissen
Er zijn 5 typen gebeurtenis die de bibliotheek ondersteunt: klikt, aanbeveling, toevoegen aan winkelwagen Shop, van inkoop en Shop winkelwagen verwijderen. Er is een extra gebeurtenis die wordt gebruikt voor het instellen van de gebruikerscontext aanmelding genoemd.

####<a name="321-click-event"></a>3.2.1. Click, gebeurtenis
Deze gebeurtenis moet worden gebruikt wanneer een gebruiker op een item klikt. Meestal wanneer de gebruiker klikt op een item wordt een nieuwe pagina geopend met de gegevens; Deze gebeurtenis moet worden gestart op deze pagina.

Parameters:
- gebeurtenis (string, verplichte) - "Klik op"
- de unieke id van het item (string, verplichte) - artikel
- Itemnaam (string, optioneel) - de naam van het artikel
- itemDescription (string, optioneel) - de omschrijving van het artikel
- itemCategory (string, optioneel) - de categorie van het artikel
        
        <script>
            if (typeof AzureMLRecommendationsEvent == "undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({ event: "click", item: "3111718" });
        </script>

Of met optionele gegevens:

        <script>
            if (typeof AzureMLRecommendationsEvent === "undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({event: "click", item: "3111718", itemName: "Plane", itemDescription: "It is a big plane", itemCategory: "Aviation"});
        </script>


####<a name="322-recommendation-click-event"></a>3.2.2. Aanbeveling klikt u op de gebeurtenis
Deze gebeurtenis moet worden gebruikt wanneer een gebruiker op een item dat is ontvangen van Azure ML aanbevelingen als een aanbevolen artikel geklikt. Meestal wanneer de gebruiker klikt op een item wordt een nieuwe pagina geopend met de gegevens; Deze gebeurtenis moet worden gestart op deze pagina.

Parameters:
- gebeurtenis (string, verplichte) - "recommendationclick"
- de unieke id van het item (string, verplichte) - artikel
- Itemnaam (string, optioneel) - de naam van het artikel
- itemDescription (string, optioneel) - de omschrijving van het artikel
- itemCategory (string, optioneel) - de categorie van het artikel
- zaden (string-matrix, optioneel) - de zaden die de aanbeveling query gegenereerd.
- recoList (string-matrix, optioneel) - het resultaat van de aanvraag van de aanbeveling dat het item waarop is geklikt wordt gegenereerd.
        
        <script>
            if (typeof AzureMLRecommendationsEvent=="undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({event: "recommendationclick", item: "18899918" });
        </script>

Of met optionele gegevens:

        <script>
            if (typeof AzureMLRecommendationsEvent == "undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({ event: eventName, item: "198", itemName: "Plane2", itemDescription: "It is a big plane2", itemCategory: "Default2", seeds: ["Seed1", "Seed2"], recoList: ["199", "198", "197"]               });
        </script>


####<a name="323-add-shopping-cart-event"></a>3.2.3. Winkelwagen winkelwagen-gebeurtenis toevoegen
Deze gebeurtenis moet worden gebruikt wanneer de gebruiker een item toevoegen aan de winkelwagen.
Parameters:
* gebeurtenis (string, verplichte) - "addshopcart"
* de unieke id van het item (string, verplichte) - artikel
* Itemnaam (string, optioneel) - de naam van het artikel
* itemDescription (string, optioneel) - de omschrijving van het artikel
* itemCategory (string, optioneel) - de categorie van het artikel
        
        <script>
            if (typeof AzureMLRecommendationsEvent == "undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({event: "addshopcart", item: "13221118" });
        </script>

####<a name="324-remove-shopping-cart-event"></a>3.2.4. Shopping Cart gebeurtenis verwijderen
Deze gebeurtenis moet worden gebruikt wanneer de gebruiker een item aan de winkelwagen verwijderd.

Parameters:
* gebeurtenis (string, verplichte) - "removeshopcart"
* de unieke id van het item (string, verplichte) - artikel
* Itemnaam (string, optioneel) - de naam van het artikel
* itemDescription (string, optioneel) - de omschrijving van het artikel
* itemCategory (string, optioneel) - de categorie van het artikel
        
        <script>
            if (typeof AzureMLRecommendationsEvent=="undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({ event: "removeshopcart", item: "111118" });
        </script>

####<a name="325-purchase-event"></a>3.2.5. Inkoop, gebeurtenis
Deze gebeurtenis moet worden gebruikt wanneer de gebruiker zijn winkelwagen gekocht.

Parameters:
* gebeurtenis (string) - 'aanschaft'
* artikelen (ingekochte []) - matrix die een vermelding voor elk object gekocht.<br><br>
Ingekochte indeling:
    * item (string) - de unieke id van het item.
    * aantal (int of string) - aantal artikelen die zijn gekocht.
    * prijs (float of tekenreeks) - optioneel veld: de prijs van het artikel.

Hieronder ziet u de aankoop van 3 artikelen (33, 34, 35), twee met alle velden ingevuld (artikel, aantal en prijs) en een (artikel 34) zonder een prijs.

        <script>
            if ( typeof AzureMLRecommendationsEvent == "undefined"){ AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({ event: "purchase", items: [{ item: "33", count: "1", price: "10" }, { item: "34", count: "2" }, { item: "35", count: "1", price: "210" }] });
        </script>

####<a name="326-user-login-event"></a>3.2.6. Gebruikersgebeurtenis-aanmelding
Azure ML aanbevelingen gebeurtenis bibliotheek wordt gemaakt en identificatie van gebeurtenissen die afkomstig van dezelfde browser is een cookie worden gebruikt. Ter verbetering van de resultaten van het model Azure ML aanbevelingen kunnen voor het instellen van een unieke id van de gebruiker die het gebruik van de cookie zal overschrijven.

Deze gebeurtenis moet worden gebruikt na de gebruikersaanmelding naar uw site.

Parameters:
* gebeurtenis (string) - "userlogin"
* gebruiker (string) - de unieke id van de gebruiker.
        <script>
  Als (typeof AzureMLRecommendationsEvent == 'undefined') {AzureMLRecommendationsEvent = [];} AzureMLRecommendationsEvent.push ({gebeurtenis: "userlogin," gebruiker: "ABCD10AA"});      </script>

##<a name="4-consume-recommendations-via-javascript"></a>4. aanbevelingen via JavaScript verbruiken
De code die de aanbeveling verbruikt wordt door van de client webpagina door sommige gebeurtenis JavaScript geactiveerd. Het antwoord van de aanbeveling bevat de id van de aanbevolen items, hun namen en hun classificaties. Het is raadzaam deze optie alleen gebruiken voor een lijst weergeven van de aanbevolen artikelen - complexe behandelingen (zoals het toevoegen van metagegevens van het item) moet worden uitgevoerd op de server-side-integratie.

###<a name="41-consume-recommendations"></a>4.1 verbruiken aanbevelingen
In beslag neemt zijn aanbevelingen u moet de benodigde JavaScript-bibliotheken in uw pagina en AzureMLRecommendationsStart aanroepen. Zie hoofdstuk 2.

Aanbevelingen voor een of meer items die u moet aanroepen van een methode met de naam gebruiken: AzureMLRecommendationsGetI2IRecommendation.

Parameters:
* (matrix van tekenreeksen) - een of meer items om aanbevelingen voor de artikelen. Als u een versie Fbt verbruiken en u kunt hier slechts één item instellen.
* numberOfResults (int) - aantal vereiste resultaten.
* includeMetadata (boolean, optioneel) - als is ingesteld op 'true' geeft aan dat het veld met metagegevens in het resultaat moet worden ingevuld.
* Verwerking van de functie, een functie waarmee de aanbevelingen worden verwerkt als resultaat gegeven. De gegevens wordt geretourneerd als een matrix van:
    * Artikel - artikel unieke id
    * naam - item naam (indien aanwezig in catalogus)
    * beoordeling - beoordeling van de aanbeveling
    * metagegevens - een tekenreeks waarmee de metagegevens van het item

Voorbeeld: De volgende code aanvragen 8 aanbevelingen voor het item '64f6eb0d-947a-4c18-a16c-888da9e228ba' (en door te geven niet includeMetadata - het impliciet zegt dat er geen metagegevens vereist is), dat vervolgens de resultaten in een buffer wilt samenvoegen.

        <script>
            var reco = AzureMLRecommendationsGetI2IRecommendation(["64f6eb0d-947a-4c18-a16c-888da9e228ba"], 8, false, function (reco) {
                var buff = "";
                for (var ii = 0; ii < reco.length; ii++) {
                    buff += reco[ii].item + "," + reco[ii].name + "," + reco[ii].rating + "\n";
                }
                alert(buff);
            });
        </script>


[1]: ./media/machine-learning-recommendation-api-javascript-integration/Drawing1.png
[2]: ./media/machine-learning-recommendation-api-javascript-integration/Drawing2.png
[3]: ./media/machine-learning-recommendation-api-javascript-integration/Drawing3.png
 
