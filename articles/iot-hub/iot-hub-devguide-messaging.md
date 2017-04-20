<properties
 pageTitle="Handleiding voor ontwikkelaars - messaging | Microsoft Azure"
 description="Azure IoT Hub developer guide - apparaat naar cloud en wolk naar apparaat messaging"
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

# <a name="send-and-receive-messages-with-iot-hub"></a>Berichten verzenden en ontvangen met IoT Hub

## <a name="overview"></a>Overzicht

IoT Hub biedt de volgende messaging primitieven te communiceren met een apparaat:

- [Apparaat-cloud-] [ lnk-d2c] back-end van een apparaat naar een toepassing.
- [Cloud-to-device] [ lnk-c2d] back-end (*service* of *cloud*) van een toepassing.

Core-eigenschappen van IoT Hub berichtenfunctionaliteit zijn betrouwbaarheid en duurzaamheid van de berichten. Deze eigenschappen kunnen veerkracht aan intermitterende verbindingen aan de kant van het apparaat, en voor het laden van pieken in de gebeurtenis verwerkt aan de kant van de wolk. IoT Hub implementeert *ten minste eenmaal* levering garanties voor zowel apparaat-wolk-wolk naar apparaat messaging.

IoT Hub ondersteunt meerdere [protocollen apparaat gerichte] [ lnk-protocols] (zoals MQTT, AMQP en HTTP). Ter ondersteuning van naadloze interoperabiliteit via protocollen, IoT Hub definieert een [gemeenschappelijke indeling] [ lnk-message-format] die alle apparaat gerichte protocollen ondersteunen.

IoT Hub beschrijft een [gebeurtenis Hub-compatibele eindpunt] [ lnk-compatible-endpoint] back-end-toepassingen het apparaat naar cloud-berichten ontvangen door de hub inschakelen.

### <a name="when-to-use"></a>Wanneer gebruikt u

Instant Messaging is een mogelijkheid core van IoT Hub. Gebruik deze optie wanneer u wilt berichten verzenden van uw apparaat naar uw back-end of verzenden van berichten naar een apparaat uit uw back-end.

Zie voor een vergelijking van de IoT Hub en Hubs gebeurtenis [vergelijking IoT Hub en gebeurtenis Hubs][lnk-compare].

## <a name="device-to-cloud-messages"></a>Apparaat-wolk-berichten

U apparaat-wolk-berichten verzenden via een gerichte apparaat eindpunt (**{deviceId} /devices/ berichten/gebeurtenissen**). Uw back-end-service ontvangt berichten via een gerichte service-eindpunt (**/messages/events**) die compatibel is met de [Gebeurtenis Hubs]apparaat-cloud-[lnk-event-hubs]. Daarom kunt u standaard- [gebeurtenis Hubs integratie en SDK's] [ lnk-compatible-endpoint] apparaat-wolk-berichten ontvangen.

IoT Hub implementeert apparaat voor cloud messaging op een manier die vergelijkbaar is met de [Gebeurtenis Hubs][lnk-event-hubs]. IoT-Hub apparaat naar cloud berichten lijken meer op Hubs gebeurtenis *gebeurtenissen* dan [Service Bus] [ lnk-servicebus] *berichten*.

Deze implementatie heeft de volgende gevolgen:

* Op dezelfde manier op gebeurtenissen gebeurtenis Hubs, apparaat-wolk-berichten zijn duurzaam en bewaard in een hub IoT gedurende zeven dagen (Zie [configuratieopties apparaat naar cloud][lnk-d2c-configuration]).
* Apparaat-wolk-berichten worden gepartitioneerd in een vaste set van partities die is ingesteld bij het maken (Zie [configuratieopties apparaat naar cloud][lnk-d2c-configuration]).
* Analogously met gebeurtenis-Hubs, moeten apparaat-wolk-berichten lezen clients omgaan met partities en controlepuntbeheer. Zie de [Gebeurtenis Hubs - gebeurtenissen die][lnk-event-hubs-consuming-events].
* Net als gebeurtenissen gebeurtenis Hubs, apparaat-wolk-berichten mag niet meer dan 256 KB en kunnen worden gegroepeerd in batches verzenden optimaliseren. Batches mag niet meer dan 256 KB en ten hoogste 500 berichten.

Er zijn echter enkele belangrijke verschillen tussen de IoT Hub apparaat voor cloud messaging en Hubs van gebeurtenis:

* Zoals uiteengezet in de [toegang tot de IoT Hub] [ lnk-devguide-security] sectie IoT Hub kan per apparaat verificatie en toegangsbeheer.
* IoT Hub kan miljoenen gelijktijdig aangesloten apparaten ( [quota en procesbeperking]Zie[lnk-quotas]), terwijl Hubs gebeurtenis beperkt tot 5000 AMQP verbindingen per naamruimte is.
* IoT Hub kan geen willekeurige partitioneren met behulp van een **PartitionKey**. Apparaat-wolk-berichten worden gepartitioneerd op basis van hun oorspronkelijke **deviceId**.
* Schalen IoT Hub is iets anders dan de gebeurtenis Hubs schalen. Voor meer informatie Zie [Schalen IoT Hub][lnk-guidance-scale].

> [AZURE.NOTE] U kunt IoT Hub voor gebeurtenis Hubs in alle scenario's niet vervangen. Bijvoorbeeld in sommige berekeningen gebeurtenis verwerken, deze mogelijk opnieuw te partitioneren gebeurtenissen met betrekking tot een andere eigenschap of een veld voor het analyseren van de gegevensstromen. In dit scenario kunt u een gebeurtenis Hub loskoppelen van twee gedeelten van de stream processing pipeline. Voor meer informatie Zie *partities* in [Azure gebeurtenis Hubs overzicht][lnk-eventhub-partitions].

Zie voor meer informatie over het gebruik van apparaat voor cloud messaging, [IoT Hub-API's en SDK's][lnk-sdks].

> [AZURE.NOTE] Als u apparaat-wolk-berichten verzenden met HTTP, namen van eigenschappen en waarden alleen ASCII-alfanumerieke tekens bevatten, plus ``{'!', '#', '$', '%, '&', "'", '*', '*', '+', '-', '.', '^', '_', '`', '|', '~'}``.

### <a name="non-telemetry-traffic"></a>Verkeer niet telemetrie

Vaak, naast telemetrie gegevenspunten verzenden apparaten ook van berichten en verzoeken die worden uitgevoerd en de verwerking van toepassingsobjectlaag van de bedrijfslogica moeten. Bijvoorbeeld kritieke waarschuwingen die in een specifieke actie in de back-end of apparaat antwoorden op opdrachten die zijn verzonden vanuit de back-end resulteren moeten.

Zie voor meer informatie over de beste manier om dit soort bericht verwerkt de [Zelfstudie: IoT Hub apparaat naar cloud berichten verwerken] [ lnk-d2c-tutorial] zelfstudie.

### <a name="device-to-cloud-configuration-options"></a>Opties voor apparaat naar cloud

Een hub IoT beschrijft de volgende eigenschappen waarmee u controle apparaat voor cloud messaging.

* **Aantal partities**. Deze eigenschap instellen op maken voor het definiëren van het aantal partities voor ingestie van apparaat-wolk-gebeurtenis.
* De **retentietijd**. Deze eigenschap geeft de retentietijd voor apparaat-wolk-berichten. De standaardwaarde is één dag, maar deze kan worden verhoogd tot zeven dagen.

Ook analogously met gebeurtenis-Hubs IoT Hub kunt u beheren groepen consumenten op het apparaat naar de cloud eindpunt ontvangen.

U kunt alle volgende eigenschappen, hetzij via programmering via de [IoT Hub resource provider REST API's][lnk-resource-provider-apis], of via de [Azure portal][lnk-management-portal].

### <a name="anti-spoofing-properties"></a>Eigenschappen van anti-spoofing

Om te voorkomen dat apparaat spoofing in apparaat-wolk-berichten, IoT Hub stempels alle berichten met de volgende eigenschappen:

* **ConnectionDeviceId**
* **ConnectionDeviceGenerationId**
* **ConnectionAuthMethod**

De eerste twee bevatten de **deviceId** - en **generationId** van het oorspronkelijke apparaat, [Apparaateigenschappen identiteit]aan de hand van[lnk-device-properties].

De eigenschap **ConnectionAuthMethod** bevat een geserialiseerd JSON-object met de volgende eigenschappen:

```
{
  "scope": "{ hub | device}",
  "type": "{ symkey | sas}",
  "issuer": "iothub"
}
```

## <a name="cloud-to-device-messages"></a>Cloud-to-device-berichten

U verzenden berichten via een gerichte service-eindpunt (**/messages/devicebound**) wolk naar apparaat. Een apparaat ontvangt ze via een apparaat-specifieke eindpunt (**{deviceId} /devices/ berichten/devicebound**).

Elk bericht wolk naar apparaat is gericht op één apparaat door de eigenschap **op** te **/devices/ {deviceId} berichten/devicebound**.

>[AZURE.IMPORTANT] Elke apparaatwachtrij bevat ten hoogste 50 berichten van wolk naar apparaat. Bij het verzenden meer berichten naar dezelfde resultaten apparaat in een fout.

> [AZURE.NOTE] Wanneer u berichten van de wolk naar apparaat verzenden, namen van eigenschappen en waarden alleen ASCII-alfanumerieke tekens bevatten, plus ``{'!', '#', '$', '%, '&', "'", '*', '*', '+', '-', '.', '^', '_', '`', '|', '~'}``.

### <a name="message-lifecycle"></a>Levenscyclus van bericht

IoT Hub persistent te waarborgen ten minste eenmaal aflevering van berichten, berichten in wachtrijen per apparaat wolk naar apparaat. Apparaten moeten expliciet erkent *voltooiing* voor IoT Hub om ze te verwijderen uit de wachtrij. Dit garandeert tolerantie ten opzichte van connectiviteit en storingen.

In het volgende diagram ziet u de grafiek lifecycle staat voor een cloud-to-device-bericht.

![Bericht van de wolk naar apparaat lifecycle][img-lifecycle]

Wanneer de service een bericht verzendt, wordt geacht *in wachtrij*. Wanneer een apparaat wil *ontvangen* een bericht, IoT Hub *vergrendelingen* het bericht (de status wordt ingesteld op **onzichtbaar**) waardoor andere threads op hetzelfde apparaat om andere berichten te ontvangen. Wanneer een thread apparaat is voltooid voor het verwerken van een bericht, meldt IoT Hub aan door het *invullen* van het bericht.

Een apparaat kan ook:

- *Weigeren* het bericht waardoor IoT Hub instellen op de **Deadlettered** staat. Opmerking: apparaten die verbinding maken met de MQTT geen C2D berichten weigeren.
- *Abandon* waardoor IoT Hub om het bericht weer in de wachtrij met de status van het bericht ingesteld op **in wachtrij**.

Een thread kan voor het verwerken van een bericht zonder te waarschuwen IoT Hub mislukken. In dit geval overgang berichten automatisch van de **onzichtbare** status terug naar de **wachtrij** staat na een *time-out van zichtbaarheid (of vergrendelen)*. De standaardwaarde van deze time-out is één minuut.

Een bericht kan overgaan **in wachtrij** en **onzichtbaar** tussen de lidstaten voor ten hoogste het aantal keren dat is opgegeven in de eigenschap **count van max levering** op IoT Hub. Na dat aantal overgangen stelt IoT Hub u de status van het bericht naar **Deadlettered**. Op dezelfde manier IoT Hub stelt u de status van een bericht naar **Deadlettered** na de verlooptijd ( [TTL]Zie[lnk-ttl]).

Zie voor een zelfstudie over cloud-to-device-berichten, [Zelfstudie: het verzenden van berichten met IoT Hub cloud naar apparaat][lnk-c2d-tutorial]. Zie voor naslaginformatie over hoe verschillende API's en SDK's de functionaliteit van de cloud-to-device tonen, [IoT Hub-API's en SDK's][lnk-sdks].

> [AZURE.NOTE] Normaal gesproken voltooien berichten wolk naar apparaat wanneer het verlies van het bericht zou geen invloed op de toepassingslogica. Bijvoorbeeld, de inhoud van het bericht heeft behouden in lokale opslag of een bewerking is uitgevoerd. Het bericht kan ook tijdelijke informatie waarvan het verlies zou niet van invloed op de functionaliteit van de toepassing uitvoeren. Soms voor langdurige taken, kunt u het bericht wolk naar apparaat voltooien na de beschrijving van de taak in de lokale opslag persistent maken. Vervolgens kunt u de back-end voor toepassing met een of meer berichten van het apparaat naar cloud melden in verschillende stadia van de voortgang van de taak.

### <a name="message-expiration-time-to-live"></a>Vervaldatum berichten (tijd op live)

Elk bericht wolk naar apparaat heeft een verlooptijd. Deze tijd is ingesteld door de service (in de eigenschap **ExpiryTimeUtc** ) of IoT Hub met behulp van de standaard *TTL* opgegeven als een eigenschap IoT Hub. Zie [configuratieopties wolk naar apparaat][lnk-c2d-configuration].

> [AZURE.NOTE] Een gebruikelijke manier om te profiteren van een verloopdatum voor het bericht en geen is berichten verzenden naar niet-verbonden apparaten, korte tijd TTL-waarden in te stellen. Deze benadering bereikt hetzelfde resultaat als de verbindingsstatus apparaat terwijl efficiënter beheren. Wanneer u een bericht bevestigingsberichten aanvraagt, IoT Hub een melding welke apparaten zijn in staat om berichten te ontvangen en welke apparaten zijn niet on line of mislukt.

### <a name="message-feedback"></a>Bericht feedback

Wanneer u een wolk naar apparaat verzenden, kan de service de levering van feedback over de definitieve status van dat bericht per bericht kunt aanvragen.

- Als u de **Ack** -eigenschap op een **positieve instelt**, genereert IoT Hub een feedbackbericht als en slechts als de wolk-to-device-bericht bereikt de status **voltooid** .
- Als u de eigenschap **Ack** in **negatief**, genereert IoT Hub een feedbackbericht als, de cloud-to-device-bericht bereikt de **Deadlettered** staat.
- Als u de eigenschap **Ack** op **full instelt**, IoT Hub een feedbackbericht genereert in beide gevallen.

> [AZURE.NOTE] Als **Ack** **vol**is en u een feedbackbericht niet ontvangt, betekent dit dat het Feedbackbericht is verlopen. De service kan niet weet wat is er gebeurd met het oorspronkelijke bericht. In de praktijk moet een service ervoor te zorgen dat de feedback verwerken kan voordat deze verloopt. De maximale verlooptijd is twee dagen, waardoor veel tijd om de service opnieuw uit te voeren als een fout optreedt.

Zoals uiteengezet in de [eindpunten][lnk-endpoints], IoT-Hub levert feedback via een gerichte service-eindpunt (**/messages/servicebound/feedback**) als berichten. De semantiek voor het ontvangen van feedback zijn hetzelfde als voor cloud-to-device-berichten en het hetzelfde [bericht lifecycle][lnk-lifecycle]. Indien mogelijk, batch bericht feedback wordt verwerkt in één bericht met de volgende notatie:

| Eigenschap | Beschrijving |
| -------- | ----------- |
| EnqueuedTime | Tijdstempel dat aangeeft wanneer het bericht is gemaakt. |
| Gebruikers-id | `{iot hub name}` |
| ContentType | `application/vnd.microsoft.iothub.feedback.json` |

De instantie is een geserialiseerd JSON matrix van records, elk met de volgende eigenschappen:

| Eigenschap | Beschrijving |
| -------- | ----------- |
| EnqueuedTimeUtc | Tijdstempel dat aangeeft wanneer het resultaat van het bericht is gebeurd. Bijvoorbeeld, verlopen het apparaat is voltooid of het bericht. |
| OriginalMessageId | **MessageId** van de wolk naar apparaat bericht waaraan feedback informatie geldt. |
| StatusCode | Integer, vereist. Feedbackberichten die worden gegenereerd door IoT Hub gebruikt. <br/> 0 = succes <br/> 1 = bericht is verlopen <br/> 2 = maximum levering aantal overschreden <br/> 3 = bericht geweigerd |
| Beschrijving | De tekenreekswaarden voor de **StatusCode**. |
| DeviceId | **DeviceId** van het doelapparaat van de wolk naar apparaat bericht waaraan dit stukje feedback betrekking heeft. |
| DeviceGenerationId | **DeviceGenerationId** van het doelapparaat van de wolk naar apparaat bericht waaraan dit stukje feedback betrekking heeft. |


>[AZURE.IMPORTANT] De service moet een **MessageId** voor het bericht van de wolk naar apparaat om de feedback te correleren met het oorspronkelijke bericht te kunnen opgeven.

In het volgende voorbeeld wordt de hoofdtekst van een feedbackbericht.

```
[
  {
    "OriginalMessageId": "0987654321",
    "EnqueuedTimeUtc": "2015-07-28T16:24:48.789Z",
    "StatusCode": 0
    "Description": "Success",
    "DeviceId": "123",
    "DeviceGenerationId": "abcdefghijklmnopqrstuvwxyz"
  },
  {
    ...
  },
  ...
]
```

### <a name="cloud-to-device-configuration-options"></a>Cloud-to-device configuratieopties

Elke hub IoT beschrijft de volgende configuratieopties voor berichtafhandeling wolk naar apparaat.

| Eigenschap | Beschrijving | Bereik en standaard |
| -------- | ----------- | ----------------- |
| defaultTtlAsIso8601 | Standaard-TTL voor cloud-to-device-berichten. | ISO_8601 interval tot 2D (minimaal 1 minuut). Standaard: 1 uur. |
| maxDeliveryCount | Levering van maximum aantal voor wachtrijen wolk naar apparaat per apparaat. | 1 tot en met 100. Standaard: 10. |
| feedback.ttlAsIso8601 | Bewaren voor service-gebonden feedbackcommentaar. | ISO_8601 interval tot 2D (minimaal 1 minuut). Standaard: 1 uur. |
| feedback.maxDeliveryCount | Levering van maximum aantal voor feedback wachtrij. | 1 tot en met 100. Standaard: 100. |

Zie voor meer informatie, [Maak IoT hubs][lnk-portal].

## <a name="read-device-to-cloud-messages"></a>Apparaat-wolk-berichten lezen

IoT Hub beschrijft een eindpunt voor uw back-end services het apparaat naar cloud-berichten ontvangen door de hub. Het eindpunt is de gebeurtenis Hub-compatibel is, kunt u gebruik van de mechanismen voor de gebeurtenis Hubs-service biedt ondersteuning voor het lezen van berichten.

Wanneer u de [Azure Service Bus SDK voor .NET] gebruikt[ lnk-servicebus-sdk] of de [Gebeurtenis Hubs - gebeurtenis Processor Host][lnk-eventprocessorhost], kunt u eventuele verbindingstekenreeksen IoT Hub met de juiste machtigingen. Vervolgens **berichten/gebeurtenissen** als de naam van de gebeurtenis Hub gebruiken.

Als u de SDK's (of product integraties) gebruikt die zich niet bewust van IoT Hub, moet u een gebeurtenis Hub-compatibele eindpunt en een gebeurtenis Hub-compatibele naam ophalen van de IoT Hub-instellingen in de [Azure portal][lnk-management-portal]:

1. Klik op **berichten**in de blade IoT-hub.
2. In de sectie **instellingen van het apparaat naar cloud** treft u de volgende waarden: **gebeurtenis Hub-compatibele eindpunt**, de **naam van de gebeurtenis Hub-compatibel**en **partities**.

    ![Instellingen van het apparaat naar cloud][img-eventhubcompatible]

> [AZURE.NOTE] Als de SDK een **hostnaam** of **Namespace** -waarde vereist, wordt het schema verwijderen vanuit het **eindpunt van de gebeurtenis Hub-compatibel**. Als uw eindpunt gebeurtenis Hub-compatibele **sb://iothub-ns-myiothub-1234.servicebus.windows.net/**is, is de **hostnaam** **iothub, ns, myiothub, 1234.servicebus.windows.net**en de **Namespace** **iothub-ns-myiothub-1234**zou zijn.

Vervolgens kunt u een beveiligingsbeleid voor gedeelde toegang met de machtigingen van de **ServiceConnect** verbinding maken met de opgegeven gebeurtenis Hub.

Als u een gebeurtenis Hub-verbindingsreeks samenstellen met behulp van de vorige gegevens, gebruikt u het volgende patroon:

```
Endpoint={Event Hub-compatible endpoint};SharedAccessKeyName={iot hub policy name};SharedAccessKey={iot hub policy key}
```

Hier volgt een lijst van de SDK's en -integratie die u kunt gebruiken met de gebeurtenis Hub-compatibele eindpunten die IoT Hub beschikbaar worden gesteld:

* [Client Java gebeurtenis Hubs](https://github.com/hdinsight/eventhubs-client)
* [Apache Storm spout](../hdinsight/hdinsight-storm-develop-csharp-event-hub-topology.md). U kunt de [bron spout](https://github.com/apache/storm/tree/master/external/storm-eventhubs) weergeven op GitHub.
* [Integratie van Apache Spark](../hdinsight/hdinsight-apache-spark-eventhub-streaming.md)

## <a name="reference-topics"></a>Naslaginformatie:

De volgende onderwerpen voor verwijzing bieden meer informatie over het uitwisselen van berichten met IoT Hub.

## <a name="message-format"></a>Berichtindeling

IoT Hub berichten bestaan uit:

* Een set *Eigenschappen*. Eigenschappen die IoT Hub worden geïnterpreteerd of ingesteld. Deze set is vooraf bepaald.
* Een set *Eigenschappen van toepassing*. Een woordenlijst met tekenreekseigenschappen die de toepassing kunt definiëren en toegang zonder te converteren van de hoofdtekst van het bericht. IoT Hub wordt nooit deze eigenschappen wijzigt.
* Een dekkende binaire instantie.

Zie voor meer informatie over de manier waarop het bericht is gecodeerd in verschillende protocollen [IoT Hub-API's en SDK's][lnk-sdks].

De volgende tabel bevat de set Systeemeigenschappen in IoT Hub berichten.

| Eigenschap | Beschrijving |
| -------- | ----------- |
| MessageId | Een gebruiker instelbare id voor het bericht, gebruikt voor de aanvraag-antwoord patronen. Indeling: Een hoofdlettergevoelige tekenreeks (maximaal 128 tekens) van de alfanumerieke tekens van ASCII-7-bits + `{'-', ':',’.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '$', '''}`. |
| Volgnummer | Een nummer (uniek per apparaat wachtrij) door IoT Hub toegewezen aan elk bericht wolk naar apparaat. |
| Aan | Een bestemming die is opgegeven in een [Wolk naar apparaat] [ lnk-c2d] berichten. |
| ExpiryTimeUtc | Datum en tijd van een verloopdatum voor het bericht. |
| EnqueuedTime | Datum en tijd die het bericht is ontvangen door de IoT Hub. |
| CorrelationId | Een tekenreekseigenschap in een antwoordbericht waarin doorgaans het MessageId van het verzoek, in antwoord op verzoek patronen. |
| Gebruikers-id | Een ID die wordt gebruikt voor het opgeven van de oorsprong van berichten. Berichten worden gegenereerd door IoT Hub, is ingesteld op `{iot hub name}`. |
| ACK | De generator van een feedback-bericht. Deze eigenschap wordt gebruikt in berichten van de wolk naar apparaat aanvragen IoT Hub voor het genereren van Feedbackberichten als gevolg van het verbruik van het bericht door het apparaat. Mogelijke waarden: **none** (standaardwaarde): geen Feedbackbericht wordt gegenereerd, **positieve**: feedback bericht als het bericht is uitgevoerd, **negatieve**: feedback bericht als het bericht is verlopen (of levering van maximum aantal is bereikt) zonder wordt voltooid door het apparaat, of de **volledige**: positieve en negatieve. Zie voor meer informatie [bericht feedback][lnk-feedback]. |
| ConnectionDeviceId | Een ID die wordt ingesteld door IoT-Hub op het apparaat naar cloud-berichten. Het bevat de **deviceId** van het apparaat waarop het bericht is verzonden. |
| ConnectionDeviceGenerationId | Een ID die wordt ingesteld door IoT-Hub op het apparaat naar cloud-berichten. Het bevat de **generationId** ( [Apparaateigenschappen identiteit]aan de hand van[lnk-device-properties]) van het apparaat waarop het bericht is verzonden. |
| ConnectionAuthMethod | Een verificatiemethode die is ingesteld door de IoT Hub op apparaat naar cloud-berichten. Deze eigenschap bevat informatie over de verificatiemethode die wordt gebruikt voor het verifiëren van het apparaat te verzenden. Zie voor meer informatie, [apparaat cloud anti-spoofing][lnk-antispoofing].|

## <a name="communication-protocols"></a>Communicatieprotocollen

IoT Hub kunt u apparaten gebruiken [MQTT][lnk-mqtt], MQTT via WebSockets, [AMQP][lnk-amqp], AMQP via WebSockets en HTTP-protocollen voor communicatie-apparaat aan. De volgende tabel bevat de op hoog niveau aanbevelingen voor de keuze van het protocol:

| Protocol | Wanneer moet u dit protocol |
| -------- | ------------------------------------ |
| MQTT <br> MQTT via WebSocket     | Op alle apparaten waarvoor geen verbinding maken met meerdere apparaten (elk met een eigen referenties per apparaat) via dezelfde verbinding TLS gebruiken. |
| AMQP <br> AMQP via WebSocket    | Op veld en cloud gateways gebruiken om te profiteren van de verbinding tussen apparaten multiplexing. |
| HTTP    | Te gebruiken voor apparaten die niet kunnen andere protocollen ondersteunen. |

Houd rekening met de volgende punten wanneer u ervoor uw protocol voor communicatie apparaat zijde kiest:

* **Patroon van de wolk naar apparaat**. HTTP beschikt niet over een efficiënte manier voor het implementeren van server push. Als zodanig wanneer u HTTP gebruikt, controleren apparaten IoT Hub voor cloud-to-device-berichten. Deze benadering is voor het apparaat en de IoT Hub inefficiënt. Elk apparaat moet controleren op berichten onder de huidige HTTP-richtsnoeren, om de 25 minuten of meer. Aan de andere kant, MQTT en AMQP ondersteuning voor server push bij het ontvangen van berichten van de wolk naar apparaat. Ze kunnen direct dit gereedschap duwt van berichten van IoT Hub het apparaat. Als de vertraging van de levering van belang is, zijn MQTT of AMQP de beste protocollen te gebruiken. Zelden aangesloten apparaten, HTTP werkt ook.
* **Veld gateways**. Wanneer u MQTT en HTTP gebruikt, kunt u meerdere apparaten (elk met een eigen referenties per apparaat) geen verbinding met de TLS verbinding. Dus, voor het [veld gateway-scenario's][lnk-azure-gateway-guidance], deze protocollen zijn niet-optimale omdat ze een TLS-verbinding tussen het veld gateway en IoT Hub voor elk apparaat is verbonden met het veld gateway is vereist.
* **Weinig systeembronnen apparaten**. De HTTP-MQTT en bibliotheken hebben minder ruimte inneemt dan de AMQP-bibliotheken. Als zodanig, als het apparaat heeft een beperkte bronnen (bijvoorbeeld minder dan 1 MB RAM-geheugen), zijn deze protocollen de enige protocol-implementatie.
* **Verandering van het netwerk**. Het standaardprotocol van AMQP gebruikt poort 5671, terwijl MQTT luistert op poort 8883, die problemen kan veroorzaken in netwerken die zijn gesloten voor niet-HTTP-protocollen. MQTT via WebSockets, AMQP via WebSockets en HTTP zijn beschikbaar in dit scenario moet worden gebruikt.
* **Pakketgrootte**. MQTT en AMQP zijn binaire protocollen, waardoor het compacter payloads dan HTTP.

> [AZURE.NOTE] Als u HTTP gebruikt, moet elk apparaat controleren op berichten wolk naar apparaat elke 25 minuten of meer. Tijdens de ontwikkeling is het echter acceptabel te vragen vaker dan om de 25 minuten.

## <a name="port-numbers"></a>Poortnummers

Apparaten kunnen communiceren met IoT Hub in Azure met behulp van verschillende protocollen. De keuze van het protocol wordt meestal bepaald door de specifieke vereisten van de oplossing. De volgende tabel worden de uitgaande poorten die geopend voor een apparaat zijn moeten te kunnen gebruiken, een specifiek protocol:

| Protocol | Poort(en) |
| -------- | ------- |
| MQTT     | 8883    |
| MQTT via WebSockets | 443    |
| AMQP     | 5671    |
| AMQP via WebSockets | 443    |
| HTTP     | 443     |
| LWM2M (Device management) | 5684 |

Als u een hub IoT hebt gemaakt in een Azure-regio, blijft de hub hetzelfde IP-adres voor de levensduur van de hub. Echter, om de kwaliteit van de dienstverlening te handhaven als Microsoft de hub IoT naar een andere schaaleenheid verplaatst vervolgens krijgt het een nieuw IP-adres.

## <a name="notes-on-mqtt-support"></a>Opmerkingen over de ondersteuning van MQTT

IoT Hub implementeert de MQTT v3.1.1-protocol met de volgende beperkingen en specifiek gedrag:

  * **QoS-2 wordt niet ondersteund**. Wanneer een client apparaat een bericht met **QoS-2 publiceert**, gesloten IoT Hub de netwerkverbinding. Als een device-client zich op een onderwerp met **QoS-2 abonneert**, geeft IoT Hub het maximale QoS-niveau 1 in het pakket **SUBACK** .
  * **Berichten behouden blijven niet bewaard**. IoT Hub wordt toegevoegd als een device-client een bericht gepubliceerd met de vlag bewaren is ingesteld op 1, de **x-opt-behoudt** de eigenschap application bij het bericht. In dit geval IoT Hub niet het bericht behouden blijft bestaan, maar in plaats daarvan wordt doorgegeven aan de back-endtoepassing.

Zie voor meer informatie, [IoT Hub MQTT ondersteuning voor][lnk-devguide-mqtt].

Als laatste vergoeding, moet u controleren de [Azure IoT-protocol gateway] [ lnk-azure-protocol-gateway] waarmee u een krachtige, aangepaste protocol gateway rechtstreeks aan de IoT Hub implementeren. De gateway Azure IoT-protocol kunt u de apparaat-protocol voor brownfield MQTT implementaties of andere aangepaste protocollen wilt aanpassen. Deze aanpak is echter vereist u uitvoert en een gateway aangepast protocol worden uitgevoerd.

## <a name="additional-reference-material"></a>Aanvullend referentiemateriaal

Er zijn andere zoeken naar onderwerpen in de handleiding voor ontwikkelaars:

- [Eindpunten IoT Hub] [ lnk-endpoints] beschrijft de verschillende eindpunten die elke hub IoT beschikbaar worden gesteld voor de uitvoering en het beheer van bewerkingen.
- [Procesbeperking en quota's] [ lnk-quotas] de quota die van toepassing zijn op de IoT Hub-service en de bandbreedteregeling gedrag u kunt verwachten als u de service wordt beschreven.
- [SDK's IoT Hub apparaat- en] [ lnk-sdks] geeft een overzicht van de verschillende taal SDK's u een gebruiken, wanneer u zowel apparaat als service toepassingen ontwikkelt die samenwerken met IoT Hub.
- [Taal voor twins, methoden en taken query] [ lnk-query] de querytaal kunt u gegevens ophalen uit de IoT Hub over uw apparaat twins, methoden en taken beschreven.
- [Ondersteuning voor IoT Hub MQTT] [ lnk-devguide-mqtt] meer informatie over de IoT Hub ondersteuning biedt voor het MQTT-protocol.

## <a name="next-steps"></a>Volgende stappen

Nu u hebt geleerd hoe u berichten verzenden en ontvangen met IoT Hub, kunt u mogelijk geïnteresseerd in de volgende onderwerpen in de handleiding voor ontwikkelaars:

- [Uploaden van bestanden vanaf een apparaat][lnk-devguide-upload]
- [Identiteiten in IoT Hub apparaat beheren][lnk-devguide-identities]
- [Toegang tot de IoT Hub][lnk-devguide-security]
- [Twins apparaat gebruiken voor het synchroniseren van staat en configuraties][lnk-devguide-device-twins]
- [Een directe methode aanroept op een apparaat][lnk-devguide-directmethods]
- [Taken plannen op meerdere apparaten][lnk-devguide-jobs]

Als u proberen sommige van de concepten die in dit artikel worden beschreven wilt, kunt u mogelijk geïnteresseerd in de volgende zelfstudies voor IoT Hub:

- [Aan de slag met Azure IoT Hub][lnk-getstarted-tutorial]
- [Het verzenden van berichten met IoT Hub cloud naar apparaat][lnk-c2d-tutorial]
- [Het verwerken van IoT Hub apparaat-wolk-berichten][lnk-d2c-tutorial]


[img-lifecycle]: ./media/iot-hub-devguide-messaging/lifecycle.png
[img-eventhubcompatible]: ./media/iot-hub-devguide-messaging/eventhubcompatible.png

[lnk-resource-provider-apis]: https://msdn.microsoft.com/library/mt548492.aspx
[lnk-azure-gateway-guidance]: iot-hub-devguide-endpoints.md#field-gateways
[lnk-guidance-scale]: iot-hub-scaling.md
[lnk-azure-protocol-gateway]: iot-hub-protocol-gateway.md
[lnk-amqp]: http://docs.oasis-open.org/amqp/core/v1.0/os/amqp-core-complete-v1.0-os.pdf
[lnk-mqtt]: http://docs.oasis-open.org/mqtt/mqtt/v3.1.1/mqtt-v3.1.1.pdf
[lnk-event-hubs]: http://azure.microsoft.com/documentation/services/event-hubs/
[lnk-event-hubs-consuming-events]: ../event-hubs/event-hubs-programming-guide.md#event-consumers
[lnk-management-portal]: https://portal.azure.com
[lnk-servicebus]: http://azure.microsoft.com/documentation/services/service-bus/
[lnk-eventhub-partitions]: ../event-hubs/event-hubs-overview.md#partitions
[lnk-portal]: iot-hub-create-through-portal.md

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-c2d]: iot-hub-devguide-messaging.md#cloud-to-device-messages
[lnk-compatible-endpoint]: iot-hub-devguide-messaging.md#read-device-to-cloud-messages
[lnk-protocols]: iot-hub-devguide-messaging.md#communication-protocols
[lnk-message-format]: iot-hub-devguide-messaging.md#message-format
[lnk-d2c-configuration]: iot-hub-devguide-messaging.md#device-to-cloud-configuration-options
[lnk-device-properties]: iot-hub-devguide-identity-registry.md#device-identity-properties
[lnk-ttl]: iot-hub-devguide-messaging.md#message-expiration-time-to-live
[lnk-c2d-configuration]: iot-hub-devguide-messaging.md#cloud-to-device-configuration-options
[lnk-lifecycle]: iot-hub-devguide-messaging.md#message-lifecycle
[lnk-feedback]: iot-hub-devguide-messaging.md#message-feedback
[lnk-antispoofing]: iot-hub-devguide-messaging.md#anti-spoofing-properties
[lnk-compare]: iot-hub-compare-event-hubs.md

[lnk-devguide-upload]: iot-hub-devguide-file-upload.md
[lnk-devguide-identities]: iot-hub-devguide-identity-registry.md
[lnk-devguide-security]: iot-hub-devguide-security.md
[lnk-devguide-device-twins]: iot-hub-devguide-device-twins.md
[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-servicebus-sdk]: https://www.nuget.org/packages/WindowsAzure.ServiceBus
[lnk-eventprocessorhost]: http://blogs.msdn.com/b/servicebus/archive/2015/01/16/event-processor-host-best-practices-part-1.aspx


[lnk-getstarted-tutorial]: iot-hub-csharp-csharp-getstarted.md
[lnk-c2d-tutorial]: iot-hub-csharp-csharp-c2d.md
[lnk-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md
