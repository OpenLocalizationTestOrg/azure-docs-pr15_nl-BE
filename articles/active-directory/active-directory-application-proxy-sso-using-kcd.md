<properties
    pageTitle="Single sign-on met de toepassingsproxy | Microsoft Azure"
    description="Hoe u eenmalige aanmelding met Azure AD toepassingsproxy dekt."
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
    ms.date="08/10/2016"
    ms.author="kgremban"/>


# <a name="single-sign-on-with-application-proxy"></a>Single sign-on met de toepassingsproxy

Eenmalige aanmelding is een essentieel element van de toepassingsproxy Azure AD. Biedt de beste gebruikerservaring met de volgende stappen uit:

1. Een gebruiker zich aanmeldt bij de cloud  
2. Alle beveiligingsvalidaties gebeuren in de cloud (vooraf-verificatie)  
3. Wanneer de aanvraag wordt verzonden naar de toepassing op prem, vertegenwoordigt de toepassing-Connector-Proxy de gebruiker. De toepassing van de back-end denkt dat dit is een gewone gebruiker die afkomstig zijn van een apparaat een domein behoren.

![Access-diagram van de eindgebruiker via de Proxy, met het bedrijfsnetwerk](./media/active-directory-application-proxy-sso-using-kcd/app_proxy_sso_diff_id_diagram.png)

Azure AD toepassingsproxy helpt u bij een eenmalige aanmelding (SSO)-ervaring te bieden voor uw gebruikers. Gebruik de volgende instructies voor het publiceren van uw toepassingen met behulp van SSO:


## <a name="sso-for-on-prem-iwa-apps-using-kcd-with-application-proxy"></a>Eenmalige aanmelding voor voor prem geïntegreerde Windows-toepassingen met KCD met toepassingsproxy
Eenmalige aanmelding kunt u inschakelen voor uw toepassingen met behulp van geïntegreerde Windows-verificatie door Application Proxy verbindingslijnen toestemming geven in Active Directory uitgeven voor gebruikers, en verzenden en ontvangen van tokens voor hun rekening.


### <a name="network-diagram"></a>Netwerkdiagram

Dit diagram wordt de gegevensstroom beschreven wanneer een gebruiker probeert toegang te krijgen tot een toepassing op prem die gebruikmaakt van geïntegreerde Windows.

![Stroomdiagram van AAD Microsoft-verificatie](./media/active-directory-application-proxy-sso-using-kcd/AuthDiagram.png)

1. De gebruiker voert de toegang tot de toepassing van prem via de Proxy-URL.
2. Toepassingsproxy stuurt de aanvraag naar Azure AD verificatieservices te worden. Op dit moment geldt Azure AD eventuele van toepassing zijnde verificatie en autorisatiebeleid zoals multifactor-verificatie. Als de gebruiker is geverifieerd, wordt Azure AD een token wordt gemaakt en verzonden naar de gebruiker.
3. De gebruiker geeft het token aan de Proxy.
4. Toepassingsproxy valideert het token worden opgehaald van de UPN (User Principal Name) van het en stuurt de aanvraag, de UPN en Service Principal Name (SPN) naar de Connector via een beveiligd kanaal voor zowel geverifieerd.
5. De Connector voert onderhandelingen met de prem op Kerberos gebonden overdracht (KCD) AD, zich voordoen als de gebruiker om een Kerberos-token aan de toepassing.
6. Active Directory, stuurt het Kerberos-token voor de toepassing van de Connector.
7. De Connector stuurt de oorspronkelijke aanvraag naar de toepassingsserver, met behulp van het Kerberos-token van AD ontvangen.
8. De toepassing stuurt het antwoord op de verbindingslijn die wordt vervolgens teruggestuurd naar de toepassingsproxy service en ten slotte aan de gebruiker.

### <a name="prerequisites"></a>Vereisten

Voordat u aan de slag met eenmalige aanmelding voor de Proxy, moet dat uw omgeving is gereed met de volgende instellingen en configuraties:

- Uw apps, zoals SharePoint-webtoepassingen zijn ingesteld op het gebruik van geïntegreerde Windows-verificatie. Zie [Ondersteuning voor Kerberos-verificatie inschakelen](https://technet.microsoft.com/library/dd759186.aspx)voor meer informatie of Zie [Kerberos-verificatie in SharePoint 2013 plannen](https://technet.microsoft.com/library/ee806870.aspx)voor SharePoint.

- Alle apps in uw hebben Service Principal-namen.

- De server met de Connector en de server met de app zijn domein gekoppeld en deel uitmaken van hetzelfde domein of vertrouwende domeinen. Zie [lid worden van een Computer aan een domein](https://technet.microsoft.com/library/dd807102.aspx)voor meer informatie over aan domein toevoegen.

- De server met de Connector heeft toegang tot het lezen van de TokenGroupsGlobalAndUniversal voor gebruikers. Dit is een standaardinstelling die mogelijk zijn beïnvloed door versterking van het milieu. Meer hulp met dit in het [KB2009157](https://support.microsoft.com/en-us/kb/2009157).

### <a name="active-directory-configuration"></a>De configuratie van Active Directory

De configuratie van Active Directory is afhankelijk van of uw toepassing Proxy-Connector en de gepubliceerde server zich in hetzelfde domein of niet.

#### <a name="connector-and-published-server-in-the-same-domain"></a>Connector en de gepubliceerde server in hetzelfde domein

1. Ga in de Active Directory **-hulpprogramma's** > **-gebruikers en Computers**.
2. Selecteer de server met de Connector.
3. Klik met de rechtermuisknop en selecteer **Eigenschappen** > **overdracht**.
4. Selecteer **deze computer mag overdragen aan de opgegeven services alleen** en voeg onder **Services die deze account kan referenties overdragen**, de waarde voor de SPN-identiteit van de application server.
5. Hiermee kunt de verbindingslijn Application Proxy uitgeven voor gebruikers in AD ten opzichte van de applicaties die worden gedefinieerd in de lijst.

![Screenshot van connector SVR-eigenschappen-venster](./media/active-directory-application-proxy-sso-using-kcd/Properties.jpg)

#### <a name="connector-and-published-server-in-different-domains"></a>Connector en de gepubliceerde server in verschillende domeinen

1. Zie voor een lijst met vereisten voor het werken met KCD tussen domeinen, [Beperkte Kerberos-overdracht tussen domeinen](https://technet.microsoft.com/library/hh831477.aspx).
2. In Windows 2012 R2, gebruikt u de `principalsallowedtodelegateto` op de server van verbindingslijn inschakelen de toepassingsproxy overdragen voor de Connector-server waar de gepubliceerde server wordt de eigenschap `sharepointserviceaccount` en de delegerende server `connectormachineaccount`.

        $connector= Get-ADComputer -Identity connectormachineaccount -server dc.connectordomain.com

        Set-ADComputer -Identity sharepointserviceaccount -PrincipalsAllowedToDelegateToAccount $connector

        Get-ADComputer sharepointserviceaccount -Properties PrincipalsAllowedToDelegateToAccount


>[AZURE.NOTE] `sharepointserviceaccount`is de account SPS-machine of een serviceaccount waaronder de SPS-toepassingen wordt uitgevoerd.


### <a name="azure-classic-portal-configuration"></a>Azure klassieke configuratie van portal

1. Publiceer de toepassing volgens de instructies die worden beschreven in [de toepassingen publiceren met de Proxy](active-directory-application-proxy-publish.md). Zorg ervoor dat **Azure Active Directory** als de **Methode van vooraf-verificatie**selecteren.
2. Wanneer uw toepassing wordt weergegeven in de lijst met toepassingen, selecteert u deze en klikt u op **configureren**.
3. Stel onder **Eigenschappen**, **Interne verificatiemethode** op **Geïntegreerde Windows-verificatie**.  
  ![Geavanceerde configuratie](./media/active-directory-application-proxy-sso-using-kcd/cwap_auth2.png)  
4. Voer de **SPN voor interne toepassing** van de application server. In dit voorbeeld wordt de SPN voor onze gepubliceerde toepassing http/lob.contoso.com.  

>[AZURE.IMPORTANT] Als de UPN in de lokalen en de UPN in Azure Active Directory niet identiek zijn, moet u de [aanmeldings-id overgedragen](#delegated-login-identity) configureren voor een vooraf-verificatie werkt.

|  |  |
| --- | --- |
| Interne verificatiemethode | Als u AD Azure voor vooraf-verificatie gebruikt, kunt u een interne verificatiemethode waarmee gebruikers kunnen profiteren van de eenmalige aanmelding (SSO) naar deze toepassing instellen. <br><br> Als uw toepassing uitgebreid wordt en kunt u Kerberos gebonden overdracht (KCD) als u eenmalige aanmelding inschakelen voor deze toepassing wilt configureren, selecteert u **Geïntegreerde Windows-verificatie** (uitgebreid). Toepassingen die gebruikmaken van geïntegreerde Windows moeten worden geconfigureerd met behulp van KCD, anders toepassingsproxy niet mogelijk voor het publiceren van deze toepassingen. <br><br> Selecteer **geen** als uw toepassing geen gebruik wordt uitgebreid. |
| Interne toepassing SPN | Dit is de Service-Principal-Name (SPN) van de interne toepassing geconfigureerd in Azure AD op prem. De SPN wordt gebruikt door de toepassing Proxy-Connector voor het ophalen van Kerberos-tokens voor de toepassing die KCD. |


## <a name="sso-for-non-windows-apps"></a>Eenmalige aanmelding voor niet-Windows-toepassingen
De Kerberos-overdracht stroom in Azure AD toepassingsproxy begint wanneer Azure AD de gebruiker in de cloud verifieert. Zodra de aanvraag op lokalen binnenkomt, problemen de Azure AD Application Proxy Connector een Kerberos-ticket voor de gebruiker door interactie met de lokale Active Directory. Dit proces wordt aangeduid als Kerberos gebonden overdracht (KCD). In de volgende fase wordt een verzoek verzonden naar de back-end-toepassing met Kerberos-ticket. Er zijn een aantal protocollen die het verzenden van dergelijke verzoeken definiëren. De meeste niet-Windows-servers verwachten onderhandelen over/SPNego dat nu wordt ondersteund door de toepassingsproxy Azure AD.

![Niet-Windows SSO-diagram](./media/active-directory-application-proxy-sso-using-kcd/app_proxy_sso_nonwindows_diagram.png)

### <a name="delegated-login-identity"></a>Gedelegeerde aanmeldings-id
Gedelegeerde aanmeldings-id kunt u twee verschillende aanmelding scenario's te verwerken:

- Niet-Windows-toepassingen die doorgaans de identiteit van de gebruiker in de vorm van een SAM-accountnaam, niet door een e-mailadres of de gebruikersnaam (username@domain).
- Aanmelding alternatieve configuraties waarbij de UPN in Azure AD en de UPN in Active Directory op locatie verschillen.

Met de Proxy, kunt u selecteren welke identiteit met behulp van het Kerberos-ticket te verkrijgen. Deze instelling is per toepassing. Sommige van deze opties zijn geschikt voor systemen die de indeling van e-mailadres niet accepteert, anderen zijn ontworpen voor alternatieve aanmeldingsnaam.

![Screenshot van overgedragen login ID parameter](./media/active-directory-application-proxy-sso-using-kcd/app_proxy_sso_diff_id_upn.png)

Als gedelegeerde aanmeldings-id de waarde mogelijk niet uniek zijn voor alle domeinen of forests in uw organisatie. U kunt dit probleem voorkomen door het publiceren van deze toepassingen tweemaal met twee verschillende groepen in de Connector. Omdat elke toepassing een ander publiek heeft, kunt u deelnemen aan de verbindingslijnen naar een ander domein.


## <a name="working-with-sso-when-on-premises-and-cloud-identities-are-not-identical"></a>Werken met eenmalige aanmelding als lokale en cloud identiteiten zijn niet identiek
Tenzij anders worden geconfigureerd, de toepassingsproxy wordt ervan uitgegaan dat gebruikers dezelfde identiteit in de cloud en op gebouwen. U kunt configureren, voor elke toepassing, welke identiteit moet worden gebruikt bij het uitvoeren van eenmalige aanmelding.  

Deze mogelijkheid kunt veel organisaties met verschillende identiteiten in op prem en cloud moeten SSO uit de cloud voor prem apps zonder dat de gebruikers verschillende gebruikersnamen en wachtwoorden opgeven. Dit geldt ook voor organisaties die:

- Hebt u meerdere domeinen intern (joe@us.contoso.com, joe@eu.contoso.com) en één domein in de cloud (joe@contoso.com).

- Niet-routeerbaar domeinnaam intern hebben (joe@contoso.usa) en een rechtspersoon in de cloud.

- Domeinnamen niet intern gebruik (joe)

- Gebruik van verschillende aliassen op prem en in de cloud. Bv. joe-johns@contoso.comVS.joej@contoso.com  

Het helpt ook bij de toepassingen die niet adressen in de vorm van een e-mailadres accepteert, het zeer gebruikelijk voor niet-Windows back-end servers is.


### <a name="setting-sso-for-different-cloud-and-on-prem-identities"></a>Eenmalige aanmelding instellen voor verschillende identiteiten van cloud en op prem

1. Azure AD verbinden instellingen configureren zodat de hoofdidentiteit het e-mailadres (e). Dit gebeurt als onderdeel van het proces aanpassen door het veld **Gebruikersnaam beginsel** in de synchronisatie-instellingen wijzigen. Opmerking dat deze instellingen bepalen ook hoe gebruikers zich aanmelden bij Office365, Windows10 apparaten, en andere toepassingen die gebruikmaken van AD Azure als de winkel van hun identiteit.  
  ![Screenshot van gebruikers - UPN-naam dropdown identificeren](./media/active-directory-application-proxy-sso-using-kcd/app_proxy_sso_diff_id_connect_settings.png)  
2. In de instellingen van configuratie van toepassingen voor de toepassing die u wilt wijzigen, selecteert u de **Overgedragen aanmeldings-id** moet worden gebruikt:
  - Beginsel van gebruikersnaam:joe@contoso.com  
  - Alternatieve naam principe:joed@contoso.local  
  - Gebruikersnaam deel van het beginsel van gebruikersnaam: joe  
  - Deel van het beginsel van alternatieve gebruikersnaam Gebruikersnaam: joed  
  - SAM-accountnaam op ruimten: afhankelijk van prem domeinconfiguratie

  ![Gedelegeerde login ID vervolgkeuzelijst screenshot](./media/active-directory-application-proxy-sso-using-kcd/app_proxy_sso_diff_id_upn.png)  

### <a name="troubleshooting-sso-for-different-identities"></a>Problemen met eenmalige aanmelding voor verschillende identiteiten
Als er een fout in de SSO-proces is er in het gebeurtenislogboek van de Connector computer zoals beschreven in [problemen oplossen](active-directory-application-proxy-troubleshoot.md).
Maar in sommige gevallen verzoek zal worden verzonden naar de back-end-toepassing terwijl deze toepassing in verschillende HTTP-antwoorden reageren zal. Het oplossen van deze gevallen moet worden gestart door het onderzoeken van gebeurtenis-id 24029 op de computer van de Connector in het gebeurtenislogboek van de Proxy-sessie. De identiteit van de gebruiker die is gebruikt voor overdracht wordt weergegeven in het veld 'gebruiker' in de details van de gebeurtenis. Als u sessie-logboek, selecteert u **Logboeken en foutopsporing weergeven analytisch** in het gebeurtenislogboek Logboeken weergeven in menu.


## <a name="see-also"></a>Zie ook

- [Uitgeven van toepassingen met de toepassingsproxy](active-directory-application-proxy-publish.md)
- [Oplossen van problemen met de toepassingsproxy](active-directory-application-proxy-troubleshoot.md)
- [Werken met vorderingen op toepassingen](active-directory-application-proxy-claims-aware-apps.md)
- [Voorwaardelijke toegang](active-directory-application-proxy-conditional-access.md)

Bekijk de [toepassingsproxy blog](http://blogs.technet.com/b/applicationproxyblog/) voor het laatste nieuws en updates


<!--Image references-->
[1]: ./media/active-directory-application-proxy-sso-using-kcd/AuthDiagram.png
[2]: ./media/active-directory-application-proxy-sso-using-kcd/Properties.jpg
