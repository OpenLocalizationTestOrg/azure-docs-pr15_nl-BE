<properties
    pageTitle="Azure-servers beveiligen met Azure PowerShell Azure Resource Manager | Microsoft Azure"
    description="Bescherming van Azure met Azure-Site-Recovery-servers automatiseren met behulp van PowerShell en Azure Resource Manager."
    services="site-recovery"
    documentationCenter=""
    authors="bsiva"
    manager="abhiag"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="backup-recovery"
    ms.date="09/27/2016"
    ms.author="bsiva"/>

# <a name="replicate-between-on-premises-hyper-v-virtual-machines-and-azure-by-using-powershell-and-azure-resource-manager"></a>Repliceren tussen op ruimten Hyper-V virtuele machines en Azure met PowerShell en Azure Resource Manager

> [AZURE.SELECTOR]
- [Azure Portal](site-recovery-hyper-v-site-to-azure.md)
- [PowerShell - Resource Manager](site-recovery-deploy-with-powershell-resource-manager.md)
- [Klassieke Portal](site-recovery-hyper-v-site-to-azure-classic.md)



## <a name="overview"></a>Overzicht

Azure-Site-Recovery draagt bij aan de continuïteit en noodherstel herstel bedrijfsstrategie door regie van replicatie, failover en herstel van virtuele machines in een aantal implementatiescenario's. Zie voor een volledige lijst van implementatiescenario's [Azure Site-overzicht](site-recovery-overview.md).

Azure PowerShell is een module waarmee u de cmdlets voor het beheren van Azure via Windows PowerShell. Deze kan werken met twee typen modules: de module Azure-profiel of de module Azure Resource Manager.

Site herstel PowerShell-cmdlets, beschikbaar met Azure PowerShell Azure Resource Manager helpt u bij het beveiligen en herstellen van uw servers in Azure.

In dit artikel wordt beschreven hoe Windows PowerShell, samen met Azure Resource Manager gebruiken voor de implementatie van Recovery-Site configureren en evoluerende Azure server-beveiliging. In het voorbeeld gebruikt in dit artikel wordt beschreven hoe u beschermen, failover en virtuele machines op een Hyper-V host naar Azure, met Azure PowerShell Azure Resource Manager te herstellen.

> [AZURE.NOTE] De Site herstel PowerShell-cmdlets op dit moment kunt u het volgende configureren: een Virtual Machine Manager site naar de andere, een Virtual Machine Manager site Azure en Azure een Hyper-V-site.

U hoeft niet te worden een expert PowerShell te gebruiken in dit artikel, maar u moet begrijpen van de basisbegrippen, zoals modules, cmdlets en sessies. Zie [Aan de slag met Windows PowerShell](http://technet.microsoft.com/library/hh857337.aspx)voor meer informatie over Windows PowerShell.

U kunt ook meer informatie over het [Gebruik van Azure PowerShell Azure Resource Manager](../powershell-azure-resource-manager.md).

> [AZURE.NOTE] Microsoft-partners die deel van het programma Cloud Solution Provider (CSP uitmaken) kunnen configureren en beheren van de bescherming van hun klanten klanten hun respectieve CSP abonnementen (huurder abonnementen)-servers.

## <a name="before-you-start"></a>Voordat u begint

Zorg ervoor dat u hebt deze vereisten:

- Een account van [Microsoft Azure](https://azure.microsoft.com/) . U kunt beginnen met een [gratis proefperiode](https://azure.microsoft.com/pricing/free-trial/). Daarnaast kunt u lezen over [Azure herstel sitebeheerder prijzen](https://azure.microsoft.com/pricing/details/site-recovery/).
- Azure PowerShell 1.0. Zie voor meer informatie over deze release en het installeren van deze [Azure PowerShell 1.0.](https://azure.microsoft.com/)
- De modules [AzureRM.SiteRecovery](https://www.powershellgallery.com/packages/AzureRM.SiteRecovery/) en [AzureRM.RecoveryServices](https://www.powershellgallery.com/packages/AzureRM.RecoveryServices/) . U krijgt de nieuwste versies van deze modules uit de [galerie met PowerShell](https://www.powershellgallery.com/)

In dit artikel ziet u hoe met Azure Powershell Azure Resource Manager configureren en beheren van de beveiliging van uw servers. In het voorbeeld gebruikt in dit artikel wordt beschreven hoe u een virtuele machine, uitgevoerd op een host Hyper-V naar Azure beschermen. De volgende vereisten gelden voor dit voorbeeld. Raadpleeg de documentatie die deel uitmaakt van dat scenario voor een meer uitgebreide reeks voorschriften voor de verschillende scenario's voor herstel van de Site.

- Een Hyper-V-host met Windows Server 2012 R2 of Microsoft Hyper-V Server 2012 R2 met een of meer virtuele machines.
- Hyper-V-servers aangesloten op het Internet, rechtstreeks of via een proxy.
- De virtuele machines die u wilt beveiligen moet met een [virtuele Machine vereisten](site-recovery-best-practices.md#virtual-machines)voldoen.


## <a name="step-1-sign-in-to-your-azure-account"></a>Stap 1: Aanmelden bij uw account Azure


1. Open een console PowerShell en uitvoeren van deze opdracht voor het aanmelden bij uw account Azure. De cmdlet wordt een webpagina, wordt u gevraagd uw referenties voor de account.

        Login-AzureRmAccount

    Ook u ook uw referenties kan opnemen als een parameter voor de `Login-AzureRmAccount` cmdlet, met behulp van de `-Credential` parameter.

    Als u CSP partner namens een huurder, op de klant als een huurder, hun tenantID of de huurder primaire domeinnaam.

        Login-AzureRmAccount -Tenant "fabrikam.com"

2. Een account kan verschillende abonnementen, hebben, zodat u het abonnement dat u wilt gebruiken met de account te koppelen.

        Select-AzureRmSubscription -SubscriptionName $SubscriptionName

3.  Controleer of uw abonnement is geregistreerd voor het gebruik van de Azure providers voor Recovery Services en het herstellen van een Site met behulp van de volgende opdrachten:

    - `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.RecoveryServices`
    -  `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.SiteRecovery`

    In de uitvoer van deze opdrachten als de **RegistrationState** is ingesteld op **geregistreerd**, kunt u doorgaan met stap 2. Als dit niet het geval is, moet u de provider ontbreekt in uw abonnement registreren.

    Om te registreren de Azure-provider voor het herstel van de Site en Recovery Services, voert u de volgende opdrachten:

        Register-AzureRmResourceProvider -ProviderNamespace Microsoft.SiteRecovery
        Register-AzureRmResourceProvider -ProviderNamespace Microsoft.RecoveryServices

    Controleer of de providers zijn geregistreerd met behulp van de volgende opdrachten: `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.RecoveryServices` en `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.SiteRecovery`.



## <a name="step-2-set-up-the-recovery-services-vault"></a>Stap 2: Instellen van de kluis Recovery Services

1. Maak een bronnenbeheerder Azure bronnengroep waarin u zult de kluis maken of een bestaande bronnengroep gebruiken. U kunt een nieuwe resourcegroep maken met de volgende opdracht:

        New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Geo  

    de variabele $ResourceGroupName bevat de naam van de resourcegroep die u wilt maken waarbij de variabele $Geo bevat de Azure regio waarin de resourcegroep (bijvoorbeeld "Brazilië-Zuid") te maken.

    Kunt u een lijst met resourcegroepen in uw abonnement via de `Get-AzureRmResourceGroup` cmdlet.

2. Maak een nieuwe kluis van Azure Recovery Services als volgt:

        $vault = New-AzureRmRecoveryServicesVault -Name <string> -ResourceGroupName <string> -Location <string>

    U kunt een lijst met bestaande kluizen ophalen met behulp van de `Get-AzureRmRecoveryServicesVault` cmdlet.

> [AZURE.NOTE] Als u bewerkingen uitvoeren op de Site herstel kluizen gemaakt met de klassieke portal of de module Azure Service Management PowerShell wilt, kunt u een lijst met dergelijke kluizen ophalen met behulp van de `Get-AzureRmSiteRecoveryVault` cmdlet. Maak een nieuwe kluis Recovery Services voor alle nieuwe bewerkingen. De Site herstel kluizen die u eerder hebt gemaakt, worden ondersteund, maar hoeft niet de nieuwste functies.

## <a name="step-3-set-the-recovery-services-vault-context"></a>Stap 3: De context van de kluis Recovery Services instellen

1.  De kluis-context instellen door de volgende opdracht uit te voeren:

        Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault

## <a name="step-4-create-a-hyper-v-site-and-generate-a-new-vault-registration-key-for-the-site"></a>Stap 4: Een Hyper-V-site maken en een nieuwe sleutel van de registratie vault voor de site genereren.

1. Een nieuwe site voor Hyper-V wordt als volgt maken:

        $sitename = "MySite"                #Specify site friendly name
        New-AzureRmSiteRecoverySite -Name $sitename

    Deze cmdlet begint een taak Recovery-Site te maken van de site en een Site Recovery Project-object als resultaat geeft. Wachten op de taak te voltooien en te controleren dat de taak is voltooid.

    U kunt het taakobject halen en daarmee de huidige status van de taak controleren via de cmdlet Get-AzureRmSiteRecoveryJob.
2. Genereren en een registratie sleutel voor de site, als volgt downloaden:

        $SiteIdentifier = Get-AzureRmSiteRecoverySite -Name $sitename | Select -ExpandProperty SiteIdentifier
        Get-AzureRmRecoveryServicesVaultSettingsFile -Vault $vault -SiteIdentifier $SiteIdentifier -SiteFriendlyName $sitename -Path $Path

    Kopieer de gedownloade sleutel naar de Hyper-V host. U moet de sleutel voor het registreren van de Hyper-V host naar de site.

## <a name="step-5-install-the-azure-site-recovery-provider-and-azure-recovery-services-agent-on-your-hyper-v-host"></a>Stap 5: De provider Azure Site herstellen en Azure Services herstelagent installeren op de Hyper-V-host

1. Het installatieprogramma downloaden voor de meest recente versie van de provider van [Microsoft](https://aka.ms/downloaddra).

2. De registratiestap uitvoeren van het installatieprogramma op de Hyper-V host en aan het einde van de installatie verder.

3. Geef desgevraagd de gedownloade registratie sleutel en een volledige registratie van de Hyper-V host naar de site.

4. Controleer of de Hyper-V host is geregistreerd op de site met de volgende opdracht:

        $server =  Get-AzureRmSiteRecoveryServer -FriendlyName $server-friendlyname

## <a name="step-6-create-a-replication-policy-and-associate-it-with-the-protection-container"></a>Stap 6: Een replicatiebeleid maken en koppelen aan de container bescherming

1. Maak een replicatiebeleid voor als volgt:

        $ReplicationFrequencyInSeconds = "300";     #options are 30,300,900
        $PolicyName = “replicapolicy”
        $Recoverypoints = 6                 #specify the number of recovery points
        $storageaccountID = Get-AzureRmStorageAccount -Name "mystorea" -ResourceGroupName "MyRG" | Select -ExpandProperty Id

        $PolicyResult = New-AzureRmSiteRecoveryPolicy -Name $PolicyName -ReplicationProvider “HyperVReplicaAzure” -ReplicationFrequencyInSeconds $ReplicationFrequencyInSeconds  -RecoveryPoints $Recoverypoints -ApplicationConsistentSnapshotFrequencyInHours 1 -RecoveryAzureStorageAccountId $storageaccountID

    Controleer de geretourneerde taak om ervoor te zorgen dat het beleid maken van de replicatie is voltooid.

    >[AZURE.IMPORTANT] De opslag hebt opgegeven, moet zich in hetzelfde gebied, Azure als uw kluis Recovery Services en geo-replicatie ingeschakeld moet hebben.
    >
    > - Als de opgegeven Recovery storage-account van het type opslag Azure (klassiek), herstellen failover van de beveiligde computers de machine Azure IaaS (Classic).
    > - Als de opgegeven Recovery storage-account van het type opslag Azure (Azure Resource Manager), herstellen failover van de beveiligde computers Azure IaaS (Azure Resource Manager) van de computer.

2. Haal de bescherming container die overeenkomt met de site, als volgt:

        $protectionContainer = Get-AzureRmSiteRecoveryProtectionContainer
3.  De koppeling van de container bescherming met het replicatiebeleid als volgt starten:

        $Policy = Get-AzureRmSiteRecoveryPolicy -FriendlyName $PolicyName
        $associationJob  = Start-AzureRmSiteRecoveryPolicyAssociationJob -Policy $Policy -PrimaryProtectionContainer $protectionContainer

    Wacht tot de taak van de koppeling te voltooien en ervoor te zorgen dat deze met succes is voltooid.

##<a name="step-7-enable-protection-for-virtual-machines"></a>Stap 7: Beveiliging voor virtuele machines inschakelen

1. Haal de bescherming entiteit die overeenkomt met de VM die u beveiligen wilt, als volgt:

        $VMFriendlyName = "Fabrikam-app"                    #Name of the VM
        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -ProtectionContainer $protectionContainer -FriendlyName $VMFriendlyName

2. Start de virtuele machine als volgt beveiligen:

        $Ostype = "Windows"                                 # "Windows" or "Linux"
        $DRjob = Set-AzureRmSiteRecoveryProtectionEntity -ProtectionEntity $protectionEntity -Policy $Policy -Protection Enable -RecoveryAzureStorageAccountId $storageaccountID  -OS $OStype -OSDiskName $protectionEntity.Disks[0].Name

    >[AZURE.IMPORTANT] De opslag hebt opgegeven, moet zich in hetzelfde gebied, Azure als uw kluis Recovery Services en geo-replicatie ingeschakeld moet hebben.
    >
    > - Als de opgegeven Recovery storage-account van het type opslag Azure (klassiek), herstellen failover van de beveiligde computers de machine Azure IaaS (Classic).
    > - Als de opgegeven Recovery storage-account van het type opslag Azure (Azure Resource Manager), herstellen failover van de beveiligde computers Azure IaaS (Azure Resource Manager) van de computer.

    > Als de VM u beschermt meer dan één schijf is gekoppeld heeft, geeft u de schijf met behulp van de parameter *OSDiskName* .

3. Wachten op de virtuele machines te bereiken een beveiligde staat na de initiële replicatie. Dit kan even duren, afhankelijk van factoren zoals de hoeveelheid gegevens worden gerepliceerd en de beschikbare upstream bandbreedte voor Azure. De taakstatus en de StateDescription zijn als volgt bijgewerkt na het bereiken van een beschermde status VM.

        PS C:\> $DRjob = Get-AzureRmSiteRecoveryJob -Job $DRjob

        PS C:\> $DRjob | Select-Object -ExpandProperty State
        Succeeded

        PS C:\> $DRjob | Select-Object -ExpandProperty StateDescription
        Completed

4. Herstel eigenschappen, zoals de grootte van de rol VM en het koppelen van de virtuele machine netwerkinterfacekaarten aan bij failover Azure netwerk bijwerken.

        PS C:\> $nw1 = Get-AzureRmVirtualNetwork -Name "FailoverNw" -ResourceGroupName "MyRG"

        PS C:\> $VMFriendlyName = "Fabrikam-App"

        PS C:\> $VM = Get-AzureRmSiteRecoveryVM -ProtectionContainer $protectionContainer -FriendlyName $VMFriendlyName

        PS C:\> $UpdateJob = Set-AzureRmSiteRecoveryVM -VirtualMachine $VM -PrimaryNic $VM.NicDetailsList[0].NicId -RecoveryNetworkId $nw1.Id -RecoveryNicSubnetName $nw1.Subnets[0].Name

        PS C:\> $UpdateJob = Get-AzureRmSiteRecoveryJob -Job $UpdateJob

        PS C:\> $UpdateJob


        Name             : b8a647e0-2cb9-40d1-84c4-d0169919e2c5
        ID               : /Subscriptions/a731825f-4bf2-4f81-a611-c331b272206e/resourceGroups/MyRG/providers/Microsoft.RecoveryServices/vault
                           s/MyVault/replicationJobs/b8a647e0-2cb9-40d1-84c4-d0169919e2c5
        Type             : Microsoft.RecoveryServices/vaults/replicationJobs
        JobType          : UpdateVmProperties
        DisplayName      : Update the virtual machine
        ClientRequestId  : 805a22a3-be86-441c-9da8-f32685673112-2015-12-10 17:55:51Z-P
        State            : Succeeded
        StateDescription : Completed
        StartTime        : 10-12-2015 17:55:53 +00:00
        EndTime          : 10-12-2015 17:55:54 +00:00
        TargetObjectId   : 289682c6-c5e6-42dc-a1d2-5f9621f78ae6
        TargetObjectType : ProtectionEntity
        TargetObjectName : Fabrikam-App
        AllowedActions   : {Restart}
        Tasks            : {UpdateVmPropertiesTask}
        Errors           : {}



## <a name="step-8-run-a-test-failover"></a>Stap 8: Voer een failover testen

1. Een taak van de failover-test als volgt uitgevoerd:

        $nw = Get-AzureRmVirtualNetwork -Name "TestFailoverNw" -ResourceGroupName "MyRG" #Specify Azure vnet name and resource group

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -FriendlyName $VMFriendlyName -ProtectionContainer $protectionContainer

        $TFjob = Start-AzureRmSiteRecoveryTestFailoverJob -ProtectionEntity $protectionEntity -Direction PrimaryToRecovery -AzureVMNetworkId $nw.Id

2. Controleer of de test VM is gemaakt in Azure. (De taak van de failover-test is opgeschort, na het maken van de test VM in Azure. De taak is voltooid door het opschonen van de gemaakte artefacten bij het hervatten van de taak, zoals geïllustreerd in de volgende stap.)

3. Voer de failover test als volgt:

        $TFjob = Resume-AzureRmSiteRecoveryJob -Job $TFjob


##<a name="next-steps"></a>Volgende stappen

[Lees meer](https://msdn.microsoft.com/library/azure/mt637930.aspx) over het herstel van Azure Site met Azure Resource Manager PowerShell-cmdlets.
