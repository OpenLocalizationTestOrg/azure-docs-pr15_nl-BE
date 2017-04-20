<properties
    pageTitle="Gebruik toegangsbeheer op basis van rollen in Azure portal | Microsoft Azure"
    description="Beheer van toegang aan de slag met toegangsbeheer op basis van rollen in de Portal Azure. Roltoewijzingen machtigingen toe te wijzen aan uw resources gebruiken."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="10/10/2016"
    ms.author="kgremban"/>

# <a name="use-role-assignments-to-manage-access-to-your-azure-subscription-resources"></a>Roltoewijzingen gebruiken voor het beheren van toegang tot de bronnen van uw abonnement Azure

> [AZURE.SELECTOR]
- [Beheer van de toegang door de gebruiker of groep](role-based-access-control-manage-assignments.md)
- [Toegang beheren door middel](role-based-access-control-configure.md)

Azure Role-Based Access Control RBAC () kunt fijnmazig toegangsbeheer voor Azure. RBAC gebruikt, kunt u geven de hoeveelheid toegang die gebruikers nodig hebben voor het uitvoeren van hun taken. In dit artikel kunt u leren werken met RBAC in Azure portal. Desgewenst kunt u meer informatie over hoe RBAC helpt u bij het beheren van toegang Zie [toegangsbeheer op basis van rollen](role-based-access-control-what-is.md).

## <a name="view-access"></a>Toegang weergeven
U kunt zien wie toegang heeft tot een resource, resourcegroep of abonnement van de belangrijkste blade in [Azure portal](https://portal.azure.com). We willen bijvoorbeeld zien wie toegang heeft tot een van onze resourcegroepen:

1. Selecteer de **resourcegroepen** in de navigatiebalk aan de linkerkant.  
    ![Resourcegroepen - pictogram](./media/role-based-access-control-configure/resourcegroups_icon.png)
2. Selecteer de naam van de resourcegroep van de bladeserver **resourcegroepen** .
3. **Toegangsbeheer (IAM)** Selecteer in het linkermenu.  
4. De Access control blade geeft een overzicht van alle gebruikers, groepen en toepassingen die toegang is verleend aan de resourcegroep.  

    ![Gebruikers blade - overgenomen vs toegewezen screenshot van access](./media/role-based-access-control-configure/view-access.png)

Bericht dat sommige gebruikers zijn **toegewezen** benaderen terwijl anderen **overgenomen** . Toegang is met name aan de resourcegroep is toegewezen of overgenomen van een toewijzing aan het bovenliggende abonnement.

> [AZURE.NOTE] Klassieke abonnement beheerders en co-beheerders worden beschouwd als eigenaars van het abonnement in het nieuwe model van RBAC.


## <a name="add-access"></a>Toegang toevoegen
U toekennen vanuit de resource, resourcegroep of abonnement dat is de omvang van de toewijzing van rol.

1. Selecteer **toevoegen** op het blad Access control.  
2. Selecteer de rol die u wilt toewijzen in het blad **een rol te selecteren** .
3. Selecteer de gebruiker, groep of toepassing in de map die u wilt toegang tot de. U kunt de map met de weergegeven namen, e-mailadressen en object-id's zoeken.  

    ![Toevoegen van gebruikers blade - screenshot zoeken](./media/role-based-access-control-configure/grant-access2.png)

4. Klik op **OK** om de toewijzing. De voortgang wordt bijgehouden door de pop-up op **gebruiker toevoegen** .  
    ![Gebruiker voortgangsbalk - schermafbeelding toevoegen](./media/role-based-access-control-configure/addinguser_popup.png)

Na het toevoegen van een roltoewijzing is, wordt weergegeven op het blad van de **gebruikers** .

## <a name="remove-access"></a>Toegang verwijderen

1. Selecteer de toewijzing van de rol van de bladeserver Access control.
2. Selecteer **verwijderen** in de blade toewijzing details.  
3. Selecteer **Ja** om te bevestigen.  
    ![Gebruikers blade - screenshot rol verwijderen](./media/role-based-access-control-configure/remove-access1.png)

Overgenomen toewijzingen kunnen niet worden verwijderd. In de onderstaande afbeelding ziet dat de knop verwijderen wordt grijs weergegeven. In plaats daarvan de **Toegewezen aan** details bekijken. Ga naar de resource in de lijst er als de van roltoewijzing wilt verwijderen.

![Gebruikers blade - overgenomen toegang uitschakelen verwijderen knop schermafbeelding](./media/role-based-access-control-configure/remove-access2.png)

## <a name="other-tools-to-manage-access"></a>Andere hulpprogramma's voor het beheren van toegang
U kunt rollen toewijzen en beheren van toegang met Azure RBAC opdrachten in programma's dan de Azure portal.  Volg de koppelingen voor meer informatie over de vereisten en aan de slag met Azure RBAC-opdrachten.

- [Azure PowerShell](role-based-access-control-manage-access-powershell.md)
- [Azure Interface met opdrachtregel](role-based-access-control-manage-access-azure-cli.md)
- [REST-API](role-based-access-control-manage-access-rest.md)

## <a name="next-steps"></a>Volgende stappen
- [Een access-rapport wijzigen geschiedenis maken](role-based-access-control-access-change-history-report.md)
- Zie de [ingebouwde RBAC-rollen](role-based-access-built-in-roles.md)
- Uw eigen [aangepaste rollen in Azure RBAC](role-based-access-control-custom-roles.md) definiëren
