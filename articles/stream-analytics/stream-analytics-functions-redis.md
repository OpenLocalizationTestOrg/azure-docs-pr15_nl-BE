<properties
    pageTitle="Uitvoer naar een Azure bestand Vgx. Cache met Azure, functies van Azure Stream Analytics | Microsoft Azure"
    description="Meer informatie over het gebruik van een functie Azure een wachtrij Service Bus, om in te vullen een Azure Cache bestand Vgx. uit de resultaten van een Stream Analytics taak verbonden."
    keywords="gegevensstroom cache bestand Vgx. service bus wachtrij"
    services="stream-analytics"
    authors="ryancrawcour"
    manager="jhubbard"
    documentationCenter=""
    />

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="09/26/2016"
    ms.author="ryancraw"/>

# <a name="how-to-store-data-from-azure-stream-analytics-in-an-azure-redis-cache-using-azure-functions"></a>Het opslaan van gegevens van Azure Stream Analytics in een Azure bestand Vgx. Cache met Azure functies

Azure Stream Analytics kunt u snel ontwikkelen en implementeren van real-time inzicht van apparaten, sensoren, infrastructuur en toepassingen of een gegevensstroom krijgen goedkope oplossingen. U kunt verschillende use-cases zoals real-time management en opdracht en controle, opsporing van fraude, verbonden auto's en nog veel meer. Veel dergelijke scenario's, u kunt in voor het opslaan van gegevens die zijn gegenereerd door Azure Stream Analytics in een gedistribueerde gegevensarchief zoals een cache Azure bestand Vgx..

Stel dat u deel uitmaken van een telecommunicatie-bedrijf. U probeert te detecteren SIM fraude waarbij meerdere aanroepen die afkomstig zijn van dezelfde identiteit op dezelfde tijd, maar in verschillende geografisch locaties. U zijn met name alle mogelijke frauduleuze telefoongesprekken op te slaan in een cache Azure bestand Vgx.. In deze blog bieden we richtlijnen op hoe u gemakkelijk uw taak kunt voltooien. 

## <a name="prerequisites"></a>Vereisten
De [Real-time fraude-opsporing] [ fraud-detection] overzicht voor ASA

## <a name="architecture-overview"></a>Architectuur-overzicht
![Screenshot van architectuur](./media/stream-analytics-functions-redis/architecture-overview.png)

Zoals in de voorgaande afbeelding, kunt u Analytics Stream streaming ingevoerde gegevens worden opgevraagd en verzonden naar een output. Op basis van de uitvoer, kunnen Azure functies vervolgens activeren sommige type gebeurtenis. 

In deze blog richten we ons op de Azure functies deel van deze pijpleiding of meer in het bijzonder het genereren van een gebeurtenis die frauduleus gegevens in de cache worden opgeslagen.
Na het voltooien van de [Fraude-opsporing van Real-time] [ fraud-detection] zelfstudie, hebt u een invoer (een hub gebeurtenis), een query en een output (blob storage) al geconfigureerd en uitgevoerd. In deze blog wijzigen we de uitvoer in plaats daarvan gebruik van een Service Bus wachtrij. Na dat verbinding we een Azure-functie naar deze wachtrij. 

## <a name="create-and-connect-a-service-bus-queue-output"></a>Maken en verbinding maken met een Bus-mailwachtrij Service-uitvoer
Een Service Bus om wachtrij te maken, stappen 1 en 2 van de sectie .NET in [Aan de slag met servicewachtrijen Bus][servicebus-getstarted].
Nu gaan we de wachtrij verbinding te maken met de gegevensstroom Analytics taak die is gemaakt in de eerdere fraude detectie overzicht.



1. Ga naar de blade **uitgangen** van uw project in het portal voor Azure en **toevoegen** aan de bovenkant van de pagina te selecteren.

    ![Uitvoer toe te voegen](./media/stream-analytics-functions-redis/adding-outputs.png)

2. Kies **Service Bus wachtrij** als het **opvangen** en volg de aanwijzingen op het scherm. Kies de naamruimte van de Service Bus-wachtrij die u hebt gemaakt in het [Aan de slag met servicewachtrijen Bus][servicebus-getstarted]. Klik op 'right' wanneer u klaar bent.
3. De volgende waarden opgeven:
    - **Gebeurtenis Serializer indeling**: JSON
    - **Codering**: UTF8
    - **Indeling**: regel gescheiden

4. Klik op de knop **maken** om toe te voegen deze bron en om te controleren dat Stream Analytics verbinding met de account van de opslag maken kan.

5. In het tabblad **Query** vervangen door de huidige query de volgende. *[Uw BUS SERVICENAAM]* vervangen door de output-naam die u in stap 3 hebt gemaakt. 

    ```    

        SELECT 
            System.Timestamp as Time, CS1.CallingIMSI, CS1.CallingNum as CallingNum1, 
            CS2.CallingNum as CallingNum2, CS1.SwitchNum as Switch1, CS2.SwitchNum as Switch2

        INTO [YOUR SERVICE BUS NAME]
    
        FROM CallStream CS1 TIMESTAMP BY CallRecTime
        JOIN CallStream CS2 TIMESTAMP BY CallRecTime
            ON CS1.CallingIMSI = CS2.CallingIMSI AND DATEDIFF(ss, CS1, CS2) BETWEEN 1 AND 5
    
        WHERE CS1.SwitchNum != CS2.SwitchNum
    
    ```

## <a name="create-an-azure-redis-cache"></a>Maak een Cache bestand Vgx Azure.
Een cache Azure bestand Vgx. Maak aan de hand van de .NET-sectie in [het bestand Vgx. Cache voor gebruik Azure] [ use-rediscache] totdat de sectie genaamd ***de cache-clients configureren***.
Als voltooid, hebt u een nieuw bestand Vgx. Cache. Selecteer onder **alle instellingen** **toegangstoetsen** en noteer de ***primaire verbinding string***.

![Screenshot van architectuur](./media/stream-analytics-functions-redis/redis-cache-keys.png)

## <a name="create-an-azure-function"></a>Maak een functie Azure
Volg [uw eerste Azure functie maken] [ functions-getstarted] zelfstudie aan de slag met Azure functies. Als u al een Azure-functie wilt gebruiken, vervolgens gaat u verder met [het schrijven naar Cache bestand Vgx.](#Writing-to-Redis-Cache)

1. In de portal App Services selecteren in de linkernavigatiebalk en klik u vervolgens op de naam van uw app Azure functie om naar de website van de functie-app.
    ![Screenshot van de lijst met services functie app](./media/stream-analytics-functions-redis/app-services-function-list.png)

2. Klik op **nieuwe functie > ServiceBusQueueTrigger-C#**. Volg deze instructies voor de volgende velden:
    - **Naam wachtrij**: dezelfde naam als de naam die u hebt opgegeven bij het maken van de wachtrij in [Aan de slag met servicewachtrijen Bus] [ servicebus-getstarted] (niet de naam van de service bus). Zorg ervoor dat u de wachtrij die is verbonden met de output Stream Analytics gebruiken.
    - **Service Bus verbinding**: Selecteer **een verbindingsreeks toevoegen**. Ga naar de klassieke portal om de verbindingsreeks, selecteert u **Bus Service**, de service bus die u hebt gemaakt en **VERBINDINGSGEGEVENS** aan de onderkant van het scherm. Zorg ervoor dat u in het hoofdscherm op deze pagina. Kopieer en plak de verbindingsreeks. Gerust voert u de naam van de verbinding.
    
        ![Screenshot van service bus verbinding](./media/stream-analytics-functions-redis/servicebus-connection.png)
    - **AccessRights**: kies **beheren**


3. Klik op **maken**

## <a name="writing-to-redis-cache"></a>Schrijven naar Cache bestand Vgx.
We hebben nu een Azure-functie die van een Service Bus wachtrij wordt gemaakt. Die nog moet doen, is onze functie gebruiken deze gegevens te schrijven naar het bestand Vgx. Cache. 

1. Selecteer de zojuist gemaakte **ServiceBusQueueTrigger**en klik op **functie app instellingen** in de rechterbovenhoek. Selecteer **Ga naar de App instellingen > Instellingen > instellingen voor toepassing**

2. Maak een naam in de sectie **naam** in de sectie Connection strings. De primaire verbindingsreeks die u in het **maken van een Cache bestand Vgx.** stap naar het gedeelte **waarde gevonden** te plakken. Selecteer **aangepaste** **SQL-Database**eens.

3. Klik op **Opslaan** op de bovenkant.

    ![Screenshot van service bus verbinding](./media/stream-analytics-functions-redis/function-connection-string.png)

4. Nu terug naar de App-instellingen en selecteer **Extra > App Service Editor (voorbeeld) > op > Ga naar**.

    ![Screenshot van service bus verbinding](./media/stream-analytics-functions-redis/app-service-editor.png)

5. JSON-bestand met de naam **project.json** met de volgende maken en opslaan op uw lokale schijf in een editor van uw keuze.

        {
            "frameworks": {
                "net46": {
                    "dependencies": {
                        "StackExchange.Redis":"1.1.603",
                        "Newtonsoft.Json": "9.0.1"
                    }
                }
            }
        }

6. Upload dit bestand naar de hoofdmap van de functie (geen WWWROOT). U ziet een bestand genaamd **project.lock.json** automatisch wordt weergegeven, waarin wordt bevestigd dat de Nuget pakketten 'StackExchange.Redis' en 'Newtonsoft.Json' zijn geïmporteerd.

7. Vervang de vooraf gegenereerde code met de volgende code in het bestand **run.csx** . De naam die u hebt gemaakt in stap 2 van **de gegevens opslaan in de cache voor het bestand Vgx.**vervangen "CONN naam" in de functie lazyConnection.

````

    using System;
    using System.Threading.Tasks;
    using StackExchange.Redis;
    using Newtonsoft.Json;
    using System.Configuration;
    
    public static void Run(string myQueueItem, TraceWriter log)
    {
        log.Info($"Function processed message: {myQueueItem}");

        // Connection refers to a property that returns a ConnectionMultiplexer
        IDatabase db = Connection.GetDatabase();
        log.Info($"Created database {db}");
    
        // Parse JSON and extract the time
        var message = JsonConvert.DeserializeObject<dynamic>(myQueueItem);
        string time = message.time;
        string callingnum1 = message.callingnum1;

        // Perform cache operations using the cache object...
        // Simple put of integral data types into the cache
        string key = time + " - " + callingnum1;
        db.StringSet(key, myQueueItem);
        log.Info($"Object put in database. Key is {key} and value is {myQueueItem}");

        // Simple get of data types from the cache
        string value = db.StringGet(key);
        log.Info($"Database got: {value}"); 
    }

    // Connect to the Service Bus
    private static Lazy<ConnectionMultiplexer> lazyConnection = 
        new Lazy<ConnectionMultiplexer>(() =>
            {
                var cnn = ConfigurationManager.ConnectionStrings["CONN NAME"].ConnectionString
                return ConnectionMultiplexer.Connect();
            });
    
    public static ConnectionMultiplexer Connection
    {
        get
        {
            return lazyConnection.Value;
        }
    }

````

## <a name="start-the-stream-analytics-job"></a>De taak van de Stream Analytics start

1. Start de toepassing telcodatagen.exe. De syntaxis is als volgt:````telcodatagen.exe [#NumCDRsPerHour] [SIM Card Fraud Probability] [#DurationHours]````

2. Klik op de Stream Analytics taak blade in de portal, **beginnen** boven aan de pagina.

    ![Screenshot van starttaak](./media/stream-analytics-functions-redis/starting-job.png)

3. In het blad **taak starten** **nu** selecteren en klik vervolgens op de knop **Start** op de onderkant van het scherm. De taak wordt gewijzigd om te starten en na bepaalde tijdwijzigingen uitgevoerd.
 
    ![Screenshot van start project tijd selectie](./media/stream-analytics-functions-redis/start-job-time.png)

## <a name="run-solution-and-check-results"></a>Oplossing uitvoert en de resultaten controleren
Teruggaan naar de pagina **ServiceBusQueueTrigger** , ziet u nu log-instructies. Deze logboeken weergeven die u hebt u iets uit de Service Bus plaatsen in de database, en deze uit met behulp van de tijd als de sleutel opgehaald!

Als u wilt controleren of de gegevens in de cache voor het bestand Vgx. is, gaat u naar de pagina cache bestand Vgx. in de nieuwe portal (zoals weergegeven in de voorgaande stap voor [het maken een Cache Azure bestand Vgx.](#Create-an-Azure-Redis-Cache) ) en selecteer Console.

U kunt nu schrijven bestand Vgx. opdrachten om te bevestigen dat gegevens in feite in de cache.

![Screenshot van Console bestand Vgx.](./media/stream-analytics-functions-redis/redis-console.png)

## <a name="next-steps"></a>Volgende stappen
Wij zijn verheugd over de nieuwe functies van Azure-zaken en we hopen dat dit nieuwe mogelijkheden voor u ontgrendelt Stream analytics samen kunt doen. Als u feedback wilt vervolgens, gerust de [UserVoice Azure-site](https://feedback.azure.com/forums/270577-stream-analytics)te gebruiken.

Als u nieuwe Microsoft Azure, nodigen we u uitproberen door het aanmelden voor een [gratis proefperiode Azure-account](https://azure.microsoft.com/pricing/free-trial/). Als u nieuwe aan Analytics Stream, vervolgens nodigen we u uit voor het [maken van uw eerste taak van de Stream Analytics](stream-analytics-create-a-job.md).

Als u een help of vragen, boeken op [MSDN](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics) of [Stackoverflow](http://stackoverflow.com/questions/tagged/azure-stream-analytics) forums. 

U ziet ook de volgende bronnen:

- [Azure functies: referentie voor ontwikkelaars](../azure-functions/functions-reference.md)
- [Azure functies in C#: referentie voor ontwikkelaars](../azure-functions/functions-reference-csharp.md)
- [Azure functies F #: referentie voor ontwikkelaars](../azure-functions/functions-reference-fsharp.md)
- [Azure NodeJS functies: referentie voor ontwikkelaars](../azure-functions/functions-reference.md)
- [Azure functies, triggers en bindingen](../azure-functions/functions-triggers-bindings.md)
- [Het controleren van Azure-Cache bestand Vgx.](../redis-cache/cache-how-to-monitor.md)

Als u wilt op de hoogte blijven van het laatste nieuws en functies, gaat u als volgt [@AzureStreaming](https://twitter.com/AzureStreaming) op Twitter.


[fraud-detection]: stream-analytics-real-time-fraud-detection.md
[servicebus-getstarted]: ../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md
[use-rediscache]: ../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md
[functions-getstarted]: ../azure-functions/functions-create-first-azure-function.md
