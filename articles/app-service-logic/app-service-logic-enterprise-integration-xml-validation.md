<properties 
    pageTitle="Overzicht van XML-validatie in de Enterprise Integration Pack | Microsoft Azure App Service | Microsoft Azure" 
    description="Meer informatie over de werking van validatie in de Enterprise Integration Pack en logica apps" 
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

# <a name="enterprise-integration-with-xml-validation"></a>Enterprise integratie met XML-validatie

## <a name="overview"></a>Overzicht
Vaak in B2B-scenario's moeten de partners tot een akkoord valideren die berichten die deze tussen elkaar uitwisselen voordat u kunt beginnen met het verwerken van de gegevens geldig zijn. In de Enterprise Integration Pack, kunt u de connector voor de validatie van XML-documenten tegen een vooraf gedefinieerd schema valideren.  

## <a name="how-to-validate-a-document-with-the-xml-validation-connector"></a>Het valideren van een document met XML-validatie-connector
1. Maak een app logica en [koppelt u deze aan uw account integratie](./app-service-logic-enterprise-integration-accounts.md "meer een integratie-account aan een app logica te koppelen") met het schema dat u gebruiken wilt om de XML-gegevens te valideren.
2. Een trigger **aanvraag - als een HTTP-aanvraag wordt ontvangen** op uw app logica toevoegen  
![](./media/app-service-logic-enterprise-integration-xml/xml-1.png)    
3. De **XML-validatie** toevoegen door eerst te selecteren **een actie toevoegen**  
4. *Xml* invoeren in het zoekvak om alle filteracties aan die u wilt gebruiken 
5. Selecteer **XML-validatie**     
![](./media/app-service-logic-enterprise-integration-xml/xml-2.png)   
6. Selecteer het tekstvak **inhoud**  
![](./media/app-service-logic-enterprise-integration-xml/xml-1-5.png)
7. Selecteer de label body als de inhoud die wordt gevalideerd.   
![](./media/app-service-logic-enterprise-integration-xml/xml-3.png)  
8. Selecteer de keuzelijst **Naam SCHEMA** in en kies het schema dat u gebruiken wilt voor het valideren van de input- *inhoud* boven     
![](./media/app-service-logic-enterprise-integration-xml/xml-4.png) 
9. Uw werk opslaan  
![](./media/app-service-logic-enterprise-integration-xml/xml-5.png) 

Nu bent u klaar met het instellen van de validatie-connector. In een echte wereld-toepassing kunt u de gevalideerde gegevens worden opgeslagen in een LOB-toepassing als televergaderingen. U kunt gemakkelijk een actie voor het verzenden van de uitvoer van de validatie aan televergaderingen toevoegen. 

Nu kunt u de actie van de validatie door een aanvraag tot aan het eindpunt van de HTTP-testen.  

## <a name="next-steps"></a>Volgende stappen

[Meer informatie over de Enterprise Integration Pack] (./app-service-logic-enterprise-integration-overview.md "Meer informatie over Enterprise Integration Pack")   