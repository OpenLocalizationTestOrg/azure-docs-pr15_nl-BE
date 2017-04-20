<properties
    pageTitle="Toegang van Azure resourcetoewijzingen weergeven | Microsoft Azure"
    description="Weergeven en beheren van alle toewijzingen van op rollen gebaseerde toegangscontrole voor elke gebruiker of groep in de portal voor Azure"
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="jeffsta"/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="10/10/2016"
    ms.author="kgremban"/>

# <a name="view-access-assignments-for-users-and-groups-in-the-azure-portal---public-preview"></a>Toegang tot toewijzingen weergeven voor gebruikers en groepen in de Azure portal - Public preview

> [AZURE.SELECTOR]
- [Beheer van de toegang door de gebruiker of groep](role-based-access-control-manage-assignments.md)
- [Toegang beheren door middel](role-based-access-control-configure.md)

Met Role-Based Access Control RBAC () in de voorvertoning Azure Active Directory, kunt u toegang tot uw Azure bronnen beheren. [Wat is in het voorbeeld?](active-directory-preview-explainer.md)

RBAC is toegewezen is toegesneden omdat u kunt u de machtigingen op twee manieren:

- **Toepassingsgebied:** RBAC roltoewijzingen worden binnen het bereik van een bepaald abonnement, resource of resourcegroep. Een gebruiker toegang krijgen tot een enkele bron heeft geen toegang tot bronnen in het hetzelfde abonnement.
- **Rol:** Binnen de werkingssfeer van de toewijzing toegang wordt teruggebracht nog verder door het toewijzen van een rol. Rollen kunnen worden op hoog niveau, zoals de eigenaar of specifieke, net als de lezer van de virtuele machine.

Rollen kunnen alleen binnen het abonnement, resourcegroep of resource dat is de omvang van de toewijzing worden toegewezen. Maar u kunt alle toewijzingen toegang voor een bepaalde gebruiker of groep op één plaats bekijken.

Meer informatie over het [Gebruik roltoewijzingen voor het beheren van toegang tot de bronnen van uw abonnement Azure](role-based-access-control-configure.md)vinden.

##  <a name="view-access-assignments"></a>Access-toewijzingen weergeven

Als u de toewijzingen toegang voor een gebruiker of groep wilt opzoeken, start in Azure Active Directory in de [portal Azure](http://portal.azure.com).

1. Selecteer **Azure Active Directory**. Als deze optie niet zichtbaar in uw lijst navigatie is, selecteer **Meer Services** en Ga naar zoeken naar **Azure Active Directory**.
2. Selecteer **gebruikers en groepen**en klik vervolgens op **alle gebruikers** of **alle groepen**. Bijvoorbeeld: we richten ons op individuele gebruikers.
    ![Beheren van gebruikers en groepen in Active Directory Azure - screenshot](./media/role-based-access-control-manage-assignments/rbac_users_groups.png)
3. Zoeken naar de gebruiker met de naam of gebruikersnaam.
4. **Azure resources** selecteren op het blad van de gebruiker. Alle toewijzingen voor die gebruiker toegang worden weergegeven.

### <a name="read-permissions-to-view-assignments"></a>Leesmachtigingen voor het weergeven van toewijzingen

Deze pagina bevat alleen de toewijzingen van access die u hebt gemachtigd om te lezen. Bijvoorbeeld leestoegang hebben tot een abonnement en Ga naar de blade Azure resources toewijzingen van de gebruiker controleren. U ziet haar toewijzingen toegang voor een abonnement, maar niet zien ze ook de toewijzingen toegang heeft op abonnement B.

## <a name="delete-access-assignments"></a>Toewijzingen van access verwijderen

Uit deze blade, kunt u toewijzingen van access rechtstreeks aan een gebruiker of groep toegewezen werden verwijderen. Als de access-toewijzing is overgenomen van een bovenliggende groep, moet u aan de resource of een abonnement gaat en er voor de toewijzing.

1. Selecteer in de lijst van alle toewijzingen toegang voor een gebruiker of groep die u wilt verwijderen.
2. Selecteer **verwijderen** en vervolgens op **Ja** om te bevestigen.
    ![Access-toewijzing - screenshot verwijderen](./media/role-based-access-control-manage-assignments/delete_assignment.png)

## <a name="related-topics"></a>Verwante onderwerpen

- Aan de slag met op rollen gebaseerde toegangscontrole voor [roltoewijzingen gebruiken voor het beheren van toegang tot de bronnen van uw abonnement Azure](role-based-access-control-configure.md)
- Zie de [ingebouwde RBAC-rollen](role-based-access-built-in-roles.md)
