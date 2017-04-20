<properties
   pageTitle="Dev-en testomgevingen publiceren met behulp van Windows PowerShell-Scripts | Microsoft Azure"
   description="Informatie over het Windows PowerShell-scripts vanuit Visual Studio gebruiken om te publiceren naar de ontwikkeling en testen omgevingen."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="using-windows-powershell-scripts-to-publish-to-dev-and-test-environments"></a>Met behulp van Windows PowerShell-scripts op dev publiceren en testen omgevingen

Als u een webtoepassing in Visual Studio maakt, kunt u een Windows PowerShell-script dat u later gebruiken kunt voor het automatiseren van het publiceren van uw website naar Azure als een Web App in Azure App Service of een virtuele machine te genereren. Bewerken en uitbreiden van de Windows PowerShell-script in de editor van Visual Studio op basis van uw behoeften, of het script te integreren met bestaande bouwen, testen en publiceren van scripts.

Met deze scripts, kunt u aangepaste versies (ook bekend als dev- en testomgevingen) van uw site voor tijdelijk gebruik inrichten. U kunt bijvoorbeeld een bepaalde versie van uw website op een Azure virtual machine of op de staging-sleuf op een website aan een test suite uitvoeren, een bug te reproduceren, test een bug fix, evaluatieversie van een voorgestelde wijziging of een aangepaste omgeving voor een demonstratie of presentatie instellen instellen. Nadat u een script dat u uw project publiceert hebt gemaakt, kunt u identieke omgevingen door het script opnieuw uit te voeren indien nodig opnieuw of Voer het script uit met uw eigen configuratie van uw webtoepassing voor het maken van een aangepaste omgeving voor het testen.

## <a name="what-you-need"></a>Wat u nodig hebt

- Azure SDK 2.3 of hoger. Zie [Downloads voor Visual Studio](http://go.microsoft.com/fwlink/?LinkID=624384) voor meer informatie.

U hoeft niet de Azure SDK voor het genereren van de scripts voor webprojecten. Deze functie is voor webprojecten, geen web-rollen in de cloud-services.

- Azure PowerShell 0.7.4 of hoger. [Het installeren en configureren van Azure PowerShell](powershell-install-configure.md) Zie voor meer informatie.

- [Windows PowerShell 3.0](http://go.microsoft.com/?linkid=9811175) of hoger.

## <a name="additional-tools"></a>Extra 's

Extra hulpprogramma's en bronnen voor het werken met PowerShell in Visual Studio voor de ontwikkeling van Azure zijn beschikbaar. Zie [PowerShell's voor Visual Studio](http://go.microsoft.com/fwlink/?LinkId=404012).

## <a name="generating-the-publish-scripts"></a>De publicatie-scripts genereren

U kunt de scripts publiceren voor een virtuele machine die fungeert als host voor uw website wanneer u een nieuw project door de volgende [instructies maken](./virtual-machines/virtual-machines-windows-classic-web-app-visual-studio.md)genereren. U kunt ook [genereren van scripts voor het web apps te publiceren in Azure App Service](./app-service-web/web-sites-dotnet-get-started.md).

## <a name="scripts-that-visual-studio-generates"></a>Scripts die worden gegenereerd met Visual Studio

Visual Studio genereert een oplossing niveau map met de naam van een module met functies die u in de scripts gebruiken kunt en **PublishScripts** met twee bestanden met Windows PowerShell, een script publiceren voor uw virtuele machine of de website. Visual Studio genereert ook een bestand in de JSON-indeling waarmee u de details van het project dat u wilt implementeren.

### <a name="windows-powershell-publish-script"></a>Windows PowerShell publiceren script

Het script publiceren bevat publiceren voor specifieke stappen voor het implementeren van een website of virtuele machine. Visual Studio biedt syntaxis kleuren voor de ontwikkeling van Windows PowerShell. Help voor de functies beschikbaar is en u kunt de functies in het script aanpassen aan de veranderende eisen vrij bewerken.

### <a name="windows-powershell-module"></a>Windows PowerShell-module

De module Windows PowerShell die worden gegenereerd met Visual Studio bevat functies die het script publiceren wordt gebruikt. Deze functies Azure PowerShell en zijn niet bedoeld om te worden gewijzigd. [Het installeren en configureren van Azure PowerShell](powershell-install-configure.md) Zie voor meer informatie.

### <a name="json-configuration-file"></a>JSON-configuratiebestand

De JSON-bestand is gemaakt in de map **configuraties** en bevat configuratiegegevens waarmee precies welke bronnen naar Azure implementeren. De naam van het bestand dat door Visual Studio gegenereerd is project-naam-WAWS-dev.json als u een website of het project de naam VM dev.json gemaakt als u een virtuele machine hebt gemaakt. Hier volgt een voorbeeld van een configuratiebestand JSON die wordt gegenereerd wanneer u een website maken. De meeste van de waarden zijn geen uitleg. Naam van de website wordt gegenereerd door Azure, zodat het mogelijk niet overeen met de projectnaam van uw.

```
{
"environmentSettings": {
"webSite": {
"name": "WebApplication26632",
"location": "West US"
},
"databases": [
{
"connectionStringName": "DefaultConnection",
"databaseName": "WebApplication26632_db",
"serverName": "YourDatabaseServerName",
"user": "sqluser2",
"password": "",
"edition": "",
"size": "",
"collation": "",
"location": "West US"
}
]
}
}
```
Wanneer u een virtuele machine maakt, lijkt de JSON-configuratiebestand op de volgende. Houd er rekening mee dat er een cloud-service als een container voor de virtuele machine wordt gemaakt. De virtuele machine bevat de gebruikelijke eindpunten voor web access via HTTP en HTTPS, alsmede de eindpunten voor webonderdelen implementeren, waarin u kunt publiceren op de website van uw lokale computer, extern bureaublad en Windows PowerShell.

```
{
"environmentSettings": {
"cloudService": {
"name": "myusernamevm1",
"affinityGroup": "",
"location": "West US",
"virtualNetwork": "",
"subnet": "",
"availabilitySet": "",
"virtualMachine": {
"name": "myusernamevm1",
"vhdImage": "a699494373c04fc0bc8f2bb1389d6106__Win2K8R2SP1-Datacenter-201403.01-en.us-127GB.vhd",
"size": "Small",
"user": "vmuser1",
"password": "",
"enableWebDeployExtension": true,
"endpoints": [
{
"name": "Http",
"protocol": "TCP",
"publicPort": "80",
"privatePort": "80"
},
{
"name": "Https",
"protocol": "TCP",
"publicPort": "443",
"privatePort": "443"
},
{
"name": "WebDeploy",
"protocol": "TCP",
"publicPort": "8172",
"privatePort": "8172"
},
{
"name": "Remote Desktop",
"protocol": "TCP",
"publicPort": "3389",
"privatePort": "3389"
},
{
"name": "Powershell",
"protocol": "TCP",
"publicPort": "5986",
"privatePort": "5986"
}
]
}
},
"databases": [
{
"connectionStringName": "",
"databaseName": "",
"serverName": "",
"user": "",
"password": ""
}
],
"webDeployParameters": {
"iisWebApplicationName": "Default Web Site"
}
}
}
```

U kunt de JSON-configuratie als u wilt wijzigen wat er gebeurt als het uitvoeren van scripts publiceren bewerken. De `cloudService` en `virtualMachine` secties zijn vereist, maar u kunt de `databases` sectie als u niet nodig hebt. De eigenschappen die zijn leeg in het standaard configuratiebestand waarmee Visual Studio is gegenereerd, zijn optioneel. die waarden in het standaardconfiguratiebestand zijn vereist.

Als u een website met meerdere omgevingen (bekend als "slots") in plaats van een enkele productielocatie in Azure, kunt u de sleufnaam in de naam van de website opnemen in het configuratiebestand JSON. Bijvoorbeeld, als u een website hebt die heeft met de naam **mijnsite** en een sleuf voor met de naam **test** vervolgens de URI mijnsite-test.cloudapp.net is, maar de juiste naam te gebruiken in het configuratiebestand is mysite(test). Kunt u alleen doen als de website en sleuven al aanwezig zijn in uw abonnement. Als deze nog niet bestaan, de website maken door het script uit te voeren zonder de sleuf, en vervolgens de sleuf in de [Azure klassieke portal](http://go.microsoft.com/fwlink/?LinkID=213885)maken en vervolgens het script uitgevoerd met de naam van de website gewijzigd. Voor meer informatie over implementatie sleuven voor web apps Zie [staging-omgevingen voor web apps in Azure App-Service](./app-service-web/web-sites-staged-publishing.md).

## <a name="how-to-run-the-publish-scripts"></a>Het uitvoeren van de scripts publiceren

Als u een Windows PowerShell-script voordat nooit hebt uitgevoerd, moet u eerst het uitvoeringsbeleid uitvoeren van scripts inschakelen instellen. Dit is een beveiligingsfunctie om te voorkomen dat gebruikers Windows PowerShell-scripts worden uitgevoerd als ze kwetsbaar voor virussen met betrekking tot het uitvoeren van scripts of andere malware.

### <a name="run-the-script"></a>Het script uitvoeren

1. De implementatie van Web-pakket voor uw project maken. Een implementatie van Web-pakket is een gecomprimeerd archief (ZIP-bestand) die bestanden bevatten die u wilt kopiëren naar uw website of virtuele machine. Kunt u pakketten Web implementeren in Visual Studio voor een webtoepassing.

![Web maken pakket](./media/vs-azure-tools-publishing-using-powershell-scripts/IC767885.png)

Zie voor meer informatie [procedure: een website een distributiepakket maken in Visual Studio](https://msdn.microsoft.com/library/dd465323.aspx). Zoals beschreven in de sectie verderop in dit onderwerp **aanpassen en uitbreiden van de scripts publiceren** , kunt u het maken van uw pakket implementeren Web automatiseren.

1. Open het contextmenu voor het script in de **Solution Explorer**en kies **openen met PowerShell ISE**.

1. Als dit de eerste keer dat u Windows PowerShell-scripts op deze computer hebt uitgevoerd, opent u een opdrachtpromptvenster met Administrator-bevoegdheden en typ de volgende opdracht:

`Set-ExecutionPolicy RemoteSigned`

1. Aanmelden bij Azure met de volgende opdracht.

`Add-AzureAccount`

Wanneer dat wordt gevraagd, Geef uw gebruikersnaam en wachtwoord.

Houd er rekening mee dat wanneer u het script automatiseren, deze methode van de Azure referenties werken niet. In plaats daarvan moet u het bestand .publishsettings referenties op te geven. Één keer alleen u gebruik de opdracht **Get-AzurePublishSettingsFile** het bestand te downloaden van Azure en daarna **Importeren AzurePublishSettingsFile** gebruiken om het bestand te importeren. Zie voor gedetailleerde instructies [het installeren en configureren van Azure PowerShell](powershell-install-configure.md).

1. (Optioneel) Als u Azure bronnen als de virtuele machine, database en website wilt zonder het publiceren van uw webtoepassing te maken, gebruikt u de opdracht **Publiceren WebApplication.ps1** met de **-configuratie** argument ingesteld op het configuratiebestand JSON. Het configuratiebestand JSON deze opdrachtregel gebruikt om te bepalen welke bronnen maken. Omdat deze de standaardinstellingen voor andere argumenten voor de opdrachtregel gebruikt, maakt u de resources, maar niet publiceren van uw webtoepassing. De – Verbose optie vindt u meer informatie over wat er gebeurt.

`Publish-WebApplication.ps1 -Verbose –Configuration C:\Path\WebProject-WAWS-dev.json`

1. Gebruik de opdracht **Publiceren WebApplication.ps1** zoals in een van de volgende voorbeelden voor het aanroepen van het script en publiceren van uw webtoepassing. Als u de standaardinstellingen negeren voor een van de argumenten, zoals de naam van het abonnement, publiceert pakketnaam, virtual machine-referenties of referenties voor de database, kunt u deze parameters kunt opgeven. Gebruik de **– uitgebreid** vindt u meer informatie over de voortgang van het publicatieproces.

```
Publish-WebApplication.ps1 –Configuration C:\Path\WebProject-WAWS-dev-json `
–SubscriptionName Contoso `
-WebDeployPackage C:\Documents\Azure\ADWebApp.zip `
-DatabaseServerPassword @{Name="dbServerName";Password="adminPassword"} `
-Verbose
```

Als u een virtuele machine maakt, wordt de opdracht ziet eruit als het volgende. In dit voorbeeld ziet ook hoe u de referenties voor meerdere databases opgeven. De virtuele machines die deze scripts maakt is het SSL-certificaat niet van een vertrouwde basiscertificeringsinstantie. Daarom moet u de optie **-AllowUntrusted** gebruikt.

```
Publish-WebApplication.ps1 `
-Configuration C:\Path\ADVM-VM-test.json `
-SubscriptionName Contoso `
-WebDeployPackage C:\Path\ADVM.zip `
-AllowUntrusted `
-VMPassword @{name = "vmUserName"; password = "YourPasswordHere"} `
-DatabaseServerPassword @{Name="server1";Password="adminPassword1"}, @{Name="server2";Password="adminPassword2"} `
-Verbose
```

Het script databases kunt maken, maar deze databaseservers niet maken. Als u een databaseserver maakt wilt, kunt u de functie **Nieuw AzureSqlDatabaseServer** in de module Azure.

## <a name="customizing-and-extending-the-publish-scripts"></a>Aanpassen en uitbreiden van de scripts publiceren

U kunt het script publiceren en JSON-configuratiebestand. De functies in de Windows PowerShell-module **AzureWebAppPublishModule.psm1** zijn niet bedoeld om te worden gewijzigd. Als u wilt een andere database opgeven of wijzigen van de eigenschappen van de virtuele machine, bewerk het configuratiebestand JSON. Als u wilt om uit te breiden de functionaliteit van het script voor het automatiseren van maken en testen van uw project, kunt u de functie stubs in **Publiceren WebApplication.ps1**implementeren.

Automatiseren van uw project wordt gebouwd, code toevoegen die roept MSBuild aan `New-WebDeployPackage` zoals in het voorbeeld. Het pad naar de opdracht MSBuild is afhankelijk van de versie van Visual Studio is geïnstalleerd. Als u het juiste pad, kunt u de functie **Get-MSBuildCmd**, zoals in het volgende voorbeeld wordt getoond.

### <a name="to-automate-building-your-project"></a>Voor het automatiseren van het project bouwen

1. Voeg toe de `$ProjectFile` -parameter in de sectie Algemene param.

```
[Parameter(Mandatory = $false)]
  [ValidateScript({Test-Path $_ -PathType Leaf})]
  [String]
  $ProjectFile,
```

1. De functie kopiëren `Get-MSBuildCmd` in het scriptbestand.

```
function Get-MSBuildCmd
{
        process
{

             $path =  Get-ChildItem "HKLM:\SOFTWARE\Microsoft\MSBuild\ToolsVersions\" |
                                   Sort-Object {[double]$_.PSChildName} -Descending |
                                   Select-Object -First 1 |
                                   Get-ItemProperty -Name MSBuildToolsPath |
                                   Select -ExpandProperty MSBuildToolsPath
       
            $path = (Join-Path -Path $path -ChildPath 'msbuild.exe')

        return Get-Item $path
    }
}
```

1. Vervangen `New-WebDeployPackage` met de volgende code en vervang de tijdelijke aanduidingen in de regel maken `$msbuildCmd`. Deze code is voor Visual Studio 2015. Als u Visual Studio 2013, wijzigt u de eigenschap **VisualStudioVersion** hieronder op `12.0`.

```
function New-WebDeployPackage
{
    #Write a function to build and package your web application
      
#To build your web application, use MsBuild.exe. For help, see MSBuild Command-Line Reference at: http://go.microsoft.com/fwlink/?LinkId=391339
      
Write-VerboseWithTime 'Build-WebDeployPackage: Start'
      
$msbuildCmd = '"{0}" "{1}" /T:Rebuild;Package /P:VisualStudioVersion=14.0 /p:OutputPath="{2}\MSBuildOutputPath" /flp:logfile=msbuild.log,v=d' -f (Get-MSBuildCmd), $ProjectFile, $scriptDirectory
      
Write-VerboseWithTime ('Build-WebDeployPackage: ' + $msbuildCmd)
      
#Start execution of the build command
$job = Start-Process cmd.exe -ArgumentList('/C "' + $msbuildCmd + '"') -WindowStyle Normal -Wait -PassThru
      
if ($job.ExitCode -ne 0)
{
throw('MsBuild exited with an error. ExitCode:' + $job.ExitCode)
}

#Obtain the project name
$projectName = (Get-Item $ProjectFile).BaseName
      
#Construct the path to web deploy zip package
$DeployPackageDir =  '.\MSBuildOutputPath\_PublishedWebsites\{0}_Package\{0}.zip' -f $projectName
      
      
#Get the full path for the web deploy zip package. This is required for MSDeploy to work
$WebDeployPackage = Resolve-Path –LiteralPath $DeployPackageDir
      
Write-VerboseWithTime 'Build-WebDeployPackage: End'
      
return $WebDeployPackage
}
```

1. Bel de `New-WebDeployPackage` functie voor deze regel: `$Config = Read-ConfigFile $Configuration` voor web apps of `$Config = Read-ConfigFile $Configuration -HasWebDeployPackage:([Bool]$WebDeployPackage)` voor virtuele machines.

```
if($ProjectFile)
{
$WebDeployPackage = New-WebDeployPackage
}
```

1. Aanroepen van het aangepaste script vanaf de opdrachtregel doorgegeven met de `$Project` argument, zoals in het volgende voorbeeld van de opdrachtregel.

```
.\Publish-WebApplicationVM.ps1 -Configuration .\Configurations\WebApplication5-VM-dev.json `
-ProjectFile ..\WebApplication5\WebApplication5.csproj `
-VMPassword @{Name="VMUser";Password="Test.123"} `
-AllowUntrusted `
-Verbose
```

Voor het automatiseren van uw toepassing testen, code toevoegen aan `Test-WebApplication`. Zorg ervoor dat opmerkingen bij de regels in **Publiceren WebApplication.ps1** waar deze functies worden genoemd. Als u een implementatie niet opgeeft, kunt u handmatig maken van uw project met Visual Studio en voer het script publiceren publiceren naar Azure.

## <a name="publishing-function-summary"></a>Publishing functie overzicht

Als u de help voor functies die u bij de Windows PowerShell-opdrachtprompt gebruiken kunt, gebruikt u de opdracht `Get-Help function-name`. De help bevat de parameter help en voorbeelden. Helptekst is ook in de bronbestanden van script, **AzureWebAppPublishModule.psm1** en **Publiceren-WebApplication.ps1**. Het script en de Help-informatie zijn in uw taal van Visual Studio gelokaliseerd.

**AzureWebAppPublishModule**

|Functienaam|Beschrijving|
|---|---|
|Toevoegen-AzureSQLDatabase|Hiermee maakt u een nieuwe Azure SQL-database.|
|Toevoegen-AzureSQLDatabases|Azure SQL-databases maakt van de waarden in het bestand JSON configuation Visual Studio is gegenereerd.|
|Toevoegen-AzureVM|Een Azure virtuele machine maakt en retourneert de URL van de geïmplementeerde VM. De functie stelt de voorwaarden en roept vervolgens de functie **Nieuw AzureVM** (Azure module) om een nieuwe virtuele machine maken.|
|Toevoegen-AzureVMEndpoints|Nieuwe invoer eindpunten toevoegen aan een virtuele machine en geeft als resultaat de virtuele machine met het nieuwe endpoint.|
|Toevoegen-AzureVMStorage|Maakt een nieuwe opslag Azure-account in het huidige abonnement. De naam van de account die begint met 'devtest', gevolgd door een unieke alfanumerieke tekenreeks. De functie retourneert de naam van de nieuwe opslag-account. Moet u een locatie of een groep affiniteit voor de nieuwe account voor opslag.|
|Toevoegen-AzureWebsite|Een website gemaakt met de opgegeven naam en locatie. Deze functie roept de functie **Nieuw AzureWebsite** in de module Azure. Als het abonnement nog een website met de opgegeven naam niet bevat, wordt deze functie wordt gemaakt van de website en een website-object retourneert. Anders is het resultaat `$null`.|
|Back-up-abonnement|Hiermee slaat u het huidige Azure abonnement in de `$Script:originalSubscription` variabele in het bereik van script. Deze functie slaat de huidige abonnement Azure (verkregen door `Get-AzureSubscription -Current`) en de account voor de opslag en het abonnement dat door dit script wordt gewijzigd (opgeslagen in de variabele `$UserSpecifiedSubscription`) en de opslag, in het bereik van script. Als u de waarden opslaat, kunt u een functie, zoals `Restore-Subscription`, de oorspronkelijke huidige abonnement en opslag account herstellen naar de huidige status als de huidige status is gewijzigd.|
|Zoeken naar AzureVM|Deze eigenschap haalt de opgegeven Azure virtual machine.|
|Indeling DevTestMessageWithTime|Voegt toe de datum en tijd aan een bericht. Deze functie is ontworpen voor berichten in de fout en uitgebreid streams.|
|Get-AzureSQLDatabaseConnectionString|Een verbindingsreeks voor het verbinden met een database van SQL Azure assembleert.|
|Get-AzureVMStorage|Retourneert de naam van de eerste opslag rekening met het naampatroon "devtest*" (niet hoofdlettergevoelig) op de opgegeven locatie of groep affiniteit. Als de "devtest*" opslag account komt niet overeen met de locatie of de groep affiniteit, de functie genegeerd. Moet u een locatie of een groep affiniteit.|
|Get-MSDeployCmd|Deze eigenschap retourneert een opdracht voor het uitvoeren van het hulpprogramma MsDeploy.exe.|
|Nieuwe AzureVMEnvironment|Zoekt of maakt u een virtuele machine in het abonnement dat overeenkomt met de waarden in het configuratiebestand JSON.|
|Publiceren WebPackage|MsDeploy.exe wordt gebruikt en een web publiceren pakket. ZIP-bestand naar bronnen op een website implementeren. Deze functie genereert geen uitvoer. Als de aanroep van MSDeploy.exe niet werkt, genereert de functie een uitzondering. Als u meer gedetailleerde uitvoer, gebruikt de **-uitgebreide** optie.|
|Publiceren WebPackageToVM|Controleert of de waarden van de parameters en roept vervolgens de functie **Publiceren WebPackage** .|
|Lezen-ConfigFile|Het configuratiebestand JSON valideert en geeft als resultaat een hash-tabel van de geselecteerde waarden.|
|Herstellen-abonnement|Het huidige abonnement wordt op de oorspronkelijke abonnement.|
|Test AzureModule|Deze eigenschap retourneert `$true` als de geïnstalleerde Azure moduleversie 0.7.4 of hoger. Deze eigenschap retourneert `$false` als de module is niet geïnstalleerd of een eerdere versie. Deze functie heeft geen parameters.|
|Test AzureModuleVersion|Deze eigenschap retourneert `$true` als de versie van de module Azure 0.7.4 of hoger. Deze eigenschap retourneert `$false` als de module is niet geïnstalleerd of een eerdere versie. Deze functie heeft geen parameters.|
|Test HttpsUrl|De ingevoerde URL wordt omgezet in een System.Uri-object. Deze eigenschap retourneert `$True` als de URL absoluut is, en haar schema https is. Deze eigenschap retourneert `$false` als de URL relatief is, de regeling niet HTTPS of de invoerreeks kan niet worden geconverteerd naar een URL.|
|Test-lid|Deze eigenschap retourneert `$true` als een eigenschap of methode van het object lid is. Anders is het resultaat `$false`.|
|Write-ErrorWithTime|Schrijft een foutbericht dat wordt voorafgegaan door de huidige tijd. Deze functie roept de functie **Format DevTestMessageWithTime** om de tijd voordat het bericht schrijven naar de stream fout Voeg.|
|Write-HostWithTime|Schrijft een bericht naar het hostprogramma (**Write-Host**) voorafgegaan door de huidige tijd. Het effect van het schrijven naar het hostprogramma varieert. De meeste programma's die als host fungeren voor Windows PowerShell schrijven deze berichten naar de standaarduitvoer.|
|Write-VerboseWithTime|Wordt een uitgebreide voorafgegaan door de huidige tijd. Omdat het **Schrijven uitgebreid**wordt aangeroepen, wordt het bericht alleen als het script wordt uitgevoerd met de parameter **uitgebreid** of als de voorkeur voor **VerbosePreference** is ingesteld op **Doorgaan**.|

**Publiceren-webtoepassing**

|Functienaam|Beschrijving|
|---|---|
|Nieuwe AzureWebApplicationEnvironment|Azure bronnen, zoals een website of een virtuele machine maakt.|
|Nieuwe WebDeployPackage|Deze functie is niet geïmplementeerd. In deze functie voor het bouwen van uw project kunt u opdrachten toevoegen.|
|Publiceren AzureWebApplication|Een webtoepassing op Azure publiceert.|
|Publiceren-webtoepassing|Maakt en implementeert Web Apps, virtuele machines, SQL-databases en accounts voor een project van Visual Studio web storage.|
|Test-webtoepassing|Deze functie is niet geïmplementeerd. U kunt opdrachten toevoegen in deze functie om uw toepassing te testen.|

## <a name="next-steps"></a>Volgende stappen

Meer informatie over PowerShell scripts door te lezen [met Windows PowerShell scripts](https://technet.microsoft.com/library/bb978526.aspx) en andere Azure PowerShell scripts in het [Script Center](https://azure.microsoft.com/documentation/scripts/)bekijken.
