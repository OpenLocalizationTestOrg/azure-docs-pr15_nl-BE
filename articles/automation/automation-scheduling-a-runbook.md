<properties 
   pageTitle="Plannen van een runbook in Azure automatisering | Microsoft Azure"
   description="Hierin wordt beschreven hoe een schema maken in Azure automatisering, zodat u een runbook automatisch op een bepaald tijdstip of op regelmatige basis starten kunt."
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
   ms.date="08/05/2016"
   ms.author="bwren" />

# <a name="scheduling-a-runbook-in-azure-automation"></a>Plannen van een runbook in Azure automatisering

Als u een runbook in Azure automatisering te starten op een opgegeven tijdstip plannen, koppelen u deze aan een of meer schema's. Een planning wilt uitvoeren op een opnieuw optreedt of één keer per uur of per dag plannen voor runbooks in de klassieke Azure portal en runbooks in de Azure portal kan worden geconfigureerd, kunt u verder plannen ze voor wekelijks, maandelijks, specifieke dagen van de week of dagen van de maand of een bepaalde dag van de maand.  Een runbook kan worden gekoppeld aan meerdere schema's en een schema kan meerdere runbooks gekoppeld hebben.


## <a name="creating-a-schedule"></a>Een planning maken

In Azure portal in de klassieke portal of met Windows PowerShell kunt u een nieuw schema voor runbooks. U hebt ook de optie een nieuwe planning maken wanneer u een runbook aan een schema met de Azure klassieke of Azure portal koppelen.

>[AZURE.NOTE] Wanneer u een schema aan een runbook koppelen, automatisering slaat de huidige versies van de modules in uw account en worden deze op schema.  Dit betekent dat als u een module met versie 1.0 in uw account wanneer u een planning gemaakt en vervolgens de module naar versie 2.0 bijwerken, de planning blijft 1.0 gebruiken.  De bijgewerkte versie gebruiken, moet u een nieuw schema maken. 

### <a name="to-create-a-new-schedule-in-the-azure-classic-portal"></a>Een nieuwe planning maken in de klassieke Azure portal

1. In de klassieke Azure portal automatisering selecteert en vervolgens selecteert u de naam van een account met automatisering.
1. Selecteer het tabblad **activa** .
1. Klik op **Toevoegen**onder aan het venster.
1. Klik op **schema toevoegen**.
1. Typ een **naam** en eventueel een **Beschrijving** voor de nieuwe planning van schedule.your **Één keer**, **per uur**, **dagelijks**, **Wekelijks**of **maandelijks**wordt uitgevoerd.
1. Geef een **Begintijd** en andere opties afhankelijk van het type schema dat u hebt geselecteerd.

### <a name="to-create-a-new-schedule-in-the-azure-portal"></a>Een nieuwe planning maken in de portal voor Azure

1. Klik op de tegel **activa** om te openen de blade **activa** in Azure portal van uw account automatisering.
2. Klik op de tegel **schema's** om te openen de blade **schema's** .
3. Klik op **een schema toevoegen** aan de bovenkant van het blad.
4. Typ een **naam** en eventueel een **Beschrijving** voor het nieuwe schema op de **nieuwe planning** -blade.
5. Selecteer het schema wordt één keer uitgevoerd, of in een terugkerende planning door **eenmaal** of het **Terugkeerpatroon**te selecteren.  Als u **eenmaal** een **Begintijd** opgeven en klik vervolgens op **maken**.  Als u een **Terugkeerpatroon**selecteert, geeft u een **Begintijd** en de frequentie van hoe vaak de runbook moet worden herhaald - per **uur**, **dag**, **week**of per **maand**.  Als u in de vervolgkeuzelijst **week** of **maand** selecteert, het **Terugkeerpatroon optie** wordt weergegeven in het blad en bij de selectie, het **Terugkeerpatroon optie** blade krijgt en kunt u de dag van de week als u **week geselecteerde**.  Als u een **maand**hebt geselecteerd, kiest u op **weekdagen** of specifieke dagen van de maand in de agenda en tot slot gaat u wilt uitvoeren op de laatste dag van de maand of niet en klik vervolgens op **OK**.   

### <a name="to-create-a-new-schedule-with-windows-powershell"></a>Een nieuwe planning maken met Windows PowerShell

U kunt de cmdlet [New-AzureAutomationSchedule](http://msdn.microsoft.com/library/azure/dn690271.aspx) een nieuwe planning maken in Azure automatisering voor klassieke runbooks, of de cmdlet [New-AzureRmAutomationSchedule](https://msdn.microsoft.com/library/mt603577.aspx) voor runbooks in het portal voor Azure. Moet u de begintijd voor de planning en de frequentie die moet worden uitgevoerd.

De volgende voorbeeldopdrachten hoe een nieuw schema dat elke dag om 3:30 PM op 20 januari 2015 starten met een cmdlet Azure Service Management wordt uitgevoerd.

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-DailySchedule"
    New-AzureAutomationSchedule –AutomationAccountName $automationAccountName –Name `
    $scheduleName –StartTime "1/20/2016 15:30:00" –DayInterval 1

De volgende voorbeeldopdrachten ziet u hoe u een planning opstellen voor de 15e en 30e van elke maand met een cmdlet Azure Resource Manager.

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-MonthlyDaysOfMonthSchedule"
    New-AzureRMAutomationSchedule –AutomationAccountName $automationAccountName –Name `
    $scheduleName -StartTime "7/01/2016 15:30:00" -MonthInterval 1 `
    -DaysOfMonth Fifteenth,Thirtieth -ResourceGroupName "ResourceGroup01"
    

## <a name="linking-a-schedule-to-a-runbook"></a>Een schema koppelen aan een runbook

Een runbook kan worden gekoppeld aan meerdere schema's en een schema kan meerdere runbooks gekoppeld hebben. Als een runbook parameters heeft, kunt u waarden opgeven voor hen. U moet waarden opgeven voor de verplichte parameters en waarden voor de optionele parameters kan bepalen.  Deze waarden worden gebruikt telkens wanneer die de runbook wordt gestart door dit schema.  U kunt de dezelfde runbook koppelen aan een ander schema en andere parameterwaarden opgeven.


### <a name="to-link-a-schedule-to-a-runbook-with-the-azure-classic-portal"></a>Een schema aan een runbook met de klassieke Azure portal koppelen

1. In de klassieke Azure portal, selecteer **automatisering** en klikt u vervolgens de naam van een account met automatisering.
2. Selecteer het tabblad **Runbooks** .
3. Klik op de naam van de runbook te plannen.
4. Klik op het tabblad **planning** .
5. Als de runbook is momenteel niet gekoppeld aan een schema, vervolgens krijgt u de optie om een **koppeling naar een nieuwe planning** of een **koppeling naar een bestaand schema**.  Als de runbook is momenteel gekoppeld aan een schema, klik op de **koppeling** onder aan het venster voor toegang tot deze opties.
6. Als de runbook parameters heeft, wordt u gevraagd hun waarden.  

### <a name="to-link-a-schedule-to-a-runbook-with-the-azure-portal"></a>Een schema aan een runbook met Azure portal koppelen

1. Klik op de tegel **Runbooks** de blade **Runbooks** openen in Azure portal van uw account automatisering.
2. Klik op de naam van de runbook te plannen.
3. Als de runbook is momenteel niet gekoppeld aan een schema, vervolgens krijgt u de optie voor het maken van een nieuwe planning of een koppeling naar een bestaand schema.  
4. De runbook parameters heeft, kunt u de optie **wijzigen (standaard: Azure) instellingen uitvoeren** als de **Parameters** blade waar kunt u de informatie dienovereenkomstig wordt gepresenteerd.  

### <a name="to-link-a-schedule-to-a-runbook-with-windows-powershell"></a>Een schema koppelen aan een runbook met Windows PowerShell

De [Kassa-AzureAutomationScheduledRunbook](http://msdn.microsoft.com/library/azure/dn690265.aspx) kunt u een schema aan een klassieke runbook of cmdlet [Kassa AzureRmAutomationScheduledRunbook](https://msdn.microsoft.com/library/mt603575.aspx) voor runbooks in de Azure portal koppelen.  Met de parameter Parameters kunt u waarden voor de parameters van de runbook. Zie [een Runbook in Azure automatisering starten](automation-starting-a-runbook.md) voor meer informatie over het opgeven van de parameterwaarden.

De volgende voorbeeldopdrachten hoe een schema met een Azure Service Management-cmdlet en parameters te koppelen.

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Test-Runbook"
    $scheduleName = "Sample-DailySchedule"
    $params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
    Register-AzureAutomationScheduledRunbook –AutomationAccountName $automationAccountName `
    –Name $runbookName –ScheduleName $scheduleName –Parameters $params

De volgende voorbeeldopdrachten hoe een schema koppelen aan een runbook met een bronnenbeheerder Azure-cmdlet en parameters.

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Test-Runbook"
    $scheduleName = "Sample-DailySchedule"
    $params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
    Register-AzureRmAutomationScheduledRunbook –AutomationAccountName $automationAccountName `
    –Name $runbookName –ScheduleName $scheduleName –Parameters $params `
    -ResourceGroupName "ResourceGroup01"

## <a name="disabling-a-schedule"></a>Het uitschakelen van een schema

Wanneer u een planning uitschakelt, wordt een runbooks gekoppeld aan het niet meer uitgevoerd op schema. U kunt handmatig een schema uitschakelen of stel een verlooptijd voor schema's met een frequentie als u ze maakt. Wanneer de verlooptijd is bereikt, wordt de planning worden uitgeschakeld.

### <a name="to-disable-a-schedule-from-the-azure-classic-portal"></a>Een schema van de klassieke Azure portal uitschakelen

U kunt een schema in Azure klassieke portal op de pagina Details van het schema voor het schema uitschakelen.

1. In de klassieke Azure portal, selecteer automatisering en klikt u vervolgens de naam van een account met automatisering.
1. Selecteer het tabblad activa.
1. Klik op de naam van een planning op de detailpagina openen.
2. Wijzig **ingeschakeld** op **Nee**.

### <a name="to-disable-a-schedule-from-the-azure-portal"></a>Een planning vanaf de portal Azure uitschakelen

1. Klik op de tegel **activa** om te openen de blade **activa** in Azure portal van uw account automatisering.
2. Klik op de tegel **schema's** om te openen de blade **schema's** .
2. Klik op de naam van een schema voor het openen van de bladeserver details.
3. Wijzig **ingeschakeld** op **Nee**.

### <a name="to-disable-a-schedule-with-windows-powershell"></a>Het uitschakelen van een schema met Windows PowerShell

De cmdlet [Set-AzureAutomationSchedule](http://msdn.microsoft.com/library/azure/dn690270.aspx) kunt u de eigenschappen van een bestaand schema voor een klassieke runbook of de cmdlet [Set-AzureRmAutomationSchedule](https://msdn.microsoft.com/library/mt603566.aspx) voor runbooks in het portal voor Azure. Als u wilt uitschakelen in de planning, geef **false** voor de parameter **IsEnabled** .

De volgende voorbeeldopdrachten hoe een schema met de cmdlet Azure Service Management uitschakelen.

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-DailySchedule"
    Set-AzureAutomationSchedule –AutomationAccountName $automationAccountName `
    –Name $scheduleName –IsEnabled $false

De volgende voorbeeldopdrachten hoe een schema maken voor een runbook met een cmdlet Azure Resource Manager uitschakelen.

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-MonthlyDaysOfMonthSchedule"
    Set-AzureRmAutomationSchedule –AutomationAccountName $automationAccountName `
    –Name $scheduleName –IsEnabled $false -ResourceGroupName "ResourceGroup01"


## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over het werken met planningen, [Schema-elementen in Azure automatisering](http://msdn.microsoft.com/library/azure/dn940016.aspx)
- Om te beginnen met runbooks in Azure automatisering Zie [starten een Runbook in Azure automatisering](automation-starting-a-runbook.md) 