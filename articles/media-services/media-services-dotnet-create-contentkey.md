<properties 
    pageTitle="ContentKeys maken met .NET" 
    description="Informatie over het maken van inhoud sleutels die beveiligde toegang tot de activa verlenen." 
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


#<a name="create-contentkeys-with-net"></a>ContentKeys maken met .NET

> [AZURE.SELECTOR]
- [REST](media-services-rest-create-contentkey.md)
- [.NET](media-services-dotnet-create-contentkey.md)

Media-Services kunt u maken en leveren van gecodeerde activa. Een **ContentKey** biedt veilige toegang tot uw s **actief**. 

Wanneer u een nieuw activum (bijvoorbeeld voordat u [bestanden uploadt](media-services-dotnet-upload-files.md)) maakt, kunt u de volgende coderingsopties: **StorageEncrypted**, **CommonEncryptionProtected**of **EnvelopeEncryptionProtected**. 

Als u activa aan uw klanten levert, kunt u [configureren voor activa dynamisch codeert](media-services-dotnet-configure-asset-delivery-policy.md) met een van de volgende twee coderingen: **DynamicEnvelopeEncryption** of **DynamicCommonEncryption**.

Gecodeerde activa moeten worden gekoppeld aan de **ContentKey**s. In dit artikel wordt beschreven hoe een sleutel te maken.

>[AZURE.NOTE] Bij het maken van een nieuwe **StorageEncrypted** actief met de .NET SDK van Media Services, wordt de **ContentKey** automatisch gemaakt en aan het activum is gekoppeld.

##<a name="contentkeytype"></a>ContentKeyType

Een van de waarden die u moet stellen wanneer een inhoud maken die sleutel is het belangrijkste type inhoud. Kies een van de volgende waarden. 

    public enum ContentKeyType
    {
        /// <summary>
        /// Specifies a content key for common encryption.
        /// </summary>
        /// <remarks>This is the default value.</remarks>
        CommonEncryption = 0,

        /// <summary>
        /// Specifies a content key for storage encryption.
        /// </summary>
        StorageEncryption = 1,

        /// <summary>
        /// Specifies a content key for configuration encryption.
        /// </summary>
        ConfigurationEncryption = 2,

        /// <summary>
        /// Specifies a content key for Envelope encryption.  Only used internally.
        /// </summary>
        EnvelopeEncryption = 4
    }

##<a id="envelope_contentkey"></a>Type envelop ContentKey maken

Het volgende codefragment maakt een sleutel van het type envelop. De sleutel wordt vervolgens gekoppeld aan de opgegeven activa.

    static public IContentKey CreateEnvelopeTypeContentKey(IAsset asset)
    {
        // Create envelope encryption content key
        Guid keyId = Guid.NewGuid();
        byte[] contentKey = GetRandomBuffer(16);

        IContentKey key = _context.ContentKeys.Create(
                                keyId,
                                contentKey,
                                "ContentKey",
                                ContentKeyType.EnvelopeEncryption);

        asset.ContentKeys.Add(key);

        return key;
    }

    static private byte[] GetRandomBuffer(int size)
    {
        byte[] randomBytes = new byte[size];
        using (RNGCryptoServiceProvider rng = new RNGCryptoServiceProvider())
        {
            rng.GetBytes(randomBytes);
        }

        return randomBytes;
    }

oproep

    IContentKey key = CreateEnvelopeTypeContentKey(encryptedsset);



##<a id="common_contentkey"></a>Voorkomende type ContentKey maken    

Het volgende codefragment maakt een sleutel van het algemene coderingstype. De sleutel wordt vervolgens gekoppeld aan de opgegeven activa.

    static public IContentKey CreateCommonTypeContentKey(IAsset asset)
    {
        // Create common encryption content key
        Guid keyId = Guid.NewGuid();
        byte[] contentKey = GetRandomBuffer(16);

        IContentKey key = _context.ContentKeys.Create(
                                keyId,
                                contentKey,
                                "ContentKey",
                                ContentKeyType.CommonEncryption);

        // Associate the key with the asset.
        asset.ContentKeys.Add(key);

        return key;
    }

    static private byte[] GetRandomBuffer(int length)
    {
        var returnValue = new byte[length];

        using (var rng =
            new System.Security.Cryptography.RNGCryptoServiceProvider())
        {
            rng.GetBytes(returnValue);
        }

        return returnValue;
    }
oproep

    IContentKey key = CreateCommonTypeContentKey(encryptedsset); 


##<a name="media-services-learning-paths"></a>Leerfasen Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
