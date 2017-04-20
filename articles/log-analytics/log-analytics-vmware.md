<properties
    pageTitle="VMware Monitoring oplossing in logboek Analytics | Microsoft Azure"
    description="Meer informatie over hoe de oplossing voor het controleren van VMware ESXi hosts te bewaken en beheren van Logboeken kan helpen."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/28/2016"
    ms.author="banders"/>

# <a name="vmware-monitoring-preview-solution-in-log-analytics"></a>Oplossing in logboek Analytics VMware controleren (voorbeeld)

De VMware Monitoring oplossing in logboek Analytics is een oplossing die helpt u bij het maken van een centrale registratie en controle-aanpak voor grote VMware Logboeken. In dit artikel wordt beschreven hoe u kunt oplossen, vastleggen en beheren van de hosts ESXi op één locatie met behulp van de oplossing. Met de oplossing, kunt u gedetailleerde gegevens voor uw ESXi hosts op één locatie bekijken. U ziet de bovenste gebeurtenis telt, status en trends van VM en ESXi hosts via de logboeken ESXi host. Als u problemen met het bekijken en het zoeken van gecentraliseerde ESXi host Logboeken. En kunt u de waarschuwingen op basis van zoekopdrachten logboek maken.

De oplossing maakt gebruik van syslog native functionaliteit van de ESXi host push-gegevens naar een doel VM met OMS-Agent. De oplossing niet echter geen bestanden schrijven naar syslog binnen een VM. De agent OMS 1514 poort wordt geopend en luistert naar dit. Zodra het de gegevens ontvangt, duwt de OMS agent de gegevens in het Kantoorbeheersysteem.

## <a name="installing-and-configuring-the-solution"></a>Installeren en configureren van de oplossing

Gebruik de volgende informatie te installeren en configureren van de oplossing.

- De VMware Monitoring oplossing toevoegen aan uw Kantoorbeheersysteem werkruimte met behulp van het proces beschreven in [oplossingen logboek Analytics toevoegen uit de galerie met oplossingen](log-analytics-add-solutions.md).

#### <a name="supported-vmware-esxi-hosts"></a>Ondersteunde VMware ESXi hosts
vSphere ESXi Host 5.5 en 6.0

#### <a name="prepare-a-linux-server"></a>Voorbereiding van een Linux-server
Maak een Linux-besturingssysteem VM alle syslog om gegevens te ontvangen van hosts die ESXi. De [OMS Linux Agent](log-analytics-linux-agents.md) is de collectie voor alle ESXi host syslog-gegevens. Voor het doorsturen van logboeken met één Linux-server, zoals in het volgende voorbeeld kunt u meerdere ESXi hosts.  

   ![Syslog-stroom](./media/log-analytics-vmware/diagram.png)

### <a name="configure-syslog-collection"></a>Collectie syslog configureren

1. Syslog doorsturen voor VSphere instellen. Zie voor gedetailleerde informatie over instellen van syslog doorsturen, [syslog configureren op ESXi 5.x en 6.0 (2003322)](https://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=2003322). Ga naar **ESXi Host configuratie** > **Software** > **Geavanceerde instellingen** > **Syslog**.
  ![vsphereconfig](./media/log-analytics-vmware/vsphere1.png)  

2. Toevoegen in het veld *Syslog.global.logHost* de Linux-server en de poort nummer *1514*. Bijvoorbeeld, `tcp://hostname:1514` of`tcp://123.456.789.101:1514`

3. Open de firewall ESXi host voor syslog. **ESXi hostconfiguratie** > **Software** > **Beveiligingsprofiel** > **Firewall** en **Eigenschappen**voor openen.  

    ![vspherefw](./media/log-analytics-vmware/vsphere2.png)  

    ![vspherefwproperties](./media/log-analytics-vmware/vsphere3.png)  

4. Controleer de vSphere Console om te controleren of deze syslog juist is ingesteld. Bevestig op de ESXI host die poort **1514** is geconfigureerd.

5. Test de verbinding tussen de Linux-server en de ESXi host met behulp van de `nc` in de ESXi Host. Bijvoorbeeld:

    ```
    [root@ESXiHost:~] nc -z 123.456.789.101 1514
    Connection to 123.456.789.101 1514 port [tcp/*] succeeded!
    ```

6. Download en installeer de OMS-Agent voor Linux op de Linux-server. Zie voor meer informatie de [documentatie bij OMS Agent voor Linux](https://github.com/Microsoft/OMS-Agent-for-Linux).

7. Nadat de OMS-Agent voor Linux is geïnstalleerd, gaat u naar de map /etc/opt/microsoft/omsagent/sysconf/omsagent.d en het vmware_esxi.conf-bestand kopiëren naar de map /etc/opt/microsoft/omsagent/conf/omsagent.d en de wijziging, de eigenaar of groep en de machtigingen van het bestand. Bijvoorbeeld:

    ```
    sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.d/vmware_esxi.conf /etc/opt/microsoft/omsagent/conf/omsagent.d
sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/conf/omsagent.d/vmware_esxi.conf
    ```

8.  Start de Agent OMS voor Linux opnieuw door `sudo /opt/microsoft/omsagent/bin/service_control restart`.

9. In de Portal OMS doorzoeken logboek voor `Type=VMware_CL`. OMS verzamelt de gegevens syslog, de syslog opmaak behouden. In het portal voor worden sommige specifieke velden vastgelegd, zoals *hostnaam* en *Procesnaam*.  

    ![type](./media/log-analytics-vmware/type.png)  

    Als uw zoekresultaten weergave logboek vergelijkbaar met de bovenstaande afbeelding zijn, stelt u bent op het dashboard OMS VMware Monitoring oplossing gebruiken.  

## <a name="vmware-data-collection-details"></a>VMware data collectie details

De VMware Monitoring oplossing verzamelt diverse statistieken en logboek en prestatiegegevens van ESXi hosts met behulp van de Agents OMS voor Linux die u hebt ingeschakeld.

De volgende tabel bevat de methoden van de collectie gegevens en andere informatie over de manier waarop gegevens worden verzameld.

| platform | OMS Agent voor Linux | SCOM agent | Azure opslag | SCOM nodig? | SCOM agent gegevens verzonden via management groep | frequentie van verzameling |
|---|---|---|---|---|---|---|
|Linux|![Ja](./media/log-analytics-vmware/oms-bullet-green.png)|![Nee](./media/log-analytics-vmware/oms-bullet-red.png)|![Nee](./media/log-analytics-vmware/oms-bullet-red.png)|            ![Nee](./media/log-analytics-containers/oms-bullet-red.png)|![Nee](./media/log-analytics-vmware/oms-bullet-red.png)| elke 3 minuten|


De volgende tabel ziet u voorbeelden van gegevensvelden die zijn verzameld door de Monitoring van VMware-oplossing:

| Veldnaam | Beschrijving |
| --- | --- |
| Device_s| VMware-opslagapparaten |
| ESXIFailure_s | Fout bij het typen |
| EventTime_t | tijd waarop de gebeurtenis is opgetreden |
| HostName_s | ESXi host-naam |
| Operation_s | VM maken of verwijderen van VM |
| ProcessName_s | naam van evenement |
| ResourceId_s | naam van de host VMware |
| ResourceLocation_s | VMware |
| ResourceName_s | VMware |
| ResourceType_s | Hyper-V |
| SCSIStatus_s | VMware SCSI-status |
| SyslogMessage_s | Syslog-gegevens |
| UserName_s | gebruiker die heeft gemaakt of verwijderd, VM |
| VMName_s | VM-naam |
| Computer | host-computer |
| TimeGenerated | Wanneer die de gegevens is gegenereerd. |
| DataCenter_s | VMware datacenter |
| StorageLatency_s | opslag wachttijd (ms) |

## <a name="vmware-monitoring-solution-overview"></a>Overzicht van VMware Monitoring oplossingen

De VMware-tegel wordt weergegeven in de portal OMS. Er wordt een globaal overzicht van storingen. Wanneer u op de tegel klikt, gaat u naar de dashboardweergave van een.

![naast elkaar](./media/log-analytics-vmware/tile.png)

#### <a name="navigate-the-dashboard-view"></a>Ga het dashboard weergeven

Klik in de dashboardweergave **VMware** zijn blades gerangschikt op:

- Aantal fouten-Status
- Top Host door gebeurtenis telt
- Telt het aantal bovenste gebeurtenis
- Activiteiten van de virtuele Machine
- ESXi Host schijf gebeurtenissen


![solution1](./media/log-analytics-vmware/solutionview1-1.png)

![solution2](./media/log-analytics-vmware/solutionview1-2.png)

Klik op elke bladeserver zoekvenster Analytics logboek waarin gedetailleerde informatie specifiek voor het blad te openen.

Hier kunt u de query wilt wijzigen voor een bepaald. Uitchecken voor een zelfstudie over de basisbeginselen van OMS zoeken, de [OMS logboek zoeken zelfstudie.](log-analytics-log-searches.md)

#### <a name="find-esxi-host-events"></a>ESXi host gebeurtenissen

Één ESXi host genereert meerdere gebeurtenislogboeken, op basis van hun processen. De oplossing voor het controleren van VMware zijn ze gecentraliseerd en geeft een overzicht van de aantallen van de gebeurtenis. Deze centrale weergave kunt u zien welke ESXi host heeft een groot aantal gebeurtenissen en welke gebeurtenissen meest in uw omgeving.

![gebeurtenis](./media/log-analytics-vmware/events.png)

U kunt inzoomen door te klikken op een ESXi host of een gebeurtenistype verder.

Wanneer u op de naam van een ESXi host, kunt u de informatie uit die ESXi host weergeven. Toevoegen als u wilt om de resultaten met het gebeurtenistype te beperken, `“ProcessName_s=EVENT TYPE”` in uw query. U kunt de **Procesnaam** in het zoekfilter selecteren. Dat de informatie voor u wordt beperkt.

![drill](./media/log-analytics-vmware/eventhostdrilldown.png)

#### <a name="find-high-vm-activities"></a>Hoge VM activiteiten zoeken

Een virtuele machine kan worden gemaakt en verwijderd op een willekeurige ESXi host. Het is nuttig als een beheerder om te bepalen hoeveel VMs een ESXi host maakt. Die in-beurt, helpt om te begrijpen, prestaties en capaciteitsplanning. Het bijhouden van activiteitsgebeurtenissen VM is cruciaal bij het beheer van uw omgeving.

![drill](./media/log-analytics-vmware/vmactivities1.png)

Als u wilt om extra ESXi host VM maken gegevens te bekijken, klikt u op de naam van een ESXi host.

![drill](./media/log-analytics-vmware/createvm.png)

#### <a name="common-search-queries"></a>Gewone zoekopdrachten

De oplossing bevat andere handige query's die u kunnen helpen bij het beheren van uw ESXi hosts, zoals hoge opslagruimte, opslagruimte latentie en padfout.

![query 's](./media/log-analytics-vmware/queries.png)

#### <a name="save-queries"></a>Query's opslaan

Zoekquery's opslaan is een standaardfunctie in OMS en kunt u alle query's die u hebt gevonden nuttig. Nadat u een query die nuttig voor u maken, door te klikken op de **Favorieten**opslaan. Een opgeslagen query, kunt u gemakkelijk later opnieuw vanuit de pagina [Mijn Dashboard](log-analytics-dashboards.md) is waar u uw eigen aangepaste dashboards kunt maken.

![DockerDashboardView](./media/log-analytics-vmware/dockerdashboardview.png)

#### <a name="create-alerts-from-queries"></a>Waarschuwingen maken in query 's

Nadat u uw query's hebt gemaakt, kunt u de query's gebruiken om u te waarschuwen wanneer bepaalde gebeurtenissen plaatsvinden. Voor meer informatie over het maken van waarschuwingen Zie: [waarschuwingen in logboek Analytics](log-analytics-alerts.md) . Zie voor voorbeelden van signalering van query's en andere voorbeelden van de query de [Monitor VMware gebruiken OMS logboek Analytics](https://blogs.technet.microsoft.com/msoms/2016/06/15/monitor-vmware-using-oms-log-analytics) blog boeken.

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

### <a name="what-do-i-need-to-do-on-the-esxi-host-setting-what-impact-will-it-have-on-my-current-environment"></a>Wat moet ik doen op de ESXi host instellen Welke invloed heeft dit op mijn huidige omgeving
De oplossing maakt gebruik van de eigen ESXi Host Syslog mechanisme doorsturen. U hoeft geen extra software op de ESXi Host voor het vastleggen van de logboeken van Microsoft. Er moet een lage impact op uw bestaande omgeving. U hoeft echter doorsturen van syslog ESXI-functionaliteit is ingesteld.

### <a name="do-i-need-to-restart-my-esxi-host"></a>Moet ik mijn ESXi host opnieuw opstarten?
Nr. Dit proces hoeft niet opnieuw op te starten. Soms vSphere niet correct bijgewerkt de syslog. In een dergelijk geval zich aanmeldt bij de host ESXi en opnieuw laden het syslog. U hebt geen opnieuw, start opnieuw op de host, zodat dit proces is niet storend is voor uw omgeving.

### <a name="can-i-increase-or-decrease-the-volume-of-log-data-sent-to-oms"></a>Kan ik verhogen of verlagen van het volume van de logboekgegevens naar OMS verzonden?
U kunt Ja. U kunt de ESXi Host logboekniveau-instellingen in vSphere. Logboek-collectie is gebaseerd op het niveau van de *info* . Als u controleren VM maken of verwijderen wilt, moet u dus het niveau van de *info* op de Hostd te houden. Zie voor meer informatie de [Knowledge Base van VMware](https://kb.vmware.com/selfservice/microsites/search.do?&cmd=displayKC&externalId=1017658).

### <a name="why-is-hostd-not-providing-data-to-oms-my-log-setting-is-set-to-info"></a>Waarom is Hostd niet dat gegevens levert aan OMS? Mijn logboek is ingesteld op info.
Er is een bug ESXi host voor het tijdstempel syslog. Zie voor meer informatie de [Knowledge Base van VMware](https://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=2111202). Nadat u de tijdelijke oplossing hebt toegepast, moet Hostd normaal functioneren.

### <a name="can-i-have-multiple-esxi-hosts-forwarding-syslog-data-to-a-single-vm-with-omsagent"></a>Kan ik meerdere ESXi hosts syslog gegevens doorsturen naar een enkele VM met omsagent hebben?
Ja. U kunt meerdere ESXi hosts worden doorgestuurd naar een enkele VM met omsagent hebben.

### <a name="why-dont-i-see-data-flowing-into-oms"></a>Waarom zie ik geen gegevens OMS inblazen?

Er zijn verschillende redenen:

- De ESXi host pusht gegevens voor het uitvoeren van omsagent VM niet correct. Als u wilt testen, voert u de volgende stappen uit:
    1. Om te bevestigen, meld u aan bij de ESXi host via ssh en voer de volgende opdracht uit:`nc -z ipaddressofVM 1514`

        Als dit niet lukt, vSphere-instellingen in de geavanceerde configuratie worden waarschijnlijk niet opgelost. Zie [Configure syslog-collectie](#configure-syslog-collection) voor meer informatie over het instellen van de ESXi host voor het doorsturen van syslog.

    2. Als syslog poort verbinding gelukt is, maar alle gegevens nog steeds niet wordt weergegeven, vervolgens opnieuw laden het syslog op de ESXi host via ssh Voer de volgende opdracht uit:` esxcli system syslog reload`

- De VM met OMS-Agent is niet juist ingesteld. U kunt dit testen, moet u de volgende stappen uitvoeren:
    1. OMS luistert op de poort 1514 en duwt de gegevens in het Kantoorbeheersysteem. Om te controleren of deze is geopend, kunt u de volgende opdracht uitvoeren:`netstat -a | grep 1514`
    2. Ziet u poort `1514/tcp` openen. Als u dit niet doet, moet u controleren of dat de omsagent correct is geïnstalleerd. Als u de informatie over de poort niet wordt weergegeven, vervolgens de syslog poort is niet open op de VM.
        1. Controleer of de OMS-Agent wordt uitgevoerd met behulp van `ps -ef | grep oms`. Als deze niet wordt uitgevoerd, start het proces met de opdracht` sudo /opt/microsoft/omsagent/bin/service_control start`
        2. Open de `/etc/opt/microsoft/omsagent/conf/omsagent.d/vmware_esxi.conf` bestand.

            Controleer of de juiste gebruiker en groep instelling geldig, vergelijkbaar met:`-rw-r--r-- 1 omsagent omiusers 677 Sep 20 16:46 vmware_esxi.conf`

            Als het bestand niet bestaat of als de gebruiker en de instelling van verkeerde, correctiemaatregelen door het [voorbereiden van een Linux-server](#prepare-a-linux-server).

## <a name="next-steps"></a>Volgende stappen

- Met [Logboek zoekopdrachten](log-analytics-log-searches.md) in logboek Analytics kunt u gedetailleerde gegevens van VMware host.
- [Uw eigen dashboards maken](log-analytics-dashboards.md) met VMware hostgegevens.
- [Waarschuwingen maken](log-analytics-alerts.md) wanneer specifieke VMware host gebeurtenissen plaatsvinden.
