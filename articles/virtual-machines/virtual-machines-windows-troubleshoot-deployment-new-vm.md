<properties
   pageTitle="Problemen oplossen met Windows VM implementatie-RM | Microsoft Azure"
   description="Resource Manager distributie problemen oplossen bij het maken van een nieuwe virtuele machine voor Windows in Azure"
   services="virtual-machines-windows, azure-resource-manager"
   documentationCenter=""
   authors="JiangChen79"
   manager="felixwu"
   editor=""
   tags="top-support-issue, azure-resource-manager"/>

<tags
  ms.service="virtual-machines-windows"
  ms.workload="na"
  ms.tgt_pltfrm="vm-windows"
  ms.devlang="na"
  ms.topic="article"
  ms.date="09/09/2016"
  ms.author="cjiang"/>

# <a name="troubleshoot-resource-manager-deployment-issues-with-creating-a-new-windows-virtual-machine-in-azure"></a>Resource Manager distributie problemen oplossen met het maken van een nieuwe virtuele machine voor Windows in Azure

[AZURE.INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-opening](../../includes/virtual-machines-troubleshoot-deployment-new-vm-opening-include.md)]

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="collect-audit-logs"></a>Audit verzamelen zich

Als u wilt beginnen met het oplossen van problemen, verzamelen de controlelogboeken voor de fout die is gekoppeld aan het probleem. De volgende koppelingen bevatten gedetailleerde informatie over het proces te volgen.

[Resource group implementaties met Azure Portal oplossen](../resource-manager-troubleshoot-deployments-portal.md)

[Bewerkingen met Resource Manager controleren](../resource-group-audit.md)

[AZURE.INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-issue1](../../includes/virtual-machines-troubleshoot-deployment-new-vm-issue1-include.md)]

[AZURE.INCLUDE [virtual-machines-windows-troubleshoot-deployment-new-vm-table](../../includes/virtual-machines-windows-troubleshoot-deployment-new-vm-table.md)]

**Y:** Als het besturingssysteem Windows generalized en deze is geüpload en/of vastgelegd met de algemene instelling, klik er niet eventuele fouten. Op dezelfde manier als het besturingssysteem is Windows gespecialiseerd, en deze is geüpload en/of vastgelegd met de instelling van de gespecialiseerde en vervolgens eventuele fouten niet.

**Uploaden van fouten:**

**N<sup>1</sup>:** Als het besturingssysteem Windows generalized en deze is geüpload als gespecialiseerd, ontvangt u een timeout inrichten met de VM blijven hangen in het OOBE-scherm.

**N<sup>2</sup>:** Als het besturingssysteem is Windows gespecialiseerd en deze is geüpload als generalized, krijgt u een inrichten is mislukt met de VM blijven hangen in het OOBE-scherm omdat de nieuwe VM wordt uitgevoerd met de oorspronkelijke computernaam, gebruikersnaam en wachtwoord.

**Resolutie**

Deze beide fouten, gebruik [Add-AzureRmVhd voor het uploaden van de oorspronkelijke VHD](https://msdn.microsoft.com/library/mt603554.aspx), beschikbaar op-ruimten, met dezelfde instellingen als die voor het besturingssysteem (generalized/gespecialiseerde). Als u wilt uploaden als generalized, moet u eerst sysprep uitvoeren.

**Fouten vastleggen:**

**N<sup>3</sup>:** Als het besturingssysteem Windows generalized en deze is vastgelegd als gespecialiseerd, ontvangt u een timeout inrichten omdat de oorspronkelijke VM kan niet worden gebruikt als deze is gemarkeerd als generalized.

**N<sup>4</sup>:** Als het besturingssysteem is Windows gespecialiseerd en deze is vastgelegd als algemene, krijgt u een inrichten is mislukt omdat de nieuwe VM wordt uitgevoerd met de oorspronkelijke computernaam, gebruikersnaam en wachtwoord. Ook de oorspronkelijke VM kan niet worden gebruikt omdat deze is gemarkeerd als speciaal.

**Resolutie**

Verwijderen de huidige afbeelding van de portal en [het opnieuw vastleggen van de huidige VHD's](virtual-machines-windows-vhd-copy.md) met dezelfde instellingen als die voor het besturingssysteem (generalized/gespecialiseerde) om het oplossen van deze beide problemen.

## <a name="issue-customgallerymarketplace-image-allocation-failure"></a>Probleem: Galerie-aangepaste/marketplace-afbeelding. Fout bij het toewijzen
Deze fout ontstaat in situaties als de nieuwe VM-aanvraag is vastgemaakt aan een cluster dat biedt geen ondersteuning voor de VM-grootte wordt aangevraagd of heeft geen beschikbare vrije ruimte voor de aanvraag.

**Veroorzaken 1:** Het cluster kan de gevraagde omvang in VM niet ondersteunen.

**Oplossing 1:**

- De aanvraag met een kleinere VM proberen.
- Als u de grootte van de aangevraagde VM kan niet worden gewijzigd:
  - Stop de VMs in de beschikbaarheid.
  Klik op **resourcegroepen** > *de resourcegroep* > **Resources** > *ingesteld voor de beschikbaarheid van* > **virtuele Machines** > *uw virtuele machine* > **stoppen**.
  - Nadat alle VMs stoppen, maakt u de nieuwe VM in het gewenste formaat.
  - Start de nieuwe VM eerst, selecteert u elk van de gestopte VMs en klikt u op **Start**.

**Leiden tot 2:** Het cluster heeft geen vrije bronnen.

**Oplossing 2:**

- De aanvraag op een later tijdstip opnieuw.
- Als de nieuwe VM deel van een set met verschillende beschikbaarheid uitmaken kan
  - Maak een nieuwe VM in een andere beschikbaarheid instellen (in hetzelfde gebied).
  - De nieuwe VM aan hetzelfde virtuele netwerk toevoegen.

## <a name="next-steps"></a>Volgende stappen
Als u problemen ondervindt bij het starten van een gestopte Windows VM of het formaat van een bestaande Windows VM in Azure, Zie [Resource Manager oplossen van problemen bij de implementatie met opnieuw op te starten of het formaat van een bestaande virtuele Windows-computer in Azure](virtual-machines-windows-restart-resize-error-troubleshooting.md).
