<properties
    pageTitle="Overzicht van het implementatiescript resourcegroep Azure project | Microsoft Azure"
    description="Beschrijving van de werking van het PowerShell-script in het project resourcegroep Azure-implementatie."
    services="visual-studio-online"
    documentationCenter="na"
    authors="tfitzmac"
    manager="timlt"
    editor="" />

 <tags
    ms.service="azure-resource-manager"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="07/26/2016"
    ms.author="tomfitz" />

# <a name="overview-of-the-azure-resource-group-project-deployment-script"></a>Overzicht van een script voor de resourcegroep Azure project implementatie

Resourcegroep Azure-implementatie projecten kunt u voorbereiden en implementeren van bestanden en andere onderdelen op Azure. Wanneer u een bronnenbeheerder Azure implementatieproject in Visual Studio maakt, wordt een PowerShell script genaamd **Distribueren AzureResourceGroup.ps1** toegevoegd aan het project. Dit onderwerp bevat informatie over wat dit script doet en het uitvoeren van deze zowel binnen als buiten de Visual Studio.

## <a name="what-does-the-script-do"></a>Wat is het script?

Het script implementeren AzureResourceGroup.ps1 doet twee dingen die belangrijk voor de van implementatieworkflow zijn.

- Alle bestanden en onderdelen die nodig zijn voor de sjabloonimplementatie uploaden
- De sjabloon distribueren

Het eerste gedeelte van het script uploaden bestanden en onderdelen voor de implementatie en de laatste cmdlet in het script daadwerkelijk implementeren van de sjabloon. Bijvoorbeeld als een virtuele machine worden geconfigureerd met een script moet, uploadt het implementatiescript eerst veilig het configuratiescript aan een account Azure opslag. Hierdoor kunt u beschikbare Azure Resource Manager voor het configureren van de virtuele machine tijdens het inrichten.

Een schakeloptie *uploadArtifacts* genoemd, omdat niet alle implementaties van de sjabloon moeten extra onderdelen die moeten worden geüpload, wordt geëvalueerd. Als alle onderdelen worden geüpload moeten, stelt u de schakeloptie *uploadArtifacts* bij het aanroepen van het script. Houd er rekening mee dat de belangrijkste sjabloonbestand en het bestand hoeft te worden geüpload. Alleen andere bestanden, zoals configuratiescripts, geneste sjablonen voor implementatie en toepassingsbestanden moeten worden geüpload.

## <a name="detailed-script-description"></a>Script gedetailleerde beschrijving

Hieronder wordt beschreven welke secties selecteren van de implementatie AzureResourceGroup.ps1 Azure PowerShell script doen.

>[AZURE.NOTE] Beschrijving van versie 1.0 van het script distribueren AzureResourceGroup.ps1.

1.  Parameters die nodig zijn voor de implementatieproject Azure Resource Manager declareren. Sommige parameters zijn standaardwaarden die zijn ingesteld als het project is gemaakt. U kunt deze waarden in het script wijzigen of andere parameterwaarden toevoegen voordat u het script uitvoert.

    ```
    Param(
      [string] [Parameter(Mandatory=$true)] $ResourceGroupLocation,
      [string] $ResourceGroupName = 'AzureResourceGroup1',
      [switch] $UploadArtifacts,
      [string] $StorageAccountName,
      [string] $StorageAccountResourceGroupName,
      [string] $StorageContainerName = $ResourceGroupName.ToLowerInvariant() + '-stageartifacts',
      [string] $TemplateFile = '..\Templates\azuredeploy.json',
      [string] $TemplateParametersFile = '..\Templates\azuredeploy.parameters.json',
      [string] $ArtifactStagingDirectory = '..\bin\Debug\staging',
      [string] $AzCopyPath = '..\Tools\AzCopy.exe',
      [string] $DSCSourceFolder = '..\DSC'
    )
    ```

  	|Parameter|Beschrijving|
  	|---|---|
  	|$ResourceGroupLocation|De regio of data center locatie voor de resourcegroep, bijvoorbeeld **US West** of **Oost-Azië**.|
  	|$ResourceGroupName|De naam van de resourcegroep Azure.|
  	|$UploadArtifacts|Een binaire waarde die aangeeft of onderdelen moeten worden geüpload naar Azure van uw systeem.|
  	|$StorageAccountName|De naam van de account van uw opslag Azure waar uw artefacten worden geüpload.|
  	|$StorageAccountResourceGroupName|De naam van de groep Azure resource met de account van de opslag.|
  	|$StorageContainerName|De naam van de opslag container gebruikt voor het uploaden van artefacten.|
  	|$TemplateFile|Het pad naar het bestand deployment (`<app name>.json`) in uw project resourcegroep Azure.|
  	|$TemplateParametersFile|Het pad naar het parameterbestand (`<app name>.parameters.json`) in uw project resourcegroep Azure.|
  	|$ArtifactStagingDirectory|Het pad van het systeem waar artefacten zijn lokaal geüpload, met inbegrip van de hoofdmap van de PowerShell-script. Dit pad kan absoluut of relatief ten opzichte van de scriptlocatie zijn.|
  	|$AzCopyPath|Het pad waar het programma AzCopy.exe de ZIP-bestanden, met inbegrip van de hoofdmap PowerShell script gekopieerd. Dit pad kan absoluut of relatief ten opzichte van de scriptlocatie zijn.|
  	|$DSCSourceFolder|Het pad naar de bronmap DSC (gewenst staat configuratie), met inbegrip van de hoofdmap van de PowerShell-script. Dit pad kan absoluut of relatief ten opzichte van de scriptlocatie zijn. Zie [Inleiding tot de extensie Azure PowerShell DSC (gewenst staat configuratie)](http://blogs.msdn.com/b/powershell/archive/2014/08/07/introducing-the-azure-powershell-dsc-desired-state-configuration-extension.aspx), indien van toepassing, voor meer informatie.|

1.  Controleer of artefacten moeten worden geüpload naar Azure. Als dit niet het geval is, gaat u verder met stap 11. Anders wordt de volgende stappen uitvoeren.

1.  Alle variabelen met relatieve paden omzetten in absolute paden. Wijzig bijvoorbeeld een pad, zoals `..\Tools\AzCopy.exe` naar `C:\YourFolder\Tools\AzCopy.exe`. Initialiseren bovendien de variabelen *ArtifactsLocationName* en *ArtifactsLocationSasTokenName* op null. *ArtifactsLocation* en *SaSToken* worden de parameters in de sjabloon. Als de waarden null na het lezen in het parameterbestand, genereert het script waarden voor hen.

    De Azure gebruikt de parameter waarden *_artifactsLocation* en *_artifactsLocationSasToken* in de sjabloon voor het beheren van artefacten. Als het PowerShell-script parameters met de namen worden gevonden, maar de waarden van de parameters worden opgegeven, wordt het script de artefacten uploadt en geeft als resultaat de waarden voor deze parameters. Deze vervolgens doorgeeft aan de cmdlet via `@OptionsParameters`.

  	|Variabele|Beschrijving|
  	|---|---|
  	|ArtifactsLocationName|Het pad naar waar de Azure artefacten zich bevinden.|
  	|ArtifactsLocationSasTokenName|De SAS (gedeelde toegang handtekening) naam van token dat wordt gebruikt door het script om de Bus Service te verifiëren. Zie [Gedeelde handtekening verificatie met Bus Service](service-bus-shared-access-signature-authentication.md) voor meer informatie.|

    ```
    if ($UploadArtifacts) {
    # Convert relative paths to absolute paths if needed
    $AzCopyPath = [System.IO.Path]::Combine($PSScriptRoot, $AzCopyPath)
    $ArtifactStagingDirectory = [System.IO.Path]::Combine($PSScriptRoot, $ArtifactStagingDirectory)
    $DSCSourceFolder = [System.IO.Path]::Combine($PSScriptRoot, $DSCSourceFolder)

    Set-Variable ArtifactsLocationName '_artifactsLocation' -Option ReadOnly
    Set-Variable ArtifactsLocationSasTokenName '_artifactsLocationSasToken' -Option ReadOnly

    $OptionalParameters.Add($ArtifactsLocationName, $null)
    $OptionalParameters.Add($ArtifactsLocationSasTokenName, $null)
    ```

1.  Deze sectie gecontroleerd of de <app name>. parameters.json-bestand (als het bestand' Parameters' genoemd) is een bovenliggend knooppunt met de naam **parameters** (in de `else` blokkeren). Anders heeft geen bovenliggende knooppunt. De indeling is aanvaardbaar.
    
    ```
    if ($JsonParameters -eq $null) {
            $JsonParameters = $JsonContent
        }
        else {
            $JsonParameters = $JsonContent.parameters
        }
    ```

1.  De collectie doorlopen en van JSON-parameters. Als een waarde voor de parameter *_artifactsLocation* of *_artifactsLocationSasToken*is toegewezen, stelt u de variabele *$OptionalParameters* met deze waarden. Hiermee voorkomt u dat het script alle parameterwaarden die u per ongeluk te overschrijven.

    ```
    $JsonParameters | Get-Member -Type NoteProperty | ForEach-Object {
        $ParameterValue = $JsonParameters | Select-Object -ExpandProperty $_.Name

        if ($_.Name -eq $ArtifactsLocationName -or $_.Name -eq $ArtifactsLocationSasTokenName) {
            $OptionalParameters[$_.Name] = $ParameterValue.value
        }
    }
    ```

1.  Lees het opslag account sleutel en de context voor de opslag account resource dat bestemd is voor de onderdelen voor de implementatie.

    ```
    $StorageAccountKey = (Get-AzureRMStorageAccountKey -ResourceGroupName $StorageAccountResourceGroupName -Name $StorageAccountName).Key1

    $StorageAccountContext = (Get-AzureRmStorageAccount -ResourceGroupName $StorageAccountResourceGroupName -Name $StorageAccountName).Context
    ```

1.  Als u een virtuele machine configureren in PowerShell DSC, moet de extensie DSC de artefacten in een enkele zip-bestand. Maak dus een ZIP-archief voor de DSC-configuratie. Hiertoe controleert u of er $DSCSourceFolder bestaat. Als een DSC-configuratie bestaat, verwijdert u deze en maakt u nieuwe gecomprimeerde bestand met de naam dsc.zip.

    ```
    # Create DSC configuration archive
    if (Test-Path $DSCSourceFolder) {
    Add-Type -Assembly System.IO.Compression.FileSystem
        $ArchiveFile = Join-Path $ArtifactStagingDirectory "dsc.zip"
        Remove-Item -Path $ArchiveFile -ErrorAction SilentlyContinue
        [System.IO.Compression.ZipFile]::CreateFromDirectory($DSCSourceFolder, $ArchiveFile)
    }
    ```

1.  Als geen pad voor Azure artefacten is opgegeven in het bestand Parameters, stelt u een pad voor het PowerShell-script gebruiken tijdens het uploaden van artefacten. U doet dit door een pad met een combinatie van opslag-account eindpunt pad plus de naam van de container opslag te maken. Vervolgens moet u het bestand Parameters bijwerken met dit nieuwe pad.

    ```
    # Generate the value for artifacts location if it is not provided in the parameter file
    $ArtifactsLocation = $OptionalParameters[$ArtifactsLocationName]
    if ($ArtifactsLocation -eq $null) {
        $ArtifactsLocation = $StorageAccountContext.BlobEndPoint + $StorageContainerName
        $OptionalParameters[$ArtifactsLocationName] = $ArtifactsLocation
    }
    ```

1.  Gebruik het hulpprogramma voor **AzCopy** (opgenomen in de map **Tools** van uw project resourcegroep Azure-implementatie) bestanden van uw lokale opslag neerzetten pad kopiëren naar uw on line opslag Azure-account. Als deze stap niet werkt, sluit het script aangezien de implementatie niet kunnen slagen zonder de vereiste onderdelen.

    ```
    # Use AzCopy to copy files from the local storage drop path to the storage account container
    & $AzCopyPath """$ArtifactStagingDirectory""", $ArtifactsLocation, "/DestKey:$StorageAccountKey", "/S", "/Y", "/Z:$env:LocalAppData\Microsoft\Azure\AzCopy\$ResourceGroupName"
    if ($LASTEXITCODE -ne 0) { return }
    ```

1.  Als een SAS-token voor de locatie artefacten is niet beschikbaar in het bestand Parameters, maakt u een tijdelijke alleen-lezen toegang tot de on line opslag container. Deze token SAS bij de cmdline vervolgens doorgeven als een 'optionalParameter'. Opmerking: de eventuele parameters die worden doorgegeven op de opdrachtregel, hebben voorrang boven de waarden in het parameterbestand.

    ```
    # Generate the value for artifacts location SAS token if it is not provided in the parameter file
    $ArtifactsLocationSasToken = $OptionalParameters[$ArtifactsLocationSasTokenName]
    if ($ArtifactsLocationSasToken -eq $null) {
       # Create a SAS token for the storage container - this gives temporary read-only access to the container
       $ArtifactsLocationSasToken = New-AzureStorageContainerSASToken -Container $StorageContainerName -Context $StorageAccountContext -Permission r -ExpiryTime (Get-Date).AddHours(4)
       $ArtifactsLocationSasToken = ConvertTo-SecureString $ArtifactsLocationSasToken -AsPlainText -Force
       $OptionalParameters[$ArtifactsLocationSasTokenName] = $ArtifactsLocationSasToken
    }
    ```

1.  De resourcegroep te maken als het niet reeds bestaan en controleer het bestand sjabloon en parameters voor de validatiefouten die voorkomen de implementatie van het mislukken dat.

    ```
    # Create or update the resource group using the specified template file and template parameters file
    New-AzureRMResourceGroup -Name $ResourceGroupName -Location $ResourceGroupLocation -Verbose -Force -ErrorAction Stop

    Test-AzureRmResourceGroupDeployment -ResourceGroupName $ResourceGroupName -TemplateFile $TemplateFile -TemplateParameterFile $TemplateParametersFile @OptionalParameters -ErrorAction Stop
    ```

1. Ten slotte implementeert u de sjabloon. Deze code maakt een unieke naam voor de implementatie met behulp van een tijdstempel.

    ```
    New-AzureRMResourceGroupDeployment -Name ((Get-ChildItem $TemplateFile).BaseName + '-' + ((Get-Date).ToUniversalTime()).ToString('MMdd-HHmm')) `
        -ResourceGroupName $ResourceGroupName `
        -TemplateFile $TemplateFile `
        -TemplateParameterFile $TemplateParametersFile `
        @OptionalParameters `
        -Force -Verbose
    ```

## <a name="deploy-the-resource-group"></a>Implementeren van de resourcegroep

### <a name="to-deploy-the-resource-group-in-visual-studio"></a>Implementatie van de resourcegroep in Visual Studio

1. Kies in het snelmenu van het project Azure, resourcegroep, **distribueren** > **Nieuwe distributie**.

    ![][0]

1. In het dialoogvenster **Deploy resourcegroep** of kies een bestaande resourcegroep in de vervolgkeuzelijst te implementeren op of kies ** &lt;nieuw... &gt;** een nieuwe resourcegroep maken.

    ![][1]

1. Als u wordt gevraagd, een resource groepsnaam en locatie opgeven in het dialoogvenster **Resourcegroep maken** en kies vervolgens de knop **maken** .

    ![][2]

1. Klik op de knop **Parameters bewerken** om het dialoogvenster **Parameters bewerken** en voer vervolgens eventuele ontbrekende parameterwaarden.

    ![][3]

    >[AZURE.NOTE] Als alle vereiste parameters waarden nodig, in dit dialoogvenster automatisch wordt weergegeven wanneer u implementeren.

    ![][4]

1. Wanneer u klaar bent parameterwaarden invoeren, klik op de knop **Opslaan** en klik op **Deploy** .

    Het script deployment (implementatie-AzureResourceGroup.ps1) wordt uitgevoerd en de sjabloon samen met eventuele artefacten, implementeert naar Azure.

### <a name="to-deploy-the-resource-group-by-using-powershell"></a>De resourcegroep met PowerShell implementeren

Als u het script wilt uitvoeren zonder gebruik van de opdracht distribueren van Visual Studio en de gebruikersinterface in het snelmenu voor het script, kiest u **openen met PowerShell ISE**.

![][5]


## <a name="command-deployment-examples"></a>Implementatie, opdracht, voorbeelden

### <a name="deploy-using-default-values"></a>Implementeren met behulp van standaardwaarden

In dit voorbeeld ziet hoe u het script met de parameter standaardwaarden. (Omdat de locatieparameter geen standaardwaarde moet leveren.)

`.\Deploy-AzureResourceGroup.ps1 -ResourceGroupLocation eastus`

### <a name="deploy-overriding-the-default-values"></a>Implementeren van de standaardwaarden overschrijven

In dit voorbeeld ziet hoe u het script uitvoeren om te implementeren bestanden sjabloon en parameters die van de standaardwaarden verschillen.

```
.\Deploy-AzureResourceGroup.ps1 -ResourceGroupLocation eastus –TemplateFile ..\templates\AnotherTemplate.json –TemplateParametersFile ..\templates\AnotherTemplate.parameters.json
```

### <a name="deploy-using-uploadartifacts-for-staging"></a>Met behulp van de UploadArtifacts voor gefaseerde installatie implementeren

In dit voorbeeld ziet hoe u het script uitvoeren om te uploaden artefacten uit de releasemap en sjablonen voor niet-standaard implementeren.

```
.\Deploy-AzureResourceGroup.ps1 -StorageAccountName 'mystorage' -StorageAccountResourceGroupName 'Default-Storage-EastUS' -ResourceGroupName 'myResourceGroup' -ResourceGroupLocation 'eastus' -TemplateFile '..\templates\windowsvirtualmachine.json' -TemplateParametersFile '..\templates\windowsvirtualmachine.parameters.json' -UploadArtifacts -ArtifactStagingDirectory ..\bin\release\staging
```

In dit voorbeeld ziet hoe u het script uitvoeren in een taak Azure PowerShell in Visual Studio Online.

```
$(Build.StagingDirectory)/AzureResourceGroup1/Scripts/Deploy-AzureResourceGroup.ps1 -StorageAccountName 'mystorage' -StorageAccountResourceGroupName 'Default-Storage-EastUS' -ResourceGroupName 'myResourceGroup' -ResourceGroupLocation 'eastus' -TemplateFile '..\templates\windowsvirtualmachine.json' -TemplateParametersFile '..\templates\windowsvirtualmachine.parameters.json' -UploadArtifacts -ArtifactStagingDirectory $(Build.StagingDirectory)
```

## <a name="next-steps"></a>Volgende stappen
Lezen van [Azure Resource Manager-overzicht](azure-resource-manager/resource-group-overview.md)voor meer informatie over Azure Resource Manager.

Zie voor meer voorbeelden van het werken met projecten in de resourcegroep Azure [implementeren en Azure bronnen beheren](https://github.com/Microsoft/HealthClinic.biz/wiki/Deploy-and-manage-Azure-resources) van de [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz) 2015 Connect [demo](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/). Zie voor meer QuickStart uit de HealthClinic.biz-demo [Azure Developer Tools QuickStart](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts).

[0]: ./media/vs-azure-tools-resource-groups-how-script-works/deploy1c.png
[1]: ./media/vs-azure-tools-resource-groups-how-script-works/deploy2bc.png
[2]: ./media/vs-azure-tools-resource-groups-how-script-works/deploy3bc.png
[3]: ./media/vs-azure-tools-resource-groups-how-script-works/deploy4bc.png
[4]: ./media/vs-azure-tools-resource-groups-how-script-works/deploy5c.png
[5]: ./media/vs-azure-tools-resource-groups-how-script-works/deploy6c.png
