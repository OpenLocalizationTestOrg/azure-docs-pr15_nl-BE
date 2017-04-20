<properties 
    pageTitle="Streaming-logboeken en console" 
    description="Streaming-logboeken en Prestatieconsole-overzicht" 
    authors="btardif" 
    manager="wpickett" 
    editor="" 
    services="app-service\web" 
    documentationCenter=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="10/12/2016" 
    ms.author="byvinyal"/>

# <a name="streaming-logs-and-the-console"></a>Streaming-logboeken en de Console

## <a name="streaming-logs"></a>Logboeken voor streaming

De **Azure portal** biedt een geïntegreerde streaming logboeken die u kunt u in real-time van traceringsgebeurtenissen vanuit uw apps **App-Service** .  

Deze functie instelt, moet een paar eenvoudige stappen:

- Sporen in uw code te schrijven
- Toepassing **Diagnostische logboeken** voor uw app inschakelen
- De stroom uit de ingebouwde gebruikersinterface van **Logboeken Streaming** in **Azure portal**weergeven.

### <a name="how-to-write-traces-in-your-code"></a>Het schrijven van sporen in code ###

Sporen in uw code schrijven is eenvoudig.  In C# is het net zo eenvoudig als het schrijven van de volgende code:

`````````````````````````
Trace.TraceInformation("My trace statement");
`````````````````````````

`````````````````````````
Trace.TraceWarning("My warning statement");
`````````````````````````

`````````````````````````
Trace.TraceError("My error statement");
`````````````````````````

De klasse Trace woont in de naamruimte System.Diagnostics.

U kunt deze code om hetzelfde resultaat bereiken schrijven in een node.js app:

`````````````````````````
console.log("My trace statement").
`````````````````````````

### <a name="how-to-enable-and-view-the-streaming-logs"></a>Gebeurtenislogboeken inschakelen en de streaming weergeven
![][BrowseSitesScreenshot]Diagnostische gegevens zijn op basis van per app ingeschakeld. Start door te bladeren naar de site die u wilt deze functie inschakelen.  
  
![][DiagnosticsLogs]Ga naar de sectie **controleren** vanuit het menu instellingen en klik op **Diagnostische logboeken (1)**. Vervolgens **inschakelen (2)** **Toepassing Logging (Filesystem)** **Melden** of de toepassing (blob) de optie **niveau** kunt u de ernst van de traces om vast te leggen. Als u alleen probeert om vertrouwd te raken met de functie, kunt u het niveau instelt op **uitgebreid** , zodat alle van de trace-instructies worden verzameld.

Klik op **Opslaan** op de bovenkant van het blad en bent u klaar om logboeken weer te geven.

>[AZURE.NOTE] Hoe hoger worden het **prioriteitsniveau** meer bronnen worden verbruikt, logboek en de meer sporen geproduceerd. Zorg ervoor dat het **ernst** is geconfigureerd voor de juiste gegevens opnemen voor een productie- of site met veel verkeer. 

![][StreamingLogsScreenshot]De **Logboeken voor streaming** vanaf binnen de portal Azure, klikt u op op **(1) Log Stream** ook in de sectie **bewaking** van het menu instellingen. Als uw app actief trace-instructies schrijven wordt, ziet u ze in de **gebruikersinterface (2) streaming zich** in bijna real-time.

## <a name="console"></a>Console
De **Azure portal** biedt toegang tot uw app. U kunt van uw app bestandssysteem te verkennen en powershell/cmd-scripts uitvoeren. Gebonden bent aan dezelfde machtigingen als uw app programmacode instellen bij het uitvoeren van opdrachten. Toegang tot beschermde mappen of het uitvoeren van scripts waarvoor verhoogde bevoegdheden is geblokkeerd.  

![][ConsoleScreenshot]Uit het menu instellingen, blader naar de sectie **Ontwikkelprogramma's** en klik op de **Console (1)** en **(2) console** UI geopend aan de rechterkant.

Als u bekend bent met de **console**, probeer basisopdrachten, zoals:

`````````````````````````
dir
`````````````````````````

`````````````````````````
cd
`````````````````````````

<!-- Images. -->
[DiagnosticsLogs]: ./media/web-sites-streaming-logs-and-console/diagnostic-logs.png
[BrowseSitesScreenshot]: ./media/web-sites-streaming-logs-and-console/browse-sites.png
[StreamingLogsScreenshot]: ./media/web-sites-streaming-logs-and-console/streaming-logs.png
[ConsoleScreenshot]: ./media/web-sites-streaming-logs-and-console/console.png
