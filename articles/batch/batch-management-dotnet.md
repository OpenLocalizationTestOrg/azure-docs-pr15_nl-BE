<properties
    pageTitle="Account Resourcemanagement met Batch Management .NET | Microsoft Azure"
    description="Maken, verwijderen en wijzigen van Azure Batch account resources met de Batch Management .NET library."
    services="batch"
    documentationCenter=".net"
    authors="mmacy"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="batch"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows"
    ms.workload="big-compute"
    ms.date="10/19/2016"
    ms.author="marsma"/>

# <a name="manage-azure-batch-accounts-and-quotas-with-batch-management-net"></a>Azure Batch accounts en quota met Batch Management .NET beheren

> [AZURE.SELECTOR]
- [Azure portal](batch-account-create-portal.md)
- [Batchbeheer .NET](batch-management-dotnet.md)

U kunt verlagen onderhoud overhead in Azure Batch toepassingen met behulp van de [Batch Management.NET] [ api_mgmt_net] bibliotheek automatiseren Batch-account maken, verwijderen, sleutelbeheer en detectie van quota.

- **Maken en verwijderen van accounts Batch** in elke regio. Als, als een onafhankelijke softwareleverancier (ISV) bijvoorbeeld u een service voor uw klanten die elk een afzonderlijke rekening voor Batch voor facturatie is toegewezen, kunt u mogelijkheden voor het maken en verwijderen van account toevoegen aan de portal voor klanten.
- **Ophalen en sleutels opnieuw genereren account** via een programma voor uw Batch-accounts. Dit kan helpen u bij het beveiligingsbeleid dat periodieke rollover of het verstrijken van de sleutels van de account afdwingen. Als u meerdere accounts voor Batch in verschillende gebieden van Azure, verhoogt automatisering van het rollover-proces de efficiëntie van uw oplossing.
- **Account quota** en nemen de verschillende vallen en opstaan om te bepalen welke accounts Batch welke grenzen hebben. Toepassingen maken of computerknooppunten toe te voegen, kunt u proactief waar aanpassen door uw account quota's controleren voordat u begint met taken, of bij het berekenen van deze resources worden gemaakt. U kunt bepalen welke accounts vereisen verhoogt de quota voor het toewijzen van extra middelen die accounts.
- **Functies van andere Azure services combineren** voor een complete management ervaring--met behulp van Batch Management .NET, [Azure Active Directory][aad_about], en de [Azure Resource Manager] [ resman_overview] samen in dezelfde toepassing. Deze functies en hun API's gebruikt, kunt u een verificatie frictionless ervaring, de mogelijkheid om te maken en verwijderen van bronnengroepen en de mogelijkheden die hierboven is beschreven voor een end-to-end oplossing bieden.

> [AZURE.NOTE] Hoewel dit artikel is gericht op het programmatische beheer van uw rekeningen Batch, sleutels en quota's, kunt u veel van deze activiteiten uitvoeren met behulp van de [portal Azure][azure_portal]. Zie [een Batch Azure-account maken met behulp van de portal Azure](batch-account-create-portal.md) en [quota en beperkingen voor de Batch Azure-service](batch-quota-limit.md)voor meer informatie.

## <a name="create-and-delete-batch-accounts"></a>Batch-accounts maken en verwijderen

Zoals gezegd, is een van de primaire functies van de API Batch Management Batchaccounts in een Azure regio maken en verwijderen. Gebruik hiervoor [BatchManagementClient.Account.CreateAsync] [ net_create] en [DeleteAsync][net_delete], of hun synchrone tegenhangers.

Het volgende codefragment wordt een account gemaakt, krijgt de nieuwe account van de Batch-service en wordt vervolgens verwijderd. In dit fragment en de andere in dit artikel, `batchManagementClient` is een volledig geïnitialiseerd exemplaar van [BatchManagementClient][net_mgmt_client].

```csharp
// Create a new Batch account
await batchManagementClient.Account.CreateAsync("MyResourceGroup",
    "mynewaccount",
    new BatchAccountCreateParameters() { Location = "West US" });

// Get the new account from the Batch service
AccountResource account = await batchManagementClient.Account.GetAsync(
    "MyResourceGroup",
    "mynewaccount");

// Delete the account
await batchManagementClient.Account.DeleteAsync("MyResourceGroup", account.Name);
```

> [AZURE.NOTE] Toepassingen die gebruikmaken van de bibliotheek Batch Management .NET en de BatchManagementClient klasse **servicebeheerder** of **coadministrator** toegang nodig tot het abonnement dat eigenaar is van de Batch-account te beheren. Zie voor meer informatie de sectie [Azure Active Directory](#azure-active-directory) en het [AccountManagement] [ acct_mgmt_sample] codevoorbeeld.

## <a name="retrieve-and-regenerate-account-keys"></a>Ophalen en opnieuw genereren van sleutels voor account

Primaire en secundaire sleutels van een Batch-account in uw abonnement verkrijgen met behulp van [ListKeysAsync][net_list_keys]. Kunt u deze sleutels opnieuw genereren met behulp van [RegenerateKeyAsync][net_regenerate_keys].

```csharp
// Get and print the primary and secondary keys
BatchAccountListKeyResult accountKeys =
    await batchManagementClient.Account.ListKeysAsync(
        "MyResourceGroup",
        "mybatchaccount");
Console.WriteLine("Primary key:   {0}", accountKeys.Primary);
Console.WriteLine("Secondary key: {0}", accountKeys.Secondary);

// Regenerate the primary key
BatchAccountRegenerateKeyResponse newKeys =
    await batchManagementClient.Account.RegenerateKeyAsync(
        "MyResourceGroup",
        "mybatchaccount",
        new BatchAccountRegenerateKeyParameters() {
            KeyName = AccountKeyType.Primary
            });
```

> [AZURE.TIP] Kunt u een verbinding met gestroomlijnde workflow voor uw toepassingen. Eerst moet u een account-toets voor de Batch-account die u wilt beheren met [ListKeysAsync][net_list_keys]. Vervolgens gebruikt u deze sleutel bij het initialiseren van de bibliotheek Batch .NET [BatchSharedKeyCredentials] [ net_sharedkeycred] klasse die wordt gebruikt bij het initialiseren van de [BatchClient][net_batch_client].

## <a name="check-azure-subscription-and-batch-account-quotas"></a>Azure abonnement en quota voor Batch-account controleren

Azure abonnementen en afzonderlijke Azure services zoals Batch alle standaard quota's hebt die het maximum van bepaalde diensten in deze. Zie voor de quota van de standaardinstelling voor abonnementen Azure, [Azure abonnement service grenzen, quota's en beperkingen](./../azure-subscription-service-limits.md). Zie voor de standaardquota van de Batch-service, [quota en beperkingen voor de Batch Azure service](batch-quota-limit.md). Met behulp van de Batch Management .NET-bibliotheek, kunt u deze quota in uw toepassingen te controleren. Hiermee kunt u de toewijzing om beslissingen te nemen voordat u accounts toevoegen of berekenen van bronnen zoals toepassingen en nodes COMPUTE.

### <a name="check-an-azure-subscription-for-batch-account-quotas"></a>Een Azure-abonnement voor Batch-account controleren

Voordat u een Batch-account in een regio, kunt u controleren uw Azure abonnement om te zien of u kunt een account toevoegen in die regio zijn.

In het onderstaande stukje code gebruiken we eerst [BatchManagementClient.Account.ListAsync] [ net_mgmt_listaccounts] om een collectie van alle rekeningen van Batch die binnen een abonnement. Als we deze collectie hebt verkregen, bepalen we hoeveel accounts zijn in de doelregio. Vervolgens we [BatchManagementClient.Subscriptions gebruiken] [ net_mgmt_subscriptions] het quotum Batch account ophalen en bepalen hoeveel accounts (indien aanwezig) kunnen worden gemaakt in die regio.

```csharp
// Get a collection of all Batch accounts within the subscription
BatchAccountListResponse listResponse =
        await batchManagementClient.Account.ListAsync(new AccountListParameters());
IList<AccountResource> accounts = listResponse.Accounts;
Console.WriteLine("Total number of Batch accounts under subscription id {0}:  {1}",
    creds.SubscriptionId,
    accounts.Count);

// Get a count of all accounts within the target region
string region = "westus";
int accountsInRegion = accounts.Count(o => o.Location == region);

// Get the account quota for the specified region
SubscriptionQuotasGetResponse quotaResponse = await batchManagementClient.Subscriptions.GetSubscriptionQuotasAsync(region);
Console.WriteLine("Account quota for {0} region: {1}", region, quotaResponse.AccountQuota);

// Determine how many accounts can be created in the target region
Console.WriteLine("Accounts in {0}: {1}", region, accountsInRegion);
Console.WriteLine("You can create {0} accounts in the {1} region.", quotaResponse.AccountQuota - accountsInRegion, region);
```

In het bovenstaande fragment `creds` is een exemplaar van [TokenCloudCredentials][azure_tokencreds]. Een voorbeeld van het maken van dit object, Zie het [AccountManagement] [ acct_mgmt_sample] codevoorbeeld op GitHub.

### <a name="check-a-batch-account-for-compute-resource-quotas"></a>Een Batch-account voor compute resource quota's controleren

Voordat u voor de resources in uw oplossing Batch compute, kunt u controleren om ervoor te zorgen de resources die u wilt toewijzen aan de quota van de account won't overschrijden. In het onderstaande stukje code, drukken we de quotumgegevens voor de Batch-account met de naam `mybatchaccount`. In uw eigen toepassing, kunt u deze informatie gebruiken om te bepalen of de aanvullende middelen moeten worden gemaakt kan worden verwerkt door de account.

```csharp
// First obtain the Batch account
BatchAccountGetResponse getResponse =
    await batchManagementClient.Account.GetAsync("MyResourceGroup", "mybatchaccount");
AccountResource account = getResponse.Resource;

// Now print the compute resource quotas for the account
Console.WriteLine("Core quota: {0}", account.Properties.CoreQuota);
Console.WriteLine("Pool quota: {0}", account.Properties.PoolQuota);
Console.WriteLine("Active job and job schedule quota: {0}", account.Properties.ActiveJobAndJobScheduleQuota);
```

> [AZURE.IMPORTANT] Er zijn standaardquota voor Azure abonnementen en diensten, veel van deze beperkingen kunnen worden verhoogd door afgifte van een aanvraag in de [Azure portal][azure_portal]. [Quota's en beperkingen voor de Batch Azure service](batch-quota-limit.md) bijvoorbeeld Zie voor instructies voor het verhogen van uw quota's Batch-account.

## <a name="batch-management-net-azure-ad-and-resource-manager"></a>Batch-beheer .NET, Azure AD, en Resource Manager

Als u met de bibliotheek Batch Management .NET werkt, u meestal ook [Azure Active Directory] gebruikt[ aad_about] (Azure AD) en de [Azure Resource Manager][resman_overview]. Hieronder besproken gebruikt in het voorbeeldproject Azure Active Directory en van de bronnenbeheerder tijdens de Batch Management .NET API laat.

### <a name="azure-active-directory"></a>Azure Active Directory

Azure zelf Azure AD wordt gebruikt voor de verificatie van de klanten, servicebeheerders en organisatie-gebruikers. In de context van Batch Management .NET met Azure AD kunt u een abonnement-beheerder of beheerder collega verifiëren. Hierdoor kan de bibliotheek management de Batch service opvragen en de bewerkingen die in dit artikel wordt beschreven.

In het voorbeeldproject besproken onder de Azure [Active Directory-verificatie Library] [ aad_adal] (ADAL) wordt gebruikt voor de gebruiker vragen om hun referenties van Microsoft. Als een beheerder of een coadministrator-service de referenties worden geleverd, kunt de toepassing Azure een query voor een overzicht van de abonnementen-- en kunt maken en resourcegroepen en de Batchaccounts verwijderen.

### <a name="resource-manager"></a>Resource Manager

Wanneer u een Batchaccounts met de bibliotheek Batch Management .NET maakt, meestal maakt u deze in een [resourcegroep][resman_overview]. U kunt de resourcegroep via programmering maken met behulp van de [ResourceManagementClient] [ resman_client] klasse in het [.NET Resource Manager] [ resman_api] bibliotheek. Of u kunt een account toevoegen aan een bestaande resourcegroep die u eerder via de [Azure portal][azure_portal].

## <a name="sample-project-on-github"></a>Voorbeeldproject op GitHub

Uitchecken als Batch Management .NET in actie zien, de [AccountManagment] [ acct_mgmt_sample] voorbeeldproject op GitHub. Deze consoletoepassing ziet u het maken en het gebruik van [BatchManagementClient] [ net_mgmt_client] en [ResourceManagementClient][resman_client]. U ziet ook het gebruik van de Azure [Active Directory-verificatie Library] [ aad_adal] (ADAL), die door beide clients is vereist.

De voorbeeldtoepassing is uitgevoerd, moet u eerst registreren deze met Azure Active Directory met behulp van de portal Azure. Volg de stappen in de sectie [een toepassing toe te voegen](../active-directory/active-directory-integrating-applications.md#adding-an-application) in [toepassingen voor integratie met Active Directory Azure] [ aad_integrate] de Default Directory voor het registreren van de voorbeeldtoepassing binnen uw eigen account. Selecteer **Native clienttoepassing** voor de toepassing en kunt u geldige URI (zoals `http://myaccountmanagementsample`) voor het **Omleiden van URI**--niet hoeft te worden van een echte eindpunt.

Na het toevoegen van uw toepassing gemachtigde de machtiging **Toegang Azure Service Management als organisatie** voor de toepassing *Windows Azure Service Management API* in de instellingen van de toepassing in de portal:

![Machtigingen voor een toepassing in Azure portal][2]

> [AZURE.TIP] Als **Windows Azure Service Management API** niet onder *machtigingen voor andere toepassingen*, klik op **Add application** **Windows Azure Service Management API**, dan klikt u op de knop ingeschakeld. Vervolgens overdragen van machtigingen zoals hierboven.

Zodra u hebt de toepassing toegevoegd, zoals hierboven beschreven, werken `Program.cs` in de [AccountManagment] [ acct_mgmt_sample] voorbeeldproject met de Redirect URI van de toepassing en de Client-ID. Deze waarden vindt u in het tabblad **configureren** van uw toepassing:

![Toepassingsconfiguratie in Azure portal][3]

De [AccountManagment] [ acct_mgmt_sample] voorbeeldtoepassing toont de volgende bewerkingen:

1. Een beveiligingssleutel uit Azure Active Directory met behulp van [ADAL]aanschaffen[aad_adal]. Als de gebruiker is nog niet aangemeld, wordt gevraagd of u hun referenties Azure.
2. Maak een [SubscriptionClient] met behulp van het beveiligingstoken verkregen van Azure AD[ resman_subclient] query Azure voor een overzicht van abonnementen die zijn gekoppeld aan de account. Hierdoor kan de selectie van een abonnement als er meerdere zijn gevonden.
3. Referenties-object dat is gekoppeld aan het geselecteerde abonnement maken.
4. Maak [ResourceManagementClient] [ resman_client] met behulp van de referenties.
5. Gebruik [ResourceManagementClient] [ resman_client] voor het maken van een resourcegroep.
6. Gebruik [BatchManagementClient] [ net_mgmt_client] voor batchbewerkingen verschillende account:
  - Een Batch-account in de nieuwe resourcegroep maken.
  - De nieuwe account ophalen van de Batch-service.
  - Afdrukken van de sleutels van de rekening voor de nieuwe account.
  - Een nieuwe primaire sleutel voor de account opnieuw te genereren.
  - De quotumgegevens voor het afdrukken.
  - Afdrukken van de quotumgegevens voor het abonnement.
  - Afdrukken van alle rekeningen in het abonnement.
  - Nieuwe account verwijderen.
7. De resourcegroep verwijderen.

Voordat u de nieuwe Batch account- en groep verwijdert, kunt u controleren op de [Azure portal][azure_portal]:

![Azure portal waarbij de bron- en Batch-account][1]
<br />
*Azure portal weergeven, nieuwe resourcegroep en Batch-account*

[aad_about]: ../active-directory/active-directory-whatis.md "Wat is Azure Active Directory?"
[aad_adal]: ../active-directory/active-directory-authentication-libraries.md
[aad_auth_scenarios]: ../active-directory/active-directory-authentication-scenarios.md "Verificatie-scenario's voor Azure AD"
[aad_integrate]: ../active-directory/active-directory-integrating-applications.md "Integratie van toepassingen met Azure Active Directory"
[acct_mgmt_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/AccountManagement
[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_mgmt_net]: https://msdn.microsoft.com/library/azure/mt463120.aspx
[azure_portal]: http://portal.azure.com
[azure_storage]: https://azure.microsoft.com/services/storage/
[azure_tokencreds]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.tokencloudcredentials.aspx
[batch_explorer_project]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[net_batch_client]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_list_keys]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.accountoperationsextensions.listkeysasync.aspx
[net_create]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.accountoperationsextensions.createasync.aspx
[net_delete]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.accountoperationsextensions.deleteasync.aspx
[net_regenerate_keys]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.accountoperationsextensions.regeneratekeyasync.aspx
[net_sharedkeycred]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.auth.batchsharedkeycredentials.aspx
[net_mgmt_client]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.batchmanagementclient.aspx
[net_mgmt_subscriptions]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.batchmanagementclient.subscriptions.aspx
[net_mgmt_listaccounts]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.iaccountoperations.listasync.aspx
[resman_api]: https://msdn.microsoft.com/library/azure/mt418626.aspx
[resman_client]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.resources.resourcemanagementclient.aspxs
[resman_subclient]: https://msdn.microsoft.com/library/azure/microsoft.azure.subscriptions.subscriptionclient.aspx
[resman_overview]: ../azure-resource-manager/resource-group-overview.md

[1]: ./media/batch-management-dotnet/portal-01.png
[2]: ./media/batch-management-dotnet/portal-02.png
[3]: ./media/batch-management-dotnet/portal-03.png
