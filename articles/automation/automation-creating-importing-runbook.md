<properties
    pageTitle="Maken of importeren van een runbook in Azure automatisering"
    description="In dit artikel wordt beschreven hoe een nieuwe runbook maken in Azure automatisering of een uit een bestand importeren."
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

# <a name="creating-or-importing-a-runbook-in-azure-automation"></a>Maken of importeren van een runbook in Azure automatisering

U kunt een runbook toevoegen aan Azure automatisering door ofwel [een nieuwe te maken](#creating-a-new-runbook) of een bestaande runbook te importeren uit een bestand of uit de [Galerie met Runbook](automation-runbook-gallery.md). Dit artikel bevat informatie over het maken en importeren van runbooks uit een bestand.  U kunt alle details over de toegang tot de communautaire runbooks en modules in de [galerieën met Runbook en de module voor het automatiseren van Azure](automation-runbook-gallery.md).

## <a name="creating-a-new-runbook"></a>Een nieuwe runbook maken

U kunt een nieuwe runbook in Azure automatisering met behulp van een van de Windows PowerShell of Azure portals maken. Zodra de runbook is gemaakt, kunt u met behulp van informatie in [Learning PowerShell Workflow](automation-powershell-workflow.md) en [grafische ontwerpen in Azure automatisering](automation-graphical-authoring-intro.md).

### <a name="to-create-a-new-azure-automation-runbook-with-the-azure-classic-portal"></a>Een nieuwe Azure automatisering runbook maken met de klassieke Azure portal

U kunt alleen werken met [PowerShell Workflow runbooks](automation-runbook-types.md#powershell-workflow-runbooks) in Azure portal.

1. In de klassieke Azure portal, klikt u op **Nieuw**, **App-Services**, **automatisering**, **Runbook**, **Snel maken**.
2. Voer de vereiste gegevens in en klik vervolgens op **maken**. De naam van de runbook kan moet beginnen met een letter en letters, cijfers, onderstrepingstekens en streepjes.
3. Als u wilt nu de runbook bewerken en klik op **Runbook bewerken**. Klik op **OK**.
4. De nieuwe runbook wordt weergegeven op het tabblad **Runbooks** .


### <a name="to-create-a-new-azure-automation-runbook-with-the-azure-portal"></a>Voor het maken van een nieuwe runbook van Azure automatisering met Azure portal

1. Open uw account automatisering in de Azure portal.
2. Klik op de tegel **Runbooks** opent de lijst met runbooks.
3. Klik op de knop **toevoegen, een runbook** en **een nieuwe runbook maken**.
2. Typ een **naam** voor de runbook en selecteert u het [Type](automation-runbook-types.md). De naam van de runbook kan moet beginnen met een letter en letters, cijfers, onderstrepingstekens en streepjes.
3. Klik op **maken** om de runbook te maken en open de editor.


### <a name="to-create-a-new-azure-automation-runbook-with-windows-powershell"></a>Een nieuwe Azure automatisering runbook maken met Windows PowerShell

U kunt de cmdlet [New-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt619376.aspx) voor het maken van een lege [runbook PowerShell Workflow](automation-runbook-types.md#powershell-workflow-runbooks). U kunt de parameter **Name** maakt een leeg runbook dat u later kunt bewerken als u wilt opgeven of kunt u het **pad** in de parameter om een runbook-bestand te importeren. De parameter **Type** moet ook worden opgenomen als u wilt opgeven dat een van de vier runbook typen.

De volgende voorbeeldopdrachten hoe maakt u een nieuwe lege runbook.

    New-AzureRmAutomationRunbook -AutomationAccountName MyAccount `
    -Name NewRunbook -ResourceGroupName MyResourceGroup -Type PowerShell

## <a name="importing-a-runbook-from-a-file-into-azure-automation"></a>Een runbook importeren uit een bestand in Azure automatisering

Kunt u een nieuwe runbook in Azure automatisering door het importeren van een PowerShell script of PowerShell Workflow (extensie .ps1) of een geëxporteerde grafische runbook (.graphrunbook).  Moet u het [type runbook](automation-runbook-types.md) dat wordt gemaakt van de rekening wordt gehouden met de volgende overwegingen importeren.

- Een .graphrunbook-bestand kan alleen worden geïmporteerd in een nieuwe [grafische runbook](automation-runbook-types.md#graphical-runbooks)en grafische runbooks kunnen alleen worden gemaakt van een .graphrunbook-bestand.
- Een .ps1-bestand met een werkstroom PowerShell kan alleen worden geïmporteerd in een [werkstroom PowerShell runbook](automation-runbook-types.md#powershell-workflow-runbooks).  Als het bestand meerdere Workflows met PowerShell bevat, wordt het importeren mislukken. Als u een apart bestand opslaan in elke workflow en elk afzonderlijk importeren.
- Een .ps1-bestand een werkstroom niet bevat kan worden geïmporteerd in een [PowerShell-runbook](automation-runbook-types.md#powershell-runbooks) of een [werkstroom PowerShell runbook](automation-runbook-types.md#powershell-workflow-runbooks).  Als deze wordt geïmporteerd in een werkstroom PowerShell runbook, vervolgens wordt geconverteerd naar een workflow en opmerkingen worden opgenomen in de runbook de wijzigingen die zijn aangebracht op te geven.

### <a name="to-import-a-runbook-from-a-file-with-the-azure-classic-portal"></a>Een runbook importeren uit een bestand met de klassieke Azure portal
De volgende procedure kunt u een scriptbestand importeren in Azure automatisering.  Opmerking u kunt alleen een .ps1-bestand importeren in een werkstroom PowerShell runbook met behulp van deze portal.  Voor andere typen moet u de portal Azure.

1. In de portal Management Azure **automatisering** selecteren en selecteer vervolgens een Account voor automatisering.
2. Klik op **importeren**.
3. Klik op **Zoeken naar een bestand** en zoek het script dat u wilt importeren.
4. Als u wilt nu de runbook bewerken en klik op **Runbook bewerken**. Klik op OK.
5. De nieuwe runbook wordt weergegeven op het tabblad **Runbooks** voor de rekening van de automatisering.
6. Moet u [de runbook publiceren](#publishing-a-runbook) voordat u deze kunt uitvoeren.


### <a name="to-import-a-runbook-from-a-file-with-the-azure-portal"></a>Een runbook importeren uit een bestand met de Azure portal
De volgende procedure kunt u een scriptbestand importeren in Azure automatisering.  

>[AZURE.NOTE] Opmerking u kunt alleen een .ps1-bestand importeren in een werkstroom PowerShell runbook met behulp van de portal.

1. Open uw account automatisering in de Azure portal.
2. Klik op de tegel **Runbooks** opent de lijst met runbooks.
3. Klik op de knop **een runbook toevoegen** en klik vervolgens op **importeren**.
4. Klik op **Runbook-bestand** om het te importeren bestand selecteren
2. Als **het veld** is ingeschakeld, hebt u de optie om deze te wijzigen.  De naam van de runbook kan moet beginnen met een letter en letters, cijfers, onderstrepingstekens en streepjes.
3. Het [type runbook](automation-runbook-types.md) wordt automatisch geselecteerd, maar u kunt het koppelingstype wijzigen na de beperkingen van toepassing rekening houdend met. 
3. De nieuwe runbook worden weergegeven in de lijst met runbooks voor de rekening van de automatisering.
4. Moet u [de runbook publiceren](#publishing-a-runbook) voordat u deze kunt uitvoeren.

>[AZURE.NOTE] Nadat u een grafische runbook of een grafische runbook van PowerShell workflow importeren, hebt u de mogelijkheid om te converteren naar het andere type als wilde. U converteren niet naar tekst.

### <a name="to-import-a-runbook-from-a-script-file-with-windows-powershell"></a>Importeren van een runbook van een scriptbestand met Windows PowerShell

De cmdlet [Import-AzureRMAutomationRunbook](https://msdn.microsoft.com/library/mt603735.aspx) kunt u een scriptbestand importeert als een concept runbook PowerShell Workflow. Als de runbook al bestaat, mislukt het importeren als u de *-Force* parameter. 

De volgende voorbeeldopdrachten hoe een scriptbestand importeren in een runbook.

    $automationAccountName =  "AutomationAccount"
    $runbookName = "Sample_TestRunbook"
    $scriptPath = "C:\Runbooks\Sample_TestRunbook.ps1"
    $RGName = "ResourceGroup"

    Import-AzureRMAutomationRunbook -Name $runbookName -Path $scriptPath `
    -ResourceGroupName $RGName -AutomationAccountName $automationAccountName `
    -Type PowerShellWorkflow 


## <a name="publishing-a-runbook"></a>Publiceren van een runbook

Wanneer u een nieuwe runbook te importeren of maakt, moet u deze publiceren voordat u deze kunt uitvoeren.  Elke runbook in automatisering is een concept en een gepubliceerde versie. Alleen de gepubliceerde versie is beschikbaar om te worden uitgevoerd en alleen de conceptversie kan worden bewerkt. De gepubliceerde versie wordt niet beïnvloed door wijzigingen in de conceptversie. Als de conceptversie beschikbaar gemaakt moet worden, vervolgens publiceert u hierbij de gepubliceerde versie met de versie van het concept overschrijft.

## <a name="to-publish-a-runbook-using-the-azure-classic-portal"></a>Voor het publiceren van een runbook met de klassieke Azure portal

1. Open het runbook in de klassieke Azure portal.
1. Aan de bovenkant van het scherm, klikt u op **auteur**.
1. Klik op **publiceren** en vervolgens op **Ja** om het bericht onderaan het scherm.

## <a name="to-publish-a-runbook-using-the-azure-portal"></a>Voor het publiceren van een runbook met de Azure portal

1. Open het runbook in het portal voor Azure.
1. Klik op de knop **bewerken** .
1. Klik op de knop **publiceren** en vervolgens **Ja** om het bericht.


## <a name="to-publish-a-runbook-using-windows-powershell"></a>Voor het publiceren van een runbook met Windows PowerShell

U kunt de cmdlet [Publiceren AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603705.aspx) voor het publiceren van een runbook met Windows PowerShell. De volgende voorbeeldopdrachten hoe een runbook monster te publiceren.

    $automationAccountName =  AutomationAccount"
    $runbookName = "Sample_TestRunbook"
    $RGName = "ResourceGroup"

    Publish-AzureRmAutomationRunbook -AutomationAccountName $automationAccountName `
    -Name $runbookName -ResourceGroupName $RGName


## <a name="next-steps"></a>Volgende stappen
- Zie voor meer informatie over hoe u van de Runbook en PowerShell Module galerie profiteren kunt [galerieën met Runbook en de module voor het automatiseren van Azure](automation-runbook-gallery.md)
- Zie voor meer informatie over het bewerken van PowerShell en PowerShell Workflow runbooks met een tekst-editor, [tekstuele runbooks in Azure automatisering bewerken](automation-edit-textual-runbook.md)
- Zie voor meer informatie over het ontwerpen van grafische runbook, [grafische ontwerpen in Azure automatisering](automation-graphical-authoring-intro.md)
