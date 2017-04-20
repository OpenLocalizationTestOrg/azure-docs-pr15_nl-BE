<properties
 pageTitle="Overzicht van Azure IoT Hub | Microsoft Azure"
 description="Overzicht van de Azure IoT Hub: Wat is iot hub, apparaat verbinding, internet van dingen communicatiepatronen en service assisted communicatiepatroon"
 services="iot-hub"
 documentationCenter=""
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="get-started-article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="08/25/2016"
 ms.author="dobett"/>

# <a name="what-is-azure-iot-hub"></a>Wat is Azure IoT Hub?

Welkom bij Azure IoT-Hub. Dit artikel biedt een overzicht van Azure IoT Hub en wordt beschreven waarom u deze service moet gebruiken voor het implementeren van een oplossing voor Internet dingen (IoT). Azure IoT Hub is een volledig beheerde service waarmee een betrouwbare en veilige bidirectionele communicatie tussen miljoenen IoT apparaten en een oplossing voor back-end. Azure IoT Hub:

- Biedt betrouwbare apparaat naar cloud en cloud-to-device berichtenverkeer op schaal.
- Hiermee activeert u beveiligde communicatie met de beveiligingsreferenties per apparaat en toegangsbeheer.
- Biedt uitgebreide controle voor apparaat verbinding en gebeurtenissen voor apparaat identiteit.
- Bevat de apparaatbibliotheken voor de meest populaire talen en platformen.

Het artikel [vergelijking IoT Hub en gebeurtenis Hubs] [ lnk-compare] beschrijft de belangrijkste verschillen tussen deze twee services en markeert u de voordelen van het gebruik van IoT Hub in IoT oplossingen.

![Azure IoT Hub als cloud-gateway in het internet van dingen oplossing][img-architecture]

> [AZURE.NOTE] Zie de [Microsoft Azure IoT referentiearchitectuur]voor uitvoerige informatie van IoT-architectuur,[lnk-refarch].

## <a name="iot-device-connectivity-challenges"></a>IoT apparaat verbinding uitdagingen

IoT Hub en het apparaatbibliotheken helpen u om te voldoen aan de uitdagingen van de betrouwbare en veilige wijze apparaten aansluiten op de oplossing voor back-end. IoT apparaten:

- Zijn vaak embedded-systemen met geen menselijke operator.
- Op externe locaties, waar de fysieke toegang kostbaar is kan zijn.
- Mogelijk alleen bereikbaar via de oplossing voor back-end.
- Mogelijk beperkt vermogen en verwerkingsbronnen.
- Mogelijk verbinding met het netwerk af en toe, langzaam en duur.
- Wellicht moet u eigen, aangepaste en branchespecifieke toepassingsprotocollen gebruiken.
- U kunt maken met behulp van een grote reeks populaire hardware- en software platforms.

Naast de vereisten, moet ook een oplossing IoT leveren schaal, beveiliging en betrouwbaarheid. De resulterende reeks vereisten voor connectiviteit is moeilijk en tijdrovend te implementeren wanneer u traditionele technologieën, zoals web-containers en messaging makelaars.

## <a name="why-use-azure-iot-hub"></a>Waarom Azure IoT Hub gebruiken

De uitdagingen van het apparaat verbinding betrekking Azure IoT Hub op de volgende manieren:

-   **Verificatie per apparaat en beveiligde verbindingen**. U kunt elk apparaat met een eigen [beveiligingssleutel] inrichten[ lnk-devguide-security] om het te verbinden met IoT Hub. De [IoT Hub identiteit register] [ lnk-devguide-identityregistry] apparaat-id's en sleutels worden opgeslagen in een oplossing. Een oplossing voor back-end kunt afzonderlijke apparaten wilt toestaan of weigeren van lijsten om volledige controle over het Apparaattoegang toevoegen.

-   **Controle van de verrichtingen van apparaat verbinding**. U kunt gedetailleerde logboeken over apparaat identity management bewerkingen en apparaat verbinding gebeurtenissen ontvangen. Deze controle mogelijkheid staat uw oplossing IoT verbindingsproblemen, te identificeren, zoals de apparaten die verbinding maken met de verkeerde referenties proberen te vaak berichten verzenden of alle berichten van cloud-to-device afwijzen.

-   **Een uitgebreide reeks apparaatbibliotheken**. [Azure IoT apparaat SDK's] [ lnk-device-sdks] zijn beschikbaar en voor verschillende talen en platforms--C voor veel distributies van Linux, Windows en real-time besturingssystemen wordt ondersteund. Azure IoT apparaat SDK's bieden ook ondersteuning voor beheerde talen, zoals C#, Java en JavaScript.

-   **IoT protocollen en uitbreidbaarheid**. Als uw oplossing niet kan het apparaatbibliotheken, beschrijft de IoT Hub een openbaar protocol waarmee apparaten gebruik kunnen de MQTT v3.1.1, HTTP 1.1 of 1.0 AMQP protocollen. U kunt ook IoT Hub om ondersteuning te bieden voor aangepaste protocollen die door uitbreiden:

    - Een gateway veld maken met de [SDK van Azure IoT Gateway] [ lnk-gateway-sdk] die uw aangepaste protocol wordt geconverteerd naar een van de drie protocollen IoT Hub te begrijpen. 
    - Aanpassen van de [gateway van Azure IoT-protocol][protocol-gateway], een open-source onderdeel dat wordt uitgevoerd in de cloud.

-   **Schaal**. Azure IoT Hub wordt geschaald naar miljoenen gelijktijdig aangesloten apparaten en miljoenen gebeurtenissen per seconde.

Deze voordelen zijn voor veel communicatiepatronen. IoT Hub kunt op dit moment u de volgende specifieke communicatiepatronen te implementeren:

-   **Gebeurtenis gebaseerde apparaat-wolk-opname.** IoT Hub kan op betrouwbare wijze miljoenen gebeurtenissen per seconde ontvangen van uw apparaten. Deze kan vervolgens deze op uw hot pad verwerken met behulp van een gebeurtenis processor motor. Dit kan op te slaan op uw koude pad voor analyse. IoT Hub behoudt de gebeurtenisgegevens gedurende zeven dagen gegarandeerd betrouwbare verwerking en pieken in de belasting te absorberen.

-   * *Reliable messaging wolk naar apparaat (of *opdrachten*). ** de oplossing voor back-end IoT Hub kunt gebruiken om berichten te verzenden met een garantie van bezorging in de minst eens naar de afzonderlijke apparaten. Elk bericht heeft een afzonderlijke instelling voor time-to-live, en de back-end ontvangsten van levering en de vervaldatum kunt aanvragen. Deze bevestigingen zorgen volledig inzicht in de levenscyclus van een bericht van de wolk naar apparaat. Vervolgens kunt u de bedrijfslogica bewerkingen die worden uitgevoerd op apparaten met implementeren.

-   **Bestanden en sensorgegevens in de cache uploaden naar de cloud.** De apparaten kunnen bestanden uploaden naar Azure opslag met SAS-URI's beheerd door IoT Hub. IoT Hub kunt meldingen genereren bij het ontvangen van bestanden in de cloud, zodat u de back-end om ze te verwerken.

## <a name="gateways"></a>Gateways

Een gateway in een oplossing voor IoT is meestal een [gateway protocol] [ lnk-gateway] die wordt geïmplementeerd in de cloud of een [veld gateway] [ lnk-field-gateway] die lokaal is geïmplementeerd met uw apparaten. Een gateway protocol uitvoert vertaling protocol, bijvoorbeeld MQTT op AMQP. Een veld gateway kunt u analytics uitvoeren op de rand, tijdgevoelige beslissingen latentie te verlagen, device management services bieden, afdwingen van beveiliging en privacy beperkingen en vertaling protocol ook uitvoeren. Beide typen gateway fungeren als schakels tussen uw apparaten en de hub IoT.

Een gateway veld verschilt van een eenvoudige verkeer routing-apparaat (zoals een apparaat network address translation of firewall) omdat normaal gesproken een actieve rol voert in het beheer van toegang en informatie stroom in de oplossing.

Een oplossing kan gateways protocol en de veld bevatten.

## <a name="how-does-iot-hub-work"></a>Hoe werkt de IoT Hub?

Azure IoT Hub implementeert de [service assisted communicatie] [ lnk-service-assisted-pattern] patroon om de interactie tussen uw apparaten en uw oplossing voor back-end bemiddelen. Het doel van de service assisted communicatie is vast te stellen, betrouwbare, bidirectionele communicatiepaden tussen een controlesysteem, zoals IoT Hub en speciale apparaten die zijn geïmplementeerd in niet-vertrouwde fysieke ruimte. Het patroon worden de volgende beginselen:

- Beveiliging heeft voorrang op alle andere mogelijkheden.
- Apparaten accepteren geen ongevraagde netwerkgegevens. Een apparaat worden alle verbindingen en routes op een manier alleen uitgaande. Het apparaat voor een apparaat voor het ontvangen van een opdracht van de back-end moet regelmatig een verbinding controleren voor opdrachten voor het verwerken van in behandeling starten.
- Apparaten moeten alleen verbinding maken met of routes naar bekende services die ze zijn peered, zoals IoT Hub tot stand brengen.
- De communicatie tussen het apparaat en welke service of apparaat en gateway wordt op de toepassingslaag van het protocol beveiligd.
- Verificatie en autorisatie op systeemniveau zijn gebaseerd op per apparaat identiteiten. Zij maken-referenties en machtigingen vrijwel direct terughalen.
- Bidirectionele communicatie voor apparaten die verbinding sporadisch vanwege stroomuitval of connectiviteit problemen maken wordt vergemakkelijkt door opdrachten en apparaatmeldingen ingedrukt totdat er een apparaat is aangesloten als u wilt ontvangen. IoT Hub onderhoudt de apparaat-specifieke wachtrijen voor de opdrachten verzendt.
- Toepassingsgegevens payload, afzonderlijk voor beveiligde doorvoer via gateways naar een bepaalde service is beveiligd.

De mobiele industrie heeft de service assisted communicatiepatroon gebruikt op enorme schaal push notification services zoals [Windows Push Notification Services]implementeren[lnk-wns], [Google Cloud Messaging][lnk-google-messaging], en [Apple Push Notification Service][lnk-apple-push].

IoT Hub wordt ondersteund via openbare peering pad van ExpressRoute.

## <a name="next-steps"></a>Volgende stappen

Zie voor informatie over hoe Azure IoT Hub maakt op standaarden gebaseerde IoT device management voor u op afstand beheren, configureren en bijwerken van uw apparaten [Apparaatbeheer overzicht Azure IoT Hub][lnk-device-management].

Om de clienttoepassingen worden geïmplementeerd op een groot aantal besturingssystemen en hardwareplatforms apparaat, kunt u het apparaat IoT SDK's. De IoT apparaat SDK's bevatten bibliotheken die de verzendende Telemetrie en een hub IoT cloud-to-device opdrachten ontvangt te vergemakkelijken. Als u de SDK's gebruikt, kunt u uit verschillende netwerkprotocollen communiceren met IoT Hub. Voor meer informatie, Zie de [informatie over het apparaat SDK's][lnk-device-sdks].

Om te beginnen wat code schrijven en uitvoeren van voorbeelden zien het [aan de slag met IoT Hub] [ lnk-get-started] zelfstudie.

[img-architecture]: media/iot-hub-what-is-iot-hub/hubarchitecture.png


[lnk-get-started]: iot-hub-csharp-csharp-getstarted.md
[protocol-gateway]: https://github.com/Azure/azure-iot-protocol-gateway/blob/master/README.md
[lnk-service-assisted-pattern]: http://blogs.msdn.com/b/clemensv/archive/2014/02/10/service-assisted-communication-for-connected-devices.aspx "Service Assisted communicatie, blogbericht door Clemens Vasters"
[lnk-compare]: iot-hub-compare-event-hubs.md
[lnk-gateway]: iot-hub-protocol-gateway.md
[lnk-field-gateway]: iot-hub-devguide-endpoints.md#field-gateways
[lnk-devguide-identityregistry]: iot-hub-devguide-identity-registry.md
[lnk-devguide-security]: iot-hub-devguide-security.md
[lnk-wns]: https://msdn.microsoft.com/library/windows/apps/mt187203.aspx
[lnk-google-messaging]: https://developers.google.com/cloud-messaging/
[lnk-apple-push]: https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW9
[lnk-device-sdks]: https://github.com/Azure/azure-iot-sdks
[lnk-refarch]: http://download.microsoft.com/download/A/4/D/A4DAD253-BC21-41D3-B9D9-87D2AE6F0719/Microsoft_Azure_IoT_Reference_Architecture.pdf
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk
[lnk-device-management]: iot-hub-device-management-overview.md
