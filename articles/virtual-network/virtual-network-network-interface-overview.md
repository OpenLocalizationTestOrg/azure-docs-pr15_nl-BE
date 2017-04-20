<properties 
   pageTitle="Netwerkinterfaces | Microsoft Azure"
   description="Informatie over Azure netwerkinterfaces in Azure Resource Manager."
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/23/2016"
   ms.author="jdial" />

# <a name="network-interfaces"></a>Netwerkinterfaces

Een netwerkinterface (NIC) is de koppeling tussen een virtuele Machine (VM) en de onderliggende software-netwerk. In dit artikel wordt uitgelegd welke netwerkinterface is en hoe het wordt gebruikt in het implementatiemodel Azure Resource Manager.

Microsoft adviseert de implementatie van nieuwe resources met behulp van het implementatiemodel Resource Manager, maar u kunt ook VMs implementeren met netwerkverbindingen in het [klassieke](virtual-network-ip-addresses-overview-classic.md) model voor de implementatie. Als u vertrouwd met het klassieke objectmodel bent, zijn er belangrijke verschillen in VM netwerken in het implementatiemodel Resource Manager. De [virtuele machine networking - klassieke](virtual-network-ip-addresses-overview-classic.md#differences-between-resource-manager-and-classic-deployments) artikel lezen voor meer informatie over de verschillen.

In Azure, een netwerkinterface:

1. Is een hulpmiddel dat kan worden gemaakt, verwijderd, en heeft een eigen configureerbare instellingen.
2. Moet worden aangesloten op één subnet in een Azure Virtual Network (VNet) wanneer deze wordt gemaakt. Als u niet vertrouwd met VNets bent, meer informatie hierover lezen van het artikel [overzicht van virtueel netwerk](virtual-networks-overview.md) . De Netwerkkaart moet zijn verbonden met een VNet die in de Azure [locatie](https://azure.microsoft.com/regions) en [abonnement](../azure-glossary-cloud-terminology.md#subscription) als de netwerkadapterkaart bestaat. Nadat een NIC is gemaakt, kunt u het subnet dat is verbonden, maar het verbonden met VNet kunt u niet wijzigen.
3. Heeft een naam toegewezen die niet kan worden gewijzigd nadat de NIC is gemaakt. De naam moet uniek zijn binnen een Azure [resourcegroep](../azure-resource-manager/resource-group-overview.md#resource-groups), maar hoeft niet uniek zijn binnen het abonnement of de Azure locatie die wordt gemaakt in de VNet verbonden. Meerdere netwerkkaarten worden doorgaans gemaakt binnen een Azure-abonnement. Het raadzaam het opstellen van een naamgevingsconventie die wordt het beheren van meerdere NIC's gemakkelijker dan het gebruik van standaardnamen. Zie het artikel [voor naamgeving van Azure resources aanbevolen](../guidance/guidance-naming-conventions.md) voor suggesties.
4. Kan worden gekoppeld aan een VM, maar kan alleen worden gekoppeld aan een enkele VM die in dezelfde locatie als de netwerkadapterkaart voorkomt.
5. Heeft een MAC-adres met de NIC voor behouden blijft zolang het blijft gekoppeld aan een VM. Het MAC-adres wordt behouden of de VM opnieuw wordt gestart (uit in het besturingssysteem) of gestopt (opgeheven toegewezen) en gestart met de Portal Azure Azure PowerShell of de opdrachtregelinterface Azure. Als het is losgekoppeld van een VM en gekoppeld aan een andere VM, ontvangt de NIC in een ander MAC-adres. Als de Netwerkkaart is verwijderd, wordt het MAC-adres toegewezen aan andere NIC's.
6. Zijn moet een primaire **persoonlijke** *IPv4* statisch of dynamisch IP-adres toegewezen.
8. Mogelijk hebben één openbaar IP-adresbron die is gekoppeld aan het.
9. Ondersteunt versnelde netwerken met één toegangspunt i/o-virtualisatie (SR-IOV) voor specifieke VM formaten met specifieke versies van het besturingssysteem Microsoft Windows Server. Lees voor meer informatie over deze functie, het artikel [Accelerated toegang voor een virtuele machine](virtual-network-accelerated-networking-powershell.md) .
10. Verkeer dat niet bestemd is voor de particuliere IP-adressen toegewezen als IP-doorsturen is ingeschakeld voor de netwerkadapterkaart kan ontvangen Als een VM bijvoorbeeld firewall-software wordt uitgevoerd, worden deze doorgestuurd voor een eigen IP-adressen. De VM nog software kunnen routering of het doorsturen van verkeer moet worden uitgevoerd, maar hiervoor IP-forwarding moet worden ingeschakeld voor een netwerkkaart.
11. Vaak gemaakt in dezelfde bronnengroep als deze gekoppeld aan VM of het dezelfde VNet die aangesloten, maar het is niet vereist zijn.

## <a name="vms-with-multiple-network-interfaces"></a>VMs met meerdere netwerkinterfaces

Meerdere NIC's kunnen worden gekoppeld aan een VM, maar wanneer u dit doet, worden op de hoogte van de volgende opties:  

- VM-grootte moet ondersteuning bieden voor meerdere netwerkkaarten. Voor meer informatie over de VM formaten ondersteunen meerdere NIC's, leest u de artikelen [Windows Server VM maten](../virtual-machines/virtual-machines-windows-sizes.md) of [Linux VM formaten](../virtual-machines/virtual-machines-linux-sizes.md) .   
- De VM moet worden gemaakt met ten minste twee netwerkadapters. Als de VM wordt gemaakt met slechts één NIC, zelfs als de grootte van de VM meer dan één ondersteunt, kunt u extra NIC's geen voor VM toevoegen nadat deze gemaakt. Zolang de VM met ten minste twee netwerkadapters is gemaakt, kunt u koppelen extra NIC's voor VM nadat deze gemaakt, zolang de VM-grootte meer dan twee netwerkkaarten ondersteunt.  
- Als de VM ten minste drie NIC's is gekoppeld heeft, kunt u secundaire NIC's (de primaire Netwerkkaart kan niet worden ontkoppeld) uit een VM loskoppelen. U kan geen NIC's loskoppelen als VM twee heeft of minder NIC's zijn gekoppeld.  
- De volgorde van de NIC's van binnen de VM is willekeurig en ook via Azure infrastructuur updates kan wijzigen. Echter, de IP-adressen en de bijbehorende Ethernet MAC-adressen, blijven hetzelfde. Stel bijvoorbeeld dat het besturingssysteem geeft NIC1 als Eth1 Azure. Eth1 is een IP-adres 10.1.0.100 en MAC-adres 00-0D-3A-B0-39-0D. Nadat een Azure infrastructuur bijwerken en opnieuw opstarten, het besturingssysteem kan nu NIC1 als Eth2 Azure identificeren, maar de IP- en MAC-adressen niet hetzelfde als toen het besturingssysteem Azure NIC1 als Eth1 geïdentificeerd. Als opnieuw moet gestart met een klant worden, blijft de volgorde van NIC hetzelfde in het besturingssysteem.  
- Als de VM lid van een [beschikbaar](../azure-glossary-cloud-terminology.md#availability-set)is, moeten alle VMs binnen de beschikbaarheid van een NIC met één of meerdere netwerkkaarten hebben. Als de VMs meerdere netwerkkaarten, het nummer zij elke hebt, is niet hetzelfde zijn, moet als elke VM ten minste twee netwerkadapters heeft.

## <a name="next-steps"></a>Volgende stappen

- Informatie over het maken van een VM met een enkele NIC door het lezen van het artikel [een VM maken](../virtual-machines/virtual-machines-windows-hero-tutorial.md) .
- Informatie over het lezen van het artikel [Deploy een VM met meerdere NIC](virtual-network-deploy-multinic-arm-ps.md) een VM maken met meerdere netwerkkaarten.
- Informatie over het maken van een NIC met meerdere IP-configuraties door het lezen van het artikel [meerdere IP-adressen voor Azure virtuele machines](virtual-network-multiple-ip-addresses-powershell.md) .
