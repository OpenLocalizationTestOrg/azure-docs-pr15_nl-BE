<properties
   pageTitle="Azure Service Fabric-clusters maken voor Windows Server en Linux | Microsoft Azure"
   description="Service Fabric-clusters worden uitgevoerd op Windows Server en Linux, wat betekent dat u zult kunt implementeren en Service Fabric hosttoepassingen overal kunt u Windows Server of Linux uitvoeren."
   services="service-fabric"
   documentationCenter=".net"
   authors="Chackdan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/22/2016"
   ms.author="chackdan"/>

# <a name="create-service-fabric-clusters-on-windows-server-or-linux"></a>Service Fabric-clusters maken op Windows Server of Linux

Azure Service Fabric kan voor het maken van clusters of computers met Windows Server en Linux VMs Fabric-Service. Dit betekent dat u kunt implementeren en Service Fabric-toepassingen uitvoeren in elke omgeving waar u hebt een set Windows Server- of Linux-computers die met elkaar zijn verbonden, worden deze in lokalen, Microsoft Azure of met een wolk provider.

##<a name="create-service-fabric-clusters-on-azure"></a>Service Fabric-clusters maken op Azure

Maken van een cluster op Azure is gedaan, hetzij via een Model met sjabloon of de Azure portal. Lees [een cluster Service weefsel met een Resource Manager-sjabloon maken](service-fabric-cluster-creation-via-arm.md) of [een Service Fabric '-cluster via de portal Azure maken](service-fabric-cluster-creation-via-portal.md) voor meer informatie.

## <a name="supported-operating-systems-for-clusters-on-azure"></a>Ondersteunde besturingssystemen voor clusters op Azure

U zijn kunt clusters maken op VMs met deze besturingssystemen:

* Windows Server 2012 R2
* Windows Server 2016 (nadat het is aangekondigd als algemeen beschikbaar)
* 16.04 van Ubuntu Linux (in public preview) 


##<a name="create-service-fabric-standalone-clusters-on-premise-or-with-any-cloud-provider"></a>Lokale of met een wolk provider Service Fabric zelfstandige clusters maken

Fabric-service biedt een installatiepakket maakt zelfstandige Service Fabric clusters van gebouwen of op een wolk provider

Lees voor meer informatie over het instellen van een zelfstandige service fabric-clusters in Windows Server [Service Fabric cluster maken voor Windows Server](service-fabric-cluster-creation-for-windows-server.md)

### <a name="any-cloud-deployments-vs-on-premises-deployments"></a>Alle implementaties cloud versus implementaties voor gebouwen
Het proces voor het maken van een cluster Service Fabric op ruimten is vergelijkbaar met het proces van het maken van een cluster op een wolk van uw keuze met een set van VMs. De eerste stappen voor het inrichten van de VMs worden beheerst door de cloud provider of het milieu op de ruimten die u gebruikt. Wanneer u een set van VMs met netwerkverbindingen tussen deze ingeschakeld hebt, vervolgens de stappen voor het instellen van het pakket Service Fabric, bewerkt u de clusterinstellingen en het maken van het cluster worden uitgevoerd en scripts voor zijn identiek. Dit zorgt ervoor dat uw kennis en ervaring van besturingssysteem en Service Fabric clusters beheren effecten wanneer u kiest voor nieuwe hosting omgevingen.

### <a name="benefits-of-creating-standalone-service-fabric-clusters"></a>Voordelen van het maken van clusters van zelfstandige Service Fabric
* U bent vrij om te kiezen een wolk provider als host voor uw cluster.
* Service Fabric toepassingen, eenmaal geschreven, kunnen worden uitgevoerd in omgevingen met meerdere hosting met minimale tot geen wijzigingen.
* Kennis van het bouwen van Service-Fabric weerspiegelt uit een hosting-omgeving naar de andere.
* Operationele ervaring met het uitvoeren en het beheren van Service Fabric clusters uitvoert van de ene omgeving naar de andere.
* Brede klant bereiken is niet-gebonden door hosting-beperkingen van de omgeving.
* Een extra niveau van betrouwbaarheid en beveiliging tegen storingen van wijdverbreide bestaat omdat u de services verplaatsen kunt naar een andere werkomgeving als een data center of cloud provider een stroomonderbreking heeft.

## <a name="supported-operating-systems-for-standalone-clusters"></a>Ondersteunde besturingssystemen voor zelfstandige clusters
U zijn kunt clusters maken op VMs of op computers waarop deze besturingssystemen:

* Windows Server 2012 R2
* Windows Server 2016 (nadat het is aangekondigd als algemeen beschikbaar)
* Linux (binnenkort beschikbaar)

## <a name="advantages-of-service-fabric-clusters-on-azure-over-standalone-service-fabric-clusters-created-on-premises"></a>Voordelen van Service Fabric clusters op Azure over zelfstandige die service Fabric clusters gemaakt in gebouwen

Service Fabric clusters op Azure biedt voordelen ten opzichte van de gebouwen op de optie, dus als er geen specifieke behoeften aan waar het uitvoeren van de clusters, en wij bevelen u deze uitvoeren op Azure. Op Azure zorgen we voor integratie met andere Azure functies en services, waardoor bewerkingen en beheer van het cluster gemakkelijker en betrouwbaarder.

* **Azure portal:** Azure portal kunt u gemakkelijk maken en beheren van clusters.

* **Azure Resource Manager:** Gebruik van Azure Resource Manager kan eenvoudig beheer van alle resources die worden gebruikt door het cluster als een eenheid en vereenvoudigt de kosten volgen en facturering.
* **Service Fabric '-Cluster als een Azure-bron** Een cluster Service Fabric is een bron van de ARM, zodat u kunt het model zoals andere ARM-bronnen in Azure.
* **Integratie met Azure infrastructuur** Coördinaten Fabric-service met de onderliggende Azure infrastructuur voor het besturingssysteem, netwerk en andere upgrades, beschikbaarheid en betrouwbaarheid van uw toepassingen te verbeteren.  
* **Diagnostiek:** Wij bieden integratie met Azure diagnose en Analytics logboek op Azure.
* **Automatisch schalen:** Wij bieden ingebouwde functionaliteit van automatisch schalen door virtuele Machine schaal-sets voor clusters op Azure. In de lokale en andere cloud-omgevingen hebt u om uw eigen automatisch schalen functie of handmatig met behulp van de API's van die stof Service beschikbaar worden gesteld voor clusters schaal schaal te bouwen.

## <a name="next-steps"></a>Volgende stappen
Maak een cluster op computers met Windows Server- of VMs: [maken van het cluster Service Fabric voor Windows Server](service-fabric-cluster-creation-for-windows-server.md)

Maak een cluster op computers met Linux of een VMs: [Service Fabric op Linux](service-fabric-linux-overview.md)
