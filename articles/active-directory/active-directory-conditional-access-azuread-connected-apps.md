<properties
    pageTitle="Azure voorwaardelijke toegang voor SaaS Apps | Microsoft Azure"
    description="Voorwaardelijke toegang in Azure AD kunt u toegangsregels per toepassing meerledige verificatie en de mogelijkheid voor het blokkeren van toegang voor gebruikers die niet in een vertrouwd netwerk configureren. "
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="markvi"/>

# <a name="getting-started-with-azure-active-directory-conditional-access"></a>Aan de slag met Azure Active Directory voorwaardelijke toegang

Azure Active Directory voorwaardelijke toegang voor [SaaS](https://azure.microsoft.com/overview/what-is-saas/) -toepassingen en Azure AD verbonden apps kunt die u de voorwaardelijke toegang op basis van de groep, de locatie en de gevoeligheid van de toepassing configureren. 

U kunt met voorwaardelijke toegang op basis van de gevoeligheid van de toepassing, toegangsregels meerledige verificatie (MVR gesloten) per toepassing instellen. MVR gesloten per toepassing biedt de mogelijkheid voor het blokkeren van toegang voor gebruikers die zich niet in een vertrouwd netwerk. U kunt de MVR gesloten regels toepassen voor alle gebruikers die zijn toegewezen aan de toepassing, of alleen voor gebruikers binnen de opgegeven beveiligingsgroepen.  Gebruikers kunnen worden uitgesloten van de verplichting van de MVR gesloten als zij toegang hebben tot de toepassing van een IP-adres in het netwerk van de organisatie.

Deze mogelijkheden zijn beschikbaar voor klanten die een Azure Active Directory Premium licentie hebt aangeschaft.

## <a name="scenario-prerequisites"></a>Scenario-vereisten
* Licentie voor Premium Azure Active Directory

* Federatieve of beheerde Azure Active Directory huurder

* Federatieve huurders moeten dat meerledige verificatie worden ingeschakeld.

## <a name="configure-per-application-access-rules"></a>Regels per toepassing toegang configureren

In deze sectie wordt beschreven hoe de regels per toepassing toegang configureren.

1. Log in om de Azure klassieke portal met een account die is een globale beheerder voor Azure AD.
2. Selecteer **Active Directory**in het linkerdeelvenster.
3. Selecteer op het tabblad map de map.
4. Selecteer op het tabblad **toepassingen** .
5. Selecteer de toepassing die voor de regel ingesteld.
6. Selecteer in het tabblad **configureren** .
7. Ga naar het gedeelte toegang regels. Selecteer de gewenste access-regel.
8. Geef de gebruikers op die de regel van toepassing.
9. Het beleid inschakelen **ingeschakeld op**selecteren.

##<a name="understanding-access-rules"></a>Wat zijn de regels voor access

Deze sectie geeft een gedetailleerde beschrijving van de toegangsregels in de toepassing van Azure Conditional Access ondersteund.

### <a name="specifying-the-users-the-access-rules-apply-to"></a>Opgeven van de gebruikers de access-regels van toepassing op

Standaard wordt het beleid toepassen op alle gebruikers die toegang tot de toepassing hebben. U kunt echter ook het beleid beperken tot gebruikers die lid van de opgegeven beveiligingsgroepen zijn. De knop **Groep toevoegen** wordt aan een of meer groepen selecteren in het groepsdialoogvenster waarop de toegangsregel van toepassing gebruikt. In dit dialoogvenster kan ook worden gebruikt voor het verwijderen van de geselecteerde groepen. Als de regels toepassen op groepen hebt geselecteerd, wordt alleen de toegangsregels voor gebruikers die tot een van de opgegeven beveiligingsgroepen behoren worden afgedwongen.

Beveiligingsgroepen kunnen ook worden expliciet uitgesloten van het beleid door de optie **behalve** en een of meer groepen op te geven. Gebruikers die lid zijn van een groep in de lijst **, behalve** worden niet meerledige verificatie vereist, zelfs als zij lid van een groep die de access-regel is van toepassing op.
De onderstaande toegangsregel wordt verplicht voor alle gebruikers in de groep Managers meerledige verificatie toegang tot de toepassing.

![Het instellen van regels voor voorwaardelijke toegang met de MVR gesloten](./media/active-directory-conditional-access-azuread-connected-apps/conditionalaccess-saas-apps.png)

## <a name="conditional-access-rules-with-mfa"></a>Regels voor voorwaardelijke toegang met de MVR gesloten
Als een gebruiker is geconfigureerd met functie meerledige verificatie, wordt deze instelling voor de gebruiker de regels meerledige verificatie van de app combineren. Dit betekent dat een gebruiker die is geconfigureerd voor elke gebruiker meerledige verificatie moet meerledige verificatie uitvoeren, zelfs als ze zijn vrijgesteld van de regels van toepassing meerledige verificatie. Meer informatie over instellingen voor meerledige verificatie en per gebruiker.

### <a name="access-rule-options"></a>Opties voor de regel
De volgende opties worden ondersteund:

* **Meerledige verificatie vereist**: de gebruikers aan wie de toegangsregels zijn van toepassing, moeten volledig meerledige verificatie voordat u de toepassing waarin het beleid is van toepassing op.

* **Meerledige verificatie wanneer niet op het werk**: een gebruiker die afkomstig zijn uit een vertrouwde IP-adres niet moet meerledige verificatie uitvoeren. De vertrouwde IP-adresbereiken kunnen worden geconfigureerd op de pagina meerledige verificatie-instellingen.

* **Blokkeren van toegang wanneer niet op het werk**: een gebruiker die niet afkomstig zijn uit een vertrouwde IP-adres worden geblokkeerd. De vertrouwde IP-adresbereiken kunnen worden geconfigureerd op de pagina meerledige verificatie-instellingen.

### <a name="setting-rule-status"></a>Regelstatus instellen
Regelstatus van Access kunt de regels in of uit te schakelen. Wanneer de toegangsregels uitgeschakeld zijn, worden de vereiste meerledige verificatie wordt niet afgedwongen.

### <a name="access-rule-evaluation"></a>Toegang tot evaluatie van de regels

Toegangsregels worden geëvalueerd wanneer een gebruiker toegang krijgt tot een federatieve toepassing die gebruikmaakt van 2.0 OAuth OpenID verbinding, SAML of WS-Federation. Bovendien toegangsregels geëvalueerd wanneer de 2.0 OAuth en OpenID verbinding maken met een token vernieuwen een toegangstoken verkrijgen. Als de evaluatie van beleid mislukt wanneer u een token vernieuwen gebruikt, wordt de fout **invalid_grant** geretourneerd, dit betekent dat de gebruiker moet opnieuw naar de client te verifiëren.

###<a name="configure-federation-services-to-provide-multi-factor-authentication"></a>Meerledige verificatie federation services configureren

Voor federatieve huurders, MVR gesloten kan worden uitgevoerd door Azure Active Directory of de lokalen op AD FS-server.

MVR gesloten wordt standaard uitgevoerd op een pagina die wordt gehost door Azure Active Directory. MVR gesloten op locatie configureren, moet u de eigenschap **– SupportsMFA** instellen op **true** in Azure Active Directory, met de Azure AD-module voor Windows PowerShell.

In het volgende voorbeeld ziet u hoe op ruimten MVR gesloten via de [cmdlet Set-MsolDomainFederationSettings](https://msdn.microsoft.com/library/azure/dn194088.aspx) op de huurder contoso.com inschakelen:

    Set-MsolDomainFederationSettings -DomainName contoso.com -SupportsMFA $true

Deze vlag instelt, moet de federatieve huurder AD FS-instantie meerledige verificatie worden geconfigureerd. Volg de instructies voor het [implementeren van Azure meerledige verificatie op gebouwen](../multi-factor-authentication/multi-factor-authentication-get-started-server.md).

## <a name="related-articles"></a>Verwante artikelen

- [Toegang tot Office 365 en andere apps beveiligen verbonden met Azure Active Directory](active-directory-conditional-access.md)
- [Artikel-Index voor Toepassingsbeheer in Azure Active Directory](active-directory-apps-index.md)
