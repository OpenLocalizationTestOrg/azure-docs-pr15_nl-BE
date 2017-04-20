> [AZURE.SELECTOR]
- [Linux](../articles/iot-hub/iot-hub-linux-gateway-sdk-get-started.md)
- [Windows](../articles/iot-hub/iot-hub-windows-gateway-sdk-get-started.md)

In dit artikel wordt een gedetailleerd overzicht van de [voorbeeldcode Hello World] [ lnk-helloworld-sample] ter illustratie van de fundamentele onderdelen van de [SDK van Azure IoT Gateway] [ lnk-gateway-sdk] architectuur. Het monster wordt de IoT Hub Gateway SDK voor het maken van een eenvoudige gateway die zich elke vijf seconden een "hello world"-bericht naar een bestand.

Dit scenario omvat:

- **Concepten**: een overzicht van de onderdelen waaruit een gateway die u met de SDK IoT Gateway maken.  
- **Hello World voorbeeldarchitectuur**: hierin wordt beschreven hoe de concepten van toepassing op het voorbeeld HelloWorld en hoe de onderdelen in elkaar passen.
- **Het bouwen van de steekproef**: de vereiste stappen voor het bouwen van het monster.
- **Het uitvoeren van de steekproef**: de vereiste stappen voor het uitvoeren van de steekproef. 
- **Normaal**: een voorbeeld van de uitvoer kunt verwachten als u het voorbeeld uitvoert.
- **Codefragmenten**: een collectie van codefragmenten weergeven hoe het voorbeeld HelloWorld implementeert belangrijke gateway-componenten.

## <a name="azure-iot-gateway-sdk-concepts"></a>Azure IoT Gateway SDK concepten

Voordat u de code van de steekproef te onderzoeken of uw eigen veld gateway met behulp van de SDK IoT-Gateway maken, moet u de basisprincipes die de architectuur van de SDK onderbouwing weten.

### <a name="modules"></a>Modules

U maken een gateway met de SDK Azure IoT-Gateway door te maken en het assembleren van *modules*. Modules *berichten* gebruiken voor het uitwisselen van gegevens met elkaar. Een module ontvangt een bericht, een handeling uitvoert op deze zet het eventueel in een nieuw bericht en vervolgens wordt gepubliceerd voor andere modules verwerken. Sommige modules kunnen alleen nieuwe berichten te produceren en nooit inkomende berichten verwerken. Een reeks modules maakt een pijpleiding gegevensverwerking met elke module een transformatie uitvoeren op de gegevens op één punt in deze pijpleiding.

![Een keten van modules in gateway gebouwd met de SDK Azure IoT Gateway][1]
 
De SDK bevat de volgende opties:

- Vooraf schriftelijke modules die common gateway functies uitvoeren.
- De interfaces een ontwikkelaar kunt gebruiken om aangepaste modules worden geschreven.
- De benodigde infrastructuur voor het implementeren en uitvoeren van een set modules.

De SDK bevat een abstractie laag waarmee u gateways worden uitgevoerd op verschillende besturingssystemen en platforms te bouwen.

![Azure IoT Gateway SDK abstraction layer][2]

### <a name="messages"></a>Berichten

Hoewel na te denken over het doorgeven van berichten aan elkaar modules een handige manier is om het concept van een gateway-functie, komt het niet nauwkeurig overeen wat er gebeurt. Modules met elkaar communiceren via een makelaar, ze berichten publiceren naar de broker (bus, pubsub of andere messaging patroon) en laat de broker die het bericht doorsturen naar de erop aangesloten modules.

Een modules wordt de functie **Broker_Publish** publiceert een bericht met de makelaar. De broker biedt berichten naar een module door een callback-functie aanroepen. Een bericht bestaat uit een set van sleutel/waarde-eigenschappen en de inhoud als een geheugenblok wordt doorgegeven.

![De rol van de makelaar in de SDK van Azure IoT Gateway][3]

### <a name="message-routing-and-filtering"></a>Berichtroutering en filteren

Er zijn twee manieren om het routeren van berichten naar de juiste modules. Een reeks koppelingen kan worden doorgegeven aan de makelaar, zodat de makelaar de bron- en sink voor elke module kent of de module op de eigenschappen van het bericht filteren kunt. Een module mag alleen reageren op een bericht als het bericht bestemd is voor het. De koppelingen en het filteren van berichten is wat daadwerkelijk wordt gemaakt van een pijplijn bericht.

## <a name="hello-world-sample-architecture"></a>Hello World voorbeeldarchitectuur

Het Hello World-voorbeeld ziet u de concepten zoals beschreven in de vorige sectie. In het voorbeeld HelloWorld implementeert een gateway een pijplijn bestaat is uit twee modules:

-   Het *hello world* -module elke vijf seconden een bericht wordt gemaakt en wordt doorgegeven aan de module logger.
-   De module *logger* schrijft de berichten die zijn ontvangen in een bestand.

![Architectuur van Hello World voorbeeld is gemaakt met de SDK Azure IoT Gateway][4]

Zoals beschreven in de vorige sectie, doorgeeft de Hello World-module niet berichten rechtstreeks naar de module logger elke vijf seconden. In plaats daarvan publiceert het een bericht met de makelaar elke vijf seconden.

De module logger ontvangt het bericht van de broker en fungeert, bij het schrijven van de inhoud van het bericht naar een bestand.

De module logger verbruikt alleen berichten van de makelaar, nooit worden nieuwe berichten gepubliceerd met de makelaar.

![Hoe de broker routeert berichten tussen de modules in de SDK van Azure IoT Gateway][5]

In de bovenstaande afbeelding wordt de architectuur van het voorbeeld HelloWorld en de relatieve paden naar de bronbestanden die verschillende delen van het monster te in de [bibliotheek implementeren][lnk-gateway-sdk]. Verken de code op uw eigen of gebruik de codefragmenten hieronder als leidraad.

<!-- Images -->
[1]: media/iot-hub-gateway-sdk-getstarted-selector/modules.png
[2]: media/iot-hub-gateway-sdk-getstarted-selector/modules_2.png
[3]: media/iot-hub-gateway-sdk-getstarted-selector/messages_1.png
[4]: media/iot-hub-gateway-sdk-getstarted-selector/high_level_architecture.png
[5]: media/iot-hub-gateway-sdk-getstarted-selector/detailed_architecture.png

<!-- Links -->
[lnk-helloworld-sample]: https://github.com/Azure/azure-iot-gateway-sdk/tree/master/samples/hello_world
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk