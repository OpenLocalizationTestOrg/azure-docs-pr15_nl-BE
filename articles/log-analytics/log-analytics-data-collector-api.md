<properties
    pageTitle="Meld u Analytics HTTP gegevensverzamelaarset API | Microsoft Azure"
    description="Logboek Analytics HTTP-gegevens verzamelen API kunt u boeken JSON-gegevens toevoegen aan de opslagplaats Analytics logboek van alle clients die de REST API kunt aanroepen. In dit artikel wordt beschreven hoe u de API en bevat voorbeelden van hoe u de gegevens publiceren met behulp van verschillende programmeertalen."
    services="log-analytics"
    documentationCenter=""
    authors="bwren"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/26/2016"
    ms.author="bwren"/>


# <a name="log-analytics-http-data-collector-api"></a>Meld u Analytics HTTP gegevensverzamelaarset API

Wanneer u de API Azure logboek Analytics HTTP-gegevens verzamelen, kunt u POST JavaScript Object Notation (JSON) gegevens toevoegen aan de opslagplaats Analytics logboek van alle clients die de REST API kunt aanroepen. Met deze methode kunt u gegevens van toepassingen van andere leveranciers of van scripts, zoals uit een runbook in Azure automatisering verzenden.  

## <a name="create-a-request"></a>Een aanvraag maken

De volgende twee tabellen bevatten de kenmerken die vereist voor elke aanvraag naar de API logboek Analytics HTTP-gegevens verzamelen zijn. Elk kenmerk later in het artikel uitvoeriger beschreven.

### <a name="request-uri"></a>Aanvraag URI

| Kenmerk | Eigenschap |
|:--|:--|
| Methode | Verzenden |
| URI | https://\<KlantId\>.ods.opinsights.azure.com/api/logs?api-version=2016-04-01 |
| Type inhoud | Application/json |

### <a name="request-uri-parameters"></a>Parameters van de aanvraag-URI
| Parameter | Beschrijving |
|:--|:--|
| Klant-id  | De unieke id voor de werkruimte van Microsoft Operations Management Suite. |
| Resource    | De naam van de resource API: api/logs. |
| API-versie | De versie van de API te gebruiken met dit verzoek. Het is momenteel 04-01-2016. |

### <a name="request-headers"></a>Aanvraagheaders
| Koptekst | Beschrijving |
|:--|:--|
| Autorisatie | De handtekening van de vergunning. Later in dit artikel, kunt u lezen over het maken van een HMAC-SHA256-header. |
| Logboek-Type | Geef het recordtype van de gegevens die wordt verzonden. Het logboektype ondersteunt op dit moment alleen alfanumerieke tekens. Numerieke waarden of speciale tekens worden niet ondersteund. |
| x-ms-datum | De datum waarop de aanvraag is verwerkt, in de indeling van RFC 1123. |
| tijd-gegenereerde veld | De naam van een veld in de gegevens die de tijdstempel van het gegevensitem bevat. Als u een veld en vervolgens de inhoud ervan worden gebruikt voor **TimeGenerated**. Als u dit veld niet opgeeft, is de standaardwaarde voor **TimeGenerated** de tijd waarop het bericht is ingenomen. De inhoud van het berichtenveld moeten volgen de ISO 8601-notatie jjjj-MM-ddTHH. |


## <a name="authorization"></a>Autorisatie

Elk verzoek tot logboek Analytics HTTP-gegevens verzamelen API moet een verificatieheader bevatten. Voor het verifiëren van een aanvraag, moet u zich aanmelden de aanvraag met de primaire of de secundaire sleutel voor de werkruimte die de aanvraag maakt. Vervolgens geeft u die handtekening als onderdeel van de aanvraag.   

Dit is de indeling voor de koptekst van de vergunning:

```
Authorization: SharedKey <WorkspaceID>:<Signature>
```

*WorkspaceID* is de unieke id voor de werkruimte Operations Management Suite. *Handtekening* is een die is gemaakt op basis van de aanvraag en vervolgens wordt berekend met behulp van het [algoritme SHA256](https://msdn.microsoft.com/library/system.security.cryptography.sha256.aspx) [Hash-based Message Authentication Code (HMAC)](https://msdn.microsoft.com/library/system.security.cryptography.hmacsha256.aspx) . Vervolgens, u deze coderen met behulp van Base64-codering.

Deze indeling gebruiken voor het coderen van de tekenreeks **SharedKey** handtekening:

```
StringToSign = VERB + "\n" +
               Content-Length + "\n" +
               Content-Type + "\n" +
               x-ms-date + "\n" +
               "/api/logs";
```

Hier volgt een voorbeeld van een tekenreeks handtekening:

```
POST\n1024\napplication/json\nx-ms-date:Mon, 04 Apr 2016 08:00:00 GMT\n/api/logs
```

Wanneer u de tekenreeks voor de handtekening hebt, deze te coderen met behulp van het algoritme HMAC SHA256 op de tekenreeks UTF-8 gecodeerd en vervolgens het resultaat als Base64 coderen. Gebruik deze indeling:

```
Signature=Base64(HMAC-SHA256(UTF8(StringToSign)))
```

De voorbeelden in de volgende secties hebben voorbeeldcode voor het maken van een verificatieheader.

## <a name="request-body"></a>Hoofdgedeelte van de aanvraag

De hoofdtekst van het bericht moet in JSON. Het omvat een of meer records met de eigenschap naam- en waardeparen in deze indeling:

```
{
"property1": "value1",
" property 2": "value2"
" property 3": "value3",
" property 4": "value4"
}
```

U kunt meerdere records samen in een enkele aanvraag batch met behulp van de volgende indeling. Alle records moet hetzelfde type.

```
{
"property1": "value1",
" property 2": "value2"
" property 3": "value3",
" property 4": "value4"
},
{
"property1": "value1",
" property 2": "value2"
" property 3": "value3",
" property 4": "value4"
}
```

## <a name="record-type-and-properties"></a>Recordtype en eigenschappen

U definieert een aangepaste type bij het indienen van gegevens via de API logboek Analytics HTTP-gegevens verzamelen. Op dit moment niet kan schrijven van gegevens naar bestaande recordtypen die zijn gemaakt door andere gegevenstypen en oplossingen. Logboek Analytics leest de binnenkomende gegevens en maakt vervolgens de eigenschappen die overeenkomen met de gegevenstypen van de waarden die u invoert.

Elke aanvraag voor het logboek Analytics API, moet een **Logboek-Type** -header met de naam voor het recordtype opnemen. Het achtervoegsel **_CL** wordt automatisch toegevoegd aan de naam die u invoert om te onderscheiden van andere typen logboek als een aangepaste logboek. Als u de naam van de **MyNewRecordType**, maakt logboek Analytics u bijvoorbeeld een record van het type **MyNewRecordType_CL**. Dit helpt ervoor te zorgen dat er geen conflicten tussen de gebruiker gedefinieerd typenamen en de meegeleverde in huidige of toekomstige Microsoft-oplossingen.

Voor het gegevenstype van de eigenschap voegt Log Analytics het achtervoegsel aan de naam van de eigenschap. Als een eigenschap een waarde null bevat, wordt de eigenschap niet opgenomen in deze record. Deze tabel bevat de eigenschap gegevenstype en het overeenkomende achtervoegsel:

| De eigenschap gegevenstype | Achtervoegsel |
|:--|:--|
| Tekenreeks    | _K |
| Boole-waarde   | _b |
| Getal met dubbele precisie    | _D |
| Datum/tijd | _Nauw |
| GUID      | _g |


Het gegevenstype dat logboek Analytics wordt gebruikt voor elke eigenschap is afhankelijk van of het recordtype voor de nieuwe record al bestaat.

- Als het type niet bestaat, maakt u Analytics logboek een nieuwe. De JSON-type inference Analytics logboek gebruikt om te bepalen welk gegevenstype voor elke eigenschap van de nieuwe record.
- Als het recordtype bestaat, probeert logboek Analytics een nieuwe record maken op basis van bestaande eigenschappen. Als het gegevenstype voor een eigenschap in de nieuwe record komt niet overeen en kan niet worden geconverteerd naar het type van een bestaande, of als de record een eigenschap die niet bestaat bevat, logboek Analytics een nieuwe eigenschap die het desbetreffende achtervoegsel heeft gemaakt.

Deze post indiening zou bijvoorbeeld een record maken met drie eigenschappen, **number_d**, **boolean_b**en **string_s**:

![Monster record 1](media/log-analytics-data-collector-api/record-01.png)

Als u vervolgens de volgende post ingediend met alle waarden die zijn opgemaakt als tekenreeksen, zou niet de eigenschappen wijzigen. Deze waarden kunnen worden geconverteerd naar bestaande gegevenstypen:

![Monster record 2](media/log-analytics-data-collector-api/record-02.png)

Maar als u vervolgens het volgende indienen, logboek Analytics maakt de nieuwe eigenschappen **boolean_d** en **string_d**. Deze waarden kunnen niet worden geconverteerd:

![Record monster 3](media/log-analytics-data-collector-api/record-03.png)

Als u vervolgens de volgende vermelding ingediend voordat het recordtype is gemaakt, maakt logboek Analytics een record met drie eigenschappen, **aantal-gunstig**, **boolean_s**en **string_s**. In deze post, is elk van de aanvankelijke waarden opgemaakt als een tekenreeks:

![Record monster 4](media/log-analytics-data-collector-api/record-04.png)


## <a name="data-limits"></a>Grenzen van de gegevens
Er zijn enkele beperkingen rond de gegevens die zijn geboekt naar de collectie logboekgegevens Analytics API.

- Maximaal 30 MB per post naar het logboek Analytics Data Collector API. Dit is een maximale grootte voor een enkele post. Als de gegevens uit een enkele die post groter is dan 30 MB, u moet de gegevens tot een kleinere grootte stukken splitsen en tegelijkertijd verzenden. 
- Maximum van de limiet van 32 KB voor de waarden in het veld. Als de waarde van het veld groter is dan 32 KB, worden de gegevens afgekapt. 
- Aanbevolen maximum aantal velden voor een bepaald type is 50. Dit is een praktische beperking van bruikbaarheid en zoeken ervaring perspectief.  


## <a name="return-codes"></a>Retourcodes

De HTTP-statuscode 202 betekent dat de aanvraag is aanvaard voor verwerking, maar de verwerking nog niet voltooid. Dit geeft aan dat de bewerking is voltooid.

Deze tabel bevat de volledige set van statuscodes die de service mogelijk retourneren:

| Code | Status | Foutcode | Beschrijving |
|:--|:--|:--|:--|
| 202 | Geaccepteerd |  | De aanvraag is geaccepteerd. |
| 400 | Ongeldig verzoek | InactiveCustomer | De werkruimte is gesloten. |
| 400 | Ongeldig verzoek | InvalidApiVersion | De API-versie die u hebt opgegeven is niet herkend door de service. |
| 400 | Ongeldig verzoek | InvalidCustomerId | De opgegeven werkruimte-ID is ongeldig. |
| 400 | Ongeldig verzoek | InvalidDataFormat | Ongeldige JSON is ingediend. Het hoofdgedeelte van de response kan bevatten meer informatie over het oplossen van de fout. |
| 400 | Ongeldig verzoek | InvalidLogType | Het logboektype opgegeven die speciale tekens of numerieke waarden. |
| 400 | Ongeldig verzoek | MissingApiVersion | De API-versie is niet opgegeven. |
| 400 | Ongeldig verzoek | MissingContentType | Het type inhoud niet is opgegeven. |
| 400 | Ongeldig verzoek | MissingLogType | Het vereiste Logboektype is niet opgegeven. |
| 400 | Ongeldig verzoek | UnsupportedContentType | Het type inhoud niet is ingesteld op **application/json**. |
| 403 | Verboden | InvalidAuthorization | De service kan voor de verificatie van de aanvraag. Controleer of de werkruimte-ID en verbinding sleutel geldig zijn. |
| 500 | Interne serverfout | UnspecifiedError | De service is een interne fout opgetreden. Probeer de aanvraag opnieuw. |
| 503 | Service is niet beschikbaar | ServiceUnavailable | De service is momenteel niet beschikbaar om aanvragen te ontvangen. Probeer uw aanvraag opnieuw. |

## <a name="query-data"></a>Gegevens opvragen

Ingediend door de API logboek Analytics HTTP-gegevens verzamelen gegevens opvragen, records zoeken met het **Type** is gelijk aan de waarde **LogType** die u hebt opgegeven, met toegevoegde **_CL**. Bijvoorbeeld, als u **MyCustomLog**gebruikt, u zou retourneert alle records met **Type = MyCustomLog_CL**.


## <a name="sample-requests"></a>Monster aanvragen

In de volgende secties vindt u voorbeelden van het indienen van gegevens naar de API logboek Analytics HTTP-gegevens verzamelen met behulp van verschillende programmeertalen.

Voer deze stappen uit om de variabelen ingesteld voor de koptekst van de vergunning voor elk monster:

1. Selecteer de tegel **Instellingen** in de portal Operations Management Suite en selecteer vervolgens het tabblad **Bronnen verbonden** .
2. Rechts van de **Werkruimte-ID**, selecteer het pictogram voor kopiëren en plak de ID als de waarde van de variabele **Klant-ID** .
3. Rechts van de **Primaire sleutel**, selecteert u het pictogram voor kopiëren en plak de ID als de waarde van de variabele **Shared Key** .

U kunt ook de variabelen voor het logboek en de JSON-gegevens wijzigen.

### <a name="powershell-sample"></a>PowerShell voorbeeld

```
# Replace with your Workspace ID
$CustomerId = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"  

# Replace with your Primary Key
$SharedKey = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"

# Specify the name of the record type that you'll be creating
$LogType = "MyRecordType"

# Specify a field with the created time for the records
$TimeStampField = "DateValue"


# Create two records with the same set of properties to create
$json = @"
[{  "StringValue": "MyString1",
    "NumberValue": 42,
    "BooleanValue": true,
    "DateValue": "2016-05-12T20:00:00.625Z",
    "GUIDValue": "9909ED01-A74C-4874-8ABF-D2678E3AE23D"
},
{   "StringValue": "MyString2",
    "NumberValue": 43,
    "BooleanValue": false,
    "DateValue": "2016-05-12T20:00:00.625Z",
    "GUIDValue": "8809ED01-A74C-4874-8ABF-D2678E3AE23D"
}]
"@

# Create the function to create the authorization signature
Function Build-Signature ($customerId, $sharedKey, $date, $contentLength, $method, $contentType, $resource)
{
    $xHeaders = "x-ms-date:" + $date
    $stringToHash = $method + "`n" + $contentLength + "`n" + $contentType + "`n" + $xHeaders + "`n" + $resource

    $bytesToHash = [Text.Encoding]::UTF8.GetBytes($stringToHash)
    $keyBytes = [Convert]::FromBase64String($sharedKey)

    $sha256 = New-Object System.Security.Cryptography.HMACSHA256
    $sha256.Key = $keyBytes
    $calculatedHash = $sha256.ComputeHash($bytesToHash)
    $encodedHash = [Convert]::ToBase64String($calculatedHash)
    $authorization = 'SharedKey {0}:{1}' -f $customerId,$encodedHash
    return $authorization
}


# Create the function to create and post the request
Function Post-OMSData($customerId, $sharedKey, $body, $logType)
{
    $method = "POST"
    $contentType = "application/json"
    $resource = "/api/logs"
    $rfc1123date = [DateTime]::UtcNow.ToString("r")
    $contentLength = $body.Length
    $signature = Build-Signature `
        -customerId $customerId `
        -sharedKey $sharedKey `
        -date $rfc1123date `
        -contentLength $contentLength `
        -fileName $fileName `
        -method $method `
        -contentType $contentType `
        -resource $resource
    $uri = "https://" + $customerId + ".ods.opinsights.azure.com" + $resource + "?api-version=2016-04-01"

    $headers = @{
        "Authorization" = $signature;
        "Log-Type" = $logType;
        "x-ms-date" = $rfc1123date;
        "time-generated-field" = $TimeStampField;
    }

    $response = Invoke-WebRequest -Uri $uri -Method $method -ContentType $contentType -Headers $headers -Body $body -UseBasicParsing
    return $response.StatusCode

}

# Submit the data to the API endpoint
Post-OMSData -customerId $customerId -sharedKey $sharedKey -body ([System.Text.Encoding]::UTF8.GetBytes($json)) -logType $logType  
```

### <a name="c-sample"></a>C#-voorbeeld

```
using System;
using System.Net;
using System.Security.Cryptography;

namespace OIAPIExample
{
    class ApiExample
    {
// An example JSON object, with key/value pairs
        static string json = @"[{""DemoField1"":""DemoValue1"",""DemoField2"":""DemoValue2""},{""DemoField1"":""DemoValue3"",""DemoField2"":""DemoValue4""}]";

// Update customerId to your Operations Management Suite workspace ID
        static string customerId = "xxxxxxxx-xxx-xxx-xxx-xxxxxxxxxxxx";

// For sharedKey, use either the primary or the secondary Connected Sources client authentication key   
        static string sharedKey = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx";

// LogName is name of the event type that is being submitted to Log Analytics
        static string LogName = "DemoExample";

// You can use an optional field to specify the timestamp from the data. If the time field is not specified, Log Analytics assumes the time is the message ingestion time
        static string TimeStampField = "";

        static void Main()
        {
// Create a hash for the API signature
            var datestring = DateTime.UtcNow.ToString("r");
            string stringToHash = "POST\n" + json.Length + "\napplication/json\n" + "x-ms-date:" + datestring + "\n/api/logs";
            string hashedString = BuildSignature(stringToHash, sharedKey);
            string signature = "SharedKey " + customerId + ":" + hashedString;

            PostData(signature, datestring, json);
        }

// Build the API signature
        public static string BuildSignature(string message, string secret)
        {
            var encoding = new System.Text.ASCIIEncoding();
            byte[] keyByte = Convert.FromBase64String(secret);
            byte[] messageBytes = encoding.GetBytes(message);
            using (var hmacsha256 = new HMACSHA256(keyByte))
            {
                byte[] hash = hmacsha256.ComputeHash(messageBytes);
                return Convert.ToBase64String(hash);
            }
        }

// Send a request to the POST API endpoint
        public static void PostData(string signature, string date, string json)
        {
            string url = "https://"+ customerId +".ods.opinsights.azure.com/api/logs?api-version=2016-04-01";
            using (var client = new WebClient())
            {
                client.Headers.Add(HttpRequestHeader.ContentType, "application/json");
                client.Headers.Add("Log-Type", LogName);
                client.Headers.Add("Authorization", signature);
                client.Headers.Add("x-ms-date", date);
                client.Headers.Add("time-generated-field", TimeStampField);
                client.UploadString(new Uri(url), "POST", json);
            }
        }
    }
}
```

### <a name="python-sample"></a>Python-voorbeeld

```
import json
import requests
import datetime
import hashlib
import hmac
import base64

# Update the customer ID to your Operations Management Suite workspace ID
customer_id = 'xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'

# For the shared key, use either the primary or the secondary Connected Sources client authentication key   
shared_key = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"

# The log type is the name of the event that is being submitted
log_type = 'WebMonitorTest'

# An example JSON web monitor object
json_data = [{
   "slot_ID": 12345,
    "ID": "5cdad72f-c848-4df0-8aaa-ffe033e75d57",
    "availability_Value": 100,
    "performance_Value": 6.954,
    "measurement_Name": "last_one_hour",
    "duration": 3600,
    "warning_Threshold": 0,
    "critical_Threshold": 0,
    "IsActive": "true"
},
{   
    "slot_ID": 67890,
    "ID": "b6bee458-fb65-492e-996d-61c4d7fbb942",
    "availability_Value": 100,
    "performance_Value": 3.379,
    "measurement_Name": "last_one_hour",
    "duration": 3600,
    "warning_Threshold": 0,
    "critical_Threshold": 0,
    "IsActive": "false"
}]
body = json.dumps(json_data)

#####################
######Functions######  
#####################

# Build the API signature
def build_signature(customer_id, shared_key, date, content_length, method, content_type, resource):
    x_headers = 'x-ms-date:' + date
    string_to_hash = method + "\n" + str(content_length) + "\n" + content_type + "\n" + x_headers + "\n" + resource
    bytes_to_hash = bytes(string_to_hash).encode('utf-8')  
    decoded_key = base64.b64decode(shared_key)
    encoded_hash = base64.b64encode(hmac.new(decoded_key, bytes_to_hash, digestmod=hashlib.sha256).digest())
    authorization = "SharedKey {}:{}".format(customer_id,encoded_hash)
    return authorization

# Build and send a request to the POST API
def post_data(customer_id, shared_key, body, log_type):
    method = 'POST'
    content_type = 'application/json'
    resource = '/api/logs'
    rfc1123date = datetime.datetime.utcnow().strftime('%a, %d %b %Y %H:%M:%S GMT')
    content_length = len(body)
    signature = build_signature(customer_id, shared_key, rfc1123date, content_length, method, content_type, resource)
    uri = 'https://' + customer_id + '.ods.opinsights.azure.com' + resource + '?api-version=2016-04-01'

    headers = {
        'content-type': content_type,
        'Authorization': signature,
        'Log-Type': log_type,
        'x-ms-date': rfc1123date
    }

    response = requests.post(uri,data=body, headers=headers)
    if (response.status_code == 202):
        print 'Accepted'
    else:
        print "Response code: {}".format(response.status_code)

post_data(customer_id, shared_key, body, log_type)
```

## <a name="next-steps"></a>Volgende stappen

- [Ontwerp weergeven](log-analytics-view-designer.md) gebruik te maken van aangepaste weergaven van de gegevens die u verzendt.
