<properties 
    pageTitle="Meer informatie over Enterprise Integration Pack decoderen AS2 bericht Connctor | Microsoft Azure App Service | Microsoft Azure" 
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

# <a name="get-started-with-decode-as2-message"></a>Aan de slag met AS2 bericht decoderen

Verbinding maken met de AS2 bericht decoderen vast te stellen en de betrouwbaarheid tijdens het overbrengen van berichten. Het biedt digitale ondertekening decodering en bevestigingen via Message Disposition Notifications (MDN).

## <a name="create-the-connection"></a>Maak de verbinding

### <a name="prerequisites"></a>Vereisten

* Een account Azure; u kunt een [gratis account](https://azure.microsoft.com/free) maken

* Een integratie-Account is vereist voor het decoderen AS2 bericht connector gebruiken. Zie meer informatie over het maken van een [Account integratie](./app-service-logic-enterprise-integration-create-integration-account.md), [partners](./app-service-logic-enterprise-integration-partners.md) en een [AS2-overeenkomst](./app-service-logic-enterprise-integration-as2.md)

### <a name="connect-to-decode-as2-message-using-the-following-steps"></a>Verbinding maken met de AS2 bericht decoderen met behulp van de volgende stappen uit:

1. [Maken van een App logica](./app-service-logic-create-a-logic-app.md) wordt een voorbeeld gegeven.

2. Deze connector hoeft niet alle triggers. Andere triggers gebruiken de App logica, zoals bijvoorbeeld een trigger aanvraag starten.  In de ontwerpfunctie voor logica App een trigger en voeg een actie.  Selecteer Microsoft beheerde API's in de vervolgkeuzelijst weergeven en voer vervolgens 'AS2' in het zoekvak weergeven.  Selecteer AS2-AS2 bericht decoderen

    ![AS2 zoeken](./media/app-service-logic-enterprise-integration-AS2connector/as2decodeimage1.png)

3. Als u eerder geen verbindingen met integratie-Account hebt gemaakt, moet u voor de details van de verbinding

    ![Integratie-verbinding maken](./media/app-service-logic-enterprise-integration-AS2connector/as2decodeimage2.png)

4. Geef de details van de integratie.  Eigenschappen met een sterretje zijn verplicht

  	| Eigenschap   | Details |
  	| --------   | ------- |
  	| Naam van de verbinding *    | Voer een naam voor de verbinding |
  	| Integratie-Account * | Voer de naam van de integratie. Zorg ervoor dat uw Account integratie en logica app in dezelfde locatie Azure |

    Eenmaal voltooid, lijkt de verbindingsdetails van uw op de volgende

    ![integratie-verbinding](./media/app-service-logic-enterprise-integration-AS2connector/as2decodeimage3.png)

5. Selecteer **maken**
    
6. U ziet dat de verbinding is gemaakt.  Nu gaat u verder met de overige stappen in uw App logica

    ![integratie-verbinding gemaakt](./media/app-service-logic-enterprise-integration-AS2connector/as2decodeimage4.png) 

7. Selecteer hoofdtekst en koppen in aanvraag uitgangen

    ![verplichte velden](./media/app-service-logic-enterprise-integration-AS2connector/as2decodeimage5.png) 

## <a name="the-as2-decode-does-the-following"></a>De AS2 decoderen doet het volgende

* AS2/HTTP-kopteksten worden verwerkt
* De handtekening wordt gecontroleerd (indien geconfigureerd)
* De berichten worden gedecodeerd (indien geconfigureerd)
* Decomprimeert het bericht (indien geconfigureerd)
* Sluitend wordt gemaakt van een MDN ontvangen met het oorspronkelijke uitgaand bericht
* Updates en verbindt de records in de database niet afwijzen
* Records voor statusrapporten AS2 schrijft
* De inhoud van de nettolading uitvoer worden base64-gecodeerde
* Bepaalt of een MDN vereist is, en of de MDN synchrone moeten of asynchrone op basis van configuratie in AS2-overeenkomst
* Genereert een synchrone of asynchrone MDN (op basis van de overeenkomst configuraties)
* De eigenschappen en correlatietokens ingesteld op de MDN

##<a name="try-it-for-yourself"></a>Probeer het zelf

Waarom zou u niet eens proberen. Klik [hier](https://azure.microsoft.com/documentation/templates/201-logic-app-as2-send-receive/) voor een volledig operationeel logica app van uw eigen met behulp van de functies voor bedrijfslogica Apps AS2 implementeren 

## <a name="next-steps"></a>Volgende stappen

[Meer informatie over de Enterprise Integration Pack] (./app-service-logic-enterprise-integration-overview.md "Meer informatie over Enterprise Integration Pack") 