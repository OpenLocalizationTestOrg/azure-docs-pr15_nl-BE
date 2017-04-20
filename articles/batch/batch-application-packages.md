<properties
    pageTitle="Van eenvoudige toepassingsinstallatie en beheer in Azure Batch | Microsoft Azure"
    description="Gebruik de functie application pakketten van Azure Batch eenvoudig beheren van meerdere toepassingen en versies voor installatie op Batch computerknooppunten."
    services="batch"
    documentationCenter=".net"
    authors="mmacy"
    manager="timlt"
    editor="" />

<tags
    ms.service="batch"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows"
    ms.workload="big-compute"
    ms.date="10/21/2016"
    ms.author="marsma" />

# <a name="application-deployment-with-azure-batch-application-packages"></a>Implementatie van toepassingen met Azure Batch toepassingspakketten

De functie application pakketten van Batch Azure biedt eenvoudig beheer van toepassingen van de taak en de implementatie ervan aan de compute nodes in uw groep. U kunt met toepassingspakketten, uploaden en beheren van meerdere versies van de toepassingen die taken uitvoeren, met inbegrip van de ondersteunende bestanden. U kunt vervolgens automatisch implementeren een of meer van deze toepassingen de compute nodes in uw groep.

In dit artikel leert u hoe u kunt uploaden en beheren van toepassingspakketten in Azure portal. Vervolgens leert u hoe ze te installeren op een groep van toepassingen computerknooppunten met de [Batchverwerking .NET] [ api_net] bibliotheek.

> [AZURE.NOTE] De toepassing pakketten functie die hier wordt beschreven, vervangt de functie 'Batch Apps' beschikbaar in vorige versies van de service.

## <a name="application-package-requirements"></a>Toepassingsvereisten pakket

Moet u [een opslag Azure-account koppelen](#link-a-storage-account) aan uw account voor Batch toepassingspakketten gebruiken.

De toepassing pakketten functie die in dit artikel besproken is compatibel *alleen* met Batch van toepassingen die zijn gemaakt na 10 maart 2016. Toepassingspakketten worden berekent u knooppunten in toepassingen die zijn gemaakt vóór deze datum niet geïmplementeerd.

Deze functie is geïntroduceerd in [Batch REST API] [ api_rest] versie 2015-12-01.2.2 en de bijbehorende [.NET Batch] [ api_net] bibliotheek versie 3.1.0. Wij raden aan dat u gebruik altijd de meest recente versie van de API bij het werken met Batch.

> [AZURE.IMPORTANT] Op dit moment ondersteunen alleen *CloudServiceConfiguration* pools toepassingspakketten. U kunt toepassingspakketten niet gebruiken in toepassingen die zijn gemaakt met behulp van afbeeldingen van VirtualMachineConfiguration. Zie de sectie [configuratie van de virtuele machine](batch-linux-nodes.md#virtual-machine-configuration) van [Linux bepaling computerknooppunten in Azure Batch van toepassingen](batch-linux-nodes.md) voor meer informatie over de twee verschillende configuraties.

## <a name="about-applications-and-application-packages"></a>Over toepassingen en toepassingspakketten

Binnen de Batch in Azure verwijst een *toepassing* naar een set van versioned binaire bestanden die automatisch kunnen worden gedownload naar de compute nodes in uw groep. Een *toepassingspakket* verwijst naar een *specifieke set* van de binaire getallen en een bepaalde *versie* van de toepassing vertegenwoordigt.

![-Diagram op hoog niveau van toepassingen en toepassingspakketten][1]

### <a name="applications"></a>Toepassingen

Een toepassing in Batch bevat een of meer toepassing verpakt en bevat configuratie-opties voor de toepassing. Een toepassing kan bijvoorbeeld opgeven dat het standaard pakketversie van de toepassing te installeren op de compute nodes en de pakketten kunnen worden bijgewerkt of verwijderd.

### <a name="application-packages"></a>Toepassingspakketten

Een toepassingspakket is een ZIP-bestand met de binaire bestanden van toepassing en de ondersteunende bestanden die nodig voor de uitvoering van uw taken zijn. Elk toepassingspakket vertegenwoordigt een specifieke versie van de toepassing.

U kunt toepassingspakketten op het niveau van toepassingen en de taak opgeven. U kunt een of meer van deze pakketten en (optioneel) een versie bij het maken van een groep of een taak opgeven.

* **Toepassingspakketten toepassingen** zijn geïmplementeerd op *elk* knooppunt in de groep. Toepassingen worden ingezet wanneer een knooppunt wordt toegevoegd aan een groep, en wanneer deze wordt opgestart of reimaged.

    Toepassingspakketten toepassingen zijn geschikt als alle knooppunten in een groep taken van een taak uitvoert. Kunt u een of meer toepassingspakketten wanneer u een groep maken en u kunt toevoegen of bijwerken van een bestaande toepassingen-pakketten. Als u een bestaande toepassingen toepassingspakketten bijwerkt, moet u opnieuw de knooppunten om het nieuwe pakket te installeren.

* **Toepassingspakketten taak** zijn uitsluitend te implementeren op een compute node gepland voor een taak uitvoeren voordat u de opdrachtregel van de taak wordt uitgevoerd. Als de opgegeven toepassingspakket en de versie al op het knooppunt, niet opnieuw gedistribueerd en het bestaande pakket wordt gebruikt.

    Toepassingspakketten taak zijn handig in omgevingen met gedeelde toepassingen, waarbij verschillende taken worden uitgevoerd op een groep en de groep wordt niet verwijderd wanneer een taak is voltooid. Als uw project minder taken dan de knooppunten in de pool heeft, minimaliseren toepassingspakketten taak gegevensoverdracht sinds de toepassing is uitsluitend te implementeren op de knooppunten die taken uitvoeren.

    Andere scenario's die van toepassingspakketten taak profiteren kunnen taken zijn die een bijzonder grote toepassing gebruiken, maar voor een klein aantal taken. Een fase vóór verwerking of een taak waarbij de voorbehandeling of het samenvoegen van toepassing heavyweight is samenvoegen.

> [AZURE.IMPORTANT] Er gelden beperkingen voor het aantal toepassingen en toepassingspakketten binnen een Batch-account, als de maximale grootte van pakket. Zie [quota's en beperkingen voor de Batch Azure-service](batch-quota-limit.md) voor meer informatie over deze beperkingen.

### <a name="benefits-of-application-packages"></a>Voordelen van toepassingspakketten

Toepassingspakketten kunnen de code in uw oplossing Batch vereenvoudigen en minder overhead nodig is voor het beheren van toepassingen die de taken worden uitgevoerd.

Taak van de groep van toepassingen start hoeft te geven van een lange lijst van afzonderlijke resource bestanden te installeren op de knooppunten. U hebt geen handmatig beheren van meerdere versies van uw toepassingsbestanden in Azure opslag of op de knooppunten. En u hoeft niet bang te genereren van [SAS-URL's](../storage/storage-dotnet-shared-access-signature-part-1.md) voor toegang tot de bestanden in uw account voor opslag. Batch werkt op de achtergrond met Azure opslag toepassingspakketten opslaan en ze computerknooppunten implementeren.

## <a name="upload-and-manage-applications"></a>Uploaden en beheren van toepassingen

U kunt de [Azure portal] [ portal] of de bibliotheek [Batch Management.NET](batch-management-dotnet.md) toepassingspakketten in uw Batch account beheren. In de volgende secties, we eerst een opslag-account koppelen en vervolgens bespreken toepassingen toe te voegen en pakketten en ze met de portal beheren.

### <a name="link-a-storage-account"></a>Een opslag-account koppelen

Voor het gebruik van toepassingspakketten, moet u eerst een Azure opslag account koppelen aan uw account Batch. Als u niet de account van een opslag voor uw Batch-account hebt geconfigureerd, wordt de Azure portal een waarschuwing dat de eerste keer dat u klikt u op de tegel **toepassingen** in de blade **Batch account** weergegeven.

> [AZURE.IMPORTANT] Momenteel ondersteunt *alleen* de **algemene** rekening opslagtype batch zoals beschreven in stap 5, [een opslag-account maken](../storage/storage-create-storage-account.md#create-a-storage-account), in [Azure over opslag rekeningen](../storage/storage-create-storage-account.md). U koppelt een account Azure opslag voor uw Batch-account koppeling *alleen* een **algemene** opslag-account.

![Geen opslag-account geconfigureerd waarschuwing in Azure portal][9]

De Batch-service gebruikt de bijbehorende opslag account voor het opslaan en ophalen van toepassingspakketten. Nadat u twee accounts hebt gekoppeld, kan de pakketten die zijn opgeslagen in de gekoppelde opslag-account aan uw computerknooppunten automatisch in Batch implementeren. Klikt u op **Accountinstellingen voor opslag** in de blade **Waarschuwing** en klik vervolgens op **Account voor opslag** in de blade **Opslag Account** een opslag-account te koppelen aan uw account Batch.

![Kies opslag account blade in Azure portal][10]

Het is raadzaam een opslag account *specifiek* voor gebruik met uw account Batch maken en hier selecteert. Zie 'Een opslag-account maken' in [Azure over opslag accounts](../storage/storage-create-storage-account.md)voor meer informatie over het maken van een account voor de opslag. Nadat u een opslag-account hebt gemaakt, kunt u vervolgens deze koppelen aan uw account Batch met behulp van de bladeserver **Opslag Account** .

> [AZURE.WARNING] Omdat de Batch Azure opslag worden gebruikt voor het opslaan van uw toepassingspakketten, worden [in rekening gebracht als een normale] [ storage_pricing] voor het blok blob-gegevens. Moet u rekening houden met de grootte en het aantal van uw toepassingspakketten en verouderde pakketten te minimaliseren kosten regelmatig verwijderen.

### <a name="view-current-applications"></a>Huidige toepassingen weergeven

De toepassingen in uw Batch-account, klik op het **toepassingen** menu-item in het linkermenu tijdens het bekijken van de **rekening van de partij** -blade.

![Toepassingen naast elkaar][2]

Hiermee opent u het blad **toepassingen** :

![Lijst met toepassingen][3]

De blade **toepassingen** geeft de ID van elke toepassing in uw account en de volgende eigenschappen:

* **Pakketten**--het aantal versies dat aan deze toepassing is gekoppeld.
* De **standaardversie**--de versie die wordt geïnstalleerd als u geen versie opgeeft wanneer u de toepassing voor een groep instelt. Deze instelling is optioneel.
* **Updates toestaan**--met de waarde die aangeeft of het pakket met updates, verwijderingen en toevoegingen zijn toegestaan. Als deze is ingesteld op **Nee**, zijn pakket bijwerken en verwijderen uitgeschakeld voor de toepassing. Alleen nieuwe pakket versies van toepassingen kunnen worden toegevoegd. De standaardinstelling is **Ja**.

### <a name="view-application-details"></a>Details van toepassing

Klik op een toepassing in de blade **toepassingen** voor het openen van het blad met de details voor die toepassing.

![Toepassingdetails][4]

In de toepassing details blade, kunt u de volgende instellingen configureren voor uw toepassing.

* **Updates toestaan**--opgeven of de toepassingspakketten kunnen worden bijgewerkt of verwijderd. Zie 'Bijwerken of verwijderen van een toepassingspakket' verderop in dit artikel.
* **Standaardversie**--een toepassingspakket standaard te implementeren om computerknooppunten opgeven.
* **Weergegeven naam**--Geef een 'vriendelijke' naam die uw oplossing kunt gebruiken wanneer u dit item bevat informatie over de toepassing, zoals in de gebruikersinterface van een service waarmee u uw klanten via Batch Batch.

### <a name="add-a-new-application"></a>Een nieuwe toepassing toevoegen

Als u een nieuwe toepassing, een toepassingspakket toevoegen en een nieuwe, unieke id opgeven Het eerste pakket van toepassing die u met de ID van de nieuwe toepassing toevoegt maakt ook de nieuwe toepassing.

Klik op **toevoegen** op het blad **toepassingen** voor het openen van de **nieuwe toepassing** blade.

![Nieuwe toepassing blade in Azure portal][5]

De **nieuwe toepassing** -bladeserver biedt de volgende velden om op te geven van de instellingen van uw nieuwe toepassing en het toepassingspakket.

**Toepassings-id**

Dit veld bevat de ID van uw nieuwe toepassing die de standaard Azure Batchcode validatieregels:

* Kan een willekeurige combinatie van alfanumerieke tekens, inclusief koppeltekens en onderstrepingstekens bevatten.
* Kan niet meer dan 64 tekens bevatten.
* Moet uniek zijn binnen de Batch-account.
* Is het behouden van de aanvraag en geval ongevoelig zijn.

**Versie**

Hiermee geeft u de versie van de toepassingspakket dat u wilt uploaden. Versietekenreeksen, gelden de volgende regels voor gegevensvalidatie:

* Kan een willekeurige combinatie van alfanumerieke tekens, zoals afbreekstreepjes, onderstrepingstekens en punten bevatten.
* Kan niet meer dan 64 tekens bevatten.
* Moet uniek zijn binnen de toepassing.
* Geval behouden en is hoofdlettergevoelig.

**Toepassingspakket**

Dit veld bevat het ZIP-bestand met de binaire bestanden van de toepassing en de ondersteunende bestanden die nodig zijn voor het uitvoeren van de toepassing. Klik op het **selecteren van een bestand** of het pictogram van de map te bladeren en selecteert u een .zip-bestand met de bestanden van uw toepassing.

Nadat u een bestand hebt geselecteerd, klikt u op **OK** om te beginnen met het uploaden naar Azure opslag. Het uploaden is voltooid, krijgt u een melding als het blad wordt gesloten. Afhankelijk van de grootte van het bestand dat u wilt uploaden en de snelheid van de netwerkverbinding, kan deze bewerking enige tijd duren.

> [AZURE.WARNING] Sluit de **toepassing van nieuwe** blade niet voordat het uploaden voltooid is. Hierdoor wordt het uploadproces gestopt.

### <a name="add-a-new-application-package"></a>Een nieuwe toepassingspakket toevoegen

Om een nieuwe versie van toepassing-pakket voor een bestaande toepassing toevoegt, selecteert u een toepassing in de blade **toepassingen** , **pakketten**, klik op **toevoegen** om de bladeserver **pakket toevoegen** .

![Toepassing pakket blade in Azure portal toevoegen][8]

Zoals u ziet, de velden overeenkomen met die van de **nieuwe toepassing** blade, maar het vak **toepassings-id** is uitgeschakeld. Als voor de nieuwe toepassing, geeft u de **versie** voor uw nieuwe pakket, bladert u naar uw **toepassingspakket** ZIP-bestand en klik op **OK** om het pakket te uploaden.

### <a name="update-or-delete-an-application-package"></a>Bijwerken of verwijderen van een toepassingspakket

Wilt bijwerken of verwijderen van een bestaand toepassingspakket, de blade details voor de toepassing te openen, klikt u op **pakketten** de blade **pakketten** te openen, klikt u op de **drie puntjes** in de rij van het pakket van de toepassing die u wilt wijzigen en selecteer de actie die u wilt uitvoeren.

![Bijwerken of verwijderen van pakket in Azure portal][7]

**Update**

Als u op **bijwerken**klikt, wordt de blade *-updatepakket* weergegeven. Deze blade is vergelijkbaar met het *nieuwe pakket van toepassing* -blade echter alleen het pakket selectieveld is ingeschakeld, zodat u kunt opgeven van een nieuwe ZIP-bestand te uploaden.

![Blade in Azure portal pakket bijwerken][11]

**Verwijderen**

Wanneer u op **verwijderen**klikt, wordt u gevraagd het verwijderen van de Pakketversie te bevestigen en Batch wordt het pakket verwijderd uit de opslag van Azure. Als u de standaardversie van een toepassing verwijdert, wordt de instelling van de **standaardversie** van de toepassing verwijderd.

![Toepassing verwijderen][12]

## <a name="install-applications-on-compute-nodes"></a>Toepassingen installeren op computerknooppunten

U hebt gezien dat het beheren van pakketten met Azure portal application, bespreken we het implementeren van deze knooppunten te berekenen en ze met de batchverwerking taken uit te voeren.

### <a name="install-pool-application-packages"></a>Toepassingspakketten toepassingen installeren

Een om toepassingspakket te installeren op alle computerknooppunten in een groep, geeft u een of meer application package *verwijzingen* voor de groep. De toepassingspakketten die u voor een groep van toepassingen opgeeft worden geïnstalleerd op elk computerknooppunt dat knooppunt wordt toegevoegd aan de groep als wanneer het knooppunt wordt opnieuw opgestart of reimaged.

Geef een of meer [CloudPool]in Batch .NET,[net_cloudpool]. [ApplicationPackageReferences] [net_cloudpool_pkgref] wanneer u een nieuwe groep maken of een bestaande groep van toepassingen. De [ApplicationPackageReference] [ net_pkgref] -klasse geeft computerknooppunten een toepassings-ID en versie te installeren op een groep van toepassingen.

```csharp
// Create the unbound CloudPool
CloudPool myCloudPool =
    batchClient.PoolOperations.CreatePool(
        poolId: "myPool",
        targetDedicated: "1",
        virtualMachineSize: "small",
        cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "4"));

// Specify the application and version to install on the compute nodes
myCloudPool.ApplicationPackageReferences = new List<ApplicationPackageReference>
{
    new ApplicationPackageReference {
        ApplicationId = "litware",
        Version = "1.1001.2b" }
};

// Commit the pool so that it's created in the Batch service. As the nodes join
// the pool, the specified application package will be installed on each.
await myCloudPool.CommitAsync();
```

>[AZURE.IMPORTANT] Als een pakket distributie om een of andere reden mislukt, markeert u de Batch-service het knooppunt [onbruikbaar][net_nodestate], en geen taken gepland voor uitvoering op dat knooppunt. In dit geval moet u **opnieuw** het knooppunt voor de implementatie van het pakket te dubbelklikken. Het knooppunt opnieuw kan ook taak opnieuw plannen op het knooppunt.

### <a name="install-task-application-packages"></a>Taak toepassingspakketten installeren

Net als in een groep wordt u toepassing pakket *verwijzingen* voor een taak. Wanneer u een taak uit te voeren op een knooppunt, wordt het pakket gedownload en uitgepakt voordat de opdrachtregel van de taak wordt uitgevoerd. Als een opgegeven pakket en de versie al op het knooppunt is geïnstalleerd, wordt het pakket gedownload en het bestaande pakket wordt gebruikt.

Een taak toepassing om pakket te installeren, configureren van de taak [CloudTask][net_cloudtask]. [ApplicationPackageReferences] [net_cloudtask_pkgref] eigenschap:

```csharp
CloudTask task =
    new CloudTask(
        "litwaretask001",
        "cmd /c %AZ_BATCH_APP_PACKAGE_LITWARE%\\litware.exe -args -here");

task.ApplicationPackageReferences = new List<ApplicationPackageReference>
{
    new ApplicationPackageReference
    {
        ApplicationId = "litware",
        Version = "1.1001.2b"
    }
};
```

## <a name="execute-the-installed-applications"></a>De geïnstalleerde toepassingen uitvoeren

De pakketten die u hebt opgegeven voor een taak of een groep van toepassingen worden gedownload en uitgepakt naar de map met een naam in de `AZ_BATCH_ROOT_DIR` van het knooppunt. Batch maakt ook een variabele die het pad naar de map met de naam bevat. De regels van de opdracht taak gebruikt deze omgevingsvariabele wanneer wordt verwezen naar de toepassing op het knooppunt. De variabele is in de volgende notatie:

`AZ_BATCH_APP_PACKAGE_APPLICATIONID#version`

`APPLICATIONID`en `version` zijn de waarden die overeenkomen met de toepassing en pakket-versie die u hebt opgegeven voor de implementatie. Bijvoorbeeld, als u hebt opgegeven die versie 2.7 van toepassing *blender* moet worden geïnstalleerd, uw taak opdrachtregels gebruikt deze omgevingsvariabele toegang krijgen tot de bestanden:

`AZ_BATCH_APP_PACKAGE_BLENDER#2.7`

Als u een versie van de standaardwaarde voor een toepassing opgeeft, kunt u het versieachtervoegsel weglaten. Als u "2.7" ingesteld als de standaardversie voor toepassing *blender*, bijvoorbeeld taken kunnen verwijzen naar de volgende omgevingsvariabele en deze versie 2.7 wordt uitgevoerd:

`AZ_BATCH_APP_PACKAGE_BLENDER`

Het volgende codefragment ziet u een voorbeeld van de taak opdrachtregel de standaardversie van de *blender* -toepassing wordt gestart:

```csharp
string taskId = "blendertask01";
string commandLine =
    @"cmd /c %AZ_BATCH_APP_PACKAGE_BLENDER%\blender.exe -args -here";
CloudTask blenderTask = new CloudTask(taskId, commandLine);
```

> [AZURE.TIP] Zie de [omgevingsinstellingen voor taken](batch-api-basics.md#environment-settings-for-tasks) in het [overzicht van Batch-functie](batch-api-basics.md) voor meer informatie over de omgevingsinstellingen compute-knooppunt.

## <a name="update-a-pools-application-packages"></a>Updatepakketten van een groep van toepassingen

Als u al een bestaande groep van toepassingen is geconfigureerd met een toepassingspakket, kunt u een nieuw pakket voor de groep. Als u een nieuw pakket verwijzing voor een groep, wordt het volgende van toepassing opgeven:

* Alle nieuwe knooppunten die deelnemen aan de groep en elk bestaand knooppunt opnieuw gestart of reimaged installeert het nieuw opgegeven pakket.
* Berekent de knooppunten die zich al in de groep van toepassingen wanneer u het pakket verwijzingen bijwerken niet automatisch de nieuwe toepassingspakket installeert. Deze berekenen knooppunten moeten worden gestart of reimaged voor het ontvangen van het nieuwe pakket.
* Wanneer een nieuw pakket wordt geïmplementeerd, weerspiegelen de omgevingsvariabelen gemaakt in de nieuwe toepassing pakket verwijzingen.

In dit voorbeeld wordt de bestaande groep van toepassingen is versie 2.7 van de *blender* toepassing geconfigureerd als een van de [CloudPool][net_cloudpool]. [ApplicationPackageReferences] [net_cloudpool_pkgref]. Geef een nieuwe [ApplicationPackageReference] wilt bijwerken van de groep van toepassingen knooppunten met versie 2.76b,[ net_pkgref] met de nieuwe versie en de wijziging doorvoeren.

```csharp
string newVersion = "2.76b";
CloudPool boundPool = await batchClient.PoolOperations.GetPoolAsync("myPool");
boundPool.ApplicationPackageReferences = new List<ApplicationPackageReference>
{
    new ApplicationPackageReference {
        ApplicationId = "blender",
        Version = newVersion }
};
await boundPool.CommitAsync();
```

Nu dat de nieuwe versie is geconfigureerd, wordt een *Nieuw* knooppunt dat wordt toegevoegd aan de groep 2.76b geïmplementeerd in deze versie hebben. 2.76b installeren op de knooppunten die *al* in de pool, opnieuw opstarten of ze reimage. Houd er rekening mee dat knooppunten opnieuw opgestarte de bestanden uit het vorige pakket implementaties behoudt.

## <a name="list-the-applications-in-a-batch-account"></a>Lijst van de toepassingen in een Batch-account

U kunt de toepassingen en de pakketten in een Batch-account aanbieden met behulp van de [ApplicationOperations][net_appops]. [ListApplicationSummaries] [net_appops_listappsummaries] methode.

```csharp
// List the applications and their application packages in the Batch account.
List<ApplicationSummary> applications = await batchClient.ApplicationOperations.ListApplicationSummaries().ToListAsync();
foreach (ApplicationSummary app in applications)
{
    Console.WriteLine("ID: {0} | Display Name: {1}", app.Id, app.DisplayName);

    foreach (string version in app.Versions)
    {
        Console.WriteLine("  {0}", version);
    }
}
```

## <a name="wrap-up"></a>Inpakken

Toepassingspakketten kunt u uw klanten de toepassingen voor hun taken selecteert en geeft u de exacte versie moet worden gebruikt bij het verwerken van taken met uw service Batch ingeschakeld. U kan ook de mogelijkheid voor uw klanten om te uploaden en bijhouden van hun eigen toepassingen in uw service bieden.

## <a name="next-steps"></a>Volgende stappen

* De [Batch REST API] [ api_rest] biedt ook ondersteuning voor het werken met toepassingspakketten. Zie bijvoorbeeld de [applicationPackageReferences] [ rest_add_pool_with_packages] -element in [een groep aan een account toevoegen] [ rest_add_pool] voor meer informatie over het opgeven van de pakketten te installeren met behulp van de REST API. [Toepassingen] Zie[ rest_applications] voor meer informatie over het verkrijgen van informatie over de toepassingen met behulp van de Batch REST API.

* Meer informatie over het programmatisch [beheren Azure Batch accounts en quota met Batch Management .NET](batch-management-dotnet.md). De [Batch Management.NET] [ api_net_mgmt] bibliotheek account maken en verwijderen van functies voor uw Batch toepassing of service kunt inschakelen.

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_mgmt]: https://msdn.microsoft.com/library/azure/mt463120.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[batch_mgmt_nuget]: https://www.nuget.org/packages/Microsoft.Azure.Management.Batch/
[github_samples]: https://github.com/Azure/azure-batch-samples
[storage_pricing]: https://azure.microsoft.com/pricing/details/storage/
[net_appops]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.applicationoperations.aspx
[net_appops_listappsummaries]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.applicationoperations.listapplicationsummaries.aspx
[net_cloudpool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_cloudpool_pkgref]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.applicationpackagereferences.aspx
[net_cloudtask]: https://msdn.microsoft.com/library/microsoft.azure.batch.cloudtask.aspx
[net_cloudtask_pkgref]: https://msdn.microsoft.com/library/microsoft.azure.batch.cloudtask.applicationpackagereferences.aspx
[net_nodestate]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.state.aspx
[net_pkgref]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.applicationpackagereference.aspx
[portal]: https://portal.azure.com
[rest_applications]: https://msdn.microsoft.com/library/azure/mt643945.aspx
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
[rest_add_pool_with_packages]: https://msdn.microsoft.com/library/azure/dn820174.aspx#bk_apkgreference

[1]: ./media/batch-application-packages/app_pkg_01.png "Pakketten op hoog niveau systeemdiagram"
[2]: ./media/batch-application-packages/app_pkg_02.png "Toepassingen naast elkaar in Azure portal"
[3]: ./media/batch-application-packages/app_pkg_03.png "Blade in Azure portal toepassingen"
[4]: ./media/batch-application-packages/app_pkg_04.png "Toepassing details blade in Azure portal"
[5]: ./media/batch-application-packages/app_pkg_05.png "Nieuwe toepassing blade in Azure portal"
[7]: ./media/batch-application-packages/app_pkg_07.png "Bijwerken of verwijderen van pakketten vervolgkeuzelijst in Azure portal"
[8]: ./media/batch-application-packages/app_pkg_08.png "Nieuwe toepassing pakket blade in Azure portal"
[9]: ./media/batch-application-packages/app_pkg_09.png "Geen gekoppelde account waarschuwing voor opslag"
[10]: ./media/batch-application-packages/app_pkg_10.png "Kies opslag account blade in Azure portal"
[11]: ./media/batch-application-packages/app_pkg_11.png "Blade in Azure portal pakket bijwerken"
[12]: ./media/batch-application-packages/app_pkg_12.png "Dialoogvenster voor bevestiging in Azure portal verwijderen"
