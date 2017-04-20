<properties 
    pageTitle="Bewaken en beheren van taken met PowerShell Stream Analytics | Microsoft Azure" 
    description="Informatie over het Azure PowerShell en -cmdlets gebruiken om te controleren en beheren van taken Stream Analytics." 
    keywords="Azure powershell, azure powershell-cmdlets, powershell-opdracht powershell-scripts" 
    services="stream-analytics" 
    documentationCenter="" 
    authors="jeffstokes72" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="stream-analytics" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="09/26/2016" 
    ms.author="jeffstok"/>


# <a name="monitor-and-manage-stream-analytics-jobs-with-azure-powershell-cmdlets"></a>Bewaken en beheren van taken met Azure PowerShell-cmdlets Stream Analytics

Informatie over het controleren en beheren van resources, Stream Analytics met Azure PowerShell-cmdlets en powershell-scripts die stroom Analytics basistaken uitvoeren.

## <a name="prerequisites-for-running-azure-powershell-cmdlets-for-stream-analytics"></a>Vereisten voor het uitvoeren van Azure PowerShell-cmdlets voor Stream Analytics

 - Maak een brongroep Azure in uw abonnement. Hier volgt een voorbeeld van Azure PowerShell script. Zie voor informatie Azure PowerShell [installeren en configureren van Azure PowerShell](../powershell-install-configure.md);  

Azure PowerShell 0.9.8:  

        # Log in to your Azure account
        Add-AzureAccount

        # Select the Azure subscription you want to use to create the resource group if you have more than one subscription on your account.
        Select-AzureSubscription -SubscriptionName <subscription name>
 
        # If Stream Analytics has not been registered to the subscription, remove remark symbol below (#) to run the Register-AzureProvider cmdlet to register the provider namespace.
        #Register-AzureProvider -Force -ProviderNamespace 'Microsoft.StreamAnalytics'

        # Create an Azure resource group
        New-AzureResourceGroup -Name <YOUR RESOURCE GROUP NAME> -Location <LOCATION>

Azure PowerShell 1.0:  

        # Log in to your Azure account
        Login-AzureRmAccount

        # Select the Azure subscription you want to use to create the resource group.
        Get-AzureRmSubscription –SubscriptionName “your sub” | Select-AzureRmSubscription

        # If Stream Analytics has not been registered to the subscription, remove remark symbol below (#) to run the Register-AzureProvider cmdlet to register the provider namespace.
        #Register-AzureRmResourceProvider -Force -ProviderNamespace 'Microsoft.StreamAnalytics'
        
        # Create an Azure resource group
        New-AzureRMResourceGroup -Name <YOUR RESOURCE GROUP NAME> -Location <LOCATION>
        


> [AZURE.NOTE] Stream Analytics taken programmatisch gemaakt geen controle standaard ingeschakeld.  U kunt handmatig inschakelen in de Portal Azure controleren door te navigeren naar de pagina van de Monitor van de taak en klikt u op de knop inschakelen of u kunt dit programmatisch doen door de stappen te vinden op [Azure Stream Analytics - Monitor Stream Analytics projecten via programmacode](stream-analytics-monitor-jobs.md).

## <a name="azure-powershell-cmdlets-for-stream-analytics"></a>Azure PowerShell-cmdlets voor Stream Analytics
De volgende Azure PowerShell cmdlets kunt bewaken en beheren van Azure Stream Analytics taken worden gebruikt. Houd er rekening mee dat Azure PowerShell verschillende versies heeft. 
**In de voorbeelden die de eerste opdracht is voor Azure PowerShell 0.9.8, de tweede opdracht is voor Azure PowerShell 1.0.** De opdrachten Azure PowerShell 1.0 hebben altijd 'AzureRM' in de opdracht.

### <a name="get-azurestreamanalyticsjob--get-azurermstreamanalyticsjob"></a>Get-AzureStreamAnalyticsJob | Get-AzureRMStreamAnalyticsJob
Geeft een overzicht van alle stroom Analytics taken gedefinieerd in de opgegeven resourcegroep of Azure abonnement of taakinformatie ophalen over een specifieke taak binnen een resourcegroep.

**Voorbeeld 1**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsJob

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsJob

Deze PowerShell-opdracht retourneert informatie over alle projecten in de Stream Analytics in het abonnement op Azure.

**Voorbeeld 2**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US 

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US 

Deze PowerShell-opdracht retourneert informatie over alle projecten in de Stream Analytics in de resourcegroep StreamAnalytics standaard-centrale VS.

**Voorbeeld 3**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob

Deze PowerShell-opdracht retourneert informatie over de Stream Analytics taak StreamingJob in de resourcegroep StreamAnalytics standaard-centrale VS.

### <a name="get-azurestreamanalyticsinput--get-azurermstreamanalyticsinput"></a>Get-AzureStreamAnalyticsInput | Get-AzureRMStreamAnalyticsInput
Alle van de "inputs" die zijn gedefinieerd in een opgegeven Stream Analytics taak of haalt informatie over een bepaalde invoer.

**Voorbeeld 1**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob

Deze PowerShell-opdracht retourneert informatie over de invoer die zijn gedefinieerd in de StreamingJob van de taak.

**Voorbeeld 2**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name EntryStream

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name EntryStream

Deze PowerShell-opdracht retourneert informatie over de invoer met de naam EntryStream die is gedefinieerd in de StreamingJob van de taak.

### <a name="get-azurestreamanalyticsoutput--get-azurermstreamanalyticsoutput"></a>Get-AzureStreamAnalyticsOutput | Get-AzureRMStreamAnalyticsOutput
Alle van de resultaten die zijn gedefinieerd in een opgegeven Stream Analytics taak of haalt informatie over een specifieke output.

**Voorbeeld 1**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob

Deze PowerShell-opdracht retourneert informatie over de resultaten die zijn gedefinieerd in de StreamingJob van de taak.

**Voorbeeld 2**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name Output

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name Output

Deze PowerShell-opdracht retourneert informatie over de uitvoer die zijn gedefinieerd in de StreamingJob van de taak met de naam uitvoer.

### <a name="get-azurestreamanalyticsquota--get-azurermstreamanalyticsquota"></a>Get-AzureStreamAnalyticsQuota | Get-AzureRMStreamAnalyticsQuota
Informatie ophalen over het quotum van streaming eenheden in een bepaald gebied.

**Voorbeeld 1**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsQuota –Location "Central US" 

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsQuota –Location "Central US" 

Deze PowerShell-opdracht retourneert informatie over de quota en het gebruik van streaming eenheden in de centraal-Amerikaanse regio.

### <a name="get-azurestreamanalyticstransformation--getazurermstreamanalyticstransformation"></a>Get-AzureStreamAnalyticsTransformation | GetAzureRMStreamAnalyticsTransformation
Informatie ophalen over de gewenste transformatie in een Stream Analytics project zijn gedefinieerd.

**Voorbeeld 1**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name StreamingJob

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name StreamingJob

Deze PowerShell-opdracht retourneert informatie over de transformatie van StreamingJob in de functie StreamingJob aangeroepen.

### <a name="new-azurestreamanalyticsinput--new-azurermstreamanalyticsinput"></a>Nieuw AzureStreamAnalyticsInput | Nieuwe AzureRMStreamAnalyticsInput
Hiermee maakt u een nieuwe invoer binnen een project Stream Analytics of updates van een bestaande opgegeven ingang.
  
De naam van de invoer kan worden opgegeven in het bestand .json of op de opdrachtregel. Als beide zijn opgegeven, moet dezelfde is als die in het bestand de naam op de opdrachtregel zijn.

Als u invoer die al opgeven en geef de – parameter Force de cmdlet wordt gevraagd al dan niet ter vervanging van de bestaande invoer.

Als u de – gedwongen parameter en geeft u een bestaande naam invoert, wordt de invoer vervangen zonder bevestiging.

Raadpleeg voor gedetailleerde informatie over de JSON-bestandsstructuur en inhoud [Maken Input (Analytics Azure Stream)] [ msdn-rest-api-create-stream-analytics-input] sectie van de [Stream Analytics beheer REST API Reference Library][stream.analytics.rest.api.reference].

**Voorbeeld 1**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" 

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" 

Deze PowerShell-opdracht maakt een nieuwe invoer uit het bestand Input.json. Als een bestaande invoer met de naam die is opgegeven in het definitiebestand invoer al is gedefinieerd, vraagt de cmdlet wel of niet vervangen.

**Voorbeeld 2**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream

Deze opdracht PowerShell maakt een nieuwe invoer in de functie EntryStream aangeroepen. Als een bestaande invoer met deze naam al is gedefinieerd, vraagt de cmdlet wel of niet vervangen.

**Voorbeeld 3**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream -Force

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream -Force

Deze PowerShell-opdracht vervangt u de definitie van de bestaande invoerbron EntryStream aangeroepen met de definitie van het bestand.

### <a name="new-azurestreamanalyticsjob--new-azurermstreamanalyticsjob"></a>Nieuw AzureStreamAnalyticsJob | Nieuwe AzureRMStreamAnalyticsJob
Maakt een nieuwe Analytics Stream-project in Microsoft Azure of de definitie van een bestaande taak bijgewerkt.

De naam van de taak kan worden opgegeven in het bestand .json of op de opdrachtregel. Als beide zijn opgegeven, moet dezelfde is als die in het bestand de naam op de opdrachtregel zijn.

Als u een taaknaam opgeven die al bestaat en geef de – parameter Force de cmdlet wordt gevraagd al dan niet ter vervanging van de bestaande taak.

Als u de – Force parameter en geeft u de taaknaam van een bestaande, de definitie zonder bevestiging vervangen.

Raadpleeg voor gedetailleerde informatie over de JSON-bestandsstructuur en inhoud, de [Stream Analytics taak maken] [ msdn-rest-api-create-stream-analytics-job] sectie van de [Stream Analytics beheer REST API Reference Library][stream.analytics.rest.api.reference].

**Voorbeeld 1**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" 

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" 

Deze opdracht PowerShell maakt een nieuwe taak van de definitie in JobDefinition.json. Als u een bestaand project met de naam die is opgegeven in het taakdefinitiebestand al is gedefinieerd, vraagt de cmdlet wel of niet vervangen.

**Voorbeeld 2**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" –Name StreamingJob -Force

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" –Name StreamingJob -Force

Met deze opdracht PowerShell vervangt de taakdefinitie voor StreamingJob.

### <a name="new-azurestreamanalyticsoutput--new-azurermstreamanalyticsoutput"></a>Nieuw AzureStreamAnalyticsOutput | Nieuwe AzureRMStreamAnalyticsOutput
Hiermee maakt u een nieuwe uitvoer binnen een project Stream Analytics of updates van een bestaande uitvoer.  

De naam van de uitvoer kan worden opgegeven in het bestand .json of op de opdrachtregel. Als beide zijn opgegeven, moet dezelfde is als die in het bestand de naam op de opdrachtregel zijn.

Als u een uitvoer die al opgeven en geef de – parameter Force de cmdlet wordt gevraagd al dan niet ter vervanging van de bestaande uitvoer.

Als u de – gedwongen parameter en geef de naam van een bestaande uitvoer, wordt de uitvoer zonder bevestiging vervangen.

Raadpleeg voor gedetailleerde informatie over de JSON-bestandsstructuur en inhoud, de [Uitvoer maken (Analytics Azure Stream)] [ msdn-rest-api-create-stream-analytics-output] sectie van de [Stream Analytics beheer REST API Reference Library][stream.analytics.rest.api.reference].

**Voorbeeld 1**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output

Deze opdracht PowerShell maakt een nieuwe uitvoer "uitvoer" genoemd in de StreamingJob van de taak. Als een bestaande uitvoer met deze naam al is gedefinieerd, vraagt de cmdlet wel of niet vervangen.

**Voorbeeld 2**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output -Force

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output -Force

Deze PowerShell-opdracht vervangt de definitie voor 'uitvoer' in de StreamingJob van de taak.

### <a name="new-azurestreamanalyticstransformation--new-azurermstreamanalyticstransformation"></a>Nieuw AzureStreamAnalyticsTransformation | Nieuwe AzureRMStreamAnalyticsTransformation
Hiermee maakt u een nieuwe transformatie binnen een project Stream Analytics of werkt u de bestaande transformatie.
  
De naam van de transformatie kan worden opgegeven in het bestand .json of op de opdrachtregel. Als beide zijn opgegeven, moet dezelfde is als die in het bestand de naam op de opdrachtregel zijn.

Als u een transformatie die al opgeven en geef de – parameter Force de cmdlet wordt gevraagd al dan niet ter vervanging van de bestaande transformatie.

Als u de – Force parameter en geef de naam van een bestaande transformatie, de transformatie wordt zonder bevestiging vervangen.

Raadpleeg voor gedetailleerde informatie over de JSON-bestandsstructuur en inhoud, de [Transformatie maken (Analytics Azure Stream)] [ msdn-rest-api-create-stream-analytics-transformation] sectie van de [Stream Analytics beheer REST API Reference Library][stream.analytics.rest.api.reference].

**Voorbeeld 1**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform

Deze PowerShell-opdracht maakt u een nieuwe transformatie genaamd StreamingJobTransform in de StreamingJob van de taak. Als een bestaande transformatie al met deze naam is gedefinieerd, vraagt de cmdlet wel of niet vervangen.

**Voorbeeld 2**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform -Force

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform -Force

 Deze PowerShell-opdracht vervangt u de definitie van StreamingJobTransform in de StreamingJob van de taak.

### <a name="remove-azurestreamanalyticsinput--remove-azurermstreamanalyticsinput"></a>Verwijderen AzureStreamAnalyticsInput | Verwijderen AzureRMStreamAnalyticsInput
Verwijdert een bepaalde invoer asynchroon uit een Analytics Stream-project in Microsoft Azure.  
Als u de-parameter Force de invoer zonder bevestiging worden verwijderd.

**Voorbeeld 1**

Azure PowerShell 0.9.8:  

    Remove-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EventStream

Azure PowerShell 1.0:  

    Remove-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EventStream

Deze PowerShell-opdracht verwijdert u de invoer EventStream in de StreamingJob van de taak.  

### <a name="remove-azurestreamanalyticsjob--remove-azurermstreamanalyticsjob"></a>Verwijderen AzureStreamAnalyticsJob | Verwijderen AzureRMStreamAnalyticsJob
Asynchroon verwijdert een bepaalde taak Stream Analytics in Microsoft Azure.  
Als u de – Force parameter, wordt de taak verwijderd zonder bevestiging.

**Voorbeeld 1**

Azure PowerShell 0.9.8:  

    Remove-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 

Azure PowerShell 1.0:  

    Remove-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 

Deze PowerShell-opdracht verwijdert u de taak StreamingJob.  

### <a name="remove-azurestreamanalyticsoutput--remove-azurermstreamanalyticsoutput"></a>Verwijderen AzureStreamAnalyticsOutput | Verwijderen AzureRMStreamAnalyticsOutput
Asynchroon Hiermee verwijdert u een specifieke output Stream Analytics taak in Microsoft Azure.  
Als u de-parameter Force de uitvoer worden verwijderd zonder bevestiging.

**Voorbeeld 1**

Azure PowerShell 0.9.8:  

    Remove-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output

Azure PowerShell 1.0:  

    Remove-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output

Deze opdracht PowerShell verwijdert de uitvoer Output in het project StreamingJob.  

### <a name="start-azurestreamanalyticsjob--start-azurermstreamanalyticsjob"></a>Start AzureStreamAnalyticsJob | Start AzureRMStreamAnalyticsJob
Asynchroon wordt uitgevoerd, implementeert en begint een taak Stream Analytics in Microsoft Azure.

**Voorbeeld 1**

Azure PowerShell 0.9.8:  

    Start-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob -OutputStartMode CustomTime -OutputStartTime 2012-12-12T12:12:12Z

Azure PowerShell 1.0:  

    Start-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob -OutputStartMode CustomTime -OutputStartTime 2012-12-12T12:12:12Z

Deze PowerShell-opdracht start de taak StreamingJob met een begintijd van aangepaste uitvoer ingesteld op 12 December 2012, 12:12:12 UTC.


### <a name="stop-azurestreamanalyticsjob--stop-azurermstreamanalyticsjob"></a>Stop AzureStreamAnalyticsJob | Stop AzureRMStreamAnalyticsJob
Asynchroon stopt een Stream Analytics taak uit te voeren in Microsoft Azure en opgeheven toegewezen middelen die waren die zouden worden gebruikt. De taakdefinitie en de metagegevens blijven beschikbaar binnen uw abonnement via de portal Azure en de beheer-API's, zodat de taak kan worden bewerkt en opnieuw gestart. U wordt niet afgeschreven voor een taak is gestopt.

**Voorbeeld 1**

Azure PowerShell 0.9.8:  

    Stop-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 

Azure PowerShell 1.0:  

    Stop-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 

Deze PowerShell-opdracht stopt u de taak StreamingJob.  

### <a name="test-azurestreamanalyticsinput--test-azurermstreamanalyticsinput"></a>Test-AzureStreamAnalyticsInput | Test AzureRMStreamAnalyticsInput
Test de mogelijkheid van Analytics Stream verbinding maken met een opgegeven ingang.

**Voorbeeld 1**

Azure PowerShell 0.9.8:  

    Test-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EntryStream

Azure PowerShell 1.0:  

    Test-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EntryStream

Deze PowerShell-opdracht test de verbindingsstatus van de ingevoerde EntryStream in StreamingJob.  

###<a name="test-azurestreamanalyticsoutput--test-azurermstreamanalyticsoutput"></a>Test-AzureStreamAnalyticsOutput | Test AzureRMStreamAnalyticsOutput
De mogelijkheid van Analytics Stream verbinding maken met de uitvoer van een opgegeven test.

**Voorbeeld 1**

Azure PowerShell 0.9.8:  

    Test-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output

Azure PowerShell 1.0:  

    Test-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output

Deze opdracht PowerShell tests de verbindingsstatus van de productie-Output in StreamingJob.  

## <a name="get-support"></a>Ondersteuning krijgen
Probeer onze [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)voor verdere ondersteuning. 


## <a name="next-steps"></a>Volgende stappen

- [Inleiding tot Analytics Azure Stream](stream-analytics-introduction.md)
- [Aan de slag met Azure Stream Analytics](stream-analytics-get-started.md)
- [Schaal Azure Stream Analytics taken](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics beheer REST API: naslag](https://msdn.microsoft.com/library/azure/dn835031.aspx)
 



[msdn-switch-azuremode]: http://msdn.microsoft.com/library/dn722470.aspx
[powershell-install]: http://azure.microsoft.com/documentation/articles/powershell-install-configure/
[msdn-rest-api-create-stream-analytics-job]: https://msdn.microsoft.com/library/dn834994.aspx
[msdn-rest-api-create-stream-analytics-input]: https://msdn.microsoft.com/library/dn835010.aspx
[msdn-rest-api-create-stream-analytics-output]: https://msdn.microsoft.com/library/dn835015.aspx
[msdn-rest-api-create-stream-analytics-transformation]: https://msdn.microsoft.com/library/dn835007.aspx

[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-get-started.md
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
 
