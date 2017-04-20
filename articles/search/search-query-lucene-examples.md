<properties
    pageTitle="Voorbeelden van Lucene query voor Azure zoeken | Microsoft Azure zoeken"
    description="Lucene query-syntaxis voor het zoeken bij benadering nabijheid zoeken, waarbij de term, reguliere expressie zoeken en zoeken met jokertekens."
    services="search"
    documentationCenter=""
    authors="LiamCa"
    manager="pablocas"
    editor=""
    tags="Lucene query analyzer syntax"
/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="liamca"
/>

# <a name="lucene-query-syntax-examples-for-building-queries-in-azure-search"></a>Voorbeelden van Lucene query-syntaxis voor het maken van query's in Azure zoeken

Bij het maken van query's voor Azure zoeken, kunt u de standaard [eenvoudige query-syntaxis](https://msdn.microsoft.com/library/azure/dn798920.aspx) of de alternatieve [Lucene Query Parser in Azure zoeken](https://msdn.microsoft.com/library/azure/mt589323.aspx). De Query Lucene Parser ondersteunt complexe query constructies, zoals query's binnen het bereik van veld, fuzzy zoeken nabijheid zoeken, waarbij de term en reqular expressie zoeken.

In dit artikel, kunt u stapsgewijs door voorbeelden van Lucene query-syntaxis en de resultaten naast elkaar weergeven. Voorbeelden uitvoeren op een vooraf geladen zoekindex in [JSFiddle](https://jsfiddle.net/), een online code editor voor het testen van scripts en HTML. 

Klik met de rechtermuisknop op de query voorbeeld-URL's te JSFiddle in een apart browservenster openen.

> [AZURE.NOTE] De volgende voorbeelden maken gebruik van een zoekindex die bestaat uit de functies die beschikbaar zijn op basis van een dataset die door het initiatief van de [Stad New York OpenData](https://nycopendata.socrata.com/) . Deze gegevens moet niet beschouwd als huidige of voltooid. De index is in een sandbox-service van Microsoft. U hoeft niet een Azure-abonnement of Azure Search om te proberen deze query's.

## <a name="viewing-the-examples-in-this-article"></a>De voorbeelden in dit artikel te bekijken

Alle voorbeelden in dit artikel opgeven de Parser Lucene Query via de parameter**queryType** zoeken. Wanneer u de Parser Lucene Query vanuit uw code gebruikt, zult u het **queryType** op elke aanvraag.  Geldige waarden zijn **eenvoudige**|**volledig**, met **eenvoudige** als de standaard- en **volledige** voor de Query Lucene Parser. Zie [Documenten zoeken (Azure Search Service REST API)](https://msdn.microsoft.com/library/azure/dn798927.aspx) voor meer informatie over het opgeven van queryparameters.

**Voorbeeld 1** : met de rechtermuisknop op het volgende fragment om deze te openen in een nieuwe browserpagina JSFiddle geladen en wordt de query uitgevoerd query:
- [& queryType = full & zoeken = *](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26searchFields=business_title%26$select=business_title%26queryType=full%26search=*)

Deze query resulteert in documenten uit onze taken index (wordt geladen in een sandbox-service)

In dit venster ziet u de bron van de JavaScript- en HTML-uitvoer naast elkaar. Een query die wordt geleverd door de voorbeeld-URL's in dit artikel wordt verwezen naar het script. In **Voorbeeld 1**wordt is de onderliggende query bijvoorbeeld als volgt:

    http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26searchFields=business_title%26$select=business_title%26queryType=full%26search=*

Aankondiging van de query wordt een vooraf geconfigureerde Azure Search index met de naam nycjobs. De parameter **searchFields** beperkt de zoekopdracht alleen de titel op werk. Het **queryType** is ingesteld op **volledige**, zorgt u ervoor dat Azure Search de Parser Lucene Query gebruiken voor deze query.

### <a name="fielded-query-operation"></a>Fielded querybewerking.

De voorbeelden in dit artikel kunt u wijzigen door op te geven van de bouw van een **fieldname:searchterm** fielded querybewerking, waarbij het veld een woord en de zoekterm is ook een enkel woord of een woordgroep, eventueel met Boole-operators te definiëren. Enkele voorbeelden zijn de volgende:

- business_title:(senior NOT junior)
- status: ("New York" en "Nieuwe Jersey")

Zorg ervoor dat meerdere tekenreeksen tussen aanhalingstekens plaatsen als u wilt dat beide tekenreeksen moeten worden geëvalueerd als één enkele entiteit, zoals in dit geval zoeken naar twee verschillende plaatsen in het locatieveld. Controleer ook of de exploitant een hoofdletter is terwijl u niet ziet en en.

Het veld dat is opgegeven in **fieldname:searchterm** moet een veld kan worden gezocht. Zie [Create Index (Azure Search Service REST API)](https://msdn.microsoft.com/library/azure/dn798941.aspx) voor informatie over hoe indexkenmerken in velddefinities worden gebruikt.

## <a name="fuzzy-search"></a>Fuzzy zoeken

Een bij benadering gezocht overeenkomsten die een soortgelijke constructie hebben. Per [Lucene documentatie](https://lucene.apache.org/core/4_10_2/queryparser/org/apache/lucene/queryparser/classic/package-summary.html), zijn onduidelijk zoekopdrachten op basis van [Damerau-Levenshtein afstand](https://en.wikipedia.org/wiki/Damerau%e2%80%93Levenshtein_distance).

Een vaag zoeken, gebruikt u de tilde ' ~ ' symbool aan het einde van een woord met een optionele parameter, een waarde tussen 0 en 2, die de afstand bewerken. Bijvoorbeeld "blauwe ~" of "blauwe ~ 1' retourneert blauw, blauwe en lijm.

**Voorbeeld 2** : klik met de rechtermuisknop in het volgende fragment van de query naar het eens proberen. Deze query gezocht naar zakelijke titels met de term senior in hen, maar niet de leerling:

- [& queryType = full & zoeken niet business_title:senior = junior](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26$select=business_title%26queryType=full%26search=business_title:senior+NOT+junior)

## <a name="proximity-search"></a>Nabijheid zoeken

Nabijheid zoekopdrachten worden gebruikt om te zoeken naar termen die zich dichtbij elkaar bevinden in een document. Plaats een tilde ' ~ ' symbool aan het einde van een zin gevolgd door het aantal woorden die de grens dicht te maken. Bijvoorbeeld "hotel luchthaven" ~ 5 zoeken de termen hotel en de luchthaven binnen 5 woorden in een document.

**Voorbeeld 3** : klik met de rechtermuisknop in het volgende fragment van de query. Deze query gezocht naar projecten met de term koppelen (waar het is verkeerd gespeld):

- [& queryType = full & zoeken business_title:asosiate = ~](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26$select=business_title%26queryType=full%26search=business_title:asosiate~)

**Voorbeeld 4** --met de rechtermuisknop op de query. Zoeken naar projecten met de term "senior analist" waarin deze worden gescheiden door niet meer dan één woord:

- [& queryType = full & zoeken = business_title: "senior analist" ~ 1](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26$select=business_title%26queryType=full%26search=business_title:%22senior%20analyst%22~1)

**Voorbeeld 5** : probeer het nogmaals de woorden tussen de term "senior analist" verwijderen.

- [& queryType = full & zoeken = business_title: "senior analist" ~ 0](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26$select=business_title%26queryType=full%26search=business_title:%22senior%20analyst%22~0)

## <a name="term-boosting"></a>Benaming prestatieverbetering

Verhoging van de term verwijst naar een hogere bevat de term gestimuleerd ten opzichte van de documenten met niet de term document rangorde. Dit verschilt van het scoren van profielen in deze score profielen bepaalde velden in plaats van specifieke voorwaarden stimuleren. In het volgende voorbeeld kunt u de verschillen laten zien.

U kunt een score profiel waarmee in een bepaald veld, bijvoorbeeld **genre** in het voorbeeld musicstoreindex komt overeen met. Verhoging van de termijn kan worden gebruikt om bepaalde zoekopdracht verder verhogen hoger is dan de andere voorwaarden. Bijvoorbeeld "rock ^ 2 elektronische" zal stimuleren documenten met de zoektermen in het veld **genre** hoger dan andere doorzoekbare velden in de index. Voorts documenten met de zoekterm "rock" zal worden krijgen een hogere rang dan andere zoekterm "elektronische" als gevolg van de term boost waarde (2).

Stimuleren van een term, gebruikt u de caret, "^", symbool met een factor van boost (getal) aan het einde van de term die u zoekt. Hoe hoger de boost-factor, de relevante zal worden ten opzichte van andere zoektermen. De boost-factor is 1. Hoewel de factor boost positief zijn moet, kan het zijn dat kleiner is dan 1 (bijvoorbeeld 0,2).

**Voorbeeld 6** --met de rechtermuisknop op de query. Zoeken naar projecten met de term "computer analist" waar wij er zijn geen resultaten met zowel woorden als analist, maar de analist taken worden aan de bovenkant van de resultaten zien.

- [& queryType = full & zoeken = business_title:computer analist](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26$select=business_title%26queryType=full%26search=business_title:computer%5e2%20analyst)

**Voorbeeld 7** : probeer het opnieuw, deze keer verhogen resultaten met de term computer via de term analist als beide woorden bestaan niet.

- [& queryType = full & zoeken business_title:computer = ^ 2 analist](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26$select=business_title%26queryType=full%26search=business_title:computer%5e2%20analyst)

## <a name="regular-expression"></a>Reguliere expressie

Een reguliere expressie zoeken gevonden op basis van de inhoud tussen een slash '/', als beschreven in de [klasse RegExp](http://lucene.apache.org/core/4_10_2/core/org/apache/lucene/util/automaton/RegExp.html).

**Voorbeeld 8** --met de rechtermuisknop op de query. Zoeken naar projecten met hetzij de term Senior of Junior.

- `&queryType=full&$select=business_title&search=business_title:/(Sen|Jun)ior/`

De URL in dit voorbeeld wordt de pagina niet goed opgebouwd. De onderstaande URL kopiëren en plakken in het URL-adres browser als tijdelijke oplossing:    `http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26queryType=full%26$select=business_title%26search=business_title:/(Sen|Jun)ior/)`


## <a name="wildcard-search"></a>Zoeken met jokertekens

U kunt in het algemeen erkende syntaxis gebruiken voor meerdere (\*) of enkele zoekopdrachten met jokertekens teken (?). Opmerking de query Lucene parser ondersteunt het gebruik van deze symbolen met een enkele term en niet een zin.

**Voorbeeld 9** --met de rechtermuisknop op de query. Zoeken naar taken die het voorvoegsel "prog", waaronder zakelijke titels met de voorwaarden programmeren en de programmer erin zou bevatten.

- [& queryType = volledige & $select = business_title & zoeken = business_title:prog*](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26queryType=full%26$select=business_title%26search=business_title:prog*)

U kunt niet gebruiken een * of? symbool als het eerste teken van een zoekopdracht.


## <a name="next-steps"></a>Volgende stappen

Probeer de Parser Lucene Query op te geven in uw code. De volgende koppelingen wordt uitgelegd hoe zoekquery's instellen voor .NET en de REST-API. De koppelingen gebruiken de standaardsyntaxis voor eenvoudige zodat u geleerd van dit artikel hebt geeft het **queryType**van toepassing.

- [Uw Azure Search-Index met de .NET SDK opvragen](search-query-dotnet.md)
- [Uw Azure Search-Index met de REST API query](search-query-rest-api.md)


 