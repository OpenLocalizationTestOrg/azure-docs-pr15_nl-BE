<properties
   pageTitle="Het bouwen van een toepassing die een Azure Active Directory-gebruiker kunt aanmelden | Microsoft Azure"
   description="Instructies voor het maken van een toepassing die kunnen stap voor stap een gebruiker uit een huurder Azure Active Directory, ook bekend als een toepassing meerdere huurder inloggen."
   services="active-directory"
   documentationCenter=""
   authors="skwan"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/11/2016"
   ms.author="skwan;bryanla"/>

# <a name="how-to-sign-in-any-azure-active-directory-ad-user-using-the-multi-tenant-application-pattern"></a>Aanmelden van gebruikers met de toepassing van meerdere huurder patroon Azure Active Directory (AD)
Als u een Software als een Service-toepassing voor veel organisaties te bieden, kunt u uw toepassing accepteert aanmeldingen van een huurder Azure AD.  Dit heet in Azure AD waardoor uw toepassing meerdere huurder.  Gebruikers in een huurder Azure AD aanmelden bij uw toepassing na stemt ermee in dat hun account gebruiken met uw toepassing kan worden.  

Als u een bestaande toepassing die heeft een eigen account systeem of andere soorten van andere aanbieders van cloud inloggen ondersteunt, Azure AD inloggen toe te voegen vanuit een huurder is net zo eenvoudig als het registreren van uw app en aanmelden met een Microsoft-knop plaatsen op uw toepassing ondertekenen in via OAuth2, OpenID verbinding of SAML-code toe te voegen. Klik op de knop hieronder voor meer informatie over de huisstijl van uw toepassing.

[! [Knop aanmelden] [AAD-aanmelden]][AAD-App-Branding]


In dit artikel wordt ervan uitgegaan dat u al bekend bent met bouwen van een toepassing één huurder voor Azure advertentie.  Als u, hoofd back-up op de [introductiepagina van developer guide twijfelt] [ AAD-Dev-Guide] en probeer een van onze snel aan de slag!

Er zijn vier eenvoudige stappen converteren van uw toepassing naar een huurder met meerdere Azure AD app:

1.  De registratie van toepassing als u met meerdere huurder bijwerken
2.  Werk uw code voor het verzenden van aanvragen naar de/Common eindpunt 
3.  Werk uw code voor het afhandelen van meerdere waarden van de uitgevende instelling
4.  Toestemming voor gebruiker en beheerder begrijpen en geschikte codewijzigingen

We bekijken bij elke stap in detail. Kunt u rechtstreeks naar [deze lijst van de monsters met meerdere huurder]springen[AAD-Samples-MT].

## <a name="update-registration-to-be-multi-tenant"></a>Registratie worden meerdere huurder bijwerken
Web app/API registraties in Azure AD zijn standaard één huurder.  Kunt u uw inschrijving met meerdere huurder door de schakeloptie 'Toepassing is met meerdere huurder' op de configuratiepagina van de registratie van uw toepassing vinden in [Azure klassieke portal] [ AZURE-classic-portal] en deze wordt ingesteld op 'Ja'.

Opmerking: Voordat een toepassing kan worden gemaakt met meerdere huurder, Azure AD, moet de URI App-ID van de toepassing moet uniek. De URI van App-ID is een van de manieren waarop die een toepassing wordt aangeduid in protocolberichten.  Een app één huurder is voldoende voor de URI App-ID moet uniek zijn binnen die huurder.  Een toepassing met meerdere huurder zich globaal unieke zodat Azure AD de toepassing over alle huurders vinden kunt.  Globale uniekheid wordt afgedwongen door te vereisen dat de URI App-ID om een host-naam die overeenkomt met een gecontroleerde domein van de pachter Azure AD.  Als de naam van de huurder contoso.onmicrosoft.com en vervolgens een geldige was URI voor App-ID zou bijvoorbeeld `https://contoso.onmicrosoft.com/myapp`.  Als de huurder had een geverifieerde domein van `contoso.com`, dan een geldige URI voor App-ID zou ook `https://contoso.com/myapp`.  Als u een toepassing als meerdere huurder mislukt als de URI App-ID niet dit patroon volgen.

Native clientregistraties zijn meerdere huurder standaard.  U hoeft niet te doen om een native client toepassing registratie met meerdere huurder.

## <a name="update-your-code-to-send-requests-to-common"></a>De code voor het verzenden van aanvragen naar/Common bijwerken
In een toepassing één huurder worden inloggen aanvragen verzonden naar inloggen eindpunt van de huurder.   Voor contoso.onmicrosoft.com kan het eindpunt zou zijn:

    https://login.microsoftonline.com/contoso.onmicrosoft.com

Aanvragen die worden verzonden naar het eindpunt van een huurder kunnen inloggen gebruikers (of gasten) in die huurder toepassingen in die huurder.  Met een toepassing met meerdere huurder weet de toepassing niet vooraf welke huurder de gebruiker is uit, zodat u aanvragen naar het eindpunt van de huurder verzenden kunt.  In plaats daarvan worden aanvragen verzonden naar een eindpunt dat multiplexes over alle Azure AD huurders:

    https://login.microsoftonline.com/common

Azure AD ontvangt wanneer een aanvraag op de/Common eindpunt, meldt de gebruiker zich en als gevolg hiervan wordt vastgesteld welke huurder afkomstig van de gebruiker is.  De/gemeenschappelijke eindpunt werkt met alle verificatieprotocollen worden ondersteund door AD Azure: verbinding OpenID, OAuth 2.0 SAML 2.0 en WS-Federation.

Het teken in reactie op de toepassing vervolgens bevat een token voor de gebruiker.  De waarde van de uitgevende instelling in het token wordt een toepassing welke huurder afkomstig van de gebruiker is.  Wanneer een antwoord geeft als resultaat van de/Common eindpunt, de waarde van de uitgevende instelling in het token, correspondeert met de huurder van de gebruiker.  Het is belangrijk te weten de/Common eindpunt is niet een huurder en is niet een uitgevende instelling, is het net een multiplexer.  Wanneer u/Common, moet de logica in uw toepassing om tokens te valideren worden bijgewerkt om daarmee rekening houden. 

Zoals eerder gezegd, moeten huurder met meerdere toepassingen bieden ook een consistente ervaring-in voor gebruikers, volgens de huisstijl richtlijnen Azure AD-toepassing. Klik op de knop hieronder voor meer informatie over de huisstijl van uw toepassing.

[! [Knop aanmelden] [AAD-aanmelden]][AAD-App-Branding]

Laten we bij het gebruik van de/Common eindpunt en de implementatie van code in meer detail.

## <a name="update-your-code-to-handle-multiple-issuer-values"></a>Werk uw code voor het afhandelen van meerdere waarden van de uitgevende instelling
Webtoepassingen en webservices API's ontvangen en tokens van Azure AD valideren.  

> [AZURE.NOTE] Native client-toepassingen aanvragen en ontvangen van tokens van Azure AD, doen ze voor het verzenden van deze API's, waar ze worden gevalideerd.  Eigen toepassingen tokens niet gevalideerd en moeten behandelen als dekkend.

We bekijken op hoe een aanvraag wordt gevalideerd met tokens die worden ontvangen van Azure AD.  Een toepassing één huurder vindt gewoonlijk een endpoint-waarde als volgt:

    https://login.microsoftonline.com/contoso.onmicrosoft.com

en als gebruiken om een URL-metagegevens (in dit geval OpenID verbinding) samen te stellen:

    https://login.microsoftonline.com/contoso.onmicrosoft.com/.well-known/openid-configuration

voor het downloaden van twee essentiële stukjes informatie die worden gebruikt om tokens te valideren: de huurder het ondertekenen van sleutels en de waarde van de uitgevende instelling.  Elke huurder Azure AD heeft een unieke uitgever van het formulier:

    https://sts.windows.net/31537af4-6d77-4bb9-a681-d2394888ea26/

waar is de GUID-waarde de naam veilige versie van de huurder-ID van de huurder.  Als je op de bovenstaande link metagegevens voor `contoso.onmicrosoft.com`, ziet u de waarde van deze uitgevende instelling in het document.

Wanneer een toepassing één huurder een token valideert, controleert de handtekening van het token tegen de handtekeningsleutels van de metagegevensdocument en zorgt ervoor dat de waarde van de uitgevende instelling in het token overeenkomt met de versie die is gevonden in de metagegevensdocument.

Sinds de/Common eindpunt komt niet overeen met een huurder en een uitgevende instelling, niet wanneer u de waarde van de uitgevende instelling in de metagegevens voor onderzoeken/gemeenschappelijk heeft een sjablonen URL in plaats van een werkelijke waarde:

    https://sts.windows.net/{tenantid}/

Daarom een toepassing met meerdere huurder kan niet valideren tokens door overeenkomen met de waarde van de uitgevende instelling in de metagegevens met de `issuer` de waarde in het token.  Een toepassing met meerdere huurder logica om te beslissen welke uitgever waarden geldig zijn en welke zijn niet vereist, op basis van de huurder-id-gedeelte van de waarde van de uitgevende instelling.  

Bijvoorbeeld als een toepassing meerdere huurder alleen aanmelden van specifieke huurders die zich hebben aangemeld voor de service staat, het moet controleert of de waarde van de uitgevende instelling of de `tid` aanspraak maken op de waarde in het token om ervoor te zorgen dat de huurder zich in hun lijst met abonnees.  Als een toepassing meerdere huurder alleen personen behandelt geen beslissingen de toegang op basis van huurders, vervolgens kan worden genegeerd de uitgevende instelling waarde helemaal niet.

In de huurder met meerdere voorbeelden u in de sectie [Verwante inhoud](#related-content) aan het einde van dit artikel vindt, uitgever validatie uitgeschakeld zodat de huurder Azure AD aan te melden.

Nu laten we de gebruikerservaring voor gebruikers die de huurder met meerdere toepassingen zich wilt aanmelden.

## <a name="understanding-user-and-admin-consent"></a>Wat zijn gebruikers- en admin toestemming
Een gebruiker aan te melden een toepassing in Azure AD, moet de toepassing worden weergegeven in de pachter van de gebruiker.  Zo kunt bijvoorbeeld unieke beleid toepassen wanneer gebruikers hun huurder bij de toepassing aanmelden van de organisatie.  Voor de toepassing van een enkele huurder is deze registratie eenvoudig; dat gebeurt wanneer u de toepassing in de [Azure klassieke portal registreert]is[AZURE-classic-portal].

Voor een toepassing met meerdere huurder woont de eerste inschrijving voor de toepassing in de Azure AD huurder gebruikt door de ontwikkelaar.  Wanneer een gebruiker van een andere huurder zich tot de toepassing voor de eerste keer aanmeldt, wordt deze instemming met de machtigingen voor de toepassing AD Azure gevraagd.  Als zij instemt, wordt een representatie van de toepassing met de naam van een *service principal* gemaakt in de pachter van de gebruiker en aanmelden kan worden voortgezet. Een overdracht wordt ook gemaakt in de map die toestemming aan de toepassing van de gebruiker worden vastgelegd. [Toepassing en Service Principal objecten] Zie[ AAD-App-SP-Objects] voor meer informatie over de toepassing van de objecten Application en ServicePrincipal en hoe ze aan elkaar gerelateerd.

![Instemming met één laag app][Consent-Single-Tier] 

De ervaring van deze toestemming wordt beïnvloed door de machtigingen voor de toepassing.  Azure AD ondersteunt twee soorten machtigingen voor alleen-app en overgedragen:

- Een gemachtigde toestemming verleent een toepassing de mogelijkheid om te fungeren als een gebruiker aangemeld voor een subset van de dingen die de gebruiker kan doen.  Zo kunt u verlenen een toepassing overgedragen machtigingen voor het lezen van de agenda van de gebruiker ingelogd.
- Een app alleen-lezen machtiging is verleend rechtstreeks aan de identiteit van de toepassing.  Bijvoorbeeld, u kan een toepassing de machtiging alleen-app voor het lezen van de lijst met gebruikers in een huurder en het is mogelijk om dit te doen ongeacht wie is aangemeld bij de toepassing.

Sommige machtigingen kunnen ingestemd met door een gewone gebruiker, terwijl andere toestemming van de beheerder van een huurder 

### <a name="admin-consent"></a>Admin toestemming
App alleen-lezen-machtigingen moet altijd toestemming van de beheerder van een huurder.  Als uw toepassing vraagt om een machtiging alleen-app en een normale gebruiker probeert aan te melden de toepassing, krijgt uw toepassing een foutmelding dat de gebruiker niet kunnen toestemming is.

Bepaalde overgedragen machtigingen ook nodig toestemming van de beheerder van een huurder.  De mogelijkheid om terug te schrijven naar AD Azure als een beheerder van de huurder toestemming nodig om de gebruiker ingelogd.  Als de machtiging alleen-app als een gewone gebruiker probeert aan te melden een toepassing waarin een gedelegeerde machtigingen waarvoor toestemming van de beheerder, ontvangt uw toepassing een fout.  Een machtiging vereist of admin toestemming wordt bepaald door de ontwikkelaar die de bron gepubliceerd en kan worden gevonden in de documentatie voor de resource.  Koppelingen naar informatie over de beschikbare machtigingen voor de Azure AD Graph API en Microsoft Graph API zijn in de sectie [Verwante inhoud](#related-content) van dit artikel.

Als uw toepassing gebruikmaakt van de machtigingen waarvoor toestemming van de beheerder, moet u een penbeweging hebt in uw toepassing, zoals een knop of koppeling waar de beheerder de actie tot stand kan brengen.  De aanvraag voor uw toepassing verzendt voor deze actie een autorisatieaanvraag gebruikelijke OAuth2/OpenID verbinding is, maar die ook de `prompt=admin_consent` query string parameter.  Zodra de admin heeft ingestemd en de service principal is gemaakt van de klant huurder, latere inloggen aanvragen hoeft niet de `prompt=admin_consent` parameter.   Aangezien de beheerder heeft besloten dat de machtigingen die zijn aangevraagd, worden geaccepteerd, wordt er geen andere gebruikers in de huurder toestemming vanaf dat moment gevraagd.

De `prompt=admin_consent` parameter kan ook worden gebruikt door toepassingen die geen admin toestemming vereist, maar geven een ervaring waar de huurder admin "aanmeldt" machtigingen aanvragen voor de toepassing één keer en er geen andere gebruikers wordt gevraagd om toestemming vanaf dat punt op.

Als een toepassing moet de toestemming van de beheerder en de beheerder zich bij de toepassing aanmeldt, maar de `prompt=admin_consent` parameter is niet verzonden, admin toe te staan de toepassing met succes kunnen worden, maar ze zullen alleen toestemming voor de gebruikersaccount.  Gewone gebruikers nog steeds worden niet kunnen aanmelden en instemming met de toepassing.  Dit is handig als u wilt dat de beheerder van de huurder de mogelijkheid te onderzoeken van de toepassing voordat andere gebruikers toegang geven.

Een beheerder van een huurder kan de mogelijkheid voor normale gebruikers toestemming voor toepassingen uitschakelen.  Als deze functie is uitgeschakeld, is altijd admin toestemming vereist is voor de toepassing in de pachter worden ingesteld.  Als u wilt dat uw toepassing testen met gewone gebruiker toestemming uitgeschakeld, vindt u de schakeloptie configuratie in de huurder Azure AD configuratiesectie van [Azure klassieke portal][AZURE-classic-portal].

> [AZURE.NOTE] Sommige toepassingen kunnen een ervaring waar gewone gebruikers kunnen in eerste instantie toestemming, en later de aanvraag kan betrekking hebben op de beheerder en vraag machtigingen waarvoor toestemming van de beheerder.  Er is geen manier om dit te doen met de registratie van een afzonderlijke toepassing in Azure AD vandaag.  De aanstaande Azure AD v2 eindpunt kan toepassingen voor toegang in runtime, in plaats van op moment van registratie, waardoor dit scenario.  Zie voor meer informatie de [Handleiding voor ontwikkelaars van Azure AD App Model v2][AAD-V2-Dev-Guide].

### <a name="consent-and-multi-tier-applications"></a>Toestemming en meerlagige toepassingen
Uw toepassing kan meerdere lagen hebben, elk vertegenwoordigd door een eigen registratie in Azure AD.  Dat wordt een web API bijvoorbeeld een eigen toepassing die een web-API wordt aangeroepen of een webtoepassing.  In beide gevallen vraagt de client (native app of web app) machtigingen voor de bron (web API) aanroepen.  De client met succes in de klant huurder worden ingestemd moeten alle resources waaraan deze machtigingen aanvragen al bestaan in van de klant huurder.  Als deze voorwaarde niet is voldaan, retourneert AD Azure een fout eerst toegevoegd van de resource.

Dit kan een probleem zijn als de logische toepassing uit twee of meer toepassing registraties, bijvoorbeeld een afzonderlijke client en een resource bestaat.  Hoe krijgt u de resource in de klant-huurder eerste?  Azure AD heeft betrekking op deze aanvraag doordat de client- en resource in één stap, waar de gebruiker de totale som van de machtigingen die zijn aangevraagd door de client en de resource op de pagina toestemming ziet worden ingestemd.  Registratie van de toepassing van de resource omvat zodat dit probleem App-ID van de client als een `knownClientApplications` in het manifest van de toepassing.  Bijvoorbeeld:

    knownClientApplications": ["94da0930-763f-45c7-8d26-04d5938baab2"]

Deze eigenschap kan worden bijgewerkt via de resource het [manifest van de toepassing van][AAD-App-Manifest], en wordt gedemonstreerd in een meerlaags native client monster web API aanroepen in de sectie [Verwante inhoud](#related-content) aan het einde van dit artikel. Het onderstaande diagram geeft een overzicht van de toestemming voor een toepassing met meerdere niveaus:

![Instemming met meerdere lagen bekende client app][Consent-Multi-Tier-Known-Client] 

Een soortgelijke aanvraag gebeurt als de verschillende lagen van een toepassing in verschillende huurders zijn geregistreerd.  Neem bijvoorbeeld het geval zijn bij het bouwen van een native clienttoepassing die de Office 365 Exchange Online-API aanroept.  Het ontwikkelen van de oorspronkelijke toepassing is, en later voor de oorspronkelijke toepassing worden uitgevoerd in de klant huurder en zijn de Exchange Online-service principal aanwezig.  In dit geval heeft de klant voor de service principal te maken in hun huurder Exchange Online kopen.  Bieden de mogelijkheid om hun klanten toe te staan hun toepassing in een klant huurder, bijvoorbeeld een webpagina die stations met behulp van de mechanismen die in dit artikel beschreven toestemming moet in het geval van een API die is gebouwd door een organisatie dan Microsoft, de ontwikkelaar van de API.  Nadat de service principal in de pachter is gemaakt, krijgt de oorspronkelijke toepassing tokens voor de API.

Het onderstaande diagram geeft een overzicht van toestemming voor de toepassing van een meerlaags geregistreerd in verschillende huurders:

![Instemming met meerdere lagen met meerdere partijen app][Consent-Multi-Tier-Multi-Party] 

### <a name="revoking-consent"></a>Toestemming intrekken
Gebruikers en beheerders kunnen toestemming geven voor de toepassing op elk gewenst moment intrekken:

- Gebruikers toegang tot afzonderlijke toepassingen intrekken door ze te verwijderen uit het [Paneel toepassingen] [ AAD-Access-Panel] lijst.
- Beheerders toegang tot toepassingen intrekken door ze te verwijderen uit Azure Active Directory met behulp van de sectie van de [Azure klassieke portal]Azure AD[AZURE-classic-portal].

Als een beheerder met een toepassing voor alle gebruikers in een huurder instemt, intrekken niet gebruikers afzonderlijk toegang.  Alleen de beheerder toegang kunt intrekken en alleen voor de gehele toepassing.

### <a name="consent-and-protocol-support"></a>Toestemming en protocolondersteuning
Toestemming wordt ondersteund in Azure AD via de OAuth, OpenID verbinden, WS-Federation en SAML-protocollen.  De SAML en WS-Federation-protocollen bieden geen ondersteuning voor de `prompt=admin_consent` parameter, zodat admin toestemming alleen mogelijk via OAuth en OpenID verbinden is.

## <a name="multi-tenant-applications-and-caching-access-tokens"></a>Huurder met meerdere toepassingen en Caching toegangstokens
Toepassingen met meerdere huurder krijgt ook toegangstokens aanroepen van API's die zijn beveiligd met AD Azure.  Een veelvoorkomende fout bij het gebruik van Active Directory verificatie bibliotheek (ADAL) met een toepassing met meerdere huurder is het in eerste instantie een token voor een gebruiker met behulp van/Common, verzoeken om een antwoord ontvangt, en vraag een latere token voor dezelfde gebruiker ook met/Common.  Aangezien de reactie van Azure AD afkomstig is van een huurder, geen/algemeen ADAL slaat het token als afkomstig van de huurder. / Common om een toegangstoken voor de gebruiker de volgende aanroep missers vermelding in de cache en de gebruiker wordt gevraagd opnieuw inloggen.  Als u wilt voorkomen dat de cache ontbreken, moet volgende aanroepen voor een gebruiker al ingelogd eindpunt van de huurder worden gedaan.

## <a name="related-content"></a>Verwante inhoud

- [Voorbeelden van toepassingen met meerdere huurder][AAD-Samples-MT]
- [Huisstijl richtlijnen voor toepassingen][AAD-App-Branding]
- [Azure AD Developer's Guide][AAD-Dev-Guide]
- [Toepassing en Service Principal-objecten][AAD-App-SP-Objects]
- [Integratie van toepassingen met Azure Active Directory][AAD-Integrating-Apps]
- [Overzicht van het kader van de toestemming][AAD-Consent-Overview]
- [Microsoft Graph API machtiging Scopes][MSFT-Graph-AAD]
- [Azure AD Graph API machtiging Scopes][AAD-Graph-Perm-Scopes]

Gebruik de Disqus opmerkingen hieronder om feedback te geven en help ons verfijnen en vorm van de inhoud.

<!--Reference style links IN USE -->
[AAD-Access-Panel]:  https://myapps.microsoft.com
[AAD-App-Branding]: ./active-directory-branding-guidelines.md
[AAD-App-Manifest]: ./active-directory-application-manifest.md
[AAD-App-SP-Objects]: ./active-directory-application-objects.md
[AAD-Auth-Scenarios]: ./active-directory-authentication-scenarios.md
[AAD-Consent-Overview]: ./active-directory-integrating-applications.md#overview-of-the-consent-framework
[AAD-Dev-Guide]: ./active-directory-developers-guide.md
[AAD-Graph-Overview]: https://azure.microsoft.com/en-us/documentation/articles/active-directory-graph-api/
[AAD-Graph-Perm-Scopes]: https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-graph-api-permission-scopes
[AAD-Integrating-Apps]: ./active-directory-integrating-applications.md
[AAD-Samples-MT]: https://azure.microsoft.com/documentation/samples/?service=active-directory&term=multitenant
[AAD-Why-To-Integrate]: ./active-directory-how-to-integrate.md
[AZURE-classic-portal]: https://manage.windowsazure.com
[MSFT-Graph-AAD]: https://graph.microsoft.io/en-us/docs/authorization/permission_scopes

<!--Image references-->
[AAD-Sign-In]: ./media/active-directory-devhowto-multi-tenant-overview/sign-in-with-microsoft-light.png
[Consent-Single-Tier]: ./media/active-directory-devhowto-multi-tenant-overview/consent-flow-single-tier.png
[Consent-Multi-Tier-Known-Client]: ./media/active-directory-devhowto-multi-tenant-overview/consent-flow-multi-tier-known-clients.png
[Consent-Multi-Tier-Multi-Party]: ./media/active-directory-devhowto-multi-tenant-overview/consent-flow-multi-tier-multi-party.png

<!--Reference style links -->
[AAD-App-Manifest]: ./active-directory-application-manifest.md
[AAD-App-SP-Objects]: ./active-directory-application-objects.md
[AAD-Auth-Scenarios]: ./active-directory-authentication-scenarios.md
[AAD-Integrating-Apps]: ./active-directory-integrating-applications.md
[AAD-Dev-Guide]: ./active-directory-developers-guide.md
[AAD-Graph-Perm-Scopes]: https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-graph-api-permission-scopes
[AAD-Graph-App-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[AAD-Graph-Sp-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity
[AAD-Graph-User-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#user-entity
[AAD-How-To-Integrate]: ./active-directory-how-to-integrate.md
[AAD-Security-Token-Claims]: ./active-directory-authentication-scenarios/#claims-in-azure-ad-security-tokens
[AAD-Tokens-Claims]: ./active-directory-token-and-claims.md
[AAD-V2-Dev-Guide]: ./active-directory-appmodel-v2-overview.md
[AZURE-classic-portal]: https://manage.windowsazure.com
[Duyshant-Role-Blog]: http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/
[JWT]: https://tools.ietf.org/html/draft-ietf-oauth-json-web-token-32
[O365-Perm-Ref]: https://msdn.microsoft.com/en-us/office/office365/howto/application-manifest
[OAuth2-Access-Token-Scopes]: https://tools.ietf.org/html/rfc6749#section-3.3
[OAuth2-AuthZ-Code-Grant-Flow]: https://msdn.microsoft.com/library/azure/dn645542.aspx
[OAuth2-AuthZ-Grant-Types]: https://tools.ietf.org/html/rfc6749#section-1.3 
[OAuth2-Client-Types]: https://tools.ietf.org/html/rfc6749#section-2.1
[OAuth2-Role-Def]: https://tools.ietf.org/html/rfc6749#page-6
[OpenIDConnect]: http://openid.net/specs/openid-connect-core-1_0.html
[OpenIDConnect-ID-Token]: http://openid.net/specs/openid-connect-core-1_0.html#IDToken














