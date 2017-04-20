<properties
   pageTitle="Meerdere VIP's voor Azure Load Balancer | Microsoft Azure"
   description="Overzicht van meerdere VIP's op Azure Load Balancer"
   services="load-balancer"
   documentationCenter="na"
   authors="chkuhtz"
   manager="narayan"
   editor=""
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/11/2016"
   ms.author="chkuhtz"
/>

# <a name="multiple-vips-for-azure-load-balancer"></a>Meerdere VIP's voor Azure Load Balancer

Azure Load Balancer kunt u services op meerdere poorten of meerdere IP-adressen saldo te laden. U kunt public en internal load balancer definities saldo stromen via een reeks VMs laden.

Dit artikel worden de basisprincipes van deze mogelijkheid, belangrijke concepten en beperkingen. Als u alleen blootstellen services op één IP-adres wilt, kunt u vinden vereenvoudigde instructies voor [openbare](load-balancer-get-started-internet-portal.md) of [interne](load-balancer-get-started-ilb-arm-portal.md) balancer configuraties laden. Meerdere VIP's toe te voegen in een configuratie met één VIP incrementele is. Met behulp van de concepten in dit artikel, kunt u een vereenvoudigde configuratie op elk gewenst moment uitbreiden.

Bij het definiëren van een taakverdeling Azure zijn een front-end en een back-end-configuratie verbonden met de regels. De gezondheid sonde waarnaar wordt verwezen door de regel wordt gebruikt om te bepalen hoe nieuwe stromen worden verzonden naar een knooppunt in de back-end-toepassingen. De frontend is gedefinieerd door een virtuele IP-(VIP), is een 3-tupel bestaat uit een IP-adres (openbare of interne), een transportprotocol (UDP of TCP) en een poortnummer. Een DIP is een IP-adres op een Azure virtual NIC gekoppeld aan een VM in de back-end-toepassingen.

De volgende tabel bevat enkele voorbeeldconfiguraties frontend:

| VIP | IP-adres | Protocol | poort |
|-----|------------|----------|------|
|1|65.52.0.1|TCP|80|
|2|65.52.0.1|TCP|_8080_|
|3|65.52.0.1|_UDP_|80|
|4|_65.52.0.2_|TCP|80|

De tabel bevat vier verschillende frontends. Frontends #1 #2 en #3 zijn een enkele VIP met meerdere regels. Hetzelfde IP-adres wordt gebruikt, maar de poort- of protocolregel verschilt voor elk frontend. Frontends #1 en #4 zijn een voorbeeld van meerdere VIP's, waarbij dezelfde frontend-protocol en poort opnieuw worden gebruikt in meerdere VIP's.

Azure Load Balancer biedt flexibiliteit bij het definiëren van regels voor de taakverdeling. Een regel verklaart hoe een adres en de poort van de frontend is toegewezen aan het doeladres en de poort op de backend. Of er back-end-poorten opnieuw verschillende regels worden gebruikt, hangt af van het type van de regel. Elk type regel heeft specifieke behoeften die van invloed kunnen zijn op de host-configuratie en sonde ontwerp. Er zijn twee soorten regels:

1. De standaardregel met geen hergebruik backend-poort
2. De zwevende IP-regel waar de back-end-poorten worden opnieuw gebruikt

Azure Load Balancer kunt u beide typen van dezelfde load balancer configuratie mengen. Verdeling van de belasting kunt ze tegelijkertijd voor een bepaalde VM of een combinatie daarvan, zolang u houdt zich aan de beperkingen van de regel. Welke regel dat u kiest, is afhankelijk van de vereisten van uw toepassing en de complexiteit van die configuratie ondersteunen. U moet evalueren welke soorten zijn het meest geschikt is voor uw scenario.

We verkennen deze scenario's verder door te starten met de standaardinstellingen.

## <a name="rule-type-1-no-backend-port-reuse"></a>Regel #1-type: geen hergebruik backend-poort

![MultiVIP afbeelding](./media/load-balancer-multivip-overview/load-balancer-multivip.png)

In dit scenario worden de frontend VIP's als volgt geconfigureerd:

| VIP | IP-adres | Protocol | poort |
|-----|------------|----------|------|
|![VIP](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) 1|65.52.0.1|TCP|80|
|![VIP](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) 2|*65.52.0.2*|TCP|80|

De DIP is de bestemming van de inkomende stroom. In de groep back-end stelt elke VM de gewenste service in een unieke poort op een DIP. Deze service is gekoppeld aan de frontend door middel van een regel definiëren.

We definiëren twee regels:

| Regel | Kaart-frontend | Met back-end-groep |
|------|--------------|-----------------|
| 1 | ![VIP](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) VIP1:80 | ![back-end](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) DIP1:80, ![back-end](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) DIP2:80 |
| 2 | ![VIP](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) VIP2:80 | ![back-end](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) DIP1:81, ![back-end](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) DIP2:81 |

De volledige toewijzing in Azure taakverdeling is nu als volgt:

| Regel | VIP-IP-adres | Protocol | poort | Bestemming | poort |
|------|----------------|----------|------|-----|------|
|![regel](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) 1|65.52.0.1|TCP|80|DIP-IP-adres|80|
|![regel](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) 2|65.52.0.2|TCP|80|DIP-IP-adres|81|

Elke regel moet leiden tot een stroom met een unieke combinatie van IP-adres en bestemmingspoort. Door te variëren op de doelpoort van de stroom, kunnen meerdere regels aan de dezelfde DIP op verschillende poorten stromen leveren.

Gezondheid sondes worden altijd doorgestuurd naar de DIP van een VM. U moet u zorgen dat de sonde de gezondheid van de VM weerspiegelt.

## <a name="rule-type-2-backend-port-reuse-by-using-floating-ip"></a>Regel #2 type: backend poort opnieuw gebruiken met behulp van zwevende IP

Azure Load Balancer biedt de flexibiliteit om de poort frontend hergebruiken in meerdere VIP's, ongeacht het regeltype gebruikt. Bovendien enkele scenario's van toepassing de voorkeur geeft aan of dezelfde poort moet worden gebruikt door meerdere toepassingsexemplaren op een enkele VM in de back-end-toepassingen vereisen. Gangbare voorbeelden van hergebruik van de poort zijn clusters voor hoge beschikbaarheid, virtuele toestellen en bloot meerdere TLS eindpunten zonder opnieuw te coderen.

Als u wilt dat de back-end-poort over meerdere regels gebruiken, moet u IP-drijvende inschakelen in de regeldefinitie van de.

Zwevende IP is een deel van wat bekend is als directe Server terug (DSR). DSR bestaat uit twee delen: een topologie voor stroom en een IP-adres toewijzingsschema. Op het niveau van een platform werkt Azure Load Balancer altijd in een topologie DSR stroom ongeacht of zwevende IP is ingeschakeld of niet. Dit betekent dat de uitgaande deel van een stroom altijd goed is herschreven om stroom rechtstreeks terug naar de oorsprong.

Met het standaardtype regel beschrijft Azure traditionele load-balancing toewijzingsschema voor IP-adres voor gebruiksgemak. Inschakelen, IP drijvende verandert toewijzingsschema voor de IP-adres om te zorgen voor extra flexibiliteit zoals hieronder uitgelegd.

In het volgende diagram ziet u deze configuratie:

![MultiVIP afbeelding](./media/load-balancer-multivip-overview/load-balancer-multivip-dsr.png)

In dit scenario heeft elke VM in de mediagroep Back-end drie netwerkinterfaces:

* DIP: een virtuele Netwerkkaart die is gekoppeld aan de VM (resource van Azure NIC)
* VIP1: een loopback-interface in het gastbesturingssysteem dat is geconfigureerd met het IP-adres VIP1
* VIP2: een loopback-interface in het gastbesturingssysteem dat is geconfigureerd met het IP-adres VIP2

>[AZURE.IMPORTANT] De configuratie van de logische interfaces wordt in het gastbesturingssysteem uitgevoerd. Deze configuratie wordt niet uitgevoerd of beheerd door Azure. Zonder deze configuratie functioneren de regels niet. Gezondheid sonde definities gebruiken de DIP van de VM in plaats van de logische VIP. Daarom moet uw service sonde antwoorden bieden op een DIP-poort die de status van de service die wordt aangeboden op de logische VIP weerspiegelen.

Laten we uitgaan van dezelfde configuratie als in het vorige scenario frontend:

| VIP | IP-adres | Protocol | poort |
|-----|------------|----------|------|
|![VIP](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) 1|65.52.0.1|TCP|80|
|![VIP](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) 2|*65.52.0.2*|TCP|80|

We definiëren twee regels:

| Regel | Kaart-frontend | Met back-end-groep |
|------|--------------|-----------------|
| 1 | ![regel](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) VIP1:80 | ![back-end](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) VIP1:80 (in VM1 en VM2) |
| 2 | ![regel](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) VIP2:80 | ![back-end](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) VIP2:80 (in VM1 en VM2) |

De volgende tabel ziet u de volledige toewijzing in de taakverdeling:

| Regel | VIP-IP-adres | Protocol | poort | Bestemming | poort |
|------|----------------|----------|------|-------------|------|
|![VIP](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) 1|65.52.0.1|TCP|80|Zelfde als VIP (65.52.0.1)|Zelfde als VIP (80)|
|![VIP](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) 2|65.52.0.2|TCP|80|Zelfde als VIP (65.52.0.2)|Zelfde als VIP (80)|

De bestemming van de inkomende stroom is het adres van de VIP op de loopback-interface in VM. Elke regel moet leiden tot een stroom met een unieke combinatie van IP-adres en bestemmingspoort. Door te variëren van het doel-IP-adres van de stroom, is poort hergebruik mogelijk op dezelfde VM. Uw service wordt blootgesteld aan de taakverdeling door afhankelijk te maken van de VIP-IP-adres en poort van de respectieve loopback-interface.

U ziet in het volgende voorbeeld wordt de doelpoort niet gewijzigd. Zelfs als dit een zwevende IP-scenario, ondersteunt taakverdeling Azure ook definiëren van een regel te herschrijven van de backend-doelpoort en verschilt van de doelpoort frontend.

Het type IP-zwevende regel vormt de basis van verschillende load balancer configuratie patronen. Een voorbeeld is momenteel beschikbaar is de configuratie [SQL AlwaysOn met meerdere Listeners](../virtual-machines/virtual-machines-windows-portal-sql-ps-alwayson-int-listener.md) . Na verloop van tijd zullen we meer van deze scenario's het document.

## <a name="limitations"></a>Beperkingen

* Meerdere configuraties van VIP worden alleen ondersteund met IaaS VMs.
* Met de zwevende IP-regel, moet de toepassing de DIP gebruiken voor uitgaande gegevensstromen. Als uw toepassing wordt gebonden aan het VIP-adres geconfigureerd op de loopback-interface in het gastbesturingssysteem, vervolgens SNAT is niet beschikbaar voor het herschrijven van de uitgaande stroom en de stroom uitvalt.
* Openbare IP-adressen hebben een invloed op de facturering. Zie voor meer informatie, [prijzen van IP-adres](https://azure.microsoft.com/pricing/details/ip-addresses/)
* Abonnementen gelden. Zie voor meer informatie, [Service-beperkingen](../azure-subscription-service-limits.md#networking-limits) voor meer informatie.
