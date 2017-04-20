<properties
    pageTitle="Het plannen van het virtuele netwerk voor een Azure RemoteApp-collectie | Microsoft Azure"
    description="Informatie over het plannen van het virtuele netwerk voor een Azure RemoteApp-collectie."
    services="remoteapp"
    documentationCenter="" 
    authors="mghosh1616"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />

# <a name="how-to-plan-your-virtual-network-for-azure-remoteapp"></a>Het plannen van het virtuele netwerk voor Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt stopgezet. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

Dit document wordt beschreven hoe u uw Azure virtual network (VNET) en het subnet voor Azure RemoteApp ingesteld. Als u niet bekend met Azure virtuele netwerken bent, is dit een mogelijkheid waarmee u kunt om te virtualiseren infrastructuur van uw netwerk naar de cloud en hybride oplossingen maken met Azure en resources in ruimten. U kunt meer lezen over het [hier](../virtual-network/virtual-networks-overview.md).

Als u definiëren beveiligingsbeleid voor verkeer (binnenkomend en uitgaand) in het virtuele netwerk waar u RemoteApp Azure implementeert wilt, wordt aangeraden een apart subnet maken voor Azure RemoteApp van de rest van de distributie in de Azure virtueel netwerk. Lees voor meer informatie over het beveiligingsbeleid op uw subnet Azure virtueel netwerk definiëren [Wat is een Network Security Group (NSG)?](../virtual-network/virtual-networks-nsg.md).

## <a name="types-of-azure-remoteapp-collections-with-azure-virtual-networks"></a>Typen collecties met Azure virtuele netwerken Azure RemoteApp

De volgende afbeeldingen tonen de twee andere verzameling opties wanneer u wilt gebruiken een virtueel netwerk.

### <a name="azure-remoteapp-cloud-collection-with-vnet"></a>Azure RemoteApp-wolk-collectie met VNET

 ![Azure RemoteApp - wolk-collectie met een VNET](./media/remoteapp-planvpn/ra-cloudvpn.png)

Deze collectie een Azure RemoteApp-waarin de bronnen waartoe de RemoteApp-sessie hosts toegang moeten worden ingezet in Azure. Ze kunnen worden in de dezelfde VNET als de VNET RemoteApp of een ander VNET in Azure.

### <a name="azure-remoteapp-hybrid-collection-with-vnet"></a>Azure RemoteApp-hybride collectie met VNET

![Azure RemoteApp - collectie met een VNET hybride](./media/remoteapp-planvpn/ra-hybridvpn.png)

Deze collectie een RemoteApp-Azure waar sommige van de bronnen die de RemoteApp-sessie hosts toegang moeten krijgen tot geïmplementeerd op gebouwen zijn. De VNET RemoteApp is gekoppeld aan het op gebouwen-netwerk met behulp van Azure hybride technologieën zoals VPN of Express Route van site naar site.


## <a name="how-the-system-works"></a>De werking van het systeem

Achter de implementeert Azure RemoteApp Azure virtuele machines (met uw geüploade afbeelding) naar het virtuele netwerk subnet dat u hebt gekozen tijdens het inrichten. Als u ervoor hebt gekozen voor een hybride collectie, proberen we op te lossen, de FQDN-naam van de domeincontroller die u hebt ingevoerd in de provisioning workflow met de DNS-server die u in het virtuele netwerk.  
Als u verbinding met een bestaande virtuele netwerk maakt, zorg om de vereiste poorten in uw netwerk beveiligingsgroepen in uw subnet RemoteApp-Azure beschikbaar te maken. 

We adviseren dat u een [groot subnet voor Azure RemoteApp](remoteapp-vnetsizing.md)gebruiken. De grootste ondersteund door Azure virtueel netwerk is /8 (met behulp van definities van CIDR-subnet). Uw subnet moet groot genoeg voor alle Azure RemoteApp VMs tijdens schalen van wanneer meer gebruikers toegang hebben tot de apps. 

De dingen die u moet inschakelen op uw subnet virtueel netwerk zijn: 

2.  Uitgaand verkeer uit het subnet moet worden toegestaan op poortbereik 10101 10175 om te communiceren met een van de interne Azure RemoteApp-services.
3.  Uitgaand verkeer mag vanaf hetzelfde subnet verbinding maken met opslag op poort 443 Azure
4.  Als u Active Directory gehost in Azure, moet elke VM binnen het subnet virtueel netwerk voor Azure RemoteApp-verbinding kunnen maken met een domeincontroller. De DNS-server in het virtuele netwerk moet kunnen omzetten van de FQDN-naam van deze domeincontroller.


## <a name="virtual-network-with-forced-tunneling"></a>Virtueel netwerk met geforceerde tunneling

[Gedwongen tunneling](../vpn-gateway/vpn-gateway-about-forced-tunneling.md) wordt nu ondersteund voor alle nieuwe Azure RemoteApp-collecties. Wij ondersteuning momenteel geen voor de migratie van een bestaande collectie ter ondersteuning van gedwongen tunneling.  U moet verwijderen van de bestaande collecties met behulp van de VNET die u aan de Azure RemoteApp koppelen wilt en maak een nieuwe te krijgen gedwongen tunneling is ingeschakeld op uw verzamelingen. 
