<properties
    pageTitle="Beheer .NET SDK voor Stream Analytics | Microsoft Azure"
    description="Aan de slag met Stream Analytics beheer .NET SDK. Meer informatie over het instellen en uitvoeren van taken analytics: maken van een project, inputs en outputs en transformaties."
    keywords=".NET SDK, analytics API"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"/>


# <a name="management-net-sdk-set-up-and-run-analytics-jobs-using-the-azure-stream-analytics-api-for-net"></a>Beheer .NET SDK: Instellen en uitvoeren van analytics taken Azure Stream Analytics API voor .NET gebruiken

Informatie over het instellen van een run analytics taken met behulp van de Stream Analytics API voor beheer .NET SDK met .NET. Een project instellen, invoer en uitvoer van bronnen, transformaties en start maken en taken stoppen. U kunt gegevens uit Blob-opslag of een gebeurtenis hub streamen voor uw analytics-projecten.

Zie de [documentatie management voor de Stream Analytics API voor .NET](https://msdn.microsoft.com/library/azure/dn889315.aspx).

Azure Stream Analytics is een volledig beheerde service met lage latentie, hoge beschikbaarheid, schaalbaar, complexe gebeurtenis verwerken via streaming gegevens in de cloud. Stream Analytics kan klanten streaming taken instellen voor het analyseren van gegevensstromen en kan ze in de buurt van real-time analytics station.  


## <a name="prerequisites"></a>Vereisten
Voordat u dit artikel hebt u het volgende:

- Installeer Visual Studio 2012 of 2013.
- Download en installeer [Azure.NET SDK](https://azure.microsoft.com/downloads/).
- Maak een brongroep Azure in uw abonnement. Hier volgt een voorbeeld van Azure PowerShell script. Zie voor informatie Azure PowerShell [installeren en configureren van Azure PowerShell](../powershell-install-configure.md);  


        # Log in to your Azure account
        Add-AzureAccount

        # Select the Azure subscription you want to use to create the resource group
        Select-AzureSubscription -SubscriptionName <subscription name>

            # If Stream Analytics has not been registered to the subscription, remove the remark symbol (#) to run the Register-AzureRMProvider cmdlet to register the provider namespace
            #Register-AzureRMProvider -Force -ProviderNamespace 'Microsoft.StreamAnalytics'

        # Create an Azure resource group
        New-AzureResourceGroup -Name <YOUR RESOURCE GROUP NAME> -Location <LOCATION>


-   Instellen van een bron invoer- en uitvoer te gebruiken. Voor meer Zie instructies [Invoer toevoegen](stream-analytics-add-inputs.md) voor het instellen van een monster input en [Output toevoegen](stream-analytics-add-outputs.md) om een voorbeeld van uitvoer.


## <a name="set-up-a-project"></a>Een project instellen

Een taak analytics kunt maken de Stream Analytics API for .NET, eerst het project instellen.

1. Maak een consoletoepassing Visual Studio C# .NET.
2. Voer de volgende opdrachten de NuGet pakketten installeren in de Package Manager-Console. De eerste is de Azure Stream Analytics beheer .NET SDK. Het tweede is de Azure Active Directory-client die wordt gebruikt voor verificatie.

        Install-Package Microsoft.Azure.Management.StreamAnalytics
        Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory

4. De volgende sectie **appSettings** toevoegen aan het bestand App.config:

        <appSettings>
          <!--CSM Prod related values-->
          <add key="ActiveDirectoryEndpoint" value="https://login.windows.net/" />
          <add key="ResourceManagerEndpoint" value="https://management.azure.com/" />
          <add key="WindowsManagementUri" value="https://management.core.windows.net/" />
          <add key="AsaClientId" value="1950a258-227b-4e31-a9cf-717495945fc2" />
          <add key="RedirectUri" value="urn:ietf:wg:oauth:2.0:oob" />
          <add key="SubscriptionId" value="YOUR AZURE SUBSCRIPTION" />
          <add key="ActiveDirectoryTenantId" value="YOU TENANT ID" />
        </appSettings>


    Waarden voor **SubscriptionId** en **ActiveDirectoryTenantId** vervangen door uw abonnement Azure en id's pachters. U kunt deze waarden verkrijgen door de volgende Azure PowerShell-cmdlet wordt uitgevoerd:

        Get-AzureAccount

5. De volgende instructies voor het **gebruik van** toevoegen aan het bronbestand (Program.cs) in het project:

        using System;
        using System.Configuration;
        using System.Threading;
        using Microsoft.Azure;
        using Microsoft.Azure.Management.StreamAnalytics;
        using Microsoft.Azure.Management.StreamAnalytics.Models;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;

6.  Een helper verificatiemethode toevoegen:

        public static string GetAuthorizationHeader()
        {
            AuthenticationResult result = null;
            var thread = new Thread(() =>
            {
                try
                {
                    var context = new AuthenticationContext(
                        ConfigurationManager.AppSettings["ActiveDirectoryEndpoint"] +
                        ConfigurationManager.AppSettings["ActiveDirectoryTenantId"]);

                    result = context.AcquireToken(
                        resource: ConfigurationManager.AppSettings["WindowsManagementUri"],
                        clientId: ConfigurationManager.AppSettings["AsaClientId"],
                        redirectUri: new Uri(ConfigurationManager.AppSettings["RedirectUri"]),
                        promptBehavior: PromptBehavior.Always);
                }
                catch (Exception threadEx)
                {
                    Console.WriteLine(threadEx.Message);
                }
            });

            thread.SetApartmentState(ApartmentState.STA);
            thread.Name = "AcquireTokenThread";
            thread.Start();
            thread.Join();

            if (result != null)
            {
                return result.AccessToken;
            }

            throw new InvalidOperationException("Failed to acquire token");
        }  


## <a name="create-a-stream-analytics-management-client"></a>Maak een Stream Analytics-client management

Een **StreamAnalyticsManagementClient** -object kunt u de taak en het project-onderdelen, zoals invoer, uitvoer en transformatie te beheren.

Voeg de volgende code aan het begin van de **Main** -methode:

    string resourceGroupName = "<YOUR AZURE RESOURCE GROUP NAME>";
    string streamAnalyticsJobName = "<YOUR STREAM ANALYTICS JOB NAME>";
    string streamAnalyticsInputName = "<YOUR JOB INPUT NAME>";
    string streamAnalyticsOutputName = "<YOUR JOB OUTPUT NAME>";
    string streamAnalyticsTransformationName = "<YOUR JOB TRANSFORMATION NAME>";

    // Get authentication token
    TokenCloudCredentials aadTokenCredentials =
        new TokenCloudCredentials(
            ConfigurationManager.AppSettings["SubscriptionId"],
            GetAuthorizationHeader());

    // Create Stream Analytics management client
    StreamAnalyticsManagementClient client = new StreamAnalyticsManagementClient(aadTokenCredentials);

Waarde van de variabele **resourceGroupName** moet hetzelfde zijn als de naam van de resourcegroep die u hebt gemaakt of verzameld in de vereiste stappen.

Zie [verificatie van een service principal Azure Resource Manager](../resource-group-authenticate-service-principal.md)voor het automatiseren van de referentie presentatie aspect van de schepping van werkgelegenheid.

De overige secties van dit artikel wordt ervan uitgegaan dat deze code aan het begin van de **Main** -methode.

## <a name="create-a-stream-analytics-job"></a>Maak een taak Stream Analytics

De volgende code maakt een Stream Analytics taak in de resourcegroep die u hebt gedefinieerd. Aan de taak zal u een invoer-, uitvoer- en transformatie later toevoegen.

    // Create a Stream Analytics job
    JobCreateOrUpdateParameters jobCreateParameters = new JobCreateOrUpdateParameters()
    {
        Job = new Job()
        {
            Name = streamAnalyticsJobName,
            Location = "<LOCATION>",
            Properties = new JobProperties()
            {
                EventsOutOfOrderPolicy = EventsOutOfOrderPolicy.Adjust,
                Sku = new Sku()
                {
                    Name = "Standard"
                }
            }
        }
    };

    JobCreateOrUpdateResponse jobCreateResponse = client.StreamingJobs.CreateOrUpdate(resourceGroupName, jobCreateParameters);


## <a name="create-a-stream-analytics-input-source"></a>Maak een invoerbron Stream Analytics

De volgende code maakt een invoerbron Analytics stroom met de blob-invoerbron type en CSV-serialisatie. Gebruik wilt maken van een hub-invoerbron gebeurtenis, **EventHubStreamInputDataSource** **BlobStreamInputDataSource**. U kunt ook het serialisatietype van de invoerbron.

    // Create a Stream Analytics input source
    InputCreateOrUpdateParameters jobInputCreateParameters = new InputCreateOrUpdateParameters()
    {
        Input = new Input()
        {
            Name = streamAnalyticsInputName,
            Properties = new StreamInputProperties()
            {
                Serialization = new CsvSerialization
                {
                    Properties = new CsvSerializationProperties
                    {
                        Encoding = "UTF8",
                        FieldDelimiter = ","
                    }
                },
                DataSource = new BlobStreamInputDataSource
                {
                    Properties = new BlobStreamInputDataSourceProperties
                    {
                        StorageAccounts = new StorageAccount[]
                        {
                            new StorageAccount()
                            {
                                AccountName = "<YOUR STORAGE ACCOUNT NAME>",
                                AccountKey = "<YOUR STORAGE ACCOUNT KEY>"
                            }
                        },
                        Container = "samples",
                        PathPattern = ""
                    }
                }
            }
        }
    };

    InputCreateOrUpdateResponse inputCreateResponse =
        client.Inputs.CreateOrUpdate(resourceGroupName, streamAnalyticsJobName, jobInputCreateParameters);

Bronnen, zijn van Blob-opslag of een hub gebeurtenis gekoppeld aan een bepaalde taak. Voor het gebruik van de dezelfde invoerbron voor verschillende taken, moet u de methode opnieuw aanroept en een andere taaknaam opgeven.


## <a name="test-a-stream-analytics-input-source"></a>Test een invoerbron Stream Analytics

De methode **TestConnection** wordt gecontroleerd of de taak Analytics Stream verbinding kunnen maken met de invoerbron, alsmede andere aspecten die specifiek voor de invoerbron is. Bijvoorbeeld in de blob-invoerbron die u hebt gemaakt in een eerdere stap, controleert de methode de accountnaam van de opslag en het sleutelpaar gebruikt voor verbinding met de account van de opslag en controleer of de opgegeven container bestaat.

    // Test input source connection
    DataSourceTestConnectionResponse inputTestResponse =
        client.Inputs.TestConnection(resourceGroupName, streamAnalyticsJobName, streamAnalyticsInputName);

## <a name="create-a-stream-analytics-output-target"></a>Een doel van de output Stream Analytics maken

Maken van een uitvoerdoel is vergelijkbaar met het maken van een invoerbron Stream Analytics. Uitvoer doelen zijn zoals invoerbronnen gekoppeld aan een bepaalde taak. Gebruik hetzelfde doel output voor verschillende taken, moet u de methode opnieuw aanroept en een andere taaknaam opgeven.

De volgende code maakt een doel output (Azure SQL-database). U kunt het gegevenstype van het uitvoerdoel en/of serialisatietype.

    // Create a Stream Analytics output target
    OutputCreateOrUpdateParameters jobOutputCreateParameters = new OutputCreateOrUpdateParameters()
    {
        Output = new Output()
        {
            Name = streamAnalyticsOutputName,
            Properties = new OutputProperties()
            {
                DataSource = new SqlAzureOutputDataSource()
                {
                    Properties = new SqlAzureOutputDataSourceProperties()
                    {
                        Server = "<YOUR DATABASE SERVER NAME>",
                        Database = "<YOUR DATABASE NAME>",
                        User = "<YOUR DATABASE LOGIN>",
                        Password = "<YOUR DATABASE LOGIN PASSWORD>",
                        Table = "<YOUR DATABASE TABLE NAME>"
                    }
                }
            }
        }
    };

    OutputCreateOrUpdateResponse outputCreateResponse =
        client.Outputs.CreateOrUpdate(resourceGroupName, streamAnalyticsJobName, jobOutputCreateParameters);

## <a name="test-a-stream-analytics-output-target"></a>Een doel van de output Stream Analytics testen

Een doel van de output Stream Analytics heeft ook de **TestConnection** -methode voor het testen van verbindingen.

    // Test output target connection
    DataSourceTestConnectionResponse outputTestResponse =
        client.Outputs.TestConnection(resourceGroupName, streamAnalyticsJobName, streamAnalyticsOutputName);

## <a name="create-a-stream-analytics-transformation"></a>Een Stream Analytics transformatie maken

De volgende code maakt een transformatie Analytics stroom met de query "Selecteer * van Input ' en geeft u één streaming eenheid worden toegewezen voor de taak Stream Analytics. Zie voor meer informatie over het aanpassen van streaming eenheden [schaal Azure Stream Analytics taken](stream-analytics-scale-jobs.md).


    // Create a Stream Analytics transformation
    TransformationCreateOrUpdateParameters transformationCreateParameters = new TransformationCreateOrUpdateParameters()
    {
        Transformation = new Transformation()
        {
            Name = streamAnalyticsTransformationName,
            Properties = new TransformationProperties()
            {
                StreamingUnits = 1,
                Query = "select * from Input"
            }
        }
    };

    var transformationCreateResp =
        client.Transformations.CreateOrUpdate(resourceGroupName, streamAnalyticsJobName, transformationCreateParameters);

Net als invoer en uitvoer, een transformaties ook is gebonden aan de Stream Analytics taak onder is gemaakt.

## <a name="start-a-stream-analytics-job"></a>Een Stream Analytics taak starten
U kunt de taak starten door het aanroepen van de methode **Start** na het maken van een taak Stream Analytics en de input(s), output(s) en transformatie.

Voorbeeld van de volgende code wordt een taak Stream Analytics met een begintijd van aangepaste uitvoer ingesteld op 12 December 2012, 12:12:12 UTC:

    // Start a Stream Analytics job
    JobStartParameters jobStartParameters = new JobStartParameters
    {
        OutputStartMode = OutputStartMode.CustomTime,
        OutputStartTime = new DateTime(2012, 12, 12, 0, 0, 0, DateTimeKind.Utc)
    };

    LongRunningOperationResponse jobStartResponse = client.StreamingJobs.Start(resourceGroupName, streamAnalyticsJobName, jobStartParameters);



## <a name="stop-a-stream-analytics-job"></a>Een Stream Analytics taak stoppen
U kunt een actieve Stream Analytics taak stoppen door het aanroepen van de methode **Stop** .

    // Stop a Stream Analytics job
    LongRunningOperationResponse jobStopResponse = client.StreamingJobs.Stop(resourceGroupName, streamAnalyticsJobName);

## <a name="delete-a-stream-analytics-job"></a>Een Stream Analytics taak verwijderen
De methode **Delete** verwijdert de taak als de onderliggende subtaken bronnen, waaronder input(s), output(s) en transformatie van de taak.

    // Delete a Stream Analytics job
    LongRunningOperationResponse jobDeleteResponse = client.StreamingJobs.Delete(resourceGroupName, streamAnalyticsJobName);


## <a name="get-support"></a>Ondersteuning krijgen
Probeer onze [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)voor verdere ondersteuning.


## <a name="next-steps"></a>Volgende stappen

U hebt leren de grondbeginselen van het gebruik van een .NET SDK maken en uitvoeren van taken met analytics. Voor meer informatie, Zie de volgende:

- [Inleiding tot Analytics Azure Stream](stream-analytics-introduction.md)
- [Aan de slag met Azure Stream Analytics](stream-analytics-get-started.md)
- [Schaal Azure Stream Analytics taken](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics beheer .NET SDK](https://msdn.microsoft.com/library/azure/dn889315.aspx).
- [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics beheer REST API: naslag](https://msdn.microsoft.com/library/azure/dn835031.aspx)


<!--Image references-->
[5]: ./media/markdown-template-for-new-articles/octocats.png
[6]: ./media/markdown-template-for-new-articles/pretty49.png
[7]: ./media/markdown-template-for-new-articles/channel-9.png


<!--Link references-->
[azure.blob.storage]: http://azure.microsoft.com/documentation/services/storage/
[azure.blob.storage.use]: http://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/

[azure.event.hubs]: http://azure.microsoft.com/services/event-hubs/
[azure.event.hubs.developer.guide]: http://msdn.microsoft.com/library/azure/dn789972.aspx

[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.forum]: http://go.microsoft.com/fwlink/?LinkId=512151

[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-get-started.md
[stream.analytics.developer.guide]: stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
