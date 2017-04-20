<properties
    pageTitle="Amazon Web Services Windows virtuele machines migreren naar Azure met terugwinning van de Site | Microsoft Azure"
    description="In dit artikel wordt beschreven hoe Windows virtuele machines met Azure met Azure Site herstellen in Amazon Web Services (AWA) migreren."
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="backup-recovery"
    ms.date="08/22/2016"
    ms.author="raynew"/>

#  <a name="migrate-windows-virtual-machines-in-amazon-web-services-aws-to-azure-with-azure-site-recovery"></a>Windows virtuele machines in de Amazon Web Services (AWS) migreren naar Azure met Azure Site herstellen

## <a name="overview"></a>Overzicht

Welkom bij herstel Azure-Site. In dit artikel gebruiken voor het migreren van Windows exemplaren die worden uitgevoerd in de AWS naar Azure met terugwinning van de Site. Houd rekening met het volgende voordat u begint:

- Azure heeft twee verschillende modellen voor het maken en werken met resources: Azure Resource Manager en klassiek. Azure heeft ook twee portals – de Azure klassieke portal die het implementatiemodel klassiek ondersteunt en de Azure portal met ondersteuning voor beide implementatiemodellen. De basisstappen voor migratie zijn hetzelfde als Recovery-Site in bronbeheer of klassiek configureren. De UI-instructies en screenshots in dit artikel zijn echter wel relevant voor de Azure portal.
- **Momenteel kunt u alleen migreren van AWS naar Azure. U kunt VMs van AWS naar Azure failover, maar u kunt geen failback ze opnieuw. Er is geen voortdurende replicatie.**
- De migratie-instructies in dit artikel zijn gebaseerd op de instructies voor het repliceren van een fysieke machine naar Azure. Bevat koppelingen naar de stappen in [VMware VMs repliceren of fysieke servers Azure](site-recovery-vmware-to-azure.md), waarin wordt beschreven hoe u een fysieke server in de portal Azure repliceren.
- Als u Site herstellen in de klassieke portal instelt, volg de gedetailleerde instructies in [dit artikel](site-recovery-vmware-to-azure-classic.md). **Niet langer gebruik** de instructies in dit [artikel met oudere](site-recovery-vmware-to-azure-classic-legacy.md).

Opmerkingen of vragen aan de onderkant van dit artikel, of op het [Forum van Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) boeken


## <a name="prerequisites"></a>Vereisten

Hier is wat u nodig hebt voor deze implementatie

- **Configuratieserver**: een in de lokalen VM met Windows Server 2012 R2 die als de configuratieserver fungeert. U installeren de andere onderdelen Site herstellen (met inbegrip van de processerver en de doelserver master) te op deze VM. Meer [scenario architectuur](site-recovery-vmware-to-azure.md#scenario-architecture) en [configuratie van server-vereisten](site-recovery-vmware-to-azure.md#configuration-server-prerequisites).
- **VM EC2 gevallen**: de Windows-exemplaren die u wilt migreren.

## <a name="deployment-steps"></a>Implementatiestappen

Deze sectie beschrijft de implementatiestappen in de nieuwe portal Azure. Raadpleeg [dit artikel](site-recovery-vmware-to-azure-classic.md)als u deze implementatiestappen voor herstel van de Site in de portal voor klassieke.

1. [Een kluis maken](site-recovery-vmware-to-azure.md#create-a-recovery-services-vault).
2. [Een server configureren voor implementatie](site-recovery-vmware-to-azure.md#step-2-set-up-the-source-environment).
3. Nadat u de configuratieserver hebt geïmplementeerd, moet u controleren of het kan communiceren met het VMs die u wilt migreren.
4. [De replicatie-instellingen instellen](site-recovery-vmware-to-azure.md#step-4-set-up-replication-settings). Een replicatiebeleid maken en toewijzen aan de configuratieserver.
5. [De mobiliteit service installeren](site-recovery-vmware-to-azure.md#step-6-replication-application). Elke VM die u wilt beschermen, moet de mobiliteit service is geïnstalleerd. Deze service verzendt gegevens naar de processerver. De mobiliteit service kan handmatig worden geïnstalleerd of geduwd en automatisch geïnstalleerd op de server verwerken wanneer er voor de VM is beveiligd. Firewall-regels op EC2 exemplaren die u wilt migreren moeten push installatie van deze service worden geconfigureerd. De beveiligingsgroep voor EC2 gevallen moet de volgende regels:

    ![firewall-regels](./media/site-recovery-migrate-aws-to-azure/migrate-firewall.png)

6. [Replicatie inschakelen](site-recovery-vmware-to-azure.md#enable-replication). Schakel replicatie voor het VMs die u wilt migreren. U kunt met behulp van de particuliere IP-adressen, u in de console EC2 vinden kunt EC2 exemplaren vinden.
7. [Een niet-geplande failover worden uitgevoerd](site-recovery-failover.md#run-an-unplanned-failover). Nadat de eerste replicatie voltooid is, kunt u een niet-geplande failover uitvoeren vanaf AWS naar Azure voor elke VM. U kunt desgewenst een herstelplan maken en uitvoeren van een niet-geplande failover, om meerdere virtuele machines migreren van AWS naar Azure. [Meer informatie](site-recovery-create-recovery-plans.md) over de herstelplannen.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over andere scenario's voor replicatie in [Wat is Azure Site terugzetten?](site-recovery-overview.md)
