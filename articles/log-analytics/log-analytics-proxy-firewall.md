<properties
    pageTitle="Proxy- en firewall-instellingen configureren in logboek Analytics | Microsoft Azure"
    description="Proxy- en firewall-instellingen configureren als uw agenten of OMS services moeten specifieke poorten gebruiken."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/23/2016"
    ms.author="banders;magoedte"/>

# <a name="configure-proxy-and-firewall-settings-in-log-analytics"></a>Proxy- en firewall-instellingen configureren in logboek Analytics

Acties die nodig zijn voor het configureren van proxy- en firewall-instellingen voor logboek Analytics in OMS verschillen wanneer u met Operations Manager en haar vertegenwoordigers versus Microsoft controle agenten die rechtstreeks verbinding met servers maken. Zie de volgende secties voor het type van de agent die u gebruikt.

## <a name="configure-proxy-and-firewall-settings-with-the-microsoft-monitoring-agent"></a>Proxy- en firewall-instellingen configureren met de Microsoft Agent controleren

Voor de Microsoft Monitoring Agent verbinding maken met en registreren met de OMS-service, moet het toegang hebben tot het poortnummer van uw domeinen en URL's. Als u een proxyserver voor de communicatie tussen de agent en de OMS-service gebruikt, moet u ervoor zorgen dat de juiste bronnen toegankelijk zijn. Als u een firewall gebruikt om toegang te beperken tot het Internet, moet u uw firewall om toegang te verlenen aan OMS configureren. De volgende tabellen staan de poorten die OMS nodig heeft.

|**Agent-bron**|**Poorten**|**Bypass HTTPS inspectie**|
|--------------|-----|--------------|
|\*. ods.opinsights.azure.com|443|Ja|
|\*. oms.opinsights.azure.com|443|Ja|
|\*. blob.core.windows.net|443|Ja|
|ods.systemcenteradvisor.com|443| |

U kunt de volgende procedure configureert proxy-instellingen voor Microsoft Monitoring Agent via het Configuratiescherm. U moet de procedure voor elke server gebruiken. Als u veel servers die u wilt configureren hebt, kan het handiger zijn een script gebruiken om dit proces te automatiseren. Als dat zo is, Zie de volgende procedure [voor het configureren van de proxy-instellingen voor Microsoft Monitoring Agent met behulp van een script](#to-configure-proxy-settings-for-the-microsoft-monitoring-agent-using-a-script).

### <a name="to-configure-proxy-settings-for-the-microsoft-monitoring-agent-using-control-panel"></a>Proxy-instellingen voor Microsoft Monitoring Agent via het Configuratiescherm configureren

1. Open **het Configuratiescherm**.

2. Open **Microsoft Agent te controleren**.

3. Klik op het tabblad **Proxy-instellingen** .<br>  
  ![tabblad proxy-instellingen](./media/log-analytics-proxy-firewall/proxy-direct-agent-proxy.png)

4. **Gebruik een proxy-server** selecteren en typ de URL en poortnummer, indien nodig, zoals in het voorbeeld. Als de proxyserver verificatie vereist is, typt u de gebruikersnaam en het wachtwoord voor toegang tot de proxyserver.

Gebruik de volgende procedure voor het maken van een PowerShell script dat u uitvoeren kunt om de proxy-instellingen voor elke medewerker die rechtstreeks met servers verbonden.

### <a name="to-configure-proxy-settings-for-the-microsoft-monitoring-agent-using-a-script"></a>Proxy-instellingen voor Microsoft Monitoring Agent met behulp van een script configureren

In het volgende voorbeeld kopiëren, bijwerken met informatie die specifiek is voor uw omgeving, opslaan met de extensie PS1 en vervolgens het script uitvoeren op elke computer die direct verbinding met de OMS-service.

        
    param($ProxyDomainName="http://proxy.contoso.com:80", $cred=(Get-Credential))

    # First we get the Health Service configuration object.  We need to determine if we
    #have the right update rollup with the API we need.  If not, no need to run the rest of the script.
    $healthServiceSettings = New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg'

    $proxyMethod = $healthServiceSettings | Get-Member -Name 'SetProxyInfo'

    if (!$proxyMethod)
    {
         Write-Output 'Health Service proxy API not present, will not update settings.'
         return
    }

    Write-Output "Clearing proxy settings."
    $healthServiceSettings.SetProxyInfo('', '', '')

    $ProxyUserName = $cred.username

    Write-Output "Setting proxy to $ProxyDomainName with proxy username $ProxyUserName."
    $healthServiceSettings.SetProxyInfo($ProxyDomainName, $ProxyUserName, $cred.GetNetworkCredential().password)
        

## <a name="configure-proxy-and-firewall-settings-with-operations-manager"></a>Proxy- en firewall-instellingen configureren met Operations Manager

Voor een groep van de management Operations Manager verbinding maken met en registreren met de OMS-service moet deze toegang hebben tot de poortnummers van de domeinen en URL's. Als u een proxyserver voor communicatie tussen de server Operations Manager en de OMS-service gebruikt, moet u ervoor zorgen dat de juiste bronnen toegankelijk zijn. Als u een firewall gebruikt om toegang te beperken tot het Internet, moet u uw firewall om toegang te verlenen aan OMS configureren. Zelfs als een Operations Manager management server zich niet achter een proxyserver, zijn haar agenten. In dit geval moet de proxy-server op dezelfde manier worden geconfigureerd agenten zijn inschakelen en beveiliging toestaan en Log-beheer oplossingsgegevens verzonden naar de OMS-webservice.

Operations Manager-agents te communiceren met de OMS-service moet, de Operations Manager-infrastructuur (inclusief agenten) hebben de juiste proxy-instellingen en de versie. De proxy-instellingen voor agenten is in de console Operations Manager opgegeven. De versie moet een van de volgende opties:

- Operations Manager 2012 SP1 updatepakket 7 of hoger
- Operations Manager 2012 R2 updatepakket 3 of hoger


De volgende tabellen staan de poorten die betrekking hebben op deze taken.

>[AZURE.NOTE] Enkele van de volgende bronnen vermeld Advisor en operationele inzichten, beide zijn vorige versies van OMS. Bronnen in de lijst wordt echter in de toekomst veranderen.

Hier volgt een lijst met resources agent en poorten:<br>

|**Agent-bron**|**Poorten**|
|--------------|-----|
|\*. ods.opinsights.azure.com|443|
|\*. oms.opinsights.azure.com|443|
|\*.BLOB.Core.Windows.NET/\*|443|
|ods.systemcenteradvisor.com|443|
<br>
Hier volgt een lijst van management server-bronnen en poorten:<br>

|**Management server-bron**|**Poorten**|**Bypass HTTPS inspectie**|
|--------------|-----|--------------|
|service.systemcenteradvisor.com|443| |
|\*. service.opinsights.azure.com|443| |
|\*. blob.core.windows.net|443|Ja| 
|Data.systemcenteradvisor.com|443| | 
|ods.systemcenteradvisor.com|443| | 
|\*. ods.opinsights.azure.com|443|Ja| 
<br>
Hier volgt een lijst met resources van OMS en Operations Manager-console en poorten.<br>

|**Bron van OMS en Operations Manager-console**|**Poorten**|
|----|----|
|service.systemcenteradvisor.com|443|
|\*. service.opinsights.azure.com|443|
|\*. live.com|Poort 80 en 443|
|\*. microsoft.com|Poort 80 en 443|
|\*. microsoftonline.com|Poort 80 en 443|
|\*. mms.microsoft.com|Poort 80 en 443|
|Login.Windows.NET|Poort 80 en 443|
<br>

Gebruik de volgende procedures voor het registreren van uw groep Operations Manager management met OMS-service. Als u communicatieproblemen tussen de management-groep en de OMS-service, gebruik de valideringsprocedures gegevensoverdracht naar de OMS-service.

### <a name="to-request-exceptions-for-the-oms-service-endpoints"></a>Uitzonderingen voor de eindpunten OMS service aanvragen

1. Gebruik de informatie uit de eerste tabel eerder gepresenteerd om ervoor te zorgen dat de middelen die nodig zijn voor de Operations Manager server toegankelijk zijn via een firewall, moet u mogelijk.
2. Gebruik de informatie uit de tweede tabel eerder gepresenteerd om ervoor te zorgen dat de middelen die nodig zijn voor de console bewerkingen in Operations Manager en OMS toegankelijk zijn via een firewall, moet u mogelijk.
3. Als u een proxyserver met Internet Explorer gebruikt, zorg ervoor dat is geconfigureerd en goed werkt. Als u wilt controleren, kunt u een beveiligde verbinding (HTTPS), bijvoorbeeld [https://bing.com](https://bing.com)openen. Als de beveiligde internetverbinding niet in een browser werkt, werken deze waarschijnlijk niet in de beheerconsole van Operations Manager met webservices in de cloud.

### <a name="to-configure-the-proxy-server-in-the-operations-manager-console"></a>De proxy-server configureren in de console Operations Manager

1. Operations Manager-console openen en selecteert u de werkruimte **beheer** .

2. Vouw **Operationele inzichten**, en selecteer vervolgens **Verbinding met operationele inzichten**.<br>  
    ![Operations Manager OMS verbinding](./media/log-analytics-proxy-firewall/proxy-om01.png)
3. In de weergave OMS-verbinding, klikt u op **Proxy-Server configureren**.<br>  
    ![Operations Manager OMS verbinding proxyserver configureren](./media/log-analytics-proxy-firewall/proxy-om02.png)
4. In operationele inzichten Wizard instellingen: Proxyserver, selecteer **een proxyserver toegang te krijgen tot de operationele inzichten Web Service**en typ vervolgens de URL met de poort nummer, bijvoorbeeld **http://myproxy:80**.<br>  
    ![Operations Manager OMS proxy-adres](./media/log-analytics-proxy-firewall/proxy-om03.png)


### <a name="to-specify-credentials-if-the-proxy-server-requires-authentication"></a>Referenties opgeven als u de proxyserver is verificatie vereist
 Referenties voor de proxyserver en instellingen moeten doorgeven aan beheerde computers OMS wordt gerapporteerd. Deze servers moeten zich in de *Microsoft System Center Advisor Monitoring Server-groep*. Referenties worden in het register van elke server in de groep gecodeerd.

1. Operations Manager-console openen en selecteert u de werkruimte **beheer** .
2. Selecteer onder **RunAs configuratie** **profielen**.
3. Open het profiel van **System Center Advisor uitvoeren als profiel Proxy** .  
    ![afbeelding van het profiel van System Center Advisor uitvoeren als Proxy](./media/log-analytics-proxy-firewall/proxy-proxyacct1.png)
4. In de uitvoeren als-Wizard profiel, klikt u op **toevoegen** om een account uitvoeren als te gebruiken. U kunt een nieuwe account uitvoeren als maken of een bestaande account gebruiken. Deze account moet beschikken over voldoende machtigingen doorgeven via de proxyserver.  
    ![afbeelding van de Wizard uitvoeren als profiel](./media/log-analytics-proxy-firewall/proxy-proxyacct2.png)
5. Instellen van de account die u wilt beheren, kies **een geselecteerde klasse, groep of object** om het Object zoeken te openen.  
    ![afbeelding van de Wizard uitvoeren als profiel](./media/log-analytics-proxy-firewall/proxy-proxyacct2-1.png)
6. Zoek en selecteer vervolgens **Microsoft System Center Advisor Monitoring Server-groep**.  
    ![afbeelding van het Object zoeken](./media/log-analytics-proxy-firewall/proxy-proxyacct3.png)
7. Klik op **OK** om het toevoegen van een vak uitvoeren als-account sluiten.  
    ![afbeelding van de Wizard uitvoeren als profiel](./media/log-analytics-proxy-firewall/proxy-proxyacct4.png)
8. De wizard hebt voltooid en de wijzigingen opslaan.  
    ![afbeelding van de Wizard uitvoeren als profiel](./media/log-analytics-proxy-firewall/proxy-proxyacct5.png)


### <a name="to-validate-that-oms-management-packs-are-downloaded"></a>Valideren dat het Kantoorbeheersysteem management packs worden gedownload.

Als u oplossingen aan OMS hebt toegevoegd, kunt u deze weergeven in de console Operations Manager als beheerpakketten onder **beheer**. Zoek naar *System Center Advisor* snel terugvinden.  
    ![beheerpakketten gedownload](./media/log-analytics-proxy-firewall/proxy-mpdownloaded.png) of u kunt ook voor OMS-beheerpakketten controleren met behulp van de volgende Windows PowerShell-opdracht in de Operations Manager server:

    ```
    Get-ScomManagementPack | where {$_.DisplayName -match 'Advisor'} | select Name,DisplayName,Version,KeyToken
    ```

### <a name="to-validate-that-operations-manager-is-sending-data-to-the-oms-service"></a>Om te valideren dat Operations Manager verzendt gegevens naar de OMS-service

1. Prestatiemeter (perfmon.exe) openen en selecteert u **Prestatiemeter**in server management Operations Manager.
2. Klik op **toevoegen**en selecteer **Health Service Management-groepen**.
3. Voeg de items die met **HTTP beginnen**.  
    ![items toevoegen](./media/log-analytics-proxy-firewall/proxy-sendingdata1.png)
4. Als uw configuratie Operations Manager goed is, dat worden er activiteiten voor gezondheid Service prestatiemeters voor gebeurtenissen en andere gegevensitems, gebaseerd op de beheerpakketten die u in de OMS en het beleid geconfigureerd log-collectie toegevoegd.  
    ![Performance Monitor tonen activiteit](./media/log-analytics-proxy-firewall/proxy-sendingdata2.png)


## <a name="next-steps"></a>Volgende stappen

- [Oplossingen voor logboek Analytics toevoegen uit de galerie met oplossingen](log-analytics-add-solutions.md) voor functionaliteit toevoegen en gegevens te verzamelen.
- Vertrouwd raken met [zoekopdrachten log](log-analytics-log-searches.md) gedetailleerde informatie verzameld door oplossingen te bekijken.
