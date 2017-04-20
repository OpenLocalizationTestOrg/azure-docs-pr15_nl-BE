<properties
    pageTitle="Hyper-V virtuele machines (zonder VMM) repliceren naar Azure met Azure Site herstellen met de Azure portal | Microsoft Azure"
    description="Hierin wordt beschreven hoe u Azure Site herstellen om replicatie, failover en herstel van voor bedrijfsruimten Hyper-V VMs die niet worden beheerd door de VMM om met de portal Azure Azure evoluerende implementeren"
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
    ms.workload="storage-backup-recovery"
    ms.date="09/19/2016"
    ms.author="raynew"/>


# <a name="replicate-hyper-v-virtual-machines-without-vmm-to-azure-using-azure-site-recovery-with-the-azure-portal"></a>Hyper-V virtuele machines (zonder VMM) repliceren naar Azure met Azure Site herstellen met de Azure portal

> [AZURE.SELECTOR]
- [Azure Portal](site-recovery-hyper-v-site-to-azure.md)
- [Azure klassiek](site-recovery-hyper-v-site-to-azure-classic.md)
- [PowerShell - Resource Manager](site-recovery-deploy-with-powershell-resource-manager.md)



Welkom bij herstel Azure Site! Gebruik dit artikel als u wilt repliceren op ruimten Hyper-V virtuele machines die **niet worden** beheerd in wolken op Azure System Center Virtual Machines Manager (VMM). In dit artikel wordt beschreven hoe een replicatie met Azure Site herstellen in de portal Azure.

> [AZURE.NOTE] Azure heeft twee verschillende [implementatiemodellen](../resource-manager-deployment-model.md) voor het maken en werken met resources: Azure Resource Manager en klassiek. Azure heeft ook twee portals – de Azure klassieke portal die het implementatiemodel klassiek ondersteunt en de Azure portal met ondersteuning voor beide implementatiemodellen.

> Azure-Site-Recovery ondersteunt het herstel en Hyper-V virtuele machines met Azure. De stappen in dit artikel gelden dezelfde tijdens het configureren van replicatie Azure voor herstel of voor het migreren van VMs naar Azure

Azure-Site-Recovery in Azure portal biedt een aantal nieuwe functies:

- In de Azure worden portal, Azure back-up en herstel van Azure Site services gecombineerd tot een enkel Recovery Services kluis zodat u kunt instellen en bedrijfscontinuïteit en noodherstel (BCDR) vanaf één locatie beheren. Een centrale dashboard kunt u controleren en beheren van bewerkingen in uw sites op gebouwen en de openbare Azure cloud.
- Gebruikers met Azure abonnementen met Cloud Solution Provider (CSP) wordt ingericht kunnen nu terugvorderingen Site in Azure portal beheren.
- Herstel van de site in de portal Azure kunt machines repliceren naar de Resource Manager opslag rekeningen. Failover wordt uitgevoerd maakt Site herstellen op basis van een Resource Manager VMs in Azure.
- Recovery site nog steeds ondersteuning voor replicatie naar klassieke opslag rekeningen en failover van VMs met behulp van het implementatiemodel klassiek.


Na het lezen van dit artikel post uw feedback onderaan in de sectie Disqus opmerkingen. Technische vragen op het [Forum van Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="overview"></a>Overzicht


Organisaties hebben behoefte aan een strategie voor BCDR die bepaalt hoe apps, werkbelasting en gegevens blijven actief en beschikbaar tijdens geplande en ongeplande uitvaltijd en normale gebruiksomstandigheden zo snel mogelijk herstellen. Uw strategie voor BCDR u bedrijfsgegevens veilig en terug te vorderen en ervoor zorgen dat werklasten continu beschikbaar zijn wanneer de ramp zich voordoet.

Herstel van de site is een Azure service die deel uitmaakt van uw strategie voor BCDR door replicatie van fysieke servers op gebouwen en virtuele machines naar de cloud (Azure) of een secundaire datacenter slaagde. Wanneer er storingen optreden in de primaire locatie, kunt u failover uitvoeren naar de secundaire locatie apps en werkbelasting beschikbaar te houden. U kunt niet terug naar de primaire locatie als resultaat wordt gegeven voor normale bewerkingen. Meer informatie in [Wat is Azure Site terugzetten?](site-recovery-overview.md)

In dit artikel vindt alle informatie die u wilt repliceren op ruimten Hyper-V virtuele machines die **niet worden** beheerd in wolken op Azure System Center Virtual Machines Manager (VMM). Het bevat een architectonisch overzicht, informatie en implementatiestappen voor het configureren van servers op lokalen, Azure een replicatiebeleid en capaciteitsplanning plannen. Nadat u hebt ingesteld met de infrastructuur kunt u Schakel replicatie op computers die u wilt beveiligen en test voor het valideren van de opzet failover uitvoeren. U kunt ook uw VMs migreert naar Azure eerste geplande failover uitvoeren en de migratie.

## <a name="business-advantages"></a>Voordelen voor uw bedrijf

- Failover van externe (Azure) biedt voor business werkbelasting en toepassingen die worden uitgevoerd in Hyper-V virtuele machines.
- Biedt één console Recovery Services voor eenvoudige installatie en beheer van replicatie, failover en herstel processen.
- Hiermee kunt u eenvoudig failover uitgevoerd van uw infrastructuur op ruimten naar Azure en failback (terugzetten) van Azure naar de site van gebouwen.
- U kunt herstelplannen met meerdere computers zo configureren dat trapsgewijze werkbelasting van toepassingen aan elkaar failover.

## <a name="scenario-architecture"></a>Scenario-architectuur

Dit zijn de onderdelen:

- **Hyper-V host of cluster**: On-premises Hyper-V host-servers of clusters. VMs die u wilt beveiligen met Hyper-V hosts worden in logische Hyper-V-sites verzameld tijdens de implementatie van het herstel van de Site.
- **Azure-siteprovider herstel en herstelagent services**: tijdens de implementatie u installeert de voorziening Azure Site herstellen en de agent Microsoft Azure Recovery Services op host-servers Hyper-V. De Provider communiceert met Azure Site herstellen via HTTPS-443 orchestration repliceren. De agent op de Hyper-V server gegevens worden gerepliceerd naar Azure opslag via HTTPS-443 standaard.
- **Azure**: moet u een abonnement op Azure, een account Azure opslag gegevens opslaan die zijn gerepliceerd en een Azure virtueel netwerk zodat Azure VMs na een failover worden verbonden met een netwerk.

![Hyper-V-site-architectuur](./media/site-recovery-hyper-v-site-to-azure/architecture.png)



## <a name="azure-prerequisites"></a>Azure-vereisten

Hier is wat u nodig hebt in Azure implementeren van dit scenario.

**Vereiste** | **Details**
--- | ---
**Azure-account**| U moet een account van [Microsoft Azure](http://azure.microsoft.com/) . U kunt beginnen met een [gratis proefperiode](https://azure.microsoft.com/pricing/free-trial/). [Meer informatie](https://azure.microsoft.com/pricing/details/site-recovery/) over het herstel van de Site-prijzen.
**Azure opslag** | U hebt een standaard opslag-account nodig. U kunt een LRS of GRS opslag-account gebruiken. Het is raadzaam GRS zodat gegevens veerkrachtig als een regionale is uitgevallen of de primaire regio kan niet worden hersteld. [Meer informatie](../storage/storage-redundancy.md). De account moet zich in hetzelfde gebied, als de kluis Recovery Services.<br/><br/> Premium opslag wordt niet ondersteund.<br/><br/> Gerepliceerde gegevens worden opgeslagen in Azure opslag en Azure VMs worden gemaakt wanneer failover plaatsvindt.<br/><br/> [Meer informatie over](../storage/storage-introduction.md) Azure opslag.
**Azure netwerk** | Je hebt een Azure virtueel netwerk die Azure VMs wordt verbonden wanneer failover plaatsvindt. De Azure virtueel netwerk moet zich in hetzelfde gebied, als de kluis Recovery Services.

## <a name="on-premises-prerequisites"></a>Vereisten voor gebouwen

Hier is wat u nodig hebt op gebouwen.

**Vereiste** | **Details**
--- | ---
**Hyper-V** | Een of meer op-premises servers met **Windows Server 2012 R2** met de meest recente updates en de functie Hyper-V ingeschakeld of **Microsoft Hyper-V Server 2012 R2**.<br/><br/>De Hyper-V server bevatten een of meer virtuele machines.<br/><br/>Hyper-V-servers moeten worden verbonden met het Internet, rechtstreeks of via een proxyserver.<br/><br/>Hyper-V-servers beschikken over correcties vermeld in de [KB2961977](https://support.microsoft.com/en-us/kb/2961977 "KB2961977") geïnstalleerd.
**Provider en agent** | Tijdens de implementatie van Azure Site herstel installeert u de Provider Azure Site herstellen. De installatie van de Provider zal de Azure Services herstelagent ook installeren op elke Hyper-V virtuele machines die u wilt beveiligen. Alle Hyper-V-servers in een kluis Site herstel moet dezelfde versies van de Provider en agent.<br/><br/>De Provider moet via het Internet verbinding maken met Azure Site herstellen. Verkeer kan worden verzonden, rechtstreeks of via een proxy. Houd er rekening mee dat op basis van HTTPS proxy wordt niet ondersteund. De proxy-server moet toegang geven tot: <br/><br/> *. hypervrecoverymanager.windowsazure.com <br/> <br/> *. accesscontrol.windows.net <br/><br/> *. backup.windowsazure.com <br/> <br/> *. blog.core.windows.net <br/><br/> *Store.Core.Windows.NET <br/><br/> https://www.msftncsi.com/ncsi.txt<br/><br/>Als u IP-adres gebaseerde firewall-regels op de server hebt, Controleer de regels dat communicatie met Azure. U moet de [Azure Datacenter IP-bereiken](https://www.microsoft.com/download/confirmation.aspx?id=41653) en de poort HTTPS (443).<br/><br/>IP-adresbereiken voor de Azure regio van uw abonnement en West VS toestaan.

## <a name="protected-machine-prerequisites"></a>Beveiligde computer vereisten


**Vereiste** | **Details**
--- | ---
**Beveiligde VMs** | Voordat u een VM failover moet u om ervoor te zorgen dat de naam die wordt toegewezen aan de Azure VM [Azure vereisten](site-recovery-best-practices.md#azure-virtual-machine-requirements)voldoet. U kunt de naam wijzigen nadat u replicatie voor het VM hebt ingeschakeld.<br/><br/> Afzonderlijke schijfcapaciteit op beveiligde computers, hoeft niet meer dan 1023 GB. Een VM kan maximaal 16 schijven hebt (dus 16 TB).<br/><br/> Gedeelde schijf Gast clusters worden niet ondersteund.<br/><br/> Als de bron VM NIC-koppeling wordt deze geconverteerd naar een enkele NIC na een failover naar Azure.<br/><br/>Bescherming van VMs waarop Linux wordt uitgevoerd met een statisch IP-adres wordt niet ondersteund.

## <a name="prepare-for-deployment"></a>Voorbereiden voor distributie

Voorbereiden voor distributie moet je:

1. [Een Azure netwerk instellen](#set-up-an-azure-network) waarin Azure VMs geplaatst worden zal wanneer ze zijn gemaakt na een failover.
2. [Een account Azure opslag instellen](#set-up-an-azure-storage-account) voor gerepliceerde gegevens.
3. [Voorbereiden van de Hyper-V hosts](#prepare-the-hyper-v-hosts) , zodat ze toegang tot de gewenste URL's.

### <a name="set-up-an-azure-network"></a>Een Azure netwerk instellen

Een Azure netwerk ingesteld. Moet u dit zodat het VMs Azure gemaakt nadat failover worden verbonden met een netwerk.

- Het netwerk moet zich in hetzelfde gebied als waar u de kluis Recovery Services gaat implementeren.
- Afhankelijk van de resource-model die u gebruiken wilt voor failover Azure VMs, zult u de Azure netwerk in [Bronbeheer](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) of [klassieke modus](../virtual-network/virtual-networks-create-vnet-classic-pportal.md)ingesteld.
- We raden u een netwerk voordat u begint. Als u dat u niet nodig hebt om het te doen tijdens de implementatie van het herstel van de Site.

> [AZURE.NOTE] [Migratie van netwerken](../resource-group-move-resources.md) via resourcegroepen in het hetzelfde abonnement of via abonnementen wordt niet ondersteund voor netwerken die worden gebruikt voor het herstel van de Site te implementeren.

### <a name="set-up-an-azure-storage-account"></a>Een opslag Azure-account instellen

- U moet een account Azure standaardopslag voor het opslaan van gegevens die zijn gerepliceerd naar Azure.
- Afhankelijk van de resource model die u gebruiken wilt voor Azure VMs failover, stelt u een rekening in [Bronbeheer](../storage/storage-create-storage-account.md) of [klassieke modus](../storage/storage-create-storage-account-classic-portal.md).
- Wij raden aan dat u een opslag account instellen voordat u begint. Als u dat u niet nodig hebt om het te doen tijdens de implementatie van het herstel van de Site. De rekeningen moeten in hetzelfde gebied, als de kluis Recovery Services.

> [AZURE.NOTE] [Migratie van accounts opslag](../resource-group-move-resources.md) via resourcegroepen in het hetzelfde abonnement of via abonnementen wordt niet ondersteund voor opslag gebruikt voor het herstel van de Site te implementeren.

### <a name="prepare-the-hyper-v-hosts"></a>Voorbereiden van de Hyper-V hosts

- Zorg ervoor dat de Hyper-V hosts voldoen aan de [vereisten](#on-premises-prerequisites).

### <a name="create-a-recovery-services-vault"></a>Maak een kluis Recovery Services

1. Log in om de [Azure portal](https://portal.azure.com).
2. Klik op **nieuwe** > **Management** > **back-up en herstel van de Site (OMS)**. U kunt ook op **Bladeren klikken** > kluizen**Recovery Services** > **toevoegen**.

    ![Nieuwe kluis](./media/site-recovery-hyper-v-site-to-azure/new-vault3.png)

3. Geef in het vak **naam** een beschrijvende naam ter identificatie van de kluis. Als u meer dan één abonnement hebt, selecteert u een van deze.
4. [Een nieuwe resourcegroep maken](../resource-group-template-deploy-portal.md) of Selecteer een bestaande en een Azure regio opgeven. Machines worden gerepliceerd naar deze regio. Ondersteunde regio's Zie geografische beschikbaarheid in [Azure Site herstel prijzen Details](https://azure.microsoft.com/pricing/details/site-recovery/)
4. Als u wilt snel toegang tot de kluis van het Dashboard op **aan het dashboard vastmaken** en klik vervolgens op **Create kluis**.

    ![Nieuwe kluis](./media/site-recovery-hyper-v-site-to-azure/new-vault-settings.png)

De nieuwe kluis wordt weergegeven op het **Dashboard** > **alle bronnen**, en in het hoofdmenu **Recovery Services kluizen** blade.

## <a name="getting-started"></a>Aan de slag

Herstel van de site biedt een introductie ervaring waarmee u zo snel mogelijk implementeren. Aan de slag, controleert de vereisten en begeleidt u bij de Site herstel implementatie in de juiste volgorde stappen.

Introductie selecteert u het type waar u wilt repliceren naar en van computers die u wilt repliceren. U instellen servers op lokalen, opslag Azure accounts en netwerken. U maakt beleid voor replicatie en capaciteitsplanning. Nadat u hebt ingesteld om uw infrastructuur schakelt u replicatie voor VMs. U kunt uitvoeren van failover voor specifieke computers of herstelplannen failover op meerdere computers te maken.

Beginnen aan de slag door te kiezen hoe u wilt implementeren Site herstellen. De stroom aan de slag verandert enigszins afhankelijk van uw vereisten voor replicatie.



## <a name="step-1-choose-your-protection-goals"></a>Stap 1: Kies uw doelstellingen bescherming

Selecteer wat u wilt repliceren en waar u wilt repliceren naar.

1. Selecteer uw kluis in de **Recovery Services kluizen** blade en klikt u op **Instellingen**.
2. In de **Instellingen** > **Aan de slag** Klik op **Herstel van de Site** > **stap 1: voorbereiden infrastructuur** > **doelstelling van bescherming**.

    ![Doelstellingen kiezen](./media/site-recovery-hyper-v-site-to-azure/choose-goals.png)

3. In de **doelstelling van bescherming** **Azure te**selecteren en selecteer **Ja, met Hyper-V**. Selecteer **Nee** om te bevestigen dat u VMM niet gebruikt. Klik vervolgens op **OK**.

    ![Doelstellingen kiezen](./media/site-recovery-hyper-v-site-to-azure/choose-goals2.png)


## <a name="step-2-set-up-the-source-environment"></a>Stap 2: De bronomgeving instellen

De Hyper-V-site instelt, de Provider Azure Site herstellen en de agent Azure Recovery Services op Hyper-V hosts installeren en registreren van de hosts in de kluis.


1. Klik op **stap 2: voorbereiden van infrastructuur** > **bron**. Als u wilt een nieuwe Hyper-V-site als een container voor de Hyper-V hosts of clusters toevoegen, klikt u op **+ Hyper-V-Site**.

    ![Bron instellen](./media/site-recovery-hyper-v-site-to-azure/set-source1.png)

2. Geef een naam voor de site in de blade **-site maken van Hyper-V** . Klik vervolgens op **OK**. Selecteer de site die u zojuist hebt gemaakt.

    ![Bron instellen](./media/site-recovery-hyper-v-site-to-azure/set-source2.png)

3. Klik op **+ Hyper-V Server** als een server toevoegen aan de site.
4. In **Server toevoegen** > **type Server** controleren dat **Hyper-V server** wordt weergegeven. Zorg ervoor dat de Hyper-V server die u wilt toevoegen aan de [vereisten](#on-premises-prerequisites) voldoet en toegang kunnen krijgen tot de opgegeven URL's.
4. Download het installatiebestand Azure Site Recovery-Provider. U kunt dit bestand de Provider en de Recovery Services agent op elke host Hyper-V installeren uitvoeren.
5. Download de sleutel van de registratie. Dit hebt u nodig wanneer u setup uitvoert. De code is geldig tot 5 dagen nadat u het hebt gegenereerd.

    ![Bron instellen](./media/site-recovery-hyper-v-site-to-azure/set-source3.png)

6. De Provider setup op elke host die u hebt toegevoegd aan de site van Hyper-V-bestand uitgevoerd. Als u op een Hyper-V cluster installeert, moet u setup uitvoeren op elk clusterknooppunt. Installeren en registreren van elke Hyper-V-clusterknooppunten zorgt ervoor dat virtuele machines beveiligd blijven, zelfs als ze op knooppunten migreren.

### <a name="install-the-provider-and-agent"></a>De Provider en agent installeren

1. De Provider setup-bestand uitgevoerd.
2. In **Microsoft Update** kunt u opt-in voor updates zodat Provider updates zijn geïnstalleerd volgens het beleid van Microsoft Update.
3. In de **installatie** accepteren of wijzigen van de standaardlocatie van de Provider en klik op **installeren**.
4. Klik op **Bladeren** om te selecteren van de kluis bestand dat u hebt gedownload in de pagina **Instellingen van de kluis** . Geef het abonnement Azure Site herstellen, de naam van de kluis en de Hyper-V-site die de Hyper-V server behoort.

    ![Server-registratie](./media/site-recovery-hyper-v-site-to-azure/provider3.png)

5. in de **Proxy-instellingen** opgeven hoe de Provider die wordt geïnstalleerd op de server maakt verbinding met Azure Site herstellen via het internet.

- Selecteer **Connect rechtstreeks zonder proxy**als u wilt dat de Provider om rechtstreeks verbinding te maken.
- Selecteer **verbinding maken met bestaande proxy-instellingen**als u wilt verbinden met de proxy die momenteel is ingesteld op de server.
- Als uw bestaande proxy verificatie vereist is, of u wilt een aangepaste proxy gebruiken voor de Provider verbinding selecteren die **verbinding maken met aangepaste proxy-instellingen**.
- Als u een aangepaste proxy gebruiken moet u het adres, poort en referenties
- Als u een proxy-Zorg ervoor dat de URL's die worden beschreven in de [voorwaarden](#on-premises-prerequisites) via het toegestaan.

    ![Internet](./media/site-recovery-hyper-v-site-to-azure/provider7.PNG)

6. Klik op **registreren** als u de server registreren in de kluis nadat de installatie is voltooid.

![Installatielocatie](./media/site-recovery-hyper-v-site-to-azure/provider2.png)

7. na registratie is voltooid de metagegevens van de Hyper-V server is opgehaald door Azure Site herstellen en de server wordt weergegeven op de **Instellingen voor** > **Herstel infrastructuur** > blade**Hyper-V Hosts** .


### <a name="command-line-installation"></a>Installatie via opdrachtregel

Azure-siteprovider herstel en agent kunnen ook worden geïnstalleerd met de volgende opdrachtregel. Deze methode kan worden gebruikt om de voorziening hebt geïnstalleerd op een Server Core van Windows Server 2012 R2.

1. De Provider installatie en registratie sleutel downloaden naar een map. Bijvoorbeeld C:\ASR.
2. Vanaf een opdrachtprompt uitvoeren van deze opdrachten om uit te pakken van de installateur van de Provider:

            C:\Windows\System32> CD C:\ASR
            C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q
3. Uitvoeren van deze opdracht de componenten te installeren:

            C:\ASR> setupdr.exe /i

4. Voer vervolgens deze opdrachten naar de server registreren in de kluis: CD C:\Program Files\Microsoft Azure Site herstel Provider\
            C:\Program Files\Microsoft Azure herstel siteprovider\> DRConfigurator.exe/r /Friendlyname <friendly name of the server> /referenties<path of the credentials file>

<br/>
Waar:

- **/Credentials** : verplichte parameter die de locatie waar het sleutelbestand registratie  
- **/FriendlyName** : verplichte parameter voor de naam van de Hyper-V host-server die wordt weergegeven in de portal Azure Site herstellen.
- **/proxyAddress** : optionele parameter die het adres van de proxyserver.
- **/proxyport** : optionele parameter die de poort van de proxyserver.
- **/proxyUsername** : optionele parameter die de naam Proxy (als de proxy verificatie vereist).
- **/proxyPassword** : optionele parameter die het wachtwoord voor verificatie met de proxyserver (als de proxy verificatie vereist).


## <a name="step-3-set-up-the-target-environment"></a>Stap 3: Instellen van de doelomgeving.

Geef de account Azure opslag moet worden gebruikt voor replicatie en het Azure netwerk waarmee Azure VMs verbinding maakt na een failover.

1.  Klik op **infrastructuur voorbereiden** > **doel** en selecteer de Azure abonnement dat u wilt gebruiken.
2.  Geef het implementatiemodel dat u wilt gebruiken voor VMs na een failover.
3.  Herstel van de site controleert dat er een of meer accounts compatibel Azure opslag en netwerken.

    ![Opslag](./media/site-recovery-hyper-v-site-to-azure/select-target.png)

4.  Als u een account voor opslag nog niet hebt gemaakt en u wilt maken met behulp van bronbeheer Klik op account **+ opslag** die inline doen. Geef een naam, type, abonnement en locatie op het blad voor de **opslag-account maken** . De account moet zich in dezelfde locatie als de kluis Recovery Services.

    ![Opslag](./media/site-recovery-hyper-v-site-to-azure/gs-createstorage.png)

    Als u wilt een opslag account maken met het klassieke objectmodel doet u dat [in de portal Azure](../storage/storage-create-storage-account-classic-portal.md).

5.  Als u een Azure netwerk nog niet hebt gemaakt en u wilt maken van een Resource Manager gebruiken Klik op **+ netwerk** die inline doen. Geef een netwerknaam, adresbereik subnet details, abonnement en locatie op de bladeserver **virtueel netwerk maken** . Het netwerk moet zich in dezelfde locatie als de kluis Recovery Services.

    ![Netwerk](./media/site-recovery-hyper-v-site-to-azure/gs-createnetwork.png)

    Als u wilt maken van een netwerk met behulp van het model Klassiek doet u dat [in de portal Azure](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).


## <a name="step-4-set-up-replication-settings"></a>Stap 4: Instellen van de replicatie-instellingen

1. Voor het maken van een nieuwe replicatie beleid **voorbereiden infrastructuur**klikt u op > **De replicatie-instellingen** > **+ maken en koppelen**.

    ![Netwerk](./media/site-recovery-hyper-v-site-to-azure/gs-replication.png)

2. Geef de naam van een beleid **maken** en koppelen beleid.
3. Opgeven hoe vaak gegevens worden gerepliceerd delta na de eerste replicatie (elke 30 seconden, 5 of 15 minuten) in het **kopiëren van de frequentie** .
4. In **herstel punt vasthouden**, opgeven in uren hoe lang het venster inhoudingen worden voor elk herstelpunt. Beveiligde computers kunnen worden hersteld naar een willekeurig punt in een venster.
6. In **App consistent momentopname frequentie** bepaalt hoe vaak (1-12 uur) herstel punten met consistente toepassing momentopnamen zijn gemaakt. Hyper-V worden twee soorten momentopnamen, een momentopname van een standaard waarmee een incrementele momentopname van de hele virtuele machine en een momentopname van een consistente toepassing die een point-in-time momentopname van de toepassingsgegevens in de virtuele machine. Volume Shadow Copy Service (VSS) toepassing consistente momentopnamen gebruiken om ervoor te zorgen dat de toepassingen in een consistente status zijn als de momentopname is gemaakt. Houd er rekening mee dat als u een toepassing consistente momentopnamen inschakelt, geldt dat de prestaties van toepassingen die worden uitgevoerd op de bron virtuele machines. Zorg ervoor dat de ingestelde waarde lager is dan extra herstel punten die u configureert.
3. In de **initiële replicatie begintijd** opgeven wanneer de aanvankelijke replicatie te starten. De replicatie plaatsvindt via de internet-bandbreedte, zodat u wilt plannen buiten de drukke uren. Klik vervolgens op **OK**.

    ![Replicatiebeleid](./media/site-recovery-hyper-v-site-to-azure/gs-replication2.png)

Wanneer u een nieuw beleid maakt is deze automatisch gekoppeld aan de Hyper-V-site. Klik op **OK**. U kunt een Hyper-V-site (en de VMs erin) koppelen aan meerdere beleidsregels voor replicatie in de **Instellingen** > **replicatie** > beleidsnaam > **Hyper-V-Site koppelen**.

## <a name="step-5-capacity-planning"></a>Stap 5: Capaciteitsplanning

Nu dat u uw basic hebt infrastructuur instellen u kunt denken over capaciteitsplanning en uitvinden of u aanvullende bronnen nodig hebt.

Herstel van de site biedt een planner capaciteit zodat u de juiste resources toewijzen voor uw bronomgeving, site herstel onderdelen, netwerk en opslag. U kunt de planner uitvoeren in de snelle modus voor schattingen op basis van een gemiddeld aantal VMs, schijven en opslag, of in de gedetailleerde modus waarin figuren op het niveau van de belasting moet worden ingevoerd. Voordat u begint moet u:

- Informatie verzamelen over uw replicatieomgeving, met inbegrip van VMs, schijven per VMs en opslag per schijf.
- De snelheid van de dagelijkse wijzigen (lospeuteren) u voor gerepliceerde gegevens hebt te schatten. U kunt de [Planner voor Hyper-V-Replica capaciteit](https://www.microsoft.com/download/details.aspx?id=39057) doet.

1.  Klik op **downloaden** om het hulpprogramma te downloaden en uitvoeren. [Lees het artikel](site-recovery-capacity-planner.md) die bij het hulpprogramma hoort.
2.  Wanneer u klaar bent in **hebt uitgevoerd de Planner capaciteit** **Ja** selecteren?

    ![Capaciteitsplanning](./media/site-recovery-hyper-v-site-to-azure/gs-capacity-planning.png)

### <a name="network-bandwidth-considerations"></a>Netwerk bandbreedte overwegingen

U kunt de Capaciteitsplanner voor het berekenen van de bandbreedte die u nodig hebt voor replicatie (de eerste replicatie en vervolgens delta). U hebt een paar opties om te bepalen van de hoeveelheid bandbreedte te gebruiken voor replicatie:

- **De bandbreedte beperken**: Hyper-V-verkeer dat wordt gerepliceerd naar Azure verloopt via een specifieke host in de Hyper-V. U kunt de bandbreedte op de hostserver.
- **Tweak bandbreedte**: kunt u de bandbreedte die wordt gebruikt voor replicatie met behulp van een paar sleutels bepalen.

#### <a name="throttle-bandwidth"></a>De bandbreedte beperken

1. Open de module MMC van Microsoft Azure back-up op de Hyper-V host-server. Een snelkoppeling voor Microsoft Azure back-up is standaard beschikbaar op het bureaublad of in C:\Program Files\Microsoft Azure Recovery Services Agent\bin\wabadmin.
2. Klik op **Eigenschappen wijzigen**in de module.
3. Klik op het tabblad **Throttling** Selecteer **internet bandbreedtegebruik voor back-upbewerkingen bandbreedtebeperking inschakelen**, en de beperkingen voor het werk en niet-werk uren. Geldig bereik zijn van 512 Kbps tot 102 Mbps per seconde.

    ![De bandbreedte beperken](./media/site-recovery-hyper-v-site-to-azure/throttle2.png)

U kunt ook de cmdlet [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409.aspx) voor het instellen van procesbeperking. Hier volgt een voorbeeld:

    $mon = [System.DayOfWeek]::Monday
    $tue = [System.DayOfWeek]::Tuesday
    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth  (512*1024) -NonWorkHourBandwidth (2048*1024)

**Set OBMachineSetting NoThrottle** geeft aan dat er geen beperking vereist is.


#### <a name="influence-network-bandwidth"></a>Invloed op de netwerkbandbreedte

1. Ga naar **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication**in het register.
    - Beïnvloeden van het verkeer van bandbreedte op een schijf vermenigvuldigt de waarde van de **UploadThreadsPerVM**wijzigen of de sleutel maken als deze niet bestaat.
    - Wijzig de waarde **DownloadThreadsPerVM**voor het beïnvloeden van de bandbreedte voor failback verkeer van Azure.
2. De standaardwaarde is 4. In een 'overprovisioned' netwerk moeten deze registersleutels van de standaardwaarden worden gewijzigd. Het maximumaantal is 32. Controleer het netwerkverkeer voor het optimaliseren van de waarde.

## <a name="step-6-enable-replication"></a>Stap 6: Enable replicatie

Nu de replicatie als volgt inschakelen:

1. Klik op **stap 2: toepassing repliceren** > **bron**. Nadat u replicatie voor het eerst hebt ingeschakeld zult u **+ repliceren** in de kluis replicatie voor extra computers inschakelen.

    ![Replicatie inschakelen](./media/site-recovery-hyper-v-site-to-azure/enable-replication.png)

2. In de **bron** -blade > Selecteer de Hyper-V-site. Klik vervolgens op **OK**.
3. Selecteer het abonnement van de kluis en het failover-model dat u wilt gebruiken in Azure (klassiek of resource management) na een failover in **doel** .
4. Selecteer de opslag account die u wilt gebruiken. Als u wilt gebruiken een andere opslag-account dan de hebt u u kunt [maken](#set-up-an-azure-storage-account). Klik op **Nieuw**om een opslag account met behulp van het model Resource Manager. Als u wilt een opslag account maken met het klassieke objectmodel doet u dat [in de portal Azure](../storage/storage-create-storage-account-classic-portal.md). Klik vervolgens op **OK**.
5.  Selecteer de Azure netwerk en subnet die Azure VMs verbinding maakt wanneer u ze hebt gesponnen van na een failover. Selecteer **Configure nu voor de geselecteerde computers** het netwerkinstelling toepassen op alle computers die u selecteert voor de bescherming. Selecteert u **later configureren** selecteren de Azure netwerk per computer. Als u wilt gebruiken een ander netwerk dan hebt u u kunt [maken](#set-up-an-azure-network). Klik op **Nieuw**om een netwerk met behulp van het model Resource Manager. Als u wilt maken van een netwerk met behulp van het model Klassiek doet u dat [in de portal Azure](../virtual-network/virtual-networks-create-vnet-classic-pportal.md). Selecteer een subnet, indien van toepassing. Klik vervolgens op **OK**.

    ![Replicatie inschakelen](./media/site-recovery-hyper-v-site-to-azure/enable-replication11.png)

6. In de **virtuele Machines** > **virtuele machines te selecteren** klikt u op en selecteert u elke computer die u wilt repliceren. U kunt alleen selecteren machines waarvoor replicatie kan worden ingeschakeld. Klik vervolgens op **OK**.

    ![Replicatie inschakelen](./media/site-recovery-hyper-v-site-to-azure/enable-replication5.png)

11. In het dialoogvenster **Eigenschappen** > **eigenschappen configureren**, selecteer het besturingssysteem voor de geselecteerde VMs en de OS-schijf. Controleer dat de Azure VM naam (doel) aan [eisen voor Azure VM voldoet](site-recovery-best-practices.md#azure-virtual-machine-requirements) en als u wilt wijzigen. Klik vervolgens op **OK**. U kunt later extra eigenschappen instellen.

    ![Replicatie inschakelen](./media/site-recovery-hyper-v-site-to-azure/enable-replication6.png)

12. In **de replicatie-instellingen** > -**replicatie-instellingen configureren**, selecteert u wilt toepassen voor de beveiligde VMs replicatiebeleid. Klik vervolgens op **OK**. Het replicatiebeleid in **Instellingen**kunt u > **replicatie beleid** > beleidsnaam > **Instellingen bewerken**. Wijzigingen die u toepast wordt gebruikt voor machines die al zijn gerepliceerd en nieuwe machines.

    ![Replicatie inschakelen](./media/site-recovery-hyper-v-site-to-azure/enable-replication7.png)

U kunt de voortgang van het project **Beveiliging inschakelen** in de **Instellingen**volgen > **taken** > **Site herstel taken**. Nadat de taak **Voltooien bescherming** wordt uitgevoerd de computer is gereed voor failover.

### <a name="view-and-manage-vm-properties"></a>Weergeven en beheren van de eigenschappen van de VM

Wij raden aan dat u de eigenschappen van de broncomputer controleren.

1. Klik op **Instellingen** > **Beveiligde Items** > **Gerepliceerd artikelen** > en selecteer de machine.

    ![Replicatie inschakelen](./media/site-recovery-hyper-v-site-to-azure/test-failover1.png)

2. In het dialoogvenster **Eigenschappen** kunt u replicatie en -failover-informatie voor de VM weergeven.

    ![Replicatie inschakelen](./media/site-recovery-hyper-v-site-to-azure/test-failover2.png)

3. In het **netwerk en de rekenkracht** > **eigenschappen berekenen** kunt u de grootte van de naam en het doel Azure VM. De naam om te voldoen aan de eisen van Azure als u wilt wijzigen. U kunt ook weergeven en wijzigen van informatie over het doelnetwerk, subnetmasker en IP-adres dat wordt toegewezen aan de Azure VM. Let op het volgende:

    - U kunt het doel-IP-adres instellen. Als u een adres, de mislukte niet via de computer opgeeft DHCP gebruikt. Als u een adres dat niet beschikbaar failover wordt uitgevoerd, mislukt de failover. Hetzelfde doel-IP-adres kan worden gebruikt voor failover-test als het adres in het failover-testnetwerk beschikbaar is.
    - Het aantal netwerkadapters wordt bepaald door de grootte die u voor de virtuele machine van doel, als volgt opgeeft:

        - Als het aantal netwerkadapters op de broncomputer kleiner dan of gelijk is aan het aantal adapters die zijn toegestaan voor de grootte van de doel machine is, vervolgens heeft het doel hetzelfde aantal adapters als de bron.
        - Als het aantal adapters voor de virtuele machine van bron het aantal dat is toegestaan overschrijdt voor de doelgrootte en het doel maximaal worden gebruikt.
        - Als bijvoorbeeld een broncomputer twee netwerkadapters heeft en de grootte van target machine ondersteunt vier, moet de doelcomputer twee adapters. Als de broncomputer twee adapters heeft maar de ondersteunde doelgrootte slechts één ondersteunt hebben de doelcomputer slechts één adapter.     
        - Als de VM meerdere netwerkadapters heeft verbinding ze al met hetzelfde netwerk.

    ![Replicatie inschakelen](./media/site-recovery-hyper-v-site-to-azure/test-failover4.png)

5.  In **schijven** kunt u het besturingssysteem en de gegevensschijven bekijken op de VM die wordt gerepliceerd.


## <a name="step-7-test-the-deployment"></a>Stap 7: De implementatie testen

U kunt de implementatie testen kunt u een failover testen voor een enkele virtuele machine of een herstelplan met een of meer virtuele machines uitvoeren.


### <a name="prepare-for-test-failover"></a>Voorbereiden voor failover-test

- Voor het uitvoeren van een test failover die wordt aangeraden dat u een nieuw Azure netwerk die is geïsoleerd uit het Azure productienetwerk (dit is standaard bij het maken van een nieuw netwerk in Azure) maken. [Meer informatie](site-recovery-failover.md#run-a-test-failover) over het uitvoeren van failover van de test.
- Als u de beste prestaties wanneer u failover uitvoeren naar Azure, de Azure-Agent op de beveiligde computer te installeren. Het wordt sneller opgestart en helpt bij het oplossen van problemen. De agent [Linux](https://github.com/Azure/WALinuxAgent) of [Windows](http://go.microsoft.com/fwlink/?LinkID=394789) te installeren.
- Om een volledige test de implementatie moet u een infrastructuur voor de gerepliceerde machine werken zoals verwacht. U kunt een virtuele machine maken als een domeincontroller met DNS en deze repliceren naar Azure met Azure Site herstellen als u wilt testen van Active Directory en DNS. Lees meer in de [overwegingen van failover-test voor Active Directory](site-recovery-active-directory.md#considerations-for-test-failover).
- Let op het volgende als u een niet-geplande failover in plaats van een test failover uitvoeren:

    - Indien mogelijk moet u afgesloten primaire machines voordat u een niet-geplande failover uitvoeren. Dit zorgt ervoor dat u hoeft niet de bron- en replica-computers op hetzelfde moment.
    - Wanneer u een niet-geplande failover uitgevoerd stopt gegevensreplicatie uit primaire machines zodat alle gegevens delta won't worden overgedragen wanneer een niet-geplande failover. Ook als u een niet-geplande failover op een herstelplan uitvoert wordt deze uitgevoerd totdat volledige, zelfs als er een fout optreedt.

### <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Verbinding maken met Azure VMs na een failover voorbereiden

Als u wilt verbinding maken met Azure VMs met RDP na een failover, moet dat u het volgende doen:

**Op de computer op gebouwen voordat failover wordt uitgevoerd**:

- RDP inschakelen voor toegang via het internet, zorg ervoor dat TCP- en UDP-regels worden toegevoegd voor de **openbare**en zodat RDP in **Windows Firewall**wordt toegestaan -> **toegestaan apps en functies** voor alle profielen.
- RDP inschakelen op de computer om toegang te krijgen via een verbinding van site naar site en ervoor te zorgen dat de RDP is toegestaan in **Windows Firewall** -> **toegestaan apps en functies** voor het **domein** en **particuliere** netwerken.
- De [Azure VM-agent](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409) installeren op de computer op gebouwen.
- Zorg ervoor dat het besturingssysteem SAN-beleid is ingesteld op OnlineAll. [Meer informatie]( https://support.microsoft.com/kb/3031135)
- De IPSec-service uitschakelen voordat u de failover-functie uitvoeren.

**Op de Azure VM na een failover**:

- Een openbaar IP-adres toevoegen aan de Netwerkadapter die is gekoppeld aan de Azure VM RDP toestaan.
- Controleer of er geen domeinbeleid om te voorkomen dat u geen verbinding maken met een virtuele machine met behulp van een openbaar adres.
- Probeer verbinding te maken. Controleer of de VM wordt uitgevoerd als u geen verbinding maken. Lees dit [artikel](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx)voor meer tips voor het oplossen van problemen.

Als u toegang wilt tot een Azure VM waarop Linux wordt uitgevoerd na een failover met een Secure Shell client (ssh), het volgende doen:

**Op de computer op gebouwen voordat failover wordt uitgevoerd**:

- Zorg ervoor dat de service Secure Shell op de Azure VM is ingesteld op automatisch starten op het systeem wordt opgestart.
- Controleer de firewall-regels dat een SSH-verbinding met het.

**Op de Azure VM na een failover**:

- De groep beveiligingsregels voor netwerken op de mislukte via VM en de Azure subnet waaraan deze is aangesloten moeten toestaan binnenkomende verbindingen naar de SSH-poort.
- Een eindpunt openbaar moet worden gemaakt zodat binnenkomende verbindingen op de SSH-poort (standaard TCP-poort 22).
- Als de VM toegankelijk is via een VPN-verbinding (Route Express of site naar site VPN) kan de client rechtstreeks verbinding maken met de VM via SSH worden gebruikt.

### <a name="run-a-test-failover"></a>Een failover test uitvoeren

De failover-test doen het volgende uitvoeren:

1. Failover van een enkele VM in **Instellingen** > **Items gerepliceerd**, klikt u op de VM > **+ failover-Test**.

    ![Test-failover](./media/site-recovery-hyper-v-site-to-azure/run-failover1.png)

2. Failover van een herstelplan in **Instellingen** > **Recovery plan**, met de rechtermuisknop op het plan > **Test Failover**. U maakt een herstelplan [voert u deze instructies](site-recovery-create-recovery-plans.md).

3. Selecteer de Azure netwerk waarop Azure VMs verbonden nadat failover plaatsvindt in **Failover-Test** .

    ![Test-failover](./media/site-recovery-hyper-v-site-to-azure/run-failover2.png)

4. Klik op **OK** om te beginnen met de overname. U kunt de voortgang bijhouden door te klikken op de VM te openen van de properies of de taak **Testen Failover** in **Instellingen** > **Site herstel taken**.
5. Als de failover-functie de **Complete test** fase bereikt, het volgende doen:
    1. De replica virtuele machine bekijken in het portal voor Azure. Controleer of de virtuele machine gestart.
    2. Als u ingesteld voor toegang tot virtuele machines vanaf het netwerk op ruimten bent kunt u een verbinding met extern bureaublad op de virtuele machine te starten.
    3. Klik op **de test voltooid** om te voltooien.
    4. Klik op **notities** als u wilt opnemen en opslaan van eventuele opmerkingen die zijn gekoppeld aan de overname van de test.
    5. Klik op **de test failover is uitgevoerd**. Schoonmaken van de testomgeving automatisch uit te schakelen en de test virtuele machine verwijderen.
    6. In dit stadium worden geen elementen of VMs automatisch gemaakt door sites worden hersteld tijdens de test failover verwijderd. Eventuele aanvullende elementen die u hebt gemaakt voor failover-test niet worden verwijderd.

    > [AZURE.NOTE] Als een failover test langer dan twee weken wordt het uitvoeren van kracht blijft.

6. Na voltooiing van de overname ook moet u kunnen zien van de replica Azure machine worden weergegeven in de portal Azure > **virtuele Machines**. U moet ervoor zorgen dat de VM de juiste grootte waarmee het juiste netwerk is verbonden en wordt uitgevoerd.
7. Als u [voorbereid voor na een failover-verbindingen](#prepare-to-connect-to-Azure-VMs-after-failover) u verbinding kunnen moet maken met de Azure VM.


## <a name="failover"></a>Failover

Nadat de eerste replicatie voltooid voor uw machines is kunt u failover kunt aanroepen als nodig is. Herstel van de site ondersteunt diverse soorten failover - failover-Test, geplande failover en niet-geplande failover.
[Meer informatie](site-recovery-failover.md) over de verschillende soorten failover en gedetailleerde beschrijvingen van wanneer en hoe u elk van hen.

> [AZURE.NOTE] Als het uw bedoeling is om te migreren van virtuele machines naar Azure, wordt aangeraden dat u een [geplande failover-bewerking](site-recovery-failover.md#run-a-planned-failover-primary-to-secondary) gebruiken om de virtuele machines migreren naar Azure. Zodra de gemigreerde toepassing in Azure met failover test is gevalideerd, gebruikt u de stappen vermeld onder [Volledige migratie](#Complete-migration-of-your-virtual-machines-to-Azure) voor het voltooien van de migratie van uw virtuele machines. U hoeft niet uit te voeren een Commit- of verwijderen. Volledige migratie de migratie is voltooid, verwijdert u de beveiliging voor de virtuele machine en facturering Azure Site herstellen voor de machine stopt.


###<a name="run-an-unplanned-failover"></a>Een niet-geplande Failover uitvoeren

Dit moet worden gekozen wanneer een primaire site ontoegankelijk zijn na een onverwachte incident, zoals een stroomstoring of virusaanval aanval voor energie. Deze procedure wordt beschreven hoe u een 'niet-geplande failover' voor een herstelplan. U kunt ook voor een enkele virtuele machine failover uitvoeren op het tabblad virtuele Machines. Voordat u begint, zorg ervoor dat alle virtuele machines die u wilt een failover de eerste replicatie is voltooid.

1. Selecteer **Recovery plan > recoveryplan_name**.
2. Klik op het Recovery plan blade, **Niet-geplande Failover**.
3. Kies de bron- en doellocaties op de pagina **niet-geplande Failover** .
4. Selecteer **afsluiten van virtuele machines en synchroniseert u de meest recente gegevens** op te geven dat herstel Site proberen moet de beschermde virtuele machines afsluiten en de gegevens synchroniseren zodat de meest recente versie van de gegevens overgenomen.
5. Na de overname zijn de virtuele machines in een commit in afwachting van de staat.  Klik op **vastleggen** vast te leggen van de failover.

[Meer informatie](site-recovery-failover.md#run-an-unplanned-failover)

## <a name="complete-migration-of-your-virtual-machines-to-azure"></a>Migratie van uw virtuele machines naar Azure


>[AZURE.NOTE] De volgende stappen alleen van toepassing als u virtuele machines naar Azure migreert

1. Geplande failover uitvoeren als vermeld [hier](site-recovery-failover.md)
2. In **Instellingen > gerepliceerd artikelen**, met de rechtermuisknop op de virtuele machine en selecteer **Volledige migratie**

    ![completemigration](./media/site-recovery-hyper-v-site-to-azure/migrate.png)

2. Klik op **OK** om de migratie te voltooien. U kunt de voortgang bijhouden door te klikken op de VM te openen, de eigenschappen ervan of met behulp van de volledige migratietaak in **Instellingen > Site herstel taken**.


## <a name="monitor-your-deployment"></a>De installatie controleren

Hier is het controleren van de configuratie-instellingen, status en gezondheid voor de implementatie van het herstel van de Site:

1. Klik op de naam van de kluis voor toegang tot de dashboard **Essentials** . In dit dashboard kunt u Site-Recovery projecten, replicatiestatus herstelplannen, serverstatus en gebeurtenissen.  U kunt de Essentials tegels en -indelingen die vooral handig voor u zijn, met inbegrip van de status van andere kluizen Site herstel- en back-up weergegeven.

    ![Essentials](./media/site-recovery-hyper-v-site-to-azure/essentials.png)

2. U kunt in de tegel **gezondheid** siteservers ondervonden probleem, en de gebeurtenissen die door de Site herstellen in de afgelopen 24 uur controleren.
3. U kunt beheren en controleren van replicatie in de **Gerepliceerde objecten**, **Recovery plan**, en de **Site herstel taken** naast elkaar. Taken in de **Instellingen voor**detailinformatie -> **taken** -> **Site herstel taken**.
