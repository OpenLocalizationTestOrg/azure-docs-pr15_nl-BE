<properties 
    pageTitle="Aan de slag met het leveren van inhoud op aanvraag met REST | Microsoft Azure" 
    description="Deze zelfstudie doorloopt u de stappen voor het implementeren van een op vraag content delivery toepassing met Azure Media Services met REST API." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/11/2016" 
    ms.author="juliako"/>

#<a name="get-started-with-delivering-content-on-demand-using-rest"></a>Aan de slag met het leveren van inhoud op aanvraag met REST 

[AZURE.INCLUDE [media-services-selector-get-started](../../includes/media-services-selector-get-started.md)]


>[AZURE.NOTE]
> Als u deze zelfstudie hebt voltooid, moet u een account Azure. Zie voor meer informatie, [Gratis proefperiode van Azure](/pricing/free-trial/?WT.mc_id=A261C142F). 


Deze quickstart doorloopt u de stappen voor het implementeren van een Video-on-Demand (VoD) content delivery-toepassing met Azure Media Services (AMS) REST API's. 

De zelfstudie introduceert de basiswerkstroom Media Services en de meest voorkomende programming objecten en taken die nodig zijn voor de ontwikkeling van Media Services. Aan het einde van de zelfstudie, u kunt streamen of progressief downloaden van een voorbeeld van een mediabestand dat geüpload, gecodeerd en gedownload worden.  

## <a name="prerequisites"></a>Vereisten
De volgende voorwaarden met Media Services met REST API's voor de ontwikkeling.

- Wat over het ontwikkelen met REST API van Media Services. Zie [media-services-rest-overzicht](http://msdn.microsoft.com/library/azure/hh973616.aspx)voor meer informatie.
- Een toepassing van uw keuze die HTTP-aanvragen en antwoorden kunt verzenden. In deze zelfstudie wordt [Fiddler](http://www.telerik.com/download/fiddler). 

De volgende taken worden weergegeven in deze quickstart.

1.  Maak een Media-Services-account (via de portal Azure).
2.  Streaming eindpunt (met behulp van de portal Azure) configureren.
1.  Verbinding maken met de Media Services rekening met REST API.
1.  Nieuwe activa maken en uploaden van een videobestand met REST API.
1.  Streaming eenheden configureren met REST API.
2.  Het bronbestand te coderen in een verzameling adaptieve bitsnelheid MP4-bestanden met REST API.
1.  Publiceren van de activa en get streaming en progressief downloaden van URL's met REST API. 
1.  De inhoud af te spelen. 

## <a name="create-an-azure-media-services-account-using-the-azure-portal"></a>Een Azure Media Services account maken met de Azure portal

De stappen in deze sectie wordt aangegeven hoe een AMS-account te maken.

1. Aanmelden op de [portal Azure](https://portal.azure.com/).
2. Klik op **+ nieuwe** > **Media + CDN** > **mediaservices**.

    ![Mediaservices maken](./media/media-services-portal-vod-get-started/media-services-new1.png)

3. Voer de vereiste waarden in **MEDIA SERVICES-ACCOUNT maken** .

    ![Mediaservices maken](./media/media-services-portal-vod-get-started/media-services-new3.png)
    
    1. Voer in het vak **Accountnaam**de naam van de nieuwe AMS-account. De naam van een Media Services alle kleine letters, cijfers of letters zonder spaties en 3 tot en met 24 tekens lang is.
    2. Selecteer in abonnement, tussen de verschillende Azure abonnementen waartoe u toegang hebt.
    
    2. Selecteer de nieuwe of bestaande resource in **Resourcegroep**.  Een resourcegroep is een verzameling bronnen die delen van de levenscyclus, machtigingen en beleid. Meer informatie [hier](azure-resource-manager/resource-group-overview.md#resource-groups).
    3. Selecteer de regio wordt op **locatie**, voor het opslaan van de records media en metagegevens voor uw Media Services-account gebruikt. Deze regio wordt gebruikt voor het verwerken en media streamen. De beschikbare Media Services gebieden worden weergegeven in het vak van de vervolgkeuzelijst. 
    
    3. Selecteer een opslag voor blobopslag van de media-inhoud van uw Media Services-account in **Rekening voor opslag**. Kunt u een bestaande account voor opslag in hetzelfde geografische gebied, als uw Media Services-account, of kunt u een account voor opslag. Een nieuwe account voor de opslag wordt gemaakt in dezelfde regio. De regels voor namen van opslag zijn hetzelfde als voor de accounts van Media Services.

        Meer informatie over opslag [hier](storage-introduction.md).

    4. Selecteer **vastmaken aan het dashboard** voor een overzicht van de voortgang van de implementatie rekening.
    
7. Klik op **maken** onder aan het formulier.

    Zodra de account is gemaakt, verandert de status **actief**. 

    ![Instellingen voor Media Services](./media/media-services-portal-vod-get-started/media-services-settings.png)

    Voor het beheren van uw account AMS (bijvoorbeeld video's te uploaden, activa te coderen, bewaken van de voortgang van het project) het venster **Instellingen** .

## <a name="configure-streaming-endpoints-using-the-azure-portal"></a>Streaming eindpunten met de Azure portal configureren

Als u werkt met Azure een van de meest voorkomende scenario's voor de clients levert video via bitsnelheid van adaptieve streaming Media-Services. Media-Services ondersteunt de volgende geavanceerde bitsnelheid streaming technologieën: HTTP Live Streaming (HLS) past, soepele Streaming MPEG-streepje en harde schijven (voor Adobe WIP/Access licentiehouders).

Media Services biedt dynamische verpakking, kunt u ervoor zorgen dat uw adaptieve bitsnelheid inhoud MP4 gecodeerd in indelingen die worden ondersteund door Media Services (MPEG-streepje, HLS, soepele Streaming, harde schijven) just-in-time, zonder dat u hoeft op te slaan van voorverpakte versies van elk van deze indelingen streamen streaming.

Als u wilt profiteren van dynamische verpakking, moet u het volgende doen:

- Het bestand met mezzanine (bron) in een reeks geavanceerde bitsnelheid MP4-bestanden (de codering stappen worden verderop in deze zelfstudie aangetoond) te coderen.  
- Maak ten minste één streaming eenheid voor het *eindpunt streaming* waaruit u leveren uw inhoud wilt. De volgende stappen uit hoe het aantal streaming eenheden wijzigen.

Met dynamische verpakking, hoeft u alleen te slaan en te betalen voor de bestanden in de opslagindeling in één en Media Services maakt en het juiste antwoord op basis van aanvragen van een client fungeert.

Maken en wijzigen van het aantal gereserveerde eenheden streaming, doet u het volgende:


1. Klik in het venster **Instellingen voor** **Streaming eindpunten**. 

2. Klik op het eindpunt streaming standaard. 

    Het venster **Std. STREAMING EINDPUNTDETAILS** weergegeven.

3. Als u het aantal eenheden streaming, schuift u de schuifregelaar **Streaming eenheden** .

    ![Streaming-eenheden](./media/media-services-portal-vod-get-started/media-services-streaming-units.png)

4. Klik op de knop **Opslaan** als uw wijzigingen wilt opslaan.

    >[AZURE.NOTE]De toewijzing van alle nieuwe eenheden kan maximaal 20 minuten in beslag nemen.

## <a id="connect"></a>Verbinding maken met de Media Services rekening met REST API

Twee dingen zijn vereist bij het benaderen van Azure Media Services: een toegangstoken verstrekt door Azure Access Control Services (ACS) en de URI van Media Services zelf. U kunt elk middel dat u bij het maken van deze aanvragen, zolang u de juiste kop waarden en in het toegangstoken goed binnenkomen bij het aanroepen van Media Services wilt gebruiken.

De volgende stappen beschrijven de meest voorkomende workflow bij het gebruik van de REST API van Media Services verbinding maken met Media-Services:

1. Een toegangstoken opgehaald. 
2. Verbinding maken met de URI van de Media-Services.  

    Houd er rekening mee dat nadat de verbinding tot stand te https://media.windows.net, ontvangt u een 301 redirect URI van een andere Media Services opgeven. U moet volgende aanroepen naar de nieuwe URI. Ook krijgt u een HTTP/1.1 200 reactie met de beschrijving van de metagegevens ODATA-API.
3. De volgende API-aanroepen naar de nieuwe URL wordt geboekt. 
    
    Als na verbinding probeert te maken, hebt u bijvoorbeeld het volgende:
        
        HTTP/1.1 301 Moved Permanently
        Location: https://wamsbayclus001rest-hs.cloudapp.net/api/

    U moet de volgende API-aanroepen naar https://wamsbayclus001rest-hs.cloudapp.net/api/ boeken.

###<a name="getting-an-access-token"></a>Een toegangstoken ophalen

Als u Media Services rechtstreeks via de REST API, een toegangstoken opgehaald uit de ACS en tijdens elke HTTP-aanvraag die u in de service aanbrengt gebruiken. Hoeft u niet alle andere vereisten voor direct verbinding met Media Services.

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

U moet aangetoond de waarden client_id en client_secret in de hoofdtekst van deze aanvraag; client_id en client_secret komen overeen met de waarden van de accountnaam en AccountKey, respectievelijk. Deze waarden worden aan u geleverd door Media Services bij het instellen van uw account. 

De AccountKey voor uw account Media Services URL-gecodeerd moet zijn wanneer deze worden gebruikt als de waarde client_secret in het token verzoek om toegang.

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
       "access_token":"http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421330840&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=uf69n82KlqZmkJDNxhJkOxpyIpA2HDyeGUTtSnq1vlE%3d",
       "expires_in":"21600",
       "scope":"urn:WindowsAzureMediaServices"
    }
    

>[AZURE.NOTE]
Het wordt aanbevolen om de waarden voor 'access_token' en 'expires_in' naar een externe opslag in cache. De token gegevens kan later worden opgehaald uit de opslag en opnieuw worden gebruikt in uw Media Services REST API-aanroepen. Dit is vooral handig voor scenario's waarin het token kan veilig kan worden gedeeld door meerdere processen of computers.

Zorg ervoor dat de waarde 'expires_in' van het toegangstoken bewaken en bijwerken van de REST API-aanroepen met nieuwe tokens, indien nodig.

###<a name="connecting-to-the-media-services-uri"></a>Verbinding maken met de URI van de Media-Services

De basis-URI voor Media Services is https://media.windows.net/. U moet eerst verbinding maken met deze URI en als u een 301-omleiding in antwoord krijgt, moet u volgende aanroepen naar de nieuwe URI. Gebruik daarnaast niet alle logica voor automatische omleiding, volgen in je verzoeken. HTTP-woorden en organen van de aanvraag wordt niet doorgestuurd naar de nieuwe URI.

De URI voor het uploaden en downloaden van bestanden van de activa van de hoofdmap is https://yourstorageaccount.blob.core.windows.net/ waarbij de naam van de opslag dezelfde die u tijdens de installatie van uw Media Services-account gebruikt.

Het volgende voorbeeld wordt de HTTP-aanvraag naar de hoofdmap van de Media Services URI (https://media.windows.net/). De aanvraag krijgt een 301-redirect terug in antwoord. De volgende aanvraag maakt gebruik van de nieuwe URI (https://wamsbayclus001rest-hs.cloudapp.net/api/).     

**HTTP-aanvraag**:
    
    GET https://media.windows.net/ HTTP/1.1
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421500579&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=ElVWXOnMVggFQl%2ft9vhdcv1qH1n%2fE8l3hRef4zPmrzg%3d
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
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421500579&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=ElVWXOnMVggFQl%2ft9vhdcv1qH1n%2fE8l3hRef4zPmrzg%3d
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
     


>[AZURE.NOTE] Van nu af aan wordt de nieuwe URI gebruikt in deze zelfstudie.

## <a id="upload"></a>Nieuwe activa maken en uploaden van een videobestand met REST API

In de Media-Services kunt u uw digitale bestanden uploaden naar een actief. De entiteit **activa** kan bevatten, video, audio, afbeeldingen, miniaturen, verzamelingen, tekst tracks en ondertiteling bestanden (en de metagegevens over deze bestanden.)  Zodra de bestanden zijn geüpload naar het activum, wordt de inhoud veilig opgeslagen in de cloud voor verdere verwerking en streaming. 

Een van de waarden die u opgeven moet bij het maken van een actief is de opties voor het activum maken. De eigenschap **Options** is een waarde die de opties voor wachtwoordcodering dat een actief kan worden gemaakt met een beschrijving. Een geldige waarde is een van de waarden in de lijst, niet een combinatie van waarden uit de lijst hieronder:

 
- **Geen** = **0** - geen codering wordt gebruikt. Wanneer u deze optie wordt de inhoud niet beveiligd in doorvoer of opslag.
    Als u van plan bent om een MP4 met progressieve download deze optie gebruiken. 
- **StorageEncrypted** = **1** - duidelijke inhoud lokaal met AES 256-bits codering gecodeerd en vervolgens geüpload naar Azure opslag waar het is opgeslagen in rust zijn gecodeerd. Activa met opslag codering beveiligd worden ongecodeerd automatisch geplaatst in een gecodeerde-bestandssysteem voor de codering en eventueel opnieuw gecodeerd voordat u het uploadt terug als een nieuwe uitvoer actief. Het primaire gebruik geval voor opslag codering is wanneer u de hoge kwaliteit invoermedium bestanden beveiligen met sterke codering in rust op schijf.
- **CommonEncryptionProtected** = **2** - Gebruik deze optie als u inhoud die al zijn gecodeerd en beveiligd met een gemeenschappelijke codering of PlayReady DRM (bijvoorbeeld goede Streaming beveiligd met DRM PlayReady) wilt uploaden.
- **EnvelopeEncryptionProtected** = **4** : Gebruik deze optie als u HLS versleuteld met AES uploaden wilt. Zijn moeten de bestanden gecodeerd en gecodeerd door Manager transformeren.

### <a name="create-an-asset"></a>Een actief maken

Een actief is een container voor meerdere typen of sets van objecten in de Media-Services, waaronder video, audio, afbeeldingen, miniaturen collecties, nummers voor tekst en bestanden met ondertiteling. In de REST-API vereist voor het maken van een actief POST-aanvraag sturen naar Media Services en het plaatsen van alle informatie over uw activa in het hoofdgedeelte van de aanvraag.

In het volgende voorbeeld ziet u hoe een actief maken.

**HTTP-aanvraag**

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/Assets HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    x-ms-client-request-id: c59de965-bc89-4295-9a57-75d897e5221e
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 45
    
    {"Name":"BigBuckBunny.mp4", "Options":"0"}
    

**HTTP-antwoord**

Als dit lukt, wordt het volgende resultaat:
    
    HTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 452
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/Assets('nb%3Acid%3AUUID%3A9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1')
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: c59de965-bc89-4295-9a57-75d897e5221e
    request-id: e98be122-ae09-473a-8072-0ccd234a0657
    x-ms-request-id: e98be122-ae09-473a-8072-0ccd234a0657
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 18 Jan 2015 22:06:40 GMT
        
    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Assets/@Element",
       "Id":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "State":0,
       "Created":"2015-01-18T22:06:40.6010903Z",
       "LastModified":"2015-01-18T22:06:40.6010903Z",
       "AlternateId":null,
       "Name":"BigBuckBunny2.mp4",
       "Options":0,
       "Uri":"https://storagetestaccount001.blob.core.windows.net/asset-9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StorageAccountName":"storagetestaccount001"
    }
    
### <a name="create-an-assetfile"></a>Een AssetFile maken

De entiteit [AssetFile](http://msdn.microsoft.com/library/azure/hh974275.aspx) vertegenwoordigt een video- of audio-bestand dat is opgeslagen in een blob-container. Een elementbestand is altijd gekoppeld aan een actief en een actief kan een of meer AssetFiles bevatten. De taak van de Encoder van Media Services mislukt als u een bestand actief object niet is gekoppeld aan een digitaal bestand in een blob-container.

Nadat u uw digitale media-bestand naar een blob-container uploadt, met het **samenvoegen** HTTP-verzoek kunt u de AssetFile bijwerken met gegevens over uw media-bestand (zoals verderop in het onderwerp). 

**HTTP-aanvraag**

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/Files HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 164
    
    {  
       "IsEncrypted":"false",
       "IsPrimary":"false",
       "MimeType":"video/mp4",
       "Name":"BigBuckBunny.mp4",
       "ParentAssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1"
    }


**HTTP-antwoord**

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 535
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/Files('nb%3Acid%3AUUID%3Af13a0137-0a62-9d4c-b3b9-ca944b5142c5')
    Server: Microsoft-IIS/8.5
    request-id: 98a30e2d-f379-4495-988e-0b79edc9b80e
    x-ms-request-id: 98a30e2d-f379-4495-988e-0b79edc9b80e
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 00:34:07 GMT
    
    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Files/@Element",
       "Id":"nb:cid:UUID:f13a0137-0a62-9d4c-b3b9-ca944b5142c5",
       "Name":"BigBuckBunny.mp4",
       "ContentFileSize":"0",
       "ParentAssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "EncryptionVersion":null,
       "EncryptionScheme":null,
       "IsEncrypted":false,
       "EncryptionKeyId":null,
       "InitializationVector":null,
       "IsPrimary":false,
       "LastModified":"2015-01-19T00:34:08.1934137Z",
       "Created":"2015-01-19T00:34:08.1934137Z",
       "MimeType":"video/mp4",
       "ContentChecksum":null
    }


### <a name="creating-the-accesspolicy-with-write-permission"></a>De AccessPolicy wordt gemaakt met de machtiging schrijven. 

Voordat u bestanden uploadt naar een blob-opslag, stelt u de toegang tot rechten voor het schrijven van een actief beleid. Daarvoor een HTTP-aanvraag aan de set AccessPolicies entiteit te boeken. Tijdens het maken van een DurationInMinutes-waarde instellen of foutbericht 500 Interne Server fout in reactie terug. Zie [AccessPolicy](http://msdn.microsoft.com/library/azure/hh974297.aspx)voor meer informatie op AccessPolicies.

In het volgende voorbeeld ziet u hoe een AccessPolicy maken:
        
**HTTP-aanvraag**

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/AccessPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 74
    
    {"Name":"NewUploadPolicy", "DurationInMinutes":"440", "Permissions":"2"} 

**HTTP-antwoord**

    If successful, the following response is returned:
    
    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 312
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/AccessPolicies('nb%3Apid%3AUUID%3Abe0ac48d-af7d-4877-9d60-1805d68bffae')
    Server: Microsoft-IIS/8.5
    request-id: 74c74545-7e0a-4cd6-a440-c1c48074a970
    x-ms-request-id: 74c74545-7e0a-4cd6-a440-c1c48074a970
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 18 Jan 2015 22:18:06 GMT

    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#AccessPolicies/@Element",
       "Id":"nb:pid:UUID:be0ac48d-af7d-4877-9d60-1805d68bffae",
       "Created":"2015-01-18T22:18:06.6370575Z",
       "LastModified":"2015-01-18T22:18:06.6370575Z",
       "Name":"NewUploadPolicy",
       "DurationInMinutes":440.0,
       "Permissions":2
    }

### <a name="get-the-upload-url"></a>De Upload-URL ophalen

Maak een SAS-Locator voor het ontvangen van de werkelijke upload-URL. Locators worden de begin- en eindpunt voor de verbinding type voor clients die toegang tot bestanden in een actief wilt definiëren. U kunt voor een bepaalde AccessPolicy en activa paar voor de afhandeling van verzoeken van verschillende clients en meerdere Locator-entiteiten maken. Elk van deze Locators de waarde voor de starttijd plus de waarde van de DurationInMinutes van de AccessPolicy gebruikt om te bepalen hoe lang die een URL kan worden gebruikt. Voor meer informatie Zie [Locator](http://msdn.microsoft.com/library/azure/hh974308.aspx).


Een SAS-URL heeft de volgende indeling:

    {https://myaccount.blob.core.windows.net}/{asset name}/{video file name}?{SAS signature}

Sommige overwegingen zijn van toepassing:

- Er kan niet meer dan vijf unieke Locators die is gekoppeld aan een bepaald activum in één keer. Voor meer informatie Zie Locator.
- Als u uw bestanden direct uploaden, moet u de waarde voor de starttijd ingesteld op vijf minuten voordat de huidige tijd. Dit is omdat er mogelijk klok scheeftrekken tussen de clientcomputer en de Media Services. Ook de waarde voor de starttijd in de volgende notatie voor datum/tijd moet zijn: jjjj-MM-ddTHH (bijvoorbeeld ' 2014-05-23T17:53:50Z ").   
- Is er mogelijk een 30-40 seconden vertraagd nadat een Locator is gemaakt aan wanneer deze voor gebruik beschikbaar is. Dit probleem geldt voor zowel SAS-URL en oorsprong Locators.

In het volgende voorbeeld ziet u hoe een Locator SAS-URL maken zoals gedefinieerd door de eigenschap Type in het hoofdgedeelte van de aanvraag ("1" voor een SAS-locator) en "2" voor een On-Demand oorsprong locator. De eigenschap **Path** geretourneerd bevat de URL die u gebruiken moet om uw bestand te uploaden.
    
**HTTP-aanvraag**
    
    POST https://wamsbayclus001rest-hs.cloudapp.net/api/Locators HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=f7f09258-6753-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 178
    
    {  
       "AccessPolicyId":"nb:pid:UUID:be0ac48d-af7d-4877-9d60-1805d68bffae",
       "AssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StartTime":"2015-02-18T16:45:53",
       "Type":1
    }


**HTTP-antwoord**

Als dit lukt, wordt het volgende antwoord geretourneerd:
        
    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 949
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/Locators('nb%3Alid%3AUUID%3Aaf57bdd8-6751-4e84-b403-f3c140444b54')
    Server: Microsoft-IIS/8.5
    request-id: 2adeb1f8-89c5-4cc8-aa4f-08cdfef33ae0
    x-ms-request-id: 2adeb1f8-89c5-4cc8-aa4f-08cdfef33ae0
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 03:01:29 GMT
    
    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Locators/@Element",
       "Id":"nb:lid:UUID:af57bdd8-6751-4e84-b403-f3c140444b54",
       "ExpirationDateTime":"2015-02-19T00:05:53",
       "Type":1,
       "Path":"https://storagetestaccount001.blob.core.windows.net/asset-f438649c-313c-46e2-8d68-7d2550288247?sv=2012-02-12&sr=c&si=af57bdd8-6751-4e84-b403-f3c140444b54&sig=fE4btwEfZtVQFeC0Wh3Kwks2OFPQfzl5qTMW5YytiuY%3D&st=2015-02-18T16%3A45%3A53Z&se=2015-02-19T00%3A05%3A53Z",
       "BaseUri":"https://storagetestaccount001.blob.core.windows.net/asset-f438649c-313c-46e2-8d68-7d2550288247",
       "ContentAccessComponent":"?sv=2012-02-12&sr=c&si=af57bdd8-6751-4e84-b403-f3c140444b54&sig=fE4btwEfZtVQFeC0Wh3Kwks2OFPQfzl5qTMW5YytiuY%3D&st=2015-02-18T16%3A45%3A53Z&se=2015-02-19T00%3A05%3A53Z",
       "AccessPolicyId":"nb:pid:UUID:be0ac48d-af7d-4877-9d60-1805d68bffae",
       "AssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StartTime":"2015-02-18T16:45:53",
       "Name":null
    }

### <a name="upload-a-file-into-a-blob-storage-container"></a>Uploaden van een bestand in een blob-opslag container
    
Als u de AccessPolicy en de Locator ingesteld hebt, wordt het bestand geüpload naar een Azure Blob-opslag container met Azure opslag REST API's. In de pagina uploaden of BLOB's blokkeren. 

>[AZURE.NOTE] U moet de naam van het bestand dat u wilt uploaden naar de waarde van de Locator **pad** ontvangen in de vorige sectie toevoegen. Bijvoorbeeld: https://storagetestaccount001.blob.core.windows.net/asset-e7b02da4-5a69-40e7-a8db-e8f4f697aac0/BigBuckBunny.mp4? . . . 

Zie voor meer informatie over het werken met opslag Azure BLOB's [Blob Service REST API](http://msdn.microsoft.com/library/azure/dd135733.aspx).


### <a name="update-the-assetfile"></a>De AssetFile bijwerken 

Nu dat u uw bestand hebt geüpload, moet u de FileAsset grootte (en andere) gegevens bijwerken. Bijvoorbeeld:
    
    MERGE https://wamsbayclus001rest-hs.cloudapp.net/api/Files('nb%3Acid%3AUUID%3Af13a0137-0a62-9d4c-b3b9-ca944b5142c5') HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    
    {  
       "ContentFileSize":"1186540",
       "Id":"nb:cid:UUID:f13a0137-0a62-9d4c-b3b9-ca944b5142c5",
       "MimeType":"video/mp4",
       "Name":"BigBuckBunny.mp4",
       "ParentAssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1"
    }


**HTTP-antwoord**

Als succesvol, het volgende geretourneerd: HTTP/1.1 204 geen inhoud

## <a name="delete-the-locator-and-accesspolicy"></a>Verwijder de Locator en AccessPolicy 

**HTTP-aanvraag**


    DELETE https://wamsbayclus001rest-hs.cloudapp.net/api/Locators('nb%3Alid%3AUUID%3Aaf57bdd8-6751-4e84-b403-f3c140444b54') HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net

    
**HTTP-antwoord**

Als dit lukt, wordt het volgende resultaat:

    HTTP/1.1 204 No Content 
    ...

**HTTP-aanvraag**

    DELETE https://wamsbayclus001rest-hs.cloudapp.net/api/AccessPolicies('nb%3Apid%3AUUID%3Abe0ac48d-af7d-4877-9d60-1805d68bffae') HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net

**HTTP-antwoord**

Als dit lukt, wordt het volgende resultaat:

    HTTP/1.1 204 No Content 
    ...

 
## <a id="configure_streaming_units"></a>Streaming eenheden configureren met REST API

Als u werkt met Azure een van de meest voorkomende scenario's voor de clients levert bitsnelheid van adaptieve streaming Media-Services. De client kunt overschakelen naar een hogere of lagere bitsnelheid met adaptieve bitsnelheid streaming, zoals de video wordt weergegeven op basis van de huidige bandbreedte, CPU-gebruik en andere factoren. Media-Services ondersteunt de volgende geavanceerde bitsnelheid streaming technologieën: HTTP Live Streaming (HLS) past, soepele Streaming MPEG-streepje en harde schijven (voor Adobe WIP/Access licentiehouders). 

Media Services biedt dynamische verpakking, zodat u de inhoud adaptieve bitsnelheid MP4 of goede Streaming gecodeerd in indelingen die worden ondersteund door Media Services (MPEG-streepje, HLS, soepele Streaming, harde schijven) streaming bezorgen zonder dat u hoeft om te verpakken in deze indelingen streamen. 

Als u wilt profiteren van dynamische verpakking, moet u het volgende doen:

- ten minste één streaming eenheid krijgen voor het **eindpunt streaming **van waaruit u van plan bent om uw inhoud (zoals beschreven in deze sectie).
- coderen of transcoderen de mezzanine (bron) bestanden in een verzameling adaptieve bitsnelheid MP4-bestanden of adaptieve bitsnelheid goede Streaming-bestanden (de codering stappen worden verderop in deze zelfstudie aangetoond)  

Met dynamische verpakking, hoeft u alleen te slaan en te betalen voor de bestanden in de opslagindeling in één en Media Services maakt en het juiste antwoord op basis van aanvragen van een client fungeert. 


>[AZURE.NOTE] Zie voor meer informatie over de prijsdetails [Media Services prijzen Details](http://go.microsoft.com/fwlink/?LinkId=275107).

Als u wilt wijzigen van het aantal gereserveerde eenheden streaming, het volgende doen:
    
### <a name="get-the-streaming-endpoint-you-want-to-update"></a>Krijgen de streaming eindpunt dat u wilt bijwerken

We gaan nu haalt bijvoorbeeld het eerste eindpunt streaming in uw account (kan maximaal twee eindpunten van streaming in staat tegelijkertijd actief hebben.)

**HTTP-aanvraag**:

    GET https://wamsbayclus001rest-hs.cloudapp.net/api/StreamingEndpoints()?$top=1 HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json;odata=verbose
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421466122&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=TiKGEOTporft4pFGU24sSZRZk5GRAWszFXldl5NXAhY%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net

**HTTP-antwoord**
    
Als dit lukt, wordt het volgende resultaat:
    
    HTTP/1.1 200 OK
    . . . 
    
### <a name="scale-the-streaming-endpoint"></a>Het eindpunt van de streaming-schaal
 
**HTTP-aanvraag**:

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/StreamingEndpoints('nb:oid:UUID:cd57670d-cc1c-0f86-16d8-3ad478bf9486')/Scale HTTP/1.1
    Content-Type: application/json;odata=verbose
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json;odata=verbose
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421466122&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=TiKGEOTporft4pFGU24sSZRZk5GRAWszFXldl5NXAhY%3d
    x-ms-version: 2.11
    x-ms-client-request-id: 39f96c93-a4b1-43ce-b97e-b2aaa44ee2dd
    Host: wamsbayclus001rest-hs.cloudapp.net
    
    {"scaleUnits":1}

**HTTP-antwoord**

    HTTP/1.1 202 Accepted
    Cache-Control: no-cache
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: 39f96c93-a4b1-43ce-b97e-b2aaa44ee2dd
    request-id: 3c1ba1c7-281c-4b2d-a898-09cb70a3a424
    x-ms-request-id: 3c1ba1c7-281c-4b2d-a898-09cb70a3a424
    operation-id: nb:opid:UUID:1853bcbf-b71f-4ed5-a4c7-a581d4f45ae7
    X-Content-Type-Options: nosniff
    DataServiceVersion: 1.0;
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Fri, 16 Jan 2015 22:16:43 GMT
    Content-Length: 0

    
### <a id="long_running_op_status"></a>Controleer de status van een langdurige bewerking

De toewijzing van alle nieuwe eenheden duurt ongeveer 20 minuten. Controleer de status van de bewerking gebruikt de methode **bewerkingen** en de Id van de bewerking opgeven. De bewerking Id die is geretourneerd in het antwoord op het verzoek van de **schaal** .

    operation-id: nb:opid:UUID:1853bcbf-b71f-4ed5-a4c7-a581d4f45ae7
 
**HTTP-aanvraag**:
    
    GET https://wamsbayclus001rest-hs.cloudapp.net/api/Operations('nb:opid:UUID:1853bcbf-b71f-4ed5-a4c7-a581d4f45ae7') HTTP/1.1
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json;odata=verbose
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421466122&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=TiKGEOTporft4pFGU24sSZRZk5GRAWszFXldl5NXAhY%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    
**HTTP-antwoord**
    
    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 515
    Content-Type: application/json;odata=verbose;charset=utf-8
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: 829e1a89-3ec2-4836-a04d-802b5aeff5e8
    request-id: f7ae8a78-af8d-4881-b9ea-ca072cfe0b60
    x-ms-request-id: f7ae8a78-af8d-4881-b9ea-ca072cfe0b60
    X-Content-Type-Options: nosniff
    DataServiceVersion: 1.0;
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Fri, 16 Jan 2015 22:57:39 GMT
    
    {  
       "d":{  
          "__metadata":{  
             "id":"https://wamsbayclus001rest-hs.cloudapp.net/api/Operations('nb%3Aopid%3AUUID%3Acc339c28-6bba-4f7d-bee5-91ea4a0a907e')",
             "uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/Operations('nb%3Aopid%3AUUID%3Acc339c28-6bba-4f7d-bee5-91ea4a0a907e')",
             "type":"Microsoft.Cloud.Media.Vod.Rest.Data.Models.Operation"
          },
          "Id":"nb:opid:UUID:cc339c28-6bba-4f7d-bee5-91ea4a0a907e",
          "State":"Succeeded",
          "TargetEntityId":"nb:oid:UUID:cd57670d-cc1c-0f86-16d8-3ad478bf9486",
          "ErrorCode":null,
          "ErrorMessage":null
       }
    }


## <a id="encode"></a>Het bronbestand in een verzameling adaptieve bitsnelheid MP4-bestanden coderen

Na ingesting die activa in Media Services, media kunnen worden gecodeerd, transmuxed, een watermerk, enzovoort, voordat deze wordt afgeleverd aan clients. Deze activiteiten worden gepland en uitgevoerd voor meerdere exemplaren van achtergrond rol te garanderen optimale prestaties en beschikbaarheid. Deze activiteiten worden taken genoemd en bestaat elke [taak](http://msdn.microsoft.com/library/azure/hh974289.aspx) van atomaire taken waarmee de werkelijke hoeveelheid werk voor het bestand actief. 

Zoals is gezegd, bij het werken met Azure Media Services is een van de meest voorkomende scenario's adaptieve streaming de bitsnelheid levert aan de clients. Media-Services dynamisch adaptieve bitsnelheid MP4-bestanden kunt inpakken in een van de volgende indelingen: HTTP Live Streaming (HLS) past, soepele Streaming MPEG-streepje en harde schijven (voor Adobe WIP/Access licentiehouders). 

Als u wilt profiteren van dynamische verpakking, moet u het volgende doen:

- coderen of transcoderen de mezzanine (bron) bestanden in een verzameling adaptieve bitsnelheid MP4-bestanden of adaptieve bitsnelheid goede Streaming-bestanden  
- ten minste één streaming eenheid voor het streaming eindpunt waaruit u wilt ervoor zorgen dat uw inhoud ophalen. 

In het volgende gedeelte ziet u hoe een taak met taak met één codering wilt maken. De taak bevat transcoderen het mezzanine-bestand in een verzameling adaptieve bitsnelheid MP4s **Media Encoder standaard**gebruiken. Ook ziet de sectie u hoe de voortgang van loonveredeling volgen. Wanneer de taak voltooid is, kunt u zou kunnen maken locators die nodig zijn om toegang te krijgen tot uw activa. 

### <a name="get-a-media-processor"></a>Een Mediaprocessor ophalen

Een Mediaprocessor is in Media Services, een onderdeel dat zorgt voor verwerking van specifieke taken, zoals codering, conversie van bestandsindeling, coderen of decoderen media-inhoud. Voor de codering taak weergegeven in deze zelfstudie, gaan we gebruiken de standaard Media-Encoder.

De volgende code aanvraagt van de encoder-id. 

**HTTP-aanvraag**

    GET https://wamsbayclus001rest-hs.cloudapp.net/api/MediaProcessors()?$filter=Name%20eq%20'Media%20Encoder%20Standard' HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=f7f09258-6753-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421675491&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=9hUudHYnATpi5hN3cvTfgw%2bL4N3tL0fdsRnQnm6ZYIU%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    

**HTTP-antwoord**

    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 273
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    request-id: 6beb04b4-55a7-480d-8aa8-e5c5d59ffa1f
    x-ms-request-id: 6beb04b4-55a7-480d-8aa8-e5c5d59ffa1f
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 07:54:09 GMT
    
    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#MediaProcessors",
       "value":[  
          {  
             "Id":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "Description":"Media Encoder Standard",
             "Name":"Media Encoder Standard",
             "Sku":"",
             "Vendor":"Microsoft",
             "Version":"1.1"
          }
       ]
    }

### <a name="create-a-job"></a>Een taak maken

Elke taak kan een of meer taken afhankelijk van het soort behandeling die u wilt bereiken hebben. Via de REST API, kunt u taken en hun verwante taken op twee manieren: taken kunnen worden gedefinieerd in line via de eigenschap Tasks navigatie op taak entiteiten of via de batchverwerking OData. De Media Services SDK maakt gebruik van batch-verwerking. Voor de leesbaarheid van de codevoorbeelden in dit onderwerp zijn taken echter gedefinieerde in line. Zie voor informatie over de batch-verwerking, [Open Data Protocol (OData) Batch-verwerking](http://www.odata.org/documentation/odata-version-3-0/batch-processing/).

In het volgende voorbeeld ziet u hoe u kunt maken en boeken van een taak met een taak die is ingesteld voor het coderen van een video op een specifieke resolutie en kwaliteit. De volgende documentatie sectie, bevat de lijst van alle [voorinstellingen taak](http://msdn.microsoft.com/library/mt269960) ondersteund door de Media Encoder standaard processor.  

**HTTP-aanvraag**
    
    POST https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Content-Type: application/json
    Accept: application/json;odata=verbose
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421675491&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=9hUudHYnATpi5hN3cvTfgw%2bL4N3tL0fdsRnQnm6ZYIU%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 482
    
    {  
       "Name":"NewTestJob",
       "InputMediaAssets":[  
          {  
             "__metadata":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Assets('nb%3Acid%3AUUID%3A9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1')"
             }
          }
       ],
       "Tasks":[  
          {  
             "Configuration":"H264 Adaptive Bitrate MP4 Set 720p",
             "MediaProcessorId":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "TaskBody":"<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset>
                <outputAsset>JobOutputAsset(0)</outputAsset></taskBody>"
          }
       ]
    }

**HTTP-antwoord**

Als dit lukt, wordt het volgende antwoord geretourneerd:

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 1215
    Content-Type: application/json;odata=verbose;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')
    Server: Microsoft-IIS/8.5
    request-id: 532ac1ec-a475-4dce-b2d5-7c8ce94ac87c
    x-ms-request-id: 532ac1ec-a475-4dce-b2d5-7c8ce94ac87c
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 08:04:35 GMT
        
    {  
       "d":{  
          "__metadata":{  
             "id":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')",
             "uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')",
             "type":"Microsoft.Cloud.Media.Vod.Rest.Data.Models.Job"
          },
          "Tasks":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/Tasks"
             }
          },
          "OutputMediaAssets":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/OutputMediaAssets"
             }
          },
          "InputMediaAssets":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1')/InputMediaAssets"
             }
          },
          "Id":"nb:jid:UUID:71d2dd33-efdf-ec43-8ea1-136a110bd42c",
          "Name":"NewTestJob",
          "Created":"2015-01-19T08:04:34.3287057Z",
          "LastModified":"2015-01-19T08:04:34.3287057Z",
          "EndTime":null,
          "Priority":0,
          "RunningDuration":0,
          "StartTime":null,
          "State":0,
          "TemplateId":null,
          "JobNotificationSubscriptions":{  
             "__metadata":{  
                "type":"Collection(Microsoft.Cloud.Media.Vod.Rest.Data.Models.JobNotificationSubscription)"
             },
             "results":[  
    
             ]
          }
       }
    }


Er zijn een paar belangrijke zaken die aan een verzoek van de taak:

- TaskBody eigenschappen moeten letterlijke XML gebruiken om het aantal invoer of uitvoer van activa die worden gebruikt door de taak. Het onderwerp bevat de XML-schemadefinitie voor het XML-bestand.
- In de definitie van TaskBody, elk binnenste waarde voor <inputAsset> en <outputAsset> als JobInputAsset(value) of JobOutputAsset(value) moet worden ingesteld.
- Een taak kan meerdere output activa hebben. Een JobOutputAsset(x) kan slechts eenmaal worden gebruikt als een output van een taak in een project.
- Kunt u JobInputAsset of JobOutputAsset als een input-element van een taak.
- Taken moeten niet een cyclus vormen.
- De waardeparameter die u aan JobInputAsset of JobOutputAsset doorgeeft geeft de indexwaarde voor een activum. De feitelijke elementen worden gedefinieerd in de eigenschappen InputMediaAssets en OutputMediaAssets op de definitie van de entiteit. 

>[AZURE.NOTE] Omdat Media Services is gebaseerd op de OData-v3 en de afzonderlijke elementen in InputMediaAssets en OutputMediaAssets navigatie eigenschappenreeksen worden verwezen via een ' __metadata: uri "naam / waarde-paar. 

- InputMediaAssets wordt toegewezen aan een of meer activa die u hebt gemaakt in de Media Services. OutputMediaAssets worden gemaakt door het systeem. Ze niet verwijzen naar een bestaand activum.
- OutputMediaAssets kan de naam van het kenmerk assetName. Als dit kenmerk niet aanwezig is, is de naam van de OutputMediaAsset ongeacht de interne tekstwaarde van de <outputAsset> -element is met een achtervoegsel van de naam van de waarde of de taak-Id-waarde (in het geval waarin de eigenschap Name niet is gedefinieerd). Bijvoorbeeld, als u een waarde voor assetName voor 'Voorbeeld', zou vervolgens de eigenschap Name van de OutputMediaAsset worden ingesteld op "Voorbeeld". Als u een waarde voor assetName niet ingesteld, maar de taaknaam op 'NewJob' heeft ingesteld, zou vervolgens de naam van de OutputMediaAsset wel "_NewJob JobOutputAsset (waarde)". 

    In het volgende voorbeeld wordt het kenmerk assetName ingesteld:
    
        "<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset assetName=\"CustomOutputAssetName\">JobOutputAsset(0)</outputAsset></taskBody>"


- Opdat de taak koppelen:

    - Een project moet ten minste twee taken hebben
    - Er moet ten minste één taak waarvan de invoer is de uitvoer van een andere taak in het project.

Zie voor meer informatie, [maken van een taak codering met de Media Services REST API](http://msdn.microsoft.com/library/azure/jj129574.aspx).

### <a name="monitor-processing-progress"></a>Monitor voortgang verwerken

U kunt de status van de ophalen met behulp van de eigenschap State zoals in het volgende voorbeeld. 

**HTTP-aanvraag**

    GET https://wamsbayclus001rest-hs.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/State HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=zf84471d-2233-4e75-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1336908022&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=RYXOraO6Z%2f7l9whWZQN%2bypeijgHwIk8XyikA01Kx1%2bk%3d
    Host: wamsbayclus001rest-hs.net
    Content-Length: 0


**HTTP-antwoord**

Als dit lukt, wordt het volgende antwoord geretourneerd:

    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 17
    Content-Type: application/json;odata=verbose;charset=utf-8
    Server: Microsoft-IIS/7.5
    x-ms-request-id: 01324d81-18e2-4493-a3e5-c6186209f0c8
    X-Content-Type-Options: nosniff
    DataServiceVersion: 1.0;
    X-AspNet-Version: 4.0.30319
    X-Powered-By: ASP.NET
    Date: Sun, 13 May 2012 05:16:53 GMT
    
    {"d":{"State":2}}


### <a name="cancel-a-job"></a>Een taak annuleren

Media-Services kunt u actieve taken via de functie CancelJob annuleren. Deze oproep geeft als resultaat een foutcode 400 als u probeert een taak annuleren wanneer de staat wordt geannuleerd, wordt geannuleerd, fout of voltooid.

In het volgende voorbeeld ziet u hoe aanroepen van CancelJob.


**HTTP-aanvraag**


    GET https://wamsbayclus001rest-hs.net/API/CancelJob?jobid='nb%3ajid%3aUUID%3a71d2dd33-efdf-ec43-8ea1-136a110bd42c' HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.2
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=2f84471d-b1ae-4e75-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1336908753&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=kolAgnFfbQIeRv4lWxKSFa4USyjWXRm15Kd%2bNd5g8eA%3d
    Host: wamsbayclus001rest-hs.net


Als dit lukt, wordt met de hoofdtekst van het bericht geen 204 reactiecode geretourneerd.

>[AZURE.NOTE] U moet coderen URL de taak-id (normaal nb:jid:UUID: somevalue) wanneer deze in als een parameter doorgegeven aan CancelJob.


### <a name="get-the-output-asset"></a>Ophalen van de activa van de uitvoer 

De volgende code ziet u hoe de uitvoer activa id aanvragen 


**HTTP-aanvraag**

    GET https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/OutputMediaAssets() HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421675491&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=9hUudHYnATpi5hN3cvTfgw%2bL4N3tL0fdsRnQnm6ZYIU%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    

**HTTP-antwoord**

    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 445
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    request-id: 73cd605d-066a-46f1-8358-f4bd25a9220a
    x-ms-request-id: 73cd605d-066a-46f1-8358-f4bd25a9220a
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 08:28:13 GMT
        
    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Assets",
       "value":[  
          {  
             "Id":"nb:cid:UUID:71d2dd33-efdf-ec43-8ea1-136a110bd42c",
             "State":0,
             "Created":"2015-01-19T07:52:15.603",
             "LastModified":"2015-01-19T07:52:15.603",
             "AlternateId":null,
             "Name":"Multibitrate MP4s",
             "Options":0,
             "Uri":"https://storagetestaccount001.blob.core.windows.net/asset-71d2dd33-efdf-ec43-8ea1-136a110bd42c",
             "StorageAccountName":"storagetestaccount001"
          }
       ]
    }



## <a id="publish_get_urls"></a>Publiceren van de activa en get streaming en progressief downloaden van URL's met REST API

Als u wilt streamen of downloaden van een actief, moet u eerst 'publiceren' door het maken van een locator. Locators bieden toegang tot bestanden in de activa. Media Services biedt ondersteuning voor twee soorten locators: OnDemandOrigin locators, stream media (bijvoorbeeld MPEG-streepje, HLS of goede Streaming) en Access-handtekening (SAS)-locators gebruikt om mediabestanden te downloaden.

Wanneer u de locators hebt gemaakt, kunt u de URL's die worden gebruikt om te streamen of te downloaden van uw bestanden maken. 


Voor soepele Streaming streaming URL heeft de volgende indeling:

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

Een streaming URL voor HLS heeft de volgende indeling:

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

Voor MPEG-streepje een streaming-URL heeft de volgende indeling:

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)


Een SAS-URL die wordt gebruikt om bestanden te downloaden, heeft de volgende indeling:

    {blob container name}/{asset name}/{file name}/{SAS signature}

Deze paragraaf toont hoe u de volgende taken uitvoeren om uw activa 'publiceren'.  

- De AccessPolicy maken met leesmachtiging 
- Maken van een SAS-URL voor het downloaden van inhoud 
- Maken van een oorspronkelijke URL voor het streamen van inhoud 

###<a name="creating-the-accesspolicy-with-read-permission"></a>De AccessPolicy maken met leesmachtiging

Voordat downloaden of streaming mediainhoud, eerst een AccessPolicy met leesrechten definiëren en maken van de juiste Locator entiteit waarmee het type van het systeem voor de levering die mogelijk te maken voor uw klanten. Zie voor meer informatie over de beschikbare eigenschappen, [AccessPolicy entiteitseigenschappen](https://msdn.microsoft.com/library/azure/hh974297.aspx#accesspolicy_properties).

In het volgende voorbeeld ziet u hoe de AccessPolicy voor lezen-machtiging voor een bepaald activum opgeven.

    POST https://wamsbayclus001rest-hs.net/API/AccessPolicies HTTP/1.1
    Content-Type: application/json
    Accept: application/json
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=2f84471d-b1ae-4e75-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1337067658&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=dithjGvlXR9HlyAf5DE99N5OCYkPAxsHIcsTSjm9%2fVE%3d
    Host: wamsbayclus001rest-hs.net
    Content-Length: 74
    Expect: 100-continue
    
    {"Name": "DownloadPolicy", "DurationInMinutes" : "300", "Permissions" : 1}

Als dit lukt, wordt een code 201 geretourneerd met een beschrijving van de AccessPolicy-entiteit die u hebt gemaakt. Vervolgens gebruikt u de AccessPolicy-Id en uw activa-Id van de activa waarop het bestand moet worden geleverd (bijvoorbeeld een output-actief) maken de Locator-entiteit.

>[AZURE.NOTE]
Deze eenvoudige werkstroom is hetzelfde als het uploaden van een bestand als een actief (zoals eerder in dit onderwerp is beschreven) ingesting. Net als bij uploaden van bestanden, als u (of uw clients) onmiddellijk toegang krijgen tot uw bestanden moeten, Stel ook de waarde voor de starttijd tot vijf minuten voordat de huidige tijd. Deze actie is nodig omdat er mogelijk klok scheeftrekken tussen de client en de Media Services. De waarde voor de starttijd in de volgende notatie voor datum/tijd moet zijn: jjjj-MM-ddTHH (bijvoorbeeld ' 2014-05-23T17:53:50Z ").


###<a name="creating-a-sas-url-for-downloading-content"></a>Maken van een SAS-URL voor het downloaden van inhoud 

De volgende code toont hoe u een URL die kan worden gebruikt voor het downloaden van een media-bestand gemaakt en eerder geüpload. De AccessPolicy heeft de machtigingen lezen en de Locator pad verwijst naar een download-URL SAS.

    POST https://wamsbayclus001rest-hs.net/API/Locators HTTP/1.1
    Content-Type: application/json
    Accept: application/json
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=zf84471d-b1ae-2233-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1337067658&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=dithjGvlXR9HlyAf5DE99N5OCYkPAxsHIcsTSjm9%2fVE%3d
    Host: wamsbayclus001rest-hs.net
    Content-Length: 182
    Expect: 100-continue
    
    {"AccessPolicyId": "nb:pid:UUID:38c71dd0-44c5-4c5f-8418-08bb6fbf7bf8", "AssetId" : "nb:cid:UUID:71d2dd33-efdf-ec43-8ea1-136a110bd42c", "StartTime" : "2014-05-17T16:45:53", "Type":1}

Als dit lukt, wordt het volgende antwoord geretourneerd:

    
    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 1150
    Content-Type: application/json;odata=verbose;charset=utf-8
    Location: https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A8e5a821d-2194-4d00-8884-adf979856874')
    Server: Microsoft-IIS/7.5
    x-ms-request-id: 8cfd8556-3064-416a-97f2-67d9e35f0911
    X-Content-Type-Options: nosniff
    DataServiceVersion: 1.0;
    X-AspNet-Version: 4.0.30319
    X-Powered-By: ASP.NET
    Date: Mon, 14 May 2012 21:41:32 GMT
        
    {  
       "d":{  
          "__metadata":{  
             "id":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A8e5a821d-2194-4d00-8884-adf979856874')",
             "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A8e5a821d-2194-4d00-8884-adf979856874')",
             "type":"Microsoft.Cloud.Media.Vod.Rest.Data.Models.Locator"
          },
          "AccessPolicy":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A8e5a821d-2194-4d00-8884-adf979856874')/AccessPolicy"
             }
          },
          "Asset":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/Asset"
             }
          },
          "Id":"nb:lid:UUID:8e5a821d-2194-4d00-8884-adf979856874",
          "ExpirationDateTime":"\/Date(1337049393000)\/",
          "Type":1,
          "Path":"https://storagetestaccount001.blob.core.windows.net/asset-71d2dd33-efdf-ec43-8ea1-136a110bd42c?st=2012-05-14T21%3A36%3A33Z&se=2012-05-15T02%3A36%3A33Z&sr=c&si=8e5a821d-2194-4d00-8884-adf979856874&sig=y75dViDpC5V8WutrXM%2B%2FGpR3uOtqmlISiNlHU1YUBOg%3D",
          "AccessPolicyId":"nb:pid:UUID:38c71dd0-44c5-4c5f-8418-08bb6fbf7bf8",
          "AssetId":"nb:cid:UUID:71d2dd33-efdf-ec43-8ea1-136a110bd42c",
          "StartTime":"\/Date(1337031393000)\/"
       }
    }


De eigenschap van het geretourneerde **pad** bevat de URL SAS.

>[AZURE.NOTE]
Als u opslag gecodeerde inhoud downloadt, moet u handmatig het decoderen voordat deze of de decodering opslag MediaProcessor gebruiken in een taak voor verwerking van verwerkte bestanden naar een OutputAsset in de uitvoer en het downloaden van dat actief. Zie een taak codering maken met de REST API van Media Services voor meer informatie over de verwerking. Ook kan niet URL-Locators SAS worden bijgewerkt nadat u ze hebt gemaakt. Bijvoorbeeld niet kunt u de Locator dezelfde met een bijgewerkte waarde voor de starttijd opnieuw gebruiken. Dit is de wijze waarop die SAS-URL's worden gemaakt. Als u toegang wilt tot een actief downloaden nadat een Locator is verstreken, maakt u een nieuw bestand met een nieuwe starttijd.

###<a name="download-files"></a>Bestanden downloaden

Als u de AccessPolicy en de Locator ingesteld hebt, kunt u downloaden van bestanden met behulp van de Azure Storage REST API's.  

>[AZURE.NOTE] U moet de naam van het bestand dat u wilt downloaden naar de waarde van de Locator **pad** ontvangen in de vorige sectie toevoegen. Bijvoorbeeld: https://storagetestaccount001.blob.core.windows.net/asset-e7b02da4-5a69-40e7-a8db-e8f4f697aac0/BigBuckBunny.mp4? . . . 

Zie voor meer informatie over het werken met opslag Azure BLOB's [Blob Service REST API](http://msdn.microsoft.com/library/azure/dd135733.aspx).

Als gevolg van de codering taak die u eerder (codering in set adaptieve MP4) hebt uitgevoerd, hebt u meerdere MP4-bestanden die u steeds kunt downloaden. Bijvoorbeeld:    
    
    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z
    
    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z
    
    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_3400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z
    
    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_2250kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z
    
    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1500kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z
    
    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1000kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z
    
    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z
    
    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_56kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z


### <a name="creating-a-streaming-url-for-streaming-content"></a>Maken van een streaming-URL voor het streamen van inhoud


De volgende code ziet u hoe een streaming URL Locator maken:

    POST https://wamsbayclus001rest-hs/API/Locators HTTP/1.1
    Content-Type: application/json
    Accept: application/json
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=2f84471d-b1ae-4e75-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1337067658&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=dithjGvlXR9HlyAf5DE99N5OCYkPAxsHIcsTSjm9%2fVE%3d
    Host: wamsbayclus001rest-hs
    Content-Length: 182
    Expect: 100-continue
    
    {"AccessPolicyId": "nb:pid:UUID:38c71dd0-44c5-4c5f-8418-08bb6fbf7bf8", "AssetId" : "nb:cid:UUID:eb5540a2-116e-4d36-b084-7e9958f7f3c3", "StartTime" : "2014-05-17T16:45:53",, "Type":2}

Als dit lukt, wordt het volgende antwoord geretourneerd:

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 981
    Content-Type: application/json;odata=verbose;charset=utf-8
    Location: https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')
    Server: Microsoft-IIS/7.5
    x-ms-request-id: 2eac4158-fc78-4fa2-81ee-c9f582dc385f
    X-Content-Type-Options: nosniff
    DataServiceVersion: 1.0;
    X-AspNet-Version: 4.0.30319
    X-Powered-By: ASP.NET
    Date: Mon, 14 May 2012 21:41:39 GMT
        
    {  
       "d":{  
          "__metadata":{  
             "id":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')",
             "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')",
             "type":"Microsoft.Cloud.Media.Vod.Rest.Data.Models.Locator"
          },
          "AccessPolicy":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')/AccessPolicy"
             }
          },
          "Asset":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')/Asset"
             }
          },
          "Id":"nb:lid:UUID:52034bf6-dfae-4d83-aad3-3bd87dcb1a5d",
          "ExpirationDateTime":"\/Date(1337049395000)\/",
          "Type":2,
          "Path":"http://wamsbayclus001rest-hs.net/52034bf6-dfae-4d83-aad3-3bd87dcb1a5d/",
          "AccessPolicyId":"nb:pid:UUID:38c71dd0-44c5-4c5f-8418-08bb6fbf7bf8",
          "AssetId":"nb:cid:UUID:eb5540a2-116e-4d36-b084-7e9958f7f3c3",
          "StartTime":"\/Date(1337031395000)\/"
       }
    }

Als u wilt streamen een oorsprong goede Streaming URL in een streaming MediaPlayer, moet u het pad eigenschap met de naam van de goede Streaming-manifestbestand, gevolgd door ' / manifest ' toevoegen.

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest

Toevoegen als u wilt streamen HLS, (format = m3u8 aapl) nadat de '/ manifest'.

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=m3u8-aapl)

Toevoegen als u wilt streamen van MPEG-streepje, (format = mpd-tijd-csf) nadat de '/ manifest'.

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=mpd-time-csf)


## <a id="play"></a>Uw inhoud afspelen  

Als u video streamen, [Azure Services mediaspeler](http://amsplayer.azurewebsites.net/azuremediaplayer.html)te gebruiken.

Progressieve download testen, plak een URL in een browser (bijvoorbeeld Internet Explorer, Chrome, Safari).


##<a name="next-steps-media-services-learning-paths"></a>Volgende stappen: Mediaservices leren werken met paden

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="looking-for-something-else"></a>Op zoek naar iets anders?

Als dit onderwerp niet wat u verwachtte, is er iets ontbreekt, of in een andere wijze niet aan uw behoeften voldoet, geef ons uw feedback met behulp van de onderstaande Disqus-thread.



