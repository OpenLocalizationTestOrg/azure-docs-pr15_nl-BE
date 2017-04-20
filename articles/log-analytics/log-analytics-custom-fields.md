<properties
   pageTitle="Aangepaste velden in het logboek Analytics | Microsoft Azure"
   description="De functie voor aangepaste velden van Analytics logboek kunt u uw eigen doorzoekbare velden maken van OMS-gegevens aan de eigenschappen van een record verzameld toevoegen.  In dit artikel wordt beschreven hoe u een aangepast veld maken en een gedetailleerd overzicht geeft een voorbeeld van de gebeurtenis."
   services="log-analytics"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="log-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="bwren" />

# <a name="custom-fields-in-log-analytics"></a>Aangepaste velden in het logboek Analytics

De functie voor **Aangepaste velden** van Analytics logboek kunt u bestaande records in de opslagplaats OMS uitbreiden door uw eigen doorzoekbare velden toe te voegen.  Aangepaste velden worden automatisch ingevuld op basis van gegevens afkomstig uit andere eigenschappen in dezelfde record.

![Overzicht van aangepaste velden](media/log-analytics-custom-fields/overview.png)

Het onderstaande voorbeeldrecord heeft bijvoorbeeld bruikbare gegevens begraven in de beschrijving van de gebeurtenis.  Extraheren van deze gegevens in afzonderlijke eigenschappen maakt het beschikbaar zijn van bijvoorbeeld sorteren en filteren.

![De knop logboek zoeken](media/log-analytics-custom-fields/sample-extract.png)

>[AZURE.NOTE] In het voorbeeld bent u beperkt tot 100 aangepaste velden in uw werkruimte.  Deze limiet wordt uitgepakt als deze functie beschikbaar is bereikt.

## <a name="creating-a-custom-field"></a>Een aangepast veld maken

Als u een aangepast veld maakt, moet logboek Analytics welke gegevens worden gebruikt voor het vullen van de waarde ervan begrijpen.  Een technologie van Microsoft Research FlashExtract genoemd wordt gebruikt deze gegevens snel te identificeren.  In plaats van dat u expliciete instructies te geven, hoort Analytics logboek van de gegevens die u wilt ophalen uit de voorbeelden die u opgeeft.

De volgende secties vindt de procedure voor het maken van een aangepast veld.  Onder aan dit artikel wordt een overzicht van de extractie van een steekproef.

> [!NOTE] Het aangepaste veld wordt ingevuld als de records die overeenkomen met de opgegeven criteria worden toegevoegd aan het gegevensarchief OMS zodat er alleen op de records die worden verzameld nadat u het aangepaste veld hebt gemaakt.  Het aangepaste veld worden niet toegevoegd aan records die al in het gegevensarchief wanneer deze wordt gemaakt.

### <a name="step-1--identify-records-that-will-have-the-custom-field"></a>Stap 1 – records waarvoor u het aangepaste veld identificeren
De eerste stap is het identificeren van de records die het aangepaste veld.  U start met een [standaard logboek zoeken](log-analytics-log-searches.md) en selecteer een record als het model dat logboek Analytics leert uit.  Als u opgeeft dat u wilt ophalen van gegevens in een aangepast veld, wordt het **Veld extractie Wizard** geopend waar valideren en verfijnen van de criteria.

2. Ga naar de **Zoekfunctie** en gebruik van een [query de records op te halen](log-analytics-log-searches.md) waarvoor u het aangepaste veld.
2. Selecteer een record dat logboek Analytics wordt gebruikt om te fungeren als een model voor het extraheren van gegevens voor het aangepaste veld.  U geeft de gegevens die u wilt ophalen van deze record en logboek Analytics deze informatie wordt gebruikt om te bepalen van de logica voor het vullen van het aangepaste veld voor alle vergelijkbare records.
3. Klik op de knop links van de eigenschap text van de record, en selecteer **velden uit te pakken**.
4. Het **veld extractie Wizard wordt geopend**en de geselecteerde record weergegeven in de kolom **Voorbeeld Main** .  Het aangepaste veld moet worden gedefinieerd voor de records met dezelfde waarde in de eigenschappen die zijn geselecteerd.  
5. Als de selectie niet precies wat u wilt, selecteert u extra velden om de criteria te beperken.  Zo wijzigt u de waarden voor de criteria, moet u annuleren en een andere record overeenkomen met de criteria die u wilt selecteren.

### <a name="step-2---perform-initial-extract"></a>Stap 2: eerste uittreksel uitvoeren.
Zodra u de records die u het aangepaste veld hebt bepaald, kunt u de gegevens die u wilt uitpakken identificeren.  Logboek Analytics wordt deze informatie gebruikt voor het identificeren van soortgelijke patronen in soortgelijke records.  In de stap na dit is u mogelijk om de resultaten te valideren en nadere details voor logboek Analytics gebruiken in haar analyse.

1. Selecteer de tekst in het voorbeeldrecord die u wilt vullen van het aangepaste veld.  U krijgt vervolgens een dialoogvenster om een naam voor het veld en voor het uitvoeren van de eerste extract.  De tekens ** \_CF** wordt automatisch toegevoegd.
2. Klik op **uitpakken** voor het uitvoeren van een analyse van de verzamelde records.  
3. De secties **Overzicht** en **Zoekresultaten** worden de resultaten van het extract weergeven zodat u de juistheid ervan kunt controleren.  **Samenvatting** wordt gebruikt voor het identificeren van records en een aantal voor elk van de waarden van de aangegeven criteria.  **Zoekresultaten** bevat een gedetailleerde lijst met records die aan de criteria voldoen.


### <a name="step-3--verify-accuracy-of-the-extract-and-create-custom-field"></a>Stap 3: Controleer of de nauwkeurigheid van het extract en aangepast veld maken

Nadat u het eerste uittreksel hebt uitgevoerd, verschijnt logboek Analytics de resultaten op basis van gegevens die zijn verzameld.  Als de resultaten nauwkeurig zoeken kunt u het aangepaste veld maken met geen verdere werkzaamheden.  Als dit niet het geval is, waarna u de resultaten verder kunt verfijnen zodat logboek Analytics de logica kunnen verbeteren.

2.  Als alle waarden in het eerste uittreksel niet juist zijn, klikt u op het pictogram **bewerken** naast een onjuiste record en selecteert u **deze markering wijzigen** om de selectie te wijzigen.
3.  De post wordt gekopieerd naar de sectie **Extra voorbeelden** onder de **Belangrijkste voorbeeld**.  U kunt hier de markering logboek Analytics begrijpen van de selectie die moet hebben gemaakt om aanpassen.
4.  Klik op **ophalen** om het gebruik van deze nieuwe informatie voor de evaluatie van de bestaande records.  De resultaten kunnen worden gewijzigd voor records dan die u zojuist hebt gewijzigd op basis van deze nieuwe intelligence.
5.  Correcties toevoegen totdat alle records in het extract de gegevens herkennen voor het nieuwe aangepaste veld blijven.
6. Klik op **Opslaan uitpakken** als u tevreden over de resultaten bent.  Het aangepaste veld is nu gedefinieerd, maar het zal niet worden toegevoegd aan records nog.
7.  Wachten op nieuwe records die de opgegeven criteria voldoen, worden verzameld en vervolgens de zoekfunctie opnieuw uitvoeren. Nieuwe records moet het aangepaste veld.
8.  Het gebruik van het aangepaste veld net als elke andere record, eigenschap  U kunt gegevens voor statistische en groep en deze zelfs gebruiken voor de productie van nieuwe inzichten.


## <a name="viewing-custom-fields"></a>Aangepaste velden weergeven
U kunt een overzicht van alle aangepaste velden in uw groep beheer van de tegel **Instellingen** van het dashboard OMS.  Selecteer **gegevens** en **aangepaste velden** voor een lijst met alle aangepaste velden in uw werkruimte.  

![Aangepaste velden](media/log-analytics-custom-fields/list.png)

## <a name="removing-a-custom-field"></a>Een aangepast veld verwijderen
Er zijn twee manieren voor het verwijderen van een aangepast veld.  De eerste is de optie **verwijderen** voor elk veld bij het weergeven van de volledige lijst zoals hierboven beschreven.  De andere methode is voor het ophalen van een record en klik op de knop aan de linkerkant van het veld.  Het menu heeft een optie voor het verwijderen van het aangepaste veld.

## <a name="sample-walkthrough"></a>Voorbeeld scenario

De volgende sectie helpt bij een volledig voorbeeld van het maken van een aangepast veld.  In dit voorbeeld haalt de naam van de service in de Windows-gebeurtenissen die wijzen op een service status wijzigen.  Dit is gebaseerd op gebeurtenissen die door servicebeheer in het systeemlogboek op Windows-computers gemaakt.  Als u dit voorbeeld wilt, moet u [het verzamelen van gebeurtenissen voor het systeemlogboek](log-analytics-data-sources-windows-events.md).

Vullen we de volgende query om te retourneren van alle gebeurtenissen van servicebeheer met de gebeurtenis-ID 7036 is de gebeurtenis die aangeeft van een service starten of stoppen.

![Query](media/log-analytics-custom-fields/query.png)

We vervolgens een record selecteren met gebeurtenis-ID 7036.

![Bronrecord](media/log-analytics-custom-fields/source-record.png)

We willen de servicenaam die wordt weergegeven in de eigenschap **RenderedDescription** en klik op de knop naast deze eigenschap.

![Uitpakken van velden](media/log-analytics-custom-fields/extract-fields.png)

Het **Veld extractie Wizard** wordt geopend en de **EventLog** en **gebeurtenis-id** -velden zijn geselecteerd in de kolom **Voorbeeld Main** .  Dit geeft aan dat het aangepaste veld moet worden gedefinieerd voor de gebeurtenissen uit het logboek met de gebeurtenis-ID 7036.  Dit is voldoende, zodat we niet hoeven te eventuele andere velden selecteren.

![Voorbeeld van de belangrijkste](media/log-analytics-custom-fields/main-example.png)

Wij selecteren de naam van de service in de eigenschap **RenderedDescription** en **-Service** gebruiken om de servicenaam te identificeren.  Het aangepaste veld wordt **Service_CF**aangeroepen.

![De titel](media/log-analytics-custom-fields/field-title.png)

We zien dat de servicenaam van de correct wordt geïdentificeerd voor bepaalde records, maar niet voor andere.   De **Zoekresultaten** weergeven in dat deel van de naam van de **WMI-prestaties-Adapter** is niet geselecteerd.  De **Samenvatting** toont dat vier records met **DPRMA** -service ten onrechte een extra word opgenomen en twee records geïdentificeerd **Modules Installer** in plaats van **Windows-Installer Modules**.  

![Zoekresultaten](media/log-analytics-custom-fields/search-results-01.png)

We beginnen met de **WMI-prestatieadapter** record.  We klikt u op het bewerkingspictogram en **deze markeren wijzigen**.  

![Markering wijzigen](media/log-analytics-custom-fields/modify-highlight.png)

We verhogen de markering het woord **WMI** en vervolgens opnieuw uitvoeren van het extract.  

![Voorbeeld van de aanvullende](media/log-analytics-custom-fields/additional-example-01.png)

U ziet dat de posten voor de **Adapter voor WMI-prestaties** zijn verbeterd en logboek Analytics die informatie ook gebruikt voor het corrigeren van de records voor **Installer voor Windows-Module**.  Al die **DPMRA** is nog steeds niet wordt geïdentificeerd correct kunnen we zien in de sectie **Samenvatting** .

![Zoekresultaten](media/log-analytics-custom-fields/search-results-02.png)

We schuiven naar een record met de DPMRA-service en hetzelfde proces gebruiken om op te lossen die record.

![Voorbeeld van de aanvullende](media/log-analytics-custom-fields/additional-example-02.png)

 Wanneer we de extractie uitvoert, u ziet dat onze resultaten nu juist zijn.

![Zoekresultaten](media/log-analytics-custom-fields/search-results-03.png)

U ziet dat de **Service_CF** is gemaakt maar nog niet is toegevoegd aan records.

![Oorspronkelijke aantal](media/log-analytics-custom-fields/initial-count.png)

Na enige tijd is verstreken, dus nieuwe gebeurtenissen worden verzameld, zien we dat het **Service_CF** veld nu wordt toegevoegd aan records die overeenkomen met die onze criteria.

![Definitieve resultaten](media/log-analytics-custom-fields/final-results.png)

We kunnen nu het aangepaste veld net als elke andere record eigenschap gebruiken.  Ter illustratie maken we een query die op het nieuwe veld aan **Service_CF** om te controleren welke services het meest actief zijn gegroepeerd.

![Query groeperen](media/log-analytics-custom-fields/query-group.png)

## <a name="next-steps"></a>Volgende stappen

- Informatie over het [logboek zoeken](log-analytics-log-searches.md) met aangepaste velden voor de criteria voor query's bouwen.
- [Aangepaste logboekbestanden](log-analytics-data-sources-custom-logs.md) parseren met aangepaste velden controleren.