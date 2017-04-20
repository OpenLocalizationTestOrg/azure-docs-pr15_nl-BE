<properties
    pageTitle="Hyper-V virtuele machines in de wolken VMM repliceren naar een secundaire VMM site met PowerShell (Resource Manager) | Microsoft Azure"
    description="Hierin wordt beschreven hoe Azure Site herstellen om de evoluerende replicatie, failover en herstel van Hyper-V VMs in wolken VMM aan een secundaire VMM-site met PowerShell (Resource Manager) implementeren"
    services="site-recovery"
    documentationCenter=""
    authors="sujaytalasila"
    manager="rochakm"
    editor="raynew"/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/19/2016"
    ms.author="sutalasi"/>

# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-a-secondary-vmm-site-using-powershell-resource-manager"></a>Hyper-V virtuele machines in de wolken VMM repliceren naar een secundaire VMM site met PowerShell (Resource Manager)

> [AZURE.SELECTOR]
- [Azure Portal](site-recovery-vmm-to-vmm.md)
- [Klassieke Portal](site-recovery-vmm-to-vmm-classic.md)
- [PowerShell - Resource Manager](site-recovery-vmm-to-vmm-powershell-resource-manager.md)

Welkom bij herstel Azure Site! Gebruik dit artikel als u wilt repliceren op ruimten Hyper-V virtuele machines in de wolken van System Center Virtual Machine Manager (VMM) naar een secundaire site beheerd. 

In dit artikel wordt beschreven hoe u PowerShell gebruiken voor het automatiseren van algemene taken die u nodig hebt bij het instellen van Azure Site herstel Hyper-V virtuele machines in System Center VMM wolken repliceren naar System Center VMM wolken in de secundaire site uitvoeren.

Het artikel bevat de vereisten voor het scenario en een overzicht van 

- Het instellen van een kluis Recovery Services
- De Azure Site herstel voorziening hebt geïnstalleerd op de bronserver VMM en de doelserver VMM
- De VMM (s) registreren in de kluis
- Replicatiebeleid configureren voor de VMM-wolk. De replicatie-instellingen in het beleid wordt toegepast op alle beveiligde virtuele machines 
- Inschakelen voor de virtuele machines. 
- Test de failover van VMs afzonderlijk of als onderdeel van een herstelplan om te controleren of dat alles werkt zoals verwacht.
- Uitvoeren van een geplande of een niet-geplande failover van VMs afzonderlijk of als onderdeel van een herstelplan om te controleren of dat alles werkt zoals verwacht.

Als u problemen met het instellen van dit scenario, vragen op het [Forum van Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

> [AZURE.NOTE] Azure heeft twee verschillende [implementatiemodellen](../resource-manager-deployment-model.md) voor het maken en werken met resources: Azure Resource Manager en klassiek. Azure heeft ook twee portals – de Azure klassieke portal die het implementatiemodel klassiek ondersteunt en de Azure portal met ondersteuning voor beide implementatiemodellen. Dit artikel heeft betrekking op het implementatiemodel Resource Manager.



## <a name="on-premises-prerequisites"></a>Vereisten voor gebouwen

Dit is wat u nodig hebt in de primaire en secundaire op-premises sites implementeren van dit scenario:

**Vereisten** | **Details** 
--- | ---
**VMM** | U implementeert een server VMM in de primaire site en een VMM-server in de secundaire site het beste.<br/><br/> U kunt ook [repliceren tussen wolken op één server VMM](site-recovery-single-vmm.md). Hiervoor moet u ten minste twee wolken op de VMM-server geconfigureerd.<br/><br/> VMM-servers waarop ten minste System Center 2012 SP1 met de nieuwste updates.<br/><br/> Elke server VMM moet op een of meer wolken geconfigureerd en alle wolken moeten de capaciteit van de Hyper-V-profiel instellen. <br/><br/>Wolken moeten een of meer VMM hostgroepen bevatten.<br/><br/>Meer informatie over het instellen van VMM wolken in [de cloud VMM fabric configureren](https://msdn.microsoft.com/library/azure/dn469075.aspx#BKMK_Fabric), en [procedure: persoonlijke wolken maken met System Center 2012 SP1 VMM](http://blogs.technet.com/b/keithmayer/archive/2013/04/18/walkthrough-creating-private-clouds-with-system-center-2012-sp1-virtual-machine-manager-build-your-private-cloud-in-a-month.aspx).<br/><br/> VMM-servers moeten beschikken over internet. 
**Hyper-V** | Hyper-V-servers moeten actief zijn ten minste Windows Server 2012 met de functie Hyper-V en de laatste updates hebt geïnstalleerd.<br/><br/> Hyper-V-server moet een of meer VMs bevatten.<br/><br/>  Hyper-V host-servers zich bevinden in de hostgroepen in de primaire en secundaire VMM wolken.<br/><br/> Als u Hyper-V in een cluster op Windows Server 2012 R2 uitvoert moet u [update 2961977](https://support.microsoft.com/kb/2961977) te installeren<br/><br/> Als u werkt met Hyper-V in een cluster op Windows Server 2012 opmerking dat cluster broker wordt niet automatisch gemaakt als u een statisch IP-adres gebaseerde cluster. U moet handmatig de broker cluster configureren. [Lees meer](http://social.technet.microsoft.com/wiki/contents/articles/18792.configure-replica-broker-role-cluster-to-cluster-replication.aspx).
**Provider** | Tijdens de implementatie van het herstel van de Site kunt u de Provider Azure Site herstel installeren op servers van VMM. De Provider communiceert met herstel van de Site via HTTPS-443 goedkeuringen van replicatie. Gegevensreplicatie plaatsvindt tussen de primaire en secundaire Hyper-V-servers via het LAN of via een VPN-verbinding.<br/><br/> De Provider op de VMM-server nodig heeft voor toegang tot deze URL's: *. hypervrecoverymanager.windowsazure.com, *. AccessControl.Windows.NET; *. backup.windowsazure.com, *. BLOB.Core.Windows.NET; *. store.core.windows.net.<br/><br/> Bovendien firewall communicatie van de VMM-servers op de [IP-adresbereiken Azure datacenter](https://www.microsoft.com/download/confirmation.aspx?id=41653) en kunnen het protocol HTTPS (443).

### <a name="network-mapping-prerequisites"></a>Voorwaarden voor toewijzing van netwerk
Netwerk toewijzen toewijzingen tussen VMM VM-netwerken op de primaire en secundaire VMM-servers:

- Replica VMs op secundaire Hyper-V hosts optimaal plaats na een failover.
- Replica VMs verbinding met passende VM-netwerken.
- Als u geen netwerk toewijzing replica die VMS niet na een failover wordt aangesloten op een netwerk.
- Als u een netwerk wilt maken is tijdens het herstel van de Site toewijzen implementatie hier wat u nodig hebt:

    - Zorg ervoor dat VMs op de bronserver voor Hyper-V host met een netwerk van VMM VM verbonden bent. Dit netwerk moet worden gekoppeld aan een logisch netwerk dat is gekoppeld aan de cloud.
    - Controleer of de secundaire wolk die u voor herstel gebruikt heeft een bijbehorende VM netwerk geconfigureerd. VM netwerk moet worden gekoppeld aan een logisch netwerk dat is gekoppeld aan de secundaire wolk.


Meer informatie over het configureren van VMM netwerken in de onderstaande artikelen

- [Logische netwerken configureren in VMM](http://go.microsoft.com/fwlink/p/?LinkId=386307)
- [VM-netwerken en gateways configureren in VMM](http://go.microsoft.com/fwlink/p/?LinkId=386308)

[Meer informatie](site-recovery-network-mapping.md) over de werking van de toewijzing van het netwerk.

###<a name="powershell-prerequisites"></a>Vereisten voor PowerShell
Zorg ervoor dat u hebt Azure PowerShell is klaar om te gaan. Als u al met PowerShell, moet u een upgrade uitvoeren naar versie 0.8.10 of hoger. Zie de [handleiding voor het installeren en configureren van Azure PowerShell](../powershell-install-configure.md)voor meer informatie over het instellen van PowerShell. Als u hebt ingesteld en geconfigureerd PowerShell, kunt u alle de cmdlets beschikbaar voor de service bekijken [hier](https://msdn.microsoft.com/library/dn850420.aspx). 

Meer informatie over tips die u kunnen helpen dat u de cmdlets gebruikt, zoals hoe parameterwaarden, ingangen en uitgangen worden meestal afgehandeld in Azure PowerShell, Zie de [handleiding aan de slag met Azure Cmdlets](https://msdn.microsoft.com/library/azure/jj554332.aspx).

## <a name="step-1-set-the-subscription"></a>Stap 1: Stel het abonnement 

1. Van Azure powershell, meld u aan bij uw account Azure: de volgende cmdlets gebruiken
 
        $UserName = "<user@live.com>"
        $Password = "<password>"
        $SecurePassword = ConvertTo-SecureString -AsPlainText $Password -Force
        $Cred = New-Object System.Management.Automation.PSCredential -ArgumentList $UserName, $SecurePassword
        Login-AzureRmAccount #-Credential $Cred 
    

2. Een lijst van uw abonnementen opvragen. Dit wordt ook een lijst met de subscriptionIDs voor elk van de abonnementen. De subscriptionID van het abonnement waarmee u wenst te maken van de kluis recovery services noteren    

        Get-AzureRmSubscription 

3. Het abonnement waar de kluis recovery services gemaakt is door vermelding van de abonnement-ID instellen

        Set-AzureRmContext –SubscriptionID <subscriptionId>


## <a name="step-2-create-a-recovery-services-vault"></a>Stap 2: Maak een kluis Recovery Services 

1. Een resourcegroep Azure Resource Manager maken als u geen sjabloon hebt

        New-AzureRmResourceGroup -Name #ResourceGroupName -Location #location

2. Maak een nieuwe Recovery Services kluis en het gemaakte ASR kluis object opslaan in een variabele (wordt later gebruikt). U kunt ook ophalen met de ASR-kluis boeken maken van het object met de cmdlet Get-AzureRMRecoveryServicesVault:-

        $vault = New-AzureRmRecoveryServicesVault -Name #vaultname -ResouceGroupName #ResourceGroupName -Location #location 

## <a name="step-3-set-the-recovery-services-vault-context"></a>Stap 3: De Recovery Services Vault-context instellen

1.  Uitvoeren als er een kluis al gemaakt, de volgende opdracht om de kluis.

        $vault = Get-AzureRmRecoveryServicesVault -Name #vaultname

2.  De kluis context ingesteld met de volgende opdracht.

        Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault



## <a name="step-4-install-the-azure-site-recovery-provider"></a>Stap 4: Installeer de Provider Azure Site herstellen

1.  Maak een map door de volgende opdracht uit te voeren op de machine VMM:
    
        New-Item c:\ASR -type directory
        
2.  Pak de bestanden door de volgende opdracht uit te voeren met behulp van de provider gedownload
    
        pushd C:\ASR\
        .\AzureSiteRecoveryProvider.exe /x:. /q

    
3.  Installeer de provider met behulp van de volgende opdrachten:
    
        .\SetupDr.exe /i
        $installationRegPath = "hklm:\software\Microsoft\Microsoft System Center Virtual Machine Manager Server\DRAdapter"
        do
        {
                        $isNotInstalled = $true;
                        if(Test-Path $installationRegPath)
                        {
                                        $isNotInstalled = $false;
                        }
        }While($isNotInstalled)

    Wacht totdat de installatie te voltooien.
    
4.  De server registreren in de kluis met de volgende opdracht:
    
        $BinPath = $env:SystemDrive+"\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin"
        pushd $BinPath
        $encryptionFilePath = "C:\temp\".\DRConfigurator.exe /r /Credentials $VaultSettingFilePath /vmmfriendlyname $env:COMPUTERNAME /dataencryptionenabled $encryptionFilePath /startvmmservice


## <a name="step-5-create-and-associate-a-replication-policy"></a>Stap 5: Maken en koppelen van een replicatiebeleid voor

1.  Maak een Hyper-V 2012 R2 replicatie-beleid met de volgende opdracht:

    
        $ReplicationFrequencyInSeconds = "300";     #options are 30,300,900
        $PolicyName = “replicapolicy”
        $RepProvider = HyperVReplica2012R2
        $Recoverypoints = 24                    #specify the number of hours to retain recovery pints
        $AppConsistentSnapshotFrequency = 4 #specify the frequency (in hours) at which app consistent snapshots are taken
        $AuthMode = "Kerberos"  #options are "Kerberos" or "Certificate"
        $AuthPort = "8083"  #specify the port number that will be used for replication traffic on Hyper-V hosts
        $InitialRepMethod = "Online" #options are "Online" or "Offline"

        $policyresult = New-AzureRmSiteRecoveryPolicy -Name $policyname -ReplicationProvider $RepProvider -ReplicationFrequencyInSeconds $Replicationfrequencyinseconds -RecoveryPoints $recoverypoints -ApplicationConsistentSnapshotFrequencyInHours $AppConsistentSnapshotFrequency -Authentication $AuthMode -ReplicationPort $AuthPort -ReplicationMethod $InitialRepMethod 

    > [AZURE.NOTE] De cloud VMM Hyper-V hosts met verschillende versies van Windows Server (zoals vermeld in de voorwaarden van de Hyper-V) kan bevatten, maar het replicatiebeleid is een specifieke versie van het besturingssysteem. Als er verschillende hosts die op verschillende besturingssystemen worden uitgevoerd, maakt u replicatie van afzonderlijke beleidsregels voor elke versie van het besturingssysteem. Voor bijvoorbeeld: als u vijf hosts waarop Windows Servers 2012 en drie op Windows Server 2012 R2 hebt, maakt u twee replicatie beleid: één voor elk type van besturingssystemen.

2.  De primaire bescherming-container (primaire VMM-wolk) en herstel bescherming container (VMM wolk recovery) opvragen door de volgende opdrachten uit te voeren:
    
        $PrimaryCloud = "testprimarycloud"
        $primaryprotectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloud;  

        $RecoveryCloud = "testrecoverycloud"
        $recoveryprotectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $RecoveryCloud;  
  
3.  Het beleid dat u hebt gemaakt in stap 1 met de beschrijvende naam van het beleid worden opgehaald

        $policy = Get-AzureRmSiteRecoveryPolicy -FriendlyName $policyname

4.  De koppeling van de container bescherming (VMM-wolk) starten met het replicatiebeleid:

        $associationJob  = Start-AzureRmSiteRecoveryPolicyAssociationJob -Policy     $Policy -PrimaryProtectionContainer $primaryprotectionContainer -RecoveryProtectionContainer $recoveryprotectionContainer

5.  Wachten op de koppeling beleid taak is voltooid. U kunt controleren als de taak is voltooid met behulp van het volgende fragment van PowerShell.
   
        $job = Get-AzureRmSiteRecoveryJob -Job $associationJob
        if($job -eq $null -or $job.StateDescription -ne "Completed")
         {
            $isJobLeftForProcessing = $true;
        }

    Nadat de verwerking is voltooid, kunt u de volgende opdracht uitvoeren:

        if($isJobLeftForProcessing)
        {
        Start-Sleep -Seconds 60
        }
        }While($isJobLeftForProcessing)


Om te controleren op de voltooiing van de bewerking, de stappen van de [Activiteit van de Monitor](#monitor).

## <a name="step-5-configure-network-mapping"></a>Stap 5: Toewijzing van netwerk configureren

1. De eerste opdracht haalt de servers voor het huidige herstel van Azure Site vault. De servers van Microsoft Azure Site herstellen met de opdracht opgeslagen in de array-variabele $Servers.

        $Servers = Get-AzureRmSiteRecoveryServer

2. De onderstaande opdrachten de recovery site netwerk voor de VMM bronserver en de doelserver VMM ophalen.

        $PrimaryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[0]        

        $RecoveryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[1]

    
    > [AZURE.NOTE] De bronserver VMM is de eerste of de tweede in de matrix servers. Controleer de namen van de servers VMM en de netwerken op de juiste manier ophalen


4. De uiteindelijke cmdlet maakt een koppeling tussen het primaire netwerk en het netwerk herstellen. De cmdlet geeft het primaire netwerk als het eerste element van de $PrimaryNetworks en het netwerk herstellen als het eerste element van de $RecoveryNetworks.

        New-AzureRmSiteRecoveryNetworkMapping -PrimaryNetwork $PrimaryNetworks[0] -RecoveryNetwork $RecoveryNetworks[0]

## <a name="step-6-configure-storage-mapping"></a>Stap 6: Opslag toewijzing configureren

1. De onderstaande opdracht haalt u de lijst met categorieën voor opslag in de variabele $storageclassifications.

        $storageclassifications = Get-AzureRmSiteRecoveryStorageClassification


2. De onderstaande opdrachten krijgt de indeling van de bron in $SourceClassificaion variabele en doel-indeling in de variabele $TargetClassification. 

        $SourceClassificaion = $storageclassifications[0]

        $TargetClassification = $storageclassifications[1]

    
    > [AZURE.NOTE] De classificaties van de bron en doel kunnen elk element in de matrix zijn. Raadpleeg de uitvoer van de onder de opdracht om de index van de bron- en classificaties in de matrix $storageclassifications. 
    
    > Get-AzureRmSiteRecoveryStorageClassification | Select-Object - eigenschap FriendlyName, Id | Tabel opmaken


3. De onderstaande cmdlet maakt een koppeling tussen de bron-indeling en de indeling van het doel. 

        New-AzureRmSiteRecoveryStorageClassificationMapping -PrimaryStorageClassification $SourceClassificaion -RecoveryStorageClassification $TargetClassification

## <a name="step-7-enable-protection-for-virtual-machines"></a>Stap 7: Beveiliging voor virtuele machines inschakelen

Nadat de servers, wolken en netwerken goed zijn geconfigureerd, kunt u de beveiliging voor virtuele machines in de cloud. 


  1. Als beveiliging wilt inschakelen, kunt u de volgende opdracht om de container bescherming uitvoeren:
    
            $PrimaryProtectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloudName
    
  2. De dienst bescherming (VM) opvragen door de volgende opdracht uit te voeren:
    
            $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -friendlyName $VMName -ProtectionContainer $PrimaryProtectionContainer
        
  3. Replicatie voor VM inschakelen door de volgende opdracht uit te voeren:

            $jobResult = Set-AzureRmSiteRecoveryProtectionEntity -ProtectionEntity $protectionentity -Protection Enable -Policy $policy


## <a name="test-your-deployment"></a>De implementatie testen

Voor het testen van de implementatie kunt u een failover testen voor een enkele virtuele machine, uitvoeren of een herstelplan dat bestaat uit meerdere virtuele machines maken en uitvoeren van failover test voor het plan. Failover van de test simuleert de failover- en herstel mechanisme in een geïsoleerd netwerk. 

> [AZURE.NOTE] U kunt een herstelplan voor de toepassing in Azure portal maken.

Om te controleren op de voltooiing van de bewerking, de stappen van de [Activiteit van de Monitor](#monitor).


### <a name="run-a-test-failover"></a>Een failover test uitvoeren

1.  Voer de onderstaande cmdlets om de VM-netwerk waarop u wilt testen van failover de VMs te.

        $Servers = Get-AzureRmSiteRecoveryServer
        $RecoveryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[1]

2.  Failover van een VM test uitvoeren door het volgende te doen:
    
        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -FriendlyName $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryTestFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity -VMNetwork $RecoveryNetworks[1] 

2.  Failover van een herstelplan test uitvoeren door het volgende te doen:
    
        $recoveryplanname = "test-recovery-plan"

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult =  Start-AzureRmSiteRecoveryTestFailoverJob -Direction PrimaryToRecovery -Recoveryplan $recoveryplan -VMNetwork $RecoveryNetworks[1] 

### <a name="run-a-planned-failover"></a>Een geplande failover uitvoeren

1. Een geplande failover van een VM uitvoeren door het volgende te doen:
    
        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity

2. Een geplande failover van een herstelplan uitvoeren door het volgende te doen:
    
        $recoveryplanname = "test-recovery-plan"

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult =  Start-AzureRmSiteRecoveryPlannedFailoverJob -Direction PrimaryToRecovery -Recoveryplan $recoveryplan

### <a name="run-an-unplanned-failover"></a>Een niet-geplande failover uitvoeren

1. Een niet-geplande failover van een VM uitvoeren door het volgende te doen:
        
        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryUnPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity 

2. een niet-geplande failover van een herstelplan uitvoeren door het volgende te doen:
        
        $recoveryplanname = "test-recovery-plan"

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult =  Start-AzureRmSiteRecoveryUnPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity 
    
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

[Lees meer](https://msdn.microsoft.com/library/azure/mt637930.aspx) over het herstel van Azure Site met Azure Resource Manager PowerShell-cmdlets.

