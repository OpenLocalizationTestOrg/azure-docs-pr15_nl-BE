<properties
    pageTitle="Aan de slag met de bibliotheek Azure CDN voor .NET | Microsoft Azure"
    description="Informatie over het schrijven van toepassingen voor het beheren van Azure CDN met behulp van Visual Studio .NET."
    services="cdn"
    documentationCenter=".net"
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/15/2016"
    ms.author="casoper"/>

# <a name="get-started-with-azure-cdn-development"></a>Aan de slag met Azure CDN-ontwikkeling

> [AZURE.SELECTOR]
- [Node.js](cdn-app-dev-node.md)
- [.NET](cdn-app-dev-net.md)

U kunt de [Azure CDN bibliotheek voor .NET](https://msdn.microsoft.com/library/mt657769.aspx) maken en beheren van CDN profielen en eindpunten te automatiseren.  Deze handleiding helpt bij het maken van een eenvoudige .NET consoletoepassing die meer van de beschikbare bewerkingen toont.  Deze zelfstudie is niet bedoeld om alle aspecten van de bibliotheek Azure CDN voor .NET beschrijven in detail.

Moet u Visual Studio 2015 voor het voltooien van deze zelfstudie.  [Visual Studio Community 2015](https://www.visualstudio.com/products/visual-studio-community-vs.aspx) is gratis te downloaden.

> [AZURE.TIP] Het [project van deze zelfstudie voltooid](https://code.msdn.microsoft.com/Azure-CDN-Management-1f2fba2c) is beschikbaar voor download op MSDN.

[AZURE.INCLUDE [cdn-app-dev-prep](../../includes/cdn-app-dev-prep.md)]

## <a name="create-your-project-and-add-nuget-packages"></a>Het project maken en Nuget pakketten toevoegen

Nu dat we hebt gemaakt van een resourcegroep voor onze CDN-profielen en toestemming van onze Azure AD toepassing voor het beheren van CDN profielen en eindpunten binnen die groep, kan begin onze toepassing maken.

Klik in Visual Studio 2015, op **bestand**, **Nieuw** **Project...** om het dialoogvenster new project openen.  Vouw **Visual C#**, en selecteer vervolgens **Windows** in het deelvenster aan de linkerkant.  Klik op **Consoletoepassing** in het middelste deelvenster.  Naam van het project en klik vervolgens op **OK**.  

![Nieuw Project](./media/cdn-app-dev-net/cdn-new-project.png)

Ons project zal sommige Azure bibliotheken opgenomen in Nuget pakketten gebruiken.  Laten we deze toevoegen aan het project.

1. Klik op het menu **Extra** , **Nuget Package Manager**en vervolgens **Package Manager-Console**.

    ![Nuget pakketten beheren](./media/cdn-app-dev-net/cdn-manage-nuget.png)

2. In de Package Manager-Console worden uitgevoerd met de volgende opdracht in de **Active Directory verificatie bibliotheek (ADAL)**installeren:

    `Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory`

3. De volgende om te installeren van de **Bibliotheek van Azure CDN Management**worden uitgevoerd:

    `Install-Package Microsoft.Azure.Management.Cdn`

## <a name="directives-constants-main-method-and-helper-methods"></a>Richtlijnen, constanten hoofdmethode en methoden

We krijgen de basisstructuur van ons programma geschreven.

1. In het tabblad Program.cs vervangen de `using` richtlijnen aan de bovenkant met het volgende:

    ```csharp
    using System;
    using System.Collections.Generic;
    using Microsoft.Azure.Management.Cdn;
    using Microsoft.Azure.Management.Cdn.Models;
    using Microsoft.Azure.Management.Resources;
    using Microsoft.Azure.Management.Resources.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Microsoft.Rest;
    ```

2. We moeten sommige constanten onze methoden gebruiken definiëren.  In de `Program` klasse, maar voordat de `Main` methode, Voeg het volgende toe.  U in plaats van tijdelijke aanduidingen, met inbegrip van de ** &lt;punthaken&gt;**, met uw eigen waarden.

    ```csharp
    //Tenant app constants
    private const string clientID = "<YOUR CLIENT ID>";
    private const string clientSecret = "<YOUR CLIENT AUTHENTICATION KEY>"; //Only for service principals
    private const string authority = "https://login.microsoftonline.com/<YOUR TENANT ID>/<YOUR TENANT DOMAIN NAME>";

    //Application constants
    private const string subscriptionId = "<YOUR SUBSCRIPTION ID>";
    private const string profileName = "CdnConsoleApp";
    private const string endpointName = "<A UNIQUE NAME FOR YOUR CDN ENDPOINT>";
    private const string resourceGroupName = "CdnConsoleTutorial";
    private const string resourceLocation = "<YOUR PREFERRED AZURE LOCATION, SUCH AS Central US>";
    ```

3. Ook op het klasseniveau van deze twee variabelen definiëren.  We gebruiken deze later om te bepalen als ons profiel- en eindpunt al bestaan.

    ```csharp
    static bool profileAlreadyExists = false;
    static bool endpointAlreadyExists = false;
    ```

4.  Vervang de `Main` methode als volgt:

    ```csharp
    static void Main(string[] args)
    {
        //Get a token
        AuthenticationResult authResult = GetAccessToken();

        // Create CDN client
        CdnManagementClient cdn = new CdnManagementClient(new TokenCredentials(authResult.AccessToken))
            { SubscriptionId = subscriptionId };

        ListProfilesAndEndpoints(cdn);

        // Create CDN Profile
        CreateCdnProfile(cdn);

        // Create CDN Endpoint
        CreateCdnEndpoint(cdn);
        
        Console.WriteLine();

        // Purge CDN Endpoint
        PromptPurgeCdnEndpoint(cdn);

        // Delete CDN Endpoint
        PromptDeleteCdnEndpoint(cdn);

        // Delete CDN Profile
        PromptDeleteCdnProfile(cdn);

        Console.WriteLine("Press Enter to end program.");
        Console.ReadLine();
    }
    ```

5. Enkele van onze andere methoden gaat de gebruiker gevraagd met ' Ja/Nee' vragen.  De volgende methode om gemakkelijker die iets toevoegen:

    ```csharp
    private static bool PromptUser(string Question)
    {
        Console.Write(Question + " (Y/N): ");
        var response = Console.ReadKey();
        Console.WriteLine();
        if (response.Key == ConsoleKey.Y)
        {
            return true;
        }
        else if (response.Key == ConsoleKey.N)
        {
            return false;
        }
        else
        {
            // They pressed something other than Y or N.  Let's ask them again.
            return PromptUser(Question);
        }
    }
    ```

Nu dat de basisstructuur van het programma is geschreven, maken we de methoden aangeroepen door de `Main` methode.

## <a name="authentication"></a>Verificatie

Voordat we de Azure CDN Management bibliotheek gebruiken kunt, moeten we onze service principal verifiëren en een verificatietoken verkrijgen.  Deze methode wordt een ADAL aan het token worden opgehaald.

```csharp
private static AuthenticationResult GetAccessToken()
{
    AuthenticationContext authContext = new AuthenticationContext(authority); 
    ClientCredential credential = new ClientCredential(clientID, clientSecret);
    AuthenticationResult authResult = 
        authContext.AcquireTokenAsync("https://management.core.windows.net/", credential).Result;

    return authResult;
}
```

Als u van verificatie van de gebruiker gebruikmaakt, de `GetAccessToken` methode er enigszins anders.

>[AZURE.IMPORTANT] In deze voorbeeldcode alleen gebruiken als u ervoor kiest om afzonderlijke gebruikersverificatie in plaats van een service principal.

```csharp
private static AuthenticationResult GetAccessToken()
{
    AuthenticationContext authContext = new AuthenticationContext(authority);
    AuthenticationResult authResult = authContext.AcquireTokenAsync("https://management.core.windows.net/",
        clientID, new Uri("http://<redirect URI>"), new PlatformParameters(PromptBehavior.RefreshSession)).Result;

    return authResult;
}
```

Zorg ervoor dat u vervangen `<redirect URI>` met het omleiden van URI die u hebt ingevoerd toen u de toepassing geregistreerd in Azure AD.

## <a name="list-cdn-profiles-and-endpoints"></a>Lijst CDN profielen en eindpunten

Nu we gaan CDN-bewerkingen uit te voeren.  Het eerste wat u doet onze methode lijst alle profielen en eindpunten in de resourcegroep en als er een overeenkomst wordt gevonden voor de profiel- en eindpunt namen opgegeven in onze constanten, maakt de aantekening dat later zodat we niet proberen te maken van duplicaten.

```csharp
private static void ListProfilesAndEndpoints(CdnManagementClient cdn)
{
    // List all the CDN profiles in this resource group
    var profileList = cdn.Profiles.ListByResourceGroup(resourceGroupName);
    foreach (Profile p in profileList)
    {
        Console.WriteLine("CDN profile {0}", p.Name);
        if (p.Name.Equals(profileName, StringComparison.OrdinalIgnoreCase))
        {
            // Hey, that's the name of the CDN profile we want to create!
            profileAlreadyExists = true;
        }

        //List all the CDN endpoints on this CDN profile
        Console.WriteLine("Endpoints:");
        var endpointList = cdn.Endpoints.ListByProfile(p.Name, resourceGroupName);
        foreach (Endpoint e in endpointList)
        {
            Console.WriteLine("-{0} ({1})", e.Name, e.HostName);
            if (e.Name.Equals(endpointName, StringComparison.OrdinalIgnoreCase))
            {
                // The unique endpoint name already exists.
                endpointAlreadyExists = true;
            }
        }
        Console.WriteLine();
    }
}
```

## <a name="create-cdn-profiles-and-endpoints"></a>CDN profielen en eindpunten maken

Vervolgens maken we een profiel.

```csharp
private static void CreateCdnProfile(CdnManagementClient cdn)
{
    if (profileAlreadyExists)
    {
        Console.WriteLine("Profile {0} already exists.", profileName);
    }
    else
    {
        Console.WriteLine("Creating profile {0}.", profileName);
        ProfileCreateParameters profileParms =
            new ProfileCreateParameters() { Location = resourceLocation, Sku = new Sku(SkuName.StandardVerizon) };
        cdn.Profiles.Create(profileName, profileParms, resourceGroupName);
    }
}
```

Als het profiel is gemaakt, gaan we een eindpunt te maken.

```csharp
private static void CreateCdnEndpoint(CdnManagementClient cdn)
{
    if (endpointAlreadyExists)
    {
        Console.WriteLine("Profile {0} already exists.", profileName);
    }
    else
    {
        Console.WriteLine("Creating endpoint {0} on profile {1}.", endpointName, profileName);
        EndpointCreateParameters endpointParms =
            new EndpointCreateParameters()
            {
                Origins = new List<DeepCreatedOrigin>() { new DeepCreatedOrigin("Contoso", "www.contoso.com") },
                IsHttpAllowed = true,
                IsHttpsAllowed = true,
                Location = resourceLocation
            };
        cdn.Endpoints.Create(endpointName, endpointParms, profileName, resourceGroupName);
    }
}
```

>[AZURE.NOTE] In het bovenstaande voorbeeld wijst het eindpunt een oorsprong met een host-naam met de naam *Contoso* `www.contoso.com`.  U moet dit naar de hostnaam van uw eigen oorsprong.

## <a name="purge-an-endpoint"></a>Een eindpunt verwijderen

Ervan uitgaande dat het eindpunt is gemaakt, is een gemeenschappelijke taak die wij in ons programma uit te voeren kunt de inhoud van onze eindpunt verwijderen.

```csharp
private static void PromptPurgeCdnEndpoint(CdnManagementClient cdn)
{
    if (PromptUser(String.Format("Purge CDN endpoint {0}?", endpointName)))
    {
        Console.WriteLine("Purging endpoint. Please wait...");
        cdn.Endpoints.PurgeContent(endpointName, profileName, resourceGroupName, new List<string>() { "/*" });
        Console.WriteLine("Done.");
        Console.WriteLine();
    }
}
```

>[AZURE.NOTE] In het bovenstaande voorbeeld wordt de tekenreeks `/*` geeft aan dat ik wil alles in de hoofdmap van het eindpunt pad verwijderen.  Dit komt overeen met **Alle leegmaken** controleren in het dialoogvenster voor de Azure portal "verwijderen". In de `CreateCdnProfile` -methode ik ons profiel gemaakt als een **Azure CDN van Verizon** -profiel met de code `Sku = new Sku(SkuName.StandardVerizon)`, zodat deze geverifieerd worden kan.  Echter **Azure CDN van Akamai** profielen ondersteunen niet **Alle leegmaken**, zodat als ik een profiel Akamai voor deze zelfstudie gebruikte, ik bepaalde paden moet te verwijderen.

## <a name="delete-cdn-profiles-and-endpoints"></a>CDN profielen en eindpunten verwijderen

De laatste methoden worden onze eindpunt en profiel verwijderd.

```csharp
private static void PromptDeleteCdnEndpoint(CdnManagementClient cdn)
{
    if(PromptUser(String.Format("Delete CDN endpoint {0} on profile {1}?", endpointName, profileName)))
    {
        Console.WriteLine("Deleting endpoint. Please wait...");
        cdn.Endpoints.DeleteIfExists(endpointName, profileName, resourceGroupName);
        Console.WriteLine("Done.");
        Console.WriteLine();
    }
}

private static void PromptDeleteCdnProfile(CdnManagementClient cdn)
{
    if(PromptUser(String.Format("Delete CDN profile {0}?", profileName)))
    {
        Console.WriteLine("Deleting profile. Please wait...");
        cdn.Profiles.DeleteIfExists(profileName, resourceGroupName);
        Console.WriteLine("Done.");
        Console.WriteLine();
    }
}
```

## <a name="running-the-program"></a>Het programma wordt uitgevoerd

We kunnen nu compileren en uitvoeren van het programma door te klikken op de knop **Start** in Visual Studio.

![Programma wordt uitgevoerd](./media/cdn-app-dev-net/cdn-program-running-1.png)

Als het programma bij de bovenstaande vragen, moet u mogelijk zijn terug naar de resourcegroep in de portal Azure en Zie dat het profiel is gemaakt.

![Succes!](./media/cdn-app-dev-net/cdn-success.png)

We vervolgens de aanwijzingen voor het uitvoeren van de rest van het programma te bevestigen.

![Programma uitvoeren](./media/cdn-app-dev-net/cdn-program-running-2.png)

## <a name="next-steps"></a>Volgende stappen

Om te zien het voltooide project van deze procedure, [downloaden van het monster](https://code.msdn.microsoft.com/Azure-CDN-Management-1f2fba2c).

Ga voor aanvullende documentatie van de Azure CDN Management Library for .NET weergeven de [reference op MSDN](https://msdn.microsoft.com/library/mt657769.aspx).

Beheer uw resources CDN met [PowerShell](./cdn-manage-powershell.md).


