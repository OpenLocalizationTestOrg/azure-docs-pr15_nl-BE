<properties
   pageTitle="MPIO configureren op de host StorSimple Linux | Microsoft Azure"
   description="MPIO configureren voor verbinding met een Linux-host met 6.6 CentOS StorSimple"
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/21/2016"
   ms.author="alkohli" />

# <a name="configure-mpio-on-a-storsimple-host-running-centos"></a>MPIO configureren op een StorSimple host met CentOS

In dit artikel worden de stappen beschreven die vereist voor het configureren van Multipath I/O (MPIO) op uw server Centos 6.6 host. De host-server is verbonden met het Microsoft Azure StorSimple apparaat voor maximale beschikbaarheid via iSCSI-initiators. Deze beschrijving gedetailleerde van de automatische detectie van apparaten met meerdere paden en de specifieke instellingen voor StorSimple volumes.

Deze procedure is van toepassing op alle modellen van apparaten StorSimple 8000-serie.

>[AZURE.NOTE] Deze procedure kan niet worden gebruikt voor een virtueel apparaat StorSimple. Zie voor meer informatie het configureren van host-servers voor het virtuele apparaat.

## <a name="about-multipathing"></a>Over meerdere paden gebruiken

De Multipath-functie kunt u meerdere i/o-paden tussen een apparaat voor gegevensopslag en de host-server configureren. Deze i/o-paden zijn fysieke SAN verbindingen met afzonderlijke kabels, switches, netwerkinterfaces en controllers. Multipath aggregeert de i/o-paden, het configureren van een nieuw apparaat dat is gekoppeld aan alle samengestelde paden.

Het doel van multipath is tweeledig:

- **Hoge beschikbaarheid**: biedt een alternatief pad als een element van het i/o-pad (zoals een kabel, switch, netwerk-interface of controller) mislukt.

- **Load balancing**: afhankelijk van de configuratie van uw opslagapparaat, kan het de prestaties verbeteren door het detecteren van belastingen op de i/o-paden en deze belasting dynamisch opnieuw.


### <a name="about-multipathing-components"></a>Multipath-onderdelen

Meerdere paden gebruiken in Linux bestaat uit de kernel componenten en onderdelen van de gebruiker-ruimte als onder de tabelindeling.

- **Kernel**: het voornaamste bestanddeel is het *apparaat mapper* I/O wordt omgeleid en biedt ondersteuning voor failover voor paden en groepen pad.

1. **Gebruiker-ruimte**: *multipath-tools* multipathed apparaten door het apparaat mapper multipath module wat te doen zijn. Het programma bestaat uit:

    - **Paden**: lijsten en multipathed apparaten configureren.

    - **Multipathd**: daemon die MPIO uitvoert en bewaakt de paden.

    - **Devmap naam**: een zinvolle apparaatnaam aan udev biedt voor devmaps.

    - **Kpartx**: lineaire devmaps wordt toegewezen aan de partities die kunnen meerdere paden kaarten partitionable apparaat.

    - **Multipath.conf**: configuratiebestand voor multipath daemon die wordt gebruikt voor de van de ingebouwde configuratietabel overschreven.

### <a name="about-the-multipathconf-configuration-file"></a>Over het configuratiebestand multipath.conf

Het configuratiebestand `/etc/multipath.conf` kunt u veel functies Multipath gebruiker worden geconfigureerd. De `multipath` opdracht en de daemon kernel `multipathd` informatie in dit bestand gebruiken. Het bestand wordt alleen tijdens de configuratie van de apparaten die MPIO geraadpleegd. Zorg ervoor dat alle wijzigingen worden aangebracht voordat u het `multipath` opdracht. Als u het bestand later wijzigt, moet u stoppen en starten van de multipathd opnieuw om de wijzigingen van kracht te laten worden.

De multipath.conf bestaat uit vijf secties:

- **Standaardwaarden systeem op siteniveau** *(standaard)*: U kunt standaardwaarden op siteniveau systeem overschrijven.

1. **Apparaten op zwarte lijst geplaatst** *(zwarte lijst)*: U kunt opgeven dat de lijst met apparaten die niet moeten worden gecontroleerd door apparaat mapper.

1. **Zwarte lijst Uitzonderingen** *(blacklist_exceptions)*: U specifieke apparaten als MPIO-apparaten worden behandeld, zelfs als vermeld in de zwarte lijst kunt herkennen.

1. **Specifieke instellingen voor opslag-controller** *(apparaten)*: kunt u configuratie-instellingen die worden toegepast op leveranciers-en apparaten.

1. **Apparaat-specifieke instellingen** *(multipaths)*: U kunt deze sectie aanpassen van de configuratie-instellingen voor afzonderlijke LUN's.

## <a name="configure-multipathing-on-storsimple-connected-to-linux-host"></a>Meerdere paden gebruiken op StorSimple verbonden met Linux host configureren

Een verbinding met een host Linux StorSimple-apparaat kan worden geconfigureerd voor hoge beschikbaarheid en taakverdeling. Bijvoorbeeld, als de host Linux twee interfaces op het SAN aangesloten heeft en het apparaat twee interfaces op het SAN aangesloten heeft dat deze interfaces op hetzelfde subnet worden, zal er 4 paden beschikbaar. Echter, als elke DATA interface op de interface-apparaat en de host in een ander IP-subnet (en niet routeerbaar), vervolgens slechts 2 paden is beschikbaar. U kunt meerdere paden gebruiken wilt automatisch de beschikbare paden ontdekken, een algoritme voor netwerktaakverdeling voor die paden kiezen, gelden specifieke configuratie-instellingen voor alleen-StorSimple volumes, inschakelen en Multipath controleren.

De volgende procedure wordt beschreven hoe u multipath configureren als een apparaat StorSimple met twee netwerkinterfaces is verbonden met een host met twee netwerkinterfaces.

## <a name="prerequisites"></a>Vereisten

Dit gedeelte wordt ingegaan op de configuratie vereisten voor de server van CentOS en het apparaat StorSimple.

### <a name="on-centos-host"></a>Op host CentOS



1. Zorg ervoor dat de host CentOS 2 netwerkinterfaces ingeschakeld. Type:

    `ifconfig`

    In het volgende voorbeeld ziet u de uitvoer wanneer twee netwerkinterfaces (`eth0` en `eth1`) op de host aanwezig zijn.

        [root@centosSS ~]# ifconfig
        eth0  Link encap:Ethernet  HWaddr 00:15:5D:A2:33:41  
        inet addr:10.126.162.65  Bcast:10.126.163.255  Mask:255.255.252.0
        inet6 addr: 2001:4898:4010:3012:215:5dff:fea2:3341/64 Scope:Global
        inet6 addr: fe80::215:5dff:fea2:3341/64 Scope:Link
        UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
        RX packets:36536 errors:0 dropped:0 overruns:0 frame:0
        TX packets:6312 errors:0 dropped:0 overruns:0 carrier:0
        collisions:0 txqueuelen:1000
        RX bytes:13994127 (13.3 MiB)  TX bytes:645654 (630.5 KiB)

        eth1  Link encap:Ethernet  HWaddr 00:15:5D:A2:33:42  
        inet addr:10.126.162.66  Bcast:10.126.163.255  Mask:255.255.252.0
        inet6 addr: 2001:4898:4010:3012:215:5dff:fea2:3342/64 Scope:Global
        inet6 addr: fe80::215:5dff:fea2:3342/64 Scope:Link
        UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
        RX packets:25962 errors:0 dropped:0 overruns:0 frame:0
        TX packets:11 errors:0 dropped:0 overruns:0 carrier:0
        collisions:0 txqueuelen:1000
        RX bytes:2597350 (2.4 MiB)  TX bytes:754 (754.0 b)

        loLink encap:Local Loopback  
        inet addr:127.0.0.1  Mask:255.0.0.0
        inet6 addr: ::1/128 Scope:Host
        UP LOOPBACK RUNNING  MTU:65536  Metric:1
        RX packets:12 errors:0 dropped:0 overruns:0 frame:0
        TX packets:12 errors:0 dropped:0 overruns:0 carrier:0
        collisions:0 txqueuelen:0
        RX bytes:720 (720.0 b)  TX bytes:720 (720.0 b)


1. *ISCSI-initiator-utils* installeren op uw server CentOS. Voer de volgende stappen uit om de *iSCSI-initiator-utils*installeren.

    1. Meld u aan als `root` in uw host CentOS.

    1. De *iSCSI-initiator-utils*installeren. Type:

        `yum install iscsi-initiator-utils`


    1. Nadat de *iSCSI-Initiator-utils* is geïnstalleerd, start u de iSCSI-service. Type:

        `service iscsid start`

        Bij gelegenheden, `iscsid` kan pas beginnen en de `--force` optie nodig zijn

    1. Om ervoor te zorgen dat uw iSCSI-initiator tijdens het opstarten is ingeschakeld, gebruikt u de `chkconfig` opdracht om te schakelen van de service.

        `chkconfig iscsi on`


    1. Om te controleren dat deze correct is instellen, moet u de opdracht uitvoeren:

        `chkconfig --list | grep iscsi`

        Hieronder vindt u een voorbeeld van uitvoer.

            iscsi   0:off   1:off   2:on3:on4:on5:on6:off
            iscsid  0:off   1:off   2:on3:on4:on5:on6:off

        Uit het bovenstaande voorbeeld kunt u zien dat uw iSCSI-omgeving wordt uitgevoerd op opstarten op run niveaus 2, 3, 4 en 5.


1. *Device-mapper-MPIO*installeren. Type:

    `yum install device-mapper-multipath`

    De installatie wordt gestart. Type **Y** om door te gaan wanneer u wordt gevraagd om bevestiging.



### <a name="on-storsimple-device"></a>Op StorSimple apparaat

Het apparaat StorSimple moet hebben:

- Een minimum van twee interfaces ingeschakeld voor iSCSI. Als u wilt controleren of twee interfaces iSCSI op uw apparaat StorSimple is ingeschakeld, moet u de volgende stappen uitvoeren in Azure klassieke portal voor uw apparaat StorSimple:

    1. Log in op de klassieke portal voor uw apparaat StorSimple.

    1. Selecteer uw service Manager StorSimple **apparaten** en op het specifieke StorSimple apparaat kiezen. Klik op **configureren** en controleren of de instellingen van de interface. Hieronder vindt u een screenshot met twee interfaces voor iSCSI-netwerk. Hier 2 gegevens en gegevens 3, beide 10 GbE interfaces zijn ingeschakeld voor iSCSI.

        ![MPIO StorsSimple DATA 2 config](./media/storsimple-configure-mpio-on-linux/IC761347.png)

        ![MPIO StorSimple gegevens 3 Config](./media/storsimple-configure-mpio-on-linux/IC761348.png)

        In de pagina **configureren**

        1. Zorg ervoor dat beide netwerkinterfaces iSCSI ingeschakeld. Het veld **iSCSI ingeschakeld** moet worden ingesteld op **Ja**.
        2. Ervoor te zorgen dat de netwerkinterfaces hebben dezelfde snelheid moeten beide 1 GbE of 10 GbE zijn.
        3. Noteer de IPv4-adressen van de iSCSI-compatibele interfaces en opslaan voor toekomstig gebruik op de host.


- De iSCSI-interfaces op het apparaat StorSimple moet bereikbaar zijn vanaf de server CentOS.

    Om dit te controleren, moet u de IP-adressen van uw StorSimple iSCSI-netwerkinterfaces op de host-server. De opdrachten en de bijbehorende output met Bestand2 (10.126.162.25) en DATA3 (10.126.162.26) wordt hieronder weergegeven:

        [root@centosSS ~]# iscsiadm -m discovery -t sendtargets -p 10.126.162.25:3260
        10.126.162.25:3260,1 iqn.1991-05.com.microsoft:storsimple8100-shx0991003g44mt-target
        10.126.162.26:3260,1 iqn.1991-05.com.microsoft:storsimple8100-shx0991003g44mt-target


### <a name="hardware-configuration"></a>Configuratie van hardware

Wij raden aan dat u verbinding maakt met de twee iSCSI-netwerkinterfaces op afzonderlijke paden voor redundantie. In de volgende afbeelding ziet u de aanbevolen hardwareconfiguratie voor hoge beschikbaarheid en Multipath-taakverdeling voor uw server voor CentOS en StorSimple apparaat.

![MPIO hardware config voor StorSimple Linux host](./media/storsimple-configure-mpio-on-linux/MPIOHardwareConfigurationStorSimpleToLinuxHost2M.png)

Zoals in de voorgaande afbeelding:

- Uw apparaat StorSimple wordt een actief-passief configuratie met twee controllers.

- Twee SAN-switches zijn aangesloten op uw apparaat.

- Twee iSCSI-initiators zijn op het apparaat StorSimple ingeschakeld.

- Twee netwerkinterfaces zijn ingeschakeld op uw host CentOS.

De bovenstaande configuratie resulteert in 4 afzonderlijke paden tussen het apparaat en de host als de host- en interfaces geschikt zijn.

>[AZURE.IMPORTANT]
>
>- Wij raden aan dat u niet met 1 GbE en 10 GbE-netwerkinterfaces voor meerdere paden gebruiken mengen. Wanneer u twee netwerkinterfaces, moeten zowel de interfaces het hetzelfde type.
>- Op het apparaat StorSimple DATA0, DATA1, DATA4 en DATA5 zijn 1 GbE-interfaces bestand2 en DATA3 10 GbE-netwerkinterfaces zijn. |

## <a name="configuration-steps"></a>Configuratiestappen

De configuratiestappen voor meerdere paden gebruiken betrekking hebben op de beschikbare paden voor automatische detectie, geven het algoritme-taakverdeling wordt gebruikt, configureert Multipath inschakelen en ten slotte de configuratie controleren. Elk van deze stappen wordt in de volgende secties gedetailleerd beschreven.

### <a name="step-1-configure-multipathing-for-automatic-discovery"></a>Stap 1: Meerdere paden gebruiken voor automatische detectie configureren

Multipath-ondersteunde apparaten kunnen automatisch worden gedetecteerd en geconfigureerd.

1. Initialiseren `/etc/multipath.conf` bestand. Type:

     `Copy mpathconf --enable`

    De bovenstaande opdracht maakt u een `sample/etc/multipath.conf` bestand.


1. Multipath-service starten. Type:

    ``Copy service multipathd start``

    Ziet u de volgende uitvoer:

    `Starting multipathd daemon:`

1. Schakel de automatische detectie van multipaths. Type:

    `mpathconf --find_multipaths y`

    Dit wijzigt de standaardinstellingen voor sectie van de `multipath.conf` als volgt:

        defaults {
        find_multipaths yes
        user_friendly_names yes
        path_grouping_policy multibus
        }

### <a name="step-2-configure-multipathing-for-storsimple-volumes"></a>Stap 2: Multipath StorSimple volumes configureren

Standaard worden alle apparaten zwart zijn vermeld in het bestand multipath.conf en zal worden omzeild. U moet een zwarte lijst uitzonderingen zodat meerdere paden gebruiken voor volumes van StorSimple apparaten maken.

1. Bewerkt de `/etc/mulitpath.conf` bestand. Type:

    `vi /etc/multipath.conf`

1. Zoek de sectie blacklist_exceptions in het bestand multipath.conf. Het apparaat StorSimple moet worden weergegeven als een zwarte lijst uitzondering in deze sectie. U kunt opmerkingen bij relevante regels in dit bestand te wijzigen zoals hieronder (Gebruik alleen het specifieke model van het apparaat dat u gebruikt):

        blacklist_exceptions {
            device {
                       vendor  "MSFT"
                       product "STORSIMPLE 8100*"
            }
            device {
                       vendor  "MSFT"
                       product "STORSIMPLE 8600*"
            }
           }

### <a name="step-3-configure-round-robin-multipathing"></a>Stap 3: Multipath round robin configureren

Dit algoritme-taakverdeling gebruikt alle beschikbare multipaths op de actieve controller op een evenwichtige, round robin manier.

1. Bewerkt de `/etc/multipath.conf` bestand. Type:

    `vi /etc/multipath.conf`

1. Onder de `defaults` sectie, stelt u de `path_grouping_policy` op `multibus`. De `path_grouping_policy` geeft het standaardpad beleid toe te passen op niet-gespecificeerde multipaths groeperen. De sectie Programmatoegang eruit zoals hieronder wordt weergegeven.

        defaults {
                user_friendly_names yes
                path_grouping_policy multibus
        }



> [AZURE.NOTE]
> De meest gangbare waarden van `path_grouping_policy` omvatten:

> - failover = 1 pad per groep
> - multibus = alle geldige paden in de groep prioriteit 1

### <a name="step-4-enable-multipathing"></a>Stap 4: Enable Multipath

1. Start de `multipathd` daemon. Type:

    `service multipathd restart`

1. De uitvoer is zoals hieronder wordt weergegeven:

        [root@centosSS ~]# service multipathd start
        Starting multipathd daemon:  [OK]




### <a name="step-5-verify-multipathing"></a>Stap 5: Controleer of meerdere paden gebruiken

1. Zorg eerst dat iSCSI-verbinding is gemaakt met het apparaat StorSimple als volgt:


    1. Ontdek het StorSimple apparaat. Type:

        `iscsiadm -m discovery -t sendtargets -p  <IP address of network interface on the device>:<iSCSI port on StorSimple device>`

        De uitvoer wanneer het IP-adres voor DATA0 is 10.126.162.25 en poort 3260 is geopend op het apparaat StorSimple voor uitgaande iSCSI-verkeer is als volgt:

            10.126.162.25:3260,1 iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target
            10.126.162.26:3260,1 iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target


        Kopieer de IQN van uw apparaat StorSimple `iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target`, uit de voorafgaande uitvoer.



    1. Verbinding maken met het apparaat met behulp van target IQN. Het apparaat StorSimple is het iSCSI-doel. Type:

        `iscsiadm -m node --login -T <IQN of iSCSI target>`

        In het volgende voorbeeld ziet u uitvoer met een doel IQN van `iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target`. De uitvoer geeft aan dat u verbinding hebt gemaakt met de twee interfaces voor iSCSI-netwerk op uw apparaat.

            Logging in to [iface: eth0, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.25,3260] (multiple)
            Logging in to [iface: eth1, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.25,3260] (multiple)
            Logging in to [iface: eth0, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.26,3260] (multiple)
            Logging in to [iface: eth1, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.26,3260] (multiple)
            Login to [iface: eth0, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.25,3260] successful.
            Login to [iface: eth1, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.25,3260] successful.
            Login to [iface: eth0, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.26,3260] successful.
                Login to [iface: eth1, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.26,3260] successful.


        Als er slechts één host-interface en hier twee paden, moet u om zowel de interfaces op de host voor iSCSI. U kunt Volg de [gedetailleerde instructies in de documentatie van Linux](https://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/5/html/Online_Storage_Reconfiguration_Guide/iscsioffloadmain.html).


    1. Een volume wordt blootgesteld aan de server van CentOS van het apparaat StorSimple. Zie voor meer informatie [stap 6: een volume maken](storsimple-deployment-walkthrough.md#step-6-create-a-volume) via de Azure klassieke portal op het apparaat StorSimple.

    1. Controleer of de beschikbare paden. Type:

        `multipath –l`

        In het volgende voorbeeld ziet u de uitvoer van twee netwerkinterfaces op een StorSimple apparaat aangesloten op een netwerk één hostinterface met twee beschikbare paden.

            mpathb (36486fd20cc081f8dcd3fccb992d45a68) dm-3 MSFT,STORSIMPLE 8100
            size=100G features='0' hwhandler='0' wp=rw
            `-+- policy='round-robin 0' prio=0 status=active
              |- 7:0:0:1 sdc 8:32 active undef running
              `- 6:0:0:1 sdd 8:48 active undef running

        In het volgende voorbeeld ziet u de uitvoer van twee netwerkinterfaces op een StorSimple apparaat verbonden met twee netwerkinterfaces host met vier beschikbare paden.

            mpathb (36486fd27a23feba1b096226f11420f6b) dm-2 MSFT,STORSIMPLE 8100
            size=100G features='0' hwhandler='0' wp=rw
            `-+- policy='round-robin 0' prio=0 status=active
              |- 17:0:0:0 sdb 8:16 active undef running
              |- 15:0:0:0 sdd 8:48 active undef running
              |- 14:0:0:0 sdc 8:32 active undef running
              `- 16:0:0:0 sde 8:64 active undef running

        Nadat de paden zijn geconfigureerd, kunt u verwijzen naar de specifieke instructies voor uw hostbesturingssysteem (6,6 Centos) om te koppelen en dit volume formatteren.


## <a name="troubleshoot-multipathing"></a>Problemen oplossen met meerdere paden gebruiken

Hier vindt u enkele handige tips als u problemen ondervindt tijdens de configuratie van meerdere paden gebruiken.

Q. Ik zie niet de wijzigingen in `multipath.conf` bestand toegepast.

A. Als u aangebrachte wijzigingen in de `multipath.conf` -bestand, moet u de Multipath-service opnieuw starten. Typ de volgende opdracht:

    service multipathd restart

Q. Ik heb twee netwerkinterfaces op het apparaat StorSimple en twee netwerkinterfaces op de host ingeschakeld. Bij het overzicht van de beschikbare paden, zie ik slechts twee paden. Ik verwacht te zien van vier beschikbare paden.

A. Zorg ervoor dat de twee paden zich op hetzelfde subnet en geschikt. Als de netwerkinterfaces op verschillende VLAN's en niet geschikt zijn, worden er slechts twee paden. Er is een manier om dit te controleren om ervoor te zorgen dat u zowel de hostinterfaces van een netwerkinterface op het apparaat StorSimple kunt bereiken. U zal moet [contact opnemen met Microsoft Support](storsimple-contact-microsoft-support.md) als deze controle kan alleen worden uitgevoerd via een support-sessie.

Q. Wanneer ik de lijst beschikbare paden, zie ik geen uitvoer.

A. Doorgaans stelt voor een probleem met de daemon Multipath multipathed paden niet zien en is het waarschijnlijk dat hier een probleem wordt veroorzaakt door de `multipath.conf` bestand.

Zou het ook niet idee dat daadwerkelijk sommige schijven zien na het maken van verbinding met het doel, als geen antwoord van de aanbiedingen van meerdere paden kan ook betekenen hoeft u niet alle schijven.

- Gebruik de volgende opdracht om het scannen van de SCSI-bus:

    `$ rescan-scsi-bus.sh `(onderdeel van het pakket sg3_utils)

- Typ de volgende opdrachten:

    `$ dmesg | grep sd*`

- Of

    `$ fdisk –l`

    Deze details van recent toegevoegde schijven als resultaat.

- Om te bepalen of het een StorSimple-schijf is, gebruikt u de volgende opdrachten:

    `cat /sys/block/<DISK>/device/model`

    Hiermee wordt een tekenreeks geretourneerd, die bepalen zal of het een schijf StorSimple.

Een minder waarschijnlijk maar een mogelijke oorzaak kan ook worden verouderde iscsid pid. Gebruik de volgende opdracht uit te schakelen vanuit de iSCSI-sessies:

    iscsiadm -m node --logout -p <Target_IP>

Herhaal deze opdracht voor de aangesloten netwerkinterfaces op de iSCSI-doel, het apparaat StorSimple is. Nadat u afgemeld bij alle iSCSI-sessies bent, gebruik van het iSCSI-doel IQN om te herstellen van de iSCSI-sessie. Typ de volgende opdracht:

    iscsiadm -m node --login -T <TARGET_IQN>


Q. Ik niet zeker weet of het apparaat whitelisted is.

A. Om te controleren of het apparaat whitelisted is, gebruik de volgende opdracht voor het oplossen van problemen interactive:

    multipathd –k
    multipathd> show devices
    available block devices:
    ram0 devnode blacklisted, unmonitored
    ram1 devnode blacklisted, unmonitored
    ram2 devnode blacklisted, unmonitored
    ram3 devnode blacklisted, unmonitored
    ram4 devnode blacklisted, unmonitored
    ram5 devnode blacklisted, unmonitored
    ram6 devnode blacklisted, unmonitored
    ram7 devnode blacklisted, unmonitored
    ram8 devnode blacklisted, unmonitored
    ram9 devnode blacklisted, unmonitored
    ram10 devnode blacklisted, unmonitored
    ram11 devnode blacklisted, unmonitored
    ram12 devnode blacklisted, unmonitored
    ram13 devnode blacklisted, unmonitored
    ram14 devnode blacklisted, unmonitored
    ram15 devnode blacklisted, unmonitored
    loop0 devnode blacklisted, unmonitored
    loop1 devnode blacklisted, unmonitored
    loop2 devnode blacklisted, unmonitored
    loop3 devnode blacklisted, unmonitored
    loop4 devnode blacklisted, unmonitored
    loop5 devnode blacklisted, unmonitored
    loop6 devnode blacklisted, unmonitored
    loop7 devnode blacklisted, unmonitored
    sr0 devnode blacklisted, unmonitored
    sda devnode whitelisted, monitored
    dm-0 devnode blacklisted, unmonitored
    dm-1 devnode blacklisted, unmonitored
    dm-2 devnode blacklisted, unmonitored
    sdb devnode whitelisted, monitored
    sdc devnode whitelisted, monitored
    dm-3 devnode blacklisted, unmonitored


Ga [voor probleemoplossing interactieve opdracht voor meerdere paden gebruiken](http://www.centos.org/docs/5/html/5.1/DM_Multipath/multipath_config_confirm.html)voor meer informatie.

## <a name="list-of-useful-commands"></a>Lijst met handige opdrachten

|Type|Opdracht|Beschrijving|
|---|---|---|
|**iSCSI**|`service iscsid start`|ISCSI-service starten|
|&nbsp;|`service iscsid stop`|ISCSI-service stoppen|
|&nbsp;|`service iscsid restart`|ISCSI-service opnieuw starten|
|&nbsp;|`iscsiadm -m discovery -t sendtargets -p <TARGET_IP>`|Ontdek de beschikbare doelen op het opgegeven adres|
|&nbsp;|`iscsiadm -m node --login -T <TARGET_IQN>`|Log in op het iSCSI-doel|
|&nbsp;|`iscsiadm -m node --logout -p <Target_IP>`|Afmelden van de iSCSI-doel|
|&nbsp;|`cat /etc/iscsi/initiatorname.iscsi`|Naam van de iSCSI-initiator afdrukken|
|&nbsp;|`iscsiadm –m session –s <sessionid> -P 3`|Controleer de status van het volume op de host worden ontdekt en iSCSI-sessie|
|&nbsp;|`iscsi –m session`|Toont alle iSCSI-sessies tot stand gebracht tussen de host en de StorSimple apparaat|
| | | |
|**Meerdere paden gebruiken**|`service multipathd start`|Multipath-daemon starten|
|&nbsp;|`service multipathd stop`|Multipath-daemon stoppen|
|&nbsp;|`service multipathd restart`|Multipath-daemon starten|
|&nbsp;|`chkconfig multipathd on` </br> OR </br> `mpathconf –with_chkconfig y`|Multipath-daemon start tijdens het opstarten inschakelen|
|&nbsp;|`multipathd –k`|Start de interactieve console voor het oplossen van problemen|
|&nbsp;|`multipath –l`|Lijst met meerdere paden verbindingen en apparaten|
|&nbsp;|`mpathconf --enable`|Een voorbeeld van mulitpath.conf in een bestand maken`/etc/mulitpath.conf`|
||||

## <a name="next-steps"></a>Volgende stappen

Als u MPIO op Linux host configureren wilt, moet u mogelijk verwijzen naar de volgende CentoS 6.6 documenten:

- [Het instellen van MPIO op CentOS](http://www.centos.org/docs/5/html/5.1/DM_Multipath/setup_procedure.html)
- [Linux-opleiding gids](http://linux-training.be/files/books/LinuxAdm.pdf)
