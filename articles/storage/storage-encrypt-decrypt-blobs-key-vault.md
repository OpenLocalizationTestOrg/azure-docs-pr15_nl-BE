<properties
    pageTitle="Zelfstudie: Coderen en decoderen van BLOB's in Microsoft Azure opslag met Azure sleutel kluis | Microsoft Azure"
    description="Deze handleiding helpt u bij het coderen en decoderen van een client-side codering gebruiken voor de opslag van Microsoft Azure met Azure sleutel kluis blob."
    services="storage"
    documentationCenter=""
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="required"
    ms.date="10/18/2016"
    ms.author="lakasa;robinsh"/>

# <a name="tutorial-encrypt-and-decrypt-blobs-in-microsoft-azure-storage-using-azure-key-vault"></a>Zelfstudie: Coderen en decoderen van BLOB's in Microsoft Azure opslag met Azure sleutel kluis

## <a name="introduction"></a>Inleiding

In deze zelfstudie wordt beschreven hoe u het gebruik van client-side storage codering met Azure sleutel kluis. Het helpt u bij het coderen en decoderen van een blob in een consoletoepassing met behulp van deze technologieën.

**Geschatte tijd:** 20 minuten

Zie voor informatie over Azure sleutel kluis, [Wat is Azure sleutel kluis?](../key-vault/key-vault-whatis.md).

Zie informatie over codering voor de opslag van Azure client-side [Client-Side codering en Azure sleutel kluis voor de opslag van Microsoft Azure](storage-client-side-encryption.md).


## <a name="prerequisites"></a>Vereisten

Deze zelfstudie hebt u het volgende:

- Een account Azure opslag
- Visual Studio 2013 of later
- Azure PowerShell


## <a name="overview-of-client-side-encryption"></a>Overzicht van client-side-codering

Zie voor een overzicht van client-side-codering voor opslag in Azure, [Client-Side codering en Azure sleutel kluis voor Microsoft Azure opslag](storage-client-side-encryption.md)

Hier volgt een korte beschrijving van de werking van client-side-codering:

1. De opslag van Azure client SDK genereert een inhoud coderingssleutel (CEK), die een symmetrische sleutel met een eenmalige gebruik is.
2. Worden klantgegevens gecodeerd met behulp van deze CEK.
3. De CEK wordt vervolgens verpakt (gecodeerd) met behulp van de coderingssleutel key (KEK). De KEK kan wordt aangeduid met een sleutel-id en een combinatie van een asymmetrische sleutel of een symmetrische sleutel en kunnen worden lokaal beheerd of opgeslagen in Azure sleutel kluis. De opslag-client zelf heeft nooit toegang tot de KEK. Dit roept alleen de belangrijke onmiddellijke algoritme die wordt geleverd door de sleutel kluis. Klanten kunnen kiezen voor het gebruik van aangepaste providers voor sleutel verpakking/uitpakken als ze willen.
4. De gecodeerde gegevens is vervolgens naar de Azure Storage-service geüpload.


## <a name="set-up-your-azure-key-vault"></a>Stel uw Azure sleutel kluis
U moet doorgaan met deze zelfstudie, de volgende stappen die worden beschreven in de zelfstudie [aan de slag met Azure sleutel kluis](../key-vault/key-vault-get-started.md):

- Maak een sleutel kluis.
- Een sleutel of een geheim om de sleutel toevoegen.
- Een toepassing met Azure Active Directory registreren.
- Toestaan dat de toepassing voor het gebruik van de sleutel of een geheim.

Noteer de ClientID en ClientSecret die zijn gegenereerd tijdens het registreren van een toepassing met Azure Active Directory.

Beide sleutels in de sleutel kluis maken. We ervan uitgaan dat voor de rest van de zelfstudie gebruikt de volgende namen: ContosoKeyVault en TestRSAKey1.


## <a name="create-a-console-application-with-packages-and-appsettings"></a>Maak een consoletoepassing met pakketten en AppSettings

Maak een nieuwe consoletoepassing in Visual Studio.

Nodig nuget pakketten in de Package Manager-Console toevoegen.

    Install-Package WindowsAzure.Storage

    // This is the latest stable release for ADAL.
    Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.16.204221202

    Install-Package Microsoft.Azure.KeyVault
    Install-Package Microsoft.Azure.KeyVault.Extensions


AppSettings toevoegen aan de App.Config.

    <appSettings>
        <add key="accountName" value="myaccount"/>
        <add key="accountKey" value="theaccountkey"/>
        <add key="clientId" value="theclientid"/>
        <add key="clientSecret" value="theclientsecret"/>
        <add key="container" value="stuff"/>
    </appSettings>

Voeg de volgende `using` -instructies en zorg ervoor dat een verwijzing naar System.Configuration toevoegen aan het project.

    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using System.Configuration;
    using Microsoft.WindowsAzure.Storage.Auth;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    using Microsoft.Azure.KeyVault;
    using System.Threading;     
    using System.IO;


## <a name="add-a-method-to-get-a-token-to-your-console-application"></a>Een methode als u een token aan een consoletoepassing toevoegen

De volgende methode wordt gebruikt door de sleutel kluis klassen die nodig zijn om te verifiëren om toegang tot uw sleutel kluis.

    private async static Task<string> GetToken(string authority, string resource, string scope)
    {
        var authContext = new AuthenticationContext(authority);
        ClientCredential clientCred = new ClientCredential(
            ConfigurationManager.AppSettings["clientId"],
            ConfigurationManager.AppSettings["clientSecret"]);
        AuthenticationResult result = await authContext.AcquireTokenAsync(resource, clientCred);

        if (result == null)
            throw new InvalidOperationException("Failed to obtain the JWT token");

        return result.AccessToken;
    }

## <a name="access-storage-and-key-vault-in-your-program"></a>Toegang tot opslag en sleutel kluis in uw programma

Voeg de volgende code in de Main-functie.

    // This is standard code to interact with Blob storage.
    StorageCredentials creds = new StorageCredentials(
        ConfigurationManager.AppSettings["accountName"],
        ConfigurationManager.AppSettings["accountKey"]);
    CloudStorageAccount account = new CloudStorageAccount(creds, useHttps: true);
    CloudBlobClient client = account.CreateCloudBlobClient();
    CloudBlobContainer contain = client.GetContainerReference(ConfigurationManager.AppSettings["container"]);
    contain.CreateIfNotExists();

    // The Resolver object is used to interact with Key Vault for Azure Storage.
    // This is where the GetToken method from above is used.
    KeyVaultKeyResolver cloudResolver = new KeyVaultKeyResolver(GetToken);


> [AZURE.NOTE] Sleutel kluis objectmodellen
>
>Het is belangrijk te begrijpen dat er daadwerkelijk twee sleutel kluis objectmodellen rekening houden met zijn: een op basis van de REST-API (KeyVault-naamruimte) en de andere is een uitbreiding voor client-side-codering.

> De sleutel kluis Client communiceert met de REST API en begrijpt de JSON-Web-sleutels en -geheimen voor de twee soorten dingen die zijn opgenomen in de sleutel kluis.

> De sleutel kluis-extensies zijn klassen die specifiek gemaakt voor client-side-codering in Azure opslag. Ze bevatten een interface voor sleutels (IKey) en klassen die is gebaseerd op het concept van een sleutel-omzetter. Er zijn twee implementaties van IKey die u nodig hebt: RSAKey en SymmetricKey. Nu ze toevallig samenvallen met de dingen die zijn opgenomen in een kluis met sleutel, maar op dit moment zijn onafhankelijke klassen (dus de sleutel en geheime opgehaald door de sleutel kluis Client IKey niet geïmplementeerd).


## <a name="encrypt-blob-and-upload"></a>Blob coderen en uploaden
Voeg de volgende code om een blob te coderen en upload deze naar uw account Azure opslag. De **ResolveKeyAsync** -methode die wordt gebruikt als resultaat een IKey.


    // Retrieve the key that you created previously.
    // The IKey that is returned here is an RsaKey.
    // Remember that we used the names contosokeyvault and testrsakey1.
    var rsa = cloudResolver.ResolveKeyAsync("https://contosokeyvault.vault.azure.net/keys/TestRSAKey1", CancellationToken.None).GetAwaiter().GetResult();


    // Now you simply use the RSA key to encrypt by setting it in the BlobEncryptionPolicy.
    BlobEncryptionPolicy policy = new BlobEncryptionPolicy(rsa, null);
    BlobRequestOptions options = new BlobRequestOptions() { EncryptionPolicy = policy };

    // Reference a block blob.
    CloudBlockBlob blob = contain.GetBlockBlobReference("MyFile.txt");

    // Upload using the UploadFromStream method.
    using (var stream = System.IO.File.OpenRead(@"C:\data\MyFile.txt"))
        blob.UploadFromStream(stream, stream.Length, null, options, null);


Hieronder vindt u een screenshot van het [Klassieke Portal Azure](https://manage.windowsazure.com) voor een blob die zijn gecodeerd met behulp van client-side-codering met een sleutel die is opgeslagen in een kluis met sleutel. De eigenschap **KeyId** wordt de URI voor de sleutel in een sleutel kluis die als de KEK fungeert. De eigenschap **EncryptedKey** bevat de gecodeerde versie van de CEK.

![Schermafbeelding van Blob-metagegevens metagegevens voor codering bevat][1]

> [AZURE.NOTE] Als u de constructor BlobEncryptionPolicy bekijkt, ziet u kunt accepteren een sleutel en/of een resolver. Let wel, die op dit moment kunt u een resolver voor codering omdat deze momenteel niet wordt ondersteund door een standaardsleutel.



## <a name="decrypt-blob-and-download"></a>Blob decoderen en downloaden
Decodering is in feite wanneer met de klassen Resolver zinvol zijn. De ID van de sleutel voor de codering is gekoppeld aan de blob in de metagegevens, dus er is geen reden voor u de sleutel ophalen en de koppeling tussen sleutel en blob onthouden. U hoeft alleen om ervoor te zorgen dat de sleutel in sleutel kluis blijft.   

De persoonlijke sleutel van RSA-sleutel in sleutel kluis, blijft dus voor decodering optreden, de gecodeerde sleutel uit de blob-metagegevens met de CEK wordt verzonden naar een kluis met sleutel voor decodering.

Voeg de volgende om te decoderen van de label die u zojuist hebt geüpload.

    // In this case, we will not pass a key and only pass the resolver because
    // this policy will only be used for downloading / decrypting.
    BlobEncryptionPolicy policy = new BlobEncryptionPolicy(null, cloudResolver);
    BlobRequestOptions options = new BlobRequestOptions() { EncryptionPolicy = policy };

    using (var np = File.Open(@"C:\data\MyFileDecrypted.txt", FileMode.Create))
        blob.DownloadToStream(np, null, options, null);


> [AZURE.NOTE] Er zijn een aantal andere soorten resolvers Sleutelbeheer om gemakkelijker te maken, met inbegrip van: AggregateKeyResolver en CachingKeyResolver.


## <a name="use-key-vault-secrets"></a>Gebruik de sleutel kluis geheimen
De manier is om een geheim met client-side-codering te gebruiken via de klasse SymmetricKey omdat het een geheim is in feite een symmetrische sleutel. Maar zoals eerder vermeld, een geheim in sleutel kluis niet is toegewezen aan een SymmetricKey precies. Er zijn een paar dingen te begrijpen:


- De sleutel in een SymmetricKey is een vaste lengte: 128, 192, 256, 384 en 512 bits.
- De sleutel in een SymmetricKey moet de Base64-gecodeerde.
- Een sleutel kluis geheim dat wordt gebruikt als een SymmetricKey moet een Content Type "application/octet-stream" in de sleutel kluis.

Hier volgt een voorbeeld in PowerShell voor het maken van een geheim in sleutel kluis die kan worden gebruikt als een SymmetricKey.
Opmerking: De vastgelegde waarde, $key, is alleen voor demonstratie. In uw eigen code wilt u deze sleutel genereren.

    // Here we are making a 128-bit key so we have 16 characters.
    //  The characters are in the ASCII range of UTF8 so they are
    //  each 1 byte. 16 x 8 = 128.
    $key = "qwertyuiopasdfgh"
    $b = [System.Text.Encoding]::UTF8.GetBytes($key)
    $enc = [System.Convert]::ToBase64String($b)
    $secretvalue = ConvertTo-SecureString $enc -AsPlainText -Force

    // Substitute the VaultName and Name in this command.
    $secret = Set-AzureKeyVaultSecret -VaultName 'ContoseKeyVault' -Name 'TestSecret2' -SecretValue $secretvalue -ContentType "application/octet-stream"

In de consoletoepassing, kunt u dezelfde aanroep als voordat dit geheim als een SymmetricKey ophalen.

    SymmetricKey sec = (SymmetricKey) cloudResolver.ResolveKeyAsync(
        "https://contosokeyvault.vault.azure.net/secrets/TestSecret2/",
        CancellationToken.None).GetAwaiter().GetResult();

Dat is. Veel plezier!

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het gebruik van Microsoft Azure opslag met C#, [Microsoft Azure opslag Client Library for .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).

Zie voor meer informatie over de API van de REST van de Blob [Blob Service REST API](https://msdn.microsoft.com/library/azure/dd135733.aspx).

Ga naar de [Microsoft Azure Storage Team Blog](http://blogs.msdn.com/b/windowsazurestorage/)voor de nieuwste informatie over Microsoft Azure opslag.


<!--Image references-->
[1]: ./media/storage-encrypt-decrypt-blobs-key-vault/blobmetadata.png
