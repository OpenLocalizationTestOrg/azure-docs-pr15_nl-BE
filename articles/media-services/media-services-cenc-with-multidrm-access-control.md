<properties 
    pageTitle="CENC met Multi-DRM en toegangsbeheer: een referentie-ontwerp en de implementatie van Azure en Azure mediaservices | Microsoft Azure" 
    description="Meer informatie over hoe u in Microsoft® goede Streaming Client overdragen Kit-licentiëring." 
    services="media-services" 
    documentationCenter="" 
    authors="willzhan"  
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016"  
    ms.author="willzhan;kilroyh;yanmf;juliako"/>

#<a name="cenc-with-multi-drm-and-access-control-a-reference-design-and-implementation-on-azure-and-azure-media-services"></a>CENC met Multi-DRM en toegangsbeheer: een referentie-ontwerp en de implementatie van Azure en Azure mediaservices

##<a name="key-words"></a>Sleutel woorden
 
Azure Azure Media Services, Active Directory Azure Media Player, dynamische codering licentie levering, PlayReady, Widevine, FairPlay, algemene Encryption(CENC), Multi-DRM, Axinom, streepje, EME, MSE, JSON Web Token (JWT), Claims, moderne Browsers, Rollover-toets, symmetrische sleutel, asymmetrische sleutel, OpenID verbinden, X509 certificaat. 

##<a name="in-this-article"></a>In dit artikel

In dit artikel komen de volgende onderwerpen aan bod:

- [Inleiding](media-services-cenc-with-multidrm-access-control.md#introduction)
    - [Overzicht van dit artikel](media-services-cenc-with-multidrm-access-control.md#overview-of-this-article)
- [Een referentie-ontwerp](media-services-cenc-with-multidrm-access-control.md#a-reference-design)
- [Toewijzing ontwerpen technologie voor implementatie](media-services-cenc-with-multidrm-access-control.md#mapping-design-to-technology-for-implementation)
- [Implementatie](media-services-cenc-with-multidrm-access-control.md#implementation)
    - [Uitvoeringsprocedures](media-services-cenc-with-multidrm-access-control.md#implementation-procedures)
    - [Sommige gotchas in uitvoering](media-services-cenc-with-multidrm-access-control.md#some-gotchas-in-implementation)
- [Extra onderwerpen voor uitvoering](media-services-cenc-with-multidrm-access-control.md#additional-topics-for-implementation)
    - [HTTP of HTTPS](media-services-cenc-with-multidrm-access-control.md#http-or-https)
    - [Azure Active Directory ondertekenen sleutel rollover](media-services-cenc-with-multidrm-access-control.md#azure-active-directory-signing-key-rollover)
    - [Waar is de Access Token?](media-services-cenc-with-multidrm-access-control.md#where-is-the-access-token)
    - [Hoe Live Streaming?](media-services-cenc-with-multidrm-access-control.md#what-about-live-streaming)
    - [Hoe zit het met licentieservers buiten Azure Media Services?](media-services-cenc-with-multidrm-access-control.md#what-about-license-servers-outside-of-azure-media-services)
    - [Wat gebeurt er als ik wil een aangepaste STS gebruiken?](media-services-cenc-with-multidrm-access-control.md#what-if-i-want-to-use-a-custom-sts)
- [De voltooide systeem en test](media-services-cenc-with-multidrm-access-control.md#the-completed-system-and-test)
    - [Gebruikersaanmelding](media-services-cenc-with-multidrm-access-control.md#user-login)
    - [Met behulp van gecodeerde Media extensies voor PlayReady](media-services-cenc-with-multidrm-access-control.md#using-encrypted-media-extensipons-for-playready)
    - [Met behulp van EME voor Widevine](media-services-cenc-with-multidrm-access-control.md#using-eme-for-widevine)
    - [Gebruikers niet recht](media-services-cenc-with-multidrm-access-control.md#not-entitled-users)
    - [Aangepaste Secure Token Service wordt uitgevoerd](media-services-cenc-with-multidrm-access-control.md#running-custom-secure-token-service)
- [Samenvatting](media-services-cenc-with-multidrm-access-control.md#summary)

##<a name="introduction"></a>Inleiding

Het is bekend dat het is een complexe taak ontwerpen en bouwen van een DRM-subsysteem voor een OTT of online streaming oplossing. En het is gebruikelijk voor operators/online video providers voor het uitbesteden van dit gedeelte speciale DRM-serviceproviders. Het doel van dit document is een referentie ontwerp en implementatie van end-to-end DRM-subsysteem in OTT of online streaming oplossing presenteren.

De beoogde lezers van dit document zijn technici werken in het DRM-subsysteem van OTT online streaming/meerdere-screen oplossingen of alle lezers die geïnteresseerd zijn in het DRM-subsysteem. De veronderstelling is dat lezers bekend met ten minste één van de DRM-technologieën op de markt, zoals PlayReady, Widevine, FairPlay of toegang tot Adobe bent.

Door DRM ook wij CENC (gemeenschappelijke codering) met multi-DRM. Een belangrijke trend in online streaming en OTT industrie is met CENC met meerdere-native-DRM client platform, namelijk een verschuiving van de vorige trend van het gebruik van een enkele DRM en de client SDK voor verschillende clientplatforms. Als u CENC met meerdere native DRM, zijn zowel PlayReady als Widevine per specificatie van de [Gemeenschappelijke codering (ISO/IEC 23001-7 CENC)](http://www.iso.org/iso/home/store/catalogue_ics/catalogue_detail_ics.htm?csnumber=65271/) worden gecodeerd.

De voordelen van CENC met multi-DRM zijn als volgt:

1. Minder kosten omdat de verwerking van een enkele codering wordt gebruikt voor verschillende platforms met haar eigen DRMs; codering
1. Vermindert de kosten van het beheer van activa gecodeerde omdat slechts één exemplaar van de gecodeerde activa nodig is;
1. Elimineert de DRM-client kosten licenties sinds de oorspronkelijke DRM-client meestal vrij op de native-platform is.

Microsoft heeft een actieve initiatiefnemer van het streepje en CENC met sommige spelers belangrijke bedrijfstak is. Microsoft Azure Media Services heeft zijn ondersteuning van het streepje en CENC. Zie voor de recente aankondigingen van Mingfei blogs: [Google Widevine aankondigen license leveringsservices in Azure Media Services](https://azure.microsoft.com/blog/announcing-general-availability-of-google-widevine-license-services/)en [Azure mediaservices voegt Google Widevine verpakking voor het leveren van multi-DRM-stream](https://azure.microsoft.com/blog/azure-media-services-adds-google-widevine-packaging-for-delivering-multi-drm-stream/).  

### <a name="overview-of-this-article"></a>Overzicht van dit artikel

Het doel van dit artikel omvat het volgende:

1. Biedt een ontwerp van de verwijzing van het DRM-subsysteem met CENC met multi-DRM;
1. Hier wordt een referentie implementatie Microsoft Azure/Azure Media Services platform;
1. Sommige onderwerpen ontwerp en implementatie beschreven.

In het artikel "multi-DRM" heeft betrekking op het volgende:

1. Microsoft PlayReady
1. Google Widevine
1. Apple FairPlay (nog niet ondersteund door Azure Media Services)

In de volgende tabel bevat een overzicht van de systeemeigen platform native modus app en browsers worden ondersteund door elke DRM.

**Client Platform**|**Ondersteuning voor native DRM**|**Browser/App**|**Streaming-indelingen**
----|------|----|----
**Smart-tv's, operator STB's, OTT STB 's**|PlayReady voornamelijk, Widevine, en/of andere|Linux, Opera, WebKit, andere|Diverse formaten
**Windows 10-apparaten (PC met Windows, Windows Tablets, Windows Phone, Xbox)**|PlayReady|MS IE11-rand/EME<br/><br/><br/>UWP|STREEPJE (voor HLS, PlayReady wordt niet ondersteund)<br/><br/>STREEPJE, soepele Streaming (HLS voor, PlayReady wordt niet ondersteund) 
**Android-apparaten (telefoon, Tablet, TV)**|Widevine|Chroom/EME|STREEPJE
**iOS (iPhone, iPad), OS X-clients en Apple TV**|FairPlay|Safari 8 +/ EME|HLS
**Invoegtoepassing: Adobe WIP**|WIP-toegang|Browser-plugin|HARDE SCHIJVEN, HLS

Gezien de huidige status van elk DRM-implementatie een service worden doorgaans wilt implementeren DRMs 2 of 3 om te controleren of dat u alle soorten eindpunten in het beste adres.

Er is een balans tussen de complexiteit van de logica van de service en de complexiteit te bereiken van een bepaald niveau van gebruikerservaring op de verschillende clients op de client.

Als u de selectie, houd rekening met deze feiten:

- PlayReady is standaard geïmplementeerd in elk Windows-apparaat op andere Android-apparaten, en is beschikbaar via software-SDK's op vrijwel elk platform
- Widevine is standaard geïmplementeerd in elk Android apparaat, chroom en sommige andere apparaten
- FairPlay is beschikbaar op iOS en Mac OS-clients of via iTunes.

Zo een typische multi-DRM zijn zou:

- Optie 1: PlayReady en Widevine
- Optie 2: PlayReady, Widevine en FairPlay


## <a name="a-reference-design"></a>Een referentie-ontwerp

In deze sectie vertonen wij een referentie ontwerp agnostische aan technologieën die worden gebruikt om dit te implementeren is.

Een DRM-subsysteem bevat de volgende onderdelen:

1. Sleutelbeheer
1. DRM-verpakking
1. Levering van DRM-licentie
1. Selectievakje recht
1. Verificatie
1. Player
1. Oorsprong/CDN

In het volgende diagram ziet u de hoogste niveau interactie tussen de verschillende componenten in het DRM-subsysteem.

![DRM-subsysteem met CENC](./media/media-services-cenc-with-multidrm-access-control/media-services-generic-drm-subsystem-with-cenc.png)
 
Er zijn drie fundamentele 'lagen' in het ontwerp:

1. Back-office-laag (in zwart-wit) die zijn niet extern beschikbaar worden gesteld;
1. 'DMZ' laag (blauw), die alle de eindpunten die openbaar zijn;
1. Openbare internetlaag (Lichtblauw) met CDN en spelers met verkeer via openbare Internet.
 
Er moet een content managementtool voor het beheren van DRM-beveiliging, ongeacht het statische of dynamische codering is. De ingangen voor DRM-codering moeten worden opgenomen:

1. MBR video-inhoud;
1. Inhoud sleutel;
1. Licentie-aanschaf URL's.

Tijdens het afspelen van is de stroom van hoog niveau:

1. Gebruiker is geverifieerd;
1. Autorisatietoken is gemaakt voor de gebruiker.
1. DRM beveiligde inhoud (manifest) gedownload naar de speler;
1. Player verwerving licentieaanvraag met licentieservers en sleutel-ID en -autorisatie indient token.

Voordat u naar het volgende onderwerp, een paar woorden over het ontwerp van Sleutelbeheer.

**ContentKey-activa:**|**Scenario**
------|---------------------------
1-op-1|Het eenvoudigste geval. Het biedt de beste controle. Maar dit in het algemeen resulteert in de hoogste licentie leveringskosten. Aanvraag is ten minste één licentie vereist voor elk activum beschermde.
1-op-veel|U kunt de inhoud dezelfde sleutel voor meerdere activa. Bijvoorbeeld voor alle activa in een logische groep zoals een genre of een subset van het genre (of film-gen), kunt u een enkele sleutel.
Veel-op-1|Meerdere content sleutels nodig zijn voor elk activum. <br/><br/>Bijvoorbeeld, als u dynamische CENC bescherming met multi-DRM voor MPEG-streepje en dynamische AES 128-codering voor HLS toe te passen, moet u twee afzonderlijke inhoud sleutels, elk met een eigen ContentKeyType. (Voor de sleutel die wordt gebruikt voor dynamische CENC bescherming, ContentKeyType.CommonEncryption moeten worden gebruikt, terwijl voor de sleutel die wordt gebruikt voor dynamische AES 128-codering, ContentKeyType.EnvelopeEncryption moet worden gebruikt.)<br/><br/>Een ander voorbeeld in CENC bescherming van inhoud in theorie een sleutel kan worden gebruikt voor de bescherming van de video-stream en een andere sleutel te beschermen audiostream streepje. 
Veel-op - veel|Combinatie van de bovenstaande twee scenario's: een set sleutels voor inhoud voor elk van de verschillende activa in dezelfde activa "groep" worden gebruikt.


Een andere belangrijke factor om te overwegen is het gebruik van permanente en tijdelijke licenties.

Waarom zijn deze belangrijk? 

Ze hebben directe gevolgen voor de leveringskosten licentie als u openbare cloud voor levering van de licentie. Laten we eens de volgende twee verschillende ontwerpprogramma gevallen te illustreren:



1. Maandelijks abonnement: permanente licentie en 1-op-veel inhoud sleutel-activa-toewijzing gebruiken. Bv. we gebruiken een enkele sleutel voor de codering voor alle kinderen films. In dit geval: 

    Totaal # licenties voor alle kinderen films/apparaat aangevraagd = 1

1. Maandelijks abonnement: gebruik niet-permanente licentie en 1-op-1 toewijzing tussen sleutel en actief. In dit geval:

    Totaal # licenties aangevraagd voor alle kinderen films/apparaat [# films gevolgde] = x [# sessies]

Zoals u kunt eenvoudig zien resulteren de twee verschillende modellen in zeer verschillende licentie aanvraag patronen dus licentie levering kosten als bezorgservice licentie wordt geleverd door een openbare cloud zoals Azure Media Services.

## <a name="mapping-design-to-technology-for-implementation"></a>Toewijzing ontwerpen technologie voor implementatie

Vervolgens wijst u onze algemene ontwerp voor technologieën op het platform Microsoft Azure/Azure Media Services, door op te geven welke technologie moet worden gebruikt voor elke bouwsteen.

De volgende tabel bevat de toewijzing:

**Bouwsteen**|**Technologie**
------|-------
**Player**|[Azure MediaPlayer](https://azure.microsoft.com/services/media-services/media-player/)
**ID-Provider (IDP)**|Azure Active Directory
**Beveiligde beveiligingstokenservice (STS)**|Azure Active Directory
**Workflow voor DRM-bescherming**|Azure Media Services dynamische beveiliging
**Levering van DRM-licentie**|1. de azure Media Services-licentie voor levering (PlayReady, Widevine, FairPlay), of <br/>2. Axinom-licentieserver <br/>3. aangepaste PlayReady-licentieserver
**Oorsprong**|Azure eindpunt Streaming-mediaservices
**Sleutelbeheer**|Niet nodig voor de uitvoering van de verwijzing
**Content Management**|Een consoletoepassing C#

Met andere woorden, zowel Identity Provider (IDP) en Secure Token Service (STS) worden AD Azure. Wij zullen voor de speler, [Azure Media Player-API](http://amp.azure.net/libs/amp/latest/docs/)gebruiken. Zowel Azure Media Services Azure Media Player ondersteuning voor en -streepje en CENC met multi-DRM.

In het volgende diagram ziet u de gegevensstructuur en -stromen met het bovenstaande technologie toewijzen.

![CENC op AMS](./media/media-services-cenc-with-multidrm-access-control/media-services-cenc-subsystem-on-AMS-platform.png)

De instelling van het dynamische CENC codering, maakt de content management tool gebruik van de volgende ingangen:

1. Open inhoud;
1. Sleutel uit de sleutel genereren/beheer;
1. Licentie-aanschaf URL's;
1. Een lijst met gegevens van AD Azure.

De uitvoer van het hulpprogramma voor het beheer van de inhoud is:

1. ContentKeyAuthorizationPolicy met de specificatie van hoe licentie levering controleert of een token JWT en specificaties van DRM-licentie;
1. AssetDeliveryPolicy met specificaties voor streaming format, DRM-beveiliging en licentie-aanschaf URL's.

Tijdens runtime, wordt de gegevensstroom is als hieronder:

1. Na verificatie van de gebruiker wordt een JWT-token gegenereerd;
1. Een van de claims die zich in het token JWT is 'groepen' claim met de object-ID van de 'EntitledUserGroup'. Dit argument wordt gebruikt voor het doorgeven van 'recht controleren'.
1. Player downloads client manifest van een CENC beveiligde inhoud en 'ziet' het volgende:
    1. ID, sleutel 
    1. de inhoud is beveiligd, CENC
    1. Licentie-aanschaf URL's.

1. Speler maakt een licentieaanvraag verwerving op basis van de browser/DRM ondersteund. ID sleutel in de licentieaanvraag verwerving en wordt ook het token JWT ingediend. Licentie bezorgservice controleert of het token JWT en de claims die alvorens de benodigde licentie.

##<a name="implementation"></a>Implementatie

###<a name="implementation-procedures"></a>Uitvoeringsprocedures

De implementatie omvat de volgende stappen uit:

1. Voorbereiden van de activa van de test: een multi-bitrate test video coderen/pakket gefragmenteerd MP4 in Azure Media Services. Deze activa is niet DRM beveiligd. DRM-beveiliging wordt door dynamische beveiliging later uitgevoerd.
1. Sleutel-ID en inhoud maken key (optioneel van belangrijke zaad). Voor ons doel, systeem voor sleutelbeheer is niet nodig omdat we met slechts één set van werkt sleutel-ID en de sleutel voor een aantal test activa;
1. AMS API gebruiken multi-DRM-licentie delivery services voor het activum test configureren. Als u aangepaste licentieservers van uw bedrijf of de leveranciers van uw bedrijf in plaats van licentie services in Azure Media Services gebruikt, kunt u deze stap overslaan en licentie-aanschaf URL's opgeven in de stap voor het configureren van de levering van de licentie. AMS API is nodig om op te geven met gedetailleerde configuraties, zoals de beleidsbeperking vergunning, licentie antwoord sjablonen voor verschillende services voor DRM-licentie, enz. Op dit moment is biedt de Azure portal nog geen gebruikersinterface voor het benodigde voor deze configuratie. U kunt API niveau info vinden en voorbeeldcode in een document van Julia Kornich: [met behulp van PlayReady en/of Widevine dynamische gemeenschappelijke codering](media-services-protect-with-drm.md). 
1. AMS API activa levering beleid voor het activum test configureren gebruiken. U kunt API niveau info vinden en voorbeeldcode in een document van Julia Kornich: [met behulp van PlayReady en/of Widevine dynamische gemeenschappelijke codering](media-services-protect-with-drm.md).
1. Maken en configureren van een huurder Azure Active Directory in Azure;
1. Een paar gebruikersaccounts en groepen in uw huurder Azure Active Directory maken: Maak ten minste "EntitledUser" groep en gebruikers toevoegen aan deze groep. Gebruikers in deze groep zal worden recht in de verwerving van licenties en gebruikers in deze groep niet doorgeven verificatie mislukt en niet mogelijk om een licentie te verkrijgen. Als lid van deze groep 'EntitledUser' is een vereiste 'groepen' claim in het token JWT is uitgegeven door AD Azure. Deze eis claim moet worden opgegeven in multi-DRM-licentie delivery services stap configureren.
1. Maak een ASP.NET-MVC app die van uw video-speler hosten. Dit app ASP.NET zijn beschermd met gebruikersverificatie ten opzichte van de huurder Azure Active Directory. Goede vorderingen worden opgenomen in de toegangstokens verkregen na verificatie van de gebruiker. Verbinding maken met OpenID-API wordt aanbevolen voor deze stap. U moet de volgende NuGet-pakketten te installeren:
    - Microsoft.Azure.ActiveDirectory.GraphClient installatiepakket
    - Microsoft.Owin.Security.OpenIdConnect installatiepakket
    - Microsoft.Owin.Security.Cookies installatiepakket
    - Microsoft.Owin.Host.SystemWeb installatiepakket
    - Microsoft.IdentityModel.Clients.ActiveDirectory installatiepakket
1. Maak een speler met [Azure Media Player-API](http://amp.azure.net/libs/amp/latest/docs/). [ProtectionInfo-API Azure Media Player](http://amp.azure.net/libs/amp/latest/docs/) kunt u opgeven welke DRM-technologie te gebruiken op verschillende DRM-platform.
1. Test-matrix:

**DRM**|**Browser**|**Resultaat voor de gebruiker heeft het recht**|**Resultaat voor de gebruiker heeft niet het recht**
---|---|-----|---------
**PlayReady**|MS rand of IE11 op Windows 10|Mislukt|Mislukt
**Widevine**|Chroom op Windows 10|Mislukt|Mislukt
**FairPlay** |NOG TE BEPALEN||

George Trifonov van Azure Media Services-Team heeft een gedetailleerde stappen in Azure Active Directory instellen voor een ASP.NET-MVC player app die een blog geschreven: [integreren Azure Media Services OWIN MVC app met Azure Active Directory gebaseerd en leveren van inhoud sleutel op basis van de claims JWT beperken](http://gtrifonov.com/2015/01/24/mvc-owin-azure-media-services-ad-integration/).

George is ook een blog geschreven op [JWT token verificatie in Azure Media Services en dynamische codering](http://gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/). En hier zijn [monster op Azure AD-integratie met belangrijke levering Azure Media Services](https://github.com/AzureMediaServicesSamples/Key-delivery-with-AAD-integration/).

Voor informatie over Azure Active Directory:

- Vindt u informatie voor ontwikkelaars in [Azure Active Directory-handleiding voor ontwikkelaars](../active-directory/active-directory-developers-guide.md).
- Informatie voor beheerders vindt u in [Uw map voor Azure-AD beheren](../active-directory/active-directory-administer.md).

### <a name="some-gotchas-in-implementation"></a>Sommige gotchas in uitvoering

Er zijn enkele "gotchas" in de uitvoering. Hopelijk kunt in de volgende lijst van "gotchas" u het oplossen van problemen in het geval u problemen ondervindt.

1. **Uitgevende instelling** De URL moet eindigen met **"/"**.  

    **Doelgroep** moet de speler de toepassing client-ID en u moet ook **"/"** toevoegen aan het einde van de URL van de uitgevende instelling.

        <add key="ida:audience" value="[Application Client ID GUID]" />
        <add key="ida:issuer" value="https://sts.windows.net/[AAD Tenant ID]/" /> 

    In [De Decoder JWT](http://jwt.calebb.net/), worden er **aud** en **iss** als onder in het token JWT:
    
    ![1e gotcha](./media/media-services-cenc-with-multidrm-access-control/media-services-1st-gotcha.png)


2. Machtigingen toevoegen aan de toepassing in AAD (op het tabblad configureren van de toepassing). Dit is vereist voor elke toepassing (lokale en gedistribueerde versie).

    ![2de gotcha](./media/media-services-cenc-with-multidrm-access-control/media-services-perms-to-other-apps.png)


3. Gebruik de juiste uitgevende instelling in de dynamische CENC beveiliging instellen:

        <add key="ida:issuer" value="https://sts.windows.net/[AAD Tenant ID]/"/>
    
    De volgende werkt niet:
    
        <add key="ida:issuer" value="https://willzhanad.onmicrosoft.com/" />
    
    De GUID wordt de id van de huurder AAD. De GUID vindt u in de pop-up eindpunten in Azure portal.

4. Subsidie groepslidmaatschap beweert bevoegdheden. Zorg ervoor dat in het manifestbestand AAD van toepassing, we hebben de volgende

    "groupMembershipClaims": "All" (de standaardwaarde is null)

5. Instelling juiste TokenType bij het maken van de eisen van de beperking.

        objTokenRestrictionTemplate.TokenType = TokenType.JWT;

    Na het toevoegen van ondersteuning van JWT (AAD) naast het SWT (ACS), is de standaardwaarde TokenType TokenType.JWT. Als u een SWT/ACS gebruikt, moet u instellen op TokenType.SWT.

## <a name="additional-topics-for-implementation"></a>Extra onderwerpen voor uitvoering
Volgende we zullen de schijf uss enkele extra onderwerpen in het ontwerp en de implementatie.

###<a name="http-or-https"></a>HTTP of HTTPS?

De toepassing van ASP.NET MVC player die we ingebouwd moet ondersteuning voor het volgende:

1. Verificatie via Azure AD die moet worden onder HTTPS;
1. JWT token uitwisseling tussen client en Azure AD die moet worden onder HTTPS;
1. DRM-licentie verkrijgen door de client die worden onder HTTPS moet als levering van de licentie wordt geleverd door Azure Media Services. Natuurlijk schrijft PlayReady-productsuite niet voor HTTPS voor de levering van de licentie. Als de licentieserver PlayReady buiten Azure Media Services is, kan HTTP of HTTPS worden gebruikt.

Daarom wordt de ASP.NET-toepassing player HTTPS beste gebruikt. Dit betekent de Azure Media Player op een HTTPS-pagina. Voor stroomsgewijze HTTP wij geven de voorkeur aan, dus we moeten rekening houden met gemengde inhoud probleem.

1. Browser kan geen gemengde inhoud. Maar invoegtoepassingen zoals Silverlight en OSMF plugin voor vloeiend en -streepje. Gemengde inhoud speelt een rol bij beveiliging - dit komt door de dreiging van de mogelijkheid om schadelijke JS waardoor de gegevens van de klant kunnen lopen ' injecteren '.  Browsers dit standaard blokkeren en tot nu toe de enige manier om dit te omzeilen is op de server (de oorsprong), zodat alle domeinen (ongeacht https of http). Dit is waarschijnlijk niet een goed idee.
1. Vermijden we gemengde inhoud: beide gebruik HTTP of HTTPS gebruik. Tijdens het afspelen van gemengde inhoud is tech silverlightSS vereist een gemengde inhoud waarschuwing uit te schakelen. flashSS tech omgaat met gemengde inhoud zonder waarschuwing voor gemengde inhoud.
1. Als uw streaming eindpunt is gemaakt vóór augustus 2014, ondersteuning er geen voor HTTPS. In dit geval Maak en een nieuwe streaming eindpunt voor HTTPS gebruiken.

In de referentie-implementatie voor DRM beveiligde inhoud, is toepassingen en streaming onder HTTTPS. Voor inhoud openen hoeft Windows media player geen verificatie of licentie, zodat u de vrijheid om HTTP-of HTTPS.

### <a name="azure-active-directory-signing-key-rollover"></a>Azure Active Directory ondertekenen sleutel rollover

Dit is een belangrijk punt van de implementatie rekening te houden. Als u niet kunt u dit in uw implementatie, niet de voltooide systeem uiteindelijk meer volledig binnen maximaal 6 weken.

Azure AD maakt gebruik van industriestandaard vertrouwen tussen zichzelf en Azure AD-toepassingen. Azure AD gebruikt met name een handtekeningsleutel die uit een openbare en persoonlijke sleutelpaar bestaat. Wanneer Azure AD een beveiligingstoken dat informatie over de gebruiker bevat maakt, wordt dit token ondertekend door Azure AD met behulp van de persoonlijke sleutel is voordat het wordt verzonden naar de toepassing. Als u wilt controleren of het token is geldig en daadwerkelijk uit Azure Active Directory is, moet de toepassing van het token handtekening met behulp van de openbare sleutel blootgesteld door Azure AD die is opgenomen in van de huurder federation metagegevensdocument valideren. Deze openbare sleutel- en de handtekeningsleutel waaruit het afkomstig is-is gelijk voor alle huurders gebruikt in Azure AD.

Gedetailleerde informatie over belangrijke rollover Azure AD vindt u in het document: [Belangrijke informatie over het ondertekenen van sleutel Rollover in Azure AD](../active-directory/active-directory-signing-key-rollover.md).

Tussen de [openbare en persoonlijke sleutelpaar](https://login.windows.net/common/discovery/keys/) 

- De persoonlijke sleutel wordt gebruikt door Azure Active Directory voor het genereren van een token JWT;
- De openbare sleutel wordt gebruikt door een toepassing als DRM-licentie leveringsservices in AMS om te controleren of het token JWT;
 
Azure Active Directory draait voor veiligheid doel regelmatig dit certificaat (elke 6 weken). In geval van inbreuken op de beveiliging treedt de belangrijkste rollover op elk gewenst moment. Daarom moeten de licentie leveringsservices in AMS voor het bijwerken van de openbare sleutel gebruikt als AD Azure het sleutelpaar draait, anders de token verificatie in AMS mislukt en wordt geen licentie afgegeven. 

Dit wordt bereikt door TokenRestrictionTemplate.OpenIdConnectDiscoveryDocument bij het configureren van services voor bezorging van DRM-licentie.

De token stroom JWT is als hieronder:

1.  Azure AD zal het token JWT uitgeven met de huidige persoonlijke sleutel voor een geverifieerde gebruiker;
2.  Wanneer een speler een CENC met multi-DRM beveiligde inhoud, wordt het token van de JWT uitgegeven door Azure AD eerst zoeken.
3.  De speler verzendt verwerving licentieaanvraag met het token JWT naar leveringsservices licentie in AMS;
4.  Services voor bezorging van de licentie in AMS gebruikt om te controleren of het token JWT vóór de uitgifte van licenties voor de huidige/geldige openbare sleutel uit Azure Active Directory.

Leveringsservices DRM-licentie zal altijd worden gecontroleerd voor de huidige geldige/openbare sleutel uit Azure Active Directory. De openbare sleutel die op Azure AD is de sleutel die wordt gebruikt voor het verifiëren van een JWT token uitgegeven door AD Azure.

Wat gebeurt er als de belangrijkste rollover gebeurt nadat AAD JWT token wordt gegenereerd, maar voordat de JWT token spelers naar DRM-licentie services voor bezorging in AMS ter controle is verzonden? 

Omdat een sleutel kan te allen tijde worden gerold, is er altijd meer dan één geldige openbare sleutel in het federation metagegevensdocument beschikbaar. Levering van de licentie Azure Media Services kunt u elk van de sleutels die zijn opgegeven in het document, omdat één toets snel kan worden gerold, andere kan worden van de vervanging, enzovoort.

### <a name="where-is-the-access-token"></a>Waar is de Access Token?

Als u hoe een web app toepassing onder de [Toepassings-id aan OAuth 2.0 Client referenties subsidie](active-directory-authentication-scenarios.md#web-application-to-web-api)API aanroept bekijkt, wordt de verificatie-stroom is als hieronder:

1.  Een gebruiker is aangemeld bij Azure AD in de webtoepassing (Zie de [Webbrowser naar de webtoepassing](active-directory-authentication-scenarios.md#web-browser-to-web-application).
2.  Het eindpunt van de vergunning Azure AD leidt de gebruikersagent terug naar de clienttoepassing met een code van de vergunning. De gebruikersagent retourneert de clienttoepassing redirect URI autorisatiecode.
3.  De webtoepassing moet een toegangstoken verkrijgen, zodat deze kan op het web API verifiëren en ophalen van de gewenste resource. Het maakt een aanvraag naar Azure AD token eindpunt, de referentie, de client-ID en-API's webtoepassing URI-ID. Het geeft de code van de vergunning om te bewijzen dat de gebruiker heeft ingestemd.
4.  Azure AD verifieert de toepassing en geeft als resultaat een toegangstoken JWT die wordt gebruikt voor het web-API aanroept.
5.  Via HTTPS gebruikt de webtoepassing de geretourneerde JWT access token de tekenreeks JWT met een aanduiding "Houder" in de kop van de vergunning van het verzoek toevoegen aan het web API. Het web API valideert het token JWT, en als de validatie voltooid is, geeft de gewenste resource.

In deze overdracht 'toepassings-id' het web API wordt vertrouwd dat de webtoepassing de gebruiker geverifieerd. Daarom wordt dit patroon een vertrouwd subsysteem genoemd. In het [diagram op deze pagina](http://msdn.microsoft.com/library/azure/dn645542.aspx/) wordt beschreven hoe de autorisatiecode stroom works verlenen.

Verwerving van licenties met token beperking volgen we hetzelfde patroon vertrouwd subsysteem. En de licentie bezorgservice in Azure Media Services is het web API resource, de 'backend resource' een webtoepassing toegang moet hebben. Dus waar het toegangstoken is?

We zijn immers toegangstoken verkrijgen uit Azure Active Directory. Na een geslaagde verificatie, wordt autorisatiecode geretourneerd. De code van de vergunning wordt vervolgens gebruikt, samen met client-ID en app sleutel om te wisselen voor het toegangstoken. En is het toegangstoken voor toegang tot een "pointer" toepassing wijzen of vertegenwoordiging bezorgservice Azure Media Services-licentie.

We moeten registreren en de app 'muisaanwijzer' in Azure AD configureren aan de hand van de volgende stappen uit:

1.  In de huurder Azure AD

    - een toepassing (resource) met eenmalige aanmelding URL toevoegen: 

    https://[resource_name].azurewebsites.NET/ en 

    - App-ID URL: 
    
    https://[aad_tenant_name].onmicrosoft.com/[resource_name]; 
2.  Voeg een nieuwe sleutel voor de resource-app;
3.  Het manifestbestand app bijwerken zodat de eigenschap groupMembershipClaims de volgende waarde heeft: "groupMembershipClaims": "All",  
4.  De Azure AD app player web App aan te wijzen, in de sectie 'machtigingen voor andere toepassingen' Voeg de bron app die is toegevoegd in stap 1 hierboven. Controleer onder 'overgedragen machtigingen' vinkje 'Toegang [resource_name]'. Dit geeft de web app-machtiging voor het maken van toegangstokens voor toegang tot de bron app. U moet dit doen voor lokale en gedistribueerde versie van de web app als u met Visual Studio en Azure web app ontwikkelt.
    
De JWT-token verleend door AD Azure dus inderdaad het toegangstoken voor toegang tot deze bron 'muisaanwijzer'.

### <a name="what-about-live-streaming"></a>Hoe Live Streaming?

In het bovenstaande heeft onze discussie is gericht op de activa op verzoek. Hoe live streaming?

Het goede nieuws is dat u kunt precies hetzelfde ontwerp en implementatie voor de bescherming van live streaming in Azure Media Services, van het activum is gekoppeld aan een programma als een activum' VOD' te behandelen.

Het is met name bekend om te doen live streaming in Azure Media Services, moet u een kanaal en vervolgens een programma onder het kanaal te maken. Te maken van het programma, moet u voor het maken van een actief met de live archief voor het programma. Oog CENC met multi-DRM-beveiliging van de live inhoud, u hoeft te doen, is het toepassen van dezelfde setup/verwerking aan het activum alsof het een actief' VOD' voordat u het programma start.

### <a name="what-about-license-servers-outside-of-azure-media-services"></a>Hoe zit het met licentieservers buiten Azure Media Services?

Klanten kunnen vaak hebben geïnvesteerd in licentie voor server-farm in hun eigen gegevens centreren of gehost door DRM-serviceproviders. Gelukkig Azure Media Services beveiliging, kunt u werken in de hybrid modus: inhoud gehost en dynamisch beschermd in Azure Media Services, terwijl de DRM-licenties worden geleverd door servers buiten Azure Media Services. In dit geval zijn de volgende wijzigingen:

1. De beveiligde Token Service moet tokens die aanvaardbaar zijn en kunnen worden geverifieerd door de server-farm licentie uitgeven. De Widevine licentieservers geleverd door Axinom moet bijvoorbeeld een JWT-token "recht-bericht" bevat. Daarom moet u een STS voor het uitgeven van dergelijke JWT token hebben. De auteurs hebben een dergelijke uitvoering voltooid en vindt u de details in het volgende document in [Azure documentatie Center](https://azure.microsoft.com/documentation/): [Met behulp van Axinom Widevine licenties Azure Media services leveren](media-services-axinom-integration.md). 
1. U moet niet langer bezorgservice licentie (ContentKeyAuthorizationPolicy) in Azure Media Services configureren. Wat u moet doen is te voorzien in de licentie aanschaf URL's (PlayReady, Widevine en FairPlay) wanneer u AssetDeliveryPolicy configureert voor het opzetten van CENC met multi-DRM.
 
### <a name="what-if-i-want-to-use-a-custom-sts"></a>Wat gebeurt er als ik wil een aangepaste STS gebruiken?

Het is mogelijk een paar redenen waarom een klant kiezen ervoor een aangepaste STS (Secure Token Service) gebruiken voor het leveren van JWT tokens. Een paar hiervan zijn:

1.  STS biedt geen ondersteuning voor het Identity Provider (IDP) door de klant wordt gebruikt. In dit geval mogelijk een aangepaste STS een optie.
2.  De klant moet meer flexibele of strengere controle STS te integreren met de klant abonnee facturering systeem. Een operator MVPD bieden bijvoorbeeld OTT abonnee pakketten zoals premium, basic, sport, enz. De operator wilt voldoen aan de claims in een pakket van een abonnee-token zodat alleen de inhoud in het juiste pakket beschikbaar wordt gesteld. In dit geval een aangepaste STS de benodigde flexibiliteit en controle biedt.

Twee moet worden gewijzigd wanneer u een aangepaste STS:

1.  Bij het configureren van de bezorgservice licentie voor een actief, moet u de beveiligingssleutel die wordt gebruikt voor verificatie door de aangepaste STS (meer details hieronder) in plaats van de huidige sleutel uit Azure Active Directory opgeven.
2.  Wanneer een JTW token wordt gegenereerd, een beveiligingssleutel is opgegeven in plaats van de persoonlijke sleutel van de huidige X509 certificaat in Azure Active Directory.

Er zijn twee soorten sleutels:

1.  Symmetrische sleutel: dezelfde sleutel wordt gebruikt voor zowel genereren en controleren van een token JWT;
2.  Asymmetrische sleutel: een openbare en persoonlijke sleutelpaar in een X509 certificaat met de persoonlijke sleutel voor het coderen van/genereren van een token JWT en de openbare sleutel voor het verifiëren van het token wordt gebruikt.

####<a name="tech-note"></a>Technische opmerking

Als u .NET Framework / C# als uw ontwikkelplatform, de X509 certificaat voor asymmetrische sleutel moet de lengte ten minste 2048 hebben. Dit is een vereiste van de klasse System.IdentityModel.Tokens.X509AsymmetricSecurityKey in .NET Framework. Anders wordt de volgende uitzondering worden gegenereerd:

IDX10630: System.IdentityModel.Tokens.X509AsymmetricSecurityKey voor ondertekening kan niet kleiner zijn dan '2048-bits. 

## <a name="the-completed-system-and-test"></a>De voltooide systeem en test

We doorlopen een paar scenario's in het voltooide end-to-end-systeem zodat lezers 'afbeelding' van het gedrag van een basic voordat u een aanmeldingsaccount.

De speler webtoepassing en de aanmelding vindt u [hier](https://openidconnectweb.azurewebsites.net/).

Als u nodig hebt is "niet-geïntegreerde" scenario: video activa die worden gehost in Azure Media Services die zijn opgeheven ofwel of DRM beveiligd maar zonder verificatie token (een licentie verlenen aan iedereen die aangevraagd), kunt u deze testen zonder aanmelding (door het activeren van HTTP als uw video-streaming via HTTP).

Als wat u moet end-to-end geïntegreerde scenario: video activa onder dynamische DRM-beveiliging in Azure Media Services, met token verificatie en JWT token wordt gegenereerd door AD Azure is, moet u inloggen.

### <a name="user-login"></a>Gebruikersaanmelding

U moet end-to-end geïntegreerde DRM-systeem testen, hebt een 'account' gemaakt of toegevoegd. 

Welke account?

Hoewel Azure oorspronkelijk toegang door gebruikers van Microsoft-account, kan deze nu toegang door gebruikers van beide systemen. Dit is gedaan door alle de Azure eigenschappen vertrouwensrelatie Azure AD voor verificatie, met Azure AD organisatie-gebruikers, en door het maken van een relatie federation waarbij AD Azure Microsoft account consument identiteit system voor het verifiëren van gebruikers van de consument vertrouwt. Als gevolg hiervan is Azure AD 'gast' Microsoft-accounts zoals "native" Azure AD accounts verifiëren.

Aangezien AD Azure Microsoft Account (MSA) domein vertrouwt, kunt u alle accounts in een van de volgende domeinen naar de aangepaste Azure AD pachters en gebruik de account om in te loggen kunt toevoegen:

**Domeinnaam**|**Domein**
-----------|----------
**Aangepaste Azure AD huurder domein**|somename.onmicrosoft.com
**Bedrijfsdomein**|Microsoft.com
**Microsoft-Account (MSA) domein**|Outlook.com, live.com, hotmail.com

U kunt contact opnemen met een van de auteurs hebben een account gemaakt of voor u toegevoegd. 

Hieronder worden de screenshots van verschillende aanmeldingspagina's die worden gebruikt door verschillende domeinaccounts.

**Aangepaste Azure AD huurder domeinaccount**: In dit geval ziet u de aangepaste aanmeldingspagina van de aangepaste Azure AD huurder domein.

![Aangepaste Azure AD huurder domeinaccount](./media/media-services-cenc-with-multidrm-access-control/media-services-ad-tenant-domain1.png)

**Microsoft domeinaccount met smartcard**: In dit geval ziet u de aanmeldingspagina aangepast door Microsoft corporate IT met verificatie met twee factoren.

![Aangepaste Azure AD huurder domeinaccount](./media/media-services-cenc-with-multidrm-access-control/media-services-ad-tenant-domain2.png)

**Microsoft-Account (MSA)**: In dit geval ziet u de aanmeldingspagina van Microsoft-Account voor consumenten.

![Aangepaste Azure AD huurder domeinaccount](./media/media-services-cenc-with-multidrm-access-control/media-services-ad-tenant-domain3.png)

### <a name="using-encrypted-media-extensions-for-playready"></a>Met behulp van gecodeerde Media extensies voor PlayReady

PlayReady op een moderne browser met gecodeerde Media extensies (EME) voor de ondersteuning van PlayReady, zoals Internet Explorer 11 op Windows 8.1 en omhoog en Microsoft Edge browser van Windows 10, worden de onderliggende DRM voor EME.

![Met behulp van EME voor PlayReady](./media/media-services-cenc-with-multidrm-access-control/media-services-eme-for-playready1.png)

Het gebied donkere speler is vanwege het feit dat PlayReady bescherming wordt voorkomen dat een schermopname maken van beschermde video maken. 

Het volgende scherm ziet u de player-Plug-ins en MSE/EME ondersteuning.

![Met behulp van EME voor PlayReady](./media/media-services-cenc-with-multidrm-access-control/media-services-eme-for-playready2.png)

EME in Microsoft Edge en Internet Explorer 11 op Windows 10 kan aanroepen van [PlayReady SL3000](https://www.microsoft.com/playready/features/EnhancedContentProtection.aspx/) op Windows 10-apparaten die dit ondersteunen. PlayReady SL3000 ontgrendelt u de stroom van verbeterde premium-inhoud (4K, HDR, enz.) en nieuwe content delivery-modellen (vroege venster voor extra inhoud).

Gericht op de Windows-apparaten: PlayReady is de enige DRM in de hardware op Windows-apparaten (PlayReady SL3000). Een streaming service met PlayReady via EME of via een toepassing UWP en bieden een hogere videokwaliteit met PlayReady SL3000 dan een andere DRM. 2K inhoud zal meestal lopen via Chrome of Firefox en 4K-inhoud via Microsoft Edge/IE11 of een toepassing UWP op hetzelfde apparaat (afhankelijk van de instellingen van de service en de uitvoering).


#### <a name="using-eme-for-widevine"></a>Met behulp van EME voor Widevine

Op een moderne browser met EME/Widevine support, zoals Chrome 41 + op Windows 10, Windows 8.1, Mac OSX Yosemite en Chrome voor Android 4.4.4, is Google Widevine de DRM achter EME.

![Met behulp van EME voor Widevine](./media/media-services-cenc-with-multidrm-access-control/media-services-eme-for-widevine1.png)

U ziet dat Widevine voorkomt niet dat een schermopname maken van beschermde video maken.

![Met behulp van EME voor Widevine](./media/media-services-cenc-with-multidrm-access-control/media-services-eme-for-widevine2.png)

### <a name="not-entitled-users"></a>Gebruikers niet recht

Als een gebruiker is geen lid van de groep "Gebruikers getiteld", de gebruiker worden niet kan worden "recht" en de multi-DRM-licentie service zal weigeren de afgifte van de gevraagde licentie zoals hieronder wordt weergegeven. De gedetailleerde beschrijving is ' licentie aanschaffen is mislukt ', die is ontworpen.

![Gebruikers niet recht](./media/media-services-cenc-with-multidrm-access-control/media-services-unentitledusers.png)

### <a name="running-custom-secure-token-service"></a>Aangepaste Secure Token Service wordt uitgevoerd

Voor het scenario van het uitvoeren van aangepaste Secure Token Service (STS), wordt de JWT-token uitgegeven door de aangepaste STS symmetrisch of asymmetrisch sleutel gebruikt. 

Het geval van het gebruik van symmetrische sleutel (met chroom):

![Uitvoeren van aangepaste STS](./media/media-services-cenc-with-multidrm-access-control/media-services-running-sts1.png)

Het geval van het gebruik van asymmetrische sleutel via een X509 certificaat (met moderne browser Microsoft).

![Uitvoeren van aangepaste STS](./media/media-services-cenc-with-multidrm-access-control/media-services-running-sts2.png)

In beide gevallen blijft hetzelfde: via Azure AD gebruikersverificatie. Het enige verschil is dat JWT tokens worden uitgegeven door de aangepaste STS in plaats van Azure AD. Natuurlijk bij het configureren van beveiliging voor dynamische CENC, de beperking van de bezorgservice licentie geeft het type JWT token, symmetrisch of asymmetrisch sleutel.

## <a name="summary"></a>Samenvatting

In dit document besproken CENC met meerdere native DRM en toegangscontrole via token verificatie: het ontwerp en de uitvoering ervan met Azure, Azure Media Services en Azure Media Player.

- Een referentie-ontwerp wordt overgelegd waarin alle onderdelen in een subsysteem DRM/CENC;
- Een referentie-implementatie op Azure, Azure Media Services en Azure Media Player.
- Sommige onderwerpen die rechtstreeks betrokken is bij het ontwerp en de implementatie worden ook besproken.


##<a name="media-services-learning-paths"></a>Leerfasen Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

###<a name="acknowledgments"></a>Bevestigingen 

William Zhang Mingfei Yan, Roland Le Franc, Kilroy Hughes, Julia Kornich
