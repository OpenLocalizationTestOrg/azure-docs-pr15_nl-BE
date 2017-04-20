<properties
    pageTitle="Beschermen van Active Directory en DNS met Azure Site herstellen | Microsoft Azure"
    description="In dit artikel wordt beschreven hoe een disaster recovery oplossing implementeren voor Active Directory met behulp van Azure Site herstellen."
    services="site-recovery"
    documentationCenter=""
    authors="prateek9us"
    manager="abhiag"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="08/31/2016"
    ms.author="pratshar"/>

# <a name="protect-active-directory-and-dns-with-azure-site-recovery"></a>Beschermen van Active Directory en DNS met Azure Site herstellen

Zakelijke toepassingen zoals SharePoint en Dynamics AX, SAP, is afhankelijk van Active Directory en DNS-infrastructuur te laten functioneren. Als u een disaster recovery oplossing voor toepassingen maakt, is het belangrijk te weten dat u wilt beveiligen en herstellen van Active Directory en DNS vóór andere componenten van de toepassing, om ervoor te zorgen dat zaken correct werken wanneer de ramp zich voordoet.

Herstel van de site is een Azure service voor noodherstel door regie van replicatie, failover en herstel van virtuele machines. Herstel van de site ondersteunt een aantal scenario's voor replicatie op consistente wijze beschermen, en naadloos failover virtuele machines en toepassingen aan particuliere of openbare hoster wolken.

Herstel van de Site kunt u een volledig geautomatiseerde calamiteitenplan maken voor Active Directory. Wanneer zich onderbrekingen voordoen, kunt u een failover starten binnen enkele seconden vanaf elke locatie en slag Active Directory in een paar minuten. Als u Active Directory hebt geïmplementeerd voor meerdere toepassingen zoals SharePoint en SAP in de primaire site en u wilt een failover de volledige site, kunt u via Active Directory niet eerst met behulp van herstel van de Site en vervolgens een failover de andere toepassingen met behulp van toepassingsspecifieke herstelplannen.

In dit artikel wordt uitgelegd hoe een disaster recovery oplossing voor Active Directory, het uitvoeren van geplande, ongeplande en test failover met behulp van een herstelplan met één klik de ondersteunde configuraties en vereisten maken.  U moet vertrouwd zijn met Active Directory en Azure Site herstellen voordat u begint.

Er zijn twee aanbevolen opties op basis van de complexiteit van uw omgeving.

### <a name="option-1"></a>Optie 1

Als er een klein aantal toepassingen en één domeincontroller en u wilt de hele site failover, vervolgens het beste gebruiken Site herstellen naar de domeincontroller worden gerepliceerd naar de secundaire site (of u bent failover Azure of een secundaire site). Dezelfde gerepliceerde virtuele machine kan worden gebruikt voor failover-test te.

### <a name="option-2"></a>Optie 2

Als u een groot aantal toepassingen en er meer dan één domeincontroller in de omgeving is, of als u een paar toepassingen tegelijk failover, we raden naast het repliceren van de domeincontroller virtuele machine met terugwinning van de Site kunt u ook een extra domeincontroller op de doelsite (Azure of een secundaire op ruimten datacenter) instellen.

>[AZURE.NOTE] Zelfs als u optie 2 implementeren wilt, voor het uitvoeren van failover van de test moet nog steeds u voor het repliceren van de domeincontroller met herstel van de Site. Lees de [test de failover-overwegingen](#considerations-for-test-failover) voor meer informatie.


In de volgende secties wordt uitgelegd hoe inschakelen voor een domeincontroller in de Site herstellen en het instellen van een domeincontroller in Azure.


## <a name="prerequisites"></a>Vereisten

- Een implementatie op ruimten van Active Directory en DNS-server.
- Een kluis Azure-Site-Recovery Services van een abonnement op Microsoft Azure.
- Ze zijn compatibel met Azure VMs en Azure-Site-Recovery Services als u naar Azure uitvoeren het hulpprogramma evaluatie van gereedheid voor Azure Virtual Machine op VMs repliceren wilt om ervoor te zorgen.


## <a name="enable-protection-using-site-recovery"></a>Beveiliging met Site herstellen


### <a name="protect-the-virtual-machine"></a>Bescherming van de virtuele machine

Bescherming van de domeincontroller en DNS virtuele machine in Recovery Site inschakelen. Recovery-Site configureren op basis van het type van de virtuele machine (Hyper-V of VMware). Het is raadzaam een consistente replicatiefrequentie crash van 15 minuten.

###<a name="configure-virtual-machine-network-settings"></a>Virtuele machine netwerkinstellingen configureren

Voor de domain controller en DNS virtuele machine netwerkinstellingen in Recovery-Site zo configureren dat na een failover de VM wordt gekoppeld aan het juiste netwerk. Bijvoorbeeld als u Hyper-V VMs naar Azure repliceren wilt kunt u de VM in de cloud VMM of in de groep bescherming voor het configureren van de netwerkinstellingen, zoals hieronder wordt weergegeven

![VM-netwerkinstellingen](./media/site-recovery-active-directory/VM-Network-Settings.png)

## <a name="protect-active-directory-with-active-directory-replication"></a>Active Directory beveiligen met Active Directory-replicatie

### <a name="site-to-site-protection"></a>Bescherming van site naar site

Een domeincontroller op de secundaire site te maken en geef de naam van het domein dat wordt gebruikt voor de primaire site wanneer u de server om een domeincontroller te promoveren. De module **Active Directory: Sites en Services** kunt u instellingen configureren op het sitekoppelingsobject waaraan de websites worden toegevoegd. Het configureren van instellingen voor een site-koppeling, kunt u bepalen wanneer de replicatie plaatsvindt tussen twee of meer sites, en hoe vaak. Zie [Replicatie plannen tussen Sites](https://technet.microsoft.com/library/cc731862.aspx) voor meer informatie.

###<a name="site-to-azure-protection"></a>Site-Azure-bescherming

Volg de instructies voor het [maken van een domeincontroller in een virtueel netwerk van Azure](../active-directory/active-directory-install-replica-active-directory-domain-controller.md). Geef dezelfde domeinnaam die wordt gebruikt op de primaire site wanneer u de server om een domeincontroller te promoveren.

[De DNS-server voor het virtuele netwerk opnieuw configureren](../active-directory/active-directory-install-replica-active-directory-domain-controller.md#reconfigure-dns-server-for-the-virtual-network), gebruikt u de DNS-server in Azure.

![Azure netwerk](./media/site-recovery-active-directory/azure-network.png)

## <a name="test-failover-considerations"></a>Test de failover-overwegingen

Test failover plaatsvindt in een netwerk dat is afgeschermd productienetwerk zodat er geen invloed op de werkbelasting van de productie.

De meeste toepassingen vereisen ook de aanwezigheid van een domeincontroller en een DNS-server kan functioneren, dus voordat de toepassing overgenomen heeft, een domeincontroller worden gemaakt in het geïsoleerde netwerk moet moet worden gebruikt voor failover-test. De eenvoudigste manier om dit te doen is de beveiliging op de domain controller en DNS virtuele machine met terugwinning van de Site en een failover test van die virtuele machine wordt uitgevoerd voordat een failover test van het herstelplan voor de toepassing. Hier ziet u hoe u dat doet:

1. Beveiliging bij het herstellen van de Site voor de virtuele machine domain controller en DNS inschakelen.
2. Maak een geïsoleerd netwerk. Een virtueel netwerk, dat standaard wordt gemaakt in Azure is geïsoleerd van andere netwerken. Wij raden aan dat het IP-adresbereik voor dit netwerk dezelfde als die van het productienetwerk is. Geen verbinding van site naar site op dit netwerk inschakelen.
3. Bieden een DNS IP-adres in het netwerk gemaakt als het IP-adres dat u verwacht dat de DNS-virtuele machine om op te halen. Als u naar Azure repliceren wilt, geeft u het IP-adres voor de VM die wordt gebruikt in failover in **Doel-IP** -instelling in het dialoogvenster Eigenschappen van de VM. Als u naar repliceren wilt andere-lokale site en u DHCP-Volg de instructies voor het [instellen van DNS en DHCP voor failover-test](site-recovery-failover.md#prepare-dhcp)

>[AZURE.NOTE] Het IP-adres dat is toegewezen aan een virtuele machine tijdens een failover test is hetzelfde als het IP-adres dat deze tijdens een failover of niet gepland krijgt als het IP-adres in het failover-testnetwerk beschikbaar is. Dit niet het geval is, ontvangt de virtuele machine een ander IP-adres dat beschikbaar is in het netwerk van de failover-test.

4. Op de virtuele machine domain controller failover van deze test worden uitgevoerd in het geïsoleerde netwerk. Meest recente beschikbare toepassing consistent herstelpunt van de virtuele machine domain controller voor failover test gebruikt. 
5. Test voor het herstelplan toepassing failover worden uitgevoerd.
6. Nadat de test is voltooid, markeert u de taak van de failover-test van domain controller virtuele machine en het herstelplan 'Voltooid' op het tabblad **taken** in de portal-Site terugzetten.

### <a name="dns-and-domain-controller-on-different-machines"></a>DNS en domeincontroller op verschillende computers

Als DNS niet op dezelfde virtuele machine als de domeincontroller moet u een DNS VM voor failover test maken. Als ze op dezelfde VM, kunt u deze sectie overslaan.

U kunt een nieuwe DNS-server gebruiken en de vereiste zones maken. Bijvoorbeeld, als uw Active Directory-domein contoso.com, kunt u een DNS-zone met de naam contoso.com. De vermeldingen overeenkomt met Active Directory moeten worden bijgewerkt in DNS is als volgt:

1. Controleer dat deze instellingen zijn geplaatst voordat andere virtuele machine in het herstelplan is afkomstig van:

    - De zone moet de naam na de hoofdnaam van het forest.
    - De zone moet een back-bestand.
    - De zone moet worden ingeschakeld voor beveiligde en niet-beveiligde updates.
    - De omzetting van de virtuele machine domain controller moet verwijzen naar het IP-adres van de DNS-virtuele machine.

2. De volgende opdracht uitvoeren op een domeincontroller virtuele machine:

    `nltest /dsregdns`

3. Een vermelding toevoegen voor het DNS-zones op de DNS-server toevoegen en niet-beveiligde updates toestaan:

        dnscmd /zoneadd contoso.com  /Primary
        dnscmd /recordadd contoso.com  contoso.com. SOA %computername%.contoso.com. hostmaster. 1 15 10 1 1
        dnscmd /recordadd contoso.com %computername%  A <IP_OF_DNS_VM>
        dnscmd /config contoso.com /allowupdate 1


## <a name="next-steps"></a>Volgende stappen

Lees [welke werklasten kunt beveiligen?](../site-recovery/site-recovery-workload.md) voor meer informatie over het beveiligen van enterprise werkbelasting met Azure Site herstellen.
