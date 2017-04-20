<properties
    pageTitle="Maak een elastische database toepassingen met C# | Microsoft Azure"
    description="Technieken van C#-database gebruik te maken van een schaalbare elastische database toepassingen in Azure SQL-Database, zodat u resources veel databases delen kunt."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="csharp"
    ms.workload="data-management"
    ms.date="10/04/2016"
    ms.author="sstein"/>

# <a name="create-an-elastic-database-pool-with-cx23"></a>Maak een elastische database toepassingen met C & #x 23;

> [AZURE.SELECTOR]
- [Azure portal](sql-database-elastic-pool-create-portal.md)
- [PowerShell](sql-database-elastic-pool-create-powershell.md)
- [C#](sql-database-elastic-pool-create-csharp.md)


In dit artikel wordt beschreven hoe u met C# een elastische database SQL Azure toepassingen maken met de [Azure SQL Database bibliotheek voor .NET](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql). Zie maken van een SQL-database zelfstandige [C# gebruiken voor het maken van een SQL-database met de SQL-Database-bibliotheek voor .NET](sql-database-get-started-csharp.md).

De bibliotheek Azure SQL Database voor .NET biedt een [Bronnenbeheerder Azure](../azure-resource-manager/resource-group-overview.md)-API die loopt van de [Resource Manager gebaseerde SQL Database REST API](https://msdn.microsoft.com/library/azure/mt163571.aspx)gebaseerd.

>[AZURE.NOTE] Veel nieuwe functies van SQL-Database worden alleen ondersteund wanneer u het [implementatiemodel Azure Resource Manager](../azure-resource-manager/resource-group-overview.md), zodat het gebruik altijd de meest recente **Azure SQL Database Management Library for .NET ([docs](https://msdn.microsoft.com/library/azure/mt349017.aspx) | [NuGet Package](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql))**. Het oudere [implementatiemodel klassieke op basis van bibliotheken](https://www.nuget.org/packages/Microsoft.WindowsAzure.Management.Sql) worden ondersteund voor achterwaartse compatibiliteit, zodat het raadzaam dat u de nieuwere Resource Manager op basis van bibliotheken gebruiken.

Als u de stappen in dit artikel hebt uitgevoerd, moet u het volgende:

- Een abonnement op Azure. Als u een abonnement op Azure gewoon **Gratis ACCOUNT** boven aan deze pagina op en teruggaan naar het voltooien van dit artikel.
- Visual Studio. Zie de pagina [Visual Studio Downloads](https://www.visualstudio.com/downloads/download-visual-studio-vs) voor een gratis exemplaar van Visual Studio.


## <a name="create-a-console-app-and-install-the-required-libraries"></a>Een console app maken en installeren van de vereiste bibliotheken.

1. Start Visual Studio.
2. Klik op **bestand** > **nieuwe** > **Project**.
3. Een C#- **Toepassing Console** maakt en deze de naam: *SqlElasticPoolConsoleApp*


Een SQL-database maken met C#, de vereiste management-bibliotheken (met de [package manager-console](http://docs.nuget.org/Consume/Package-Manager-Console)) te laden:

1. Klik op **Extra** > **NuGet Package Manager** > **Package Manager-Console**.
2. Type `Install-Package Microsoft.Azure.Management.Sql –Pre` te installeren van de [Bibliotheek van Microsoft Azure SQL Management](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql).
3. Type `Install-Package Microsoft.Azure.Management.ResourceManager –Pre` te installeren van de [Bibliotheek van Microsoft Azure Resource Manager](https://www.nuget.org/packages/Microsoft.Azure.Management.ResourceManager).
4. Type `Install-Package Microsoft.Azure.Common.Authentication –Pre` te installeren van de [Microsoft Azure algemene verificatie bibliotheek](https://www.nuget.org/packages/Microsoft.Azure.Common.Authentication). 



> [AZURE.NOTE] De voorbeelden in dit artikel een synchrone vorm van elke aanvraag API gebruiken en blokkeren totdat de voltooiing van de REST van de onderliggende service aanroepen. Er zijn asynchrone methoden.


## <a name="create-a-sql-elastic-database-pool---c-example"></a>Maak een groep met elastische database SQL - C#-voorbeeld

In het volgende voorbeeld maakt u een resourcegroep, server, firewallregel, elastische groep en maakt u een SQL-database in de groep. Zie, [een service principal toegang krijgen tot bronnen maken](#create-a-service-principal-to-access-resources) om de `_subscriptionId, _tenantId, _applicationId, and _applicationSecret` variabelen.

Update en de inhoud van de **Program.cs** vervangen door de volgende de `{variables}` met de waarden in de app (niet de `{}`).


```
using Microsoft.Azure;
using Microsoft.Azure.Management.ResourceManager;
using Microsoft.Azure.Management.ResourceManager.Models;
using Microsoft.Azure.Management.Sql;
using Microsoft.Azure.Management.Sql.Models;
using Microsoft.IdentityModel.Clients.ActiveDirectory;
using System;

namespace SqlElasticPoolConsoleApp
{
    class Program
        {

        // For details about these four (4) values, see
        // https://azure.microsoft.com/documentation/articles/resource-group-authenticate-service-principal/
        static string _subscriptionId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}";
        static string _tenantId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}";
        static string _applicationId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}";
        static string _applicationSecret = "{your-password}";

        // Create management clients for the Azure resources your app needs to work with.
        // This app works with Resource Groups, and Azure SQL Database.
        static ResourceManagementClient _resourceMgmtClient;
        static SqlManagementClient _sqlMgmtClient;

        // Authentication token
        static AuthenticationResult _token;

        // Azure resource variables
        static string _resourceGroupName = "{resource-group-name}";
        static string _resourceGrouplocation = "{Azure-region}";

        static string _serverlocation = _resourceGrouplocation;
        static string _serverName = "{server-name}";
        static string _serverAdmin = "{server-admin-login}";
        static string _serverAdminPassword = "{server-admin-password}";

        static string _firewallRuleName = "{firewall-rule-name}";
        static string _startIpAddress = "{0.0.0.0}";
        static string _endIpAddress = "{255.255.255.255}";

        static string _poolName = "{pool-name}";
        static string _poolEdition = "{Standard}";
        static int _poolDtus = {100};
        static int _databaseMinDtus = {10};
        static int _databaseMaxDtus = {100};

        static string _databaseName = "{elasticdbfromcsarticle}";



        static void Main(string[] args)
        {
            // Authenticate:
            _token = GetToken(_tenantId, _applicationId, _applicationSecret);
            Console.WriteLine("Token acquired. Expires on:" + _token.ExpiresOn);

            // Instantiate management clients:
            _resourceMgmtClient = new ResourceManagementClient(new Microsoft.Rest.TokenCredentials(_token.AccessToken));
            _sqlMgmtClient = new SqlManagementClient(new TokenCloudCredentials(_subscriptionId, _token.AccessToken));


            Console.WriteLine("Resource group...");
            ResourceGroup rg = CreateOrUpdateResourceGroup(_resourceMgmtClient, _subscriptionId, _resourceGroupName, _resourceGrouplocation);
            Console.WriteLine("Resource group: " + rg.Id);


            Console.WriteLine("Server...");
            ServerGetResponse sgr = CreateOrUpdateServer(_sqlMgmtClient, _resourceGroupName, _serverlocation, _serverName, _serverAdmin, _serverAdminPassword);
            Console.WriteLine("Server: " + sgr.Server.Id);

            Console.WriteLine("Server firewall...");
            FirewallRuleGetResponse fwr = CreateOrUpdateFirewallRule(_sqlMgmtClient, _resourceGroupName, _serverName, _firewallRuleName, _startIpAddress, _endIpAddress);
            Console.WriteLine("Server firewall: " + fwr.FirewallRule.Id);

            Console.WriteLine("Elastic pool...");
            ElasticPoolCreateOrUpdateResponse epr = CreateOrUpdateElasticDatabasePool(_sqlMgmtClient, _resourceGroupName, _serverName, _poolName, _poolEdition, _poolDtus, _databaseMinDtus, _databaseMaxDtus);
            Console.WriteLine("Elastic pool: " + epr.ElasticPool.Id);

            Console.WriteLine("Database...");
            DatabaseCreateOrUpdateResponse dbr = CreateOrUpdateDatabase(_sqlMgmtClient, _resourceGroupName, _serverName, _databaseName, _poolName);
            Console.WriteLine("Database: " + dbr.Database.Id);


            Console.WriteLine("Press any key to continue...");
            Console.ReadKey();
        }

        static ResourceGroup CreateOrUpdateResourceGroup(ResourceManagementClient resourceMgmtClient, string subscriptionId, string resourceGroupName, string resourceGroupLocation)
        {
            ResourceGroup resourceGroupParameters = new ResourceGroup()
            {
                Location = resourceGroupLocation,
            };
            resourceMgmtClient.SubscriptionId = subscriptionId;
            ResourceGroup resourceGroupResult = resourceMgmtClient.ResourceGroups.CreateOrUpdate(resourceGroupName, resourceGroupParameters);
            return resourceGroupResult;
        }

        static ServerGetResponse CreateOrUpdateServer(SqlManagementClient sqlMgmtClient, string resourceGroupName, string serverLocation, string serverName, string serverAdmin, string serverAdminPassword)
        {
            ServerCreateOrUpdateParameters serverParameters = new ServerCreateOrUpdateParameters()
            {
                Location = serverLocation,
                Properties = new ServerCreateOrUpdateProperties()
                {
                    AdministratorLogin = serverAdmin,
                    AdministratorLoginPassword = serverAdminPassword,
                    Version = "12.0"
                }
            };
            ServerGetResponse serverResult = sqlMgmtClient.Servers.CreateOrUpdate(resourceGroupName, serverName, serverParameters);
            return serverResult;
        }


        static FirewallRuleGetResponse CreateOrUpdateFirewallRule(SqlManagementClient sqlMgmtClient, string resourceGroupName, string serverName, string firewallRuleName, string startIpAddress, string endIpAddress)
        {
            FirewallRuleCreateOrUpdateParameters firewallParameters = new FirewallRuleCreateOrUpdateParameters()
            {
                Properties = new FirewallRuleCreateOrUpdateProperties()
                {
                    StartIpAddress = startIpAddress,
                    EndIpAddress = endIpAddress
                }
            };
            FirewallRuleGetResponse firewallResult = sqlMgmtClient.FirewallRules.CreateOrUpdate(resourceGroupName, serverName, firewallRuleName, firewallParameters);
            return firewallResult;
        }



        static ElasticPoolCreateOrUpdateResponse CreateOrUpdateElasticDatabasePool(SqlManagementClient sqlMgmtClient, string resourceGroupName, string serverName, string poolName, string poolEdition, int poolDtus, int databaseMinDtus, int databaseMaxDtus)
        {
            // Retrieve the server that will host this elastic pool
            Server currentServer = sqlMgmtClient.Servers.Get(resourceGroupName, serverName).Server;

            // Create elastic pool: configure create or update parameters and properties explicitly
            ElasticPoolCreateOrUpdateParameters newPoolParameters = new ElasticPoolCreateOrUpdateParameters()
            {
                Location = currentServer.Location,
                Properties = new ElasticPoolCreateOrUpdateProperties()
                {
                    Edition = poolEdition,
                    Dtu = poolDtus,
                    DatabaseDtuMin = databaseMinDtus,
                    DatabaseDtuMax = databaseMaxDtus
                }
            };

            // Create the pool
            var newPoolResponse = sqlMgmtClient.ElasticPools.CreateOrUpdate(resourceGroupName, serverName, poolName, newPoolParameters);
            return newPoolResponse;
        }




        static DatabaseCreateOrUpdateResponse CreateOrUpdateDatabase(SqlManagementClient sqlMgmtClient, string resourceGroupName, string serverName, string databaseName, string poolName)
        {
            // Retrieve the server that will host this database
            Server currentServer = sqlMgmtClient.Servers.Get(resourceGroupName, serverName).Server;

            // Create a database: configure create or update parameters and properties explicitly
            DatabaseCreateOrUpdateParameters newDatabaseParameters = new DatabaseCreateOrUpdateParameters()
            {
                Location = currentServer.Location,
                Properties = new DatabaseCreateOrUpdateProperties()
                {
                    CreateMode = DatabaseCreateMode.Default,
                    ElasticPoolName = poolName
                }
            };
            DatabaseCreateOrUpdateResponse dbResponse = sqlMgmtClient.Databases.CreateOrUpdate(resourceGroupName, serverName, databaseName, newDatabaseParameters);
            return dbResponse;
        }



        private static AuthenticationResult GetToken(string tenantId, string applicationId, string applicationSecret)
        {
            AuthenticationContext authContext = new AuthenticationContext("https://login.windows.net/" + tenantId);
            _token = authContext.AcquireToken("https://management.core.windows.net/", new ClientCredential(applicationId, applicationSecret));
            return _token;
        }
    }
}
```





## <a name="create-a-service-principal-to-access-resources"></a>Een service principal voor toegang tot bronnen te maken

De volgende PowerShell script maakt de toepassing van Active Directory (AD) en de service principal die we nodig hebben voor het verifiëren van de C#-app. Het script levert waarden die we nodig hebben om de voorgaande C#-voorbeeld. Zie voor gedetailleerde informatie [Gebruik Azure PowerShell om een service principal toegang tot bronnen te maken](../resource-group-authenticate-service-principal.md).

   
    # Sign in to Azure.
    Add-AzureRmAccount
    
    # If you have multiple subscriptions, uncomment and set to the subscription you want to work with.
    #$subscriptionId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}"
    #Set-AzureRmContext -SubscriptionId $subscriptionId
    
    # Provide these values for your new AAD app.
    # $appName is the display name for your app, must be unique in your directory.
    # $uri does not need to be a real uri.
    # $secret is a password you create.
    
    $appName = "{app-name}"
    $uri = "http://{app-name}"
    $secret = "{app-password}"
    
    # Create a AAD app
    $azureAdApplication = New-AzureRmADApplication -DisplayName $appName -HomePage $Uri -IdentifierUris $Uri -Password $secret
    
    # Create a Service Principal for the app
    $svcprincipal = New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId
    
    # To avoid a PrincipalNotFound error, I pause here for 15 seconds.
    Start-Sleep -s 15
    
    # If you still get a PrincipalNotFound error, then rerun the following until successful. 
    $roleassignment = New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $azureAdApplication.ApplicationId.Guid
    
    
    # Output the values we need for our C# application to successfully authenticate
    
    Write-Output "Copy these values into the C# sample app"
    
    Write-Output "_subscriptionId:" (Get-AzureRmContext).Subscription.SubscriptionId
    Write-Output "_tenantId:" (Get-AzureRmContext).Tenant.TenantId
    Write-Output "_applicationId:" $azureAdApplication.ApplicationId.Guid
    Write-Output "_applicationSecret:" $secret


  

## <a name="next-steps"></a>Volgende stappen

- [Uw groep beheren](sql-database-elastic-pool-manage-csharp.md)
- [Elastische taken maken](sql-database-elastic-jobs-overview.md): elastische taken kunnen u T-SQL-scripts uitvoeren op een onbeperkt aantal databases in een groep.
- [Geschaald uitbreiden met Azure SQL-Database](sql-database-elastic-scale-introduction.md): elastische Databasehulpprogramma's gebruiken voor het schalen van.

## <a name="additional-resources"></a>Aanvullende bronnen

- [SQL-Database](https://azure.microsoft.com/documentation/services/sql-database/)
- [Azure Resource Management API 's](https://msdn.microsoft.com/library/azure/dn948464.aspx)
