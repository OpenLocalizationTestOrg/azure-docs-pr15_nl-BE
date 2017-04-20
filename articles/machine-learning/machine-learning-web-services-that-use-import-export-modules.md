<properties
    pageTitle="Azure ML webservices die gebruikmaken van gegevens importeren en exporteren van gegevens implementeren | Microsoft Azure"
    description="Informatie over het gebruik van de modules importeren en exporteren van gegevens te verzenden en ontvangen van gegevens van een webservice."
    services="machine-learning"
    documentationCenter=""
    authors="vDonGlover"
    manager="raymondlaghaeian"
    editor=""/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/12/2016"
    ms.author="v-donglo"/>



# <a name="deploying-azure-ml-web-services-that-use-data-import-and-data-export-modules"></a>Azure ML webservices die gebruikmaken van gegevens importeren en exporteren van gegevens-modules distribueren 

Bij het maken van een anticiperende experiment toevoegen u meestal een web service invoer en uitvoer. Als u het experiment implementeert, kunnen consumenten gegevens verzenden en ontvangen van de webservice via de ingangen en uitgangen. Voor sommige toepassingen, gegevens van de consument van een kanaal gegevens beschikbaar zijn of al zijn opgeslagen in een externe gegevensbron, zoals een Azure Blob-opslag. In deze gevallen hoeven ze niet lezen en schrijven van gegevens met behulp van web service ingangen en uitgangen. Ze kunnen in plaats daarvan de Batch uitvoeren van Service (BES) gebruiken om gegevens te lezen uit de gegevensbron met behulp van een module gegevens importeren en de score resultaten schrijven naar een andere locatie met behulp van een module gegevens exporteren.

De gegevens importeren en exporteren gegevens modules, lezen en schrijven op een aantal locaties, bijvoorbeeld de URL van een Web via HTTP, een Query component, een Azure SQL-database, tabel Azure opslag, Azure Blob-opslag, een Data-kanaal bieden gegevens of een lokale SQL-database.

In dit onderwerp wordt het "monster 5: trein, testen, evalueren voor binaire indeling: volwassen Dataset" monster en wordt ervan uitgegaan dat de dataset is al geladen in een Azure SQL-tabel met de naam censusdata.

## <a name="create-the-training-experiment"></a>Het experiment training maken 
 
Bij het openen van het "monster 5: trein, testen, evalueren voor binaire indeling: volwassen Dataset" monster de dataset monster volwassen Census inkomsten binaire indeling wordt gebruikt. En het experiment in het canvas ziet er ongeveer als de volgende afbeelding.

![Eerste configuratie van het experiment.](./media/machine-learning-web-services-that-use-import-export-modules/initial-look-of-experiment.png)
  

De gegevens uit de tabel Azure SQL lezen:

1.  De dataset module verwijderen.
2.  Typ in het zoekvak onderdelen importeren.
3.  Toevoegen vanuit de lijst met resultaten een module *Gegevens importeren* naar het canvas experiment.
4.  Verbinding maken met de uitvoer van de module *Gegevens importeren* de invoer van de module *Schoon ontbrekende gegevens* .
5.  Selecteer in het deelvenster Eigenschappen, **Azure SQL-Database** in de vervolgkeuzelijst voor de **Gegevensbron** .
6.  Voer de juiste gegevens voor uw database in de **naam van de databaseserver** **databasenaam**, **gebruikersnaam**en **wachtwoord** -velden.
7.  Voer in het veld Database query de volgende query.

        select [age],
           [workclass],
           [fnlwgt],
           [education],
           [education-num],
           [marital-status],
           [occupation],
           [relationship],
           [race],
           [sex],
           [capital-gain],
           [capital-loss],
           [hours-per-week],
           [native-country],
           [income]
        from dbo.censusdata;

8.  Canvas aan de onderkant van het experiment, klik op **uitvoeren**.

## <a name="create-the-predictive-experiment"></a>De voorspellende experiment maken

Volgende die u stelt de voorspellende experiment waaruit u uw web-service implementeert.

1.  Onderaan in het canvas experiment **Webservice instellen** op en selecteer **Webservice voorspellende (aanbevolen)**.
2.  De *Web Service invoer* en *uitvoer van Web Service-modules* verwijderen uit de voorspellende experiment. 
3.  Typ in het zoekvak van componenten exporteren.
4.  Toevoegen vanuit de lijst met resultaten een module *Gegevens exporteren* naar het canvas experiment.
5.  Verbinding maken met de uitvoer van de module *Score-Model* de invoer van de module *Gegevens exporteren* . 
6.  Selecteer in het deelvenster Eigenschappen, **Azure SQL-Database** in de vervolgkeuzelijst bestemming voor gegevens.
7.  Voer de juiste gegevens voor uw database in de **naam van de databaseserver** **databasenaam**, **gebruikersnaam Server**en **serverwachtwoord** velden.
8.  Typ Labels gescoord in het veld **door komma's gescheiden lijst met kolommen die moet worden opgeslagen** .
9.  Typ in het **naamveld gegevenstabel**dbo. ScoredLabels. Als de tabel niet bestaat nog, wordt deze gemaakt wanneer de proef wordt uitgevoerd of de webservice wordt aangeroepen.
10. Typ in het veld **door komma's gescheiden lijst met kolommen datatable** ScoredLabels.

Wanneer u een toepassing die de uiteindelijke webservice aanroept schrijven, kunt u een andere invoer voor de query of tabel van bestemming opgeven tijdens het uitvoeren. Configureren van deze ingangen en uitgangen, kunt u de functie Web Service Parameters instellen voor de eigenschap voor *gegevensbron* module *Gegevens importeren* en de eigenschap *Gegevens exporteren* modus data bestemming.  Zie voor meer informatie over de Parameters van de Web Service, de [Parameters van de AzureML Web Service post](https://blogs.technet.microsoft.com/machinelearning/2014/11/25/azureml-web-service-parameters/) op de Cortana intelligentie en Machine Learning Blog.

Parameters van de webservice voor de query importeren en de doeltabel configureren:

1.  Klik op het pictogram boven in het deelvenster Eigenschappen voor de module *Gegevens importeren* , rechts van de **databasequery** veld en selecteer **instellen als webserviceparameter**.
2.  Klik op het pictogram boven in het deelvenster Eigenschappen voor de module *Gegevens exporteren* , rechts van het veld **naam voor gegevenstabellen** en selecteer **instellen als webserviceparameter**.
3.  Onder aan het deelvenster *Gegevens exporteren* module eigenschappen, in de sectie **Parameters van de Web Service** databasequery op en wijzig de naam van deze Query.
4.  Klik op **gegevens tabel naam** en wijzig de naam van deze **tabel**.

Wanneer u klaar bent, moet uw experiment lijken op de volgende afbeelding.
 
![Definitieve uiterlijk van het experiment.](./media/machine-learning-web-services-that-use-import-export-modules/experiment-with-import-data-added.png)

Nu kunt u het experiment implementeren als een webservice.

## <a name="deploy-the-web-service"></a>De web-service implementeren 
U kunt implementeren op een klassieke of een nieuwe webservice.

### <a name="deploy-a-classic-web-service"></a>Een klassieke Internet implementeren

Als een klassieke Internet implementeren en verbruikt deze toepassing maken:

1.  Klik onderaan in het canvas experiment uitvoeren.
2.  Wanneer de sessie is voltooid, klikt u op **Webservice implementeren** en selecteer **Webservice implementeren [Classic]**.
3.  Zoek uw API-sleutel op het webdashboard-service. Kopiëren en opslaan voor later gebruik.
4.  Klik op de koppeling **Batch worden uitgevoerd** om de API te pagina te openen in de tabel **Standaard eindpunt** .
5.  Maak een C#-consoletoepassing in Visual Studio.
6.  Op de pagina API helpen de **Voorbeeldcode** sectie vinden onder aan de pagina.
7.  Kopiëren en plakken van de C#-voorbeeldcode in het bestand Program.cs en verwijder alle verwijzingen naar de blobopslag.
8.  De waarde van de variabele *apiKey* met eerdere API-sleutel bijwerken.
9.  Zoek de declaratie van de aanvraag en de waarden van Web Service-Parameters die worden doorgegeven aan de modules *Importeren* en *Exporteren van gegevens* bijwerken. In dit geval wordt u de oorspronkelijke query gebruiken, maar een nieuwe tabelnaam definiëren.

        var request = new BatchExecutionRequest() 
        {   
            GlobalParameters = new Dictionary<string, string>() {
            { "Query", @"select [age], [workclass], [fnlwgt], [education], [education-num], [marital-status], [occupation], [relationship], [race], [sex], [capital-gain], [capital-loss], [hours-per-week], [native-country], [income] from dbo.censusdata" },
            { "Table", "dbo.ScoredTable2" },
            }
        };

10. De toepassing wordt uitgevoerd. 

Na afloop van de sessie wordt een nieuwe tabel toegevoegd aan de database met de resultaten van de score.

### <a name="deploy-a-new-web-service"></a>Een nieuwe webservice implementeren

Als een nieuwe webservice implementeren en verbruikt deze toepassing maken:

1.  Canvas aan de onderkant van het experiment, klik op **uitvoeren**.
2.  Wanneer de sessie is voltooid, klikt u op **Webservice implementeren** en selecteer **Distribueren Web Service [Nieuw]**.
3.  Voer een naam voor uw web-service op de pagina Experiment implementeren en selecteer een prijsstelling plan en klik vervolgens op **distribueren**.
4.  Klik op de pagina **Quickstart** **verbruiken**.
5.  Klik in het gedeelte met **Voorbeeldcode** **Batch**.
6.  Maak een C#-consoletoepassing in Visual Studio.
7.  Kopieer en plak de C#-voorbeeldcode in het bestand Program.cs.
8.  De waarde van de variabele *apiKey* bijwerken met de **Primaire sleutel** in de sectie **info Basic verbruik** .
9.  Zoek de declaratie met *scoreRequest* en de waarden van Web Service-Parameters die worden doorgegeven aan de modules *Importeren* en *Exporteren van gegevens* bijwerken. In dit geval wordt u de oorspronkelijke query gebruiken, maar een nieuwe tabelnaam definiëren.

        var scoreRequest = new
        {
            Inputs = new Dictionary<string, StringTable>()
            {
            },
            GlobalParameters = new Dictionary<string, string>() {
                 { "Query", @"select [age], [workclass], [fnlwgt], [education], [education-num], [marital-status], [occupation], [relationship], [race], [sex], [capital-gain], [capital-loss], [hours-per-week], [native-country], [income] from dbo.censusdata" },
                { "Table", "dbo.ScoredTable3" },
            }
        };

10. De toepassing wordt uitgevoerd. 
 

