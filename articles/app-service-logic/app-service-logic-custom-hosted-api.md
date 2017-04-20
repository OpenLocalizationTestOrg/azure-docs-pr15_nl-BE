<properties
    pageTitle="Een aangepaste API aanroepen in logica apps"
    description="Met behulp van uw aangepaste API gehost op App Service met logica apps"
    authors="stepsic-microsoft-com"
    manager="dwrede"
    editor=""
    services="logic-apps"
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/31/2016"
    ms.author="stepsic"/>

# <a name="using-your-custom-api-hosted-on-app-service-with-logic-apps"></a>Met behulp van uw aangepaste API gehost op App Service met logica apps

Hoewel logica apps heeft een uitgebreide set van 40 + aansluitingen voor tal van services, kunt u bellen in uw eigen aangepaste API die door uw eigen code kunt uitvoeren. Een van de eenvoudigste en meest schaalbare methoden voor het hosten van uw eigen aangepaste web-API's is het App-Service. In dit artikel wordt beschreven hoe u een web API die wordt gehost in een app, web app of mobiele app in App Service API aanroepen.

Bekijk [dit artikel](app-service-logic-create-api-app.md)voor meer informatie over het bouwen van API's als een trigger of actie in de logica apps.

## <a name="deploy-your-web-app"></a>Implementatie van uw Web App

Eerst moet u uw API implementeren als een Web App in de App-Service. Eenvoudige distributie betrekking hebben op de volgende instructies: [een ASP.NET-webtoepassing maken](../app-service-web/web-sites-dotnet-get-started.md).  Terwijl u API's vanuit een app logica aanroepen kunt, voor het beste resultaat wordt aangeraden dat u metagegevens Swagger eenvoudig integreren met logica apps acties toevoegen.  Vindt u meer informatie over het [toevoegen van swagger](../app-service-api/app-service-api-dotnet-get-started.md#use-swagger-api-metadata-and-ui).

### <a name="api-settings"></a>API-instellingen

Om de logica apps designer parseren van uw Swagger, is het belangrijk dat u CORS inschakelen en de APIDefinition eigenschappen van uw web app.  Dit is heel eenvoudig in te stellen binnen de Portal Azure.  Gewoon het blad van de instellingen van uw Web App openen onder de sectie API stelt de 'API definitie' aan de URL van het swagger.json-bestand (dit is meestal https://{name}.azurewebsites.net/swagger/docs/v1) en CORS beleid voor toevoegen ' *' voor het aanvragen van de logica apps Designer.

## <a name="calling-into-the-api"></a>Aanroepen van de API

Wanneer binnen de logica apps portal, als u CORS hebt ingesteld en de definitie van API-eigenschappen u kunnen gemakkelijk moet toevoegen API voor aangepaste acties in de stroom.  In het ontwerp kunt u naar uw abonnement websites om de websites met een URL swagger is gedefinieerd.  Ook kunt u het HTTP + Swagger actie wijst een swagger en een overzicht van beschikbare acties en ingangen.  Ten slotte kunt u altijd een API die niet heeft of een doc swagger blootstellen aan te roepen met behulp van de actie HTTP-verzoek maken.

Als u beveiligen uw API wilt, klik zijn er een aantal verschillende manieren doen:

1. Geen codewijziging vereist - Azure Active Directory kan worden gebruikt ter bescherming van uw API zonder codewijzigingen of opnieuw installeren.
1. Eenvoudige verificatie, AAD Auth of certificaat Auth afdwingen in de code van de API.

## <a name="securing-calls-to-your-api-without-a-code-change"></a>Aanroepen van de API zonder een codewijziging te beveiligen

In dit gedeelte maakt u twee Azure Active Directory-toepassingen: één voor uw app logica en één voor uw Web App.  U zult gesprekken naar uw Web App met behulp van de service-principal die is gekoppeld aan de toepassing AAD voor uw app logica (client-id en geheim) verifiëren. Ten slotte, zult u de toepassing opnemen in de definitie van de logica app id.

### <a name="part-1-setting-up-an-application-identity-for-your-logic-app"></a>Deel 1: Instellen van een toepassings-id voor uw app logica

Dit is de app logica wordt gebruikt voor verificatie met active directory. Alleen u *nodig hebt* om dit te doen een keer voor de map. Zo kunt u dezelfde identiteit gebruiken voor al uw apps logica, hoewel u unieke identiteiten per logica app ook maken mogelijk als u wilt. U kunt dit doen in de gebruikersinterface van of PowerShell gebruiken.

#### <a name="create-the-application-identity-using-the-azure-classic-portal"></a>De identiteit van de met de Azure klassieke portal maken

1. Ga naar [Active directory in de klassieke Azure portal](https://manage.windowsazure.com/#Workspaces/ActiveDirectoryExtension/directory) en selecteer de map die u voor uw Web App
2. Klik op het tabblad **toepassingen**
3. Klik op **toevoegen** in de werkbalk onder aan de pagina
4. Uw identiteit geeft u een naam wilt gebruiken, klikt u op de pijl volgende
5. Plaats in een unieke tekenreeks die is opgemaakt als een domein in de twee tekstvakken en klik op het selectievakje
6. Klik op het tabblad **configureren** voor deze toepassing
7. Kopieer de **Client-ID**, wordt dit gebruikt in uw app logica
8. In de sectie **sleutels** op **duur selecteren** en kies 1 jaar of 2 jaar
9. Klik op de knop **Opslaan** onder in het scherm (u wellicht een paar seconden wachten)
10. Nu moet u de sleutel in het vak te kopiëren. Dit wordt ook gebruikt in uw app logica

#### <a name="create-the-application-identity-using-powershell"></a>De toepassings-id met PowerShell maken
1. `Switch-AzureMode AzureResourceManager`
2. `Add-AzureAccount`
3. `New-AzureADApplication -DisplayName "MyLogicAppID" -HomePage "http://someranddomain.tld" -IdentifierUris "http://someranddomain.tld" -Password "Pass@word1!"`
4. De **ID van de huurder**, de **Toepassings-ID** en het wachtwoord dat u gebruikt kopiëren

### <a name="part-2-protect-your-web-app-with-an-aad-app-identity"></a>Deel 2: Uw Web App met een app AAD identiteit beschermen

Als uw Web app is al geïmplementeerd kunt u alleen inschakelen in de portal. Anders kunt u inschakelen vergunning deel uitmaakt van uw implementatie Azure Resource manager.

#### <a name="enable-authorization-in-the-azure-portal"></a>Vergunning in Azure Portal inschakelen

1. Navigeer naar het Web app en de **Instellingen** in de opdrachtbalk op.
2. Klik op **Verificatie/verificatie**.
3. **Inschakelen.**

Op dit punt een toepassing automatisch voor u gemaakt. Moet u de Client-ID van de toepassing voor deel 3, dus moet je:

1. Ga naar [Active directory in de klassieke Azure portal](https://manage.windowsazure.com/#Workspaces/ActiveDirectoryExtension/directory) en selecteer de map.
2. Voor de toepassing in het zoekvak zoeken
3. Klik op het in de lijst
4. Klik op het tabblad **configureren**
5. Ziet u de **Client-ID**

#### <a name="deploying-your-web-app-using-an-arm-template"></a>Implementatie van uw Web App met behulp van een sjabloon ARM

Eerst moet u een toepassing voor uw Web app maken. Dit moet verschillen van de toepassing die wordt gebruikt voor uw app logica. Start na de bovenstaande stappen in deel 1, maar nu voor het gebruik van de **HomePage** en **IdentifierUris** de werkelijke https://**URL** van uw Web app.

>[AZURE.NOTE]Wanneer u de toepassing voor uw Web app maakt, moet u de [Azure klassieke portal aanpak](https://manage.windowsazure.com/#Workspaces/ActiveDirectoryExtension/directory), zoals de PowerShell-commandlet niet van de vereiste machtigingen instellen aan gebruikers inloggen op een website gebruiken.

Eenmaal hebt u de client-ID en ID van de huurder, als een sub-bron van het Web app toevoegen aan een sjabloon voor uw implementatie:

```
"resources" : [
    {
        "apiVersion" : "2015-08-01",
        "name" : "web",
        "type" : "config",
        "dependsOn" : [
            "[concat('Microsoft.Web/sites/','parameters('webAppName'))]"
        ],
        "properties" : {
            "siteAuthEnabled": true,
            "siteAuthSettings": {
              "clientId": "<<clientID>>",
              "issuer": "https://sts.windows.net/<<tenantID>>/",
            }
        }
    }
]
```

Als u wilt uitvoeren op een installatie automatisch een lege Web app en logica app samen met AAD implementeert, klikt u op de volgende knop:

[![Implementeren op Azure](./media/app-service-logic-custom-hosted-api/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-logic-app-custom-api%2Fazuredeploy.json)

Zie voor de volledige sjabloon [logica app aanroepen voor een aangepaste API App-service wordt gehost en beveiligd door AAD](https://github.com/Azure/azure-quickstart-templates/blob/master/201-logic-app-custom-api/azuredeploy.json).


### <a name="part-3-populate-the-authorization-section-in-the-logic-app"></a>Deel 3: De autorisatie-sectie in de app logica vullen

In de sectie **verificatie** van de **HTTP** -actie:`{"tenant":"<<tenantId>>", "audience":"<<clientID from Part 2>>", "clientId":"<<clientID from Part 1>>","secret": "<<Password or Key from Part 1>>","type":"ActiveDirectoryOAuth" }`

| Element | Beschrijving |
|---------|-------------|
| type | Het type verificatie. De waarde is voor verificatie, ActiveDirectoryOAuth, ActiveDirectoryOAuth. |
| huurder | De huurder id gebruikt voor het identificeren van de pachter AD. |
| doelgroep | Vereist. De bron die u verbinding maakt. |
| clientID | De client-id voor de toepassing van Azure AD. |
| geheim | Vereist. Het geheim van de client waarmee het token wordt aangevraagd. |

De bovenstaande template is al dit ingesteld, maar als u de app logica rechtstreeks, moet u de machtiging volledige sectie moet worden opgenomen.

## <a name="securing-your-api-in-code"></a>De API in code beveiligen

### <a name="certificate-auth"></a>Verificatie certificaat

Clientcertificaten kunt u inkomende verzoeken om uw Web app te valideren. Zie [Hoe te configureren TLS wederzijdse verificatie voor Web App](../app-service-web/app-service-web-configure-tls-mutual-auth.md) voor het instellen van uw code.

In de sectie *vergunning* dient u: `{"type": "clientcertificate","password": "test","pfx": "long-pfx-key"}`.

| Element | Beschrijving |
|---------|-------------|
| type | Vereist. Het type verificatie. Voor SSL-clientcertificaten moet de waarde ClientCertificate. |
| PFX | Vereist. Base64-gecodeerde inhoud van het PFX-bestand. |
| wachtwoord | Vereist. Het wachtwoord voor toegang tot het PFX-bestand. |

### <a name="basic-auth"></a>Eenvoudige verificatie

Basisverificatie (bijvoorbeeld gebruikersnaam en wachtwoord) kunt u de inkomende aanvragen valideren. Eenvoudige verificatie is een vast patroon en kunt u dit doen in welke taal die u uw app bouwen in.

In de sectie *vergunning* dient u: `{"type": "basic","username": "test","password": "test"}`.

| Element | Beschrijving |
|---------|-------------|
| type | Vereist. Het type verificatie. Voor basisverificatie moet de waarde Basic. |
| gebruikersnaam | Vereist. De gebruikersnaam te verifiëren. |
| wachtwoord | Vereist. Wachtwoord te verifiëren. |

### <a name="handle-aad-auth-in-code"></a>Greep AAD auth in code

De Azure Active Directory-verificatie die u in staat in de Portal stellen wordt standaard niet toegesneden machtigingen. Bijvoorbeeld, wordt de API voor een specifieke gebruiker of toepassing, maar alleen om een bepaalde huurder niet worden vergrendeld.

Als u wilt dat de API beperken tot alleen de logica app, bijvoorbeeld in de code, kunt u de kop van de JWT te die de beller, weigert aanvragen die niet overeenkomen met uitpakken.

Verder kunnen gaan als u wilt dat deze volledig in uw eigen code implementeren en niet profiteren van de Portal-functie, kunt u dit artikel lezen: [Active Directory gebruiken voor verificatie in Azure App-Service](../app-service-web/web-sites-authentication-authorization.md).

U moet nog steeds Volg de bovenstaande stappen voor het maken van een toepassings-id voor uw app logica en gebruiken die de API aan te roepen.
