<properties
    pageTitle="Geo omheind push-meldingen met Azure melding Hubs en Bing ruimtelijke gegevens | Microsoft Azure"
    description="In deze zelfstudie leert u locatie-gebaseerde push-meldingen met Azure melding Hubs en Bing ruimtelijke gegevens leveren."
    services="notification-hubs"
    documentationCenter="windows"
    keywords="push-bericht, push-bericht"
    authors="dend"
    manager="yuaxu"
    editor="dend"/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-phone"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="05/31/2016"
    ms.author="dendeli"/>
    
# <a name="geo-fenced-push-notifications-with-azure-notification-hubs-and-bing-spatial-data"></a>Geo omheind push-meldingen met Azure melding Hubs en Bing ruimtelijke gegevens
 
 > [AZURE.NOTE] Deze zelfstudie hebt u een actieve account Azure. Als u geen account hebt, kunt u een gratis proefperiode account in een paar minuten. Zie voor meer informatie, [Gratis proefperiode van Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02).

In deze zelfstudie leert u het leveren van locatie-gebaseerde push-meldingen met Azure melding Hubs en Bing ruimtelijke gegevens, uit binnen een toepassing Universal Windows Platform ontregeld.

##<a name="prerequisites"></a>Vereisten
Allereerst omdat u nodig om ervoor te zorgen dat u beschikt over alle de software en service minimumvereisten:

* [Visual Studio 2015 Update 1](https://www.visualstudio.com/en-us/downloads/download-visual-studio-vs.aspx) of hoger ([Community Edition](https://go.microsoft.com/fwlink/?LinkId=691978&clcid=0x409) zal ook doen). 
* Meest recente versie van de [SDK van Azure](https://azure.microsoft.com/downloads/). 
* [Dev Center van Bing Maps-account](https://www.bingmapsportal.com/) (u kunt een gratis maken en koppelen aan uw Microsoft-account). 

##<a name="getting-started"></a>Aan de slag

Laten we beginnen met het maken van het project. Start een nieuw project van het type **Leeg App (Universal Windows)**in Visual Studio.

![](./media/notification-hubs-geofence/notification-hubs-create-blank-app.png)

Zodra het project gemaakt is, beschikt u over de kabelboom voor de app zelf. Nu we stellen alles voor de geo-hekwerk-infrastructuur. Omdat we services voor deze Bing te gebruiken, is er een openbare eindpunt REST API waarmee wij specifieke locatie frames query:

    http://spatial.virtualearth.net/REST/v1/data/
    
U moet de volgende parameters voor het werken opgeven:

* **Gegevensbron-ID** en de **Naam van de gegevensbron** – Bing Maps API, gegevensbronnen bevatten verschillende bucketed metagegevens, zoals locaties en openingstijden van de bewerking. U kunt meer lezen over deze hier. 
* **Naam van de entiteit** : de entiteit die u wilt gebruiken als referentiepunt voor de melding. 
* **Bing Maps API-sleutel** – dit is de sleutel die u eerder hebt gekregen bij het maken van de Bing Dev Center-account.
 
We doen een uitvoerige op de instellingen voor elk van de bovenstaande elementen.

##<a name="setting-up-the-data-source"></a>De gegevensbron instellen

U kunt dit doen in de Bing Maps Dev Center. Gewoon klik u op de **gegevensbronnen** in de bovenste navigatiebalk en selecteer **Gegevensbronnen beheren**.

![](./media/notification-hubs-geofence/bing-maps-manage-data.png)

Als u niet met Bing Maps-API gewerkt heb, waarschijnlijk er niet alle gegevensbronnen aanwezig, zodat u gewoon een nieuw door te klikken op het uploaden van gegevens naar een gegevensbron kunt maken. Zorg ervoor dat u alle vereiste velden invullen:

![](./media/notification-hubs-geofence/bing-maps-create-data.png)

U kan vraagt zich af: Wat is het gegevensbestand en wat moet u worden uploaden? We kunnen op basis van pipe monster die frames van een gebied van de kust van San Francisco NET gebruiken voor de toepassing van deze test:

    Bing Spatial Data Services, 1.0, TestBoundaries
    EntityID(Edm.String,primaryKey)|Name(Edm.String)|Longitude(Edm.Double)|Latitude(Edm.Double)|Boundary(Edm.Geography)
    1|SanFranciscoPier|||POLYGON ((-122.389825 37.776598,-122.389438 37.773087,-122.381885 37.771849,-122.382186 37.777022,-122.389825 37.776598))
    
Het bovenstaande staat deze entiteit:

![](./media/notification-hubs-geofence/bing-maps-geofence.png)

Gewoon kopiëren en plak de bovenstaande tekenreeks in een nieuw bestand opslaan als **NotificationHubsGeofence.pipe**en in het midden van de Dev Bing uploaden.

>[AZURE.NOTE]U wordt mogelijk gevraagd om een nieuwe sleutel opgeven voor de **hoofdsleutel** die verschilt van de **Sleutel van de Query**. Gewoon een nieuwe sleutel via het dashboard maken en het uploaden van pagina vernieuwen.

Nadat u het bestand hebt geüpload, moet u om ervoor te zorgen dat u de gegevensbron hebt gepubliceerd. 

Ga naar **Gegevensbronnen beheren**, net zoals we hierboven hebben, vinden de gegevensbron in de lijst en klik op **publiceren** in de kolom **Acties** . Met een beetje ziet u de gegevensbron op het tabblad **Bronnen gepubliceerd** :

![](./media/notification-hubs-geofence/bing-maps-published-data.png)

Als u op **bewerken**klikt, kunt u zijn te zien in één oogopslag welke locaties we geïntroduceerd in het:

![](./media/notification-hubs-geofence/bing-maps-data-details.png)

Op dit moment wordt de portal niet weergegeven u de grenzen voor het geofence dat wij – alles wat we nodig hebben is een bevestiging dat de opgegeven locatie in de nabijheid van de juiste is.

U hebt nu de eisen voor de gegevensbron. Als u de informatie in de aanvraag-URL voor de API-aanroep in de Bing Maps Dev Center, klikt u op de **gegevensbronnen** en **Gegevens van de bron**te selecteren.

![](./media/notification-hubs-geofence/bing-maps-data-info.png)

De **Query-URL** is hier wat we zoeken. Dit is het eindpunt waartegen we query's om te controleren of het apparaat op dit moment binnen de grenzen van een locatie of niet is kunt uitvoeren. Als deze controle wordt uitgevoerd, moeten we gewoon een oproep krijgen tegen de URL query uitvoeren met de volgende parameters toegevoegd:

    ?spatialFilter=intersects(%27POINT%20LONGITUDE%20LATITUDE)%27)&$format=json&key=QUERY_KEY

Op die manier u een doelpunt die we van het apparaat en de Bing Maps krijgen wordt automatisch de berekeningen uitvoeren om te zien bepaalt of deze zich in de geofence. Nadat u de aanvraag via een browser (of krul) uitvoert, krijgt u standaard een JSON-reactie:

![](./media/notification-hubs-geofence/bing-maps-json.png)

Dit gebeurt alleen wanneer het punt dat daadwerkelijk binnen de grenzen van de aangewezen is. Als dat niet het geval is, krijgt u een emmer leeg **resultaten** :

![](./media/notification-hubs-geofence/bing-maps-nores.png)

##<a name="setting-up-the-uwp-application"></a>De UWP-toepassing instellen

Nu dat we de gegevensbron gereed hebben, we kunnen aan de slag op de UWP toepassing die we gestart met een bootstrap eerder.

In de allereerste plaats, moeten we locatie services voor onze toepassing inschakelen. Hiertoe dubbelklikt u op `Package.appxmanifest` bestand in de **Solution Explorer**.

![](./media/notification-hubs-geofence/vs-package-manifest.png)

Op het tabblad pakket-eigenschappen die u zojuist hebt geopend, klikt u op de **mogelijkheden** en zorg ervoor dat u de **locatie**selecteren:

![](./media/notification-hubs-geofence/vs-package-location.png)

Als de capaciteit van de locatie wordt aangegeven, een nieuwe map maken in de oplossing met de naam `Core`, en het toevoegen van een nieuw bestand binnen deze zogenaamde `LocationHelper.cs`:

![](./media/notification-hubs-geofence/vs-location-helper.png)

De `LocationHelper` klasse zelf is op dit moment vrij algemeen – deze methode wordt kunnen we de gebruikerslocatie via de systeem-API downloaden:

    using System;
    using System.Threading.Tasks;
    using Windows.Devices.Geolocation;

    namespace NotificationHubs.Geofence.Core
    {
        public class LocationHelper
        {
            private static readonly uint AppDesiredAccuracyInMeters = 10;

            public async static Task<Geoposition> GetCurrentLocation()
            {
                var accessStatus = await Geolocator.RequestAccessAsync();
                switch (accessStatus)
                {
                    case GeolocationAccessStatus.Allowed:
                        {
                            Geolocator geolocator = new Geolocator { DesiredAccuracyInMeters = AppDesiredAccuracyInMeters };

                            return await geolocator.GetGeopositionAsync();
                        }
                    default:
                        {
                            return null;
                        }
                }
            }

        }
    }

U kunt meer lezen over het opvragen van de locatie van de gebruiker in UWP apps in het officiële [document MSDN](https://msdn.microsoft.com/library/windows/apps/mt219698.aspx).

Om te controleren of de verwerving van locatie werkelijk werkt, opent u de kant van de code van de hoofdpagina (`MainPage.xaml.cs`). Maak een nieuwe gebeurtenis-handler voor de `Loaded` gebeurtenis in het `MainPage` constructor:

    public MainPage()
    {
        this.InitializeComponent();
        this.Loaded += MainPage_Loaded;
    }

De uitvoering van de gebeurtenis-handler is als volgt:

    private async void MainPage_Loaded(object sender, RoutedEventArgs e)
    {
        var location = await LocationHelper.GetCurrentLocation();

        if (location != null)
        {
            Debug.WriteLine(string.Concat(location.Coordinate.Longitude,
                " ", location.Coordinate.Latitude));
        }
    }

U ziet dat we de handler async verklaarde omdat `GetCurrentLocation` awaitable is en dat bijgevolg moet worden uitgevoerd in de context van een asynchrone. Ook, omdat onder bepaalde omstandigheden kunnen we uiteindelijk met een null-locatie (bv. de locatie services zijn uitgeschakeld of de aanvraag is geweigerd machtigingen voor toegang tot locatie), moeten we om te controleren of het correct wordt verwerkt met een null-cheque.

De toepassing wordt uitgevoerd. Zorg ervoor dat u de locatie toegang toestaan:

![](./media/notification-hubs-geofence/notification-hubs-location-access.png)

Zodra de toepassing gestart, moet u de coördinaten in het venster **Output** te zien:

![](./media/notification-hubs-geofence/notification-hubs-location-output.png)

Nu weet u locatie overname werkt – gerust verwijderen van de gebeurtenis-handler test voor geladen omdat we niet het niet meer gebruikt.

De volgende stap is voor het vastleggen van locatie verandert. Daarvoor gaan we terug naar de `LocationHelper` klasse en voor de gebeurtenis-handler toevoegen `PositionChanged`:

    geolocator.PositionChanged += Geolocator_PositionChanged;

De uitvoering worden de coördinaten van de locatie weergegeven in het venster **uitvoer** :

    private static async void Geolocator_PositionChanged(Geolocator sender, PositionChangedEventArgs args)
    {
        await CoreApplication.MainView.CoreWindow.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
        {
            Debug.WriteLine(string.Concat(args.Position.Coordinate.Longitude, " ", args.Position.Coordinate.Latitude));
        });
    }

##<a name="setting-up-the-backend"></a>Het instellen van de back-end

Het [Monster van .NET back-end van GitHub](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/NotifyUsers)downloaden. Nadat het downloaden is voltooid, opent u de `NotifyUsers` map, en vervolgens – de `NotifyUsers.sln` bestand.

Stel de `AppBackend` project als het **Project opstarten** en het starten.

![](./media/notification-hubs-geofence/vs-startup-project.png)

Het project is al geconfigureerd voor push-meldingen verzenden naar doelapparaten, dus moeten we twee dingen doen: vervangen de juiste verbinding de tekenreeks van de hub kennisgeving en grens-id om de melding te verzenden wanneer de gebruiker binnen de geofence toevoegen.

Configureren van de verbindingsreeks in de `Models` map geopend `Notifications.cs`. De `NotificationHubClient.CreateClientFromConnectionString` functie bevat de informatie over uw melding hub die u in de [Portal Azure](https://portal.azure.com) (uiterlijk binnen de blade **-Beleid** in de **instellingen krijgen kunt**). Sla het bestand bijgewerkt configuratie.

Nu moeten we een model voor het resultaat van de Bing Maps API maakt. De eenvoudigste manier om dat te doen is met de rechtermuisknop op de `Models` map **toevoegen** > **klasse**. Geef deze de naam `GeofenceBoundary.cs`. Zodra u klaar bent, de JSON van de API-reactie die we in de eerste sectie en gebruik van Visual Studio **bewerken besproken**kopiëren > **Plakken speciaal** > **JSON plakken als klassen**. 

Op die manier die we ervoor te zorgen dat het object deserialisatie wordt toegepast, zoals de bedoeling was. De resulterende set van de klasse moet er als volgt:

    namespace AppBackend.Models
    {
        public class Rootobject
        {
            public D d { get; set; }
        }

        public class D
        {
            public string __copyright { get; set; }
            public Result[] results { get; set; }
        }

        public class Result
        {
            public __Metadata __metadata { get; set; }
            public string EntityID { get; set; }
            public string Name { get; set; }
            public float Longitude { get; set; }
            public float Latitude { get; set; }
            public string Boundary { get; set; }
            public string Confidence { get; set; }
            public string Locality { get; set; }
            public string AddressLine { get; set; }
            public string AdminDistrict { get; set; }
            public string CountryRegion { get; set; }
            public string PostalCode { get; set; }
        }

        public class __Metadata
        {
            public string uri { get; set; }
        }
    }

Open vervolgens `Controllers`  >  `NotificationsController.cs`. We moeten de oproep boeken naar de rekening voor de doel-lengte en breedte aanpassen. Voor die gewoon twee tekenreeksen toevoegen aan de functiehandtekening – `latitude` en `longitude`.

    public async Task<HttpResponseMessage> Post(string pns, [FromBody]string message, string to_tag, string latitude, string longitude)

Maakt u een nieuwe klasse in het project met de naam `ApiHelper.cs` – we verbinding maken met Bing te controleren zult gebruiken snijpunten grens aan te wijzen. Implementeer een `IsPointWithinBounds` functie als volgt:

    public class ApiHelper
    {
        public static readonly string ApiEndpoint = "{YOUR_QUERY_ENDPOINT}?spatialFilter=intersects(%27POINT%20({0}%20{1})%27)&$format=json&key={2}";
        public static readonly string ApiKey = "{YOUR_API_KEY}";

        public static bool IsPointWithinBounds(string longitude,string latitude)
        {
            var json = new WebClient().DownloadString(string.Format(ApiEndpoint, longitude, latitude, ApiKey));
            var result = JsonConvert.DeserializeObject<Rootobject>(json);
            if (result.d.results != null && result.d.results.Count() > 0)
            {
                return true;
            }
            else
            {
                return false;
            }
        }
    }

>[AZURE.NOTE] Zorg ervoor dat het eindpunt API met de query-URL die u eerder hebt verkregen van de Bing Dev Center (geldt ook voor de API-sleutel) te vervangen. 

Als er resultaten van de query, die betekent dat het opgegeven punt binnen de grenzen van de geofence, zodat we terugkeren `true`. Als er geen resultaten, Bing is vertellen ons dat het is buiten het kader van de zoekactie, zodat we terugkeren `false`.

In `NotificationsController.cs`, een controle na de switchinstructie maken:

    if (ApiHelper.IsPointWithinBounds(longitude, latitude))
    {
        switch (pns.ToLower())
        {
            case "wns":
                //// Windows 8.1 / Windows Phone 8.1
                var toast = @"<toast><visual><binding template=""ToastText01""><text id=""1"">" +
                            "From " + user + ": " + message + "</text></binding></visual></toast>";
                outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, userTag);

                // Windows 10 specific Action Center support
                toast = @"<toast><visual><binding template=""ToastGeneric""><text id=""1"">" +
                            "From " + user + ": " + message + "</text></binding></visual></toast>";
                outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, userTag);

                break;
        }
    }

Op die manier de melding alleen verzonden wanneer het punt binnen de grenzen is.

##<a name="testing-push-notifications-in-the-uwp-app"></a>Push-meldingen in de UWP app testen

Gaat de app UWP, moet we nu kunnen meldingen te testen. In de `LocationHelper` klasse, maakt u een nieuwe functie: `SendLocationToBackend`:

    public static async Task SendLocationToBackend(string pns, string userTag, string message, string latitude, string longitude)
    {
        var POST_URL = "http://localhost:8741/api/notifications?pns=" +
            pns + "&to_tag=" + userTag + "&latitude=" + latitude + "&longitude=" + longitude;

        using (var httpClient = new HttpClient())
        {
            try
            {
                await httpClient.PostAsync(POST_URL, new StringContent("\"" + message + "\"",
                    System.Text.Encoding.UTF8, "application/json"));
            }
            catch (Exception ex)
            {
                Debug.WriteLine(ex.Message);
            }
        }
    }

>[AZURE.NOTE] Wisselen de `POST_URL` naar de locatie van de geïmplementeerde webtoepassing die we in de vorige sectie hebt gemaakt. Op dit moment is OK lokaal uitvoeren, maar als u over de implementatie van een openbare versie werkt, moet u deze met een externe aanbieder als host.

We controleren of dat we de UWP app voor push-meldingen registreren. In Visual Studio, klikt u op **Project** > **Opslaan** > **app in de winkel te koppelen**.

![](./media/notification-hubs-geofence/vs-associate-with-store.png)

Wanneer u zich bij uw account developer aanmeldt, Controleer of Selecteer een bestaande app of maak een nieuwe en het pakket aan gekoppeld. 

Ga naar de Dev Center en open de app die u zojuist hebt gemaakt. Klik op **Services** > **Push-meldingen** > **Live Services-site**.

![](./media/notification-hubs-geofence/ms-live-services.png)

Op de site, noteert u het **Geheim van de toepassing** en het **Pakket SID**. Zowel in de Portal Azure: open uw melding hub, klik op **Instellingen**, moet u > **Notification Services** > **Windows (WNS)** en voer de gegevens in de desbetreffende velden.

![](./media/notification-hubs-geofence/notification-hubs-wns.png)

Klik op **Opslaan**.

Klik met de rechtermuisknop op **References** in de **Solution Explorer** en selecteer **NuGet pakketten beheren**. We moeten een verwijzing naar de **Microsoft Azure Service Bus beheerde bibliotheek** toevoegen – gewoon zoeken naar `WindowsAzure.Messaging.Managed` en voeg deze toe aan uw project.

![](./media/notification-hubs-geofence/vs-nuget.png)

Voor testdoeleinden, maken we de `MainPage_Loaded` , nogmaals de gebeurtenis-handler en dit codefragment aan toe te voegen:

    var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

    var hub = new NotificationHub("HUB_NAME", "HUB_LISTEN_CONNECTION_STRING");
    var result = await hub.RegisterNativeAsync(channel.Uri);

    // Displays the registration ID so you know it was successful
    if (result.RegistrationId != null)
    {
        Debug.WriteLine("Reg successful.");
    }

Het bovenstaande registreert de app met de melding hub. U bent startklaar! 

In `LocationHelper`, binnen de `Geolocator_PositionChanged` -handler, kunt u een stukje code test die geforceerd de locatie in de geofence toevoegen:

    await LocationHelper.SendLocationToBackend("wns", "TEST_USER", "TEST", "37.7746", "-122.3858");

Omdat we de reële coördinaten (die mogelijk binnen de grenzen van de op het moment) niet doorgeeft en vooraf gedefinieerde testwaarden gebruikt, zien we een melding weergegeven op de update:

![](./media/notification-hubs-geofence/notification-hubs-test-notification.png)

##<a name="whats-next"></a>Hoe nu verder?

Er zijn een aantal stappen die u moet kan volgen naast de hierboven om ervoor te zorgen dat de oplossing klaar voor productie is.

In de allereerste plaats, is het mogelijk om ervoor te zorgen dat de dynamische geofences zijn. Dit vereist wat extra werk met de Bing-API te kunnen uploaden, nieuwe grenzen binnen de bestaande gegevensbron. Raadpleeg de [Bing ruimtelijke Data Services API-documentatie](https://msdn.microsoft.com/library/ff701734.aspx) voor meer informatie over dit onderwerp.

Ten tweede, terwijl u werkt om ervoor te zorgen dat de levering wordt gedaan aan de juiste deelnemers, u kunt richten op via [tags](notification-hubs-tags-segment-push-message.md).

De bovenstaande oplossing beschrijft een scenario waarin u mogelijk een groot aantal doelplatforms, zodat de geofencing met systeem-specifieke mogelijkheden is niet beperkt. Dat gezegd, de Universal Windows Platform biedt mogelijkheden voor het [detecteren van geofences rechts out-of-the-box-](https://msdn.microsoft.com/windows/uwp/maps-and-location/set-up-a-geofence)zijn.

Bekijk onze [documentatie portal](https://azure.microsoft.com/documentation/services/notification-hubs/)voor meer informatie over de mogelijkheden voor berichtgeving Hubs.
