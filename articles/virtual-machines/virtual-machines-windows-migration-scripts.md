<properties
    pageTitle="Communautaire instrumenten voor servicebeheer Azure Azure Resource Manager migreren"
    description="In dit artikel catalogiseren de hulpprogramma's die zijn getroffen door de Gemeenschap te helpen bij de migratie van IaaS bronnen van Azure Service Management aan de bronnenbeheerder Azure stapel."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="singhkays"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/29/2016"
    ms.author="singhkay"/>

# <a name="community-tools-for-azure-service-management-to-azure-resource-manager-migration"></a>Communautaire instrumenten voor servicebeheer Azure Azure Resource Manager migreren

In dit artikel catalogiseren de hulpprogramma's die zijn getroffen door de Gemeenschap te helpen bij de migratie van IaaS bronnen van Azure Service Management aan de bronnenbeheerder Azure stapel.

>[AZURE.NOTE]Deze hulpprogramma's worden niet officieel ondersteund door Microsoft Support. Daarom zijn openen die afkomstig zijn van Github en we willen graag PRs accepteren voor correcties of nog meer scenario's. Een probleem melden, gebruik de functie Github problemen.
>
> Met deze hulpprogramma's voor migratie wordt uitvaltijd voor de klassieke virtuele Machine. Als u op zoek bent voor de migratie van platforms ondersteund, gaat u naar 
>
>- [Migratie van IaaS bronnen van klassiek naar Azure Resource Manager stack ondersteund platform](./virtual-machines-windows-migration-classic-resource-manager.md)
>- [Technische diepe kennismaking op Platform ondersteund migratie van klassiek Azure Resource Manager](./virtual-machines-windows-migration-classic-resource-manager-deep-dive.md)
>- [IaaS resources migreren van klassiek naar Azure Resource Manager met Azure PowerShell](./virtual-machines-windows-ps-migration-classic-resource-manager.md)

## <a name="asm2arm"></a>ASM2ARM

Dit is een module PowerShell script voor het migreren van uw **één** virtuele Machine (VM) van Azure Service Management stapel op stapel Azure Resource Manager. 

[Een koppeling naar de documentatie](https://github.com/Azure/classic-iaas-resourcemanager-migration/tree/master/asm2arm)

## <a name="migaz"></a>migAz

migAz is een extra optie voor het migreren van een volledige set van Azure Service Management IaaS resources Azure Resource Manager IaaS bronnen. De migratie kan plaatsvinden binnen het hetzelfde abonnement of tussen verschillende abonnementen en abonnement typen (ex: CSP-abonnementen).

[Een koppeling naar de documentatie](https://github.com/Azure/classic-iaas-resourcemanager-migration/tree/master/migaz)