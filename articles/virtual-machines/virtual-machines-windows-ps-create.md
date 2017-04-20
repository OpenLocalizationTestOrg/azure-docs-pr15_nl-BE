<properties
    pageTitle="Maak een Azure VM met PowerShell | Microsoft Azure"
    description="Azure PowerShell en Azure Resource Manager gebruiken om eenvoudig te maken een nieuwe VM met Windows Server."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/21/2016"
    ms.author="davidmu"/>

# <a name="create-a-windows-vm-using-resource-manager-and-powershell"></a>Maak een Windows VM met behulp van bronbeheer en PowerShell

In dit artikel wordt beschreven hoe u snel een Azure Virtual Machine met Windows Server en de bronnen die nodig zijn met behulp van [Bronbeheer](../azure-resource-manager/resource-group-overview.md) en PowerShell maken. 

De stappen in dit artikel zijn vereist voor het maken van een virtuele machine en het moet ongeveer 30 minuten voordat de stappen uitvoeren. Voorbeeld van de parameterwaarden in de opdrachten vervangen door namen die zinvol zijn voor uw omgeving.

## <a name="step-1-install-azure-powershell"></a>Stap 1: Installeer Azure PowerShell

[Het installeren en configureren van Azure PowerShell](../powershell-install-configure.md) Zie voor informatie over het installeren van de nieuwste versie van Azure PowerShell, uw abonnement selecteren en aanmelden bij uw account.
        
## <a name="step-2-create-a-resource-group"></a>Stap 2: Maak een resourcegroep

Alle resources moeten worden opgenomen in een groep, dus kunt maken dat eerst.  

1. Krijg een lijst met beschikbare locaties waar de resources kunnen worden gemaakt.

    ```powershell
    Get-AzureRmLocation | sort Location | Select Location
    ```

2. De locatie voor de resources instellen. Deze opdracht wordt de locatie ingesteld op **centralus**.

    ```powershell
    $location = "centralus"
    ```
    
3. Een resourcegroep maken. Met deze opdracht maakt de groep met de naam **myResourceGroup** op de locatie die u hebt ingesteld.

    ```powershell
    $myResourceGroup = "myResourceGroup"
    New-AzureRmResourceGroup -Name $myResourceGroup -Location $location
    ```
    
## <a name="step-3-create-a-storage-account"></a>Stap 3: Maak een account voor opslag

Een [account voor opslag](../storage/storage-introduction.md) is nodig voor het opslaan van de virtuele harde schijf die wordt gebruikt door de virtuele machine die u maakt. Opslag-accountnamen moeten tussen 3 en 24 tekens bevatten en mag alleen cijfers en kleine letters bevatten.

1. Test de accountnaam van de opslag voor uniekheid. Deze opdracht test de naam **myStorageAccount**.

    ```powershell
    $myStorageAccountName = "mystorageaccount"
    Get-AzureRmStorageAccountNameAvailability $myStorageAccountName
    ```
    
    Als deze opdracht **de waarde True**retourneert, is de voorgestelde naam uniek in Azure. 
    
2. Maak nu de opslag-account.
    
    ```powershell    
    $myStorageAccount = New-AzureRmStorageAccount -ResourceGroupName $myResourceGroup -Name $myStorageAccountName -SkuName "Standard_LRS" -Kind "Storage" -Location $location
    ```
    
## <a name="step-4-create-a-virtual-network"></a>Stap 4: Maak een virtueel netwerk

Alle virtuele machines maken deel uit van een [virtueel netwerk](../virtual-network/virtual-networks-overview.md).

1. Een subnet maken voor het virtuele netwerk. Met deze opdracht maakt een subnet genaamd **mySubnet** met adresvoorvoegsel 10.0.0.0/24.
        
    ```powershell
    $mySubnet = New-AzureRmVirtualNetworkSubnetConfig -Name "mySubnet" -AddressPrefix 10.0.0.0/24
    ```
    
2. Maak nu het virtuele netwerk. Met deze opdracht maakt een virtueel netwerk met de naam **myVnet** met behulp van het subnet dat u hebt gemaakt en het voorvoegsel van een adres van **10.0.0.0/16**.

    ```powershell
    $myVnet = New-AzureRmVirtualNetwork -Name "myVnet" -ResourceGroupName $myResourceGroup -Location $location -AddressPrefix 10.0.0.0/16 -Subnet $mySubnet
    ```
        
## <a name="step-5-create-a-public-ip-address-and-network-interface"></a>Stap 5: Maak een openbare IP-adres en netwerk interface

Om te kunnen communiceren met de virtuele machine in het virtuele netwerk, moet u een [openbaar IP-adres](../virtual-network/virtual-network-ip-addresses-overview-arm.md) en een netwerkinterface.

1. Het openbare IP-adres maken. Met deze opdracht maakt een openbaar IP-adres met de naam **myPublicIp** met de **dynamische**toewijzingsmethode.
 
    ```powershell
    $myPublicIp = New-AzureRmPublicIpAddress -Name "myPublicIp" -ResourceGroupName $myResourceGroup -Location $location -AllocationMethod Dynamic
    ```
        
2. Maak de netwerkinterface. Deze opdracht maakt u een netwerkinterface met de naam **myNIC**.

    ```powershell
    $myNIC = New-AzureRmNetworkInterface -Name "myNIC" -ResourceGroupName $myResourceGroup -Location $location -SubnetId $myVnet.Subnets[0].Id -PublicIpAddressId $myPublicIp.Id
    ```
       
## <a name="step-6-create-a-virtual-machine"></a>Stap 6: Een virtuele machine maken

Nu u alle stukken op zijn plaats hebt, is het tijd voor het maken van de virtuele machine.

1. Met deze opdracht stelt u de naam van de administrator-account en het wachtwoord voor de virtuele machine wordt uitgevoerd.

        $cred = Get-Credential -Message "Type the name and password of the local administrator account."
        
    Het wachtwoord moet op 123 van 12 tekens lang zijn en minimaal één kleine letter, teken met een hoofdletter, één nummer en één speciaal teken hebben. 
        
2. Maak het configuratieobject in voor de virtuele machine. Met deze opdracht maakt een configuratieobject met de naam **myVmConfig** waarin de naam van de VM en de grootte van de VM.

    ```powershell
    $myVm = New-AzureRmVMConfig -VMName "myVM" -VMSize "Standard_DS1_v2"
    ```
     
    Zie [voor virtuele machines in Azure](virtual-machines-windows-sizes.md) voor een lijst met beschikbare formaten voor een virtuele machine.
    
3. Configureren-instellingen voor de VM. Deze opdracht stelt de computernaam, het besturingssysteemtype en de referenties voor de VM.

    ```powershell
    $myVM = Set-AzureRmVMOperatingSystem -VM $myVM -Windows -ComputerName "myVM" -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    ```
    
4. Definieert de afbeelding met behulp van de VM inrichten. Met deze opdracht definieert de installatiekopie van Windows Server voor de VM. 

    ```powershell
    $myVM = Set-AzureRmVMSourceImage -VM $myVM -PublisherName "MicrosoftWindowsServer" -Offer "WindowsServer" -Skus "2012-R2-Datacenter" -Version "latest"
    ```
        
    Zie voor meer informatie over het selecteren van afbeeldingen te gebruiken, [navigeren en selecteer Windows virtuele machine afbeeldingen in Azure PowerShell of de CLI](virtual-machines-windows-cli-ps-findimage.md).
        
5. De netwerkinterface die u hebt gemaakt toevoegen aan de configuratie.

    ```powershell
    $myVM = Add-AzureRmVMNetworkInterface -VM $myVM -Id $myNIC.Id
    ```
        
6. De naam en locatie van de VM vaste schijf definiëren. De virtuele harde schijf-bestand wordt opgeslagen in een container. Met deze opdracht maakt de schijf in een container met de naam **vhds/WindowsVMosDisk.vhd** in de opslag-account die u hebt gemaakt.

    ```powershell
    $blobPath = "vhds/myOsDisk1.vhd"
    $osDiskUri = $myStorageAccount.PrimaryEndpoints.Blob.ToString() + $blobPath
    ```
        
7. De informatie over het besturingssysteem schijf toevoegen aan de VM-configuratie. De waarde van **$diskName** vervangen door een naam voor de schijf. De variabele maken en de gegevens van de schijf toevoegen aan de configuratie.
    
    ```powershell
    $vm = Set-AzureRmVMOSDisk -VM $myVM -Name "myOsDisk1" -VhdUri $osDiskUri -CreateOption fromImage
    ```
        
8. Tot slot maakt u de virtuele machine.

    ```
    New-AzureRmVM -ResourceGroupName $myResourceGroup -Location $location -VM $myVM
    ```
                                  
## <a name="next-steps"></a>Volgende stappen

- Als er problemen met de implementatie, zou in een volgende stap zijn om naar te kijken [Troubleshooting resource groep implementaties met Azure portal](../resource-manager-troubleshoot-deployments-portal.md)
- Informatie over het beheren van de virtuele machine die u hebt gemaakt aan de hand van [beheren virtuele machines met behulp van bronbeheer Azure en PowerShell](virtual-machines-windows-ps-manage.md).
- Profiteer van het gebruik van een sjabloon voor het maken van een virtuele machine met behulp van de gegevens in [een virtuele Windows-computer met een Resource Manager-sjabloon maken](virtual-machines-windows-ps-template.md)
