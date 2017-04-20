
<properties
    pageTitle="Azure Active Directory voorwaardelijke toegang technische naslaginformatie | Microsoft Azure"
    description="Azure Active Directory gecontroleerd met voorwaardelijke toegang beheren, de specifieke voorwaarden die u kiest bij het verifiëren van de gebruiker en voor het toestaan van toegang tot de toepassing. Zodra deze voorwaarden wordt voldaan, wordt de gebruiker geverifieerd en toegang hebben tot de toepassing."
    services="active-directory"
    documentationCenter=""
    authors="MarkusVi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity" 
    ms.date="10/20/2016"
    ms.author="markvi"/>

# <a name="azure-active-directory-conditional-access-technical-reference"></a>Azure Active Directory voorwaardelijke toegang technische naslaginformatie

## <a name="services-enabled-with-conditional-access"></a>Diensten met voorwaardelijke toegang ingeschakeld
Regels voor voorwaardelijke toegang worden via verschillende Azure AD toepassingstypen ondersteund. Deze lijst bevat:

- Federatieve toepassingen uit de galerie Azure AD toepassing
- Wachtwoord SSO-toepassingen uit de galerie Azure AD toepassing
- Toepassingen die zijn geregistreerd bij de toepassingsproxy Azure
- Ontwikkelde lijn van bedrijfs- en huurder met meerdere toepassingen geregistreerd met Azure AD
- Visual Studio Online
- Azure Remote App
-   Dynamics CRM
- Microsoft Office 365 Yammer
- Microsoft Office 365 Exchange Online
- Microsoft Office 365 SharePoint Online (met inbegrip van OneDrive voor bedrijven)


## <a name="enable-access-rules"></a>Regels voor access inschakelen

Elke regel kan worden ingeschakeld of uitgeschakeld op een per toepassing grondslagen. Wanneer regels zijn **op** worden ze ingeschakeld en afgedwongen voor gebruikers met toegang tot de toepassing. Wanneer ze **uitgeschakeld zijn** ze niet gebruikt en is niet van invloed op de gebruikers aanmelden ervaring.

## <a name="applying-rules-to-specific-users"></a>Regels toepassen op specifieke gebruikers
Regels kunnen worden toegepast op bepaalde groepen gebruikers op basis van de beveiligingsgroep door **Toepassen op**te stellen. **Toepassen op** kan worden ingesteld op **Alle gebruikers** of **groepen**. Als **Alle gebruikers** de regels van toepassing op elke gebruiker met toegang tot de toepassing. Met de optie **groepen** kunnen specifieke beveiligings- en distributiegroepen worden geselecteerd voor deze groepen alleen regels worden afgedwongen.

Wanneer u een regel implementeert, is het gebruikelijk dat een beperkt aantal gebruikers die lid van een groep piloting zijn eerst toepassen. Nadat de regel kan worden toegepast op **Alle gebruikers**. Hierdoor wordt de regel moet worden toegepast voor alle gebruikers in de organisatie.

Bepaalde groepen kunnen ook worden vrijgesteld van beleid met de optie **behalve** . Alle leden van deze groepen wordt vrijgesteld, zelfs als ze worden weergegeven in een groep opgenomen.

## <a name="at-work-networks"></a>Netwerken 'op het werk"


Regels voor voorwaardelijke toegang die gebruikmaken van een netwerk 'op het werk' vertrouwde IP-adresbereiken die zijn geconfigureerd in Azure AD, afhankelijk of het gebruik van de claim "binnen corpnet" van AD FS. Deze regels zijn:

- Meerledige verificatie wanneer niet op het werk
- Blokkeren van toegang wanneer niet op het werk

Opties voor LDAP-netwerken "op het werk"

1. Vertrouwde IP-adresbereiken in de [configuratiepagina meerledige verificatie](../multi-factor-authentication/multi-factor-authentication-whats-next.md)configureren. Voorwaardelijke-beleid met de geconfigureerde bereiken op elke verificatieaanvraag en token kunt regels. 
2. Gebruik van de binnenkant configureren corpnet claim deze optie kan worden gebruikt met een federatieve AD FS met mappen. [Meer informatie over de binnenkant coronet claims](../multi-factor-authentication/multi-factor-authentication-whats-next.md#trusted-ips).
3. Openbare IP-adresbereiken configureren. Op het tabblad configureren kunt voor de map, u openbare IP-adressen instellen. Voorwaardelijke toegang gebruiken deze als 'op het werk' IP-adressen, hierdoor extra bereiken boven de 50 IP-adres dat wordt afgedwongen door de pagina van de MVR gesloten instelling configureren.



## <a name="rules-based-on-application-sensitivity"></a>Regels op basis van de gevoeligheid van de toepassing

Regels zijn per toepassing zodat de services van hoge waarde moet worden beveiligd, zonder invloed op de toegang tot andere services geconfigureerd. Regels voor voorwaardelijke toegang kunnen worden geconfigureerd op het tabblad **configureren** van de toepassing. 

De regels die momenteel worden aangeboden:

- **Meerledige verificatie vereist**
 - Dit beleid wordt toegepast op alle gebruikers moeten worden geverifieerd via ten minste eenmaal meerledige verificatie.
 
- **Meerledige verificatie wanneer niet op het werk**
 - Als dit beleid wordt toegepast, zullen alle gebruikers moeten meerledige verificatie ten minste eenmaal hebt uitgevoerd als ze toegang de service vanaf een externe locatie niet werken tot. Als ze van een werk naar een externe locatie gaat, ze moet uitvoeren multifactor-verificatie bij de toegang tot de service.
 
- **Blokkeren van toegang wanneer niet op het werk** 
 - Wanneer gebruikers van het werk op een externe locatie, worden ze geblokkeerd als het beleid "Block access wanneer niet op het werk" wordt toegepast.  Ze worden opnieuw kunnen als op een locatie werk toegang.


## <a name="related-topics"></a>Verwante onderwerpen

- [Toegang tot Office 365 en andere apps beveiligen verbonden met Azure Active Directory](active-directory-conditional-access.md)
- [Artikel-Index voor Toepassingsbeheer in Azure Active Directory](active-directory-apps-index.md)
