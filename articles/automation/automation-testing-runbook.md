<properties 
    pageTitle="Testen van een runbook in Azure automatisering | Microsoft Azure"
    description="Voordat u een runbook in Azure automatisering publiceert, kunt u testen om te zorgen dat correct werkt.  In dit artikel wordt beschreven hoe een runbook testen en weergeven van de uitvoer."
    services="automation"
    documentationCenter=""
    authors="mgoedtel"
    manager="jwhit"
    editor="tysonn" />
<tags 
    ms.service="automation"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="09/12/2016"
    ms.author="magoedte;bwren" />

# <a name="testing-a-runbook-in-azure-automation"></a>Testen van een runbook in Azure automatisering
Wanneer u een runbook test, de [conceptversie](automation-creating-importing-runbook.md#publishing-a-runbook) wordt uitgevoerd en de acties die worden uitgevoerd, zijn voltooid. Geen geschiedenis wordt gemaakt, maar de streams van [Output](automation-runbook-output-and-messages.md#output-stream) en [waarschuwingen en fouten](automation-runbook-output-and-messages.md#message-streams) worden weergegeven in de Test deelvenster Uitvoer. [Uitgebreide stroom](automation-runbook-output-and-messages.md#message-streams) berichten worden weergegeven in het deelvenster Uitvoer alleen als de [variabele $VerbosePreference](automation-runbook-output-and-messages.md#preference-variables) ingesteld op Doorgaan.

Hoewel de ontwerp-versie wordt uitgevoerd, wordt de runbook nog steeds voert de werkstroom normaal en voert alle acties tegen bronnen in de omgeving. Om deze reden moet u alleen runbooks in niet-productieve bronnen te testen.

De procedure voor het testen van elke [soort runbook](automation-runbook-types.md) is hetzelfde en er is geen verschil tussen de tekst editor en de grafische editor in de portal Azure testen.  


## <a name="to-test-a-runbook-in-the-azure-portal"></a>Een runbook in de portal Azure testen

U kunt werken met elk [type runbook](automation-runbook-types.md) in het portal voor Azure.

1. Open de conceptversie van de runbook in de [tekst-editor](automation-editing-a-runbook.md#Portal) of een [grafische editor](automation-graphical-authoring-intro.md).
2. Klik op de knop **testen** om de Test blade.
3. Als de runbook parameters heeft, zal deze worden weergegeven in het linkerdeelvenster, waar u kunt bevatten waarden die worden gebruikt voor de test.
4. Als u wilt dat de test wilt uitvoeren op een [Hybride Runbook werknemer](automation-hybrid-runbook-worker.md), **Hybride werknemer** **Uitgevoerd instellingen** wijzigen en selecteert u de naam van de doelgroep.  Anders, houd de standaard **Azure** de test wilt uitvoeren in de cloud.
5. Klik op de knop **Start** om de test te starten.
6. Als de runbook [PowerShell Workflow](automation-runbook-types.md#powershell-workflow-runbooks) of [grafische](automation-runbook-types.md#graphical-runbooks), kunt u stoppen of schorst terwijl wordt getest met de knoppen onder in het deelvenster Uitvoer. Wanneer u de runbook, wordt de huidige activiteit voltooid voordat deze wordt onderbroken. Zodra de runbook is onderbroken, te stoppen of opnieuw starten.
7. Bekijk de uitvoer van de runbook in het deelvenster Uitvoer.


## <a name="next-steps"></a>Volgende stappen

- Zie informatie over het maken of importeren van een runbook, [maken of importeren van een runbook in Azure automatisering](automation-creating-importing-runbook.md)
- Zie voor meer informatie over grafische ontwerpen, [grafische ontwerpen in Azure automatisering](automation-graphical-authoring-intro.md)
- Om te beginnen met PowerShell workflow runbooks, Zie [Mijn eerste PowerShell workflow runbook](automation-first-runbook-textual.md)
- Zie voor meer informatie over het configureren van runboks om terug te keren statusberichten en fouten, met inbegrip van aanbevolen procedures, [Runbook uitvoer en berichten in Azure automatisering](automation-runbook-output-and-messages.md)