<properties 
   pageTitle="B2B berichten in uw apps logica in Azure App-Service traceren | Microsoft Azure" 
   description="In dit onderwerp worden de tracking van de B2B verwerking" 
   services="logic-apps" 
   documentationCenter=".net,nodejs,java" 
   authors="rajram" 
   manager="erikre" 
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="04/20/2016"
   ms.author="rajram"/>


# <a name="track-b2b-messages"></a>B2B berichten traceren

[AZURE.INCLUDE [app-service-logic-version-message](../../includes/app-service-logic-version-message.md)]

## <a name="b2b-tracking-information"></a>B2B controle-informatie
B2B-communicatie betreft tussen handelspartners verwerken bericht. De relaties zijn gedefinieerd als overeenkomsten tussen twee handelspartners. Zodra de communicatie tot stand is gebracht, moet er een manier om te controleren als de communicatie plaatsvindt, zoals verwacht. 

We hebben geïmplementeerd bijhouden van berichten voor de volgende scenario's voor B2B: AS2, EDIFACT, en X12.

## <a name="as2"></a>AS2
Als u een exemplaar van een AS2-API App hebt gemaakt, bladert u naar die instantie en selecteer **bijhouden**. Hierin kunt u bekijken en alle AS2 tracking informatie te filteren:  

![][1]  

## <a name="edifact"></a>EDIFACT
Als u een exemplaar van een EDIFACT API App hebt gemaakt, bladert u naar die instantie en selecteer **bijhouden**. Hierin kunt u bekijken en filteren alle EDIFACT informatie bijhouden. Bovendien kunt u het niveau van de uitwisseling, groepeerniveau weergeven en transactie gegevens niveau, alles in één weergave instellen. 

Als partijen worden gemaakt als onderdeel van de overeenkomsten in de bijbehorende Trading Partner beheer API app EDIFACT, bevat de sectie Batching deze partijen. U kunt een batch voor een overzicht van het actieve bericht (indien aanwezig) en ook de informatie voor de voltooide selecteren:  

![][2]      

## <a name="x12"></a>X12
Als u een exemplaar van een X12 hebt gemaakt API App, en blader naar dat exemplaar, **bijhouden**. Hierin kunt u bekijken en filteren van alle X12 gegevens bijhouden. Bovendien kunt u het niveau van de uitwisseling, groepeerniveau weergeven en transactie gegevens niveau, alles in één weergave instellen.

Als partijen worden gemaakt als onderdeel van X12 overeenkomsten in de bijbehorende app voor Trading Partner beheer-API wordt de Batching sectie geeft een overzicht van deze partijen. Kunt u een batch voor een overzicht van het actieve bericht (indien aanwezig) en ook de informatie voor de voltooide batches.

<!--Image references-->
[1]: ./media/app-service-logic-track-b2b-messages/AS2Tracking.png
[2]: ./media/app-service-logic-track-b2b-messages/EDIFACTTracking.png
