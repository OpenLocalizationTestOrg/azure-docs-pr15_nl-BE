<properties
    pageTitle="Azure IaaS virtuele machines migreren van de ene Azure regio naar de andere met terugwinning van de Site | Microsoft Azure"
    description="Azure-Site-Recovery Azure regio Azure IaaS virtuele machines migreren naar een andere gebruiken."
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor="tysonn"/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/21/2016"
    ms.author="raynew"/>

#  <a name="migrate-azure-iaas-virtual-machines-between-azure-regions-with-azure-site-recovery"></a>Migratie van virtuele machines Azure IaaS tussen Azure gebieden met Azure Site herstellen

## <a name="overview"></a>Overzicht

Welkom bij herstel Azure Site! Gebruik dit artikel als u wilt migreren Azure VMs tussen Azure regio's. Houd rekening met het volgende voordat u begint:

- Azure heeft twee verschillende modellen voor het maken en werken met resources: Azure Resource Manager en klassiek. Azure heeft ook twee portals – de Azure klassieke portal die het implementatiemodel klassiek ondersteunt en de Azure portal met ondersteuning voor beide implementatiemodellen. De basisstappen voor migratie zijn hetzelfde als Recovery-Site in bronbeheer of klassiek configureren. De UI-instructies en screenshots in dit artikel zijn echter wel relevant voor de Azure portal.
- **Momenteel kunt u alleen migreren van de ene regio naar de andere. U kunt VMs uit één Azure gebied naar het andere failover, maar u kunt geen failback ze opnieuw.**
- De migratie-instructies in dit artikel zijn gebaseerd op de instructies voor het repliceren van een fysieke machine naar Azure. Bevat koppelingen naar de stappen in [VMware VMs repliceren of fysieke servers Azure](site-recovery-vmware-to-azure.md), waarin wordt beschreven hoe u een fysieke server in de portal Azure repliceren.
- Als u Site herstellen in de klassieke portal instelt, volg de gedetailleerde instructies in [dit artikel](site-recovery-vmware-to-azure-classic.md). **Niet langer gebruik** de instructies in dit [artikel met oudere](site-recovery-vmware-to-azure-classic-legacy.md).

Opmerkingen of vragen aan de onderkant van dit artikel, of op het [Forum van Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)boeken.


## <a name="prerequisites"></a>Vereisten

Dit is wat u nodig hebt voor deze installatie:

- **Configuratieserver**: een in de lokalen VM met Windows Server 2012 R2 die als de configuratieserver fungeert. U installeren de andere onderdelen Site herstellen (met inbegrip van de processerver en de doelserver master) te op deze VM. Meer [scenario architectuur](site-recovery-vmware-to-azure.md#scenario-architecture) en [configuratie van server-vereisten](site-recovery-vmware-to-azure.md#configuration-server-prerequisites).
- **IaaS virtuele machines**: de VMs die u wilt migreren. U kunt deze VMs migreren door ligaturen te beschouwen als fysieke machines.

## <a name="deployment-steps"></a>Implementatiestappen

Deze sectie beschrijft de implementatiestappen in de nieuwe portal Azure. Raadpleeg [dit artikel](site-recovery-vmware-to-azure-classic.md)als u deze implementatiestappen voor herstel van de Site in de portal voor klassieke.

1. [Een kluis maken](site-recovery-vmware-to-azure.md#create-a-recovery-services-vault).
2. [Een server configureren voor implementatie](site-recovery-vmware-to-azure.md#step-2-set-up-the-source-environment).
3. Nadat u de configuratieserver hebt geïmplementeerd, moet u controleren kan communiceren met het VMs die u wilt migreren.
4. [De replicatie-instellingen instellen](site-recovery-vmware-to-azure.md#step-4-set-up-replication-settings). Een replicatiebeleid maken en toewijzen aan de configuratieserver.
5. [De mobiliteit service installeren](site-recovery-vmware-to-azure.md#step-6-replication-application). Elke VM die u wilt beschermen, moet de mobiliteit service is geïnstalleerd. Deze service verzendt gegevens naar de processerver. De mobiliteit service kan handmatig worden geïnstalleerd of geduwd en automatisch geïnstalleerd op de server verwerken wanneer er voor de VM is beveiligd. Firewall-regels op de VMs die u wilt migreren moeten push installatie van deze service worden geconfigureerd.
6. [Replicatie inschakelen](site-recovery-vmware-to-azure.md#enable-replication). Schakel replicatie voor het VMs die u wilt migreren. U kunt de IaaS virtuele machines die u wilt migreren naar het persoonlijke IP-adres van de virtuele machines met Azure vinden. Dit adres vinden op de virtuele machine dashboard in Azure. Als u replicatie inschakelt, stelt u het machinetype voor het VMs als fysieke machines.
7. [Een niet-geplande failover worden uitgevoerd](site-recovery-failover.md#run-an-unplanned-failover). Nadat de eerste replicatie voltooid is, kunt u een niet-geplande failover uitvoeren vanaf één Azure regio naar de andere. U kunt desgewenst een herstelplan maken en uitvoeren van een niet-geplande failover, om te migreren van meerdere virtuele machines tussen regio's. [Meer informatie](site-recovery-create-recovery-plans.md) over de herstelplannen.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over andere scenario's voor replicatie in [Wat is Azure Site terugzetten?](site-recovery-overview.md)
