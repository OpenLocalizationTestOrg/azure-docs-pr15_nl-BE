<properties
   pageTitle="Hoe u een virtuele Linux machine code | Microsoft Azure"
   description="Meer informatie over het coderen van een Linux virtuele machine gemaakt in het implementatiemodel Resource Manager met Azure."
   services="virtual-machines-linux"
   documentationCenter=""
   authors="mmccrory"
   manager="timlt"
   editor="tysonn"
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="07/05/2016"
   ms.author="memccror"/>

# <a name="how-to-tag-a-linux-virtual-machine-in-azure"></a>Hoe u een virtuele Linux machine code in Azure

Dit artikel beschrijft verschillende manieren naar een virtuele Linux machine code in Azure via het implementatiemodel Resource Manager. Tags zijn door de gebruiker gedefinieerde sleutel/waarde-paren die rechtstreeks op een resource of resourcegroep kunnen worden geplaatst. Azure ondersteunt momenteel maximaal 15 tags per resource en de resourcegroep. Codes is geplaatst op een bron op het moment van het maken van of toegevoegd aan een bestaande bron. Let op: tags worden ondersteund voor bronnen die zijn gemaakt via de Resource Manager alleen implementatiemodel.

[AZURE.INCLUDE [virtual-machines-common-tag](../../includes/virtual-machines-common-tag.md)]

## <a name="tagging-with-azure-cli"></a>Gelabeld met Azure CLI

Om te beginnen, [installeren en configureren van de CLI Azure](../xplat-cli-azure-resource-manager.md) en zorg ervoor dat u in de modus Resource Manager (`azure config mode arm`).

U kunt alle eigenschappen voor een bepaalde virtuele Machine, met inbegrip van de tags, met behulp van deze opdracht bekijken:

        azure vm show -g MyResourceGroup -n MyTestVM

Als u wilt een nieuwe VM-code via de CLI Azure toevoegt, kunt u de `azure vm set` en de tag-parameter **-t**opdracht:

        azure vm set -g MyResourceGroup -n MyTestVM –t myNewTagName1=myNewTagValue1;myNewTagName2=myNewTagValue2

Als u wilt dat alle codes, kunt u de parameter **-T** in de `azure vm set` opdracht.

        azure vm set – g MyResourceGroup –n MyTestVM -T


Nu dat we tags op onze middelen Azure CLI en de Portal toegepast hebben, laten we op de details van gebruik voor een overzicht van de codes in de portal voor facturering.

[AZURE.INCLUDE [virtual-machines-common-tag-usage](../../includes/virtual-machines-common-tag-usage.md)]

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over het coderen van uw resources Azure, [Azure Resource Manager-overzicht][] en [Labels gebruiken voor het ordenen van uw Resources Azure][].
* Hoe labels kunt u uw gebruik van Azure resources beheren, Zie [Wat zijn uw factuur Azure][] en [inzicht in uw verbruik van Microsoft Azure krijgen][].





[Azure CLI environment]: ./xplat-cli-azure-resource-manager.md
[Azure Resource Manager-overzicht]: ../azure-resource-manager/resource-group-overview.md
[Met behulp van labels voor het ordenen van uw Resources Azure]: ../resource-group-using-tags.md
[Wat is uw Azure-rekening?]: ../billing/billing-understand-your-bill.md
[Inzicht in uw verbruik van Microsoft Azure krijgen]: ../billing-usage-rate-card-overview.md
