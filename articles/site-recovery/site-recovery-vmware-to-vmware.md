<properties
    pageTitle="Repliceren op-premises VMware virtuele machines of fysieke servers met een secundaire site | Microsoft Azure"
    description="In dit artikel gebruiken voor het repliceren van VMs VMware of Windows/Linux fysieke servers met een secundaire site met Azure Site herstellen."
    services="site-recovery"
    documentationCenter=""
    authors="nsoneji"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="nisoneji"/>


# <a name="replicate-on-premises-vmware-virtual-machines-or-physical-servers-to-a-secondary-site"></a>Over lokalen VMware virtuele machines of fysieke servers repliceren naar een secundaire site


## <a name="overview"></a>Overzicht

InMage Scout in Azure-Site-Recovery biedt real-time replicatie tussen sites voor voor bedrijfsruimten VMware. InMage Scout is opgenomen in Azure-Site-Recovery serviceabonnementen.


## <a name="prerequisites"></a>Vereisten

**Azure account**: moet u een account van [Microsoft Azure](https://azure.microsoft.com/) . U kunt beginnen met een [gratis proefperiode](https://azure.microsoft.com/pricing/free-trial/). [Meer informatie](https://azure.microsoft.com/pricing/details/site-recovery/) over het herstel van de Site-prijzen.


## <a name="step-1-create-a-vault"></a>Stap 1: Maak een kluis
1. Log in om de [Azure portal](https://portal.azure.com).
2. Klik op nieuwe > Beheer > back-up en herstel van de Site (OMS). U kunt ook op Bladeren klikken > Recovery Services kluis > toevoegen.
3. Geef in het vak **naam** een beschrijvende naam ter identificatie van de kluis. Als u meer dan één abonnement hebt, selecteert u een van deze.
4. Maak een nieuwe resourcegroep in de **resourcegroep** of Selecteer een bestaande. Geef een Azure gebied om te voltooien vereiste velden. 
5.  Selecteer in de **locatie**, de geografische regio van de kluis. Ondersteunde regio's Zie [Azure Site herstel prijzen](https://azure.microsoft.com/pricing/details/site-recovery/).
5. Als u snel toegang tot de kluis van het Dashboard Klik op Pin aan dashboard en klik op maken.
6. De nieuwe kluis wordt weergegeven op het Dashboard > alle resources en op de belangrijkste Recovery Services kluizen blade.

## <a name="step-2-configure-the-vault-and-download-inmage-scout-components"></a>Stap 2: Configureren van de kluis en InMage Scout onderdelen downloaden
7. Selecteer uw kluis in de Recovery Services kluizen blade en klikt u op instellingen.
8. In de **Instellingen** > **Aan de slag** Klik op **Herstel van de Site** > stap 1: **Voorbereiden infrastructuur** > **doelstelling van bescherming**.
9. In de **doelstelling van bescherming** recovery site selecteren en selecteert u Ja, met VMware vSphere Hypervisor. Klik vervolgens op OK.
10. Klik op downloaden om te downloaden GA InMage Scout 8.0.1 en de registratie sleutel in **Scout setup**. De setup-bestanden voor alle vereiste onderdelen zijn in het gedownloade ZIP-bestand.


## <a name="step-3-install-component-updates"></a>Stap 3: Installeer updates van onderdelen

Meer informatie over de nieuwste [updates](#updates). Installeert u de updatebestanden op servers in de volgende volgorde:

1. RX-server als er een
2. Van configuratieservers
3. Proces-servers
3. Master doelservers.
4. vContinuum servers
5. De bronserver (Windows en Linux Server)

Als volgt te werk om de updates te installeren:

1. Download de [update](https://aka.ms/asr-scout-update4) ZIP-bestand. Dit zip-bestand bevat de volgende bestanden:

    - RX_8.0.4.0_GA_Update_4_8725872_16Sep16.tar.GZ
    - CX_Windows_8.0.4.0_GA_Update_4_8725865_14Sep16.exe
    - UA_Windows_8.0.4.0_GA_Update_4_9035261_27Sep16.exe
    - UA_RHEL6 64_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz
    - vCon_Windows_8.0.4.0_GA_Update_4_8921562_16Sep16.exe
    - UA update4 bits voor RHEL5, OL5, OL6, SUSE 10, SUSE 11: UA_<Linux OS>_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz 
    
2. Pak het ZIP-bestanden.<br>
3. **Server voor RX**: kopie, **RX_8.0.4.0_GA_Update_4_8725872_16Sep16.tar.gz** naar de server RX en pak het bestand. In de uitgepakte **map/Install**uitvoeren.<br>
4. **Voor de configuratie van server-proces-server**: kopiëren **CX_Windows_8.0.4.0_GA_Update_4_8725865_14Sep16.exe** aan de configuratieserver en processerver. Dubbelklik op uit te voeren.<br>
5. **Voor Windows-doelserver master**: wilt bijwerken de uniforme agent, **UA_Windows_8.0.4.0_GA_Update_4_9035261_27Sep16.exe** naar de master-doelserver kopiëren. Dubbelklik op het uit te voeren. Opmerking de uniforme agent is ook van toepassing op de bronserver. U moet deze installeren op de bronserver en, zoals verderop in deze lijst vermeld.<br>
7. **Voor de vContinuum-server**: **vCon_Windows_8.0.4.0_GA_Update_4_8921562_16Sep16.exe** kopiëren naar de server vContinuum.  Zorg ervoor dat u de wizard vContinuum hebt gesloten. Dubbelklik op het bestand uit te voeren.<br>
8. **Master voor Linux-doelserver**: om te werken de uniforme agent, **UA_RHEL6-64_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz** naar de master-doelserver kopiëren en pak het bestand. In de uitgepakte **map/Install**uitvoeren.<br>
9. **Voor Windows server bron**: kopiëren om de uniforme agent bijwerken, **UA_Windows_8.0.4.0_GA_Update_4_9035261_27Sep16.exe** naar de bronserver. Dubbelklik op het uit te voeren.<br>
10. **De bronserver voor Linux**: om te werken de uniforme agent, overeenkomstige versie van UA-bestand kopiëren naar de Linux-server en pak het bestand. In de uitgepakte **map/Install**uitvoeren.  Voorbeeld: RHEL 6,7 64 bits server **UA_RHEL6 64_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz** naar de server kopiëren en pak deze. In de uitgepakte **map/Install**uitvoeren.

## <a name="step-4-set-up-replication"></a>Stap 4: Instellen van de replicatie
1. Instellen van de replicatie tussen de bron en doel van VMware sites.
2. Gebruik de InMage Scout documentatie die gedownload met het product als leidraad. U kunt de documentatie ook als volgt openen:

    - [Release-opmerkingen](https://aka.ms/asr-scout-release-notes)
    - [Compatibiliteitstabel](https://aka.ms/asr-scout-cm)
    - [Gebruikershandleiding](https://aka.ms/asr-scout-user-guide)
    - [RX-gebruikershandleiding](https://aka.ms/asr-scout-rx-user-guide)
    - [Handleiding voor snelle installatie](https://aka.ms/asr-scout-quick-install-guide)


## <a name="updates"></a>Updates

### <a name="azure-site-recovery-scout-801-update-4"></a>Azure-Site herstellen Scout 8.0.1 Update 4
Scout Update 4 is een cumulatieve update. Heeft de correcties van update1 tot update3 en volgende nieuwe bugfixes en verbeteringen.

**Nieuwe platformondersteuning** 

- Is er ondersteuning toegevoegd voor vCenter/vSphere 6.0, 6.1 en 6.2
- Is er ondersteuning toegevoegd voor de volgende besturingssystemen voor Linux
    - Red Hat Enterprise Linux (RHEL) 7.0, 7.1 en 7.2 
    - CentOS 7.0, 7.1 en 7.2
    - Red Hat Enterprise Linux (RHEL) 6,8
    - CentOS 6,8

>[AZURE.NOTE]
>
> 7 RHEL/CentOS 64-bits **InMage_UA_8.0.1.0_RHEL7-64_GA_06Oct2016_release.tar.gz** wordt geleverd met een basispakket van Scout GA **InMage_Scout_Standard_8.0.1 GA.zip**. Downloaden van de portal Scout NH pakket zoals vermeld in [stap 1](site-recovery-vmware-to-vmware.md#Step 1: Create a vault).

**Bugfixes en verbeteringen** 

- Verbeterde verwerking voor volgende Linux-OSes en klonen om te voorkomen dat ongewenste een nieuwe synchronisatie problemen wordt afgesloten.
    - Red Hat Enterprise Linux (RHEL) 6.x
    - Oracle Linux (OL) 6.x
- Voer voor Linux, toegang tot mappen machtigingen in de installatiemap van uniforme agent nu alleen voor de lokale gebruiker beperkt zijn.
- Time-out probleem bij afgifte van gemeenschappelijke consistentie van gedistribueerde boek merk op zwaar geladen in Windows gedistribueerde toepassingen zoals SQL en gedeelde clusters.
- Toegevoegde logboek gerelateerde fix in basis CX-installatieprogramma.
- Downloadkoppeling voor VMware 6.0 vCLI wordt toegevoegd aan Windows Master doel base-installer.
- Meer controles en logboeken voor wijzigingen in het netwerk configuraties tijdens failover en DR oefeningen toegevoegd.
- Sometime bewaren informatie is niet aan de CX gemeld.  
- Voor fysieke cluster mislukt volume bewerking formaat wijzigen met de wizard vContinuum bij bron volume verkleinen is gebeurd.
- Cluster-bescherming is mislukt met fout 'Kan de schijfhandtekening vinden' wanneer cluster PRDM schijf betreft.
- cxps transport server loopt vast vanwege een uitzondering buiten het bereik. 
- Servernaam en IP-kolommen zijn nu resizable push installatie pagina van de wizard vContinuum.
- RX API-uitbreidingen
    - Geeft de vijf meest recente beschikbare algemene consistentie punten (alleen gegarandeerd-tags).
    - Capaciteit en de details van de vrije ruimte biedt voor de beveiligde apparaten.
    - Scout stuurprogramma staat hier op de bronserver. 
    
>[AZURE.NOTE] 
>
>- Het basispakket **InMage_Scout_Standard_8.0.1_GA.zip** heeft nu bijgewerkte CX basis installer **InMage_CX_8.0.1.0_Windows_GA_26Feb2015_release.exe** en Master Windows Target base installer **InMage_Scout_vContinuum_MT_8.0.1.0_Windows_GA_26Feb2015_release.exe**. Gebruiken voor alle nieuwe installatie nieuwe CX- en het doeldomein van Windows Master GA bits.
>- Update 4 rechtstreeks kan worden toegepast op 8.0.1 GA.
>- De configuratieserver en RX-updates kunnen niet worden hersteld nadat deze worden toegepast op het systeem.

### <a name="azure-site-recovery-scout-801-update-3"></a>Azure-Site-Recovery Scout 8.0.1 Update 3
Update 3 bevat de volgende correcties en verbeteringen:

- De van configuratieserver en RX niet registreren om de Site herstellen wanneer ze achter de proxy.
- Het aantal uren dat niet wordt voldaan aan de doelstelling van herstel punt (vrijgegeven Productieorder) is niet bijgewerkt in het rapport van de gezondheid.
- De configuratieserver is niet gesynchroniseerd met RX wanneer de ESX-hardwaregegevens of netwerkgegevens UTF-8-tekens bevat.
- Windows Server 2008 R2-domeincontrollers niet kunnen worden opgestart na herstel.
- Synchronisatie van off line werken niet zoals verwacht.
- Na een failover van de virtuele machine (VM) replicatie paar verwijdering niet verder komt dan de CX UI voor een lange tijd en gebruikers kunnen niet de failback-bewerking te voltooien of hervatten.
- Algemene verbroken snapshot bewerkingen die worden uitgevoerd door de taak consistentie geoptimaliseerd zijn om het te verminderen toepassing zoals SQL-clients.
- De prestaties van het hulpprogramma consistentie (VACP.exe) is verbeterd door vermindering van het gebruik van het geheugen dat nodig is voor het maken van momentopnamen op Windows.
- De push installeren service loopt vast wanneer u het wachtwoord dat groter is dan 16 tekens.
- vContinuum is niet gecontroleerd en nieuwe vCenter referenties vragen wanneer de referenties worden gewijzigd.
- Op Linux, is master target cache-beheer (cachemgr) niet downloaden van bestanden vanaf de server verwerken, waardoor de replicatie paar beperken.
- Wanneer u de volgorde van fysieke failover-cluster (MSCS) is niet hetzelfde op alle knooppunten, wordt replicatie is niet ingesteld voor sommige van de clustervolumes.
<br/>Houd er rekening mee dat het cluster moet worden reprotected om te profiteren van deze oplossing is opgenomen.  
- SMTP-functionaliteit werkt niet zoals verwacht nadat RX Scout 7.1 wordt bijgewerkt naar Scout 8.0.1.
- Meer statistieken zijn toegevoegd in het logboek voor de bewerking ongedaan maken voor het bijhouden van de tijd die hij heeft genomen om deze te voltooien.
- Ondersteuning is voor Linux-systemen op de bronserver toegevoegd:
    - Red Hat Enterprise Linux (RHEL) 6 update 7
    - CentOS 6 bijwerken 7
- De CX- en RX UI kunt nu de melding weergeven voor de twee in de bitmapmodus.
- De volgende oplossingen zijn in RX toegevoegd:

**Beschrijving van probleem**|**Uitvoeringsprocedures**
---|---
Verificatie via parameter manipulatie overslaan|Beperkte toegang voor gebruikers die niet van toepassing zijn.
Cross-site request vervalsing|Het begrip pagina token, dat wordt willekeurig gegenereerd voor elke pagina wordt geïmplementeerd. <br/>Hiermee wordt weergegeven: <li> Er is slechts één teken in exemplaar voor dezelfde gebruiker.</li><li>Pagina vernieuwen werkt niet--deze wordt omgeleid naar het dashboard.</li>
Schadelijk bestand uploaden|Beperkte bestanden met bepaalde extensies. Extensies zijn toegestaan: 7z, aiff, asf, avi, bmp, csv, doc, docx, FLA-, FLV-, GIF-, gz, gzip, JPEG-, jpg-, logboek, mid, mov, mp3, mp4, mpc, mpeg, mpg, ods-, odt, pdf, PNG-, ppt, pptx, pxd, qt, ram, rar, rm, rmi, rmvb, RTF-, sdc, sitd, swf, sxc, sxw, tar, tgz, tif, TIFF-, txt, vsd, wav, wma, wmv, xls, xlsx, xml, en zip.
Permanente cross-site scripting | Input validaties toegevoegd.


>[AZURE.NOTE]
>
>-  Alle updates voor herstel van de Site zijn cumulatief. Update 3 bevat alle correcties van Update 1 en 2 van de Update. Update 3 rechtstreeks kan worden toegepast op 8.0.1 GA.
>-  De configuratieserver en RX-updates kunnen niet worden hersteld nadat deze worden toegepast op het systeem.

### <a name="azure-site-recovery-scout-801-update-2-update-03dec15"></a>Azure-Site herstellen Scout 8.0.1 Update 2 (Update 03 Dec 15)

Update 2-correcties, waaronder:

- **Configuratieserver**: correctie voor een probleem waardoor de functie 31 dagen gratis meting niet werken zoals verwacht wanneer u de van configuratieserver is geregistreerd bij het herstellen van de Site.
- **Unified agent**: correctie voor een probleem in Update 1, waardoor de update niet geïnstalleerd op de master-doelserver wanneer deze is bijgewerkt van versie 8.0-8.0.1.


### <a name="azure-site-recovery-scout-801-update-1"></a>Azure-Site-Recovery Scout 8.0.1 Update 1

Update 1 bevat de volgende bugfixes en nieuwe functies:

- 31 dagen van gratis bescherming per server-exemplaar. Hiermee kunt u de functionaliteit testen of het instellen van een bewijs van concept.
    - Alle bewerkingen op de server, inclusief failover en failback, zijn gratis voor de eerste 31 dagen vanaf het tijdstip waarop een server eerst met Site herstel Scout is beveiligd.
    - Uit de 32nd dag, wordt elke server beveiligd afgeschreven exemplaar standaard snelheid voor Azure Site hersteld naar een site eigendom van klant.
    - Het aantal op beveiligde servers die worden momenteel berekend is op elk moment beschikbaar op de Dashboard-pagina van de kluis Azure Site herstellen.
- Ondersteuning toegevoegd voor vSphere opdrachtregelinterface (vCLI) 5.5 Update 2.
- Ondersteuning voor Linux-systemen op de bronserver wordt toegevoegd:
    - 6 RHEL 6 bijwerken
    - RHEL 5 11 bijwerken
    - Update centOS 6 6
    - Update centOS 5 11
- Bug opgelost met de volgende problemen op te lossen:
    - Registratie Vault voor configuratieserver of RX-server is mislukt.
    - Clustervolumes worden niet weergegeven zoals verwacht wanneer geclusterde virtuele machines worden reprotected wanneer ze weer.
    - Failback-bewerking mislukt als de doelserver master wordt gehost op een andere server ESXi van de virtuele machines op ruimten productie.
    - Configuratie bestandsmachtigingen worden gewijzigd wanneer u een upgrade naar 8.0.1 die van invloed op de bescherming en de bewerkingen.
    - De synchronisatieprocedure drempel niet afgedwongen zoals verwacht, die leidt tot replicatie inconsistent gedrag.
    - De instellingen van de vrijgegeven Productieorder worden niet correct weergegeven in de configuratie van de interface van server. De waarde van de niet-gecomprimeerde gegevens ten onrechte weergegeven de gecomprimeerde waarde.
    -  De bewerking verwijderen verwijdert, wordt niet zoals verwacht in de wizard vContinuum en replicatie wordt niet verwijderd uit de interface van de server configuratie.
    -  In de wizard vContinuum is de schijf automatisch geselecteerd wanneer u klikt op **Details** in de weergave van de schijf tijdens de bescherming van de MSCS virtuele machines.
    - Tijdens de fysieke naar virtuele (P2V)-scenario worden niet vereist HP services, zoals CIMnotify en CqMgHost, verplaatst naar handleiding bij het herstellen van de virtuele machine. Dit resulteert in extra opstarten.
    - Bescherming van de virtuele machine Linux mislukt als er meer dan 26 schijven op de doelserver master.

## <a name="next-steps"></a>Volgende stappen

Vragen die op de [Azure Recovery Services forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)posten.
