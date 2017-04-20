<properties
    pageTitle="VMware virtuele machines naar Azure repliceren door Site-herstel met Azure automatisering DSC | Microsoft Azure"
    description="Beschrijving van Azure automatisering DSC automatisch implementeren de Azure Site herstel mobiliteit service en Azure agent voor virtuele/fysieke machines met Azure."
    services="site-recovery"
    documentationCenter=""
    authors="krnese"
    manager="lorenr"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/26/2016"
    ms.author="krnese"/>

# <a name="replicate-vmware-virtual-machines-to-azure-by-using-site-recovery-with-azure-automation-dsc"></a>VMware virtuele machines naar Azure repliceren door Site-herstel met Azure automatisering DSC

In het Operations Management Suite bieden wij u met een uitgebreide back-up en disaster recovery-oplossing die u als onderdeel van uw plan voor bedrijfscontinuïteit gebruiken kunt.

We begonnen met deze reis met Hyper-V met behulp van Hyper-V-Replica. Maar we hebben uitgebreid met ondersteuning van een heterogene setup omdat klanten meerdere hypervisors en platforms in de wolken.

Als u met VMware werkbelasting en/of fysieke servers vandaag, een management-server alle onderdelen Azure Site herstellen wordt uitgevoerd in uw omgeving om de communicatie- en replicatie met Azure, wanneer Azure uw bestemming is.

## <a name="deploy-the-site-recovery-mobility-service-by-using-automation-dsc"></a>De Site herstel mobiliteit service implementeren met behulp van automatisering DSC
Laten we beginnen door te doen een snelle verdeling van de werking van deze server beheren.

De server wordt uitgevoerd voor verschillende serverfuncties. Een van deze rollen is *configuratie*, die communicatie coördineert en beheert gegevens repliceren en herstel processen.

Bovendien fungeert de rol *proces* als gateway voor replicatie. Deze rol replicatiegegevens worden ontvangen van beveiligde bron machines optimaliseert met caching, compressie en codering en verzendt dit naar een rekening Azure opslag. Een van de functies voor de Procesrol is ook push-installatie van de service mobiliteit voor beveiligde computers en het uitvoeren van automatische detectie van VMs VMware.

Als er een failback van Azure, verwerkt de *master* functie de replicatiegegevens als onderdeel van deze bewerking.

Wij vertrouwen op de *mobiliteit service*voor de beveiligde computers. Dit onderdeel is geïmplementeerd op elke machine (VM VMware of fysieke server) die u wilt repliceren naar Azure. Het schrijven van gegevens op de computer worden vastgelegd en ze doorstuurt naar de server (Procesrol).

Als bedrijfscontinuïteit is besteed, is het belangrijk om te weten uw werkbelasting, de infrastructuur en de betrokken onderdelen. U kunt vervolgens voldoen aan de vereisten voor uw herstel tijd doelstelling (RTO) en herstel punt doelstelling (vrijgegeven Productieorder). In deze context is de mobiliteit service sleutel om ervoor te zorgen dat uw werklast worden beschermd, zoals u zou verwachten.

Hoe kunt u, in een geoptimaliseerde manier ervoor zorgen dat u beschikt over een betrouwbare beveiligd instellen met behulp van enkele onderdelen Operations Management Suite?

Dit artikel bevat een voorbeeld van hoe u Azure automatisering gewenst staat configuratie (DSC), samen met het herstellen van een Site gebruiken kunt om ervoor te zorgen dat:

- De mobiliteit service en Azure VM agent worden gedistribueerd naar de Windows-computers die u wilt beveiligen.
- De mobiliteit service en Azure VM agent worden altijd uitgevoerd wanneer Azure het doel van de replication is.

## <a name="prerequisites"></a>Vereisten

- Een opslagplaats voor het opslaan van de vereiste instellingen
- Een opslagplaats voor het opslaan van de vereiste wachtwoordzin registreren met de server

 > [AZURE.NOTE] Een unieke wachtwoordzin is gegenereerd voor elke server management. Als u meerdere servers voor het beheer te implementeren, moet u ervoor te zorgen dat de juiste wachtwoordzin is opgeslagen in het bestand passphrase.txt.

- Windows Management Framework (WMF) 5.0 is geïnstalleerd op de computers die u wilt inschakelen voor bescherming (een vereiste voor automatisering DSC)

 > [AZURE.NOTE] Als DSC voor Windows-systemen waarop WMF 4.0 is geïnstalleerd gewenste, raadpleegt u de sectie [DSC gebruiken in een niet-verbonden omgeving](#Use DSC in disconnected environments).

De mobiliteit service kan worden geïnstalleerd via de opdrachtregel en verschillende argumenten. Daarom moet u de binaire bestanden (na het uitpakken ze uit de setup) hebt en deze opslaan op een plaats waar u ze ophalen kunt met behulp van een DSC-configuratie.

## <a name="step-1-extract-binaries"></a>Stap 1: Extract binaire bestanden

1. De bestanden die u nodig hebt voor deze instelling wilt uitpakken, gaat u naar de volgende map op uw server voor documentbeheer:

    **\Microsoft azure Site Recovery\home\svsystems\pushinstallsvc\repository**

    In deze map ziet u een MSI-bestand met de naam:

    **Microsoft ASR_UA_version_Windows_GA_date_Release.exe**

    Gebruik de volgende opdracht de installer uitpakken:

    **.\Microsoft-ASR_UA_9.1.0.0_Windows_GA_02May2016_release.exe/q /x:C:\Users\Administrator\Desktop\Mobility_Service\Extract**

2. Selecteer alle bestanden en stuur ze naar een gecomprimeerde (ingepakte) map.

U hebt nu de binaire bestanden die u nodig hebt om de setup van de dienst mobiliteit via automatisering DSC.

### <a name="passphrase"></a>Wachtwoordzin

Vervolgens moet u bepalen waar u deze gecomprimeerde map plaatsen. Kunt u een account Azure opslag, zoals later, voor het opslaan van de wachtwoordzin die u nodig hebt voor de installatie. De agent wordt vervolgens geregistreerd met de server als onderdeel van het proces.

De wachtwoordzin die u kreeg toen u de server geïmplementeerd kan worden opgeslagen in een tekstbestand opgeslagen als passphrase.txt.

Plaats de gecomprimeerde map en de wachtwoordzin in een speciale container in de opslag van Azure-account.

![De locatie](./media/site-recovery-automate-mobilitysevice-install/folder-and-passphrase-location.png)

Als u liever houden deze bestanden op een share op uw netwerk, kunt u dat doen. U hoeft alleen maar om ervoor te zorgen dat de DSC-bron die u later wilt gebruiken toegang hebben en kunt de setup en wachtwoordzin.

## <a name="step-2-create-the-dsc-configuration"></a>Stap 2: Maak de DSC-configuratie

WMF 5.0 is afhankelijk van de instellingen. WMF 5.0 moet aanwezig zijn voor de machine de configuratie via automatisering DSC niet toepassen.

De omgeving wordt gebruikt voor het volgende voorbeeld DSC-configuratie:

```powershell
configuration ASRMobilityService {

    $RemoteFile = 'https://knrecstor01.blob.core.windows.net/asr/ASR.zip'
    $RemotePassphrase = 'https://knrecstor01.blob.core.windows.net/asr/passphrase.txt'
    $RemoteAzureAgent = 'http://go.microsoft.com/fwlink/p/?LinkId=394789'
    $LocalAzureAgent = 'C:\Temp\AzureVmAgent.msi'
    $TempDestination = 'C:\Temp\asr.zip'
    $LocalPassphrase = 'C:\Temp\Mobility_service\passphrase.txt'
    $Role = 'Agent'
    $Install = 'C:\Program Files (x86)\Microsoft Azure Site Recovery'
    $CSEndpoint = '10.0.0.115'
    $Arguments = '/Role "{0}" /InstallLocation "{1}" /CSEndpoint "{2}" /PassphraseFilePath "{3}"' -f $Role,$Install,$CSEndpoint,$LocalPassphrase

    Import-DscResource -ModuleName xPSDesiredStateConfiguration

    node localhost {

        File Directory {
            DestinationPath = 'C:\Temp\ASRSetup\'
            Type = 'Directory'            
        }

        xRemoteFile Setup {
            URI = $RemoteFile
            DestinationPath = $TempDestination
            DependsOn = '[File]Directory'
        }

        xRemoteFile Passphrase {
            URI = $RemotePassphrase
            DestinationPath = $LocalPassphrase
            DependsOn = '[File]Directory'
        }

        xRemoteFile AzureAgent {
            URI = $RemoteAzureAgent
            DestinationPath = $LocalAzureAgent
            DependsOn = '[File]Directory'
        }

        Archive ASRzip {
            Path = $TempDestination
            Destination = 'C:\Temp\ASRSetup'
            DependsOn = '[xRemotefile]Setup'
        }

        Package Install {
            Path = 'C:\temp\ASRSetup\ASR\UNIFIEDAGENT.EXE'
            Ensure = 'Present'
            Name = 'Microsoft Azure Site Recovery mobility Service/Master Target Server'
            ProductId = '275197FC-14FD-4560-A5EB-38217F80CBD1'
            Arguments = $Arguments
            DependsOn = '[Archive]ASRzip'
        }

        Package AzureAgent {
            Path = 'C:\Temp\AzureVmAgent.msi'
            Ensure = 'Present'
            Name = 'Windows Azure VM Agent - 2.7.1198.735'
            ProductId = '5CF4D04A-F16C-4892-9196-6025EA61F964'
            Arguments = '/q /l "c:\temp\agentlog.txt'
            DependsOn = '[Package]Install'
        }

        Service ASRvx {
            Name = 'svagents'
            Ensure = 'Present'
            State = 'Running'
            DependsOn = '[Package]Install'
        }

        Service ASR {
            Name = 'InMage Scout Application Service'
            Ensure = 'Present'
            State = 'Running'
            DependsOn = '[Package]Install'
        }

        Service AzureAgentService {
            Name = 'WindowsAzureGuestAgent'
            Ensure = 'Present'
            State = 'Running'
            DependsOn = '[Package]AzureAgent'
        }

        Service AzureTelemetry {
            Name = 'WindowsAzureTelemetryService'
            Ensure = 'Present'
            State = 'Running'
            DependsOn = '[Package]AzureAgent'
        }
    }
}
```
De configuratie wordt het volgende doen:

- De variabelen de configuratie zal vertellen waar u de binaire bestanden voor de service van de mobiliteit en de agent Azure VM waar u de wachtwoordzin en voor het opslaan van de uitvoer.
- De configuratie wordt de resource xPSDesiredStateConfiguration DSC importeren zodat u kunt `xRemoteFile` de bestanden te downloaden uit de bibliotheek.
- De configuratie maakt een map waar u de binaire bestanden worden opgeslagen.
- De bron van het archief wordt Pak de bestanden vanuit de gecomprimeerde map.
- Bron van de installatie wordt geïnstalleerd de mobiliteit service van de UNIFIEDAGENT. EXE installer met de specifieke argumenten. (De variabelen die het samenstellen van de argumenten moeten worden aangepast aan uw omgeving.)
- Het pakket AzureAgent-bron installeert de Azure VM-agent wordt aanbevolen voor elke die wordt uitgevoerd in Azure VM. De agent Azure VM stelt het toevoegen van extensies voor VM na een failover.
- Service of meer resources wordt ervoor te zorgen dat verwante mobiliteit en de Azure services altijd worden uitgevoerd.

Sla de configuratie op als **ASRMobilityService**.

>[AZURE.NOTE] Vergeet niet te vervangen door de CSIP in uw configuratie zodat de werkelijke server, zodat de agent goed zijn verbonden en wordt de juiste wachtwoordzin gebruiken.

## <a name="step-3-upload-to-automation-dsc"></a>Stap 3: Uploaden naar DSC automatisering

Omdat de DSC-configuratie die u importeert een vereiste resource DSC-module (xPSDesiredStateConfiguration), moet u die module in automatisering importeren voordat u de configuratie van DSC uploaden.

Aanmelden bij uw account automatisering, Ga naar **activa** > **Modules**, en klik op **Galerie bladeren**.

Hier kunt u zoeken naar de module en importeren in uw account.

![Import-module](./media/site-recovery-automate-mobilitysevice-install/search-and-import-module.png)

Wanneer u klaar bent, gaat u naar de computer waar u de bronnenbeheerder Azure modules geïnstalleerd hebt en gaat u verder met de nieuwe DSC-configuratie importeren.

### <a name="import-cmdlets"></a>Cmdlets importeren

In PowerShell, log in om uw abonnement op Azure. Wijzig de cmdlets te nemen van uw omgeving en uw accountgegevens automatisering in een variabele vastgelegd:

```powershell
$AAAccount = Get-AzureRmAutomationAccount -ResourceGroupName 'KNOMS' -Name 'KNOMSAA'
```

De configuratie naar DSC automatisering met behulp van de volgende cmdlet uploaden:

```powershell
$ImportArgs = @{
    SourcePath = 'C:\ASR\ASRMobilityService.ps1'
    Published = $true
    Description = 'DSC Config for Mobility Service'
}
$AAAccount | Import-AzureRmAutomationDscConfiguration @ImportArgs
```

### <a name="compile-the-configuration-in-automation-dsc"></a>De configuratie in automatisering DSC compileren

Vervolgens moet u de configuratie in automatisering DSC, compileren zodat u beginnen kunt met het registreren van knooppunten aan het. U bereikt die door de volgende cmdlet wordt uitgevoerd:

```powershell
$AAAccount | Start-AzureRmAutomationDscCompilationJob -ConfigurationName ASRMobilityService
```

Dit kan enkele minuten duren, omdat u in feite de configuratie de gehoste DSC pull-service introduceert.

Na het compileren van de configuratie kunt u de functie-informatie ophalen met PowerShell (Get-AzureRmAutomationDscCompilationJob) of via de [portal Azure](https://portal.azure.com/).

![Taak ophalen](./media/site-recovery-automate-mobilitysevice-install/retrieve-job.png)

Je hebt nu gepubliceerd en de configuratie van de DSC geüpload naar DSC automatisering.

## <a name="step-4-onboard-machines-to-automation-dsc"></a>Stap 4: Onboard computers DSC automatisering
>[AZURE.NOTE] Een van de vereisten voor het voltooien van dit scenario is dat uw Windows-computers worden bijgewerkt met de nieuwste versie van WMF. U kunt downloaden en installeren van de juiste versie voor uw platform van het [Download Center](https://www.microsoft.com/download/details.aspx?id=50395).

Nu maakt u een metaconfig voor DSC die u op de knooppunten vereffenen wilt. Om te slagen met deze, moet u de eindpunt-URL en de primaire sleutel voor de geselecteerde rekening automatisering in Azure ophalen. U kunt deze waarden onder de **toetsen** op het blad **alle instellingen** voor de rekening van de automatisering.

![Sleutelwaarden](./media/site-recovery-automate-mobilitysevice-install/key-values.png)

In dit voorbeeld wordt er een Windows Server 2012 R2 fysieke server die u beveiligen wilt met behulp van sites worden hersteld.

### <a name="check-for-any-pending-file-rename-operations-in-the-registry"></a>Alle wachtende hernoemen van bestanden in het register controleren

Voordat u de server koppelen aan het eindpunt DSC automatisering, wordt u aangeraden te controleren of alle wachtende hernoemen van bestanden in het register. Ze kunnen verbieden de setup voltooien als gevolg van in behandeling opnieuw.

De volgende cmdlet om te controleren of er in behandeling opnieuw opstarten is niet op de server worden uitgevoerd:

```powershell
Get-ItemProperty 'HKLM:\SYSTEM\CurrentControlSet\Control\Session Manager\' | Select-Object -Property PendingFileRenameOperations
```
Als dit leeg weergegeven, bent u OK om verder te gaan. Als dit niet het geval is, moet u dit adres door de server opnieuw opstarten tijdens een onderhoudsvenster.

De configuratie van toepassing op de server, start de PowerShell Integrated Scripting Environment (ISE) en voer het volgende script. Dit is in feite een DSC lokale configuratie waarmee de motor Local Configuration Manager te registreren bij de automatisering DSC-service en de specifieke configuratie (ASRMobilityService.localhost) op te halen.

```powershell
[DSCLocalConfigurationManager()]
configuration metaconfig {
    param (
        $URL,
        $Key
    )
    node localhost {
        Settings {
            RefreshFrequencyMins = '30'
            RebootNodeIfNeeded = $true
            RefreshMode = 'PULL'
            ActionAfterReboot = 'ContinueConfiguration'
            ConfigurationMode = 'ApplyAndMonitor'
            AllowModuleOverwrite = $true
        }

        ResourceRepositoryWeb AzureAutomationDSC {
            ServerURL = $URL
            RegistrationKey = $Key
        }

        ConfigurationRepositoryWeb AzureAutomationDSC {
            ServerURL = $URL
            RegistrationKey = $Key
            ConfigurationNames = 'ASRMobilityService.localhost'
        }

        ReportServerWeb AzureAutomationDSC {
            ServerURL = $URL
            RegistrationKey = $Key
        }
    }
}
metaconfig -URL 'https://we-agentservice-prod-1.azure-automation.net/accounts/<YOURAAAccountID>' -Key '<YOURAAAccountKey>'

Set-DscLocalConfigurationManager .\metaconfig -Force -Verbose
```

Deze configuratie wordt de engine Local Configuration Manager registreren zichzelf bij DSC automatisering. Het zal ook bepalen hoe de motor moet werken, wat het moet doen als er een configuratie drift (ApplyAndAutoCorrect) en hoe het verder met de configuratie als nodig is.

Nadat u dit script uitvoert, moet het knooppunt beginnen bij DSC automatisering registreren.

![Knooppunt registratie in behandeling](./media/site-recovery-automate-mobilitysevice-install/register-node.png)

Als u terug naar de portal Azure, kunt u zien dat de nieuw ingeschreven knooppunt nu is gebleken in de portal.

![Geregistreerde knooppunt in de portal](./media/site-recovery-automate-mobilitysevice-install/registered-node.png)

U kunt de volgende PowerShell-cmdlet om te controleren of het knooppunt correct is geregistreerd uitvoeren op de server:

```powershell
Get-DscLocalConfigurationManager
```

Nadat de configuratie is opgehaald en toegepast op de server, kunt u dit controleren door de volgende cmdlet wordt uitgevoerd:

```powershell
Get-DscConfigurationStatus
```

De uitvoer ziet u dat de server heeft de configuratie is opgehaald:

![Uitvoer](./media/site-recovery-automate-mobilitysevice-install/successful-config.png)

Bovendien heeft de mobiliteit service setup een eigen logboek dat kan worden gevonden op *systeemstation*\ProgramData\ASRSetupLogs.

Dat is. U hebt nu met succes geïmplementeerd en de mobiliteit service op de computer die u beveiligen wilt met behulp van Systeemherstel Site geregistreerd. DSC ervoor dat altijd de vereiste services worden uitgevoerd.

![Succesvolle implementatie](./media/site-recovery-automate-mobilitysevice-install/successful-install.png)

Nadat de server de geslaagde implementatie detecteert, kunt u beveiliging configureren en replicatie op de computer inschakelen met behulp van sites worden hersteld.

## <a name="use-dsc-in-disconnected-environments"></a>DSC gebruiken in een niet-verbonden omgeving

Als uw computers niet zijn verbonden met het Internet, u kunt nog steeds van DSC afhankelijk zijn om te implementeren en de mobiliteit service configureren op de werklast die u wilt beveiligen.

U kunt uw eigen DSC pull-server in uw omgeving te bieden in principe dezelfde functionaliteit die u via automatisering DSC ontvangt instantiëren. Dat wil zeggen wordt de clients klikt, de configuratie naar het eindpunt DSC (nadat het geregistreerd). Er is echter een andere optie om handmatig de DSC-configuratie op uw computers, lokaal of extern.

U ziet dat in dit voorbeeld wordt een extra parameter voor de naam van de computer. De externe bestanden staan nu op een externe share die toegankelijk zijn door de machines die u wilt beveiligen. Het einde van het script enacts van de configuratie en start vervolgens de DSC-configuratie op de doelcomputer toepassen.

### <a name="prerequisites"></a>Vereisten

Zorg ervoor dat de module xPSDesiredStateConfiguration PowerShell is geïnstalleerd. Voor Windows-computers waarop WMF 5.0 is geïnstalleerd, kunt u de module xPSDesiredStateConfiguration door de volgende cmdlet wordt uitgevoerd op de doelcomputers te installeren:

```powershell
Find-Module -Name xPSDesiredStateConfiguration | Install-Module
```

U kunt ook downloaden en opslaan van de module voor het geval u nodig hebt om het te verspreiden voor Windows-computers met WMF 4.0. Deze cmdlet uitvoeren op een machine waar PowerShellGet (WMF 5.0) aanwezig is:

```powershell
Save-Module -Name xPSDesiredStateConfiguration -Path <location>
```

Ook voor WMF 4.0, ervoor te zorgen dat de [Windows 8.1 update KB2883200](https://www.microsoft.com/download/details.aspx?id=40749) is geïnstalleerd op de computers.

De volgende configuratie kan worden geduwd voor Windows-computers met WMF 5.0 en 4.0 WMF:

```powershell
configuration ASRMobilityService {
    param (
        [Parameter(Mandatory=$true)]
        [ValidateNotNullOrEmpty()]
        [System.String] $ComputerName
    )

    $RemoteFile = '\\myfileserver\share\asr.zip'
    $RemotePassphrase = '\\myfileserver\share\passphrase.txt'
    $RemoteAzureAgent = '\\myfileserver\share\AzureVmAgent.msi'
    $LocalAzureAgent = 'C:\Temp\AzureVmAgent.msi'
    $TempDestination = 'C:\Temp\asr.zip'
    $LocalPassphrase = 'C:\Temp\Mobility_service\passphrase.txt'
    $Role = 'Agent'
    $Install = 'C:\Program Files (x86)\Microsoft Azure Site Recovery'
    $CSEndpoint = '10.0.0.115'
    $Arguments = '/Role "{0}" /InstallLocation "{1}" /CSEndpoint "{2}" /PassphraseFilePath "{3}"' -f $Role,$Install,$CSEndpoint,$LocalPassphrase

    Import-DscResource -ModuleName xPSDesiredStateConfiguration

    node $ComputerName {      
        File Directory {
            DestinationPath = 'C:\Temp\ASRSetup\'
            Type = 'Directory'            
        }

        xRemoteFile Setup {
            URI = $RemoteFile
            DestinationPath = $TempDestination
            DependsOn = '[File]Directory'
        }

        xRemoteFile Passphrase {
            URI = $RemotePassphrase
            DestinationPath = $LocalPassphrase
            DependsOn = '[File]Directory'
        }

        xRemoteFile AzureAgent {
            URI = $RemoteAzureAgent
            DestinationPath = $LocalAzureAgent
            DependsOn = '[File]Directory'
        }

        Archive ASRzip {
            Path = $TempDestination
            Destination = 'C:\Temp\ASRSetup'
            DependsOn = '[xRemotefile]Setup'
        }

        Package Install {
            Path = 'C:\temp\ASRSetup\ASR\UNIFIEDAGENT.EXE'
            Ensure = 'Present'
            Name = 'Microsoft Azure Site Recovery mobility Service/Master Target Server'
            ProductId = '275197FC-14FD-4560-A5EB-38217F80CBD1'
            Arguments = $Arguments
            DependsOn = '[Archive]ASRzip'
        }

        Package AzureAgent {
            Path = 'C:\Temp\AzureVmAgent.msi'
            Ensure = 'Present'
            Name = 'Windows Azure VM Agent - 2.7.1198.735'
            ProductId = '5CF4D04A-F16C-4892-9196-6025EA61F964'
            Arguments = '/q /l "c:\temp\agentlog.txt'
            DependsOn = '[Package]Install'
        }

        Service ASRvx {
            Name = 'svagents'
            State = 'Running'
            DependsOn = '[Package]Install'
        }

        Service ASR {
            Name = 'InMage Scout Application Service'
            State = 'Running'
            DependsOn = '[Package]Install'
        }

        Service AzureAgentService {
            Name = 'WindowsAzureGuestAgent'
            State = 'Running'
            DependsOn = '[Package]AzureAgent'
        }

        Service AzureTelemetry {
            Name = 'WindowsAzureTelemetryService'
            State = 'Running'
            DependsOn = '[Package]AzureAgent'
        }
    }
}
ASRMobilityService -ComputerName 'MyTargetComputerName'

Start-DscConfiguration .\ASRMobilityService -Wait -Force -Verbose
```

Zie [een DSC pull webserver instellen](https://msdn.microsoft.com/powershell/dsc/pullserver?f=255&MSPPError=-2147217396)voor het starten van uw eigen DSC pull-server op uw bedrijfsnetwerk om na te bootsen de mogelijkheden die u uit DSC automatisering krijgen kunt.

## <a name="optional-deploy-a-dsc-configuration-by-using-an-azure-resource-manager-template"></a>Optioneel: Een configuratie DSC implementeren met behulp van een sjabloon Azure Resource Manager

Dit artikel is gericht op hoe u uw eigen DSC configuratie maken kunt om automatisch te implementeren voor de service van de mobiliteit en de Azure VM Agent-- en ervoor te zorgen dat ze worden uitgevoerd op de computers die u wilt beveiligen. We hebben ook een sjabloon Azure Resource Manager die deze DSC-configuratie naar een nieuwe of bestaande Azure automatisering implementeert. Invoerparameters wordt gebruikt om activa met automatisering met de variabelen voor uw omgeving te maken.

Nadat u de sjabloon implementeert, kunt u gewoon verwijzen naar stap 4 in deze handleiding voor on-board uw machines.

De sjabloon wordt als volgt:

1. Een bestaande account voor automatisering gebruiken of een nieuwe maken
2. Invoerparameters voor nemen:
    - ASRRemoteFile--de locatie waar u de mobiliteit service-instellingen hebt opgeslagen
    - ASRPassphrase--de locatie waar u het bestand passphrase.txt hebt opgeslagen
    - ASRCSEndpoint--het IP-adres van de server voor documentbeheer
3. De module xPSDesiredStateConfiguration PowerShell importeren
4. De DSC-configuratie maken en de

De voorgaande stappen gebeurt in de juiste volgorde, zodat u kunt onboarding uw machines voor bescherming.

Met instructies voor de implementatie van de sjabloon bevindt zich op [GitHub](https://github.com/krnese/AzureDeploy/tree/master/OMS/MSOMS/DSC).

De sjabloon met PowerShell implementeren:

```powershell
$RGDeployArgs = @{
    Name = 'DSC3'
    ResourceGroupName = 'KNOMS'
    TemplateFile = 'https://raw.githubusercontent.com/krnese/AzureDeploy/master/OMS/MSOMS/DSC/azuredeploy.json'
    OMSAutomationAccountName = 'KNOMSAA'
    ASRRemoteFile = 'https://knrecstor01.blob.core.windows.net/asr/ASR.zip'
    ASRRemotePassphrase = 'https://knrecstor01.blob.core.windows.net/asr/passphrase.txt'
    ASRCSEndpoint = '10.0.0.115'
    DSCJobGuid = [System.Guid]::NewGuid().ToString()
}
New-AzureRmResourceGroupDeployment @RGDeployArgs -Verbose
```

## <a name="next-steps"></a>Volgende stappen

Nadat u de medewerkers van de mobiliteit implementeert, kunt u de [Replicatie inschakelen](site-recovery-vmware-to-azure.md#step-6-replicate-applications) voor de virtuele machines.
