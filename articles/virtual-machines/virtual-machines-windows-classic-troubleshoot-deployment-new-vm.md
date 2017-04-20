<properties
   pageTitle="Problemen oplossen met Windows VM implementatie klassieke | Microsoft Azure"
   description="Klassieke implementatieproblemen oplossen bij het maken van een nieuwe virtuele machine voor Windows in Azure"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="JiangChen79"
   manager="felixwu"
   editor=""
   tags="top-support-issue"/>

<tags
  ms.service="virtual-machines-windows"
  ms.workload="na"
  ms.tgt_pltfrm="vm-windows"
  ms.devlang="na"
  ms.topic="article"
  ms.date="09/06/2016"
  ms.author="cjiang"/>

# <a name="troubleshoot-classic-deployment-issues-with-creating-a-new-windows-virtual-machine-in-azure"></a>Oplossen van implementatieproblemen met het maken van een nieuwe virtuele machine voor Windows in Azure klassieke

[AZURE.INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-selectors](../../includes/virtual-machines-windows-troubleshoot-deployment-new-vm-selectors-include.md)]

[AZURE.INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-opening](../../includes/virtual-machines-troubleshoot-deployment-new-vm-opening-include.md)]

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="collect-audit-logs"></a>Audit verzamelen zich

Als u wilt beginnen met het oplossen van problemen, verzamelen de controlelogboeken voor de fout die is gekoppeld aan het probleem.

Klik op **Bladeren**in het portal voor Azure > **virtuele machines** > *uw virtuele Windows-computer* > **Instellingen** > **controlelogboeken**.

[AZURE.INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-issue1](../../includes/virtual-machines-troubleshoot-deployment-new-vm-issue1-include.md)]

[AZURE.INCLUDE [virtual-machines-windows-troubleshoot-deployment-new-vm-table](../../includes/virtual-machines-windows-troubleshoot-deployment-new-vm-table.md)]

**Y:** Als het besturingssysteem Windows generalized en deze is geüpload en/of vastgelegd met de algemene instelling, klik er niet eventuele fouten. Op dezelfde manier als het besturingssysteem is Windows gespecialiseerd, en deze is geüpload en/of vastgelegd met de instelling van de gespecialiseerde en vervolgens eventuele fouten niet.

**Uploaden van fouten:**

**N<sup>1</sup>:** Als het besturingssysteem Windows generalized en deze is geüpload als gespecialiseerd, ontvangt u een timeout inrichten met de VM blijven hangen in het OOBE-scherm.

**N<sup>2</sup>:** Als het besturingssysteem is Windows gespecialiseerd en deze is geüpload als generalized, krijgt u een inrichten is mislukt met de VM blijven hangen in het OOBE-scherm omdat de nieuwe VM wordt uitgevoerd met de oorspronkelijke computernaam, gebruikersnaam en wachtwoord.

**Resolutie:**

Upload de oorspronkelijke VHD, beschikbaar op-prem, met dezelfde instellingen als die voor het besturingssysteem (generalized/gespecialiseerde) om het oplossen van deze beide problemen. Als u wilt uploaden als generalized, moet u eerst sysprep uitvoeren. [Maken en uploaden van een Windows Server VHD naar Azure](virtual-machines-windows-classic-createupload-vhd.md) Zie voor meer informatie.

**Fouten vastleggen:**

**N<sup>3</sup>:** Als het besturingssysteem Windows generalized en deze is vastgelegd als gespecialiseerd, ontvangt u een timeout inrichten omdat de oorspronkelijke VM kan niet worden gebruikt als deze is gemarkeerd als generalized.

**N<sup>4</sup>:** Als het besturingssysteem is Windows gespecialiseerd en deze is vastgelegd als algemene, krijgt u een inrichten is mislukt omdat de nieuwe VM wordt uitgevoerd met de oorspronkelijke computernaam, gebruikersnaam en wachtwoord. Ook de oorspronkelijke VM kan niet worden gebruikt omdat deze is gemarkeerd als speciaal.

**Resolutie:**

Verwijderen de huidige afbeelding van de portal en [het opnieuw vastleggen van de huidige VHD's](virtual-machines-windows-classic-capture-image.md) met dezelfde instellingen als die voor het besturingssysteem (generalized/gespecialiseerde) om het oplossen van deze beide problemen.

## <a name="issue-custom-gallery-marketplace-image-allocation-failure"></a>Probleem: Aangepaste / galerij / marketplace-afbeelding. Fout bij het toewijzen
Deze fout zich voordoet in situaties wanneer de nieuwe VM-aanvraag wordt verzonden naar een cluster dat is geen beschikbare vrije ruimte voor de aanvraag of biedt geen ondersteuning voor de VM-grootte wordt aangevraagd. Het is niet mogelijk verschillende reeksen van VMs in dezelfde service cloud mengen. Dus als u maken van een nieuwe VM van een ander formaat wilt dan wat uw cloud-service kan ondersteunen, mislukt de compute-aanvraag.

Afhankelijk van de beperkingen van de cloud-service die de nieuwe VM maakt u kunnen er een fout wordt veroorzaakt door een van de twee situaties optreden.

**Veroorzaken 1:** De service cloud is vastgemaakt aan een specifiek cluster of het is gekoppeld aan een groep affiniteit en vandaar vastgemaakt aan een specifiek cluster in het ontwerp. Dus nieuwe compute-bron in die groep affiniteit in hetzelfde cluster waar de bestaande bronnen worden gehost worden geprobeerd-aanvragen. Echter hetzelfde cluster mogelijk geen ondersteuning voor de gevraagde omvang in VM of hebben onvoldoende beschikbare ruimte, wat resulteert in een toewijzingsfout. Dit geldt of nieuwe resources worden gemaakt via een nieuwe wolk service of via een bestaande cloud-service.

**Oplossing 1:**

- Een nieuwe wolk service maken en koppelen aan een bepaalde regio of een virtueel netwerk op basis van de regio.
- Maak een nieuwe VM in de nieuwe wolk service.
  Als er een fout bij het maken van een nieuwe wolk service, opnieuw op een later tijdstip of de regio voor de cloud-service wijzigen.

> [AZURE.IMPORTANT] Als u probeerde te maken van een nieuwe VM in een bestaande cloud service maar kon niet en had voor het maken van een nieuwe wolk service voor uw nieuwe VM, kunt u uw VMs in dezelfde service cloud consolideren. Hiertoe het VMs in de bestaande cloud-service verwijderen en deze opnieuw vastleggen van de schijven in de nieuwe wolk service. Het is echter belangrijk te weten dat de nieuwe wolk service heeft een nieuwe naam en VIP, dus moet u deze voor de afhankelijkheden die momenteel gebruikmaken van deze informatie voor de bestaande cloud service bijwerken.

**Leiden tot 2:** De cloud-service is gekoppeld aan een virtueel netwerk die is gekoppeld aan een groep affiniteit, zodat deze op een specifiek cluster is vastgemaakt aan het ontwerp. Compute resource voor alle nieuwe aanvragen in die groep affiniteit worden daarom geprobeerd in hetzelfde cluster waar de bestaande bronnen worden gehost. Echter hetzelfde cluster mogelijk geen ondersteuning voor de gevraagde omvang in VM of hebben onvoldoende beschikbare ruimte, wat resulteert in een toewijzingsfout. Dit geldt of nieuwe resources worden gemaakt via een nieuwe wolk service of via een bestaande cloud-service.

**Oplossing 2:**

- Een nieuwe regionale virtueel netwerk maken.
- De nieuwe VM in de nieuwe virtuele netwerken maken.
- [Verbinding maken met uw bestaande virtuele netwerk](https://azure.microsoft.com/blog/vnet-to-vnet-connecting-virtual-networks-in-azure-across-different-regions/) naar het nieuwe virtuele netwerk. Zie voor meer informatie over [regionale virtuele netwerken](https://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/). U kunt ook [uw affiniteit groepen gebaseerd virtueel netwerk met een virtueel netwerk van regionale migreren](https://azure.microsoft.com/blog/2014/11/26/migrating-existing-services-to-regional-scope/), en maak vervolgens de nieuwe VM.

## <a name="next-steps"></a>Volgende stappen
Als u problemen ondervindt bij het starten van een gestopte Windows VM of het formaat van een bestaande Windows VM in Azure, Zie [problemen met klassieke implementatieproblemen met opnieuw op te starten of het formaat van een bestaande virtuele Windows-computer in Azure](windows/classic/virtual-machines-windows-classic-restart-resize-error-troubleshooting.md).
