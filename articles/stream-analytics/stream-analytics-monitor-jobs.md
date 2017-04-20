<properties
    pageTitle="programmatisch controleren projecten in stroom Analytics | Microsoft Azure"
    description="Informatie over het programmatisch controleren stroom Analytics taken gemaakt via een REST API's of SDK Azure Powershell."
    keywords=".NET-monitor, job controle, controleren app"
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


# <a name="programmatically-create-a-stream-analytics-job-monitor"></a>Een taak Stream Analytics monitor via programmering maken
 Dit artikel wordt beschreven hoe u kunt controle voor een Stream Analytics-functie inschakelen. Stream Analytics gemaakt via een REST API's of SDK Azure Powershell taken hebben controle standaard niet ingeschakeld.  Handmatig kunt u dit in het Portal Azure door te navigeren naar de pagina van de Monitor van de taak en klikt u op de knop inschakelen, of u kunt dit proces automatiseren door de stappen in dit artikel. De controlegegevens worden weergegeven in het tabblad 'Monitor' in Azure Portal voor uw project Stream Analytics.

![Job-monitor tabblad taken](./media/stream-analytics-monitor-jobs/stream-analytics-monitor-jobs-tab.png)

## <a name="prerequisites"></a>Vereisten
Voordat u dit artikel hebt u het volgende:

- Visual Studio 2012 of 2013.
- Download en installeer [Azure.NET SDK](https://azure.microsoft.com/downloads/).
- Een bestaande stroom Analytics taak die controle ingeschakeld moet.

## <a name="setup-a-project"></a>Een project instellen

1.  Maak een consoletoepassing Visual Studio C# .net.
2.  Voer de volgende opdrachten de NuGet pakketten installeren in de Package Manager-Console. De eerste is de Azure Stream Analytics beheer .NET SDK. Het tweede is de Azure Monitor SDK die wordt gebruikt voor het inschakelen van controle. Laatste is de Azure Active Directory-client die wordt gebruikt voor verificatie.

    ```
    Install-Package Microsoft.Azure.Management.StreamAnalytics
    Install-Package Microsoft.Azure.Insights -Pre
    Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
    ```

3.  De volgende sectie appSettings toevoegen aan het bestand App.config.

    ```
    <appSettings>
        <!--CSM Prod related values-->
        <add key="ResourceGroupName" value="RESOURCE GROUP NAME" />
        <add key="JobName" value="YOUR JOB NAME" />
        <add key="StorageAccountName" value="YOUR STORAGE ACCOUNT"/>
        <add key="ActiveDirectoryEndpoint" value="https://login.windows.net/" />
        <add key="ResourceManagerEndpoint" value="https://management.azure.com/" />
        <add key="WindowsManagementUri" value="https://management.core.windows.net/" />
        <add key="AsaClientId" value="1950a258-227b-4e31-a9cf-717495945fc2" />
        <add key="RedirectUri" value="urn:ietf:wg:oauth:2.0:oob" />
        <add key="SubscriptionId" value="YOUR AZURE SUBSCRIPTION ID" />
        <add key="ActiveDirectoryTenantId" value="YOUR TENANT ID" />
    </appSettings>
    ```
Waarden voor *SubscriptionId* en *ActiveDirectoryTenantId* vervangen door uw abonnement Azure en id's pachters. U kunt deze waarden verkrijgen door de volgende PowerShell-cmdlet wordt uitgevoerd:

    ```
    Get-AzureAccount
    ```
4.  Voeg de volgende overzichten met het bronbestand (Program.cs) in het project.

    ```
        using System;
        using System.Configuration;
        using System.Threading;
        using Microsoft.Azure;
        using Microsoft.Azure.Management.Insights;
        using Microsoft.Azure.Management.Insights.Models;
        using Microsoft.Azure.Management.StreamAnalytics;
        using Microsoft.Azure.Management.StreamAnalytics.Models;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
    ```
5.  Een helper verificatiemethode toevoegen.

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

## <a name="create-management-clients"></a>Beheer van Clients maken
De volgende code stelt de nodige variabelen en beheer van clients.

    string resourceGroupName = "<YOUR AZURE RESOURCE GROUP NAME>";
    string streamAnalyticsJobName = "<YOUR STREAM ANALYTICS JOB NAME>";

    // Get authentication token
    TokenCloudCredentials aadTokenCredentials =
        new TokenCloudCredentials(
            ConfigurationManager.AppSettings["SubscriptionId"],
            GetAuthorizationHeader());

    Uri resourceManagerUri = new
    Uri(ConfigurationManager.AppSettings["ResourceManagerEndpoint"]);

    // Create Stream Analytics and Insights management client
    StreamAnalyticsManagementClient streamAnalyticsClient = new
    StreamAnalyticsManagementClient(aadTokenCredentials, resourceManagerUri);
    InsightsManagementClient insightsClient = new
    InsightsManagementClient(aadTokenCredentials, resourceManagerUri);

## <a name="enable-monitoring-for-an-existing-stream-analytics-job"></a>Controle voor een bestaande stroom Analytics-functie inschakelen

De volgende code wordt voor een **bestaand** Analytics Stream-project-controle inschakelen. Het eerste deel van de code uitvoert een GET-verzoek tegen de stroom Analytics-service voor het ophalen van informatie over de specifieke stroom Analytics taak. Wordt de eigenschap 'Id' (opgehaald uit het GET-verzoek) als een parameter voor de methode Put in de tweede helft van de code waarmee een PUT verzonden verzoek aan de service inzichten voor het project Stream Analytics-controle inschakelen.

> [AZURE.WARNING]
> Als u eerder hebt ingeschakeld voor een andere baan Analytics stroom via de Portal Azure of via programmacode via monitoring de onderstaande code, **is het aanbevolen om dezelfde opslag accountnaam toen u eerder controle ingeschakeld.**
>
> De opslag-account is gekoppeld aan de regio die u hebt gemaakt met uw Analytics Stream-project in, niet specifiek voor de taak zelf.
>
> Alle stroom Analytics taak (en andere Azure bronnen) in dat gebied dezelfde delen deze opslag account controlegegevens opslaan. Als u een andere opslag-account, wordt ongewenste neveneffecten voor de bewaking van de andere taken van de Stream Analytics en/of andere bronnen Azure.
>
> De naam van de opslag worden vervangen ```“<YOUR STORAGE ACCOUNT NAME>”``` hieronder moet een opslag-account die in het abonnement op dezelfde is als de stroom Analytics taak u wilt inschakelen voor controle.

    // Get an existing Stream Analytics job
    JobGetParameters jobGetParameters = new JobGetParameters()
    {
        PropertiesToExpand = "inputs,transformation,outputs"
    };
    JobGetResponse jobGetResponse = streamAnalyticsClient.StreamingJobs.Get(resourceGroupName, streamAnalyticsJobName, jobGetParameters);

    // Enable monitoring
    ServiceDiagnosticSettingsPutParameters insightPutParameters = new ServiceDiagnosticSettingsPutParameters()
    {
            Properties = new ServiceDiagnosticSettings()
            {
                StorageAccountName = "<YOUR STORAGE ACCOUNT NAME>"
            }
    };
    insightsClient.ServiceDiagnosticSettingsOperations.Put(jobGetResponse.Job.Id, insightPutParameters);



## <a name="get-support"></a>Ondersteuning krijgen
Probeer onze [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)voor verdere ondersteuning.


## <a name="next-steps"></a>Volgende stappen

- [Inleiding tot Analytics Azure Stream](stream-analytics-introduction.md)
- [Aan de slag met Azure Stream Analytics](stream-analytics-get-started.md)
- [Schaal Azure Stream Analytics taken](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics beheer REST API: naslag](https://msdn.microsoft.com/library/azure/dn835031.aspx)
