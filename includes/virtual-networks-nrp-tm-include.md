## <a name="traffic-manager-profile"></a>Verkeer van Verbindingsbeheer

Beheer van verkeer en de onderliggende eindpunt resource kunnen DNS-routering naar eindpunten in Azure en buiten Azure. Deze verdeling van het verkeer wordt geregeld door de routeringsmethoden beleid. Verkeer-beheer kunt ook eindpunt gezondheid te bewaken en verkeer op de juiste manier wordt gewijzigd op basis van de gezondheid van een eindpunt. 

| Eigenschap | Beschrijving |
|---|---|
|**trafficRoutingMethod**| mogelijke waarden zijn *prestaties*, *gewogen*en *prioriteit* | 
| **dnsConfig** | FQDN-naam voor het profiel | 
| **Protocol** | controle protocol, zijn mogelijke waarden *HTTP* en *HTTPS*|
| **Poort** | controle-aansluiting |  
| **Pad** | toezicht op pad |
| **Eindpunten** |  container voor eindpunt resources | 

### <a name="endpoint"></a>Eindpunt 

Een eindpunt is een onderliggende bron van een serviceprofiel verkeer. Vertegenwoordigt een service of website eindpunt aan welke gebruiker verkeer wordt verdeeld op basis van de geconfigureerde beleid in de bron van het verkeer van Verbindingsbeheer. 

| Eigenschap | Beschrijving | 
|---|---| 
| **Type** |  het type van het eindpunt, mogelijke waarden zijn *Einde Azure*en *Externe Endpoint*en *Eindpunt genest* | 
| **targetResourceId** |  openbare IP-adres van een website of -service-eindpunt. Dit kan een eindpunt Azure of extern zijn. | 
| **Gewicht** | eindpunt gewicht wordt gebruikt in het verkeerbeheer van. | 
| **Prioriteit** | de prioriteit van het eindpunt, gebruikt voor het definiëren van een failover-actie |

Voorbeeld van verkeer Manager in Json-indeling: 


        {
            "apiVersion": "[variables('tmApiVersion')]",
            "type": "Microsoft.Network/trafficManagerProfiles",
            "name": "VMEndpointExample",
            "location": "global",
            "dependsOn": [
                "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'), '0')]",
                "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'), '1')]",
                "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'), '2')]",
            ],
            "properties": {
                "profileStatus": "Enabled",
                "trafficRoutingMethod": "Weighted",
                "dnsConfig": {
                    "relativeName": "[parameters('dnsname')]",
                    "ttl": 30
                },
                "monitorConfig": {
                    "protocol": "http",
                    "port": 80,
                    "path": "/"
                },
                "endpoints": [
                    {
                        "name": "endpoint0",
                        "type": "Microsoft.Network/trafficManagerProfiles/azureEndpoints",
                        "properties": {
                            "targetResourceId": "[resourceId('Microsoft.Network/publicIPAddresses',concat(variables('publicIPAddressName'), 0))]",
                            "endpointStatus": "Enabled",
                            "weight": 1
                        }
                    },
                    {
                        "name": "endpoint1",
                        "type": "Microsoft.Network/trafficManagerProfiles/azureEndpoints",
                        "properties": {
                            "targetResourceId": "[resourceId('Microsoft.Network/publicIPAddresses',concat(variables('publicIPAddressName'), 1))]",
                            "endpointStatus": "Enabled",
                            "weight": 1
                        }
                    },
                    {
                        "name": "endpoint2",
                        "type": "Microsoft.Network/trafficManagerProfiles/azureEndpoints",
                        "properties": {
                            "targetResourceId": "[resourceId('Microsoft.Network/publicIPAddresses',concat(variables('publicIPAddressName'), 2))]",
                            "endpointStatus": "Enabled",
                            "weight": 1
                        }
                    }
                ]
            }
        }

 
## <a name="additional-resources"></a>Aanvullende bronnen

Lees de [REST API-documentatie voor verkeer-Manager](https://msdn.microsoft.com/library/azure/mt163664.aspx) voor meer informatie.
