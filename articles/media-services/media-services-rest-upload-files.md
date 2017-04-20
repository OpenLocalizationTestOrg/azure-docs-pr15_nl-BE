<properties 
    pageTitle="Bestanden uploaden in Media Services-account met behulp van REST | Microsoft Azure" 
    description="Informatie over hoe u media-inhoud in de Media Services op maken en uploaden van activa." 
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
    ms.date="09/19/2016"
    ms.author="juliako"/>


# <a name="upload-files-into-a-media-services-account-using-rest"></a>Bestanden uploaden in Media Services-account met behulp van REST

 > [AZURE.SELECTOR]
 - [.NET](media-services-dotnet-upload-files.md)
 - [REST](media-services-rest-upload-files.md)
 - [Portal](media-services-portal-upload-files.md)

In de Media-Services kunt u uw digitale bestanden uploaden naar een actief. De entiteit [activa](https://msdn.microsoft.com/library/azure/hh974277.aspx) kan bevatten, video, audio, afbeeldingen, miniaturen, verzamelingen, tekst tracks en ondertiteling bestanden (en de metagegevens over deze bestanden.)  Zodra de bestanden zijn geüpload naar het activum, wordt de inhoud veilig opgeslagen in de cloud voor verdere verwerking en streaming. 

>[AZURE.NOTE]De volgende overwegingen zijn van toepassing bij het kiezen van de naam van een activum:
>
>- De waarde van de eigenschap IAssetFile.Name van Media Services wordt gebruikt bij het maken van URL's voor de stroomsgewijze inhoud (bijvoorbeeld http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters). Om deze reden is procent codering niet toegestaan. De waarde van de eigenschap **Name** geen van de volgende [tekens voor percentage-codering-gereserveerd](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters): !*'();:@&=+$,/?%#[]". Bovendien kunnen alleen er een '.' voor de bestandsextensie.
>
>- De lengte van de naam mag niet langer zijn dan 260 tekens.

De algemene workflow voor het uploaden van activa is onderverdeeld in de volgende secties:

- Een actief maken
- Coderen van een actief (optioneel)
- Upload een bestand naar een blob-opslag

AMS kunt u activa in bulk te uploaden. Zie voor meer informatie [in deze](media-services-rest-upload-files.md#upload_in_bulk) sectie.

##<a name="upload-assets"></a>Uploaden van activa

###<a name="create-an-asset"></a>Een actief maken

>[AZURE.NOTE] Als u werkt met de overige Media Services API, gelden de volgende overwegingen:
>
>Bij de toegang tot diensten in Media Services, moet u specifieke header-velden en waarden instellen in uw HTTP-aanvragen. Zie [Instellingen voor de ontwikkeling van Media Services REST API](media-services-rest-how-to-use.md)voor meer informatie.

>Nadat de verbinding tot stand te https://media.windows.net, ontvangt u een 301 redirect URI van een andere Media Services opgeven. U moet volgende aanroepen naar de nieuwe URI zoals beschreven in [verbinding maken met Media-Services met behulp van REST API](media-services-rest-connect-programmatically.md). 
 
Een actief is een container voor meerdere typen of sets van objecten in de Media-Services, waaronder video, audio, afbeeldingen, miniaturen collecties, nummers voor tekst en bestanden met ondertiteling. In de REST-API vereist voor het maken van een actief POST-aanvraag sturen naar Media Services en het plaatsen van alle informatie over uw activa in het hoofdgedeelte van de aanvraag.

Een van de eigenschappen die u opgeven kunt wanneer de **Opties**voor het maken van een actief is. **Opties** is een waarde die de opties voor wachtwoordcodering dat een actief kan worden gemaakt met een beschrijving. Een geldige waarde is een van de waarden in de lijst hieronder niet een combinatie van waarden. 

- **Geen** = **0**: geen codering wordt gebruikt. Dit is de standaardwaarde. Er rekening mee dat wanneer u deze optie uw inhoud niet in doorvoer of opslag.
    Als u van plan bent om een MP4 met progressieve download deze optie gebruiken. 

- **StorageEncrypted** = **1**: Geef op of u voor de bestanden worden versleuteld met AES 256-bits codering voor uploaden en opslag.

    Als het activum opslag gecodeerd is, moet u beleid voor levering van activa. Zie [configureren activa levering beleid](media-services-rest-configure-asset-delivery-policy.md)voor meer informatie.

- **CommonEncryptionProtected** = **2**: als u van bestanden die zijn beveiligd met een gemeenschappelijke coderingsmethode (zoals PlayReady uploaden) opgeven. 

- **EnvelopeEncryptionProtected** = **4**: als u HLS versleuteld met AES-bestanden uploadt opgeven. Houd er rekening mee dat de bestanden moeten zijn gecodeerd en gecodeerd door Manager transformeren.

>[AZURE.NOTE]Als het activum codering gebruikt wordt, moet u een **ContentKey** maken en koppelen aan uw activa, zoals beschreven in het volgende onderwerp:[het maken van een ContentKey](media-services-rest-create-contentkey.md). Opmerking Nadat u de bestanden naar het activum uploaden, moet u de coderingseigenschappen van de entiteit **AssetFile** bijwerken met de waarden die u hebt gekregen tijdens de codering van de **activa** . Dit doen via de HTTP-aanvraag **samenvoegen** . 


In het volgende voorbeeld ziet u hoe een actief maken.

**HTTP-aanvraag**

    POST https://media.windows.net/api/Assets HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-2233-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: media.windows.net
    
    {"Name":"BigBuckBunny.mp4"}
    

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
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 18 Jan 2015 22:06:40 GMT
    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Assets/@Element",
       "Id":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "State":0,
       "Created":"2015-01-18T22:06:40.6010903Z",
       "LastModified":"2015-01-18T22:06:40.6010903Z",
       "AlternateId":null,
       "Name":"BigBuckBunny.mp4",
       "Options":0,
       "Uri":"https://storagetestaccount001.blob.core.windows.net/asset-9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StorageAccountName":"storagetestaccount001"
    }
    
###<a name="create-an-assetfile"></a>Een AssetFile maken

De entiteit [AssetFile](http://msdn.microsoft.com/library/azure/hh974275.aspx) vertegenwoordigt een video- of audio-bestand dat is opgeslagen in een blob-container. Een elementbestand is altijd gekoppeld aan een actief en een actief mogelijk een of meer activa-bestanden bevatten. De taak van de Encoder van Media Services mislukt als u een bestand actief object niet is gekoppeld aan een digitaal bestand in een blob-container.

Houd er rekening mee dat het exemplaar van de **AssetFile** en de werkelijke mediabestand twee afzonderlijke objecten zijn. De AssetFile instantie bevat metagegevens over het mediabestand terwijl het mediabestand dat de werkelijke media-inhoud bevat.

Nadat u uw digitale media-bestand naar een blob-container uploadt, gebruikt u het **samenvoegen** HTTP-verzoek naar de AssetFile bijwerken met gegevens over uw media-bestand (zoals verderop in het onderwerp). 

**HTTP-aanvraag**

    POST https://media.windows.net/api/Files HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-4ca2-2233-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: media.windows.net
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

Voordat u bestanden uploadt naar een blob-opslag, stelt u de toegang tot rechten voor het schrijven van een actief beleid. Daarvoor een HTTP-aanvraag aan de set AccessPolicies entiteit te boeken. Tijdens het maken van een DurationInMinutes-waarde instellen of ontvangt u een foutbericht van 500 Interne Server in antwoord terug. Zie [AccessPolicy](http://msdn.microsoft.com/library/azure/hh974297.aspx)voor meer informatie op AccessPolicies.

In het volgende voorbeeld ziet u hoe een AccessPolicy maken:
        
**HTTP-aanvraag**

    POST https://media.windows.net/api/AccessPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-2233-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: media.windows.net
    
    {"Name":"NewUploadPolicy", "DurationInMinutes":"440", "Permissions":"2"} 

**HTTP-aanvraag**

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

###<a name="get-the-upload-url"></a>De Upload-URL ophalen

Maak een SAS-Locator voor het ontvangen van de werkelijke upload-URL. Locators worden de begin- en eindpunt voor de verbinding type voor clients die toegang tot bestanden in een actief wilt definiëren. U kunt voor een bepaalde AccessPolicy en activa paar voor de afhandeling van verzoeken van verschillende clients en meerdere Locator-entiteiten maken. Elk van deze Locators de waarde voor de starttijd plus de waarde van de DurationInMinutes van de AccessPolicy gebruiken om te bepalen hoe lang een URL kan worden gebruikt. Voor meer informatie Zie [Locator](http://msdn.microsoft.com/library/azure/hh974308.aspx).


Een SAS-URL heeft de volgende indeling:

    {https://myaccount.blob.core.windows.net}/{asset name}/{video file name}?{SAS signature}

Sommige overwegingen zijn van toepassing:

- Er kan niet meer dan vijf unieke Locators die is gekoppeld aan een bepaald activum in één keer. Voor meer informatie Zie Locator.
- Als u uw bestanden direct uploaden, moet u de waarde voor de starttijd ingesteld op vijf minuten voordat de huidige tijd. Dit is omdat er mogelijk klok scheeftrekken tussen de clientcomputer en de Media Services. Ook de waarde voor de starttijd in de volgende notatie voor datum/tijd moet zijn: jjjj-MM-ddTHH (bijvoorbeeld ' 2014-05-23T17:53:50Z ").   
- Is er mogelijk een 30-40 seconden vertraagd nadat een Locator is gemaakt aan wanneer deze voor gebruik beschikbaar is. Dit probleem geldt voor zowel SAS-URL en oorsprong Locators.

In het volgende voorbeeld ziet u hoe een Locator SAS-URL maken zoals gedefinieerd door de eigenschap Type in het hoofdgedeelte van de aanvraag ("1" voor een SAS-locator) en "2" voor een On-Demand oorsprong locator. De eigenschap **Path** geretourneerd bevat de URL die u gebruiken moet om uw bestand te uploaden.
    
**HTTP-aanvraag**
    
    POST https://media.windows.net/api/Locators HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-4ca2-2233-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: media.windows.net
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
    
    MERGE https://media.windows.net/api/Files('nb%3Acid%3AUUID%3Af13a0137-0a62-9d4c-b3b9-ca944b5142c5') HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-4ca2-2233-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: media.windows.net
    
    {  
       "ContentFileSize":"1186540",
       "Id":"nb:cid:UUID:f13a0137-0a62-9d4c-b3b9-ca944b5142c5",
       "MimeType":"video/mp4",
       "Name":"BigBuckBunny.mp4",
       "ParentAssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1"
    }


**HTTP-antwoord**

Als succesvol, het volgende geretourneerd: HTTP/1.1 204 geen inhoud

### <a name="delete-the-locator-and-accesspolicy"></a>Verwijder de Locator en AccessPolicy 

**HTTP-aanvraag**


    DELETE https://media.windows.net/api/Locators('nb%3Alid%3AUUID%3Aaf57bdd8-6751-4e84-b403-f3c140444b54') HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-2233-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: media.windows.net

    
**HTTP-antwoord**

Als dit lukt, wordt het volgende resultaat:

    HTTP/1.1 204 No Content 
    ...

**HTTP-aanvraag**

    DELETE https://media.windows.net/api/AccessPolicies('nb%3Apid%3AUUID%3Abe0ac48d-af7d-4877-9d60-1805d68bffae') HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-2233-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: media.windows.net

**HTTP-antwoord**

Als dit lukt, wordt het volgende resultaat:

    HTTP/1.1 204 No Content 
    ...

##<a id="upload_in_bulk"></a>Activa in bulk te uploaden

###<a name="create-the-ingestmanifest"></a>De IngestManifest maken

De IngestManifest is een container voor een set van activa, activa-bestanden en statistische informatie die kan worden gebruikt voor het bepalen van de voortgang van grote hoeveelheden ingesting voor de set.


**HTTP-aanvraag**

    POST https:// media.windows.net/API/IngestManifests HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net
    Content-Length: 36
    Expect: 100-continue
    
    { "Name" : "ExampleManifestREST" }

###<a name="create-assets"></a>Activa maken

Voordat u de IngestManifestAsset maakt, moet u de activa die worden uitgevoerd met behulp van grote hoeveelheden ingesting maken. Een actief is een container voor meerdere typen of sets van objecten in de Media-Services, waaronder video, audio, afbeeldingen, miniaturen collecties, nummers voor tekst en bestanden met ondertiteling. In de REST-API vereist voor het maken van een actief een HTTP POST-aanvraag verzenden naar Microsoft Azure Media Services en het plaatsen van alle informatie over uw activa in het hoofdgedeelte van de aanvraag. In dit voorbeeld wordt het activum gemaakt met de optie StorageEncrption(1) is opgenomen in het hoofdgedeelte van de aanvraag.

**HTTP-antwoord**

    POST https://media.windows.net/API/Assets HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net
    Content-Length: 55
    Expect: 100-continue
    
    { "Name" : "ExampleManifestREST_Asset", "Options" : 1 }

###<a name="create-the-ingestmanifestassets"></a>De IngestManifestAssets maken

IngestManifestAssets vertegenwoordigen activa binnen een IngestManifest die worden gebruikt voor bulk ingesting. Het in feite het activum koppelen aan het manifest. Azure Media Services horloges intern voor het uploaden van het bestand op basis van IngestManifestFiles-collectie die is gekoppeld aan de IngestManifestAsset. Als deze bestanden zijn geüpload, wordt het activum is voltooid. U kunt een nieuwe IngestManifestAsset maken met een HTTP POST-aanvraag. In het hoofdgedeelte van de aanvraag, zijn de IngestManifest-Id en de activa-Id de IngestManifestAsset aan elkaar voor bulk ingesting koppelen moet.

**HTTP-antwoord**

    POST https://media.windows.net/API/IngestManifestAssets HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net
    Content-Length: 152
    Expect: 100-continue
    { "ParentIngestManifestId" : "nb:mid:UUID:5c77f186-414f-8b48-8231-17f9264e2048", "Asset" : { "Id" : "nb:cid:UUID:b757929a-5a57-430b-b33e-c05c6cbef02e"}}


###<a name="create-the-ingestmanifestfiles-for-each-asset"></a>De IngestManifestFiles voor elk activum maken

Een IngestManifestFile geeft een werkelijke video- of blob object die worden geüpload als onderdeel van de bulk ingesting voor een activum. Codering gerelateerde eigenschappen zijn niet vereist, tenzij het actief een optie codering wordt gebruikt. In het voorbeeld gebruikt in deze sectie ziet u StorageEncryption maken van een IngestManifestFile die worden gebruikt voor het activum hebt gemaakt.


**HTTP-antwoord**

    POST https://media.windows.net/API/IngestManifestFiles HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net
    Content-Length: 367
    Expect: 100-continue
    
    { "Name" : "REST_Example_File.wmv", "ParentIngestManifestId" : "nb:mid:UUID:5c77f186-414f-8b48-8231-17f9264e2048", "ParentIngestManifestAssetId" : "nb:maid:UUID:beed8531-9a03-9043-b1d8-6a6d1044cdda", "IsEncrypted" : "true", "EncryptionScheme" : "StorageEncryption", "EncryptionVersion" : "1.0", "EncryptionKeyId" : "nb:kid:UUID:32e6efaf-5fba-4538-b115-9d1cefe43510" }
    
###<a name="upload-the-files-to-blob-storage"></a>De bestanden uploaden naar een Blob-opslag

U kunt een clienttoepassing hoge snelheid kan de activa-bestanden uploaden naar de blob storage container Uri door de eigenschap BlobStorageUriForUpload van de IngestManifest opgegeven. Een opvallend hoge snelheid upload service is [Aspera op verzoek voor de toepassing van Azure](http://go.microsoft.com/fwlink/?LinkId=272001).

###<a name="monitor-bulk-ingest-progress"></a>Monitor Bulk consumptie van voortgang

U kunt de voortgang van de bulk ingesting bewerkingen voor een IngestManifest door de polling-eigenschap van de statistieken van de IngestManifest controleren. Deze eigenschap is een complex type, [IngestManifestStatistics](https://msdn.microsoft.com/library/azure/jj853027.aspx). Als u wilt controleren op de eigenschap van statistieken, indienen van een HTTP GET-verzoek doorgeven van de IngestManifest Id.
 

##<a name="create-contentkeys-used-for-encryption"></a>ContentKeys gebruikt voor codering maken

Als het activum codering gebruikt wordt, moet u de ContentKey moet worden gebruikt voor de codering voordat u de bestanden actief maken. Voor opslag van versleuteling, moeten de volgende eigenschappen worden opgenomen in het hoofdgedeelte van de aanvraag.
 
Aanvraag body, eigenschap   | Beschrijving
---|---
ID | De ContentKey-Id die we onszelf genereren met de volgende notatie "nb:kid:UUID:<NEW GUID>".
ContentKeyType | Dit is het belangrijkste type inhoud als een geheel getal voor deze sleutel. Geven we de waarde 1 voor de codering van de opslag.
EncryptedContentKey | We maken nieuwe inhoud waarde van een sleutel die de waarde van een 256-bits (32 bytes). De sleutel wordt gecodeerd met opslag codering x.509-certificaat dat we uit Microsoft Azure Media Services ophalen door het uitvoeren van een HTTP GET-verzoek voor de methoden GetProtectionKey en GetProtectionKeyId.
ProtectionKeyId | Dit is de sleutel-id bescherming voor opslag codering x.509-certificaat dat is gebruikt om onze inhoud sleutel te coderen.
ProtectionKeyType | Dit is het coderingstype voor de beveiliging-sleutel die is gebruikt voor het coderen van de sleutel. Deze waarde is de StorageEncryption(1) in ons voorbeeld.
Checksum |De MD5 berekende controlesom voor de sleutel. Deze wordt berekend door de inhoud-Id met de sleutel te coderen. In de voorbeeldcode wordt gedemonstreerd hoe de controlesom wordt berekend.


**HTTP-antwoord**
    
    POST https://media.windows.net/api/ContentKeys HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net
    Content-Length: 572
    Expect: 100-continue
    
    {"Id" : "nb:kid:UUID:316d14d4-b603-4d90-b8db-0fede8aa48f8", "ContentKeyType" : 1, "EncryptedContentKey" : "Y4NPej7heOFa2vsd8ZEOcjjpu/qOq3RJ6GRfxa8CCwtAM83d6J2mKOeQFUmMyVXUSsBCCOdufmieTKi+hOUtNAbyNM4lY4AXI537b9GaY8oSeje0NGU8+QCOuf7jGdRac5B9uIk7WwD76RAJnqyep6U/OdvQV4RLvvZ9w7nO4bY8RHaUaLxC2u4aIRRaZtLu5rm8GKBPy87OzQVXNgnLM01I8s3Z4wJ3i7jXqkknDy4VkIyLBSQvIvUzxYHeNdMVWDmS+jPN9ScVmolUwGzH1A23td8UWFHOjTjXHLjNm5Yq+7MIOoaxeMlKPYXRFKofRY8Qh5o5tqvycSAJ9KUqfg==", "ProtectionKeyId" : "7D9BB04D9D0A4A24800CADBFEF232689E048F69C", "ProtectionKeyType" : 1, "Checksum" : "TfXtjCIlq1Y=" }

### <a name="link-the-contentkey-to-the-asset"></a>De ContentKey aan het activum koppelen

De ContentKey is gekoppeld aan een of meer activa door een HTTP POST-aanvraag te verzenden. De volgende aanvraag is een voorbeeld in het voorbeeld ContentKey koppelen aan het activum voorbeeld op Id.

**HTTP-antwoord**
    
    POST https://media.windows.net/API/Assets('nb:cid:UUID:b3023475-09b4-4647-9d6d-6fc242822e68')/$links/ContentKeys HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net
    Content-Length: 113
    Expect: 100-continue
    
    { "uri": "https://media.windows.net/api/ContentKeys('nb%3Akid%3AUUID%3A32e6efaf-5fba-4538-b115-9d1cefe43510')"}

**HTTP-antwoord**

    GET https://media.windows.net/API/IngestManifests('nb:mid:UUID:5c77f186-414f-8b48-8231-17f9264e2048') HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net



##<a name="next-step"></a>Volgende stap

Bekijk Media Services paden te leren.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]



 
[How to Get a Media Processor]: media-services-get-media-processor.md
 
