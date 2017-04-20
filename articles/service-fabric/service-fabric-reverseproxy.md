<properties
   pageTitle="Fabric Reverse Proxy service | Microsoft Azure"
   description="Service-Fabric reverse-proxy gebruiken voor communicatie met microservices van binnen en buiten het cluster"
   services="service-fabric"
   documentationCenter=".net"
   authors="BharatNarasimman"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="required"
   ms.date="10/04/2016"
   ms.author="vturecek"/>

# <a name="service-fabric-reverse-proxy"></a>Reverse Proxy-service Fabric

De Service Fabric Reverse proxy is een reverse proxy service ingebouwd in weefsel waarmee adressering microservices in het cluster service-structuur die HTTP-eindpunten worden blootgesteld.

## <a name="microservices-communication-model"></a>Microservices communicatiemodel

Microservices service stof wordt doorgaans uitgevoerd op een subset van de VM in het cluster en om verschillende redenen van een VM kunt verplaatsen naar een andere. Zodat de eindpunten voor microservices dynamisch wijzigen kunnen. Het normale patroon te communiceren met de microservice is de lus oplossen hieronder

1. De locatie van de in eerste instantie via de Naming Service oplossen.
2. Verbinding maken met de service.
3. De oorzaak van verbindingsfouten en de locatie van de indien nodig te zetten.

Dit proces omvat in het algemeen verpakking communicatie van client-side-bibliotheken opnieuw herhaald dat de service-oplossing en probeer het opnieuw beleid implementeert.
Zie voor meer informatie over dit onderwerp, de [communicatie met services](service-fabric-connect-and-communicate-with-services.md).

### <a name="communicating-via-sf-reverse-proxy"></a>Communicatie via SF reverse-proxy
Reverse proxy Fabric-service wordt uitgevoerd op alle knooppunten in het cluster. Het omzettingsproces van de volledige service uitvoert namens een client en stuurt de aanvraag van de client. Zodat kunt clients op het cluster alleen alle bibliotheken communicatie van client-side HTTP-Neem contact op met de target-service via de SF reverse proxy lokaal op hetzelfde knooppunt worden uitgevoerd.

![Interne communicatie][1]

## <a name="reaching-microservices-from-outside-the-cluster"></a>Microservices van buiten het cluster is bereikt
Het standaardmodel voor externe communicatie voor microservices is **opt-in** , waarbij elke service standaard niet toegankelijk vanuit externe clients rechtstreeks. De [Taakverdeling Azure](../load-balancer/load-balancer-overview.md) is een netwerk grens tussen microservices en externe clients die NAT wordt uitgevoerd en de externe aanvragen worden doorgestuurd naar de interne **IP: poort** eindpunten. Om het eindpunt van een microservice rechtstreeks toegankelijk maken voor externe clients, zijn de taakverdeling Azure eerst verkeer wilt doorsturen naar elke poort wordt gebruikt door de service in het cluster geconfigureerd. Bovendien de meeste microservices (esp. stateful microservices) op alle knooppunten van het cluster niet live en ze kunnen worden verplaatst tussen knooppunten in een failover zodat het in dergelijke gevallen wordt de taakverdeling Azure effectief de doelnode van de replica's niet bepalen bevinden zich voor het doorsturen van verkeer.

### <a name="reaching-microservices-via-the-sf-reverse-proxy-from-outside-the-cluster"></a>Microservices bereiken via de SF reverse proxy van buiten het cluster

In plaats van afzonderlijke service poorten configureren in de azure taakverdeling, kan alleen de SF Reverse proxypoort in de Azure Load Balancer worden geconfigureerd. Hiermee kunnen clients buiten het cluster services binnen het cluster via de reverse proxy zonder een aanvullende configuraties te bereiken.

![Externe communicatie][0]

>[AZURE.WARNING] De reverse proxy poort configureren op de taakverdeling, kunt u de micro services in het cluster die een HTTP-eindpunt, worden addressible van buiten het cluster.


## <a name="uri-format-for-addressing-services-via-the-reverse-proxy"></a>URI-indeling voor het adresseren van diensten via de reverse-proxy

URI-indeling Reverse proxy gebruikt om te bepalen welke binnenkomende aanvraag moet worden doorgestuurd naar service-partitie:

```
http(s)://<Cluster FQDN | internal IP>:Port/<ServiceInstanceName>/<Suffix path>?PartitionKey=<key>&PartitionKind=<partitionkind>&Timeout=<timeout_in_seconds>
```

 - **http (s):** De reverse proxy kan worden geconfigureerd voor het accepteren van HTTP- of HTTPS-verkeer. In het geval van HTTPS-verkeer plaatsvindt SSL-beëindiging op de reverse proxy. Aanvragen die zijn doorgestuurd door de reverse proxy services in het cluster worden via http.
 - **Cluster FULLY | interne IP:** Voor externe clients kan de reverse proxy worden geconfigureerd zodat deze bereikbaar via het clusterdomein (bijvoorbeeld mycluster.eastus.cloudapp.azure.com is). Standaard die de reverse proxy wordt uitgevoerd op elk knooppunt dus voor het interne verkeer dat kan worden bereikt op de localhost of op elk intern knooppunt IP-(bijv. 10.0.0.1).
 - **Poort:** De poort die is opgegeven voor de reverse proxy. Bijvoorbeeld: 19008.
 - **ServiceInstanceName:** Dit is de naam van het volledige gedistribueerde service exemplaar van de service die u probeert te bereiken de ' fabric: / ' schema. Om bijvoorbeeld te bereiken service *fabric: / myapp/MijnService/*, gebruikt u *myapp/myservice*.
 - **Achtervoegsel pad:** Dit is de werkelijke URL-pad voor de service waarmee u verbinding wilt maken. Voorbeeld: *myapi/waarden/toevoegen/3*
 - **PartitionKey:** Dit is de partitiesleutel berekende van de partitie die u wilt bereiken voor een gepartitioneerde service. Opmerking: dit is *niet* de partitie-ID GUID. Deze parameter is niet vereist voor het partitieschema singleton-services.
 - **PartitionKind:** Het partitieschema van de service. Dit is de 'Int64Range' of 'Naam'. Deze parameter is niet vereist voor het partitieschema singleton-services.
 - **-Time-out:**  Hiermee geeft u de time-out voor de HTTP-aanvraag die door de reverse proxy naar de service voor aanvraag van de client zijn gemaakt. De standaardwaarde voor dit is 60 seconden. Dit is een optionele parameter.

### <a name="example-usage"></a>Voorbeeld van gebruik

Als voorbeeld nemen we service **fabric: / MyApp/MijnService** die wordt geopend met een HTTP-listener op de volgende URL:

```
http://10.0.05:10592/3f0d39ad-924b-4233-b4a7-02617c6308a6-130834621071472715/
```

Met de volgende bronnen:

 - `/index.html`
 - `/api/users/<userId>`

Als u de service gebruikt het partitieschema singleton, de queryreeksparameters *PartitionKey* en *PartitionKind* zijn niet vereist en de service kan worden bereikt via de gateway als:

 - Extern:`http://mycluster.eastus.cloudapp.azure.com:19008/MyApp/MyService`
 - Intern:`http://localhost:19008/MyApp/MyService`

Als de service wordt gebruikt voor het partitieschema van uniforme Int64, moeten de queryreeksparameters *PartitionKey* en *PartitionKind* worden gebruikt voor het bereiken van een partitie van de service:

 - Extern:`http://mycluster.eastus.cloudapp.azure.com:19008/MyApp/MyService?PartitionKey=3&PartitionKind=Int64Range`
 - Intern:`http://localhost:19008/MyApp/MyService?PartitionKey=3&PartitionKind=Int64Range`

Voor het bereiken van de bronnen die door de service, plaatst u het pad van de resource na de naam van de URL:

 - Extern:`http://mycluster.eastus.cloudapp.azure.com:19008/MyApp/MyService/index.html?PartitionKey=3&PartitionKind=Int64Range`
 - Intern:`http://localhost:19008/MyApp/MyService/api/users/6?PartitionKey=3&PartitionKind=Int64Range`

De gateway vervolgens doorstuurt deze verzoeken naar de URL van de service:

 - `http://10.0.05:10592/3f0d39ad-924b-4233-b4a7-02617c6308a6-130834621071472715/index.html`
 - `http://10.0.05:10592/3f0d39ad-924b-4233-b4a7-02617c6308a6-130834621071472715/api/users/6`

## <a name="special-handling-for-port-sharing-services"></a>Speciaal voor het delen van poorten services

De Gateway van de toepassing probeert te zetten een adres en de aanvraag opnieuw als een service niet kan worden bereikt. Dit is een van de belangrijkste voordelen van de gateway, clientcode hoeft niet een eigen service-oplossing te implementeren en lus oplossen.

In het algemeen als een service niet kan worden bereikt betekent dat het exemplaar of de replica is verplaatst naar een ander knooppunt als onderdeel van hun normale levenscyclus. Als dit gebeurt, wordt de gateway een netwerkfout verbinding een eindpunt is niet langer geopend op de oorspronkelijk omgezette adres aangeeft.

Echter replica's of exemplaren van de service een hostproces kunt delen en een poort wanneer deze wordt gehost door een HTTP-webserver kan ook worden gedeeld met inbegrip van:

 - [System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener%28v=vs.110%29.aspx)
 - [ASP.NET-Core WebListener](https://docs.asp.net/latest/fundamentals/servers.html#weblistener)
 - [Katana](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.OwinSelfHost/)

In dit geval is het waarschijnlijk dat de webserver beschikbaar is in het hostproces en reageert op aanvragen, maar het omgezette exemplaar of de replica niet langer beschikbaar is op de host is. De gateway ontvangt in dit geval een 404 HTTP-reactie van de webserver. Als gevolg hiervan heeft een HTTP 404 twee verschillende betekenissen:

 1. Het adres van de juiste is, maar de resource is aangevraagd door de gebruiker bestaat niet.
 2. Het adres is onjuist en de resource is aangevraagd door de gebruiker kan werkelijk bestaat op een ander knooppunt.

In het eerste geval is dit een normale HTTP 404, wordt beschouwd als een fout van de gebruiker. Echter, in het tweede geval de gebruiker heeft aangevraagd een bron die bestaat, maar de gateway is niet vinden omdat de service zelf is verplaatst, in welk geval de gateway moet het adres opnieuw en probeer het opnieuw.

De gateway moet dus te onderscheiden tussen deze twee gevallen. Dit onderscheid maken, is een aanwijzing van de server vereist.

 - De Gateway van de toepassing wordt ervan uitgegaan dat geval #2 en standaard probeert te zetten en de aanvraag opnieuw uitgeven.
 - Om aan te geven geval #1 op de Gateway-toepassing, moet de service de volgende HTTP-antwoordheader retourneren:

`X-ServiceFabric : ResourceNotFound`

Deze HTTP-antwoordheader geeft aan dat een normale HTTP 404 situatie waarin de aangevraagde bron bestaat niet en de gateway zal niet proberen om te zetten van het adres van de.

## <a name="setup-and-configuration"></a>Installatie en configuratie
De service fabric Reverse proxy kan worden ingeschakeld voor het cluster via de [sjabloon Azure Resource Manager](./service-fabric-cluster-creation-via-arm.md).

Zodra u de sjabloon voor het cluster dat u wilt implementeren (uit de voorbeeldsjablonen of door een aangepaste resource manager-sjabloon te maken) de Reverse proxy in de sjabloon kan worden ingeschakeld door de volgende stappen uit.

1. Definieer een poort voor de reverse proxy in de [sectie Parameters](../resource-group-authoring-templates.md) van de sjabloon.

    ```json
    "SFReverseProxyPort": {
        "type": "int",
        "defaultValue": 19008,
        "metadata": {
            "description": "Endpoint for Service Fabric Reverse proxy"
        }
    },
    ```
2. De poort opgeven voor elk van de objecten nodetype in de **Cluster** [Resource type sectie](../resource-group-authoring-templates.md)

    Voor de apiVersion voor 2016-09-01' de poort wordt aangegeven door de parameter ***httpApplicationGatewayEndpointPort*** een naam geven

    ```json
    {
        "apiVersion": "2016-03-01",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        ...
       "nodeTypes": [
          {
           ...
           "httpApplicationGatewayEndpointPort": "[parameters('SFReverseProxyPort')]",
           ...
          },
        ...
        ],
        ...
    }
    ```

    Voor apiVersion van op of na 2016-09-01' de poort wordt aangegeven door de parameter ***reverseProxyEndpointPort*** een naam geven

    ```json
    {
        "apiVersion": "2016-09-01",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        ...
       "nodeTypes": [
          {
           ...
           "reverseProxyEndpointPort": "[parameters('SFReverseProxyPort')]",
           ...
          },
        ...
        ],
        ...
    }
    ```

3. Setup om de reverse proxy van buiten het cluster azure, de **azure load balancer-regels** voor de poort die is opgegeven in stap 1.

    ```json
    {
        "apiVersion": "[variables('lbApiVersion')]",
        "type": "Microsoft.Network/loadBalancers",
        ...
        ...
        "loadBalancingRules": [
            ...
            {
                "name": "LBSFReverseProxyRule",
                "properties": {
                    "backendAddressPool": {
                        "id": "[variables('lbPoolID0')]"
                    },
                    "backendPort": "[parameters('SFReverseProxyPort')]",
                    "enableFloatingIP": "false",
                    "frontendIPConfiguration": {
                        "id": "[variables('lbIPConfig0')]"
                    },
                    "frontendPort": "[parameters('SFReverseProxyPort')]",
                    "idleTimeoutInMinutes": "5",
                    "probe": {
                        "id": "[concat(variables('lbID0'),'/probes/SFReverseProxyProbe')]"
                    },
                    "protocol": "tcp"
                }
            }
        ],
        "probes": [
            ...
            {
                "name": "SFReverseProxyProbe",
                "properties": {
                    "intervalInSeconds": 5,
                    "numberOfProbes": 2,
                    "port":     "[parameters('SFReverseProxyPort')]",
                    "protocol": "tcp"
                }
            }  
        ]
    }
    ```
4. Het configureren van SSL-certificaten op de poort voor Reverse proxy, kunt u het certificaat toevoegen aan de eigenschap httpApplicationGatewayCertificate in het **Cluster** [Resource type sectie](../resource-group-authoring-templates.md)

    Voor de apiVersion voor 2016-09-01' het certificaat wordt geïdentificeerd door de parameter ***httpApplicationGatewayCertificate*** een naam geven

    ```json
    {
        "apiVersion": "2016-03-01",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        "dependsOn": [
            "[concat('Microsoft.Storage/storageAccounts/', parameters('supportLogStorageAccountName'))]"
        ],
        "properties": {
            ...
            "httpApplicationGatewayCertificate": {
                "thumbprint": "[parameters('sfReverseProxyCertificateThumbprint')]",
                "x509StoreName": "[parameters('sfReverseProxyCertificateStoreName')]"
            },
            ...
            "clusterState": "Default",
        }
    }
    ```
    Voor apiVersion van op of na 2016-09-01' het certificaat wordt geïdentificeerd door de parameter ***reverseProxyCertificate*** een naam geven
    
    ```json
    {
        "apiVersion": "2016-09-01",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        "dependsOn": [
            "[concat('Microsoft.Storage/storageAccounts/', parameters('supportLogStorageAccountName'))]"
        ],
        "properties": {
            ...
            "reverseProxyCertificate": {
                "thumbprint": "[parameters('sfReverseProxyCertificateThumbprint')]",
                "x509StoreName": "[parameters('sfReverseProxyCertificateStoreName')]"
            },
            ...
            "clusterState": "Default",
        }
    }
    ```

## <a name="next-steps"></a>Volgende stappen
 - Zie een voorbeeld van HTTP-communicatie tussen services in een [voorbeeldproject op GitHUb](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/master/Services/WordCount).

 - [Externe procedure-oproepen met betrouwbare diensten remoting](service-fabric-reliable-services-communication-remoting.md)

 - [Web API die gebruikmaakt van OWIN in betrouwbare diensten](service-fabric-reliable-services-communication-webapi.md)

 - [WCF-communicatie via betrouwbare diensten](service-fabric-reliable-services-communication-wcf.md)


[0]: ./media/service-fabric-reverseproxy/external-communication.png
[1]: ./media/service-fabric-reverseproxy/internal-communication.png
