# <a name="securing-your-iot-deployment"></a>De IoT-implementatie beveiligen

Dit artikel bevat de volgende detailniveau voor het beveiligen van de infrastructuur op basis van Azure IoT Internet van dingen (IoT). Koppelingen naar niveau implementatiegegevens voor het configureren en implementeren van elk onderdeel. Het biedt ook vergelijkingen en de keuze tussen verschillende concurrerende methoden.

De IoT Azure-implementatie beveiligen kan worden onderverdeeld in de volgende drie beveiligingsgebieden:

- **Apparaat beveiliging**: het IoT apparaat beveiligen terwijl deze is geïmplementeerd in het wild.
- **Verbindingsbeveiliging**: ervoor te zorgen dat alle gegevens die worden verzonden tussen de IoT apparaat en IoT Hub is vertrouwelijk en fraudebestendig.
- **Cloud beveiliging**: de mogelijkheid om gegevens te beveiligen tijdens het doorlopen en wordt opgeslagen in de cloud.

![Drie beveiligingsgebieden][img-overview]

## <a name="secure-device-provisioning-and-authentication"></a>Verificatie en veilige apparaten inrichten

De Suite Azure IoT beveiligt IoT apparaten door de volgende twee methoden:

- Door middel van een unieke id sleutel (beveiligingstokens) voor elk apparaat door het apparaat kan worden gebruikt om te communiceren met de IoT-Hub.

- Met behulp van een op-apparaat [x.509-certificaat] [ lnk-x509] en de persoonlijke sleutel als middel voor het verifiëren van het apparaat op de IoT Hub. Deze verificatiemethode zorgt ervoor dat de persoonlijke sleutel op het apparaat niet bekend is buiten het apparaat op elk gewenst moment, een hogere mate van beveiliging bieden.

De methode token biedt verificatie voor elke aanroep van het apparaat met IoT Hub door het koppelen van de symmetrische sleutel voor elke aanroep. Verificatie op basis van de X.509 kunt verificatie van een IoT apparaat op de fysieke laag als onderdeel van de inrichting TLS verbinding. De security tokens gebaseerde methode kan worden gebruikt zonder de x.509-verificatie is een patroon minder veilig. De keuze tussen de twee methoden wordt voornamelijk bepaald door hoe veilig apparaat verificatie moet worden en de beschikbaarheid van veilige opslag op het apparaat (voor het veilig opslaan van de persoonlijke sleutel).

## <a name="iot-hub-security-tokens"></a>Beveiligingstokens IoT Hub

IoT-Hub maakt gebruik van beveiligingstokens te verifiëren, apparaten en services om te voorkomen dat toetsen verzenden via het netwerk. Bovendien zijn beveiligingstokens beperkte geldigheid en de scope. Tokens Azure IoT Hub SDK's automatisch genereren zonder dat u een speciale configuratie vereist. Sommige scenario vereist echter dat de gebruiker te genereren en rechtstreeks gebruik van beveiligingstokens. Deze omvatten het rechtstreekse gebruik van de MQTT, AMQP of HTTP-oppervlakken, of voor de uitvoering van het token service patroon.

In de volgende artikelen vindt u meer informatie over de structuur van het beveiligingstoken en het gebruik ervan:

-   [Security token-structuur][lnk-security-tokens]
-   [SAS-tokens gebruiken als een apparaat][lnk-sas-tokens]

Elke IoT Hub heeft een [Apparaat identiteit register] [ lnk-identity-registry] waarmee u resources per apparaat maken in de service, zoals een wachtrij waarin berichten tijdens een vlucht wolk naar apparaat, en toegang tot het apparaat gerichte eindpunten. Het register van de identiteit IoT Hub biedt een veilige opslag van apparaat-id's en sleutels voor een oplossing. Afzonderlijke of groepen van apparaat-id's kunnen worden toegevoegd aan een lijst of een lijst met geblokkeerde volledige controle over het Apparaattoegang inschakelen. De volgende artikelen bieden meer informatie over de structuur van het register van de identiteit apparaat en bewerkingen ondersteund.

[IoT Hub biedt ondersteuning voor protocollen zoals MQTT, AMQP en HTTP-][lnk-protocols]. Elk van deze protocollen anders beveiligingstokens uit de IoT apparaat met IoT Hub gebruiken:

- AMQP: SASL PLAIN en AMQP op Claims gebaseerde beveiliging ({policyName}@sas.root.{iothubName} voor tokens Hub niveau; {deviceId} bij een apparaat binnen het bereik van tokens).

- MQTT: Pakket gebruik {deviceId} VERBINDEN als {ClientId}, {IoThubhostname} / {deviceId} in het veld **gebruikersnaam** en een token SAS in het veld **wachtwoord** .

- HTTP: Geldig token wordt in de vergunning.

Register IoT Hub apparaat-id kan worden gebruikt voor het configureren van beveiligingsreferenties per apparaat en toegangsbeheer. Echter, als er een oplossing voor IoT al een aanzienlijke investering in een [aangepast apparaat identiteit register en/of schema][lnk-custom-auth], deze kan worden geïntegreerd in een bestaande infrastructuur met IoT Hub door een token service maken.

### <a name="x509-certificate-based-device-authentication"></a>X.509-apparaat voor op certificaten gebaseerde verificatie

Het gebruik van een [x.509-certificaat op basis van het apparaat] [ lnk-use-x509] en de bijbehorende persoonlijke en openbare sleutelpaar kan extra verificatie bij de fysieke laag. De persoonlijke sleutel worden opgeslagen in het apparaat en niet buiten het apparaat detecteerbaar is. Het x.509-certificaat bevat informatie over het apparaat, zoals apparaat-ID, en andere organisatorische details. Een handtekening van het certificaat wordt met de persoonlijke sleutel gegenereerd.

Provisioning stroom op hoog niveau apparaat:

- Een fysiek apparaat-id apparaat en/of een x.509-certificaat dat is gekoppeld aan het apparaat tijdens de productie of de inbedrijfstelling van een id koppelen.

- Maak een bijbehorende vermelding van de identiteit in IoT Hub – apparaat identiteit en de gegevens in het register IoT Hub apparaat gekoppeld apparaat.

- X.509-certificaatvingerafdruk veilig opgeslagen in het register van de IoT Hub apparaat.

### <a name="root-certificate-on-device"></a>Basiscertificaat op apparaat

Tijdens het maken van een beveiligde TLS verbinding met IoT Hub, verifieert de IoT apparaat IoT Hub met behulp van een basiscertificaat dat deel van de SDK van het apparaat uitmaakt. Voor de C-client SDK het certificaat bevindt zich in de map "\\c\\certificaten" onder de hoofdmap van de repo. Hoewel deze hoofdcertificaten lange levensduur zijn, ze nog steeds kunnen verlopen of worden ingetrokken. Als er geen enkele manier van het bijwerken van het certificaat op het apparaat, het apparaat mogelijk niet vervolgens verbinding maken met de IoT Hub (of een andere cloud-service). Met een middel voor het bijwerken van het basiscertificaat zodra het IoT apparaat is geïmplementeerd, wordt dit risico effectief beperken.

## <a name="securing-the-connection"></a>Beveiligen van de verbinding

Internet-verbinding tussen het IoT apparaat en IoT Hub wordt beveiligd met behulp van de standaard Transport Layer Security (TLS). Azure IoT [TLS 1.2]ondersteunt[lnk-tls12], TLS 1.1 en TLS 1.0, in deze volgorde. TLS 1.0 wordt ondersteund alleen voor achterwaartse compatibiliteit. Het verdient aanbeveling gebruik van TLS 1.2 omdat het de beste beveiliging biedt.

Azure IoT Suite ondersteunt de volgende Suites Cipher, in deze volgorde.

| Cipher-Suite | Lengte |
|--------------|--------|
| TLS\_ECDHE\_RSA\_WITH\_AES\_256\_CBC\_SHA384 (0xc028) ECDH secp384r1 (eq. FS 7680 bits RSA) | 256    |
| TLS\_ECDHE\_RSA\_WITH\_AES\_128\_CBC\_SHA256 (0xc027) ECDH secp256r1 (eq. FS 3072 bits RSA) | 128    |
| TLS\_ECDHE\_RSA\_WITH\_AES\_256\_CBC\_SHA (0xc014) ECDH secp384r1 (eq. FS 7680 bits RSA)           | 256    |
| TLS\_ECDHE\_RSA\_WITH\_AES\_128\_CBC\_SHA (0xc013) ECDH secp256r1 (eq. FS 3072 bits RSA)           | 128    |
| TLS\_RSA\_WITH\_AES\_256\_GCM\_SHA384 (0x9d) | 256    |
| TLS\_RSA\_WITH\_AES\_128\_GCM\_SHA256 (0x9c) | 128    |
| TLS\_RSA\_WITH\_AES\_256\_CBC\_SHA256 (0x3d) | 256    |
| TLS\_RSA\_WITH\_AES\_128\_CBC\_SHA256 (0x3c) | 128    |
| TLS\_RSA\_WITH\_AES\_256\_CBC\_SHA (0x35)    | 256    |
| TLS\_RSA\_WITH\_AES\_128\_CBC\_SHA (0x2f)    | 128    |
| TLS\_RSA\_WITH\_3DES\_EDE\_CBC\_SHA (0xa)    | 112    |

## <a name="securing-the-cloud"></a>Beveiligen van de cloud

Azure IoT Hub kan de definitie van [het beleid voor toegang] [ lnk-protocols] voor elke beveiligingssleutel. Wordt de volgende reeks machtigingen toegang tot elk van de eindpunten IoT-Hub. Machtigingen beperken de toegang tot een IoT Hub op basis van functionaliteit.

- **RegistryRead**. Verleent leestoegang aan de apparaat-id-register. Zie voor meer informatie, [Apparaat id register][lnk-identity-registry].

- **RegistryReadWrite**. Subsidies lees- en schrijftoegang tot het register van de identiteit apparaat. Zie voor meer informatie, [Apparaat id register][lnk-identity-registry].

- **ServiceConnect**. Verleent toegang tot de cloud service gerichte communicatie- en eindpunten te controleren. Zo geeft het recht tot cloud back-end services aan apparaat-wolk-berichten ontvangen, wolk naar apparaat verzenden en ophalen van de overeenkomstige delivery bevestigingen.

- **DeviceConnect**. Verleent toegang tot apparaat gerichte communicatie eindpunten. Zo geeft het recht om het apparaat naar cloud-berichten verzenden en ontvangen van berichten van de wolk naar apparaat. Deze machtiging wordt gebruikt door apparaten.

Er zijn twee manieren om toegang te verkrijgen **DeviceConnect** machtigingen met IoT Hub met [beveiligingstokens][lnk-sas-tokens]: met een sleutel apparaat identiteit of gedeelde toegang beleidssleutel. Bovendien is het belangrijk te weten dat alle functionaliteit die toegankelijk is vanaf apparaten wordt blootgesteld aan het ontwerp op de eindpunten met het voorvoegsel `/devices/{deviceId}`.

[Service-onderdelen kunnen alleen genereren voor beveiligingstokens] [ lnk-service-tokens] met behulp van gedeelde-beleid de juiste machtigingen te verlenen.

Azure IoT-Hub en andere services die deel van de oplossing uitmaken kunnen kunnen beheer van Azure Active Directory-gebruikers.

Gegevens binnenkrijgen Azure IoT Hub kan worden gebruikt door diverse services zoals Azure Stream Analytics en Azure blob-opslag. Deze services toestaan beheertoegang. Voor meer informatie over deze services en de onderstaande opties beschikbaar:

- [Azure DocumentDB][lnk-docdb]: een schaalbare, volledig geïndexeerd databaseservice voor semi-gestructureerde gegevens dat metagegevens voor de apparaten beheert u inrichten, zoals kenmerken, configuratie en eigenschappen. DocumentDB biedt hoge prestaties en hoge gegevensdoorvoer verwerking, schema agnostic het indexeren van gegevens en een uitgebreide interface voor SQL-query.

- [Azure Stream Analytics][lnk-asa]: realtime stream processing in de cloud, waarmee u snel ontwikkelen en implementeren van een oplossing voor lage kosten analytics om real-time inzicht van apparaten, sensoren, infrastructuur en toepassingen aan het licht komen. De gegevens van deze service volledig beheerd kunnen worden aangepast aan elk volume en de nog steeds hoge doorvoer, lage latentie en tolerantie.

- [Azure App Services][lnk-appservices]: een wolk platform te bouwen van krachtige web- en mobiele toepassingen die verbinding met gegevens in een willekeurige plaats maken; in de cloud of op locatie. Bouw mobiele apps voor iOS, Android en Windows uitoefenen. Integreren met uw Software als een Service (SaaS) en enterprise toepassingen out-of-the-box connectiviteit tot tientallen cloud-gebaseerde services en zakelijke toepassingen. De code in uw favoriete taal en IDE (.NET, NodeJS, PHP, Python of Java) bouwen API's en web apps sneller dan ooit.

- [Logica Apps][lnk-logicapps]: de logica Apps functie van Azure App Service helpt uw IoT oplossing om uw bestaande line of business-systemen te integreren en automatiseren van workflow-processen. Logica Apps kan ontwikkelaars om workflows te ontwerpen die van een trigger starten en vervolgens een aantal stappen uitvoeren, regels en acties die krachtige verbindingslijnen gebruiken om met uw bedrijfsprocessen te integreren. Logica Apps biedt out-of-the-box-connectiviteit met een enorme ecosysteem van SaaS, cloud-gebaseerde en toepassingen in gebouwen.

- [Azure blob-opslag][lnk-blob]: betrouwbare, voordelige cloud opslag van de gegevens die uw apparaten naar de cloud verzendt.

## <a name="conclusion"></a>Conclusie

In dit artikel overzicht van uitvoering niveau details voor het ontwerpen en implementeren van een IoT infrastructuur met Azure IoT. Configureren van elk onderdeel worden beveiligd, is de sleutel in de algehele IoT-infrastructuur te beveiligen. Het ontwerp mogelijkheden voor IoT Azure een bepaalde mate van flexibiliteit en keuze; elke keuze hebben echter invloed op de beveiliging. Het is raadzaam om elk van deze opties worden geëvalueerd door middel van een evaluatie van de risico's / kosten.

[img-overview]: media/iot-secure-your-deployment/overview.png

[lnk-security-tokens]: ../articles/iot-hub/iot-hub-devguide-security.md#security-token-structure
[lnk-sas-tokens]: ../articles/iot-hub/iot-hub-devguide-security.md#use-sas-tokens-as-a-device
[lnk-identity-registry]: ../articles/iot-hub/iot-hub-devguide-identity-registry.md
[lnk-protocols]: ../articles/iot-hub/iot-hub-devguide-security.md
[lnk-custom-auth]: ../articles/iot-hub/iot-hub-devguide-security.md#custom-device-authentication
[lnk-x509]: http://www.itu.int/rec/T-REC-X.509-201210-I/en
[lnk-use-x509]: ../articles/iot-hub/iot-hub-devguide-security.md
[lnk-tls12]: https://tools.ietf.org/html/rfc5246
[lnk-service-tokens]: ../articles/iot-hub/iot-hub-devguide-security.md#using-security-tokens-from-service-components
[lnk-docdb]: https://azure.microsoft.com/services/documentdb/
[lnk-asa]: https://azure.microsoft.com/services/stream-analytics/
[lnk-appservices]: https://azure.microsoft.com/services/app-service/
[lnk-logicapps]: https://azure.microsoft.com/services/app-service/logic/
[lnk-blob]: https://azure.microsoft.com/services/storage/
