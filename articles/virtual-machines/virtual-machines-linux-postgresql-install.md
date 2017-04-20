<properties
    pageTitle="PostgreSQL op een Linux VM instellen | Microsoft Azure"
    description="Meer informatie over het installeren en configureren van PostgreSQL op een virtuele Linux machine in Azure"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="SuperScottz"
    manager="timlt"
    editor=""
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure-services"
    ms.date="02/01/2016"
    ms.author="mingzhan"/>


# <a name="install-and-configure-postgresql-on-azure"></a>Installeren en configureren van PostgreSQL op Azure

PostgreSQL is een geavanceerde open source database vergelijkbaar met Oracle en DB2. Enterprise-ready functies zoals volledige zuur naleving, betrouwbare transactionele bewerkingen en meerdere gelijktijdige controle omvat. Ondersteunt ook standaarden zoals ANSI SQL- en SQL/MED (inclusief buitenlandse gegevens wrappers voor Oracle, MySQL, MongoDB en vele andere). Het is zeer uitbreidbaar met ondersteuning voor meer dan 12 talen van de procedure, GIN en basisvertalingen indexen, gegevensondersteuning van ruimtelijke en meerdere NoSQL-functies voor JSON of sleutel waarde-gebaseerde toepassingen.

In dit artikel leert u hoe u kunt installeren en configureren van PostgreSQL op een Azure virtuele machine waarop Linux.


[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]


## <a name="install-postgresql"></a>PostgreSQL installeren

> [AZURE.NOTE] U moet al een Azure virtuele machine waarop Linux wordt uitgevoerd voordat u deze zelfstudie te voltooien. Als u wilt maken en instellen van een Linux VM voordat u verdergaat, Zie de [Azure Linux VM zelfstudie](virtual-machines-linux-quick-create-cli.md).

In dit geval poort 1999 als de PostgreSQL-poort gebruiken.  

Verbinding maken met de Linux VM die u hebt gemaakt via stopverf. Als dit de eerste keer dat u een Linux Azure VM gebruikt, Zie [hoe gebruik SSH met Linux op Azure](virtual-machines-linux-mac-create-ssh-keys.md) informatie over het gebruik van stopverf verbinding maken met een Linux VM.

1. Voer de volgende opdracht om over te schakelen naar de hoofdmap (admin):

        # sudo su -

2. Sommige distributies hebben afhankelijkheden die u installeren moet voordat u het PostgreSQL. Controleer of uw distro in deze lijst en de desbetreffende opdracht uitvoeren:

    - Basis Linux Red Hat:

            # yum install readline-devel gcc make zlib-devel openssl openssl-devel libxml2-devel pam-devel pam  libxslt-devel tcl-devel python-devel -y  

    - Basis Linux debian:

            # apt-get install readline-devel gcc make zlib-devel openssl openssl-devel libxml2-devel pam-devel pam libxslt-devel tcl-devel python-devel -y  

    - SUSE Linux:

            # zypper install readline-devel gcc make zlib-devel openssl openssl-devel libxml2-devel pam-devel pam  libxslt-devel tcl-devel python-devel -y  

3. PostgreSQL downloaden naar de hoofdmap en vervolgens unzip het pakket:

        # wget https://ftp.postgresql.org/pub/source/v9.3.5/postgresql-9.3.5.tar.bz2 -P /root/

        # tar jxvf  postgresql-9.3.5.tar.bz2

    Het bovenstaande is een voorbeeld. De meer gedetailleerde downloaden adres kunt u vinden in de [Index van de/pub/bron /](https://ftp.postgresql.org/pub/source/).

4. Om te beginnen het opbouwen, moet u deze opdrachten uitvoeren:

        # cd postgresql-9.3.5

        # ./configure --prefix=/opt/postgresql-9.3.5

5. Als u wilt bouwen alles dat kan worden opgebouwd, met inbegrip van de documentatie (HTML- en man-pagina's) en de aanvullende modules (contrib), in plaats daarvan de volgende opdracht uitvoeren:

        # gmake install-world

    U moet het volgende bevestigingsbericht ontvangen:

        PostgreSQL, contrib, and documentation successfully made. Ready to install.

## <a name="configure-postgresql"></a>PostgreSQL configureren

1. (Optioneel) Een symbolische koppeling om de PostgreSQL verwijzingen niet het versienummer korter te maken:

        # ln -s /opt/pgsql9.3.5 /opt/pgsql

2. Maak een map voor de database:

        # mkdir -p /opt/pgsql_data

3. Een niet-root gebruiker maken en wijzigen van het profiel van die gebruiker. Ga vervolgens naar deze nieuwe gebruiker ( *postgres* in ons voorbeeld genoemd):

        # useradd postgres

        # chown -R postgres.postgres /opt/pgsql_data

        # su - postgres

   > [AZURE.NOTE] Om veiligheidsredenen wordt PostgreSQL gebruikt een niet-root gebruiker initialiseren, starten of afsluiten van de database.


4. Bewerk het bestand *bash_profile* door het invoeren van de onderstaande opdrachten. Deze regels worden toegevoegd aan het einde van het bestand *bash_profile* :

        cat >> ~/.bash_profile <<EOF
        export PGPORT=1999
        export PGDATA=/opt/pgsql_data
        export LANG=en_US.utf8
        export PGHOME=/opt/pgsql
        export PATH=\$PATH:\$PGHOME/bin
        export MANPATH=\$MANPATH:\$PGHOME/share/man
        export DATA=`date +"%Y%m%d%H%M"`
        export PGUSER=postgres
        alias rm='rm -i'
        alias ll='ls -lh'
        EOF

5. Het bestand *bash_profile* uitvoeren:

        $ source .bash_profile

6. De installatie controleren door de volgende opdracht:

        $ which psql

    Als de installatie voltooid is, ziet u het volgende antwoord:

        /opt/pgsql/bin/psql

7. U kunt ook de versie van PostgreSQL controleren:

        $ psql -V

8. Initialiseren van de database:

        $ initdb -D $PGDATA -E UTF8 --locale=C -U postgres -W

    De volgende uitvoer wordt weergegeven:

![afbeelding](./media/virtual-machines-linux-postgresql-install/no1.png)

## <a name="set-up-postgresql"></a>PostgreSQL instellen

<!--    [postgres@ test ~]$ exit -->

Voer de volgende opdrachten:

    # cd /root/postgresql-9.3.5/contrib/start-scripts

    # cp linux /etc/init.d/postgresql

Twee variabelen in het bestand /etc/init.d/postgresql wijzigen. Het voorvoegsel is ingesteld op het installatiepad van PostgreSQL: **/opt/pgsql**. PGDATA is ingesteld op het gegevenspad opslag van PostgreSQL: **/opt/pgsql_data**.

    # sed -i '32s#usr/local#opt#' /etc/init.d/postgresql

    # sed -i '35s#usr/local/pgsql/data#opt/pgsql_data#' /etc/init.d/postgresql

![afbeelding](./media/virtual-machines-linux-postgresql-install/no2.png)

U kunt het uitvoerbare bestand wijzigen:

    # chmod +x /etc/init.d/postgresql

PostgreSQL starten:

    # /etc/init.d/postgresql start

Controleer of het eindpunt van de PostgreSQL ingeschakeld is:

    # netstat -tunlp|grep 1999

Hier ziet u de volgende uitvoer:

![afbeelding](./media/virtual-machines-linux-postgresql-install/no3.png)

## <a name="connect-to-the-postgres-database"></a>Verbinding maken met de Postgres database

Ga naar de gebruiker postgres opnieuw:

    # su - postgres

Een Postgres database maken:

    $ createdb events

Verbinding maken met de database van de gebeurtenissen die u zojuist hebt gemaakt:

    $ psql -d events

## <a name="create-and-delete-a-postgres-table"></a>Maken en verwijderen van een tabel met Postgres

Nu dat u hebt verbonden met de database, kunt u tabellen in deze.

Bijvoorbeeld een nieuw voorbeeld Postgres-tabel maken met de volgende opdracht:

    CREATE TABLE potluck (name VARCHAR(20), food VARCHAR(30),   confirmed CHAR(1), signup_date DATE);

U hebt nu een tabel met vier kolommen met de volgende namen van kolommen en beperkingen instellen:

1. De kolom 'name' heeft is beperkt door de opdracht VARCHAR onder 20 tekens lang zijn.
2. De kolom "levensmiddel" geeft aan dat het item voedsel dat elke persoon kan worden gebracht. VARCHAR beperkt deze tekst onder 30 tekens lang zijn.
3. De kolom "bevestigde" wordt vastgelegd of de persoon aan de Amerikaans is RSVP'd. Acceptabele waarden zijn "Y" en "N".
4. De 'datum' kolom ziet u wanneer ze zich voor de gebeurtenis. Postgres is vereist dat de datums worden geschreven als jjjj-mm-dd.

Zie de volgende als uw tabel heeft gemaakt:

![afbeelding](./media/virtual-machines-linux-postgresql-install/no4.png)

U kunt ook de tabelstructuur controleren door de volgende opdracht:

![afbeelding](./media/virtual-machines-linux-postgresql-install/no5.png)

### <a name="add-data-to-a-table"></a>Gegevens toevoegen aan een tabel

Voeg eerst de informatie in een rij:

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES('John', 'Casserole', 'Y', '2012-04-11');

Deze uitvoer wordt weergegeven:

![afbeelding](./media/virtual-machines-linux-postgresql-install/no6.png)

U kunt een paar meer personen toevoegen aan de tabel ook. Hier volgen enkele opties, of u kunt uw eigen maken:

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES('Sandy', 'Key Lime Tarts', 'N', '2012-04-14');

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES ('Tom', 'BBQ','Y', '2012-04-18');

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES('Tina', 'Salad', 'Y', '2012-04-18');

### <a name="show-tables"></a>Tabellen weergeven

Gebruik de volgende opdracht als u een tabel weergeven:

    select * from potluck;

De uitvoer is:

![afbeelding](./media/virtual-machines-linux-postgresql-install/no7.png)

### <a name="delete-data-in-a-table"></a>Gegevens in een tabel verwijderen

Gebruik de volgende opdracht om de gegevens in een tabel te verwijderen:

    delete from potluck where name=’John’;

Hiermee verwijdert u alle gegevens in de rij 'Jan'. De uitvoer is:

![afbeelding](./media/virtual-machines-linux-postgresql-install/no8.png)

### <a name="update-data-in-a-table"></a>Gegevens in een tabel bijwerken

Gebruik de volgende opdracht als u gegevens in een tabel bij te werken. Voor deze een heeft Sandy bevestigd dat ze bijwonen, zodat we haar RSVP van "N" naar "Y" wijzigen:

    UPDATE potluck set confirmed = 'Y' WHERE name = 'Sandy';


##<a name="get-more-information-about-postgresql"></a>Meer informatie over PostgreSQL
Nu dat u de installatie van PostgreSQL in een Linux Azure VM hebt voltooid, kunt u genieten van het gebruik in Azure. Meer informatie over PostgreSQL, Bezoek de [website van PostgreSQL](http://www.postgresql.org/).
