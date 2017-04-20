<properties
   pageTitle="Upgraden van een cluster Azure Service Fabric | Microsoft Azure"
   description="Upgrade Service Fabric code en/of configuratie van een cluster Service Fabric, inclusief het instellen van update clustermodus, certificaten, het toevoegen van poorten, OS patches, doet een upgrade uitgevoerd enzovoort. Wat kunt u verwachten wanneer de upgrades worden uitgevoerd?"
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/10/2016"
   ms.author="chackdan"/>


# <a name="upgrade-an-azure-service-fabric-cluster"></a>Upgraden van een cluster Azure Service Fabric

> [AZURE.SELECTOR]
- [Azure Cluster](service-fabric-cluster-upgrade.md)
- [Autonome Cluster](service-fabric-cluster-upgrade-windows-server.md)

Voor alle moderne systemen is ontwerpen voor kunnen de sleutel tot het succes van uw product op lange termijn te bereiken. Een Azure Service Fabric '-cluster is een bron die u bezit, maar ten dele wordt beheerd door Microsoft. In dit artikel wordt beschreven wat automatisch wordt beheerd en wat kunt u zelf.

## <a name="controlling-the-fabric-version-that-runs-on-your-cluster"></a>De fabric-versie die wordt uitgevoerd op een Cluster beheren

U kunt instellen dat uw cluster fabric automatische upgrades, ontvangen wanneer Microsoft een nieuwe versie is vrijgegeven of kies een gewenste cluster op ondersteunde fabric-versie te selecteren.

U doet dit door het instellen van de configuratie van de cluster 'upgradeMode' op de portal of het gebruik van Resource Manager op het moment van maken of hoger op een live-cluster 

>[AZURE.NOTE] Zorg ervoor dat het cluster waarop een versie van de ondersteunde fabric altijd behouden. Als en wanneer we de release van een nieuwe versie van de service fabric aankondigen, wordt de vorige versie gemarkeerd voor einde van ondersteuning na een minimum van 60 dagen na die datum. de nieuwe releases worden aangekondigd [op de blog service fabric-team](https://blogs.msdn.microsoft.com/azureservicefabric/ ). De nieuwe release is beschikbaar voor vervolgens kiezen. 

14 dagen vóór het verstrijken van de versie die het cluster wordt uitgevoerd, een gebeurtenis gezondheid gegenereerd die het cluster in de status van een waarschuwing wordt geplaatst. Het cluster blijft in een waarschuwing staat totdat u een naar een ondersteunde fabric-versie upgrade.


### <a name="setting-the-upgrade-mode-via-portal"></a>De upgrade-modus via de portal instellen 

Als u het cluster maakt, kunt u het cluster instellen op automatisch of handmatig.

![Create_Manualmode][Create_Manualmode]

U kunt instellen het cluster op automatisch of handmatig wanneer u op een live cluster met behulp van de ervaring beheren. 

#### <a name="upgrading-to-a-new-version-on-a-cluster-that-is-set-to-manual-mode-via-portal"></a>Upgraden naar een nieuwe versie op een cluster dat is ingesteld op handmatige modus via de portal.
 
Als u wilt upgraden naar een nieuwe versie, is hoeft u de versie in de vervolgkeuzelijst te selecteren en op te slaan. De upgrade Fabric wordt automatisch gestart. Het statusbeleid cluster (een combinatie van een knooppunt en de gezondheidsvoorschriften alle toepassingen in het cluster wordt uitgevoerd) aan zijn gehouden tijdens de upgradeprocedure.

Als het statusbeleid van het cluster niet wordt voldaan, is de upgrade teruggedraaid. Scroll naar beneden in dit document voor meer informatie over het instellen van deze aangepaste statusbeleid. 

Nadat de problemen die in het terugdraaien resulteerde is verholpen, moet u de upgrade opnieuw starten door de stappen hierboven te volgen.

![Manage_Automaticmode][Manage_Automaticmode]

### <a name="setting-the-upgrade-mode-via-a-resource-manager-template"></a>De upgrade via een sjabloon Resource Manager-modus instellen 

De configuratie van de "upgradeMode" toevoegen aan de definitie van de bron Microsoft.ServiceFabric/clusters en de 'clusterCodeVersion' ingesteld op een van de ondersteunde fabric versies zoals hieronder wordt weergegeven en vervolgens de sjabloon toepassen. De geldige waarden voor 'upgradeMode' zijn 'Handmatig' of 'Automatic'
 
![ARMUpgradeMode][ARMUpgradeMode]

#### <a name="upgrading-to-a-new-version-on-a-cluster-that-is-set-to-manual-mode-via-a-resource-manager-template"></a>Upgraden naar een nieuwe versie op een cluster dat is ingesteld op handmatige modus via een sjabloon Resource Manager.
 
Als het cluster in de handmatige modus is, om te upgraden naar een nieuwe versie, de 'clusterCodeVersion' te wijzigen naar een ondersteunde versie en deze implementeren. De implementatie van de sjabloon gang van de upgrade Fabric haalt gestarte automatisch. Het statusbeleid cluster (een combinatie van een knooppunt en de gezondheidsvoorschriften alle toepassingen in het cluster wordt uitgevoerd) aan zijn gehouden tijdens de upgradeprocedure.

Als het statusbeleid van het cluster niet wordt voldaan, is de upgrade teruggedraaid. Scroll naar beneden in dit document voor meer informatie over het instellen van deze aangepaste statusbeleid. 

Nadat de problemen die in het terugdraaien resulteerde is verholpen, moet u de upgrade opnieuw starten door de stappen hierboven te volgen.

### <a name="get-list-of-all-available-version-for-all-environments-for-a-given-subscription"></a>Lijst van alle beschikbare versie ophalen voor alle omgevingen voor een bepaald abonnement

Voer de volgende opdracht uit en ontvangt u een output vergelijkbaar met dit.

"supportExpiryUtc", vertelt je wanneer een bepaalde versie is verlopen of is verlopen. De meest recente versie beschikt niet over een geldige datum - deze heeft een waarde van "9999-12-31T23:59:59.9999999", wat betekent dat de uiterste datum is nog niet ingesteld.

```REST
GET https://<endpoint>/subscriptions/{{subscriptionId}}/providers/Microsoft.ServiceFabric/clusterVersions?api-version= 2016-09-01

Output:
{
                  "value": [
                    {
                      "id": "subscriptions/35349203-a0b3-405e-8a23-9f1450984307/providers/Microsoft.ServiceFabric/environments/Windows/clusterVersions/5.0.1427.9490",
                      "name": "5.0.1427.9490",
                      "type": "Microsoft.ServiceFabric/environments/clusterVersions",
                      "properties": {
                        "codeVersion": "5.0.1427.9490",
                        "supportExpiryUtc": "2016-11-26T23:59:59.9999999",
                        "environment": "Windows"
                      }
                    },
                    {
                      "id": "subscriptions/35349203-a0b3-405e-8a23-9f1450984307/providers/Microsoft.ServiceFabric/environments/Windows/clusterVersions/4.0.1427.9490",
                      "name": "5.1.1427.9490",
                      "type": " Microsoft.ServiceFabric/environments/clusterVersions",
                      "properties": {
                        "codeVersion": "5.1.1427.9490",
                        "supportExpiryUtc": "9999-12-31T23:59:59.9999999",
                        "environment": "Windows"
                      }
                    },
                    {
                      "id": "subscriptions/35349203-a0b3-405e-8a23-9f1450984307/providers/Microsoft.ServiceFabric/environments/Windows/clusterVersions/4.4.1427.9490",
                      "name": "4.4.1427.9490",
                      "type": " Microsoft.ServiceFabric/environments/clusterVersions",
                      "properties": {
                        "codeVersion": "4.4.1427.9490",
                        "supportExpiryUtc": "9999-12-31T23:59:59.9999999",
                        "environment": "Linux"
                      }
                    }
                  ]
                }


```

## <a name="fabric-upgrade-behavior-when-the-cluster-upgrade-mode-is-automatic"></a>Fabric upgrade gedrag wanneer het cluster een Upgrade-modus automatisch is

Microsoft onderhoudt de fabric-code en de configuratie die wordt uitgevoerd in een cluster Azure. We voeren automatisch gecontroleerde upgrades van de software in als nodig is. Upgrades mogelijk code of configuratie. Om ervoor te zorgen dat uw toepassing geen impact of overlast door deze upgrades suffers, uitvoeren we de upgrades in de volgende fasen:

### <a name="phase-1-an-upgrade-is-performed-by-using-all-cluster-health-policies"></a>Fase 1: Een upgrade is uitgevoerd met alle cluster statusbeleid

Tijdens deze fase worden de upgrades upgrade domeinen tegelijk gaan en de toepassingen die actief waren in het cluster worden uitgevoerd zonder enige uitvaltijd. Het statusbeleid cluster (een combinatie van een knooppunt en de gezondheidsvoorschriften alle toepassingen in het cluster wordt uitgevoerd) aan zijn gehouden tijdens de upgradeprocedure.

Als het statusbeleid van het cluster niet wordt voldaan, is de upgrade teruggedraaid. Een e-mailbericht wordt verzonden naar de eigenaar van het abonnement. Het e-mailbericht bevat de volgende informatie:

- Melding dat we hadden terug te draaien het upgraden van een cluster.
- Voorgestelde corrigerende acties, indien van toepassing.
- Het aantal dagen (n) totdat we fase 2 uitvoeren.

We proberen uit te voeren de upgrade dezelfde een paar keer in het geval kan de eventuele upgrades niet omwille van de infrastructuur. We gaan na de n dagen vanaf de datum waarop die de e-mail is verzonden naar fase 2.

Als het cluster statusbeleid wordt voldaan, wordt de upgrade als geslaagd beschouwd en als voltooid zijn gemarkeerd. Dit kan gebeuren tijdens de upgrade of een van de upgrade herhalingen in deze fase. Er is geen e-mailbevestiging van een geslaagde uitvoering. Dit is om te voorkomen dat u te veel e-mails--ontvangen van een e-mailbericht moet worden gezien als een uitzondering op de normale verzenden. We verwachten dat de meeste van de cluster-upgrades te kunnen uitvoeren zonder invloed op de beschikbaarheid van uw toepassing.

### <a name="phase-2-an-upgrade-is-performed-by-using-default-health-policies-only"></a>Fase 2: Een upgrade is uitgevoerd met behulp van statusbeleid alleen standaard

Het statusbeleid in deze fase worden ingesteld op zodanige wijze dat het nummer van de toepassingen die aan het begin van de upgrade in orde zijn blijft hetzelfde voor de duur van het upgradeproces. In fase 1, de fase 2 upgrades upgrade domeinen tegelijk gaan en de toepassingen die actief waren in het cluster worden uitgevoerd zonder enige uitvaltijd. Het statusbeleid cluster (een combinatie van een knooppunt en de gezondheidsvoorschriften alle toepassingen die worden uitgevoerd in het cluster) voor de duur van de upgrade wordt opgevolgd.

Als het statusbeleid cluster in feite niet wordt voldaan, is de upgrade teruggedraaid. Een e-mailbericht wordt verzonden naar de eigenaar van het abonnement. Het e-mailbericht bevat de volgende informatie:

- Melding dat we hadden terug te draaien het upgraden van een cluster.
- Voorgestelde corrigerende acties, indien van toepassing.
- Het aantal dagen (n) totdat we uitvoeren van fase 3.

We proberen uit te voeren de upgrade dezelfde een paar keer in het geval kan de eventuele upgrades niet omwille van de infrastructuur. Een herinnering een paar dagen voordat de n dagen van zijn verzonden. Na de n dagen vanaf de datum waarop die het e-mailbericht is verzonden, gaan we in fase 3. De e-mailberichten we u in fase 2 sturen serieus moeten worden genomen en herstelmaatregelen moeten worden genomen.

Als het cluster statusbeleid wordt voldaan, wordt de upgrade als geslaagd beschouwd en als voltooid zijn gemarkeerd. Dit kan gebeuren tijdens de upgrade of een van de upgrade herhalingen in deze fase. Er is geen e-mailbevestiging van een geslaagde uitvoering.

### <a name="phase-3-an-upgrade-is-performed-by-using-aggressive-health-policies"></a>Fase 3: Een upgrade is uitgevoerd met behulp van agressieve statusbeleid

Deze statusbeleid in deze fase zijn gericht op de voltooiing van de upgrade in plaats van de gezondheid van de toepassingen. Zeer weinig cluster upgrades uiteindelijk in deze fase. Als het cluster naar deze fase wordt, is er een goede kans dat uw toepassing raakt beschadigd en/of beschikbaarheid verliest.

Net als bij de andere twee fasen, upgrades van fase 3 gaan domein voor een upgrade op een tijdstip.

Als het statusbeleid van het cluster niet wordt voldaan, is de upgrade teruggedraaid. We proberen uit te voeren de upgrade dezelfde een paar keer in het geval kan de eventuele upgrades niet omwille van de infrastructuur. Daarna is het cluster vastgemaakt, zodat deze niet langer ondersteuning en/of upgrades.

Een e-mail met deze informatie wordt verzonden naar de eigenaar van het abonnement, samen met de herstelmaatregelen. We verwachten niet dat alle clusters in een staat waar de fase 3 is mislukt.

Als het cluster statusbeleid wordt voldaan, wordt de upgrade als geslaagd beschouwd en als voltooid zijn gemarkeerd. Dit kan gebeuren tijdens de upgrade of een van de upgrade herhalingen in deze fase. Er is geen e-mailbevestiging van een geslaagde uitvoering.

## <a name="cluster-configurations-that-you-control"></a>Clusterconfiguraties die u beheren

Naast de mogelijkheid om het cluster een upgrade modus, Hier vindt u de configuraties die u in een live-cluster wijzigen kunt.

### <a name="certificates"></a>Certificaten

U kunt toevoegen of verwijderen van certificaten voor het cluster en clientcomputers via het portal eenvoudig. Verwijzen naar [Dit document voor gedetailleerde instructies](service-fabric-cluster-security-update-certs-azure.md)

![Schermafdruk van vingerafdrukken certificaat in Azure portal.][CertificateUpgrade]


### <a name="application-ports"></a>Poorten

U kunt poorten wijzigen door de Load Balancer resource-eigenschappen die gekoppeld aan het knooppunttype zijn te wijzigen. U kunt de portal of kunt u PowerShell Resource Manager rechtstreeks.

U opent een nieuwe poort op alle VMs in een knooppunttype, het volgende doen:

1. Voeg een nieuwe sonde aan de juiste taakverdeling.

    Als u het cluster hebt geïmplementeerd via de portal, de netwerktaakverdeling zijn met de naam "LB-naam van de Resource-groep-NodeTypename", een voor elk knooppunttype. Aangezien de load balancer namen alleen uniek binnen een groep zijn, is het beste als u ze onder een bepaalde resourcegroep.

    ![Schermafdruk van een sonde toe te voegen aan een taakverdeling in de portal.][AddingProbes]

2. Een nieuwe regel toevoegen aan de taakverdeling.

    Een nieuwe regel toevoegen aan dezelfde verdeling van de belasting met behulp van de sonde die u in de vorige stap hebt gemaakt.

    ![Een nieuwe regel toevoegen aan een taakverdeling in de portal.][AddingLBRules]


### <a name="placement-properties"></a>Positie-eigenschappen

Voor elk type knooppunt, kunt u de plaatsing van de aangepaste eigenschappen die u wilt gebruiken in uw toepassingen toevoegen. NodeType is een standaardinstelling voor de eigenschap die u gebruiken kunt zonder het expliciet toe te voegen.

>[AZURE.NOTE] Raadpleeg de sectie 'plaatsing beperkingen en eigenschappen van het knooppunt"in het Document Service Fabric Cluster Resource Manager op [Met een beschrijving van uw Cluster](service-fabric-cluster-resource-manager-cluster-description.md)voor meer informatie over het gebruik van plaatsing beperkingen, eigenschappen van het knooppunt en hoe ze kunnen worden gedefinieerd.

### <a name="capacity-metrics"></a>Capaciteit metrics

Voor elk type knooppunt, kunt u aangepaste capaciteit maatstaven die u wilt gebruiken in uw toepassingen te laden rapport toevoegen. Voor meer informatie over het gebruik van capaciteit maatstelsel rapport laden, raadpleegt u de Service Fabric Cluster Resource Manager documenten op [Met een beschrijving van uw Cluster](service-fabric-cluster-resource-manager-cluster-description.md) en [maatstaven en laden](service-fabric-cluster-resource-manager-metrics.md).

### <a name="fabric-upgrade-settings---health-polices"></a>Upgrade fabric-instellingen - gezondheid beleid

U kunt aangepaste gezondheid voor fabric upgrade beleid. Als u het cluster hebt ingesteld op automatische fabric upgrades, krijgen deze beleid toegepast op de fase-1 van de fabric automatische upgrades.
Als u het cluster voor handmatige fabric upgrades, krijgen deze beleidsregels telkens wanneer die u een nieuwe versie die het systeem te ere van de upgrade van de stof in het cluster selecteert toegepast. Als u het beleid niet boven, worden de standaardwaarden gebruikt.

U kunt de aangepaste statusbeleid opgeven of de huidige instellingen onder het blad 'upgrade fabric' bekijken door de geavanceerde instellingen voor de upgrade. Bekijk hoe de volgende afbeelding. 

![Aangepaste statusbeleid beheren][HealthPolices]

### <a name="customize-fabric-settings-for-your-cluster"></a>Instellingen voor uw cluster Fabric aanpassen

Zie [instellingen van fabric fabric cluster-service](service-fabric-cluster-fabric-settings.md) op wat en hoe u ze kunt aanpassen.

### <a name="os-patches-on-the-vms-that-make-up-the-cluster"></a>OS patches op de VMs die het cluster tezamen

Deze mogelijkheid is gepland voor de toekomst als een geautomatiseerde functie. Maar op dit moment bent u verantwoordelijk voor uw VMs patch. Doet u dit één VM tegelijk, zodat u doen niet af op meer dan één voor één.

### <a name="os-upgrades-on-the-vms-that-make-up-the-cluster"></a>Upgrades voor het besturingssysteem op de VMs die het cluster tezamen

Als u de installatiekopie van het besturingssysteem op de virtuele machines van het cluster upgraden moet, doet u het een VM tegelijk. U bent zelf verantwoordelijk voor deze upgrade--er is momenteel geen automatisering.

## <a name="next-steps"></a>Volgende stappen
- Informatie over het aanpassen van sommige van de [instellingen van fabric fabric cluster-service](service-fabric-cluster-fabric-settings.md)
- Meer informatie over het [schalen van het cluster in-en uitchecken](service-fabric-cluster-scale-up-down.md)
- Meer informatie over [toepassingsupgrades](service-fabric-application-upgrade.md)

<!--Image references-->
[CertificateUpgrade]: ./media/service-fabric-cluster-upgrade/CertificateUpgrade2.png
[AddingProbes]: ./media/service-fabric-cluster-upgrade/addingProbes2.PNG
[AddingLBRules]: ./media/service-fabric-cluster-upgrade/addingLBRules.png
[HealthPolices]: ./media/service-fabric-cluster-upgrade/Manage_AutomodeWadvSettings.PNG
[ARMUpgradeMode]: ./media/service-fabric-cluster-upgrade/ARMUpgradeMode.PNG
[Create_Manualmode]: ./media/service-fabric-cluster-upgrade/Create_Manualmode.PNG
[Manage_Automaticmode]: ./media/service-fabric-cluster-upgrade/Manage_Automaticmode.PNG