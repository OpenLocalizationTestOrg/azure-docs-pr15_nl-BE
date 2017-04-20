<properties
    pageTitle="Wat is Systeemherstel Site? | Microsoft Azure"
    description="Biedt een overzicht van de Azure-Site-Recovery-service en geeft een overzicht van implementatiescenario's."
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="cfreeman"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="10/13/2016"
    ms.author="raynew"/>

#  <a name="what-is-site-recovery"></a>Wat is Systeemherstel Site?

Welkom bij het herstel van Azure Site! Dit artikel bevat een kort overzicht van de Site-Recovery-service en hoe het bijdraagt aan uw bedrijf.

Uw organisatie moet een bedrijfscontinuïteit en (BCDR) noodherstelplan die ervoor zorgen dat apps, werkbelasting en gegevens veilig en beschikbaar tijdens geplande en ongeplande uitvaltijd en zo spoedig mogelijk hersteld naar de normale gebruiksomstandigheden. Herstel van de site is een Azure-service die deel uitmaakt van de strategie.

Herstel site orchestrates replicatie van werkbelasting met fysieke servers op gebouwen en virtuele machines. U kunt servers en VMs uit een primaire datacenter repliceren naar de cloud (Azure) of naar een secundaire datacenter. Wanneer er storingen optreden in de primaire site, failover u uitvoeren naar de secundaire site om apps en werkbelasting toegankelijk en beschikbaar. U niet terug naar de primaire locatie als resultaat wordt gegeven voor normale bewerkingen.

## <a name="site-recovery-in-the-azure-portal"></a>Herstel van de site in de portal voor Azure

Azure heeft twee verschillende [implementatiemodellen](../resource-manager-deployment-model.md) voor het maken en werken met resources. De bronnenbeheerder Azure-model en het model Klassiek services management. Azure heeft ook twee portals – [Azure klassieke portal](https://manage.windowsazure.com/) die het implementatiemodel klassiek ondersteunt en de [Azure portal](https://portal.azure.com) met ondersteuning voor zowel het klassieke modellen Resource Manager.

- Herstel van de site is beschikbaar in zowel de klassieke portal en de Azure portal.
- In de klassieke Azure portal kunt u sites worden hersteld met het model Klassiek services management ondersteunen.
- De Azure portal kunt u het klassieke model of Resource Manager-implementaties ondersteunen. 

De informatie in dit artikel is van toepassing op classic en Azure portals. Verschillen worden vermeld, indien van toepassing.


## <a name="why-deploy-site-recovery"></a>Waarom Recovery Site implementeren?

Dit is wat Recovery-Site voor uw bedrijf kan doen:

- **BCDR vereenvoudigen**, kunt u de replicatie, failover en herstel van meerdere werklasten in één locatie in de portal Azure verwerken. Herstel site orchestrates replicatie en -failover, maar niet de toepassingsgegevens onderscheppen of informatie over het.
- **Bieden flexibele replicatie**-Site herstellen met behulp van kunt u werkbelasting uitgevoerd op ondersteunde VMs van Hyper-V, VMware VMs en fysieke Windows/Linux-servers repliceren.
- **Testen van eenvoudige replicatie uitvoeren**— Site herstel test failovers disaster recovery oefeningen, zonder gevolgen voor productieomgevingen ondersteuning biedt.
- **Failover en herstellen**, kunt u geplande failover voor de verwachte storingen met een nul-gegevens verloren gaan of niet-geplande failover uitvoeren met minimale gegevens verloren gaan (afhankelijk van de frequentie van replicatie) voor onverwachte rampen. Na een failover kunt u failback naar uw primaire sites. Herstel van de site biedt herstelplannen die scripts en automatisering Azure-werkmappen kunnen opnemen zodat u failover en het herstel van toepassingen met meerdere lagen kunt aanpassen.
- **Een secundaire datacenter te elimineren**, kunt u werkbelasting op Azure en niet op een secundaire site repliceren. Hierdoor worden de kosten en complexiteit onderhouden van een secundaire datacenter. Gerepliceerde gegevens worden opgeslagen in Azure-opslag met de veerkracht die biedt. VMs worden gemaakt met de gerepliceerde gegevens wanneer failover plaatsvindt.
- **Integratie met bestaande technologieën voor BCDR**— Site herstel is geïntegreerd met andere functies van de BCDR. Bijvoorbeeld, kunt u sites hersteld ter bescherming van de SQL Server back-end van corporate werklasten, inclusief ondersteuning voor SQL Server AlwaysOn voor het beheren van de failover-functie van de beschikbaarheid.

## <a name="what-can-i-replicate"></a>Wat kan ik repliceren?

Hier volgt een overzicht van wat u kunt repliceren met herstel van de Site.

![Voor ruimten tot op de gebouwen](./media/site-recovery-overview/asr-overview-graphic.png)

**REPLICEREN** | **REPLICEREN NAAR** 
---|---
Werkbelasting op op-premises VMs VMware | [Azure](site-recovery-vmware-to-azure-classic.md)<br/><br/> [Secundaire site](site-recovery-vmware-to-vmware.md)
Werkbelasting waarop Hyper-V-VMs voor bedrijfsruimten beheerd in VMM wolken  | [Azure](site-recovery-vmm-to-azure.md)<br/><br/> [Secundaire site](site-recovery-vmm-to-vmm.md) 
Werklast voor bedrijfsruimten Hyper-V VMs waarop beheerd in VMM wolken, met SAN-opslagruimte|  [Secundaire site](site-recovery-vmm-san.md)
Werkbelasting op voor-premises Hyper-V VMs zonder VMM | [Azure](site-recovery-hyper-v-site-to-azure.md)
Werkbelasting op fysieke Windows/Linux-servers voor gebouwen | [Azure](site-recovery-vmware-to-azure-classic.md)<br/><br/> [Secundaire site](site-recovery-vmware-to-vmware.md)


## <a name="what-workloads-can-i-protect"></a>De werkbelasting kan ik beveiligen?

Herstel van de site kan toepassingsbewuste BCDR, zodat de werkbelasting en -toepassingen uitgevoerd op een consistente manier worden wanneer er storingen optreden. Herstel van de site biedt:

- **Toepassing consistente momentopnamen**— Machines repliceren met consistente toepassing snapshots, voor één of meerdere lagen apps. Naast het vastleggen van schijfgegevens vastleggen toepassing consistente momentopnamen vastleggen alle gegevens in het geheugen en alle transacties in het proces.
- **In de buurt van de synchrone replicatie**, herstel van de Site biedt de replicatiefrequentie is slechts 30 seconden voor Hyper-V en voortdurende replicatie voor VMware.
- **Flexibele herstelplannen**— u kunt maken en aanpassen van de herstelplannen met externe scripts en handmatige acties. Integratie met Azure automatisering runbooks kunt u een volledige toepassing stapel met één klik te herstellen.
- **Integratie met SQL Server AlwaysOn**, kunt u de failover-functie van de beschikbaarheid in de herstelplannen voor herstel van de Site beheren.
- **Automation-bibliotheek**, een uitgebreide bibliotheek met Azure automatisering geeft productie-ready, specifieke scripts die kunnen worden gedownload en kan worden geïntegreerd met het herstel van de Site.
- **Simple network management**— geavanceerde network management in sites hersteld en Azure vereenvoudigt de toepassing netwerkvereisten, met inbegrip van de IP-adressen te reserveren, het configureren van netwerktaakverdeling en Azure verkeer Manager integreert voor efficiënte netwerk switchovers.


## <a name="next-steps"></a>Volgende stappen

- Lees meer in [welke werklasten kunt Recovery Site beveiligen?](site-recovery-workload.md)
- Meer informatie over de architectuur van de Site herstellen in [hoe werkt Site terugzetten?](site-recovery-components.md)
 
