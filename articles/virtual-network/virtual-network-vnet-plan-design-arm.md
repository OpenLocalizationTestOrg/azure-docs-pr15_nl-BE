<properties
   pageTitle="Plan Azure Virtual Network (VNet) and Design Guide | Microsoft Azure"
   description="Informatie over het plannen en ontwerpen van virtuele netwerken in Azure op basis van uw vereisten voor isolatie, connectiviteit en locatie."
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/08/2016"
   ms.author="jdial" />

# <a name="plan-and-design-azure-virtual-networks"></a>Plannen en ontwerpen van Azure virtuele netwerken

Maken van een VNet om te experimenteren met is eenvoudig genoeg, maar waarschijnlijk hebt u meerdere VNets na verloop van tijd voor de ondersteuning van de productie behoeften van uw organisatie gaat implementeren. Met een aantal plannen en ontwerpen kun je VNets implementeren en de resources die effectiever u moet verbinding maken. Als u niet bekend met VNets bent, is het raadzaam dat u [meer informatie over VNets](virtual-networks-overview.md) en [het implementeren van](virtual-networks-create-vnet-arm-pportal.md) een voordat u verdergaat. 

## <a name="plan"></a>Plan

Een goed begrip van Azure abonnementen, regio's en netwerkbronnen is essentieel voor succes. U kunt de lijst van overwegingen hieronder als uitgangspunt. Als u deze overwegingen eenmaal begrijpt, kunt u de vereisten van een netwerk.

### <a name="considerations"></a>Overwegingen met betrekking tot

Voordat u de planning beantwoorden vragen hieronder, het volgende overwegen:

- Alles wat u in Azure is samengesteld uit een of meer resources. Een virtuele machine (VM) is een bron, is een bron van de adapter netwerkinterface (NIC) die wordt gebruikt door een VM, is een bron van het openbare IP-adres van een Netwerkkaart, de VNet de Netwerkkaart is verbonden met een resource is.
- U maken bronnen binnen een [regio Azure](https://azure.microsoft.com/regions/#services) en een abonnement. En resources kunnen alleen worden verbonden met een VNet die in dezelfde regio en zijn abonnement bestaat. 
- U kunt VNets met elkaar verbinden met behulp van een [VPN-Gateway](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)-Azure. U kunt ook verbinding maken VNets in regio's van abonnementen op deze manier.
- Kunt u VNets op uw netwerk in ruimten met een van de [connectiviteitsopties](../vpn-gateway/vpn-gateway-about-vpngateways.md#site-to-site-and-multi-site) in Azure. 
- Verschillende bronnen kunnen worden gegroepeerd in [resourcegroepen](../azure-resource-manager/resource-group-overview.md#resource-groups), waardoor het gemakkelijker wordt om het beheer van de bron als een eenheid. Een resourcegroep kan resources in meerdere regio's bevatten als de resources tot het hetzelfde abonnement behoren.

### <a name="define-requirements"></a>Vereisten definiëren

De vragen hieronder gebruiken als uitgangspunt voor het netwerkontwerp van uw Azure.  

1. Welke locaties Azure gaat u naar de host VNets
2. Moet u de communicatie tussen deze locaties Azure?
3. Moet u de communicatie tussen uw Azure-VNet(s) en de datacenter(s) in de lokalen?
4. Hoeveel infrastructuur als een VMs Service (IaaS) wolk services-rollen en web apps doen die u nodig hebt voor uw oplossing?
5. Moet u verkeer op basis van groepen van VMs (dat wil zeggen front-end-webservers en back-end-databaseservers) isoleren?
6. Moet u de verkeersstroom met behulp van virtuele apparaten beheren?
7. Moeten gebruikers verschillende sets machtigingen voor verschillende Azure middelen

### <a name="understand-vnet-and-subnet-properties"></a>De eigenschappen VNet en subnet begrijpen

VNet en subnetten informatiebronnen definiëren een beveiligingsgrens voor werkbelasting in Azure uitgevoerd. Een VNet wordt gekenmerkt door een verzameling adresruimten, gedefinieerd als de CIDR-blokken. 

>[AZURE.NOTE] Netwerkbeheerders bent bekend met CIDR-notatie. Als u niet bekend bent met CIDR, [meer informatie over](http://whatismyipaddress.com/cidr).

VNets bevatten de volgende eigenschappen.

|Eigenschap|Beschrijving|Beperkingen|
|---|---|---|
|**naam**|VNet naam|Een tekenreeks van maximaal 80 tekens. Mogen letters, cijfers, onderstrepingstekens, punten of streepjes. Moet beginnen met een letter of cijfer. Moet eindigen met een letter, cijfer of onderstrepingsteken. Kan bevat hoofdletters of kleine letters.|  
|**locatie**|Azure locatie (ook wel regio genoemd).|Moet een van de geldige locaties Azure.|
|**addressSpace**|Collectie-adresprefixen waaruit de VNet in de CIDR-notatie.|Moet u een matrix van geldige CIDR-adresblokken, met inbegrip van openbare IP-adresbereiken.|
|**subnetten**|Verzameling subnetten van de VNet|Zie de tabel hieronder eigenschappen van subnet.||
|**dhcpOptions**|Object dat één vereiste eigenschap bevat met de naam **dnsServers**.||
|**dnsServers**|Een matrix van DNS-servers die worden gebruikt door de VNet. Als er geen server is opgegeven, wordt de interne naamomzetting Azure gebruikt.|Moet u een matrix van maximaal 10 DNS-servers het IP-adres.| 

Een subnet is een onderliggende bron van een VNet en helpt bij het segmenten van adresruimten binnen een CIDR-blok met IP-adresprefixen definiëren. NIC's kunnen worden toegevoegd aan subnetten en verbonden met VMs, een connectiviteit voor verschillende werkbelastingen.

Subnetten zijn de volgende eigenschappen bevatten. 

|Eigenschap|Beschrijving|Beperkingen|
|---|---|---|
|**naam**|Subnetnaam|Een tekenreeks van maximaal 80 tekens. Mogen letters, cijfers, onderstrepingstekens, punten of streepjes. Moet beginnen met een letter of cijfer. Moet eindigen met een letter, cijfer of onderstrepingsteken. Kan bevat hoofdletters of kleine letters.|
|**locatie**|Azure locatie (ook wel regio genoemd).|Moet een van de geldige locaties Azure.|
|**addressPrefix**|Één adres-voorvoegsel van het subnet in CIDR-notatie|Moet één CIDR blok dat deel uitmaakt van een van de adresruimte van de VNet.|
|**networkSecurityGroup**|Toegepast op het subnet NSG|Zie [NSGs](resource-groups-networking.md#Network-Security-Group)|
|**routeTable**|Routetabel toegepast op het subnet|Zie [UDR](resource-groups-networking.md#Route-table)|
|**ipConfigurations**|Collectie gebruikt door NIC's die zijn verbonden met het subnet-IP-configuratieobjecten|Zie [IP-configuratie](../resource-groups-networking.md#IP-configurations)|

### <a name="name-resolution"></a>Naamomzetting

De VNet standaard [geleverde Azure naamomzetting.](virtual-networks-name-resolution-for-vms-and-role-instances.md#Azure-provided-name-resolution) voor het herleiden van namen in de VNet en op het openbare Internet. Als u uw VNets aan uw datacenters op gebouwen op de verbinding, moet u [uw eigen DNS-server](virtual-networks-name-resolution-for-vms-and-role-instances.md#Name-resolution-using-your-own-DNS-server) voor het herleiden van namen tussen de netwerken bieden.  

### <a name="limits"></a>Limieten

Bekijk de netwerken grenzen in het artikel [Azure beperkt](../azure-subscription-service-limits.md#networking-limits) om ervoor te zorgen dat uw ontwerp niet in strijd met een van de grenzen. Enkele beperkingen kunnen worden verhoogd door een support ticket openen.

### <a name="role-based-access-control-rbac"></a>Role-Based Access Control RBAC)

[Azure RBAC](../active-directory/role-based-access-built-in-roles.md) kunt u bepalen hoeveel verschillende gebruikers toegang tot verschillende bronnen in Azure hebben kunnen. Op die manier die kunt u het werk gedaan door het team op basis van hun behoeften te scheiden. 

Wat betreft virtuele netwerken zijn, hebben gebruikers met de rol van **Inzender netwerk** volledig beheer over de bronnenbeheerder Azure virtual netwerkbronnen. Gebruikers met de rol van **Inzender voor klassieke netwerk** hebben ook volledig beheer over klassieke virtuele netwerkbronnen.

>[AZURE.NOTE] U kunt ook [uw eigen rollen maken](../active-directory/role-based-access-control-configure.md) om te scheiden van uw administratieve behoeften.

## <a name="design"></a>Ontwerp

Als u eenmaal de antwoorden op de vragen in de sectie [planning weet](#Plan) , lees het volgende voordat u uw VNets definiëren.

### <a name="number-of-subscriptions-and-vnets"></a>Aantal abonnementen en VNets

U kunt meerdere VNets maken in de volgende scenario's:

- **VMs die moeten worden geplaatst op verschillende locaties van Azure**. VNets in Azure zijn regionale. Deze niet kunnen locaties beslaan. Daarom moet u ten minste één VNet voor elke gewenste host VMs in Azure locatie.
- **Werkbelasting die moeten worden volledig van elkaar gescheiden**. U kunt afzonderlijke VNets, die zelfs de spaties hetzelfde IP-adres gebruiken voor verschillende werkbelastingen van elkaar isoleren. 

Houd er rekening mee dat de bovenstaand per regio, per abonnement zijn. Dit betekent dat u kunt meerdere abonnementen van bronnen die u in Azure onderhouden kunt limiet verhogen. Kunt u een site naar site VPN of een circuit ExpressRoute verbinding maken met VNets in verschillende abonnementen.

### <a name="subscription-and-vnet-design-patterns"></a>Abonnement en VNet ontwerppatronen

De onderstaande tabel ziet u enkele algemene ontwerppatronen voor abonnementen en VNets.

|Scenario|Diagram|Pros|Nadelen|
|---|---|---|---|
|Abonnement van één, twee VNets per app|![Één abonnement](./media/virtual-network-vnet-plan-design-arm/figure1.png)|Slechts één abonnement te beheren.|Maximum aantal VNets per regio Azure. Daarna moet u meer abonnementen. Bekijk de [Azure beperkt](../azure-subscription-service-limits.md#networking-limits) artikel voor meer informatie.|
|Met één abonnement per app, twee VNets per app|![Één abonnement](./media/virtual-network-vnet-plan-design-arm/figure2.png)|Slechts twee VNets per abonnement gebruikt.|Moeilijker te beheren als er te veel apps.|
|Met één abonnement per bedrijfsunit, twee VNets per app.|![Één abonnement](./media/virtual-network-vnet-plan-design-arm/figure3.png)|Evenwicht tussen het aantal abonnementen en VNets.|Maximum aantal VNets per bedrijfsunit (abonnement). Bekijk de [Azure beperkt](../azure-subscription-service-limits.md#networking-limits) artikel voor meer informatie.|
|Met één abonnement per bedrijfsunit, twee VNets per groep van toepassingen.|![Één abonnement](./media/virtual-network-vnet-plan-design-arm/figure4.png)|Evenwicht tussen het aantal abonnementen en VNets.|Apps moeten worden geïsoleerd met subnetten en NSGs.|


### <a name="number-of-subnets"></a>Aantal subnetten

U kunt meerdere subnetten in een VNet in de volgende scenario's:

- **Er is onvoldoende particuliere IP-adressen voor alle netwerkadapters in een subnet**. Als de adresruimte van uw subnet niet voldoende IP-adressen voor het aantal netwerkkaarten in het subnet bevat, moet u meerdere subnetten maken. Houd er rekening mee dat Azure 5 particuliere IP-adressen van elk subnet dat kan niet worden gebruikt: de eerste en laatste adressen van de adresruimte (voor het subnetadres en multicast) en 3 adressen intern (voor DHCP en DNS) worden gebruikt. 
- **Beveiliging**. U kunt de subnetten gebruiken om groepen van VMs van elkaar scheiden voor de werklast die een gelaagde structuur hebben en verschillende [beveiligingsgroepen netwerk (NSGs)](virtual-networks-nsg.md#subnets) voor deze subnetten zijn van toepassing.
- **Hybride verbindingen**. Kunt u VPN-gateways en -circuits ExpressRoute [verbinding maken met](../vpn-gateway/vpn-gateway-about-vpngateways.md#site-to-site-and-multi-site) uw VNets aan elkaar en aan de gegevens op de locatie center(s). VPN-gateways en -circuits ExpressRoute vereisen een subnet van hun eigen te maken.
- **Virtuele apparaten**. U kunt een virtueel apparaat, zoals een firewall, VPN-gateway of WAN-accelerator een Azure-VNet. Wanneer u dit doet, kunt u wilt [routeren van netwerkverkeer](virtual-networks-udr-overview.md) naar apparaten en deze groep isoleren in hun eigen subnet.

### <a name="subnet-and-nsg-design-patterns"></a>Subnet en NSG ontwerppatronen

De onderstaande tabel ziet u enkele algemene ontwerppatronen voor het gebruik van subnetten.

|Scenario|Diagram|Pros|Nadelen|
|---|---|---|---|
|Één subnet, NSGs per toepassingslaag per app|![Één subnet](./media/virtual-network-vnet-plan-design-arm/figure5.png)|Slechts één subnet beheren.|Meerdere NSGs die nodig zijn voor het isoleren van elke toepassing.|
|Één subnet per app, NSGs per toepassingslaag|![Subnet per app](./media/virtual-network-vnet-plan-design-arm/figure6.png)|Minder NSGs te beheren.|Meerdere subnetten beheren.|
|Één subnet per toepassingslaag, NSGs per app.|![Subnet per laag](./media/virtual-network-vnet-plan-design-arm/figure7.png)|Evenwicht tussen het aantal subnetten en NSGs.|Maximum aantal NSGs per abonnement. Bekijk de [Azure beperkt](../azure-subscription-service-limits.md#networking-limits) artikel voor meer informatie.|
|Één subnet per toepassingslaag per app, NSGs per subnet|![Subnet per laag per app](./media/virtual-network-vnet-plan-design-arm/figure8.png)|Mogelijk kleiner aantal NSGs.|Meerdere subnetten beheren.|

## <a name="sample-design"></a>Voorbeeld ontwerp

Neem het volgende scenario dient ter illustratie van de toepassing van de informatie in dit artikel.

U werkt voor een bedrijf met 2 datacenters in Noord-Amerika en twee datacenters Europa. U bepaald 6 andere klant gerichte toepassingen onderhouden door 2 verschillende business units die u wilt migreren naar Azure als een pilot. De basisarchitectuur van toepassingen zijn als volgt:

- 1, 2, App3 en App4 zijn de webtoepassingen die worden gehost op Ubuntu Linux servers. Elke toepassing maakt verbinding met de server van een afzonderlijke toepassing die als host fungeert voor RESTful services op Linux-servers. De RESTful services verbinding maken met een back-end-database MySQL.
- App5 en App6 zijn de webtoepassingen die worden gehost op Windows-servers met Windows Server 2012 R2. Elke toepassing maakt verbinding met SQL Server-databases back-end.
- Alle toepassingen worden die momenteel gehost in een van de datacenters van het bedrijf in Noord-Amerika.
- De datacenters van lokalen de 10.0.0.0/8-adresruimte gebruiken.

U moet een virtueel netwerkoplossing ontwerpen die voldoet aan de volgende vereisten:

- Elke business unit worden niet beïnvloed door het brongebruik van andere business units.
- Het bedrag van de VNets en subnetten voor gemakkelijker beheer te beperken.
- Elke business unit moet beschikken over een enkele test/ontwikkeling VNet gebruikt voor alle toepassingen.
- Elke toepassing wordt gehost in 2 verschillende Azure datacenters per continent (Noord-Amerika en Europa).
- Elke toepassing is volledig van elkaar gescheiden.
- Elke toepassing toegankelijk voor klanten via het Internet met behulp van HTTP.
- Elke toepassing kan worden geopend door gebruikers via een gecodeerde tunnel verbonden met de gegevenscentra op gebouwen.
- Verbinding met datacenters voor bedrijfsruimten moet bestaande VPN-apparaten gebruiken.
- Netwerk van het bedrijf-groep moet hebben volledige controle over de configuratie van de VNet.
- Alleen moet ontwikkelaars in elke business unit kunnen VMs naar bestaande subnetten te implementeren.
- Alle toepassingen worden gemigreerd worden naar Azure (lift-en-ploeg).
- De databases op elke locatie moeten worden gerepliceerd naar andere locaties Azure eenmaal per dag.
- Elke toepassing gebruik 5 front-end webservers en toepassingsservers 2 (indien nodig) 2-databaseservers.

### <a name="plan"></a>Plan

Het ontwerp van de planning door het beantwoorden van de vraag in de sectie [vereisten definiëren](#Define-requirements) als volgt moet worden gestart.

1. Welke locaties Azure gaat u naar de host VNets

    2 vestigingen in Noord-Amerika, en 2 locaties in Europa. U moet kiezen die zijn gebaseerd op de fysieke locatie van uw bestaande op locatie datacenters. De verbinding van uw fysieke locaties met Azure heeft op die manier een beter latentie.

2. Moet u de communicatie tussen deze locaties Azure?

    Ja. Aangezien de databases moeten worden gerepliceerd naar alle locaties.

3. Moet u ten behoeve van communicatie tussen uw Azure-VNet(s) en uw locatie op de center(s) gegevens?

    Ja. Aangezien gebruikers met verbonden moet de gegevenscentra op ruimten toegang tot de toepassingen die via een gecodeerde tunnel zijn.
 
4. Hoeveel IaaS VMs moet u voor uw oplossing?

    200 IaaS VMs. 1, 2 en App3 moet 5 webservers elke 2 toepassingsservers elke en databaseservers 2 elke. Dat is een totaal van 9 IaaS VMs per aanvraag of 36 IaaS VMs. App5 en App6 moeten 5 webservers en databaseservers 2. Dat is een totaal van 7 IaaS VMs per aanvraag of 14 IaaS VMs. Daarom moet u 50 IaaS VMs voor alle toepassingen in elke regio Azure. Omdat we 4 regio's nodig, zullen er 200 IaaS VMs.

    Ook moet u DNS-servers in elke VNet of in de datacenters op ruimten naam tussen uw Azure IaaS VMs en uw netwerk op gebouwen bieden. 

5. Moet u verkeer op basis van groepen van VMs (dat wil zeggen front-end-webservers en back-end-databaseservers) isoleren?

    Ja. Elke toepassing volledig van elkaar gescheiden worden en elke toepassingslaag moet ook worden geïsoleerd. 

6. Moet u de verkeersstroom met behulp van virtuele apparaten beheren?

    Nr. Virtuele apparaten kunnen worden gebruikt om meer controle over de verkeersstroom, waaronder logboekregistratie voor meer gedetailleerde gegevens vlak. 

7. Moeten gebruikers verschillende sets machtigingen voor verschillende Azure middelen

    Ja. Het netwerk team moet volledig beheer over de instellingen van het virtuele netwerk, terwijl ontwikkelaars moeten alleen hun VMs op bestaande subnetten te implementeren. 

### <a name="design"></a>Ontwerp

Gaat u als volgt het ontwerp-abonnementen, VNets, subnetten en NSGs op te geven. NSGs Hier wordt besproken, maar u moet weten over [NSGs](virtual-networks-nsg.md) voordat u voltooit het ontwerp van de.

**Aantal abonnementen en VNets**

De volgende vereisten hebben betrekking op abonnementen en VNets:

- Elke business unit worden niet beïnvloed door het brongebruik van andere business units.
- Het bedrag van VNets en subnetten te beperken.
- Elke business unit moet beschikken over een enkele test/ontwikkeling VNet gebruikt voor alle toepassingen.
- Elke toepassing wordt gehost in 2 verschillende Azure datacenters per continent (Noord-Amerika en Europa).

Op basis van deze eisen wordt voldaan, moet u een abonnement voor elke bedrijfsunit. Op die manier verbruik van resources uit een bedrijfsunit zal tellen niet mee in limieten voor andere business units. En aangezien u het aantal VNets te beperken wilt, moet u overwegen het patroon **met één abonnement per bedrijfsunit, twee VNets per groep van toepassingen,** zoals hieronder wordt weergegeven.

![Één abonnement](./media/virtual-network-vnet-plan-design-arm/figure9.png)

U moet ook de adresruimte opgeven voor elke VNet. Aangezien u nodig hebt gecentreerd connectiviteit tussen de gegevens op de locatie de Azure regio's, de adresruimte gebruikt voor Azure VNets kan niet conflicteren met het netwerk op gebouwen en de adresruimte die wordt gebruikt door elke VNet mag niet conflicteren met andere bestaande VNets. U kunt de adresruimte in de onderstaande tabel om te voldoen aan deze eisen.  

|**Abonnement**|**VNet**|**Azure regio**|**Adresruimte**|
|---|---|---|---|
|BU1|ProdBU1US1|West-Verenigde Staten|172.16.0.0/16|
|BU1|ProdBU1US2|Oost-Verenigde Staten|172.17.0.0/16|
|BU1|ProdBU1EU1|Noord-Europa|172.18.0.0/16|
|BU1|ProdBU1EU2|West-Europa|172.19.0.0/16|
|BU1|TestDevBU1|West-Verenigde Staten|172.20.0.0/16|
|BU2|TestDevBU2|West-Verenigde Staten|172.21.0.0/16|
|BU2|ProdBU2US1|West-Verenigde Staten|172.22.0.0/16|
|BU2|ProdBU2US2|Oost-Verenigde Staten|172.23.0.0/16|
|BU2|ProdBU2EU1|Noord-Europa|172.24.0.0/16|
|BU2|ProdBU2EU2|West-Europa|172.25.0.0/16|

**Aantal subnetten en NSGs**

De volgende vereisten hebben betrekking op subnetten en NSGs:

- Het bedrag van VNets en subnetten te beperken.
- Elke toepassing is volledig van elkaar gescheiden.
- Elke toepassing toegankelijk voor klanten via het Internet met behulp van HTTP.
- Elke toepassing kan worden geopend door gebruikers via een gecodeerde tunnel verbonden met de gegevenscentra op gebouwen.
- Verbinding met datacenters voor bedrijfsruimten moet bestaande VPN-apparaten gebruiken.
- De databases op elke locatie moeten worden gerepliceerd naar andere locaties Azure eenmaal per dag.

Op basis van deze eisen, kan u één subnet per toepassingslaag gebruik en NSGs verkeer filteren per toepassing te gebruiken. Op die manier hoeft u slechts 3 de subnetten in elke VNet (front-end application layer en data laag) en één NSG per toepassing per subnet. In dit geval moet u overwegen het ontwerppatroon **één subnet per toepassingslaag, NSGs per app** . In de volgende afbeelding ziet u het gebruik van het ontwerppatroon die de VNet **ProdBU1US1** .

![Één subnet per laag, één NSG per toepassing per laag](./media/virtual-network-vnet-plan-design-arm/figure11.png)

Echter, moet u ook een extra subnet maken voor de VPN-verbinding tussen de VNets en uw datacenters op gebouwen. En moet u de adresruimte voor elk subnet opgeven. In de volgende afbeelding ziet u een oplossing voor **ProdBU1US1** VNet. U zou dit scenario repliceren voor elke VNet. Elke kleur vertegenwoordigt een andere toepassing.

![Voorbeeld VNet](./media/virtual-network-vnet-plan-design-arm/figure10.png)

**Toegangsbeheer**

De volgende vereisten hebben betrekking op het besturingselement:

- Netwerk van het bedrijf-groep moet hebben volledige controle over de configuratie van de VNet.
- Alleen moet ontwikkelaars in elke business unit kunnen VMs naar bestaande subnetten te implementeren.

Op basis van deze eisen, kan u gebruikers van het netwerk team toevoegen aan de ingebouwde functie van **Inzender netwerk** in elk abonnement; en maakt u een aangepaste rol voor de ontwikkelaars van toepassingen in elk abonnement door ze te machtigen VMs toevoegen aan bestaand subnetten.

## <a name="next-steps"></a>Volgende stappen

- [Een virtueel netwerk distribueren](virtual-networks-create-vnet-arm-template-click.md) op basis van een scenario.
- Begrijpen hoe IaaS VMs [taakverdeling](../load-balancer/load-balancer-overview.md) en [Routering via meerdere Azure regio's beheren](../traffic-manager/traffic-manager-overview.md).
- Meer informatie over [NSGs en het plannen en ontwerpen van](virtual-networks-nsg.md) een oplossing voor NSG.
- Meer informatie over de [cross-ruimten en VNet connectiviteitsopties](../vpn-gateway/vpn-gateway-about-vpngateways.md#site-to-site-and-multi-site).
