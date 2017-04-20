<properties
    pageTitle="Azure AD verbinding Health Agent installatie | Microsoft Azure"
    description="Dit is de pagina Azure AD verbinding maken met de gezondheid van die de installatie van de agent voor AD FS en synchronisatie beschrijft."
    services="active-directory"
    documentationCenter=""
    authors="karavar"
    manager="samueld"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="vakarand"/>


# <a name="azure-ad-connect-health-agent-installation"></a>Azure AD Health Agent-installatie aansluiten

Dit document helpt u bij het installeren en configureren van de Azure AD verbinding Health Agents. U kunt de agenten downloaden vanaf [hier](active-directory-aadconnect-health.md#download-and-install-azure-ad-connect-health-agent).

##  <a name="requirements"></a>Vereisten
De volgende tabel bevat een lijst met vereisten voor het gebruik van Azure AD verbinding maken met de gezondheid.

| Vereiste | Beschrijving|
| ----------- | ---------- |
|Azure AD Premium| Azure AD verbinding maken met de gezondheid is een functie Azure AD Premium en Azure AD Premium vereist. </br></br>Zie voor meer informatie, [aan de slag met Azure AD Premium](active-directory-get-started-premium.md) </br>Zie het starten van een gratis proefperiode van 30 dagen [Start een proefversie.](https://azure.microsoft.com/trial/get-started-active-directory/)|
|U moet een globale beheerder van uw Azure advertentie aan de slag met Azure AD verbinding gezondheid|Standaard kunnen de globale groep administrators installeren en configureren van de gezondheid agenten aan de slag, toegang tot de portal en alle bewerkingen in Azure AD verbinding maken met de gezondheid. Voor meer informatie, Zie [beheren van uw directory AD Azure](active-directory-administer.md). <br><br> Rollen gebaseerd toegangsbeheer kunt u toegang tot Azure AD verbinding maken met de gezondheid van andere gebruikers in uw organisatie. Zie voor meer informatie [rollen gebaseerd toegangsbeheer voor gezondheid verbinding Azure AD.](active-directory-aadconnect-health-operations.md#manage-access-with-role-based-access-control) </br></br>**Belangrijk:** De account die wordt gebruikt bij het installeren van de stoffen moet een werk of school-account. Een Microsoft-account kan niet. Zie voor meer informatie, [Aanmelden voor Azure als organisatie](sign-up-organization.md)
|De Azure AD verbinding Health Agent is geïnstalleerd op elke server waarop| Gezondheid verbinding Azure AD vereist de installatie van een agent op de doelservers, zodat de gegevens die worden weergegeven in de portal. </br></br>Als u gegevens uit uw AD FS-infrastructuur in gebouwen, moet de agent bijvoorbeeld worden geïnstalleerd op de AD FS en AD FS Proxy Web Application Proxy servers. Ook voor het ophalen van gegevens op uw locatie op AD DS-infrastructuur, de agent moet worden geïnstalleerd op de domeincontrollers. </br></br>**Belangrijk:** De account die wordt gebruikt bij het installeren van de stoffen moet een werk of school-account. Een Microsoft-account kan niet. Zie voor meer informatie, [Aanmelden voor Azure als organisatie](sign-up-organization.md)|
|Uitgaande verbindingen op de eindpunten Azure service|De agent moet tijdens de installatie en uitvoering connectiviteit met eindpunten verbinden gezondheid Azure AD. Als uitgaande verbindingen geblokkeerd is, ervoor te zorgen dat de volgende eindpunten worden toegevoegd aan de lijst met toegestane: </br></br><li>& #42;. BLOB.Core.Windows.NET </li><li>& #42;. Queue.Core.Windows.NET</li><li>adhsprodwus.servicebus.Windows.NET - poort: 5671 </li><li>https://Management.Azure.com </li><li>https://S1.adhybridhealth.Azure.com/</li><li>https://policykeyservice.dc.AD.MSFT.NET/</li><li>https://login.Windows.NET</li><li>https://login.microsoftonline.com</li><li>https://Secure.aadcdn.microsoftonline-p.com</li> |
|Firewall-poorten op de server waarop de agent wordt uitgevoerd.| De agent moet de volgende firewallpoorten geopend zodat de agent kan communiceren met de service-endpoints Azure AD gezondheid te zijn.</br></br><li>TCP/UDP-poort 443</li><li>TCP/UDP-poort 5671</li>
|De volgende websites toestaan als verbeterde beveiliging van Internet Explorer is ingeschakeld| Als Verbeterde beveiliging van Internet Explorer is ingeschakeld, moeten de volgende websites zijn toegestaan op de server die u wilt hebben van de agent is geïnstalleerd.</br></br><li>https://login.microsoftonline.com</li><li>https://Secure.aadcdn.microsoftonline-p.com</li><li>https://login.Windows.NET</li><li>De federation-server voor uw organisatie vertrouwd door Azure Active Directory. Bijvoorbeeld: https://sts.contoso.com</li>




## <a name="installing-the-azure-ad-connect-health-agent-for-ad-fs"></a>Health Agent voor AD FS installeren de Azure AD verbinding
U start de agent-installatie, dubbelklik op het .exe-bestand dat u hebt gedownload. Klik op installeren op het eerste scherm.

![Controleer of Azure AD gezondheid verbinding maken](./media/active-directory-aadconnect-health-requirements/install1.png)

Zodra de installatie is voltooid, klikt u op nu configureren.

![Controleer of Azure AD gezondheid verbinding maken](./media/active-directory-aadconnect-health-requirements/install2.png)

Een opdrachtprompt wordt gestart, gevolgd door enkele PowerShell die Register AzureADConnectHealthADFSAgent wordt uitgevoerd. Wanneer u wordt gevraagd aan te melden Azure, doorgaan en log in.

![Controleer of Azure AD gezondheid verbinding maken](./media/active-directory-aadconnect-health-requirements/install3.png)


Na de ondertekening blijft PowerShell. Zodra deze is voltooid, kunt u PowerShell sluiten en de configuratie is voltooid.

Nu moeten de services worden gestart waardoor automatisch de agent te controleren en gegevens te verzamelen. Als u voldoet niet aan de vereisten die worden beschreven in de voorgaande secties, worden waarschuwingen weergegeven in het venster PowerShell. Zorg ervoor dat de [vereisten](active-directory-aadconnect-health-agent-install.md#requirements) voor de installatie van de agent te voltooien. De volgende schermafdruk is een voorbeeld van deze fouten.

![Controleer of Azure AD gezondheid verbinding maken](./media/active-directory-aadconnect-health-requirements/install4.png)

Als u wilt controleren of dat de agent is geïnstalleerd, zoekt u de volgende services op de server. Als u de configuratie hebt voltooid, moeten ze al worden uitgevoerd. Anders zijn ze gestopt totdat de configuratie voltooid is.

- Azure AD verbinding maken met de gezondheid van AD FS diagnostische Service
- Azure AD verbinding AD FS inzichten gezondheidszorg
- Azure AD verbinding maken met de gezondheid van AD FS-Service controleren

![Controleer of Azure AD gezondheid verbinding maken](./media/active-directory-aadconnect-health-requirements/install5.png)


### <a name="agent-installation-on-windows-server-2008-r2-servers"></a>Agent installeren op Windows Server 2008 R2-Servers

Stappen voor Windows Server 2008 R2-servers:

1. Zorg ervoor dat de server Service Pack 1 of hoger wordt uitgevoerd.
1. Verbeterde beveiliging voor agent-installatie uitschakelen:
1. Windows PowerShell 4.0 installeren op de servers voor het installeren van de AD-Health agent. Windows PowerShell 4.0 installeren:
 - [Microsoft .NET Framework 4.5](https://www.microsoft.com/download/details.aspx?id=40779) , met de volgende koppeling voor het downloaden van de offline installer installeren.
 - Installeren PowerShell ISE (van Windows-functies)
 - Installeer de [Windows Management Framework 4.0.](https://www.microsoft.com/download/details.aspx?id=40855)
 - Installeer Internet Explorer versie 10 of hoger op de server. (Vereist door de Health Service te verifiëren, met uw referenties Azure Admin).
1. Zie de wiki-artikel voor meer informatie over het installeren van Windows PowerShell 4.0 in Windows Server 2008 R2 [hier](http://social.technet.microsoft.com/wiki/contents/articles/20623.step-by-step-upgrading-the-powershell-version-4-on-2008-r2.aspx).

### <a name="enable-auditing-for-ad-fs"></a>Controle voor AD FS inschakelen

> [AZURE.NOTE] Deze sectie is alleen van toepassing op de AD FS-federation-servers.

Om de functie voor het verzamelen en analyseren van gegevens voor gebruiksanalyse, moet de agent Azure AD verbinding maken met de gezondheid van de gegevens in de controlelogboeken voor AD FS. Deze logboekbestanden zijn standaard niet ingeschakeld. Gebruik de volgende procedures voor het inschakelen van controle voor AD FS en de AD FS-controlelogboeken vinden op de AD FS-servers.

#### <a name="to-enable-auditing-for-ad-fs-20"></a>Inschakelen van controle voor AD FS 2.0

1. Klik op **Start**en wijs **programma's** **Lokaal beveiligingsbeleid** **Systeembeheer**.
2. Ga naar de map **Security-instellingen\Beveiligingsinstellingen\Lokaal beleid\Toewijzing Rights Management** en dubbelklik op Beveiligingscontrole genereren.
3. Controleer of de serviceaccount voor AD FS 2.0 is vermeld op het tabblad **Lokale beveiligingsinstelling** . Als dit niet aanwezig is, klikt u op **gebruiker of groep toevoegen** toe te voegen aan de lijst en klik vervolgens op **OK**.
4. Als u wilt controleren, opent u een opdrachtprompt met verhoogde bevoegdheden en voer de volgende opdracht uit:<code>auditpol.exe /set /subcategory:"Application Generated" /failure:enable /success:enable</code>
5. Sluit Lokaal beveiligingsbeleid en open de MMC-module. Om de MMC-module openen, klikt u op **Start**, wijs **programma's**, **Systeembeheer**en klik vervolgens op AD FS 2.0 Management.
6. Klik in het deelvenster Acties op Federation-Service-eigenschappen bewerken.
7. Klik op het tabblad **gebeurtenissen** in het dialoogvenster **Eigenschappen voor Federation-Service** .
8. Schakel de selectievakjes **geslaagde** en **mislukte gebeurtenissen uit te voeren** .
9. Klik op **OK**.

#### <a name="to-enable-auditing-for-ad-fs-on-windows-server-2012-r2"></a>Inschakelen van controle voor AD FS in Windows Server 2012 R2

1. Open **Lokaal beveiligingsbeleid** door **Server Manager** op het startscherm of Serverbeheer openen op de taakbalk op het bureaublad en klik vervolgens op **Extra/lokaal beveiligingsbeleid**.
2. Ga naar de map **Security instellingen\Beveiligingsinstellingen\Lokaal Beleid\toewijzing** en dubbelklik op **Beveiligingscontrole genereren**.
3. Controleer of de serviceaccount voor AD FS is vermeld op het tabblad **Lokale beveiligingsinstelling** . Als dit niet aanwezig is, klikt u op **gebruiker of groep toevoegen** toe te voegen aan de lijst en klik vervolgens op **OK**.
4. Als u wilt controleren, opent u een opdrachtprompt met verhoogde bevoegdheden en voer de volgende opdracht uit:<code>auditpol.exe /set /subcategory:"Application Generated" /failure:enable /success:enable.</code>
5. Sluit **Lokaal beveiligingsbeleid**en open de module **Beheer van AD FS** (in Serverbeheer, klik op Extra en selecteer AD FS beheren).
6. Klik in het deelvenster Acties op **Federation Service-eigenschappen bewerken**.
7. Klik op het tabblad **gebeurtenissen** in het dialoogvenster Eigenschappen voor Federation-Service.
8. Schakel de selectievakjes **geslaagde en mislukte gebeurtenissen uit te voeren** en klik op **OK**.






#### <a name="to-locate-the-ad-fs-audit-logs"></a>Registreert de AD FS-audit vinden


1. Open **Logboeken**.
2. Ga naar de Windows-Logboeken en selecteer **beveiliging**.
3. Aan de rechterkant, klikt u op **Huidige Logboeken filteren**.
4. Selecteer **AD FS controleren**bij bron van gebeurtenis.

![Controlelogboeken voor AD FS](./media/active-directory-aadconnect-health-requirements/adfsaudit.png)

> [AZURE.WARNING] Als een groepsbeleid dat is het uitschakelen van AD FS controle bestaat, zijn de Azure AD verbinding Health Agent is om informatie te verzamelen. Zorg ervoor dat er geen groepsbeleid die controle kan uitschakelen.

[//]: # (Start of Agent Proxy Configuration Section)

## <a name="installing-the-azure-ad-connect-health-agent-for-sync"></a>Installeren van de agent gezondheid Azure AD verbinden voor synchronisatie
De agent gezondheid Azure AD verbinden voor synchronisatie wordt automatisch geïnstalleerd in de laatste build van Azure AD verbinden. Voor het gebruik van Azure AD verbinden voor synchronisatie, moet u de nieuwste versie van Azure AD verbinden downloaden en installeren. U kunt de meest recente versie downloaden [hier](http://www.microsoft.com/download/details.aspx?id=47594).

Als u wilt controleren of dat de agent is geïnstalleerd, zoekt u de volgende services op de server. Als u de configuratie hebt voltooid, moeten ze al worden uitgevoerd. Anders zijn ze gestopt totdat de configuratie voltooid is.

- Azure AD gezondheidszorg Sync inzichten verbinding
- Azure AD verbinding gezondheid Sync-Service controleren

![Controleer of Azure AD gezondheid verbinden voor synchronisatie](./media/active-directory-aadconnect-health-sync/services.png)

> [AZURE.NOTE] Houd er rekening mee dat met behulp van gezondheid verbinding Azure AD Azure AD Premium vereist. Als er geen Azure AD Premium, bent u niet voltooien van de configuratie in de portal Azure. Zie de [pagina met systeemvereisten](active-directory-aadconnect-health-agent-install.md#requirements)voor meer informatie.


## <a name="manual-azure-ad-connect-health-for-sync-registration"></a>Azure AD handmatig verbinding gezondheid voor registratie van synchronisatie
Als de Azure AD verbinding maken met de gezondheid van voor registratie van synchronisatie-agent mislukt na installatie Azure AD verbinden, kunt u de volgende PowerShell-opdracht de agent handmatig registreren.

>[AZURE.IMPORTANT] Met deze opdracht PowerShell is alleen vereist als de registratie van de agent mislukt na het installeren van Azure AD verbinden.

De volgende PowerShell opdracht is vereist alleen wanneer de registratie van de gezondheid van agent mislukt zelfs na een succesvolle installatie en configuratie van Azure AD verbinden. De Azure AD verbinding gezondheidsdiensten begint nadat de agent is geregistreerd.

U kunt de agent gezondheid Azure AD verbinden voor synchronisatie met de volgende opdracht met PowerShell handmatig registreren:

`Register-AzureADConnectHealthSyncAgent -AttributeFiltering $false -StagingMode $false`

De opdracht wordt na de parameters:

- AttributeFiltering: $true (standaard) - als Azure AD verbinden is niet het kenmerk default synchronisatie ingesteld en is aangepast voor het gebruik van een gefilterde kenmerkset. $false anders.
- StagingMode: $false (standaard) - als de Azure AD Connect-server niet in een staging-$true-modus als de server is geconfigureerd om te worden in de staging-modus.

Wanneer u wordt gevraagd voor de verificatie moet u dezelfde algemene admin-account (zoals admin@domain.onmicrosoft.com) die is gebruikt voor het configureren van Azure AD verbinden.

## <a name="installing-the-azure-ad-connect-health-agent-for-ad-ds"></a>Installeren van de AD Azure verbinding Health Agent voor AD DS
U start de agent-installatie, dubbelklik op het .exe-bestand dat u hebt gedownload. Klik op installeren op het eerste scherm.

![Controleer of Azure AD gezondheid verbinding maken](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install1.png)

Zodra de installatie is voltooid, klikt u op nu configureren.

![Controleer of Azure AD gezondheid verbinding maken](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install2.png)

Een opdrachtprompt wordt gestart, gevolgd door enkele PowerShell die Register AzureADConnectHealthADDSAgent wordt uitgevoerd. Wanneer u wordt gevraagd aan te melden Azure, doorgaan en log in.

![Controleer of Azure AD gezondheid verbinding maken](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install3.png)

Na de ondertekening blijft PowerShell. Zodra deze is voltooid, kunt u PowerShell sluiten en de configuratie is voltooid.

Nu moeten de services worden gestart waardoor automatisch de agent te controleren en gegevens te verzamelen. Als u voldoet niet aan de vereisten die worden beschreven in de voorgaande secties, worden waarschuwingen weergegeven in het venster PowerShell. Zorg ervoor dat de [vereisten](active-directory-aadconnect-health-agent-install.md#requirements) voor de installatie van de agent te voltooien. De volgende schermafdruk is een voorbeeld van deze fouten.

![Controleer of Azure AD gezondheid verbinding voor AD DS](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install4.png)

Als u wilt controleren of dat de agent is geïnstalleerd, zoekt u de volgende services op de domeincontroller.

- Azure AD verbinding AD DS inzichten gezondheidszorg
- Azure AD verbinding maken met de gezondheid van AD DS-Service controleren

Als u de configuratie hebt voltooid, moeten al deze services worden uitgevoerd. Anders zijn ze gestopt totdat de configuratie voltooid is.

![Controleer of Azure AD gezondheid verbinding maken](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install5.png)

## <a name="installing-the-azure-ad-connect-health-agent-for-ad-ds-on-server-core"></a>U installeert de Azure AD verbinding Health Agent voor AD DS op een Server Core.
Na de installatie van het .exe-bestand kunt u het registratieproces voltooien met behulp van de volgende PowerShell-opdracht:

`Register-AzureADConnectHealthADDSAgent -Credential $cred`

## <a name="configure-azure-ad-connect-health-agents-to-use-http-proxy"></a>Azure AD verbinding Health Agents configureren voor het gebruik van HTTP-Proxy
U kunt de Azure AD verbinding Health Agents werken met een HTTP-Proxy configureren.

>[AZURE.NOTE]
- Met behulp van 'Netsh WinHttp set ProxyServerAddress' wordt niet ondersteund als de agent System.Net web aanvragen in plaats van Microsoft Windows HTTP Services gebruikt.
- De geconfigureerde HTTP-Proxy-adres wordt gebruikt voor gecodeerde Pass Through-Https-berichten.
- Geverifieerde proxy's (met HTTPBasic) worden niet ondersteund.

### <a name="change-health-agent-proxy-configuration"></a>Wijziging Health Agent proxyconfiguratie
U hebt de volgende opties voor het configureren van Azure AD verbinding Health Agent voor het gebruik van een HTTP-Proxy.

>[AZURE.NOTE]Alle Azure AD verbinding Health Agent-services moeten opnieuw worden opgestart, zodat de proxy-instellingen worden bijgewerkt. Voer de volgende opdracht:<br>
AdHealth opnieuw-Service *

#### <a name="import-existing-proxy-settings"></a>Bestaande proxy-instellingen importeren

##### <a name="import-from-internet-explorer"></a>Importeren vanuit Internet Explorer
Proxyinstellingen van Internet Explorer kunnen worden geïmporteerd om te worden gebruikt door de Azure AD verbinding Health Agents. Op elk van de servers met de Health agent kunt u de volgende PowerShell-opdracht uitgevoerd:

    Set-AzureAdConnectHealthProxySettings -ImportFromInternetSettings

##### <a name="import-from-winhttp"></a>Importeren van WinHTTP
WinHTTP-proxy-instellingen kunnen worden geïmporteerd om te worden gebruikt door de Azure AD verbinding Health Agents. Op elk van de servers met de Health agent kunt u de volgende PowerShell-opdracht uitgevoerd:

    Set-AzureAdConnectHealthProxySettings -ImportFromWinHttp

#### <a name="specify-proxy-addresses-manually"></a>Proxy-adressen handmatig opgeven
Op elk van de servers met de Health Agent door de volgende PowerShell-opdracht wordt uitgevoerd, kunt u handmatig een proxy-server opgeven:

    Set-AzureAdConnectHealthProxySettings -HttpsProxyAddress address:port

Voorbeeld: *myproxyserver Set AzureAdConnectHealthProxySettings - HttpsProxyAddress: 443*

- 'adres' is de naam van een DNS-server omgezet of een IPv4-adres
- "poort" kan worden weggelaten. Als dit argument wordt weggelaten en vervolgens als standaard-poort 443 wordt gekozen.

#### <a name="clear-existing-proxy-configuration"></a>Bestaande proxyconfiguratie wissen
U kunt de bestaande proxyconfiguratie van de uitschakelen door de volgende opdracht uit te voeren:

    Set-AzureAdConnectHealthProxySettings -NoProxy


### <a name="read-current-proxy-settings"></a>Huidige proxy-instellingen lezen
U kunt de momenteel geconfigureerde proxyinstellingen lezen door de volgende opdracht uit te voeren:

    Get-AzureAdConnectHealthProxySettings


## <a name="test-connectivity-to-azure-ad-connect-health-service"></a>Verbindingen testen met Azure AD Health Service verbinding
Het is mogelijk dat problemen zich kunnen voordoen die ertoe leiden dat de gezondheid verbinding Azure AD-agent met de service verbinding gezondheid Azure AD verbroken. Deze omvatten netwerkproblemen, toestemming problemen of andere redenen.

Als de agent geen gegevens verzenden naar de gezondheid verbinding Azure AD-service voor meer dan twee uur, dat wordt aangegeven met de volgende waarschuwing in de portal: "Health servicegegevens niet up-to-date is." U kunt controleren of de betrokken Azure AD verbinding Health agent kunnen gegevens uploaden naar de gezondheid verbinding Azure AD-service door de volgende PowerShell-opdracht uit te voeren is:

    Test-AzureADConnectHealthConnectivity -Role ADFS

De parameter rol heeft op dit moment de volgende waarden:

- AD FS
- Sync
- TOEGEVOEGD

U kunt de vlag - ShowResults in de opdracht gedetailleerde logboeken bekijken. In het volgende voorbeeld gebruiken:

    Test-AzureADConnectHealthConnectivity -Role Sync -ShowResult

>[AZURE.NOTE]Gebruik het hulpprogramma verbinding, moet u de registratie van de agent te voltooien. Als u niet kunt voltooien van de registratie van de agent, zorg ervoor dat u hebt voldaan aan alle [vereisten](active-directory-aadconnect-health-agent-install.md#requirements) voor Azure AD verbinding maken met de gezondheid van. Deze Connectiviteitstest wordt tijdens de registratie van de agent standaard uitgevoerd.



## <a name="related-links"></a>Verwante koppelingen

* [Azure AD verbinding gezondheid](active-directory-aadconnect-health.md)
* [Azure AD verbinding gezondheid bewerkingen](active-directory-aadconnect-health-operations.md)
* [Met behulp van Azure AD gezondheid verbinding met AD FS](active-directory-aadconnect-health-adfs.md)
* [Met behulp van gezondheid Azure AD verbinden voor synchronisatie](active-directory-aadconnect-health-sync.md)
* [Met behulp van Azure AD gezondheid verbinding met AD DS](active-directory-aadconnect-health-adds.md)
* [Azure AD gezondheid Veelgestelde vragen over verbinding](active-directory-aadconnect-health-faq.md)
* [Azure AD verbinding gezondheid versiegeschiedenis](active-directory-aadconnect-health-version-history.md)
