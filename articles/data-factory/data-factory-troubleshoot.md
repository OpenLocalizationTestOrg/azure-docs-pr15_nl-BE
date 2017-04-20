<properties 
    pageTitle="Problemen met Azure Data Factory" 
    description="Informatie over het oplossen van problemen met het gebruik van Azure Data Factory." 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/31/2016" 
    ms.author="spelluru"/>

# <a name="troubleshoot-data-factory-issues"></a>Problemen met Data Factory
Dit artikel bevat tips voor probleemoplossing voor problemen bij het gebruik van Azure Data Factory. In dit artikel de lijst bevat niet alle mogelijke problemen bij het gebruik van de service, maar enkele algemene tips voor probleemoplossing en dekt.   

## <a name="troubleshooting-tips"></a>Tips voor probleemoplossing

### <a name="error-the-subscription-is-not-registered-to-use-namespace-microsoftdatafactory"></a>Fout: Het abonnement is niet geregistreerd voor het gebruik van 'Microsoft.DataFactory' naamruimte
Als u dit foutbericht ontvangt, is de resource Azure Data Factory provider niet geregistreerd op uw computer. Voer de volgende handelingen uit: 

1. Azure PowerShell starten. 
2. Log in op uw Azure rekening met de volgende opdracht.
        Inloggen AzureRmAccount 
3. Voer de volgende opdracht voor het registreren van de provider Azure Data Factory.
        Register-AzureRmResourceProvider - ProviderNamespace Microsoft.DataFactory

### <a name="problem-unauthorized-error-when-running-a-data-factory-cmdlet"></a>Probleem: Niet-geautoriseerde fout bij het uitvoeren van een cmdlet Data Factory
U werkt waarschijnlijk niet rechts Azure-account of -abonnement met de Azure PowerShell. Gebruik de volgende cmdlets om de rechts Azure-account en abonnement gebruiken met de Azure PowerShell. 

1. Login-AzureRmAccount - gebruik de juiste gebruikersnaam en wachtwoord
2. Get-AzureRmSubscription - alle abonnementen voor de rekening weergeven. 
3. Selecteer AzureRmSubscription &lt;de naam van abonnement&gt; -Selecteer het juiste abonnement. Gebruik dezelfde die u gebruikt voor het maken van een fabriek gegevens op de portal Azure.

### <a name="problem-fail-to-launch-data-management-gateway-express-setup-from-azure-portal"></a>Probleem: Geen Data Management Gateway Express Setup starten vanuit Azure portal
De Express-installatie voor de Data Management Gateway vereist Internet Explorer of een compatibel ClickOnce Microsoft-webbrowser. Als de Express-installatie niet kunt starten, kunt u het volgende doen: 

- Gebruik Internet Explorer of een compatibel ClickOnce Microsoft-webbrowser.

    Als u chroom gebruikt, gaat u aan de [Chrome web store](https://chrome.google.com/webstore/)zoeken met het trefwoord 'ClickOnce', kies een van de extensies ClickOnce en installeren. 
    
    Doe hetzelfde voor de Firefox (installatie-invoegtoepassing). Klik op de knop Menu openen op de werkbalk (drie horizontale lijnen in de rechterbovenhoek), klik op invoegtoepassingen met 'ClickOnce' trefwoord zoeken, kiest u een van de extensies ClickOnce en installeren. 

- Gebruik de **Handmatige installatie** koppeling weergegeven op het blad dezelfde in de portal. U kunt deze benadering installatiebestand downloaden en handmatig uitvoeren. Nadat de installatie voltooid is, ziet u in het dialoogvenster configuratie van Data Management Gateway. De **sleutel** van het portal-scherm kopiëren en gebruiken in de configuration manager de gateway handmatig registreren met de service.  

### <a name="problem-fail-to-connect-to-on-premises-sql-server"></a>Probleem: Geen verbinding met de SQL-Server voor gebouwen 
**Data Management Gateway Configuration Manager** starten op de gateway-computer en test de verbinding met SQL Server van de gateway-computer met het tabblad **Probleemoplossing** . Zie [problemen oplossen gateway](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) voor tips over het oplossen van verbinding/gateway verband houdende vraagstukken.   
 

### <a name="problem-input-slices-are-in-waiting-state-for-ever"></a>Probleem: Input segmenten zijn in staat om ooit nog

De segmenten kunnen niet **wachten** om verscheidene redenen. Een belangrijke reden is dat de eigenschap **external** niet is ingesteld op **true**. Een dataset die buiten het bereik van Azure Data Factory wordt geproduceerd, moet worden gemarkeerd met de eigenschap **external** . Deze eigenschap geeft aan dat de gegevens niet door pijplijnen binnen de fabriek gegevens back-ups en externe. De segmenten van de gegevens worden gemarkeerd als **Gereed** zodra de gegevens beschikbaar in de betreffende winkel zijn. 

Zie het volgende voorbeeld voor het gebruik van de eigenschap **external** . Desgewenst kunt u **externalData**opgeven * wanneer u externe op true instelt.

Zie [Datasets](data-factory-create-datasets.md) artikel voor meer informatie over deze eigenschap.
    
    {
      "name": "CustomerTable",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "MyLinkedService",
        "typeProperties": {
          "folderPath": "MyContainer/MySubFolder/",
          "format": {
            "type": "TextFormat",
            "columnDelimiter": ",",
            "rowDelimiter": ";"
          }
        },
        "external": true,
        "availability": {
          "frequency": "Hour",
          "interval": 1
        },
        "policy": {
          }
        }
      }
    }

De fout is opgelost, de eigenschap **external** en de **externalData** van de optionele sectie toevoegen aan de JSON-definitie van de tabel invoer en de tabel opnieuw te maken. 

### <a name="problem-hybrid-copy-operation-fails"></a>Probleem: Hybride kopieerbewerking mislukt
Zie [problemen met gateway](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) voor stappen voor het oplossen van problemen met het kopiëren van een gegevensarchief voor op ruimten met behulp van de Data Management Gateway. 

### <a name="problem-on-demand-hdinsight-provisioning-fails"></a>Probleem: On-demand HDInsight inrichten mislukt
Wanneer u een gekoppelde service van het type HDInsightOnDemand gebruikt, moet u een linkedServiceName die naar een Azure Blob-opslag verwijst opgeven. Data Factory-service gebruikt deze opslag voor het opslaan van Logboeken en ondersteunende bestanden voor uw cluster bellen op HDInsight.  Soms het inrichten van een cluster op verzoek HDInsight mislukt met de volgende fout:

        Failed to create cluster. Exception: Unable to complete the cluster create operation. Operation failed with code '400'. Cluster left behind state: 'Error'. Message: 'StorageAccountNotColocated'.

Deze fout duidt meestal op de locatie van de opslag-account die is opgegeven in de linkedServiceName is niet op dezelfde data center locatie waar het inrichten van HDInsight plaatsvindt. Voorbeeld: als uw gegevens fabriek in westelijk Verenigde Staten en de Azure opslag in Oost-VS, op de vraag-aanbod in West VS mislukt.

Er is een tweede JSON-eigenschap additionalLinkedServiceNames waar extra opslagruimte accounts kunnen worden opgegeven in HDInsight op verzoek. Die extra opslagruimte voor gekoppelde accounts moeten zich op dezelfde locatie als het cluster HDInsight of met dezelfde fout mislukt.

### <a name="problem-custom-net-activity-fails"></a>Probleem: Custom .NET activiteit mislukt
Zie [fouten opsporen in een pijpleiding met aangepaste activiteit](data-factory-use-custom-activities.md#debug-the-pipeline) voor gedetailleerde stappen. 

## <a name="use-azure-portal-to-troubleshoot"></a>Azure portal gebruiken om problemen met 

### <a name="using-portal-blades"></a>Met behulp van de portal-blades
Zie [Monitor pipeline](data-factory-build-your-first-pipeline-using-editor.md#monitor-pipeline) voor werk. 

### <a name="using-monitor-and-manage-app"></a>Met behulp van de Monitor en App beheren
Zie [controleren en beheren van data factory pijpleidingen Monitor en App beheren met](data-factory-monitor-manage-app.md) voor meer informatie. 

## <a name="use-azure-powershell-to-troubleshoot"></a>Azure PowerShell gebruiken om problemen met

### <a name="use-azure-powershell-to-troubleshoot-an-error"></a>Azure PowerShell gebruiken bij het oplossen van een fout  
Zie [Monitor Data Factory pijpleidingen met Azure PowerShell](data-factory-build-your-first-pipeline-using-powershell.md#monitor-pipeline) voor meer informatie. 


[adfgetstarted]: data-factory-copy-data-from-azure-blob-storage-to-sql-database.md
[use-custom-activities]: data-factory-use-custom-activities.md
[troubleshoot]: data-factory-troubleshoot.md
[developer-reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[cmdlet-reference]: http://go.microsoft.com/fwlink/?LinkId=517456
[json-scripting-reference]: http://go.microsoft.com/fwlink/?LinkId=516971

[azure-portal]: https://portal.azure.com/

[image-data-factory-troubleshoot-with-error-link]: ./media/data-factory-troubleshoot/DataFactoryWithErrorLink.png

[image-data-factory-troubleshoot-datasets-with-errors-blade]: ./media/data-factory-troubleshoot/DatasetsWithErrorsBlade.png

[image-data-factory-troubleshoot-table-blade-with-problem-slices]: ./media/data-factory-troubleshoot/TableBladeWithProblemSlices.png

[image-data-factory-troubleshoot-activity-run-with-error]: ./media/data-factory-troubleshoot/ActivityRunDetailsWithError.png

[image-data-factory-troubleshoot-dataslice-blade-with-active-runs]: ./media/data-factory-troubleshoot/DataSliceBladeWithActivityRuns.png

[image-data-factory-troubleshoot-walkthrough2-with-errors-link]: ./media/data-factory-troubleshoot/Walkthrough2WithErrorsLink.png

[image-data-factory-troubleshoot-walkthrough2-datasets-with-errors]: ./media/data-factory-troubleshoot/Walkthrough2DataSetsWithErrors.png

[image-data-factory-troubleshoot-walkthrough2-table-with-problem-slices]: ./media/data-factory-troubleshoot/Walkthrough2TableProblemSlices.png

[image-data-factory-troubleshoot-walkthrough2-slice-activity-runs]: ./media/data-factory-troubleshoot/Walkthrough2DataSliceActivityRuns.png

[image-data-factory-troubleshoot-activity-run-details]: ./media/data-factory-troubleshoot/Walkthrough2ActivityRunDetails.png
 