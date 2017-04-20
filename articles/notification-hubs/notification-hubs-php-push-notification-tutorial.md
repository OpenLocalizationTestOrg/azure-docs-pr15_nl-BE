<properties 
    pageTitle="Het gebruik van Hubs kennisgeving met PHP" 
    description="Leren werken met Azure melding Hubs vanuit een back-end PHP." 
    services="notification-hubs" 
    documentationCenter="" 
    authors="ysxu" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="notification-hubs" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="php" 
    ms.devlang="php" 
    ms.topic="article" 
    ms.date="06/07/2016" 
    ms.author="yuaxu"/>

# <a name="how-to-use-notification-hubs-from-php"></a>Het gebruik van Hubs melding van PHP
[AZURE.INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]

Toegang tot alle functies van de kennisgeving Hubs vanuit een PHP-Java/Ruby back-end de interface melding Hub REST zoals beschreven in het onderwerp MSDN [Melding Hubs REST API's](http://msdn.microsoft.com/library/dn223264.aspx).

In dit onderwerp wordt gedemonstreerd hoe u:

* Een andere client voor kennisgeving Hubs functies in PHP;
* Volg de [gestarte zelfstudie ophalen](notification-hubs-ios-apple-push-notification-apns-get-started.md) voor uw mobiele platform van keuze, het gedeelte back-end implementatie in PHP.

## <a name="client-interface"></a>Clientinterface
De hoofdprinter van client-interface biedt dezelfde methoden die beschikbaar in de [SDK voor .NET melding Hubs zijn](http://msdn.microsoft.com/library/jj933431.aspx), Hierdoor kunt u rechtstreeks vertalen de zelfstudies en monsters die momenteel beschikbaar op deze site, en de bijdrage van de Gemeenschap op het internet.

Hier vindt u alle code in de [PHP REST wrapper monster]beschikbaar.

Bijvoorbeeld voor het maken van een client:

    $hub = new NotificationHub("connection string", "hubname"); 

Een iOS native bericht verzenden:
    
    $notification = new Notification("apple", '{"aps":{"alert": "Hello!"}}');
    $hub->sendNotification($notification, null);

## <a name="implementation"></a>Implementatie
Als u nog niet hebt gedaan, voert u onze [Get begonnen zelfstudie] tot de laatste sectie waar u hebt voor de uitvoering van de back-end.
Ook als u wilt dat kunt u de code van de [PHP REST wrapper monster] en Ga rechtstreeks naar de sectie [voltooid de zelfstudie](#complete-tutorial) .

Op [MSDN](http://msdn.microsoft.com/library/dn530746.aspx)vindt u de details voor het implementeren van een volledige REST wrapper. In deze sectie wordt de PHP-implementatie van de belangrijkste stappen die zijn vereist voor toegang tot kennisgeving Hubs REST eindpunten worden beschreven:

1. De verbindingsreeks parseren
2. De autorisatietoken genereren
3. De HTTP-oproep uitvoeren

### <a name="parse-the-connection-string"></a>De verbindingsreeks parseren

Dit is de belangrijkste klasse implementeren van de client, waarvan constructor die de verbindingsreeks parseert:

    class NotificationHub {
        const API_VERSION = "?api-version=2013-10";
    
        private $endpoint;
        private $hubPath;
        private $sasKeyName;
        private $sasKeyValue;
    
        function __construct($connectionString, $hubPath) {
            $this->hubPath = $hubPath;
    
            $this->parseConnectionString($connectionString);
        }
    
        private function parseConnectionString($connectionString) {
            $parts = explode(";", $connectionString);
            if (sizeof($parts) != 3) {
                throw new Exception("Error parsing connection string: " . $connectionString);
            }
    
            foreach ($parts as $part) {
                if (strpos($part, "Endpoint") === 0) {
                    $this->endpoint = "https" . substr($part, 11);
                } else if (strpos($part, "SharedAccessKeyName") === 0) {
                    $this->sasKeyName = substr($part, 20);
                } else if (strpos($part, "SharedAccessKey") === 0) {
                    $this->sasKeyValue = substr($part, 16);
                }
            }
        }
    }


### <a name="create-security-token"></a>Beveiligingssleutel maken
De details van het maken van security-token beschikbaar zijn [hier](http://msdn.microsoft.com/library/dn495627.aspx).
De volgende methode moet worden toegevoegd aan de klasse **NotificationHub** te maken van het token op basis van de URI van de huidige aanvraag en de referenties uit de verbindingsreeks hebt uitgepakt.

    private function generateSasToken($uri) {
        $targetUri = strtolower(rawurlencode(strtolower($uri)));

        $expires = time();
        $expiresInMins = 60;
        $expires = $expires + $expiresInMins * 60;
        $toSign = $targetUri . "\n" . $expires;

        $signature = rawurlencode(base64_encode(hash_hmac('sha256', $toSign, $this->sasKeyValue, TRUE)));

        $token = "SharedAccessSignature sr=" . $targetUri . "&sig="
                    . $signature . "&se=" . $expires . "&skn=" . $this->sasKeyName;

        return $token;
    }

### <a name="send-a-notification"></a>Een bericht verzenden
Eerst definiëren we een klasse die een melding.

    class Notification {
        public $format;
        public $payload;
    
        # array with keynames for headers
        # Note: Some headers are mandatory: Windows: X-WNS-Type, WindowsPhone: X-NotificationType
        # Note: For Apple you can set Expiry with header: ServiceBusNotification-ApnsExpiry in W3C DTF, YYYY-MM-DDThh:mmTZD (for example, 1997-07-16T19:20+01:00).
        public $headers;
    
        function __construct($format, $payload) {
            if (!in_array($format, ["template", "apple", "windows", "gcm", "windowsphone"])) {
                throw new Exception('Invalid format: ' . $format);
            }
    
            $this->format = $format;
            $this->payload = $payload;
        }
    }

Deze klasse is een container voor een instantie van de oorspronkelijke kennisgeving, of een set eigenschappen op het geval van een melding van de sjabloon en een aantal koppen met de indeling (native platform of sjabloon) en platform-specifieke eigenschappen (zoals Apple vervaldatum eigenschap en kopteksten WNS).

Raadpleeg de [documentatie melding Hubs REST API's](http://msdn.microsoft.com/library/dn495827.aspx) en specifieke mededeling platforms indelingen voor de beschikbare opties.

Gewapend met deze klasse, kunnen we nu schrijven verzenden methoden in de klasse **NotificationHub** .

    public function sendNotification($notification, $tagsOrTagExpression="") {
        if (is_array($tagsOrTagExpression)) {
            $tagExpression = implode(" || ", $tagsOrTagExpression);
        } else {
            $tagExpression = $tagsOrTagExpression;
        }

        # build uri
        $uri = $this->endpoint . $this->hubPath . "/messages" . NotificationHub::API_VERSION;
        $ch = curl_init($uri);

        if (in_array($notification->format, ["template", "apple", "gcm"])) {
            $contentType = "application/json";
        } else {
            $contentType = "application/xml";
        }

        $token = $this->generateSasToken($uri);

        $headers = [
            'Authorization: '.$token,
            'Content-Type: '.$contentType,
            'ServiceBusNotification-Format: '.$notification->format
        ];

        if ("" !== $tagExpression) {
            $headers[] = 'ServiceBusNotification-Tags: '.$tagExpression;
        }

        # add headers for other platforms
        if (is_array($notification->headers)) {
            $headers = array_merge($headers, $notification->headers);
        }
        
        curl_setopt_array($ch, array(
            CURLOPT_POST => TRUE,
            CURLOPT_RETURNTRANSFER => TRUE,
            CURLOPT_SSL_VERIFYPEER => FALSE,
            CURLOPT_HTTPHEADER => $headers,
            CURLOPT_POSTFIELDS => $notification->payload
        ));

        // Send the request
        $response = curl_exec($ch);

        // Check for errors
        if($response === FALSE){
            throw new Exception(curl_error($ch));
        }

        $info = curl_getinfo($ch);

        if ($info['http_code'] <> 201) {
            throw new Exception('Error sending notificaiton: '. $info['http_code'] . ' msg: ' . $response);
        }
    } 

De bovenstaande manieren verzenden een HTTP POST-aanvraag en het eindpunt van de /messages van uw melding hub, met de juiste hoofdtekst en koptekst om de melding te verzenden.

##<a name="complete-tutorial"></a>Voltooi de zelfstudie
U kunt de zelfstudie aan de slag nu voltooien door het verzenden van de kennisgeving van een PHP-back-end.

Initialiseren van uw melding Hubs client (vervangende tekenreeks en hub naam van de verbinding als de gebruiksaanwijzing in de [gestarte zelfstudie Get]):

    $hub = new NotificationHub("connection string", "hubname"); 

Voeg vervolgens de code verzenden afhankelijk van uw doel mobiel platform.

### <a name="windows-store-and-windows-phone-81-non-silverlight"></a>Windows Store en Windows Phone 8.1 (niet-Silverlight)

    $toast = '<toast><visual><binding template="ToastText01"><text id="1">Hello from PHP!</text></binding></visual></toast>';
    $notification = new Notification("windows", $toast);
    $notification->headers[] = 'X-WNS-Type: wns/toast';
    $hub->sendNotification($notification, null);

### <a name="ios"></a>iOS

    $alert = '{"aps":{"alert":"Hello from PHP!"}}';
    $notification = new Notification("apple", $alert);
    $hub->sendNotification($notification, null);

### <a name="android"></a>Android
    $message = '{"data":{"msg":"Hello from PHP!"}}';
    $notification = new Notification("gcm", $message);
    $hub->sendNotification($notification, null);

### <a name="windows-phone-80-and-81-silverlight"></a>Windows Phone 8.0 en 8.1 Silverlight

    $toast = '<?xml version="1.0" encoding="utf-8"?>' .
                '<wp:Notification xmlns:wp="WPNotification">' .
                   '<wp:Toast>' .
                        '<wp:Text1>Hello from PHP!</wp:Text1>' .
                   '</wp:Toast> ' .
                '</wp:Notification>';
    $notification = new Notification("windowsphone", $toast);
    $notification->headers[] = 'X-WindowsPhone-Target : toast';
    $notification->headers[] = 'X-NotificationClass : 2';
    $hub->sendNotification($notification, null);


### <a name="kindle-fire"></a>Kindle Fire
    $message = '{"data":{"msg":"Hello from PHP!"}}';
    $notification = new Notification("adm", $message);
    $hub->sendNotification($notification, null);

Uitvoeren van uw PHP-code moet produceren nu een melding die wordt weergegeven op het doelapparaat.


## <a name="next-steps"></a>Volgende stappen
In dit onderwerp bleek we het maken van een eenvoudige client Java REST voor kennisgeving Hubs. Hier kunt u het volgende doen:

* Download de volledige [PHP REST wrapper monster]dat de bovenstaande code bevat.
* Blijven leren over melding Hubs tagging functie in [nieuws breken zelfstudie]
* Meer informatie over meldingen voor afzonderlijke gebruikers duwen in [zelfstudie gebruikers waarschuwen]

Zie ook de [PHP Developer Center](/develop/php/)voor meer informatie.

[PHP REST wrapper monster]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/notificationhubs-rest-php
[Get begonnen zelfstudie]: http://azure.microsoft.com/documentation/articles/notification-hubs-ios-get-started/
 
