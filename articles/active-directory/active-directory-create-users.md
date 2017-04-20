<properties
    pageTitle="Nieuwe gebruikers toevoegen aan Active Directory Azure | Microsoft Azure"
    description="Wordt uitgelegd hoe u nieuwe gebruikers toevoegen of wijzigen van gebruikersgegevens in Azure Active Directory."
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/22/2016"
    ms.author="curtand"/>

# <a name="add-new-users--or-users-with-microsoft-accounts-to-azure-active-directory"></a>Toevoegen van nieuwe gebruikers of gebruikers met accounts voor Microsoft Azure Active Directory

Gebruikers vullen de map toevoegen. In dit artikel wordt uitgelegd hoe het toevoegen van nieuwe gebruikers in uw organisatie en het toevoegen van gebruikers met accounts voor Microsoft. Zie [gebruikers toevoegen uit andere mappen of partnerbedrijven in Azure Active Directory](active-directory-create-users-external.md)voor meer informatie over het toevoegen van gebruikers uit andere mappen in Azure Active Directory of het toevoegen van gebruikers van partnerbedrijven. Toegevoegde gebruikers die geen beheerdersmachtigingen hebben standaard, maar u kunt rollen toewijzen aan deze op elk gewenst moment.

## <a name="add-a-user"></a>Een gebruiker toevoegen

1. Log in om de [Azure klassieke portal](https://manage.windowsazure.com) met een account die is een algemene admin voor de map.
2. Selecteer **Active Directory**en selecteer vervolgens de naam van de organisatiemap van uw.
3. Klik op het tabblad **gebruikers** en selecteer vervolgens in de opdrachtbalk **Gebruiker toevoegen**.
4. Selecteer op de pagina **ons over deze gebruiker** onder **Type gebruiker**:

    - **Nieuwe gebruiker in uw organisatie** : een nieuwe gebruikersaccount in de map toegevoegd.
    - **Gebruiker met een bestaande Microsoft-account** – toevoegt een bestaande Microsoft consumer account aan uw map (bijvoorbeeld een Outlook-account)

5. Voer een gebruikersnaam (voor de nieuwe gebruiker) of een e-mailadres (voor een gebruiker met een Microsoft-account), afhankelijk van **het Type gebruiker**.
6. Bieden een eerste en laatste naam, een beschrijvende naam en een gebruikersrol in de lijst **rollen** op **de profielpagina van de gebruiker** . Zie voor meer informatie over rollen voor gebruiker en beheerder [toewijzen beheerdersrollen in Azure AD](active-directory-assign-admin-roles.md). Geef op of u wilt een **Meerledige verificatie inschakelen** voor de gebruiker.
7. Selecteer op de pagina **tijdelijke wachtwoord aanvragen** **maken**.

> [AZURE.IMPORTANT] Als uw organisatie gebruikmaakt van meer dan één domein, moet u weten over de volgende punten wanneer u een gebruikersaccount toevoegen:
>
> - Gebruikersaccounts met de dezelfde UPN (User Principal Name) toevoegen voor domeinen, **eerste** toevoegen, bijvoorbeeld geoffgrisso@contoso.onmicrosoft.com, **gevolgd door** geoffgrisso@contoso.com.
> - **Geen** geoffgrisso@contoso.com voordat u geoffgrisso@contoso.onmicrosoft.com. Deze volgorde is belangrijk en lastig ongedaan te maken.

## <a name="change-user-information"></a>Gebruikersgegevens wijzigen

Kunt u elk gebruikerskenmerk, met uitzondering van de object-ID.

1. Open de map.
2. Klik op het tabblad **gebruikers** en selecteer vervolgens de naam weergegeven van de gebruiker die u wilt wijzigen.
3. Voer de gewenste wijzigingen aan en klik op **Opslaan**.

Als de gebruiker die u wilt wijzigen wordt gesynchroniseerd met uw Active Directory-service op locatie, kunt u de gegevens van de gebruikersaccount met behulp van deze procedure niet wijzigen. Wilt wijzigen van de gebruiker, gebruikt u uw op ruimten Active Directory-beheerprogramma's.

## <a name="guest-user-management-and-limitations"></a>Gast gebruiker management en beperkingen

Gastaccounts zijn gebruikers uit andere mappen die zijn uitgenodigd voor de map voor toegang tot documenten, toepassingen of andere Azure bronnen van SharePoint. Een Gast-account in de map heeft het onderliggende UserType kenmerk is ingesteld op 'Gast'. Gewone gebruikers (met name leden van de map) hebben het kenmerk UserType "Lid".

Gasten hebben een beperkte set rechten in de directory. Deze rechten beperken de mogelijkheid voor gasten om informatie over andere gebruikers in de directory. Gastgebruikers kunnen echter nog steeds communiceren met de gebruikers en groepen die zijn gekoppeld aan de resources die aan werkt. Gastgebruikers kunnen:

- Zie andere gebruikers en groepen die zijn gekoppeld aan een Azure-abonnement waaraan ze zijn toegewezen
- De leden van de groepen waartoe zij behoren
- Opzoeken van andere gebruikers in de directory, als zij weten dat het volledige e-mailadres van de gebruiker
- Zie een beperkt aantal kenmerken van de gebruikers die ze--beperkt opzoeken tot naam, e-mailadres, UPN (user Principal name) en miniatuurfoto weergeven
- Een lijst met gecontroleerde domeinen in Active directory
- Toestemming voor het verlenen van dezelfde machtigingen als leden in de map hebben toepassingen

## <a name="set-guest-user-access-policies"></a>Gast gebruiker-beleid instellen

In het tabblad **configureren** van een map bevat opties voor het toegangsbeheer voor gastgebruikers. Deze opties kunnen worden gewijzigd in Azure klassieke portal alleen door een globale directory-beheerder. Er is momenteel geen methode PowerShell of API.

Selecteer **Active Directory**wilt openen in het tabblad **configureren** in de klassieke Azure portal, en selecteer vervolgens de naam van de map.

![Tabblad in Azure Active Directory configureren][1]

Vervolgens kunt u de opties voor het toegangsbeheer voor gastgebruikers.

![Opties voor toegangsbeheer voor gastgebruikers][2]


## <a name="whats-next"></a>Wat is het volgende

- [Gebruikers uit andere mappen of partnerbedrijven in Azure Active Directory toevoegen](active-directory-create-users-external.md)
- [Azure AD beheren](active-directory-administer.md)
- [Wachtwoorden beheren in Azure AD](active-directory-manage-passwords.md)
- [Groepen in AD Azure beheren](active-directory-manage-groups.md)

<!--Image references-->
[1]: ./media/active-directory-create-users/RBACDirConfigTab.png
[2]: ./media/active-directory-create-users/RBACGuestAccessControls.png
