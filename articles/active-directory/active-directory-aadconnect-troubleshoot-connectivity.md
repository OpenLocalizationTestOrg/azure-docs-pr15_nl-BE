<properties
    pageTitle="Azure AD verbinding maken: Problemen met verbindingen | Microsoft Azure"
    description="Wordt uitgelegd hoe u problemen met verbindingen met Azure AD verbinden."
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
    ms.date="06/27/2016"
    ms.author="billmath"/>

# <a name="troubleshoot-connectivity-issues-with-azure-ad-connect"></a>Problemen met verbindingen met Azure AD verbinden
In dit artikel wordt uitgelegd hoe werkt connectiviteit tussen Azure AD Connect en Azure AD en het oplossen van verbindingsproblemen. Deze problemen worden waarschijnlijk kunnen worden bekeken in een omgeving met een proxy-server.

## <a name="troubleshoot-connectivity-issues-in-the-installation-wizard"></a>Problemen met verbindingen in de installatiewizard
Azure AD verbinden moderne verificatie (met het ADAL library) gebruikt voor verificatie. De wizard installeren en de juiste synchronisatie-engine vereisen machine.config worden juist geconfigureerd aangezien die .NET-toepassingen.

In dit artikel zullen we zien hoe Fabrikam naar Azure AD via de proxy verbinding maakt. De proxy-server met de naam fabrikamproxy en poort 8080.

Eerst moet controleren of [**machine.config**](active-directory-aadconnect-prerequisites.md#connectivity) correct is geconfigureerd.  
![machineconfig](./media/active-directory-aadconnect-troubleshoot-connectivity/machineconfig.png)

>[AZURE.NOTE]
In sommige blogs niet door Microsoft wordt beschreven dat er wijzigingen moeten worden aangebracht in de miiserver.exe.config in plaats daarvan. Dit bestand is echter bij elke upgrade, dus zelfs dat als het tijdens de oorspronkelijke installatie werkt, het systeem werkt niet meer op de eerste upgrade overschreven. Daarom is de aanbeveling machine.config in plaats daarvan bijwerken.

De proxy-server moet ook de gewenste URL's geopend hebben. De officiële lijst wordt beschreven in de [Office 365-URL's en IP-adresbereiken ](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2).

De volgende tabel is van deze absolute minimaal moet verbinding kunnen maken met AD Azure helemaal. Deze lijst omvat niet alle optionele functies, zoals wachtwoord writeback of Azure AD verbinding maken met de gezondheid. Het is hier om u te helpen bij het oplossen van problemen voor de eerste configuratie beschreven.

URL | Poort | Beschrijving
---- | ---- | ----
mscrl.Microsoft.com | HTTP/80 | Gebruikt om lijsten met CRL's te downloaden.
\*. verisign.com | HTTP/80 | Gebruikt om lijsten met CRL's te downloaden.
\*. entrust.com | HTTP/80 | Gebruikt om lijsten met CRL downloaden voor MVR gesloten.
\*. windows.net | HTTPS/443 | Gebruikt voor aanmelding bij AD Azure.
Secure.aadcdn.microsoftonline p.com | HTTPS/443 | Gebruikt voor de MVR gesloten.
\*. microsoftonline.com | HTTPS/443 | Gebruikt voor het configureren van de map Azure AD en gegevens importeren/exporteren.

## <a name="errors-in-the-wizard"></a>Fouten in de wizard
De installatiewizard maakt gebruik van twee verschillende beveiligingscontexten. Op de pagina **verbinding maken met Azure AD** deze gebruik maakt van de gebruiker die momenteel is aangemeld. Op de pagina **configureren** is het wijzigen van de [account waarop de service voor de synchronisatie-engine](active-directory-aadconnect-accounts-permissions.md#azure-ad-connect-sync-service-accounts). De proxyconfiguraties we maken zijn globaal voor de machine dus als er een probleem, het probleem zal de meeste waarschijnlijk al op de pagina **verbinding maken met Azure AD** in de wizard weergegeven.

Dit zijn de meest voorkomende fouten u in de installatiewizard ziet.

### <a name="the-installation-wizard-has-not-been-correctly-configured"></a>De installatiewizard niet juist is geconfigureerd
Deze fout wordt weergegeven wanneer de wizard zelf de proxy kan bereiken.
![nomachineconfig](./media/active-directory-aadconnect-troubleshoot-connectivity/nomachineconfig.png)

- Als u dit ziet, controleert u of dat de [machine.config](active-directory-aadconnect-prerequisites.md#connectivity) correct is geconfigureerd.
- Als die er goed uitziet, volgt u de stappen in de [proxy-connectiviteit controleren](#verify-proxy-connectivity) om te zien of het probleem niet met de wizard ook aanwezig is.

### <a name="the-mfa-endpoint-cannot-be-reached"></a>Het eindpunt van de MVR gesloten kan niet worden bereikt.
Deze fout wordt weergegeven als het eindpunt **https://secure.aadcdn.microsoftonline-p.com** kan niet worden bereikt en globale beheerder van de MVR gesloten ingeschakeld is.  
![nomachineconfig](./media/active-directory-aadconnect-troubleshoot-connectivity/nomicrosoftonlinep.png)

- Als u dit ziet, controleert u of het eindpunt secure.aadcdn.microsoftonline-p.com is toegevoegd aan de proxy.

### <a name="the-password-cannot-be-verified"></a>Het wachtwoord kan niet worden gecontroleerd.
Als de installatiewizard gelukt is verbinding met Azure AD, maar het wachtwoord zelf kan niet worden gecontroleerd is dit zichtbaar: ![badpassword](./media/active-directory-aadconnect-troubleshoot-connectivity/badpassword.png)

- Het wachtwoord is een tijdelijk wachtwoord en moet worden gewijzigd? Is het werkelijk het juiste wachtwoord? Probeer te melden bij https://login.microsoftonline.com (op een andere computer dan de Azure AD Connect-server) en controleer of dat de account kan worden gebruikt.

### <a name="verify-proxy-connectivity"></a>Proxy-connectiviteit controleren
Als u wilt controleren of de server verbinden met Azure AD werkelijke connectiviteit met het Internet en de Proxy heeft gebruiken we sommige PowerShell te zien als de proxy webverzoeken is toestaan of niet. Uitvoeren in de PowerShell-prompt, `Invoke-WebRequest -Uri https://adminwebservice.microsoftonline.com/ProvisioningService.svc`. (Technisch gezien de eerste aanroep van https://login.microsoftonline.com is en dit werkt ook, maar de andere URI is sneller te reageren.)

PowerShell gebruikt de configuratie machine.config contact opnemen met de proxy. Deze cmdlets mag niet van invloed op de instellingen in de winhttp/netsh.

Als de proxyserver correct is geconfigureerd, krijgt u de status van een succes: ![proxy200](./media/active-directory-aadconnect-troubleshoot-connectivity/invokewebrequest200.png)

Als er **geen verbinding maken met de externe server** vervolgens PowerShell wordt geprobeerd een rechtstreekse oproep te maken zonder dat de proxy of DNS is niet correct geconfigureerd. Controleer of dat het bestand **machine.config** correct is geconfigureerd.
![unabletoconnect](./media/active-directory-aadconnect-troubleshoot-connectivity/invokewebrequestunable.png)

Als de proxy niet juist is geconfigureerd, krijgen we een fout: ![proxy200](./media/active-directory-aadconnect-troubleshoot-connectivity/invokewebrequest403.png)
![proxy407](./media/active-directory-aadconnect-troubleshoot-connectivity/invokewebrequest407.png)

Fout | Tekst van foutbericht | Opmerking
---- | ---- | ---- |
403 | Verboden | De proxy is voor de aangevraagde URL niet geopend. Terugkeren naar de configuratie van de proxyserver en controleer of de [URL's](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2) zijn geopend.
407 | Proxyverificatie vereist | De proxy-server aanmelden vereist en geen is opgegeven. Als de proxyserver verificatie vereist is, moet deze worden geconfigureerd in het machine.config. Controleer ook of dat u voor de gebruiker met de wizard ook als de serviceaccount voor domeinaccounts gebruikt.

## <a name="the-communication-pattern-between-azure-ad-connect-and-azure-ad"></a>Het communicatiepatroon tussen Azure AD verbinden en Azure AD
Als u al deze stappen hierboven en nog steeds geen verbinding maken hebt gevolgd kan u op dit moment starten netwerk Logboeken bekijkt. In deze sectie wordt een patroon van de normale en geslaagde verbindingen te documenteren. Het is ook gemeenschappelijk red herrings die kan worden genegeerd als u de logboeken netwerk leest aanbieding.

- Er worden aanroepen van https://dc.services.visualstudio.com. Het is niet verplicht om deze openen in de proxy voor de installatie te kunnen uitvoeren en kan worden genegeerd.
- U ziet dat de werkelijke hosts in de DNS-naam ruimte nsatc.net en andere naamruimten, niet onder de microsoftonline.com wordt een lijst met DNS-omzetting. Echter zal niet er een web serviceaanvragen op de namen van werkelijke en er geen u deze naar de proxy.
- De eindpunten, adminwebservice en provisioningapi (Zie hieronder in de logboeken) discovery eindpunten en gezocht naar de werkelijke eindpunt te gebruiken en zal afhankelijk van uw regio worden verschilden.

### <a name="reference-proxy-logs"></a>Verwijzing proxy-logboekbestanden
Hier wordt een dump van een werkelijke proxy-logboekbestand en de pagina van de wizard installatie van waar deze is genomen (hetzelfde eindpunt van dubbele items zijn verwijderd). Dit kan worden gebruikt als referentie voor uw eigen proxy- en Logboeken. De werkelijke eindpunten in uw omgeving kunnen afwijken (in het bijzonder die welke in *cursief*).

**Verbinding maken met Azure AD**

Tijd | URL
--- | ---
11-1/2016 8:31 | Connect://login.microsoftonline.com:443
11-1/2016 8:31 | Connect://adminwebservice.microsoftonline.com:443
11-1/2016 8:32 | verbinding maken: / /*bba800-anker*. microsoftonline.com:443
11-1/2016 8:32 | Connect://login.microsoftonline.com:443
11-1/2016 8:33 | Connect://provisioningapi.microsoftonline.com:443
11-1/2016 8:33 | verbinding maken: / /*bwsc02-relay*. microsoftonline.com:443

**Configureren**

Tijd | URL
--- | ---
11-1/2016 8:43 | Connect://login.microsoftonline.com:443
11-1/2016 8:43 | verbinding maken: / /*bba800-anker*. microsoftonline.com:443
11-1/2016 8:43 | Connect://login.microsoftonline.com:443
11-1/2016 8:44 | Connect://adminwebservice.microsoftonline.com:443
11-1/2016 8:44 | verbinding maken: / /*bba900-anker*. microsoftonline.com:443
11-1/2016 8:44 | Connect://login.microsoftonline.com:443
11-1/2016 8:44 | Connect://adminwebservice.microsoftonline.com:443
11-1/2016 8:44 | verbinding maken: / /*bba800-anker*. microsoftonline.com:443
11-1/2016 8:44 | Connect://login.microsoftonline.com:443
11-1/2016 8:46 | Connect://provisioningapi.microsoftonline.com:443
11-1/2016 8:46 | verbinding maken: / /*bwsc02-relay*. microsoftonline.com:443

**Initiële synchronisatie**

Tijd | URL
--- | ---
11-1/2016 8:48 | Connect://login.Windows.NET:443
11-1/2016 8:49 | Connect://adminwebservice.microsoftonline.com:443
11-1/2016 8:49 | verbinding maken: / /*bba900-anker*. microsoftonline.com:443
11-1/2016 8:49 | verbinding maken: / /*bba800-anker*. microsoftonline.com:443

## <a name="authentication-errors"></a>Verificatiefouten
Deze sectie bevat fouten die door het ADAL (de bibliotheek verificatie die wordt gebruikt door Azure AD verbinden) en PowerShell kunnen worden geretourneerd. De uitgelegd moet helpen u inzicht in de volgende stappen.

### <a name="invalid-grant"></a>Ongeldige subsidie
Ongeldige gebruikersnaam of wachtwoord. Zie [het wachtwoord kan niet worden geverifieerd](#the-password-cannot-be-verified) voor meer informatie.

### <a name="unknown-user-type"></a>Gebruiker onbekend Type
Worden uw Azure advertentie kunnen niet gevonden of opgelost. Misschien wilt u aanmelden met een gebruikersnaam in een niet-geverifieerde domein?

### <a name="user-realm-discovery-failed"></a>Gebruiker Realm Discovery is mislukt
Problemen met netwerk- of proxy configuratie. Het netwerk kan niet worden bereikt, Zie [problemen oplossen in de installatiewizard](#troubleshoot-connectivity-issues-in-the-installation-wizard).

### <a name="user-password-expired"></a>Wachtwoord verlopen
Uw referenties zijn verlopen. Uw wachtwoord wijzigen.

### <a name="authorizationfailure"></a>AuthorizationFailure
Onbekend probleem.

### <a name="authentication-cancelled"></a>Verificatie is geannuleerd
De uitdaging meerledige verificatie (MVR gesloten) is geannuleerd.

### <a name="connecttomsonline"></a>ConnectToMSOnline
Verificatie is gelukt, maar Azure AD PowerShell heeft een probleem met verificatie.

### <a name="azurerolemissing"></a>AzureRoleMissing
Verificatie is gelukt. U bent geen algemene beheerder.

### <a name="privilegedidentitymanagement"></a>PrivilegedIdentityManagement
Verificatie is gelukt. Beschermde identiteitsbeheer is ingeschakeld en u bent momenteel geen algemene beheerder. Zie [Beschermde Identity Management](active-directory-privileged-identity-management-getting-started.md) voor meer informatie.

### <a name="companyinfounavailable"></a>CompanyInfoUnavailable
Verificatie is gelukt. Bedrijfsgegevens kan niet worden opgehaald uit AD Azure.

### <a name="retrievedomains"></a>RetrieveDomains
Verificatie is gelukt. Kan geen domeingegevens ophalen uit Azure Active Directory.

## <a name="troubleshooting-steps-for-previous-releases"></a>Probleemoplossing voor vorige versies.
Met versies vanaf build-nummer 1.1.105.0 (uitgegeven in februari 2016) is de aanmeldhulp ingetrokken. In deze sectie en de configuratie niet meer is vereist, maar wordt bewaard als referentie.

Voor de eenmalige in de Office-assistent te werken, moet winhttp worden geconfigureerd. Dit kan worden gedaan met [**netsh**](active-directory-aadconnect-prerequisites.md#connectivity).  
![Netsh](./media/active-directory-aadconnect-troubleshoot-connectivity/netsh.png)

### <a name="the-sign-in-assistant-has-not-been-correctly-configured"></a>De aanmeldhulp niet juist is geconfigureerd
Deze fout weergegeven als de assistent-in de proxy niet bereikbaar of de proxy wordt de aanvraag is niet toegestaan.
![nonetsh](./media/active-directory-aadconnect-troubleshoot-connectivity/nonetsh.png)

- Als u dit ziet, bekijkt u de configuratie van de proxy in [netsh](active-directory-aadconnect-prerequisites.md#connectivity) en controleer of dat correct is.
![netshshow](./media/active-directory-aadconnect-troubleshoot-connectivity/netshshow.png)
- Als die er goed uitziet, volgt u de stappen in de [proxy-connectiviteit controleren](#verify-proxy-connectivity) om te zien of het probleem niet met de wizard ook aanwezig is.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over de [integratie van uw identiteiten op ruimten met Azure Active Directory](active-directory-aadconnect.md).
