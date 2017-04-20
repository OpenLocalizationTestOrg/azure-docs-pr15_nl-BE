<properties
   pageTitle="Azure architectuur referentie - IaaS: Maken van een Active Directory-forest met bronnen in Azure | Microsoft Azure"
   description="Het maken van een vertrouwd Active Directory-domein in Azure."
   services="guidance,vpn-gateway,expressroute,load-balancer,virtual-network,active-directory"
   documentationCenter="na"
   authors="telmosampaio"
   manager="christb"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="telmos"/>

# <a name="creating-a-active-directory-directory-services-adds-resource-forest-in-azure"></a>Maken van een forest met bronnen van Active Directory Directory Services (ADDS) in Azure

[AZURE.INCLUDE [pnp-RA-branding](../../includes/guidance-pnp-header-include.md)]

In dit artikel wordt beschreven hoe u voor het maken van een Active Directory-domein in Azure dat los staat van, maar door vertrouwde domeinen in uw forest op gebouwen.

> [AZURE.NOTE] Azure heeft twee verschillende modellen: [Resource Manager] [ resource-manager-overview] en klassiek. Deze referentiearchitectuur gebruikt resourcemanager, wordt aangeraden voor nieuwe implementaties.

Een organisatie met Active Directory (AD) van gebouwen hebben een forest die bestaat uit veel verschillende domeinen. Bijvoorbeeld, kunt u afzonderlijke domeinen voor verschillende afdelingen of suborganizations maken of nieuwe domeinen kunnen worden toegevoegd als gevolg van de overname of de fusie van andere organisaties. U kunt domeinen isolatie tussen functionele gebieden die moeten worden geïsoleerd, mogelijk om veiligheidsredenen bieden, maar u kunt gegevens uitwisselen tussen domeinen door vertrouwensrelaties tot stand te brengen.

Een organisatie die gebruikmaakt van afzonderlijke domeinen kunt profiteren van Azure een of meer van deze domeinen verplaatst naar de cloud. U kunt ook een organisatie mogelijk wilt houden alle cloud bronnen logisch afzonderlijk van de aangehouden op-ruimten en informatie over de cloud opslaan in hun eigen directory, in een domein ook wordt gehouden in de cloud.

U kunt implementeren van Active Directory in Azure op verschillende manieren, zoals beschreven in de artikelen in [Active Directory uitbreiden naar Azure] [ extending-ad-to-azure] en [Azure Active Directory implementeert][implementing-aad]. Dit document is gericht op één specifieke scenario: maken van een domein in de cloud die verschilt van alle andere domeinen in gebouwen gehouden, maar dat een vertrouwensrelatie met domeinen voor gebouwen kan hebben. 

Typische use-cases voor deze architectuur zijn:

- Het onderhouden van beveiliging scheiding voor objecten en identiteiten in de cloud bewaard.

- Afzonderlijke domeinen migreren op ruimten naar de cloud.

## <a name="architecture-diagram"></a>Architectuurdiagram

In het volgende diagram worden de belangrijke onderdelen van deze architectuur (*Klik om in te zoomen*). Lees voor meer informatie over de elementen beschikbaar [ter uitvoering van een beveiligde hybride netwerkarchitectuur in Azure] [ implementing-a-secure-hybrid-network-architecture] en de [uitvoering van een beveiligde hybride netwerkarchitectuur met toegang tot het Internet in Azure][implementing-a-secure-hybrid-network-architecture-with-internet-access]:

[! [0]][0]

- **Netwerk van gebouwen.** Het netwerk op gebouwen bevat een eigen AD forest en de domeinen.

- **AD-Servers.** Dit zijn domeincontrollers implementeren van directoryservices (AD DS) die als VMs in de cloud. Deze servers als host een forest met één of meer domeinen die los staat van de zich op-ruimten.

- **Eenzijdige vertrouwensrelatie.** Het voorbeeld in het diagram ziet een eenrichtingsvertrouwensrelatie instellen tussen het domein in de cloud aan het domein op gebouwen. Deze relatie kan gebruikers toegang krijgen tot bronnen in het domein in de cloud, maar niet andersom op gebouwen. Dit is een veelvoorkomend geval. Echter, kunt u een vertrouwensrelatie in twee richtingen als cloud gebruikers ook toegang tot bronnen voor gebouwen.

- **Active Directory-subnet.** De AD DS-servers worden gehost in een apart subnet. NSG-regels beschermen de AD DS-servers en biedt een firewall tegen verkeer uit onverwachte bronnen.

- **Web tier subnet** **Business tier subnet**en **gegevens trapsgewijs subnet**. Deze subnetten host de servers en onderdelen die applicaties in de cloud. Zie voor meer informatie, [VMs uitvoeren voor een N-lagige architectuur op Azure][running-VMs-for-an-N-tier-architecture-on-Azure]. De bronnen en het VMs in dit subnet bevinden zich in het domein van de wolk.

- **Azure-Gateway**. De gateway Azure biedt een verbinding tussen het netwerk van gebouwen en de Azure-VNet. Dit is een [VPN-verbinding] [ azure-vpn-gateway] of [Azure ExpressRoute][azure-expressroute]. Zie voor meer informatie, [ter uitvoering van een beveiligde hybride netwerkarchitectuur in Azure][implementing-a-secure-hybrid-network-architecture].

## <a name="recommendations"></a>Aanbevelingen

Deze sectie bevat een lijst met aanbevelingen op basis van de essentiële onderdelen die nodig zijn voor de basisarchitectuur implementeren. Deze aanbevelingen hebben betrekking op:

- ADDS instellingen, en

- Vertrouwen relatie configuratie.

Mogelijk hebt u aanvullende of afwijkende eisen van de hier beschreven. Kunt u de artikelen in deze sectie als uitgangspunt voor het aanpassen van de architectuur voor uw eigen systeem te nemen.

### <a name="adds-recommendations"></a>ADDS aanbevelingen

Raadpleeg het document [Uitbreiden van Active Directory naar Azure]voor specifieke aanbevelingen over het implementeren van Active Directory in de cloud,[extending-ad-to-azure]. Het artikel [richtlijnen voor het implementeren van Windows Server Active Directory Azure virtuele Machines] [ ad-azure-guidelines] bevat meer gedetailleerde informatie.

### <a name="trust-recommendations"></a>Aanbevelingen vertrouwen

De domeinen in de lokalen bevinden zich in een ander forest van de domeinen in de cloud. Domeinen in de cloud moeten het aanmeldingsdomein in het forest op ruimten vertrouwen zodat de verificatie van gebruikers in de wolk voor gebouwen. Op dezelfde manier als de cloud een aanmeldingsdomein voor externe gebruikers biedt, kan het nodig zijn voor het forest op het bedrijf naar de cloud domein vertrouwen.

Door het [maken van forest-vertrouwensrelaties]kunt u vertrouwensrelaties op forest-niveau maken[creating-forest-trusts], of op het domeinniveau door [externe vertrouwensrelaties te maken][creating-external-trusts]. Een niveau forest-vertrouwensrelatie maakt een relatie tussen alle domeinen in twee forests. Een extern domein niveau vertrouwensrelatie maakt alleen een relatie tussen twee opgegeven domeinen. Maak alleen externe niveau vertrouwensrelaties tussen domeinen in verschillende forests.

Vertrouwensrelaties kunnen worden unidirectionele (eenrichtingsverkeer) of met bidirectionele (tweerichtings):

- Een eenrichtingsvertrouwensrelatie kan gebruikers in een domein of forest (bekend als de *binnenkomende* domein of forest) toegang te krijgen tot de bronnen die in een ander ( *Uitgaand* domein of forest). 

- Een vertrouwensrelatie in twee richtingen kan gebruikers in het domein of forest kunnen toegang krijgen tot bronnen in het andere.

De volgende tabel bevat een overzicht van de vertrouwensrelatie configuraties voor sommige eenvoudige scenario's:

| Scenario | Vertrouwen in gebouwen | Wolk vertrouwen |
|----------|-------------------|-------------|
| Gebruikers moeten toegang hebben tot bronnen in de cloud, op locatie, maar niet andersom | Binnenkomend (eenrichtingsverkeer), | Eenzijdig, uitgaande |
| Gebruikers in de wolk moeten toegang hebben tot bronnen in lokalen, maar niet andersom | Eenzijdig, uitgaande | Binnenkomend (eenrichtingsverkeer), |
| Gebruikers in de cloud en op ruimten is vereist voor toegang tot bronnen in de cloud en in gebouwen gehouden | Twee richtingen, inkomende en uitgaande | Twee richtingen, inkomende en uitgaande |

## <a name="security-considerations"></a>Beveiligingsoverwegingen

Niveau forest-vertrouwensrelaties zijn transitief. Als u een niveau forest-vertrouwensrelatie tussen een forest op gebouwen en een forest in de cloud, deze vertrouwensrelatie uitgebreid naar andere nieuwe domeinen in een forest wordt gemaakt. Als u domeinen gebruiken om scheiding voor beveiligingsdoeleinden, kunt u de vertrouwensrelaties maken op domeinniveau alleen. Niveau vertrouwensrelaties tussen domeinen zijn niet-transitief.

Zie de sectie *Overwegingen over de beveiliging* in [Active Directory uitbreiden naar Azure]voor AD-specifieke aspecten van de beveiliging,[extending-ad-to-azure].

## <a name="availability-considerations"></a>Overwegingen met betrekking tot beschikbaarheid

Ten minste twee domeincontrollers in elk domein worden geïmplementeerd. Hiermee kunt automatische replicatie tussen servers. Maak een beschikbaarheid voor de VMs die fungeert als AD servers verwerken elk domein ingesteld. Zorg ervoor dat er ten minste twee servers in de set. 

Ook kunt u een of meer servers in elk domein aanwijzen als [stand-by operations-masters] [ standby-operations-masters] in het geval de verbinding met een server die fungeert als een FSMO-functie is mislukt.

## <a name="scalability-considerations"></a>Overwegingen voor schaalbaarheid

AD wordt automatisch aangepast voor domeincontrollers die deel van hetzelfde domein uitmaken. Aanvragen worden gedistribueerd over alle domeincontrollers in een domein. U kunt een andere domeincontroller toevoegen en worden automatisch gesynchroniseerd met het domein. Configureer een aparte load balancer rechtstreeks verkeer naar domeincontrollers binnen het domein. Zorg ervoor dat alle domeincontrollers beschikt over voldoende geheugen en opslag van middelen voor het verwerken van de database. Alle domeincontroller VMs dezelfde grootte maken.

## <a name="management-and-monitoring-considerations"></a>Beheer en bewaking van de overwegingen

Zie voor informatie over het beheer en bewaking van de overwegingen, de gelijkwaardige secties in [Active Directory uitbreiden naar Azure][extending-ad-to-azure]. 

Zie [Active Directory controleren]voor meer informatie,[monitoring_ad]. U kunt hulpprogramma's zoals [Microsoft Systems Center] installeren[ microsoft_systems_center] op een server monitoring in het beheer van subnet waarmee deze taken uitvoeren. 

## <a name="solution-components"></a>Onderdelen van oplossingen

Een oplossing voorbeeldscript, [Deploy ReferenceArchitecture.ps1][solution-script], is beschikbaar die u gebruiken kunt voor het implementeren van de architectuur die volgt op de aanbevelingen in dit artikel beschreven. Dit script maakt gebruik van sjablonen Azure Resource Manager. De sjablonen zijn beschikbaar als een set van fundamentele bouwstenen, die elk een bepaalde actie, zoals het maken van een VNet of een NSG configureren uitvoert. Het doel van het script is goedkeuringen door sjabloonimplementatie.

De sjablonen zijn parameters met de parameters die in aparte JSON bestanden bewaard. U kunt de parameters in de volgende bestanden voor het configureren van het aan uw eigen vereisten aanpassen. U hoeft niet te wijzigen van de sjablonen zelf. U moet de schema's van de objecten in de parameterbestanden niet wijzigen.

Wanneer u sjablonen bewerkt, objecten maken die de naamconventies die worden beschreven in de [Conventies aanbevolen voor bronnen Azure][naming-conventions].

De oplossing maakt en configureert u een omgeving in de cloud die een domein met de naam *treyresearch.com implementeert*. Deze omgeving omvat de ADDS subnet en servers, DMZ weblaag, business tier en laag voor gegevenstoegang, VPN-gateway en management-laag. De oplossing bevat ook een optionele configuratie voor het maken van een gesimuleerde op gebouwen-omgeving met een eigen domein *contoso.com*. De oplossing bevat scripts waarmee u een vertrouwensrelatie tot stand te brengen tussen deze domeinen waarmee gebruikers toegang krijgen tot objecten in het domein *treyresearch.com* in de cloud op locatie.

In de volgende secties beschrijven de elementen van het pand op en configuraties van de wolk.

### <a name="on-premises-components"></a>Onderdelen van gebouwen

>[AZURE.NOTE] Deze onderdelen zijn niet de primaire focus van de architectuur wordt beschreven in dit document, en gewoon voor geeft u de gelegenheid voor het testen van de cloud-omgeving veilig, in plaats van een echte productieomgeving. Daarom in deze sectie alleen geeft een overzicht van de belangrijkste parameterbestanden. Kunt u instellingen voor de IP-adressen of de grootte van het VMs, maar het is aan te raden om veel van de andere parameters ongewijzigd laat.

Deze omgeving bestaat uit een AD forest het domein *contoso.com* . Het domein bevat twee ADDS servers met IP-adressen 192.168.0.4 en 192.168.0.5. Deze twee servers ook de DNS-service worden uitgevoerd. De lokale administrator-account op beide VMs heet `testuser` wachtwoord `AweS0me@PW`. Daarnaast stelt de configuratie van een VPN-gateway voor de verbinding met de VNet in de cloud. U kunt de configuratie wijzigen door het bewerken van de volgende JSON bestanden in de [**parameters/lokale**] [ on-premises-folder] map:

- ** [virtualNetwork.parameters.json][on-premises-vnet-parameters]**. Dit bestand definieert de netwerk-adresruimte voor het milieu op het bedrijf.

- ** [virtualMachines adds.parameters.json][on-premises-virtualmachines-adds-parameters]**. Dit bestand bevat de configuratie voor het in lokalen VMs ADDS hostingservices. Twee *standaard-DS3-v2* VMs worden standaard gemaakt.

- ** [virtualNetworkGateway.parameters.json] [ on-premises-virtualnetworkgateway-parameters] ** en ** [connection.parameters.json][on-premises-connection-parameters]**. Deze bestanden bevatten de instellingen voor de VPN-verbinding op de Azure VPN-gateway in de cloud, met inbegrip van de gedeelde sleutel moet worden gebruikt voor het beveiligen van verkeer via de gateway worden verzonden.

De overige bestanden in de map bevat de configuratie-informatie voor het maken van het domein op gebouwen (*contoso.com*) met behulp van deze infrastructuur. U kunt toevoegen, DNS, setup installeert en maakt het forest op gebouwen.

De oplossing wordt ook het volgende script met de naam [binnenkomende trust.ps1][incoming-trust], die wordt uitgevoerd op een computer in het domein op ruimten:

```Powershell
# Run the following powershell script in ra-adtrust-onpremise-ad-vm1 (ip 192.168.0.4)

$TrustedDomainName = "contoso.com"
#$TrustedDomainDnsIpAddresses = "192.168.0.4,192.168.0.5"

$TrustingDomainName = "treyresearch.com"
$TrustingDomainDnsIpAddresses = "10.0.4.4,10.0.4.5"

$ForwardIpAddress  = $TrustingDomainDnsIpAddresses
$ForwardDomainName = $TrustingDomainName

$IpAddresses = @()
foreach($address in $ForwardIpAddress.Split(',')){
    $IpAddresses += [IPAddress]$address.Trim()
}
Add-DnsServerConditionalForwarderZone -Name "$ForwardDomainName" -ReplicationScope "Domain" -MasterServers $IpAddresses

netdom trust $TrustingDomainName /d:$TrustedDomainName /add
```

Dit script voegt u de IP-adressen van de AD DS-servers in de cloud (Zie de volgende sectie) naar de lokale DNS-service, en gebruikt u vervolgens de [netdom] [ netdom] opdracht voor het maken van een binnenkomende eenrichtingsvertrouwensrelatie uit het domein in de cloud (*treyresearch.com*).

### <a name="cloud-components"></a>Onderdelen van de cloud

Deze onderdelen vormen de kern van deze architectuur. Deze setup de infrastructuur voor het domein *treyresearch.com* en de vertrouwensrelaties maken met het domein van lokalen *contoso.com* . De [**parameters/azure**] [ azure-folder] map bevat de volgende parameterbestanden voor het configureren van deze onderdelen:

- ** [virtualNetwork.parameters.json][vnet-parameters]**. Dit bestand definieert de structuur van de VNet voor de VMs en andere onderdelen in de cloud. Het bevat de instellingen, zoals de naam, adres, subnetten en de adressen van de DNS-servers vereist. De DNS-adressen uit dit voorbeeld verwijst de IP-adressen van DNS-servers op de gebouwen en de standaard Azure DNS-server. Deze adressen om te verwijzen naar uw eigen DNS-instellingen als u niet de voorbeeldomgeving op ruimten te wijzigen:
    
    ```json
    "virtualNetworkSettings": {
      "value": {
        "name": "ra-adtrust-vnet",
        "resourceGroup": "ra-adtrust-network-rg",
        "addressPrefixes": [
          "10.0.0.0/16"
        ],
        "subnets": [
          {
            "name": "dmz-private-in",
            "addressPrefix": "10.0.0.0/27"
          },
          {
            "name": "dmz-private-out",
            "addressPrefix": "10.0.0.32/27"
          },
          {
            "name": "dmz-public-in",
            "addressPrefix": "10.0.0.64/27"
          },
          {
            "name": "dmz-public-out",
            "addressPrefix": "10.0.0.96/27"
          },
          {
            "name": "mgmt",
            "addressPrefix": "10.0.0.128/25"
          },
          {
            "name": "GatewaySubnet",
            "addressPrefix": "10.0.255.224/27"
          },
          {
            "name": "web",
            "addressPrefix": "10.0.1.0/24"
          },
          {
            "name": "biz",
            "addressPrefix": "10.0.2.0/24"
          },
          {
            "name": "data",
            "addressPrefix": "10.0.3.0/24"
          },
          {
            "name": "adds",
            "addressPrefix": "10.0.4.0/27"
          }
        ],
        "dnsServers": [
          "10.0.4.4",
          "10.0.4.5",
          "168.63.129.16"
        ]
      }
    }
    ```

- ** [virtualMachines adds.parameters.json ] [ virtualmachines-adds-parameters] **. Dit bestand wordt geconfigureerd voor het VMs ADDS uitgevoerd in de cloud. De configuratie bestaat uit twee VMs. Wijzig de gebruikersnaam admin en het wachtwoord in de `virtualMachineSettings` sectie, en u kunt desgewenst zo wijzigen dat de grootte van de VM aan de vereisten van het domein:

    Zie voor meer informatie de [Active Directory uitbreiden naar Azure][extending-ad-to-azure].
    
    ```json
    "virtualMachinesSettings": {
      "value": {
        "namePrefix": "ra-adtrust-ad",
        "computerNamePrefix": "aad",
        "size": "Standard_DS3_v2",
        "osType": "Windows",
        "adminUsername": "testuser",
        "adminPassword": "AweS0me@PW",
        "osAuthenticationType": "password",
        "nics": [
          {
            "isPublic": "false",
            "subnetName": "adds",
            "privateIPAllocationMethod": "Static",
            "startingIPAddress": "10.0.4.4",
            "enableIPForwarding": false,
            "dnsServers": [
            ],
            "isPrimary": "true"
          }
        ],
        "imageReference": {
          "publisher": "MicrosoftWindowsServer",
          "offer": "WindowsServer",
          "sku": "2012-R2-Datacenter",
          "version": "latest"
        },
        "dataDisks": {
          "count": 1,
          "properties": {
            "diskSizeGB": 127,
            "caching": "None",
            "createOption": "Empty"
          }
        },
        "osDisk": {
          "caching": "ReadWrite"
        },
        "extensions": [
        ],
        "availabilitySet": {
          "useExistingAvailabilitySet": "No",
          "name": "ra-adtrust-as"
        }
      }
    },
    "virtualNetworkSettings": {
      "value": {
        "name": "ra-adtrust-vnet",
        "resourceGroup": "ra-adtrust-network-rg"
      }
    },
    "buildingBlockSettings": {
      "value": {
        "storageAccountsCount": 2,
        "vmCount": 2,
        "vmStartIndex": 1
      }
    }
    ```

- ** [Toevoegen-voegt-domein-controller.parameters.json][add-adds-domain-controller-parameters]**. Dit bestand bevat de instellingen voor het maken van het *treyresearch.com* domein beslaat de servers toevoegen. Aangepaste extensies die tot stand brengen van het domein en de servers toevoegen aan toe te voegen wordt gebruikt. Tenzij u extra ADDS servers (in dat geval moet u deze toevoegen aan de `vms` matrix), de namen wijzigen van de standaardinstelling of wilt maken van een domein met een andere naam die u niet nodig om dit bestand te wijzigen.

- ** [virtualNetworkGateway.parameters.json][virtualnetworkgateway-parameters]**. Dit bestand bevat de instellingen voor de Azure VPN-gateway maken in de cloud gebruikt voor verbinding met het netwerk op gebouwen. U moet wijzigen de `sharedKey` de waarde in de `connectionsSettings` sectie aangepast aan die van het VPN-apparaat voor op gebouwen. Zie voor meer informatie, [een hybride netwerkarchitectuur met Azure en On-premises VPN-implementatie van][hybrid-azure-on-prem-vpn].

- ** [dmz private.parameters.json] [ dmz-private-parameters] ** en ** [dmz public.parameters.json ] [ dmz-public-parameters] **. Deze bestanden configureren in de binnenkomende (openbaar) en uitgaande (particuliere) zijden van het VMs bestaat uit de DMZ, bescherming van de servers in de cloud. Zie voor meer informatie over deze elementen en hun configuratie, [implementatie van een DMZ tussen Azure en Internet][implementing-a-secure-hybrid-network-architecture-with-internet-access].

- ** [loadBalancer web.parameters.json][loadBalancer-web-parameters]**, ** [loadBalancer biz.parameters.json][loadBalancer-biz-parameters]**, en ** [loadBalancer data.parameters.json][loadBalancer-data-parameters]**. Deze parameters-bestanden bevatten de VM-specificaties voor de toegang tot web-, business- en onderlaag en configureren van netwerktaakverdeling voor elke laag. Dit zijn de VMs die host web apps en databases, en de werklast business uitvoeren voor de organisatie. Het VMs in de weblaag worden toegevoegd aan het domein in de cloud met behulp van de instellingen die zijn opgegeven de ** [web vm-domein join.parameters.json] [ web-vm-domain-join-parameters] ** bestand.

    Elk bestand bevat twee sets van configuratieparameters. De `virtualMachineSettings` gedeelte definieert de VMs die als host van de AD FS-service in de cloud. Standaard maakt het script twee van deze VMs in bepaalde beschikbaarheid. De volgende fragmenten tonen de relevante delen van het bestand *loadBalancer-web.parameters.json* :

    ```json
    "virtualMachinesSettings": {
      "value": {
        "namePrefix": "ra-adtrust-web",
        "computerNamePrefix": "web",
        "size": "Standard_DS1_v2",
        "osType": "windows",
        "adminUsername": "testuser",
        "adminPassword": "AweS0me@PW",
        "osAuthenticationType": "password",
        "nics": [
          {
            "isPublic": "false",
            "subnetName": "web",
            "privateIPAllocationMethod": "Dynamic",
            "isPrimary": "true",
            "enableIPForwarding": false,
            "dnsServers": [ ]
          }
        ],
        "imageReference": {
          "publisher": "MicrosoftWindowsServer",
          "offer": "WindowsServer",
          "sku": "2012-R2-Datacenter",
          "version": "latest"
        },
        "dataDisks": {
          "count": 1,
          "properties": {
            "diskSizeGB": 128,
            "caching": "None",
            "createOption": "Empty"
          }
        },
        "osDisk": {
          "caching": "ReadWrite"
        },
        "extensions": [ ],
        "availabilitySet": {
          "useExistingAvailabilitySet": "No",
          "name": "ra-adtrust-web-vm-as"
        }
      }
    },
    ...
    "buildingBlockSettings": {
      "value": {
        "storageAccountsCount": 2,
        "vmCount": 2,
        "vmStartIndex": 1
      }
    }
    ````
    Desgewenst kunt u de grootte en aantal VMs in elke laag wijzigen.

    De `loadBalancerSettings` vindt u de beschrijving van de verdeling van de belasting voor deze VMs. Verdeling van de belasting wordt verkeer dat wordt weergegeven op poort 80 (HTTP) en poort 443 (HTTPS) doorgegeven aan een of andere van de VMs. 

    >[AZURE.NOTE] De regel voor poort 80 gebruikt een TCP-verbinding in plaats van HTTP. Dit is omdat de installatie van IIS op de weblaag is geconfigureerd voor Windows-verificatie alleen ondersteuning. Anonieme verificatie is uitgeschakeld. *Ping* willen mislukt poort 80 via een HTTP-verbinding met een (niet-gemachtigde) fout 401, dat alleen met behulp van een TCP-verbinding wordt gedetecteerd of de poort actief is:

    ```json
    "loadBalancerSettings": {
      "value": {
        "name": "ra-adtrust-web-lb",
        "frontendIPConfigurations": [
          {
            "name": "ra-adtrust-web-lb-fe",
            "loadBalancerType": "internal",
            "internalLoadBalancerSettings": {
              "privateIPAddress": "10.0.1.254",
              "subnetName": "web"
            }
          }
        ],
        "backendPools": [
          {
            "name": "ra-adtrust-web-lb-bep",
            "nicIndex": 0
          }
        ],
        "loadBalancingRules": [
          {
            "name": "http-rule",
            "frontendPort": 80,
            "backendPort": 80,
            "protocol": "Tcp",
            "backendPoolName": "ra-adtrust-web-lb-bep",
            "frontendIPConfigurationName": "ra-adtrust-web-lb-fe",
            "probeName": "http-probe",
            "enableFloatingIP": false
          },
          {
            "name": "https-rule",
            "frontendPort": 443,
            "backendPort": 443,
            "protocol": "Tcp",
            "backendPoolName": "ra-adtrust-web-lb-bep",
            "frontendIPConfigurationName": "ra-adtrust-web-lb-fe",
            "probeName": "https-probe",
            "enableFloatingIP": false
          }
        ],
        "probes": [
          {
            "name": "http-probe",
            "port": 80,
            "protocol": "Tcp",
            "requestPath": null
          },
          {
            "name": "https-probe",
            "port": 443,
            "protocol": "Tcp",
            "requestPath": null
          }
        ],
        "inboundNatRules": [ ]
      }
    }
    ```

- ** [virtualMachines mgmt.parameters.json][virtualMachines-mgmt-parameters]**. Dit bestand bevat de configuratie voor het vak sprong/beheer VMs. Het kan alleen aanmeldings- en beheertoegang tot de VMs in het web, zakelijke en lagen van gegevens uit het vak sprong krijgen. Het script maakt een enkele *Standard_DS1_v2* VM, maar kunt u dit bestand groter of meer VMs maken als u de werkbelasting management is waarschijnlijk beduidend zal zijn.

De configuratie worden ook gebruikt voor de [uitgaande trust.ps1] [ outgoing-trust] script voor het maken van een uitgaande eenrichtingsvertrouwensrelatie met het domein *contoso.com* hieronder weergegeven:

```powershell
# prerequiste: 
# You need to first run incoming-trust.ps1 in ra-adtrust-onpremise-ad-vm1 (ip 192.168.0.4)
# Then,
# Run the following powershell script in ra-adtrust-ad-vm1 (ip 10.0.4.4)

$TrustedDomainName = "contoso.com"
$TrustedDomainDnsIpAddresses = "192.168.0.4,192.168.0.5"

#$TrustingDomainName = "treyresearch.com"
#$TrustingDomainDnsIpAddresses = "10.0.4.4,10.0.4.5"

$ForwardIpAddress  = $TrustedDomainDnsIpAddresses
$ForwardDomainName = $TrustedDomainName

$IpAddresses = @()
foreach($address in $ForwardIpAddress.Split(',')){
    $IpAddresses += [IPAddress]$address.Trim()
}
Add-DnsServerConditionalForwarderZone -Name "$ForwardDomainName" -ReplicationScope "Domain" -MasterServers $IpAddresses

#netdom trust $TrustingDomainName /d:$TrustedDomainName /add
```

Dit script is vergelijkbaar met het *inkomende trust.ps1* script dat eerder is beschreven. Wordt de IP-adressen van de bedrijfsruimten van AD DS-servers naar de lokale DNS-service, en gebruikt u vervolgens de [netdom] [ netdom] opdracht voor het maken van uitgaande vertrouwensrelatie tot stand brengen.

## <a name="solution-deployment"></a>Implementatie van oplossingen

De oplossing wordt ervan uitgegaan dat de volgende vereisten:

- U hebt een Azure abonnement waar kunt u resourcegroepen maken.

- U hebt gedownload en geïnstalleerd van de meest recente versie van Azure Powershell. Zie [hier] [ azure-powershell-download] voor instructies.

Het script uitvoeren dat de oplossing implementeert:

1. Verplaatsen naar een geschikte map op uw lokale computer en maak de volgende submappen:

    - Scripts

    - Scripts/Parameters

    - Parameters-scripts/lokale

    - Parameters-scripts/Azure

2. Downloaden van de [Implementatie ReferenceArchitecture.ps1] [ solution-script] bestand in de map Scripts

3. Downloaden van de inhoud van de [parameters/lokale] [ on-premises-folder] map naar de map Parameters-Scripts/lokale:

4. De inhoud van de [parameters/azure] downloaden[ azure-folder] map naar de map Parameters-Scripts/Azure.

5. Het bestand distribueren ReferenceArchitecture.ps1 in de map Scripts bewerken en wijzigt u de volgende regels als u wilt aangeven welke resourcegroepen die moeten worden gemaakt of voor het opslaan van de resources die zijn gemaakt door het script:

    ```powershell
    # Azure Onpremise Deployments
    $onpremiseNetworkResourceGroupName = "ra-adtrust-onpremise-rg"

    # Azure ADDS Deployments
    $azureNetworkResourceGroupName = "ra-adtrust-network-rg"
    $workloadResourceGroupName = "ra-adtrust-workload-rg"
    $securityResourceGroupName = "ra-adtrust-security-rg"
    $addsResourceGroupName = "ra-adtrust-adds-rg"
    ```

6. De parameterbestanden in de mappen van de Parameters-Scripts/Azure en Parameters-Scripts/lokale bewerken. Verwijzingen naar de resource in deze bestanden die overeenkomen met de namen van de resourcegroepen is toegewezen aan de variabelen in het bestand distribueren ReferenceArchitecture.ps1 bijwerken. De volgende tabel ziet u welke parameterbestanden verwijst naar welke resourcegroep. De *ra adfs-werkbelasting rg*, *ra adfs-beveiliging rg* *ra-adfs-toegevoegd-rg*, *ra adfs-adfs rg*en *ra adfs-proxy rg* resourcegroepen worden alleen gebruikt in het PowerShell script en niet op bij de parameterbestanden.

  	|Resourcegroep|Parameterbestanden|
  	|--------------|--------------|
  	|Ra adtrust-lokale rg|parameters\onpremise\connection.parameters.JSON<br /> parameters\onpremise\virtualMachines-ADDS.parameters.JSON<br />parameters\onpremise\virtualNetwork-ADDS-DNS.parameters.JSON<br />parameters\onpremise\virtualNetwork.parameters.JSON<br />parameters\onpremise\virtualNetworkGateway.parameters.JSON<br />parameters\azure\virtualNetworkGateway.parameters.JSON
  	|Ra adtrust-netwerk rg|parameters\onpremise\connection.parameters.JSON<br />parameters\azure\dmz-Private.parameters.JSON<br />parameters\azure\dmz-Public.parameters.JSON<br />parameters\azure\loadBalancer-Biz.parameters.JSON<br />parameters\azure\loadBalancer-Data.parameters.JSON<br />parameters\azure\loadBalancer-web.parameters.JSON<br />parameters\azure\virtualMachines-ADDS.parameters.JSON<br />parameters\azure\virtualMachines-Mgmt.parameters.JSON<br />parameters\azure\virtualNetwork-ADDS-DNS.parameters.JSON<br />parameters\azure\virtualNetwork.parameters.JSON<br />parameters\azure\virtualNetworkGateway.parameters.JSON (*twee keer*)

    Daarnaast stelt de configuratie voor de lokalen op en cloud-onderdelen, zoals wordt beschreven in de [Onderdelen van de oplossing] [ solution-components] sectie.

7. Een Azure PowerShell-venster openen, verplaatsen naar de map Scripts en voer de volgende opdracht uit:

    ```powershell
    .\Deploy-ReferenceArchitecture.ps1 <subscription id> <location> <mode>
    ```

    Vervangen `<subscription id>` bij de Azure abonnement-ID.

    Voor `<location>`, Geef een Azure regio, zoals `eastus` of `westus`.

    De `<mode>` parameter kan een van de volgende waarden hebben:

    - `Onpremise`, de gesimuleerde op ruimten omgeving maken.

    - `Infrastructure`, voor het maken van de infrastructuur van VNet en springen vak in de cloud.

    - `CreateVpn`, Azure virtueel netwerkgateway bouwen en verbinden met het netwerk op gebouwen.

    - `AzureADDS`, om het VMs fungeren als servers ADDS samenstellen, implementeren van Active Directory op deze VMs en het domein maken in de cloud.

    - `WebTier`, waarbij het web automatisch trapsgewijs VMs en de belasting voor documentconversies.

    - `Prepare`, die de voorafgaande taken uitvoert. **Dit is de aanbevolen optie als u een volledig nieuwe installatie bouwt en u niet een bestaande op gebouwen-infrastructuur hoeft.**

    - `Workload`de bedrijfs- en laag VMs maken en netwerktaakverdelers. Deze VMs worden niet opgenomen als onderdeel van de `Prepare` optie.

    >[AZURE.NOTE] Als u de `Prepare` optie, het script duurt enkele uren.

8.  Als u de configuratie van monster op ruimten:

    1. Verbinding maken met het vak sprong (*ra-adtrust-mgmt-vm1* in de resourcegroep *ra, adtrust, beveiliging, rg* ). Meld u aan als *' testuser '* wachtwoord *AweS0me@PW*.

    2.  Open op het vak koppeling een RDP-sessie op de eerste VM in het domein *contoso.com* (het domein van gebouwen). Deze VM heeft het IP-adres 192.168.0.4. De gebruikersnaam is *contoso\testuser* met wachtwoord *AweS0me@PW*.

    3. Downloaden van de [inkomende trust.ps1] [ incoming-trust] script en uit te voeren voor de binnenkomende vertrouwensrelatie te maken van het domein *treyresearch.com* .

9. Als u uw eigen infrastructuur op ruimten:

    1. Downloaden van de [inkomende trust.ps1] [ incoming-trust] script.

    2. Bewerk het script en vervang de waarde van de `$TrustedDomainName` variabele met de naam van uw eigen domein.

    3. Het script uitvoeren.

10. Klik in de sprong-verbinding maken met de eerste VM in het *treyresearch.com* domein (het domein in de cloud). Deze VM heeft het IP-adres 10.0.4.4. De gebruikersnaam is *treyresearch\testuser* met wachtwoord *AweS0me@PW*.

11. Downloaden van de [uitgaande trust.ps1] [ outgoing-trust] script en uit te voeren voor de binnenkomende vertrouwensrelatie te maken van het domein *treyresearch.com* . Als u met behulp van uw eigen machines van lokalen en eerst het script te bewerken. Stel de `$TrustedDomainName` variabele de naam van uw domein op gebouwen en geef de IP-adressen van servers voor dit domein in AD DS de `$TrustedDomainDnsIpAddresses` variabele.

12. Uitvoeren op een computer op het bedrijf de stappen in het artikel [een vertrouwensrelatie controleren] [ verify-a-trust] om te bepalen of de vertrouwensrelatie is juist geconfigureerd tussen de domeinen *contoso.com* en *treyresearch.com* . Wellicht moet u wachten op een paar minuten na het voltooien van de voorgaande stappen voordat u de vertrouwensrelatie kan worden gevalideerd.

De overige optionele stappen laten zien hoe om te bepalen of de vertrouwensrelatie tussen domeinen werkt zoals verwacht. Deze stap is vereist dat u toegang tot een computer development met Visual Studio is geïnstalleerd hebt.

1.  Uitvoeren vanuit het venster Azure PowerShell de volgende opdracht om de weblaag maken als u geen hebt ingesteld dat eerder (met behulp van de `Prepare` optie):

    ```powershell
    .\Deploy-ReferenceArchitecture.ps1 <subscription id> <location> WebTier
    ```

    Met deze opdracht de weblaag wordt gemaakt en wordt het VMs toegevoegd aan het domein *treyresearch.com* .

2. Met behulp van Visual Studio op de ontwikkelcomputer, maak een ASP.NET-webtoepassing met de naam *TreyResearchWebApp*. Gebruik van het .NET Framework 4.5.2.

3. Selecteer de sjabloon *MVC* en de verificatie op *Windows-verificatie*te wijzigen. Maak geen een App-Service in de cloud.

3. Bouwen en uitvoeren van de toepassing voor het testen van de verificatie. Controleer of uw huidige Windows-gebruikersnaam wordt weergegeven in de menubalk boven aan de pagina naar rechts.

4. Sluit Internet Explorer.

5. In het venster *Solution Explorer* met de rechtermuisknop op het TreyResearchWebApp-project, klikt u op *publiceren*.

6. Klik op *aangepast*in het venster *Web publiceren* . Maak een aangepast profiel met de naam *TreyResearchWebApp*.

7. De *methode Publish* instellen op het *Bestandssysteem* op de pagina *verbinding* en geef een map met de naam *TreyResearchWebApp*, zich op een handige locatie op de computer.

8. Instellen op de pagina *Instellingen voor* de *configuratie van* *release*.

9. Klik op de pagina *voorvertoning* *publiceren*.

10. Verbinding maken met elke VM in de weblaag weer (via het vak sprong) en de volgende taken uitvoeren. De IP-adressen van het web tier VMs zijn 10.0.1.4 en 10.0.1.5. De gebruikersnaam voor beide VMs *treyresearch\testuser* met een wachtwoord is *AweS0me@PW*:

    1. De map *TreyResearchWebApp* en de inhoud ervan kopiëren vanaf de ontwikkelcomputer naar de map *C:\inetpub* .

    2. Met de console Beheer van Internet Information Services (IIS), Ga naar *de website Sites\Default* op de computer.

    3. In het deelvenster *Acties* op de *Basisinstellingen*en het fysieke pad van de website wijzigen in *%SystemDrive%\inetpub\TreyResearchWebApp*.

    4. Dubbelklik in het deelvenster *Weergave functies* op *verificatie*. Controleer of *Windows-verificatie* is ingeschakeld en of *Anonieme verificatie* is uitgeschakeld.

11. open Internet Explorer en Ga naar de website op http://10.0.1.254 (dit is het adres van de taakverdeling web tier) van een computer op gebouwen.

12. Voer de referenties van een gebruiker in het domein van lokalen in het dialoogvenster *Windows-beveiliging* . Geef een gebruikersnaam die niet in het domein *treyresearch bestaat* . Als u de omgeving gesimuleerde op ruimten Maak eerst een gebruiker in het domein *contoso* en geeft u de referenties van de gebruiker.

13. Wanneer de introductiepagina wordt weergegeven, controleert u of het juiste domein en de gebruikersnaam in de menubalk boven aan de pagina naar rechts weergegeven.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over de aanbevolen procedures voor het [uitbreiden van uw domein op ruimten ADDS Azure][adds-extend-domain]

- Informatie over de aanbevolen procedures voor het [maken van een AD FS-infrastructuur] [ adfs] in Azure.

<!-- links -->

[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[adfs]: ./guidance-identity-adfs.md
[adds-extend-domain]: ./guidance-identity-adds-extend-domain.md
[extending-ad-to-azure]: ./guidance-identity-adds-extend-domain.md
[implementing-aad]: ./guidance-identity-aad.md
[implementing-a-multi-tier-architecture-on-Azure]: ./guidance-compute-n-tier-vm.md
[implementing-a-secure-hybrid-network-architecture-with-internet-access]: ./guidance-iaas-ra-secure-vnet-dmz.md
[implementing-a-secure-hybrid-network-architecture]: ./guidance-iaas-ra-secure-vnet-hybrid.md
[implementing-a-hybrid-network-architecture-with-vpn]: ./guidance-hybrid-network-vpn.md
[running-VMs-for-an-N-tier-architecture-on-Azure]: ./guidance-compute-n-tier-vm.md
[azure-vpn-gateway]: https://azure.microsoft.com/documentation/articles/vpn-gateway-about-vpngateways/
[azure-expressroute]: https://azure.microsoft.com/documentation/articles/expressroute-introduction/
[ad-azure-guidelines]: https://msdn.microsoft.com/library/azure/jj156090.aspx
[standby-operations-masters]: https://technet.microsoft.com/library/cc794737(v=ws.10).aspx
[best_practices_ad_password_policy]: https://technet.microsoft.com/magazine/ff741764.aspx
[monitoring_ad]: https://msdn.microsoft.com/library/bb727046.aspx
[microsoft_systems_center]: https://www.microsoft.com/server-cloud/products/system-center-2016/
[creating-forest-trusts]: https://technet.microsoft.com/library/cc816810(v=ws.10).aspx
[creating-external-trusts]: https://technet.microsoft.com/library/cc816837(v=ws.10).aspx
[naming-conventions]: ./guidance-naming-conventions.md
[azure-powershell-download]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[hybrid-azure-on-prem-vpn]: ./guidance-hybrid-network-vpn.md
[verify-a-trust]: https://technet.microsoft.com/library/cc753821.aspx
[netdom]: https://technet.microsoft.com/library/cc835085.aspx
[solution-script]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/Deploy-ReferenceArchitecture.ps1
[on-premises-folder]: https://github.com/mspnp/reference-architectures/tree/master/guidance-identity-adds-trust/parameters/onpremise
[on-premises-vnet-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/onpremise/virtualNetwork.parameters.json
[on-premises-virtualmachines-adds-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/onpremise/virtualMachines-adds.parameters.json
[on-premises-virtualnetworkgateway-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/onpremise/virtualNetworkGateway.parameters.json
[on-premises-connection-parameters]: https://github.com/mspnp/reference-architectures/blob/master/guidance-identity-adds-trust/parameters/onpremise/connection.parameters.json
[incoming-trust]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/extensions/incoming-trust.ps1
[outgoing-trust]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/extensions/outgoing-trust.ps1
[azure-folder]: https://github.com/mspnp/reference-architectures/tree/master/guidance-identity-adds-trust/parameters/azure
[vnet-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/virtualNetwork.parameters.json
[virtualmachines-adds-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/virtualMachines-adds.parameters.json
[add-adds-domain-controller-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/add-adds-domain-controller.parameters.json
[virtualnetworkgateway-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/virtualNetwork.parameters.json
[dmz-private-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/dmz-private.parameters.json
[dmz-public-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/dmz-public.parameters.json
[loadBalancer-web-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/loadBalancer-web.parameters.json
[loadBalancer-biz-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/loadBalancer-biz.parameters.json
[loadBalancer-data-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/loadBalancer-data.parameters.json
[virtualMachines-mgmt-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/virtualMachines-mgmt.parameters.json
[web-vm-domain-join-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/web-vm-domain-join.parameters.json
[solution-components]: [#solution_components]
[0]: ./media/guidance-identity-aad-resource-forest/figure1.png "Hybride netwerkarchitectuur met afzonderlijke domeinen die AD beveiligen"