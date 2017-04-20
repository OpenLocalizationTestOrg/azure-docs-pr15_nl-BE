<properties
   pageTitle="Data Lake winkel .NET SDK gebruiken om toepassingen te ontwikkelen | Microsoft Azure"
   description="Azure Data Lake winkel .NET SDK gebruiken om toepassingen te ontwikkelen"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/27/2016"
   ms.author="nitinme"/>

# <a name="get-started-with-azure-data-lake-store-using-net-sdk"></a>Aan de slag met Azure Lake gegevensarchief maken met .NET SDK

> [AZURE.SELECTOR]
- [Portal](data-lake-store-get-started-portal.md)
- [PowerShell](data-lake-store-get-started-powershell.md)
- [.NET SDK](data-lake-store-get-started-net-sdk.md)
- [Java SDK](data-lake-store-get-started-java-sdk.md)
- [REST-API](data-lake-store-get-started-rest-api.md)
- [Azure CLI](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)

Informatie over de [Azure Data Lake winkel .NET SDK](https://msdn.microsoft.com/library/mt581387.aspx) gebruiken voor het uitvoeren van eenvoudige bewerkingen zoals zoals mappen maakt, uploaden en downloaden van bestanden, enz. Zie voor meer informatie over gegevens Lake [Azure Lake gegevensarchief](data-lake-store-overview.md).

## <a name="prerequisites"></a>Vereisten

* **Visual Studio 2013 of 2015**. De volgende instructies gebruiken Visual Studio 2015.

* **Azure een abonnement**. Zie [Azure krijg gratis proefperiode](https://azure.microsoft.com/pricing/free-trial/).

* **Azure Lake gegevensarchief account**. Zie voor instructies voor het maken van een account [aan de slag met Azure Lake gegevensarchief](data-lake-store-get-started-portal.md)

* **Een Azure Active Directory-toepassing maken**. De Azure AD-toepassing kunt u de toepassing van het gegevensarchief Lake met Azure Active Directory te verifiëren. Er zijn verschillende benaderingen voor de verificatie van AD Azure, **eindgebruikers** of **verificatie services zijn**. Zie voor instructies en meer informatie over het verifiëren [verifiëren met Lake gegevensarchief Azure Active Directory](data-lake-store-authenticate-using-active-directory.md).

## <a name="create-a-net-application"></a>Een .NET-toepassing maken

1. Visual Studio openen en maken van een console-programma.

2. Klik op **Nieuw**in het menu **bestand** en klik vervolgens op **Project**.

3. Vanuit een **Nieuw Project**, typt of selecteert u de volgende waarden:

  	| Eigenschap | Waarde                       |
  	|----------|-----------------------------|
  	| Categorie | Sjablonen/Visual C# / Windows |
  	| Sjabloon | Consoletoepassing         |
  	| Naam     | CreateADLApplication        |

4. Klik op **OK** om het project te maken.

5. De pakketten Nuget toevoegen aan uw project.

    1. Klik met de rechtermuisknop op de naam van het project in de Solution Explorer en klik op **Beheren NuGet pakketten**.
    2. In het tabblad **Nuget Package Manager** Zorg dat **pakket bron** is ingesteld op **nuget.org** en deze **voorlopige versie opnemen** is ingeschakeld.
    3. Zoeken naar en installeren van de volgende NuGet pakketten:

        * `Microsoft.Azure.Management.DataLake.Store`-In deze zelfstudie wordt v0.12.5 voorbeeld.
        * `Microsoft.Azure.Management.DataLake.StoreUploader`-In deze zelfstudie wordt v0.10.6 voorbeeld.
        * `Microsoft.Rest.ClientRuntime.Azure.Authentication`-In deze zelfstudie wordt v2.2.8 voorbeeld.

        ![Een Nuget bron toevoegen] (./media/data-lake-store-get-started-net-sdk/ADL.Install.Nuget.Package.png "Een nieuwe Azure gegevens Lake-account maken")

    4. Sluit de **Nuget Package Manager**.

6. **Program.cs**openen, verwijderen van de bestaande code, en vervolgens de volgende instructies om de verwijzingen naar naamruimten toevoegen.

        using System;
        using System.Threading;
        
        using Microsoft.Rest.Azure.Authentication;
        using Microsoft.Azure.Management.DataLake.Store;
        using Microsoft.Azure.Management.DataLake.StoreUploader;

7. De variabelen te declareren, zoals hieronder wordt weergegeven en de waarden leveren voor gegevensopslag Lake naam en de naam van de resource die al bestaan. Controleer ook of het lokale pad en de bestandsnaam die u hier opgeeft, moet op de computer aanwezig. Het volgende codefragment toevoegen na de naamruimtedeclaraties.

        namespace SdkSample
        {
            class Program
            {
                private static DataLakeStoreAccountManagementClient _adlsClient;
                private static DataLakeStoreFileSystemManagementClient _adlsFileSystemClient;
                
                private static string _adlsAccountName;
                private static string _resourceGroupName;
                private static string _location;
                private static string _subId;

                
                private static void Main(string[] args)
                {
                    _adlsAccountName = "<DATA-LAKE-STORE-NAME>"; // TODO: Replace this value with the name of your existing Data Lake Store account.
                    _resourceGroupName = "<RESOURCE-GROUP-NAME>"; // TODO: Replace this value with the name of the resource group containing your Data Lake Store account.
                    _location = "East US 2";
                    _subId = "<SUBSCRIPTION-ID>";
                    
                    string localFolderPath = @"C:\local_path\"; // TODO: Make sure this exists and can be overwritten.
                    string localFilePath = localFolderPath + "file.txt"; // TODO: Make sure this exists and can be overwritten.
                    string remoteFolderPath = "/data_lake_path/";
                    string remoteFilePath = remoteFolderPath + "file.txt";
                }
            }
        }

In de overige secties van het artikel ziet u hoe u de beschikbare methoden voor .NET gebruiken voor het uitvoeren van bewerkingen, zoals verificatie, uploaden, enz.

## <a name="authentication"></a>Verificatie

### <a name="if-you-are-using-end-user-authentication-recommended-for-this-tutorial"></a>Als u eindgebruikers-verificatie (aanbevolen voor deze zelfstudie)

Combinatie met een bestaande Azure AD 'Native Client' toepassing; een vindt u hieronder. Als u deze zelfstudie sneller, is het raadzaam dat u deze methode gebruikt.

    // User login via interactive popup
    // Use the client ID of an existing AAD "Native Client" application.
    SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());
    var domain = "common"; // Replace this string with the user's Azure Active Directory tenant ID or domain name, if needed.
    var nativeClientApp_clientId = "1950a258-227b-4e31-a9cf-717495945fc2";
    var activeDirectoryClientSettings = ActiveDirectoryClientSettings.UsePromptOnly(nativeClientApp_clientId, new Uri("urn:ietf:wg:oauth:2.0:oob"));
    var creds = UserTokenProvider.LoginWithPromptAsync(domain, activeDirectoryClientSettings).Result;

Een paar dingen die u moet weten over dit fragment hierboven.

* Om u te helpen de zelfstudie sneller, in dit fragment wordt een een Azure AD-domein en de client-ID die is standaard beschikbaar voor alle abonnementen van Azure. Zo kunt u **gebruiken in dit fragment als-is in uw toepassing**.
* Echter als u uw eigen Azure AD domein- en client-ID gebruiken wilt, moet u een eigen toepassing Azure AD maken de Azure AD-domein-ID van de client gebruik en omleiden van URI voor de toepassing die u hebt gemaakt. Zie [een Active Directory-toepassing maken](../resource-group-create-service-principal-portal.md#create-an-active-directory-application) voor instructies.

>[AZURE.NOTE] De instructies in de bovenstaande koppelingen zijn voor een webtoepassing Azure AD. De stappen zijn echter precies dezelfde zelfs als u in plaats daarvan een native clienttoepassing maken. 

### <a name="if-you-are-using-service-to-service-authentication-with-client-secret"></a>Als u met geheim client services verificatie 

Het volgende codefragment kan worden gebruikt voor het verifiëren van uw toepassing niet-interactief toepassen, met de client geheim / sleutel voor een toepassing / service principal. Gebruik deze optie als u met een bestaande [toepassing van de "Web App" Azure AD](../resource-group-create-service-principal-portal.md).

    // Service principal / appplication authentication with client secret / key
    // Use the client ID and certificate of an existing AAD "Web App" application.
    SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());
    var domain = "<AAD-directory-domain>";
    var webApp_clientId = "<AAD-application-clientid>";
    var clientSecret = "<AAD-application-client-secret>";
    var clientCredential = new ClientCredential(webApp_clientId, clientSecret);
    var creds = ApplicationTokenProvider.LoginSilentAsync(domain, clientCredential).Result;

### <a name="if-you-are-using-service-to-service-authentication-with-certificate"></a>Als u met certificate Services verificatie

Een derde optie, kan het volgende codefragment worden gebruikt voor het verifiëren van uw toepassing niet-interactief toepassen, met behulp van het certificaat voor een toepassing / service principal. Gebruik deze optie als u met een bestaande [toepassing van de "Web App" Azure AD](../resource-group-create-service-principal-portal.md).

    // Service principal / application authentication with certificate
    // Use the client ID and certificate of an existing AAD "Web App" application.
    SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());
    var domain = "<AAD-directory-domain>";
    var webApp_clientId = "<AAD-application-clientid>";
    System.Security.Cryptography.X509Certificates.X509Certificate2 clientCert = <AAD-application-client-certificate>
    var clientAssertionCertificate = new ClientAssertionCertificate(webApp_clientId, clientCert);
    var creds = ApplicationTokenProvider.LoginSilentWithCertificateAsync(domain, clientAssertionCertificate).Result;

## <a name="create-client-objects"></a>Client-objecten maken

Het volgende codefragment maakt het gegevensarchief Lake account en filesystem client-objecten die worden gebruikt om te verzoeken om de service te verlenen.

    // Create client objects and set the subscription ID
    _adlsClient = new DataLakeStoreAccountManagementClient(creds);
    _adlsFileSystemClient = new DataLakeStoreFileSystemManagementClient(creds);

    _adlsClient.SubscriptionId = _subId;

## <a name="list-all-data-lake-store-accounts-within-a-subscription"></a>Lijst van alle Lake gegevensarchief rekeningen binnen een abonnement

Het volgende codefragment worden alle accounts weergegeven Lake gegevensarchief binnen een bepaald abonnement Azure.

    // List all ADLS accounts within the subscription
    public static List<DataLakeStoreAccount> ListAdlStoreAccounts()
    {
        var response = _adlsClient.Account.List();
        var accounts = new List<DataLakeStoreAccount>(response);
        
        while (response.NextPageLink != null)
        {
            response = _adlsClient.Account.ListNext(response.NextPageLink);
            accounts.AddRange(response);
        }
        
        return accounts;
    }

## <a name="create-a-directory"></a>Een map maken

Het volgende fragment een `CreateDirectory` methode die u gebruiken kunt voor het maken van een map in een gegevensarchief Lake-account.

    // Create a directory
    public static void CreateDirectory(string path)
    {
        _adlsFileSystemClient.FileSystem.Mkdirs(_adlsAccountName, path);
    }

## <a name="upload-a-file"></a>Upload een bestand

Het volgende fragment een `UploadFile` methode die u gebruiken kunt om bestanden te uploaden naar een gegevensarchief Lake.

    // Upload a file
    public static void UploadFile(string srcFilePath, string destFilePath, bool force = true)
    {
        var parameters = new UploadParameters(srcFilePath, destFilePath, _adlsAccountName, isOverwrite: force);
        var frontend = new DataLakeStoreFrontEndAdapter(_adlsAccountName, _adlsFileSystemClient);
        var uploader = new DataLakeStoreUploader(parameters, frontend);
        uploader.Execute();
    }

`DataLakeStoreUploader`ondersteunt recursieve uploaden en downloaden tussen een lokaal bestandspad en een gegevensarchief Lake pad.    

## <a name="get-file-or-directory-info"></a>Bestand of map info ophalen

Het volgende fragment een `GetItemInfo` methode die u gebruiken kunt voor het ophalen van informatie over een bestand of directory die beschikbaar zijn in het gegevensarchief Lake. 

    // Get file or directory info
    public static FileStatusProperties GetItemInfo(string path)
    {
        return _adlsFileSystemClient.FileSystem.GetFileStatus(_adlsAccountName, path).FileStatus;
    }

## <a name="list-file-or-directories"></a>Lijst met bestanden of mappen

Het volgende fragment een `ListItem` methode die u gebruiken kunt om de bestanden en mappen in een gegevensarchief Lake account weer.

    // List files and directories
    public static List<FileStatusProperties> ListItems(string directoryPath)
    {
        return _adlsFileSystemClient.FileSystem.ListFileStatus(_adlsAccountName, directoryPath).FileStatuses.FileStatus.ToList();
    }

## <a name="concatenate-files"></a>Bestanden samenvoegen

Het volgende fragment een `ConcatenateFiles` methode die u gebruikt om bestanden samen te voegen. 

    // Concatenate files
    public static void ConcatenateFiles(string[] srcFilePaths, string destFilePath)
    {
        _adlsFileSystemClient.FileSystem.Concat(_adlsAccountName, destFilePath, srcFilePaths);
    }

## <a name="append-to-a-file"></a>Toevoegen aan een bestand

Het volgende fragment een `AppendToFile` methode waarmee u gegevens toevoegen aan een bestand al is opgeslagen in een gegevensarchief Lake-account.

    // Append to file
    public static void AppendToFile(string path, string content)
    {
        var stream = new MemoryStream(Encoding.UTF8.GetBytes(content));
        
        _adlsFileSystemClient.FileSystem.Append(_adlsAccountName, path, stream);
    }

## <a name="download-a-file"></a>Een bestand downloaden

Het volgende fragment een `DownloadFile` methode die u gebruikt een bestand te downloaden van een gegevensarchief Lake-account.

    // Download file
    public static void DownloadFile(string srcPath, string destPath)
    {
        var stream = _adlsFileSystemClient.FileSystem.Open(_adlsAccountName, srcPath);
        var fileStream = new FileStream(destPath, FileMode.Create);
        
        stream.CopyTo(fileStream);
        fileStream.Close();
        stream.Close();
    }

## <a name="next-steps"></a>Volgende stappen

- [Beveiliging van gegevens in het gegevensarchief Lake](data-lake-store-secure-data.md)
- [Azure gegevens Lake Analytics gebruiken met Lake gegevensarchief](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Azure HDInsight gebruiken met Lake gegevensarchief](data-lake-store-hdinsight-hadoop-use-portal.md)
- [Verwijzing naar Lake gegevensarchief .NET SDK](https://msdn.microsoft.com/library/mt581387.aspx)
- [Gegevensarchief meer REST verwijzing](https://msdn.microsoft.com/library/mt693424.aspx)
