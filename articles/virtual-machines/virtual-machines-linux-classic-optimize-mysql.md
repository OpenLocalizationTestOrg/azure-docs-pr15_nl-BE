<properties
    pageTitle="Optimaliseren van de prestaties van MySQL op Linux VMs | Microsoft Azure"
    description="Informatie over het optimaliseren MySQL op een Azure VM (virtual machine) waarop Linux wordt uitgevoerd."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="NingKuang"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/15/2015"
    ms.author="ningk"/>

#<a name="optimizing-mysql-performance-on-azure-linux-vms"></a>Optimaliseren MySQL prestaties op Azure Linux VMs

Er zijn veel factoren die invloed op de prestaties van MySQL op Azure, zowel in de virtuele selectie- en hardwareconfiguratie. Dit artikel is gericht op optimaliseren prestaties door opslag-, systeem- en database-configuraties.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


##<a name="utilizing-raid-on-an-azure-virtual-machine"></a>Gebruik van RAID op een Azure virtuele machine
Opslag is de belangrijkste factor die van invloed op prestaties van de database in de cloud-omgevingen.  In vergelijking met een enkele schijf, biedt RAID sneller toegang via gelijktijdigheid.  Zie [Standaard-RAID-niveaus](http://en.wikipedia.org/wiki/Standard_RAID_levels) voor meer details.   

I/O schijfdoorvoer- en i/o-responstijd in Azure kunnen worden aanzienlijk verbeterd via RAID. Onze lab tests weergeven schijf I/O doorvoer kan worden verdubbeld en i/o-responstijd kan worden verminderd met de helft gemiddeld wanneer het nummer van de RAID-schijven wordt verdubbeld (van 2 tot 4, 4-8, enz.). Zie [bijlage A](#AppendixA) voor meer informatie.  

Naast schijf I/O verbeterd MySQL prestaties wanneer u het RAID-niveau verhogen.  Zie [Bijlage B](#AppendixB) voor meer informatie.  

U kunt ook rekening houden met de grootte van het segment. In het algemeen wanneer er een groter deel, ontvangt u een lagere overhead, vooral voor grote schrijfopdrachten. Wanneer de grootte van het segment te groot is, kan deze extra overhead toevoegen en u kunt geen gebruikmaken van de RAID. De huidige standaardgrootte is 512KB is optimaal voor de meest algemene productieomgevingen worden bewezen. Zie [Bijlage C](#AppendixC) voor meer informatie.   

Opmerking: Er zijn beperkingen op hoeveel schijven u voor andere virtuele machine toevoegen kunt. Deze beperkingen worden beschreven in de [virtuele Machine en Cloud Service formaten voor Azure](http://msdn.microsoft.com/library/azure/dn197896.aspx). Moet u 4 gekoppelde gegevensschijven te volgen van het RAID-voorbeeld in dit artikel, maar kunt u RAID met minder schijven instellen.  

In dit artikel wordt ervan uitgegaan dat u al een Linux virtuele machine hebt gemaakt en MYSQL is geïnstalleerd en geconfigureerd. Raadpleeg voor meer informatie over de introductie installeren MySQL op Azure.  

###<a name="setting-up-raid-on-azure"></a>RAID op Azure instellen
De volgende stappen laten zien hoe RAID op Azure maken met behulp van de klassieke Azure portal. U kunt ook instellen RAID met behulp van Windows PowerShell-scripts.
In dit voorbeeld zullen we RAID 0 met 4 schijven configureren.  

####<a name="step-1-add-a-data-disk-to-your-virtual-machine"></a>Stap 1: Een schijf met gegevens toevoegen aan uw virtuele Machine  

Klik op de virtuele machine die u wilt toevoegen van een schijf met gegevens van de klassieke Azure portal op de pagina virtuele Machines. In dit voorbeeld wordt de virtuele machine mysqlnode1.  

![][1]

Klik op de pagina voor de virtuele machine op **Dashboard**.  

![][2]


In de taakbalk, klikt u op **toevoegen**.

![][3]

En klik vervolgens op **lege schijf toevoegen**.  

![][4]

Voor gegevensschijven die, moet de **Host-Cache voorkeur** worden ingesteld op **geen**.  

Hiermee wordt een lege schijf in uw virtuele machine toegevoegd. Herhaal deze stap drie keer, zodat er 4 gegevensschijven voor RAID.  

U ziet de toegevoegde stations in de virtuele machine op het berichtenlogboek kernel. Bijvoorbeeld om dit te zien op Ubuntu, gebruikt u de volgende opdracht:  

    sudo grep SCSI /var/log/dmesg

####<a name="step-2-create-raid-with-the-additional-disks"></a>Stap 2: Maak RAID met extra schijven
In dit artikel voor gedetailleerde RAID setup stappen volgen:  

[Software RAID onder Linux configureren](virtual-machines-linux-configure-raid.md)

>[AZURE.NOTE] Als u van het bestandssysteem XFS gebruikmaakt, volg de onderstaande stappen als u RAID hebt gemaakt.

XFS installeren op Debian of Ubuntu Linux munt, gebruikt u de volgende opdracht:  

    apt-get -y install xfsprogs  

Wilt installeren op Fedora XFS, CentOS of RHEL, gebruikt u de volgende opdracht:  

    yum -y install xfsprogs  xfsdump


####<a name="step-3-set-up-a-new-storage-path"></a>Stap 3: Een nieuwe opslag pad instellen
Gebruik de volgende opdracht:  

    root@mysqlnode1:~# mkdir -p /RAID0/mysql

####<a name="step-4-copy-the-original-data-to-the-new-storage-path"></a>Stap 4: De oorspronkelijke gegevens kopiëren naar de nieuwe locatie voor opslag
Gebruik de volgende opdracht:  

    root@mysqlnode1:~# cp -rp /var/lib/mysql/* /RAID0/mysql/

####<a name="step-5-modify-permissions-so-mysql-can-access-read-and-write-the-data-disk"></a>Stap 5: Machtigingen wijzigen zodat MySQL kunt openen (lezen en schrijven) de gegevensschijf
Gebruik de volgende opdracht:  

    root@mysqlnode1:~# chown -R mysql.mysql /RAID0/mysql && chmod -R 755 /RAID0/mysql


##<a name="adjust-the-disk-io-scheduling-algorithm"></a>Het algoritme van schijf-i/o--planning aanpassen
Linux implementeert vier soorten I/O planning algoritmen:  

-   Nooperation-algoritme (geen bewerking)
-   Deadline-algoritme (termijn)
-   Volledig reële queuing algoritme (CFQ)
-   Budget periode algoritme (Anticipatory)  

U kunt verschillende i/o-planners onder verschillende scenario's om prestaties te optimaliseren. In een omgeving volledig willekeurig toegang is er een groot verschil tussen de CFQ en de Deadline algoritmen voor prestaties. Het is raadzaam de MySQL-databaseomgeving om op te stellen termijn voor de stabiliteit. Als er een groot aantal opeenvolgende I/O, kan CFQ schijf-i/o-prestaties verminderen.   

Met Nooperation of Deadline leveren betere prestaties dan de standaard Taakplanner voor SSD-technologie en andere apparatuur.   

Vanuit de kernel 2.5 is het standaard i/o-planning algoritme Deadline. Begin van de kernel 2.6.18, werd CFQ de standaard i/o-planning algoritme.  U kunt deze instelling tijdens het opstarten van de Kernel of dynamisch deze instelling te wijzigen wanneer het systeem wordt uitgevoerd.  

In het volgende voorbeeld laat zien hoe om te controleren en de scheduler standaard ingesteld op de Nooperation-algoritme.  

Voor de distributie van Debian-familie:

###<a name="step-1view-the-current-io-scheduler"></a>Stap 1. weergave huidige I/O scheduler
Gebruik de volgende opdracht:  

    root@mysqlnode1:~# cat /sys/block/sda/queue/scheduler

Ziet u na uitvoer geeft de huidige scheduler.  

    noop [deadline] cfq


###<a name="step-2-change-the-current-device-devsda-of-io-scheduling-algorithm"></a>Stap 2. Het huidige apparaat (/ dev/sda) van I/O algoritme planning wijzigen
Gebruik de volgende opdrachten:  

    azureuser@mysqlnode1:~$ sudo su -
    root@mysqlnode1:~# echo "noop" >/sys/block/sda/queue/scheduler
    root@mysqlnode1:~# sed -i 's/GRUB_CMDLINE_LINUX=""/GRUB_CMDLINE_LINUX_DEFAULT="quiet splash elevator=noop"/g' /etc/default/grub
    root@mysqlnode1:~# update-grub

>[AZURE.NOTE] Instelling voor /dev/sda alleen is niet handig. Deze moet worden ingesteld op alle gegevensschijven waarop de database zich bevindt.  

U ziet de volgende uitvoer, die aangeeft dat de grub.cfg zijn opnieuw gemaakt en dat de standaard-scheduler is bijgewerkt naar Nooperation.  

    Generating grub configuration file ...
    Found linux image: /boot/vmlinuz-3.13.0-34-generic
    Found initrd image: /boot/initrd.img-3.13.0-34-generic
    Found linux image: /boot/vmlinuz-3.13.0-32-generic
    Found initrd image: /boot/initrd.img-3.13.0-32-generic
    Found memtest86+ image: /memtest86+.elf
    Found memtest86+ image: /memtest86+.bin
    done

Voor de distributie Redhat-familie hoeft u alleen de volgende opdracht:   

    echo 'echo noop >/sys/block/sda/queue/scheduler' >> /etc/rc.local

##<a name="configure-system-file-operations-settings"></a>Configureer instellingen bewerkingen
Één beste manier is om de logboekfunctie van atime op het bestandssysteem uitschakelen. Atime is de tijd van laatste toegang bestand. Wanneer een bestand wordt geopend, vastgelegd het bestandssysteem de tijdstempel in het logboek. Deze informatie wordt echter zelden gebruikt. U kunt deze optie uitschakelen als u niet nodig hebt, waarvan de totale schijftijd toegang wordt beperkt.  

Atime logging uitschakelen, moet u het bestand systeem configuratie bestand /etc/ bevinden fstab wijzigen en toevoegen van de optie **noatime** .  

Bijvoorbeeld het vim /etc/fstab bestand bewerken, toevoegen van de noatime zoals hieronder wordt weergegeven.  

    # CLOUD_IMG: This file was created/modified by the Cloud Image build process
    UUID=3cc98c06-d649-432d-81df-6dcd2a584d41       /        ext4   defaults,discard        0 0
    #Add the “noatime” option below to disable atime logging
    UUID="431b1e78-8226-43ec-9460-514a9adf060e"     /RAID0   xfs   defaults,nobootwait, noatime 0 0
    /dev/sdb1       /mnt    auto    defaults,nobootwait,comment=cloudconfig 0       2

Vervolgens opnieuw koppelen het bestandssysteem met de volgende opdracht:  

    mount -o remount /RAID0

Test de gewijzigde resultaat. Opmerking Wanneer u de testbestand wijzigt, wordt de tijd van toegang niet bijgewerkt.  

Voordat het voorbeeld:     

![][5]

Na het voorbeeld:

![][6]

##<a name="increase-the-maximum-number-of-system-handles-for-high-concurrency"></a>Verhoog het maximum aantal verzorgt voor hoge gelijktijdigheid
MySQL is een database met hoge gelijktijdigheid. Het aantal gelijktijdige ingangen is 1024 voor Linux, dat is niet altijd voldoende. **Gebruik de volgende stappen uit om de maximale gelijktijdige grepen van het systeem ter ondersteuning van hoge gelijktijdigheid van MySQL verhogen**.

###<a name="step-1-modify-the-limitsconf-file"></a>Stap 1: Het bestand limits.conf wijzigen
Voeg de volgende vier regels in het bestand /etc/security/limits.conf te verhogen, de maximale toegestane, gelijktijdige grepen. Houd er rekening mee dat 65536 is het hoogste getal dat het systeem kan ondersteunen.   

    * zachte nofile 65536
    * vaste nofile 65536
    * zachte nproc 65536
    * vaste nproc 65536

###<a name="step-2-update-the-system-for-the-new-limits"></a>Stap 2: Het systeem voor de nieuwe grenzen bijwerken
Voer de volgende opdrachten:  

    ulimit -SHn 65536
    ulimit -SHu 65536

###<a name="step-3-ensure-that-the-limits-are-updated-at-boot-time"></a>Stap 3: Zorg ervoor dat de grenzen worden bijgewerkt tijdens het opstarten
Plaats de volgende opstartopdrachten in het bestand /etc/rc.local zodat het van tijdens elke keer opstarten kracht wordt.  

    echo “ulimit -SHn 65536” >>/etc/rc.local
    echo “ulimit -SHu 65536” >>/etc/rc.local

##<a name="mysql-database-optimization"></a>MySQL-database optimaliseren
De strategie voor het afstemmen van prestaties dezelfde kunt u MySQL op Azure als configureren op een computer op gebouwen.  

De belangrijkste i/o-optimalisatie regels zijn:   

-   De cache te vergroten.
-   I/o-responstijd verkorten.  

Optimaliseren MySQL server-instellingen, kunt u het bestand my.cnf, dat het standaard configuratiebestand voor server- en clientcomputers is kunt bijwerken.  

De volgende configuratie-items zijn de belangrijkste factoren die invloed hebben op de prestaties van MySQL:  

-   **innodb_buffer_pool_size**: de buffergroep gebufferde gegevens bevat en de index. Dit wordt meestal ingesteld op 70% van het fysieke geheugen.
-   **innodb_log_file_size**: dit is de grootte van het logboek opnieuw uitvoeren. Met Logboeken opnieuw kunt u ervoor zorgen dat schrijfbewerkingen snelle, betrouwbare en terug te vorderen na een systeemstoring zijn. Dit is ingesteld op 512MB, zodat u voldoende ruimte voor het vastleggen van schrijfbewerkingen.
-   **max_connections**: soms toepassingen niet sluit verbindingen goed. Een hogere waarde krijgt de server meer tijd te recyclen inactief sinds verbindingen. Het maximum aantal verbindingen is 10000, maar het aanbevolen maximum is 5000.
-   **Innodb_file_per_table**: deze instelling in- of uitschakelen van de mogelijkheid van InnoDB tabellen opslaan in afzonderlijke bestanden. Inschakelen van de optie zal erop toezien dat verschillende geavanceerde beheerbewerkingen efficiënt kunnen worden toegepast. Uit oogpunt van prestaties, kunt het versnellen van de tabel ruimte transmissie en de restanten management prestaties optimaliseren. Zo is de aanbevolen instelling voor deze ingeschakeld.</br>
    Van MySQL 5.6 is standaard ingeschakeld. Er is dus geen actie vereist. Voor andere versies die ouder is dan 5.6, standaardinstellingen is uitgeschakeld. Door deze in te schakelen is vereist. En moet toepassen voordat de gegevens worden geladen, omdat alleen gemaakte tabellen worden beïnvloed.
-   **innodb_flush_log_at_trx_commit**: standaardwaarde is 1, met de scope ingesteld op 0 ~ 2. De standaardwaarde is de meest geschikte optie voor zelfstandige MySQL DB. De instelling van 2 kan de meeste gegevensintegriteit en is geschikt voor Master in MySQL cluster. De instelling 0 kan verlies van gegevens, die invloed op de betrouwbaarheid, in sommige gevallen met betere prestaties en is geschikt voor Slave in MySQL cluster.
-   **Innodb_log_buffer_size**: de logboekbuffer kan transacties uit te voeren zonder het logboek op schijf leegmaken voordat de transacties doorgevoerd. Als er groot binair object of een tekstveld, wordt de cache zeer snel verbruikt en frequente disk I/O wordt geactiveerd. Het is beter de buffergrootte verhogen als Innodb_log_waits staat variabele niet is 0.
-   **query_cache_size**: de beste optie is het uitschakelen van het begin af. Query_cache_size ingesteld op 0 (dit is nu de standaardinstelling in MySQL 5.6) en andere methoden gebruiken om query's te versnellen.  

Zie [Bijlage D](#AppendixD) voor het vergelijken van prestaties na de optimalisatie.


##<a name="turn-on-the-mysql-slow-query-log-for-analyzing-the-performance-bottleneck"></a>De MySQL trage query-logboek voor het analyseren van de bottleneck inschakelen
De MySQL trage query-logboek kunt u de trage query's voor MySQL te identificeren. Na het inschakelen van de MySQL trage query-logboek, kunt u MySQL hulpmiddelen zoals **mysqldumpslow** de bottleneck te identificeren.  

Houd er rekening mee dat dit is niet standaard ingeschakeld. De trage query-logboek inschakelen, kan sommige CPU-bronnen verbruiken. Daarom wordt het aanbevolen dat u de mogelijkheid deze tijdelijk voor het oplossen van knelpunten.

###<a name="step-1-modify-mycnf-file-by-adding-the-following-lines-to-the-end"></a>Stap 1: My.cnf-bestand aanpassen door de volgende regels toe te voegen aan het einde   

    long_query_time = 2
    slow_query_log = 1
    slow_query_log_file = /RAID0/mysql/mysql-slow.log

###<a name="step-2-restart-mysql-server"></a>Stap 2: Mysql-server opnieuw starten
    service  mysql  restart

###<a name="step-3-check-whether-the-setting-is-taking-effect-using-the-show-command"></a>Stap 3: Controleer of de instelling met de opdracht 'show' effect duurt

![][7]   

![][8]

In dit voorbeeld ziet u dat de query wordt vertraagd-functie is ingeschakeld. Vervolgens kunt u het hulpprogramma **mysqldumpslow** vaststellen van knelpunten en optimaliseren, zoals het toevoegen van indexen.





##<a name="appendix"></a>Aanhangsel

De volgende prestaties test voorbeeldgegevens op gerichte testomgeving geproduceerd zijn, zij verstrekken algemene achtergrondinformatie over de trend van de prestaties gegevens met verschillende prestaties afstemmen benaderingen, maar de resultaten kunnen variëren in verschillende versies van milieu of product.

<a name="AppendixA"></a>Bijlage A:  
**Prestaties (IOP's) van de schijf met verschillende RAID-niveaus**


![][9]

**Test-opdrachten:**  

    fio -filename=/path/test -iodepth=64 -ioengine=libaio -direct=1 -rw=randwrite -bs=4k -size=5G -numjobs=64 -runtime=30 -group_reporting -name=test-randwrite

>AZURE. Opmerking: De belasting van deze test gebruikt 64 threads, probeert te bereiken, de bovengrens van RAID.

<a name="AppendixB"></a>Bijlage B:  
**MySQL prestaties (doorvoer) vergelijking met verschillende RAID-niveaus**   
(XFS-bestandssysteem)


![][10]  
![][11]

**Test-opdrachten:**

    mysqlslap -p0ps.123 --concurrency=2 --iterations=1 --number-int-cols=10 --number-char-cols=10 -a --auto-generate-sql-guid-primary --number-of-queries=10000 --auto-generate-sql-load-type=write –engine=innodb

**MySQL prestaties (OLTP) vergelijking met verschillende RAID-niveaus**  
![][12]

**Test-opdrachten:**

    time sysbench --test=oltp --db-driver=mysql --mysql-user=root --mysql-password=0ps.123  --mysql-table-engine=innodb --mysql-host=127.0.0.1 --mysql-port=3306 --mysql-socket=/var/run/mysqld/mysqld.sock --mysql-db=test --oltp-table-size=1000000 prepare

<a name="AppendixC"></a>Bijlage C:   
**Vergelijking van prestaties (IOP's) schijf voor segment van verschillende grootten**  
(XFS-bestandssysteem)


![][13]

**Test-opdrachten:**  

    fio -filename=/path/test -iodepth=64 -ioengine=libaio -direct=1 -rw=randwrite -bs=4k -size=30G -numjobs=64 -runtime=30 -group_reporting -name=test-randwrite
    fio -filename=/path/test -iodepth=64 -ioengine=libaio -direct=1 -rw=randwrite -bs=4k -size=1G -numjobs=64 -runtime=30 -group_reporting -name=test-randwrite  

Opmerking de grootte van het bestand waarmee u deze test is respectievelijk 30 en 1GB, met RAID-XFS 0(4 disks) ve systeem.


<a name="AppendixD"></a>Bijlage D:  
**MySQL prestaties (doorvoer) vergelijking voor en na de optimalisatie**  
(XFS File System)


![][14]

**Test-opdrachten:**

    mysqlslap -p0ps.123 --concurrency=2 --iterations=1 --number-int-cols=10 --number-char-cols=10 -a --auto-generate-sql-guid-primary --number-of-queries=10000 --auto-generate-sql-load-type=write –engine=innodb,misam

**De configuratie-instelling voor standaard- en optmization is als volgt:**

|Parameters |Standaard    |optmization
|-----------|-----------|-----------
|**innodb_buffer_pool_size**    |Geen   |7G
|**innodb_log_file_size**   |5M |512M
|**max_connections**    |100    |5000
|**innodb_file_per_table**  |0  |1
|**innodb_flush_log_at_trx_commit** |1  |2
|**innodb_log_buffer_size** |8M |128M
|**query_cache_size**   |16M    |0


Meer gedetailleerde optimalisatie configuratieparameters, Raadpleeg de officiële instructies mysql.  

[http://dev.mysql.com/doc/refman/5.6/en/innodb-Configuration.HTML](http://dev.mysql.com/doc/refman/5.6/en/innodb-configuration.html)  

[http://dev.mysql.com/doc/refman/5.6/en/innodb-parameters.HTML#sysvar_innodb_flush_method](http://dev.mysql.com/doc/refman/5.6/en/innodb-parameters.html#sysvar_innodb_flush_method)

**Testomgeving**  

|Hardware   |Details
|-----------|-------
|CPU    |AMD Opteron (TM) Processor 4171 HE / 4 cores
|Geheugen |14 GRAM
|schijf   |10G/schijf
|OS |14.04.1 Ubuntu LTS



[1]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-01.png
[2]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-02.png
[3]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-03.png
[4]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-04.png
[5]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-05.png
[6]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-06.png
[7]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-07.png
[8]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-08.png
[9]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-09.png
[10]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-10.png
[11]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-11.png
[12]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-12.png
[13]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-13.png
[14]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-14.png
