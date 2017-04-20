<properties
    pageTitle="Leer te coderen of decoderen van bestanden met behulp van de Enterprise Integration Pack en logica apps | Microsoft Azure App Service | Microsoft Azure"
    description="Met de functies van Enterprise Integration Pack en logica apps kunt coderen of decoderen van bestanden"
    services="app-service\logic"
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

# <a name="enterprise-integration-with-flat-files"></a>Integratie in de onderneming met platte bestanden.

## <a name="overview"></a>Overzicht

U kunt XML-inhoud coderen voordat u deze naar een zakelijke partner in een business-to-business (B2B verzendt). In een logica app gemaakt door de functie logica Apps van de App Azure Service, kunt u de plat bestand codering connector om dit te doen. De logica app die u maakt, krijgt de XML inhoud uit tal van bronnen, met inbegrip van een HTTP-aanvraag-trigger, vanuit een andere toepassing of zelfs van een van de vele [aansluitingen](../connectors/apis-list.md). Zie voor meer informatie over apps logica, de [logica apps documentatie](./app-service-logic-what-are-logic-apps.md "meer informatie over Logic apps").  

## <a name="how-to-create-the-flat-file-encoding-connector"></a>De platte bestand codering verbindingslijn maken

Volg deze stappen om een flat connector om uw app logica te coderen.

1. Maak een app logica en [koppelt u deze aan uw account integratie](./app-service-logic-enterprise-integration-accounts.md "meer een integratie-account aan een app logica te koppelen"). Deze rekening bevat het schema dat u gebruiken wilt voor het coderen van de XML-gegevens.  
2. Een trigger **aanvraag - als een HTTP-aanvraag wordt ontvangen** aan uw app logica toevoegen.  
![Screenshot van trigger selecteren](./media/app-service-logic-enterprise-integration-flatfile/flatfile-1.png)    
3. De platte bestanden coderen actie, als volgt toevoegen:

    een. Selecteer het **plus** -teken.

    b. Selecteer de koppeling **toevoegen een actie** (wordt weergegeven nadat u het plusteken hebt geselecteerd).

    c. Voer *Flat* om de acties op het gewenste filter in het zoekvak.

    d. Selecteer de optie **Platte bestandscodering** in de lijst.   
![Screenshot van platte bestandscodering optie](./media/app-service-logic-enterprise-integration-flatfile/flatfile-2.png)   
4. Selecteer in het tekstvak **inhoud** op de **Platte bestand coderen** in het dialoogvenster.  
![Screenshot van de inhoud van het tekstvak](./media/app-service-logic-enterprise-integration-flatfile/flatfile-3.png)  
5. Selecteer de label body als de inhoud die u wilt coderen. De label body vult het veld inhoud.     
![Schermafbeelding van een body-tag](./media/app-service-logic-enterprise-integration-flatfile/flatfile-4.png)  
6. Selecteer de keuzelijst **Schema** en kies het schema dat u gebruiken wilt voor het coderen van de inhoud van de invoer.    
![Screenshot van Schema keuzelijst](./media/app-service-logic-enterprise-integration-flatfile/flatfile-5.png)  
7. Sla uw werk op.   
![Pictogram Screenshot opslaan](./media/app-service-logic-enterprise-integration-flatfile/flatfile-6.png)  

Nu bent u klaar met uw codering connector plat bestand instellen. In een echte wereld-toepassing kunt u de gecodeerde gegevens worden opgeslagen in een lijn-of-business-toepassing, zoals televergaderingen. Of u dat gecodeerde gegevens naar een trading partner kunt verzenden. U kunt een actie voor het verzenden van de uitvoer van de codering actie op televergaderingen of op uw handelspartner met behulp van een van de andere verbindingslijnen die gemakkelijk toevoegen.

Nu kunt u de verbindingslijn testen door het doen van een aanvraag tot aan het eindpunt van de HTTP- en met inbegrip van de XML-inhoud in de hoofdtekst van de aanvraag.  

## <a name="how-to-create-the-flat-file-decoding-connector"></a>De platte bestand decoderen verbindingslijn maken

>[AZURE.NOTE] Als u deze stappen hebt uitgevoerd, moet u een schemabestand al geüpload rekening u integratie hebt.

1. Een trigger **aanvraag - als een HTTP-aanvraag wordt ontvangen** aan uw app logica toevoegen.  
![Screenshot van trigger selecteren](./media/app-service-logic-enterprise-integration-flatfile/flatfile-1.png)    
2. De platte bestanden decoderen actie, als volgt toevoegen:

    een. Selecteer het **plus** -teken.

    b. Selecteer de koppeling **toevoegen een actie** (wordt weergegeven nadat u het plusteken hebt geselecteerd).

    c. Voer *Flat* om de acties op het gewenste filter in het zoekvak.

    d. Selecteer de optie **Decoderen van platte bestand** uit de lijst.   
![Optie Screenshot van platte bestand decoderen](./media/app-service-logic-enterprise-integration-flatfile/flatfile-2.png)   
- Selecteer **het inhoudsbesturingselement** . Dit resulteert in een lijst met de inhoud van de eerdere stappen die u als de inhoud gebruiken kunt decoderen. U ziet dat de *hoofdtekst* van de binnenkomende HTTP-aanvraag beschikbaar om te worden gebruikt als de inhoud is decoderen. Ook kunt u de inhoud rechtstreeks in het besturingselement voor **inhoud** decoderen.     
- Selecteer de label *Body* . U ziet dat de label body is nu in **het inhoudsbesturingselement** .
- Selecteer de naam van het schema dat u gebruiken wilt voor het decoderen van de inhoud. Het volgende screenshot toont aan dat *OrderFile* de naam van het geselecteerde schema is. De naam van dit schema hadden eerder de integratie rekening geüpload.

 ![Het dialoogvenster Screenshot van platte bestand decoderen](./media/app-service-logic-enterprise-integration-flatfile/flatfile-decode-1.png)    
- Sla uw werk op.  
![Pictogram Screenshot opslaan](./media/app-service-logic-enterprise-integration-flatfile/flatfile-6.png)    

Nu bent u klaar met het instellen van uw plat bestand decoderen connector. In een concrete toepassing kunt u de gecodeerde gegevens opslaan in een bedrijfs toepassing, zoals televergaderingen. U kunt gemakkelijk een actie om de uitvoer van de decodering actie verzenden naar televergaderingen toevoegen.

Nu kunt u de verbindingslijn testen door het doen van een aanvraag tot aan het eindpunt van de HTTP- en met inbegrip van de XML-inhoud die u wilt decoderen in het hoofdgedeelte van de aanvraag.  

## <a name="next-steps"></a>Volgende stappen
- [Meer informatie over de Enterprise Integration Pack] (./app-service-logic-enterprise-integration-overview.md "Meer informatie over Enterprise Integration Pack").  
