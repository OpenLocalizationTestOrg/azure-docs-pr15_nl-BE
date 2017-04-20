<properties
   pageTitle="EAI logica App met VETR in apps logica in Azure App Service maken | Microsoft Azure"
   description="Valideren, coderen en functies van services BizTalk XML transformeren"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="rajeshramabathiran"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="04/20/2016"
   ms.author="rajram"/>


# <a name="create-eai-logic-app-using-vetr"></a>EAI logica App met VETR maken

[AZURE.INCLUDE [app-service-logic-version-message](../../includes/app-service-logic-version-message.md)]

De meeste scenario's voor Enterprise Application Integration (EAI) bemiddelen tussen een bron en een bestemming. Dergelijke scenario's hebben vaak een gemeenschappelijke set van eisen:

- Zorg ervoor dat de gegevens uit verschillende systemen correct zijn opgemaakt.
- 'Zoekopties' uitvoeren op binnenkomende gegevens om beslissingen te nemen.
- Gegevens van de ene indeling naar de andere converteren. Gegevens van gegevensindeling voor een CRM-systeem bijvoorbeeld converteren naar de indeling van een ERP-systeem.
- De gegevens van de route naar de gewenste toepassing of het systeem.

In dit artikel ziet u een algemene integratie patroon: "bemiddeling in één richting bericht" of VETR (valideren toegang bieden, transformatie, Route). Het patroon VETR bemiddelt tussen een bronentiteit en een bestemming entiteit. Meestal zijn de bron- en doeladressen gegevensbronnen.

U kunt een website die orders worden geaccepteerd. Gebruikers boeken orders op het systeem met behulp van HTTP. Achter de schermen wordt het systeem valideert de binnenkomende gegevens op juistheid Normaliseert het en blijft deze bestaan in een wachtrij Service Bus voor verdere verwerking. Het systeem neemt orders uit de wachtrij, in een bepaalde indeling wordt verwacht. De end-to-end-stroom is dus:

**HTTP** → **valideren** → **Transformeren** → **Service Bus**

![Basic VETR stroom][1]

De volgende toepassingen voor BizTalk-API bij het opstellen van dit patroon:

* **HTTP Trigger** - bron op een berichtgebeurtenis trigger
* **Validate** - controleert de juistheid van de binnenkomende gegevens
* **Transformatie** - transformaties gegevens van binnenkomende format-indeling vereist voor downstream-systeem
* **Service Bus Connector** - doelentiteit waarbij gegevens worden verzonden


## <a name="constructing-the-basic-vetr-pattern"></a>Het basispatroon van VETR maken
### <a name="the-basics"></a>De grondbeginselen

In het portal voor Azure selecteren **+ Nieuw**en selecteer **Web + Mobile** **App logica**selecteert. Kies een naam, locatie, abonnement, resourcegroep en locatie die werkt. Resourcegroepen fungeren als containers voor uw toepassingen; alle bronnen voor uw app gaat u naar dezelfde resourcegroep.

Vervolgens voegen we triggers en acties.


## <a name="add-http-trigger"></a>HTTP-Trigger toevoegen
1. Selecteer in de **Sjablonen van Logic App** **maken helemaal**.
1. Selecteer de **Http-Listener** uit de galerie voor het maken van een nieuwe listener. Noem het **HTTP1**.
2. Stel de **automatisch antwoord verzenden?** instellen op false. Configureer de triggeractiviteit wordt uitgevoerd door de _HTTP-methode_ _POST_ en _Relatieve URL_ aan _/OneWayPipeline_:  
    ![HTTP-Trigger][2]
3. Selecteer het groene vinkje voor het voltooien van de trigger.

## <a name="add-validate-action"></a>Toevoegen valideren, actie

Nu we invoeren die worden uitgevoerd wanneer de trigger wordt gestart, dat wil zeggen, wanneer een oproep wordt ontvangen op het eindpunt van de HTTP.

1. **Validatie van BizTalk-XML** toevoegen uit de galerie en noem deze _(Validate1)_ om een exemplaar te maken.
2. Een XSD-schema voor het valideren van de inkomende XML-berichten configureren. Selecteer de actie _valideren_ en selecteer _triggers('httplistener').outputs. Inhoud_ als de waarde voor de parameter _inputXml_ .

Nu is de actie valideren de eerste actie na de HTTP-listener: 

![Validatie van BizTalk-XML][3]

Laten we toevoegen op dezelfde manier de rest van de acties. 

## <a name="add-transform-action"></a>Transformatie actie toevoegen
Laten we de transformaties voor het normaliseren van de binnenkomende gegevens configureren.

1. **BizTalk-transformatie Service** toevoegen uit de galerie.
2. Een transformatie wilt transformeren de inkomende XML-berichten configureren, selecteert u de actie **Transformeren** als de actie uit te voeren wanneer deze API wordt aangeroepen. Selecteer ```triggers(‘httplistener’).outputs.Content``` als de waarde voor _inputXml_. *Kaart* is een optionele parameter, aangezien de inkomende gegevens zijn gekoppeld aan alle geconfigureerde transformaties en die voldoen aan het schema worden toegepast.
3. Ten slotte, de transformatie wordt alleen uitgevoerd als valideren is gelukt. Configureren van deze voorwaarde, selecteer het pictogram vistuig rechts bovenin en selecteer _toevoegen een voorwaarde wordt voldaan_. Stel in op ```equals(actions('xmlvalidator').status,'Succeeded')```:  

![BizTalk-transformaties][4]


## <a name="add-service-bus-connector"></a>Service busconnector toevoegen
Vervolgens voegen we de bestemming, een wachtrij Service Bus — gegevens voor te schrijven.

1. Een **Service Bus Connector** toevoegen uit de galerie. De **naam** ingesteld op _Servicebus1_, **Verbindingsreeks** ingesteld op de verbindingstekenreeks aan uw exemplaar van de bus service en **Naam van entiteit** instellen voor _wachtrij_ **naam abonnement**overslaan.
2. Selecteer de actie **Bericht verzenden** en stel het veld **inhoud** voor de actie op _actions('transformservice').outputs. OutputXml_.
3. Het veld **Inhoudstype** ingesteld op *application/xml*:  

![Bus service][5]


## <a name="send-http-response"></a>HTTP-antwoord verzenden
Als pijpleiding verwerking is voltooid, weer een HTTP-antwoord verzenden voor zowel geslaagde en mislukte met de volgende stappen uit:

1. Een **Http-Listener** toevoegen uit de galerie en selecteert u de actie **HTTP-antwoord verzenden** .
2. **Antwoord-ID** voor het verzenden van *bericht*instellen.
2. **Response Content** *pijpleiding*verwerking voltooid ingesteld.
3. **Statuscode van antwoord** op *200* HTTP 200 OK aangeven.
4. Selecteer het vervolgkeuzemenu rechts bovenin en selecteer **toevoegen een voorwaarde wordt voldaan**.  Stel op de volgende expressie:  
    ```@equals(actions('azureservicebusconnector').status,'Succeeded')```  <br/>
5. Herhaal deze stappen voor het verzenden van een HTTP-antwoord ook mislukt. **Voorwaarde** wijzigen op de volgende expressie:  
```@not(equals(actions('azureservicebusconnector').status,'Succeeded'))``` <br/>
6. Kies **OK** **maken**.



## <a name="completion"></a>Voltooiing
Elke keer dat iemand een bericht naar het eindpunt van de HTTP-activeert de app en voert de acties die u zojuist hebt gemaakt. Voor het beheren van een dergelijke logica apps maken, selecteert u **Bladeren** in de Portal Azure en **Logica Apps**selecteren. Selecteer uw app voor meer informatie.

Handige onderwerpen:

[De API Apps en de verbindingen controleren en beheren](app-service-logic-monitor-your-connectors.md)  <br/>
[De logica Apps controleren](app-service-logic-monitor-your-logic-apps.md)

<!--image references -->
[1]: ./media/app-service-logic-create-EAI-logic-app-using-VETR/BasicVETR.PNG
[2]: ./media/app-service-logic-create-EAI-logic-app-using-VETR/HTTPListener.PNG
[3]: ./media/app-service-logic-create-EAI-logic-app-using-VETR/BizTalkXMLValidator.PNG
[4]: ./media/app-service-logic-create-EAI-logic-app-using-VETR/BizTalkTransforms.PNG
[5]: ./media/app-service-logic-create-EAI-logic-app-using-VETR/AzureServiceBus.PNG
