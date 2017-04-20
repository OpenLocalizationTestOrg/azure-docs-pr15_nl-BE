<properties
    pageTitle="Voorwaardelijke toegang voor toepassingen die worden gepubliceerd met de toepassingsproxy Azure AD"
    description="Heeft betrekking op het instellen van voorwaardelijke toegang voor toepassingen die u publiceert op afstand met behulp van AD-toepassingsproxy Azure worden benaderd."
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
    ms.date="06/22/2016"
    ms.author="kgremban"/>

# <a name="working-with-conditional-access"></a>Werken met voorwaardelijke toegang

U kunt toegangsregels voor voorwaardelijke toegang verlenen tot toepassingen die worden gepubliceerd met de Proxy. U kunt:

- Meerledige verificatie per toepassing
- Meerledige verificatie vereist, alleen wanneer gebruikers niet op het werk
- Voorkomen dat gebruikers toegang hebben tot de toepassing als ze niet op het werk

Deze regels kunnen worden toegepast voor alle gebruikers en groepen of alleen voor bepaalde gebruikers en groepen. Standaard wordt de regel toepassen op alle gebruikers die toegang tot de toepassing hebben. De regel kan echter ook worden beperkt tot gebruikers die lid van bepaalde beveiligingsgroepen zijn.  

Toegangsregels worden geëvalueerd wanneer een gebruiker toegang krijgt tot een federatieve toepassing die gebruikmaakt van 2.0 OAuth OpenID verbinding, SAML of WS-Federation. Toegangsregels zijn bovendien met 2.0 OAuth en OpenID verbinding geëvalueerd wanneer een token vernieuwen wordt gebruikt voor de verwerving van een toegangstoken.

## <a name="conditional-access-prerequisites"></a>Vereisten voor voorwaardelijke toegang

- Abonnement op Premium Azure Active Directory
- Een huurder federatieve of beheerde Azure Active Directory
- Federatieve huurders vereisen dat meerledige verificatie (MVR gesloten) worden ingeschakeld.  
    ![Regels voor access configureren - meerledige verificatie vereist](./media/active-directory-application-proxy-conditional-access/application-proxy-conditional-access.png)

## <a name="configure-per-application-multi-factor-authentication"></a>Per toepassing meerledige verificatie configureren
1. Log in als beheerder in de klassieke Azure portal.
2. Ga naar Active Directory en selecteer de map waarin u wilt inschakelen van Proxy-toepassing.
3. Klik op **toepassingen** en blader naar het gedeelte **Toegangsregels** . De sectie toegang regels worden alleen weergegeven voor gepubliceerd toepassingsproxy met toepassingen die gebruikmaken van federatieve verificatie.
4. De regel door **Toegangsregels inschakelen** **op**inschakelen.
5. Opgeven welke gebruikers en groepen aan wie de regels van toepassing. Gebruik de knop **Groep toevoegen** om een of meer groepen die de access-regel van toepassing. In dit dialoogvenster kan ook worden gebruikt voor het verwijderen van de geselecteerde groepen.  Als de regels toepassen op groepen hebt geselecteerd, wordt alleen de toegangsregels voor gebruikers die tot een van de opgegeven beveiligingsgroepen behoren worden afgedwongen.  

  - Controleren **, behalve** als expliciet uitsluiten beveiligingsgroepen van de regel, en een of meer groepen opgeven. Gebruikers die lid van een groep in de lijst Except zijn niet moet meerledige verificatie uitvoeren.  

  - Als een gebruiker is geconfigureerd met functie meerledige verificatie, heeft deze instelling voorrang boven de regels van toepassing meerledige verificatie. Dit houdt in dat een gebruiker die is geconfigureerd voor elke gebruiker meerledige verificatie meerledige verificatie uitvoeren, zelfs als ze zijn vrijgesteld van de regels voor de toepassing van meerledige verificatie vereist. Meer informatie over [meerledige verificatie en instellingen per gebruiker](../multi-factor-authentication/multi-factor-authentication.md).

6. Selecteer de access-regel die u wilt instellen:
    - **Vereisen meerledige verificatie**: gebruikers aan wie toegangsregels van toepassing zullen moeten volledige meerledige verificatie voordat u de toepassing waarvoor de regel geldt.
    - **Vereisen meerledige verificatie wanneer niet op het werk**: gebruikers die toegang proberen te krijgen tot de toepassing van een vertrouwde IP-adres niet moet meerledige verificatie uitvoeren. De vertrouwde IP-adresbereiken kunnen worden geconfigureerd op de pagina meerledige verificatie-instellingen.
    - **Blokkeren van toegang wanneer niet op het werk**: gebruikers die toegang proberen te krijgen tot de toepassing van buiten uw bedrijfsnetwerk niet mogelijk toegang te krijgen tot de toepassing.


## <a name="configuring-mfa-for-federation-services"></a>MVR gesloten voor federation-services configureren
Voor federatieve huurders, meerledige verificatie (MVR gesloten) kan worden uitgevoerd door Azure Active Directory of de lokalen op AD FS-server. MVR gesloten wordt standaard uitgevoerd op alle pagina's die worden gehost door Azure Active Directory. Uitvoeren van Windows PowerShell en de eigenschap – SupportsMFA voor het instellen van de module AD Azure wilt configureren MVR gesloten op gebouwen.

In het volgende voorbeeld ziet u hoe op ruimten MVR gesloten via de [cmdlet Set-MsolDomainFederationSettings](https://msdn.microsoft.com/library/azure/dn194088.aspx) op de huurder contoso.com inschakelen:`Set-MsolDomainFederationSettings -DomainName contoso.com -SupportsMFA $true `

Deze vlag instelt, moet de federatieve huurder AD FS-instantie meerledige verificatie worden geconfigureerd. Volg de instructies voor het [implementeren van Microsoft Azure meerledige verificatie op de ruimten](../multi-factor-authentication/multi-factor-authentication-get-started-server.md).


## <a name="see-also"></a>Zie ook

- [Werken met vorderingen op toepassingen](active-directory-application-proxy-claims-aware-apps.md)
- [Uitgeven van toepassingen met de toepassingsproxy](active-directory-application-proxy-publish.md)
- [Eenmalige inschakelen](active-directory-application-proxy-sso-using-kcd.md)
- [Uitgeven van toepassingen met behulp van uw eigen domeinnaam](active-directory-application-proxy-custom-domains.md)

Bekijk de [toepassingsproxy blog](http://blogs.technet.com/b/applicationproxyblog/) voor het laatste nieuws en updates
