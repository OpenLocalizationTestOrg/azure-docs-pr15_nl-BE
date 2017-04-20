<properties
    pageTitle="Hyper-V virtuele machines in de wolken VMM repliceren naar Azure | Microsoft Azure"
    description="In dit artikel wordt beschreven hoe voor het repliceren van Hyper-V virtuele machines op Hyper-V hosts die zich bevinden in System Center VMM wolken op Azure."
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
    ms.topic="hero-article"
    ms.date="05/06/2016"
    ms.author="raynew"/>

#  <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-azure"></a>Hyper-V virtuele machines in de wolken VMM repliceren naar Azure

> [AZURE.SELECTOR]
- [Azure Portal](site-recovery-vmm-to-azure.md)
- [PowerShell - Resource Manager](site-recovery-vmm-to-azure-powershell-resource-manager.md)
- [Klassieke Portal](site-recovery-vmm-to-azure-classic.md)
- [PowerShell - klassiek](site-recovery-deploy-with-powershell.md)



De Azure-Site-Recovery-service draagt bij aan de continuïteit en noodherstel recovery (BCDR) bedrijfsstrategie door regie van replicatie, failover en herstel van virtuele machines en fysieke servers. Machines kunnen worden gerepliceerd naar Azure, of naar een secundaire op ruimten Datacenter. Voor een snel overzicht lezen [Wat is Azure Site terugzetten?](site-recovery-overview.md).

## <a name="overview"></a>Overzicht

In dit artikel wordt beschreven hoe herstel om te repliceren van Hyper-V virtuele machines op Hyper-V host-servers die zich in particuliere wolken VMM naar Azure-Site te implementeren.

Het artikel bevat de vereisten voor het scenario en ziet u hoe een kluis Recovery-Site instelt, krijgt de Azure Site herstel Provider geïnstalleerd op de bronserver VMM, de server registreren in de kluis, een Azure opslag toevoegen, de agent Azure Recovery Services op de host-servers Hyper-V installeren, beveiligingsinstellingen voor VMM wolken die wordt toegepast op alle beveiligde virtuele machines configureren , en bescherming van deze virtuele machines in te schakelen. Voltooien van door het testen van de failover-functie om te controleren of dat alles werkt zoals verwacht.

Opmerkingen of vragen aan de onderkant van dit artikel, of op het [Forum van Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)boeken.

## <a name="architecture"></a>Architectuur

![Architectuur](./media/site-recovery-vmm-to-azure-classic/topology.png)

- Het herstel van Azure siteprovider is geïnstalleerd op de VMM tijdens de implementatie van het herstel van de Site en de VMM-server is geregistreerd in de kluis Site herstellen. De Provider communiceert met Recovery Site replication orchestration verwerken.
- De agent Azure Recovery Services is geïnstalleerd op de host-servers Hyper-V tijdens de implementatie van het herstel van de Site. Gegevensreplicatie naar Azure opslag wordt omgegaan.


## <a name="azure-prerequisites"></a>Azure-vereisten

Hier is wat je hebt in Azure.

**Vereiste** | **Details**
--- | ---
**Azure-account**| U moet een account van [Microsoft Azure](https://azure.microsoft.com/) . U kunt beginnen met een [gratis proefperiode](https://azure.microsoft.com/pricing/free-trial/). [Meer informatie](https://azure.microsoft.com/pricing/details/site-recovery/) over het herstel van de Site-prijzen.
**Azure opslag** | U moet een account Azure opslag gerepliceerde gegevens wilt opslaan. Gerepliceerde gegevens worden opgeslagen in Azure opslag en Azure VMs worden gesponnen omhoog wanneer failover plaatsvindt. <br/><br/>U moet een [account standaard geo-redundante opslag](../storage/storage-redundancy.md#geo-redundant-storage). De account moet in hetzelfde gebied, als de Site-Recovery-service en worden gekoppeld aan het abonnement hetzelfde. Opmerking replicatie naar premium opslag accounts die momenteel niet wordt ondersteund en niet moet worden gebruikt.<br/><br/>[Meer informatie over](../storage/storage-introduction.md) Azure opslag.
**Azure netwerk** | Je hebt een Azure virtueel netwerk die Azure VMs wordt verbonden wanneer failover plaatsvindt. De Azure virtueel netwerk moet in hetzelfde gebied, als de Site herstel kluis.

## <a name="on-premises-prerequisites"></a>Vereisten voor gebouwen

Hier is wat u nodig hebt op gebouwen.

**Vereiste** | **Details**
--- | ---
**VMM** | U moet ten minste één VMM server geïmplementeerd als een zelfstandige fysieke of virtuele server, of als een virtueel cluster. <br/><br/>De VMM-server moet System Center 2012 R2 worden uitgevoerd met de meest recente cumulatieve updates.<br/><br/>U moet ten minste één cloud op de VMM-server geconfigureerd.<br/><br/>De wolk van de bron die u wilt beveiligen, moet een of meer VMM hostgroepen bevatten.<br/><br/>Meer informatie over het instellen van VMM wolken in [scenario: particuliere wolken maken met System Center 2012 SP1 VMM](http://blogs.technet.com/b/keithmayer/archive/2013/04/18/walkthrough-creating-private-clouds-with-system-center-2012-sp1-virtual-machine-manager-build-your-private-cloud-in-a-month.aspx) op de blog van Keith Mayer.
**Hyper-V** | Je hebt een of meer Hyper-V host-servers of clusters in de cloud VMM. De hostserver moet hebben en een of meer VMs. <br/><br/>De Hyper-V server moet worden uitgevoerd op ten minste **Windows Server 2012 R2** met de functie Hyper-V of **Microsoft Hyper-V Server 2012 R2** en de laatste updates hebt geïnstalleerd.<br/><br/>Een VMs die u wilt beveiligen met Hyper-V-server moet zich bevinden in een wolk van VMM.<br/><br/>Als u werkt met Hyper-V in een notitie van het cluster dat cluster broker wordt niet automatisch gemaakt als u een statisch IP-adres gebaseerde cluster. U moet handmatig de broker cluster configureren. [Meer informatie](https://www.petri.com/use-hyper-v-replica-broker-prepare-host-clusters) in het blogbericht van Arjan Finn.
**Beveiligde computers** | [Azure eisen](site-recovery-best-practices.md#azure-virtual-machine-requirements)moeten voldoen aan VMs die u wilt beveiligen.


## <a name="network-mapping-prerequisites"></a>Voorwaarden voor toewijzing van netwerk
Als u virtuele machines in Azure netwerk toewijzing toewijzingen tussen netwerken op de bronserver VMM VM beschermen en Azure netwerken, zodat het volgende doel:

- Alle computers waarvoor failover op hetzelfde netwerk verbinding met elkaar maken kan, ongeacht welke herstelplan zijn.
- Als een netwerkgateway setup op de doelcomputer Azure netwerk, worden virtuele machines kunt verbinden met andere op ruimten virtuele machines.
- Als u geen netwerk-toewijzing alleen virtuele machines configureren die in het herstelplan dezelfde failover niet verbinding kunnen maken met elkaar na een failover naar Azure.

Als u de toewijzing van het netwerk te implementeren hebt u het volgende nodig:

- De virtuele machines die u wilt beveiligen op de bronserver VMM moet worden verbonden met een netwerk voor VM. Dit netwerk moet worden gekoppeld aan een logisch netwerk dat is gekoppeld aan de cloud.
- Een Azure netwerk die gerepliceerde virtuele machines verbinding na een failover maken kan. U zult dit netwerk selecteren op het moment van failover. Het netwerk moet zich in hetzelfde gebied, als uw abonnement Azure Site herstellen.

Voorbereiding voor het netwerk als volgt toewijzen:

1. [Meer informatie over](site-recovery-network-mapping.md) vereisten voor toewijzing.
2. VM-netwerken in VMM voorbereiden:

    - [Logische netwerken instellen](https://technet.microsoft.com/library/jj721568.aspx).
    - [VM-netwerken instellen](https://technet.microsoft.com/library/jj721575.aspx).


## <a name="step-1-create-a-site-recovery-vault"></a>Stap 1: Maak een kluis Site herstellen

1. Aanmelden bij de [Portal voor beheer](https://portal.azure.com) van de VMM-server die u wilt registreren.
2. Klik op de **Data Services** > **Recovery Services** > **Site herstel kluis**.
3. Klik op **Nieuw** > **snel tabellen maken**.
4. Voer in het vak **naam**een beschrijvende naam ter identificatie van de kluis.
5. Selecteer de regio voor de kluis in de **regio**. Ondersteunde regio's Zie geografische beschikbaarheid in [Azure Site herstel prijzen Details](https://azure.microsoft.com/pricing/details/site-recovery/)controleren.
6. Klik op **de kluis maken**.

    ![Nieuwe kluis](./media/site-recovery-vmm-to-azure-classic/create-vault.png)

Controleer de statusbalk om te bevestigen dat de kluis is gemaakt. De kluis wordt aangeboden als **actief** op de hoofdpagina Recovery Services.

## <a name="step-2-generate-a-vault-registration-key"></a>Stap 2: Een kluis registratie sleutel genereren

Een registratie sleutel genereren in de kluis. Nadat u de Provider Azure Site Recovery downloaden en op de server VMM installeren, gebruikt u deze sleutel de VMM-server registreren in de kluis.

1. Klik op de kluis Open de pagina Quick Start **Recovery Services** op de pagina. Quick Start kan ook worden geopend via het pictogram.

    ![Quick Start-pictogram](./media/site-recovery-vmm-to-azure-classic/qs-icon.png)

2. Selecteer in de vervolgkeuzelijst **tussen een site op ruimten VMM en Microsoft Azure**.
3. **VMM Servers voorbereiden**, klikt u op bestand **registratie-sleutel genereren** . Het bestand wordt automatisch gegenereerd en is geldig voor 5 dagen nadat het gegenereerd. Als u niet de Azure portal van de VMM-server moet u dit bestand kopiëren naar de server.

    ![Registratie sleutel](./media/site-recovery-vmm-to-azure-classic/register-key.png)

## <a name="step-3-install-the-azure-site-recovery-provider"></a>Stap 3: Installeer de Provider Azure Site herstellen

1. In de **Quick Start** > **voorbereiden VMM-servers**, klik op **Download Microsoft Azure Site Recovery-Provider voor installatie op VMM-servers** om de meest recente versie van het installatiebestand van de Provider.
2. Dit bestand op de bronserver VMM uitvoeren.

    >[AZURE.NOTE] VMM is geïmplementeerd in een cluster en u installeert de voorziening voor de eerste keer installeren op een actief knooppunt als de installatie om de VMM-server registreren in de kluis te voltooien. Vervolgens installeert u de Provider op de andere knooppunten. Houd er rekening mee dat als u een de Provider upgrade uitvoert u op alle knooppunten een upgrade moet omdat ze moeten al worden uitgevoerd dezelfde versie van de Provider.

3. Het installatieprogramma voert een prerequirements uit en vraagt toestemming om te stoppen met de VMM-service Provider setup start. De VMM-Service worden gestart nadat setup is voltooid. Als u op een servercluster VMM installeert wordt u gevraagd om te stoppen met de rol van het Cluster.

4. In **Microsoft Update** kunt u opt-in voor updates. Met deze instelling ingeschakeld Provider volgens het beleid van Microsoft Update updates worden geïnstalleerd.

    ![Microsoft-Updates](./media/site-recovery-vmm-to-azure-classic/updates.png)


5.  De installatielocatie voor de Provider is ingesteld op ** <SystemDrive>\Program Files\Microsoft Manager\bin van System Center 2012 R2\Virtual Machine**. Klik op **installeren**.

    ![InstallLocation](./media/site-recovery-vmm-to-azure-classic/install-location.png)

6. Klik op **registreren** als u de server registreren in de kluis wilt nadat u de voorziening hebt geïnstalleerd.

    ![InstallComplete](./media/site-recovery-vmm-to-azure-classic/install-complete.png)

9. Controleer of de naam van de kluis waarin de server wordt geregistreerd in het vak **naam van de kluis**. Klik op *volgende*.

    ![Server-registratie](./media/site-recovery-vmm-to-azure-classic/vaultcred.PNG)

7. Opgeven hoe de Provider op de VMM-server verbinding maakt met Internet in **Verbinding met Internet** . Selecteer **verbinding maken met bestaande proxy-instellingen** op over het gebruik van Internet-verbinding geconfigureerd op de server.

    ![Internet-instellingen](./media/site-recovery-vmm-to-azure-classic/proxydetails.PNG)

    - Als u een aangepaste proxy gebruiken moet u deze instellen voordat u de voorziening hebt geïnstalleerd. Wanneer u aangepaste proxy-instellingen configureren wordt een test uitgevoerd om te controleren van de proxyverbinding.
    - Als u een aangepaste proxy gebruikt, of de standaardproxy verificatie moet u de proxy-details vereist, inclusief proxy-adres en poort opgeven.
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

    ![LastPage](./media/site-recovery-vmm-to-azure-classic/provider13.PNG)

Na registratie wordt metagegevens van de VMM-server opgehaald door Azure Site herstellen. De server wordt weergegeven op het tabblad **Servers van VMM** op de pagina **Servers** in de kluis.

### <a name="command-line-installation"></a>Installatie via opdrachtregel

De Provider Azure Site herstel kan ook worden geïnstalleerd met de volgende opdrachtregel. Deze methode kan worden gebruikt om de voorziening hebt geïnstalleerd op een Server Core van Windows Server 2012 R2.

1. De Provider installatie en registratie sleutel downloaden naar een map. Bijvoorbeeld: C:\ASR.
2. De System Center Virtual Machine Manager-service stoppen
3. Pak de installer Provider met de volgende opdrachten uit vanaf een opdrachtprompt:

        C:\Windows\System32> CD C:\ASR
        C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q

4. De voorziening als volgt installeren:

        C:\ASR> setupdr.exe /i

5. De voorziening als volgt registreren:

        CD C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin
        C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin\> DRConfigurator.exe /r  /Friendlyname <friendly name of the server> /Credentials <path of the credentials file> /EncryptionEnabled <full file name to save the encryption certificate>       

Wanneer parameters zijn als volgt:

 - **/Credentials** : verplichte parameter die de locatie waar het sleutelbestand registratie  
 - **/FriendlyName** : verplichte parameter voor de naam van de Hyper-V host-server die wordt weergegeven in de portal Azure Site herstellen.
 - **/EncryptionEnabled** : optionele parameter opgeven als u coderen uw virtuele machines in Azure (bij codering met rest wilt). De bestandsnaam moet de extensie **.pfx** hebben.
 - **/proxyAddress** : optionele parameter die het adres van de proxyserver.
 - **/proxyport** : optionele parameter die de poort van de proxyserver.
 - **/proxyUsername** : optionele parameter die de naam van de proxyserver.
 - **/proxyPassword** : optionele parameter die het proxywachtwoord.  


## <a name="step-4-create-an-azure-storage-account"></a>Stap 4: Maak een account Azure opslag

1. Als u geen Klik een account Azure opslag **een Azure opslag Account toevoegen** aan een account maken.
2. Maak een account met geo-replicatie is ingeschakeld. Het moet in hetzelfde gebied, als de Azure-Site-Recovery-service en worden gekoppeld aan het abonnement hetzelfde.

    ![Opslag account](./media/site-recovery-vmm-to-azure-classic/storage.png)

> [AZURE.NOTE] [Migratie van accounts opslag](../resource-group-move-resources.md) via resourcegroepen in het hetzelfde abonnement of via abonnementen wordt niet ondersteund voor opslag gebruikt voor het herstel van de Site te implementeren.

## <a name="step-5-install-the-azure-recovery-services-agent"></a>Stap 5: De herstelagent voor Azure Services installeren

De agent Azure Recovery Services installeren op elke host Hyper-V server in de cloud VMM.

1. Klik op de **Werkbalk Snel starten** > **downloaden Azure Site Services herstelagent en installeren op de hosts** om de meest recente versie van het installatiebestand van de agent.

    ![Herstelagent Services installeren](./media/site-recovery-vmm-to-azure-classic/install-agent.png)

2. Het bestand voor installatie op elke host Hyper-V server uitvoeren.
3. Klik op **volgende**op de pagina **Controle van vereisten** . Eventuele ontbrekende vereisten wordt automatisch geïnstalleerd.

    ![De herstelagent Services voorwaarden](./media/site-recovery-vmm-to-azure-classic/agent-prereqs.png)

4. Geef op waar u de agent installeren en selecteer de locatie van de cache waarin back-metagegevens worden geïnstalleerd op de pagina **Instellingen voor de installatie** . Klik vervolgens op **installeren**.
5. Klik op **sluiten** om de wizard te voltooien nadat de installatie is voltooid.

    ![MARS Agent registreren](./media/site-recovery-vmm-to-azure-classic/agent-register.png)

### <a name="command-line-installation"></a>Installatie via opdrachtregel

U kunt ook de Microsoft Azure herstelagent Services installeren vanaf de opdrachtregel met behulp van deze opdracht:

    marsagentinstaller.exe /q /nu

## <a name="step-6-configure-cloud-protection-settings"></a>Stap 6: Wolk configureren beveiligingsinstellingen

Nadat de VMM-server is geregistreerd, kunt u de cloud-beveiligingsinstellingen kunt configureren. U kunt de optie **cloud-gegevens synchroniseren met de kluis** ingeschakeld wanneer u de voorziening hebt geïnstalleerd, zodat alle wolken op de VMM-server wordt weergegeven op het tabblad <b>Beveiligde Items</b> in de kluis.

![Gepubliceerde Cloud](./media/site-recovery-vmm-to-azure-classic/clouds-list.png)

1. Klik op **beveiliging van VMM wolken instellen**op de pagina Quick Start.
2. Klik op het tabblad **Beveiligde Items** op de wolk die u wilt configureren en Ga naar het tabblad **configuratie** .
3. Selecteer **Azure**in **doel** .
4. Selecteer de opslag Azure-account die u voor replicatie gebruikt in **Opslag Account** .
5. Stel **coderen opgeslagen gegevens** op **uit**. Deze instelling geeft aan dat gegevens gerepliceerd tussen de site op gebouwen en Azure moeten worden gecodeerd.
6. Laat de standaardinstelling in **frequentie kopiëren** . Deze waarde bepaalt hoe vaak de gegevens moeten worden gesynchroniseerd tussen bron- en doellocaties.
7. Laat de standaardinstelling in **bewaren herstellen punten voor**. Met een standaardwaarde van nul, wordt alleen het meest recente herstelpunt voor een primaire virtuele machine op een replica host-server opgeslagen.
8. Laat de standaardinstelling in **frequentie van toepassing consistente momentopnamen**. Deze waarde geeft aan hoe vaak momentopnamen te maken. Volume Shadow Copy Service (VSS) momentopnamen gebruiken om ervoor te zorgen dat de toepassingen in een consistente status zijn als de momentopname is gemaakt.  Als u een waarde instellen wilt, moet minder dan het aantal extra herstel punten die u configureert.
9. In de **begintijd van de replicatie**, opgeven wanneer de eerste replicatie van gegevens naar Azure moet beginnen. De tijdzone op de hostserver Hyper-V wordt gebruikt. We raden aan dat de aanvankelijke replicatie tijdens rustige uren.

    ![De replicatie-instellingen voor cloud](./media/site-recovery-vmm-to-azure-classic/cloud-settings.png)

Nadat u de instellingen van een taak wordt gemaakt en kan worden gecontroleerd op het tabblad **taken** opslaan. Alle Hyper-V host-servers in de cloud VMM bron wordt geconfigureerd voor replicatie.

Na het opslaan, kunnen cloud-instellingen worden gewijzigd op het tabblad **configureren** . Als de doelaccount opslag of doellocatie wilt wijzigen moet u de configuratie van de wolk te verwijderen en vervolgens opnieuw de cloud. Houd er rekening mee dat als u de opslag account wijzigt de wijziging alleen voor virtuele machines die zijn ingeschakeld voor bescherming geldt, nadat de opslag account is gewijzigd. Bestaande virtuele machines worden niet gemigreerd naar de nieuwe account voor opslag.

## <a name="step-7-configure-network-mapping"></a>Stap 7: Toewijzing van netwerk configureren
Voordat u begint met het netwerk toewijzing controleert u of dat virtuele machines op de bronserver VMM bent verbonden met een netwerk voor VM. Ook een of meer Azure virtuele netwerken maken. Houd er rekening mee dat meerdere VM-netwerken kunnen worden toegewezen aan één Azure netwerk.

1. Klik op de pagina Quick Start **netwerken toewijzen**.
2. Selecteer de bronserver VMM op de **bronlocatie**, op het tabblad **netwerken** . Selecteer in de **doellocatie** Azure.
3. In netwerken met **bron** een lijst van VM-netwerken die zijn gekoppeld aan de VMM-server worden weergegeven. In netwerken met **doel** worden de Azure netwerken die zijn gekoppeld aan het abonnement weergegeven.
4. Selecteer de bron VM netwerk en klik op **toewijzen**.
5. Selecteer op de pagina **Selecteer een doelnetwerk** , het doel Azure netwerk dat u wilt gebruiken.
6. Klik op het selectievakje om de toewijzing te voltooien.

    ![De replicatie-instellingen voor cloud](./media/site-recovery-vmm-to-azure-classic/map-networks.png)

Nadat u de instellingen opslaan een taak wordt gestart voor het bijhouden van de voortgang van de toewijzing en het op het tabblad taken kan worden gecontroleerd. Eventuele bestaande replica virtuele machines die met de bron VM netwerk overeenkomen worden verbonden met het doel Azure netwerken. Nieuwe virtuele machines die zijn verbonden met de bron VM netwerk worden verbonden met het toegewezen netwerkstation Azure na replicatie. Als u een bestaande toewijzing met een nieuw netwerk wijzigt, worden replica virtuele machines verbonden met de nieuwe instellingen.

Houd er rekening mee dat als het netwerk meerdere subnetten heeft en een van de subnetten heeft dezelfde naam als het subnet waarop de virtuele machine van bron zich bevindt en vervolgens de virtuele machine van replica worden verbonden met dat doel subnet na een failover. Als er geen subnet doel met een overeenkomende naam, wordt de virtuele machine worden verbonden met het eerste subnet in het netwerk.

> [AZURE.NOTE] [Migratie van netwerken](../resource-group-move-resources.md) via resourcegroepen in het hetzelfde abonnement of via abonnementen wordt niet ondersteund voor netwerken die worden gebruikt voor het herstel van de Site te implementeren.

## <a name="step-8-enable-protection-for-virtual-machines"></a>Stap 8: Beveiliging voor virtuele machines inschakelen

Nadat de servers, wolken en netwerken goed zijn geconfigureerd, kunt u de beveiliging voor virtuele machines in de cloud. Let op het volgende:

- Virtuele machines moeten voldoen aan de [eisen van Azure](site-recovery-best-practices.md#azure-virtual-machine-requirements).
- Om de beveiliging van het besturingssysteem en het besturingssysteem inschakelen moeten schijfeigenschappen worden ingesteld voor de virtuele machine. U kunt de eigenschap instellen wanneer u een virtuele machine in VMM met behulp van de sjabloon voor een virtuele machine maakt. U kunt deze eigenschappen voor bestaande virtuele machines ook instellen op de tabbladen **Algemeen** en **Hardwareconfiguratie** van de eigenschappen van de virtuele machine. Als u deze eigenschappen niet in de VMM instellen zult u kunnen ze in de portal Azure Site herstel configureren.

    ![Virtuele machine maken](./media/site-recovery-vmm-to-azure-classic/enable-new.png)

    ![Eigenschappen van de virtuele machine wijzigen](./media/site-recovery-vmm-to-azure-classic/enable-existing.png)


1. Als beveiliging op het tabblad **virtuele Machines** in de cloud waarin de virtuele machine bevindt, klikt u op **Beveiliging inschakelen** > **virtuele machines toevoegen**.
2. Selecteer in de lijst van virtuele machines in de cloud, die u wilt beveiligen.

    ![Virtuele machine beveiliging inschakelen](./media/site-recovery-vmm-to-azure-classic/select-vm.png)

    De voortgang van de actie **Beveiliging inschakelen** op het tabblad **taken** , met inbegrip van de aanvankelijke replicatie bijhouden. Nadat de taak **Voltooien bescherming** wordt uitgevoerd is de virtuele machine gereed voor failover. Nadat de beveiliging is ingeschakeld en virtuele machines worden gerepliceerd, zult u kunnen ze bekijken in Azure.


    ![Virtuele machine bescherming taak](./media/site-recovery-vmm-to-azure-classic/vm-jobs.png)

3. De eigenschappen van de virtuele machine controleren en indien nodig wijzigen.

    ![Controleer of de virtuele machines](./media/site-recovery-vmm-to-azure-classic/vm-properties.png)


4. Op het tabblad **configureren** van de eigenschappen van de virtuele machine de volgende netwerkeigenschappen kunnen worden gewijzigd.





- **Aantal netwerkadapters op de virtuele machine van doel** - het aantal netwerkadapters wordt bepaald door de grootte die u voor de virtuele machine van doel opgeeft. Controleer de [specificaties van de virtuele machine grootte](../virtual-machines/virtual-machines-linux-sizes.md#size-tables) voor het aantal adapters worden ondersteund door de grootte van de virtuele machine. Wanneer u de grootte van een virtuele machine wijzigen en opslaan van de instellingen, wordt het nummer van de netwerkadapter wordt gewijzigd wanneer u **configureren** pagina de volgende keer opent. Het aantal netwerkadapters van target virtuele machines is het minimum aantal netwerkadapters op bron virtuele machine en het maximum aantal netwerkadapters die worden ondersteund door de grootte van de virtuele machine als volgt gekozen:

    - Als het aantal netwerkadapters op de broncomputer kleiner dan of gelijk is aan het aantal adapters die zijn toegestaan voor de grootte van de doel machine is, vervolgens heeft het doel hetzelfde aantal adapters als de bron.
    - Als het aantal adapters voor de virtuele machine van bron het aantal dat is toegestaan overschrijdt voor de doelgrootte en het doel maximaal worden gebruikt.
    - Als een broncomputer twee netwerkadapters heeft en de grootte van target machine vier ondersteunt, wordt de doelcomputer twee adapters hebben. Als de broncomputer twee adapters heeft maar de ondersteunde doelgrootte slechts één ondersteunt hebben de doelcomputer slechts één adapter.    

- **Netwerk van de virtuele machine van doel** - de waarop de virtuele machine maakt verbinding met netwerk wordt bepaald door de toewijzing van het netwerk van de broncomputer virtueel netwerk. Als de bron virtuele machine meer dan één netwerkadapter heeft en bron netwerken worden toegewezen aan verschillende netwerken op doel, vervolgens moet u kiezen tussen een van de doel-netwerken.
- **Subnet van elke netwerkadapter** - voor elke netwerkadapter kunt u het subnet waaraan de mislukte via virtuele machine wil maken.
- **Doel-IP-adres** - als de netwerkadapter van de bron virtuele machine is geconfigureerd voor het gebruik van een statisch IP-adres en vervolgens u het IP-adres voor de virtuele machine van doel opgeven kunt. Gebruik deze functie het IP-adres van een virtuele machine van bron behouden nadat failover is uitgevoerd. Als er geen IP-adres wordt vervolgens alle beschikbare IP-adres gegeven aan de netwerkadapter op het moment van failover. Als het doel-IP-adres is opgegeven, maar wordt al gebruikt door een andere virtuele machine uitgevoerd in Azure mislukt failover.  

    ![Netwerkeigenschappen wijzigen](./media/site-recovery-vmm-to-azure-classic/multi-nic.png)

>[AZURE.NOTE] Linux virtuele machines met een statisch IP-adres worden niet ondersteund.

## <a name="test-the-deployment"></a>De implementatie testen

Voor het testen van de implementatie test failover voor een enkele virtuele machine uitvoert of een herstelplan dat bestaat uit meerdere virtuele machines maken en uitvoeren van failover test voor het plan.  

Failover van de test simuleert de failover- en herstel mechanisme in een geïsoleerd netwerk. Houd rekening met:

- Als u wilt verbinding maken met de virtuele machine in Azure met extern bureaublad na de failover, inschakelen verbinding met extern bureaublad op de virtuele machine voordat u de test failover uitvoeren.
- Na een failover gebruikt u een openbaar IP-adres verbinding kunnen maken met de virtuele machine in Azure met extern bureaublad. Als u dit doen wilt, zorg ervoor dat u geen domeinbeleid om te voorkomen dat u geen verbinding maken met een virtuele machine met behulp van een openbaar adres.

>[AZURE.NOTE] Als u de beste prestaties wanneer u een failover naar Azure uitvoeren, ervoor te zorgen dat de Agent Azure in de beveiligde computer is geïnstalleerd. Dit helpt in sneller opstarten en helpt ook bij het stellen van een diagnose bij problemen. Linux-agent kan worden gevonden [hier](https://github.com/Azure/WALinuxAgent) - en Windows agent vindt u [hier](http://go.microsoft.com/fwlink/?LinkID=394789)

### <a name="create-a-recovery-plan"></a>Maak een herstelplan

1. Voeg een nieuw plan op het tabblad **Systeemherstel plannen** . Geef een naam, **VMM** in **brontype**en de bronserver VMM in de **bron**, het doel is Azure.

    ![Herstelplan maken](./media/site-recovery-vmm-to-azure-classic/recovery-plan1.png)

2. Selecteer op de pagina **Selecteer virtuele Machines** virtuele machines toe te voegen aan het herstelplan. Deze virtuele machines worden toegevoegd aan de standaardgroep recovery plan, groep 1. Een maximum van 100 virtuele machines in een enkele herstelplan zijn getest.

- Als u wilt dat de eigenschappen van de virtuele machine controleren voordat u deze toevoegt aan het plan, klikt u op de virtuele machine op de eigenschappenpagina van de wolk waarin het zich bevindt. U kunt ook de eigenschappen van de virtuele machine configureren in de console van VMM.
- Alle van de virtuele machines die worden weergegeven is voor bescherming ingeschakeld. De lijst bevat zowel virtuele machines die zijn ingeschakeld voor de bescherming en de eerste replicatie is voltooid en die zijn ingeschakeld voor de bescherming bij de eerste replicatie in behandeling. Alleen de virtuele machines met de eerste replicatie is voltooid kan worden uitgevoerd als onderdeel van een herstelplan.

    ![Herstelplan maken](./media/site-recovery-vmm-to-azure-classic/select-rp.png)

Nadat een herstelplan is gemaakt dat wordt weergegeven op het tabblad **Systeemherstel plannen** . U kunt ook [Azure automatisering runbooks](site-recovery-runbook-automation.md) toevoegen aan het herstelplan voor het automatiseren van acties tijdens failover.

### <a name="run-a-test-failover"></a>Een failover test uitvoeren

Er zijn twee manieren om een test failover uitvoeren naar Azure.

- **Test de failover-functie zonder een netwerk Azure**— dit type test failover wordt gecontroleerd dat de virtuele machine juist in Azure wordt. De virtuele machine wordt niet aangesloten op een netwerk Azure na een failover.
- **Test de failover-functie met een netwerk Azure**: dit type van failover-de volledige replicatieomgeving afkomstig van verwacht en die niet via de virtuele machines worden verbonden met het opgegeven doel Azure netwerk. Voor het verwerken van subnet voor test zal failover het subnet van de virtuele machine van test worden gekeken hoe op basis van het subnet van de replica virtuele machine. Dit is om de normale replicatie als het subnet van een virtuele machine van replica is gebaseerd op het subnet van de bron virtuele machine.

Als u wilt dat voor het uitvoeren van failover voor een virtuele machine voor Azure-beveiliging is ingeschakeld zonder een doelnetwerk Azure test moet u niet iets voorbereiden. Een failover test uitgevoerd met een doel Azure netwerk moet u een nieuwe Azure netwerk dat geïsoleerd is van het productienetwerk Azure (standaardgedrag bij het maken van een nieuw netwerk in Azure) maken. Bekijk [een test failover uitvoeren](site-recovery-failover.md#run-a-test-failover) voor meer informatie.


Ook moet u voor het instellen van de infrastructuur voor de virtuele machine met gerepliceerde werken zoals verwacht. Bijvoorbeeld een virtuele machine met een domeincontroller en DNS kunnen worden gerepliceerd naar Azure met Azure Site herstellen en kan worden gemaakt in de testnetwerk met failover-Test. Bekijk de sectie [test de failover-overwegingen voor active directory](site-recovery-active-directory.md#considerations-for-test-failover) voor meer informatie.

Een failover-test doen het volgende uitvoeren:

1. Klik op het tabblad **Systeemherstel plannen** het plan en klik op **Failover testen**.
2. Selecteer op de pagina **Bevestigen Test Failover** **geen** of een bepaald netwerk Azure.  Houd er rekening mee dat als u geen failover test u de virtuele machine correct gerepliceerd controleert naar Azure maar uw netwerkconfiguratie replicatie controleert.

    ![Geen netwerk](./media/site-recovery-vmm-to-azure-classic/test-no-network.png)

3. Als codering is ingeschakeld voor de cloud, selecteer het certificaat dat is uitgegeven tijdens de installatie van de Provider op de VMM-server bij het inschakelen van de optie voor het inschakelen van codering van gegevens voor een wolk in **Coderingssleutel** .
4. Klik op het tabblad **taken** kunt u failover-voortgang bijhouden. Ook moet u de replica van de virtuele machine testen in de portal Azure zien. Als u ingesteld voor toegang tot virtuele machines vanaf het netwerk op ruimten bent kunt u een verbinding met extern bureaublad op de virtuele machine te starten.
5. Als de failover-functie de **Complete test** fase bereikt, klikt u op **Volledige Test** van de overname van de test is voltooid. U kunt inzoomen op het tabblad **taak** om bij te houden van de status en voortgang van failover- en eventuele acties uitvoeren die nodig zijn.
6. Na een failover zult u kunnen zien van de replica van de virtuele machine testen in de portal Azure. Als u ingesteld voor toegang tot virtuele machines vanaf het netwerk op ruimten bent kunt u een verbinding met extern bureaublad op de virtuele machine te starten. Voer de volgende handelingen uit:

    1. Controleer of de virtuele machines is gestart.
    2. Als u wilt verbinding maken met de virtuele machine in Azure met extern bureaublad na de failover, inschakelen verbinding met extern bureaublad op de virtuele machine voordat u de test failover uitvoeren. Ook moet u een RDP-eindpunt op de virtuele machine toevoegen. U kunt gebruikmaken van een [Azure automatisering Runbooks](site-recovery-runbook-automation.md) daarvoor.
    3. Nadat de failover, als u een openbaar IP-adres gebruikt om verbinding met de virtuele machine in Azure met extern bureaublad te garanderen hebt u geen domeinbeleid om te voorkomen dat u geen verbinding maken met een virtuele machine met behulp van een openbaar adres.

7.  Nadat de test voltooid is als volgt:
    - Klik op **de test failover is uitgevoerd**. Schoonmaken van de testomgeving automatisch uit te schakelen en verwijder de test virtuele machines.
    - Klik op **notities** als u wilt opnemen en opslaan van eventuele opmerkingen die zijn gekoppeld aan de overname van de test.

>

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [het instellen van herstelplannen](site-recovery-create-recovery-plans.md) en [overname bij storingen](site-recovery-failover.md).
