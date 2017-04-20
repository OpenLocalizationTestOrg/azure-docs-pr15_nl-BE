<properties 
    pageTitle="Het gebruik van de Service Bus onderwerpen met Node.js | Microsoft Azure" 
    description="Informatie over Service Bus onderwerpen en abonnementen in Azure gebruiken vanuit een app Node.js." 
    services="service-bus" 
    documentationCenter="nodejs" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="10/04/2016" 
    ms.author="sethm"/>


# <a name="how-to-use-service-bus-topics-and-subscriptions"></a>Gebruik Service Bus onderwerpen en abonnementen

[AZURE.INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Deze handleiding wordt beschreven hoe u Service Bus onderwerpen en abonnementen van Node.js toepassingen te gebruiken. De scenario's die zijn **maken van onderwerpen en abonnementen**, **abonnement filters maken**, **verzenden van berichten** naar een onderwerp, **ontvangen van berichten van een abonnement**en **onderwerpen en abonnementen te verwijderen**. Zie de sectie [volgende stappen](#next-steps) voor meer informatie over onderwerpen en abonnementen.

[AZURE.INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

## <a name="create-a-nodejs-application"></a>Een Node.js-toepassing maken

Een lege Node.js-toepassing maken. Zie voor meer informatie over het maken van een toepassing Node.js, [maken en implementeren van een toepassing Node.js naar Azure-website], [Node.js Cloud Service][] met Windows PowerShell of website met WebMatrix.

## <a name="configure-your-application-to-use-service-bus"></a>Uw toepassing configureren voor gebruik van de Service Bus

Voor het gebruik van de Service Bus Node.js Azure pakket te downloaden. Dit pakket bevat een reeks bibliotheken die met de REST van Service Bus services communiceren.

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a>Knooppunt Package Manager (NPM) gebruiken voor het verkrijgen van het pakket

1.  Een opdrachtregelinterface gebruiken zoals **PowerShell** (Windows), **Terminal** (Mac) of **Bash** (Unix), Ga naar de map waar u de voorbeeldtoepassing hebt gemaakt.

2.  Typ **npm azure installeren** in het opdrachtvenster in de volgende uitvoer resulteren moet:

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

3.  U kunt handmatig uitvoeren met de opdracht **ls** te controleren dat een **knooppunt\_modules** map is gemaakt. In deze map het **azure** -pakket de bibliotheken die u nodig hebt bevat voor toegang tot de Service Bus onderwerpen te vinden.

### <a name="import-the-module"></a>Importeren van de module

Met Kladblok of een andere teksteditor, Voeg het volgende toe boven aan het bestand **server.js** van de toepassing:

```
var azure = require('azure');
```

### <a name="set-up-a-service-bus-connection"></a>Een Service Bus verbinding instellen

De module Azure leest de omgevingsvariabelen AZURE\_SERVICEBUS\_NAAMRUIMTE en AZURE\_SERVICEBUS\_ACCESS\_sleutel voor meer informatie nodig voor een Service Bus. Als deze omgevingsvariabelen zijn niet ingesteld, moet u de accountgegevens opgeven bij het aanroepen van **createServiceBusService**.

Zie voor een voorbeeld van het instellen van de omgevingsvariabelen in een configuratiebestand voor een Azure Cloud Service [Node.js Cloud Service met opslag][].

Zie voor een voorbeeld van het instellen van de omgevingsvariabelen in [Azure klassieke portal][] voor een Website Azure [Node.js webtoepassing met opslag][].

## <a name="create-a-topic"></a>Een onderwerp maken

Het **ServiceBusService** -object kunt u werken met onderwerpen. De volgende code maakt een **ServiceBusService** -object. Toe te voegen aan de bovenkant van het bestand **server.js** na de instructie voor het importeren van de module azure:

```
var serviceBusService = azure.createServiceBusService();
```

Het opgegeven onderwerp (indien aanwezig) geretourneerd door het aanroepen van **createTopicIfNotExists** in het **ServiceBusService** -object, of een nieuw onderwerp met de opgegeven naam wordt gemaakt. De volgende code wordt een **createTopicIfNotExists** maken of verbinding maken met het onderwerp met de naam 'MyTopic':

```
serviceBusService.createTopicIfNotExists('MyTopic',function(error){
    if(!error){
        // Topic was created or exists
        console.log('topic created or exists.');
    }
});
```

**createServiceBusService** ondersteunt ook extra opties waarmee u kunt de standaardinstellingen voor onderwerp zoals bericht tijd om live of onderwerp maximale grootte negeren. In het volgende voorbeeld wordt de grootte van de maximale onderwerp ingesteld op 5GB met een tijd van 1 minuut live:

```
var topicOptions = {
        MaxSizeInMegabytes: '5120',
        DefaultMessageTimeToLive: 'PT1M'
    };

serviceBusService.createTopicIfNotExists('MyTopic', topicOptions, function(error){
    if(!error){
        // topic was created or exists
    }
});
```

### <a name="filters"></a>Filters

Optionele filterbewerkingen kunnen worden toegepast op de bewerkingen die worden uitgevoerd met behulp van **ServiceBusService**. Filteren van bewerkingen kunt opnemen, aanmelden, automatisch opnieuw wordt geprobeerd, enz. Filters zijn objecten die een methode met de handtekening te implementeren:

```
function handle (requestOptions, next)
```

Na het uitvoeren van voorverwerking van de opties van de aanvraag, de methode roept `next` geven een retouraanroep met de volgende handtekening:

```
function (returnObject, finalCallback, next)
```

In dit mechanisme, en na de verwerking van de **returnObject** (de reactie van de aanvraag naar de server), aanroepen callback moet het een volgende als deze bestaat als u wilt doorgaan met het verwerken van andere filters of gewoon **finalCallback** anders om uiteindelijk het oproepen van de service starten.

Twee filters die logica opnieuw implementeren zijn opgenomen in de SDK Azure voor Node.js, **ExponentialRetryPolicyFilter** en **LinearRetryPolicyFilter**. De volgende manier maakt een **ServiceBusService** -object dat de **ExponentialRetryPolicyFilter**wordt gebruikt:

    var retryOperations = new azure.ExponentialRetryPolicyFilter();
    var serviceBusService = azure.createServiceBusService().withFilter(retryOperations);

## <a name="create-subscriptions"></a>Abonnementen maken

Onderwerp abonnementen worden ook gemaakt met het **ServiceBusService** -object. Abonnementen zijn naam en een optionele filter dat ervoor zorgt dat de reeks berichten die in de virtuele wachtrij van het abonnement kunnen hebben.

> [AZURE.NOTE] Abonnementen zijn permanent en blijft bestaan totdat ofwel ze of het onderwerp zijn gekoppeld, worden verwijderd. Als uw toepassing bevat de logica voor het maken van een abonnement, moet eerst controleren of het abonnement al bestaat met de methode **getSubscription** .

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Een abonnement met het standaard (MatchAll)-filter maken

Het **MatchAll** -filter is het standaard-filter die wordt gebruikt als er geen filter is opgegeven bij het maken van een nieuw abonnement. Als u het filter **MatchAll** gebruikt, worden alle berichten die zijn gepubliceerd op het onderwerp in van het abonnement virtuele wachtrij geplaatst. In het volgende voorbeeld wordt gemaakt van een abonnement met de naam 'AllMessages' en wordt het standaardfilter voor **MatchAll** .

```
serviceBusService.createSubscription('MyTopic','AllMessages',function(error){
    if(!error){
        // subscription created
    }
});
```

### <a name="create-subscriptions-with-filters"></a>Abonnementen met filters maken

U kunt ook filters maken waarmee u naar een bereik dat berichten worden verzonden naar een onderwerp moet worden weergegeven binnen een bepaald onderwerp-abonnement.

De meest flexibele type filter wordt ondersteund door abonnementen is de **SqlFilter**, die een subset van de SQL92-standaard implementeert. SQL-filters worden toegepast op de eigenschappen van de berichten die worden gepubliceerd naar het onderwerp. Voor meer informatie over de expressies die kunnen worden gebruikt met een SQL-filter controleren de [SqlFilter.SqlExpression] [ SqlFilter.SqlExpression] syntaxis.

Filters kunnen worden toegevoegd aan een abonnement met behulp van de methode **createRule** van het **ServiceBusService** -object. Met deze methode kunt u nieuwe filters toevoegen aan een bestaand abonnement.

> [AZURE.NOTE] Omdat het standaardfilter automatisch op alle nieuwe abonnementen toegepast wordt, moet u eerst het standaardfilter te verwijderen of de **MatchAll** , heeft voorrang op andere filters die u kunt opgeven. U kunt de standaardregel verwijderen met behulp van de methode **deleteRule** van het **ServiceBusService** -object.

In het volgende voorbeeld maakt u een abonnement met de naam `HighMessages` met een **SqlFilter** dat alleen berichten die groter is dan 3 **messagenumber** van de aangepaste eigenschap selecteert:

```
serviceBusService.createSubscription('MyTopic', 'HighMessages', function (error){
    if(!error){
        // subscription created
        rule.create();
    }
});
var rule={
    deleteDefault: function(){
        serviceBusService.deleteRule('MyTopic',
            'HighMessages', 
            azure.Constants.ServiceBusConstants.DEFAULT_RULE_NAME, 
            rule.handleError);
    },
    create: function(){
        var ruleOptions = {
            sqlExpressionFilter: 'messagenumber > 3'
        };
        rule.deleteDefault();
        serviceBusService.createRule('MyTopic', 
            'HighMessages', 
            'HighMessageFilter', 
            ruleOptions, 
            rule.handleError);
    },
    handleError: function(error){
        if(error){
            console.log(error)
        }
    }
}
```

Ook in het volgende voorbeeld maakt u een abonnement met de naam `LowMessages` met een **SqlFilter** die u selecteert alleen berichten met een **messagenumber** eigenschap kleiner is dan of gelijk aan 3:

```
serviceBusService.createSubscription('MyTopic', 'LowMessages', function (error){
    if(!error){
        // subscription created
        rule.create();
    }
});
var rule={
    deleteDefault: function(){
        serviceBusService.deleteRule('MyTopic',
            'LowMessages', 
            azure.Constants.ServiceBusConstants.DEFAULT_RULE_NAME, 
            rule.handleError);
    },
    create: function(){
        var ruleOptions = {
            sqlExpressionFilter: 'messagenumber <= 3'
        };
        rule.deleteDefault();
        serviceBusService.createRule('MyTopic', 
            'LowMessages', 
            'LowMessageFilter', 
            ruleOptions, 
            rule.handleError);
    },
    handleError: function(error){
        if(error){
            console.log(error)
        }
    }
}
```

Wanneer nu een bericht wordt verzonden naar `MyTopic`, wordt altijd geleverd met ontvangers geabonneerd op de `AllMessages` onderwerp-abonnement en selectief geleverde naar ontvangers die zijn geabonneerd op de `HighMessages` en `LowMessages` onderwerp abonnementen (afhankelijk van de inhoud van het bericht).

## <a name="how-to-send-messages-to-a-topic"></a>Het verzenden van berichten naar een onderwerp

Als u een bericht verzendt naar een onderwerp Service Bus, moet uw toepassing de methode **sendTopicMessage** van het **ServiceBusService** -object gebruiken.
Berichten die worden verzonden naar Service Bus onderwerpen zijn **BrokeredMessage** objecten.
**BrokeredMessage** -objecten hebben een aantal standaard eigenschappen (zoals **Label** en **TimeToLive**), een woordenlijst die wordt gebruikt om specifieke eigenschappen van aangepaste toepassingen en een hoofdtekst van tekenreeksen. Een toepassing kunt instellen in de hoofdtekst van het bericht een string-waarde doorgeven aan de **sendTopicMessage** en alle vereiste eigenschappen van de standaard zal worden gevuld met de standaardwaarden.

In het volgende voorbeeld ziet u het verzenden van vijf testberichten naar 'MyTopic'. Merk op dat de waarde van de eigenschap **messagenumber** van elk bericht is afhankelijk van de herhaling van de lus (dit veld bepaalt welke abonnementen ontvangen):

```
var message = {
    body: '',
    customProperties: {
        messagenumber: 0
    }
}

for (i = 0;i < 5;i++) {
    message.customProperties.messagenumber=i;
    message.body='This is Message #'+i;
    serviceBusService.sendTopicMessage(topic, message, function(error) {
      if (error) {
        console.log(error);
      }
    });
}
```

Onderwerpen Service Bus ondersteunt een maximale grootte van 256 KB in de [standaard laag](service-bus-premium-messaging.md) en 1 MB in de [premie laag](service-bus-premium-messaging.md). De kop, die de van standaard- en aangepaste toepassingseigenschappen bevat, hebben een maximale grootte van 64 KB. Er is geen limiet op het aantal berichten in een onderwerp gehouden, maar er is een cap op de totale grootte van de berichten die door een onderwerp. De grootte van dit onderwerp is gedefinieerd bij het maken, met een maximum van 5 GB.

## <a name="receive-messages-from-a-subscription"></a>Berichten ontvangen van een abonnement

Berichten worden ontvangen van een abonnement op het **ServiceBusService** -object met behulp van de methode **receiveSubscriptionMessage** . Standaard zijn berichten verwijderd van het abonnement zoals ze worden gelezen; u kunt echter (peek) lezen en vergrendelen van het bericht zonder het te verwijderen uit het abonnement met de optionele parameter **isPeekLock** instelt op **true**.

Het standaardgedrag van lezen en het bericht verwijderen als onderdeel van het ontvangen van de eenvoud en het meest geschikt voor scenario's waarin een toepassing kan tolereren niet verwerken van een bericht bij een storing. Om dit te begrijpen, kunt u overwegen een scenario waarin de consument ontvangen verzoek verzendt en vervolgens loopt vast voordat deze worden verwerkt. Omdat Service Bus wordt het bericht als het wordt verbruikt, gemarkeerd en vervolgens, wanneer de toepassing wordt opnieuw opgestart en begint berichten opnieuw te gebruiken, deze zal hebt gemist het bericht dat is verbruikt vóór de crash.

Als de parameter **isPeekLock** is ingesteld op **true**, wordt de ontvangen een bewerking twee fase, die het mogelijk maakt voor van ondersteuningstoepassingen die ontbrekende berichten niet kunnen tolereren. Bus-Service een aanvraag ontvangt, deze vindt u het volgende bericht wordt verbruikt, wordt vergrendeld om te voorkomen dat andere consumenten ontvangen als geeft als resultaat de toepassing.
Nadat de toepassing is het bericht verwerkt (of betrouwbaar voor toekomstige verwerking opgeslagen), wordt de tweede fase van de procedure ontvangen voltooid door het aanroepen van de methode **deleteMessage** en het leveren van het bericht moet worden verwijderd als een parameter. De methode **deleteMessage** wordt markeren van het bericht worden gebruikt en het verwijderen van het abonnement.

In het volgende voorbeeld wordt gedemonstreerd hoe berichten kunnen worden ontvangen en verwerkt met behulp van **receiveSubscriptionMessage**. In het voorbeeld eerst ontvangt een bericht verwijdert uit het abonnement 'LowMessages' en vervolgens een bericht ontvangt van de 'HighMessages'-abonnement met **isPeekLock** is ingesteld op true. Het verwijdert het bericht met behulp van **deleteMessage**:

```
serviceBusService.receiveSubscriptionMessage('MyTopic', 'LowMessages', function(error, receivedMessage){
    if(!error){
        // Message received and deleted
        console.log(receivedMessage);
    }
});
serviceBusService.receiveSubscriptionMessage('MyTopic', 'HighMessages', { isPeekLock: true }, function(error, lockedMessage){
    if(!error){
        // Message received and locked
        console.log(lockedMessage);
        serviceBusService.deleteMessage(lockedMessage, function (deleteError){
            if(!deleteError){
                // Message deleted
                console.log('message has been deleted.');
            }
        }
    }
});
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Het afhandelen van toepassing crashes en berichten onleesbaar

Bus service biedt functies waarmee u zonder problemen herstellen van fouten in uw toepassing of moeilijkheden verwerken van een bericht. Als een ontvanger het bericht verwerkt voor een of andere reden niet is, kan deze de methode **unlockMessage** aanroepen voor het object **ServiceBusService** . Hierdoor wordt de Service Bus te ontgrendelen van het bericht binnen het abonnement en beschikbaar stellen aan opnieuw worden ontvangen door de toepassing in beslag nemen of door een andere toepassing voor in beslag nemen.

Er is ook een time-out is gekoppeld aan een bericht in het abonnement is vergrendeld en als de aanvraag niet verwerken het bericht voordat u de vergrendeling time-out is verstreken (bijvoorbeeld, als de toepassing loopt vast), vervolgens Bus-Service het bericht automatisch wordt ontgrendeld en beschikbaar moeten opnieuw worden ontvangen.

In het geval dat de toepassing vastloopt nadat het bericht is verwerkt, maar voordat de methode **deleteMessage** wordt aangeroepen, zal vervolgens het bericht worden geleverd aan de toepassing opnieuw wordt gestart. Dit wordt vaak genoemd **Tenminste eenmaal verwerken**dat elk bericht wordt ten minste eenmaal verwerkt maar in bepaalde situaties kan hetzelfde bericht worden geleverd. Als het scenario kan niet dubbele verwerking tolereren, kunnen ontwikkelaars van toepassingen moeten extra logica toevoegen aan hun toepassing voor aflevering van dubbele berichten verwerken. Dit wordt vaak bereikt met behulp van de eigenschap **MessageId** van het bericht over zelf constant blijven.

## <a name="delete-topics-and-subscriptions"></a>Onderwerpen en abonnementen verwijderen

Onderwerpen abonnementen zijn permanent en moeten expliciet worden verwijderd door de [Azure klassieke portal][] of via programmering.
In het volgende voorbeeld geeft aan hoe u het onderwerp met de naam `MyTopic`:

    serviceBusService.deleteTopic('MyTopic', function (error) {
        if (error) {
            console.log(error);
        }
    });

Een onderwerp worden ook verwijdert, alle abonnementen die zijn geregistreerd bij het onderwerp. Abonnementen kunnen ook afzonderlijk worden verwijderd. In het volgende voorbeeld ziet u hoe te verwijderen van een abonnement met de naam `HighMessages` van de `MyTopic` onderwerp:

    serviceBusService.deleteSubscription('MyTopic', 'HighMessages', function (error) {
        if(error) {
            console.log(error);
        }
    });

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd dat de grondbeginselen van Service Bus-onderwerpen, klik op deze koppelingen voor meer informatie.

-   Zie [wachtrijen, onderwerpen, en abonnementen][].
-   API-Naslaggids voor [SqlFilter][].
-   Ga naar de opslagplaats [Azure SDK voor knooppunt][] op GitHub.

  [Azure SDK voor knooppunt]: https://github.com/Azure/azure-sdk-for-node
  [Azure klassieke portal]: https://manage.windowsazure.com
  [SqlFilter.SqlExpression]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
  [Wachtrijen, onderwerpen en abonnementen]: service-bus-queues-topics-subscriptions.md
  [SqlFilter]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.aspx
  [Node.js Cloud Service]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
  [Maken en implementeren van een toepassing Node.js naar Azure-website]: ../app-service-web/web-sites-nodejs-develop-deploy-mac.md
  [Node.js Cloud Service met opslag]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
  [Node.js webtoepassing met opslag]: ../cloud-services/storage-nodejs-use-table-storage-cloud-service-app.md
 
