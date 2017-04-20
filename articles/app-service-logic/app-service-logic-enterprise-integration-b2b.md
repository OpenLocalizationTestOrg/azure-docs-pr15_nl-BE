<properties 
    pageTitle="B2B-oplossingen maken met Enterprise Integration Pack | Microsoft Azure App Service | Microsoft Azure" 
    description="Klik hier voor informatie over het ontvangen van gegevens via de B2B-functies van de Enterprise Integration Pack" 
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

# <a name="learn-about-receiving-data-using-the-b2b-features-of-the-enterprise-integration-pack"></a>Klik hier voor informatie over het ontvangen van gegevens via de B2B-functies van de Enterprise Integration Pack#

## <a name="overview"></a>Overzicht ##

Dit document maakt deel uit van de logica Apps Enterprise Integration Pack. Bekijk het overzicht voor meer informatie over de [mogelijkheden van de Enterprise Integration Pack](./app-service-logic-enterprise-integration-overview.md).

## <a name="prerequisites"></a>Vereisten ##

Gebruik de AS2 en X12 acties moet u een Account voor de integratie van de onderneming

[Het maken van een Account voor de integratie van de onderneming](./app-service-logic-enterprise-integration-accounts.md)

## <a name="how-to-use-the-logic-apps-b2b-connectors"></a>Het gebruik van de logica Apps B2B-connectors ##

Zodra u hebt gemaakt van een account integratie en partners toegevoegd en deze overeenkomsten u klaar bent voor het maken van een App Logic implementeert die een werkstroom bedrijven (B2B).

In deze walkthru ziet u hoe u de AS2 en X12 acties voor het maken van een business-business logica App dat gegevens van een handelspartner ontvangt.

1. Maak een nieuwe logica app en [koppel deze aan uw account integratie](./app-service-logic-enterprise-integration-accounts.md).  
2. Een trigger **aanvraag - als een HTTP-aanvraag wordt ontvangen** op uw app logica toevoegen  
![](./media/app-service-logic-enterprise-integration-b2b/flatfile-1.png)  
3. Voeg de actie **AS2 decoderen** door eerst te selecteren **een actie toevoegen**  
![](./media/app-service-logic-enterprise-integration-b2b/transform-2.png)  
4. Het woord **as2** invoeren in het zoekvak om alle filteracties aan die u wilt gebruiken  
![](./media/app-service-logic-enterprise-integration-b2b/b2b-5.png)  
6. Selecteer de **AS2 - bericht decoderen AS2** -actie  
![](./media/app-service-logic-enterprise-integration-b2b/b2b-6.png)  
7. Zoals u ziet, de **instantie** die u zullen toevoegen als invoer. Selecteer in dit voorbeeld wordt het hoofdgedeelte van de HTTP-aanvraag die de logica app geactiveerd. U kunt ook een expressie voor het invoeren van de koppen in het veld**kop** invoeren:

    @triggerOutputs()['headers']

8. De **kopteksten** die vereist zijn voor AS2 toevoegen. Deze worden in de HTTP-aanvraagheaders. In dit voorbeeld selecteert u de koppen van de HTTP-aanvraag die de logica app geactiveerd.
9. Het bericht decoderen X12 actie nu toevoegen door het **toevoegen van een actie** opnieuw selecteren  
![](./media/app-service-logic-enterprise-integration-b2b/b2b-9.png)   
10. Het woord **x12** invoeren in het zoekvak om alle filteracties aan die u wilt gebruiken  
![](./media/app-service-logic-enterprise-integration-b2b/b2b-10.png)  
11. Selecteer de **X12-X12 decoderen bericht** actie toe te voegen aan de toepassing van de logica  
![](./media/app-service-logic-enterprise-integration-b2b/b2b-as2message.png)  
12. Nu moet u de invoer voor deze actie worden de output van de bovenstaande AS2-actie opgeven. De inhoud van het bericht is in een JSON-object en base64-gecodeerd is. Daarom moet u een expressie opgeeft als de invoer dus geeft u de volgende expressie in het invoerveld **X12 PLATTE bestand bericht te decoderen**  

    @base64ToString(body('Decode_AS2_message')?['AS2Message']?['Content'])  

13. Deze stap wordt de X12 gegevens ontvangen van de handelspartner en wordt een aantal items in een object JSON-uitvoer te decoderen. Zodat door de partner weet van de ontvangst van de gegevens kunt u sturen weer een antwoord met de AS2 Message Disposition kennisgeving (MDN) in een HTTP-reactie-actie  
14. De actie **reactie** toevoegen door het **toevoegen van een actie** selecteren   
![](./media/app-service-logic-enterprise-integration-b2b/b2b-14.png)  
15. Het woord **antwoord** invoeren in het zoekvak om alle filteracties aan die u wilt gebruiken  
![](./media/app-service-logic-enterprise-integration-b2b/b2b-15.png)  
16. Selecteer de actie **reactie** toe te voegen  
![](./media/app-service-logic-enterprise-integration-b2b/b2b-16.png)  
17. Het antwoordveld **BODY** instellen met behulp van de volgende expressie toegang krijgen tot de MDN uit de resultaten van de actie **bericht decoderen X12**  

    @base64ToString(body('Decode_AS2_message')?['OutgoingMdn']?['Content'])  

![](./media/app-service-logic-enterprise-integration-b2b/b2b-17.png)  
18. Uw werk opslaan  
![](./media/app-service-logic-enterprise-integration-b2b/transform-5.png)  

Nu bent u klaar met het instellen van uw app B2B-logica. In een echte wereld-toepassing, kunt u de gedecodeerde X12 opgeslagen gegevens in een LOB-toepassingen of gegevens winkel. U kunt gemakkelijk verdere acties om dit of Schrijf aangepaste API's verbinding maken met uw eigen LOB-toepassingen en het gebruik van deze API's in uw app logica toevoegen.

## <a name="features-and-use-cases"></a>Functies en use-cases ##

- De AS2 en X12 decoderen en coderen van acties kunt u gegevens ontvangen van en verzenden van gegevens naar de handelspartners via standaardprotocollen met logica apps  
- U kunt AS2 en X12 met of zonder elkaar gegevens uitwisselen met handelspartners zoals vereist
- De B2B-acties kunnen u eenvoudig maken van partners en overeenkomsten in de integratie en verbruiken een app logica  
- Door uw logica app met andere acties uit te breiden kunt verzenden en ontvangen van gegevens van en naar andere toepassingen en services, zoals televergaderingen  

## <a name="learn-more"></a>Meer informatie ##

[Meer informatie over de Enterprise Integration Pack](./app-service-logic-enterprise-integration-overview.md)  