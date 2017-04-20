<properties
    pageTitle="MySQL installeren op een OpenSUSE VM | Microsoft Azure"
    description="Informatie over MySQL installeren op een OpenSUSE Linux VMirtual machine in Azure."
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
    ms.date="07/19/2016"
    ms.author="cynthn"/>

# <a name="install-mysql-on-a-virtual-machine-running-opensuse-linux-in-azure"></a>MySQL installeren op een virtuele machine met OpenSUSE Linux in Azure

[MySQL] [ MySQL] is een populaire open source SQL database. In deze zelfstudie wordt beschreven hoe u een virtuele machine waarop OpenSUSE Linux maken en vervolgens installeren MySQL.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


<br>


## <a name="create-a-virtual-machine-running-opensuse-linux"></a>Maak een virtuele machine waarop OpenSUSE Linux

[AZURE.INCLUDE [create-and-configure-opensuse-vm-in-portal](../../includes/create-and-configure-opensuse-vm-in-portal.md)]

## <a name="install-and-run-mysql-on-the-virtual-machine"></a>Installeren en uitvoeren van MySQL op de virtuele machine

[AZURE.INCLUDE [install-and-run-mysql-on-opensuse-vm](../../includes/install-and-run-mysql-on-opensuse-vm.md)]

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over MySQL [MySQL documentatie]van de[MySQLDocs].

[MySQLDocs]: http://dev.mysql.com/doc/index-topic.html
[MySQL]: http://www.mysql.com

