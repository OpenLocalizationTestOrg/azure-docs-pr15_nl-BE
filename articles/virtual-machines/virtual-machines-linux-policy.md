<properties
    pageTitle="Beleid toepassen op Azure Resource Manager virtuele Machines | Microsoft Azure"
    description="Een beleid toepassen op een Azure Linux virtuele Machine Resource Manager"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="singhkays"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="04/13/2016"
    ms.author="singhkay"/>

# <a name="apply-policies-to-azure-resource-manager-virtual-machines"></a>Beleid toepassen op Azure Resource Manager virtuele Machines

Een organisatie kan met behulp van beleid afdwingen verschillende verdragen en regels in de hele onderneming. Handhaving van het gewenste gedrag kan helpen risico en bij te dragen tot het succes van de organisatie. In dit artikel wordt beschreven hoe u kunt bronbeheer Azure beleid het gewenste gedrag definiëren voor uw organisatie van virtuele Machines.

De omtrek van de stappen om dit te bereiken is als hieronder

1. Azure Resource Manager beleid 101
2. Een beleid definiëren voor uw virtuele Machine
3. Het beleid maken
4. Het beleid wordt toegepast

## <a name="azure-resource-manager-policy-101"></a>Azure Resource Manager beleid 101

Voor aan de slag met Azure Resource Manager-beleid, wordt aangeraden het onderstaande artikel lezen en vervolgens door te gaan met de stappen in dit artikel. Het onderstaande artikel beschrijft de basisdefinitie en structuur van een beleid, hoe beleid geëvalueerd ophalen en biedt u verschillende voorbeelden van beleidsdefinities.

* [Bronnen beheren en toegang beheren](../resource-manager-policy.md)

## <a name="define-a-policy-for-your-virtual-machine"></a>Een beleid definiëren voor uw virtuele Machine

Een van de gebruikelijke scenario's voor een onderneming mogelijk alleen gebruikers met hun virtuele Machines maken van specifieke besturingssystemen die zijn getest voor compatibiliteit met een LOB-toepassing. Met behulp van een beleid voor Azure Resource Manager kan deze taak worden uitgevoerd in een paar stappen. In dit voorbeeld beleid gaan we toestaan alleen Ubuntu 14.04.2-LTS virtuele Machines te maken. De definitie van het beleid eruitziet als hieronder

```
"if": {
  "allOf": [
    {
      "field": "type",
      "equals": "Microsoft.Compute/virtualMachines"
    },
    {
      "not": {
        "allOf": [
          {
            "field": "Microsoft.Compute/virtualMachines/imagePublisher",
            "equals": "Canonical"
          },
          {
            "field": "Microsoft.Compute/virtualMachines/imageOffer",
            "equals": "UbuntuServer"
          },
          {
            "field": "Microsoft.Compute/virtualMachines/imageSku",
            "equals": "14.04.2-LTS"
          }
        ]
      }
    }
  ]
},
"then": {
  "effect": "deny"
}
```

Het bovenstaande beleid gemakkelijk kan worden gewijzigd met een scenario waar u toestaan een Ubuntu LTS afbeelding wilt mogelijk moet worden gebruikt voor de implementatie van een virtuele Machine met de onderstaande wijzigen

```
{
  "field": "Microsoft.Compute/virtualMachines/imageSku",
  "like": "*LTS"
}
```

#### <a name="virtual-machine-property-fields"></a>Eigenschapsvelden virtuele Machine

In de onderstaande tabel beschrijft de eigenschappen van virtuele Machine die kunnen worden gebruikt als de velden in de definitie van het beleid. Zie het onderstaande artikel voor meer informatie over velden voor het beleid:

* [Velden en bronnen](../resource-manager-policy.md#fields-and-sources)


| Veldnaam     | Beschrijving                                        |
|----------------|----------------------------------------------------|
| imagePublisher | Hiermee geeft u de uitgever van de afbeelding               |
| imageOffer     | Hiermee geeft u het aanbod voor de gekozen afbeelding uitgever |
| imageSku       | Hiermee geeft u de SKU voor de aanbieding van het gekozen             |
| imageVersion   | Hiermee geeft u de afbeeldingsversie voor de gekozen SKU     |

## <a name="create-the-policy"></a>Het beleid maken

Een beleid kan gemakkelijk worden gemaakt met de REST API rechtstreeks of de PowerShell-cmdlets. Voor het maken van het beleid, Zie het artikel hieronder:

* [Een beleid maken](../resource-manager-policy.md#creating-a-policy)


## <a name="apply-the-policy"></a>Het beleid wordt toegepast

Na het maken van het beleid moet u deze toepassen op een welbepaald. De scope is een abonnement, resourcegroep of zelfs het middel. Voor de toepassing van het beleid, Zie het artikel hieronder:

* [Een beleid maken](../resource-manager-policy.md#applying-a-policy)
