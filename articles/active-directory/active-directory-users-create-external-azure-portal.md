<properties
    pageTitle="Gebruikers uit andere mappen toevoegen of partnerbedrijven in Azure Active Directory preview | Microsoft Azure"
    description="Hoe gebruikers toevoegen of wijzigen van gebruikersgegevens in Azure Active Directory, met inbegrip van Gast en externe gebruikers."
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
    ms.topic="article"
    ms.date="09/12/2016"
    ms.author="curtand"/>

# <a name="add-users-from-other-directories-or-partner-companies-in-azure-active-directory-preview"></a>Gebruikers uit andere mappen of partnerbedrijven in Azure Active Directory voorbeeldweergave toevoegen

> [AZURE.SELECTOR]
- [Azure portal](active-directory-users-create-external-azure-portal.md)
- [Azure klassieke portal](active-directory-create-users-external.md)

In dit artikel wordt uitgelegd hoe u gebruikers toe te voegen vanuit andere mappen in de voorvertoning Azure Active Directory (AD Azure) of partnerbedrijven. [Wat is in het voorbeeld?](active-directory-preview-explainer.md) Zie voor meer informatie over het toevoegen van nieuwe gebruikers in uw organisatie en het toevoegen van gebruikers met accounts voor Microsoft [de nieuwe gebruikers toevoegen aan Azure Active Directory](active-directory-users-create-azure-portal.md). Toegevoegde gebruikers die geen beheerdersmachtigingen hebben standaard, maar u kunt rollen toewijzen aan deze op elk gewenst moment.

## <a name="add-a-user"></a>Een gebruiker toevoegen

1.  Log in om de [Azure portal](https://portal.azure.com) met een account die is een algemene admin voor de map.

2.  Selecteer **meer services**, **gebruikers en groepen** in het tekstvak invoeren en selecteer vervolgens **Enter**.

    ![Gebruikersbeheer openen](./media/active-directory-users-create-external-azure-portal/create-users-user-management.png)

3.  Op het blad **gebruikers en groepen** **gebruikers**selecteren en selecteer vervolgens **toevoegen**.

    ![Selectie van de opdracht toevoegen](./media/active-directory-users-create-external-azure-portal/create-users-add-command.png)

4. Op het blad **gebruiker** bieden een weergegeven naam in het vak **naam** en de naam van de gebruiker aanmelden **naam**in.

5. Kopieer of anders ziet het gegenereerde wachtwoord zodat u deze informatie aan de gebruiker verstrekt kunt nadat dit proces voltooid is.

6. Selecteer **profiel** eerst gebruikers toevoegen en achternaam, een functietitel en de naam van een afdeling.
    
    ![Het gebruikersprofiel te openen](./media/active-directory-users-create-external-azure-portal/create-users-user-profile.png)

    - **Groepen** de gebruiker toevoegen aan een of meer groepen selecteren.

        ![Een gebruiker toevoegen aan groepen](./media/active-directory-users-create-external-azure-portal/create-users-user-groups.png)

    - Selecteer de **rol binnen de organisatie** toewijzen aan de gebruiker aan een rol uit de lijst met **rollen** . Zie voor meer informatie over rollen voor gebruiker en beheerder [toewijzen beheerdersrollen in Azure AD](active-directory-assign-admin-roles.md).

        ![Een gebruiker toewijzen aan een rol](./media/active-directory-users-create-external-azure-portal/create-users-assign-role.png)

7. Selecteer **maken**.

8. Het gegenereerde wachtwoord voor de nieuwe gebruiker veilig verspreiden zodat de gebruiker zich kan aanmelden.

> [AZURE.IMPORTANT] Als uw organisatie gebruikmaakt van meer dan één domein, moet u weten over de volgende punten wanneer u een gebruikersaccount toevoegen:
>
> - Gebruikersaccounts met de dezelfde UPN (User Principal Name) toevoegen voor domeinen, **eerste** toevoegen, bijvoorbeeld geoffgrisso@contoso.onmicrosoft.com, **gevolgd door** geoffgrisso@contoso.com.
> - **Geen** geoffgrisso@contoso.com voordat u geoffgrisso@contoso.onmicrosoft.com. Deze volgorde is belangrijk en lastig ongedaan te maken.

Als u gegevens voor een gebruiker wiens identiteit wordt gesynchroniseerd met uw Active Directory-service op locatie wijzigt, kunt u de gebruikersgegevens in de klassieke Azure portal niet wijzigen. De om gebruikersinformatie te wijzigen, gebruikt u uw op ruimten Active Directory-beheerprogramma's.


## <a name="whats-next"></a>Wat is het volgende

- [Een gebruiker toevoegen](active-directory-users-create-azure-portal.md)
- [Een gebruikerswachtwoord in de nieuwe portal Azure opnieuw instellen](active-directory-users-reset-password-azure-portal.md)
- [Een gebruiker toewijzen aan een rol in uw advertentie Azure](active-directory-users-assign-role-azure-portal.md)
- [Gegevens over het werk van een gebruiker wijzigen](active-directory-users-work-info-azure-portal.md)
- [Gebruikersprofielen beheren](active-directory-users-profile-azure-portal.md)
- [Een gebruiker in uw Azure advertentie verwijderen](active-directory-users-delete-user-azure-portal.md)
