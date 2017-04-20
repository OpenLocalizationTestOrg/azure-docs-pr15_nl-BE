<properties
    pageTitle="Azure Batch Functieoverzicht voor ontwikkelaars | Microsoft Azure"
    description="Informatie over de functies van de Batch-service en de API's vanuit een oogpunt ontwikkeling."
    services="batch"
    documentationCenter=".net"
    authors="mmacy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="batch"
    ms.devlang="multiple"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="big-compute"
    ms.date="09/29/2016"
    ms.author="marsma"/>

# <a name="batch-feature-overview-for-developers"></a>Overzicht van batch-functie voor ontwikkelaars

In dit overzicht van de belangrijkste onderdelen van de service Azure Batch, bespreken we de primaire servicefuncties en bronnen die ontwikkelaars Batch kunt ontwikkelen grootschalige parallelle oplossingen berekenen.

Of u ontwikkelt een gedistribueerde rekenkundige toepassing of service die problemen direct [REST API] [ batch_rest_api] gesprekken of u gebruikt een van de [Batch-SDK's](batch-technical-overview.md#batch-development-apis), kunt u veel van de middelen en de functies die in dit artikel besproken.

> [AZURE.TIP] Zie voor een inleiding op een hoger niveau met de Batch-service, [Basisbeginselen Azure Batch](batch-technical-overview.md).

## <a name="batch-service-workflow"></a>Batch-werkstroom

De volgende workflow op hoog niveau is typisch voor vrijwel alle toepassingen en services die gebruikmaken van de Batch-service voor parallelle werklasten verwerken:

1. Uploaden van **bestanden** die u wilt verwerken met een [Opslag van Azure] [ azure_storage] account. Batch beschikt over ingebouwde ondersteuning voor het openen van Azure Blob-opslag en uw taken kunnen deze bestanden downloaden met [computerknooppunten](#compute-node) als de taken worden uitgevoerd.

2. Upload **toepassingsbestanden** die de taken worden uitgevoerd. Deze bestanden kunnen worden binaire bestanden of scripts en bijbehorende afhankelijkheden en worden uitgevoerd door de taken in uw projecten. Uw taken kunnen downloaden vanaf uw account opslag of kunt u de functie voor [toepassingspakketten](#application-packages) batch voor toepassingen beheren en distribueren.

3. Een [groep](#pool) van computerknooppunten maken. Als u een groep maakt, geeft u het aantal knooppunten berekenen voor de toepassingen, de grootte en het besturingssysteem. Wanneer elke taak in uw project wordt uitgevoerd, is deze uit te voeren op een van de knooppunten in de groep toegewezen.

4. Een [taak](#job)maken. Een project beheert een verzameling taken. Koppelt u elke taak aan een bepaalde groep van toepassingen waarbij de taken van de taak wordt uitgevoerd.

5. [Taken](#task) toevoegen aan het project. Elke taak voert de toepassing of het script dat u hebt geüpload voor het verwerken van de gegevensbestanden van die het downloaden van uw account voor opslag. Bij elke taak is voltooid, kan het de uitvoer uploaden naar Azure opslag.

6. Voortgang van de taak en de uitvoer van de taak ophalen uit de opslag van Azure.

De volgende secties worden deze en de andere bronnen van de Batch waarmee uw gedistribueerde rekenkundige scenario.

> [AZURE.NOTE] U moet een [account Batch](batch-account-create-portal.md) de Batch-service wilt gebruiken. Ook bijna alle oplossingen gebruiken een [Azure opslag] [ azure_storage] rekening voor het bestand opslaan en ophalen. Batch momenteel ondersteunt alleen het **algemene** opslagtype zoals beschreven in stap 5 van [de account van een opslag maken](../storage/storage-create-storage-account.md#create-a-storage-account) in [Azure over opslag rekeningen](../storage/storage-create-storage-account.md).

## <a name="batch-service-resources"></a>Batch-servicebronnen

Enkele van de volgende bronnen--accounts, berekenen knooppunten, groepen van toepassingen, projecten en taken – zijn vereist voor alle oplossingen die gebruikmaken van de service Batch. Andere, zijn zoals taakschema en toepassingspakketten, nuttig, maar optioneel functies.

- [Account](#account)
- [Knooppunt berekenen](#compute-node)
- [Groep van toepassingen](#pool)
- [Taak](#job)

  - [Taakschema 's](#scheduled-jobs)

- [Taak](#task)

  - [Taak starten](#start-task)
  - [Taak manager](#job-manager-task)
  - [Projecttaken opstellen en vrijgeven](#job-preparation-and-release-tasks)
  - [Meerdere exemplaren taak (MPI)](#multi-instance-tasks)
  - [Afhankelijkheden van taken](#task-dependencies)

- [Toepassingspakketten](#application-packages)

## <a name="account"></a>Account

Een Batch-account is een uniek geïdentificeerd entiteit in de Batch-service. De verwerking is gekoppeld aan een Batch-account. Wanneer u bewerkingen met de Batch-service uitvoeren, moet u de accountnaam en een van de sleutels voor haar rekening. U kunt [een Batch Azure account maken met de Azure portal](batch-account-create-portal.md).

## <a name="compute-node"></a>Knooppunt berekenen

Een compute node is een Azure VM (virtual machine) die gericht is op een gedeelte van de werklast van uw toepassing verwerkt. De grootte van een knooppunt bepaalt het aantal CPU cores, geheugencapaciteit en grootte van het lokale bestand dat is toegewezen aan het knooppunt. Met behulp van afbeeldingen van Azure Cloud Services of virtuele Machines Marketplace kunt u groepen van Windows- of Linux-knooppunten. Zie de volgende sectie [van toepassingen](#pool) voor meer informatie over deze opties.

Knooppunten kunnen een uitvoerbaar bestand of script dat wordt ondersteund door het besturingssysteem van het knooppunt worden uitgevoerd. Dit omvat \*.exe, \*.cmd, \*.bat en PowerShell-scripts voor Windows-- en binaire bestanden, shell en Python scripts voor Linux.

Alle berekenen knooppunten in een Batch ook:

- Een standaard [mapstructuur](#files-and-directories) en de bijbehorende [omgevingsvariabelen](#environment-settings-for-tasks) die beschikbaar zijn voor taken.
- **Firewall** -instellingen die zijn geconfigureerd voor toegangsbeheer.
- [RAS](#connecting-to-compute-nodes) (Remote Desktop Protocol (RDP)) voor Windows en Linux (Secure Shell (SSH))-knooppunten.

## <a name="pool"></a>Groep van toepassingen

Een groep is een verzameling knooppunten die op uw toepassing wordt uitgevoerd. De toepassingen kan handmatig worden gemaakt door u of automatisch door de service Batch als u opgeeft dat het werk moet worden gedaan. U kunt maken en beheren van een groep die voldoet aan de behoeften van uw toepassing. Een groep kan alleen worden gebruikt door de batchverwerking account waarin deze is gemaakt. Een Batch-account kan hebben meer dan één groep.

Azure Batch toepassingen bouwen op het platform core Azure berekenen. Ze bieden grote toewijzing, installatie van toepassingen gegevensdistributie, statuscontrole en flexibele aanpassing van het aantal knooppunten in een groep ([schalen](#scaling-compute-resources)) berekenen.

Elk knooppunt dat wordt toegevoegd aan een groep van toepassingen wordt een unieke naam en IP-adres toegewezen. Alle wijzigingen die in het besturingssysteem of de bestanden zijn aangebracht, gaan verloren wanneer een knooppunt wordt verwijderd uit een groep, en de naam en het IP-adres worden vrijgegeven voor toekomstig gebruik. Wanneer een knooppunt een groep verlaat, is de levensduur.

Als u een groep maakt, kunt u de volgende kenmerken:

- Knooppunt **besturingssysteem** en **versie** berekenen

    U hebt twee opties wanneer u een besturingssysteem voor de knooppunten in uw groep: **Virtuele machineconfiguratie** en de **Configuratie van de Cloud-Services**.

    **Virtuele machineconfiguratie** biedt zowel Linux als Windows-installatiekopieën voor computerknooppunten van [Azure Virtual Machines Marketplace][vm_marketplace].
    Wanneer u een pool met virtuele machineconfiguratie knooppunten maakt, moet u niet alleen de grootte van de knooppunten, maar ook de **verwijzing naar de virtuele machine-afbeelding** en de Batch- **knooppunt agent SKU** moet worden geïnstalleerd op de knooppunten. Zie [voorziening Linux computerknooppunten in Azure Batch van toepassingen](batch-linux-nodes.md)voor meer informatie over het opgeven van deze eigenschappen van toepassingen.

    **Configuratie van cloud Services** biedt dat Windows knooppunten *alleen*berekenen. Beschikbare besturingssystemen voor Cloud Services configuratie van toepassingen worden weergegeven in het [Gastbesturingssysteem Azure releases en SDK compatibiliteit matrix](../cloud-services/cloud-services-guestos-update-matrix.md). Bij het maken van een groep die Cloud Services-knooppunten bevat, moet u opgeven voor de grootte van het knooppunt en de *Familie van OS*. Bij het maken van groepen van Windows computerknooppunten, u meest Cloud Services.

    - De *OS-familie* bepaalt ook welke versies van .NET worden geïnstalleerd met het besturingssysteem.
    - Als u met de rol van de werknemer binnen Cloud-Services, kunt u een *Versie van het besturingssysteem* (Zie het gedeelte [meer informatie over de cloud-services](../cloud-services/cloud-services-choose-me.md#tell-me-about-cloud-services) in de [Cloud-Services-overzicht](../cloud-services/cloud-services-choose-me.md)voor meer informatie over rollen van de werknemer,).
    - Als u met de rol van de werknemer, wordt aangeraden dat u `*` voor de *Versie van het besturingssysteem* dat de knooppunten automatisch bijgewerkt worden en er geen moeten geschikt zijn is voor nieuwe versies uitgebracht. Het primaire gebruik geval voor het selecteren van een specifieke versie van het besturingssysteem is om de compatibiliteit van toepassingen, waardoor de compatibiliteit getest om te worden uitgevoerd voordat de versie moet worden bijgewerkt. Na validatie van de *Versie van het besturingssysteem* voor de groep van toepassingen kan worden bijgewerkt en de nieuwe installatiekopie van het besturingssysteem kan worden geïnstalleerd--alle actieve taken worden onderbroken en opnieuw.

- **De grootte van de knooppunten**

    **Configuratie van cloud Services** compute knooppunt formaten worden vermeld in de [formaten voor Cloud Services](../cloud-services/cloud-services-sizes-specs.md). Batch ondersteunt alle formaten van Cloud-diensten, met uitzondering van `ExtraSmall`.

    **Configuratie van de virtuele Machine** berekenen knooppunt formaten worden vermeld in de [formaten voor virtuele machines in Azure](../virtual-machines/virtual-machines-linux-sizes.md) (Linux) en - [grootten voor virtuele machines in Azure](../virtual-machines/virtual-machines-windows-sizes.md) (Windows). Batch ondersteunt alle formaten voor Azure VM, met uitzondering van `STANDARD_A0` en de premie opslag (`STANDARD_GS`, `STANDARD_DS`, en `STANDARD_DSV2` reeks).

    Als u de grootte van een compute knooppunt selecteert, kunt u de kenmerken en de eisen van de toepassingen die u op de knooppunten uitvoeren wilt. Aspecten zoals of de toepassing multithreaded en hoeveel geheugen deze verbruikt kan helpen bepalen de grootte van de meest geschikte en kosteneffectieve knooppunt. Het is typisch een knooppunt grootte uitgaande van een taak wordt uitgevoerd op een knooppunt tegelijk selecteren. Het is echter mogelijk om meerdere taken (en dus meerdere toepassingsexemplaren) [parallel uitgevoerd](batch-parallel-node-tasks.md) op computerknooppunten tijdens de taakuitvoering. In dit geval is het gebruikelijk om te kiezen van een knooppunt groter aan de toegenomen vraag van de uitvoering van parallelle taken. Zie [beleid voor plannen van taken](#task-scheduling-policy) voor meer informatie.

    Alle knooppunten in een groep hebben dezelfde grootte. Als toepassingen met verschillende vereisten voor het uitvoeren en/of niveaus, laden wilt aangeraden afzonderlijke groepen van toepassingen te gebruiken.

- **Aantal knooppunten dat doel**

    Dit is het aantal computerknooppunten die u wilt implementeren in de groep. Dit is bedoeld als *doel* omdat in sommige situaties uw groep niet het gewenste aantal knooppunten kan bereiken. Een groep kan het gewenste aantal knooppunten niet bereiken als het [Quotum core](batch-quota-limit.md#batch-account-quotas) voor uw Batch-account is bereikt of als er een automatisch schalen formule die u hebt toegepast op de groep die het maximum aantal knooppunten beperkt (Zie 'Schalen beleid' hieronder).

- **Schalen van beleid**

    Naast het opgeven van een statische aantal knooppunten, kunt u in plaats daarvan schrijft en een [formule automatisch schalen](#scaling-compute-resources) toepassen op een groep. De Batch-service periodiek de formule wordt geëvalueerd en past u het aantal knooppunten in de groep op basis van verschillende toepassingen, taak en taakparameters die u kunt opgeven.

- **Beleid voor taakplanning**

    De optie [maximum aantal taken per knooppunt](batch-parallel-node-tasks.md) bepaalt het maximum aantal taken dat gelijktijdig kan worden uitgevoerd op elk computerknooppunt binnen de groep.

    De standaardconfiguratie is dat een taak wordt uitgevoerd op een knooppunt, maar er scenario's waar is het nuttig zijn om meerdere taken gelijktijdig uitgevoerd op een knooppunt hebt. Zie het [voorbeeldscenario](batch-parallel-node-tasks.md#example-scenario) in het [knooppunt gelijktijdige taken](batch-parallel-node-tasks.md) artikel om te zien hoe u kunt profiteren van meerdere taken per knooppunt.

    Ook kunt u een *Opvullingstype* die bepaalt of Batch de taken gelijkmatig gespreid over alle knooppunten in een groep of elk knooppunt met het maximum aantal taken pakt voordat u taken toewijst aan een ander knooppunt.

- **Communicatiestatus** van computerknooppunten

    In de meeste gevallen taken onafhankelijk van elkaar werken en hoeft niet met elkaar communiceren. Er zijn echter enkele toepassingen waarin taken, zoals [MPI-scenario's communiceren moeten](batch-mpi.md).

    U kunt een groep als u wilt dat de communicatie tussen de knooppunten binnen it--**de communicatie tussen knooppunten**. Als de communicatie tussen knooppunten is ingeschakeld, knooppunten in de configuratie van Cloud Services-toepassingen kunnen met elkaar communiceren op de poorten van meer dan 1100 en virtuele machineconfiguratie van toepassingen verkeer op elke poort niet beperken.

    Houd er rekening mee dat communicatie tussen knooppunten inschakelen ook van invloed op de plaatsing van de knooppunten binnen clusters en het maximum aantal knooppunten in een groep van toepassingen vanwege de beperkingen van de implementatie beperkt mogelijk. Als uw toepassing niet dat de communicatie tussen knooppunten vereist, de Batch-service een potentieel groot aantal knooppunten kunt toewijzen aan de groep van veel verschillende clusters en datacenters te schakelen parallelle verwerking vermogen verhoogd.

- Van **taak starten** voor computerknooppunten

    De optionele *taak starten* wordt uitgevoerd op elk knooppunt dat knooppunt wordt toegevoegd aan de groep, en elke keer dat een knooppunt opnieuw wordt opgestart of reimaged. De starttaak is vooral handig voor het voorbereiden van computerknooppunten voor het uitvoeren van taken, zoals het installeren van de toepassingen die de taken worden uitgevoerd op de compute nodes.

- **Toepassingspakketten**

    U kunt [toepassingspakketten](#application-packages) voor de implementatie van de compute nodes in de groep opgeven. Toepassingspakketten bieden versies van de toepassingen die worden uitgevoerd voor uw taken en vereenvoudigde implementatie. Toepassingspakketten die u voor een groep van toepassingen opgeeft worden geïnstalleerd op elk knooppunt dat wordt toegevoegd aan die groep en telkens wanneer een knooppunt wordt opnieuw opgestart of reimaged. Toepassingspakketten worden momenteel niet ondersteund op Linux-computerknooppunten.

- **Netwerkconfiguratie**

    Kunt u de ID van een Azure [virtueel netwerk (VNet)](../virtual-network/virtual-networks-overview.md) , waarin de groep van toepassingen computerknooppunten moet worden gemaakt. Vereisten voor het opgeven van een VNet voor uw groep u in [een groep aan een account toevoegen vinden kunt] [ vnet] in de Batch REST API reference.

> [AZURE.IMPORTANT] Voor alle Batch zijn een **Quotum** dat het aantal **cores** (en dus computerknooppunten) in een Batch-account beperkt. Vindt u de standaard-quota's en instructies voor het [quotum verhogen](batch-quota-limit.md#increase-a-quota) (zoals het maximum aantal cores in uw account Batch) in [quota's en beperkingen voor de Batch Azure service](batch-quota-limit.md). Als u merkt dat u vragen "Waarom won't mijn groep bereiken meer dan X knooppunten?" Dit quotum core kan de oorzaak zijn.

## <a name="job"></a>Taak

Een taak is een verzameling taken. Het beheren van hoe de berekening wordt uitgevoerd door de taken op de compute nodes in een groep.

- De taak geeft de **groep van toepassingen** waarin het werk wordt uitgevoerd. U kunt een nieuwe toepassingen voor elke taak maken of een groep van toepassingen gebruiken voor veel taken. U kunt een groep voor elke taak die is gekoppeld aan een projectplanning of voor alle taken die zijn gekoppeld aan een projectplanning maken.

- U kunt een optionele **prioriteit van taak**opgeven. Wanneer een taak wordt verzonden met een hogere prioriteit dan de taken die momenteel uitgevoerd worden, worden de taken voor de taak hogere prioriteit ingevoegd in de wachtrij voor taken voor de taken met lagere prioriteit. Taken in taken met lagere prioriteit die al worden uitgevoerd, worden niet afgebroken.

- Taak **beperkingen** kunt u bepaalde limieten opgeven voor uw projecten:

    U kunt een **maximale wallclock tijd**zo instellen dat als een taak wordt uitgevoerd langer dan de maximale wallclock tijd die is opgegeven, de taak en alle taken zijn beëindigd.

    Batch detecteert en mislukte taken opnieuw. Kunt u het **maximum aantal pogingen voor taak** als een beperking, met inbegrip van of de taak wordt *altijd* of *nooit* opnieuw geprobeerd. Bezig met opnieuw proberen een taak houdt in dat de taak wordt opnieuw opnieuw worden uitgevoerd.

- De clienttoepassing taken kunt toevoegen aan een project of u kunt opgeven dat een [projecttaak manager](#job-manager-task). Een projecttaak manager bevat de gegevens die nodig zijn om de vereiste taken voor een taak, de taak van de manager wordt uitgevoerd op een van de compute nodes in de groep. De taak van de manager wordt afgehandeld door Batch--speciaal deze in een wachtrij is zodra de taak wordt gemaakt en opnieuw wordt opgestart als dat niet werkt. Een projecttaak manager is *vereist* voor de taken die door een [schema](#scheduled-jobs) worden gemaakt omdat dit de enige manier om de taken definiëren voordat de taak wordt gestart.

- Standaard blijven de taken in de actieve status als alle taken binnen de taak voltooid is. U kunt dit gedrag wijzigen zodat de taak automatisch wordt beëindigd wanneer alle taken in het project voltooid zijn. Stel de eigenschap **onAllTasksComplete** van het project ([OnAllTasksComplete] [ net_onalltaskscomplete] in Batch .NET) te *terminatejob* aan de taak automatisch opgeheven wanneer alle taken in de eindstatus.

    Houd er rekening mee dat de Batch-service een taak *zonder* taken hebben alle taken worden voltooid beschouwt. Daarom worden deze optie wordt meestal gebruikt bij een [projecttaak manager](#job-manager-task). Als u wilt dat automatische taak beëindigen zonder een manager van de taak gebruikt, moet u in eerste instantie een nieuwe taak **onAllTasksComplete** eigenschap ingesteld op *noaction*en vervolgens ingesteld op *terminatejob* nadat u klaar bent met het toevoegen van taken aan het project.

### <a name="job-priority"></a>Prioriteit van taak

U kunt een prioriteit toewijzen aan taken die u in de batchverwerking. De prioriteit van de taak de Batch-service gebruikt om te bepalen van de volgorde van de taakplanning binnen een account (dit is niet te verwarren met een [geplande taak](#scheduled-jobs)). De waarden variëren van-1000 tot en met 1000 met-1000 de laagste prioriteit en 1000 de hoogste. U kunt de prioriteit van een taak bijwerken met behulp van [de eigenschappen van een taak bijwerken] [ rest_update_job] bewerking (Batch REST) of door aanpassing van de [CloudJob.Priority] [ net_cloudjob_priority] eigenschap (Batch .NET).

Binnen dezelfde account, taken met een hogere prioriteit hebben voorrang op taken met een lagere prioriteit plannen. Een project met een hogere prioriteit in één account heeft geen voorrang boven een andere taak met een lagere prioriteit in een andere account planning.

Verschillende groepen van de taakplanning is onafhankelijk. Tussen de verschillende mediagroepen, is niet gegarandeerd dat de eerste taak met een hogere prioriteit worden gepland als de bijbehorende groep van toepassingen onder een niet-actieve knooppunten wordt. Taken met de dezelfde prioriteit hebben in dezelfde groep, een gelijke kans wordt gepland.

### <a name="scheduled-jobs"></a>Geplande taken

[Taak schema's] [ rest_job_schedules] kunt u terugkerende taken in de Batch-service maken. Een taakplanning geeft bij het uitvoeren van taken en bevat de specificaties van de projecten die moeten worden uitgevoerd. U kunt de duur van de planning--hoe lang en wanneer de planning in feite is-- en hoe vaak in die periode die taken moeten worden gemaakt.

## <a name="task"></a>Taak

Een taak is een eenheid van een berekening die is gekoppeld aan een taak. Deze wordt uitgevoerd op een knooppunt. Taken zijn toegewezen aan een knooppunt voor de uitvoering van of in de wachtrij totdat een knooppunt weer beschikbaar. Eenvoudig gesteld: een taak wordt uitgevoerd een of meer programma's of scripts op een compute node naar de werkzaamheden die u moet uitvoeren.

Wanneer u een taak maakt, kunt u het volgende opgeven:

- De **opdrachtregel** van de taak. Dit is de opdrachtregel waarmee de toepassing of het script op de compute node wordt uitgevoerd.

    Het is belangrijk te weten dat de opdrachtregel niet daadwerkelijk onder een shell voert. Daarom deze standaard gebruik kan maken van shellfuncties zoals de [omgevingsvariabele](#environment-settings-for-tasks) -uitbreiding (dit geldt ook voor de `PATH`). Als u wilt profiteren van deze functies, start u de shell op de opdrachtregel--bijvoorbeeld door het starten van `cmd.exe` op knooppunten Windows of `/bin/sh` op Linux:

    `cmd /c MyTaskApplication.exe %MY_ENV_VAR%`

    `/bin/sh -c MyTaskApplication $MY_ENV_VAR`

    Als uw taken moeten uitvoeren van een toepassing of het script dat niet van het knooppunt `PATH` of verwijst naar de omgevingsvariabelen, de shell expliciet in de opdrachtregel van taak starten.

- **Resource-bestanden** met de gegevens kunnen worden verwerkt. Deze bestanden worden automatisch gekopieerd naar het knooppunt Blob-opslag in een **algemene** opslag Azure-account voordat de opdrachtregel van de taak wordt uitgevoerd. Zie de secties [taak starten](#start-task) en [bestanden en mappen](#files-and-directories)voor meer informatie.

- De **omgevingsvariabelen** die nodig zijn voor uw toepassing. Zie de sectie [de instellingen voor taken](#environment-settings-for-tasks) voor meer informatie.

- De **beperkingen** die de taak moet worden uitgevoerd. Voor bijvoorbeeld de maximale tijd dat een taak uit te voeren, het maximum aantal keren dat een mislukte taak moet opnieuw worden gestart, is toegestaan en de maximale tijd dat bestanden in de werkmap van de taak worden bewaard.

- **Toepassingspakketten** te implementeren op de compute node waarop de taak is gepland. [Toepassingspakketten](#application-packages) bieden versies van de toepassingen die worden uitgevoerd voor uw taken en vereenvoudigde implementatie. Op taakniveau toepassingspakketten zijn met name handig in omgevingen met gedeelde toepassingen, waarbij verschillende taken worden uitgevoerd op een groep en de groep wordt niet verwijderd wanneer een taak is voltooid. Als uw project minder taken dan de knooppunten in de pool heeft, minimaliseren toepassingspakketten taak gegevensoverdracht sinds de toepassing is uitsluitend te implementeren op de knooppunten die taken uitvoeren.

Naast de taken die u definieert voor het uitvoeren van berekeningen op een knooppunt, zijn de volgende speciale taken ook beschikbaar door de Batch-service:

- [Taak starten](#start-task)
- [Taak manager](#job-manager-task)
- [Projecttaken opstellen en vrijgeven](#job-preparation-and-release-tasks)
- [Taken met meerdere instanties (MPI)](#multi-instance-tasks)
- [Afhankelijkheden van taken](#task-dependencies)

### <a name="start-task"></a>Taak starten

Door het koppelen van een **taak starten** met een groep, kunt u de werkomgeving van de knooppunten opstellen. Zo kunt u bewerkingen zoals het installeren van de toepassingen die uw taken uitvoeren en starten van achtergrondprocessen uitvoeren. De starttaak wordt uitgevoerd wanneer een knooppunt wordt gestart, voor zolang deze in de pool--inclusief wanneer eerst het knooppunt toegevoegd aan de groep en wanneer deze opnieuw wordt opgestart of reimaged.

Een belangrijk voordeel van de starttaak is dat alle informatie die nodig is bevatten kan voor het configureren van een compute node en installeer de toepassingen die vereist voor de uitvoering van taken zijn. Verhoging van het aantal knooppunten in een groep is dus net zo eenvoudig als het nieuwe doel knooppunt count--Batch al geven de informatie die nodig is voor het configureren van de nieuwe knooppunten en krijgen ze gereed zijn voor het accepteren van taken is.

Als u met een batchtaak Azure, kunt u een lijst van **bronbestanden** in [Azure opslag][azure_storage], naast een **opdracht** moet worden uitgevoerd. Batch resource-bestanden eerst gekopieerd naar het knooppunt uit Azure opslag en de opdrachtregel wordt uitgevoerd. Voor een taak van toepassingen start bevat de lijst met bestanden meestal de toepassing van de taak en de bijbehorende afhankelijkheden.

Het kan echter ook referentiegegevens worden gebruikt door alle taken die worden uitgevoerd op de compute node bevatten. Voorbeeld van een starttaak vanaf de opdrachtregel kan uitvoeren een `robocopy` bewerking toepassingsbestanden (die zijn opgegeven als bronbestanden en gedownload naar het knooppunt) van de starttaak [werkmap](#files-and-directories) kopiëren naar de [gedeelde map](#files-and-directories)en voert u een MSI-bestand of `setup.exe`.

> [AZURE.IMPORTANT] Batch momenteel ondersteunt *alleen* het type **algemene** opslag zoals beschreven in stap 5 van [de account van een opslag maken](../storage/storage-create-storage-account.md#create-a-storage-account) in [Azure over opslag rekeningen](../storage/storage-create-storage-account.md). Batchtaken (met inbegrip van standaardtaken, start taken, projecttaken voorbereiden en projecttaken release) moeten de bronbestanden die zich *alleen* op **algemene** opslag rekeningen opgeven.

Is het meestal wenselijk voor de Batch-service wacht de starttaak voordat het knooppunt kunnen worden toegewezen taken voltooien, maar kunt u dit.

Als u een starttaak niet op een compute node, vervolgens de status van het knooppunt wordt bijgewerkt met de fout en het knooppunt is niet toegewezen aan taken. Een starttaak kan mislukken als er een probleem is de bronbestanden van de opslag te kopiëren of als het proces dat wordt uitgevoerd door de opdrachtregel een afsluitcode van nul als resultaat.

Als u toevoegen of bijwerken van de starttaak voor een *bestaande* groep van toepassingen, moet u de computerknooppunten voor de starttaak moet worden toegepast op de knooppunten opnieuw opstarten.

### <a name="job-manager-task"></a>Taak manager

U meestal een **taak manager** beheren en/of monitor taakuitvoering--bijvoorbeeld maken en indienen van de taken van een taak, bepalen van de aanvullende taken uit te voeren en bepalen wanneer het werk is voltooid. Een projecttaak manager is echter niet beperkt tot deze activiteiten. Het is een volledig zelfstandige taak die acties die vereist voor de taak zijn kunt uitvoeren. Bijvoorbeeld kan een taak manager een bestand downloaden dat is opgegeven als een parameter analyseren van de inhoud van het bestand en extra taken op basis van de inhoud verzenden.

Een projecttaak manager wordt gestart voordat alle andere taken. Biedt de volgende functies:

- Deze is automatisch aangeboden als een taak door de Batch-service wanneer de taak is gemaakt.

- Deze is moet worden uitgevoerd voordat de andere taken in een project gepland.

- Het bijbehorende knooppunt is het laatste wordt verwijderd uit een groep bij wordt de groep van toepassingen wordt ingekrimpt.

- De overeenkomst kan worden gekoppeld aan de beëindiging van alle taken in het project.

- Een taak manager, krijgt de hoogste prioriteit wanneer deze moet opnieuw worden gestart. Als een actief knooppunt niet beschikbaar is, kan een van de actieve taken in de pool ruimte maken voor de projecttaak manager uit te voeren de Batch-service beëindigen.

- Een taak in één project manager heeft geen prioriteit over de taken van andere taken. In projecten, worden alleen taakniveau prioriteiten waargenomen.

### <a name="job-preparation-and-release-tasks"></a>Projecttaken opstellen en vrijgeven

Batch biedt projecttaken voorbereiden voor installatie vooraf taakuitvoering. Release taken zijn voor de taak na onderhoud of opruimen.

- **Voorbereiding taak**: een taak van het preparaat wordt uitgevoerd op alle knooppunten van berekenen die zijn gepland voor het uitvoeren van taken, voordat een van de andere taken worden uitgevoerd. U kunt een taak kopiëren van gegevens die wordt gedeeld door alle taken, maar uniek is voor de taak, bijvoorbeeld voorbereiding.
- **Projecttaak release**: wanneer een taak is voltooid, een release-taak wordt uitgevoerd op elk knooppunt in de toepassingen die ten minste één taak uitgevoerd. Gegevens die is gekopieerd met de projecttaak voorbereiding te verwijderen of te comprimeren en uploaden van diagnostische gegevens, bijvoorbeeld kunt u een taak taak vrijgeven.

Voorbereiding van beide taken en release taken kunt u een opdrachtregel uit te voeren wanneer de taak wordt gestart. Ze bieden functies zoals het downloaden van bestand, verhoogde uitvoering, aangepaste omgevingsvariabelen, uitvoering maximale duur, aantal nieuwe pogingen en retentietijd bestand.

Voor meer informatie over de voorbereiding en het vrijgeven van projecttaken, Zie [computerknooppunten uitvoeren voorbereiding en voltooiing van taken op Azure Batch](batch-job-prep-release.md).

### <a name="multi-instance-task"></a>Taak meerdere exemplaren

Een [taak meerdere exemplaren](batch-mpi.md) is een taak die is geconfigureerd voor uitvoering op meer dan één computerknooppunt tegelijk. Meerdere exemplaren van taken kunt u krachtige situaties waarvoor een groep computerknooppunten die samen worden toegewezen voor het verwerken van een enkele werkbelasting (zoals Message Passing Interface (MPI)).

Uitchecken voor gedetailleerde informatie over het uitvoeren van taken MPI in Batch met behulp van de Batch .NET library [Gebruik meerdere exemplaren taken Message Passing Interface (MPI)-toepassingen in Azure Batch uit te voeren](batch-mpi.md).

### <a name="task-dependencies"></a>Afhankelijkheden van taken

[Taakafhankelijkheden](batch-task-dependencies.md), zoals de naam al aangeeft, kunt u opgeven dat een taak afhankelijk van de voltooiing van andere taken vóór de uitvoering ervan is. Deze functie biedt ondersteuning voor situaties waarin een "downstream" verbruik de uitvoer van een "upstream"-taak - of bij een upstream-taak wordt uitgevoerd voor sommige initialisatie die nodig is voor een taak downstream. Deze functie wilt gebruiken, moet u eerst de taakafhankelijkheden inschakelen op uw batchverwerking. Klik voor elke taak die afhankelijk van een andere is (of vele andere), geeft u de taken die deze taak afhankelijk van is.

Met afhankelijkheden van taken, kunt u scenario's als volgt uit:

* *taskB* is afhankelijk van *taskA* (*taskB* uitvoering pas begint *taskA* is voltooid).
* *taskC* , hangt af van de *taskA* en *taskB*.
* *taskD* is afhankelijk van een reeks taken, zoals taken *1* tot en met *10*, voordat deze wordt uitgevoerd.

[Taakafhankelijkheden in Azure Batch](batch-task-dependencies.md) en de [TaskDependencies] [ github_sample_taskdeps] codevoorbeeld in [azure-batch-monsters] [ github_samples] GitHub opslagplaats voor meer gedetailleerde informatie over deze functie.

## <a name="environment-settings-for-tasks"></a>Omgevingsinstellingen voor taken

Elke taak die wordt uitgevoerd door de batchverwerking-service heeft toegang tot omgevingsvariabelen die wordt ingesteld op computerknooppunten. Dit omvat omgevingsvariabelen van de Batch-service ([service gedefinieerd][msdn_env_vars]) en aangepaste omgevingsvariabelen die u voor uw taken definiëren kunt. De toepassingen en scripts uitvoeren van uw taken hebt toegang tot deze omgevingsvariabelen tijdens het uitvoeren.

Vullen met de eigenschap *omgevingsinstellingen* voor deze entiteiten kunt u aangepaste omgevingsvariabelen instellen op het niveau van de taak. Zie bijvoorbeeld het [toevoegen van een taak aan een project] [ rest_add_task] bewerking (Batch REST API) of de [CloudTask.EnvironmentSettings] [ net_cloudtask_env] en [CloudJob.CommonEnvironmentSettings] [ net_job_env] eigenschappen in Batch .NET.

Uw toepassing of service van een taak omgevingsvariabelen, zowel service gedefinieerde en aangepaste, verkrijgen met behulp van de [informatie over een taak] [ rest_get_task_info] bewerking (Batch REST) of in de [CloudTask.EnvironmentSettings] van[ net_cloudtask_env] eigenschap (Batch .NET). Processen die worden uitgevoerd op een computerknooppunt toegang tot deze en andere omgevingsvariabelen op het knooppunt, bijvoorbeeld via de vertrouwde `%VARIABLE_NAME%` (Windows) of `$VARIABLE_NAME` (Linux)-syntaxis.

U kunt een volledige lijst van alle service gedefinieerde omgevingsvariabelen vinden in het [knooppunt omgevingsvariabelen berekenen][msdn_env_vars].

## <a name="files-and-directories"></a>Bestanden en mappen

Elke taak heeft een *werkmap* waarin er nul of meer bestanden en mappen gemaakt. Deze werkmap kan worden gebruikt voor het opslaan van het programma dat wordt uitgevoerd door de taak, de gegevens die worden verwerkt en de uitvoer van de verwerking die wordt uitgevoerd. Alle bestanden en mappen van een taak zijn eigendom van de gebruiker van de taak.

De Batch-service stelt een gedeelte van het bestandssysteem op een knooppunt als de *hoofdmap*. Taken kunnen toegang krijgen tot de hoofdmap door te verwijzen naar de `AZ_BATCH_NODE_ROOT_DIR` omgevingsvariabele. Zie voor meer informatie over het gebruik van omgevingsvariabelen [omgevingsinstellingen voor taken](#environment-settings-for-tasks).

De hoofdmap bevat de volgende mappenstructuur:

![Knooppunt mapstructuur berekenen][1]

- **gedeeld**: deze map lezen/schrijven-toegang biedt tot *alle* taken die worden uitgevoerd op een knooppunt. Een taak die wordt uitgevoerd op het knooppunt kunt maken, lezen, bijwerken en verwijderen van bestanden in deze map. Taken kunnen toegang tot deze map door te verwijzen naar de `AZ_BATCH_NODE_SHARED_DIR` omgevingsvariabele.

- **opstarten**: deze map door een starttaak wordt gebruikt als de werkmap. Alle bestanden die worden gedownload naar het knooppunt door de starttaak hier opgeslagen. De starttaak kunt maken, lezen, bijwerken en verwijderen van bestanden in deze map. Taken kunnen toegang tot deze map door te verwijzen naar de `AZ_BATCH_NODE_STARTUP_DIR` omgevingsvariabele.

- **Taken**: een map gemaakt voor elke taak die wordt uitgevoerd op het knooppunt. Deze wordt geopend door te verwijzen naar de `AZ_BATCH_TASK_DIR` omgevingsvariabele.

    Binnen elke map taak de Batch-service maakt een werkmap (`wd`) waarvan het unieke pad wordt opgegeven door de `AZ_BATCH_TASK_WORKING_DIR` omgevingsvariabele. Deze map bevat een lezen/schrijven-toegang tot de taak. De taak kunt maken, lezen, bijwerken en verwijderen van bestanden in deze map. Deze map wordt ingehouden op basis van de beperking van de *RetentionTime* die is opgegeven voor de taak.

    `stdout.txt`en `stderr.txt`: deze bestanden zijn opgeslagen in de taakmap tijdens de uitvoering van de taak.

>[AZURE.IMPORTANT] Wanneer een knooppunt wordt verwijderd uit de groep, worden *alle* bestanden die zijn opgeslagen op het knooppunt worden verwijderd.

## <a name="application-packages"></a>Toepassingspakketten

De functie [toepassingspakketten](batch-application-packages.md) biedt eenvoudig beheer en distributie van toepassingen op de compute nodes in uw toepassingen. U kunt uploaden en beheren van meerdere versies van de toepassingen die worden uitgevoerd door uw taken, met inbegrip van binaire bestanden en de ondersteunende bestanden. Vervolgens kunt u automatisch een of meer van deze toepassingen om de compute nodes te implementeren in uw groep.

U kunt toepassingspakketten op het niveau van toepassingen en de taak opgeven. Wanneer u toepassingen toepassingspakketten opgeeft, wordt de toepassing wordt gedistribueerd naar elk knooppunt in de groep. Wanneer u een taak toepassingspakketten opgeeft, de toepassing wordt gedistribueerd alleen op de knooppunten die zijn gepland om te worden uitgevoerd ten minste een van de taken van de taak voordat de opdrachtregel van de taak wordt uitgevoerd.

Batch verwerkt de details van het werken met Azure opslagruimte voor het opslaan van uw toepassingspakketten en implementeren om computerknooppunten, zodat de overhead van de code en het beheer kan worden vereenvoudigd.

Uitchecken als u wilt meer weten over de functie voor het pakket van toepassing, [implementatie van toepassingen met Azure Batch toepassingspakketten](batch-application-packages.md).

>[AZURE.NOTE] Als u toepassingen toepassingspakketten aan een *bestaande* toepassingen toevoegen, moet u opnieuw opstarten computerknooppunten voor de toepassingspakketten op de knooppunten kunnen worden geïmplementeerd.

## <a name="pool-and-compute-node-lifetime"></a>Levensduur van het knooppunt en het berekenen

Wanneer u uw Azure Batch-oplossing ontwerpt, hebt u om een ontwerpbesluit over hoe en wanneer toepassingen worden gemaakt, en berekenen hoe lang knooppunten binnen de groepen ter beschikking worden gehouden.

U kunt aan één kant van het spectrum maakt een groep van toepassingen voor elke taak die u wilt verzenden en de groep te verwijderen zodra de uitvoering van haar taken zijn voltooid. Hiermee maximaliseert gebruik omdat de knooppunten alleen toegewezen zijn wanneer nodig en afsluiten als ze niet actief zijn. Dit betekent dat de taak moet wachten tot de knooppunten toe te wijzen, is het belangrijk te weten dat taken worden gepland voor uitvoering als knooppunten afzonderlijk beschikbaar toegewezen zijn, en de starttaak heeft voltooid. Batch wordt *niet* gewacht totdat alle knooppunten binnen een groep beschikbaar zijn voordat u taken toewijst aan de knooppunten. Dit zorgt voor maximale benutting van alle beschikbare knooppunten.

Aan de andere kant als taken onmiddellijk beginnen met de hoogste prioriteit is u van tevoren een toepassingen maken en knooppunten beschikbaar maken voordat projecten worden ingediend. In dit scenario taken direct kunnen starten, maar knooppunten inactief kunnen blijven tijdens het wachten om te worden toegewezen.

Een gecombineerde benadering wordt meestal gebruikt voor het verwerken van een variabele, maar voortdurend, belasting. Hebt u een groep van toepassingen die meerdere taken te worden ingediend, maar kunnen het aantal knooppunten omhoog of omlaag, afhankelijk van het project worden geladen (Zie [schaal berekenen bronnen](#scaling-compute-resources) in de volgende sectie). U kunt dit doen ontwikkelingscyclus, die is gebaseerd op de huidige belasting of proactief, belasting kan worden voorspeld.

## <a name="scaling-compute-resources"></a>Schalen compute-bronnen

Met [automatisch schalen](batch-automatic-scaling.md)hebt u de Batch-service die het aantal computerknooppunten in een groep op basis van de huidige werkbelasting en brongebruik van uw scenario compute dynamisch aanpassen. Hiermee kunt u lagere totale kosten van uw toepassing wordt uitgevoerd met behulp van de benodigde bronnen en brengt die u niet nodig hebt.

U kunt automatisch schalen door het schrijven van een [formule van automatische schaling](batch-automatic-scaling.md#automatic-scaling-formulas) en die formule koppelen aan een groep. De Batch-service gebruikt de formule om te bepalen van het doel aantal knooppunten in de groep voor de volgende schaal interval (interval die u kunt configureren). U kunt de instellingen voor automatische schaling voor een groep opgeven wanneer u deze maakt of inschakelen van verderop schalen op een groep. U kunt ook de instellingen voor schalen op een groep schaling ingeschakeld bijwerken.

Als u bijvoorbeeld misschien vereist een taak dat u een zeer groot aantal taken verzendt moeten worden uitgevoerd. Een schaal formule kunt u toewijzen aan de groep die het aantal knooppunten in de pool op basis van het aantal taken in de wachtrij en de snelheid van de voltooiing van de taken in het project worden aangepast. De Batch-service periodiek wordt de formule geëvalueerd en wordt de groep, op basis van werkbelasting (knooppunten voor veel taken in de wachtrij toevoegen en verwijderen van knooppunten voor geen taken in de wachtrij of wordt uitgevoerd) en andere instellingen voor uw formule.

Een formule schaal kan worden gebaseerd op de volgende criteria:

- **Tijd metrics** zijn gebaseerd op statistieken die zijn verzameld in het opgegeven aantal uren om de vijf minuten.

- **Bron cijfers** zijn gebaseerd op CPU-gebruik, bandbreedte, geheugengebruik en aantal knooppunten.

- **Taak statistieken** zijn gebaseerd op de taakstatus, zoals *actieve* (in de wachtrij geplaatst), *uitgevoerd*of *voltooid*.

Wanneer automatische schaling het aantal knooppunten in een groep berekenen vermindert, moet u rekening houden met het verwerken van taken die worden uitgevoerd op het moment van de daling-bewerking. Batch biedt daarvoor een *knooppunt af te breken-optie* die u wilt in uw formules opnemen. U kunt bijvoorbeeld opgeven dat actieve taken worden onmiddellijk gestopt, onmiddellijk gestopt en vervolgens opnieuw wordt uitgevoerd op een ander knooppunt of toegestaan voltooien voordat het knooppunt wordt verwijderd uit de groep.

Zie [automatisch schalen computerknooppunten in een Batch Azure-toepassingen](batch-automatic-scaling.md)voor meer informatie over het automatisch schalen van een toepassing.

> [AZURE.TIP] Optimaliseer het Resourcegebruik berekenen, stelt u doel van knooppunten bij nul aan het einde van een taak, maar u kunt het uitvoeren van taken te voltooien.

## <a name="security-with-certificates"></a>Beveiliging met certificaten

Meestal moet u certificaten gebruiken wanneer u versleutelen of ontsleutelen van vertrouwelijke gegevens voor taken, zoals de sleutel voor een [Azure opslag rekening][azure_storage]. Daarom kunt u certificaten installeren op de knooppunten. Gecodeerde geheimen worden doorgegeven aan de taken via opdrachtregelparameters of ingesloten in een van de resources voor de taak en geïnstalleerde certificaten kunnen worden gebruikt om ze te decoderen.

Gebruik van het [certificaat toevoegen] [ rest_add_cert] bewerking (Batch REST) of [CertificateOperations.CreateCertificate] [ net_create_cert] (Batch .NET) een certificaat toevoegen aan de account van een Batch-methode. Vervolgens koppelt u het certificaat naar een nieuwe of bestaande groep. Wanneer een certificaat gekoppeld aan een groep van toepassingen is, installeert u het certificaat met de Batch-service op elk knooppunt in de groep. De Batch-service installeert de juiste certificaten wanneer het knooppunt wordt gestart, voordat u start taken (met inbegrip van de starttaak en taak manager).

Als u certificaten aan een *bestaande* groep van toepassingen toevoegen, moet u de computerknooppunten voor de certificaten moeten worden toegepast op de knooppunten opnieuw opstarten.

## <a name="error-handling"></a>Foutafhandeling

U vindt het misschien nodig voor het verwerken van de taak en de toepassing storingen in uw oplossing Batch.

### <a name="task-failure-handling"></a>Fout bij verwerking van taak
Fouten vallen in deze categorieën:

- **Fouten bij de planning**

    Als de overdracht van bestanden die zijn opgegeven voor een taak om welke reden dan ook mislukt, wordt een 'planning fout' ingesteld voor de taak.

    Planning fouten kan optreden als de bronbestanden van de taak hebt verplaatst, de opslag-account beschikbaar is of een ander probleem is opgetreden waardoor de succesvolle kopiëren van bestanden naar het knooppunt.

- **Toepassingsfouten**

    Het proces dat is opgegeven met de opdrachtregel van de taak kan ook mislukken. Het proces wordt geacht te zijn mislukt wanneer u een afsluitcode van nul wordt geretourneerd door het proces dat door de taak wordt uitgevoerd (Zie *afsluitcodes taak* in de volgende sectie).

    U kunt voor toepassingsfouten, Batch als u de taak tot aan een opgegeven aantal keren automatisch opnieuw wilt configureren.

- **Beperking fouten**

    U kunt een beperking waarmee de maximale uitvoering duur voor een taak of taak, het *maxWallClockTime*instellen. Dit is handig voor taken "vastgelopen" beëindigd.

    Wanneer de maximale hoeveelheid tijd is overschreden, wordt de taak als *voltooid*is gemarkeerd, maar wordt de afsluitcode ingesteld op `0xC000013A` en het *schedulingError* -veld is gemarkeerd als `{ category:"ServerError", code="TaskEnded"}`.

### <a name="debugging-application-failures"></a>Opsporen van toepassingsfouten

- `stderr`en`stdout`

    Tijdens de uitvoering kan een toepassing diagnostische uitvoer die u gebruiken kunt voor het oplossen van problemen met opleveren. Zoals vermeld in de sectie [bestanden en mappen](#files-and-directories), schrijft de Batch service standaarduitvoer en standaardfouten uitvoer naar `stdout.txt` en `stderr.txt` bestanden in de map taak in de compute node. U kunt de Azure portal of een van de Batch-SDK's om deze bestanden te downloaden. Bijvoorbeeld, vindt u deze en andere bestanden voor het oplossen van problemen met behulp van [ComputeNode.GetNodeFile] [ net_getfile_node] en [CloudTask.GetNodeFile] [ net_getfile_task] in de bibliotheek Batch .NET.

- **De afsluitcodes van taak**

    Zoals eerder vermeld, wordt een taak is gemarkeerd als door de Batch-service mislukt als het proces dat wordt uitgevoerd door de taak een afsluitcode van nul als resultaat. Wanneer een taak wordt uitgevoerd met een proces, vult de Batch van de taak uitgang code eigenschap met de *geretourneerde code van het proces*. Het is belangrijk te weten dat de afsluitcode van een taak wordt **niet** bepaald door de Batch-service wordt bepaald door het proces zelf of het besturingssysteem waarop het proces wordt uitgevoerd.

### <a name="accounting-for-task-failures-or-interruptions"></a>De administratieve verwerking van fouten of onderbrekingen

Taken mislukken van tijd tot tijd of worden onderbroken. De toepassing van de taak zelf kan mislukken, het knooppunt waarop de taak wordt uitgevoerd, kan worden gestart of het knooppunt mogelijk verwijderd uit de groep tijdens een formaat als de groep van toepassingen niet toewijzen, beleid is ingesteld op knooppunten onmiddellijk verwijderen zonder te wachten totdat de taken te voltooien. In alle gevallen kan de taak worden automatisch opnieuw door de batchverwerking voor uitvoering op een ander knooppunt.

Het is ook mogelijk voor een regelmatig terugkerende probleem te veroorzaken van een taak vastloopt of duurt lang om uit te voeren. U kunt de maximale uitvoeringstijd voor een taak instellen. Als deze wordt overschreden, Batch wordt onderbroken door de toepassing van de taak.

### <a name="connecting-to-compute-nodes"></a>Verbinding maken met computerknooppunten

U kunt extra foutopsporing en het oplossen van problemen met het aanmelden bij een compute node op afstand uitvoeren. De Azure portal kunt u downloaden van een bestand Remote Desktop Protocol (RDP) voor Windows-knooppunten en Secure Shell (SSH) verbindingsinformatie voor Linux knooppunten verkrijgen. U kunt dit ook doen met behulp van de Batch-API's--bijvoorbeeld met [Batch.NET] [ net_rdpfile] of [Python, Batch](batch-linux-nodes.md#connect-to-linux-nodes).

>[AZURE.IMPORTANT] Als u wilt verbinden aan een knooppunt via RDP of SSH, moet u eerst een gebruiker maken op het knooppunt. Hiervoor kunt u de Azure portal, [een gebruikersaccount aan een knooppunt toevoegen] [ rest_create_user] aanroept met behulp van de Batch REST API, de [ComputeNode.CreateComputeNodeUser] [ net_create_user] methode in Batch .NET, of bel de [add_user] [ py_add_user] -methode in de Batch Python module.

### <a name="troubleshooting-bad-compute-nodes"></a>Troubleshooting "slecht" computerknooppunten

In situaties waarin uw taken mislukken, onderzoeken uw clienttoepassing Batch of de service de metagegevens van de mislukte taken identificeren een geheugenlek optreedt knooppunt. Elk knooppunt in een groep krijgt een unieke ID en het knooppunt waarop een taak wordt uitgevoerd, is opgenomen in de metagegevens van de taak. Nadat u hebt aangegeven dat een knooppunt van het probleem, kunt u verschillende acties mee uitvoeren:

- **Start het knooppunt opnieuw op** ([REST][rest_reboot] | [.NET][net_reboot])

    Het knooppunt opnieuw kan soms ruimen latente problemen zoals zitten of vastgelopen processen. Houd er rekening mee dat als uw groep een starttaak wordt of uw taak een taak voor de bereiding gebruikt, worden deze uitgevoerd wanneer het knooppunt opnieuw wordt opgestart.

- **Het knooppunt reimage** ([REST][rest_reimage] | [.NET][net_reimage])

    Dit installeert het besturingssysteem opnieuw op het knooppunt. Net als bij een knooppunt opnieuw is opgestart, start taken en projecttaken preparaat worden opnieuw uitgevoerd nadat het knooppunt heeft zijn reimaged.

- **Het knooppunt uit de groep verwijderen** ([REST][rest_remove] | [.NET][net_remove])

    Soms is het nodig het knooppunt volledig te verwijderen uit de groep.

- **Taak plannen op het knooppunt uitschakelen** ([REST][rest_offline] | [.NET][net_offline])

    Dit effectief neemt het knooppunt "offline" zodat geen verdere taken zijn toegewezen, maar kan het knooppunt blijft actief en in de groep. Hiermee kunt u verder onderzoek naar de oorzaak van de mislukte uitvoeren zonder verlies van gegevens van de mislukte taak-- en zonder het knooppunt veroorzaakt meer fouten. U kunt bijvoorbeeld taken worden gepland op het knooppunt vervolgens [aanmelden op afstand](#connecting-to-compute-nodes) bekijken van gebeurtenislogboeken van het knooppunt of andere problemen oplossen door het uitschakelen. Nadat u klaar bent met uw onderzoek kunt u vervolgens het knooppunt weer on line brengen doordat het plannen van taken ([REST][rest_online] | [.NET][net_online]), of een van de andere hierboven beschreven acties uit te voeren.

> [AZURE.IMPORTANT] Bij elke actie die wordt beschreven in deze sectie--opnieuw opstarten, reimage verwijderen en uitschakelen taakplanning--u kunt opgeven hoe de taken die momenteel worden uitgevoerd op het knooppunt worden verwerkt wanneer u de actie uitvoert. Bijvoorbeeld bij het plannen van taken op een knooppunt met behulp van de Batch .NET client library is uitgeschakeld, kunt u een [DisableComputeNodeSchedulingOption] [ net_offline_option] enum-waarde op te geven of te **beëindigen** actieve taken, **Requeue** ze voor het plannen van de andere knooppunten of toestaan taken te voltooien voordat u de actie (**TaskCompletion**) wordt uitgevoerd.

## <a name="next-steps"></a>Volgende stappen

- Een Batch voorbeeldtoepassing in [aan de slag met de bibliotheek Azure Batch voor .NET](batch-dotnet-get-started.md)stapsgewijs doorlopen. Er is ook een [Python versie](batch-python-tutorial.md) van de zelfstudie waarop een werkbelasting computerknooppunten Linux wordt uitgevoerd.

- Downloaden en bouwen van de [Batch Explorer] [ github_batchexplorer] voorbeeldproject voor gebruik tijdens het ontwikkelen van oplossingen voor uw Batch. Met behulp van de Batch Explorer, kunt u de volgende en nog veel meer uitvoeren:
  - Controleren en bewerken van toepassingen, taken en taken binnen uw account Batch
  - Download `stdout.txt`, `stderr.txt`, en andere bestanden van knooppunten
  - Gebruikers maken op knooppunten en RDP-bestanden voor externe aanmelding downloaden

- Meer informatie over het [maken van toepassingen van Linux-computerknooppunten](batch-linux-nodes.md).

- Ga naar het [forum Azure Batch] [ batch_forum] op MSDN. Het forum is een goede plaats om te vragen, of gewoon leren of een deskundige bij het gebruik van de Batch zijn.

[1]: ./media/batch-api-basics/node-folder-structure.png

[azure_storage]: https://azure.microsoft.com/services/storage/
[batch_forum]: https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurebatch
[cloud_service_sizes]: ../cloud-services/cloud-services-sizes-specs.md
[msmpi]: https://msdn.microsoft.com/library/bb524831.aspx
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_sample_taskdeps]:  https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/TaskDependencies
[github_batchexplorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[batch_net_api]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[msdn_env_vars]: https://msdn.microsoft.com/library/azure/mt743623.aspx
[net_cloudjob_jobmanagertask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobmanagertask.aspx
[net_cloudjob_priority]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.priority.aspx
[net_cloudpool_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.starttask.aspx
[net_cloudtask_env]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.environmentsettings.aspx
[net_create_cert]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.certificateoperations.createcertificate.aspx
[net_create_user]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.createcomputenodeuser.aspx
[net_getfile_node]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.getnodefile.aspx
[net_getfile_task]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.getnodefile.aspx
[net_job_env]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.commonenvironmentsettings.aspx
[net_multiinstancesettings]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.aspx
[net_onalltaskscomplete]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.onalltaskscomplete.aspx
[net_rdp]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.getrdpfile.aspx
[net_reboot]: https://msdn.microsoft.com/library/azure/mt631495.aspx
[net_reimage]: https://msdn.microsoft.com/library/azure/mt631496.aspx
[net_remove]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.removefrompoolasync.aspx
[net_offline]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.disableschedulingasync.aspx
[net_online]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.enableschedulingasync.aspx
[net_offline_option]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.common.disablecomputenodeschedulingoption.aspx
[net_rdpfile]: https://msdn.microsoft.com/library/azure/Mt272127.aspx
[vnet]: https://msdn.microsoft.com/library/azure/dn820174.aspx#bk_netconf

[py_add_user]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.operations.html#azure.batch.operations.ComputeNodeOperations.add_user

[batch_rest_api]: https://msdn.microsoft.com/library/azure/Dn820158.aspx
[rest_add_job]: https://msdn.microsoft.com/library/azure/mt282178.aspx
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
[rest_add_cert]: https://msdn.microsoft.com/library/azure/dn820169.aspx
[rest_add_task]: https://msdn.microsoft.com/library/azure/dn820105.aspx
[rest_create_user]: https://msdn.microsoft.com/library/azure/dn820137.aspx
[rest_get_task_info]: https://msdn.microsoft.com/library/azure/dn820133.aspx
[rest_job_schedules]: https://msdn.microsoft.com/library/azure/mt282179.aspx
[rest_multiinstance]: https://msdn.microsoft.com/library/azure/mt637905.aspx
[rest_multiinstancesettings]: https://msdn.microsoft.com/library/azure/dn820105.aspx#multiInstanceSettings
[rest_update_job]: https://msdn.microsoft.com/library/azure/dn820162.aspx
[rest_rdp]: https://msdn.microsoft.com/library/azure/dn820120.aspx
[rest_reboot]: https://msdn.microsoft.com/library/azure/dn820171.aspx
[rest_reimage]: https://msdn.microsoft.com/library/azure/dn820157.aspx
[rest_remove]: https://msdn.microsoft.com/library/azure/dn820194.aspx
[rest_offline]: https://msdn.microsoft.com/library/azure/mt637904.aspx
[rest_online]: https://msdn.microsoft.com/library/azure/mt637907.aspx

[vm_marketplace]: https://azure.microsoft.com/marketplace/virtual-machines/
