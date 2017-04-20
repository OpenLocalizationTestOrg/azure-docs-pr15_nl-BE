<properties 
   pageTitle="Onderliggende runbooks in Azure automatisering | Microsoft Azure"
   description="Beschrijving van de verschillende methoden voor het starten van een runbook in Azure automatisering vanuit een andere runbook en delen van informatie tussen hen."
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
   ms.date="08/17/2016"
   ms.author="magoedte;bwren" />

# <a name="child-runbooks-in-azure-automation"></a>Onderliggende runbooks in Azure automatisering

Het is raadzaam in Azure automatisering schrijven herbruikbare, modulaire runbooks met een aparte functie die kan worden gebruikt door andere runbooks. Een bovenliggende runbook roept vaak een of meer onderliggende runbooks voor het uitvoeren van de vereiste functionaliteit. Er zijn twee manieren om aan te roepen van een onderliggende runbook en elk heeft duidelijke verschillen die u moet begrijpen, zodat u kunt bepalen welke voor de verschillende scenario's.

##  <a name="invoking-a-child-runbook-using-inline-execution"></a>Aanroepen van een onderliggende runbook met in line worden uitgevoerd

Om aan te roepen van een inline runbook van een andere runbook, de naam van de runbook en waarden opgeven voor de parameters precies net als een activiteit of de cmdlet.  Alle runbooks op dezelfde rekening automatisering zijn beschikbaar voor alle andere gebruikers op deze manier worden gebruikt. De bovenliggende runbook wacht tot de onderliggende runbook voltooien voordat u naar de volgende regel en uitvoer rechtstreeks aan het bovenliggende object wordt geretourneerd.

Als u een inline runbook aanroept, wordt deze uitgevoerd in dezelfde taak als de bovenliggende runbook. Er zijn geen aanwijzingen in de geschiedenis van de onderliggende runbook die worden uitgevoerd. Uitzonderingen en alle uitvoer van de onderliggende runbook stream zijn gekoppeld aan het bovenliggende object. Dit resulteert in minder taken en maakt ze gemakkelijker om bij te houden en op te lossen omdat de eventuele uitzonderingen die door de onderliggende runbook en een van de stream uitvoer zijn gekoppeld aan de bovenliggende taak.

Wanneer een runbook wordt gepubliceerd, moeten alle onderliggende runbooks die roept al worden gepubliceerd. Dit komt doordat de Azure automatisering maakt een koppeling met de onderliggende runbooks tijdens het compileren van een runbook. Als ze niet, de bovenliggende runbook publiceren correct wordt weergegeven, maar wordt een uitzondering gegenereerd wanneer het wordt gestart. Als dit gebeurt, kunt u de bovenliggende runbook opnieuw publiceren om correct te verwijzen naar de onderliggende runbooks. U hoeft niet opnieuw te publiceren van de bovenliggende runbook als een van de onderliggende runbooks worden gewijzigd, omdat de koppeling wordt al zijn gemaakt.

De parameters van een onderliggende runbook inline genoemd kunnen worden gegevens met inbegrip van complexe objecten, en er geen [JSON serialisatie](automation-starting-a-runbook.md#runbook-parameters) omdat er bij het starten van de runbook met de Azure Management Portal of met de cmdlet Start AzureRmAutomationRunbook.


### <a name="runbook-types"></a>Typen Runbook

Welke typen kunnen bellen elkaar:

- Een [PowerShell runbook](automation-runbook-types.md#powershell-runbooks) en [grafische runbooks](automation-runbook-types.md#graphical-runbooks) kan elke andere inline (beide zijn op basis van PowerShell) aanroepen.
- Een runbooks [runbook PowerShell Workflow](automation-runbook-types.md#powershell-workflow-runbooks) en grafische PowerShell Workflow kunt bellen elke andere inline (beide zijn op basis van PowerShell-Workflow)
- De PowerShell-typen de typen PowerShell Workflow inline elkaar niet aanroepen en Start AzureRmAutomationRunbook moeten gebruiken.
    
Wanneer volgorde materiaal wordt gepubliceerd:

- Het publiceren van runbooks alleen acties voor PowerShell Workflow en grafische PowerShell Workflow runbooks.


Wanneer u een grafische of PowerShell Workflow onderliggende runbook met in line worden uitgevoerd, gebruikt u de naam van de runbook.  Wanneer u een kind PowerShell runbook aanroept, moet u de naam ervan met voorafgegaan *.\\ * om aan te geven dat het script in de lokale map zich bevindt. 

### <a name="example"></a>Voorbeeld

In het volgende voorbeeld wordt een test onderliggend runbook die drie parameters, een complexe object, een geheel getal en een Boolean-waarde accepteert. De uitvoer van de onderliggende runbook wordt toegewezen aan een variabele.  In dit geval is de onderliggende runbook een runbook PowerShell Workflow

    $vm = Get-AzureRmVM –ResourceGroupName "LabRG" –Name "MyVM"
    $output = PSWF-ChildRunbook –VM $vm –RepeatCount 2 –Restart $true

Hieronder ziet u hetzelfde voorbeeld met een PowerShell-runbook als het kind.

    $vm = Get-AzureRmVM –ResourceGroupName "LabRG" –Name "MyVM"
    $output = .\PS-ChildRunbook –VM $vm –RepeatCount 2 –Restart $true



##  <a name="starting-a-child-runbook-using-cmdlet"></a>De runbook van een kind met cmdlet starten

U kunt de cmdlet [Start AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603661.aspx) start een runbook zoals beschreven in de [start van een runbook met Windows PowerShell](../automation-starting-a-runbook.md#starting-a-runbook-with-windows-powershell). Er zijn twee modi voor deze cmdlet.  De cmdlet retourneert in de ene, de taak-id als de onderliggende taak voor de onderliggende runbook gemaakt.  In de andere modus, waarmee u door te geven de **-Wacht** parameter, de cmdlet wordt gewacht tot de onderliggende taak is voltooid en de uitvoer wordt geretourneerd uit de onderliggende runbook.

De taak van een onderliggende runbook gestart met een cmdlet wordt uitgevoerd in een afzonderlijke taak uit de bovenliggende runbook. Dit leidt tot meer functies dan de inline runbook roepen en maakt hen moeilijker om bij te houden. De bovenliggende kunt meerdere onderliggende runbooks asynchroon starten zonder te wachten totdat elk te voltooien. Voor die dezelfde soort parallelle uitvoering die aanroepen van de onderliggende runbooks inline, moet de bovenliggende runbook gebruikt u het [sleutelwoord parallelle](automation-powershell-workflow.md#parallel-processing).

Parameters voor een onderliggende runbook gestart met een cmdlet worden geleverd als een hashtable zoals beschreven in de [Parameters van de Runbook](automation-starting-a-runbook.md#runbook-parameters). Alleen eenvoudige gegevenstypen kunnen worden gebruikt. Als het runbook een parameter met een complex gegevenstype heeft, moet vervolgens het worden aangeroepen in line.

### <a name="example"></a>Voorbeeld

In het volgende voorbeeld een kind runbook begint met parameters en wacht tot het met behulp van de Start-AzureRmAutomationRunbook-parameter wachten. Zodra voltooid, wordt de uitvoer van de onderliggende runbook verzameld.

    $params = @{"VMName"="MyVM";"RepeatCount"=2;"Restart"=$true} 
    $joboutput = Start-AzureRmAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Test-ChildRunbook" -ResouceGroupName "LabRG" –Parameters $params –wait


## <a name="comparison-of-methods-for-calling-a-child-runbook"></a>Vergelijking van de methoden voor het aanroepen van een onderliggende runbook

De volgende tabel bevat een overzicht van de verschillen tussen de twee methoden voor het aanroepen van een runbook van een andere runbook.

| | In line| Cmdlet|
|:---|:---|:---|
|Taak|Onderliggende runbooks uitgevoerd in dezelfde taak als het bovenliggende element.|Een afzonderlijke taak gemaakt voor de onderliggende runbook.|
|Uitvoering|Bovenliggende runbook wacht op de onderliggende runbook voltooien voordat u doorgaat.|Bovenliggende runbook blijft direct na het onderliggende runbook wordt gestart *of* bovenliggende runbook wacht op de onderliggende taak is voltooid.|
|Uitvoer|Bovenliggende runbook krijgt uitvoer rechtstreeks uit de onderliggende runbook.|Bovenliggende runbook uitvoer moet ophalen uit de onderliggende runbook *of* bovenliggende runbook rechtstreeks krijg uitvoer uit de onderliggende runbook.|
|Parameters|Waarden voor de parameters van de runbook kind afzonderlijk worden opgegeven en alle gegevenstypen kunnen gebruiken.|Waarden voor de onderliggende runbook parameters moeten worden gecombineerd tot één hash-tabel en kunnen alleen eenvoudige matrix en object gegevenstypen die hefboomwerking JSON serialisatie.|
|Automatisering-Account|Bovenliggende runbook kan alleen onderliggende runbook op dezelfde rekening automatisering gebruiken.|Bovenliggende runbook kunt als u een verbinding met het hebt onderliggende runbook van elke account automatisering van het abonnement met dezelfde Azure en zelfs een ander abonnement.|
|Publiceren|Onderliggende runbook moet worden gepubliceerd voordat bovenliggende runbook wordt gepubliceerd.|Onderliggende runbook moet worden gepubliceerd voordat de bovenliggende runbook is gestart.|

## <a name="next-steps"></a>Volgende stappen

- [Een runbook starten in Azure automatisering](automation-starting-a-runbook.md)
- [Runbook output en berichten in Azure automatisering](automation-runbook-output-and-messages.md)
