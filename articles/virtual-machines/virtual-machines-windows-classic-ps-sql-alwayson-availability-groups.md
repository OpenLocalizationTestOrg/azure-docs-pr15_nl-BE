<properties
    pageTitle="Groep altijd op beschikbaarheid in Azure VM met PowerShell configureren"
    description="In deze zelfstudie worden gemaakt met het klassieke implementatiemodel bronnen gebruikt en gebruikt PowerShell een altijd op beschikbaarheid-groep maken in Azure."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="MikeRayMSFT"
    manager="jhubbard"
    editor=""
    tags="azure-service-management" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="09/22/2016"
    ms.author="mikeray" />

# <a name="configure-always-on-availability-group-in-azure-vm-with-powershell"></a>Groep altijd op beschikbaarheid in Azure VM met PowerShell configureren

> [AZURE.SELECTOR]
- [Resource Manager: sjabloon](virtual-machines-windows-portal-sql-alwayson-availability-groups.md)
- [Resource Manager: handleiding](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md)
- [Klassiek: UI](virtual-machines-windows-classic-portal-sql-alwayson-availability-groups.md)
- [Klassiek: PowerShell](virtual-machines-windows-classic-ps-sql-alwayson-availability-groups.md)

<br/>

> [AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

Azure virtuele machines (VMs) kunt databasebeheerders lager de kosten van een hoge beschikbaarheid van SQL Server-systeem implementeren. In deze zelfstudie wordt beschreven hoe u een beschikbaarheidsgroep met behulp van SQL Server altijd op end-to-end binnen een Azure-omgeving te implementeren. Uw oplossing SQL Server altijd op in Azure wordt aan het einde van de zelfstudie bestaat uit de volgende elementen:

- Een virtueel netwerk meerdere subnetten, met inbegrip van een front-end- en back-end subnet met

- Een domeincontroller met een domein van Active Directory (AD)

- Twee SQL Server VMs geïmplementeerd op het subnet van de back-end en toegevoegd aan het domein AD

- Een cluster 3-node WSFC met de meerderheid van de knooppunt quorum model

- Een beschikbaarheidsgroep met twee replica's van synchrone doorvoeren van een database met beschikbaarheid

In dit scenario wordt gekozen voor de eenvoud, niet voor de kosteneffectiviteit of andere factoren op Azure. Bijvoorbeeld, kunt u het aantal VMs voor een beschikbaarheidsgroep van twee replica om op te slaan op uren in Azure berekenen met behulp van de domeincontroller als het quorum bestandssharewitness in een cluster met 2 nodes WSFC minimaliseren. Deze methode vermindert het aantal VM door een van de bovenstaande configuratie.

Deze zelfstudie is bedoeld om aan te tonen u de vereiste stappen voor het instellen van de beschreven oplossing hierboven zonder dat de details van elke stap bespreekt. Dus in plaats van met u de configuratiestappen GUI, wordt PowerShell scripts waarmee u snel door de verschillende stappen. Hierbij wordt ervan uitgegaan:

- U hebt al een abonnement op de virtuele machine Azure account.

- U kunt de [Azure PowerShell-cmdlets](../powershell-install-configure.md)hebt geïnstalleerd.

- U hebt al een goed begrip van altijd op beschikbaarheidsgroepen voor lokale oplossingen. Zie voor meer informatie [Altijd op beschikbaarheidsgroepen (SQL Server)](https://msdn.microsoft.com/library/hh510230.aspx).

## <a name="connect-to-your-azure-subscription-and-create-the-virtual-network"></a>Verbinding maken met uw abonnement Azure en het virtuele netwerk maken

1. De Azure module importeren in een venster PowerShell op uw lokale computer, een bestand publiceren naar uw computer downloaden en verbinding maken met de PowerShell-sessie uw Azure-abonnement met de gedownloade publicatie-instellingen importeren.

        Import-Module "C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\Azure\Azure.psd1"
        Get-AzurePublishSettingsFile
        Import-AzurePublishSettingsFile <publishsettingsfilepath>

    De opdracht **Get-AzurePublishgSettingsFile** genereert automatisch een certificaat met Azure downloaden naar uw computer. Een browser worden automatisch geopend en u wordt gevraagd referenties op te geven de Microsoft-account voor uw abonnement op Azure. Het **.publishsettings** van de gedownloade bestand bevat alle informatie die u nodig hebt voor het beheren van uw abonnement op Azure. Na het opslaan van dit bestand naar een lokale map, importeren met de opdracht **Importeren van AzurePublishSettingsFile** .

    >[AZURE.NOTE] Het bestand publishsettings bevat de referenties (ongecodeerd) die worden gebruikt voor het beheren van uw abonnementen Azure en services. Het wordt aangeraden voor dit bestand is tijdelijk buiten de mappen van uw bron (bijvoorbeeld in de map Libraries\Documents) opslaan en verwijderen nadat het importeren is voltooid. Een kwaadwillende gebruiker toegang krijgen tot het publishsettings-bestand kunt bewerken, maken en verwijderen van uw Azure services.

1. Een reeks variabelen die u gebruiken wilt voor het maken van de cloud IT-infrastructuur te definiëren.

        $location = "West US"
        $affinityGroupName = "ContosoAG"
        $affinityGroupDescription = "Contoso SQL HADR Affinity Group"
        $affinityGroupLabel = "IaaS BI Affinity Group"
        $networkConfigPath = "C:\scripts\Network.netcfg"
        $virtualNetworkName = "ContosoNET"
        $storageAccountName = "<uniquestorageaccountname>"
        $storageAccountLabel = "Contoso SQL HADR Storage Account"
        $storageAccountContainer = "https://" + $storageAccountName + ".blob.core.windows.net/vhds/"
        $winImageName = (Get-AzureVMImage | where {$_.Label -like "Windows Server 2008 R2 SP1*"} | sort PublishedDate -Descending)[0].ImageName
        $sqlImageName = (Get-AzureVMImage | where {$_.Label -like "SQL Server 2012 SP1 Enterprise*"} | sort PublishedDate -Descending)[0].ImageName
        $dcServerName = "ContosoDC"
        $dcServiceName = "<uniqueservicename>"
        $availabilitySetName = "SQLHADR"
        $vmAdminUser = "AzureAdmin"
        $vmAdminPassword = "Contoso!000"
        $workingDir = "c:\scripts\"

    Let op het volgende om ervoor te zorgen dat uw opdrachten hoger worden uitgevoerd:

    - Variabelen **$storageAccountName** en **$dcServiceName** moeten uniek zijn omdat ze worden gebruikt voor het identificeren van de cloud opslag account en cloud server, respectievelijk op het internet.

    - Namen die zijn opgegeven voor de variabelen **$affinityGroupName** en **$virtualNetworkName** zijn geconfigureerd in het virtuele netwerk configuratie document die u later wilt gebruiken.

    - **$sqlImageName** bevat de bijgewerkte naam van de afbeelding VM met SQL Server 2012 Service Pack 1 Enterprise Edition.

    - Om redenen van eenvoud **Contoso! 000** hetzelfde wachtwoord gebruikt gedurende de gehele zelfstudie is.

1. Maak een groep affiniteit.

        New-AzureAffinityGroup `
            -Name $affinityGroupName `
            -Location $location `
            -Description $affinityGroupDescription `
            -Label $affinityGroupLabel

1. Een virtueel netwerk maken door een configuratiebestand te importeren.

        Set-AzureVNetConfig `
            -ConfigurationPath $networkConfigPath

    Het configuratiebestand bevat de volgende XML-document. Kort samengevat: Hiermee geeft u aan een virtueel netwerk genaamd **ContosoNET** in de affiniteit groep met de naam **ContosoAG**en is het adres ruimte **10.10.0.0/16** en heeft twee subnetten, **10.10.1.0/24** en **10.10.2.0/24**, die het subnet front en back-subnet, respectievelijk. Het voorste subnet is waarbij u clienttoepassingen zoals Microsoft SharePoint kunt plaatsen en het subnet weer waarin u de SQL Server-VMs zal plaatsen. Als u eerder de variabelen **$affinityGroupName** en **$virtualNetworkName** wijzigt, moet u ook de bijbehorende namen hieronder.

        <NetworkConfiguration xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
          <VirtualNetworkConfiguration>
            <Dns />
            <VirtualNetworkSites>
              <VirtualNetworkSite name="ContosoNET" AffinityGroup="ContosoAG">
                <AddressSpace>
                  <AddressPrefix>10.10.0.0/16</AddressPrefix>
                </AddressSpace>
                <Subnets>
                  <Subnet name="Front">
                    <AddressPrefix>10.10.1.0/24</AddressPrefix>
                  </Subnet>
                  <Subnet name="Back">
                    <AddressPrefix>10.10.2.0/24</AddressPrefix>
                  </Subnet>
                </Subnets>
              </VirtualNetworkSite>
            </VirtualNetworkSites>
          </VirtualNetworkConfiguration>
        </NetworkConfiguration>

1. Een opslag account maken die is gekoppeld aan de groep affiniteit u hebt gemaakt en deze instellen als de huidige account voor opslag in uw abonnement.

        New-AzureStorageAccount `
            -StorageAccountName $storageAccountName `
            -Label $storageAccountLabel `
            -AffinityGroup $affinityGroupName
        Set-AzureSubscription `
            -SubscriptionName (Get-AzureSubscription).SubscriptionName `
            -CurrentStorageAccount $storageAccountName

1. De DC-server maken in nieuwe wolk service en beschikbaarheid.

        New-AzureVMConfig `
            -Name $dcServerName `
            -InstanceSize Medium `
            -ImageName $winImageName `
            -MediaLocation "$storageAccountContainer$dcServerName.vhd" `
            -DiskLabel "OS" |
            Add-AzureProvisioningConfig `
                -Windows `
                -DisableAutomaticUpdates `
                -AdminUserName $vmAdminUser `
                -Password $vmAdminPassword |
                New-AzureVM `
                    -ServiceName $dcServiceName `
                    –AffinityGroup $affinityGroupName `
                    -VNetName $virtualNetworkName

    Deze reeks opdrachten doorgesluisd het volgende doen:

    - **Nieuw AzureVMConfig** maakt een VM-configuratie.

    - **Toevoegen-AzureProvisioningConfig** geeft de parameters voor de configuratie van een zelfstandige Windows server.

    - **Add-AzureDataDisk** wordt de schijf met gegevens die u gebruiken wilt voor het opslaan van gegevens in Active Directory, met cache de optie is ingesteld op geen.

    - **Nieuw AzureVM** maakt een nieuwe wolk service en de nieuwe Azure VM maakt in de nieuwe wolk service.

1. Wachten op de nieuwe VM volledig worden ingericht en het externe bureaublad bestand downloaden naar uw werkmap. Aangezien de nieuwe Azure VM duurt erg lang om in te richten, terwijl lus blijft op te vragen voor de nieuwe VM totdat deze gereed voor gebruik is.

        $VMStatus = Get-AzureVM -ServiceName $dcServiceName -Name $dcServerName

        While ($VMStatus.InstanceStatus -ne "ReadyRole")
        {
            write-host "Waiting for " $VMStatus.Name "... Current Status = " $VMStatus.InstanceStatus
            Start-Sleep -Seconds 15
            $VMStatus = Get-AzureVM -ServiceName $dcServiceName -Name $dcServerName
        }

        Get-AzureRemoteDesktopFile `
            -ServiceName $dcServiceName `
            -Name $dcServerName `
            -LocalPath "$workingDir$dcServerName.rdp"

De DC-server is nu zijn ingericht. Vervolgens configureert u het domein van Active Directory op deze server DC. Sluit het venster PowerShell op uw lokale computer. U kunt later opnieuw maken van de twee SQL Server VMs.

## <a name="configure-the-domain-controller"></a>De domeincontroller configureren

1. Maakt verbinding met de DC-server het externe bureaublad bestand te starten. AzureAdmin gebruikersnaam en het wachtwoord van de beheerder van de computer gebruiken **Contoso! 000**, die u hebt opgegeven bij het maken van de nieuwe VM.

1. Open een venster PowerShell in de administrator-modus.

1. Voer de volgende **DCPROMO. EXE** opdracht voor het instellen van het domein **corp.contoso.com** , waarbij de gegevens mappen op station M.

        dcpromo.exe `
            /unattend `
            /ReplicaOrNewDomain:Domain `
            /NewDomain:Forest `
            /NewDomainDNSName:corp.contoso.com `
            /ForestLevel:4 `
            /DomainNetbiosName:CORP `
            /DomainLevel:4 `
            /InstallDNS:Yes `
            /ConfirmGc:Yes `
            /CreateDNSDelegation:No `
            /DatabasePath:"C:\Windows\NTDS" `
            /LogPath:"C:\Windows\NTDS" `
            /SYSVOLPath:"C:\Windows\SYSVOL" `
            /SafeModeAdminPassword:"Contoso!000"

    Nadat de opdracht is voltooid, wordt de VM automatisch opnieuw gestart.

1. Maakt verbinding met de DC-server opnieuw starten van de extern bureaublad-bestand. Deze keer aanmelden als **CORP\Administrator**.

1. Open een venster PowerShell in de administrator-modus en importeren van de module Active Directory PowerShell is met de volgende opdracht:

        Import-Module ActiveDirectory

1. Voer de volgende opdrachten drie gebruikers toevoegen aan het domein.

        $pwd = ConvertTo-SecureString "Contoso!000" -AsPlainText -Force
        New-ADUser `
            -Name 'Install' `
            -AccountPassword  $pwd `
            -PasswordNeverExpires $true `
            -ChangePasswordAtLogon $false `
            -Enabled $true
        New-ADUser `
            -Name 'SQLSvc1' `
            -AccountPassword  $pwd `
            -PasswordNeverExpires $true `
            -ChangePasswordAtLogon $false `
            -Enabled $true
        New-ADUser `
            -Name 'SQLSvc2' `
            -AccountPassword  $pwd `
            -PasswordNeverExpires $true `
            -ChangePasswordAtLogon $false `
            -Enabled $true

    **CORP\Install** wordt gebruikt voor het configureren van alles wat betrekking heeft op de exemplaren van SQL Server-service, de cluster WSFC en de van beschikbaarheidsgroep. **CORP\SQLSvc1** en **CORP\SQLSvc2** worden gebruikt als de SQL Server-serviceaccounts voor de twee SQL Server VMs.

1. Voer de volgende opdrachten voor **CORP\Install** machtiging Computerobjecten maken in het domein.

        Cd ad:
        $sid = new-object System.Security.Principal.SecurityIdentifier (Get-ADUser "Install").SID
        $guid = new-object Guid bf967a86-0de6-11d0-a285-00aa003049e2
        $ace1 = new-object System.DirectoryServices.ActiveDirectoryAccessRule $sid,"CreateChild","Allow",$guid,"All"
        $corp = Get-ADObject -Identity "DC=corp,DC=contoso,DC=com"
        $acl = Get-Acl $corp
        $acl.AddAccessRule($ace1)
        Set-Acl -Path "DC=corp,DC=contoso,DC=com" -AclObject $acl

    De hierboven opgegeven GUID is de GUID voor het computertype. De **CORP\Install** -account, moet de machtiging **Alle eigenschappen lezen** en **Maken van computerobjecten** te maken voor het cluster WSFC directe actieve objecten. De machtiging **Lezen alle eigenschappen** is al standaard toegekend aan CORP\Install, dus u hoeft niet expliciet te kennen. Zie voor meer informatie over machtigingen die nodig zijn voor het maken van het cluster WSFC [Stapsgewijze handleiding voor Failover-Cluster: computeraccounts in Active Directory configureren](https://technet.microsoft.com/library/cc731002%28v=WS.10%29.aspx).

    Nu dat u klaar bent met het configureren van Active Directory en de gebruikersobjecten die u maakt twee SQL Server VMs en voeg ze toe aan dit domein.

## <a name="create-the-sql-server-vms"></a>Maak de SQL Server-VMs

1. Blijven de PowerShell-venster dat is geopend op de lokale computer gebruiken. De volgende aanvullende variabelen definiëren:

        $domainName= "corp"
        $FQDN = "corp.contoso.com"
        $subnetName = "Back"
        $sqlServiceName = "<uniqueservicename>"
        $quorumServerName = "ContosoQuorum"
        $sql1ServerName = "ContosoSQL1"
        $sql2ServerName = "ContosoSQL2"
        $availabilitySetName = "SQLHADR"
        $dataDiskSize = 100
        $dnsSettings = New-AzureDns -Name "ContosoBackDNS" -IPAddress "10.10.0.4"

    Het IP-adres **10.10.0.4** wordt meestal toegewezen aan de eerste VM die u in het subnet van de **10.10.0.0/16** van uw Azure virtueel netwerk maken. Dit is het adres van uw server DC **IPCONFIG**uitvoert, moet u controleren.

1. Voer de volgende piped opdrachten voor het maken van de eerste VM in het cluster WSFC, met de naam **ContosoQuorum**:

        New-AzureVMConfig `
            -Name $quorumServerName `
            -InstanceSize Medium `
            -ImageName $winImageName `
            -MediaLocation "$storageAccountContainer$quorumServerName.vhd" `
            -AvailabilitySetName $availabilitySetName `
            -DiskLabel "OS" |
            Add-AzureProvisioningConfig `
                -WindowsDomain `
                -AdminUserName $vmAdminUser `
                -Password $vmAdminPassword `
                -DisableAutomaticUpdates `
                -Domain $domainName `
                -JoinDomain $FQDN `
                -DomainUserName $vmAdminUser `
                -DomainPassword $vmAdminPassword |
                Set-AzureSubnet `
                    -SubnetNames $subnetName |
                    New-AzureVM `
                        -ServiceName $sqlServiceName `
                        –AffinityGroup $affinityGroupName `
                        -VNetName $virtualNetworkName `
                        -DnsSettings $dnsSettings

    Let op het volgende met betrekking tot de bovenstaande opdracht:

    - **Nieuw AzureVMConfig** maakt een VM-configuratie met de naam van de gewenste beschikbaarheid. De volgende VMs wordt gemaakt met dezelfde naam van de beschikbaarheid zodat ze lid zijn van dezelfde set beschikbaarheid.

    - **Add-AzureProvisioningConfig** lid van de VM aan de Active Directory-domein dat u hebt gemaakt.

    - **Set AzureSubnet** plaatst de VM in het subnet weer.

    - **Nieuw AzureVM** maakt een nieuwe wolk service en de nieuwe Azure VM maakt in de nieuwe wolk service. De parameter **DnsSettings** geeft aan dat de DNS-server voor de servers in de nieuwe wolk service het IP-adres **10.10.0.4**, waarbij het IP-adres van de DC-server heeft. Deze parameter is vereist voor de nieuwe VMs in de cloud-service toevoegen aan de Active Directory-domein is. Zonder deze parameter, moet u de IPv4-instellingen handmatig instellen in uw VM de DC-server gebruiken als het primaire DNS-server nadat de VM is ingericht en vervolgens de VM toevoegen aan de Active Directory-domein.

1. Voer de volgende piped opdrachten voor het maken van de SQL Server VMs, met de naam **ContosoSQL1** en **ContosoSQL2**.

        # Create ContosoSQL1...
        New-AzureVMConfig `
            -Name $sql1ServerName `
            -InstanceSize Large `
            -ImageName $sqlImageName `
            -MediaLocation "$storageAccountContainer$sql1ServerName.vhd" `
            -AvailabilitySetName $availabilitySetName `
            -HostCaching "ReadOnly" `
            -DiskLabel "OS" |
            Add-AzureProvisioningConfig `
                -WindowsDomain `
                -AdminUserName $vmAdminUser `
                -Password $vmAdminPassword `
                -DisableAutomaticUpdates `
                -Domain $domainName `
                -JoinDomain $FQDN `
                -DomainUserName $vmAdminUser `
                -DomainPassword $vmAdminPassword |
                Set-AzureSubnet `
                    -SubnetNames $subnetName |
                    Add-AzureEndpoint `
                        -Name "SQL" `
                        -Protocol "tcp" `
                        -PublicPort 1 `
                        -LocalPort 1433 |
                        New-AzureVM `
                            -ServiceName $sqlServiceName

        # Create ContosoSQL2...
        New-AzureVMConfig `
            -Name $sql2ServerName `
            -InstanceSize Large `
            -ImageName $sqlImageName `
            -MediaLocation "$storageAccountContainer$sql2ServerName.vhd" `
            -AvailabilitySetName $availabilitySetName `
            -HostCaching "ReadOnly" `
            -DiskLabel "OS" |
            Add-AzureProvisioningConfig `
                -WindowsDomain `
                -AdminUserName $vmAdminUser `
                -Password $vmAdminPassword `
                -DisableAutomaticUpdates `
                -Domain $domainName `
                -JoinDomain $FQDN `
                -DomainUserName $vmAdminUser `
                -DomainPassword $vmAdminPassword |
                Set-AzureSubnet `
                    -SubnetNames $subnetName |
                    Add-AzureEndpoint `
                        -Name "SQL" `
                        -Protocol "tcp" `
                        -PublicPort 2 `
                        -LocalPort 1433 |
                        New-AzureVM `
                            -ServiceName $sqlServiceName

    Let op het volgende met betrekking tot de bovenstaande opdrachten:

    - **Nieuw AzureVMConfig** maakt gebruik van de beschikbaarheid van dezelfde naam als de DC-server en de SQL Server 2012 Service Pack 1 Enterprise Edition-afbeelding in de galerie met virtuele machine gebruikt. Ook wordt de schijf ingesteld op alleen-cachen alleen (geen schrijfcache). Het wordt aanbevolen de databasebestanden op een afzonderlijke schijf u koppelen aan de VM en worden geconfigureerd met niet lezen of de schrijfcache te migreren. De volgende beste is echter de schrijfcache op de schijf verwijderen, omdat u niet verwijderen in de cache opslaan op de schijf lezen.

    - **Add-AzureProvisioningConfig** lid van de VM aan de Active Directory-domein dat u hebt gemaakt.

    - **Set AzureSubnet** plaatst de VM in het subnet weer.

    - **Add-AzureEndpoint** toegevoegd toegang eindpunten, zodat toepassingen die toegang hebben tot deze services-exemplaren van SQL Server op het internet. Verschillende poorten zijn gegeven aan ContosoSQL1 en ContosoSQL2.

    - **Nieuw AzureVM** maakt de nieuwe SQL Server-VM in dezelfde service cloud als ContosoQuorum. Als u wilt dat ze in bepaalde beschikbaarheid, moet u het VMs in dezelfde service cloud plaatsen.

1. Wacht elke VM volledig worden ingericht en de extern bureaublad-bestand downloaden naar uw werkmap. De voor een lus doorlopen van de drie nieuwe VMs en voert de opdrachten tussen gekrulde haakjes op het hoogste niveau voor elk van hen.

        Foreach ($VM in $VMs = Get-AzureVM -ServiceName $sqlServiceName)
        {
            write-host "Waiting for " $VM.Name "..."

            # Loop until the VM status is "ReadyRole"
            While ($VM.InstanceStatus -ne "ReadyRole")
            {
                write-host "  Current Status = " $VM.InstanceStatus
                Start-Sleep -Seconds 15
                $VM = Get-AzureVM -ServiceName $VM.ServiceName -Name $VM.InstanceName
            }

            write-host "  Current Status = " $VM.InstanceStatus

            # Download remote desktop file
            Get-AzureRemoteDesktopFile -ServiceName $VM.ServiceName -Name $VM.InstanceName -LocalPath "$workingDir$($VM.InstanceName).rdp"
        }

    De SQL Server-VMs nu zijn ingericht en uitgevoerd, maar ze worden geïnstalleerd met SQL Server met de standaardopties.

## <a name="initialize-the-wsfc-cluster-vms"></a>De WSFC Cluster VMs initialiseren

In deze sectie moet u de drie servers die u in het cluster WSFC en de installatie van SQL Server gebruiken wilt te wijzigen. Met name:

- (Alle servers) U moet het **Onderdeel Failover** Clustering installeren.

- (Alle servers) U moet **CORP\Install** als de **beheerder**van de computer toevoegen.

- (ContosoSQL1 en ContosoSQL2 alleen) U moet **CORP\Install** als een rol **sysadmin** in de standaarddatabase toevoegen.

- (ContosoSQL1 en ContosoSQL2 alleen) U moet een aanmelding met de volgende machtigingen toevoegen **NT AUTHORITY\System** :

    - Beschikbaarheid groepen wijzigen

    - Verbinding maken met SQL

    - Status weergeven

- (ContosoSQL1 en ContosoSQL2 alleen) Het **TCP-** protocol is reeds ingeschakeld op de SQL Server-VM. U moet echter nog steeds voor het openen van de firewall voor externe toegang van SQL Server.

U bent nu klaar om te starten. Beginnen met **ContosoQuorum**, de volgende stappen:

1. Verbinding maken met **ContosoQuorum** door de bestanden met extern bureaublad te starten. **AzureAdmin** gebruikersnaam en het wachtwoord van de beheerder van de computer gebruiken **Contoso! 000**, die u hebt opgegeven bij het maken van het VMs.

1. Controleer of dat de computers hebt toegevoegd aan **corp.contoso.com**.

1. Wacht tot de SQL Server-installatie voltooit de van geautomatiseerde initialisatietaken voordat u verdergaat.

1. Open een venster PowerShell in de administrator-modus.

1. Installeer het onderdeel Failover Clustering in Windows.

        Import-Module ServerManager
        Add-WindowsFeature Failover-Clustering

1. **CORP\Install** toevoegen als lokale beheerder.

        net localgroup administrators "CORP\Install" /Add

1. Meld u bij de ContosoQuorum. U bent nu klaar met deze server.

        logoff.exe

Vervolgens initialiseren **ContosoSQL1** en **ContosoSQL2**. Volg de stappen hieronder, identiek voor zowel van de VMs SQL Server zijn.

1. Verbinding maken met de twee VMs voor SQL Server door de bestanden met extern bureaublad te starten. **AzureAdmin** gebruikersnaam en het wachtwoord van de beheerder van de computer gebruiken **Contoso! 000**, die u hebt opgegeven bij het maken van het VMs.

1. Controleer of dat de computers hebt toegevoegd aan **corp.contoso.com**.

1. Wacht tot de SQL Server-installatie voltooit de van geautomatiseerde initialisatietaken voordat u verdergaat.

1. Open een venster PowerShell in de administrator-modus.

1. Installeer het onderdeel Failover Clustering in Windows.

        Import-Module ServerManager
        Add-WindowsFeature Failover-Clustering

1. **CORP\Install** toevoegen als lokale beheerder

        net localgroup administrators "CORP\Install" /Add

1. De PowerShell-voorziening voor SQL Server importeren.

        Set-ExecutionPolicy -Execution RemoteSigned -Force
        Import-Module -Name "sqlps" -DisableNameChecking

1. **CORP\Install** toevoegen als de rol sysadmin voor het standaardexemplaar van SQL Server.

        net localgroup administrators "CORP\Install" /Add
        Invoke-SqlCmd -Query "EXEC sp_addsrvrolemember 'CORP\Install', 'sysadmin'" -ServerInstance "."

1. **NT AUTHORITY\System** toevoegen als een aanmelding met de drie hierboven voor machtigingen.

        Invoke-SqlCmd -Query "CREATE LOGIN [NT AUTHORITY\SYSTEM] FROM WINDOWS" -ServerInstance "."
        Invoke-SqlCmd -Query "GRANT ALTER ANY AVAILABILITY GROUP TO [NT AUTHORITY\SYSTEM] AS SA" -ServerInstance "."
        Invoke-SqlCmd -Query "GRANT CONNECT SQL TO [NT AUTHORITY\SYSTEM] AS SA" -ServerInstance "."
        Invoke-SqlCmd -Query "GRANT VIEW SERVER STATE TO [NT AUTHORITY\SYSTEM] AS SA" -ServerInstance "."

1. Open de firewall voor externe toegang van SQL Server.

        netsh advfirewall firewall add rule name='SQL Server (TCP-In)' program='C:\Program Files\Microsoft SQL Server\MSSQL11.MSSQLSERVER\MSSQL\Binn\sqlservr.exe' dir=in action=allow protocol=TCP

1. Meld u bij beide VMs.

        logoff.exe

Tot slot, bent u klaar voor het configureren van de groep van de beschikbaarheid. U kunt de PowerShell-voorziening voor SQL Server al het werk uitvoeren op **ContosoSQL1**.

## <a name="configure-the-availability-group"></a>Configureer de beschikbaarheid van groep

1. Maakt verbinding met **ContosoSQL1** opnieuw starten van de remote desktop-bestanden. In plaats van aanmelden met de computeraccount, zich aanmelden met behulp van **CORP\Install**.

1. Open een venster PowerShell in de administrator-modus.

1. De volgende variabelen definiëren:

        $server1 = "ContosoSQL1"
        $server2 = "ContosoSQL2"
        $serverQuorum = "ContosoQuorum"
        $acct1 = "CORP\SQLSvc1"
        $acct2 = "CORP\SQLSvc2"
        $password = "Contoso!000"
        $clusterName = "Cluster1"
        $timeout = New-Object System.TimeSpan -ArgumentList 0, 0, 30
        $db = "MyDB1"
        $backupShare = "\\$server1\backup"
        $quorumShare = "\\$server1\quorum"
        $ag = "AG1"

1. PowerShell-Provider voor SQL Server importeren.

        Set-ExecutionPolicy RemoteSigned -Force
        Import-Module "sqlps" -DisableNameChecking

1. De SQL Server-serviceaccount wijzigen voor ContosoSQL1 in CORP\SQLSvc1.

        $wmi1 = new-object ("Microsoft.SqlServer.Management.Smo.Wmi.ManagedComputer") $server1
        $wmi1.services | where {$_.Type -eq 'SqlServer'} | foreach{$_.SetServiceAccount($acct1,$password)}
        $svc1 = Get-Service -ComputerName $server1 -Name 'MSSQLSERVER'
        $svc1.Stop()
        $svc1.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Stopped,$timeout)
        $svc1.Start();
        $svc1.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Running,$timeout)

1. De SQL Server-serviceaccount wijzigen voor ContosoSQL2 in CORP\SQLSvc2.

        $wmi2 = new-object ("Microsoft.SqlServer.Management.Smo.Wmi.ManagedComputer") $server2
        $wmi2.services | where {$_.Type -eq 'SqlServer'} | foreach{$_.SetServiceAccount($acct2,$password)}
        $svc2 = Get-Service -ComputerName $server2 -Name 'MSSQLSERVER'
        $svc2.Stop()
        $svc2.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Stopped,$timeout)
        $svc2.Start();
        $svc2.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Running,$timeout)

1. **CreateAzureFailoverCluster.ps1** van [WSFC-Cluster maken voor altijd op beschikbaarheidsgroepen in Azure VM](http://gallery.technet.microsoft.com/scriptcenter/Create-WSFC-Cluster-for-7c207d3a) downloaden naar de lokale werkmap. U kunt dit script kunt u een functionele WSFC cluster maken. Zie belangrijke informatie over hoe WSFC met het netwerk Azure omgaat, [hoge beschikbaarheid en noodherstel voor SQL Server in Azure virtuele Machines](virtual-machines-windows-sql-high-availability-dr.md).

1. Wijzigen in uw werkmap en het cluster WSFC maken met het gedownloade script.

        Set-ExecutionPolicy Unrestricted -Force
        .\CreateAzureFailoverCluster.ps1 -ClusterName "$clusterName" -ClusterNode "$server1","$server2","$serverQuorum"

1. Altijd op beschikbaarheidsgroepen inschakelen voor de standaard SQL Server-exemplaren op **ContosoSQL1** en **ContosoSQL2**.

        Enable-SqlAlwaysOn `
            -Path SQLSERVER:\SQL\$server1\Default `
            -Force
        Enable-SqlAlwaysOn `
            -Path SQLSERVER:\SQL\$server2\Default `
            -NoServiceRestart
        $svc2.Stop()
        $svc2.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Stopped,$timeout)
        $svc2.Start();
        $svc2.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Running,$timeout)

1. Een back-upmap maken en machtigingen voor de SQL Server-serviceaccounts. U kunt deze map de beschikbaarheid van database op de secundaire replica voorbereiden.

        $backup = "C:\backup"
        New-Item $backup -ItemType directory
        net share backup=$backup "/grant:$acct1,FULL" "/grant:$acct2,FULL"
        icacls.exe "$backup" /grant:r ("$acct1" + ":(OI)(CI)F") ("$acct2" + ":(OI)(CI)F")

1. Een database maakt op **ContosoSQL1** **MyDB1**genoemd, nemen zowel een volledige back-up een back-up en op **ContosoSQL2** met de optie **Wellicht** herstellen.

        Invoke-SqlCmd -Query "CREATE database $db"
        Backup-SqlDatabase -Database $db -BackupFile "$backupShare\db.bak" -ServerInstance $server1
        Backup-SqlDatabase -Database $db -BackupFile "$backupShare\db.log" -ServerInstance $server1 -BackupAction Log
        Restore-SqlDatabase -Database $db -BackupFile "$backupShare\db.bak" -ServerInstance $server2 -NoRecovery
        Restore-SqlDatabase -Database $db -BackupFile "$backupShare\db.log" -ServerInstance $server2 -RestoreAction Log -NoRecovery

1. De beschikbaarheid op de SQL Server-VMs groep eindpunten maken en de juiste machtigingen ingesteld op de eindpunten.

        $endpoint =
            New-SqlHadrEndpoint MyMirroringEndpoint `
            -Port 5022 `
            -Path "SQLSERVER:\SQL\$server1\Default"
        Set-SqlHadrEndpoint `
            -InputObject $endpoint `
            -State "Started"
        $endpoint =
            New-SqlHadrEndpoint MyMirroringEndpoint `
            -Port 5022 `
            -Path "SQLSERVER:\SQL\$server2\Default"
        Set-SqlHadrEndpoint `
            -InputObject $endpoint `
            -State "Started"

        Invoke-SqlCmd -Query "CREATE LOGIN [$acct2] FROM WINDOWS" -ServerInstance $server1
        Invoke-SqlCmd -Query "GRANT CONNECT ON ENDPOINT::[MyMirroringEndpoint] TO [$acct2]" -ServerInstance $server1
        Invoke-SqlCmd -Query "CREATE LOGIN [$acct1] FROM WINDOWS" -ServerInstance $server2
        Invoke-SqlCmd -Query "GRANT CONNECT ON ENDPOINT::[MyMirroringEndpoint] TO [$acct1]" -ServerInstance $server2

1. De beschikbaarheid van replica's maken.

        $primaryReplica =
            New-SqlAvailabilityReplica `
            -Name $server1 `
            -EndpointURL "TCP://$server1.corp.contoso.com:5022" `
            -AvailabilityMode "SynchronousCommit" `
            -FailoverMode "Automatic" `
            -Version 11 `
            -AsTemplate
        $secondaryReplica =
            New-SqlAvailabilityReplica `
            -Name $server2 `
            -EndpointURL "TCP://$server2.corp.contoso.com:5022" `
            -AvailabilityMode "SynchronousCommit" `
            -FailoverMode "Automatic" `
            -Version 11 `
            -AsTemplate

1. Ten slotte maakt de beschikbaarheidsgroep en de secundaire replica toevoegen aan de beschikbaarheidsgroep.

        New-SqlAvailabilityGroup `
            -Name $ag `
            -Path "SQLSERVER:\SQL\$server1\Default" `
            -AvailabilityReplica @($primaryReplica,$secondaryReplica) `
            -Database $db
        Join-SqlAvailabilityGroup `
            -Path "SQLSERVER:\SQL\$server2\Default" `
            -Name $ag
        Add-SqlAvailabilityDatabase `
            -Path "SQLSERVER:\SQL\$server2\Default\AvailabilityGroups\$ag" `
            -Database $db

## <a name="next-steps"></a>Volgende stappen
U hebt nu met succes geïmplementeerd SQL Server altijd op door het maken van een beschikbaarheidsgroep in Azure. Zie [een listener ILB voor altijd op beschikbaarheidsgroepen in Azure configureren](virtual-machines-windows-classic-ps-sql-int-listener.md)configureren een listener voor deze beschikbaarheidsgroep.

Zie voor meer informatie over het gebruik van SQL Server in Azure, [SQL Server Azure virtuele Machines](virtual-machines-windows-sql-server-iaas-overview.md).
