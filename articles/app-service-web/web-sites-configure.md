<properties 
    pageTitle="Web apps in Azure App-Service configureren" 
    description="Het configureren van een web app in Azure App Services" 
    services="app-service\web" 
    documentationCenter="" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

# <a name="configure-web-apps-in-azure-app-service"></a>Web apps in Azure App-Service configureren #

In dit onderwerp wordt uitgelegd hoe u een web app met behulp van de [Portal Azure]configureren.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="application-settings"></a>Instellingen voor toepassing

1. Open het blad voor de web-app in de [Portal Azure].
2. Klik op **alle instellingen**.
3. Klik op **instellingen van de toepassing**.

![Instellingen voor toepassing][configure01]

De **Toepassingsinstellingen** blade heeft gegroepeerd onder verschillende categorieën.

### <a name="general-settings"></a>Algemene instellingen

**Framework-versies**. Deze opties instellen als uw app alle volgende kaders gebruikt: 

- **.NET Framework**: instellen van de .NET framework-versie. 
- **PHP**: PHP versie of **uit** te schakelen, PHP instellen. 
- **Java**: Selecteer de versie van Java of **uit** Java uitschakelen. Gebruik de optie **Web Container** te kiezen tussen versies Tomcat en Jetty.
- **Python**: Selecteer de versie van Python of **uit** te schakelen, Python.

Om technische redenen Java inschakelen voor uw toepassing .NET, PHP en Python worden de opties uitgeschakeld.

<a name="platform"></a>
**Platform**. Hiermee selecteert u of uw web app wordt uitgevoerd in een 32-bits of 64-bits omgeving. De 64-bits omgeving is vereist voor Basic of Standard-modus. Vrij en gedeelde modi worden altijd uitgevoerd in een 32-bits omgeving.

**Web-Sockets**. Stel **op** het WebSocket-protocol inschakelen Als bijvoorbeeld uw web app gebruikt de [ASP.NET-SignalR] of [socket.io].

<a name="alwayson"></a>
**Altijd op**. Standaard zijn web apps uit het geheugen verwijderd als ze gedurende bepaalde tijd niet actief zijn. Hierdoor wordt het systeem te besparen. In de Basic- of Standard-modus kunt u **Altijd aan** om de app geladen alle tijd. Als uw app continu web taken uitvoert, moet u **Altijd op**inschakelen of de taken van het web kunnen niet op betrouwbare wijze worden uitgevoerd.

**Beheerde Pipeline-versie**. Hiermee stelt u de IIS- [pipeline-modus]. Laat deze set geïntegreerde (de standaardinstelling) tenzij u een oude toepassingen waarvoor een oudere versie van IIS.

**Wisselen van auto**. Als Auto ruilen voor een sleuf voor de implementatie, zal App-Service automatisch de web app wisselen in productie wanneer u een update aan die sleuf push. Zie voor meer informatie [distribueren naar de staging-sleuven voor web apps in Azure App Service] (web-sites-klaargezet-publishing.md).

### <a name="debugging"></a>Foutopsporing

**Foutopsporing op afstand**. Hiermee kunt foutopsporing op afstand. Wanneer ingeschakeld, kunt u de externe foutopsporing in Visual Studio rechtstreeks aansluiten op uw web app. Foutopsporing op afstand blijven ingeschakeld gedurende 48 uur. 

### <a name="app-settings"></a>App instellingen

Deze sectie bevat een naam/waarde-paren die u web-app wordt geladen op start. 

- Deze instellingen zijn voor .NET-toepassingen, geïnjecteerd in de configuratie van .NET `AppSettings` tijdens runtime, overschrijft u bestaande instellingen. 

- PHP, Python, Java en knooppunt toepassingen toegang tot deze instellingen van de omgevingsvariabelen tijdens runtime. Voor elke instelling app worden twee omgevingsvariabelen gemaakt; een met de naam die door de post app instellen en een andere met het voorvoegsel APPSETTING_. Beide bevatten dezelfde waarde.

### <a name="connection-strings"></a>Verbindingsreeksen

Verbindingstekenreeksen voor gekoppelde bronnen. 

Voor .NET-toepassingen deze tekenreeksen worden geïnjecteerd in de configuratie van .NET `connectionStrings` instellingen tijdens runtime, bestaande posten waar de sleutel is gelijk aan de naam van de gekoppelde database te overschrijven. 

Deze instellingen zijn beschikbaar als de omgevingsvariabelen tijdens runtime, voorafgegaan door het verbindingstype voor PHP, Python, Java en knooppunt toepassingen. De omgeving variabele voorvoegsels zijn als volgt: 

- SQL Server:`SQLCONNSTR_`
- MySQL:`MYSQLCONNSTR_`
- SQL-Database:`SQLAZURECONNSTR_`
- Aangepast:`CUSTOMCONNSTR_`

Bijvoorbeeld een verbindingsreeks MySql zijn met de naam `connectionstring1`, zou het worden geopend via de omgevingsvariabele `MYSQLCONNSTR_connectionString1`.

### <a name="default-documents"></a>Standaarddocumenten

Het standaarddocument is de webpagina die wordt weergegeven op de basis-URL voor een website.  Het eerste overeenkomende bestand in de lijst wordt gebruikt. 

Web apps modules die route op basis van URL in plaats van als zodanig geen standaarddocument voor statische inhoud, in welk geval er wordt gebruikt.    

### <a name="handler-mappings"></a>Handlertoewijzingen

Gebruik dit gebied aangepast script processors voor het afhandelen van aanvragen voor specifieke extensies toevoegen. 

- **Uitbreiding**. De bestandsextensie moet worden verwerkt, zoals *.php of handler.fcgi. 
- **Pad naar script Processor**. Het absolute pad van het script-processor. Aanvragen voor bestanden die overeenkomen met de bestandsextensie wordt verwerkt door de script-processor. Het pad `D:\home\site\wwwroot` om te verwijzen naar de hoofdmap van uw app.
- **Extra argumenten**. Optionele argumenten voor de opdrachtregel voor de processor script 


### <a name="virtual-applications-and-directories"></a>Virtuele toepassingen en mappen 
 
Geef elke virtuele map en de bijbehorende fysieke pad ten opzichte van de hoofdmap van de website configureren van virtuele toepassingen en mappen. Desgewenst kunt u het selectievakje **toepassing** in een virtuele map als een toepassing wordt gemarkeerd.


## <a name="enabling-diagnostic-logs"></a>Diagnostische logboeken inschakelen

Diagnostische logboeken inschakelen:

1. Klik op **alle instellingen**in de blade voor uw web app.
2. Klik op **Diagnostische logboeken**. 

Opties voor het schrijven van diagnostische logboeken uit een webtoepassing die door logboekregistratie worden ondersteund: 

- **Logboekregistratie van toepassing**. Toepassingslogboeken schrijft naar het bestandssysteem. Registratie duurt voor een periode van 12 uur. 

**Niveau**. Wanneer de registratie van toepassing is ingeschakeld, wordt deze optie geeft u de hoeveelheid gegevens die worden vastgelegd (fout, waarschuwing, informatie of uitgebreid).

**Logboekregistratie van webserver**. Logboeken worden opgeslagen in de indeling W3C extended log. 

**Gedetailleerde foutberichten worden weergegeven**. Bespaart foutberichten gedetailleerde htm-bestanden. De bestanden worden opgeslagen in /LogFiles/DetailedErrors. 

**Tracering van mislukte aanvragen**. Logboeken kunnen geen aanvragen voor XML-bestanden. De bestanden worden opgeslagen onder /LogFiles/W3SVC*xxx*, waarbij xxx staat voor een unieke id. Deze map bevat een XSL-bestand en een of meer XML-bestanden. Zorg voor het downloaden van het XSL-bestand omdat deze functionaliteit biedt voor de opmaak en de inhoud van de XML-bestanden te filteren.

Als u wilt bekijken in de logboekbestanden, moet u FTP-referenties als volgt maken:

1. Klik op **alle instellingen**in de blade voor uw web app.
2. Klik op **referenties voor implementatie**.
3. Geef een gebruikersnaam en wachtwoord.
4. Klik op **Opslaan**.

![Set implementatie referenties][configure03]

De volledige FTP-gebruikersnaam is 'app\username' *app* is waar de naam van uw web app. De gebruikersnaam wordt weergegeven in de web app blade, onder **Essentials**.  

![FTP-implementatie referenties][configure02]

## <a name="other-configuration-tasks"></a>Andere configuratietaken

### <a name="ssl"></a>SSL 

In de Basic- of Standard-modus kunt u SSL-certificaten voor een aangepast domein te uploaden. Zie [HTTPS inschakelen voor een web app] voor meer informatie. 

De geüploade als certificaten wilt bekijken, klikt u op **Alle instellingen** > **Custom domains en SSL**.

### <a name="domain-names"></a>Domeinnamen

Voeg aangepaste domeinnamen voor uw web app. Zie [configureren voor een web app in Azure App-Service een aangepaste domeinnaam] voor meer informatie.

Klik op **Alle instellingen**om uw domeinnamen > **Custom domains en SSL**.

### <a name="deployments"></a>Implementaties

- Continuous deployment instellen. Zie [Met behulp van Git Web Apps in Azure App Service implementeren](./web-sites-deploy.md).
- Implementatie van "slots". Zie [implementeren op de Staging-omgevingen voor Web Apps in Azure App-Service].


U kunt uw implementatie-sleuven op **Alle instellingen** > **implementatie sleuven**.

### <a name="monitoring"></a>Monitoring

In de Basic- of Standard-modus kunt u de beschikbaarheid van HTTP of HTTPS eindpunten, vanaf drie locaties geo distributed testen. Een controle test mislukt als de HTTP-antwoordcode een fout (4xx of 5xx is) of het antwoord langer dan 30 seconden duurt. Een eindpunt is beschikbaar als u de tests voor controle van de opgegeven locaties slagen beschouwd. 

Zie voor meer informatie [procedure: web Eindpuntstatus controleren].

>[AZURE.NOTE] Als u wilt aan de slag met Azure App-Service voordat u aanmelden voor een account met Azure, gaat u naar de [App-Service probeert], waar u direct een starter tijdelijk web app in de App-Service maken kunt. Geen creditcard vereist; geen verplichtingen.

## <a name="next-steps"></a>Volgende stappen

- [Een aangepaste domeinnaam in Azure App-Service configureren]
- [HTTPS voor een app in Azure App-Service inschakelen]
- [Schaal van een web app in Azure App-Service]
- [Basisbeginselen voor Web Apps in Azure App-Service controleren]

<!-- URL List -->

[ASP.NET-SignalR]: http://www.asp.net/signalr
[Azure Portal]: https://portal.azure.com/
[Een aangepaste domeinnaam in Azure App-Service configureren]: ./web-sites-custom-domain-name.md
[Naar de Staging-omgevingen voor Web Apps in Azure App Service implementeren]: ./web-sites-staged-publishing.md
[HTTPS voor een app in Azure App-Service inschakelen]: ./web-sites-configure-ssl-certificate.md
[Procedure: web Eindpuntstatus controleren]: http://go.microsoft.com/fwLink/?LinkID=279906
[Basisbeginselen voor Web Apps in Azure App-Service controleren]: ./web-sites-monitor.md
[pipeline-modus]: http://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture#Application
[Schaal van een web app in Azure App-Service]: ./web-sites-scale.md
[socket.IO]: ./web-sites-nodejs-chat-app-socketio.md
[Probeer de Service App]: http://go.microsoft.com/fwlink/?LinkId=523751

<!-- IMG List -->

[configure01]: ./media/web-sites-configure/configure01.png
[configure02]: ./media/web-sites-configure/configure02.png
[configure03]: ./media/web-sites-configure/configure03.png
