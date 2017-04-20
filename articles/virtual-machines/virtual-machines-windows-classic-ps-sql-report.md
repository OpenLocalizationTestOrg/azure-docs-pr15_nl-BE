<properties 
    pageTitle="PowerShell gebruiken voor het maken van een VM met een Native-modus-rapportserver | Microsoft Azure"
    description="In dit onderwerp wordt beschreven, en begeleidt u bij de implementatie en configuratie van een server voor SQL Server Reporting Services native modus rapport in een Azure Virtual Machine. "
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="guyinacube"
    manager="erikre"
    editor="monicar" 
    tags="azure-service-management"/>
<tags 
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/04/2016"
    ms.author="asaxton" />

# <a name="use-powershell-to-create-an-azure-vm-with-a-native-mode-report-server"></a>PowerShell gebruiken voor het maken van een Azure VM met een rapportserver Native modus

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]
 

In dit onderwerp wordt beschreven, en begeleidt u bij de implementatie en configuratie van een server voor SQL Server Reporting Services native modus rapport in een Azure Virtual Machine. De stappen in dit document gebruikt een combinatie van handmatige stappen voor het maken van de virtuele machine en een Windows PowerShell-script voor het configureren van Reporting Services op de VM. Het configuratiescript bevat een firewallpoort te openen voor HTTP of HTTPs.

>[AZURE.NOTE] Als u geen **HTTPS** op de rapportserver, **slaat u stap 2 vereist**.
>
>Na het maken van de VM in stap 1, Ga naar de sectie script gebruiken voor het configureren van de report server en HTTP. Nadat u het script uitvoert, is de report server gereed voor gebruik.

## <a name="prerequisites-and-assumptions"></a>Voorwaarden en uitgangspunten

- **Azure-abonnement**: Controleer of het aantal cores beschikbaar zijn in uw abonnement Azure. Als u de aanbevolen grootte VM **a3**maakt, moet u **4** beschikbare cores. Als u een VM-grootte van **A2**gebruikt, moet u **2** beschikbare cores.
    
    - Om te controleren of de core limiet van uw abonnement, in de klassieke Azure portal, klikt u op instellingen in het linkerdeelvenster en klikt u op TAAKGEBRUIK in het bovenste menu.
    
    - U verhoogt het quotum core, neem contact op met [Ondersteuning van Azure](https://azure.microsoft.com/support/options/). Zie voor informatie over de grootte van VM [Formaten voor Azure Virtual Machine](virtual-machines-linux-sizes.md).

- **Windows PowerShell-scripts**: het onderwerp wordt ervan uitgegaan dat u een basiskennis van de werking van Windows PowerShell. Zie de volgende onderwerpen voor meer informatie over het gebruik van Windows PowerShell:

    - [Windows PowerShell zo starten op Windows Server](https://technet.microsoft.com/library/hh847814.aspx)
    
    - [Aan de slag met Windows PowerShell](https://technet.microsoft.com/library/hh857337.aspx)

## <a name="step-1-provision-an-azure-virtual-machine"></a>Stap 1: Een Azure Virtual Machine inrichten

1. Ga naar de klassieke Azure portal.

1. Klik op **virtuele Machines** in het linkerdeelvenster.

    ![Microsoft azure virtuele machines](./media/virtual-machines-windows-classic-ps-sql-report/IC660124.gif)

1. Klik op **Nieuw**.

    ![knop Nieuw](./media/virtual-machines-windows-classic-ps-sql-report/IC692019.gif)

1. Klik op **uit galerie**.

    ![nieuwe vm uit galerie](./media/virtual-machines-windows-classic-ps-sql-report/IC692020.gif)

1. Klik op **SQL Server 2014 RTM Standard: Windows Server 2012 R2** en klik vervolgens op de pijl om door te gaan.

    ![volgende](./media/virtual-machines-windows-classic-ps-sql-report/IC692021.gif)

    Als u de functie voor abonnementen voor gegevensgestuurde van Reporting Services, kiest u **SQL Server 2014 RTM Enterprise: Windows Server 2012 R2**. Zie [Features Supported by de edities van SQL Server 2012](https://msdn.microsoft.com/library/cc645993.aspx#Reporting)voor meer informatie over edities van SQL Server en de voor functieondersteuning.

1. Bewerken op de pagina **configuratie van de virtuele machine** de volgende velden:
                                    
    - Als er meer dan één **versie datum**, selecteert u de meest recente versie.
    
    - **Naam van de virtuele Machine**: naam van de computer ook op de volgende configuratiepagina wordt gebruikt als de standaard Cloud Service DNS-naam. De DNS-naam moet uniek zijn binnen de Azure service. U kunt de VM configureren met de naam van een computer waarmee wordt beschreven wat de VM wordt gebruikt. Bijvoorbeeld ssrsnativecloud.
    
    - **Lagen**: standaard
    
    - **Formaat: A3** is de aanbevolen grootte van de VM voor werkbelasting van SQL Server. Als een VM alleen als een rapportserver volstaat een VM-grootte van A2 gebruikt wordt tenzij de report server te maken krijgt met een grote werkbelasting. Zie [Virtuele Machines prijzen](https://azure.microsoft.com/pricing/details/virtual-machines/)voor VM prijsinformatie.
    
    - **Nieuwe naam**: de naam die u opgeeft wordt gemaakt door een beheerder op de VM.
    
    - **Nieuw wachtwoord** en **Bevestig**dit. Dit wachtwoord wordt gebruikt voor de nieuwe administrator-account en het verdient aanbeveling dat een sterk wachtwoord.
    
    - Klik op **volgende**. ![volgende](./media/virtual-machines-windows-classic-ps-sql-report/IC692021.gif)

1. Bewerk de volgende velden op de volgende pagina:

    - **Cloud Service**: Selecteer **een nieuwe wolk Service maken**.
    
    - **De naam van de DNS-Service cloud**: dit is de naam van de Cloud-Service die is gekoppeld aan de VM. De standaardnaam is de naam die u hebt opgegeven voor de naam van de VM. Als u in latere stappen van het onderwerp maakt u een vertrouwd certificaat voor SSL en vervolgens de DNS-naam voor de waarde van de "**verleend aan**" van het certificaat wordt gebruikt.
    
    - **Regio/affiniteit groep/Virtual Network**: Kies de regio die het dichtst bij uw eindgebruikers.
    
    - **Opslag rekening**: een automatisch gegenereerde opslag gebruikt.
    
    - **Beschikbaar**: geen.
    
    - **EINDPUNTEN** De eindpunten van **Extern bureaublad** en **PowerShell** houden en voeg vervolgens een HTTP- of HTTPS-eindpunt, afhankelijk van uw omgeving.

        - **HTTP**: de standaard openbare en particuliere poorten **80**zijn. Opmerking: als u een private poort 80, wijzigen **$HTTPport = 80** in het HTTP-script.

        - **HTTPS**: de standaard openbare en particuliere poorten **443**zijn. Veiligheidsoverwegingen is privé poort wijzigen en uw firewall en de report server om de persoonlijke poort configureren. Zie voor meer informatie over de eindpunten, [het instellen van communicatie met een virtuele Machine](virtual-machines-windows-classic-setup-endpoints.md). Opmerking: als u een andere poort dan poort 443, wijzigt u de parameter **$HTTPsport = 443** in het script HTTPS.
    
    - Klik op volgende. ![volgende](./media/virtual-machines-windows-classic-ps-sql-report/IC692021.gif)

1. Behouden standaard **de VM-agent installeren** hebt geselecteerd op de laatste pagina van de wizard. De stappen in dit onderwerp maken geen gebruik van de VM-agent, maar als u van plan bent om deze VM VM agent en uitbreidingen kunt u verbeteren hij CM.  Zie voor meer informatie over de VM-agent, [Agent VM en extensies – deel 1](https://azure.microsoft.com/blog/2014/04/11/vm-agent-and-extensions-part-1/). Een van de standaard-extensies geïnstalleerd advertentie met is de extensie 'BGINFO' dat wordt weergegeven op het bureaublad VM, systeeminformatie over interne IP-adres en vrije schijfruimte.

1. Klik op voltooien. ![OK](./media/virtual-machines-windows-classic-ps-sql-report/IC660122.gif)

1. De **Status** van de VM als **eerste (creëren)** tijdens de bepaling en wordt vervolgens **uitgevoerd** wanneer de VM is ingericht en klaar voor gebruik.

## <a name="step-2-create-a-server-certificate"></a>Stap 2: Maak een servercertificaat

>[AZURE.NOTE] Als u geen HTTPS op de rapportserver vereist, kunt u **stap 2 overslaan** en Ga naar de sectie **script gebruiken voor het configureren van de report server en HTTP**. Het script HTTP gebruiken om snel de report server configureren en de report server is gereed voor gebruik.

HTTPS op de VM gebruikt, moet u een vertrouwd certificaat voor SSL. Afhankelijk van uw scenario kunt u een van de volgende twee methoden:

- Een geldig SSL-certificaat uitgegeven door een certificeringsinstantie (CA) en vertrouwd door Microsoft. De CA-basiscertificaten moeten worden gedistribueerd via het Microsoft Root Certificate Program. Zie voor meer informatie over dit programma voor [Windows en Windows Phone 8 SSL Root Certificate Program (lid CA's)](http://social.technet.microsoft.com/wiki/contents/articles/14215.windows-and-windows-phone-8-ssl-root-certificate-program-member-cas.aspx) en een [Inleiding tot het Microsoft Root Certificate Program](http://social.technet.microsoft.com/wiki/contents/articles/3281.introduction-to-the-microsoft-root-certificate-program.aspx).

- Een zelfondertekend certificaat. Zelfondertekende certificaten worden niet aanbevolen voor productieomgevingen.

### <a name="to-use-a-certificate-created-by-a-trusted-certificate-authority-ca"></a>Een certificaat dat is gemaakt door een vertrouwde certificeringsinstantie (CA) gebruiken

1. **Vraag een servercertificaat voor de website van een certificeringsinstantie**. 

    U kunt de Wizard Webservercertificaat voor het genereren van een certificaataanvraagbestand (Certreq.txt) die u naar een certificeringsinstantie verzendt of een aanvraag voor een on line certificeringsinstantie te genereren. Bijvoorbeeld Microsoft Certificate Services in Windows Server 2012. Afhankelijk van het garantieniveau dat uw servercertificaat moet bieden, is het enkele dagen tot enkele maanden duren voor de certificeringsinstantie de aanvraag goedkeurt en u een certificaatbestand ontvangt. 

    Zie de volgende onderwerpen voor meer informatie over het aanvragen van een servercertificaten: 
    
    - [Certreq](https://technet.microsoft.com/library/cc725793.aspx) [Certreq](https://technet.microsoft.com/library/cc725793.aspx)gebruikt.
    
    - Security Tools voor het beheer van Windows Server 2012.

    [Security Tools voor het beheer van Windows Server 2012](https://technet.microsoft.com/library/jj730960.aspx)

    >[AZURE.NOTE] Het veld **uitgegeven aan** van het SSL-certificaat van vertrouwd moet hetzelfde zijn als de **Cloud Service DNS-naam** die u voor de nieuwe VM gebruikt.

1. **Installeer het servercertificaat op de webserver**. De webserver is in dit geval de VM die fungeert als host voor de report server en de website is gemaakt in latere stappen wanneer u Reporting Services configureren. Zie [een servercertificaat installeren](https://technet.microsoft.com/library/cc740068)voor meer informatie over het installeren van het servercertificaat op de webserver met behulp van het certificaat MMC-module.
    
    Als u wilt dat het script dat is opgenomen in dit onderwerp gebruiken, om de rapportserver de waarde van de certificaten **vingerafdruk** is vereist als een parameter van het script. Zie de volgende sectie voor meer informatie over het verkrijgen van de blauwdruk van het certificaat.

1. Het servercertificaat aan de report server toewijzen. De toewijzing is voltooid in de volgende sectie als u de report server configureren.

### <a name="to-use-the-virtual-machines-self-signed-certificate"></a>Voor het gebruik van de virtuele Machines zelf-ondertekend certificaat

Een zelfondertekend certificaat is gemaakt op de VM wanneer de VM werd ingericht. Het certificaat heeft dezelfde naam als de naam van de VM DNS. Om te voorkomen dat certificaatfouten, is het vereist dat het certificaat vertrouwd op de VM zelf en voor alle gebruikers van de site wordt.

1. Als u de basis-CA van het certificaat op de lokale VM vertrouwt, moet u het certificaat toevoegen aan de **Vertrouwde basiscertificeringsinstanties**. Hier volgt een overzicht van de stappen die nodig zijn. Zie voor gedetailleerde stappen over het vertrouwen van de Certificeringsinstantie [een servercertificaat installeren](https://technet.microsoft.com/library/cc740068).

    1. Selecteer de VM vanuit Azure klassieke portal en klik op verbinden. Afhankelijk van de browserconfiguratie van uw, u gevraagd om op te slaan van een RDP-bestand voor de verbinding met de VM.
    
        ![verbinding maken met azure virtual machine](./media/virtual-machines-windows-classic-ps-sql-report/IC650112.gif) Gebruik de VM-gebruikersnaam, gebruikersnaam en wachtwoord die u hebt geconfigureerd bij het maken van de VM. 
    
        Bijvoorbeeld in de volgende afbeelding de VM-naam is **ssrsnativecloud** en de gebruikersnaam **' testuser '**.
        
        ![vm-aanmeldingsnaam zijn opgenomen](./media/virtual-machines-windows-classic-ps-sql-report/IC764111.png)
    
    1. Mmc.exe uitvoeren. Zie voor meer informatie [procedure: certificaten weergeven met de MMC-module](https://msdn.microsoft.com/library/ms788967.aspx).
    
    1. De module **certificaten** toevoegen in het menu console toepassing **bestand** , selecteert u **De Account** wanneer daarom wordt gevraagd en klik op **volgende**.
    
    1. Selecteer **Lokale Computer** te beheren en klik vervolgens op **Voltooien**.
    
    1. Klik op **Ok** en klik op de **-certificaten - persoonlijke** knooppunten en klik vervolgens op **certificaten**. Het certificaat wordt de naam van de DNS-naam van de VM en eindigt met **cloudapp.net**. Klik met de rechtermuisknop op de certificaatnaam en klik op **kopiëren**.
    
    1. Vouw het knooppunt van de **Vertrouwde basiscertificeringsinstanties** en klik met de rechtermuisknop op **certificaten** en klik vervolgens op **Plakken**.
    
    1. Als u wilt valideren, dubbelklikt u op de certificaatnaam van het onder **Vertrouwde basiscertificeringsinstanties** en controleer of er geen fouten zijn en u ziet dat uw certificaat. Als u het HTTPS-script dat is opgenomen in dit onderwerp gebruiken wilt, om de rapportserver de waarde van de certificaten **vingerafdruk** is vereist als een parameter van het script. **Om de vingerafdrukwaarde**, vult u de volgende. Er is ook een PowerShell voorbeeld voor het ophalen van de vingerafdruk in sectie [script gebruiken voor het configureren van de report server en HTTPS](#use-script-to-configure-the-report-server-and-HTTPS).
        
        1. Dubbelklik op de naam van het certificaat, bijvoorbeeld ssrsnativecloud.cloudapp.net.
        
        1. Klik op het tabblad **Details** .
        
        1. Klik op **vingerafdruk**. De waarde van de vingerafdruk wordt weergegeven in het veld details, bijvoorbeeld a6 08 3c df f9 0b f7 e3 7c 25 ed a4 ed 7e ac 91 9c 2c fb 2f.
        
        1. Kopieer de vingerafdruk en de waarde opslaan voor later of het script nu bewerken.
        
        1. (*) Voordat u het script uitvoert, verwijdert de spaties tussen de paren van waarden. Zo zou de vingerafdruk zeiden nu a6083cdff90bf7e37c25eda4ed7eac919c2cfb2f zijn.
        
        1. Het servercertificaat aan de report server toewijzen. De toewijzing is voltooid in de volgende sectie als u de report server configureren.

Als u een zelfondertekend certificaat voor SSL gebruikt, komt de hostnaam van de VM al overeen met de naam van het certificaat. De DNS-server van de machine is daarom wereldwijd al is geregistreerd en toegankelijk vanaf elke client.

## <a name="step-3-configure-the-report-server"></a>Stap 3: De Report Server configureren

Deze sectie helpt u bij de VM te configureren als een native-modus-rapportserver van Reporting Services. U kunt een van de volgende methoden de report server configureren:

- Gebruik de report server configureren met het script

- Configuration Manager gebruiken voor het configureren van de Report Server.

Zie de sectie [verbinding maken met de virtuele Machine en Reporting Services Configuration Manager starten](virtual-machines-windows-classic-ps-sql-bi.md#connect-to-the-virtual-machine-and-start-the-reporting-services-configuration-manager)voor meer gedetailleerde stappen.

**Opmerking verificatie:** Windows-verificatie is de aanbevolen verificatiemethode en is de standaardverificatie van Reporting Services. Alleen gebruikers die zijn geconfigureerd op de VM toegang tot Reporting Services en Reporting Services rollen toegewezen.

### <a name="use-script-to-configure-the-report-server-and-http"></a>Report server en HTTP configureren met behulp van script

Als u het Windows PowerShell-script wilt de report server configureren, moet u de volgende stappen uitvoeren. De configuratie omvat geen HTTPS HTTP:

1. Selecteer de VM vanuit Azure klassieke portal en klik op verbinden. Afhankelijk van de browserconfiguratie van uw, u gevraagd om op te slaan van een RDP-bestand voor de verbinding met de VM.

    ![verbinding maken met azure virtual machine](./media/virtual-machines-windows-classic-ps-sql-report/IC650112.gif) Gebruik de VM-gebruikersnaam, gebruikersnaam en wachtwoord die u hebt geconfigureerd bij het maken van de VM. 

    Bijvoorbeeld in de volgende afbeelding de VM-naam is **ssrsnativecloud** en de gebruikersnaam **' testuser '**.
    
    ![vm-aanmeldingsnaam zijn opgenomen](./media/virtual-machines-windows-classic-ps-sql-report/IC764111.png)

1. Open op de VM, **Windows PowerShell ISE** met beheerdersbevoegdheden. De PowerShell ISE wordt standaard geïnstalleerd op Windows server 2012. Het verdient dat u de ISE gebruiken in plaats van een standaard Windows PowerShell-venster, zodat het script in de ISE plakken, het script wijzigen en vervolgens het script uitvoeren.

1. Klik in het menu **Beeld** en klik op **Script-veld weergeven**in Windows PowerShell ISE.

1. Kopieer het script en het script in het Windows PowerShell ISE script-veld te plakken.

        ## This script configures a Native mode report server without HTTPS
        $ErrorActionPreference = "Stop"
        
        $server = $env:COMPUTERNAME
        $HTTPport = 80 # change the value if you used a different port for the private HTTP endpoint when the VM was created.
        
        ## Set PowerShell execution policy to be able to run scripts
        Set-ExecutionPolicy RemoteSigned -Force
        
        ## Utility method for verifying an operation's result
        function CheckResult
        {
            param($wmi_result, $actionname)
            if ($wmi_result.HRESULT -ne 0) {
                write-error "$actionname failed. Error from WMI: $($wmi_result.Error)"
            }
        }
        
        $starttime=Get-Date
        write-host -foregroundcolor DarkGray $starttime StartTime
        
        ## ReportServer Database name - this can be changed if needed
        $dbName='ReportServer'
        
        ## Register for MSReportServer_ConfigurationSetting
        ## Change the version portion of the path to "v11" to use the script for SQL Server 2012
        $RSObject = Get-WmiObject -class "MSReportServer_ConfigurationSetting" -namespace "root\Microsoft\SqlServer\ReportServer\RS_MSSQLSERVER\v12\Admin"
        
        ## Report Server Configuration Steps
        
        ## Setting the web service URL ##
        write-host -foregroundcolor green "Setting the web service URL"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
        
        ## SetVirtualDirectory for ReportServer site
            write-host 'Calling SetVirtualDirectory'
            $r = $RSObject.SetVirtualDirectory('ReportServerWebService','ReportServer',1033)
            CheckResult $r "SetVirtualDirectory for ReportServer"
        
        ## ReserveURL for ReportServerWebService - port $HTTPport (for local usage)
            write-host "Calling ReserveURL port $HTTPport"
            $r = $RSObject.ReserveURL('ReportServerWebService',"http://+:$HTTPport",1033)
            CheckResult $r "ReserveURL for ReportServer port $HTTPport" 
           
        ## Setting the Database ##
        write-host -foregroundcolor green "Setting the Database"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
        
        ## GenerateDatabaseScript - for creating the database
            write-host "Calling GenerateDatabaseCreationScript for database $dbName"
            $r = $RSObject.GenerateDatabaseCreationScript($dbName,1033,$false)
            CheckResult $r "GenerateDatabaseCreationScript"
            $script = $r.Script
        
        ## Execute sql script to create the database
            write-host 'Executing Database Creation Script'
            $savedcvd = Get-Location
            Import-Module SQLPS              ## this automatically changes to sqlserver provider
            Invoke-SqlCmd -Query $script
            Set-Location $savedcvd
          
        ## GenerateGrantRightsScript 
            $DBUser = "NT Service\ReportServer"
            write-host "Calling GenerateDatabaseRightsScript with user $DBUser"
            $r = $RSObject.GenerateDatabaseRightsScript($DBUser,$dbName,$false,$true)
            CheckResult $r "GenerateDatabaseRightsScript"
            $script = $r.Script
        
        ## Execute grant rights script
            write-host 'Executing Database Rights Script'
            $savedcvd = Get-Location
            cd sqlserver:\
            Invoke-SqlCmd -Query $script
            Set-Location $savedcvd
        
        ## SetDBConnection - uses Windows Service (type 2), username is ignored
            write-host "Calling SetDatabaseConnection server $server, DB $dbName"
            $r = $RSObject.SetDatabaseConnection($server,$dbName,2,'','')
            CheckResult $r "SetDatabaseConnection"  
        
        ## Setting the Report Manager URL ##
        
        write-host -foregroundcolor green "Setting the Report Manager URL"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
        
        ## SetVirtualDirectory for Reports (Report Manager) site
            write-host 'Calling SetVirtualDirectory'
            $r = $RSObject.SetVirtualDirectory('ReportManager','Reports',1033)
            CheckResult $r "SetVirtualDirectory"
        
        ## ReserveURL for ReportManager  - port $HTTPport
            write-host "Calling ReserveURL for ReportManager, port $HTTPport"
            $r = $RSObject.ReserveURL('ReportManager',"http://+:$HTTPport",1033)
            CheckResult $r "ReserveURL for ReportManager port $HTTPport"
        
        write-host -foregroundcolor green "Open Firewall port for $HTTPport"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
        
        ## Open Firewall port for $HTTPport
            New-NetFirewallRule -DisplayName “Report Server (TCP on port $HTTPport)” -Direction Inbound –Protocol TCP –LocalPort $HTTPport
            write-host "Added rule Report Server (TCP on port $HTTPport) in Windows Firewall"
        
        write-host 'Operations completed, Report Server is ready'
        write-host -foregroundcolor DarkGray $starttime StartTime
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time

1. Als u de VM met een HTTP-poort 80 gemaakt, wijzigt u de parameter $HTTPport = 80.

1. Het script is momenteel geconfigureerd voor Reporting Services. Als u wilt dat het script voor Reporting Services wordt uitgevoerd, het gedeelte van de versie van het pad naar de naamruimte "V11,:" op de instructie Get-WmiObject wijzigen

1. Het script uitvoeren.

**Validatie**: om te controleren dat de functionaliteit van de server eenvoudig rapport werkt, raadpleegt u de sectie [de configuratie controleren](#verify-the-configuration) verderop in dit onderwerp.

### <a name="use-script-to-configure-the-report-server-and-https"></a>Scripts gebruiken voor het configureren van de report server en HTTPS

Als u Windows PowerShell gebruiken voor het configureren van de report server, moet u de volgende stappen uitvoeren. De configuratie omvat HTTPS, niet het HTTP.

1. Selecteer de VM vanuit Azure klassieke portal en klik op verbinden. Afhankelijk van de browserconfiguratie van uw, u gevraagd om op te slaan van een RDP-bestand voor de verbinding met de VM.

    ![verbinding maken met azure virtual machine](./media/virtual-machines-windows-classic-ps-sql-report/IC650112.gif) Gebruik de VM-gebruikersnaam, gebruikersnaam en wachtwoord die u hebt geconfigureerd bij het maken van de VM. 

    Bijvoorbeeld in de volgende afbeelding de VM-naam is **ssrsnativecloud** en de gebruikersnaam **' testuser '**.

    ![vm-aanmeldingsnaam zijn opgenomen](./media/virtual-machines-windows-classic-ps-sql-report/IC764111.png)

1. Open op de VM, **Windows PowerShell ISE** met beheerdersbevoegdheden. De PowerShell ISE wordt standaard geïnstalleerd op Windows server 2012. Het verdient dat u de ISE gebruiken in plaats van een standaard Windows PowerShell-venster, zodat het script in de ISE plakken, het script wijzigen en vervolgens het script uitvoeren.

1. Als u actieve scripts, voert u de volgende Windows PowerShell-opdracht:

        Set-ExecutionPolicy RemoteSigned

    Vervolgens kunt u het volgende om te controleren of het beleid uitvoeren:

        Get-ExecutionPolicy

1. In **Windows PowerShell ISE**, klikt u op het menu **Beeld** en klik op **Script-veld weergegeven**.

1. Het volgende script kopiëren en plakken in het Windows PowerShell ISE script-veld.

        ## This script configures the report server, including HTTPS
        $ErrorActionPreference = "Stop"
        $httpsport=443 # modify if you used a different port number when the HTTPS endpoint was created.
        
        # You can run the following command to get (.cloudapp.net certificates) so you can copy the thumbprint / certificate hash
        #dir cert:\LocalMachine -rec | Select-Object * | where {$_.issuer -like "*cloudapp*" -and $_.pspath -like "*root*"} | select dnsnamelist, thumbprint, issuer
        #
        # The certifacte hash is a REQUIRED parameter
        $certificatehash="" 
        # the certificate hash should not contain spaces
        
        if ($certificatehash.Length -lt 1) 
        {
            write-error "certificatehash is a required parameter"
        } 
        # Certificates should be all lower case
        $certificatehash=$certificatehash.ToLower()
        $server = $env:COMPUTERNAME
        # If the certificate is not a wildcard certificate, comment out the following line, and enable the full $DNSNAme reference.
        $DNSName="+"
        #$DNSName="$server.cloudapp.net"
        $DNSNameAndPort = $DNSName + ":$httpsport"
        
        ## Utility method for verifying an operation's result
        function CheckResult
        {
            param($wmi_result, $actionname)
            if ($wmi_result.HRESULT -ne 0) {
                write-error "$actionname failed. Error from WMI: $($wmi_result.Error)"
            }
        }
        
        $starttime=Get-Date
        write-host -foregroundcolor DarkGray $starttime StartTime
        
        ## ReportServer Database name - this can be changed if needed
        $dbName='ReportServer'
        
        write-host "The script will use $DNSNameAndPort as the DNS name and port" 
        
        ## Register for MSReportServer_ConfigurationSetting
        ## Change the version portion of the path to "v11" to use the script for SQL Server 2012
        $RSObject = Get-WmiObject -class "MSReportServer_ConfigurationSetting" -namespace "root\Microsoft\SqlServer\ReportServer\RS_MSSQLSERVER\v12\Admin"
        
        ## Reporting Services Report Server Configuration Steps
        
        ## 1. Setting the web service URL ##
        write-host -foregroundcolor green "Setting the web service URL"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
        
        ## SetVirtualDirectory for ReportServer site
            write-host 'Calling SetVirtualDirectory'
            $r = $RSObject.SetVirtualDirectory('ReportServerWebService','ReportServer',1033)
            CheckResult $r "SetVirtualDirectory for ReportServer"
        
        ## ReserveURL for ReportServerWebService - port 80 (for local usage)
            write-host 'Calling ReserveURL port 80'
            $r = $RSObject.ReserveURL('ReportServerWebService','http://+:80',1033)
            CheckResult $r "ReserveURL for ReportServer port 80" 
        
        ## ReserveURL for ReportServerWebService - port $httpsport
            write-host "Calling ReserveURL port $httpsport, for URL: https://$DNSNameAndPort"
            $r = $RSObject.ReserveURL('ReportServerWebService',"https://$DNSNameAndPort",1033)
            CheckResult $r "ReserveURL for ReportServer port $httpsport" 
        
        ## CreateSSLCertificateBinding for ReportServerWebService port $httpsport
            write-host "Calling CreateSSLCertificateBinding port $httpsport, with certificate hash: $certificatehash"
            $r = $RSObject.CreateSSLCertificateBinding('ReportServerWebService',$certificatehash,'0.0.0.0',$httpsport,1033)
            CheckResult $r "CreateSSLCertificateBinding for ReportServer port $httpsport" 
            
        ## 2. Setting the Database ##
        write-host -foregroundcolor green "Setting the Database"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
        
        ## GenerateDatabaseScript - for creating the database
            write-host "Calling GenerateDatabaseCreationScript for database $dbName"
            $r = $RSObject.GenerateDatabaseCreationScript($dbName,1033,$false)
            CheckResult $r "GenerateDatabaseCreationScript"
            $script = $r.Script
        
        ## Execute sql script to create the database
            write-host 'Executing Database Creation Script'
            $savedcvd = Get-Location
            Import-Module SQLPS                    ## this automatically changes to sqlserver provider
            Invoke-SqlCmd -Query $script
            Set-Location $savedcvd
          
        ## GenerateGrantRightsScript 
            $DBUser = "NT Service\ReportServer"
            write-host "Calling GenerateDatabaseRightsScript with user $DBUser"
            $r = $RSObject.GenerateDatabaseRightsScript($DBUser,$dbName,$false,$true)
            CheckResult $r "GenerateDatabaseRightsScript"
            $script = $r.Script
        
        ## Execute grant rights script
            write-host 'Executing Database Rights Script'
            $savedcvd = Get-Location
            cd sqlserver:\
            Invoke-SqlCmd -Query $script
            Set-Location $savedcvd
        
        ## SetDBConnection - uses Windows Service (type 2), username is ignored
            write-host "Calling SetDatabaseConnection server $server, DB $dbName"
            $r = $RSObject.SetDatabaseConnection($server,$dbName,2,'','')
            CheckResult $r "SetDatabaseConnection"  
        
        ## 3. Setting the Report Manager URL ##
        
        write-host -foregroundcolor green "Setting the Report Manager URL"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
        
        ## SetVirtualDirectory for Reports (Report Manager) site
            write-host 'Calling SetVirtualDirectory'
            $r = $RSObject.SetVirtualDirectory('ReportManager','Reports',1033)
            CheckResult $r "SetVirtualDirectory"
        
        ## ReserveURL for ReportManager  - port 80
            write-host 'Calling ReserveURL for ReportManager, port 80'
            $r = $RSObject.ReserveURL('ReportManager','http://+:80',1033)
            CheckResult $r "ReserveURL for ReportManager port 80"
        
        ## ReserveURL for ReportManager - port $httpsport
            write-host "Calling ReserveURL port $httpsport, for URL: https://$DNSNameAndPort"
            $r = $RSObject.ReserveURL('ReportManager',"https://$DNSNameAndPort",1033)
            CheckResult $r "ReserveURL for ReportManager port $httpsport" 
        
        ## CreateSSLCertificateBinding for ReportManager port $httpsport
            write-host "Calling CreateSSLCertificateBinding port $httpsport with certificate hash: $certificatehash"
            $r = $RSObject.CreateSSLCertificateBinding('ReportManager',$certificatehash,'0.0.0.0',$httpsport,1033)
            CheckResult $r "CreateSSLCertificateBinding for ReportManager port $httpsport" 
        
        write-host -foregroundcolor green "Open Firewall port for $httpsport"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
        
        ## Open Firewall port for $httpsport
            New-NetFirewallRule -DisplayName “Report Server (TCP on port $httpsport)” -Direction Inbound –Protocol TCP –LocalPort $httpsport
            write-host "Added rule Report Server (TCP on port $httpsport) in Windows Firewall"
        
        write-host 'Operations completed, Report Server is ready'
        write-host -foregroundcolor DarkGray $starttime StartTime
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time

1. De parameter **$certificatehash** in het script te wijzigen:

    - Dit is een **verplichte** parameter. Als u de waarde van het certificaat niet uit de vorige stap hebt opgeslagen, gebruikt u een van de volgende methoden de certificaat-hash van de vingerafdruk certificaten kopiëren.:

        Op de VM, opent u Windows PowerShell ISE en voer de volgende opdracht uit:

            dir cert:\LocalMachine -rec | Select-Object * | where {$_.issuer -like "*cloudapp*" -and $_.pspath -like "*root*"} | select dnsnamelist, thumbprint, issuer

        De uitvoer ziet er ongeveer als volgt. Als het script retourneert een lege regel, de VM niet voorzien van een certificaat bijvoorbeeld is geconfigureerd, raadpleegt u de sectie [de virtuele Machines zelfondertekende certificaat gebruiken](#to-use-the-virtual-machines-self-signed-certificate).
    
    OR
    
    - Uitvoeren op de VM mmc.exe en voeg de module **certificaten** .
    
    - Dubbelklik op de naam van het certificaat onder het knooppunt voor **Vertrouwde basiscertificeringsinstanties** . Als u een zelf-ondertekend certificaat van VM gebruikt, wordt het certificaat wordt de naam van de DNS-naam van de VM en eindigt met **cloudapp.net**.
    
    - Klik op het tabblad **Details** .
    
    - Klik op **vingerafdruk**. De waarde van de vingerafdruk wordt weergegeven in het veld details, bijvoorbeeld af 11 60 b6 4b 28 8 d 89 0a 82 12 ff 6b a9 c3 66 4f 31 90 48
    
    - **Voordat u het script uitvoert**, verwijdert de spaties tussen de paren van waarden. Bijvoorbeeld af1160b64b288d890a8212ff6ba9c3664f319048

1. Wijzig de parameter **$httpsport** : 

    - Als u poort 443 voor het HTTPS-eindpunt gebruikt, hoeft niet u deze parameter in het script bijwerken. Gebruik anders de poortwaarde die u hebt geselecteerd toen u het HTTPS-persoonlijke eindpunt geconfigureerd op de VM.

1. Wijzig de parameter **$DNSName** : 

    - Het script is geconfigureerd voor een jokerteken certificaat $DNSName = "+". Als u niet wilt configureren voor een certificaat jokertekens binding reactie op $DNSName ="+"en de volgende regel, de volledige $DNSNAme referentie, ## $DNSName="$server.cloudapp.net inschakelen".

        Wijzig de waarde $DNSName als u niet wilt gebruiken DNS-naam van de virtuele machine voor Reporting Services. Als u de parameter gebruikt, moet deze naam ook gebruiken om het certificaat en u de naam wereldwijd op een DNS-server registreren.

1. Het script is momenteel geconfigureerd voor Reporting Services. Als u wilt dat het script voor Reporting Services wordt uitgevoerd, het gedeelte van de versie van het pad naar de naamruimte "V11,:" op de instructie Get-WmiObject wijzigen

1. Het script uitvoeren.

**Validatie**: om te controleren dat de functionaliteit van de server eenvoudig rapport werkt, raadpleegt u de sectie [de configuratie controleren](#verify-the-connection) verderop in dit onderwerp. Binding open een opdrachtprompt met beheerdersbevoegdheden en voer de volgende opdracht om te controleren of het certificaat:

    netsh http show sslcert

Het resultaat zal zijn:

    IP:port                      : 0.0.0.0:443

    Certificate Hash             : f98adf786994c1e4a153f53fe20f94210267d0e7

### <a name="use-configuration-manager-to-configure-the-report-server"></a>Configuration Manager gebruiken voor het configureren van de Report Server

Als u niet de report server configureren PowerShell script uitvoeren wilt, de stappen in deze sectie gebruiken om Configuratiebeheer van Reporting Services native-modus te configureren de report server.

1. Selecteer de VM vanuit Azure klassieke portal en klik op verbinden. Gebruik de gebruikersnaam en het wachtwoord die u hebt geconfigureerd bij het maken van de VM.

    ![verbinding maken met azure virtual machine](./media/virtual-machines-windows-classic-ps-sql-report/IC650112.gif)

1. Windows update uitvoeren en installeren van updates voor VM. Als het opnieuw opstarten van de VM vereist is, de VM opnieuw en opnieuw verbinding maken met de VM vanaf de klassieke Azure portal.

1. Uit het menu Start op de VM, typt u **Reporting Services** en **Reporting Services Configuration Manager**openen.

1. Laat de standaardwaarden voor de **Servernaam** en **Het serverexemplaar rapport**. Klik op **verbinden**.

1. Klik in het linkerdeelvenster op **URL webservice**.

1. RS is standaard geconfigureerd voor HTTP-poort 80 met IP "Alle toegewezen". HTTPS toevoegen:

    1. In **SSL-certificaat**: Selecteer het certificaat dat u gebruiken wilt, bijvoorbeeld [naam VM]. cloudapp.net. Als er geen certificaten worden vermeld, raadpleegt u de sectie **stap 2: Maak een servercertificaat** voor meer informatie over het installeren en het vertrouwen van het certificaat op de VM.
    
    1. Klik onder **SSL-poort**: 443 kiezen. Als u de persoonlijke HTTPS-eindpunt in VM met een andere private poort hebt geconfigureerd, hier die waarde gebruiken.
    
    1. Klik op **toepassen** en wacht totdat de bewerking is voltooid.

1. Klik in het linkerdeelvenster op **Database**.

    1. Klik op **wijziging Databas**e.
    
    1. Klik op **een report server-database maken** en klik vervolgens op **volgende**.
    
    1. Laat de standaard **Servernaam**: naam als de VM en laat de standaard **Type verificatie** als **Huidige gebruiker** – **Geïntegreerde beveiliging**. Klik op **volgende**.
    
    1. Laat de standaard **Naam van de Database** **ReportServer** en klik op **volgende**.
    
    1. Laat het standaard **Type verificatie** als de **Service de referenties** en klik op **volgende**.
    
    1. Klik op de pagina **Samenvatting** op **volgende** .
    
    1. Wanneer de configuratie voltooid is, klikt u op **Voltooien**.

1. Klik in het linkerdeelvenster op **URL van Report Manager**. Laat de standaard **Virtuele map** als **rapporten** en klik op **toepassen**.

1. Klik op **Afsluiten** om af te sluiten van Reporting Services Configuration Manager.

## <a name="step-4-open-windows-firewall-port"></a>Stap 4: Open Windows Firewall poort

>[AZURE.NOTE] Als u een van de scripts voor het configureren van de report server gebruikt, kunt u deze sectie overslaan. Het script opgenomen een stap om de firewallpoort te openen. De standaardwaarde is poort 80 voor HTTP en poort 443 voor HTTPS.

Om extern verbinding maken met Report Manager of de Report Server op de virtuele machine, is een TCP-eindpunt vereist op de VM. Het is vereist om dezelfde poort te openen in de firewall van de VM. Het eindpunt is gemaakt toen de VM werd ingericht.

Deze sectie bevat algemene informatie over het openen van de firewallpoort. Zie voor meer informatie, [een Firewall voor toegang tot rapporten-Server configureren](https://technet.microsoft.com/library/bb934283.aspx)

>[AZURE.NOTE] Als u het script voor het configureren van de report server gebruikt, kunt u deze sectie overslaan. Het script opgenomen een stap om de firewallpoort te openen.

Als u een persoonlijke poort voor HTTPS dan 443 hebt geconfigureerd, wijzigt u het volgende script op de juiste manier. Poort **443** in Windows Firewall openen, voert u het volgende:

1. Open een Windows PowerShell-venster met beheerdersbevoegdheden.

1. Als u een andere poort dan 443 gebruikt wanneer u het HTTPS-eindpunt op de VM hebt ingesteld, werken de poort in de volgende opdracht en vervolgens de opdracht uitvoeren:

        New-NetFirewallRule -DisplayName “Report Server (TCP on port 443)” -Direction Inbound –Protocol TCP –LocalPort 443

1. Wanneer de opdracht is voltooid, **Ok** wordt weergegeven in de MS-DOS-prompt.

Om te controleren dat de poort is geopend, opent u een Windows PowerShell-venster en voer de volgende opdracht uit:

    get-netfirewallrule | where {$_.displayname -like "*report*"} | select displayname,enabled,action

## <a name="verify-the-configuration"></a>De configuratie controleren

De browser openen met Administrator-bevoegdheden om te controleren dat de functionaliteit van de server eenvoudig rapport nu werkt, en Ga naar de volgende rapport server ad report manager URL's:

- Op de VM, bladert u naar de report server-URL:

        http://localhost/reportserver

- Op de VM, bladert u naar de URL van report Manager:

        http://localhost/Reports

- Blader naar het **externe** rapport Manager op de VM vanaf de lokale computer. De DNS-naam in het volgende voorbeeld zo nodig bijgewerkt. Wanneer u wordt gevraagd om een wachtwoord, gebruikt u de beheerdersreferenties die u hebt gemaakt toen de VM werd ingericht. De gebruikersnaam van de wordt in de [domein]\[gebruikersnaam]-indeling, waarin het domein zich VM naam van de computer, bijvoorbeeld ssrsnativecloud\testuser. Als u geen HTTP-**S**, verwijder de **s** in de URL. Zie de volgende sectie voor meer informatie over het maken van extra gebruikers op een VM.

        https://ssrsnativecloud.cloudapp.net/Reports

- Blader naar de externe report server-URL van uw lokale computer. De DNS-naam in het volgende voorbeeld zo nodig bijgewerkt. Als u niet van HTTPS gebruikmaakt, verwijdert u de s in de URL.

        https://ssrsnativecloud.cloudapp.net/ReportServer

## <a name="create-users-and-assign-roles"></a>Gebruikers en rollen toewijzen

Na het configureren en controleren van de report server, is een algemene administratieve taak het maken van een of meer gebruikers en gebruikers toewijzen aan rollen voor Reporting Services. Zie de volgende onderwerpen voor meer informatie:

- [Een lokale gebruikersaccount maken](https://technet.microsoft.com/library/cc770642.aspx)

- [Verleen toegang tot een rapportserver (Report Manager)](https://msdn.microsoft.com/library/ms156034.aspx))

- [Maken en beheren van roltoewijzingen](https://msdn.microsoft.com/library/ms155843.aspx)

## <a name="to-create-and-publish-reports-to-the-azure-virtual-machine"></a>Voor het maken en publiceren van rapporten op de Azure Virtual Machine

De volgende tabel bevat een overzicht van enkele van de opties beschikbaar voor het publiceren van een computer op gebouwen bestaande rapporten met de report server gehost op Microsoft Azure Virtual Machine:

- **RS.exe script**: gebruik RS.exe script van elementen op een rapport en bestaande kopiëren naar uw Microsoft Azure Virtual Machine. Zie de sectie 'Native modus naar de Native-modus-Microsoft Azure Virtual Machine' in [monster Reporting Services rs.exe-Script om te migreren van inhoud tussen Servers rapport](https://msdn.microsoft.com/library/dn531017.aspx)voor meer informatie.

- **Report Builder**: de virtuele machine bevat de Klik-eenmaal een versie van Microsoft SQL Server Report Builder. Rapport opbouwfunctie voor de eerste keer starten op de virtuele machine:

    1. Start uw browser met beheerdersbevoegdheden.
    
    1. Ga naar Rapportbeheer op de virtuele machine en klikt u op **Report Builder** op het lint.

    Zie voor meer informatie, [installeren, Uninstalling, en ondersteunende Report Builder](https://technet.microsoft.com/library/dd207038.aspx).

- **SQL Server Data Tools: VM**: als u de VM met SQL Server 2012 gemaakt en vervolgens SQL Server Data Tools op de virtuele machine is geïnstalleerd en kan worden gebruikt voor het **Rapport Server-projecten** en rapporten op de virtuele machine maken. SQL Server Data Tools kunt u de rapporten publiceren naar de report server op de virtuele machine.
    
    Als u de VM met SQL server 2014 hebt gemaakt, kunt u SQL Server Data Tools - BI voor visual Studio te installeren. Zie de volgende onderwerpen voor meer informatie:

    - [Microsoft SQL Server Data Tools - Business Intelligence voor Visual Studio 2013](https://www.microsoft.com/download/details.aspx?id=42313)

    - [Microsoft SQL Server Data Tools - Business Intelligence voor Visual Studio 2012](https://www.microsoft.com/download/details.aspx?id=36843)

    - [Hulpmiddelen voor gegevens van SQL Server en SQL Server Business Intelligence (BI SSDT)](http://curah.microsoft.com/30004/sql-server-data-tools-ssdt-and-sql-server-business-intelligence)

- **SQL Server Data Tools: externe**: op de lokale computer een Reporting Services-project in SQL Server Data Tools met Reporting Services-rapporten te maken. Het project verbinding maken met de URL van de webservice configureren.

    ![SSDT Projecteigenschappen voor SQL Server Reporting Services-project](./media/virtual-machines-windows-classic-ps-sql-report/IC650114.gif)

- **Gebruik script**: rapportinhoud kopiëren met behulp van script. Zie [voorbeeld Reporting Services rs.exe-Script om te migreren van inhoud tussen Servers rapport](https://msdn.microsoft.com/library/dn531017.aspx)voor meer informatie.

## <a name="minimize-cost-if-you-are-not-using-the-vm"></a>Als u geen van de VM gebruikmaakt kosten minimaliseren

>[AZURE.NOTE] Afgesloten om te minimaliseren kosten voor uw Azure virtuele Machines niet in gebruik, de VM vanaf de klassieke Azure portal. Als u het energiebeheer van Windows in een VM VM afgesloten, worden u nog steeds hetzelfde bedrag aangerekend voor de VM. Om de kosten verminderen, moet u de VM in Azure klassieke portal afgesloten. Als u niet langer de VM, moet u de VM en de bijbehorende VHD-bestanden om te voorkomen dat de kosten voor opslag te verwijderen. Zie de sectie Veelgestelde vragen op [Virtuele Machines prijzen Details](https://azure.microsoft.com/pricing/details/virtual-machines/)voor meer informatie.

## <a name="more-information"></a>Meer informatie

### <a name="resources"></a>Bronnen

- Zie [Windows PowerShell gebruiken voor het maken van een Azure VM met SQL Server BI en SharePoint 2013](https://msdn.microsoft.com/library/azure/dn385843.aspx)voor vergelijkbare inhoud gerelateerd aan een distributie naar één server met SQL Server Business Intelligence en SharePoint 2013.

- Zie voor soortgelijke inhoud gerelateerd aan een distributie met meerdere servers van Business Intelligence voor SQL Server en SharePoint 2013, [Implementatie van SQL Server Business Intelligence in Azure virtuele Machines](https://msdn.microsoft.com/library/dn321998.aspx).

- Zie voor algemene informatie over de implementatie van SQL Server Business Intelligence in Azure virtuele Machines, [SQL Server Business Intelligence in Azure virtuele Machines](virtual-machines-windows-classic-ps-sql-bi.md).

- Voor meer informatie over de kosten van Azure kosten berekenen, Zie het tabblad virtuele Machines van [Azure prijzen Rekenmachine](https://azure.microsoft.com/pricing/calculator/?scenario=virtual-machines).

### <a name="community-content"></a>Community-inhoud

- Zie voor stapsgewijze instructies voor het maken van een eigen Reporting Services report-server modus zonder script te gebruiken, [Waarop SQL Reporting Service Azure Virtual Machine](http://adititechnologiesblog.blogspot.in/2012/07/hosting-sql-reporting-service-on-azure.html).

### <a name="links-to-other-resources-for-sql-server-in-azure-vms"></a>Koppelingen naar andere bronnen voor SQL Server in Azure VMs

[SQL Server op Azure virtuele Machines overzicht](virtual-machines-windows-sql-server-iaas-overview.md)
