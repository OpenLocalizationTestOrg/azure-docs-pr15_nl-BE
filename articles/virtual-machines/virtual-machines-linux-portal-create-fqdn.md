<properties
   pageTitle="FQDN-naam maken voor een VM in Azure portal | Microsoft Azure"
   description="Informatie over het maken van een volledig gekwalificeerde domeinnaam of FQDN-naam voor een Resource Manager op basis van virtuele machines in de portal Azure."
   services="virtual-machines-linux"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor="tysonn"
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="08/23/2016"
   ms.author="iainfou"/>

# <a name="create-a-fully-qualified-domain-name-in-the-azure-portal"></a>Een volledig gekwalificeerde domeinnaam te maken in de portal voor Azure
Wanneer u een virtuele machine (VM) in [Azure portal](https://portal.azure.com) met behulp van het implementatiemodel Resource Manager maakt, wordt automatisch een openbaar IP-bron voor de virtuele machine gemaakt. Met dit IP-adres kunt u op afstand toegang krijgen tot de VM. Hoewel de portal geen een [FQDN-naam](https://en.wikipedia.org/wiki/Fully_qualified_domain_name)of FQDN-naam, standaard maakt, kunt u toevoegen zodra de VM is gemaakt. In dit artikel worden de stappen voor het maken van een DNS-naam of de FQDN-naam.

[AZURE.INCLUDE [virtual-machines-common-portal-create-fqdn](../../includes/virtual-machines-common-portal-create-fqdn.md)]

U kunt nu extern verbinding maken met de VM met behulp van deze DNS-naam, zoals `ssh adminuser@testdnslabel.centralus.cloudapp.azure.com`.

## <a name="next-steps"></a>Volgende stappen
Nu dat uw VM een openbaar IP-adres en DNS-naam heeft, kunt u implementeren algemene toepassingskaders of services zoals nginx, MongoDB, Docker, enz.

U kunt ook meer informatie over het [gebruik van Resource Manager](../azure-resource-manager/resource-group-overview.md) voor tips over het bouwen van uw Azure-implementaties.