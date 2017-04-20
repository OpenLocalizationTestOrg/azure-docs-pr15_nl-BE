## <a name="setting-up-powershell-for-resource-manager-templates"></a>PowerShell instellen voor bronbeheer sjablonen

Voordat u Azure PowerShell met Resource Manager kunt, moet u het recht Windows PowerShell en Azure PowerShell versies.

### <a name="verify-powershell-versions"></a>PowerShell versies controleren

Controleer of u beschikt over Windows PowerShell versie 3.0 of 4.0. Typ de volgende opdracht achter de opdrachtprompt van Windows PowerShell om de versie van Windows PowerShell.

    $PSVersionTable

U ontvangt het volgende gegevenstype:

    Name                           Value
    ----                           -----
    PSVersion                      3.0
    WSManStackVersion              3.0
    SerializationVersion           1.1.0.1
    CLRVersion                     4.0.30319.18444
    BuildVersion                   6.2.9200.16481
    PSCompatibleVersions           {1.0, 2.0, 3.0}
    PSRemotingProtocolVersion      2.2


Controleer of de waarde van **PSVersion** 3.0 of 4.0. Als dat niet het geval is, raadpleegt u [Windows Management Framework 3.0](http://www.microsoft.com/download/details.aspx?id=34595) of [Windows Management Framework 4.0](http://www.microsoft.com/download/details.aspx?id=40855).

### <a name="set-your-azure-account-and-subscription"></a>Stel uw Azure-account en abonnement

Als u niet al een Azure-abonnement hebt, kunt u uw [MSDN-abonnee vergoedingen](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) of teken omhoog activeren voor een [gratis proefperiode](https://azure.microsoft.com/pricing/free-trial/).

Open een opdrachtprompt Azure PowerShell en Azure aanmelden met deze opdracht.

    Login-AzureRmAccount

Als er meerdere Azure abonnementen, kunt u met deze opdracht de Azure-abonnementen aanbieden.

    Get-AzureRmSubscription

U ontvangt het volgende gegevenstype:

    SubscriptionId            : fd22919d-eaca-4f2b-841a-e4ac6770g92e
    SubscriptionName          : Visual Studio Ultimate with MSDN
    Environment               : AzureCloud
    SupportedModes            : AzureServiceManagement,AzureResourceManager
    DefaultAccount            : johndoe@contoso.com
    Accounts                  : {johndoe@contoso.com}
    IsDefault                 : True
    IsCurrent                 : True
    CurrentStorageAccountName :
    TenantId                  : 32fa88b4-86f1-419f-93ab-2d7ce016dba7

Het huidige Azure abonnement kunt u instellen door het uitvoeren van deze opdrachten achter de opdrachtprompt Azure PowerShell. Vervang alles binnen de aanhalingstekens, met inbegrip van de < en > tekens met de juiste naam.

    $subscr="<SubscriptionName from the display of Get-AzureRmSubscription>"
    Select-AzureRmSubscription -SubscriptionName $subscr -Current

Zie voor meer informatie over accounts en Azure abonnementen [procedure: verbinding maken met uw abonnement](powershell-install-configure.md#Connect).
