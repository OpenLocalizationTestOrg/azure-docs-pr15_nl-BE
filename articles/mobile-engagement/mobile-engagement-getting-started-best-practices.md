<properties 
    pageTitle="Azure mobiele Engagement aan de slag met Best Practices"
    description="Getting Started guide voor Azure Mobile Engagement en aanbevolen procedures voor onboarding" 
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="wesmc7777"
    manager="erikre"
    editor=""/>

<tags
    ms.service="mobile-engagement"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="mobile-multiple"
    ms.workload="mobile" 
    ms.date="10/04/2016"
    ms.author="wesmc;ricksal"/>

# <a name="azure-mobile-engagement---getting-started-guide-with-best-practices"></a>Mobiele Engagement Azure - handleiding aan de slag met beste praktijken

## <a name="overview"></a>Overzicht

**Is een zeer drukke ruimte van het mobiele scherm:** In 2013 bleek een onderzoek dat de gemiddelde mobiele apparaat was 27-toepassingen geïnstalleerd. Gebruikers die meestal 30 uur per maand aan hun apps besteed. De meeste van deze tijd is besteed aan sociale netwerken en games (ongeveer 20 uur). Door 2014 had de Android markt ongeveer 1,5 miljoen toepassingen voor gebruikers om uit te kiezen. De Apple winkel bevat ongeveer 1,2 miljoen apps. App voor mobiel gebruik is nog steeds zoals ontwikkelaars concurreren in deze groeiende markt te vergroten. 

De gemiddelde mobiele gebruiker zal installeren en verwijderen van toepassingen met hoge frequentie afhankelijk van belangen te wijzigen en in app ervaringen. Bepalen van het succes van een app wordt het belangrijk te weten niet alleen hoeveel gebruikers uw app installeren. Is het belangrijk te weten hoe nuttig de app is en als die trend gebruik wordt gewijzigd. De volgende vragen zijn belangrijk:

- Zijn uw gebruikers beginnen zoeken naar uw app uninteresting of verouderd? 
- Hoeveel gebruikers zijn gestopt met behulp van uw app helemaal? 
- Worden in-app aankopen volgen van trends omhoog of omlaag?
- Gebruikers niet kunnen processen worden voltooid vanwege problemen met de app of het ontbreken van belang? 
- Kan u uw app nuttige en relevante door verse inhoud voor uw gebruikersgroep pushen? 
- Deze verse inhoud zou hetzelfde zijn voor alle gebruikers of gericht op de Gebruikersegmenten op basis van gedrag in uw app? 
 
Antwoorden op vragen van deze strekking kunnen helpen bij het uitbreiden van het leven en de omzet van uw app. Ook kunt u definiëren en behouden van uw gebruikersgroep. 

Media gerelateerde apps zijn over het algemeen zijn enkele van de hoogste handhaving tussen gebruikers. Een reden hiervoor is dat ze voortdurend vernieuwde inhoud bieden aan gebruikers. Vroege vaststelling van handig push-meldingen gericht aan een segment van de gebruiker vaak een grote invloed hebben op het behoud van de toepassing. 

Het programma Azure Mobile Engagement is ontworpen om de levensduur en het behoud van uw app uitbreiden door middel van een methode voor het verzamelen en analyseren van gedetailleerde informatie over het gebruik van uw app. Kunt u uw gebruikers volgens gedrag classificeren en gerichte campagnes voor het leveren van push-meldingen en berichten-app aan Gebruikersegmenten geïdentificeerde maken. Prestatie-indicatoren (KPI) meet hoe actief zijn met de verschillende aspecten van uw app. Azure Mobile Engagement biedt de methoden moet u deze KPI's bepalen. Het helpt bij het vergroten van het rendement op uw investering (ROI) via de infrastructuur moet u verhogen betrokkenheid bij uw mobiele app. 

U moet optimaal Azure Mobile Engagement, beginnen met een plan goed ontworpen engagement. Uw planning kunt u de gedetailleerde gegevens, moet u mogelijk uw gebruikersgroep in segmenten te identificeren. Dit kan op basis van gedrag en -app ervaringen. Om uw plan te laten slagen, is het raadzaam de KPI die de doelstellingen van uw app meet duidelijk te definiëren. Met duidelijke prestatie-indicatoren gedefinieerd, kunt u gemakkelijk de benodigde logica insluiten in uw app probleemloos korrelig gegevens die u gebruiken wilt voor het analyseren en evalueren van de KPI's te verzamelen. Dit onderwerp is een handleiding met aanbevolen procedures voor het definiëren van de KPI's die u met uw betrokkenheid bij de planning gebruiken wilt. 


## <a name="step-1-define-your-kpis-to-fit-the-bet-model"></a>Stap 1: Definieer de KPI's aanpassen aan het model BET


Definiëren van KPI's correct zijn een moeilijke taak te voltooien. Ontworpen voor verschillende industrieën Apps hebben hun eigen specifieke kenmerken en doelstellingen. Dit kan vaak verward uw benadering. Om te voorkomen dat dit, doelstellingen en KPI's dienen te worden ingedeeld in drie hoofdcategorieën: **Business**, **betrokkenheid**en **technische**. Dit is wat we noemen de **BET-model**.

Een goed plan in het algemeen hebben doelstellingen met de KPI's die de uitkomsten in elk van de volgende categorieën van de BET-model te meten.


#### <a name="business-kpis"></a>Business KPI 's

Business KPI's is de gemakkelijkste onderdeel te bouwen. U waarschijnlijk al gedefinieerd in een formulier wanneer u uw mobiele app gepland. Deze KPI's in het algemeen maatregel inkomsten en ROI voor u app. De volgende lijst bevat enkele voorbeelden van Business KPI's die u begeleiden mogelijk tijdens het definiëren van de prestatie-indicatoren:

- Media Business KPI 's
    - Aantal advertenties geklikt
    - Het aantal bezoeken per gebruiker
    - Nummer van huidige abonnementen
- Gaming Business KPI 's
    - Aantal aankopen-app
    - Gemiddelde opbrengst per gebruiker (ARPU)
    - Tijd per sessie
    - Dagen afgespeeld en het huidige spel niveau
- E-Commerce Business KPI 's
    - Dagen app gebruikt
    - Gemiddelde opbrengst per gebruiker (ARPU)
    - Gemiddeld bedrag in winkelwagen afrekenen.
    - Categorie van producten voor de meeste weergaven en aankopen
- Bank- en verzekeringswezen Business KPI 's
    - Aantal accounts
    - Functies die worden geactiveerd
    - Aanbieding pagina's bezocht
    - Waarschuwingen geklikt of geactiveerd      



#### <a name="engagement-kpis"></a>Betrokkenheid van KPI 's

Een KPI betrokkenheid is een prestatie-indicator voor het meten van de betrokkenheid van de gebruikers. Trends op dit gebied te bepalen van het behoud van uw app. Hier volgen een paar voorbeeld van prestatie-indicatoren voor dit type KPI:

- Actieve gebruikers in de laatste 7 dagen
- Aantal niet-actieve gebruikers voor de laatste 7 dagen
- Aantal gebruikers die de app niet hebt gebruikt in 30 dagen  

Enkele voor de hand liggende externe factoren kunnen van invloed zijn op indicatoren op dit gebied. Bijvoorbeeld, kunt u overwegen een mobiel apparaat te allen tijde met een gebruiker. Dit al kan dan niet true. Hogere verbruik rond de feestdagen als een gamer bij het meer uit werk of school spelen kan hebben mogelijk een app games meestal.   

Goed gedefinieerde kunt KPI's in deze categorie u de relatie tussen uw app en uw klanten te meten.



#### <a name="technical-kpis"></a>Technische KPI 's

Prestatie-indicatoren in deze categorie kunnen u bepalen als uw app is goed, vastloopt of crasht. Deze indicatoren kunnen de gezondheid van uw app meten en bruikbaarheid problemen die kunnen voorkomen dat gebruikers met behulp van de app te bepalen. Voor deze categorie verzamelde informatie kan ook informatie over de prestaties die relevant zijn voor marketing teams bevatten. Het kan ook gaan gebruiken voor het oplossen van IT en ondersteuning van teams om vast te stellen, niet-aangegeven fouten. 
 
Hier volgen enkele voorbeelden van technische KPI's:

- Van niet-verwerkte of verwerkte uitzonderingsgegevens en het aantal 
- De tijdstempel voor laatste crash
- Laatste knop geklikt of laatste pagina bezocht
- Het geheugengebruik van de app.
- Framesnelheid App
- Versie van het besturingssysteem waarop de toepassing wordt uitgevoerd
- Versie toep.

Definieer deze KPI's om te helpen de prestaties meten en app en lokaliseren van mogelijke fouten. Deze indicatoren moeten verminderen, de tijd die u nodig hebt om een oplossing te bieden voor uw klanten. Zij kunnen ook helpen u bij het definiëren van een segment voor gebruikers die een bepaalde problemen hebben ondervonden. Campagnes voor meldingen met betrekking tot oplossingen beschikbaar en mogelijke acties te herstellen van de tevredenheid van klanten maken kunt u die gebruiker Segmentatie. 


#### <a name="playbook-exercise-1-create-your-kpi-dashboard"></a>Playbook Oefening 1: Uw KPI dashboard maken

Bij het definiëren van uw marketingstrategie, dient de KPI's een weergave te presenteren voor elk van de belangrijkste doelstellingen. Ze moeten duidelijk gedefinieerde punten waarmee u voor het verzamelen van essentiële informatie voor het controleren van uw app en het gedrag van de eindgebruiker.

Bouwen van een KPI dashboard waarin de gegevens

1.  Wat zijn de KPI's voor de app?
2.  Welke gegevens punten zal ik gebruiken voor het weergeven van de KPI's?
3.  Waar bevindt deze gegevens voor de toepassing (d.w.z. scherm, instellingen, systeem...)
4.  Kan ik een reeks Engagement voor deze KPI spelen?

U kunt het werkblad **KPI Builder** in onze [Mediasjabloon Playbook] [ Media Playbook link] voor instructies en voorbeelden.



## <a name="step-2-your-engagement-program"></a>Stap 2: Uw betrokkenheid programma


Een geweldige mobiele engagement programma moet worden beschouwd als een belangrijk onderdeel van uw app. Dit dient absoluut een geweldige Welkom programma dat wordt uitgevoerd voor een gebruiker tijdens de eerste dagen van het gebruik van app omvatten. Dit leidt doorgaans tot een zeer positief effect hebben op betrokkenheid en het vasthouden van uw app. Onderzoeken hebben uitgewezen dat de meeste gebruikers de eerste paar dagen na de installatie met behulp van een app stoppen. U wilt het streven om te voldoen aan of overtreffen klant verwachting groot belang vroeg terwijl de gebruiker nog steeds op uw app gericht is. Zorg ervoor dat u de waarde van de sleutel en de voordelen van uw app presenteren aan uw klanten. 


![](./media/mobile-engagement-getting-started-best-practices/unsegmented-push-notifications.png)

Push-meldingen zijn de beste manier voor vroege betrekkingen met gebruikers van mobiele apparaten. Echter, veel zorg moet worden genomen tijdens het segmenteren van gebruikers voor push-meldingen. Omdat zodra een gebruiker iets zij ontvangen spam of uninteresting meldingen, ernstige gevolgen kan hebben. Met enkele muisklikken kan een gebruiker de toepassing nooit terug verwijderen. De gebruiker moet zeer persoonlijke in app-waarde in plaats van een algemene spam ontvangt.

Zodra gebruikers actief zijn samen, vervolgens uw betrokkenheid bij de programma kan helpen bij andere aspecten van de app drive.

U kan bijvoorbeeld een campagne die vraagt om uw actieve gebruikers om te beoordelen uw app instellingen. Aangezien dit segment van de gebruiker de meest actieve is en de meeste ervaring met je app heeft, kunt u de meest nauwkeurige beoordeling geven ze zou verwachten. Zodra u een hoge app beoordeling hebt, maar u kunt opzetten om het organische downloaden van uw app ook verminderen de aanschafkosten van de nieuwe klant.



#### <a name="engagement-sequence"></a>Betrokkenheid bij de reeks


Een globale overeenkomst opgenomen in de betrokkenheid bij de verschillende reeksen. Elke reeks beoogt verschillende doelstellingen te bereiken.


###### <a name="life-push-sequence"></a>Levensduur van de push-reeks


De doelstellingen voor een levensduur van de push-reeks zijn afhankelijk van de levenscyclus van de betrokkenheid van de gebruiker met de app. Een bepaalde gebruiker kan nieuwe, zeer actief of inactief zijn. Gebruikers kunnen in verschillende stadia van de levenscyclus een documentatie profiteren van verse inhoud in de vorm van tips en koppelingen. 

Bijvoorbeeld kan een nieuwe gebruiker help getting gericht op een app of profiteren van een nieuwe gebruiker stimulans met de volgende strekking van de eerste keer dat ze de app starten...

*"Blij dat u boord hebben! Vergeet niet om de 1e maand gratis aanmelden!"*


###### <a name="behavioral-push-sequence"></a>Volgorde doorgevoerd push

De volgorde doorgevoerd push beoogt te verhogen op basis van gebruikersgedrag verzameld voor de app gebruik.  

Bijvoorbeeld, nuttig een zeer actieve gebruiker van een fantasy football app kan met de volgende push kennisgeving te verrichten...

*'Jan u een ernstige voetbal fan bent! Log in op onze sectie NFL en vrije toegang tot de SuperBowl winnen!"*


###### <a name="alerting-push-sequence"></a>Signalering van push-reeks

Gebruikers waarderen relevant nieuws, gericht op hun belangen. Betrokkenheid bij verbetert een reeks waarschuwingen push door het verzenden van waarschuwingen op basis van de belangen van een gebruiker heeft duidelijk aangetoond. Dit kan zijn expliciete wanneer een gebruiker hun eigen belangen in de app selecteert. Ook kan worden bepaald impliciet op basis van gegevens die zijn verzameld tijdens de gebruikersinteractie met de app.

De gebruiker van een E-Commerce-app kan bijvoorbeeld regelmatig kopen voor een bepaald merk van koffie die u hebt vastgelegd met een business KPI. De betrokkenheid van de gebruiker met de app kunt verbeteren door de volgende waarschuwing.
 
*"Hi Wes, een van uw favoriete merken koffie worden in de uitverkoop 25% korting op de eerste week van September 2015. Bedankt dat u als klant en wilde om te controleren of u zich bewust was."*

###### <a name="rentention-push-sequence"></a>Push-reeks Rentention

Deze reeks beoogt te behouden met behulp van een terugkerende push notification campagnes station een regelmatige gewoonte van het werken met de app gebruikers. Dit kan helpen app inhouding verhogen als de gebruiker de interacties geniet. 

De gebruiker van een sport gerelateerde app ontvangt bijvoorbeeld de volgende push melding wekelijks op basis van favoriete teams van de gebruiker:

*"Een kans om te winnen van 200 punten, Ga voor stemming of de New York Yankees hun spel deze week tegen Toronto Blue Jays winnen zal!"*


#### <a name="the-3w-approach"></a>De aanpak van 3W

De reeksen verschillende push mastering kunt u oefenen met eindgebruikers. U moet echter nog steeds gebruik van 3W benadering voor het personaliseren van uw berichten. De aanpak van 3W kunnen aanpakken die, wat en wanneer voor elk bericht. Als u duidelijk voldoen aan deze drie vragen dat u meldingen moet goed gericht voor engagement.

![](./media/mobile-engagement-getting-started-best-practices/who-what-when.png)



###### <a name="who-the-user-segment-that-will-receive-messages"></a>Wie: De gebruiker in segmenten die berichten ontvangen

Meldingen pushen naar uw gebruikers moet worden beschouwd als een zeer gevoelige communicatiekanaal. Zorg ervoor dat de meldingen die te verzenden naar een gebruiker segment zijn gericht zijn en binnen het bereik van de belangen van dat segment van de gebruiker. Een melding van een onjuiste routering is het zeer waarschijnlijk een negatieve invloed hebben op een gebruiker. Ze kunnen beschouwen als spam leidt tot uw app verwijderd. 

Een combinatie van specifieke technische en doorgevoerd criteria gebruiken bij het definiëren van afdelingen die meldingen ontvangt. Een eenvoudig voorbeeld definieert een gebruiker segment zijn vergelijkbaar met de volgende instructie:

"Alle gebruikers die gestart het een mobiele toepassing voor de eerste keer 3 dagen geleden en de aanmeldingspagina hebt bezocht tweemaal zonder daadwerkelijk een aanmelding is voltooid '.
 
Deze instructie kunt u de gegevens die u verzamelen moet voor een specifieke scenario identificeren.


###### <a name="what-the-message-that-you-will-send"></a>Wat: Het bericht dat u verzendt

**Toon**

Gebruik in het algemeen een toon die geschikt is voor uw gebruikers gesegmenteerd. Dit is wel degelijk een goede manier om verbinding te maken met uw eindgebruikers en bevorderen van de interesse van een gebruiker in uw app. 

**Omleiding**

Een push-bericht kan worden gebruikt voor het openen van meer dan de toepassing. Als de melding een context zoals broadcast nieuws of een actie product, deze melding biedt kan diep rechtstreekse koppeling naar de juiste inhoud binnen de toepassing. Hiertoe maakt u een URL-schema waarmee de toepassing de omleiding te beheren. Als u werkt op uw betrokkenheid bij de reeksen, is dit een belangrijke stap die u moet niet vergeten.

Omleiding kan ook voor andere systemen worden beheerd. Bijvoorbeeld, is een actie-URL het mogelijk om te leiden van eindgebruikers naar veel andere systemen, waaronder de volgende:

- Een website
- Een postbus met e-mailadres is al ingesteld
- Een SMS-vak
- Een externe service
- Direct naar de winkel van toepassing voor het beoordelen van de toepassing. 

Dit biedt tal van mogelijkheden om eindgebruikers te verrichten en maak automatisch regels ter verbetering van de prestaties.


**Format-inhoud**

Verschillende typen en indelingen voor Push-melding:

1. **Aankondigingen** : Hiermee kunt u reclame berichten verzenden naar gebruikers op verschillende momenten (van app in de app of altijd).
2. **Polls** : u om informatie te verzamelen van eindgebruikers ze door vragen te stellen. Deze antwoorden zijn vervolgens beschikbaar bij het maken van criteria voor doel eindgebruikers.
3. **Gegevens duwt** : kunt u een binair of een base64-gegevensbestand aan de app update verzenden. De informatie in een push gegevens verzonden naar uw toepassing aan het afstemmen van de gebruikers in uw app. Uw toepassing moet worden ter ondersteuning van de gegevens in een gegevens-push.
4. **Tegels (alleen Windows Phone)** : u gebruikt Microsoft Push Notification Service (MPNS) voor het verzenden van Native Push-bericht met XML-gegevens (sinds versie 0.9.0 SDK wordt ondersteund. De uiteindelijke nettolading voor tegels kan niet groter zijn dan 32 kB.). Het bericht wordt weergegeven direct op de steen van uw kamer.
5. **Webweergave** : een webweergave is een pop-up met webinhoud. Dit pop-upmenu verschijnt als de eindgebruiker op een push-bericht. De weergave van een webpagina kunt u meer interactie met de eindgebruiker.
 
>[AZURE.NOTE] Zorg ervoor dat de inhoud die u als push-meldingen verzendt in overeenstemming met de respectieve platform (iOS, Android, Windows) richtlijnen voor het ontwikkelen van apps en push-meldingen te verzenden.

 


###### <a name="when-the-timing-of-your-campaign"></a>Wanneer: De timing van uw campagne

Wanneer is het beste tijdstip om een campagne activeren starten de push-meldingen? Moet het handmatig of automatisch zijn? Moet het worden een terugkerende? Bepaling van de tijd of frequentie is essentieel voor het verrichten van gebruikers met de beste resultaten. Voor elke reeks betrokkenheid en scenario moet u opgeven wanneer is de beste tijd om push-meldingen te verzenden. Hier volgen enkele mogelijke voorbeelden:

![](./media/mobile-engagement-getting-started-best-practices/campaign-timing-examples.png)

Als u veel berichten per dag verzendt, moet u ernstige overweging nemen dat uw gebruikers uw communicatie als spam kunnen zien. 

Betrokkenheid bij de mobiele Azure biedt twee manieren om te voorkomen dat uw communicatie wordt beschouwd als ongewenste e-mail. Eerste fijne korrel segmentatie gebruiken zodat u geen misbruik van dezelfde gebruikers. Azure Mobile Engagement biedt bovendien een functie 'target'. Deze functie kunt beperken dat meldingen worden verzonden voor een campagne. Bijvoorbeeld een quotumlimiet instellen op 5 per week om te zorgen dat een gebruiker die wordt opgenomen als onderdeel van de campagne gebruiker segment niet meer dan 5 meldingen voor die week ontvangt.





#### <a name="playbook-exercise-2-create-your-engagement-program"></a>Playbook Oefening 2: Uw betrokkenheid programma maken

Even samenvatten van uw doelstellingen en definiëren van de campagnes die u verwacht te spelen met behulp van specifieke reeksen. Zorg ervoor dat u de aanpak van 3W toepassen op de berichten in uw campagnes. 

Het werkblad **Betrokkenheid wordt** gebruikt in de [Mediasjabloon Playbook] [ Media Playbook link] voor instructies en voorbeelden.


## <a name="step-3-app-integration"></a>Stap 3: App integratie


#### <a name="create-a-tag-plan"></a>Een label maken

Azure Mobile Engagement integreren in uw toepassing moet u een label maken. Het tag-plan is de hoeksteen van het project. Hiermee definieert u de relatie tussen de specificaties van de marketing, de werkstroom van de toepassing en de echte tag verzamelde gegevens in de toepassing voor het meten van KPI's. Hiermee wordt aangegeven welke analytics is het mogelijk om te zien in de portal. Ook kunt u Gebruikersegmenten te definiëren en gerichte push-meldingen uit te oefenen uw eindgebruikers te verzenden. Eenmaal u de code plan hebben gedefinieerd, toevoegen van de code te integreren in uw app is eenvoudig met behulp van de SDK Azure Mobile Engagement.

Een tag plan moet alles niet code in een toepassing. Bevatten gegevens die deel uitmaakt van uw betrokkenheid bij de mobiele strategie. Dit waarschijnlijk uit diverse andere toepassingen. De [Mediasjabloon Playbook] [ Media Playbook link] bepaald door betrokkenheid bij de mobiele Azure helpt u een tag plan met een bepaalde methode maken. Het werkblad van het **Label** als een handleiding voor het bouwen van uw plan tag gebruiken.

Bij het definiëren van een sectie met code in het werkblad, worden zeer specifieke. Dit is zeer belangrijk om verwarring te voorkomen. Details voor elk scenario waarin elke code ontvangt verwacht. De naam van de activiteit waarbij elke code is ingesloten bevatten. Deze moet alle opgenomen in de **Informative** van het werkblad. Het werkblad van het label moet de belangrijkste referentie ter controle van de test. 

In de sectie **gegevens te verzamelen** en zoek het ontwikkelteam te typen, namen, waarden en extra info sleutel/waarde-paren die zijn vereist voor elke code die in de toepassing worden ingesloten.

Wij raden u aan het plan van de Tag met alle teams die aan het project bekijken. Noodzakelijke wijzigingen aanbrengen en controleren of dat alles is duidelijk voor marketing- en teams.

Het werkblad **overzicht van werkzaamheden** kan aan de hand van iedereen die betrokken bij het project zijn worden gebruikt.


#### <a name="data-types"></a>Gegevenstypen

Dit zijn algemene typen gegevensondersteuning door Azure Mobile Engagement.

###### <a name="devices-and-users"></a>Apparaten en gebruikers

Azure Mobile Engagement identificeert gebruikers door het genereren van een unieke id voor elk apparaat. Deze id wordt de apparaat-id (of deviceid) genoemd. Het wordt op zodanige wijze dat alle toepassingen die worden uitgevoerd op hetzelfde apparaat dezelfde apparaat-id delen gegenereerd.

###### <a name="sessions-and-activities"></a>Sessies en activiteiten

Een sessie wordt een exemplaar van de toepassing wordt uitgevoerd door een gebruiker. De sessie omvat vanaf het moment dat de gebruiker de toepassing, start niet meer.

Een activiteit is een logische groepering van een aantal dingen die de app tijdens een sessie doen kan. Het is meestal een bepaald venster in de app kan, maar het dat gedefinieerd door de logica van de toepassing. Minimaal moet u elk scherm of elke activiteit voor uw app labelen. Hierdoor kunt u inzicht in de gebruiker-pad.


###### <a name="events"></a>Gebeurtenissen

Gebeurtenissen worden gebruikt om gebruikersinteractie met de app. Ze kunnen worden direct acties, zoals het delen van inhoud of een video te starten. Labelen van gebeurtenissen, krijgt u met het verzamelen van gegevens die laten zien hoe gebruikers werken met de app. 

###### <a name="jobs"></a>Taken

Taken worden gebruikt om acties die een duur hebben. Enkele zou voorbeelden:

- Uitvoeren van API-aanroepen
- Tijd van advertenties weergeven
- Achtergrond taken duur 
- Aankoop proces duur
- Een video bekijken


###### <a name="errors"></a>Fouten

Fouten worden melden problemen ontdekt door de toepassing gebruikt. Bijvoorbeeld acties van de gebruiker onjuist of API-aanroep mislukt.

###### <a name="application-information"></a>Informatie over de toepassingen

Toepassingsgegevens (App Info) wordt gebruikt voor het coderen van gegevens met betrekking tot de gebruikerservaring met een toepassing. Deze wordt gegenereerd door interactie met de toepassing van de gebruiker. 

Voor een bepaalde app-info-sleutel Azure Mobile Engagement alleen houdt van de laatste waarde (geen geschiedenis). App-info geeft de status van uw app of uw eindgebruikers. Bijvoorbeeld de status log-in of een gebruikersgroep favoriete product.

###### <a name="crash-data"></a>Crash-gegevens

Gegevens worden automatisch verzameld door de SDK van Mobile Engagement rapporten toepassingsfouten niet wordt afgehandeld door de toepassing crashen. Bijvoorbeeld een niet-verwerkte uitzondering die plaatsvindt.


###### <a name="extra-data"></a>Extra gegevens

Gebeurtenissen, fouten, activiteiten en projecten kunnen met parameters worden verbeterd. Dit is een ontwikkelaar, als specifieke gegevens uit de toepassing bepalen kan van extra-informatie. Dit is belangrijk voor het definiëren van fijnmazig Segmentatie. 

Bijvoorbeeld kunt de waarde van een tag 'artikel' u voor eindgebruikers van segment op basis van die specifieke bepaling bekeken. Maar dat is niet altijd genoeg. Wellicht beter als die dezelfde 'artikel' code ook extra-info zoals 'news_category' binnen een activiteit opgenomen. Dit is handig om te bepalen dynamisch de favoriete rubrieken voor de gebruiker. 

Extra info is gerapporteerd als een sleutel/waarde-paar. In het voorbeeld voor deze mediatoepassing is de extra-info voor 'news_category' de waarde voor die categorie. Bijvoorbeeld "sports", "economie" of "politiek".





#### <a name="tag-and-sdk-integration"></a>Label en SDK-integratie 

Volg de [Integratie van betrokkenheid bij de SDK](mobile-engagement-windows-store-integrate-engagement.md) -documentatie op Azure-website voor stapsgewijze instructies voor de integratie van de Azure Mobile Engagement SDK in uw app. Kies uw doelplatform in de koppelingen aan de bovenkant van de pagina.

Wij raden aan projecten voor twee toepassingen die zijn gebaseerd op Azure Mobile Engagement te maken. Een voor de ontwikkeling en proef fasering en de andere voor het klaarzetten van productie. Uw IT-team kan vervolgens van proef fasering doorverwijzen naar productie wanneer de tests voor gebruikersaanvaarding geslaagd is.



#### <a name="user-acceptance-testing-uat"></a>Gebruikers acceptatie testen (UAT) —

Tests voor gebruikersaanvaarding (UAT) — is ervoor te zorgen dat alles werkt zoals bedoeld. Processen kunnen worden voltooid en verzamel alle vereiste gegevens op basis van de code plan:
 
- Codering van gegevens moet plaats op basis van gedocumenteerde AZME concepten worden
- Alle benodigde informatie is verzameld (met inbegrip van Extra info waarde, App info)
- Nomenclatuur volgens Plan Tag deling komt overeen met
- Er is geen dubbele labels verzonden

De typen gewenste gedrag is ingesloten in uw app grondig te testen

- Aankondigingen, Polls, gegevens worden uit de app en in de app
- Tekst/webweergaven
- Badge-update categorieën



#### <a name="setup"></a>Setup

Het instellen van Azure Mobile Engagement is heel eenvoudig. Alle documenten die betrekking hebben op de interface is beschikbaar op de website van Azure Mobile Engagement, [de gebruikersinterface te navigeren](mobile-engagement-user-interface-home.md).

Het wordt aanbevolen dat u start met het instellen van de juiste rollen en rollidmaatschappen voor de gebruikers van uw project. Dit helpt u bij het beheren van toegang tot het platform voor alle gebruikers. De rollen kunnen omvatten:

- Beheerders
- Ontwikkelaars
- Viewers 

Daarna:
- Registreer uw deviceID te testen op uw eigen apparaat.
- Ga naar de instellingen van uw account en de tijdzone instellen om grafieken en levertijd meldingen instellen voor uw tijdzone.
- Ga naar de instellingen van uw toepassing en de "App-info" u binnen handbereik doel eindgebruiker moet registreren.

Bekijk voor meer informatie over het uitvoeren van uw eerste push-bericht campagne, [het gebruik en beheer van dit gereedschap duwt te bereiken aan uw eindgebruikers werken](mobile-engagement-how-tos.md).



## <a name="conclusion"></a>Conclusie


Betrokkenheid bij de programma's zijn iteratief, en u moet uw bedrijf continu verbeteren tijdens het experimenteren met wat het beste voor uw app werkt. 

In eerste instantie tijdens de ontwikkeling van de ervaring met engagement strategieën niet probeert te maken van een volledige betrokkenheid bij de mondiale strategie. Een benadering van de identificatie van de KPI's en hoe u kunt profiteren van deze stap voor stap nemen. Betrokkenheid bij de strategie is uniek voor elke toepassing.

Nadat u enige ervaring hebt ontwikkeld kunt u overwegen het volgende toe te voegen aan uw betrokkenheid bij de programma's:

- Bijhouden: U gebruikers aan te schaffen en definieert u waarschijnlijk bronnen verzamelen van gegevens. Azure mobiele Engagement kan worden gekoppeld aan het verzamelen van gegevens-bronnen. U kunt voor het controleren van de prestaties van elke bron. Deze informatie is interessant om te maximaliseren uw belegging verwerven. 

- A / B testen: dit is een essentieel onderdeel van het programma van Engagement. Elke app heeft een eigen specifieke kenmerken. Met A / B testen, kunt u uw programma engagement verbeteren.

- Geo-locatie: Dit is een grote kans voor merken. Dankzij deze functie kunt u bereiken op de juiste plaats en tijd. Het is raadzaam om te controleren dat u hebt onvoldoende gegevens voor het gedrag van eindgebruikers verzameld voordat u begint met het gebruik van geo-locatie.

- Push gegevens: gegevens push is een onzichtbare push. Push gegevens kunt aanpassen van uw toepassing op basis van het gedrag van de eindgebruiker. Als een gebruiker segment vaak high tech-producten raadpleegt, kan de eigenaar van de app gegevens push die zal haar startpagina met high-tech inhoud aanpassen verzenden.



## <a name="next-steps"></a>Volgende stappen

- [Een betrokkenheid bij de Azure mobiele account maken](mobile-engagement-create.md).
- Ga naar [uw betrokkenheid bij de mobiele strategie definiëren](mobile-engagement-define-your-mobile-engagement-strategy.md) voor meer informatie over het definiëren van uw betrokkenheid bij de mobiele strategie.



  

<!--Image references-->


<!--Link references-->
[Media Playbook link]: https://github.com/Azure/azure-mobile-engagement-samples/tree/master/Playbooks
