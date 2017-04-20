<properties 
    pageTitle="Met behulp van .NET SDK toegang te krijgen tot Azure Mobile Engagement API 's" 
    description="Wordt beschreven hoe u de mobiele betrokkenheid bij de .NET SDK toegang te krijgen tot Azure Mobile Engagement API 's"        
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="erikre" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-multiple" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="using-net-sdk-to-access-azure-mobile-engagement-service-apis"></a>Met behulp van .NET SDK toegang te krijgen tot Azure Mobile Engagement API 's

Azure Mobile Engagement beschrijft een set API's voor het beheer van apparaten, bereik/Push campagnes, enz. Om te kunnen communiceren met deze API's, bieden wij ook u een [Swagger-bestand](https://github.com/Azure/azure-rest-api-specs/blob/master/arm-mobileengagement/2014-12-01/swagger/mobile-engagement.json) die u met extra gebruiken kunt SDK's genereren voor uw voorkeurstaal. U wordt aangeraden de SDK van onze Swagger-bestand genereren met het hulpprogramma [AutoRest](https://github.com/Azure/AutoRest) . 

Hebben wij een .NET SDK op een vergelijkbare manier kunt u met deze API's communiceren met een C# wrapper en u hoeft de verificatie token onderhandeling en zelf vernieuwen.  

In dit voorbeeld wordt de reeks stappen te volgen voor het gebruik van de .NET SDK:

1. Ten eerste moet u de verificatie voor de API's met Azure Active Directory als beschreven [hier](mobile-engagement-api-authentication.md#authentication)instellen. Aan het einde van deze stappen hebt u een geldig **SubscriptionId**, **TenantId**, **ApplicationId** en **geheim**. 

2. We gebruiken een eenvoudige Windows-Console app om te werken met de SDK voor .NET met het scenario voor het maken van een aankondiging campagne tonen. Zo opent u Visual Studio en een **Consoletoepassing**maken.   

3. Vervolgens dient u de .NET SDK is beschikbaar als **Microsoft Azure Engagement Management Library** in de galerie Nuget downloaden [hier](https://www.nuget.org/packages/Microsoft.Azure.Management.Engagement/).
Als u de Nuget van Visual Studio installeert, moet u ervoor zorgen dat u beschikt over de optie **inclusief de voorlopige versie** bij het zoeken naar het pakket ingeschakeld:

    ![][1]

4. In de `Program.cs` het bestand, voeg de volgende naamruimten:

        using Microsoft.Rest.Azure.Authentication;
        using Microsoft.Azure.Management.Engagement;
        using Microsoft.Azure.Management.Engagement.Models;

5. Vervolgens moet u de volgende constanten die worden gebruikt voor verificatie en interactie met de Mobile Engagement App waarin u de campagne aankondiging maakt definiëren:

        // For authentication
        const string TENANT_ID = "<Your TenantId>";
        const string CLIENT_ID = "<Your Application Id>";
        const string CLIENT_SECRET = "<Your Secret>";
        const string SUBSCRIPTION_ID = "<Your Subscription Id>";

        // This is the Azure Resource group concept for grouping together resources 
        //  see here: https://azure.microsoft.com/en-us/documentation/articles/resource-group-portal/
        const string RESOURCE_GROUP = "";

        // For Mobile Engagement operations
        // App Collection Name 
        const string APP_COLLECTION_NAME = "";
        // Application Resource Name - make sure you are using the one as specified in the Azure portal (NOT the App Name)
        const string APP_RESOURCE_NAME = "";

6. Definieer de `EngagementManagementClient` -variabele die met de SDK van Mobile Engagement methoden aanroepen:

        static EngagementManagementClient engagementClient; 

7. Voeg de volgende aan uw `Main` methode:

        try
            {
                // Initialize the Engagement SDK to call out APIs. 
                InitEngagementClient().Wait();

                // Create a Reach campaign
                CreateCampaign().Wait();
            }
            catch (Exception ex)
            {
                Console.WriteLine(ex.InnerException.Message);
                throw ex;
            }

8. Definieer de volgende methode die voor het initialiseren van zorgt de `EngagementManagementClient` door eerst te verifiëren en vervolgens zelf te koppelen met de mobiele Engagement App waarin u van plan bent om de campagne aankondiging te maken:

        private static async Task InitEngagementClient()
        {
            var credentials = await ApplicationTokenProvider.LoginSilentAsync(TENANT_ID, CLIENT_ID, CLIENT_SECRET);
            engagementClient = new EngagementManagementClient(credentials) { SubscriptionId = SUBSCRIPTION_ID };
            
            // This is the Azure concept of ResourceGroup
            engagementClient.ResourceGroupName = RESOURCE_GROUP;

            // This is your Mobile Engagement App Collection & App Resource Name in which you create the campaign
            engagementClient.AppCollection = APP_COLLECTION_NAME;
            engagementClient.AppName = APP_RESOURCE_NAME;
        }

    > [AZURE.IMPORTANT] Opmerking u moet de **Naam van de Resource App** gebruikt zoals gedefinieerd in de portal Azure management voor de parameter AppName. 

9. Ten slotte, definiëren de CreateCampaign-methode die wordt gedaan met behulp van de EngagementClient eerder is geïnitialiseerd voor het maken van een eenvoudig **op elk gewenst moment** & **melding alleen** campagne met een titel en een bericht: 

        private async static Task CreateCampaign()
        {
            //  Refer to the Announcement Campaign format from here - 
            //      https://msdn.microsoft.com/en-us/library/azure/mt683751.aspx
            // Make sure you are passing all the non-optional parameters
            Campaign parameters = new Campaign(
                name:"WelcomeCampaign",
                notificationTitle: "Welcome", 
                notificationMessage: "Thank you for installing the app!",
                type:"only_notif",
                deliveryTime:"any"
                );

            // Refer to the Campaign Kinds from here - https://msdn.microsoft.com/en-us/library/azure/mt683742.aspx
            CampaignStateResult result = 
                await engagementClient.Campaigns.CreateAsync(CampaignKinds.Announcements, parameters);
            Console.WriteLine("Campaign Id '{0}' was created successfully and it is in '{1}' state", result.Id, result.State);
        }

10. De app console worden uitgevoerd en ziet u het volgende op de installatie van de campagne:

    **Campagne-Id '159' is gemaakt en is in staat 'Concept'**

<!-- Images. -->

[1]: ./media/mobile-engagement-dotnet-sdk-service-api/include-prerelease.png
