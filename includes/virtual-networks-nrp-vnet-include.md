## <a name="virtual-network"></a>Virtueel netwerk
Virtuele informatiebronnen netwerken (VNET) en subnetten definiëren een beveiligingsgrens voor werkbelasting in Azure uitgevoerd. Een VNet wordt gekenmerkt door een verzameling adresruimten, gedefinieerd als de CIDR-blokken. 

>[AZURE.NOTE] Netwerkbeheerders bent bekend met CIDR-notatie. Als u niet bekend bent met CIDR, [meer informatie over](http://whatismyipaddress.com/cidr).

![VNet met meerdere subnetten](./media/resource-groups-networking/Figure4.png)

VNets bevatten de volgende eigenschappen.

|Eigenschap|Beschrijving|Voorbeeldwaarden|
|---|---|---|
|**addressSpace**|Collectie van adresprefixen waaruit de VNet in de CIDR-notatie|192.168.0.0/16|
|**subnetten**|Verzameling subnetten van de VNet|Zie de onderstaande [subnetten](#Subnets) .|
|**IP-adres**|IP-adres is toegewezen aan een object. Dit is een alleen-lezen eigenschap.|104.42.233.77|

### <a name="subnets"></a>Subnetten
Een subnet is een onderliggende bron van een VNet en helpt bij het segmenten van adresruimten binnen een CIDR-blok met IP-adresprefixen definiëren. NIC's kunnen worden toegevoegd aan subnetten en verbonden met VMs, een connectiviteit voor verschillende werkbelastingen.

Subnetten zijn de volgende eigenschappen bevatten. 

|Eigenschap|Beschrijving|Voorbeeldwaarden|
|---|---|---|
|**addressPrefix**|Één adres-voorvoegsel van het subnet in CIDR-notatie|192.168.1.0/24|
|**networkSecurityGroup**|Toegepast op het subnet NSG|Zie [NSGs](#Network-Security-Group)|
|**routeTable**|Routetabel toegepast op het subnet|Zie [UDR](#Route-table)|
|**ipConfigurations**|Collectie gebruikt door NIC's die zijn verbonden met het subnet-IP-configruation objecten|Zie [UDR](#Route-table)|


Monster VNet in JSON-indeling:

    {
        "name": "TestVNet",
        "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet",
        "etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
        "type": "Microsoft.Network/virtualNetworks",
        "location": "westus",
        "tags": {
            "displayName": "VNet"
        },
        "properties": {
            "provisioningState": "Succeeded",
            "resourceGuid": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
            "addressSpace": {
                "addressPrefixes": [
                    "192.168.0.0/16"
                ]
            },
            "subnets": [
                {
                    "name": "FrontEnd",
                    "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                    "etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                    "properties": {
                        "provisioningState": "Succeeded",
                        "addressPrefix": "192.168.1.0/24",
                        "networkSecurityGroup": {
                            "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkSecurityGroups/NSG-BackEnd"
                        },
                        "routeTable": {
                            "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/routeTables/UDR-FrontEnd"
                        },
                        "ipConfigurations": [
                            {
                                "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICWEB1/ipConfigurations/ipconfig1"
                            },
                            ...]
                    }
                },
                ...]
        }
    }

### <a name="additional-resources"></a>Aanvullende bronnen

- Meer informatie vinden over [VNet](../articles/virtual-network/virtual-networks-overview.md).
- Lees de [REST API-documentatie](https://msdn.microsoft.com/library/azure/mt163650.aspx) voor VNets.
- Lees de [REST API-documentatie](https://msdn.microsoft.com/library/azure/mt163618.aspx) voor subnetten.