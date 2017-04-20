<properties
    pageTitle="Eenmalige aanmelding Azure Active Directory is geïntegreerd met SaaS apps |  Microsoft Azure"
    description="Enkele aanmelding, verificatie en management van apps in Azure Active Directory SaaS gecentraliseerde toegang van gebruikers inschakelen. Een overzicht van het integreren van Azure Active Directory naar SaaS apps."
    services="active-directory"
      keywords="Azure AD integreren met SaaS apps"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="09/30/2016"
    ms.author="curtand"/>

# <a name="integrate-azure-active-directory-single-sign-on-with-saas-apps"></a>Eenmalige aanmelding Azure Active Directory is geïntegreerd met SaaS apps  

> [AZURE.SELECTOR]
- [Azure portal](active-directory-enterprise-apps-manage-sso.md)
- [Azure klassieke portal](active-directory-sso-integrate-saas-apps.md)

[AZURE.INCLUDE [active-directory-sso-use-case-intro](../../includes/active-directory-sso-use-case-intro.md)]

Om te beginnen het instellen van eenmalige aanmelding voor een toepassing die u in uw organisatie hebt gebracht, wordt u een bestaande map in Azure Active Directory (AD Azure). U kunt een Azure AD-directory die u kunt via Microsoft Azure, Office 365 of Windows Intune verkrijgen. Als er twee of meer van deze, Zie [de Azure AD directory beheren](active-directory-administer.md) om te bepalen naar welke gebruiken.

## <a name="authentication"></a>Verificatie

Voor toepassingen die ondersteunen de SAML 2.0, WS-Federation of protocollen OpenID verbinding Azure Active Directory gebruikt handtekeningcertificaten voor vertrouwensrelaties tot stand brengen. Zie voor meer informatie over deze [certificaten voor federatieve eenmalige aanmelding beheren](active-directory-sso-certs.md).

Voor toepassingen die ondersteuning bieden voor alleen HTML-formulieren gebaseerde aanmelden, Azure Active Directory 'wachtwoord vaulting' gebruikt om vertrouwensrelaties tot stand brengen. Hierdoor kunnen gebruikers in uw organisatie worden automatisch aangemeld bij een SaaS-toepassing door Azure AD met gegevens van de gebruikersaccount van de SaaS-toepassing. Azure AD verzameld en veilig opgeslagen gegevens van de gebruikersaccount en het bijbehorende wachtwoord. Zie voor meer informatie [op basis van wachtwoorden eenmalige aanmelding](active-directory-appssoaccess-whatis.md#password-based-single-sign-on).

## <a name="authorization"></a>Autorisatie

Een ingerichte account kan een gebruiker wilt machtigen om een toepassing te gebruiken nadat ze zijn geverifieerd via eenmalige aanmelding. Gebruikersaanvragen is mogelijk handmatig of in sommige gevallen kunt u toevoegen of verwijderen van gebruikersgegevens vanuit de SaaS-app op basis van wijzigingen in Azure Active Directory. Zie voor meer informatie over het gebruik van bestaande Azure AD connectors voor het inrichten van geautomatiseerde [gebruiker automatisch inrichten en verwijderen van gegevens voor SaaS-toepassingen](active-directory-saas-app-provisioning.md).

Anders, u kunt handmatig gebruikersinformatie toevoegen aan een app, of gebruik andere provisioning oplossingen die beschikbaar in de markt zijn.

## <a name="access"></a>Toegang

Azure AD biedt verschillende manieren aanpasbare toepassingen aan eindgebruikers in uw organisatie implementeren. U zijn niet vergrendeld in een bepaalde implementatie of een access-oplossing. U kunt [de oplossing die het meest geschikt](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users).

## <a name="additional-considerations-for-applications-already-in-use"></a>Aanvullende overwegingen voor toepassingen al gebruik in

Het instellen van eenmalige op voor een toepassing die al in uw organisatie wordt gebruikt is een ander proces van het proces van het maken van nieuwe accounts voor een nieuwe toepassing. Er zijn een aantal voorbereidende stappen, met inbegrip van: toewijzing van gebruikersidentiteiten in de toepassing aan Azure AD identiteiten en inzicht in hoe gebruikers krijgen met aanmelden op een toepassing nadat deze is geïntegreerd.

> [AZURE.NOTE] Als u eenmalige aanmelding voor een bestaande toepassing instelt, moet u beheerdersrechten globale beide Azure AD en de SaaS-toepassing.

### <a name="mapping-user-accounts"></a>Gebruikersaccounts toewijzen

De identiteit van een gebruiker heeft meestal een unieke id die een e-mailadres of de UPN (user Principal name). U moet de koppeling (hyperlink) toepassings-id van de gebruiker aan hun respectieve Azure AD identiteit. Er zijn een aantal manieren om dit te bereiken afhankelijk van hoe de eis van de verificatie van uw toepassing.

Zie voor meer informatie over het toewijzen van toepassing identiteiten met Azure AD identiteiten [aanpassen vorderingen in het SAML-token verleend](http://social.technet.microsoft.com/wiki/contents/articles/31257.azure-active-directory-customizing-claims-issued-in-the-saml-token-for-pre-integrated-apps.aspx) en [kenmerktoewijzingen aanpassen voor de inrichting](active-directory-saas-customizing-attribute-mappings.md).

### <a name="understanding-the-users-log-in-experience"></a>Inzicht aanmeldingsgegevens van de gebruiker ervaring

Als u eenmalige aanmelding voor een toepassing die al in gebruik is geïntegreerd, is het belangrijk om te realiseren dat de gebruikerservaring worden beïnvloed. Voor alle toepassingen starten gebruikers met hun Azure AD-referenties aan te melden. Het kan ook zijn dat ze een andere portal gebruiken moeten voor toegang tot de toepassingen.

Eenmalige aanmelding voor sommige toepassingen kan worden uitgevoerd op het teken van de toepassing in de interface, maar voor andere toepassingen, moet de gebruiker via een centraal portal zoals ([Mijn Apps](http://myapps.microsoft.com) of [Office365](http://portal.office.com/myapps)) aan te melden. Meer informatie over de verschillende soorten SSO en hun ervaringen in [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](active-directory-appssoaccess-whatis.md).

Een andere waardevolle bron is *Suppressing gebruiker stemt ermee* in het artikel [Guiding ontwikkelaars](active-directory-applications-guiding-developers-for-lob-applications.md) .

## <a name="next-steps"></a>Volgende stappen


AD Azure biedt voor SaaS-toepassingen die u in de galerij App vindt, een aantal [zelfstudies over het integreren van SaaS-toepassingen](active-directory-saas-tutorial-list.md).

Als app niet in de galerij App, kunt u [toevoegen aan de galerie Azure AD App als een aangepaste toepassing](http://blogs.technet.com/b/ad/archive/2015/06/17/bring-your-own-app-with-azure-ad-self-service-saml-configuration-gt-now-in-preview.aspx).

Er is veel meer informatie over al deze kwesties in de bibliotheek Azure.com beginnen met [Wat is toegang tot toepassingen en eenmalige aanmelding met Active Directory Azure.](active-directory-appssoaccess-whatis.md).

## <a name="see-also"></a>Zie ook

- [Artikel-Index voor Toepassingsbeheer in Azure Active Directory](active-directory-apps-index.md)
