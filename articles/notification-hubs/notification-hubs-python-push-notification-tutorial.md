<properties 
    pageTitle="Het gebruik van Hubs kennisgeving met Python" 
    description="Leren werken met Azure melding Hubs vanuit een back-end Python." 
    services="notification-hubs" 
    documentationCenter="" 
    authors="ysxu"
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="notification-hubs" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="python" 
    ms.devlang="php" 
    ms.topic="article" 
    ms.date="06/29/2016" 
    ms.author="yuaxu"/>

# <a name="how-to-use-notification-hubs-from-python"></a>Het gebruik van Hubs melding van Python
[AZURE.INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]
        
Toegang tot alle functies van de kennisgeving Hubs vanuit een Java/PHP/Python/Ruby back-end de interface melding Hub REST zoals beschreven in het onderwerp MSDN [Melding Hubs REST API's](http://msdn.microsoft.com/library/dn223264.aspx).

> [AZURE.NOTE] Dit is een voorbeeld referentie implementatie voor de uitvoering van de melding verzendt in Python en is niet de overheid gesteunde meldingen Hub Python SDK.
>
> In dit voorbeeld is geschreven met Python 3.4.

In dit onderwerp wordt gedemonstreerd hoe u:

* Bouw een REST-client voor kennisgeving Hubs functies in Python.
* Met behulp van Python interface voor de REST API's voor melding Hub meldingen verzenden. 
* Krijg een dump van de REST van de HTTP-aanvraag/reactie voor debugging/educatieve doeleinden. 

U kunt volgen de [gestarte zelfstudie ophalen](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) voor uw mobiele platform van keuze, het gedeelte back-end implementatie in Python.

> [AZURE.NOTE] De omvang van het monster wordt alleen beperkt om meldingen te verzenden en het beheer van de registratie niet.

## <a name="client-interface"></a>Clientinterface
De hoofdprinter van client-interface bieden dezelfde methoden die beschikbaar in de [SDK voor .NET melding Hubs zijn](http://msdn.microsoft.com/library/jj933431.aspx). Dit kunt u de zelfstudies en monsters die momenteel beschikbaar op deze site direct te vertalen en de bijdrage van de Gemeenschap op het internet.

Hier vindt u alle code die in de [REST van Python wrapper monster]beschikbaar.

Bijvoorbeeld voor het maken van een client:

    isDebug = True
    hub = NotificationHub("myConnectionString", "myNotificationHubName", isDebug)
    
Een toast Windows bericht verzenden:
    
    wns_payload = """<toast><visual><binding template=\"ToastText01\"><text id=\"1\">Hello world!</text></binding></visual></toast>"""
    hub.send_windows_notification(wns_payload)
    
## <a name="implementation"></a>Implementatie
Als u nog niet hebt gedaan, voert u onze [Get begonnen zelfstudie] tot de laatste sectie waar u hebt voor de uitvoering van de back-end.

Op [MSDN](http://msdn.microsoft.com/library/dn530746.aspx)vindt u de details voor het implementeren van een volledige REST wrapper. In deze sectie worden beschreven de Python implementatie van de belangrijkste stappen die nodig zijn voor toegang tot kennisgeving Hubs REST eindpunten en meldingen verzenden

1. De verbindingsreeks parseren
2. De autorisatietoken genereren
3. Een bericht met behulp van HTTP-REST API verzenden

### <a name="parse-the-connection-string"></a>De verbindingsreeks parseren

Dit is de belangrijkste klasse implementeren van de client, waarvan de constructor de verbindingsreeks parseert:

    class NotificationHub:
        API_VERSION = "?api-version=2013-10"
        DEBUG_SEND = "&test"
    
        def __init__(self, connection_string=None, hub_name=None, debug=0):
            self.HubName = hub_name
            self.Debug = debug
    
            # Parse connection string
            parts = connection_string.split(';')
            if len(parts) != 3:
                raise Exception("Invalid ConnectionString.")
    
            for part in parts:
                if part.startswith('Endpoint'):
                    self.Endpoint = 'https' + part[11:]
                if part.startswith('SharedAccessKeyName'):
                    self.SasKeyName = part[20:]
                if part.startswith('SharedAccessKey'):
                    self.SasKeyValue = part[16:]


### <a name="create-security-token"></a>Beveiligingssleutel maken
De details van het maken van security-token beschikbaar zijn [hier](http://msdn.microsoft.com/library/dn495627.aspx).
De volgende methoden moeten worden toegevoegd aan de klasse **NotificationHub** te maken van het token op basis van de URI van de huidige aanvraag en de referenties uit de verbindingsreeks hebt uitgepakt.

    @staticmethod
    def get_expiry():
        # By default returns an expiration of 5 minutes (=300 seconds) from now
        return int(round(time.time() + 300))

    @staticmethod
    def encode_base64(data):
        return base64.b64encode(data)

    def sign_string(self, to_sign):
        key = self.SasKeyValue.encode('utf-8')
        to_sign = to_sign.encode('utf-8')
        signed_hmac_sha256 = hmac.HMAC(key, to_sign, hashlib.sha256)
        digest = signed_hmac_sha256.digest()
        encoded_digest = self.encode_base64(digest)
        return encoded_digest

    def generate_sas_token(self):
        target_uri = self.Endpoint + self.HubName
        my_uri = urllib.parse.quote(target_uri, '').lower()
        expiry = str(self.get_expiry())
        to_sign = my_uri + '\n' + expiry
        signature = urllib.parse.quote(self.sign_string(to_sign))
        auth_format = 'SharedAccessSignature sig={0}&se={1}&skn={2}&sr={3}'
        sas_token = auth_format.format(signature, expiry, self.SasKeyName, my_uri)
        return sas_token

### <a name="send-a-notification-using-http-rest-api"></a>Een bericht met behulp van HTTP-REST API verzenden
Eerste, laat gebruik definieert een klasse die een melding.

    class Notification:
        def __init__(self, notification_format=None, payload=None, debug=0):
            valid_formats = ['template', 'apple', 'gcm', 'windows', 'windowsphone', "adm", "baidu"]
            if not any(x in notification_format for x in valid_formats):
                raise Exception(
                    "Invalid Notification format. " +
                    "Must be one of the following - 'template', 'apple', 'gcm', 'windows', 'windowsphone', 'adm', 'baidu'")
    
            self.format = notification_format
            self.payload = payload
    
            # array with keynames for headers
            # Note: Some headers are mandatory: Windows: X-WNS-Type, WindowsPhone: X-NotificationType
            # Note: For Apple you can set Expiry with header: ServiceBusNotification-ApnsExpiry
            # in W3C DTF, YYYY-MM-DDThh:mmTZD (for example, 1997-07-16T19:20+01:00).
            self.headers = None

Deze klasse is een container voor een instantie van de oorspronkelijke kennisgeving of een set met eigenschappen in het geval van een melding van de sjabloon, set headers met (native platform of sjabloon) en platform-specifieke eigenschappen (zoals Apple vervaldatum eigenschap en kopteksten WNS).

Raadpleeg de [documentatie melding Hubs REST API's](http://msdn.microsoft.com/library/dn495827.aspx) en specifieke mededeling platforms indelingen voor de beschikbare opties.

Nu met deze klasse, kunnen we schrijven de verzenden methoden in de klasse **NotificationHub** .

    def make_http_request(self, url, payload, headers):
        parsed_url = urllib.parse.urlparse(url)
        connection = http.client.HTTPSConnection(parsed_url.hostname, parsed_url.port)

        if self.Debug > 0:
            connection.set_debuglevel(self.Debug)
            # adding this querystring parameter gets detailed information about the PNS send notification outcome
            url += self.DEBUG_SEND
            print("--- REQUEST ---")
            print("URI: " + url)
            print("Headers: " + json.dumps(headers, sort_keys=True, indent=4, separators=(' ', ': ')))
            print("--- END REQUEST ---\n")

        connection.request('POST', url, payload, headers)
        response = connection.getresponse()

        if self.Debug > 0:
            # print out detailed response information for debugging purpose
            print("\n\n--- RESPONSE ---")
            print(str(response.status) + " " + response.reason)
            print(response.msg)
            print(response.read())
            print("--- END RESPONSE ---")

        elif response.status != 201:
            # Successful outcome of send message is HTTP 201 - Created
            raise Exception(
                "Error sending notification. Received HTTP code " + str(response.status) + " " + response.reason)

        connection.close()

    def send_notification(self, notification, tag_or_tag_expression=None):
        url = self.Endpoint + self.HubName + '/messages' + self.API_VERSION

        json_platforms = ['template', 'apple', 'gcm', 'adm', 'baidu']

        if any(x in notification.format for x in json_platforms):
            content_type = "application/json"
            payload_to_send = json.dumps(notification.payload)
        else:
            content_type = "application/xml"
            payload_to_send = notification.payload

        headers = {
            'Content-type': content_type,
            'Authorization': self.generate_sas_token(),
            'ServiceBusNotification-Format': notification.format
        }

        if isinstance(tag_or_tag_expression, set):
            tag_list = ' || '.join(tag_or_tag_expression)
        else:
            tag_list = tag_or_tag_expression

        # add the tags/tag expressions to the headers collection
        if tag_list != "":
            headers.update({'ServiceBusNotification-Tags': tag_list})

        # add any custom headers to the headers collection that the user may have added
        if notification.headers is not None:
            headers.update(notification.headers)

        self.make_http_request(url, payload_to_send, headers)

    def send_apple_notification(self, payload, tags=""):
        nh = Notification("apple", payload)
        self.send_notification(nh, tags)

    def send_gcm_notification(self, payload, tags=""):
        nh = Notification("gcm", payload)
        self.send_notification(nh, tags)

    def send_adm_notification(self, payload, tags=""):
        nh = Notification("adm", payload)
        self.send_notification(nh, tags)

    def send_baidu_notification(self, payload, tags=""):
        nh = Notification("baidu", payload)
        self.send_notification(nh, tags)

    def send_mpns_notification(self, payload, tags=""):
        nh = Notification("windowsphone", payload)

        if "<wp:Toast>" in payload:
            nh.headers = {'X-WindowsPhone-Target': 'toast', 'X-NotificationClass': '2'}
        elif "<wp:Tile>" in payload:
            nh.headers = {'X-WindowsPhone-Target': 'tile', 'X-NotificationClass': '1'}

        self.send_notification(nh, tags)

    def send_windows_notification(self, payload, tags=""):
        nh = Notification("windows", payload)

        if "<toast>" in payload:
            nh.headers = {'X-WNS-Type': 'wns/toast'}
        elif "<tile>" in payload:
            nh.headers = {'X-WNS-Type': 'wns/tile'}
        elif "<badge>" in payload:
            nh.headers = {'X-WNS-Type': 'wns/badge'}

        self.send_notification(nh, tags)

    def send_template_notification(self, properties, tags=""):
        nh = Notification("template", properties)
        self.send_notification(nh, tags)

De bovenstaande manieren verzenden een HTTP POST-aanvraag en het eindpunt van de /messages van uw melding hub, met de juiste hoofdtekst en koptekst om de melding te verzenden.

### <a name="using-debug-property-to-enable-detailed-logging"></a>Met behulp van de eigenschap debug gedetailleerde logboekregistratie inschakelen
Eigenschap debug inschakelen tijdens het initialiseren van de Hub kennisgeving wordt gedetailleerde registratie-informatie over het HTTP-verzoek en antwoord dump, alsmede een gedetailleerde melding verzenden uitkomst. We hebben deze eigenschap met de naam van de [eigenschap melding Hubs TestZenden](http://msdn.microsoft.com/library/microsoft.servicebus.notifications.notificationhubclient.enabletestsend.aspx) die resulteert in gedetailleerde informatie over de uitkomst van de kennisgeving verzenden onlangs toegevoegd. Voor het gebruik van deze - initialiseren met behulp van de volgende opties:

    hub = NotificationHub("myConnectionString", "myNotificationHubName", isDebug)

Hub verzonden verzoek HTTP URL wordt daarom met een querystring "test" toegevoegd. 

##<a name="complete-tutorial"></a>Voltooi de zelfstudie
U kunt de zelfstudie aan de slag nu voltooien door het verzenden van de kennisgeving van een Python back-end.

Initialiseren van uw melding Hubs client (vervangende tekenreeks en hub naam van de verbinding als de gebruiksaanwijzing in de [gestarte zelfstudie Get]):

    hub = NotificationHub("myConnectionString", "myNotificationHubName")

Voeg vervolgens de code verzenden afhankelijk van uw doel mobiel platform. In dit voorbeeld voegt ook hogere niveau methoden voor het verzenden van meldingen op basis van het platform bijvoorbeeld send_windows_notification voor windows; inschakelen send_apple_notification (voor apple) enz. 

### <a name="windows-store-and-windows-phone-81-non-silverlight"></a>Windows Store en Windows Phone 8.1 (niet-Silverlight)

    wns_payload = """<toast><visual><binding template=\"ToastText01\"><text id=\"1\">Test</text></binding></visual></toast>"""
    hub.send_windows_notification(wns_payload)

### <a name="windows-phone-80-and-81-silverlight"></a>Windows Phone 8.0 en 8.1 Silverlight

    hub.send_mpns_notification(toast)

### <a name="ios"></a>iOS

    alert_payload = {
        'data':
            {
                'msg': 'Hello!'
            }
    }
    hub.send_apple_notification(alert_payload)

### <a name="android"></a>Android
    gcm_payload = {
        'data':
            {
                'msg': 'Hello!'
            }
    }
    hub.send_gcm_notification(gcm_payload)

### <a name="kindle-fire"></a>Kindle Fire
    adm_payload = {
        'data':
            {
                'msg': 'Hello!'
            }
    }
    hub.send_adm_notification(adm_payload)

### <a name="baidu"></a>Baidu
    baidu_payload = {
        'data':
            {
                'msg': 'Hello!'
            }
    }
    hub.send_baidu_notification(baidu_payload)

De Python-code wordt uitgevoerd, moet een melding die wordt weergegeven op het doelapparaat produceren.

## <a name="examples"></a>Voorbeelden:

### <a name="enabling-debug-property"></a>Eigenschap voor foutopsporing inschakelen
Wanneer u de markering foutopsporing inschakelt tijdens het initialiseren van de NotificationHub ziet u gedetailleerde HTTP-aanvraag en respons dump, alsmede NotificationOutcome zoals hieronder waar u inzicht in welke HTTP-headers worden doorgegeven in de aanvraag en welke HTTP-antwoord van de Hub van de kennisgeving is ontvangen:    ![][1]

Ziet u gedetailleerde kennisgeving Hub resultaat bv. 

- Wanneer het bericht wordt verzonden naar de Push Notification Service. 
    
        <Outcome>The Notification was successfully sent to the Push Notification System</Outcome>

- Als er geen doelen gevonden voor een push-bericht vervolgens gaat u waarschijnlijk het ziet er in het antwoord (waarmee wordt aangegeven dat er geen registraties gevonden voor de kennisgeving waarschijnlijk omdat de registraties had enkele niet-overeenkomende codes zijn)

        '<NotificationOutcome xmlns="http://schemas.microsoft.com/netservices/2010/10/servicebus/connect" xmlns:i="http://www.w3.org/2001/XMLSchema-instance"><Success>0</Success><Failure>0</Failure><Results i:nil="true"/></NotificationOutcome>'

### <a name="broadcast-toast-notification-to-windows"></a>Uitzending toast aanmelding bij Windows 

U ziet de koppen die verzonden u wanneer u toast broadcast-bericht naar een Windows-client verzendt. 

    hub.send_windows_notification(wns_payload)

![][2]

### <a name="send-notification-specifying-a-tag-or-tag-expression"></a>Een label (of de labelexpressie) melding verzenden

U ziet de Tags HTTP-header die wordt toegevoegd aan de HTTP-aanvraag (in het onderstaande voorbeeld we zijn gemeld dat het bericht alleen voor registraties met 'sports' payload)

    hub.send_windows_notification(wns_payload, "sports")

![][3]

### <a name="send-notification-specifying-multiple-tags"></a>Meerdere labels opgeven melding verzenden

Ziet u hoe de header HTTP-codes gewijzigd wanneer meerdere tags worden verzonden. 
    
    tags = {'sports', 'politics'}
    hub.send_windows_notification(wns_payload, tags)

![][4]

### <a name="templated-notification"></a>Kennisgeving van sjablonen

U ziet dat de wijzigingen indeling http-header en de nettolading instantie als onderdeel van het hoofdgedeelte van de HTTP-aanvraag wordt verzonden:

**Client-side - geregistreerde sjabloon**

        var template =
                        @"<toast><visual><binding template=""ToastText01""><text id=""1"">$(greeting_en)</text></binding></visual></toast>";
            
**Server-side - de nettolading verzenden**
        
        template_payload = {'greeting_en': 'Hello', 'greeting_fr': 'Salut'}
        hub.send_template_notification(template_payload)

![][5]


## <a name="next-steps"></a>Volgende stappen
In dit onderwerp bleek we het maken van een eenvoudige client Python REST voor kennisgeving Hubs. Hier kunt u het volgende doen:

* Download de volledige [REST Python wrapper monster]dat de bovenstaande code bevat.
* Blijven leren over melding Hubs tagging functie in de [zelfstudie breken nieuws]
* Meer informatie over sjablonen voor melding Hubs functie blijven in de [zelfstudie lokaliseren nieuws]

<!-- URLs -->
[REST Python wrapper monster]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/notificationhubs-rest-python
[Get begonnen zelfstudie]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-get-started/
[Overtredingen van de zelfstudie voor nieuws]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-send-breaking-news/
[Nieuws-zelfstudie lokaliseren]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-send-localized-breaking-news/

<!-- Images. -->
[1]: ./media/notification-hubs-python-backend-how-to/DetailedLoggingInfo.png
[2]: ./media/notification-hubs-python-backend-how-to/BroadcastScenario.png
[3]: ./media/notification-hubs-python-backend-how-to/SendWithOneTag.png
[4]: ./media/notification-hubs-python-backend-how-to/SendWithMultipleTags.png
[5]: ./media/notification-hubs-python-backend-how-to/TemplatedNotification.png
 
