<properties 
    pageTitle="Meer informatie over Enterprise Integration Pack X12 decoderen Message Connctor | Microsoft Azure App Service | Microsoft Azure" 
    description="Informatie over het gebruik van partners met de apps Enterprise Integration Pack en logica" 
    services="logic-apps" 
    documentationCenter=".net,nodejs,java"
    authors="padmavc" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="logic-apps" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="padmavc"/>

# <a name="get-started-with-decode-x12-message"></a>Aan de slag met het bericht decoderen X12

Partner-specifieke eigenschappen en EDI valideert, XML-document voor elke transactie genereert en bevestiging voor verwerkte transactie genereert.

## <a name="create-the-connection"></a>Maak de verbinding

### <a name="prerequisites"></a>Vereisten

* Een account Azure; u kunt een [gratis account](https://azure.microsoft.com/free) maken

* Een integratie-Account is vereist voor het decoderen X12 bericht connector gebruiken. Zie informatie over het maken van een [Account integratie](./app-service-logic-enterprise-integration-create-integration-account.md), [partners](./app-service-logic-enterprise-integration-partners.md) en [X12 overeenkomst](./app-service-logic-enterprise-integration-x12.md)

### <a name="connect-to-decode-x12-message-using-the-following-steps"></a>Verbinding maken met een bericht van X12 decoderen met behulp van de volgende stappen uit:

1. Wordt een voorbeeld van [een App logica maken](./app-service-logic-create-a-logic-app.md)

2. Deze connector hoeft niet alle triggers. Andere triggers gebruiken de App logica, zoals bijvoorbeeld een trigger aanvraag starten.  In de ontwerpfunctie voor logica App een trigger en voeg een actie.  Selecteer Microsoft beheerde API's in de vervolgkeuzelijst weergeven en voer vervolgens de 'x12' weergeven in het zoekvak.  Selecteer X12 – X12 decoderen bericht

    ![x12 zoeken](./media/app-service-logic-enterprise-integration-x12connector/x12decodeimage1.png)  

3. Als u eerder geen verbindingen met integratie-Account hebt gemaakt, moet u voor de details van de verbinding

    ![integratie-account verbinding](./media/app-service-logic-enterprise-integration-x12connector/x12decodeimage4.png)    

4. Geef de details van de integratie.  Eigenschappen met een sterretje zijn verplicht

  	| Eigenschap | Details |
  	| -------- | ------- |
  	| Naam van de verbinding * | Voer een naam voor de verbinding |
  	| Integratie-Account * | Voer de naam van de integratie. Zorg ervoor dat uw Account integratie en logica app in dezelfde locatie Azure |

    Eenmaal voltooid, lijkt de verbindingsdetails van uw op de volgende
    
    ![integratie-account verbinding gemaakt](./media/app-service-logic-enterprise-integration-x12connector/x12decodeimage5.png) 

5. Selecteer **maken**
    
6. U ziet dat de verbinding is gemaakt.

    ![integratie-account Verbindingsdetails](./media/app-service-logic-enterprise-integration-x12connector/x12decodeimage6.png) 

7. Selecteer X12 LCD-bericht te decoderen

    ![verplichte velden](./media/app-service-logic-enterprise-integration-x12connector/x12decodeimage7.png) 

## <a name="x12-decode-does-following"></a>X12 decoderen wordt na

* Valideert de envelop tegen trading partnerovereenkomst
* Genereert een XML-document voor elke transactie.
* Valideert EDI en partner-specifieke eigenschappen
    * EDI structurele validatie en uitgebreide schemavalidatie
    * Validatie van de structuur van de envelop interchange.
    * De schemavalidatie van de envelop ten opzichte van het schema van het besturingselement.
    * De schemavalidatie van de gegevenselementen van de transactie set ten opzichte van het schema weergegeven.
    * EDI validatie uitgevoerd op gegevens van transactie elementen 
* Controleert of de controlenummers interchange, groep en transactie set zijn geen dubbele records
    * Het controlenummer interchange ten opzichte van eerder ontvangen uitgewisseld wordt gecontroleerd.
    * Het nummer van de controle ten opzichte van andere nummers in het knooppunt controle wordt gecontroleerd.
    * Controleert dat de transactie stelt u besturingselement ten opzichte van andere nummers transactie set besturingselement in de groep.
* De hele uitwisseling converteert naar XML 
    * Interchange splitsen als sets van transactie - transactie sets onderbreken bij fout: elke transactie die is ingesteld in een knooppunt in een apart XML-document wordt geparseerd. Als een of meer transacties wordt ingesteld in de uitwisseling mislukt de validatie, X12 decoderen wordt alleen deze transactie wordt uitgesteld.
    * Interchange splitsen als sets van transactie - uitwisseling onderbreken bij fout: elke transactie die is ingesteld in een knooppunt in een apart XML-document wordt geparseerd.  Als een of meer transacties wordt ingesteld in de uitwisseling mislukt de validatie, X12 decoderen de volledige uitwisseling wordt uitgesteld.
    * Behouden Interchange - transactie sets onderbreken bij fout: Hiermee maakt u een XML-document voor de hele batch uitwisseling. X12 decoderen die transactie-sets die validatie niet wordt uitgesteld, terwijl alle andere transacties verwerken ingesteld
    * Behouden Interchange - uitwisseling onderbreken bij fout: Hiermee maakt u een XML-document voor de hele batch uitwisseling. Als een of meer transacties wordt ingesteld in de uitwisseling mislukt de validatie, X12 decoderen onderbreekt de hele uitwisseling 
* Genereert een bevestiging van de technische en/of functionele (indien geconfigureerd).
    * Een technische bevestiging gegenereerd als gevolg van de validatie van de kop. De technische bevestiging meldt de status van de verwerking van een interchange-header en trailer door de ontvanger adres.
    * Een functionele bevestiging gegenereerd als gevolg van de validatie van de instantie. De functionele bevestiging meldt een fout opgetreden tijdens het verwerken van ontvangen document

## <a name="next-steps"></a>Volgende stappen

[Meer informatie over de Enterprise Integration Pack] (./app-service-logic-enterprise-integration-overview.md "Meer informatie over Enterprise Integration Pack") 


