<properties
    pageTitle="Beheer registratie"
    description="In dit onderwerp wordt uitgelegd hoe u apparaten registreren bij melding hubs push-meldingen ontvangen."
    services="notification-hubs"
    documentationCenter=".net"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-multiple"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

# <a name="registration-management"></a>Beheer registratie

##<a name="overview"></a>Overzicht

In dit onderwerp wordt uitgelegd hoe u apparaten registreren bij melding hubs push-meldingen ontvangen. Het onderwerp wordt beschreven registraties op een hoog niveau en vervolgens introduceert de twee belangrijkste patronen voor het registreren van apparaten: registratie van het apparaat rechtstreeks op de hub kennisgeving en registreren via de backend voor een toepassing. 


##<a name="what-is-device-registration"></a>Wat is registratie

De registratie met een Hub kennisgeving wordt gedaan met behulp van een **inschrijving** of **installatie**.

#### <a name="registrations"></a>Registraties
Een registratie koppelt de greep Platform Notification Service (PNS) voor een apparaat met tags en eventueel een sjabloon. De PNS-ingang kan zijn een ChannelURI, GCM registratie-id of token apparaat. Tags worden gebruikt om berichten doorsturen naar de juiste set van apparaat grepen. Zie [Routering en Tag expressies](notification-hubs-tags-segment-push-message.md)voor meer informatie. Sjablonen worden gebruikt voor het implementeren van transformatie per inschrijving. Zie [sjablonen](notification-hubs-templates-cross-platform-push-messages.md)voor meer informatie.

#### <a name="installations"></a>Installaties
Een installatie is een uitgebreide registratie met een groot aantal push gerelateerde eigenschappen. Het is de nieuwste en beste aanpak voor het registreren van uw apparaten. Het is echter niet ondersteund door de client side .NET SDK ([Melding Hub SDK voor backend bewerkingen](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)) op nog.  Dit betekent dat als u in het apparaat zelf registreert, zou u moeten gebruiken de [Melding Hubs REST API](https://msdn.microsoft.com/library/mt621153.aspx) aanpak ter ondersteuning van installaties. Als u een back-end-service, moet u [Melding Hub SDK voor back-end-bewerkingen](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)gebruiken zijn.

Dit zijn de belangrijkste voordelen van installaties:

* Maken of bijwerken van een installatie is volledig idempotency is ingeschakeld. Dus u kunt opnieuw proberen het zonder enige bezorgdheid over de dubbele registraties.
* Het model van de installatie kunt u gemakkelijk doen individuele ladingen - gericht op specifieke apparaat. Een tag systeem **"$InstallationId: [omwille van]"** wordt automatisch toegevoegd aan elke installatie op basis van registratie. Zo kunt u een verzenden naar deze code op een specifiek apparaat zonder extra codering kan bellen.
* Met installaties kunt u gedeeltelijke registratie-updates uitvoeren. De gedeeltelijke update van een installatie wordt met een PATCH-methode met behulp van de [standaard JSON-Patch](https://tools.ietf.org/html/rfc6902)aangevraagd. Dit is vooral handig als u wilt bijwerken, codes van de registratie. U hoeft niet te halen de volledige registratie en de vorige tags weer opnieuw te verzenden.

Een installatie kan bevatten de volgende eigenschappen. Voor een volledig overzicht van de installatie eigenschappen bekijken, [maken of een installatie met REST API overschrijven](https://msdn.microsoft.com/library/azure/mt621153.aspx) of [Installatie-eigenschappen](https://msdn.microsoft.com/library/azure/microsoft.azure.notificationhubs.installation_properties.aspx) voor de.

    // Example installation format to show some supported properties
    {
        installationId: "",
        expirationTime: "",
        tags: [],
        platform: "",
        pushChannel: "",
        ………
        templates: {
            "templateName1" : {
                body: "",
                tags: [] },
            "templateName2" : {
                body: "",
                // Headers are for Windows Store only
                headers: {
                    "X-WNS-Type": "wns/tile" }
                tags: [] }
        },
        secondaryTiles: {
            "tileId1": {
                pushChannel: "",
                tags: [],
                templates: {
                    "otherTemplate": {
                        bodyTemplate: "",
                        headers: {
                            ... }
                        tags: [] }
                }
            }
        }
    }

 

Het is belangrijk te weten dat registraties en installaties standaard niet meer verlopen.

Registraties en installaties moeten een geldige PNS-ingang voor elk apparaat/kanaal bevatten. Omdat PNS handgrepen alleen kunnen worden verkregen in een client-app op het apparaat, is het patroon van een rechtstreeks op het apparaat met de clienttoepassing registreren. Aan de andere kant, beveiligingsoverwegingen en bedrijfslogica die verband houden met de tags moet u mogelijk voor het beheren van de registratie in de app back-end. 

#### <a name="templates"></a>Sjablonen

Als u wilt met behulp van [sjablonen](notification-hubs-templates-cross-platform-push-messages.md), installatie van het apparaat ook houdt alle sjablonen die zijn gekoppeld aan dat apparaat in een JSON opmaken (Zie voorbeeld hierboven). De namen van helpen verschillende sjablonen voor hetzelfde apparaat doel.

Houd er rekening mee dat de naam van elke sjabloon worden toegewezen aan een instantie van de sjabloon en een optionele set codes. Elk platform kan bovendien extra eigenschappen hebben. Voor de Windows Store (met WNS) en Windows Phone 8 (met MPNS), kan een extra set headers deel uitmaken van de sjabloon. U kunt in het geval van APNs, een eigenschap verstrijken instellen of een constante of een sjabloonexpressie. Voor een volledig overzicht van de installatie Zie eigenschappen onderwerp [maken of een installatie met REST overschrijven](https://msdn.microsoft.com/library/azure/mt621153.aspx) .

#### <a name="secondary-tiles-for-windows-store-apps"></a>Secundaire tegels voor de Windows Store Apps

Voor de Windows Store-clienttoepassingen, meldingen verzenden naar secundaire tegels is hetzelfde als deze te verzenden naar het primaire beeldscherm. Dit wordt ook ondersteund in installaties. Houd er rekening mee dat secundaire tegels hebben een verschillende ChannelUri die de SDK op de client app transparant verwerkt.

De woordenlijst SecondaryTiles maakt gebruik van de dezelfde TileId die wordt gebruikt voor het maken van het SecondaryTiles-object in de Windows Store-app.
Net als bij de primaire ChannelUri kan ChannelUris van secundaire tegels op elk moment wijzigen. Om te voorkomen dat de installaties in de hub melding is bijgewerkt, moet het apparaat deze vernieuwen met de huidige ChannelUris van de secundaire tegels.


##<a name="registration-management-from-the-device"></a>Registratie van beheer van het apparaat

Als u de registratie van clienttoepassingen beheert, is alleen de back-end die verantwoordelijk is voor het verzenden van meldingen. Clienttoepassingen PNS grepen te houden en codes te registreren. In de volgende afbeelding ziet u dit patroon.

![](./media/notification-hubs-registration-management/notification-hubs-registering-on-device.png)

Het apparaat eerst de ingang PNS opgehaald uit de PNS en registreert bij de melding hub rechtstreeks. Nadat de registratie voltooid is, kan de backend app een melding voor deze inschrijving kunt verzenden. Zie [Routering en Tag expressies](notification-hubs-tags-segment-push-message.md)voor meer informatie over het verzenden van meldingen.
Merk op dat in dit geval gebruikt u luistert alleen rechten voor toegang tot uw melding hubs van het apparaat. Zie [beveiliging](notification-hubs-push-notification-security.md)voor meer informatie.

Registratie van het apparaat is de eenvoudigste methode, maar heeft enkele nadelen.
Het eerste nadeel is dat een clienttoepassing alleen de codes bijwerken kunt wanneer de toepassing actief is. Bijvoorbeeld als een gebruiker twee apparaten die codes gerelateerd aan sport teams, wanneer het eerste apparaat voor een aanvullende code (bijvoorbeeld Seahawks registreert) te registreren, ontvangt het tweede apparaat geen meldingen over de Seahawks totdat de app op het tweede apparaat een tweede keer wordt uitgevoerd. Meer in het algemeen wanneer tags worden beïnvloed door meerdere apparaten, is tags beheren vanaf de back-end een wenselijk optie.
Het tweede nadeel van registratie van beheer van de clienttoepassing is dat, aangezien apps kunnen worden hacked, extra aandacht de registratie te beveiligen vereist zoals wordt beschreven in de sectie "Tag-level security".



#### <a name="example-code-to-register-with-a-notification-hub-from-a-device-using-an-installation"></a>Van de voorbeeldcode te registreren met een hub melding van een apparaat met behulp van een installatie 

Dit wordt op dit moment alleen ondersteund met de [Melding Hubs REST API](https://msdn.microsoft.com/library/mt621153.aspx).

U kunt ook de methode van de PATCH met de [standaard JSON-Patch](https://tools.ietf.org/html/rfc6902) voor het bijwerken van de installatie.

    class DeviceInstallation
    {
        public string installationId { get; set; }
        public string platform { get; set; }
        public string pushChannel { get; set; }
        public string[] tags { get; set; }
    }

    private async Task<HttpStatusCode> CreateOrUpdateInstallationAsync(DeviceInstallation deviceInstallation,
         string hubName, string listenConnectionString)
    {
        if (deviceInstallation.installationId == null)
            return HttpStatusCode.BadRequest;

        // Parse connection string (https://msdn.microsoft.com/library/azure/dn495627.aspx)
        ConnectionStringUtility connectionSaSUtil = new ConnectionStringUtility(listenConnectionString);
        string hubResource = "installations/" + deviceInstallation.installationId + "?";
        string apiVersion = "api-version=2015-04";

        // Determine the targetUri that we will sign
        string uri = connectionSaSUtil.Endpoint + hubName + "/" + hubResource + apiVersion;

        //=== Generate SaS Security Token for Authorization header ===
        // See, https://msdn.microsoft.com/library/azure/dn495627.aspx
        string SasToken = connectionSaSUtil.getSaSToken(uri, 60);

        using (var httpClient = new HttpClient())
        {
            string json = JsonConvert.SerializeObject(deviceInstallation);

            httpClient.DefaultRequestHeaders.Add("Authorization", SasToken);

            var response = await httpClient.PutAsync(uri, new StringContent(json, System.Text.Encoding.UTF8, "application/json"));
            return response.StatusCode;
        }
    }

    var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

    string installationId = null;
    var settings = ApplicationData.Current.LocalSettings.Values;

    // If we have not stored a installation id in application data, create and store as application data.
    if (!settings.ContainsKey("__NHInstallationId"))
    {
        installationId = Guid.NewGuid().ToString();
        settings.Add("__NHInstallationId", installationId);
    }

    installationId = (string)settings["__NHInstallationId"];

    var deviceInstallation = new DeviceInstallation
    {
        installationId = installationId,
        platform = "wns",
        pushChannel = channel.Uri,
        //tags = tags.ToArray<string>()
    };

    var statusCode = await CreateOrUpdateInstallationAsync(deviceInstallation, 
                        "<HUBNAME>", "<SHARED LISTEN CONNECTION STRING>");

    if (statusCode != HttpStatusCode.Accepted)
    {
        var dialog = new MessageDialog(statusCode.ToString(), "Registration failed. Installation Id : " + installationId);
        dialog.Commands.Add(new UICommand("OK"));
        await dialog.ShowAsync();
    }
    else
    {
        var dialog = new MessageDialog("Registration successful using installation Id : " + installationId);
        dialog.Commands.Add(new UICommand("OK"));
        await dialog.ShowAsync();
    }

   

#### <a name="example-code-to-register-with-a-notification-hub-from-a-device-using-a-registration"></a>Van de voorbeeldcode te registreren met een hub melding van een apparaat dat een registratie


Deze methoden maken of bijwerken van een registratie voor het apparaat waarop zij worden genoemd. Dit betekent dat de volledige registratie bij te werken aan de ingang of de tags, moet overschrijven. Houd er rekening mee dat registraties tijdelijk, zijn zodat u moet altijd een betrouwbare winkel met de huidige codes waarvoor u een bepaald apparaat.


    // Initialize the Notification Hub
    NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(listenConnString, hubName);

    // The Device id from the PNS
    var pushChannel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

    // If you are registering from the client itself, then store this registration id in device
    // storage. Then when the app starts, you can check if a registration id already exists or not before
    // creating.
    var settings = ApplicationData.Current.LocalSettings.Values;

    // If we have not stored a registration id in application data, store in application data.
    if (!settings.ContainsKey("__NHRegistrationId"))
    {
        // make sure there are no existing registrations for this push handle (used for iOS and Android)    
        string newRegistrationId = null;
        var registrations = await hub.GetRegistrationsByChannelAsync(pushChannel.Uri, 100);
        foreach (RegistrationDescription registration in registrations)
        {
            if (newRegistrationId == null)
            {
                newRegistrationId = registration.RegistrationId;
            }
            else
            {
                await hub.DeleteRegistrationAsync(registration);
            }
        }

        newRegistrationId = await hub.CreateRegistrationIdAsync();

        settings.Add("__NHRegistrationId", newRegistrationId);
    }
     
    string regId = (string)settings["__NHRegistrationId"];

    RegistrationDescription registration = new WindowsRegistrationDescription(pushChannel.Uri);
    registration.RegistrationId = regId;
    registration.Tags = new HashSet<string>(YourTags);

    try
    {
        await hub.CreateOrUpdateRegistrationAsync(registration);
    }
    catch (Microsoft.WindowsAzure.Messaging.RegistrationGoneException e)
    {
        settings.Remove("__NHRegistrationId");
    }


## <a name="registration-management-from-a-backend"></a>Registratie van beheer van een end

Rapporten beheren vanaf de back-end moet extra code schrijven. De app van het apparaat moet opgeven voor de bijgewerkte PNS worden verwerkt op de backend telkens wanneer de toepassing wordt gestart (in combinatie met codes en sjablonen) en de back-end deze greep op de hub kennisgeving moet bijwerken. In de volgende afbeelding ziet u dit ontwerp.

![](./media/notification-hubs-registration-management/notification-hubs-registering-on-backend.png)

De registraties beheren vanaf de back-end voordelen tags om registraties te wijzigen, zelfs als de bijbehorende app op het apparaat niet actief is en de clienttoepassing verifiëren voordat u een label toe te voegen aan de registratie.


#### <a name="example-code-to-register-with-a-notification-hub-from-a-backend-using-an-installation"></a>Voorbeeldcode voor het registreren van een hub melding van een back-end met behulp van een installatie

Het apparaat nog steeds haalt de PNS-ingang en de eigenschappen van de betreffende installatie als vóór en een aangepaste API aanroept op de backend die kan uitvoeren van de registratie en toestaan van codes, enz. De backend kan gebruikmaken van de [Kennisgeving Hub SDK voor back-end-bewerkingen](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).

U kunt ook de methode van de PATCH met de [standaard JSON-Patch](https://tools.ietf.org/html/rfc6902) voor het bijwerken van de installatie.
 

    // Initialize the Notification Hub
    NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(listenConnString, hubName);

    // Custom API on the backend
    public async Task<HttpResponseMessage> Put(DeviceInstallation deviceUpdate)
    {

        Installation installation = new Installation();
        installation.InstallationId = deviceUpdate.InstallationId;
        installation.PushChannel = deviceUpdate.Handle;
        installation.Tags = deviceUpdate.Tags;

        switch (deviceUpdate.Platform)
        {
            case "mpns":
                installation.Platform = NotificationPlatform.Mpns;
                break;
            case "wns":
                installation.Platform = NotificationPlatform.Wns;
                break;
            case "apns":
                installation.Platform = NotificationPlatform.Apns;
                break;
            case "gcm":
                installation.Platform = NotificationPlatform.Gcm;
                break;
            default:
                throw new HttpResponseException(HttpStatusCode.BadRequest);
        }


        // In the backend we can control if a user is allowed to add tags
        //installation.Tags = new List<string>(deviceUpdate.Tags);
        //installation.Tags.Add("username:" + username);

        await hub.CreateOrUpdateInstallationAsync(installation);

        return Request.CreateResponse(HttpStatusCode.OK);
    }


#### <a name="example-code-to-register-with-a-notification-hub-from-a-device-using-a-registration-id"></a>Voorbeeldcode voor het registreren van een hub melding van een apparaat dat een registratie-id

U kunt van uw app-end CRUDS basisbewerkingen op registraties uitvoeren. Bijvoorbeeld:

    var hub = NotificationHubClient.CreateClientFromConnectionString("{connectionString}", "hubName");
            
    // create a registration description object of the correct type, e.g.
    var reg = new WindowsRegistrationDescription(channelUri, tags);

    // Create
    await hub.CreateRegistrationAsync(reg);

    // Get by id
    var r = await hub.GetRegistrationAsync<RegistrationDescription>("id");

    // update
    r.Tags.Add("myTag");

    // update on hub
    await hub.UpdateRegistrationAsync(r);

    // delete
    await hub.DeleteRegistrationAsync(r);


De back-end, gelijktijdigheid tussen registratie updates moet verwerken. Bus service biedt OCC. voor registratie van beheer. Dit wordt geïmplementeerd op http-niveau, met het gebruik van ETag op registratie beheertaken uit te voeren. Deze functie wordt transparant gebruikt door Microsoft SDKs, die een uitzondering genereren als een update is geweigerd om redenen van gelijktijdigheid. De backend app is verantwoordelijk voor het verwerken van deze uitzonderingen en de update opnieuw indien nodig.