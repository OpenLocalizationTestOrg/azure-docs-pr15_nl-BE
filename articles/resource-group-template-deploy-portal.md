<properties 
    pageTitle="Azure portal gebruiken voor de implementatie van Azure resources | Microsoft Azure" 
    description="Azure portal en Azure Resource beheren gebruiken voor de implementatie van uw resources." 
    services="azure-resource-manager,azure-portal" 
    documentationCenter="" 
    authors="tfitzmac" 
    manager="timlt" 
    editor="tysonn"/>

<tags 
    ms.service="azure-resource-manager" 
    ms.workload="multiple" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/15/2016" 
    ms.author="tomfitz"/>

# <a name="deploy-resources-with-resource-manager-templates-and-azure-portal"></a>Resources met bronbeheer sjablonen en Azure portal implementeren

> [AZURE.SELECTOR]
- [PowerShell](resource-group-template-deploy.md)
- [Azure CLI](resource-group-template-deploy-cli.md)
- [Portal](resource-group-template-deploy-portal.md)
- [REST-API](resource-group-template-deploy-rest.md)

In dit onderwerp wordt beschreven hoe de [Azure portal](https://portal.azure.com) met [Azure Resource Manager](azure-resource-manager/resource-group-overview.md) gebruiken voor de implementatie van uw resources Azure. Zie meer informatie over het beheer van bronnen, [resources Azure beheren via de portal](./azure-portal/resource-group-portal.md).

Op dit moment ondersteunt niet elke service portal of resourcemanager. Voor deze services moet u de [klassieke portal](https://manage.windowsazure.com)te gebruiken. Zie [grafiek voor beschikbaarheid van Azure portal](https://azure.microsoft.com/features/azure-portal/availability/)voor de status van elke service.

## <a name="create-resource-group"></a>Resourcegroep maken

1. Een lege als resourcegroep wilt maken, selecteert u **Nieuw** > **Management** > **Resourcegroep**.

    ![lege bronnengroep maken](./media/resource-group-template-deploy-portal/create-empty-group.png)

2. Geef deze een naam en locatie en selecteer indien nodig een abonnement. U moet een locatie voor de resourcegroep opgeven omdat de resourcegroep worden metagegevens over de resources opgeslagen. Omwille van de compatibiliteit kunt u opgeven waar deze metagegevens worden opgeslagen. In het algemeen wordt aangeraden dat u een locatie voor de meeste van uw resources op te geven. Met behulp van dezelfde locatie, kan uw sjabloon vereenvoudigen.

    ![setwaarden](./media/resource-group-template-deploy-portal/set-group-properties.png)

## <a name="deploy-resources-from-marketplace"></a>Bronnen van Marketplace implementeren

Nadat u een resourcegroep hebt gemaakt, kunt u kunt resources uit de markt implementeren. De markt biedt vooraf gedefinieerde oplossingen voor veelvoorkomende scenario's.

1. Om te beginnen een distributie, selecteer **Nieuw** en het type bron dat u wilt implementeren. Vervolgens zoekt u de specifieke versie van de resource die u wilt implementeren.

    ![resource implementeren](./media/resource-group-template-deploy-portal/deploy-resource.png)

2. Als er niet de specifieke oplossing wilt implementeren, kunt u de markt voor het zoeken.

    ![marketplace zoeken](./media/resource-group-template-deploy-portal/search-resource.png)

3. U hebt een verzameling relevante eigenschappen in te stellen voor implementatie, afhankelijk van het type van de geselecteerde resource. Deze opties worden hier niet weergegeven zoals ze zijn afhankelijk van het resourcetype. Voor alle typen, moet u een resourcegroep bestemming. De volgende afbeelding ziet u hoe een web app maken en deze implementeren aan de resourcegroep die u hebt gemaakt.

    ![resourcegroep maken](./media/resource-group-template-deploy-portal/select-existing-group.png)

    U kunt ook een resourcegroep maken bij het implementeren van uw resources. Selecteer **nieuwe maken** en een naam geven aan de resourcegroep.

    ![nieuwe resourcegroep maken](./media/resource-group-template-deploy-portal/select-new-group.png)

4. De installatie begint. De installatie kan enkele minuten duren. Wanneer de installatie is voltooid, ziet u een melding.

    ![melding weergeven](./media/resource-group-template-deploy-portal/view-notification.png)

5. Na de implementatie van uw resources, kunt u meer resources toevoegen aan de resourcegroep met behulp van de opdracht **toevoegen** op de resource groep blade.

    ![resource toevoegen](./media/resource-group-template-deploy-portal/add-resource.png)

## <a name="deploy-resources-from-custom-template"></a>Resources van de aangepaste sjabloon distribueren

Als u wilt uitvoeren van een distributie, maar geen gebruik van de sjablonen in de markt, kunt u een aangepaste sjabloon waarin de infrastructuur voor uw oplossing. Zie meer informatie over het maken van sjablonen, [sjablonen ontwerpen Azure Resource Manager](resource-group-authoring-templates.md).

1. Selecteer **Nieuw**en beginnen met zoeken voor de **Implementatie van de sjabloon** , totdat u deze uit de opties kiezen kunt voor de implementatie van een aangepaste sjabloon via de portal.

    ![sjabloon voor implementatie van zoeken](./media/resource-group-template-deploy-portal/search-template.png)

2. Selecteer **Sjabloon voor implementatie** van de beschikbare middelen.

    ![Selecteer de voor sjabloonimplementatie](./media/resource-group-template-deploy-portal/select-template.png)

3. Open de lege sjabloon die beschikbaar is voor het aanpassen van na het starten van de distributie van de sjabloon.

    ![sjabloon maken](./media/resource-group-template-deploy-portal/show-custom-template.png)

    In de editor, voeg de syntaxis JSON waarmee de resources die u wilt implementeren. Selecteer **Opslaan** wanneer u klaar bent. Zie voor instructies over het schrijven de syntaxis JSON [Resource Manager sjabloon scenario](resource-manager-template-walkthrough.md).

    ![template bewerken](./media/resource-group-template-deploy-portal/edit-template.png)

4. Of u kunt een bestaande sjabloon selecteren in de [Azure quickstart sjablonen](https://azure.microsoft.com/documentation/templates/). Deze sjablonen worden aangeleverd door de Gemeenschap. Deze veel voorkomende scenario's omvatten en iemand een sjabloon die vergelijkbaar is met wat u wilt implementeren hebt toegevoegd. U kunt de sjablonen zoeken die overeenkomt met uw scenario zoeken.

    ![Selecteer de sjabloon quickstart](./media/resource-group-template-deploy-portal/select-quickstart-template.png)

    In de editor kunt u de geselecteerde sjabloon weergeven.

5. Na het opgeven van de waarden **maken** voor de implementatie van de sjabloon te selecteren. 

    ![sjabloon distribueren](./media/resource-group-template-deploy-portal/create-custom-deploy.png)

## <a name="deploy-resources-from-a-template-saved-to-your-account"></a>Implementeren van de bronnen van een sjabloon die is opgeslagen in uw account

De portal kunt u een sjabloon aan uw account Azure opslaan en later implementeren. Voor meer informatie over het werken met deze sjablonen [aan de slag met persoonlijke sjablonen op de Azure portal](./marketplace-consumer/mytemplates-getstarted.md)opgeslagen.

1. Als u uw opgeslagen sjablonen zoekt, selecteert u **Bladeren** > **sjablonen**.

    ![Bladeren naar sjablonen](./media/resource-group-template-deploy-portal/browse-templates.png)

2. Selecteer uit de lijst met sjablonen die zijn opgeslagen in uw account die u werken wilt op.

    ![opgeslagen sjablonen](./media/resource-group-template-deploy-portal/saved-templates.png)

3. Selecteer **distribueren** te implementeren dit opgeslagen sjabloon.

    ![implementeren van de opgeslagen sjabloon](./media/resource-group-template-deploy-portal/deploy-saved-template.png)

## <a name="next-steps"></a>Volgende stappen

- Controlelogboeken, Zie [bewerkingen met Resource Manager controleren](resource-group-audit.md).
- Zie [Troubleshooting resource groep implementaties met Azure portal](resource-manager-troubleshoot-deployments-portal.md)voor het oplossen van implementatiefouten.
- Zie een sjabloon worden opgehaald uit een resourcegroep of een implementatie, [sjabloon exporteren Azure Resource Manager aan de bestaande bronnen](resource-manager-export-template.md).





