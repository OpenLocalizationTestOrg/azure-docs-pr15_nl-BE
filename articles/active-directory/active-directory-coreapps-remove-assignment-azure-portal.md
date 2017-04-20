<properties
    pageTitle="Toewijzing van een gebruiker of groep verwijderen uit een onderneming in Azure Active Directory voorbeeld app | Microsoft Azure"
    description="De toewijzing van de toegang van een gebruiker of groep verwijderen uit een enterprise app in Azure Active Directory"
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
    ms.date="09/30/2016"
    ms.author="curtand"/>


# <a name="remove-a-user-or-group-assignment-from-an-enterprise-app-in-azure-active-directory-preview"></a>Een gebruiker of groepstoewijzing verwijderen uit een enterprise app in Azure Active Directory-voorbeeld

Het is eenvoudig te verwijderen van een gebruiker of groep toegang wordt toegewezen aan een van uw bedrijfstoepassingen in Azure Active Directory (AD Azure) voorbeeld. [Wat is in het voorbeeld?](active-directory-preview-explainer.md) U moet de juiste machtigingen voor het beheren van de enterprise app hebben. In het huidige voorbeeld moet u globale admin voor de map.

## <a name="how-do-i-remove-a-user-or-group-assignment"></a>Hoe verwijder ik een gebruiker of groepstoewijzing

1. Log in om de [Azure portal](https://portal.azure.com) met een account die is een algemene admin voor de map.

2. Selecteer **meer services**en selecteer vervolgens **Enter** **Azure Active Directory** in het tekstvak invoeren.

3. Op de * *Azure Active Directory - *directoryname* ** blade (dat wil zeggen de Azure AD blade voor de map die u beheert), selecteer **Enterprise applications **.

    ![Opening Enterprise apps](./media/active-directory-coreapps-remove-assignment-user-azure-portal/open-enterprise-apps.png)

4. Selecteer **alle toepassingen**op de bladeserver **bedrijfstoepassingen** . Hier ziet u een lijst van de apps die u kunt beheren.

5. Selecteer een app op het blad **bedrijfstoepassingen - alle toepassingen** .

6. Selecteer **gebruikers en groepen**op de bladeserver ***appname*** (het blad met de naam van de geselecteerde toepassing in de titel).

    ![Gebruikers of groepen selecteren](./media/active-directory-coreapps-remove-assignment-user-azure-portal/remove-app-users.png)

7. Op de ***toepassingsnaam*** **-gebruiker & groepstoewijzing** blade, een van de meer gebruikers of groepen selecteren en selecteer vervolgens de opdracht **verwijderen** . Bevestig uw keuze bij de opdrachtprompt.

    ![De opdracht verwijderen](./media/active-directory-coreapps-remove-assignment-user-azure-portal/remove-users.png)

## <a name="next-steps"></a>Volgende stappen

- [Al mijn groepen bekijken](active-directory-groups-view-azure-portal.md)
- [Een gebruiker of groep toewijzen aan een onderneming app](active-directory-coreapps-assign-user-azure-portal.md)
- [Gebruiker aanmeldingen voor een enterprise app uitschakelen](active-directory-coreapps-disable-app-azure-portal.md)
- [Wijzig de naam of het logo van een onderneming app](active-directory-coreapps-change-app-logo-user-azure-portal.md)
