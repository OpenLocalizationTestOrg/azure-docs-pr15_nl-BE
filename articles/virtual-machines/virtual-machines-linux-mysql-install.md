<properties
    pageTitle="MySQL op een Linux VM instellen | Microsoft Azure "
    description="Informatie over het installeren van de MySQL-stapel op een Linux virtuele machine (Ubuntu of RedHat-OS) in Azure"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="SuperScottz"
    manager="timlt"
    editor=""
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/01/2016"
    ms.author="mingzhan"/>


#<a name="how-to-install-mysql-on-azure"></a>Het installeren van MySQL op Azure


In dit artikel leert u hoe u kunt installeren en configureren van MySQL op een Azure virtuele machine waarop Linux.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]


##<a name="install-mysql-on-your-virtual-machine"></a>MySQL installeren op uw virtuele machine

> [AZURE.NOTE] U moet een Microsoft Azure virtuele machine waarop Linux voltooien van deze zelfstudie reeds. Zie de [Azure Linux VM-zelfstudie](virtual-machines-linux-quick-create-cli.md) voor het maken en instellen van een Linux VM met `mysqlnode` als de naam van de VM en `azureuser` als de gebruiker voordat u verdergaat.

In dit geval poort 3306 als de MySQL-poort gebruiken.  

Verbinding maken met de Linux VM die u hebt gemaakt via stopverf. Als dit de eerste keer dat u Azure Linux VM gebruikt, het gebruik van stopverf Zie verbinding maken met een VM Linux [hier](virtual-machines-linux-mac-create-ssh-keys.md).

We gebruiken repository-pakket installeren MySQL5.6 als voorbeeld in dit artikel. MySQL5.6 heeft eigenlijk meer verbetering in de prestaties dan MySQL5.5.  Meer informatie [hier](http://www.mysqlperformanceblog.com/2013/02/18/is-mysql-5-6-slower-than-mysql-5-5/).


###<a name="how-to-install-mysql56-on-ubuntu"></a>MySQL5.6 installeren op Ubuntu
We gebruiken Linux VM hier met Ubuntu van Azure.

- Stap 1: Installeer MySQL Server 5.6 overschakelen naar `root` gebruiker:

            #[azureuser@mysqlnode:~]sudo su -

    5.6 mysql-server installeren:

            #[root@mysqlnode ~]# apt-get update
            #[root@mysqlnode ~]# apt-get -y install mysql-server-5.6

    U ziet een dialoogvenster venster poping tot vragen u MySQL root wachtwoord hieronder in te stellen, en u moet het wachtwoord hier instellen tijdens de installatie.

    ![afbeelding](./media/virtual-machines-linux-mysql-install/virtual-machines-linux-install-mysql-p1.png)


    Geef het wachtwoord ter bevestiging opnieuw op.

    ![afbeelding](./media/virtual-machines-linux-mysql-install/virtual-machines-linux-install-mysql-p2.png)

- Stap 2: Login MySQL-Server

    Wanneer de installatie van MySQL server is voltooid, wordt MySQL-service automatisch gestart. U kunt zich aanmelden met MySQL-Server `root` gebruiker.
    Gebruik de onderstaande opdracht voor het wachtwoord voor aanmelding en input.

             #[root@mysqlnode ~]# mysql -uroot -p

- Stap 3: De actieve MySQL-service beheren

    (a) de status van MySQL service ophalen

             #[root@mysqlnode ~]# service mysql status

    (b) MySQL-Service starten

             #[root@mysqlnode ~]# service mysql start

    (c) MySQL-service stoppen

             #[root@mysqlnode ~]# service mysql stop

    (d) de MySQL-service opnieuw starten

             #[root@mysqlnode ~]# service mysql restart


###<a name="how-to-install-mysql-on-red-hat-os-family-like-centos-oracle-linux"></a>MySQL installeren op Red Hat OS-familie, zoals CentOS, Oracle Linux
Wij zullen hier Linux VM met CentOS of Oracle Linux gebruiken.

- Stap 1: De MySQL Yum repository schakeloptie toevoegen aan `root` gebruiker:

            #[azureuser@mysqlnode:~]sudo su -

    Download en installeer het pakket MySQL versie:

            #[root@mysqlnode ~]# wget http://repo.mysql.com/mysql-community-release-el6-5.noarch.rpm
            #[root@mysqlnode ~]# yum localinstall -y mysql-community-release-el6-5.noarch.rpm

- Stap 2: Bewerk dit hieronder zodat de MySQL bibliotheek voor het downloaden van het pakket MySQL5.6.

            #[root@mysqlnode ~]# vim /etc/yum.repos.d/mysql-community.repo

    Update voor elke waarde van dit bestand hieronder:

        \# *Enable to use MySQL 5.6*

        [mysql56-community]
        name=MySQL 5.6 Community Server

        baseurl=http://repo.mysql.com/yum/mysql-5.6-community/el/6/$basearch/

        enabled=1

        gpgcheck=1

        gpgkey=file:/etc/pki/rpm-gpg/RPM-GPG-KEY-mysql

- Stap 3: Installeer MySQL uit de opslagplaats MySQL MySQL installeren:

           #[root@mysqlnode ~]#yum install mysql-community-server

    Pakket MySQL RPM en alle gerelateerde pakketten worden geïnstalleerd.

- Stap 4: De actieve MySQL-service beheren

    (a) Controleer de status van de MySQL-server:

           #[root@mysqlnode ~]#service mysqld status

    (b) controleren of de standaard poort van MySQL-server wordt uitgevoerd:

           #[root@mysqlnode ~]#netstat  –tunlp|grep 3306


    (c) start de MySQL-server:

           #[root@mysqlnode ~]#service mysqld start

    (d) de MySQL-server stoppen:

           #[root@mysqlnode ~]#service mysqld stop

    (e) set MySQL starten wanneer het systeem boot-up:

           #[root@mysqlnode ~]#chkconfig mysqld on


###<a name="how-to-install-mysql-on-suse-linux"></a>Het installeren van MySQL op SUSE Linux
We gebruiken Linux VM hier met OpenSUSE.

- Stap 1: Download en installeer MySQL-Server

    Ga naar `root` gebruiker door de volgende opdracht:  

           #sudo su -

    Download en installeer MySQL-pakket:

           #[root@mysqlnode ~]# zypper update

           #[root@mysqlnode ~]# zypper install mysql-server mysql-devel mysql

- Stap 2: De actieve MySQL-service beheren

    (a) de status van de MySQL-server controleren:

           #[root@mysqlnode ~]# rcmysql status

    (b) het selectievakje of de standaardpoort van de MySQL-server:

           #[root@mysqlnode ~]# netstat  –tunlp|grep 3306


    (c) start de MySQL-server:

           #[root@mysqlnode ~]# rcmysql start

    (d) de MySQL-server stoppen:

           #[root@mysqlnode ~]# rcmysql stop

    (e) set MySQL starten wanneer het systeem boot-up:

           #[root@mysqlnode ~]# insserv mysql

###<a name="next-step"></a>Volgende stap
Meer gebruik en informatie met betrekking tot MySQL [hier](https://www.mysql.com/)vinden.
