<properties 
    pageTitle="Testomgeving voor LOB-toepassing | Microsoft Azure" 
    description="Informatie over het maken van een web-gebaseerd, aantal zakelijke toepassingen in een omgeving met hybride cloud voor IT pro of ontwikkeling testen." 
    services="virtual-machines-windows" 
    documentationCenter="" 
    authors="JoeDavies-MSFT" 
    manager="timlt" 
    editor=""
    tags="azure-resource-manager"/>

<tags 
    ms.service="virtual-machines-windows" 
    ms.workload="infrastructure-services" 
    ms.tgt_pltfrm="vm-windows" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/30/2016" 
    ms.author="josephd"/>

# <a name="set-up-a-web-based-lob-application-in-a-hybrid-cloud-for-testing"></a>Een LOB webtoepassing instellen in een hybride wolk voor het testen

In dit onderwerp begeleidt u bij het maken van een gesimuleerde hybride cloud omgeving voor het testen van een regel op het web business (LOB)-toepassing die wordt gehost in Microsoft Azure. Hier is de resulterende configuratie.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph3.png)

Deze configuratie bestaat uit:

- Een gesimuleerde op ruimten netwerk ondergebracht in Azure (het TestLab VNet).
- Een cross-premises virtueel netwerk ondergebracht in Azure (TestVNET).
- Een VNet-VNet-VPN-verbinding.
- Een web-based LOB server, SQL server en secundaire domeincontroller in het virtuele netwerk TestVNET.

Deze configuratie biedt een basis en een gemeenschappelijk startpunt van waaruit u kunt:

- Ontwikkelen en testen van LOB-toepassingen die worden gehost op Internet Information Services (IIS) met een SQL Server 2014 databaseback-end in Azure.
- Testen van gesimuleerde hybride cloud-gebaseerde IT de werklast uit te voeren.

Er zijn drie belangrijke fasen om deze hybride cloud-testomgeving in te stellen:

1.  De gesimuleerde hybride cloud-omgeving instellen.
2.  Configureer de SQL server-computer (SQL1).
3.  Configureer de LOB-server (LOB1).

De werklast is een Azure-abonnement nodig. Als u een abonnement op MSDN of Visual Studio, Zie [maandelijkse Azure krediet voor abonnees van Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).

Zie voor een voorbeeld van een LOB-toepassing die wordt gehost in Azure-productie, de blauwdruk **bedrijfstoepassingen** -architectuur bij [Microsoft Software architectuur's en blauwdrukken](http://msdn.microsoft.com/dn630664).

## <a name="phase-1-set-up-the-simulated-hybrid-cloud-environment"></a>Fase 1: De gesimuleerde hybride cloud-omgeving instellen

Maak de [hybride cloud testomgeving gesimuleerd](virtual-machines-windows-ps-hybrid-cloud-test-env-sim.md). Omdat de aanwezigheid van de server 1 in subnet Corpnet niet nodig is deze testomgeving, kunt u deze afsluiten nu.

Dit is uw huidige configuratie.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph1.png)
 
## <a name="phase-2-configure-the-sql-server-computer-sql1"></a>Fase 2: De computer configureren voor SQL server (SQL1)

Vanaf de portal Azure, start u de computer DC2 als dat nodig is.

Maak vervolgens een virtuele machine voor SQL1 met de volgende opdrachten bij een opdrachtprompt Azure PowerShell op uw lokale computer. Voordat u deze opdrachten uitvoert, vult u de waarden van variabelen en verwijder de < en > tekens.

    $rgName="<your resource group name>"
    $locName="<the Azure location of your resource group>"
    $saName="<your storage account name>"
    
    $vnet=Get-AzureRMVirtualNetwork -Name "TestVNET" -ResourceGroupName $rgName
    $subnet=Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name "TestSubnet"
    $pip=New-AzureRMPublicIpAddress -Name SQL1-NIC -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
    $nic=New-AzureRMNetworkInterface -Name SQL1-NIC -ResourceGroupName $rgName -Location $locName -Subnet $subnet -PublicIpAddress $pip
    $vm=New-AzureRMVMConfig -VMName SQL1 -VMSize Standard_A4
    $storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
    $vhdURI=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/SQL1-SQLDataDisk.vhd"
    Add-AzureRMVMDataDisk -VM $vm -Name "Data" -DiskSizeInGB 100 -VhdUri $vhdURI  -CreateOption empty
    
    $cred=Get-Credential -Message "Type the name and password of the local administrator account for the SQL Server computer." 
    $vm=Set-AzureRMVMOperatingSystem -VM $vm -Windows -ComputerName SQL1 -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm=Set-AzureRMVMSourceImage -VM $vm -PublisherName MicrosoftSQLServer -Offer SQL2014-WS2012R2 -Skus Standard -Version "latest"
    $vm=Add-AzureRMVMNetworkInterface -VM $vm -Id $nic.Id
    $storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
    $osDiskUri=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/SQL1-OSDisk.vhd"
    $vm=Set-AzureRMVMOSDisk -VM $vm -Name "OSDisk" -VhdUri $osDiskUri -CreateOption fromImage
    New-AzureRMVM -ResourceGroupName $rgName -Location $locName -VM $vm

De Azure portal gebruiken om te verbinden met SQL1 met de lokale administrator-account van SQL1.

Vervolgens configureert u Windows Firewall-regels als u wilt dat SQL Server verkeer en eenvoudige connectiviteit testen. Uit de beheerdersniveau Windows PowerShell-prompt op SQL1, deze opdrachten worden uitgevoerd.

    New-NetFirewallRule -DisplayName "SQL Server" -Direction Inbound -Protocol TCP -LocalPort 1433,1434,5022 -Action allow 
    Set-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" -enabled True
    ping dc2.corp.contoso.com

De opdracht ping moet leiden tot vier geslaagde antwoorden van IP-adres 192.168.0.4.

Voeg vervolgens de schijf met extra gegevens op SQL1 in een nieuw volume met de stationsletter F:.

1.  Klik op **Bestands- en opslagservices**in het linkerdeelvenster van Serverbeheer en klik vervolgens op **schijven**.
2.  In het deelvenster inhoud in de groep **schijven** , klikt u op **schijf 2** (met de **partitie** ingesteld op **Onbekend**).
3.  Klik op **taken**en klik vervolgens op **NieuwVolume**.
4.  Op de Before begin pagina van de Wizard Nieuw Volume en klik op **volgende**.
5.  Selecteer op de pagina van de server en de schijf, klikt u op **schijf 2**en klik op **volgende**. Klik op **OK**wanneer daarom wordt gevraagd.
6.  Geef de grootte van het volume, klik op **volgende**.
7.  Bij het toewijzen aan een station of een map pagina, klik op **volgende**.
8.  Klik op **volgende**op de pagina Selecteer bestand systeem instellingen.
9.  Klik op de pagina bevestigen selecties **maken**.
10. Als alles klaar is, klikt u op **sluiten**.

Deze opdrachten uitvoeren vanaf de opdrachtprompt van Windows PowerShell op SQL1:

    md f:\Data
    md f:\Log
    md f:\Backup

Vervolgens lid maken van SQL1 CORP Windows Server Active Directory-domein met de volgende opdrachten bij de opdrachtprompt van de Windows PowerShell op SQL1.

    Add-Computer -DomainName corp.contoso.com
    Restart-Computer

CORP\User1-account wanneer u wordt gevraagd referenties domein-account voor de opdracht **Add-Computer** gebruiken.

Start opnieuw op en gebruik de Azure portal verbinding maken met een SQL1 *met de lokale administrator-account van SQL1*.

Vervolgens configureert u SQL Server 2014 gebruik van het station F: voor nieuwe databases en machtigingen van gebruikersaccount.

1.  Van het startscherm, typt u **Beheer van SQL Server**en klik vervolgens op **SQL Server 2014 Management Studio**.
2.  In **verbinding met de Server**, klikt u op **verbinden**.
3.  In het structuurvenster Object Explorer met de rechtermuisknop op **SQL1**en klik vervolgens op **Eigenschappen**.
4.  Klik in het venster **Eigenschappen van** **Database-instellingen**.
5.  Ga naar de **standaardlocatie van de Database** en deze waarden instellen: 
    - **Gegevens**, typt u het pad **f:\Data**.
    - **Aanmelden**, typt u het pad **f:\Log**.
    - Typ het pad **f:\Backup**voor **back-up**.
    - Opmerking: Alleen nieuwe databases gebruiken deze locaties.
6.  Klik op **OK** om het venster te sluiten.
7.  Open in het structuurvenster **Object Explorer** **beveiliging**.
8.  Klik met de rechtermuisknop op **aanmeldingen** en klik op **Nieuwe aanmelding**.
9.  Typ in het vak **aanmeldingsnaam**voor **CORP\User1**.
10. Op de pagina **Serverrollen** **sysadmin**op en klik vervolgens op **OK**.
11. Sluit Microsoft SQL Server Management Studio.

Dit is uw huidige configuratie.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph2.png)
 
## <a name="phase-3-configure-the-lob-server-lob1"></a>Fase 3: De server configureren voor LOB (LOB1)

Maak eerst een virtuele machine voor LOB1 met de volgende opdrachten bij de opdrachtprompt Azure PowerShell op uw lokale computer.

    $rgName="<your resource group name>"
    $locName="<your Azure location, such as West US>"
    $saName="<your storage account name>"
    
    $vnet=Get-AzureRMVirtualNetwork -Name "TestVNET" -ResourceGroupName $rgName
    $subnet=Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name "TestSubnet"
    $pip=New-AzureRMPublicIpAddress -Name LOB1-NIC -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
    $nic=New-AzureRMNetworkInterface -Name LOB1-NIC -ResourceGroupName $rgName -Location $locName -Subnet $subnet -PublicIpAddress $pip
    $vm=New-AzureRMVMConfig -VMName LOB1 -VMSize Standard_A2
    $storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
    $cred=Get-Credential -Message "Type the name and password of the local administrator account for LOB1."
    $vm=Set-AzureRMVMOperatingSystem -VM $vm -Windows -ComputerName LOB1 -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm=Set-AzureRMVMSourceImage -VM $vm -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
    $vm=Add-AzureRMVMNetworkInterface -VM $vm -Id $nic.Id
    $osDiskUri=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/LOB1-TestLab-OSDisk.vhd"
    $vm=Set-AzureRMVMOSDisk -VM $vm -Name LOB1-TestVNET-OSDisk -VhdUri $osDiskUri -CreateOption fromImage
    New-AzureRMVM -ResourceGroupName $rgName -Location $locName -VM $vm

Gebruik vervolgens de Azure portal verbinding maken met de LOB1 met de referenties van de lokale administrator-account van LOB1.

Configureer een Windows Firewall-regel voor het toestaan van verkeer voor eenvoudige connectiviteit testen. Uit de beheerdersniveau Windows PowerShell-prompt op LOB1, deze opdrachten worden uitgevoerd.

    Set-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" -enabled True
    ping dc2.corp.contoso.com

De opdracht ping moet leiden tot vier geslaagde antwoorden van IP-adres 192.168.0.4.

Vervolgens lid maken van LOB1 CORP, Active Directory-domein met de volgende opdrachten bij de opdrachtprompt van Windows PowerShell.

    Add-Computer -DomainName corp.contoso.com
    Restart-Computer

CORP\User1-account wanneer u wordt gevraagd referenties domein-account voor de opdracht **Add-Computer** gebruiken.

Start opnieuw op en gebruik de Azure portal verbinding maken met de LOB1 met de CORP\User1-account en wachtwoord.

Vervolgens LOB1 voor IIS configureren en testen vanaf CLIENT1 toegang.

1.  Vanuit Serverbeheer, klikt u op **functies en functies toevoegen**.
2.  Klik op **volgende**op de pagina **voordat u begint** .
3.  Klik op **volgende**op de pagina **installatietype selecteren** .
4.  Klik op **volgende**op de pagina **Selecteer de doelserver** .
5.  Klik op de pagina **Serverrollen** **Web Server (IIS)** in de lijst met **rollen**.
6.  Wanneer dat wordt gevraagd, klikt u op **Onderdelen toevoegen**en klik vervolgens op **volgende**.
7.  Klik op **volgende**op de pagina **functies selecteren** .
8.  Klik op **volgende**op de pagina **Webserver (IIS)** .
9.  Klik op de pagina **Rolservices** selecteren of schakel de selectievakjes uit voor de services die u nodig hebt voor het testen van uw LOB-toepassing en klik op **volgende**.
10. Klik op de pagina **installatie bevestigen zijn geselecteerd** op **installeren**.
11. Wacht tot de installatie van onderdelen is voltooid en klik vervolgens op **sluiten**.
12. Verbinding maken met de CLIENT1-computer met de referenties van het CORP\User1 van de Azure portal en start Internet Explorer.
13. In de adresbalk, typ **http://lob1/** en druk op ENTER. U ziet de pagina standaard IIS-8.

Dit is uw huidige configuratie.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph3.png)
 
Deze omgeving is nu gereed voor u uw webtoepassing op LOB1 implementeren en testen vanaf CLIENT1-functionaliteit op het subnet Corpnet.

## <a name="next-step"></a>Volgende stap

- Voeg een nieuwe virtuele machine met behulp van de [portal Azure](virtual-machines-windows-hero-tutorial.md).
