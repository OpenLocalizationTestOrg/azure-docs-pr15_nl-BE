<properties 
    pageTitle="Meer informatie over Enterprise Integration Pack decoderen EDIFACT bericht Connector | Microsoft Azure App Service | Microsoft Azure" 
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

# <a name="get-started-with-decode-edifact-message"></a>Aan de slag met EDIFACT bericht decoderen

Partner-specifieke eigenschappen en EDI valideert, XML-document voor elke transactie genereert en bevestiging voor verwerkte transactie genereert.

## <a name="create-the-connection"></a>Maak de verbinding

### <a name="prerequisites"></a>Vereisten

* Een account Azure; u kunt een [gratis account](https://azure.microsoft.com/free) maken

* Een integratie-Account is vereist voor het decoderen EDIFACT bericht connector gebruiken. Zie meer informatie over het maken van een [Account integratie](./app-service-logic-enterprise-integration-create-integration-account.md), [partners](./app-service-logic-enterprise-integration-partners.md) en [EDIFACT overeenkomst](./app-service-logic-enterprise-integration-edifact.md)

### <a name="connect-to-decode-edifact-message-using-the-following-steps"></a>Verbinding maken met een EDIFACT bericht decoderen met behulp van de volgende stappen uit:

1. [Maken van een App logica](./app-service-logic-create-a-logic-app.md) wordt een voorbeeld gegeven.

2. Deze connector hoeft niet alle triggers. Andere triggers gebruiken de App logica, zoals bijvoorbeeld een trigger aanvraag starten.  In de ontwerpfunctie voor logica App een trigger en voeg een actie.  Selecteer Microsoft beheerde API's in de vervolgkeuzelijst weergeven en voer vervolgens 'EDIFACT' in het zoekvak weergeven.  Selecteer decoderen EDIFACT bericht

    ![EDIFACT zoeken](./media/app-service-logic-enterprise-integration-edifactorconnector/edifactdecodeimage1.png)
    
3. Als u eerder geen verbindingen met integratie-Account hebt gemaakt, moet u voor de details van de verbinding

    ![integratie-account maken](./media/app-service-logic-enterprise-integration-edifactorconnector/edifactdecodeimage2.png)  

4. Geef de details van de integratie.  Eigenschappen met een sterretje zijn verplicht

  	| Eigenschap | Details |
  	| -------- | ------- |
  	| Naam van de verbinding * | Voer een naam voor de verbinding |
  	| Integratie-Account * | Voer de naam van de integratie. Zorg ervoor dat uw Account integratie en logica app in dezelfde locatie Azure |

    Eenmaal voltooid, lijkt de verbindingsdetails van uw op de volgende

    ![integratie-account gemaakt](./media/app-service-logic-enterprise-integration-edifactorconnector/edifactdecodeimage3.png)  

5. Selecteer **maken**

6. U ziet dat de verbinding is gemaakt.

    ![integratie-account Verbindingsdetails](./media/app-service-logic-enterprise-integration-edifactorconnector/edifactdecodeimage5.png)  

7. Selecteer EDIFACT plat bestand bericht decoderen

    ![verplichte velden](./media/app-service-logic-enterprise-integration-edifactorconnector/edifactdecodeimage5.png)  

## <a name="edifact-decode-does-following"></a>EDIFACT decoderen wordt na

* De overeenkomst door de kwalificatie van de afzender-id en kwalificatie van de ontvanger & ID oplossen
* Meerdere uitgewisseld in één bericht in afzonderlijke gesplitst.
* Valideert de envelop tegen trading partnerovereenkomst
* De uitwisseling ontleed.
* Valideert EDI en partner-specifieke eigenschappen bevat
    * Validatie van de structuur van de envelop interchange.
    * De schemavalidatie van de envelop ten opzichte van het schema van het besturingselement.
    * De schemavalidatie van de gegevenselementen van de transactie set ten opzichte van het schema weergegeven.
    * EDI validatie uitgevoerd op gegevens van transactie elementen
* Controleert of de controlenummers interchange, groep en transactie set zijn geen dubbele records (indien geconfigureerd) 
    * Het controlenummer interchange ten opzichte van eerder ontvangen uitgewisseld wordt gecontroleerd. 
    * Het nummer van de controle ten opzichte van andere nummers in het knooppunt controle wordt gecontroleerd. 
    * Controleert dat de transactie stelt u besturingselement ten opzichte van andere nummers transactie set besturingselement in de groep.
* Genereert een XML-document voor elke transactie.
* De hele uitwisseling converteert naar XML 
    * Interchange splitsen als sets van transactie - transactie sets onderbreken bij fout: elke transactie die is ingesteld in een knooppunt in een apart XML-document wordt geparseerd. Als een of meer transacties wordt ingesteld in de uitwisseling validatie is mislukt, en vervolgens wordt alleen die transactie sets EDIFACT decoderen uitgesteld. 
    * Interchange splitsen als sets van transactie - uitwisseling onderbreken bij fout: elke transactie die is ingesteld in een knooppunt in een apart XML-document wordt geparseerd.  Als een of meer transacties wordt ingesteld in de uitwisseling validatie is mislukt, en vervolgens decoderen EDIFACT de hele uitwisseling onderbreekt.
    * Behouden Interchange - transactie sets onderbreken bij fout: Hiermee maakt u een XML-document voor de hele batch uitwisseling. EDIFACT decoderen opschort die transactie-sets die validatie terwijl u doorgaat met het verwerken van alle andere sets transactie niet
    * Behouden Interchange - uitwisseling onderbreken bij fout: Hiermee maakt u een XML-document voor de hele batch uitwisseling. Als een of meer transacties wordt ingesteld in de uitwisseling validatie is mislukt, en vervolgens decoderen EDIFACT de hele uitwisseling onderbreekt 
* Genereert technische (beheer) en/of functionele acknowledgment (indien geconfigureerd).
    * Een technische bevestiging of de ACK CONTRL rapporteert de resultaten van een syntactische controle van de volledige uitwisseling ontvangen.
    * Een functionele bevestiging erkent accepteren of weigeren van een ontvangen interchange of een groep

## <a name="next-steps"></a>Volgende stappen

[Meer informatie over de Enterprise Integration Pack] (./app-service-logic-enterprise-integration-overview.md "Meer informatie over Enterprise Integration Pack") 