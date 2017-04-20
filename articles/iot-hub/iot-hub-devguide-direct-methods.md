<properties
 pageTitle="Handleiding voor ontwikkelaars - directe methoden | Microsoft Azure"
 description="Azure IoT Hub handleiding voor ontwikkelaars - directe methoden aan te roepen van de code op uw apparaten gebruiken"
 services="iot-hub"
 documentationCenter=".net"
 authors="nberdy"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="nberdy"/>

# <a name="invoke-a-direct-method-on-a-device-preview"></a>Een directe methode aanroept op een apparaat (voorbeeld)

## <a name="overview"></a>Overzicht

IoT Hub kunt u methoden op apparaten uit de cloud. Methoden geven een aanvraag-antwoord interactie met een apparaat, vergelijkbaar met een HTTP-oproep in die zin dat zij slagen of (zodra een gebruiker opgegeven time-out) mislukken aan de gebruiker duidelijk de status van de oproep. Dit is handig voor scenario's waarin het verloop van de onmiddellijke actie afhankelijk van of het apparaat kon reageren is, zoals het verzenden van een SMS-wake-up naar een apparaat als een apparaat is off line (SMS wordt duurder dan een aanroep van de methode).

U kunt een methode beschouwen als een remote procedure call rechtstreeks naar het apparaat. Alleen de methoden die zijn genomen op een apparaat kunnen worden aangeroepen vanuit de cloud. Als de wolk probeert om een methode aanroept op een apparaat dat u geen methode gedefinieerd, mislukt de aanroep van de methode.

Elke methode apparaat is bedoeld voor één apparaat. [Taken] [ lnk-devguide-jobs] kunt u methoden op meerdere apparaten, en in de wachtrij methodeaanroep voor niet-verbonden apparaten.

Iedereen met machtigingen op IoT Hub **service verbinding** kan een methode aanroept op een apparaat.

### <a name="when-to-use"></a>Wanneer gebruikt u

Methoden zijn vergelijkbaar met de [berichten van de wolk naar apparaat] [ lnk-devguide-messages] in dat zowel de cloud back-end informatie doorgeven aan een apparaat inschakelen, maar ze in fundamentele manieren verschillen. In dat geval zijn synchrone en niet duurzaam terwijl cloud-to-device-berichten asynchroon met maximaal 48 uur van de duurzaamheid worden.

Methoden een verzoek-antwoord-patroon volgen en niet duurzaam zijn. Het gebrek aan duurzaamheid biedt twee onmiddellijke voordelen wanneer u apparaten zijn bestuurt:

- **Meteen feedback krijgen op het uitvoeren van methode** betekent is niet nodig voor het beheer van de correlatie tussen de aanvraag en het antwoord.
- **Snellere verwerking** betekent dat de bewerkingen sneller kunnen worden uitgevoerd omdat de IoT Hub biedt niet de duurzaamheid. IoT Hub kan meer methodeaanroepen per eenheid dan berichten wolk naar apparaat.

Cloud-to-device-berichten zijn niet noodzakelijkerwijs opdrachten naar het apparaat, maar liever vertegenwoordigen een cloud-service een deel van de informatie doorgeven van het apparaat voor het afhalen op het gemak en dat het apparaat mogelijk of reageert niet. Cloud-to-device berichten hebben een time-out langer (48 uur) terwijl methoden verloopt veel sneller.

Methoden gebruiken voor de opdrachtaanroep onmiddellijk op een apparaat en de taken voor geplande aanroep van een opdracht op een apparaat.

## <a name="method-lifecycle"></a>Methode lifecycle

Methoden worden geïmplementeerd op het apparaat en mogelijk nul of meer ingangen in de nettolading van de methode correct wordt gestart. Aanroepen van een directe methode via een gerichte service-URI (`{iot hub}/twins/{device id}/methods/`). Een apparaat ontvangt rechtstreekse methoden door een MQTT apparaat-specifieke onderwerp (`$iothub/methods/POST/{method name}/`). We kunnen in de toekomst methoden op extra apparaatspecifiek netwerkprotocollen ondersteunen.

> [AZURE.NOTE] Wanneer u een directe methode op een apparaat aanroept, de namen van eigenschappen en waarden mogen alleen US-ASCII-afdrukbare alfanumerieke, met uitzondering van een in de volgende set: ``{'$', '(', ')', '<', '>', '@', ',', ';', ':', '\', '"', '/', '[', ']', '?', '=', '{', '}', SP, HT}``.

Methoden zijn synchrone en een slagen of mislukken nadat de time-outperiode (standaard: 30 seconden, instelbare van 3600 seconden). Methoden zijn nuttig in interactieve scenario's waar u een apparaat op te treden als het apparaat online en ontvangen opdrachten is, zoals het inschakelen van een lampje op een telefoon. In deze scenario's die u wilt zien van een onmiddellijke slagen of mislukken zodat de cloud-service op het resultaat zo spoedig mogelijk reageren kan. Het apparaat retourneert dan sommige berichttekst als gevolg van de methode, maar het is niet vereist voor de methode om dit te doen. Er is geen garantie op volgorde of enige gelijktijdigheid semantiek op methodeaanroepen.

Apparaat methodeaanroepen worden HTTP-only uit de cloud kant en MQTT alleen van de kant van het apparaat.

## <a name="reference-topics"></a>Naslaginformatie:

De volgende verwijzing onderwerpen bevatten meer informatie over het gebruik van directe methoden.

## <a name="invoke-a-direct-method-from-a-back-end-app"></a>Directe methode vanuit een back-end-app

### <a name="method-invocation"></a>Methode-aanroep

Directe methode aanroepen op een apparaat zijn HTTP-oproepen die bestaan uit:

- De *URI* voor het apparaat (`{iot hub}/twins/{device id}/methods/`)
- De POST- *methode*
- *Kopteksten* bevatten de vergunning aanvraag-ID, type inhoud en inhoud coderen
- Een transparante JSON *instantie* in de volgende notatie:

```
{
    "methodName": "reboot",
    "timeoutInSeconds": 200,
    "payload": {
        "input1": "someInput",
        "input2": "anotherInput"
    }
}
```

  Er is een time-out in seconden. Als er geen time-out is ingesteld, wordt standaard 30 seconden.
  
### <a name="response"></a>Reactie

De back-end ontvangt een reactie die bestaat uit:

- *HTTP-statuscode*die wordt gebruikt voor fouten die afkomstig zijn van de IoT Hub, met inbegrip van een 404-fout voor apparaten die momenteel niet verbonden
- *Kopteksten* bevatten de etag, aanvragen ID, type inhoud en inhoud codering
- Een JSON *instantie* in de volgende notatie:

```
{
    "status" : 201,
    "payload" : {...}
}
```
  
   Beide `status` en `body` worden geleverd door het apparaat en gebruikt om te reageren met de statuscode van het apparaat en/of beschrijving.

## <a name="handle-a-direct-method-on-a-devcie"></a>Een directe methode op een devcie

### <a name="method-invocation"></a>Methode-aanroep

Apparaten ontvangen aanvragen op het onderwerp MQTT directe methode:`$iothub/methods/POST/{method name}/?$rid={request id}`

De instantie die het apparaat ontvangt, wordt in de volgende notatie:

```
{
    "input1": "someInput",
    "input2": "anotherInput"
}
```

Methode-aanvragen zijn QoS 0.

### <a name="response"></a>Reactie

Het apparaat wordt verzonden reacties op `$iothub/methods/res/{status}/?$rid={request id}`, waarbij:

 - De `status` eigenschap is de geleverde apparaat status van de uitvoering van de methode.
 - De `$rid` eigenschap, wordt de aanvraag-ID van de methode aanroepen ontvangen van IoT Hub.

De instantie kan die door het apparaat is ingesteld en elke status.

## <a name="additional-reference-material"></a>Aanvullend referentiemateriaal

Er zijn andere zoeken naar onderwerpen in de handleiding voor ontwikkelaars:

- [Eindpunten IoT Hub] [ lnk-endpoints] beschrijft de verschillende eindpunten die elke hub IoT beschikbaar worden gesteld voor de uitvoering en het beheer van bewerkingen.
- [Procesbeperking en quota's] [ lnk-quotas] de quota die van toepassing zijn op de IoT Hub-service en de bandbreedteregeling gedrag u kunt verwachten als u de service wordt beschreven.
- [SDK's IoT Hub apparaat- en] [ lnk-sdks] geeft een overzicht van de verschillende taal SDK's u een gebruiken, wanneer u zowel apparaat als service toepassingen ontwikkelt die samenwerken met IoT Hub.
- [Taal voor twins, methoden en taken query] [ lnk-query] de querytaal kunt u gegevens ophalen uit de IoT Hub over uw apparaat twins, methoden en taken beschreven.
- [Ondersteuning voor IoT Hub MQTT] [ lnk-devguide-mqtt] meer informatie over de IoT Hub ondersteuning biedt voor het MQTT-protocol.

## <a name="next-steps"></a>Volgende stappen

Nu u hebt geleerd hoe u directe methoden gebruiken, kunt u wellicht geïnteresseerd bent in het volgende onderwerp in de handleiding voor ontwikkelaars:

- [Taken plannen op meerdere apparaten][lnk-devguide-jobs]

Als u proberen sommige van de concepten die in dit artikel worden beschreven wilt, kunt u mogelijk geïnteresseerd in de volgende zelfstudie voor IoT Hub:

- [Cloud-to-device-methoden gebruiken][lnk-methods-tutorial]

<!-- links and images -->

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-methods-tutorial]: iot-hub-c2d-methods.md
[lnk-devguide-messages]: iot-hub-devguide-messaging.md
