<properties
   pageTitle="VM opnieuw op te starten of het formaat van problemen | Microsoft Azure"
   description="Resource Manager distributie problemen oplossen met opnieuw op te starten of het formaat van een bestaande Linux virtuele Machine in Azure"
   services="virtual-machines-linux, azure-resource-manager"
   documentationCenter=""
   authors="Deland-Han"
   manager="felixwu"
   editor=""
   tags="top-support-issue"/>

<tags
   ms.service="virtual-machines-linux"
   ms.topic="support-article"
   ms.tgt_pltfrm="vm-linux"
   ms.devlang="na"
   ms.workload="required"
   ms.date="09/09/2016"
   ms.author="delhan"/>

# <a name="troubleshoot-resource-manager-deployment-issues-with-restarting-or-resizing-an-existing-linux-virtual-machine-in-azure"></a>Resource Manager distributie problemen oplossen met opnieuw op te starten of het formaat van een bestaande Linux virtuele Machine in Azure

Wanneer u probeert een bestaande Azure VM verkleinen of gestopte Azure Virtual Machine (VM) start, is de gemeenschappelijke fout die u ondervindt een fout bij de toewijzing. Deze fout treedt op wanneer het cluster of het gebied geen bronnen beschikbaar of niet de gevraagde omvang in VM ondersteunt.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="collect-audit-logs"></a>Audit verzamelen zich

Als u wilt beginnen met het oplossen van problemen, verzamelen de controlelogboeken voor de fout die is gekoppeld aan het probleem. De volgende koppelingen bevatten gedetailleerde informatie over het proces:

[Troubleshooting resource groep implementaties met Azure Portal](../resource-manager-troubleshoot-deployments-portal.md)

[Bewerkingen met Resource Manager controleren](../resource-group-audit.md)

## <a name="issue-error-when-starting-a-stopped-vm"></a>Probleem: Fout bij het starten van een gestopte VM

U probeert te starten een VM gestopt maar krijg een fout bij de toewijzing.

### <a name="cause"></a>Oorzaak

De aanvraag voor de VM gestopt moet worden geprobeerd in de oorspronkelijke cluster waarop de service cloud. Het cluster heeft echter geen vrije ruimte beschikbaar is om de aanvraag te voldoen.

### <a name="resolution"></a>Resolutie

*   Stop de VMs in de beschikbaarheid en start elke VM.

  1. Klik op **resourcegroepen** > _de resourcegroep_ > **Resources** > _ingesteld voor de beschikbaarheid van_ > **virtuele Machines** > _uw virtuele machine_ > **stoppen**.

  2. Nadat alle VMs stoppen, selecteert u elk van de gestopte VMs en klik op starten.

*   De aanvraag opnieuw op een later tijdstip opnieuw.

## <a name="issue-error-when-resizing-an-existing-vm"></a>Probleem: Fout wanneer het formaat van een bestaande VM

U wilt het formaat van een bestaande VM maar krijg een fout bij de toewijzing.

### <a name="cause"></a>Oorzaak

Het verzoek om het formaat van de VM moet worden geprobeerd in de oorspronkelijke cluster waarop de service cloud. Het cluster ondersteunt echter niet de gevraagde omvang in VM.

### <a name="resolution"></a>Resolutie

* De aanvraag met een kleinere VM proberen.

* Als u de grootte van de aangevraagde VM kan niet worden gewijzigd:

  1. Stop de VMs in de beschikbaarheid.

    * Klik op **resourcegroepen** > _de resourcegroep_ > **Resources** > _ingesteld voor de beschikbaarheid van_ > **virtuele Machines** > _uw virtuele machine_ > **stoppen**.

  2. Nadat alle VMs stoppen, de grootte van de gewenste VM in een groter formaat.
  3. Selecteer het nieuwe formaat VM klikt u op **Start**en start het VMs gestopt.

## <a name="next-steps"></a>Volgende stappen

Als u problemen ondervindt bij het maken van een nieuwe Linux VM in Azure, Zie [problemen bij de implementatie problemen oplossen met het maken van een nieuwe virtuele machine voor Linux in Azure](../virtual-machines/virtual-machines-linux-troubleshoot-deployment-new-vm.md).
