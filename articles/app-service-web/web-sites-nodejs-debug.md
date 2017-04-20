<properties
    pageTitle="Hoe kunt u fouten opsporen in een Node.js in Azure App Service web app"
    description="Informatie over het opsporen van een Node.js web app in Azure App-Service."
    tags="azure-portal"
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

# <a name="how-to-debug-a-nodejs-web-app-in-azure-app-service"></a>Hoe kunt u fouten opsporen in een Node.js in Azure App Service web app

Azure biedt ingebouwde diagnostische gegevens om te helpen bij foutopsporing Node.js toepassingen die worden gehost in [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) Web Apps. In dit artikel leert u hoe u schakelt het vastleggen van stdout en stderr, foutgegevens weergeven in de browser en hoe u de logboekbestanden weergeven.

Diagnostische gegevens voor Node.js toepassingen die worden gehost op Azure wordt geleverd door [IISNode]. Hoewel in dit artikel wordt de meest gebruikte instellingen voor het verzamelen van diagnostische gegevens, biedt geen volledige informatie over voor het werken met IISNode. Zie het [Leesmij-bestand IISNode] op GitHub voor meer informatie over het werken met IISNode.

<a id="enablelogging"></a>
## <a name="enable-logging"></a>Logboekregistratie inschakelen

Standaard wordt een web App Service app alleen diagnostische gegevens over implementaties, zoals bij het implementeren van een web app met Git vastgelegd. Deze informatie is handig als u problemen ondervindt tijdens de implementatie, zoals een storing bij het installeren van een module waarnaar wordt verwezen in de **package.json**, of als u een script voor aangepaste implementatie.

Zodat de registratie van stdout en stderr streams moet u een **IISNode.yml** -bestand maken in de hoofdmap van de toepassing Node.js en voeg het volgende toe:

    loggingEnabled: true

Hiermee kunt de registratie van stderr en stdout vanuit uw toepassing Node.js.

Het bestand **IISNode.yml** kan ook worden gebruikt om te bepalen of beschrijvende fouten of ontwikkelaar fouten worden naar de browser geretourneerd wanneer er een fout optreedt. Voeg de volgende regel aan het bestand **IISNode.yml** zodat ontwikkelaars fouten:

    devErrorsEnabled: true

Als deze optie is ingeschakeld, wordt IISNode de laatste 64 kB gegevens verzonden naar stderr in plaats van een aangepaste fout, zoals 'Er is een interne serverfout opgetreden' geretourneerd.

> [AZURE.NOTE] DevErrorsEnabled is handig bij het oplossen van problemen tijdens de ontwikkeling, waardoor deze in een productieomgeving kan leiden tot ontwikkeling fouten die worden verzonden naar gebruikers.

Als het bestand **IISNode.yml** binnen de toepassing nog niet bestaat, moet u uw web app na publicatie van de bijgewerkte toepassing opnieuw. Als u instellingen in een bestaand **IISNode.yml** -bestand dat eerder is gepubliceerd te wijzigen, is geen opnieuw opstarten vereist.

> [AZURE.NOTE] Als uw web app is gemaakt met de Azure PowerShell-Cmdlets of Azure opdrachtregelprogramma's, wordt er automatisch een standaard **IISNode.yml** -bestand gemaakt.

Om opnieuw te starten in de web app, selecteert u de web app in de [Portal Azure](https://portal.azure.com)en vervolgens klikt u op de knop **opnieuw opstarten** :

![knop opnieuw starten][restart-button]

Als de Azure opdrachtregelprogramma's zijn geïnstalleerd in uw ontwikkelomgeving, kunt u de volgende opdracht op te starten van de web app:

    azure site restart [sitename]

> [AZURE.NOTE] LoggingEnabled en devErrorsEnabled zijn de meest gebruikte IISNode.yml configuratie-opties voor het vastleggen van diagnostische gegevens, kan IISNode.yml worden gebruikt voor het configureren van een aantal opties voor de hosting-omgeving. Zie het bestand [iisnode_schema.xml](https://github.com/tjanczuk/iisnode/blob/master/src/config/iisnode_schema.xml) voor een volledige lijst van de configuratieopties.

<a id="viewlogs"></a>
## <a name="accessing-logs"></a>Toegang tot logboeken

Diagnoselogboeken toegankelijk is op drie manieren; Met behulp van FTP File Transfer Protocol (), een Zip-archief downloaden of bijgewerkt als een live stream van het logboek (staart). Moet de Azure opdrachtregelprogramma's downloaden van het Zip-archief van de logboekbestanden of bekijken van de live gegevensstroom. Deze kunnen worden geïnstalleerd door de volgende opdracht:

    npm install azure-cli -g

Eenmaal geïnstalleerd, zijn de hulpprogramma's toegankelijk met de opdracht 'azure'. Opdrachtregelprogramma's moeten eerst worden geconfigureerd voor gebruik van uw abonnement op Azure. Zie voor meer informatie over het uitvoeren van deze taak, de **downloaden en importeren in publicatie-instellingen** sectie van het artikel [hoe gebruik de Azure opdrachtregelprogramma's](../xplat-cli-connect.md) .

###<a name="ftp"></a>FTP

Diagnostische gegevens via FTP toegang, gaat u naar de [Portal Azure](https://portal.azure.com), uw web app selecteren en selecteer vervolgens het **DASHBOARD**. In de sectie **Snelle koppelingen** bieden de koppelingen **Diagnostische LOGBOEKEN van FTP-** en **DIAGNOSELOGBOEKEN FTPS** toegang tot de logboeken met behulp van het FTP-protocol.

> [AZURE.NOTE] Als u niet eerder geconfigureerd gebruikersnaam en wachtwoord voor FTP- of implementatie, kunt u dat doen vanaf de beheerpagina van de **QuickStart** door **implementatie referenties instellen**.

De FTP-URL geretourneerd in het dashboard is voor **de map LogFiles, waarin de volgende submappen** :

* [Implementatiemethode](web-sites-deploy.md) - als u een distributiemethode zoals Git, een map met dezelfde naam wordt gemaakt en bevat gegevens met betrekking tot de implementatie.

* nodejs - Stdout en stderr informatie uit alle exemplaren van uw toepassing opgenomen (als loggingEnabled true is.)

###<a name="zip-archive"></a>ZIP-archief

Gebruik de volgende opdracht uit de Azure opdrachtregelprogramma's voor het downloaden van een Zip-archief van de diagnostische logboeken:

    azure site log download [sitename]

Dit zal een **diagnostics.zip** in de huidige map downloaden. Dit archief bevat de volgende mappenstructuur:

* implementaties - een logboek van informatie over de distributie van uw toepassing

* Logboekbestanden

    * [Implementatiemethode](web-sites-deploy.md) - als u een distributiemethode zoals Git, een map met dezelfde naam wordt gemaakt en bevat gegevens met betrekking tot de implementatie.

    * nodejs - Stdout en stderr informatie uit alle exemplaren van uw toepassing opgenomen (als loggingEnabled true is.)

###<a name="live-stream-tail"></a>Live stream (staart)

Als u wilt weergeven in een live gegevensstroom van de diagnostische logboekgegevens, gebruik de volgende opdracht bij de Azure opdrachtregelprogramma's:

    azure site log tail [sitename]

Dit resulteert in een reeks gebeurtenissen die worden bijgewerkt als deze op de server plaatsvinden. Deze stroom retourneert informatie over de implementatie alsmede stdout en stderr informatie (als loggingEnabled true is.)

<a id="nextsteps"></a>
## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd hoe inschakelen en diagnostische informatie voor Azure. Deze informatie is handig als u informatie over problemen die zich bij de toepassing voordoen, kan het verwijzen naar een probleem met een module die u gebruikt of de versie van Node.js gebruikt door App Service Web Apps is anders dan in uw werkomgeving.

Zie voor informatie bij het werken met modules voor Azure, [Met behulp van Node.js Modules met Azure-toepassingen](../nodejs-use-node-modules-azure-apps.md).

Zie [een Node.js versie in een toepassing Azure opgeven]voor meer informatie over het opgeven van een Node.js versie voor uw toepassing.

Zie ook de [Node.js Developer Center](/develop/nodejs/)voor meer informatie.

## <a name="whats-changed"></a>Wat er veranderd
* Zie voor een handleiding voor het wijzigen van Websites met App-Service: [Azure App Service en de Impact op de bestaande Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)

>[AZURE.NOTE] Als u wilt aan de slag met Azure App-Service voordat u aanmelden voor een account met Azure, gaat u naar de [App-Service probeert](http://go.microsoft.com/fwlink/?LinkId=523751), waar u direct een starter tijdelijk web app in de App-Service maken kunt. Geen creditcard vereist; geen verplichtingen.

[IISNode]: https://github.com/tjanczuk/iisnode
[Leesmij-bestand IISNode]: https://github.com/tjanczuk/iisnode#readme
[How to Use The Azure Command-Line Interface]: ../xplat-cli-install.md
[Using Node.js Modules with Azure Applications]: ../nodejs-use-node-modules-azure-apps.md
[Een Node.js versie op te geven in een toepassing Azure]: ../nodejs-specify-node-version-azure-apps.md

[restart-button]: ./media/web-sites-nodejs-debug/restartbutton.png
 
