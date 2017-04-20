<properties
    pageTitle="Beste praktijken en de troubleshooting guide for knooppunt toepassingen op Azure Web Apps"
    description="Informatie over de aanbevolen procedures en stappen voor probleemoplossing voor knooppunt toepassingen op Azure Web Apps."
    services="app-service\web"
    documentationCenter="nodejs"
    authors="ranjithr"
    manager="wadeh"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="article"
    ms.date="06/06/2016"
    ms.author="ranjithr;wadeh"/>
    
# <a name="best-practices-and-troubleshooting-guide-for-node-applications-on-azure-web-apps"></a>Beste praktijken en de troubleshooting guide for knooppunt toepassingen op Azure Web Apps

[AZURE.INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

In dit artikel leert u de aanbevolen procedures en stappen voor probleemoplossing voor [knooppunt toepassingen](app-service-web-nodejs-get-started.md) op Azure Webapps (met [iisnode](https://github.com/azure/iisnode)).

>[AZURE.WARNING] Wees voorzichtig wanneer u stappen op uw productiesite. Aanbeveling is het oplossen van problemen met uw app op een niet-productie-instellingen zoals de staging-sleuf en als het probleem wordt opgelost, wisselen de staging-sleuf met sleuf voor uw productie.

## <a name="iisnode-configuration"></a>IISNODE configuratie

Dit [bestand](https://github.com/Azure/iisnode/blob/master/src/config/iisnode_schema_x64.xml) bevat alle instellingen die kunnen worden geconfigureerd voor iisnode. Sommige van de instellingen die nuttig is voor uw toepassing zijn:

* nodeProcessCountPerApplication

    Deze instelling bepaalt u het aantal processen dat knooppunt per IIS-toepassing wordt gestart. Standaardwaarde is 1. U kunt zoveel node.exe als uw core VM aantal starten door deze op 0. Aanbevolen waarde is 0 voor de meeste toepassingen, zodat u van alle van de cores op uw computer gebruikmaken kunt. Node.exe is één thread zodat een node.exe van 1 core en geniet van maximale prestaties uit de toepassing van het knooppunt kunt u gebruikmaken van alle cores maximaal verbruiken.

* nodeProcessCommandLine

    Deze instelling bepaalt het pad naar de node.exe. U kunt deze waarde om te verwijzen naar de versie van uw node.exe instellen.

* maxConcurrentRequestsPerProcess

    Deze instelling bepaalt het maximum aantal gelijktijdige aanvragen die door iisnode worden verzonden naar elke node.exe. Op azure webapps is de standaardwaarde voor deze oneindig. U geen zorgen te maken over deze instelling. De standaardwaarde is buiten azure webapps, 1024. U kunt dit configureren afhankelijk van hoeveel aanvragen dat de toepassing krijgt en hoe snel uw toepassing verwerkt de aanvraag.

* maxNamedPipeConnectionRetry

    Deze instelling bepaalt het maximum aantal keren iisnode maken verbinding met de named pipe waarnaar de aanvraag aan de node.exe zal worden herhaald. Deze instelling in combinatie met de namedPipeConnectionRetryDelay bepaalt de totale tijd van elke aanvraag binnen iisnode. Standaardwaarde is 200 op Azure Webapps. Totaal van de time-out in seconden = (maxNamedPipeConnectionRetry \* namedPipeConnectionRetryDelay) / 1000

* namedPipeConnectionRetryDelay

    Deze instelling bepaalt u de hoeveelheid tijd (in ms) iisnode wacht tussen nieuwe pogingen te verzenden via de named pipe aanvraag naar node.exe. Standaardwaarde is 250ms.
    Totaal van de time-out in seconden = (maxNamedPipeConnectionRetry \* namedPipeConnectionRetryDelay) / 1000

    Standaard is de time-out van de totale in iisnode op azure webapps 200 \* 250ms = 50 seconden.

* logDirectory

    Deze instelling bepaalt de directory waar iisnode stdout/stderr wordt vastgelegd. Dit is de iisnode die is gerelateerd aan de belangrijkste script map (directory waarin de belangrijkste server.js aanwezig is)

* debuggerExtensionDll

    Deze instelling bepaalt welke versie van knooppunt inspector iisnode wordt gebruikt bij het opsporen van fouten in de toepassing van het knooppunt. Inspector-iisnode-0.7.3.dll en iisnode inspector.dll zijn momenteel slechts 2 geldige waarden voor deze instelling. Dit is de iisnode-inspecteur-0.7.3.dll. inspector-iisnode-0.7.3.dll-versie gebruikt knooppunt-inspecteur-0.7.3 en maakt gebruik van websockets, dus moet u websockets op uw azure webapp om deze versie te gebruiken inschakelen. Zie <http://www.ranjithr.com/?p=98> voor meer informatie over het configureren van iisnode voor het gebruik van het nieuwe knooppunt-inspecteur.

* flushResponse

    Het standaardgedrag van IIS is dat deze buffers Responsgegevens van tot 4MB voor afboeken, of tot het einde van het antwoord, afhankelijk van wat zich het eerst. iisnode biedt een configuratie-instelling dit gedrag wijzigen: een fragment van het hoofdgedeelte van de response leeggemaakt zodra iisnode het ontvangen van node.exe, moet u de iisnode/@flushResponse kenmerk in web.config op 'true':
    
    ```
    <configuration>    
        <system.webServer>    
            <!-- ... -->    
            <iisnode flushResponse="true" />    
        </system.webServer>    
    </configuration>
    ```

    Inschakelen van afboeking van elk fragment van het hoofdgedeelte van de response beïnvloedt de systeemprestaties waardoor de doorvoer van het systeem met ~ 5% (op v0.1.13), is het verstandig om het bereik van deze instelling alleen voor eindpunten die streaming reactie vereisen (bijvoorbeeld met de <location> -element in het bestand web.config)

    Daarnaast moet voor streaming van toepassingen, u ook de responseBufferLimit van de iisnode-handler ingesteld op 0.
    
    ```
    <handlers>    
        <add name="iisnode" path="app.js" verb="\*" modules="iisnode" responseBufferLimit="0"/>    
    </handlers>
    ```

* watchedFiles

    Dit is een puntkomma's gescheiden lijst met bestanden die wijzigingen zullen worden bekeken. Een wijziging in een bestand, wordt de toepassing te recyclen. Elke vermelding bestaat uit een optionele mapnaam plus vereiste bestandsnaam zijn ten opzichte van de map waarin het ingangspunt van het hoofdvenster van de toepassing zich bevindt. Jokertekens zijn toegestaan in het bestand-gedeelte alleen. Standaardwaarde is '\*. js;web.config "

* recycleSignalEnabled

    Standaardwaarde is false. Als ingeschakeld, uw knooppunt toepassing verbinding kan maken met een named pipe (omgevingsvariabele IISNODE\_CONTROL\_PIPE) en een "Prullenbak"-bericht verzenden. Hierdoor wordt de w3wp te recyclen, zonder problemen.

* idlePageOutTimePeriod

    Standaardwaarde is 0, wat betekent dat deze functie is uitgeschakeld. Als de waarde naar een waarde groter dan 0, wordt iisnode pagina uit alle onderliggende processen elke milliseconden 'idlePageOutTimePeriod'. Om te begrijpen welke pagina van de middelen, Raadpleeg deze [documentatie](https://msdn.microsoft.com/library/windows/desktop/ms682606.aspx). Deze instelling is handig voor toepassingen die veel geheugen in beslag nemen en pageout-geheugen naar de schijf af en toe wilt vrijmaken RAM-geheugen.

>[AZURE.WARNING] Wees voorzichtig met het inschakelen van de volgende configuratie-instellingen voor productietoepassingen. Advies is om niet op de van live-productietoepassingen.

* debugHeaderEnabled

    De standaardwaarde is false. Als het is ingesteld op true, iisnode een HTTP response-header iisnode-debug toevoegt aan elke HTTP-antwoord verzendt de waarde van de header iisnode debug is een URL. Afzonderlijke stukjes diagnostische gegevens kunnen worden die zijn opgedaan door te kijken naar de URL-fragment, maar een veel betere visualisatie wordt bereikt door de URL in de browser openen.

* loggingEnabled

    Deze instelling bepaalt de registratie van stdout en stderr door iisnode. Iisnode wordt stdout/stderr van knooppunt processen wordt het vastleggen en schrijven naar de map die is opgegeven met de instelling 'logDirectory'. Zodra dit is inschakelen, uw toepassing worden logboeken worden schrijven naar het bestandssysteem en het bedrag van de registratie door de toepassing wordt uitgevoerd, kan er gevolgen voor prestaties.

* devErrorsEnabled

    Standaardwaarde is false. Wanneer ingesteld op true, iisnode de HTTP-statuscode en de Win32-foutcode in de browser weergegeven wordt. De win32-code is handig bij het opsporen van fouten in bepaalde soorten problemen.
    
* debuggingEnabled (niet inschakelen op productiesite live)

    Deze instelling bepaalt u de functie voor foutopsporing. Iisnode is geïntegreerd met het knooppunt inspector. Als u deze instelling inschakelt, inschakelen u foutopsporing van de toepassing van het knooppunt. Als deze instelling is ingeschakeld, wordt iisnode lay-out de knooppunt-inspecteur nodig bestanden in de map 'debuggerVirtualDir' bij de eerste aanvraag voor foutopsporing op uw toepassing knooppunt. U kunt de knooppunt-inspecteur laden door een verzoek te sturen naar http://yoursite/server.js/debug. Met de instelling 'debuggerPathSegment' kunt u de URL debug segment bepalen. Door de standaard debuggerPathSegment = 'debug'. U kunt instellen dit naar een GUID bijvoorbeeld is moeilijker te ontdekken door anderen.

    Controleer deze [koppeling](https://tomasz.janczuk.org/2011/11/debug-nodejs-applications-on-windows.html) voor meer informatie over het opsporen van fouten.

## <a name="scenarios-and-recommendationstroubleshooting"></a>Scenario's en aanbevelingen/het oplossen

### <a name="my-node-application-is-making-too-many-outbound-calls"></a>Mijn toepassing knooppunt is het te veel uitgaande aanroepen.

Veel toepassingen wilt uitgaande verbindingen maken als onderdeel van hun normale werking. Als een aanvraag binnenkomt, wordt door uw app knooppunt wilt contact opnemen met een REST API elders en bepaalde informatie aan de aanvraag niet verwerken. U wilt een keep alive-agent gebruikt het HTTP- of https-aanroepen. Bijvoorbeeld: u kunt de module agentkeepalive als de keep alive-agent bij deze uitgaande gesprekken. Dit zorgt ervoor dat de sockets op uw webtoepassing voor azure VM worden opnieuw gebruikt, waardoor de overhead van het nieuwe sockets voor elke uitgaande verzoek maken. Dit zorgt ook ervoor dat u minder aantal sockets uitgaande aanvragen verzenden en daarom u de maxSockets die worden toegewezen per VM niet overschrijdt. Aanbeveling inzake Azure Webapps zou zijn om een agentKeepAlive maxSockets waarde aan een totaal van 160 sockets per VM. Dit betekent dat als u 4 node.exe uitvoeren op de VM hebt, u wilt de maxSockets agentKeepAlive ingesteld op 40 per node.exe is 160 totaal per VM.

Voorbeeld van de agentKeepALive configuratie:

```
var keepaliveAgent = new Agent({    
    maxSockets: 40,    
    maxFreeSockets: 10,    
    timeout: 60000,    
    keepAliveTimeout: 300000    
});
```

In dit voorbeeld wordt ervan uitgegaan dat u 4 node.exe uitvoeren op uw VM hebt. Als u een ander aantal node.exe uitgevoerd op de VM hebt, moet u wijzigen de maxSockets overeenkomstig wordt ingesteld.

### <a name="my-node-application-is-consuming-too-much-cpu"></a>Mijn toepassing knooppunt verbruikt te veel CPU.

Waarschijnlijk krijgt u een aanbeveling van Azure Webapps op uw portal over hoog cpu-verbruik. U kunt ook monitoren om bepaalde [parameters](web-sites-monitor.md)instellen. Wanneer het CPU-gebruik op het [Dashboard van Azure Portal](../application-insights/app-insights-web-monitor-performance.md)te controleren, Controleer of de waarden MAX CPU zodat u niet de piek waarden vergeten.
In gevallen waarin u denkt dat uw toepassing te veel CPU verbruikt en u kunt geen reden, moet u het profiel van de toepassing van het knooppunt.

### 

#### <a name="profiling-your-node-application-on-azure-webapps-with-v8-profiler"></a>Profileren van uw toepassing knooppunt op azure webapps met V8-Profiler

Zo kunt zeggen u hebben een hello world-app die het gewenste profiel als volgt:

```
var http = require('http');    
function WriteConsoleLog() {    
    for(var i=0;i<99999;++i) {    
        console.log('hello world');    
    }    
}

function HandleRequest() {    
    WriteConsoleLog();    
}

http.createServer(function (req, res) {    
    res.writeHead(200, {'Content-Type': 'text/html'});    
    HandleRequest();    
    res.end('Hello world!');    
}).listen(process.env.PORT);
```

Ga naar uw site scm https://yoursite.scm.azurewebsites.net/DebugConsole

Ziet u een opdrachtregel zoals hieronder wordt weergegeven. Ga naar de map van uw site/wwwroot

![](./media/app-service-web-nodejs-best-practices-and-troubleshoot-guide/scm_install_v8.png)

Voer de opdracht 'v8 profiler npm installeren'

Dit moet v8-profiler onder knooppunt installeren\_modules map en alle bijbehorende afhankelijkheden.
Nu uw server.js om uw toepassing profiel bewerken.

```
var http = require('http');    
var profiler = require('v8-profiler');    
var fs = require('fs');

function WriteConsoleLog() {    
    for(var i=0;i<99999;++i) {    
        console.log('hello world');    
    }    
}

function HandleRequest() {    
    profiler.startProfiling('HandleRequest');    
    WriteConsoleLog();    
    fs.writeFileSync('profile.cpuprofile', JSON.stringify(profiler.stopProfiling('HandleRequest')));    
}

http.createServer(function (req, res) {    
    res.writeHead(200, {'Content-Type': 'text/html'});    
    HandleRequest();    
    res.end('Hello world!');    
}).listen(process.env.PORT);
```

De bovenstaande wijzigingen wordt profiel van de functie WriteConsoleLog en vervolgens de output profiel schrijven naar bestand 'profile.cpuprofile' onder de wwwroot voor uw site. Een aanvraag verzenden naar uw toepassing. Een 'profile.cpuprofile'-bestand gemaakt onder de map wwwroot uw site wordt weergegeven.

![](./media/app-service-web-nodejs-best-practices-and-troubleshoot-guide/scm_profile.cpuprofile.png)

Download dit bestand en moet u dit bestand openen met chroom F12 Tools. Druk op F12 op chroom en klik vervolgens op het tabblad' profielen'. Klik op de knop 'Load'. Selecteer het bestand profile.cpuprofile dat u zojuist hebt gedownload. Klik op het profiel dat u zojuist hebt geladen.

![](./media/app-service-web-nodejs-best-practices-and-troubleshoot-guide/chrome_tools_view.png)

U ziet dat 95% van de tijd door de functie WriteConsoleLog is verbruikt, zoals hieronder wordt weergegeven. Dit ziet u ook de exacte cijfers en de bronbestanden die het probleem veroorzaken.

### <a name="my-node-application-is-consuming-too-much-memory"></a>Mijn toepassing knooppunt is er te veel geheugen.

Waarschijnlijk krijgt u een aanbeveling van Azure Webapps op uw portal over hoge geheugenverbruik. U kunt ook monitoren om bepaalde [parameters](web-sites-monitor.md)instellen. Bij het controleren van het geheugengebruik op de [Portal-Dashboard Azure](../application-insights/app-insights-web-monitor-performance.md), Controleer of de maximale waarden voor geheugen zodat u niet de piek waarden vergeten.

#### <a name="leak-detection-and-heap-diffing-for-nodejs"></a>Lekdetectie en Heap vergelijken voor node.js 

[Knooppunt memwatch](https://github.com/lloyd/node-memwatch) kan u helpen u geheugenlekkages.
U kunt memwatch net als v8 profiler installeren en bewerken van de code voor het vastleggen en diff heaps ter identificatie van het geheugen lekt in uw toepassing.

### <a name="my-nodeexes-are-getting-killed-randomly"></a>Van mijn node.exe worden willekeurig krijgen gedood. 

Er zijn een paar redenen waarom dit kan gebeuren:

1.  Uw toepassing is dat niet-afgevangen uitzonderingen – Neem selectievakje d:\\home\\logboekbestanden\\toepassing\\registratie errors.txt-bestand voor de details op de uitzondering is opgetreden. Dit bestand heeft de stacktrace, zodat u uw toepassing op basis van dit kunt verhelpen.

2.  De toepassing is er te veel geheugen die gevolgen voor andere processen heeft uit aan de slag. Als de totale hoeveelheid geheugen voor VM bijna 100% is, kan van de node.exe worden gedood door de manager van het proces dat andere processen krijgen een kans om te werken. U kunt dit oplossen, zorg ervoor dat uw toepassing niet lekt geheugen of als u toepassingen echt veel geheugen gebruikt moet, moet worden opgewaardeerd naar een grotere VM met veel meer RAM-geheugen.

### <a name="my-node-application-does-not-start"></a>Mijn toepassing knooppunt start niet

Als uw toepassing als 500 fouten bij het opstarten resultaat, kan er een paar redenen:

1.  Node.exe is niet aanwezig op de juiste locatie. NodeProcessCommandLine-instelling controleren.

2.  Belangrijkste script-bestand is niet aanwezig op de juiste locatie. Web.config te controleren en controleer of de naam van het primaire scriptbestand in de sectie-handlers komt overeen met het primaire scriptbestand.

3.  Web.config-configuratie is niet correct: Controleer de instellingen namen/waarden.

4.  Koude Start – uw aanvraag te lang duurt om te starten. Als uw toepassing langer dan duurt (maxNamedPipeConnectionRetry \* namedPipeConnectionRetryDelay) / 1000 seconden iisnode retourneert fout 500. Verhoog de waarden van deze instellingen overeenkomen met de starttijd van toepassingen om te voorkomen dat iisnode van een time-out en de 500-fout.

### <a name="my-node-application-crashed"></a>Mijn knooppunt toepassing is vastgelopen

Uw toepassing is dat niet-afgevangen uitzonderingen – Neem selectievakje d:\\home\\logboekbestanden\\toepassing\\registratie errors.txt-bestand voor de details op de uitzondering is opgetreden. Dit bestand heeft de stacktrace, zodat u uw toepassing op basis van dit kunt verhelpen.

### <a name="my-node-application-takes-too-much-time-to-startup-cold-start"></a>Mijn toepassing knooppunt duurt te lang om te starten (koude Start)

Meest voorkomende reden hiervoor is dat de toepassing een groot aantal bestanden in het knooppunt heeft\_modules en de toepassing wordt geprobeerd de meeste van deze bestanden laden tijdens het opstarten. Standaard, omdat de bestanden zich op de netwerkshare op Azure-Webapps bevinden kan zoveel bestanden laden enige tijd duren.
Er zijn enkele oplossingen voor deze sneller maken:

1.  Controleer of er een platte afhankelijkheidsstructuur en geen dubbele afhankelijkheden met behulp van npm3 modules te installeren.

2.  Probeer te langzaam laden in uw knooppunt\_modules en niet alle modules tijdens het opstarten worden geladen. Dit betekent dat de aanroep van require('module') worden uitgevoerd wanneer u deze daadwerkelijk in de functie die u probeert te gebruiken van de module nodig.

3.  Azure Webapps biedt een functie met de naam van de lokale cache. Deze functie kopieert de inhoud van de netwerkshare naar de lokale schijf op de VM. Nadat de bestanden lokale, de laadtijd van knooppunt zijn\_modules gaat veel sneller. -Deze [documentatie](../app-service/app-service-local-cache.md) wordt beschreven hoe u van lokale Cache in meer detail.

## <a name="iisnode-http-status-and-substatus"></a>IISNODE HTTP-status en substatus

Dit [bestand](https://github.com/Azure/iisnode/blob/master/src/iisnode/cnodeconstants.h) bevat alle de iisnode mogelijke status/substatus combinatie in geval van een fout retourneren kan.

FREB voor uw toepassing voor een overzicht van de win32-foutcode inschakelen (Controleer of FREB alleen op niet-productieve sites voor betere prestaties is ingeschakeld).

| HTTP-Status | HTTP-SubStatus | Mogelijke reden?                                                                                                                                                                                            
|-------------|----------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------
| 500         | 1000           | Er is sommige probleem verzenden van de aanvraag voor IISNODE – controleren als node.exe is gestart. Node.exe kan zijn vastgelopen bij het opstarten. Controleer uw web.config-configuratie op fouten.                                                                                                                                                                                                                                                                                     |
| 500         | 1001           | -Win32Error 0x2 - App reageert niet op de URL. Controleer de URL herschrijven regels of als uw app express de juiste routes gedefinieerd heeft. -Win32Error 0x6d – named pipes is bezet – Node.exe accepteert geen aanvragen omdat de pipe bezet is. Hoog cpu-gebruik controleren. -Andere fouten – controleren als node.exe is vastgelopen.
| 500         | 1002           | Vastgelopen node.exe – Controleer d:\\home\\logboekbestanden\\logboekregistratie-errors.txt voor de stack-tracering.                                                                                                                                                                                                                                                                                                                                                                                        |
| 500         | 1003           | Pipe configuratie probleem – moet u nooit zien, maar als u dat doet, de named pipe-configuratie is onjuist.                                                                                                                                                                                                                                                                                                                                                          |
| 500         | 1004-1018      | Er is een fout tijdens het verzenden van de aanvraag of het antwoord van node.exe verwerken. Controleer of node.exe is vastgelopen. d: controleren\\home\\logboekbestanden\\logboekregistratie-errors.txt voor de stack-tracering.                                                                                                                                                                                                                                                                                    |
| 503         | 1000           | Onvoldoende geheugen toewijzen meer named pipe-verbindingen. Selectievakje waarom uw app zo veel geheugen verbruikt. Controleer de waarde van de maxConcurrentRequestsPerProcess. Als het is niet oneindig en u hebt een groot aantal aanvragen, hier een hogere waarde om te voorkomen dat deze fout.                                                                                                                                                                                                                                                                                                                  |
| 503         | 1001           | Aanvraag kan niet worden verzonden naar node.exe omdat de toepassing is recycling. Nadat de toepassing is herhaald, moeten normaliter aanvragen worden aangeboden.                                                                                                                                                                                                                                                                                                               |
| 503         | 1002           | Selectievakje win32-foutcode voor de werkelijke reden – aanvraag kan niet worden verzonden naar een node.exe.                                                                                                                                                                                                                                                                                                                                                                               |
| 503         | 1003           | Named pipes is bezet – controleren als knooppunt veel CPU verbruikt                                                                                                                                                                                                                                                                                                                                                                                                        
                                                                                                                                                                                                                                                                                                            
                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         
Er is een instelling in het knooppunt genoemd NODE.exe\_in behandeling\_PIPE\_exemplaren. Deze waarde is standaard buiten azure webapps 4. Dit betekent dat node.exe accepteert alleen 4 aanvragen tegelijk op de named pipe. Deze waarde is ingesteld op 5000 op Azure Webapps, en deze waarde moet goed genoeg voor de meeste knooppunt toepassingen op azure webapps. Zie geen 503.1003 op azure webapps omdat we een hoge waarde voor het knooppunt hebben\_in behandeling\_PIPE\_exemplaren.  |

## <a name="more-resources"></a>Meer bronnen

Klik op deze koppelingen voor meer informatie over node.js toepassingen op Azure App-Service.

* [Aan de slag met Node.js web apps in Azure App-Service](app-service-web-nodejs-get-started.md)
* [Hoe kunt u fouten opsporen in een Node.js in Azure App Service web app](web-sites-nodejs-debug.md)
* [Met behulp van Node.js Modules met Azure toepassingen](../nodejs-use-node-modules-azure-apps.md)
* [Azure App Service Web Apps: Node.js](https://blogs.msdn.microsoft.com/silverlining/2012/06/14/windows-azure-websites-node-js/)
* [Node.js Developer Center](../nodejs-use-node-modules-azure-apps.md)
* [De Super geheime Kudu Foutopsporingsconsole verkennen](https://azure.microsoft.com/documentation/videos/super-secret-kudu-debug-console-for-azure-web-sites/)