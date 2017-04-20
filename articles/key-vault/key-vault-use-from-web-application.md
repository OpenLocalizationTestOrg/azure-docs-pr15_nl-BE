<properties
    pageTitle="Azure sleutel kluis uit een webtoepassing gebruiken | Microsoft Azure"
    description="Met deze zelfstudie kunt u informatie over het gebruik van Azure sleutel kluis uit een webtoepassing."
    services="key-vault"
    documentationCenter=""
    authors="adhurwit"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/05/2016"
    ms.author="adhurwit"/>

# <a name="use-azure-key-vault-from-a-web-application"></a>Azure sleutel kluis uit een webtoepassing gebruiken #

## <a name="introduction"></a>Inleiding  
Met deze zelfstudie kunt u informatie over het gebruik van Azure sleutel kluis uit een webtoepassing in Azure. Het helpt u bij het proces van het openen van een geheim uit een Azure sleutel kluis zodat deze kan worden gebruikt in uw webtoepassing.

**Geschatte tijd:** 15 minuten


Zie voor informatie over Azure sleutel kluis, [Wat is Azure sleutel kluis?](key-vault-whatis.md)

## <a name="prerequisites"></a>Vereisten

Deze zelfstudie hebt u het volgende:

- Een URI voor een geheim in een kluis Azure-sleutel
- Een Client-ID en een geheim van de Client voor een webtoepassing geregistreerd met Azure Active Directory die toegang tot uw sleutel kluis heeft
- Een webtoepassing. We tonen de stappen voor de toepassing van een ASP.NET-MVC geïmplementeerd in Azure als een Web App.

> [AZURE.NOTE]  Het is essentieel dat u de stappen in [Aan de slag met Azure sleutel kluis](key-vault-get-started.md) voor deze zelfstudie zodat u de URI een geheim, de Client-ID en het geheim van de Client voor een webtoepassing hebt voltooid.

De webtoepassing die toegang hebben tot de sleutel kluis is die is geregistreerd in Azure Active Directory en die toegang heeft gekregen tot uw sleutel kluis. Als dit niet het geval is, gaat u terug naar kassa een toepassing in de zelfstudie aan de slag en Herhaal de stappen vermeld.

Deze zelfstudie is ontworpen voor webontwikkelaars die de basisbeginselen van het maken van webtoepassingen op Azure. Zie voor meer informatie over Azure Web Apps, [Web Apps-overzicht](../app-service-web/app-service-web-overview.md).



## <a id="packages"></a>Nuget pakketten toevoegen ##
Er zijn twee pakketten die uw webtoepassing moet zijn geïnstalleerd.

- Active Directory-verificatie Library - bevat methoden voor interactie met Azure Active Directory en het beheren van gebruikers-id
- Azure sleutel kluis bibliotheek - bevat methoden voor het communiceren met Azure sleutel kluis


Beide van deze pakketten kunnen worden geïnstalleerd met de Package Manager-Console met de opdracht Install-Package.

    // this is currently the latest stable version of ADAL
    Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.16.204221202

    Install-Package Microsoft.Azure.KeyVault


## <a id="webconfig"></a>Web.Config wijzigen ##
Er zijn drie instellingen die moeten worden toegevoegd aan het bestand web.config als volgt.

    <!-- ClientId and ClientSecret refer to the web application registration with Azure Active Directory -->
    <add key="ClientId" value="clientid" />
    <add key="ClientSecret" value="clientsecret" />

    <!-- SecretUri is the URI for the secret in Azure Key Vault -->
    <add key="SecretUri" value="secreturi" />


Als u niet als host voor de toepassing als een Web App van Azure gaat, moet u de werkelijke waarden voor ClientId en geheim Client geheim URI toevoegen aan het bestand web.config. Laat deze dummy waarden anders omdat we de werkelijke waarden in Azure Portal voor een extra niveau van beveiliging toevoegen.


## <a id="gettoken"></a>Add, methode als u een Access Token ##
U moet een toegangstoken gebruikmaken van de API sleutel kluis. De sleutel kluis Client aanroepen naar de API sleutel kluis verwerkt, maar u hoeft te leveren met een functie die het toegangstoken opgehaald.  

Hieronder vindt u de code in een access token verkrijgen van Azure Active Directory. Deze code kan overal in uw toepassing. Ik wil graag een klasse Utils of EncryptionHelper toevoegen.  

    //add these using statements
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using System.Threading.Tasks;
    using System.Web.Configuration;

    //this is an optional property to hold the secret after it is retrieved
    public static string EncryptSecret { get; set; }

    //the method that will be provided to the KeyVaultClient
    public static async Task<string> GetToken(string authority, string resource, string scope)
    {
        var authContext = new AuthenticationContext(authority);
        ClientCredential clientCred = new ClientCredential(WebConfigurationManager.AppSettings["ClientId"],
                    WebConfigurationManager.AppSettings["ClientSecret"]);
        AuthenticationResult result = await authContext.AcquireTokenAsync(resource, clientCred);

        if (result == null)
            throw new InvalidOperationException("Failed to obtain the JWT token");

        return result.AccessToken;
    }

> [AZURE.NOTE] 
> Met behulp van een Client-ID en het geheim van de Client is de eenvoudigste manier om een toepassing AD Azure verifiëren. En deze in uw webtoepassing kunt voor een scheiding van rechten en meer controle over het beheer van sleutels. Maar het is afhankelijk van de op het geheim van de Client te plaatsen in uw configuratieinstellingen die voor sommige zo gevaarlijk als het plaatsen van het geheim dat u in de configuratie-instellingen wilt beveiligen. Zie hieronder voor een discussie over het gebruik van een Client-ID en een certificaat in plaats van de Client-ID en het geheim van de Client te verifiëren van de toepassing AD Azure.



## <a id="appstart"></a>Ophalen van het geheim op toepassing starten ##
Nu moeten we de code roept de API sleutel kluis op te halen van het geheim. De volgende code kan overal worden geplaatst, zolang deze wordt aangeroepen voordat u nodig hebt om. Ik hebben deze code in de gebeurtenis toepassing starten in het Global.asax plaatsen zodat er één keer uitgevoerd op start en het geheim voor de toepassing beschikbaar wordt.

    //add these using statements
    using Microsoft.Azure.KeyVault;
    using System.Web.Configuration;

    // I put my GetToken method in a Utils class. Change for wherever you placed your method.
    var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(Utils.GetToken));

    var sec = kv.GetSecretAsync(WebConfigurationManager.AppSettings["SecretUri"]).Result.Value;

    //I put a variable in a Utils class to hold the secret for general  application use.
    Utils.EncryptSecret = sec;



## <a id="portalsettings"></a>App instellingen toevoegen in de Portal Azure (optioneel) ##
Als u een Azure Web App kunt u de werkelijke waarden voor de AppSettings in Azure Portal toevoegen. Op deze manier de werkelijke waarden worden niet in het bestand web.config, maar via het Portal waar u afzonderlijke-besturingsmogelijkheden hebt beveiligd. Deze waarden zullen worden vervangen door de waarden die u hebt opgegeven in uw web.config. Zorg ervoor dat de namen identiek zijn.

![Instellingen van de toepassing weergegeven in Azure Portal][1]


## <a name="authenticate-with-a-certificate-instead-of-a-client-secret"></a>Met een certificaat in plaats van een geheim van de Client worden geverifieerd.
Een andere manier voor het verifiëren van een toepassing AD Azure is via een Client-ID en een certificaat in plaats van een Client-ID en het geheim van de Client. Hier volgen de stappen voor het gebruik van een certificaat in een Azure Web App:

1. Ophalen of maken van een certificaat
2. Het certificaat koppelen aan een toepassing AD Azure
3. Voeg code toe aan uw Web App het certificaat gebruiken
4. Een certificaat toevoegen aan uw Web App


**Ophalen of maken van een certificaat** Voor ons doel maken we een testcertificaat. Hier zijn een aantal opdrachten die u in Developer vanaf de opdrachtprompt kunt gebruiken om een certificaat te maken. Wijzig de map aan waar u de cert-bestanden moet worden gemaakt.

    makecert -sv mykey.pvk -n "cn=KVWebApp" KVWebApp.cer -b 07/31/2015 -e 07/31/2016 -r
    pvk2pfx -pvk mykey.pvk -spc KVWebApp.cer -pfx KVWebApp.pfx -po test123

Noteer de einddatum en het wachtwoord voor de .pfx (in dit voorbeeld: 31-07/2016 en test123). U moet deze hieronder.

Zie voor meer informatie over het maken van een testcertificaat [hoe: Maak je eigen Test certificaat](https://msdn.microsoft.com/library/ff699202.aspx)


**Het certificaat met een toepassing Azure AD koppelen** Nu u een certificaat hebt, moet u koppelen aan een toepassing AD Azure. Maar de Azure Management Portal biedt geen ondersteuning voor dit nu. U moet in plaats daarvan gebruikt u Powershell. Hieronder staan de opdrachten die u nodig hebt om uit te voeren:

    $x509 = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2

    PS C:\> $x509.Import("C:\data\KVWebApp.cer")

    PS C:\> $credValue = [System.Convert]::ToBase64String($x509.GetRawCertData())

    PS C:\> $now = [System.DateTime]::Now

    # this is where the end date from the cert above is used
    PS C:\> $yearfromnow = [System.DateTime]::Parse("2016-07-31")

    PS C:\> $adapp = New-AzureRmADApplication -DisplayName "KVWebApp" -HomePage "http://kvwebapp" -IdentifierUris "http://kvwebapp" -KeyValue $credValue -KeyType "AsymmetricX509Cert" -KeyUsage "Verify" -StartDate $now -EndDate $yearfromnow

    PS C:\> $sp = New-AzureRmADServicePrincipal -ApplicationId $adapp.ApplicationId

    PS C:\> Set-AzureRmKeyVaultAccessPolicy -VaultName 'contosokv' -ServicePrincipalName $sp.ServicePrincipalName -PermissionsToSecrets all -ResourceGroupName 'contosorg'

    # get the thumbprint to use in your app settings
    PS C:\>$x509.Thumbprint

Nadat u deze opdrachten hebt uitgevoerd, kunt u de toepassing in Azure AD bekijken. Als u de toepassing op de eerste, zoeken naar 'Toepassingen mijn bedrijf eigenaar is van' niet ziet in plaats van "Toepassingen gebruikt voor mijn bedrijf".

Zie voor meer informatie over Azure AD toepassing en ServicePrincipal-objecten, [toepassing en Service Principal-objecten](../active-directory/active-directory-application-objects.md)



**Code toevoegen aan uw Web App het certificaat gebruiken** Nu zullen we code toevoegen aan uw Web App voor toegang tot het certificaat en gebruiken voor verificatie.

Er is eerst code toegang krijgen tot het certificaat.

    public static class CertificateHelper
    {
        public static X509Certificate2 FindCertificateByThumbprint(string findValue)
        {
            X509Store store = new X509Store(StoreName.My, StoreLocation.CurrentUser);
            try
            {
                store.Open(OpenFlags.ReadOnly);
                X509Certificate2Collection col = store.Certificates.Find(X509FindType.FindByThumbprint,
                    findValue, false); // Don't validate certs, since the test root isn't installed.
                if (col == null || col.Count == 0)
                    return null;
                return col[0];
            }
            finally
            {
                store.Close();
            }
        }
    }


Let erop dat de StoreLocation CurrentUser in plaats van LocalMachine. En dat we 'false' voor de Find-methode levert omdat we een test-certificaat.


Vervolgens wordt code die gebruikmaakt van de CertificateHelper en maakt een ClientAssertionCertificate dat nodig voor verificatie is.

    public static ClientAssertionCertificate AssertionCert { get; set; }

    public static void GetCert()
    {
        var clientAssertionCertPfx = CertificateHelper.FindCertificateByThumbprint(WebConfigurationManager.AppSettings["thumbprint"]);
        AssertionCert = new ClientAssertionCertificate(WebConfigurationManager.AppSettings["clientid"], clientAssertionCertPfx);
    }


Dit is de nieuwe code voor het toegangstoken. Deze optie vervangt de GetToken van de bovenstaande methoden. Ik te kennen gegeven dat een andere naam voor het gemak.

    public static async Task<string> GetAccessToken(string authority, string resource, string scope)
    {
        var context = new AuthenticationContext(authority, TokenCache.DefaultShared);
        var result = await context.AcquireTokenAsync(resource, AssertionCert);
        return result.AccessToken;
    }

Ik hebben alle van deze code in Utils-klasse van mijn project Web App voor gebruiksgemak gebracht.

De laatste codewijziging wordt de methode Application_Start. Eerst moet roept de methode GetCert() om de ClientAssertionCertificate te laden. En we de callback-methode die we leveren bij het maken van een nieuwe KeyVaultClient wijzigt. Houd er rekening mee dat deze optie vervangt u de code die we hierboven hebben gehad.

    Utils.GetCert();
    var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(Utils.GetAccessToken));


**Een certificaat toevoegen aan uw Web-App via de Portal Azure** Een certificaat toevoegen aan uw Web App is een eenvoudig proces. Eerst gaat u naar de Portal Azure en Ga naar uw Web App. Klik op het blad van de instellingen voor uw Web App, op de vermelding voor "Custom domains en SSL". Op het blad dat u geopend kunnen worden voor het uploaden van het certificaat dat u hierboven KVWebApp.pfx hebt gemaakt, zorg ervoor dat u niet het wachtwoord voor de pfx vergeet.

![Een certificaat toevoegen aan een Web App in Azure Portal][2]


Het laatste wat u hoeft te doen is een toepassingsinstelling toevoegen aan uw Web App met de naam WEBSITE\_laden\_certificaten en de waarde *. Hierdoor weet u zeker dat alle certificaten die worden geladen. Als u laden, alleen de certificaten die u hebt geüpload wilt, kunt u een door komma's gescheiden lijst van hun vingerafdrukken invoeren.

Zie voor meer informatie over het toevoegen van een certificaat aan een Web App, [Met behulp van certificaten in Azure Websites toepassingen](https://azure.microsoft.com/blog/2014/10/27/using-certificates-in-azure-websites-applications/)


**Een certificaat aan de kluis als een geheime sleutel toevoegen** U kunt in plaats van rechtstreeks uploaden van uw certificaat naar de service Web App, opslaan in een sleutel kluis als een geheim en van daaruit te implementeren. Dit is een proces dat wordt beschreven in de volgende blogbericht [Azure Web App certificaat implementeren via de sleutel kluis](https://blogs.msdn.microsoft.com/appserviceteam/2016/05/24/deploying-azure-web-app-certificate-through-key-vault/)



## <a id="next"></a>Volgende stappen ##


Zie voor het programmeren van verwijzingen, [Azure sleutel kluis C# Client API Reference](https://msdn.microsoft.com/library/azure/dn903628.aspx).


<!--Image references-->
[1]: ./media/key-vault-use-from-web-application/PortalAppSettings.png
[2]: ./media/key-vault-use-from-web-application/PortalAddCertificate.png
