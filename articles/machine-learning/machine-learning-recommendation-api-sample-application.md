<properties 
    pageTitle="Veelvoorkomende bewerkingen in de Machine Learning aanbevelingen API | Microsoft Azure" 
    description="Azure ML aanbeveling voorbeeldtoepassing" 
    services="machine-learning" 
    documentationCenter="" 
    authors="LuisCabrer" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/08/2016" 
    ms.author="luisca"/> 


# <a name="recommendations-api-sample-application-walkthrough"></a>Aanbevelingen API Sample Application procedure

>[AZURE.NOTE]U moet beginnen met de cognitieve aanbevelingen API-Service in plaats van deze versie. De cognitieve Service aanbevelingen zal deze service worden vervangen en de nieuwe functies zal er worden ontwikkeld. Het heeft nieuwe mogelijkheden, zoals batchen ondersteuning, een betere API Explorer, een duidelijkere API oppervlak, consistenter aanmelden/facturering ervaring, enz.
> Meer informatie over het [migreren naar de nieuwe cognitieve Service](http://aka.ms/recomigrate)

##<a name="purpose"></a>Doel

Dit document wordt het gebruik van de API Azure Machine Learning aanbevelingen via een [voorbeeldtoepassing](https://code.msdn.microsoft.com/Recommendations-144df403).

Deze toepassing is niet bedoeld om de volledige functionaliteit, en gebruik de API's. Laat enkele veelvoorkomende bewerkingen uit te voeren als u eerst wilt spelen met de computer leren aanbeveling service. 

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

##<a name="introduction-to-machine-learning-recommendation-service"></a>Inleiding tot Machine Learning aanbeveling service

Aanbevelingen via de Machine Learning aanbeveling-service zijn ingeschakeld wanneer u een aanbeveling model op basis van de volgende gegevens:

* Een opslagplaats voor de items die u wilt aan te bevelen, ook bekend als een catalogus
* Gegevens over het gebruik van artikelen per gebruiker of sessie (dit kan worden verkregen na verloop van tijd via de overname van de gegevens, niet als onderdeel van het monster app)

Nadat een aanbeveling model is gebouwd, kunt u het voorspellen van de objecten die een gebruiker mogelijk geïnteresseerd zijn in, een reeks artikelen (of een enkel item) de gebruiker selecteert.

Doe het volgende in de aanbeveling Machine Learning service zodat het vorige scenario:

* Een model maken: dit is een logische container waarin de gegevens (catalogus en gebruik) en de modellen voorspellen. Elke container model wordt geïdentificeerd via een unieke ID die wordt toegewezen wanneer deze wordt gemaakt. Deze ID wordt de model-ID genoemd en wordt gebruikt door de meeste van de API's. 
* Uploaden van catalogus: wanneer een container van het model is gemaakt, kunt u koppelen aan een catalogus.

**Opmerking**: een model maken en uploaden naar een catalogus worden meestal eenmaal uitgevoerd voor de levenscyclus van het model.

* Gebruik uploaden: dit gebruiksgegevens worden toegevoegd aan de container model.
* Bouwen van een model aanbeveling: nadat u over voldoende gegevens beschikt, kunt u de aanbeveling model bouwen. Deze bewerking wordt de bovenste Machine Learning algoritmen voor het maken van een model van de aanbeveling. Elke build is gekoppeld aan een unieke ID. U moet bijhouden van deze ID omdat het is noodzakelijk voor de functionaliteit van bepaalde API's.
* Het bouwproces volgen: een aanbeveling model build is een asynchrone bewerking en kan duurt enkele minuten tot enkele uren, afhankelijk van de hoeveelheid gegevens (catalogus en gebruik) en de build-parameters. Daarom moet u controleren de build. Een model van de aanbeveling wordt alleen gemaakt als de bijbehorende Opbouwen voltooid is.
* (Optioneel) Kies een model actieve aanbeveling build: deze stap is alleen nodig als er meer dan één aanbeveling model ingebouwd in de container voor uw model. Elk verzoek om aanbevelingen te krijgen zonder vermelding van het model actief aanbeveling wordt automatisch door het systeem naar de standaard active build omgeleid. 

**Opmerking**: een actieve aanbeveling model productie gereed is en het is gebouwd voor de werkbelasting van de productie. Dit verschilt van een niet-actieve aanbeveling model, dat in een test-achtige omgeving blijft (ook wel een gefaseerde installatie genoemd).

* Aanbevelingen krijgen: nadat u een aanbeveling model hebt, aanbevelingen voor een enkel item of een lijst met items die u hebt geselecteerd kan worden geactiveerd. 

U roept meestal aanbeveling krijgen gedurende een bepaalde periode. Tijdens die periode, kunt u de gebruiksgegevens omleiden naar de Machine Learning aanbeveling waarin deze gegevens worden toegevoegd aan de container van het opgegeven model. Wanneer er voldoende gegevens over het gebruik, kunt u een nieuwe aanbeveling model dat is uitgerust met de gegevens over het gebruik van extra te maken. 

##<a name="prerequisites"></a>Vereisten

* Visual Studio 2013
* Toegang tot het Internet 
* Abonnement op de aanbevelingen API (https://datamarket.azure.com/dataset/amla/recommendations).

##<a name="azure-machine-learning-sample-app-solution"></a>Azure Machine Learning app monsteroplossing

Deze oplossing bevat de broncode Voorbeeldgebruik, catalogusbestand en richtlijnen voor het downloaden van de pakketten die vereist voor het compileren zijn.

##<a name="the-apis-used"></a>De API's gebruikt

De toepassing gebruikt de Machine Learning aanbeveling functionaliteit via een subset van beschikbare API's. De volgende API's worden getoond in de toepassing:

* Model maken: een logische container voor het opslaan van gegevens en de aanbeveling-modellen maken. Een model wordt geïdentificeerd door een naam en u kunt meer dan één model met dezelfde naam maken.
* Catalogus uploaden: gebruiken voor het uploaden van gegevens in de catalogus.
* Gebruik uploaden: gebruiken voor het uploaden van gegevens over het gebruik.
* Build activeren: gebruik maken van een model van de aanbeveling.
* Vlotte uitvoering build: gebruiken voor het controleren van de status van een aanbeveling model bouwen.
* Kies een model gebouwd voor aanbeveling: gebruikt om aan te geven welk model aanbeveling om standaard te gebruiken voor een bepaalde container in het model. Deze stap is alleen nodig als er meer dan één model van de aanbeveling en u een niet-actieve build als model voor de actieve aanbeveling activeren.
* Aanbeveling krijgen: aanbevolen artikelen op basis van één bepaald item of een groep objecten ophalen. 

Zie de documentatie van Microsoft Azure Marketplace voor een volledige beschrijving van de API's. 

**Opmerking**: een model kan hebben verschillende builds na verloop van tijd (niet tegelijkertijd). Elke build wordt gemaakt met dezelfde of een bijgewerkte catalogus en aanvullende gegevens.

## <a name="common-pitfalls"></a>Veelvoorkomende valkuilen

* U moet uw gebruikersnaam en uw Microsoft Azure Marketplace primaire sleutel uit te voeren van het monster app.
* En met het monster app zal mislukken. De stroom van de toepassing omvat maken, uploaden voor het bouwen van de monitor en aanbevelingen ophalen uit een vooraf gedefinieerd model; Daarom zal mislukken op opeenvolgende worden uitgevoerd als u de naam van het model tussen aanroepen niet wijzigen.
* Aanbevelingen kunnen geven zonder gegevens. Het monster app maakt gebruik van een zeer kleine catalogus en het gebruik van het bestand. Sommige items uit de catalogus heeft daarom geen aanbevolen producten.

## <a name="disclaimer"></a>Disclaimer
Het monster app is niet bedoeld om te worden uitgevoerd in een productieomgeving. De gegevens in de catalogus is erg klein en biedt geen zinvolle aanbeveling model. De gegevens worden verstrekt als een demonstratie. 
 
