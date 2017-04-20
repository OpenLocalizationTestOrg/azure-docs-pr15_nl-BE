<properties
    pageTitle="Opnieuw instellen van het wachtwoord of de configuratie voor extern bureaublad op een Windows VM | Microsoft Azure"
    description="Informatie over het opnieuw instellen van wachtwoord van een account of een extern bureaublad-services op een Windows VM met de Azure portal of Azure PowerShell."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/01/2016"
    ms.author="iainfou"/>

# <a name="how-to-reset-the-remote-desktop-service-or-its-login-password-in-a-windows-vm"></a>Het opnieuw instellen van de service Extern bureaublad of het aanmeldingswachtwoord in een Windows-VM

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Als u geen verbinding maken met een Windows VM (virtual machine), kunt u de lokale administrator-wachtwoord opnieuw instellen of opnieuw instellen van de configuratie van de service Extern bureaublad. U kunt de Azure portal of de uitbreiding toegang tot VM in Azure PowerShell het wachtwoord opnieuw instellen. Als u PowerShell gebruikt, moet u de meest recente PowerShell-module op de computer geïnstalleerd hebt en bent aangemeld bij uw abonnement Azure. Lees voor gedetailleerde stappen [het installeren en configureren van Azure PowerShell](../powershell-install-configure.md).

> [AZURE.TIP] U kunt controleren welke versie van PowerShell die u hebt geïnstalleerd met behulp van`Import-Module Azure, AzureRM; Get-Module Azure, AzureRM | Format-Table Name, Version`

## <a name="windows-vms-in-resource-manager-deployment-model"></a>Windows VMs in implementatiemodel Resource Manager

### <a name="azure-portal"></a>Azure Portal
Selecteer uw VM klikt u op **Bladeren** > **virtuele machines** > *uw virtuele Windows-computer* > **alle instellingen** > **wachtwoord opnieuw instellen**. De blade wachtwoord opnieuw instellen wordt weergegeven:

![Pagina-wachtwoord opnieuw instellen](./media/virtual-machines-windows-reset-rdp/Portal-RM-PW-Reset-Windows.png)

Voer de gebruikersnaam en een nieuw wachtwoord en klik op **Opslaan**. Probeer opnieuw verbinding te maken met uw VM.

### <a name="vmaccess-extension-and-powershell"></a>De extensie VMAccess en PowerShell

Zorg ervoor dat u Azure PowerShell 1.0 of hoger hebt geïnstalleerd en u hebt aangemeld bij uw account met behulp van de `Login-AzureRmAccount` cmdlet.

#### <a name="reset-the-local-administrator-account-password"></a>**Het wachtwoord van de lokale administrator-account opnieuw instellen**

Met de opdracht [Set AzureRmVMAccessExtension](https://msdn.microsoft.com/library/mt619447.aspx) PowerShell kunt u de naam van de administrator-wachtwoord of gebruiker herstellen.

Maak de lokale beheerder referenties door de volgende opdracht:

    $cred=Get-Credential

Als u een andere naam dan de huidige account, de volgende opdracht van de VMAccess-extensie wijzigt de naam van de lokale administrator-account het wachtwoord toegewezen aan de account en een extern bureaublad afmelden gebeurtenis gestart. Als de lokale administrator-account is uitgeschakeld, kan de extensie VMAccess het.

Gebruik de uitbreiding van de VM toegang tot de nieuwe referenties als volgt instellen:

    Set-AzureRmVMAccessExtension -ResourceGroupName "myRG" -VMName "myVM" -Name "myVMAccess" `
        -Location WestUS -UserName $cred.GetNetworkCredential().Username `
        -Password $cred.GetNetworkCredential().Password -typeHandlerVersion "2.0"


Vervangen `myRG`, `myVM`, `myVMAccess`, en de locatie met de waarden die relevant zijn voor de installatie.


#### <a name="reset-the-remote-desktop-service-configuration"></a>**De configuratie voor extern bureaublad-service opnieuw instellen**

Externe toegang kunt voor uw VM u herstellen met behulp van [Set AzureRmVMExtension](https://msdn.microsoft.com/library/mt603745.aspx) of [Set-AzureRmVMAccessExtension](https://msdn.microsoft.com/library/mt619447.aspx), als volgt. (Vervangen door de `myRG`, `myVM`, `myVMAccess` en locatie door uw eigen waarden.)

    Set-AzureRmVMExtension -ResourceGroupName "myRG" -VMName "myVM" `
        -Name "myVMAccess" -ExtensionType "VMAccessAgent" -Location WestUS `
        -Publisher "Microsoft.Compute" -typeHandlerVersion "2.0"

Of:<br>

    Set-AzureRmVMAccessExtension -ResourceGroupName "myRG" -VMName "myVM" `
        -Name "myVMAccess" -Location WestUS -typeHandlerVersion "2.0


> [AZURE.TIP] Een nieuwe naam VM toegang agent toevoegen beide opdrachten aan de virtuele machine. Een VM kan slechts een enkel VM toegang agent hebben op elk gewenst moment. Verwijderen om de VM toegang Agenteigenschappen is ingesteld, de eerder is ingesteld met behulp van access-agent `Remove-AzureRmVMAccessExtension` of `Remove-AzureRmVMExtension`. Vanaf Azure PowerShell versie 1.2.2, kunt u voorkomen dat deze stap bij het gebruik van `Set-AzureRmVMExtension` met een `-ForceRerun` optie. Bij het gebruik van `-ForceRerun`, zorg ervoor dat u dezelfde naam gebruiken voor de VM toegang agent zoals door de vorige opdracht.

Als u nog steeds geen verbinding op afstand op uw virtuele machine, Zie meer stappen om te proberen bij [problemen met extern bureaublad-verbindingen met een Windows Azure virtual machine](virtual-machines-windows-troubleshoot-rdp-connection.md).


## <a name="windows-vms-in-the-classic-deployment-model"></a>VMs in het implementatiemodel klassiek Windows

### <a name="azure-portal"></a>Azure portal

Voor virtuele machines met behulp van het implementatiemodel klassiek gemaakt, kunt u de [Azure portal](https://portal.azure.com) de extern bureaublad-service opnieuw instellen. Klik op: **Bladeren** > **virtuele machines (klassiek)** > *uw virtuele Windows-computer* > **Opnieuw externe...**. De volgende pagina wordt weergegeven.

![RDP configuratiepagina herstellen](./media/virtual-machines-windows-reset-rdp/Portal-RDP-Reset-Windows.png)

U kunt ook de naam en het wachtwoord van de lokale administrator-account opnieuw in te stellen. Klik op: **Bladeren** > **virtuele machines (klassiek)** > *uw virtuele Windows-computer* > **alle instellingen** > **wachtwoord opnieuw instellen**. De volgende pagina wordt weergegeven.

![Pagina-wachtwoord opnieuw instellen](./media/virtual-machines-windows-reset-rdp/Portal-PW-Reset-Windows.png)

Nadat u de nieuwe gebruikersnaam en wachtwoord hebt ingevoerd, klikt u op **Opslaan**.

### <a name="vmaccess-extension-and-powershell"></a>De extensie VMAccess en PowerShell

Zorg ervoor dat de VM-Agent is geïnstalleerd op de virtuele machine. De extensie VMAccess hoeft niet te worden geïnstalleerd voordat u het gebruiken kunt, zolang de VM-Agent beschikbaar is. Controleer of dat de Agent VM al is geïnstalleerd door de volgende opdracht. (Vervang 'myCloudService' en 'myVM' met de namen van de cloud-service en de VM, respectievelijk. Kunt u deze namen meer door het uitvoeren van `Get-AzureVM` zonder parameters.)

    $vm = Get-AzureVM -ServiceName "myCloudService" -Name "myVM"
    write-host $vm.VM.ProvisionGuestAgent

Als de opdracht **write-host** **True wordt**, wordt de VM-Agent wordt geïnstalleerd. Als **False**weergegeven, raadpleegt u de instructies en een link naar de download in de [VM-Agent en extensies - deel 2](http://go.microsoft.com/fwlink/p/?linkid=403947&clcid=0x409) Azure blogbericht.

Als u de virtuele machine hebt gemaakt met behulp van de portal, controleert u of `$vm.GetInstance().ProvisionGuestAgent` retourneert de **waarde True**. Als dat niet het geval is, kunt u deze eigenschap instellen met behulp van deze opdracht:

    $vm.GetInstance().ProvisionGuestAgent = $true

Deze opdracht zorgt ervoor dat het volgende foutbericht wanneer u de opdracht **Set-AzureVMExtension** in de volgende stappen uitvoert: "Bepaling Gast Agent moet zijn ingeschakeld op de VM-object voordat u de uitbreiding van de toegang tot de VM IaaS."

#### <a name="reset-the-local-administrator-account-password"></a>**Het wachtwoord van de lokale administrator-account opnieuw instellen**

Een referentie-in met de huidige naam van de lokale administrator-account en een nieuw wachtwoord maken en voer vervolgens de `Set-AzureVMAccessExtension` als volgt.

    $cred=Get-Credential
    Set-AzureVMAccessExtension –vm $vm -UserName $cred.GetNetworkCredential().Username `
        -Password $cred.GetNetworkCredential().Password  | Update-AzureVM

Als u een andere naam dan de huidige account, de extensie VMAccess wijzigt de naam van de lokale administrator-account, wordt het wachtwoord toegewezen aan de account, en problemen met een extern bureaublad afmelden. Als de lokale administrator-account is uitgeschakeld, kan de extensie VMAccess het.

Deze opdrachten worden ook de configuratie voor extern bureaublad-service opnieuw.

#### <a name="reset-the-remote-desktop-service-configuration"></a>**De configuratie voor extern bureaublad-service opnieuw instellen**

Als u de configuratie van de service Extern bureaublad herstellen, moet u de volgende opdracht uitvoeren:

    Set-AzureVMAccessExtension –vm $vm | Update-AzureVM

De extensie VMAccess twee opdrachten op de virtuele machine wordt uitgevoerd:

- `netsh advfirewall firewall set rule group="Remote Desktop" new enable=Yes`

Met deze opdracht kunt de ingebouwde groep van Windows Firewall waarmee binnenkomend verkeer voor extern bureaublad, die gebruikmaakt van TCP-poort 3389.

- `Set-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Terminal Server' -name "fDenyTSConnections" -Value 0`

Met deze opdracht wordt de fDenyTSConnections registerwaarde ingesteld op 0, het inschakelen van extern bureaublad-verbindingen.


## <a name="next-steps"></a>Volgende stappen

Als de extensie voor Azure VM access niet reageert en kan het wachtwoord opnieuw wilt instellen, kunt u [offline de lokale Windows wachtwoord opnieuw instellen](virtual-machines-windows-reset-local-password-without-agent.md). Deze methode is een meer geavanceerde proces en moet u de virtuele harde schijf van de problematische VM verbinding met een andere VM. Volg de stappen in dit artikel beschreven eerst en alleen de methode offline wachtwoord opnieuw instellen als laatste redmiddel te proberen.

[Functies en uitbreidingen voor Azure VM](virtual-machines-windows-extensions-features.md)

[Verbinding maken met een Azure virtuele machine met RDP of SSH](http://msdn.microsoft.com/library/azure/dn535788.aspx)

[Problemen met extern bureaublad-verbindingen met een Windows Azure virtual machine](virtual-machines-windows-troubleshoot-rdp-connection.md)
