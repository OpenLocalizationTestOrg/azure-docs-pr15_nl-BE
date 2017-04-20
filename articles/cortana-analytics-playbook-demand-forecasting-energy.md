<properties
    pageTitle="Sjabloon Playbook van Cortana Intelligence-oplossing voor het voorspellen van de vraag van energie | Microsoft Azure"
    description="Een sjabloon voor oplossing met Microsoft Cortana Intelligence waarmee de prognose van de vraag voor een hulpprogramma energie bedrijf."
    services="cortana-analytics"
    documentationCenter=""
    authors="ilanr9"
    manager="ilanr9"
    editor="yijichen"/>

<tags
    ms.service="cortana-analytics"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="01/24/2016"
    ms.author="ilanr9;yijichen;garye"/>

# <a name="cortana-intelligence-solution-template-playbook-for-demand-forecasting-of-energy"></a>Sjabloon Playbook van Cortana Intelligence-oplossing voor het voorspellen van de vraag van energie  

## <a name="executive-summary"></a>Samenvatting  

In de afgelopen paar jaar hebt Internet dingen (IoT), alternatieve energiebronnen en grote gegevens samengevoegd om enorme opportunities maken in het domein hulpprogramma en energie. Terzelfder tijd kenden het hulpprogramma en de gehele energiesector verbruik afvlakken uit met betere manieren om hun gebruik van energie veeleisende consumenten. Dus zijn het hulpprogramma en slimme raster bedrijven nodig om te innoveren en zich te vernieuwen. Veel hulpprogramma en de rasters worden bovendien steeds verouderde en zeer kostbare te onderhouden en te beheren. Tijdens het laatste jaar is het team werkt op een aantal opdrachten die binnen het domein energie. Tijdens deze betrekkingen, hebben we veel gevallen waarin de hulpprogramma's of ISV's (Independent Software Vendors) hebben op zoek was bij de prognoses voor toekomstige energiebehoefte is opgetreden. Deze prognoses een belangrijke rol spelen in hun huidige en toekomstige werkzaamheden en de basis voor verschillende use-cases zijn geworden. Deze omvatten korte en lange termijn energie belasting prognose, handel, taakverdeling, raster optimalisatie enz. BIG data en geavanceerde Analytics (AA) methoden zoals Machine Learning (ML) zijn de sleutel om voor de productie van nauwkeurige en betrouwbare prognoses.  

In deze playbook, we samengesteld business en analytische richtsnoeren die nodig zijn voor een succesvolle ontwikkeling en implementatie van de energiebehoefte van prognose-oplossing. Deze voorgestelde richtlijnen kunt u hulpprogramma's, gegevens, wetenschappers en technici van gegevens bij het maken van oplossingen volledig geoperationaliseerd, cloud-gebaseerde, het voorspellen van de vraag. Voor bedrijven die net hun big data en geavanceerde analytics reis beginnen, kan een dergelijke oplossing de basiswaarde in hun langetermijnstrategie smart raster vertegenwoordigen.

>[AZURE.TIP] Zie een diagram een structuuroverzicht van deze sjabloon biedt downloaden, [Cortana Intelligence oplossing sjabloon architectuur voor het voorspellen van de vraag van energie](cortana-analytics-architecture-demand-forecasting-energy.md).  

## <a name="overview"></a>Overzicht  

Dit document heeft betrekking op het bedrijf, gegevens en technische aspecten van het gebruik van Cortana Intelligence en in bepaalde Azure Machine Learning (AML) voor het opzetten en implementeren van oplossingen voor energie prognoses. Het document bestaat uit drie hoofdonderdelen:  

1. Wat zijn Business  
2. Wat zijn de gegevens  
3. Technische uitvoering

Het **Bedrijf wat** deel geeft een overzicht van het zakelijke aspect één moet te begrijpen en te overwegen alvorens een beleggingsbeslissing. Hierin wordt uitgelegd hoe u het zakelijke probleem bij de hand om ervoor te zorgen dat voorspellende analyses en machine learning inderdaad doeltreffend en die van toepassing zijn. Het document verder worden de basisprincipes van machine learning en hoe deze wordt gebruikt om de energie-prognoses problemen uitgelegd. Het overzicht van de voorwaarden en de criteria voor erkenning van een use-case. Sommige monster gebruik gevallen en business case scenario's zijn ook beschikbaar.

Gegevens is het belangrijkste ingrediënt voor een machine learning-oplossing. Het gedeelte **Wat gegevens** van dit document heeft betrekking op enkele belangrijke aspecten van de gegevens. Het overzicht van het soort gegevens dat nodig is voor het voorspellen van energie kwaliteitseisen gegevens en welke gegevensbronnen bestaat meestal. Ook wordt uitgelegd hoe de onbewerkte gegevens ter voorbereiding van de functies die daadwerkelijk het deel van het model wordt gebruikt.

Het derde deel van het document heeft betrekking op het aspect van de **Technische implementatie** van een oplossing. Functie engineering en de modellen vormen de kern van het proces van de wetenschappelijke gegevens en worden daarom in sommige detail besproken. Dit omvat het concept van webservices een belangrijk voertuig voor de implementatie van de wolk van anticiperende analytische oplossingen zijn. We ook een overzicht maken van een typische architectuur van een end-to-end-oplossing voor operationalized.

Het document bevat ook referentiemateriaal die u gebruiken kunt om toegang te krijgen meer inzicht in het domein en de technologie.

Het is belangrijk te weten dat we niet van plan bent te dekken in dit document het diepere gegevens wetenschap proces de wiskundige en technische aspecten. Deze gegevens vindt u in [documentatie Azure ML](http://azure.microsoft.com/services/machine-learning/) en [blogs](http://blogs.microsoft.com/blog/tag/azure-machine-learning/).

### <a name="target-audience"></a>Doelgroep   
De doelgroep voor dit document is bedrijfs- en technisch personeel die graag willen verwerven van kennis en begrip van de Machine Learning solutions en hoe deze worden gebruikt met name binnen het domein energie prognoses gebaseerd.

Wetenschappers van de gegevens kunnen ook profiteren van het lezen van dit document krijgen een beter begrip van het hoogste niveau proces dat de implementatie van een oplossing prognoses energie stations. In dit verband kan ook worden gebruikt voor het tot stand brengen van een goede basis en uitgangspunt voor meer gedetailleerde en geavanceerde materialen.

### <a name="industry-trends"></a>Trends in de bedrijfstak  
In de afgelopen jaren hebben IoT, alternatieve energiebronnen en grote gegevens vast opportunities maken in het hulpprogramma en de energie-ruimte samengevoegd. Op hetzelfde moment kenden het hulpprogramma en de gehele energiesector verbruik afvlakken uit met betere manieren om hun gebruik van energie veeleisende consumenten.

Veel hulpprogramma en intelligente energie bedrijven hebben het [Slim raster](https://en.wikipedia.org/wiki/Smart_grid) is pioneering door het implementeren van een aantal gevallen die het gebruik van de gegevens die zijn gegenereerd door het raster gebruiken. Gebruik vaak gebaseerd op de inherente kenmerken van opwekking van elektriciteit: kan niet worden samengevoegd of gereserveerd als voorraad wordt opgeslagen. Dus moet wat wordt geproduceerd worden gebruikt. Hulpprogramma's die u wilt weten hoe u efficiënter energieverbruik alleen prognose nodig omdat die ze meer mogelijkheden voor het **Saldo vraag en aanbod geven**, waardoor wordt voorkomen dat verliezen energie, **gas-emissie van broeikasgassen te verminderen**en de kosten beheer.

Bij het bespreken van de kosten, is er een ander belangrijk aspect, de prijs is. Nieuwe mogelijkheden aan stroom tussen de hulpprogramma's van de handel hebben gebracht een prognose van de **toekomstige vraag en de toekomstige prijs van de elektriciteit**nodig. Hierdoor kunnen bedrijven bepalen van de omvang van de productie.

Wanneer we het woord 'smart', we feitelijk naar verwijzen een raster dat u kunt leren en voorspellingen. Deze kan anticiperen op seizoensgebonden veranderingen in de consumptie als **tijdelijke overbelasting situaties voorzien en automatisch aangepast aan het**. Door het op afstand regelen van consumptie (met behulp van deze slimme meters), kunnen gelokaliseerde overbelasting situaties worden verwerkt. **Door eerst te voorspellen en stelt vervolgens**het raster kunt u zelf slimmer na verloop van tijd.

Voor de rest van dit document gaan we in op een bepaalde familie van use-cases die betrekking hebben op prognoses van toekomstige, korte termijn en op lange termijn energiebehoefte. We werken al in deze gebieden een paar maanden en hebben opgedaan enkele kennis en vaardigheden waarmee wij de industrie kwaliteit oplevert. Andere gevallen gebruik worden gedekt en in het document in de nabije toekomst.

## <a name="business-understanding"></a>Wat zijn Business

### <a name="business-goals"></a>Bedrijfsdoelen
**Demo energie** is bedoeld om aan te tonen een typische voorspellende analyses en machine learning-oplossing die kan worden geïmplementeerd in een zeer korte tijd-frame. Onze huidige focus is specifiek voor het inschakelen van de energie vraag prognose oplossingen zodat de zakelijke waarde kan snel worden gerealiseerd en ontregeld na. De informatie in deze playbook kunt de klant de volgende doelstellingen bereiken:
-   Oplossing op basis van korte tijd waarde van machine learning
-   Mogelijkheid om uit te breiden van een pilot use-case een bredere scope op basis van hun zakelijke behoeften of andere use-cases
-   Snel krijgen Cortana Intelligence Suite productkennis

Met deze doelstellingen in gedachten, deze playbook is gericht op het aanbieden van zakelijke en technische kennis die helpt bij het bereiken van deze doelstellingen.

### <a name="power-load-and-demand-forecasting"></a>Stroom laden en het voorspellen van de vraag
In de energiesector, kan er veel manieren waarop vraag prognoses kan helpen oplossen van bedrijfskritische problemen. In feite kan voorspellen van de vraag worden beschouwd de basis voor veel core use-cases in de industrie. In het algemeen, we rekening houden met twee soorten prognoses zijn energie vraag: korte termijn en lange termijn. Elk kan een ander doel dienen en maken gebruik van een andere benadering. Het belangrijkste verschil tussen de twee is de prognose horizon, wat betekent dat het bereik van de tijd in de toekomst die we zouden prognose.

#### <a name="short-term-load-forecasting"></a>Korte termijn laden prognoses
Binnen de context van de energiebehoefte van wordt korte termijn laden prognoses (STLF) gedefinieerd als de geaggregeerde belasting die in de nabije toekomst op verschillende delen van het raster (of het raster als geheel) wordt een prognose. Korte termijn is in deze context wordt gedefinieerd als tijdshorizon binnen het bereik van 1 uur tot 24 uur. In sommige gevallen is een periode van 48 uur ook mogelijk. STLF is dus zeer gemeenschappelijk in een geval van het operationele gebruik van het raster. Hier volgen enkele voorbeelden van de basis-use-cases STLF:
-   Vraag en aanbod, Netwerktaakverdeling
-   Trading ondersteuning voor energiebeheer
-   Markt maken (instelling power prijs)
-   Operationele optimalisatie raster
-   [Vraag antwoord](https://en.wikipedia.org/wiki/Demand_response)
-   Pieksnelheden voorspellen van de vraag
-   Demand side management
-   Taakverdeling en overbelasting te voorkomen
-   Lange termijn prognose laden
-   Detectie van probleem- en afwijking
-   Belangrijke inperking piek/herverdeling 

STLF model zijn voornamelijk gebaseerd op het nabije verleden (laatste dag of week) gegevens over het verbruik en gebruik de temperatuur als een belangrijke predictor prognose. Het verkrijgen van nauwkeurige temperatuur prognose voor het komende uur worden uitgezonden en van 24 uur wordt steeds minder van een uitdaging nu dagen. Deze modellen zijn minder gevoelig voor seizoensgebonden patronen en tendensen op lange termijn verbruik.

SLTF oplossingen waarschijnlijk ook voor het genereren van grote hoeveelheden voorspelling oproepen (aanvragen) omdat ze worden aangeroepen op uurbasis en in sommige gevallen zelfs met een hogere frequentie. Het is ook heel gebruikelijk bij stadium van de innesteling waarbij elke afzonderlijke onderstation of transformator wordt voorgesteld als een zelfstandige model en het volume van de voorspelling aanvragen zijn dus nog meer.

#### <a name="long-term-load-forecasting"></a>Lange termijn prognose laden
Het doel van de lange termijn laden prognoses (LTLF) is het gevraagde vermogen prognose met een periode variërend van 1 week tot meerdere maanden (en in sommige gevallen voor een aantal jaren). Dit bereik van de horizon is voornamelijk van toepassing voor de planning en investering use-cases.

Voor de lange termijn scenario's is het belangrijk om gegevens van hoge kwaliteit die betrekking heeft op een reeks van meerdere jaren (ten minste 3 jaar). Deze modellen worden meestal seizoensgebonden patronen van de historische gegevens uitpakken en gebruik maken van externe predicators zoals als patronen van weer en klimaat.

Het is belangrijk om te verduidelijken dat hoe langer de prognose horizon is, hoe minder nauwkeurig de prognose is mogelijk. Het is daarom belangrijk voor de productie van sommige betrouwbaarheidsintervallen en de werkelijke prognose waardoor mensen te houden met de mogelijke variatie in het planningsproces.

Omdat het verbruik voor de LTLF is voornamelijk scenarioplanning, verwachten we veel lagere volumes voorspelling (in vergelijking met STLF). We meestal zien deze voorspellingen in visualisatie tools zoals Excel of PowerBI ingesloten en handmatig worden geactiveerd door de gebruiker.

### <a name="short-term-vs-long-term-prediction"></a>Korte termijn versus lange termijn voorspelling
In de volgende tabel worden de STLF en LTLF vergeleken met betrekking tot de belangrijkste kenmerken:

|Kenmerk|Korte termijn laden prognose|Lange termijn prognose laden|
|---|---|---|
|Prognose Horizon|Van 1 uur en 48 uur|Van 1 tot 6 maanden of meer|
|Gedetailleerdheid van de gegevens|Per uur|Elk uur of dagelijks|
|Typische use-cases|<ul><li>Vraag/aanbod, Netwerktaakverdeling</li><li>Kies uur prognoses</li><li>Vraag antwoord</li></ul>|<ul><li>Langetermijnplanning</li><li>Raster activa plannen</li><li>Resourceplanning</li></ul>|
|Typische variabelen|<ul><li>Dag of week</li><li>Het uur van de dag</li><li>Temperatuur per uur</li></ul>|<ul><li>Maand van het jaar</li><li>Dag van de maand</li><li>Lange termijn temperatuur en klimaat</li></ul>|
|Bereik van historische gegevens|Twee tot drie jaar ter waarde van gegevens|5 tot 10 jaar ter waarde van gegevens|
|Gemiddelde nauwkeurigheid|MAPE * van 5% of minder|MAPE * van 25% of minder|
|Prognose-frequentie|Elk uur of elke 24 uur geproduceerd|Eenmaal per maand, kwartaal of jaar geproduceerd|
\*[MAPE](https://en.wikipedia.org/wiki/Mean_absolute_percentage_error) – gemiddelde gemiddelde percentage fout

Zoals u kunt zien uit deze tabel, is het heel belangrijk dat onderscheid maken tussen de korte en lange termijn scenario's te voorspellen als deze verschillende zakelijke behoeften vertegenwoordigen en mogelijk andere implementatie- en consumptiepatronen.

### <a name="example-use-case-1-esmart-systems--overload-optimization"></a>Voorbeeld van de Use-Case 1: eSmart-systemen – overbelasting optimalisatie
Een belangrijke rol van een [slimme raster](https://en.wikipedia.org/wiki/Smart_grid) is dynamisch en voortdurend optimaliseren en aanpassen aan de veranderende consumptiepatronen. Energieverbruik kan negatieve gevolgen hebben op korte termijn wijzigingen die voornamelijk worden veroorzaakt door schommelingen van temperatuur (*bv.*meer vermogen wordt gebruikt voor lucht voorwaarde of verwarming). Energieverbruik wordt op het moment ook beïnvloed door tendensen op lange termijn. Deze kunnen seizoensgebonden effecten, nationale feestdagen op lange termijn groei van de consumptie en zelfs economische factoren zoals de index van de consument, de olieprijs en bbp bevatten.

In dit geval gebruik wilde [eSmart](http://www.esmartsystems.com/) implementeren een cloud-oplossing waarmee de investeringsneiging van de overbelasting van het raster op een bepaalde onderstation voorspellen. ESmart wilden met name onderstations die waarschijnlijk overbelasting binnen een uur, zodat een onmiddellijke actie kan worden genomen om te voorkomen of oplossen van die situatie te identificeren.

Een nauwkeurig en snel uitvoeren van voorspelling vereist uitvoering van drie voorspellende modellen:
-   Lange termijn model waarmee de prognoses van het energieverbruik op elke onderstation tijdens de volgende paar weken of maanden
-   Korte termijn model waarmee de voorspelling van overbelasting gedurende een uur op een bepaalde onderstation
-   Temperatuur-model waarmee de prognose van de toekomstige temperatuur via meerdere scenario 's

Het doel van het model op de lange termijn is de onderstations rangschikken op basis van hun investeringsneiging van overbelasting (gezien de capaciteit van de power transmission) tijdens de volgende week of maand. Hierdoor kan het maken van een korte lijst van onderstations die als input voor de voorspelling op korte termijn dienen zou. Temperatuur is een belangrijke predictor voor het model op de lange termijn, behoeft een voortdurend scenario met meerdere temperatuur prognoses maken en deze feed als invoer in het model op de lange termijn. De prognose voor de korte termijn wordt vervolgens aangeroepen om te voorspellen welke onderstation dreigt te overbelasten via het volgende uur.

De modellen op korte en lange termijn worden afzonderlijk per elke onderstation geïmplementeerd. Daarom moet de praktische uitvoering van deze modellen uitgebreide orchestration. Om te krijgen hogere nauwkeurigheid van de voorspelling voor de korte termijn, is voor elk uur van de dag een nauwkeurigere model gereserveerd. Alle modellen worden elk uur uitgevoerd en voltooid kan worden uitgevoerd binnen een paar minuten te laat voldoende tijd om te reageren en preventieve acties uitvoeren als dat nodig is. Deze collectie modellen wordt up-to-date gehouden door periodieke opnieuw opleiden met behulp van de meest recente gegevens.

Meer informatie over deze use-case vindt u [hier](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18945).

#### <a name="use-case-qualification-criteria--prerequisites"></a>Case kwalificatiecriteria – voorwaarden gebruiken
De belangrijkste troef van Cortana Intelligence is het krachtige te distribueren en schalen van machine learning georiënteerde oplossingen. Het is ontworpen voor ondersteuning van duizenden van voorspellingen die gelijktijdig worden uitgevoerd. Deze kan automatisch aangepast om te voldoen aan een veranderende consumptiepatroon. De focus van een oplossing dus, op nauwkeurigheid en computerprestaties. Bijvoorbeeld, is een nutsbedrijf geïnteresseerd in het produceren van accurate energiebehoefte prognose voor het komende uur worden uitgezonden en voor elk uur van de dag. Aan de andere kant zijn we minder geïnteresseerd zijn bij het beantwoorden van de vraag waarom de vraag wordt voorspeld te zijn als het is (het model zelf wordt gedaan die).

Daarom is het belangrijk te beseffen dat niet alle use-cases en zakelijke problemen effectief kunnen worden opgelost met behulp van machine learning.

Cortana intelligentie en machine learning zijn zeer effectief bij het oplossen van een bepaald bedrijfsproces probleem wanneer de volgende criteria wordt voldaan:
-   De business probleem in de hand is **preventief** van aard. Een case voorspellende gebruiken-voorbeeld is een elektriciteitsbedrijf dat u wilt laden op een bepaalde onderstation power voorspellen gedurende het volgende uur. Aan de andere kant, zou analyseren en classificeren van stuurprogramma's van de historische vraag zijn **beschrijvende** aard en derhalve minder die van toepassing zijn.
-   Er is een duidelijk **pad van actie** moeten worden genomen zodra de voorspelling beschikbaar is. Bijvoorbeeld, kan voorspellen een overbelasting van een onderstation gedurende het volgende uur resulteren in een proactieve actie van de belasting die is gekoppeld aan die onderstation verminderen en zo mogelijk te voorkomen dat een overbelasting.
-   De use-case vertegenwoordigt een **normale soort probleem** dat wordt opgelost dat kan de weg effenen gebruiken voor het oplossen van andere soortgelijke gevallen.
-   **Kwantitatieve en kwalitatieve doelstellingen** om aan te tonen van de uitvoering van een succesvolle oplossing kunt instellen dat de klant. Een goede kwantitatieve doelstelling voor energie vraagprognose zou bijvoorbeeld de vereiste nauwkeurigheid drempel (*bv.*maximaal 5% fout is toegestaan) of wanneer vervolgens de precisie (het aantal positieve true) onderstation voorspellen overbelasting en moet worden ingetrokken (mate van true positieven) boven een bepaalde drempel. Deze doelstellingen moeten afkomstig zijn van de doelstellingen van de klant.
-   Er is een duidelijk **integratiescenario** met workflow van het bedrijf binnen het bedrijf. Bijvoorbeeld kan de prognose onderstation belasting worden geïntegreerd in het midden van het besturingselement raster om overbelasting te voorkomen-activiteiten.
-   De klant is klaar voor gebruik van **gegevens van voldoende kwaliteit** voor de ondersteuning van de use-case (Zie meer in de volgende sectie, **Kwaliteit van de gegevens**van deze playbook).
-   De klant bestrijkt cloud data georiënteerde architectuur of **cloud-gebaseerde machine leren**, met inbegrip van Azure ML en andere Cortana Intelligence Suite-onderdelen.
-   De klant is bereid te stellen **een complete gegevensstroom** die faciliteiten de levering van gegevens in de cloud voortdurend, en de oplossing is bereid om **mogelijk maken** .
-   De klant is gereed om **resources toe te wijzen aan** die wordt actief bezig zijn tijdens de uitvoering van de eerste prototype dat kennis en eigendom van de oplossing kunnen worden overgedragen aan de klant succesvol is afgerond.
-   De bron van de klant moet een **bekwame professionele gegevens**, bij voorkeur een wetenschappelijk gegevens zijn.

Kwalificatie van een use-case op basis van de bovenstaande criteria kan aanzienlijk verbeteren, de succespercentages van een use-case en tot stand brengen van een goede beachhead voor de uitvoering van aanvragen voor toekomstig gebruik.

### <a name="cloud-based-solutions"></a>Cloud-oplossingen
Cortana Intelligence Suite op Azure is een geïntegreerde omgeving die zich in de cloud bevindt. De implementatie van een geavanceerde oplossing in een cloud-omgeving heeft aanzienlijke voordelen voor bedrijven en tegelijkertijd grote wijziging kunnen betekenen voor bedrijven die nog steeds gebruik van lokale IT-oplossingen. In de energiesector is er een duidelijke trend van geleidelijke migratie van bewerkingen in de cloud. Deze trend gaat gepaard met de ontwikkeling van de smart raster zoals hierboven besproken in **Trends in de bedrijfstak**. Als deze playbook is gericht op een cloud-oplossing in het domein energie, is het belangrijk om uit te leggen de voordelen en andere overwegingen van de implementatie van een cloud-gebaseerde oplossing.

Misschien is het grootste voordeel van een cloud-gebaseerde oplossing de kosten. Als oplossing maakt gebruik van cloud geïmplementeerd onderdelen is er geen opstartkosten of kosten van onderdeel KPV (kostprijs van verkochte goederen) die is gekoppeld aan het. Dit betekent dat hoeft niet te investeren in hardware, software en IT-onderhoud, dus er is een aanzienlijke vermindering van bedrijfsrisico.

Een ander belangrijk voordeel is de pay-as-you-go kostenstructuur van cloud-gebaseerde oplossingen. Cloud-gebaseerde servers voor computergebruik of opslag kunnen worden geïmplementeerd en geschaald op basis van alleen wanneer nodig. Hiermee wordt het voordeel van de kosten-efficiëntie van een cloud-gebaseerde oplossing.

Ten slotte is niet nodig voor het investeren in IT-onderhoud of de infrastructuurontwikkeling van toekomstige als dit deel van het cloud-gebaseerde aanbieden uitmaakt. In die mate, Cortana Intelligence Suite bevat de beste in de klasse-services en de routekaart blijft in ontwikkeling. Nieuwe functies, onderdelen en voorzieningen worden voortdurend geïntroduceerd en ontwikkelen.

Voor een bedrijf dat is de overgang naar de cloud net begint, aanraden we sterk om een geleidelijke aanpak door de implementatie van een wegenkaart cloud migratie. Wij geloven dat de use-cases die worden besproken in deze playbook voor hulpprogramma's en bedrijven in het domein van energie, een uitstekende gelegenheid voor anticiperende analytische oplossingen in de cloud haalbaarheidsonderzoek vertegenwoordigen.

#### <a name="business-case-justification-considerations"></a>Overwegingen met betrekking tot zakelijke rechtvaardiging van de aanvraag
In veel gevallen kan de klant zijn geïnteresseerd in het maken van een zakelijke reden voor een bepaald gebruiksvoorbeeld waar een cloud-oplossing en Machine Learning belangrijke onderdelen zijn. In tegenstelling tot een lokale oplossing, in het geval van een cloud-gebaseerde oplossing, het onderdeel direct kosten minimaal is en de meeste van de kostenelementen zijn gekoppeld met het werkelijke verbruik. Bij de implementatie van een oplossing van Cortana Intelligence Suite prognoses energie, kunnen meerdere services kunnen worden geïntegreerd met een enkele algemene kostenstructuur. Bijvoorbeeld databases (*bijvoorbeeld*SQL Azure) kunnen worden gebruikt om de onbewerkte gegevens op te slaan en vervolgens voor de werkelijke prognoses Azure ML gebruikt als host voor de diensten van de opdrachtgever. In dit voorbeeld kan de kostenstructuur opslag en transactionele onderdelen bevatten.

Aan de andere kant moet een een goed inzicht in de bedrijfswaarde van een (korte of lange termijn) prognoses energiebehoefte werkzaam zijn. In feite is het belangrijk dat u zich realiseert dat de economische waarde van elke bewerking prognose. Zo nauwkeurig energie belasting prognoses voor de komende 24 uur kunt voorkomen dat overproduction of overbelastingen in het raster kunt voorkomen en dit kan worden gekwantificeerd in termen van financiële besparingen op een dagelijkse basis.

Een eenvoudige formule voor het berekenen van de financiële voordelen van vraag, prognose oplossing zou zijn: ![eenvoudige formule voor het berekenen van de financiële voordelen van vraag, prognose oplossing](media/cortana-analytics-playbook-demand-forecasting-energy/financial-benefit-formula.png)

Sinds Cortana Intelligence Suite een "pay-as-you-go" pricing model biedt, is niet nodig voor het aangaan van een onderdeel van de vaste kosten op deze formule. Deze formule kan worden berekend op een dagelijkse, maandelijkse of jaarlijkse basis.

Huidige Cortana Intelligence Suite en Azure ML plannen prijzen vindt u [hier](http://azure.microsoft.com/pricing/details/machine-learning/).

### <a name="solution-development-process"></a>Ontwikkelingsproces van oplossing
De ontwikkelingscyclus van een energiebehoefte forecasting oplossing meestal bestaat uit 4 fasen, die wij maken gebruik van de cloud-gebaseerde technologieën en services binnen de Cortana Intelligence Suite.

Dit wordt geïllustreerd in het volgende diagram:

![Slimme raster cyclus](media/cortana-analytics-playbook-demand-forecasting-energy/smart-grid-cycle.png)

Het volgende lid wordt dit proces stap 4 beschreven:

1.  **Het verzamelen van gegevens** : een geavanceerde analytics gebaseerd oplossing is gebaseerd op gegevens (Zie **Wat zijn de gegevens**). Met name wanneer het gaat om voorspellende analyses en prognoses, vertrouwen wij op continue, dynamische stroom van gegevens. Bij het voorspellen van energie de vraag, deze gegevens kan worden die afkomstig zijn van slimme meters of al in een database op prem worden samengevoegd. We zijn ook afhankelijk van andere externe gegevensbronnen zoals weer en temperatuur. Deze voortdurende stroom van gegevens moet worden ingedeeld, gepland en opgeslagen. [Azure Data Factory](http://azure.microsoft.com/services/data-factory/) Onze belangrijkste werkpaard is (ADF) voor het uitvoeren van deze taak.
2.  **Modeling** – voor nauwkeurige en betrouwbare energie prognoses, moet een (trein) ontwikkelen en onderhouden een prima model dat maakt gebruik van historische gegevens en de betekenis en voorspellende patronen in de gegevens worden uitgepakt. Het gebied van Machine Learning (ML) heeft groeien snel met meer geavanceerde algoritmen die regelmatig wordt ontwikkeld. Azure ML Studio biedt een gebruikerservaring die helpt bij het gebruik van de meest geavanceerde algoritmen ML in een werkstroom voltooid. Werkstroom wordt geïllustreerd in een intuïtieve gegevensstroomdiagrammen en omvat de gegevensvoorbereiding functie extractie, modellering en model-evaluatie. De gebruiker kan worden getrokken in honderden verschillende modellen die zijn opgenomen in deze omgeving. Aan het einde van deze fase wordt een wetenschappelijk gegevens hebben een werkend model dat volledig geëvalueerd en gereed is voor implementatie.

    In het volgende diagram is een voorbeeld van een doorsnee werkstroom:

    ![Modellering van een werkstroom](media/cortana-analytics-playbook-demand-forecasting-energy/modeling-workflow.png)

3.  **Implementatie** – met een werkend model in voorraad, de volgende stap is implementatie. Hier wordt het model geconverteerd naar een webservice die een RESTful API die gelijktijdig kunnen worden aangeroepen via het Internet vanaf verschillende clients voor verbruik beschikbaar worden gesteld. Azure ML biedt een eenvoudige manier van de implementatie van een model rechtstreeks vanuit de Studio Azure ML met één klik van een knop. Het hele implementatieproces gebeurt achter de schermen. Deze oplossing kan automatisch aangepast om te voldoen aan de vereiste verbruik.

4.  **Verbruik** : In deze fase, we maken gebruik van het prognosemodel voor de productie van voorspellingen. Het verbruik kunnen worden aangestuurd vanuit een Gebruikerstoepassing (*bijv.*, dashboard) of rechtstreeks vanuit een operationeel systeem, zoals in vraag/aanbod balancing systeem of een raster optimalisatie oplossing. Meerdere use-cases kunnen worden aangestuurd vanuit een enkel model.

## <a name="data-understanding"></a>Wat zijn de gegevens
Na die betrekking hebben op de zakelijke overwegingen (Zie **Wat zijn Business**) van een energiebehoefte forecasting oplossing, gaan we nu het gedeelte gegevens bespreken. Een anticiperende analytische oplossing is afhankelijk van betrouwbare gegevens. Voor het voorspellen van energie de vraag, zijn we afhankelijk van de historische verbruiksgegevens met verschillende niveaus. Historische gegevens wordt gebruikt als de grondstof. Zij worden onderworpen aan een zorgvuldige analyse waarin het wetenschappelijk gegevens variabelen (ook functies genoemd) die kunnen worden geplaatst in een model dat uiteindelijk de vereiste prognoses genereert wordt geïdentificeerd.

In de rest van deze sectie worden de diverse stappen en overwegingen voor informatie over de gegevens en over te brengen naar een bruikbare vorm worden beschreven.

### <a name="the-model-development-cycle"></a>De ontwikkelingscyclus van Model
Productie van goede prognoses modellen vereist een zorgvuldige voorbereiding en planning. Het resultaat van het hele proces kunnen aanzienlijk verbeteren door het modelleren proces in meerdere stappen splitsen en concentreren op één stap per keer.

In het volgende diagram ziet u hoe het modelleren proces kan worden onderverdeeld in meerdere stappen:

![De ontwikkelingscyclus model](media/cortana-analytics-playbook-demand-forecasting-energy/model-development-cycle.png)

Zoals te zien is dat de cyclus bestaat uit zes stappen:
-   Probleem formulering
-   Gegevens via ingestie en verkennen
-   Gegevens voorbereiden en functie engineering
-   Modellering
-   Model-evaluatie
-   Ontwikkeling

In de rest van deze sectie worden de afzonderlijke stappen en rekening mee moet houden bij elke stap worden beschreven.

### <a name="problem-formulation"></a>Probleem formulering
Wij kunt de formulering van het probleem als de meest kritieke stap één moet worden gehouden voor de uitvoering van een anticiperende analytische oplossing overwegen. Hier zouden we het probleem van de business transformatie en deze op bepaalde elementen die kunnen worden opgelost door middel van gegevens technieken te ontleden. Het is een goede gewoonte om het probleem te formuleren als een reeks vragen die beantwoorden wij graag. Hier volgen enkele mogelijke vragen die mogelijk van toepassing binnen het bereik van het voorspellen van de energie vraag:
-   Wat is de verwachte belasting van een afzonderlijke onderstation in het volgende uur of dag?
-   Op welk tijdstip van de dag wordt mijn raster piekperioden optreden?
-   Wat is de kans mijn raster ter ondersteuning van de verwachte belasting?
-   Hoeveel vermogen moet de waterkrachtcentrale genereren tijdens elk uur van de dag

Uitwerking van deze vragen kan wij gericht op de juiste gegevens opvragen en implementatie van een oplossing die volledig is afgestemd op zakelijk probleem bij de hand. Bovendien kunnen we enkele belangrijke parameters waarmee wij de voor de evaluatie van de prestaties van het model wordt ingesteld. Bijvoorbeeld: hoe nauwkeurig de prognose moet en wat is het bereik van de fout die nog steeds aanvaardbaar door het bedrijf zijn?

### <a name="data-sources"></a>Gegevensbronnen
De moderne slimme raster verzamelt gegevens uit verschillende onderdelen en componenten van het raster. Deze gegevens vertegenwoordigt verschillende aspecten van de werking en het gebruik van het raster van kracht. Binnen de werkingssfeer van de energie vraag prognose, zijn we de discussie over de gegevensbronnen die overeenkomen met de werkelijke vraag verbruik te beperken. Een belangrijke bron van het energieverbruik zijn slimme meters. Hulpprogramma's over de hele wereld zijn slimme meters snel implementeren voor hun consumenten. Slimme meters vastleggen van het werkelijke energieverbruik en voortdurend sturen deze gegevens naar het elektriciteitsbedrijf. Gegevens worden verzameld en verzonden terug met een vast interval, variërend van elke 5 minuten tot 1 uur. Meer geavanceerde slimme meters kunnen op afstand worden geprogrammeerd en het werkelijke verbruik binnen een huishouden in evenwicht. Slimme meter gegevens relatief betrouwbaar is en een tijdstempel bevat. Dat maakt het een belangrijk ingrediënt voor een vraag, prognose. Meter-gegevens kunnen worden samengevoegd (getotaliseerde) op verschillende niveaus binnen het raster topologie: transformer, onderstation, regio, *enz*. We kunt vervolgens de vereiste aggregatieniveau bouwen een prognosemodel voor kiezen. Bijvoorbeeld als het elektriciteitsbedrijf prognose van de toekomstige werklast op elk van de onderstations raster vervolgens alle meters gegevens kunnen worden samengevoegd voor elke afzonderlijke onderstations en gebruikt als invoer voor het prognosemodel. We noemen slimme meters een interne bron.

Een prognose van betrouwbare energie vraag zal ook afhankelijk zijn van andere externe gegevensbronnen. Een belangrijke factor die van invloed is op het stroomverbruik is het weer, of meer precies de temperatuur. Historische gegevens bevat sterke correlatie tussen de buitentemperatuur en het energieverbruik. Tijdens de hete zomer dagen, de consument maken gebruik van de airconditioning en tijdens de winter aan verwarming. Een betrouwbare bron van historische temperaturen op de rasterlocatie is daarom essentieel. Bovendien zijn we ook afhankelijk van nauwkeurige voorspelling van de temperatuur van energieverbruik te voorspellen.

Andere externe gegevensbronnen kunnen ook helpen bij het bouwen van energie vraag prognosemodellen. Deze lange termijn kunnen bevatten wijzigingen klimaat, economische indexen (*bv.*bbp) en anderen. In dit document nemen niet we deze andere gegevensbronnen.

### <a name="data-structure"></a>Gegevensstructuur
Nadat u de benodigde gegevensbronnen, willen we ervoor zorgen dat de onbewerkte gegevens die zijn verzameld de functies van de juiste gegevens bevat. Een betrouwbare vraag prognosemodel bouwen, moeten we om ervoor te zorgen dat de verzamelde gegevens omvatten gegevenselementen die kunnen helpen bij het voorspellen van de toekomstige vraag. Hier vindt u enkele essentiële eisen betreffende de gegevensstructuur (schema) van de onbewerkte gegevens.

De onbewerkte gegevens bestaat uit rijen en kolommen. Elke meting wordt weergegeven als één rij met gegevens. Elke rij bevat meerdere kolommen (ook wel genoemd functies of velden).

1.  **Tijdstempel** – het tijdstempelveld geeft de werkelijke tijd waarop de meting is geregistreerd. Het moet voldoen aan een van de algemene datum/tijd-notaties. Onderdelen van datum en de tijd moeten worden opgenomen. In de meeste gevallen is er niet nodig voor de tijd tot het tweede niveau van granulatie worden vastgelegd. Het is belangrijk om op te geven van de tijdzone waarin de gegevens worden vastgelegd.
2.  **Meter-ID** - dit veld geeft de meter of het apparaat voor de meting. Dit is een variabele bestaan en kan bestaan uit een combinatie van cijfers en tekens.
3.  **Waarde van verbruik** – dit is het werkelijke verbruik op een bepaalde datum en tijd. Het verbruik in kWh (kilowatt-hour) kan worden gemeten of een andere primaire eenheden. Het is belangrijk te weten dat de maateenheid in alle maten in de gegevens consistent moet blijven. In sommige gevallen kan verbruik meer dan 3 power fasen worden geleverd. In dat geval moeten we verzamelen alle fasen van de onafhankelijke consumptie.
4.  **Temperatuur** : de temperatuur wordt meestal verzameld van een onafhankelijke bron. Echter, moet compatibel zijn met de gegevens over het verbruik. Neemt een tijdstempel zoals hierboven beschreven waardoor deze worden gesynchroniseerd met de gegevens van het werkelijke verbruik. De waarde van de temperatuur in graden Celsius of Fahrenheit kan worden opgegeven, maar consistent voor alle metingen moet blijven.
5.  **Locatie:** Het locatieveld is doorgaans gekoppeld aan de plaats waar de temperatuur gegevens zijn verzameld. Worden kan weergegeven als een postcode in getal of latitude/lengtegraad (lat/long).

De volgende tabellen ziet u voorbeelden van een goede gegevensindeling en de temperatuur:

|**Datum**|**Tijd**|**Meter-ID**|**Fase 1**|**Fase 2**|**Fase 3**|
|--------|--------|------------|-----------|-----------|-----------|
|1-7/2015|10:00:00|ABC1234     |7.0        |2.1        |5.3        |
|1-7/2015|10:00:01|ABC1234     |7.1        |2.2        |4.3        |
|1-7/2015|10:00:02|ABC1234     |6.0        |2.1        |4.0        |

|**Datum**|**Tijd**|**Locatie**|**Temperatuur**|
|--------|--------|-------------|---------------|
|1-7/2015|10:00:00|11242        |24,4           |
|1-7/2015|10:00:01|11242        |24,4           |
|1-7/2015|10:00:02|11242        |24,5           |

Zoals u hierboven kunt zien, heeft dit voorbeeld 3 verschillende waarden voor consumptie die is gekoppeld aan de 3 fasen van kracht. Let er ook op dat de velden datum en tijd worden gescheiden, maar ze kunnen ook worden gecombineerd in één kolom. In dit geval wordt de kolom locatie weergegeven in de indeling van een 5-cijferige postcode en de temperatuur in graden Celsius-indeling.

### <a name="data-format"></a>Gegevens opmaken
Cortana Intelligence Suite ondersteunt de meestgebruikte gegevensindelingen zoals CSV-, TSV, JSON, *enz*. Het is belangrijk dat de gegevensindeling consistent voor de volledige levenscyclus van het project blijft.

### <a name="data-ingestion"></a>Opname van de gegevens
Aangezien energie vraagprognose wordt voortdurend en vaak voorspeld, moeten we ervoor zorgen dat de onbewerkte gegevens door middel van een solide en betrouwbare gegevens opname proces stroomt. Het opname proces moet waarborgen dat de onbewerkte gegevens beschikbaar voor het proces van de opdrachtgever op het moment dat vereist is. Dat betekent dat de frequentie van gegevens via de mond groter dan de prognose frequentie zijn moet.

Bijvoorbeeld: als onze vraag forecasting oplossing een nieuwe prognose bij 8:00 uur op een dagelijkse basis genereren zou dan we nodig hebben om ervoor te zorgen dat alle gegevens die zijn verzameld tijdens de afgelopen 24 uur heeft ingenomen is volledig tot dat punt en moet ook het laatste uur van gegevens bevatten.

Cortana Intelligence Suite biedt hiervoor verschillende manieren ter ondersteuning van een proces van betrouwbare gegevens via ingestie. Dit wordt verder besproken in het gedeelte van de **implementatie** van dit document.

### <a name="data-quality"></a>Kwaliteit van de gegevens
De bron van onbewerkte gegevens die vereist zijn voor het uitvoeren van betrouwbare en nauwkeurige vraag prognoses moet voldoen aan bepaalde kwaliteitscriteria basisgegevens. Hoewel geavanceerde statistische methoden kunnen worden gebruikt voor het compenseren van sommige gegevens mogelijk kwaliteit probleem, moeten we nog steeds om ervoor te zorgen dat we enkele basisgegevens kwaliteit drempel overschrijden wanneer nieuwe gegevens ingesting. Hier zijn een paar overwegingen betreffende de kwaliteit van de ruwe gegevens:
-   **Ontbrekende waarde** – dit verwijst naar de situatie wanneer bepaalde meting is niet verzameld. Hier een eerste vereiste is dat de ontbrekende waarde tarief mag niet langer dan 10% voor een bepaalde periode. In het geval dat een enkele waarde in deze ontbreekt moet worden vermeld met behulp van een vooraf gedefinieerde waarde (bijvoorbeeld: '9999') en niet '0', die een geldige waarde kan zijn.
-   **Nauwkeurigheid van de meting** : de werkelijke waarde van verbruik of temperatuur moeten nauwkeurig worden geregistreerd. Onjuiste metingen worden onjuiste prognoses te produceren. De meetfout moet normaal gesproken minder dan 1% van de werkelijke waarde.
-   **Tijdstip van de meting** : het is vereist dat de werkelijke tijdstempel van de gegevens verzameld zal niet meer dan 10 seconden het tijdstip waar de werkelijke meting afwijken.
-   **Synchronisatie** : wanneer u meerdere gegevensbronnen worden gebruikt (*bijvoorbeeld*, verbruik en temperatuur) wij moet ervoor zorgen dat er geen synchronisatie tussen deze problemen zijn. Dit betekent dat het tijdsverschil tussen de verzamelde tijdstempel van elke twee onafhankelijke bronnen moet niet meer dan 10 seconden overschrijden.
-   **Latentie** - zoals hierboven besproken in **Gegevens via ingestie**, we zijn afhankelijk van een betrouwbare gegevens en de opname-proces. Om te bepalen dat we ervoor zorgen moeten dat we de latentie gegevens bepalen. Hiermee wordt opgegeven als het tijdsverschil tussen het moment dat de werkelijke meting heeft plaatsgevonden en het tijdstip waarop deze is geladen in de opslag van Cortana Intelligence Suite en is klaar voor gebruik. Voor het laden van de korte termijn prognoses van de totale vertraging mag niet langer dan 30 minuten. Voor het laden van de lange termijn prognose van de totale vertraging mag niet langer dan 1 dag.

### <a name="data-preparation-and-feature-engineering"></a>Gegevens voorbereiden en functie Engineering
Zodra de onbewerkte gegevens is geconsumeerde (Zie **Data opname**) en veilig zijn opgeslagen, is klaar om te worden verwerkt. De voorbereidingsfase van de gegevens is in principe de onbewerkte gegevens en converteren (transformeert, omvormen) in een formulier voor de modellering van de fasen. Die eenvoudige bewerkingen, zoals met de onbewerkte gegevenskolom met de werkelijke gemeten waarde, gestandaardiseerde waarden meer complexe bewerkingen zoals [tijd vertraging](https://en.wikipedia.org/wiki/Lag_operator)en anderen kunnen bevatten. Kolommen met de nieuwe gegevens worden aangeduid als de functies en het proces van het genereren van deze heet functie engineering. Aan het einde van dit proces zou er een nieuwe gegevensgroep die u heeft is afgeleid van de ruwe gegevens en kan worden gebruikt voor de modellering. Bovendien moet de voorbereidingsfase gegevens zorgen voor de ontbrekende waarden (Zie de **Kwaliteit van de gegevens**) en ze compenseren. In sommige gevallen moeten we zou ook de gegevens om ervoor te zorgen dat alle waarden worden weergegeven in dezelfde schaal normaliseren.

In deze sectie die is een overzicht van de algemene functies van gegevens die zijn opgenomen in de energie prognosemodellen vraag.

**Tijd driven functies:** Deze functies zijn afgeleid van de datum/tijdstempel gegevens. Deze worden uitgepakt en omgezet in bestaan functies zoals:
-   Tijd van de dag: is dit het uur van de dag waarop de waarden van 0 tot en met 23
-   Dag van de week: dit geeft de dag van de week en waarden van 1 (zondag) tot 7 (zaterdag)
-   Dag van de maand: dit geeft de werkelijke datum en kan waarden van 1 tot en met 31
-   Maand van het jaar – dit staat voor de maand en waarden van 1 (januari) tot en met 12 (December)
-   Weekend – dit is de functie van een binaire waarde waarmee de waarden van 0 voor weekdagen of 1 voor het weekend
-   Feestdagen - dit is de functie van een binaire waarde waarmee de waarde 0 voor een gewone dag en op 1 voor een feestdag
-   Fourier-termen – de Fourier-voorwaarden zijn gewichten die zijn afgeleid van het tijdstempel en worden gebruikt voor het vastleggen van de seizoengebonden (cycli) in de gegevens. Omdat we in onze gegevens meerdere seizoenen zijn moeten we meerdere Fourier-termen. Vraag waarden kunnen bijvoorbeeld jaarlijks, dagelijkse en wekelijkse seizoenen/cycli die tot 3 Fourier-termen leidt.

**Onafhankelijke meting functies:** De onafhankelijke functies omvatten de gegevenselementen die we willen graag gebruiken als variabelen in het model. Hier sluiten wij de afhankelijke functie die we nodig om te voorspellen.
-   Functie vertraging: dit zijn tijd verschoven waarden van de werkelijke vraag. Functies lag 1 wordt bijvoorbeeld de waarde vraag houdt in het vorige uur ten opzichte van de huidige tijdstempel (ervan uitgaande dat per uur). Op dezelfde manier wij toevoegen 2 vertraging, vertraging van 3, *enz*. De werkelijke combinatie van vertraging-functies die worden gebruikt tijdens de fase van het model bepaald door de evaluatie van de resultaten van het model.
-   Lange termijn trending: deze functie geeft de lineaire groei van de vraag tussen jaren.

**Afhankelijke functie:** De functie afhankelijk is de gegevenskolom die willen we graag onze model om te voorspellen. Met de [onder toezicht staande machine leren](https://en.wikipedia.org/wiki/Supervised_learning)moeten we eerst trainen met behulp van de afhankelijke functies model (dat is ook bedoeld als labels). Hierdoor wordt het model voor meer informatie over de patronen in de gegevens die zijn gekoppeld aan de functie afhankelijk is. In energiebehoefte prognose meestal willen we de werkelijke vraag te voorspellen en dus we gebruiken als de functie afhankelijk is.

**Behandeling van ontbrekende waarden:** Tijdens de voorbereidingsfase van gegevens moeten we de beste strategie voor het verwerken van ontbrekende waarden bepalen. Dit gebeurt meestal met behulp van de verschillende statistische [gegevens aanrekening methoden](https://en.wikipedia.org/wiki/Imputation_(statistics)). In het geval van de energie vraag prognoses rekenen we meestal ontbrekende waarden met behulp van voortschrijdend gemiddelde van de vorige beschikbare gegevenspunten.

**Gegevensnormalisatie:** Gegevensnormalisatie is een ander type transformatie dat wordt gebruikt voor het doen van alle numerieke gegevens, zoals de vraag prognose in een vergelijkbare schaal. Dit meestal verbetert het model nauwkeurigheid en precisie. We meestal doet dit door de werkelijke waarde te delen door het bereik van de gegevens.
Hiermee wordt de oorspronkelijke waarde beneden de schaal in een kleiner bereik, meestal tussen -1 en 1.

## <a name="modeling"></a>Modellering
De modellering is waar de conversie van de gegevens in een model wordt uitgevoerd. In de kern van dit proces er zijn geavanceerde algoritmen die bij het scannen van de historische gegevens (training), ophalen van patronen en bouwen van een model. Dat model kan later worden gebruikt om te voorspellen op nieuwe gegevens die niet is gebruikt voor het bouwen van het model.

Zodra we een betrouwbaar werkmodel hebben kunnen we het gebruiken voor het verkrijgen van nieuwe gegevens die de structuur van de vereiste voorzieningen (X). Het scorepatroon proces maakt gebruik van het vastgelegde model (object uit de fase van de opleiding) en de doel-variabele die wordt aangeduid met Ŷ voorspellen.

### <a name="demand-forecasting-modeling-techniques"></a>Vraag voorspellen modellering technieken
In het geval van vraag, prognose maken we gebruik van historische gegevens die besteld is door de tijd. We verwijzen in het algemeen naar gegevens met de tijddimensie als de [tijdreeks](https://en.wikipedia.org/wiki/Time_series). In de tijd-serie modeling is bedoeld om te zoeken naar gerelateerde trends, seizoensgebonden, auto-correlatie (correlatie in de tijd), tijd en in een model te formuleren.

In de afgelopen jaren zijn geavanceerde algoritmen ontwikkeld voor tijd reeksen prognoses en forecasting nauwkeurigheid te verbeteren. Enkele van deze hier kort besproken.

> [AZURE.NOTE] Deze sectie is niet bedoeld om te worden gebruikt als een machine learning en prognoses overzicht, maar veeleer als een korte enquête van modellering technieken die vaak worden gebruikt voor het voorspellen van de vraag. Voor meer informatie en educatief materiaal over tijd reeksen prognoses wordt ten zeerste aanbevolen de on line handleiding [prognose: beginselen en praktijk](https://www.otexts.org/book/fpp).

#### <a name="ma-moving-averagehttpswwwotextsorgfpp62"></a>[**MA (voortschrijdend gemiddelde)**](https://www.otexts.org/fpp/6/2)
Zwevend gemiddelde is een van de eerste analysetechnieken die is gebruikt voor het voorspellen van de tijd-serie en is nog steeds een van de meest gebruikte technieken vanaf vandaag. Het is ook de basis voor meer geavanceerde technieken voorspellen. Met een zwevend gemiddelde zijn we de volgende gegevenspunt gemiddelde over de meest recente punten K, waar K de volgorde van het voortschrijdend gemiddelde geeft van de prognoses.

Het zwevend gemiddelde techniek heeft het effect van de prognose vloeiend maken en daarom mogelijk niet verwerken en grote volatiliteit in de gegevens.

#### <a name="ets-exponential-smoothinghttpswwwotextsorgfpp75"></a>[**ETS (exponentiële demping)**](https://www.otexts.org/fpp/7/5)
Exponentiële demping (ETS) is een familie van verschillende methoden die een gewogen gemiddelde van de recente gegevenspunten gebruiken om te voorspellen het volgende punt. Het idee is dat het hogere gewicht aan de meest recente waarden toewijzen en dit gewicht voor oudere gemeten waarden geleidelijk te verlagen. Er zijn een aantal verschillende manieren met deze familie, sommige van deze behandeling van seizoensgebonden opnemen in de gegevens zoals [Holt Winters seizoensgebonden methode](https://www.otexts.org/fpp/7/5).

Enkele van deze methoden ook factor in het seizoensgebonden karakter van de gegevens.

#### <a name="arima-auto-regression-integrated-moving-averagehttpswwwotextsorgfpp8"></a>[**ARIMA (automatische regressie geïntegreerd voortschrijdend gemiddelde)**](https://www.otexts.org/fpp/8)
Automatische regressie geïntegreerde verplaatsen gemiddelde (ARIMA) is een andere familie van methoden die wordt gebruikt om prognoses tijdreeks. Het combineert vrijwel automatische regressie methoden met een zwevend gemiddelde. Automatische regressie methoden maken gebruik van regressie-modellen door middel van vorige keer reekswaarden wilt berekenen van de volgende datum punt. ARIMA methoden ook differentiërende methoden voor de berekening van het verschil tussen de gegevenspunten en die in plaats van de oorspronkelijke waarde gemeten met behulp van toepassing. Ten slotte ARIMA ook gebruik maakt van het zwevend gemiddelde technieken die hierboven zijn besproken. De combinatie van al deze methoden op verschillende manieren is wat wordt gemaakt van de familie ARIMA methoden.

ETS en ARIMA zijn veelgebruikte vandaag voor het voorspellen van de energie vraag en veel economische problemen. In veel gevallen worden deze gecombineerd gecombineerd om zeer nauwkeurige resultaten.

**Algemene meervoudige regressie** Regressie-modellen is de belangrijkste benadering van modellering binnen het domein van machine learning en statistieken. In de context van de tijdreeks gebruiken we regressie (*bv.*van de vraag) de toekomstige waarden te voorspellen. In regressie we nemen een lineaire combinatie van de variabelen en informatie over de gewichten (ook wel als coëfficiënten) van deze variabelen tijdens de training. De doelstelling is de regressielijn die wordt verwacht van de voorspelde waarde produceren. Wanneer de variabele doel numeriek is en daarom ook tijd reeksen prognoses past zijn regressie methoden geschikt. Er is een breed scala van regressie methoden, met inbegrip van zeer eenvoudige regressie-modellen zoals [Lineaire regressie](https://en.wikipedia.org/wiki/Linear_regression) en meer geavanceerde vrienden zoals beslissingsstructuren, [Willekeurige bossen](https://en.wikipedia.org/wiki/Random_forest), [Neural Networks](https://en.wikipedia.org/wiki/Artificial_neural_network)en beslissingsstructuren versterkt.

Energiebehoefte prognoses als een regressie-probleem samen geeft ons de nodige flexibiliteit bij de keuze van onze functies die kunnen worden gecombineerd van de werkelijke vraag wanneer reeksgegevens en externe factoren zoals temperatuur. Meer informatie over de geselecteerde onderdelen worden besproken in de functie Engineering (Zie **gegevens voorbereiden en technische voorziening**) gedeelte van deze playbook.

Uit onze ervaring met de implementatie en distributie van energie vraag prognoses leider, we hebben gevonden dat de geavanceerde regressie-modellen die beschikbaar in Azure ML zijn zijn over het algemeen het beste resultaat en we maken gebruik van hen.

## <a name="model-evaluation"></a>Model-evaluatie
Model-evaluatie heeft een belangrijke rol binnen de **Ontwikkelingscyclus van Model**. Op dit moment kijken we naar het model en de prestaties met werkelijke gegevens valideren. We gebruiken een deel van de beschikbare gegevens voor het trainen van het model tijdens de stap modellering. Tijdens de evaluatiefase nemen we de rest van de gegevens voor het testen van het model. Praktisch betekent dit dat we de nieuwe modelgegevens die heeft is geherstructureerd en bevat dezelfde functies als de dataset training zijn voeding. Echter tijdens de validatie gebruiken we het model om te voorspellen van de variabele doel in plaats van de variabele doel beschikbaar bieden. Wij verwijzen vaak naar dit proces als model scoren. We zouden vervolgens de doelwaarden true gebruiken en ze vergelijken met de voorspelde. Het doel hier is om te meten en het minimaliseren van de voorspelling fout, wat betekent dat het verschil tussen de voorspellingen en de werkelijke waarde. Kwantificeren van de fout-meting is essentieel aangezien wij willen graag het model aanpassen en nagaan of de fout daadwerkelijk afneemt. "Fine-tuning"-het model kan worden gedaan door aanpassing van de Modelparameters die het leerproces bepalen of door het toevoegen of verwijderen van onderdelen van gegevens (hierna aangeduid als [parameters sweep](https://channel9.msdn.com/Blogs/Windows-Azure/Data-Science-Series-Building-an-Optimal-Model-With-Parameter-Sweep)). Dit betekent praktisch dat we mogelijk bladeren tussen de functie engineering, modellerings- en evaluatie fasen model meerdere malen totdat we kunnen de fout op het vereiste niveau verlagen.

Het is belangrijk te benadrukken dat de voorspelling fout nooit zal nul als er is nooit een model dat elke uitkomst perfect kunt voorspellen. Er is echter een bepaalde omvang van de fout die acceptabel is voor het bedrijf. Tijdens de validatie, willen we ervoor zorgen dat onze model voorspelling fout op het niveau is of beter dan het tolerantieniveau business. Daarom is het belangrijk dat u het niveau van de maximaal toelaatbare fout aan het begin van de cyclus in de fase **Probleem formulering** .

### <a name="typical-evaluation-techniques"></a>Typische evaluatietechnieken
Er zijn verschillende manieren in welke voorspelling fout kan worden gemeten en gekwantificeerd. In dit gedeelte gaan we de discussie over de betrokken tijdreeksen en in specifieke voor prognose energiebehoefte evaluatietechnieken.

#### <a name="mapehttpsenwikipediaorgwikimeanabsolutepercentageerror"></a>[**MAPE**](https://en.wikipedia.org/wiki/Mean_absolute_percentage_error)
MAPE staat voor de gemiddelde Absolute Percentage fout. Met MAPE zijn we berekenen het verschil tussen elk punt en de waarde van dat punt prognose. We vervolgens kwantificeren de fout per punt door berekening van het aandeel van het verschil ten opzichte van de werkelijke waarde. Bij de laatste stap gemiddelde we deze waarden. De wiskundige formule die wordt gebruikt voor MAPE is de volgende:

![De formule MAPE](media/cortana-analytics-playbook-demand-forecasting-energy/mape-formula.png)
*waarbij een<sub>t</sub> de werkelijke waarde is, F<sub>t</sub> de voorspelde waarde, en n de prognose horizon is.*

## <a name="deployment"></a>Implementatie
Zodra we hebben oprecht de modellering fase en de prestaties van het model gevalideerd zijn we klaar voor de implementatiefase. In deze context betekent implementatie zodat de klant voor het gebruik van het model door werkelijke voorspellingen op deze grote schaal. Het concept van de implementatie is de sleutel in Azure ML omdat onze belangrijkste doelstelling is om op te roepen voortdurend voorspellingen in plaats van alleen het verkrijgen van het inzicht van de gegevens. De implementatiefase is het deel waar we het model op grote schaal worden verbruikt inschakelen.

Ons doel is binnen de context van de energie vraagprognose, doorlopende en periodieke prognoses aanroepen tijdens het ervoor te zorgen dat nieuwe gegevens beschikbaar voor het model is en dat de verwachte gegevens worden verzonden naar de client in beslag nemen.

### <a name="web-services-deployment"></a>Web Services-implementatie
De belangrijkste bruikbare bouwsteen in Azure ML is de webservice. Dit is de meest effectieve manier om het verbruik van een voorspellende model in de cloud. De webservice kapselt het model en het wordt afgerond met een [RESTful](http://www.restapitutorial.com/) API (Application Programming Interface). De API kan worden gebruikt als onderdeel van een clientcode zoals aangegeven in het onderstaande diagram.

![We Service, distributie en verbruik](media/cortana-analytics-playbook-demand-forecasting-energy/web-service-deployment-and-consumption.png)

Zoals u kunt zien, is de webservice wordt geïmplementeerd in de cloud Cortana Intelligence Suite en kan worden aangeroepen via de blootgestelde REST API-eindpunt. Ander type clients in verschillende domeinen kan tegelijkertijd de service via de Web-API aanroepen. De webservice kunt ook schalen voor de ondersteuning van duizenden gelijktijdige gesprekken.

### <a name="a-typical-solution-architecture"></a>Een standaardoplossing architectuur
Wanneer u een energiebehoefte forecasting oplossing implementeert, zijn we geïnteresseerd in het implementeren van een complete oplossing die gaat verder dan de webservice voorspelling en vergemakkelijkt de gehele gegevensstroom. Op het moment dat we een nieuwe prognose roepen, moeten we om ervoor te zorgen dat het model is ingevoerd met de functies van de meest recente gegevens. Dat betekent dat de nieuwe verzamelde ruwe gegevens wordt voortdurend ingenomen, verwerkt en in de vereiste functie is ingesteld omgezet op het model is gebouwd. Op hetzelfde moment willen we de verwachte gegevens beschikbaar maken voor het einde clients verbruikt. Een voorbeeld van de gegevens stroom cyclus (of gegevens pijpleiding) wordt in het onderstaande diagram geïllustreerd:

![Complete gegevensstroom prognose van de energie vraag](media/cortana-analytics-playbook-demand-forecasting-energy/energy-demand-forecase-end-data-flow.png)

Dit zijn de stappen die uitgevoerd als onderdeel van de energie vraag prognose cyclus worden:
1.  Miljoenen meters geïmplementeerde gegevens voortdurend energie verbruiksgegevens genereert in real-time.
2.  Deze gegevens worden verzameld en geüpload naar de bibliotheek van een wolk (*bv.*Azure Blob).
3.  De onbewerkte gegevens worden samengevoegd naar een onderstation of regionaal niveau alvorens te worden verwerkt, zoals gedefinieerd door het bedrijf.
4.  De verwerking van de functie (Zie **gegevens voorbereiden en verwerken van functie**) vervolgens wordt uitgevoerd en geeft de gegevens die zijn vereist voor model training of scoren – de functie setgegevens worden opgeslagen in een database (*zoals*SQL Azure).
5.  De service opnieuw training wordt opgeroepen opnieuw trainen het prognosemodel – dat de nieuwste versie van het model behouden zodat deze kan worden gebruikt door de webservice score.
6.  De score van de webservice wordt aangeroepen volgens een schema dat past bij de vereiste frequentie van de prognose.
7.  De verwachte gegevens worden opgeslagen in een database die kan worden benaderd door de client einde verbruik.
8.  Het verbruik client haalt de prognoses, past deze weer in het raster en verbruikt deze volgens de vereiste use-case.

Het is belangrijk te weten dat deze hele cyclus is volledig geautomatiseerd en volgens een schema uitgevoerd. De volledige integratie van deze cyclus gegevens kan worden gedaan met hulpmiddelen zoals [Azure Data Factory](http://azure.microsoft.com/services/data-factory/).

### <a name="end-to-end-deployment-architecture"></a>End-to-End implementatie-architectuur
We moeten praktisch implementeren een prognose energie demand-oplossing op Cortana Intelligence, ervoor te zorgen dat de vereiste onderdelen worden vastgesteld en geconfigureerd.

In het volgende diagram ziet u een typische Cortana Intelligence gebaseerd architectuur die wordt geïmplementeerd en de data stroom cyclus die hierboven beschreven is orchestrates:

![End-to-End implementatie-architectuur](media/cortana-analytics-playbook-demand-forecasting-energy/architecture.png)

Raadpleeg voor meer informatie over elk van de onderdelen en de architectuur de sjabloon energie oplossing.
