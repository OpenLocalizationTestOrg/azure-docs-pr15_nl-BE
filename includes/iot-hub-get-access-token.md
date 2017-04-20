## <a name="obtain-an-azure-resource-manager-token"></a>Een bronnenbeheerder Azure-token verkrijgen

Azure Active Directory moet worden geverifieerd door de taken die u met bronbeheer Azure bronnen uitvoeren. Het voorbeeld maakt gebruik van wachtwoordverificatie, voor andere benaderingen Zie [Bronbeheer Azure verificatie aanvragen][lnk-authenticate-arm].

1. Voeg de volgende code de methode **Main** in Program.cs een token ophalen van Azure AD de toepassings-id en wachtwoord.

    ```
    var authContext = new AuthenticationContext(string.Format  
      ("https://login.windows.net/{0}", tenantId));
    var credential = new ClientCredential(applicationId, password);
    AuthenticationResult token = authContext.AcquireTokenAsync
      ("https://management.core.windows.net/", credential).Result;
    
    if (token == null)
    {
      Console.WriteLine("Failed to obtain the token");
      return;
    }
    ```

2. Maak een **ResourceManagementClient** -object dat de token wordt gebruikt door de volgende code toe te voegen aan het einde van de **Main** -methode:

    ```
    var creds = new TokenCredentials(token.AccessToken);
    var client = new ResourceManagementClient(creds);
    client.SubscriptionId = subscriptionId;
    ```

3. Maken, of een verwijzing naar de resourcegroep die u gebruikt:

    ```
    var rgResponse = client.ResourceGroups.CreateOrUpdate(rgName,
        new ResourceGroup("East US"));
    if (rgResponse.Properties.ProvisioningState != "Succeeded")
    {
      Console.WriteLine("Problem creating resource group");
      return;
    }
    ```

[lnk-authenticate-arm]: https://msdn.microsoft.com/library/azure/dn790557.aspx