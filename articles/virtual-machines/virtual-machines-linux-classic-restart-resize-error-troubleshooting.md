<properties
   pageTitle="VM opnieuw op te starten of het formaat van problemen | Microsoft Azure"
   description="Klassieke implementatieproblemen oplossen met opnieuw op te starten of het formaat van een bestaande Linux virtuele Machine in Azure"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="Deland-Han"
   manager="felixwu"
   editor=""
   tags="top-support-issue"/>

<tags
   ms.service="virtual-machines-linux"
   ms.topic="support-article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="required"
   ms.date="09/20/2016"
   ms.devlang="na"
   ms.author="delhan"/>

# <a name="troubleshoot-classic-deployment-issues-with-restarting-or-resizing-an-existing-linux-virtual-machine-in-azure"></a>Klassieke implementatieproblemen oplossen met opnieuw op te starten of het formaat van een bestaande Linux virtuele Machine in Azure

> [AZURE.SELECTOR]
- [Klassiek](../articles/virtual-machines/virtual-machines-linux-classic-restart-resize-error-troubleshooting.md)
- [Resource Manager](../articles/virtual-machines/virtual-machines-linux-restart-resize-error-troubleshooting.md)

Wanneer u probeert een bestaande Azure VM verkleinen of gestopte Azure Virtual Machine (VM) start, is de gemeenschappelijke fout die u ondervindt een fout bij de toewijzing. Deze fout treedt op wanneer het cluster of het gebied geen bronnen beschikbaar of niet de gevraagde omvang in VM ondersteunt.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="collect-audit-logs"></a>Audit verzamelen zich

Als u wilt beginnen met het oplossen van problemen, verzamelen de controlelogboeken voor de fout die is gekoppeld aan het probleem.

Klik op **Bladeren**in het portal voor Azure > **virtuele machines** > _uw Linux virtuele machine_ > **Instellingen** > **controlelogboeken**.

## <a name="issue-error-when-starting-a-stopped-vm"></a>Probleem: Fout bij het starten van een gestopte VM

U probeert te starten een VM gestopt maar krijg een fout bij de toewijzing.

### <a name="cause"></a>Oorzaak

De aanvraag voor de VM gestopt moet worden geprobeerd in de oorspronkelijke cluster waarop de service cloud. Het cluster heeft echter geen vrije ruimte beschikbaar is om de aanvraag te voldoen.

### <a name="resolution"></a>Resolutie

* Een nieuwe wolk service maken en koppelen aan een regio of een virtueel netwerk op basis van de regio, maar niet een groep affiniteit.

* Verwijder de VM gestopt.

* In de nieuwe wolk service VM met behulp van de schijven opnieuw.

* Start de VM opnieuw worden gemaakt.

Als er een fout bij het maken van een nieuwe wolk service, opnieuw op een later tijdstip of de regio voor de cloud-service wijzigen.

> [AZURE.IMPORTANT] De nieuwe wolk service hebben een nieuwe naam en VIP, dus moet u die gegevens voor de afhankelijkheden die deze informatie voor de bestaande cloud-service gebruiken wijzigen.

## <a name="issue-error-when-resizing-an-existing-vm"></a>Probleem: Fout wanneer het formaat van een bestaande VM

U wilt het formaat van een bestaande VM maar krijg een fout bij de toewijzing.

### <a name="cause"></a>Oorzaak

Het verzoek om het formaat van de VM moet worden geprobeerd in de oorspronkelijke cluster waarop de service cloud. Het cluster ondersteunt echter niet de gevraagde omvang in VM.

### <a name="resolution"></a>Resolutie

De gevraagde VM verkleinen en probeer opnieuw de aanvraag formaat.

* Klik op **Bladeren, alle** > **virtuele machines (klassiek)** > _de virtuele machine_ > **Instellingen** > **formaat**. Zie de [grootte van de virtuele machine](https://msdn.microsoft.com/library/dn168976.aspx)voor gedetailleerde stappen.

Als het niet mogelijk de grootte van de VM als volgt te werk:

  * Maak een nieuwe cloud-service ervoor te zorgen dat het is niet gekoppeld aan een groep affiniteit en niet is gekoppeld aan een virtueel netwerk die is gekoppeld aan een groep affiniteit.

  * Maak een nieuw, groter formaat VM in het.

U kunt uw VMs in dezelfde service cloud consolideren. Als uw bestaande cloud-service gekoppeld aan een virtuele netwerk op basis van regio is, kunt u de nieuwe cloud-service op de bestaande virtuele netwerk.

Als de bestaande cloud-service niet gekoppeld aan een virtuele netwerk op basis van regio is, hebt u het VMs in de bestaande cloud-service verwijderen en opnieuw gecreëerd in de nieuwe wolk service van hun schijven. Het is echter belangrijk te weten dat de nieuwe wolk service heeft een nieuwe naam en VIP, dus moet u deze voor de afhankelijkheden die momenteel gebruikmaken van deze informatie voor de bestaande cloud service bijwerken.

## <a name="next-steps"></a>Volgende stappen

Als u problemen ondervindt bij het maken van een nieuwe Linux VM in Azure, Zie [problemen bij de implementatie problemen oplossen met het maken van een nieuwe virtuele machine voor Linux in Azure](../virtual-machines/virtual-machines-linux-troubleshoot-deployment-new-vm.md).
