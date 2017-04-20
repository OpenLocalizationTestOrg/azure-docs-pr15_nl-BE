<properties
    pageTitle="MySQL clusterize met taakverdeling sets | Microsoft Azure"
    description="Een evenwichtige taakverdeling hoge beschikbaarheid Linux MySQL cluster met de klassieke implementatiemodel op Azure gemaakt instellen"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="bureado"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="04/14/2015"
    ms.author="jparrel"/>

# <a name="using-load-balanced-sets-to-clusterize-mysql-on-linux"></a>Taakverdeling sets gebruiken om MySQL op Linux clusterize

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Het doel van dit artikel is om te verkennen en te illustreren de verschillende methoden beschikbaar voor het implementeren van maximaal beschikbare Linux-gebaseerde services op Microsoft Azure, hoge beschikbaarheid van de MySQL-Server als een primer verkennen. Een video ter illustratie van deze benadering is beschikbaar op [Channel 9](http://channel9.msdn.com/Blogs/Open/Load-balancing-highly-available-Linux-services-on-Windows-Azure-OpenLDAP-and-MySQL).

Overzicht van een gedeelde twee knooppunten single-master MySQL hoge beschikbaarheid-oplossing op basis van DRBD, Corosync en pacemaker heeft. Slechts één knooppunt actief is op een moment dat MySQL. Lezen en schrijven van de resource DRBD is ook beperkt tot slechts één knooppunt tegelijk.

Er is niet nodig voor een VIP-oplossing zoals de LVS omdat we van Microsoft Azure Load Balanced Sets gebruiken voor zowel de functie round-robin en het eindpunt detectie, verwijdering en gecontroleerde herstelprocedure van de VIP. De VIP is een globaal routeerbaar IPv4-adres toegewezen door Microsoft Azure wanneer we eerst de cloud-service maken.

Er zijn andere mogelijke architectuur voor MySQL, inclusief Next Business Day-Cluster, Percona en Galera, evenals verschillende middleware-oplossingen, waaronder ten minste één beschikbaar als een VM op [VM Depot](http://vmdepot.msopentech.com). Als deze oplossingen op unicast en multicast- of broadcast repliceren kunnen en Vertrouw niet op gedeelde opslag of meerdere netwerkinterfaces, zijn de scenario's moet eenvoudig te implementeren op Microsoft Azure.

Deze clustering architecturen kunnen natuurlijk worden uitgebreid tot andere producten zoals PostgreSQL en OpenLDAP op soortgelijke wijze. Bijvoorbeeld deze procedure voor taakverdeling met gedeelde is met succes getest met meerdere master OpenLDAP en u kunt deze bekijken op onze blog Channel 9.

## <a name="getting-ready"></a>Voorbereiden

U moet een Microsoft Azure-account met een geldig abonnement kunnen maken van ten minste twee (2) VMs (XS is gebruikt in dit voorbeeld), een netwerk en een subnet een groep affiniteit en de beschikbaarheid van een ingesteld, en ook de mogelijkheid nieuwe VHD's in hetzelfde gebied, als de service cloud maken en deze koppelen aan de Linux VMs.

### <a name="tested-environment"></a>Geteste omgeving

- 13.10 Ubuntu
  - DRBD
  - MySQL-Server
  - Corosync en pacemaker heeft

### <a name="affinity-group"></a>Affiniteit groep

Een groep affiniteit voor de oplossing wordt gemaakt door in Azure klassieke portal schuiven naar de instellingen voor logboekregistratie en het maken van een nieuwe groep met affiniteit. Toegewezen bronnen gemaakt later wordt toegewezen aan deze groep affiniteit.

### <a name="networks"></a>Netwerken

Een nieuw netwerk wordt gemaakt en een subnet binnen het netwerk wordt gemaakt. Wij hebben gekozen voor een 10.10.10.0/24 netwerk met slechts één /24 subnet binnen.

### <a name="virtual-machines"></a>Virtuele machines

De eerste Ubuntu 13.10 VM is gemaakt met behulp van een galerijafbeelding Endorsed Ubuntu en genoemd `hadb01`. Een nieuwe wolk service is gemaakt in het proces hadb genoemd. Wij bellen op deze manier om te illustreren de gedeelde, taakverdeling aard die de service hebben zal wanneer we meer bronnen toe te voegen. Het maken van `hadb01` wordt de probleemloze ingevuld met behulp van de portal. Een eindpunt voor SSH wordt automatisch gemaakt en ons netwerk gemaakt is geselecteerd. We ook kiezen voor het maken van een nieuwe beschikbaarheid voor de VMs instellen.

Zodra de eerste VM is gemaakt (technisch, wanneer de service cloud wordt gemaakt) we doorgaan met het maken van de tweede VM `hadb02`. Voor de tweede VM ook gebruiken we Ubuntu 13.10 VM uit de galerie met behulp van de portal, maar kiezen wij gebruik van een bestaande service cloud, `hadb.cloudapp.net`, in plaats van een nieuwe maken. Het netwerk en de beschikbaarheid van de set automatisch geselecteerd voor ons. Een eindpunt SSH wordt gemaakt, te.

Nadat beide VMs zijn gemaakt, zullen we nemen nota van de SSH-poort voor `hadb01` (TCP-22) en `hadb02` (automatisch toegewezen door Azure)

### <a name="attached-storage"></a>Gekoppelde opslag

We een nieuwe schijf toevoegen aan beide VMs en nieuwe 5 GB schijven maken in het proces. De schijven worden in de container VHD in gebruik voor onze belangrijkste besturingssysteem schijven worden gehost. Nadat de schijven zijn gemaakt en gekoppeld is niet nodig voor ons opnieuw op te starten Linux als de kernel het nieuwe apparaat ziet (meestal `/dev/sdc`, kunt u controleren `dmesg` voor de uitvoer)

Op elke VM die we doorgaan met het maken van een nieuwe partitie met `cfdisk` (primair, Linux-partitie) en schrijf de nieuwe partitietabel. **Een bestandssysteem op deze partitie niet maken** .

## <a name="setting-up-the-cluster"></a>Instellen van het cluster

We moeten in beide VMs Ubuntu APT gebruiken voor het installeren van Corosync, pacemaker heeft en DRBD. Met behulp van `apt-get`:

    sudo apt-get install corosync pacemaker drbd8-utils.

**Installeer MySQL op dit moment niet** . Debian en Ubuntu installatiescripts zal een map MySQL gegevens initialiseren op `/var/lib/mysql`, maar omdat de map zal worden vervangen door een DRBD-bestandssysteem, moeten we dit later doen.

Nu we ook moeten controleren (met behulp van `/sbin/ifconfig`) dat beide VMs adressen in het subnet 10.10.10.0/24, en dat zij elkaar op naam kunnen pingen gebruikt. Indien gewenst kunt u ook gebruiken `ssh-keygen` en `ssh-copy-id` om ervoor te zorgen dat beide VMs kunnen communiceren via SSH zonder een wachtwoord.

### <a name="setting-up-drbd"></a>DRBD instellen

Maken we een DRBD-bron die gebruikmaakt van de onderliggende `/dev/sdc1` partitie voor de productie van een `/dev/drbd1` resource kunnen worden opgemaakt met behulp van de ext3 en gebruikt in de primaire en secundaire knooppunten. Open hiertoe `/etc/drbd.d/r0.res` en kopieer de volgende definitie voor de resource. Dit doet u in beide VMs:

    resource r0 {
      on `hadb01` {
        device  /dev/drbd1;
        disk   /dev/sdc1;
        address  10.10.10.4:7789;
        meta-disk internal;
      }
      on `hadb02` {
        device  /dev/drbd1;
        disk   /dev/sdc1;
        address  10.10.10.5:7789;
        meta-disk internal;
      }
    }

Nadat u dit doet, initialiseren van de resource met behulp van `drbdadm` in beide VMs:

    sudo drbdadm -c /etc/drbd.conf role r0
    sudo drbdadm up r0

En ten slotte op de primaire (`hadb01`) (primair) eigendom van de resource DRBD afdwingen:

    sudo drbdadm primary --force r0

Als u de inhoud van/proc/drbd onderzoeken (`sudo cat /proc/drbd`) in beide VMs, worden er `Primary/Secondary` op `hadb01` en `Secondary/Primary` op `hadb02`, in overeenstemming met de oplossing op dit moment. De 5 GB schijf gesynchroniseerd via het netwerk 10.10.10.0/24 gratis aan klanten.

Nadat de schijf is gesynchroniseerd kunt u het bestandssysteem op `hadb01`. Voor testdoeleinden we ext2 gebruikt, maar met de volgende instructie maakt u een ext3 bestandssysteem:

    mkfs.ext3 /dev/drbd1

### <a name="mounting-the-drbd-resource"></a>Montage van de resource DRBD

Op `hadb01` nu gaan we de DRBD resources koppelen. Gebruik van debian en afgeleide `/var/lib/mysql` als de gegevensmap van MySQL. Aangezien we MySQL niet hebt geïnstalleerd, we maken van de map en de bron DRBD koppelen. On `hadb01`:

    sudo mkdir /var/lib/mysql
    sudo mount /dev/drbd1 /var/lib/mysql

## <a name="setting-up-mysql"></a>MySQL instellen

Nu u klaar bent om het installeren van MySQL op `hadb01`:

    sudo apt-get install mysql-server

Voor `hadb02`, hebt u twee opties. U kunt mysql-server installeren nu /var/lib/mysql maken en vullen met een nieuwe gegevensmap en gaat u verder met het verwijderen van de inhoud. On `hadb02`:

    sudo apt-get install mysql-server
    sudo service mysql stop
    sudo rm –rf /var/lib/mysql/*

Failover naar de tweede optie is `hadb02` en installeer vervolgens mysql server er (installatiescripts zullen merken dat de bestaande installatie en heeft geen invloed op het)

On `hadb01`:

    sudo drbdadm secondary –force r0

On `hadb02`:

    sudo drbdadm primary –force r0
    sudo apt-get install mysql-server

Als u geen failover-DRBD nu, is de eerste optie eenvoudiger Hoewel hoewel minder elegant. Nadat u dit hebt u ingesteld, kunt u starten met het werken aan uw MySQL-database. Op `hadb02` (of indien dat een van de servers actief is, volgens DRBD):

    mysql –u root –p
    CREATE DATABASE azureha;
    CREATE TABLE things ( id SERIAL, name VARCHAR(255) );
    INSERT INTO things VALUES (1, "Yet another entity");
    GRANT ALL ON things.\* TO root;

**Waarschuwing**: deze laatste verklaring effectief verificatie voor de hoofdgebruiker in deze tabel is uitgeschakeld. Dit moet worden vervangen door de kwaliteit van uw productie instructies geven en alleen ter illustratie is opgenomen.

U moet ook inschakelen van toegang voor MySQL als u wilt query's van buiten het VMs, dat het doel van deze handleiding is. Open op beide VMs, `/etc/mysql/my.cnf` en Ga naar `bind-address`, het wijzigen van 127.0.0.1 op 0.0.0.0. Nadat het bestand is opgeslagen, geeft een `sudo service mysql restart` op uw huidige primaire.

### <a name="creating-the-mysql-load-balanced-set"></a>Maken van de MySQL-Load Balanced Set

We terug naar de portal en blader naar de `hadb01` VM dan eindpunten. Wij zullen maken van een nieuw eindpunt, MySQL (3306 TCP) kiest in de vervolgkeuzelijst en tik op het vak *maken nieuwe load balanced set* . Noemen we onze taakverdeling eindpunt `lb-mysql`. Gebruikt u de meeste opties alleen, met uitzondering van die we tot en met 5 (seconden, minimum verlagen)

Nadat het eindpunt is gemaakt we Ga naar `hadb02`, eindpunten, en maak een nieuwe eindpunt maar kiezen wij `lb-mysql`, selecteer MySQL in de vervolgkeuzelijst. U kunt ook de CLI Azure voor deze stap.

Op dit moment hebben we alles wat die we nodig hebben voor een handmatige bewerking van het cluster.

### <a name="testing-the-load-balanced-set"></a>Testen van de belasting in evenwicht instellen

Tests kunnen worden uitgevoerd vanaf een externe computer, met behulp van een MySQL-client als toepassingen (bijvoorbeeld phpMyAdmin wordt uitgevoerd als een Website Azure) In dit geval hebben we het opdrachtregelprogramma van MySQL gebruikt in een andere Linux:

    mysql azureha –u root –h hadb.cloudapp.net –e "select * from things;"

### <a name="manually-failing-over"></a>Handmatig failover

U kunt nu failover simuleren door MySQL afgesloten, schakelen tussen DRBD van primaire en MySQL opnieuw te starten.

Op hadb01:

    service mysql stop && umount /var/lib/mysql ; drbdadm secondary r0

Klik vervolgens op hadb02:

    drbdadm primary r0 ; mount /dev/drbd1 /var/lib/mysql && service mysql start

Als u failover handmatig kunt u herhalen uw externe query en deze moeten werken perfect.

## <a name="setting-up-corosync"></a>Corosync instellen

Corosync is de onderliggende cluster infrastructuur vereist voor pacemaker heeft om te werken. Voor Heartbeat v1 en v2-gebruikers (en andere methoden zoals Ultramonkey) Corosync is een splitsing van de CRM-functionaliteit, terwijl de pacemaker heeft nog meer lijken op heartbeat in functionaliteit.

De belangrijkste beperking voor Corosync in Azure is dat Corosync multicast boven uitzending via unicast-communicatie verkiest, maar Microsoft Azure netwerken alleen unicast ondersteunt.

Gelukkig Corosync heeft een werkende unicast-modus en de enige echte beperking is dat, omdat alle knooppunten niet onderling *automagically communiceren*, u de knooppunten in de configuratiebestanden moet, met inbegrip van de IP-adressen definiëren. De Corosync voorbeeldbestanden kunt gebruiken voor Unicast en wijzigt adres, knooppunt, lijsten en registratiemap binden (Ubuntu gebruikt `/var/log/corosync` terwijl de voorbeeld gebruik bestanden `/var/log/cluster`) en het inschakelen van extra quorum.

**Opmerking de `transport: udpu` richtlijn hieronder en de handmatig opgegeven IP-adressen voor de knooppunten**.

Op `/etc/corosync/corosync.conf` voor beide knooppunten:

    totem {
      version: 2
      crypto_cipher: none
      crypto_hash: none
      interface {
        ringnumber: 0
        bindnetaddr: 10.10.10.0
        mcastport: 5405
        ttl: 1
      }
      transport: udpu
    }

    logging {
      fileline: off
      to_logfile: yes
      to_syslog: yes
      logfile: /var/log/corosync/corosync.log
      debug: off
      timestamp: on
      logger_subsys {
        subsys: QUORUM
        debug: off
        }
      }

    nodelist {
      node {
        ring0_addr: 10.10.10.4
        nodeid: 1
      }

      node {
        ring0_addr: 10.10.10.5
        nodeid: 2
      }
    }

    quorum {
      provider: corosync_votequorum
    }

We dit configuratiebestand kopieert in beide VMs en Corosync in beide knooppunten te starten:

    sudo service start corosync

Kort na het starten van de service het cluster moet worden vastgesteld in de huidige ring en quorum moet worden gevormd. We kunnen deze functionaliteit logboeken bekijken om te controleren of:

    sudo corosync-quorumtool –l

Voer een resultaat zoals in de onderstaande afbeelding:

![voorbeelduitvoer van corosync-quorumtool - l](media/virtual-machines-linux-classic-mysql-cluster/image001.png)

## <a name="setting-up-pacemaker"></a>Pacemaker heeft instellen

Pacemaker heeft wordt het cluster controleren voor resources, wanneer de primaire kleuren gaat omlaag te definiëren en resources overschakelen naar secundaire servers. Resources kunnen worden gedefinieerd, uit een reeks beschikbare scripts of MSB (init-achtige) scripts onder andere opties.

We willen een pacemaker heeft "eigenaar" de DRBD resource, het koppelpunt en de MySQL-service. Als in- en uitschakelen van DRBD pacemaker heeft inschakelen, te koppelen / umount en starten en stoppen van MySQL in de juiste volgorde wanneer er iets verkeerd gebeurt met de primaire, onze setup is voltooid.

Tijdens de installatie van pacemaker heeft, moet de configuratie zo eenvoudig is, bijvoorbeeld zijn:

    node $id="1" hadb01
      attributes standby="off"
    node $id="2" hadb02
      attributes standby="off"

Dit controleren door het uitvoeren van `sudo crm configure show`. Maak nu een bestand (bijvoorbeeld `/tmp/cluster.conf`) met de volgende bronnen:

    primitive drbd_mysql ocf:linbit:drbd \
          params drbd_resource="r0" \
          op monitor interval="29s" role="Master" \
          op monitor interval="31s" role="Slave"

    ms ms_drbd_mysql drbd_mysql \
          meta master-max="1" master-node-max="1" \
            clone-max="2" clone-node-max="1" \
            notify="true"

    primitive fs_mysql ocf:heartbeat:Filesystem \
          params device="/dev/drbd/by-res/r0" \
          directory="/var/lib/mysql" fstype="ext3"

    primitive mysqld lsb:mysql

    group mysql fs_mysql mysqld

    colocation mysql_on_drbd \
           inf: mysql ms_drbd_mysql:Master

    order mysql_after_drbd \
           inf: ms_drbd_mysql:promote mysql:start

    property stonith-enabled=false

    property no-quorum-policy=ignore

En nu in de configuratie (alleen moet u dit doen in een van de knooppunten) te laden:

    sudo crm configure
      load update /tmp/cluster.conf
      commit
      exit

Zorg er ook voor dat de pacemaker heeft wordt gestart tijdens het opstarten in beide knooppunten:

    sudo update-rc.d pacemaker defaults

Na een paar seconden, en het gebruik van `sudo crm_mon –L`, controleert u of een van de knooppunten is geworden van het model voor het cluster en alle resources is uitgevoerd. Kunt u bevestiging en ps om te controleren of de bronnen worden uitgevoerd.

De volgende schermafbeelding ziet u `crm_mon` met één knooppunt gestopt (afsluiten met CTRL-C)

![crm_mon knooppunt gestopt](media/virtual-machines-linux-classic-mysql-cluster/image002.png)

En deze screenshot toont beide knooppunten, met één master en één slave:

![crm_mon operationele master/slave](media/virtual-machines-linux-classic-mysql-cluster/image003.png)

## <a name="testing"></a>Testen

We zijn klaar voor de simulatie van een automatische failover. Er zijn twee manieren om dit te doen: zachte en harde. De zachte manier afsluiten-functie van het cluster wordt gebruikt: ``crm_standby -U `uname -n` -v on``. Met dit in het model, zal de slave overnemen. Denk eraan dat dit weer ingesteld op uit (crm_mon ziet u één knooppunt is anders op stand-by)

De harde is manier afgesloten met de primaire VM (hadb01) via de portal of de (uitvoeringsniveau) op de VM (dat wil zeggen, stilstand, shutdown) wijzigen en vervolgens wij helpen Corosync en pacemaker heeft door signalering van model naar beneden te gaan. Dit kunnen we toetsen (handig voor windows onderhoud) maar we het scenario ook kunt afdwingen door bevriezing NET de VM.

## <a name="stonith"></a>STONITH

Moet het mogelijk om een VM afsluiten via de CLI Azure in plaats van een STONITH script dat een fysiek apparaat bestuurt. U kunt `/usr/lib/stonith/plugins/external/ssh` als een base en STONITH inschakelen in de configuratie van het cluster. Azure CLI globaal moet worden geïnstalleerd en de instellingen/publicatieprofiel voor de gebruiker van het cluster moet worden geladen.

Voorbeeld van code voor de resource beschikbaar op [GitHub](https://github.com/bureado/aztonith). Moet u de configuratie van het cluster wijzigen door het toevoegen van de volgende `sudo crm configure`:

    primitive st-azure stonith:external/azure \
      params hostlist="hadb01 hadb02" \
      clone fencing st-azure \
      property stonith-enabled=true \
      commit

**Opmerking:** het script niet omhoog/omlaag-controles uitvoeren. De oorspronkelijke bron van SSH had 15 ping controles maar hersteltijd voor een Azure VM kan meer variabele.

## <a name="limitations"></a>Beperkingen

Gelden de volgende beperkingen:

- Het script linbit DRBD resource die DRBD als resource in het gebruik van de pacemaker heeft beheert `drbdadm down` bij het afsluiten van een knooppunt, zelfs als het knooppunt wordt alleen stand-by. Dit is niet ideaal omdat de slave niet de resource DRBD synchroniseert wordt terwijl het model geschreven wordt. Als het model vriendelijk niet mislukken wordt, kan de slave een oudere bestandssysteem staat overnemen. Er zijn twee mogelijke manieren voor het oplossen van dit:
  - Afdwingen van een `drbdadm up r0` in alle knooppunten van het cluster via een lokale watchdog (niet clusterized), of,
  - Ervoor te zorgen dat de linbit DRBD bewerkt script `down` niet wordt aangeroepen, in `/usr/lib/ocf/resource.d/linbit/drbd`.
- Taakverdeling moet ten minste 5 seconden om te reageren, zodat toepassingen clusterdetectie en grotere tolerantie voor time-out; andere architecturen kunnen ook helpen, bijvoorbeeld wachtrijen in app, query middlewares, enz.
- MySQL tuning is geschreven in een gezond tempo en caches worden leeggemaakt naar de schijf zo vaak mogelijk verlies van geheugen minimaliseren
- Prestaties zijn afhankelijk in VM schrijven in de virtual switch interconnect als dit het mechanisme is voor het repliceren van het apparaat door DRBD
