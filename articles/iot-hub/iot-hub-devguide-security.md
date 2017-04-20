<properties
 pageTitle="Handleiding voor ontwikkelaars - toegang beheren met IoT Hub | Microsoft Azure"
 description="Azure IoT Hub developer guide - toegang tot IoT Hub en beveiliging beheren"
 services="iot-hub"
 documentationCenter=".net"
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="dobett"/>

# <a name="control-access-to-iot-hub"></a>Toegang tot de IoT Hub

## <a name="overview"></a>Overzicht

Dit artikel beschrijft de opties voor het beveiligen van uw hub-IoT. IoT-Hub maakt gebruik van *machtigingen* toegang verlenen tot de eindpunten van elk IoT hub. Machtigingen beperken de toegang tot een IoT hub op basis van functionaliteit.

In dit artikel wordt beschreven:

- De verschillende machtigingen kunt u geven aan een apparaat of back-end-app toegang krijgen tot uw hub IoT.
- Het verificatieproces en de tokens die wordt gebruikt om machtigingen te controleren.
- Het bereik referenties toegang tot bepaalde bronnen te beperken.
- IoT Hub ondersteuning voor x.509-certificaten.
- Apparaat aangepaste verificatiemechanismen die gebruikmaken van bestaande apparaat identiteit registers of verificatieschema's.

### <a name="when-to-use"></a>Wanneer gebruikt u

U moet de juiste machtigingen voor toegang tot een van de eindpunten IoT Hub hebben. Een apparaat moet bijvoorbeeld een token met de beveiligingsreferenties die bij elk bericht naar IoT Hub stuurt.

## <a name="access-control-and-permissions"></a>Toegangsbeheer en machtigingen

U kunt [machtigingen](#iot-hub-permissions) toekennen op de volgende manieren:

* **Hub-niveau gedeelde-beleid**. Gedeelde-beleid kunnen een willekeurige combinatie van [machtigingen](#iot-hub-permissions)toekennen. U kunt beleid definiëren in de [Azure portal][lnk-management-portal], of via programmacode met behulp van de [IoT Hub resource provider REST API's][lnk-resource-provider-apis]. Een zojuist gemaakte IoT hub heeft de volgende standaardbeleid:

    - **iothubowner**: beleid met alle machtigingen.
    - **service**: beleid met toestemming van de ServiceConnect.
    - **apparaat**: beleid met toestemming van de DeviceConnect.
    - **registryRead**: beleid met toestemming van de RegistryRead.
    - **registryReadWrite**: beleid met machtigingen voor RegistryRead en RegistryWrite.


* **Beveiligingsreferenties voor per apparaat**. Elke IoT Hub bevat een [apparaat identiteit register][lnk-identity-registry]. Voor elk apparaat in het register, kunt u beveiligingsreferenties die de machtigingen **DeviceConnect** binnen het bereik van de eindpunten van het desbetreffende apparaat.

Bijvoorbeeld, in een typisch IoT-oplossing:

- Het onderdeel voor documentbeheer van apparaat gebruikmaakt van het beleid van *registryReadWrite* .
- De gebeurtenis processor-component gebruikt de *service* beleid.
- De runtime apparaat business logica-component gebruikt de *service* -beleid.
- Afzonderlijke apparaten verbinding maken met behulp van referenties die zijn opgeslagen in het register van de IoT-hub identiteit.

## <a name="authentication"></a>Verificatie

Azure IoT Hub verleent toegang tot de eindpunten door een token met gedeelde-beleid en de beveiligingsreferenties van apparaat identiteit register te controleren.

Gebruikersnaam en symmetrische sleutels worden nooit via het netwerk verzonden.

> [AZURE.NOTE] De provider Azure IoT Hub resource is beveiligd via uw abonnement Azure, zijn alle providers in [Azure Resource Manager][lnk-azure-resource-manager].

Zie voor meer informatie over het maken en gebruiken van beveiligingstokens [beveiligingstokens IoT Hub][lnk-sas-tokens].

### <a name="protocol-specifics"></a>Protocol specificaties

Elk ondersteund protocol, zoals MQTT, AMQP en HTTP transports tokens op verschillende manieren.

Bij het gebruik van MQTT, het pakket VERBINDEN apparaat-id heeft als de ClientId, {iothubhostname} / {deviceId} in het veld gebruikersnaam en een token SAS in het veld wachtwoord. {iothubhostname} moet de volledige CName van de hub IoT (bijvoorbeeld contoso.azure-devices.net).

Bij het gebruik van [AMQP][lnk-amqp], IoT Hub [SASL zonder opmaak] ondersteunt[ lnk-sasl-plain] en [AMQP vorderingen-beveiliging op basis van-][lnk-cbs].

Als u AMQP vorderingen gebaseerd-beveiliging gebruikt, is de standaard geeft aan hoe deze tokens te verzenden.

SASL zonder opmaak is de **gebruikersnaam** :

* `{policyName}@sas.root.{iothubName}`Als de hub niveau tokens gebruiken.
* `{deviceId}@sas.{iothubname}`Als het apparaat binnen het bereik tokens gebruiken.

In beide gevallen wordt het wachtwoordveld bevat het token, zoals omschreven in [beveiligingstokens IoT Hub][lnk-sas-tokens].

HTTP-verificatie van de wordt geïmplementeerd door een geldige token in de aanvraagheader **vergunning** .

#### <a name="example"></a>Voorbeeld

Gebruikersnaam (DeviceId is hoofdlettergevoelig):`iothubname.azure-devices.net/DeviceId`

Wachtwoord (SAS met apparaat Explorer genereren):`SharedAccessSignature sr=iothubname.azure-devices.net%2fdevices%2fDeviceId&sig=kPszxZZZZZZZZZZZZZZZZZAhLT%2bV7o%3d&se=1487709501`

> [AZURE.NOTE] De [SDK's Azure IoT Hub] [ lnk-sdks] tokens automatisch genereren wanneer u verbinding maakt met de service. In sommige gevallen ondersteunen de SDK's niet alle protocollen of de verificatiemethoden.

### <a name="special-considerations-for-sasl-plain"></a>Speciale overwegingen voor SASL zonder opmaak

Wanneer u SASL zonder opmaak met AMQP, kan een client die verbinding met een hub IoT één token voor elke TCP-verbinding gebruiken. Wanneer het token is verlopen, wordt de TCP-verbinding verbreekt u de service en een opnieuw activeert. Dit gedrag is zijn niet geschikt voor een back-end onderdeel van toepassing, zeer schadelijk voor een apparaat-toepassing om de volgende redenen:

*  Gateways verbinden gewoonlijk voor een groot aantal apparaten. Wanneer u ONBEWERKTE SASL, hebben ze voor het maken van een afzonderlijke TCP-verbinding voor elk apparaat dat verbinding maakt met een hub IoT. In dit scenario aanzienlijk vergroot het verbruik van energie en -netwerkbronnen en verhoogt de latentie van elk apparaatverbinding.
* Apparaten met beperkte capaciteit nadelig worden beïnvloed door het toenemend gebruik van bronnen te herstellen na het verlopen van elke token.

## <a name="scope-hub-level-credentials"></a>Scope hub niveau referenties

U kunt het bereik van beleidsregels voor beveiliging op gebruikersniveau hub door tokens maken met een beperkte bron-URI. Het eindpunt apparaat naar cloud om berichten te verzenden vanaf een apparaat is bijvoorbeeld **{deviceId} /devices/ berichten/gebeurtenissen**. Ook kunt u een beleid voor gedeelde toegang op gebruikersniveau hub met **DeviceConnect** machtigingen voor het ondertekenen van een token waarvan resourceURI **/devices/ {deviceId}**is. Deze benadering maakt u een token die alleen kan worden gebruikt voor het verzenden van berichten voor apparaat **deviceId**.

Dit mechanisme is vergelijkbaar met de [gebeurtenis Hubs publisher beleid][lnk-event-hubs-publisher-policy], en kunt u aangepaste verificatiemethoden implementeren.

## <a name="security-tokens"></a>Beveiligingstokens

IoT-Hub maakt gebruik van beveiligingstokens te verifiëren, apparaten en services om te voorkomen dat toetsen op het netwerk te verzenden. Bovendien zijn beveiligingstokens beperkte geldigheid en de scope. [Azure IoT Hub SDK's] [ lnk-sdks] automatisch tokens te genereren zonder dat u een speciale configuratie vereist. Sommige scenario vereist echter dat de gebruiker te genereren en rechtstreeks gebruik van beveiligingstokens. Deze omvatten het rechtstreekse gebruik van de MQTT, AMQP of HTTP-oppervlakken of de uitvoering van het token service patroon, zoals uiteengezet in de [verificatie van aangepaste apparaat][lnk-custom-auth].

IoT Hub kunnen ook worden geverifieerd met IoT Hub-apparaten met [x.509-certificaten][lnk-x509]. 

### <a name="security-token-structure"></a>Security token-structuur
U kunt beveiligingstokens tijd begrensd toegang tot apparaten en services in de specifieke functionaliteit van IoT Hub verlenen. Om ervoor te zorgen dat alleen geautoriseerde apparaten en services verbinding kunnen maken, moeten worden beveiligingstokens ondertekend met een beleid voor gedeelde toegang sleutel of een symmetrische sleutel die is opgeslagen met de identiteit van een apparaat in het register van de identiteit.

Een token is ondertekend met een gedeelde toegang beleid belangrijke verleent toegang tot alle functies die zijn gekoppeld aan de toegangsmachtigingen voor gedeelde beleid. Aan de andere kant een token dat is ondertekend met een symmetrische sleutel van de identiteit van een apparaat alleen de **DeviceConnect** een machtiging wordt verleend voor de identiteit gekoppeld apparaat.

Het beveiligingstoken heeft de volgende indeling:

    SharedAccessSignature sig={signature-string}&se={expiry}&skn={policyName}&sr={URL-encoded-resourceURI}

De verwachte waarden zijn:

| Waarde | Beschrijving |
| ----- | ----------- |
| {handtekening} | Een tekenreeks van HMAC SHA256 handtekening van het formulier: `{URL-encoded-resourceURI} + "\n" + expiry`. **Belangrijk**: de sleutel is verkregen door base64 en gebruikt als sleutel voor het uitvoeren van de berekening van HMAC-SHA256. |
| {resourceURI} | URI-voorvoegsel (per segment) van de eindpunten die kunnen worden geopend met deze token, beginnend met de hostnaam van de IoT hub (geen protocol). Bijvoorbeeld:`myHub.azure-devices.net/devices/device1` |
| {verstrijken} | UTF8-tekenreeksen voor het aantal seconden sinds de epoche 00:00:00 UTC op 1 januari 1970. |
| {URL-gecodeerde-resourceURI} | Lagere geval URL-codering van de kleine bron-URI |
| {naambeleid} | De naam van het beleid voor gedeelde toegang deze token betrekking heeft. Afwezig in het geval van tokens die verwijzen naar het apparaat register referenties. |

**Opmerking op prefix**: de URI-prefix wordt berekend per segment en niet door teken. Bijvoorbeeld `/a/b` is een voorvoegsel voor `/a/b/c` , maar niet voor `/a/bc`.

In het volgende fragment van Node.js bevat een functie genaamd **generateSasToken** die het token van de "inputs berekent" `resourceUri, signingKey, policyName, expiresInMins`. De volgende secties bevatten informatie over het initialiseren van de verschillende ingangen voor de verschillende token use-cases.

    var generateSasToken = function(resourceUri, signingKey, policyName, expiresInMins) {
        resourceUri = encodeURIComponent(resourceUri.toLowerCase()).toLowerCase();

        // Set expiration in seconds
        var expires = (Date.now() / 1000) + expiresInMins * 60;
        expires = Math.ceil(expires);
        var toSign = resourceUri + '\n' + expires;

        // Use crypto
        var hmac = crypto.createHmac('sha256', new Buffer(signingKey, 'base64'));
        hmac.update(toSign);
        var base64UriEncoded = encodeURIComponent(hmac.digest('base64'));

        // Construct autorization string
        var token = "SharedAccessSignature sr=" + resourceUri + "&sig="
        + base64UriEncoded + "&se=" + expires;
        if (policyName) token += "&skn="+policyName;
        return token;
    };

Als een vergelijking is de gelijkwaardige Python code genereren een beveiligingstoken:

    from base64 import b64encode, b64decode
    from hashlib import sha256
    from time import time
    from urllib import quote_plus, urlencode
    from hmac import HMAC

    def generate_sas_token(uri, key, policy_name, expiry=3600):
        ttl = time() + expiry
        sign_key = "%s\n%d" % ((quote_plus(uri)), int(ttl))
        print sign_key
        signature = b64encode(HMAC(b64decode(key), sign_key, sha256).digest())

        rawtoken = {
            'sr' :  uri,
            'sig': signature,
            'se' : str(int(ttl))
        }

        if policy_name is not None:
            rawtoken['skn'] = policy_name

        return 'SharedAccessSignature ' + urlencode(rawtoken)

> [AZURE.NOTE] Aangezien de geldigheid van het token wordt gevalideerd op IoT Hub-computers, is het belangrijk dat de drift van de klok van de computer waarmee het token wordt gegenereerd minimaal.

### <a name="use-sas-tokens-in-a-device-client"></a>SAS-tokens gebruiken in een device-client

Er zijn twee manieren om toegang te verkrijgen **DeviceConnect** machtigingen met IoT Hub met beveiligingstokens: een [symmetrische apparaatsleutel uit het register van de identiteit apparaat](#use-a-symmetric-key-in-the-identity-registry)gebruikt of een [toegangstoets beleid worden gedeeld](#use-a-shared-access-policy).

Houd er rekening mee dat alle functionaliteit die toegankelijk is vanaf apparaten wordt blootgesteld aan het ontwerp op de eindpunten met het voorvoegsel `/devices/{deviceId}`.

> [AZURE.IMPORTANT] De enige manier om een specifiek apparaat wordt geverifieerd door IoT-Hub maakt gebruik van de symmetrische sleutel voor het apparaat identiteit. In de gevallen als een gedeelde-beleid wordt gebruikt voor toegang tot de apparaatfunctionaliteit van het, de oplossing moet rekening houden met de afgifte van het beveiligingstoken als een vertrouwde subonderdeel component.

De eindpunten apparaat gericht zijn (onafhankelijk van het protocol):

| Eindpunt | Functionaliteit |
| ----- | ----------- |
| `{iot hub host name}/devices/{deviceId}/messages/events` | Apparaat-wolk-berichten verzenden. |
| `{iot hub host name}/devices/{deviceId}/devicebound` | Cloud-to-device-berichten ontvangen. |

### <a name="use-a-symmetric-key-in-the-identity-registry"></a>Gebruik van een symmetrische sleutel in het register van de identiteit

Bij het gebruik van symmetrische sleutel van de identiteit van een apparaat voor het genereren van een token de policyName (`skn`)-element van het token wordt weggelaten.

Een token gemaakt voor toegang tot alle functies van het apparaat moet bijvoorbeeld de volgende parameters:

* bron-URI: `{IoT hub name}.azure-devices.net/devices/{device id}`,
* handtekeningsleutel: de symmetrische sleutel voor het `{device id}` -id
* Er is geen beleidsnaam
* een verloopdatum.

Een voorbeeld met behulp van de bovenstaande knooppunt functie is:

    var endpoint ="myhub.azure-devices.net/devices/device1";
    var deviceKey ="...";

    var token = generateSasToken(endpoint, deviceKey, null, 60);

Het resultaat, dat toegang tot alle functies voor device1 verleent, zijn:

    SharedAccessSignature sr=myhub.azure-devices.net%2fdevices%2fdevice1&sig=13y8ejUk2z7PLmvtwR5RqlGBOVwiq7rQR3WZ5xZX3N4%3D&se=1456971697

> [AZURE.NOTE] Het is mogelijk een hulpprogramma voor het .NET [Explorer apparaat]veilig-token genereren[lnk-device-explorer].

### <a name="use-a-shared-access-policy"></a>Gebruik een gedeelde-beleid

Bij het maken van een token uit een gedeelde access-beleid, het beleid voor het veld naam `skn` moet worden ingesteld op de naam van het beleid dat wordt gebruikt. Het is ook vereist dat het beleid de machtiging **DeviceConnect verleent** .

De twee belangrijkste scenario's voor het gebruik van beleid voor gedeelde toegang te krijgen tot de apparaatfunctionaliteit van het zijn:

* [protocol gateways cloud][lnk-endpoints],
* [token services] [ lnk-custom-auth] gebruikt voor het implementeren van aangepaste verificatieschema's.

Aangezien het beleid voor gedeelde toegang kan mogelijk toegang tot de verbinding als een apparaat, is het belangrijk dat u de juiste bron-URI gebruikt bij het maken van beveiligingstokens. Dit is vooral belangrijk voor token services, moeten u het bereik van het token voor een specifiek apparaat met behulp van de bron-URI. Dit punt is minder relevant voor gateways protocol omdat ze al het verkeer voor alle apparaten zijn clienthyperlinkafbeeldingen.

Als u bijvoorbeeld maakt een token service met behulp van het gedeelde toegang vooraf gemaakte beleid **apparaat** een token met de volgende parameters:

* bron-URI: `{IoT hub name}.azure-devices.net/devices/{device id}`,
* handtekeningsleutel: een van de sleutels van de `device` -beleid
* Beleidsnaam: `device`,
* een verloopdatum.

Een voorbeeld met behulp van de bovenstaande knooppunt functie is:

    var endpoint ="myhub.azure-devices.net/devices/device1";
    var policyName = 'device';
    var policyKey = '...';

    var token = generateSasToken(endpoint, policyKey, policyName, 60);

Het resultaat, dat toegang tot alle functies voor device1 verleent, zijn:

    SharedAccessSignature sr=myhub.azure-devices.net%2fdevices%2fdevice1&sig=13y8ejUk2z7PLmvtwR5RqlGBOVwiq7rQR3WZ5xZX3N4%3D&se=1456971697&skn=device

Een gateway protocol gebruiken hetzelfde voor alle apparaten eenvoudig instellen van de bron-URI naar `myhub.azure-devices.net/devices`.

### <a name="use-security-tokens-from-service-components"></a>Gebruik van beveiligingstokens uit service-onderdelen

Service-onderdelen kunnen alleen beveiligingstokens gedeelde-beleid de juiste machtigingen verlenen zoals eerder uitgelegd genereren.

Dit zijn de servicefuncties op de eindpunten worden blootgesteld:

| Eindpunt | Functionaliteit |
| ----- | ----------- |
| `{iot hub host name}/devices` | Maken, bijwerken, ophalen en verwijderen van apparaat-id's. |
| `{iot hub host name}/messages/events` | Apparaat-wolk-berichten ontvangen. |
| `{iot hub host name}/servicebound/feedback` | Feedback voor cloud-to-device-berichten ontvangen. |
| `{iot hub host name}/devicebound` | Wolk naar apparaat verzenden. |

Als u bijvoorbeeld maakt een service genereren met behulp van het gedeelde toegang vooraf gemaakte beleid voor **registryRead** een token met de volgende parameters:

* bron-URI: `{IoT hub name}.azure-devices.net/devices`,
* handtekeningsleutel: een van de sleutels van de `registryRead` -beleid
* Beleidsnaam: `registryRead`,
* een verloopdatum.

    var eindpunt = "myhub.azure-devices.net/devices";   var policyName = 'apparaat';   var policyKey =...;

    var token = generateSasToken (eindpunt, policyKey, policyName, 60);

Het resultaat zou lezen alle apparaat-id's worden verleend, zijn:

    SharedAccessSignature sr=myhub.azure-devices.net%2fdevices&sig=JdyscqTpXdEJs49elIUCcohw2DlFDR3zfH5KqGJo4r4%3D&se=1456973447&skn=registryRead

## <a name="supported-x509-certificates"></a>Ondersteunde x.509-certificaten

U kunt een x.509-certificaat voor het verifiëren van een apparaat met IoT Hub. Dit omvat:

-   **Een bestaand x.509-certificaat**. Een apparaat mogelijk al een x.509-certificaat dat is gekoppeld. Het apparaat kan dit certificaat gebruiken voor verificatie met IoT Hub.

-   **Een X-509 automatisch gegenereerd en zelf-ondertekend certificaat**. Een fabrikant of een interne implementatie van deze certificaten en de bijbehorende persoonlijke sleutel (en het certificaat) op het apparaat opslaan. Kunt u hulpprogramma's zoals [OpenSSL] [ lnk-openssl] en [SelfSignedCertificate voor Windows] [ lnk-selfsigned] hulpprogramma voor dit doel.

-   **CA ondertekend x.509-certificaat**. U kunt ook een x.509-certificaat gegenereerd en ondertekend door een certificeringsinstantie (CA) gebruiken voor het identificeren van een apparaat en een apparaat met IoT Hub verifiëren.

Een apparaat kan gebruiken een x.509-certificaat of een beveiligingstoken voor verificatie, maar niet beide.

### <a name="register-an-x509-client-certificate-for-a-device"></a>Een x.509-certificaat voor een apparaat registreren

De [Azure IoT Service SDK voor C#] [ lnk-service-sdk] (versie 1.0.8+) ondersteunt de registratie van een apparaat dat gebruikmaakt van een x.509-certificaat voor verificatie. Andere API's zoals importeren/exporteren van apparaten ook ondersteunen x.509-certificaten.

### <a name="c-support"></a>C\# ondersteuning

De klasse **RegistryManager** bevat een programmatische manier om een apparaat registreren. In het bijzonder kunnen de methoden **AddDeviceAsync** en **UpdateDeviceAsync** registreren en bijwerken van een apparaat in het register Iot Hub apparaat-id van een gebruiker. Deze twee methoden nemen een exemplaar van het **apparaat** als invoer. De klasse van het **apparaat** bevat een eigenschap voor **verificatie** waarmee de gebruiker op te geven van de primaire en secundaire X.509-certificaat vingerafdrukken. De vingerafdruk vertegenwoordigt een SHA-1 hash van het x.509-certificaat (binaire DER-codering met opgeslagen). Gebruikers hebben de mogelijkheid om de vingerafdruk van een primaire of een secundaire vingerafdruk of beide opgeven. Primaire en secundaire vingerafdrukken worden verwerken certificaat rollover-scenario's ondersteund.

> [AZURE.NOTE] IoT Hub vereist of hele x.509-certificaat, alleen de vingerafdruk opslaan.

Hier volgt een voorbeeld van een C\# codefragment voor het registreren van een apparaat met behulp van een x.509-certificaat:

```
var device = new Device(deviceId)
{
  Authentication = new AuthenticationMechanism()
  {
    X509Thumbprint = new X509Thumbprint()
    {
      PrimaryThumbprint = "921BC9694ADEB8929D4F7FE4B9A3A6DE58B0790B"
    }
  }
};
RegistryManager registryManager = RegistryManager.CreateFromConnectionString(deviceGatewayConnectionString);
await registryManager.AddDeviceAsync(device);
```

### <a name="use-an-x509-client-certificate-during-runtime-operations"></a>Een x.509-certificaat tijdens de runtime gebruiken

De [Azure IoT apparaat SDK voor .NET] [ lnk-client-sdk] (versie 1.0.11+) ondersteunt het gebruik van x.509-certificaten.

### <a name="c-support"></a>C\# ondersteuning

De klasse **DeviceAuthenticationWithX509Certificate** ondersteunt het maken van instanties van  **DeviceClient** met behulp van een x.509-certificaat.

Dit is een monster codefragment:

```
var authMethod = new DeviceAuthenticationWithX509Certificate("<device id>", x509Certificate);

var deviceClient = DeviceClient.Create("<IotHub DNS HostName>", authMethod);
```

## <a name="custom-device-authentication"></a>Aangepast apparaat verificatie

Kunt u de IoT Hub [apparaat identiteit register] [ lnk-identity-registry] beveiligingsreferenties per apparaat configureren en toegangsbeheer met [tokens][lnk-sas-tokens]. Echter als een oplossing voor IoT al een aanzienlijke investering in een aangepast apparaat identiteit register en/of schema, kunt u integreren deze bestaande infrastructuur met IoT Hub door een *token service*maken. Op deze manier kunt u andere IoT-functies in de oplossing.

Een token is een aangepaste cloud-service. Wordt een IoT Hub *gedeelde-beleid* bij **DeviceConnect** machtigingen voor het maken van tokens *apparaat binnen het bereik* . Deze tokens kunnen een apparaat wilt aansluiten op uw hub-IoT.

  ![Stappen van het token service patroon][img-tokenservice]

Dit zijn de belangrijkste stappen van het token service patroon:

1. Een gedeelde IoT-Hub-beleid maken met machtigingen voor uw hub IoT **DeviceConnect** . U kunt dit beleid maken in de [portal Azure] [ lnk-management-portal] of via programmacode. Dit beleid de token service gebruikt voor het ondertekenen van de tokens die wordt gemaakt.
2. Wanneer een apparaat toegang hebben tot uw hub IoT moet, vraagt een ondertekende token om van uw token service. Het apparaat kan verifiëren met uw apparaat aangepaste identiteit register/verificatieschema bepalen de apparaat-id die de token service gebruikt voor het maken van het token.
3. Het token wordt een token. Het token wordt gemaakt met behulp van `/devices/{deviceId}` als `resourceURI`, met `deviceId` als het apparaat wordt geverifieerd. De token service gebruikmaakt van het beleid voor gedeelde toegang te maken van het token.
4. Het apparaat maakt gebruik van het token rechtstreeks met de IoT-hub.

> [AZURE.NOTE] U kunt de .NET-klasse [SharedAccessSignatureBuilder] [ lnk-dotnet-sas] of de Java-klasse [IotHubServiceSasToken] [ lnk-java-sas] voor het maken van een token in uw token service.

De token-service kan het token verlopen naar wens instellen. Als het token is verstreken, verbreekt de IoT hub aansluiting van het apparaat. Het apparaat moet vervolgens een nieuwe beveiligingssleutel aanvragen bij de token service. Als u een korte verlooptijd, verhoogt dit de belasting van het apparaat en de token service.

Voor een apparaat wilt aansluiten op uw hub, moet u nog steeds deze toevoegen aan het register IoT Hub apparaat identiteit, zelfs als het apparaat is een token en niet de sleutel voor een apparaat om verbinding te maken. Daarom kunt u blijven gebruiken toegangsbeheer per apparaat in- of uitschakelen van de apparaat-id's in het [register van IoT Hub identiteit] [ lnk-identity-registry] wanneer het apparaat wordt geverifieerd met een token. Dit vermindert de risico's van het gebruik van tokens met tijden lang verstrijken.

### <a name="comparison-with-a-custom-gateway"></a>Vergelijking met een aangepaste gateway

Het token service patroon is de aanbevolen manier voor het implementeren van een aangepaste identiteit register/verificatieschema met IoT Hub. Het wordt aanbevolen omdat IoT Hub blijft het grootste gedeelte van het verkeer van de oplossing. Er zijn echter gevallen waarin het aangepaste verificatieschema dus is verweven met het protocol dat een service verwerkt alle verkeer (*aangepaste gateway*) vereist is. Een voorbeeld hiervan is [Transport Layer Security (TLS) en vooraf gedeelde sleutels (PSKs)][lnk-tls-psk]. Zie voor meer informatie de [gateway protocol] [ lnk-protocols] onderwerp.

## <a name="reference-topics"></a>Naslaginformatie:

De volgende onderwerpen voor verwijzing bieden meer informatie over het beheren van toegang tot uw hub IoT.

## <a name="iot-hub-permissions"></a>Machtigingen voor IoT Hub

De volgende tabel worden de machtigingen die kunt u toegang tot uw hub IoT.

| Machtiging            | Notities |
| --------------------- | ----- |
| **RegistryRead**      | Verleent toegang tot het register van de identiteit apparaat lezen. Zie voor meer informatie, [apparaat id register][lnk-identity-registry]. |
| **RegistryReadWrite** | Subsidies lees- en schrijftoegang tot het register van de identiteit apparaat. Zie voor meer informatie, [apparaat id register][lnk-identity-registry]. |
| **ServiceConnect**    | Verleent toegang tot de cloud service gerichte communicatie- en eindpunten te controleren. Zo geeft het recht tot cloud back-end services aan apparaat-wolk-berichten ontvangen, wolk naar apparaat verzenden en ophalen van de overeenkomstige delivery bevestigingen. |
| **DeviceConnect**     | Verleent toegang tot apparaat gerichte communicatie eindpunten. Zo geeft het recht om het apparaat naar cloud-berichten verzenden en ontvangen van berichten van de wolk naar apparaat. Deze machtiging wordt gebruikt door apparaten. |

## <a name="additional-reference-material"></a>Aanvullend referentiemateriaal

Er zijn andere zoeken naar onderwerpen in de handleiding voor ontwikkelaars:

- [Eindpunten IoT Hub] [ lnk-endpoints] beschrijft de verschillende eindpunten die elke hub IoT beschikbaar worden gesteld voor de uitvoering en het beheer van bewerkingen.
- [Procesbeperking en quota's] [ lnk-quotas] de quota die van toepassing zijn op de IoT Hub-service en de bandbreedteregeling gedrag u kunt verwachten als u de service wordt beschreven.
- [SDK's IoT Hub apparaat- en] [ lnk-sdks] geeft een overzicht van de verschillende taal SDK's u een gebruiken, wanneer u zowel apparaat als service toepassingen ontwikkelt die samenwerken met IoT Hub.
- [Taal voor twins, methoden en taken query] [ lnk-query] de querytaal kunt u gegevens ophalen uit de IoT Hub over uw apparaat twins, methoden en taken beschreven.
- [Ondersteuning voor IoT Hub MQTT] [ lnk-devguide-mqtt] meer informatie over de IoT Hub ondersteuning biedt voor het MQTT-protocol.

## <a name="next-steps"></a>Volgende stappen

Nu u hebt geleerd hoe toegang IoT Hub, kunt u mogelijk geïnteresseerd in de volgende onderwerpen in de handleiding voor ontwikkelaars:

- [Twins apparaat gebruiken voor het synchroniseren van staat en configuraties][lnk-devguide-device-twins]
- [Een directe methode aanroept op een apparaat][lnk-devguide-directmethods]
- [Taken plannen op meerdere apparaten][lnk-devguide-jobs]

Als u proberen sommige van de concepten die in dit artikel worden beschreven wilt, kunt u mogelijk geïnteresseerd in de volgende zelfstudies voor IoT Hub:

- [Aan de slag met Azure IoT Hub][lnk-getstarted-tutorial]
- [Het verzenden van berichten met IoT Hub cloud naar apparaat][lnk-c2d-tutorial]
- [Het verwerken van IoT Hub apparaat-wolk-berichten][lnk-d2c-tutorial]

<!-- links and images -->

[img-tokenservice]: ./media/iot-hub-devguide-security/tokenservice.png
[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-openssl]: https://www.openssl.org/
[lnk-selfsigned]: https://technet.microsoft.com/library/hh848633

[lnk-resource-provider-apis]: https://msdn.microsoft.com/library/mt548492.aspx
[lnk-sas-tokens]: iot-hub-devguide-security.md#security-tokens
[lnk-amqp]: https://www.amqp.org/
[lnk-azure-resource-manager]: ../azure-resource-manager/resource-group-overview.md
[lnk-cbs]: https://www.oasis-open.org/committees/download.php/50506/amqp-cbs-v1%200-wd02%202013-08-12.doc
[lnk-event-hubs-publisher-policy]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-99ce67ab
[lnk-management-portal]: https://portal.azure.com
[lnk-sasl-plain]: http://tools.ietf.org/html/rfc4616
[lnk-identity-registry]: iot-hub-devguide-identity-registry.md
[lnk-dotnet-sas]: https://msdn.microsoft.com/library/microsoft.azure.devices.common.security.sharedaccesssignaturebuilder.aspx
[lnk-java-sas]: http://azure.github.io/azure-iot-sdks/java/service/api_reference/com/microsoft/azure/iot/service/auth/IotHubServiceSasToken.html
[lnk-tls-psk]: https://tools.ietf.org/html/rfc4279
[lnk-protocols]: iot-hub-protocol-gateway.md
[lnk-custom-auth]: iot-hub-devguide-security.md#custom-device-authentication
[lnk-x509]: iot-hub-devguide-security.md#supported-x509-certificates
[lnk-devguide-device-twins]: iot-hub-devguide-device-twins.md
[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-service-sdk]: https://github.com/Azure/azure-iot-sdks/tree/master/csharp/service
[lnk-client-sdk]: https://github.com/Azure/azure-iot-sdks/tree/master/csharp/device
[lnk-device-explorer]: https://github.com/Azure/azure-iot-sdks/blob/master/tools/DeviceExplorer/doc/how_to_use_device_explorer.md

[lnk-getstarted-tutorial]: iot-hub-csharp-csharp-getstarted.md
[lnk-c2d-tutorial]: iot-hub-csharp-csharp-c2d.md
[lnk-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md
