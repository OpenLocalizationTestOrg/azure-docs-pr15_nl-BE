<properties
   pageTitle="Recovery Services kluis Veelgestelde vragen | Microsoft Azure"
   description="Deze versie van de veelgestelde vragen over ondersteunt de openbare Preview-versie van de back-up Azure-service. Antwoorden op veelgestelde vragen over de reservekopie-agent, back-up en behoud, herstel, beveiliging en andere veelgestelde vragen over de Azure back-upoplossing."
   services="backup"
   documentationCenter=""
   authors="markgalioto"
   manager="jwhit"
   editor=""
   keywords="back-upoplossing; back-service"/>

<tags
   ms.service="backup"
   ms.workload="storage-backup-recovery"
     ms.tgt_pltfrm="na"
     ms.devlang="na"
     ms.topic="get-started-article"
     ms.date="10/21/2016"
     ms.author="trinadhk; markgal; jimpark;"/>

# <a name="recovery-services-vault---faq"></a>Recovery Services vault - Veelgestelde vragen


Dit artikel bevat informatie die specifiek is voor de kluis Recovery Services en het vormt een aanvulling op de [Azure-Veelgestelde vragen over back-up](backup-azure-backup-faq.md). De veelgestelde vragen over back-up van Azure biedt de volledige set van vragen en antwoorden over de Azure back-up service.  

U kunt vragen stellen over Azure back-up in de sectie Disqus van dit artikel of een relevant artikel. U kunt ook vragen over de back-up Azure service boeken in het [forum voor discussie](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazureonlinebackup).

## <a name="recovery-services-vaults-are-resource-manager-based-are-backup-vaults-classic-mode-still-supported-br"></a>Recovery Services kluizen zijn Resource Manager gebaseerd. Worden back-up kluizen (klassieke modus) nog steeds ondersteund? <br/>
Ja, back-up kluizen worden nog steeds ondersteund. Back-up kluizen in de [klassieke portal](https://manage.windowsazure.com)maken. Recovery Services kluizen in [Azure portal](https://portal.azure.com)maken. Wij raden u recovery services kluis maken als alle toekomstige verbeteringen zal wel alleen beschikbaar in de Recovery Services kluis.

## <a name="can-i-migrate-a-backup-vault-to-a-recovery-services-vault-br"></a>Kan ik een back-up kluis migreren naar een kluis Recovery Services <br/>
Helaas Nee, migreren op dit moment u niet de inhoud van de kluis van een back-up naar een kluis Recovery Services. Wij werken aan deze functionaliteit toe te voegen, maar is niet beschikbaar als onderdeel van de Public Preview.

## <a name="do-recovery-services-vaults-support-classic-vms-or-resource-manager-based-vms-br"></a>Kluizen Recovery Services ondersteunen het klassieke VMs of Resource Manager op basis van VMs? <br/>
Recovery Services kluizen ondersteunen beide modellen.  U kunt back-up een VM gemaakt in de klassieke portal (de klassieke modus VMs) of een VM in Azure portal (de bronnenbeheerder gebaseerd) worden gemaakt naar een kluis Recovery Services.

## <a name="i-have-backed-up-my-classic-vms-in-backup-vault-now-i-want-to-migrate-my-vms-from-classic-mode-to-resource-manager-mode--how-can-i-backup-them-in-recovery-services-vault"></a>Ik een reservekopie van mijn klassieke VMs in back-kluis. Ik wil mijn VMs migreren van de klassieke modus naar de Resource Manager-modus.  Hoe kan ik back-up ze in recovery services kluis?
Back-ups van klassieke VMs in back-kluis won't automatisch migreren naar recovery services kluis als u het VMs van klassieke modus Resource Manager migreert. Volg de onderstaande stappen voor migratie van VM back-ups:

1. In back-kluis, Ga naar het tabblad **Beveiligde Items** en selecteer de VM. Klik op [Beveiliging stoppen](backup-azure-manage-vms-classic.md#stop-protecting-virtual-machines). Laat *verwijderen van de bijbehorende gegevens back-up* optie **uitgeschakeld**.
2. De virtuele machine van klassieke modus naar de modus Resource Manager migreren. Zorg ervoor dat de opslag- en overeenkomt met de virtuele machine ook gemigreerd naar de Resource Manager-modus.
3. Een kluis recovery services maken en configureren van back-up van de gemigreerde virtuele machine met behulp van **back-up** actie op de kluis dashboard. Meer informatie over het [inschakelen van de back-up in recovery services kluis](backup-azure-vms-first-look-arm.md)
