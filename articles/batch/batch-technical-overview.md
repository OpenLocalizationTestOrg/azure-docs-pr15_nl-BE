<properties
    pageTitle="Basisbeginselen van Azure Batch service | Microsoft Azure"
    description="Informatie over het gebruik van de Batch Azure service voor grootschalige parallel en HPC werkbelasting"
    services="batch"
    documentationCenter=""
    authors="mmacy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="batch"
    ms.workload="big-compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/22/2016"
    ms.author="marsma"/>

# <a name="basics-of-azure-batch"></a>Basisbeginselen van Azure Batch

Azure Batch kunt u efficiënt grote parallelle en high-performance computing (HPC) toepassingen uitvoeren in de cloud. Het is een platform-service die intensieve werk uit te voeren op een beheerde collectie van virtuele machines wordt gepland en kan automatisch schalen berekenen middelen aan de behoeften van uw taken.

Met de Batch-service, kunt u bronnen voor het uitvoeren van uw toepassingen tegelijkertijd en op schaal Azure compute definiëren. U kunt uitvoeren op de vraag of geplande taken en u hoeft niet handmatig maken, configureren, en beheren van een HPC-cluster, afzonderlijke virtuele computers, virtuele netwerken of een complexe taak en taak plannen infrastructuur.

## <a name="use-cases-for-batch"></a>Use-cases voor Batch

Batch is een beheerde Azure service die wordt gebruikt voor *batch-verwerking* of *batch computing*--met een grote hoeveelheid gelijksoortige taken om sommige gewenste resultaat te krijgen. Batch computing wordt meestal gebruikt door organisaties die regelmatig verwerken, transformeren en analyseren van grote hoeveelheden gegevens.

Batch werkt goed intrinsiek parallelle (ook bekend als ' embarrassingly parallelle') toepassingen en werklast. Intrinsiek parallelle werklasten zijn gemakkelijk opsplitsen in meerdere taken tegelijkertijd werk uitvoeren op meerdere computers.

![Parallelle taken.][1]<br/>

Enkele voorbeelden van de werklast die vaak worden verwerkt met behulp van deze methode zijn:

* Modellering van de financiële risico 's
* Klimaat en hydrologische gegevensanalyse
* Weergave van afbeeldingen, analyse en verwerking
* Media-codering en transcodering
* Volgorde van genetische analyse
* Engineering stress analyse
* Testen van software

Batch ook parallelle berekeningen met een stap verminderen aan het eind, en meer complexe HPC werkbelasting zoals [Message Passing Interface (MPI)](batch-mpi.md) -toepassingen worden uitgevoerd.

Zie voor een vergelijking tussen de partij en andere opties van de HPC-oplossing in Azure, [Batch en HPC-oplossingen](batch-hpc-solutions.md).

## <a name="developing-with-batch"></a>Ontwikkelen met Batch

Parallelle werkbelasting met Batch processing wordt meestal gedaan via programmacode met behulp van een van de [Batch-API's](#batch-development-apis). Met de Batch-API's u maken en beheren van toepassingen van computerknooppunten (virtuele machines) en plannen van taken en taken uit te voeren op die knooppunten. Een clienttoepassing of een service die u maakt gebruik van de Batch-API's om te communiceren met de Batch-service.

U kunt efficiënt grootschalige werklasten verwerken voor uw organisatie of een front-end voor de service aan uw klanten biedt, zodat ze projecten en taken – op verzoek of volgens een planning--op één, honderden of zelfs duizenden knooppunten uitvoeren kunnen. U kunt ook de batchverwerking gebruiken als onderdeel van een grotere werkstroom beheerd door hulpmiddelen zoals [Azure Data Factory](../data-factory/data-factory-data-processing-using-batch.md).

> [AZURE.TIP] Wanneer u klaar om zich te verdiepen bent de Batch-API voor een meer diepgaande informatie over de mogelijkheden van die dit programma, controleert u het [overzicht van Batch-functie voor ontwikkelaars](batch-api-basics.md).

### <a name="azure-accounts-youll-need"></a>Azure accounts die u nodig hebt

Wanneer u de batchverwerking oplossingen ontwikkelt, gebruikt u de volgende accounts in Microsoft Azure.

- **Azure-account en abonnement** - als u niet al een Azure-abonnement hebt, kunt u uw [MSDN-abonnee voordeel][msdn_benefits], of u aanmelden voor een [gratis account voor Azure][free_account]. Als u een account maakt, wordt er een standaardabonnement voor u gemaakt.

- **Batch-account** - uw toepassingen communiceren met de Batch-service, de naam, de URL van de rekening en een toegangstoets worden referenties gebruikt. De Batch-bronnen zoals pools, knooppunten, taken, berekenen en taken zijn gekoppeld aan een Batch-account. U kunt [Batch-account maken](batch-account-create-portal.md) in de portal Azure.

- **Opslag account** - Batch beschikt over ingebouwde ondersteuning voor het werken met bestanden in [Azure opslag][azure_storage]. Bijna elke Batch scenario gebruikt Azure opslag--voor de fasering van de programma's die uw taken uitvoeren en de gegevens die zij verwerken, en voor de opslag van gegevens over de output die ze genereren. Zie [over Azure opslag rekeningen](./../storage/storage-create-storage-account.md)een opslag-account maken.

### <a name="batch-development-apis"></a>Batch-API's voor ontwikkeling

De toepassingen en services kunnen verlenen directe REST API-aanroepen, gebruik een of meer van de volgende clientbibliotheken of een combinatie van beide voor het beheren van bronnen en uitvoeren van parallelle taken op schaal met behulp van de Batch-service berekenen.

| API    | API-Naslaggids | Downloaden | Codevoorbeelden |
| ----------------- | ------------- | -------- | ------------ |
| **REST partij** | [MSDN][batch_rest] | N.V.T. | [MSDN][batch_rest] |
| **Batch .NET**    | [MSDN][api_net] | [NuGet][api_net_nuget] | [GitHub][api_sample_net] |
| **Batch-Python**  | [readthedocs.IO][api_python] | [PyPI][api_python_pypi] |[GitHub][api_sample_python] |
| **Batch Node.js** | [github.IO][api_nodejs] | [npm][api_nodejs_npm] | - |
| **Batch-Java** (voorbeeld) | [github.IO][api_java] | [Maven][api_java_jar] | [GitHub][api_sample_java] |

### <a name="batch-resource-management"></a>Batch Resourcemanagement

Naast de client-API kun je de volgende bronnen binnen uw Batch account beheren.

- [PowerShell cmdlets batch][batch_ps]: de Azure Batch cmdlets in de module [Azure PowerShell](../powershell-install-configure.md) kunt u Batch resources met PowerShell beheren.

- [CLI Azure](../xplat-cli-install.md): de Azure opdrachtregelinterface (CLI Azure) is een cross-platform toolset shell-opdrachten waarmee u de interactie met veel Azure services, met inbegrip van de Batch.

- [Batch Management.NET](batch-management-dotnet.md) client library: ook beschikbaar via [NuGet][api_net_mgmt_nuget], kunt u de bibliotheek Batch Management .NET client programmatisch beheren van accounts, quota en toepassingspakketten Batch. Verwijst naar de bibliotheek management is op [MSDN][api_net_mgmt].

### <a name="batch-tools"></a>Batch-hulpprogramma 's

Niet vereist voor het bouwen van oplossingen met Batch, maar hier zijn enkele waardevolle hulpmiddelen bij het bouwen en het opsporen van fouten in de Batch-toepassingen en services te gebruiken.

 - [Azure portal][portal]: maken, controleren en verwijderen van toepassingen, taken en taken in de portal Azure Batch blades Batch. U kunt de statusgegevens voor deze bekijken en andere bronnen die tijdens het uitvoeren van uw taken en zelfs downloaden van bestanden van de compute nodes in uw toepassingen (downloaden van een mislukte taak `stderr.txt` bij het oplossen van problemen, bijvoorbeeld). U kunt ook downloaden van bestanden van RDP (Remote Desktop) die u gebruiken kunt voor aanmelding bij het berekenen van de knooppunten.

 - [Azure Batch Explorer][batch_explorer]: Batch Explorer biedt vergelijkbare resource management batchfunctie als de Azure portal, maar in een zelfstandige toepassing voor client Windows Presentation Foundation (WPF). Een van de voorbeeldtoepassingen voor Batch .NET op [GitHub][github_samples], kunt u deze gebruiken om te bladeren en de bronnen in uw account Batch beheren terwijl u ontwikkelen en testen van uw oplossingen Batch en bouwen met Visual Studio 2015 of hoger. Taak weergeven van toepassingen en de details, downloaden van bestanden van computerknooppunten en extern verbinding maken met knooppunten via RDP (Remote Desktop) bestanden die kunt u downloaden met Batch Explorer.

 - [Microsoft Azure Opslagverkenner][storage_explorer]: tijdens het niet strikt een Batch Azure tool de Opslagverkenner is een ander waardevol hulpmiddel om tijdens het ontwikkelen en debuggen van uw oplossingen Batch.

## <a name="scenario-scale-out-a-parallel-workload"></a>Scenario: Schaal van een parallelle werkbelasting

Een algemene oplossing die de Batch-API's gebruikt om te communiceren met de Batch-service omvat intrinsiek parallelle werk--zoals de weergave van afbeeldingen voor 3D-scènes--op een pool van computerknooppunten schalen. Deze groep met computerknooppunten uw 'render farm' kan zijn die bijvoorbeeld biedt tientallen, honderden of zelfs duizenden cores aan uw project weergeven.

In het volgende diagram ziet u een algemene Batch workflow met een clienttoepassing of een gehoste service met Batch voor het uitvoeren van een parallelle werkbelasting.

![Batch-oplossing-workflow][2]

In dit scenario gemeenschappelijke verwerkt uw toepassing of service een rekenkundige werkbelasting in Azure Batch door de volgende stappen uit te voeren:

1. Upload de **invoerbestanden** en de **toepassing** die de bestanden op uw account Azure opslag worden verwerkt. De input-bestanden kunnen worden alle gegevens die door uw toepassing worden verwerkt, zoals financiële modellering gegevens of bestanden worden getranscodeerd. Bestanden voor de toepassing is een toepassing die wordt gebruikt voor het verwerken van de gegevens, zoals een 3D-beeldopbouw of media transcoder.

2. Een Batch- **toepassingen** maken van computerknooppunten in uw Batch-account deze knooppunten worden de virtuele machines die taken worden uitgevoerd. U geeft eigenschappen voor de [grootte van het knooppunt](./../cloud-services/cloud-services-sizes-specs.md), het besturingssysteem en de locatie in Azure opslag van de toepassing te installeren wanneer de knooppunten deelnemen aan de groep (de toepassing die u hebt geüpload in stap #1). U kunt ook de groep van toepassingen voor het [automatisch schalen](batch-automatic-scaling.md)--dynamisch aanpassen van het aantal knooppunten in de pool--in reactie op de werklast die uw taken genereren berekenen.

3. Maak een **taak** uitvoert om de werkbelasting op de groep met computerknooppunten uitgevoerd. Wanneer u een taak maakt, kunt u deze koppelen aan een Batch-toepassingen.

4. **Taken** toevoegen aan het project. Als u taken aan een project toevoegt, de Batch-service automatisch de taken worden gepland voor uitvoering op de compute nodes in de groep. Elke taak wordt de toepassing die u hebt geüpload voor het verwerken van de invoerbestanden.

    - 4a. Voordat een taak wordt uitgevoerd, kan deze gegevens (de invoerbestanden) die het proces van de compute node die is toegewezen aan downloaden. Als de toepassing niet al is geïnstalleerd op het knooppunt (Zie stap #2), kan worden gedownload hier in plaats daarvan. Als het downloaden voltooid is, worden de taken uitgevoerd op hun toegewezen knooppunten.

5. Als de taken worden uitgevoerd, kunt u de batchverwerking om de voortgang van de taak en de bijbehorende taken kunt zoeken. Uw toepassing of service communiceert met de Batch-service via HTTPS en omdat u duizenden op duizenden computerknooppunten taken controleren mogelijk, zorg ervoor dat u [efficiënt opvragen van de Batch-service](batch-efficient-list-queries.md).

6. Als de taken die voltooid, kunnen ze hun gegevens uploaden naar Azure opslag. U kunt ook bestanden rechtstreeks vanuit computerknooppunten ophalen.

7. Wanneer de controle vaststelt dat de taken in uw project hebt voltooid, kan uw toepassing of service uitvoergegevens voor verdere verwerking of evaluatie downloaden.

Houd in gedachten dat slechts enkele Batch gebruiken en dit scenario wordt beschreven dat slechts enkele van de beschikbare functies. Zo kunt u [meerdere taken tegelijkertijd](batch-parallel-node-tasks.md) uitvoeren op elk computerknooppunt en u kunt [voorbereiden en de voltooiing van taken](batch-job-prep-release.md) voorbereiden van de knooppunten voor uw taken en klik daarna opruimen.

## <a name="next-steps"></a>Volgende stappen

Nu u een overzicht van de Batch-service hebt, is het tijd voor meer informatie over hoe u het kunt gebruiken voor het verwerken van uw computerintensieve parallelle werklasten achtergrondinformatie.

- Lees het [overzicht van Batch-functie voor ontwikkelaars](batch-api-basics.md), essentiële informatie voor iedereen die voorbereidingen voor het gebruik van de Batch. Het artikel bevat meer gedetailleerde informatie over Batch serviceresources zoals pools, knooppunten, projecten en taken en de vele API-functies die u gebruiken kunt bij het samenstellen van uw toepassing Batch.

- [Aan de slag met de bibliotheek Azure Batch voor .NET](batch-dotnet-get-started.md) voor informatie over het gebruik van C# en de Batch .NET-bibliotheek voor het uitvoeren van een eenvoudige werkbelasting met behulp van een gemeenschappelijke Batch workflow. In dit artikel moet een van uw eerste stopt tijdens het leren gebruiken van de Batch-service. Er is ook een [Python versie](batch-python-tutorial.md) van de zelfstudie.

- Downloaden van de [codevoorbeelden op GitHub] [ github_samples] om te zien hoe zowel C# en Python kan verbinding maken met Batch te plannen en proces monster werkbelastingen.

- De [Batch Learning pad] uitchecken[ learning_path] om een idee krijgen van de beschikbare middelen aan u als u leren werken met Batch.

[azure_storage]: https://azure.microsoft.com/services/storage/
[api_java]: http://azure.github.io/azure-sdk-for-java/
[api_java_jar]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-batch%22
[api_net]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_nuget]: https://www.nuget.org/packages/Azure.Batch/
[api_net_mgmt]: https://msdn.microsoft.com/library/azure/mt463120.aspx
[api_net_mgmt_nuget]: https://www.nuget.org/packages/Microsoft.Azure.Management.Batch/
[api_nodejs]: http://azure.github.io/azure-sdk-for-node/azure-batch/latest/
[api_nodejs_npm]: https://www.npmjs.com/package/azure-batch
[api_python]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.html
[api_python_pypi]: https://pypi.python.org/pypi/azure-batch
[api_sample_net]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp
[api_sample_python]: https://github.com/Azure/azure-batch-samples/tree/master/Python/Batch
[api_sample_java]: https://github.com/Azure/azure-batch-samples/tree/master/Java/
[batch_ps]: https://msdn.microsoft.com/library/azure/mt125957.aspx
[batch_rest]: https://msdn.microsoft.com/library/azure/Dn820158.aspx
[free_account]: https://azure.microsoft.com/free/
[github_samples]: https://github.com/Azure/azure-batch-samples
[learning_path]: https://azure.microsoft.com/documentation/learning-paths/batch/
[msdn_benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[batch_explorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[storage_explorer]: http://storageexplorer.com/
[portal]: https://portal.azure.com

[1]: ./media/batch-technical-overview/tech_overview_01.png
[2]: ./media/batch-technical-overview/tech_overview_02.png
