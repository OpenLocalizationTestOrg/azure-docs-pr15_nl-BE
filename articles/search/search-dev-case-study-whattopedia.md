<properties 
    pageTitle="Azure Search Developer Case-Study: Hoe WhatToPedia een portal infomedia gebouwd op Microsoft Azure | Microsoft Azure | De zoekservice hosted cloud" 
    description="Informatie over het maken van een portal en meta-zoekmachine van informatie, met Azure zoeken, een zoekservice gehost wolk voor ontwikkelaars." 
    services="search, sql-database,  storage, web-sites" 
    documentationCenter="" 
    authors="HeidiSteen" 
    manager="jhubbard"/>

<tags 
    ms.service="search" 
    ms.devlang="NA" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="search" 
    ms.date="08/29/2016" 
    ms.author="heidist"/>

# <a name="azure-search-developer-case-study"></a>Case-Study van Azure Search Developer

## <a name="how-whattopediacomhttpwhattopediacom-built-an-infomedia-portal-on-microsoft-azure"></a>Hoe [WhatToPedia.com](http://whattopedia.com/) een portal infomedia gebouwd op Microsoft Azure

 ![][6]  &nbsp;&nbsp;&nbsp;  <font size="9">Het grote idee</font> 


Ons idee is het bouwen van een informatie-portal die helpt bij het verbinding maken met andere detailhandelaren met een zeer relevant, binnen het bereik van de zoekopdracht ervaring, vergelijkbaar met hoe portals match toeristen van reizen met hotels, restaurants en entertainment in uncharted grondgebied kopers. 

De portal die we voor ogen hebt bieden een uitzonderlijk hoge kwaliteit zoekervaring detailhandelaar gegevens op een bepaalde markt, biedt zodat kopers zoeken naar winkels op basis van locatie en andere faciliteiten de detailhandelaar. Zullen we de zoekmachine met een initiële gegevensset zaad, maar diepere waarde zal worden gebouwd in de tijd met behulp van de detailhandelaar abonnees die gegevens over hun bedrijf te boeken. Speciale aanbiedingen, nieuwe producten, populaire merken, interne speciale diensten-– zijn voorbeelden van gegevens die wordt waarde toegevoegd aan onze site. Deze gegevens zelf gemeld en geïntegreerd in de corpus zoeken wanneer de detailhandelaar zich als abonnee aanmeldt. Reclame, plus het abonnementsmodel bieden de inkomsten genereren voor onze nieuwe onderneming.

Zoeken is het model in de overheersende interactie, op een zuivere cloud-platform. Voor de toepassing van de omvang en de lage kosten worden vrijwel alles wat die we, van de portal ervaringen op bronbeheer doen, via een on line service. Met behulp van een zoekmachine die beschikt over de functies die we nodig hebben uit de verpakking, kunnen we snel een zoektoepassing maken, zonder te ontwikkelen en beheren van een zoekactie in de engine onszelf.

## <a name="what-we-built"></a>Wat we gebouwd

WhatToPedia is een bedrijf opstarten infomedia. We gebruikten [WhatToPedia.com](http://whattopedia.com/) – - momenteel in test-markt in Noord-Europa met een startdatum van 2 februari 2015. Onze klantenkring is hoofdzakelijk baksteen en mortel winkels die een betaalbare online zijn eenvoudig te beheren en te onderhouden.

Onze taak is het aantrekken van kopers via een goede online zoekfunctie, waarbij de resultaten op stad of wijk, merken, namen opslaan of typen opslaan. Kopers langetermijnmiddelen heeft een Domino-effect, motiveren detailhandelaren te abonneren op onze portal-site. Abonnementen zijn kosten gebaseerde, tegen een betaalbaar tarief.

 ![][7] 

Na de ondertekening voor een abonnement heeft een detailhandelaar op het bestaande profiel (gemaakt in eerste instantie door ons ingekochte gegevens), bijgewerkt met aanvullende gegevens over promoties, aanbevolen merken of aankondigingen. Interne mogelijkheden, zoals talenkennis, valuta's worden geaccepteerd, tax-free winkelen zijn zelf gemeld voor het aantrekken van kopers die op zoek zijn naar deze voorzieningen beter.

## <a name="who-we-are"></a>Wie zijn we

Mijn naam is Thomas Segato (Microsoft Consulting) en ik met Jesper Boelling, Lead Developer bij WhatToPedia voor het ontwerpen van de oplossing heeft gewerkt. 

WhatToPedia is een opstarten, test marketing van de nieuwe portal bedrijf in Zweden, waar de meeste van de detailhandelaren 60.000 bakstenen en mortel MKB (midden- en kleinbedrijf) zijn. Omdat we weten dat meerdere talen spreekt en meerdere valuta's wordt een persoon winkelen in Europa, bouwen we oplossingen die geschikt voor een meertalige shopper. We nodig en gevonden, een zoekmachine die onze meertalige vereisten in Azure Search ondersteunt.

Azure Search is een spel-wisselaar voor ons project. Voor de beschikbaarheid van Azure zoeken we opgebruikt aanzienlijke energie met behulp van de kinks van onze eigen zoekmachine te bouwen. Met Azure zoeken als een on line service de grootste drempel voor technische en administratieve verwijderd uit onze oplossing, die bedoeld ophalen op de markt sneller en met een krachtigere zoekfunctie.  

## <a name="how-we-did-it"></a>Hoe we deden

Onze visie is een complete infrastructuur op basis van alleen een cloud-services bouwen. Microsoft is gekozen als de strategische platform, omdat de provider die de nodige aangeboden services (voor samenwerking en ontwikkeling), schalen op vraag en betaalbare prijzen.
 
### <a name="high-level-components"></a>High-level componenten

We een bedrijf, niet alleen een site gebouwd. Ondersteuning van de gehele inspanning vereist een volledige reeks hulpprogramma's en toepassingen. We vastgesteld Visual Studio en Visual Studio Team Services voor ontwikkeling, Online Team Foundation Service (TFS) voor het besturingselement en het beheer van scrum, Office 365 voor communicatie en samenwerking, en natuurlijk Microsoft Azure voor alle bewerkingen die betrekking hebben op site- en opslag. Met Visual Studio geleverd de IDE directe ingericht naar Azure, met integratie met TFS Online bieden een extra productiviteit verhogen.

Het onderstaande diagram illustreert de high-level componenten die worden gebruikt in de infrastructuur van WhatToPedia.

   ![][8]

### <a name="how-we-use-microsoft-azure"></a>Hoe gebruiken we Microsoft Azure

De groene vakken in het vorige diagram bekijkt, ziet u dat de WhatToPedia-oplossing is gebaseerd op deze services:

- [Azure zoeken](https://azure.microsoft.com/services/search/)
- [Azure Websites met behulp van MVC 4](https://azure.microsoft.com/services/websites/)
- [Azure WebJobs voor geplande taken](../app-service-web/websites-webjobs-resources.md)
- [Azure SQL-Database](https://azure.microsoft.com/services/sql-database/)
- [Azure BLOB-opslag](https://azure.microsoft.com/services/storage/)
- [Levering van e-SendGrid](https://azure.microsoft.com/marketplace/partners/sendgrid/sendgrid-azure/)

De zeer kern van de oplossing is gegevens en zoeken. De stroom van gegevens van de provider van de leverancier naar de klant wordt hierna geïllustreerd:

  ![][9]

Primaire gegevensopslag is de leverancier en de accounting-gegevens in Azure SQL-Database. Dit is de eerste dataset plus leverancier-specifieke gegevens over de tijd toegevoegd. We maken gebruik van een WebJob Azure updates van SQL-Database naar de corpus zoeken in Azure Search boeken.

### <a name="presentation-layer"></a>Presentatie laag

De portal is een Website Azure geïmplementeerd in 4 MVC en [Twitter Bootstrap](http://en.wikipedia.org/wiki/Bootstrap_%28front-end_framework%29). Wij hebben gekozen voor MVC omdat het een veel schonere aanpak naar HTML dan de ontwikkeling van ASP.NET-formulieren zijn gebaseerd biedt. Om te voorkomen dat apps voor meerdere apparaten maken en onderhouden van meerdere mobiele platforms, is Twitter Bootstrap gekozen voor de ondersteuning van alle apparaten en platforms.

### <a name="authentication-permissions-and-sensitive-data"></a>Verificatie, machtigingen en gevoelige gegevens

Kopers surfen anoniem op de site. Als zodanig, er zijn geen vereisten voor kopers voor aanmelding, noch doen wij slaan geen consumentgegevens. 

Detailhandelaren zijn een ander verhaal. Wij slaan hier publieke profielinformatie, informatie over facturering en media-inhoud die u wilt weergeven op de site. Elke leverancier die geabonneerd op de site krijgt een gebruikersaanmelding gebruikt voor het verifiëren van de gebruiker voordat u updates van de abonnee-profiel.  Alle gegevens veilig opgeslagen in Azure SQL-Database en Azure BLOB-opslag.
Wij geopteerd voor een verificatiemodel gebaseerd op formulieren gebaseerde verificatie van .NET. Wij hebben gekozen voor deze aanpak voor de eenvoud; We hebben nodig niet de rollen, UI ondersteuning en andere overbodige functies die tot andere benaderingen behoren. 

Detailhandelaren ziet alleen de gegevens waartoe deze behoort, zodat wij een leverancier-ID voor elke leverancier die later wordt gebruikt op alle lezen en schrijven van operaties met betrekking tot leverancier-specifieke gegevens. Met deze aanpak vinden we we is niet nodig machtigingen toekennen aan afzonderlijke detailhandelaren. Detailhandelaren alle interactie met het systeem onder een enkele databaserol het leverancier-ID als onze gegevens isolatie-techniek.

Omdat ons bedrijf is alles over de downstream effecten (meer zakelijke verkeer aan detailhandelaren, stimulans om te adverteren en abonneren maken), we de lijn tekenen op aankopen via het web te verwerken. Als zodanig vindt u niet op onze site, die onze beveiligingsvereisten vereenvoudigt uw winkelwagen. 

Een andere vereenvoudiging zijn die we werkzaam was voor het uitbesteden van onze te betalen factuur- en -bewerkingen. Door klant betaling routeringsgegevens rechtstreeks aan een derde partij ([SveaWebPay](http://www.sveawebpay.se/)), beperken we de risico's koppelen aan het opslaan en beschermen van vertrouwelijke gegevens in onze opgeslagen gegevens. 

### <a name="search-engine"></a>Zoekmachine

De kern van onze oplossing is de zoekmachine gebaseerd op Azure Search-service. In eerste instantie we een aangepaste zoekmachine gebouwd tijdens dit proces hebben we ons gerealiseerd dat de complexiteit en inspanning is zeer hoog inderdaad maar die ons overweeg dan alternatieven voor gevraagd. 

Belangrijkste functies zijn het meest belangrijk voor ons opgenomen:

- Filters
- Beperkt navigatie
- Verhoging van de resultaten
- Paginering van AJAX

Zoeken op internet die ons op de volgende video die geïnspireerd ons probeer Azure Search: [Grondige Kennismaking met TechEd Europe](http://channel9.msdn.com/events/TechEd/Europe/2014/DBI-B410) 

Na het bekijken van de video is klaar om te bouwen, een prototype op basis van wat we zagen. Omdat we al een gegevensmodel in MVC hadden, is het prototype maken eenvoudig omdat de gegevens bevatten een doorzoekbare termen en we hadden al de vereisten voor de manier waarop we wilden facet, sorteren en filteren van gegevens uitgewerkt. 

Dit is hoe we het prototype gemaakt.

**Azure Search-Service configureren**

1. Aanmelden bij de klassieke Portal Azure en de Search-service toegevoegd aan onze abonnement. We gebruiken de gedeelde versie (gratis met onze abonnement).
2. Een index maken. Voor het prototype, wij de portal UI gebruikt de score profielen maken en definiëren van de velden met zoekcriteria. Onze score profiel is gebaseerd op locatiegegevens: land | stad | adres (Zie: score profielen toevoegen).
3. Kopieer de URL van de service en beheer-api-sleutel tot onze configuratiebestanden. Deze sleutel wordt op de pagina zoeken in de portal en wordt gebruikt voor verificatie bij de service.
    
**Ontwikkelen van een taak van de Search Indexer – Windows-Console**

1. Alle leveranciers van de database lezen.
2. De Azure Search Service API wederverkopers één voor één uploaden aanroepen (Zie: http://msdn.microsoft.com/library/azure/dn798930.aspx).
3. Een eigenschap instellen in de database reseller voor incrementele indexering wordt geïndexeerd. Wij dat hebben gedaan door het toevoegen van een veld 'indexering' waarin de status van de index van elk profiel (geïndexeerd of niet). 

Zie de bijlage voor het stukje code dat wordt gemaakt van de indexer-taak.

**Een webportal zoeken – MVC ontwikkelen**

1. Bel Azure Search-Service als u alle documenten vanuit zoeken (Zie: http://msdn.microsoft.com/library/azure/dn798927.aspx)
2. Uittreksel uit het antwoord van de service zoeken na (via json.net http://james.newtonking.com/json)
   - Resultaten
   - Facetten
   - Resultaat telt
   - Ontwikkelen van een gebruikersinterface voor het weergeven van zoekresultaten, facetten en tellingen (we hadden al deze).

Dit is de code die we hebben gebruikt om de resultaten van Azure zoeken:

    string requestUrl = 
    string.Format("https://{0}.search.windows.net/indexes/profiles/docs?searchMode=all&$count=true&search={1}&facet=city,count:20&facet=category&$top=10&$skip={2}&api-version=2014-07-31-Preview{3}", Config.SearchServiceName, EscapeODataString(q), skip, filter);
      var response = client.GetAsync(requestUrl).Result; //  + Uri.EscapeDataString("hennes")
      response.EnsureSuccessStatusCode();
     dynamic json = JsonConvert.DeserializeObject(response.Content.ReadAsStringAsync().Result);

**Waarbij de locatie**

De belangrijkste eis om te controleren of in het model opgenomen waarschijnlijk een zoekwoord locatie toe te voegen aan de query. Onze portal die als een gebruiker een naam invoert in de zoekopdracht query, die noodzakelijk is de wederverkopers in bepaalde stad zou hoger dan met het sleutelwoord plaats in de beschrijving van de wederverkopers rangschikken. Voor deze vereiste, hebben we een score profiel gebruikt het plaatsveld hoger is dan de andere velden rangschikken.

**Ondersteuning van meerdere talen**

We nodig om de juiste zoekresultaten worden weergegeven in de juiste talen en hebben een optie voor het zoeken naar dezelfde resultaten in verschillende talen. De twee zijden voor dit probleem zijn: 

- Woorden zoeken in meerdere talen
- Zoekresultaten weergeven in de juiste taal

We de presentatie deel opgelost door het toevoegen van een document voor elke taal met gelokaliseerde tekst en een eigenschap met de taal. Wanneer een gebruiker een zoekterm invoert we gebruiker `$filter` expressies waarop u wilt filteren op de taal van de gebruiker heeft gekozen.

Elk van de documenten heeft een verborgen eigenschap met de naam 'cities', gebaseerd op het collectietype. Deze eigenschap bevat namen van steden in alle talen, waardoor de gebruiker om te zoeken in meerdere talen.

###<a name="data-storage"></a>Opslag van gegevens

Alle gegevens (profiel, abonnement en accounting) wordt opgeslagen in een SQL-Database. Alle mediabestanden worden in Azure BLOB opslag, met inbegrip van afbeeldingen en video's die worden geleverd door de detailhandelaar opgeslagen. Afzonderlijk BLOB-opslag geïsoleerd van de gevolgen van het uploaden van bestanden. bestanden worden nooit gezamenlijk mingled met de website, zodat we niet hoeven te bouwen van de site wanneer we bestanden toevoegen.

Een belangrijk voordeel van onze Opslagontwerp is dat meerdere ontwikkelaars een enkele ontwikkeling opslag kunnen delen. Een van de vereisten voor het WhatToPedia-project was een ontwikkelomgeving binnen 15 minuten, met inbegrip van de leverancier, gegevens, afbeeldingen en video's maken. Door de meest recente gegevens van TFS Online, uitvoeren van een SQL-script en uitvoeren van de taak importeren, een complete omgeving kan worden bevond zich omhoog in geen tijd helemaal. Deze oefening verbetert ook de gefaseerde installatie proces.

###<a name="webjobs"></a>WebJobs

Azure WebJobs gebruiken we gegevens bij te werken aan de index. Door een search indexer taak maakt, is de indexing deel zeer eenvoudig te integreren in onze oplossing. De enige codewijziging we is aangepast aan de indexering taak was ingesteld op een `Indexed` veld naar onze gegevensmodel geven aan de status van de index. Wanneer een nieuw profiel wordt toegevoegd of bijgewerkt, de `Indexed` veld is ingesteld op false. Hetzelfde geldt als de leverancier zijn of haar profielgegevens via de portal verandert.  

De taak wordt gezocht naar alle rijen met `Indexed` ingesteld op false. Wanneer de rij wordt gevonden, het document wordt geboekt naar Azure Search, en vervolgens de `Indexed` is ingesteld op true. We hebben plannen voor het toevoegen en bijwerken van gegevens omdat de zoekservice Azure daadwerkelijk voor dit zorgt. Als u een document dat al aanwezig is, wordt de service automatisch een update doen.

Alle projecten van het web zijn ontwikkeld als consoletoepassingen die kunnen worden geüpload naar Azure websites als ZIP-bestanden zijn uitgepakt en wordt gepland.

De taak is gepland als een geplande taken om de 5 minuten. Wij berekend dat de service neemt ongeveer drie minuten voor het uploaden van documenten, 3000, die was binnen onze behoeften. 

> [AZURE.NOTE] Er is een prototype indexeerfunctie functie die onlangs is geïntroduceerd in Azure zoeken. Deze functie is te laat voor ons om deze te gebruiken in onze eerste versie wordt geleverd, maar blijkt het probleem opgelost door dezelfde dat we onze taak indexeerfunctie, gebruikt namelijk gegevens laden bewerkingen kunnen worden geautomatiseerd.


###<a name="backup-strategy"></a>Back-upstrategie

We ontworpen een meerlagige back-upstrategie om te herstellen van een aantal scenario's uit een catastrofale storing, tot herstel van een afzonderlijke transactie. De activa te beschermen hebben drie soorten gegevens (website, gegevens en mediabestanden). 

Ten eerste doordat de broncode van de website op TFS Online weten we als de site zich begeeft, kunnen we het opbouwen door opnieuw te publiceren van TFS. 

Abonneeservergegevens in Azure SQL-Database is de meest gevoelige activa. Wij terug dit met behulp van de ingebouwde functie (Zie [Azure SQL Database back-up en terugzetten](http://msdn.microsoft.com/library/azure/jj650016.aspx)). Het back-upschema is volledige databaseback-up eenmaal per week, differentiële back-ups eenmaal per dag en transactielogboekback-ups om de 5 minuten.  Gezien de omvang van de gegevens, is deze oplossing meer dan voldoende voor ons direct en verwachte gegevensvolumes.

Ten derde, we afbeeldings- en videobestanden bestanden opslaan in Azure BLOB-opslag. We nog steeds beoordeelt de ultieme back-upplan voor deze gegevens Cloudberry Explorer voor Azure als mogelijke oplossing te overwegen. Nu we een WebJob gebruiken om afbeeldingen en video's kopiëren naar een andere locatie.

##<a name="what-we-learned"></a>Wat hebt u geleerd

Omdat we al gegevens, is het eenvoudig vast te stellen bewijs van concept. Binnen uur hadden we een prototype met facetten en items van het wisselbestand, Rank profielen zoekresultaten. De zoekresultaten zijn dus precies dat we besloten om te verwijderen van de filters aan de klant aangeboden. 

De grootste verrassing voor ons was hoe snel we Azure Search kan leren en hoeveel wij terug. Letterlijk, gevestigde we bewijs van concept in een paar uur (Zie de onderstaande opmerking), 500 regels code vervangen door 3 regels code in de front-end-toepassing (plus een nieuwe WebJob), en betere resultaten. 

Onze code geïmplementeerd voorheen paginering, aantallen en andere problemen die standaard als u worden wilt zoeken. Azure zoekopdracht zijn de resultaten we terug krijgen de treffers zoeken facetten, gegevens, telt--alle dingen die we nodig en hoeven onszelf leveren zijn van het wisselbestand. Deze prestatieverbetering en ingebouwde beperkt navigatie die we in onze oorspronkelijke oplossing hebben ook opgenomen.

De grootste uitdaging tijdens uitvoering is dat deze Preview-versie was en moeilijk vinden van informatie en ervaringen gedeeld is. Nadat we een paar punten verbonden, wij heeft geconstateerd dat met Azure Search Service heel eenvoudig omdat de REST API en JSON-gegevensindeling. Wij het kader kan bellen rechtstreeks vanuit de meeste open source Plug-ins zoals JQuery JSON.Net en hulpmiddelen zoals Fiddler kan worden gebruikt voor snelle experimenten en foutopsporing. 

> [AZURE.NOTE] Behalve dat de gegevens domainprep, geholpen die we bouwen van het prototype al begrepen hoe technologie werkt, waardoor ons productiever en meer beleggingsfaciliteit van de ingebouwde functies zoeken. Als u aangeeft hoeveel van de voor de bouw van search query, beperkt navigatie, filters, enz., u kunt verwachten prototype maken duurt langer. 

###<a name="controlling-facets-in-the-search-presentation-page"></a>Facetten op de presentatiepagina besturen

Een van onze geleerde lessen tijdens het bewijs van concept was facetten op voorhand zorgvuldig plannen. Na het laden van een grote hoeveelheid gegevens in de oplossing, zagen we dat het grote aantal facetten te hoog is voor de gebruikers. 

We dit opgelost door de parameter facet aantal beperkingen. De parameter aantal legt een vaste limiet op het aantal facetten aan de gebruiker geretourneerd. Een koppeling met een bespreking van de parameter aantal vindt u [hier](search-faceted-navigation.md).

###<a name="webjobs-for-scheduling-tasks"></a>WebJobs voor het plannen van taken

Azure Search is niet het alleen prettig verrassing voor ons. Hebben we ontdekt dat we onze activiteiten gegevens laden Azure zoeken automatiseren met behulp van WebJobs sterk superieur was aan onze vorige benadering, die tot gevolg dat met behulp van een speciale VM Windows Scheduler, uitgevoerd met geplande taken voor het bijwerken van de zoekindex. WebJobs eenvoudiger te configureren en foutopsporing en natuurlijk veel eenvoudiger is goedkoper dan hoeven betalen voor een specifieke VM.

###<a name="azure-blob-storage-explorer-for-updating-images"></a>Azure BLOB Storage Explorer-installatiekopieën bijwerken

Wij vinden dat heel handig zijn bij het beheren van afbeeldingen en video-updates naar de site met [Azure BLOB Storage Explorer](https://azurestorageexplorer.codeplex.com/) (beschikbaar op codeplex). We gebruiken als een hulpmiddel voor ontwikkelaars handmatig bijwerken van afbeeldingen en video's die deel van onze belangrijkste site uitmaken. We geconstateerd dat het flexibeler is dan het implementeren van wijzigingen op de portal en elimineert een herhaling van de test voltooid wanneer we nodig voor het bijwerken van een installatiekopie. 

##<a name="a-few-final-words"></a>Een paar laatste woorden

Dankzij de geweldige mensen op WhatToPedia voor ons toestemming te delen van hun verhaal!  

We hopen dat u deze casestudy nuttig gevonden. Als u gaat op te zoeken in Azure, aanbevelen ik een aantal bronnen snel langs:

- [MSDN-forum gewijd aan Azure zoeken](https://social.msdn.microsoft.com/forums/azure/home?forum=azuresearch)
- [StackOverflow heeft ook een tag](http://stackoverflow.com/questions/tagged/azure-search)
- [Documentatiepagina op Azure.com](https://azure.microsoft.com/documentation/services/search/)
- [Azure Search documentatie op MSDN](http://msdn.microsoft.com/library/azure/dn798933.aspx)


##<a name="appendix-search-indexer-webjob"></a>Aanhangsel: Search Indexer WebJob

De volgende code wordt de indexering vermeld in de sectie over het bouwen van het prototype.

        static void Main(string[] args)
        {
            int success = 0;
            int errors = 0;

            Log.Write("Starting job","", System.Diagnostics.TraceLevel.Info);

            var serviceName = ConfigurationManager.AppSettings["SearchServiceName"];
            var serviceKey = ConfigurationManager.AppSettings["SearchServiceKey"];

            HttpClient client = new HttpClient();
            client.DefaultRequestHeaders.Add("api-key", serviceKey);

            var db = new DB(Config.ConectionString);

            var recreateIndex = false;
            Boolean.TryParse(ConfigurationManager.AppSettings["RecreateIndex"], out recreateIndex);

            if(recreateIndex)
            {
                Log.Write("Recreating index and set all to no index", "", System.Diagnostics.TraceLevel.Info);
                db.SetAllToNotIndexed();
                RecreateIndex(serviceName, client);
            }            
            
            var profiles = db.Profiles.Where(p=>!p.Indexed).ToList();

            Log.Write(string.Format("Indexing {0} profiles",profiles.Count),"", System.Diagnostics.TraceLevel.Info);

            var cities = db.Cities.ToList();
            var categories = db.Tags.Where(p=>p.ParentId==null).ToList();            

            foreach (var profile in profiles)
            {
                Log.Write(string.Format("Indexing profile {0}", profile.Name),"",profile.ProfileId,0,System.Diagnostics.TraceLevel.Verbose);

                try
                {
                    var city = cities.Where(p => p.CityId == profile.CityId);
                    var category = categories.Where(p => p.TagId == profile.CategoryId);

                    var cityse = city.Where(p => p.Lang == "se").FirstOrDefault();
                    var cityen = city.Where(p => p.Lang == "en").FirstOrDefault();
                    var categoryse = category.Where(p => p.Lang == "se").FirstOrDefault();
                    var categoryen = category.Where(p => p.Lang == "en").FirstOrDefault();

                    var citysename = cityse == null ? "" : cityse.Name;
                    var cityenname = cityen == null ? "" : cityen.Name;
                    var categorysename = categoryse == null ? "" : categoryse.Name;
                    var categoryenname = categoryen == null ? "" : categoryen.Name;

                    var tags = db.GetTagsFromProfile(profile.ProfileId);

                    var batch = new
                    {
                        value = new[] 
                    { 
                        new 
                        { 
                            id = profile.ProfileId.ToString()+"_en",
                            profileid = profile.ProfileId.ToString(),
                            city = cityenname,
                            category = categoryenname,
                            address = profile.Adress1,
                            email = profile.Email,
                            name = profile.Name,
                            lang = "en",
                            brands = profile.Brands,
                            descen=profile.DescEn,
                            descse=profile.DescSe,
                            orgnumber=profile.OrgNumber,
                            phone=profile.Phone,
                            zip=profile.Zip,
                            cities = city.Select(p=>p.Name).ToArray(),
                            categories = category.Select(p=>p.Name).ToArray(),
                            cityid = profile.CityId.ToString(),
                            tags=tags.ToArray()
                        },
                        new 
                        { 
                            id = profile.ProfileId.ToString()+"_se",
                            profileid = profile.ProfileId.ToString(),
                            city = citysename,
                            category = categorysename,
                            address = profile.Adress1,
                            email = profile.Email,
                            name = profile.Name,
                            lang = "se",
                            brands = profile.Brands,
                            descen=profile.DescEn,
                            descse=profile.DescSe,
                            orgnumber=profile.OrgNumber,
                            phone=profile.Phone,
                            zip=profile.Zip,
                            cities = city.Select(p=>p.Name).ToArray(),
                            categories = category.Select(p=>p.Name).ToArray(),
                            cityid = profile.CityId.ToString(),
                            tags=tags.ToArray()
                        }
                    },
                    };

                    var response = client.PostAsync("https://" + serviceName + ".search.windows.net/indexes/profiles/docs/index?api-version=2014-10-20-Preview", new StringContent(JsonConvert.SerializeObject(batch), Encoding.UTF8, "application/json")).Result;
                    response.EnsureSuccessStatusCode();

                    db.Entry(profile).State = System.Data.Entity.EntityState.Modified;
                    profile.Indexed = true;
                    db.SaveChanges();
                    success++;
                }
                catch(Exception ex)
                {
                    Log.Write("Error indexing profile", ex.Message, profile.ProfileId, 0, System.Diagnostics.TraceLevel.Verbose);
                    errors++;
                }
            }
            if(errors > 0)
            {
                Log.Write(string.Format("Job ended success ({0}), errors ({1})", success, errors), "", System.Diagnostics.TraceLevel.Error);
            }
            else
            {
                Log.Write(string.Format("Job ended success ({0}), errors ({1})", success, errors), "", System.Diagnostics.TraceLevel.Info);
            }
            
        }

        static void RecreateIndex( string ServiceName, HttpClient client)
        {
            var index = new
            {
                name = "profiles",
                fields = new[] 
                { 
                    new { name = "id",              type = "Edm.String",         key = true,  searchable = false, filterable = false, sortable = false, facetable = false, retrievable = true,  suggestions = false },
                    new { name = "profileid",       type = "Edm.String",         key = false,  searchable = false, filterable = false, sortable = false, facetable = false, retrievable = true,  suggestions = false },
                    new { name = "cityid",          type = "Edm.String",         key = false,  searchable = false, filterable = false, sortable = false, facetable = false, retrievable = true,  suggestions = false },
                    new { name = "city",            type = "Edm.String",         key = false, searchable = true,  filterable = true, sortable = true,  facetable = true, retrievable = true,  suggestions = true  },
                    new { name = "category",        type = "Edm.String",         key = false, searchable = true,  filterable = true, sortable = false, facetable = true, retrievable = true,  suggestions = false  },
                    new { name = "address",         type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true,  suggestions = false },
                    new { name = "email",           type = "Edm.String",         key = false, searchable = true,  filterable = false, sortable = true, facetable = false, retrievable = true,  suggestions = false },
                    new { name = "name",            type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true, suggestions = true },
                    new { name = "lang",            type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = false,  facetable = false,  retrievable = true, suggestions = false },
                    new { name = "brands",          type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true,  suggestions = false },
                    new { name = "descen",          type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true,  suggestions = false },
                    new { name = "descse",          type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true,  suggestions = false },
                    new { name = "orgnumber",       type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true,  suggestions = false },
                    new { name = "phone",           type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true,  suggestions = false },
                    new { name = "zip",             type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true,  suggestions = false },
                    new { name = "cities",          type = "Collection(Edm.String)",         key = false, searchable = true,  filterable = false,  sortable = false,  facetable = false,  retrievable = false, suggestions = false },
                   new { name = "categories",      type = "Collection(Edm.String)",         key = false, searchable = true,  filterable = false,  sortable = false,  facetable = false,  retrievable = false, suggestions = false },
                    new { name = "tags",            type = "Collection(Edm.String)",         key = false, searchable = true,  filterable = false,  sortable = false,  facetable = false,  retrievable = false, suggestions = false }
                    
                }
            };

            var url = "https://" + ServiceName + ".search.windows.net/indexes/?api-version=2014-10-20-Preview";

            var deleteUrl = "https://" + ServiceName + ".search.windows.net/indexes/profiles?api-version=2014-10-20-Preview";

            try
            {
                var deleteResponseIndex = client.DeleteAsync(deleteUrl).Result;
                deleteResponseIndex.EnsureSuccessStatusCode();
            }
            catch (Exception ex)
            {

            }

            var responseIndex = client.PostAsync(url, new StringContent(JsonConvert.SerializeObject(index), Encoding.UTF8, "application/json")).Result;
            responseIndex.EnsureSuccessStatusCode();            
          


<!--Anchors-->
[Subheading 1]: #subheading-1
[Subheading 2]: #subheading-2
[Subheading 3]: #subheading-3
[Subheading 4]: #subheading-4
[Subheading 5]: #subheading-5
[Next steps]: #next-steps

<!--Image references-->
[6]: ./media/search-dev-case-study-whattopedia/lightbulb.png
[7]: ./media/search-dev-case-study-whattopedia/WhatToPedia-Search-Bageri.png
[8]: ./media/search-dev-case-study-whattopedia/WhatToPedia-Stack.png
[9]: ./media/search-dev-case-study-whattopedia/WhatToPedia-Archicture.png


<!--Link references-->
[Link 1 to another azure.microsoft.com documentation topic]: ../virtual-machines-windows-hero-tutorial.md
[Link 2 to another azure.microsoft.com documentation topic]: ../web-sites-custom-domain-name.md
[Link 3 to another azure.microsoft.com documentation topic]: ../storage-whatis-account.md
 
