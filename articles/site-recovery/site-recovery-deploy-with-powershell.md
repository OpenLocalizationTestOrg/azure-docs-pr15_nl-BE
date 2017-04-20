<properties
    pageTitle="Hyper-V virtuele machines in de VMM wolken met Azure Site herstellen en PowerShell repliceren | Microsoft Azure"
    description="Informatie over het automatiseren van de replicatie van Hyper-V virtuele machines in de VMM wolken Site herstellen en PowerShell gebruiken."
    services="site-recovery"
    documentationCenter=""
    authors="bsiva"
    manager="abhiag"
    editor="tysonn"/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="bsiva"/>

# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-azure-using-powershell---classic"></a>Hyper-V virtuele machines in de wolken VMM repliceren naar Azure Powershell - klassiek gebruiken

> [AZURE.SELECTOR]
- [Azure Portal](site-recovery-vmm-to-azure.md)
- [PowerShell - Resource Manager](site-recovery-vmm-to-azure-powershell-resource-manager.md)
- [Klassieke Portal](site-recovery-vmm-to-azure-classic.md)
- [PowerShell - klassiek](site-recovery-deploy-with-powershell.md)


## <a name="overview"></a>Overzicht

Azure-Site-Recovery draagt bij aan uw continuïteit en disaster recovery (BCDR) bedrijfsstrategie door regie van replicatie, failover en herstel van virtuele machines in een aantal implementatiescenario's. Zie voor een volledige lijst van implementatiescenario's [Azure Site-overzicht](site-recovery-overview.md).

In dit artikel wordt beschreven hoe u PowerShell gebruiken voor het automatiseren van algemene taken die u uitvoeren moet bij het instellen van Azure Site herstel Hyper-V virtuele machines in System Center VMM wolken repliceren naar Azure opslag.

Het artikel bevat de vereisten voor het scenario en ziet u hoe een kluis Recovery Site instellen, de Azure Site herstel voorziening hebt geïnstalleerd op de bronserver VMM, de server registreren in de kluis, een Azure opslag toevoegen, de agent Azure Recovery Services op de host-servers Hyper-V installeren, instellingen voor gegevensbeveiliging voor VMM wolken die wordt toegepast op alle beveiligde virtuele machines configureren , en bescherming van deze virtuele machines in te schakelen. Voltooien van door het testen van de failover-functie om te controleren of dat alles werkt zoals verwacht.

Als u problemen met het instellen van dit scenario, vragen op het [Forum van Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


> [AZURE.NOTE] Azure heeft twee verschillende modellen voor het maken en werken met resources: [Resource Manager en klassiek](../resource-manager-deployment-model.md). In dit artikel beschreven hoe u met het model Klassiek implementatie.



## <a name="before-you-start"></a>Voordat u begint

Zorg ervoor dat u hebt deze vereisten:

### <a name="azure-prerequisites"></a>Azure-vereisten

- U moet een account van [Microsoft Azure](https://azure.microsoft.com/) . U kunt beginnen met een [gratis proefperiode](https://azure.microsoft.com/pricing/free-trial/).
- U moet een account Azure opslag gerepliceerde gegevens wilt opslaan. De account moet geo-replicatie is ingeschakeld. Het moet in hetzelfde gebied, als de kluis Azure Site herstel en worden gekoppeld aan het abonnement hetzelfde. [Meer informatie over Azure opslag](../storage/storage-introduction.md).
- U moet om ervoor te zorgen dat virtuele machines die u wilt beveiligen [Azure VM vereisten](site-recovery-best-practices.md#virtual-machines)voldoen.

### <a name="vmm-prerequisites"></a>VMM-vereisten
- U moet de VMM server met System Center 2012 R2.
- U moet ten minste één cloud op de VMM-server die u wilt beveiligen. De cloud moet bevatten:
    - Een of meer VMM hostgroepen.
    - Hyper-V host-servers of clusters in de hostgroep van elke.
    - Een of meer virtuele machines op de bronserver Hyper-V.

### <a name="hyper-v-prerequisites"></a>Hyper-V-vereisten

- De hostservers Hyper-V wordt uitgevoerd ten minste **Windows Server 2012** met Hyper-V-rol of **Microsoft Hyper-V Server 2012** en de laatste updates hebt geïnstalleerd.
- Als u werkt met Hyper-V in een notitie van het cluster dat cluster broker wordt niet automatisch gemaakt als u een statisch IP-adres gebaseerde cluster. U moet handmatig de broker cluster configureren. Om dit te doen, in Serverbeheer > Failoverclusterbeheer, verbinding maken met het cluster en klik op **Configureren rol** **Hyper-V Replica Broker** selecteren in het scherm **Selecteer rol** van de wizard maximale beschikbaarheid.
- Een Hyper-V host-server of cluster die u wilt beheren, beveiliging moet worden opgenomen in een wolk van VMM.

### <a name="network-mapping-prerequisites"></a>Voorwaarden voor toewijzing van netwerk
Als u virtuele machines in Azure netwerk toewijzing toewijzingen tussen netwerken op de bronserver VMM VM beschermen en Azure netwerken, zodat het volgende doel:

- Alle machines die op hetzelfde netwerk failover kunnen met elkaar verbinden, ongeacht welke herstelplan zijn.
- Als een netwerkgateway setup op de doelcomputer Azure netwerk, worden virtuele machines kunt verbinden met andere op ruimten virtuele machines.
- Als u geen netwerk-toewijzing alleen virtuele machines configureren die in het herstelplan dezelfde failover niet verbinding kunnen maken met elkaar na een failover naar Azure.

Als u de toewijzing van het netwerk te implementeren hebt u het volgende nodig:

- De virtuele machines die u wilt beveiligen op de bronserver VMM moet worden verbonden met een netwerk voor VM. Dit netwerk moet worden gekoppeld aan een logisch netwerk dat is gekoppeld aan de cloud.
- Een Azure netwerk die gerepliceerde virtuele machines verbinding na een failover maken kan. U zult dit netwerk selecteren op het moment van failover. Het netwerk moet zich in hetzelfde gebied, als uw abonnement Azure Site herstellen.
- [Meer informatie](site-recovery-network-mapping.md) over het netwerk toewijzen:

###<a name="powershell-prerequisites"></a>Vereisten voor PowerShell
Zorg ervoor dat u hebt Azure PowerShell is klaar om te gaan. Als u al met PowerShell, moet u een upgrade uitvoeren naar versie 0.8.10 of hoger. Zie voor meer informatie over het instellen van PowerShell [installeren en configureren van Azure PowerShell](../powershell-install-configure.md). Als u hebt ingesteld en geconfigureerd PowerShell, kunt u alle de cmdlets beschikbaar voor de service bekijken [hier](https://msdn.microsoft.com/library/dn850420.aspx).

Zie meer informatie over tips die u kunnen helpen dat u de cmdlets gebruikt, zoals hoe parameterwaarden, ingangen en uitgangen worden meestal afgehandeld in Azure PowerShell, [Aan de slag met Azure Cmdlets](https://msdn.microsoft.com/library/azure/jj554332.aspx).

## <a name="step-1-set-the-subscription"></a>Stap 1: Stel het abonnement

In PowerShell deze cmdlets worden uitgevoerd:

```
$UserName = "<user@live.com>"
$Password = "<password>"
$AzureSubscriptionName = "prod_sub1"

$SecurePassword = ConvertTo-SecureString -AsPlainText $Password -Force
$Cred = New-Object System.Management.Automation.PSCredential -ArgumentList $UserName, $securePassword
Add-AzureAccount -Credential $Cred;
$AzureSubscription = Select-AzureSubscription -SubscriptionName $AzureSubscriptionName

```

De elementen in de "combinatie" vervangen door de specifieke informatie.

## <a name="step-2-create-a-site-recovery-vault"></a>Stap 2: Maak een kluis Site herstellen

De elementen in de "combinatie" vervangen door de specifieke informatie in PowerShell, en deze opdrachten uitvoeren:

```

$VaultName = "<testvault123>"
$VaultGeo  = "<Southeast Asia>"
$OutputPathForSettingsFile = "<c:\>"

```


```
New-AzureSiteRecoveryVault -Location $VaultGeo -Name $VaultName;
$vault = Get-AzureSiteRecoveryVault -Name $VaultName;

```

## <a name="step-3-generate-a-vault-registration-key"></a>Stap 3: Een kluis registratie sleutel genereren

Een registratie sleutel genereren in de kluis. Nadat u de Provider Azure Site Recovery downloaden en op de server VMM installeren, gebruikt u deze sleutel de VMM-server registreren in de kluis.

1.  De instelling kluis bestand ophalen en instellen van de context:

    ```

    $VaultName = "<testvault123>"
    $VaultGeo  = "<Southeast Asia>"
    $OutputPathForSettingsFile = "<c:\>"

    $VaultSetingsFile = Get-AzureSiteRecoveryVaultSettingsFile -Location $VaultGeo -Name $VaultName -Path $OutputPathForSettingsFile;

    ```

2.  De kluis-context instellen door de volgende opdrachten:

    ```

    $VaultSettingFilePath = $vaultSetingsFile.FilePath
    $VaultContext = Import-AzureSiteRecoveryVaultSettingsFile -Path $VaultSettingFilePath -ErrorAction Stop

    ```

## <a name="step-4-install-the-azure-site-recovery-provider"></a>Stap 4: Installeer de Provider Azure Site herstellen

1.  Maak een map door de volgende opdracht uit te voeren op de machine VMM:

    ```

    pushd C:\ASR\

    ```

2.  Pak de bestanden door de volgende opdracht uit te voeren met behulp van de provider gedownload

    ```

    AzureSiteRecoveryProvider.exe /x:. /q

    ```

3.  Installeer de provider met behulp van de volgende opdrachten:

    ```

    .\SetupDr.exe /i

    ```

    ```

    $installationRegPath = "hklm:\software\Microsoft\Microsoft System Center Virtual Machine Manager Server\DRAdapter"
    do
    {
        $isNotInstalled = $true;
        if(Test-Path $installationRegPath)
        {
            $isNotInstalled = $false;
        }
    }While($isNotInstalled)

    ```

    Wacht totdat de installatie te voltooien.

4.  De server registreren in de kluis met de volgende opdracht:

    ```

    $BinPath = $env:SystemDrive+"\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin"
    pushd $BinPath
    $encryptionFilePath = "C:\temp\"
    .\DRConfigurator.exe /r /Credentials $VaultSettingFilePath /vmmfriendlyname $env:COMPUTERNAME /dataencryptionenabled $encryptionFilePath /startvmmservice

    ```

## <a name="step-5-create-an-azure-storage-account"></a>Stap 5: Maak een account Azure opslag

Als u geen account Azure opslag, maakt u een account geo-replicatie is ingeschakeld door de volgende opdracht uit te voeren:

```

$StorageAccountName = "teststorageacc1"
$StorageAccountGeo  = "Southeast Asia"

New-AzureStorageAccount -StorageAccountName $StorageAccountName -Label $StorageAccountName -Location $StorageAccountGeo;

```

Houd er rekening mee dat de account van de opslag moet in hetzelfde gebied, als de Azure-Site-Recovery-service, en gekoppeld aan het abonnement hetzelfde worden.


## <a name="step-6-install-the-azure-recovery-services-agent"></a>Stap 6: De herstelagent voor Azure Services installeren

Vanuit de Azure portal de agent Azure Recovery Services te installeren op elke Hyper-V host-server zich bevindt in de VMM wolken die u wilt beveiligen.

De volgende opdracht uitvoeren op alle hosts van VMM:

```

marsagentinstaller.exe /q /nu

```


## <a name="step-7-configure-cloud-protection-settings"></a>Stap 7: Wolk configureren beveiligingsinstellingen

1.  Cloud beveiliging profiel Azure maken door de volgende opdracht uit te voeren:

    ```

    $ReplicationFrequencyInSeconds = "300";
    $ProfileResult = New-AzureSiteRecoveryProtectionProfileObject -ReplicationProvider HyperVReplica -RecoveryAzureSubscription $AzureSubscriptionName -RecoveryAzureStorageAccount $StorageAccountName -ReplicationFrequencyInSeconds  $ReplicationFrequencyInSeconds;

    ```

2.  Een container bescherming krijgen door de volgende opdrachten:

    ```

    $PrimaryCloud = "testcloud"
    $protectionContainer = Get-AzureSiteRecoveryProtectionContainer -Name $PrimaryCloud;

    ```

3.  De koppeling van de container bescherming starten met de cloud:

    ```

    $associationJob = Start-AzureSiteRecoveryProtectionProfileAssociationJob -ProtectionProfile $profileResult -PrimaryProtectionContainer $protectionContainer;        

    ```

4.  Nadat de afdruktaak is voltooid, voert u de volgende opdracht:


        $job = Get-AzureSiteRecoveryJob -Id $associationJob.JobId;
        if($job -eq $null -or $job.StateDescription -ne "Completed")
        {
        $isJobLeftForProcessing = $true;
        }


5.  Nadat de verwerking is voltooid, kunt u de volgende opdracht uitvoeren:


        Do
        {
        $job = Get-AzureSiteRecoveryJob -Id $associationJob.JobId;
        Write-Host "Job State:{0}, StateDescription:{1}" -f Job.State, $job.StateDescription;
        if($job -eq $null -or $job.StateDescription -ne "Completed")
        {
            $isJobLeftForProcessing = $true;
        }

        if($isJobLeftForProcessing)
        {
            Start-Sleep -Seconds 60
        }
        }While($isJobLeftForProcessing)



Om te controleren op de voltooiing van de bewerking, de stappen van de [Activiteit van de Monitor](#monitor).

## <a name="step-8-configure-network-mapping"></a>Stap 8: Toewijzing van netwerk configureren
Voordat u begint met het netwerk toewijzing controleert u of dat virtuele machines op de bronserver VMM bent verbonden met een netwerk voor VM. Daarnaast een of meer Azure virtuele netwerken maken. Houd er rekening mee dat meerdere VM-netwerken kunnen worden toegewezen aan één Azure netwerk.

Houd er rekening mee dat als het netwerk meerdere subnetten heeft en een van de subnetten heeft dezelfde naam als het subnet waarop de virtuele machine van bron zich bevindt en vervolgens de virtuele machine van replica worden verbonden met dat doel subnet na een failover. Als er niet een doel-subnet met een overeenkomende naam, wordt de virtuele machine worden verbonden met het eerste subnet in het netwerk.

De eerste opdracht haalt de servers voor het huidige herstel van Azure Site vault. De servers van Microsoft Azure Site herstellen met de opdracht opgeslagen in de array-variabele $Servers.

    $Servers = Get-AzureSiteRecoveryServer


De tweede opdracht wordt de site-recovery-netwerk voor de eerste server in de matrix $Servers. De opdracht wordt de netwerken in de variabele $Networks opgeslagen.


    $Networks = Get-AzureSiteRecoveryNetwork -Server $Servers[0]

De derde opdracht haalt uw Azure-abonnementen via de cmdlet Get-AzureSubscription en die waarde vervolgens opgeslagen in de variabele $Subscriptions.

    $Subscriptions = Get-AzureSubscription



De vierde opdracht haalt Azure virtuele netwerken via de cmdlet Get-AzureVNetSite en die waarde in de variabele $AzureVmNetworks.


    $AzureVmNetworks = Get-AzureVNetSite



De uiteindelijke cmdlet maakt een koppeling tussen het primaire netwerk en het netwerk Azure virtuele machine. De cmdlet geeft het primaire netwerk als het eerste element van de $Networks. De cmdlet geeft een netwerk van virtuele machine als het eerste element van $AzureVmNetworks met behulp van de-ID. De opdracht omvat de Azure abonnement-ID.


    New-AzureSiteRecoveryNetworkMapping -PrimaryNetwork $Networks[0] -AzureSubscriptionId $Subscriptions[0].SubscriptionId -AzureVMNetworkId $AzureVmNetworks[0].Id


## <a name="step-9-enable-protection-for-virtual-machines"></a>Stap 9: Beveiliging voor virtuele machines inschakelen

Nadat de servers, wolken en netwerken goed zijn geconfigureerd, kunt u de beveiliging voor virtuele machines in de cloud. Let op het volgende:

Virtuele machines moeten voldoen aan de [vereisten van Azure virtual machine](site-recovery-best-practices.md#virtual-machines).

Om de beveiliging van het besturingssysteem en het besturingssysteem inschakelen moeten schijfeigenschappen worden ingesteld voor de virtuele machine. U kunt de eigenschap instellen wanneer u een virtuele machine in VMM met behulp van de sjabloon voor een virtuele machine maakt. U kunt deze eigenschappen voor bestaande virtuele machines ook instellen op de tabbladen **Algemeen** en **Hardwareconfiguratie** van de eigenschappen van de virtuele machine. Als u deze eigenschappen niet in de VMM instellen zult u kunnen ze in de portal Azure Site herstel configureren.



1.  Als beveiliging wilt inschakelen, kunt u de volgende opdracht om de container bescherming uitvoeren:

        $ProtectionContainer = Get-AzureSiteRecoveryProtectionContainer -Name $CloudName



2. De dienst bescherming (VM) opvragen door de volgende opdracht uit te voeren:


        $protectionEntity = Get-AzureSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $protectionContainer



3. De DR voor VM inschakelen door de volgende opdracht uit te voeren:



        $jobResult = Set-AzureSiteRecoveryProtectionEntity -ProtectionEntity $protectionEntity  -Protection Enable -Force



## <a name="test-your-deployment"></a>De implementatie testen

Voor het testen van de implementatie kunt u een failover testen voor een enkele virtuele machine, uitvoeren of een herstelplan dat bestaat uit meerdere virtuele machines maken en uitvoeren van failover test voor het plan. Failover van de test simuleert de failover- en herstel mechanisme in een geïsoleerd netwerk. Houd rekening met:

- Als u wilt verbinding maken met de virtuele machine in Azure met extern bureaublad na de failover, inschakelen verbinding met extern bureaublad op de virtuele machine voordat u de test failover uitvoeren.
- Na een failover gebruikt u een openbaar IP-adres verbinding kunnen maken met de virtuele machine in Azure met extern bureaublad. Als u dit doen wilt, zorg ervoor dat u geen domeinbeleid om te voorkomen dat u geen verbinding maken met een virtuele machine met behulp van een openbaar adres.

Om te controleren op de voltooiing van de bewerking, de stappen van de [Activiteit van de Monitor](#monitor).

### <a name="create-a-recovery-plan"></a>Maak een herstelplan

1. Een XML-bestand als een sjabloon voor uw herstelplan met behulp van de onderstaande gegevens te maken en vervolgens opslaan als 'C:\RPTemplatePath.xml'.
2. Wijzig de RecoveryPlan knooppunt-Id, naam, PrimaryServerId en SecondaryServerId.
3. Het knooppunt ProtectionEntity PrimaryProtectionEntityId (vmid van VMM) wijzigen.
4. U kunt meer VMs toevoegen door meer ProtectionEntity knooppunten toe te voegen.



        <#
        <?xml version="1.0" encoding="utf-16"?>
        <RecoveryPlan Id="d0323b26-5be2-471b-addc-0a8742796610" Name="rp-test"  PrimaryServerId="9350a530-d5af-435b-9f2b-b941b5d9fcd5"  SecondaryServerId="21a9403c-6ec1-44f2-b744-b4e50b792387" Description=""     Version="V2014_07">
          <Actions />
          <ActionGroups>
            <ShutdownAllActionGroup Id="ShutdownAllActionGroup">
              <PreActionSequence />
              <PostActionSequence />
            </ShutdownAllActionGroup>
            <FailoverAllActionGroup Id="FailoverAllActionGroup">
              <PreActionSequence />
              <PostActionSequence />
            </FailoverAllActionGroup>
            <BootActionGroup Id="DefaultActionGroup">
              <PreActionSequence />
              <PostActionSequence />
              <ProtectionEntity PrimaryProtectionEntityId="d4c8ce92-a613-4c63-9b03- cf163cc36ef8" />
            </BootActionGroup>
          </ActionGroups>
          <ActionGroupSequence>
            <ActionGroup Id="ShutdownAllActionGroup" ActionId="ShutdownAllActionGroup"  Before="FailoverAllActionGroup" />
            <ActionGroup Id="FailoverAllActionGroup" ActionId="FailoverAllActionGroup"  After="ShutdownAllActionGroup" Before="DefaultActionGroup" />
            <ActionGroup Id="DefaultActionGroup" ActionId="DefaultActionGroup" After="FailoverAllActionGroup"/>
          </ActionGroupSequence>
        </RecoveryPlan>
        #>



4. Vul de gegevens in de sjabloon:


        $TemplatePath = "C:\RPTemplatePath.xml";



5. De RecoveryPlan maken:

        $RPCreationJob = New-AzureSiteRecoveryRecoveryPlan -File $TemplatePath -WaitForCompletion;



### <a name="run-a-test-failover"></a>Een failover test uitvoeren

1.  Het RecoveryPlan-object ophalen door de volgende opdracht uit te voeren:

        $RPObject = Get-AzureSiteRecoveryRecoveryPlan -Name $RPName;


2.  Start de failover testen door de volgende opdracht uit te voeren:


        $jobIDResult = Start-AzureSiteRecoveryTestFailoverJob -RecoveryPlan $RPObject -Direction PrimaryToRecovery;


## <a name=monitor></a>Activiteiten controleren

De volgende opdrachten gebruiken voor het controleren van de activiteit. Houd er rekening mee dat er moet worden gewacht bij taken voor de verwerking te voltooien.


    Do
    {
            $job = Get-AzureSiteRecoveryJob -Id $associationJob.JobId;
            Write-Host "Job State:{0}, StateDescription:{1}" -f Job.State, $job.StateDescription;
            if($job -eq $null -or $job.StateDescription -ne "Completed")
            {
                $isJobLeftForProcessing = $true;
            }

        if($isJobLeftForProcessing)
            {
                Start-Sleep -Seconds 60
            }
    }While($isJobLeftForProcessing)



## <a name="next-steps"></a>Volgende stappen

[Lees meer](https://msdn.microsoft.com/library/dn850420.aspx) over Azure Site herstel PowerShell-cmdlets. </a>.
