<properties
    pageTitle="Hyper-V virtuele machines in de wolken VMM repliceren naar Azure met herstel van de Site met de Azure portal | Microsoft Azure"
    description="Hierin wordt beschreven hoe u Azure Site herstellen als u wilt repliceren, failover en herstel van Hyper-V VMs in wolken VMM aan met behulp van de portal Azure Azure evoluerende implementeren"
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
    ms.topic="hero-article"
    ms.date="09/16/2016"
    ms.author="raynew"/>

# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-azure-using-azure-site-recovery-with-the-azure-portal--microsoft-azure"></a>Hyper-V virtuele machines in de wolken VMM repliceren naar Azure met Azure Site herstellen met de Azure portal | Microsoft Azure

> [AZURE.SELECTOR]
- [Azure portal](site-recovery-vmm-to-azure.md)
- [Klassieke Azure](site-recovery-vmm-to-azure-classic.md)
- [PowerShell Resource Manager](site-recovery-vmm-to-azure-powershell-resource-manager.md)
- [Klassieke PowerShell](site-recovery-deploy-with-powershell.md)

Welkom bij herstel Azure Site! Gebruik dit artikel als u wilt repliceren op ruimten Hyper-V virtuele machines beheerd in wolken op Azure Azure Site herstellen in de Azure portal met System Center Virtual Machine Manager (VMM).

> [AZURE.NOTE]Azure heeft twee verschillende [implementatiemodellen](../resource-manager-deployment-model
> ) voor het maken en werken met resources: Azure Resource Manager en klassiek. Azure heeft ook twee portals – de Azure klassieke portal die het implementatiemodel klassiek ondersteunt en de Azure portal met ondersteuning voor beide implementatiemodellen.


Azure-Site herstellen in de portal Azure biedt verschillende nieuwe functies:

- De Azure back-up en herstel van Azure Site services worden gecombineerd tot een enkel Recovery Services-kluis in Azure portal zodat u kunt instellen en bedrijfscontinuïteit en noodherstel (BCDR) vanaf één locatie beheren. Een centrale dashboard kunt u controleren en beheren van bewerkingen in uw sites op gebouwen en de openbare Azure cloud.
- Gebruikers met Azure abonnementen met Cloud Solution Provider (CSP) wordt ingericht kunnen nu terugvorderingen Site in Azure portal beheren.
- Herstel van de site in de portal voor Azure kunt machines repliceren naar Azure Resource Manager opslag rekeningen. Failover wordt uitgevoerd maakt Site herstellen op basis van een Resource Manager VMs in Azure.
- Herstel van de site nog steeds ondersteuning voor replicatie naar klassieke opslag rekeningen. Failover wordt uitgevoerd maakt u Site herstellen met het klassieke objectmodel VMs.


Boeken opmerkingen onderaan in de Disqus opmerkingen na het lezen van dit artikel. Technische vragen op het [Forum van Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="overview"></a>Overzicht

Organisaties hebben behoefte aan een strategie voor BCDR die bepaalt hoe apps, werkbelasting en gegevens blijven actief en beschikbaar tijdens geplande en ongeplande uitvaltijd en normale gebruiksomstandigheden zo snel mogelijk herstellen. Uw strategie voor BCDR moet u bedrijfsgegevens veilig en terug te vorderen en dat werklasten continu beschikbaar blijven als ramp plaatsvindt.

Herstel van de site is een Azure-service die deel uitmaakt van uw strategie voor BCDR door middel van replicatie regie van fysieke servers op gebouwen en virtuele machines naar de cloud (Azure) of naar een secundaire datacenter. Wanneer er storingen optreden in de primaire locatie, failover u uitvoeren naar de secundaire locatie apps en werkbelasting beschikbaar te houden. U niet terug naar de primaire locatie als resultaat wordt gegeven voor normale bewerkingen. Meer informatie in [Wat is Azure Site terugzetten?](site-recovery-overview.md)

Dit artikel bevat alle informatie die u wilt repliceren op-premises Hyper-V VMs in wolken VMM naar Azure. Het bevat een architectonisch overzicht, informatie en implementatiestappen voor het configureren van Azure op gebouwen-servers, replicatie-instellingen en capaciteitsplanning plannen. Nadat u hebt ingesteld met de infrastructuur kunt u replicatie op computers die u wilt beveiligen en controleren dat failover werkt.


## <a name="business-advantages"></a>Voordelen voor uw bedrijf

- Herstel van de site biedt off-site bescherming business werkbelasting en van Hyper-V VMs-toepassingen.
- De portal Recovery Services biedt één locatie wilt instellen, beheren en controleren van replicatie, failover en herstel.
- In uw site op locatie kunt u gemakkelijk failover uit uw infrastructuur op gebouwen Azure en failback (terugzetten) van Azure Hyper-V host-servers uitvoeren.
- U kunt herstelplannen met meerdere computers zo configureren dat trapsgewijze werkbelasting van toepassingen aan elkaar failover.


## <a name="scenario-architecture"></a>Scenario-architectuur


Dit zijn de onderdelen:

- **VMM-server**: een server op locatie VMM met een of meer wolken.
- **Hyper-V host of cluster**: Hyper-V host-servers of clusters beheren in wolken VMM.
- **Azure-siteprovider herstel en herstelagent services**: tijdens de implementatie u de Provider Azure Site herstellen op de VMM-server en de agent Microsoft Azure Recovery Services op de host-servers Hyper-V installeren. De Provider op de VMM-server communiceert met Site herstellen via HTTPS-443 orchestration repliceren. De agent op de Hyper-V server gegevens worden gerepliceerd naar Azure opslag via HTTPS-443 standaard.
- **Azure**: moet u een abonnement op Azure, een account Azure opslag gegevens opslaan die zijn gerepliceerd en een Azure virtueel netwerk zodat Azure VMs na een failover worden verbonden met een netwerk.

![Topologie E2A](./media/site-recovery-vmm-to-azure/architecture.png)


## <a name="azure-prerequisites"></a>Azure-vereisten

Hier is wat u nodig hebt in Azure implementeren van dit scenario.

**Vereiste** | **Details**
--- | ---
**Azure-account**| U moet een account van [Microsoft Azure](http://azure.microsoft.com/) . U kunt beginnen met een [gratis proefperiode](https://azure.microsoft.com/pricing/free-trial/). [Meer informatie](https://azure.microsoft.com/pricing/details/site-recovery/) over het herstel van de Site-prijzen.
**Azure opslag** | U moet een account Azure standaardopslag voor het opslaan van de gerepliceerde gegevens. U kunt een LRS of GRS opslag-account gebruiken. Het is raadzaam GRS zodat gegevens veerkrachtig als een regionale is uitgevallen of de primaire regio kan niet worden hersteld. [Meer informatie](../storage/storage-redundancy.md). De account moet zich in hetzelfde gebied, als de kluis Recovery Services.<br/><br/>Premium opslag wordt niet ondersteund.<br/><br/> Gerepliceerde gegevens worden opgeslagen in Azure opslag en Azure VMs worden gemaakt wanneer failover plaatsvindt. <br/><br/> [Meer informatie over](../storage/storage-introduction.md) Azure opslag.
**Azure netwerk** | U moet een Azure virtueel netwerk die met Azure VMs verbinden wanneer failover plaatsvindt. Het netwerk moet zich in hetzelfde gebied, als de kluis Recovery Services.

## <a name="on-premises-prerequisites"></a>Vereisten voor gebouwen

Hier is wat u nodig hebt voor gebouwen

**Vereiste** | **Details**
--- | ---
**VMM**| Een of meer VMM servers op System Center 2012 R2. Elke server VMM moet een of meer wolken geconfigureerd. Een wolk moet bevatten:<br/><br/> Een of meer VMM hostgroepen.<br/><br/> Hyper-V host-servers of clusters in de hostgroep van elke.<br/><br/>[Meer informatie](http://www.server-log.com/blog/2011/8/26/vmm-2012-and-the-clouds.html) over het instellen van VMM wolken.
**Hyper-V** | Hyper-V host-servers ten minste moeten worden uitgevoerd in **Windows Server 2012 R2** met Hyper-V-rol of **Microsoft Hyper-V Server 2012 R2** en de laatste updates hebt geïnstalleerd.<br/><br/> Hyper-V-server moet een of meer VMs bevatten.<br/><br/> Een Hyper-V host-server of cluster die u wilt repliceren VMs moet worden beheerd in een wolk van VMM.<br/><br/>Hyper-V-servers moeten worden verbonden met het Internet, rechtstreeks of via een proxyserver.<br/><br/>Hyper-V-servers beschikken over correcties vermeld in artikel [2961977](https://support.microsoft.com/kb/2961977) geïnstalleerd.<br/><br/>Hyper-V host-servers moeten toegang tot internet voor gegevensreplicatie naar Azure.
**Provider en agent** | Tijdens de implementatie van Azure Site herstel installeert u de Provider Azure Site terugzetten op de VMM-server en de agent Recovery Services op Hyper-V hosts. De Provider en de agent moeten via het internet rechtstreeks of via een proxy verbinding maken met Azure. Houd er rekening mee dat een op basis van een HTTPS-proxy wordt niet ondersteund. De proxy-server op de VMM server en Hyper-V hosts moet toegang geven tot: <br/><br/> *. hypervrecoverymanager.windowsazure.com <br/> <br/> *. accesscontrol.windows.net <br/><br/> *. backup.windowsazure.com <br/> <br/> *. blob.core.windows.net <br/><br/> *. store.core.windows.net<br/><br/>Als u IP-adres gebaseerde firewall-regels op de server VMM, controleren de regels dat communicatie met Azure. U moet de [Azure Datacenter IP-bereiken](https://www.microsoft.com/download/confirmation.aspx?id=41653) en de poort HTTPS (443).<br/><br/>IP-adresbereiken voor de Azure regio van uw abonnement en West VS toestaan.<br/><br/>Bovendien. de proxy-server op de server VMM moet toegang tot https://www.msftncsi.com/ncsi.txt


## <a name="protected-machine-prerequisites"></a>Beveiligde computer vereisten


**Vereiste** | **Details**
--- | ---
**Beveiligde VMs** | Voordat u een VM failover, ervoor zorgen dat de naam die is toegewezen aan de Azure VM [Azure vereisten](site-recovery-best-practices.md#azure-virtual-machine-requirements)voldoet. U kunt de naam wijzigen nadat u replicatie voor het VM hebt ingeschakeld. <br/><br/> Afzonderlijke schijfcapaciteit op beveiligde computers, hoeft niet meer dan 1023 GB. Een VM kan maximaal 16 schijven hebt (dus 16 TB).<br/><br/> Gedeelde schijf Gast clusters worden niet ondersteund.<br/><br/> Unified Extensible Firmware Interface (UEFI) / opstarten met Extensible Firmware Interface(EFI) wordt niet ondersteund.<br/><br/> Als de bron VM NIC-koppeling wordt deze geconverteerd naar een enkele NIC na een failover naar Azure.<br/><br/>Bescherming van VMs waarop Linux wordt uitgevoerd met een statisch IP-adres wordt niet ondersteund.

## <a name="prepare-for-deployment"></a>Voorbereiden voor distributie

Voorbereiden voor distributie, u moet:

1. [Een Azure netwerk instellen](#set-up-an-azure-network) waarin Azure VMs opgeslagen na de failover worden.
2. [Een account Azure opslag instellen](#set-up-an-azure-storage-account) voor gerepliceerde gegevens.
4. [De server VMM voorbereiden](#prepare-the-vmm-server) voor herstel van de Site-implementatie.
5. [Voorbereiden voor het netwerk toewijzen](#prepare-for-network-mapping). Netwerken instellen zodat u kunt netwerk toewijzing tijdens de implementatie van de Recovery-Site configureren.

### <a name="set-up-an-azure-network"></a>Een Azure netwerk instellen

U moet een Azure netwerk zodat de VMs Azure nadat failover wordt verbinding gemaakt.

- Het netwerk moet in hetzelfde gebied als waar u de kluis Recovery Services implementeren.
- Afhankelijk van de resource-model die u gebruiken wilt voor failover Azure VMs, zult u de Azure netwerk in [Bronbeheer](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) of [klassieke modus](../virtual-network/virtual-networks-create-vnet-classic-pportal.md)ingesteld.
- We raden u een netwerk voordat u begint. Als u dit niet doet, moet u tijdens het herstel van de Site-implementatie.

> [AZURE.NOTE] [Migratie van netwerken](../resource-group-move-resources.md) via resourcegroepen in het hetzelfde abonnement of via abonnementen wordt niet ondersteund voor netwerken die worden gebruikt voor het herstel van de Site te implementeren.


### <a name="set-up-an-azure-storage-account"></a>Een opslag Azure-account instellen

- U moet een account Azure standaardopslag voor het opslaan van gegevens die zijn gerepliceerd naar Azure. De account moet zich in hetzelfde gebied, als de kluis Recovery Services.
- Afhankelijk van de resource-model die u gebruiken wilt voor failover Azure VMs, moet u een account in [Bronbeheer](../storage/storage-create-storage-account.md) of [klassieke modus](../storage/storage-create-storage-account-classic-portal.md)instellen.
- Wij raden aan dat u een account instellen voordat u begint. Als u dit niet doet, moet u tijdens het herstel van de Site-implementatie.

> [AZURE.NOTE] [Migratie van accounts opslag](../resource-group-move-resources.md) via resourcegroepen in het hetzelfde abonnement of via abonnementen wordt niet ondersteund voor opslag gebruikt voor het herstel van de Site te implementeren.

### <a name="prepare-the-vmm-server"></a>Voorbereiding van de VMM-server

- Zorg ervoor dat de VMM-server aan de [vereisten voldoet](#on-premises-prerequisites).
- Tijdens de implementatie van herstel van de Site, kunt u opgeven dat alle wolken op een server VMM beschikbaar in de portal Azure zijn moeten. Als u alleen specifieke wolken in de portal wordt weergegeven, kunt u deze instelling van de wolk in de beheerconsole van VMM.


### <a name="prepare-for-network-mapping"></a>Voorbereiden voor het netwerk toewijzen

U moet network toewijzing tijdens de implementatie van het herstel van de Site instellen. Netwerk-toewijzing wordt toegewezen tussen bron VMM VM netwerken en Azure netwerken, zodat het volgende doel:

- Machines die op hetzelfde netwerk failover kunnen verbinden met elkaar, zelfs als niet overgenomen in hetzelfde of in hetzelfde herstelplan heeft nog.
- Als een netwerkgateway is ingesteld op het doel Azure netwerk, Azure virtuele machines verbinding maken voor bedrijven virtuele machines.
- Als u een netwerk wilt instellen is toewijzing hier wat u nodig hebt voor te bereiden:

    - Zorg ervoor dat VMs op de bronserver voor Hyper-V host met een netwerk van VMM VM verbonden bent. Dit netwerk moet worden gekoppeld aan een logisch netwerk dat is gekoppeld aan de cloud.
    - Een Azure netwerk zoals beschreven [hierboven](#set-up-an-azure-network)

- [Meer informatie](site-recovery-network-mapping.md) over de werking van de toewijzing van het netwerk.


## <a name="create-a-recovery-services-vault"></a>Maak een kluis Recovery Services

1. Log in om de [Azure portal](https://portal.azure.com).
2. Klik op **nieuwe** > **Management** > **Recovery Services**. U kunt ook op **Bladeren klikken** > kluizen**Recovery Services** > **toevoegen**.

    ![Nieuwe kluis](./media/site-recovery-vmm-to-azure/new-vault3.png)

3. Geef in het vak **naam**een beschrijvende naam ter identificatie van de kluis. Als u meer dan één abonnement hebt, selecteert u een van deze.
4. [Een resourcegroep maken](../resource-group-template-deploy-portal.md), of Selecteer een bestaande. Geef een regio Azure. Machines worden gerepliceerd naar deze regio. Ondersteunde regio's Zie geografische beschikbaarheid in [Azure Site herstel prijzen Details](https://azure.microsoft.com/pricing/details/site-recovery/)
4. Als u snel toegang tot de kluis van het Dashboard wilt, klik op **vastmaken aan dashboard** > **kluis maken**.

    ![Nieuwe kluis](./media/site-recovery-vmm-to-azure/new-vault-settings.png)

De nieuwe kluis wordt weergegeven op het **Dashboard** > **alle bronnen**, en in het hoofdmenu **Recovery Services kluizen** blade.

## <a name="getting-started"></a>Aan de slag

Herstel van de site biedt een introductie ervaring waarmee u zo snel mogelijk implementeren. Aan de slag, controleert de vereisten en begeleidt u bij de Site herstel implementatie in de juiste volgorde stappen.

Introductie selecteert u het type waar u wilt repliceren naar en van computers die u wilt repliceren. U instellen servers op lokalen, opslag Azure accounts en netwerken. U maakt beleid voor replicatie en capaciteitsplanning. Nadat u hebt ingesteld om uw infrastructuur schakelt u replicatie voor VMs. U kunt uitvoeren van failover voor specifieke computers of herstelplannen failover op meerdere computers te maken.

Beginnen aan de slag door te kiezen hoe u wilt implementeren Site herstellen. De stroom aan de slag verandert enigszins afhankelijk van uw vereisten voor replicatie.



## <a name="step-1-choose-your-protection-goals"></a>Stap 1: Kies uw doelstellingen bescherming

Selecteer wat u wilt repliceren en waar u wilt repliceren naar.

1. Selecteer uw kluis in de blade **Recovery Services kluizen** en klikt u op **Instellingen**.
2. Klik in **Aan de slag** op **Herstel van de Site** > **stap 1: voorbereiden infrastructuur** > **doel bescherming**.

    ![Doelstellingen kiezen](./media/site-recovery-vmm-to-azure/choose-goals.png)

3. In de **doelstelling van bescherming** **Azure te**selecteren en selecteer **Ja, met Hyper-V**. Selecteer **Ja** om te bevestigen u VMM voor het beheer van Hyper-V hosts en de recovery-site. Klik vervolgens op **OK**.

    ![Doelstellingen kiezen](./media/site-recovery-vmm-to-azure/choose-goals2.png)



## <a name="step-2-set-up-the-source-environment"></a>Stap 2: De bronomgeving instellen

De Provider Azure Site terugzetten op de VMM-server installeren en de server registreren in de kluis. De agent Azure Recovery Services installeren op Hyper-V hosts.

1. Klik op **stap 2: voorbereiden van infrastructuur** > **bron**.

    ![Bron instellen](./media/site-recovery-vmm-to-azure/set-source1.png)

2. Klik op **+ VMM** als een VMM-server wilt toevoegen in **de bron voorbereiden** .

    ![Bron instellen](./media/site-recovery-vmm-to-azure/set-source2.png)

3. In de **Server toevoegen** blade controle dat **System Center VMM-server** wordt weergegeven in het **type Server** en die de VMM-server voldoet aan de [voorwaarden en vereisten van de URL](#on-premises-prerequisites).
4. Download het installatiebestand Azure Site Recovery-Provider.
5. Download de sleutel van de registratie. U moet dit wanneer u setup uitvoert. De sleutel is geldig gedurende vijf dagen nadat u het hebt gegenereerd.

    ![Bron instellen](./media/site-recovery-vmm-to-azure/set-source3.png)

6. De Provider Azure Site terugzetten op de VMM-server installeren.


### <a name="set-up-the-azure-site-recovery-provider"></a>Instellen van de Provider Azure Site herstellen

1.  De Provider setup-bestand uitgevoerd.
2. In **Microsoft Update** kunt u opt-in voor updates zodat Provider updates zijn geïnstalleerd volgens het beleid van Microsoft Update.
3. Bij **installatie**accepteren of wijzigen van de standaardlocatie van de Provider en klik op **installeren**.

    ![Installatielocatie](./media/site-recovery-vmm-to-azure/provider2.png)

4. Klik op **registreren** als u wilt de VMM-server registreren in de kluis als installatie is voltooid.
5. Klik op **Bladeren** om de sleutel kluis-bestand te selecteren in de pagina **Instellingen van de kluis** . Geef het abonnement Azure Site herstellen en de naam van de kluis.

    ![Server-registratie](./media/site-recovery-vmm-to-azure/provider10.PNG)

6. In **Internet-verbinding**, opgeven hoe de Provider op de VMM-server maakt verbinding met de Site herstellen via het internet.

    - Selecteer **rechtstreeks verbinding maken met Azure Site herstellen zonder proxy**als u wilt dat de Provider om rechtstreeks verbinding te maken.
    - Als uw bestaande proxy verificatie vereist is, of u wilt gebruiken, selecteert u een aangepaste proxy **verbinding maken met Azure Site herstellen via een proxyserver**.
    - Als u een aangepaste proxy gebruikt, geeft u het adres, de poort en de referenties.
    - Als u een proxy gebruikt hebt u al mag de URL's die worden beschreven in de [voorwaarden](#on-premises-prerequisites).
    - Als u een aangepaste proxy een VMM RunAs-account (DRAProxyAccount) automatisch met de proxyreferenties opgegeven gemaakt. De proxy-server zo configureren dat deze account met succes kan worden geverifieerd. De VMM RunAs account-instellingen kunnen worden gewijzigd in de console van VMM. Vouw in de **Instellingen voor** **beveiliging** > **Uitgevoerd als Accounts**en wijzigt u het wachtwoord voor DRAProxyAccount. U moet de VMM-service opnieuw starten zodat deze instelling van kracht wordt.

    ![Internet](./media/site-recovery-vmm-to-azure/provider13.PNG)

7. Accepteer of wijzig de locatie van een SSL-certificaat dat automatisch wordt gegenereerd voor gegevenscodering. Dit certificaat wordt gebruikt als u gegevenscodering voor een wolk beschermd door Azure in de portal Azure Site herstellen inschakelen. Dit certificaat veilig te houden. Wanneer u een failover naar Azure uitvoeren moet u dit decoderen, als codering is ingeschakeld.


8. Geef in het vak **servernaam**een beschrijvende naam voor de VMM-server in de kluis. Geef in een clusterconfiguratie, de rol van VMM clusternaam.
9. **Sync wolk metagegevens** inschakelen als u wilt dat om metagegevens te synchroniseren voor alle wolken op de VMM-server met de kluis. Deze actie moet slechts één keer gebeuren op elke server. U kunt als u niet dat alle wolken synchroniseren wilt, schakelt u deze instelling niet en elke wolk afzonderlijk in de eigenschappen van de wolk in de VMM-console synchroniseren. Klik op **registreren** om het proces te voltooien.

    ![Server-registratie](./media/site-recovery-vmm-to-azure/provider16.PNG)

10. Registratie wordt gestart. Nadat de registratie is voltooid, de server wordt weergegeven op de **Instellingen voor** > blade**Servers** in de kluis.


#### <a name="command-line-installation-for-the-azure-site-recovery-provider"></a>Installatie via opdrachtregel voor de Provider Azure Site herstellen

De Provider Azure Site herstel kan worden geïnstalleerd vanaf de opdrachtregel. Deze methode kan worden gebruikt om de voorziening hebt geïnstalleerd op een Server Core van Windows Server 2012 R2.

1. De Provider installatie en registratie sleutel downloaden naar een map. Bijvoorbeeld C:\ASR.
2. Vanaf een opdrachtprompt uitvoeren van deze opdrachten om uit te pakken van de installateur van de Provider:

            C:\Windows\System32> CD C:\ASR
            C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q
3. Uitvoeren van deze opdracht de componenten te installeren:

            C:\ASR> setupdr.exe /i

4. Deze opdrachten naar de server registreren in de kluis vervolgens uitvoeren:

        CD C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin
        C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin\> DRConfigurator.exe /r  /Friendlyname <friendly name of the server> /Credentials <path of the credentials file> /EncryptionEnabled <full file name to save the encryption certificate>       

Waar:

- **/Credentials**: verplichte parameter die aangeeft waar de registratie sleutel bestand zich bevindt.  
- **/FriendlyName**: verplichte parameter voor de naam van de Hyper-V host-server die wordt weergegeven in de portal Azure Site herstellen.
- - **/EncryptionEnabled**: optionele parameter wanneer u bij het repliceren van Hyper-V VMs in VMM wolken op Azure. Opgeven als u wilt coderen van virtuele machines in Azure (bij codering met rest). Zorg ervoor dat de naam van het bestand de extensie **.pfx** . Codering is standaard uitgeschakeld.
- **/proxyAddress**: optionele parameter die het adres van de proxyserver.
- **/proxyport**: optionele parameter die de poort van de proxyserver.
- **/proxyUsername**: optionele parameter die de proxynaam (als de proxy verificatie vereist).
- **/proxyPassword**: optionele parameter die het wachtwoord voor verificatie met de proxyserver (als de proxy verificatie vereist is).


### <a name="install-the-azure-recovery-services-agent-on-hyper-v-hosts"></a>De agent Azure Recovery Services installeren op Hyper-V hosts

1. Nadat u de voorziening hebt ingesteld, moet u het installatiebestand voor de agent Azure Recovery Services downloaden. Voer setup uit op elke server Hyper-V in de cloud VMM.

    ![Hyper-V-sites](./media/site-recovery-vmm-to-azure/hyperv-agent1.png)

2. Klik op **volgende**op de pagina **Controle van vereisten** . Eventuele ontbrekende vereisten wordt automatisch geïnstalleerd.

    ![De herstelagent Services voorwaarden](./media/site-recovery-vmm-to-azure/hyperv-agent2.png)

3. Op de pagina **Instellingen voor de installatie** te accepteren of wijzigen van de locatie van de installatie en de locatie van de cache. Kunt u de cache op een station met ten minste 5 GB opslagruimte beschikbaar, maar wij raden aan een station cache met 600 GB of meer vrije ruimte. Klik vervolgens op **installeren**.
4. Nadat de installatie is voltooid, klikt u op **sluiten** om het te voltooien.

    ![MARS Agent registreren](./media/site-recovery-vmm-to-azure/hyperv-agent3.png)

#### <a name="command-line-installation-for-azure-site-recovery-services-agent"></a>Installatie via opdrachtregel voor agent Azure-Site-Recovery Services

De Microsoft Azure herstelagent Services kunt u installeren vanaf de opdrachtregel met de volgende opdracht:

     marsagentinstaller.exe /q /nu

#### <a name="set-up-internet-proxy-access-to-site-recovery-from-hyper-v-hosts"></a>Toegang tot het internet proxy instellen voor herstel van Hyper-V hosts Site

De agent Recovery Services wordt uitgevoerd op Hyper-V hosts moet toegang tot internet voor Azure voor VM-replicatie. Als u toegang het internet via een proxy tot nog, deze instellen als volgt:

1. Open de module MMC van Microsoft Azure back-up op de Hyper-V host. Een snelkoppeling voor Microsoft Azure back-up is standaard beschikbaar op het bureaublad of in C:\Program Files\Microsoft Azure Recovery Services Agent\bin\wabadmin.
2. Klik op **Eigenschappen wijzigen**in de module.
3. Geef op het tabblad **Configuratie van Proxy** proxy serverinformatie.

    ![MARS Agent registreren](./media/site-recovery-vmm-to-azure/mars-proxy.png)

4. Zorg ervoor dat de agent de URL's die worden beschreven in de [voorwaarden](#on-premises-prerequisites)kan bereiken.


## <a name="step-3-set-up-the-target-environment"></a>Stap 3: Instellen van de doelomgeving.

Geef de account Azure opslag moet worden gebruikt voor replicatie en het Azure netwerk waarmee Azure VMs verbinding maakt na een failover.

1.  Klik op **infrastructuur voorbereiden** > **doel** en selecteer de Azure abonnement dat u wilt gebruiken.
2.  Geef het implementatiemodel dat u wilt gebruiken voor VMs na een failover.
3.  Herstel van de site controleert dat er een of meer accounts compatibel Azure opslag en netwerken.

    ![Opslag](./media/site-recovery-vmm-to-azure/compatible-storage.png)

4.  Als u een account voor opslag nog niet hebt gemaakt en u wilt maken met behulp van bronbeheer, klikt u op **+ opslag account** die inline doen.  Geef een naam, type, abonnement en locatie op het blad voor de **opslag-account maken** . De account moet zich in dezelfde locatie als de kluis Recovery Services.

    ![Opslag](./media/site-recovery-vmm-to-azure/gs-createstorage.png)

    Houd rekening met:

    - Als u een opslag account maken met het klassieke model wilt, doet u dat in de Azure portal. [Meer informatie](../storage/storage-create-storage-account-classic-portal.md)
    - Als u een account premie opslag voor gerepliceerde gegevens gebruikt, moet u een extra opslagruimte voor standaard-account instellen voor het opslaan van Logboeken van bestandsreplicatie die voortdurende wijzigingen in de gegevens voor de lokalen vastleggen.

4.  Als u een Azure netwerk nog niet hebt gemaakt en u wilt maken van een Resource Manager gebruiken Klik op **+ netwerk** die inline doen. Geef een netwerknaam, adresbereik subnet details, abonnement en locatie op de bladeserver **virtueel netwerk maken** . Het netwerk moet zich in dezelfde locatie als de kluis Recovery Services.

    ![Netwerk](./media/site-recovery-vmm-to-azure/gs-createnetwork.png)

    Als u wilt maken van een netwerk met behulp van het klassieke model zult u dit doen in de portal Azure. [Meer informatie](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).

### <a name="configure-network-mapping"></a>Toewijzing van netwerk configureren

- [Lees](#prepare-for-network-mapping) een beknopt overzicht van welke toewijzing netwerk heeft. [Lees dit](site-recovery-network-mapping.md) voor een uitgebreidere uitleg.
- Controleer of dat virtuele machines op de VMM-server met een netwerk voor VM verbonden bent en dat u ten minste één Azure virtueel netwerk hebt gemaakt. Meerdere VM-netwerken kunnen worden toegewezen aan één Azure netwerk.

Configureer als volgt toewijzen:

1. In de **Instellingen** > **Herstel infrastructuur** > **toewijzingen netwerk** > **Netwerk toewijzen**, klikt u op het pictogram **+ netwerk toewijzen** .

    ![Netwerk toewijzen](./media/site-recovery-vmm-to-azure/network-mapping1.png)

2. Selecteer de bronserver VMM en **Azure** als doel op de **netwerk-toewijzing toevoegen** .
3. Controleer of u het abonnement en het implementatiemodel na een failover.
4. Selecteer de bron van lokalen VM netwerk die u wilt toewijzen in de lijst die is gekoppeld aan de server VMM in **Bronnetwerk**.
5. Selecteer in het **doelnetwerk**, het Azure netwerk in welke replica Azure VMs geplaatst worden zal wanneer ze zijn gemaakt. Klik vervolgens op **OK**.

    ![Netwerk toewijzen](./media/site-recovery-vmm-to-azure/network-mapping2.png)

Dit is wat er gebeurt als het netwerk toewijzing begint:

- Bestaande VMs in de bron VM netwerk zijn verbonden met het netwerk wanneer de toewijzing begint. Nieuwe VMs verbonden met de bron VM netwerk zijn verbonden met de toegewezen Azure netwerk terwijl de replicatie plaatsvindt.
- Als u een bestaande toewijzing van netwerk wijzigt, worden replica virtuele machines verbonden met de nieuwe instellingen.
- Als het netwerk meerdere subnetten heeft en een van de subnetten heeft dezelfde naam als het subnet waarop de virtuele machine van bron zich bevindt, zal dan de replica virtuele machine worden aangesloten op dat subnet doel na een failover.
- Als er geen subnet doel met een overeenkomende naam, wordt de virtuele machine worden verbonden met het eerste subnet in het netwerk.



## <a name="step-4-set-up-replication-settings"></a>Stap 4: Instellen van de replicatie-instellingen


1. Klik op **infrastructuur voorbereiden**om een nieuw replicatiebeleid voor > **De replicatie-instellingen** > **+ maken en koppelen**.

    ![Netwerk](./media/site-recovery-vmm-to-azure/gs-replication.png)

2. Geef de naam van een beleid **maken en koppelen beleid**.
3. Geef in het **kopiëren van de frequentie**, hoe vaak gegevens worden gerepliceerd delta na de eerste replicatie (elke 30 seconden, 5 of 15 minuten).
4. In **herstel punt vasthouden**, opgeven in uren hoe lang het venster inhoudingen worden voor elk herstelpunt. Beveiligde computers kunnen worden hersteld naar een willekeurig punt in een venster.
6. In **App consistent momentopname frequentie**, opgeven hoe vaak (1-12 uur) herstel punten met consistente toepassing momentopnamen worden gemaakt. Hyper-V worden twee soorten momentopnamen, een momentopname van een standaard waarmee een incrementele momentopname van de hele virtuele machine en een momentopname van een consistente toepassing die een point-in-time momentopname van de toepassingsgegevens in de virtuele machine. Volume Shadow Copy Service (VSS) toepassing consistente momentopnamen gebruiken om ervoor te zorgen dat de toepassingen in een consistente status zijn als de momentopname is gemaakt. Houd er rekening mee dat als u een toepassing consistente momentopnamen inschakelt, geldt dat de prestaties van toepassingen die worden uitgevoerd op de bron virtuele machines. Zorg ervoor dat de ingestelde waarde lager is dan extra herstel punten die u configureert.
3. In de **initiële replicatie starttijd**aangeven wanneer de eerste replicatie start. De replicatie plaatsvindt via de internet-bandbreedte, zodat u wilt plannen buiten de drukke uren.
5. In **coderen gegevens op Azure**, opgeven of voor het coderen van gegevens van de rest in Azure opslag. Klik vervolgens op **OK**.

    ![Replicatiebeleid](./media/site-recovery-vmm-to-azure/gs-replication2.png)

6. Wanneer u een nieuw beleid maakt is deze automatisch gekoppeld aan de VMM wolk. Klik op **OK**. U kunt extra VMM wolken (en de VMs erin) koppelen aan dit replicatiebeleid in de **Instellingen** > **replicatie** > beleidsnaam > **VMM wolk koppelen**.

    ![Replicatiebeleid](./media/site-recovery-vmm-to-azure/policy-associate.png)

## <a name="step-5-capacity-planning"></a>Stap 5: Capaciteitsplanning

Nu dat u uw basic hebt infrastructuur instellen u kunt denken over capaciteitsplanning en uitvinden of u aanvullende bronnen nodig hebt.

Herstel van de site biedt een planner capaciteit zodat u de juiste resources toewijzen voor uw bronomgeving, site herstel onderdelen, netwerk en opslag. U kunt de planner uitvoeren in de snelle modus voor schattingen op basis van een gemiddeld aantal VMs, schijven en opslag, of in de gedetailleerde modus waarin figuren op het niveau van de belasting moet worden ingevoerd. Voordat u begint moet u:

- Informatie verzamelen over uw replicatieomgeving, met inbegrip van VMs, schijven per VMs en opslag per schijf.
- De snelheid van de dagelijkse wijzigen (lospeuteren) u voor gerepliceerde gegevens hebt te schatten. U kunt de [planner voor Hyper-V-Replica capaciteit](https://www.microsoft.com/download/details.aspx?id=39057) doet.

1.  Klik op **downloaden** om het hulpprogramma te downloaden en uitvoeren. [Lees het artikel](site-recovery-capacity-planner.md) die bij het hulpprogramma hoort.
2.  Wanneer u klaar bent in **hebt uitgevoerd de Planner capaciteit** **Ja** selecteren?

    ![Capaciteitsplanning](./media/site-recovery-vmm-to-azure/gs-capacity-planning.png)

### <a name="network-bandwidth-considerations"></a>Netwerk bandbreedte overwegingen

U kunt de Capaciteitsplanner voor het berekenen van de bandbreedte die u nodig hebt voor replicatie (de eerste replicatie en vervolgens delta). U hebt een paar opties om te bepalen van de hoeveelheid bandbreedte te gebruiken voor replicatie:

- **De bandbreedte beperken**: Hyper-V-verkeer dat wordt gerepliceerd naar een secundaire site verloopt via een specifieke host in de Hyper-V. U kunt de bandbreedte op de hostserver.
- **Tweak bandbreedte**: kunt u de bandbreedte die wordt gebruikt voor replicatie met behulp van een paar sleutels bepalen.

#### <a name="throttle-bandwidth"></a>De bandbreedte beperken

1. Open de module MMC van Microsoft Azure back-up op de Hyper-V host-server. Een snelkoppeling voor Microsoft Azure back-up is standaard beschikbaar op het bureaublad of in C:\Program Files\Microsoft Azure Recovery Services Agent\bin\wabadmin.
2. Klik op **Eigenschappen wijzigen**in de module.
3. Klik op het tabblad **Throttling** Selecteer **internet bandbreedtegebruik voor back-upbewerkingen bandbreedtebeperking inschakelen**, en de beperkingen voor het werk en niet-werk uren. Geldig bereik zijn van 512 Kbps tot 102 Mbps per seconde.

    ![De bandbreedte beperken](./media/site-recovery-vmm-to-azure/throttle2.png)

U kunt ook de cmdlet [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409.aspx) voor het instellen van procesbeperking. Hier volgt een voorbeeld:

    $mon = [System.DayOfWeek]::Monday
    $tue = [System.DayOfWeek]::Tuesday
    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth  (512*1024) -NonWorkHourBandwidth (2048*1024)

**Set OBMachineSetting NoThrottle** geeft aan dat er geen beperking vereist is.


#### <a name="influence-network-bandwidth"></a>Invloed op de netwerkbandbreedte

De registerwaarde **UploadThreadsPerVM** bepaalt het aantal threads die worden gebruikt voor de overdracht van gegevens (initiële of delta-replicatie) van een schijf. Een hogere waarde verhoogt de netwerkbandbreedte gebruikt voor replicatie. De registerwaarde **DownloadThreadsPerVM** geeft het aantal threads gebruikt voor de overdracht van gegevens tijdens de failback-bewerking.

1. Ga naar **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication**in het register.

    - Wijzig de waarde **UploadThreadsPerVM** (of de sleutel maken als deze niet bestaat) control threads gebruikt voor replicatie van de schijf.
    - Wijzig de waarde **DownloadThreadsPerVM** (of de sleutel maken als deze niet bestaat) control threads gebruikt voor failback verkeer van Azure.
2. De standaardwaarde is 4. In een 'overprovisioned' netwerk moeten deze registersleutels van de standaardwaarden worden gewijzigd. Het maximumaantal is 32. Controleer het netwerkverkeer voor het optimaliseren van de waarde.

## <a name="step-6-enable-replication"></a>Stap 6: Enable replicatie

Nu de replicatie als volgt inschakelen:

1. Klik op **stap 2: toepassing repliceren** > **bron**. Nadat u replicatie voor het eerst hebt ingeschakeld klik u op **+ repliceren** in de kluis replicatie voor extra computers inschakelen.

    ![Replicatie inschakelen](./media/site-recovery-vmm-to-azure/enable-replication1.png)

2. In de **bron** -blade > Selecteer de VMM-server en de wolk waarin de Hyper-V hosts bevinden. Klik vervolgens op **OK**.

    ![Replicatie inschakelen](./media/site-recovery-vmm-to-azure/enable-replication-source.png)

3. Selecteer het abonnement, post-failover implementeren model en de opslag account die u voor gerepliceerde gegevens gebruikt in **doel** .

    ![Replicatie inschakelen](./media/site-recovery-vmm-to-azure/enable-replication-target.png)

4. Selecteer de opslag account die u wilt gebruiken. Als u wilt gebruiken een andere opslag-account dan de hebt u u kunt [maken](#set-up-an-azure-storage-account). Klik op **Nieuw**om een opslag account met behulp van het model Resource Manager. Als u een opslag account maken met het klassieke model wilt, doet u dat [in de portal Azure](../storage/storage-create-storage-account-classic-portal.md). Klik vervolgens op **OK**.
5. Selecteer de Azure netwerk en subnet die Azure VMs verbinding maakt wanneer u ze hebt gesponnen van na een failover. Selecteer **Configure nu voor de geselecteerde computers** het netwerkinstelling toepassen op alle computers die u selecteert voor de bescherming. Selecteert u **later configureren** selecteren de Azure netwerk per computer. Als u wilt gebruiken een ander netwerk dan hebt u u kunt [maken](#set-up-an-azure-network). Klik op **Nieuw**om een netwerk met behulp van het model Resource Manager. Als u wilt maken van een netwerk met behulp van het model Klassiek doet u dat [in de portal Azure](../virtual-network/virtual-networks-create-vnet-classic-pportal.md). Selecteer een subnet, indien van toepassing. Klik vervolgens op **OK**.
6. In de **virtuele Machines** > **virtuele machines te selecteren** klikt u op en selecteert u elke computer die u wilt repliceren. U kunt alleen selecteren machines waarvoor replicatie kan worden ingeschakeld. Klik vervolgens op **OK**.

    ![Replicatie inschakelen](./media/site-recovery-vmm-to-azure/enable-replication5.png)

5. In het dialoogvenster **Eigenschappen** > **eigenschappen configureren**, selecteer het besturingssysteem voor de geselecteerde VMs en de OS-schijf. Klik vervolgens op **OK**. U kunt later extra eigenschappen instellen.

    ![Replicatie inschakelen](./media/site-recovery-vmm-to-azure/enable-replication6.png)


12. In **de replicatie-instellingen** > -**replicatie-instellingen configureren**, selecteert u wilt toepassen voor de beveiligde VMs replicatiebeleid. Klik vervolgens op **OK**. Het replicatiebeleid in **Instellingen**kunt u > **replicatie beleid** > beleidsnaam > **Instellingen bewerken**. Wijzigingen die u toepast, worden gebruikt voor machines die al zijn gerepliceerd en nieuwe machines.

    ![Replicatie inschakelen](./media/site-recovery-vmm-to-azure/enable-replication7.png)

U kunt de voortgang van het project **Beveiliging inschakelen** in de **Instellingen**volgen > **taken** > **Site herstel taken**. Nadat de taak **Voltooien bescherming** wordt uitgevoerd de computer is gereed voor failover.

### <a name="view-and-manage-vm-properties"></a>Weergeven en beheren van de eigenschappen van de VM

Wij raden aan dat u de eigenschappen van de broncomputer controleren. Houd er rekening mee dat de naam Azure VM met [Azure VM eisen](site-recovery-best-practices.md#azure-virtual-machine-requirements)dienen te voldoen.

1. Klik op **Instellingen** > **Beveiligde Items** > **Gerepliceerd artikelen** > en selecteer de machine om de details te bekijken.

    ![Replicatie inschakelen](./media/site-recovery-vmm-to-azure/vm-essentials.png)

2. In het dialoogvenster **Eigenschappen** kunt u replicatie en -failover-informatie voor de VM weergeven.

    ![Replicatie inschakelen](./media/site-recovery-vmm-to-azure/test-failover2.png)

3. In het **netwerk en de rekenkracht** > **eigenschappen berekenen** kunt u de grootte van de naam en het doel Azure VM. De naam om te voldoen aan de [eisen van Azure](site-recovery-best-practices.md#azure-virtual-machine-requirements) als u wilt wijzigen. U kunt ook weergeven en wijzigen van informatie over het doelnetwerk, subnetmasker en IP-adres dat toegewezen aan de Azure VM. Let op het volgende:

    - U kunt het doel-IP-adres instellen. Als u een adres, de mislukte niet via de computer opgeeft DHCP gebruikt. Als u een adres dat niet beschikbaar failover wordt uitgevoerd, mislukt de failover. Hetzelfde doel-IP-adres kan worden gebruikt voor failover-test als het adres in het failover-testnetwerk beschikbaar is.
    - Het aantal netwerkadapters wordt bepaald door de grootte die u voor de virtuele machine van doel, als volgt opgeeft:

        - Als het aantal netwerkadapters op de broncomputer kleiner dan of gelijk is aan het aantal adapters die zijn toegestaan voor de grootte van de doel machine is, vervolgens heeft het doel hetzelfde aantal adapters als de bron.
        - Als het aantal adapters voor de virtuele machine van bron groter is dan het voor de doelgrootte wordt het doelformaat maximum toegestane aantal wordt gebruikt.
        - Als bijvoorbeeld een broncomputer twee netwerkadapters heeft en de grootte van target machine ondersteunt vier, moet de doelcomputer twee adapters. Als de broncomputer twee adapters heeft maar de ondersteunde doelgrootte slechts één ondersteunt hebben de doelcomputer slechts één adapter.     
        - Als de VM meerdere netwerkadapters heeft verbinding ze al met hetzelfde netwerk.

    ![Replicatie inschakelen](./media/site-recovery-vmm-to-azure/test-failover4.png)

5.  In **schijven** kunt u het besturingssysteem en de gegevensschijven bekijken op de VM die wordt gerepliceerd.



## <a name="step-7-test-your-deployment"></a>Stap 7: De implementatie testen

U kunt de implementatie testen kunt u een failover testen voor een enkele virtuele machine of een herstelplan met een of meer virtuele machines uitvoeren.


### <a name="prepare-for-failover"></a>Voorbereiden voor failover

- Voor het uitvoeren van een test failover die wordt aangeraden dat u een nieuw Azure netwerk die is geïsoleerd uit het Azure productienetwerk (dit is standaard bij het maken van een nieuw netwerk in Azure) maken. [Meer informatie](site-recovery-failover.md#run-a-test-failover) over het uitvoeren van failover van de test.
- Als u de beste prestaties wanneer u failover uitvoeren naar Azure, de Azure-Agent op de beveiligde computer te installeren. Het wordt sneller opgestart en helpt bij het oplossen van problemen. De agent [Linux](https://github.com/Azure/WALinuxAgent) of [Windows](http://go.microsoft.com/fwlink/?LinkID=394789) te installeren.
- Om uw installatie volledig te testen, moet u een infrastructuur voor de gerepliceerde machine werken zoals verwacht. U kunt een virtuele machine maken als een domeincontroller met DNS en deze repliceren naar Azure met Azure Site herstellen als u wilt testen van Active Directory en DNS. Lees meer in de [overwegingen van failover-test voor Active Directory](site-recovery-active-directory.md#considerations-for-test-failover).
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

- Een openbare eindpunt voor het RDP-protocol (poort 3389) toevoegen en geef de referenties voor aanmelding.
- Controleer of er geen domeinbeleid om te voorkomen dat u geen verbinding maken met een virtuele machine met behulp van een openbaar adres.
- Probeer verbinding te maken. Als u geen verbinding kunt maken, of de VM wordt uitgevoerd. Lees dit [artikel](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx)voor meer tips voor het oplossen van problemen.

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
2. Failover van een herstelplan in **Instellingen** > **Recovery plan**, met de rechtermuisknop op het plan > **Test Failover**. U maakt een herstelplan [voert u deze instructies](site-recovery-create-recovery-plans.md).

3. Selecteer de Azure netwerk waarmee verbinding Azure VMs nadat failover plaatsvindt in **Failover-Test** .
4. Klik op **OK** om te beginnen met de overname. U kunt de voortgang bijhouden door te klikken op de VM de eigenschappen ervan te openen of op de taak **Testen Failover** in **Instellingen** > **Site herstel taken**.
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


## <a name="monitor-your-deployment"></a>De installatie controleren

Hier is het controleren van de configuratie-instellingen, status en gezondheid voor de implementatie van het herstel van de Site:

1. Klik op de naam van de kluis voor toegang tot de dashboard **Essentials** . In dit dashboard kunt u Site-Recovery projecten, replicatiestatus herstelplannen, serverstatus en gebeurtenissen.  U kunt de Essentials tegels en -indelingen die vooral handig voor u zijn, met inbegrip van de status van andere kluizen Site herstel- en back-up weergegeven.

    ![Essentials](./media/site-recovery-vmm-to-azure/essentials.png)

2. In de tegel **gezondheid** kunt u site-servers (VMM of configuratie van servers) ondervonden probleem, en de gebeurtenissen die door de Site herstellen in de afgelopen 24 uur controleren.
3. U kunt beheren en controleren van replicatie in de **Gerepliceerde objecten**, **Recovery plan**, en de **Site herstel taken** naast elkaar. Taken in de **Instellingen voor**detailinformatie -> **taken** -> **Site herstel taken**.


## <a name="next-steps"></a>Volgende stappen

Na de implementatie is ingesteld en uitgevoerd, [meer informatie](site-recovery-failover.md) over verschillende typen van failover.
