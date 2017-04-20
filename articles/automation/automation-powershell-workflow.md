<properties 
   pageTitle="Leren PowerShell Workflow"
   description="Dit artikel is bedoeld als een snelle les voor auteurs die bekend zijn met PowerShell om de specifieke verschillen tussen PowerShell en PowerShell Workflow."
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
   ms.author="bwren" />

# <a name="learning-windows-powershell-workflow"></a>Leren Windows PowerShell Workflow

Runbooks in Azure automatisering worden geïmplementeerd als een Windows PowerShell-Workflows.  Een Windows PowerShell-Workflow is vergelijkbaar met een Windows PowerShell-script, maar heeft enkele belangrijke verschillen die verwarrend voor een nieuwe gebruiker zijn kunnen.  Dit artikel is bedoeld voor gebruikers die bekend zijn met PowerShell en concepten die u nodig hebt als u een PowerShell script naar een PowerShell Workflow voor gebruik in een runbook converteert in het kort uitgelegd.  

Een workflow is een reeks stappen geprogrammeerde, verbonden die langdurige taken uit te voeren of de coördinatie van de verschillende stappen nodig over meerdere apparaten of beheerde knooppunten. De voordelen van een workflow op een normale script bevatten de mogelijkheid een actie tegen meerdere apparaten tegelijk uitvoeren en automatisch te herstellen na storingen. Een werkstroom van Windows PowerShell is een Windows PowerShell-script dat gebruikmaakt van Windows Workflow Foundation. Terwijl de werkstroom wordt geschreven met de syntaxis van Windows PowerShell en door Windows PowerShell gestart, wordt deze verwerkt door Windows Workflow Foundation.

Zie [Aan de slag met Windows PowerShell Workflow](http://technet.microsoft.com/library/jj134242.aspx)voor volledige informatie over de onderwerpen in dit artikel.

## <a name="types-of-runbook"></a>Soorten runbook

Er zijn drie soorten runbook in automatisering Azure, *PowerShell Workflow*, *PowerShell* en *grafische*.  U definieert het type van de runbook bij het maken van de runbook, en u niet een runbook naar het andere type converteren nadat deze is gemaakt.

PowerShell workflow runbooks en PowerShell runbooks voor gebruikers die liever werkt rechtstreeks met de PowerShell code hetzij gebruikt in Azure automatisering de tekst-editor of een off line editor zoals PowerShell ISE. Als u een runbook PowerShell Workflow maakt, moet u de informatie in dit artikel weten. 

Grafische runbooks kunt u voor het maken van een runbook met behulp van de activiteiten en de cmdlets, maar via een grafische interface waarmee de complexiteit van de onderliggende PowerShell workflow wordt verborgen.  Concepten in dit artikel als controlepunten en parallelle uitvoering nog steeds van toepassing op grafische runbooks, maar heb je geen zorgen te maken over de syntaxis van de gedetailleerde. 

## <a name="basic-structure-of-a-workflow"></a>De basisstructuur van een werkstroom

De eerste stap bij het converteren van een PowerShell script aan een workflow PowerShell is tussen het sleutelwoord **Workflow** .  Een werkstroom wordt gestart met de **werkstroom** trefwoord gevolgd door de tekst van het script tussen accolades. De naam van de werkstroom, zoals in de volgende syntaxis het sleutelwoord **Workflow** volgt. 

    Workflow Test-Workflow
    {
       <Commands>
    }

De naam van de werkstroom moet overeenkomen met de naam van de runbook voor automatisering. Als de runbook wordt geïmporteerd, de bestandsnaam moet overeenkomen met de naam van de werkstroom en moet de extensie .ps1.

Als parameters wilt toevoegen aan de workflow, gebruik het sleutelwoord **Param** net zoals u met een script. 

## <a name="code-changes"></a>Codewijzigingen

PowerShell workflow code ziet er bijna identiek zijn aan PowerShell script-code, met uitzondering van enkele belangrijke wijzigingen.  De volgende secties worden de wijzigingen die u nodig hebt uit te voeren in een werkstroom een PowerShell-script voor het aanbrengen.

### <a name="activities"></a>Activiteiten

Een activiteit is een specifieke taak in de werkstroom. Net als een script uit een of meer opdrachten bestaat, een werkstroom bestaat uit een of meer activiteiten die worden uitgevoerd in een reeks. Windows PowerShell Workflow omgezet automatisch in veel van de Windows PowerShell-cmdlets activiteiten tijdens het uitvoeren van een werkstroom. Wanneer u een van deze cmdlets in uw runbook opgeeft, wordt de bijbehorende activiteit daadwerkelijk uitvoeren door Windows Workflow Foundation. De cmdlets zonder een bijbehorende activiteit, Windows PowerShell Workflow automatisch wordt uitgevoerd de cmdlet binnen een [InlineScript](#inlinescript) activiteit. Er is een set cmdlets die uitgesloten zijn en in een werkstroom kan niet worden gebruikt tenzij u expliciet in een blok voor InlineScript opnemen. Zie voor meer informatie over deze concepten [Met behulp van activiteiten in Script-Workflows](http://technet.microsoft.com/library/jj574194.aspx).

Workflowactiviteiten delen een reeks gemeenschappelijke parameters voor het configureren van de werking. Zie [about_WorkflowCommonParameters](http://technet.microsoft.com/library/jj129719.aspx)voor meer informatie over de werkstroom algemene parameters.

### <a name="positional-parameters"></a>Positionele parameters

U kunt met activiteiten en in een werkstroom-cmdlets positionele parameters gebruiken.  Alle deze middelen is dat u de parameternamen moet gebruiken.

Neem bijvoorbeeld de volgende code alle actieve services haalt.

     Get-Service | Where-Object {$_.Status -eq "Running"}

Als u deze code uitvoert in een werkstroom, krijgt u een bericht zoals "set Parameter kan niet worden omgezet met behulp van het opgegeven benoemde parameters."  Verstrek om dit te corrigeren, zoals hieronder de parameternaam van de.

    Workflow Get-RunningServices
    {
        Get-Service | Where-Object -FilterScript {$_.Status -eq "Running"}
    }

### <a name="deserialized-objects"></a>Gedeserialiseerd objecten

Objecten in werkstromen worden gedeserialiseerd.  Dit betekent dat hun eigenschappen zijn nog steeds beschikbaar, maar niet de methoden.  Neem bijvoorbeeld de volgende PowerShell code waardoor een service met de methode Stop van het object.

    $Service = Get-Service -Name MyService
    $Service.Stop()

Als u dit in een werkstroom wordt uitgevoerd, krijgt u een foutmelding 'methode-aanroep wordt niet ondersteund in een Windows PowerShell-werkstroom' verschijnt.  

Een optie is om deze twee regels code in een [InlineScript](#InlineScript) -blok, in welk geval $Service een serviceobject binnen het blok zou zijn. 

    Workflow Stop-Service
    {
        InlineScript {
            $Service = Get-Service -Name MyService
            $Service.Stop()
        }
    } 

Een andere optie is met een andere cmdlet die dezelfde functionaliteit als de methode wordt uitgevoerd als er een beschikbaar is.  In het geval van ons voorbeeld, de cmdlet Stop-Service biedt dezelfde functionaliteit als de methode Stop en kunt u het volgende voor een werkstroom.

    Workflow Stop-MyService
    {
        $Service = Get-Service -Name MyService
        Stop-Service -Name $Service.Name
    }


## <a name="inlinescript"></a>InlineScript

De activiteit **InlineScript** is handig wanneer moet u een of meer opdrachten uitvoeren als traditionele PowerShell script in plaats van PowerShell workflow.  Terwijl de opdrachten in een werkstroom voor verwerking naar Windows Workflow Foundation worden verzonden, worden door Windows PowerShell-opdrachten in een blok InlineScript verwerkt. 

InlineScript maakt gebruik van de onderstaande syntaxis.

    InlineScript
    {
      <Script Block>
    } <Common Parameters>

Uitvoer kunt van een InlineScript u terugkeren door de uitvoer toe te wijzen aan een variabele. In het volgende voorbeeld wordt een service gestopt en wordt vervolgens de servicenaam van de.

    Workflow Stop-MyService
    {
        $Output = InlineScript {
            $Service = Get-Service -Name MyService
            $Service.Stop()
            $Service
        }

        $Output.Name
    }


U kunt waarden doorgeven in een blok InlineScript, maar **$Using** scope-optie moet worden gebruikt.  In het volgende voorbeeld is gelijk aan het vorige voorbeeld, behalve dat de servicenaam van de wordt geleverd door een variabele. 

    Workflow Stop-MyService
    {
        $ServiceName = "MyService"
    
        $Output = InlineScript {
            $Service = Get-Service -Name $Using:ServiceName
            $Service.Stop()
            $Service
        }

        $Output.Name
    }


Hoewel InlineScript activiteiten mogelijk in sommige workflows voor kritieke, ze bieden geen ondersteuning voor workflow constructies en mag alleen worden gebruikt wanneer dat nodig is om de volgende redenen:

- [Controlepunten](#Checkpoints) in een blok voor InlineScript kunt u niet gebruiken. Als een fout in het blok optreedt, moet het worden hervat vanaf het begin van het blok.
- [Parallelle uitvoering](#parallel-execution) in een InlineScriptBlock kunt u niet gebruiken.
- InlineScript heeft invloed op de schaalbaarheid van de werkstroom omdat het bezit is van de Windows PowerShell-sessie voor de hele lengte van de InlineScript blokkeren.

Zie [Windows PowerShell-opdrachten uitvoeren in een werkstroom](http://technet.microsoft.com/library/jj574197.aspx) en [about_InlineScript](http://technet.microsoft.com/library/jj649082.aspx)voor meer informatie over het gebruik van InlineScript.


## <a name="parallel-processing"></a>Parallelle verwerking

Een voordeel van Windows PowerShell werkstromen is de mogelijkheid voor het uitvoeren van een reeks opdrachten in plaats van parallel opeenvolgend net als bij een normale script. 

De **parallelle** trefwoord kunt u een scriptblok maken met meerdere opdrachten die gelijktijdig wordt uitgevoerd. Dit wordt gebruikt voor de onderstaande syntaxis. In dit geval, wordt Activity1 en Activity2 gestart op hetzelfde moment. Activity3 begint pas nadat u zowel Activity1 als Activity2 hebt voltooid.

    Parallel
    {
      <Activity1>
      <Activity2>
    }
    <Activity3>


Neem bijvoorbeeld de volgende PowerShell-opdrachten waarmee u meerdere bestanden naar een netwerkbestemming kopieert.  Deze opdrachten worden opeenvolgend uitgevoerd, zodanig dat een bestand moet zijn voltooid voordat wordt gestart met de volgende kopiëren.     

    $Copy-Item -Path C:\LocalPath\File1.txt -Destination \\NetworkPath\File1.txt
    $Copy-Item -Path C:\LocalPath\File2.txt -Destination \\NetworkPath\File2.txt
    $Copy-Item -Path C:\LocalPath\File3.txt -Destination \\NetworkPath\File3.txt

De volgende workflow wordt deze opdrachten gelijktijdig uitgevoerd zodat ze allemaal tegelijk kopiëren.  Pas nadat ze alle zijn volledig gekopieerd het voltooiingsbericht weergegeven.

    Workflow Copy-Files
    {
        Parallel 
        {
            $Copy-Item -Path "C:\LocalPath\File1.txt" -Destination "\\NetworkPath"
            $Copy-Item -Path "C:\LocalPath\File2.txt" -Destination "\\NetworkPath"
            $Copy-Item -Path "C:\LocalPath\File3.txt" -Destination "\\NetworkPath"
        }

        Write-Output "Files copied."
    }


U kunt de **ForEach-parallelle** constructie met opdrachten in het proces voor elk item in een collectie gelijktijdig. De items in de collectie worden parallel verwerkt tijdens het uitvoeren van de opdrachten in het scriptblok opeenvolgend. Dit wordt gebruikt voor de onderstaande syntaxis. In dit geval start Activity1 op hetzelfde moment voor alle items in de collectie. Voor elk artikel start Activity2 nadat de Activity1 is voltooid. Activity3 begint pas nadat u zowel Activity1 als Activity2 voor alle items hebt voltooid.

    ForEach -Parallel ($<item> in $<collection>)
    {
      <Activity1>
      <Activity2>
    }
    <Activity3>

In het volgende voorbeeld is vergelijkbaar met het vorige voorbeeld bestanden tegelijkertijd kopiëren.  In dit geval wordt een bericht weergegeven voor elk bestand nadat deze zijn gekopieerd.  Pas nadat ze alle zijn volledig gekopieerd is de uiteindelijke voltooiingsbericht weergegeven.

    Workflow Copy-Files
    {
        $files = @("C:\LocalPath\File1.txt","C:\LocalPath\File2.txt","C:\LocalPath\File3.txt")

        ForEach -Parallel ($File in $Files) 
        {
            $Copy-Item -Path $File -Destination \\NetworkPath
            Write-Output "$File copied."
        }
        
        Write-Output "All files copied."
    }

> [AZURE.NOTE]  Onderliggende runbooks parallelle uitvoering aangezien dit blijkt dat de resultaten onbetrouwbaar wordt niet aanbevolen.  De uitvoer van de onderliggende runbook soms niet weergegeven en instellingen in een onderliggende runbook kunnen invloed hebben op de andere parallelle onderliggende runbooks 


## <a name="checkpoints"></a>Controlepunten

Een *controlepunt* is een momentopname van de huidige status van de werkstroom met de huidige waarde voor variabelen en geen uitvoer gegenereerd naar dat punt. Als een werkstroom in fout eindigt of is geschorst, wordt vervolgens de volgende keer die wordt uitgevoerd gestart vanaf de laatste checkpoint in plaats van het begin van de worfklow.  Een controlepunt kunt u instellen in een workflow met de activiteit **Checkpoint-Workflow** .

In de volgende voorbeeldcode wordt een uitzondering voordoet na Activity2 veroorzaakt door de werkstroom te beëindigen. Het starten met Activity2 omdat dit vlak nadat het laatste controlepunt ingesteld wanneer de werkstroom opnieuw wordt uitgevoerd.

    <Activity1>
    Checkpoint-Workflow
    <Activity2>
    <Exception>
    <Activity3>

Nadat de activiteiten die mogelijk gevoelig voor uitzondering en dient niet te worden herhaald als de werkstroom wordt hervat, moet u de controlepunten instellen in een werkstroom. De werkstroom kan bijvoorbeeld een virtuele machine maken. U kunt een controlepunt instellen, zowel vóór als na de opdrachten voor het maken van de virtuele machine. Als het maken is mislukt, zou vervolgens de opdrachten worden herhaald als de workflow opnieuw wordt gestart. Als het de worfklow mislukt nadat het maken is geslaagd, wordt de virtuele machine niet opnieuw gemaakt wordt wanneer de werkstroom wordt hervat.

In het volgende voorbeeld worden meerdere bestanden kopieert naar een netwerklocatie en een controlepunt ingesteld na elk bestand.  Als de netwerklocatie verbroken wordt, wordt de werkstroom beëindigd fout.  Wanneer opnieuw wordt gestart, wordt deze weer op het laatste controlepunt, wat betekent dat alleen de bestanden die al zijn gekopieerd wordt overgeslagen.

    Workflow Copy-Files
    {
        $files = @("C:\LocalPath\File1.txt","C:\LocalPath\File2.txt","C:\LocalPath\File3.txt")

        ForEach ($File in $Files) 
        {
            $Copy-Item -Path $File -Destination \\NetworkPath
            Write-Output "$File copied."
            Checkpoint-Workflow
        }
        
        Write-Output "All files copied."
    }

Omdat de referenties in gebruikersnaam worden niet doorgevoerd na het aanroepen van de [Stand-bymodus Workflow](https://technet.microsoft.com/library/jj733586.aspx) activiteit of na het laatste controlepunt, moet u de te stellen op null en vervolgens haalt ze opnieuw uit het archief van de activa na **Onderbreking Workflow** of controlepunt wordt genoemd.  Anders verschijnt het volgende foutbericht weergegeven: *de werkstroomtaak kan niet worden voortgezet, hetzij omdat de permanente gegevens kan niet worden opgeslagen, of opgeslagen gegevens persistentie is beschadigd. U moet de workflow opnieuw.*

De volgende code dezelfde laat zien hoe dit in uw werkstroom PowerShell runbooks verwerken.

       
    workflow CreateTestVms
    {
       $Cred = Get-AzureAutomationCredential -Name "MyCredential"
       $null = Add-AzureRmAccount -Credential $Cred

       $VmsToCreate = Get-AzureAutomationVariable -Name "VmsToCreate"

       foreach ($VmName in $VmsToCreate)
         {
          # Do work first to create the VM (code not shown)
        
          # Now add the VM
          New-AzureRmVm -VM $Vm -Location "WestUs" -ResourceGroupName "ResourceGroup01"

          # Checkpoint so that VM creation is not repeated if workflow suspends
          $Cred = $null
          Checkpoint-Workflow
          $Cred = Get-AzureAutomationCredential -Name "MyCredential"
          $null = Add-AzureRmAccount -Credential $Cred
         }
     } 


Dit is niet vereist als u zich aanmelden met een uitvoeren als-account geconfigureerd met een service principal.  

Zie voor meer informatie over controlepunten [Controlepunten toevoegen aan een Script-Workflow](http://technet.microsoft.com/library/jj574114.aspx).


## <a name="next-steps"></a>Volgende stappen

- Om te beginnen met PowerShell workflow runbooks, Zie [Mijn eerste PowerShell workflow runbook](automation-first-runbook-textual.md) 
