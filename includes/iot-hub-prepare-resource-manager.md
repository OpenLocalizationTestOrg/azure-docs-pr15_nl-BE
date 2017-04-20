## <a name="prepare-to-authenticate-azure-resource-manager-requests"></a>Voorbereiden om te verifiëren aanvragen Azure Resource Manager

U moet verifiëren dat de bewerkingen die u uitvoert met [Azure Resource Manager] bronnen[ lnk-authenticate-arm] met Azure Active Directory (AD). De eenvoudigste manier om dit te configureren is PowerShell of Azure CLI te gebruiken.

Installeer [Azure PowerShell 1.0] [ lnk-powershell-install] of hoger voordat u doorgaat.

De volgende stappen wordt aangegeven hoe voor het instellen van verificatie met een wachtwoord voor een AD-toepassing met PowerShell. U kunt deze opdrachten uitvoeren in een standaard PowerShell-sessie.

1. Aanmelden bij uw Azure-abonnement met de volgende opdracht:

    ```
    Login-AzureRmAccount
    ```

2. Maak een notitie van uw **TenantId** en **SubscriptionId**. U moet ze later.

3. Maak een nieuwe Azure Active Directory-toepassing met de volgende opdracht, vervangt de houders plaats:

    - **{Naam}:** een weergavenaam voor uw toepassing bijvoorbeeld **MySampleApp**
    - **{URL introductiepagina}:** de URL van de introductiepagina van uw app zoals **http://mysampleapp/home**. Deze URL hoeft niet te verwijzen naar een echte toepassing.
    - **{Toepassings-id}:** Een unieke id, zoals **http://mysampleapp**. Deze URL hoeft niet te verwijzen naar een echte toepassing.
    - **{Wachtwoord}:** Een wachtwoord dat u gebruiken wilt om te verifiëren met uw app.

    ```
    New-AzureRmADApplication -DisplayName {Display name} -HomePage {Home page URL} -IdentifierUris {Application identifier} -Password {Password}
    ```
    
4. Maak een notitie van de **ApplicationId** van de toepassing die u hebt gemaakt. U moet dit later.

5. Maak een nieuwe service principal met de volgende opdracht, **{MyApplicationId}** vervangen door de **ApplicationId** uit de vorige stap:

    ```
    New-AzureRmADServicePrincipal -ApplicationId {MyApplicationId}
    ```
    
6. Een roltoewijzing met de volgende opdracht, vervangen door de **ApplicationId** **{MyApplicationId}** Setup.

    ```
    New-AzureRmRoleAssignment -RoleDefinitionName Owner -ServicePrincipalName {MyApplicationId}
    ```
    
U bent nu klaar om de Azure AD-toepassing waarmee u voor de verificatie van uw aangepaste C#-toepassing maken. Verderop in deze handleiding moet u de volgende waarden:

- TenantId
- SubscriptionId
- ApplicationId
- Wachtwoord

[lnk-authenticate-arm]: https://msdn.microsoft.com/library/azure/dn790557.aspx
[lnk-powershell-install]: ../articles/powershell-install-configure.md
