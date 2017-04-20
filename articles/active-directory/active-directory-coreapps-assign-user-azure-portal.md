<properties
    pageTitle="Een gebruiker of groep toewijzen aan een onderneming in Azure Active Directory voorbeeld app | Microsoft Azure"
    description="Het selecteren van een enterprise app aan het toewijzen van een gebruiker of groep in Azure Active Directory"
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
    ms.date="10/03/2016"
    ms.author="curtand"/>

# <a name="assign-a-user-or-group-to-an-enterprise-app-in-azure-active-directory-preview"></a>Een gebruiker of groep toewijzen aan een onderneming app in Azure Active Directory-voorbeeld

Het is eenvoudig om een gebruiker of groep toewijzen aan uw bedrijfstoepassingen in Azure Active Directory (AD Azure) voorbeeld. [Wat is in het voorbeeld?](active-directory-preview-explainer.md) U moet de juiste machtigingen voor het beheren van de enterprise app hebben. In het huidige voorbeeld moet u globale admin voor de map.

## <a name="how-do-i-assign-user-access-to-an-enterprise-app"></a>Hoe wijs ik toegang tot een enterprise app?

1. Log in om de [Azure portal](https://portal.azure.com) met een account die is een algemene admin voor de map.

2. Selecteer **meer services**en selecteer vervolgens **Enter**Azure Active Directory in het tekstvak invoeren.

3. Op de * *Azure Active Directory - *directoryname* ** blade (dat wil zeggen de Azure AD blade voor de map die u beheert), selecteer **Enterprise applications **.

    ![Opening Enterprise apps](./media/active-directory-coreapps-assign-user-azure-portal/open-enterprise-apps.png)

4. Selecteer **alle toepassingen**op de bladeserver **bedrijfstoepassingen** . Hier ziet u een lijst van de apps die u kunt beheren.

5. Selecteer een app op het blad **bedrijfstoepassingen - alle toepassingen** .

6. Selecteer **gebruikers en groepen**op de bladeserver ***appname*** (het blad met de naam van de geselecteerde toepassing in de titel).

    ![De opdracht Alles toepassingen selecteren](./media/active-directory-coreapps-assign-user-azure-portal/select-app-users.png)

7. Op de ***toepassingsnaam*** **-gebruiker & groepstoewijzing** blade, selecteert u de opdracht **toevoegen** .

8. Selecteer **gebruikers en groepen**op de bladeserver **Toewijzing toevoegen** .

    ![Een gebruiker of groep toewijzen aan de app.](./media/active-directory-coreapps-assign-user-azure-portal/assign-users.png)

9. Op het blad **gebruikers en groepen** selecteren een of meer gebruikers of groepen uit de lijst en selecteer vervolgens de knop **selecteren** op de onderkant van het blad.

10. Selecteer de **rol**op het blad **Toewijzing toevoegen** . Selecteer een rol toe te passen op de geselecteerde gebruikers of groepen, op het blad **Rol selecteren** en selecteer vervolgens de knop **OK** onderaan het blad.

11. Selecteer de knop **toewijzen** aan de onderkant van het blad op het blad **Toewijzing toevoegen** . Toegewezen gebruikers of groepen heeft de machtigingen die zijn gedefinieerd door de geselecteerde rol voor de toepassing van deze onderneming.

## <a name="next-steps"></a>Volgende stappen

- [Al mijn groepen bekijken](active-directory-groups-view-azure-portal.md)
- [Toewijzing van een gebruiker of groep verwijderen uit een enterprise app](active-directory-coreapps-remove-assignment-azure-portal.md)
- [Gebruiker aanmeldingen voor een enterprise app uitschakelen](active-directory-coreapps-disable-app-azure-portal.md)
- [Wijzig de naam of het logo van een onderneming app](active-directory-coreapps-change-app-logo-user-azure-portal.md)
