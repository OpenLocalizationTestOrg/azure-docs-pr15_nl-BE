<properties
   pageTitle="Controlelijst voor tolerantie | Microsoft Azure"
   description="Deze controlelijst richtlijnen voor tolerantie problemen tijdens het ontwerpen bevat."
   services=""
   documentationCenter="na"
   authors="petertaylor9999"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="best-practice"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="petertay"/>

# <a name="azure-resiliency-guidance-resiliency-checklist"></a>Azure resiliency richtlijnen: tolerantie, controlelijst

Ontwerpen van uw toepassing voor tolerantie is vereist voor planning en beperkende verschillende modi fout die kan optreden. Controleer de artikelen in deze controlelijst tegen het ontwerp van uw toepassing om veerkrachtiger.

## <a name="requirements"></a>Vereisten

- **Definieer de vereiste beschikbaarheid van uw klant.** Uw klant heeft beschikbaarheid eisen voor de componenten in uw toepassing en dit heeft invloed op het ontwerp van uw toepassing. Overeenkomst van uw klant krijgen voor de doelen van de beschikbaarheid van elk stuk van uw toepassing, anders uw ontwerp niet voldoet aan de verwachtingen van de klant. Zie de sectie [uw herstellingsvermogen vereisten definiëren](guidance-resiliency-overview.md#defining-your-resiliency-requirements) van het [ontwerpen van robuuste toepassingen voor Azure](guidance-resiliency-overview.md) -document voor meer informatie.

## <a name="failure-mode-analysis"></a>Modus Foutanalyse

- **Een analyse op modus (FMA) voor de toepassing uitvoeren.** FMA is een proces voor het bouwen van tolerantie in een toepassing vroeg in het ontwerpstadium. De doelstellingen van een volledige Postbustoegang omvatten:  

    - Bepalen welke soorten storingen in een toepassing kan optreden. 
    - Vastleggen van de potentiële effecten en de gevolgen van elk type fout in de toepassing.
    - Strategieën voor gegevensherstel identificeren. 

    Zie voor meer informatie [ontwerpen van robuuste toepassingen voor Azure: analyse op modus][fma].  

## <a name="application"></a>Toepassing

- **Meerdere exemplaren van services implementeren.** Services niet onvermijdelijk, en als uw toepassing afhankelijk van een enkel exemplaar van een service is zal onvermijdelijk mislukken ook. Als u wilt meerdere exemplaren van [Azure App Service](../app-service/app-service-value-prop-what-is.md)inrichten, selecteren een [App Service plannen](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) met meerdere exemplaren. Configureer alle gebruik van [meerdere exemplaren](../cloud-services/cloud-services-choose-me.md#scaling-and-management)van de rollen voor Azure Cloud-Services. [Azure virtuele Machines (VMs)](../virtual-machines/virtual-machines-windows-about.md), ervoor zorgen dat uw VM-architectuur meer dan een VM omvat en dat elke VM is opgenomen in een [beschikbaar][availability-sets].   

- **Gebruik een load balancer voor het distribueren van aanvragen.** Taakverdeling van uw toepassing aanvragen naar exemplaren van gezonde service door het verwijderen van beschadigde exemplaren van rotatie. Als uw service Azure App Service of Azure Cloud Services gebruikt, is het al voor u taakverdeling. Echter, als uw toepassing gebruikmaakt van VMs Azure, moet u een load balancer inrichten. Zie de [Azure Load Balancer](../load-balancer/load-balancer-overview.md) -overzicht voor meer informatie. 

- **Toepassingsgateways Azure als u meerdere exemplaren wilt configureren.** Afhankelijk van de eisen van uw toepassing een [Toepassingsgateway Azure](../application-gateway/application-gateway-introduction.md) mogelijk beter geschikt voor het aanvragen van uw toepassing Services distribueren. Echter zijn enkele exemplaren van de service Application Gateway niet gegarandeerd door een SLA zodat het mogelijk is dat uw toepassing mislukken kan als de instantie toepassingsgateway mislukt. Meer dan een middelgrote of grotere Application Gateway-instantie te waarborgen van de beschikbaarheid van de service onder de voorwaarden van de [SERVICEOVEREENKOMST](https://azure.microsoft.com/support/legal/sla/application-gateway/v1_0/)inrichten.

- **Gebruik beschikbaar stelt voor elke toepassingslaag van de**. Het plaatsen van uw sessies in een [beschikbaar] [ availability-sets] garandeert connectiviteit met ten minste één exemplaar van de VM binnen de voorwaarden van de [SERVICEOVEREENKOMST](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_2/). Als uw VMs niet in een set beschikbaarheid is er geen garanties beschermd en is het mogelijk dat zij alle mislukken of tegelijkertijd worden bijgewerkt. 

- **Overweeg de toepassing distribueren over meerdere regio's.** Als uw toepassing is geïmplementeerd op één gebied, in het zeldzame geval dat het gehele gebied niet beschikbaar is, wordt uw toepassing ook niet beschikbaar. Dit zijn onaanvaardbaar zijn volgens de voorwaarden van de SERVICEOVEREENKOMST van uw toepassing. In dat geval kunt u uw toepassing en de bijbehorende services distribueren over meerdere regio's. Een implementatie met meerdere regio kunt gebruiken voor een actieve patroon (aanvragen distribueren over meerdere actieve exemplaren) of een actief-passief patroon (een 'warme' instantie houden in reserve, in het geval de primaire sessie mislukt). Wij raden aan dat u meerdere exemplaren van de services van uw toepassing via regionale paren implementeert. Zie voor meer informatie [Business continuïteit en disaster recovery (BCDR): Azure gekoppeld regio's](../best-practices-availability-paired-regions.md).

- **In voorkomend geval resiliency patronen voor externe bewerkingen worden geïmplementeerd.** Als uw toepassing afhankelijk van de communicatie tussen RAS-services is, mislukt de communicatie is onvermijdelijk. Als er meerdere fouten zijn, kunnen de resterende gezonde exemplaren van de services van uw toepassing worden overweldigd met aanvragen. Er zijn verschillende patronen voor de verwerking van algemene fouten, met inbegrip van de time-patroon, het [patroon opnieuw]nuttig[retry-pattern], de [stroomonderbreker] [ circuit-breaker] patroon en anderen. Zie voor meer informatie, [ontwerpen van robuuste toepassingen voor Azure](guidance-resiliency-overview.md#implementing-resiliency-strategies). 

- **Autoscaling gebruiken om te reageren op de stijgingen van de belasting.** Als uw toepassing niet is geconfigureerd voor het schalen van automatisch naarmate de belasting toeneemt, is het mogelijk dat de services van uw toepassing niet als ze worden verzadigd met aanvragen van gebruikers. Zie de volgende onderwerpen voor meer informatie:

    - Algemeen: [Controlelijst voor schaalbaarheid](../best-practices-scalability-checklist.md) 
    - Azure App Service: [schalen exemplaar aantal handmatig of automatisch][app-service-autoscale]
    - Cloud Services: [het automatisch schalen een cloud-service][cloud-service-autoscale]
    - Virtuele Machines: [Automatische schaling en virtuele machine schaal instellen][vmss-autoscale]


- **Asynchrone bewerkingen zoveel mogelijk worden geïmplementeerd.** Synchrone bewerkingen kunnen bronnen in beslag nemen en andere bewerkingen te blokkeren, terwijl de aanroeper wacht tot het proces voltooid. Elk onderdeel van uw toepassing voor asynchrone bewerkingen zoveel mogelijk ontwerp. Zie voor meer informatie over het implementeren van asynchrone programmering in C# [asynchrone programmeren met asynchrone en wachten op een][asynchronous-c-sharp].

- **Azure verkeer Manager gebruiken voor het routeren van netwerkverkeer van uw toepassing naar verschillende regio's.**  [Azure verkeer Manager] [ traffic-manager] voor taakverdeling op het niveau van DNS wordt uitgevoerd en verkeer doorsturen naar de verschillende regio's op basis van de [verkeersroutering] [ traffic-manager-routing] methode die u opgeeft en de gezondheid van de eindpunten van de toepassing. 

- **Configureren en testen van gezondheid sondes voor netwerktaakverdeling en managers van verkeer.** Zorg ervoor dat de logica van de gezondheid van de essentiële onderdelen van het systeem gecontroleerd en op de juiste manier op gezondheid sondes reageert.

    - De gezondheid van de sondes voor [Azure verkeer Manager] [ traffic-manager] en [Azure Load Balancer] [ load-balancer] een specifieke functie. Voor verkeer Manager de sonde gezondheid bepaalt of een failover uitvoeren naar een andere regio. Voor een taakverdeling bepaalt of een VM verwijderen van rotatie.      

    - Voor een sonde verkeer Manager controleert uw gezondheid eindpunt kritieke afhankelijkheden die zijn geïmplementeerd in dezelfde regio en wiens fout een failover naar een ander gebied moet worden geactiveerd.  

    - Voor een taakverdeling moet het eindpunt van de gezondheid van de gezondheid van de VM verslag. Neem geen andere lagen of externe services. Een fout die buiten de VM optreedt, anders zal de verdeling van de belasting voor het verwijderen van de VM van rotatie.

    - Voor instructies over het implementeren van statuscontrole in uw toepassing, Zie [Gezondheid eindpunt Monitoring patroon](https://msdn.microsoft.com/library/dn589789.aspx).

- **Services van andere leveranciers controleren.** Als uw toepassing afhankelijkheden voor services van andere leveranciers, bepalen waar en hoe deze diensten van derden kunnen mislukken en wat deze storingen effect zullen hebben op uw toepassing. Een service van derden mag niet inhouden bewaking en diagnostiek, dus het is belangrijk om te melden het aanroepen van deze en deze te correleren met de gezondheid van uw toepassing en diagnostische met behulp van een unieke id. Zie voor meer informatie over aanbevolen procedures voor controle en diagnose, de [richtsnoeren voor bewaking en diagnose] [ monitoring-and-diagnostics-guidance] document.

- **Zorg ervoor dat elke derde partij service die u verbruiken een SLA biedt.** Als uw toepassing afhankelijk van een service van derden is, maar de derde partij geen garantie voor de beschikbaarheid in de vorm van een SLA biedt, kan ook de beschikbaarheid van de toepassing kan niet worden gegarandeerd. De SLA is net zo goed als het minste beschikbaar onderdeel van uw toepassing.


## <a name="data-management"></a>Gegevensbeheer

- **Begrijp de replication, methoden voor gegevensbronnen van uw toepassing.** De toepassingsgegevens worden opgeslagen in verschillende gegevensbronnen en beschikbaarheid van verschillende vereisten. Evalueer de replication, methoden voor elk type gegevens worden opgeslagen in Azure, [Azure Storage replicatie](../storage/storage-redundancy.md) en [SQL Database actief Geo-replicatie](../sql-database/sql-database-geo-replication-overview.md) om ervoor te zorgen dat de vereiste gegevens van uw toepassing wordt voldaan.

- **Zorg ervoor dat er geen één gebruikersaccount toegang tot de productie en de back-up van gegevens.** Back-ups van uw gegevens in gevaar komen als één enkele gebruikersaccount heeft schrijfmachtiging voor de productie en de back-bronnen. Een kwaadwillende gebruiker kan uw gegevens opzettelijk verwijderen terwijl een gewone gebruiker kan per ongeluk verwijdert. Ontwerp uw toepassing de machtigingen van de account van elke gebruiker beperken zodat alleen de gebruikers die schrijftoegang moeten schrijftoegang hebben en het is alleen voor de productie of back-up, maar niet beide.

- **Document uw gegevensbron een failover en failback verwerken en testen.** In het geval wanneer de gegevensbron niet catastrophically moet een menselijke operator als volgt een set instructies naar een nieuwe gegevensbron overgeplaatst. De beschreven stappen hebt fouten, wordt een operator niet mogelijk om succes te volgen en de bron een failover. De instructie stappen om te controleren of na deze operator kunnen voor failover en failback van de gegevensbron regelmatig testen.

- **Valideren van de back-ups van gegevens.** Regelmatig controleren of uw back-up gegevens oplevert door het uitvoeren van een script voor het valideren van de integriteit van gegevens, het schema en query's. Er is geen enkel punt met een back-up als het geen zin om de gegevensbronnen. Melden en eventuele inconsistenties rapporteren, zodat u de back-service kan worden hersteld.

- **Overweeg het gebruik van een opslagtype die geo-redundante is.** Gegevens die zijn opgeslagen in een opslagruimte Azure-account wordt altijd lokaal gerepliceerd. Er zijn echter verschillende strategieën voor replicatie kunt kiezen uit een Account opslag is ingericht. Selecteer [Azure leestoegang Geo redundante opslag (RA-GRS)](../storage/storage-redundancy.md#read-access-geo-redundant-storage) uw om toepassingsgegevens te beschermen tegen het zeldzame geval wanneer een hele regio niet meer beschikbaar is.

    > [AZURE.NOTE] Voor VMs, Vertrouw niet op replicatie RA GRS herstellen de VM-schijven (VHD-bestanden). Gebruik in dat geval [Azure back-up][azure-backup].   

## <a name="operations"></a>Bewerkingen

- **Controle en signalering van beste praktijken in uw toepassing implementeert.** Zonder goede controle, diagnose en waarschuwingen is er op geen enkele manier fouten opsporen in uw toepassing en een waarschuwing een operator om problemen op te lossen. Zie voor meer informatie over beste praktijken, de [richtsnoeren voor bewaking en diagnose] [ monitoring-and-diagnostics-guidance] document.

- **Statistieken voor externe aanroep te meten en de informatie beschikbaar maken voor het team van toepassing.**  Als u niet bijhouden en externe aanroep statistieken in real-time rapporteren en bieden een gemakkelijke manier om deze gegevens te bekijken, het operationele team geen een momentopname bekijken in de gezondheid van uw toepassing. Problemen in de services en als u alleen maatregel externe aanroep gemiddelde tijd niet u voldoende informatie hebt te geven. Maatstaven voor externe aanroep zoals latentie en doorvoer, fouten in de percentielen 99 en 95 samenvatten. Statistische analyses uitvoeren op de maatstaven om fouten die zich binnen elke percentiel voordoen.

- **Het aantal tijdelijke uitzonderingen en pogingen gedurende een passende termijn bijhouden.** Als u niet bijhouden en controleren van tijdelijke uitzonderingen en na verloop van tijd herhalingspogingen, is het mogelijk dat een probleem of storing door uw toepassingslogica opnieuw kan worden verborgen. Dat wil zeggen, als de prestatiecontrole en -registratie alleen slagen of mislukken van een bewerking, worden het feit dat de bewerking moest opnieuw meerdere keren worden uitgevoerd omdat er uitzonderingen verborgen. Een trend van toenemende uitzonderingen na verloop van tijd geeft aan dat de service is een probleem dat kan mislukken. Zie voor meer informatie, [richtlijnen voor specifieke services opnieuw][retry-service-guidance].

- **Implementatie van een systeem voor vroegtijdige waarschuwing dat een exploitant een waarschuwing.** Identificeer de belangrijkste prestatie indicatoren van de gezondheid van uw toepassing, zoals tijdelijke uitzonderingen en externe aanroepen latentie en passende drempelwaarden instellen voor elk van hen. Als de drempelwaarde is bereikt, kunt u een waarschuwing verzenden aan bewerkingen. Deze drempels op niveaus die problemen identificeren voordat ze kritiek en een herstel reactie moeten worden ingesteld.

- **Het release-proces voor uw toepassing het document.** Een operator kan zonder gedetailleerde release procesdocumentatie, een slechte update implementeren of onjuiste instellingen configureren voor uw toepassing. Duidelijk definiëren en documenteren van het release-proces en ervoor te zorgen dat deze beschikbaar voor het hele operations team is. Aanbevolen procedures voor de robuuste distributie van uw toepassing worden gegeven in de [implementatie van robuuste] [ guidance-resilient-deployment] sectie van de leidraad voor tolerantie.

- **Zorg ervoor dat meer dan één persoon in het team is getraind om te houden op de toepassing en eventuele handmatige stappen uit te voeren.** Als u slechts één operator op het team dat u kunt houden op de toepassing en ere van de procedure, wordt deze persoon een potentieel risico. Meerdere personen van detectie en herstel van de trein en zorg ervoor dat er altijd ten minste één actieve op elk gewenst moment.

- **Automatiseren van het implementatieproces van uw toepassing.** Het operationele personeel is vereist voor de implementatie van uw toepassing handmatig, menselijke fouten kan leiden tot de implementatie mislukt. Zie voor meer informatie over aanbevolen procedures voor het automatiseren van de implementatie van toepassingen, de [robuuste implementatie] [ guidance-resilient-deployment] sectie van de leidraad voor tolerantie.

- **Het release-proces voor maximale beschikbaarheid van toepassingen ontwerpen.** Als het proces release zijn services om offline te gaan tijdens de implementatie vereist, uw toepassing niet beschikbaar totdat deze weer on line. Gebruik de techniek van de implementatie [blauw/groen](http://martinfowler.com/bliki/BlueGreenDeployment.html) of [Canarische vrijgeven](http://martinfowler.com/bliki/CanaryRelease.html) voor de implementatie van de toepassing op de productie. Beide van deze technieken omvatten uw code release naast productiecode implementeren, zodat gebruikers van versie kunnen worden omgeleid naar de productiecode bij een storing. Zie voor meer informatie, de [robuuste implementatie] [ guidance-resilient-deployment] sectie van de leidraad voor tolerantie.

- **Melden en controleren van uw toepassing implementaties.** Als u gefaseerde implementatie technieken zoals blauw/groen of kokospalm releases zal er meer dan één versie van de toepassing die wordt uitgevoerd in de productie. Als er een probleem optreed, is het van essentieel belang om te bepalen welke versie van uw toepassing een probleem veroorzaakt. Een robuuste logboekregistratie strategie voor het vastleggen van zoveel mogelijk informatie versie-specifieke implementeren. 

- **Zorg ervoor dat de toepassing niet wordt uitgevoerd zich verhouden tot de [Azure abonnement beperkt](../azure-subscription-service-limits.md).** Azure abonnementen hebben beperkingen voor bepaalde brontypen, zoals het nummer van de resource, aantal kernen en aantal accounts voor opslag.  Als uw toepassingsvereisten Azure abonnement limieten overschrijden, maken een andere Azure abonnement en er voldoende bronnen er.

- **Zorg ervoor dat de toepassing niet wordt uitgevoerd zich verhouden tot de [limieten per service](../azure-subscription-service-limits.md).** Afzonderlijke Azure services hebben grenzen verbruik &mdash; bijvoorbeeld beperkt op opslag, doorvoer, aantal verbindingen aanvragen per seconde en andere criteria. De toepassing werkt niet als bronnen buiten deze grenzen wordt gebruikt. Hierdoor wordt de bandbreedteregeling en mogelijke onderhoudstijd voor de betreffende gebruikers. 

    Afhankelijk van de specifieke dienst en de toepassingsvereisten van uw, kunt u deze limieten vaak voorkomen (bijvoorbeeld het kiezen van een andere prijzen laag) verticaal schalen of horizontaal schalen (toevoegen van nieuwe exemplaren).  

- **Ontwerp de opslagvereisten van uw toepassing te vallen onder Azure opslag schaalbaarheid en prestaties doelen.** Azure-opslag is ontworpen om te werken binnen een vooraf gedefinieerde doelen voor schaalbaarheid en prestaties, zodat uw toepassingen gebruikmaken van de opslag binnen deze doelen ontwerpen. Als u deze doelen overschrijdt treden uw toepassing opslag beperken. U kunt dit verhelpen inrichten extra opslag Accounts. Als u zich verhouden tot de opslag limiet, extra Azure abonnementen inrichten en richt er extra opslag-Accounts. Zie voor meer informatie [Azure opslag schaalbaarheid en prestaties, doelen](../storage/storage-scalability-targets.md).

- **Selecteer de juiste grootte VM voor uw toepassing.** De werkelijke CPU, geheugen, schijf en i/o-van het VMs in productie meet en controleer of de VM-grootte die u hebt geselecteerd, is voldoende. Zo niet, kan de toepassing problemen capaciteit als de VMs benadering van hun grenzen. VM-formaten worden gedetailleerd beschreven in het document [formaten voor virtuele machines in Azure](../virtual-machines/virtual-machines-windows-sizes.md) .

- **Nagaan of de werkbelasting van uw toepassing stabiel of wisselende na verloop van tijd.** Als uw werkbelasting na verloop van tijd fluctueert, gebruik Azure VM schaal wordt ingesteld op automatisch schalen het aantal exemplaren van de VM. Anders wordt er handmatig verhogen of verlagen van het aantal VMs. Zie de [Virtuele Machine schaal Sets-overzicht](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md)voor meer informatie.

- **Selecteer de laag van de juiste service voor Azure SQL-Database.** Als uw toepassing gebruikmaakt van Azure SQL-Database, moet u ervoor zorgen dat u de juiste servicelaag hebt geselecteerd. Als u een laag die niet kunnen verwerken van de vereisten van uw toepassing database transactie-eenheid (DTU), het gebruik van uw gegevens wordt de snelheid van. Zie voor meer informatie over het selecteren van de juiste serviceplan de [Opties voor SQL-Database en prestaties: begrijpen wat beschikbaar is in elke servicelaag](../sql-database/sql-database-service-tiers.md) document. 

- **Hebben een rollback-plan voor de implementatie.** Het is mogelijk dat uw implementatie van toepassingen kan mislukken en zorgen dat uw toepassing niet meer beschikbaar. Ontwerp een herstelprocedure wilt teruggaan naar de laatst bekende juiste versie en de uitvaltijd tot een minimum. Zie de [implementatie van robuuste] [ guidance-resilient-deployment] sectie van de tolerantie Guidance document voor meer informatie. 

- **Een proces voor het communiceren met ondersteuning van Azure maken.** Als het proces voor het contact met [ondersteuning van Azure](https://azure.microsoft.com/support/plans/) niet is ingesteld voordat u contact opnemen met ondersteuning nodig is, wordt uitvaltijd verlengd als het ondersteuningsproces voor de eerste keer is gegaan. Het proces voor ondersteuning contact opnemen met en problemen escaleren als onderdeel van de tolerantie vanaf het begin van de toepassing opnemen.

- **Zorg ervoor dat uw toepassing maakt geen gebruik van meer dan het maximale aantal accounts opslag per abonnement.** Azure kan maximaal 200 opslag rekeningen per abonnement. Als uw toepassing accounts meer opslagruimte dan op dit moment beschikbaar zijn in uw abonnement, moet u een nieuw abonnement maken en er extra opslagruimte accounts maken. Zie voor meer informatie [Azure abonnement service grenzen, quota's en beperkingen](../azure-subscription-service-limits.md#storage-limits).

- **Zorg ervoor dat uw toepassing de schaalbaarheid doelen voor virtuele machine schijven niet overschrijden.** Een IaaS Azure VM ondersteunt een aantal gegevensschijven afhankelijk van verschillende factoren, waaronder de VM-grootte en het type van opslag account koppelen. Als uw toepassing groter is dan de schaalbaarheid doelen voor virtuele machine schijven, accounts extra opslagruimte creëren en er de virtuele machine-diskettes maken. Zie voor meer informatie [Azure opslag schaalbaarheid en prestaties, doelen] (.. / storage/storage-scalability-targets.md#scalability-targets-for-virtual-machine-disks)

## <a name="test"></a>Test

- **Uitvoeren van failover en failback voor uw toepassing testen.** Als u nog niet volledig failover en failback getest, u kan niet worden bepaald dat de afhankelijke services in uw toepassing terug in een gesynchroniseerde wijze tijdens noodherstel komen. Zorg ervoor dat afhankelijk van uw toepassing services mislukt terug in de juiste volgorde en overname bij storingen.

- **Fouttolerantie-injectie voor uw toepassing testen uitvoeren.** Uw toepassing kan mislukken voor verschillende oorzaken hebben, zoals het certificaat verloopt, uitputting van bronnen in een VM of opslag fouten. Testen van de toepassing in een omgeving die zo dicht mogelijk tot productie, simuleren of die echte fouten. Bijvoorbeeld certificaten verwijderen, kunstmatig systeembronnen in beslag nemen of een opslagbron verwijderen. Controleer of de mogelijkheid om te herstellen van alle soorten fouten, alleen en in combinatie van uw toepassing. Controleer of de fouten niet doorgeven of trapsgewijze via uw systeem.

- **Tests uitvoeren in de productie met behulp van de gebruikersgegevens van beide synthetische en echte.** Test- en zelden identiek zijn, dus het is belangrijk u blauw/groen of een kokospalm implementeren en testen van de toepassing in de productie. Hiermee kunt u uw toepassing testen in productie bij belasting van de reële en ervoor te zorgen dat het werkt zoals verwacht wanneer het volledig is geïmplementeerd.

## <a name="security"></a>Beveiliging

- **Bescherming tegen distributed denial of service (DDoS)-aanvallen op toepassingsniveau worden geïmplementeerd.** Azure services worden beschermd tegen DDos-aanvallen op de netwerklaag. Echter beveiligen niet Azure tegen aanvallen van de toepassingslaag, omdat het moeilijk is om te onderscheiden waar gebruikersaanvragen vanuit de kwaadwillende gebruikersaanvragen. Zie de sectie "Bescherming tegen DDoS" [Microsoft Azure Network Security](http://download.microsoft.com/download/C/A/3/CA3FC5C0-ECE0-4F87-BF4B-D74064A00846/AzureNetworkSecurity_v3_Feb2015.pdf) (download PDF) voor meer informatie over het niveau van de toepassingslaag DDoS-aanvallen.

- **Het principe van minimale bevoegdheden voor toegang tot de systeembronnen van de toepassing wordt geïmplementeerd.** De standaardinstelling voor toegang tot de systeembronnen van de toepassing moet zo strikt mogelijk zijn. Machtigingen op hoger niveau op basis van de goedkeuring verlenen. Te soepele toegang verlenen tot de bronnen van uw toepassing standaard kan leiden tot iemand per ongeluk of opzettelijk bronnen verwijderen. Azure biedt [op rollen gebaseerde toegangscontrole](../active-directory/role-based-access-built-in-roles.md) voor gebruikersrechten beheren, maar het is belangrijk om te controleren of ten minste bevoegdheden machtigingen voor andere bronnen die hun eigen machtigingen systemen, zoals SQL Server hebben. 

## <a name="telemetry"></a>Telemetrie

- **Telemetriegegevens vastleggen terwijl de toepassing wordt uitgevoerd in de productieomgeving.** Robuuste telemetrie gegevens vastleggen terwijl de toepassing in de productieomgeving wordt uitgevoerd of u niet voldoende gegevens hebt voor het vaststellen van de oorzaak van problemen terwijl er gebruikers actief fungeert. Meer informatie is beschikbaar in de registratie aanbevolen procedures is beschikbaar in de [richtsnoeren voor bewaking en diagnose] [ monitoring-and-diagnostics-guidance] document.

- **Aanmelden met behulp van een asynchrone patroon implementeren.** Als logboekregistratie bewerkingen synchrone, kunnen zij de toepassingscode blokkeren. Zorg ervoor dat uw registratie bewerkingen worden uitgevoerd als asynchrone bewerkingen. 

- **Correleren logboekgegevens naar een service.** In een typische n-tier-toepassing kan een aanvraag verschillende servicegrenzen doorkruisen. Bijvoorbeeld is een aanvraag van de gebruiker gewoonlijk in de weblaag afkomstig is en doorgegeven aan de business-laag en ten slotte in de gegevenslaag. In meer complexe scenario's, kan een aanvraag worden gedistribueerd naar veel verschillende services en gegevens winkels. Controleer uw systeem aanmelden verbindt gesprekken buiten de servicegrenzen van de, zodat u de aanvraag in uw toepassing kunt bijhouden.

##  <a name="azure-resources"></a>Azure Resources 

- **Sjablonen aan resources voorziening Azure Resource Manager gebruiken.** Sjablonen voor bronbeheer gemakkelijker implementaties via PowerShell of de CLI Azure, die tot een meer betrouwbare implementatieproces leidt automatiseren. Zie [overzicht van Azure Resource Manager]voor meer informatie[resource-manager].

- **Resources betekenisvolle namen geven.** Resources betekenisvolle namen geven makkelijker een specifieke bron zoekt en de rol ervan begrijpen. Zie [aanbevolen naamgevingsconventies voor Azure bronnen](guidance-naming-conventions.md) voor meer informatie. 

- **Toegangsbeheer gebruiken op basis van rollen (RBAC)**. Gebruik RBAC toegang tot de Azure bronnen die u implementeert. RBAC kunt u vergunning rollen toewijzen aan leden van uw team DevOps om te voorkomen dat per ongeluk wordt verwijderd of wijzigingen gedistribueerde bronnen. Zie voor meer informatie, [aan de slag met access management in Azure portal](../active-directory/role-based-access-control-what-is.md) 

- **Resource vergrendelingen voor kritieke bronnen, zoals VMs gebruiken.** Resource vergrendelingen dat per ongeluk verwijderen van een resource een operator. Zie [bronnen vergrendelen met Azure Resource Manager](../resource-group-lock-resources.md) voor meer informatie. 

- **Regionale paren.** Bij de implementatie van de twee regio's, kiest u van dezelfde regionale twee regio's. In geval van een grote stroomstoring is herstel van een regio van elk paar prioriteit krijgen. Sommige services zoals Geo-redundante opslag bieden automatische replicatie naar het gekoppelde gebied. Zie voor meer informatie [Business continuïteit en disaster recovery (BCDR): Azure gekoppeld regio's](../best-practices-availability-paired-regions.md) 

- **Resourcegroepen organiseren door de functie en de levenscyclus**.  Een resourcegroep dient in het algemeen bevatten bronnen die delen van de levenscyclus van de. Dit gemakkelijker op implementaties beheren test implementaties te verwijderen en het toewijzen van toegangsrechten, waarmee de kans dat een productie-implementatie per ongeluk is verwijderd of gewijzigd. Maak aparte bronnengroepen voor ontwikkeling, productie en omgevingen te testen. In een implementatie met meerdere regio bronnen voor elke regio in aparte bronnengroepen te plaatsen. Dit vereenvoudigt met één gebied implementeren zonder dat de andere regio('s). 

## <a name="azure-services"></a>Azure Services 

De volgende controlelijstitems van toepassing op specifieke services in Azure.

###  <a name="app-service"></a>App-Service 

- **Gebruik de Standard of Premium laag.** Deze lagen staging-sleuven ondersteunen en geautomatiseerde back-ups. Zie voor meer informatie, [gedetailleerde overzicht van Azure App Service plannen](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) 

- **Vermijd omhoog of omlaag te schalen.** In plaats daarvan, selecteer een laag en grootte instantie die voldoen aan de prestatie-eisen onder normale belasting en [de schaal van](../app-service-web/web-sites-scale.md) de exemplaren voor het verwerken van wijzigingen in het netwerkverkeer. Schaal kan omhoog en omlaag starten en een toepassing opnieuw starten.  

- **Configuratie opslaan als app instellingen.** App instellingen gebruiken voor het opslaan van configuratieinstellingen als app. De instellingen definiëren in uw sjablonen Resource Manager of met PowerShell, zodat u kunt als onderdeel van een geautomatiseerde implementatie toepassen / update proces, dat meer betrouwbaar is. Zie [webtoepassingen configureren in Azure App-Service](../app-service-web/web-sites-configure.md)voor meer informatie. 

- **Afzonderlijke App serviceplannen voor productie- en maken.** Gebruik geen sleuven op de productie-implementatie voor het testen.  Alle apps binnen dezelfde App serviceplan delen dezelfde VM exemplaren. Als u productie en implementaties testen in hetzelfde plan, dit een negatieve invloed op de productie-implementatie. Load-tests kunnen bijvoorbeeld de productiesite live afnemen. De toevoeging van test-implementaties in een apart plan, isoleren u ze van de productieversie.  

- **Afzonderlijke web-apps van web-API's**. Als uw oplossing een web-front- en een web API is, kunt u ze in afzonderlijke App Service apps ontleedt. Dit ontwerp makkelijker om de oplossing door de werkbelasting. U kunt de web app en de API uitvoeren in afzonderlijke App serviceplannen dus onafhankelijk van elkaar worden geschaald. Als u geen niveau van schaalbaarheid op eerst, kunt u de apps implementeren in hetzelfde plan en ze in afzonderlijke plannen later verplaatst, indien nodig.

- **Vermijd het gebruik van de back-upfunctie App Service back-up Azure SQL-databases.** In plaats daarvan gebruiken [geautomatiseerde back-ups van SQL-Database][sql-backup]. Back-up App Service exporteert de database naar een SQL .bacpac-bestand DTUs kosten.  

- **Implementatie van een staging-sleuf.** Maak een sleuf implementatie voor gefaseerde installatie. Implementeren van updates voor toepassingen op de staging-sleuf en controleer of de implementatie vóór het wisselen in productie. Hierdoor verkleint u de kans van een slechte update van de productie. Ook zorgt ervoor dat alle exemplaren worden opgewarmd voordat de productie wordt geruild. Veel toepassingen hebben een belangrijke warmup en koude-startvertraging tijd. Voor meer informatie Zie [staging-omgevingen voor web apps in Azure App-Service](../app-service-web/web-sites-staged-publishing.md). 

-  **Maak een implementatie-sleuf voor de implementatie van laatste bekende juiste (LKG).** Wanneer u een update voor de productie implementeert, verplaatst u de vorige productie-implementatie in de sleuf LKG. Hierdoor gemakkelijker terug te draaien een slechte implementatie. Als u later een probleem ontdekt, kunt u snel terugkeren naar de LKG-versie. Zie voor meer informatie [Azure referentiearchitectuur: eenvoudige webtoepassing](guidance-web-apps-basic.md). 

- **Diagnostische logboekregistratie inschakelen**, met inbegrip van de toepassingslogboeken en logboekregistratie van webserver. Logboekregistratie is belangrijk voor de bewaking en diagnostiek. Zie [Diagnostische gegevens vastleggen voor web apps in Azure App-Service inschakelen](../app-service-web/web-sites-enable-diagnostic-log.md)

- **Logboek naar een blob-opslag**. Dit maakt het gemakkelijker te verzamelen en analyseren van de gegevens. 

- **Maak een aparte opslag account voor Logboeken.** Gebruik niet dezelfde account opslag voor logboeken en toepassingsgegevens. Dit voorkomt dat de registratie van een vermindering van de prestaties van toepassingen. 

- **Prestaties controleren.** Gebruik een service zoals [Nieuwe Relic](http://newrelic.com/) of [Inzichten van toepassing](../application-insights/app-insights-overview.md) op de prestaties van de toepassing controleren en het gedrag onder belasting voor prestatiecontrole.  Prestaties controleren, geeft u real-time inzicht in de toepassing. Hiermee kunt u problemen opsporen en analyse van de onderliggende oorzaak van storingen. 


###  <a name="application-gateway"></a>Application Gateway 

- **Inrichten van ten minste twee exemplaren.** Application Gateway implementeren met ten minste twee exemplaren. Een enkel exemplaar is een potentieel risico. Twee of meer instanties gebruiken voor redundantie en schaalbaarheid. Om de [SLA](https://azure.microsoft.com/support/legal/sla/application-gateway/v1_0/), moet u twee of meer exemplaren voor middelgrote of grotere inrichten. 

### <a name="azure-search"></a>Azure zoeken

- **Meer dan één replica inrichten.** Ten minste twee replica's gebruiken voor het lezen van hoge beschikbaarheid of drie voor lezen en schrijven, hoge beschikbaarheid.

- **Indexeerfuncties voor implementaties met meerdere regio configureren.** Hebt u een distributie met meerdere regio, kunt u uw opties voor continuïteit in de indexering.

    - Als de gegevensbron geo gerepliceerd is, wijs aan elke indexeerfunctie van elke regionale Azure Search-service de bronreplica lokale gegevens.  

    - Als de gegevensbron niet geo gerepliceerd, wijst u meerdere indexeerfuncties op dezelfde gegevensbron zodat Azure zoekservices in meerdere regio's continu en onafhankelijk van de gegevensbron indexeren. Zie voor meer informatie, [Overwegingen bij prestaties en optimalisatie van Azure Search][search-optimization].

###  <a name="azure-storage"></a>Azure opslag 

- **Gebruik voor toepassingsgegevens, geo-redundante opslag lezen-toegang (RA-GRS).** De gegevens worden gerepliceerd naar een gebied met secundaire RA GRS opslag en biedt alleen-lezen toegang van de secundaire regio. Als er een stroomstoring opslag in de primaire regio, kan de toepassing de gegevens lezen van de secundaire regio. Zie [replicatie Azure opslag](../storage/storage-redundancy.md)voor meer informatie.

- **Schijven voor VM Premium opslag gebruiken** Zie voor meer informatie [Premium-opslag: krachtige opslag voor een standaardwerkbelasting Azure Virtual Machine](../storage/storage-premium-storage.md).

- **Voor Queue storage, maakt u een back-wachtrij in een ander gebied.** Queue storage, een alleen-lezen-replica slechts beperkt gebruik, omdat u niet in de wachtrij of wachtrij-items. In plaats daarvan een back-wachtrij maken op een rekening voor opslag in een ander gebied. Als er een stroomstoring opslag, de toepassing de wachtrij back-up gebruiken totdat de primaire regio weer beschikbaar is. Op die manier de toepassing kan nog steeds nieuwe aanvragen verwerken.  

###  <a name="documentdb"></a>DocumentDB 

- **De database gerepliceerd tussen de regio's.** Met een account met meerdere regio bevat DocumentDB één schrijven regio en meerdere lezen regio's. Als er een storing in de regio schrijven, kunt u lezen in een andere replica. De Client SDK doet dit automatisch. U kunt ook niet op het gebied van het schrijven naar een andere regio. Zie [gegevens verdelen wereldwijd met DocumentDB](../documentdb/documentdb-distribute-data-globally.md)voor meer informatie.


###  <a name="sql-database"></a>SQL-Database 

- **Gebruik de Standard of Premium laag.** Deze lagen bieden een periode langer point-in-time restore (35 dagen). Zie [Opties voor SQL-Database en de prestaties](../sql-database/sql-database-service-tiers.md)voor meer informatie.

- **Het inschakelen van controle voor SQL-Database.** Controle kan worden gebruikt voor het vaststellen van schadelijke aanvallen of menselijke fouten. Zie [aan de slag met SQL-database controleren](../sql-database/sql-database-auditing-get-started.md)voor meer informatie. 

- **Gebruik Active Geo-replicatie** Actieve Geo-replicatie gebruikt een secundair leesbaar maken in een andere regio.  Als uw primaire database mislukt, of gewoon moet worden off line genomen, een handmatige failover naar de secundaire database uitvoeren.  Totdat u failover, blijft de secundaire database alleen-lezen.  Zie [SQL Database actief Geo-replicatie](../sql-database/sql-database-geo-replication-overview.md)voor meer informatie. 

- **Gebruik sharding**. U kunt overwegen sharding horizontaal partitioneren van de database. Fout met betrekking tot isolatie bieden sharding. Zie [Scaling out met Azure SQL-Database](../sql-database/sql-database-elastic-scale-introduction.md)voor meer informatie. 

- **Met point-in-time terugzetten herstellen vanuit menselijke fouten.**  Restore Point-in-time retourneert uw database op een eerder tijdstip. Zie voor meer informatie, [Azure SQL-database met behulp van geautomatiseerde back-ups herstellen][sql-restore].

- **Geo-terugzetten herstellen na een stroomstoring service gebruiken.** Geo hersteld-een database van een geo-redundante back-up.  Zie voor meer informatie, [Azure SQL-database met behulp van geautomatiseerde back-ups herstellen][sql-restore].


###  <a name="sql-server-running-in-a-vm"></a>SQL Server (wordt uitgevoerd in een VM)

- **De database wordt gerepliceerd.** SQL Server altijd op beschikbaarheidsgroepen gebruiken om de database te repliceren. Hoge beschikbaarheid biedt als één exemplaar van SQL Server uitvalt. Voor meer informatie, Zie voor [meer informatie...](guidance-compute-n-tier-vm.md) 

- Een **back-up van de database**. Als u al via [Azure back-up](https://azure.microsoft.com/documentation/services/backup/) back-up van uw VMs, kunt u overwegen [Azure back-up voor de werkbelasting van SQL Server met behulp van DPM](../backup/backup-azure-backup-sql.md). Met deze methode wordt een back-beheerdersrol hebben voor de organisatie en een uniforme herstelprocedure voor VMs en SQL Server. Anders gebruikt u [SQL Server beheerd Backup Microsoft Azure](https://msdn.microsoft.com/library/dn449496.aspx). 


###  <a name="traffic-manager"></a>Beheer van netwerkverkeer 

- **Handmatig failback worden uitgevoerd.** Uitvoeren na een failover verkeer Manager handmatig failback in plaats van automatisch weer anders. Alvorens terug, controleert u of alle subsystemen voor toepassing in orde.  Anders kunt u een situatie waarin de toepassing heen en weer tussen datacenters draait. Voor meer informatie Zie [VMs actief in meerdere regio's](guidance-compute-multiple-datacenters.md).

- **Een eindpunt van de sonde gezondheid maken**. Maak een aangepaste eindpunt die over de algemene toestand van de toepassing rapporten. Hiermee wordt verkeer Manager worden uitgevoerd als een kritiek pad niet werkt, niet alleen de front-endtoepassing. Het eindpunt moet een HTTP-foutcode retourneren als een essentiële afhankelijkheid beschadigd of niet bereikbaar is. Rapporteert fouten voor niet-essentiële services echter niet. De sonde gezondheid mogelijk anders failover geactiveerd wanneer het niet nodig, maken van onjuiste positieve detecties. Zie voor meer informatie [verkeer Manager eindpunt controle en failover](../traffic-manager/traffic-manager-monitoring.md).


###  <a name="virtual-machines"></a>Virtuele Machines 

- **Voorkomen dat de werklast voor een productie wordt uitgevoerd op een enkele VM.** Een VM-implementatie is niet tegen of niet gepland onderhoud. Plaats meerdere VMs in een beschikbaarheid of VM schaal instellen met een load balancer op de voorgrond. Om de SLA, moeten ten minste twee virtuele Machines in dezelfde reeks beschikbaarheid worden geïmplementeerd. Zie [Virtuele Machine schaal Sets-overzicht](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md)voor meer informatie. 

- **Geef de beschikbaarheid instellen wanneer u de VM inrichten.** Er is momenteel geen manier om een VM Resource Manager toevoegen aan een beschikbaarheid ingesteld nadat de VM is ingericht. Wanneer u toevoegt een nieuwe VM aan de beschikbaarheid van een bestaande instellen, zorg ervoor dat u een NIC voor VM, en de NIC toevoegen aan de back-end-adresgroep op de verdeling van de belasting. De taakverdeling won't anders netwerkverkeer doorsturen naar die VM. 

- **Elke toepassingslaag in een afzonderlijke Set beschikbaarheid plaatsen.** In een N-tier-toepassing niet plaats VMs van verschillende lagen in dezelfde set beschikbaarheid. VMs in een set beschikbaarheid worden gebracht tussen domeinen veroorzaakt (FDs) en domeinen (UD) bijwerken. Als u het voordeel van de redundantie van FDs en UDs, moet elke VM in de set beschikbaarheid wel kunnen de hetzelfde clientaanvragen te verwerken. 

- **Kies de juiste VM-grootte op basis van prestatie-eisen.** Als u een bestaande werklast naar Azure, beginnen met de VM-grootte die het meest overeenkomt met uw servers op gebouwen. Vervolgens de prestaties van uw werkelijke werkbelasting van CPU, geheugen en schijfruimte IOP's meten en de grootte aanpassen indien nodig. Dit helpt om ervoor te zorgen dat de toepassing zich gedraagt zoals verwacht in een cloud-omgeving. Als u meerdere netwerkkaarten, wordt u op de hoogte van de maximale NIC voor elke grootte. 

- **Premium opslag VHD's gebruiken.** Azure Premium opslag biedt ondersteuning voor schijven met hoge prestaties, lage latentie. Zie voor meer informatie [Premium-opslag: krachtige opslag voor Azure VM werklasten](../storage/storage-premium-storage.md) Kies een VM-grootte die premie opslag ondersteunt. 

- **Een aparte opslag-account maken voor elke VM.** Plaats de VHD's voor één VM rekening gescheiden op te slaan. Dit helpt te voorkomen kunt u door de limieten van IOP's voor opslag rekeningen. Zie voor meer informatie [Azure opslag schaalbaarheid en prestaties, doelen](../storage/storage-scalability-targets.md). Als u een groot aantal VMs implementeert, wel op de hoogte van de limiet per abonnement voor opslag rekeningen. Zie [limieten voor berichtopslag](../azure-subscription-service-limits.md#storage-limits).

- **Een aparte opslag account voor diagnostische logboeken maken**. Diagnostische logboeken schrijven naar dezelfde rekening als de VHD's, hoeft u het vastleggen van diagnostische gegevens opslag geen invloed hebben op de IOP's voor de VM-schijven. Een standaardaccount lokaal redundante opslag (LRS) is voldoende voor diagnostische logboeken. 

- **Toepassingen installeren op een schijf met gegevens, niet op de OS-schijf.** Anders kan je de limiet voor de schijf bereiken. 

- **Azure back-up gebruiken om een back-up van VMs.** Back-ups beveiligen tegen onbedoeld gegevensverlies. Zie [Azure VMs beschermen met een kluis recovery services](../backup/backup-azure-vms-first-look-arm.md)voor meer informatie.

- **Diagnostische logboeken inschakelen**, met inbegrip van basic gezondheid maatstaven, logboeken van de infrastructuur en [boot diagnostische gegevens][boot-diagnostics]. Diagnostisch opstarten kunt u een opstartfout diagnose als uw VM in een niet-opstartbare staat haalt. Zie [Overzicht van Azure diagnostische logboeken]voor meer informatie[diagnostics-logs].

- **Gebruik de extensie AzureLogCollector**. (Alleen Windows VMs.) Deze extensie voegt Azure platform logboeken en ze uploaden naar Azure opslag, zonder dat de operator extern aanmelden bij de VM. Zie voor meer informatie [De extensie AzureLogCollector](../virtual-machines/virtual-machines-windows-log-collector-extension.md).


###  <a name="virtual-network"></a>Virtueel netwerk 

- **"Witte" lijst of blok toevoegen openbare IP-adressen, een NSG aan het subnet.** Alleen van gebruikers die beschikken over bevoegdheden voor de toepassing toestaan of blokkeren van de toegang van kwaadwillende gebruikers.  

- **Maak een aangepaste gezondheid sonde.** Load Balancer gezondheid sondes kunt testen HTTP- of TCP. Als een HTTP-server wordt uitgevoerd in een VM, is de sonde HTTP een betere indicator van de gezondheidsstatus dan een sonde met TCP. Gebruik een aangepaste eindpunt dat rapporten van de algehele gezondheid van de toepassing, met inbegrip van alle kritieke afhankelijkheden voor een sonde HTTP. Zie [Azure Load Balancer-overzicht](../load-balancer/load-balancer-overview.md)voor meer informatie.

- **De sonde gezondheid niet worden geblokkeerd.** De Load Balancer gezondheid sonde wordt verzonden vanaf een bekend IP-adres 168.63.129.16. Geen verkeer of naar dit IP-netwerk security group (NSG) regels of firewallbeleid van blokkeren. Blokkeren van de sonde gezondheid zou de verdeling van de belasting voor het verwijderen van de VM van rotatie veroorzaken. 

- **Load Balancer-logboekregistratie inschakelen.** De logboeken weergeven hoeveel VMs in de back-end netwerkverkeer als gevolg van defecte sonde antwoorden niet ontvangt. Zie [logboek analytics voor Azure taakverdeling](../load-balancer/load-balancer-monitor-log.md)voor meer informatie.


<!-- links -->
[app-service-autoscale]: ../monitoring-and-diagnostics/insights-how-to-scale.md
[asynchronous-c-sharp]:https://msdn.microsoft.com/library/mt674882.aspx
[availability-sets]:../virtual-machines/virtual-machines-windows-manage-availability.md
[azure-backup]: https://azure.microsoft.com/documentation/services/backup/
[boot-diagnostics]: https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/
[circuit-breaker]: https://msdn.microsoft.com/library/dn589784.aspx
[cloud-service-autoscale]: ../cloud-services/cloud-services-how-to-scale.md
[diagnostics-logs]: ../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md
[fma]: guidance-resiliency-failure-mode-analysis.md
[guidance-resilient-deployment]: guidance-resiliency-overview.md#resilient-deployment
[load-balancer]: load-balancer/load-balancer-overview.md
[monitoring-and-diagnostics-guidance]: ../best-practices-monitoring.md
[resource-manager]: ../azure-resource-manager/resource-group-overview.md
[retry-pattern]: https://msdn.microsoft.com/library/dn589788.aspx
[retry-service-guidance]: ../best-practices-retry-service-specific.md
[search-optimization]: ../search/search-performance-optimization.md
[sql-backup]: ../sql-database/sql-database-automated-backups.md
[sql-restore]: ../sql-database/sql-database-recovery-using-backups.md
[traffic-manager]: ../traffic-manager/traffic-manager-overview.md
[traffic-manager-routing]: ../traffic-manager/traffic-manager-routing-methods.md
[vmss-autoscale]: ../virtual-machine-scale-sets/virtual-machine-scale-sets-autoscale-overview.md
