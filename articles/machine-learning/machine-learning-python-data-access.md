<properties 
    pageTitle="Toegang tot datasets met Machine Learning Python client library | Microsoft Azure" 
    description="Installeren en gebruiken van de client Python bibliotheek en Azure Machine Learning gegevens veilig te beheren vanuit een lokale Python-omgeving." 
    services="machine-learning" 
    documentationCenter="python" 
    authors="bradsev" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/12/2016" 
    ms.author="huvalo;bradsev" />


#<a name="access-datasets-with-python-using-the-azure-machine-learning-python-client-library"></a>Access gegevenssets met Python met de client Azure Machine Learning Python bibliotheek 

Het voorbeeld van de client-bibliotheek van Microsoft Azure Machine Learning Python beveiligde toegang kunt inschakelen op uw Azure Machine Learning datasets van een lokale Python omgeving en kunt het maken en beheren van datasets in een werkruimte.

In dit onderwerp vindt u instructies voor het:

* installeren van de client Machine Learning Python bibliotheek 
* toegang en datasets, inclusief instructies voor het opvragen van de machtiging voor toegang tot Azure Machine Learning datasets in uw lokale omgeving van Python uploaden
*  toegang tot tussentijdse datasets van experimenten
*  gebruikt de clientbibliotheek Python datasets inventariseren, toegang krijgen tot de metagegevens, de inhoud van een dataset lezen, nieuwe datasets maken en bijwerken van bestaande datasets

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

 
##<a name="prerequisites"></a>Vereisten

De client Python bibliotheek is getest onder de volgende omgevingen:

 - Windows, Mac en Linux
 - Python 2.7, 3.3 en 3.4

Deze is afhankelijk van de volgende pakketten:

 - aanvragen
 - Python-dateutil
 - pandas

Wij raden u aan met behulp van die een verdeling van Python, zoals hierboven genoemde [Anaconda](http://continuum.io/downloads#all) of [bladerdak](https://store.enthought.com/downloads/), die komen met Python, IPython en de drie pakketten geïnstalleerd. Hoewel IPython niet strikt nodig is, is een geweldige omgeving voor het manipuleren en interactief visualiseren van gegevens.


###<a name="installation"></a>Het installeren van de client Azure Machine Learning Python bibliotheek

De client Azure Machine Learning Python bibliotheek moet ook worden geïnstalleerd om de taken die in dit onderwerp worden beschreven. Het is beschikbaar via de [Index van Python pakket](https://pypi.python.org/pypi/azureml). Om het te installeren in uw omgeving Python, moet u de volgende opdracht uitvoeren vanuit uw lokale Python omgeving:

    pip install azureml

U kunt ook downloaden en installeren van de bronnen op [github](https://github.com/Azure/Azure-MachineLearning-ClientLibrary-Python).

    python setup.py install

Als u git geïnstalleerd op uw computer hebt, kunt u pip rechtstreeks vanaf de opslagplaats git installeren:

    pip install git+https://github.com/Azure/Azure-MachineLearning-ClientLibrary-Python.git


##<a name="datasetAccess"></a>Codefragmenten Studio gebruiken voor toegang tot datasets

De client Python bibliotheek hebt u programmatische toegang tot uw bestaande datasets van experimenten die zijn uitgevoerd.

Vanuit de Studio web-interface kunt u de codefragmenten die alle nodige informatie om te downloaden en datasets deserialize als objecten op uw machine locatie Pandas DataFrame genereren.

### <a name="security"></a>Beveiliging voor toegang tot gegevens

De codefragmenten door Studio geleverd voor gebruik met de client Python bibliotheek uw werkruimte-id en -autorisatie omvat token. Deze volledige toegang tot uw werkruimte en als een wachtwoord moeten worden beschermd.

De code stukje functionaliteit is alleen beschikbaar voor gebruikers die hun rol als **eigenaar** van de werkruimte instellen om veiligheidsredenen. Uw rol in Azure Machine Learning Studio weergegeven op de pagina **gebruikers** onder **Instellingen**.

![Beveiliging][security]

Als uw rol is niet ingesteld als **eigenaar**, kunt u een aanvraag voor opnieuw worden uitgenodigd als eigenaar of vraag de eigenaar van de werkruimte waarin u het codefragment.

Als u wilt de Autorisatietoken verkrijgen, kunt u het volgende doen:



- Vraag om een token van een eigenaar. Eigenaren van toegang tot hun vergunning tokens van de pagina instellingen van de werkruimte in de Studio. Selecteer **Instellingen** in het linkerdeelvenster en klik op **TOKENS vergunning** om te zien van de primaire en secundaire tokens.  Hoewel de primaire of de secundaire vergunning tokens kunnen worden gebruikt in het volgende codefragment, wordt aangeraden dat eigenaren van alleen de tokens secundaire vergunning delen.

![](./media/machine-learning-python-data-access/ml-python-access-settings-tokens.png)

- Vragen worden gepromoveerd tot de rol van eigenaar.  U doet dit door een huidige eigenaar van de werkruimte, moet eerst worden verwijderd uit de werkruimte en nodigen u opnieuw aan als eigenaar van een.

Als ontwikkelaars de id van de werkruimte en de vergunning hebben verkregen token, ze hebben toegang tot de werkruimte met behulp van het codefragment ongeacht hun rol.

Vergunning tokens worden beheerd op de pagina **Verificatie TOKENS** onder **Instellingen**. U kunt opnieuw genereren, maar deze procedure trekt u toegang tot de vorige token.

### <a name="accessingDatasets"></a>Datasets toegang vanuit een lokale toepassing van Python

1. Klik in de Studio van Machine Learning, **DATASETS** in de navigatiebalk aan de linkerkant.

2. Selecteer de gegevensset die u wilt openen. Kunt u een van de gegevenssets in de lijst **Mijn DATASETS** of uit de lijst met **voorbeelden** .

3. Klik op de werkbalk onder **Data Access Code genereren**. Als de gegevens in een indeling die niet compatibel is met de client Python bibliotheek, wordt deze knop beschikbaar.

    ![DataSets][datasets]

4. Het codefragment selecteren in het venster dat wordt weergegeven en naar het Klembord kopiëren.

    ![Toegangscode][dataset-access-code]

5. Plak de code in de laptop van uw lokale toepassing van Python.

    ![Laptop][ipython-dataset]

## <a name="accessingIntermediateDatasets"></a>Tussenliggende datasets van Machine Learning experimenten toegang

Na een experiment wordt uitgevoerd in de Studio van Machine Learning, is het mogelijk toegang te krijgen tot de tussenliggende datasets van de knooppunten van de uitvoer van modules. Tussenliggende datasets zijn de gegevens die zijn gemaakt en gebruikt voor de tussenliggende stappen wanneer u een model programma is uitgevoerd.

Tussenliggende datasets toegankelijk zijn als de gegevensindeling compatibel met de mediawisselaar Python client is.

De volgende indelingen worden ondersteund (constanten voor deze zijn de `azureml.DataTypeIds` klasse):

 - Leesbare tekst
 - GenericCSV
 - GenericTSV
 - GenericCSVNoHeader
 - GenericTSVNoHeader

Door het aanwijzen van een knooppunt van de output module kunt u de indeling bepalen. Samen met de naam van het knooppunt in de knopinfo wordt weergegeven.

Sommige van de modules, zoals de [gesplitste] [ split] module, de uitvoer naar een indeling met de naam `Dataset`, die niet wordt ondersteund door de client Python bibliotheek.

![DataSet-indeling][dataset-format]

Moet u een conversie-module, zoals het [converteren naar de bestandsindeling CSV][convert-to-csv], voor uitvoer naar een ondersteunde indeling.

![Indeling GenericCSV][csv-format]

De volgende stappen geven een voorbeeld van een experiment wordt gemaakt, wordt deze uitgevoerd en toegang heeft tot de tussenliggende dataset.

1. Maak een nieuw experiment.

2. Voeg een module **dataset volwassen Census inkomsten binaire indeling** .

3. Invoegen van een [gesplitste] [ split] module, en sluit de invoer voor de uitvoer van de module dataset.

4. Voeg een [converteren naar de bestandsindeling CSV] [ convert-to-csv] module en verbinding maken met de invoer naar een van de [gesplitste] [ split] module levert.

5. Sla het experiment uit te voeren en wachten op het voltooien.

6. Klik op het knooppunt van de uitvoer in de [CSV omzetten] [ convert-to-csv] module.

7. Wanneer het snelmenu wordt weergegeven, selecteert u **Data Access Code genereren**.

    ![Contextmenu][experiment]

8. Het codefragment selecteren en naar het Klembord kopiëren in het venster dat verschijnt.

    ![Toegangscode][intermediate-dataset-access-code]

9. Plak de code in uw notitieblok.

    ![Laptop][ipython-intermediate-dataset]

10. U kunt de gegevens visualiseren met matplotlib. In de kolom Leeftijd een histogram weergegeven:

    ![Histogram][ipython-histogram]


##<a name="clientApis"></a>Gebruik de Machine Learning Python client-bibliotheek te openen, lezen, maken en beheren van datasets

### <a name="workspace"></a>Werkruimte

De werkruimte is het startpunt voor de client Python bibliotheek. Geef de `Workspace` klasse met uw werkruimte-id en -autorisatie token om een exemplaar te maken:

    ws = Workspace(workspace_id='4c29e1adeba2e5a7cbeb0e4f4adfb4df',
                   authorization_token='f4f3ade2c6aefdb1afb043cd8bcf3daf')


### <a name="enumerate-datasets"></a>Datasets opsommen

Inventarisatie van alle gegevenssets in een bepaalde werkruimte:

    for ds in ws.datasets:
        print(ds.name)

Alleen de gebruiker gemaakte datasets opsommen:

    for ds in ws.user_datasets:
        print(ds.name)

Alleen de datasets voorbeeld opsommen:

    for ds in ws.example_datasets:
        print(ds.name)

Een dataset kunt u openen met de naam (dit is hoofdlettergevoelig):

    ds = ws.datasets['my dataset name']

Of u toegang hebt tot het met de index:

    ds = ws.datasets[0]


### <a name="metadata"></a>Metagegevens

DataSets hebben metagegevens naast inhoud. (Tussenliggende datasets vormen een uitzondering op deze regel en zijn geen metagegevens.)

Sommige metagegevenswaarden worden toegewezen door de gebruiker bij het maken:

    print(ds.name)
    print(ds.description)
    print(ds.family_id)
    print(ds.data_type_id)

Anderen zijn toegewezen door Azure ML:

    print(ds.id)
    print(ds.created_date)
    print(ds.size)

Zie de `SourceDataset` voor meer informatie over de beschikbare metagegevens.


### <a name="read-contents"></a>Inhoud lezen

De codefragmenten automatisch aangeleverd door Machine Learning Studio downloaden en terugconverteren naar een object Pandas DataFrame dataset. Dit wordt gedaan met de `to_dataframe` methode:

    frame = ds.to_dataframe()

U kunt desgewenst de onbewerkte gegevens downloaden en uitvoeren van de deserialisatie is dat een optie. Op het moment dat is dit de enige optie voor 'ARFF', de client Python bibliotheek niet meer terugconverteren van de volgende bestandsindelingen.

De inhoud lezen als tekst:

    text_data = ds.read_as_text()

De inhoud lezen als binair bestand:

    binary_data = ds.read_as_binary()

U kunt ook gewoon een stroom naar de inhoud openen:

    with ds.open() as file:
        binary_data_chunk = file.read(1000)


### <a name="create-a-new-dataset"></a>Een nieuwe gegevensset maken

De client Python bibliotheek kunt u uploaden datasets vanuit uw programma Python. Deze datasets zijn vervolgens beschikbaar voor gebruik in uw werkruimte.

Als u de gegevens in een DataFrame Pandas hebt, kunt u de volgende code gebruiken:

    from azureml import DataTypeIds

    dataset = ws.datasets.add_from_dataframe(
        dataframe=frame,
        data_type_id=DataTypeIds.GenericCSV,
        name='my new dataset',
        description='my description'
    )

Als uw gegevens al een serienummer heeft, kunt u gebruiken:

    from azureml import DataTypeIds

    dataset = ws.datasets.add_from_raw_data(
        raw_data=raw_data,
        data_type_id=DataTypeIds.GenericCSV,
        name='my new dataset',
        description='my description'
    )

De clientbibliotheek Python, kan een DataFrame Pandas naar de volgende indelingen serialiseren (constanten voor deze zijn de `azureml.DataTypeIds` klasse):

 - Leesbare tekst
 - GenericCSV
 - GenericTSV
 - GenericCSVNoHeader
 - GenericTSVNoHeader


### <a name="update-an-existing-dataset"></a>Een bestaande gegevensset bijwerken

Als u een nieuwe dataset met een naam die overeenkomt met een bestaande dataset uploaden probeert, krijgt u een foutbericht.

Als het bijwerken van een bestaande dataset, moet u eerst een verwijzing naar de bestaande gegevensset:

    dataset = ws.datasets['existing dataset']

    print(dataset.data_type_id) # 'GenericCSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up to jan 2015'

Gebruik vervolgens `update_from_dataframe` serialiseren en de inhoud van de dataset op Azure vervangen:

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(frame2)

    print(dataset.data_type_id) # 'GenericCSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up to jan 2015'

Als u de gegevens naar een andere indeling wilt, geeft u een waarde voor de optionele `data_type_id` parameter.

    from azureml import DataTypeIds

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(
        dataframe=frame2,
        data_type_id=DataTypeIds.GenericTSV,
    )

    print(dataset.data_type_id) # 'GenericTSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up to jan 2015'

U kunt desgewenst een nieuwe beschrijving instellen door een waarde opgeeft voor de `description` parameter.

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(
        dataframe=frame2,
        description='data up to feb 2015',
    )

    print(dataset.data_type_id) # 'GenericCSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up to feb 2015'

U kunt desgewenst een nieuwe naam instellen door een waarde opgeeft voor de `name` parameter. Van nu af aan zult u de gegevensset met behulp van de nieuwe naam alleen ophalen. De volgende code werkt de gegevens, de naam en beschrijving.

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(
        dataframe=frame2,
        name='existing dataset v2',
        description='data up to feb 2015',
    )

    print(dataset.data_type_id)                    # 'GenericCSV'
    print(dataset.name)                            # 'existing dataset v2'
    print(dataset.description)                     # 'data up to feb 2015'

    print(ws.datasets['existing dataset v2'].name) # 'existing dataset v2'
    print(ws.datasets['existing dataset'].name)    # IndexError

De `data_type_id`, `name` en `description` parameters zijn optioneel en standaard naar de vorige waarde. De `dataframe` parameter is altijd vereist.

Als uw gegevens al een serienummer heeft, gebruikt u `update_from_raw_data` in plaats van `update_from_dataframe`. Als u alleen `raw_data` in plaats van `dataframe`, het werkt op een soortgelijke manier.



<!-- Images -->
[security]:./media/machine-learning-python-data-access/security.png
[dataset-format]:./media/machine-learning-python-data-access/dataset-format.png
[csv-format]:./media/machine-learning-python-data-access/csv-format.png
[datasets]:./media/machine-learning-python-data-access/datasets.png
[dataset-access-code]:./media/machine-learning-python-data-access/dataset-access-code.png
[ipython-dataset]:./media/machine-learning-python-data-access/ipython-dataset.png
[experiment]:./media/machine-learning-python-data-access/experiment.png
[intermediate-dataset-access-code]:./media/machine-learning-python-data-access/intermediate-dataset-access-code.png
[ipython-intermediate-dataset]:./media/machine-learning-python-data-access/ipython-intermediate-dataset.png
[ipython-histogram]:./media/machine-learning-python-data-access/ipython-histogram.png


<!-- Module References -->
[convert-to-csv]: https://msdn.microsoft.com/library/azure/faa6ba63-383c-4086-ba58-7abf26b85814/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
 
