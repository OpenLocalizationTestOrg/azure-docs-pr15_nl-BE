<properties
   pageTitle="Statuscontrole in Service Fabric | Microsoft Azure"
   description="Een inleiding tot de Azure Service Fabric gezondheid model, waarmee de bewaking van het cluster en de toepassingen en services te controleren."
   services="service-fabric"
   documentationCenter=".net"
   authors="oanapl"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/28/2016"
   ms.author="oanapl"/>

# <a name="introduction-to-service-fabric-health-monitoring"></a>Inleiding tot Service Fabric statuscontrole
Azure Service Fabric introduceert een statusmodel met krachtige, flexibele en uitbreidbare gezondheid evaluatie en rapportage. Het model kunt in de buurt van de real-time bewaking van de status van het cluster en de services die worden uitgevoerd in het. Gemakkelijk verkrijgen van informatie over gezondheid en corrigeren potentiële problemen voordat ze trapsgewijs en enorme storingen veroorzaken. In het standaard model services verzonden rapporten op basis van hun lokale standpunt en dat gegevens worden samengevoegd om een algemeen cluster niveau weergeven.

Fabric service-onderdelen gebruiken dit model veel gezondheid te rapporteren hun huidige status. U kunt hetzelfde mechanisme rapport gezondheid van uw toepassingen. Als u investeert in goede gezondheid melden dat uw aangepaste voorwaarden worden vastgelegd, kunt u detecteren en oplossen van problemen voor de actieve toepassing veel gemakkelijker.

> [AZURE.NOTE] We begonnen met het subsysteem gezondheid om de behoefte aan een gecontroleerde upgrades. Fabric-service beschikt over een gecontroleerde toepassing en cluster upgrades die volledige beschikbaarheid, geen downtime en minimale tot geen tussenkomst van de gebruiker. Om deze doelstellingen te bereiken, controleert de upgrade op basis van gezondheid upgrade beleid geconfigureerd en kunt een upgrade doorgaan alleen wanneer de gezondheid van gewenste drempels eerbiedigt. Anders wordt wordt de upgrade automatisch hersteld of gepauzeerd zodat beheerders een kans om de problemen opgelost. Zie voor meer informatie over toepassingsupgrades, de [in dit artikel](service-fabric-application-upgrade.md).

## <a name="health-store"></a>Gezondheid winkel
Het archief van de gezondheid, gezondheid verband houdende informatie over entiteiten blijft in gemakkelijk kunt ophalen en de evaluatie van de cluster. Het is geïmplementeerd als een Service Fabric stateful-service vastgelegde om ervoor te zorgen voor hoge beschikbaarheid en schaalbaarheid. De gezondheid winkel maakt deel uit van de **fabric: / System** toepassing en deze zijn beschikbaar als het cluster actief is.

## <a name="health-entities-and-hierarchy"></a>Gezondheid entiteiten en hiërarchie
De gezondheid entiteiten zijn ingedeeld in een logische hiërarchie waarin interacties en afhankelijkheden tussen verschillende entiteiten zijn vastgelegd. Entiteiten en hiërarchie ingebouwd automatisch door de gezondheid winkel op basis van rapporten van Fabric Service onderdelen ontvangen.

De diensten van de gezondheid van mirror de entiteiten Service Fabric. (Bijvoorbeeld **gezondheid Toepassingsentiteit** komt overeen met een toepassingsexemplaar geïmplementeerd in het cluster, terwijl **gezondheid knooppunt entiteit** komt overeen met een clusterknooppunt Service Fabric.) De hiërarchie van de gezondheid van de interactie van de systeementiteiten worden vastgelegd en het is de basis voor de evaluatie van de gezondheid van geavanceerde. U kunt informatie over belangrijke Fabric Service concepten in [Service Fabric-technisch overzicht](service-fabric-technical-overview.md). Zie voor meer informatie over de toepassing, [Service Fabric toepassingsmodel](service-fabric-application-model.md).

De diensten van de gezondheid en de hiërarchie kunnen de cluster en toepassingen worden daadwerkelijk gerapporteerd, gecontroleerd en bewaakt. Het statusmodel geeft nauwkeurige, *gedetailleerde* weergave van de gezondheid van de vele bewegende delen in het cluster.

![Diensten voor de gezondheid.][1]
De entiteiten gezondheid geordend in een hiërarchie op basis van de bovenliggende-onderliggende relaties.

[1]: ./media/service-fabric-health-introduction/servicefabric-health-hierarchy.png

De gezondheid van diensten zijn:

- **Cluster**. Hiermee geeft u de status van een Service Fabric '-cluster. Cluster gezondheid rapporten beschreven omstandigheden die van invloed zijn op het gehele cluster en kunnen niet aan een of meer beschadigde kinderen worden teruggebracht. Voorbeelden zijn de hersenen van het cluster door netwerk partitioneren of communicatie problemen splitsen.

- **Knooppunt**. Hiermee geeft u de status van een knooppunt Fabric-Service. Knooppunt statusrapporten beschrijven omstandigheden die van invloed zijn op de functionaliteit van het knooppunt. Ze meestal invloed op de geïmplementeerde entiteiten uitgevoerd. Voorbeelden zijn als een knooppunt niet op schijfruimte (of een andere eigenschap van de hele machine, zoals geheugen, verbindingen wordt) en wanneer een knooppunt verbroken is. De entiteit knooppunt wordt geïdentificeerd door de naam van het knooppunt (tekenreeks).

- **Toepassing**. Hiermee geeft u de status van de instantie van een toepassing in het cluster worden uitgevoerd. Statusrapporten toepassing beschreven omstandigheden die van invloed zijn op de algehele gezondheid van de toepassing. Ze kunnen niet worden teruggebracht naar afzonderlijke onderliggende objecten (services of gedistribueerde toepassingen). Voorbeelden zijn de end-to-end-interactie tussen de verschillende diensten in de toepassing. De Toepassingsentiteit wordt geïdentificeerd door de naam van de toepassing (URI).

- **Service**. Hiermee geeft u de status van een service die wordt uitgevoerd in het cluster. Statusrapporten service beschreven omstandigheden die van invloed zijn op de algehele gezondheid van de service en kan niet worden teruggebracht tot een partitie of een replica. Voorbeelden zijn een configuratie van de service (bijvoorbeeld poort of externe bestandsshare) veroorzaakt problemen voor alle partities. De service-entiteit wordt aangeduid met de servicenaam van de (URI).

- **Partitie**. Hiermee geeft u de status van een service-partitie. Statusrapporten partitie beschreven omstandigheden die van invloed zijn op de hele replicaset. Voorbeelden zijn het aantal replica's is lager dan het aantal doel en een partitie is quorum verloren. De partitie-eenheid wordt aangeduid met de partitie-ID (GUID).

- **Replica**. Hiermee geeft u de status van een replica van de stateful-service of een staatloze service-exemplaar. De kleinste eenheid die watchdogs en systeemonderdelen kunnen rapporteren voor een toepassing. Voor stateful services, voorbeelden zijn een primaire replica melden wanneer deze bewerkingen kan niet repliceren naar secundaire servers en wanneer replicatie is niet de verwachte plaats tempo. Een stateless exemplaar kan ook rapporteren wanneer deze wordt uitgevoerd, onvoldoende bronnen of problemen heeft. De replica entiteit wordt aangeduid met de partitie-ID (GUID) en de replica of het exemplaar-ID (lang).

- **DeployedApplication**. Hiermee geeft u de status van een *toepassing wordt uitgevoerd op een knooppunt*. Gedistribueerde toepassing statusrapporten beschrijven omstandigheden die specifiek zijn voor de toepassing op het knooppunt dat naar servicepakketten geïmplementeerd op hetzelfde knooppunt kan niet worden teruggebracht. Voorbeelden zijn het toepassingspakket op het knooppunt en als er een probleem is het instellen van toepassing beveiligings-principals op het knooppunt kan niet worden gedownload. De gedistribueerde toepassing wordt geïdentificeerd door de naam van de toepassing (URI) en de knooppuntnaam (tekenreeks).

- **DeployedServicePackage**. Hiermee geeft u de status van een servicepakket uitgevoerd op een knooppunt in het cluster. Voorwaarden voor een servicepakket hebben geen invloed op andere servicepakketten op hetzelfde knooppunt voor dezelfde toepassing worden beschreven. Voorbeelden zijn een pakket van de code in het servicepakket kan niet worden gestart en een configuratiepakket dat kan worden gelezen. Het servicepakket van de gedistribueerde wordt geïdentificeerd door de toepassingsnaam (URI), knooppuntnaam (tekenreeks) en manifest servicenaam (tekenreeks).

De granulatie van het statusmodel gemakkelijk opsporen en oplossen van problemen. Bijvoorbeeld, als een service niet reageert, is het haalbaar is om te melden dat het exemplaar van de toepassing beschadigd is. Rapportage op dat niveau niet ideaal, maar is omdat het probleem zonder de services binnen die toepassing dat mogelijk. Het rapport moet worden toegepast op de beschadigde service of een specifieke onderliggende partitie, als u meer informatie verwijst naar die partitie. De gegevens automatisch oppervlakken door middel van de hiërarchie en een beschadigde partitie is zichtbaar gemaakt op diensten- en niveaus. Deze samenvoeging kunt identificeren en de oorzaak van het probleem sneller opgelost.

De hiërarchie van de gezondheid is samengesteld uit de bovenliggende-onderliggende relaties. Een cluster bestaat uit knooppunten en toepassingen. Toepassingen services en toepassingen geïmplementeerd. Gebruikte toepassingen hebt geïmplementeerd servicepakketten. Services zijn partities en elke partitie heeft een of meer replica's. Er is een speciale relatie tussen knooppunten en geïmplementeerde entiteiten. Als een knooppunt is beschadigd, zoals gerapporteerd door de autoriteit systeemonderdeel (Failover Management service), geldt voor de gebruikte toepassingen, servicepakketten en geïmplementeerd op deze replica's.

De hiërarchie van de gezondheid van vertegenwoordigt de meest recente status van het systeem op basis van de meest recente rapporten van de gezondheid is bijna real-time informatie.
Interne en externe watchdogs kunnen op basis van specifieke logica of aangepaste gecontroleerde omstandigheden dezelfde diensten rapporteren. Gebruikersrapporten worden gecombineerd met de systeemrapporten.

Plannen om te investeren in het rapport en reageren op de gezondheid tijdens het ontwerp van een grote cloud-service, zodat de service beter debug, bewaken en beheren.

## <a name="health-states"></a>Statussen
Fabric-service maakt gebruik van drie statussen beschrijven of een entiteit al dan niet in orde is: OK, waarschuwing en fout. Alle rapporten die zijn verzonden naar het archief van de gezondheid moet een van deze Staten opgeven. Het resultaat van de evaluatie gezondheid is een van deze Staten.

De mogelijke [statussen](https://msdn.microsoft.com/library/azure/system.fabric.health.healthstate) zijn:

- **OK**. De entiteit is in orde. Er zijn geen bekende problemen gerapporteerd of de onderliggende objecten (indien van toepassing).

- **Waarschuwing**. De entiteit sommige problemen ondervindt, maar is nog niet beschadigd (bijvoorbeeld: zijn er vertragingen, maar ze veroorzaken geen functionele problemen nog). In sommige gevallen de waarschuwingsvoorwaarde kan zelf oplossen zonder tussenkomst van de speciale en is het nuttig om inzicht in wat er gaande is. In andere gevallen kan de waarschuwingsvoorwaarde afnemen in een ernstig probleem zonder tussenkomst van de gebruiker.

- **Fout**. De entiteit is beschadigd. Actie moet worden genomen om te herstellen van de staat van de entiteit, omdat deze niet naar behoren.

- **Onbekend**. De entiteit bestaat niet in het archief van de gezondheid. Dit resultaat kan worden verkregen vanuit de gedistribueerde query's resultaten uit meerdere onderdelen samenvoegen. Haalt knooppunt een query elektroplaat **FailoverManager** en **HealthManager**, get-toepassing een query gaat naar **ClusterManager** en **HealthManager**. Deze query's samenvoegen resultaten uit meerdere onderdelen van het systeem. Als systeemonderdeel van een ander een entiteit die niet heeft bereikt, of uit het archief van de gezondheid is opgeruimd geeft, het samengevoegde resultaat heeft onbekende status.

## <a name="health-policies"></a>Statusbeleid
Het archief van de gezondheid van toepassing statusbeleid om te bepalen of een entiteit orde op basis van de rapporten en de onderliggende.

> [AZURE.NOTE] Statusbeleid kunnen worden opgegeven in het manifest van het cluster (voor clusters en knooppunten gezondheid evaluatie) of in het manifest van de toepassing (voor de evaluatie van de toepassing en alle onderliggende knooppunten). Gezondheid beoordeling aanvragen kunnen doorgeven ook aangepaste evaluatie statusbeleid wordt alleen gebruikt voor deze evaluatie.

Standaard geldt Service Fabric strenge regels (Alles moet gezonde) voor het hiërarchische relatie bovenliggend-onderliggend. Als één van de kinderen een beschadigde gebeurtenis heeft, het bovenliggende object wordt beschouwd als beschadigd.

### <a name="cluster-health-policy"></a>Cluster beleid
Het [cluster statusbeleid](https://msdn.microsoft.com/library/azure/system.fabric.health.clusterhealthpolicy.aspx) wordt gebruikt voor de evaluatie van de status van de cluster en een knooppunt statussen. Het beleid kan worden gedefinieerd in het manifest van het cluster. Als dit niet aanwezig is, wordt het standaardbeleid (nul fouten verdragen) gebruikt.
Het statusbeleid cluster bevat:

- [ConsiderWarningAsError](https://msdn.microsoft.com/library/azure/system.fabric.health.clusterhealthpolicy.considerwarningaserror.aspx). Hiermee geeft u aan of gezondheid waarschuwing beschouwen als fouten tijdens de beoordeling van de gezondheid. Default: false.

- [MaxPercentUnhealthyApplications](https://msdn.microsoft.com/library/azure/system.fabric.health.clusterhealthpolicy.maxpercentunhealthyapplications.aspx). Hiermee geeft u het maximumpercentage verdragen van toepassingen die beschadigd worden kunnen voordat het cluster als onjuist wordt beschouwd.

- [MaxPercentUnhealthyNodes](https://msdn.microsoft.com/library/azure/system.fabric.health.clusterhealthpolicy.maxpercentunhealthynodes.aspx). Hiermee geeft u het maximumpercentage verdragen van knooppunten die beschadigd worden kunnen voordat het cluster als onjuist wordt beschouwd. In grote clusters sommige knooppunten zijn altijd naar beneden of uit voor reparaties, zodat dit percentage moet zodanig zijn dat tolereren.

- [ApplicationTypeHealthPolicyMap](https://msdn.microsoft.com/library/azure/system.fabric.health.clusterhealthpolicy.applicationtypehealthpolicymap.aspx). De toepassing type gezondheid beleid kan worden gebruikt tijdens de beoordeling van cluster gezondheid typen speciale toepassingen te beschrijven. Standaard worden alle toepassingen in een groep plaatsen en bij de beoordeling van MaxPercentUnhealthyApplications. Als bepaalde toepassingstypen moeten anders worden behandeld, kunnen worden gemaakt van de globale groep. Ze worden geëvalueerd tegen de percentages die zijn gekoppeld aan de naam van hun toepassing in de kaart. Bijvoorbeeld in een cluster zijn er duizenden toepassingen van verschillende typen, en een paar besturingselement toepassingsexemplaren van een type speciale toepassing. De toepassingen zijn nooit fout. U kunt globale MaxPercentUnhealthyApplications tot 20% in sommige gevallen tolereren, maar voor het toepassingstype "ControlApplicationType" de MaxPercentUnhealthyApplications ingesteld op 0. Op deze manier zou als sommige van de vele toepassingen beschadigd zijn, maar onder het algemene percentage beschadigd, het cluster worden geëvalueerd voor de waarschuwing. Upgrade van cluster heeft geen gevolgen voor de status van een waarschuwing of andere controle geactiveerd door de status van de fout. Maar zelfs een beheertoepassing fout zou beschadigd en cluster die vorig activeert of de upgrade van de cluster, afhankelijk van de configuratie van de upgrade wordt onderbroken.
Voor de toepassing in de map gedefinieerd, worden alle toepassingsexemplaren genomen uit de globale groep van toepassingen. Ze worden geëvalueerd op basis van het totale aantal toepassingen van het toepassingstype, met behulp van de specifieke MaxPercentUnhealthyApplications van de kaart. De rest van de toepassingen in de globale groep blijven en met MaxPercentUnhealthyApplications worden geëvalueerd.

In het volgende voorbeeld is een fragment uit een manifest van het cluster. Voorvoegsel om te posten in de overzichtsweergave van het type toepassing definiëren, de parameternaam van de met "ApplicationTypeMaxPercentUnhealthyApplications-", gevolgd door de naam van de toepassing.

```xml
<FabricSettings>
  <Section Name="HealthManager/ClusterHealthPolicy">
    <Parameter Name="ConsiderWarningAsError" Value="False" />
    <Parameter Name="MaxPercentUnhealthyApplications" Value="20" />
    <Parameter Name="MaxPercentUnhealthyNodes" Value="20" />
    <Parameter Name="ApplicationTypeMaxPercentUnhealthyApplications-ControlApplicationType" Value="0" />
  </Section>
</FabricSettings>
```

### <a name="application-health-policy"></a>Toepassingenbeleid gezondheid.
Het [statusbeleid toepassing](https://msdn.microsoft.com/library/azure/system.fabric.health.applicationhealthpolicy.aspx) wordt beschreven hoe de evaluatie van de gebeurtenissen en onderliggende Staten samenvoeging wordt uitgevoerd voor toepassingen en hun kinderen. Het kan worden gedefinieerd in het manifest van de toepassing, **ApplicationManifest.xml**, in het toepassingspakket. Als er geen beleidsregels zijn opgegeven, Fabric-Service wordt ervan uitgegaan dat de entiteit beschadigd is als er een statusrapport of een kind op de status waarschuwing of fout.
De configureerbare beleid zijn:

- [ConsiderWarningAsError](https://msdn.microsoft.com/library/azure/system.fabric.health.applicationhealthpolicy.considerwarningaserror.aspx). Hiermee geeft u aan of gezondheid waarschuwing beschouwen als fouten tijdens de beoordeling van de gezondheid. Default: false.

- [MaxPercentUnhealthyDeployedApplications](https://msdn.microsoft.com/library/azure/system.fabric.health.applicationhealthpolicy.maxpercentunhealthydeployedapplications.aspx). Hiermee geeft u het maximumpercentage verdragen van gedistribueerde toepassingen die beschadigd worden kunnen voordat de toepassing wordt beschouwd als fout. Dit percentage wordt berekend door het aantal beschadigde gedistribueerde toepassingen te verdelen over het aantal knooppunten die de toepassingen die momenteel worden geïmplementeerd op in het cluster. De berekening wordt afgerond tot op één storing op een klein aantal knooppunten tolereren. Standaard percentage: nul.

- [DefaultServiceTypeHealthPolicy](https://msdn.microsoft.com/library/azure/system.fabric.health.applicationhealthpolicy.defaultservicetypehealthpolicy.aspx). Hiermee geeft u de standaard service type statusbeleid, die de standaard statusbeleid voor alle servicetypen in de toepassing vervangt.

- [ServiceTypeHealthPolicyMap](https://msdn.microsoft.com/library/azure/system.fabric.health.applicationhealthpolicy.servicetypehealthpolicymap.aspx). Geeft een overzicht van het volksgezondheidsbeleid service per servicetype. Dit beleid vervangen door de standaard service type statusbeleid voor elk type van de opgegeven service. Bijvoorbeeld als een toepassing een stateless gateway service-type en een type stateful engine-service heeft, kunt u het statusbeleid voor hun evaluatie verschillend. Wanneer u het beleid per servicetype opgeeft, kunt u meer gedetailleerde controle van de gezondheid van de service toegang.

### <a name="service-type-health-policy"></a>Service type statusbeleid
Het [statusbeleid voor service-type](https://msdn.microsoft.com/library/azure/system.fabric.health.servicetypehealthpolicy.aspx) geeft aan hoe te evalueren en de services en de onderliggende services samen te voegen. Het beleid bevat:

- [MaxPercentUnhealthyPartitionsPerService](https://msdn.microsoft.com/library/azure/system.fabric.health.servicetypehealthpolicy.maxpercentunhealthypartitionsperservice.aspx). Bevat het maximumpercentage verdragen van beschadigde partities voordat u een service wordt gezien als beschadigd. Standaard percentage: nul.

- [MaxPercentUnhealthyReplicasPerPartition](https://msdn.microsoft.com/library/azure/system.fabric.health.servicetypehealthpolicy.maxpercentunhealthyreplicasperpartition.aspx). Hiermee geeft u het maximumpercentage verdragen van beschadigde replica's voordat een partitie als beschadigd wordt beschouwd. Standaard percentage: nul.

- [MaxPercentUnhealthyServices](https://msdn.microsoft.com/library/azure/system.fabric.health.servicetypehealthpolicy.maxpercentunhealthyservices.aspx). Hiermee geeft u het maximumpercentage verdragen van beschadigde services voordat de toepassing wordt beschouwd wat de netwerkverbindingsmogelijkheden ervan. Standaard percentage: nul.

In het volgende voorbeeld is een fragment uit een manifest van de toepassing:

```xml
    <Policies>
        <HealthPolicy ConsiderWarningAsError="true" MaxPercentUnhealthyDeployedApplications="20">
            <DefaultServiceTypeHealthPolicy
                   MaxPercentUnhealthyServices="0"
                   MaxPercentUnhealthyPartitionsPerService="10"
                   MaxPercentUnhealthyReplicasPerPartition="0"/>
            <ServiceTypeHealthPolicy ServiceTypeName="FrontEndServiceType"
                   MaxPercentUnhealthyServices="0"
                   MaxPercentUnhealthyPartitionsPerService="20"
                   MaxPercentUnhealthyReplicasPerPartition="0"/>
            <ServiceTypeHealthPolicy ServiceTypeName="BackEndServiceType"
                   MaxPercentUnhealthyServices="20"
                   MaxPercentUnhealthyPartitionsPerService="0"
                   MaxPercentUnhealthyReplicasPerPartition="0">
            </ServiceTypeHealthPolicy>
        </HealthPolicy>
    </Policies>
```

## <a name="health-evaluation"></a>Evaluatie van de gezondheid
Gebruikers en geautomatiseerde services kunnen evalueren van de status voor een entiteit op elk gewenst moment. Evaluatie van de gezondheid van de entiteit, de gezondheid winkel aggregaten alle gezondheid rapporten op de entiteit en evalueert alle onderliggende (indien van toepassing). De aggregatie van gezondheid algoritme gebruikt statusbeleid opgeven voor de evaluatie van statusrapporten en statistische onderliggende statussen (indien van toepassing).

### <a name="health-report-aggregation"></a>Aggregatie van de gezondheid van rapport
Een entiteit kan meerdere statusrapporten verzonden door verschillende verslaggevers (onderdelen van het systeem of watchdogs) op verschillende eigenschappen hebben. De aggregatie gebruikt de bijbehorende statusbeleid, met name het lid ConsiderWarningAsError van de toepassing of het cluster statusbeleid. ConsiderWarningAsError geeft aan hoe waarschuwingen te evalueren.

De samengevoegde status wordt geactiveerd door de *ergste* statusrapporten op de entiteit. Als er ten minste één rapport gezondheid, is de geaggregeerde gezondheid staat een fout.

![Aggregatie voor de gezondheid van rapport met een foutenrapport.][2]

Een entiteit gezondheid met een of meer gezondheid foutenrapporten wordt geëvalueerd als een fout. Hetzelfde geldt voor een rapport verlopen gezondheid, ongeacht de status.

[2]: ./media/service-fabric-health-introduction/servicefabric-health-report-eval-error.png

Als er geen foutrapporten en een of meer waarschuwingen, is de geaggregeerde status waarschuwing of fout, afhankelijk van de vlag ConsiderWarningAsError beleid.

![Gezondheid rapport samenvoegen met waarschuwing rapport en ConsiderWarningAsError false.][3]

Gezondheid rapport samenvoegen met waarschuwing rapport en ConsiderWarningAsError ingesteld op false (standaard).

[3]: ./media/service-fabric-health-introduction/servicefabric-health-report-eval-warning.png

### <a name="child-health-aggregation"></a>Onderliggende gezondheid aggregatie
De samengevoegde status van een entiteit weerspiegelt de statussen van het kind (indien van toepassing). De algoritme voor het verzamelen van onderliggende statussen maakt gebruik van het statusbeleid van toepassing op basis van het entiteitstype.

![Onderliggende entiteiten gezondheid aggregatie.][4]

Onderliggende-aggregatie op basis van het statusbeleid.

[4]: ./media/service-fabric-health-introduction/servicefabric-health-hierarchy-eval.png

Nadat het archief van de gezondheid van alle kinderen heeft beoordeeld aggregeert deze hun gezondheid staat op basis van het geconfigureerde maximum percentage van beschadigde kinderen. Dit percentage wordt overgenomen uit het beleid op basis van het type entiteit en een kind.

- Als alle kinderen OK Staten, is de status van de onderliggende samengevoegd OK.

- Als kinderen OK en Staten waarschuwing hebt, wordt de status van de onderliggende samengevoegd waarschuwing.

- Als er kinderen met fout van Staten die geen rekening voor de maximaal toegestane percentage van beschadigde kinderen gehouden, is de geaggregeerde gezondheid staat een fout.

- Als de kinderen met fout lidstaten met inachtneming van de maximaal toegestane percentage van beschadigde kinderen, waarschuwing de status van de geaggregeerde.

## <a name="health-reporting"></a>Gezondheid melden
Onderdelen van het systeem, systeem Fabric toepassingen en interne of externe watchdogs kunnen Fabric Service entiteiten rapporteren. De verslaggevers zorg *lokale* bepalingen van de gezondheid van de gecontroleerde entiteiten, op basis van de voorwaarden die zij controleren. Ze hoeft niet te kijken naar een globale staat of statistische gegevens. Het gewenste gedrag is te eenvoudig verslaggevers en niet-complexe organismen die u wilt zoeken op een aantal zaken afleiden welke informatie te verzenden.

Om gezondheidsgegevens te verzenden naar de winkel voor gezondheid, moet een reporter voor het identificeren van de betrokken entiteit en een statusrapport maken. Het rapport kan vervolgens worden verzonden via de API via [FabricClient.HealthClient.ReportHealth](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient_members.aspx), via PowerShell, of REST.

### <a name="health-reports"></a>Statusrapporten
De [statusrapporten](https://msdn.microsoft.com/library/azure/system.fabric.health.healthreport.aspx) voor elk van de entiteiten in het cluster bevatten de volgende informatie:

- **Bron-id**. Een tekenreeks die een unieke aanduiding voor de reporter van de gebeurtenis voor de gezondheid.

- **Entiteits-id**. Identificatie van de dienst waar het rapport wordt toegepast. Deze verschilt op basis van de [entiteit, typ](service-fabric-health-introduction.md#health-entities-and-hierarchy):

  - Het cluster. Geen.

  - Knooppunt. Knooppuntnaam (tekenreeks).

  - Toepassing. De naam van toepassing (URI). Hiermee geeft u de naam van het toepassingsexemplaar dat is geïmplementeerd in het cluster.

  - Service. Naam van service (URI). Hiermee geeft u de naam van het exemplaar van de geïmplementeerd in het cluster.

  - Partitie. Partitie-ID (GUID). Hiermee geeft u de unieke id van de partitie.

  - Replica. De stateful replica-ID of de staatloze exemplaar-ID (INT64).

  - DeployedApplication. Naam van de toepassing (URI) en de knooppuntnaam (tekenreeks).

  - DeployedServicePackage. Naam van de toepassing (URI), knooppuntnaam (tekenreeks) en service manifest van de naam (tekenreeks).

- **Eigenschap**. Een *tekenreeks* (geen vaste opsomming) waarmee de reporter voor het categoriseren van de gezondheid van gebeurtenis voor een bepaalde eigenschap van de entiteit. Bijvoorbeeld kan A reporter rapporteren over dat de gezondheid van de eigenschap Node01 'storage' en reporter B kan rapporteren over de status van de eigenschap Node01 'verbinding'. In de winkel de gezondheid van worden deze rapporten behandeld als afzonderlijke gezondheid gebeurtenissen voor de entiteit Node01.

- **Beschrijving**. Een tekenreeks waarmee een reporter op vindt u gedetailleerde informatie over de gebeurtenis voor de gezondheid. **Bron-id**, **eigenschap**en **HealthState** moeten het rapport volledig worden beschreven. De omschrijving wordt leesbare informatie over het rapport toegevoegd. De tekst gemakkelijker voor beheerders en gebruikers te begrijpen van het statusrapport.

- **HealthState**. Een [opsomming](service-fabric-health-introduction.md#health-states) die de status van het rapport wordt beschreven. De geaccepteerde waarden zijn OK, waarschuwing en fout.

- **TimeToLive**. Een timespan die aangeeft hoe lang het statusrapport geldig is. In combinatie met **RemoveWhenExpired**, kunnen de gezondheid winkel verlopen gebeurtenissen te evalueren. De waarde is oneindig en het rapport is altijd geldig.

- **RemoveWhenExpired**. Een Boole-waarde. Als is ingesteld op true, wordt het rapport verlopen gezondheid wordt automatisch verwijderd uit het archief van de gezondheid en het rapport niet van invloed op entiteit gezondheid te kunnen beoordelen. Gebruikt wanneer het rapport voor een bepaalde periode slechts geldig is en moet de verslaggever niet expliciet ruimen. Het wordt ook gebruikt om rapporten verwijderen uit het archief van de gezondheid (bijvoorbeeld een watchdog is gewijzigd en stopt met het verzenden van rapporten met vorige bron en de eigenschap). Een rapport met een korte TimeToLive en RemoveWhenExpired uit te schakelen van een vorige staat van het archief van de gezondheid kan verzenden. Als de waarde is ingesteld op false, wordt het rapport verlopen wordt behandeld als een fout op de gezondheid te kunnen beoordelen. De waarde false geeft aan het archief van de gezondheid die de bron moet periodiek verslag over deze eigenschap. Als dat niet het geval is, moet er iets mis is met de watchdog. De gezondheid van de watchdog is vastgelegd op basis van de gebeurtenis als een fout.

- **SequenceNumber**. Een positief geheel getal dat moet worden steeds meer, deze geeft de volgorde van de rapporten. Het wordt gebruikt door de winkel de gezondheid van verouderde rapporten die laat zijn ontvangen door vertragingen in het netwerk of andere problemen detecteren. Een rapport wordt geweigerd als het volgnummer is dat kleiner dan of gelijk is aan de meest recent nummer voor dezelfde entiteit, de bron en de eigenschap toegepast. Als dit niet is opgegeven, wordt het volgnummer wordt automatisch gegenereerd. Het is noodzakelijk om te zetten in het volgnummer alleen bij de voortgangsrapportage over overgangen staat. In dit geval moet de bron te onthouden welke rapporten het verzonden, zodat de informatie voor herstel op failover.

Deze vier stuks van informatie--bron-id, entiteits-id, eigenschap en HealthState--zijn vereist voor elke statusrapport. De bron-id tekenreeks mag niet beginnen met het voorvoegsel "**System.**", dat is gereserveerd voor systeem-rapporten. Er is slechts één rapport voor de dezelfde bron en de eigenschap voor de dezelfde entiteit. Meerdere rapporten voor de bron en de eigenschap negeren elkaar, de aan de clientzijde gezondheid (als ze batch verwerkt zijn) of op de gezondheid winkel aan de zijkant. De vervanging is gebaseerd op een reeks getallen. nieuwe rapporten (met een hoger volgordenummer) vervangen door oudere rapporten.

### <a name="health-events"></a>Gezondheid-gebeurtenissen
Intern, houdt de winkel gezondheid [gezondheid gebeurtenissen](https://msdn.microsoft.com/library/azure/system.fabric.health.healthevent.aspx), die de informatie van de rapporten en aanvullende metagegevens bevatten. De metagegevens bevat de tijd die het rapport werd gegeven aan de client van de gezondheid en de tijd die op de server is gewijzigd. De gebeurtenissen van de gezondheid worden geretourneerd door de [query's voor de gezondheid](service-fabric-view-entities-aggregated-health.md#health-queries).

De extra metagegevens bevat:

- **SourceUtcTimestamp**. Het moment dat het rapport werd besteed aan de gezondheid-client (Coordinated Universal Time).

- **LastModifiedUtcTimestamp**. De tijd die het rapport het laatst is gewijzigd op de server (Coordinated Universal Time).

- **IsExpired**. Een vlag om aan te geven of het rapport als de query is uitgevoerd door de winkel de gezondheid is verlopen. Een gebeurtenis kan zijn verlopen als RemoveWhenExpired ingesteld op false is. Anders wordt de gebeurtenis niet wordt geretourneerd door de query en verwijderd uit de winkel.

- **LastOkTransitionAt**, **LastWarningTransitionAt**, **LastErrorTransitionAt**. De laatste tijd voor OK/Waarschuwingsfout overgangen. Deze velden geven de geschiedenis van de gezondheid overgangen staat voor de gebeurtenis.

De velden staat overgang kunnen worden gebruikt voor slimmer waarschuwingen of informatie over gezondheid 'historische' gebeurtenissen. Deze inschakelen scenario's, zoals:

- Waarschuwen wanneer een eigenschap bij waarschuwing/fout voor meer dan X minuten is. Controle van de voorwaarde voor een bepaalde tijdsperiode voorkomt waarschuwingen op tijdelijke omstandigheden. Bijvoorbeeld een waarschuwing als de status heeft van meer dan vijf minuten zijn waarschuwing kan worden vertaald (HealthState waarschuwing en nu - LastWarningTransitionTime == > 5 minuten).

- Alleen op de voorwaarden die zijn gewijzigd in de laatste waarschuwing X minuten. Als een rapport al bij fout vóór de opgegeven tijd is, kan worden genegeerd omdat deze al eerder werd gesignaleerd.

- Als een eigenschap is schakelen tussen waarschuwings- en foutberichten, bepalen hoe lang het is beschadigd (dat wil zeggen niet OK). Bijvoorbeeld een waarschuwing als de eigenschap niet in orde voor meer dan vijf minuten is kan worden vertaald (HealthState! = Ok en nu - LastOkTransitionTime > 5 minuten).

## <a name="example-report-and-evaluate-application-health"></a>Voorbeeld: Rapporteren en evalueren van de toepassingsstatus
In het volgende voorbeeld wordt een rapport gezondheid via PowerShell op de toepassing verzonden **fabric: / WordCount** uit de bron **MyWatchdog**. Het statusrapport bevat informatie over de eigenschap gezondheid "beschikbaarheid" in een foutstatus met oneindige TimeToLive. Vervolgens hiervoor wordt de toepassingsstatus, welke geeft samengevoegd gezondheid staat fouten en gebeurtenissen gemeld gezondheid in de lijst met gebeurtenissen voor de gezondheid.

```powershell
PS C:\> Send-ServiceFabricApplicationHealthReport –ApplicationName fabric:/WordCount –SourceId "MyWatchdog" –HealthProperty "Availability" –HealthState Error

PS C:\> Get-ServiceFabricApplicationHealth fabric:/WordCount


ApplicationName                 : fabric:/WordCount
AggregatedHealthState           : Error
UnhealthyEvaluations            :
                                  Error event: SourceId='MyWatchdog', Property='Availability'.

ServiceHealthStates             :
                                  ServiceName           : fabric:/WordCount/WordCountService
                                  AggregatedHealthState : Error

                                  ServiceName           : fabric:/WordCount/WordCountWebService
                                  AggregatedHealthState : Ok

DeployedApplicationHealthStates :
                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_0
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_2
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_3
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_4
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_1
                                  AggregatedHealthState : Ok

HealthEvents                    :
                                  SourceId              : System.CM
                                  Property              : State
                                  HealthState           : Ok
                                  SequenceNumber        : 360
                                  SentAt                : 3/22/2016 7:56:53 PM
                                  ReceivedAt            : 3/22/2016 7:56:53 PM
                                  TTL                   : Infinite
                                  Description           : Application has been created.
                                  RemoveWhenExpired     : False
                                  IsExpired             : False
                                  Transitions           : Error->Ok = 3/22/2016 7:56:53 PM, LastWarning = 1/1/0001 12:00:00 AM

                                  SourceId              : MyWatchdog
                                  Property              : Availability
                                  HealthState           : Error
                                  SequenceNumber        : 131032204762818013
                                  SentAt                : 3/23/2016 3:27:56 PM
                                  ReceivedAt            : 3/23/2016 3:27:56 PM
                                  TTL                   : Infinite
                                  Description           :
                                  RemoveWhenExpired     : False
                                  IsExpired             : False
                                  Transitions           : Ok->Error = 3/23/2016 3:27:56 PM, LastWarning = 1/1/0001 12:00:00 AM
```

## <a name="health-model-usage"></a>Gebruik de gezondheid-modellen
Het model van de gezondheid kan cloud services en het onderliggende weefsel Service platform kunnen worden geschaald, omdat de bepalingen en de gezondheid zijn verdeeld over de verschillende beeldschermen binnen het cluster.
Andere systemen hebben een centrale service op het clusterniveau van het waarin alle *potentieel* nuttige informatie uitgestoten door services worden geparseerd. Deze aanpak, kunnen de schaalbaarheid. Deze ook dat niet ze zeer specifieke gegevens om problemen en mogelijke problemen als dicht bij de oorzaak mogelijk achterhalen.

Het model van de gezondheid wordt intensief gebruikt voor bewaking en diagnose voor het evalueren van de cluster- en gezondheid en voor gecontroleerde upgrades. Andere gezondheidsgegevens gebruikt om automatische reparaties uitvoeren, cluster gezondheid geschiedenis bouwen en afgeven van signalen onder bepaalde voorwaarden.

## <a name="next-steps"></a>Volgende stappen
[Service Fabric-statusrapporten weergeven](service-fabric-view-entities-aggregated-health.md)

[Statusrapporten systeem gebruiken voor het oplossen van problemen](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)

[Het rapport en de service controleren](service-fabric-diagnostics-how-to-report-and-check-service-health.md)

[Aangepaste Service Fabric statusrapporten toevoegen](service-fabric-report-health.md)

[Controleren en diagnosticeren lokaal services](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Service Fabric toepassing upgrade](service-fabric-application-upgrade.md)
