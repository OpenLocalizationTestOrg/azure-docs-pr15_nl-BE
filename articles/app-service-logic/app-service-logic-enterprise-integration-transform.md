<properties 
    pageTitle="Overzicht van Enterprise Integration Pack | Microsoft Azure App Service | Microsoft Azure" 
    description="Gebruik de functies van Enterprise Integration Pack business proces van integratie en scenario's met Microsoft Azure App-service inschakelen" 
    services="logic-apps" 
    documentationCenter=".net,nodejs,java"
    authors="msftman" 
    manager="erikre" 
    editor="cgronlun"/>

<tags 
    ms.service="logic-apps" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/08/2016" 
    ms.author="deonhe"/>

# <a name="enterprise-integration-with-xml-transforms"></a>Enterprise integratie met XML-transformaties

## <a name="overview"></a>Overzicht
De Enterprise integration transformatie-connector worden gegevens van de ene indeling naar een andere indeling geconverteerd. Bijvoorbeeld, wellicht een binnenkomend bericht met de huidige datum in de notatie YearMonthDay. U kunt een transformatie opnieuw formatteren van de datum in de notatie MonthDayYear.

## <a name="what-does-a-transform-do"></a>Wat is een transformatie?
Een transformatie die ook wel een kaart wordt, bestaat uit een bron XML-schema (input) en een doel XML-schema (uitvoer). U kunt verschillende ingebouwde functies om te manipuleren of controle van de gegevens, met inbegrip van de tekenreeks te springen, voorwaardelijke toewijzingen, rekenkundige expressies, datum tijd formatters en zelfs herhaling constructies.

## <a name="how-to-create-a-transform"></a>Het maken van een transformatie?
Met behulp van de Visual Studio [Enterprise Integration-SDK](https://aka.ms/vsmapsandschemas)kunt u een transformatie/toewijzen. Wanneer u klaar bent met het maken en testen van de transformatie, kunt u de transformatie uploaden naar uw rekening integratie. 

## <a name="how-to-use-a-transform"></a>Het gebruik van een transformatie
Nadat u de transformatie hebt geüpload naar uw account integratie, kunt u het maken van een app logica. De app logica wordt dan uw transformaties uitvoeren als de app logica wordt geactiveerd (en invoer inhoud die moet worden omgezet).

**Hier volgen de stappen voor het gebruik van een transformatie**:

### <a name="prerequisites"></a>Vereisten 
In het voorbeeld ziet moet u:  

-  [Een container Azure functies maken] (https://ms.portal.azure.com/#create/Microsoft.FunctionApp "Een container Azure functies maken")  
-  [Een functie aan de container Azure functies toevoegen] (https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-logic-app-transform-function%2Fazuredeploy.json "Met deze sjabloon maakt u een webhook op basis van C# azure functie met mogelijkheden voor transformatie gebruiken in scenario's logica apps integratie")    
-  Maak een account integratie en een kaart aan toe te voegen.  

>[AZURE.TIP] Maak een notitie van de de naam van de container Azure-functies en de functie Azure, moet u deze in de volgende stap.  

Nu u van de vereisten hebt afgehandeld, is het tijd voor het maken van uw app logica:  

1. Maak een app logica en [koppelt u deze aan uw account integratie](./app-service-logic-enterprise-integration-accounts.md "meer een integratie-account aan een app logica te koppelen") met de kaart.
2. Een trigger **aanvraag - als een HTTP-aanvraag wordt ontvangen** op uw app logica toevoegen  
![](./media/app-service-logic-enterprise-integration-transforms/transform-1.png)    
3. Het **XML-transformatie** toevoegen door eerst te selecteren **een actie toevoegen**   
![](./media/app-service-logic-enterprise-integration-transforms/transform-2.png)   
4. Het woord *transformatie* invoeren in het zoekvak om alle filteracties aan die u wilt gebruiken  
![](./media/app-service-logic-enterprise-integration-transforms/transform-3.png)  
5. Selecteer de **XML-transformatie** -actie   
![](./media/app-service-logic-enterprise-integration-transforms/transform-4.png)  
6. Selecteer de **Functie CONTAINER** waarin de functie die u wilt gebruiken. Dit is de naam van de functies Azure container die u eerder in deze procedure hebt gemaakt.
7. Selecteer de **functie** die u wilt gebruiken. Dit is de naam van de Azure-functie die u eerder hebt gemaakt.
8. Voeg de XML- **inhoud** die u zal transformeren. Houd er rekening mee dat u kunt XML-gegevens die u in de HTTP-aanvraag als de **inhoud ontvangt**. Selecteer in dit voorbeeld wordt het hoofdgedeelte van de HTTP-aanvraag die de logica app geactiveerd.
9. Selecteer de naam van de **MAP** die u gebruiken wilt voor het uitvoeren van de transformatie. De map moet al in uw account integratie. In een eerdere stap heeft u al uw logica app toegang aan uw account integratie met de kaart.
10. Uw werk opslaan  
![](./media/app-service-logic-enterprise-integration-transforms/transform-5.png) 

Nu bent u klaar met het instellen van de kaart. In een echte wereld-toepassing kunt u de getransformeerde gegevens worden opgeslagen in een LOB-toepassing als televergaderingen. U kunt gemakkelijk als een actie voor het verzenden van de uitvoer van de transformatie naar televergaderingen. 

Nu kunt u de transformatie testen door een aanvraag naar de HTTP-eindpunt.  

## <a name="features-and-use-cases"></a>Functies en use-cases

- De transformatie is gemaakt in een map is eenvoudig, zoals het kopiëren van een naam en adres van het ene document naar het andere. Of kunt u complexe transformaties in de map out-of-the-box-bewerkingen.  
- Verschillende bewerkingen van de kaart of functies zijn direct beschikbaar, met inbegrip van tekenreeksen, datum-tijd-functies, enzovoort.  
- U kunt een kopie van de gegevens direct tussen de schema's doen. Bij de toewijzing die is opgenomen in de SDK, is dit net zo eenvoudig als een lijn die de elementen in het schema van de gegevensbron met hun tegenhangers in het doelschema verbindt.  
- Wanneer u een toewijzing maakt, weergave u een grafische van de kaart, die alle relaties en koppelingen die u maakt.
- Gebruik de functie Test kaart een voorbeeld-XML-bericht toevoegen. U kunt met een enkele klik, testen van de kaart die u hebt gemaakt en Zie de gegenereerde output.  
- Bestaande kaarten uploaden  
- Biedt ondersteuning voor de XML-indeling.


## <a name="learn-more"></a>Meer informatie
- [Meer informatie over de Enterprise Integration Pack] (./app-service-logic-enterprise-integration-overview.md "Meer informatie over Enterprise Integration Pack")  
- [Meer informatie over toewijzingen] (./app-service-logic-enterprise-integration-maps.md "Meer informatie over de integratie in de onderneming worden toegewezen")  
 