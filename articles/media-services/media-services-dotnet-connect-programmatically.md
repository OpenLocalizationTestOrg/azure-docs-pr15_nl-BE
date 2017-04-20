<properties 
    pageTitle="Verbinding maken met Media Services-Account met behulp van .NET" 
    description="In dit onderwerp wordt beschreven hoe verbinding met Media Services uisng .NET." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
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


# <a name="connecting-to-media-services-account-using-media-services-sdk-for-net"></a>Verbinding maken met Media Services-Account met behulp van Media Services SDK voor .NET

> [AZURE.SELECTOR]
- [REST](media-services-rest-connect-programmatically.md)
- [.NET](media-services-dotnet-connect-programmatically.md)


In dit onderwerp wordt beschreven hoe u een programma verbinding met Microsoft Azure Media Services ophalen tijdens het programmeren met de Media Services SDK voor .NET.


## <a name="connecting-to-media-services"></a>Verbinding maken met mediaservices

Verbinding van Media Services via programmering, moet u hebben eerder een Azure account instellen, Media Services geconfigureerd op die account en stel een Visual Studio-project voor de ontwikkeling van met de Media Services SDK voor .NET. Zie instellingen voor ontwikkeling met de Media Services SDK voor .NET voor meer informatie.

Aan het einde van de installatieprocedure van Media Services-account, moet u de volgende waarden voor de vereiste verbinding verkregen. Gebruik dit programma om verbindingen te maken met Media Services.

- De naam van uw Media Services.

- De sleutel van Media Services-account.

Om te zoeken naar deze waarden, gaat u naar de Portal Azure Management, selecteer uw Media Service-account en klik op het pictogram '**Sleutels beheren**' aan de onderkant van het venster van de portal. De waarde te klikken op het pictogram naast elk tekstvak worden gekopieerd naar het systeemklembord.


## <a name="creating-a-cloudmediacontext-instance"></a>Het maken van een exemplaar van de CloudMediaContext

Om te beginnen met programmeren tegen Media Services moet u een **CloudMediaContext** exemplaar met de servercontext te maken. De **CloudMediaContext** bevat verwijzingen naar belangrijke collecties, met inbegrip van activa, taken, bestanden, -beleid en locators.

>[AZURE.NOTE] De klasse **CloudMediaContext** is niet thread-safe. Maak een nieuwe CloudMediaContext per thread of per set bewerkingen.


CloudMediaContext heeft vijf constructor overbelastingen. Het verdient aanbeveling gebruik constructors die **MediaServicesCredentials** als parameter. Zie voor meer informatie het **Opnieuw gebruiken van Access Control Service Tokens** die volgt. 

In het volgende voorbeeld gebruikt de constructor van de openbare CloudMediaContext(MediaServicesCredentials credentials):

    // _cachedCredentials and _context are class member variables. 
    _cachedCredentials = new MediaServicesCredentials(
                    _mediaServicesAccountName,
                    _mediaServicesAccountKey);
    
    _context = new CloudMediaContext(_cachedCredentials);


## <a name="reusing-access-control-service-tokens"></a>Toegangstokens Control-Service opnieuw gebruiken

In dit gedeelte ziet u hoe u Access Control-Service tokens gebruiken met behulp van CloudMediaContext-constructors die MediaServicesCredentials als parameter.


[Azure Active Directory-toegangsbeheer](https://msdn.microsoft.com/library/hh147631.aspx) (ook bekend als Access Control-Service of ACS) is een cloud-service een eenvoudige manier biedt voor verificatie en machtiging van gebruikers toegang krijgen tot hun webtoepassingen. Microsoft Azure Media Services beheert de toegang tot haar diensten al OAuth-protocol waarmee een ACS-token is vereist. De ACS-tokens ontvangt Media Services van een vergunning server.

Wanneer u met de Media Services SDK ontwikkelt, kunt u niet omgaan met de tokens omdat de SDK code managers ze voor u. Echter, zodat het volledig beheer van de ACS-tokens SDK leidt tot onnodige token aanvragen. Aanvragen van tokens kost tijd en systeembronnen client en server. De ACS-server bandbreedte ook de aanvragen als de frequentie te hoog is. De limiet is 30-aanvragen per seconde, [Beperkingen van ACS-Service](https://msdn.microsoft.com/library/gg185909.aspx) Zie voor meer informatie.

Beginnen met de Media Services SDK 3.0.0.0-prestatiemeters versie, kunt u de ACS-tokens gebruiken. De constructors **CloudMediaContext** die **MediaServicesCredentials** als parameter kunnen delen tussen verschillende contexten van de ACS-tokens. De klasse MediaServicesCredentials kapselt de referenties Media Services. Indien een ACS-token beschikbaar is en de verlooptijd bekend is, kunt u een nieuw exemplaar van de MediaServicesCredentials maken met het token en doorgegeven aan de constructor van de CloudMediaContext. Houd er rekening mee dat de Media Services SDK tokens automatisch vernieuwd wanneer ze verlopen. Er zijn twee manieren om ACS-tokens, opnieuw te gebruiken zoals in de onderstaande voorbeelden.

- U kunt het **MediaServicesCredentials** -object in het geheugen (in bijvoorbeeld een met statische klassenvariabele) cache. De cache object vervolgens doorgeven aan de constructor CloudMediaContext. Het object MediaServicesCredentials bevat een ACS-token die opnieuw kan worden gebruikt als nog steeds geldig is. Als het token niet geldig is, wordt deze vernieuwd door de Media Services SDK met de referenties die zijn toegekend aan de constructor MediaServicesCredentials.

    Houd er rekening mee dat het **MediaServicesCredentials** -object een geldige token wordt na de RefreshToken wordt genoemd. De **CloudMediaContext** roept de methode **RefreshToken** in de constructor. Als u van plan bent de token waarden opslaan in een externe opslag controleren om te controleren of de waarde TokenExpiration geldig is voor het opslaan van het token. Niet geldig is, dan belt u RefreshToken voordat het in de cache opslaan.

        // Create and cache the Media Services credentials in a static class variable.
        _cachedCredentials = new MediaServicesCredentials(_mediaServicesAccountName, _mediaServicesAccountKey);

        
        // Use the cached credentials to create a new CloudMediaContext object.
        if(_cachedCredentials == null)
        {
            _cachedCredentials = new MediaServicesCredentials(_mediaServicesAccountName, _mediaServicesAccountKey);
        }
        
        CloudMediaContext context = new CloudMediaContext(_cachedCredentials);

- U kunt ook de tekenreeks AccessToken en de waarden TokenExpiration cache. Het kan later worden gebruikt voor het maken van een nieuw MediaServicesCredentials-object met de gegevens in de cache token.  Dit is vooral handig voor scenario's waarin het token kan veilig kan worden gedeeld door meerdere processen of computers.

    De volgende codefragmenten de SaveTokenDataToExternalStorage, GetTokenDataFromExternalStorage en UpdateTokenDataInExternalStorageIfNeeded methoden aanroepen die niet zijn gedefinieerd in het volgende voorbeeld. Deze methoden voor het opslaan, ophalen en bijwerken van token gegevens in een externe opslag kunt u definiëren. 

        CloudMediaContext context1 = new CloudMediaContext(_mediaServicesAccountName, _mediaServicesAccountKey);
        
        // Get token values from the context.
        var accessToken = context1.Credentials.AccessToken;
        var tokenExpiration = context1.Credentials.TokenExpiration;
        
        // Save token values for later use. 
        // The SaveTokenDataToExternalStorage method should check 
        // whether the TokenExpiration value is valid before saving the token data. 
        // If it is not valid, call MediaServicesCredentials’s RefreshToken before caching.
        SaveTokenDataToExternalStorage(accessToken, tokenExpiration);
        
    De opgeslagen waarden token gebruik te maken van MediaServicesCredentials.


        var accessToken = "";
        var tokenExpiration = DateTime.UtcNow;
        
        // Retrieve saved token values.
        GetTokenDataFromExternalStorage(out accessToken, out tokenExpiration);
        
        // Create a new MediaServicesCredentials object using saved token values.
        MediaServicesCredentials credentials = new MediaServicesCredentials(_mediaServicesAccountName, _mediaServicesAccountKey)
        {
            AccessToken = accessToken,
            TokenExpiration = tokenExpiration
        };
        
        CloudMediaContext context2 = new CloudMediaContext(credentials);

    Het token exemplaar bijwerken in het geval het token is bijgewerkt door de Media Services SDK. 
    
        if(tokenExpiration != context2.Credentials.TokenExpiration)
        {
            UpdateTokenDataInExternalStorageIfNeeded(accessToken, context2.Credentials.TokenExpiration);
        }
        

- Als u meerdere accounts voor Media Services (bijvoorbeeld voor belastingverdeling doeleinden of Geo-verdeling) kunt u met behulp van de System.Collections.Concurrent.ConcurrentDictionary-collectie (de ConcurrentDictionary-collectie vertegenwoordigt een collectie thread-veilige sleutel/waarde-paren die kunnen worden benaderd door meerdere threads gelijktijdig) MediaServicesCredentials-objecten in een cache. Vervolgens kunt u de methode GetOrAdd om de referenties in de cache. 

        // Declare a static class variable of the ConcurrentDictionary type in which the Media Services credentials will be cached.  
        private static readonly ConcurrentDictionary<string, MediaServicesCredentials> mediaServicesCredentialsCache = 
            new ConcurrentDictionary<string, MediaServicesCredentials>();
        

        // Cache (or get already cached) Media Services credentials. Use these credentials to create a new CloudMediaContext object.
        static public CloudMediaContext CreateMediaServicesContext(string accountName, string accountKey)
        {
            CloudMediaContext cloudMediaContext;
            MediaServicesCredentials mediaServicesCredentials;
        
            mediaServicesCredentials = mediaServicesCredentialsCache.GetOrAdd(
                accountName,
                valueFactory => new MediaServicesCredentials(accountName, accountKey));
        
            cloudMediaContext = new CloudMediaContext(mediaServicesCredentials);
        
            return cloudMediaContext;
        }
        
## <a name="connecting-to-a-media-services-account-located-in-the-north-china-region"></a>Verbinding maken met een Media-Services-account zich bevindt in de regio Noord-China

Als uw account zich bevindt in de regio Noord-China, de volgende constructor gebruiken:

    public CloudMediaContext(Uri apiServer, string accountName, string accountKey, string scope, string acsBaseAddress)

Bijvoorbeeld:


    _context = new CloudMediaContext(
        new Uri("https://wamsbjbclus001rest-hs.chinacloudapp.cn/API/"),
        _mediaServicesAccountName,
        _mediaServicesAccountKey,
        "urn:WindowsAzureMediaServices",
        "https://wamsprodglobal001acs.accesscontrol.chinacloudapi.cn");


## <a name="storing-connection-values-in-configuration"></a>Verbinding-waarden in de configuratie opslaan

Het is ten zeerste aanbevolen verbinding waarden, met name gevoelige waarden, zoals uw accountnaam en wachtwoord opslaan in de configuratie. Het is ook aanbevolen voor het coderen van gevoelige gegevens. U kunt de gehele configuratiebestand coderen met behulp van het Windows Encrypting File System (EFS). Voor het inschakelen van EFS op een bestand, met de rechtermuisknop op het bestand en selecteer **Eigenschappen**in het instellingen-tabblad **Geavanceerd** -codering inschakelen. Of u kunt een aangepaste oplossing voor het coderen van geselecteerde gedeelten van een configuratiebestand met de beveiligde configuratie maken. Zie [codering configuratie informatie met behulp van beveiligde configuratie](https://msdn.microsoft.com/library/53tyfkaw.aspx).

De volgende App.config bestand bevat de waarden voor de vereiste verbinding. De waarden in de <appSettings> element zijn de vereiste waarden die u hebt gekregen van Media Services gestart.

    <configuration>
      <appSettings>
        <add key="MediaServicesAccountName" value="Media-Services-Account-Name" />
        <add key="MediaServicesAccountKey" value="Media-Services-Account-Key" />
      </appSettings>
    </configuration>


U kunt verbinding om waarden te vinden uit de configuratie, gebruikt u de klasse **ConfigurationManager** en vervolgens de waarden toewijzen aan velden in uw code:
    
    private static readonly string _accountName = ConfigurationManager.AppSettings["MediaServicesAccountName"];
    private static readonly string _accountKey = ConfigurationManager.AppSettings["MediaServicesAccountKey"];



##<a name="media-services-learning-paths"></a>Leerfasen Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
