<properties 
    pageTitle="Het gebruik van de Service Bus wachtrijen in Node.js | Microsoft Azure" 
    description="Informatie over het Service Bus wachtrijen in Azure vanuit een app Node.js gebruikt." 
    services="service-bus" 
    documentationCenter="nodejs" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="10/03/2016" 
    ms.author="sethm"/>

# <a name="how-to-use-service-bus-queues"></a>Het gebruik van wachtrijen Service Bus

[AZURE.INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

Dit artikel wordt beschreven hoe u wachtrijen Service Bus in Node.js. De monsters zijn geschreven in JavaScript en gebruikt de module Node.js Azure. De scenario's die zijn **het maken van wachtrijen** **verzenden en ontvangen van berichten**en **wachtrijen verwijderen**. Zie de sectie [volgende stappen](#next-steps) voor meer informatie over wachtrijen.

[AZURE.INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

## <a name="create-a-nodejs-application"></a>Een Node.js-toepassing maken

Een lege Node.js-toepassing maken. Zie voor instructies voor het maken van een toepassing Node.js, [maken en implementeren van een toepassing een Website Azure Node.js][], of [Node.js Cloud Service][] met Windows PowerShell.

## <a name="configure-your-application-to-use-service-bus"></a>Uw toepassing configureren voor gebruik van de Service Bus

Azure Service Bus, downloaden en gebruiken van het pakket Node.js Azure. Dit pakket bevat een reeks bibliotheken die met de REST van Service Bus services communiceren.

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a>Knooppunt Package Manager (NPM) gebruiken voor het verkrijgen van het pakket

1. Het venster **Windows PowerShell voor Node.js** gebruiken om te navigeren naar de **c:\\knooppunt\\sbqueues\\WebRole1** map waarin u de voorbeeldtoepassing hebt gemaakt.

2. Typ **npm azure installeren** in het opdrachtvenster in de volgende uitvoer resulteren moet:

    ```
    azure@0.7.5 node_modules\azure
        ├── dateformat@1.0.2-1.2.3
        ├── xmlbuilder@0.4.2
        ├── node-uuid@1.2.0
        ├── mime@1.2.9
        ├── underscore@1.4.4
        ├── validator@1.1.1
        ├── tunnel@0.0.2
        ├── wns@0.5.3
        ├── xml2js@0.2.7 (sax@0.5.2)
        └── request@2.21.0 (json-stringify-safe@4.0.0, forever-agent@0.5.0, aws-sign@0.3.0, tunnel-agent@0.3.0, oauth-sign@0.3.0, qs@0.6.5, cookie-jar@0.3.0, node-uuid@1.4.0, http-signature@0.9.11, form-data@0.0.8, hawk@0.13.1)
    ```

3. U kunt handmatig uitvoeren met de opdracht **ls** te controleren dat een **knooppunt\_modules** map is gemaakt. In deze map vindt u de **azure** -pakket de bibliotheken die u nodig hebt bevat voor toegang tot de Service Bus wachtrijen.

### <a name="import-the-module"></a>Importeren van de module

Met Kladblok of een andere teksteditor, Voeg het volgende toe boven aan het bestand **server.js** van de toepassing:

```
var azure = require('azure');
```

### <a name="set-up-an-azure-service-bus-connection"></a>Een Azure Service Bus verbinding instellen

De module Azure leest de omgevingsvariabelen AZURE\_SERVICEBUS\_-NAAMRUIMTE en AZURE\_SERVICEBUS\_ACCESS\_sleutel gegevens die nodig zijn om verbinding met de Service Bus te verkrijgen. Als deze omgevingsvariabelen zijn niet ingesteld, moet u de accountgegevens opgeven bij het aanroepen van **createServiceBusService**.

Zie voor een voorbeeld van het instellen van de omgevingsvariabelen in een configuratiebestand voor een Azure Cloud Service [Node.js Cloud Service met opslag][].

Zie voor een voorbeeld van het instellen van de omgevingsvariabelen in [Azure klassieke portal][] voor een Website Azure [Node.js webtoepassing met opslag][].

## <a name="create-a-queue"></a>Een wachtrij maken

Het **ServiceBusService** -object kunt u werken met wachtrijen Service Bus. De volgende code maakt een **ServiceBusService** -object. Toe te voegen aan de bovenkant van het bestand **server.js** na de instructie voor het importeren van de module Azure:

```
var serviceBusService = azure.createServiceBusService();
```

De opgegeven wachtrij (indien aanwezig) wordt geretourneerd door het aanroepen van **createQueueIfNotExists** in het **ServiceBusService** -object, of een nieuwe wachtrij met de opgegeven naam gemaakt. De volgende code wordt een **createQueueIfNotExists** maken of verbinding maken met de wachtrij met de naam `myqueue`:

```
serviceBusService.createQueueIfNotExists('myqueue', function(error){
    if(!error){
        // Queue exists
    }
});
```

**createServiceBusService** ondersteunt ook extra opties waarmee u kunt de standaardinstellingen voor wachtrij zoals live of maximale grootte weergegeven tijd negeren. Het volgende voorbeeld wordt de maximale grootte van 5 GB en een keer met live (TTL)-waarde van 1 minuut:

```
var queueOptions = {
      MaxSizeInMegabytes: '5120',
      DefaultMessageTimeToLive: 'PT1M'
    };

serviceBusService.createQueueIfNotExists('myqueue', queueOptions, function(error){
    if(!error){
        // Queue exists
    }
});
```

### <a name="filters"></a>Filters

Optionele filterbewerkingen kunnen worden toegepast op de bewerkingen die worden uitgevoerd met behulp van **ServiceBusService**. Filteren van bewerkingen kunt opnemen, aanmelden, automatisch opnieuw wordt geprobeerd, enz. Filters zijn objecten die een methode met de handtekening te implementeren:

```
function handle (requestOptions, next)
```

Na de voorbehandeling van opties die de aanvraag doet, roept de methode `next`, een retouraanroep met de volgende handtekening doorgegeven:

```
function (returnObject, finalCallback, next)
```

In dit mechanisme, en na de verwerking van de **returnObject** (de reactie van de aanvraag naar de server), de retouraanroep ofwel moet aanroepen `next` als deze bestaat als u wilt doorgaan met het verwerken van andere filters of gewoon roepen `finalCallback`, die het oproepen van de service wordt beëindigd.

Twee filters die logica opnieuw implementeren zijn opgenomen in de SDK Azure voor Node.js, **ExponentialRetryPolicyFilter** en **LinearRetryPolicyFilter**. De volgende manier maakt een **ServiceBusService** -object dat de **ExponentialRetryPolicyFilter**wordt gebruikt:

```
var retryOperations = new azure.ExponentialRetryPolicyFilter();
var serviceBusService = azure.createServiceBusService().withFilter(retryOperations);
```

## <a name="send-messages-to-a-queue"></a>Berichten naar een wachtrij verzenden

Als u een bericht verzendt naar een wachtrij Service Bus, roept de toepassing de methode **sendQueueMessage** op het **ServiceBusService** -object. Berichten verzonden naar (en ontvangen van) Service Bus wachtrijen zijn **BrokeredMessage** objecten en een aantal standaard eigenschappen (zoals **Label** en **TimeToLive**) hebben een woordenlijst die wordt gebruikt om aangepaste toepassing-specifieke eigenschappen en een willekeurige toepassing gegevens. Een toepassing stelt de hoofdtekst van het bericht in een tekenreeks als het bericht wordt doorgegeven. Alle vereiste eigenschappen van de standaard worden gevuld met de standaardwaarden.

In het volgende voorbeeld wordt gedemonstreerd hoe u een testbericht verzenden naar de wachtrij met de naam `myqueue` met **sendQueueMessage**:

```
var message = {
    body: 'Test message',
    customProperties: {
        testproperty: 'TestValue'
    }};
serviceBusService.sendQueueMessage('myqueue', message, function(error){
    if(!error){
        // message sent
    }
});
```

Service Bus wachtrijen ondersteuning voor een maximale grootte van 256 KB in de [standaard laag](service-bus-premium-messaging.md) en 1 MB in de [Premium-laag](service-bus-premium-messaging.md). De kop, die de van standaard- en aangepaste toepassingseigenschappen bevat, hebben een maximale grootte van 64 KB. Er is geen limiet op het aantal berichten in een wachtrij is gehouden, maar er is een cap op de totale grootte van de berichten die door een wachtrij. De grootte van de wachtrij is gedefinieerd bij het maken, met een maximum van 5 GB. Zie voor meer informatie over quota, [contingenten Service Bus][].

## <a name="receive-messages-from-a-queue"></a>Berichten uit een wachtrij

Berichten worden ontvangen van een wachtrij met behulp van de methode **receiveQueueMessage** van het **ServiceBusService** -object. Standaard worden berichten uit de wachtrij verwijderd als ze worden gelezen; u kunt echter (peek) lezen en vergrendelen van het bericht zonder het te verwijderen uit de wachtrij met de optionele parameter **isPeekLock** instelt op **true**.

Het standaardgedrag van lezen en het bericht verwijderen als onderdeel van het ontvangen van de eenvoud en het meest geschikt voor scenario's waarin een toepassing kan tolereren niet verwerken van een bericht bij een storing. Om dit te begrijpen, kunt u overwegen een scenario waarin de consument ontvangen verzoek verzendt en vervolgens loopt vast voordat deze worden verwerkt. Omdat Service Bus wordt het bericht als het wordt verbruikt, gemarkeerd en vervolgens, wanneer de toepassing wordt opnieuw opgestart en begint berichten opnieuw te gebruiken, deze zal hebt gemist het bericht dat is verbruikt vóór de crash.

Als de parameter **isPeekLock** is ingesteld op **true**, wordt de ontvangen een bewerking twee fase, die het mogelijk maakt voor van ondersteuningstoepassingen die ontbrekende berichten niet kunnen tolereren. Bus-Service een aanvraag ontvangt, deze vindt u het volgende bericht wordt verbruikt, wordt vergrendeld om te voorkomen dat andere consumenten ontvangen als geeft als resultaat de toepassing. Nadat de toepassing is het bericht verwerkt (of betrouwbaar voor toekomstige verwerking opgeslagen), wordt de tweede fase van de procedure ontvangen voltooid door het aanroepen van de methode **deleteMessage** en het leveren van het bericht moet worden verwijderd als een parameter. De methode **deleteMessage** wordt markeren van het bericht worden gebruikt en uit de wachtrij verwijderen.

In het volgende voorbeeld laat zien hoe ontvangen en verwerken van berichten met behulp van **receiveQueueMessage**. In het voorbeeld eerst ontvangt en een bericht verwijdert en vervolgens ontvangt een bericht met behulp van **isPeekLock** ingesteld op **true**en vervolgens verwijdert u het bericht met behulp van **deleteMessage**:

```
serviceBusService.receiveQueueMessage('myqueue', function(error, receivedMessage){
    if(!error){
        // Message received and deleted
    }
});
serviceBusService.receiveQueueMessage('myqueue', { isPeekLock: true }, function(error, lockedMessage){
    if(!error){
        // Message received and locked
        serviceBusService.deleteMessage(lockedMessage, function (deleteError){
            if(!deleteError){
                // Message deleted
            }
        });
    }
});
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Het afhandelen van toepassing crashes en berichten onleesbaar

Bus service biedt functies waarmee u zonder problemen herstellen van fouten in uw toepassing of moeilijkheden verwerken van een bericht. Als een ontvanger het bericht verwerkt voor een of andere reden niet is, kan deze de methode **unlockMessage** aanroepen voor het object **ServiceBusService** . Hierdoor wordt de Service Bus te ontgrendelen van het bericht in de wachtrij en beschikbaar stellen aan opnieuw worden ontvangen door de toepassing in beslag nemen of door een andere toepassing voor in beslag nemen.

Er is ook een time-out is gekoppeld aan een bericht in de wachtrij is vergrendeld en als de aanvraag niet verwerken het bericht voordat u de vergrendeling time-out is verstreken (bijv. Als de toepassing loopt vast), vervolgens Service Bus wordt het bericht automatisch ontgrendelen en beschikbaar stellen aan opnieuw worden ontvangen.

In het geval dat de toepassing vastloopt nadat het bericht is verwerkt, maar voordat de methode **deleteMessage** wordt aangeroepen, zal vervolgens het bericht worden geleverd aan de toepassing opnieuw wordt gestart. Dit wordt vaak genoemd **Tenminste eenmaal verwerken**dat elk bericht wordt ten minste eenmaal verwerkt maar in bepaalde situaties kan hetzelfde bericht worden geleverd. Als het scenario kan niet dubbele verwerking tolereren, kunnen ontwikkelaars van toepassingen moeten extra logica toevoegen aan hun toepassing voor aflevering van dubbele berichten verwerken. Dit wordt vaak bereikt met behulp van de eigenschap **MessageId** van het bericht over zelf constant blijven.

## <a name="next-steps"></a>Volgende stappen

Zie de volgende bronnen voor meer informatie over wachtrijen.

-   [Wachtrijen, onderwerpen en abonnementen][]
-   [Azure SDK voor knooppunt][] opslagplaats op GitHub
-   [Node.js Developer Center](/develop/nodejs/)

  [Azure SDK voor knooppunt]: https://github.com/Azure/azure-sdk-for-node
  [Azure klassieke portal]: http://manage.windowsazure.com
  
  [Node.js Cloud Service]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
  [Wachtrijen, onderwerpen en abonnementen]: service-bus-queues-topics-subscriptions.md
  [Maken en implementeren van een toepassing Node.js naar een Website Azure]: ../app-service-web/web-sites-nodejs-develop-deploy-mac.md
  [Node.js Cloud Service met opslag]: ../cloud-services/storage-nodejs-use-table-storage-cloud-service-app.md
  [Node.js webtoepassing met opslag]: ../storage/storage-nodejs-how-to-use-table-storage.md
  [Quota Service Bus]: service-bus-quotas.md
 
