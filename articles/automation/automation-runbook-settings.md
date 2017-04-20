<properties 
   pageTitle="Runbook instellingen"
   description="Beschrijving van de configuratie-instellingen voor een runbook in Azure automatisering en hoe u ze met Azure Management Portal en Windows PowerShell."
   services="automation"
   documentationCenter=""
   authors="bwren"
   manager="stevenka"
   editor="tysonn" />
<tags 
   ms.service="automation"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/09/2016"
   ms.author="bwren" />

# <a name="runbook-settings"></a>Runbook instellingen

Elke runbook in Azure automatisering heeft meerdere instellingen waarmee deze kan worden geïdentificeerd en de logboekregistratie wijzigen. Elk van deze instellingen wordt hieronder beschreven gevolgd door procedures voor het wijzigen van deze.

## <a name="settings"></a>Instellingen

### <a name="name-and-description"></a>Naam en beschrijving

U kunt de naam van een runbook niet wijzigen nadat deze is gemaakt. De beschrijving kan is optioneel en maximaal 512 tekens bevatten.

### <a name="tags"></a>Tags

Tags kunnen u afzonderlijke woorden en zinnen om vast te stellen een runbook toewijzen. Bijvoorbeeld wanneer u een runbook aan de [Galerie met Runbook](https://msdn.microsoft.com/library/dn781422.aspx)indient, u bepaalde tags om te bepalen welke categorieën de runbook moet worden weergegeven in. U kunt meerdere labels voor een runbook opgeven door deze te scheiden met komma's.

### <a name="logging"></a>Logboekregistratie

Standaard worden uitgebreid en de voortgang van de records niet naar geschiedenis geschreven. U kunt de instellingen voor een bepaalde runbook als u deze records wilt wijzigen. Zie voor meer informatie over deze records, [Runbook, uitvoer en berichten](https://msdn.microsoft.com/library/dn879148.aspx).

## <a name="changing-runbook-settings"></a>Runbook wijzigen

### <a name="changing-runbook-settings-with-the-azure-management-portal"></a>Wijzigen van instellingen met Azure Management Portal runbook

Kunt u instellingen voor een runbook in de Portal Azure beheer van de pagina **configureren** voor de runbook.

1. In de Portal Management Azure **automatisering** selecteert en klikt u vervolgens de naam van een account met automatisering.
1. Selecteer het tabblad **Runbooks** .
1. Klik op de naam van een runbook.
1. Selecteer in het tabblad **configureren** .

### <a name="changing-runbook-settings-with-windows-powershell"></a>Runbook wijzigen met Windows PowerShell

De cmdlet [Set-AzureAutomationRunbook](https://msdn.microsoft.com/library/dn690275.aspx) kunt u de instellingen wijzigen voor een runbook. Als u meerdere labels opgeven wilt, kunt u ofwel een matrix of een tekenreeks met door komma's gescheiden waarden voor de parameter codes bieden. U kunt de huidige codes met de [Get-AzureAutomationRunbook](https://msdn.microsoft.com/library/dn690278.aspx).

De volgende voorbeeldopdrachten hoe de eigenschappen instellen voor een runbook. In dit voorbeeld worden drie tags toegevoegd aan de bestaande codes en geeft aan dat uitgebreide records moeten worden vastgelegd.

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Sample-TestRunbook"
    $tags = (Get-AzureAutomationRunbook –AutomationAccountName $automationAccountName –Name $runbookName).Tags
    $tags += "Tag1,Tag2,Tag3"
    Set-AzureAutomationRunbook –AutomationAccountName $automationAccountName –Name $runbookName –LogVerbose $true –Tags $tags

## <a name="related-articles"></a>Verwante artikelen
- [Runbook Output en berichten](../automation-runbook-output-and-messages) 
- [Maken of importeren van een Runbook](https://msdn.microsoft.com/library/dn643637.aspx) 