<properties
    pageTitle="Verticaal schalen Azure virtuele machine met Azure automatisering | Microsoft Azure"
    description="Hoe een Linux virtuele Machine verticaal schalen in reactie op de monitoring van waarschuwingen met Azure automatisering"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="singhkays"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="03/29/2016"
    ms.author="singhkay"/>

# <a name="vertically-scale-azure-virtual-machine-with-azure-automation"></a>Verticaal schalen Azure virtuele machine met Azure automatisering

Verticaal schalen is het proces van het verhogen of verlagen van de bronnen van een machine in reactie op de werkbelasting. In Azure dit kunt u doen door de grootte van de virtuele Machine. Dit kan helpen bij de volgende scenario 's

- Als de virtuele Machine niet vaak gebruikt wordt, kunt u het formaat op een kleiner formaat uw maandelijkse kosten verlagen
- Als de virtuele Machine een piekbelasting ziet is, het formaat kan worden gewijzigd in een groter formaat te verhogen van de capaciteit

De omtrek van de stappen om dit te bereiken is als hieronder

1. Automatisering van Azure Setup toegang te krijgen tot uw virtuele Machines
2. De verticale schaal van Azure automatisering runbooks importeren in uw abonnement
3. Een webhook toevoegen aan uw runbook
4. Een waarschuwing toevoegen aan uw virtuele Machine

> [AZURE.NOTE] Vanwege de omvang van de eerste virtuele Machine, de grootte van deze kan worden geschaald, kan worden beperkt door de beschikbaarheid van de andere formaten in het cluster die in huidige virtuele Machine wordt geïmplementeerd. In de gepubliceerde automatisering runbooks die in dit artikel wordt gebruikt voor dit geval zorgen en we alleen schaal binnen de onderstaande VM grootte paren. Dit betekent dat een Standard_D1v2 virtuele Machine niet plotseling worden aangepast aan Standard_G5 of verkleind tot Basic_A0.

>| VM-formaten paar schalen |   |
|---|---|
|  Basic_A0 |  Basic_A4 |
|  Standard_A0 | Standard_A4 |
|  Standard_A5 | Standard_A7  |
|  Standard_A8 | Standard_A9  |
|  Standard_A10 |  Standard_A11 |
|  Standard_D1 |  Standard_D4 |
|  Standard_D11 | Standard_D14  |
|  Standard_DS1 |  Standard_DS4 |
|  Standard_DS11 | Standard_DS14  |
|  Standard_D1v2 |  Standard_D5v2 |
|  Standard_D11v2 |  Standard_D14v2 |
|  Standard_G1 |  Standard_G5 |
|  Standard_GS1 |  Standard_GS5 |

## <a name="setup-azure-automation-to-access-your-virtual-machines"></a>Automatisering van Azure Setup toegang te krijgen tot uw virtuele Machines

Het eerste wat dat u moet doen is een Azure automatisering account maken die is van de runbooks gebruikt host voor het schalen van de exemplaren VM schaal ingesteld. De service automatisering introduceerde onlangs de 'Uitvoeren als'-functie waardoor de instelling van de Service Principal voor het automatisch uitvoeren van de runbooks voor rekening van de gebruiker heel eenvoudig. U kunt meer lezen over dit in het artikel hieronder:

* [Runbooks met Azure uitvoeren als-account verifiëren](../automation/automation-sec-configure-azure-runas-account.md)

## <a name="import-the-azure-automation-vertical-scale-runbooks-into-your-subscription"></a>De verticale schaal van Azure automatisering runbooks importeren in uw abonnement

De runbooks die nodig zijn voor het verticaal schalen van uw virtuele Machine zijn al gepubliceerd in de galerie met Azure automatisering Runbook. U wilt importeren in uw abonnement. Voor meer informatie over het importeren van runbooks door het volgende artikel lezen.

* [Runbook en module galerijen voor Azure automatisering](../automation/automation-runbook-gallery.md)

De runbooks die moeten worden geïmporteerd, worden weergegeven in de onderstaande afbeelding

![Runbooks importeren](./media/virtual-machines-vertical-scaling-automation/scale-runbooks.png)

## <a name="add-a-webhook-to-your-runbook"></a>Een webhook toevoegen aan uw runbook

Zodra u hebt geïmporteerd toevoegen de runbooks u moet een webhook aan de runbook, zodat deze kan worden geactiveerd door een waarschuwing van een virtuele Machine. De details van het maken van een webhook voor uw Runbook kunnen hier worden gelezen.

* [Azure automatisering webhooks](../automation/automation-webhooks.md)

Zorg ervoor dat u de webhook kopiëren voordat u het dialoogvenster webhook te sluiten, want u dit in de volgende sectie moet.

## <a name="add-an-alert-to-your-virtual-machine"></a>Een waarschuwing toevoegen aan uw virtuele Machine

1. Selecteer instellingen voor virtuele Machine
2. Selecteer "Waarschuwingsregels"
3. Selecteer "Waarschuwing toevoegen"
4. Selecteer een metric bij brand van de waarschuwing op
5. Selecteer een voorwaarde, welke wanneer wordt voldaan aan de waarschuwing bij brand veroorzaken
6. Selecteer een drempel voor de voorwaarde in stap 5. moet worden voldaan
7. Selecteer een periode waarin de monitoring-service wordt gecontroleerd voor de voorwaarde en de drempel in stap 5 en 6
8. Plak in het webhook dat u hebt gekopieerd uit de vorige sectie.

![Een waarschuwing toevoegen aan de virtuele Machine 1](./media/virtual-machines-vertical-scaling-automation/add-alert-webhook-1.png)

![Een waarschuwing toevoegen aan de virtuele Machine 2](./media/virtual-machines-vertical-scaling-automation/add-alert-webhook-2.png)