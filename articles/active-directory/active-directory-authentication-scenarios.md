
<properties
   pageTitle="Verificatie-scenario's voor Azure AD | Microsoft Azure"
   description="Een overzicht van de vijf meest voorkomende verificatie scenario's voor Azure Active Directory (AAD)"
   services="active-directory"
   documentationCenter="dev-center-name"
   authors="bryanla"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/16/2016"
   ms.author="mbaldwin"/>

# <a name="authentication-scenarios-for-azure-ad"></a>Verificatie-scenario's voor Azure AD

Azure Active Directory (AD Azure) vereenvoudigt de verificatie voor ontwikkelaars door middel van identiteit als een service, met ondersteuning voor industriestandaard protocollen zoals 2.0 OAuth en OpenID verbinding als open bron bibliotheken voor verschillende platforms om u snel coderen. Dit document helpt u bij het begrijpen van de verschillende scenario's Azure AD ondersteund en wordt beschreven hoe u aan de slag. Het onderverdeeld in de volgende secties:

- [Basisbeginselen van verificatie in Azure AD](#basics-of-authentication-in-azure-ad)

- [Claims in beveiligingstokens Azure AD](#claims-in-azure-ad-security-tokens)

- [Basisbeginselen van het registreren van een toepassing in Azure AD](#basics-of-registering-an-application-in-azure-ad)

- [Toepassingstypen en scenario 's](#application-types-and-scenarios)

  - [Webbrowser-webtoepassing](#web-browser-to-web-application)

  - [Toepassing van één pagina (SPA)](#single-page-application-spa)

  - [Oorspronkelijke toepassing web API](#native-application-to-web-api)

  - [Webtoepassing voor Web-API](#web-application-to-web-api)

  - [Daemon- of servertoepassing met Web API](#daemon-or-server-application-to-web-api)



## <a name="basics-of-authentication-in-azure-ad"></a>Basisbeginselen van verificatie in Azure AD

Lees deze sectie als u niet bekend met de basisconcepten van verificatie in AD Azure bent. U kunt anders gaat u verder naar de [typen toepassingen en scenario's](#application-types-and-scenarios).

Laten we eens de meest elementaire scenario waarbij identiteit nodig: een gebruiker in een webbrowser nodig heeft om een webtoepassing te verifiëren. In dit scenario wordt beschreven in de sectie [Webtoepassing webbrowser](#web-browser-to-web-application) uitgebreider, maar het is een handig beginpunt voor het illustreren de mogelijkheden van Azure AD en voorstelling van de werking van het scenario. Houd rekening met het volgende diagram voor dit scenario:

![Overzicht van de aanmelding bij de webtoepassing](./media/active-directory-authentication-scenarios/basics_of_auth_in_aad.png)

Met het diagram hierboven in gedachten is hier wat u moet weten over de verschillende onderdelen:

- Azure AD is de id-provider, die verantwoordelijk is voor de identiteit van gebruikers en toepassingen die aanwezig zijn in de directory van een organisatie en het afgeven van uiteindelijk beveiligingstokens bij een geslaagde verificatie van gebruikers en toepassingen.


- Een toepassing die u wil uitbesteden Azure AD-verificatie moet zijn geregistreerd in Azure AD, registreert en een unieke aanduiding voor de app in de directory.


- Ontwikkelaars kunnen de open-source AD Azure verificatie bibliotheken gebruiken waarmee u verificatie door de verwerking van gegevens van het protocol voor u gemakkelijk. Zie [Azure Active Directory verificatie bibliotheken](active-directory-authentication-libraries.md) voor meer informatie.


• Nadat een gebruiker is geverifieerd, moet de toepassing het beveiligingstoken van de gebruiker om ervoor te zorgen dat de verificatie is geslaagd voor de beoogde partijen valideren. Ontwikkelaars kunnen de beschikbare verificatie-bibliotheken gebruiken voor het verwerken van de validatie van een token uit Azure Active Directory, met inbegrip van JSON Web Tokens (JWT) of SAML 2.0. Zie de documentatie van het [Beveiligingstoken JWT-Handler](https://msdn.microsoft.com/library/dn205065.aspx) handmatig uitvoeren van validatie.


> [AZURE.IMPORTANT] Azure AD maakt gebruik van cryptografie met openbare sleutels voor het ondertekenen van tokens en controleer of deze geldig zijn. Zie [Belangrijke informatie over het ondertekenen van sleutel Rollover in Azure AD](active-directory-signing-key-rollover.md) voor meer informatie over de logica nodig hebt in uw toepassing om ervoor te zorgen dat altijd wordt bijgewerkt met de meest recente toetsen.


• De stroom van verzoeken en antwoorden voor het verificatieproces wordt bepaald door het verificatieprotocol dat wordt gebruikt, zoals 2.0 OAuth OpenID verbinden, WS-Federation of SAML 2.0. Deze protocollen worden hieronder nader besproken in het onderwerp [Azure Active Directory-verificatieprotocollen](active-directory-authentication-protocols.md) en in de onderstaande secties.

> [AZURE.NOTE] Azure AD ondersteunt de 2.0-OAuth en OpenID verbinding normen uitgebreid maken gebruik van toonder-tokens, aan toonder tokens weergegeven als JWTs. Een token aan toonder is een lichtgewicht beveiligingstoken dat door de "houder" toegang tot een beveiligde bron verleent. In dit opzicht is de "houder" een partij die het token kan presenteren. Als een partij moet eerst worden geverifieerd met Azure Active Directory voor het ontvangen van de token aan toonder als worden niet de vereiste stappen voor het beveiligen van de token in de overdracht en opslag is genomen, kan het worden onderschept en gebruikt door een onbedoelde partij. Sommige beveiligingstokens beschikken over een ingebouwd mechanisme om te voorkomen dat onbevoegden kunnen gebruiken, kunnen aan toonder tokens hoeft dit mechanisme niet en moeten worden vervoerd in een beveiligd kanaal zoals transport layer security (HTTPS). Als een token aan toonder wordt verzonden in de Open, kan een man in het midden aanval worden gebruikt door een kwaadwillende partij de token verkrijgen en het gebruik voor een onbevoegde toegang tot een beveiligde bron. Dezelfde beveiligingsprincipes van toepassing wanneer of aan toonder tokens voor later gebruik opslaan in de cache opslaan. Er altijd voor zorgen dat uw toepassing worden verzonden en vruchtdragende tokens worden opgeslagen op een veilige manier. Zie voor meer overwegingen over de beveiliging op tokens aan toonder, [RFC 6750 deel 5](http://tools.ietf.org/html/rfc6750).


Nu dat er een overzicht van de basisbeginselen, lees de secties hieronder om te begrijpen hoe de inrichting in Azure AD werkt en de algemene scenario's Azure AD ondersteunt.


## <a name="claims-in-azure-ad-security-tokens"></a>Claims in beveiligingstokens Azure AD

Beveiligingstokens die zijn afgegeven door AD Azure bevatten claims of verklaringen van informatie over het onderwerp dat is geverifieerd. Deze vorderingen kunnen door de toepassing worden gebruikt voor verschillende taken. Ze kunnen bijvoorbeeld het token valideren, identificatie van de certificaathouder directory huurder, gebruikersgegevens weergeven, bepalen van de certificaathouder toestemming, enzovoort worden gebruikt. De claims in een bepaalde beveiligings-token aanwezig zijn afhankelijk van het type van het token, het type referenties gebruikt voor de verificatie van de gebruiker en de configuratie van de toepassing. In de onderstaande tabel vindt u een korte beschrijving van elk type claim dat door AD Azure. Zie [ondersteunde Token en typen](active-directory-token-and-claims.md)voor meer informatie.


| Claim | Beschrijving |
|-------|-------------|
| Toepassings-ID | De toepassing die van het token gebruikmaakt wordt geïdentificeerd.
| Doelgroep | Geeft de ontvanger bron die het token is bedoeld voor. |
| Toepassing verificatie Context klassenverwijzing | Geeft aan hoe de client is geverifieerd (openbare client versus vertrouwelijke client). |
| Instant-verificatie | Registreert de datum en tijd waarop de verificatie heeft plaatsgevonden. |
| Verificatiemethode | Geeft aan hoe het onderwerp van het token is geverifieerd (wachtwoord, certificaat, enz.). |
| Voornaam | De opgegeven naam van de gebruiker biedt zoals AD Azure. |
| Groepen | Bevat de object-id's van Azure AD groepen die de gebruiker lid van is. |
| ID-Provider | De id-provider die het onderwerp van het token van de geverifieerde vastgelegd. |
| Uitgegeven op | De tijd waarop het token is uitgegeven, vaak gebruikt voor token versheid wordt vastgelegd. |
| Uitgevende instelling | Identificeert de STS die het token als de huurder Azure AD uitgestoten. |
| Achternaam | Hier de achternaam van de gebruiker zoals ingesteld in Azure AD. |
| Naam | Biedt een mens leesbaar waarde waarmee het onderwerp van het token. |
| Object-Id | Bevat een onveranderbare, unieke id van het onderwerp in Azure AD. |
| Rollen | Bevat de beschrijvende namen van Azure rollen van AD-toepassing die de gebruiker heeft gekregen. |
| Toepassingsgebied | Geeft aan dat de machtigingen die zijn toegekend aan de clienttoepassing. |
| Onderwerp | De aangever waarover het token wordt gesteld informatie geeft. |
| Huurder-Id | Bevat een onveranderbare, unieke id van de pachter directory die het token uitgegeven. |
| Levensduur van tokens | Hiermee definieert u het tijdsinterval waarbinnen een token geldig is. |
| UPN-naam | Bevat de UPN-naam van het onderwerp. |
| Versie | Bevat het versienummer van het token. |


## <a name="basics-of-registering-an-application-in-azure-ad"></a>Basisbeginselen van het registreren van een toepassing in Azure AD

Elke toepassing die verificatie van AD Azure heeft moet worden geregistreerd in een map. Deze stap omvat Azure AD vertellen over de toepassing, met inbegrip van de URL waar dit zich, de URL voor het verzenden van antwoord na verificatie van de URI te identificeren en de toepassing. Deze informatie is vereist om een paar belangrijke redenen:

- Azure AD moet coördinaten om te communiceren met de toepassing bij het verwerken van tokens voor eenmalige aanmelding of uitwisselen. Dit omvat het volgende:

  - URI van toepassing-ID: De id voor een toepassing. Deze waarde wordt verzonden naar Azure AD tijdens verificatie om aan te geven welke toepassing de beller wil een token. Deze waarde is bovendien opgenomen in het token dat de toepassing weet was het beoogde doel.


  - URL en de URI omleiden beantwoorden: in het geval van een web-API of de webtoepassing de antwoord-URL is de locatie waarop Azure AD de verificatiereactie, met inbegrip van een token als verificatie gelukt is, verzendt. De URI voor omleiden is een eigen toepassing een unieke id waaraan Azure AD user-agent in OAuth 2.0 aanvragen worden omgeleid.


  - Client-ID: De ID voor een toepassing die wordt gegenereerd door Azure AD wanneer de toepassing is geregistreerd. Bij het aanvragen van een vergunning- of token worden de client-ID en de sleutel verzonden naar Azure AD tijdens verificatie.


  - Sleutel: De sleutel die wordt verzonden met een client-ID bij het verifiëren van naar Azure AD een web API aanroepen.


- Azure AD moet ervoor zorgen dat de toepassing heeft de vereiste machtigingen voor toegang tot andere toepassingen in uw organisatie, uw Active directory-gegevens, enzovoort

Provisioning wordt duidelijker wanneer u weet er zijn twee categorieën van toepassingen die kunnen worden ontwikkeld en geïntegreerd met AD Azure:

- Eén huurder toepassing: een toepassing één huurder is bedoeld voor gebruik in een organisatie. Dit zijn meestal van branchespecifieke toepassingen geschreven door de ontwikkelaar van een onderneming. Een toepassing één huurder moet alleen worden geopend door gebruikers in een bepaalde map en daardoor alleen moet worden ingericht in een bepaalde map. Deze toepassingen worden meestal geregistreerd door een ontwikkelaar in de organisatie.


- Huurder met meerdere toepassingen: een toepassing met meerdere huurder is niet bedoeld voor gebruik in veel organisaties één organisatie. Dit zijn meestal software-as-a-service (SaaS)-toepassingen geschreven door een onafhankelijke softwareleverancier (ISV). Toepassingen met meerdere huurder moeten worden ingericht in elke map waarin ze worden gebruikt, waarvoor toestemming van gebruiker of beheerder om te registreren. Dit proces toestemming wordt gestart wanneer een toepassing is geregistreerd in Active directory en wordt toegang verleend tot de Graph API of een andere web-API. Als een gebruiker of beheerder van een andere organisatie aanmeldt om de toepassing te gebruiken, moeten zij worden aangeboden met een dialoogvenster waarin de machtigingen die voor de toepassing moet worden weergegeven. De gebruiker of beheerder kan vervolgens toestemming voor de toepassing die de toepassing toegang geeft tot de vermelde gegevens en registreert u ten slotte de toepassing in de map. Zie [overzicht van instemming Framework](active-directory-integrating-applications.md#overview-of-the-consent-framework)voor meer informatie.

Aantal aanvullende overwegingen ontstaan bij het ontwikkelen van een toepassing met meerdere huurder in plaats van een toepassing één huurder. Bijvoorbeeld, als u uw toepassing beschikbaar voor gebruikers in meerdere mappen, moet u een mechanisme om te bepalen welke huurder ze. Een toepassing één huurder hoeft alleen te zoeken in een eigen map voor een gebruiker, terwijl een toepassing met meerdere huurder dient ter identificatie van een bepaalde gebruiker van alle mappen in Azure AD. Azure AD biedt voor het uitvoeren van deze taak een gemeenschappelijke verificatie eindpunt waar elke toepassing met meerdere huurder-in-aanvragen, in plaats van een huurder specifiek eindpunt kunt verwijzen. Dit eindpunt is https://login.microsoftonline.com/common voor alle mappen in Azure AD, dat een huurder specifiek eindpunt https://login.microsoftonline.com/contoso.onmicrosoft.com worden kan. Het gemeenschappelijke eindpunt is vooral belangrijk bij de ontwikkeling van uw toepassing omdat u de benodigde logica moet voor het verwerken van meerdere huurders tijdens het aanmelden, afmelden en token validatie.

Als u een toepassing één huurder momenteel ontwikkelt zodat deze beschikbaar is voor veel organisaties, kunt u gemakkelijk wijzigingen aanbrengen aan de toepassing en de configuratie in Azure AD te maken met meerdere huurder staat. Azure AD gebruikt bovendien de handtekeningsleutel dezelfde voor alle tokens in alle mappen of u verificatie in een enkele huurder of pachter met meerdere toepassing aanbiedt.

Elk scenario vermeld in dit document bevat een beschrijving van de inrichting eisen deel. Zie voor meer informatie over het inrichten van een toepassing in Azure AD en de verschillen tussen de één en de huurder met meerdere toepassingen, [Integratie van toepassingen met Azure Active Directory](active-directory-integrating-applications.md) voor meer informatie. Blijven lezen om te begrijpen van de gebruikelijke scenario's voor toepassing in Azure AD.

## <a name="application-types-and-scenarios"></a>Toepassingstypen en scenario 's

De scenario's beschreven in dit document kan worden ontwikkeld met behulp van diverse talen en platforms. Ze zijn gemaakt door de volledige codevoorbeelden die beschikbaar in onze [gids codevoorbeelden](active-directory-code-samples.md)of rechtstreeks vanuit de bijbehorende [Github monster bibliotheken zijn](https://github.com/Azure-Samples?utf8=%E2%9C%93&query=active-directory). Ook als uw toepassing nodig heeft voor een bepaald gedeelte of een segment van een end-to-end scenario, in de meeste gevallen deze functionaliteit kan worden toegevoegd onafhankelijk van elkaar. Als er een originele toepassing die een web-API aanroept, kunt u bijvoorbeeld eenvoudig een webtoepassing die ook het web-API aanroept toevoegen. Het volgende diagram worden deze scenario's en de toepassingstypen, en hoe de verschillende onderdelen kunnen worden toegevoegd:

![Toepassingstypen en scenario 's](./media/active-directory-authentication-scenarios/application_types_and_scenarios.png)

Dit zijn de vijf primaire toepassing scenario's worden ondersteund door AD Azure:

- [Webbrowser-webtoepassing](#web-browser-to-web-application): moet een gebruiker zich aanmelden bij een webtoepassing die wordt beveiligd door AD Azure.

- [Toepassing van één pagina (SPA)](#single-page-application-spa): een gebruiker moet zich aanmelden bij een enkele pagina toepassing die is beveiligd met AD Azure.

- [Oorspronkelijke toepassing Web API](#native-application-to-web-api): een eigen toepassing die wordt uitgevoerd op een telefoon, tablet of PC nodig heeft voor het verifiëren van een gebruiker als u resources uit een web API die is beveiligd met AD Azure.

- [Webtoepassing voor Web-API](#web-application-to-web-api): een webtoepassing moet verkrijgen van de bronnen van een web API beveiligd door AD Azure.

- [Daemon of servertoepassing Web API](#daemon-or-server-application-to-web-api): een daemontoepassing of een servertoepassing zonder gebruikersinterface web resources ophalen uit een web API beveiligd door AD Azure nodig heeft.

### <a name="web-browser-to-web-application"></a>Webbrowser-webtoepassing

Deze sectie beschrijft een toepassing die een gebruiker in een webbrowser om een webtoepassing te worden geverifieerd. In dit scenario wordt de webtoepassing zorgt ervoor dat de browser van de gebruiker aanmelden naar Azure AD. Azure AD retourneert een antwoord aanmelden via de browser van de gebruiker, die claims over de gebruiker in een beveiligingstoken bevat. In dit scenario biedt ondersteuning voor aanmelding via de protocollen van WS-Federation, SAML 2.0 en OpenID verbinding.


#### <a name="diagram"></a>Diagram
![Overdracht van verificatie voor browser om de webtoepassing](./media/active-directory-authentication-scenarios/web_browser_to_web_api.png)


#### <a name="description-of-protocol-flow"></a>Beschrijving van de stroom van Protocol


1. Wanneer een gebruiker bezoekt de toepassing en de behoeften aan te melden, worden zij omgeleid via een verzoek-in het eindpunt van de verificatie in Azure AD.


2. De gebruiker zich aanmeldt op de pagina aanmelden.


3. Verificatie is geslaagd, Azure AD maakt een verificatietoken als geeft als resultaat een reactie-in van de toepassing antwoord URL die is geconfigureerd in de Portal Azure Management. Voor de toepassing van een productieorder moet deze URL antwoord HTTPS. Het resulterende token bevat over de gebruikers- en Azure AD claims die voor de toepassing vereist zijn voor het valideren van het token.


4. De aanvraag valideert het token met behulp van een openbare sleutel voor ondertekening en informatie over de uitgever beschikbaar op de federation-metagegevensdocument voor Azure advertentie. Nadat de aanvraag het token valideert, Azure AD een nieuwe sessie gestart met de gebruiker. Deze sessie kan de gebruiker toegang tot de toepassing totdat deze verloopt.


#### <a name="code-samples"></a>Codevoorbeelden


Zie de codevoorbeelden voor webbrowser webtoepassing scenario's. En kom kijken vaak--we voortdurend nieuwe monsters toevoegen. [Webbrowser-webtoepassing](active-directory-code-samples.md#web-browser-to-web-application).


#### <a name="registering"></a>Registreren


- Één huurder: Als u een toepassing speciaal voor uw organisatie bouwt, het moet worden geregistreerd in de map van uw bedrijf met behulp van de Portal Azure Management.


- Meerdere huurder: Als u een toepassing die door gebruikers buiten uw organisatie kan worden gebruikt, deze moet worden geregistreerd in de map van uw bedrijf, maar ook moet worden geregistreerd in de map van elke organisatie die de toepassing zullen gebruiken. Als u uw toepassing in hun directory, kunt u een aanmeldingsproces voor uw klanten kunnen ze toe te staan uw toepassing opnemen. Wanneer zij zich voor uw toepassing aanmelden, zij krijgt een dialoogvenster waarin u de machtigingen vereist voor de toepassing en de optie toe te staan. Afhankelijk van de vereiste machtigingen een beheerder in de andere organisatie mogelijk moet toestemming geven. Wanneer de gebruiker of beheerder instemt, wordt de toepassing geregistreerd in hun directory. Zie [Integratie van toepassingen met Azure Active Directory](active-directory-integrating-applications.md)voor meer informatie.


#### <a name="token-expiration"></a>Verlopen van token

De sessie verloopt als de levensduur van het token is uitgegeven door AD Azure is verlopen. Uw toepassing kunt inkorten van deze periode, indien gewenst, zoals het afmelden van gebruikers op basis van een periode van inactiviteit. Wanneer de sessie is verlopen, wordt de gebruiker gevraagd opnieuw inloggen.





### <a name="single-page-application-spa"></a>Toepassing van één pagina (SPA)

Deze sectie beschrijft de verificatie voor een enkele pagina toepassing die gebruik Azure AD en OAuth 2.0 impliciete vergunning verlenen beveiliging van het web API weer beëindigen. Toepassingen van één pagina worden meestal gestructureerd als JavaScript presentatie laag (front-end) die wordt uitgevoerd in de browser en een Web API back-end die wordt uitgevoerd op een server en wordt de bedrijfslogica van de toepassing wordt geïmplementeerd. Zie [Wat de stroom OAuth2 impliciete verlening in Azure Active Directory](active-directory-dev-understanding-oauth2-implicit-grant.md)voor meer informatie over de impliciete toestemming verlenen en help u bepalen of deze geschikt voor uw scenario toepassing is.

In dit scenario is wanneer de gebruiker zich aanmeldt, de JavaScript front eindbestemmingen [Active Directory-verificatie Library voor JavaScript (ADAL. JS)](https://github.com/AzureAD/azure-activedirectory-library-for-js/tree/dev) en de impliciete toestemming verlenen voor een token ID (id_token) van Azure AD. Het token wordt in de cache en de client gekoppeld aan de aanvraag als het token aan toonder het aanroepen van de Web-API back-end, die is beveiligd met de OWIN middleware. 
#### <a name="diagram"></a>Diagram

![Diagram met één pagina toepassing](./media/active-directory-authentication-scenarios/single_page_app.png)

#### <a name="description-of-protocol-flow"></a>Beschrijving van de stroom van Protocol

1. De gebruiker gaat naar de webtoepassing.


2. De toepassing geeft als resultaat de JavaScript-front-end (presentatie laag) naar de browser.


3. De gebruiker start log in, bijvoorbeeld door een teken in de koppeling te klikken. GET verzendt de browser naar het eindpunt Azure AD vergunning voor het aanvragen van een ID-token. Deze aanvraag bevat de client-ID en het antwoord op de URL in de queryparameters.


4. Azure AD valideert de URL van het antwoord tegen het geregistreerde URL antwoord dat is geconfigureerd in de Azure Management Portal.


5. De gebruiker zich aanmeldt op de pagina aanmelden.


6. Als de verificatie is geslaagd, Azure AD een ID-token wordt gemaakt en wordt het als een URL-fragment (#) antwoord-URL van de toepassing. Voor de toepassing van een productieorder moet deze URL antwoord HTTPS. Het resulterende token bevat over de gebruikers- en Azure AD claims die voor de toepassing vereist zijn voor het valideren van het token.


7. De JavaScript-clientcode uitgevoerd in de browser haalt het token uit het antwoord moet worden gebruikt in de aanroepen van de toepassing web dat API back end beveiliging.


8. De browser van de toepassing web API weer eindigen met het toegangstoken in de koptekst van de vergunning wordt aangeroepen.

#### <a name="code-samples"></a>Codevoorbeelden


Zie de voorbeelden van code voor de toepassing van één pagina (SPA) scenario's. Controleer regelmatig terug--we voortdurend nieuwe monsters toevoegen. [Toepassing van één pagina (SPA)](active-directory-code-samples.md#single-page-application-spa).


#### <a name="registering"></a>Registreren


- Één huurder: Als u een toepassing speciaal voor uw organisatie bouwt, het moet worden geregistreerd in de map van uw bedrijf met behulp van de Portal Azure Management.


- Meerdere huurder: Als u een toepassing die door gebruikers buiten uw organisatie kan worden gebruikt, deze moet worden geregistreerd in de map van uw bedrijf, maar ook moet worden geregistreerd in de map van elke organisatie die de toepassing zullen gebruiken. Als u uw toepassing in hun directory, kunt u een aanmeldingsproces voor uw klanten kunnen ze toe te staan uw toepassing opnemen. Wanneer zij zich voor uw toepassing aanmelden, zij krijgt een dialoogvenster waarin u de machtigingen vereist voor de toepassing en de optie toe te staan. Afhankelijk van de vereiste machtigingen een beheerder in de andere organisatie mogelijk moet toestemming geven. Wanneer de gebruiker of beheerder instemt, wordt de toepassing geregistreerd in hun directory. Zie [Integratie van toepassingen met Azure Active Directory](active-directory-integrating-applications.md)voor meer informatie.

Na de registratie van de toepassing, moet deze worden geconfigureerd om impliciete verlening van OAuth 2.0-protocol te gebruiken. Dit protocol is standaard uitgeschakeld voor toepassingen. Het manifest van de toepassing downloaden vanaf de Portal Azure Management zodat de impliciete verlening van OAuth2-protocol voor uw toepassing de waarde 'oauth2AllowImplicitFlow' ingesteld op true en upload het manifest terug naar de portal. Zie voor gedetailleerde instructies [Zodat OAuth 2.0 impliciete subsidie voor toepassingen met één pagina](active-directory-integrating-applications.md).


#### <a name="token-expiration"></a>Verlopen van token

Wanneer u ADAL.js gebruikt voor het beheren van verificatie met Azure AD, profiteert u van diverse functies die een token verlopen te vernieuwen, alsmede tokens ophalen voor extra API webbronnen die kunnen worden aangeroepen door de toepassing te vergemakkelijken. Wanneer de gebruiker is geverifieerd met Azure Active Directory, een sessie beveiligd door een cookie wordt tot stand gebracht voor de gebruiker tussen de browser en Azure AD. Het is belangrijk te weten dat de sessie tussen de gebruiker en Azure AD en niet tussen de gebruiker en de webtoepassing die wordt uitgevoerd op de server bestaat. Als een token verlopen, wordt ADAL.js deze sessie zonder bericht een andere token verkrijgen. Dit gebeurt met behulp van een verborgen iFrame te verzenden en ontvangen van de aanvraag met het impliciete subsidie OAuth-protocol. ADAL.js kunt ook hetzelfde mechanisme zonder bericht ophalen toegangstokens van Azure AD voor andere web API aanroepen van de toepassing, zolang deze bronnen ondersteuning cross oorsprong resource sharing (CORS), worden geregistreerd in de map van de gebruiker en eventuele vereiste toestemming is gegeven door de gebruiker bij het aanmelden.


### <a name="native-application-to-web-api"></a>Oorspronkelijke toepassing web API


Deze sectie beschrijft een native-toepassing die een web API voor een gebruiker wordt aangeroepen. In dit scenario is gebaseerd op het OAuth 2.0 vergunning verlenen codetype met een openbare-client, zoals beschreven in punt 4.1 van de [OAuth 2.0-specificatie](http://tools.ietf.org/html/rfc6749). Een toegangstoken voor de gebruiker verkrijgt de oorspronkelijke toepassing via het protocol OAuth 2.0. Dit toegangstoken wordt vervolgens verzonden in de aanvraag voor het web API, die de gebruiker toestaat en retourneert de gewenste resource.

#### <a name="diagram"></a>Diagram

![De toepassing waarmee het Web API-Diagram](./media/active-directory-authentication-scenarios/native_app_to_web_api.png)

#### <a name="authentication-flow-for-native-application-to-api"></a>Overdracht van verificatie voor native API-toepassing

#### <a name="description-of-protocol-flow"></a>Beschrijving van de stroom van Protocol


Als u de bibliotheken AD-verificatie gebruikt, worden de meeste van de protocoldetails die hieronder worden beschreven, zoals de pop-up browser token-caching en verwerking van tokens vernieuwen verwerkt.

1. Met behulp van een browser die pop-de oorspronkelijke toepassing in Azure AD tot aan het eindpunt van de vergunning een aanvraag indient. Deze aanvraag bevat de client-ID en het omleiden van de URI van de oorspronkelijke toepassing, zoals wordt weergegeven in de Portal beheren en de aanvraag-ID-URI voor het web API. Als de gebruiker nog niet is aangemeld, wordt gevraagd opnieuw inloggen


2. Azure AD verifieert de gebruiker. Als het is een toepassing met meerdere huurder toestemming is vereist voor het gebruik van de toepassing, de gebruiker moet toestemming als ze dat nog niet hebt gedaan. Na het verlenen van toestemming en na een geslaagde verificatie problemen Azure AD een vergunning code antwoord terug naar de clienttoepassing redirect URI.


3. Azure AD verleent een vergunning code antwoord terug naar de redirect URI, de clienttoepassing browser interactie stopt als de code van de vergunning wordt geëxtraheerd uit het antwoord. Met deze code van de vergunning, verzendt de clienttoepassing een verzoek naar Azure AD token eindpunt dat de code van de vergunning bevat, gedetailleerde informatie over de clienttoepassing (de client-ID en redirect URI) en de gewenste resource (toepassing URI-ID voor het web API).


4. De code van de vergunning en informatie over de API van toepassing en de web client worden gevalideerd door AD Azure. Na een succesvolle validatie Azure AD geeft twee tokens: een toegangstoken JWT en een token JWT vernieuwen. Bovendien Azure AD geeft als resultaat informatie over de gebruiker, zoals de weergegeven naam en huurder-ID.


5. Via HTTPS, de clienttoepassing gebruikmaakt van de geretourneerde JWT access token de tekenreeks JWT met een aanduiding "Houder" in de kop van de vergunning van het verzoek toevoegen aan het web API. Het web API valideert het token JWT, en als de validatie voltooid is, geeft de gewenste resource.


6. Wanneer het toegangstoken is verlopen, ontvangt de clienttoepassing een foutbericht weergegeven dat de gebruiker moet opnieuw worden geverifieerd. Als de toepassing een token geldig vernieuwen, kan het worden gebruikt ter verwerving van een nieuwe toegangstoken zonder tussenkomst van de gebruiker opnieuw inloggen. Als het token vernieuwen is verstreken, moet de toepassing interactief de gebruiker opnieuw verifiëren.


> [AZURE.NOTE] Het vernieuwen token uitgegeven door AD Azure kan worden gebruikt voor toegang tot meerdere bronnen. Bijvoorbeeld als er een clienttoepassing die gemachtigd is om te bellen twee web API's, kan het token vernieuwd kan worden gebruikt voor een toegang naar de andere web API ook token.


#### <a name="code-samples"></a>Codevoorbeelden


Zie de voorbeelden van code voor de toepassing waarmee de scenario's voor Web-API. En kom kijken vaak--we voortdurend nieuwe monsters toevoegen. De [oorspronkelijke toepassing web API](active-directory-code-samples.md#native-application-to-web-api).


#### <a name="registering"></a>Registreren


- Één huurder: Zowel de eigen toepassing en het web API moeten zijn geregistreerd in dezelfde map in Azure AD. Het web API kan worden geconfigureerd om een set machtigingen die worden gebruikt voor het beperken van de oorspronkelijke toepassing toegang tot de bronnen weer te geven. De clienttoepassing vervolgens selecteert u de gewenste machtigingen uit de vervolgkeuzelijst 'Machtigingen voor andere toepassingen' in Azure Management Portal.


- Meerdere huurder: Eerst de oorspronkelijke toepassing alleen ooit geregistreerd in de ontwikkelaar of uitgever van directory. Ten tweede, de oorspronkelijke toepassing is geconfigureerd om aan te geven van de machtigingen die nodig is voor het functioneel. Deze lijst met de vereiste machtigingen wordt in een dialoogvenster weergegeven wanneer een gebruiker of beheerder in de doelmap geeft toestemming voor de toepassing die beschikbaar zijn voor hun organisatie. Sommige toepassingen vereisen alleen op gebruikersniveau machtigingen die een gebruiker in de organisatie kan de instemming met. Andere toepassingen nodig beheerdersrechten machtigingen die een gebruiker in de organisatie kan geen toestemming voor. Alleen de beheerder van een directory kunt toestemming geven tot toepassingen waarvoor dit niveau van machtigingen. Wanneer de gebruiker of beheerder instemt, wordt het web API geregistreerd in hun directory. Zie [Integratie van toepassingen met Azure Active Directory](active-directory-integrating-applications.md)voor meer informatie.


#### <a name="token-expiration"></a>Verlopen van token


Wanneer de oorspronkelijke toepassing de code van de vergunning gebruikt voor een JWT toegang token, ontvangt deze ook een token JWT vernieuwen. Wanneer het toegangstoken verloopt, kan het token vernieuwen opnieuw de gebruiker te verifiëren zonder dat ze opnieuw worden gebruikt. Deze token vernieuwen wordt vervolgens gebruikt voor het verifiëren van de gebruiker, wat resulteert in een nieuwe access token en token vernieuwen.





### <a name="web-application-to-web-api"></a>Webtoepassing voor Web-API


Deze sectie beschrijft een webtoepassing die moet verkrijgen van de bronnen van een web-API. In dit scenario zijn twee identiteitstypen die de webtoepassing gebruiken kunt om te verifiëren en roept de API van het web: een toepassings-id of de identiteit van een gedelegeerde gebruiker.

*Toepassings-id:* In dit scenario gebruikt OAuth 2.0-client referenties subsidie worden geverifieerd als de toepassing en toegang tot het web API. Bij het gebruik van een toepassings-id, het web API kan alleen worden vastgesteld dat de webtoepassing wordt gebeld, als het web krijgt API geen informatie over de gebruiker. De toepassing ontvangt informatie over de gebruiker, wordt verzonden via het toepassingsprotocol als het niet is ondertekend door AD Azure. De web-API wordt vertrouwd dat de webtoepassing de gebruiker geverifieerd. Daarom wordt dit patroon een vertrouwd subsysteem genoemd.

*Overgedragen gebruikers-id:* In dit scenario kan op twee manieren worden uitgevoerd: verbinding OpenID en OAuth 2.0 vergunning code toekennen met vertrouwelijke-client. De webtoepassing verkrijgt een toegangstoken voor de gebruiker, blijkt dat het web API die de gebruiker geverifieerd aan de webtoepassing en de webtoepassing is een gedelegeerde gebruikersidentiteit het web API aanroepen krijgen. Dit toegangstoken wordt verzonden in de aanvraag voor het web API, die de gebruiker toestaat en retourneert de gewenste resource.

#### <a name="diagram"></a>Diagram

![Webtoepassing voor Web-API-diagram](./media/active-directory-authentication-scenarios/web_app_to_web_api.png)



#### <a name="description-of-protocol-flow"></a>Beschrijving van de stroom van Protocol

De toepassings-id en de overgedragen gebruiker identiteitstypen worden in de stroom die hieronder besproken. Het belangrijkste verschil tussen beide is dat de identiteit van de gedelegeerde gebruiker eerst een autorisatiecode aanschaffen moet voordat de gebruiker kunt aanmelden en toegang krijgen tot het web API.

##### <a name="application-identity-with-oauth-20-client-credentials-grant"></a>Toepassings-id met OAuth 2.0-Client referenties subsidie

1. Een gebruiker is aangemeld bij Azure AD in de webtoepassing (Zie de bovenstaande [Webbrowser Web Application](#web-browser-to-web-application) ).


2. De webtoepassing moet een toegangstoken verkrijgen, zodat deze kan op het web API verifiëren en ophalen van de gewenste resource. Het maakt een aanvraag naar Azure AD token eindpunt, de referentie, de client-ID en-API's webtoepassing URI-ID.


3. Azure AD verifieert de toepassing en geeft als resultaat een toegangstoken JWT die wordt gebruikt voor het web-API aanroept.


4. Via HTTPS gebruikt de webtoepassing de geretourneerde JWT access token de tekenreeks JWT met een aanduiding "Houder" in de kop van de vergunning van het verzoek toevoegen aan het web API. Het web API valideert het token JWT, en als de validatie voltooid is, geeft de gewenste resource.

##### <a name="delegated-user-identity-with-openid-connect"></a>OpenID identiteit overgedragen gebruiker verbinding

1. Een gebruiker is aangemeld bij een webtoepassing met Azure AD (Zie [Webbrowser webtoepassing](#web-browser-to-web-application) hierboven). Als de gebruiker van de webtoepassing niet nog heeft ingestemd met het toestaan van de webtoepassing op het web API aanroepen namens haar, moet de gebruiker toe te staan. De machtigingen die vereist door de toepassing wordt weergegeven en als een van de volgende beheerdersrechten, een normale gebruiker in Active directory niet mogelijk toe te staan. Hierbij toestemming geldt alleen voor toepassingen met meerdere huurder, huurder geen afzonderlijke toepassingen, zoals de toepassing beschikt al over de benodigde machtigingen. Wanneer de gebruiker heeft aangemeld, ontvangen de webtoepassing een token ID met informatie over de gebruiker, evenals een code van de vergunning.


2. De webtoepassing verzendt met de autorisatiecode uitgegeven door AD Azure, een verzoek naar Azure AD token eindpunt dat de code van de vergunning bevat, gedetailleerde informatie over de clienttoepassing (de client-ID en redirect URI) en de gewenste resource (toepassing URI-ID voor het web API).


3. De code van de vergunning en informatie over de webtoepassing en de web-API worden gevalideerd door AD Azure. Na een succesvolle validatie Azure AD geeft twee tokens: een toegangstoken JWT en een token JWT vernieuwen.


4. Via HTTPS gebruikt de webtoepassing de geretourneerde JWT access token de tekenreeks JWT met een aanduiding "Houder" in de kop van de vergunning van het verzoek toevoegen aan het web API. Het web API valideert het token JWT, en als de validatie voltooid is, geeft de gewenste resource.

##### <a name="delegated-user-identity-with-oauth-20-authorization-code-grant"></a>Overgedragen gebruikersidentiteit bij OAuth 2.0 vergunning Code subsidie

1. Een gebruiker is al aangemeld bij een webtoepassing waarvan verificatiemechanisme onafhankelijk van Azure AD is.


2. De webtoepassing is een autorisatiecode te verwerven van een toegangstoken zodat het een aanvraag via de browser naar Azure AD vergunning eindpunt verzendt, die de client-ID en redirect URI voor de webtoepassing na een succesvolle verificatie vereist. De gebruiker zich aanmeldt bij AD Azure.


3. Als de gebruiker van de webtoepassing niet nog heeft ingestemd met het toestaan van de webtoepassing op het web API aanroepen namens haar, moet de gebruiker toe te staan. De machtigingen die vereist door de toepassing wordt weergegeven en als een van de volgende beheerdersrechten, een normale gebruiker in Active directory niet mogelijk toe te staan. Hierbij toestemming geldt alleen voor toepassingen met meerdere huurder, huurder geen afzonderlijke toepassingen, zoals de toepassing beschikt al over de benodigde machtigingen.


4. Nadat de gebruiker heeft ingestemd, ontvangt de webtoepassing de autorisatiecode die nodig zijn voor het verkrijgen van een toegangstoken.


5. De webtoepassing verzendt met de autorisatiecode uitgegeven door AD Azure, een verzoek naar Azure AD token eindpunt dat de code van de vergunning bevat, gedetailleerde informatie over de clienttoepassing (de client-ID en redirect URI) en de gewenste resource (toepassing URI-ID voor het web API).


6. De code van de vergunning en informatie over de webtoepassing en de web-API worden gevalideerd door AD Azure. Na een succesvolle validatie Azure AD geeft twee tokens: een toegangstoken JWT en een token JWT vernieuwen.


7. Via HTTPS gebruikt de webtoepassing de geretourneerde JWT access token de tekenreeks JWT met een aanduiding "Houder" in de kop van de vergunning van het verzoek toevoegen aan het web API. Het web API valideert het token JWT, en als de validatie voltooid is, geeft de gewenste resource.

#### <a name="code-samples"></a>Codevoorbeelden

Zie de voorbeelden van code voor webtoepassing voor scenario's voor Web-API. En kom kijken vaak--we voortdurend nieuwe monsters toevoegen. Web [API Web toepassing](active-directory-code-samples.md#web-application-to-web-api).


#### <a name="registering"></a>Registreren

- Één huurder: Voor de toepassings-id en de overgedragen gebruiker identiteit gevallen, de webtoepassing en de web-API moeten worden geregistreerd in dezelfde map in Azure AD. Het web API kan worden geconfigureerd om een set machtigingen die worden gebruikt voor het beperken van toegang tot de bronnen van de webtoepassing zichtbaar te maken. Als een identiteitstype overgedragen gebruiker wordt gebruikt, moet de webtoepassing selecteren de gewenste machtigingen in de vervolgkeuzelijst 'Machtigingen voor andere toepassingen' in Azure Management Portal. Deze stap is niet vereist als het type toepassing identiteit wordt gebruikt.


- Meerdere huurder: Ten eerste de webtoepassing is geconfigureerd om aan te geven van de machtigingen die nodig is voor het functioneel. Deze lijst met de vereiste machtigingen wordt in een dialoogvenster weergegeven wanneer een gebruiker of beheerder in de doelmap geeft toestemming voor de toepassing die beschikbaar zijn voor hun organisatie. Sommige toepassingen vereisen alleen op gebruikersniveau machtigingen die een gebruiker in de organisatie kan de instemming met. Andere toepassingen nodig beheerdersrechten machtigingen die een gebruiker in de organisatie kan geen toestemming voor. Alleen de beheerder van een directory kunt toestemming geven tot toepassingen waarvoor dit niveau van machtigingen. Wanneer de gebruiker of beheerder instemt, zijn de webtoepassing en de web-API beide geregistreerd in hun directory.

#### <a name="token-expiration"></a>Verlopen van token

Als de webtoepassing de code van de vergunning gebruikt voor een JWT toegang token, ontvangt deze ook een token JWT vernieuwen. Wanneer het toegangstoken verloopt, kan het token vernieuwen opnieuw de gebruiker te verifiëren zonder dat ze opnieuw worden gebruikt. Deze token vernieuwen wordt vervolgens gebruikt voor het verifiëren van de gebruiker, wat resulteert in een nieuwe access token en token vernieuwen.


### <a name="daemon-or-server-application-to-web-api"></a>Daemon- of servertoepassing met Web API


Deze sectie beschrijft een daemon of server-toepassing die bronnen ophalen uit een web API. Er zijn twee subtypen scenario's die betrekking hebben op dit punt: een daemon die nodig zijn om aan te roepen van een web-API gebaseerd op OAuth 2.0 referenties subsidie clienttype; en een server-toepassing (zoals een web API) die nodig zijn om aan te roepen van een web-API gebaseerd op conceptspecificatie OAuth 2.0 On-Behalf-Of.

Voor het scenario wanneer een daemontoepassing moet aanroepen van een web-API is het belangrijk een aantal zaken begrijpt. Ten eerste is interactie van de gebruiker niet mogelijk met een daemontoepassing waarvoor u de toepassing op een eigen identiteit hebben. Een voorbeeld van een daemontoepassing is een batchverwerking, of een besturingssysteemservice die op de achtergrond uitgevoerd. Dit type toepassing vraagt om een toegangstoken met behulp van de toepassings-id en presenteren van de client-ID en referenties (wachtwoord of certificaat) toepassing URI naar Azure AD-ID. Na een succesvolle verificatie ontvangt de daemon een toegangstoken Azure AD, die vervolgens wordt gebruikt om aan te roepen van het web API.

Voor het scenario wanneer een servertoepassing moet aanroepen van een web API, is het handig een voorbeeld wilt gebruiken. Stel dat een gebruiker is geverifieerd op een oorspronkelijke toepassing en deze oorspronkelijke toepassing moet een web API aanroept. Azure AD problemen een toegangstoken JWT het web API aanroepen. Als de web-API, een andere downstream web API aanroept moet, deze de stroom op-rekening-of de identiteit van de gebruiker te delegeren en kunt verifiëren bij de tweede laag web API.

#### <a name="diagram"></a>Diagram

![Daemon of servertoepassing Web API-diagram](./media/active-directory-authentication-scenarios/daemon_server_app_to_web_api.png)

#### <a name="description-of-protocol-flow"></a>Beschrijving van de stroom van Protocol

##### <a name="application-identity-with-oauth-20-client-credentials-grant"></a>Toepassings-id met OAuth 2.0-Client referenties subsidie

1. De servertoepassing moet eerst worden geverifieerd bij Azure AD als zelf, zonder menselijke tussenkomst van bijvoorbeeld een dialoogvenster voor interactieve aanmelding. Het maakt een aanvraag naar Azure AD token eindpunt, de referenties, de client-ID en aanvraag-ID-URI.


2. Azure AD verifieert de toepassing en geeft als resultaat een toegangstoken JWT die wordt gebruikt voor het web-API aanroept.


3. Via HTTPS gebruikt de webtoepassing de geretourneerde JWT access token de tekenreeks JWT met een aanduiding "Houder" in de kop van de vergunning van het verzoek toevoegen aan het web API. Het web API valideert het token JWT, en als de validatie voltooid is, geeft de gewenste resource.


##### <a name="delegated-user-identity-with-oauth-20-on-behalf-of-draft-specification"></a>OAuth 2.0 op-rekening-Of conceptspecificatie overgedragen gebruikersidentiteit

De stroom die hieronder worden beschreven, wordt ervan uitgegaan dat een gebruiker is geverifieerd op een andere toepassing (zoals een eigen toepassing) en de identiteit van de gebruiker is gebruikt voor het verkrijgen van een toegangstoken op de eerste laag web API.

1. De oorspronkelijke toepassing stuurt het toegangstoken op de eerste laag web API.


2. De eerste laag web API verzendt een verzoek naar Azure AD token eindpunt, die de client-ID en referenties, evenals het toegangstoken van de gebruiker. Bovendien wordt de aanvraag verzonden met een on_behalf_of parameter die aangeeft van het web API vraagt om nieuwe tokens voor de oorspronkelijke gebruiker een downstream web API aanroepen.


3. Azure AD wordt gecontroleerd of het eerste tier web API beschikt over machtigingen voor toegang tot de tweede laag web API en beoordeelt de aanvraag, een toegangstoken JWT retourneren een JWT token op de eerste laag web API vernieuwen.


4. Via HTTPS, de eerste laag web API vervolgens de tweede laag web API aangeroepen door de token-reeks in de koptekst van de vergunning in de aanvraag toe te voegen. De eerste laag web API kunt blijven de tweede laag web API aanroepen, zolang de toegangstoken en vernieuwen tokens geldig zijn.

#### <a name="code-samples"></a>Codevoorbeelden

Zie de voorbeelden van code voor Daemon- of servertoepassing met scenario's voor Web-API. En kom kijken vaak--we voortdurend nieuwe monsters toevoegen. [Server of daemontoepassing op Web API](active-directory-code-samples.md#server-or-daemon-application-to-web-api)

#### <a name="registering"></a>Registreren

- Één huurder: Voor de toepassings-id en de overgedragen gebruiker identiteit gevallen, de daemon- of servertoepassing moet worden geregistreerd in dezelfde map in Azure AD. Het web API kan worden geconfigureerd om een set machtigingen die worden gebruikt voor het beperken van de daemon of toegang tot de bronnen van de server weer te geven. Als een identiteitstype overgedragen gebruiker wordt gebruikt, moet de servertoepassing uit de vervolgkeuzelijst 'Machtigingen voor andere toepassingen' in Azure Management Portal selecteert u de gewenste machtigingen. Deze stap is niet vereist als het type toepassing identiteit wordt gebruikt.


- Meerdere huurder: Eerst de daemon- of servertoepassing is geconfigureerd om aan te geven van de machtigingen die nodig is voor het functioneel. Deze lijst met de vereiste machtigingen wordt in een dialoogvenster weergegeven wanneer een gebruiker of beheerder in de doelmap geeft toestemming voor de toepassing die beschikbaar zijn voor hun organisatie. Sommige toepassingen vereisen alleen op gebruikersniveau machtigingen die een gebruiker in de organisatie kan de instemming met. Andere toepassingen nodig beheerdersrechten machtigingen die een gebruiker in de organisatie kan geen toestemming voor. Alleen de beheerder van een directory kunt toestemming geven tot toepassingen waarvoor dit niveau van machtigingen. Wanneer de gebruiker of beheerder instemt, beide van de web-API's in hun directory geregistreerd.

#### <a name="token-expiration"></a>Verlopen van token

Als de eerste toepassing de code van de vergunning gebruikt voor een JWT toegang token, ontvangt deze ook een token JWT vernieuwen. Wanneer het toegangstoken verloopt, kan het token vernieuwen worden gebruikt voor de verificatie van de gebruiker opnieuw zonder te vragen om referenties. Deze token vernieuwen wordt vervolgens gebruikt voor het verifiëren van de gebruiker, wat resulteert in een nieuwe access token en token vernieuwen.

## <a name="see-also"></a>Zie ook

[Azure Active Directory Developer's Guide](active-directory-developers-guide.md)

[Codevoorbeelden Azure Active Directory](active-directory-code-samples.md)

[Belangrijke informatie over het ondertekenen van sleutels Rollover in Azure AD](active-directory-signing-key-rollover.md)

[OAuth 2.0 in Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx)
