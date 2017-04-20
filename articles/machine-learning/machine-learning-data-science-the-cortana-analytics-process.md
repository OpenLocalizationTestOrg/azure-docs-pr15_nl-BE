<properties 
    pageTitle="Wat is Team gegevens wetenschap proces?  | Microsoft Azure" 
    description="Het Team gegevens wetenschap proces is een systematische methode voor het maken van intelligente toepassingen die gebruikmaken van geavanceerde analytics." 
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


# <a name="what-is-the-team-data-science-process-tdsp"></a>Wat is het Team gegevens wetenschap proces (TDSP)?

Het [Team gegevens wetenschap proces (TDSP)](data-science-process-overview.md) biedt een systematische aanpak voor intelligente toepassingen te bouwen waardoor teams van wetenschappers gegevens effectief samen te werken gedurende de volledige levenscyclus van de activiteiten die nodig zijn om deze toepassingen in de producten. Een overzicht van de TDSP een reeks stappen met **informatie** over het definiëren van het probleem, stelt u de hulpprogramma's en de omgeving nodig, relevante gegevens analyseren, bouwen en voorspellende modellen evalueren en implementeert u deze modellen in de bedrijfstoepassingen. 

Hier zijn de stappen in de **Procedure voor het Team gegevens wetenschap**:  

![CAP-werkstroom](./media/machine-learning-data-science-the-cortana-analytics-process/CAP-workflow.png)

Het **iteratieve**is: het begrijpen van nieuwe en bestaande of verfijningen in het model wordt uitgebreid en vereist omwerking van de stappen die eerder in de reeks is voltooid. Bestaande organisatie-ontwikkeling en projectplanning processen zijn **gemakkelijk worden aangepast** om te werken met de TDSP omschreven volgorde van stappen. 

De stappen in het proces worden in kaart gebracht en gekoppeld aan het [pad van TDSP leren](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) en die hieronder worden beschreven.  

## <a name="preparation-steps"></a>Voorbereidende stappen 

## <a name="p1-plan-the-analytics-project"></a>P1. Het project analytics plannen 

Een analytics-project starten door het definiëren van uw zakelijke doelstellingen en problemen. Deze zijn opgegeven voor de **zakelijke vereisten**. Een centrale doelstelling van deze stap is voor het identificeren van de variabelen voor belangrijke zakelijke (verkoopprognose of de waarschijnlijkheid van een order wordt frauduleuze, bijvoorbeeld) die de analyse moet worden om te voldoen aan deze eisen te voorspellen. Extra planning vervolgens meestal essentieel is voor het ontwikkelen van een goed begrip van de **gegevensbronnen** die nodig zijn om de doelstellingen van het project vanuit een analytisch perspectief. Het is bijvoorbeeld niet ongebruikelijk is, te vinden van de bestaande systemen voor het verzamelen en melden van aanvullende soorten gegevens aan het probleem en de verwezenlijking van de projectdoelstellingen. Raadpleeg [uw omgeving voor het Team gegevens wetenschap proces plannen](machine-learning-data-science-plan-your-environment.md) en [scenario's voor geavanceerde analytics in Azure Machine Learning](machine-learning-data-science-plan-sample-scenarios.md).  

## <a name="p2-setup-analytics-environment"></a>P2. Analytics omgeving instellen 

Een analytics omgeving voor het Team gegevens wetenschap proces bestaat uit verschillende onderdelen: 

- **gegevens werkruimten** waar de gegevens voor de analyse en modellering wordt klaargezet 
- een **verwerking van infrastructuur** voor het vooraf verwerken en modellering van de gegevens verkennen
- een **runtime-infrastructuur** mogelijk maken van de analytische modellen en het uitvoeren van de intelligente clienttoepassingen die verbruiken de modellen.  

De analytics-infrastructuur die moet worden ingesteld uitmaakt vaak deel van een omgeving die los staat van de core operationele systemen. Maar deze meestal maakt gebruik van gegevens uit meerdere systemen binnen de onderneming als bronnen buiten het bedrijf. De infrastructuur analytics zuiver cloud-gebaseerde, kan zijn of een installatie op locatie, of een mengvorm van beide. Zie [gegevens wetenschap omgevingen voor gebruik in het Team gegevens wetenschap proces](machine-learning-data-science-environment-setup.md)voor opties.

## <a name="analytics-steps"></a>Analytics stappen:  

## <a name="1-ingest-data-into-the-analytical-environment"></a>1. consumptie van gegevens in het analytische milieu 

De eerste stap is om te zorgen dat de relevante gegevens uit verschillende bronnen, zowel van binnen als van buiten de onderneming, in een analytisch omgevingen waar de gegevens kunnen worden verwerkt. De **indeling** van de gegevens aan de bron kan afwijken van de indeling van de bestemming. Dus wellicht sommige gegevenstransformatie ook worden uitgevoerd door de opname tooling. Zie [gegevens laden in omgevingen met opslag voor analytics](machine-learning-data-science-ingest-data.md) voor opties

Naast de eerste opname van de gegevens moeten veel intelligente toepassingen de gegevens regelmatig te vernieuwen als onderdeel van een continu leerproces. Dit kan worden gedaan door het instellen van een **pijpleiding van de gegevens** of de werkstroom. Dit maakt deel uit van het iteratieve deel van het proces met opnieuw opbouwen en de analytische modellen die worden gebruikt door de intelligente toepassing implementeren de oplossing opnieuw te evalueren. Zie bijvoorbeeld [gegevens van een lokale SQL server SQL Azure met Azure Data Factory verplaatsen](machine-learning-data-science-move-sql-azure-adf.md).


## <a name="2-explore-and-pre-process-data"></a>2. verkennen en vooraf gegevens verwerken 

De volgende stap is het verkrijgen van een beter begrip van de gegevens door te kijken naar haar **samenvattingsinfo** , relaties, en met behulp van technieken dergelijke **visualisatie**. Dit is ook waar de problemen van de **kwaliteit van de gegevens** en integriteit, zoals ontbrekende waarden, niet-overeenkomende typen gegevens en inconsistente gegevensrelaties, worden verwerkt. Pre-processing transformaties worden gebruikt voor het opschonen van de onbewerkte gegevens voordat verdere analytics en modellering kan plaatsvinden. Zie voor een beschrijving van [taken voorbereiden van gegevens voor verbeterde machine leren](machine-learning-data-science-prepare-data.md).


## <a name="3-develop-features"></a>3. functies ontwikkelen 

Gegevens wetenschappers, in samenwerking met deskundigen van het domein, moeten omgaan met de functies die de belangrijke eigenschappen van de gegevensset vastleggen en die best kan worden gebruikt om de belangrijkste zakelijke variabelen geïdentificeerd tijdens de planning. Deze nieuwe functies kunnen worden afgeleid van bestaande gegevens of u mogelijk aanvullende gegevens moeten worden verzameld. Dit proces staat bekend als een **technische functie** en is een van de belangrijkste stappen bij het bouwen van een effectieve voorspellende analytics. Deze stap is vereist voor een creatieve combinatie van expertise in verschillende domeinen en de inzichten verkregen uit de gegevens verkennen stap. Zie de [functie engineering in het Team gegevens wetenschap proces](machine-learning-data-science-create-features.md)voor instructies.


## <a name="4-create-predictive-models"></a>4. voorspellende modellen maken 

Gegevens wetenschappers bouwen analytische modellen te voorspellen wat de belangrijkste variabelen, aangeduid met de zakelijke vereisten gedefinieerd in de planning stap met gegevens dat is opgeschoond en featurized. Machine ondersteuning learning voor meerdere **modellen algoritmen** die van toepassing zijn op een groot aantal gevallen. Raadpleeg [het kiezen van algoritmen voor Team Azure Machine Learning](machine-learning-algorithm-choice.md).

Wetenschappers van de gegevens het meest passende model voor hun taak voorspelling moeten kiezen en het is niet ongebruikelijk dat de resultaten van meerdere modellen worden gecombineerd moeten voor de beste resultaten. De ingevoerde gegevens voor modellering is meestal willekeurig verdeeld in drie delen:

- een gegevensset training 
- een gegevensset validatie 
- een gegevensset testen 

De modellen worden gebouwd met behulp van de **gegevensset training**. De optimale combinatie van modellen (met parameters afgestemd) is geselecteerd door de modellen en de fouten van de voorspelling voor de **validatie-gegevensset**meten. Ten slotte wordt de **gegevensset test** gebruikt voor de evaluatie van de prestaties van het gekozen model van de onafhankelijke gegevens die niet is gebruikt om te trainen of valideren van het model.  Zie voor procedures, [het evalueren van model prestaties in Azure Machine Learning](machine-learning-evaluate-model-performance.md).


## <a name="5-deploy-and-consume-models"></a>5. implementeren en verbruiken modellen 

Zodra we een aantal modellen die goed worden uitgevoerd hebben, kunnen ze worden **geoperationaliseerd** voor andere toepassingen te gebruiken. Afhankelijk van de bedrijfsvereisten voorspellingen bestaan in **real time** of op basis van de **batch** . Als u wilt worden geoperationaliseerd, de modellen met een **open API-interface** die gemakkelijk vanuit verschillende toepassingen wordt verbruikt worden blootgesteld hebben dergelijke website online, spreadsheets, dashboards of bedrijfs- en back-end-toepassingen. Zie [een webservice Azure Machine Learning implementeren](machine-learning-publish-a-machine-learning-web-service.md).

## <a name="summary-and-next-steps"></a>Samenvatting en volgende stappen

Het [Team gegevens wetenschap proces](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) wordt gemodelleerd als een reeks stappen herhaald die **richtsnoeren te verschaffen** over de taken die nodig zijn voor een intelligente toepassingen maken gebruik van geavanceerde analytics. Elke stap bevat ook informatie over het gebruik van verschillende Microsoft-technologieën voor het voltooien van de taken die worden beschreven. 

TDSP schrijft niet voor specifieke soorten **documentatie** artefacten, is het raadzaam om de resultaten van de gegevens te verkennen, modellering en evaluatie en herhaald als dat nodig is om de relevante code zodanig opslaan dat kan de analyse. Hierdoor kunnen ook hergebruik van het werk van analytics voor andere toepassingen met vergelijkbare gegevens en taken voorspelling.

Volledige end-to-end-scenario's die aantonen de stappen in het proces voor het **specifieke scenario's dat** zijn ook beschikbaar. Zie, bijvoorbeeld:

- [Het Team gegevens wetenschap proces in actie: met behulp van SQL Server](machine-learning-data-science-process-sql-walkthrough.md)
- [Het Team gegevens wetenschap proces in actie: HDInsight Hadoop clusters met](machine-learning-data-science-process-hive-walkthrough.md).
- [Gegevens wetenschap met vonkontsteking in Azure, HD.mdnsight](machine-learning-data-science-spark-overview.md)
- [Schaalbare Data Science in Azure gegevens Lake: een end-to-end-overzicht](machine-learning-data-science-process-data-lake-walkthrough.md)

 
