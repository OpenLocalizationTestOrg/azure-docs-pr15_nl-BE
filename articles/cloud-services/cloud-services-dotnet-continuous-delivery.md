<properties
    pageTitle="Doorlopende levering voor cloud services met TFS in Azure | Microsoft Azure"
    description="Informatie over het instellen van de continue levering voor Azure cloud-toepassingen. Voorbeelden van code voor de opdrachtregel instructies MSBuild en PowerShell-scripts."
    services="cloud-services"
    documentationCenter=""
    authors="TomArcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="07/30/2016"
    ms.author="tarcher"/>

# <a name="continuous-delivery-for-cloud-services-in-azure"></a>Doorlopende levering voor Cloud Services in Azure

Het proces dat wordt beschreven in dit artikel wordt beschreven hoe u doorlopende levering voor Azure cloud-toepassingen instellen. Dit proces kunt u automatisch pakketten maken en implementeren van het pakket in Azure na elke code inchecken. Het pakket bouwproces beschreven in dit artikel komt overeen met de opdracht **Inpakken** in Visual Studio en publishing stappen komen overeen met de opdracht **publiceren** in Visual Studio.
Het artikel heeft betrekking op de methoden die u gebruikt voor het maken van een gebouwde server vanaf de opdrachtregel MSBuild-instructies en Windows PowerShell-scripts en u ziet ook het desgewenst configureren van Visual Studio Team Foundation Server - Team samenstellen definities MSBuild opdrachten en PowerShell-scripts te gebruiken. Het proces is op maat van uw build omgeving en Azure doel omgevingen.

U kunt ook Visual Studio Team Services, een versie van TFS die wordt gehost in Azure dit gemakkelijk doen. Zie [Continue levering aan Azure door met behulp van Visual Studio Team Services][]voor meer informatie.

Voordat u begint, moet u de toepassing vanuit Visual Studio publiceren.
Hierdoor weet u zeker dat alle resources beschikbaar en geïnitialiseerd zijn wanneer u probeert om de publicatieproces te automatiseren.

## <a name="1-configure-the-build-server"></a>1: de Build-Server configureren

Voordat u een Azure-pakket maken kunt met behulp van MSBuild, moet u de vereiste software en hulpprogramma's installeren op de server maken.

Visual Studio hoeft niet te worden geïnstalleerd op de server maken. Als u wilt Team Foundation bouwen om Service te gebruiken voor het beheren van uw server maken, volg de aanwijzingen in de documentatie [Team Foundation Build-Service][] .

1.  Installeer de [.NET Framework 4.5.2][], inclusief MSBuild op de server maken.
2.  Installeer de meest recente [Azure Authoring Tools for.NET](https://azure.microsoft.com/develop/net/).
3.  Installeer de [Azure bibliotheken voor .NET](http://go.microsoft.com/fwlink/?LinkId=623519).
4.  Kopieer het bestand Microsoft.WebApplication.targets van een installatie van Visual Studio met de server maken.

    Op een computer waarop Visual Studio is geïnstalleerd, dit bestand bevindt zich in de map C:\\programma Files(x86)\\MSBuild\\Microsoft\\VisualStudio\\v14.0\\WebApplications. U moet kopiëren naar dezelfde map op de server maken.
5.  Installeer de [Azure Tools voor Visual Studio](https://www.visualstudio.com/features/azure-tools-vs.aspx).

## <a name="2-build-a-package-using-msbuild-commands"></a>2: een pakket met MSBuild opdrachten maken

In deze sectie wordt beschreven hoe een MSBuild-opdracht een Azure pakket bouwt samenstellen. Deze stap uitvoeren op de server maken om te controleren of alles correct is geconfigureerd en dat de MSBuild-opdracht doet wat u wilt doen. Kunt u deze opdrachtregel toevoegen aan bestaande build scripts op de server maken, of kunt u de opdrachtregel in een definitie TFS bouwen, zoals beschreven in de volgende sectie. Zie [MSBuild opdrachtregel-naslaginformatie](https://msdn.microsoft.com/library/ms164311%28v=vs.140%29.aspx)voor meer informatie over opdrachtregelparameters en MSBuild.

1.  Als Visual Studio is geïnstalleerd op de server maken, zoeken en kiest u **Visual Studio opdracht Prompt** in de map **Visual Studio Tools** in Windows.

    Als Visual Studio niet is geïnstalleerd op de server maken, open een opdrachtprompt en zorg ervoor dat de MSBuild.exe op het pad toegankelijk is. MSBuild is geïnstalleerd met het .NET Framework in het pad % WINDIR %\\Microsoft.NET\\Framework\\*versie*. Bijvoorbeeld wilt toevoegen aan de omgevingsvariabele PATH MSBuild.exe wanneer u .NET Framework 4 is geïnstalleerd, typt u de volgende opdracht bij de opdrachtprompt:

        set PATH=%PATH%;"C:\Windows\Microsoft.NET\Framework\v4.0.30319"

2.  Ga bij de opdrachtprompt naar de map met het Azure-projectbestand dat u wilt maken.

3.  MSBuild worden uitgevoerd met het/target: publiceren de optie als volgt:

        MSBuild /target:Publish

    Deze optie kan worden afgekort tot /t: publiceren. De optie /t:Publish in MSBuild moet niet verwarren met de publicatie-opdrachten die beschikbaar zijn in Visual Studio wanneer u de Azure SDK is geïnstalleerd. De /t: publiceren de optie alleen builds Azure pakketten. Het heeft de pakketten niet implementeren als de publicatie-opdrachten in Visual Studio.

    Desgewenst kunt u de naam van het project als een parameter MSBuild. Als u niet opgeeft, wordt de huidige map gebruikt. Zie [MSBuild-opdrachtregel-naslaginformatie](https://msdn.microsoft.com/library/ms164311%28v=vs.140%29.aspx)voor meer informatie over opdrachtregelopties voor MSBuild.

4.  Zoek de uitvoer. Met deze opdracht maakt standaard een map in de hoofdmap van het project, zoals *ProjectDir*\\bin\\*configuratie*\\app.publish\\. Wanneer u een project Azure, genereert u twee bestanden, het pakketbestand zelf en de bijbehorende configuratiebestand:

    -   Project.cspkg
    -   ServiceConfiguration. *TargetProfile*.cscfg

    Standaard elke Azure project bevat een service configuratiebestand (.cscfg) voor de opbouw van lokale (foutopsporing) en een andere voor de opbouw van de wolk (gefaseerde installatie of productie), maar u kunt toevoegen of verwijderen van service-configuratiebestanden wanneer dat nodig is. Wanneer u een pakket binnen Visual Studio, wordt u gevraagd welke service configuratiebestand naast het pakket opnemen.

5.  Geef het configuratiebestand van de service. Wanneer u een pakket maken met behulp van MSBuild, wordt het configuratiebestand van de lokale service standaard opgenomen. Als u wilt opnemen in het configuratiebestand van een andere service, stelt u de eigenschap TargetProfile van de MSBuild-opdracht, zoals in het volgende voorbeeld:

        MSBuild /t:Publish /p:TargetProfile=Cloud

6.  Geef de locatie voor de uitvoer. Het pad instellen met behulp van de /p:PublishDir =*map* \\ optie, met inbegrip van de afsluitende backslash scheidingsteken, zoals in het volgende voorbeeld:

        MSBuild /target:Publish /p:PublishDir=\\myserver\drops\

    Zodra u hebt gebouwd en getest een juiste MSBuild-opdrachtregel om te bouwen van uw projecten en deze combineren in een Azure-pakket, kunt u deze opdrachtregel kunt toevoegen aan uw scripts bouwen. Als uw server bouwen aangepaste scripts gebruikt, afhankelijk dit proces van de specifieke kenmerken van het proces van het bouwen. Als u TFS als een build-omgeving gebruikt, kunt u de instructies in de volgende stap de build Azure pakket toevoegen aan uw bouwproces volgen.

## <a name="3-build-a-package-using-tfs-team-build"></a>3: een pakket met het bouwen van TFS-Team samenstellen

Als u Team Foundation Server (TFS) instellen als een build-controller en de build-server instellen als een machine TFS build en vervolgens kunt u desgewenst instellen een geautomatiseerde versie voor uw pakket Azure. Zie voor meer informatie over het instellen en Team Foundation server gebruiken als een systeem opbouwen, [geschaald uitbreiden van uw systeem maken][]. Met name de volgende procedure wordt ervan uitgegaan dat u uw build-server hebt geconfigureerd zoals beschreven in [implementeren en configureren van een server build][], en dat u een teamproject hebt gemaakt een cloud service project gemaakt in het teamproject.

Configureren van TFS Azure pakketten samenstellen, kunt u de volgende stappen uitvoeren:

1.  In Visual Studio op de computer, in het menu Beeld, kiest u **Explorer Team**of kies Ctrl +\\, Ctrl + M. Vouw het knooppunt **maakt** in het venster Explorer-Team of kies de pagina **maakt** en kiest u **Nieuwe definitie maken**.

    ![Nieuwe definitie maken, optie][0]

2.  Kies het tabblad **Trigger** en de gewenste voorwaarden opgeven voor wanneer u het pakket moet worden opgebouwd. Geef bijvoorbeeld **Continue integratie** om het pakket te maken wanneer een bron controle check-in.

3.  Klik op het tabblad **Broninstellingen** en zorg ervoor dat uw projectmap wordt weergegeven in de kolom **Map Control** en de status **actief**is.

4.  Kies het tabblad **Standaardinstellingen maken** en controleer of de naam van de server bouwen onder de Build-controller.  Ook de optie **kopie uitvoer naar de volgende map maken** en de gewenste doellocatie opgeven.

5.  Kies het tabblad **proces** . Op het tabblad proces de standaardsjabloon onder **maken**kiest, kiest u het project als deze nog niet is geselecteerd, en vouw de sectie **Geavanceerd** in de sectie **bouw** van het raster.

6.  Kies **MSBuild argumenten**en de juiste MSBuild-opdrachtregelargumenten instellen, zoals beschreven in stap 2 hierboven. Voer bijvoorbeeld **/t: publiceren van /p:PublishDir =\\\\MijnServer\\daalt\\ ** een pakket maken en de bestanden kopiëren naar de locatie \\ \\MijnServer\\daalt\\:

    ![MSBuild-argumenten][2]

    **Opmerking:** Het kopiëren van bestanden naar een openbare share gemakkelijker de pakketten van de computer handmatig te implementeren.

5.  Het succes van uw build stap testen door een wijziging in het project is ingecheckt of wachtrijen voor een nieuwe build. **Alle definities van bouwen,** met de rechtermuisknop en kies vervolgens **Nieuwe Build wachtrij**om de wachtrij van een nieuwe build, in het Explorer-Team.

## <a name="4-publish-a-package-using-a-powershell-script"></a>4: een pakket met een PowerShell Script publiceren

Deze sectie wordt beschreven hoe u een Windows PowerShell-script dat de uitvoer van Cloud app pakket publiceert op Azure met optionele parameters opgeven. Dit script kan worden aangeroepen nadat de build stap in uw automatisering bouwen. Het kan ook worden aangeroepen vanuit sjabloon werkstroomactiviteiten in Visual Studio TFS Team samenstellen.

1.  Installeer de [Azure PowerShell-cmdlets][] (v0.6.1 of hoger).
    Tijdens de installatiefase cmdlet wilt installeren als een module. Houd er rekening mee dat deze officieel ondersteunde versie wordt vervangen door de oudere versie aangeboden via CodePlex, hoewel de vorige versies zijn genummerd 2.x.x.

2.  Start Azure PowerShell via het menu Start of de startpagina. Als u op deze manier start, worden de Azure PowerShell-cmdlets wordt geladen.

3.  Controleren of de PowerShell cmdlets worden geladen door de gedeeltelijke opdracht achter de PowerShell-prompt `Get-Azure` en druk vervolgens op de Tab-toets voor de voltooiing van instructies.

    Als u herhaaldelijk op de Tab-toets drukt, worden er diverse Azure PowerShell-opdrachten.

4.  Controleer of dat u verbinding met uw abonnement Azure maken kunt uw door abonnementsgegevens te importeren uit het bestand .publishsettings.

    `Import-AzurePublishSettingsFile c:\scripts\WindowsAzure\default.publishsettings`

    Voer vervolgens de opdracht

    `Get-AzureSubscription`

    Dit geeft informatie over uw abonnement. Controleer of alles juist is.

4.  Het script opslaan die aan het einde van dit artikel in de scriptmap van c:\\scripts\\WindowsAzure\\**PublishCloudService.ps1**.

5.  Raadpleeg de sectie parameters van het script. Toevoegen of wijzigen van de standaardwaarden. Deze waarden kunnen altijd worden overschreven door doorgeven in expliciete parameters.

6.  Zorg er geldige cloud service- en accounts worden gemaakt van uw abonnement op die door het script publiceren kan worden gericht. De opslag-account (blob storage) wordt gebruikt voor het uploaden en de implementatie van pakket en config-bestand tijdelijk op te slaan tijdens de installatie wordt gemaakt.

    -   U maakt een nieuwe wolk service, kan u dit script aanroepen of de [Azure klassieke portal](http://go.microsoft.com/fwlink/?LinkID=213885). De naam van de cloud-service wordt gebruikt als voorvoegsel in een volledig gekwalificeerde domeinnaam en dus het moet uniek zijn.

            New-AzureService -ServiceName "mytestcloudservice" -Location "North Central US" -Label "mytestcloudservice"

    -   U kunt dit script aanroepen of de [Azure klassieke portal](http://go.microsoft.com/fwlink/?LinkID=213885)gebruiken om een nieuwe opslag-account maken. De naam van de opslag wordt gebruikt als voorvoegsel in een volledig gekwalificeerde domeinnaam en dus het moet uniek zijn. U kunt met dezelfde naam als de service cloud.

            New-AzureStorageAccount -ServiceName "mytestcloudservice" -Location "North Central US" -Label "mytestcloudservice"

7.  Aanroepen van het script rechtstreeks vanuit Azure PowerShell of op wire-aan de automatisering van uw host build na de pakket-versie van dit script.

    >[AZURE.IMPORTANT] Het script wordt altijd verwijderen of vervangen van uw bestaande installaties standaard, als ze worden aangetroffen. Dit is nodig om een continue levering van automatisering in wanneer er geen gebruiker vragen mogelijk is.

    **Voorbeeldscenario 1:** continue implementeren in de testomgeving van een service:

        PowerShell c:\scripts\windowsazure\PublishCloudService.ps1 -environment Staging -serviceName mycloudservice -storageAccountName mystoragesaccount -packageLocation c:\drops\app.publish\ContactManager.Azure.cspkg -cloudConfigLocation c:\drops\app.publish\ServiceConfiguration.Cloud.cscfg -subscriptionDataFile c:\scripts\default.publishsettings

    Dit is meestal opgevolgd door controle en een VIP-swap test. De VIP-swap kan worden gedaan via de [Azure klassieke portal](http://go.microsoft.com/fwlink/?LinkID=213885) of via de cmdlet verplaatsen-implementatie.

    **Voorbeeldscenario 2:** continue implementatie in de productieomgeving van een speciale test-service

        PowerShell c:\scripts\windowsazure\PublishCloudService.ps1 -environment Production -enableDeploymentUpgrade 1 -serviceName mycloudservice -storageAccountName mystorageaccount -packageLocation c:\drops\app.publish\ContactManager.Azure.cspkg -cloudConfigLocation c:\drops\app.publish\ServiceConfiguration.Cloud.cscfg -subscriptionDataFile c:\scripts\default.publishsettings

    **Extern bureaublad:**

    Als Extern bureaublad is ingeschakeld in uw Azure project, u moet mogelijk aanvullende eenmalige stappen om dat het juiste certificaat voor Cloud-Service op alle cloud services die het doelwit van dit script wordt geüpload.

    Zoek het certificaat vingerafdruk waarden door de rollen wordt verwacht. De vingerafdruk-waarden worden weergegeven in het gedeelte van de certificaten van de wolk config-bestand (dat wil zeggen ServiceConfiguration.Cloud.cscfg). Het is ook zichtbaar in het dialoogvenster configuratie voor extern bureaublad in Visual Studio wanneer u opties en de weergave van het geselecteerde certificaat.

        <Certificates>
              <Certificate name="Microsoft.WindowsAzure.Plugins.RemoteAccess.PasswordEncryption" thumbprint="C33B6C432C25581601B84C80F86EC2809DC224E8" thumbprintAlgorithm="sha1" />
        </Certificates>

    Certificaten voor extern bureaublad als een eenmalige installatie stap met behulp van de volgende cmdlet script uploaden:

        Add-AzureCertificate -serviceName <CLOUDSERVICENAME> -certToDeploy (get-item cert:\CurrentUser\MY\<THUMBPRINT>)

    Bijvoorbeeld:

        Add-AzureCertificate -serviceName 'mytestcloudservice' -certToDeploy (get-item cert:\CurrentUser\MY\C33B6C432C25581601B84C80F86EC2809DC224E8

    U kunt ook het bestand voor het pfx-met de persoonlijke sleutel exporteren en certificaten uploaden naar elk doel cloud service met de [Azure klassieke portal](http://go.microsoft.com/fwlink/?LinkID=213885). 
    Lees het volgende artikel voor meer informatie: [http://msdn.microsoft.com/library/windowsazure/gg443832.aspx] [].

    **Implementatie implementatie vs. verwijderen - upgrade\> nieuwe distributie**

    Het script wordt standaard uitgevoerd een Upgrade-installatie ($enableDeploymentUpgrade = 1) als er geen parameter wordt doorgegeven of de waarde 1 expliciet wordt doorgegeven. Dit heeft het voordeel van het nemen van minder tijd dan een volledige implementatie voor enkele instanties. Voor exemplaren die hoge beschikbaarheid, die dit ook het voordeel heeft van het verlaten van sommige exemplaren die worden uitgevoerd terwijl anderen moeten worden bijgewerkt (het domein van de update lopen), plus de VIP worden niet verwijderd.

    Upgrade-implementatie kan worden uitgeschakeld in het script ($enableDeploymentUpgrade = 0) of door *- enableDeploymentUpgrade 0* wordt doorgegeven als parameter, die het gedrag van het script op elke bestaande installatie eerst verwijderen en maak vervolgens een nieuwe implementatie wijzigt.

    >[AZURE.IMPORTANT] Het script wordt altijd verwijderen of vervangen van uw bestaande installaties standaard, als ze worden aangetroffen. Dit is nodig om een continue levering van automatisering in wanneer er geen gebruiker/operator vragen mogelijk is.

## <a name="5-publish-a-package-using-tfs-team-build"></a>5: een pakket met behulp van TFS Team samenstellen publiceren

Deze optionele stap verbindt TFS Team samenstellen voor het script dat in stap 4 hebt gemaakt die omgaat met het publiceren van de build van het pakket met Azure. Dit houdt in dat de sjabloon gebruikt door uw definitie opbouwen zodat deze een activiteit publiceren wordt uitgevoerd aan het einde van de werkstroom wijzigen. De activiteit publiceren wordt de PowerShell-opdracht geven in de parameters van de build uitgevoerd. Uitvoer van de MSBuild-doelen en publiceren van script zal worden doorgesluisd naar de standard build-uitvoer.

1.  Bewerk de definitie bouwen die verantwoordelijk is voor continue implementeren.

2.  Selecteer het tabblad **proces** .

3.  Volg [deze instructies](http://msdn.microsoft.com/library/dd647551.aspx) om een activiteit-project voor de sjabloon build proces toevoegen, downloaden van de standaardsjabloon, toe te voegen aan het project en het inchecken. Geef de build processjabloon een nieuwe naam, zoals AzureBuildProcessTemplate.

3.  Terug naar het tabblad **proces** en **Details weergeven** met een lijst met beschikbare build processjablonen weergeven. Klik op de knop **Nieuw...** en Ga naar het project dat u zojuist hebt toegevoegd en ingecheckt. Zoek de sjabloon die u zojuist hebt gemaakt en klik op **OK**.

4.  De geselecteerde sjabloon voor bewerken openen. U kunt rechtstreeks in de werkstroomontwerper of in de XML-editor om te werken met de XAML openen.

5.  De volgende lijst met nieuwe argumenten toevoegen als afzonderlijke posten op het tabblad van de argumenten van de werkstroomontwerper. Alle argumenten moeten hebben richting = In en typ = tekenreeks. Deze wordt gebruikt voor parameters van de stroom van de build-definitie in de werkstroom, die vervolgens gebruikt ophalen voor het aanroepen van het script publiceren.

        SubscriptionName
        StorageAccountName
        CloudConfigLocation
        PackageLocation
        Environment
        SubscriptionDataFileLocation
        PublishScriptLocation
        ServiceName

    ![Lijst met argumenten][3]

    De bijbehorende XAML ziet er zo uit:

        <Activity  _ />
          <x:Members>
            <x:Property Name="BuildSettings" Type="InArgument(mtbwa:BuildSettings)" />
            <x:Property Name="TestSpecs" Type="InArgument(mtbwa:TestSpecList)" />
            <x:Property Name="BuildNumberFormat" Type="InArgument(x:String)" />
            <x:Property Name="CleanWorkspace" Type="InArgument(mtbwa:CleanWorkspaceOption)" />
            <x:Property Name="RunCodeAnalysis" Type="InArgument(mtbwa:CodeAnalysisOption)" />
            <x:Property Name="SourceAndSymbolServerSettings" Type="InArgument(mtbwa:SourceAndSymbolServerSettings)" />
            <x:Property Name="AgentSettings" Type="InArgument(mtbwa:AgentSettings)" />
            <x:Property Name="AssociateChangesetsAndWorkItems" Type="InArgument(x:Boolean)" />
            <x:Property Name="CreateWorkItem" Type="InArgument(x:Boolean)" />
            <x:Property Name="DropBuild" Type="InArgument(x:Boolean)" />
            <x:Property Name="MSBuildArguments" Type="InArgument(x:String)" />
            <x:Property Name="MSBuildPlatform" Type="InArgument(mtbwa:ToolPlatform)" />
            <x:Property Name="PerformTestImpactAnalysis" Type="InArgument(x:Boolean)" />
            <x:Property Name="CreateLabel" Type="InArgument(x:Boolean)" />
            <x:Property Name="DisableTests" Type="InArgument(x:Boolean)" />
            <x:Property Name="GetVersion" Type="InArgument(x:String)" />
            <x:Property Name="PrivateDropLocation" Type="InArgument(x:String)" />
            <x:Property Name="Verbosity" Type="InArgument(mtbw:BuildVerbosity)" />
            <x:Property Name="Metadata" Type="mtbw:ProcessParameterMetadataCollection" />
            <x:Property Name="SupportedReasons" Type="mtbc:BuildReason" />
            <x:Property Name="SubscriptionName" Type="InArgument(x:String)" />
            <x:Property Name="StorageAccountName" Type="InArgument(x:String)" />
            <x:Property Name="CloudConfigLocation" Type="InArgument(x:String)" />
            <x:Property Name="PackageLocation" Type="InArgument(x:String)" />
            <x:Property Name="Environment" Type="InArgument(x:String)" />
            <x:Property Name="SubscriptionDataFileLocation" Type="InArgument(x:String)" />
            <x:Property Name="PublishScriptLocation" Type="InArgument(x:String)" />
            <x:Property Name="ServiceName" Type="InArgument(x:String)" />
          </x:Members>

          <this:Process.MSBuildArguments>

6.  Een nieuwe reeks toegevoegd aan het einde van de Agent worden uitgevoerd op:

    1.  Gestart door een instructie als activiteit om te controleren op een geldig script-bestand toe te voegen. Stel in op deze waarde:

            Not String.IsNullOrEmpty(PublishScriptLocation)

    2.  Zaak in de tijd van de If-instructie, een nieuwe reeks-activiteit toevoegen. Stelt de weergavenaam 'Start publiceren'

    3.  Met het begin publiceren reeks nog steeds is geselecteerd, de volgende lijst met nieuwe variabelen toevoegen als afzonderlijke posten op het tabblad variabelen van de werkstroomontwerper. Alle variabelen moeten hebben type variabele = String en Scope = Start publiceren. Deze wordt gebruikt voor parameters van de stroom van de build-definitie in de werkstroom, die vervolgens gebruikt ophalen voor het aanroepen van het script publiceren.

        -   SubscriptionDataFilePath van het type String

        -   PublishScriptFilePath van het type String

            ![Nieuwe variabelen][4]

    4.  Als u TFS 2012 of eerder, een ConvertWorkspaceItem activiteit aan het begin van de nieuwe reeks toevoegen. Als u TFS 2013 of later een GetLocalPath activiteit aan het begin van de nieuwe reeks toevoegen. Voor een ConvertWorkspaceItem, de eigenschappen als volgt instellen: richting = ServerToLocal, DisplayName = 'Convert publiceert script bestandsnaam', Input = PublishScriptLocation, resultaat = PublishScriptFilePath, werkruimte = 'Werkruimte'. Stel de eigenschap IncomingPath op 'PublishScriptLocation' en het resultaat naar 'PublishScriptFilePath' voor een GetLocalPath-activiteit. Deze activiteit omgezet in het pad naar het script publiceren van TFS serverlocaties (indien van toepassing) op een lokale schijf standaard pad.

    5.  Als u TFS 2012 of eerder, een andere ConvertWorkspaceItem activiteit aan het einde van de nieuwe reeks toevoegen. Richting ServerToLocal, DisplayName = = 'Bestandsnaam abonnement omzetten,' Input = SubscriptionDataFileLocation, resultaat = SubscriptionDataFilePath, werkruimte = 'Werkruimte'. Als u TFS 2013 of later een andere GetLocalPath toevoegen. IncomingPath = 'SubscriptionDataFileLocation', en het resultaat = 'SubscriptionDataFilePath'.

    6.  Een InvokeProcess activiteit toevoegen aan het einde van de nieuwe reeks.
        Deze activiteit roept PowerShell.exe met de argumenten die zijn doorgegeven door de definitie van de bouw.

        1.  Argumenten = String.Format ("-bestand"' {0}"" - servicenaam {1} - storageAccountName {2} - packageLocation "" {3}"" - cloudConfigLocation "" {4}"" - subscriptionDataFile "" {5}"" selectedSubscription - {6}-omgeving "" {7}"" ", PublishScriptFilePath, ServiceName, StorageAccountName, PackageLocation, CloudConfigLocation, SubscriptionDataFilePath, SubscriptionName, milieu)

        2.  DisplayName = Execute script publiceren

        3.  FileName = 'PowerShell' (inclusief de aanhalingstekens)

        4.  OutputEncoding = System.Text.Encoding.GetEncoding(System.Globalization.CultureInfo.InstalledUICulture.TextInfo.OEMCodePage)

    7.  In de sectie **Verwerken standaarduitvoer** waarde textbox van de InvokeProcess, de textbox op 'gegevens'. Dit is een variabele voor het opslaan van de gegevens over de standaard output.

    8.  Een activiteit WriteBuildMessage net onder de sectie **Verwerken standaarduitvoer** toevoegen. De urgentie = 'Microsoft.TeamFoundation.Build.Client.BuildMessageImportance.High' en het bericht = 'data'. Hierdoor worden de standaard uitvoer van het script wordt weggeschreven naar de build-uitvoer.

    9.  In het gedeelte **Uitvoer van foutmeldingen verwerken** waarde textbox van de InvokeProcess, de textbox op 'gegevens'. Dit is een variabele voor het opslaan van de foutgegevens standaard.

    10. Een activiteit WriteBuildError net onder de sectie **Uitvoer van foutmeldingen verwerken** toevoegen. Stel het bericht = 'data'. Hierdoor worden dat de standaard fouten van het script wordt weggeschreven naar de uitvoer van de fout maken.

    11. Corrigeer eventuele fouten, aangegeven met een blauw uitroepteken plaatsen. De muisaanwijzer op de merken uitroepteken om een hint over de fout. De werkstroom fouten schakelt opslaan.

    In de ontwerpfunctie voor er het uiteindelijke resultaat van de activiteiten van de werkstroom publiceren als volgt:

    ![Workflowactiviteiten][5]

    Het uiteindelijke resultaat van de activiteiten van de werkstroom publiceren ziet er als volgt in XAML:

        <If Condition="[Not String.IsNullOrEmpty(PublishScriptLocation)]" sap2010:WorkflowViewState.IdRef="If_1">
            <If.Then>
              <Sequence DisplayName="Start Publish" sap2010:WorkflowViewState.IdRef="Sequence_4">
                <Sequence.Variables>
                  <Variable x:TypeArguments="x:String" Name="SubscriptionDataFilePath" />
                  <Variable x:TypeArguments="x:String" Name="PublishScriptFilePath" />
                </Sequence.Variables>
                <mtbwa:ConvertWorkspaceItem DisplayName="Convert publish script filename" sap2010:WorkflowViewState.IdRef="ConvertWorkspaceItem_1" Input="[PublishScriptLocation]" Result="[PublishScriptFilePath]" Workspace="[Workspace]" />
                <mtbwa:ConvertWorkspaceItem DisplayName="Convert subscription filename" sap2010:WorkflowViewState.IdRef="ConvertWorkspaceItem_2" Input="[SubscriptionDataFileLocation]" Result="[SubscriptionDataFilePath]" Workspace="[Workspace]" />
                <mtbwa:InvokeProcess Arguments="[String.Format(&quot; -File &quot;&quot;{0}&quot;&quot; -serviceName {1}&#xD;&#xA;            -storageAccountName {2} -packageLocation &quot;&quot;{3}&quot;&quot;&#xD;&#xA;            -cloudConfigLocation &quot;&quot;{4}&quot;&quot; -subscriptionDataFile &quot;&quot;{5}&quot;&quot;&#xD;&#xA;            -selectedSubscription {6} -environment &quot;&quot;{7}&quot;&quot;&quot;,&#xD;&#xA;            PublishScriptFilePath, ServiceName, StorageAccountName,&#xD;&#xA;            PackageLocation, CloudConfigLocation,&#xD;&#xA;            SubscriptionDataFilePath, SubscriptionName, Environment)]" DisplayName="'Execute Publish Script'" FileName="[PowerShell]" sap2010:WorkflowViewState.IdRef="InvokeProcess_1">
                  <mtbwa:InvokeProcess.ErrorDataReceived>
                    <ActivityAction x:TypeArguments="x:String">
                      <ActivityAction.Argument>
                        <DelegateInArgument x:TypeArguments="x:String" Name="data" />
                      </ActivityAction.Argument>
                      <mtbwa:WriteBuildError Message="{x:Null}" sap2010:WorkflowViewState.IdRef="WriteBuildError_1" />
                    </ActivityAction>
                  </mtbwa:InvokeProcess.ErrorDataReceived>
                  <mtbwa:InvokeProcess.OutputDataReceived>
                    <ActivityAction x:TypeArguments="x:String">
                      <ActivityAction.Argument>
                        <DelegateInArgument x:TypeArguments="x:String" Name="data" />
                      </ActivityAction.Argument>
                      <mtbwa:WriteBuildMessage sap2010:WorkflowViewState.IdRef="WriteBuildMessage_2" Importance="[Microsoft.TeamFoundation.Build.Client.BuildMessageImportance.High]" Message="[data]" mva:VisualBasic.Settings="Assembly references and imported namespaces serialized as XML namespaces" />
                    </ActivityAction>
                  </mtbwa:InvokeProcess.OutputDataReceived>
                </mtbwa:InvokeProcess>
              </Sequence>
            </If.Then>
          </If>
        </Sequence>


7.  Het build proces sjabloon workflow en controleer In dit bestand opslaan.

8.  Bewerk de definitie build (sluit als het al geopend is), en klik op de knop **Nieuw** als u nog niet de nieuwe sjabloon in de lijst met sjablonen proces ziet.

9.  De parameter waarden van eigenschappen in de sectie Diversen als volgt instellen:

    1.  CloudConfigLocation = "c:\\daalt\\app.publish\\ServiceConfiguration.Cloud.cscfg' *Deze waarde is afgeleid van: ($PublishDir)ServiceConfiguration.Cloud.cscfg*

    2.  PackageLocation = "c:\\daalt\\app.publish\\ContactManager.Azure.cspkg' *Deze waarde is afgeleid van: ($PublishDir)($ProjectName) .cspkg*

    3.  PublishScriptLocation = "c:\\scripts\\WindowsAzure\\PublishCloudService.ps1"

    4.  Servicenaam = 'mycloudservicename' *Gebruik de juiste cloud naam hier*

    5.  Omgeving = "Staging"

    6.  StorageAccountName = 'mystorageaccountname' *Gebruik de naam van de juiste opslag hier*

    7.  SubscriptionDataFileLocation = "c:\\scripts\\WindowsAzure\\Subscription.xml"

    8.  SubscriptionName = 'default'

    ![Waarden van de eigenschap parameter][6]

10. Sla de wijzigingen in de definitie van de bouw.

11. Wachtrij een Build uit te voeren zowel het pakket samenstellen en publiceren. Als u een trigger instellen op continue integratie, wordt u dit probleem uitgevoerd op elk selectievakje in.

### <a name="publishcloudserviceps1-script-template"></a>Sjabloon voor PublishCloudService.ps1 script

```
Param(  $serviceName = "",
        $storageAccountName = "",
        $packageLocation = "",
        $cloudConfigLocation = "",
        $environment = "Staging",
        $deploymentLabel = "ContinuousDeploy to $servicename",
        $timeStampFormat = "g",
        $alwaysDeleteExistingDeployments = 1,
        $enableDeploymentUpgrade = 1,
        $selectedsubscription = "default",
        $subscriptionDataFile = ""
     )


function Publish()
{
    $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot -ErrorVariable a -ErrorAction silentlycontinue
    if ($a[0] -ne $null)
    {
        Write-Output "$(Get-Date -f $timeStampFormat) - No deployment is detected. Creating a new deployment. "
    }
    #check for existing deployment and then either upgrade, delete + deploy, or cancel according to $alwaysDeleteExistingDeployments and $enableDeploymentUpgrade boolean variables
    if ($deployment.Name -ne $null)
    {
        switch ($alwaysDeleteExistingDeployments)
        {
            1
            {
                switch ($enableDeploymentUpgrade)
                {
                    1  #Update deployment inplace (usually faster, cheaper, won't destroy VIP)
                    {
                        Write-Output "$(Get-Date -f $timeStampFormat) - Deployment exists in $servicename.  Upgrading deployment."
                        UpgradeDeployment
                    }
                    0  #Delete then create new deployment
                    {
                        Write-Output "$(Get-Date -f $timeStampFormat) - Deployment exists in $servicename.  Deleting deployment."
                        DeleteDeployment
                        CreateNewDeployment

                    }
                } # switch ($enableDeploymentUpgrade)
            }
            0
            {
                Write-Output "$(Get-Date -f $timeStampFormat) - ERROR: Deployment exists in $servicename.  Script execution cancelled."
                exit
            }
        } #switch ($alwaysDeleteExistingDeployments)
    } else {
            CreateNewDeployment
    }
}

function CreateNewDeployment()
{
    write-progress -id 3 -activity "Creating New Deployment" -Status "In progress"
    Write-Output "$(Get-Date -f $timeStampFormat) - Creating New Deployment: In progress"

    $opstat = New-AzureDeployment -Slot $slot -Package $packageLocation -Configuration $cloudConfigLocation -label $deploymentLabel -ServiceName $serviceName

    $completeDeployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $completeDeploymentID = $completeDeployment.deploymentid

    write-progress -id 3 -activity "Creating New Deployment" -completed -Status "Complete"
    Write-Output "$(Get-Date -f $timeStampFormat) - Creating New Deployment: Complete, Deployment ID: $completeDeploymentID"

    StartInstances
}

function UpgradeDeployment()
{
    write-progress -id 3 -activity "Upgrading Deployment" -Status "In progress"
    Write-Output "$(Get-Date -f $timeStampFormat) - Upgrading Deployment: In progress"

    # perform Update-Deployment
    $setdeployment = Set-AzureDeployment -Upgrade -Slot $slot -Package $packageLocation -Configuration $cloudConfigLocation -label $deploymentLabel -ServiceName $serviceName -Force

    $completeDeployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $completeDeploymentID = $completeDeployment.deploymentid

    write-progress -id 3 -activity "Upgrading Deployment" -completed -Status "Complete"
    Write-Output "$(Get-Date -f $timeStampFormat) - Upgrading Deployment: Complete, Deployment ID: $completeDeploymentID"
}

function DeleteDeployment()
{

    write-progress -id 2 -activity "Deleting Deployment" -Status "In progress"
    Write-Output "$(Get-Date -f $timeStampFormat) - Deleting Deployment: In progress"

    #WARNING - always deletes with force
    $removeDeployment = Remove-AzureDeployment -Slot $slot -ServiceName $serviceName -Force

    write-progress -id 2 -activity "Deleting Deployment: Complete" -completed -Status $removeDeployment
    Write-Output "$(Get-Date -f $timeStampFormat) - Deleting Deployment: Complete"

}

function StartInstances()
{
    write-progress -id 4 -activity "Starting Instances" -status "In progress"
    Write-Output "$(Get-Date -f $timeStampFormat) - Starting Instances: In progress"

    $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $runstatus = $deployment.Status

    if ($runstatus -ne 'Running')
    {
        $run = Set-AzureDeployment -Slot $slot -ServiceName $serviceName -Status Running
    }
    $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $oldStatusStr = @("") * $deployment.RoleInstanceList.Count

    while (-not(AllInstancesRunning($deployment.RoleInstanceList)))
    {
        $i = 1
        foreach ($roleInstance in $deployment.RoleInstanceList)
        {
            $instanceName = $roleInstance.InstanceName
            $instanceStatus = $roleInstance.InstanceStatus

            if ($oldStatusStr[$i - 1] -ne $roleInstance.InstanceStatus)
            {
                $oldStatusStr[$i - 1] = $roleInstance.InstanceStatus
                Write-Output "$(Get-Date -f $timeStampFormat) - Starting Instance '$instanceName': $instanceStatus"
            }

            write-progress -id (4 + $i) -activity "Starting Instance '$instanceName'" -status "$instanceStatus"
            $i = $i + 1
        }

        sleep -Seconds 1

        $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    }

    $i = 1
    foreach ($roleInstance in $deployment.RoleInstanceList)
    {
        $instanceName = $roleInstance.InstanceName
        $instanceStatus = $roleInstance.InstanceStatus

        if ($oldStatusStr[$i - 1] -ne $roleInstance.InstanceStatus)
        {
            $oldStatusStr[$i - 1] = $roleInstance.InstanceStatus
            Write-Output "$(Get-Date -f $timeStampFormat) - Starting Instance '$instanceName': $instanceStatus"
        }

        $i = $i + 1
    }

    $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $opstat = $deployment.Status

    write-progress -id 4 -activity "Starting Instances" -completed -status $opstat
    Write-Output "$(Get-Date -f $timeStampFormat) - Starting Instances: $opstat"
}

function AllInstancesRunning($roleInstanceList)
{
    foreach ($roleInstance in $roleInstanceList)
    {
        if ($roleInstance.InstanceStatus -ne "ReadyRole")
        {
            return $false
        }
    }

    return $true
}

#configure powershell with Azure 1.7 modules
Import-Module Azure

#configure powershell with publishsettings for your subscription
$pubsettings = $subscriptionDataFile
Import-AzurePublishSettingsFile $pubsettings
Set-AzureSubscription -CurrentStorageAccountName $storageAccountName -SubscriptionName $selectedsubscription
Select-AzureSubscription $selectedsubscription

#set remaining environment variables for Azure cmdlets
$subscription = Get-AzureSubscription $selectedsubscription
$subscriptionname = $subscription.subscriptionname
$subscriptionid = $subscription.subscriptionid
$slot = $environment

#main driver - publish & write progress to activity log
Write-Output "$(Get-Date -f $timeStampFormat) - Azure Cloud Service deploy script started."
Write-Output "$(Get-Date -f $timeStampFormat) - Preparing deployment of $deploymentLabel for $subscriptionname with Subscription ID $subscriptionid."

Publish

$deployment = Get-AzureDeployment -slot $slot -serviceName $servicename
$deploymentUrl = $deployment.Url

Write-Output "$(Get-Date -f $timeStampFormat) - Created Cloud Service with URL $deploymentUrl."
Write-Output "$(Get-Date -f $timeStampFormat) - Azure Cloud Service deploy script finished."
```

## <a name="next-steps"></a>Volgende stappen

Zie foutopsporing op afstand bij het gebruik van continue levering inschakelen [bij het gebruik van continue levering publiceren naar Azure foutopsporing op afstand inschakelen](cloud-services-virtual-machines-dotnet-continuous-delivery-remote-debugging.md).

  [Doorlopende levering aan Azure met behulp van Visual Studio teamservices]: cloud-services-continuous-delivery-use-vso.md  
  [Team Foundation Build-Service]: https://msdn.microsoft.com/library/ee259687.aspx
  [.NET Framework 4]: https://www.microsoft.com/download/details.aspx?id=17851
  [.NET Framework 4.5]: https://www.microsoft.com/download/details.aspx?id=30653
  [.NET framework 4.5.2]: https://www.microsoft.com/download/details.aspx?id=42643
  [Schaal van uw systeem samenstellen]: https://msdn.microsoft.com/library/dd793166.aspx
  [Implementeren en configureren van een server maken]: https://msdn.microsoft.com/library/ms181712.aspx
  [Azure PowerShell-cmdlets]: powershell-install-configure.md
  [the .publishsettings file]: https://manage.windowsazure.com/download/publishprofile.aspx?wa=wsignin1.0
  [0]: ./media/cloud-services-dotnet-continuous-delivery/tfs-01bc.png
  [2]: ./media/cloud-services-dotnet-continuous-delivery/tfs-02.png
  [3]: ./media/cloud-services-dotnet-continuous-delivery/common-task-tfs-03.png
  [4]: ./media/cloud-services-dotnet-continuous-delivery/common-task-tfs-04.png
  [5]: ./media/cloud-services-dotnet-continuous-delivery/common-task-tfs-05.png
  [6]: ./media/cloud-services-dotnet-continuous-delivery/common-task-tfs-06.png
