<properties
    pageTitle="Stap 3: Maak een nieuwe Machine Learning-experiment | Microsoft Azure"
    description="Stap 3 van het opstellen van een scenario voorspellende oplossing: Maak een nieuwe opleiding experiment in Azure Machine Learning Studio."
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016" 
    ms.author="garye"/>


# <a name="walkthrough-step-3-create-a-new-azure-machine-learning-experiment"></a>Procedure-stap 3: Maak een nieuwe Azure Machine Learning experiment

Dit is de derde stap van de procedure, het [ontwikkelen van een voorspellende analytics oplossing in Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md)


1.  [Een Machine Learning werkruimte maken](machine-learning-walkthrough-1-create-ml-workspace.md)
2.  [Bestaande gegevens uploaden](machine-learning-walkthrough-2-upload-data.md)
3.  **Maak een nieuw experiment**
4.  [Trainen en het evalueren van de modellen](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5.  [De web-service implementeren](machine-learning-walkthrough-5-publish-web-service.md)
6.  [Toegang tot de webservice.](machine-learning-walkthrough-6-access-web-service.md)

----------

De volgende stap in dit scenario is voor het maken van een experiment in Machine Learning Studio die gebruikmaakt van de dataset die is geüpload.  

1.  Klik in Studio **+ Nieuw** onder aan het venster.
2.  Selecteer **EXPERIMENT**, en selecteer vervolgens "Lege Experiment". De standaardnaam van de proef op de bovenkant van het papier en herkenbare naam

    > [AZURE.TIP] Het is een goede gewoonte om de **Samenvatting** en **Omschrijving** invullen voor het experiment in het deelvenster **Eigenschappen** . Deze eigenschappen geven u de kans om het experiment zodat iedereen die ze later uw doelstellingen en methodologie begrijpen zullen.

3.  Papier, vouw **Datasets opgeslagen**in het palet van de module aan de linkerkant van het experiment.
4.  Zoek de gegevensset die u hebt gemaakt onder **Mijn Datasets** en sleept u het naar het canvas. U kunt ook de dataset vinden door te voeren in **het zoekvak boven het palet** .  

##<a name="prepare-the-data"></a>De gegevens gereedmaken
U kunt de eerste 100 rijen van de gegevens en bepaalde statistische gegevens voor de hele gegevensset weergeven door op de uitvoerpoort van de gegevensset (de kleine cirkel onder) en selecteert u **visualiseren**.  

Omdat het bestand niet is meegeleverd met kolomkoppen, heeft Studio algemene koppen (Col1, Kol2, *enz.*) verstrekt. Goede koppen niet essentieel zijn voor het maken van een model, maar ze gemakkelijker werken met de gegevens in het experiment. Ook wanneer we uiteindelijk dit model in een webservice publiceert, helpt de koppen de kolommen aan de gebruiker van de service te identificeren.  

De [Metagegevens bewerken] met kolomkoppen kunt toevoegen[ edit-metadata] module.
U gebruikt de [Metagegevens bewerken] [ edit-metadata] module wijzigen van metagegevens die zijn gekoppeld aan een dataset. In dit geval kan bieden meer beschrijvende namen voor kolomkoppen. 

[Metagegevens bewerken]gebruiken[edit-metadata], u eerst opgeven welke kolommen u wilt wijzigen (in dit geval allemaal.) Geef vervolgens de actie moet worden uitgevoerd op deze kolommen (in dit geval kolomkoppen wijzigen.)

1.  Typ in het palet module 'metagegevens' in **het zoekvak** . Ziet u [Metagegevens bewerken] [ edit-metadata] weergegeven in de modulelijst.
2.  Klik en sleep de [Metagegevens bewerken] [ edit-metadata] module op het tekenpapier en beneden de gegevensset die we eerder toegevoegd.
3.  De dataset verbinden met de [Metagegevens bewerken][edit-metadata]: klik op de uitvoerpoort van de gegevensset (de kleine cirkel onderaan in de dataset.) Vervolgens sleept u de invoerpoort van [Metagegevens bewerken] [ edit-metadata] (de kleine cirkel aan de bovenkant van de module), laat de muisknop vervolgens los. De module en de dataset blijven verbonden, zelfs als u een op het tekenpapier verplaatsen.

    Het experiment moet nu als volgt uitzien:  

    ![Metagegevens bewerken][2]
    
    Het rode uitroepteken geeft aan dat we de eigenschappen voor deze module nog niet hebt ingesteld. We doen dat volgende.
    
    > [AZURE.TIP] U kunt een opmerking toevoegen aan een module door te dubbelklikken op de module en het invoeren van tekst. Zo kunt u in één oogopslag zien wat de module doet in het experiment. Dubbelklik in dit geval de [Metagegevens bewerken] [ edit-metadata] module en typ de opmerking "Add kolomkoppen". Klik ergens anders op het tekenpapier te sluiten in het tekstvak. Het commentaar, klikt u op de pijl-omlaag op de module.

4.  Selecteer [Metagegevens bewerken][edit-metadata], klikt u in het deelvenster **Eigenschappen** aan de rechterkant van het canvas **kolomkiezer te starten**.
5.  In het dialoogvenster **kolommen selecteren** , selecteert u alle rijen in de **Beschikbare kolommen** en klik op > om ze te verplaatsen naar **Kolommen geselecteerd**.
Het dialoogvenster ziet er zo uit: ![kolomkiezer met alle geselecteerde kolommen][4]
7.  Klik op het vinkje **OK** .
8.  Zoek de **nieuwe kolomnamen** parameter in het deelvenster **Eigenschappen** . Voer in dit veld een lijst met de namen van de kolommen 21 in de dataset, gescheiden door komma's en in de volgorde van kolommen. U kunt de namen van kolommen ophalen van de dataset-documentatie op de website van ICB's of voor het gemak kunt u kopiëren en plakken in de volgende lijst:  

          Status of checking account, Duration in months, Credit history, Purpose, Credit amount, Savings account/bond, Present employment since, Installment rate in percentage of disposable income, Personal status and sex, Other debtors, Present residence since, Property, Age in years, Other installment plans, Housing, Number of existing credits, Job, Number of people providing maintenance for, Telephone, Foreign worker, Credit risk  

    Het eigenschappenvenster ziet er zo uit:

    ![Eigenschappen voor metagegevens bewerken][1]

> [AZURE.TIP] Als u controleren of de kolomkoppen wilt, voert u het experiment (Klik op **uitvoeren** onder het canvas experiment). Wanneer is voltooid (een groen vinkje verschijnt op [Metagegevens bewerken][edit-metadata]), klikt u op de uitvoerpoort van de [Metagegevens bewerken] [ edit-metadata] module en selecteer **visualiseren**. U kunt de uitvoer van een willekeurige module weergeven op dezelfde manier weer te geven van de voortgang van de gegevens via het experiment.

##<a name="create-training-and-test-datasets"></a>Opleiding maken en testen van datasets

De volgende stap van het experiment is voor het genereren van afzonderlijke datasets die we gebruiken voor training en testen van ons model.

Hiervoor gebruiken we de [Gesplitste gegevens] [ split] module.  

1.  De [Gesplitste gegevens] gevonden[ split] module, sleept u het naar het canvas en verbind deze met de laatste [Metagegevens bewerken] [ edit-metadata] module.
2.  De splitsingsverhouding wordt 0,5 standaard, en de parameter **Randomized splitsen** is ingesteld. Dit betekent dat een willekeurig deel van de gegevens uitgevoerd via een poort van de [Gesplitste gegevens wordt] [ split] -module en de helft door de andere. U kunt deze als de parameter **Random zaad** te wijzigen van de splitsing tussen opleiding en gegevens testen. Bijvoorbeeld, laten we ze als-is.
    
    > [AZURE.TIP] De eigenschap **fractie van rijen in de eerste uitvoer dataset** bepaalt hoeveel gegevens uitvoer via de linker uitvoerpoort. Bijvoorbeeld, als u de verhouding op 0,7, wordt 70% van de gegevens uitvoer via de poort links en 30% via de juiste poort.  
    
3. Dubbelklik op de [Gesplitste gegevens] [ split] module en voert u de opmerking, ' Training/testen gegevens splitsen 50% '. 

Gebruiken we de uitgangen van de [Gesplitste gegevens] [ split] module echter we graag, maar we kiezen als u wilt gebruiken de uitvoer links trainingsgegevens en rechts als testen gegevens uitvoeren.  

Zoals vermeld op de website van ICB's, de kosten van een hoge kredietrisico als lage onjuiste vijf keer groter is dan de kosten van een laag kredietrisico zo hoog onjuiste. Als u daarmee rekening houden, wij een nieuwe gegevensset die overeenkomt met deze functie kosten genereren. In de nieuwe gegevensset wordt elk voorbeeld met een hoog risico vijf keer gerepliceerd terwijl elk voorbeeld met laag risico wordt niet gerepliceerd.   

Doen we deze replicatie met R-code:  

1.  Zoeken en sleept u het [R-Script uitvoeren] [ execute-r-script] module naar het experiment canvas en verbinding maken met de linker-uitvoerpoort van de [Gesplitste gegevens] [ split] module aan de eerste invoer poort ("Dataset1") van de [R-Script uitvoeren] [ execute-r-script] module.
2. Dubbelklik op de [R-Script uitvoeren] [ execute-r-script] module en voert u de opmerking, "Set kosten van aanpassing".
2.  De standaardtekst die in de parameter **R Script** verwijderen in het deelvenster **Eigenschappen** en voer dit script:

          dataset1 <- maml.mapInputPort(1)
          data.set<-dataset1[dataset1[,21]==1,]
          pos<-dataset1[dataset1[,21]==2,]
          for (i in 1:5) data.set<-rbind(data.set,pos)
          maml.mapOutputPort("data.set")


Moeten we deze dezelfde replicatiebewerking voor elke uitvoer van de [Gesplitste gegevens] [ split] module zodat de gegevens van de opleiding en de tests de herwaardering van dezelfde kosten hebben.

1.  Klik met de rechtermuisknop op de [R-Script uitvoeren] [ execute-r-script] module en selecteer **kopiëren**.
2.  Klik met de rechtermuisknop op het tekenpapier experiment en selecteer **Plakken**.
3.  Verbinding maken met de eerste invoerpoort van deze [R-Script uitvoeren] [ execute-r-script] module rechts-uitvoerpoort van de [Gesplitste gegevens] [ split] module. 
4.  Klik op **uitvoeren**op de onderkant van het papier. 

> [AZURE.TIP] Het exemplaar van de module R Script uitvoeren bevat als de oorspronkelijke module hetzelfde script. Wanneer u kopieert en plakt een module op het canvas, behoudt de kopie de eigenschappen van het origineel.  

Ons experiment er nu ongeveer als volgt:

![Module splitsen en R scripts toe te voegen][3]

Zie voor meer informatie over het gebruik van scripts R in uw experimenten [uitbreiden uw experiment met R](machine-learning-extend-your-experiment-with-r.md).

**Volgende: [trein en evalueren van de modellen](machine-learning-walkthrough-4-train-and-evaluate-models.md)**


[1]: ./media/machine-learning-walkthrough-3-create-new-experiment/create1.png
[2]: ./media/machine-learning-walkthrough-3-create-new-experiment/create2.png
[3]: ./media/machine-learning-walkthrough-3-create-new-experiment/create3.png
[4]: ./media/machine-learning-walkthrough-3-create-new-experiment/columnselector.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[edit-metadata]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
