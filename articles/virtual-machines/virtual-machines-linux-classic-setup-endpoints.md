<properties
    pageTitle="Instellen van de eindpunten van een klassieke Linux VM | Microsoft Azure"
    description="Meer informatie over de eindpunten van een Linux VM in Azure klassieke portal instellen voor de communicatie met een virtuele Linux machine in Azure"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/13/2016"
    ms.author="cynthn"/>

# <a name="how-to-set-up-endpoints-on-a-linux-classic-virtual-machine-in-azure"></a>Het instellen van de eindpunten van een Linux klassieke virtuele machine in Azure

Alle Linux virtuele machines die u maakt in het klassieke implementatiemodel met Azure kunnen automatisch via een particulier netwerk kanaal met andere virtuele machines in dezelfde cloud service- of virtuele netwerk communiceren. Computers op het Internet of andere virtuele netwerken vereisen echter de eindpunten aan de directe het binnenkomende netwerkverkeer op een virtuele machine. Dit artikel is ook beschikbaar voor [Windows virtuele machines](virtual-machines-windows-classic-setup-endpoints.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Eindpunten zijn geconfigureerd met behulp van **Beveiligingsgroepen netwerk (NSGs)**in het implementatiemodel **Resource Manager** . Zie voor meer informatie [openen van poorten en eindpunten] (virtuele-machines-linux-nsg-quickstart.md).

Wanneer u een virtuele Linux machine in de klassieke Azure portal maakt, wordt een eindpunt voor Secure Shell (SSH) meestal automatisch voor u gemaakt. Tijdens het maken van de virtuele machine of later indien nodig kunt u extra eindpunten configureren.
 

[AZURE.INCLUDE [virtual-machines-common-classic-setup-endpoints](../../includes/virtual-machines-common-classic-setup-endpoints.md)]

## <a name="next-steps"></a>Volgende stappen

* U kunt ook een eindpunt VM maken via de [opdrachtregelinterface van Azure](../virtual-machines-command-line-tools.md). Voer de opdracht **azure vm eindpunt maken** .

* Als u een virtuele machine hebt gemaakt in het implementatiemodel Resource Manager, kunt u de CLI Azure in Resource Manager-modus [netwerk-beveiligingsgroepen maken](../virtual-network/virtual-networks-create-nsg-arm-cli.md) voor verkeer voor netwerkbesturing voor VM.