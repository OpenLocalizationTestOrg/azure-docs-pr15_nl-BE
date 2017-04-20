<properties
    pageTitle="Mijn eerste runbook PowerShell Workflow in Azure automatisering | Microsoft Azure"
    description="Zelfstudie u bij het maken begeleidt, testen en publiceren van een eenvoudige tekst runbook via PowerShell Workflow."
    services="automation"
    documentationCenter=""
    authors="mgoedtel"
    manager="jwhit"
    editor=""
    keywords="PowerShell workflow powershell voorbeelden van werkstromen, werkstroom powershell"/>
<tags
    ms.service="automation"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="07/19/2016"
    ms.author="magoedte;bwren"/>

# <a name="my-first-powershell-workflow-runbook"></a>Mijn eerste PowerShell Workflow runbook

> [AZURE.SELECTOR] - [Grafische](automation-first-runbook-graphical.md) - [PowerShell](automation-first-runbook-textual-PowerShell.md) - [PowerShell Workflow](automation-first-runbook-textual.md)

Deze handleiding helpt u bij het maken van een [werkstroom PowerShell runbook](automation-runbook-types.md#powerShell-workflow-runbooks) in Azure automatisering. We beginnen met een eenvoudige runbook die we testen en publiceren terwijl leggen we uit hoe bij te houden van de status van de taak van de runbook. We zullen wijzigt u de runbook voor het beheren van bronnen Azure, werkelijk in dit geval een Azure virtuele machine te starten. Vervolgens gaat maken we de runbook nog krachtiger door runbook parameters toe te voegen.

## <a name="prerequisites"></a>Vereisten

Als u deze zelfstudie hebt voltooid, moet u het volgende.

-   Abonnement op Azure. Als u geen één nog hebt, kunt u [uw MSDN subscriber voordelen te activeren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) of <a href="/pricing/free-account/" target="_blank"> [Aanmelden voor een gratis account](https://azure.microsoft.com/free/).
-   [Automatisering rekening](automation-security-overview.md) houdt de runbook en geverifieerd bij bronnen Azure.  Deze account moet gemachtigd zijn om te starten en stoppen van de virtuele machine.
-   Een Azure virtuele machine. We zullen stoppen en starten van deze computer zodat deze productie niet moet worden.

## <a name="step-1---create-new-runbook"></a>Stap 1 - nieuwe runbook maken

We beginnen met het maken van een eenvoudig runbook dat de tekst *Hallo wereld*oplevert.

1.  Open uw account automatisering in de Portal Azure.  
    De pagina automatisering biedt u een snelle weergave van de bronnen in deze account. U moet al bepaalde activa. De meeste van deze zijn de modules die automatisch worden opgenomen in een nieuwe account voor automatisering. Ook hebt u de referentie actief dat wordt vermeld in de [voorwaarden](#prerequisites).
2.  Klik op de tegel **Runbooks** opent de lijst met runbooks.<br> ![Runbooks-besturingselement](media/automation-first-runbook-textual/runbooks-control.png)
3.  Een nieuwe runbook maken door te klikken op de knop **toevoegen, een runbook** en **een nieuwe runbook maken**.
4.  De runbook geeft u de naam *MyFirstRunbook Workflow*.
5.  In dit geval gaan we een [runbook PowerShell Workflow](automation-runbook-types.md#powerShell-workflow-runbooks) maken dus **Powershell Workflow** selecteren voor het **type Runbook**.<br> ![Nieuwe runbook](media/automation-first-runbook-textual/new-runbook.png)
6.  Klik op **maken** om het maken van de runbook en de tekst-editor openen.

## <a name="step-2---add-code-to-the-runbook"></a>Stap 2: Voeg code toe aan de runbook

Kunt u ofwel de code rechtstreeks in de runbook, of u kunt cmdlets, runbooks en elementen uit de bibliotheek-besturingselement selecteren en deze toegevoegd aan de runbook met alle bijbehorende parameters. Voor dit scenario zullen we Typ rechtstreeks in de runbook.

1.  Onze runbook is momenteel leeg met alleen het trefwoord vereist *workflow* , de naam van onze runbook en de accolades die de gehele werkstroom wordt encase. 

    ```
    Workflow MyFirstRunbook-Workflow
    {
    }
    ```

2.  Type *schrijven uitvoer "Hello World."* tussen de accolades. 
   
    ```
    Workflow MyFirstRunbook-Workflow
    {
      Write-Output "Hello World"
    }
    ```

3.  De runbook opslaan door op **Opslaan**te klikken.<br> ![Runbook opslaan](media/automation-first-runbook-textual/runbook-edit-toolbar-save.png)

## <a name="step-3---test-the-runbook"></a>Stap 3: testen van de runbook

Voordat we de runbook zodat deze beschikbaar zijn in de productie kunt publiceren, willen we het testen om ervoor te zorgen dat het goed werkt. Wanneer u een runbook test, kunt u **de conceptversie** uitvoeren en de uitvoer interactief weergeven.

1.  Klik op **testvenster** om de Test-venster te openen.<br> ![Testvenster](media/automation-first-runbook-textual/runbook-edit-toolbar-test-pane.png)
2.  Klik op **Start** om de test te starten. Dit moet de enige optie is ingeschakeld.
3.  Een [runbook taak](automation-runbook-execution.md) wordt gemaakt en wordt de status weergegeven.  
    Status van de taak wordt gestart als *in wachtrij* , die aangeeft dat deze nog voor een werknemer van de runbook in de cloud beschikbaar komen. Deze vervolgens verplaatst naar het *begin* wanneer een werknemer beweert dat de taak en *wordt uitgevoerd* wanneer de runbook daadwerkelijk wordt gestart.  
4.  Wanneer de runbook-taak is voltooid, wordt de uitvoer weergegeven. In ons geval zien we *Hallo wereld*.<br> ![Hallo mensen](media/automation-first-runbook-textual/test-output-hello-world.png)
5.  Sluit u het testvenster om terug te keren naar het canvas.

## <a name="step-4---publish-and-start-the-runbook"></a>Stap 4 - publiceren en starten van de runbook

De runbook die we zojuist hebben gemaakt, is nog steeds in de conceptmodus. We moeten publiceren voordat we kan worden uitgevoerd in de productie. Wanneer u een runbook publiceert, kunt u de bestaande gepubliceerde versie overschrijven met de conceptversie. In ons geval nog we geen een gepubliceerde versie omdat we net hebben de runbook gemaakt.

1.  Klik op **publiceren** op de runbook publiceren en vervolgens op **Ja** wanneer u wordt gevraagd.<br> ![Publiceren](media/automation-first-runbook-textual/runbook-edit-toolbar-publish.png)
2.  Als u de schuifbalk links de runbook in het deelvenster **Runbooks** nu, wordt het weergegeven een **Status ontwerpen** van **gepubliceerd**.
3.  Ga terug naar rechts om het deelvenster voor **MyFirstRunbook Workflow**weer te geven.  
    De opties boven kunnen we de runbook start, plannen te starten op een bepaald moment in de toekomst of een [webhook](automation-webhooks.md) te maken, zodat deze kan worden gestart via een HTTP-oproep.
4.  We willen net start de runbook dus klikt u op **Start** en vervolgens op **Ja** wanneer u wordt gevraagd.<br> ![Runbook starten](media/automation-first-runbook-textual/runbook-toolbar-start.png)
5.  Een deelvenster Project is geopend voor het runbook project dat we zojuist hebben gemaakt. We kunnen dit venster sluiten, maar in dit geval we laten het open zodat we de voortgang van het project kan bekijken.
6.  Status van de taak wordt weergegeven in de **Samenvatting** en komt overeen met de statussen die u hebt gezien wanneer we de runbook getest.<br> ![Samenvatting](media/automation-first-runbook-textual/job-pane-summary.png)
7.  Wanneer de status van de runbook *voltooid wordt*, klikt u op **uitvoer**. Het deelvenster Uitvoer geopend en kunnen we onze *Hallo wereld*zien.<br> ![Samenvatting](media/automation-first-runbook-textual/job-pane-output.png)  
8.  Sluit het deelvenster Uitvoer.
9.  Klik op de **stromen** om het deelvenster Streams voor het runbook-project te openen. We moeten alleen *Hallo wereld* in de uitvoerstroom zien, maar dit andere stromen voor een project van runbook zoals uitgebreid en fout kunt weergeven als de runbook voor hen schrijft.<br> ![Samenvatting](media/automation-first-runbook-textual/job-pane-streams.png)
10. Sluit het deelvenster Streams en het deelvenster Project om terug te keren naar het venster MyFirstRunbook.
11. Klik op **taken** om te openen van het deelvenster Taken voor deze runbook. Hier worden alle taken die door deze runbook gemaakt. Moeten zien we slechts één taak sinds we de taak slechts eenmaal hebt weergegeven.<br> ![Taken](media/automation-first-runbook-textual/runbook-control-jobs.png)
12. U kunt klikken op deze taak opent het deelvenster dezelfde taak die we weergegeven wanneer we begonnen met de runbook. Hiermee kunt u teruggaan in de tijd en de details van een taak die is gemaakt voor een bepaalde runbook weergeven.

## <a name="step-5---add-authentication-to-manage-azure-resources"></a>Stap 5 - verificatie voor het beheren van Azure resources toevoegen

We hebben getest en onze runbook gepubliceerd, maar tot nu toe het doet nog niets nuttig. Wij willen deze Azure bronnen beheren. Niet mogelijk doen maar tenzij wij hebben geverifieerd met behulp van de referenties die worden genoemd in de [voorwaarden](#prerequisites). Dat doen we met de cmdlet **Add-AzureRMAccount** .

1.  De tekstuele editor openen door te klikken op **bewerken** in het deelvenster MyFirstRunbook Workflow.<br> ![Runbook bewerken](media/automation-first-runbook-textual/runbook-toolbar-edit.png)
2.  We de regel **Schrijven uitvoer** niet meer nodig, dus ga verder en verwijder deze.
3.  Plaats de cursor op een lege regel tussen de accolades.
4.  Typ of kopieer en plak de volgende code waarmee de verificatie met uw automatisering uitvoeren als-account worden verwerkt:

    ```
    $Conn = Get-AutomationConnection -Name AzureRunAsConnection 
    Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID `
    -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
    ```

5.  Klik op **venster testen** zodat we de runbook kunt testen.
6.  Klik op **Start** om de test te starten. Zodra deze is voltooid, ontvangt u uitvoer vergelijkbaar met basisinformatie over het volgende, weergave van uw account. Dit bevestigt dat de referentie geldig is.<br> ![Verifiëren](media/automation-first-runbook-textual/runbook-auth-output.png)

## <a name="step-6---add-code-to-start-a-virtual-machine"></a>Stap 6 - code toevoegen om op te starten van een virtuele machine

Nu dat onze runbook tot onze Azure abonnement verifieert, kunt we bronnen beheren. We gaan een opdracht aan een virtuele machine start toevoegen. Kunt u een virtuele machine in uw abonnement Azure en nu we hardcoding die naar de cmdlet naam zult zijn.

1.  Typ na *Add-AzureRmAccount*, *Start AzureRmVM-naam 'VMName' - ResourceGroupName 'NameofResourceGroup'* met de naam en de naam van de resourcegroep van de virtuele machine te starten.  

    ```
    workflow MyFirstRunbook-Workflow
    {
      $Conn = Get-AutomationConnection -Name AzureRunAsConnection
      Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
      Start-AzureRmVM -Name 'VMName' -ResourceGroupName 'ResourceGroupName'
    }
    ``` 

2.  De runbook opslaan en klik op **venster testen** zodat we het kunnen testen.
3.  Klik op **Start** om de test te starten. Zodra deze is voltooid, moet u controleren dat de virtuele machine is gestart.

## <a name="step-7---add-an-input-parameter-to-the-runbook"></a>Stap 7: een invoerparameter toevoegen aan de runbook

Onze runbook op dat moment start de virtuele machine die we hardcoded in de runbook, maar het zou zijn als we de virtuele machine opgeven kunt wanneer de runbook wordt gestart. We zullen nu invoerparameters toevoegen aan de runbook om deze functionaliteit te bieden.

1.  Parameters voor *VMName* en *ResourceGroupName* toevoegen aan de runbook en het gebruik van deze variabelen met de cmdlet **Start AzureRmVM** zoals in het onderstaande voorbeeld. 

    ```
    workflow MyFirstRunbook-Workflow
    {
       Param(
        [string]$VMName,
        [string]$ResourceGroupName
       )  
     $Conn = Get-AutomationConnection -Name AzureRunAsConnection 
     Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
     Start-AzureRmVM -Name $VMName -ResourceGroupName $ResourceGroupName
    }
    ```

2.  De runbook opslaan en openen van de te beproeven ruit. Opmerking u kunt nu waarden opgeven voor de twee ingevoerde variabelen die worden gebruikt in de test.
3.  De te beproeven ruit sluit.
4.  Klik op **publiceren** om de nieuwe versie van de runbook te publiceren.
5.  Stop de virtuele machine die u in de vorige stap hebt gestart.
6.  Klik op **Start** om te beginnen de runbook. Typ in het **VMName** en **ResourceGroupName** voor de virtuele machine die u wilt starten.<br> ![Runbook starten](media/automation-first-runbook-textual/automation-pass-params.png)

7.  Wanneer de runbook is voltooid, kunt u controleren of de virtuele machine is gestart.

## <a name="next-steps"></a>Volgende stappen

-  Om te beginnen met een grafische runbooks, Zie [Mijn eerste grafische runbook](automation-first-runbook-graphical.md)
-  Om te beginnen met PowerShell runbooks, Zie [Mijn eerste PowerShell-runbook](automation-first-runbook-textual-powershell.md)
-  Zie voor meer informatie over de typen runbook, hun voordelen en beperkingen, [Azure automatisering runbook typen](automation-runbook-types.md)
-  Voor meer informatie over PowerShell script functie ondersteunen, Zie [Native PowerShell script in Azure automatisering ondersteunen](https://azure.microsoft.com/blog/announcing-powershell-script-support-azure-automation-2/)