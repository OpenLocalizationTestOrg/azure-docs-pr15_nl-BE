<properties
    pageTitle="Taken voor het voorbereiden van gegevens voor verbeterde machine learning | Microsoft Azure"
    description="Vooraf verwerken en gegevens voorbereiden machine learning opschonen."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016" 
    ms.author="bradsev" />


# <a name="tasks-to-prepare-data-for-enhanced-machine-learning"></a>Taken voor het voorbereiden van gegevens voor verbeterde machine learning

Vooraf en schoonmaken van de gegevens zijn belangrijke taken die normaal gesproken moeten worden uitgevoerd voordat de dataset effectief kan worden gebruikt voor de machine learning. Onbewerkte gegevens is vaak veel ruis en onbetrouwbare en waarden ontbreekt. Voor modellering met behulp van deze gegevens kan misleidende resultaten opleveren. Deze taken maken deel uit van het Team gegevens wetenschap proces (TDSP) en doorgaans als volgt een eerste exploratie van een dataset die is gebruikt om te ontdekken en van plan bent de voorbehandeling nodig. Zie voor meer gedetailleerde instructies voor het TDSP-proces, de stappen in de [Procedure voor het Team gegevens wetenschap](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).

Voorbehandeling en reiniging van taken, zoals het de taak van de gegevens te verkennen, kunnen worden uitgevoerd in tal van omgevingen, zoals SQL of component of Azure Machine Learning Studio en met verschillende hulpprogramma's en talen, zoals R of Python, afhankelijk van waar uw gegevens worden opgeslagen en hoe deze wordt geformatteerd. Aangezien TDSP iteratieve karakter, kunnen deze taken plaatsvinden bij de verschillende stappen in de workflow van het proces.

Dit artikel bevat verschillende gegevensverwerking concepten en taken die kunnen worden uitgevoerd voor of na de ingesting gegevens in Azure Machine Learning.

Zie voor een voorbeeld van de gegevens te verkennen en voorbehandeling gedaan in Azure Machine Learning studio, het [vooraf verwerken van gegevens in Azure Machine Learning Studio](https://azure.microsoft.com/documentation/videos/preprocessing-data-in-azure-ml-studio/) video.


## <a name="why-pre-process-and-clean-data"></a>Waarom vooraf verwerken en gegevens opschonen?

Praktische gegevens verzameld uit verschillende bronnen en processen en onregelmatigheden of beschadigde gegevens in gevaar te brengen in de kwaliteit van de dataset kunnen bevatten. De typische kwaliteitsproblemen die ontstaan zijn:

* **Onvolledig**: gegevens bevat geen kenmerken of met ontbrekende waarden.
* **Noisy**: gegevens onjuiste records bevat of uitschieters.
* **Inconsistente**: gegevens conflicterende records bevat of verschillen.

Kwaliteitsgegevens is een voorwaarde voor kwaliteit voorspellende modellen. Om te voorkomen "garbage in, garbage out" en de kwaliteit van de gegevens te verbeteren en daarom model prestaties, is het absoluut noodzakelijk een scherm gezondheid vroegtijdig te herkennen die problemen met gegevens en een besluit over de corresponderende gegevens verwerken en reinigen stappen uitvoeren.

## <a name="what-are-some-typical-data-health-screens-that-are-employed"></a>Wat zijn enkele typische gegevens gezondheid schermen die werkzaam zijn?

We kunnen de algemene kwaliteit van de gegevens controleren door te controleren:

* Het aantal **records**.
* Het aantal **kenmerken** (of **onderdelen**).
* De kenmerk **-gegevenstypen** (nominale, ordinale of continu).
* Het aantal **waarden ontbreken**.
* **-Opmaak** van de gegevens.
    * Als de gegevens in CSV- of de TSV, Controleer dat de kolom scheidingstekens en scheidingstekens voor de regel altijd goed kolommen en regels gescheiden.
    * Als de gegevens in HTML- of XML-indeling, moet u controleren of de gegevens is goed gevormd op basis van hun respectieve normen.
    * Parseren kan ook nodig zijn om gestructureerde gegevens extraheren uit semi-gestructureerde of ongestructureerde gegevens.
* **Records met inconsistente gegevens**. Controleer het bereik van waarden zijn toegestaan. bijvoorbeeld als de gegevens student GPA, controleren of de GPA in het opgegeven bereik is zeggen 0 ~ 4.

Als u problemen met gegevens, **verwerken stappen** nodig zijn die vaak gebruikt door reiniging ontbrekende waarden, gegevens normaliseren, afzondering, tekst verwerkt om te verwijderen en/of vervangen door de ingesloten tekens die van invloed kunnen zijn op de gegevensuitlijning, gemengde gegevenstypen gemeenschappelijke velden en anderen.

**Azure Machine Learning juist opgemaakte gegevens in tabelvorm worden verbruikt**.  Als de gegevens al in tabelvorm, kan voorbehandeling gegevens rechtstreeks met Azure Machine Learning in de Studio van Machine Learning worden uitgevoerd.  Als de gegevens niet in tabelvorm in XML, zeg kan parseren nodig zijn om het omzetten van de gegevens in tabelvorm.  

## <a name="what-are-some-of-the-major-tasks-in-data-pre-processing"></a>Wat zijn enkele van de belangrijkste taken in de voorbehandeling van gegevens?

* **Gegevens opschonen**: invullen of ontbrekende waarden detecteren en verwijderen van ruis gegevens en uitschieters.
* **Data transformation**: normaliseren van gegevens voor dimensies en Ruis reduceren.
* **Data reductie**: voorbeeld van records met gegevens of kenmerken voor gemakkelijker gegevens verwerken.
* **Gegevens afzondering**: omzetten in continue kenmerken bestaan kenmerken voor gebruiksgemak met een bepaalde machine learning methoden.
* **Tekst schoonmaken**: Verwijder ingesloten tekens waardoor de uitlijning van gegevens, bijvoorbeeld ingesloten tabbladen in een gegevensbestand met door tabs gescheiden ingesloten nieuwe regels die mogelijk worden verbroken records, enz.

In de volgende secties bevatten informatie over enkele van deze verwerkingsstappen van gegevens.

## <a name="how-to-deal-with-missing-values"></a>Het omgaan met ontbrekende waarden?

Deal met waarden ontbreken, is het raadzaam eerst de reden voor de ontbrekende waarden beter wordt verwerkt het probleem te achterhalen. Typische ontbrekende waarde hanteren methoden zijn:

* **Verwijderen**: verwijderen van records met ontbrekende waarden
* **Vervanging van de pop**: ontbrekende waarden vervangen door een dummy-waarde: bijvoorbeeld, _Onbekend_ voor bestaan of 0 voor numerieke waarden.
* **Substitutie betekent**: als de ontbrekende gegevens numerieke, vervangen door de ontbrekende waarden het gemiddelde.
* **Regelmatig vervangen**: als u de ontbrekende gegevens bestaan, de ontbrekende waarden vervangen met de meest voorkomende item
* **Vervanging van regressie**: een regressiemethode gebruiken om de ontbrekende waarden vervangen door waarden opgelost.  

## <a name="how-to-normalize-data"></a>Hoe kan ik gegevens normaliseren?

Gegevensnormalisatie wordt opnieuw numerieke waarden op een bepaald paginabereik. Populaire gegevens normaliseren methoden omvatten:

* **Min Max normalisatie**: Lineair transformeren naar een bereik, zeg tussen 0 en 1, waarbij de minimale waarde moet worden geschaald naar 0 en maximaal de waarde 1.
* **Normalisatie van de Z-score**: schalen op basis van gemiddelde en standaarddeviatie: het verschil tussen de gegevens en het gemiddelde wordt gedeeld door de standaarddeviatie.
* **Schalen van decimale**: schalen van de gegevens door het verplaatsen van het decimaalteken van de waarde van het kenmerk.  

## <a name="how-to-discretize-data"></a>Hoe gegevens moeten worden onderscheiden?

Gegevens kunnen worden discretized door continue waarden omzetten in nominale kenmerken of intervallen. Er zijn enkele manieren waarop dit te doen:

* **Gelijke breedte Binning**: het bereik van alle mogelijke waarden van een kenmerk verdelen in groepen van N van dezelfde grootte en de waarden die niet in een opslaglocatie toewijzen met het opslaglocatienummer van de.
* **Gelijke hoogte Binning**: het bereik van alle mogelijke waarden van een kenmerk verdelen in groepen van N, elk met hetzelfde aantal exemplaren en vervolgens de waarden die niet in een opslaglocatie toewijzen met het opslaglocatienummer van de.  

## <a name="how-to-reduce-data"></a>Het verminderen van gegevens?

Er zijn verschillende manieren verkleinen van gegevens gemakkelijker gegevens verwerken. Afhankelijk van de gegevensgrootte en het domein, kunnen de volgende methoden worden toegepast:

* **Bemonstering van record**: voorbeeld van de records met gegevens en kies alleen de representatieve subset van de gegevens.
* **Bemonstering van kenmerk**: Selecteer alleen een subset van de belangrijkste kenmerken van de gegevens.  
* **Samenvoeging**: de gegevens in groepen te verdelen en opslaan van de nummers voor elke groep. Bijvoorbeeld kan de dagelijkse omzet nummers van een keten restaurant in de afgelopen 20 jaar worden samengevoegd om de maandelijkse inkomsten te verminderen van de grootte van de gegevens.  

## <a name="how-to-clean-text-data"></a>Hoe kan ik tekstgegevens opschonen?

**Tekstvelden in gegevens in tabelvorm** , kunnen tekens die gevolgen hebben voor de uitlijning en/of grenzen van kolommen bevatten. Voor bijvoorbeeld tabbladen in een door tabs gescheiden bestand oorzaak kolom uitlijning ingesloten en ingesloten tekens voor nieuwe regels worden verbroken regels vastleggen. Codering verwerken van onjuiste tekst tijdens het schrijven/lezen tekst leidt tot gegevensverlies, onopzettelijke introductie van onleesbare tekens bijvoorbeeld nulwaarden en mogelijk ook van invloed zijn op tekst te parseren. Voorzichtig bij het parseren en bewerken mogelijk niet vereist voor het reinigen van de tekstvelden voor de juiste uitlijning en/of uittreksel gestructureerde gegevens uit niet-gestructureerde of semi-gestructureerde gegevens.

**Verkennen** biedt een snelle weergave in de gegevens. Een aantal problemen met gegevens vanuit een ongedekte positie kan worden tijdens deze stap en de overeenkomstige methoden kunnen worden toegepast om deze problemen op te lossen.  Het is belangrijk om te vragen, zoals wat de oorzaak van het probleem is en hoe het probleem kan zijn binnengebracht. Dit kunt u ook een besluit over de gegevensverwerking stappen die worden genomen moeten, deze op te lossen. Het soort inzichten een voornemens is afgeleid van de gegevens kan ook worden gebruikt om de prioriteit van de gegevensverwerking inspanning te.

## <a name="references"></a>Verwijzingen

>*Datamining: concepten en technieken*, derde editie, Morgan Kaufmann, 2011, Jiawei Han en Micheline Kamber Jian Pei
