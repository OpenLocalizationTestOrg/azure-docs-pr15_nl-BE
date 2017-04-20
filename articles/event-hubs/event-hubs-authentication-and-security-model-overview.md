<properties 
    pageTitle="Overzicht van de verificatie en beveiliging model gebeurtenis Hubs | Microsoft Azure"
    description="Hubs verificatie en beveiliging model overzicht van de gebeurtenissen."
    services="event-hubs"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="event-hubs"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="08/16/2016"
    ms.author="sethm;clemensv" />

# <a name="event-hubs-authentication-and-security-model-overview"></a>Gebeurtenis Hubs verificatie- en model-overzicht

Het beveiligingsmodel van Hubs gebeurtenis voldoet aan de volgende vereisten:

- Alleen apparaten die geldige referenties kunnen verzenden van gegevens naar een gebeurtenis Hub.
- Een apparaat kan een ander apparaat niet imiteren.
- Een rogue-apparaat kan verzenden van gegevens naar een gebeurtenis Hub worden geblokkeerd.

## <a name="device-authentication"></a>Verificatie apparaat

De gebeurtenis Hubs beveiligingsmodel is gebaseerd op een combinatie van tokens voor [Gedeelde toegang handtekening (SAS)](../service-bus-messaging/service-bus-shared-access-signature-authentication.md) en *gebeurtenisuitgevers*. Een gebeurtenis uitgever definieert een virtuele eindpunt voor een gebeurtenis-Hub. De uitgever kan alleen worden gebruikt om berichten te verzenden naar een gebeurtenis Hub. Het is niet mogelijk om berichten te ontvangen van een uitgever.

Een gebeurtenis Hub wordt meestal één uitgever per apparaat. Alle berichten die worden verzonden naar een van de uitgevers van de Hub van een gebeurtenis zijn in een wachtrij binnen deze Hub gebeurtenis. Uitgevers inschakelen toegesneden toegangsbeheer en bandbreedtebeperking.

Elk apparaat is een unieke token is geüpload naar het apparaat worden toegewezen. De tokens worden geproduceerd, dat elke unieke token toegang aan een andere unieke uitgever verleent. Een apparaat dat een token in bezit kan alleen verzenden naar een uitgever, maar geen andere uitgever. Als u meerdere apparaten delen hetzelfde, deelt elk van deze apparaten een uitgever.

Hoewel niet aanbevolen, is het mogelijk om apparaten met tokens die directe toegang tot een Hub voor de gebeurtenis verlenen plaats. Elk apparaat dat dit token bevat verzenden berichten rechtstreeks op de Hub van die gebeurtenis. Een dergelijk apparaat worden niet beperken. Bovendien het apparaat kan niet worden op zwarte lijst geplaatst van het verzenden van de gebeurtenis Hub.

Alle tokens zijn ondertekend met een SAS-sleutel. Normaal gesproken zijn alle tokens ondertekend met dezelfde sleutel. Apparaten zijn niet op de hoogte van de sleutel; Dit voorkomt dat apparaten productie-tokens.

### <a name="create-the-sas-key"></a>Maak de sleutel SAS

Bij het maken van een naamruimte Hubs gebeurtenis genereert Azure gebeurtenis Hubs een 256-bits SAS-sleutel met de naam **RootManageSharedAccessKey**. Deze belangrijke subsidies verzenden, luisteren en rechten om de naamruimte te beheren. U kunt extra toetsen maken. Het wordt aanbevolen dat u een sleutel dat subsidies machtigingen naar de specifieke gebeurtenis Hub verzenden produceren. Voor de rest van dit onderwerp wordt ervan uitgegaan dat u deze sleutel de naam `EventHubSendKey`.

In het volgende voorbeeld wordt een sleutel alleen verzenden gemaakt bij het maken van de Hub van de gebeurtenis:

```
// Create namespace manager.
string serviceNamespace = "YOUR_NAMESPACE";
string namespaceManageKeyName = "RootManageSharedAccessKey";
string namespaceManageKey = "YOUR_ROOT_MANAGE_SHARED_ACCESS_KEY";
Uri uri = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, string.Empty);
TokenProvider td = TokenProvider.CreateSharedAccessSignatureTokenProvider(namespaceManageKeyName, namespaceManageKey);
NamespaceManager nm = new NamespaceManager(namespaceUri, namespaceManageTokenProvider);

// Create Event Hub with a SAS rule that enables sending to that Event Hub
EventHubDescription ed = new EventHubDescription("MY_EVENT_HUB") { PartitionCount = 32 };
string eventHubSendKeyName = "EventHubSendKey";
string eventHubSendKey = SharedAccessAuthorizationRule.GenerateRandomKey();
SharedAccessAuthorizationRule eventHubSendRule = new SharedAccessAuthorizationRule(eventHubSendKeyName, eventHubSendKey, new[] { AccessRights.Send });
ed.Authorization.Add(eventHubSendRule); 
nm.CreateEventHub(ed);
```

### <a name="generate-tokens"></a>Tokens genereren

U kunt genereren met behulp van de sleutel SAS tokens. U kunt slechts één token per apparaat moet produceren. Tokens kunnen vervolgens worden geproduceerd met behulp van de volgende methode. De namen van tokens worden gegenereerd met behulp van de sleutel **EventHubSendKey** . Elke token is een unieke URI toegewezen.

```
public static string SharedAccessSignatureTokenProvider.GetSharedAccessSignature(string keyName, string sharedAccessKey, string resource, TimeSpan tokenTimeToLive)
```

Als u deze methode aanroept, moet de URI worden opgegeven als `//<NAMESPACE>.servicebus.windows.net/<EVENT_HUB_NAME>/publishers/<PUBLISHER_NAME>`. De URI voor de namen van tokens, is identiek zijn, met uitzondering van `PUBLISHER_NAME`, die verschillende token moet worden. In het ideale geval `PUBLISHER_NAME` vertegenwoordigt de ID van het apparaat dat deze token ontvangt.

Deze methode genereert een token met de volgende structuur:

```
SharedAccessSignature sr={URI}&sig={HMAC_SHA256_SIGNATURE}&se={EXPIRATION_TIME}&skn={KEY_NAME}
```

De token van de verlooptijd wordt opgegeven in seconden vanaf 1 januari 1970. Hier volgt een voorbeeld van een token:

```
SharedAccessSignature sr=contoso&sig=nPzdNN%2Gli0ifrfJwaK4mkK0RqAB%2byJUlt%2bGFmBHG77A%3d&se=1403130337&skn=RootManageSharedAccessKey
```

De tokens hebben gewoonlijk een levensduur die lijkt op de levensduur van het apparaat. Als het apparaat de mogelijkheid heeft om een nieuw token verkrijgen, worden tokens met een kortere levensduur gebruikt.

### <a name="devices-sending-data"></a>Apparaten voor het verzenden van gegevens

Zodra de tokens zijn gemaakt, is elk apparaat met een eigen unieke token ingericht.

Wanneer het apparaat gegevens naar een gebeurtenis Hub verzendt, codes het apparaat de token met de verzendaanvraag. Als u wilt voorkomen dat een aanvaller afluisteren en stelen van de token, plaatsvinden de communicatie tussen het apparaat en de Hub gebeurtenis via een gecodeerd kanaal.

### <a name="blacklisting-devices"></a>Beschermde apparaten

Als een token wordt gestolen door een aanvaller, kan de aanvaller zich voordoen als het apparaat waarvan de token is gestolen. Blacklisting van een apparaat wordt het apparaat onbruikbaar totdat het apparaat een nieuwe beveiligingssleutel die gebruikmaakt van een andere uitgever wordt gegeven.

## <a name="authentication-of-back-end-applications"></a>Verificatie van de back-end-toepassingen

Hubs gebeurtenis maakt gebruik van een beveiligingsmodel dat vergelijkbaar is met het model dat is gebruikt voor Service Bus onderwerpen voor de verificatie van de back-end-toepassingen die de gegevens die zijn gegenereerd door apparaten verbruiken. Een groep van de consument Hubs gebeurtenis komt overeen met een abonnement op een onderwerp Service Bus. Een client kan een groep consumenten maken als de aanvraag voor het maken van de consument groep vergezeld van een token gaat dat subsidies machtigingen beheren voor de gebeurtenis Hub of voor de naamruimte waartoe de Hub gebeurtenis behoort. Een client mag worden gegevens uit een groep consumenten verbruikt als het ontvangen verzoek vergezeld gaat van een token dat verleent rechten op die groep consumenten, de Hub van de gebeurtenis of de naamruimte waartoe de Hub gebeurtenis behoort ontvangen.

De huidige versie van de Bus Service biedt geen ondersteuning voor SAS regels voor afzonderlijke abonnementen. Hetzelfde geldt voor de gebeurtenis Hubs consumentengroeperingen. Ondersteuning voor SAS wordt in de toekomst voor beide functies toegevoegd.

Bij afwezigheid van SAS-verificatie voor de individuele consument groepen kunt u sleutels voor SA's alle groepen van consumenten met een gemeenschappelijke sleutel te beveiligen. Deze benadering kan een toepassing gegevens opnemen uit een van de groepen consumenten van een Hub-gebeurtenis.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de gebeurtenis Hubs, gaat u naar de volgende onderwerpen:

- [Overzicht van de gebeurtenissen Hubs]
- Een [oplossing voor expresberichten in de wachtrij] met wachtrijen Service Bus.
- Een volledig [voorbeeldtoepassing die gebruikmaakt van gebeurtenis Hubs].

[Overzicht van de gebeurtenissen Hubs]: event-hubs-overview.md
[de voorbeeldtoepassing met gebeurtenis-Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[oplossing voor expresberichten in wachtrij]: ../service-bus-messaging/service-bus-dotnet-multi-tier-app-using-service-bus-queues.md
 
