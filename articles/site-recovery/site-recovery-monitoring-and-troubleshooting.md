<properties
    pageTitle="Bewaken en problemen oplossen met de beveiliging voor virtuele machines en fysieke servers | Microsoft Auzre" 
    description="Azure-Site-Recovery coördineert de replicatie, failover en herstel van virtuele machines die zich op de servers voor bedrijfsruimten Azure of een secundaire datacenter. In dit artikel gebruiken om te controleren en problemen oplossen met de beveiliging VMM of Hyper-V-Site." 
    services="site-recovery" 
    documentationCenter="" 
    authors="anbacker" 
    manager="mkjain" 
    editor=""/>

<tags 
    ms.service="site-recovery" 
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery" 
    ms.date="10/13/2016"    
    ms.author="rajanaki"/>
    
# <a name="monitor-and-troubleshoot-protection-for-virtual-machines-and-physical-servers"></a>Bewaken en problemen oplossen met de beveiliging voor virtuele machines en fysieke servers

Deze Monitoring and Troubleshooting Guide kunt u meer informatie over het bijhouden van de status van de replicatie en het oplossen van technieken voor nuttige toepassing Azure-Site.

## <a name="understanding-the-components"></a>Wat zijn de onderdelen

### <a name="vmwarephysical-site-deployment-for-replication-between-on-premises-and-azure"></a>VMware/fysieke implementatie voor replicatie tussen lokale en Azure.
Instellen DR tussen voor bedrijfsruimten VMware/fysieke machine; Configuratieserver, Target model en Process Server moet geconfigureerd. Tijdens de bescherming van de bronserver installeert Azure Site herstel mobiliteit service. Boeken op ruimten stroomstoring zodra de bronserver mislukt op Azure, behoeften van klanten voor het instellen van een Server Process in Azure en het doel van een Master server op-lokalen ter bescherming van de bronserver naar gegenereerde op gebouwen. 

![VMware/fysieke implementatie voor replicatie tussen voor bedrijfsruimten & Azure](media/site-recovery-monitoring-and-troubleshooting/image18.png)

### <a name="vmm-site-deployment-for-replication-between-on-premises-site"></a>VMM website-implementatie voor replicatie tussen site voor gebouwen.

Als onderdeel van het instellen van DR tussen twee op lokalen locatie; Azure-Site-Recovery Provider moet worden gedownload en geïnstalleerd op de server van VMM. Provider moet verbinding met internet om ervoor te zorgen dat alle bewerkingen die vanaf de Portal Azure geactiveerd wordt vertaald voor bedrijfsruimten bewerkingen zoals beveiliging, afsluiten primaire virtuele machines als onderdeel van de failover, enz.

![Website-implementatie VMM voor replicatie tussen site voor gebouwen](media/site-recovery-monitoring-and-troubleshooting/image1.png)

### <a name="vmm-site-deployment-for-replication-between-on-premises--azure"></a>Voor replicatie tussen voor bedrijfsruimten & Azure VMM website-implementatie.

Als onderdeel van het instellen van DR tussen voor bedrijfsruimten & Azure; Azure-Site-Recovery Provider moet worden gedownload en geïnstalleerd op de server VMM en Azure Services herstelagent die moet worden geïnstalleerd op elke host Hyper-V. Raadpleeg [Wat bescherming Azure-Site](./site-recovery-understanding-site-to-azure-protection.md) voor meer informatie.

![Website-implementatie voor replicatie tussen voor bedrijfsruimten & Azure VMM](media/site-recovery-monitoring-and-troubleshooting/image2.png)

### <a name="hyper-v-site-deployment-for-replication-between-on-premises--azure"></a>Implementatie Hyper-V voor replicatie tussen voor bedrijfsruimten & Azure

Dit is dezelfde als die van VMM implementatie – alleen verschil Provider & Agent wordt geïnstalleerd op de Hyper-V host zelf. Raadpleeg [Wat bescherming Azure-Site](./site-recovery-understanding-site-to-azure-protection.md) voor meer informatie.

## <a name="monitor-configuration-protection-and-recovery-operations"></a>Controleren van de configuratie, beveiliging en herstelbewerkingen

Alle bewerkingen van ASR wordt gecontroleerd en wordt bijgehouden op het tabblad 'Projecten'. Ga naar het tabblad taken en zien of er fouten in het geval van een configuratie, bescherming of herstel fout.

![Controleren van de configuratie, beveiliging en herstelbewerkingen](media/site-recovery-monitoring-and-troubleshooting/image3.png)

Als u fouten optreden bij het weergeven van de taken hebt gevonden, selecteert u de taak en klikt u op FOUTGEGEVENS voor die taak.

![Controleren van de configuratie, beveiliging en herstelbewerkingen](media/site-recovery-monitoring-and-troubleshooting/image4.png)

De details van deze fout kunt u mogelijke oorzaak en de aanbeveling voor het probleem te identificeren.

![Controleren van de configuratie, beveiliging en herstelbewerkingen](media/site-recovery-monitoring-and-troubleshooting/image5.png)

In het bovenstaande geval lijkt er een andere bewerking die wordt uitgevoerd door die bescherming configuratie is mislukt. Zorg ervoor dat u het probleem aan de hand van de aanbeveling – er na Klik op RESART om opnieuw de oplossing.

![Controleren van de configuratie, beveiliging en herstelbewerkingen](media/site-recovery-monitoring-and-troubleshooting/image6.png)

Optie voor opnieuw opstarten is niet beschikbaar voor alle bewerkingen: voor degenen die niet de optie opnieuw terug te keren naar het object en de bewerking weer opnieuw. Elke taak kan worden geannuleerd op elk tijdstip tijdens de voortgang met de knop Annuleren.

![Controleren van de configuratie, beveiliging en herstelbewerkingen](media/site-recovery-monitoring-and-troubleshooting/image7.png)

## <a name="monitor-replication-health-for-virtual-machine"></a>Status van de replicatie voor virtuele machine controleren

ASR provider centraal & externe controle via de Portal Azure voor elk van de beschermde diensten. Ga naar de beveiligde ITEMS er na VMM WOLKEN of bescherming groepen. Tabblad VMM WOLKEN is alleen voor implementaties op basis van VMM en andere scenario's hebben de beschermde entiteiten onder het tabblad Beveiliging groepen.

![Status van de replicatie voor virtuele machine controleren](media/site-recovery-monitoring-and-troubleshooting/image8.png)

Er zijn na selecteren de beschermde dienst onder de respectieve cloud of de groep bescherming. Nadat u de beveiligde entiteit alle toegestane bewerkingen in het onderste deelvenster weergegeven.

![Status van de replicatie voor virtuele machine controleren](media/site-recovery-monitoring-and-troubleshooting/image9.png)

Zoals hierboven in geval de virtuele machine dat gezondheid is van cruciaal belang – kunt u de knop FOUTDETAILS aan de onderzijde voor een overzicht van de fout. Op basis van de "oorzaken" en "Aanbeveling" genoemd, het probleem opgelost.

![Status van de replicatie voor virtuele machine controleren](media/site-recovery-monitoring-and-troubleshooting/image10.png)

![Status van de replicatie voor virtuele machine controleren](media/site-recovery-monitoring-and-troubleshooting/image11.png)

Opmerking: Als er actieve bewerkingen die in uitvoering of mislukte Ga naar de weergave taken zoals eerder vermeld de taak-specifieke fout.

## <a name="troubleshoot-on-premises-hyper-v-issues"></a>Problemen met Hyper-V in gebouwen

Verbinding maken met de console op ruimten Hyper-V manager, selecteert u de virtuele machine en de gezondheid van de replicatie Zie.

![Problemen met Hyper-V in gebouwen](media/site-recovery-monitoring-and-troubleshooting/image12.png)

In dit geval wordt *Replicatie gezondheid* vermeld als kritiek – *Weergave replicatie gezondheid* om de details te bekijken.

![Problemen met Hyper-V in gebouwen](media/site-recovery-monitoring-and-troubleshooting/image13.png)

Gevallen waarin replicatie voor de virtuele machine is onderbroken, klik met de rechtermuisknop Selecteer *replicatie*->*cv replicatie*
![probleem oplossen op-premises Hyper-V-problemen](media/site-recovery-monitoring-and-troubleshooting/image19.png)

Als de virtuele machine worden gemigreerd van een nieuwe Hyper-V host (binnen het cluster of een zelfstandige computer), die via automatisch Systeemherstel is geconfigureerd, wouldn't negatieve gevolgen hebben voor replicatie voor de virtuele machine. Zorg ervoor dat de nieuwe Hyper-V host voldoet aan alle de per-materiaal en is geconfigureerd met behulp van automatisch Systeemherstel.

### <a name="event-log"></a>Gebeurtenislogboek

| Bronnen van gebeurtenissen                | Details                                                                                                                                                                                           |
|-------------------------  |:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------    |
| **Toepassingen en Service Logs/Microsoft/VirtualMachineManager/Server/Admin** (VMM Server)   |  Dit biedt nuttige vastleggen voor het oplossen van problemen met veel verschillende VMM. |
| **Toepassingen en Service MicrosoftAzureRecoveryServices/logboeken/replicatie** (Hyper-V Host)   | Dit biedt nuttige vastleggen voor het oplossen van problemen met veel Microsoft Azure Services herstelagent. <br/> ![Bron van gebeurtenis voor Hyper-V host](media/site-recovery-monitoring-and-troubleshooting/eventviewer03.png) |
| **Toepassingen en Service Microsoft-logboeken/Azure Site herstellen/Provider/operationele** (Hyper-V Host)   | Dit biedt handig vastleggen voor het oplossen van problemen met veel Microsoft Azure Site Recovery-Service. <br/> ![Bron van gebeurtenis voor Hyper-V host](media/site-recovery-monitoring-and-troubleshooting/eventviewer02.png) |
| **Toepassingen en Service Logs/Microsoft/Windows/Hyper-V-VMMS/Admin** (Hyper-V Host) | Dit biedt nuttige vastleggen voor het oplossen van veel problemen met Hyper-V virtuele machine management. <br/> ![Bron van gebeurtenis voor Hyper-V host](media/site-recovery-monitoring-and-troubleshooting/eventviewer01.png) |


### <a name="hyper-v-replication-logging-options"></a>Opties voor logboekregistratie van Hyper-V-replicatie

Alle gebeurtenissen die betrekking hebben op Hyper-V-Replica worden vastgelegd in de Hyper-V-VMMS\\vindt u onder logboek **Logboeken toepassingen en Services\\Microsoft\\Windows**. Bovendien kan een analytische logboek voor Hyper-V-VMMS worden ingeschakeld. Zodat dit logboek eerst maken de logboeken voor analysen en foutopsporing weergeven in Logboeken. Open Logboeken, klik in het **menu Beeld**, klikt u op **weergeven analytisch en logboeken voor foutopsporing**.

![Problemen met Hyper-V in gebouwen](media/site-recovery-monitoring-and-troubleshooting/image14.png)

Er is een analytische logboek zichtbaar onder Hyper-V-VMMS

![Problemen met Hyper-V in gebouwen](media/site-recovery-monitoring-and-troubleshooting/image15.png)

Klik in het deelvenster **Acties** op **Logboek inschakelen**. Eenmaal is ingeschakeld, deze wordt weergegeven in **Prestatiemeter** zoals een gebeurtenistraceersessie zich onder **gegevensverzamelaarsets.**

![Problemen met Hyper-V in gebouwen](media/site-recovery-monitoring-and-troubleshooting/image16.png)

Als u de informatie verzameld, eerst de traceringssessie stoppen door het logboek uit te schakelen en het logboek opslaan en opnieuw openen in Logboeken of andere hulpprogramma's gebruiken om deze te converteren naar wens.



## <a name="reaching-out-for-microsoft-support"></a>Voor Microsoft Support brengen

### <a name="log-collection"></a>Logboek-collectie

Zie [ASR verzamelen Support Diagnostics Platform (SDP) gebruikt](http://social.technet.microsoft.com/wiki/contents/articles/28198.asr-data-collection-and-analysis-using-the-vmm-support-diagnostics-platform-sdp-tool.aspx) voor het verzamelen van de vereiste logboeken voor Site VMM-bescherming.

Download het [hulpprogramma](https://dcupload.microsoft.com/tools/win7files/DIAG_ASRHyperV_global.DiagCab) & uitvoeren op de Hyper-V host voor het verzamelen van de logboeken voor bescherming van Hyper-V-Site.

Raadpleeg [Azure herstel logboek siteverzameling voor VMware en fysieke site bescherming](http://social.technet.microsoft.com/wiki/contents/articles/30677.azure-site-recovery-log-collection-for-vmware-and-physical-site-protection.aspx) voor het verzamelen van de vereiste logboeken voor VMware/fysieke scenario's.

De logboekbestanden lokaal in een willekeurig benoemde submap onder **%LocalAppData%\ElevatedDiagnostics** worden verzameld

![Voorbeeld van de stappen van bescherming van Hyper-V-site weergegeven.](media/site-recovery-monitoring-and-troubleshooting/animate01.gif)

### <a name="opening-a-support-ticket"></a>U opent een support ticket

Wilt verhogen support ticket voor automatisch Systeemherstel, bereiken Azure ondersteunen met behulp van de URL op <http://aka.ms/getazuresupport>

## <a name="kb-articles"></a>KB-artikelen

-   [Het behouden van de stationsaanduiding voor beveiligde virtuele machines die worden overgenomen of gemigreerd naar Azure](http://support.microsoft.com/kb/3031135)
-   [Het beheren van op-pand aan het verbruik van de netwerkbandbreedte Azure bescherming](https://support.microsoft.com/kb/3056159)
-   [ASR: "de clusterbron kan niet worden gevonden" Fout bij het inschakelen voor een virtuele machine](http://support.microsoft.com/kb/3010979)
-   [Begrijpen en oplossen van problemen met Hyper-V-Replica gids](http://www.microsoft.com/en-in/download/details.aspx?id=29016) 

## <a name="common-asr-errors-and-their-resolutions"></a>Veelvoorkomende fouten in ASR en hun oplossingen

Hieronder vindt u veelvoorkomende fouten die u mogelijk raakt en de resolutie. Elk van de fout wordt beschreven in een aparte wikipagina.

### <a name="general"></a>Algemeen
-   <span style="color:green;">Nieuw</span> [Taken mislukken met het foutbericht "een bewerking wordt uitgevoerd." Fout 505, 514, 532](http://social.technet.microsoft.com/wiki/contents/articles/32190.azure-site-recovery-jobs-failing-with-error-an-operation-is-in-progress-error-505-514-532.aspx)
-   <span style="color:green;">Nieuw</span> [Taken mislukken met het foutbericht 'Server niet is verbonden met het Internet'. Fout 25018](http://social.technet.microsoft.com/wiki/contents/articles/32192.azure-site-recovery-jobs-failing-with-error-server-isn-t-connected-to-the-internet-error-25018.aspx)

### <a name="setup"></a>Setup
-   [De VMM-server kan niet worden geregistreerd door een interne fout. Zie de weergave van taken in de Portal-Site terugzetten voor meer informatie over de fout. Voer Setup uit om opnieuw te registreren van de server.](http://social.technet.microsoft.com/wiki/contents/articles/25570.the-vmm-server-cannot-be-registered-due-to-an-internal-error-please-refer-to-the-jobs-view-in-the-site-recovery-portal-for-more-details-on-the-error-run-setup-again-to-register-the-server.aspx)
-   [Verbinding kan geen worden gemaakt om de Hyper-V-beheer voor herstel. Controleer of de proxy-instellingen of probeer het later opnieuw.](http://social.technet.microsoft.com/wiki/contents/articles/25571.a-connection-cant-be-established-to-the-hyper-v-recovery-manager-vault-verify-the-proxy-settings-or-try-again-later.aspx)

### <a name="configuration"></a>Configuratie
-   [Kan geen bescherming groep maken: Er is een fout opgetreden bij het ophalen van de lijst met servers.](http://blogs.technet.com/b/somaning/archive/2015/08/12/unable-to-create-the-protection-group-in-azure-site-recovery-portal.aspx)
-   [Hyper-V host cluster bevat ten minste één statische netwerkadapter of geen aangesloten adapters zijn geconfigureerd voor gebruik van DHCP.](http://social.technet.microsoft.com/wiki/contents/articles/25498.hyper-v-host-cluster-contains-at-least-one-static-network-adapter-or-no-connected-adapters-are-configured-to-use-dhcp.aspx)
-   [VMM is niet gemachtigd om een actie te voltooien](http://social.technet.microsoft.com/wiki/contents/articles/31110.vmm-does-not-have-permissions-to-complete-an-action.aspx)
-   [De rekening voor opslag binnen het abonnement bij het configureren van beveiliging selecteren niet](http://social.technet.microsoft.com/wiki/contents/articles/32027.can-t-select-the-storage-account-within-the-subscription-while-configuring-protection.aspx)

### <a name="protection"></a>Bescherming
- <span style="color:green;">Nieuw</span> [Beveiliging inschakelen mislukt met fout 'bescherming kan niet worden geconfigureerd voor de virtuele machine'. Fout 60007, 40003](http://social.technet.microsoft.com/wiki/contents/articles/32194.azure-site-recovery-enable-protection-failing-with-error-protection-couldn-t-be-configured-for-the-virtual-machine-error-60007-40003.aspx)
- <span style="color:green;">Nieuw</span> [Beveiliging inschakelen mislukt met fout 'bescherming kan niet worden ingeschakeld voor de virtuele machine'. Fout 70094](http://social.technet.microsoft.com/wiki/contents/articles/32195.azure-site-recovery-enable-protection-failing-with-error-protection-couldn-t-be-enabled-for-the-virtual-machine-error-70094.aspx)
- <span style="color:green;">Nieuw</span> [Livemigratie fout 23848 - de virtuele machine gaat worden verplaatst met behulp van het type Live. Hierdoor kan de status van het herstel van de virtuele machine verbroken.](http://social.technet.microsoft.com/wiki/contents/articles/32021.live-migration-error-23848-the-virtual-machine-is-going-to-be-moved-using-type-live-this-could-break-the-recovery-protection-status-of-the-virtual-machine.aspx) 
- [Beveiliging is mislukt omdat de Agent niet is geïnstalleerd op de hostcomputer inschakelen](http://social.technet.microsoft.com/wiki/contents/articles/31105.enable-protection-failed-since-agent-not-installed-on-host-machine.aspx)
- [Een geschikte host voor de virtuele machine van replica is niet gevonden - door lage compute-bronnen](http://social.technet.microsoft.com/wiki/contents/articles/25501.a-suitable-host-for-the-replica-virtual-machine-can-t-be-found-due-to-low-compute-resources.aspx)
- [Een geschikte host voor de virtuele machine van replica is niet gevonden - omdat er geen logische netwerk aangesloten](http://social.technet.microsoft.com/wiki/contents/articles/25502.a-suitable-host-for-the-replica-virtual-machine-can-t-be-found-due-to-no-logical-network-attached.aspx)
- [Kan geen verbinding maken met de replica host machine - kan geen verbinding worden gemaakt](http://social.technet.microsoft.com/wiki/contents/articles/31106.cannot-connect-to-the-replica-host-machine-connection-could-not-be-established.aspx)


### <a name="recovery"></a>Herstel
- VMM bewerking, de host kan niet worden voltooid
    -   [Failover uitvoeren naar het geselecteerde herstelpunt voor virtuele machine: algemene fout: toegang geweigerd.](http://social.technet.microsoft.com/wiki/contents/articles/25504.fail-over-to-the-selected-recovery-point-for-virtual-machine-general-access-denied-error.aspx)
    -   [Hyper-V failover uitvoeren naar het geselecteerde herstelpunt voor virtuele machine is mislukt: bewerking is afgebroken, probeer een meer recente herstelpunt. (0x80004004)](http://social.technet.microsoft.com/wiki/contents/articles/25503.hyper-v-failed-to-fail-over-to-the-selected-recovery-point-for-virtual-machine-operation-aborted-try-a-more-recent-recovery-point-0x80004004.aspx)
    -   Een verbinding met de server kan niet worden vastgesteld (0x00002EFD)
        -   [Hyper-V kan niet worden ingeschakeld voor virtuele machine reverse-replicatie](http://social.technet.microsoft.com/wiki/contents/articles/25505.a-connection-with-the-server-could-not-be-established-0x00002efd-hyper-v-failed-to-enable-reverse-replication-for-virtual-machine.aspx)
        -   [Hyper-V kan geen replicatie voor virtuele machine virtuele machine inschakelen](http://social.technet.microsoft.com/wiki/contents/articles/25506.a-connection-with-the-server-could-not-be-established-0x00002efd-hyper-v-failed-to-enable-replication-for-virtual-machine-virtual-machine.aspx)
    -   [Failover voor de virtuele machine kan niet worden doorgevoerd.](http://social.technet.microsoft.com/wiki/contents/articles/25508.could-not-commit-failover-for-virtual-machine.aspx)
-   [Het herstelplan bevat virtuele machines, die niet gereed voor geplande failover](http://social.technet.microsoft.com/wiki/contents/articles/25509.the-recovery-plan-contains-virtual-machines-which-are-not-ready-for-planned-failover.aspx)
-   [De virtuele machine niet klaar voor geplande failover](http://social.technet.microsoft.com/wiki/contents/articles/25507.the-virtual-machine-isn-t-ready-for-planned-failover.aspx)
-   [Virtuele machine wordt niet uitgevoerd en wordt niet uitgeschakeld](http://social.technet.microsoft.com/wiki/contents/articles/25510.virtual-machine-is-not-running-and-is-not-powered-off.aspx)
-   [Van de band is er gebeurd op een virtuele machine en commit failover is mislukt](http://social.technet.microsoft.com/wiki/contents/articles/25507.the-virtual-machine-isn-t-ready-for-planned-failover.aspx)
-   Failover testen
    -   [Failover kan niet worden gestart omdat failover testen](http://social.technet.microsoft.com/wiki/contents/articles/31111.failover-could-not-be-initiated-since-test-failover-is-in-progress.aspx)
-   <span style="color:green;">Nieuw</span>  Failover wordt geblokkeerd bij PreFailoverWorkflow taak WaitForScriptExecutionTaskTimeout als gevolg van de configuratie-instellingen op de netwerk-beveiligingsgroep die is gekoppeld aan de virtuele Machine of het subnet waartoe de machine behoort. Verwijzen naar [PreFailoverWorkflow taak WaitForScriptExecutionTaskTimeout](https://aka.ms/troubleshoot-nsg-issue-azure-site-recovery) voor meer informatie.


### <a name="configuration-server-process-server-master-target"></a>Configuratie Server, processerver model doel
Configuratieserver (CS), processerver (PS), model Targer (MT)
-   [De ESXi host waarop de PS/CS wordt gehost als een VM mislukt met een paarse scherm van de dood.](http://social.technet.microsoft.com/wiki/contents/articles/31107.vmware-esxi-host-experiences-a-purple-screen-of-death.aspx)

### <a name="remote-desktop-troubleshooting-after-failover"></a>Extern bureaublad oplossen na een failover
-   Veel klanten werden geconfronteerd met problemen met verbinding maken met de mislukte via VM in Azure. [Gebruik van het RDP-document voor probleemoplossing in de VM](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx)

#### <a name="adding-a-public-ip-on-a-resource-manager-virtual-machine"></a>Een openbaar IP-adres toe te voegen op een virtuele machine voor resource manager
Als de knop **verbinding maken** in de portal is grijs en u niet met Azure via een Route Express of Site naar Site VPN-verbinding verbonden bent, moet u deze maken en uw VM een openbaar IP-adres toewijzen voordat u RDP/SSH kunt gebruiken. Volg onderstaande stappen voor het toevoegen van een openbaar IP-adres op de netwerkinterface van de virtuele machine.  

![Virtuele machine toe te voegen een openbaar IP-adres op de netwerkinterface van failover](media/site-recovery-monitoring-and-troubleshooting/createpublicip.gif)