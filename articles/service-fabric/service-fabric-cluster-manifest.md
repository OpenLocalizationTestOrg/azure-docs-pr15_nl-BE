<properties
   pageTitle="De autonome cluster configureren | Microsoft Azure"
   description="In dit artikel wordt beschreven hoe een zelfstandige of een persoonlijke Service Fabric '-cluster configureren."
   services="service-fabric"
   documentationCenter=".net"
   authors="dsk-2015"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/07/2016"
   ms.author="dkshir"/>


# <a name="configuration-settings-for-standalone-windows-cluster"></a>Configuratie-instellingen voor een zelfstandige Windows-cluster

In dit artikel wordt beschreven hoe u van een zelfstandige Service Fabric '-cluster met behulp van het bestand _**ClusterConfig.JSON**_ . U kunt dit bestand gebruiken om informatie te geven zoals de knooppunten Fabric-Service en de IP-adressen, de verschillende typen knooppunten in het cluster, de beveiligingsconfiguraties, evenals de netwerktopologie van fouttolerantie/upgraden domeinen voor uw cluster zelfstandige.

Als u [de zelfstandige Service Fabric downloaden](service-fabric-cluster-creation-for-windows-server.md#downloadpackage), een paar voorbeelden van het ClusterConfig.JSON-bestand worden gedownload naar uw computer werken. De monsters met *DevCluster* in hun naam kunt u een cluster maken met alle drie knooppunten op dezelfde computer als logische knooppunten. Van deze, moet ten minste één knooppunt worden gemarkeerd als primaire knooppunt. Dit cluster is handig voor een ontwikkel- of testomgeving is bedoeld en wordt niet ondersteund als een productie-cluster. De monsters met *MultiMachine* in hun namen, kunt u een productie kwaliteit-cluster maken met elk knooppunt op een afzonderlijke computer. Het nummer van de primaire knooppunten die voor deze cluster zal worden gebaseerd op het [niveau van betrouwbaarheid](#reliability).

1. Laten zien hoe maakt u een onbeveiligde test- of cluster respectievelijk *ClusterConfig.Unsecure.DevCluster.JSON* en *ClusterConfig.Unsecure.MultiMachine.JSON* . 
    
2. *ClusterConfig.Windows.DevCluster.JSON* en *ClusterConfig.Windows.MultiMachine.JSON* weergeven het maken van de test- of cluster, beveiligd met [Windows-beveiliging](service-fabric-windows-cluster-windows-security.md)

3. Hoe te maken van de test- of cluster, beveiligd met *ClusterConfig.X509.DevCluster.JSON* en *ClusterConfig.X509.MultiMachine.JSON* [X509 op certificaten gebaseerde beveiliging](service-fabric-windows-cluster-x509-security.md). 


Nu bespreken we de verschillende secties van een _**ClusterConfig.JSON**_ bestand als hieronder.

## <a name="general-cluster-configurations"></a>Algemene clusterconfiguraties
Dit betreft de brede cluster specifieke configuraties, zoals in het onderstaande stukje van JSON.

    "name": "SampleCluster",
    "clusterConfigurationVersion": "1.0.0",
    "apiVersion": "2015-01-01-alpha",

U kunt een beschrijvende naam geven om uw Service Fabric '-cluster toewijzen aan de variabele **naam** . De **clusterConfigurationVersion** is het versienummer van de cluster. Telkens wanneer u een upgrade uitvoert van uw Service Fabric '-cluster, moet u deze verhogen. Maar laat u de **apiVersion** op de standaardwaarde.


<a id="clusternodes"></a>
## <a name="nodes-on-the-cluster"></a>Knooppunten in het cluster
Kunt u de knooppunten in de cluster-Service Fabric met behulp van de sectie **knooppunten** , zoals in het volgende fragment wordt getoond.

    "nodes": [{
        "nodeName": "vm0",
        "iPAddress": "localhost",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r0",
        "upgradeDomain": "UD0"
    }, {
        "nodeName": "vm1",
        "iPAddress": "localhost",
        "nodeTypeRef": "NodeType1",
        "faultDomain": "fd:/dc1/r1",
        "upgradeDomain": "UD1"
    }, {
        "nodeName": "vm2",
        "iPAddress": "localhost",
        "nodeTypeRef": "NodeType2",
        "faultDomain": "fd:/dc1/r2",
        "upgradeDomain": "UD2"
    }],

Een Service Fabric '-cluster moet ten minste 3 knooppunten bevatten. In deze sectie kunt u meer knooppunten toevoegen aan de hand van uw instellingen. De volgende tabel beschrijft de configuratie-instellingen voor elk knooppunt.

|**Knooppuntconfiguratie**|**Beschrijving**|
|-----------------------|--------------------------|
|Knooppuntnaam|U kunt een beschrijvende naam geven aan het knooppunt.|
|IP-adres|Ontdek het IP-adres van het knooppunt door een opdrachtvenster te openen en te typen `ipconfig`. Noteer het IPv4-adres en wijst u deze toe aan de variabele **IP-adres** .|
|nodeTypeRef|Elk knooppunt kan worden toewijst aan een ander knooppunt. De [knooppunttypen](#nodetypes) worden gedefinieerd in de sectie hieronder.|
|faultDomain|Fout met betrekking tot domeinen kunnen clusterbeheerders definieert de fysieke knooppunten die mogelijk niet op hetzelfde moment als gevolg van afhankelijkheden van gedeelde fysieke.|
|upgradeDomain|Upgraden van domeinen sets van knooppunten die zijn afgesloten voor Fabric Service-upgrades op ongeveer tegelijk wordt beschreven. U kunt welke knooppunten toe te wijzen aan welke domeinen Upgrade, als ze niet worden beperkt door de fysieke vereisten.| 


## <a name="cluster-properties"></a>Eigenschappen van cluster

De sectie **-Eigenschappen** in de ClusterConfig.JSON wordt gebruikt voor het configureren van het cluster als volgt.

<a id="reliability"></a>
### <a name="reliability"></a>Betrouwbaarheid 
Het **reliabilityLevel** gedeelte definieert het aantal exemplaren van de system-services die kunnen worden uitgevoerd op de primaire knooppunten van het cluster. Dit verhoogt de betrouwbaarheid van deze services en dus het cluster. U kunt deze variabele instellen op hetzij *Bronze*, *Silver*, *Gold* of *Platinum* voor 3, 5, 7 of 9 exemplaren van deze services respectievelijk. Zie het onderstaande voorbeeld.

    "reliabilityLevel": "Bronze",
    
Houd er rekening mee dat sinds een primaire knooppunt één exemplaar van de systeemservices wordt, u minimaal 3 primaire knooppunten voor *Brons*, 5 voor *Zilver*, 7 voor *goud* en 9 voor *Platinum* betrouwbaarheid niveaus moet.


### <a name="diagnostics"></a>Diagnostische gegevens
De sectie **diagnosticsStore** kunt u voor het configureren van parameters als diagnose en het oplossen van fouten in knooppunt of een cluster, zoals in het volgende fragment wilt inschakelen. 

    "diagnosticsStore": {
        "metadata":  "Please replace the diagnostics store with an actual file share accessible from all cluster machines.",
        "dataDeletionAgeInDays": "7",
        "storeType": "FileShare",
        "IsEncrypted": "false",
        "connectionstring": "c:\\ProgramData\\SF\\DiagnosticsStore"
    }

De **metagegevens** is een beschrijving van de diagnostische gegevens van het cluster en aan de hand van de instellingen kan worden ingesteld. Deze variabelen helpen ETW traceerlogboeken, crashdumps alsmede prestatiemeteritems verzamelt. [Tracelog](https://msdn.microsoft.com/library/windows/hardware/ff552994.aspx) en [ETW-tracering](https://msdn.microsoft.com/library/ms751538.aspx) voor meer informatie over Logboeken voor het traceren van ETW gelezen. Alle logboeken inclusief [Crash dumpen](https://blogs.technet.microsoft.com/askperf/2008/01/08/understanding-crash-dump-files/) en [prestatiemeteritems](https://msdn.microsoft.com/library/windows/desktop/aa373083.aspx) kunnen worden doorgestuurd naar de **connectionString** -map op uw computer. U kunt ook *AzureStorage* gebruiken om diagnostische gegevens op te slaan. Zie hieronder voor een steekproef fragment.

    "diagnosticsStore": {
        "metadata":  "Please replace the diagnostics store with an actual file share accessible from all cluster machines.",
        "dataDeletionAgeInDays": "7",
        "storeType": "AzureStorage",
        "IsEncrypted": "false",
        "connectionstring": "xstore:DefaultEndpointsProtocol=https;AccountName=[AzureAccountName];AccountKey=[AzureAccountKey]"
    }

### <a name="security"></a>Beveiliging 
De sectie **beveiliging** is nodig voor een veilig zelfstandig Service Fabric-cluster. In het volgende fragment toont een deel van deze sectie.

    "security": {
        "metadata": "This cluster is secured using X509 certificates.",
        "ClusterCredentialType": "X509",
        "ServerCredentialType": "X509",
        . . .
    }

De **metagegevens** is een beschrijving van het cluster beveiligen en aan de hand van de instellingen kan worden ingesteld. De **ClusterCredentialType** en **ServerCredentialType** bepalen het type beveiliging dat de knooppunten en het cluster implementeert. Ze kunnen worden ingesteld op een *X509* voor een beveiliging op basis van certificaten of *Windows* voor een beveiliging op basis van Azure Active Directory. De rest van de sectie **beveiliging** zal worden gebaseerd op het type van de beveiliging. [Beveiliging op basis van certificaten in een cluster van zelfstandige](service-fabric-windows-cluster-x509-security.md) of [Windows-beveiliging in een cluster zelfstandig](service-fabric-windows-cluster-windows-security.md) lezen voor informatie over het invullen van de rest van de sectie **beveiliging** .


<a id="nodetypes"></a>
### <a name="node-types"></a>Knooppunttypen
De sectie **nodeTypes** beschrijft het type van de knooppunten van dat cluster heeft. Type ten minste één knooppunt moet worden opgegeven voor een cluster, zoals in het onderstaande stukje. 

    "nodeTypes": [{
        "name": "NodeType0",
        "clientConnectionEndpointPort": "19000",
        "clusterConnectionEndpointPort": "19001",
        "leaseDriverEndpointPort": "19002"
        "serviceConnectionEndpointPort": "19003",
        "httpGatewayEndpointPort": "19080",
        "applicationPorts": {
            "startPort": "20575",
            "endPort": "20605"
        },
        "ephemeralPorts": {
            "startPort": "20606",
            "endPort": "20861"
        },
        "isPrimary": true
    }]

De **naam** is de beschrijvende naam voor dit type bepaald knooppunt. Voor het maken van een knooppunt van dit knooppunttype, het toewijzen van de beschrijvende naam voor de variabele **nodeTypeRef** voor dat knooppunt, als vermeld [hierboven](#clusternodes). Voor elk knooppunttype definiëren de eindpunten van de verbinding die wordt gebruikt. U kunt elk poortnummer voor deze eindpunten verbinding, zolang zij niet in strijd met eindpunten in dit cluster. In een cluster met meerdere knooppunten worden een of meer primaire knooppunten (dat wil zeggen **isPrimary** ingesteld op *true*), afhankelijk van de [**reliabilityLevel**](#reliability). [Service Fabric cluster capaciteit overwegingen bij de planning](service-fabric-cluster-capacity.md) voor informatie op **nodeTypes** en **reliabilityLevel** , en te weten wat zijn primaire en de niet-primaire knooppunttypen lezen. 

#### <a name="endpoints-used-to-configure-the-node-types"></a>Gebruikt voor het configureren van de knooppunttypen eindpunten

- *clientConnectionEndpointPort* is de poort die door de client verbinding maken met het cluster, als u de client-API gebruikt. 
- *clusterConnectionEndpointPort* is de poort die de knooppunten met elkaar communiceren.
- *leaseDriverEndpointPort* is de poort die wordt gebruikt door het cluster lease-stuurprogramma om na te gaan of de knooppunten nog steeds actief zijn. 
- *serviceConnectionEndpointPort* is de poort die door de toepassingen en services die zijn geïmplementeerd op een knooppunt, gebruikt om te communiceren met de client Service stof op het desbetreffende knooppunt.
- *httpGatewayEndpointPort* is de poort die wordt gebruikt door de Service Fabric Explorer verbinding maken met het cluster.
- *ephemeralPorts* zijn de [dynamische poorten worden gebruikt door het besturingssysteem](https://support.microsoft.com/kb/929851). Fabric-service gebruikt een deel van deze poorten en de resterende beschikbaar zal zijn voor het besturingssysteem. Het wordt ook dit bereik toewijzen aan het bestaande bereik aanwezig zijn in het besturingssysteem, zodat voor alle doeleinden, kunt u de bereiken in de voorbeeldbestanden JSON gegeven. U moet ervoor zorgen dat het verschil tussen het begin en het einde poorten ten minste 255 is. 
- *applicationPorts* zijn de poorten die worden gebruikt door de Service Fabric-toepassingen. Deze moet een subset van de *ephemeralPorts*, genoeg voor de behoefte van het eindpunt van uw toepassingen. Fabric-service gebruikt deze wanneer nieuwe poorten vereist zijn, alsmede voor het openen van de firewall voor deze poorten zorgen. 
- *reverseProxyEndpointPort* is een optioneel reverse proxy-eindpunt. Zie [Service Fabric Reverse-Proxy](service-fabric-reverseproxy.md) voor meer informatie. 


### <a name="other-settings"></a>Andere instellingen
De sectie **fabricSettings** kunt u de hoofdmappen voor de Service Fabric-gegevens en logboekbestanden instellen. U kunt deze alleen tijdens het initiële cluster maken. Zie hieronder voor een steekproef fragment van deze sectie.

    "fabricSettings": [{
        "name": "Setup",
        "parameters": [{
            "name": "FabricDataRoot",
            "value": "C:\\ProgramData\\SF"
        }, {
            "name": "FabricLogRoot",
            "value": "C:\\ProgramData\\SF\\Log"
    }]

Het is raadzaam een station niet OS als de FabricDataRoot en FabricLogRoot gebruiken als het biedt dat meer betrouwbaarheid tegen OS loopt vast. Houd er rekening mee dat als u alleen de hoofdmap van de gegevens aanpassen, vervolgens de log-hoofdmap worden geplaatst één niveau onder de hoofdmap van de gegevens.


## <a name="next-steps"></a>Volgende stappen

Wanneer u een volledig ClusterConfig.JSON bestand aan de hand van uw cluster stand-alone installatie geconfigureerd hebt, kunt u het cluster implementeren door het artikel [een Azure Service Fabric '-cluster maken in ruimten of in de cloud](service-fabric-cluster-creation-for-windows-server.md) en gaat u verder met [het cluster Service Fabric Explorer visualiseren](service-fabric-visualizing-your-cluster.md).


