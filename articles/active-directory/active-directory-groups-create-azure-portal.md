<properties
    pageTitle="Een nieuwe groep maken in Azure Active Directory preview | Microsoft Azure"
    description="Een groep maken in Azure Active Directory en (leden) gebruikers toevoegen aan de groep"
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


# <a name="create-a-new-group-in-azure-active-directory-preview"></a>Een nieuwe groep maken in Azure Active Directory-voorbeeld

> [AZURE.SELECTOR]
- [Azure portal](active-directory-groups-create-azure-portal.md)
- [Azure klassieke portal](active-directory-accessmanagement-manage-groups.md)
- [PowerShell](active-directory-accessmanagement-groups-settings-v2-cmdlets.md)

In dit artikel wordt uitgelegd hoe te maken en een nieuwe groep in het voorbeeld van Azure Active Directory (AD Azure) te vullen. [Wat is in het voorbeeld?](active-directory-preview-explainer.md) Een groep voor het uitvoeren van beheertaken, zoals het toewijzen van licenties of machtigingen aan een aantal gebruikers of apparaten tegelijk gebruiken.

## <a name="how-do-i-create-a-group"></a>Hoe maak ik een groep?

1. Log in om de [Azure portal](https://portal.azure.com) met een account die is een algemene admin voor de map.

2. Selecteer **meer services**, **gebruikers en groepen** in het tekstvak invoeren en selecteer vervolgens **Enter**.

  ![Gebruikersbeheer openen](./media/active-directory-groups-create-azure-portal/search-user-management.png)

3. Selecteer **alle groepen**op de bladeserver **gebruikers en groepen** .

  ![De blade groepen openen](./media/active-directory-groups-create-azure-portal/view-groups-blade.png)

4. Selecteer de opdracht **toevoegen** op het blad **gebruikers en groepen - alle groepen** .

  ![Selectie van de opdracht toevoegen](./media/active-directory-groups-create-azure-portal/add-group-command.png)

5. Op het blad **groep** toevoegen een naam en beschrijving voor de groep.

6. **Toegewezen** selecteren in het vak **type lidmaatschap** om leden te selecteren om toe te voegen aan de groep, en selecteer **leden**. Zie voor meer informatie over het beheren van het lidmaatschap van een groep dynamisch [met kenmerken voor het maken van geavanceerde regels voor lidmaatschap van de groep](active-directory-groups-dynamic-membership-azure-portal.md).

  ![Leden toevoegen selecteren](./media/active-directory-groups-create-azure-portal/select-members.png)

5. Selecteer een of meer gebruikers of apparaten toevoegen aan de groep en klik op de knop **selecteren** onderaan het blad aan de groep wilt toevoegen op het blad **leden** . Het vak **gebruiker** filters weergegeven op basis van uw post in een deel van de naam van een gebruiker of apparaat overeenkomen. Geen jokertekens worden in dit vak geaccepteerd.

6. Wanneer u klaar bent met het toevoegen van leden aan de groep, selecteer **maken** in de **groep** blade.    

  ![Bevestiging van de groep maken](./media/active-directory-groups-create-azure-portal/create-group-confirmation.png)




## <a name="additional-information"></a>Als u meer informatie

Deze artikelen bevatten aanvullende informatie over Azure Active Directory.

* [Zie bestaande groepen](active-directory-groups-view-azure-portal.md)
* [Instellingen van een groep beheren](active-directory-groups-settings-azure-portal.md)
* [Leden van een groep beheren](active-directory-groups-members-azure-portal.md)
* [Lidmaatschap van een groep beheren](active-directory-groups-membership-azure-portal.md)
* [Dynamische regels beheren voor gebruikers in een groep](active-directory-groups-dynamic-membership-azure-portal.md)
