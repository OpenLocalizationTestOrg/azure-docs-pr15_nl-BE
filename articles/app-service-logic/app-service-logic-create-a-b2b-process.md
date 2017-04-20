<properties 
   pageTitle="Maken van een B2B-proces in Azure App Service | Microsoft Azure" 
   description="Overzicht van het maken van een Business-to-Business proces" 
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

# <a name="creating-a-b2b-process"></a>Maken van een B2B-proces

[AZURE.INCLUDE [app-service-logic-version-message](../../includes/app-service-logic-version-message.md)]


## <a name="business-scenario"></a>Bedrijfsscenario 
Contoso en Noordenwind zijn twee zakelijke partners. Contoso (detailhandelaar) stuurt inkooporders naar Noordenwind (leverancier) via een niveau transport industrie zoals AS2. Noordenwind slaat alle inkomende orders in de Cloud-opslag. De inkooporders zijn XML-berichten tussen de twee partners. Zodra het bericht wordt opgeslagen in de cloud-opslag van Noordenwind verwerken van Noordenwind interne processen de order vanaf dat punt op.
 
Het doel van deze zelfstudie is vast te stellen hoe Noordenwind een bedrijfsproces waarlangs kan ontvangen (inkooporders in XML) van de partner Contoso via AS2 en deze vervolgens in de Cloud opslag aanhouden tot stand kan brengen.


## <a name="capabilities-demonstrated"></a>Mogelijkheden aangetoond 
Deze zelfstudie kunt hier de volgende mogelijkheden: 

- **Vervoer van bericht**: de detailhandelaar en de leverancier op verschillende platforms kunnen worden, maar zij nog steeds de communicatie tussen de twee kunnen bereiken. In deze zelfstudie ze communiceren via AS2 (Applicability Statement 2). AS2 is een populaire manier om gegevens tussen handelspartners in business-to-business communicatie transport.
- **Persistentie van gegevens**: zodra het bericht is ontvangen via AS2 en Noordenwind wil behouden deze vóór verdere verwerking. Het kunt een verbindingslijn gebruiken om berichten in de Cloud-opslag. In deze zelfstudie is als de opslag van de wolk voor Noordenwind Azure BLOB's worden gebruikt.
- **Maken van een bedrijfsproces**: In een stroom, meerdere API apps samen kunnen worden stitched om een business resultaat behalen, hier.


## <a name="before-you-begin"></a>Voordat u begint
In deze zelfstudie wordt ervan uitgegaan dat u een basiskennis van Azure App Services hebben, weten hoe API apps maken en een stroom bij elkaar BIND.


## <a name="steps-to-achieve-the-business-scenario"></a>Stappen voor het bereiken van het scenario
**Maken en configureren van de vereiste API apps**

1. Maak een instantie van de **Azure opslag Blob-Connector**. Hiervoor moet de referenties voor een account Azure opslag. Zorg ervoor dat deze gereed is voordat u deze maken.
2. Een exemplaar maken van het **Beheer van handel BizTalk-Partner**. U moet hiervoor een lege Database SQL functie. Zorg ervoor dat deze gereed is voordat u deze maken.
3. Maak een instantie van de **AS2-Connector**. Dit vereist ook een lege Database SQL functie. Zorg ervoor dat deze gereed is voordat u deze maken. Ook als u wilt dat berichten archiveren als onderdeel van AS2 verwerken, kunt u desgewenst referenties naar een Azure Blob tijdens het maken.
4. Configureer de TPM (Trading Partner Management)-service die wordt gemaakt:  
    1. Ga naar het exemplaar van de service van de TPM gemaakt als onderdeel van de bovenstaande stappen.
    2. Gebruik de optie **Partners** onder *onderdelen* voor het **toevoegen** van een nieuwe partner met de naam **Contoso** en in het profiel de identiteit van de vereiste AS2 toevoegen.
    3. Gebruik de optie **Partners** onder *onderdelen* voor het **toevoegen** van een nieuwe partner **Noordenwind** en in het profiel de identiteit van de vereiste AS2 toevoegen.
    4. Gebruik de optie **overeenkomsten** onder *onderdelen* voor het **toevoegen** van een nieuwe AS2 overeenkomst tussen Noordenwind en Contoso. Noordenwind is hier de gehoste partner en Contoso de Gast partner zal zijn. Passende ondertekening, codering, compressie en bevestigingen configureren tijdens het maken van deze overeenkomst. Certificaten moeten worden gebruikt, wanneer deze kunnen worden geüpload via de optie **certificaten** tijdens het bladeren door de TPM-service die wordt gemaakt.


## <a name="create-a-flow--business-process"></a>Maak een stroom / business proces
1. Maak een nieuwe stroom waarin de eerste stap AS2 is. Slepen en neerzetten de **AS2-Connector** en kies het exemplaar al gemaakt. Kies de trigger als de functionaliteit:  
    ![][1]  
2. Vervolgens slepen en neerzetten **Azure opslag Blob Connector** en kiest u de sessie al gemaakt. Actie als de functionaliteit en binnen die kiezen, selecteert u **Blob uploaden** als de gewenste functionaliteit. Configureer zo nodig.
3. Nu maken/implementeren van de stroom.


## <a name="message-processing--troubleshooting"></a>Berichtverwerking en probleemoplossing
1. Het is tijd voor het testen van de stroom die we hebben geïmplementeerd. Tekstterugloop in AS2 (volgens de hierboven gemaakte AS2-overeenkomst) van XML-berichten naar het eindpunt AS2 is opgehaald door de AS2Connector-exemplaar dat u hebt gemaakt verzenden. Wellicht moet u de verificatie configureren voor het eindpunt zodat het publiek toegankelijk is.
2. Informatie over de stroom uitvoering zichtbaar is door te bladeren naar de stroom en vervolgens stap voor stap in de stroom-exemplaar die u hebt uitgevoerd
3. Ga naar de AS2Connector instantie betrokken AS2 verwerking van informatie, en volg stapsgewijs te doorlopen in het gedeelte bijhouden. U kunt de filters die de weergave beperken tot de informatie die nodig is.

![][2]

<!--Image references-->
[1]: ./media/app-service-logic-create-a-b2b-process/Flow.png
[2]: ./media/app-service-logic-create-a-b2b-process/Tracking.png
 
