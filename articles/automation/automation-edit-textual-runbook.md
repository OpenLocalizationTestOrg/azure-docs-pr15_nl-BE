<properties 
    pageTitle="Tekstuele runbooks in Azure automatisering bewerken"
    description="Dit artikel bevat verschillende procedures voor het werken met PowerShell en PowerShell Workflow runbooks in Azure automatisering met behulp van de tekst-editor."
    services="automation"
    documentationCenter=""
    authors="mgoedtel"
    manager="stevenka"
    editor="tysonn" />
<tags 
    ms.service="automation"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="02/23/2016"
    ms.author="magoedte;bwren" />

# <a name="editing-textual-runbooks-in-azure-automation"></a>Tekstuele runbooks in Azure automatisering bewerken

De tekstuele editor in Azure automatisering kan worden gebruikt om [PowerShell runbooks](automation-runbook-types.md#powershell-runbooks) en [runbooks PowerShell Workflow](automation-runbook-types.md#powershell-workflow-runbooks)te bewerken. Dit heeft de typische kenmerken van een andere code-editors zoals intellisense en kleurcodering met extra speciale functies om u te helpen bij de toegang tot bronnen voor runbooks.  Dit artikel bevat gedetailleerde stappen voor het uitvoeren van verschillende functies met deze editor.

De tekstuele editor bevat een functie voor het invoegen van code voor de activiteiten, de activa en de onderliggende runbooks in een runbook. In plaats van te typen in de code zelf, kunt u selecteren uit een lijst van de beschikbare middelen en de juiste code ingevoegd in de runbook.

Elke runbook in Azure automatisering heeft twee versies, concept en gepubliceerd. U bewerkt de conceptversie van de runbook en publiceren zodat het kan worden uitgevoerd. De gepubliceerde versie kan niet worden bewerkt. Zie de [publicatie van een runbook](automation-creating-importing-runbook.md#publishing-a-runbook) voor meer informatie.

Als u wilt werken met [Grafische Runbooks](automation-runbook-types.md#graphical-runbooks), Zie [grafische ontwerpen in Azure automatisering](automation-graphical-authoring-intro.md).

## <a name="to-edit-a-runbook-with-the-azure-portal"></a>Voor het bewerken van een runbook met Azure portal

Gebruik de volgende procedure voor het openen van een runbook bewerken in de tekst editor.

1. Selecteer uw account automatisering in de Azure portal.
2. Klik op de tegel **Runbooks** om de lijst van runbooks.
3. Klik op de naam van de runbook die u wilt bewerken en klik vervolgens op de knop **bewerken** .
6. De vereiste bewerking uitvoeren
7. Klik op **Opslaan** wanneer u uw bewerkingen zijn voltooid.
8. Klik op **publiceren** als u wilt dat de nieuwste conceptversie van de runbook worden gepubliceerd.

### <a name="to-insert-a-cmdlet-into-a-runbook"></a>Een cmdlet invoegen in een runbook

2. Op het papier van de tekst-editor, plaatst u de cursor waar u de cmdlet plaatsen.
3. Vouw het knooppunt **Cmdlets** in het besturingselement voor de bibliotheek. 
3. Vouw in de module met de cmdlet die u wilt gebruiken.
4. Klik de cmdlet invoegen en selecteert u **toevoegen aan het tekenpapier**.  Als de cmdlet meer dan één parameter is ingesteld heeft, wordt de standaardset worden toegevoegd.  U kunt ook de cmdlet, selecteer een andere parameter ingesteld uitbreiden.
4. De code voor de cmdlet wordt ingevoegd met de volledige lijst met parameters.
5. Een geschikte waarde in plaats van het gegevenstype dat wordt omsloten door accolades <> voor alle vereiste parameters bevatten.  De parameters die u niet nodig hebt verwijderen.

### <a name="to-insert-code-for-a-child-runbook-into-a-runbook"></a>Code voor de runbook van een kind in een runbook invoegen

2. Op het papier van de tekst-editor, plaats de cursor waar u de code voor de [onderliggende runbook](automation-child-runbooks.md).
3. Vouw het knooppunt **Runbooks** in het besturingselement voor de bibliotheek. 
3. De runbook als u wilt invoegen en selecteer **aan het canvas toevoegen**met de rechtermuisknop op.
4. De code voor de onderliggende runbook wordt ingevoegd met de tijdelijke aanduidingen voor eventuele parameters runbook.
5. Vervang de tijdelijke aanduidingen met de juiste waarden voor elke parameter.

### <a name="to-insert-an-asset-into-a-runbook"></a>Een actief in een runbook invoegen

2. Op het papier van de tekst-editor, plaats de cursor waar u de code voor de onderliggende runbook.
3. Vouw het knooppunt van de **activa** in het besturingselement voor de bibliotheek. 
4. Vouw het knooppunt voor het type van activum.
3. Klik het activum invoegen en selecteert u **toevoegen aan het tekenpapier**.  Selecteer voor [variabele elementen](automation-variables.md), **toevoegen "variabele ophalen ' canvas** of **" variabele instellen ' aan het canvas toevoegen** of u wilt ophalen of instellen van de variabele.
4. De code voor het activum wordt ingevoegd in de runbook.



## <a name="to-edit-a-runbook-with-the-azure-portal"></a>Voor het bewerken van een runbook met Azure portal

Gebruik de volgende procedure voor het openen van een runbook bewerken in de tekst editor.

1. In het portal voor Azure **automatisering** selecteert en klikt u vervolgens de naam van een account met automatisering.
2. Selecteer het tabblad **Runbooks** .
3. Klik op de naam van de runbook die u wilt bewerken en selecteer vervolgens het tabblad **auteur** .
5. Klik op de knop **bewerken** onder aan het scherm.
6. De vereiste bewerking uitvoeren
7. Klik op **Opslaan** wanneer u uw bewerkingen zijn voltooid.
8. Klik op **publiceren** als u wilt dat de nieuwste conceptversie van de runbook worden gepubliceerd.

### <a name="to-insert-an-activity-into-a-runbook"></a>Een activiteit in een Runbook invoegen

1. Op het papier van de tekst-editor, plaats de cursor waar u de activiteit plaats.
1. Onderaan op het scherm, klikt u op **Invoegen** en **activiteit**.
1. Selecteer de module met de activiteit in de kolom **Integratiemodule** .
1. Selecteer in het venster **activiteit** een activiteit.
1. Let op de beschrijving van de activiteit in de kolom **Beschrijving** . Desgewenst kunt u weergave gedetailleerde help help voor de activiteit in de browser te starten.
1. Klik op de pijl-rechts.  Als de activiteit parameters heeft, zal zij ter informatie weergegeven.
1. Klik op de knop controleren.  Code voor het uitvoeren van de activiteit worden ingevoegd in de runbook.
1. Als de activiteit parameters vereist, bieden een juiste waarde in plaats van het gegevenstype dat wordt omsloten door accolades <>.

### <a name="to-insert-code-for-a-child-runbook-into-a-runbook"></a>Code voor de runbook van een kind in een runbook invoegen

1. Op het papier van de tekst-editor, plaats de cursor waar u de [onderliggende runbook](automation-child-runbooks.md)plaatsen.
2. Onderaan op het scherm, klikt u op **Invoegen** en klik vervolgens op **Runbook**.
3. Selecteer de runbook van de middelste kolom invoegen en klik op de pijl-rechts.
4. Als de runbook parameters heeft, zal zij ter informatie weergegeven.
5. Klik op de knop controleren.  Code voor het uitvoeren van de geselecteerde runbook worden ingevoegd in de huidige runbook.
7. Als de runbook parameters vereist, bieden een juiste waarde in plaats van het gegevenstype dat wordt omsloten door accolades <>.

### <a name="to-insert-an-asset-into-a-runbook"></a>Een actief in een runbook invoegen

1. Op het papier van de tekst-editor, plaatst u de cursor waar u de activiteit voor het ophalen van het activum te plaatsen.
1. Onderaan op het scherm, klikt u op **Invoegen** en klik vervolgens op **instellen**.
1. Selecteer de gewenste actie in de kolom **Actie van de instelling** .
1. Selecteer uit de beschikbare elementen in de middelste kolom.
1. Klik op de knop controleren.  Code ophalen of instellen van de activa worden ingevoegd in de runbook.



## <a name="to-edit-an-azure-automation-runbook-using-windows-powershell"></a>Voor het bewerken van een Azure automatisering runbook met Windows PowerShell

Als u wilt een runbook met Windows PowerShell bewerken, gebruikt u de editor van uw keuze en opslaan als een bestand .ps1. U kunt de cmdlet [Get-AzureAutomationRunbookDefinition](http://aka.ms/runbookauthor/cmdlet/getazurerunbookdefinition) voor het ophalen van de inhoud van de runbook en het bestaande concept runbook vervangen door de gewijzigde cmdlet [Set-AzureAutomationRunbookDefinition](http://aka.ms/runbookauthor/cmdlet/setazurerunbookdefinition) .

### <a name="to-retrieve-the-contents-of-a-runbook-using-windows-powershell"></a>Voor het ophalen van de inhoud van een Runbook met Windows PowerShell

De volgende voorbeeldopdrachten hoe het script voor een runbook ophalen en opslaan naar een scriptbestand. In dit voorbeeld de conceptversie opgehaald. Het is ook mogelijk om op te halen van de gepubliceerde versie van de runbook maar deze versie kan niet worden gewijzigd.

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Sample-TestRunbook"
    $scriptPath = "c:\runbooks\Sample-TestRunbook.ps1"
    
    $runbookDefinition = Get-AzureAutomationRunbookDefinition -AutomationAccountName $automationAccountName -Name $runbookName -Slot Draft
    $runbookContent = $runbookDefinition.Content

    Out-File -InputObject $runbookContent -FilePath $scriptPath

### <a name="to-change-the-contents-of-a-runbook-using-windows-powershell"></a>Wijzigen van de inhoud van een Runbook met Windows PowerShell

De volgende voorbeeldopdrachten hoe de bestaande inhoud van een runbook vervangen door de inhoud van een scriptbestand. Opmerking Dit is dezelfde voorbeeldprocedure als [voor het importeren van een runbook van een scriptbestand met Windows PowerShell](../automation-creating-or-importing-a-runbook#ImportRunbookScriptPS).

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Sample-TestRunbook"
    $scriptPath = "c:\runbooks\Sample-TestRunbook.ps1"

    Set-AzureAutomationRunbookDefinition -AutomationAccountName $automationAccountName -Name $runbookName -Path $scriptPath -Overwrite
    Publish-AzureAutomationRunbook –AutomationAccountName $automationAccountName –Name $runbookName

## <a name="related-articles"></a>Verwante artikelen

- [Maken of importeren van een runbook in Azure automatisering](automation-creating-importing-runbook.md)
- [Leren werken met PowerShell workflow](automation-powershell-workflow.md)
- [Grafische ontwerpen in Azure automatisering](automation-graphical-authoring-intro.md)
- [Certificaten](automation-certificates.md)
- [Verbindingen](automation-connections.md)
- [Referenties](automation-credentials.md)
- [Schema 's](automation-schedules.md)
- [Variabelen](automation-variables.md)