<properties
    pageTitle="Hadoop, HBase of Storm clusters maken op Linux in HDInsight met krul en de REST Azure API | Microsoft Azure"
    description="Informatie over het maken HDInsight Linux gebaseerde clusters met krul, Azure Resource Manager templates en de Azure REST API. U kunt opgeven welk clustertype (Hadoop, HBase of Storm) of scripts gebruiken om aangepaste onderdelen te installeren."
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="10/11/2016"
    ms.author="larryfr"/>

#<a name="create-linux-based-clusters-in-hdinsight-using-curl-and-the-azure-rest-api"></a>Linux gebaseerde clusters maken in HDInsight met krul en de REST Azure API

[AZURE.INCLUDE [selector](../../includes/hdinsight-selector-create-clusters.md)]

De REST Azure-API kunt u van beheerbewerkingen uitvoeren op de services van het Azure-platform, met inbegrip van het maken van nieuwe bronnen zoals HDInsight Linux gebaseerde clusters. In dit document leert u het maken van een HDInsight cluster en de bijbehorende opslag configureren en vervolgens de sjabloon op de Azure REST API voor het maken van een nieuw HDInsight cluster implementeren met krul sjablonen Azure Resource Manager.

> [AZURE.IMPORTANT] Het aantal knooppunten werknemer (4) de stappen in dit document gebruiken voor een cluster HDInsight. Als u van plan op meer dan 32 werknemer knooppunten te maken van het cluster of bent door de schaal van het cluster nadat het is gemaakt, selecteert u de grootte van een hoofd knooppunt met minimaal 8 cores en 14GB ram.
>
> Zie voor meer informatie op het knooppunt formaten en bijbehorende kosten, [prijzen HDInsight](https://azure.microsoft.com/pricing/details/hdinsight/).

##<a name="prerequisites"></a>Vereisten

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


- **Azure een abonnement**. Zie [Azure krijg gratis proefperiode](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- __Azure CLI__. De CLI Azure wordt gebruikt voor het maken van een service principal, die vervolgens wordt gebruikt voor het genereren van verificatietokens voor aanvragen voor de Azure REST API.

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

- __cURL__. Dit hulpprogramma is beschikbaar via uw pakket managementsysteem, of kan worden gedownload van [http://curl.haxx.se/](http://curl.haxx.se/).

    > [AZURE.NOTE] Als u de opdrachten in dit document uitvoeren via PowerShell, moet u eerst verwijderen de `curl` alias wordt standaard gemaakt. Deze alias Invoke-WebRequest, PowerShell-cmdlet gebruikt in plaats van de krul bij gebruik van de `curl` van PowerShell-prompt de opdracht en wordt er een foutmelding voor veel van de opdrachten in dit document gebruikt.
    > 
    > U kunt deze alias verwijderen, gebruikt u de volgende uit de PowerShell-prompt:
    >
    > `Remove-item alias:curl`
    >
    > Zodra de alias is verwijderd, moet u gebruikmaken van de versie van de krul die u hebt geïnstalleerd op uw systeem zijn.

### <a name="access-control-requirements"></a>Access controle-eisen

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

##<a name="create-a-template"></a>Een sjabloon maken

Azure Resource Management sjablonen worden JSON-documenten die een beschrijving geven van een __resourcegroep__ en alle bronnen in deze (bijvoorbeeld HDInsight). Deze benadering op basis van sjabloon kunt u de resources die u nodig hebt voor HDInsight in een sjabloon opgeven en voor het beheren van wijzigingen in de groep als geheel tot en met- __implementaties__ die wijzigingen toepassen op de groep.

Sjablonen worden gewoonlijk geleverd in twee delen; de sjabloon zelf en een parameterbestand die u in de configuratie te vullen met specifieke waarden. Voor een voorbeeld, de naam van het cluster, admin naam en wachtwoord. Wanneer u rechtstreeks de REST API gebruikt, moet u deze in één bestand te combineren. De indeling van dit document JSON is:

    {
        "properties": {
            "template": {
                contents of template file
            },
            "mode": "deploymentmode",
            "Parameters": {
                contents of the parameters element from parameters file
            }
        }
    }

Het volgende is bijvoorbeeld een fusie van de sjabloon en parameters van [https://github.com/Azure/azure-quickstart-templates/tree/master/101-hdinsight-linux-ssh-password](https://github.com/Azure/azure-quickstart-templates/tree/master/101-hdinsight-linux-ssh-password), waarbij een Linux-gebaseerde clusters met een wachtwoord beveiligen de SSH-gebruikersaccount wordt gemaakt.

    {
        "properties": {
            "template": {
                "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
                "contentVersion": "1.0.0.0",
                "parameters": {
                    "location": {
                        "type": "string",
                        "allowedValues": ["Central US",
                        "East Asia",
                        "East US",
                        "Japan East",
                        "Japan West",
                        "North Europe",
                        "South Central US",
                        "Southeast Asia",
                        "West Europe",
                        "West US"],
                        "metadata": {
                            "description": "The location where all azure resources will be deployed."
                        }
                    },
                    "clusterType": {
                        "type": "string",
                        "allowedValues": ["hadoop",
                        "hbase",
                        "storm",
                        "spark"],
                        "metadata": {
                            "description": "The type of the HDInsight cluster to create."
                        }
                    },
                    "clusterName": {
                        "type": "string",
                        "metadata": {
                            "description": "The name of the HDInsight cluster to create."
                        }
                    },
                    "clusterLoginUserName": {
                        "type": "string",
                        "metadata": {
                            "description": "These credentials can be used to submit jobs to the cluster and to log into cluster dashboards."
                        }
                    },
                    "clusterLoginPassword": {
                        "type": "securestring",
                        "metadata": {
                            "description": "The password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
                        }
                    },
                    "sshUserName": {
                        "type": "string",
                        "metadata": {
                            "description": "These credentials can be used to remotely access the cluster."
                        }
                    },
                    "sshPassword": {
                        "type": "securestring",
                        "metadata": {
                            "description": "The password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
                        }
                    },
                    "clusterStorageAccountName": {
                        "type": "string",
                        "metadata": {
                            "description": "The name of the storage account to be created and be used as the cluster's storage."
                        }
                    },
                    "clusterWorkerNodeCount": {
                        "type": "int",
                        "defaultValue": 4,
                        "metadata": {
                            "description": "The number of nodes in the HDInsight cluster."
                        }
                    }
                },
                "variables": {
                    "defaultApiVersion": "2015-05-01-preview",
                    "clusterApiVersion": "2015-03-01-preview"
                },
                "resources": [{
                    "name": "[parameters('clusterStorageAccountName')]",
                    "type": "Microsoft.Storage/storageAccounts",
                    "location": "[parameters('location')]",
                    "apiVersion": "[variables('defaultApiVersion')]",
                    "dependsOn": [],
                    "tags": {
                        
                    },
                    "properties": {
                        "accountType": "Standard_LRS"
                    }
                },
                {
                    "name": "[parameters('clusterName')]",
                    "type": "Microsoft.HDInsight/clusters",
                    "location": "[parameters('location')]",
                    "apiVersion": "[variables('clusterApiVersion')]",
                    "dependsOn": ["[concat('Microsoft.Storage/storageAccounts/',parameters('clusterStorageAccountName'))]"],
                    "tags": {
                        
                    },
                    "properties": {
                        "clusterVersion": "3.2",
                        "osType": "Linux",
                        "clusterDefinition": {
                            "kind": "[parameters('clusterType')]",
                            "configurations": {
                                "gateway": {
                                    "restAuthCredential.isEnabled": true,
                                    "restAuthCredential.username": "[parameters('clusterLoginUserName')]",
                                    "restAuthCredential.password": "[parameters('clusterLoginPassword')]"
                                }
                            }
                        },
                        "storageProfile": {
                            "storageaccounts": [{
                                "name": "[concat(parameters('clusterStorageAccountName'),'.blob.core.windows.net')]",
                                "isDefault": true,
                                "container": "[parameters('clusterName')]",
                                "key": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('clusterStorageAccountName')), variables('defaultApiVersion')).key1]"
                            }]
                        },
                        "computeProfile": {
                            "roles": [{
                                "name": "headnode",
                                "targetInstanceCount": "2",
                                "hardwareProfile": {
                                    "vmSize": "Standard_D3"
                                },
                                "osProfile": {
                                    "linuxOperatingSystemProfile": {
                                        "username": "[parameters('sshUserName')]",
                                        "password": "[parameters('sshPassword')]"
                                    }
                                }
                            },
                            {
                                "name": "workernode",
                                "targetInstanceCount": "[parameters('clusterWorkerNodeCount')]",
                                "hardwareProfile": {
                                    "vmSize": "Standard_D3"
                                },
                                "osProfile": {
                                    "linuxOperatingSystemProfile": {
                                        "username": "[parameters('sshUserName')]",
                                        "password": "[parameters('sshPassword')]"
                                    }
                                }
                            }]
                        }
                    }
                }],
                "outputs": {
                    "cluster": {
                        "type": "object",
                        "value": "[reference(resourceId('Microsoft.HDInsight/clusters',parameters('clusterName')))]"
                    }
                }
            },
            "mode": "incremental",
            "Parameters": {
                "location": {
                    "value": "North Europe"
                },
                "clusterName": {
                    "value": "newclustername"
                },
                "clusterType": {
                    "value": "hadoop"
                },
                "clusterStorageAccountName": {
                    "value": "newstoragename"
                },
                "clusterLoginUserName": {
                    "value": "admin"
                },
                "clusterLoginPassword": {
                    "value": "changeme"
                },
                "sshUserName": {
                    "value": "sshuser"
                },
                "sshPassword": {
                    "value": "changeme"
                }
            }
        }
    }

In dit voorbeeld wordt in de stappen in dit document worden gebruikt. U moet de tijdelijke _waarden_ in de sectie __Parameters__ aan het einde van het document vervangen door de waarden die u wilt gebruiken voor uw cluster.

##<a name="login-to-your-azure-subscription"></a>Meld u aan bij uw abonnement Azure

Volg de stappen beschreven in [verbinding maken met een Azure-abonnement van de Azure-opdrachtregelinterface (CLI Azure)](../xplat-cli-connect.md) en maak verbinding met uw abonnement met behulp van de `azure login` opdracht.

##<a name="create-a-service-principal"></a>Een service principal maken

> [AZURE.NOTE] Deze stappen zijn een verkorte versie van de informatie in de sectie _verifiëren service principal met een wachtwoord - Azure CLI_ van het document [verificatie van een service principal Azure Resource Manager](../resource-group-authenticate-service-principal.md#authenticate-service-principal-with-password---azure-cli) . Deze stappen maakt u een nieuwe service principal die kunnen worden gebruikt voor het verifiëren van de REST API aanvragen Azure bronnen zoals een HDInsight-cluster maken.

1. Gebruik de volgende opdracht om uw Azure abonnementen vanaf de opdrachtprompt een sessie of de shell.

        azure account list
        
    Selecteer het abonnement dat u wilt gebruiken en noteer de __id-__ kolom in de lijst. Dit is de __abonnement-ID__ en in de meeste van de stappen in dit document worden gebruikt.

2. Maak een nieuwe toepassing in Azure Active Directory.

        azure ad app create --name "exampleapp" --home-page "https://www.contoso.org" --identifier-uris "https://www.contoso.org/example" --password <Your_Password>
        
    Vervang de waarden voor de `--name`, `--home-page`, en `--identifier-uris` met uw eigen waarden. Een wachtwoord opgeven voor de nieuwe vermelding van Active Directory.
    
    > [AZURE.NOTE] Nadat u deze toepassing voor de verificatie via een service principal, maakt de `--home-page` en `--identifier-uris` waarden niet nodig hebt om te verwijzen naar een webpagina die wordt gehost op internet. ze moeten alleen worden unieke URI's.
    
    Sla de __toepassings-id__ -waarde van de geretourneerde gegevens.
    
        data:    AppId:          4fd39843-c338-417d-b549-a545f584a745
        data:    ObjectId:       4f8ee977-216a-45c1-9fa3-d023089b2962
        data:    DisplayName:    exampleapp
        ...
        info:    ad app create command OK
    
3. Een service principal met de waarde van de __AppId__ eerder heeft gemaakt.

        azure ad sp create 4fd39843-c338-417d-b549-a545f584a745
        
     Sla de __Object-Id__ -waarde van de geretourneerde gegevens.
     
        info:    Executing command ad sp create
        - Creating service principal for application 4fd39843-c338-417d-b549-a545f584a74+
        data:    Object Id:        7dbc8265-51ed-4038-8e13-31948c7f4ce7
        data:    Display Name:     exampleapp
        data:    Service Principal Names:
        data:                      4fd39843-c338-417d-b549-a545f584a745
        data:                      https://www.contoso.org/example
        info:    ad sp create command OK
        
4. De rol __eigenaar__ toewijzen aan de service eerder principal met behulp van de __Object-ID__ -waarde geretourneerd. Ook moet u de __abonnement-ID__ die u eerder hebt aangeschaft.
    
        azure role assignment create --objectId 7dbc8265-51ed-4038-8e13-31948c7f4ce7 -o Owner -c /subscriptions/{SubscriptionID}/
        
    Nadat deze opdracht is voltooid, de belangrijkste service nu eigenaar toegang heeft tot de opgegeven abonnement-ID.

##<a name="get-an-authentication-token"></a>Een verificatietoken ophalen

1. De volgende gebruiken om de __Huurder-ID__ voor uw abonnement.

        azure account show -s <subscription ID>
        
    Zoeken naar de __Huurder-ID__van de geretourneerde gegevens.
    
        info:    Executing command account show
        data:    Name                        : MyAzureAccount
        data:    ID                          : 45a1014d-0f27-25d2-b838-b8f373d6d52e
        data:    State                       : Enabled
        data:    Tenant ID                   : 22f988bf-56f1-41af-91ab-3d7cd011db47
        data:    Is Default                  : true
        data:    Environment                 : AzureCloud
        data:    Has Certificate             : No
        data:    Has Access Token            : Yes
        data:    User name                   : myname@contoso.org
        data:    
        info:    account show command OK

2. Genereer een nieuwe beveiligingssleutel Azure REST API gebruiken.

        curl -X "POST" "https://login.microsoftonline.com/TenantID/oauth2/token" \
        -H "Cookie: flight-uxoptin=true; stsservicecookie=ests; x-ms-gateway-slice=productionb; stsservicecookie=ests" \
        -H "Content-Type: application/x-www-form-urlencoded" \
        --data-urlencode "client_id=AppID" \
        --data-urlencode "grant_type=client_credentials" \
        --data-urlencode "client_secret=password" \
        --data-urlencode "resource=https://management.azure.com/"
    
    __TenantID__, __toepassings-id__en __wachtwoord__ vervangen door de waarden verkregen of eerder gebruikt.

    Als deze aanvraag voltooid is, ontvangt u een reactie van de 200-serie en de hoofdtekst van het antwoord bevat een JSON-document.

    De JSON-document die door deze aanvraag bevat een element met de naam __access_token__; de waarde van dit element is het toegangstoken dat moet u verificatie de aanvragen in de volgende secties van dit document worden gebruikt.
    
        {
            "token_type":"Bearer",
            "expires_in":"3599",
            "expires_on":"1463409994",
            "not_before":"1463406094",
            "resource":"https://management.azure.com/","access_token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWoNBVGZNNXBPWWlKSE1iYTlnb0VLWSIsImtpZCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSJ9.eyJhdWQiOiJodHRwczovL21hbmFnZW1lbnQuYXp1cmUuY29tLyIsImlzcyI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzcyZjk4OGJmLTg2ZjEtNDFhZi05MWFiLTJkN2NkMDExZGI2Ny8iLCJpYXQiOjE0NjM0MDYwOTQsIm5iZiI6MTQ2MzQwNjA5NCwiZXhwIjoxNDYzNDA5OTk5LCJhcHBpZCI6IjBlYzcyMzM0LTZkMDMtNDhmYi04OWU1LTU2NTJiODBiZDliYiIsImFwcGlkYWNyIjoiMSIsImlkcCI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzcyZjk4OGJmLTg2ZjEtNDFhZi05MWFiLTJkN2NkMDExZGI0Ny8iLCJvaWQiOiJlNjgxZTZiMi1mZThkLTRkZGUtYjZiMS0xNjAyZDQyNWQzOWYiLCJzdWIiOiJlNjgxZTZiMi1mZThkLTRkZGUtYjZiMS0xNjAyZDQyNWQzOWYiLCJ0aWQiOiI3MmY5ODhiZi04NmYxLTQxYWYtOTFhYi0yZDdjZDAxMWRiNDciLCJ2ZXIiOiIxLjAifQ.nJVERbeDHLGHn7ZsbVGBJyHOu2PYhG5dji6F63gu8XN2Cvol3J1HO1uB4H3nCSt9DTu_jMHqAur_NNyobgNM21GojbEZAvd0I9NY0UDumBEvDZfMKneqp7a_cgAU7IYRcTPneSxbD6wo-8gIgfN9KDql98b0uEzixIVIWra2Q1bUUYETYqyaJNdS4RUmlJKNNpENllAyHQLv7hXnap1IuzP-f5CNIbbj9UgXxLiOtW5JhUAwWLZ3-WMhNRpUO2SIB7W7tQ0AbjXw3aUYr7el066J51z5tC1AK9UC-mD_fO_HUP6ZmPzu5gLA6DxkIIYP3grPnRVoUDltHQvwgONDOw"
        }

##<a name="create-a-resource-group"></a>Een resourcegroep maken

Gebruik de volgende een nieuwe resourcegroep te maken. U moet de groep eerst maken voordat u de bronnen, zoals de cluster HDInsight kunt maken. 

* Vervang __SubscriptionID__ door de abonnement-ID hebt ontvangen bij het maken van de service principal.
* Vervang __AccessToken__ door het toegangstoken in de vorige stap hebt ontvangen.
* __DataCenterLocation__ vervangen door het datacenter die u wenst te maken van de bronnengroep en bronnen, in. Bijvoorbeeld 'Zuid-centraal ons op'. 
* Vervang __ResourceGroupName__ met de naam die u wilt gebruiken voor deze groep:

```
curl -X "PUT" "https://management.azure.com/subscriptions/SubscriptionID/resourcegroups/ResourceGroupName?api-version=2015-01-01" \
    -H "Authorization: Bearer AccessToken" \
    -H "Content-Type: application/json" \
    -d $'{
"location": "DataCenterLocation"
}'
```

Als deze aanvraag voltooid is, ontvangt u een reactie van de 200-serie en de hoofdtekst van het antwoord bevat een JSON-document met informatie over de groep. De `"provisioningState"` element bevat een waarde van `"Succeeded"`.

##<a name="create-a-deployment"></a>Een implementatie maken

Gebruik de volgende voor de implementatie van de clusterconfiguratie (sjabloon en parameter waarden) aan de resourcegroep.

* __SubscriptionID__ en __AccessToken__ vervangen door de waarden die u eerder hebt gebruikt. 
* __ResourceGroupName__ vervangen door de naam van de resource-groep die u in de vorige sectie hebt gemaakt.
* __DeploymentName__ vervangen door de naam die u wilt gebruiken voor deze installatie.

```
curl -X "PUT" "https://management.azure.com/subscriptions/SubscriptionID/resourcegroups/ResourceGroupName/providers/microsoft.resources/deployments/DeploymentName?api-version=2015-01-01" \
-H "Authorization: Bearer AccessToken" \
-H "Content-Type: application/json" \
-d "{set your body string to the template and parameters}"
```

> [AZURE.NOTE] Als u de JSON-document met de parameters in een bestand en de sjabloon hebt opgeslagen, kunt u het volgende in plaats van `-d "{ template and parameters}"`:
>
> `--data-binary "@/path/to/file.json"`

Als deze aanvraag voltooid is, ontvangt u een reactie van de 200-serie en de hoofdtekst van het antwoord bevat een JSON-document met informatie over de werking van de implementatie.

> [AZURE.IMPORTANT] Houd er rekening mee dat de implementatie is ingediend, maar op dit moment niet is voltooid. Het kan enkele minuten duren, meestal ongeveer 15, voor de installatie te voltooien.

##<a name="check-the-status-of-a-deployment"></a>Controleer de status van een implementatie

Controleer de status van de implementatie, gebruik het volgende:

* __SubscriptionID__ en __AccessToken__ vervangen door de waarden die u eerder hebt gebruikt. 
* __ResourceGroupName__ vervangen door de naam van de resource-groep die u in de vorige sectie hebt gemaakt.

```
curl -X "GET" "https://management.azure.com/subscriptions/SubscriptionID/resourcegroups/ResourceGroupName/providers/microsoft.resources/deployments/DeploymentName?api-version=2015-01-01" \
-H "Authorization: Bearer AccessToken" \
-H "Content-Type: application/json"
```

Dit levert een JSON-document met informatie over de werking van de implementatie van informatie. De `"provisioningState"` element bevat de status van de implementatie; Als dit de waarde van `"Succeeded"`, en vervolgens de installatie is voltooid. Op dit moment moet het cluster worden gebruikt.

##<a name="next-steps"></a>Volgende stappen

Nu dat u een HDInsight-cluster hebt gemaakt, gebruikt u de volgende voor meer informatie over het werken met het cluster. 

###<a name="hadoop-clusters"></a>Hadoop clusters

* [Gebruik component met HDInsight](hdinsight-use-hive.md)
* [Varken met HDInsight gebruiken](hdinsight-use-pig.md)
* [MapReduce gebruiken met HDInsight](hdinsight-use-mapreduce.md)

###<a name="hbase-clusters"></a>HBase clusters

* [Aan de slag met HBase op HDInsight](hdinsight-hbase-tutorial-get-started-linux.md)
* [Ontwikkelen van Java-toepassingen voor HBase op HDInsight](hdinsight-hbase-build-java-maven-linux.md)

###<a name="storm-clusters"></a>Storm-clusters

* [Ontwikkelen van Java topologieën voor Storm op HDInsight](hdinsight-storm-develop-java-topology.md)
* [Python-componenten gebruiken in de Storm op HDInsight](hdinsight-storm-develop-python-topology.md)
* [Implementeren en topologieën met Storm op HDInsight controleren](hdinsight-storm-deploy-monitor-topology-linux.md)
