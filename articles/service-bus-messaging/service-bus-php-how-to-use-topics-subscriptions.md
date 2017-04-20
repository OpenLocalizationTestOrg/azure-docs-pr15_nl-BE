<properties 
    pageTitle="Het gebruik van de Service Bus onderwerpen met PHP | Microsoft Azure" 
    description="Informatie over Service Bus onderwerpen gebruiken met PHP in Azure." 
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
    ms.date="10/14/2016" 
    ms.author="sethm"/>


# <a name="how-to-use-service-bus-topics-and-subscriptions"></a>Het gebruik van de Service Bus onderwerpen en abonnementen

[AZURE.INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

In dit artikel wordt beschreven hoe u Service Bus onderwerpen en abonnementen. De monsters zijn geschreven in PHP en gebruikt u de [SDK voor PHP Azure](../php-download-sdk.md). De scenario's die zijn **onderwerpen en abonnementen maken** **abonnement filters maken**, **berichten verzenden naar een onderwerp**, **ontvangen van berichten van een abonnement**en **onderwerpen en abonnementen verwijderen**.

[AZURE.INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

## <a name="create-a-php-application"></a>Een PHP-toepassing maken

De enige vereiste voor het maken van een PHP-toepassing die toegang heeft tot de service Azure Blob is verwijzen naar klassen in de [SDK voor PHP Azure](../php-download-sdk.md) van uw code. Voor het maken van uw toepassing of Kladblok kunt u alle ontwikkelingsprogramma's.

> [AZURE.NOTE] Ook moet de PHP-installatie de [extensie OpenSSL](http://php.net/openssl) geïnstalleerd en ingeschakeld.

Dit artikel wordt beschreven hoe u met functies die kunnen worden aangeroepen vanuit een PHP-toepassing lokaal of in de code die wordt uitgevoerd binnen een webpagina Azure rol, rol van werknemer of website.

## <a name="get-the-azure-client-libraries"></a>De client Azure bibliotheken ophalen

[AZURE.INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-to-use-service-bus"></a>Uw toepassing configureren voor gebruik van de Service Bus

De Service Bus-API's gebruiken:

1. Een verwijzing naar de autoloader-bestand met behulp van de [require_once] [ require-once] instructie.
2. Verwijzen naar alle klassen die u kunt gebruiken.

In het volgende voorbeeld ziet u hoe de autoloader-bestand opnemen en verwijzen naar de klasse **ServiceBusService** .

> [AZURE.NOTE] In dit voorbeeld (en andere voorbeelden in dit artikel) wordt aangenomen dat u de PHP Client Libraries voor Azure via Composer hebt geïnstalleerd. Als u de bibliotheken handmatig of als PEER-pakket hebt geïnstalleerd, moet u verwijzen naar het bestand **WindowsAzure.php** -autoloader.

```
require_once 'vendor\autoload.php';
use WindowsAzure\Common\ServicesBuilder;
```

In de volgende voorbeelden wordt de `require_once` instructie worden altijd weergegeven, maar alleen de klassen die nodig zijn voor het voorbeeld uit te voeren naar worden verwezen.

## <a name="set-up-a-service-bus-connection"></a>Een Service Bus verbinding instellen

Voor het concretiseren van een client Service Bus moet u een geldige verbindingsreeks in de volgende notatie hebben:

```
Endpoint=[yourEndpoint];SharedSecretIssuer=[Default Issuer];SharedSecretValue=[Default Key]
```

Waarbij `Endpoint` is gewoonlijk de indeling van `https://[yourNamespace].servicebus.windows.net`.

Elke client Azure service maken, moet u de klasse **ServicesBuilder** gebruiken. U kunt:

* De verbindingsreeks voor het doorgeven.
* **CloudConfigurationManager (CCM)** gebruiken om te controleren van meerdere externe bronnen voor de verbindingsreeks:
    * Standaard wordt geleverd met ondersteuning voor een externe bron - omgevingsvariabelen.
    * U kunt nieuwe bronnen toevoegen door de klasse **ConnectionStringSource** uit te breiden.

Voor de hier beschreven voorbeelden, de verbindingsreeks rechtstreeks doorgegeven.

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
    
$connectionString = "Endpoint=[yourEndpoint];SharedSecretIssuer=[Default Issuer];SharedSecretValue=[Default Key]";

$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
```

## <a name="create-a-topic"></a>Een onderwerp maken

U kunt beheertaken uit te voeren voor een Service Bus onderwerpen via de klasse **ServiceBusRestProxy** uitvoeren. Een **ServiceBusRestProxy** -object is gemaakt via de fabrieksmethode **ServicesBuilder::createServiceBusService** met een geschikte verbindingsreeks die de token machtigingen kapselt te beheren.

In het volgende voorbeeld ziet u hoe een **ServiceBusRestProxy** maken en roept **ServiceBusRestProxy -> createTopic** te maken van een onderwerp met de naam `mytopic` binnen een `MySBNamespace` naamruimte:

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\TopicInfo;
    
// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
    
try {       
    // Create topic.
    $topicInfo = new TopicInfo("mytopic");
    $serviceBusRestProxy->createTopic($topicInfo);
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

> [AZURE.NOTE] U kunt de `listTopics` methode op `ServiceBusRestProxy` objecten om te controleren of een onderwerp met een opgegeven naam al in de Servicenaamruimte van een bestaat.

## <a name="create-a-subscription"></a>Een abonnement maken

Onderwerp abonnementen worden ook gemaakt met de methode **ServiceBusRestProxy -> createSubscription** . Abonnementen worden benoemd en een optionele filter dat ervoor zorgt dat de reeks berichten die worden doorgegeven aan de virtuele wachtrij van het abonnement kunnen hebben.

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Een abonnement met het standaard (MatchAll)-filter maken

Het **MatchAll** -filter is het standaard-filter die wordt gebruikt als er geen filter is opgegeven bij het maken van een nieuw abonnement. Als u het filter **MatchAll** gebruikt, worden alle berichten die zijn gepubliceerd op het onderwerp in van het abonnement virtuele wachtrij geplaatst. In het volgende voorbeeld wordt gemaakt van een abonnement met de naam 'mysubscription' en wordt het standaardfilter voor **MatchAll** .

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\SubscriptionInfo;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
    
try {
    // Create subscription.
    $subscriptionInfo = new SubscriptionInfo("mysubscription");
    $serviceBusRestProxy->createSubscription("mytopic", $subscriptionInfo);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here: 
    // http://msdn.microsoft.com/library/azure/dd179357
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

### <a name="create-subscriptions-with-filters"></a>Abonnementen met filters maken

U kunt ook filters waarmee u kunt opgeven welke berichten worden verzonden naar een onderwerp moeten instellen binnen een bepaald onderwerp abonnement weergegeven. De meest flexibele type filter wordt ondersteund door abonnementen is de **SqlFilter**, die een subset van de SQL92-standaard implementeert. SQL-filters worden toegepast op de eigenschappen van de berichten die worden gepubliceerd naar het onderwerp. Zie [De eigenschap SqlFilter.SqlExpression]voor meer informatie over SqlFilters,[sqlfilter].

> [AZURE.NOTE] Elke regel in een abonnement verwerkt inkomende berichten onafhankelijk van elkaar, hun Resultaatberichten toe te voegen aan het abonnement. Elk nieuw abonnement heeft bovendien een standaard **Rule** -object met een filter waarmee alle berichten van het onderwerp aan het abonnement wordt toegevoegd. Als u wilt alleen berichten die overeenkomen met het filter, moet u de standaardregel verwijderen. U kunt de standaardregel verwijderen met behulp van de `ServiceBusRestProxy->deleteRule` methode.

In het volgende voorbeeld maakt u een abonnement met de naam **HighMessages** met een **SqlFilter** die u alleen berichten met een aangepaste eigenschap op de **MessageNumber** groter is dan 3 selecteert (Zie [berichten verzenden naar een onderwerp](#send-messages-to-a-topic) voor meer informatie over aangepaste eigenschappen toevoegen aan berichten):

```
$subscriptionInfo = new SubscriptionInfo("HighMessages");
$serviceBusRestProxy->createSubscription("mytopic", $subscriptionInfo);

$serviceBusRestProxy->deleteRule("mytopic", "HighMessages", '$Default');

$ruleInfo = new RuleInfo("HighMessagesRule");
$ruleInfo->withSqlFilter("MessageNumber > 3");
$ruleResult = $serviceBusRestProxy->createRule("mytopic", "HighMessages", $ruleInfo);
```

Dat deze code het gebruik van een naamruimte aanvullende vereist Opmerking: `WindowsAzure\ServiceBus\Models\SubscriptionInfo`.

Het volgende voorbeeld wordt op dezelfde manier een abonnement met de naam **LowMessages** met een **SqlFilter** die u selecteert alleen berichten met een **MessageNumber** eigenschap kleiner is dan of gelijk aan 3:

```
$subscriptionInfo = new SubscriptionInfo("LowMessages");
$serviceBusRestProxy->createSubscription("mytopic", $subscriptionInfo);

$serviceBusRestProxy->deleteRule("mytopic", "LowMessages", '$Default');

$ruleInfo = new RuleInfo("LowMessagesRule");
$ruleInfo->withSqlFilter("MessageNumber <= 3");
$ruleResult = $serviceBusRestProxy->createRule("mytopic", "LowMessages", $ruleInfo);
```

Nu, wanneer een bericht wordt verzonden naar de `mytopic` onderwerp, er wordt altijd geleverd met ontvangers geabonneerd op de `mysubscription` -abonnement en selectief geleverde naar ontvangers die zijn geabonneerd op de `HighMessages` en `LowMessages` abonnementen (afhankelijk van de inhoud van het bericht).

## <a name="send-messages-to-a-topic"></a>Berichten verzenden naar een onderwerp

Als u een bericht verzendt naar een onderwerp Service Bus, roept de toepassing de methode **ServiceBusRestProxy -> sendTopicMessage** . De volgende code toont hoe u berichten verzenden naar de `mytopic` onderwerp hebt gemaakt in de `MySBNamespace` Servicenaamruimte.

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
    $serviceBusRestProxy->sendTopicMessage("mytopic", $message);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here: 
    // http://msdn.microsoft.com/library/azure/hh780775
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

Berichten die worden verzonden naar Service Bus onderwerpen zijn instanties van de klasse **BrokeredMessage** . **BrokeredMessage** -objecten hebben een aantal standaardeigenschappen en -methoden (zoals **getLabel**, **getTimeToLive**, **setLabel**en **setTimeToLive**), en eigenschappen die kunnen worden gebruikt voor het opslaan van aangepaste eigenschappen voor specifieke applicaties. In het volgende voorbeeld ziet u hoe 5 testbericht verzenden naar de `mytopic` onderwerp hebt gemaakt. De methode **setProperty** wordt gebruikt om een aangepaste eigenschap toevoegen (`MessageNumber`) aan elk bericht. Houd er rekening mee dat de `MessageNumber` waarde van de eigenschap verschilt voor elk bericht (u kunt deze waarde om te bepalen welke abonnementen ontvangen, zoals wordt weergegeven in de sectie [een abonnement maken](#create-a-subscription) ):

```
for($i = 0; $i < 5; $i++){
    // Create message.
    $message = new BrokeredMessage();
    $message->setBody("my message ".$i);
            
    // Set custom property.
    $message->setProperty("MessageNumber", $i);
            
    // Send message.
    $serviceBusRestProxy->sendTopicMessage("mytopic", $message);
}
```

Onderwerpen Service Bus ondersteunt een maximale grootte van 256 KB in de [standaard laag](service-bus-premium-messaging.md) en 1 MB in de [premie laag](service-bus-premium-messaging.md). De kop, die de van standaard- en aangepaste toepassingseigenschappen bevat, hebben een maximale grootte van 64 KB. Er is geen limiet op het aantal berichten in een onderwerp gehouden, maar er is een cap op de totale grootte van de berichten die door een onderwerp. Deze bovengrens van onderwerp grootte is 5 GB. Zie voor meer informatie over quota, [contingenten Service Bus][].

## <a name="receive-messages-from-a-subscription"></a>Berichten ontvangen van een abonnement

De beste manier om berichten te ontvangen van een abonnement is met een **ServiceBusRestProxy -> receiveSubscriptionMessage** -methode. Ontvangen berichten kunnen in twee verschillende modi werken: **ReceiveAndDelete** (de standaardinstelling) en **PeekLock**.

Wanneer de modus **ReceiveAndDelete** wordt is een single-shot-bewerking dat wil zeggen, wanneer de Bus Service een leesbevestiging voor een bericht in een abonnement ontvangt, het markeert het bericht worden gebruikt en terug naar de toepassing. **ReceiveAndDelete** modus is de eenvoudigste en meest geschikt voor scenario's waarin een toepassing kan tolereren niet verwerken van een bericht bij een storing. Om dit te begrijpen, kunt u overwegen een scenario waarin de consument ontvangen verzoek verzendt en vervolgens loopt vast voordat deze worden verwerkt. Omdat Service Bus wordt het bericht als het wordt verbruikt, gemarkeerd en vervolgens, wanneer de toepassing wordt opnieuw opgestart en begint berichten opnieuw te gebruiken, deze zal hebt gemist het bericht dat is verbruikt vóór de crash.

In de **PeekLock** modus wordt ontvangen van een bericht een bewerking twee fase, die het mogelijk maakt voor van ondersteuningstoepassingen die ontbrekende berichten niet kunnen tolereren. Bus-Service een aanvraag ontvangt, deze vindt u het volgende bericht wordt verbruikt, wordt vergrendeld om te voorkomen dat andere consumenten ontvangen als geeft als resultaat de toepassing. Nadat de toepassing is het bericht verwerkt (of betrouwbaar voor toekomstige verwerking opgeslagen), wordt de tweede fase van de procedure ontvangen door het ontvangen bericht wordt doorgegeven aan **ServiceBusRestProxy -> deleteMessage**voltooid. Wanneer Service Bus **deleteMessage** aanroept, wordt het markeren van het bericht worden gebruikt en uit de wachtrij verwijderen.

In het volgende voorbeeld ziet u hoe ontvangen en verwerken van een bericht met behulp van **PeekLock** -modus (niet de standaardmodus). 

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\ReceiveMessageOptions;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
        
try {
    // Set receive mode to PeekLock (default is ReceiveAndDelete)
    $options = new ReceiveMessageOptions();
    $options->setPeekLock();
    
    // Get message.
    $message = $serviceBusRestProxy->receiveSubscriptionMessage("mytopic", "mysubscription", $options);

    echo "Body: ".$message->getBody()."<br />";
    echo "MessageID: ".$message->getMessageId()."<br />";
        
    /*---------------------------
        Process message here.
    ----------------------------*/
        
    // Delete message. Not necessary if peek lock is not set.
    echo "Deleting message...<br />";
    $serviceBusRestProxy->deleteMessage($message);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/hh780735
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Procedure: toepassing crashes en onleesbare berichten verwerken

Bus service biedt functies waarmee u zonder problemen herstellen van fouten in uw toepassing of moeilijkheden verwerken van een bericht. Als een ontvanger het bericht verwerkt voor een of andere reden niet is, kan deze de methode **unlockMessage** aanroepen voor het ontvangen bericht (in plaats van de methode **deleteMessage** ). Hierdoor wordt de Service Bus te ontgrendelen van het bericht in de wachtrij en beschikbaar stellen aan opnieuw worden ontvangen door de toepassing in beslag nemen of door een andere toepassing voor in beslag nemen.

Er is ook een time-out is gekoppeld aan een bericht in de wachtrij is vergrendeld en als de aanvraag niet verwerken het bericht voordat u de vergrendeling time-out is verstreken (bijvoorbeeld, als de toepassing loopt vast), vervolgens Service Bus wordt het bericht automatisch ontgrendelen en beschikbaar stellen aan opnieuw worden ontvangen.

In het geval dat de toepassing vastloopt nadat het bericht is verwerkt, maar voordat de **deleteMessage** -aanvraag wordt ingediend, zal vervolgens het bericht worden geleverd aan de toepassing opnieuw wordt gestart. Dit wordt vaak genoemd **Tenminste één keer verwerken**; elk bericht wordt verwerkt, ten minste één keer maar in bepaalde situaties kan hetzelfde bericht worden geleverd. Als het scenario kan niet dubbele verwerking tolereren, kunnen ontwikkelaars van toepassingen moeten extra logica toevoegen aan toepassingen aflevering van dubbele berichten verwerken. Dit wordt vaak bereikt met behulp van de methode **getMessageId** van het bericht over zelf constant blijft.

## <a name="delete-topics-and-subscriptions"></a>Onderwerpen en abonnementen verwijderen

Als u wilt verwijderen van een onderwerp of een abonnement, de **ServiceBusRestProxy -> deleteTopic** of de **ServiceBusRestProxy -> deleteSubscripton** -methoden, respectievelijk gebruiken. Houd er rekening mee dat alle abonnementen die zijn geregistreerd bij het onderwerp als u een onderwerp verwijdert ook verwijderd.

In het volgende voorbeeld ziet u hoe te verwijderen van een onderwerp met de naam `mytopic` en de geregistreerde abonnementen.

```
require_once 'vendor/autoload.php';

use WindowsAzure\ServiceBus\ServiceBusService;
use WindowsAzure\ServiceBus\ServiceBusSettings;
use WindowsAzure\Common\ServiceException;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
    
try {       
    // Delete topic.
    $serviceBusRestProxy->deleteTopic("mytopic");
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here: 
    // http://msdn.microsoft.com/library/azure/dd179357
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

U kunt een abonnement afzonderlijk verwijderen met behulp van de methode **deleteSubscription** :

```
$serviceBusRestProxy->deleteSubscription("mytopic", "mysubscription");
```

## <a name="next-steps"></a>Volgende stappen

Zie u hebt geleerd dat de grondbeginselen van Service Bus wachtrijen, [wachtrijen, onderwerpen, en abonnementen][] voor meer informatie.

[Wachtrijen, onderwerpen en abonnementen]: service-bus-queues-topics-subscriptions.md
[sqlfilter]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
[require-once]: http://php.net/require_once
[Quota Service Bus]: service-bus-quotas.md
