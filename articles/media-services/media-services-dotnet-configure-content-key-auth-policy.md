<properties 
    pageTitle="Inhoud sleutel autorisatiebeleid met .NET SDK van Media Services configureren | Microsoft Azure" 
    description="Informatie over het configureren van autorisatiebeleid voor een sleutel met behulp van Media Services .NET SDK." 
    services="media-services" 
    documentationCenter="" 
    authors="Mingfeiy" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/15/2016"
    ms.author="juliako;mingfeiy"/>



# <a name="dynamic-encryption-configure-content-key-authorization-policy"></a>Dynamische codering: inhoud sleutel autorisatiebeleid configureren

[AZURE.INCLUDE [media-services-selector-content-key-auth-policy](../../includes/media-services-selector-content-key-auth-policy.md)]

##<a name="overview"></a>Overzicht

Microsoft Azure Media Services kunt u MPEG-streepje, soepele Streaming en HTTP-Live Streaming (HLS) past streams met Advanced Encryption Standard (AES) (met 128-bits coderingssleutels) of [Microsoft PlayReady-DRM](https://www.microsoft.com/playready/overview/)beveiligd leveren. AMS kunt u streepje streams die zijn gecodeerd met de DRM Widevine leveren. Zowel PlayReady als Widevine worden per specificatie van de gemeenschappelijke codering (ISO/IEC 23001-7 CENC) gecodeerd.

Media Services biedt ook een **Sleutel/licentie bezorgservice** van die clients ophalen kunnen, AES-sleutels of PlayReady/Widevine licenties voor de gecodeerde inhoud af te spelen.

Als u Media-Services voor het coderen van een actief, moet u een coderingssleutel (**CommonEncryption** of **EnvelopeEncryption**) koppelen aan de activa (zoals beschreven [hier](media-services-dotnet-create-contentkey.md)) en ook autorisatiebeleid voor de sleutel configureren (zoals beschreven in dit artikel).

Een stroom door een speler is aangevraagd, Media Services maakt gebruik van de opgegeven sleutel voor het coderen van inhoud met AES of DRM-versleuteling dynamisch. Om te decoderen van de gegevensstroom, de speler vraagt om de sleutel van de belangrijkste leveringsservice. De service geëvalueerd om te bepalen of de gebruiker is gemachtigd om de sleutel, het autorisatiebeleid dat u hebt opgegeven voor de sleutel.

Media Services biedt ondersteuning voor meerdere manieren voor het verifiëren van gebruikers die sleutel aanvragen. De inhoud sleutel autorisatiebeleid kan zijn voor een of meer vergunning beperkingen: beperking **openen** of **token** . Het token beperkte beleid moet vergezeld gaan van een token dat wordt afgegeven door een beveiligde Token Service (STS). Media Services ondersteunt tokens in de **Eenvoudige Web-Tokens** ([SWT](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2)) en **JSON Web Token** ([JWT](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3))-indeling.

Media Services biedt geen Token Services beveiligen. U kunt een aangepaste STS maken of gebruikmaken van Microsoft Azure ACS op tokens probleem. De STS moeten worden geconfigureerd voor een token dat is ondertekend met de opgegeven sleutel en probleem claims die u hebt opgegeven in de configuratie van de token beperking (zoals beschreven in dit artikel). De belangrijkste bezorgingsservice Media Services wordt de coderingssleutel terug naar de client als het token geldig is en die zijn geconfigureerd voor de sleutel komen overeen met de vorderingen in het token.

Zie voor meer informatie

[JWT token-authenitcation](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/)

[Integratie van Azure Media Services OWIN MVC app met Azure Active Directory gebaseerd en leveren van inhoud sleutel op basis van de claims JWT beperken](http://www.gtrifonov.com/2015/01/24/mvc-owin-azure-media-services-ad-integration/).

[Gebruik Azure ACS op tokens probleem](http://mingfeiy.com/acs-with-key-services).

###<a name="some-considerations-apply"></a>Sommige overwegingen zijn van toepassing:

- Als u dynamische verpakking en dynamische codering gebruikt, moet u ervoor zorgen om ten minste één gereserveerde eenheid streaming. Zie [voor het schalen van een Media-Service](media-services-portal-manage-streaming-endpoints.md)voor meer informatie.
- Uw activa moet bevatten een reeks geavanceerde bitsnelheid MP4s of adaptieve bitsnelheid goede Streaming-bestanden. Zie voor meer informatie, [een actief coderen](media-services-encode-asset.md).
- Uploaden en uw activa met de optie **AssetCreationOptions.StorageEncrypted** coderen.
- Als u van plan bent meerdere inhoud sleutels waarvoor dezelfde beleidsconfiguratie, moet het is raadzaam een enkele vergunning beleid maken en deze opnieuw gebruiken met meerdere sleutels van de inhoud.
- De sleutel bezorgingsservice slaat ContentKeyAuthorizationPolicy en de bijbehorende objecten (beleidsopties en beperkingen) gedurende 15 minuten.  Als u een ContentKeyAuthorizationPolicy maken en geef een beperking van de "Token" gebruikt, test, en pas het beleid aan de beperking 'Openen', duurt het ongeveer 15 minuten voordat het beleid voor de 'Open' versie van het beleid.
- Als u toevoegen of bijwerken van beleid voor levering van het actief, moet u een bestaande locator verwijderen (indien aanwezig) en het maken van een nieuwe locator.
- Op dit moment u streaming format harde schijven niet coderen of progressief gedownload.


##<a name="aes-128-dynamic-encryption"></a>Dynamische Encryption AES-128

###<a name="open-restriction"></a>Open beperking

Open beperking betekent dat het systeem de sleutel bieden voor iedereen die een belangrijke verzoek indient. Deze beperking kan handig zijn voor testdoeleinden.

In het volgende voorbeeld wordt autorisatiebeleid open gemaakt en toegevoegd aan de sleutel.

statische public void AddOpenAuthorizationPolicy (IContentKey contentKey) {/ / ContentKeyAuthorizationPolicy Open beperkingen maken / / en maakt u autorisatiebeleid beleid IContentKeyAuthorizationPolicy = _context.
ContentKeyAuthorizationPolicies.
CreateAsync ("Open vergunning Policy"). Resultaat;

Lijst<ContentKeyAuthorizationPolicyRestriction> beperkingen nieuwe lijst =<ContentKeyAuthorizationPolicyRestriction>();
    
        ContentKeyAuthorizationPolicyRestriction restriction =
            new ContentKeyAuthorizationPolicyRestriction
            {
                Name = "HLS Open Authorization Policy",
                KeyRestrictionType = (int)ContentKeyRestrictionType.Open,
                Requirements = null // no requirements needed for HLS
            };
    
        restrictions.Add(restriction);
    
        IContentKeyAuthorizationPolicyOption policyOption =
            _context.ContentKeyAuthorizationPolicyOptions.Create(
            "policy", 
            ContentKeyDeliveryType.BaselineHttp, 
            restrictions, 
            "");
    
        policy.Options.Add(policyOption);
    
        // Add ContentKeyAutorizationPolicy to ContentKey
        contentKey.AuthorizationPolicyId = policy.Id;
        IContentKey updatedKey = contentKey.UpdateAsync().Result;
        Console.WriteLine("Adding Key to Asset: Key ID is " + updatedKey.Id);
    }


###<a name="token-restriction"></a>Token-beperking

In deze sectie wordt beschreven hoe een inhoud sleutel verificatiebeleid maken en koppelen aan de sleutel. Het verificatiebeleid wordt beschreven welke behoeften moeten worden voldaan om te bepalen of de gebruiker is gemachtigd voor het ontvangen van de sleutel (bijvoorbeeld, wordt de lijst 'sleutel verificatie' bevatten het token is ondertekend met de sleutel).

De token als beperkingsoptie wilt configureren, moet u een XML voor het beschrijven van de behoeften van het token gebruikt. De configuratie van de token beperking XML moet voldoen aan de volgende XML-schema.

####<a id="schema"></a>Schema token beperking
    
    <?xml version="1.0" encoding="utf-8"?>
    <xs:schema xmlns:tns="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/TokenRestrictionTemplate/v1" elementFormDefault="qualified" targetNamespace="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/TokenRestrictionTemplate/v1" xmlns:xs="http://www.w3.org/2001/XMLSchema">
      <xs:complexType name="TokenClaim">
        <xs:sequence>
          <xs:element name="ClaimType" nillable="true" type="xs:string" />
          <xs:element minOccurs="0" name="ClaimValue" nillable="true" type="xs:string" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="TokenClaim" nillable="true" type="tns:TokenClaim" />
      <xs:complexType name="TokenRestrictionTemplate">
        <xs:sequence>
          <xs:element minOccurs="0" name="AlternateVerificationKeys" nillable="true" type="tns:ArrayOfTokenVerificationKey" />
          <xs:element name="Audience" nillable="true" type="xs:anyURI" />
          <xs:element name="Issuer" nillable="true" type="xs:anyURI" />
          <xs:element name="PrimaryVerificationKey" nillable="true" type="tns:TokenVerificationKey" />
          <xs:element minOccurs="0" name="RequiredClaims" nillable="true" type="tns:ArrayOfTokenClaim" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="TokenRestrictionTemplate" nillable="true" type="tns:TokenRestrictionTemplate" />
      <xs:complexType name="ArrayOfTokenVerificationKey">
        <xs:sequence>
          <xs:element minOccurs="0" maxOccurs="unbounded" name="TokenVerificationKey" nillable="true" type="tns:TokenVerificationKey" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="ArrayOfTokenVerificationKey" nillable="true" type="tns:ArrayOfTokenVerificationKey" />
      <xs:complexType name="TokenVerificationKey">
        <xs:sequence />
      </xs:complexType>
      <xs:element name="TokenVerificationKey" nillable="true" type="tns:TokenVerificationKey" />
      <xs:complexType name="ArrayOfTokenClaim">
        <xs:sequence>
          <xs:element minOccurs="0" maxOccurs="unbounded" name="TokenClaim" nillable="true" type="tns:TokenClaim" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="ArrayOfTokenClaim" nillable="true" type="tns:ArrayOfTokenClaim" />
      <xs:complexType name="SymmetricVerificationKey">
        <xs:complexContent mixed="false">
          <xs:extension base="tns:TokenVerificationKey">
            <xs:sequence>
              <xs:element name="KeyValue" nillable="true" type="xs:base64Binary" />
            </xs:sequence>
          </xs:extension>
        </xs:complexContent>
      </xs:complexType>
      <xs:element name="SymmetricVerificationKey" nillable="true" type="tns:SymmetricVerificationKey" />
    </xs:schema>

Bij het configureren van de **token** beleid worden beperkt, moet u de primaire** sleutel voor verificatie**, **uitgever** en **doelgroep** parameters opgeven. De **verificatie van de primaire sleutel **bevat de sleutel die het token is ondertekend met, **uitgever** is de veilige token service het token verleent. De **doelgroep** (soms **scope**genoemd) beschrijft de intentie van het token of de resource de token gemachtigd voor toegang tot. De levering van belangrijke Media Services-service wordt gevalideerd dat deze waarden in het token overeenkomen met de waarden in de sjabloon. 

Wanneer u **Media Services SDK voor .NET**gebruikt, kunt u de klasse **TokenRestrictionTemplate** om de beperking-token te genereren.
In het volgende voorbeeld wordt een verificatiebeleid gemaakt met een beperking van de token. In dit voorbeeld wordt de client moet een token met presenteren: ondertekening sleutel (VerificationKey), de uitgever van een token en vorderingen vereist.
    
    public static string AddTokenRestrictedAuthorizationPolicy(IContentKey contentKey)
    {
        string tokenTemplateString = GenerateTokenRequirements();
    
        IContentKeyAuthorizationPolicy policy = _context.
                                ContentKeyAuthorizationPolicies.
                                CreateAsync("HLS token restricted authorization policy").Result;
    
        List<ContentKeyAuthorizationPolicyRestriction> restrictions =
                new List<ContentKeyAuthorizationPolicyRestriction>();
    
        ContentKeyAuthorizationPolicyRestriction restriction =
                new ContentKeyAuthorizationPolicyRestriction
                {
                    Name = "Token Authorization Policy",
                    KeyRestrictionType = (int)ContentKeyRestrictionType.TokenRestricted,
                    Requirements = tokenTemplateString
                };
    
        restrictions.Add(restriction);
    
        //You could have multiple options 
        IContentKeyAuthorizationPolicyOption policyOption =
            _context.ContentKeyAuthorizationPolicyOptions.Create(
                "Token option for HLS",
                ContentKeyDeliveryType.BaselineHttp,
                restrictions,
                null  // no key delivery data is needed for HLS
                );
    
        policy.Options.Add(policyOption);
    
        // Add ContentKeyAutorizationPolicy to ContentKey
        contentKey.AuthorizationPolicyId = policy.Id;
        IContentKey updatedKey = contentKey.UpdateAsync().Result;
        Console.WriteLine("Adding Key to Asset: Key ID is " + updatedKey.Id);
    
        return tokenTemplateString;
    }
    
    static private string GenerateTokenRequirements()
    {
        TokenRestrictionTemplate template = new TokenRestrictionTemplate(TokenType.SWT);
    
        template.PrimaryVerificationKey = new SymmetricVerificationKey();
        template.AlternateVerificationKeys.Add(new SymmetricVerificationKey());
            template.Audience = _sampleAudience.ToString();
            template.Issuer = _sampleIssuer.ToString();
    
        template.RequiredClaims.Add(TokenClaim.ContentKeyIdentifierClaim);
    
        return TokenRestrictionTemplateSerializer.Serialize(template);
    }

####<a id="test"></a>Test-token

Als u een test token op basis van het token beperking die is gebruikt om het verificatiebeleid key, doet u het volgende.
    
    // Deserializes a string containing an Xml representation of a TokenRestrictionTemplate
    // back into a TokenRestrictionTemplate class instance.
    TokenRestrictionTemplate tokenTemplate =
        TokenRestrictionTemplateSerializer.Deserialize(tokenTemplateString);
    
    // Generate a test token based on the the data in the given TokenRestrictionTemplate.
    // Note, you need to pass the key id Guid because we specified 
    // TokenClaim.ContentKeyIdentifierClaim in during the creation of TokenRestrictionTemplate.
    Guid rawkey = EncryptionUtils.GetKeyIdAsGuid(key.Id);
    
    //The GenerateTestToken method returns the token without the word “Bearer” in front
    //so you have to add it in front of the token string. 
    string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate, null, rawkey);
    Console.WriteLine("The authorization token is:\nBearer {0}", testToken);
    Console.WriteLine();


##<a name="playready-dynamic-encryption"></a>Dynamische PlayReady-codering 

Media-Services kunt u voor het configureren van de rechten en beperkingen die u voor de runtime PlayReady DRM afdwingen wilt wanneer een gebruiker probeert om beveiligde inhoud af te spelen. 

Bij het beveiligen van uw inhoud met PlayReady, is een van de dingen die u moet opgeven in uw beleid een XML-tekenreeks die de [PlayReady licentie sjabloon](media-services-playready-license-template-overview.md)definieert. In Media Services SDK voor .NET kunt de klassen **PlayReadyLicenseResponseTemplate** en **PlayReadyLicenseTemplate** u de sjabloon PlayReady definiëren.

[In dit onderwerp](media-services-protect-with-drm.md) ziet u hoe uw inhoud met **PlayReady** en **Widevine**coderen.

###<a name="open-restriction"></a>Open beperking
    
Open beperking betekent dat het systeem de sleutel bieden voor iedereen die een belangrijke verzoek indient. Deze beperking kan handig zijn voor testdoeleinden.

In het volgende voorbeeld wordt autorisatiebeleid open gemaakt en toegevoegd aan de sleutel.

    static public void AddOpenAuthorizationPolicy(IContentKey contentKey)
    {
    
        // Create ContentKeyAuthorizationPolicy with Open restrictions 
        // and create authorization policy          
    
        List<ContentKeyAuthorizationPolicyRestriction> restrictions = new List<ContentKeyAuthorizationPolicyRestriction>
        {
            new ContentKeyAuthorizationPolicyRestriction 
            { 
                Name = "Open", 
                KeyRestrictionType = (int)ContentKeyRestrictionType.Open, 
                Requirements = null
            }
        };
    
        // Configure PlayReady license template.
        string newLicenseTemplate = ConfigurePlayReadyLicenseTemplate();
    
        IContentKeyAuthorizationPolicyOption policyOption =
            _context.ContentKeyAuthorizationPolicyOptions.Create("",
                ContentKeyDeliveryType.PlayReadyLicense,
                    restrictions, newLicenseTemplate);
    
        IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                    ContentKeyAuthorizationPolicies.
                    CreateAsync("Deliver Common Content Key with no restrictions").
                    Result;
    
    
        contentKeyAuthorizationPolicy.Options.Add(policyOption);
    
        // Associate the content key authorization policy with the content key.
        contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
        contentKey = contentKey.UpdateAsync().Result;
    }

###<a name="token-restriction"></a>Token-beperking

De token als beperkingsoptie wilt configureren, moet u een XML voor het beschrijven van de behoeften van het token gebruikt. De configuratie van de token beperking XML moet voldoen aan het XML-schema in [deze](#schema) sectie wordt weergegeven.
    
    public static string AddTokenRestrictedAuthorizationPolicy(IContentKey contentKey)
    {
        string tokenTemplateString = GenerateTokenRequirements();
    
        IContentKeyAuthorizationPolicy policy = _context.
                                ContentKeyAuthorizationPolicies.
                                CreateAsync("HLS token restricted authorization policy").Result;
    
        List<ContentKeyAuthorizationPolicyRestriction> restrictions = new List<ContentKeyAuthorizationPolicyRestriction>
        {
            new ContentKeyAuthorizationPolicyRestriction 
            { 
                Name = "Token Authorization Policy", 
                KeyRestrictionType = (int)ContentKeyRestrictionType.TokenRestricted,
                Requirements = tokenTemplateString, 
            }
        };
    
        // Configure PlayReady license template.
        string newLicenseTemplate = ConfigurePlayReadyLicenseTemplate();
    
        IContentKeyAuthorizationPolicyOption policyOption =
            _context.ContentKeyAuthorizationPolicyOptions.Create("Token option",
                ContentKeyDeliveryType.PlayReadyLicense,
                    restrictions, newLicenseTemplate);
    
        IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                    ContentKeyAuthorizationPolicies.
                    CreateAsync("Deliver Common Content Key with no restrictions").
                    Result;
                
        policy.Options.Add(policyOption);
    
        // Add ContentKeyAutorizationPolicy to ContentKey
        contentKeyAuthorizationPolicy.Options.Add(policyOption);
    
        // Associate the content key authorization policy with the content key
        contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
        contentKey = contentKey.UpdateAsync().Result;
    
        return tokenTemplateString;
    }
    
    static private string GenerateTokenRequirements()
    {
    
        TokenRestrictionTemplate template = new TokenRestrictionTemplate(TokenType.SWT);
    
        template.PrimaryVerificationKey = new SymmetricVerificationKey();
        template.AlternateVerificationKeys.Add(new SymmetricVerificationKey());
            template.Audience = _sampleAudience.ToString();
            template.Issuer = _sampleIssuer.ToString();
    
    
        template.RequiredClaims.Add(TokenClaim.ContentKeyIdentifierClaim);
    
        return TokenRestrictionTemplateSerializer.Serialize(template);
    } 
    
    static private string ConfigurePlayReadyLicenseTemplate()
    {
        // The following code configures PlayReady License Template using .NET classes
        // and returns the XML string.

        //The PlayReadyLicenseResponseTemplate class represents the template for the response sent back to the end user. 
        //It contains a field for a custom data string between the license server and the application 
        //(may be useful for custom app logic) as well as a list of one or more license templates.
        PlayReadyLicenseResponseTemplate responseTemplate = new PlayReadyLicenseResponseTemplate();

        // The PlayReadyLicenseTemplate class represents a license template for creating PlayReady licenses
        // to be returned to the end users. 
        //It contains the data on the content key in the license and any rights or restrictions to be 
        //enforced by the PlayReady DRM runtime when using the content key.
        PlayReadyLicenseTemplate licenseTemplate = new PlayReadyLicenseTemplate();
        //Configure whether the license is persistent (saved in persistent storage on the client) 
        //or non-persistent (only held in memory while the player is using the license).  
        licenseTemplate.LicenseType = PlayReadyLicenseType.Nonpersistent;
       
        // AllowTestDevices controls whether test devices can use the license or not.  
        // If true, the MinimumSecurityLevel property of the license
        // is set to 150.  If false (the default), the MinimumSecurityLevel property of the license is set to 2000.
        licenseTemplate.AllowTestDevices = true;


        // You can also configure the Play Right in the PlayReady license by using the PlayReadyPlayRight class. 
        // It grants the user the ability to playback the content subject to the zero or more restrictions 
        // configured in the license and on the PlayRight itself (for playback specific policy). 
        // Much of the policy on the PlayRight has to do with output restrictions 
        // which control the types of outputs that the content can be played over and 
        // any restrictions that must be put in place when using a given output.
        // For example, if the DigitalVideoOnlyContentRestriction is enabled, 
        //then the DRM runtime will only allow the video to be displayed over digital outputs 
        //(analog video outputs won’t be allowed to pass the content).

        //IMPORTANT: These types of restrictions can be very powerful but can also affect the consumer experience. 
        // If the output protections are configured too restrictive, 
        // the content might be unplayable on some clients. For more information, see the PlayReady Compliance Rules document.

        // For example:
        //licenseTemplate.PlayRight.AgcAndColorStripeRestriction = new AgcAndColorStripeRestriction(1);

        responseTemplate.LicenseTemplates.Add(licenseTemplate);

        return MediaServicesLicenseTemplateSerializer.Serialize(responseTemplate);
    }


Zie [in deze](#test) sectie als u een test token op basis van het token beperking die is gebruikt voor de verificatie van de sleutel. 

##<a id="types"></a>Die worden gebruikt bij het definiëren van ContentKeyAuthorizationPolicy

###<a id="ContentKeyRestrictionType"></a>ContentKeyRestrictionType

    public enum ContentKeyRestrictionType
    {
        Open = 0,
        TokenRestricted = 1,
        IPRestricted = 2,
    }

###<a id="ContentKeyDeliveryType"></a>ContentKeyDeliveryType

    public enum ContentKeyDeliveryType
    {
      None = 0,
      PlayReadyLicense = 1,
      BaselineHttp = 2,
      Widevine = 3
    }

###<a id="TokenType"></a>TokenType

    public enum TokenType
    {
        Undefined = 0,
        SWT = 1,
        JWT = 2,
    }



##<a name="media-services-learning-paths"></a>Leerfasen Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="next-step"></a>Volgende stap
Nu dat u autorisatiebeleid inhoud sleutel hebt geconfigureerd, gaat u naar het onderwerp van [het activum levering beleid configureren](media-services-dotnet-configure-asset-delivery-policy.md) .
 
