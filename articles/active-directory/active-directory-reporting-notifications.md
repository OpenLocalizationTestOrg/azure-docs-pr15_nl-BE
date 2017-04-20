<properties
    pageTitle="Azure Active Directory Reporting meldingen"
    description="Het gebruik van Active Directory reporting meldingen voor verdacht Azure modules."
    services="active-directory"
    documentationCenter=""
    authors="dhanyahk"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="03/07/2016"
    ms.author="dhanyahk"/>

# <a name="azure-active-directory-reporting-notifications"></a>Azure Active Directory Reporting meldingen

## <a name="what-reports-generate-email-notifications"></a>Welke rapporten genereren van e-mailberichten

Op dit moment is de onregelmatige aanmelden activiteit rapport triggers e-mailberichten.

## <a name="what-is-an-irregular-sign-in"></a>Wat is een 'onregelmatige inloggen'?

Onregelmatige aanmeldingen zijn die welke zijn vastgesteld door onze machine learning algoritmen op basis van een "onmogelijk reizen"-voorwaarde in combinatie met een afwijkende locatie aanmelden en het apparaat. Dit kan betekenen dat een hacker is geprobeerd aan te melden met deze account.

## <a name="who-receives-the-email-notifications"></a>Wie ontvangt de e-mailberichten?

Het e-mailbericht wordt verzonden naar alle globale beheerders die een Active Directory-Premium licentie zijn toegewezen. Deze wordt afgeleverd, zodat verzenden wij naar de admins alternatieve e-mailadres ook. Beheerders moeten opnemen aad-alerts-noreply@mail.windowsazure.com in de lijst met veilige afzenders, zodat zij het e-mailbericht niet missen.

## <a name="how-often-are-these-emails-sent"></a>Hoe vaak worden deze verzonden e-mails?

Het e-mailbericht wordt verzonden of 10 nieuwe onregelmatige aanmelden activiteiten plaatsvinden in de laatste 30 dagen nadat het laatste e-mailbericht is verzonden, indien deze lager is.

## <a name="how-do-i-access-the-report-mentioned-in-the-email"></a>Hoe krijg ik toegang tot het rapport vermeld in de e-mail?

Wanneer u op de koppeling klikt, wordt u omgeleid naar de rapportpagina in de Azure klassieke portal. U moet toegang tot het rapport, worden beide:

- Een beheerder of een co-beheerder van uw abonnement Azure

- Een globale beheerder in de directory, en een Active Directory-Premium licentie toegewezen. Zie [edities Azure Active Directory](active-directory-editions.md)voor meer informatie.

## <a name="can-i-turn-off-these-emails"></a>Kan ik deze e-mails uitschakelen?

Ja, als u wilt uitschakelen berichten over afwijkende aanmeldingen binnen de klassieke Azure portal, klikt u op **configureren**en selecteer **uitgeschakeld** in het gedeelte **meldingen** .

## <a name="whats-next"></a>Wat is het volgende
- Nieuwsgierig naar welke beveiligings-, controle- en activiteit rapporten beschikbaar zijn? [Azure AD beveiliging, controle en Activiteitenoverzichten](active-directory-view-access-usage-reports.md) uitchecken
- [Aan de slag met Azure Active Directory Premium](active-directory-get-started-premium.md)
- [Huisstijlen aanmelden en toegang deelvenster pagina's toevoegen](active-directory-add-company-branding.md)
