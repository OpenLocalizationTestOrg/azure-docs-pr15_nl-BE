<properties
    pageTitle="Overzicht van IPv6 voor Azure Load Balancer | Microsoft Azure"
    description="Informatie over IPv6-ondersteuning voor taakverdeling Azure en VMs verdeeld."
    services="load-balancer"
    documentationCenter="na"
    authors="sdwheeler"
    manager="carmonm"
    editor=""
    keywords="IPv6, azure-taakverdeling, dual-stack, openbaar IP-, systeemeigen ipv6, mobiele, iot"
/>
<tags
    ms.service="load-balancer"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="09/14/2016"
    ms.author="sewhee"
/>

# <a name="overview-of-ipv6-for-azure-load-balancer"></a>Overzicht van IPv6 voor Azure Load Balancer

Netwerktaakverdeling internetverbinding kunnen worden geïmplementeerd met een IPv6-adres. Naast IPv4-verbindingen kan dit de volgende mogelijkheden:

* Oorspronkelijke end-to-end IPv6-connectiviteit tussen de openbare Internet-clients en Azure virtuele Machines (VMs) via de taakverdeling.
* Native end-to-end IPv6 uitgaande verbindingen tussen VMs en clients met openbare Internet IPv6-ondersteuning.

In de volgende afbeelding ziet u de IPv6-functionaliteit voor taakverdeling Azure.

![Azure taakverdeling met IPv6](./media/load-balancer-ipv6-overview/load-balancer-ipv6.png)

Eenmaal geïmplementeerd, communiceren IPv4 of IPv6 ingeschakeld Internet-client met de openbare IPv4- of IPv6-adressen (of host-namen) van de taakverdeling Azure internetgerichte. De taakverdeling stuurt de IPv6-pakketten naar de particuliere IPv6-adressen van de VMs met network address translation (NAT). Het IPv6-Internet-client kan niet communiceren rechtstreeks met het IPv6-adres van het VMs.

## <a name="features"></a>Functies

IPv6-ondersteuning voor VMs geïmplementeerd via Azure Resource Manager bevat:

1. IPv6-diensten verdeeld voor IPv6-clients op het Internet
2. Systeemeigen IPv6 en IPv4-eindpunten van VMs ('dual gestapeld")
3. Inkomende en uitgaande geïnitieerde systeemeigen IPv6-verbindingen
4. Ondersteunde protocollen zoals TCP, UDP en HTTP (S) inschakelen een groot aantal service-architecturen

## <a name="benefits"></a>Voordelen

Met deze functionaliteit kunt de volgende belangrijke voordelen:

* Voldoen aan wettelijke voorschriften vereisen dat nieuwe toepassingen toegankelijk voor alleen IPv6-clients zijn
* Mobile inschakelen en het Internet van ontwikkelaars kunt gebruiken dual gestapeld (IPv4 + IPv6) Azure virtuele Machines voor de groeiende mobiele & markten IOT dingen (IOT)

## <a name="details-and-limitations"></a>Details en beperkingen

Details

* De Azure DNS-service bevat zowel een IPv4 als IPv6 AAAA-naamrecords en reageert met beide records voor de taakverdeling. De client kiezen welk adres (IPv4 of IPv6) communiceert.
* Wanneer een VM een verbinding met een openbare IPv6 met Internet verbonden apparaat initieert, is de bron van de VM IPv6-adres netwerkadres vertaling (NAT) op het openbare IPv6-adres van de taakverdeling.
* Met het besturingssysteem Linux VMs moeten worden geconfigureerd voor het ontvangen van een IPv6 IP-adres via DHCP. Veel van de Linux-afbeeldingen in de galerie met Azure zijn al geconfigureerd voor ondersteuning van IPv6 niet is gewijzigd. Voor meer informatie Zie [DHCPv6 configureren voor Linux VMs](load-balancer-ipv6-for-linux.md)
* Als u kiest voor een sonde gezondheid met de verdeling van de belasting, een IPv4-sonde maken en gebruiken met het IPv4- en IPv6-eindpunten. Als de service op uw VM uitvalt, zijn de IPv4- en IPv6-eindpunten uit rotatie genomen.

Beperkingen

* U kunt IPv6-load balancing regels in de Azure portal toevoegen. De regels kunnen alleen worden gemaakt door middel van de sjabloon, CLI, PowerShell.
* U mag geen upgrade bestaande VMs voor het gebruik van IPv6-adressen. U moet nieuwe VMs implementeren.
* Een enkel IPv6-adres kan worden toegewezen aan een afzonderlijke netwerkinterface in elke VM.
* De openbare IPv6-adressen worden niet toegewezen aan een VM. Ze kunnen alleen worden toegewezen aan een taakverdeling.
* U kunt de achterwaartse DNS-zoekopdracht niet configureren voor uw openbare IPv6-adressen.
* Het VMs met de IPv6-adressen kunnen geen lid zijn van een Azure Cloud-Service. Ze kunnen worden aangesloten op een virtueel netwerk van Azure (VNet) en met elkaar communiceren via de IPv4-adressen.
* Privé-IPv6-adressen op afzonderlijke VMs in een groep kunnen worden ingezet, maar kunnen niet worden geïmplementeerd in een resourcegroep via schaal Sets.
* Azure VMs kunnen geen verbinding maken via IPv6 andere VMs, andere Azure services of apparaten op gebouwen. Ze kunnen alleen met de Azure taakverdeling communiceren via IPv6. Echter, kunnen ze communiceren met deze andere bronnen met IPv4.
* Bescherming van netwerk Security Group (NSG) voor IPv4 wordt dual-stack (IPv4 + IPv6)-implementaties ondersteund. NSGs niet van toepassing op de eindpunten van IPv6.
* Het eindpunt van de IPv6-op de VM is niet rechtstreeks blootgesteld aan internet. Het is achter een taakverdeling. Alleen de poorten in de load balancer regels zijn toegankelijk via IPv6.
* De parameter IdleTimeout wijzigen voor IPv6 wordt **momenteel niet ondersteund**. De standaardwaarde is 4 minuten.

## <a name="next-steps"></a>Volgende stappen

Informatie over het implementeren van een taakverdeling met IPv6.

* [Beschikbaarheid van IPv6 per regio](https://go.microsoft.com/fwlink/?linkid=828357)
* [Implementeren van een taakverdeling met IPv6 met behulp van een sjabloon](load-balancer-ipv6-internet-template.md)
* [Een taakverdeling met IPv6 met Azure PowerShell implementeren](load-balancer-ipv6-internet-ps.md)
* [Implementeren van een taakverdeling met IPv6 via Azure CLI](load-balancer-ipv6-internet-cli.md)
