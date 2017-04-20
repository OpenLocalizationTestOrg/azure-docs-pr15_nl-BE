<properties
   pageTitle="Toegangsbeheerlijsten (ACL's) voor eindpunten via PowerShell beheren"
   description="Informatie over het beheren van ACL's met PowerShell"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/15/2016"
   ms.author="jdial" />

# <a name="how-to-manage-access-control-lists-acls-for-endpoints-by-using-powershell"></a>Toegangsbeheerlijsten (ACL's) voor eindpunten via PowerShell beheren

U kunt maken en beheren van Network Access Control Lists (ACL's) voor eindpunten met Azure PowerShell of in de Portal beheren. In dit onderwerp vindt u procedures voor veelvoorkomende taken ACL die u kunt uitvoeren met PowerShell. Zie voor een overzicht van Azure PowerShell cmdlets [Azure Management Cmdlets](http://go.microsoft.com/fwlink/?LinkId=317721). Zie voor meer informatie over toegangsbeheerlijsten [Wat is een Network Access Control-lijst (ACL)?](virtual-networks-acl.md). Als u uw ACL's beheren wilt met behulp van de Portal Management, Zie [van eindpunten instellen op een virtuele Machine](../virtual-machines/virtual-machines-windows-classic-setup-endpoints.md).

## <a name="manage-network-acls-by-using-azure-powershell"></a>ACL's netwerk met Azure PowerShell beheren

U kunt Azure PowerShell-cmdlets maken, verwijderen en configureren (set) Network Access Control Lists (ACL's). Enkele voorbeelden van manieren die kunt u een ACL met PowerShell hebt opgenomen.

Voor het ophalen van een volledige lijst van de ACL-PowerShell-cmdlets kunt u een van de volgende:

    Get-Help *AzureACL*
    Get-Command -Noun AzureACLConfig

### <a name="create-a-network-acl-with-rules-that-permit-access-from-a-remote-subnet"></a>De ACL van een netwerk met regels die toegang vanaf een extern subnet verlenen maken

In het volgende voorbeeld ziet u een manier voor het maken van een nieuwe ACL die regels bevat. Deze ACL wordt vervolgens toegepast op het eindpunt van een virtuele machine. De ACL-regels in het volgende voorbeeld kan toegang vanaf een extern subnet. Open een nieuw netwerk ACL maken met de voorschriften van de vergunning voor een extern subnet, een Azure PowerShell ISE. Kopiëren en plakken van het script onder het script configureren met uw eigen waarden en vervolgens het script uitvoeren.

1. Het nieuwe netwerk ACL-object maken.

        $acl1 = New-AzureAclConfig

1. Stel een regel waarmee de toegang van een extern subnet. In het volgende voorbeeld stelt u regel *100* (dit heeft voorrang op de regel 200 en hoger) naar het externe subnet *10.0.0.0/8* toegang geven tot het eindpunt van de virtuele machine. De waarden vervangen door uw eigen configuratievereisten. De naam 'SharePoint ACL config' moet worden vervangen door de beschrijvende naam die u wilt bellen, deze regel.

        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 100 `
            –Action permit –RemoteSubnet "10.0.0.0/8" `
            –Description "SharePoint ACL config"

1. Herhaal de cmdlet, de waarden te vervangen door uw eigen configuratievereisten voor aanvullende regels. Wijzig de regel nummer volgorde aangepast aan de volgorde waarin de regels worden toegepast. De onderste regelnummer heeft voorrang op een hoger nummer.

        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 200 `
            –Action permit –RemoteSubnet "157.0.0.0/8" `
            –Description "web frontend ACL config"

1. Vervolgens kunt u een nieuw eindpunt (toevoegen) maken of de ACL voor een bestaand eindpunt (Set) instellen. In dit voorbeeld wordt een nieuwe virtuele machine eindpunt naam 'web' toevoegen en bijwerken van het eindpunt van de virtuele machine met de ACL-instellingen.

        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Add-AzureEndpoint –Name "web" –Protocol tcp –Localport 80 - PublicPort 80 –ACL $acl1 `
        | Update-AzureVM

1. Vervolgens wordt de cmdlets combineren en het script wordt uitgevoerd. In dit voorbeeld eruit de gecombineerde cmdlets als volgt:

        $acl1 = New-AzureAclConfig
        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 100 `
            –Action permit –RemoteSubnet "10.0.0.0/8" `
            –Description "Sharepoint ACL config"
        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 200 `
            –Action permit –RemoteSubnet "157.0.0.0/8" `
            –Description "web frontend ACL config"
        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        |Add-AzureEndpoint –Name "web" –Protocol tcp –Localport 80 - PublicPort 80 –ACL $acl1 `
        |Update-AzureVM

### <a name="remove-a-network-acl-rule-that-permits-access-from-a-remote-subnet"></a>Een netwerk ACL regel waarmee de toegang van een extern subnet verwijderen

In het volgende voorbeeld ziet u een manier om een netwerk ACL regel verwijderen.  Als u een netwerk ACL regel met de voorschriften van de vergunning voor een extern subnet bevinden, opent u een Azure PowerShell ISE. Kopiëren en plakken van het script onder het script configureren met uw eigen waarden en vervolgens het script uitvoeren.

1. Eerste stap is het netwerk ACL-object ophalen voor het eindpunt van de virtuele machine. Vervolgens verwijdert u de ACL regel. In dit geval we het wilt verwijderen van regel-ID. Hiermee wordt de regel-ID 0 alleen verwijderd uit de ACL. Het verwijdert niet de ACL-object vanuit het eindpunt van de virtuele machine.

        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Get-AzureAclConfig –EndpointName "web" `
        | Set-AzureAclConfig –RemoveRule –ID 0 –ACL $acl1

1. Vervolgens moet u de ACL van de netwerk-object van toepassing op het eindpunt van de virtuele machine en bijwerken van de virtuele machine.

        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Set-AzureEndpoint –ACL $acl1 –Name "web" `
        | Update-AzureVM

### <a name="remove-a-network-acl-from-a-virtual-machine-endpoint"></a>De ACL van een netwerk van het eindpunt van een virtuele machine verwijderen

In bepaalde scenario's wilt u mogelijk een netwerk ACL-object verwijderen uit een eindpunt van de virtuele machine. Open hiervoor een Azure PowerShell ISE. Kopiëren en plakken van het script onder het script configureren met uw eigen waarden en vervolgens het script uitvoeren.

        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Remove-AzureAclConfig –EndpointName "web" `
        | Update-AzureVM

## <a name="next-steps"></a>Volgende stappen

[Wat is een Network Access Control-lijst (ACL)?](virtual-networks-acl.md)
