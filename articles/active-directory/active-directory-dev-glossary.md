<properties
   pageTitle="Azure Active Directory Developer woordenlijst | Microsoft Azure"
   description="Een lijst van termen voor vaak gebruikte Azure Active Directory developer concepten en functies."
   services="active-directory"
   documentationCenter=""
   authors="bryanla"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/31/2016"
   ms.author="bryanla"/>

# <a name="azure-active-directory-developer-glossary"></a>Azure Active Directory developer-verklarende woordenlijst
Dit artikel bevat de definities voor enkele van de belangrijkste Azure Active Directory (AD) developer begrippen, die handig zijn als u meer informatie over het ontwikkelen van toepassingen voor AD Azure.

## <a name="access-token"></a>toegangstoken
Een type [beveiligingstoken](#security-token) uitgegeven door een [vergunning server](#authorization-server)en voor toegang tot een [beveiligde server resource](#resource-server)wordt gebruikt door een [clienttoepassing](#client-application) . Meestal in de vorm van een [JSON Web Token (JWT)][JWT], het token naar de client door de [eigenaar van de bron](#resource-owner)voor het gewenste toegangsniveau verleende vergunning belichaamd. Het token bevat alle van toepassing zijnde [claims](#claim) over het onderwerp, zodat de clienttoepassing te gebruiken als referentie bij de toegang tot een bepaalde bron. Hierdoor wordt ook de noodzaak van de eigenaar van de resource om referenties naar de client beschikbaar te maken.

Toegangstokens worden ook genoemd als 'Gebruiker + App' of 'App-Only', afhankelijk van de referenties die wordt weergegeven. Wanneer een clienttoepassing gebruikt bijvoorbeeld de:

- ["Autorisatiecode" vergunning verlenen](#authorization-grant), verifieert de gebruiker eerst als de eigenaar van de resource delegeren van de vergunning voor de client toegang tot de bron. De client verifieert achteraf bij het verkrijgen van het toegangstoken. Het token kan soms worden genoemd meer in het bijzonder een token 'Gebruiker + App' als de gebruiker die gemachtigd de clienttoepassing en de toepassing vertegenwoordigt.
- ["Clientreferenties" vergunning verlenen](#authorization-grant), de client biedt de enige verificatie functioneren zonder vergunning/resource-eigenaar van de verificatie, zodat de token kan soms worden verwezen als een token 'App-Only'.

[Azure AD Token] Zie[ AAD-Tokens-Claims] voor meer informatie.

## <a name="application-manifest"></a>manifest van de toepassing  
Een functie van de [Azure klassieke portal][AZURE-classic-portal], waardoor een JSON representatie van de identiteit de configuratie van toepassing, gebruikt als een mechanisme voor het bijwerken van de bijbehorende [toepassing] wordt[ AAD-Graph-App-Entity] en [ServicePrincipal] [ AAD-Graph-Sp-Entity] entiteiten. [Inzicht in het toepassingsmanifest Azure Active Directory] Zie[ AAD-App-Manifest] voor meer informatie.

## <a name="application-object"></a>Application-object  
Wanneer u registreren of bij te werken een toepassing in de [Azure klassieke portal][AZURE-classic-portal], de portal maakt/updates zowel een application-object en een bijbehorende [service principal-object](#service-principal-object) voor deze huurder. Het application-object *definieert* de toepassing de identiteit configuratie globaal (voor alle huurders waar deze toegang heeft), bieden een sjabloon waarvan de bijbehorende service principal objecten worden *afgeleid* voor gebruik tijdens runtime (in een bepaalde huurder) lokaal.

[Toepassing en Service Principal objecten] Zie[ AAD-App-SP-Objects] voor meer informatie.

## <a name="application-registration"></a>registratie van toepassingen  
Om een toepassing integreren en Azure AD Identity and Access Management functies delegeren, moet deze geregistreerd zijn bij een Azure AD [huurder](#tenant). Wanneer u uw toepassing met Azure Active Directory hebt geregistreerd, bieden u de configuratie van een identiteit voor uw toepassing, te integreren met AD Azure en functies gebruiken, zoals:

- Krachtige beheer van eenmalige aanmelding met Azure AD identiteits- en [OpenID verbinding] [ OpenIDConnect] protocol implementatie
- Brokered toegang tot [beveiligde bronnen](#resource-server) door [clienttoepassingen](#client-application)via Azure AD OAuth 2.0 [vergunning server](#authorization-server) -implementatie
- [Instemming raamwerk](#consent) voor het beheren van de toegang van clients tot beveiligde bronnen, op basis van verificatie van de bron eigenaar.

[Integratie van toepassingen met Azure Active Directory] Zie[ AAD-Integrating-Apps] voor meer informatie.

## <a name="authentication"></a>verificatie
Het besluit om een partij voor betrouwbare referenties, die de basis voor het maken van een beveiligings-principal worden gebruikt voor identiteits- en toegangsbeheer te bestrijden. Tijdens een [OAuth2 vergunning verlenen](#authorization-grant) , bijvoorbeeld invullen de partij verifiëren, de rol van de [eigenaar van de bron](#resource-owner) of [clienttoepassing](#client-application), afhankelijk van de subsidie gebruikt.

## <a name="authorization"></a>autorisatie
De handeling van het verlenen van een geverifieerde security principal toestemming om iets te doen. Er zijn twee primaire use-cases in het AD Azure programmeermodel:

- Tijdens een [OAuth2 vergunning verlenen](#authorization-grant) stroom: als de [bron eigenaar](#resource-owner) toestemming om de [clienttoepassing verleent](#client-application), zodat de client toegang krijgen tot de bronnen van de eigenaar van de resource.
- Tijdens de toegang tot bronnen door de client: door de [bronserver](#resource-server)worden geïmplementeerd, met behulp van de [claim](#claim) waarden aanwezig in het [toegangstoken](#access-token) voor toegang tot het toegangsbeheer op basis van deze.

## <a name="authorization-code"></a>validatiecode
Een korte levensduur "token" die een [clienttoepassing](#client-application) van het [eindpunt van de vergunning](#authorization-endpoint), als onderdeel van de stroom "autorisatiecode" een van de vier OAuth2 [vergunning verleent](#authorization-grant). De code wordt geretourneerd naar de clienttoepassing in reactie op de verificatie van een [eigenaar van de bron](#resource-owner), die dat de eigenaar van de resource machtiging voor toegang tot de aangevraagde bronnen heeft overgedragen. De code wordt als onderdeel van de stroom, later ingewisseld voor een [toegangstoken](#access-token).

## <a name="authorization-endpoint"></a>eindpunt van de vergunning
Een van de eindpunten geïmplementeerd door de [server verificatie](#authorization-server)gebruikt voor interactie met de [eigenaar van de resource](#resource-owner) om een [vergunning verlenen](#authorization-grant) tijdens een OAuth2 vergunning verlenen stroom. Variëren afhankelijk van de vergunning verlenen stroom gebruikt, de werkelijke subsidie verstrekt kan, met inbegrip van een [autorisatiecode](#authorization-code) of het [beveiligingstoken](#security-token).

Zie de OAuth2-specificatie, [vergunning verlenen typen] [ OAuth2-AuthZ-Grant-Types] en het [eindpunt van de vergunning] [ OAuth2-AuthZ-Endpoint] secties en de [specificatie van OpenIDConnect] [ OpenIDConnect-AuthZ-Endpoint] voor meer informatie.

## <a name="authorization-grant"></a>verlening van de vergunning
Een referentie die de [bron van de eigenaar](#resource-owner) [toestemming](#authorization) toegang te krijgen tot de beveiligde bronnen, toegekend aan een [clienttoepassing](#client-application). Een clienttoepassing kunt gebruiken op een van de [vier subsidietypen die zijn gedefinieerd in het kader van de vergunning OAuth2] [ OAuth2-AuthZ-Grant-Types] voor een subsidie, afhankelijk van de clientvereisten type: "verlenen vergunning code", "clientreferenties verlenen" 'impliciete subsidie' en "resource eigenaar wachtwoordreferenties verlenen". De referenties naar de client teruggestuurd is een [toegangstoken](#access-token)of een [autorisatiecode](#authorization-code) (uitgewisseld later voor een toegangstoken), afhankelijk van het type vergunning verlenen gebruikt.

## <a name="authorization-server"></a>verificatie server
Zoals gedefinieerd door de [Vergunning-Framework OAuth2][OAuth2-Role-Def], de server die verantwoordelijk is voor het verlenen van toegang naar de [client](#client-application) worden tokens na met succes verifieert de [eigenaar van de bron](#resource-owner) en het verkrijgen van de vergunning. Een [clienttoepassing](#client-application) communiceert met de server van de vergunning bij uitvoering via de [vergunning](#authorization-endpoint) en [token](#token-endpoint) eindpunten, volgens de OAuth2 gedefinieerd [vergunning verleent](#authorization-grant).

Voor de integratie van Azure AD, Azure AD implementeert de serverrol van de vergunning voor Azure AD-toepassingen en Microsoft service API's, bijvoorbeeld [Microsoft Graph API's][Microsoft-Graph].

## <a name="claim"></a>claim
Een [beveiligingstoken](#security-token) bevat de claims die verklaringen over een entiteit (bijvoorbeeld een [clienttoepassing](#client-application) of een [resource eigenaar](#resource-owner)) aan een andere entiteit (bijvoorbeeld de [bronserver](#resource-server)). Claims zijn naam/waarde-paren die feiten over het token onderwerp (bijvoorbeeld de beveiligings-principal die is geverifieerd door de [server van de vergunning](#authorization-server)) sturen. De claims in een bepaalde token aanwezig zijn afhankelijk van diverse variabelen, waaronder het type token, het type van de referenties die worden gebruikt voor het verifiëren van het onderwerp, de configuratie van toepassingen, enz.

Zie [AD Azure token] [ AAD-Tokens-Claims] voor meer informatie.

## <a name="client-application"></a>clienttoepassing  
Zoals gedefinieerd door de [Vergunning-Framework OAuth2][OAuth2-Role-Def], een toepassing die beveiligde aanvragen namens de [eigenaar van een resource](#resource-owner)resource. De term 'client' betekent niet dat een bepaalde hardware implementatie kenmerken (bijvoorbeeld, of de toepassing wordt uitgevoerd op een server, een desktop of andere apparaten).  

Een clienttoepassing vraagt [toestemming](#authorization) van de eigenaar van een resource om deel te nemen in een stroom [OAuth2 vergunning verlenen](#authorization-grant) en toegang tot API's / gegevens namens de eigenaar van de resource. De OAuth2 vergunning Framework [definieert twee typen clients][OAuth2-Client-Types], 'vertrouwelijk' en 'public', op basis van de mogelijkheid te handhaven van de vertrouwelijkheid van de referenties van de client. Toepassingen kunnen implementeren een [webclient (vertrouwelijke)](#web-client) op een webserver, een [native client (openbare)](#native-client) zijn geïnstalleerd op een apparaat of een [client op basis van gebruiker-agent (openbare)](#user-agent-based-client) wordt uitgevoerd in de browser van een apparaat wordt uitgevoerd.

## <a name="consent"></a>toestemming
Het proces van een [resource eigenaar](#resource-owner) verlening van vergunning voor een [clienttoepassing](#client-application), specifieke [machtigingen](#permissions) voor toegang tot beveiligde bronnen namens de eigenaar van de resource. Afhankelijk van de machtigingen die door de client aangevraagde als beheerder of een gebruiker toe te staan om toegang te verlenen tot hun organisatie/individuele gegevens respectievelijk wordt gevraagd. Let op, in een scenario [met meerdere pachters](#multi-tenant-application) van de toepassing, [service principal](#service-principal-object) ook is vastgelegd in de pachter van de gebruiker consenting.

## <a name="id-token"></a>Id-token
Een [OpenID verbinding] [ OpenIDConnect-ID-Token] [-beveiligingstoken](#security-token) dat is opgegeven door een [vergunning van de server](#authorization-server) [eindpunt vergunning](#authorization-endpoint), waarin de [vorderingen](#claim) met betrekking tot de verificatie van een gebruiker [eigenaar van de resource](#resource-owner). ID-tokens worden net als een toegangstoken ook weergegeven als een digitaal ondertekende [JSON Web Token (JWT)][JWT]. In tegenstelling tot een toegangstoken echter een ID-token schuldvorderingen niet worden gebruikt voor doeleinden die betrekking hebben op de toegang tot bronnen en toegangsbeheer specifiek.

Zie [AD Azure token] [ AAD-Tokens-Claims] voor meer informatie.

## <a name="multi-tenant-application"></a>toepassingen met meerdere huurder
Aanmelden voor een [clienttoepassing](#client-application) waarmee klasse en ingericht [toestemming](#consent) door gebruikers in een Azure AD [pachter](#tenant), met inbegrip van huurders dan die waar de client is geregistreerd. Daarentegen, kunnen een toepassing die is geregistreerd als single-huurder alleen aanmeldingen van gebruikersaccounts in de huurder hetzelfde als de ingericht waar de aanvraag is geregistreerd. [Native client](#native-client) -toepassingen zijn meerdere huurder standaard dat [WebClient](#web-client) -toepassingen de mogelijkheid te kiezen tussen één en meerdere huurder hebben.

[Aanmelden van gebruikers met de toepassing van meerdere huurder patroon Azure AD] Zie[ AAD-Multi-Tenant-Overview] voor meer informatie.

## <a name="native-client"></a>native client
Een type [clienttoepassing](#client-application) die standaard is geïnstalleerd op een apparaat. Aangezien alle code wordt uitgevoerd op een apparaat, wordt een "publiek" client vanwege het feit dat voor het opslaan van referenties privé/vertrouwelijk beschouwd. [OAuth2 clienttypen en profielen] Zie[ OAuth2-Client-Types] voor meer informatie.

## <a name="permissions"></a>machtigingen
Een [clienttoepassing](#client-application) krijgt toegang tot een [server resource](#resource-server) door het declareren van verzoeken voor toestemming. Er zijn twee typen beschikbaar:

- "Gedelegeerde" machtigingen die toegang onder [scope gebaseerde](#scopes) verzoeken overgedragen verificatie van handtekening in van de [eigenaar van de resource](#resource-owner), als ["scp" vorderingen](#claim) van de client [access token](#access-token)aan de resource tijdens runtime worden weergegeven.
- Machtigingen voor 'Application', die [op rollen gebaseerde](#roles) toegang onder referenties/identiteit van de clienttoepassing vraagt, worden aan de resource tijdens runtime als ['rollen' vorderingen](#claim) in het toegangstoken van de client weergegeven.

Ze ook oppervlak tijdens de [toestemming](#consent) geven de beheerder of de eigenaar van de bron kunt u de clienttoegang tot bronnen in hun huurder toestaan/weigeren.

Verzoeken voor toestemming zijn geconfigureerd op de "toepassingen" / "Configureren" tab in de [Azure klassieke portal][AZURE-classic-portal], onder 'machtigingen voor andere toepassingen", door te selecteren de gewenste"overgedragen machtigingen"en"Machtigingen"(de laatste vereist lidmaatschap van de rol van globale beheerder). Omdat een [openbare-client](#client-application) kan geen referenties te behouden, kan deze alleen overgedragen machtigingen aanvragen terwijl een [vertrouwelijke client](#client-application) heeft een aanvraag voor overgedragen en machtigingen voor een toepassing. Van de client [application-object](#application-object) de opgegeven machtigingen worden opgeslagen in de [eigenschap requiredResourceAccess][AAD-Graph-App-Entity].

## <a name="resource-owner"></a>de eigenaar van de bron
Zoals gedefinieerd door de [Vergunning-Framework OAuth2][OAuth2-Role-Def], een entiteit die toegang verlenen tot een beveiligde bron kan worden. Wanneer de eigenaar van de resource een persoon is, is het bedoeld als een eindgebruiker. Bijvoorbeeld wanneer een [clienttoepassing](#client-application) toegang wil tot de postbus van de gebruiker via de [API van Microsoft Graph][Microsoft-Graph], moet de machtiging eigenaar van de bron van de postbus.

## <a name="resource-server"></a>bronserver
Zoals gedefinieerd door de [Vergunning-Framework OAuth2][OAuth2-Role-Def], een server of hosts beveiligd bronnen kunnen accepteren en het reageren op verzoeken van resource om beschermd door [clienttoepassingen](#client-application) die een [toegangstoken](#access-token). Ook bekend als een beveiligde bronserver of brontoepassing.

Een server resource worden API's en toegang tot de beveiligde bronnen via [scopes](#scopes) en [rollen](#roles), met het Framework OAuth 2.0 vergunning. Voorbeelden zijn de Azure AD Graph API die toegang biedt tot gegevens van AD Azure huurder en de Office 365-API's die toegang bieden tot gegevens, zoals e-mail en agenda. Deze zijn ook toegankelijk via de [Microsoft Graph API][Microsoft-Graph].  

Net als een clienttoepassing configuratie van de identiteit van de toepassing van de resource tot stand is gebracht via de [registratie](#application-registration) in een huurder Azure AD, bieden de toepassing en de belangrijkste service-object. Sommige geleverde Microsoft-API's, zoals de Azure AD Graph API, vooraf service beveiligings-principals in alle huurders beschikbaar gemaakt tijdens het inrichten ingeschreven.

## <a name="roles"></a>rollen
Zoals [scopes](#scopes)kunnen rollen u voor een [resource-server](#resource-server) voor de toegang tot de beveiligde bronnen. Er zijn twee typen: een "gebruiker" rol implementeert op rollen gebaseerde toegangscontrole voor gebruikers/groepen die toegang tot de bron, vereisen terwijl een rol 'application' implementeert voor [clienttoepassingen](#client-application) nodig hebben.

Rollen zijn tekenreeksen resource gedefinieerd (bijvoorbeeld "last fiatteur', 'Alleen-lezen', 'Directory.ReadWrite.All'), beheerde in [Azure klassieke portal] [ AZURE-classic-portal] via de bron van het [manifest van de toepassing](#application-manifest), en opgeslagen in de bron van [de eigenschap appRoles][AAD-Graph-Sp-Entity]. De klassieke Azure portal wordt ook gebruikt voor gebruikers toewijzen aan rollen 'gebruiker' en configureer client [machtigingen](#permissions) voor toegang tot een rol 'application'.

Zie voor een uitgebreide bespreking van de toepassingsrollen blootgesteld door Azure AD Graph API [Graph API machtiging Scopes][AAD-Graph-Perm-Scopes]. Zie [toegangsbeheer in AD Azure cloud toepassingen op basis van rollen]voor een voorbeeld stapsgewijze implementatie[Duyshant-Role-Blog].

## <a name="scopes"></a>scopes
Net als [rollen](#roles)kunnen scopes u voor een [resource-server](#resource-server) voor de toegang tot de beveiligde bronnen. Scopes worden gebruikt voor de implementatie [op basis van een scope] [ OAuth2-Access-Token-Scopes] toegangsbeheer voor een [clienttoepassing](#client-application) die toegang heeft gekregen overgedragen aan de bron door de eigenaar.

Scopes zijn resource gedefinieerde tekenreeksen (bijvoorbeeld "Mail.Read", "Directory.ReadWrite.All") beheerd in de [Azure klassieke portal] [ AZURE-classic-portal] via de bron van het [manifest van de toepassing](#application-manifest), en opgeslagen in de bron van [de eigenschap oauth2Permissions][AAD-Graph-Sp-Entity]. De klassieke Azure portal wordt ook gebruikt voor client toepassing [overgedragen machtigingen](#permissions) voor toegang tot een scope configureren.

Beste praktijken voor naamgeving, is een 'resource.operation.constraint'-indeling gebruiken. Zie voor een uitgebreide bespreking van de scopes weergegeven door Azure AD Graph API [Graph API machtiging Scopes][AAD-Graph-Perm-Scopes]. Voor scopes weergegeven door de services van Office 365, Zie [referentie voor API voor Office 365-machtigingen][O365-Perm-Ref].

## <a name="security-token"></a>beveiligingstoken
Een ondertekend document met vorderingen, zoals een OAuth2 token of bevestiging van SAML 2.0. Voor een OAuth2 [vergunning verlenen](#authorization-grant), een [toegangstoken](#access-token) (OAuth2) en een [Id-Token](OpenID Connect) beveiligingstokens soorten worden, die beide worden geïmplementeerd als een [JSON Web Token (JWT)][JWT].

## <a name="service-principal-object"></a>service principal-object
Wanneer u registreren of bij te werken een toepassing in de [Azure klassieke portal][AZURE-classic-portal], de portal maakt/updates zowel een [application-object](#application-object) en een bijbehorende service principal-object voor deze huurder. Het application-object *definieert* configuratie van de identiteit van de toepassing globaal (voor alle huurders waar de bijbehorende toepassing toegang is verleend), en de sjabloon van waaruit de bijbehorende service principal objecten worden *afgeleid* voor gebruik tijdens runtime (in een bepaalde huurder) lokaal is.

[Toepassing en Service Principal objecten] Zie[ AAD-App-SP-Objects] voor meer informatie.

## <a name="sign-in"></a>aanmelden
Het proces van een [clienttoepassing](#client-application) eindgebruiker verificatie initiëren en verwante staat voor het verkrijgen van een [beveiligingstoken](#security-token) en de toepassingssessie aan die staat scoping vastleggen. State afgeleid van tokens, claims en artefacten, zoals informatie over gebruikersprofielen kunt opnemen.

De functie-in van een toepassing wordt meestal gebruikt voor het implementeren van eenmalige aanmelding (SSO). Het kan ook worden voorafgegaan door een "aanmelden" functie als het startpunt van een eindgebruiker toegang te krijgen tot een toepassing (bij het eerste teken-in). De aanmelding functie wordt gebruikt voor het verzamelen en meer specifiek voor de gebruiker staat behouden en moet u mogelijk [toestemming van de gebruiker](#consent).

## <a name="sign-out"></a>afmelden
Het proces niet verifiëren een eindgebruiker loskoppelen van de staat van de gebruiker die is gekoppeld aan de [clienttoepassing](#client-application) sessie tijdens [aanmelden](#sign-in)

## <a name="tenant"></a>huurder
Een exemplaar van een directory AD Azure een huurder Azure AD genoemd. Het biedt tal van functies, waaronder:

- een registry-service voor geïntegreerde toepassingen
- verificatie van gebruikersaccounts en geregistreerde toepassingen
- REST eindpunten nodig ter ondersteuning van verschillende protocollen, zoals OAuth2 en SAML, met inbegrip van het [eindpunt van de vergunning](#authorization-endpoint), [token eindpunt](#token-endpoint) en de "gemeenschappelijke" eindpunt dat wordt gebruikt door de [huurder met meerdere toepassingen](#multi-tenant-application).

Een huurder is ook gekoppeld aan een Azure advertentie of abonnement tijdens het inrichten van het abonnement, Identity & Access Management functies biedt voor het abonnement op Office 365. [Het verkrijgen van een huurder Azure Active Directory] Zie[ AAD-How-To-Tenant] voor meer informatie over de verschillende manieren waarop u toegang kan krijgen tot een huurder. [Hoe Azure abonnementen zijn gekoppeld aan Azure Active Directory] Zie[ AAD-How-Subscriptions-Assoc] voor meer informatie over de relatie tussen de abonnementen en een huurder Azure AD.

## <a name="token-endpoint"></a>token-eindpunt
Een van de eindpunten geïmplementeerd door de [vergunning server](#authorization-server) ter ondersteuning van OAuth2 [vergunning verleent](#authorization-grant). Afhankelijk van de subsidie, deze kan worden gebruikt ter verwerving van een [toegangstoken](#access-token) (en de bijbehorende token 'refresh') aan een [klant](#client-application)of [ID token](#ID-token) gebruikt in combinatie met de [OpenID verbinding] [ OpenIDConnect] protocol.

## <a name="user-agent-based-client"></a>Gebruiker-agent-clients
Een type [-clienttoepassing](#client-application) die code vanaf een webserver gedownload en wordt uitgevoerd binnen een gebruikersagent (bijvoorbeeld een webbrowser), zoals een enkele pagina toepassing (SPA). Aangezien alle code wordt uitgevoerd op een apparaat, wordt een "publiek" client vanwege het feit dat voor het opslaan van referenties privé/vertrouwelijk beschouwd. [OAuth2 clienttypen en profielen] Zie[ OAuth2-Client-Types] voor meer informatie.

## <a name="user-principal"></a>UPN
Dezelfde manier als die een service SPN-object wordt gebruikt voor een instantie van een toepassing een user principal-object is een ander type beveiligings-principal, die een gebruiker vertegenwoordigt. De grafiek van Azure AD [entiteit gebruiker] [ AAD-Graph-User-Entity] definieert het schema voor een object, met inbegrip van gebruikers gerelateerde eigenschappen zoals voornaam en achternaam, UPN-naam, het rollidmaatschap directory, enz. Dit zorgt voor de configuratie van de gebruiker identiteit voor Azure AD vast te stellen een UPN tijdens runtime. De UPN wordt gebruikt voor een geverifieerde gebruiker voor eenmalige aanmelding, registratie van de delegatie [toestemming](#consent) , zodat wijze het toegangsbeheer, enz.

## <a name="web-client"></a>WebClient
Een type [-clienttoepassing](#client-application) die alle code wordt uitgevoerd op een webserver en kan functioneren als een 'vertrouwelijk' client door de referenties veilig op de server opslaan. [OAuth2 clienttypen en profielen] Zie[ OAuth2-Client-Types] voor meer informatie.

## <a name="next-steps"></a>Volgende stappen
De [Azure AD Developer Guide] [ AAD-Dev-Guide] is de portal voor de ontwikkeling van alle Azure AD Verwante onderwerpen, waaronder een overzicht van de [integratie van] [ AAD-How-To-Integrate] en de basisbeginselen van [Azure AD-verificatie en verificatie ondersteund scenario's][AAD-Auth-Scenarios].

Gebruik de volgende Disqus commentaar sectie om feedback te geven en help ons verfijnen en vorm van de inhoud.

<!--Image references-->

<!--Reference style links -->
[AAD-App-Manifest]: ./active-directory-application-manifest.md
[AAD-App-SP-Objects]: ./active-directory-application-objects.md
[AAD-Auth-Scenarios]: ./active-directory-authentication-scenarios.md
[AAD-Dev-Guide]: ./active-directory-developers-guide.md
[AAD-Graph-Perm-Scopes]: https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-graph-api-permission-scopes
[AAD-Graph-App-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[AAD-Graph-Sp-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity
[AAD-Graph-User-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#user-entity
[AAD-How-Subscriptions-Assoc]: ./active-directory-how-subscriptions-associated-directory.md
[AAD-How-To-Integrate]: ./active-directory-how-to-integrate.md
[AAD-How-To-Tenant]: active-directory-howto-tenant.md
[AAD-Integrating-Apps]: ./active-directory-integrating-applications.md
[AAD-Multi-Tenant-Overview]: active-directory-devhowto-multi-tenant-overview.md
[AAD-Security-Token-Claims]: ./active-directory-authentication-scenarios/#claims-in-azure-ad-security-tokens
[AAD-Tokens-Claims]: ./active-directory-token-and-claims.md
[AZURE-classic-portal]: https://manage.windowsazure.com
[Duyshant-Role-Blog]: http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/
[JWT]: https://tools.ietf.org/html/draft-ietf-oauth-json-web-token-32
[Microsoft-Graph]: https://graph.microsoft.io
[O365-Perm-Ref]: https://msdn.microsoft.com/en-us/office/office365/howto/application-manifest
[OAuth2-Access-Token-Scopes]: https://tools.ietf.org/html/rfc6749#section-3.3
[OAuth2-AuthZ-Endpoint]: https://tools.ietf.org/html/rfc6749#section-3.1
[OAuth2-AuthZ-Grant-Types]: https://tools.ietf.org/html/rfc6749#section-1.3
[OAuth2-Client-Types]: https://tools.ietf.org/html/rfc6749#section-2.1
[OAuth2-Role-Def]: https://tools.ietf.org/html/rfc6749#page-6
[OpenIDConnect]: http://openid.net/specs/openid-connect-core-1_0.html
[OpenIDConnect-AuthZ-Endpoint]: http://openid.net/specs/openid-connect-core-1_0.html#AuthorizationEndpoint
[OpenIDConnect-ID-Token]: http://openid.net/specs/openid-connect-core-1_0.html#IDToken
