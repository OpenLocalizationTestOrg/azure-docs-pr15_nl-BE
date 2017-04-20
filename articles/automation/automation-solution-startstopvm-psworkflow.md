<properties 
    pageTitle="Starten en stoppen van de virtuele machines met Azure automatisering - PowerShell Workflow | Microsoft Azure"
    description="Grafische versie van Azure automatisering scenario, met inbegrip van de runbooks te starten en stoppen van klassieke virtuele machines."
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
    ms.date="07/06/2016"
    ms.author="bwren" />

# <a name="azure-automation-scenario---starting-and-stopping-virtual-machines"></a>Azure scenario voor automatisering - starten en stoppen van virtuele machines

In dit scenario voor automatisering Azure bevat runbooks om te starten en stoppen van klassieke virtuele machines.  U kunt dit scenario voor het volgende gebruiken:  

- Gebruik het runbooks zonder wijziging in uw eigen omgeving. 
- Wijzig de runbooks voor het uitvoeren van aangepaste functionaliteit.  
- De runbooks aanroepen vanuit een andere runbook als onderdeel van een algemene oplossing. 
- De runbooks als zelfstudie gebruiken voor meer informatie over authoring concepten runbook. 

> [AZURE.SELECTOR]
- [Grafische](automation-solution-startstopvm-graphical.md)
- [PowerShell Workflow](automation-solution-startstopvm-psworkflow.md)

Dit is de werkstroom PowerShell runbook versie van dit scenario. Het is ook beschikbaar met behulp van [grafische runbooks](automation-solution-startstopvm-graphical.md).

## <a name="getting-the-scenario"></a>Ophalen van het scenario

In dit scenario bestaat uit twee PowerShell Workflow runbooks die u van de volgende koppelingen downloaden kunt.  Zie de [grafische versie](automation-solution-startstopvm-graphical.md) van dit scenario voor koppelingen naar de grafische runbooks.

| Runbook | Koppeling | Type | Beschrijving |
|:---|:---|:---|:---|
| Start AzureVMs | [Azure klassieke VMs starten](https://gallery.technet.microsoft.com/Start-Azure-Classic-VMs-86ef746b) | PowerShell Workflow | Start alle klassieke virtuele machines in een Azure subscriptionor alle virtuele machines met de naam van een bepaalde service. |
| Stop AzureVMs | [Azure klassieke VMs stoppen](https://gallery.technet.microsoft.com/Stop-Azure-Classic-VMs-7a4ae43e) | PowerShell Workflow | Hiermee stopt u alle virtuele machines in een automatisering-account of alle virtuele machines met de naam van een bepaalde service.  |


## <a name="installing-and-configuring-the-scenario"></a>Installeren en configureren van het scenario

### <a name="1-install-the-runbooks"></a>1. Installeer de runbooks

Na het downloaden van de runbooks, kunt u deze met de procedure in [een Runbook importeren](http://msdn.microsoft.com/library/dn643637.aspx#ImportRunbook)importeren.

### <a name="2-review-the-description-and-requirements"></a>2. Bekijk de beschrijving en de vereisten
De runbooks bevatten commentaar help-tekst met een beschrijving en de vereiste elementen.  U kunt dezelfde informatie ook opvragen uit dit artikel. 

### <a name="3-configure-assets"></a>3. activa configureren
De runbooks moeten de volgende elementen die u moet maken en vullen met de juiste waarden.

| Activatype | Activumnaam | Beschrijving |
|:---|:---|:---|:---|
| Referentie | AzureCredential | Referenties voor een account die is gemachtigd om te starten en stoppen van virtuele machines in de Azure-abonnement bevat.  Ook kunt u een andere referentie actief in de **referentie** -parameter van de activiteit **Toevoegen AzureAccount** . |
| Variabele | AzureSubscriptionId | Bevat de abonnement-ID van uw abonnement Azure. |

## <a name="using-the-scenario"></a>Met behulp van het scenario

### <a name="parameters"></a>Parameters

De runbooks hebben de volgende parameters.  U moet waarden opgeven voor de verplichte parameters en u kunt desgewenst waarden voor andere parameters afhankelijk van uw vereisten.

| Parameter | Type | Verplicht | Beschrijving |
|:---|:---|:---|:---|
| Servicenaam | tekenreeks | Nee | Als een waarde is opgegeven, worden alle virtuele machines met die servicenaam zijn gestart of gestopt.  Als geen waarde is opgegeven, worden alle klassieke virtuele machines in de Azure abonnement de gestart of gestopt. |
| AzureSubscriptionIdAssetName | tekenreeks | Nee | Bevat de naam van de [variabele activa](#installing-and-configuring-the-scenario) met de abonnement-ID van uw abonnement op Azure.  Als u geen waarde opgeeft, wordt *AzureSubscriptionId* gebruikt.  |
| AzureCredentialAssetName | tekenreeks | Nee | Bevat de naam van de [referentie actief](#installing-and-configuring-the-scenario) met de referenties voor de runbook te gebruiken.  Als u geen waarde opgeeft, wordt *AzureCredential* gebruikt.  |

### <a name="starting-the-runbooks"></a>De runbooks starten

U kunt een van de methoden bij het [starten van een runbook in Azure automatisering](automation-starting-a-runbook.md) start een van de runbooks in dit scenario.

De volgende voorbeeldopdrachten gebruikt Windows PowerShell voor het uitvoeren van **StartAzureVMs** om te beginnen alle virtuele machines met de naam *MyVMService*.

    $params = @{"ServiceName"="MyVMService"}
    Start-AzureAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Start-AzureVMs" –Parameters $params

### <a name="output"></a>Uitvoer

De runbooks wordt [uitvoer van een bericht](automation-runbook-output-and-messages.md) voor elke virtuele machine die aangeeft of er met de instructie starten of stoppen met succes is ingediend.  U kunt zoeken naar een bepaalde tekenreeks in de uitvoer van het resultaat voor elke runbook bepalen.  De mogelijke uitvoertekenreeksen worden in de volgende tabel weergegeven.

| Runbook | Voorwaarde | Bericht |
|:---|:---|:---|
| Start AzureVMs | Virtuele machine wordt al uitgevoerd.  | MyVM wordt al uitgevoerd. |
| Start AzureVMs | Start-aanvraag voor virtuele machine is verzonden | MyVM is gestart |
| Start AzureVMs | Startaanvraag voor virtuele machine is mislukt  | MyVM kan niet worden gestart |
| Stop AzureVMs | Virtuele machine is al gestopt.  | MyVM is al gestopt. |
| Stop AzureVMs | Stop-aanvraag voor virtuele machine is verzonden | MyVM is gestopt |
| Stop AzureVMs | Stop-aanvraag voor virtuele machine is mislukt  | MyVM kan niet stoppen. |

Het volgende codefragment uit een runbook probeert alle virtuele machines beginnen met de naam *MyServiceName*.  Als een van de aanvragen start mislukt, kunnen acties worden genomen. 

    $results = Start-AzureVMs -ServiceName "MyServiceName"
    foreach ($result in $results) {
        if ($result -like "* has been started" ) {
            # Action to take in case of success.
        }
        else {
            # Action to take in case of error.
        }
    }


## <a name="detailed-breakdown"></a>In detail

Hieronder vindt u een gedetailleerde uitsplitsing van de runbooks in dit scenario.  U kunt deze gegevens gewoon meer van hen voor het ontwerpen van uw eigen automatiseringsscenario's of de runbooks aanpassen.

### <a name="parameters"></a>Parameters

    param (
        [Parameter(Mandatory=$false)] 
        [String]  $AzureCredentialAssetName = 'AzureCredential',
        
        [Parameter(Mandatory=$false)]
        [String] $AzureSubscriptionIdAssetName = 'AzureSubscriptionId',

        [Parameter(Mandatory=$false)] 
        [String] $ServiceName
    )

De werkstroom wordt gestart door de waarden voor de [Invoerparameters](#using-the-scenario).  Standaardnamen worden gebruikt als de elementnamen worden opgegeven.

### <a name="output"></a>Uitvoer

    # Returns strings with status messages
    [OutputType([String])]

Deze regel verklaart dat de uitvoer van de runbook zal een tekenreeks.  Dit is niet vereist, maar wordt aanbevolen voor wanneer de runbook wordt gebruikt als een [onderliggende runbook](automation-child-runbooks.md) zodat een bovenliggende runbook weten het uitvoertype kan worden verwacht.

### <a name="authentication"></a>Verificatie

    # Connect to Azure and select the subscription to work against
    $Cred = Get-AutomationPSCredential -Name $AzureCredentialAssetName
    $null = Add-AzureAccount -Credential $Cred -ErrorAction Stop
    $SubId = Get-AutomationVariable -Name $AzureSubscriptionIdAssetName
    $null = Select-AzureSubscription -SubscriptionId $SubId -ErrorAction Stop

De volgende regels instellen voor [referenties](automation-configuring.md#configuring-authentication-to-azure-resources) en Azure abonnement die wordt gebruikt voor de rest van de runbook.
Eerst gebruiken we **Get-AutomationPSCredential** om de activa die in het bezit van de referenties die toegang hebben tot het starten en stoppen van virtuele machines in de Azure abonnement. **Add-AzureAccount** wordt dit activum referenties instellen.  De uitvoer is toegewezen aan een dummy-variabele, zodat het niet wordt opgenomen in de uitvoer van de runbook.  

De variabele activa met de abonnement-ID opgehaald met **Get-AutomationVariable** en het abonnement met **Select AzureSubscription**instellen.

### <a name="get-vms"></a>VMs ophalen

    # If there is a specific cloud service, then get all VMs in the service,
    # otherwise get all VMs in the subscription.
    if ($ServiceName) 
    { 
        $VMs = Get-AzureVM -ServiceName $ServiceName
    }
    else 
    { 
        $VMs = Get-AzureVM
    }

**Get-AzureVM** wordt gebruikt voor het ophalen van de virtuele machines die met de runbook werken.  Als geen waarde is opgegeven in de invoervariabele **servicenaam** , worden alleen de virtuele machines met die servicenaam opgehaald.  Als **servicenaam** leeg is, worden alle virtuele machines opgehaald.

### <a name="startstop-virtual-machines-and-send-output"></a>Starten en stoppen van virtuele machines en de uitvoer verzenden

    # Start each of the stopped VMs
    foreach ($VM in $VMs)
    {
        if ($VM.PowerState -eq "Started")
        {
            # The VM is already started, so send notice
            Write-Output ($VM.InstanceName + " is already running")
        }
        else
        {
            # The VM needs to be started
            $StartRtn = Start-AzureVM -Name $VM.Name -ServiceName $VM.ServiceName -ErrorAction Continue

            if ($StartRtn.OperationStatus -ne 'Succeeded')
            {
                # The VM failed to start, so send notice
                Write-Output ($VM.InstanceName + " failed to start")
            }
            else
            {
                # The VM started, so send notice
                Write-Output ($VM.InstanceName + " has been started")
            }
        }
    }

De volgende regels doorlopen elke virtuele machine.  De **PowerState** van de virtuele machine wordt eerst gecontroleerd om te zien als deze is gestart of gestopt, afhankelijk van de runbook.  Als het al in het doel staat, vervolgens een bericht verzonden naar de uitvoer en de uiteinden van de runbook.  Als dit niet het geval is, wordt vervolgens **Start AzureVM** of **Stop AzureVM** wordt gebruikt om te proberen te starten of stoppen van de virtuele machine met het resultaat van de aanvraag aan een variabele opgeslagen.  Een bericht wordt vervolgens verzonden naar uitvoer opgeven of het verzoek om te starten of stoppen met succes is ingediend.


## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over het werken met de onderliggende runbooks, [runbooks kind in Azure automatisering](automation-child-runbooks.md) 
- Zie voor meer informatie over de Uitvoerberichten tijdens de registratie voor het oplossen en uitvoeren van runbook, [Runbook-uitvoer en berichten in Azure automatisering](automation-runbook-output-and-messages.md)
