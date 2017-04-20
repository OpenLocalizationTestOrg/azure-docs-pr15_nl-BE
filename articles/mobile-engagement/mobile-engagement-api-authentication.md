<properties 
    pageTitle="Verificatie met mobiel betrokkenheid bij de REST API 's"
    description="Beschrijving van verificatie met Azure Mobile Engagement REST API 's" 
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo"
    manager="erikre"
    editor=""/>

<tags
    ms.service="mobile-engagement"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="mobile-multiple"
    ms.workload="mobile" 
    ms.date="10/05/2016"
    ms.author="wesmc;ricksal"/>

# <a name="authenticate-with-mobile-engagement-rest-apis"></a>Verificatie met mobiel betrokkenheid bij de REST API 's

## <a name="overview"></a>Overzicht

Dit document wordt beschreven hoe u een geldige AAD Oauth token te verifiëren met de Mobile Engagement REST API's. 

Er wordt verondersteld dat u een geldig abonnement Azure en hebt u een betrokkenheid bij de mobiele app met een van onze [Ontwikkelaars zelfstudies](mobile-engagement-windows-store-dotnet-get-started.md)hebt gemaakt.

## <a name="authentication"></a>Verificatie

OAuth-token wordt gebruikt voor verificatie op basis van een Microsoft Azure Active Directory. 

Op verificatieverzoek een API moet, een verificatieheader worden toegevoegd aan elke aanvraag van de volgende vorm:

    Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGmJlNmV2ZWJPamg2TTNXR1E...

>[AZURE.NOTE] Azure Active Directory tokens vervalt binnen 1 uur.

Er zijn verschillende manieren waarop u een token. Aangezien de API's over het algemeen een cloud-service aangeroepen zijn, wilt u een API-sleutel gebruiken. Een API-sleutel in Azure-terminologie heet een Service principal wachtwoord. De volgende procedure beschrijft hoe kunt u deze handmatig instellen.

### <a name="one-time-setup-using-script"></a>Eenmalige setup (met script)

Volg de onderstaande instructies voor het uitvoeren van de installatie met een PowerShell script die de minimale tijd voor de installatie duurt maar gebruikt de meest toelaatbare standaardinstellingen set. U kunt desgewenst ook de instructies in de [handmatige installatie](mobile-engagement-api-authentication-manual.md) rechtstreeks vanaf de portal Azure doen en fijner instellingen. 

1. De nieuwste versie van Azure PowerShell krijgen van [hier](http://aka.ms/webpi-azps). Voor meer informatie over de instructies voor het downloaden, kunt u deze [koppeling](../powershell-install-configure.md)zien.  

2. Nadat Azure PowerShell is geïnstalleerd, moet u de volgende opdrachten gebruiken om ervoor te zorgen dat u beschikt over de **Azure module** geïnstalleerd:

    een. Zorg ervoor dat de module Azure PowerShell is beschikbaar in de lijst met beschikbare modules. 
    
        Get-Module –ListAvailable 

    ![Beschikbare Modules voor Azure][1]
        
    b. Als u de module Azure PowerShell niet in de bovenstaande lijst vinden moet u het volgende:
        
        Import-Module Azure 
        
3. Aanmelding bij de Azure Resource Manager vanuit PowerShell door de volgende opdracht wordt uitgevoerd en uw gebruikersnaam en wachtwoord voor uw account Azure bieden: 
        
        Login-AzureRmAccount

4. Als er meerdere abonnementen en vervolgens moet u het volgende:

    een. Een overzicht van alle abonnementen en kopieer de SubscriptionId van het abonnement dat u wilt gebruiken. Zorg ervoor dat dit abonnement is hetzelfde met de Mobile Engagement App die u communiceren wilt met behulp van de API's. 

        Get-AzureRmSubscription

    b. Voer de volgende opdracht geven de SubscriptionId voor het configureren van het abonnement moet worden gebruikt.

        Select-AzureRmSubscription –SubscriptionId <subscriptionId>

5. De tekst voor het [Nieuwe AzureRmServicePrincipalOwner.ps1](https://raw.githubusercontent.com/matt-gibbs/azbits/master/src/New-AzureRmServicePrincipalOwner.ps1) -script kopiëren naar uw lokale computer en deze opslaan als een PowerShell-cmdlet (bv. `APIAuth.ps1`) en het uitvoeren van `.\APIAuth.ps1`. 
    
6. Het script wordt u gevraagd om input voor **principalName**. Hier een geschikte naam opgeven die u gebruiken wilt voor het maken van uw Active Directory-toepassing (bv. APIAuth). 

7. Nadat het script is voltooid, verschijnt de volgende vier waarden die u moet verifiëren via programmering met AD dus zorg ervoor dat deze kopiëren. 
        
    **TenantId**, **SubscriptionId**, **ApplicationId**en **geheim**.

    U gebruikt TenantId als `{TENANT_ID}`, ApplicationId als `{CLIENT_ID}` en als `{CLIENT_SECRET}`.

    > [AZURE.NOTE] Het standaardbeveiligingsbeleid mogelijk geblokkeerd PowerShell-scripts worden uitgevoerd. In dat geval tijdelijk u uw beleid kan worden uitgevoerd als u wilt dat de uitvoering van het script met de volgende opdracht:

        > Set-ExecutionPolicy RemoteSigned

8. Hier ziet u hoe de set cmdlets PS eruit zou zien. 

    ![][3]

9. Controleer in de portal Management Azure dat een nieuwe AD-toepassing is gemaakt met de naam die u aan het script **principalName** onder **toepassingen weergeven mijn bedrijf eigenaar is van**de naam.

    ![][4]

#### <a name="steps-to-get-a-valid-token"></a>Stappen om een geldige token ophalen

1. Roept de API met de volgende parameters en vervang de HUURDER\_-ID, CLIENT\_en de CLIENT-ID\_GEHEIM:

    - **Verzoek-URL** als *https://login.microsoftonline.com/ {HUURDER\_ID} token oauth2/*
    - **Header voor HTTP-Content-Type** *application/x-1-800-www-Dell-form-urlencoded*
    - **HTTP-aanvragen Body** *verlenen\_type = client\_referenties & client_id = {CLIENT\_ID} & client_secret = {CLIENT\_GEHEIM} & resource=https%3A%2F%2Fmanagement.core.windows.net%2F*

    Hier volgt een voorbeeld van de aanvraag:

        POST /{TENANT_ID}/oauth2/token HTTP/1.1
        Host: login.microsoftonline.com
        Content-Type: application/x-www-form-urlencoded
        grant_type=client_credentials&client_id={CLIENT_ID}&client_secret={CLIENT_SECRET}&reso
        urce=https%3A%2F%2Fmanagement.core.windows.net%2F

    Hier volgt een voorbeeld van de reactie:

        HTTP/1.1 200 OK
        Content-Type: application/json; charset=utf-8
        Content-Length: 1234
    
        {"token_type":"Bearer","expires_in":"3599","expires_on":"1445395811","not_before":"144
        5391911","resource":"https://management.core.windows.net/","access_token":{ACCESS_TOKEN}}

    In dit voorbeeld opgenomen URL-codering van de parameters na `resource` waarde daadwerkelijk is `https://management.core.windows.net/`. Zorg ervoor dat u ook de URL coderen `{CLIENT_SECRET}` als deze speciale tekens kan bevatten.

    > [AZURE.NOTE] Voor het testen, kunt u een hulpprogramma HTTP-client zoals [Fiddler](http://www.telerik.com/fiddler) of [Postman Chrome-extensie](https://chrome.google.com/webstore/detail/postman/fhbjgbiflinjbdggehcddcbncdddomop) 

2. Nu in alle API-aanroep de koprij vergunning aanvragen:

        Authorization: Bearer {ACCESS_TOKEN}

    Controleer de hoofdtekst van het antwoord als een 401 geretourneerde statuscode, het kan erop wijzen dat het token is verlopen. In dat geval krijgt u een nieuwe beveiligingssleutel.

##<a name="using-the-apis"></a>Met behulp van de API 's

Nu u hebt een geldige token, bent u gereed om de API-aanroepen.

1. Bij elke aanvraag API moet u een geldige, niet-verlopen token die u hebt verkregen in de vorige sectie doorgeven.

2. U moet aansluiten in sommige parameters in de aanvraag-URI die de toepassing aangeeft. De aanvraag-URI lijkt op de volgende

        https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/
        providers/Microsoft.MobileEngagement/appcollections/{app-collection}/apps/{app-resource-name}/

    Parameters ophalen, klikt u op de naam van de toepassing en klikt u op het Dashboard en u ziet een pagina als volgt met de 3 parameters.

    - **1**`{subscription-id}`
    - **2**`{app-collection}`
    - **3**`{app-resource-name}`
    - **4** naam van de bronnengroep zal worden **MobileEngagement** , tenzij u een nieuwe gemaakt. 

    ![Mobile Engagement API URI-parameters][2]

>[AZURE.NOTE] <br/>
>1. Het adres van de basis-API negeren als dit voor de vorige API's was.<br/>
>2. Als u de app met klassieke Azure portal gemaakt moet u de naam van de Resource van toepassing dat anders dan de naam van de toepassing is gebruiken. Als u de toepassing hebt gemaakt in de Portal Azure moet vervolgens u de naam van de App zelf (Er is geen onderscheid tussen naam toepassingsbron en de naam van de App voor toepassingen die zijn gemaakt in de nieuwe portal).  

<!-- Images -->
[1]: ./media/mobile-engagement-api-authentication/azure-module.png
[2]: ./media/mobile-engagement-api-authentication/mobile-engagement-api-uri-params.png
[3]: ./media/mobile-engagement-api-authentication/ps-cmdlets.png
[4]: ./media/mobile-engagement-api-authentication/ad-app-creation.png



