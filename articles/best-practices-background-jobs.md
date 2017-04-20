<properties
   pageTitle="Achtergrond taken richtlijnen | Microsoft Azure"
   description="Richtlijnen voor de achtergrond taken die uitgevoerd onafhankelijk van de gebruikersinterface."
   services=""
   documentationCenter="na"
   authors="dragon119"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="best-practice"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/21/2016"
   ms.author="masashin"/>

# <a name="background-jobs-guidance"></a>Achtergrond taken richtlijnen

[AZURE.INCLUDE [pnp-header](../includes/guidance-pnp-header-include.md)]

## <a name="overview"></a>Overzicht

Vele soorten toepassingen vereisen achtergrondtaken die worden uitgevoerd onafhankelijk van de gebruikersinterface (UI). Voorbeelden zijn de batchverwerkingen, intensieve taken en langdurige processen zoals werkstromen. Achtergrondtaken kunnen worden uitgevoerd zonder tussenkomst van de gebruiker--de toepassing kan de taak start en gaat u verder om interactieve aanvragen van gebruikers te verwerken. Dit kan helpen minimaliseren de belasting van de toepassing UI, verbetering van de beschikbaarheid en responstijden interactieve verminderen.

Bijvoorbeeld als een toepassing is vereist voor het genereren van miniatuurweergaven van afbeeldingen die door gebruikers zijn geüpload, kunt het doen als een achtergrondtaak en sla de miniatuur op opslag wanneer deze voltooid, zonder dat de gebruiker hoeft is te wachten tot het proces wordt voltooid. Op dezelfde manier, het plaatsen van een order van een gebruiker tot stand kan brengen een werkstroom achtergrond die de order verwerkt, terwijl de gebruikersinterface kan de gebruiker om door te gaan met surfen op het web app. Als de achtergrondtaak voltooid is, kan het bijwerken van de gegevens opgeslagen orders en een e-mailbericht verzenden naar de gebruiker die de order bevestigt.

Wanneer u in overweging nemen of voor de uitvoering van een taak als een achtergrondtaak, de belangrijkste criteria is of de taak kan worden uitgevoerd zonder tussenkomst van de gebruiker en zonder de UI hoeft te wachten totdat de taak moet zijn voltooid. Taken waarvoor de gebruiker of de gebruikersinterface om te wachten terwijl ze voltooid zijn mogelijk niet geschikt als achtergrondtaken.

## <a name="types-of-background-jobs"></a>Soorten achtergrondtaken

Achtergrondtaken omvatten een of meer van de volgende soorten taken:

- CPU-intensieve taken, zoals wiskundige berekeningen of structurele model analyse.
- I/O-intensieve taken, zoals het uitvoeren van een reeks transacties opslag of het indexeren van bestanden.
- Batchverwerkingen, zoals nachtelijke bijwerkt of geplande verwerking.
- Langdurige werkstromen, zoals orderafhandeling of het aanbod van diensten en systemen.
- Verwerking van gevoelige gegevens waarop de taak wordt doorgegeven aan een veilige locatie voor verwerking. Bijvoorbeeld, misschien u niet wilt verwerken gevoelige gegevens in een web app. In plaats daarvan kunt u een patroon zoals [Gatekeeper](http://msdn.microsoft.com/library/dn589793.aspx) de gegevens overbrengen naar een geïsoleerde achtergrondproces dat toegang tot beveiligde opslag heeft.

## <a name="triggers"></a>Triggers

Achtergrondtaken kunnen op verschillende manieren worden gestart. Ze vallen in een van de volgende categorieën:

- [**Gebeurtenisgestuurde triggers**](#event-driven-triggers). De taak wordt gestart in reactie op een gebeurtenis, meestal een actie van een gebruiker of een stap in een werkstroom.
- [**Triggers voor planning op basis van beschikbare resources**](#schedule-driven-triggers). De taak wordt aangeroepen volgens een schema op basis van een timer. Dit kan zijn een terugkerend schema of een eenmalige aanroep die is opgegeven voor een later tijdstip.

### <a name="event-driven-triggers"></a>Gebeurtenisgestuurde triggers

Gebeurtenisgestuurde aanroep wordt een trigger start de taak op de achtergrond. Voorbeelden van het gebruik van gebeurtenisgestuurde triggers zijn:

- De UI of een andere taak wordt een bericht in een wachtrij geplaatst. Het bericht bevat gegevens over een actie die heeft plaatsgevonden, zoals de gebruiker een bestelling te plaatsen. Taak op de achtergrond voor deze wachtrij luistert en detecteert de aankomst van een nieuw bericht. Het bericht wordt gelezen en de gegevens erin worden gebruikt als invoer voor de achtergrondtaak.
- De UI of een andere taak worden opgeslagen, of een waarde in de opslag. Taak op de achtergrond de opslag gecontroleerd en wijzigingen gedetecteerd. De gegevens worden gelezen en gebruikt als invoer voor de achtergrondtaak.
- De UI of een andere taak indient een verzoek om een eindpunt, zoals een URI HTTPS of een API die wordt weergegeven als een webservice. De gegevens die nodig is voor het voltooien van de taak op de achtergrond als onderdeel van de aanvraag worden doorgegeven. De webservice eindpunt roept taak op de achtergrond, die de gegevens als invoer gebruikt.

Typische voorbeelden van taken die voor gebeurtenisgestuurde aanroep geschikt zijn zijn beeldbewerking, werkstromen, het verzenden van gegevens met externe services, e-mailberichten verzenden en inrichten van nieuwe gebruikers in multitenant-toepassingen.

### <a name="schedule-driven-triggers"></a>Planning op basis van beschikbare resources triggers

Planning op basis van beschikbare resources aanroep wordt een timer start de taak op de achtergrond. Voorbeelden van het gebruik van planning op basis van beschikbare resources triggers zijn:

- Een timer die lokaal wordt uitgevoerd in de toepassing of als onderdeel van het besturingssysteem van de toepassing roept een achtergrondtaak op regelmatige basis.
- Een timer die wordt uitgevoerd in een andere toepassing of een timerservice zoals Azure Scheduler, verzendt een verzoek naar een webservice API op een regelmatige basis. De webservice-API aanroept taak op de achtergrond.
- Een afzonderlijk proces of een toepassing start een timer die ervoor zorgt de taak dat moet worden gestart na een opgegeven vertraging eenmaal of op een bepaalde tijd op de achtergrond.

Typische voorbeelden van taken die voor planning op basis van beschikbare resources aanroep geschikt zijn zijn batch-verwerking routines (zoals de bijwerking van de lijsten voor gerelateerde producten voor gebruikers op basis van hun recente gedrag), gegevensverwerking routinematige taken (zoals het bijwerken van indexen of samengevoegde resultaten genereren), gegevensanalyse voor dagelijkse rapporten, gegevens bewaren opschonen en controle gegevens.

Als u een taak plannen op basis van hoeveelheid werk moet worden uitgevoerd als een enkel exemplaar, worden op de hoogte van de volgende opties:

- Als de compute-exemplaar met de scheduler (zoals een virtuele machine met Windows geplande taken) aangepast, hebt u meerdere exemplaren van de scheduler is uitgevoerd. Dit kunnen meerdere exemplaren van de taak worden gestart.
- Als taken uitgevoerd langer dan de periode tussen de scheduler gebeurtenissen, kan de scheduler nog een exemplaar van de taak starten terwijl de vorige taak is nog actief.

## <a name="returning-results"></a>Resultaten

Achtergrondtaken uitvoeren asynchroon in een afzonderlijk proces, of zelfs op een andere locatie, vanuit de gebruikersinterface of het proces dat de taak op de achtergrond geactiveerd. In het ideale geval achtergrondtaken ' brand-en vergeet ', en hun uitvoering heeft geen invloed op de gebruikersinterface of het aanroepende proces. Dit betekent dat het aanroepende proces niet totdat de taken zijn voltooid wacht. Dus het kan niet automatisch detecteren wanneer de taak wordt beëindigd.

Als u een achtergrondtaak om te communiceren met de aanroepende taak om aan te geven van de uitvoering of voltooiing vereist, moet u een mechanisme voor deze implementeren. Enkele voorbeelden zijn:

- Een waarde statusindicator schrijven naar de opslag die toegankelijk is voor de gebruikersinterface of de beller taak kunt controleren of u deze waarde als dat nodig is. Andere gegevens die de taak op de achtergrond naar de aanroepende functie terugkeren moet kan worden geplaatst in dezelfde opslag.
- Stellen een antwoordenwachtrij die de gebruikersinterface of de beller luistert. Taak op de achtergrond kunt u berichten verzenden naar de wachtrij die geven de status en de voltooiing. Gegevens die de taak op de achtergrond naar de aanroepende functie terugkeren moet kan worden geplaatst in de berichten. Als u van Azure Service Bus gebruikmaakt, kunt u de eigenschappen **ReplyTo** en **CorrelationId** voor de uitvoering van deze mogelijkheid. Zie de [correlatie in de Service Bus Brokered Messaging](http://www.cloudcasts.net/devguide/Default.aspx?id=13029)voor meer informatie.
- Een API- of eindpunt van de taak op de achtergrond waarop de gebruikersinterface of de beller kan verkrijgen van statusgegevens worden blootgesteld. Gegevens die de taak op de achtergrond naar de aanroepende functie terugkeren moet kunnen worden opgenomen in het antwoord.
- Terugbellen naar de gebruikersinterface of de beller via een API om aan te geven de status op vooraf gedefinieerde momenten of na de taak op de achtergrond hebben. Dit is mogelijk via de gebeurtenissen lokaal of via een mechanisme voor publicaties en abonnementen. Gegevens die de taak op de achtergrond naar de aanroepende functie terugkeren moet kunnen worden opgenomen in de nettolading van de aanvraag of gebeurtenis.

## <a name="hosting-environment"></a>Hosting-omgeving

U kunt de achtergrondtaken met behulp van een reeks verschillende Azure platform services hosten:

- [**Azure Web Apps en WebJobs**](#azure-web-apps-and-webjobs). U kunt WebJobs gebruiken voor het uitvoeren van aangepaste taken op basis van een bereik van verschillende soorten scripts of uitvoerbare programma's in het kader van een web app.
- [**Azure Cloud Services web en werknemer rollen**](#azure-cloud-services-web-and-worker-roles). U kunt de code in een functie die wordt uitgevoerd als een achtergrondtaak schrijven.
- [**Azure virtuele Machines**](#azure-virtual-machines). Een Windows-service of de Windows Taakplanner gebruikt, wordt het gebruikt als host voor uw achtergrondtaken binnen een specifieke virtuele machine.
- [**Azure Batch**](./batch/batch-technical-overview.md). Het is een platform-service die intensieve werk uit te voeren op een beheerde collectie van virtuele machines wordt gepland en kan automatisch schalen berekenen middelen aan de behoeften van uw taken.

In de volgende secties beschrijven elk van deze opties gedetailleerder en opnemen van overwegingen waarmee u de gewenste optie kiezen.

## <a name="azure-web-apps-and-webjobs"></a>Azure Web Apps en WebJobs

Azure-WebJobs kunt u uitvoeren van aangepaste taken als achtergrondtaken binnen een Azure Web App. WebJobs, uitgevoerd binnen de context van uw web app als een doorlopend proces. WebJobs worden ook uitgevoerd als reactie op een triggergebeurtenis van Azure planner of externe factoren, zoals wijzigingen in de BLOB's opslag en berichtenwachtrijen. Taken kunnen gestart en gestopt op afroep en afsluiten. Als een continu actief WebJob mislukt, het wordt automatisch opnieuw gestart. Fout-en opnieuw worden geconfigureerd.

Wanneer u een WebJob configureert:

- Als u wilt dat de taak om te reageren op een gebeurtenis gebaseerde trigger, moet u deze configureren als **doorlopend**. Het script of programma is opgeslagen in de map met de naam site/wwwroot/app_data/taken/continue.
- Als u wilt dat de taak om te reageren op een trigger planning op basis van beschikbare resources, moet u deze configureren uitvoert **volgens een schema**. Het script of programma is opgeslagen in de map met de naam site/wwwroot/app_data/taken/geactiveerd.
- Als u de optie **op aanvraag uitvoeren** bij het configureren van een taak, wordt deze dezelfde code als de optie **uitvoeren volgens een schema** uitgevoerd wanneer u het programma start.

Azure WebJobs uitgevoerd in de sandbox van het web app. Dit betekent dat ze kunnen toegang omgevingsvariabelen tot en informatie, zoals verbindingstekenreeksen met een met het web app delen. De taak heeft toegang tot de unieke id van de computer waarop de taak wordt uitgevoerd. De verbindingsreeks met de naam **AzureWebJobsStorage** biedt toegang tot wachtrijen opslag Azure BLOB's en tabellen voor de toepassingsgegevens en toegang tot de Service Bus voor messaging en communicatie. De verbindingsreeks met de naam **AzureWebJobsDashboard** biedt toegang tot de logboekbestanden van de actie van de taak.

Azure WebJobs hebben de volgende kenmerken:

- **Beveiliging**: WebJobs worden beschermd door de referenties van de implementatie van de web app.
- **Ondersteunde bestandstypen**: U kunt WebJobs definiëren met behulp van opdrachtscripts (.cmd), batch-bestanden (.bat), PowerShell-scripts (.ps1), bash shell-scripts (.sh), PHP-scripts (.php) scripts Python (.py), JavaScript-code (js) en uitvoerbare programma's (.exe, JAR en meer).
- **Implementatie**: U kunt scripts en uitvoerbare bestanden implementeren met behulp van de portal Azure, via de [WebJobsVs](https://visualstudiogallery.msdn.microsoft.com/f4824551-2660-4afa-aba1-1fcc1673c3d0) in voor Visual Studio of [Visual Studio 2013 Update 4](http://www.visualstudio.com/news/vs2013-update4-rc-vs) (u kunt maken en implementeren van deze optie), met behulp van de [SDK van Azure-WebJobs](./app-service-web/websites-dotnet-webjobs-sdk-get-started.md), of door ze te kopiëren rechtstreeks naar de volgende locaties:
  - Geactiveerd voor uitvoering: site/wwwroot/app_data/taken/geactiveerd / {job naam}
  - Voor continue uitvoering: site/wwwroot/app_data/taken/continue / {job naam}
- **Registratie**: Console.Out (gemarkeerde) wordt beschouwd als INFO. Console.Error wordt beschouwd als een fout. Controle en diagnose-informatie is toegankelijk via de portal Azure. U kunt logboekbestanden rechtstreeks vanaf de site downloaden. Ze worden opgeslagen op de volgende locaties:
  - Geactiveerd voor uitvoering: Vfs/data/taken/geactiveerd/taaknaam
  - Voor continue uitvoering: Vfs/data/taken/continue/taaknaam
- **Configuratie**: U WebJobs kunt configureren met behulp van de portal, de REST API en PowerShell. Configuratie-informatie voor een project kunt u een configuratiebestand met de naam settings.job in de hoofdmap hetzelfde als het script van de taak. Bijvoorbeeld:
  - {"stopping_wait_time": 60}
  - {"is_singleton": true}

### <a name="considerations"></a>Overwegingen met betrekking tot

- Standaard WebJobs schaal met de web app. U kunt echter taken uit te voeren op één exemplaar van de **is_singleton** configuratie-eigenschap instellen op **true**. SIS WebJobs zijn handig voor taken die u niet wilt schalen of als meerdere gelijktijdige instanties, zoals het opnieuw indexeren gegevensanalyse en soortgelijke taken.
- Om te minimaliseren van de impact van de projecten op de prestaties van de web app, kunt u een lege Azure Web App-exemplaar maken in een nieuwe App serviceplan WebJobs die lang met host- of bronnaam intensieve.

### <a name="more-information"></a>Meer informatie

- [Azure WebJobs aanbevolen bronnen](./app-service-web/websites-webjobs-resources.md) staan veel nuttige bronnen, downloads en voorbeelden voor WebJobs.

## <a name="azure-cloud-services-web-and-worker-roles"></a>Azure Cloud Services-rollen voor web- en werknemer

Achtergrondtaken binnen de rol van een webpagina of in een afzonderlijke werknemer rol kan worden uitgevoerd. Wanneer u zijn besluit om de rol van een werknemer gebruiken, kunt u de schaalbaarheid en elasticiteit eisen, de levensduur van de taak, laat u cadence, beveiliging, fouttolerantie, bronconflicten, complexiteit en de logische architectuur. Zie voor meer informatie, [Berekenen Resource consolidatie patroon](http://msdn.microsoft.com/library/dn589778.aspx).

Er zijn verschillende manieren voor het implementeren van achtergrondtaken binnen een Cloud Services-rol:

- Een implementatie van de klasse **RoleEntryPoint** te maken in de rol en achtergrondtaken uitvoeren met de methoden ervan. De taken worden uitgevoerd in de context van WaIISHost.exe. Zij kunnen de **GetSetting** , methode van de klasse **CloudConfigurationManager** gebruiken om configuratieinstellingen te laden. Zie [Lifecycle (Cloud-Services)](#lifecycle-cloud-services)voor meer informatie.
- Opstarttaken gebruiken achtergrondtaken uitvoeren als de toepassing wordt gestart. Als u de taken te blijven op de achtergrond worden uitgevoerd, stelt u de eigenschap **taskType** **achtergrond** (als u dit doet, het opstarten van de toepassing zal stoppen en wachten totdat de taak is voltooid). Zie [opstarttaken in Azure uitvoeren](./cloud-services/cloud-services-startup-tasks.md)voor meer informatie.
- De WebJobs SDK gebruiken voor het implementeren van achtergrondtaken zoals WebJobs die worden gestart als een taak is gestart. Zie [aan de slag met de WebJobs Azure SDK](./app-service-web/websites-dotnet-webjobs-sdk-get-started.md)voor meer informatie.
- Een taak starten gebruiken voor de installatie van een Windows-service die een of meer achtergrondtaken wordt uitgevoerd. U moet de eigenschap **taskType** instellen op de **achtergrond** , zodat de service wordt uitgevoerd op de achtergrond. Zie [opstarttaken in Azure uitvoeren](./cloud-services/cloud-services-startup-tasks.md)voor meer informatie.

### <a name="running-background-tasks-in-the-web-role"></a>Achtergrondtaken uitvoeren in de Webrol

Het belangrijkste voordeel van achtergrondtaken uitvoeren in de Webrol is op te slaan in de hosting kosten omdat er geen vereiste aanvullende rollen te implementeren.

### <a name="running-background-tasks-in-a-worker-role"></a>Achtergrondtaken uitvoeren in de rol van een werknemer

Achtergrondtaken uitvoeren in de rol van een werknemer heeft verschillende voordelen:

- U kunt schalen afzonderlijk voor elk type rol beheren. Bijvoorbeeld, moet u mogelijk meer exemplaren van een web-functie voor de ondersteuning van de huidige belasting, maar minder exemplaren van de rol van de werknemer die achtergrondtaken worden uitgevoerd. Schaal achtergrond taak compute exemplaren afzonderlijk van de rollen van de gebruikersinterface, kunt u hosting kosten, beperken terwijl de aanvaardbare prestaties.
- Het compatibele de verwerkingscapaciteit voor achtergrondtaken uit de Webrol. De Webrol die de UI biedt kan blijven reageren en het kan betekenen dat minder exemplaren vereist zijn ter ondersteuning van een bepaald volume van verzoeken van gebruikers.
- Hiermee kunt u de scheiding van de bezwaren te implementeren. Elk Roltype kan een specifieke reeks taken duidelijk omschreven en verwante implementeren. Dit maakt het ontwerpen en onderhouden van de code eenvoudiger omdat er minder onderlinge afhankelijkheid van code en functionaliteit tussen elke rol.
- Deze kunt u gegevens en gevoelige processen isoleren. Web-rollen die de gebruikersinterface implementeren hoeft bijvoorbeeld niet toegang hebben tot gegevens die wordt beheerd en gecontroleerd door de rol van een werknemer. Dit is handig in een betere beveiliging, vooral wanneer u een patroon zoals de [Gatekeeper patroon](http://msdn.microsoft.com/library/dn589793.aspx).  

### <a name="considerations"></a>Overwegingen met betrekking tot

De volgende punten rekening houden bij het kiezen van hoe en waar achtergrondtaken implementeren wanneer u Cloud Services web en werknemer rollen:

- Achtergrondtaken in een bestaande Webrol hosting bespaart de kosten van een afzonderlijke werknemer rol voor de volgende taken uitvoeren. Het is echter waarschijnlijk invloed op de prestaties en beschikbaarheid van de toepassing als er bronconflicten voor verwerking en andere. De rol van een afzonderlijke werknemer beschermt de Webrol van de gevolgen van langdurige of resource-intensieve achtergrondtaken.
- Als u met behulp van de klasse **RoleEntryPoint** achtergrondtaken, kunt u dit eenvoudig verplaatsen naar een andere rol. Als u de klasse in de rol van een webpagina maken en later besluit dat u wilt uitvoeren van de taken in de rol van een werknemer, kunt u de uitvoering van de klasse **RoleEntryPoint** verplaatsen in de rol van de werknemer.
- Opstarttaken zijn bedoeld om een programma of een script uit te voeren. Implementatie van een achtergrondtaak als een uitvoerbaar programma mogelijk moeilijker, vooral als het vereist ook de implementatie van afhankelijke assembly's. Het is mogelijk eenvoudiger te implementeren en gebruiken van een script voor het definiëren van een achtergrondtaak wanneer u opstarttaken.
- Uitzonderingen die ervoor zorgen een achtergrondtaak dat mislukken hebben andere gevolgen, afhankelijk van de manier waarop deze worden gehost:
  - Als u de klasse **RoleEntryPoint** benadering, zal een mislukte taak de rol op te starten zodat de taak automatisch opnieuw wordt opgestart. Dit kan invloed hebben op de beschikbaarheid van de toepassing. Om dit te voorkomen, ervoor te zorgen dat u robuuste binnen de klasse **RoleEntryPoint** en alle achtergrondtaken de verwerking van uitzonderingen bevatten. Gebruik code opnieuw op te starten van taken die niet als dat nodig is en de uitzondering om opnieuw de rol alleen als u zonder problemen de fout niet in uw code herstellen te genereren.
  - Als u opstarttaken, bent u verantwoordelijk voor de uitvoering van taken beheren en controleren of het niet.
- Beheren en controleren van opstarttaken is moeilijker dan het gebruik van de benadering van de klasse **RoleEntryPoint** . De Azure WebJobs SDK bevat echter een dashboard voor het beheer van WebJobs die u kunt bij opstarttaken starten te vereenvoudigen.

### <a name="more-information"></a>Meer informatie

- [Resource consolidatie patroon berekenen](http://msdn.microsoft.com/library/dn589778.aspx)
- [Aan de slag met de SDK Azure WebJobs](./app-service-web/websites-dotnet-webjobs-sdk-get-started.md)

## <a name="azure-virtual-machines"></a>Azure virtuele Machines

Achtergrondtaken kunnen worden uitgevoerd op een manier die voorkomt dat wordt ingezet voor het Web Apps van Azure Cloud-Services of deze opties mogelijk niet handig. Typische voorbeelden zijn Windows-services en hulpprogramma's van derden en uitvoerbare programma's. Een ander voorbeeld is mogelijk programma's die zijn geschreven voor een omgeving die anders is dan die als host voor de toepassing. Het kan bijvoorbeeld een Unix- of Linux-programma dat u wilt uitvoeren vanaf een Windows- of .NET-toepassing zijn. U kunt kiezen uit een reeks van besturingssystemen voor een Azure virtuele machine en uw service of uitvoerbaar bestand uitvoeren op die virtuele machine.

Als u kiest voor het gebruik van virtuele Machines, Zie [Azure App Services, Cloud Services en vergelijking van virtuele Machines](./app-service-web/choose-web-site-cloud-service-vm.md). Zie voor informatie over de opties voor virtuele Machines, [grootte van de virtuele Machine en Cloud Service voor Azure](http://msdn.microsoft.com/library/azure/dn197896.aspx). Zie voor meer informatie over de besturingssystemen en vooraf gedefinieerde afbeeldingen die beschikbaar voor virtuele Machines zijn, [Azure virtuele Machines Marketplace](https://azure.microsoft.com/gallery/virtual-machines/).

Om de taak op de achtergrond in een afzonderlijke virtuele machine start, hebt u een aantal opties:

- U kunt de taak op aanvraag uitvoeren rechtstreeks vanuit uw toepassing een aanvraag verzenden naar een eindpunt dat toegang biedt tot de taak. Dit geeft in alle gegevens die de taak is vereist. Dit eindpunt roept de taak.
- U kunt de taak uit te voeren op een schema met een planner of een timer die beschikbaar is in het gekozen besturingssysteem. Bijvoorbeeld, in Windows kunt u Windows Taakplanner voor het uitvoeren van scripts en taken. Of als u SQL Server geïnstalleerd op de virtuele machine hebt, kunt u de SQL Server Agent-scripts en taken uitvoeren.
- Azure Scheduler kunt u de taak starten door een bericht naar een wachtrij die de taak luistert toe te voegen of door een verzoek te sturen naar een API die de taak bevat.

Zie de vorige sectie [Triggers](#triggers) voor meer informatie over hoe u de achtergrondtaken kunt starten.  

### <a name="considerations"></a>Overwegingen met betrekking tot

U worden besloten of achtergrondtaken in een Azure virtuele machine te implementeren, moet u rekening houden met de volgende punten:

- Achtergrondtaken in een aparte Azure virtuele machine host biedt flexibiliteit en kunt nauwkeurige controle over de opening, uitvoering, planning en toewijzing van middelen. Echter wordt verhoogd runtime kosten als een virtuele machine alleen om uit te voeren achtergrondtaken moeten worden geïmplementeerd.
- Er is geen voorziening voor het controleren van de taken in de Azure portal en geen automatische herstart mogelijkheden voor mislukte taken--maar de algemene status van de virtuele machine en beheert via de [Azure Service Management-Cmdlets](http://msdn.microsoft.com/library/azure/dn495240.aspx). Er zijn echter geen middelen om de processen en threads in computerknooppunten bepalen. Met behulp van een virtuele machine moet meestal extra inspanningen voor de uitvoering van een mechanisme waarmee gegevens worden verzameld van instrumenten in de taak en van het besturingssysteem op de virtuele machine. Het [System Center Management Pack voor Azure](http://technet.microsoft.com/library/gg276383.aspx)is één oplossing die thuishoren.
- U kunt controle sondes die worden weergegeven via een HTTP-eindpunten te maken. De code voor deze sondes kan controles uitvoeren, statistieken-- en operationele informatie verzamelen of foutinformatie verzamelen en terugsturen naar een toepassing. Zie voor meer informatie, [Gezondheid eindpunt Monitoring patroon](http://msdn.microsoft.com/library/dn589789.aspx).

### <a name="more-information"></a>Meer informatie

- [Virtuele Machines](https://azure.microsoft.com/services/virtual-machines/) op Azure
- [Veelgestelde vragen over Azure virtuele Machines](virtual-machines/virtual-machines-linux-classic-faq.md)

## <a name="design-considerations"></a>Overwegingen bij het ontwerpen

Er zijn verschillende fundamentele factoren rekening houden bij het ontwerpen van achtergrondtaken. De volgende secties worden gepartitioneerd, conflicten en coördinatie.

## <a name="partitioning"></a>Partitioneren

Als u achtergrond taken binnen een bestaand exemplaar berekenen (zoals een web app, web-functie, bestaande rol werknemer of virtuele machine), moet u overwegen hoe dit heeft invloed op de kenmerken van de kwaliteit van de compute-exemplaar en de taak op de achtergrond zelf. Van deze factoren kunt u bepalen of de taken met het bestaande exemplaar van de compute colocate of onderscheiden in een aparte compute-exemplaar:

- **Beschikbaarheid**: achtergrondtaken mogelijk niet hetzelfde niveau van beschikbaarheid van andere delen van de toepassing, in het bijzonder hebben de UI en andere onderdelen die rechtstreeks bij de interactie van de gebruiker betrokken zijn. Achtergrondtaken mogelijk grotere tolerantie voor latentie, opnieuw geprobeerd verbinding mislukt, en andere factoren die van invloed zijn op beschikbaarheid omdat de bewerkingen in de wachtrij kunnen worden geplaatst. Er moet wel voldoende capaciteit om te voorkomen dat de back-up van de aanvragen die van invloed zijn op de toepassing als geheel en wachtrijen geblokkeerd.
- **Schaalbaarheid**: achtergrondtaken zijn waarschijnlijk een vereiste verschillende schaalbaarheid dan de UI en de interactieve onderdelen van de toepassing. Schaal van de gebruikersinterface kan het nodig zijn om te voldoen aan de pieken in de vraag, terwijl uitstaande achtergrondtaken tijdens de minder drukke tijdstippen kunnen worden voltooid door een kleiner aantal compute exemplaren.
- **Tolerantie**: storing van een compute instance waarin alleen achtergrondtaken mogelijk niet fatally afbreuk aan de toepassing als geheel als de aanvragen voor deze taken worden in de wachtrij geplaatst of uitgesteld totdat de taak weer beschikbaar is. Als de compute-instantie en/of taken opnieuw kunnen worden gestart in een adequate wachttijd, gebruikers van de toepassing mogelijk niet van invloed.
- **Beveiliging**: achtergrondtaken kunnen ook verschillende beveiligingseisen of beperkingen dan voor de gebruikersinterface of andere delen van de toepassing. Met behulp van een instantie afzonderlijk berekenen, kunt u een andere beveiligingsomgeving voor de taken. U kunt ook patronen zoals Gatekeeper gebruiken voor het isoleren van de achtergrond compute exemplaren van de gebruikersinterface voor het optimale beveiligings- en.
- **Prestaties**: kunt u het type van compute exemplaar voor achtergrond taken naar met name de overeenkomst van de prestatie-eisen van de taken. Dit betekent met een goedkoper compute-optie als u de taken hoeft niet dezelfde verwerkingsmogelijkheden als de UI of een groter exemplaar als ze extra capaciteit en middelen nodig.
- **Beheerbaarheid**: achtergrondtaken mogelijk een verschillende ritme voor de ontwikkeling en implementatie van de code van het hoofdvenster van de toepassing of de gebruikersinterface. Deze kunnen op een aparte compute-instantie kunt vereenvoudigen, updates en versiebeheer.
- **Kosten**: toevoegen van instanties uit te voeren achtergrond taken verhogingen hosting kosten berekenen. De verhouding tussen extra capaciteit en deze extra kosten moet u zorgvuldig overwegen.

Voor meer informatie Zie [Leader Election patroon](http://msdn.microsoft.com/library/dn568104.aspx) en [Concurrerende consumenten patroon](http://msdn.microsoft.com/library/dn568101.aspx).

## <a name="conflicts"></a>Conflicten

Als u meerdere exemplaren van een achtergrondtaak hebt, is het mogelijk dat ze om toegang te krijgen tot bronnen en services, zoals databases en opslag concurreren zullen. Dit kan leiden tot bronconflicten die conflicten in de beschikbaarheid van de diensten en de integriteit van de gegevens in de opslag veroorzaken mogelijk. Bronconflicten kunt u oplossen met behulp van een pessimistische vergrendeling aanpak. Hiermee voorkomt u dat concurrerende exemplaren van een taak gelijktijdig toegang krijgen tot een service of gegevens te beschadigen.

Een andere manier conflicten op te lossen is om achtergrondtaken te definiëren als een singleton, dat er ooit slechts één exemplaar dat wordt uitgevoerd. Hierdoor wordt echter de betrouwbaarheid en prestaties voordelen die een configuratie met meerdere exemplaar leveren. Dit geldt met name als de gebruikersinterface voor het bezet houden van meer dan een achtergrondtaak voldoende werk kan leveren.

Het is essentieel om ervoor te zorgen dat taak op de achtergrond automatisch opnieuw opstarten kunt en dat er voldoende capaciteit om met pieken in de vraag. U kunt dit bereiken door een instantie compute met voldoende middelen toewijzen door het implementeren van een wachtrij mechanisme waarin de aanvragen voor later als vraag afneemt of door een combinatie van deze technieken.

## <a name="coordination"></a>Coördinatie

De achtergrondtaken kunnen complex zijn en mogelijk meerdere afzonderlijke taken uit te voeren om een resultaat te produceren of om te voldoen aan alle eisen. Het is gebruikelijk in deze scenario's aan de taak opsplitsen in kleinere onopvallende stappen of subtaken die kunnen worden uitgevoerd door meerdere klanten. Dergelijk complex taken kan zijn efficiënter en flexibeler omdat afzonderlijke stappen mogelijk geschikt zijn voor hergebruik in meerdere projecten. Het is ook eenvoudig toevoegen, verwijderen of wijzigen van de volgorde van de stappen.

Coördinatie van de verschillende sub- en deelactiviteiten kan lastig zijn, maar er zijn drie algemene patronen die u gebruiken kunt om uw implementatie van een oplossing:

- **Een taak in meerdere stappen voor herbruikbare ontleedt**. Een toepassing kan nodig zijn voor het uitvoeren van allerlei taken van verschillende complexiteit van de gegevens die worden verwerkt. Een eenvoudige, maar niet-flexibele benadering voor de implementatie van deze toepassing kan zijn voor het uitvoeren van deze verwerking als een monolithisch module. Deze aanpak is echter waarschijnlijk beperkt de mogelijkheden voor de refactoring van de code, zodat deze wordt geoptimaliseerd of het hergebruik van als onderdelen van de veredeling van de elders in de toepassing nodig. Zie voor meer informatie, [leidingen en Filters patroon](http://msdn.microsoft.com/library/dn568100.aspx).
- **Uitvoering van het werk voor een taak beheren**. Een toepassing kan taken uitvoeren die bestaan uit een aantal stappen (waarvan sommige mogelijk externe services starten of toegang krijgen tot externe bronnen). De afzonderlijke stappen mogelijk onafhankelijk van elkaar, maar ze zijn ingedeeld op basis van de toepassingslogica waarmee de taak. Voor meer informatie Zie [Taakplanner Agent Supervisor patroon](http://msdn.microsoft.com/library/dn589780.aspx).
- **Herstel voor taakstappen die mislukken beheren**. Een toepassing moet mogelijk ongedaan maken van de hoeveelheid werk die door een reeks stappen (die samen een uiteindelijk consistente werking) wordt uitgevoerd als er een of meer van de stappen niet. Zie voor meer informatie, [Compenserende transactie patroon](http://msdn.microsoft.com/library/dn589804.aspx).

## <a name="lifecycle-cloud-services"></a>Levenscyclus (Cloud-Services)

 Als u voor de uitvoering van achtergrondtaken voor Cloud Services-toepassingen die gebruikmaken van web- en werknemer rollen met behulp van de klasse **RoleEntryPoint** , is het belangrijk om te begrijpen van de levenscyclus van deze klasse om deze te gebruiken correct.

Web- en werknemer rollen gaan door een reeks afzonderlijke fasen terwijl ze start, uitvoeren en beëindigen. De klasse **RoleEntryPoint** wordt een reeks gebeurtenissen die aangeven wanneer deze fasen optreden. U gebruikt deze initialiseren, uitvoeren, en aangepaste achtergrondtaken stoppen. De volledige cyclus is:

- Azure wordt de functie-assembly geladen en zoekt een klasse die is afgeleid van **RoleEntryPoint**.
- Als deze klasse wordt gevonden, wordt **RoleEntryPoint.OnStart()**aangeroepen. U kunt deze methode voor het initialiseren van de achtergrondtaken negeren.
- Na de methode **OnStart** , presenteren Azure aanroepen van **Application_Start()** in het globale bestand van de toepassing als deze is (bijvoorbeeld Global.asax in de rol van een webpagina waarop ASP.NET wordt uitgevoerd).
- Azure roept **RoleEntryPoint.Run()** op een nieuwe voorgrond-thread die wordt uitgevoerd in combinatie met **OnStart()**. U kunt deze methode voor het starten van uw achtergrondtaken negeren.
- Als de methode Run wordt beëindigd, roept Azure **Application_End()** in het globale bestand als dit aanwezig is en roept vervolgens de **RoleEntryPoint.OnStop()**. U overschrijft de methode **OnStop** om uw achtergrondtaken stoppen, resources opruimen objecten verwijderen en sluiten van verbindingen die de taken gebruikt.
- De Azure rol host werkproces is gestopt. Op dit moment de rol van gerecycleerd en opnieuw wordt gestart.

Zie voor meer informatie en een voorbeeld van het gebruik van de methoden van de klasse **RoleEntryPoint** , [Compute Resource consolidatie patroon](http://msdn.microsoft.com/library/dn589778.aspx).

## <a name="considerations"></a>Overwegingen met betrekking tot

Wanneer u van plan bent hoe u een web of werknemer rol achtergrondtaken wordt uitgevoerd, moet u rekening houden met de volgende punten:

- De standaard **uitgevoerd** methode-implementatie in de klasse **RoleEntryPoint** bevat een aanroep naar **Thread.Sleep(Timeout.Infinite)** die de functie voor onbepaalde tijd actief blijft. Als u de methode **Run** (dit is meestal nodig achtergrondtaken uitvoeren) overschrijft, moet u uw code om af te sluiten tenzij u wilt recyclen van het exemplaar van de rol van de methode niet toegestaan.
- Een typische implementatie van de methode **Run** bevat start de achtergrondtaken en de constructie van een lus die periodiek de status van alle achtergrondtaken controleert-code. Zij die mislukken of controleren voor annulering tokens die aangeven dat de taken zijn voltooid opnieuw opstarten.
- Als een achtergrondtaak een onverwerkte uitzondering genereert, moet die taak terwijl andere achtergrondtaken in de rol worden uitgevoerd te worden gerecycled. Echter, als de uitzondering wordt veroorzaakt door beschadigde objecten buiten de taak, zoals gedeelde opslag de uitzondering moet worden afgehandeld door de klasse **RoleEntryPoint** , alle taken moeten worden geannuleerd en de methode **Run** mag eindigen. Azure start vervolgens opnieuw de rol.
- Met de methode **OnStop** kunt onderbreken of kill achtergrondtaken en resources opruimen. U dient hiervoor mogelijk langdurige of een dergelijk complex taken stoppen. Het is van cruciaal belang te overwegen hoe dit kan worden gedaan om te voorkomen dat de inconsistente gegevens. Als het exemplaar van een rol om welke reden dan een gebruiker aangestuurde afsluiting wordt gestopt, moet de code in de methode **OnStop** uitgevoerd worden voltooid binnen vijf minuten voordat deze wordt beëindigd. Zorg ervoor dat uw code in die tijd kan worden voltooid of kan tolereren niet volledig wordt uitgevoerd.  
- De taakverdeling Azure begint om verkeer naar de instantie rol wanneer de methode **RoleEntryPoint.OnStart** de waarde **true retourneert**. Overweeg daarom uw initialisatiecode plaatsen in de methode **OnStart** zodat rol exemplaren die met succes niet initialiseren niet al het verkeer.
- U kunt taken naast de methoden van de klasse **RoleEntryPoint** is gestart. Initialiseren van alle instellingen die u wijzigen in de Azure taakverdeling wilt, omdat deze taken worden uitgevoerd voordat de rol verzoeken ontvangt, moet u opstarttaken gebruiken. Zie [opstarttaken in Azure uitvoeren](./cloud-services/cloud-services-startup-tasks.md)voor meer informatie.
- Als er een fout in een taak is gestart, kan deze de rol voortdurend opnieuw opstarten forceren. Dit kan voorkomen dat u uitvoert virtuele IP-(VIP) adres wisselbestand terug naar een eerder voorbereide versie omdat de swap exclusieve toegang tot de rol vereist. Dit kan niet worden verkregen tijdens het opstarten van de rol. Om dit probleem oplossen:
    -  Voeg de volgende code aan het begin van de methoden **OnStart** en **uitvoeren** in uw rol:

    ```C#
    var freeze = CloudConfigurationManager.GetSetting("Freeze");
    if (freeze != null)
    {
        if (Boolean.Parse(freeze))
        {
            Thread.Sleep(System.Threading.Timeout.Infinite);
        }
    }
    ```

   - De definitie van de **titels** die u als een Boole-waarde toevoegen aan de ServiceDefinition.csdef en ServiceConfiguration. *.cscfg-bestanden voor de rol en wordt ingesteld op* *False* *. Als de functie in de modus van een herhaalde opnieuw op te starten gaat, kunt u de instelling van * *true** rol uitvoering blokkeren en toestaan dat deze worden verwisseld met een eerdere versie.

## <a name="resiliency-considerations"></a>Overwegingen met betrekking tot tolerantie

Achtergrondtaken moet veerkrachtig om betrouwbare diensten verlenen aan de toepassing. Bij het plannen en ontwerpen van achtergrondtaken, moet u rekening houden met de volgende punten:

- Achtergrondtaken moet kunnen worden verwerkt rol of service wordt opnieuw gestart zonder gegevens te beschadigen of invoering van inconsistentie in de toepassing. Voor langdurige of een dergelijk complex taken, kunt u overwegen _wijzen controleren_ door het opslaan van de status van taken in de permanente opslag of als de berichten in een wachtrij als dit van toepassing is. U kunt bijvoorbeeld informatie in een bericht in een wachtrij staat behouden en deze informatie staat incrementeel bijwerken met de voortgang van de taak, zodat de taak kan worden verwerkt in de laatste bekende goede checkpoint--in plaats van opnieuw op te starten vanaf het begin. Wanneer u de wachtrijen Azure Service Bus, kunt u berichten uitwisselen om hetzelfde scenario. Sessies kunnen u opslaan en ophalen van de status van de toepassing verwerken met behulp van de methoden [SetState](http://msdn.microsoft.com/library/microsoft.servicebus.messaging.messagesession.setstate.aspx) en [GetState](http://msdn.microsoft.com/library/microsoft.servicebus.messaging.messagesession.getstate.aspx) . Zie voor meer informatie over het ontwerpen van workflows en betrouwbare processen voor dergelijk complex [Scheduler Agent Supervisor patroon](http://msdn.microsoft.com/library/dn589780.aspx).
- Wanneer u web of de werknemer gebruikt als host voor meerdere achtergrondtaken, ontwerp het negeren van de methode **Run** voor mislukte of vastgelopen taken controleren en start deze opnieuw op. Indien dit praktisch niet haalbaar en u de functie van een werknemer gebruikt, ervoor zorgen dat de rol van de werknemer op te starten vanaf de methode **Run** af te sluiten.
- Wanneer u wachtrijen gebruikt om te communiceren met de achtergrondtaken, de wachtrijen kunnen fungeren als een buffer voor het opslaan van aanvragen die worden verzonden aan de taken die bij de toepassing is hoger dan de normale belasting. Hiermee kunt de taken op te vangen met de gebruikersinterface tijdens de minder drukke perioden. Het betekent ook dat de rol van recycling niet de gebruikersinterface blokkeert. Zie voor meer informatie [op basis van wachtrijen laden bij het herverdelen patroon](http://msdn.microsoft.com/library/dn589783.aspx). Als sommige taken belangrijker dan anderen zijn, kunt u de uitvoering van de [Prioritaire wachtrij patroon](http://msdn.microsoft.com/library/dn589794.aspx) om ervoor te zorgen dat deze taken worden uitgevoerd voordat die minder belangrijk.
- Achtergrondtaken die zijn geïnitieerd door berichten of berichten verwerken moeten zijn ontworpen voor het verwerken van inconsistenties, zoals berichten die binnenkomen volgorde berichten die herhaaldelijk een fout veroorzaken (vaak _gevaarlijke berichten_genoemd) en berichten die meer dan één keer worden bezorgd. Houd rekening met het volgende:
  - Berichten die moeten worden verwerkt in een bepaalde volgorde, zoals die die veranderen op basis van de bestaande waarde (bijvoorbeeld een waarde toe te voegen aan een bestaande waarde), kan niet in de oorspronkelijke volgorde waarin ze zijn verzonden arriveren. Ze kunnen ook worden verwerkt door verschillende exemplaren van een achtergrondtaak die in een andere volgorde als gevolg van verschillende belastingen op elk exemplaar. Berichten die moeten worden verwerkt in een specifieke volgorde moeten bevatten een volgnummer, sleutel of een andere indicator die achtergrondtaken gebruiken kunnen om ervoor te zorgen dat in de juiste volgorde worden verwerkt. Als u van Azure Service Bus gebruikmaakt, kunt u berichten uitwisselen te waarborgen, de volgorde van afleveren. Het is echter meestal efficiënter, waar mogelijk, om het proces zo ontwerpen dat de volgorde van de berichten niet belangrijk is.
  - Meestal wordt een achtergrondtaak bekijken van berichten in de wachtrij die tijdelijk te van andere consumenten bericht verbergen. Vervolgens worden de berichten nadat ze met succes zijn verwerkt. Als een achtergrondtaak mislukt tijdens het verwerken van een bericht, verschijnt dat bericht in de wachtrij opnieuw nadat de peek time-out is verlopen. Wordt verwerkt door een ander exemplaar van de taak of tijdens de volgende cyclus van de verwerking van dit exemplaar. Als het bericht wordt steeds een fout in de consument veroorzaakt, blokkeert het de taak, de wachtrij en uiteindelijk de toepassing zelf wanneer de wachtrij vol raakt. Daarom is het zeer belangrijk voor het detecteren en verwijderen van gevaarlijke berichten uit de wachtrij. Berichten als u van Azure Service Bus gebruikmaakt, die een fout veroorzaken kunnen worden verplaatst automatisch of handmatig naar een wachtrij voor onbestelbare gekoppeld.
  - Wachtrijen bij de _minste_ levering mechanismen zijn gegarandeerd, maar ze hetzelfde bericht meerdere keren kunnen leveren. Bovendien, als een achtergrondtaak mislukt na het verwerken van een bericht, maar voordat u het verwijdert uit de wachtrij, komen het bericht beschikbaar voor het opnieuw verwerken. Achtergrondtaken moet idempotency is ingeschakeld, wat betekent dat het verwerken van hetzelfde bericht meerdere keren niet leidt een fout of een inconsistentie in de gegevens van de toepassing tot. Bepaalde bewerkingen zijn natuurlijk idempotency is ingeschakeld, zoals een opgeslagen waarde instellen op een specifieke waarde voor de nieuwe. Echter, bewerkingen, zoals een waarde toe te voegen aan een bestaande opgeslagen waarde zonder te controleren dat de opgeslagen waarde is nog steeds hetzelfde als bij het oorspronkelijk bericht is inconsistent wordt. Azure Service Bus wachtrijen kunnen worden geconfigureerd om de gedupliceerde berichten automatisch te verwijderen.
  - Sommige systemen voor berichtenafhandeling, zoals opslag Azure wachtrijen en wachtrijen met Azure Service Bus, ondersteunen een de-queue aantal eigenschap die het aantal keren dat een bericht is gelezen uit de wachtrij aangeeft. Dit is handig in de verwerking van herhaalde en gevaarlijke berichten. Zie [Asynchronous Messaging Primer](http://msdn.microsoft.com/library/dn589781.aspx) en [Idempotency patronen](http://blog.jonathanoliver.com/2010/04/idempotency-patterns/)voor meer informatie.

## <a name="scaling-and-performance-considerations"></a>Overwegingen bij het schalen en prestaties

Achtergrondtaken moeten bieden voldoende prestaties om ervoor te zorgen zij niet blokkeren van de toepassing, of er inconsistenties optreden als gevolg van vertraagde werking wanneer het systeem belast is. Normaal gesproken is verbeterd door de compute-exemplaren die host zijn van de achtergrondtaken schalen. Houd rekening met de volgende punten rond de schaalbaarheid en de prestaties bij het plannen en ontwerpen van achtergrondtaken:

- Azure ondersteunt autoscaling (horizontaal schalen en schalen in) op basis van de huidige vraag- en laden-- of staan op een vooraf bepaald schema voor Web-Apps web Services Cloud en werknemer rollen en virtuele Machines implementaties. Gebruik deze functie om ervoor te zorgen dat de toepassing als geheel voldoende prestatiemogelijkheden tijdens runtime kosten tot een minimum beperken.
- Wanneer de achtergrondtaken hebben een andere prestaties van de andere delen van de toepassing van een Cloud-Services (bijvoorbeeld de UI of onderdelen, zoals de gegevenstoegangslaag), kan die als host fungeert voor de achtergrondtaken samen met de rol van een afzonderlijke werknemer de UI en de achtergrond rollen taak onafhankelijk schaal voor het beheer van de belasting. Als meerdere achtergrondtaken aanzienlijk verschillende prestatiemogelijkheden om met elkaar hebt, kunt u ze verdelen in afzonderlijke werknemer rollen en elk type functie onafhankelijk schalen. Bedenk wel dat dit de runtime kosten in vergelijking met het combineren van alle taken in minder rollen verhogen.
- De rollen gewoon schalen mogelijk niet voldoende om verlies van prestaties onder belasting te voorkomen. U moet mogelijk ook schalen opslag wachtrijen en andere bronnen om te voorkomen dat een centraal punt van de totale keten van een knelpunt wordt verwerkt. Bedenk ook andere beperkingen, zoals de maximale doorvoer van opslag en andere services die de toepassing en de achtergrondtaken.
- Achtergrondtaken moeten worden ontworpen voor schalen. Ze moeten bijvoorbeeld dynamisch detecteert het aantal wachtrijen opslag gebruikt om te luisteren of berichten verzenden naar de juiste wachtrij.
- Standaard WebJobs schaal met hun bijbehorende Azure Web Apps-exemplaar. Echter, als u een WebJob uit te voeren als slechts één exemplaar, kunt u een Settings.job-bestand met de JSON-gegevens **{'is_singleton': true}**. Wordt gedwongen Azure uitvoeren slechts één exemplaar van de WebJob, zelfs als er meerdere exemplaren van de bijbehorende web app. Dit is een handige techniek voor geplande taken die u als slechts één exemplaar uitvoeren moet.

## <a name="related-patterns"></a>Gerelateerde patronen

- [Asynchrone berichtenafhandeling Primer](http://msdn.microsoft.com/library/dn589781.aspx)
- [AutoScaling richtlijnen](http://msdn.microsoft.com/library/dn589774.aspx)
- [Compenserende transactie patroon](http://msdn.microsoft.com/library/dn589804.aspx)
- [Concurrerende consumenten patroon](http://msdn.microsoft.com/library/dn568101.aspx)
- [Richtlijnen voor partitionering berekenen](http://msdn.microsoft.com/library/dn589773.aspx)
- [Resource consolidatie patroon berekenen](http://msdn.microsoft.com/library/dn589778.aspx)
- [Patroon van de gatekeeper](http://msdn.microsoft.com/library/dn589793.aspx)
- [Leader Election patroon](http://msdn.microsoft.com/library/dn568104.aspx)
- [Buizen en Filters patroon](http://msdn.microsoft.com/library/dn568100.aspx)
- [Prioriteit wachtrij patroon](http://msdn.microsoft.com/library/dn589794.aspx)
- [Op basis van wachtrijen laden herverdeling patroon](http://msdn.microsoft.com/library/dn589783.aspx)
- [Scheduler-Agent Supervisor patroon](http://msdn.microsoft.com/library/dn589780.aspx)

## <a name="more-information"></a>Meer informatie

- [Azure-toepassingen met de rol van werknemer schalen](http://msdn.microsoft.com/library/hh534484.aspx#sec8)
- [Achtergrondtaken uitvoeren](http://msdn.microsoft.com/library/ff803365.aspx)
- [Levenscyclus van Azure rol opstarten](http://blog.syntaxc4.net/post/2011/04/13/windows-azure-role-startup-life-cycle.aspx) (blog post)
- [Levenscyclus van Azure Cloud Services-rol](http://channel9.msdn.com/Series/Windows-Azure-Cloud-Services-Tutorials/Windows-Azure-Cloud-Services-Role-Lifecycle) (video)
- [Aan de slag met de Azure WebJobs SDK](./app-service-web/websites-dotnet-webjobs-sdk-get-started.md)
- [Azure wachtrijen en Bus servicewachtrijen - vergeleken en tegenstelling tot](./service-bus-messaging/service-bus-azure-and-service-bus-queues-compared-contrasted.md)
- [Diagnostische gegevens in een Cloud-Service inschakelen](./cloud-services/cloud-services-dotnet-diagnostics.md)
