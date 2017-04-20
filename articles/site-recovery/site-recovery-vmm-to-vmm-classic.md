<properties
    pageTitle="Hyper-V virtuele machines in de wolken VMM repliceren naar een secundaire VMM-site | Microsoft Azure"
    description="In dit artikel wordt beschreven hoe Hyper-V VMs in wolken VMM repliceren naar een secundaire site VMM met Azure Site herstellen."
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

# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-a-secondary-vmm-site"></a>Hyper-V virtuele machines in de wolken VMM repliceren naar een secundaire VMM-site

> [AZURE.SELECTOR]
- [Azure portal](site-recovery-vmm-to-vmm.md)
- [Klassieke portal](site-recovery-vmm-to-vmm-classic.md)
- [PowerShell - Resource Manager](site-recovery-vmm-to-vmm-powershell-resource-manager.md)

De Azure-Site-Recovery-service draagt bij aan de continuïteit en noodherstel recovery (BCDR) bedrijfsstrategie door regie van replicatie, failover en herstel van virtuele machines en fysieke servers. Machines kunnen worden gerepliceerd naar Azure, of naar een secundaire op ruimten Datacenter. Voor een snel overzicht lezen [Wat is Azure Site terugzetten?](site-recovery-overview.md)

## <a name="overview"></a>Overzicht

In dit artikel wordt beschreven hoe voor het repliceren van Hyper-V virtuele machines op Hyper-V host-servers die worden beheerd in wolken VMM secundaire VMM site met Azure Site herstellen.

Het artikel bevat vereisten, ziet u hoe een kluis Recovery Site instellen, de Azure Site herstel voorziening hebt geïnstalleerd op een bron en VMM doelservers de servers registreren in de kluis, bescherming-instellingen configureren voor VMM wolken en schakelt u beveiliging voor VMs Hyper-V. Voltooien van door het testen van de failover-functie om te controleren of dat alles werkt zoals verwacht.

Opmerkingen of vragen aan de onderkant van dit artikel, of op het [Forum van Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)boeken.

## <a name="architecture"></a>Architectuur

De volgende afbeelding ziet u de verschillende communicatiekanalen en de poorten die door Azure Site herstel voor orchestration en replicatie

![E2E-topologie](./media/site-recovery-vmm-to-vmm-classic/e2e-topology.png)

## <a name="before-you-start"></a>Voordat u begint

Zorg ervoor dat u hebt deze vereisten:

**Vereisten** | **Details**
--- | ---
**Azure**| U moet een account van [Microsoft Azure](https://azure.microsoft.com/) . U kunt beginnen met een [gratis proefperiode](https://azure.microsoft.com/pricing/free-trial/). [Meer informatie](https://azure.microsoft.com/pricing/details/site-recovery/) over het herstel van de Site-prijzen.
**VMM** | U moet ten minste één VMM-server.<br/><br/>De VMM-server waarop ten minste System Center 2012 SP1 met de meest recente cumulatieve updates.<br/><br/>Als u de beveiliging met één server VMM instellen wilt, moet u ten minste twee wolken op de server geconfigureerd.<br/><br/>Als u implementeren bescherming met twee VMM-servers wilt, moet elke server ten minste één cloud geconfigureerd op de primaire VMM-server die u wilt beveiligen en een wolk die zijn geconfigureerd op de secundaire VMM-server die u wilt gebruiken voor bescherming en herstel<br/><br/>Alle VMM wolken moeten hebben van de Hyper-V-mogelijkheid profiel instellen.<br/><br/>De wolk van de bron die u wilt beveiligen, moet een of meer VMM hostgroepen bevatten.<br/><br/>Meer informatie over het instellen van VMM wolken in [scenario: particuliere wolken maken met System Center 2012 SP1 VMM](http://blogs.technet.com/b/keithmayer/archive/2013/04/18/walkthrough-creating-private-clouds-with-system-center-2012-sp1-virtual-machine-manager-build-your-private-cloud-in-a-month.aspx) op de blog van Keith Mayer.
**Hyper-V** | U moet een of meer Hyper-V host-servers in de primaire en secundaire VMM hostgroepen en een of meer virtuele machines op elke host Hyper-V server.<br/><br/>De host- en Hyper-V-servers moeten actief zijn ten minste Windows Server 2012 met de functie Hyper-V en de laatste updates hebt geïnstalleerd.<br/><br/>Een VMs die u wilt beveiligen met Hyper-V-server moet zich bevinden in een wolk van VMM.<br/><br/>Als u Hyper-V in een cluster uitvoert, ziet u dat cluster broker wordt niet automatisch gemaakt als u een statisch IP-adres gebaseerde cluster. U moet handmatig de broker cluster configureren. [Meer informatie](https://www.petri.com/use-hyper-v-replica-broker-prepare-host-clusters) in het blogbericht van Arjan Finn.
**Netwerk toewijzen** | U kunt netwerk toewijzen om ervoor te zorgen dat gerepliceerde virtuele machines zijn optimaal geplaatst op secundaire servers voor Hyper-V host na een failover en ze verbinding kunnen maken met passende VM-netwerken. Als de toewijzing van het netwerk niet is geconfigureerd, wordt niet replica VMs na een failover op een netwerk aangesloten.<br/><br/>Zorg ervoor dat de virtuele machines in de bron van Hyper-V server met een netwerk voor VM VMM verbonden bent toewijzing tijdens de implementatie van netwerk stelt. Dit netwerk moet worden gekoppeld aan een logisch netwerk dat is gekoppeld aan de cloud. < br /<br/>De cloud doel op de secundaire VMM-server die u voor herstel gebruikt moet een corresponderende VM netwerk is geconfigureerd en deze op zijn beurt moet worden gekoppeld aan een bijbehorende logisch netwerk dat is gekoppeld aan de doel-wolk.<br/><br/>[Meer informatie](site-recovery-network-mapping.md) over het toewijzen van netwerk.
**Toewijzing van opslagruimte** | Standaard wanneer u een virtuele machine op een bron Hyper-V host-server naar een Hyper-V host doelserver repliceert, gerepliceerde gegevens opgeslagen op de standaardlocatie die wordt aangegeven voor de doelhost Hyper-V in Hyper-V-beheer. Voor meer controle over welke gerepliceerde gegevens zijn opgeslagen, kunt u de toewijzing van opslagruimte<br/><br/> Opslag toewijzing configureren, moet u instellen opslag classificaties op de bron en VMM doelservers voordat u implementeren gaat. [Meer informatie](site-recovery-storage-mapping.md).


## <a name="step-1-create-a-site-recovery-vault"></a>Stap 1: Maak een kluis Site herstellen

1. Aanmelden bij de [Portal voor beheer](https://portal.azure.com) van de VMM-server die u wilt registreren.

2. Vouw **Services Data** > **Recovery Services** en klik op **Site-Recovery kluis**.

3. Klik op **Nieuw** > **snel tabellen maken**.

4. Voer in het vak **naam**een beschrijvende naam ter identificatie van de kluis.

5. Selecteer de regio voor de kluis in **de regio** . Ondersteunde regio's Zie geografische beschikbaarheid in [Azure Site herstel prijzen Details](http://go.microsoft.com/fwlink/?LinkId=389880)controleren.

6. Klik op **de kluis maken**.

    ![Kluis maken](./media/site-recovery-vmm-to-vmm-classic/create-vault.png)

Controleer op de statusbalk of de kluis is gemaakt. De kluis wordt aangeboden als **actief** op de hoofdpagina Recovery Services.

## <a name="step-2-generate-a-vault-registration-key"></a>Stap 2: Een kluis registratie sleutel genereren

Een registratie sleutel genereren in de kluis. Nadat u de Provider Azure Site Recovery downloaden en op de server VMM installeren, gebruikt u deze sleutel de VMM-server registreren in de kluis.

1. Klik op de kluis Open de pagina Quick Start **Recovery Services** op de pagina. Quick Start kan ook worden geopend via het pictogram.

    ![Quick Start-pictogram](./media/site-recovery-vmm-to-vmm-classic/quick-start-icon.png)

2. Selecteer in de vervolgkeuzelijst **tussen twee op ruimten VMM sites**.
3. Klik in **VMM Servers voorbereiden**op **belangrijke registratiebestand genereren**. Het bestand wordt automatisch gegenereerd en is geldig voor 5 dagen nadat het gegenereerd. Als u niet de Azure portal van de VMM-server, moet u dit bestand kopiëren naar de server.

    ![Registratie sleutel](./media/site-recovery-vmm-to-vmm-classic/register-key.png)

## <a name="step-3-install-the-azure-site-recovery-provider"></a>Stap 3: Installeer de Provider Azure Site herstellen

4. Klik op **Download Microsoft Azure Site Recovery-Provider voor installatie op VMM-servers** om de meest recente versie van het installatiebestand van de Provider op de pagina **Quick Start** in **servers voorbereiden VMM**.

2. Dit bestand op de bronserver VMM uitvoeren.

    >[AZURE.NOTE] VMM is geïmplementeerd in een cluster en u installeert de voorziening voor de eerste keer installeren op een actief knooppunt als de installatie om de VMM-server registreren in de kluis te voltooien. Vervolgens installeert u de Provider op de andere knooppunten. Opmerking Als u een de Provider upgrade uitvoert u op alle knooppunten een upgrade moet omdat ze moeten al worden uitgevoerd dezelfde versie van de Provider.

3. Het installatieprogramma heeft een paar **Vereisten vooraf controleren** en aanvragen toestemming de VMM-service om te beginnen met instellingen voor voorziening te stoppen. De VMM-Service worden gestart nadat setup is voltooid. Als u op een servercluster VMM installeert wordt u gevraagd om te stoppen met de rol van het Cluster.

4. In **Microsoft Update** kunt u opt-in voor updates. Met deze instelling ingeschakeld Provider volgens het beleid van Microsoft Update updates worden geïnstalleerd.

    ![Microsoft-Updates](./media/site-recovery-vmm-to-vmm-classic/ms-update.png)

5. De locatie voor de installatie is ingesteld op ** <SystemDrive>\Program Files\Microsoft Manager\bin van System Center 2012 R2\Virtual Machine**. Klik op de knop installeren om te beginnen met het installeren van de Provider.

    ![InstallLocation](./media/site-recovery-vmm-to-vmm-classic/install-location.png)

6. Klik op **registreren** als u de server registreren in de kluis wilt nadat u de voorziening hebt geïnstalleerd.

    ![InstallComplete](./media/site-recovery-vmm-to-vmm-classic/install-complete.png)
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

13.  Klik op **volgende** om het proces te voltooien. Na registratie wordt metagegevens van de VMM-server opgehaald door Azure Site herstellen. De server wordt weergegeven in de **VMM Servers** > -**Servers** in de kluis.

    ![Servers](./media/site-recovery-vmm-to-vmm-classic/provider13.PNG)

### <a name="command-line-installation"></a>Installatie via opdrachtregel

De Provider Azure Site herstel kan ook worden geïnstalleerd vanaf de opdrachtregel. Deze methode kan worden gebruikt om de voorziening hebt geïnstalleerd op een Server CORE van Windows Server 2012 R2.

1. De Provider installatie en registratie sleutel downloaden naar een map. Bijvoorbeeld C:\ASR.
2. De System Center Virtual Machine Manager-Service stoppen
3. Het installatieprogramma van de Provider door deze opdrachten uitvoeren vanaf een opdrachtprompt met **beheerdersbevoegdheden** te pakken.

        C:\Windows\System32> CD C:\ASR
        C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q

4. De provider installeren door te voeren:

        C:\ASR> setupdr.exe /i

5. Registreren van de provider door te voeren:

        CD C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin
        C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin\> DRConfigurator.exe /r  /Friendlyname <friendly name of the server> /Credentials <path of the credentials file> /EncryptionEnabled <full file name to save the encryption certificate>     

Wanneer de parameters zijn:

 - **/Credentials**: verplichte parameter die de locatie waar het sleutelbestand registratie  
 - **/FriendlyName**: verplichte parameter voor de naam van de Hyper-V host-server die wordt weergegeven in de portal Azure Site herstellen.
 - **/EncryptionEnabled**: optionele Parameter die u alleen gebruiken in de VMM Azure scenario wilt als u codering van uw virtuele machines op in rust in Azure. Zorg ervoor dat de naam van het bestand dat u opgeeft, krijgt de extensie **.pfx** .
 - **/proxyAddress**: optionele parameter die het adres van de proxyserver.
 - **/proxyport**: optionele parameter die de poort van de proxyserver.
 - **/proxyUsername**: optionele parameter die de naam Proxy (als de proxy verificatie vereist).
 - **/proxyPassword**: optionele parameter die het wachtwoord voor verificatie met de proxyserver (als de proxy verificatie vereist).  

## <a name="step-4-configure-cloud-protection-settings"></a>Stap 4: Configureer wolk beveiligingsinstellingen

Nadat de VMM servers zijn geregistreerd, kunt u beveiligingsinstellingen wolk. Als u de optie **cloud-gegevens synchroniseren met de kluis** ingeschakeld tijdens de installatie van de Provider worden zodat alle wolken op de VMM-server wordt weergegeven op het tabblad **Beveiligde Items** in de kluis. Als u niet u hebt een specifieke cloud kunt synchroniseren met Azure Site herstellen op het tabblad **Algemeen** van de eigenschappenpagina van wolk in de console van VMM.

![Gepubliceerde Cloud](./media/site-recovery-vmm-to-vmm-classic/clouds-list.png)

1. Klik op **beveiliging van VMM wolken instellen**op de pagina Quick Start.
2. Selecteer op het tabblad **VMM wolken** de wolk die u wilt configureren en Ga naar het tabblad **configuratie** .
3. Selecteer in het **doel**, **VMM**.
4. Selecteer in **doellocatie**, de on-site VMM-server die u beheert de wolk die u wilt gebruiken voor herstel.
4. Selecteer het doel wolk die u wilt gebruiken voor failover van virtuele machines in de cloud bron in **doel cloud**. Houd rekening met:

    - Wij raden aan dat u een doel wolk die voldoet aan de eisen herstel voor de virtuele machines die u kunt beveiligen.
    - Een wolk kan slechts deel uitmaken van een wolk van één paar, als een primaire of een doel-wolk.

5. Opgeven hoe vaak in **frequentie kopiëren**, moeten gegevens worden gesynchroniseerd tussen de bron- en doellocaties 5he. Houd er rekening mee dat deze instelling is alleen van toepassing als de host met Hyper-V Windows Server 2012 R2 wordt uitgevoerd. Een standaardinstelling van vijf minuten wordt gebruikt voor andere servers.
6. In **herstel extra punten**, opgeven of u wilt aanvullende herstel punten maken. De standaardwaarde nul geeft aan dat alleen het meest recente herstelpunt voor een primaire virtuele machine op een hostserver replica is opgeslagen. Houd er rekening mee dat meerdere punten voor Systeemherstel inschakelen extra opslagruimte vereist voor de momentopnamen die zijn opgeslagen op elk moment herstellen. Herstel punten worden elk uur wordt gemaakt zodat elk herstelpunt van een uur lang gegevens bevat. Het herstel waarde in punten die u voor de virtuele machine in de console VMM toewijst moet niet kleiner is dan de waarde die u in de herstelconsole Azure Site toewijzen.
7. In de **frequentie van de toepassing consistente momentopnamen**, opgeven hoe vaak toepassing consistente momentopnamen te maken. Hyper-V worden twee soorten momentopnamen, een momentopname van een standaard waarmee een incrementele momentopname van de hele virtuele machine en een momentopname van een consistente toepassing die een point-in-time momentopname van de toepassingsgegevens in de virtuele machine. Volume Shadow Copy Service (VSS) toepassing consistente momentopnamen gebruiken om ervoor te zorgen dat de toepassingen in een consistente status zijn als de momentopname is gemaakt. Houd er rekening mee dat als u een toepassing consistente momentopnamen inschakelt, geldt dat de prestaties van toepassingen die worden uitgevoerd op de bron virtuele machines. Zorg ervoor dat de ingestelde waarde lager is dan extra herstel punten die u configureert.

    ![Beveiligingsinstellingen configureren](./media/site-recovery-vmm-to-vmm-classic/cloud-settings.png)

8. **Compressie van de gegevensoverdracht**, opgeven of gerepliceerde gegevens die wordt overgedragen moet worden gecomprimeerd.
9. Bij **verificatie**opgeven hoe verkeer tussen de primaire en herstel Hyper-V host-servers wordt geverifieerd. Selecteer HTTPS als u beschikt over een werkende Kerberos-omgeving is geconfigureerd. Azure-Site-Recovery configureert automatisch certificaten voor HTTPS-verificatie. Er is geen handmatige configuratie vereist. Als u Kerberos selecteert, wordt een Kerberos-ticket gebruikt voor wederzijdse verificatie van de hostservers. Poort 8083 en 8084 (voor certificaten) wordt standaard geopend in Windows Firewall op de host-servers Hyper-V. Opmerking deze instelling is alleen relevant voor Hyper-V host-servers waarop Windows Server 2012 R2.
10. Wijzigen in de **haven**, het nummer van de poort waarop de bron- en host-computers voor het verkeer luisteren. U zou bijvoorbeeld de instelling wijzigen als u wilt toepassen, Quality of Service (QoS) netwerk bandbreedtebeperking voor replicatieverkeer. Controleer of de poort niet wordt gebruikt door een andere toepassing en het is geopend in de firewall-instellingen.
11. Opgeven hoe de eerste replicatie van gegevens van de bron naar de doellocatie worden verwerkt voordat de normale replicatie wordt bij **replicatiemethode**:

    - **Via het netwerk**, het kopiëren van gegevens via het netwerk kan worden tijdrovende en bronintensieve. Wij raden aan dat u deze optie gebruiken als de wolk virtuele machines met relatief kleine virtuele harde schijven bevat en als de primaire site met de secundaire site is verbonden via een verbinding met een grote bandbreedte. U kunt opgeven dat de kopie moet onmiddellijk beginnen of Selecteer een tijd. Als u replicatie netwerk gebruikt, kunt u het beste plannen tijdens rustige uren.
    - **Offline**— met deze methode geeft aan dat de eerste replicatie wordt uitgevoerd met behulp van externe media. Het is handig als u wilt voorkomen dat de afbraak in de prestaties van het netwerk, of voor de geografisch externe locaties. Deze methode kunt u de exportlocatie van de wolk bron en de locatie van het importeren van de wolk doel opgeven. Wanneer u de beveiliging voor een virtuele machine inschakelt, wordt de virtuele harde schijf gekopieerd naar de locatie van de opgegeven exporteren. U verzenden naar de doelsite en kopiëren naar de locatie van de import. De geïmporteerde gegevens opgehaald met de replica virtuele machines.

12. **Replica virtuele Machine verwijderen** om op te geven dat de virtuele machine van replica worden verwijderd als u de virtuele machine te beveiligen door de optie **bescherming voor de virtuele machine te verwijderen** op het tabblad virtuele Machines van de wolk eigenschappen selecteren. Met deze instelling is ingeschakeld, wanneer u de beveiliging uitschakelen de virtuele machine wordt verwijderd uit de Azure Site herstellen, het herstel van de Site-instellingen voor de virtuele machine worden verwijderd in de VMM-console en de replica wordt verwijderd.

    ![Beveiligingsinstellingen configureren](./media/site-recovery-vmm-to-vmm-classic/cloud-settings-replica.png)

Nadat u de instellingen van een taak wordt gemaakt en kan worden gecontroleerd op het tabblad **taken** opslaan. Alle Hyper-V host-servers in de cloud VMM bron wordt geconfigureerd voor replicatie. Cloud-instellingen kunnen worden gewijzigd op het tabblad **configureren** . Als u wilt wijzigen van de wolk van locatie of het doel doel moet u verwijderen van de configuratie van de wolk en de cloud configureren.

### <a name="prepare-for-offline-initial-replication"></a>Voorbereiden voor off line initiële replicatie

U moet de volgende acties voor te bereiden voor de eerste replicatie off line uitvoeren:

- Op de bronserver, moet u een locatie waar het exporteren van gegevens zal plaatsvinden. Volledig beheer toewijzen voor NTFS- en Share-machtigingen de VMM-service het exportpad. Op de doelserver, zult u een padlocatie van waaruit het importeren van gegevens wordt uitgevoerd. Op dit Importpad dezelfde machtigingen toewijzen.
- Als het pad of uitvoer wordt gedeeld, toewijzen Administrator, gebruiker, Printeroperator of serveroperator groepslidmaatschap voor de VMM-serviceaccount op de externe computer waarop de gedeelde zich bevindt.
- Als u geen accounts uitvoeren als hosts toevoegen op de paden importeren en exporteren, toewijzen lezen en schrijven naar de rekeningen uitvoeren als in de VMM.
- Importeren en exporteren van aandelen zich niet bevinden op elke computer die wordt gebruikt als een Hyper-V host-server omdat de loopback-configuratie wordt niet ondersteund door Hyper-V.
- In Active Directory beperkt host-server met virtuele machines die u wilt beveiligen, inschakelen en configureren op elke Hyper-V overdracht om het vertrouwen van de externe computers waarop de paden importeren en exporteren, als volgt bevinden:
    1. Open **Active Directory: gebruikers en Computers**op de domeincontroller.
    2. Klik in de consolestructuur op **domeinnaam** > **Computers**.
    3. Klik met de rechtermuisknop op de naam van de Hyper-V host > **Eigenschappen**.
    4. Klik op het tabblad **overdracht** op T**deze computer mag overdragen aan de opgegeven services alleen roest**.
    5. Klik op **elk protocol voor verificatie gebruiken**.
    6. Klik op **toevoegen** > **-gebruikers en Computers**.
    7. Typ de naam van de computer waarop het exportpad > **OK**. In de lijst met beschikbare services, houdt u CTRL ingedrukt en klik op **cifs** > **OK**. Herhaal dit voor de naam van de computer waarop het importpad. Herhaal deze stappen voor de extra host-servers Hyper-V.

## <a name="step-5-configure-network-mapping"></a>Stap 5: Toewijzing van netwerk configureren
1. Klik op de pagina Quick Start **netwerken toewijzen**.
2. Selecteer de bron VMM-server die u wilt toewijzen van netwerken en de doelserver VMM waaraan de netwerken worden toegewezen. De lijst van bron-netwerken en hun bijbehorende doel-netwerken worden weergegeven. Een lege waarde weergegeven voor de netwerken die momenteel niet zijn toegewezen.
3. Selecteer een netwerk in een **netwerk op bron** > **kaart**. De service op de doelserver de VM-netwerken gedetecteerd en weergegeven. Klik op het informatiepictogram bij de bron- en netwerknamen weergeven van de subnetten voor elk netwerk.

    ![Toewijzing van netwerk configureren](./media/site-recovery-vmm-to-vmm-classic/network-mapping1.png)

4. Selecteer in het dialoogvenster een van de VM-netwerken van de doelserver VMM.

    ![Selecteer een netwerk](./media/site-recovery-vmm-to-vmm-classic/network-mapping2.png)

5. Wanneer u een netwerk selecteert, worden de beschermde wolken die gebruik maakt van het Bronnetwerk weergegeven. Beschikbare doel-netwerken die gekoppeld aan de wolken die wordt gebruikt voor beveiliging zijn worden ook weergegeven. Wij raden aan dat u een netwerk dat beschikbaar is voor de wolken die u voor bescherming gebruikt. Of u kunt ook Ga naar de VMM-Server en de cloud eigenschappen wijzigen om de logische netwerk overeenkomt met de vm-netwerk dat u wilt toevoegen.
6. Klik op het selectievakje om de toewijzing te voltooien. Een taak wordt gestart voor het bijhouden van de voortgang van de toewijzing. U kunt deze bekijken op het tabblad **taken** .


## <a name="step-6-configure-storage-mapping"></a>Stap 6: Opslag toewijzing configureren
Standaard wanneer u een virtuele machine op een bron Hyper-V host-server naar een Hyper-V host doelserver repliceert, gerepliceerde gegevens opgeslagen op de standaardlocatie die wordt aangegeven voor de doelhost Hyper-V in Hyper-V-beheer. Voor meer controle over welke gerepliceerde gegevens zijn opgeslagen, kunt u de toewijzingen van bestanden als volgt:



1. Classificaties voor opslag op servers van de bron- en doel-VMM definiëren. [Meer informatie](https://technet.microsoft.com/library/gg610685.aspx). Classificaties moeten beschikbaar zijn voor de host-servers Hyper-V in bron- en wolken. Classificaties hoeft niet hetzelfde soort opslag hebben. U kunt bijvoorbeeld een bron classificatie met SMB-shares voor een doel-classificatie met CSVs toewijzen.
2. Nadat classificaties aanwezig zijn, kunt u toewijzingen maken. Om dit te doen, klik op de pagina **Quick Start** > **opslag toewijzen**.
3. Klik op het tabblad **opslag** > **opslag classificaties toewijzen**.
4. Op het tabblad **opslag classificaties toewijzen** , selecteert u classificaties op de bron en VMM doelservers. Sla uw instellingen.

    ![Selecteer een netwerk](./media/site-recovery-vmm-to-vmm-classic/storage-mapping.png)


## <a name="step-7-enable-virtual-machine-protection"></a>Stap 7: De virtuele machine beveiliging inschakelen
Nadat de servers, wolken en netwerken goed zijn geconfigureerd, kunt u de beveiliging voor virtuele machines in de cloud.

1. Klik op **Beveiliging inschakelen**op het tabblad **virtuele Machines** in de cloud waar de virtuele machine gevestigd is, > **virtuele machines toevoegen**.
2. Selecteer in de lijst van virtuele machines in de cloud, die u wilt beveiligen.

    ![Virtuele machine beveiliging inschakelen](./media/site-recovery-vmm-to-vmm-classic/enable-protection.png)

3. De voortgang van de actie beveiliging inschakelen op het tabblad **taken** , met inbegrip van de aanvankelijke replicatie bijhouden. Nadat de taak voltooien bescherming wordt uitgevoerd is de virtuele machine gereed voor failover. Nadat de beveiliging is ingeschakeld en virtuele machines worden gerepliceerd, zult u kunnen ze bekijken in Azure.

    ![Virtuele machine bescherming taak](./media/site-recovery-vmm-to-vmm-classic/vm-jobs.png)

>[AZURE.NOTE] U kunt ook beveiliging voor virtuele machines in de VMM console inschakelen. Klik op **Beveiliging inschakelen** op de werkbalk op het tabblad **Azure Site herstellen** in de eigenschappen van de virtuele machine.

### <a name="on-board-existing-virtual-machines"></a>On-board bestaande virtuele machines

Als u een bestaande virtuele machines in de VMM die met Hyper-V-Replica repliceert hebt moet u on-board ze voor bescherming door Azure Site als volgt:

1. Controleer of u primaire en secundaire wolken. Zorg ervoor dat de Hyper-V server die als host fungeert voor de bestaande virtuele machine bevindt zich in de primaire cloud en dat de Hyper-V server die als host fungeert voor de virtuele machine van replica bevindt zich in de secundaire wolk. Zorg ervoor dat u de beveiligingsinstellingen voor de wolken hebt geconfigureerd. De instellingen moeten overeenkomen met die momenteel zijn geconfigureerd voor Hyper-V-Replica. Anders virtuele machine-replicatie werkt mogelijk niet zoals verwacht.
2. Vervolgens de beveiliging voor de primaire virtuele machine inschakelen. Azure Site herstellen en VMM zal ervoor zorgen dat de host van dezelfde replica en de virtuele machine wordt gedetecteerd en Azure Site herstel zal gebruiken en herstellen met behulp van de instellingen die zijn geconfigureerd tijdens de configuratie van de wolk.


## <a name="test-your-deployment"></a>De implementatie testen

Voor het testen van de implementatie kunt u een failover testen voor een enkele virtuele machine, uitvoeren of een herstelplan dat bestaat uit meerdere virtuele machines maken en uitvoeren van failover test voor het plan.  Failover van de test simuleert de failover- en herstel mechanisme in een geïsoleerd netwerk.

### <a name="create-a-recovery-plan"></a>Maak een herstelplan

1. Klik op het tabblad **Systeemherstel plannen** **Herstelplan maken**.
2. Geef een naam voor het herstelplan en de bron- en doeldomein VMM-servers. De bronserver hebt virtuele machines die zijn ingeschakeld voor failover en herstel. Schakel de **Hyper-V** te bekijken alleen de wolken die zijn geconfigureerd voor de replicatie van Hyper-V.

    ![Herstelplan maken](./media/site-recovery-vmm-to-vmm-classic/recovery-plan1.png)

3. Selecteer in **Selecteer virtuele Machine**, replicatiegroepen. Alle virtuele machines gekoppeld aan de replicatiegroep worden geselecteerd en toegevoegd aan het herstelplan. Deze virtuele machines worden toegevoegd aan de standaardgroep recovery plan, groep 1. u kunt meer groepen toevoegen als nodig is. Houd er rekening mee dat nadat de replicatie virtuele machines wordt gestart volgens de volgorde van de groepen recovery plan.

    ![Virtuele machines toevoegen](./media/site-recovery-vmm-to-vmm-classic/recovery-plan2.png)

Na een recovery is plan gemaakt, wordt deze weergegeven in de lijst op het tabblad **Systeemherstel plannen** .

###<a name="run-a-test-failover"></a>Een failover test uitvoeren

1. Klik op het tabblad **Systeemherstel plannen** het plan en klik op **Failover testen**.
2. Selecteer op de pagina **Bevestigen Test Failover** **geen**. Opmerking: met deze optie ingeschakeld in de mislukte plaats replica virtuele machines wordt niet aangesloten op een netwerk. Hierdoor wordt de virtuele machine wordt overgenomen, zoals verwacht, maar test uw netwerkomgeving replicatie niet getest. Bekijk [een test failover uitvoeren](site-recovery-failover.md#run-a-test-failover) voor meer informatie over het gebruik van verschillende netwerkopties.
3. De test virtuele machine wordt gemaakt op dezelfde host als de host waarop de replica virtuele machine bestaat. Deze is toegevoegd aan de dezelfde wolk waarin de replica virtuele machine bevindt.

### <a name="run-a-recovery-plan"></a>Uitvoeren van een herstelplan
De replica virtuele machine mogelijk na replicatie geen IP-adres is hetzelfde als het IP-adres van de primaire virtuele machine. Virtuele machines werkt u de DNS-server die ze gebruiken wanneer ze worden gestart. U kunt ook een script voor het bijwerken van de DNS-Server om ervoor te zorgen een tijdige update toevoegen.

#### <a name="script-to-retrieve-the-ip-address"></a>Script voor het ophalen van het IP-adres
Dit voorbeeldscript voor het ophalen van het IP-adres wordt uitgevoerd.

        $vm = Get-SCVirtualMachine -Name <VM_NAME>
        $na = $vm[0].VirtualNetworkAdapters>
        $ip = Get-SCIPAddress -GrantToObjectID $na[0].id
        $ip.address  

#### <a name="script-to-update-dns"></a>Script voor het bijwerken van DNS
Dit voorbeeldscript wilt bijwerken, DNS, opgeven van het IP-adres dat u opgehaald met behulp van het voorbeeldscript uitvoeren.

        string]$Zone,
        [string]$name,
        [string]$IP
        )
        $Record = Get-DnsServerResourceRecord -ZoneName $zone -Name $name
        $newrecord = $record.clone()
        $newrecord.RecordData[0].IPv4Address  =  $IP
        Set-DnsServerResourceRecord -zonename $zone -OldInputObject $record -NewInputObject $Newrecord



## <a name="privacy-information-for-site-recovery"></a>Informatie over de privacy voor herstel van de Site

Deze sectie bevat aanvullende informatie over privacy voor de Microsoft Azure Site Recovery service ("Service"). Zie de [Privacyverklaring van Microsoft Azure](http://go.microsoft.com/fwlink/?LinkId=324899) om de privacyverklaring voor Microsoft Azure services

**Functie: registratie**

- **Wat het doet**: server met service registreert zodat virtuele machines kunnen worden beveiligd.
- **Informatie verzameld**: na het registreren van de Service worden verzameld, verwerkt en verzendt de certificaatgegevens management van de VMM-server die is aangewezen om te bieden met behulp van de naam van de VMM-server en de naam van de virtuele machine wolken op uw server VMM noodherstel.
- **Het gebruik van gegevens**:
    - Certificaat beheer, dit wordt gebruikt om vast te stellen en de geregistreerde VMM-server voor toegang tot de Service te verifiëren. Het openbare sleutelgedeelte van het certificaat wordt gebruikt voor het beveiligen van een token dat de geregistreerde VMM server toegang krijgt tot de Service. De server moet deze token toegang te krijgen tot de functies van de Service te gebruiken.
    - Naam van de server VMM — de naam van de VMM is vereist voor het identificeren en communiceren met de juiste VMM-server waarop de wolken zich bevinden.
    - Namen van de server VMM cloud, cloud-naam is vereist als u de Service cloud functie combinatie/unpairing die hieronder worden beschreven. Wanneer u uw wolk van een primaire datacenter met een andere wolk in het midden van de data recovery pair besluit, worden de namen van de wolken van het recovery Datacenter gepresenteerd.

- **Keuze**: deze informatie is een essentieel onderdeel van het registratieproces Service omdat het helpt u en de Service de VMM-server waarvoor u Azure Site herstellen om bescherming te bieden, alsmede over het identificeren van de juiste server voor geregistreerde VMM wilt identificeren. Als u niet dat deze informatie naar de Service verzenden wilt, gebruikt u deze Service niet. Als u de server registreren en vervolgens later wilt deze registratie, kunt u dit doen door de VMM-serverinformatie verwijderen uit de serviceportal (dit is de portal voor Azure).

**Functie: Azure Site herstel beveiliging inschakelen**

- **Wat het doet**: de Azure herstel siteprovider geïnstalleerd op de server VMM is het kanaal voor communicatie met de Service. De Provider is een dynamic link library (DLL) die worden gehost in het proces van VMM. Nadat u de voorziening hebt geïnstalleerd, wordt de functie 'Herstel Datacenter' ingeschakeld in de beheerdersconsole van VMM. Een nieuwe of bestaande virtuele machines in een wolk kan een eigenschap met de naam 'Datacenter Recovery' ter bescherming van de virtuele machine kunt inschakelen. Als deze eigenschap is ingesteld, verzendt de Provider de naam en ID van de virtuele machine voor de Service. De virtuele beveiliging is ingeschakeld op Windows Server 2012- of Windows Server 2012 R2 Hyper-V technologie voor replicatie. De virtuele machine-gegevens worden gerepliceerd van de ene Hyper-V host naar de andere (meestal te vinden in een datacenter verschillende "recovery").

- **Verzamelde gegevens**: de Service worden verzameld, verwerkt en verzendt de metagegevens voor de virtuele machine met de naam, ID, virtueel netwerk en de naam van de wolk waartoe het behoort.

- **Gebruik van informatie**: de Service maakt gebruik van de bovenstaande informatie voor het vullen van de gegevens van de virtuele machine op uw serviceportal.

- **Keuze**: dit is een essentieel onderdeel van de service en kan niet worden uitgeschakeld. Als u niet dat deze gegevens worden verzonden naar de Service wilt niet inschakelen Azure Site bescherming voor alle virtuele machines. Houd er rekening mee dat alle gegevens die door de Provider aan de Service worden verzonden via HTTPS wordt verzonden.

**Functie: Herstelplan**

- **Wat het doet**: met deze functie kunt u een orchestration plan voor de 'recovery' datacenter bouwen. U kunt de volgorde waarin de virtuele machines of een groep van virtuele machines moet worden gestart op de herstel-site definiëren. Ook kunt u een geautomatiseerde scripts worden uitgevoerd of een handmatige actie moet worden ondernomen, op het moment van herstel voor elke virtuele machine. Failover (behandeld in de volgende sectie) wordt doorgaans geactiveerd op het niveau van herstelplan voor gecoördineerde herstel.

- **Verzamelde gegevens**: de Service worden verzameld, verwerkt en metagegevens voor het herstelplan, met inbegrip van de metagegevens van de virtuele machine en metagegevens van een automatische scripts en handmatige actie notities worden verzonden.

- **Gebruik van informatie**: de hierboven beschreven metagegevens maken het herstelplan in uw serviceportal wordt gebruikt.

- **Keuze**: dit is een essentieel onderdeel van de service en kan niet worden uitgeschakeld. Als u niet dat deze gegevens worden verzonden naar de Service wilt niet bouwen Recovery plan in deze Service.

**Functie: Netwerk toewijzen**

- **Wat het doet**: met deze functie kunt u netwerkgegevens te herstellen vanuit het midden van de primaire gegevens toewijzen. Wanneer de virtuele machines worden hersteld op de site van herstel, helpt deze toewijzing in verbinding met het netwerk tot stand te brengen voor hen.

- **Verzamelde informatie**: als onderdeel van de functie voor het netwerk, de Service worden verzameld, verwerkt, en verstuurt de metagegevens van de logische netwerken voor elke site (primaire en datacenter).

- **Gebruik van informatie**: de Service gebruikt de metagegevens voor het vullen van uw serviceportal waar u de netwerkgegevens kunt toewijzen.

- **Keuze**: dit is een essentieel onderdeel van de Service en kan niet worden uitgeschakeld. Als u niet dat deze informatie naar de Service verzonden wilt, hoeft u de functie voor het netwerk.

**Functie: Failover - test geplande, ongeplande,**

- **Wat het doet**: deze functie helpt failover van een virtuele machine van een beheerde VMM Datacenter naar een ander beheerd gegevenscentrum VMM. De failover-actie wordt gestart door de gebruiker op de portal van hun Service. Mogelijke oorzaken voor een failover-zijn een niet-geplande gebeurtenis (bijvoorbeeld in geval van een natuurlijke disaster0; een geplande gebeurtenis (bijvoorbeeld datacenter load balancing), een test-failover (bijvoorbeeld een recovery plan try-out).

De Provider op de server VMM wordt in kennis gesteld van de gebeurtenis van de Service en een failover-actie wordt uitgevoerd op de Hyper-V host via VMM-interfaces. Werkelijke failover van de virtuele machine van de ene Hyper-V host naar de andere (meestal uitgevoerd in een datacenter verschillende "recovery") wordt uitgevoerd door de Windows Server 2012- of Windows Server 2012 R2 Hyper-V-technologie voor replicatie. Nadat de failover uitgevoerd is, stuurt de Provider is geïnstalleerd op de server VMM van het datacenter "recovery" succes informatie aan de Service.

- **Verzamelde gegevens**: de Service maakt gebruik van de bovenstaande informatie voor het vullen van de status van de failover-actie-informatie op uw serviceportal.

- **Gebruik van informatie**: de Service gebruikt de bovenbedoelde gegevens als volgt:

    - Certificaat beheer, dit wordt gebruikt om vast te stellen en de geregistreerde VMM-server voor toegang tot de Service te verifiëren. Het openbare sleutelgedeelte van het certificaat wordt gebruikt voor het beveiligen van een token dat de geregistreerde VMM server toegang krijgt tot de Service. De server moet deze token toegang te krijgen tot de functies van de Service te gebruiken.
    - Naam van de server VMM — de naam van de VMM is vereist voor het identificeren en communiceren met de juiste VMM-server waarop de wolken zich bevinden.
    - Namen van de server VMM cloud, cloud-naam is vereist als u de Service cloud functie combinatie/unpairing die hieronder worden beschreven. Wanneer u uw wolk van een primaire datacenter met een andere wolk in het midden van de data recovery pair besluit, worden de namen van de wolken van het recovery Datacenter gepresenteerd.

- **Keuze**: dit is een essentieel onderdeel van de service en kan niet worden uitgeschakeld. Als u niet dat deze informatie naar de Service verzonden wilt, hoeft u deze Service.

## <a name="next-steps"></a>Volgende stappen

U hebt na een failover testen om te controleren dat uw omgeving werkt zoals verwacht, [informatie over](site-recovery-failover.md) verschillende soorten failover uitgevoerd.
