<properties
    pageTitle="Load testen van de toepassing met behulp van Visual Studio Team Services | Microsoft Azure"
    description="Meer informatie over het testen uw Azure Service Fabric-toepassingen met behulp van Visual Studio Team Services."
    services="service-fabric"
    documentationCenter="na"
    authors="cawams"
    manager="timlt"
    editor="" />

<tags
    ms.service="multiple"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="multiple"
    ms.date="07/29/2016"
    ms.author="cawa" />

# <a name="load-test-your-application-by-using-visual-studio-team-services"></a>Load testen van de toepassing met behulp van Visual Studio Team Services

In dit artikel wordt het gebruik van Microsoft Visual Studio load test functies testen een toepassing weergegeven. Een Azure Service Fabric stateful service back-end en een service van stateless front-end voor het web wordt gebruikt. Het voorbeeld van de toepassing gebruikt hier is een locatie vliegtuig simulator. U opgeven een vliegtuig-ID, tijdstip van vertrek en bestemming. De aanvragen worden verwerkt door de toepassing van de back-end en front-end geeft op een kaart van het vliegtuig dat voldoet aan de criteria.

In het volgende diagram illustreert de toepassing Fabric-Service die u zult worden testen.

![Diagram van de toepassing voorbeeld vliegtuig locatie][0]

## <a name="prerequisites"></a>Vereisten
Voordat u aan de slag, moet u het volgende doen:

- Een Visual Studio Team Services-account aanvragen. U kunt een gratis voor [Visual Studio Team Services](https://www.visualstudio.com).
- Ophalen en installeren van Visual Studio 2013 of Visual Studio 2015. In dit artikel 2015 met Visual Studio Enterprise edition gebruikt, maar Visual Studio 2013 en andere versies op dezelfde manier moeten werken.
- Uw toepassing kan een staging-omgeving implementeren. Zie [het implementeren van toepassingen op een externe cluster met behulp van Visual Studio](service-fabric-publish-app-remote-cluster.md) voor meer informatie hierover.
- Begrijpen van uw toepassing gebruikspatroon. Deze informatie wordt gebruikt voor het simuleren van het patroon van de belasting.
- De doelstelling voor het testen van uw werklast begrijpen. Dit helpt u bij het interpreteren en analyseren van de resultaten laden.

## <a name="create-and-run-the-web-performance-and-load-test-project"></a>Maken en uitvoeren van het project Web-prestaties en Load-Test

### <a name="create-a-web-performance-and-load-test-project"></a>Maak een project webprestaties en Load-Test

1. Open de Visual Studio 2015. Kies **bestand** > **Nieuw** > **Project** in het menu te openen in het dialoogvenster **Nieuw Project** .

2. Vouw het knooppunt uit **Visual C#** en kies **Test** > **webprestaties en Load Test project**. Geef het project een naam en klik op **OK** .

    ![Schermafdruk van het dialoogvenster Nieuw Project][1]

    Hier ziet u een nieuw webprestaties en Load Test project in de Solution Explorer.

    ![Schermafdruk van Solution Explorer met het nieuwe project][2]

### <a name="record-a-web-performance-test"></a>Record een prestatietest web

1. Open het project .webtest.

2. Kies het pictogram **Opname toevoegen** aan een opnamesessie starten in de browser.

    ![Schermafdruk van het pictogram opname toevoegen in een browser][3]

    ![Schermafdruk van de knop opnemen in een browser][4]

3. Ga naar de application Service Fabric. De webverzoeken moet worden weergegeven in het deelvenster vastleggen.

    ![Schermafdruk van webaanvragen in het deelvenster vastleggen][5]

4. Uitvoeren van een reeks acties die u verwacht dat de gebruikers uit te voeren. Deze acties worden gebruikt als een patroon voor het genereren van de belasting.

5. Wanneer u bent klaar, kiest u de knop **stoppen** om te stoppen met opnemen.

    ![Schermafdruk van de knop stoppen][6]

    Het .webtest-project in Visual Studio moet een reeks aanvragen zijn vastgelegd. Dynamische parameters worden automatisch vervangen. Op dit moment kunt u extra, herhaalde afhankelijkheid verzoeken die geen deel uitmaken van uw testscenario verwijderen.

6. Sla het project op en kies vervolgens de opdracht **Testen uitvoeren** voor de prestatietest web lokaal uitvoeren en controleer of dat alles goed werkt.

    ![Schermafdruk van de opdracht Test uitvoeren][7]

### <a name="parameterize-the-web-performance-test"></a>De prestatietest web voorzien

U kunt de prestatietest web voorzien door te converteren naar een gecodeerde web performance test en vervolgens de code te bewerken. Als alternatief kunt u de prestatietest web binden aan een gegevenslijst met zodat de test worden de gegevens doorlopen. Zie [genereren en uitvoeren van een test waarbij de prestaties gecodeerde web](https://msdn.microsoft.com/library/ms182552.aspx) voor meer informatie over de prestatietest voor web converteren naar een gecodeerde test. Zie [toevoegen een gegevensbron om de prestaties van een webpagina testen](https://msdn.microsoft.com/library/ms243142.aspx) voor informatie over het koppelen van gegevens naar een web-prestatietest.

In dit voorbeeld zullen we de prestatietest voor web converteren naar een gecodeerde test zodat u kunt de ID van het vliegtuig door een gegenereerde GUID vervangen en meer aanvragen voor het verzenden van vluchten naar verschillende locaties toevoegen.

### <a name="create-a-load-test-project"></a>Maak een load testproject

Een test het project bestaat uit een of meer scenario's beschreven door de prestatietest voor web en testen van eenheden, samen met de extra belasting van de opgegeven test instellingen. De volgende stappen laten zien hoe een load testproject te maken:

1. Kies **toevoegen**in het snelmenu van het project Web-prestaties en Load-Test, > **Load Test**. Kies de knop **volgende** om de instellingen testen in de wizard **Load testen** .

2. Kies of u wilt een constant gebruiker laden of een stap die begint met een paar gebruikers en de gebruikers na verloop van tijd verhoogt belasting in de sectie **Geladen patroon** .

    Als u een goede schatting van het bedrag van de belasting van de gebruiker hebt en wilt zien hoe het huidige systeem uitvoert, kiest u **Constant laden**. Als u wilt weten of het systeem voortdurend onder verschillende ladingen uitvoert, kiest u **Load stap**.

3. Kies de knop **toevoegen** en selecteer de test die u wilt opnemen in de load test in het gedeelte **Test Mix** . U kunt de kolom **verdeling** geeft het percentage van de totale tests uitvoeren voor elke test.

4. Geef de duur van de test load in de sectie **Instellingen uitvoeren** .

    >[AZURE.NOTE] De **Test iteraties** optie is alleen beschikbaar als u een lokaal met behulp van Visual Studio load-test uitvoeren.

5. Geef de locatie op waar load test aanvragen worden gegenereerd in de sectie **locatie** van **Instellingen uitvoeren**. De wizard kan u zich aanmelden bij uw account Team Services gevraagd. Log in en kies een geografische locatie. Wanneer u klaar bent, klik op de knop **Voltooien** .

6. Nadat de load-test is gemaakt, opent u het .loadtest-project en kiest u de huidige instelling, zoals **Instellingen uitvoeren**uitvoeren > **Settings1 uitvoeren [actief]**. Hiermee opent u de instellingen uitvoeren in het venster **Eigenschappen** .

7. In de sectie van de **resultaten** van het eigenschappenvenster van de **Instellingen worden uitgevoerd** , moet de instelling van de **Timing Details opslag** **geen** hebben als standaardwaarde. Deze waarde wijzigen in **Alle afzonderlijke Details** voor meer informatie het laden van de testresultaten. Zie [Laden testen](https://www.visualstudio.com/load-testing.aspx) voor meer informatie over verbinding maken met Visual Studio Team Services en een load-test uitvoeren.

### <a name="run-the-load-test-by-using-visual-studio-team-services"></a>De load-test uitvoert met behulp van Visual Studio Team Services

Kies de opdracht **Load testen uitgevoerd** start van de test worden uitgevoerd.

![Schermafdruk van de opdracht Load Test uitvoeren][8]

## <a name="view-and-analyze-the-load-test-results"></a>Weergeven en analyseren van de resultaten laden

Als de belasting testen vordert, prestatiegegevens voor de grafiek wilt weergeven. Zoals in het volgende diagram weergegeven.

![Schermafdruk van een grafiek van uw webprestaties voor resultaten laden][9]

1. Kies de koppeling **Download rapport** aan de bovenkant van de pagina. Nadat het rapport wordt gedownload, klik op de knop **rapport weergeven** .

    Klik op het tabblad **grafiek** kunt u grafieken voor verschillende prestatiemeteritems te bekijken. Op het tabblad **Samenvatting** het totale resultaten worden weergegeven. Het tabblad **tabellen** ziet u het totale aantal tests doorgegeven en laden.

2. Kies het aantal koppelingen op de **Test** > **is mislukt** en de **fouten** > **aantal** kolommen voor een overzicht van de details van deze fout.

    Het tabblad **Details** bevat virtuele gebruikers- en informatie over scenario's voor mislukte aanvragen. Deze gegevens kunnen zijn handig als de belasting test meerdere scenario's bevat.

Zie [Resultaten voor het laden van testen analyseren in de weergave van de grafieken van de laden Test Analyzer](https://www.visualstudio.com/load-testing.aspx) voor meer informatie over het weergeven van resultaten laden.

## <a name="automate-your-load-test"></a>De belasting testen automatiseren

Visual Studio Team Services laden Test biedt API's waarmee u load tests beheren en analyseren van resultaten in een Team Services-account. Zie [Wolk Load testen Rest-API's](http://blogs.msdn.com/b/visualstudioalm/archive/2014/11/03/cloud-load-testing-rest-apis-are-here.aspx) voor meer informatie.

## <a name="next-steps"></a>Volgende stappen
- [Controle en diagnose van services in de instellingen voor een lokale computer ontwikkeling](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[0]: ./media/service-fabric-vso-load-test/OverviewDiagram.png
[1]: ./media/service-fabric-vso-load-test/NewProjectDialog.png
[2]: ./media/service-fabric-vso-load-test/Project.png
[3]: ./media/service-fabric-vso-load-test/AddRecording.png
[4]: ./media/service-fabric-vso-load-test/AddRecording2.png
[5]: ./media/service-fabric-vso-load-test/ActionSequence.png
[6]: ./media/service-fabric-vso-load-test/StopRecording.png
[7]: ./media/service-fabric-vso-load-test/RunTest.png
[8]: ./media/service-fabric-vso-load-test/RunTest2.png
[9]: ./media/service-fabric-vso-load-test/Graph.png
