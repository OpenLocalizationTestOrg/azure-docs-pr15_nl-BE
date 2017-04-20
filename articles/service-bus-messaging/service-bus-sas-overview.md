<properties
    pageTitle="Gedeelde toegang handtekeningen overzicht | Microsoft Azure"
    description="Wat zijn gedeelde toegang handtekeningen, hoe ze werken, en hoe ze te gebruiken vanaf een knooppunt, PHP, en C#."
    services="service-bus"
    documentationCenter="na"
    authors="djrosanova"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/02/2016"
    ms.author="darosa;sethm"/>

# <a name="shared-access-signatures"></a>Gedeelde toegang handtekeningen

*Gedeelde toegang handtekeningen* (SAS) vormen de primaire beveiligingsmechanisme voor Service Bus, met inbegrip van de gebeurtenis Hubs, brokered messaging (wachtrijen en onderwerpen) en doorsturen van berichten. In dit artikel wordt beschreven gedeelde toegang handtekeningen, hoe ze werken en hoe ze te gebruiken op een manier platform agnostic.

## <a name="overview-of-sas"></a>Overzicht van SAS

Gedeelde Access-handtekeningen zijn een verificatiemechanisme op basis van SHA 256 secure hashes of URI's. SAS is een zeer krachtig mechanisme dat wordt gebruikt door alle services met Service Bus. Bij werkelijk gebruik SAS bestaat uit twee onderdelen: een *Gedeelde-beleid* en een *Gedeelde Access-handtekening* (vaak genoemd een *token*).

U vindt meer gedetailleerde informatie over gedeelde toegang handtekeningen met Bus Service in een [gedeelde Access-handtekening verificatie met Service Bus](service-bus-shared-access-signature-authentication.md).

## <a name="shared-access-policy"></a>Gedeelde-beleid

Een belangrijk ding om te weten over SA's is dat alles met een beleid begint. Voor elk beleid dat u besluit op drie typen gegevens: **de naam**, het **bereik**en **machtigingen**. De **naam** is slechts; een unieke naam binnen dat bereik. Het bereik is eenvoudig genoeg: de URI van de desbetreffende resource is. Voor een Service Bus-naamruimte, is het bereik de volledig gekwalificeerde domeinnaam (FQDN) zoals `https://<yournamespace>.servicebus.windows.net/`.

De beschikbare machtigingen voor een beleid zijn grotendeels geen uitleg:

  + Verzenden
  + Luisteren
  + Beheren

Nadat u het beleid hebt gemaakt, krijgt het een *Primaire sleutel* en een *Secundaire sleutel*. Dit zijn cryptografisch sterke sleutels. Niet dat ze kwijtraken of lekken ze - ze altijd beschikbaar zijn in de [portal Azure][]zult zijn. Gebruikt u een van de gegenereerde sleutels en u ze op elk gewenst moment kunt genereren. Als u toevoegen of wijzigen van de primaire sleutel in het beleid, worden er toegang tot gedeelde handtekeningen gemaakt op basis van deze ongeldig.

Wanneer u een naamruimte Bus Service maakt, een beleid wordt automatisch gemaakt voor de gehele naamruimte **RootManageSharedAccessKey**genoemd en dit beleid heeft alle machtigingen. U zich niet aanmelden als **root**, zodat dit beleid niet gebruiken tenzij er een goede reden. In het tabblad **configureren** voor de naamruimte in de portal kunt u extra beleidsregels. Is het belangrijk dat opmerking dat één niveau in Service Bus structuur (naamruimte, wachtrij, Hub gebeurtenis, enz.) kan slechts maximaal 12 beleid gekoppeld hebben.

## <a name="shared-access-signature-token"></a>Gedeelde toegang handtekening (token)

Het beleid zelf is niet het toegangstoken voor Service Bus. Is het object van waaruit het toegangstoken wordt gegenereerd - met de primaire of secundaire sleutel. Het token wordt gegenereerd door zorgvuldig een tekenreeks in de volgende notatie:

```
SharedAccessSignature sig=<signature-string>&se=<expiry>&skn=<keyName>&sr=<URL-encoded-resourceURI>
```

Waarbij `signature-string` is het SHA-256 hash van het toepassingsgebied van het token (**bereik** zoals beschreven in de vorige sectie) met een CRLF toegevoegd en een verstrijken tijd (in seconden sinds de epoche: `00:00:00 UTC` op 1 januari 1970).

De hash lijkt op de volgende pseudocode en 32 bytes als resultaat gegeven.

```
SHA-256('https://<yournamespace>.servicebus.windows.net/'+'\n'+ 1438205742)
```

De niet-hashing-waarden worden in de tekenreeks **SharedAccessSignature** zodat de ontvanger kan de hash met dezelfde parameters, om ervoor te zorgen dat deze het resultaat berekenen. Geeft het bereik van de URI en de naam van het beleid moet worden gebruikt voor het berekenen van de hash wordt aangeduid. Dit is belangrijk uit veiligheidsoverwegingen. De handtekening overeenkomt met die welke de ontvanger (Bus-Service) wordt berekend, wordt de toegang geweigerd. U kunt op dit moment zijn ervoor dat de afzender toegang tot de sleutel heeft en de rechten die zijn opgegeven in het beleid moet worden verleend.

## <a name="generating-a-signature-from-a-policy"></a>Een handtekening te genereren uit een beleid

Hoe u werkelijk doet dit in code? Laten we bij enkele van deze.

### <a name="nodejs"></a>NodeJS

```
function createSharedAccessToken(uri, saName, saKey) { 
    if (!uri || !saName || !saKey) { 
            throw "Missing required parameter"; 
        } 
    var encoded = encodeURIComponent(uri); 
    var now = new Date(); 
    var week = 60*60*24*7;
    var ttl = Math.round(now.getTime() / 1000) + week;
    var signature = encoded + '\n' + ttl; 
    var signatureUTF8 = utf8.encode(signature); 
    var hash = crypto.createHmac('sha256', saKey).update(signatureUTF8).digest('base64'); 
    return 'SharedAccessSignature sr=' + encoded + '&sig=' +  
        encodeURIComponent(hash) + '&se=' + ttl + '&skn=' + saName; 
}
``` 

### <a name="java"></a>Java

```
private static String GetSASToken(String resourceUri, String keyName, String key)
  {
      long epoch = System.currentTimeMillis()/1000L;
      int week = 60*60*24*7;
      String expiry = Long.toString(epoch + week);

      String sasToken = null;
      try {
          String stringToSign = URLEncoder.encode(resourceUri, "UTF-8") + "\n" + expiry;
          String signature = getHMAC256(key, stringToSign);
          sasToken = "SharedAccessSignature sr=" + URLEncoder.encode(resourceUri, "UTF-8") +"&sig=" +
                  URLEncoder.encode(signature, "UTF-8") + "&se=" + expiry + "&skn=" + keyName;
      } catch (UnsupportedEncodingException e) {

          e.printStackTrace();
      }

      return sasToken;
  }


public static String getHMAC256(String key, String input) {
    Mac sha256_HMAC = null;
    String hash = null;
    try {
        sha256_HMAC = Mac.getInstance("HmacSHA256");
        SecretKeySpec secret_key = new SecretKeySpec(key.getBytes(), "HmacSHA256");
        sha256_HMAC.init(secret_key);
        Encoder encoder = Base64.getEncoder();

        hash = new String(encoder.encode(sha256_HMAC.doFinal(input.getBytes("UTF-8"))));

    } catch (InvalidKeyException e) {
        e.printStackTrace();
    } catch (NoSuchAlgorithmException e) {
        e.printStackTrace();
   } catch (IllegalStateException e) {
        e.printStackTrace();
    } catch (UnsupportedEncodingException e) {
        e.printStackTrace();
    }

    return hash;
}
```

### <a name="php"></a>PHP

```
function generateSasToken($uri, $sasKeyName, $sasKeyValue) 
{ 
$targetUri = strtolower(rawurlencode(strtolower($uri))); 
$expires = time();  
$expiresInMins = 60; 
$week = 60*60*24*7;
$expires = $expires + $week; 
$toSign = $targetUri . "\n" . $expires; 
$signature = rawurlencode(base64_encode(hash_hmac('sha256',             
 $toSign, $sasKeyValue, TRUE))); 

$token = "SharedAccessSignature sr=" . $targetUri . "&sig=" . $signature . "&se=" . $expires .      "&skn=" . $sasKeyName; 
return $token; 
}
```
 
### <a name="c35"></a>C & #35;

```
private static string createToken(string resourceUri, string keyName, string key)
{
    TimeSpan sinceEpoch = DateTime.UtcNow - new DateTime(1970, 1, 1);
    var week = 60 * 60 * 24 * 7;
    var expiry = Convert.ToString((int)sinceEpoch.TotalSeconds + week);
    string stringToSign = HttpUtility.UrlEncode(resourceUri) + "\n" + expiry;
    HMACSHA256 hmac = new HMACSHA256(Encoding.UTF8.GetBytes(key));
    var signature = Convert.ToBase64String(hmac.ComputeHash(Encoding.UTF8.GetBytes(stringToSign)));
    var sasToken = String.Format(CultureInfo.InvariantCulture, "SharedAccessSignature sr={0}&sig={1}&se={2}&skn={3}", HttpUtility.UrlEncode(resourceUri), HttpUtility.UrlEncode(signature), expiry, keyName);
    return sasToken;
}
```

## <a name="using-the-shared-access-signature-at-http-level"></a>Met behulp van de toegang tot gedeelde handtekening (op http-niveau)
 
Nu u weet hoe u gedeelde toegang handtekeningen maken voor alle entiteiten in de Bus Service, bent u klaar voor het uitvoeren van een HTTP POST:

```
POST https://<yournamespace>.servicebus.windows.net/<yourentity>/messages
Content-Type: application/json
Authorization: SharedAccessSignature sr=https%3A%2F%2F<yournamespace>.servicebus.windows.net%2F<yourentity>&sig=<yoursignature from code above>&se=1438205742&skn=KeyName
ContentType: application/atom+xml;type=entry;charset=utf-8
``` 
    
Vergeet niet dat dit werkt voor alles. SAS kunt u maken voor een wachtrij, onderwerp, abonnement, gebeurtenis Hub of doorsturen. Als u publisher-identiteit voor gebeurtenis Hubs, u toe te voegen `/publishers/< publisherid>`.

Als u een afzender of een client een SAS-token, zij niet rechtstreeks de sleutel hebt en de hash te verkrijgen kan niet worden omgekeerd. Zo hebt u controle over wat zij toegang hebben en hoe lang. Een belangrijk ding om te onthouden is dat als u de primaire sleutel in het beleid wijzigt, wordt de toegang tot gedeelde handtekeningen gemaakt op basis van deze ongeldig.

## <a name="using-the-shared-access-signature-at-amqp-level"></a>Met behulp van de toegang tot gedeelde handtekening (op AMQP niveau)

In de vorige sectie hebt u gezien hoe het token SAS gebruiken met een HTTP POST-aanvraag voor het verzenden van gegevens naar de Bus-Service. Zoals u weet, kunt u toegang tot Service Bus met behulp van de geavanceerde Message Queuing-Protocol (AMQP) is het voorkeursprotocol voor betere prestaties in veel scenario's gebruiken. Het SAS token gebruik met AMQP wordt beschreven in het document in concept werken is sinds 2013 maar goed ondersteund door Azure vandaag [AMQP Claim-Based Security versie 1.0](https://www.oasis-open.org/committees/download.php/50506/amqp-cbs-v1%200-wd02%202013-08-12.doc) .

Voordat u begint met het verzenden van gegevens naar de Bus-Service, moet de uitgever het token SAS binnen een AMQP bericht verzenden naar een goed gedefinieerde AMQP knooppunt met de naam **$cbs** (Zie het als een "speciale" wachtrij te verwerven en alle SAS-tokens te valideren door de service gebruikt). De uitgever moet geeft u het veld **ReplyTo** in het bericht van AMQP; Dit is het knooppunt waarop de service de uitgever met het resultaat van de token validatie (tussen publisher en een eenvoudige aanvraag/antwoord-patroon beantwoordt). Dit knooppunt antwoord wordt gemaakt 'on the fly,"spreken over"dynamisch maken van extern knooppunt"zoals beschreven door de AMQP 1.0-specificatie. Nadat u hebt gecontroleerd of het token SAS geldig is, de uitgever vooruitgaan en om gegevens te verzenden naar de service start.

De volgende stappen laten zien hoe het token SAS met AMQP-protocol met behulp van de bibliotheek [Lite AMQP.Net](https://github.com/Azure/amqpnetlite) verzenden. Dit is handig als u niet de officiële Service Bus SDK gebruiken (bijvoorbeeld op WinRT, .net Compact Framework, .net Micro Framework en Mono) ontwikkelen in C\#. Deze bibliotheek is natuurlijk handig om te begrijpen hoe op basis van claims op het niveau van AMQP werkt zoals u hebt gezien hoe het werkt op het niveau van HTTP (met een HTTP POST-aanvraag en het SAS token verzonden in de koptekst "Vergunning"). Als u dergelijke uitgebreide kennis van de AMQP niet nodig hebt, kunt u de officiële Service Bus SDK met .net Framework-toepassingen die het voor u doen.

### <a name="c35"></a>C & #35;

```
/// <summary>
/// Send claim-based security (CBS) token
/// </summary>
/// <param name="shareAccessSignature">Shared access signature (token) to send</param>
private bool PutCbsToken(Connection connection, string sasToken)
{
    bool result = true;
    Session session = new Session(connection);

    string cbsClientAddress = "cbs-client-reply-to";
    var cbsSender = new SenderLink(session, "cbs-sender", "$cbs");
    var cbsReceiver = new ReceiverLink(session, cbsClientAddress, "$cbs");

    // construct the put-token message
    var request = new Message(sasToken);
    request.Properties = new Properties();
    request.Properties.MessageId = Guid.NewGuid().ToString();
    request.Properties.ReplyTo = cbsClientAddress;
    request.ApplicationProperties = new ApplicationProperties();
    request.ApplicationProperties["operation"] = "put-token";
    request.ApplicationProperties["type"] = "servicebus.windows.net:sastoken";
    request.ApplicationProperties["name"] = Fx.Format("amqp://{0}/{1}", sbNamespace, entity);
    cbsSender.Send(request);

    // receive the response
    var response = cbsReceiver.Receive();
    if (response == null || response.Properties == null || response.ApplicationProperties == null)
    {
        result = false;
    }
    else
    {
        int statusCode = (int)response.ApplicationProperties["status-code"];
        if (statusCode != (int)HttpStatusCode.Accepted && statusCode != (int)HttpStatusCode.OK)
        {
            result = false;
        }
    }

    // the sender/receiver may be kept open for refreshing tokens
    cbsSender.Close();
    cbsReceiver.Close();
    session.Close();

    return result;
}
```

De `PutCbsToken()` methode ontvangt de *verbinding* (AMQP verbinding klasse-instantie als bedoeld in de [bibliotheek van AMQP .NET Lite](https://github.com/Azure/amqpnetlite)) met de TCP-verbinding voor de service en de *sasToken* -parameter die is het SAS-token te verzenden. 

> [AZURE.NOTE] Het is belangrijk dat de verbinding wordt gemaakt met **SASL-verificatiemechanisme instellen op externe** (en niet de standaard zonder opmaak met gebruikersnaam en wachtwoord die worden gebruikt wanneer u niet nodig voor het verzenden van het token SAS).

De uitgever maakt vervolgens twee AMQP koppelingen voor het verzenden van het token SAS en ontvangst van het antwoord (het validatieresultaat van een token) van de service.

De AMQP-bericht bevat een set eigenschappen en meer informatie dan een eenvoudig bericht. De SAS-token is de hoofdtekst van het bericht (met behulp van de constructor). De eigenschap **'ReplyTo'** is ingesteld op de naam van het knooppunt voor het ontvangen van het validatieresultaat op de koppeling van de ontvanger (u kunt de naam wijzigen als u wilt, en worden dynamisch door de service worden gemaakt). De laatste drie toepassing/aangepaste eigenschappen worden door de service gebruikt om aan te geven welk type bewerking niet uitvoeren. Zoals beschreven door de specificatie van CBS concept, moeten ze de **naam van de bewerking** ("put-token') het **type token** (in dit geval een 'servicebus.windows.net:sastoken') en de **'naam' van de doelgroep** waarop het token van toepassing is (het geheel).

De uitgever moet na het verzenden van het token SAS op de koppeling van de afzender, het antwoord op de koppeling van de ontvanger worden gelezen. Het antwoord is een eenvoudig bericht AMQP met een eigenschap van een toepassing met de naam **' status ' - code** die dezelfde waarden als een HTTP-statuscode kan bevatten. 

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over wat u met deze tokens SAS doen kunt [Service Bus REST API reference](https://msdn.microsoft.com/library/azure/hh780717.aspx) .

Zie voor meer informatie over verificatie Bus Service [Service Bus verificatie en machtiging](service-bus-authentication-and-authorization.md). 

Er zijn meer voorbeelden van SA's in C# en Java Script in [dit blogbericht](http://developers.de/blogs/damir_dobric/archive/2013/10/17/how-to-create-shared-access-signature-for-service-bus.aspx).

[Azure portal]: https://portal.azure.com