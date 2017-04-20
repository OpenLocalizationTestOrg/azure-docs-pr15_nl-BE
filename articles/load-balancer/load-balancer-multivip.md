<properties
   pageTitle="VIP's Mutiple voor een cloud-service"
   description="Overzicht van multiVIP en het instellen van meerdere VIP's in een cloud-service"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />

# <a name="configure-multiple-vips-for-a-cloud-service"></a>Meerdere VIP's voor een cloud-service configureren

Toegankelijk Azure cloud services via het openbare Internet met behulp van een IP-adres van Azure. Deze openbare IP-adres wordt aangeduid als een VIP (virtuele IP) sinds het is gekoppeld aan de Azure taakverdeling en niet de virtuele Machine (VM) van exemplaren in de cloud-service. Toegang tot elk exemplaar VM in een cloud-service met behulp van een enkel VIP.

Er zijn echter scenario's waarin u wellicht meer dan een VIP als een post verwijzen naar dezelfde cloud-service. Uw cloud-service kan bijvoorbeeld host voor meerdere websites die met behulp van poort 443, de standaardpoort van SSL-verbinding vereisen, zoals elke site wordt gehost, voor een andere klant of pachters. In dit scenario moet u een andere openbare gerichte IP-adres voor elke website. Het onderstaande diagram ziet u een typische meerdere huurder webhosting voor meerdere SSL-certificaten op de openbare poort.

![Scenario met meerdere VIP SSL](./media/load-balancer-multivip/Figure1.png)

In het bovenstaande voorbeeld alle VIP's gebruiken dezelfde openbare poort (443) en verkeer wordt omgeleid naar een of meer Netwerktaakverdeling VMs op een unieke persoonlijke poort voor het interne IP-adres van de cloud-service die als host fungeert voor alle websites.

>[AZURE.NOTE] Een andere situatie waarin het gebruik van de meerdere VIP's fungeert als host voor meerdere SQL AlwaysOn beschikbaarheid groep luisteraars op dezelfde set van virtuele Machines.

VIP's zijn standaard dynamisch, wat betekent dat het IP-adres toegewezen aan de service cloud na verloop van tijd kan worden gewijzigd. Om dat te voorkomen, kunt u een VIP reserveren voor uw service. Zie voor meer informatie over gereserveerde VIP's, [Gereserveerde openbare IP](../virtual-network/virtual-networks-reserved-public-ip.md).

>[AZURE.NOTE] Zie [IP-adres bij de prijsbepaling](https://azure.microsoft.com/pricing/details/ip-addresses/) voor informatie over de prijzen van VIP's en gereserveerde IP-adressen.

U kunt PowerShell gebruiken om te controleren of de VIP's die worden gebruikt door uw cloud-diensten, alsmede toevoegen en VIP's verwijderen, een VIP om een eindpunt te koppelen en taakverdeling in op een bepaalde VIP configureren.

## <a name="limitations"></a>Beperkingen

Op dit moment is meerdere VIP-functionaliteit beperkt tot de volgende scenario's:

- **Alleen IaaS**. U kunt alleen Multi VIP inschakelen voor cloud services met VMs. U kan meerdere VIP gebruiken in PaaS-scenario's met exemplaren van de rol.
- **Alleen PowerShell**. U kunt alleen meerdere VIP beheren met PowerShell.

Deze beperkingen zijn tijdelijk en kunnen op elk moment wijzigen. Zorg ervoor dat terugkeren naar deze pagina om te controleren of toekomstige wijzigingen.


## <a name="how-to-add-a-vip-to-a-cloud-service"></a>Een VIP toevoegen aan een service cloud

Als u wilt toevoegen een VIP met uw service, kunt u de volgende PowerShell-opdracht uitvoeren:

    Add-AzureVirtualIP -VirtualIPName Vip3 -ServiceName myService

Deze opdracht geeft een resultaat zoals in het volgende voorbeeld:

    OperationDescription OperationId                          OperationStatus
    -------------------- -----------                          ---------------
    Add-AzureVirtualIP   4bd7b638-d2e7-216f-ba38-5221233d70ce Succeeded

## <a name="how-to-remove-a-vip-from-a-cloud-service"></a>Een VIP verwijderen uit een cloud-service

Als u wilt dat de VIP met uw service in het bovenstaande voorbeeld is toegevoegd, kunt u de volgende PowerShell-opdracht uitvoeren:

    Remove-AzureVirtualIP -VirtualIPName Vip3 -ServiceName myService

>[AZURE.IMPORTANT] Een VIP kunt u alleen verwijderen als er geen eindpunten die is gekoppeld aan het.

## <a name="how-to-retrieve-vip-information-from-a-cloud-service"></a>Het VIP gegevens ophalen uit een Cloud-Service

Voer het volgende PowerShell-script voor het ophalen van de VIP's die zijn gekoppeld aan een cloud service:

    $deployment = Get-AzureDeployment -ServiceName myService
    $deployment.VirtualIPs

Het script geeft een resultaat zoals in het volgende voorbeeld:

    Address         : 191.238.74.148
    IsDnsProgrammed : True
    Name            : Vip1
    ReservedIPName  :
    ExtensionData   :

    Address         :
    IsDnsProgrammed :
    Name            : Vip2
    ReservedIPName  :
    ExtensionData   :

    Address         :
    IsDnsProgrammed :
    Name            : Vip3
    ReservedIPName  :
    ExtensionData   :

In dit voorbeeld heeft de cloud service 3 VIP's:

- **Vip1** is de standaard-VIP, weet u dat omdat de waarde voor IsDnsProgrammedName is ingesteld op true.
- **Vip2** en **Vip3** niet worden gebruikt als er geen IP-adressen. Zij wordt alleen gebruikt als u een eindpunt tot de VIP koppelen.

>[AZURE.NOTE] Uw abonnement op eBay brengt alleen voor extra VIP's nadat ze gekoppeld aan een eindpunt zijn. Zie voor meer informatie over prijzen, de [prijzen van IP-adres](https://azure.microsoft.com/pricing/details/ip-addresses/).

## <a name="how-to-associate-a-vip-to-an-endpoint"></a>Het koppelen van een VIP naar een eindpunt

Als u wilt koppelen een VIP op een eindpunt een cloud-service, moet u de volgende PowerShell-opdracht uitvoeren:

    Get-AzureVM -ServiceName myService -Name myVM1 `
  	| Add-AzureEndpoint -Name myEndpoint -Protocol tcp -LocalPort 8080 -PublicPort 80 -VirtualIPName Vip2 `
  	| Update-AzureVM

De opdracht maakt u een eindpunt dat is gekoppeld aan de VIP genaamd *Vip2* op poort *80*en koppelingen naar de VM met de naam *myVM1* in een cloud-service met de naam *MijnService* met *TCP* op poort *8080*.

Om te controleren of de configuratie, kunt u de volgende PowerShell-opdracht uitvoeren:

    $deployment = Get-AzureDeployment -ServiceName myService
    $deployment.VirtualIPs

De uitvoer ziet er uit zoals in het volgende voorbeeld:

    Address         : 191.238.74.148
    IsDnsProgrammed : True
    Name            : Vip1
    ReservedIPName  :
    ExtensionData   :

    Address         : 191.238.74.13
    IsDnsProgrammed :
    Name            : Vip2
    ReservedIPName  :
    ExtensionData   :

    Address         :
    IsDnsProgrammed :
    Name            : Vip3
    ReservedIPName  :
    ExtensionData   :

## <a name="how-to-enable-load-balancing-on-a-specific-vip"></a>Het inschakelen van taakverdeling op een bepaalde VIP

U kunt een enkele VIP koppelen aan meerdere virtuele machines voor load balancing doeleinden. U hebt bijvoorbeeld een cloud-service met de naam *MijnService*en twee virtuele machines met de naam *myVM1* en *myVM2*. En uw cloud-service heeft meerdere VIP's, één van beide met de naam *Vip2*. Als u ervoor wilt zorgen dat is alle verkeer naar poort *81* op *Vip2* verdeeld tussen *myVM1* en *myVM2* op poort *8181*, de volgende PowerShell script uitvoeren:

    Get-AzureVM -ServiceName myService -Name myVM1 `
  	| Add-AzureEndpoint -Name myEndpoint -LoadBalancedEndpointSetName myLBSet `
        -Protocol tcp -LocalPort 8181 -PublicPort 81 -VirtualIPName Vip2  -DefaultProbe `
  	| Update-AzureVM

    Get-AzureVM -ServiceName myService -Name myVM2 `
  	| Add-AzureEndpoint -Name myEndpoint -LoadBalancedEndpointSetName myLBSet `
        -Protocol tcp -LocalPort 8181 -PublicPort 81 -VirtualIPName Vip2  -DefaultProbe `
  	| Update-AzureVM

U kunt ook de taakverdeling voor het gebruik van een andere VIP bijwerken. Als u de onderstaande PowerShell-opdracht uitvoert, wordt u bijvoorbeeld de taakverdeling te gebruiken een VIP met de naam Vip1 wijzigen:

    Set-AzureLoadBalancedEndpoint -ServiceName myService -LBSetName myLBSet -VirtualIPName Vip1

## <a name="next-steps"></a>Volgende stappen

[Logboek analytics voor taakverdeling Azure](load-balancer-monitor-log.md)

[Internet gerichte load balancer-overzicht](load-balancer-internet-overview.md)

[Aan de slag op Internet facing taakverdeling](load-balancer-get-started-internet-arm-ps.md)

[Virtueel netwerk-overzicht](../virtual-network/virtual-networks-overview.md)

[Gereserveerde IP-REST API 's](https://msdn.microsoft.com/library/azure/dn722420.aspx)