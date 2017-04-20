<properties
    pageTitle="Azure App Service web app geavanceerde configuratie en uitbreidingen"
    description="Gebruik XML-Document Transformation(XDT) aangiften te transformeren van het bestand ApplicationHost.config in uw web app in Azure App Service en persoonlijke extensies beheer van aangepaste acties inschakelen als u wilt toevoegen."
    authors="cephalin"
    writer="cephalin"
    editor="mollybos"
    manager="wpickett"
    services="app-service"
    documentationCenter=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/25/2016"
    ms.author="cephalin"/>

# <a name="azure-app-service-web-app-advanced-config-and-extensions"></a>Azure App Service web app geavanceerde configuratie en uitbreidingen

Met behulp van declaraties van de [Transformatie van XML-Document](http://msdn.microsoft.com/library/dd465326.aspx) (XDT), kunt u het bestand [ApplicationHost.config](http://www.iis.net/learn/get-started/planning-your-iis-architecture/introduction-to-applicationhostconfig) transformeren in uw web app in Azure App-Service. U kunt ook XDT aangiften particuliere extensies waarmee aangepaste web app beheer acties toevoegen. Dit artikel bevat een monster Manager PHP web app-extensie waarmee het beheer van de PHP-instellingen via een webinterface.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

##<a id="transform"></a>Geavanceerde configuratie via ApplicationHost.config
De App Service platform biedt flexibiliteit en controle voor configuratie van web app. Hoewel het standaard configuratiebestand van IIS ApplicationHost.config niet rechtstreeks bewerken in App-Service beschikbaar is, ondersteunt het platform een declaratieve ApplicationHost.config transformatie model dat is gebaseerd op XML-Document transformatie (XDT).

Als u wilt gebruikmaken van deze functionaliteit transformatie, maakt u een bestand ApplicationHost.xdt met XDT inhoud en plaats onder de hoofdmap van de site (d:\home\site) in de [Kudu Console](https://github.com/projectkudu/kudu/wiki/Kudu-console). Wellicht moet u de Web App om wijzigingen van kracht te laten starten.

De volgende applicationHost.xdt-voorbeeld ziet u hoe een nieuwe aangepaste omgevingsvariabele toevoegen aan een web app die gebruikmaakt van PHP 5.4.

    <?xml version="1.0"?>
    <configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
        <system.webServer>
                <fastCgi>
                    <application>
                        <environmentVariables>
                                <environmentVariable name="CONFIGTEST" value="TEST" xdt:Transform="Insert" xdt:Locator="XPath(/configuration/system.webServer/fastCgi/application[contains(@fullPath,'5.4')]/environmentVariables)" />
                        </environmentVariables>
                    </application>
                </fastCgi>
        </system.webServer>
    </configuration>


Een logboekbestand met de details en status van de transformatie is beschikbaar via de FTP-hoofdmap onder LogFiles\Transform.

Raadpleeg [https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples](https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples)voor meer voorbeelden.

**Opmerking**<br />
Elementen uit de lijst met modules onder `system.webServer` kan niet worden verwijderd of opnieuw gerangschikt, maar de toevoegingen aan de lijst zijn mogelijk.


##<a id="extend"></a>Uitbreiden van uw web app

###<a id="overview"></a>Overzicht van de particuliere web app-extensies

App-Service ondersteunt extensies voor web app als een punt uitbreidbaarheid voor beheertaken. In feite zijn sommige functies van de App Service platform geïmplementeerd als vooraf geïnstalleerde uitbreidingen. U kunt tijdens de vooraf geïnstalleerde platform-uitbreidingen kunnen niet worden gewijzigd, maken en persoonlijke serverextensies configureren voor uw eigen web app. Deze functionaliteit is ook afhankelijk van XDT verklaringen. De belangrijkste stappen voor het maken van een persoonlijke web app-extensie zijn de volgende:

1. -App extensie **inhoud**: maken van een webtoepassing die wordt ondersteund door de App Service
2. Web app extensie **aangifte**: een ApplicationHost.xdt-bestand maken
3. Web app extensie **implementatie**: inhoud plaatsen in de map SiteExtensions onder`root`

Interne koppelingen voor het web app moet wijzen naar het pad naar het pad van de toepassing opgegeven in het bestand ApplicationHost.xdt. Elke wijziging van het bestand ApplicationHost.xdt is een web app recycle vereist.

**Opmerking**: extra informatie voor deze belangrijke elementen is beschikbaar op [https://github.com/projectkudu/kudu/wiki/Azure-Site-Extensions](https://github.com/projectkudu/kudu/wiki/Azure-Site-Extensions).

Een gedetailleerd voorbeeld is ter illustratie van de stappen voor het maken en een persoonlijke web app-extensie inschakelen opgenomen. De broncode voor het volgende voorbeeld van PHP Manager kan worden gedownload van [https://github.com/projectkudu/PHPManager](https://github.com/projectkudu/PHPManager).

###<a id="SiteSample"></a>Voorbeeld van de Web app extensie: PHP Manager

PHP-Manager is een web app-extensie waarmee web app beheerders op eenvoudige wijze weergeven en hun PHP instellingen configureren via een web-interface in plaats van PHP ini-bestanden niet rechtstreeks wijzigen. Gemeenschappelijke configuratiebestanden voor PHP opnemen in het php.ini bestand onder Program Files en de. user.ini bestand in de hoofdmap van uw web app. Omdat het php.ini-bestand kan niet rechtstreeks worden bewerkt op het platform App-Service, de Manager PHP extensie gebruikt de. user.ini bestand om wijzigingen in de instellingen van toepassing.

####<a id="PHPwebapp"></a>De webtoepassing Manager PHP

Dit is de introductiepagina van de Manager van de PHP-implementatie:

![TransformSitePHPUI][TransformSitePHPUI]

Zoals u ziet, is de uitbreiding van een web app net als een gewone webtoepassing, maar met een extra ApplicationHost.xdt bestand geplaatst in de hoofdmap van het web app (meer informatie over het bestand ApplicationHost.xdt zijn beschikbaar in de volgende sectie van dit artikel).

De Manager PHP extensie is gemaakt met de sjabloon voor Visual Studio ASP.NET MVC 4 webtoepassing. In de volgende weergave in Solution Explorer ziet u de structuur van de Manager van de PHP-extensie.

![TransformSiteSolEx][TransformSiteSolEx]

De enige speciale logica die nodig is voor het bestand I/O is om aan te geven waar de map wwwroot van de web app zich bevindt. Zoals in het volgende voorbeeld wordt getoond, omgeving variabele 'thuis' geeft aan dat het pad naar de hoofdmap van de web-app en het pad van de map wwwroot kan worden samengesteld door 'site\wwwroot' toe te voegen:

    /// <summary>
    /// Gives the location of the .user.ini file, even if one doesn't exist yet
    /// </summary>
    private static string GetUserSettingsFilePath()
    {
            var rootPath = Environment.GetEnvironmentVariable("HOME"); // For use on Azure Websites
            if (rootPath == null)
            {
                rootPath = System.IO.Path.GetTempPath(); // For testing purposes
            };
            var userSettingsFile = Path.Combine(rootPath, @"site\wwwroot\.user.ini");
            return userSettingsFile;
    }


Nadat u het pad naar de map hebt, kunt u gewone file i/o-bewerkingen voor het lezen en schrijven naar bestanden.

Voorzichtig met web app extensions van één punt betreft de afhandeling van interne koppelingen.  Als u alle koppelingen in uw HTML-bestanden die absolute paden tot interne koppelingen op uw web app geven hebt, moet u ervoor zorgen dat deze koppelingen worden voorafgegaan door uw naam als de hoofdmap. Dit is nodig omdat de hoofdmap voor uw extensie is ' /`[your-extension-name]`/ "in plaats van dat alleen"/", dus interne koppelingen dienovereenkomstig moeten worden bijgewerkt. Stel bijvoorbeeld dat uw code bevat een koppeling naar het volgende:

`"<a href="/Home/Settings">PHP Settings</a>"`

Als de koppeling deel van een web app-extensie uitmaakt, moet de koppeling in de volgende vorm:

`"<a href="/[your-site-name]/Home/Settings">Settings</a>"`

Kunt u deze vereiste omzeilen door een van beide gebruiken alleen relatieve paden in uw webtoepassing of in het geval van ASP.NET-toepassingen met behulp van de `@Html.ActionLink` methode die op de juiste koppelingen voor u gemaakt.

####<a id="XDT"></a>Het bestand applicationHost.xdt

De code voor uw web app extensie gaat onder %HOME%\SiteExtensions\[-extensie-naam]. Wij bellen u dit de hoofdmap van de uitbreiding.  

Om uw web app extensie registreren in het bestand applicationHost.config, moet u een bestand met de naam ApplicationHost.xdt in de basis van de plaats. De inhoud van het bestand ApplicationHost.xdt zijn als volgt:

    <?xml version="1.0"?>
    <configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
        <system.applicationHost>
                <sites>
                    <site name="%XDT_SCMSITENAME%" xdt:Locator="Match(name)">
                        <!-- NOTE: Add your extension name in the application paths below -->
                        <application path="/[your-extension-name]" xdt:Locator="Match(path)" xdt:Transform="Remove" />
                        <application path="/[your-extension-name]" applicationPool="%XDT_APPPOOLNAME%" xdt:Transform="Insert">
                            <virtualDirectory path="/" physicalPath="%XDT_EXTENSIONPATH%" />
                        </application>
                    </site>
                </sites>
        </system.applicationHost>
    </configuration>

De naam die u hebt geselecteerd als de naam van de uitbreiding moet dezelfde naam hebben als de hoofdmap van uw extensie.

Dit heeft het effect van het toevoegen van een nieuwe toepassingspad naar de `system.applicationHost` lijst met sites onder de SCM-site. De SCM-site is een site administration eindpunt met specifieke toegangsreferenties. De URL is `https://[your-site-name].scm.azurewebsites.net`.  

    <system.applicationHost>
        ...
        <site name="~1[your-website]" id="1716402716">
                <bindings>
                    <binding protocol="http" bindingInformation="*:80: [your-website].scm.azurewebsites.net" />
                    <binding protocol="https" bindingInformation="*:443: [your-website].scm.azurewebsites.net" />
                </bindings>
                <traceFailedRequestsLogging enabled="false" directory="C:\DWASFiles\Sites\[your-website]\VirtualDirectory0\LogFiles" />
                <detailedErrorLogging enabled="false" directory="C:\DWASFiles\Sites\[your-website]\VirtualDirectory0\LogFiles\DetailedErrors" />
                <logFile logSiteId="false" />
                <application path="/" applicationPool="[your-website]">
                    <virtualDirectory path="/" physicalPath="D:\Program Files (x86)\SiteExtensions\Kudu\1.24.20926.5" />
                </application>
                <!-- Note the custom changes that go here -->
                <application path="/[your-extension-name]" applicationPool="[your-website]">
                    <virtualDirectory path="/" physicalPath="C:\DWASFiles\Sites\[your-website]\VirtualDirectory0\SiteExtensions\[your-extension-name]" />
                </application>
            </site>
    </sites>
      ...
    </system.applicationHost>

###<a id="deploy"></a>Web app-extensie implementeren

Om uw web app extensie hebt geïnstalleerd, kunt u FTP te kopiëren van de bestanden van uw webtoepassing aan de `\SiteExtensions\[your-extension-name]` map van het web app die u wilt installeren van de uitbreiding.  Zorg ervoor dat het bestand ApplicationHost.xdt kopiëren naar deze locatie ook. Start uw web app als de extensie wilt inschakelen.

U moet uw web app extensie te zien:

`https://[your-site-name].scm.azurewebsites.net/[your-extension-name]`

Houd er rekening mee dat de URL er net als de URL voor uw web app, met dien verstande dat deze gebruikmaakt van HTTPS en '.scm' bevat.

Het is mogelijk om alle persoonlijke (niet vooraf geïnstalleerd) extensies voor uw web app uitschakelen tijdens de ontwikkeling en onderzoek door een app instellingen toe te voegen met de sleutel `WEBSITE_PRIVATE_EXTENSIONS` en een waarde van `0`.

>[AZURE.NOTE] Als u wilt aan de slag met Azure App-Service voordat u aanmelden voor een account met Azure, gaat u naar de [App-Service probeert](http://go.microsoft.com/fwlink/?LinkId=523751), waar u direct een starter tijdelijk web app in de App-Service maken kunt. Geen creditcard vereist; geen verplichtingen.

## <a name="whats-changed"></a>Wat er veranderd
* Zie voor een handleiding voor het wijzigen van Websites met App-Service: [Azure App Service en de Impact op de bestaande Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)

<!-- IMAGES -->
[TransformSitePHPUI]: ./media/web-sites-transform-extend/TransformSitePHPUI.png
[TransformSiteSolEx]: ./media/web-sites-transform-extend/TransformSiteSolEx.png
 
