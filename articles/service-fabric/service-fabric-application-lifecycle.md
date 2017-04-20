<properties
   pageTitle="Application lifecycle Service stof | Microsoft Azure"
   description="Beschrijving van ontwikkelen, implementeren, testen, bijwerken, onderhouden en Service Fabric-toepassingen verwijderen."
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor=""/>


<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/25/2016"
   ms.author="ryanwi"/>


# <a name="service-fabric-application-lifecycle"></a>Fabric service application lifecycle
Zoals met andere platforms, een toepassing op Azure Service Fabric meestal de volgende fasen doorloopt: ontwerp, ontwikkeling, testen, implementatie, upgraden, onderhoud en verwijdering. Fabric-service biedt eersteklas ondersteuning voor de volledige levenscyclus van cloud-toepassingen, van ontwikkeling tot implementatie, dagelijkse beheer en onderhoud tot uiteindelijk buiten gebruik stellen. Het service-model kan diverse verschillende functies kunnen onafhankelijk van elkaar deelnemen aan de levenscyclus van toepassing. Dit artikel bevat een overzicht van de API's en hoe ze worden gebruikt door de verschillende rollen in alle fases van de levenscyclus van de toepassing Fabric-Service.

## <a name="service-model-roles"></a>Functies van service model
De service model-rollen zijn:

- **Ontwikkelaar**: modulaire en algemene diensten die kunnen worden opnieuw gebruikt en gebruikt in verschillende toepassingen van dezelfde soort of verschillende typen ontwikkelt. Queue-service kan bijvoorbeeld worden gebruikt voor het maken van een ticketing (helpdesk) of een e-commerce-toepassing (winkelwagen).

- **Ontwikkelaar**: toepassingen door integratie van een verzameling services die voldoen aan bepaalde specifieke eisen of scenario's maakt. Bijvoorbeeld 'JSON Stateless Front-End Service' kan integreren in een e-commercewebsite 'Veiling Stateful Service' en 'Service wachtrij Stateful' om een auctioning oplossing te bouwen.

- **Beheerder van de toepassing**: beslissingen op de configuratie van de toepassing (de configuratieparameters van de sjabloon invullen), distributie (toewijzing aan de beschikbare middelen) en kwaliteit van de service. Een beheerder van de toepassing besluit bijvoorbeeld de taalinstelling (Engels voor de Verenigde Staten) of Japans voor Japan, voorbeeld van de toepassing. Een andere gedistribueerde toepassing kan verschillende instellingen hebben.

- **Operator**: toepassingen op basis van de configuratie van de toepassing en, zoals opgegeven door de beheerder van de toepassing implementeert. Bijvoorbeeld, een operator bepalingen en de toepassing implementeert en zorgt ervoor dat deze wordt uitgevoerd in Azure. Operatoren informatie over gezondheid en de prestaties van toepassingen beheren en bijhouden van de fysieke infrastructuur als nodig is.


## <a name="develop"></a>Ontwikkelen
1. Een *ontwikkelaar van* ontwikkelt verschillende soorten services met het programmeermodel [Betrouwbare actoren](service-fabric-reliable-actors-introduction.md) of [Betrouwbare diensten](service-fabric-reliable-services-introduction.md) .
2. Een *ontwikkelaar van* beschrijving declaratief van de ontwikkelde servicetypen in een manifestbestand van service die bestaat uit een of meer code-, configuratie- en pakketten.
3. Een *ontwikkelaar* maakt vervolgens een toepassing met behulp van verschillende typen.
4. Een *ontwikkelaar van toepassingen* wordt het toepassingstype in een toepassingsmanifest declaratief beschreven door te verwijzen naar de service manifesten van de samenstellende diensten en op de juiste manier overschrijven en verschillende instellingen voor de configuratie en implementatie van het onderdeel services van parameters voorzien.

Zie [aan de slag met betrouwbare actoren](service-fabric-reliable-actors-get-started.md) en [aan de slag met betrouwbare diensten](service-fabric-reliable-services-quick-start.md) voor voorbeelden.

## <a name="deploy"></a>Implementeren
1. Een *beheerder van de toepassing* van aanpassen het toepassingstype voor een specifieke toepassing worden geïmplementeerd met een cluster Service Fabric door te geven van de juiste parameters van het element **ApplicationType** in het manifest van de toepassing.

2. Een *operator* uploadt het toepassingspakket aan het archief van de afbeelding cluster met behulp van de [methode **CopyApplicationPackage** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.copyapplicationpackage.aspx) of de [ **Kopie-ServiceFabricApplicationPackage** -cmdlet](https://msdn.microsoft.com/library/azure/mt125905.aspx). Het programmapakket bevat het manifest van de toepassing en de collectie van servicepakketten. Fabric-service implementeert de toepassingen van het toepassingspakket opgeslagen in het archief van de installatiekopie, die een Azure blob-opslag of de Fabric Service-systeemservice kan zijn.

3. De *operator* bepalingen vervolgens het toepassingstype in het doelcluster van het pakket met geüploade toepassing met de [methode **ProvisionApplicationAsync** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync.aspx)of de [cmdlet **Register ServiceFabricApplicationType** ](https://msdn.microsoft.com/library/azure/mt125958.aspx)de [werking van de REST van de **bepaling van een toepassing** ](https://msdn.microsoft.com/library/azure/dn707672.aspx).

4. Na het inrichten van de toepassing start een *operator* de toepassing met de parameters die zijn opgegeven door de *beheerder van de toepassing* met de [methode **CreateApplicationAsync** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.createapplicationasync.aspx)de [cmdlet **New-ServiceFabricApplication** ](https://msdn.microsoft.com/library/azure/mt125913.aspx)of de [werking van de REST van de **Toepassing maken** ](https://msdn.microsoft.com/library/azure/dn707676.aspx).

5. Nadat de toepassing is geïmplementeerd, wordt een *operator* de [methode **CreateServiceAsync** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.servicemanagementclient.createserviceasync.aspx), de [cmdlet **New-ServiceFabricService** ](https://msdn.microsoft.com/library/azure/mt125874.aspx)of de [werking van de REST van de **Service maakt** ](https://msdn.microsoft.com/library/azure/dn707657.aspx) nieuwe exemplaren van de service voor de toepassing op basis van de beschikbare servicetypen.

6. De toepassing wordt nu uitgevoerd in de cluster-Service Fabric.

Zie [een toepassing distribueren](service-fabric-deploy-remove-applications.md) voor voorbeelden.

## <a name="test"></a>Test
1. Na de implementatie aan de plaatselijke ontwikkeling cluster of een cluster test, uitgevoerd een *ontwikkelaar van* het scenario ingebouwde failover-test met behulp van de klassen [**FailoverTestScenarioParameters**](https://msdn.microsoft.com/library/azure/system.fabric.testability.scenario.failovertestscenarioparameters.aspx) en [**FailoverTestScenario**](https://msdn.microsoft.com/library/azure/system.fabric.testability.scenario.failovertestscenario.aspx) of de [cmdlet **Invoke-ServiceFabricFailoverTestScenario** ](https://msdn.microsoft.com/library/azure/mt644783.aspx). De failover-Testscenario doorloopt een opgegeven service belangrijke overgangen en failover om ervoor te zorgen dat het nog steeds beschikbaar en actief is.

2. De *ontwikkelaar van* het ingebouwde chaos Testscenario met behulp van de klassen [**ChaosTestScenarioParameters**](https://msdn.microsoft.com/library/azure/system.fabric.testability.scenario.chaostestscenarioparameters.aspx) en [**ChaosTestScenario**](https://msdn.microsoft.com/library/azure/system.fabric.testability.scenario.chaostestscenario.aspx) of de [cmdlet **Invoke ServiceFabricChaosTestScenario** ](https://msdn.microsoft.com/library/azure/mt644774.aspx)vervolgens worden uitgevoerd. De chaos Testscenario induceert willekeurig meerdere knooppunt, pakket code en replica fouten in het cluster.

3. De *ontwikkelaar van* [tests Services communicatie](service-fabric-testability-scenarios-service-communication.md) door het maken van testscenario's primaire replica's om het cluster te verplaatsen.

Zie [Inleiding tot de fouttolerantie Analysis Service](service-fabric-testability-overview.md) voor meer informatie.

## <a name="upgrade"></a>Upgrade
1. Een *ontwikkelaar van* het onderdeel services van de geactiveerde toepassing bijgewerkt en/of corrigeert fouten en biedt een nieuwe versie van het servicemanifest.

2. Een *ontwikkelaar van toepassingen* overschrijft, configuratie en implementatie-instellingen van de services van consistente parameterizes en biedt een nieuwe versie van het manifest van de toepassing. Ontwikkelaar van de toepassing wordt vervolgens in de toepassing is uitgerust met de nieuwe versies van de manifesten service en biedt een nieuwe versie van het toepassingstype in een pakket met bijgewerkte toepassing.

3. Een *beheerder van de toepassing* is uitgerust met de nieuwe versie van het toepassingstype in de doeltoepassing met de juiste parameters bijwerken.

5. Een *operator* uploadt u het pakket met bijgewerkte toepassing aan het cluster afbeelding archief met behulp van de [methode **CopyApplicationPackage** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.copyapplicationpackage.aspx) of de [ **Kopie-ServiceFabricApplicationPackage** -cmdlet](https://msdn.microsoft.com/library/azure/mt125905.aspx). Het programmapakket bevat het manifest van de toepassing en de collectie van servicepakketten.

6. De nieuwe versie van de toepassing in het doelcluster bepalingen een *operator* met behulp van de [methode **ProvisionApplicationAsync** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync.aspx), de [cmdlet **Kassa ServiceFabricApplicationType** ](https://msdn.microsoft.com/library/azure/mt125958.aspx)of de [werking van de REST van de **bepaling van een toepassing** ](https://msdn.microsoft.com/library/azure/dn707672.aspx).

7. Een *operator* werkt de doeltoepassing naar de nieuwe versie met de [methode **UpgradeApplicationAsync** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.upgradeapplicationasync.aspx)of de [cmdlet **Start ServiceFabricApplicationUpgrade** ](https://msdn.microsoft.com/library/azure/mt125975.aspx)de [bewerking REST **upgrades uitvoeren voor toepassingen** ](https://msdn.microsoft.com/library/azure/dn707633.aspx).

8. Een *operator* controleert de voortgang van de upgrade met de [methode **GetApplicationUpgradeProgressAsync** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.getapplicationupgradeprogressasync.aspx)of de [cmdlet **Get-ServiceFabricApplicationUpgrade** ](https://msdn.microsoft.com/library/azure/mt125988.aspx)de [REST **Ophalen toepassing upgraden voortgang** bewerking](https://msdn.microsoft.com/library/azure/dn707631.aspx).

9. Indien nodig, wordt de *operator* wijzigt en de parameters van de huidige toepassing upgrade met de [methode **UpdateApplicationUpgradeAsync** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.updateapplicationupgradeasync.aspx)of de [cmdlet **Update-ServiceFabricApplicationUpgrade** ](https://msdn.microsoft.com/library/azure/mt126030.aspx)de [REST **Update toepassing upgraden** bewerking](https://msdn.microsoft.com/library/azure/mt628489.aspx)opnieuw toegepast.

10. Indien nodig, teruggedraaid de *operator* de huidige toepassing upgrade met de [methode **RollbackApplicationUpgradeAsync** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.rollbackapplicationupgradeasync.aspx)of de [cmdlet **Start ServiceFabricApplicationRollback** ](https://msdn.microsoft.com/library/azure/mt125833.aspx)de [bewerking **Ongedaan maken toepassing upgraden** REST](https://msdn.microsoft.com/library/azure/mt628494.aspx).

11. De doeltoepassing in het cluster wordt uitgevoerd zonder verlies van de beschikbaarheid van een van de samenstellende services Fabric service-upgrades.

Zie de [toepassing bijwerken zelfstudie](service-fabric-application-upgrade-tutorial.md) voor voorbeelden.

## <a name="maintain"></a>Onderhouden
1. Voor het besturingssysteem en patches, Service Fabric interfaces met de Azure infrastructuur te waarborgen van de beschikbaarheid van alle toepassingen in het cluster worden uitgevoerd.

2. Voor upgrades en patches voor de Service Fabric platform Fabric Service-upgrades zelf zonder verlies van de beschikbaarheid van een van de toepassingen die op het cluster worden uitgevoerd.

3. Een *beheerder van de toepassing* heeft goedgekeurd voor het toevoegen of verwijderen van knooppunten van een cluster na analyse van de capaciteit van historische gegevens over het gebruik en de verwachte toekomstige vraag.

4. Een *operator* wordt toegevoegd en verwijdert u de knooppunten die zijn opgegeven door de *beheerder van de toepassing*.

5. Wanneer nieuwe knooppunten worden toegevoegd aan of bestaande knooppunten uit het cluster zijn verwijderd, Fabric-Service automatisch laden-saldi actieve toepassingen op alle knooppunten in het cluster een optimale prestatie.

## <a name="remove"></a>Verwijderen
1. Een *operator* kunt u een specifiek exemplaar van een actieve service in het cluster verwijderen zonder de hele toepassing met de [methode **DeleteServiceAsync** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.servicemanagementclient.deleteserviceasync.aspx)of de [cmdlet **Verwijderen ServiceFabricService** ](https://msdn.microsoft.com/library/azure/mt126033.aspx)de [werking van de REST van de **Service verwijderen** ](https://msdn.microsoft.com/library/azure/dn707687.aspx).  

2. Een *operator* kan ook een toepassingsexemplaar en alle bijbehorende services met de [methode **DeleteApplicationAsync** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.deleteapplicationasync.aspx)of de [cmdlet **Verwijderen ServiceFabricApplication** ](https://msdn.microsoft.com/library/azure/mt125914.aspx)de [werking van de REST van de **Toepassing verwijderen** ](https://msdn.microsoft.com/library/azure/dn707651.aspx)verwijderen.

3. Nadat de toepassingen en services hebt gestopt, kan de *exploitant* van het toepassingstype met de [methode **UnprovisionApplicationAsync** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.unprovisionapplicationasync.aspx)of de [ **Registratie-ServiceFabricApplicationType** -cmdlet](https://msdn.microsoft.com/library/azure/mt125885.aspx)de [werking van de REST van de **inrichting een toepassing verwijderen** ](https://msdn.microsoft.com/library/azure/dn707671.aspx)inrichting. Hierbij het toepassingstype verwijdert niet het toepassingspakket van de ImageStore. Het toepassingspakket moet u handmatig verwijderen.

4. Een *operator* verwijdert het toepassingspakket van de ImageStore met behulp van de [methode **RemoveApplicationPackage** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.removeapplicationpackage.aspx) of de [cmdlet **Verwijderen ServiceFabricApplicationPackage** ](https://msdn.microsoft.com/library/azure/mt163532.aspx).

Zie [een toepassing distribueren](service-fabric-deploy-remove-applications.md) voor voorbeelden.

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over het ontwikkelen, Zie testen en beheren van Service Fabric toepassingen en services:

- [Betrouwbare actoren](service-fabric-reliable-actors-introduction.md)
- [Betrouwbare diensten](service-fabric-reliable-services-introduction.md)
- [Een toepassing](service-fabric-deploy-remove-applications.md)
- [Upgrade van de toepassing](service-fabric-application-upgrade.md)
- [Testbaarheid-overzicht](service-fabric-testability-overview.md)
- [REST-toepassing lifecycle monster](service-fabric-rest-based-application-lifecycle-sample.md)
