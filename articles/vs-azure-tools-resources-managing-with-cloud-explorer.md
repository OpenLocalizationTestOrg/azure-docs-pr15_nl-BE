<properties 
   pageTitle="Azure bronnen beheren met Cloud Explorer | Microsoft Azure"
   description="Informatie over het gebruik van Cloud Explorer kunt doornemen en beheren van Azure bronnen binnen Visual Studio."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags 
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="managing-azure-resources-with-cloud-explorer"></a>Azure bronnen beheren met Cloud Explorer

##<a name="overview"></a>Overzicht

Cloud Explorer is bedoeld om u snel en gemakkelijk bladeren en beheren van uw Azure bronnen binnen de Visual Studio IDE. U kunt bijvoorbeeld gebruiken voor het openen van een Web app in de [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) of in een browser of een foutopsporingsprogramma te koppelen, of u kunt de eigenschappen van een blob-container en Blob Container Editor openen.

Cloud Explorer is gebaseerd op de stack Azure resource manager, net als de [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040). Deze bronnen zoals Azure resourcegroepen en Azure services zoals logica apps en apps API begrijpt en ondersteunt [op rollen gebaseerde toegangscontrole](./active-directory/role-based-access-control-configure.md) (RBAC). Azure als resources wilt weergeven die zijn toegevoegd of gewijzigd, kiest u de knop **vernieuwen** op de werkbalk van Cloud Explorer.

Cloud Explorer is geïnstalleerd als onderdeel van de Visual Studio Tools voor Azure SDK 2.7. 

## <a name="prerequisites"></a>Vereisten

- Visual Studio 2015 RTM.

- De Visual Studio Tools for Azure SDK. 
- U moet ook een Azure-account en worden vastgelegd in het Azure bronnen weergeven in de Cloud Explorer. Als u er geen hebt, maakt u een account in een paar minuten. Als u een MSDN-abonnement hebt, Zie [Azure voordeel voor MSDN-abonnees](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/). Raadpleeg anders [een gratis proefperiode account maken](https://azure.microsoft.com/pricing/free-trial/).

- Als Cloud Explorer niet zichtbaar is, kunt u deze weergeven door **Beeld**, **Overige vensters** **Cloud Explorer** in het menu te kiezen.

## <a name="manage-azure-accounts-and-subscriptions"></a>Azure accounts en abonnementen beheren

Overzicht van resources in de Cloud Explorer Azure, moet u zich aanmelden bij een Azure account met een of meer actieve abonnementen. Als u meer dan één Azure-account hebt, kunt u deze toevoegen in de Cloud Explorer en kies vervolgens de abonnementen die u wilt opnemen in de Cloud Explorer resourceweergave.

Als u Azure voordat niet hebt gebruikt of als u de benodigde rekeningen hebt toegevoegd aan Visual Studio, wordt u gevraagd om dit te doen.

## <a name="to-add-azure-accounts-to-cloud-explorer"></a>Azure Cloud Explorer accounts toevoegen

1. Kies het pictogram op de werkbalk van Cloud Explorer.

1. Kies de link **een account toevoegen** . Log in op de Azure account waarvan bronnen die u wilt zoeken. De account die u zojuist hebt toegevoegd, moet worden geselecteerd in de vervolgkeuzelijst account kiezen. De abonnementen voor de account wordt weergegeven onder de post.

    ![Azure abonnementen toevoegen](./media/vs-azure-tools-resources-managing-with-cloud-explorer/IC819514.png)

    ![Azure abonnementen kiezen](./media/vs-azure-tools-resources-managing-with-cloud-explorer/IC819515.png)

1. Schakel de selectievakjes in voor de account abonnementen die u wilt bladeren en kies vervolgens de knop **toepassen** .

    De Azure bronnen voor de geselecteerde abonnementen weergegeven in de Cloud Explorer.

## <a name="to-remove-an-azure-account"></a>Een Azure-account verwijderen

1. Kies **bestand**, **Accountinstellingen** in het menu.

1. Kies de opdracht **verwijderen** naast de account waarvoor dat u wilt verwijderen in de sectie **Alle Accounts** in het dialoogvenster **Accountinstellingen** . Opmerking dat met deze opdracht verwijdert u alleen de account vanuit Visual Studio – it heeft geen invloed op de Azure account zelf.

## <a name="view-resource-types-or-groups"></a>Brontypen weergeven of groepen

Als u wilt uw Azure resources weergeven, kunt u **Typen resources** of **Resourcegroepen** weergeven.

![Vervolgkeuzelijsten resource](./media/vs-azure-tools-resources-managing-with-cloud-explorer/IC819516.png)

- **Brontypen** ziet, die ook de algemene weergave op de [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040)gebruikt, uw Azure resources ingedeeld op type, zoals web apps, rekeningen voor opslag en virtuele machines. Dit is gelijk aan hoe Azure resources worden weergegeven in Server Explorer.

- Groepen resourceweergave Azure resources op de bijbehorende Azure groep ingedeeld.

 
    Een resourcegroep is een bundel van Azure bronnen, meestal gebruikt door een bepaalde toepassing. Voor meer informatie over resourcegroepen Azure, overzicht [Azure Resource Manager](./resource-group-overview.md).

## <a name="view-and-navigate-resources"></a>Weergeven en navigeren resources

Om te navigeren naar een Azure-bron en de informatie in de Cloud Explorer bekijken, vouwt u het type van het artikel of de bijbehorende resourcegroep en kies vervolgens de bron. Als u een bron kiest, wordt informatie weergegeven in de twee tabbladen onderaan in de Cloud Explorer.

![Kies een resourceweergave](./media/vs-azure-tools-resources-managing-with-cloud-explorer/IC819517.png)

- Het tabblad **Acties** ziet u welke acties die u kunt uitvoeren voor de geselecteerde resource in Cloud Explorer. Beschikbare acties kunt u ook bekijken in het snelmenu van de resource.

- Het tabblad **Eigenschappen** worden de eigenschappen van de bron, zoals het type, landinstellingen en resource-groep dat is gekoppeld.

Elke resource heeft de actie **openen in de portal**. Als u deze actie, waarin Cloud Explorer de geselecteerde resource de [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040). Deze functie is vooral handig voor het navigeren naar de bronnen diep genest.

Aanvullende acties en waarden van de eigenschap kunnen ook worden weergegeven op basis van de resource Azure. Web apps en apps logica ook bijvoorbeeld de acties **openen in browser** en een **debugger koppelen** **geopend in de portal**. Acties voor het openen van editors worden weergegeven wanneer u een account-blob storage, wachtrij of tabel. Azure apps hebben de eigenschappen **URL** en **Status** , opslagbronnen tekenreekseigenschappen sleutel en verbinding.

## <a name="search-resources"></a>Resources zoeken

Voer de naam in het vak Zoeken in de Cloud Explorer voor resources met een specifieke naam in uw account Azure abonnementen.

![Resources zoeken in de Cloud Explorer](./media/vs-azure-tools-resources-managing-with-cloud-explorer/IC820394.png)

U tekens invoert in het vak zoeken, alleen de resources die overeenkomen met de tekens die worden weergegeven in de resourcestructuur.

