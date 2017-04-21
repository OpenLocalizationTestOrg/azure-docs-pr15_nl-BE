<properties services="virtual-machines" title="Setting up PowerShell" authors="JoeDavies-MSFT" solutions="" manager="timlt" editor="tysonn" />

<tags
   ms.service="virtual-machines"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm=""
   ms.workload="infrastructure"
   ms.date="05/12/2015"
   ms.author="rasquill" />

## <a name="setting-up-powershell"></a>PowerShell instellen

Voordat u de Azure PowerShell gebruiken kunt, volg deze stappen.

### <a name="verify-powershell-versions"></a>PowerShell versies controleren

Voordat u Windows PowerShell gebruiken kunt, moet u Windows PowerShell, versie 3.0 of 4.0 hebben. Typ de volgende opdracht achter de opdrachtprompt van Windows PowerShell om de versie van Windows PowerShell.

    $PSVersionTable

U ziet er ongeveer als volgt.

    Name                           Value
    ----                           -----
    PSVersion                      3.0
    WSManStackVersion              3.0
    SerializationVersion           1.1.0.1
    CLRVersion                     4.0.30319.18444
    BuildVersion                   6.2.9200.16481
    PSCompatibleVersions           {1.0, 2.0, 3.0}
    PSRemotingProtocolVersion      2.2

Controleer of de waarde van **PSVersion** 3.0 of 4.0. Zie [Windows Management Framework 3.0](http://www.microsoft.com/download/details.aspx?id=34595) of [4.0 voor Windows Management Framework](http://www.microsoft.com/download/details.aspx?id=40855)installeren van een compatibele versie.

Hebben ook Azure PowerShell versie 0.8.0 of hoger. U kunt controleren welke versie van Azure PowerShell die u met deze opdracht bij de opdrachtprompt Azure PowerShell hebt geïnstalleerd.

    Get-Module azure | format-table version

U ziet er ongeveer als volgt.

    Version
    -------
    0.8.16.1

Zie voor instructies en een koppeling naar de nieuwste versie [installeren en configureren van Azure PowerShell](powershell-install-configure.md).


### <a name="set-your-azure-account-and-subscription"></a>Stel uw Azure-account en abonnement

Als u niet al een Azure-abonnement hebt, kunt u uw [MSDN-abonnee vergoedingen](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) of teken omhoog activeren voor een [gratis proefperiode](https://azure.microsoft.com/pricing/free-trial/).

Open een opdrachtprompt Azure PowerShell en Azure aanmelden met deze opdracht.

    Add-AzureAccount

Als er meerdere Azure abonnementen, kunt u met deze opdracht de Azure-abonnementen aanbieden.

    Get-AzureSubscription

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

    $subscr="<SubscriptionName from the display of Get-AzureSubscription>"
    Select-AzureSubscription -SubscriptionName $subscr -Current 

Zie voor meer informatie over accounts en Azure abonnementen [procedure: verbinding maken met uw abonnement](powershell-install-configure.md#Connect).
