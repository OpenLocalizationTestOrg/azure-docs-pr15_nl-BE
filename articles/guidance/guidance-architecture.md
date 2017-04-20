
<properties
   pageTitle="Azure richtlijnen | patronen en procedures | Microsoft Azure"
   description="Azure referentie-architecturen"
   services=""
   documentationCenter="na"
   authors="bennage"
   manager="marksou"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="christb"/>

# <a name="azure-reference-architectures"></a>Azure referentie-architecturen

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

_Deze inhoud is in actieve ontwikkeling. Is het handig vandaag, dus we beschikbaar te voor het voorbeeld maken zijn. We waarderen uw feedback._

Onze referentie-architecturen zijn geordend op scenario met meerdere verwante architecturen gegroepeerd.
Elke afzonderlijke architectuur biedt aanbevolen procedures en aanbevolen stappen, evenals een uitvoerbare component die de aanbevelingen.
Veel van de architectuur zijn progressieve; bouwen op voorgaande architecturen met minder vereisten.

## <a name="designing-your-infrastructure-for-resiliency"></a>Ontwerpen van uw infrastructuur voor meer herstellingsvermogen

Deze serie begint met aanbevolen procedures voor een optimale configuratie van de VM en culminates in een distributie met meerdere regio met failover.

- [Uitvoeren van een Windows VM in Azure](guidance-compute-single-vm.md)
- [Uitvoeren van een Linux VM in Azure](guidance-compute-single-vm-linux.md)
- [Uitvoeren van meerdere VMs voor schaalbaarheid en beschikbaarheid](guidance-compute-multi-vm.md)
- [Windows VMs uitvoeren voor een N-lagige architectuur](guidance-compute-n-tier-vm.md)
- [Linux VMs uitvoeren voor een N-lagige architectuur](guidance-compute-n-tier-vm-linux.md)
- [Windows VMs actief in meerdere regio's voor hoge beschikbaarheid](guidance-compute-multiple-datacenters.md)
- [Linux VMs actief in meerdere regio's voor hoge beschikbaarheid](guidance-compute-multiple-datacenters-linux.md)

## <a name="connecting-your-on-premises-network-to-azure"></a>Uw netwerk op ruimten verbinden met Azure

Deze reeks wordt gestart door het aantonen van de manieren om uw bestaande netwerk verbinden met Azure. Vervolgens uitgebreid tot de vereisten voor beschikbaarheid en beveiliging.

- [Implementatie van een hybride netwerkarchitectuur met Azure en VPN bedrijfsruimten](guidance-hybrid-network-vpn.md)
- [Implementatie van een hybride netwerkarchitectuur met Azure ExpressRoute](guidance-hybrid-network-expressroute.md)
- [Implementatie van een maximaal beschikbare hybride netwerkarchitectuur](guidance-hybrid-network-expressroute-vpn-failover.md)

## <a name="securing-your-hybrid-network"></a>Het hybride netwerk beveiligen

Deze reeks heeft betrekking op bewezen over het maken van de DMZ in Azure op beveiligde verbindingen die afkomstig zijn van uw datacenter in gebouwen en het Internet.

- [Implementatie van een DMZ tussen Azure en uw datacenter in gebouwen](guidance-iaas-ra-secure-vnet-hybrid.md)
- [Implementatie van een DMZ tussen Azure en het Internet](guidance-iaas-ra-secure-vnet-dmz.md)

## <a name="providing-identity-services"></a>Identiteit beveiligingsdiensten

Deze reeks wordt gestart door aan te tonen hoe Azure Active Directory gebruiken voor gebruikersverificatie in Azure. Vervolgens uitgebreid tot complexe scenario's voor uw ADDS infrastructuur uitbreiden naar Azure en het gebruik van AD FS voor overdracht.

- [Implementeren van Azure Active Directory](./guidance-identity-aad.md)
- [Active Directory directoryservices (ADDS) uitbreiden naar Azure](./guidance-identity-adds-extend-domain.md)
- [Maken van een forest met bronnen van Active Directory Directory Services (ADDS) in Azure](./guidance-identity-adds-resource-forest.md)
- [Het implementeren van Active Directory Federation Services (ADFS) in Azure](./guidance-identity-adfs.md)

## <a name="architecting-scalable-web-application-using-azure-paas"></a>Schaalbare webtoepassing met Azure PaaS uitbreidt

Deze serie bevat aanbevelingen voor het maken van schaalbare en uiterst beschikbare web apps. 

- [Eenvoudige webtoepassing](guidance-web-apps-basic.md)
- [Verbeteren van de schaalbaarheid in een webtoepassing](guidance-web-apps-scalability.md)
- [Webtoepassing met hoge beschikbaarheid](guidance-web-apps-multi-region.md)
