<properties 
  pageTitle="Implementeren van uw eigen persoonlijke Docker register op Azure | Microsoft Azure"
  description="Hierin wordt beschreven hoe u Docker register kunt gebruiken voor het hosten van uw afbeeldingen container op de Azure Blob Storage-service."
  services="virtual-machines-linux"
  documentationCenter="virtual-machines"
  authors="ahmetalpbalkan"
  editor="squillace"
  manager="timlt"
  tags="azure-service-management,azure-resource-manager" />

<tags
  ms.service="virtual-machines-linux"
  ms.devlang="multiple"
  ms.topic="article"
  ms.tgt_pltfrm="vm-linux"
  ms.workload="infrastructure-services"
  ms.date="09/27/2016" 
  ms.author="ahmetb" />

# <a name="deploying-your-own-private-docker-registry-on-azure"></a>Implementeren van uw eigen persoonlijke Docker register op Azure

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]



Dit document wordt beschreven welke Docker persoonlijke register is en ziet u hoe u een afbeelding van de container Docker register 2.0 kunt implementeren met een persoonlijke register van Docker op Microsoft Azure Azure Blob-opslag.

Dit document wordt ervan uitgegaan dat:

1. U weet hoe u Docker en Docker afbeeldingen op te slaan. (U niet? [Meer informatie over Docker](https://www.docker.com))
2. U hebt een server met Docker-engine geïnstalleerd. (U niet? [Snel doen op Azure.](https://azure.microsoft.com/documentation/templates/docker-simple-on-ubuntu/))


## <a name="what-is-a-private-docker-registry"></a>Wat is een persoonlijke Docker register?

Om uw beperkte toepassingen naar de cloud te verzenden, een container Docker image samenstellen en ergens opslaan zodat deze kan worden gebruikt door uzelf en door anderen. 

Een container-image maken en verzenden naar de cloud eenvoudig is, is maar het een uitdaging de gegenereerde afbeelding om op te slaan. Om die reden Docker biedt een centrale [Hub Docker] genoemd[ docker-hub] voor het opslaan van container afbeeldingen van de wolk en kunt u containers op elk gewenst moment met behulp van deze afbeeldingen maken.

Hoewel de [Hub Docker] [ docker-hub] is een betaalde service voor het opslaan van uw persoonlijke toepassingscontainer afbeeldingen, Docker opzichten ontwikkelaars behoeften en biedt een open source set hulpmiddelen voor het opslaan van de afbeeldingen in uw eigen persoonlijke register Docker achter een firewall of op locatie zonder op het openbare Internet.
Omdat Azure Blob-opslag eenvoudig is te beveiligen, kunt u deze snel maken en gebruiken in Azure waarmee u zelf een persoonlijke Docker register gebruiken.

## <a name="why-should-you-host-a-docker-registry-on-azure"></a>Waarom moet u een register Docker op Azure hosten?

Door het hosten van uw exemplaar van het register Docker op Microsoft Azure en opslaan van uw afbeeldingen op Azure Blob-opslag, hebt u verschillende voordelen:

**Beveiliging:** Docker afbeeldingen verlaten niet Azure datacenters, zodat ze niet het openbare Internet cross als wanneer u de Hub Docker zouden.
  
**Prestaties:** Docker afbeeldingen worden opgeslagen in het datacenter of de regio als uw toepassingen. Dit betekent dat de afbeeldingen worden opgevraagd snellere en meer betrouwbare prestaties ten opzichte van Docker Hub.

**Betrouwbaarheid:** Met behulp van Microsoft Azure Blob-opslag kunt u maken gebruik van veel Opslageigenschappen zoals hoge beschikbaarheid, redundantie, premium opslag (SSD), enzovoort.

## <a name="configuring-docker-registry-to-use-azure-blob-storage"></a>Docker register gebruik Azure Blob-opslag configureren

(Het is raadzaam om te lezen van de [documentatie van Docker register 2.0][register documenten] voordat u verdergaat.)

U kunt [configureren] [ registry-config] het register Docker op twee verschillende manieren.
U kunt:

1. Gebruik een `config.yml` bestand. In dit geval moet u een afzonderlijke afbeelding Docker boven maken `registry` afbeelding.
2. De standaard configuratiebestand door middel van omgevingsvariabelen vervangen: dingen gedaan zonder maken en onderhouden van een afzonderlijke afbeelding van Docker als resultaat gegeven.

In dit onderwerp volgt omwille van de eenvoud optie 2, de omgevingsvariabelen gebruiken.

Een register Docker exemplaar wilt uitvoeren die:

* uw Account Azure opslag gebruikt voor het opslaan van de afbeeldingen
* luistert op de virtuele Machine poort 5000
* is er geen verificatie is geconfigureerd (niet aanbevolen, Zie de onderstaande opmerking)

moet u de volgende opdracht voor Docker uitvoeren in uw terminal bash (Vervang `<storage-account>` en `<storage-key>` met uw referenties):

```sh
$ docker run -d -p 5000:5000 \
     -e REGISTRY_STORAGE=azure \
     -e REGISTRY_STORAGE_AZURE_ACCOUNTNAME="<storage-account>" \
     -e REGISTRY_STORAGE_AZURE_ACCOUNTKEY="<storage-key>" \
     -e REGISTRY_STORAGE_AZURE_CONTAINER="registry" \
     --name=registry \
     registry:2
```

Zodra de opdracht wordt afgesloten, ziet u de container die als host fungeert een exemplaar van uw persoonlijke Docker register door te voeren het `docker ps` in uw host:

```sh
$ docker ps
CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS              PORTS                    NAMES
3698ddfebc6f        registry:2          "registry cmd/regist   2 seconds ago       Up 1 seconds        0.0.0.0:5000->5000/tcp   registry
```

> [AZURE.IMPORTANT] Configureren van de beveiliging voor het register Docker is niet beschreven in dit document en het register zijn toegankelijk voor iedereen zonder verificatie standaard als u de poort op de poort van het register van het eindpunt van de virtuele Machine te openen of de belasting voor documentconversies als u bovenstaande opdracht implementatie.
>
> Lees het [Configureren van Docker register] [ registry-config] documentatie voor meer informatie over het beveiligen van het register en de afbeeldingen.

## <a name="next-steps"></a>Volgende stappen

Zodra u uw register is ingesteld hebt, is het tijd om wat meer te gebruiken. Beginnen met de docker [register docs]. 

[docker-hub]: https://hub.docker.com/
[registry]: https://github.com/docker/distribution
[register-docs]: http://docs.docker.com/registry/
[registry-config]: http://docs.docker.com/registry/configuration/
 
