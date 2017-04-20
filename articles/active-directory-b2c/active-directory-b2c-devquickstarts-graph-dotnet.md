<properties
    pageTitle="Azure Active Directory B2C: Gebruik de Graph API | Microsoft Azure"
    description="Hoe u de Graph API aanroepen voor een huurder B2C met behulp van een toepassings-id om het proces te automatiseren."
    services="active-directory-b2c"
    documentationCenter=".net"
    authors="dstrockis"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/30/2016"
    ms.author="dastrock"/>

# <a name="azure-ad-b2c-use-the-graph-api"></a>Azure AD B2C: De Graph API gebruiken

Azure Active Directory (AD Azure) B2C huurders zijn meestal erg groot. Dit betekent dat veel algemene beheertaken voor huurder moeten via programmacode worden uitgevoerd. Een voorbeeld van de primaire is gebruiker management. Mogelijk moet u voor het migreren van een bestaand gebruikersarchief naar een huurder B2C. U kunt de gebruikersregistratie op de pagina en maakt gebruikersaccounts in Azure AD achter de schermen. Dit soort taken nodig hebben kunnen maken, lezen, bijwerken en verwijderen van gebruikersaccounts. U voert deze taken met de Azure AD Graph API.

Er zijn twee primaire manieren om te communiceren met de API van de grafiek voor B2C-huurders.

- Voor taken, interactieve, eenmaal wordt uitgevoerd, moet u fungeren als een administrator-account in de huurder B2C wanneer u de taken uitvoert. In deze modus moet een beheerder aan te melden met de referenties voordat die admin oproepen aan de Graph API kunt uitvoeren.
- Voor geautomatiseerde, continue taken, moet u een type service-account dat u voorzien van de benodigde bevoegdheden voor het uitvoeren van beheertaken. In Azure AD, kunt u dit doen door een toepassing geregistreerd en geverifieerd bij AD Azure. Dit gebeurt met behulp van een **Toepassings-ID** die wordt gebruikt de [referenties van de client 2.0 OAuth verlenen](../active-directory/active-directory-authentication-scenarios.md#daemon-or-server-application-to-web-api). In dit geval fungeert de toepassing als zichzelf niet als een gebruiker de Graph API aan te roepen.

In dit artikel hebben bespreken we het uitvoeren van de automatisch-use-case. Om aan te tonen, zullen wij bouwen een .NET 4.5 `B2CGraphClient` die de gebruiker uitvoert maken, lezen, bijwerken en verwijderen (CRUD). De client moet een Windows opdrachtregelinterface (CLI) waarmee u verschillende methoden aanroepen. De code is echter om te werken in een niet-interactieve, geautomatiseerde manier geschreven.

## <a name="get-an-azure-ad-b2c-tenant"></a>Een huurder Azure AD B2C ophalen

Voordat u kunt toepassingen of gebruikers maken of op alle interactie met Azure Active Directory, moet u een huurder Azure AD B2C en een globale administrator-account in de pachter. Als u niet over een huurder al [aan de slag met Azure AD B2C](active-directory-b2c-get-started.md).

## <a name="register-a-service-application-in-your-tenant"></a>Een application service registreren in uw huurder

Nadat u een huurder B2C hebt, moet u uw servicetoepassing maken met Azure AD PowerShell-cmdlets.
Eerst, download en installeer de [Microsoft Online Services-In-assistent](http://go.microsoft.com/fwlink/?LinkID=286152). Vervolgens downloaden en installeren van de [64-bits Azure Active Directory-module voor Windows PowerShell](http://go.microsoft.com/fwlink/p/?linkid=236297).

> [AZURE.IMPORTANT]
Gebruik de API van de grafiek met uw huurder B2C, moet u een speciale toepassing registreren met PowerShell. Volg de instructies in dit artikel dat moet doen. U kunt de reeds bestaande B2C toepassingen die u hebt geregistreerd in Azure portal niet opnieuw gebruiken.

Na de installatie van de module PowerShell PowerShell openen en verbinding maken met uw huurder B2C. Nadat u `Get-Credential`, wordt u gevraagd voor een gebruikersnaam en wachtwoord, voert u de gebruikersnaam en het wachtwoord van de administrator-account van uw B2C-huurder.

```
> $msolcred = Get-Credential
> Connect-MsolService -credential $msolcred
```

Voordat u uw toepassing maakt, moet u een nieuw **geheim client**genereren.  De toepassing gebruikt het geheim van de client worden geverifieerd bij AD Azure en toegangstokens verkrijgen. U kunt een geldige geheim in PowerShell genereren:

```
> $bytes = New-Object Byte[] 32
> $rand = [System.Security.Cryptography.RandomNumberGenerator]::Create()
> $rand.GetBytes($bytes)
> $rand.Dispose()
> $newClientSecret = [System.Convert]::ToBase64String($bytes)
> $newClientSecret
```

De laatste opdracht het nieuwe geheim van de client moet worden afgedrukt. Kopieer het ergens veilig. U zult het later nodig hebt. Nu kunt u uw toepassing met behulp van het nieuwe geheim van de client als referentie voor de app:

```
> New-MsolServicePrincipal -DisplayName "My New B2C Graph API App" -Type password -Value $newClientSecret

DisplayName           : My New B2C Graph API App
ServicePrincipalNames : {dd02c40f-1325-46c2-a118-4659db8a55d5}
ObjectId              : e2bde258-6691-410b-879c-b1f88d9ef664
AppPrincipalId        : dd02c40f-1325-46c2-a118-4659db8a55d5
TrustedForDelegation  : False
AccountEnabled        : True
Addresses             : {}
KeyType               : Password
KeyId                 : a261e39d-953e-4d6a-8d70-1f915e054ef9
StartDate             : 9/2/2015 1:33:09 AM
EndDate               : 9/2/2016 1:33:09 AM
Usage                 : Verify
```

Als u de toepassing maakt, moet deze eigenschappen van de toepassing, zoals de bovenstaande afdrukken. U moet beide `ObjectId` en `AppPrincipalId`, dus deze waarden te kopiëren.

Nadat u een toepassing in uw huurder B2C maakt, moet u de machtigingen die nodig zijn voor bewerkingen van gebruikers CRUD toewijzen. De toepassing drie rollen toewijzen: directory lezers (gebruikers lezen), de schrijvers van de directory (voor het maken en bijwerken van gebruikers), en de beheerder van een gebruiker (om gebruikers te verwijderen). Deze rollen zijn bekende id's, dus u vervangt de `-RoleMemberObjectId` parameter met `ObjectId` van boven en voer de volgende opdrachten. Voor een overzicht van alle directory-rollen, probeer uit te voeren `Get-MsolRole`.

```
> Add-MsolRoleMember -RoleObjectId 88d8e3e3-8f55-4a1e-953a-9b9898b8876b -RoleMemberObjectId <Your-ObjectId> -RoleMemberType servicePrincipal
> Add-MsolRoleMember -RoleObjectId 9360feb5-f418-4baa-8175-e2a00bac4301 -RoleMemberObjectId <Your-ObjectId> -RoleMemberType servicePrincipal
> Add-MsolRoleMember -RoleObjectId fe930be7-5e62-47db-91af-98c3a49a38b1 -RoleMemberObjectId <Your-ObjectId> -RoleMemberType servicePrincipal
```

U nu een toepassing hebt die gemachtigd om te maken is, lezen, bijwerken en verwijderen van gebruikers uit uw huurder B2C.

## <a name="download-configure-and-build-the-sample-code"></a>Downloaden, configureren en bouwen van de voorbeeldcode

Eerst de voorbeeldcode downloaden en deze uitvoeren. Vervolgens gaat we dichter bij kijken.  U kunt [de voorbeeldcode in een ZIP-bestand downloaden](https://github.com/AzureADQuickStarts/B2C-GraphAPI-DotNet/archive/master.zip). U kunt ook het klonen in een map van uw keuze:

```
git clone https://github.com/AzureADQuickStarts/B2C-GraphAPI-DotNet.git
```

Open de `B2CGraphClient\B2CGraphClient.sln` Visual Studio-oplossing in Visual Studio. In de `B2CGraphClient` project, opent u het bestand `App.config`. De drie app instellingen vervangen door uw eigen waarden:

```
<appSettings>
    <add key="b2c:Tenant" value="contosob2c.onmicrosoft.com" />
    <add key="b2c:ClientId" value="{The AppPrincipalId from above}" />
    <add key="b2c:ClientSecret" value="{The client secret you generated above}" />
</appSettings>
```

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

Vervolgens met de rechtermuisknop op de `B2CGraphClient` -oplossing en het opnieuw opbouwen van het monster. Als u geslaagd bent, u hebt nu een `B2C.exe` uitvoerbare bestand zich bevindt in `B2CGraphClient\bin\Debug`.

## <a name="build-user-crud-operations-by-using-the-graph-api"></a>Bewerkingen van gebruikers CRUD bouwen met de API van de grafiek

Open voor het gebruik van de B2CGraphClient, een `cmd` Windows command prompt en de map te wijzigen de `Debug` directory. Voer het `B2C Help` opdracht.

```
> cd B2CGraphClient\bin\Debug
> B2C Help
```

Een korte beschrijving van elke opdracht wordt weergegeven. Telkens wanneer u een van deze opdrachten oproepen `B2CGraphClient` een aanvraag indient voor de Azure AD Graph API.

### <a name="get-an-access-token"></a>Een toegangstoken ophalen

Elk verzoek om de Graph API vereist een toegangstoken voor verificatie. `B2CGraphClient`de open source Active Directory verificatie bibliotheek (ADAL) gebruikt om te verwerven toegangstokens. ADAL gemakkelijker token verkrijgen door een eenvoudige API en verzorgen van enkele belangrijke details, zoals caching toegangstokens. U hebt geen ADAL met tokens, hoewel opgehaald. U kunt ook de tokens ophalen door HTTP-aanvragen.

> [AZURE.NOTE]
    Deze voorbeeldcode wordt gebruikgemaakt van ADAL v2 voor de communicatie met de API van de grafiek.  U moet ADAL v2 of v3 om toegangstokens die kunnen worden gebruikt met de Azure AD Graph API gebruiken.

Wanneer `B2CGraphClient` wordt uitgevoerd, maakt u een exemplaar van de `B2CGraphClient` klasse. De constructor voor deze klasse wordt ingesteld van een steiger ADAL verificatie:

```C#
public B2CGraphClient(string clientId, string clientSecret, string tenant)
{
    // The client_id, client_secret, and tenant are provided in Program.cs, which pulls the values from App.config
    this.clientId = clientId;
    this.clientSecret = clientSecret;
    this.tenant = tenant;

    // The AuthenticationContext is ADAL's primary class, in which you indicate the tenant to use.
    this.authContext = new AuthenticationContext("https://login.microsoftonline.com/" + tenant);

    // The ClientCredential is where you pass in your client_id and client_secret, which are
    // provided to Azure AD in order to receive an access_token by using the app's identity.
    this.credential = new ClientCredential(clientId, clientSecret);
}
```

We gebruiken de `B2C Get-User` als een voorbeeld van de opdracht. Wanneer `B2C Get-User` wordt aangeroepen zonder alle extra ingangen, het oproepen van de CLI de `B2CGraphClient.GetAllUsers(...)` methode. Deze methode roept `B2CGraphClient.SendGraphGetRequest(...)`, die een HTTP GET-verzoek naar de Graph API indient. Voordat `B2CGraphClient.SendGraphGetRequest(...)` verzendt de GET-aanvragen, het voor de eerste keer een token met behulp van ADAL:

```C#
public async Task<string> SendGraphGetRequest(string api, string query)
{
    // First, use ADAL to acquire a token by using the app's identity (the credential)
    // The first parameter is the resource we want an access_token for; in this case, the Graph API.
    AuthenticationResult result = authContext.AcquireToken("https://graph.windows.net", credential);

    ...

```

U krijgt een token voor de Graph API door het aanroepen van de ADAL `AuthenticationContext.AcquireToken(...)` methode. ADAL geeft als resultaat een `access_token` dat de identiteit van de toepassing vertegenwoordigt.

### <a name="read-users"></a>Gebruikers lezen

Als u wilt een lijst met gebruikers of een bepaalde gebruiker ophalen uit de Graph API, kunt u verzenden met een HTTP `GET` vragen aan de `/users` eindpunt. Een aanvraag voor alle gebruikers in een huurder ziet er zo uit:

```
GET https://graph.windows.net/contosob2c.onmicrosoft.com/users?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
```

Overzicht van deze aanvraag worden uitgevoerd:

 ```
 > B2C Get-User
 ```

Er zijn twee belangrijke dingen om te weten:

- Het toegangstoken verkregen via ADAL wordt toegevoegd aan de `Authorization` kop met behulp van de `Bearer` schema.
- Voor B2C huurders, moet u de query-parameter `api-version=1.6`.

Beide van deze gegevens worden verwerkt in de `B2CGraphClient.SendGraphGetRequest(...)` methode:

```C#
public async Task<string> SendGraphGetRequest(string api, string query)
{
    ...

    // For B2C user management, be sure to use the 1.6 Graph API version.
    HttpClient http = new HttpClient();
    string url = "https://graph.windows.net/" + tenant + api + "?" + "api-version=1.6";
    if (!string.IsNullOrEmpty(query))
    {
        url += "&" + query;
    }

    // Append the access token for the Graph API to the Authorization header of the request by using the Bearer scheme.
    HttpRequestMessage request = new HttpRequestMessage(HttpMethod.Get, url);
    request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);
    HttpResponseMessage response = await http.SendAsync(request);

    ...
```

### <a name="create-consumer-user-accounts"></a>Consument gebruikersaccounts maken

Als u gebruikersaccounts in uw huurder B2C maakt, kunt u verzenden met een HTTP `POST` vragen aan de `/users` eindpunt:

```
POST https://graph.windows.net/contosob2c.onmicrosoft.com/users?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
Content-Type: application/json
Content-Length: 338

{
    // All of these properties are required to create consumer users.

    "accountEnabled": true,
    "signInNames": [                            // controls which identifier the user uses to sign in to the account
        {
            "type": "emailAddress",             // can be 'emailAddress' or 'userName'
            "value": "joeconsumer@gmail.com"
        }
    ],
    "creationType": "LocalAccount",            // always set to 'LocalAccount'
    "displayName": "Joe Consumer",              // a value that can be used for displaying to the end user
    "mailNickname": "joec",                     // an email alias for the user
    "passwordProfile": {
        "password": "P@ssword!",
        "forceChangePasswordNextLogin": false   // always set to false
    },
    "passwordPolicies": "DisablePasswordExpiration"
}
```

De meeste van deze eigenschappen in deze aanvraag zijn vereist om gebruikers van de consument te maken. Voor meer informatie, klik [hier](https://msdn.microsoft.com/library/azure/ad/graph/api/users-operations#CreateLocalAccountUser). Houd er rekening mee dat de `//` opmerkingen zijn opgenomen voor een afbeelding. Neem deze dus niet in een werkelijke aanvraag.

Overzicht van de aanvraag een van de volgende opdrachten worden uitgevoerd:

```
> B2C Create-User ..\..\..\usertemplate-email.json
> B2C Create-User ..\..\..\usertemplate-username.json
```

De `Create-User` opdracht wordt een .json-bestand als een invoerparameter. Dit document bevat een JSON representatie van een gebruikersobject. Er zijn twee monster .json-bestanden in de voorbeeldcode: `usertemplate-email.json` en `usertemplate-username.json`. U kunt deze bestanden aan uw behoeften aanpassen. Naast de bovenstaande vereiste velden zijn kunt u verschillende optionele velden opgenomen in deze bestanden. Meer informatie over de optionele velden vindt u in de [verwijzing naar Azure AD Graph API-entiteit](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#user-entity).

Kunt u zien hoe de POST-aanvraag is gebouwd `B2CGraphClient.SendGraphPostRequest(...)`.

- Het raakpunt van een toegangstoken voor de `Authorization` de kop van de aanvraag.
- Wordt `api-version=1.6`.
- Bevat het gebruikersobject JSON in het hoofdgedeelte van de aanvraag.

> [AZURE.NOTE]
Als de accounts die u wilt migreren van een bestaand gebruikersarchief heeft lagere Wachtwoordsterkte dan de [sterkte van wachtwoord afgedwongen door Azure AD B2C](https://msdn.microsoft.com/library/azure/jj943764.aspx), kunt u uitschakelen sterk wachtwoord vereiste via de `DisableStrongPassword` de waarde in het `passwordPolicies` eigenschap. Bijvoorbeeld, kunt u de aanvraag van de gebruiker maken hierboven beschreven als volgt: `"passwordPolicies": "DisablePasswordExpiration, DisableStrongPassword"`.

### <a name="update-consumer-user-accounts"></a>Bijwerken van de gebruikersaccounts van de consument

Tijdens het bijwerken van gebruikersobjecten, is het proces vergelijkbaar met gebruikersobjecten maken wilt gebruiken. Maar dit proces gebruikt de HTTP `PATCH` methode:

```
PATCH https://graph.windows.net/contosob2c.onmicrosoft.com/users/<user-object-id>?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
Content-Type: application/json
Content-Length: 37

{
    "displayName": "Joe Consumer",              // this request updates only the user's displayName
}
```

Probeert bij te werken van een gebruiker uw JSON bestanden bijwerken met nieuwe gegevens. U kunt `B2CGraphClient` een van deze opdrachten uit te voeren:

```
> B2C Update-User <user-object-id> ..\..\..\usertemplate-email.json
> B2C Update-User <user-object-id> ..\..\..\usertemplate-username.json
```

Bekijk de `B2CGraphClient.SendGraphPatchRequest(...)` voor meer informatie over het verzenden van deze aanvraag.

### <a name="search-users"></a>Gebruikers zoeken

U kunt zoeken naar gebruikers in uw huurder B2C in een aantal manieren. Een van de gebruiker met object-ID of twee, met behulp van de gebruiker aanmelden id (dat wil zeggen, de `signInNames` eigenschap).

Een van de volgende opdrachten om te zoeken naar een specifieke gebruiker worden uitgevoerd:

```
> B2C Get-User <user-object-id>
> B2C Get-User <filter-query-expression>
```

Hier volgen enkele voorbeelden:

```
> B2C Get-User 2bcf1067-90b6-4253-9991-7f16449c2d91
> B2C Get-User $filter=signInNames/any(x:x/value%20eq%20%27joeconsumer@gmail.com%27)
```

### <a name="delete-users"></a>Gebruikers verwijderen

Het proces voor het verwijderen van een gebruiker is eenvoudig. Gebruik het HTTP `DELETE` methode en constructie de URL met de juiste object-ID:

```
DELETE https://graph.windows.net/contosob2c.onmicrosoft.com/users/<user-object-id>?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
```

Voer de volgende opdracht om een voorbeeld en het delete-verzoek dat wordt afgedrukt om de console te bekijken:

```
> B2C Delete-User <object-id-of-user>
```

Bekijk de `B2CGraphClient.SendGraphDeleteRequest(...)` voor meer informatie over het verzenden van deze aanvraag.

U kunt met de Azure AD Graph API naast Gebruikersbeheer veel acties uitvoeren. De [Azure AD Graph API reference](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) bevat details van elke actie, bij voorbeeld aanvragen.

## <a name="use-custom-attributes"></a>Gebruik aangepaste kenmerken

De meeste consumenten toepassingen moeten sommige type aangepaste informatie over gebruikersprofielen worden opgeslagen. Een manier waarop u dit kunt doen is het definiëren van een aangepast attribuut in uw huurder B2C. U kunt dit kenmerk vervolgens dezelfde manier als u andere eigenschappen van een gebruikersobject behandelen behandelen. U kunt het kenmerk bijwerken, verwijdert u het kenmerk, opvragen door het attribuut, het kenmerk als een claim in tokens voor aanmelden en meer verzenden.

Zie een aangepast attribuut definiëren in uw huurder B2C, [B2C aangepaste verwijzing naar](active-directory-b2c-reference-custom-attr.md).

Ziet u de aangepaste kenmerken in de huurder B2C gedefinieerd met `B2CGraphClient`:

```
> B2C Get-B2C-Application
> B2C Get-Extension-Attribute <object-id-in-the-output-of-the-above-command>
```

De uitvoer van deze functies geeft de details van elk aangepast kenmerk, zoals:

```JSON
{
      "odata.type": "Microsoft.DirectoryServices.ExtensionProperty",
      "objectType": "ExtensionProperty",
      "objectId": "cec6391b-204d-42fe-8f7c-89c2b1964fca",
      "deletionTimestamp": null,
      "appDisplayName": "",
      "name": "extension_55dc0861f9a44eb999e0a8a872204adb_Jersey_Number",
      "dataType": "Integer",
      "isSyncedFromOnPremises": false,
      "targetObjects": [
        "User"
      ]
}
```

U kunt de volledige naam, zoals `extension_55dc0861f9a44eb999e0a8a872204adb_Jersey_Number`, als een eigenschap van de gebruikersobjecten.  De .json-bestand bijwerken met de nieuwe eigenschap en een waarde voor de eigenschap en voer vervolgens:

```
> B2C Update-User <object-id-of-user> <path-to-json-file>
```

Met behulp van `B2CGraphClient`, hebt u een service-toepassing die de huurder B2C gebruikers via de programmacode kunt beheren. `B2CGraphClient`de eigen toepassingsidentiteit gebruikt voor verificatie bij de Azure AD Graph API. Ook verwerft tokens met behulp van een geheim van de client. Als u deze functionaliteit in uw toepassing opnemen, houd rekening met enkele belangrijke punten voor B2C-toepassingen:

- U moet de toepassing de juiste machtigingen in de huurder verlenen.
- Nu moet u ADAL v2 toegangstokens ophalen gebruiken. (U kunt ook protocolberichten verzenden direct, zonder gebruik van een bibliotheek.)
- Wanneer u de Graph API aanroept, gebruikt u `api-version=1.6`.
- Bij het maken en bijwerken van gebruikers van de consument zijn enkele eigenschappen vereist, zoals hierboven beschreven.

Hebt u vragen of verzoeken voor acties die u wilt uitvoeren met behulp van de API van de grafiek op de huurder B2C, een reactie achterlaten op dit artikel of een probleem in de opslagplaats van GitHub code voorbeeld bestand.
