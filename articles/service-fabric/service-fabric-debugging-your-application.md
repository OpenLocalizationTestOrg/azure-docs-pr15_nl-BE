<properties
   pageTitle="Fouten opsporen in uw toepassing in Visual Studio | Microsoft Azure"
   description="De betrouwbaarheid en prestaties van uw diensten verbeteren door te ontwikkelen en ze op een cluster van plaatselijke ontwikkeling foutopsporing in Visual Studio."
   services="service-fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="06/21/2016"
   ms.author="vturecek;mikhegn"/>

# <a name="debug-your-service-fabric-application-by-using-visual-studio"></a>Fouten opsporen in uw toepassing Fabric-Service met behulp van Visual Studio

## <a name="debug-a-local-service-fabric-application"></a>Fouten opsporen in een lokale Service Fabric-toepassing

U kunt tijd en geld besparen door de implementatie en het opsporen van fouten in uw toepassing Azure Service Fabric in een cluster van lokale computer ontwikkeling. Visual Studio kunt u de toepassing op het lokale cluster implementeren en automatisch verbinding maken met de debugger alle exemplaren van uw toepassing.

1. Een cluster van plaatselijke ontwikkeling door de stappen in het [instellen van uw ontwikkelomgeving Fabric Service](service-fabric-get-started.md)starten.

2. Druk op **F5** of klik op **Foutopsporing** > **Foutopsporing starten**.

    ![Foutopsporing op een toepassing starten][startdebugging]

3. Onderbrekingspunten instellen in uw code en de toepassing stap voor stap door te klikken op opdrachten in het menu **Foutopsporing** .

    > [AZURE.NOTE] Visual Studio hecht aan alle exemplaren van uw toepassing. Terwijl u de code hebt doorlopen, kunnen onderbrekingspunten ophalen geraakt door meerdere processen waardoor gelijktijdige sessies. Kunt u de onderbrekingspunten uitschakelen nadat ze geraakt bent door middel van de onderbrekingspunten afhankelijk zijn van de thread-ID of met behulp van diagnostische gebeurtenissen.

4. Het venster **Diagnostische gebeurtenissen** wordt automatisch geopend zodat u diagnostische gebeurtenissen in real time kunt bekijken.

    ![Diagnostische gebeurtenissen weergeven in real time][diagnosticevents]

5. U kunt ook het venster **Diagnostische gebeurtenissen** in Cloud Explorer openen.  Onder **Service Fabric**, klik met de rechtermuisknop op een willekeurig knooppunt en kies **Weergeven Streaming sporen**.

    ![Open het venster diagnostische gebeurtenissen][viewdiagnosticevents]

    Als u uw sporen naar een bepaalde service of toepassing te filteren wilt, kunt u gewoon streaming traces op die specifieke service of toepassing.

6. De diagnostische gebeurtenissen kunnen u bekijken in het automatisch gegenereerde **ServiceEventSource.cs** bestand en worden aangeroepen vanuit toepassingscode.

    ```csharp
    ServiceEventSource.Current.ServiceMessage(this, "My ServiceMessage with a parameter {0}", result.Value.ToString());
    ```

7. Het venster **Diagnostische gebeurtenissen** ondersteunt filtering, onderbreken en gebeurtenissen in real-time controleren.  Het filter is een eenvoudige tekenreeks zoeken van de gebeurtenis weergegeven, inclusief de inhoud ervan.

    ![Filteren, onderbreken en hervatten of gebeurtenissen in real-time controleren][diagnosticeventsactions]

8. Services voor foutopsporing is vergelijkbaar met het opsporen van fouten in een andere toepassing. Normaal gesproken wordt u onderbrekingspunten in Visual Studio ingesteld voor eenvoudige foutopsporing. Hoewel betrouwbare collecties naar meerdere knooppunten repliceren, implementeren ze toch IEnumerable. Dit betekent dat u de resultaten weergeven in Visual Studio gebruiken kunt tijdens het opsporen van fouten om te zien wat u hebt opgeslagen in. Gewoon een onderbrekingspunt instellen overal in uw code.

    ![Foutopsporing op een toepassing starten][breakpoint]

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->

## <a name="debug-a-remote-service-fabric-application"></a>Fouten opsporen in een externe toepassing voor Service Fabric

Als uw Service Fabric-toepassingen worden uitgevoerd op een cluster Service Fabric in Azure, kan u op afstand fouten opsporen in deze rechtstreeks vanuit Visual Studio.

> [AZURE.NOTE] De functie vereist [Service Fabric SDK 2.0](http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015) en de [SDK voor .NET 2,9 Azure](https://azure.microsoft.com/downloads/).    

<!-- -->
> [AZURE.WARNING] Foutopsporing op afstand is bedoeld voor scenario's dev/test en niet moet worden gebruikt in een productieomgeving, omdat de impact op de actieve toepassingen.

1. Ga naar uw cluster in **Cloud Explorer**, klik met de rechtermuisknop en kiest u **Foutopsporing inschakelen**

    ![Foutopsporing op afstand inschakelen][enableremotedebugging]

    Dit zal uitgangspunt voor het proces van het inschakelen van de uitbreiding voor externe foutopsporing op de knooppunten van het cluster, evenals de vereiste netwerkconfiguraties.

2. Klik met de rechtermuisknop op het clusterknooppunt in **Cloud Explorer**en kies **Debugger koppelen**

    ![Debugger koppelen][attachdebugger]

3. In het dialoogvenster **koppelen om te verwerken** , kiest u het proces dat u fouten wilt opsporen en klikt u op **koppelen**

    ![Proces kiezen][chooseprocess]

    De naam van het proces dat u koppelen wilt, is gelijk aan de naam van de assembly-naam van uw service-project.

    De debugger zal toevoegen aan alle knooppunten waarop het proces wordt uitgevoerd.
    - In het geval waarin u fouten van een staatloos service opsporen wilt uitmaken alle exemplaren van de service op alle knooppunten deel van de debug-sessie.
    - Als u fouten een stateful-service opspoort, worden alleen de primaire replica van de partitie actief en daarom gevangen door het foutopsporingsprogramma. Als de primaire replica tijdens de debug-sessie verplaatst, wordt de verwerking van deze replica nog steeds deel uitmaken van de debug-sessie.
    - Om werkelijk alleen relevante partities of exemplaren van een bepaalde service, kunt u voorwaardelijke onderbrekingspunten om alleen een specifieke partitie of -instantie.

    ![Voorwaardelijke onderbrekingspunt][conditionalbreakpoint]

    > [AZURE.NOTE] Momenteel ondersteuning we geen voor foutopsporing van een Service Fabric '-cluster met meerdere exemplaren van dezelfde service uitvoerbare naam.

4. Zodra u klaar bent met het opsporen van fouten in uw toepassing, kunt u de externe foutopsporing extensie uitschakelen door met de rechtermuisknop op het cluster in de **Cloud Explorer** en kies **Foutopsporing uitschakelen**

    ![Foutopsporing op afstand uitschakelen][disableremotedebugging]

## <a name="streaming-traces-from-a-remote-cluster-node"></a>Sporen van een clusterknooppunt externe streaming

Bent u ook kunnen op sporen van stroom rechtstreeks van een externe clusterknooppunt naar Visual Studio. Deze functie kunt u de stream ETW traceringsgebeurtenissen, geproduceerd op een clusterknooppunt Fabric Service rechtstreeks in Visual Studio.

> [AZURE.NOTE] De functie vereist [Service Fabric SDK 2.0](http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015) en de [SDK voor .NET 2,9 Azure](https://azure.microsoft.com/downloads/).

<!-- -->
> [AZURE.WARNING]Sporen streaming is bedoeld voor scenario's dev/test en niet moet worden gebruikt in een productieomgeving, omdat de impact op de actieve toepassingen.
> In een productiescenario, moet u vertrouwen op gebeurtenissen met Azure diagnostische gegevens doorsturen.

1. Ga naar uw cluster in **Cloud Explorer**, klik met de rechtermuisknop en kiest u **Streaming sporen inschakelen**

    ![Externe streaming sporen inschakelen][enablestreamingtraces]

    Dit zal uitgangspunt voor het proces van het inschakelen van de streaming sporen-extensie op de knooppunten van het cluster, evenals de vereiste netwerkconfiguraties.

2. Vouw de **knooppunten** element in de **Cloud Explorer**, klik met de rechtermuisknop op het knooppunt dat u wilt streamen sporen uit en kiest u **Weergave-sporen Streaming**

    ![Externe streaming sporen weergeven][viewremotestreamingtraces]

    Herhaal stap 2 voor zoveel knooppunten als u wilt zien van de sporen van. Elke gegevensstroom knooppunten wordt in een speciaal venster weergegeven.

    U bent nu de traces uitgestoten door Fabric-Service en de services te zien. Als u filteren wilt, zodat alleen een specifieke toepassing vinden de gebeurtenissen, typ gewoon de naam van de toepassing in het filter.

    ![Sporen streaming weergeven][viewingstreamingtraces]

4. Als u klaar bent streaming sporen van het cluster, kunt u externe streaming sporen, uitschakelen door met de rechtermuisknop op het cluster in de **Cloud Explorer** en kies **Streaming sporen uitschakelen**

    ![Externe streaming sporen uitschakelen][disablestreamingtraces]

## <a name="next-steps"></a>Volgende stappen

- [Test een service Service Fabric](service-fabric-testability-overview.md).
- [Beheren van uw Service Fabric-toepassingen in Visual Studio](service-fabric-manage-application-in-visual-studio.md).

<!--Image references-->
[startdebugging]: ./media/service-fabric-debugging-your-application/startdebugging.png
[diagnosticevents]: ./media/service-fabric-debugging-your-application/diagnosticevents.png
[viewdiagnosticevents]: ./media/service-fabric-debugging-your-application/viewdiagnosticevents.png
[diagnosticeventsactions]: ./media/service-fabric-debugging-your-application/diagnosticeventsactions.png
[breakpoint]: ./media/service-fabric-debugging-your-application/breakpoint.png
[enableremotedebugging]: ./media/service-fabric-debugging-your-application/enableremotedebugging.png
[attachdebugger]: ./media/service-fabric-debugging-your-application/attachdebugger.png
[chooseprocess]: ./media/service-fabric-debugging-your-application/chooseprocess.png
[conditionalbreakpoint]: ./media/service-fabric-debugging-your-application/conditionalbreakpoint.png
[disableremotedebugging]: ./media/service-fabric-debugging-your-application/disableremotedebugging.png
[enablestreamingtraces]: ./media/service-fabric-debugging-your-application/enablestreamingtraces.png
[viewingstreamingtraces]: ./media/service-fabric-debugging-your-application/viewingstreamingtraces.png
[viewremotestreamingtraces]: ./media/service-fabric-debugging-your-application/viewremotestreamingtraces.png
[disablestreamingtraces]: ./media/service-fabric-debugging-your-application/disablestreamingtraces.png
