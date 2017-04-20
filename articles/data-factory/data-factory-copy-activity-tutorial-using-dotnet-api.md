<properties 
    pageTitle="Zelfstudie: Maak een pijpleiding met kopie activiteit met .NET API | Microsoft Azure" 
    description="In deze zelfstudie maakt u een pijpleiding Azure Data Factory met een activiteit kopiëren met behulp van .NET API." 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="10/27/2016" 
    ms.author="spelluru"/>

# <a name="tutorial-create-a-pipeline-with-copy-activity-using-net-api"></a>Zelfstudie: Een pijpleiding met kopie activiteit met .NET API maken
> [AZURE.SELECTOR]
- [Overzicht en vereisten](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
- [Wizard kopiëren](data-factory-copy-data-wizard-tutorial.md)
- [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md)
- [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
- [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
- [Sjabloon voor Azure Resource Manager](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
- [REST-API](data-factory-copy-activity-tutorial-using-rest-api.md)
- [.NET API](data-factory-copy-activity-tutorial-using-dotnet-api.md)


In deze zelfstudie wordt beschreven hoe u maken en controleren van een Azure data factory .NET API gebruiken. Een activiteit kopiëren de pijpleiding die in de fabriek gegevens gebruikt om gegevens te kopiëren van Azure Blob-opslag met Azure SQL-Database.

De activiteit kopie uitvoert verplaatsing van gegevens in Azure Data Factory. De activiteit wordt aangedreven door een wereldwijd beschikbare service waarmee gegevens tussen verschillende opgeslagen gegevens op een veilige, schaalbare en betrouwbare manier kunt kopiëren. Zie [Gegevensverplaatsing activiteiten](data-factory-data-movement-activities.md) -artikel voor meer informatie over de activiteit van de kopie.   

> [AZURE.NOTE] 
> In dit artikel geldt niet voor alle de Data Factory .NET API. Zie [Data Factory.NET API Reference](https://msdn.microsoft.com/library/mt415893.aspx) voor meer informatie over Data Factory .NET SDK. 

## <a name="prerequisites"></a>Vereisten
- [Overzicht van zelfstudie en de noodzakelijke voorwaarden](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) voor een overzicht van de zelfstudie en de **vereiste** stappen doorlopen. 
- Visual Studio 2012 of 2013 of 2015
- Download en installeer [Azure.NET SDK](http://azure.microsoft.com/downloads/)
- Azure PowerShell. Volg de instructies in [het installeren en configureren van Azure PowerShell](../powershell-install-configure.md) artikel Azure PowerShell installeren op uw computer. Azure PowerShell u maakt een toepassing Azure Active Directory.

### <a name="create-an-application-in-azure-active-directory"></a>Een toepassing in Azure Active Directory maken
Een Azure Active Directory-toepassing maken, een service principal voor de toepassing maken en toewijzen aan de rol van **Inzender fabriek** .  

1. **PowerShell**start. 
1. Voer de volgende opdracht uit en voert u de gebruikersnaam en het wachtwoord die u gebruikt voor aanmelding bij de Azure portal.
    
        Login-AzureRmAccount   
2. Voer de volgende opdracht om de abonnementen voor deze account bekijken.

        Get-AzureRmSubscription 
3. Selecteer het abonnement dat u werken wilt met de volgende opdracht uitvoeren. Vervangen ** &lt;NameOfAzureSubscription** &gt; met de naam van uw abonnement op Azure. 

        Get-AzureRmSubscription -SubscriptionName <NameOfAzureSubscription> | Set-AzureRmContext

    > [AZURE.IMPORTANT] Let op **SubscriptionId** en **TenantId** uit de uitvoer van deze opdracht. 
4. Maak een groep Azure resource met de naam **ADFTutorialResourceGroup** door de volgende opdracht uit te voeren in de PowerShell.  

        New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"

    Als de resourcegroep al bestaat, opgeven of u wilt werken (Y) of als (N). 

    Als u een resourcegroep gebruikt, moet u de naam van de resourcegroep in plaats van ADFTutorialResourceGroup in deze zelfstudie gebruiken.
5. Azure Active Directory-toepassing maken. 

        $azureAdApplication = New-AzureRmADApplication -DisplayName "ADFCopyTutotiralApp" -HomePage "https://www.contoso.org" -IdentifierUris "https://www.adfcopytutorialapp.org/example" -Password "Pass@word1"

    Als u de volgende fout krijgt, Geef een andere URL en voert u de opdracht opnieuw. 

        Another object with the same value for property identifierUris already exists.

6. Maak de advertentie service principal. 

        New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId

7. Service principal toevoegen aan de rol van **Inzender fabriek** . 

        New-AzureRmRoleAssignment -RoleDefinitionName "Data Factory Contributor" -ServicePrincipalName $azureAdApplication.ApplicationId.Guid

8. Ophalen van de toepassings-ID.

        $azureAdApplication

    Let op de toepassings-ID (**applicationID** uit de uitvoer).

Hebt u na vier waarden uit de volgende stappen uit: 

- Huurder-ID
- Abonnement-ID
- Toepassings-ID 
- Wachtwoord (opgegeven in de eerste opdracht)   

## <a name="walkthrough"></a>Stapsgewijze instructies
1. Met Visual Studio 2012/2013/2015 maakt u een consoletoepassing C# .NET.
    1. Start **Visual Studio** 2012/2013/2015.
    2. Klik op **bestand**, wijs **Nieuw**aan en klik op **Project**.
    3. **Sjablonen**en selecteer **Visual C#**. In dit scenario maakt u gebruik van C#, maar kunt u een .NET-taal.
    4. **Console-programma** uit de lijst met projecttypen aan de rechterkant selecteren.
    5. **DataFactoryAPITestApp** voor de naam invoeren.
    6. **C:\ADFGetStarted** voor de locatie selecteren.
    7. Klik op **OK** om het project te maken.
2. Klik op **Extra**en klik op **Package Manager-Console**wijs **Nuget Package Manager**.
3.  Voer de volgende stappen uit in de **Package Manager-Console**: 
    1.  Voer de volgende opdracht Data Factory-pakket installeren:`Install-Package Microsoft.Azure.Management.DataFactories`       
    2.  Voer de volgende opdracht pakket Azure Active Directory installeren (u gebruikt Active Directory API in de code):`Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.19.208020213`
6. In het volgende gedeelte van de **appSetttings** toevoegen aan het bestand **App.config** . Deze instellingen worden gebruikt door de helpmethode: **GetAuthorizationHeader**. 

    Vervang de waarden voor ** &lt;-toepassings-ID&gt;**, ** &lt;wachtwoord&gt;**, ** &lt;abonnements-ID&gt;**, en ** &lt;ID pachters&gt; ** met uw eigen waarden. 

        <?xml version="1.0" encoding="utf-8" ?>
        <configuration>
            <startup> 
                <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5.2" />
            </startup>
            <appSettings>
                <add key="ActiveDirectoryEndpoint" value="https://login.windows.net/" />
                <add key="ResourceManagerEndpoint" value="https://management.azure.com/" />
                <add key="WindowsManagementUri" value="https://management.core.windows.net/" />

                <add key="ApplicationId" value="your application ID" />
                <add key="Password" value="Password you used while creating the AAD application" />
                <add key="SubscriptionId" value= "Subscription ID" />
                <add key="ActiveDirectoryTenantId" value="Tenant ID" />
            </appSettings>
        </configuration>
6. De volgende instructies voor **Gebruik** met het bronbestand (Program.cs) in het project toevoegen.

        using System.Threading;
        using System.Configuration;
        using System.Collections.ObjectModel;
        
        using Microsoft.Azure.Management.DataFactories;
        using Microsoft.Azure.Management.DataFactories.Models;
        using Microsoft.Azure.Management.DataFactories.Common.Models;
        
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Azure;
6. Voeg de volgende code een instantie van de klasse **DataPipelineManagementClient** aan de **Main** -methode maakt. U kunt dit object gebruiken voor het maken van een data factory, een gekoppelde service, invoer en uitvoer datasets en een pijpleiding. U dit object ook gebruiken voor het controleren van de segmenten van een dataset tijdens runtime.    

            // create data factory management client
            string resourceGroupName = "ADFTutorialResourceGroup";
            string dataFactoryName = "APITutorialFactory";

            TokenCloudCredentials aadTokenCredentials =
                new TokenCloudCredentials(
                    ConfigurationManager.AppSettings["SubscriptionId"],
                    GetAuthorizationHeader());

            Uri resourceManagerUri = new Uri(ConfigurationManager.AppSettings["ResourceManagerEndpoint"]);

            DataFactoryManagementClient client = new DataFactoryManagementClient(aadTokenCredentials, resourceManagerUri);

    > [AZURE.IMPORTANT] 
    > De waarde van **resourceGroupName** vervangen door de naam van de resourcegroep Azure. 
    > 
    > Naam van de data factory (**dataFactoryName**) uniek bijwerken. Naam van de fabriek gegevens moet uniek zijn. Zie [Data Factory - regels voor naamgeving van](data-factory-naming-rules.md) onderwerp voor naamgevingsregels voor Data Factory artefacten. 

7. Voeg de volgende code die wordt gemaakt van een **fabriek van gegevens** aan de **Main** -methode.

            // create a data factory
            Console.WriteLine("Creating a data factory");
            client.DataFactories.CreateOrUpdate(resourceGroupName,
                new DataFactoryCreateOrUpdateParameters()
                {
                    DataFactory = new DataFactory()
                    {
                        Name = dataFactoryName,
                        Location = "westus",
                        Properties = new DataFactoryProperties() { }
                    }
                }
            );

8. Voeg de volgende code waarmee een **Azure Storage service gekoppeld** aan de **Main** -methode. 

    > [AZURE.IMPORTANT] **Storageaccountname** en **accountkey** vervangen door de naam en de sleutel van uw account Azure opslag. 

            // create a linked service for input data store: Azure Storage
            Console.WriteLine("Creating Azure Storage linked service");
            client.LinkedServices.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new LinkedServiceCreateOrUpdateParameters()
                {
                    LinkedService = new LinkedService()
                    {
                        Name = "AzureStorageLinkedService",
                        Properties = new LinkedServiceProperties
                        (
                            new AzureStorageLinkedService("DefaultEndpointsProtocol=https;AccountName=<storageaccountname>;AccountKey=<accountkey>")
                        )
                    }
                }
            );
9. Voeg de volgende code waarmee een **Azure SQL service gekoppeld** aan de **Main** -methode.
 
    > [AZURE.IMPORTANT] **Servernaam**, de **databasenaam**, **gebruikersnaam**en **wachtwoord** vervangen door namen van uw Azure SQL server-database, gebruiker en wachtwoord.  

            // create a linked service for output data store: Azure SQL Database
            Console.WriteLine("Creating Azure SQL Database linked service");
            client.LinkedServices.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new LinkedServiceCreateOrUpdateParameters()
                {
                    LinkedService = new LinkedService()
                    {
                        Name = "AzureSqlLinkedService",
                        Properties = new LinkedServiceProperties
                        (
                            new AzureSqlDatabaseLinkedService("Data Source=tcp:<servername>.database.windows.net,1433;Initial Catalog=<databasename>;User ID=<username>;Password=<password>;Integrated Security=False;Encrypt=True;Connect Timeout=30")
                        )
                    }
                }
            );
10. Voeg de volgende code die de **invoer en uitvoer datasets** aan de **Main** -methode maakt. 

            // create input and output datasets
            Console.WriteLine("Creating input and output datasets");
            string Dataset_Source = "DatasetBlobSource";
            string Dataset_Destination = "DatasetAzureSqlDestination";

            Console.WriteLine("Creating input dataset of type: Azure Blob");
            client.Datasets.CreateOrUpdate(resourceGroupName, dataFactoryName,


            new DatasetCreateOrUpdateParameters()
            {
                Dataset = new Dataset()
                {
                    Name = Dataset_Source,
                    Properties = new DatasetProperties()
                    {
                        Structure = new List<DataElement>()
                        {
                            new DataElement() { Name = "FirstName", Type = "String" },
                            new DataElement() { Name = "LastName", Type = "String" }
                        },
                        LinkedServiceName = "AzureStorageLinkedService",
                        TypeProperties = new AzureBlobDataset()
                        {
                            FolderPath = "adftutorial/",
                            FileName = "emp.txt"
                        },
                        External = true,
                        Availability = new Availability()
                        {
                            Frequency = SchedulePeriod.Hour,
                            Interval = 1,
                        },

                        Policy = new Policy()
                        {
                            Validation = new ValidationPolicy()
                            {
                                MinimumRows = 1
                            }
                        }
                    }
                }
            });


            Console.WriteLine("Creating output dataset of type: Azure SQL");
            client.Datasets.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new DatasetCreateOrUpdateParameters()
                {
                    Dataset = new Dataset()
                    {
                        Name = Dataset_Destination,
                        Properties = new DatasetProperties()
                        {
                            Structure = new List<DataElement>()
                            {
                                new DataElement() { Name = "FirstName", Type = "String" },
                                new DataElement() { Name = "LastName", Type = "String" }
                            },
                            LinkedServiceName = "AzureSqlLinkedService",
                            TypeProperties = new AzureSqlTableDataset()
                            {
                                TableName = "emp"
                            },

                            Availability = new Availability()
                            {
                                Frequency = SchedulePeriod.Hour,
                                Interval = 1,
                            },
                        }
                    }
                });

11. Voeg de volgende code die **wordt gemaakt en activeert u een pijpleiding** aan de **Main** -methode. Deze pijpleiding heeft een **CopyActivity** die **BlobSource** als een bron- en **BlobSink** als een sink.

            // create a pipeline
            Console.WriteLine("Creating a pipeline");
            DateTime PipelineActivePeriodStartTime = new DateTime(2016, 8, 9, 0, 0, 0, 0, DateTimeKind.Utc);
            DateTime PipelineActivePeriodEndTime = PipelineActivePeriodStartTime.AddMinutes(60);
            string PipelineName = "ADFTutorialPipeline";

            client.Pipelines.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new PipelineCreateOrUpdateParameters()
                {
                    Pipeline = new Pipeline()
                    {
                        Name = PipelineName,
                        Properties = new PipelineProperties()
                        {
                            Description = "Demo Pipeline for data transfer between blobs",

                    // Initial value for pipeline's active period. With this, you won't need to set slice status
                    Start = PipelineActivePeriodStartTime,
                            End = PipelineActivePeriodEndTime,

                            Activities = new List<Activity>()
                            {
                                new Activity()
                                {
                                    Name = "BlobToAzureSql",
                                    Inputs = new List<ActivityInput>()
                                    {
                                        new ActivityInput() {
                                            Name = Dataset_Source
                                        }
                                    },
                                    Outputs = new List<ActivityOutput>()
                                    {
                                        new ActivityOutput()
                                        {
                                            Name = Dataset_Destination
                                        }
                                    },
                                    TypeProperties = new CopyActivity()
                                    {
                                        Source = new BlobSource(),
                                        Sink = new BlobSink()
                                        {
                                            WriteBatchSize = 10000,
                                            WriteBatchTimeout = TimeSpan.FromMinutes(10)
                                        }
                                    }
                                }
                            },
                        }
                    }
                }); 

12. Voeg de volgende code aan de **belangrijkste** methode om de status van een segment van de dataset uitvoer. Er is alleen segment verwacht in dit voorbeeld.   
 
            // Pulling status within a timeout threshold
            DateTime start = DateTime.Now;
            bool done = false;

            while (DateTime.Now - start < TimeSpan.FromMinutes(5) && !done)
            {
                Console.WriteLine("Pulling the slice status");
                // wait before the next status check
                Thread.Sleep(1000 * 12);

                var datalistResponse = client.DataSlices.List(resourceGroupName, dataFactoryName, Dataset_Destination,
                    new DataSliceListParameters()
                    {
                        DataSliceRangeStartTime = PipelineActivePeriodStartTime.ConvertToISO8601DateTimeString(),
                        DataSliceRangeEndTime = PipelineActivePeriodEndTime.ConvertToISO8601DateTimeString()
                    });

                foreach (DataSlice slice in datalistResponse.DataSlices)
                {
                    if (slice.State == DataSliceState.Failed || slice.State == DataSliceState.Ready)
                    {
                        Console.WriteLine("Slice execution is done with status: {0}", slice.State);
                        done = true;
                        break;
                    }
                    else
                    {
                        Console.WriteLine("Slice status is: {0}", slice.State);
                    }
                }
            }

14. Voeg de volgende code om details voor een segment aan de **Main** -methode get uitvoeren.

            Console.WriteLine("Getting run details of a data slice");

            // give it a few minutes for the output slice to be ready
            Console.WriteLine("\nGive it a few minutes for the output slice to be ready and press any key.");
            Console.ReadKey();

            var datasliceRunListResponse = client.DataSliceRuns.List(
                    resourceGroupName,
                    dataFactoryName,
                    Dataset_Destination,
                    new DataSliceRunListParameters()
                    {
                        DataSliceStartTime = PipelineActivePeriodStartTime.ConvertToISO8601DateTimeString()
                    }
                );

            foreach (DataSliceRun run in datasliceRunListResponse.DataSliceRuns)
            {
                Console.WriteLine("Status: \t\t{0}", run.Status);
                Console.WriteLine("DataSliceStart: \t{0}", run.DataSliceStart);
                Console.WriteLine("DataSliceEnd: \t\t{0}", run.DataSliceEnd);
                Console.WriteLine("ActivityId: \t\t{0}", run.ActivityName);
                Console.WriteLine("ProcessingStartTime: \t{0}", run.ProcessingStartTime);
                Console.WriteLine("ProcessingEndTime: \t{0}", run.ProcessingEndTime);
                Console.WriteLine("ErrorMessage: \t{0}", run.ErrorMessage);
            }

            Console.WriteLine("\nPress any key to exit.");
            Console.ReadKey();

13. Voeg de volgende helpmethode door de **Main** -methode aan de klasse **wordt** gebruikt.  
 
        public static string GetAuthorizationHeader()
        {
            AuthenticationResult result = null;
            var thread = new Thread(() =>
            {
                try
                {
                    var context = new AuthenticationContext(ConfigurationManager.AppSettings["ActiveDirectoryEndpoint"] + ConfigurationManager.AppSettings["ActiveDirectoryTenantId"]);

                    ClientCredential credential = new ClientCredential(ConfigurationManager.AppSettings["ApplicationId"], ConfigurationManager.AppSettings["Password"]);
                    result = context.AcquireToken(resource: ConfigurationManager.AppSettings["WindowsManagementUri"], clientCredential: credential);
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


15. Uitbreiden van het project (**DataFactoryAPITestApp**), met de rechtermuisknop op **References**in de Solution Explorer en klikt u op **Add Reference**. Schakel het selectievakje voor de assembly '**System.Configuration**' en klik op **OK**. 
16. Bouwen van de consoletoepassing. Klik op **Opbouwen** in het menu en klik op de **Oplossing bouwen**. 
16. Controleer of er ten minste één bestand in de container **adftutorial** in Azure blobopslag. Zo niet, **Emp.txt** bestand in Kladblok met de volgende inhoud maken en uploaden naar de container adftutorial.

        John, Doe
        Jane, Doe
     
17. Het programmeervoorbeeld uitvoeren door te klikken op **Debug** -> **Foutopsporing starten** in het menu. Wanneer u de **details van een segment gegevens ophalen uitgevoerd**, wacht een paar minuten en druk op **ENTER**. 
18. De Azure portal gebruiken om te controleren of de gegevens fabriek **APITutorialFactory** is gemaakt met de volgende onderdelen: 
    - Service gekoppeld: **LinkedService_AzureStorage** 
    - Gegevensset: **DatasetBlobSource** en **DatasetBlobDestination**.
    - Pijpleiding: **PipelineBlobSample** 
18. Controleer of dat de twee werknemerrecords in de tabel "**emp**" in de opgegeven Azure SQL-database worden gemaakt.

## <a name="next-steps"></a>Volgende stappen

- Lees artikel [Data verkeer activiteiten](data-factory-data-movement-activities.md) , biedt gedetailleerde informatie over de kopie activiteit die u hebt gebruikt in de zelfstudie.
- Zie [Data Factory.NET API Reference](https://msdn.microsoft.com/library/mt415893.aspx) voor meer informatie over Data Factory .NET SDK. In dit artikel geldt niet voor alle de Data Factory .NET API. 

 
