<properties
   pageTitle="Schaal van de ACS-cluster met de CLI Azure | Microsoft Azure"
   description="Het cluster met behulp van de CLI Azure Azure Container Service schalen."
   services="container-service"
   documentationCenter=""
   authors="Thraka"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Docker, Containers, Micro-services, Mesos, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/03/2016"
   ms.author="timlt"/>

# <a name="scale-an-azure-container-service"></a>Schaal een Azure Container Service

U kunt het aantal knooppunten de Azure Container Service (ACS) is met behulp van het hulpprogramma Azure CLI schalen. Wanneer u de Azure CLI te schalen, retourneert u het hulpprogramma een nieuw configuratiebestand dat de wijziging van toepassing op de container vertegenwoordigt.

## <a name="about-the-command"></a>Over de opdracht

De Azure CLI moet zijn in de modus voor interactie met Azure Containers Azure Resource Manager. U kunt overschakelen naar de modus Resource Manager door het aanroepen van `azure config mode arm`. De `acs` opdracht heeft een kind-opdracht met de naam `scale` die de schaal bewerkingen voor een container service biedt. U kunt help-informatie over de verschillende parameters gebruikt in de opdracht door te voeren `azure acs scale --help`, die er ongeveer uitzien als deze uitvoer:

```azurecli
azure acs scale --help

help:    The operation to scale a container service.
help:
help:    Usage: acs scale [options] <resource-group> <name> <new-agent-count>
help:
help:    Options:
help:      -h, --help                               output usage information
help:      -v, --verbose                            use verbose output
help:      -vv                                      more verbose with debug output
help:      --json                                   use json output
help:      -g, --resource-group <resource-group>    resource-group
help:      -n, --name <name>                        name
help:      -o, --new-agent-count <new-agent-count>  New agent count
help:      -s, --subscription <subscription>        The subscription identifier
help:
help:    Current Mode: arm (Azure Resource Management)
```

## <a name="use-the-command-to-scale"></a>Gebruik de opdracht moet worden geschaald

Wilt schalen op een container-service, moet u eerst weten de **resourcegroep** en de **naam Azure Container Service (ACS)**en het nieuwe aantal agenten ook opgeven. Met behulp van een bedrag kleiner of hoger, kunt u de schaal omlaag of omhoog, respectievelijk.

U wilt weten wat het huidige aantal agenten voor een container service voordat u de schaal. Gebruik de `azure acs show <resource group> <ACS name>` de opdracht om op te vragen van de ACS-config. Opmerking het resultaat van de <mark>telling</mark> .

#### <a name="see-current-count"></a>Huidig aantal Zie

```azurecli
azure acs show containers-test containerservice-containers-test

info:    Executing command acs show
data:
data:     Id                 : /subscriptions/<guid>/resourceGroups/containers-test/providers/Microsoft.ContainerService/containerServices/containerservice-containers-test
data:     Name               : containerservice-containers-test
data:     Type               : Microsoft.ContainerService/ContainerServices
data:     Location           : westus
data:     ProvisioningState  : Succeeded
data:     OrchestratorProfile
data:       OrchestratorType : DCOS
data:     MasterProfile
data:       Count            : 1
data:       DnsPrefix        : myprefixmgmt
data:       Fqdn             : myprefixmgmt.westus.cloudapp.azure.com
data:     AgentPoolProfiles
data:       #0
data:         Name           : agentpools
data:         <mark>Count          : 1</mark>
data:         VmSize         : Standard_D2
data:         DnsPrefix      : myprefixagents
data:         Fqdn           : myprefixagents.westus.cloudapp.azure.com
data:     LinuxProfile
data:       AdminUsername    : azureuser
data:       Ssh
data:         PublicKeys
data:           #0
data:             KeyData    : ssh-rsa <ENCODED VALUE>
data:     DiagnosticsProfile
data:       VmDiagnostics
data:         Enabled        : true
data:         StorageUri     : https://<storageid>.blob.core.windows.net/
```  

#### <a name="scale-to-new-count"></a>Aantal nieuwe schalen

Zoals u waarschijnlijk al is het voor de hand liggende, kunt u de container service schalen door het aanroepen van `azure acs scale` en het leveren van de **resourcegroep**, **ACS-naam**en **aantal van de agent**. Wanneer u een container service schaalt, retourneert Azure CLI JSON string die de nieuwe configuratie van de container service, met inbegrip van de nieuwe agent telling.

```azurecli
azure acs scale containers-test containerservice-containers-test 10

info:    Executing command acs scale
data:    {
data:        id: '/subscriptions/<guid>/resourceGroups/containers-test/providers/Microsoft.ContainerService/containerServices/containerservice-containers-test',
data:        name: 'containerservice-containers-test',
data:        type: 'Microsoft.ContainerService/ContainerServices',
data:        location: 'westus',
data:        provisioningState: 'Succeeded',
data:        orchestratorProfile: { orchestratorType: 'DCOS' },
data:        masterProfile: {
data:            count: 1,
data:            dnsPrefix: 'myprefixmgmt',
data:            fqdn: 'myprefixmgmt.westus.cloudapp.azure.com'
data:        },
data:        agentPoolProfiles: [
data:            {
data:                name: 'agentpools',
data:                <mark>count: 10</mark>,
data:                vmSize: 'Standard_D2',
data:                dnsPrefix: 'myprefixagents',
data:                fqdn: 'myprefixagents.westus.cloudapp.azure.com'
data:            }
data:        ],
data:        linuxProfile: {
data:            adminUsername: 'azureuser',
data:            ssh: {
data:                publicKeys: [
data:                    { keyData: 'ssh-rsa <ENCODED VALUE>' }
data:                ]
data:            }
data:        },
data:        diagnosticsProfile: {
data:            vmDiagnostics: { enabled: true, storageUri: 'https://<storageid>.blob.core.windows.net/' }
data:        }
data:    }
info:    acs scale command OK
``` 

## <a name="next-steps"></a>Volgende stappen

- [Een cluster implementeren](container-service-deployment.md)