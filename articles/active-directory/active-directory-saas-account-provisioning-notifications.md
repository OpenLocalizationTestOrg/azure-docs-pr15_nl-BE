<properties
    pageTitle="Meldingen ingericht account | Microsoft Azure"
    description="Leren om ervoor te zorgen dat u wordt gewaarschuwd van problemen die betrekking hebben op gebruikersaanvragen die uw aandacht nodig doordat account meldingen wordt ingericht."
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
    ms.date="10/10/2016"
    ms.author="markusvi"/>


# <a name="account-provisioning-notifications"></a>Account meldingen inrichten

Met de gebruiker wordt ingericht, kunt u het proces van het beheer van gebruikers in de SaaS-toepassingen van derden te automatiseren. <br>
Dit is een geautomatiseerd proces, uw interactie met dit proces is van tijd tot tijd nodig. <br>
Dit is bijvoorbeeld het geval wanneer het wachtwoord van de account die u hebt geconfigureerd voor het uitwisselen van gegevens met een derde partij SaaS-toepassing is verlopen. 

Meldingen ingericht account is ingeschakeld, kunt u ervoor zorgen dat u wordt gewaarschuwd van kwesties die betrekking hebben op gebruikersaanvragen die uw aandacht nodig.

U meldingen wordt ingericht als onderdeel van uw configuratie voor een derde partij SaaS-applicatie van gebruikers account deactiveren of activeren.

![Gebruikersaanvragen][1] 



Meldingen ingericht account activeren, schakelt u het selectievakje gerelateerde op de pagina **bevestiging** dialoogvenster en typt u het e-mailalias van de ontvanger.

![Account meldingen inrichten][2]
 


Kunt u een distributielijst als ontvanger; het is echter belangrijk te weten dat de e-mailmelding bevat koppelingen naar rapporten die alleen toegankelijk zijn voor de beheerders Azure AD.

Hebt u een account ingericht meldingen ingeschakeld, ontvangt u e-mailberichten over belangrijke problemen die betrekking op gebruikersaanvragen hebben. Echter om te voorkomen dat een e-mailbericht overbelasting, ontvangt u alleen een e-mailmelding per dag voor elke SaaS-applicatie die voor de e-mailmelding is ingeschakeld.


##<a name="related-articles"></a>Verwante artikelen

- [Artikel-Index voor Toepassingsbeheer in Azure Active Directory](active-directory-apps-index.md)
- [Automatiseren van de gebruiker wordt ingericht/Deprovisioning voor SaaS-Apps](active-directory-saas-app-provisioning.md)
- [Kenmerktoewijzingen voor gebruikersaanvragen aanpassen](active-directory-saas-customizing-attribute-mappings.md)
- [Het schrijven van expressies voor het toewijzen van kenmerken](active-directory-saas-writing-expressions-for-attribute-mappings.md)
- [Filters voor gebruikersaanvragen scoping](active-directory-saas-scoping-filters.md)
- [Met behulp van SCIM automatisch inrichten van gebruikers en groepen van Azure Active Directory voor toepassingen inschakelen](active-directory-scim-provisioning.md)
- [Lijst met zelfstudies over het integreren van SaaS-toepassingen](active-directory-saas-tutorial-list.md)



<!--Image references-->
[1]: ./media/active-directory-saas-account-provisioning-notifications/ic766307.png
[2]: ./media/active-directory-saas-account-provisioning-notifications/ic766308.png