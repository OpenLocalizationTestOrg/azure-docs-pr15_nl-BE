<properties
    pageTitle="Visual Studio-sjablonen voor Batch Azure | Microsoft Azure"
    description="Leer hoe deze Visual Studio-project-sjablonen kunt u implementeren en voer uw computerintensieve werklasten op Azure Batch"
    services="batch"
    documentationCenter=".net"
    authors="fayora"
    manager="timlt"
    editor="" />

<tags
    ms.service="batch"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows"
    ms.workload="big-compute"
    ms.date="09/07/2016"
    ms.author="marsma" />

# <a name="visual-studio-project-templates-for-azure-batch"></a>Visual Studio-projectsjablonen voor Azure Batch

De **Manager van de taak** en **taak Processor Visual Studio-sjablonen** voor Batch bevatten code die u helpen te implementeren en uw computerintensieve werklasten worden uitgevoerd op een Batch met de minste inspanning. Dit document beschrijft deze sjablonen en biedt richtlijnen voor het gebruik ervan.

>[AZURE.IMPORTANT] In dit artikel wordt alleen informatie met betrekking tot deze twee sjablonen beschreven en wordt ervan uitgegaan dat u bekend met de Batch-service en de belangrijkste begrippen met betrekking tot het bent: pools, knooppunten, projecten en taken, projecttaken manager, omgevingsvariabelen en andere relevante gegevens berekenen. U kunt meer informatie vinden in de [Basisprincipes van Azure Batch](batch-technical-overview.md) [Batch Functieoverzicht voor ontwikkelaars](batch-api-basics.md)en [aan de slag met de bibliotheek Azure Batch voor .NET](batch-dotnet-get-started.md).

## <a name="high-level-overview"></a>Overzicht

De Manager van de taak en taak Processor-sjablonen kunnen worden gebruikt voor het maken van twee nuttige componenten:

* Een projecttaak manager waarmee een taak splitsen die u een taak opsplitsen kunt naar beneden in meerdere taken, die onafhankelijk van elkaar, gelijktijdig kunnen worden uitgevoerd.

* Een taak-processor die kan worden gebruikt om vooraf en achteraf worden verwerkt om een toepassing vanaf de opdrachtregel uitvoeren.

Bijvoorbeeld in een film rendering scenario, zou de gesplitste taak zet een taak één film in honderden of duizenden afzonderlijke taken die de afzonderlijke frames apart zou verwerken. Dienovereenkomstig, de processor voor de taak wilt renderen toepassing verklaren en alle afhankelijke processen die nodig zijn voor de weergave van elk frame, alsmede eventuele aanvullende acties (bij voorbeeld het weergegeven frame kopiëren naar een opslaglocatie) uitvoeren.

>[AZURE.NOTE] De Manager van de taak en taak Processor sjablonen zijn onafhankelijk van elkaar, zodat u kiezen kunt met beide of slechts één van beide, afhankelijk van de eisen van de afdruktaak compute en van uw voorkeuren.

Zoals in het onderstaande diagram, doorlopen een compute project dat met deze sjablonen gebruikt drie fasen:

1. De clientcode (bijvoorbeeld toepassing, webservice, enz.) wordt een taak aan de Batch-service op Azure, opgeven als de manager van de taak taak taak manager wordt ingediend.

2. De Batch-service de taak manager-taak wordt uitgevoerd op een compute node en de taak splitsen wordt het opgegeven aantal processor taken taak gestart op zoals veel zoals vereist computerknooppunten, op basis van de parameters en specificaties in de code van de gesplitste taak.

3. De taken van de processor taak uitgevoerd onafhankelijk van elkaar, gelijktijdig verwerken de invoergegevens en uitvoergegevens genereren.

![Diagram waarin wordt getoond hoe de clientcode samenwerkt met de Batch-service][diagram01]

## <a name="prerequisites"></a>Vereisten

De Batch als sjablonen wilt gebruiken, moet u het volgende:

* Een computer met Visual Studio 2015, of nieuwer, al is geïnstalleerd.

* De Batch-sjablonen beschikbaar in de [Visual Studio-galerie zijn] [ vs_gallery] als uitbreidingen Visual Studio. Er zijn twee manieren om de sjablonen:

  * Installeer de **Updates en uitbreidingen** in het dialoogvenster in de Visual Studio-sjablonen (Zie [Zoeken en met behulp van Visual Studio Extensions]voor meer informatie,[vs_find_use_ext]). Klik in het dialoogvenster **uitbreidingen en Updates** zoeken en downloaden van de volgende twee uitbreidingen:

    * Azure Batch Job Manager met de taak splitsen
    * Azure Batch taak Processor

  * De sjablonen downloaden vanaf de on line galerie voor Visual Studio: [Microsoft Azure Batch Project-sjablonen][vs_gallery_templates]

* Als u van plan bent de [Toepassingspakketten](batch-application-packages.md) gebruiken voor de implementatie van de manager van de taak en de processor van de taak aan de Batch compute nodes, moet u een opslag-account te koppelen aan uw account voor de Batch.

## <a name="preparation"></a>Voorbereiding

Wij raden u aan een oplossing met de manager van de taak als uw processor taak maken omdat dit kunt u gemakkelijker voor het delen van code, tussen de manager van de taak en taak processor-programma's. Ga als volgt te werk om te maken van deze oplossing:

1. Visual Studio 2015 openen en selecteert u **bestand** > **Nieuw** > **Project**.

2. **Andere projecttypen**Vouw onder **sjablonen**naar **Oplossingen met Visual Studio**en selecteer vervolgens de **Blanco-oplossing**.

3. Typ een naam voor uw toepassing en het doel van deze oplossing (bijvoorbeeld "LitwareBatchTaskPrograms").

4. Klik op **OK**om de nieuwe oplossing.

## <a name="job-manager-template"></a>Sjabloon voor Project Manager

De sjabloon Job Manager helpt u een taak manager kan de volgende acties uitvoeren:

* Een taak splitsen in meerdere taken.
* Deze taken uit te voeren op Batch indienen.

>[AZURE.NOTE] Zie [overzicht van Batch-functie voor ontwikkelaars](batch-api-basics.md#job-manager-task)voor meer informatie over het beheer van taken.

### <a name="create-a-job-manager-using-the-template"></a>Een Project Manager met behulp van de sjabloon maken

Als een project manager aan de oplossing die u eerder hebt gemaakt, als volgt te werk:

1. Open uw bestaande oplossing in Visual Studio 2015.

2. Klik met de rechtermuisknop op de oplossing in de Solution Explorer, klikt u op **toevoegen** > **Nieuw Project**.

3. In **Visual C#**, klikt u op de **Cloud**en klik vervolgens op **Azure Batch Job Manager met de taak splitsen**.

4. Typ een naam die uw toepassing worden beschreven en wordt dit project als de manager van de taak (b.v. "LitwareJobManager").

5. Klik op **OK**om het project.

6. Tot slot het project als u Visual Studio conservenindustrie waarnaar wordt verwezen NuGet laden en om te controleren of het project geldig is voordat u begint met het wijzigen.

### <a name="job-manager-template-files-and-their-purpose"></a>Job Manager sjabloonbestanden en hun doel

Wanneer u een project met de Job Manager sjabloon maakt, genereert bestanden met drie groepen:

* Het belangrijkste bestand (Program.cs). Dit document bevat het ingangspunt programma en de afhandeling van uitzonderingen op het hoogste niveau. U moet niet normaal moet dit wijzigen.

* De Framework-map. Dit bevat de bestanden die verantwoordelijk is voor het 'standaard' werk gedaan door het programma project manager – uitpakken van parameters, taken toe te voegen aan de batchverwerking, enz. U moet niet normaal moet deze bestanden wijzigen.

* De splitser taakbestand (JobSplitter.cs). Dit is waar u uw specifieke logica voor het splitsen van een taak in taken plaatst.

Natuurlijk kunt u extra bestanden zoals vereist voor de ondersteuning van uw taak splitsen code, afhankelijk van de complexiteit van de taak splitsen logica toevoegen.

De sjabloon genereert ook standaard .NET projectbestanden zoals een .csproj bestand app.config, packages.config, enz.

De rest van deze sectie beschrijft de verschillende bestanden en de codestructuur en wordt uitgelegd wat elke klasse wordt.

![Visual Studio Solution Explorer met de Manager van de taak sjabloonoplossing][solution_explorer01]

**Framework-bestanden**

* `Configuration.cs`: Het laden van de configuratiegegevens van de taak zoals details van Batch gekoppelde opslag referenties, taak en taakgegevens en taakparameters ingekapseld. Het biedt ook toegang tot Batch gedefinieerde omgevingsvariabelen (Zie instellingen voor taken in de documentatie van de Batch omgeving) via de klasse Configuration.EnvironmentVariable.

* `IConfiguration.cs`: De uitvoering van de klasse configuratie abstracts zodat u kunt testen van eenheden van uw taak splitsen met behulp van een valse of model configuration-object.

* `JobManager.cs`: De onderdelen van het programma project manager orchestrates. Het is verantwoordelijk voor het initialiseren van de gesplitste taak, aanroepen van de gesplitste taak, en het verzenden van de taken die door de taak splitsen wordt geretourneerd aan de indiener van de taak.

* `JobManagerException.cs`: Geeft een fout de manager van de taak is te beëindigen. JobManagerException wordt gebruikt om fouten 'verwacht' waar specifieke diagnostische informatie kan worden geleverd als onderdeel van de beëindiging.

* `TaskSubmitter.cs`: Deze klasse is toe te voegen die wordt geretourneerd door het splitsen van de taak met de batchverwerking taken verantwoordelijk. De aggregaten van de klasse Taakbeheer heeft de volgorde van taken in batches voor efficiënte maar tijdig toevoeging aan de taak, roept vervolgens TaskSubmitter.SubmitTasks op een achtergrondthread voor elke partij.

**Taak splitsen**

`JobSplitter.cs`: Deze klasse bevat de logica voor het splitsen van de taak in taken toepassingsspecifieke. Het kader roept de methode JobSplitter.Split voor het verkrijgen van een reeks taken die worden toegevoegd aan het project zoals deze door de methode als resultaat. Dit is de klasse waarin u de logica van uw project worden gevoegd. De methode gesplitst als u een reeks CloudTask-instanties die de taken die u wilt partitioneren van de taak wilt implementeren.

**Standaard .NET opdrachtregel project-bestanden**

* `App.config`: Standaard .NET toepassingsconfiguratiebestand.

* `Packages.config`: Standaard NuGet afhankelijkheid pakketbestand.

* `Program.cs`: Bevat het ingangspunt programma en de afhandeling van uitzonderingen op het hoogste niveau.

### <a name="implementing-the-job-splitter"></a>Uitvoering van de taak splitsen

Wanneer u het sjabloonproject Manager project-opent, wordt het project het JobSplitter.cs bestand standaard geopend hebben. U kunt de logica van de splitsing van de taken in uw werkbelasting implementeren met behulp van de onderstaande Split() methode weergeven:

```csharp
/// <summary>
/// Gets the tasks into which to split the job. This is where you inject
/// your application-specific logic for decomposing the job into tasks.
///
/// The job manager framework invokes the Split method for you; you need
/// only to implement it, not to call it yourself. Typically, your
/// implementation should return tasks lazily, for example using a C#
/// iterator and the "yield return" statement; this allows tasks to be added
/// and to start running while splitting is still in progress.
/// </summary>
/// <returns>The tasks to be added to the job. Tasks are added automatically
/// by the job manager framework as they are returned by this method.</returns>
public IEnumerable<CloudTask> Split()
{
    // Your code for the split logic goes here.
    int startFrame = Convert.ToInt32(_parameters["StartFrame"]);
    int endFrame = Convert.ToInt32(_parameters["EndFrame"]);

    for (int i = startFrame; i <= endFrame; i++)
    {
        yield return new CloudTask("myTask" + i, "cmd /c dir");
    }
}
```

>[AZURE.NOTE] De sectie aantekeningen in de `Split()` methode is het enige gedeelte van de taak Manager sjablooncode die voor u wijzigen door de logica toe te voegen geschikt is aan uw taken opsplitsen in verschillende taken. Als u een andere sectie van de sjabloon wijzigen wilt, zorg ervoor dat u met Batch werking, zijn familiarized en een paar van de [Batch-codevoorbeelden]uitproberen[github_samples].

Uw implementatie Split() heeft toegang tot:

* De taakparameters via de `_parameters` veld.
* Het CloudJob-object dat de taak, die de `_job` veld.
* Het CloudTask-object dat de taak manager, die de `_jobManagerTask` veld.

Uw `Split()` uitvoering hoeft geen taken rechtstreeks toevoegen aan de taak. In plaats daarvan uw code moet een reeks objecten CloudTask terug en worden deze toegevoegd aan het project automatisch door de framework-klassen die aanroepen van de gesplitste taak. Meestal wordt gebruik van C# iterator (`yield return`) functie te implementeren taak splitsbalken als hierdoor de taken starten zo spoedig mogelijk niet gewacht tot alle taken worden berekend.

**Taak splitsen storing**

Als u de taak splitsen zich een fout voordoet, moet het ofwel:

* Beëindigen van de reeks met behulp van de C#- `yield break` -instructie, in welke geval de manager van de taak wordt behandeld als geslaagd; of

* Veroorzaak een uitzondering, waarin die de manager van de taak, worden behandeld als mislukt en opnieuw kunnen worden uitgevoerd afhankelijk van hoe de client deze heeft geconfigureerd).

In beide gevallen worden al die wordt geretourneerd door de taak splitsen en toegevoegd aan de batchverwerking taken kunnen worden uitgevoerd. Als u niet wilt dat dit gebeurt, klikt kan u:

* De taak beëindigen voordat u terugkeert uit de taak splitsen

* Formuleren van de collectie van de gehele taak voordat deze wordt geretourneerd (dat wil zeggen, terug een `ICollection<CloudTask>` of `IList<CloudTask>` in plaats van de uitvoering van uw taak splitsen met behulp van een iterator voor C#)

* Gebruik taakafhankelijkheden om alle taken die afhankelijk zijn van de succesvolle afronding van de project manager

**Project manager pogingen**

Als de manager van de taak mislukt, wordt er opnieuw geprobeerd door de Batch-service afhankelijk van de instellingen van de client opnieuw. Dit is in het algemeen veilig, omdat wanneer de framework-taken aan het project toevoegen, worden alle taken die al bestaan genegeerd. Echter, als het berekenen van taken is duur, u wilt niet voor wiens rekening de kosten van de herberekening van de taken die al zijn toegevoegd aan het project; omgekeerd, als het opnieuw uitvoeren is niet gegarandeerd dezelfde taak-id's genereren Ga het gedrag 'dubbele negeren' wordt niet lekker. In deze gevallen moet u de taak splitsen voor het detecteren van het werk dat al is uitgevoerd en niet herhaald, bijvoorbeeld door een CloudJob.ListTasks in te voeren voordat u begint met het rendement van taken te ontwerpen.

### <a name="exit-codes-and-exceptions-in-the-job-manager-template"></a>De afsluitcodes en uitzonderingen in de sjabloon Job Manager

De afsluitcodes en uitzonderingen voorzien van een mechanisme om het resultaat van een programma vast te stellen en ze kunnen helpen bij het identificeren van problemen met de uitvoering van het programma. De sjabloon Job Manager implementeert de afsluitcodes en uitzonderingen in deze sectie beschreven.

Een projecttaak manager die wordt geïmplementeerd met de sjabloon Job Manager kan drie mogelijke afsluitcodes retourneren:

| Code | Beschrijving |
|------|-------------|
| 0    | De project manager met succes is voltooid. Uw taak splitsen code volledig is uitgevoerd en alle taken aan het project zijn toegevoegd. |
| 1    | De taak van de manager kan niet met een uitzondering in een 'verwacht' deel van het programma. De uitzondering is omgezet in een JobManagerException met diagnostische gegevens en, waar mogelijk, suggesties voor het oplossen van de storing. |
| 2    | De taak van de manager kan niet met een 'onverwachte' uitzondering. De uitzondering naar de standaarduitvoer is vastgelegd, maar de manager van de taak kan niet een diagnose of herstel informatie toegevoegd. |

Wanneer een project manager taak optreedt, kunnen sommige taken nog steeds zijn toegevoegd aan de service voordat de fout is opgetreden. Deze taken worden uitgevoerd als normale. Zie 'Taak splitsen Failure' hierboven voor de bespreking van dit codepad.

Alle informatie die wordt geretourneerd door de uitzonderingen wordt in de bestanden stdout.txt en stderr.txt geschreven. Zie [Fouten afhandelen](batch-api-basics.md#error-handling)voor meer informatie.

### <a name="client-considerations"></a>Overwegingen voor clients

Deze sectie beschrijft de vereisten voor een implementatie van de client bij het aanroepen van een project manager op basis van deze sjabloon. Zie [parameters en variabelen van de clientcode doorgeven](#pass-environment-settings) voor meer informatie over het doorgeven van parameters en omgevingsinstellingen.

**Verplichte referenties**

Taken toevoegen aan de batchverwerking Azure, moet de taak manager Azure Batch account URL en de sleutel. U moet de volgende omgevingsvariabelen YOUR_BATCH_URL en YOUR_BATCH_KEY met de naam doorgeven. U kunt instellingen voor deze taak Manager taak omgeving. Bijvoorbeeld, in een C# client:

```csharp
job.JobManagerTask.EnvironmentSettings = new [] {
    new EnvironmentSetting("YOUR_BATCH_URL", "https://account.region.batch.azure.com"),
    new EnvironmentSetting("YOUR_BATCH_KEY", "{your_base64_encoded_account_key}"),
};
```
**Opslag referenties**

De client hoeft meestal niet om de gekoppelde opslag referenties voor de taak manager omdat (a) meest project managers niet hoeft expliciet toegang tot de account gekoppelde opslag en (b) de opslag van gekoppelde account is vaak voor alle taken als de omgevingsinstelling van een gemeenschappelijke voor het project. Als u niet de account gekoppelde opslag via de algemene instellingen voor het milieu bieden en de manager van de taak is vereist voor toegang tot gekoppelde opslag, moet u opgeven de referenties van de gekoppelde opslag als volgt:

```csharp
job.JobManagerTask.EnvironmentSettings = new [] {
    /* other environment settings */
    new EnvironmentSetting("LINKED_STORAGE_ACCOUNT", "{storageAccountName}"),
    new EnvironmentSetting("LINKED_STORAGE_KEY", "{storageAccountKey}"),
};
```

**Project manager taakinstellingen**

De client moet de vlag taak manager *killJobOnCompletion* ingesteld op **false**.

Het is doorgaans veilig zijn voor de client *runExclusive* instellen op **false**.

De client moet de *resourceFiles* of *applicationPackageReferences* -collectie gebruiken om de taak manager uitvoerbare (en de vereiste DLL-bestanden) naar de compute node geïmplementeerd.

Standaard, de manager van de taak wordt niet opnieuw geprobeerd is mislukt. Afhankelijk van uw project manager logica, de client wilt inschakelen, pogingen via *beperkingen*/*maxTaskRetryCount*.

**Taakinstellingen**

Als u de taak splitsen zendt de taken met afhankelijkheden, moet de client van het project usesTaskDependencies ingesteld op true.

In het model van de gesplitste taak is het ongebruikelijk dat clients kunnen taken toevoegen aan andere projecten naast wat het splitsen van de taak wordt gemaakt. De client moet van het project *onAllTasksComplete* daarom normaal gesproken ingesteld op **terminatejob**.

## <a name="task-processor-template"></a>Sjabloon voor taak-Processor

Een taak Processor-sjabloon kunt u voor de uitvoering van een taak-processor kan de volgende acties uitvoeren:

* Stel de informatie die is vereist voor elke batchtaak uit te voeren.
* Alle acties die zijn vereist voor elke batchtaak uitvoeren.
* Uitgangen van de taak opslaan in permanente opslag.

Hoewel de processor van een taak niet uit te voeren taken op Batch is vereist, is het belangrijkste voordeel van een processor voor de taak dat u een wrapper implementatie van alle acties van de taak kan worden uitgevoerd op één locatie. Bijvoorbeeld, als u meerdere toepassingen worden uitgevoerd in de context van elke taak, of als u nodig hebt om te kopiëren gegevens permanent opslaan na het voltooien van elke taak.

De acties die worden uitgevoerd door de processor voor de taak kunnen worden als eenvoudig of complex, en zo veel of weinig, zoals vereist door uw werkbelasting. Bovendien door de uitvoering van alle acties van de taak in één taak processor, kunt u gemakkelijk bijwerken of toevoegen acties op basis van wijzigingen in toepassingen of vereisten van de werklast. Echter, in sommige gevallen een processor voor de taak mogelijk niet de optimale oplossing voor uw implementatie zoals deze onnodige complexiteit, bijvoorbeeld bij het uitvoeren van taken die snel kunnen worden gestart vanaf de opdrachtregel voor een eenvoudige kunt toevoegen.

### <a name="create-a-task-processor-using-the-template"></a>De Processor van een taak met behulp van de sjabloon maken

Als de processor van een taak toevoegen aan de oplossing die u eerder hebt gemaakt, als volgt te werk:

1. Open uw bestaande oplossing in Visual Studio 2015.

2. Met de rechtermuisknop op de oplossing in de Solution Explorer, klikt u op **toevoegen**en klik vervolgens op **Nieuw Project**.

3. Klik op **wolk**onder **Visual C#**, en klik op **Azure Batch taak Processor**.

4. Typ een naam die uw toepassing worden beschreven en wordt dit project als de taak processor (b.v. "LitwareTaskProcessor").

5. Klik op **OK**om het project.

6. Tot slot het project als u Visual Studio conservenindustrie waarnaar wordt verwezen NuGet laden en om te controleren of het project geldig is voordat u begint met het wijzigen.

### <a name="task-processor-template-files-and-their-purpose"></a>Processor-sjabloonbestanden taak en hun doel

Wanneer u een project met de taak processor sjabloon maakt, genereert bestanden met drie groepen:

* Het belangrijkste bestand (Program.cs). Dit document bevat het ingangspunt programma en de afhandeling van uitzonderingen op het hoogste niveau. U moet niet normaal moet dit wijzigen.

* De Framework-map. Dit bevat de bestanden die verantwoordelijk is voor het 'standaard' werk gedaan door het programma project manager – uitpakken van parameters, taken toe te voegen aan de batchverwerking, enz. U moet niet normaal moet deze bestanden wijzigen.

* De processor een bestand (TaskProcessor.cs). Dit is waar u uw specifieke logica voor het uitvoeren van een taak (meestal door te bellen naar een bestaande uitvoerbaar) wordt geplaatst. Vóór en na verwerking komt, bijvoorbeeld extra gegevens downloaden of uploaden van bestanden met resultaten, ook hier code.

Natuurlijk kunt u extra bestanden zoals vereist voor de ondersteuning van de code voor de processor, afhankelijk van de complexiteit van de taak splitsen logica toevoegen.

De sjabloon genereert ook standaard .NET projectbestanden zoals een .csproj bestand app.config, packages.config, enz.

De rest van deze sectie beschrijft de verschillende bestanden en de codestructuur en wordt uitgelegd wat elke klasse wordt.

![Visual Studio Solution Explorer met de sjabloon taak Processor oplossing][solution_explorer02]

**Framework-bestanden**

* `Configuration.cs`: Het laden van de configuratiegegevens van de taak zoals details van Batch gekoppelde opslag referenties, taak en taakgegevens en taakparameters ingekapseld. Het biedt ook toegang tot Batch gedefinieerde omgevingsvariabelen (Zie instellingen voor taken in de documentatie van de Batch omgeving) via de klasse Configuration.EnvironmentVariable.

* `IConfiguration.cs`: De uitvoering van de klasse configuratie abstracts zodat u kunt testen van eenheden van uw taak splitsen met behulp van een valse of model configuration-object.

* `TaskProcessorException.cs`: Geeft een fout de manager van de taak is te beëindigen. TaskProcessorException wordt gebruikt om fouten 'verwacht' waar specifieke diagnostische informatie kan worden geleverd als onderdeel van de beëindiging.

**Taak-Processor**

* `TaskProcessor.cs`: De taak wordt uitgevoerd. Het kader roept de methode TaskProcessor.Run. Dit is de klasse waarin u de toepassingsspecifieke logica van de taak worden gevoegd. De methode Run te implementeren:
  * Parseren en valideren van de taakparameters
  * De opdrachtregel voor alle externe programma's die u wilt aanroepen opstellen
  * Meld u nodig hebt mogelijk voor foutopsporing diagnostische gegevens
  * Starten met deze opdrachtregel
  * Wacht tot het proces af te sluiten
  * Vastleggen van de afsluitcode van het proces om te bepalen of het is gelukt of mislukt
  * Sla alle uitvoerbestanden u wilt behouden voor permanente opslag

**Standaard .NET opdrachtregel project-bestanden**

* `App.config`: Standaard .NET toepassingsconfiguratiebestand.
* `Packages.config`: Standaard NuGet afhankelijkheid pakketbestand.
* `Program.cs`: Bevat het ingangspunt programma en de afhandeling van uitzonderingen op het hoogste niveau.

## <a name="implementing-the-task-processor"></a>Uitvoering van de taak-processor

Wanneer u het project taak Processor-sjabloon opent, wordt het project het TaskProcessor.cs bestand standaard geopend hebben. U kunt de logica uitvoeren voor de taken in uw werkbelasting implementeren met behulp van de methode Run() hieronder weergegeven:

```csharp
/// <summary>
/// Runs the task processing logic. This is where you inject
/// your application-specific logic for decomposing the job into tasks.
///
/// The task processor framework invokes the Run method for you; you need
/// only to implement it, not to call it yourself. Typically, your
/// implementation will execute an external program (from resource files or
/// an application package), check the exit code of that program and
/// save output files to persistent storage.
/// </summary>
public async Task<int> Run()

{
    try
    {
        //Your code for the task processor goes here.
        var command = $"compare {_parameters["Frame1"]} {_parameters["Frame2"]} compare.gif";
        using (var process = Process.Start($"cmd /c {command}"))
        {
            process.WaitForExit();
            var taskOutputStorage = new TaskOutputStorage(
            _configuration.StorageAccount,
            _configuration.JobId,
            _configuration.TaskId
            );
            await taskOutputStorage.SaveAsync(
            TaskOutputKind.TaskOutput,
            @"..\stdout.txt",
            @"stdout.txt"
            );
            return process.ExitCode;
        }
    }
    catch (Exception ex)
    {
        throw new TaskProcessorException(
        $"{ex.GetType().Name} exception in run task processor: {ex.Message}",
        ex
        );
    }
}
```
>[AZURE.NOTE] De sectie aantekeningen in de methode Run() is het enige gedeelte van de taak Processor sjablooncode die voor u wijzigen geschikt is door de logica uitvoeren voor de taken in uw werkbelasting toe te voegen. Als u een andere sectie van de sjabloon wijzigen wilt, maak eerst Lees hoe Batch werkt door de Batch-documentatie bekijken en uitproberen van een aantal van de codevoorbeelden Batch.

De methode Run() is verantwoordelijk voor het starten van de opdrachtregel starten van een of meer processen, wachten tot alle proces te voltooien, opslaan van de resultaten en ten slotte met een afsluitcode terug. De methode Run() is waar u de verwerking logica implementeren voor uw taken. Het kader van de processor taak roept de methode Run() voor u; u hoeft niet zelf bellen.

Uw implementatie Run() heeft toegang tot:

* De taakparameters, via de `_parameters` veld.
* De taak en taak-id's, via de `_jobId` en `_taskId` velden.
* De configuratie van de taak, via de `_configuration` veld.

**Taak mislukt**

Bij een storing, kunt u de methode Run() afsluiten met uitzondering, maar dit het bovenste niveau uitzonderings-handler voor de controle op de afsluitcode taak blijft. Als u de afsluitcode wordt weergegeven zodat u onderscheid maken verschillende soorten storingen, bijvoorbeeld voor diagnostische doeleinden tussen kunt of omdat sommige soorten de taak moeten worden beëindigd en anderen niet te beheren, moet u de methode Run() afsluiten door een niet-nul afsluitcode terug te keren. Dit wordt de afsluitcode voor de taak.

### <a name="exit-codes-and-exceptions-in-the-task-processor-template"></a>De afsluitcodes en uitzonderingen in de sjabloon taak Processor

De afsluitcodes en uitzonderingen voorzien van een mechanisme om het resultaat van een programma vast te stellen en ze kunnen helpen bij problemen met de uitvoering van het programma. De taak Processor sjabloon implementeert de afsluitcodes en uitzonderingen in deze sectie beschreven.

Processor met een taak die is geïmplementeerd met de sjabloon taak Processor kan drie mogelijke afsluitcodes retourneren:

| Code | Beschrijving |
|------|-------------|
|  [Process.ExitCode][process_exitcode] | De processor voor de taak hebt voltooid. Opmerking: dit niet zeggen wil dat het programma dat u hebt aangeroepen, alleen gelukt is – dat de processor voor de taak is aangeroepen en uitgevoerd zonder uitzonderingen na verwerking. De betekenis van de afsluitcode wordt weergegeven is afhankelijk van de geactiveerde programma – meestal de afsluitcode 0 betekent het programma is voltooid en de afsluitcode kan. |
| 1    | De processor van de taak kan niet met een uitzondering in een 'verwacht' deel van het programma. De uitzondering is omgezet in een `TaskProcessorException` met diagnostische gegevens en, waar mogelijk, suggesties voor het oplossen van de storing. |
| 2    | De processor van de taak kan niet met een 'onverwachte' uitzondering. De uitzondering naar de standaarduitvoer is vastgelegd, maar de processor voor de taak kan niet extra diagnostische of herstel informatie toevoegen. |

>[AZURE.NOTE] Als die u oproepen de afsluitcodes van 1 en 2 wordt om aan te geven van de specifieke storingen op te sporen, is vervolgens met behulp van de afsluitcodes van 1 en 2 voor fouten-processor niet eenduidig. Kunt u deze taak processor-foutcodes onderscheidend exit codes door de uitzondering aanvragen in het bestand Program.cs te bewerken.

Alle informatie die wordt geretourneerd door de uitzonderingen wordt in de bestanden stdout.txt en stderr.txt geschreven. Zie Fouten afhandelen, in de Batch-documentatie voor meer informatie.

### <a name="client-considerations"></a>Overwegingen voor clients

**Opslag referenties**

Als de taak processor gebruikmaakt van Azure blob-opslag persistent outputs, bijvoorbeeld met behulp van de verdragen helper bibliotheek, vervolgens moet deze toegang tot *beide* de cloud opslag referenties *of* een blob container URL met een gedeelde access-handtekening (SAS). De sjabloon bevat ondersteuning voor het verstrekken van referenties via gemeenschappelijke omgevingsvariabelen. De client kunt als volgt de opslag referenties doorgeven:

```csharp
job.CommonEnvironmentSettings = new [] {
    new EnvironmentSetting("LINKED_STORAGE_ACCOUNT", "{storageAccountName}"),
    new EnvironmentSetting("LINKED_STORAGE_KEY", "{storageAccountKey}"),
};
```

De opslag-account is beschikbaar in de klasse TaskProcessor via de `_configuration.StorageAccount` eigenschap.

Als u liever een container URL met SAS, kunt u dit ook doorgeven via een instelling voor taak gemeenschappelijke omgeving, maar de sjabloon taak processor momenteel geen ingebouwde ondersteuning voor dit deel.

**Setup van opslag**

Het wordt aanbevolen dat de client- of manager-taak maken de recipiënten moet met taken voordat u de taken aan het project toe te voegen. Dit is verplicht dat als u de URL van een container met SAS, als zodanig een URL bevat geen machtiging voor het maken van de container. Het verdient zelfs als u de referenties van opslag, zoals elke taak hoeven aanroepen van CloudBlobContainer.CreateIfNotExistsAsync in de container wordt opgeslagen.

## <a name="pass-parameters-and-environment-variables"></a>Doorgeven van parameters en variabelen

### <a name="pass-environment-settings"></a>Instellingen uit de omgeving

Een client kan informatie doorgeven aan de taak in de vorm van de instellingen voor beheer. Deze informatie kan vervolgens worden gebruikt door de taak manager bij het genereren van de taak processor taken die worden uitgevoerd als onderdeel van het project berekenen. Voorbeelden van de informatie die u als de instellingen doorgeven kunt zijn:

* Opslag account naam en account-toetsen
* Batch account URL
* Batch account sleutel

De Batch-service heeft een eenvoudig mechanisme omgevingsinstellingen doorgeven aan een taak manager met behulp van de `EnvironmentSettings` eigenschap in [Microsoft.Azure.Batch.JobManagerTask][net_jobmanagertask].

Bijvoorbeeld, om de `BatchClient` instantie voor een Batch-account kunt u doorgeven als u omgevingsvariabelen van de client de URL en de gedeelde sleutel referenties voor de account van de Batch-code. Ook voor toegang tot de opslag-account die is gekoppeld aan de Batch-account, kunt u doorgeven de naam van de opslag en de sleutel van de account opslag als omgevingsvariabelen.

### <a name="pass-parameters-to-the-job-manager-template"></a>Parameters doorgeven aan de sjabloon Job Manager

In veel gevallen is het handig per project parameters doorgeven aan de manager taak, de taak splitsen proces bepalen of de taken voor de taak te configureren. U kunt dit doen door een JSON-bestand met de naam parameters.json als een bronbestand voor het project manager uploaden. De parameters kunnen dan beschikbaar zijn in de `JobSplitter._parameters` veld in de sjabloon van de Manager van de taak.

>[AZURE.NOTE] De ingebouwde parameter-handler ondersteunt alleen string-naar-tekenreeks-woordenboeken. Als u complexe JSON-waarden als parameters doorgeven wilt, moet u deze als tekenreeksen worden doorgegeven en ze worden geparseerd in de taak splitsen of van het kader wijzigen `Configuration.GetJobParameters` methode.

### <a name="pass-parameters-to-the-task-processor-template"></a>Parameters doorgeven aan de sjabloon taak Processor

U kunt ook parameters doorgeven aan de afzonderlijke taken die zijn geïmplementeerd met behulp van de sjabloon voor de Processor van de taak. Net als met de project manager-sjabloon zoekt de sjabloon taak processor een bestand met de naam

parameters.JSON, en als het gevonden als de parameters woordenlijst geladen. Er zijn een aantal opties waarmee u parameters doorgeven aan de processor taak taken:

* De taakparameters JSON gebruiken. Dit werkt goed als de enige parameters die van de hele taak (voor bijvoorbeeld een render hoogte en breedte zijn). Hiervoor, bij het maken van een CloudTask in de gesplitste taak toevoegen een verwijzing naar het object parameters.json resource file van de projecttaak manager ResourceFiles (`JobSplitter._jobManagerTask.ResourceFiles`) aan de CloudTask van ResourceFiles-collectie.

* Genereren en die blob in van de taak resource files-collectie verwijzen naar een taak-specifieke parameters.json document uploaden als onderdeel van de taakuitvoering splitter. Dit is nodig als er verschillende taken hebben verschillende parameters. Een voorbeeld kan zijn van een 3D-beeldopbouw scenario waar de frame-index naar de taak als parameter wordt doorgegeven.

>[AZURE.NOTE] De ingebouwde parameter-handler ondersteunt alleen string-naar-tekenreeks-woordenboeken. Als u complexe JSON-waarden als parameters doorgeven wilt, moet u deze als tekenreeksen worden doorgegeven en ze worden geparseerd in de processor van de taak of van het kader wijzigen `Configuration.GetTaskParameters` methode.

## <a name="next-steps"></a>Volgende stappen

### <a name="persist-job-and-task-output-to-azure-storage"></a>Project en uitvoer naar Azure opslag aanhouden

Een ander nuttig hulpmiddel in de ontwikkeling van oplossingen voor Batch [Azure Batch bestand verdragen]is[nuget_package]. Deze .NET-klassenbibliotheek (momenteel in het voorbeeld) in Batch .NET toepassingen gemakkelijk opslaan en ophalen van taak uitgangen en naar Azure opslag gebruiken. [Azure-batchverwerking behouden en de taak wordt uitgevoerd](batch-task-output.md) , bevat een gedetailleerde beschrijving van de bibliotheek en het gebruik ervan.

### <a name="batch-forum"></a>Batch-Forum

Het [Forum van Azure Batch] [ forum] is een prima plek om te bespreken Batch en vragen stellen over de service op MSDN. Hoofd op over voor nuttige 'vastzetten' geposte berichten en vragen posten dat ze zich voordoen terwijl u uw Batch oplossingen bouwen.

[forum]: https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=azurebatch
[net_jobmanagertask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobmanagertask.aspx
[github_samples]: https://github.com/Azure/azure-batch-samples
[nuget_package]: https://www.nuget.org/packages/Microsoft.Azure.Batch.Conventions.Files
[process_exitcode]: https://msdn.microsoft.com/library/system.diagnostics.process.exitcode.aspx
[vs_gallery]: https://visualstudiogallery.msdn.microsoft.com/
[vs_gallery_templates]: https://go.microsoft.com/fwlink/?linkid=820714
[vs_find_use_ext]: https://msdn.microsoft.com/library/dd293638.aspx

[diagram01]: ./media/batch-visual-studio-templates/diagram01.png
[solution_explorer01]: ./media/batch-visual-studio-templates/solution_explorer01.png
[solution_explorer02]: ./media/batch-visual-studio-templates/solution_explorer02.png
