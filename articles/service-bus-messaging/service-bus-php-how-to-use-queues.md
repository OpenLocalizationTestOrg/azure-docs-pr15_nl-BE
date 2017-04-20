<properties 
    pageTitle="Het gebruik van de Service Bus wachtrijen met PHP | Microsoft Azure" 
    description="Informatie over het gebruik van wachtrijen Service Bus in Azure. Voorbeelden van code is geschreven in PHP." 
    services="service-bus" 
    documentationCenter="php" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="PHP" 
    ms.topic="article" 
    ms.date="10/04/2016" 
    ms.author="sethm"/>

# <a name="how-to-use-service-bus-queues"></a>Het gebruik van wachtrijen Service Bus

[AZURE.INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

Deze handleiding wordt beschreven hoe u met de Service Bus wachtrijen. De monsters zijn geschreven in PHP en gebruikt u de [SDK voor PHP Azure](../php-download-sdk.md). De scenario's die zijn **het maken van wachtrijen** **verzenden en ontvangen van berichten**en **wachtrijen verwijderen**.

[AZURE.INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

## <a name="create-a-php-application"></a>Een PHP-toepassing maken

De enige vereiste voor het maken van een PHP-toepassing die toegang heeft tot de service Azure Blob is de verwijzing naar de klassen in de [SDK voor PHP Azure](../php-download-sdk.md) vanuit uw code. Voor het maken van uw toepassing of Kladblok kunt u alle ontwikkelingsprogramma's.

> [AZURE.NOTE] Ook moet de PHP-installatie de [extensie OpenSSL](http://php.net/openssl) geïnstalleerd en ingeschakeld.

In deze handleiding vindt u service-functies die kunnen worden aangeroepen vanuit binnen een PHP-toepassing lokaal of in de code die wordt uitgevoerd binnen een webpagina Azure rol, rol van werknemer of website gebruikt.

## <a name="get-the-azure-client-libraries"></a>De client Azure bibliotheken ophalen

[AZURE.INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-to-use-service-bus"></a>Uw toepassing configureren voor gebruik van de Service Bus

Voor het gebruik van de Service Bus wachtrij API's het volgende doen:

1. Een verwijzing naar de autoloader-bestand met behulp van de [require_once] [ require_once] instructie.
2. Verwijzen naar alle klassen die u kunt gebruiken.

In het volgende voorbeeld ziet u hoe de autoloader-bestand opnemen en verwijzen naar de klasse **ServicesBuilder** .

> [AZURE.NOTE] In dit voorbeeld (en andere voorbeelden in dit artikel) wordt aangenomen dat u de PHP Client Libraries voor Azure via Composer hebt geïnstalleerd. Als u de bibliotheken handmatig of als PEER-pakket hebt geïnstalleerd, moet u verwijzen naar het bestand **WindowsAzure.php** -autoloader.

```
require_once 'vendor/autoload.php';
use WindowsAzure\Common\ServicesBuilder;
```

In de voorbeelden hieronder, de `require_once` instructie worden altijd weergegeven, maar alleen de klassen die nodig zijn voor het voorbeeld uit te voeren naar worden verwezen.

## <a name="set-up-a-service-bus-connection"></a>Een Service Bus verbinding instellen

Voor het concretiseren van een client Service Bus, moet u eerst een geldige verbindingsreeks in deze indeling hebben:

```
Endpoint=[yourEndpoint];SharedSecretIssuer=[Default Issuer];SharedSecretValue=[Default Key]
```

Waar **eindpunt** is doorgaans de notatie `[yourNamespace].servicebus.windows.net`.

Elke client Azure service maken, moet u de klasse **ServicesBuilder** gebruiken. U kunt:

* De verbindingsreeks voor het doorgeven.
* **CloudConfigurationManager (CCM)** gebruiken om te controleren van meerdere externe bronnen voor de verbindingsreeks:
    * Standaard wordt geleverd met ondersteuning voor een externe bron - omgevingsvariabelen
    * U kunt nieuwe bronnen toevoegen door de klasse **ConnectionStringSource** uit te breiden

Voor de hier beschreven voorbeelden, de verbindingsreeks rechtstreeks doorgegeven.

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;

$connectionString = "Endpoint=[yourEndpoint];SharedSecretIssuer=[Default Issuer];SharedSecretValue=[Default Key]";

$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
```

## <a name="how-to-create-a-queue"></a>Procedure: een wachtrij maken

U kunt beheertaken uit te voeren voor een Service Bus wachtrijen via de klasse **ServiceBusRestProxy** uitvoeren. Een **ServiceBusRestProxy** -object is gemaakt via de fabrieksmethode **ServicesBuilder::createServiceBusService** met een geschikte verbindingsreeks die de token machtigingen kapselt te beheren.

In het volgende voorbeeld ziet u hoe een **ServiceBusRestProxy** maken en roept **ServiceBusRestProxy createQueue ->** Maak een wachtrij met de naam `myqueue` binnen een `MySBNamespace` Servicenaamruimte:

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\QueueInfo;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
    
try {
    $queueInfo = new QueueInfo("myqueue");
        
    // Create queue.
    $serviceBusRestProxy->createQueue($queueInfo);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here: 
    // http://msdn.microsoft.com/library/windowsazure/dd179357
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

> [AZURE.NOTE] U kunt de `listQueues` methode op `ServiceBusRestProxy` objecten om te controleren of een wachtrij met een opgegeven naam al in een naamruimte bestaat.

## <a name="how-to-send-messages-to-a-queue"></a>Procedure: berichten naar een wachtrij verzenden

Als u een bericht verzendt naar een wachtrij Service Bus, roept de toepassing de methode **ServiceBusRestProxy -> sendQueueMessage** . De volgende code toont hoe u berichten verzenden naar de `myqueue` wachtrij hebt gemaakt in de `MySBNamespace` Servicenaamruimte.

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\BrokeredMessage;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
        
try {
    // Create message.
    $message = new BrokeredMessage();
    $message->setBody("my message");
    
    // Send message.
    $serviceBusRestProxy->sendQueueMessage("myqueue", $message);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here: 
    // http://msdn.microsoft.com/library/windowsazure/hh780775
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

Berichten verzonden naar (en ontvangen van) Service Bus wachtrijen zijn instanties van de klasse **BrokeredMessage** . **BrokeredMessage** -objecten hebben een set van standaardmethoden (zoals **getLabel**, **getTimeToLive**, **setLabel**en **setTimeToLive**) en eigenschappen die worden gebruikt voor het opslaan van aangepaste eigenschappen voor specifieke applicaties en een instantie van een willekeurige toepassingsgegevens.

Service Bus wachtrijen ondersteuning voor een maximale grootte van 256 KB in de [standaard laag](service-bus-premium-messaging.md) en 1 MB in de [Premium-laag](service-bus-premium-messaging.md). De kop, die de van standaard- en aangepaste toepassingseigenschappen bevat, hebben een maximale grootte van 64 KB. Er is geen limiet op het aantal berichten in een wachtrij is gehouden, maar er is een cap op de totale grootte van de berichten die door een wachtrij. Deze bovengrens voor de grootte van de wachtrij is 5 GB.

## <a name="how-to-receive-messages-from-a-queue"></a>Het ontvangen van berichten uit een wachtrij

De beste manier voor het ontvangen van berichten uit een wachtrij is met een **ServiceBusRestProxy -> receiveQueueMessage** -methode. Berichten kunnen worden ontvangen in twee verschillende modi: **ReceiveAndDelete** (de standaardinstelling) en **PeekLock**.

Wanneer de modus **ReceiveAndDelete** wordt één single-shot - dat wil zeggen, wanneer Service Bus een leesbevestiging voor een bericht in een wachtrij ontvangt, wordt gemarkeerd als wordt verbruikt en terug naar de toepassing. **ReceiveAndDelete** modus is de eenvoudigste en meest geschikt voor scenario's waarin een toepassing kan tolereren niet verwerken van een bericht bij een storing. Om dit te begrijpen, kunt u overwegen een scenario waarin de consument ontvangen verzoek verzendt en vervolgens loopt vast voordat deze worden verwerkt. Omdat Service Bus wordt het bericht als het wordt verbruikt, gemarkeerd en vervolgens, wanneer de toepassing wordt opnieuw opgestart en begint berichten opnieuw te gebruiken, deze zal hebt gemist het bericht dat is verbruikt vóór de crash.

In de **PeekLock** modus wordt ontvangen van een bericht een bewerking twee fase, die het mogelijk maakt voor van ondersteuningstoepassingen die ontbrekende berichten niet kunnen tolereren. Bus-Service een aanvraag ontvangt, deze vindt u het volgende bericht wordt verbruikt, wordt vergrendeld om te voorkomen dat andere consumenten ontvangen als geeft als resultaat de toepassing. Nadat de toepassing is het bericht verwerkt (of betrouwbaar voor toekomstige verwerking opgeslagen), wordt de tweede fase van de procedure ontvangen door het ontvangen bericht wordt doorgegeven aan **ServiceBusRestProxy -> deleteMessage**voltooid. Wanneer Service Bus **deleteMessage** aanroept, wordt het markeren van het bericht worden gebruikt en uit de wachtrij verwijderen.

In het volgende voorbeeld ziet u hoe een bericht kan worden ontvangen en verwerkt met behulp van **PeekLock** -modus (niet de standaardmodus).

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\ReceiveMessageOptions;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
        
try {
    // Set the receive mode to PeekLock (default is ReceiveAndDelete).
    $options = new ReceiveMessageOptions();
    $options->setPeekLock();
        
    // Receive message.
    $message = $serviceBusRestProxy->receiveQueueMessage("myqueue", $options);
    echo "Body: ".$message->getBody()."<br />";
    echo "MessageID: ".$message->getMessageId()."<br />";
        
    /*---------------------------
        Process message here.
    ----------------------------*/
        
    // Delete message. Not necessary if peek lock is not set.
    echo "Message deleted.<br />";
    $serviceBusRestProxy->deleteMessage($message);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/windowsazure/hh780735
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Procedure: toepassing crashes en onleesbare berichten verwerken

Bus service biedt functies waarmee u zonder problemen herstellen van fouten in uw toepassing of moeilijkheden verwerken van een bericht. Als een ontvanger het bericht verwerkt voor een of andere reden niet is, kan deze de methode **unlockMessage** aanroepen voor het ontvangen bericht (in plaats van de methode **deleteMessage** ). Hierdoor wordt de Service Bus te ontgrendelen van het bericht in de wachtrij en beschikbaar stellen aan opnieuw worden ontvangen door de toepassing in beslag nemen of door een andere toepassing voor in beslag nemen.

Er is ook een time-out is gekoppeld aan een bericht in de wachtrij is vergrendeld en als de aanvraag niet verwerken het bericht voordat u de vergrendeling time-out is verstreken (bijvoorbeeld, als de toepassing loopt vast), vervolgens Service Bus wordt het bericht automatisch ontgrendelen en beschikbaar stellen aan opnieuw worden ontvangen.

In het geval dat de toepassing vastloopt nadat het bericht is verwerkt, maar voordat de **deleteMessage** -aanvraag wordt ingediend, zal vervolgens het bericht worden geleverd aan de toepassing opnieuw wordt gestart. Dit wordt vaak genoemd **Tenminste één keer verwerken**; elk bericht wordt verwerkt, ten minste één keer maar in bepaalde situaties kan hetzelfde bericht worden geleverd. Als het scenario kan geen dubbele verwerking tolereren, wordt vervolgens extra logica toe te voegen aan toepassingen voor het verwerken van dubbele berichtbezorging aanbevolen. Dit wordt vaak bereikt met behulp van de methode **getMessageId** van het bericht over zelf constant blijft.

## <a name="next-steps"></a>Volgende stappen

Zie u hebt geleerd dat de grondbeginselen van Service Bus wachtrijen, [wachtrijen, onderwerpen, en abonnementen][] voor meer informatie.

Zie ook [PHP Developer Center](/develop/php/)voor meer informatie.

[Wachtrijen, onderwerpen en abonnementen]: service-bus-queues-topics-subscriptions.md
[require_once]: http://php.net/require_once

 
