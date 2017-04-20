<properties
    pageTitle="De groepen die de groep lid van in de voorvertoning Azure Active Directory is beheren | Microsoft Azure"
    description="Groepen kunnen andere groepen in Azure Active Directory bevatten. Hier ziet u hoe deze lidmaatschappen beheren."
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


# <a name="manage-the-groups-your-group-is-a-member-of-in-azure-active-directory-preview"></a>De groepen die de groep lid van in de voorvertoning Azure Active Directory is beheren

Groepen kunnen andere groepen in de voorvertoning Azure Active Directory bevatten. [Wat is in het voorbeeld?](active-directory-preview-explainer.md) Hier ziet u hoe deze lidmaatschappen beheren.

## <a name="how-do-i-find-the-groups-my-group-is-a-member-of"></a>Hoe vind ik mijn groep lid van is groepen

1.  Log in om de [Azure portal](https://portal.azure.com) met een account die is een algemene admin voor de map.

2.  Selecteer **meer services**, **gebruikers en groepen** in het tekstvak invoeren en selecteer vervolgens **Enter**.

  ![Gebruikersbeheer openen](./media/active-directory-groups-membership-azure-portal/search-user-management.png)

3.  Selecteer **alle groepen**op de bladeserver **gebruikers en groepen** .

  ![De blade groepen openen](./media/active-directory-groups-membership-azure-portal/view-groups-blade.png)

4. Selecteer een groep op de bladeserver **gebruikers en groepen - alle groepen** .

5. Groep lidmaatschappen ** op het * *Group - *groepsnaam* ** blade selecteren **.

  ![De groep lidmaatschappen blade openen](./media/active-directory-groups-membership-azure-portal/group-membership-blade.png)

6. Selecteer de opdracht **Add** uw groep toevoegen als lid van een andere groep op de blade **-groep - groepslidmaatschappen** .

7. Selecteer een groep in de blade **Groep selecteren** en selecteer vervolgens de knop **selecteren** op de onderkant van het blad. U kunt de groep slechts één groep tegelijk toevoegen. Het vak **gebruiker** filters weergegeven op basis van uw post in een deel van de naam van een gebruiker of apparaat overeenkomen. Geen jokertekens worden in dit vak geaccepteerd.

  ![Lidmaatschap van een groep toevoegen](./media/active-directory-groups-membership-azure-portal/add-group-membership.png)

8. Selecteer een groep om de groep te verwijderen als lid van een andere groep op de blade **-groep - groepslidmaatschappen** .

9. Op het blad ***groupname*** , selecteert u de opdracht **verwijderen** en Bevestig uw keuze bij de opdrachtprompt.

  ![lidmaatschap opdracht verwijderen](./media/active-directory-groups-membership-azure-portal/remove-group-membership.png)

9. Wanneer u klaar bent met het wijzigen van groepslidmaatschappen voor uw groep, selecteert u **Opslaan**.


## <a name="additional-information"></a>Als u meer informatie

Deze artikelen bevatten aanvullende informatie over Azure Active Directory.

* [Zie bestaande groepen](active-directory-groups-view-azure-portal.md)
* [Een nieuwe groep maken en leden toevoegen](active-directory-groups-create-azure-portal.md)
* [Instellingen van een groep beheren](active-directory-groups-settings-azure-portal.md)
* [Leden van een groep beheren](active-directory-groups-members-azure-portal.md)
* [Dynamische regels beheren voor gebruikers in een groep](active-directory-groups-dynamic-membership-azure-portal.md)
