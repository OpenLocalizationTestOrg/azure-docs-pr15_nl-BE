<properties
    pageTitle="Abonnement op Microsoft Azure en grenzen van de Service, quota's en beperkingen"
    description="Bevat een lijst met veelvoorkomende Azure abonnement en grenzen van de service, quota en beperkingen. Dit omvat informatie over hoe te verruimen en maximale waarden."
    services=""
    documentationCenter=""
    authors="rothja"
    manager="jeffreyg"
    editor=""
    tags="billing"
    />

<tags
    ms.service="billing"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/29/2016"
    ms.author="btardif"/>

# <a name="azure-subscription-and-service-limits-quotas-and-constraints"></a>Azure abonnement en grenzen van de service, quota's en beperkingen

## <a name="overview"></a>Overzicht

Dit document bevat enkele van de meest voorkomende Microsoft Azure grenzen. Dit omvat momenteel niet alle Azure services. Na verloop van tijd worden deze limieten uitgebreid en bijgewerkt ter dekking van het platform.

Ga naar [Azure prijzen-overzicht](https://azure.microsoft.com/pricing/) voor meer informatie over Azure prijzen. Er, raming van de kosten met behulp van de [Rekenmachine prijzen](https://azure.microsoft.com/pricing/calculator/) of Ga naar de pagina met prijzen voor een service (bijvoorbeeld [Windows VMs](https://azure.microsoft.com/pricing/details/virtual-machines/#Windows)).

> [AZURE.NOTE] Als u wilt dat de begrenzing van de **Standaard limiet**verhogen, kunt u [een verzoek voor ondersteuning van online klant gratis openen](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). De grenzen kunnen niet boven de **Maximale** waarde in de onderstaande tabellen worden verhoogd. Als er geen **Maximumlimiet** kolom, vervolgens heeft de opgegeven resource geen verstelbare grenzen.

## <a name="limits-and-the-azure-resource-manager"></a>Limieten en de Azure Resource Manager

Nu is het mogelijk meerdere Azure resources in een enkele Azure resourcegroep combineren. Als u resourcegroepen, worden op een regionaal niveau met Azure Resource Manager limieten die eenmaal globaal zijn beheerd. Zie voor meer informatie over resourcegroepen Azure [Azure Resource Manager-overzicht](azure-resource-manager/resource-group-overview.md).

In de onderstaande grenzen is een nieuwe tabel toegevoegd aan eventuele verschillen in beperkingen bij het gebruik van de bronnenbeheerder Azure. Er is bijvoorbeeld een **Abonnementen** -tabel en een tabel **Abonnementen - Azure Resource Manager** . Wanneer u een limiet geldt voor beide scenario's, wordt het alleen weergegeven in de eerste tabel. Tenzij anders aangegeven limieten gelden voor alle alle regio's.

> [AZURE.NOTE] Het is belangrijk om te benadrukken dat quota voor resources in resourcegroepen Azure per regio toegankelijk voor uw abonnement zijn, en niet per abonnement zijn de quota service management. Laten we core quota gebruiken als voorbeeld. Als u nodig hebt voor het aanvragen van een verhoging van de quota met ondersteuning voor cores, moet u bepalen hoeveel kernen die u wilt gebruiken in welke regio's, en een specifiek verzoek voor een resourcegroep Azure core quota voor de bedragen en de regio's die u wilt. Dus, als u 30 cores in West-Europa gebruiken moet voor het uitvoeren van de toepassing; u moet specifiek vragen 30 cores in West-Europa. Maar u hebt een core quota verhogen in een andere regio--alleen West-Europa heeft het quotum 30 core.
<!-- -->
U kunt hierdoor handig rekening te houden met het bepalen van uw quota Azure resourcegroep moeten worden voor de werkbelasting in elk gebied één en vragen dat bedrag in elke regio waarin u van plan implementeren te bent. Zie [het oplossen van problemen bij de implementatie](./resource-manager-common-deployment-errors.md) voor meer informatie uw huidige quota voor bepaalde regio's te ontdekken.

## <a name="service-specific-limits"></a>Service-specifieke limieten

- [Active Directory](#active-directory-limits)
- [API-beheer](#api-management-limits)
- [App-Service](#app-service-limits)
- [Application Gateway](#application-gateway-limits)
- [Inzichten van toepassing](#application-insights-limits)
- [Automatisering](#automation-limits)
- [Cache bestand Vgx Azure.](#azure-redis-cache-limits)
- [Azure RemoteApp](#azure-remoteapp-limits)
- [Back-up](#backup-limits)
- [Batch](#batch-limits)
- [BizTalk-Services](#biztalk-services-limits)
- [CDN](#cdn-limits)
- [Cloud Services](#cloud-services-limits)
- [Data Factory](#data-factory-limits)
- [Gegevens Lake Analytics](#data-lake-analytics-limits)
- [DNS](#dns-limits)
- [DocumentDB](#documentdb-limits)
- [Gebeurtenis Hubs](#event-hubs-limits)
- [IoT Hub](#iot-hub-limits)
- [Sleutel kluis](#key-vault-limits)
- [Mediaservices](#media-services-limits)
- [Betrokkenheid bij de mobiele](#mobile-engagement-limits)
- [Mobiele Services](#mobile-services-limits)
- [Monitoring](#monitoring-limits)
- [Meerledige verificatie](#multi-factor-authentication)
- [Netwerken](#networking-limits)
- [Hub berichtgevingsservice](#notification-hub-service-limits)
- [Operationele inzichten](#operational-insights-limits)
- [Resourcegroep](#resource-group-limits)
- [Scheduler](#scheduler-limits)
- [Zoeken](#search-limits)
- [Bus service](#service-bus-limits)
- [Site herstellen](#site-recovery-limits)
- [SQL-Database](#sql-database-limits)
- [Opslag](#storage-limits)
- [StorSimple systeem](#storsimple-system-limits)
- [Stream Analytics](#stream-analytics-limits)
- [Abonnement](#subscription-limits)
- [Beheer van netwerkverkeer](#traffic-manager-limits)
- [Virtuele Machines](#virtual-machines-limits)
- [Virtuele Machine schaal Sets](#virtual-machine-scale-sets-limits)


### <a name="subscription-limits"></a>Abonnementen
#### <a name="subscription-limits"></a>Abonnementen
[AZURE.INCLUDE [azure-subscription-limits](../includes/azure-subscription-limits.md)]

#### <a name="subscription-limits---azure-resource-manager"></a>Abonnementen - Azure Resource Manager

De volgende beperkingen gelden bij het gebruik van de bronnenbeheerder Azure en Azure resourcegroepen. Grenzen die niet met Azure Resource Manager hebt gewijzigd staan niet hieronder. Zie de vorige tabel voor deze maxima.

Zie voor meer informatie over het afhandelen van limieten voor verzoeken van Resource Manager [Resource Manager bandbreedtebeperking aanvragen](resource-manager-request-limits.md).

[AZURE.INCLUDE [azure-subscription-limits-azure-resource-manager](../includes/azure-subscription-limits-azure-resource-manager.md)]


### <a name="resource-group-limits"></a>Grenzen van de resourcegroep

[AZURE.INCLUDE [azure-resource-groups-limits](../includes/azure-resource-groups-limits.md)]

### <a name="virtual-machines-limits"></a>Limieten voor virtuele Machines
#### <a name="virtual-machine-limits"></a>Limieten voor virtuele Machine
[AZURE.INCLUDE [azure-virtual-machines-limits](../includes/azure-virtual-machines-limits.md)]


#### <a name="virtual-machines-limits---azure-resource-manager"></a>Limieten voor virtuele Machines - Azure Resource Manager

De volgende beperkingen gelden bij het gebruik van de bronnenbeheerder Azure en Azure resourcegroepen. Grenzen die niet met Azure Resource Manager hebt gewijzigd staan niet hieronder. Zie de vorige tabel voor deze maxima.

[AZURE.INCLUDE [azure-virtual-machines-limits-azure-resource-manager](../includes/azure-virtual-machines-limits-azure-resource-manager.md)]

### <a name="virtual-machine-scale-sets-limits"></a>Limieten voor virtuele Machine schaal Sets

[AZURE.INCLUDE [virtual-machine-scale-sets-limits](../includes/azure-virtual-machine-scale-sets-limits.md)]

### <a name="networking-limits"></a>Limieten voor netwerken

[AZURE.INCLUDE [expressroute-limits](../includes/expressroute-limits.md)]

#### <a name="networking-limits"></a>Limieten voor netwerken
[AZURE.INCLUDE [azure-virtual-network-limits](../includes/azure-virtual-network-limits.md)]

#### <a name="application-gateway-limits"></a>Toepassingsgateway beperkt

[AZURE.INCLUDE [application-gateway-limits](../includes/application-gateway-limits.md)]

#### <a name="traffic-manager-limits"></a>Beheer van netwerkverkeer beperkt

[AZURE.INCLUDE [traffic-manager-limits](../includes/traffic-manager-limits.md)]

#### <a name="dns-limits"></a>DNS-beperkingen

[AZURE.INCLUDE [dns-limits](../includes/dns-limits.md)]

### <a name="storage-limits"></a>Opslagbeperkingen

Zie voor meer informatie over limieten voor opslag, [Azure opslag schaalbaarheid en prestaties, doelen](../articles/storage/storage-scalability-targets.md).

#### <a name="storage-service-limits"></a>Opslagbeperkingen Service

[AZURE.INCLUDE [azure-storage-limits](../includes/azure-storage-limits.md)]

#### <a name="virtual-machine-disk-limits"></a>Limieten voor virtuele Machine schijf

[AZURE.INCLUDE [azure-storage-limits-vm-disks](../includes/azure-storage-limits-vm-disks.md)]

Zie [de grootte van de virtuele machine](../articles/virtual-machines/virtual-machines-linux-sizes.md) voor meer informatie.

**Standaardopslag accounts**

[AZURE.INCLUDE [azure-storage-limits-vm-disks-standard](../includes/azure-storage-limits-vm-disks-standard.md)]

**Premium opslag rekeningen**

[AZURE.INCLUDE [azure-storage-limits-vm-disks-premium](../includes/azure-storage-limits-vm-disks-premium.md)]

#### <a name="storage-resource-provider-limits"></a>Opslagbeperkingen Resource Provider

[AZURE.INCLUDE [azure-storage-limits-azure-resource-manager](../includes/azure-storage-limits-azure-resource-manager.md)]


### <a name="cloud-services-limits"></a>Limieten voor cloud Services

[AZURE.INCLUDE [azure-cloud-services-limits](../includes/azure-cloud-services-limits.md)]


### <a name="app-service-limits"></a>Beperkt App-Service
De volgende App Service grenzen zijn grenzen voor Web Apps, mobiele Apps, API Apps en Apps van logica.

[AZURE.INCLUDE [azure-websites-limits](../includes/azure-websites-limits.md)]

### <a name="scheduler-limits"></a>Scheduler-limieten

[AZURE.INCLUDE [scheduler-limits-table](../includes/scheduler-limits-table.md)]

### <a name="batch-limits"></a>Batch-limieten

[AZURE.INCLUDE [azure-batch-limits](../includes/azure-batch-limits.md)]

###<a name="biztalk-services-limits"></a>Grenzen van de BizTalk-Services
De volgende tabel ziet de limieten voor Azure BizTalk-Services.

[AZURE.INCLUDE [biztalk-services-service-limits](../includes/biztalk-services-service-limits.md)]


### <a name="documentdb-limits"></a>Limieten voor DocumentDB

[AZURE.INCLUDE [azure-documentdb-limits](../includes/azure-documentdb-limits.md)]

Quota weergegeven met een sterretje (*) [kan worden aangepast door contact opnemen met ondersteuning van Azure](./documentdb/documentdb-increase-limits.md).

### <a name="mobile-engagement-limits"></a>Mobile Engagement limieten

[AZURE.INCLUDE [azure-mobile-engagement-limits](../includes/azure-mobile-engagement-limits.md)]


### <a name="search-limits"></a>Grenzen zoeken

Prijzen lagen bepalen de capaciteit en de grenzen van de zoekservice. Lagen zijn:

- *Gratis* meerdere huurder service gedeeld met andere Azure abonnees, die bestemd zijn voor de evaluatie en van kleine ontwikkelingsprojecten.
- *Basic* biedt speciale computerbronnen voor productie werkbelasting op kleinere schaal met drie replica's voor hoge beschikbaarheid query werkbelasting.
- *Standaard (S1, S2, S3, S3 met hoge dichtheid)* is voor een grotere werklast voor productie. Meerdere niveaus bestaan binnen de standaard laag, zodat u de configuratie van een bron voor specifieke scenario's kunt.

**Limieten per abonnement**

[AZURE.INCLUDE [azure-search-limits-per-subscription](../includes/azure-search-limits-per-subscription.md)]

**Limieten per search-service**

[AZURE.INCLUDE [azure-search-limits-per-service](../includes/azure-search-limits-per-service.md)]

Zie voor meer gedetailleerde informatie over andere grenzen, met inbegrip van de grootte van het document query's per seconde, sleutels, aanvragen en antwoorden [in Azure Search Service limieten](search/search-limits-quotas-capacity.md).

### <a name="media-services-limits"></a>Limieten van Media Services

[AZURE.INCLUDE [azure-mediaservices-limits](../includes/azure-mediaservices-limits.md)]

### <a name="cdn-limits"></a>CDN limieten

[AZURE.INCLUDE [cdn-limits](../includes/cdn-limits.md)]

### <a name="mobile-services-limits"></a>Limieten voor mobiele Services

[AZURE.INCLUDE [mobile-services-limits](../includes/mobile-services-limits.md)]

### <a name="monitoring-limits"></a>Grenzen bewaken

[AZURE.INCLUDE [monitoring-limits](../includes/monitoring-limits.md)]

### <a name="notification-hub-service-limits"></a>Meldingslimieten Hub Service

[AZURE.INCLUDE [notification-hub-limits](../includes/notification-hub-limits.md)]

### <a name="event-hubs-limits"></a>Gebeurtenis Hubs limieten

[AZURE.INCLUDE [azure-servicebus-limits](../includes/event-hubs-limits.md)]

### <a name="service-bus-limits"></a>Grenzen van de service Bus

[AZURE.INCLUDE [azure-servicebus-limits](../includes/service-bus-quotas-table.md)]

### <a name="iot-hub-limits"></a>IoT Hub limieten

[AZURE.INCLUDE [azure-iothub-limits](../includes/iot-hub-limits.md)]

### <a name="data-factory-limits"></a>Data Factory beperkt

[AZURE.INCLUDE [azure-data-factory-limits](../includes/azure-data-factory-limits.md)]

### <a name="data-lake-analytics-limits"></a>Data Analytics voor meer grenzen
[AZURE.INCLUDE [azure-data-lake-analytics-limits](../includes/azure-data-lake-analytics-limits.md)]

### <a name="stream-analytics-limits"></a>Stream Analytics limieten
[AZURE.INCLUDE [stream-analytics-limits-table](../includes/stream-analytics-limits-table.md)]

### <a name="active-directory-limits"></a>Active Directory beperkt

[AZURE.INCLUDE [AAD-service-limits](../includes/active-directory-service-limits-include.md)]


### <a name="azure-remoteapp-limits"></a>Azure RemoteApp-limieten

[AZURE.INCLUDE [azure-remoteapp-limits](../includes/azure-remoteapp-limits.md)]

### <a name="storsimple-system-limits"></a>Beperkt StorSimple systeem

[AZURE.INCLUDE [storsimple-limits-table](../includes/storsimple-limits-table.md)]


### <a name="operational-insights-limits"></a>Bedrijfslimieten inzichten

[AZURE.INCLUDE [operational-insights-limits](../includes/operational-insights-limits.md)]

### <a name="backup-limits"></a>Back-limieten

[AZURE.INCLUDE [azure-backup-limits](../includes/azure-backup-limits.md)]

### <a name="site-recovery-limits"></a>Herstel van de site is beperkt

[AZURE.INCLUDE [site-recovery-limits](../includes/site-recovery-limits.md)]

### <a name="application-insights-limits"></a>Inzichten limieten van toepassing

[AZURE.INCLUDE [application-insights-limits](../includes/application-insights-limits.md)]

### <a name="api-management-limits"></a>Limieten voor de beheer-API

[AZURE.INCLUDE [api-management-service-limits](../includes/api-management-service-limits.md)]

### <a name="azure-redis-cache-limits"></a>Limieten voor Azure Cache bestand Vgx.

[AZURE.INCLUDE [redis-cache-service-limits](../includes/redis-cache-service-limits.md)]

### <a name="key-vault-limits"></a>Limieten sleutel kluis

[AZURE.INCLUDE [key-vault-limits](../includes/key-vault-limits.md)]

### <a name="multi-factor-authentication"></a>Meerledige verificatie
[AZURE.INCLUDE [azure-mfa-service-limits](../includes/azure-mfa-service-limits.md)]

### <a name="automation-limits"></a>Limieten voor automatisering
[AZURE.INCLUDE [automation-limits](../includes/azure-automation-service-limits.md)]

### <a name="sql-database-limits"></a>Grenzen van de SQL-Database

Zie [SQL Database Resource limieten](sql-database/sql-database-resource-limits.md)voor SQL-Database-limieten.

## <a name="see-also"></a>Zie ook

[Wat is Azure limieten en verhogingen?](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)

[Virtuele Machine en Cloud Service formaten voor Azure](virtual-machines/virtual-machines-linux-sizes.md)

[Formaten voor Cloud Services](cloud-services/cloud-services-sizes-specs.md)
