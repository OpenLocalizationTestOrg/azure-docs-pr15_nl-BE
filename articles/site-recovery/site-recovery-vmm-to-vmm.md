<properties
    pageTitle="Hyper-V virtuele machines in de wolken VMM repliceren naar een secundaire VMM-site met de Azure portal | Microsoft Azure"
    description="Beschrijving van het Azure Site herstellen als u wilt repliceren, failover en herstel van Hyper-V VMs in wolken VMM aan een secundaire VMM-site met behulp van de portal Azure evoluerende implementeren."
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/23/2016"
    ms.author="raynew"/>

# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-a-secondary-vmm-site-using-the-azure-portal"></a>Hyper-V virtuele machines in de wolken VMM repliceren naar een secundaire VMM-site met behulp van de portal Azure

> [AZURE.SELECTOR]
- [Azure portal](site-recovery-vmm-to-vmm.md)
- [Klassieke portal](site-recovery-vmm-to-vmm-classic.md)
- [PowerShell - Resource Manager](site-recovery-vmm-to-vmm-powershell-resource-manager.md)

Welkom bij herstel Azure Site! Gebruik dit artikel als u wilt repliceren op ruimten Hyper-V virtuele machines in de wolken van System Center Virtual Machine Manager (VMM) naar een secundaire site beheerd. In dit artikel wordt beschreven hoe een replicatie met Azure Site herstellen in de portal Azure.

> [AZURE.NOTE] Azure heeft twee verschillende [implementatiemodellen](../resource-manager-deployment-model.md) voor het maken en werken met resources: Azure Resource Manager en klassiek. Azure heeft ook twee portals – de Azure klassieke portal die het implementatiemodel klassiek ondersteunt en de Azure portal met ondersteuning voor beide implementatiemodellen.


Azure-Site herstellen in de portal Azure biedt verschillende nieuwe functies:

- In de Azure worden portal, de back-up Azure en Azure-Site-Recovery services gecombineerd tot een enkel kluis Recovery Services, zodat u kunt instellen en bedrijfscontinuïteit en noodherstel (BCDR) vanaf één locatie beheren. Een centrale dashboard kunt u controleren en beheren van bewerkingen in uw sites op gebouwen en de openbare Azure cloud.
- Gebruikers met Azure abonnementen met Cloud Solution Provider (CSP) wordt ingericht kunnen nu terugvorderingen Site in Azure portal beheren.


Boeken opmerkingen onderaan in de Disqus opmerkingen na het lezen van dit artikel. Technische vragen op het [Forum van Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="overview"></a>Overzicht

Organisaties hebben behoefte aan een strategie voor BCDR die bepaalt hoe apps, werkbelasting en gegevens blijven actief en beschikbaar tijdens geplande en ongeplande uitvaltijd en normale gebruiksomstandigheden zo snel mogelijk herstellen. Uw strategie voor BCDR moet u bedrijfsgegevens veilig en terug te vorderen en dat werklasten continu beschikbaar blijven als ramp plaatsvindt.

Herstel van de site is een Azure service die deel uitmaakt van uw strategie voor BCDR door replicatie van fysieke servers op gebouwen en virtuele machines naar de cloud (Azure) of een secundaire datacenter slaagde. Wanneer er storingen optreden in de primaire locatie, failover u uitvoeren naar de secundaire locatie apps en werkbelasting beschikbaar te houden. U niet terug naar de primaire locatie als resultaat wordt gegeven voor normale bewerkingen. Meer informatie in [Wat is Azure Site terugzetten?](site-recovery-overview.md)

Dit artikel bevat alle informatie die u wilt repliceren op-premises Hyper-V VMs in wolken VMM aan een secundaire VMM-site. Het bevat een architectonisch overzicht, informatie en implementatiestappen voor het configureren van servers op lokalen, replicatie-instellingen en capaciteitsplanning plannen. Nadat u hebt ingesteld met de infrastructuur kunt u replicatie op computers die u wilt beveiligen en controleren dat failover werkt.

## <a name="business-advantages"></a>Voordelen voor uw bedrijf

- Herstel van de site biedt bescherming voor business werkbelasting en toepassingen die worden uitgevoerd in Hyper-V VMs door ze te repliceren naar een secundaire server voor Hyper-V.
- De portal Recovery Services biedt één locatie wilt instellen, beheren en controleren van replicatie, failover en herstel.
- U kunt failover eenvoudig uitvoeren uit uw primaire op gebouwen-infrastructuur van de secundaire site naar de primaire failback (herstellen) en de secundaire site uitvoeren.
- U kunt herstelplannen met meerdere computers zo configureren dat trapsgewijze werkbelasting van toepassingen aan elkaar failover.

## <a name="scenario-architecture"></a>Scenario-architectuur

Dit zijn de onderdelen:

- **Primaire site**: In de primaire site er een of meer servers met Hyper-V host met VMs bron die u wilt repliceren. Deze primaire host-servers bevinden zich in een private cloud VMM.
- **Secundaire site**: In de secundaire site worden een of meer Hyper-V hostservers met doel VMs waaraan u de primaire VMs repliceren. Deze hostservers bevinden zich in een private cloud VMM. De cloud kan zijn op de primaire server (als u slechts één server VMM) of op een secundaire server voor de VMM.
- **Provider**: tijdens de implementatie van het herstel van de Site, u de Provider Azure Site herstellen op de VMM-servers installeren en registreren die servers in een kluis Recovery Services. De Provider op de VMM-server communiceert met Site herstellen via HTTPS-443 orchestration repliceren. Gegevensreplicatie plaatsvindt tussen primaire en secundaire Hyper-V host-servers. Gerepliceerde gegevens blijft binnen sites op de gebouwen en netwerken en niet verzonden naar Azure. Meer informatie over [privacy](#privacy-information-for-site-recovery).

![E2E-topologie](./media/site-recovery-vmm-to-vmm/architecture.png)

### <a name="data-privacy-overview"></a>Overzicht van privacy

Deze tabel bevat een overzicht van hoe gegevens worden opgeslagen in dit scenario:
****
Actie | **Details** | **Verzamelde gegevens** | **Gebruik** | **Vereist**
--- | --- | --- | --- | ---
**Registratie** | U kunt een VMM-server registreren in een kluis Recovery Services. Als u later de registratie van een server, kunt u dit doen door het verwijderen van de servergegevens uit de Azure portal. | Nadat een VMM-server is geregistreerd herstel van de Site worden verzameld, processen, en de metagegevens over de VMM-server en de namen van de VMM wolken gedetecteerd door sites worden hersteld. | De gegevens worden gebruikt om te identificeren en communiceren met de juiste VMM-server en -instellingen configureren voor een juiste VMM wolken. | Deze functie is vereist. Als u niet wilt dat deze gegevens te verzenden naar de Site herstellen kunt u de Site-Recovery-service niet gebruiken.
**Replicatie inschakelen** | De Provider Azure Site herstellen op de VMM-server is geïnstalleerd en is het kanaal voor communicatie met de Site-Recovery-service. De Provider is een dynamic link library (DLL) die worden gehost in het proces van VMM. Nadat u de voorziening hebt geïnstalleerd, wordt de functie 'Herstel Datacenter' ingeschakeld in de beheerdersconsole van VMM. Nieuwe en bestaande VMs kunnen deze functie inschakelen voor een VM inschakelen. | Met deze eigenschap wordt ingesteld verzendt de Provider de naam en ID van de VM voor herstel van de Site.  Replicatie is ingeschakeld op Windows Server 2012- of Windows Server 2012 R2 Hyper-V-Replica. De virtuele machine-gegevens worden gerepliceerd van de ene Hyper-V host naar de andere (meestal te vinden in een datacenter verschillende "recovery"). | Site herstellen gebruikt de metagegevens voor het vullen van de informatie VM in Azure portal. | Deze functie is een essentieel onderdeel van de service en kan niet worden uitgeschakeld. Als u niet wilt dat deze informatie te verzenden, geen bescherming voor VMs Recovery Site inschakelen. Houd er rekening mee dat alle gegevens die door de Provider Site nuttige toepassing worden verzonden via HTTPS wordt verzonden.
**Herstelplan** | Herstelplannen helpen u bij het maken van een planning orchestration voor de recovery Datacenter. U kunt de volgorde waarin VMs of een groep van virtuele machines moet worden gestart op de herstel-site definiëren. Ook kunt u een geautomatiseerde scripts worden uitgevoerd of een handmatige actie moet worden ondernomen, op het moment van herstel voor elke VM. Failover wordt doorgaans geactiveerd op het niveau van de recovery plan voor gecoördineerde herstel. | Herstel van de site worden verzameld, verwerkt en metagegevens voor het herstelplan, met inbegrip van de metagegevens van de virtuele machine en metagegevens van een automatische scripts en handmatige actie notities worden verzonden. | De metagegevens wordt gebruikt voor het bouwen van het herstelplan in Azure portal. | Deze functie is een essentieel onderdeel van de service en kan niet worden uitgeschakeld. Als u niet wilt dat deze gegevens te verzenden naar het herstel van de Site, maak geen herstelplannen.
**Netwerk toewijzen** | Kaarten netwerkinformatie uit het primaire gegevenscentrum te herstellen. Wanneer een VMs worden hersteld op de site van herstel, helpt netwerk toewijzen bij het maken van verbinding met het netwerk. | Herstel van de site worden verzameld, verwerkt en verzendt de metagegevens van de logische netwerken voor elke site (primaire en datacenter). | De metagegevens wordt gebruikt om de netwerkinstellingen te vullen zodat u de netwerkgegevens kunt toewijzen. | Deze functie is een essentieel onderdeel van de service en kan niet worden uitgeschakeld. Als u niet wilt dat deze gegevens te verzenden naar de Site herstellen, hoeft u netwerk toewijzen.
**Failover (gepland/niet-geplande/test)** | Failover wordt overgenomen VMs uit één VMM beheerd Datacenter naar het andere. De failover-actie wordt handmatig geactiveerd in de portal Azure. | De Provider op de VMM server Recovery Site ontvangt van de failover-gebeurtenis en een failover-actie wordt uitgevoerd op de Hyper-V host via VMM-interfaces. Werkelijke failover van een VM is van een Hyper-V host naar een ander en verwerkt door Windows Server 2012- of Windows Server 2012 R2 Hyper-V-Replica. Nadat failover uitgevoerd is, verzendt de Provider op de VMM-server in het datacenter herstel gegevens met succes tot herstel van de Site. | Herstel van de site gebruikt de informatie verzonden naar de status van de failover-actie-informatie in de portal Azure vullen. | Deze functie is een essentieel onderdeel van de service en kan niet worden uitgeschakeld. Als u niet wilt dat deze gegevens te verzenden naar de Site herstellen, hoeft u failover.


## <a name="azure-prerequisites"></a>Azure-vereisten

Dit is wat u nodig hebt in Azure dit scenario te implementeren:

**Vereisten** | **Details**
--- | ---
**Azure**| U moet een account van [Microsoft Azure](http://azure.microsoft.com/) . U kunt beginnen met een [gratis proefperiode](https://azure.microsoft.com/pricing/free-trial/). [Meer informatie](https://azure.microsoft.com/pricing/details/site-recovery/) over het herstel van de Site-prijzen.


## <a name="on-premises-prerequisites"></a>Vereisten voor gebouwen

Dit is wat u nodig hebt in de primaire en secundaire op-premises sites implementeren van dit scenario:

**Vereisten** | **Details**
--- | ---
**VMM** | U implementeert een server VMM in de primaire site en een VMM-server in de secundaire site het beste.<br/><br/> U kunt ook [repliceren tussen wolken op één server VMM](site-recovery-single-vmm.md). Hiervoor moet u ten minste twee wolken op de VMM-server geconfigureerd.<br/><br/> VMM-servers waarop ten minste System Center 2012 SP1 met de nieuwste updates.<br/><br/> Elke server VMM moet op een of meer wolken geconfigureerd en alle wolken moeten de capaciteit van de Hyper-V-profiel instellen. <br/><br/>Wolken moeten een of meer VMM hostgroepen bevatten.<br/><br/>Meer informatie over het instellen van VMM wolken in [de cloud VMM fabric configureren](https://msdn.microsoft.com/library/azure/dn469075.aspx#BKMK_Fabric), en [procedure: persoonlijke wolken maken met System Center 2012 SP1 VMM](http://blogs.technet.com/b/keithmayer/archive/2013/04/18/walkthrough-creating-private-clouds-with-system-center-2012-sp1-virtual-machine-manager-build-your-private-cloud-in-a-month.aspx).<br/><br/> VMM-servers moeten de toegang tot het internet.
**Hyper-V** | Hyper-V-servers moeten actief zijn ten minste Windows Server 2012 met de functie Hyper-V en de laatste updates hebt geïnstalleerd.<br/><br/> Hyper-V-server moet een of meer VMs bevatten.<br/><br/>  Hyper-V host-servers zich bevinden in de hostgroepen in de primaire en secundaire VMM wolken.<br/><br/> Als u Hyper-V in een cluster op Windows Server 2012 R2 uitvoert moet u [update 2961977](https://support.microsoft.com/kb/2961977) te installeren<br/><br/> Als u Hyper-V in een cluster op Windows Server 2012, houd er rekening mee dat cluster broker wordt niet automatisch gemaakt als u een statisch IP-adres gebaseerde cluster. U moet handmatig de broker cluster configureren. [Lees meer](http://social.technet.microsoft.com/wiki/contents/articles/18792.configure-replica-broker-role-cluster-to-cluster-replication.aspx).
**Provider** | Tijdens de implementatie van herstel van de Site, kunt u de Provider Azure Site herstel installeren op servers van VMM. De Provider communiceert met herstel van de Site via HTTPS-443 goedkeuringen van replicatie. Gegevensreplicatie plaatsvindt tussen de primaire en secundaire Hyper-V-servers via het LAN of via een VPN-verbinding.<br/><br/> De Provider op de VMM-server nodig heeft voor toegang tot deze URL's: *. hypervrecoverymanager.windowsazure.com, *. AccessControl.Windows.NET; *. backup.windowsazure.com, *. BLOB.Core.Windows.NET; *. store.core.windows.net.<br/><br/> Bovendien firewall communicatie van de VMM-servers op de [IP-adresbereiken Azure datacenter](https://www.microsoft.com/download/confirmation.aspx?id=41653) en kunnen het protocol HTTPS (443).

## <a name="prepare-for-deployment"></a>Voorbereiden voor distributie

Voorbereiden voor distributie, u moet:

1. [De server VMM voorbereiden](#prepare-the-vmm-server) voor herstel van de Site-implementatie.
2. [Voorbereiden voor het netwerk toewijzen](#prepare-for-network-mapping). Netwerken zo instellen dat u de toewijzing van het netwerk kunt configureren.

### <a name="prepare-the-vmm-server"></a>Voorbereiding van de VMM-server

Zorg ervoor dat de VMM-server aan de [vereisten voldoet](#on-premises-prerequisites) en toegang de vermelde URL tot.


### <a name="prepare-for-network-mapping"></a>Voorbereiden voor het netwerk toewijzen

Netwerk toewijzen toewijzingen tussen VMM VM-netwerken op de primaire en secundaire VMM-servers:

- Replica VMs op secundaire Hyper-V hosts optimaal plaats na een failover.
- Replica VMs verbinding met passende VM-netwerken.
- Als u geen netwerk toewijzing replica die VMS niet na een failover wordt aangesloten op een netwerk.
- Als u wilt instellen van netwerk is toewijzen tijdens het herstel van de Site hier implementatie wat u nodig hebt:

    - Zorg ervoor dat VMs op de bronserver voor Hyper-V host met een netwerk van VMM VM verbonden bent. Dit netwerk moet worden gekoppeld aan een logisch netwerk dat is gekoppeld aan de cloud.
    - Controleer of de secundaire wolk die u voor herstel gebruikt heeft een bijbehorende VM netwerk geconfigureerd. VM netwerk moet worden gekoppeld aan een logisch netwerk dat is gekoppeld aan de secundaire wolk.

- [Meer informatie](site-recovery-network-mapping.md) over de werking van de toewijzing van het netwerk.

## <a name="prepare-for-deployment-with-a-single-vmm-server"></a>Voorbereiden voor implementaties met één server VMM

Als u slechts één server VMM repliceert u VMs in Hyper-V hosts in de cloud VMM [Azure](site-recovery-vmm-to-azure.md) of een secundaire VMM-wolk. Wij raden u aan dat de eerste optie omdat repliceren tussen wolken niet naadloos, maar als u nodig hebt om dit te doen hier is wat u moet doen:

1. **VMM op een Hyper-V VM instellen**. Wij bevelen dat u SQL Server-sessie door VMM op de dezelfde VM colocate wanneer we dit doen. Dit bespaart u tijd als slechts één VM heeft gemaakt. Als u extern exemplaar van SQL Server en een storing optreedt, moet u dat exemplaar herstellen voordat u de VMM kunt herstellen.
2. **Zorg dat de VMM-server heeft ten minste twee wolken geconfigureerd**. Een wolk bevat de VMs u wilt repliceren en de andere wolk fungeert als secundaire locatie. [Vereisten](#on-premises-prerequisites)moet voldoen aan de cloud met het VMs die u wilt beveiligen.
3. Recovery-Site instellen, zoals beschreven in dit artikel. Maken en de VMM-server registreren in de kluis, instellen van een replicatiebeleid voor en replicatie inschakelen. U moet opgeven dat de initiële replicatie plaatsvindt via het netwerk.
4. Bij het instellen van het netwerk toewijzen wijst u het VM-netwerk voor de primaire wolk met de VM-netwerk voor de secundaire wolk.
5. In de console van Hyper-V-beheer inschakelen van Hyper-V-Replica op de Hyper-V host waarop de VM VMM en Schakel replicatie op de VM. Zorg ervoor dat u de VMM virtuele machine niet toevoegen aan de wolken die worden beveiligd door het herstellen van een Site om ervoor te zorgen dat Replica Hyper-V-instellingen niet worden overschreven door de Site herstellen.
6. Als u de herstelplannen voor failover maakt u dezelfde server VMM voor bron en doel.
7. U failover en als volgt herstellen:

    - Handmatig een failover de VMM VM aan de secundaire site met Hyper-V-beheer met een geplande failover.
    - Het VMs failover.
    - Nadat de primaire VMM VM is hersteld, meld u aan op de Azure portal -> Recovery Services kluis en een niet-geplande failover van de VMs van de secundaire site uitvoeren op de primaire site.
    - Nadat de niet-geplande failover uitgevoerd is, alle bronnen kunnen worden benaderd vanuit de primaire site opnieuw.


### <a name="create-a-recovery-services-vault"></a>Maak een kluis Recovery Services

1. Log in om de [Azure portal](https://portal.azure.com).
2. Klik op **nieuwe** > **Management** > **Recovery Services**. U kunt ook op **Bladeren klikken** > kluizen**Recovery Services** > **toevoegen**.

    ![Nieuwe kluis](./media/site-recovery-vmm-to-vmm/new-vault3.png)

3. Geef in het vak **naam** een beschrijvende naam ter identificatie van de kluis. Als u meer dan één abonnement hebt, selecteert u een van deze.
4. [Een nieuwe resourcegroep maken](../resource-group-template-deploy-portal.md) of Selecteer een bestaande en een Azure regio opgeven. Machines worden gerepliceerd naar deze regio. Ondersteunde regio's Zie geografische beschikbaarheid in [Azure Site herstel prijzen Details](https://azure.microsoft.com/pricing/details/site-recovery/)
4. Als u wilt snel toegang tot de kluis van het Dashboard op **aan het dashboard vastmaken** > **kluis maken**.

    ![Nieuwe kluis](./media/site-recovery-vmm-to-vmm/new-vault-settings.png)

De nieuwe kluis wordt weergegeven op het **Dashboard** > **alle bronnen**, en in het hoofdmenu **Recovery Services kluizen** blade.




## <a name="getting-started"></a>Aan de slag

Herstel van de site biedt een introductie ervaring waarmee u zo snel mogelijk implementeren. Aan de slag, controleert de vereisten en begeleidt u bij de Site herstel implementatie in de juiste volgorde stappen.

Introductie selecteert u het type waar u wilt repliceren naar en van computers die u wilt repliceren. U servers voor ruimten instellen, replicatie-beleid maken en capaciteitsplanning. Nadat u hebt ingesteld om uw infrastructuur schakelt u replicatie voor VMs. U kunt uitvoeren van failover voor specifieke computers of herstelplannen failover op meerdere computers te maken.

Beginnen aan de slag door te kiezen hoe u wilt implementeren Site herstellen. De stroom aan de slag verandert enigszins afhankelijk van uw vereisten voor replicatie.

## <a name="step-1-choose-your-protection-goal"></a>Stap 1: Kies uw doel bescherming

Selecteer wat u wilt repliceren en waar u wilt repliceren naar.

1. Selecteer uw kluis in de **Recovery Services kluizen** blade en klikt u op **Instellingen**.
2. In de **Instellingen** > **Aan de slag** Klik op **Herstel van de Site** > **stap 1: voorbereiden infrastructuur** > **doelstelling van bescherming**.
3. In de **doelstelling van bescherming** **tot herstel site**selecteren en selecteer **Ja, met Hyper-V**.
4. Selecteer **Ja** om aan te geven dat u de Hyper-V hosts beheren en klik op **Ja** als er een secundaire server voor de VMM VMM gebruikt. Als u replicatie tussen wolken op één server VMM, klik u op **Nee**. Klik vervolgens op **OK**.

    ![Doelstellingen kiezen](./media/site-recovery-vmm-to-vmm/choose-goals.png)


## <a name="step-2-set-up-the-source-environment"></a>Stap 2: De bronomgeving instellen

De Azure Site herstel Provider installeren op VMM-servers en servers registreren in de kluis.

1. Klik op **stap 2: voorbereiden van infrastructuur** > **bron**.

    ![Bron instellen](./media/site-recovery-vmm-to-vmm/goals-source.png)

2. Klik op **+ VMM** als een VMM-server wilt toevoegen in **de bron voorbereiden** .

    ![Bron instellen](./media/site-recovery-vmm-to-vmm/set-source1.png)

2. In de **Server toevoegen** blade controle dat **System Center VMM-server** wordt weergegeven in het **type Server** en die de VMM-server voldoet aan de [voorwaarden en vereisten van de URL](#on-premises-prerequisites).
4. Download het installatiebestand Azure Site Recovery-Provider.
5. Download de sleutel van de registratie. U moet dit wanneer u setup uitvoert. De code is geldig tot 5 dagen nadat u het hebt gegenereerd.

    ![Bron instellen](./media/site-recovery-vmm-to-vmm/set-source3.png)

6. De Provider Azure Site terugzetten op de VMM-server installeren.

> [AZURE.NOTE] U hoeft niet expliciet geïnstalleerd op de host-servers Hyper-V.


### <a name="set-up-the-azure-site-recovery-provider"></a>Instellen van de Provider Azure Site herstellen

1. De Provider worden uitgevoerd op elke server VMM setup-bestand. VMM is geïmplementeerd in een cluster en u installeert de voorziening voor de eerste keer installeren op een actief knooppunt als de installatie om de VMM-server registreren in de kluis te voltooien. Vervolgens installeert u de Provider op de andere knooppunten. Clusterknooppunten moeten alle dezelfde versie van de Provider uitgevoerd.
2. Setup enkele prerequirement controles uitgevoerd en vraagt toestemming de VMM-service te stoppen. De VMM-Service worden gestart nadat setup is voltooid. Als u op een servercluster VMM installeert wordt u gevraagd om te stoppen met de rol van het Cluster.

2.  In **Microsoft Update** kunt u opt-in voor updates zodat Provider updates zijn geïnstalleerd volgens het beleid van Microsoft Update.
3. In de **installatie** accepteren of wijzigen van de standaardlocatie van de Provider en klik op **installeren**.

    ![Installatielocatie](./media/site-recovery-vmm-to-vmm/provider-location.png)

3. Klik op **registreren** als u de server registreren in de kluis wilt nadat de installatie is voltooid.

    ![Installatielocatie](./media/site-recovery-vmm-to-vmm/provider-register.png)

9. Controleer of de naam van de kluis waarin de server wordt geregistreerd in het vak **naam van de kluis**. Klik op *volgende*.

    ![Server-registratie](./media/site-recovery-vmm-to-vmm-classic/vaultcred.PNG)

7. Opgeven hoe de Provider op de VMM-server verbinding maakt met Internet in **Verbinding met Internet** . Selecteer **verbinding maken met bestaande proxy-instellingen** op over het gebruik van Internet-verbinding geconfigureerd op de server.

    ![Internet-instellingen](./media/site-recovery-vmm-to-vmm-classic/proxydetails.PNG)

    - Als u een aangepaste proxy gebruiken moet u deze instellen voordat u de voorziening hebt geïnstalleerd. Wanneer u aangepaste proxy-instellingen configureren wordt een test uitgevoerd om te controleren van de proxyverbinding.
    - Als u een aangepaste proxy gebruikt, of de standaardproxy verificatie u de proxy-details vereist moet, inclusief proxy-adres en poort opgeven.
    - Volgende URL's moet toegankelijk zijn vanuit de VMM-Server en de Hyper-v hosts
        - *. hypervrecoverymanager.windowsazure.com
        - *. accesscontrol.windows.net
        - *. backup.windowsazure.com
        - *. blob.core.windows.net
        - *. store.core.windows.net
    - Toestaan dat de IP-adressen die worden beschreven in (443) [Azure Datacenter IP-bereiken](https://www.microsoft.com/download/confirmation.aspx?id=41653) en HTTPS-protocol. U moet IP-adresbereiken witte lijst van het Azure regio die u wilt gebruiken en die van de VS West.
    - Als u een aangepaste proxy een VMM RunAs-account (DRAProxyAccount) automatisch met de proxyreferenties opgegeven gemaakt. De proxy-server zo configureren dat deze account met succes kan worden geverifieerd. De VMM RunAs account-instellingen kunnen worden gewijzigd in de console van VMM. Hiervoor openen de werkruimte **Instellingen** , **beveiliging**, klik op **Uitvoeren als Accounts**en wijzigt u het wachtwoord voor DRAProxyAccount. U moet de VMM-service opnieuw starten zodat deze instelling van kracht wordt.


8. Selecteer de sleutel die u hebt gedownload van Azure Site herstellen en gekopieerd naar de server VMM in **Registratie sleutel**.


10.  De coderingsinstelling van de wordt alleen gebruikt wanneer u VMs Hyper-V in wolken VMM naar Azure repliceren bent. Als u naar een secundaire site repliceren wilt wordt niet gebruikt.

11.  Geef in het vak **servernaam**een beschrijvende naam voor de VMM-server in de kluis. Geef in een clusterconfiguratie de rol van VMM clusternaam.
12.  In de **metagegevens van de cloud synchroniseren** , selecteer of u wilt om metagegevens te synchroniseren voor alle wolken op de VMM-server met de kluis. Deze actie moet slechts één keer gebeuren op elke server. U kunt als u niet dat alle wolken synchroniseren wilt, schakelt u deze instelling niet en elke wolk afzonderlijk in de eigenschappen van de wolk in de VMM-console synchroniseren.

13.  Klik op **volgende** om het proces te voltooien. Na registratie wordt metagegevens van de VMM-server opgehaald door Azure Site herstellen. De server wordt weergegeven op het tabblad **Servers van VMM** op de pagina **Servers** in de kluis.

    ![Server](./media/site-recovery-vmm-to-vmm-classic/provider13.PNG)

11. Nadat de server beschikbaar in de Site-herstelconsole in de **bron is** > **voorbereiden bron** de VMM-server selecteren en selecteer de wolk waarin de Hyper-V host zich bevindt. Klik vervolgens op **OK**.

#### <a name="command-line-installation"></a>Installatie via opdrachtregel

De Provider Azure Site herstel kan worden geïnstalleerd vanaf de opdrachtregel. Deze methode kan worden gebruikt om de voorziening hebt geïnstalleerd op een Server Core van Windows Server 2012 R2.

1. De Provider installatie en registratie sleutel downloaden naar een map. Bijvoorbeeld C:\ASR.
2. De System Center Virtual Machine Manager-Service stoppen.
3. Vanaf een opdrachtprompt uitvoeren van deze opdrachten om uit te pakken van de installateur van de Provider:

        C:\Windows\System32> CD C:\ASR
        C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q

4. Uitvoeren van deze opdracht kunt u de voorziening hebt geïnstalleerd:

        C:\ASR> setupdr.exe /i

5. Deze opdrachten naar de server registreren in de kluis vervolgens uitvoeren:

        CD C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin
        C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin\> DRConfigurator.exe /r  /Friendlyname <friendly name of the server> /Credentials <path of the credentials file> /EncryptionEnabled <full file name to save the encryption certificate>     

Wanneer de parameters zijn:

 - **/Credentials**: verplichte parameter die de locatie waar het sleutelbestand registratie  
 - **/FriendlyName**: verplichte parameter voor de naam van de Hyper-V host-server die wordt weergegeven in de portal Azure Site herstellen.
 - **/EncryptionEnabled**: optionele Parameter die u alleen gebruikt bij het repliceren van VMM van Azure.
 - **/proxyAddress**: optionele parameter die het adres van de proxyserver.
 - **/proxyport**: optionele parameter die de poort van de proxyserver.
 - **/proxyUsername**: optionele parameter die de naam Proxy (als de proxy verificatie vereist).
 - **/proxyPassword**: optionele parameter die het wachtwoord voor verificatie met de proxyserver (als de proxy verificatie vereist).  

## <a name="step-3-set-up-the-target-environment"></a>Stap 3: Instellen van de doelomgeving.

Selecteer de doelserver VMM en cloud.

1. Klik op **infrastructuur voorbereiden** > **doel** en selecteer de doelserver VMM u wilt gebruiken.
2.  Wolken op de server die zijn gesynchroniseerd met het herstel van de Site wordt weergegeven. Selecteer het doel-wolk.

    ![Doel](./media/site-recovery-vmm-to-vmm/target-vmm.png)

## <a name="step-4-set-up-replication-settings"></a>Stap 4: Instellen van de replicatie-instellingen

1. Voor het maken van een nieuwe replicatie beleid **voorbereiden infrastructuur**klikt u op > **De replicatie-instellingen** > **+ maken en koppelen**.

    ![Netwerk](./media/site-recovery-vmm-to-vmm/gs-replication.png)

2. Geef de naam van een beleid **maken** en koppelen beleid. Het type bron en doel moet **Hyper-V**.
3. Selecteer welk besturingssysteem wordt uitgevoerd op de host in **Hyper-V host versie** .

    > [AZURE.NOTE] De cloud VMM Hyper-V hosts met verschillende (ondersteunde) versies van Windows Server bevatten, maar een replicatiebeleid toegepaste hosts waarop hetzelfde besturingssysteem wordt uitgevoerd. Als u met meer dan één besturingssysteemversie hosts vervolgens afzonderlijke replicatie beleid te maken.

4. In het **type verificatie** en **verificatiepoort** opgeven hoe verkeer tussen de primaire en herstel Hyper-V host-servers wordt geverifieerd. **Certificaat** selecteren, tenzij u beschikt over een werkende Kerberos-omgeving. Azure-Site-Recovery configureert automatisch certificaten voor HTTPS-verificatie. U hoeft niet alles handmatig doen. Poort 8083 en 8084 (voor certificaten) wordt standaard geopend in Windows Firewall op de host-servers Hyper-V. Als u **Kerberos**selecteert, wordt een Kerberos-ticket gebruikt voor wederzijdse verificatie van de hostservers. Opmerking deze instelling is alleen relevant voor Hyper-V host-servers waarop Windows Server 2012 R2.
3. Opgeven hoe vaak gegevens worden gerepliceerd delta na de eerste replicatie (elke 30 seconden, 5 of 15 minuten) in het **kopiëren van de frequentie** .
4. In **herstel punt vasthouden**, opgeven in uren hoe lang het venster inhoudingen worden voor elk herstelpunt. Beveiligde computers kunnen worden hersteld naar een willekeurig punt in een venster.
6. In **App consistent momentopname frequentie** bepaalt hoe vaak (1-12 uur) herstel punten met consistente toepassing momentopnamen zijn gemaakt. Hyper-V worden twee soorten momentopnamen, een momentopname van een standaard waarmee een incrementele momentopname van de hele virtuele machine en een momentopname van een consistente toepassing die een point-in-time momentopname van de toepassingsgegevens in de virtuele machine. Volume Shadow Copy Service (VSS) toepassing consistente momentopnamen gebruiken om ervoor te zorgen dat de toepassingen in een consistente status zijn als de momentopname is gemaakt. Houd er rekening mee dat als u een toepassing consistente momentopnamen inschakelt, geldt dat de prestaties van toepassingen die worden uitgevoerd op de bron virtuele machines. Zorg ervoor dat de ingestelde waarde lager is dan extra herstel punten die u configureert.
7. In de **compressie van de gegevensoverdracht** opgeven of gerepliceerde gegevens die wordt overgedragen moet worden gecomprimeerd.
8. Selecteer **VM replica verwijderen** om op te geven dat de virtuele machine van replica worden verwijderd als u beveiliging voor de bron VM uitschakelt. Als u deze instelling inschakelt wanneer u de beveiliging voor de bron wordt deze verwijderd uit de Site Herstelconsole VM uitschakelt, herstel van de Site-instellingen voor de VMM worden verwijderd uit de console VMM en de replica wordt verwijderd.
3. In de **initiële replicatiemethode** als u te via het netwerk repliceren bent opgeven of de eerste replicatie starten of plannen. U bespaart netwerkbandbreedte mogelijk u wilt plannen buiten de drukke uren. Klik vervolgens op **OK**.

    ![Replicatiebeleid](./media/site-recovery-vmm-to-vmm/gs-replication2.png)

6. Wanneer u een nieuw beleid maakt is deze automatisch gekoppeld aan de VMM wolk. Klik op **OK**in het **replicatiebeleid** . U kunt extra VMM wolken (en de VMs erin) koppelen aan dit replicatiebeleid in de **Instellingen** > **replicatie** > beleidsnaam > **VMM wolk koppelen**.

    ![Replicatiebeleid](./media/site-recovery-vmm-to-vmm/policy-associate.png)

### <a name="prepare-for-offline-initial-replication"></a>Voorbereiden voor off line initiële replicatie

U kunt off line replicatie voor de initiële gegevens doen. U kunt dit als volgt voorbereiden:

- Op de bronserver, moet u een locatie waar het exporteren van gegevens zal plaatsvinden. Volledig beheer toewijzen voor NTFS- en Share-machtigingen de VMM-service het exportpad. Op de doelserver, zult u een padlocatie van waaruit het importeren van gegevens wordt uitgevoerd. Op dit Importpad dezelfde machtigingen toewijzen.
- Als het pad of uitvoer wordt gedeeld, toewijzen Administrator, gebruiker, Printeroperator of serveroperator groepslidmaatschap voor de VMM-serviceaccount op de externe computer waarop de gedeelde zich bevindt.
- Als u geen accounts uitvoeren als hosts toevoegen op de paden importeren en exporteren, toewijzen lezen en schrijven naar de rekeningen uitvoeren als in de VMM.
- Importeren en exporteren van aandelen zich niet bevinden op elke computer die wordt gebruikt als een Hyper-V host-server omdat de loopback-configuratie wordt niet ondersteund door Hyper-V.
- In Active Directory beperkt host-server met virtuele machines die u wilt beveiligen, inschakelen en configureren op elke Hyper-V overdracht om het vertrouwen van de externe computers waarop de paden importeren en exporteren, als volgt bevinden:
    1. Open **Active Directory: gebruikers en Computers**op de domeincontroller.
    2. Klik in de consolestructuur op **domeinnaam** > **Computers**.
    3. Klik met de rechtermuisknop op de naam van de Hyper-V host > **Eigenschappen**.
    4. Klik op het tabblad **overdracht** op **deze computer mag overdragen aan de opgegeven services alleen**.
    5. Klik op **elk protocol voor verificatie gebruiken**.
    6. Klik op **toevoegen** > **-gebruikers en Computers**.
    7. Typ de naam van de computer waarop het exportpad > **OK**. In de lijst met beschikbare services, houdt u CTRL ingedrukt en klik op **cifs** > **OK**. Herhaal dit voor de naam van de computer waarop het importpad. Herhaal deze stappen voor de extra host-servers Hyper-V.


### <a name="configure-network-mapping"></a>Toewijzing van netwerk configureren

Netwerk toewijzing instellen tussen bron-en doel.

- [Lees](#prepare-for-network-mapping) voor een snel overzicht van welke toewijzing netwerk heeft. In deze aanvulling [lezen](site-recovery-network-mapping.md) voor een uitgebreidere uitleg.
- Of virtuele machines op VMM-servers zijn verbonden met een netwerk voor VM.

Configureer als volgt toewijzen:

1. **Instellingen** > **Site infrastructuur** > **Mapping netwerk** > **netwerk toewijzingen** , klikt u op **+ netwerk toewijzen**.

    ![Netwerk toewijzen](./media/site-recovery-vmm-to-azure/network-mapping1.png)

2. Selecteer de bron in een tabblad **toevoegen netwerk toewijzen** en VMM doelservers. De VM-netwerken die zijn gekoppeld aan de VMM-servers worden opgehaald.
3. Selecteer het netwerk dat u wilt gebruiken in de lijst van VM-netwerken die zijn gekoppeld aan de primaire server van VMM in **Bronnetwerk**.
6. Selecteer het netwerk dat u wilt gebruiken op de secundaire server voor de VMM in het **doelnetwerk** . Klik vervolgens op **OK**.

    ![Netwerk toewijzen](./media/site-recovery-vmm-to-vmm/network-mapping2.png)

Dit is wat er gebeurt als het netwerk toewijzing begint:

- Enige bestaande replica virtuele machines die met de bron VM netwerk overeenkomen wordt verbonden met het netwerk VM.
- Nieuwe virtuele machines die zijn verbonden met de bron VM netwerk worden verbonden met het doelnetwerk toegewezen na replicatie.
- Als u een bestaande toewijzing met een nieuw netwerk wijzigt, worden replica virtuele machines verbonden met de nieuwe instellingen.
- Als het netwerk meerdere subnetten heeft en een van de subnetten heeft dezelfde naam als het subnet waarop de virtuele machine van bron zich bevindt, zal dan de replica virtuele machine worden aangesloten op dat subnet doel na een failover. Als er geen subnet doel met een overeenkomende naam, wordt de virtuele machine worden verbonden met het eerste subnet in het netwerk.

### <a name="configure-storage-mapping"></a>Toewijzing van opslag configureren
Standaard wanneer u een virtuele machine op een bron Hyper-V host-server naar een Hyper-V host doelserver repliceert, gerepliceerde gegevens opgeslagen op de standaardlocatie die wordt aangegeven voor de doelhost Hyper-V in Hyper-V-beheer. Voor meer controle over welke gerepliceerde gegevens zijn opgeslagen, kunt u de toewijzing van opslagruimte<br/><br/> Opslag toewijzing configureren, moet u instellen opslag classificaties op de bron en VMM doelservers voordat u implementeren gaat. Opslag toewijzing via nieuwe Azure portal wordt momenteel niet ondersteund. Het kan echter worden ingeschakeld via Powershell. [Meer informatie](site-recovery-vmm-to-vmm-powershell-resource-manager.md#step-6-configure-storage-mapping).

## <a name="step-5-capacity-planning"></a>Stap 5: Capaciteitsplanning

Nu dat u uw basic hebt infrastructuur instellen u kunt denken over capaciteitsplanning en uitvinden of u aanvullende bronnen nodig hebt.

Herstel van de site biedt een planner capaciteit op basis van Excel zodat u de juiste resources toewijzen voor uw bronomgeving, site herstel onderdelen, netwerk en opslag. U kunt de planner uitvoeren in de snelle modus voor schattingen op basis van een gemiddeld aantal VMs, schijven en opslag, of in de gedetailleerde modus waarin figuren op het niveau van de belasting moet worden ingevoerd. Voordat u begint moet u:

- Informatie verzamelen over uw replicatieomgeving, met inbegrip van VMs, schijven per VMs en opslag per schijf.
- De snelheid van de dagelijkse wijzigen (lospeuteren) u voor delta gerepliceerde gegevens hebt te schatten. U kunt de [planner voor Hyper-V-Replica capaciteit](https://www.microsoft.com/download/details.aspx?id=39057) doet.

1.  Klik op **downloaden** om het hulpprogramma te downloaden en uitvoeren. [Lees het artikel](site-recovery-capacity-planner.md) die bij het hulpprogramma hoort.
2.  Wanneer u klaar bent in **hebt uitgevoerd de Planner capaciteit** **Ja** selecteren?

    ![Capaciteitsplanning](./media/site-recovery-vmm-to-azure/gs-capacity-planning.png)

### <a name="control-bandwidth"></a>Besturingselement bandbreedte

Als u replicatiegegevens real-time delta van de capaciteit Planner Hyper-V-Replica hebt verzameld, helpt de Capaciteitsplanner voor op basis van Excel te berekenen van de bandbreedte die u nodig hebt voor replicatie (initiële en delta). U kunt het NetQos beleid met behulp van Groepsbeleid of de Windows PowerShell configureren om te bepalen van de hoeveelheid bandbreedte die wordt gebruikt voor replicatie. Er zijn enkele manieren waarop u dit kunt doen:

**PowerShell** | **Details**
--- | ---
**Nieuw - NetQosPolicy "Met bestemming subnet QoS"** | Verkeer van een Hyper-V-host met Windows Server 2012 R2 naar een secundaire subnet beperken. Gebruik deze optie als uw primaire en secundaire subnetten verschillend zijn.
**Nieuw - NetQosPolicy "Met bestemmingspoort QoS"** | Verkeer van een Hyper-V-host met Windows Server 2012 R2 naar een doelpoort beperken.
**Nieuw - NetQosPolicy "Gashendel verkeer van VMM"** | Verkeer van de vmms.exe beperken. Hiermee wordt verkeer in Hyper-V en Live migratie verkeer beperken. U kunt IP-protocollen en poorten om nader te vergelijken.

U kunt bandbreedte gewicht instellingen gebruiken of verkeer beperken door bits per secundaire. Als u een cluster, moet u dit doen op alle clusterknooppunt. Zie voor meer informatie:


- Blog van Thomas Maurer op [Hyper-V Replica verkeer beperken](http://www.thomasmaurer.ch/2013/12/throttling-hyper-v-replica-traffic/)
- Meer informatie over de [cmdlet New-NetQosPolicy](https://technet.microsoft.com/library/hh967468.aspx).


## <a name="step-6-enable-replication"></a>Stap 6: Enable replicatie

Nu de replicatie als volgt inschakelen:

1. Klik op **stap 2: toepassing repliceren** > **bron**. Nadat u replicatie voor het eerst hebt ingeschakeld, klik u op **+ repliceren** in de kluis replicatie voor extra computers inschakelen.

    ![Replicatie inschakelen](./media/site-recovery-vmm-to-vmm/enable-replication1.png)

2. In de **bron** -blade > Selecteer VMM-server en de wolk waarin de Hyper-V hosts die u wilt repliceren bevinden. Klik vervolgens op **OK**.

    ![Replicatie inschakelen](./media/site-recovery-vmm-to-vmm/enable-replication2.png)

3. Controleer of de secundaire server voor VMM en cloud in de **doel** -blade.
4. Selecteer het VMs u wilt voorkomen dat de lijst in **virtuele machines** .

    ![Virtuele machine beveiliging inschakelen](./media/site-recovery-vmm-to-vmm/enable-replication5.png)

U kunt de voortgang van de actie **Beveiliging inschakelen** in de instellingen volgen > **taken** > **Site herstel taken**. Nadat de taak **Voltooien bescherming** wordt uitgevoerd is de virtuele machine gereed voor failover.


>[AZURE.NOTE] U kunt ook beveiliging voor virtuele machines in de VMM console inschakelen. Klik op **Beveiliging inschakelen** op de werkbalk in de eigenschappen van de virtuele machine > tabblad **Azure Site herstel** .

Nadat u replicatie hebt ingeschakeld, kunt u eigenschappen bekijken voor VM in **Instellingen** > **Gerepliceerd artikelen** > vm-naam. Op het dashboard **Essentials** ziet u informatie over het replicatiebeleid voor en de status van de VM. Klik op **Eigenschappen** voor meer informatie.

### <a name="onboard-existing-virtual-machines"></a>On-board bestaande virtuele machines

Als u bestaande virtuele machines in de VMM die met Hyper-V-Replica repliceert kunt u boord hen voor Azure Site bescherming als volgt:

1. Zorg ervoor dat de Hyper-V server die als host fungeert voor de bestaande VM bevindt zich in de primaire cloud en dat de Hyper-V server die als host fungeert voor de virtuele machine van replica bevindt zich in de secundaire wolk.
2. Zorg ervoor dat een replicatiebeleid is geconfigureerd voor de primaire VMM-wolk.
2. Schakel replicatie voor de primaire virtuele machine. Azure Site herstellen en VMM zal ervoor zorgen dat de host van dezelfde replica en de virtuele machine wordt gedetecteerd en Azure Site herstel zal gebruiken en herstellen met behulp van de opgegeven instellingen.


## <a name="step-7-test-your-deployment"></a>Stap 7: De implementatie testen

U kunt de implementatie testen uitvoeren van failover voor een enkele virtuele machine testen of een herstelplan met een of meer virtuele machines maken.

### <a name="prepare-for-failover"></a>Voorbereiden voor failover

- Om uw installatie volledig te testen, moet u een infrastructuur voor de gerepliceerde machine werken zoals verwacht. U kunt een virtuele machine maken als een domeincontroller met DNS en deze repliceren naar Azure met Azure Site herstellen als u wilt testen van Active Directory en DNS. Lees meer in de [overwegingen van failover-test voor Active Directory](site-recovery-active-directory.md#considerations-for-test-failover).
- De instructies in dit artikel wordt beschreven hoe u een failover test uitgevoerd met geen netwerk. Deze optie wordt de VM wordt overgenomen, maar won't netwerkinstellingen voor de VM test test. [Meer informatie](site-recovery-failover.md#run-a-test-failover) over andere opties.
- Let op het volgende als u een niet-geplande failover in plaats van een test failover uitvoeren:

    - Indien mogelijk moet u afgesloten primaire machines voordat u een niet-geplande failover uitvoeren. Dit zorgt ervoor dat u hoeft niet de bron- en replica-computers op hetzelfde moment.
    - Wanneer u een niet-geplande failover uitgevoerd stopt gegevensreplicatie uit primaire machines zodat alle gegevens delta won't worden overgedragen wanneer een niet-geplande failover. Ook als u een niet-geplande failover op een herstelplan uitvoert wordt deze uitgevoerd totdat volledige, zelfs als er een fout optreedt.




### <a name="run-a-test-failover"></a>Een failover test uitvoeren

1. Failover van een enkele VM in **Instellingen** > **Items gerepliceerd**, klikt u op de VM > **+ failover-Test**.

    ![Test-failover](./media/site-recovery-vmm-to-vmm/test-failover.png)

2. Failover van een herstelplan in **Instellingen** > **Recovery plan**, met de rechtermuisknop op het plan > **Test Failover**. U maakt een herstelplan [voert u deze instructies](site-recovery-create-recovery-plans.md).
2. In de **Test Failover**, selecteert u **geen**. Met deze optie test u de VM failover wordt uitgevoerd zoals verwacht, maar de gerepliceerde VM niet wordt aangesloten op een netwerk.

    ![Test-failover](./media/site-recovery-vmm-to-vmm/test-failover1.png)

3. Klik op **OK** om te beginnen met de overname. U kunt de voortgang bijhouden door te klikken op de VM de eigenschappen ervan te openen of op de taak **Testen Failover** in **Instellingen** > **taken** > **Site herstel taken**.
4. Als de failover-functie is de **Complete test** fase bereikt, het volgende doen:

    -  De replica VM weergeven in de secundaire VMM-wolk.
    -  Klik op **de test voltooid** op Voltooien om de overname van de test.
    -  Klik op **notities** als u wilt opnemen en opslaan van eventuele opmerkingen die zijn gekoppeld aan de overname van de test.

5. De test virtuele machine wordt gemaakt op dezelfde host als de host waarop de replica virtuele machine bestaat. Deze is toegevoegd aan de dezelfde wolk waarin de replica virtuele machine bevindt.
6. Nadat u hebt gecontroleerd dat VMs start is klikt u op **de test failover is uitgevoerd**. In dit stadium worden geen elementen automatisch gemaakt door sites worden hersteld tijdens de test failover verwijderd.  

    > [AZURE.NOTE] Als een failover test langer dan twee weken wordt het uitvoeren van kracht blijft.



### <a name="update-dns-with-the-replica-vm-ip-address"></a>DNS bijwerken met de replica VM IP-adres

Na een failover de VM-replica mogelijk niet hetzelfde IP-adres als de primaire virtuele machine.

- Virtuele machines werkt u de DNS-server die ze gebruiken wanneer ze worden gestart.
- U kunt ook handmatig DNS als volgt bijwerken:

#### <a name="retrieve-the-ip-address"></a>Het IP-adres ophalen

Dit voorbeeldscript voor het ophalen van het IP-adres wordt uitgevoerd.

        $vm = Get-SCVirtualMachine -Name <VM_NAME>
        $na = $vm[0].VirtualNetworkAdapters>
        $ip = Get-SCIPAddress -GrantToObjectID $na[0].id
        $ip.address  

#### <a name="update-dns"></a>DNS bijwerken

Dit voorbeeldscript wilt bijwerken, DNS, opgeven van het IP-adres dat u opgehaald met behulp van het voorbeeldscript uitvoeren.

        string]$Zone,
        [string]$name,
        [string]$IP
        )
        $Record = Get-DnsServerResourceRecord -ZoneName $zone -Name $name
        $newrecord = $record.clone()
        $newrecord.RecordData[0].IPv4Address  =  $IP
        Set-DnsServerResourceRecord -zonename $zone -OldInputObject $record -NewInputObject $Newrecord

## <a name="next-steps"></a>Volgende stappen

Na de implementatie is ingesteld en uitgevoerd, [meer informatie](site-recovery-failover.md) over verschillende typen van failover.
