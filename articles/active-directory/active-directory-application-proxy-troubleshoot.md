<properties
    pageTitle="Problemen oplossen met de toepassingsproxy | Microsoft Azure"
    description="Het oplossen van fouten in Azure AD toepassingsproxy dekt."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/19/2016"
    ms.author="kgremban"/>



# <a name="troubleshoot-application-proxy"></a>Toepassingsproxy oplossen

Als er fouten optreden bij het openen van een uitgegeven toepassing of in het uitgeven van toepassingen, controleert u de volgende opties om te zien of Microsoft Azure AD toepassingsproxy correct werkt:

- Open de console Windows-Services en controleer of de **Connector van Microsoft AAD Application Proxy** -service is ingeschakeld en wordt uitgevoerd. U kunt ook kijken op de eigenschappenpagina van de Proxy-service zoals in de volgende afbeelding:  
  ![Screenshot van Microsoft AAD Application Proxy Connector Properties-venster](./media/active-directory-application-proxy-troubleshoot/connectorproperties.png)

- Open Logboeken en bekijk de toepassingsproxy connector gebeurtenissen in **Logboeken toepassingen en Services** > **Microsoft** > **AadApplicationProxy** > **Connector** > **Admin**.
- Indien nodig, zijn meer gedetailleerde logboeken beschikbaar door analytics en logboeken voor foutopsporing en het sessielogboek toepassingsproxy connector inschakelen inschakelen.

## <a name="the-page-is-not-rendered-correctly"></a>De pagina wordt niet correct weergegeven

Als u niet een specifiek foutbericht krijgt, u mogelijk nog problemen met uw toepassing voor het weergeven of functioneren niet goed. Dit kan gebeuren als u het pad van het artikel hebt gepubliceerd, maar de toepassing is vereist voor inhoud die buiten het desbetreffende pad bestaat.

Bijvoorbeeld, als u het pad https://yourapp/app publiceren, maar roept de toepassing de afbeeldingen in de https://yourapp/media, won't ze worden weergegeven. Zorg ervoor dat u de toepassing die gebruikmaakt van het hoogste niveau pad, moet u alle relevante inhoud publiceren. In dit voorbeeld zou http://yourapp/ worden.

Als u het pad naar de inhoud waarnaar wordt verwezen, maar nog steeds moeten gebruikers op een diepere koppeling in het pad wijzigt, ziet u het [instellen van de juiste koppeling voor toepassingen in het deelvenster Azure AD-toegang en Office 365-app launcher toepassingsproxy](https://blogs.technet.microsoft.com/applicationproxyblog/2016/04/06/setting-the-right-link-for-application-proxy-applications-in-the-azure-ad-access-panel-and-office-365-app-launcher/)boeken blog.

## <a name="general-errors"></a>Algemene fouten

Fout | Beschrijving | Resolutie
--- | --- | ---
Deze corporate app kan niet worden geopend. U bent niet gemachtigd voor toegang tot deze toepassing. Verificatie mislukt. Zorg ervoor dat de gebruiker met toegang tot deze toepassing toewijzen. | Mogelijk hebt u de gebruiker voor deze toepassing niet toegewezen. | Ga naar het tabblad **vereffening** en onder **gebruikers en groepen**, deze gebruiker of groep toewijzen aan deze toepassing.
Deze corporate app kan niet worden geopend. U bent niet gemachtigd voor toegang tot deze toepassing. Verificatie mislukt. Zorg ervoor dat de gebruiker een licentie voor Azure Active Directory Premium of Basic. | Gebruikers kunnen deze fout ophalen wanneer u probeert toegang te krijgen tot de app die u gepubliceerd als ze expliciet met een licentie voor Premium en eenvoudige niet zijn toegewezen door de beheerder van de abonnee. | Ga naar de tab van de Active Directory- **licenties** van de abonnee en ervoor zorgen dat deze gebruiker of groep een premie of een gewone licentie is toegewezen.


## <a name="connector-troubleshooting"></a>Connector oplossen
Als de registratie is mislukt tijdens de installatie van de wizard Connector, zijn er twee manieren om de reden voor de fout weer te geven. Zoeken in het gebeurtenislogboek onder **toepassingen en Services, Logs\Microsoft\AadApplicationProxy\Connector\Admin**of de volgende Windows PowerShell-opdracht uitvoeren.

    Get-EventLog application –source “Microsoft AAD Application Proxy Connector” –EntryType “Error” –Newest 1

| Fout | Beschrijving | Resolutie |
| --- | --- | --- |
| Connector-registratie is mislukt: Zorg ervoor dat u de toepassingsproxy in Azure Management Portal en u uw Active Directory-gebruikersnaam en wachtwoord juist ingevoerd ingeschakeld. Fout: 'een of meer fouten opgetreden." | U het venster registratie zonder aanmelding bij AD Azure gesloten. | De Connector-wizard opnieuw uitvoeren en registreren van de Connector. |
| Connector-registratie is mislukt: Zorg ervoor dat u de toepassingsproxy in Azure Management Portal en u uw Active Directory-gebruikersnaam en wachtwoord juist ingevoerd ingeschakeld. Fout: "AADSTS50001: Resource `https://proxy.cloudwebappproxy.net /registerapp` is uitgeschakeld." | Toepassingsproxy is uitgeschakeld.  | Zorg ervoor dat het inschakelen van Proxy-toepassing in de klassieke Azure portal voor het registreren van de Connector. Zie voor meer informatie over het inschakelen van de Proxy [services toepassingsproxy inschakelen](active-directory-application-proxy-enable.md). |
| Connector-registratie is mislukt: Zorg ervoor dat u de toepassingsproxy in Azure Management Portal en u uw Active Directory-gebruikersnaam en wachtwoord juist ingevoerd ingeschakeld. Fout: 'een of meer fouten opgetreden." | Als het registratie-venster wordt geopend en onmiddellijk gesloten zonder dat u zich aanmeldt, krijgt u waarschijnlijk deze fout. Deze fout treedt op als er een netwerkfout op uw systeem. | Zorg ervoor dat het mogelijk is om vanuit een browser verbinding maken met een openbare website is en dat de poorten geopend zoals is opgegeven in [de toepassingsproxy vereisten](active-directory-application-proxy-enable.md)zijn. |
| Connector-registratie is mislukt: Controleer of uw computer is verbonden met het Internet. Fout: "Er is geen eindpunt luisteren op `https://connector.msappproxy.net :9090/register/RegisterConnector` die het bericht kan accepteren. Dit wordt vaak veroorzaakt door een onjuist adres of SOAP-actie. Zie InnerException, indien aanwezig, voor meer informatie.' | Als u zich aanmeldt met uw Azure AD gebruikersnaam en wachtwoord, maar dit foutbericht, kan het zijn dat alle poorten boven 8081 worden geblokkeerd. | Zorg ervoor dat de vereiste poorten geopend zijn. Zie [vereisten voor de Proxy](active-directory-application-proxy-enable.md)voor meer informatie. |
| Duidelijke fout wordt weergegeven in het venster van de registratie. Niet – niet doorgaan alleen het venster te sluiten. | U hebt de verkeerde gebruikersnaam of wachtwoord ingevoerd. | &Opnieuw. |
| Connector-registratie is mislukt: Zorg ervoor dat u de toepassingsproxy in Azure Management Portal en u uw Active Directory-gebruikersnaam en wachtwoord juist ingevoerd ingeschakeld. Fout: "AADSTS50059: geen huurder identificatiegegevens gevonden in een van de aanvraag of geïmpliceerd door de referenties op en zoeken op service beginsel URI is mislukt. | U probeert aan te melden met een Microsoft-Account en niet van een domein dat deel uitmaakt van de organisatie-ID van de map die u probeert te openen. | Zorg ervoor dat het admin deel uitmaakt van dezelfde domeinnaam als het domein van de huurder, bijvoorbeeld als de Azure AD-domein contoso.com, admin moet zijn admin@contoso.com. |
| Kan niet worden opgehaald van het huidige beleid kan worden uitgevoerd voor het uitvoeren van scripts in PowerShell. | Als de installatie van de Connector is mislukt, Controleer of het uitvoeringsbeleid PowerShell is niet uitgeschakeld. | Open de Groepsbeleid-Editor. Ga naar **Computerconfiguratie** > **Beheersjablonen** > **Windows-onderdelen** > **Windows PowerShell** en dubbelklik op de **uitvoering van het Script inschakelen**. Dit kan worden ingesteld op **Niet geconfigureerd** of **ingeschakeld**. Indien ingesteld op **ingeschakeld**, zorg ervoor dat onder Opties voor het uitvoeringsbeleid is ingesteld op een **lokale scripts toestaan en externe ondertekend scripts** of **Alle scripts**toestaan. |
| Downloaden van de configuratie van de connector is mislukt. | Certificaat van de Connector-client die voor verificatie wordt gebruikt, is verlopen. Dit kan ook optreden als u de Connector geïnstalleerd achter een proxy. In dit geval de Connector heeft geen toegang tot het Internet en niet kunnen verstrekken aan externe gebruikers. | Vernieuwen van de vertrouwensrelatie die handmatig met behulp van de `Register-AppProxyConnector` cmdlet in Windows PowerShell. Als de verbindingslijn zich achter een proxy, is het om toegang tot het Internet met de Connector rekeningen 'network services' en 'lokale systeem'. Dit kunt u doen door ze toegang verlenen tot de Proxy of door ze naar de proxyserver wordt overgeslagen. |
| Connector-registratie is mislukt: Zorg ervoor dat u een globale beheerder van uw Active Directory voor het registreren van de Connector. Fout: "de registratieaanvraag is geweigerd." | De alias die u probeert aan te melden met een beheerder op dit domein niet. De verbindingslijn wordt altijd voor de map die eigenaar is van het domein van de gebruiker geïnstalleerd. | Zorg ervoor dat de beheerder u probeert aan te melden als globale machtigingen heeft voor de huurder Azure AD.|


## <a name="kerberos-errors"></a>Kerberos-fouten

| Fout | Beschrijving | Resolutie |
| --- | --- | --- |
| Kan niet worden opgehaald van het huidige beleid kan worden uitgevoerd voor het uitvoeren van scripts in PowerShell. | Als de installatie van de Connector is mislukt, Controleer of het uitvoeringsbeleid PowerShell is niet uitgeschakeld. | Open de Groepsbeleid-Editor. Ga naar **Computerconfiguratie** > **Beheersjablonen** > **Windows-onderdelen** > **Windows PowerShell** en dubbelklik op de **uitvoering van het Script inschakelen**. Dit kan worden ingesteld op **Niet geconfigureerd** of **ingeschakeld**. Indien ingesteld op **ingeschakeld**, zorg ervoor dat onder Opties voor het uitvoeringsbeleid is ingesteld op een **lokale scripts toestaan en externe ondertekend scripts** of **Alle scripts**toestaan. |
| 12008 - azure advertentie heeft het maximum aantal toegestane verificatiepogingen van Kerberos in de back-endserver overschreden. | Deze gebeurtenis kan wijzen op problemen met de configuratie tussen AD Azure en de back-end application server of een probleem in de configuratie van de datum en tijd op beide computers. | De back-endserver daalde het Kerberos-ticket gemaakt door AD Azure. Controleer of deze Azure AD en de back-end application server correct zijn geconfigureerd. Zorg ervoor dat de configuratie van de datum en tijd op de Azure AD en de back-end application server worden gesynchroniseerd. |
| 13016 - azure AD kan een Kerberos-ticket voor de gebruiker niet ophalen omdat er geen UPN in het token van de rand of in de access-cookie. | Er is een probleem met de configuratie van de STS. | De UPN-claim-configuratie corrigeren in de STS. |
| 13019 - azure AD kan een Kerberos-ticket voor de gebruiker niet vanwege de volgende algemene API-fout ophalen. | Deze gebeurtenis kan wijzen op problemen met de configuratie tussen AD Azure en de domein-controller-server of een probleem in de configuratie van de datum en tijd op beide computers. | De domeincontroller het Kerberos-ticket gemaakt door AD Azure afgewezen. Controleer of die Azure AD en de back-end application server zijn geconfigureerd, met name de SPN-configuratie. Zorg ervoor dat de advertentie Azure is een domein dat deel uitmaakt van hetzelfde domein bevinden als de domeincontroller om ervoor te zorgen dat de domeincontroller vertrouwensrelatie met Azure Active Directory te worden ingesteld. Zorg ervoor dat de configuratie van de datum en tijd op de Azure AD en de domeincontroller worden gesynchroniseerd. |
| 13020 - AD azure kan een Kerberos-ticket voor de gebruiker niet ophalen omdat de back endserver-SPN is niet gedefinieerd. | Deze gebeurtenis kan wijzen op problemen met de configuratie tussen AD Azure en de domein-controller-server of een probleem in de configuratie van de datum en tijd op beide computers. | De domeincontroller het Kerberos-ticket gemaakt door AD Azure afgewezen. Controleer of die Azure AD en de back-end application server zijn geconfigureerd, met name de SPN-configuratie. Zorg ervoor dat de advertentie Azure is een domein dat deel uitmaakt van hetzelfde domein bevinden als de domeincontroller om ervoor te zorgen dat de domeincontroller vertrouwensrelatie met Azure Active Directory te worden ingesteld. Zorg ervoor dat de configuratie van de datum en tijd op de Azure AD en de domeincontroller worden gesynchroniseerd. |
| 13022 - AD azure kan de gebruiker niet verifiëren omdat de back endserver op de pogingen van Kerberos-verificatie met een HTTP 401-fout reageert. | Deze gebeurtenis kan wijzen op problemen met de configuratie tussen AD Azure en de back-end application server of een probleem in de configuratie van de datum en tijd op beide computers. | De back-endserver daalde het Kerberos-ticket gemaakt door AD Azure. Controleer of deze Azure AD en de back-end application server correct zijn geconfigureerd. Zorg ervoor dat de configuratie van de datum en tijd op de Azure AD en de back-end application server worden gesynchroniseerd. |
| De pagina weergeven niet. | De gebruiker kan deze fout ophalen wanneer u probeert toegang te krijgen tot de toepassing die u als de toepassing een uitgebreid is gepubliceerd. De gedefinieerde SPN voor deze toepassing is mogelijk onjuist. | Voor geïntegreerde Windows-apps: Zorg ervoor dat de SPN die is geconfigureerd voor deze toepassing correct is. |
| De pagina weergeven niet. | De gebruiker kan deze fout ophalen wanneer u probeert toegang te krijgen tot de toepassing die u als de toepassing een OWA is gepubliceerd. Dit kan een van de volgende oorzaken hebben: <br> -De gedefinieerde SPN voor deze toepassing is onjuist. <br> -De gebruiker die probeert toegang te krijgen tot de toepassing gebruikmaakt van een Microsoft-account in plaats van de juiste zakelijke account aan te melden, of de gebruiker een gastgebruiker is. <br> -De gebruiker die probeert toegang te krijgen tot de toepassing is niet goed gedefinieerd voor deze toepassing aan de kant van prem. | De stappen voor het dienovereenkomstig te beperken: <br> -Controleer of de SPN die is geconfigureerd voor deze toepassing correct is. <br> -Controleer of de gebruiker zich aanmeldt met behulp van hun zakelijke account die overeenkomt met het domein van de uitgegeven toepassing. Gebruikers van Microsoft-Account en Gast geen toegang tot de geïntegreerde Windows-toepassingen. <br> -Zorg ervoor dat deze gebruiker de juiste machtigingen heeft, zoals gedefinieerd voor deze back-end-toepassing op de computer op prem. |
| Deze corporate app kan niet worden geopend. U bent niet gemachtigd voor toegang tot deze toepassing. Verificatie mislukt. Zorg ervoor dat de gebruiker met toegang tot deze toepassing toewijzen. | Gebruikers kunnen deze fout ophalen wanneer u probeert toegang te krijgen tot de app die u gepubliceerd als ze Microsoft-accounts in plaats van hun zakelijke account gebruiken aan te melden. Gastgebruikers kunnen deze fout ook ophalen. | Microsoft-Account, gebruikers en gasten geen toegang tot de geïntegreerde Windows-toepassingen. Zorg ervoor dat de gebruiker zich aanmeldt met behulp van hun zakelijke account die overeenkomt met het domein van de uitgegeven toepassing. |
| Deze corporate app nu niet toegankelijk. Probeer het later opnieuw... Er is een time-out opgetreden voor de verbindingslijn. | Gebruikers kunnen deze fout ophalen wanneer u probeert toegang te krijgen tot de toepassing die u als ze niet correct zijn gedefinieerd voor deze toepassing aan de kant van prem gepubliceerd. | Zorg ervoor dat uw gebruikers de juiste machtigingen hebt, zoals gedefinieerd voor deze back-end-toepassing op de computer op prem. |


## <a name="see-also"></a>Zie ook

- [Toepassingsproxy voor Azure Active Directory inschakelen](active-directory-application-proxy-enable.md)
- [Uitgeven van toepassingen met de toepassingsproxy](active-directory-application-proxy-publish.md)
- [Eenmalige aanmelding inschakelen](active-directory-application-proxy-sso-using-kcd.md)
- [Voorwaardelijke toegang](active-directory-application-proxy-conditional-access.md)

Bekijk de [toepassingsproxy blog](http://blogs.technet.com/b/applicationproxyblog/) voor het laatste nieuws en updates


<!--Image references-->
[1]: ./media/active-directory-application-proxy-troubleshoot/connectorproperties.png
[2]: ./media/active-directory-application-proxy-troubleshoot/sessionlog.png
