<properties
    pageTitle="Een cluster van MariaDB (MySQL) uitgevoerd op Azure"
    description="Maak een MariaDB + Galera MySQL cluster Azure virtuele Machines"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="sabbour"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure-services"
    ms.date="04/15/2015"
    ms.author="v-ahsab"/>

# <a name="mariadb-mysql-cluster---azure-tutorial"></a>MariaDB (MySQL), cluster - Azure zelfstudie

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

> [AZURE.NOTE]  MariaDB Enterprise cluster is nu beschikbaar in de markt Azure.  Het nieuwe aanbod wordt automatisch een cluster MariaDB Galera op ARM implementeren. Moet u het nieuwe aanbod van https://azure.microsoft.com/en-us/marketplace/partners/mariadb/cluster-maxscale/ 

We creëren een cluster met meerdere masters [Galera](http://galeracluster.com/products/) [MariaDBs](https://mariadb.org/en/about/), een robuuste, schaalbare en betrouwbare onverwachte vervanging voor MySQL werken in een omgeving met hoge beschikbaarheid op Azure virtuele Machines.

## <a name="architecture-overview"></a>Architectuur-overzicht

In dit onderwerp worden de volgende stappen uitgevoerd:

1. Maak een cluster 3-knooppunt
2. De gegevensschijven worden gescheiden van de OS-schijf
3. De schijven gegevens in RAID 0 striped/instelling verhogen IOP's maken
4. De Load Balancer voor Azure voor een evenwichtige verdeling van de 3 knooppunten gebruiken
5. Om te minimaliseren herhaalde werk, een VM-afbeelding met MariaDB + Galera maken en het andere cluster VMs maken.

![Architectuur](./media/virtual-machines-linux-classic-mariadb-mysql-cluster/Setup.png)

> [AZURE.NOTE]  In dit onderwerp wordt de [Azure CLI](../xplat-cli-install.md) -hulpprogramma's, dus zorg ervoor dat u deze downloaden en deze koppelen aan uw abonnement Azure volgens de instructies. Als u een verwijzing naar de opdrachten die beschikbaar zijn in de CLI Azure moet, kijk dan eens deze link voor de [Azure CLI-opdrachten-naslag](../virtual-machines-command-line-tools.md). U ook nodig hebt voor het [maken van een SSH-sleutel voor verificatie] en noteer de **locatie van het bestand .pem**.


## <a name="creating-the-template"></a>De sjabloon maken

### <a name="infrastructure"></a>Infrastructuur

1. Maak een groep affiniteit voor de bronnen samen

        azure account affinity-group create mariadbcluster --location "North Europe" --label "MariaDB Cluster"

2. Een virtueel netwerk maken

        azure network vnet create --address-space 10.0.0.0 --cidr 8 --subnet-name mariadb --subnet-start-ip 10.0.0.0 --subnet-cidr 24 --affinity-group mariadbcluster mariadbvnet

3. Maak een Account opslag als host voor alle onze schijven. Opmerking dat u niet moet worden het plaatsen van meer dan 40 intensief gebruikt schijven op dezelfde Account opslag te voorkomen kunt u door de 20.000 IOP's account opslaglimiet. In dit geval we niet vanaf dit nummer zodat we alles op dezelfde account voor eenvoud gaat opslaan

        azure storage account create mariadbstorage --label mariadbstorage --affinity-group mariadbcluster

3. Zoek de naam van de afbeelding CentOS 7 virtuele Machine

        azure vm image list | findstr CentOS
Dit wordt als uitvoer `5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-70-20140926`. De naam gebruiken in de volgende stap.

4. De **/path/to/key.pem** vervangen door het pad waar u de gegenereerde .pem SSH-sleutel opgeslagen VM-sjabloon maken

        azure vm create --virtual-network-name mariadbvnet --subnet-names mariadb --blob-url "http://mariadbstorage.blob.core.windows.net/vhds/mariadbhatemplate-os.vhd"  --vm-size Medium --ssh 22 --ssh-cert "/path/to/key.pem" --no-ssh-password mariadbtemplate 5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-70-20140926 azureuser

5. 4 x 500GB data schijven koppelen aan de VM voor gebruik in de RAID-configuratie

        FOR /L %d IN (1,1,4) DO azure vm disk attach-new mariadbhatemplate 512 http://mariadbstorage.blob.core.windows.net/vhds/mariadbhatemplate-data-%d.vhd

6. SSH naar de sjabloon VM die u hebt gemaakt op de **mariadbhatemplate.cloudapp.net:22** en verbinding maken met uw persoonlijke sleutel.

### <a name="software"></a>Software

1. Verkrijgen root

        sudo su

2. Ondersteuning voor RAID installeren:

     - Mdadm installeren

                yum install mdadm

     - De RAID 0 stripe/configuratie maken met een EXT4-bestandssysteem

                mdadm --create --verbose /dev/md0 --level=stripe --raid-devices=4 /dev/sdc /dev/sdd /dev/sde /dev/sdf
                mdadm --detail --scan >> /etc/mdadm.conf
                mkfs -t ext4 /dev/md0

     - De koppelpuntmap maken

                mkdir /mnt/data

     - Ophalen van de UUID van de nieuwe RAID-apparaat

                blkid | grep /dev/md0

     - /Etc/fstab bewerken

                vi /etc/fstab

     - Toevoegen van het apparaat er mouting automatisch tijdens het opstarten vervangen door de waarde van de opdracht **blkid** voordat de UUID inschakelen

                UUID=<UUID FROM PREVIOUS>   /mnt/data ext4   defaults,noatime   1 2

     - Koppel de nieuwe partitie

                mount /mnt/data

3. MariaDB installeren:

     - Maak het bestand MariaDB.repo:

                vi /etc/yum.repos.d/MariaDB.repo

     - Vullen met de onderstaande inhoud

                [mariadb]
                name = MariaDB
                baseurl = http://yum.mariadb.org/10.0/centos7-amd64
                gpgkey=https://yum.mariadb.org/RPM-GPG-KEY-MariaDB
                gpgcheck=1

     - Bestaande postfix-mariadb-libs om conflicten te voorkomen en verwijderen

            yum remove postfix mariadb-libs-*

     - MariaDB met Galera installeren

            yum install MariaDB-Galera-server MariaDB-client galera

4. De map MySQL gegevens verplaatsen naar het schijfstation RAID

     - De huidige MySQL map kopiëren naar de nieuwe locatie en de oude map verwijderen

            cp -avr /var/lib/mysql /mnt/data  
            rm -rf /var/lib/mysql

     - Machtigingen voor de nieuwe map dienovereenkomstig instellen

            chown -R mysql:mysql /mnt/data && chmod -R 755 /mnt/data/  

     - Maak een symlink naar de oude map naar de nieuwe locatie op de RAID-partitie

            ln -s /mnt/data/mysql /var/lib/mysql

5. Omdat [dat SELinux clusterbewerkingen verstoort](http://galeracluster.com/documentation-webpages/configuration.html#selinux), is het noodzakelijk te schakelen voor de huidige sessie (totdat u een compatibele versie wordt weergegeven). Bewerken `/etc/selinux/config` wilt uitschakelen voor latere opnieuw wordt opgestart:

            setenforce 0

       then editing `/etc/selinux/config` to set `SELINUX=permissive`

6. Valideren van MySQL wordt uitgevoerd

    - Start de MySQL

            service mysql start

    - De installatie van MySQL veilig, het root-wachtwoord instellen, anonieme gebruikers externe hoofdmap aanmelding uitschakelen en het verwijderen van de testdatabase verwijderen

            mysql_secure_installation

    - Een gebruiker van de database voor clusterbewerkingen en eventueel uw toepassingen maken

            mysql -u root -p
            GRANT ALL PRIVILEGES ON *.* TO 'cluster'@'%' IDENTIFIED BY 'p@ssw0rd' WITH GRANT OPTION; FLUSH PRIVILEGES;
            exit

   - MySQL stoppen

            service mysql stop

7. Configuratie maken

    - De MySQL configuratie te maken van een tijdelijke aanduiding voor de clusterinstellingen bewerken. Geen vervanging voor de **`<Vairables>`** of opmerking nu verwijderen. Dat gebeurt nadat we een VM maken op basis van deze sjabloon.

            vi /etc/my.cnf.d/server.cnf

    - De sectie **[galera]** bewerken en maak het

    - Bewerken van de sectie **[mariadb]**

            wsrep_provider=/usr/lib64/galera/libgalera_smm.so
            binlog_format=ROW
            wsrep_sst_method=rsync
            bind-address=0.0.0.0 # When set to 0.0.0.0, the server listens to remote connections
            default_storage_engine=InnoDB
            innodb_autoinc_lock_mode=2

            wsrep_sst_auth=cluster:p@ssw0rd # CHANGE: Username and password you created for the SST cluster MySQL user
            #wsrep_cluster_name='mariadbcluster' # CHANGE: Uncomment and set your desired cluster name
            #wsrep_cluster_address="gcomm://mariadb1,mariadb2,mariadb3" # CHANGE: Uncomment and Add all your servers
            #wsrep_node_address='<ServerIP>' # CHANGE: Uncomment and set IP address of this server
            #wsrep_node_name='<NodeName>' # CHANGE: Uncomment and set the node name of this server

8. Open de vereiste poorten op de firewall (met FirewallD op CentOS-7)

    - MySQL:`firewall-cmd --zone=public --add-port=3306/tcp --permanent`
    - GALERA:`firewall-cmd --zone=public --add-port=4567/tcp --permanent`
    - GALERA IST:`firewall-cmd --zone=public --add-port=4568/tcp --permanent`
    - RSYNC:`firewall-cmd --zone=public --add-port=4444/tcp --permanent`
    - Opnieuw laden de firewall:`firewall-cmd --reload`

9.  Het systeem voor de prestaties optimaliseren. Raadpleeg dit artikel over de [strategie voor het afstemmen van prestaties](virtual-machines-linux-classic-optimize-mysql.md) voor meer informatie

    - Het configuratiebestand voor MySQL opnieuw bewerken

            vi /etc/my.cnf.d/server.cnf

    - De sectie **[mariadb]** bewerken en voeg de volgende

    > [AZURE.NOTE] Het is raadzaam om **innodb\_buffer\_pool_size** 70% van het geheugen van uw VM. Instelling van 2,45 GB hier voor Azure VM gemiddeld met 3,5 GB RAM.

            innodb_buffer_pool_size = 2508M # The buffer pool contains buffered data and the index. This is usually set to 70% of physical memory.
            innodb_log_file_size = 512M #  Redo logs ensure that write operations are fast, reliable, and recoverable after a crash
            max_connections = 5000 # A larger value will give the server more time to recycle idled connections
            innodb_file_per_table = 1 # Speed up the table space transmission and optimize the debris management performance
            innodb_log_buffer_size = 128M # The log buffer allows transactions to run without having to flush the log to disk before the transactions commit
            innodb_flush_log_at_trx_commit = 2 # The setting of 2 enables the most data integrity and is suitable for Master in MySQL cluster
            query_cache_size = 0

10. MySQL stoppen, uitschakelen van MySQL-service uit te voeren bij het opstarten om te voorkomen dat het cluster als u een nieuw knooppunt toevoegt en deprovision van de machine.

        service mysql stop
        chkconfig mysql off
        waagent -deprovision

11. Vastleggen van de VM via de portal. (Op dit moment extra [1268 # in de CLI Azure probleem] wordt beschreven het feit dat afbeeldingen die zijn vastgelegd door de Azure CLI hulpmiddelen niet de gegevensschijven van de gekoppelde vastlegt.)

    - Afsluiten van de computer via de portal
    - Klik op opname en geeft u de naam van de afbeelding als **mariadb-galera-afbeelding** en een beschrijving en controleren "Ik hebben waagent uitvoeren".
    ![De virtuele Machine vastleggen](./media/virtual-machines-linux-classic-mariadb-mysql-cluster/Capture.png)
    ![de virtuele Machine vastleggen](./media/virtual-machines-linux-classic-mariadb-mysql-cluster/Capture2.PNG)

## <a name="creating-the-cluster"></a>Het cluster maken

3 VMs uit de sjabloon die u zojuist hebt gemaakt en vervolgens configureren en starten van de cluster maken.

1. Maken van de eerste CentOS 7 VM uit de **mariadb-galera-image** -image die u hebt gemaakt, mits het virtuele netwerk naam **mariadbvnet** en de **mariadb**van het subnet van de computer formaat **Medium**, doorgeven in de Cloud-Service de naam worden **mariadbha** (of welke naam u wilt worden benaderd via mariadbha.cloudapp.net), instellen van de naam van deze machine worden **mariadb1** en de gebruikersnaam zijn **azureuser**en SSH toegang en het doorgeven van de SSH-certificaat .pem bestands- en **/path/to/key.pem** door het pad vervangen waar u de gegenereerde .pem SSH-sleutel wordt opgeslagen.

    > [AZURE.NOTE] De volgende opdrachten zijn verdeeld over meerdere lijnen voor de duidelijkheid, maar u moet elk als één regel invoeren.

        azure vm create
        --virtual-network-name mariadbvnet
        --subnet-names mariadb
        --availability-set clusteravset
        --vm-size Medium
        --ssh-cert "/path/to/key.pem"
        --no-ssh-password
        --ssh 22
        --vm-name mariadb1
        mariadbha mariadb-galera-image azureuser

2. 2 meer virtuele Machines maken _door_ ze aan de huidige gemaakte **mariadbha** Cloud-Service de **VM de naam** als de **SSH-poort** met een unieke poort niet strijdig zijn met andere VMs in dezelfde Cloud-Service wijzigen.

        azure vm create
        --virtual-network-name mariadbvnet
        --subnet-names mariadb
        --availability-set clusteravset
        --vm-size Medium
        --ssh-cert "/path/to/key.pem"
        --no-ssh-password
        --ssh 23
        --vm-name mariadb2
        --connect mariadbha mariadb-galera-image azureuser
en voor MariaDB3

        azure vm create
        --virtual-network-name mariadbvnet
        --subnet-names mariadb
        --availability-set clusteravset
        --vm-size Medium
        --ssh-cert "/path/to/key.pem"
        --no-ssh-password
        --ssh 24
        --vm-name mariadb3
        --connect mariadbha mariadb-galera-image azureuser

3. U moet het interne IP-adres van elk van de 3 VMs ophalen voor de volgende stap:

    ![IP-adres ophalen](./media/virtual-machines-linux-classic-mariadb-mysql-cluster/IP.png)

4. SSH in de 3 VMs en en bewerk het configuratiebestand op elke

        sudo vi /etc/my.cnf.d/server.cnf

    uncommenting **`wsrep_cluster_name`** en **`wsrep_cluster_address`** door de **#** aan het begin en validatie zijn inderdaad wat u wilt.
    Bovendien vervangen door **`<ServerIP>`** in **`wsrep_node_address`** en **`<NodeName>`** in **`wsrep_node_name`** van de VM-IP-adres en naam respectievelijk en opmerkingen bij deze regels ook.

5. Start de cluster op MariaDB1 en laat deze uitvoeren bij het opstarten

        sudo service mysql bootstrap
        chkconfig mysql on

6. Start MySQL op MariaDB2 en MariaDB3 en laat deze uitvoeren bij het opstarten

        sudo service mysql start
        chkconfig mysql on

## <a name="load-balancing-the-cluster"></a>Load balancing het cluster
Als u de geclusterde VMs hebt gemaakt, kunt u deze in een beschikbaarheid instellen **clusteravset** aangeroepen om te garanderen dat zij op verschillende veroorzaakt worden gebracht en domeinen en dat Azure nooit onderhoud op alle computers tegelijk heeft bijwerken toegevoegd. Deze configuratie voldoet aan de eisen moet worden ondersteund door die Azure Service niveau overeenkomst (SLA).

Nu kunt u de taakverdelingsvoorziening Azure aanvragen tussen onze 3 knooppunten in evenwicht.

Voer de volgende opdrachten op de computer met behulp van de CLI Azure.
De opdracht parameters-structuur is:`azure vm endpoint create-multiple <MachineName> <PublicPort>:<VMPort>:<Protocol>:<EnableDirectServerReturn>:<Load Balanced Set Name>:<ProbeProtocol>:<ProbePort>`

    azure vm endpoint create-multiple mariadb1 3306:3306:tcp:false:MySQL:tcp:3306
    azure vm endpoint create-multiple mariadb2 3306:3306:tcp:false:MySQL:tcp:3306
    azure vm endpoint create-multiple mariadb3 3306:3306:tcp:false:MySQL:tcp:3306

Ten slotte, omdat de load balancer sonde interval de CLI ingesteld op 15 seconden (die mogelijk iets te lang), deze wijzigen in de portal onder de **eindpunten** voor elk van de VMs

![Eindpunt bewerken](./media/virtual-machines-linux-classic-mariadb-mysql-cluster/Endpoint.PNG)

Klik vervolgens op Reconfigure The Load-Balanced instellen en naartoe?

![RECONFIGURE evenwichtige Set laden](./media/virtual-machines-linux-classic-mariadb-mysql-cluster/Endpoint2.PNG)

Wijzig vervolgens de Interval Probe in 5 seconden en opslaan

![Sonde-Interval wijzigen](./media/virtual-machines-linux-classic-mariadb-mysql-cluster/Endpoint3.PNG)

## <a name="validating-the-cluster"></a>De cluster te valideren

Het harde werk is gedaan. Het cluster wordt nu beschikbaar op `mariadbha.cloudapp.net:3306` die wordt druk op de taakverdeling en doorsturen van aanvragen tussen de 3 VMs soepeler en efficiënter.

Gebruik uw favoriete MySQL-client verbinding te maken of gewoon verbinding maken met een van de VMs te controleren of dat dit cluster werkt.

     mysql -u cluster -h mariadbha.cloudapp.net -p

Vervolgens een nieuwe database maken en deze te vullen met gegevens

    CREATE DATABASE TestDB;
    USE TestDB;
    CREATE TABLE TestTable (id INT NOT NULL AUTO_INCREMENT PRIMARY KEY, value VARCHAR(255));
    INSERT INTO TestTable (value)  VALUES ('Value1');
    INSERT INTO TestTable (value)  VALUES ('Value2');
    SELECT * FROM TestTable;

Resulteert in de volgende tabel.

    +----+--------+
  	| id | value  |
    +----+--------+
  	|  1 | Value1 |
  	|  4 | Value2 |
    +----+--------+
    2 rows in set (0.00 sec)

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Volgende stappen

In dit artikel, die u hebt gemaakt een 3 knooppunt MariaDB + Galera maximaal beschikbare cluster op Azure virtuele Machines met 7 CentOS. Het VMs worden verdeeld met taakverdeling Azure.

U wilt bekijken op [een andere manier met MySQL op Linux cluster](virtual-machines-linux-classic-mysql-cluster.md) en manieren om te [optimaliseren en testen van de prestaties MySQL op Azure Linux VMs](virtual-machines-linux-classic-optimize-mysql.md).

<!--Anchors-->
[Architecture overview]: #architecture-overview
[Creating the template]: #creating-the-template
[Creating the cluster]: #creating-the-cluster
[Load balancing the cluster]: #load-balancing-the-cluster
[Validating the cluster]: #validating-the-cluster
[Next steps]: #next-steps

<!--Image references-->

<!--Link references-->
[Galera]: http://galeracluster.com/products/
[MariaDBs]: https://mariadb.org/en/about/
[Maak een SSH-sleutel voor verificatie]:http://www.jeff.wilcox.name/2013/06/secure-linux-vms-with-ssh-certificates/
[probleem #1268 in de CLI Azure]:https://github.com/Azure/azure-xplat-cli/issues/1268
