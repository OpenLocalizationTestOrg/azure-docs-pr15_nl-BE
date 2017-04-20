<properties 
   pageTitle="Extern verbinding maken met uw apparaat StorSimple | Microsoft Azure"
   description="Wordt uitgelegd hoe u het apparaat voor extern beheer configureren en verbinding maken met Windows PowerShell voor StorSimple via HTTP of HTTPS."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="06/21/2016"
   ms.author="alkohli" />

# <a name="connect-remotely-to-your-storsimple-device"></a>Extern verbinding maken met uw apparaat StorSimple

## <a name="overview"></a>Overzicht

U kunt Windows PowerShell remoting verbinding maken met uw apparaat StorSimple. Als u op deze manier verbinding maakt, ziet u een menu. (Ziet u een menu alleen als u de seriële console op het apparaat om verbinding te maken.) Met Windows PowerShell remoting, de verbinding met een specifieke runspace. U kunt ook de taal opgeven. 

Ga voor meer informatie over het beheren van uw apparaat met Windows PowerShell remoting [Gebruik Windows](storsimple-windows-powershell-administration.md)PowerShell voor StorSimple voor het beheren van uw apparaat StorSimple.

In deze zelfstudie wordt uitgelegd hoe u het apparaat voor extern beheer configureren en vervolgens verbinding maken met Windows PowerShell voor StorSimple. U kunt HTTP- of HTTPS-verbinding via externe toegang van Windows PowerShell. Echter, wanneer u verbinding maken met Windows PowerShell voor StorSimple zijn beslissing, het volgende overwegen: 

- Rechtstreeks verbinding maken met de seriële console apparaat veilig is, maar geen verbinding maken met de seriële console via netwerk-switches is. Wees voorzichtig met het over de beveiligingsrisico's bij het verbinden met de seriële console apparaat via netwerk-switches. 

- Verbinding maken via een HTTP-sessie biedt meer beveiliging dan een verbinding via de seriële console via het netwerk mogelijk. Hoewel dit niet de meest veilige methode is, is het aanvaardbaar op vertrouwde netwerken. 

- Verbinding maken via een HTTPS-sessie met een zelf-ondertekend certificaat is de veiligste en de aanbevolen optie.

U kunt extern verbinding maken met de interface van Windows PowerShell. Externe toegang tot het apparaat StorSimple via de Windows PowerShell-interface is echter niet standaard ingeschakeld. U moet extern beheer op het apparaat eerst inschakelen en vervolgens op de client die wordt gebruikt voor toegang tot het apparaat.

In dit artikel beschreven stappen zijn uitgevoerd op een host-computer met Windows Server 2012 R2.

## <a name="connect-through-http"></a>Verbinding maken via HTTP

Verbinding maken met Windows PowerShell voor StorSimple via een HTTP-sessie biedt meer beveiliging dan een verbinding via de seriële console van uw apparaat StorSimple. Hoewel dit niet de meest veilige methode is, is het aanvaardbaar op vertrouwde netwerken.

U kunt de Azure klassieke portal of de seriële console extern beheer configureren. Selecteer een van de volgende procedures:

- [De klassieke Azure portal gebruiken voor extern beheer via HTTP inschakelen](#use-the-azure-classic-portal-to-enable-remote-management-over-http)

- [Extern beheer via HTTP inschakelen via de seriële console](#use-the-serial-console-to-enable-remote-management-over-http)

Nadat u extern beheer inschakelt, gebruik de volgende procedure voor het voorbereiden van de client voor verbinding met een extern.

- [De client voor verbinding met extern voorbereiden](#prepare-the-client-for-remote-connection)

### <a name="use-the-azure-classic-portal-to-enable-remote-management-over-http"></a>De klassieke Azure portal gebruiken voor extern beheer via HTTP inschakelen 

De volgende stappen uitvoeren in de Azure klassieke portal voor extern beheer via HTTP inschakelen.

#### <a name="to-enable-remote-management-through-the-azure-classic-portal"></a>Extern beheer via de klassieke Azure portal inschakelen

1. Toegang tot **apparaten** > **configureren** voor uw apparaat.

2. Ga naar de sectie **Extern beheer** .

3. **Extern beheer inschakelen** op **Ja**ingesteld.

4. U kunt nu verbinding maken via HTTP. (De standaardwaarde is via HTTPS verbinding maken). Zorg ervoor dat HTTP is ingeschakeld.

    >[AZURE.NOTE] Verbinding maken via HTTP is op vertrouwde netwerken alleen aanvaardbaar.

6. Klik op **Opslaan** onder aan de pagina.

### <a name="use-the-serial-console-to-enable-remote-management-over-http"></a>Extern beheer via HTTP inschakelen via de seriële console

De volgende stappen uitvoeren op de seriële console apparaat extern beheer inschakelen.

#### <a name="to-enable-remote-management-through-the-device-serial-console"></a>Extern beheer via de seriële console apparaat inschakelen

1. In de seriële console-menu de optie 1. Ga voor meer informatie over het gebruik van de seriële console op het apparaat te [verbinden met Windows PowerShell voor StorSimple via de seriële console apparaat](storsimple-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-device-serial-console).

2. Typ het volgende achter de opdrachtprompt:`Enable-HcsRemoteManagement –AllowHttp`

3. U wordt gewaarschuwd over de beveiligingslekken van via HTTP verbinding maken met het apparaat. Bevestig desgevraagd door **Y**te typen.

4. Controleren of HTTP is ingeschakeld door te typen:`Get-HcsSystem`

5. Controleer of het veld **RemoteManagementMode** wordt **HttpsAndHttpEnabled**. De volgende afbeelding ziet deze instellingen in stopverf.

     ![Serie ingeschakeld HTTP- en HTTPS](./media/storsimple-remote-connect/HCS_SerialHttpsAndHttpEnabled.png)

### <a name="prepare-the-client-for-remote-connection"></a>De client voor verbinding met extern voorbereiden

De volgende stappen uitvoeren op de client voor extern beheer inschakelen.

#### <a name="to-prepare-the-client-for-remote-connection"></a>De client voor verbinding met extern voorbereiden

1. Een Windows PowerShell-sessie starten als administrator.

2. Typ de volgende opdracht het IP-adres van het apparaat StorSimple toevoegen aan de lijst met vertrouwde hosts van de client: 

     `Set-Item wsman:\localhost\Client\TrustedHosts <device_ip> -Concatenate -Force`

     <*Device_ip*> vervangt door het IP-adres van het apparaat; bijvoorbeeld: 

     `Set-Item wsman:\localhost\Client\TrustedHosts 10.126.173.90 -Concatenate -Force`

3. Typ de volgende opdracht om de inloggegevens in een variabele opslaan: 

     *$cred = get-Credential.*

4. In het dialoogvenster dat wordt weergegeven:

    1. Typ de gebruikersnaam in de volgende notatie: *device_ip\SSAdmin*.
    2. Typ het administrator-wachtwoord voor het apparaat dat is ingesteld als het apparaat is geconfigureerd met de wizard setup. Het standaardwachtwoord is *Wachtwoord1*.

7. Start een Windows PowerShell-sessie op het apparaat door de volgende opdracht:

     `Enter-PSSession -Credential $cred -ConfigurationName SSAdminConsole -ComputerName <device_ip>`

     >[AZURE.NOTE] Toevoegen als u wilt maken voor gebruik in een Windows PowerShell-sessie met het virtuele apparaat StorSimple, de `–Port` parameter en de openbare poort die u in externe toegang geconfigureerd voor virtuele toestel StorSimple opgeven.

     Op dit punt moet u een actieve externe Windows PowerShell-sessie met het apparaat.

    ![PowerShell remoting via HTTP](./media/storsimple-remote-connect/HCS_PSRemotingUsingHTTP.png)

## <a name="connect-through-https"></a>Verbinding maken via HTTPS

Verbinding maken met Windows PowerShell voor StorSimple via een HTTPS-sessie is de veiligste en aanbevolen methode van externe verbindingen met uw Microsoft Azure StorSimple apparaat. De volgende procedures wordt uitgelegd hoe de seriële console en de client-computer zodanig instellen dat u kunt HTTPS verbinding maken met Windows PowerShell voor StorSimple.

U kunt de Azure klassieke portal of de seriële console extern beheer configureren. Selecteer een van de volgende procedures:

- [De klassieke Azure portal gebruiken voor extern beheer inschakelen via HTTPS](#use-the-azure-classic-portal-to-enable-remote-management-over-https)

- [Gebruik de seriële console extern beheer inschakelen via HTTPS](#use-the-serial-console-to-enable-remote-management-over-https)

Nadat u extern beheer inschakelt, gebruik de volgende procedures voor het voorbereiden van de host voor een extern beheer en vanaf de externe host verbinding maken met het apparaat.

- [Voorbereiden van de host voor extern beheer](#prepare-the-host-for-remote-management)

- [Verbinding maken met het apparaat van de externe host](#connect-to-the-device-from-the-remote-host)

### <a name="use-the-azure-classic-portal-to-enable-remote-management-over-https"></a>De klassieke Azure portal gebruiken voor extern beheer inschakelen via HTTPS

De volgende stappen uitvoeren in de Azure klassieke portal voor extern beheer inschakelen via HTTPS.

#### <a name="to-enable-remote-management-over-https-from-the-azure-classic-portal"></a>Extern beheer inschakelen via HTTPS via de klassieke Azure portal

1. Toegang tot **apparaten** > **configureren** voor uw apparaat.

2. Ga naar de sectie **Extern beheer** .

3. **Extern beheer inschakelen** op **Ja**ingesteld.

4. U kunt nu verbinding maken met behulp van HTTPS. (De standaardwaarde is via HTTPS verbinding maken). Zorg ervoor dat HTTPS is geselecteerd. 

5. Klik op **extern beheer-certificaat downloaden**. Geef een locatie voor het opslaan van dit bestand. U moet dit certificaat op de client of de host-computer die u gebruiken wilt om verbinding met het apparaat te installeren.

6. Klik op **Opslaan** onder aan de pagina.

### <a name="use-the-serial-console-to-enable-remote-management-over-https"></a>Gebruik de seriële console extern beheer inschakelen via HTTPS

De volgende stappen uitvoeren op de seriële console apparaat extern beheer inschakelen.

#### <a name="to-enable-remote-management-through-the-device-serial-console"></a>Extern beheer via de seriële console apparaat inschakelen

1. In de seriële console-menu de optie 1. Ga voor meer informatie over het gebruik van de seriële console op het apparaat te [verbinden met Windows PowerShell voor StorSimple via de seriële console apparaat](storsimple-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-device-serial-console).

2. Typ het volgende achter de opdrachtprompt: 

     `Enable-HcsRemoteManagement`

    Op het apparaat moet hierdoor HTTPS.

3. Controleer of HTTPS is ingeschakeld door te typen: 

     `Get-HcsSystem`

    Controleer of het veld **RemoteManagementMode** wordt **HttpsEnabled**. De volgende afbeelding ziet deze instellingen in stopverf.

     ![Seriële HTTPS ingeschakeld](./media/storsimple-remote-connect/HCS_SerialHttpsEnabled.png)

4. Uit de resultaten van `Get-HcsSystem`, het serienummer van het apparaat kopiëren en opslaan voor later gebruik.

    >[AZURE.NOTE] Het serienummer wordt toegewezen aan de CN-naam in het certificaat.

5. Beheer van externe certificaten verkrijgen door te typen: 
 
     `Get-HcsRemoteManagementCert`

    Een certificaat met de volgende strekking wordt weergegeven.

    ![Extern beheer certificaat ophalen](./media/storsimple-remote-connect/HCS_GetRemoteManagementCertificate.png)

5. Kopieer de gegevens in het certificaat van **---BEGIN certificaat---** **---** beëindigen certificaat---in een teksteditor zoals Kladblok en opslaan als een cer-bestand. (Kopieert u dit bestand naar de externe host bij het voorbereiden van de host.)

    >[AZURE.NOTE] Om een nieuw certificaat genereert, gebruikt u de `Set-HcsRemoteManagementCert` cmdlet.

### <a name="prepare-the-host-for-remote-management"></a>Voorbereiden van de host voor extern beheer

Als de hostcomputer voorbereiden voor een externe verbinding die gebruikmaakt van een HTTPS-sessie, moet u de volgende procedures uitvoeren:

- Het [importeren van het cer-bestand in het archief Basiscertificeringsinstanties van de client of de externe host](#to-import-the-certificate-on-the-remote-host).

- [De serienummers van het apparaat aan het hosts-bestand op uw externe host toevoegen](#to-add-device-serial-numbers-to-the-remote-host).

Elk van deze procedures wordt hieronder beschreven.

#### <a name="to-import-the-certificate-on-the-remote-host"></a>Voor het importeren van het certificaat op de externe host

1. Klik met de rechtermuisknop op het cer-bestand en selecteer het **certificaat installeren**. Hiermee start u de Wizard Certificaat importeren.

    ![Wizard Certificaat importeren 1](./media/storsimple-remote-connect/HCS_CertificateImportWizard1.png)

2. **Locatie van berichtenarchief**voor **Lokale computer**selecteren en klik op **volgende**.

3. Selecteer **alle certificaten in het onderstaande archief opslaan**en klik vervolgens op **Bladeren**. Ga naar het archief met basiscertificaten van de externe host en klik op **volgende**.

    ![Wizard Certificaat importeren 2](./media/storsimple-remote-connect/HCS_CertificateImportWizard2.png)

4. Klik op **Voltooien**. Er verschijnt een bericht waarin wordt gemeld dat het importeren is voltooid.

    ![Wizard Certificaat importeren 3](./media/storsimple-remote-connect/HCS_CertificateImportWizard3.png)

#### <a name="to-add-device-serial-numbers-to-the-remote-host"></a>Serienummers apparaat toevoegen aan de externe host

1. Start Kladblok als administrator en open vervolgens het Hostsbestand te vinden op \Windows\System32\Drivers\etc.

2. De volgende drie vermeldingen toevoegen aan het Hostsbestand: **0 gegevens IP-adres**, **Controller 0 vast IP-adres**en **Controller 1 vast IP-adres**.

3. Voer het serienummer van het apparaat dat u eerder hebt opgeslagen. Wijst dit op het IP-adres zoals weergegeven in de volgende afbeelding. Append voor Controller 0 en 1 Controller, **Controller0** en **Controller1** aan het einde van het serienummer (CN-naam).

    ![De CN-naam toevoegen aan het hosts-bestand](./media/storsimple-remote-connect/HCS_AddingCNNameToHostsFile.png)

4. Sla het Hostsbestand.

### <a name="connect-to-the-device-from-the-remote-host"></a>Verbinding maken met het apparaat van de externe host

Gebruik Windows PowerShell en SSL voor een SSAdmin-sessie op uw apparaat vanaf een externe host of client. De SSAdmin-sessie wordt toegewezen aan de optie 1 in de [seriële console](storsimple-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-device-serial-console) -menu van het apparaat.

De volgende procedure uitvoeren op de computer van waaruit u wilt maken van de externe verbinding van Windows PowerShell.

#### <a name="to-enter-an-ssadmin-session-on-the-device-by-using-windows-powershell-and-ssl"></a>Invoeren van een SSAdmin-sessie op het apparaat met behulp van Windows PowerShell en SSL

1. Een Windows PowerShell-sessie starten als administrator.

2. Het IP-adres van de client vertrouwde hosts toevoegen door te typen:

     `Set-Item wsman:\localhost\Client\TrustedHosts <device_ip> -Concatenate -Force`

    Waarbij <*device_ip*> staat voor het IP-adres van het apparaat; bijvoorbeeld: 

     `Set-Item wsman:\localhost\Client\TrustedHosts 10.126.173.90 -Concatenate -Force`

3. Een nieuwe referentie maken door te typen: 

     `$cred = New-Object pscredential @("<IP of target device>\SSAdmin", (ConvertTo-SecureString -Force -AsPlainText "<Device Administrator Password>"))`

    Waarbij <*IP van doelapparaat*> is het IP-adres gegevens 0 voor uw apparaat; bijvoorbeeld **10.126.173.90** zoals in de voorgaande afbeelding van het hosts-bestand. Ook het administrator-wachtwoord voor het apparaat opgeven.

4. Een sessie maken door te typen:

     `$session = New-PSSession -UseSSL -ComputerName <Serial number of target device> -Credential $cred -ConfigurationName "SSAdminConsole"`

    Voor de parameter - computernaam in de cmdlet, het <*volgnummer van het doelapparaat*> verstrekken. Dit serienummer is toegewezen aan het IP-adres van de gegevens van 0 in het hosts-bestand op uw externe host; bijvoorbeeld **SHX0991003G44MT** zoals in de volgende afbeelding.

5. Type: 

     `Enter-PSSession $session`

6. Moet u enkele minuten en vervolgens wordt u verbonden met uw apparaat via HTTPS via SSL. U ziet een bericht dat u bent verbonden met het apparaat aangeeft.

    ![PowerShell remoting HTTPS en SSL](./media/storsimple-remote-connect/HCS_PSRemotingUsingHTTPSAndSSL.png)

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [gebruik van Windows PowerShell voor het beheren van uw apparaat StorSimple](storsimple-windows-powershell-administration.md).

- Meer informatie over het [gebruik van de StorSimple Manager-service voor het beheren van uw apparaat StorSimple](storsimple-manager-service-administration.md).
