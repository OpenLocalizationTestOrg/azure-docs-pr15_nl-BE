<properties
   pageTitle="Implementatie van bewerkingen met de portal weergeven | Microsoft Azure"
   description="Hierin wordt beschreven hoe de Azure portal gebruiken voor het detecteren van fouten van de Resource Manager-implementatie."
   services="azure-resource-manager,virtual-machines"
   documentationCenter=""
   tags="top-support-issue"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-multiple"
   ms.workload="infrastructure"
   ms.date="06/15/2016"
   ms.author="tomfitz"/>

# <a name="view-deployment-operations-with-azure-portal"></a>Implementatie-bewerkingen met Azure Portal weergeven

> [AZURE.SELECTOR]
- [Portal](resource-manager-troubleshoot-deployments-portal.md)
- [PowerShell](resource-manager-troubleshoot-deployments-powershell.md)
- [Azure CLI](resource-manager-troubleshoot-deployments-cli.md)
- [REST-API](resource-manager-troubleshoot-deployments-rest.md)

U kunt de bewerkingen van een distributie via de portal Azure weergeven. Kan zijn dat u graag de bewerkingen weergeven wanneer heb je een fout tijdens de implementatie zodat dit artikel richt zich op het weergeven van bewerkingen die zijn mislukt. De portal biedt een interface waarmee u eenvoudig zoeken naar fouten en mogelijke oplossingen bepalen.

[AZURE.INCLUDE [resource-manager-troubleshoot-introduction](../includes/resource-manager-troubleshoot-introduction.md)]

## <a name="use-deployment-operations-to-troubleshoot"></a>Implementatie-bewerkingen gebruiken voor het oplossen van problemen met

Overzicht van de bewerkingen implementatie, gebruik de volgende stappen uit:

1. U ziet de status van de laatste implementatie voor de resourcegroep die betrokken zijn bij de implementatie. U kunt deze status voor meer informatie.

    ![Implementatiestatus](./media/resource-manager-troubleshoot-deployments-portal/deployment-status.png)

2. U ziet de recente geschiedenis van de implementatie. Selecteer de installatie is mislukt.

    ![Implementatiestatus](./media/resource-manager-troubleshoot-deployments-portal/select-deployment.png)

3. Selecteer **is mislukt. Klik hier voor meer informatie** voor een beschrijving van de reden waarom de installatie is mislukt. De DNS-record is niet uniek in de onderstaande afbeelding.  

    ![mislukte implementatie weergeven](./media/resource-manager-troubleshoot-deployments-portal/view-error.png)

    Dit foutbericht moet u beginnen met het oplossen van problemen. Als u meer informatie nodig hebt over welke taken zijn voltooid, kunt u de bewerkingen bekijken, zoals wordt weergegeven in de volgende stappen uit.

4. U kunt alle bewerkingen implementatie weergeven in de blade **implementatie** . Selecteer een willekeurige bewerking voor meer details.

    ![bewerkingen weergeven](./media/resource-manager-troubleshoot-deployments-portal/view-operations.png)

    In dit geval ziet u dat de rekening voor opslag, virtueel netwerk en beschikbaarheid set zijn gemaakt. Het openbare IP-adres is mislukt en andere bronnen die niet zijn geprobeerd.

5. U kunt gebeurtenissen voor de implementatie weergeven door het selecteren van **gebeurtenissen**.

    ![gebeurtenissen weergeven](./media/resource-manager-troubleshoot-deployments-portal/view-events.png)

6. U ziet alle gebeurtenissen voor de implementatie en selecteer een voor meer details.

    ![Zie gebeurtenissen](./media/resource-manager-troubleshoot-deployments-portal/see-all-events.png)

## <a name="use-audit-logs-to-troubleshoot"></a>Gebruik van controlelogboeken oplossen

[AZURE.INCLUDE [resource-manager-audit-limitations](../includes/resource-manager-audit-limitations.md)]

Overzicht van fouten in een implementatie, gebruik de volgende stappen uit:

1. De controlelogboeken voor een groep weergeven door **Logboeken controleren**.

    ![Selecteer de controlelogboekbestanden](./media/resource-manager-troubleshoot-deployments-portal/select-audit-logs.png)

2. In de **Controlelogboeken** blade ziet u een overzicht van recente transacties voor alle resourcegroepen in uw abonnement. Het bevat een grafische weergave van de tijd en de status van de werkzaamheden, alsmede een lijst van de bewerkingen.

    ![acties weergeven](./media/resource-manager-troubleshoot-deployments-portal/audit-summary.png)

3. U kunt de weergave van de controlelogboeken, zich richten op bepaalde voorwaarden filteren. Selecteer **Filter** aan de bovenkant van de **controlelogboeken** blade.

    ![Logboeken filteren](./media/resource-manager-troubleshoot-deployments-portal/filter-logs.png)

4. Selecteer in de blade **Filter** voorwaarden voor uw weergave van de controlelogboeken beperken tot alleen de bewerkingen die u wilt zien. U kunt bijvoorbeeld filteren bewerkingen om fouten in de resourcegroep alleen weer te geven.

    ![Stel filteropties](./media/resource-manager-troubleshoot-deployments-portal/set-filter.png)

5. U kunt bewerkingen verder filteren door in te stellen een tijdsspanne. De volgende afbeelding filters de weergave voor een bepaalde tijdsduur van 20 minuten.

    ![tijd instellen](./media/resource-manager-troubleshoot-deployments-portal/select-time.png)

6. U kunt een van de bewerkingen in de lijst selecteren. Kies de bewerking met de fout die u wilt onderzoeken.

    ![Selecteer bewerking](./media/resource-manager-troubleshoot-deployments-portal/select-operation.png)
  
7. Ziet u alle gebeurtenissen voor die bewerking. U ziet de **Correlatie-id's** in de samenvatting. Deze ID wordt gebruikt voor het bijhouden van gebeurtenissen. Het kan handig zijn wanneer u werkt met de technische ondersteuning van een probleem kunt oplossen. U kunt een gebeurtenis voor informatie over de gebeurtenis te selecteren.

    ![gebeurtenis selecteren](./media/resource-manager-troubleshoot-deployments-portal/select-event.png)

8. Ziet u informatie over de gebeurtenis. In het bijzonder aandacht besteden aan de **Eigenschappen** voor meer informatie over de fout.

    ![audit logboekgegevens weergeven](./media/resource-manager-troubleshoot-deployments-portal/audit-details.png)

Het filter dat u hebt toegepast in het controlelogboek wordt de volgende keer dat u het bekijken, dus u deze waarden moet om het uitbreiden van uw weergave van de bewerkingen wijzigen ingehouden.

## <a name="next-steps"></a>Volgende stappen

- Zie [veelvoorkomende fouten bij het implementeren van Azure Azure Resource Manager bronnen oplossen](resource-manager-common-deployment-errors.md)voor hulp bij het oplossen van fouten in specifieke implementatie.
- Zie informatie over het gebruik van de controlelogboeken om te controleren van andere soorten acties, [bewerkingen met Resource Manager controleren](resource-group-audit.md).
- Zie [een bronnengroep met Azure Resource Manager sjabloon distribueren](resource-group-template-deploy.md)voor het valideren van uw implementatie voordat deze wordt uitgevoerd.
