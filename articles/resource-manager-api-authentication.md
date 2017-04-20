<properties 
   pageTitle="Verificatie van Active Directory en Resource Manager | Microsoft Azure"
   description="Een developer's guide voor verificatie met Active Directory en Azure Resource Manager API voor de integratie van een app met andere Azure abonnementen."
   services="azure-resource-manager,active-directory"
   documentationCenter="na"
   authors="dushyantgill"
   manager="timlt"
   editor="tysonn" />
<tags 
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/31/2016"
   ms.author="dugill;tomfitz" />

# <a name="how-to-use-azure-active-directory-and-resource-manager-to-manage-a-customers-resources"></a>Azure Active Directory en Resource Manager gebruiken voor het beheren van middelen van de klant

## <a name="introduction"></a>Inleiding

Als u een softwareontwikkelaar die voor het maken van een app die worden beheerd met Azure resources van de klant nodig heeft, in dit onderwerp wordt beschreven hoe u met de Azure Resource Manager API's geverifieerd en toegang krijgen tot bronnen in andere abonnementen. 

Uw app toegang tot de API's voor bronbeheer in twee manieren:

1. **Gebruiker + app toegang**: voor toepassingen die toegang hebben tot bronnen een gebruiker ingelogd. Deze benadering werkt voor toepassingen zoals web apps en opdrachtregelprogramma's die betrekking op alleen 'interactieve management' Azure bronnen hebben.
1. **App alleen-lezen toegang**: voor toepassingen die daemon services en geplande taken worden uitgevoerd. De identiteit van de app is directe toegang tot de bronnen worden verleend. Deze benadering werkt voor toepassingen die op de lange termijn "off line toegang' tot Azure.

In dit onderwerp vindt stapsgewijze instructies voor het maken van een toepassing die gebruikmaakt van deze machtigingsmethoden. Wordt het uitvoeren van elke stap met REST API of C#. De volledige toepassing van ASP.NET MVC is beschikbaar op [https://github.com/dushyantgill/VipSwapper/tree/master/CloudSense](https://github.com/dushyantgill/VipSwapper/tree/master/CloudSense).

De code voor dit onderwerp wordt uitgevoerd als een web app die u op [http://vipswapper.azurewebsites.net/cloudsense proberen kunt](http://vipswapper.azurewebsites.net/cloudsense). 

## <a name="what-the-web-app-does"></a>Betekenis van de web app.

De web-app:

1. Symptomen bij een Azure-gebruiker.
2. Vraagt de gebruiker de web app om toegang te verlenen aan de bronnenbeheerder.
3. Haalt de gebruiker + toegangstoken app voor toegang tot de Resource Manager.
4. Bel Resource Manager en van de app service principal toewijzen aan een rol in het abonnement, waarbij de app op lange termijn toegang krijgt tot het abonnement wordt token (uit stap 3).
5. Met deze eigenschap wordt alleen app token.
6. Token (uit stap 5) wordt gebruikt voor het beheren van bronnen in het abonnement via Resource Manager.

Hier is de end-to-end-stroom van de webtoepassing.

![Stroom Resource Manager-verificatie](./media/resource-manager-api-authentication/Auth-Swim-Lane.png)

Als een gebruiker, kunt u de abonnement-id opgeven voor het abonnement dat u wilt gebruiken:

![abonnement-id opgeven](./media/resource-manager-api-authentication/sample-ux-1.png)

Selecteer de account die u wilt gebruiken voor het aanmelden.

![Selecteer account](./media/resource-manager-api-authentication/sample-ux-2.png)

Geef uw referenties.

![referenties opgeven](./media/resource-manager-api-authentication/sample-ux-3.png)

De app toegang verlenen tot uw Azure abonnementen:
 
![Toegang verlenen](./media/resource-manager-api-authentication/sample-ux-4.png)
 
Beheren van uw abonnementen verbonden:

![Abonnement koppelen](./media/resource-manager-api-authentication/sample-ux-7.png)


## <a name="register-application"></a>Toepassing registreren

Voordat u codering, registreert u uw web app met Azure Active Directory (AD). De identiteit van een centrale voor uw app in maakt de app registratie in Azure AD. Het bevat basisinformatie over de toepassing zoals OAuth Client-ID, antwoord op URL's en referenties die door de toepassing worden gebruikt voor de verificatie en toegang tot de API's van Azure Resource Manager. De app-registratie registreert ook de verschillende overgedragen machtigingen die uw toepassing nodig heeft bij het openen van Microsoft APIs namens de gebruiker. 

Omdat uw app toegang krijgt andere abonnement tot, kunt u deze moet configureren als een toepassing meerdere huurder. Om te worden gevalideerd, bieden een die is gekoppeld aan uw Active Directory-domein. Overzicht van de domeinen die zijn gekoppeld aan uw Active Directory, log in op de [klassieke portal](https://manage.windowsazure.com). Selecteer uw Active Directory en selecteer vervolgens **domeinen**.

In het volgende voorbeeld ziet u hoe de app met Azure PowerShell registreren. U hebt de nieuwste versie (augustus 2016) van Azure PowerShell voor deze opdracht om te werken. 

    $app = New-AzureRmADApplication -DisplayName "{app name}" -HomePage "https://{your domain}/{app name}" -IdentifierUris "https://{your domain}/{app name}" -Password "{your password}" -AvailableToOtherTenants $true
    
U moet de toepassings-id en het wachtwoord te melden als de toepassing AD. Overzicht van de toepassings-id die wordt geretourneerd uit de vorige opdracht gebruiken:

    $app.ApplicationId

In het volgende voorbeeld ziet u hoe de app te registreren met behulp van de CLI Azure. 

    azure ad app create --name {app name} --home-page https://{your domain}/{app name} --identifier-uris https://{your domain}/{app name} --password {your password} --available true

De resultaten omvatten de AppId, u moet bij het verifiëren van de toepassing.

### <a name="optional-configuration---certificate-credential"></a>Optionele configuratie - certificaat referentie

Azure AD certificaat referenties ook ondersteuning voor toepassingen: een zelf-ondertekend certificaat maken, behouden de persoonlijke sleutel en de openbare sleutel toevoegen aan uw inschrijving Azure AD toepassing. Uw toepassing een kleine payload verzendt naar Azure AD ondertekend met de persoonlijke sleutel voor de verificatie en Azure AD valideert de handtekening met de openbare sleutel die u hebt geregistreerd.

Zie voor meer informatie over het maken van een AD-app met een certificaat [Met Azure PowerShell voor het maken van een service principal toegang krijgen tot bronnen](resource-group-authenticate-service-principal.md#create-service-principal-with-certificate) of [Azure-CLI gebruiken voor het maken van een service principal toegang krijgen tot bronnen](resource-group-authenticate-service-principal-cli.md#create-service-principal-with-certificate).

## <a name="get-tenant-id-from-subscription-id"></a>Huurder-id opgehaald van de abonnements-id

Als u een token dat kan worden gebruikt voor de Resource Manager aanroepen, moet uw toepassing de huurder-ID van de pachter Azure AD waarop het abonnement Azure bekend. Waarschijnlijk weet dat uw gebruikers hun abonnement-id's, maar ze misschien niet weet de huurder id's voor Active Directory. Als u de gebruikersnaam van de pachter, door de gebruiker te vragen voor de abonnement-id. Bieden die abonnement-id wanneer u vragen over het abonnement:

    https://management.azure.com/subscriptions/{subscription-id}?api-version=2015-01-01

De aanvraag niet gehonoreerd omdat de gebruiker niet is aangemeld nog, maar u de huurder-id uit het antwoord ophalen kunt. Bij deze uitzondering ophalen met de id van de huurder de waarde van de response-header voor **WWW-verificatie**. U ziet deze implementatie van de methode [GetDirectoryForSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L20) .

## <a name="get-user--app-access-token"></a>Gebruiker + app access token ophalen

De toepassing wordt de gebruiker omgeleid naar Azure AD met een OAuth 2.0 toestemming vragen - voor de verificatie van de referenties van de gebruiker en een autorisatiecode terug te gaan. Uw toepassing gebruikt de code van de vergunning te beschikken over een token voor Resource Manager. De methode [ConnectSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/Controllers/HomeController.cs#L42) wordt de autorisatieaanvraag gemaakt.

Dit onderwerp bevat de REST API-verzoeken om de gebruiker te verifiëren. U kunt ook helper bibliotheken gebruiken voor het uitvoeren van verificatie in uw code. Zie voor meer informatie over deze bibliotheken, [Azure Active Directory verificatie Libraries](./active-directory/active-directory-authentication-libraries.md). Zie voor richtlijnen over de integratie van identiteitsbeheer in een toepassing, [ontwikkelaarsgids Azure Active Directory](./active-directory/active-directory-developers-guide.md).

### <a name="auth-request-oauth-20"></a>Auth-aanvraag (OAuth 2.0)

Een Open-ID Connect/OAuth2.0 toestemming aanvraag verstrekken naar het eindpunt Azure AD machtigen:

    https://login.microsoftonline.com/{tenant-id}/OAuth2/Authorize

De queryreeks-parameters die beschikbaar voor deze aanvraag zijn worden beschreven in het onderwerp van de [aanvraag een verificatie-code](./active-directory/active-directory-protocols-oauth-code.md#request-an-authorization-code) .

In het volgende voorbeeld ziet u hoe OAuth2.0 vergunning aanvragen:

    https://login.microsoftonline.com/{tenant-id}/OAuth2/Authorize?client_id=a0448380-c346-4f9f-b897-c18733de9394&response_mode=query&response_type=code&redirect_uri=http%3a%2f%2fwww.vipswapper.com%2fcloudsense%2fAccount%2fSignIn&resource=https%3a%2f%2fgraph.windows.net%2f&domain_hint=live.com

Azure AD verifieert de gebruiker en, indien nodig, vraagt de gebruiker wilt machtigen om de app. Wordt de code van de vergunning de antwoord-URL van uw toepassing. Afhankelijk van de gevraagde response_mode, Azure AD een queryaanvraag in de queryreeks of als post-gegevens.

    code=AAABAAAAiL****FDMZBUwZ8eCAA&session_state=2d16bbce-d5d1-443f-acdf-75f6b0ce8850

### <a name="auth-request-open-id-connect"></a>Auth-aanvraag (Open ID verbinding maken)

Als u niet alleen Azure Resource Manager namens de gebruiker toegang wilt, maar ook kan de gebruiker zich aanmelden bij uw toepassing met hun Azure AD-account, een Open-ID verbinding toestaan aanvraag verstrekken. Met Open-ID Connect ontvangt de toepassing ook een id_token van Azure AD die uw app gebruiken kunt om de gebruiker zich aanmeldt.

De queryreeks-parameters die beschikbaar voor deze aanvraag zijn worden beschreven in het onderwerp [aanmelden verzoek verzenden](./active-directory/active-directory-protocols-openid-connect-code.md#send-the-sign-in-request) .

Een voorbeeld van de Open-ID Connect-aanvraag is:

     https://login.microsoftonline.com/{tenant-id}/OAuth2/Authorize?client_id=a0448380-c346-4f9f-b897-c18733de9394&response_mode=form_post&response_type=code+id_token&redirect_uri=http%3a%2f%2fwww.vipswapper.com%2fcloudsense%2fAccount%2fSignIn&resource=https%3a%2f%2fgraph.windows.net%2f&scope=openid+profile&nonce=63567Dc4MDAw&domain_hint=live.com&state=M_12tMyKaM8

Azure AD verifieert de gebruiker en, indien nodig, vraagt de gebruiker wilt machtigen om de app. Wordt de code van de vergunning de antwoord-URL van uw toepassing. Afhankelijk van de gevraagde response_mode, Azure AD een queryaanvraag in de queryreeks of als post-gegevens.

Een voorbeeld van Open-ID Connect antwoord is:

    code=AAABAAAAiL*****I4rDWd7zXsH6WUjlkIEQxIAA&id_token=eyJ0eXAiOiJKV1Q*****T3GrzzSFxg&state=M_12tMyKaM8&session_state=2d16bbce-d5d1-443f-acdf-75f6b0ce8850

### <a name="token-request-oauth20-code-grant-flow"></a>Tokenaanvraag (OAuth2.0 Code subsidie stromen)

Nu uw toepassing heeft de autorisatiecode ontvangen uit Azure Active Directory, is het tijd om de toegang token voor Azure Resource Manager.  Boeken met een OAuth2.0 Code subsidie Token aanvragen naar het eindpunt Azure AD Token: 

    https://login.microsoftonline.com/{tenant-id}/OAuth2/Token

De queryreeks-parameters die beschikbaar voor deze aanvraag zijn worden beschreven in het onderwerp van [de code van de vergunning](./active-directory/active-directory-protocols-oauth-code.md#use-the-authorization-code-to-request-an-access-token) .

In het volgende voorbeeld ziet u een aanvraag voor subsidie-token met wachtwoord referentie code:

    POST https://login.microsoftonline.com/7fe877e6-a150-4992-bbfe-f517e304dfa0/oauth2/token HTTP/1.1

    Content-Type: application/x-www-form-urlencoded
    Content-Length: 1012

    grant_type=authorization_code&code=AAABAAAAiL9Kn2Z*****L1nVMH3Z5ESiAA&redirect_uri=http%3A%2F%2Flocalhost%3A62080%2FAccount%2FSignIn&client_id=a0448380-c346-4f9f-b897-c18733de9394&client_secret=olna84E8*****goScOg%3D

Als u werkt met referenties certificaat, een JSON Web Token (JWT) en teken (RSA SHA256) met behulp van de persoonlijke sleutel van een certificaat-referenties van uw toepassing maken. De typen voor het token worden [JWT token claims](./active-directory/active-directory-protocols-oauth-code.md#jwt-token-claims)weergegeven. Zie de [Active Directory Auth bibliotheek (.NET) code](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/blob/dev/src/ADAL.PCL.Desktop/CryptographyHelper.cs) voor het ondertekenen van tokens Client bevestiging JWT ter referentie.

Zie de [spec Open ID verbinding maken](http://openid.net/specs/openid-connect-core-1_0.html#ClientAuthentication) voor meer informatie over verificatie van de client. 

In het volgende voorbeeld ziet u een aanvraag voor subsidie-token met certificaat referentie code:

    POST https://login.microsoftonline.com/7fe877e6-a150-4992-bbfe-f517e304dfa0/oauth2/token HTTP/1.1
    
    Content-Type: application/x-www-form-urlencoded
    Content-Length: 1012
    
    grant_type=authorization_code&code=AAABAAAAiL9Kn2Z*****L1nVMH3Z5ESiAA&redirect_uri=http%3A%2F%2Flocalhost%3A62080%2FAccount%2FSignIn&client_id=a0448380-c346-4f9f-b897-c18733de9394&client_assertion_type=urn%3Aietf%3Aparams%3Aoauth%3Aclient-assertion-type%3Ajwt-bearer&client_assertion=eyJhbG*****Y9cYo8nEjMyA

Een voorbeeld van de respons voor code token verlenen: 

    HTTP/1.1 200 OK

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1432039858","not_before":"1432035958","resource":"https://management.core.windows.net/","access_token":"eyJ0eXAiOiJKV1Q****M7Cw6JWtfY2lGc5A","refresh_token":"AAABAAAAiL9Kn2Z****55j-sjnyYgAA","scope":"user_impersonation","id_token":"eyJ0eXAiOiJKV*****-drP1J3P-HnHi9Rr46kGZnukEBH4dsg"}

#### <a name="handle-code-grant-token-response"></a>Code subsidie token respons verwerken

Een positief antwoord token (gebruiker + app) bevat het toegangstoken voor Azure Resource Manager. De toepassing gebruikt dit toegangstoken Resource Manager namens de gebruiker toegang tot. De levensduur van toegangstokens uitgegeven door AD Azure is één uur. Het is onwaarschijnlijk dat uw webtoepassing vernieuwen moet (gebruiker + app)-toegangstoken. Als het toegangstoken te vernieuwen, gebruik de token vernieuwen die uw toepassing in het token antwoord krijgt. Boeken met een OAuth2.0 Token aanvragen naar het eindpunt Azure AD Token: 

    https://login.microsoftonline.com/{tenant-id}/OAuth2/Token

Te gebruiken met de aanvraag voor vernieuwing van de parameters worden beschreven in [het toegangstoken te vernieuwen](./active-directory/active-directory-protocols-oauth-code.md#refreshing-the-access-tokens).

In het volgende voorbeeld ziet u hoe u met de vernieuwing van de token:

    POST https://login.microsoftonline.com/7fe877e6-a150-4992-bbfe-f517e304dfa0/oauth2/token HTTP/1.1

    Content-Type: application/x-www-form-urlencoded
    Content-Length: 1012

    grant_type=refresh_token&refresh_token=AAABAAAAiL9Kn2Z****55j-sjnyYgAA&client_id=a0448380-c346-4f9f-b897-c18733de9394&client_secret=olna84E8*****goScOg%3D

Hoewel vernieuwen tokens kunnen worden gebruikt om nieuwe toegangstokens voor Azure Resource Manager, zijn ze niet geschikt zijn voor off line toegang door de toepassing. Beperkt de levensduur van de tokens vernieuwen en vernieuwen tokens zijn afhankelijk van de gebruiker. Als de gebruiker de organisatie verlaat, verliest de toepassing die gebruikmaakt van het token vernieuwen toegang. Deze benadering is niet geschikt voor toepassingen die worden gebruikt door de teams hun Azure bronnen beheren.

## <a name="check-if-user-can-assign-access-to-subscription"></a>Als een gebruiker toegang tot abonnement kunt geven controleren

Nu uw toepassing heeft een token Azure Resource Manager namens de gebruiker toegang. De volgende stap is uw app verbinding met het abonnement. Nadat u hebt aangesloten, uw app deze abonnementen kunt beheren zelfs als de gebruiker niet aanwezig is (off line toegang op lange termijn). 

Bel [Lijstmachtigingen Resource Manager](https://msdn.microsoft.com/library/azure/dn906889.aspx) API om te bepalen of de gebruiker het beheer van toegangsrechten voor het abonnement voor elk abonnement om verbinding te maken.

De methode [UserCanManagerAccessForSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L44) van het monster ASP.NET MVC app implementeert deze oproep.

In het volgende voorbeeld ziet u hoe de machtigingen van een gebruiker op een abonnement aanvragen. 83cfe939-2402-4581-b761-4f59b0a041e4 is de id van het abonnement.

    GET https://management.azure.com/subscriptions/83cfe939-2402-4581-b761-4f59b0a041e4/providers/microsoft.authorization/permissions?api-version=2015-07-01 HTTP/1.1

    Authorization: Bearer eyJ0eXAiOiJKV1QiLC***lwO1mM7Cw6JWtfY2lGc5A

Een voorbeeld van het antwoord voor de machtigingen van de gebruiker op abonnement is:

    HTTP/1.1 200 OK

    {"value":[{"actions":["*"],"notActions":["Microsoft.Authorization/*/Write","Microsoft.Authorization/*/Delete"]},{"actions":["*/read"],"notActions":[]}]}

De machtigingen voor API retourneert meerdere machtigingen. Elke machtiging bestaat uit toegestane acties (acties) en niet-toegestane acties (notactions). Als een actie in de lijst met toegestane acties van toestemming en niet aanwezig in de lijst met notactions van deze machtiging, wordt de gebruiker mag deze actie uit te voeren. **Microsoft.Authorization/RoleAssignments/Write** is de actie dat die toegang rights management verleent. Uw toepassing moet het resultaat van de machtigingen om te zoeken naar een overeenkomst regex op deze actie-tekenreeks in de acties en de notactions van elke machtiging parseren.

## <a name="get-app-only-access-token"></a>App alleen token ophalen

U weet nu, als de gebruiker toegang tot de Azure abonnement kunt geven. De volgende stappen zijn:

1. De juiste RBAC-rol toewijzen aan de identiteit van de toepassing op het abonnement.
2. Opvragen van de machtiging van de toepassing van het abonnement of bronnenbeheerder token gebruikt app alleen-lezen toegang tot de access-toewijzing worden gevalideerd.
1. De verbinding opnemen in uw toepassingen "verbonden abonnementen" gegevensstructuur - persistent maken de id van het abonnement.

We bekijken dichter bij de eerste stap. De juiste RBAC-rol toewijzen aan de identiteit van de toepassing, moet u het volgende vaststellen:

- De object-id van de identiteit van uw toepassing in van de gebruiker Azure Active Directory
- De id van de RBAC-rol die uw toepassing nodig op het abonnement heeft

Als uw aanvraag wordt geverifieerd door een gebruiker uit een Azure Active Directory, wordt een service principal-object voor de toepassing die Azure advertentie gemaakt. Azure kunt RBAC-rollen worden toegewezen aan de beveiligings-principals service directe toegang verlenen tot overeenkomstige toepassingen op Azure bronnen. Deze actie is precies wat we willen doen. De Azure AD Graph API query om te bepalen van de service van de hoofdsom van de toepassing in de aangemelde gebruiker-id's Azure AD.

U hoeft alleen een toegangstoken voor Azure Resource Manager - moet u een nieuwe toegangstoken de Azure AD Graph API aan te roepen. Iedere toepassing in Azure AD is gemachtigd een eigen service-principal object opvragen zodat alleen app-toegangstoken voldoende is.

<a id="app-azure-ad-graph">
### <a name="get-app-only-access-token-for-azure-ad-graph-api"></a>Alleen app voor Azure AD Graph API token ophalen

Als u wilt verifiëren uw app en een token voor Azure AD Graph API, geven een Client referenties subsidie OAuth2.0 tokenaanvraag voor stroom naar Azure AD token eindpunt (**https://login.microsoftonline.com/ {directory_domain_name} / OAuth2/Token**).

De methode [GetObjectIdOfServicePrincipalInOrganization](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureADGraphAPIUtil.cs) van de voorbeeldtoepassing ASP.net MVC haalt een app alleen-lezen toegang token voor Graph API met behulp van de bibliotheek voor verificatie van Active Directory voor .NET.

De queryreeks-parameters die beschikbaar voor deze aanvraag zijn worden beschreven in het onderwerp [een Access Token aanvragen](./active-directory/active-directory-protocols-oauth-service-to-service.md#request-an-access-token) .

Een voorbeeld van de aanvraag voor clientreferenties token verlenen: 

    POST https://login.microsoftonline.com/62e173e9-301e-423e-bcd4-29121ec1aa24/oauth2/token HTTP/1.1
    Content-Type: application/x-www-form-urlencoded
    Content-Length: 187</pre>
    <pre>grant_type=client_credentials&client_id=a0448380-c346-4f9f-b897-c18733de9394&resource=https%3A%2F%2Fgraph.windows.net%2F &client_secret=olna8C*****Og%3D

Een voorbeeld van de reactie van clientreferenties token verlenen: 

    HTTP/1.1 200 OK

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1432039862","not_before":"1432035962","resource":"https://graph.windows.net/","access_token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSIsImtpZCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSJ9.eyJhdWQiOiJodHRwczovL2dyYXBoLndpbmRv****G5gUTV-kKorR-pg"}

### <a name="get-objectid-of-application-service-principal-in-user-azure-ad"></a>Object-id van application service principal in Azure AD gebruiker ophalen

Nu het toegangstoken alleen app gebruik opvragen van de [beveiligings-Principals van AD-grafiek Azure Service](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity) API om te bepalen van de Object-Id van de toepassing van de service principal in Active directory.

De methode [GetObjectIdOfServicePrincipalInOrganization](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureADGraphAPIUtil.cs#) van de voorbeeldtoepassing ASP.net MVC implementeert deze oproep.

In het volgende voorbeeld ziet u hoe aanvragen van een toepassing service principal. a0448380-c346-4f9f-b897-c18733de9394 is de client-id van de toepassing.

    GET https://graph.windows.net/62e173e9-301e-423e-bcd4-29121ec1aa24/servicePrincipals?api-version=1.5&$filter=appId%20eq%20'a0448380-c346-4f9f-b897-c18733de9394' HTTP/1.1

    Authorization: Bearer eyJ0eXAiOiJK*****-kKorR-pg

In het volgende voorbeeld ziet u een antwoord op de aanvraag voor een toepassing de service principal 

    HTTP/1.1 200 OK

    {"odata.metadata":"https://graph.windows.net/62e173e9-301e-423e-bcd4-29121ec1aa24/$metadata#directoryObjects/Microsoft.DirectoryServices.ServicePrincipal","value":[{"odata.type":"Microsoft.DirectoryServices.ServicePrincipal","objectType":"ServicePrincipal","objectId":"9b5018d4-6951-42ed-8a92-f11ec283ccec","deletionTimestamp":null,"accountEnabled":true,"appDisplayName":"CloudSense","appId":"a0448380-c346-4f9f-b897-c18733de9394","appOwnerTenantId":"62e173e9-301e-423e-bcd4-29121ec1aa24","appRoleAssignmentRequired":false,"appRoles":[],"displayName":"CloudSense","errorUrl":null,"homepage":"http://www.vipswapper.com/cloudsense","keyCredentials":[],"logoutUrl":null,"oauth2Permissions":[{"adminConsentDescription":"Allow the application to access CloudSense on behalf of the signed-in user.","adminConsentDisplayName":"Access CloudSense","id":"b7b7338e-683a-4796-b95e-60c10380de1c","isEnabled":true,"type":"User","userConsentDescription":"Allow the application to access CloudSense on your behalf.","userConsentDisplayName":"Access CloudSense","value":"user_impersonation"}],"passwordCredentials":[],"preferredTokenSigningKeyThumbprint":null,"publisherName":"vipswapper"quot;,"replyUrls":["http://www.vipswapper.com/cloudsense","http://www.vipswapper.com","http://vipswapper.com","http://vipswapper.azurewebsites.net","http://localhost:62080"],"samlMetadataUrl":null,"servicePrincipalNames":["http://www.vipswapper.com/cloudsense","a0448380-c346-4f9f-b897-c18733de9394"],"tags":["WindowsAzureActiveDirectoryIntegratedApp"]}]}

### <a name="get-azure-rbac-role-identifier"></a>Azure RBAC-rol-id ophalen

Om de juiste RBAC-rol toewijzen aan uw service principal, moet u de id van de Azure RBAC-rol bepalen.

De juiste RBAC-rol voor uw toepassing:

- Als uw toepassing alleen het abonnement, bewaakt zonder wijzigingen, moet leesmachtigingen voor het abonnement. De rol van **lezer** toegewezen.
- Als uw toepassing Azure het abonnement, entiteiten maken/wijzigen/verwijderen beheert, moet een van de machtigingen van de Inzender.
  - Voor het beheren van een bepaald type resource rollen toe te wijzen de inzender specifiek (Inzender virtuele Machine, Virtual Network Inzender, opslag Account Inzender, enz.)
  - Toewijzen voor het beheer van elk resourcetype, de rol van **Inzender** .

De roltoewijzing voor de toepassing is zichtbaar voor gebruikers selecteren de minst vereiste bevoegdheid.

Roept de [bronnenbeheerder roldefinitie API](https://msdn.microsoft.com/library/azure/dn906879.aspx) om alle Azure RBAC-rollen en zoeken en vervolgens het resultaat aan de definitie van de gewenste rol zoeken op naam doorlopen.

De methode [GetRoleId](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L246) van het monster ASP.net MVC app implementeert deze oproep.

In het volgende verzoek-voorbeeld ziet u hoe Azure RBAC-rol-id ophalen. 09cbd307-aa71-4aca-b346-5f253e6e3ebb is de id van het abonnement.

    GET https://management.azure.com/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions?api-version=2015-07-01 HTTP/1.1

    Authorization: Bearer eyJ0eXAiOiJKV*****fY2lGc5

Het antwoord is in de volgende notatie: 

    HTTP/1.1 200 OK

    {"value":[{"properties":{"roleName":"API Management Service Contributor","type":"BuiltInRole","description":"Lets you manage API Management services, but not access to them.","scope":"/","permissions":[{"actions":["Microsoft.ApiManagement/Services/*","Microsoft.Authorization/*/read","Microsoft.Resources/subscriptions/resources/read","Microsoft.Resources/subscriptions/resourceGroups/read","Microsoft.Resources/subscriptions/resourceGroups/resources/read","Microsoft.Resources/subscriptions/resourceGroups/deployments/*","Microsoft.Insights/alertRules/*","Microsoft.Support/*"],"notActions":[]}]},"id":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions/312a565d-c81f-4fd8-895a-4e21e48d571c","type":"Microsoft.Authorization/roleDefinitions","name":"312a565d-c81f-4fd8-895a-4e21e48d571c"},{"properties":{"roleName":"Application Insights Component Contributor","type":"BuiltInRole","description":"Lets you manage Application Insights components, but not access to them.","scope":"/","permissions":[{"actions":["Microsoft.Insights/components/*","Microsoft.Insights/webtests/*","Microsoft.Authorization/*/read","Microsoft.Resources/subscriptions/resources/read","Microsoft.Resources/subscriptions/resourceGroups/read","Microsoft.Resources/subscriptions/resourceGroups/resources/read","Microsoft.Resources/subscriptions/resourceGroups/deployments/*","Microsoft.Insights/alertRules/*","Microsoft.Support/*"],"notActions":[]}]},"id":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions/ae349356-3a1b-4a5e-921d-050484c6347e","type":"Microsoft.Authorization/roleDefinitions","name":"ae349356-3a1b-4a5e-921d-050484c6347e"}]}

U hoeft niet deze API aanroepen doorlopend. Nadat u hebt vastgesteld dat de bekende GUID van de roldefinitie van de, kunt u de definitie van rol-id als samenstellen:

    /subscriptions/{subscription_id}/providers/Microsoft.Authorization/roleDefinitions/{well-known-role-guid}

Hier vindt u de bekende GUID's van algemeen gebruikte ingebouwde functies:

| Rol | GUID |
| ----- | ------ |
| Reader | acdd72a7-3385-48ef-bd42-f606fba81ae7
| Inzender | b24988ac-6180-42a0-ab88-20f7382dd24c
| Virtuele Machine Inzender | d73bb868-a0df-4d4d-bd69-98a00b01fccb
| Virtueel netwerk Inzender | b34d265f-36f7-4a0d-a4d4-e158ca92e90f
| Opslag Account Inzender | 86e8f5dc-a6e9-4c67-9d15-de283e8eac25
| Website-medewerker | de139f84-1756-47ae-9be6-808fbbe84772
| Web Plan Inzender | 2cc479cb-7b4d-49a8-b449-8c00fd0f0a4b
| SQL Server Inzender | 6d8ee4ec-f05a-4a1d-8b00-a9b17e38b437
| SQL DB Inzender | 9b7fa17d-e63e-47b0-bb0a-15c516ac86ec

### <a name="assign-rbac-role-to-application"></a>RBAC-rol toewijzen aan toepassingen

Hebt u alles wat die u nodig hebt om de juiste RBAC-rol toewijzen aan uw service principal met behulp van de [bronnenbeheerder roltoewijzing maken](https://msdn.microsoft.com/library/azure/dn906887.aspx) API.

De methode [GrantRoleToServicePrincipalOnSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L170) van het monster ASP.net MVC app implementeert deze oproep.

Een voorbeeld van de aanvraag RBAC-rol toewijzen aan toepassingen: 

    PUT https://management.azure.com/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/microsoft.authorization/roleassignments/4f87261d-2816-465d-8311-70a27558df4c?api-version=2015-07-01 HTTP/1.1

    Authorization: Bearer eyJ0eXAiOiJKV1QiL*****FlwO1mM7Cw6JWtfY2lGc5
    Content-Type: application/json
    Content-Length: 230

    {"properties": {"roleDefinitionId":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7","principalId":"c3097b31-7309-4c59-b4e3-770f8406bad2"}}

In de aanvraag, worden de volgende waarden gebruikt:

| GUID | Beschrijving |
| ------ | --------- |
| 09cbd307-aa71-4aca-b346-5f253e6e3ebb | de id van het abonnement
| c3097b31-7309-4c59-b4e3-770f8406bad2 | de object-id van de service principal van de toepassing
| acdd72a7-3385-48ef-bd42-f606fba81ae7 | de id van de rol van lezer
| 4f87261d-2816-465d-8311-70a27558df4c | een nieuwe guid voor de nieuwe roltoewijzing gemaakt

Het antwoord is in de volgende notatie: 

    HTTP/1.1 201 Created

    {"properties":{"roleDefinitionId":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7","principalId":"c3097b31-7309-4c59-b4e3-770f8406bad2","scope":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb"},"id":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleAssignments/4f87261d-2816-465d-8311-70a27558df4c","type":"Microsoft.Authorization/roleAssignments","name":"4f87261d-2816-465d-8311-70a27558df4c"}

### <a name="get-app-only-access-token-for-azure-resource-manager"></a>App alleen-lezen toegang krijgen token voor Azure Resource Manager

Om te valideren dat app is het gewenste access op het abonnement, het uitvoeren van een test op het abonnement met een toegangstoken op app.

Als u een alleen-app toegang token, volg de aanwijzingen van de sectie [alleen-app toegang token voor Azure AD Graph API krijgen](#app-azure-ad-graph), met een andere waarde voor de parameter resource: 

    https://management.core.windows.net/

De methode [ServicePrincipalHasReadAccessToSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L110) van de voorbeeldtoepassing ASP.NET MVC haalt een app alleen-lezen toegang token voor Azure Resource Manager met behulp van de bibliotheek voor verificatie van Active Directory voor .net.

#### <a name="get-applications-permissions-on-subscription"></a>De machtigingen van toepassing op abonnement ophalen

Om te controleren dat uw toepassing de gewenste toegang op een Azure-abonnement heeft, kan u ook [Machtigingen voor de Resource Manager](https://msdn.microsoft.com/library/azure/dn906889.aspx) API aanroepen. Deze benadering is vergelijkbaar met hoe u bepaald of de gebruiker toegang tot beheer van beheerrechten voor het abonnement heeft. Deze keer roept echter de machtigingen API met de app alleen token die u in de vorige stap hebt ontvangen.

De methode [ServicePrincipalHasReadAccessToSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L110) van het monster ASP.NET MVC app implementeert deze oproep.

## <a name="manage-connected-subscriptions"></a>Verbonden abonnementen beheren

Wanneer de juiste RBAC-rol is toegewezen aan een van uw toepassing service principal op het abonnement, kunt uw toepassing houden toezicht/beheren met alleen app toegangstokens voor Azure Resource Manager.

Als eigenaar van een abonnement worden verwijderd van uw toepassing roltoewijzing via de klassieke portal of opdrachtregelprogramma's, is niet langer uw toepassing toegang kunnen krijgen tot abonnement. In dat geval moet u kennis van de gebruiker die de verbinding met het abonnement is afgescheiden van buiten de toepassing en geef ze een optie te 'repareren' van de verbinding. 'Repair' zou simpelweg opnieuw maken met de roltoewijzing die off line is verwijderd.

Net als u de gebruiker verbinding maken met abonnementen aan uw toepassing hebt ingeschakeld, moet u toestaan dat de gebruiker om abonnementen te verbreken. Verbreken van een access-beheer betekent dat de toewijzing van rol van de toepassing service principal op het abonnement heeft te verwijderen. Desgewenst kan elke staat in de toepassing voor het abonnement te worden verwijderd. Alleen gebruikers met toegangsmachtigingen voor beheer van het abonnement kunnen loskoppelen van het abonnement.

De [methode RevokeRoleFromServicePrincipalOnSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L200) van het monster ASP.net MVC app implementeert deze oproep.

Dat is het - gebruikers kunnen nu eenvoudig verbinding maken en beheren hun Azure abonnementen met uw toepassing.

