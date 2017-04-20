<properties
    pageTitle="Instellen van de eindpunten van een klassieke Windows VM | Microsoft Azure"
    description="Informatie over eindpunten voor een Windows VM in Azure klassieke portal instellen voor de communicatie met een virtuele Windows-computer in Azure."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="cynthn"/>

# <a name="how-to-set-up-endpoints-on-a-classic-windows-virtual-machine-in-azure"></a>Het instellen van de eindpunten van een klassieke Windows virtuele machine in Azure


Alle vensters virtuele machines die u maakt in het klassieke implementatiemodel met Azure automatisch kunt communiceren via een particulier netwerk kanaal met andere virtuele machines in dezelfde cloud service- of virtuele netwerk. Computers op het Internet of andere virtuele netwerken vereisen echter de eindpunten aan de directe het binnenkomende netwerkverkeer op een virtuele machine. Dit artikel is ook beschikbaar voor [Linux virtuele machines](virtual-machines-linux-classic-setup-endpoints.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Eindpunten zijn geconfigureerd met behulp van **Beveiligingsgroepen netwerk (NSGs)**in het implementatiemodel **Resource Manager** . Zie voor meer informatie [toestaan externe toegang tot uw VM met behulp van de Portal Azure] (virtual-machines-windows-nsg-quickstart-portal.md).

Wanneer u een virtuele Windows-computer in de klassieke Azure portal, worden gemeenschappelijke eindpunten, zoals Extern bureaublad en Windows PowerShell Remoting meestal automatisch voor u gemaakt. Tijdens het maken van de virtuele machine of later indien nodig kunt u extra eindpunten configureren.



[AZURE.INCLUDE [virtual-machines-common-classic-setup-endpoints](../../includes/virtual-machines-common-classic-setup-endpoints.md)]

## <a name="next-steps"></a>Volgende stappen

* Als u wilt een Azure PowerShell-cmdlet gebruikt om een VM-eindpunt in te stellen, Zie [Add-AzureEndpoint](https://msdn.microsoft.com/library/azure/dn495300.aspx).

* Als u een Azure PowerShell-cmdlet wilt beheren een ACL op een eindpunt, Zie [beheren toegangsbeheerlijsten (ACL's) voor eindpunten met PowerShell](../virtual-network/virtual-networks-acl-powershell.md).

* Als u een virtuele machine hebt gemaakt in het implementatiemodel Resource Manager, kunt u Azure PowerShell [netwerk beveiligingsgroepen maken](../virtual-network/virtual-networks-create-nsg-arm-ps.md) naar verkeer voor netwerkbesturing voor VM.
