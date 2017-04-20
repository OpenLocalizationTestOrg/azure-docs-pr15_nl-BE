<properties
   pageTitle="Aan de slag met Azure Batch PowerShell | Microsoft Azure"
   description="Een korte inleiding tot de Azure PowerShell-cmdlets die voor het beheren van de Batch Azure-service kunt u ophalen"
   services="batch"
   documentationCenter=""
   authors="mmacy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="batch"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="powershell"
   ms.workload="big-compute"
   ms.date="10/20/2016"
   ms.author="marsma"/>

# <a name="get-started-with-azure-batch-powershell-cmdlets"></a>Aan de slag met Azure Batch PowerShell-cmdlets
U kunt met de Azure Batch PowerShell-cmdlets, uitvoeren en scripts veel van de taken die u met de Batch-API's en de Azure portal Azure opdrachtregelinterface (CLI uitvoert). Dit is een korte inleiding tot de cmdlets die kunt u uw Batchaccounts beheren en werken met uw Batch-bronnen, zoals toepassingen, projecten en taken.

Zie voor een volledige lijst van Batch-cmdlets en gedetailleerde cmdlet syntaxis de [Azure Batch cmdlet referentie](https://msdn.microsoft.com/library/azure/mt125957.aspx).

Dit artikel is gebaseerd op cmdlets in Azure PowerShell versie 3.0.0. Wij raden aan dat u uw Azure PowerShell regelmatig om te profiteren van service-updates en verbeteringen worden bijgewerkt.

## <a name="prerequisites"></a>Vereisten

De volgende bewerkingen als Azure PowerShell gebruiken voor het beheren van uw resources Batch uitvoeren.

* [Installeer en configureer Azure PowerShell](../powershell-install-configure.md)

* Voer de cmdlet **Aanmelding AzureRmAccount** verbinding maken met uw abonnement (de Azure Batch cmdlets schip in de module Azure Resource Manager):

    `Login-AzureRmAccount`

* **Registreren bij de provider Batch naamruimte**. Deze bewerking moet alleen worden uitgevoerd **Wanneer u eenmaal per abonnement**.

    `Register-AzureRMResourceProvider -ProviderNamespace Microsoft.Batch`

## <a name="manage-batch-accounts-and-keys"></a>Batch-accounts en sleutels beheren

### <a name="create-a-batch-account"></a>Een Batch-account maken

**Nieuw AzureRmBatchAccount** maakt een Batch-account in een opgegeven groep. Als u nog niet een resourcegroep, maken door de cmdlet [New-AzureRmResourceGroup](https://msdn.microsoft.com/library/azure/mt603739.aspx) wordt uitgevoerd. Een van de Azure regio's opgeven in de parameter **locatie** zoals "Centraal US". Bijvoorbeeld:

    New-AzureRmResourceGroup –Name MyBatchResourceGroup –location "Central US"

Maak een Batch-account in de resourcegroep opgeven van een naam voor de account in <*accountnaam*> en de locatie en naam van de resourcegroep. De Batch-account kan enige tijd duren om te voltooien. Bijvoorbeeld:

    New-AzureRmBatchAccount –AccountName <account_name> –Location "Central US" –ResourceGroupName <res_group_name>

> [AZURE.NOTE] De naam van de account moet uniek zijn voor de regio Azure voor de resourcegroep, tussen 3 en 24 tekens bevatten en kleine letters en cijfers gebruiken.

### <a name="get-account-access-keys"></a>Toegangstoetsen account ophalen
**Get-AzureRmBatchAccountKeys** bevat de toegangstoetsen die is gekoppeld aan een account Azure Batch. Voer bijvoorbeeld het volgende als u de primaire en secundaire sleutels van de account die u hebt gemaakt.

    $Account = Get-AzureRmBatchAccountKeys –AccountName <account_name>

    $Account.PrimaryAccountKey

    $Account.SecondaryAccountKey

### <a name="generate-a-new-access-key"></a>Een nieuwe access-sleutel genereren
**Nieuw AzureRmBatchAccountKey** genereert een nieuwe sleutel van de primaire of secundaire account voor een account Azure Batch. Typ bijvoorbeeld voor het genereren van een nieuwe primaire sleutel voor uw Batch-account:

    New-AzureRmBatchAccountKey -AccountName <account_name> -KeyType Primary

> [AZURE.NOTE] Voor het genereren van een nieuwe secundaire sleutel opgeven "Secundaire" voor de parameter **KeyType** . U moet de primaire en secundaire sleutels apart te genereren.

### <a name="delete-a-batch-account"></a>Een Batch-account verwijderen
**Verwijderen AzureRmBatchAccount** verwijdert een Batch-account. Bijvoorbeeld:

    Remove-AzureRmBatchAccount -AccountName <account_name>

Bevestig desgevraagd dat de account verwijderen. Account verwijderen kan enige tijd duren.

## <a name="create-a-batchaccountcontext-object"></a>Een BatchAccountContext-object maken

Maakt een BatchAccountContext-object voor het opslaan van uw accountnaam en sleutels voor de verificatie van de Batch PowerShell cmdlets gebruiken bij het maken en beheren van toepassingen, projecten, taken en andere bronnen van Batch:

    $context = Get-AzureRmBatchAccountKeys -AccountName <account_name>

U doorgeven het object BatchAccountContext naar cmdlets waarmee de **BatchContext** -parameter.

> [AZURE.NOTE] Standaard de primaire sleutel van de account die wordt gebruikt voor verificatie, maar u kunt de sleutel moet worden gebruikt door uw BatchAccountContext **KeyInUse** eigenschap expliciet selecteren: `$context.KeyInUse = "Secondary"`.

## <a name="create-and-modify-batch-resources"></a>Maken en wijzigen van Batch-bronnen
Als **Nieuw AzureBatchPool**, **Nieuwe AzureBatchJob**en **Nieuw AzureBatchTask** -cmdlets gebruiken middelen van een Batch-account maken. Er zijn die **Get -** en **Set -** cmdlets voor het bijwerken van de eigenschappen van bestaande bronnen en **verwijderen -** cmdlets om resources in een Batch-account verwijderen.

Bij het gebruik van veel van deze cmdlets, naast het geven van een BatchContext-object dat u wilt maken of doorgeven van objecten die gedetailleerde instellingen bevatten, zoals in het volgende voorbeeld. Zie de gedetailleerde help voor elke cmdlet voor meer voorbeelden.

### <a name="create-a-batch-pool"></a>Een Batch-toepassingen maken

Wanneer het maken of bijwerken van een Batch-toepassingen, selecteert u de configuratie van een cloud-service of de configuratie van een virtuele machine voor het besturingssysteem op de compute nodes (Zie [overzicht van Batch-functie](batch-api-basics.md#pool)). Uw keuze bepaalt u of uw computerknooppunten een image wordt gemaakt met een van de [Azure Gast OS releases](../cloud-services/cloud-services-guestos-update-matrix.md#releases) of met een van de ondersteunde Linux of Windows VM afbeeldingen op Marketplace Azure.

Wanneer u een **Nieuw AzureBatchPool**uitvoert, geeft u de instellingen in een PSCloudServiceConfiguration of PSVirtualMachineConfiguration-object. De volgende cmdlet wordt een nieuwe Batch toepassingen gemaakt met kleine computerknooppunten grootte in de serviceconfiguratie wolk image gemaakt met de nieuwste versie van het besturingssysteem van de familie 3 (Windows Server 2012). De parameter **CloudServiceConfiguration** geeft hier de variabele *$configuration* als het PSCloudServiceConfiguration-object. De parameter **BatchContext** geeft een vooraf gedefinieerde variabele *$context* als het BatchAccountContext-object.

    $configuration = New-Object -TypeName "Microsoft.Azure.Commands.Batch.Models.PSCloudServiceConfiguration" -ArgumentList @(4,"*")

    New-AzureBatchPool -Id "AutoScalePool" -VirtualMachineSize "Small" -CloudServiceConfiguration $configuration -AutoScaleFormula '$TargetDedicated=4;' -BatchContext $context

Het aantal doel computerknooppunten in de nieuwe groep van toepassingen wordt bepaald door een formule autoscaling. De formule is in dit geval gewoon **$TargetDedicated = 4**, 4 die het aantal computerknooppunten in de pool aangeeft maximaal is.

## <a name="query-for-pools-jobs-tasks-and-other-details"></a>Query voor groepen van toepassingen, projecten, taken en andere details

Cmdlets, zoals **Get-AzureBatchPool**, **Get-AzureBatchJob**en **Get-AzureBatchTask** query voor entiteiten die zijn gemaakt met een Batch-account gebruiken.

### <a name="query-for-data"></a>Query voor gegevens

Als u bijvoorbeeld **Get-AzureBatchPools** te gebruiken om uw toepassingen. Standaard worden deze query's voor alle toepassingen op uw account, ervan uitgaande dat u al het BatchAccountContext-object opgeslagen in *$context*:

    Get-AzureBatchPool -BatchContext $context

### <a name="use-an-odata-filter"></a>Gebruik een filter OData

U kunt een OData-filter alleen de objecten waarin je geïnteresseerd bent te zoeken met de parameter **Filter** opgeven. Zo vindt u alle groepen met id's beginnen met "myPool":

    $filter = "startswith(id,'myPool')"

    Get-AzureBatchPool -Filter $filter -BatchContext $context

Deze methode is niet zo flexibel als 'waar '-Object gebruiken in een lokale pijplijn. Echter, de query wordt verzonden naar de Batch-service rechtstreeks zodat alle filters op de server, Internet-bandbreedte opslaan gebeurt.

### <a name="use-the-id-parameter"></a>Gebruik de parameter Id

Een alternatief voor een filter OData is met de **id-** parameter. Om te zoeken op een specifieke groep met id 'myPool':

    Get-AzureBatchPool -Id "myPool" -BatchContext $context

De parameter **Id** ondersteunt alleen volledige-id zoeken, geen jokertekens of filters OData-stijl.

### <a name="use-the-maxcount-parameter"></a>Gebruik de parameter MaxCount

Standaard wordt elke cmdlet maximaal 1000 objecten. Als u deze limiet bereikt, verfijnen van het filter weer minder objecten weer te geven of expliciet instellen met behulp van de parameter **MaxCount** maximaal. Bijvoorbeeld:

    Get-AzureBatchTask -MaxCount 2500 -BatchContext $context

Instellen als u wilt dat de bovengrens, **MaxCount** op 0 of minder.

### <a name="use-the-powershell-pipeline"></a>Gebruik de PowerShell-pijplijn

De PowerShell-pijplijn voor het verzenden van gegevens tussen de cmdlets kunt gebruikmaken van batch-cmdlets. Dit heeft hetzelfde effect als een parameter op te geven, maar gemakkelijker werken met meerdere entiteiten.

Bijvoorbeeld, zoeken en weergeven van alle taken die bij uw account:

    Get-AzureBatchJob -BatchContext $context | Get-AzureBatchTask -BatchContext $context

Restart (opnieuw opstarten) elk computerknooppunt in een groep:

    Get-AzureBatchComputeNode -PoolId "myPool" -BatchContext $context | Restart-AzureBatchComputeNode -BatchContext $context

## <a name="application-package-management"></a>Pakket Toepassingsbeheer

Toepassingspakketten biedt een vereenvoudigde manier voor de implementatie van toepassingen aan de compute nodes in uw toepassingen. Met de batchverwerking PowerShell-cmdlets kunt u uploaden en toepassingspakketten in uw Batch-account beheren en computerknooppunten pakket versies implementeren.

**Maken** van een toepassing:

    New-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication"

**Toevoegen** van een toepassingspakket:

    New-AzureRmBatchApplicationPackage -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication" -ApplicationVersion "1.0" -Format zip -FilePath package001.zip

De **standaardversie** voor de toepassing instellen:

    Set-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication" -DefaultVersion "1.0"

**Lijst** van de toepassing-pakketten

    $application = Get-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication"

    $application.ApplicationPackages

**Verwijder** een toepassingspakket

    Remove-AzureRmBatchApplicationPackage -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication" -ApplicationVersion "1.0"

**Verwijder** een toepassing

    Remove-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication"

>[AZURE.NOTE] U moet alle pakket-versies van een toepassing toepassingen verwijderen voordat u de toepassing verwijderen. U ontvangt een foutbericht als u probeert te verwijderen van een toepassing met toepassingspakketten.

### <a name="deploy-an-application-package"></a>Een pakket van toepassing

Als u een groep maakt, kunt u een of meer toepassingspakketten voor de implementatie opgeven. Wanneer u een pakket bij het maken van toepassingen opgeeft, wordt deze geïnstalleerd op elk knooppunt als de joins knooppunt toepassingen. Pakketten worden ook geïmplementeerd als een knooppunt wordt opnieuw opgestart of reimaged.

Geef de `-ApplicationPackageReference` bij het maken van een een toepassingspakket op knooppunten van de toepassingen installeren als zij lid worden van de groep van toepassingen de optie. Maak eerst een **PSApplicationPackageReference** -object, en worden geconfigureerd met de pakket-Id en de versie van de toepassing die u wilt implementeren op een van de pool-computerknooppunten:

    $appPackageReference = New-Object Microsoft.Azure.Commands.Batch.Models.PSApplicationPackageReference

    $appPackageReference.ApplicationId = "MyBatchApplication"

    $appPackageReference.Version = "1.0"

Nu maakt de groep en het pakket reference-object opgeven als argument voor de `ApplicationPackageReferences` optie:

    New-AzureBatchPool -Id "PoolWithAppPackage" -VirtualMachineSize "Small" -CloudServiceConfiguration $configuration -BatchContext $context -ApplicationPackageReferences $appPackageReference

U vindt meer informatie over toepassingspakketten in de [implementatie van toepassingen met Azure Batch toepassingspakketten](batch-application-packages.md).

>[AZURE.IMPORTANT] Moet u [een opslag Azure-account koppelen](#linked-storage-account-autostorage) aan uw account voor Batch toepassingspakketten gebruiken.

### <a name="update-a-pools-application-packages"></a>Updatepakketten van een groep van toepassingen

Om te werken de toepassingen toegewezen aan een bestaande toepassingen, maakt u eerst een PSApplicationPackageReference-object met de gewenste eigenschappen (pakket-Id en de versie van toepassing):

    $appPackageReference = New-Object Microsoft.Azure.Commands.Batch.Models.PSApplicationPackageReference

    $appPackageReference.ApplicationId = "MyBatchApplication"

    $appPackageReference.Version = "2.0"

Vervolgens krijgt de groep van Batch, wissen om alle bestaande pakketten onze nieuwe pakket verwijzing toevoegen en de Batch-service bijwerken met de nieuwe instellingen voor groep van toepassingen:

    $pool = Get-AzureBatchPool -BatchContext $context -Id "PoolWithAppPackage"

    $pool.ApplicationPackageReferences.Clear()

    $pool.ApplicationPackageReferences.Add($appPackageReference)

    Set-AzureBatchPool -BatchContext $context -Pool $pool

U hebt nu de eigenschappen van de groep in de Batch-service bijgewerkt. Het nieuwe toepassingspakket als u wilt berekenen van de knooppunten in de groep daadwerkelijk implementeren, echter moet opnieuw start of reimage die knooppunten. U kunt elk knooppunt opnieuw te starten in een groep met deze opdracht:

    Get-AzureBatchComputeNode -PoolId "PoolWithAppPackage" -BatchContext $context | Restart-AzureBatchComputeNode -BatchContext $context

>[AZURE.TIP] U kunt meerdere toepassingspakketten implementeren op de compute nodes in een groep. Weglaten als u *toevoegen* een toepassingspakket in plaats van de bestaande pakketten vervangen wilt, de `$pool.ApplicationPackageReferences.Clear()` lijn erboven.

## <a name="next-steps"></a>Volgende stappen

* Zie voor gedetailleerde cmdlet syntaxis en voorbeelden, [Azure Batch cmdlet referentie](https://msdn.microsoft.com/library/azure/mt125957.aspx).

* Zie voor meer informatie over toepassingen en toepassingspakketten in Batch [implementatie van toepassingen met Azure Batch toepassingspakketten](batch-application-packages.md).
