

#<a name="metadata-for-azure-technical-articles"></a>Metagegevens voor Azure technische artikelen

Alle Azure technische artikelen bevatten twee metagegevens secties - een sectie met eigenschappen en een tags-sectie. De sectie-eigenschappen kunt u sommige website automatisering en SEO spul, terwijl het gedeelte labels veel inhoud interne rapportage kunt. Beide gedeelten zijn vereist.

- [Syntaxis]
- [Gebruik]
- [Kenmerken en waarden voor de sectie-eigenschappen]
- [Kenmerken en waarden voor de sectie labels]

##<a name="syntax"></a>Syntaxis

De sectie-eigenschappen gebruikt de volgende syntaxis:

    <properties
       pageTitle="Page title that displays in search results and the browser tab | Microsoft Azure"
       description="Article description that will be displayed on landing pages and in most search results"
       services="service-name"
       documentationCenter="dev-center-name"
       authors="GitHub-alias-of-only-one-author"
       manager="manager-alias"
       editor=""
       tags="optional"
       keywords="For use by SEO champs only. Separate terms with commas. Check with your SEO champ before you change content in this article containing these terms."/>

De sectie tags gebruikt de volgende syntaxis:

    <tags
       ms.service="required"
       ms.devlang="may be required"
       ms.topic="article"
       ms.tgt_pltfrm="may be required"
       ms.workload="na"
       ms.date="mm/dd/yyyy"
       ms.author="Your MSFT alias or your full email address;semicolon separates two or more"/>

##<a name="usage"></a>Gebruik

- De naam en kenmerken zijn hoofdlettergevoelig.
- De <properties> sectie moet de eerste regel van uw bestand.
- Laat een lege regel na elke metagegevenssectie en voor de titel van de pagina om ervoor te zorgen dat de titel is goed geconverteerd naar HTML tijdens het publicatieproces.

## <a name="attributes-and-values-for-the-properties-section"></a>Kenmerken en waarden voor de sectie-eigenschappen

![](./media/article-metadata/checkmark-small.png)**Paginatitel**: vereist; belangrijk voor SEO. De tekst voor dit kenmerk weergegeven in het tabblad browser en als de titel in een zoekresultaat. 55-60 tekens inclusief spaties en met inbegrip van de site-id *| Microsoft Azure* (getypeerd als: pipe ruimte Microsoft Azure ruimte).  De paginatitel moet verschillen van de H1.

![](./media/article-metadata/checkmark-small.png)**Beschrijving**: vereist; belangrijk voor Zoekmachineoptimalisatie (relevantie) en functionaliteit van de site. De beschrijving moet ten minste 125 tekens lang tot 155 tekens maximaal inclusief spaties. Omschrijving van het doel van de inhoud zodat klanten weet of om deze te kiezen uit een lijst met zoekresultaten. De waarde is:

- Deze tekst kan worden weergegeven als de omschrijving of abstracte lid in de zoekresultaten op Google.
- Deze tekst wordt weergegeven in [de resultaten van het artikel-index](https://azure.microsoft.com/documentation/articles/).

![](./media/article-metadata/checkmark-small.png)**Services**: vereist voor artikelen die betrekking op een service hebben. Deze waarde is ofter de witruimte' service' genoemd. Een overzicht van alle toepasselijke services, gescheiden door komma's. De broodkruimels navigatie van de pagina en de linkernavigatiebalk weergegeven met de pagina die zorgen voor de eerste service die u aanbiedt.

In de artikelen die zowel een services-waarde en een waarde van documentationCenter opgeeft, wordt de waarde van de diensten de ' breadcrumb ' station. Als u meer waarden die u lijst als labels in het gepubliceerde artikel weergegeven. Waarden:

- Active directory
- Active-directory-b2c
- Active-directory-ds
- App-service\api
- API-management
- App-service.
- App-servic\mobile
- App-service\web
- App-service\logic
- toepassing-gateway
- toepassing inzichten
- automatisering
- Azure-portal
- Azure bronnenbeheerder
- Azure-stack
- back-up
- batch
- beste praktijken
- BizTalk-services
- cache
- CDN
- cloud-services
- catalogus gegevens
- Data factory
- lake-gegevens-analytics
- gegevens-lake-store
- devtest lab
- DNS
- documentdb
- expressroute
- gebeurtenis-hubs
- hdinsight
- IOT-hub
- sleutel kluis
- verdeling van de belasting
- machine learning
- Marketplace
- Media-services
- Mobile engagement
- mobiele services
- meerdere factor verificatie
- melding hubs
- operationele inzichten
- bewerkingen-management-suite
- powerapps
- herstel manager
- cache bestand Vgx.
- RemoteApp
- Information Rights management
- Scheduler
- zoeken
- Security center
- Service-bus
- Service-fabric
- site herstellen
- SQL-database
- SQL-datawarehouse
- SQL reporting
- opslag
- winkel
- storsimple
- Stream-analytics
- verkeer-manager
- virtuele machines
- virtueel netwerk
- Visual studio online
- VPN-gateway
- websites

![](./media/article-metadata/checkmark-small.png)**documentationCenter**: vereist voor dev-georiënteerde artikelen beste Topaanbieding via een dev center. De enkele dev center of de taal die voor het artikel opgeven. De waarde die u opgeeft, wordt de broodkruimels navigatie voor de pagina station. In de artikelen die zowel een services-waarde en een waarde van documentationCenter opgeeft, wordt de waarde van de diensten de ' breadcrumb ' station. Waarden:

- **.NET**
- **nodejs**
- **Java**
- **PHP**
- **Python**
- **Ruby**
- **mobiel**: afgeschaft. Vervangen door specifieke mobiel platform.
- **IOS**: Verifing deze nieuwe waarde
- **android**: controle van deze nieuwe waarde
- **Windows**: controle van deze nieuwe waarde
- **xamarin**: controle van deze nieuwe waarde

![](./media/article-metadata/checkmark-small.png)**auteurs**: slechts één waarde vereist. Een overzicht van de GitHub-account voor de primaire auteur of artikel MKB. Dit kenmerk wordt de naamregel stations op gepubliceerd artikel. Een overzicht van slechts één, ondanks het meervoud van het kenmerk.

![](./media/article-metadata/checkmark-small.png)**Manager**: als u een medewerker van Microsoft zijn vereist. Een overzicht van de e-mailalias van de publishing Inhoudsbeheerder voor het gebied van technologie. Als u een medewerker van de Gemeenschap, het kenmerk zonder deze leeg zodat we het kunnen invullen.

![](./media/article-metadata/checkmark-small.png)**Editor**: niet gebruikt. Gebruik deze niet voor andere doeleinden.

![](./media/article-metadata/checkmark-small.png)**tags**: optioneel. Alleen opnemen als u wilt een koppeling in het artikel ' breadcrumb ' naar de indexpagina artikel (http://azure.microsoft.com/documentation/articles/) inschakelen om een prefiltered lijst met artikelen die overeenkomen met een van de erkende waarden. Deze waarden zijn bedoeld als een manier bieden om te groeperen inhoud wanneer u de groepering van de inhoud is geen specifieke service. Deze tags kunnen ook bieden dat labeling geeft aan dat de technologiestack die het artikel is van toepassing op. Deze waarde **niet** ondersteuning voor vrije vorm-codes of hashtags; de afsluitcodes moeten zijn ingeschakeld op de site. U kunt meerdere waarden van tags aan een artikel, gescheiden door komma's geven. De goedgekeurde waarden zijn:

  - architectuur
  - Azure bronnenbeheerder
  - Azure servicebeheer
  - facturering
  - MySQL

![](./media/article-metadata/checkmark-small.png)**trefwoorden**: optioneel. Voor gebruik door alleen Zoekmachineoptimalisatie champs. Scheid termen met komma's. **Neem contact op met uw SEO soepel verlopen voordat u wijzigen of verwijderen van de inhoud van dit artikel met deze voorwaarden.** Dit kenmerk trefwoorden de Zoekmachineoptimalisatie soepel verlopen is gericht en houdt ter verbetering van de rang van de zoekopdracht wordt vastgelegd. De trefwoorden worden niet weergegeven in de gepubliceerde HTML. Validatie is niet vereist voor dit kenmerk.

## <a name="attributes-and-values-for-the-tags-section"></a>Kenmerken en waarden voor de sectie labels

![](./media/article-metadata/checkmark-small.png)**MS.service**: vereist. Hiermee geeft u de Azure service, hulpprogramma of functie die het artikel is van toepassing op. Één waarde per pagina.

 Als een pagina meerdere diensten van toepassing is, kiest u de service waarvoor het meest rechtstreeks van toepassing is; bijvoorbeeld moet een artikel met een app die worden gehost op websites demonstratie van de functionaliteit van de Bus-Service de **service bus** waarde, in plaats van **websites**. Als een pagina eveneens voor meerdere services geldt, kies **meerdere**. Als een pagina niet van toepassing op alle services (dit is zeldzaam), kiest u **NA**.

 - **Active directory**
 - **Active-directory-b2c**
 - **Active-directory-ds**
 - **API-management**
 - **App-service**: alleen van toepassing op de algemene concepten illustreren van App-Service
 - **App-service-api**
 - **App-service-logica**
 - **App-service-mobiel**
 - **App-service-web**
 - **toepassing inzichten**
 - **toepassing-gateway**
 - **automatisering**
 - **Azure bronnenbeheerder**
 - **Azure-beveiliging**
 - **Azure-stack**
 - **back-up**
 - **batch**
 - **beste praktijken**
 - **BizTalk-services**
 - **facturering**
 - **cache**
 - **CDN**
 - **cloud-services**
 - **catalogus gegevens**
 - **gegevens-lake-store**
 - **lake-gegevens-analytics**
 - **devtest lab**
 - **expressroute**
 - **hdinsight**
 - **Internet van dingen**
 - **IOT-hub**
 - **sleutel kluis**
 - **machine learning**
 - **Marketplace**: artikelen over de Azure marketplace
 - **Media-services**
 - **Mobile engagement**
 - **mobiele services**
 - **meerdere factor verificatie**
 - **meerdere**: de pagina gelijkelijk van toepassing is op meerdere services
 - **NB**: de pagina is niet van toepassing op alle services (zeldzaam)
 - **melding hubs**
 - **operationele inzichten**
 - **powerapps**
 - **herstel manager**
 - **cache bestand Vgx.**
 - **RemoteApp**
 - **Information Rights management**
 - **Scheduler**
 - **Security center**
 - **Service-bus**
 - **Service-fabric**
 - **site-recovery**: voorheen recovery services
 - **SQL-database**
 - **SQL-datawarehouse**
 - **SQL reporting**
 - **opslag**
 - **Opslaan**: artikelen over de services die beschikbaar zijn via de winkel Azure
 - **storsimple**
 - **verkeer-manager**
 - **virtuele machines**
 - **virtueel netwerk**
 - **Visual studio online**
 - **VPN-gateway**
 - **websites**

![](./media/article-metadata/checkmark-small.png)**MS.devlang**: vereist. Hiermee geeft u de programmeertaal die het artikel is van toepassing op. Één waarde per pagina.

 Als een pagina eveneens voor programmeertalen geldt, kies **meerdere**. Als een pagina voornamelijk conceptuele is en de inhoud ervan in het algemeen van toepassing op meerdere programmeertalen is, kies **meerdere**. Als een pagina is niet bedoeld voor ontwikkelaars en de programmering talen niet relevant is, kiest u **NA**. **Rest-api** gebruiken ter identificatie van REST API reference onderwerpen.

 - **cpp**
 - **DotNet**
 - **Java**
 - **JavaScript**
 - **meerdere**: de pagina gelijkelijk van toepassing is op meerdere programmeertalen.
 - **NB**: de pagina is niet gericht op ontwikkelaars en niet specifiek is voor alle programmeertalen.
 - **nodejs**
 - **doel-c**
 - **PHP**
 - **Python**
 - **rest-api**
 - **Ruby**


![](./media/article-metadata/checkmark-small.png)**MS.topic**: vereist. Specifieke informatie het onderwerp typen. De meeste nieuwe pagina's gemaakt door de medewerkers worden artikel of verwijzing.

 - **artikel**: een conceptueel onderwerp, zelfstudie, functieoverzicht of andere niet-artikel

 - **campagne-pagina**: alleen Azure.com.  Een pagina die speciaal is ontworpen als een landingspagina voor externe campagnes en is niet opgenomen als onderdeel van de primaire site IA.  Mag niet worden gebruikt voor documentatie artikelen of gewone doc landingspagina's.  Voorbeelden: azure.microsoft.com/develop/net/aspnet/; Azure.Microsoft.com/Develop/Mobile/IOS/

 - **dev center-startpagina's**: alleen Azure.com.  Een dev center-startpagina, bijvoorbeeld/ontwikkelen net /

 - **Get-slag artikel**: toewijzen aan artikelen die worden opgenomen in de sectie aan de slag of een overzicht van de navigatiebalk aan de linkerkant van een service.

 - **held-artikel**: een zelfstudie 'held' die is ontworpen om een inleiding tot een service of functie waarbij haalt bezoekers snel gebruik van de service is gestart en vrije proefperiode sign-ups en activeringen op MSDN. Deze waarde alleen naar artikelen die aan de bovenkant van de openingspagina van de documentatie voor uw service, worden opgenomen.

 - **Start-pagina**: startpagina documentatie van het bovenste niveau. We hebben slechts twee: azure.microsoft.com/documentation/ en msdn.microsoft.com/library/azure/

 - **pagina index**: tweede niveau landingspagina's voor het programmeren van talen, services of functies. Deze zijn verspreid over de Azure.com en de bibliotheek en worden gebruikt als ingang voor meer specifieke informatie bereik. Voorbeelden: http://azure.microsoft.com/develop/mobile/resources-wp8/, http://msdn.microsoft.com/library/azure/jj673460.aspx, http://msdn.microsoft.com/library/azure/hh689864.aspx

 - **infographic-pagina**: alleen Azure.com. Een pagina die is uitgerust met een infographic waarin u kunt zoeken of een poster, bijvoorbeeld http://azure.microsoft.com/documentation/infographics/windows-azure/

 - **referentie**: An API reference (inclusief REST API) of PowerShell cmdlet referentie pagina

 - **Service-startpagina**: alleen Azure.com.  Een introductiepagina doc service, bijv. /documentation/services/virtual-machines /

 - **site-sectie-start-pagina**: alleen Azure.com. Een "startpagina" voor een bepaald type inhoud op azure.com voorbeelden: http://azure.microsoft.com/documentation/infographics/, http://azure.microsoft.com/documentation/scripts/, http://azure.microsoft.com/documentation/videos/home/, http://azure.microsoft.com/downloads/

 - **video-pagina**: alleen Azure.com.  Een pagina die is uitgerust met een video, bijvoorbeeld http://azure.microsoft.com/documentation/videos/azure-webjobs-hosting-testing-net/

![](./media/article-metadata/checkmark-small.png)**MS.tgt_pltfrm**: vereist. Hiermee geeft u het doelplatform bijvoorbeeld Windows, Linux, Windows Phone, iOS, Android of speciale cache-platforms. Één waarde per pagina. Deze waarde wordt **NA** zijn voor de meeste onderwerpen, met uitzondering van mobiele en virtuele machines.

 - **cache in rol**
 - **cache-multi**
 - **cache-bestand Vgx.**
 - **cache-service**
 - **gedeelde cache**
 - **vanaf de opdrachtregel line-interface**
 - **Ibiza**: inhoud op basis van de Ibiza-portal. Gebruik deze optie alleen in gevallen waarin de besproken functie beschikbaar in zowel de Ibiza-portal en de huidige portal is.
 - **mobiele android**: alleen nu Azure.com
 - **html-Mobile**: nu slechts Azure.com
 - **mobiele ios**: alleen nu Azure.com
 - **mobiele kindle**: alleen nu Azure.com
 - **Mobile-multi**
 - **Mobile-nokia-x**: alleen nu Azure.com
 - **mobiele phonegap**: alleen nu Azure.com
 - **Mobile-sencha**: alleen nu Azure.com
 - **windows Mobile**: Azure.com alleen nu; Universele Windows
 - **windows Mobile telefoon**
 - **windows Mobile winkel**
 - **mobiele xamarin**: Azure.com alleen nu; Xamarin alle platforms
 - **Mobile-xamarin-android**: alleen nu Azure.com
 - **Mobile-xamarin-ios**: alleen nu Azure.com
 - **meerdere**: de pagina gelijkelijk van toepassing is op meerdere platforms
 - **NB**: een aanduiding van het platform is niet van toepassing voor deze pagina
 - **PowerShell**
 - **VM-linux**
 - **VM-multi**
 - **VM-windows**
 - **VM windows sharepoint**
 - **VM-windows-sql-server**
 - **VS-getting started**: identificeert de paginagroep VS aan de slag. Label 1-12-14 toegevoegd.
 - **VS-wat-is er gebeurd**: identificeert de pagina VS ophalen gestart wat er is gebeurd. Label 1-12-14 toegevoegd.

![](./media/article-metadata/checkmark-small.png)**MS.workload**: vereist. Hiermee geeft u de Azure werkbelasting van de pagina. Alleen per artikel één waarde.

**6-8/15 bijwerken** De waarde ms.workload wordt toegewezen door een xls, niet de waarde in het bestand .md. De waarde ms.workload is nog steeds vereist voor validering tot de functie kan worden bijgewerkt. Dat werk wordt gepland.  Gebruik de **waarde ' n.v.t.'** als de waarde nu.

![](./media/article-metadata/checkmark-small.png)**MS.date**: vereist. Hiermee geeft u de datum waarop die het artikel voor het laatst is bijgewerkt voor de relevantie, nauwkeurigheid, juiste schermopnamen en de koppelingen werken. Voer de datum in de notatie dd-mm-jjjj. Deze datum wordt ook weergegeven op de gepubliceerde artikel als de laatst bijgewerkte datum.

![](./media/article-metadata/checkmark-small.png)**MS.Author**: vereist. Hiermee geeft u de auteur (s) die is gekoppeld aan het onderwerp. Interne rapporten (zoals versheid) deze waarde gebruiken om de juiste auteur (s) aan het artikel koppelen. Als u meerdere waarden moet u scheiden met puntkomma's. Microsoft-aliassen of volledige e-mailadressen worden geaccepteerd. De lengte mag niet langer zijn dan 200 tekens.


###<a name="contributors-guide-links"></a>Medewerkers handleiding voor koppelingen

- [Overzichtsartikel](./../README.md)
- [Index van artikelen](./contributor-guide-index.md)


<!--Anchors-->
[Syntaxis]: #syntax
[Gebruik]: #usage
[Kenmerken en waarden voor de sectie-eigenschappen]: #attributes-and-values-for-the-properties-section
[Kenmerken en waarden voor de sectie labels]: #attributes-and-values-for-the-tags-section
