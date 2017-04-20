<properties
   pageTitle="Azure AD verbinden: De vereisten en hardware | Microsoft Azure"
   description="Dit onderwerp beschrijft de vereisten en de hardwarevereisten voor Azure AD verbinden"
   services="active-directory"
   documentationCenter=""
   authors="andkjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.workload="identity"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="10/12/2016"
   ms.author="billmath"/>

# <a name="prerequisites-for-azure-ad-connect"></a>Vereisten voor AD Azure verbinding
Dit onderwerp beschrijft de vereisten en de hardwarevereisten voor Azure AD verbinden.

## <a name="before-you-install-azure-ad-connect"></a>Voordat u Azure AD verbinden
Voordat u Azure AD-verbinding installeert, zijn er een paar dingen die u nodig hebt.

### <a name="azure-ad"></a>Azure AD
- Een Azure-abonnement of een [proefabonnement op Azure](https://azure.microsoft.com/pricing/free-trial/). Dit is alleen vereist voor toegang tot de portal Azure en niet voor het gebruik van Azure AD verbinden. Als u werkt met PowerShell of Office 365 hoeft u niet een Azure abonnement op verbinding maken met de Azure AD. Als u al een Office 365-licentie kunt u ook de Office 365 portal. Met een betaalde licentie voor Office 365 Ook kun je in de portal Azure vanuit Office 365 portal.
    - Ook kunt u de functie AD Azure voorbeeldweergave in [Azure portal](https://portal.azure.com). Deze portal is niet een Azure-licentie vereist.
- [Toevoegen en controleer of het domein](active-directory-add-domain.md) u wilt gebruiken in Azure AD. Bijvoorbeeld, als u van plan bent contoso.com gebruiken voor uw gebruikers, moet u ervoor zorgen dit domein is geverifieerd en u niet alleen het domein van de standaard contoso.onmicrosoft.com gebruikt.
- Een huurder Azure AD kan door 50k-standaardobjecten. Bij het controleren van uw domein, wordt de limiet verhoogd tot 300 k-objecten. Als u nog meer objecten in AD Azure, moet u een transactieprobleem ondersteuning als u wilt dat de limiet nog verder verhoogd. Als u meer dan 500 k objecten moet u een licentie, bijvoorbeeld Office 365, Azure AD Basic, Azure AD Premium of Enterprise Mobility Suite.

### <a name="prepare-your-on-premises-data"></a>Voorbereiden van uw gegevens op de gebouwen
- [Optionele synchronisatiefuncties die u in Azure AD inschakelen kunt](active-directory-aadconnectsyncservice-features.md) bekijken en beoordelen van de functies die u moet inschakelen.

### <a name="on-premises-servers-and-environment"></a>Servers op gebouwen en omgeving
- Het AD-schema versie en forest-functionaliteitsniveau moet Windows Server 2003 of hoger. Domeincontrollers kunnen elke versie wordt uitgevoerd, zolang de schema- en vereisten wordt voldaan.
- Als u wilt gebruiken de functie **wachtwoord terugschrijven naar** moet de domeincontrollers zijn op Windows Server 2008 (met het laatste SP) of hoger. Als de DCs op 2008 (pre-R2) moet u ook [de hotfix KB2386717](http://support.microsoft.com/kb/2386717)toepassen.
- De domeincontroller die wordt gebruikt door AD Azure moet schrijfbaar zijn. Het wordt niet ondersteund voor het gebruik van een alleen-lezen domeincontroller (alleen-lezen domeincontroller) en Azure AD verbinden niet voldoet aan alle omleidingen schrijven.
- Azure AD-verbinding kan niet worden geïnstalleerd op Small Business Server of Windows Server Essentials. De server moet Windows Server standard of hoger gebruiken.
- De Azure AD Connect-server moet een volledige GUI geïnstalleerd hebben. Het wordt niet ondersteund als u wilt installeren op een server core.
- Azure AD verbinden moet zijn geïnstalleerd op Windows Server 2008 of hoger. Deze server kan een domeincontroller of een lidserver zijn als express-instellingen. Als u aangepaste instellingen gebruikt, wordt de server kan ook zelfstandig worden en hoeft niet te worden toegevoegd aan een domein.
- Als u Azure AD verbinding maken in Windows Server 2008 installeert, zorg ervoor dat de meest recente patches toepassen vanaf Windows Update. De installatie kan niet beginnen met een unpatched server.
- Als u van plan het onderdeel **Wachtwoordsynchronisatie bent**, moet de Azure verbinden met AD-server op Windows Server 2008 R2 SP1 of later.
- De server verbinden met Azure AD moet [.NET Framework 4.5.1](#component-prerequisites) of hoger en [Microsoft PowerShell 3.0](#component-prerequisites) of hoger is geïnstalleerd.
- Als u Active Directory Federation Services wordt ingezet, moeten de servers waarop AD FS of Web Application Proxy zijn geïnstalleerd Windows Server 2012 R2 of later. [Windows Extern beheer](#windows-remote-management) moet worden ingeschakeld op deze servers voor installatie op afstand.
- Als u Active Directory Federation Services wordt ingezet, moet u [SSL-certificaten](#ssl-certificate-requirements).
- Als Active Directory Federation Services wordt ingezet, moet u voor het configureren van [naamomzetting](#name-resolution-for-federation-servers).
- Azure AD verbinding maken met vereist een SQL Server-database voor het opslaan van id-gegevens. Een SQL Server 2012 Express LocalDB (een lichte versie van SQL Server Express) wordt standaard geïnstalleerd en de service-account voor de service op de lokale computer wordt gemaakt. SQL Server Express heeft een limiet van 10GB kunt u ongeveer 100.000 objecten beheren. Als u nodig hebt voor het beheren van een groter volume van directory-objecten, moet u de installatiewizard verwijzen naar een andere installatie van SQL Server.
- Als u een afzonderlijke SQL-Server, gelden deze vereisten:
    - Azure AD Connect ondersteunt alle versies van Microsoft SQL Server in SQL Server 2008 (met SP4) voor SQL Server 2014. Microsoft Azure SQL-Database wordt **niet ondersteund** als een database.
    - Hebt u een SQL-sortering niet hoofdlettergevoelig. Deze worden aangeduid met een \_CI_ in hun naam. Het is **niet ondersteund** voor het gebruik van een hoofdlettergevoelige sortering, aangegeven door \_CS_ in hun naam.
    - U kunt slechts één sync engine per exemplaar van de database hebben. Is het delen van de database-instantie met FIM MIM/Sync of DirSync Azure AD Sync wordt **niet ondersteund** .

### <a name="accounts"></a>Rekeningen
- Een Azure AD globale Administrator-account voor de Azure AD-map die u wilt integreren. Dit moet een **account van school of organisatie** en kan niet van een **Microsoft-account**.
- Als u express-instellingen gebruiken of een upgrade van DirSync uitvoert, moet u een Enterprise Administrator-account hebben voor uw lokale Active Directory.
- [Computeraccounts in Active Directory](./connect/active-directory-aadconnect-accounts-permissions.md) als u het installatiepad van de aangepaste instellingen.

### <a name="azure-ad-connect-server-configuration"></a>Azure AD verbinding maken met server-configuratie
- Als uw globale beheerders MVR gesloten ingeschakeld, moet de URL- **https://secure.aadcdn.microsoftonline-p.com** worden in de lijst met vertrouwde sites. U wordt gevraagd om toe te voegen aan de lijst met vertrouwde sites als deze niet is toegevoegd voordat u wordt gevraagd om een uitdaging MVR gesloten. U kunt Internet Explorer toe te voegen aan uw vertrouwde sites.

### <a name="connectivity"></a>Connectiviteit
- De Azure AD Connect-server moet de DNS-omzetting voor intranet- en internet. De DNS-server moet kunnen omzetten van namen, zowel voor uw Active Directory op gebouwen als de eindpunten Azure AD.
- Als u firewalls op uw Intranet en u poorten wilt openen tussen de Azure AD verbinding maken met servers en domeincontrollers, moet u vervolgens [Azure AD verbinden poorten](active-directory-aadconnect-ports.md) Zie voor meer informatie.
- Als de proxy of firewall limiet die URL's kunnen worden benaderd, vervolgens de URL's in [Office 365-URL's en IP-adresbereiken](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2) gedocumenteerd moet worden geopend.
    - Als u de Microsoft Cloud in Duitsland of de regering van Microsoft Azure cloud dan zien [Azure AD verbinden sync service instanties overwegingen](active-directory-aadconnect-instances.md) voor URL's.
- Azure AD Connect is standaard gebruik van TLS 1.0 om te communiceren met Azure Active Directory. U kunt dit wijzigen naar TLS 1.2 door de stappen in [TLS 1.2 inschakelen voor Azure AD verbinden](#enable-tls-12-for-azure-ad-connect).
- Als u een uitgaande proxy gebruikt voor de verbinding met het Internet, moet de volgende instelling in het bestand **C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\machine.config** voor de wizard installeren en verbinden met Azure AD sync kunnen verbinding maken met het Internet en Azure AD worden toegevoegd. Deze tekst moet worden ingevoerd onder aan het bestand. In deze code, &lt;PROXYADRESS&gt; vertegenwoordigt de naam werkelijke proxy IP-adres of de hostnaam.

```
    <system.net>
        <defaultProxy>
            <proxy
            usesystemdefault="true"
            proxyaddress="http://<PROXYADDRESS>:<PROXYPORT>"
            bypassonlocal="true"
            />
        </defaultProxy>
    </system.net>
```

- Als de proxyserver verificatie vereist is, de [service-account](./connect/active-directory-aadconnect-accounts-permissions.md#azure-ad-connect-sync-service-accounts) moet zich bevinden in het domein en moet u het installatiepad van de aangepaste instellingen gebruiken voor een [aangepaste service-account](./connect/active-directory-aadconnect-get-started-custom.md#install-required-components)opgeven. U moet ook een andere wijziging in machine.config. Met deze reageren wijziging in de installatiewizard machine.config en sync engine op verificatieaanvragen van de proxyserver. In de installatiewizard op alle worden pagina's, met uitzondering van de pagina **configureren** de ondertekende in de referenties van de gebruiker gebruikt. Op de pagina **configureren** aan het einde van de installatiewizard wordt de context overgeschakeld naar de [serviceaccount](./connect/active-directory-aadconnect-accounts-permissions.md#azure-ad-connect-sync-service-accounts) die door u is gemaakt. De sectie machine.config ziet er zo uit.

```
    <system.net>
        <defaultProxy enabled="true" useDefaultCredentials="true">
            <proxy
            usesystemdefault="true"
            proxyaddress="http://<PROXYADDRESS>:<PROXYPORT>"
            bypassonlocal="true"
            />
        </defaultProxy>
    </system.net>
```

Zie MSDN voor meer informatie over de [standaardproxy Element](https://msdn.microsoft.com/library/kd3cf2ex.aspx).

Als u problemen met connectiviteit hebt, Zie [problemen met verbindingen oplossen](active-directory-aadconnect-troubleshoot-connectivity.md).

### <a name="other"></a>Andere
- Optioneel: Een gebruikersaccount test om te controleren of de synchronisatie.

## <a name="component-prerequisites"></a>Vereisten voor onderdelen

### <a name="powershell-and-net-framework"></a>PowerShell en .net Framework
Azure AD verbinding afhankelijk van Microsoft PowerShell en .NET Framework 4.5.1. U moet deze versie of een latere versie is geïnstalleerd op uw server. Afhankelijk van uw versie van Windows Server, het volgende doen:

- Windows Server 2012R2
  - Microsoft PowerShell standaard wordt geïnstalleerd, is geen actie vereist.
  - .NET framework 4.5.1 en latere versies worden aangeboden via Windows Update. Zorg ervoor dat u de nieuwste updates hebt geïnstalleerd op een Server in het Configuratiescherm van Windows.
- Windows Server 2008R2 en Windows Server 2012
  - De meest recente versie van Microsoft PowerShell is beschikbaar in **Windows Management Framework 4.0**, beschikbaar op het [Microsoft Download Center](http://www.microsoft.com/downloads).
  - .NET framework 4.5.1 en latere versies zijn beschikbaar op het [Microsoft Download Center](http://www.microsoft.com/downloads).
- Windows Server 2008
  - De laatste ondersteunde versie van PowerShell is beschikbaar in **Windows Management Framework 3.0**is beschikbaar op het [Microsoft Download Center](http://www.microsoft.com/downloads).
 - .NET framework 4.5.1 en latere versies zijn beschikbaar op het [Microsoft Download Center](http://www.microsoft.com/downloads).

### <a name="enable-tls-12-for-azure-ad-connect"></a>TLS 1.2 voor Azure AD verbinding inschakelen
Azure AD Connect maakt gebruik van TLS 1.0 standaard voor het coderen van de communicatie tussen de server van synchronisatie-engine en Azure AD. U kunt dit wijzigen door .net toepassingen configureren voor gebruik van TLS 1.2 standaard op de server. U vindt meer informatie over TLS 1.2 in [Microsoft Security Advisory-2960358](https://technet.microsoft.com/security/advisory/2960358).

1. TLS 1.2 kan niet worden ingeschakeld op Windows Server 2008. U moet Windows Server 2008R2 of hoger. Zorg ervoor dat u de hotfix op .net 4.5.1 voor het besturingssysteem is geïnstalleerd, raadpleegt u [Microsoft Security Advisory-2960358](https://technet.microsoft.com/security/advisory/2960358). U hebt dit of een latere versie is al geïnstalleerd op uw server.
2. Als u Windows Server 2008R2, Controleer of dat TLS 1.2 is ingeschakeld. TLS 1.2 moet al zijn ingeschakeld op Windows Server 2012 server en latere versies.
```
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2]
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client] "DisabledByDefault"=dword:00000000 "Enabled"=dword:00000001
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server] "DisabledByDefault"=dword:00000000 "Enabled"=dword:00000001
```
3. Deze registersleutel instellen voor alle besturingssystemen, en de server opnieuw opstarten.
```
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\v4.0.30319
"SchUseStrongCrypto"=dword:00000001
```
4. Als u ook TLS 1.2 tussen de synchronisatieserver-engine en een externe SQL-Server inschakelen wilt, zorg ervoor dat u beschikt over de vereiste versies voor [ondersteuning van TLS 1.2 voor Microsoft SQL Server](https://support.microsoft.com/kb/3135244)is geïnstalleerd.

## <a name="prerequisites-for-federation-installation-and-configuration"></a>Vereisten voor de federation-installatie en configuratie

### <a name="windows-remote-management"></a>Windows Extern beheer
Wanneer u Azure AD verbinding maken met Active Directory Federation Services of de webproxy-toepassing implementeren, controleert u de onderstaande voorwaarden om ervoor te zorgen connectiviteits- en slagen:

- Als de doelserver lid van domein, moet u controleren of extern beheerde Windows is ingeschakeld
    - Opdracht gebruiken in een opdrachtvenster verhoogde PSH`Enable-PSRemoting –force`
- Als de doelserver een niet-domein gekoppelde WAP-computer is, zijn er een aantal aanvullende eisen
    - Op de doelcomputer (WAP-machine):
         - Controleer de winrm (Windows Remote Management / WS-Management)-service wordt uitgevoerd via de module Services
         - Opdracht gebruiken in een opdrachtvenster verhoogde PSH`Enable-PSRemoting –force`
    - Op de computer waarop de wizard wordt uitgevoerd (als de doelcomputer niet-verbonden of niet-vertrouwde domein):
        - De opdracht gebruiken in een opdrachtvenster verhoogde PSH`Set-Item WSMan:\localhost\Client\TrustedHosts –Value <DMZServerFQDN> -Force –Concatenate`
        - In Serverbeheer:
             - WAP DMZ host toevoegen aan groep machines (Serverbeheer beheren -> Servers toevoegen... -> tabblad DNS gebruiken)
             - Tabblad Server Manager alle Servers: WAP-server met de rechtermuisknop op en kies beheren als..., lokale (niet domein) creds invoeren voor de WAP-machine
             - PSH verbindingen, op het tabblad Server Manager alle Servers valideren: WAP-server met de rechtermuisknop op en kiest u Windows PowerShell.  Een externe sessie voor PSH moet worden geopend om ervoor te zorgen de PowerShell-sessies kunnen worden vastgesteld.

### <a name="ssl-certificate-requirements"></a>Vereisten voor SSL-certificaten
**Belangrijk:** is het aan te raden hetzelfde SSL-certificaat gebruiken op alle knooppunten van de AD FS-farm, alsmede alle proxyservers van de webtoepassing.

- Het certificaat moet een X509 certificaat.
- U kunt een zelfondertekend certificaat voor federation-servers in een testomgeving. Echter voor een productie-omgeving raden wij aan dat u het certificaat van een openbare Certificeringsinstantie aanvragen.
    - Als een certificaat dat niet openbaar vertrouwde, ervoor te zorgen dat het certificaat is geïnstalleerd op elke Web Application Proxy server vertrouwd op de lokale server en alle federation-servers wordt
- De identiteit van het certificaat moet overeenkomen met de naam van de federation-service (bijvoorbeeld sts.contoso.com).
    - De identiteit is een onderwerp alternatieve (SAN) extensie van type DNS-naam of als er geen posten SAN, de naam wordt opgegeven als een gemeenschappelijke naam.  
    - Meerdere vermeldingen van SAN kunnen aanwezig zijn in het certificaat, indien een van deze overeenkomt met de naam van de federation-service.
    - Als u van plan bent gebruik werkplek deelnemen aan een extra SAN is vereist als de waarde **enterpriseregistration.** gevolgd door het achtervoegsel UPN (User Principal Name) van uw organisatie, bijvoorbeeld **enterpriseregistration.contoso.com**.
- Certificaten op basis van de volgende generatie (CNG) toetsen CryptoAPI en sleutelopslagproviders worden niet ondersteund. Hierdoor dat hebt u een certificaat op basis van een CSP (cryptographic serviceprovider) en niet een KSP (opslag van sleutels provider).
- Certificaten met jokerteken worden ondersteund.

### <a name="name-resolution-for-federation-servers"></a>Naamomzetting voor federatieservers
- DNS-records voor de AD FS federation servicenaam (bv. sts.contoso.com) voor het intranet (uw interne DNS-server) en het extranet (openbare DNS via uw domeinregistratieservice) instellen. Voor de intranet-DNS record Zorg ervoor dat u een records en geen CNAME-records. Dit is vereist voor windows-verificatie werkt alleen correct uit uw domein gekoppelde computer.
- Als u meer dan één AD FS-server of Web Application Proxy server implementeert, zorg dan dat u de verdeling van de belasting hebt geconfigureerd en of de DNS-records voor de servicenaam van de AD FS-federatie (bijvoorbeeld sts.contoso.com) naar de taakverdeling verwijzen.
- Voor geïntegreerde windows-verificatie werkt voor browsertoepassingen met behulp van Internet Explorer op uw intranet, moet u ervoor zorgen dat de servicenaam van de AD FS-federatie (bijvoorbeeld sts.contoso.com) wordt toegevoegd aan de zone Lokaal intranet in Internet Explorer. Dit kan via Groepsbeleid worden beheerd en geïmplementeerd op alle computers in uw domein toegevoegd.

## <a name="azure-ad-connect-supporting-components"></a>Azure AD verbinden ondersteunende onderdelen
Hier volgt een lijst met onderdelen die Azure AD verbinden wordt geïnstalleerd op de server waarop de Azure AD verbinden is geïnstalleerd. Deze lijst is voor een basisinstallatie Express.  Als u een andere SQL Server op de pagina installatie synchronisatie services gebruikt, is SQL Express LocalDB niet lokaal geïnstalleerd.

- Azure AD verbinding gezondheid
- Microsoft Online Services-In-assistent voor IT-Professionals (geïnstalleerd, maar geen afhankelijkheid van het)
- Opdrachtregelprogramma's van Microsoft SQL Server 2012
- Microsoft SQL Server 2012 Express LocalDB
- Native Client voor Microsoft SQL Server 2012
- Distributiepakket van Microsoft Visual C++ 2013

## <a name="hardware-requirements-for-azure-ad-connect"></a>Hardwarevereisten voor Azure AD verbinden
In de onderstaande tabel toont de minimale systeemvereisten voor de computer verbinden met Azure AD sync.

| Aantal objecten in Active Directory | CPU | Geheugen | Grootte harde schijf |
| ------------------------------------- | --- | ------ | --------------- |
| Minder dan 10.000 | 1,6 GHz | 4 GB | 70 GB |
| 10.000 – 50.000 | 1,6 GHz | 4 GB | 70 GB |
| 50.000-100.000 | 1,6 GHz | 16 GB | 100 GB |
| 100.000 of meer objecten is de volledige versie van SQL Server vereist voor|  |  |  |
| 100.000 – 300.000 | 1,6 GHz | 32 GB | 300 GB |
| 300.000 – 600.000 | 1,6 GHz | 32 GB | 450 GB |
| Meer dan 600.000 | 1,6 GHz | 32 GB | 500 GB |

De minimale vereisten voor computers met AD FS of Web Application Servers is het volgende:

- Processor: Dual core 1,6 GHz of hoger
- GEHEUGEN: 2GB of hoger
- Azure VM: A2 configuratie of hoger

## <a name="next-steps"></a>Volgende stappen
Meer informatie over de [integratie van uw identiteiten op ruimten met Azure Active Directory](active-directory-aadconnect.md).
