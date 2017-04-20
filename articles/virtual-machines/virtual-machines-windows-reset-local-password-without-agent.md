<properties
   pageTitle="Een lokale Windows-wachtwoord opnieuw instellen als gast Azure agent is niet geïnstalleerd | Microsoft Azure"
   description="Het wachtwoord van een lokale gebruikersaccount opnieuw instellen wanneer de agent Azure Gast niet geïnstalleerd of functioneert op een VM is"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="10/05/2016"
   ms.author="iainfou"/>

# <a name="how-to-reset-local-windows-password-for-azure-vm"></a>Het lokale Windows-wachtwoord opnieuw instellen voor Azure VM
De lokale Windows-wachtwoord van een VM in Azure met behulp van de [Azure portal of Azure PowerShell](virtual-machines-windows-reset-rdp.md) dat de Azure Gast agent is geïnstalleerd, kunt u herstellen. Deze methode is de eenvoudigste manier om uw wachtwoord opnieuw instellen voor een Azure VM. Als u problemen ondervindt met de Gast Azure-agent niet meer reageert of niet kon worden geïnstalleerd na het uploaden van een aangepaste afbeelding, u handmatig kunt uw Windows-wachtwoord opnieuw instellen. Dit artikel wordt beschreven hoe u een lokale wachtwoord opnieuw instellen door de bron OS virtuele schijf koppelen aan een andere VM. 

> [AZURE.WARNING] Gebruik deze methode alleen als laatste redmiddel. Probeer altijd eerst de [Azure portal of Azure PowerShell](virtual-machines-windows-reset-rdp.md) met wachtwoord opnieuw instellen.


## <a name="overview-of-the-process"></a>Overzicht van het proces
De core stappen voor het uitvoeren van een lokale wachtwoord opnieuw instellen voor een Windows VM in Azure, als er geen toegang tot de Azure Gast agent is als volgt:

- De VM-bron verwijderen. De virtuele schijven blijven behouden.
- De bron-VM OS schijf koppelen aan een andere VM in Azure abonnement. Deze VM VM probleemoplossing genoemd.
- Maak met de probleemoplossing VM configuratiebestanden op de bron-VM OS schijf.
- Loskoppelen van de VM OS schijf van de VM voor het oplossen van problemen.
- Een sjabloon Resource Manager gebruiken voor het maken van een VM, met behulp van de oorspronkelijke virtuele schijf.
- Als de nieuwe VM wordt opgestart, bijwerken configuratiebestanden die u maakt het wachtwoord van de gebruiker vereist.


## <a name="detailed-steps"></a>Gedetailleerde stappen
Probeer altijd opnieuw instellen van een wachtwoord met de [Azure portal of Azure PowerShell](virtual-machines-windows-reset-rdp.md) voordat u de volgende stappen uit. Zorg ervoor dat u een back-up van uw VM hebt voordat u begint. 

1. De betrokken VM in Azure portal verwijderen. De VM verwijderen verwijdert u alleen de metagegevens, de referentie van de VM in Azure. De virtuele schijven blijven behouden wanneer de VM wordt verwijderd:

    - Selecteer de VM in Azure portal, klik op *verwijderen*:

    ![Verwijder bestaande VM](./media/virtual-machines-windows-reset-local-password-without-guest-agent/delete_vm.png)

2. De bron-VM OS schijf koppelen aan de VM voor het oplossen van problemen. De probleemoplossing VM moet zich in hetzelfde gebied, als de bron-VM OS schijf (zoals `West US`):

    - Selecteer de probleemoplossing VM in Azure portal. Klik op *schijven* | *bestaande bijvoegen*:

    ![Bestaande schijf koppelen](./media/virtual-machines-windows-reset-local-password-without-guest-agent/disks_attach_existing.png)

    *VHD-bestand* selecteren en selecteert u de account van de opslag die de bron VM bevat:

    ![Selecteer de account opslag](./media/virtual-machines-windows-reset-local-password-without-guest-agent/disks_select_storageaccount.PNG)

    Selecteer de Broncontainer. De Broncontainer wordt meestal *VHD's*:

    ![Opslag container selecteren](./media/virtual-machines-windows-reset-local-password-without-guest-agent/disks_select_container.png)

    Selecteer de vhd OS te koppelen. Klik op *selecteren* om het proces te voltooien:

    ![Selecteer de virtuele bronschijf](./media/virtual-machines-windows-reset-local-password-without-guest-agent/disks_select_source_vhd.png)

3. Verbinding met extern bureaublad oplossen VM en controleer dat de bron-VM OS schijf zichtbaar is:

    - Selecteer de probleemoplossing VM in Azure portal en klik op *verbinding maken*.
    - Open het RDP-bestand dat wordt gedownload. Voer de gebruikersnaam en het wachtwoord van de VM voor het oplossen van problemen.
    - Zoek in File Explorer, naar de data schijven. Als de bron van de VM VHD de schijf alleen gegevens is gekoppeld aan de VM voor het oplossen van problemen is, moet het station F::

    ![Gekoppelde gegevensschijf bekijken](./media/virtual-machines-windows-reset-local-password-without-guest-agent/troubleshooting_vm_fileexplorer.png)

4. Maak `gpt.ini` in `\Windows\System32\GroupPolicy` op de bron-VM station (als gpt.ini bestaat, wijzig in gpt.ini.bak):

    > [AZURE.WARNING] Zorg ervoor dat u niet per ongeluk de volgende bestanden in C:\Windows, het station OS voor probleemoplossing VM maakt. De volgende bestanden in het station OS voor uw bron VM die is gekoppeld als een gegevensschijf maken.

    - Voeg de volgende regels in de `gpt.ini` dat u hebt gemaakt:

    ```
    [General]
    gPCFunctionalityVersion=2
    gPCMachineExtensionNames=[{42B5FAAE-6536-11D2-AE5A-0000F87571E3}{40B6664F-4972-11D1-A7CA-0000F87571E3}]
    Version=1
    ```

    ![Gpt.ini maken](./media/virtual-machines-windows-reset-local-password-without-guest-agent/create_gpt_ini.png)
 
5. Maak `scripts.ini` in `\Windows\System32\GroupPolicy\Machine\Scripts`. Zorg ervoor dat verborgen mappen worden weergegeven. Maak indien nodig de `Machine` of `Scripts` mappen.

    - Voeg de volgende regels de `scripts.ini` dat u hebt gemaakt:

    ```
    [Startup]
    0CmdLine=C:\Windows\System32\FixAzureVM.cmd
    0Parameters=
    ```

    ![Scripts.ini maken](./media/virtual-machines-windows-reset-local-password-without-guest-agent/create_scripts_ini.png)
 
6. Maak `FixAzureVM.cmd` in `\Windows\System32` met de volgende inhoud, vervangen, `<username>` en `<newpassword>` met uw eigen waarden:

    ```
    NET USER <username> <newpassword>
    ```

    ![FixAzureVM.cmd maken](./media/virtual-machines-windows-reset-local-password-without-guest-agent/create_fixazure_cmd.png)

    U moet voldoen aan de complexiteitsvereisten van het ingestelde wachtwoord voor uw VM bij het definiëren van het nieuwe wachtwoord.

7. Ontkoppel de schijf van de probleemoplossing VM in Azure portal:

    - Selecteer de probleemoplossing VM in Azure portal, klikt u op *schijven*.
    - Selecteer de gegevensschijf is toegevoegd in stap 2, klikt u op *ontkoppelen*:

    ![Schijf ontkoppelen](./media/virtual-machines-windows-reset-local-password-without-guest-agent/detach_disk.png)

8. Voordat u een VM maken, krijgen de URI naar de bron OS schijf:

    - Selecteer de rekening opslag in de portal Azure, klikt u op de *BLOB's*.
    - Selecteer de container. De Broncontainer wordt meestal *VHD's*:

    ![Selecteer de account-blob storage](./media/virtual-machines-windows-reset-local-password-without-guest-agent/select_storage_details.png)

    Uw VM OS VHD-bron selecteren en klik op de knop *kopiëren* naast de naam van de *URL* :

    ![URI-schijf kopiëren](./media/virtual-machines-windows-reset-local-password-without-guest-agent/copy_source_vhd_uri.png)

9. Maak een VM vanaf de bron-VM OS schijf:

    - Gebruik [deze sjabloon Azure Resource Manager](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd) een VM maken van een gespecialiseerde VHD. Klik op de `Deploy to Azure` knop voor het openen van de Azure portal met de details van sjablonen voor u gevuld.
    - Als u bewaren van de vorige instellingen voor VM wilt, selecteert u de *sjabloon bewerken* om uw bestaande VNet, subnet, netwerkadapter of openbare IP.
    - In de `OSDISKVHDURI` parameter tekstvak, de URI van de VHD-bron verkrijgen in de vorige stap plakken:

    ![Een VM maken via sjabloon](./media/virtual-machines-windows-reset-local-password-without-guest-agent/create_new_vm_from_template.png)

10. Nadat de nieuwe VM wordt uitgevoerd, verbinding maken met de VM met extern bureaublad met het nieuwe wachtwoord dat u hebt opgegeven in de `FixAzureVM.cmd` script.

11. Verwijder de volgende bestanden voor het opschonen van de omgeving van de externe sessie voor de nieuwe VM:

    - Van %windir%\System32
        - FixAzureVM.cmd verwijderen
    - Van %windir%\System32\GroupPolicy\Machine\
        - scripts.ini verwijderen
    - Van %windir%\System32\GroupPolicy
        - gpt.ini verwijderen (als gpt.ini sprake was en u deze hebt gewijzigd in gpt.ini.bak, het .bak-bestand terug naar gpt.ini hernoemen)

## <a name="next-steps"></a>Volgende stappen
Als u nog steeds geen verbinding met extern bureaublad, Zie de [gids voor probleemoplossing van RDP](virtual-machines-windows-troubleshoot-rdp-connection.md). De [gedetailleerde RDP probleemoplossingsgids](virtual-machines-windows-detailed-troubleshoot-rdp.md) gezocht bij het oplossen van methoden in plaats van specifieke stappen. U kunt ook [een ondersteuningsverzoek Azure openen](https://azure.microsoft.com/support/options/) voor praktische hulp.