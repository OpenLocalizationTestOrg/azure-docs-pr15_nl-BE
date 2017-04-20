<properties
 pageTitle="Handleiding voor ontwikkelaars - apparaat identiteit register | Microsoft Azure"
 description="Azure IoT Hub developer guide - beschrijving van het apparaat-id-register en het gebruik ervan voor het beheren van uw apparaten"
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

# <a name="manage-device-identities-in-iot-hub"></a>Identiteiten in IoT Hub apparaat beheren

## <a name="overview"></a>Overzicht

Elke hub IoT heeft een apparaat identiteit register met informatie over de apparaten die verbinding maken met de hub zijn toegestaan. Voordat u een apparaat kunt aansluiten op een hub, moet er een vermelding voor het apparaat in de hub apparaat identiteit register. Een apparaat moet ook worden geverifieerd bij de hub op basis van referenties die zijn opgeslagen in het register van de identiteit apparaat.

Op een hoog niveau is het apparaat identiteit register REST kan een verzameling bronnen voor apparaat-id. Wanneer u een vermelding aan het register toevoegt, maakt IoT Hub een set bronnen per apparaat in de dienst, zoals de wachtrij die u tijdens een vlucht wolk naar apparaat berichten bevat.

### <a name="when-to-use"></a>Wanneer gebruikt u

Het register van de identiteit apparaat gebruiken wanneer u apparaten die verbinding maken met u IoT hub en wanneer u per apparaat toegang tot het apparaat gerichte eindpunten in de hub moet inrichten.

> [AZURE.NOTE] Het register apparaat-id bevat geen specifieke metagegevens.

## <a name="device-identity-registry-operations"></a>Registerbewerkingen apparaat-id

De IoT Hub apparaat identiteit register bevat de volgende bewerkingen:

* Apparaat-id maken
* Bijwerken van apparaat-id
* Apparaat-id door de ID ophalen
* Apparaat-id verwijderen
* Lijst van maximaal 1000 identiteiten
* Alle identiteiten exporteren naar Azure blob-opslag
* Identiteiten importeren vanuit Azure blob-opslag

Alle volgende bewerkingen optimistische gelijktijdigheid kunnen gebruiken als opgegeven in [RFC7232][lnk-rfc7232].

> [AZURE.IMPORTANT] De enige manier voor het ophalen van alle identiteiten in een hub van identiteit register is [exporteren] gebruiken[ lnk-export] functionaliteit.

Een IoT Hub apparaat identiteit register:

- Bevat niet alle metagegevens van toepassing.
- Toegankelijk als een woordenboek, met behulp van de **deviceId** als de sleutel.
- Expressieve query's ondersteund niet.

Een oplossing voor IoT heeft meestal een aparte oplossing specifieke winkel met metagegevens specifiek voor toepassing. De oplossing specifieke winkel in een oplossing voor intelligente gebouw zou bijvoorbeeld vastleggen dat de ruimte waarin een temperatuursensor wordt geïmplementeerd.

> [AZURE.IMPORTANT] Het register van de identiteit apparaat alleen gebruiken voor het Apparaatbeheer en de toewijzing van operations. Hoge doorvoer bewerkingen tijdens de uitvoering moeten niet afhankelijk is van het uitvoeren van bewerkingen in het register van de identiteit apparaat. De status van de verbinding van een apparaat controleren voordat een opdracht wordt verzonden is bijvoorbeeld niet een ondersteunde patroon. Controleer de [tarieven beperking] [ lnk-quotas] voor het register van de identiteit apparaat en het [apparaat heartbeat] [ lnk-guidance-heartbeat] patroon.

## <a name="disable-devices"></a>Apparaten uitschakelen

U kunt apparaten uitschakelen door de eigenschap **status** van een identiteit in het register bij te werken. U gebruikt deze eigenschap meestal in twee scenario's:

- Tijdens een inrichten orchestration. Zie voor meer informatie, [Apparaten inrichten][lnk-guidance-provisioning].
- Als voor een of andere reden u rekening houden met een apparaat is beschadigd of is geworden door onbevoegden.

## <a name="import-and-export-device-identities"></a>Importeren en exporteren van apparaat-id 's

U kunt apparaat identiteiten in bulk exporteren uit het register van de identiteit van een IoT hub, met asynchrone bewerkingen op de [IoT Hub resource provider-eindpunt][lnk-endpoints]. Uitvoer langdurige taken zijn die met een klant geleverde blob container opslaan apparaat id-gegevens lezen uit het register van de identiteit.

Kunt u de apparaat identiteiten in bulk importeren naar het register van de identiteit van een hub IoT, met behulp van asynchrone bewerkingen op de [IoT Hub resource provider-eindpunt][lnk-endpoints]. Invoer langdurige taken zijn die gebruikmaken van gegevens in een container voor de klant verstrekte blob apparaat identiteit om gegevens te schrijven naar het register van de identiteit apparaat.

- Zie [IoT Hub resource provider REST API's]voor gedetailleerde informatie over de import en export-API's[lnk-resource-provider-apis].
- Zie voor meer informatie over het importeren en exporteren van projecten, [Bulk beheer van identiteiten van IoT Hub apparaat][lnk-bulk-identity].

## <a name="device-provisioning"></a>Inrichten van apparaat

Het apparaatgegevens waarin een bepaalde oplossing voor IoT is afhankelijk van de specifieke vereisten van die oplossing. Maar ten minste een oplossing apparaat identiteiten en verificatiesleutels moet opslaan. Azure IoT Hub bevat een identiteit register dat waarden voor elk apparaat als id's, verificatiesleutels en statuscodes kan worden opgeslagen. Een oplossing kunt andere Azure diensten zoals Azure tabelopslag, Azure blob-opslag of Azure DocumentDB een extra apparaatgegevens op te slaan.

*Apparaat-aanbod* is het proces van het eerste apparaatgegevens toe te voegen aan de winkels in uw oplossing. Als u een nieuw apparaat verbinding maken met de hub, moet u toetsen en een nieuw apparaat-ID toevoegen aan het register van de identiteit IoT Hub. Als onderdeel van het inrichtingsproces moet u mogelijk het initialiseren van apparaat-specifieke gegevens in andere winkels oplossing.

## <a name="device-heartbeat"></a>Apparaat heartbeat

Het register van de identiteit IoT Hub bevat een veld met de naam **connectionState**. U moet het veld **connectionState** alleen gebruiken tijdens de ontwikkeling en foutopsporing, IoT oplossingen moeten geen query het veld tijdens het uitvoeren (bijvoorbeeld om te controleren of een apparaat om te bepalen of een wolk naar apparaat-bericht of een SMS-bericht verzenden is verbonden).

Als de IoT-oplossing weten als een apparaat is aangesloten (tijdens het uitvoeren, of met meer nauwkeurigheid dan de eigenschap **connectionState** ), uw oplossing te implementeren het *heartbeat-patroon*.

Het apparaat verzendt berichten apparaat naar cloud ten minste eenmaal elke vaste hoeveelheid tijd (bijvoorbeeld ten minste eenmaal per uur) in het patroon heartbeat. Dit betekent dat zelfs als een apparaat geen gegevens verzenden, nog steeds verzendt een leeg bericht apparaat naar cloud (meestal met een eigenschap die wordt aangeduid als een heartbeat). De oplossing houdt met de laatste heartbeat ontvangen voor elk apparaat en wordt ervan uitgegaan dat er een probleem met een apparaat als dit een heartbeat-bericht niet binnen de verwachte tijd ontvangt aan de service.

Een complexere implementatie kan bevat de gegevens uit het [controleren van bewerkingen] [ lnk-devguide-opmon] voor apparaten die probeert verbinding te maken of communiceren, maar niet goed werkt. Als u het patroon heartbeat implementeert, Controleer [IoT Hub quota en Throttles][lnk-quotas].

> [AZURE.NOTE] Als een IoT-oplossing moet de status van het apparaat verbinding uitsluitend om te bepalen of de wolk naar apparaat verzenden en berichten niet worden uitgezonden naar grote verzamelingen van apparaten, is een veel eenvoudigere patroon rekening te houden met korte tijd verstrijken. Dit bereikt hetzelfde resultaat als een apparaat verbinding status register met behulp van het patroon heartbeat, terwijl aanzienlijk efficiënter beheren. Het is ook mogelijk, door te vragen bericht bevestigingen, IoT Hub van welke apparaten zijn in staat om berichten te ontvangen en die niet online zijn of niet worden ontvangen.

## <a name="reference-topics"></a>Naslaginformatie:

De volgende verwijzing onderwerpen bevatten meer informatie over het register van de identiteit devcie.

## <a name="device-identity-properties"></a>Apparaateigenschappen-identiteit

Apparaat-id's worden weergegeven als de JSON-documenten met de volgende eigenschappen.

| Eigenschap | Opties | Beschrijving |
| -------- | ------- | ----------- |
| deviceId | vereist, alleen-lezen voor updates | Een hoofdlettergevoelige tekenreeks (maximaal 128 tekens) van de alfanumerieke tekens van ASCII-7-bits + `{'-', ':', '.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '$', '''}`. |
| generationId | vereist, alleen-lezen | Een hub gegenereerd hoofdlettergevoelige tekenreeks van maximaal 128 tekens. Dit wordt gebruikt om apparaten met dezelfde **deviceId**onderscheiden wanneer u ze hebt verwijderd en opnieuw gemaakt. |
| ETag | vereist, alleen-lezen | Een string die een zwak etag voor de identiteit van het apparaat, aan de hand van [RFC7232][lnk-rfc7232].|
| auth | optioneel | Een samengesteld object met verificatie-informatie- en materiaal. |
| auth.symkey | optioneel | Een samengesteld object met een primaire en een secundaire sleutel opgeslagen in base64-indeling. |
| status | Vereist | Een access-indicator. Kan worden **ingeschakeld** of **uitgeschakeld**. Indien **ingeschakeld**, het apparaat is toegestaan verbinding te maken. Als **uitgeschakeld**, dit apparaat geen toegang het eindpunt van een apparaat gericht tot. |
| statusReason | optioneel | Een 128 tekens lange tekenreeks waarin de reden voor de status van het apparaat-id. Alle UTF-8-tekens zijn toegestaan. |
| statusUpdateTime | alleen-lezen | Een tijdelijke aanduiding met de datum en tijd van de laatste statusupdate. |
| connectionState | alleen-lezen | Een veld verbindingsstatus aangeeft: **verbonden** of **verbroken**. Dit veld geeft de IoT Hub-weergave van de verbindingsstatus van het apparaat. **Belangrijk**: dit veld dient alleen ter ontwikkeling/debuggen worden gebruikt. Status van de verbinding wordt alleen bijgewerkt voor apparaten met behulp van MQTT of AMQP. Ook is gebaseerd op protocol niveau pings (pings MQTT of AMQP pings) en er een maximale vertraging van slechts 5 minuten. Om deze redenen kan er valse meldingen, zoals gemeld op apparaten verbonden maar die feitelijk verbinding wordt verbroken. |
| connectionStateUpdatedTime | alleen-lezen | Een tijdelijke indicator, met de datum en tijd van laatste status van de verbinding is bijgewerkt. |
| lastActivityTime  | alleen-lezen | Een tijdelijke aanduiding waarin de datum en de laatste keer dat het apparaat verbonden, ontvangen of een bericht gestuurd. |

> [AZURE.NOTE] Verbindingsstatus kan alleen vertegenwoordigen de IoT Hub-weergave van de status van de verbinding. Updates op deze status kunnen worden vertraagd, afhankelijk van de netwerkomstandigheden en configuraties.

## <a name="additional-reference-material"></a>Aanvullend referentiemateriaal

Er zijn andere zoeken naar onderwerpen in de handleiding voor ontwikkelaars:

- [Eindpunten IoT Hub] [ lnk-endpoints] beschrijft de verschillende eindpunten die elke hub IoT beschikbaar worden gesteld voor de uitvoering en het beheer van bewerkingen.
- [Procesbeperking en quota's] [ lnk-quotas] de quota die van toepassing zijn op de IoT Hub-service en de bandbreedteregeling gedrag u kunt verwachten als u de service wordt beschreven.
- [SDK's IoT Hub apparaat- en] [ lnk-sdks] geeft een overzicht van de verschillende taal SDK's u een gebruiken, wanneer u zowel apparaat als service toepassingen ontwikkelt die samenwerken met IoT Hub.
- [Taal voor twins, methoden en taken query] [ lnk-query] de querytaal kunt u gegevens ophalen uit de IoT Hub over uw apparaat twins, methoden en taken beschreven.
- [Ondersteuning voor IoT Hub MQTT] [ lnk-devguide-mqtt] meer informatie over de IoT Hub ondersteuning biedt voor het MQTT-protocol.

## <a name="next-steps"></a>Volgende stappen

Nu u hebt geleerd hoe u het register IoT Hub apparaat identiteit gebruikt, kunt u mogelijk geïnteresseerd in de volgende onderwerpen in de handleiding voor ontwikkelaars:

- [Toegang tot de IoT Hub][lnk-devguide-security]
- [Twins apparaat gebruiken voor het synchroniseren van staat en configuraties][lnk-devguide-device-twins]
- [Een directe methode aanroept op een apparaat][lnk-devguide-directmethods]
- [Taken plannen op meerdere apparaten][lnk-devguide-jobs]

Als u proberen sommige van de concepten die in dit artikel worden beschreven wilt, kunt u mogelijk geïnteresseerd in de volgende zelfstudie voor IoT Hub:

- [Aan de slag met Azure IoT Hub][lnk-getstarted-tutorial]


<!-- Links and images -->

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-resource-provider-apis]: https://msdn.microsoft.com/library/mt548492.aspx
[lnk-guidance-provisioning]: iot-hub-devguide-identity-registry.md#device-provisioning
[lnk-guidance-heartbeat]: iot-hub-devguide-identity-registry.md#device-heartbeat
[lnk-rfc7232]: https://tools.ietf.org/html/rfc7232
[lnk-bulk-identity]: iot-hub-bulk-identity-mgmt.md
[lnk-export]: iot-hub-devguide-identity-registry.md#import-and-export-device-identities
[lnk-devguide-opmon]: iot-hub-operations-monitoring.md

[lnk-devguide-security]: iot-hub-devguide-security.md
[lnk-devguide-device-twins]: iot-hub-devguide-device-twins.md
[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md

[lnk-getstarted-tutorial]: iot-hub-csharp-csharp-getstarted.md