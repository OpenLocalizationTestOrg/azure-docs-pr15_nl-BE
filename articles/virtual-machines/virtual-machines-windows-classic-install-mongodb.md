<properties
    pageTitle="MongoDB installeren op een Windows VM | Microsoft Azure"
    description="Informatie over het installeren van MongoDB op een Azure VM gemaakt met het klassieke implementatiemodel met Windows Server."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="iainfou"/>

#<a name="install-mongodb-on-a-windows-vm"></a>MongoDB installeren op een Windows VM

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Als u wilt installeren en configureren met behulp van het implementatiemodel Resource Manager MongoDB, Zie [dit artikel](virtual-machines-windows-classic-install-mongodb.md).

[MongoDB] [ MongoDB] is een populaire open-source, krachtige NoSQL database. Dit artikel helpt u bij het maken van een Windows Server virtuele machine (VM) met de [Azure klassieke portal][AzurePortal]. U maakt en een schijf met gegevens koppelen aan de VM voor het installeren en configureren van MongoDB. Als u een bestaande VM in Azure die u wilt gebruiken, kunt u rechtstreeks naar het [installeren en configureren van MongoDB](#install-and-run-mongodb-on-the-virtual-machine)springen.


## <a name="create-a-virtual-machine-running-windows-server"></a>Een virtuele machine met Windows Server maken

Volg deze instructies voor het maken van een virtuele machine.

[AZURE.INCLUDE [virtual-machines-create-WindowsVM](../../includes/virtual-machines-create-windowsvm.md)]

> [AZURE.NOTE] Een eindpunt voor MongoDB toevoegen tijdens het maken van de virtuele machine en configureer als volgt: naam **Mongo**, **TCP** gebruikt als het protocol en de openbare en particuliere poorten ingesteld op **27017**.

## <a name="attach-a-data-disk"></a>Een schijf met gegevens koppelen
Opslag voor de virtuele machine, koppelt u een schijf met gegevens en initialiseren zodat Windows kunt gebruiken. Als u al een schijf met gegevens, kunt u de bestaande schijf koppelen of kunt u een lege schijf koppelen.

[AZURE.INCLUDE [howto-attach-disk-windows-linux](../../includes/howto-attach-disk-windows-linux.md)]

Zie voor instructies voor het initialiseren van de schijf, "hoe: initialiseren van een gegevensschijf in Windows Server" in [het koppelen van een schijf met gegevens naar een virtuele Windows-computer](virtual-machines-windows-classic-attach-disk.md).

## <a name="install-and-run-mongodb-on-the-virtual-machine"></a>Installeren en uitvoeren van MongoDB op de virtuele machine

[AZURE.INCLUDE [install-and-run-mongo-on-win2k8-vm](../../includes/install-and-run-mongo-on-win2k8-vm.md)]

## <a name="summary"></a>Samenvatting
In deze zelfstudie hebt u geleerd hoe maakt een virtuele machine met Windows Server op afstand verbinding te maken en koppelen van een schijf met gegevens.  Ook hebt u geleerd hoe u kunt installeren en configureren van MongoDB op de virtuele machine onder Windows. U hebt nu toegang tot MongoDB op Windows gebaseerde virtuele machine door de volgende geavanceerde onderwerpen in de [documentatie van MongoDB][MongoDocs].

[MongoDocs]: http://docs.mongodb.org/manual/
[MongoDB]: http://www.mongodb.org/
[AzurePortal]: http://manage.windowsazure.com
