<properties 
    pageTitle="Levering van activa met REST API van Media Services configureren | Microsoft Azure" 
    description="In dit onderwerp wordt uitgelegd hoe verschillende levering activa met REST API van Media Services configureren." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/19/2016"  
    ms.author="juliako"/>

#<a name="configuring-asset-delivery-policies"></a>Activa-levering-beleid configureren

[AZURE.INCLUDE [media-services-selector-asset-delivery-policy](../../includes/media-services-selector-asset-delivery-policy.md)]

Als u van plan bent om dynamisch gecodeerde activa, is een van de stappen in de workflow van Media Services leveren van inhoud levering beleid voor activa configureren. De levering actief beleid Media Services vertelt hoe u het activum wilt worden geleverd: in welke streaming protocol moet uw activa worden dynamisch verpakt (voor bijvoorbeeld MPEG-streepje, HLS, soepele Streaming, of alle), al dan niet u wilt coderen dynamisch uw activa en hoe (envelop of gemeenschappelijke codering).

Dit onderwerp wordt uitgelegd waarom en hoe u kunt maken en configureren van de levering van activa.

>[AZURE.NOTE]Als u dynamische verpakking en dynamische codering gebruikt, moet u ervoor zorgen dat ten minste één schaaleenheid (ook bekend als streaming eenheid). Zie [voor het schalen van een Media-Service](media-services-portal-manage-streaming-endpoints.md)voor meer informatie.
>
>Het activum moet ook een verzameling adaptieve bitsnelheid MP4s of adaptieve bitsnelheid goede Streaming-bestanden bevatten.

U kunt verschillende soorten beleid toepassen aan dezelfde activa. U kan bijvoorbeeld PlayReady codering toepassen op goede Streaming en envelop AES-codering MPEG-streepje en HLS. Alle protocollen die niet zijn gedefinieerd in het beleid voor een levering (bijvoorbeeld u één beleid toevoegen die alleen HLS als het protocol) van streaming worden geblokkeerd. De uitzondering hierop is als u geen activa levering beleid helemaal gedefinieerd. Vervolgens worden alle protocollen kunnen in de open.

Als u bieden een versleutelde activabeheer voor opslag wilt, moet u beleid voor levering van het actief. Voordat uw activa kan worden gestreamd, de server voor mediagegevensstromen verwijdert u de codering voor de opslag en het stroomsgewijs inhoud met het beleid voor levering opgegeven. Bijvoorbeeld, als u wilt ervoor zorgen dat uw activa die zijn gecodeerd met een coderingssleutel voor envelop Advanced Encryption Standard (AES), het beleidstype ingesteld op **DynamicEnvelopeEncryption**. Opslag-codering verwijderen en het actief in de stream, stelt u het type op **NoDynamicEncryption**. Voorbeelden van het configureren van deze beleidstypen volgen.

Afhankelijk van hoe u de levering actief beleid configureert u zou kunnen dynamisch inpakken, dynamisch coderen en stroomsgewijs verzenden van de volgende streaming protocollen: streams goede Streaming, HLS MPEG-streepje en harde schijven.

De volgende lijst bevat de indelingen gebruiken om te streamen vloeiend, HLS, STREEPJES en harde schijven.

Goede Streaming:

{streaming eindpunt naam media services rekening name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

HLS:

{streaming eindpunt naam media services rekening name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

MPEG-STREEPJE

{streaming eindpunt naam media services rekening name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)

HARDE SCHIJVEN

{streaming eindpunt naam media services rekening name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=f4m-f4f)

Zie [een streaming URL maken](media-services-deliver-streaming-content.md)voor instructies over het publiceren van een actief en een streaming URL maken.


##<a name="considerations"></a>Overwegingen met betrekking tot

- U kunt een AssetDeliveryPolicy die is gekoppeld aan een actief, terwijl een locator OnDemand (streaming) voor de activa bestaat niet verwijderen. De aanbeveling is het beleid van het activum te verwijderen voordat u het beleid verwijdert.
- Een streaming locator kan niet worden gemaakt op een opslag gecodeerde actief wanneer er geen levering actief beleid is ingesteld.  Als het activum niet gecodeerd opslag, het systeem, kunt u een locator maken en de activa in de wissen zonder een beleid actief levering streamen.
- Hebt u meerdere actief beleid voor de levering die is gekoppeld aan een enkel actief, maar u kunt slechts één manier voor het verwerken van een bepaalde AssetDeliveryProtocol opgeven.  Wat betekent dat als u probeert te koppelen van twee beleidsregels voor levering die het AssetDeliveryProtocol.SmoothStreaming-protocol die in een fout resulteren omdat het systeem niet zeker dat u wilt toepassen weet wanneer een client een aanvraag voor een soepele Streaming opgeven.
- Als er een actief met een bestaande streaming locator, kan u een nieuw beleid voor het activum koppelen, ontkoppelen van een bestaand beleid uit het actief of bijwerken van een beleid voor levering die aan het activum is gekoppeld.  U moet eerst de streaming-locator verwijderen, aanpassen van het beleid en de streaming-locator opnieuw maken.  U kunt de dezelfde locatorId wanneer u opnieuw de streaming-locator, maar u ervoor dat niet toe leiden dat problemen voor clients zorgen moet aangezien inhoud kan worden opgeslagen in de oorsprong of een stroomafwaartse CDN.

>[AZURE.NOTE] Als u werkt met de overige Media Services API, gelden de volgende overwegingen:
>
>Bij de toegang tot diensten in Media Services, moet u specifieke header-velden en waarden instellen in uw HTTP-aanvragen. Zie [Instellingen voor de ontwikkeling van Media Services REST API](media-services-rest-how-to-use.md)voor meer informatie.

>Nadat de verbinding tot stand te https://media.windows.net, ontvangt u een 301 redirect URI van een andere Media Services opgeven. U moet volgende aanroepen naar de nieuwe URI zoals beschreven in [verbinding maken met Media-Services met behulp van REST API](media-services-rest-connect-programmatically.md).


##<a name="clear-asset-delivery-policy"></a>Beleid voor levering activa wissen

###<a id="create_asset_delivery_policy"></a>Maak een beleid voor activa levering
De volgende HTTP-aanvraag wordt gemaakt van een activum levering beleid waarmee dynamische codering niet van toepassing en ervoor zorgen dat de stroom in een van de volgende protocollen: MPEG-streepje HLS en goede Streaming-protocollen. 

Voor meer informatie over welke waarden u kunt opgeven bij het maken van een AssetDeliveryPolicy, Zie de sectie [bij het definiëren van AssetDeliveryPolicy gebruikt](#types) .   


Aanvraag:
      
    POST https://media.windows.net/api/AssetDeliveryPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amsaccount1&urn%3aSubscriptionId=zbbef702-e769-2233-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423397827&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=Szo6lbJAvL3dyecAeVmyAnzv3mGzfUNClR5shk9Ivbk%3d
    x-ms-version: 2.11
    x-ms-client-request-id: 4651882c-d7ad-4d5e-86ab-f07f47dcb41e
    Host: media.windows.net
    
    {"Name":"Clear Policy",
    "AssetDeliveryProtocol":7,
    "AssetDeliveryPolicyType":2,
    "AssetDeliveryConfiguration":null}

Antwoord:
    
    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 363
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://media.windows.net/api/AssetDeliveryPolicies('nb%3Aadpid%3AUUID%3A92b0f6ba-3c9f-49b6-a5fa-2a8703b04ecd')
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: 4651882c-d7ad-4d5e-86ab-f07f47dcb41e
    request-id: 6aedbf93-4bc2-4586-8845-fd45590136af
    x-ms-request-id: 6aedbf93-4bc2-4586-8845-fd45590136af
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 08 Feb 2015 06:21:27 GMT
    
    {"odata.metadata":"https://media.windows.net/api/$metadata#AssetDeliveryPolicies/@Element",
    "Id":"nb:adpid:UUID:92b0f6ba-3c9f-49b6-a5fa-2a8703b04ecd",
    "Name":"Clear Policy",
    "AssetDeliveryProtocol":7,
    "AssetDeliveryPolicyType":2,
    "AssetDeliveryConfiguration":null,
    "Created":"2015-02-08T06:21:27.6908329Z",
    "LastModified":"2015-02-08T06:21:27.6908329Z"}
    
###<a id="link_asset_with_asset_delivery_policy"></a>Activa koppelen met het beleid voor levering van activa

Het opgegeven activum koppelt de volgende HTTP-aanvraag aan de levering actief beleid.

Aanvraag:

    POST https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3A86933344-9539-4d0c-be7d-f842458693e0')/$links/DeliveryPolicies HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Content-Type: application/json
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amsaccount1&urn%3aSubscriptionId=zbbef702-e769-3344-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423397827&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=Szo6lbJAvL3dyecAeVmyAnzv3mGzfUNClR5shk9Ivbk%3d
    x-ms-version: 2.11
    x-ms-client-request-id: 56d2763f-6e72-419d-ba3c-685f6db97e81
    Host: media.windows.net
    
    {"uri":"https://media.windows.net/api/AssetDeliveryPolicies('nb%3Aadpid%3AUUID%3A92b0f6ba-3c9f-49b6-a5fa-2a8703b04ecd')"}

Antwoord:

    HTTP/1.1 204 No Content


##<a name="dynamicenvelopeencryption-asset-delivery-policy"></a>DynamicEnvelopeEncryption activa levering beleid 

###<a name="create-content-key-of-the-envelopeencryption-type-and-link-it-to-the-asset"></a>Sleutel van het type EnvelopeEncryption maken en deze koppelen aan de activa

Wanneer DynamicEnvelopeEncryption levering beleid op te geven, moet u controleren of het activum koppelen aan een sleutel van het type EnvelopeEncryption. Zie voor meer informatie: [een sleutel te maken](media-services-rest-create-contentkey.md)).


###<a id="get_delivery_url"></a>Levering URL ophalen

De levering URL ophalen voor de opgegeven leveringsmethode van de sleutel in de vorige stap hebt gemaakt. Een client gebruikt de geretourneerde URL voor het aanvragen van een AES-sleutel of een PlayReady licentie om het afspelen van beveiligde inhoud.

Geef de URL om in het hoofdgedeelte van de HTTP-aanvraag. Als de beveiliging van uw inhoud met PlayReady, een URL verwerving van Media Services PlayReady licentie aanvragen met behulp van 1 voor de keyDeliveryType: {'keyDeliveryType': 1}. Als de beveiliging van uw inhoud met de envelop-codering, een belangrijke aanschaf URL aanvragen door te geven, 2 voor keyDeliveryType: {'keyDeliveryType': 2}.

Aanvraag:
    
    POST https://media.windows.net/api/ContentKeys('nb:kid:UUID:dc88f996-2859-4cf7-a279-c52a9d6b2f04')/GetKeyDeliveryUrl HTTP/1.1
    Content-Type: application/json
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amsaccount1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423452029&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=IEXV06e3drSIN5naFRBdhJZCbfEqQbFZsGSIGmawhEo%3d
    x-ms-version: 2.11
    x-ms-client-request-id: 569d4b7c-a446-4edc-b77c-9fb686083dd8
    Host: media.windows.net
    Content-Length: 21
    
    {"keyDeliveryType":2}

Antwoord:
    
    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 198
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: 569d4b7c-a446-4edc-b77c-9fb686083dd8
    request-id: d26f65d2-fe65-4136-8fcf-31545be68377
    x-ms-request-id: d26f65d2-fe65-4136-8fcf-31545be68377
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 08 Feb 2015 21:42:30 GMT
    
    {"odata.metadata":"media.windows.net/api/$metadata#Edm.String","value":"https://amsaccount1.keydelivery.mediaservices.windows.net/?KID=dc88f996-2859-4cf7-a279-c52a9d6b2f04"}


###<a name="create-asset-delivery-policy"></a>Maak een beleid voor activa levering

De volgende HTTP-aanvraag wordt gemaakt van de **AssetDeliveryPolicy** die is geconfigureerd voor het dynamisch voertuigprofiel codering (**DynamicEnvelopeEncryption**) van toepassing op het protocol **HLS** (in dit voorbeeld, andere protocollen wordt geblokkeerd van streaming). 


Voor meer informatie over welke waarden u kunt opgeven bij het maken van een AssetDeliveryPolicy, Zie de sectie [bij het definiëren van AssetDeliveryPolicy gebruikt](#types) .   

Aanvraag:

    POST https://media.windows.net/api/AssetDeliveryPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amsaccount1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423480651&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=T2FG3tIV0e2ETzxQ6RDWxWAsAzuy3ez2ruXPhrBe62Y%3d
    x-ms-version: 2.11
    x-ms-client-request-id: fff319f6-71dd-4f6c-af27-b675c0066fa7
    Host: media.windows.net
    
    {"Name":"AssetDeliveryPolicy","AssetDeliveryProtocol":4,"AssetDeliveryPolicyType":3,"AssetDeliveryConfiguration":"[{\"Key\":2,\"Value\":\"https:\\/\\/amsaccount1.keydelivery.mediaservices.windows.net\\/\"}]"}

    
Antwoord:
    
    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 460
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: media.windows.net/api/AssetDeliveryPolicies('nb%3Aadpid%3AUUID%3Aec9b994e-672c-4a5b-8490-a464eeb7964b')
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: fff319f6-71dd-4f6c-af27-b675c0066fa7
    request-id: c2a1ac0e-9644-474f-b38f-b9541c3a7c5f
    x-ms-request-id: c2a1ac0e-9644-474f-b38f-b9541c3a7c5f
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 09 Feb 2015 05:24:38 GMT
    
    {"odata.metadata":"media.windows.net/api/$metadata#AssetDeliveryPolicies/@Element","Id":"nb:adpid:UUID:ec9b994e-672c-4a5b-8490-a464eeb7964b","Name":"AssetDeliveryPolicy","AssetDeliveryProtocol":4,"AssetDeliveryPolicyType":3,"AssetDeliveryConfiguration":"[{\"Key\":2,\"Value\":\"https:\\/\\/amsaccount1.keydelivery.mediaservices.windows.net\\/\"}]","Created":"2015-02-09T05:24:38.9167436Z","LastModified":"2015-02-09T05:24:38.9167436Z"}


###<a name="link-asset-with-asset-delivery-policy"></a>Activa koppelen met het beleid voor levering van activa

Zie de [koppeling actief met het beleid voor levering van activa](#link_asset_with_asset_delivery_policy)

##<a name="dynamiccommonencryption-asset-delivery-policy"></a>DynamicCommonEncryption activa levering beleid 

###<a name="create-content-key-of-the-commonencryption-type-and-link-it-to-the-asset"></a>Sleutel van het type CommonEncryption maken en deze koppelen aan de activa

Wanneer DynamicCommonEncryption levering beleid op te geven, moet u controleren of het activum koppelen aan een sleutel van het type CommonEncryption. Zie voor meer informatie: [een sleutel te maken](media-services-rest-create-contentkey.md)).


###<a name="get-delivery-url"></a>Levering URL ophalen

De levering URL ophalen voor de methode PlayReady levering van de sleutel in de vorige stap hebt gemaakt. Een client gebruikt de geretourneerde URL voor het aanvragen van een licentie PlayReady om het afspelen van beveiligde inhoud. Zie voor meer informatie de [Levering URL ophalen](#get_delivery_url).

###<a name="create-asset-delivery-policy"></a>Maak een beleid voor activa levering

De volgende HTTP-aanvraag wordt gemaakt van de **AssetDeliveryPolicy** die is geconfigureerd voor de dynamische gemeenschappelijke codering (**DynamicCommonEncryption**) van toepassing op de **Goede Streaming** protocol (in dit voorbeeld, andere protocollen wordt geblokkeerd van streaming). 

Voor meer informatie over welke waarden u kunt opgeven bij het maken van een AssetDeliveryPolicy, Zie de sectie [bij het definiëren van AssetDeliveryPolicy gebruikt](#types) .   


Aanvraag:

    POST https://media.windows.net/api/AssetDeliveryPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amsaccount1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423480651&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=T2FG3tIV0e2ETzxQ6RDWxWAsAzuy3ez2ruXPhrBe62Y%3d
    x-ms-version: 2.11
    x-ms-client-request-id: fff319f6-71dd-4f6c-af27-b675c0066fa7
    Host: media.windows.net
    
    {"Name":"AssetDeliveryPolicy","AssetDeliveryProtocol":1,"AssetDeliveryPolicyType":4,"AssetDeliveryConfiguration":"[{\"Key\":2,\"Value\":\"https:\\/\\/amsaccount1.keydelivery.mediaservices.windows.net\/PlayReady\/"}]"}


Als u beveiligen van uw inhoud met DRM Widevine wilt, de AssetDeliveryConfiguration-waarden voor het gebruik van WidevineLicenseAcquisitionUrl (dit is de waarde van 7) en geef de URL van een licentie bezorgservice. U kunt de volgende AMS partners helpen u licenties Widevine leveren: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/), [castLabs](http://castlabs.com/company/partners/azure/).

Bijvoorbeeld: 
 
    
    {"Name":"AssetDeliveryPolicy","AssetDeliveryProtocol":2,"AssetDeliveryPolicyType":4,"AssetDeliveryConfiguration":"[{\"Key\":7,\"Value\":\"https:\\/\\/example.net\/WidevineLicenseAcquisition\/"}]"}

>[AZURE.NOTE]Bij het coderen met Widevine, zou het alleen mogelijk te leveren met streepje. Zorg ervoor dat streepje (2) in het protocol van de levering activum opgeven.
  
###<a name="link-asset-with-asset-delivery-policy"></a>Activa koppelen met het beleid voor levering van activa

Zie de [koppeling actief met het beleid voor levering van activa](#link_asset_with_asset_delivery_policy)


##<a id="types"></a>Die worden gebruikt bij het definiëren van AssetDeliveryPolicy

###<a name="assetdeliveryprotocol"></a>AssetDeliveryProtocol 

    /// <summary>
    /// Delivery protocol for an asset delivery policy.
    /// </summary>
    [Flags]
    public enum AssetDeliveryProtocol
    {
        /// <summary>
        /// No protocols.
        /// </summary>
        None = 0x0,

        /// <summary>
        /// Smooth streaming protocol.
        /// </summary>
        SmoothStreaming = 0x1,

        /// <summary>
        /// MPEG Dynamic Adaptive Streaming over HTTP (DASH)
        /// </summary>
        Dash = 0x2,

        /// <summary>
        /// Apple HTTP Live Streaming protocol.
        /// </summary>
        HLS = 0x4,

        /// <summary>
        /// Adobe HTTP Dynamic Streaming (HDS)
        /// </summary>
        Hds = 0x8,

        /// <summary>
        /// Include all protocols.
        /// </summary>
        All = 0xFFFF
    }

###<a name="assetdeliverypolicytype"></a>AssetDeliveryPolicyType

    /// <summary>
    /// Policy type for dynamic encryption of assets.
    /// </summary>
    public enum AssetDeliveryPolicyType
    {
        /// <summary>
        /// Delivery Policy Type not set.  An invalid value.
        /// </summary>
        None,

        /// <summary>
        /// The Asset should not be delivered via this AssetDeliveryProtocol. 
        /// </summary>
        Blocked, 

        /// <summary>
        /// Do not apply dynamic encryption to the asset.
        /// </summary>
        /// 
        NoDynamicEncryption,  

        /// <summary>
        /// Apply Dynamic Envelope encryption.
        /// </summary>
        DynamicEnvelopeEncryption,

        /// <summary>
        /// Apply Dynamic Common encryption.
        /// </summary>
        DynamicCommonEncryption
        }

###<a name="contentkeydeliverytype"></a>ContentKeyDeliveryType


    /// <summary>
    /// Delivery method of the content key to the client.
    ///
    </summary>
    public enum ContentKeyDeliveryType
    {
        /// <summary>
        /// None.
        ///
        </summary>
        None = 0,

        /// <summary>
        /// Use PlayReady License acquistion protocol
        ///
        </summary>
        PlayReadyLicense = 1,

        /// <summary>
        /// Use MPEG Baseline HTTP key protocol.
        ///
        </summary>
        BaselineHttp = 2,

        /// <summary>
        /// Use Widevine License acquistion protocol
        ///
        </summary>
        Widevine = 3

    }


###<a name="assetdeliverypolicyconfigurationkey"></a>AssetDeliveryPolicyConfigurationKey

    /// <summary>
    /// Keys used to get specific configuration for an asset delivery policy.
    /// </summary>

    public enum AssetDeliveryPolicyConfigurationKey
    {
        /// <summary>
        /// No policies.
        /// </summary>
        None,

        /// <summary>
        /// Exact Envelope key URL.
        /// </summary>
        EnvelopeKeyAcquisitionUrl,

        /// <summary>
        /// Base key url that will have KID=<Guid> appended for Envelope.
        /// </summary>
        EnvelopeBaseKeyAcquisitionUrl,
        
        /// <summary>
        /// The initialization vector to use for envelope encryption in Base64 format.
        /// </summary>
        EnvelopeEncryptionIVAsBase64,

        /// <summary>
        /// The PlayReady License Acquisition Url to use for common encryption.
        /// </summary>
        PlayReadyLicenseAcquisitionUrl,

        /// <summary>
        /// The PlayReady Custom Attributes to add to the PlayReady Content Header
        /// </summary>
        PlayReadyCustomAttributes,

        /// <summary>
        /// The initialization vector to use for envelope encryption.
        /// </summary>
        EnvelopeEncryptionIV,

        /// <summary>
        /// Widevine DRM acquisition url
        /// </summary>
        WidevineLicenseAcquisitionUrl
    }


##<a name="media-services-learning-paths"></a>Leerfasen Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
