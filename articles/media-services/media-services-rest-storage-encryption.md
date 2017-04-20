<properties 
    pageTitle="Coderen van uw inhoud met opslag codering AMS REST API gebruiken" 
    description="Informatie over het coderen van uw inhoud met opslag codering met AMS REST API's." 
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
    ms.date="09/26/2016"
    ms.author="juliako"/>


#<a name="encrypting-your-content-with-storage-encryption-using-ams-rest-api"></a>Coderen van uw inhoud met opslag codering AMS REST API gebruiken

Het wordt nadrukkelijk aanbevolen voor het coderen van inhoud lokaal met AES 256-bits codering en upload deze naar Azure opslag waar deze worden opgeslagen in rust zijn gecodeerd.

Dit artikel geeft een overzicht van AMS opslag codering en hoe u de opslag gecodeerde inhoud te uploaden:

- Maak een sleutel.
- Een actief maken. Stel de AssetCreationOption op StorageEncryption bij het maken van de activa.

     Gecodeerde activa moeten worden gekoppeld aan de inhoud van toetsen.
- De sleutel aan het activum koppelt.  
- De codering instellen gerelateerde parameters in de AssetFile entiteiten.
 
>[AZURE.NOTE]Als u bieden een versleutelde activabeheer voor opslag wilt, moet u beleid voor levering van het actief. Voordat uw activa kan worden gestreamd, de server voor mediagegevensstromen verwijdert u de codering voor de opslag en het stroomsgewijs inhoud met het beleid voor levering opgegeven. Zie [Beleid voor activa levering configureren](media-services-rest-configure-asset-delivery-policy.md)voor meer informatie.


>[AZURE.NOTE] Als u werkt met de overige Media Services API, gelden de volgende overwegingen:
>
>Bij de toegang tot diensten in Media Services, moet u specifieke header-velden en waarden instellen in uw HTTP-aanvragen. Zie [Instellingen voor de ontwikkeling van Media Services REST API](media-services-rest-how-to-use.md)voor meer informatie.

>Nadat de verbinding tot stand te https://media.windows.net, ontvangt u een 301 redirect URI van een andere Media Services opgeven. U moet volgende aanroepen naar de nieuwe URI zoals beschreven in [verbinding maken met Media-Services met behulp van REST API](media-services-rest-connect-programmatically.md). 

##<a name="storage-encryption-overview"></a>Opslag bestandscodering-overzicht 

**AES-CTR** modus codering geldt de AMS opslag versleuteling voor het hele bestand.  AES-CTR-modus is een blokcodering die gegevens zonder de noodzaak voor deze opvulling willekeurige lengte kunt coderen. Het werkt door het coderen van een teller blok met de AES-algoritme en XOR-ing de uitvoer van AES met de gegevens te coderen of te decoderen.  Het item dat wordt gebruikt door de waarde van de InitializationVector kopiëren naar 0 tot en met 7 bytes van de waarde van de teller is opgebouwd en worden 8 tot en met 15 bytes van de waarde van de teller op nul ingesteld. Van het blok van 16 bytes teller, 8 en 15 (dat wil zeggen de minst significante bytes) bytes gebruikt een eenvoudige 64-bits geheel getal zonder voorteken dat wordt verhoogd voor elk volgende blok met 1 verwerkt en wordt gehouden in netwerkbytevolgorde. Houd er rekening mee dat als dit geheel de maximale waarde (0xFFFFFFFFFFFFFFFF) verhogen het opnieuw instellen van de teller nul (bytes 8 tot en met 15) blokkeren bereikt zonder dat de 64 bits van de teller (dat wil zeggen bytes 0-7).   Ter handhaving van de veiligheid van de codering AES CTR-modus, de InitializationVector-waarde voor een bepaalde sleutel-id voor elke sleutel moeten uniek zijn voor elk bestand en bestanden zijn minder dan 2 ^ 64 blokken in lengte.  Dit is om ervoor te zorgen dat de waarde van een teller nooit met een bepaalde sleutel wordt hergebruikt. Zie voor meer informatie over de modus CTR [deze wiki-pagina](https://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#CTR) (het wiki-artikel wordt de term 'Nonce' in plaats van "InitializationVector").

**Opslag-codering** gebruikt voor het coderen van duidelijke inhoud lokaal met AES 256-bits codering en upload deze naar Azure opslag waar het is opgeslagen in rust zijn gecodeerd. Activa met opslag codering beveiligd worden ongecodeerd automatisch geplaatst in een gecodeerde-bestandssysteem voor de codering en eventueel opnieuw gecodeerd voordat u het uploadt terug als een nieuwe uitvoer actief. Het primaire gebruik geval voor opslag codering is als u wilt uw hoge kwaliteit invoermedium bestanden beveiligen met sterke codering in rust op schijf.

Voor het leveren van een gecodeerde activabeheer voor opslag, moet u beleid voor levering van het actief zodat Media Services weet hoe u wilt uw inhoud te leveren. Voordat uw activa kan worden gestreamd, de server voor mediagegevensstromen verwijdert u de codering voor de opslag en het stroomsgewijs inhoud met het beleid voor levering opgegeven (bijvoorbeeld AES, gemeenschappelijke codering of geen codering).

##<a name="create-contentkeys-used-for-encryption"></a>ContentKeys gebruikt voor codering maken

Gecodeerde activa moeten worden gekoppeld aan de sleutel voor bestandscodering voor opslag. U moet de sleutel moet worden gebruikt voor de codering voordat u de bestanden actief maken. In deze sectie wordt beschreven hoe een sleutel te maken.

De volgende zijn algemene stappen voor het genereren van inhoud sleutels die u wilt koppelen aan de activa die u wilt worden gecodeerd. 

1. Voor opslag van codering, willekeurig 32-byte AES sleutel genereren. 

    Dit is de sleutel voor de activa, wat betekent dat alle bestanden die zijn gekoppeld aan dat actief moet dezelfde inhoud sleutel gebruiken tijdens het ontsleutelen. 
2.  Aanroepen van de methoden [GetProtectionKeyId](https://msdn.microsoft.com/library/azure/jj683097.aspx#getprotectionkeyid) en [GetProtectionKey](https://msdn.microsoft.com/library/azure/jj683097.aspx#getprotectionkey) als u het juiste x.509-certificaat dat moet worden gebruikt voor het coderen van uw sleutel.
3.  Codeer uw sleutel met de openbare sleutel van het x.509-certificaat. 

    Media Services .NET SDK gebruikt RSA OAEP bij het uitvoeren van de codering.  Hier ziet u een voorbeeld van .NET in de [functie EncryptSymmetricKeyData](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.FileEncryption/EncryptionUtils.cs).
4.  Maak een controlesomwaarde die wordt berekend met behulp van de sleutel-id en sleutel. De volgende .NET functie berekent de controlesom toe met het deel van de GUID van de sleutel-id en de sleutel wissen.
    

        public static string CalculateChecksum(byte[] contentKey, Guid keyId)
        {
            const int ChecksumLength = 8;
            const int KeyIdLength = 16;

            byte[] encryptedKeyId = null;

            // Checksum is computed by AES-ECB encrypting the KID
            // with the content key.
            using (AesCryptoServiceProvider rijndael = new AesCryptoServiceProvider())
            {
                rijndael.Mode = CipherMode.ECB;
                rijndael.Key = contentKey;
                rijndael.Padding = PaddingMode.None;

                ICryptoTransform encryptor = rijndael.CreateEncryptor();
                encryptedKeyId = new byte[KeyIdLength];
                encryptor.TransformBlock(keyId.ToByteArray(), 0, KeyIdLength, encryptedKeyId, 0);
            }

            byte[] retVal = new byte[ChecksumLength];
            Array.Copy(encryptedKeyId, retVal, ChecksumLength);

            return Convert.ToBase64String(retVal);
        }


5. De sleutel maken met de **EncryptedContentKey** (geconverteerd naar een base64-gecodeerde string), **ProtectionKeyId**, **ProtectionKeyType**, **ContentKeyType**en **controlesom** waarden die u in de vorige stappen hebt ontvangen.

    
    Voor opslag van versleuteling, moeten de volgende eigenschappen worden opgenomen in het hoofdgedeelte van de aanvraag.
     
    Aanvraag body, eigenschap   | Beschrijving
    ---|---
    ID | De ContentKey-Id die we onszelf genereren met de volgende notatie "nb:kid:UUID:<NEW GUID>".
    ContentKeyType | Dit is het belangrijkste type inhoud als een geheel getal voor deze sleutel. Geven we de waarde 1 voor de codering van de opslag.
    EncryptedContentKey | We maken nieuwe inhoud waarde van een sleutel die de waarde van een 256-bits (32 bytes). De sleutel wordt gecodeerd met opslag codering x.509-certificaat dat we uit Microsoft Azure Media Services ophalen door het uitvoeren van een HTTP GET-verzoek voor de methoden GetProtectionKey en GetProtectionKeyId. Zie bijvoorbeeld de volgende .NET-code: de methode die is gedefinieerd **EncryptSymmetricKeyData** [hier](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.FileEncryption/EncryptionUtils.cs).
    ProtectionKeyId | Dit is de sleutel-id bescherming voor opslag codering x.509-certificaat dat is gebruikt om onze inhoud sleutel te coderen.
    ProtectionKeyType | Dit is het coderingstype voor de beveiliging-sleutel die is gebruikt voor het coderen van de sleutel. Deze waarde is de StorageEncryption(1) in ons voorbeeld.
    Checksum |De MD5 berekende controlesom voor de sleutel. Deze wordt berekend door de inhoud-Id met de sleutel te coderen. In de voorbeeldcode wordt gedemonstreerd hoe de controlesom wordt berekend.
    

###<a name="retrieve-the-protectionkeyid"></a>De ProtectionKeyId ophalen 
 

In het volgende voorbeeld ziet u hoe u kunt ophalen van de ProtectionKeyId, een vingerafdruk van het certificaat, het certificaat dat u gebruiken moet om de sleutel te coderen. Voer deze stap om ervoor te zorgen dat u al het gewenste certificaat op uw computer.


Aanvraag:
    
    
    GET https://media.windows.net/api/GetProtectionKeyId?contentKeyType=0 HTTP/1.1
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423034908&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=7eSLe1GHnxgilr3F2FPCGxdL2%2bwy%2f39XhMPGY9IizfU%3d
    x-ms-version: 2.11
    Host: media.windows.net
    

Antwoord:
    
    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 139
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    request-id: 2b6aa7a4-3a09-4b08-b581-26b55667f817
    x-ms-request-id: 2b6aa7a4-3a09-4b08-b581-26b55667f817
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Wed, 04 Feb 2015 02:42:52 GMT
    
    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Edm.String","value":"7D9BB04D9D0A4A24800CADBFEF232689E048F69C"}

###<a name="retrieve-the-protectionkey-for-the-protectionkeyid"></a>De ProtectionKey voor de ProtectionKeyId ophalen

In het volgende voorbeeld ziet u hoe de x.509-certificaat met behulp van de ProtectionKeyId dat u in de vorige stap hebt ontvangen op te halen.

Aanvraag:
        
    GET https://media.windows.net/api/GetProtectionKey?ProtectionKeyId='7D9BB04D9D0A4A24800CADBFEF232689E048F69C' HTTP/1.1
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=zbbef702-e769-2233-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423141026&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=lDBz5YXKiWe5L7eXOHsLHc9kKEUcUiFJvrNFFSksgkM%3d
    x-ms-version: 2.11
    x-ms-client-request-id: 78d1247a-58d7-40e5-96cc-70ff0dfa7382
    Host: media.windows.net
    


Antwoord:
    
    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 1227
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: 78d1247a-58d7-40e5-96cc-70ff0dfa7382
    request-id: 1523e8f3-8ed2-40fe-8a9a-5d81eb572cc8
    x-ms-request-id: 1523e8f3-8ed2-40fe-8a9a-5d81eb572cc8
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Thu, 05 Feb 2015 07:52:30 GMT
    
    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Edm.String",
    "value":"MIIDSTCCAjGgAwIBAgIQqf92wku/HLJGCbMAU8GEnDANBgkqhkiG9w0BAQQFADAuMSwwKgYDVQQDEyN3YW1zYmx1cmVnMDAxZW5jcnlwdGFsbHNlY3JldHMtY2VydDAeFw0xMjA1MjkwNzAwMDBaFw0zMjA1MjkwNzAwMDBaMC4xLDAqBgNVBAMTI3dhbXNibHVyZWcwMDFlbmNyeXB0YWxsc2VjcmV0cy1jZXJ0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAzR0SEbXefvUjb9wCUfkEiKtGQ5Gc328qFPrhMjSo+YHe0AVviZ9YaxPPb0m1AaaRV4dqWpST2+JtDhLOmGpWmmA60tbATJDdmRzKi2eYAyhhE76MgJgL3myCQLP42jDusWXWSMabui3/tMDQs+zfi1sJ4Ch/lm5EvksYsu6o8sCv29VRwxfDLJPBy2NlbV4GbWz5Qxp2tAmHoROnfaRhwp6WIbquk69tEtu2U50CpPN2goLAqx2PpXAqA+prxCZYGTHqfmFJEKtZHhizVBTFPGS3ncfnQC9QIEwFbPw6E5PO5yNaB68radWsp5uvDg33G1i8IT39GstMW6zaaG7cNQIDAQABo2MwYTBfBgNVHQEEWDBWgBCOGT2hPhsvQioZimw8M+jOoTAwLjEsMCoGA1UEAxMjd2Ftc2JsdXJlZzAwMWVuY3J5cHRhbGxzZWNyZXRzLWNlcnSCEKn/dsJLvxyyRgmzAFPBhJwwDQYJKoZIhvcNAQEEBQADggEBABcrQPma2ekNS3Wc5wGXL/aHyQaQRwFGymnUJ+VR8jVUZaC/U/f6lR98eTlwycjVwRL7D15BfClGEHw66QdHejaViJCjbEIJJ3p2c9fzBKhjLhzB3VVNiLIaH6RSI1bMPd2eddSCqhDIn3VBN605GcYXMzhYp+YA6g9+YMNeS1b+LxX3fqixMQIxSHOLFZ1G/H2xfNawv0VikH3djNui3EKT1w/8aRkUv/AAV0b3rYkP/jA1I0CPn0XFk7STYoiJ3gJoKq9EMXhit+Iwfz0sMkfhWG12/XO+TAWqsK1ZxEjuC9OzrY7pFnNxs4Mu4S8iinehduSpY+9mDd3dHynNwT4="}

### <a name="create-the-content-key"></a>De sleutel maken 

Nadat u hebt opgehaald van het x.509-certificaat en de openbare sleutel die wordt gebruikt voor het coderen van uw sleutel, een entiteit **ContentKey** maken en waarden van de eigenschap ook op Westers ingesteld.

Een van de waarden die u moet stellen wanneer de inhoud maken die sleutel is het type. In het geval van de opslag-codering is de waarde '1'. 

In het volgende voorbeeld ziet u hoe een **ContentKey** maken met een **ContentKeyType** voor opslag codering ("1") en de **ProtectionKeyType** ingesteld op '0' geeft aan dat de bescherming van sleutel-Id de vingerafdruk van het x.509-certificaat is.  


Aanvraag

    POST https://media.windows.net/api/ContentKeys HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423034908&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=7eSLe1GHnxgilr3F2FPCGxdL2%2bwy%2f39XhMPGY9IizfU%3d
    x-ms-version: 2.11
    Host: media.windows.net
    {
    "Name":"ContentKey",
    "ProtectionKeyId":"7D9BB04D9D0A4A24800CADBFEF232689E048F69C", 
    "ContentKeyType":"1", 
    "ProtectionKeyType":"0",
    "EncryptedContentKey":"your encrypted content key",
    "Checksum":"calculated checksum"
    }


Antwoord:
    
    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 777
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://media.windows.net/api/ContentKeys('nb%3Akid%3AUUID%3A9c8ea9c6-52bd-4232-8a43-8e43d8564a99')
    Server: Microsoft-IIS/8.5
    request-id: 76e85e0f-5cf1-44cb-b689-b3455888682c
    x-ms-request-id: 76e85e0f-5cf1-44cb-b689-b3455888682c
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Wed, 04 Feb 2015 02:37:46 GMT
    
    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#ContentKeys/@Element",
    "Id":"nb:kid:UUID:9c8ea9c6-52bd-4232-8a43-8e43d8564a99","Created":"2015-02-04T02:37:46.9684379Z",
    "LastModified":"2015-02-04T02:37:46.9684379Z",
    "ContentKeyType":1,
    "EncryptedContentKey":"your encrypted content key",
    "Name":"ContentKey",
    "ProtectionKeyId":"7D9BB04D9D0A4A24800CADBFEF232689E048F69C",
    "ProtectionKeyType":0,
    "Checksum":"calculated checksum"}

## <a name="create-an-asset"></a>Een actief maken

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
    
    {"Name":"BigBuckBunny" "Options":1}


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
       "Options":1,
       "Uri":"https://storagetestaccount001.blob.core.windows.net/asset-9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StorageAccountName":"storagetestaccount001"
    }
    
##<a name="associate-the-contentkey-with-an-asset"></a>De ContentKey koppelen aan een activum

Na het maken van de ContentKey te koppelen aan de activa met de bewerking $links zoals in het volgende voorbeeld wordt getoond:
    
Aanvraag:
    
    POST https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3Afbd7ce05-1087-401b-aaae-29f16383c801')/$links/ContentKeys HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Content-Type: application/json
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423141026&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=lDBz5YXKiWe5L7eXOHsLHc9kKEUcUiFJvrNFFSksgkM%3d
    x-ms-version: 2.11
    Host: media.windows.net

    
    {"uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/ContentKeys('nb%3Akid%3AUUID%3A01e6ea36-2285-4562-91f1-82c45736047c')"}

Antwoord:

    HTTP/1.1 204 No Content 

##<a name="create-an-assetfile"></a>Een AssetFile maken

De entiteit [AssetFile](http://msdn.microsoft.com/library/azure/hh974275.aspx) vertegenwoordigt een video- of audio-bestand dat is opgeslagen in een blob-container. Een elementbestand is altijd gekoppeld aan een actief en een actief mogelijk een of meer activa-bestanden bevatten. De taak van de Encoder van Media Services mislukt als u een bestand actief object niet is gekoppeld aan een digitaal bestand in een blob-container.

Houd er rekening mee dat het exemplaar van de **AssetFile** en de werkelijke mediabestand twee afzonderlijke objecten zijn. De AssetFile instantie bevat metagegevens over het mediabestand terwijl het mediabestand dat de werkelijke media-inhoud bevat.

Nadat u uw digitale media-bestand naar een blob-container uploadt, gebruikt u het **samenvoegen** HTTP-verzoek naar de AssetFile bijwerken met gegevens over uw media-bestand (niet weergegeven in dit onderwerp). 

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
       "IsEncrypted":"true",
       "EncryptionScheme" : "StorageEncryption", 
       "EncryptionVersion" : "1.0",       
       "EncryptionKeyId" : "nb:kid:UUID:32e6efaf-5fba-4538-b115-9d1cefe43510",
       "InitializationVector" : "397304628502661816</d:InitializationVector",
       "Options":0,
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
       "EncryptionVersion": "1.0",
       "EncryptionScheme": "StorageEncryption",
       "IsEncrypted":true,
       "EncryptionKeyId":"nb:kid:UUID:32e6efaf-5fba-4538-b115-9d1cefe43510",
       "InitializationVector":"397304628502661816</d:InitializationVector",
       "IsPrimary":false,
       "LastModified":"2015-01-19T00:34:08.1934137Z",
       "Created":"2015-01-19T00:34:08.1934137Z",
       "MimeType":"video/mp4",
       "ContentChecksum":null
    }
