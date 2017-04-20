<properties 
   pageTitle="DR automatiseren voor bestandsshares op StorSimple met Azure Site herstellen | Microsoft Azure"
   description="Beschrijving van de stappen en aanbevolen procedures voor het maken van een ramp hersteloplossing voor bestandsshares op StorSimple opslag gehost."
   services="storsimple"
   documentationCenter="NA"
   authors="vidarmsft"
   manager="syadav"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="05/16/2016"
   ms.author="vidarmsft" />

# <a name="automated-disaster-recovery-solution-using-azure-site-recovery-for-file-shares-hosted-on-storsimple"></a>Geautomatiseerde Disaster Recovery-oplossing met Azure Site herstel voor gedeelde bestanden die worden gehost op StorSimple

## <a name="overview"></a>Overzicht

Microsoft Azure StorSimple is een hybride cloud opslag-oplossing die voldoet aan de complexiteit van ongestructureerde gegevens zijn vaak gekoppeld aan gedeelde bestanden. StorSimple gebruikt cloud opslag als een uitbreiding van de oplossing op gebouwen en automatisch lagen gegevens over opslag in de bedrijfsruimten en cloud opslag. Geïntegreerde bescherming van gegevens van een lokale en momentopnamen van de wolk, een veelzijdig opslaginfrastructuur overbodig.

[Azure-Site-Recovery](../site-recovery/site-recovery-overview.md) is een op basis van Azure service die voorziet in disaster recovery (DR) functies door regie van replicatie, failover en herstel van virtuele machines. Azure-Site-Recovery ondersteunt een aantal technologieën replicatie consequent repliceren, beschermen en virtuele machines en wolken persoonlijke/openbare of gehoste toepassingen naadloos mislukken.

Herstel van Azure-Site, virtuele machine replicatie en StorSimple wolk momentopname mogelijkheden gebruikt, kunt u het volledige bestand server-omgeving beveiligen. In geval van een onderbreking, kunt u één klik uw gedeelde bestanden on line brengen in Azure in een paar minuten.

Dit document wordt in detail uitgelegd hoe u een disaster recovery-oplossing voor uw gedeelde bestanden die worden gehost op StorSimple opslag, maken en uitvoeren van de geplande, ongeplande, en testen met behulp van een herstelplan met één klik failover. Het bevat in wezen, hoe kunt u het herstelplan in uw kluis Azure Site herstellen om StorSimple failover tijdens het herstel na storing scenario's. Daarnaast beschrijft het ondersteunde configuraties en vereisten. Dit document wordt ervan uitgegaan dat u bekend met de basisbeginselen van Azure Site herstellen en StorSimple-architecturen bent.

## <a name="supported-azure-site-recovery-deployment-options"></a>Ondersteunde Azure Site implementatie herstelopties

Klanten kunnen implementeren bestandsservers als fysieke servers of virtuele machines (VMs) waarop Hyper-V of VMware en vervolgens bestandsshares maken van volumes geen opslagruimte StorSimple verzinken. Azure-Site-Recovery kan zowel fysieke en virtuele implementaties Azure of een secundaire site te beschermen. Dit document heeft betrekking op de details van een DR-oplossing met Azure als de recovery-site voor een bestandsserver die VM op Hyper-V gehost en bestandsshares op StorSimple opslag. Andere scenario's waarin de bestandsserver VM op een VM VMware of een fysieke machine is kunnen ook worden geïmplementeerd.

## <a name="prerequisites"></a>Vereisten

Implementatie van een ramp met één klik hersteloplossing die Azure Site herstel voor gedeelde bestanden die worden gehost op StorSimple opslag gebruikt, gelden de volgende vereisten:

-   Bestand van Windows Server 2012 R2 server voor VM Hyper-V of VMware of een fysieke machine ruimten

-   StorSimple opslag apparaat op ruimten geregistreerd met Azure StorSimple manager

-   StorSimple wolk toestel gemaakt in Azure StorSimple manager (dit kan worden onderhouden afsluiten)

-   Gedeelde bestanden op volumes die zijn geconfigureerd op het opslagapparaat StorSimple gehost

-   [Azure-Site-Recovery services kluis](../site-recovery/site-recovery-vmm-to-vmm.md) gemaakt van een abonnement op Microsoft Azure

Ook als uw site herstel is Azure, het uitvoeren [Azure Virtual Machine Readiness Assessment tool](http://azure.microsoft.com/downloads/vm-readiness-assessment/) op VMs om ervoor te zorgen dat ze compatibel met Azure VMs-en wegsleepdiensten voor Azure-Site zijn.

Om te voorkomen dat problemen (die kunnen leiden tot hogere kosten), zorg ervoor dat u uw StorSimple wolk toestel, automatisering account en opslag maakt latentie (s) in dezelfde regio.

## <a name="enable-dr-for-storsimple-file-shares"></a>DR voor bestandsshares StorSimple inschakelen  

Elk onderdeel van het milieu op het bedrijf moet worden beschermd om replicatie is voltooid en herstel. Deze sectie wordt beschreven hoe u kunt:

-   Instellen van de replicatie van Active Directory en DNS (optioneel)

-   Azure-Site herstellen gebruiken om de bescherming van de bestandsserver VM

-   Beveiliging van StorSimple volumes

-   Het netwerk configureren

### <a name="set-up-active-directory-and-dns-replication-optional"></a>Instellen van de replicatie van Active Directory en DNS (optioneel)

Als u wilt dat de computers met Active Directory en DNS, zodat ze beschikbaar op de site DR zijn beveiligen, moet u expliciet beveiligen (zodat de bestandsservers toegankelijk nadat de failover-verificatie zijn). Er zijn twee aanbevolen opties op basis van de complexiteit van de omgeving op de gebouwen van de klant.

#### <a name="option-1"></a>Optie 1

Als de klant een klein aantal toepassingen heeft, één domeincontroller voor de gehele site lokalen en zal worden failover de gehele site en vervolgens het beste gebruiken Azure Site herstel replicatie voor het repliceren van de machine domain controller met een secundaire site (dit geldt voor zowel de site naar site als de site naar Azure).

#### <a name="option-2"></a>Optie 2

Als de klant heeft een groot aantal toepassingen, een Active Directory-forest wordt uitgevoerd en zal worden failover enkele toepassingen tegelijk, is het instellen van een extra domeincontroller op de site DR wordt aangeraden (een secundaire site of in Azure).

Zie [Automatische DR oplossing voor Active Directory en DNS met Azure Site herstellen](../site-recovery/site-recovery-active-directory.md) voor instructies bij het maken van een domeincontroller beschikbaar op de website van DR. Voor de rest van dit document wordt ervan uitgegaan dat een domeincontroller beschikbaar is op de site DR.

### <a name="use-azure-site-recovery-to-enable-protection-of-the-file-server-vm"></a>Azure-Site herstellen gebruiken om de bescherming van de bestandsserver VM

Deze stap moet u de serveromgeving op gebouwen-bestand voorbereiden, maken en voorbereiden van een kluis Azure Site herstellen, en bestandsbeveiliging van de VM inschakelen.

#### <a name="to-prepare-the-on-premises-file-server-environment"></a>Voorbereiden van de serveromgeving op gebouwen bestand

1.  Het **beheer van gebruikersaccount** ingesteld op **nooit waarschuwen**. Dit is nodig zodat Azure automatiseringsscripts kunt verbinding maken met de iSCSI-doelen na door Azure Site herstel is mislukt.

    1.  Druk op de Windows-toets + Q en zoek naar de **UAC**.

    2.  Selecteer **Instellingen voor Gebruikersaccountbeheer wijzigen**.

    3.  Sleep de balk aan de onderkant naar **Nooit waarschuwen**.

    4.  Klik op **OK** en selecteer vervolgens **Ja** wanneer dat wordt gevraagd.

        ![](./media/storsimple-dr-using-asr/image1.png)

1.  De VM-Agent installeren op elk van de VMs-bestandsserver. Dit is vereist, zodat u Azure automatische scripts op de mislukte via VMs uitvoeren kunt.

    1.  [De agent downloaden](http://aka.ms/vmagentwin) op `C:\\Users\\<username>\\Downloads`.

    2.  Windows PowerShell te openen in de Administrator-modus (uitvoeren als Administrator) en voer de volgende opdracht om te navigeren naar de locatie:

        `cd C:\\Users\\<username>\\Downloads\\WindowsAzureVmAgent.2.6.1198.718.rd\_art\_stable.150415-1739.fre.msi`

        > [AZURE.NOTE] Naam van het bestand kan wijzigen afhankelijk van de versie.

1.  Klik op **volgende**.

2.  Accepteer de **Voorwaarden van de overeenkomst** en klik op **volgende**.

3.  Klik op **Voltooien**.


1.  Verzinken van StorSimple storage volumes met bestandsshares maken. Zie [werken met de service Manager StorSimple volumes beheren](storsimple-manage-volumes.md)voor meer informatie.

    1.  Druk op de Windows-toets + Q en zoeken voor **iSCSI**uw VMs in gebouwen.

    2.  Selecteer de **iSCSI-initiator**.

    3.  Klik op het tabblad **configuratie** en de initiatornaam van de kopiëren.

    4.  Log in op de [Azure klassieke portal](https://manage.windowsazure.com/).

    5.  Selecteer het tabblad **StorSimple** en selecteer vervolgens de StorSimple Manager-Service met het fysieke apparaat.

    6.  Container(s) volume maken en maak vervolgens de volumes. (Deze volumes zijn voor het bestand share(s) op de bestandsserver VMs). Kopiëren, de naam van de initiator en geef een passende naam voor de Control Access Records als u de volumes maken.

    7.  Selecteer het tabblad **configureren** en noteer het IP-adres van het apparaat.

    8.  Ga opnieuw naar de **iSCSI-initiator** op uw VMs in gebouwen en geef het IP-in de sectie snel verbinding maken. Klik op **Snel verbinding maken** (het apparaat moet nu worden verbonden).

    9.  Open de Azure Management Portal en klik op het tabblad **Volumes en apparaten** . Klik op **automatisch configureren**. Het volume dat u zojuist hebt gemaakt, moet worden weergegeven.

    10. In de portal, selecteer het tabblad **apparaten** en selecteer vervolgens **maakt een nieuwe virtuele apparaatstuurprogramma.** (Deze virtueel apparaat wordt gebruikt als een failover optreedt). Deze nieuwe virtueel apparaat kan worden bewaard in een off line status voor de extra kosten te vermijden. Virtueel apparaat offline halen, Ga naar de sectie **virtuele Machines** op de Portal en deze afsluiten.

    11. Ga terug naar de VMs op gebouwen en Schijfbeheer opent (druk op de Windows-toets + X en selecteert u **Schijfbeheer**).

    12. U zult sommige extra schijven (afhankelijk van het aantal volumes dat u hebt gemaakt). Klik met de rechtermuisknop op het eerste bestand, selecteer **Schijf initialiseren**en selecteer **OK**. Klik met de rechtermuisknop op de sectie **Unallocated** , selecteer **Nieuw eenvoudig Volume**, een stationsletter toewijzen en voltooi de wizard.

    13. Herhaal stap l voor alle schijven. U ziet nu alle schijven op **Deze PC** in Windows Verkenner.

    14. De rol van bestands- en opslagservices gebruiken om bestandsshares te maken op deze volumes.

#### <a name="to-create-and-prepare-an-azure-site-recovery-vault"></a>Voor het maken en voorbereiden van een kluis Azure Site herstellen

Raadpleeg de [documentatie van Azure Site herstellen](../site-recovery/site-recovery-hyper-v-site-to-azure.md) aan de slag met Azure Site herstellen voordat de VM-bestandsserver is beveiligd.

#### <a name="to-enable-protection"></a>Beveiliging inschakelen

1.  De iSCSI-target(s) loskoppelen van het VMs in ruimten die u wilt beveiligen met Azure-Site-Recovery:

    1.  Druk op de Windows-toets + Q en zoeken voor **iSCSI**.

    2.  Selecteer de **iSCSI-initiator instellen**.

    3.  Het apparaat StorSimple die u eerder hebt aangesloten. U kunt ook de server uitschakelen voor een paar minuten om bescherming.

    > [AZURE.NOTE] Hierdoor wordt de gedeelde bestanden tijdelijk niet beschikbaar

1.  [Beveiliging van de virtuele machine](../site-recovery/site-recovery-hyper-v-site-to-azure.md##step-6-enable-replication) van de bestandsserver VM vanaf de portal Azure Site herstellen.

2.  Wanneer u de initiële synchronisatie begint, kunt u het doel opnieuw. Ga naar de iSCSI-initiator, selecteert u het apparaat StorSimple en klik op **verbinding maken**.

3.  Wanneer de synchronisatie voltooid is en de status van de VM **beveiligd is**, de VM selecteren, selecteert u het tabblad **configureren** en het netwerk van de VM dienovereenkomstig bijwerken (dit is het netwerk van de mislukte via VM(s) een deel van is). Als het netwerk niet wordt weergegeven, betekent dit dat de synchronisatie is nog steeds gebeurt.

### <a name="enable-protection-of-storsimple-volumes"></a>Beveiliging van StorSimple volumes

Als u de optie **een standaard back-up voor dit volume** voor de StorSimple volumes niet hebt geselecteerd, gaat u naar de **Back-up beleid** in de StorSimple Manager-service en een geschikt back-beleid voor alle volumes maken. Het is raadzaam dat u de frequentie van de back-ups instellen op herstel punt doel (vrijgegeven Productieorder) die u zou willen zien voor de toepassing.

### <a name="configure-the-network"></a>Het netwerk configureren

Voor de bestandsserver VM netwerkinstellingen bij het herstellen van Azure-Site zodanig configureren dat de VM-netwerken zijn aangesloten op het juiste netwerk van DR na een failover.

Zoals in de volgende afbeelding wordt weergegeven, kunt u de VM de **VMM wolk** van de **Groep bescherming** om de netwerkinstellingen te configureren.

![](./media/storsimple-dr-using-asr/image2.png)

## <a name="create-a-recovery-plan"></a>Maak een herstelplan

U kunt een herstelplan in ASR voor het automatiseren van failover van de bestandsshares maken. Als een onderbreking optreedt, kunt u de bestandsshares omhoog brengen in een paar minuten met één klik. Als u wilt dat deze vorm van automatisering, moet u een account Azure automatisering.

#### <a name="to-create-the-account"></a>Account wilt maken.

1.  Ga naar de klassieke Azure portal en Ga naar de sectie **automatisering** .

1.  Maak een nieuwe account voor automatisering. Houd het in dezelfde geo/regio waarin de StorSimple wolk toestel en opslag accounts zijn gemaakt.

2.  Klik op **Nieuw** &gt; **App Services** &gt; **automatisering** &gt; **Runbook** &gt; **Uit de galerie** voor het importeren van de vereiste runbooks rekening van de automatisering.

    ![](./media/storsimple-dr-using-asr/image3.png)

1.  De volgende runbooks toevoegen vanuit het deelvenster **Disaster Recovery** in de galerie:

    -   Failover StorSimple volume containers

    -   Clean-up van de volumes StorSimple na Test Failover (TFO)

    -   Volumes koppelen op StorSimple apparaat na een failover

    -   Start de virtuele toestel StorSimple

    -   Aangepast script extensie in Azure VM verwijderen

        ![](./media/storsimple-dr-using-asr/image4.png)


1.  Alle scripts selecteert u de runbook op de rekening van de automatisering en **auteur** tabblad wilt publiceren. Na deze stap wordt het tabblad **Runbooks** als volgt weergegeven:

     ![](./media/storsimple-dr-using-asr/image5.png)

1.  Ga naar het tabblad **activa** in de automatisering, **Instelling toevoegen** klikt u op &gt; **Referentie toevoegen**, en voeg uw referenties Azure – het activum AzureCredential naam.

    Gebruik de Windows PowerShell-referentie. Dit moet een referentie waarin een organisatie-ID en het wachtwoord met toegang tot dit abonnement Azure en meerledige verificatie uitgeschakeld. Dit is vereist voor het verifiëren van de gebruiker tijdens de failover en om het bestand servervolumes op de site DR.

1.  Selecteer het tabblad **activa** in de automatisering-account en klik vervolgens op **Instelling toevoegen** &gt; **variabele toevoegen** en voeg de volgende variabelen. U kunt kiezen voor het coderen van deze activa. Deze variabelen zijn specifieke plan voor herstel. Als uw herstelplan (die u maakt in de volgende stap) moeten naam TestPlan is, is de variabelen TestPlan StorSimRegKey en TestPlan AzureSubscriptionName.

    -   *RecoveryPlanName* **-StorSimRegKey**: de registratie sleutel voor de StorSimple Manager-service.

    -   *RecoveryPlanName* **-AzureSubscriptionName**: de naam van het abonnement Azure.

    -   *RecoveryPlanName* **-ResourceName**: de naam van de StorSimple resource met het apparaat StorSimple.

    -   *RecoveryPlanName* **-Apparaatnaam**: het apparaat dat moet worden overgenomen.

    -   *RecoveryPlanName* **-TargetDeviceName**: de StorSimple wolk toestel waarvan de containers zijn moet worden overgenomen.

    -   *RecoveryPlanName* **-VolumeContainers**: een door komma's gescheiden tekenreeks van containers voor volume op het apparaat dat moet worden overgenomen; bijvoorbeeld, volcon1, volcon2, volcon3.

    -   RecoveryPlanName**-TargetDeviceDnsName**: de naam van het doelapparaat (dit kunt u vinden in de sectie **virtuele Machine** : naam van de service is hetzelfde als de DNS-naam).

    -   *RecoveryPlanName* **-StorageAccountName**: de naam van de opslag die het script (die worden uitgevoerd op de mislukte via VM heeft) wordt opgeslagen. Dit is een opslag-account met ruimte voor tijdelijke opslag van het script.

    -   *RecoveryPlanName* **-StorageAccountKey**: de toegangstoets voor de bovenstaande opslag account.

    -   *RecoveryPlanName* **-ScriptContainer**: de naam van de container waarin het script worden opgeslagen in de cloud. Als de container niet bestaat, wordt deze gemaakt.

    -   *RecoveryPlanName* **-VMGUIDS**: bij het beveiligen van een VM Azure Site herstel elke VM een unieke ID toegewezen die de details van de mislukte in VM krijgt. Als u de VMGUID, selecteer het tabblad **Recovery Services** en klik op **Item beveiligd** &gt; **Bescherming groepen** &gt; **Machines** &gt; **Eigenschappen**. Als er meerdere VMs, voegt u de GUID's als een door komma's gescheiden tekenreeks.

    -   *RecoveryPlanName* **-AutomationAccountName** : de naam van de rekening van de automatisering die u aan de runbooks en de activa toevoegt.

    Bijvoorbeeld als de naam van het herstelplan fileServerpredayRP is, ziet vervolgens het tabblad **activa** er als volgt na het toevoegen van alle activa.

    ![](./media/storsimple-dr-using-asr/image6.png)


1.  Ga naar de sectie **Recovery Services** en selecteer de kluis Azure Site herstellen die u eerder hebt gemaakt.

2.  Selecteer het tabblad **Systeemherstel plannen** en maakt u een nieuw herstelplan als volgt:

    een.  Geef een naam en selecteer de gewenste **Groep bescherming**.

    b.  Selecteer het VMs uit de bescherming die u wilt opnemen in het herstelplan.

    c.  Na het herstel plan is gemaakt, selecteert u het openen van de weergave voor aanpassing van Recovery plan.

    d.  Selecteer **afsluiten voor alle groepen**, klikt u op **Script**en kies **toevoegen een primaire script voordat alle groep afgesloten**.

    e.  Selecteer de account die automatisering (u toegevoegd waarin de runbooks) en selecteer vervolgens de runbook **over StorSimple-Volume Containers mislukken** .

    f.  Klik op **groep 1: Start** **virtuele Machines**te kiezen en toevoegen van het VMs die moeten worden beschermd in het herstelplan.

    g.  Klik op **groep 1: Start**, kies **Script**en voeg de volgende scripts in volgorde als stappen **na groep 1** .

    - Runbook start-StorSimple-virtuele-toestel
    - Meer StorSimple-volume containers runbook mislukt
    - Mount volumes nadat failover-runbook
    - Runbook verwijderen-custom-script-uitbreiding

1.  Een handmatige actie na de bovenstaande 4 scripts toevoegen in dezelfde **groep 1: stappen na** sectie. Deze actie is het punt waarop u controleren kunt of alles goed werkt. Deze actie moet worden toegevoegd als een deel van de test failover (alleen selecteren de **Failover-Test** selectievakje).

2.  Nadat de handmatige actie de Cleanup script toevoegen met dezelfde procedure die u voor de andere runbooks gebruikt. Sla het herstelplan.

    > [AZURE.NOTE] Wanneer een test failover wordt uitgevoerd, moet u controleren of alles op de handmatige Actiestap omdat de StorSimple volumes die was op het doelapparaat is gekloond als onderdeel van het opruimen worden verwijderd nadat de handmatige actie is voltooid.

    ![](./media/storsimple-dr-using-asr/image7.png)

## <a name="perform-a-test-failover"></a>Een failover test uitvoeren

Raadpleeg de handleiding voor [Active Directory DR oplossing](../site-recovery/site-recovery-active-directory.md) voor specifieke overwegingen voor Active Directory tijdens de test-failover. De installatie in gebouwen is niet helemaal verstoord wanneer de test failover plaatsvindt. De StorSimple volumes die zijn aangesloten op de VM op ruimten worden op het toestel StorSimple wolk op Azure gekloond. Een VM voor testdoeleinden is opgeroepen in Azure en de gekloonde volumes zijn gekoppeld aan de VM.

#### <a name="to-perform-the-test-failover"></a>Voor het uitvoeren van failover testen

1.  Selecteer uw site herstel kluis in de Azure klassieke portal.

1.  Klik op het herstelplan voor de bestandsserver VM gemaakt.

2.  Klik op **Test Failover**.

3.  Selecteer het virtuele netwerk het failover-testproces te starten.

    ![](./media/storsimple-dr-using-asr/image8.png)

1.  Als de secundaire-omgeving, kunt u uw validaties uitvoeren.

2.  Nadat de validatie voltooid is, klikt u op **Volledige validaties**. De failover-testomgeving zullen worden verwijderd en de TFO bewerking wordt voltooid.

## <a name="perform-an-unplanned-failover"></a>Een niet-geplande failover uitvoeren

Tijdens een niet-geplande failover StorSimple volumes worden overgenomen door het virtuele apparaat en een replica VM in Azure worden gebracht van de volumes zijn gekoppeld aan de VM.

#### <a name="to-perform-an-unplanned-failover"></a>Een niet-geplande failover uitvoeren

1.  Selecteer uw site herstel kluis in de Azure klassieke portal.

1.  Klik op het herstelplan voor de bestandsserver VM gemaakt.

2.  Klik op **Failover** en selecteer vervolgens de **Niet-geplande Failover**.

    ![](./media/storsimple-dr-using-asr/image9.png)

1.  Selecteer het netwerk en klik vervolgens op het pictogram voor ✓ de failover-procedure starten.

## <a name="perform-a-planned-failover"></a>Een geplande failover uitvoeren

De lokalen op bestand tijdens een geplande failover server die VM ' elegant ' afsluiten en een wolk back-momentopname van de volumes op StorSimple apparaat is gemaakt. De StorSimple volumes worden overgenomen door het virtuele apparaat, een replica VM in Azure is opgeroepen en de volumes zijn gekoppeld aan de VM.

#### <a name="to-perform-a-planned-failover"></a>Voor het uitvoeren van een geplande failover

1.  Selecteer uw site herstel kluis in de Azure klassieke portal.

1.  Klik op het herstelplan voor de bestandsserver VM gemaakt.

2.  Klik op **Failover** en selecteer vervolgens de **Geplande Failover**.

3.  Selecteer het netwerk en klik vervolgens op het pictogram voor ✓ de failover-procedure starten.

## <a name="perform-a-failback"></a>Een failback-bewerking uitvoeren

Tijdens een failback-bewerking worden StorSimple volume containers overgenomen naar het fysieke apparaat nadat u een back-up wordt genomen.

#### <a name="to-perform-a-failback"></a>Een failback-bewerking uitvoeren

1.  Selecteer uw site herstel kluis in de Azure klassieke portal.

1.  Klik op het herstelplan voor de bestandsserver VM gemaakt.

2.  Klik op **Failover** en selecteer **geplande failover** of **niet-geplande failover**.

3.  Klik op **de richting wijzigen**.

4.  Selecteer de juiste gegevenssynchronisatie en opties voor VM maken.

5.  Klik op het pictogram voor ✓ het failback-proces te starten.

    ![](./media/storsimple-dr-using-asr/image10.png)

## <a name="best-practices"></a>Best Practices

### <a name="capacity-planning-and-readiness-assessment"></a>Capaciteit planning en gereedheid beoordeling


#### <a name="hyper-v-site"></a>Hyper-V-site

De [gebruiker Capaciteitsplanner](http://www.microsoft.com/download/details.aspx?id=39057) voor het ontwerpen van de server, opslag- en netwerkinfrastructuur voor uw replica Hyper-V-omgeving gebruiken.

#### <a name="azure"></a>Azure

U kunt de [Azure Virtual Machine Readiness Assessment tool](http://azure.microsoft.com/downloads/vm-readiness-assessment/) uitvoeren op VMs om ervoor te zorgen dat ze compatibel met Azure VMs en Azure-Site-Recovery Services zijn. De Readiness Assessment Tool VM configuraties controleert en waarschuwt als de configuraties niet compatibel zijn met Azure. Deze problemen, bijvoorbeeld een waarschuwing als een C:-station groter dan 127 GB is.


Capaciteitsplanning bestaat uit ten minste twee belangrijke processen:

-   Toewijzing op-premises Hyper-V VMs voor Azure VM formaten (bijvoorbeeld A6, A7, A8 en A9).

-   Bepaling van de vereiste bandbreedte voor Internet.

## <a name="limitations"></a>Beperkingen

- Op dit moment kan slechts 1 StorSimple apparaat failover worden (op een enkele StorSimple wolk). Het scenario van een bestandsserver die verschillende StorSimple apparaten omvat wordt nog niet ondersteund.

- Als u een foutmelding krijgt tijdens het inschakelen van beveiliging voor een VM, ervoor zorgen dat u de iSCSI-doelen hebt verbroken.

- Alle volume-containers die samen zijn gegroepeerd door back-up beleid over containers volume wordt failover uitgevoerd bij elkaar.

- De volumes in de volume-containers die u hebt gekozen wordt failover uitgevoerd.

- Volumes die maximaal meer dan 64 TB toevoegen kunnen niet worden via is mislukt omdat de maximale capaciteit van een enkel StorSimple wolk toestel 64 TB.

- Als de geplande/niet-geplande failover mislukt en het VMs worden gemaakt in Azure, vervolgens niet door Ntfrs.exe opgeschoond het VMs. In plaats daarvan een failback-bewerking uitvoeren. Als u de VMs verwijdert kunnen niet vervolgens het VMs in gebouwen worden weer ingeschakeld.

- Nadat failover is uitgevoerd, als u niet kunt zien van de volumes, gaat u naar de VMs, opent u Schijfbeheer de schijven opnieuw controleren en vervolgens on line brengen.

- In sommige gevallen de stationsletters in de site DR mogelijk anders dan de letters op gebouwen. Als dit gebeurt, moet u het probleem handmatig corrigeren nadat de failover is voltooid.

- Meerledige verificatie moet zijn uitgeschakeld voor de Azure referentie die is ingevoerd als een actief in de automatisering. Deze verificatie niet is uitgeschakeld, scripts, kunnen niet automatisch worden uitgevoerd als het herstelplan zal mislukken.

- Time-out van failover-taak: de StorSimple-script time-out wordt als de overname van volume containers langer duurt dan de limiet van Azure Site herstellen per script (momenteel 120 minuten).

- Time-out van back-uptaak: de StorSimple-script time-out optreedt als de back-up van de volumes langer dan de limiet van Azure Site herstellen per script (momenteel 120 minuten duurt).
 
    > [AZURE.IMPORTANT] De back-up handmatig uitvoeren vanuit de portal Azure en voer het herstelplan opnieuw.

- Time-out van taak klonen: de StorSimple-script time-out als het klonen van volumes langer dan de limiet van Azure Site herstellen per script (momenteel 120 minuten duurt).

- Fout bij de gegevenssynchronisatie tijd: de StorSimple scripts fouten om te zeggen dat de back-ups mislukt, waren zelfs als de back-up voltooid in de portal is. Een mogelijke oorzaak van dit is mogelijk wanneer de StorSimple-toestel mogelijk niet synchroon met de huidige tijd in de tijdzone.
 
    > [AZURE.IMPORTANT] Synchroniseer de tijd van het toestel met de huidige tijd in de tijdzone.

- Toestel failover-fout: de StorSimple script kan mislukken als er een failover van het toestel is als het herstelplan wordt uitgevoerd.
    
    > [AZURE.IMPORTANT] Het herstelplan opnieuw nadat het toestel failover uitgevoerd is.

## <a name="summary"></a>Samenvatting

Azure-Site herstellen gebruikt, kunt u een volledig geautomatiseerde calamiteitenplan voor een bestandsserver VM met gedeelde bestanden die worden gehost op StorSimple opslag. Binnen enkele seconden vanaf elke locatie kunt u de failover-functie starten in geval van een onderbreking en de toepassing in een paar minuten bedrijf te krijgen.
