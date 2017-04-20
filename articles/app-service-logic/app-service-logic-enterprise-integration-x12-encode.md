<properties 
    pageTitle="Meer informatie over Enterprise Integration Pack coderen X12 bericht Connctor | Microsoft Azure App Service | Microsoft Azure" 
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

# <a name="get-started-with-encode-x12-message"></a>Aan de slag met bericht coderen X12

EDI en partner-specifieke eigenschappen worden gevalideerd, XML-gecodeerde berichten converteert naar EDI transactie sets in de uitwisseling en vraagt om een bevestiging van de technische en/of functionele

## <a name="create-the-connection"></a>Maak de verbinding

### <a name="prerequisites"></a>Vereisten

* Een account Azure; u kunt een [gratis account](https://azure.microsoft.com/free) maken

* Een integratie-Account is vereist voor het coderen x12 bericht connector gebruiken. Zie informatie over het maken van een [Account integratie](./app-service-logic-enterprise-integration-create-integration-account.md), [partners](./app-service-logic-enterprise-integration-partners.md) en [X12 overeenkomst](./app-service-logic-enterprise-integration-x12.md)

### <a name="connect-to-encode-x12-message-using-the-following-steps"></a>Verbinding maken met een bericht van X12 coderen met behulp van de volgende stappen uit:

1. Wordt een voorbeeld van [een App logica maken](./app-service-logic-create-a-logic-app.md)

2. Deze connector hoeft niet alle triggers. Andere triggers gebruiken de App logica, zoals bijvoorbeeld een trigger aanvraag starten.  In de ontwerpfunctie voor logica App een trigger en voeg een actie.  Selecteer Microsoft beheerde API's in de vervolgkeuzelijst weergeven en voer vervolgens de 'x12' weergeven in het zoekvak.  Selecteer beide X12-X12 gecodeerd bericht met de Overeenkomstnaam of X12-X 12 bericht coderen met identiteiten.  

    ![x12 zoeken](./media/app-service-logic-enterprise-integration-x12connector/x12decodeimage1.png) 

3. Als u eerder geen verbindingen met integratie-Account hebt gemaakt, moet u voor de details van de verbinding

    ![integratie-account verbinding](./media/app-service-logic-enterprise-integration-x12connector/x12encodeimage1.png) 


4. Geef de details van de integratie.  Eigenschappen met een sterretje zijn verplicht

  	| Eigenschap | Details |
  	| -------- | ------- |
  	| Naam van de verbinding * | Voer een naam voor de verbinding |
  	| Integratie-Account * | Voer de naam van de integratie. Zorg ervoor dat uw Account integratie en logica app in dezelfde locatie Azure |

    Eenmaal voltooid, lijkt de verbindingsdetails van uw op de volgende

    ![integratie-account verbinding gemaakt](./media/app-service-logic-enterprise-integration-x12connector/x12encodeimage2.png) 


5. Selecteer **maken**

6. U ziet dat de verbinding is gemaakt.

    ![integratie-account Verbindingsdetails](./media/app-service-logic-enterprise-integration-x12connector/x12encodeimage3.png) 

#### <a name="x12---encode-x12-message-by-agreement-name"></a>X12-X12 gecodeerd bericht met de Overeenkomstnaam

7. Selecteer X12 overeenkomst uit de vervolgkeuzelijst en XML-berichten te coderen.

    ![verplichte velden](./media/app-service-logic-enterprise-integration-x12connector/x12encodeimage4.png) 

#### <a name="x12---encode-x12-message-by-identities"></a>X12-X12 gecodeerd bericht door identiteiten

7.  Afzender-id, kwalificatie van de afzender, ontvanger id en kwalificatie van de ontvanger opgeven, zoals ingesteld in de X12 overeenkomst.  Selecteer XML-bericht coderen

    ![verplichte velden](./media/app-service-logic-enterprise-integration-x12connector/x12encodeimage5.png) 

## <a name="x12-encode-does-following"></a>X12 coderen wordt na:

* Resolutie van de overeenkomst op overeenkomsten tussen de contexteigenschappen van zender en ontvanger.
* Serialiseert de uitwisseling van EDI, XML-gecodeerde berichten converteren naar EDI transactie sets in de uitwisseling.
* Het set-header en trailer segmenten transactie van toepassing is
* Genereert een controlenummer interchange, het nummer van een besturingselement en een transactie set controlenummer voor elke uitgaande uitwisseling
* Vervangt de scheidingstekens in de nettolading van gegevens
* Valideert EDI en partner-specifieke eigenschappen
    * De schemavalidatie van de gegevenselementen van de transactie set tegen het bericht Schema
    * EDI gevalideerd transactie set elementen.
    * Uitgebreide validatie uitgevoerd op gegevens van transactie elementen
* Vraagt om een bevestiging van de technische en/of functionele (indien geconfigureerd).
    * Een technische bevestiging gegenereerd als gevolg van de validatie van de kop. De technische bevestiging meldt de status van de verwerking van een interchange-header en trailer door de ontvanger adres
    * Een functionele bevestiging gegenereerd als gevolg van de validatie van de instantie. De functionele bevestiging meldt een fout opgetreden tijdens het verwerken van ontvangen document

## <a name="next-steps"></a>Volgende stappen

[Meer informatie over de Enterprise Integration Pack] (./app-service-logic-enterprise-integration-overview.md "Meer informatie over Enterprise Integration Pack") 

