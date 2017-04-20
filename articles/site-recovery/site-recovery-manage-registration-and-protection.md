<properties
    pageTitle="Servers verwijderen en uitschakelen van beveiliging | Microsoft Azure" 
    description="In dit artikel wordt beschreven hoe servers uit de kluis van een Site herstellen registratie en bescherming van virtuele machines en fysieke servers uitschakelen." 
    services="site-recovery" 
    documentationCenter="" 
    authors="rayne-wiselman" 
    manager="jwhit" 
    editor=""/>

<tags 
    ms.service="site-recovery" 
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery" 
    ms.date="10/05/2016" 
    ms.author="raynew"/>

# <a name="remove-servers-and-disable-protection"></a>Servers verwijderen en uitschakelen van beveiliging

De Azure-Site-Recovery-service draagt bij aan de continuïteit en noodherstel recovery (BCDR) bedrijfsstrategie door regie van replicatie, failover en herstel van virtuele machines en fysieke servers. Machines kunnen worden gerepliceerd naar Azure, of naar een secundaire op ruimten Datacenter. Voor een snel overzicht lezen [Wat is Azure Site terugzetten?](site-recovery-overview.md)

## <a name="overview"></a>Overzicht

Dit artikel wordt beschreven hoe u servers uit de kluis Site Recovery registratie en het uitschakelen van beveiliging voor virtuele machines die zijn beschermd door het herstel van de Site. 

Opmerkingen of vragen aan de onderkant van dit artikel, of op het [Forum van Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)boeken.

## <a name="unregister-a-vmm-server"></a>De registratie van een server VMM

U de registratie van een VMM-server van een kluis door het verwijderen van de server op het tabblad **Servers** in de portal Azure Site herstellen. Houd rekening met:

-  **VMM verbonden server**: aangeraden dat u de VMM registratie server ongedaan maken als deze verbonden met Azure. Dit zorgt ervoor dat instellingen op de lokale VMM-server en de VMM servers gekoppeld (VMM servers met wolken die zijn toegewezen aan de wolken op de server die u wilt verwijderen) naar behoren worden gereinigd. Wij raden u aan dat u alleen een niet-verbonden server verwijderen als er een permanent probleem met de verbinding.
- **Server niet verbonden VMM**: als de VMM-server niet is verbonden wanneer u het verwijderen moet u voor het uitvoeren van scripts handmatig uitvoeren voor het opruimen. Het script is beschikbaar in de [galerie van Microsoft](http://aka.ms/asr-cleanup-script-vmm). Noteer de VMM-ID van de server het opruimen te voltooien.
- **VMM-server in het cluster**: als de registratie van een VMM-server die geïmplementeerd in een cluster moet u het volgende doen:

    - Als de server de aangesloten, verwijdert u de verbonden VMM-server op het tabblad **Servers** . De Provider op de server verwijderen, aanmelden op elk clusterknooppunt en van het Configuratiescherm verwijderen. Voer het opruimen script waarnaar wordt verwezen in de vorige sectie op alle passieve knooppunten in het cluster te verwijderen van registervermeldingen.
    - Als de server niet is aangesloten moet u het script opschonen uitvoeren op alle clusterknooppunten.

### <a name="unregister-an-unconnected-vmm-server"></a>De registratie van een niet-verbonden VMM-server

Op de VMM-server die u wilt verwijderen:

1. De registratie van de VMM-server vanaf de portal Azure.
2. Download het script opruimen op de server VMM.
3. PowerShell openen met uitvoeren als beheerder optie voor het wijzigen van de uitvoering van het beleid voor het standaardbereik (LocalMachine).
4. Volg de instructies in het script. 

Op de servers VMM met wolken die zijn gekoppeld aan de wolken op de server die u wilt verwijderen:

1. Voer het script opruimen en volg de stappen 2 tot en met 4.
2. De VMM-ID opgeven voor de VMM-server die verwijderd is. 
3. Dit script worden de registratiegegevens voor de VMM Server en cloud koppelingsgegevens verwijderd.


## <a name="unregister-a-hyper-v-server-in-a-hyper-v-site"></a>De registratie van een Hyper-V server in een site met Hyper-V

Wanneer herstel van Azure-Site is geïmplementeerd ter bescherming van virtuele machines die zich bevindt op een Hyper-V server in een site met Hyper-V (met geen VMM-server) kunt u als volgt een Hyper-V server uit een kluis registratie:

1. Beveiliging voor virtuele machines die op de Hyper-V server uitschakelen.
2. Selecteer de server op het tabblad **Servers** in de portal Azure Site herstellen > verwijderen. De server hoeft te worden verbonden met Azure wanneer u dit doet.
3. Voer het volgende script om de instellingen op de server opschonen en dat de registratie van de kluis. 

        pushd .
        try
        {
             $windowsIdentity=[System.Security.Principal.WindowsIdentity]::GetCurrent()
             $principal=new-object System.Security.Principal.WindowsPrincipal($windowsIdentity)
             $administrators=[System.Security.Principal.WindowsBuiltInRole]::Administrator
             $isAdmin=$principal.IsInRole($administrators)
             if (!$isAdmin)
             {
                "Please run the script as an administrator in elevated mode."
                $choice = Read-Host
                return;       
             }
    
            $error.Clear()    
            "This script will remove the old Azure Site Recovery Provider related properties. Do you want to continue (Y/N) ?"
            $choice =  Read-Host
        
            if (!($choice -eq 'Y' -or $choice -eq 'y'))
            {
            "Stopping cleanup."
            return;
            }
        
            $serviceName = "dra"
            $service = Get-Service -Name $serviceName
            if ($service.Status -eq "Running")
            {
                "Stopping the Azure Site Recovery service..."
                net stop $serviceName
            }
        
            $asrHivePath = "HKLM:\SOFTWARE\Microsoft\Azure Site Recovery"
            $registrationPath = $asrHivePath + '\Registration'
            $proxySettingsPath = $asrHivePath + '\ProxySettings'
            $draIdvalue = 'DraID'
        
            if (Test-Path $asrHivePath)
            {
                if (Test-Path $registrationPath)
                {
                    "Removing registration related registry keys."  
                    Remove-Item -Recurse -Path $registrationPath
                }
    
                if (Test-Path $proxySettingsPath)
            {
                    "Removing proxy settings"
                    Remove-Item -Recurse -Path $proxySettingsPath
                }
    
                $regNode = Get-ItemProperty -Path $asrHivePath
                if($regNode.DraID -ne $null)
                {            
                    "Removing DraId"
                    Remove-ItemProperty -Path $asrHivePath -Name $draIdValue
                }
                "Registry keys removed."
            }
    
            # First retrive all the certificates to be deleted
            $ASRcerts = Get-ChildItem -Path cert:\localmachine\my | where-object {$_.friendlyname.startswith('ASR_SRSAUTH_CERT_KEY_CONTAINER') -or $_.friendlyname.startswith('ASR_HYPER_V_HOST_CERT_KEY_CONTAINER')}
            # Open a cert store object
            $store = New-Object System.Security.Cryptography.X509Certificates.X509Store("My","LocalMachine")
            $store.Open('ReadWrite')
            # Delete the certs
            "Removing all related certificates"
            foreach ($cert in $ASRcerts)
            {
                $store.Remove($cert)
            }
        }catch
        {   
            [system.exception]
            Write-Host "Error occured" -ForegroundColor "Red"
            $error[0] 
            Write-Host "FAILED" -ForegroundColor "Red"
        }
        popd


## <a name="stop-protecting-a-hyper-v-virtual-machine"></a>Stoppen met Hyper-V virtuele machines beveiligen

Als u wilt stoppen met Hyper-V virtuele machines beveiligen moet u de beveiliging voor het verwijderen. Afhankelijk van hoe u de beveiliging opheffen moet u mogelijk de beveiligingsinstellingen op de computer handmatig opschonen. 

### <a name="remove-protection"></a>Beveiliging opheffen

1. Selecteer de virtuele machine van de cloud-eigenschappen op het tabblad **virtuele Machines** > **verwijderen**.
2. Op de pagina **verwijderen bevestigen van de virtuele Machine** hebt u verschillende mogelijkheden:

    - **Uitschakelen bescherming**— als u deze optie opslaan en de virtuele machine niet langer beschermd door het herstel van de Site. Beveiligingsinstellingen voor de virtuele machine wordt automatisch worden opgeruimd.
    - **Verwijderen uit de kluis**, als u deze optie de virtuele machine worden verwijderd uit de kluis Recovery Site alleen. Instellingen voor de beveiliging van gebouwen voor de virtuele machine worden niet beïnvloed. U moet de instellingen handmatig aan de instellingen voor de beveiliging en de virtuele machine te verwijderen uit de Azure abonnement en beveiligingsinstellingen die u wilt opschonen ze handmatig met behulp van de onderstaande instructies verwijderd verwijderen.

Als u de virtuele machine en de vaste schijven te verwijderen wordt deze verwijderd uit de doellocatie.

### <a name="clean-up-protection-settings-manually-between-vmm-sites"></a>Instellingen voor gegevensbeveiliging (tussen sites VMM) handmatig opschonen

Als u **Stoppen met het beheer van de virtuele machine**hebt geselecteerd, handmatig instellingen verwijderen:

1. Op de primaire server dit script uitvoeren vanuit de console VMM voor het opschonen van de instellingen voor de primaire virtuele machine. Klik op de knop PowerShell om de VMM PowerShell-console te openen in de console van VMM. SQLVM1 vervangen door de naam van de virtuele machine.

         $vm = get-scvirtualmachine -Name "SQLVM1"
         Set-SCVirtualMachine -VM $vm -ClearDRProtection

2. Voer dit script voor het opschonen van de instellingen voor de secundaire virtuele machine op de secundaire server voor de VMM:

        $vm = get-scvirtualmachine -Name "SQLVM1"
        Remove-SCVirtualMachine -VM $vm -Force

3. Op de secundaire server VMM, nadat u het script uitvoeren vernieuwen, de virtuele machines in de Hyper-V host-server zodat de secundaire virtuele machine opnieuw wordt gedetecteerd in de console van VMM.
4. De replicatie-instellingen alleen VMM-server, wordt de bovenstaande stappen gewist. Als u verwijderen van de replicatie van de virtuele machine voor de virtuele machine wilt. Moet u de onderstaande stappen op zowel de primaire en secundaire doen virtuele machines. Voer het onderstaande script verwijderen van replicatie en SQLVM1 vervangt door de naam van de virtuele machine.

        Remove-VMReplication –VMName “SQLVM1”


### <a name="clean-up-protection-settings-manually-between-on-premises-vmm-sites-and-azure"></a>Instellingen voor gegevensbeveiliging handmatig (tussen VMM-sites op gebouwen en Azure) opschonen

1. Voer dit script voor het opschonen van de instellingen voor de primaire virtuele machine op de bronserver VMM:

        $vm = get-scvirtualmachine -Name "SQLVM1"
        Set-SCVirtualMachine -VM $vm -ClearDRProtection

2. De replicatie-instellingen alleen VMM-server, wordt de bovenstaande stappen gewist. Zodra u de replicatie van VMM server hebt verwijderd, ervoor zorgen u replicatie voor de virtuele machine op de Hyper-V host-server met dit script verwijdert. SQLVM1 vervangen door de naam van de virtuele machine en host01.contoso.com met de naam van de Hyper-V host-server.

        $vmName = "SQLVM1"
        $hostName  = "host01.contoso.com"
        $vm = Get-WmiObject -Namespace "root\virtualization\v2" -Query "Select * From Msvm_ComputerSystem Where ElementName = '$vmName'" -computername $hostName
        $replicationService = Get-WmiObject -Namespace "root\virtualization\v2"  -Query "Select * From Msvm_ReplicationService"  -computername $hostName
        $replicationService.RemoveReplicationRelationship($vm.__PATH)

### <a name="clean-up-protection-settings-manually-between-hyper-v-sites-and-azure"></a>Instellingen voor gegevensbeveiliging handmatig (tussen sites Hyper-V en Azure) opschonen

1. Op de Hyper-V host bronserver, u kunt replicatie voor de virtuele machine met dit script. SQLVM1 vervangen door de naam van de virtuele machine.

        $vmName = "SQLVM1"
        $vm = Get-WmiObject -Namespace "root\virtualization\v2" -Query "Select * From Msvm_ComputerSystem Where ElementName = '$vmName'"
        $replicationService = Get-WmiObject -Namespace "root\virtualization\v2"  -Query "Select * From Msvm_ReplicationService"
        $replicationService.RemoveReplicationRelationship($vm.__PATH)

## <a name="stop-protecting-a-vmware-virtual-machine-or-a-physical-server"></a>Stop een VMware virtuele machine of een fysieke server beveiligen

Als u wilt stoppen met het beveiligen van een VMware virtuele machine of een fysieke server moet u de beveiliging voor het verwijderen. Afhankelijk van hoe u de beveiliging opheffen moet u mogelijk de beveiligingsinstellingen op de computer handmatig opschonen. 

### <a name="remove-protection"></a>Beveiliging opheffen

1. Selecteer de virtuele machine van de cloud-eigenschappen op het tabblad **virtuele Machines** > **verwijderen**.
2. Selecteer een van de opties op de **virtuele Machine verwijderen** :

    - **Uitschakelen bescherming (gebruik voor herstel drill en volume-formaat)**, zult u alleen zien en schakel deze optie in als u hebt:
        - **Het volume voor de virtuele machine resized**— als u de grootte van een volume dat de virtuele machine in een kritieke toestand wordt. Selecteer deze optie als dit gebeurt. Bescherming, behoud punten in Azure herstel wordt uitgeschakeld. Wanneer u opnieuw voor de computer inschakelen wordt de gegevens voor het nieuwe formaat volume overgemaakt naar Azure.
        - **Een failover uitvoeren**, nadat u uw omgeving hebt getest door een failover uitvoeren van op ruimten VMware virtuele machines of fysieke servers naar Azure, selecteer deze optie om de bescherming van uw virtuele machines van op gebouwen opnieuw start. Deze optie wordt elke virtuele machine uitgeschakeld en moet u vervolgens weer in te schakelen voor hen. Houd rekening met:
            - Het uitschakelen van de virtuele machine met deze instelling heeft geen invloed op de virtuele machine van replica in Azure.
            - U kunt de mobiliteit service kan niet verwijderen van de virtuele machine.
    
    - **Uitschakelen bescherming**— als u deze optie opslaan en de computer niet meer beschermd door sites worden hersteld. Beveiligingsinstellingen voor de computer zal automatisch worden opgeruimd.
    - **Verwijderen uit de kluis**, als u deze optie alleen uit de kluis van het herstel van de Site de machine worden verwijderd. Instellingen voor de beveiliging van gebouwen voor de machine worden niet beïnvloed. Om de instellingen op de computer te verwijderen en te verwijderen van de virtuele machine uit de Azure moet abonnement en u de instellingen voor opschonen door het verwijderen van de mobiliteit service.
    
        ![Opties verwijderen](./media/site-recovery-manage-registration-and-protection/remove-vm.png)

















