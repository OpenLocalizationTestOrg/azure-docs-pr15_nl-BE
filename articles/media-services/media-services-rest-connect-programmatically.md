<properties 
    pageTitle="Verbinding maken met Media Services-Account met REST API | Microsoft Azure" 
    description="In dit onderwerp wordt beschreven hoe verbinding met Media Services uisng REST API." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="09/26/2016"  
    ms.author="juliako"/>


# <a name="connecting-to-media-services-account-using-media-services-rest-api"></a>Verbinding maken met Media Services-Account met behulp van Media Services REST API

> [AZURE.SELECTOR]
- [.NET](media-services-dotnet-connect-programmatically.md)
- [REST](media-services-rest-connect-programmatically.md)

In dit onderwerp wordt beschreven hoe u een programma verbinding met Microsoft Azure Media Services ophalen tijdens het programmeren met de Media Services REST API.

Twee dingen zijn vereist bij het verkrijgen van toegang tot Microsoft Azure Media Services: een toegangstoken verstrekt door Azure Access Control Services (ACS) en de URI van Media Services zelf. U kunt elk middel dat u bij het maken van deze aanvragen, zolang u de juiste kop waarden en in het toegangstoken goed binnenkomen bij het aanroepen van Media Services wilt gebruiken.

De volgende stappen beschrijven de meest voorkomende workflow bij het gebruik van de REST API van Media Services verbinding maken met Media-Services:

1. Een toegangstoken ophalen 
2. Verbinding maken met de URI van de Media-Services 

    >[AZURE.NOTE] Nadat de verbinding tot stand te https://media.windows.net, ontvangt u een 301 redirect URI van een andere Media Services opgeven. U moet volgende aanroepen naar de nieuwe URI.
Ook krijgt u een HTTP/1.1 200 reactie met de beschrijving van de metagegevens ODATA-API.

3. Boek uw volgende API-aanroepen naar de nieuwe URL. 

    Als na verbinding probeert te maken, hebt u bijvoorbeeld het volgende:

        HTTP/1.1 301 Moved Permanently
        Location: https://wamsbayclus001rest-hs.cloudapp.net/api/

    U moet de volgende API-aanroepen naar https://wamsbayclus001rest-hs.cloudapp.net/api/ boeken.

##<a name="access-control-address"></a>Adres voor mediatoegangsbeheer

Media Services-adres is https://wamsprodglobal001acs.accesscontrol.windows.net, met uitzondering van de regio Noord China waar het is https://wamsprodglobal001acs.accesscontrol.chinacloudapi.cn.

##<a name="getting-an-access-token"></a>Een toegangstoken ophalen

Als u Media Services rechtstreeks via de REST API, een toegangstoken opgehaald uit de ACS en tijdens elke HTTP-aanvraag die u in de service aanbrengt gebruiken. Deze token is vergelijkbaar met andere tokens die door de ACS op basis van access claims die zijn opgegeven in de header van een HTTP-aanvraag en het OAuth-v2-protocol gebruikt. Hoeft u niet alle andere vereisten voor direct verbinding met Media Services.

In het volgende voorbeeld ziet u het HTTP-verzoek-header en de hoofdtekst die wordt gebruikt voor het ophalen van een token.

**Kop**:

    POST https://wamsprodglobal001acs.accesscontrol.windows.net/v2/OAuth2-13 HTTP/1.1
    Content-Type: application/x-www-form-urlencoded
    Host: wamsprodglobal001acs.accesscontrol.windows.net
    Content-Length: 120
    Expect: 100-continue
    Connection: Keep-Alive
    Accept: application/json

    
**Instantie**:

U moet bewijzen de client_id en client_secret-waarden in de hoofdtekst van deze aanvraag; client_id en client_secret komen overeen met de waarden van de accountnaam en AccountKey, respectievelijk. Deze waarden worden aan u geleverd door Media Services bij het instellen van uw account. 

Zorg ervoor dat de AccountKey voor uw account Media Services URL-gecodeerde (Zie [Procent coderen](http://tools.ietf.org/html/rfc3986#section-2.1) wanneer deze worden gebruikt als de waarde client_secret in het token verzoek om toegang.

    grant_type=client_credentials&client_id=ams_account_name&client_secret=URL_encoded_ams_account_key&scope=urn%3aWindowsAzureMediaServices


Bijvoorbeeld: 

    grant_type=client_credentials&client_id=amstestaccount001&client_secret=wUNbKhNj07oqjqU3Ah9R9f4kqTJ9avPpfe6Pk3YZ7ng%3d&scope=urn%3aWindowsAzureMediaServices


In het volgende voorbeeld ziet u het HTTP-antwoord met de access token in het hoofdgedeelte van de response.

    HTTP/1.1 200 OK
    Cache-Control: no-cache, no-store
    Pragma: no-cache
    Content-Type: application/json; charset=utf-8
    Expires: -1
    request-id: a65150f5-2784-4a01-a4b7-33456187ad83
    X-Content-Type-Options: nosniff
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Thu, 15 Jan 2015 08:07:20 GMT
    Content-Length: 670
    
    {  
       "token_type":"http://schemas.xmlsoap.org/ws/2009/11/swt-token-profile-1.0",
       "access_token":"http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f19258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421330840&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=uf69n82KlqZmkJDNxhJkOxpyIpA2HDyeGUTtSnq1vlE%3d",
       "expires_in":"21600",
       "scope":"urn:WindowsAzureMediaServices"
    }
    

>[AZURE.NOTE]
Het wordt aanbevolen om de waarden voor 'access_token' en 'expires_in' naar een externe opslag in cache. De token gegevens kan later worden opgehaald uit de opslag en opnieuw worden gebruikt in uw Media Services REST API-aanroepen. Dit is vooral handig voor scenario's waarin het token kan veilig kan worden gedeeld door meerdere processen of computers.

Zorg ervoor dat de waarde 'expires_in' van het toegangstoken bewaken en bijwerken van de REST API-aanroepen met nieuwe tokens, indien nodig.

###<a name="connecting-to-the-media-services-uri"></a>Verbinding maken met de URI van de Media-Services

De basis-URI voor Media Services is https://media.windows.net/. U moet eerst verbinding maken met deze URI en als u een 301-omleiding in antwoord krijgt, moet u volgende aanroepen naar de nieuwe URI. Gebruik daarnaast niet alle logica voor automatische omleiding, volgen in je verzoeken. HTTP-woorden en organen van de aanvraag wordt niet doorgestuurd naar de nieuwe URI.

Let erop dat de hoofdmap URI voor het uploaden en downloaden van bestanden actief https://yourstorageaccount.blob.core.windows.net/ waarbij de naam van de opslag is dezelfde die u tijdens de installatie van uw Media Services-account gebruikt.

Het volgende voorbeeld wordt de HTTP-aanvraag naar de hoofdmap van de Media Services URI (https://media.windows.net/). De aanvraag krijgt een 301-redirect terug in antwoord. De volgende aanvraag maakt gebruik van de nieuwe URI (https://wamsbayclus001rest-hs.cloudapp.net/api/).     

**HTTP-aanvraag**:
    
    GET https://media.windows.net/ HTTP/1.1
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f19258-6753-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421500579&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=ElVWXOnMVggFQl%2ft9vhdcv1qH1n%2fE8l3hRef4zPmrzg%3d
    x-ms-version: 2.11
    Accept: application/json
    Host: media.windows.net


**HTTP-antwoord**:
    
    HTTP/1.1 301 Moved Permanently
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/
    Server: Microsoft-IIS/8.5
    request-id: 987d7652-497a-44e5-b815-f492e02aef97
    x-ms-request-id: 987d7652-497a-44e5-b815-f492e02aef97
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sat, 17 Jan 2015 07:44:53 GMT
    Content-Length: 164
    
    <html><head><title>Object moved</title></head><body>
    <h2>Object moved to <a href="https://wamsbayclus001rest-hs.cloudapp.net/api/">here</a>.</h2>
    </body></html>


**HTTP-aanvraag** (met behulp van de nieuwe URI):
            
    GET https://wamsbayclus001rest-hs.cloudapp.net/api/ HTTP/1.1
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f19258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421500579&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=ElVWXOnMVggFQl%2ft9vhdcv1qH1n%2fE8l3hRef4zPmrzg%3d
    x-ms-version: 2.11
    Accept: application/json
    Host: wamsbayclus001rest-hs.cloudapp.net


**HTTP-antwoord**:
    
    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 1250
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    request-id: 5f52ea9d-177e-48fe-9709-24953d19f84a
    x-ms-request-id: 5f52ea9d-177e-48fe-9709-24953d19f84a
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sat, 17 Jan 2015 07:44:52 GMT
    
    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata","value":[{"name":"AccessPolicies","url":"AccessPolicies"},{"name":"Locators","url":"Locators"},{"name":"ContentKeys","url":"ContentKeys"},{"name":"ContentKeyAuthorizationPolicyOptions","url":"ContentKeyAuthorizationPolicyOptions"},{"name":"ContentKeyAuthorizationPolicies","url":"ContentKeyAuthorizationPolicies"},{"name":"Files","url":"Files"},{"name":"Assets","url":"Assets"},{"name":"AssetDeliveryPolicies","url":"AssetDeliveryPolicies"},{"name":"IngestManifestFiles","url":"IngestManifestFiles"},{"name":"IngestManifestAssets","url":"IngestManifestAssets"},{"name":"IngestManifests","url":"IngestManifests"},{"name":"StorageAccounts","url":"StorageAccounts"},{"name":"Tasks","url":"Tasks"},{"name":"NotificationEndPoints","url":"NotificationEndPoints"},{"name":"Jobs","url":"Jobs"},{"name":"TaskTemplates","url":"TaskTemplates"},{"name":"JobTemplates","url":"JobTemplates"},{"name":"MediaProcessors","url":"MediaProcessors"},{"name":"EncodingReservedUnitTypes","url":"EncodingReservedUnitTypes"},{"name":"Operations","url":"Operations"},{"name":"StreamingEndpoints","url":"StreamingEndpoints"},{"name":"Channels","url":"Channels"},{"name":"Programs","url":"Programs"}]}
     


>[AZURE.NOTE] Als u eenmaal de nieuwe URI, die de URI die moet worden gebruikt om te communiceren met de Media Services is. 


##<a name="media-services-learning-paths"></a>Leerfasen Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
