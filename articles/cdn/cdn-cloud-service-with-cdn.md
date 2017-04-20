<properties
    pageTitle="Een cloud-service geïntegreerd met Azure CDN | Microsoft Azure"
    description="Een zelfstudie over u het implementeren van een cloud-service die de inhoud van een geïntegreerde Azure CDN eindpunt dient"
    services="cdn, cloud-services"
    documentationCenter=".net"
    authors="camsoper"
    manager="erikre"
    editor="tysonn"/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="casoper"/>


# <a name="intro"></a>Een cloud-service geïntegreerd met Azure CDN

Een cloud-service kan worden geïntegreerd met Azure CDN, de inhoud van de locatie van de cloud-service. Deze benadering biedt de volgende voordelen:

- Eenvoudig implementeren en afbeeldingen, scripts en opmaakmodellen in de cloud-service project mappen bijwerken
- Eenvoudige upgrade NuGet pakketten in de cloud-service, zoals jQuery of een Bootstrap versie
- Beheren van uw webtoepassing en uw CDN aangeboden inhoud alle van dezelfde interface voor Visual Studio
- Implementatieworkflow voor uw webtoepassing en uw inhoud CDN betekend uniforme
- Bundeling van ASP.NET en minification integreren met Azure CDN

## <a name="what-you-will-learn"></a>U leert ##

In deze zelfstudie leert u hoe:

-   [Een eindpunt Azure CDN integreren met uw service cloud en bedienen van statische inhoud in uw webpagina's van Azure CDN](#deploy)
-   [Cache-instellingen voor statische inhoud in uw cloud-service configureren](#caching)
-   [Inhoud van de controller acties via Azure CDN](#controller)
-   [Serve gebundeld en inhoud via Azure CDN minified behoud van de ervaring in Visual Studio voor foutopsporing in scripts](#bundling)
-   [Terugval scripts en CSS configureren wanneer uw CDN Azure off line is.](#fallback)

## <a name="what-you-will-build"></a>Wordt opgebouwd ##

U implementeert een cloud service Web-functie met behulp van de sjabloon voor ASP.NET MVC, code toevoegen om te dienen inhoud van een geïntegreerde Azure CDN, zoals een afbeelding, controller actie resultaten en de standaard JavaScript en CSS-bestanden, en ook code schrijven voor het fallback mechanisme configureren voor bundels betekend in het geval dat de CDN off line is.

## <a name="what-you-will-need"></a>Wat moet u ##

Deze zelfstudie bevat de volgende vereisten:

-   Een actieve [account Microsoft Azure](/account/)
-   Visual Studio 2015 met [Azure SDK](http://go.microsoft.com/fwlink/?linkid=518003&clcid=0x409)

> [AZURE.NOTE] Een Azure-account voor het voltooien van deze zelfstudie hebt u nodig:
> + U kunt [gratis een Azure account openen](/pricing/free-trial/) - credits dat u kunt u uitproberen betaalde Azure services en zelfs nadat ze gebruikt omhoog kunt u de account en gebruik gratis Azure services, zoals Websites.
> + Kunt u de [voordelen van MSDN subscriber activeren](/pricing/member-offers/msdn-benefits-details/) - uw MSDN-abonnement biedt u credits elke maand die u voor betaalde Azure services gebruiken kunt.

<a name="deploy"></a>
## <a name="deploy-a-cloud-service"></a>Een cloud-service implementeren ##

In deze sectie kunt u de standaard ASP.NET MVC sjabloon in Visual Studio 2015 implementeren op een wolk service Webrol, en vervolgens integreren met een nieuw CDN eindpunt. Volg de onderstaande instructies:

1. In Visual Studio 2015, een nieuwe Azure cloud service maken in het menu gaat u naar **Bestand > Nieuw > Project > Cloud > Azure Cloud Service**. Geef het een naam en klik op **OK**.

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-1-new-project.PNG)

2. **ASP.NET Webrol** te selecteren en op de **>** knop. Klik op OK.

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-2-select-role.PNG)

3. **MVC** en klik op **OK**.

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-3-mvc-template.PNG)

4. Nu deze rol Web publiceren naar een Azure cloud-service. Klik met de rechtermuisknop op de service cloud project en selecteer **publiceren**.

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-4-publish-a.png)

5. Als u nog niet ondertekend in Microsoft Azure, klikt u op de vervolgkeuzelijst **... een account toevoegen** en klik op het menu-item **toevoegen een account** .

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-5-publish-signin.png)

6. Log in met het Microsoft-account dat u gebruikt om uw Azure account te activeren op de pagina aanmelden.
7. Zodra u bent ingelogd, klikt u op **volgende**.

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-6-publish-signedin.png)

8. Ervan uitgaande dat u een wolk of de opslag-account nog niet hebt gemaakt, kunt Visual Studio u beide te maken. In het dialoogvenster **Cloud-Service maken en rekening** , typt u de gewenste naam en selecteer de gewenste regio. Klik vervolgens op **maken**.

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-7-publish-createserviceandstorage.png)

9. Controleer de configuratie in de instellingenpagina publiceren en klik op **publiceren**.

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-8-publish-finalize.png)

    >[AZURE.NOTE] Het publicatieproces voor cloud services duurt erg lang. De inschakelen Web implementeren voor alle rollen optie kunt u foutopsporing in uw service cloud veel sneller door te geven (maar tijdelijk) snel updates naar uw Web-rollen. Zie [een Cloud-Service met behulp van de hulpprogramma's voor Azure publiceren](http://msdn.microsoft.com/library/ff683672.aspx)voor meer informatie over deze optie.

    Als het **Logboek met faxactiviteit Microsoft Azure** wordt weergegeven dat de status publiceren **voltooid is**, maakt u een CDN eindpunt dat geïntegreerd met deze cloud-service.

    >[AZURE.WARNING] Als na de publicatie, de service geïmplementeerde wolk een foutscherm verschijnt wordt, is het waarschijnlijk omdat de cloud-service die u hebt geïmplementeerd met behulp van een [gastbesturingssysteem .NET 4.5.2 die niet is opgenomen](../cloud-services/cloud-services-guestos-update-matrix.md#news-updates).  U kunt dit probleem omzeilen door het [implementeren van .NET 4.5.2 als een taak is gestart](../cloud-services/cloud-services-dotnet-install-dotnet.md).

## <a name="create-a-new-cdn-profile"></a>Maak een nieuw profiel voor CDN

Een CDN-profiel is een verzameling van CDN eindpunten.  Elk profiel bevat een of meer CDN-eindpunten.  U kunt meerdere profielen worden gebruikt voor het ordenen van uw eindpunten CDN door internet-domein, webtoepassing of andere criteria.

> [AZURE.TIP] Als u al een CDN-profiel dat u wilt gebruiken voor deze zelfstudie hebt, kunt u doorgaan met het [maken een nieuw CDN-eindpunt](#create-a-new-cdn-endpoint).

[AZURE.INCLUDE [cdn-create-profile](../../includes/cdn-create-profile.md)]

## <a name="create-a-new-cdn-endpoint"></a>Maak een nieuwe CDN eindpunt

**Een nieuw eindpunt CDN voor uw opslag-account maken**

1. Ga naar uw profiel CDN in [Azure Management Portal](https://portal.azure.com).  U kan zijn vastgemaakt dat aan het dashboard in de vorige stap.  Als u niet, u vinden kunt door te klikken op **Bladeren**en vervolgens **CDN profielen**en op het profiel dat u van plan bent uw eindpunt wilt toevoegen.

    De blade CDN profiel verschijnt.

    ![CDN-profiel][cdn-profile-settings]

2. Klik op **Toevoegen eindpunt** .

    ![Eindpunt knop toevoegen][cdn-new-endpoint-button]

    Het blad **een eindpunt toevoegen** wordt weergegeven.

    ![Eindpunt blade toevoegen][cdn-add-endpoint]

3. Voer een **naam** voor dit eindpunt CDN.  Deze naam wordt gebruikt voor toegang tot de bronnen in de cache op het domein `<EndpointName>.azureedge.net`.

4. Selecteer in de vervolgkeuzelijst **type oorsprong** *Cloud-service*.  

5. Selecteer in de vervolgkeuzelijst **hostnaam oorsprong** uw cloud-service.

6. Laat de standaardinstellingen voor het **pad van oorsprong**, **herkomst host-header**en **Protocol/oorsprong poort**.  U moet ten minste één protocol (HTTP of HTTPS).

7. Klik op de knop **toevoegen** als u wilt maken van het nieuwe endpoint.

8. Als het eindpunt is gemaakt, wordt deze weergegeven in een lijst van eindpunten voor het profiel. De lijstweergave toont de URL moet worden gebruikt voor toegang tot inhoud in cache, als het oorspronkelijke domein.

    ![CDN eindpunt][cdn-endpoint-success]

    > [AZURE.NOTE] Het eindpunt wordt niet onmiddellijk worden gebruikt.  Het kan tot 90 minuten voor de inschrijving doorgeven via het netwerk van CDN duren. Gebruikers die proberen te gebruiken de CDN-domeinnaam onmiddellijk wordt statuscode 404 totdat de inhoud beschikbaar via de CDN is.

## <a name="test-the-cdn-endpoint"></a>Het eindpunt CDN testen

Wanneer de publicatiestatus **voltooid is**, open een browservenster en Ga naar * *http://<cdnName>*.azureedge.net/Content/bootstrap.css**. In mijn setup is deze URL:

    http://camservice.azureedge.net/Content/bootstrap.css

Dat komt overeen met de volgende URL oorsprong op het eindpunt van de CDN:

    http://camcdnservice.cloudapp.net/Content/bootstrap.css

Wanneer u navigeren naar * *http://*&lt;cdnName >*.azureedge.net/Content/bootstrap.css**, afhankelijk van uw browser, wordt u gevraagd om te downloaden of de bootstrap.css die afkomstig van uw gepubliceerde Web app is te openen.

![](media/cdn-cloud-service-with-cdn/cdn-1-browser-access.PNG)

U ook toegang tot een openbaar toegankelijke URL bij * *http://*&lt;servicenaam >*.cloudapp.net/** rechte lijn vanuit het eindpunt van de CDN. Bijvoorbeeld:

-   Een JS-bestand van het pad/script
-   Een bestand uit de/Content pad
-   Elke controller/actie
-   Als de query-tekenreeks is ingeschakeld op het eindpunt van de CDN, een URL met de query-tekenreeks

Met de bovenstaande configuratie kunt u in feite de hele cloud-service van host * *http://*&lt;cdnName >*.azureedge.net/**. Als ik ga naar **http://camservice.azureedge.net/ **, krijg ik het resultaat van de actie van Home/Index.

![](media/cdn-cloud-service-with-cdn/cdn-2-home-page.PNG)

Dit betekent niet, maar dat altijd is een goed idee (of in het algemeen een goed idee) een hele cloud-service via Azure CDN fungeren. Enkele valkuilen zijn:

-   Hiervoor is de gehele site als public, omdat Azure CDN persoonlijke inhoud op dit moment niet kan dienen.
-   Als het eindpunt CDN voor een of andere reden off line gaat of gepland onderhoud of gebruikersfouten, uw hele cloud service offline gaat tenzij de klanten kunnen worden omgeleid naar de oorspronkelijke URL * *http://*&lt;servicenaam >*.cloudapp.net/**.
-   Zelfs met de aangepaste Cache-instellingen (Zie [Cacheopties voor configureren voor statische bestanden in de cloud-service](#caching)), een CDN-eindpunt de prestaties van zeer dynamische inhoud niet worden verbeterd. Als u de startpagina van uw eindpunt CDN laden als hierboven aangetoond aankondiging die ten minste 5 seconden kostte de standaardintroductiepagina het eerst een tamelijk eenvoudige pagina is laden probeert. Stel, wat gebeurt er met de clientervaring als deze pagina bevat dynamische inhoud die moet worden elke minuut bijgewerkt. Dynamische inhoud vanuit het eindpunt van een CDN vereist korte cache vervaldatum, die wordt vertaald naar frequente Cachemissers op het eindpunt van de CDN. Dit kwaad de prestaties van uw cloud-service en het doel van een CDN defeats.

Het alternatief is om te bepalen welke inhoud van Azure CDN dienen op basis van per geval in de cloud-service. Te dien einde hebt u toegang tot afzonderlijke bestanden vanuit het eindpunt CDN al gezien. Leest u hoe u een specifieke controller actie via het eindpunt CDN dienen in de [inhoud van de acties via Azure CDN domeincontroller fungeren](#controller).

<a name="caching"></a>
## <a name="configure-caching-options-for-static-files-in-your-cloud-service"></a>Opties voor opslaan in cache voor statische bestanden in de cloud-service configureren ##

Met Azure CDN integratie in de cloud-service, kunt u aangeven hoe u statische inhoud in de cache opgeslagen in de CDN-eindpunt. Hiertoe opent u *Web.config* van uw webproject rol (bv. WebRole1) en voeg een `<staticContent>` element `<system.webServer>`. De onderstaande XML configureert u de cache om de 3 dagen vervalt.  

    <system.webServer>
      <staticContent>
        <clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="3.00:00:00"/>
      </staticContent>
      ...
    </system.webServer>

Als u dit doet, zien alle statische bestanden in de cloud-service dezelfde regel in de cache CDN. Voor meer gedetailleerde controle van de cache-instellingen, een *Web.config* -bestand in een map en voeg er uw instellingen. Bijvoorbeeld een *Web.config* -bestand toevoegen aan de map *\Content* en de inhoud vervangen door de volgende XML:

    <?xml version="1.0"?>
    <configuration>
      <system.webServer>
        <staticContent>
          <clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="15.00:00:00"/>
        </staticContent>
      </system.webServer>
    </configuration>

Deze instelling zorgt ervoor dat alle statische bestanden uit de map *\Content* in de cache opgeslagen gedurende 15 dagen.

Voor meer informatie over het configureren van de `<clientCache>` -element, Zie [clientcache &lt;clientCache >](http://www.iis.net/configreference/system.webserver/staticcontent/clientcache).

In de [inhoud van de controller acties via Azure CDN](#controller), ook leest u hoe u de cache-instellingen voor de controller actie resultaten in de cache CDN kunt configureren.

<a name="controller"></a>
## <a name="serve-content-from-controller-actions-through-azure-cdn"></a>Inhoud van de controller acties via Azure CDN ##

Wanneer u een cloud service Webrol met Azure CDN integreert, is het betrekkelijk eenvoudig inhoud tegen acties via het CDN Azure controller. Dan levert uw cloud-service via Azure CDN (aangetoond hierboven), [Maarten Balliauw](https://twitter.com/maartenballiauw) laat zien hoe u dit doen met een leuke MemeGenerator-controller in [degressieve latentie op het web met de CDN Azure](http://channel9.msdn.com/events/TechDays/Techdays-2014-the-Netherlands/Reducing-latency-on-the-web-with-the-Windows-Azure-CDN). Ik zal gewoon reproduceren deze hier.

Stel dat in de cloud-service die u wilt genereren op basis van een afbeelding van een jonge chucks Norris (foto door [Alan licht](http://www.flickr.com/photos/alan-light/218493788/)) als volgt memes:

![](media/cdn-cloud-service-with-cdn/cdn-5-memegenerator.PNG)

U hebt een eenvoudige `Index` de meme actie waarmee de klanten op te geven van de items in de afbeelding wordt gegenereerd zodra ze boeken aan de actie. Aangezien het chucks Norris, verwacht u op deze pagina wereldwijd sterk populair geworden. Dit is een goed voorbeeld van een semi-dynamische inhoud die met Azure CDN.

De stappen uit om deze controller actie in te stellen:

1. Maak een nieuw:. cs-bestand genaamd *MemeGeneratorController.cs* in de map *\Controllers* en de inhoud vervangen door de volgende code. Zorg dat u het gemarkeerde gedeelte vervangen door de naam van uw CDN.  

        using System;
        using System.Collections.Generic;
        using System.Diagnostics;
        using System.Drawing;
        using System.IO;
        using System.Net;
        using System.Web.Hosting;
        using System.Web.Mvc;
        using System.Web.UI;

        namespace WebRole1.Controllers
        {
            public class MemeGeneratorController : Controller
            {
                static readonly Dictionary<string, Tuple<string ,string>> Memes = new Dictionary<string, Tuple<string, string>>();

                public ActionResult Index()
                {
                    return View();
                }

                [HttpPost, ActionName("Index")]
                public ActionResult Index_Post(string top, string bottom)
                {
                    var identifier = Guid.NewGuid().ToString();
                    if (!Memes.ContainsKey(identifier))
                    {
                        Memes.Add(identifier, new Tuple<string, string>(top, bottom));
                    }

                    return Content("<a href=\"" + Url.Action("Show", new {id = identifier}) + "\">here's your meme</a>");
                }

                [OutputCache(VaryByParam = "*", Duration = 1, Location = OutputCacheLocation.Downstream)]
                public ActionResult Show(string id)
                {
                    Tuple<string, string> data = null;
                    if (!Memes.TryGetValue(id, out data))
                    {
                        return new HttpStatusCodeResult(HttpStatusCode.NotFound);
                    }

                    if (Debugger.IsAttached) // Preserve the debug experience
                    {
                        return Redirect(string.Format("/MemeGenerator/Generate?top={0}&bottom={1}", data.Item1, data.Item2));
                    }
                    else // Get content from Azure CDN
                    {
                        return Redirect(string.Format("http://<yourCdnName>.azureedge.net/MemeGenerator/Generate?top={0}&bottom={1}", data.Item1, data.Item2));
                    }
                }

                [OutputCache(VaryByParam = "*", Duration = 3600, Location = OutputCacheLocation.Downstream)]
                public ActionResult Generate(string top, string bottom)
                {
                    string imageFilePath = HostingEnvironment.MapPath("~/Content/chuck.bmp");
                    Bitmap bitmap = (Bitmap)Image.FromFile(imageFilePath);

                    using (Graphics graphics = Graphics.FromImage(bitmap))
                    {
                        SizeF size = new SizeF();
                        using (Font arialFont = FindBestFitFont(bitmap, graphics, top.ToUpperInvariant(), new Font("Arial Narrow", 100), out size))
                        {
                            graphics.DrawString(top.ToUpperInvariant(), arialFont, Brushes.White, new PointF(((bitmap.Width - size.Width) / 2), 10f));
                        }
                        using (Font arialFont = FindBestFitFont(bitmap, graphics, bottom.ToUpperInvariant(), new Font("Arial Narrow", 100), out size))
                        {
                            graphics.DrawString(bottom.ToUpperInvariant(), arialFont, Brushes.White, new PointF(((bitmap.Width - size.Width) / 2), bitmap.Height - 10f - arialFont.Height));
                        }
                    }

                    MemoryStream ms = new MemoryStream();
                    bitmap.Save(ms, System.Drawing.Imaging.ImageFormat.Png);
                    return File(ms.ToArray(), "image/png");
                }

                private Font FindBestFitFont(Image i, Graphics g, String text, Font font, out SizeF size)
                {
                    // Compute actual size, shrink if needed
                    while (true)
                    {
                        size = g.MeasureString(text, font);

                        // It fits, back out
                        if (size.Height < i.Height &&
                             size.Width < i.Width) { return font; }

                        // Try a smaller font (90% of old size)
                        Font oldFont = font;
                        font = new Font(font.Name, (float)(font.Size * .9), font.Style);
                        oldFont.Dispose();
                    }
                }
            }
        }

2. Klik met de rechtermuisknop in de standaard `Index()` actie en selecteer **Weergave toevoegen**.

    ![](media/cdn-cloud-service-with-cdn/cdn-6-addview.PNG)

3.  Accepteer de volgende instellingen en klik op **toevoegen**.

    ![](media/cdn-cloud-service-with-cdn/cdn-7-configureview.PNG)

4. De nieuwe *Views\MemeGenerator\Index.cshtml* te openen en de inhoud vervangen door de volgende eenvoudige HTML-code voor het indienen van de items:

        <h2>Meme Generator</h2>

        <form action="" method="post">
            <input type="text" name="top" placeholder="Enter top text here" />
            <br />
            <input type="text" name="bottom" placeholder="Enter bottom text here" />
            <br />
            <input class="btn" type="submit" value="Generate meme" />
        </form>

5. Publiceer de cloud-service opnieuw en Ga naar * *http://*&lt;servicenaam >*.cloudapp.net/MemeGenerator/Index** in uw browser.

Wanneer u de formulierwaarden te verzenden `/MemeGenerator/Index`, de `Index_Post` actie-methode geeft als resultaat een koppeling naar de `Show` methode van de actie met de desbetreffende invoer-ID. Wanneer u op de koppeling klikt, kunt u bereiken met de volgende code:  

    [OutputCache(VaryByParam = "*", Duration = 1, Location = OutputCacheLocation.Downstream)]
    public ActionResult Show(string id)
    {
        Tuple<string, string> data = null;
        if (!Memes.TryGetValue(id, out data))
        {
            return new HttpStatusCodeResult(HttpStatusCode.NotFound);
        }

        if (Debugger.IsAttached) // Preserve the debug experience
        {
            return Redirect(string.Format("/MemeGenerator/Generate?top={0}&bottom={1}", data.Item1, data.Item2));
        }
        else // Get content from Azure CDN
        {
            return Redirect(string.Format("http://<yourCDNName>.azureedge.net/MemeGenerator/Generate?top={0}&bottom={1}", data.Item1, data.Item2));
        }
    }

Als uw lokale debugger is aangesloten, krijgt u de normale debug ervaring met een lokale omleiding. Als het in de cloud-service wordt uitgevoerd, wordt naar het omleiden:

    http://<yourCDNName>.azureedge.net/MemeGenerator/Generate?top=<formInput>&bottom=<formInput>

Dat komt overeen met de volgende URL oorsprong op het eindpunt van de CDN:

    http://<youCloudServiceName>.cloudapp.net/MemeGenerator/Generate?top=<formInput>&bottom=<formInput>


U kunt de `OutputCacheAttribute` van het kenmerk op de `Generate` methode om aan te geven hoe het resultaat van de actie moet worden in de cache die Azure CDN wordt gerespecteerd. De volgende code geeft een Vervaltijd van de cache van 1 uur (3600 seconden).

    [OutputCache(VaryByParam = "*", Duration = 3600, Location = OutputCacheLocation.Downstream)]

Ook kan u dienen om de inhoud van elke controller actie in de cloud-service via uw Azure CDN, met de gewenste optie voor opslaan in cache.

In de volgende sectie leest u hoe u de gebundelde en minified scripts en CSS via Azure CDN dienen.

<a name="bundling"></a>
## <a name="integrate-aspnet-bundling-and-minification-with-azure-cdn"></a>Bundeling van ASP.NET en minification integreren met Azure CDN ##

Scripts en CSS-opmaakmodellen niet vaak worden gewijzigd en zijn voornaamste kandidaten voor de Azure CDN-cache. Voor de gehele functie Web via uw CDN Azure is de eenvoudigste manier om bundeling en minification integreren met Azure CDN. Echter als u niet kunt om dit te doen, leest u hoe dat te doen terwijl het behouden van de ervaring van de gewenste ontwikkelaars van ASP.NET bundeling en minification, zoals:

-   Geweldige debug modus ervaring
-   Gestroomlijnde implementatie
-   Directe updates van clients voor script/CSS versie-upgrades
-   Fallback mechanisme wanneer uw eindpunt CDN mislukt
-   Codewijzigingen minimaliseren

Open in het project die u hebt gemaakt in [een eindpunt Azure CDN met Azure website en dienen statische inhoud in uw webpagina's CDN Azure integreren](#deploy) **WebRole1** , *App_Start\BundleConfig.cs* en een kijkje nemen op de `bundles.Add()` methodeaanroepen.

    public static void RegisterBundles(BundleCollection bundles)
    {
        bundles.Add(new ScriptBundle("~/bundles/jquery").Include(
                    "~/Scripts/jquery-{version}.js"));
        ...
    }

De eerste `bundles.Add()` instructie voegt u een bundel script op de virtuele map `~/bundles/jquery`. Vervolgens opent u *Views\Shared\_Layout.cshtml* om te zien hoe de code script bundel wordt weergegeven. U moet de volgende regel code Razor vinden:

    @Scripts.Render("~/bundles/jquery")

Wanneer deze Razor-code wordt uitgevoerd in de rol van Azure Web, krijgt deze een `<script>` de bundel script vergelijkbaar met de volgende code:

    <script src="/bundles/jquery?v=FVs3ACwOLIVInrAl5sdzR2jrCDmVOWFbZMY6g6Q0ulE1"></script>

Echter, als deze wordt uitgevoerd in Visual Studio door `F5`, het elk scriptbestand in de bundel afzonderlijk worden weergegeven (in het geval slechts één scriptbestand staat in de bundel):

    <script src="/Scripts/jquery-1.10.2.js"></script>

Hiermee kunt u fouten opsporen in de JavaScript-code in uw ontwikkelomgeving terwijl minder gelijktijdige clientverbindingen (bundeling) en het bestand verbeteren prestaties (minification) van de productie downloaden. Het is een geweldige functie voor het behoud van de integratie van de Azure CDN. Voorts omdat de gesmolten bundel al een automatisch gegenereerde tekenreeks bevat, u wilt dat de functionaliteit repliceren zodat de telkens als u uw versie van jQuery via NuGet bijwerkt, deze kan worden bijgewerkt op de client zo spoedig mogelijk.

Voer de onderstaande stappen voor bundeling van integratie van ASP.NET en minification met uw eindpunt CDN.

1. Wijzigen in *App_Start\BundleConfig.cs*, de `bundles.Add()` methoden om een andere [constructor bundel](http://msdn.microsoft.com/library/jj646464.aspx), die een CDN-adres gebruiken. Hiervoor vervangen door de `RegisterBundles` methodedefinitie met de volgende code:  

        public static void RegisterBundles(BundleCollection bundles)
        {
            bundles.UseCdn = true;
            var version = System.Reflection.Assembly.GetAssembly(typeof(Controllers.HomeController))
                .GetName().Version.ToString();
            var cdnUrl = "http://<yourCDNName>.azureedge.net/{0}?v=" + version;

            bundles.Add(new ScriptBundle("~/bundles/jquery", string.Format(cdnUrl, "bundles/jquery")).Include(
                        "~/Scripts/jquery-{version}.js"));

            bundles.Add(new ScriptBundle("~/bundles/jqueryval", string.Format(cdnUrl, "bundles/jqueryval")).Include(
                        "~/Scripts/jquery.validate*"));

            // Use the development version of Modernizr to develop with and learn from. Then, when you're
            // ready for production, use the build tool at http://modernizr.com to pick only the tests you need.
            bundles.Add(new ScriptBundle("~/bundles/modernizr", string.Format(cdnUrl, "bundles/modernizer")).Include(
                        "~/Scripts/modernizr-*"));

            bundles.Add(new ScriptBundle("~/bundles/bootstrap", string.Format(cdnUrl, "bundles/bootstrap")).Include(
                        "~/Scripts/bootstrap.js",
                        "~/Scripts/respond.js"));

            bundles.Add(new StyleBundle("~/Content/css", string.Format(cdnUrl, "Content/css")).Include(
                        "~/Content/bootstrap.css",
                        "~/Content/site.css"));
        }

    Zorg ervoor dat u vervangen `<yourCDNName>` met de naam van uw CDN Azure.

    In duidelijke taal instellen `bundles.UseCdn = true` en een zorgvuldig samengestelde CDN-URL toegevoegd aan elke bundel. Bijvoorbeeld de eerste constructor in de code:

        new ScriptBundle("~/bundles/jquery", string.Format(cdnUrl, "bundles/jquery"))

    is hetzelfde als:

        new ScriptBundle("~/bundles/jquery", string.Format(cdnUrl, "http://<yourCDNName>.azureedge.net/bundles/jquery?v=<W.X.Y.Z>"))

    Deze constructor doorgegeven ASP.NET bundeling en minification Renderbestanden afzonderlijke script als lokaal worden opgespoord, maar het opgegeven CDN-adres gebruiken voor toegang tot het desbetreffende script. Let echter twee belangrijke kenmerken met deze zorgvuldig vervaardigde CDN-URL:

    -   De oorsprong van deze CDN-URL is `http://<yourCloudService>.cloudapp.net/bundles/jquery?v=<W.X.Y.Z>`, die daadwerkelijk op de virtuele map van de bundel script in de cloud-service is.
    -   Aangezien u de constructor CDN, bevat de CDN scriptcode voor de bundel niet meer de automatisch gegenereerde tekenreeks in de URL weergegeven. Telkens wanneer de bundel script als u wilt dat een cache-missers bij uw CDN Azure is aangepast, moet u handmatig een unieke tekenreeks genereren. Deze unieke tekenreeks moet blijven constant gedurende de hele levenscyclus van de implementatie te maximaliseren treffers in cache op uw CDN Azure nadat de bundel is geïmplementeerd op hetzelfde moment.
    -   De query-tekenreeks v = < W.X.Y.Z > trekkrachten van *Properties\AssemblyInfo.cs* in uw project Web rol. U kunt een werkstroom voor implementatie met de assembly-versie wordt verhoogd telkens wanneer u naar Azure publiceert hebben. Of u kunt *Properties\AssemblyInfo.cs* alleen wijzigen in uw project aan de tekenreeks automatisch opgehoogd telkens wanneer u bouwt met het jokerteken ' *'. Bijvoorbeeld:

            [assembly: AssemblyVersion("1.0.0.*")]

        Een andere strategie voor het stroomlijnen van het genereren van een unieke tekenreeks voor de duur van een implementatie werkt hier.

3. De cloud-service opnieuw publiceren en toegang tot de introductiepagina.

4. De HTML-code voor de pagina weergeven. U moet mogelijk de CDN-URL weergegeven, met een unieke tekenreeks telkens wanneer u wijzigingen opnieuw naar de cloud-service publiceren Zie. Bijvoorbeeld:  

        ...

        <link href="http://camservice.azureedge.net/Content/css?v=1.0.0.25449" rel="stylesheet"/>

        <script src="http://camservice.azureedge.net/bundles/modernizer?v=1.0.0.25449"></script>

        ...

        <script src="http://camservice.azureedge.net/bundles/jquery?v=1.0.0.25449"></script>

        <script src="http://camservice.azureedge.net/bundles/bootstrap?v=1.0.0.25449"></script>

        ...

5. Foutopsporing in Visual Studio de service cloud in Visual Studio door `F5`.,

6. De HTML-code voor de pagina weergeven. Elk scriptbestand afzonderlijk weergegeven zodat u een consistent foutopsporing in Visual Studio ervaring wordt nog steeds weergegeven.  

        ...

            <link href="/Content/bootstrap.css" rel="stylesheet"/>
        <link href="/Content/site.css" rel="stylesheet"/>

            <script src="/Scripts/modernizr-2.6.2.js"></script>

        ...

            <script src="/Scripts/jquery-1.10.2.js"></script>

            <script src="/Scripts/bootstrap.js"></script>
        <script src="/Scripts/respond.js"></script>

        ...   

<a name="fallback"></a>
## <a name="fallback-mechanism-for-cdn-urls"></a>Fallback mechanisme voor CDN URL 's ##

Als uw eindpunt Azure CDN voor een of andere reden mislukt, wilt u uw webpagina's zijn slim genoeg om de toegang tot uw webserver oorsprong als de optie voor het laden van de JavaScript- of Bootstrap voor terugval. Ernstige verliezen van afbeeldingen op uw website door CDN niet beschikbaar, maar veel meer ernstige verliezen pagina essentiële functionaliteit van uw scripts en stylesheets is.

De klasse [bundel](http://msdn.microsoft.com/library/system.web.optimization.bundle.aspx) bevat een eigenschap met de naam [CdnFallbackExpression](http://msdn.microsoft.com/library/system.web.optimization.bundle.cdnfallbackexpression.aspx) waarmee de terugval mechanisme CDN niet configureren. Volg de onderstaande stappen voor het gebruik van deze eigenschap:

1. In uw rol webproject open *App_Start\BundleConfig.cs*, waar u een CDN-URL toegevoegd in de [constructor van de bundel](http://msdn.microsoft.com/library/jj646464.aspx), en brengt de volgende gemarkeerde wijzigingen fallback mechanisme toevoegen aan de standaard bundels:  

        public static void RegisterBundles(BundleCollection bundles)
        {
            var version = System.Reflection.Assembly.GetAssembly(typeof(BundleConfig))
                .GetName().Version.ToString();
            var cdnUrl = "http://cdnurl.azureedge.net/.../{0}?" + version;
            bundles.UseCdn = true;

            bundles.Add(new ScriptBundle("~/bundles/jquery", string.Format(cdnUrl, "bundles/jquery"))
                        { CdnFallbackExpression = "window.jquery" }
                        .Include("~/Scripts/jquery-{version}.js"));

            bundles.Add(new ScriptBundle("~/bundles/jqueryval", string.Format(cdnUrl, "bundles/jqueryval"))
                        { CdnFallbackExpression = "$.validator" }
                        .Include("~/Scripts/jquery.validate*"));

            // Use the development version of Modernizr to develop with and learn from. Then, when you&#39;re
            // ready for production, use the build tool at http://modernizr.com to pick only the tests you need.
            bundles.Add(new ScriptBundle("~/bundles/modernizr", string.Format(cdnUrl, "bundles/modernizer"))
                        { CdnFallbackExpression = "window.Modernizr" }
                        .Include("~/Scripts/modernizr-*"));

            bundles.Add(new ScriptBundle("~/bundles/bootstrap", string.Format(cdnUrl, "bundles/bootstrap"))     
                        { CdnFallbackExpression = "$.fn.modal" }
                        .Include(
                                "~/Scripts/bootstrap.js",
                                "~/Scripts/respond.js"));

            bundles.Add(new StyleBundle("~/Content/css", string.Format(cdnUrl, "Content/css")).Include(
                        "~/Content/bootstrap.css",
                        "~/Content/site.css"));
        }

    Wanneer `CdnFallbackExpression` is niet nul, script wordt geïnjecteerd in de HTML-code om te testen of de bundel met succes is geladen en als dat niet rechtstreeks toegang tot de bundel vanuit de oorsprong webserver. Deze eigenschap moet worden ingesteld op een JavaScript-expressie waarmee wordt getest of de respectieve CDN bundel correct wordt geladen. De expressie die nodig is voor het testen van elke bundel is afhankelijk van de inhoud. Voor de standaard bundels hierboven:

    -   `window.jquery`is gedefinieerd in jquery-{versie,} js
    -   `$.validator`is gedefinieerd in jquery.validate.js
    -   `window.Modernizr`is gedefinieerd in de modernizer-{versie,} js
    -   `$.fn.modal`is gedefinieerd in bootstrap.js

    U hebt waarschijnlijk al gemerkt dat ik niet ingesteld CdnFallbackExpression voor de `~/Cointent/css` bundel. Dit komt doordat er is momenteel een [bug in System.Web.Optimization](https://aspnetoptimization.codeplex.com/workitem/104) die injects een `<script>` -tag voor de terugval CSS in plaats van de verwachte `<link>` code.

    Er is echter een goede [Stijl bundel terugval](https://github.com/EmberConsultingGroup/StyleBundleFallback) aangeboden door [Ember Consulting Group](https://github.com/EmberConsultingGroup).

2. De oplossing wordt gebruikt voor CSS, maak een nieuw:. cs-bestand in uw Web rol project *App_Start* map met de naam *StyleBundleExtensions.cs*en de inhoud vervangen door de [code van GitHub](https://github.com/EmberConsultingGroup/StyleBundleFallback/blob/master/Website/App_Start/StyleBundleExtensions.cs).

4. In de *App_Start\StyleFundleExtensions.cs*, de naam van de naamruimte van de rol van uw Web naam (bijvoorbeeld **WebRole1**).

3. Ga terug naar `App_Start\BundleConfig.cs` en wijzig de laatste `bundles.Add` -instructie met de volgende code gemarkeerd:  

        bundles.Add(new StyleBundle("~/Content/css", string.Format(cdnUrl, "Content/css"))
            <mark>.IncludeFallback("~/Content/css", "sr-only", "width", "1px")</mark>
            .Include(
                  "~/Content/bootstrap.css",
                  "~/Content/site.css"));

    Deze nieuwe uitbreidingsmethode het hetzelfde idee wordt vanaf een script in de HTML-code te controleren van de DOM voor het een overeenkomende naam, naam van de regel en regel waarde die is gedefinieerd in de CSS-bundel en valt terug naar de oorsprong webserver als niet aan de overeenkomst.

4. De cloud-service opnieuw te publiceren en toegang tot de introductiepagina.

5. De HTML-code voor de pagina weergeven. Zult u ingespoten scripts met de volgende strekking:    

        ...

        <link href="http://az632148.azureedge.net/Content/css?v=1.0.0.25474" rel="stylesheet"/>
        <script>(function() {
                        var loadFallback,
                            len = document.styleSheets.length;
                        for (var i = 0; i < len; i++) {
                            var sheet = document.styleSheets[i];
                            if (sheet.href.indexOf('http://camservice.azureedge.net/Content/css?v=1.0.0.25474') !== -1) {
                                var meta = document.createElement('meta');
                                meta.className = 'sr-only';
                                document.head.appendChild(meta);
                                var value = window.getComputedStyle(meta).getPropertyValue('width');
                                document.head.removeChild(meta);
                                if (value !== '1px') {
                                    document.write('<link href="/Content/css" rel="stylesheet" type="text/css" />');
                                }
                            }
                        }
                        return true;
                    }())||document.write('<script src="/Content/css"><\/script>');</script>

            <script src="http://camservice.azureedge.net/bundles/modernizer?v=1.0.0.25474"></script>
        <script>(window.Modernizr)||document.write('<script src="/bundles/modernizr"><\/script>');</script>

        ...

            <script src="http://camservice.azureedge.net/bundles/jquery?v=1.0.0.25474"></script>
        <script>(window.jquery)||document.write('<script src="/bundles/jquery"><\/script>');</script>

            <script src="http://camservice.azureedge.net/bundles/bootstrap?v=1.0.0.25474"></script>
        <script>($.fn.modal)||document.write('<script src="/bundles/bootstrap"><\/script>');</script>

        ...


    Ingevoegd script voor de CSS-bundel bevat nog steeds de onjuiste overblijfsel van het `CdnFallbackExpression` eigenschap in de regel:

        }())||document.write('<script src="/Content/css"><\/script>');</script>

    Maar sinds het eerste deel van de || expressie retourneert altijd true (in de regel die direct boven), de functie document.write() wordt nooit uitgevoerd.

## <a name="more-information"></a>Meer informatie ##
- [Overzicht van de Azure Content Delivery Network (CDN)](http://msdn.microsoft.com/library/azure/ff919703.aspx)
- [Met behulp van Azure CDN](cdn-create-new-endpoint.md)
- [Bundeling van ASP.NET en Minification](http://www.asp.net/mvc/tutorials/mvc-4/bundling-and-minification)



[new-cdn-profile]: ./media/cdn-cloud-service-with-cdn/cdn-new-profile.png
[cdn-profile-settings]: ./media/cdn-cloud-service-with-cdn/cdn-profile-settings.png
[cdn-new-endpoint-button]: ./media/cdn-cloud-service-with-cdn/cdn-new-endpoint-button.png
[cdn-add-endpoint]: ./media/cdn-cloud-service-with-cdn/cdn-add-endpoint.png
[cdn-endpoint-success]: ./media/cdn-cloud-service-with-cdn/cdn-endpoint-success.png
