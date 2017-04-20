<properties
    pageTitle="Kan geen RDP voor een Azure VM | Microsoft Azure"
    description="Problemen wanneer u geen verbinding met de virtuele Windows-computer in Azure met extern bureaublad"
    keywords="Extern bureaublad fout, fout, verbinding met extern bureaublad, kan geen verbinding met de VM, probleemoplossing extern bureaublad"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="top-support-issue,azure-service-management,azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="support-article"
    ms.date="10/26/2016"
    ms.author="iainfou"/>

# <a name="troubleshoot-remote-desktop-connections-to-an-azure-virtual-machine"></a>Problemen met extern bureaublad-verbindingen met een Azure virtuele machine

De Remote Desktop Protocol (RDP) verbinding met de Windows Azure virtuele machine (VM) kan verschillende oorzaken hebben, zodat u geen toegang tot uw VM mislukken. Het probleem kan zijn met de service Extern bureaublad op de VM, de netwerkverbinding of de extern bureaublad-client op de hostcomputer. Dit artikel begeleidt u door enkele van de meest voorkomende methoden RDP-verbindingsproblemen op te lossen. 

Als u meer Help-informatie op elk punt in dit artikel, kunt u contact opnemen met de Azure experts op [de MSDN Azure en forums stackoverloop](https://azure.microsoft.com/support/forums/). U kunt ook een incident Azure ondersteuning indienen. Ga naar de [site ondersteuning van Azure](https://azure.microsoft.com/support/options/) en selecteer **Ondersteuning krijgen**.

<a id="quickfixrdp"></a>

## <a name="quick-troubleshooting-steps"></a>Snelle stappen voor probleemoplossing
Na elke stap opnieuw verbinding te maken voor de VM:

1. Opnieuw configureren van extern bureaublad.
2. Netwerkgroep beveiliging controleren regels / Cloud Services-eindpunten.
3. VM-console logboeken bekijken.
4. Controleer de status van de bron VM.
5. De VM-wachtwoord opnieuw instellen.
6. Start uw VM.
7. Implementeer uw VM.

Doorgaan met lezen als u meer gedetailleerde instructies en uitleg nodig.

> [AZURE.TIP] Als de knop **verbinding maken** voor uw VM is niet beschikbaar in de portal en u niet met Azure via een [Route Express](../expressroute/expressroute-introduction.md) of [Site naar Site VPN](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md) -verbinding verbonden bent, moet u voor het maken en uw VM een openbaar IP-adres toewijzen voordat u RDP kunt. U kunt meer lezen over het [openbare IP-adressen in Azure](../virtual-network/virtual-network-ip-addresses-overview-arm.md).


## <a name="ways-to-troubleshoot-rdp-issues"></a>Manieren om problemen met RDP
U kunt de VMs gemaakt met behulp van het implementatiemodel Resource Manager met behulp van een van de volgende manieren oplossen:

- [Azure portal](#using-the-azure-portal) - ideaal als u moet snel opnieuw de RDP- of de gebruiker referenties en u hoeft niet de Azure's hebben geïnstalleerd.
- [Azure PowerShell](#using-azure-powershell) - opnieuw als u vertrouwd met de PowerShell-prompt bent, snel de RDP of de gebruiker referenties met de Azure PowerShell-cmdlets.

Stappen voor het oplossen van VMs gemaakt met behulp van het [model Klassiek-implementatie](#troubleshoot-vms-created-using-the-classic-deployment-model)kunt u ook vinden.


<a id="fix-common-remote-desktop-errors"></a>
## <a name="troubleshoot-using-the-azure-portal"></a>Problemen oplossen met behulp van de portal Azure
Probeer na elke stap opnieuw verbinding te maken met uw VM. Als u nog steeds geen verbinding kunt maken, probeert u de volgende stap.

1. **De RDP-verbinding opnieuw ingesteld**. Deze stap wordt de RDP-configuratie opnieuw ingesteld als externe verbindingen zijn uitgeschakeld of Windows Firewall-regels RDP, bijvoorbeeld blokkeren.

    Selecteer uw VM in Azure portal. Blader in het deelvenster instellingen naar de sectie **Support + problemen** aan de onderkant van de lijst. Klik op **wachtwoord opnieuw instellen** . De **modus** **alleen configuratie opnieuw** wilt instellen en klik vervolgens op de knop **bijwerken** :

    ![De RDP-configuratie in de portal Azure opnieuw instellen](./media/virtual-machines-windows-troubleshoot-rdp-connection/reset-rdp.png)

2. **Controleer of Network Security Group regels**. Deze stap wordt gecontroleerd of u hebt een regel in uw netwerk beveiligingsgroep voor RDP-verkeer toestaan. De standaardpoort voor RDP is TCP-poort 3389. Een regel voor het toestaan van RDP-verkeer kan niet automatisch worden gemaakt bij het maken van uw VM.

    Selecteer uw VM in Azure portal. Klik op de **Netwerkinterfaces** van het venster instellingen.

    ![Netwerkinterfaces weergeven voor een VM in Azure portal](./media/virtual-machines-windows-troubleshoot-rdp-connection/select-network-interfaces.png)

    Selecteer de netwerkinterface in de lijst (Er is meestal slechts één):

    ![Selecteer netwerkinterface in Azure portal](./media/virtual-machines-windows-troubleshoot-rdp-connection/select-interface.png)

    Selecteer **groep netwerk** om de netwerk-beveiligingsgroep die is gekoppeld aan uw netwerk-interface weer te geven:

    ![Selecteer Network Security Group in Azure portal](./media/virtual-machines-windows-troubleshoot-rdp-connection/select-nsg.png)

    Controleer of een binnenkomende regel waarmee RDP-verkeer op TCP-poort 3389. In het volgende voorbeeld ziet u een geldige regel die RDP-verkeer is toegestaan. U kunt zien `Service` en `Action` correct zijn geconfigureerd:

    ![Controleer of regel in de portal Azure RDP NSG](./media/virtual-machines-windows-troubleshoot-rdp-connection/verify-nsg-rules.png)

    Als er een regel waarmee het RDP-verkeer, [een regel netwerk beveiligingsgroep maken](virtual-machines-windows-nsg-quickstart-portal.md). TCP-poort 3389 toestaan.

3. **Revisie VM opstarten diagnostische gegevens**. Deze stap controleert de VM console Logboeken om te bepalen als de VM van een probleem melden. Niet alle VMs hebben boot diagnostics is ingeschakeld, zodat deze stap is niet verplicht.
    
    Specifieke stappen voor probleemoplossing zijn buiten het bestek van dit artikel, maar kunnen duiden op een groter probleem dat gevolgen voor RDP-verbindingen heeft. Zie voor meer informatie over het weergeven van de console logs and VM screenshot [Boot Diagnostics voor VMs](https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/).

4. **Controleer de status van de bron VM**. Deze stap wordt gecontroleerd of er zijn geen bekende problemen met het Azure platform die mogelijk van invloed op de VM-verbinding.

    Selecteer uw VM in Azure portal. Blader in het deelvenster instellingen naar de sectie **Support + problemen** aan de onderkant van de lijst. Klik op **de gezondheid van de Resource** . Een gezonde VM rapporten **beschikbaar**als:

    ![Resource-gezondheid VM in Azure portal controleren](./media/virtual-machines-windows-troubleshoot-rdp-connection/check-resource-health.png)

5. De **referenties van de gebruiker opnieuw instellen**. Deze stap wordt het wachtwoord voor een lokale administrator-account als u niet zeker weet of de referenties bent vergeten.

    Selecteer uw VM in Azure portal. Blader in het deelvenster instellingen naar de sectie **Support + problemen** aan de onderkant van de lijst. Klik op **wachtwoord opnieuw instellen** . Zorg ervoor dat de **modus** is ingesteld op **wachtwoord opnieuw instellen** en voer uw gebruikersnaam en een nieuw wachtwoord. Ten slotte klikt u op de knop **bijwerken** :

    ![De referenties van de gebruiker in de portal Azure opnieuw instellen](./media/virtual-machines-windows-troubleshoot-rdp-connection/reset-password.png)

6. **Start uw VM**. Deze stap kan de VM zelf heeft met het onderliggende problemen te corrigeren.

    Selecteer uw VM in Azure portal en klik op het tabblad **Overzicht** . Klik op de knop **opnieuw opstarten** :

    ![Start de VM in Azure portal](./media/virtual-machines-windows-troubleshoot-rdp-connection/restart-vm.png)

7. **Implementeer uw VM**. Deze stap redeploys de VM aan een andere host in Azure elk onderliggende platform of netwerkproblemen.

    Selecteer uw VM in Azure portal. Blader in het deelvenster instellingen naar de sectie **Support + problemen** aan de onderkant van de lijst. Klik op de knop **opnieuw** en klik op **opnieuw implementeren**:

    ![Implementeer de VM in Azure portal](./media/virtual-machines-windows-troubleshoot-rdp-connection/redeploy-vm.png)

    Wanneer deze bewerking is voltooid, tijdelijke schijfgegevens verloren zijn gegaan en dynamische IP-adressen die gekoppeld aan de VM zijn worden bijgewerkt.

Als u nog steeds problemen met RDP ondervindt, u [opent een ondersteuningsverzoek](https://azure.microsoft.com/support/options/) of Lees [meer gedetailleerde RDP probleemoplossing concepten en werk](virtual-machines-windows-detailed-troubleshoot-rdp.md).


## <a name="troubleshoot-using-azure-powershell"></a>Problemen oplossen met behulp van Azure PowerShell
Als u nog niet gedaan, [installeert en configureert u de meest recente Azure PowerShell hebt](../powershell-install-configure.md).

De volgende voorbeelden gebruiken de variabelen, zoals `myResourceGroup`, `myVM`, en `myVMAccessExtension`. Deze variabele namen en locaties vervangen door uw eigen waarden.

> [AZURE.NOTE] U herstellen referenties van de gebruiker en de RDP-configuratie met behulp van de PowerShell-cmdlet [Set-AzureRmVMAccessExtension](https://msdn.microsoft.com/library/mt619447.aspx) . In de volgende voorbeelden `myVMAccessExtension` is een naam die u als onderdeel van het proces opgeeft. Als u de VMAccessAgent eerder hebt gewerkt, kunt u de naam van de bestaande extensie krijgen met behulp van `Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVM"` om de eigenschappen van de VM. Als u de naam wilt, zien onder de sectie 'Extensions' van de uitvoer.

Probeer na elke stap opnieuw verbinding te maken met uw VM. Als u nog steeds geen verbinding kunt maken, probeert u de volgende stap.

1. **De RDP-verbinding opnieuw ingesteld**. Deze stap wordt de RDP-configuratie opnieuw ingesteld als externe verbindingen zijn uitgeschakeld of Windows Firewall-regels RDP, bijvoorbeeld blokkeren.

    In het volgende voorbeeld wordt de RDP-verbinding op een VM met de naam `myVM` in de `WestUS` locatie en in de bronnengroep met de naam `myResourceGroup`:

    ```powershell
    Set-AzureRmVMAccessExtension -ResourceGroupName "myResourceGroup" `
        -VMName "myVM" -Location Westus -Name "myVMAccessExtension"
    ```

2. **Controleer of Network Security Group regels**. Deze stap wordt gecontroleerd of u hebt een regel in uw netwerk beveiligingsgroep voor RDP-verkeer toestaan. De standaardpoort voor RDP is TCP-poort 3389. Een regel voor het toestaan van RDP-verkeer kan niet automatisch worden gemaakt bij het maken van uw VM.

    Toewijzen eerst alle configuratiegegevens voor uw netwerk beveiligingsgroep voor het `$rules` variabele. In het volgende voorbeeld wordt informatie over de netwerk-beveiligingsgroep met de naam `myNetworkSecurityGroup` in de bronnengroep met de naam `myResourceGroup`:

    ```powershell
    $rules = Get-AzureRmNetworkSecurityGroup -ResourceGroupName "myResourceGroup" `
        -Name "myNetworkSecurityGroup"
    ```

    Bekijk nu de regels die zijn geconfigureerd voor deze groep netwerk. Controleren of er een regel bestaat als u wilt toestaan dat TCP-poort 3389 voor binnenkomende verbindingen als volgt:

    ```powershell
    $rules.SecurityRules
    ```

    In het volgende voorbeeld ziet u een geldige regel die RDP-verkeer is toegestaan. U kunt zien `Protocol`, `DestinationPortRange`, `Access`, en `Direction` correct zijn geconfigureerd:

    ```powershell
    Name                     : default-allow-rdp
    Id                       : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/securityRules/default-allow-rdp
    Etag                     : 
    ProvisioningState        : Succeeded
    Description              : 
    Protocol                 : TCP
    SourcePortRange          : *
    DestinationPortRange     : 3389
    SourceAddressPrefix      : *
    DestinationAddressPrefix : *
    Access                   : Allow
    Priority                 : 1000
    Direction                : Inbound
    ```

    Als er een regel waarmee het RDP-verkeer, [een regel netwerk beveiligingsgroep maken](virtual-machines-windows-nsg-quickstart-powershell.md). TCP-poort 3389 toestaan.

3. De **referenties van de gebruiker opnieuw instellen**. Deze stap herstelt het wachtwoord voor de lokale administrator-account die u opgeeft wanneer u niet zeker van bent of bent vergeten, de referenties.

    Eerst geeft u de gebruikersnaam en een nieuw wachtwoord door het toewijzen van referenties naar de `$cred` variabele als volgt:

    ```powershell
    $cred=Get-Credential
    ```

    Nu de referenties op uw VM bijwerken. In het volgende voorbeeld werkt u de referenties op een VM met de naam `myVM` in de `WestUS` locatie en in de bronnengroep met de naam `myResourceGroup`:

    ```powershell
    Set-AzureRmVMAccessExtension -ResourceGroupName "myResourceGroup" `
        -VMName "myVM" -Location WestUS -Name "myVMAccessExtension" `
        -UserName $cred.GetNetworkCredential().Username `
        -Password $cred.GetNetworkCredential().Password
    ```

4. **Start uw VM**. Deze stap kan de VM zelf heeft met het onderliggende problemen te corrigeren.

    In het volgende voorbeeld wordt opnieuw gestart met de naam VM `myVM` in de bronnengroep met de naam `myResourceGroup`:

    ```powershell
    Restart-AzureRmVM -ResourceGroup "myResourceGroup" -Name "myVM"
    ```

5. **Implementeer uw VM**. Deze stap redeploys de VM aan een andere host in Azure elk onderliggende platform of netwerkproblemen.

    In het volgende voorbeeld de VM met de naam redeploys `myVM` in de `WestUS` locatie en in de bronnengroep met de naam `myResourceGroup`:

    ```powershell
    Set-AzureRmVM -Redeploy -ResourceGroupName "myResourceGroup" -Name "myVM"
    ```

Als u nog steeds problemen met RDP ondervindt, u [opent een ondersteuningsverzoek](https://azure.microsoft.com/support/options/) of Lees [meer gedetailleerde RDP probleemoplossing concepten en werk](virtual-machines-windows-detailed-troubleshoot-rdp.md).


## <a name="troubleshoot-vms-created-using-the-classic-deployment-model"></a>Gemaakt met behulp van het model Klassiek implementatie VMs oplossen

Probeer na elke stap opnieuw verbinding maken met de VM.

1. **De RDP-verbinding opnieuw ingesteld**. Deze stap wordt de RDP-configuratie opnieuw ingesteld als externe verbindingen zijn uitgeschakeld of Windows Firewall-regels RDP, bijvoorbeeld blokkeren.

    Selecteer uw VM in Azure portal. Klik op de **... Meer** klik vervolgens op **Externe toegang opnieuw instellen**:

    ![De RDP-configuratie in de portal Azure opnieuw instellen](./media/virtual-machines-windows-troubleshoot-rdp-connection/classic-reset-rdp.png)

2.  **Cloud-Services controleren of eindpunten**. Deze stap wordt gecontroleerd of het hebben van eindpunten in de Cloud-Services om RDP-verkeer mogelijk te maken. De standaardpoort voor RDP is TCP-poort 3389. Een regel voor het toestaan van RDP-verkeer kan niet automatisch worden gemaakt bij het maken van uw VM.

    Selecteer uw VM in Azure portal. Klik op **eindpunten** als u wilt weergeven van de eindpunten die momenteel zijn geconfigureerd voor uw VM. Controleer of eindpunten bestaan die RDP-verkeer op TCP-poort 3389 toestaan.
    
    In het volgende voorbeeld ziet u geldige eindpunten die RDP-verkeer toestaan:

    ![Controleer of de eindpunten in de portal Azure Cloud Services](./media/virtual-machines-windows-troubleshoot-rdp-connection/classic-verify-cloud-services-endpoints.png)

    Als u niet een eindpunt waarmee het RDP-verkeer, [een eindpunt Cloud Services maken](virtual-machines-windows-classic-setup-endpoints.md). TCP private poort 3389 toestaan.

3. **Revisie VM opstarten diagnostische gegevens**. Deze stap controleert de VM console Logboeken om te bepalen als de VM van een probleem melden. Niet alle VMs hebben boot diagnostics is ingeschakeld, zodat deze stap is niet verplicht.
    
    Specifieke stappen voor probleemoplossing zijn buiten het bestek van dit artikel, maar kunnen duiden op een groter probleem dat gevolgen voor RDP-verbindingen heeft. Zie voor meer informatie over het weergeven van de console logs and VM screenshot [Boot Diagnostics voor VMs](https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/).

4. **Controleer de status van de bron VM**. Deze stap wordt gecontroleerd of er zijn geen bekende problemen met het Azure platform die mogelijk van invloed op de VM-verbinding.

    Selecteer uw VM in Azure portal. Blader in het deelvenster instellingen naar de sectie **Support + problemen** aan de onderkant van de lijst. Klik op **De gezondheid van de Resource** . Een gezonde VM rapporten **beschikbaar**als:

    ![Resource-gezondheid VM in Azure portal controleren](./media/virtual-machines-windows-troubleshoot-rdp-connection/classic-check-resource-health.png)

5. De **referenties van de gebruiker opnieuw instellen**. Deze stap herstelt het wachtwoord voor de lokale administrator-account die u opgeeft wanneer u niet zeker weet of de referenties bent vergeten.

    Selecteer uw VM in Azure portal. Blader in het deelvenster instellingen naar de sectie **Support + problemen** aan de onderkant van de lijst. Klik op **wachtwoord opnieuw instellen** . Voer uw gebruikersnaam en een nieuw wachtwoord. Ten slotte klikt u op de knop **Opslaan** :

    ![De referenties van de gebruiker in de portal Azure opnieuw instellen](./media/virtual-machines-windows-troubleshoot-rdp-connection/classic-reset-password.png)

6. **Start uw VM**. Deze stap kan de VM zelf heeft met het onderliggende problemen te corrigeren.

    Selecteer uw VM in Azure portal en klik op het tabblad **Overzicht** . Klik op de knop **opnieuw opstarten** :

    ![Start de VM in Azure portal](./media/virtual-machines-windows-troubleshoot-rdp-connection/classic-restart-vm.png)
    
Als u nog steeds problemen met RDP ondervindt, u [opent een ondersteuningsverzoek](https://azure.microsoft.com/support/options/) of Lees [meer gedetailleerde RDP probleemoplossing concepten en werk](virtual-machines-windows-detailed-troubleshoot-rdp.md).


## <a name="troubleshoot-specific-rdp-errors"></a>Met specifieke RDP-fouten oplossen
Een specifiek foutbericht kunnen optreden bij het verbinding maken met uw VM via RDP. Dit zijn de meest voorkomende foutberichten worden weergegeven:

- [De externe sessie is beëindigd omdat er geen extern bureaublad-licentieservers beschikbaar een licentie kunnen verlenen](virtual-machines-windows-troubleshoot-specific-rdp-errors.md#rdplicense).
- [Extern bureaublad kan de computer 'naam' niet vinden](virtual-machines-windows-troubleshoot-specific-rdp-errors.md#rdpname).
- [Een verificatie fout. De Local Security Authority kan geen contact maken met](virtual-machines-windows-troubleshoot-specific-rdp-errors.md#rdpauth).
- [Windows-beveiliging-fout: uw referenties werken niet](virtual-machines-windows-troubleshoot-specific-rdp-errors.md#wincred).
- [Deze computer kan geen verbinding maken met de externe computer](virtual-machines-windows-troubleshoot-specific-rdp-errors.md#rdpconnect).


## <a name="additional-resources"></a>Aanvullende bronnen
Als geen van deze fouten is opgetreden en u nog steeds geen verbinding met de VM via Extern bureaublad, lees de gedetailleerde [gids voor extern bureaublad oplossen](virtual-machines-windows-detailed-troubleshoot-rdp.md).

- [Azure diagnostics-pakket IaaS (Windows)](https://home.diagnostics.support.microsoft.com/SelfHelp?knowledgebaseArticleFilter=2976864)
- Zie [problemen met toegang tot een toepassing wordt uitgevoerd op een Azure VM](virtual-machines-linux-troubleshoot-app-connection.md)voor probleemoplossing bij het openen van toepassingen op een VM.
- Als u problemen met behulp van Secure Shell (SSH ondervindt) verbinding maken met een Linux VM in Azure, Zie [problemen met SSH-verbindingen naar een Linux VM in Azure](virtual-machines-linux-troubleshoot-ssh-connection.md).
