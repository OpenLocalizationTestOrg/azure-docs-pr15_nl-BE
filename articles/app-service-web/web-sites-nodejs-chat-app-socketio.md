<properties
    pageTitle="Maak een chatprogramma Node.js met Socket.IO in Azure App-Service"
    description="Een zelfstudie waarin wordt gedemonstreerd met socket.io in een node.js web app gehost op Azure."
    services="app-service\web"
    documentationCenter="nodejs"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

# <a name="create-a-nodejs-chat-application-with-socketio-in-azure-app-service"></a>Maak een chatprogramma Node.js met Socket.IO in Azure App-Service

Socket.IO biedt real-time communicatie tussen uw node.js server en clients met behulp van WebSockets. Ondersteunt ook terugval naar andere transporten (zoals long polling) die met oudere browsers werken. Deze zelfstudie kunt u doorlopen een chatprogramma gebaseerd Socket.IO als een app Azure web hosting en laten u zien hoe u de schaal van de toepassing die gebruikmaakt van [Azure bestand Vgx. Cache]. Zie <http://socket.io/>voor meer informatie op Socket.IO.

> [AZURE.NOTE] De procedures in deze taak van toepassing op het [Web Apps van App Service]; Zie [een toepassing Node.js Chat met Socket.IO op een Azure Cloud-Service maakt]voor Cloud-Services.

## <a name="download-the-chat-example"></a>Voorbeeld van de chat downloaden

Voor dit project gebruiken we het voorbeeld chat uit de [opslagplaats Socket.IO GitHub]. Voer de volgende stappen uit om het voorbeeld te downloaden en toe te voegen aan het project dat u eerder hebt gemaakt.

1.  Download een [ZIP- of GZ gearchiveerde versie] van het project Socket.IO (versie 1.3.5 voor dit document is gebruikt)

1.  Pak het archief en de kopie de **voorbeelden\\chat** map naar een nieuwe locatie. Bijvoorbeeld: ** \\knooppunt\\chat**.

## <a name="modify-appjs-and-install-modules"></a>App.js wijzigen en modules installeren

1.  Naam van het bestand **index.js** **app.js**. Hierdoor Azure te detecteren dat dit een Node.js is.

1.  Open het bestand **app.js** in een teksteditor. Wijzig de regel die `var io = require('../..')(server);` zoals hieronder wordt weergegeven:

        var express = require('express');
        var app = express();
        var server = require('http').createServer(app);
        // var io = require('../..')(server);
        // New:
        var io = require('socket.io')(server);
        var port = process.env.PORT || 3000;

1. Open het bestand **package.json** en voeg een verwijzing naar de socket.io onder `dependencies`, zoals hieronder wordt weergegeven:

        "dependencies": {
          "express": "3.4.8",
          "socket.io": "1.3.5"
        }

1. Vanaf de opdrachtregel wijzigen in de ** \\knooppunt\\chat** directory en het gebruik van npm modules die nodig zijn door deze toepassing te installeren:

        npm install

    Hiermee installeert u de modules in een submap met de naam **node_modules**.

## <a name="create-an-azure-web-app"></a>Maak een Azure Web App

Volg deze stappen om een Azure web app maken, Git publiceren inschakelen en vervolgens WebSocket ondersteuning inschakelen voor de web app.

> [AZURE.NOTE] Als u deze zelfstudie hebt voltooid, moet u een account Azure. Als u geen account hebt, kunt u een gratis proefperiode account in een paar minuten. Zie voor meer informatie, <a href="http://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A7171371E" target="_blank">Gratis proefperiode van Azure</a>.

1. Installeer de Azure opdrachtregelinterface (CLI Azure) en verbinding maken met uw abonnement Azure. [Installeer en configureer de Azure CLI](../xplat-cli-install.md)Zie.

1. Als dit de eerste keer instellen van een opslagplaats in Azure, moet u de inloggegevens maken. Voer de volgende opdracht uit de CLI Azure:

        azure site deployment user set [username] [password]

1. Wijzigen in de ** \\node\chat** map en gebruik de volgende opdracht maakt u een nieuwe Azure web app en een lokaal Git repository. Met deze opdracht maakt ook een Git remote benoemde 'azure'.

        azure site create mysitename --git

    U moet een unieke naam voor uw web app 'mysitename' vervangen.

1. Bestaande bestanden in de lokale bibliotheek vastleggen met behulp van de volgende opdrachten:

        git add .
        git commit -m "Initial commit"

1. Push-bestanden naar de opslagplaats Azure Web Apps met de volgende opdracht:

        git push azure master

    Voer desgevraagd de referenties uit stap 2. U ontvangt statusberichten als modules worden geïmporteerd op de server. Nadat dit proces is voltooid, wordt de toepassing worden gehost op uw Azure web app.

    > [AZURE.NOTE] Tijdens de installatie van de module is het mogelijk fouten die ' het geïmporteerde project... is niet gevonden '. Deze kunnen veilig worden genegeerd.

1. Socket.IO maakt gebruik van WebSockets, die zijn niet standaard ingeschakeld op Azure. Om te schakelen op sockets web, gebruik de volgende opdracht:

        azure site set -w

    Typ desgevraagd de naam van de web app.

    >[AZURE.NOTE]
    >De 'azure site set -w-opdracht zal werken alleen met versie 0.7.4 of hoger van de opdrachtregelinterface van Azure. U kunt ook WebSocket ondersteuning via de [Portal Azure](https://portal.azure.com)inschakelen.
    >
    >Om te schakelen met behulp van de Portal Azure WebSockets, klikt u op het web app vanaf het Web Apps blade, klikt u op **alle instellingen** > **Toepassingsinstellingen**. Klik onder **Web Sockets**, **op**. Klik vervolgens op **Opslaan**.

1. Als u wilt bekijken van de web app op Azure, kunt de volgende opdracht start uw webbrowser en Ga naar de gehoste web app:

        azure site browse

De app is nu actief op Azure en chat-berichten tussen de verschillende clients met behulp van Socket.IO kan doorsturen.

## <a name="scale-out"></a>Schaalbare

Socket.IO-toepassingen kunnen worden geschaald uit met behulp van een __adapter__ om berichten en evenementen tussen meerdere instanties van de toepassing te distribueren. Er zijn verschillende adapters, kan de [socket.io-bestand Vgx.] adapter gemakkelijk worden gebruikt met de functie bestand Vgx. Azure-Cache.

> [AZURE.NOTE] Een aanvullende eis voor geschaalde uitbreiding van een Socket.IO oplossing is ondersteuning voor sticky sessies. Sticky-sessies worden standaard ingeschakeld voor Azure Web Apps via Azure routering aanvragen. Zie [Exemplaar affiniteit in Azure websites]voor meer informatie.

### <a name="create-a-redis-cache"></a>Maak een cache bestand Vgx.

Voer de stappen in [een cache in Azure bestand Vgx. Cache maken] voor het maken van een nieuwe cache.

> [AZURE.NOTE] Sla de __hostnaam__ en de __primaire sleutel__ voor de cache, zoals deze in de volgende stappen nodig.

### <a name="add-the-redis-and-socketio-redis-modules"></a>Voeg het bestand Vgx. en modules socket.io-bestand Vgx.

1. Vanaf een opdrachtregel wijzigen in de __ \\knooppunt\\chat__ map en gebruik de volgende opdracht.

        npm install socket.io-redis@0.1.4 redis@0.12.1 --save

    > [AZURE.NOTE] De versie die is opgegeven in deze opdracht worden de versies die worden gebruikt bij het testen van dit artikel.

1. Wijzig het bestand __app.js__ om toe te voegen de volgende regels direct na`var io = require('socket.io')(server);`

        var pub = require('redis').createClient(6379,'redishostname', {auth_pass: 'rediskey', return_buffers: true});
        var sub = require('redis').createClient(6379,'redishostname', {auth_pass: 'rediskey', return_buffers: true});

        var redis = require('socket.io-redis');
        io.adapter(redis({pubClient: pub, subClient: sub}));

    __Redishostname__ en __rediskey__ vervangen door de hostnaam en de sleutel voor uw bestand Vgx. cache.

    Dit maakt van publiceren en abonneren client naar de cache bestand Vgx. eerder hebt gemaakt. De clients worden gebruikt met de adapter voor het configureren van Socket.IO voor het gebruik van de cache voor het bestand Vgx. voor het doorgeven van berichten, gebeurtenissen en andere instanties van de toepassing

    > [AZURE.NOTE] Terwijl de __socket.io-bestand Vgx.__ adapter rechtstreeks naar het bestand Vgx. communiceren kan, is de huidige versie ondersteunt niet de verificatie die wordt vereist door de cache Azure bestand Vgx.. Dus de eerste verbinding wordt gemaakt met behulp van de module __bestand Vgx.__ vervolgens de client wordt doorgegeven aan de __socket.io-bestand Vgx.__ adapter.
    >
    > Terwijl Azure bestand Vgx. Cache beveiligde verbindingen via poort 6380 ondersteunt, ondersteund beveiligde verbindingen op 14-7/2014 niet door de modules die in dit voorbeeld wordt gebruikt. De bovenstaande code maakt gebruik van de standaard, niet-beveiligde poort van 6379.

1. Sla de gewijzigde __app.js__

### <a name="commit-changes-and-redeploy"></a>Wijzigingen vastleggen en opnieuw implementeren

Vanaf de opdrachtregel in de __ \\knooppunt\\chat__ directory, gebruikt u de volgende opdrachten wijzigingen vastleggen en implementeren van de toepassing.

    git add .
    git commit -m "implementing scale out"
    git push azure master

Zodra de wijzigingen hebben op de server zijn geplaatst, kunt u uw site via meerdere exemplaren met de volgende opdracht schalen.

    azure site scale instances --instances #

Waarbij __#__ is het aantal exemplaren te maken.

Kunt u uw web app vanaf meerdere computers om te controleren of berichten worden correct verzonden aan alle clients of browsers.

## <a name="troubleshooting"></a>Het oplossen van problemen

### <a name="connection-limits"></a>Verbindingslimieten

Azure Web Apps is beschikbaar in meerdere SKU's, de middelen die beschikbaar zijn voor uw site te bepalen. Dit omvat het aantal toegestane verbindingen met WebSocket. Voor meer informatie, Zie de [pagina met prijzen voor Web Apps].

### <a name="messages-arent-being-sent-using-websockets"></a>Berichten worden niet verzonden met behulp van WebSockets

Als clientbrowsers houden teruggevallen op lange verbindingsintegriteit in plaats van WebSockets dit mogelijk een van de volgende.

* **Probeer het beperken van het vervoer naar de zojuist WebSockets**

    Socket.IO WebSockets als het transport van berichten gebruiken om moeten de server en de client ondersteuning voor WebSockets. Als het ene of het andere niet, moet Socket.IO een ander transport, zoals lange polling onderhandelen. De standaardlijst van transporten die wordt gebruikt door Socket.IO ` websocket, htmlfile, xhr-polling, jsonp-polling`. U kunt zorgen dat alleen WebSockets worden gebruikt door de volgende code in het bestand **app.js** toe te voegen na de regel die `, nicknames = {};`.

        io.configure(function() {
          io.set('transports', ['websocket']);
        });

    > [AZURE.NOTE] Houd er rekening mee dat oudere browsers die geen ondersteuning voor WebSockets bieden niet zal kunnen verbinding maken met de site terwijl de bovenstaande code actief is, zoals het communicatie WebSockets slechts beperkt.

* **SSL gebruiken**

    WebSockets is afhankelijk van enkele mindere gebruikt HTTP-headers, zoals de koptekst van de **Upgrade** . Sommige tussenliggende netwerkapparaten, zoals web-proxy's verwijderen deze headers. Als u wilt voorkomen dat dit probleem, kunt u de WebSocket verbinding maken via SSL.

    Een eenvoudige manier om dit te bereiken is het configureren van Socket.IO naar `match origin protocol`. Dit geeft de opdracht Socket.IO WebSockets communicatie beveiligen hetzelfde als de oorspronkelijke HTTP/HTTPS-aanvraag voor de webpagina. Als een browser een HTTPS-URL gebruikt voor uw website bezoeken, worden daaropvolgende WebSocket communicatie via Socket.IO beveiligd via SSL.

    Als u wilt wijzigen in het volgende voorbeeld als deze configuratie wilt inschakelen, de volgende code toevoegen aan het bestand **app.js** na de lijn die `, nicknames = {};`.

        io.configure(function() {
          io.set('match origin protocol', true);
        });

* **Controleer de instellingen van web.config**

    Het bestand **web.config** Azure web apps met Node.js toepassingen gebruiken voor het routeren van binnenkomende aanvragen voor de toepassing van Node.js. De WebSockets werkt goed met Node.js toepassingen bevatten **web.config** de volgende vermelding.

        <webSocket enabled="false"/>

    Hierdoor wordt de module IIS-WebSockets, met een eigen implementatie van WebSockets en conflicten met specifieke WebSocket modules zoals Socket.IO Node.js uitgeschakeld. Als deze regel niet aanwezig is of is ingesteld op `true`, dit is de reden waarom het transport WebSocket niet voor uw toepassing werkt mogelijk.

    Normaal gesproken bevatten Node.js toepassingen geen een **web.config** -bestand, zodat de Azure Websites automatisch gegenereerd voor Node.js toepassingen wanneer ze worden geïmplementeerd. Aangezien dit bestand wordt automatisch gegenereerd op de server, moet u de FTP- of FTPS-URL voor uw website om dit bestand te bekijken. U vindt de FTP- en TRANSFEREERT URL's voor uw site in de portal voor klassieke door uw web app en de **Dashboard** -koppeling te selecteren. De URL's worden weergegeven in de sectie **Overzicht** .

    > [AZURE.NOTE] Het bestand **web.config** wordt alleen gegenereerd door Azure Websites als uw toepassing beschikt niet over. Als u een **web.config** -bestand in de hoofdmap van het toepassingsproject, wordt deze gebruikt door Azure Web Apps.

    Als de post niet aanwezig is of is ingesteld op een waarde van `true`, en vervolgens moet u een **web.config** in de hoofdmap van de toepassing Node.js maken en geef een waarde van `false`.  Voor een verwijzing naar het hieronder vindt u een standaard- **web.config** voor een toepassing die gebruikmaakt van **app.js** als het toegangspunt.

        <?xml version="1.0" encoding="utf-8"?>
        <!--
             This configuration file is required if iisnode is used to run node processes behind
             IIS or IIS Express.  For more information, visit:

             https://github.com/tjanczuk/iisnode/blob/master/src/samples/configuration/web.config
        -->

        <configuration>
          <system.webServer>
            <!-- Visit http://blogs.msdn.com/b/windowsazure/archive/2013/11/14/introduction-to-websockets-on-windows-azure-web-sites.aspx for more information on WebSocket support -->
            <webSocket enabled="false" />
            <handlers>
              <!-- Indicates that the server.js file is a node.js web app to be handled by the iisnode module -->
              <add name="iisnode" path="app.js" verb="*" modules="iisnode"/>
            </handlers>
            <rewrite>
              <rules>
                <!-- Do not interfere with requests for node-inspector debugging -->
                <rule name="NodeInspector" patternSyntax="ECMAScript" stopProcessing="true">
                  <match url="^app.js\/debug[\/]?" />
                </rule>

                <!-- First we consider whether the incoming URL matches a physical file in the /public folder -->
                <rule name="StaticContent">
                  <action type="Rewrite" url="public{REQUEST_URI}"/>
                </rule>

                <!-- All other URLs are mapped to the node.js web app entry point -->
                <rule name="DynamicContent">
                  <conditions>
                    <add input="{REQUEST_FILENAME}" matchType="IsFile" negate="True"/>
                  </conditions>
                  <action type="Rewrite" url="app.js"/>
                </rule>
              </rules>
            </rewrite>
            <!--
              You can control how Node is hosted within IIS using the following options:
                * watchedFiles: semi-colon separated list of files that will be watched for changes to restart the server
                * node_env: will be propagated to node as NODE_ENV environment variable
                * debuggingEnabled - controls whether the built-in debugger is enabled

              See https://github.com/tjanczuk/iisnode/blob/master/src/samples/configuration/web.config for a full list of options
            -->
            <!--<iisnode watchedFiles="web.config;*.js"/>-->
          </system.webServer>
        </configuration>

    Als uw toepassing een ingangspunt dan **app.js**gebruikt, moet u alle exemplaren van de **app.js** vervangen door het juiste toegangspunt. Bijvoorbeeld vervangen **app.js** door **server.js**.

>[AZURE.NOTE] Als u wilt aan de slag met Azure App-Service voordat u aanmelden voor een account met Azure, gaat u naar de [App-Service probeert], waar u direct een starter tijdelijk web app in de App-Service maken kunt. Geen creditcard vereist; geen verplichtingen.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe een chatprogramma dat wordt gehost in een Azure web app maken. U kunt ook deze toepassing als Azure Cloud Service host. Zie [een toepassing Node.js Chat met Socket.IO op een Azure Cloud-Service maakt]voor stapsgewijze instructies over hoe om dit te bereiken.

Zie ook de [Node.js Developer Center]voor meer informatie.

## <a name="whats-changed"></a>Wat er veranderd

* Zie voor een handleiding voor het wijzigen van Websites met App-Service: [Azure App Service en de Impact op de bestaande Azure Services].

<!-- URL List -->

[Cache bestand Vgx Azure.]: /documentation/services/redis-cache/
[App Service Web Apps]: http://go.microsoft.com/fwlink/?LinkId=529714
[Prijzen voor Apps webpagina]: http://go.microsoft.com/fwlink/?LinkId=511643
[Bouwen van een toepassing Node.js Chat met Socket.IO op een Azure Cloud-Service]: ../cloud-services/cloud-services-nodejs-chat-app-socketio.md
[Install and Configure the Azure CLI]: ../xplat-cli-install.md
[Azure App Service en de gevolgen voor bestaande Azure Services]: http://go.microsoft.com/fwlink/?LinkId=529714
[Node.js Developer Center]: /develop/nodejs/
[Probeer de Service App]: http://go.microsoft.com/fwlink/?LinkId=523751
[Exemplaar-affiniteit in Azure websites]: https://azure.microsoft.com/blog/2013/11/18/disabling-arrs-instance-affinity-in-windows-azure-web-sites/
[Maak een cache in Azure-Cache bestand Vgx.]: ../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md

[socket.IO-bestand Vgx.]: https://github.com/socketio/socket.io-redis
[Socket.IO GitHub opslagplaats]: https://github.com/socketio/socket.io
[ZIP- of GZ gearchiveerde versie]: https://github.com/socketio/socket.io/releases

<!-- IMG List -->

[chat-example-view]: ./media/web-sites-nodejs-chat-app-socketio/socketio-2.png
[npm-output]: ./media/web-sites-nodejs-chat-app-socketio/socketio-7.png
[completed-app]: ./media/web-sites-nodejs-chat-app-socketio/websitesocketcomplete.png
