<properties
    pageTitle="Gebruiker aanmeldingen voor een onderneming in Azure Active Directory voorbeeld app uitschakelen | Microsoft Azure"
    description="Het uitschakelen van een bedrijfstoepassing zodat geen enkele gebruiker bij het in Azure Active Directory aanmelden kunnen"
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
    ms.date="10/17/2016"
    ms.author="curtand"/>


# <a name="disable-user-sign-ins-for-an-enterprise-app-in-azure-active-directory-preview"></a>Gebruiker aanmeldingen voor een onderneming in Azure Active Directory voorbeeld app uitschakelen

Het is eenvoudig een bedrijfstoepassing uitschakelen zodat geen enkele gebruiker bij het in Azure Active Directory (AD Azure) voorbeeld aanmelden kunnen. [Wat is in het voorbeeld?](active-directory-preview-explainer.md) U moet de juiste machtigingen voor het beheren van de enterprise app hebben. In het huidige voorbeeld moet u globale admin voor de map.

## <a name="how-do-i-disable-user-sign-ins"></a>Hoe kan ik de aanmeldingen van gebruikers uitschakelen?

1. Log in om de [Azure portal](https://portal.azure.com) met een account die is een algemene admin voor de map.

2. Selecteer **meer services**en selecteer vervolgens **Enter** **Azure Active Directory** in het tekstvak invoeren.

3. Op de * *Azure Active Directory - *directoryname* ** blade (dat wil zeggen de Azure AD blade voor de map die u beheert), selecteer **Enterprise applications **.

    ![Opening Enterprise apps](./media/active-directory-coreapps-disable-app-azure-portal/open-enterprise-apps.png)

4. Selecteer **alle toepassingen**op de bladeserver **bedrijfstoepassingen** . Ziet u een lijst van de apps die u kunt beheren.

5. Selecteer een app op het blad **bedrijfstoepassingen - alle toepassingen** .

6. Selecteer **Eigenschappen**in het blad ***appname*** (het blad met de naam van de geselecteerde toepassing in de titel).

    ![De opdracht Alles toepassingen selecteren](./media/active-directory-coreapps-disable-app-azure-portal/select-app.png)

7. Op de ***toepassingsnaam*** **-Eigenschappen** selecteren **niet** voor blade **ingeschakeld voor gebruikers aanmelden?**.

8. Selecteer de opdracht **Opslaan** .

## <a name="next-steps"></a>Volgende stappen

- [Mijn groepen bekijken](active-directory-groups-view-azure-portal.md)
- [Een gebruiker of groep toewijzen aan een onderneming app](active-directory-coreapps-assign-user-azure-portal.md)
- [Toewijzing van een gebruiker of groep verwijderen uit een enterprise app](active-directory-coreapps-remove-assignment-azure-portal.md)
- [Wijzig de naam of het logo van een onderneming app](active-directory-coreapps-change-app-logo-user-azure-portal.md)
