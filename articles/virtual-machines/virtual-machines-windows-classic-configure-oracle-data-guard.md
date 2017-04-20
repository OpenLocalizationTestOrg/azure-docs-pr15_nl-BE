<properties
    pageTitle="Configureren van Oracle Data Guard in VMs | Microsoft Azure"
    description="Stap voor stap een zelfstudie voor het opzetten en implementeren van Oracle Data Guard op Azure virtuele machines voor hoge beschikbaarheid en disaster recovery."
    services="virtual-machines-windows"
    authors="rickstercdn"
    manager="timlt"
    documentationCenter=""
    tags="azure-service-management"/>
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows"
    ms.workload="infrastructure-services"
    ms.date="09/06/2016"
    ms.author="rclaus" />

#<a name="configuring-oracle-data-guard-for-azure"></a>Oracle Data Guard voor Azure configureren


Deze zelfstudie laat zien hoe instellen en implementeren van Oracle Data Guard in Azure virtuele Machines omgeving voor hoge beschikbaarheid en noodherstel. De zelfstudie is gericht op een replicatieverbinding voor RAC Oracle-databases.

Oracle Data Guard ondersteunt gegevensbescherming en noodherstel voor Oracle-Database. Het is een eenvoudige, krachtige, onverwachte oplossing voor noodherstel, gegevensbescherming en beschikbaarheid voor de volledige Oracle database.

In deze zelfstudie wordt ervan uitgegaan dat u al theoretische en praktische kennis van concepten voor Oracle Database hoge beschikbaarheid en noodherstel. Zie voor informatie de [website van Oracle](http://www.oracle.com/technetwork/database/features/availability/index.html) en ook de [handleiding en Oracle Data Guard concepten](https://docs.oracle.com/cd/E11882_01/server.112/e41134/toc.htm).

Bovendien de zelfstudie wordt ervan uitgegaan dat u de volgende vereiste onderdelen al hebt geïmplementeerd:

- U hebt het gedeelte voor hoge beschikbaarheid en herstel na storing herstel overwegingen in het onderwerp [afbeeldingen Oracle Virtual Machine - diverse overwegingen](virtual-machines-windows-classic-oracle-considerations.md) al hebt bekeken. Azure ondersteunt momenteel exemplaren van zelfstandige Oracle-Database, maar niet Oracle Real Application Clusters (RAC Oracle).


- U hebt twee virtuele Machines (VMs) gemaakt in hetzelfde platform image Oracle Enterprise Edition wordt geleverd met Azure. Controleer of de virtuele Machines in de [dezelfde cloud service](virtual-machines-windows-load-balance.md) en in hetzelfde virtuele netwerk zodat ze toegang tot elkaar via het permanente persoonlijke IP-adres. Bovendien verdient het VMs in de dezelfde [beschikbaar](virtual-machines-windows-manage-availability.md) dat Azure plaatsen in de fout met betrekking tot afzonderlijke domeinen en domeinen upgraden te plaatsen. Oracle Data Guard is alleen beschikbaar bij Oracle Database Enterprise Edition. Elke machine moet ten minste 2 GB geheugen en 5 GB schijfruimte hebben. Zie voor de meest actuele informatie op het platform VM formaten geleverd [Formaten voor Azure Virtual Machine](virtual-machines-windows-sizes.md). Als u extra schijf nodig hebt voor uw VMs, kunt u extra schijven koppelen. Zie voor informatie [hoe u een schijf met gegevens op een virtuele Machine koppelt](virtual-machines-windows-classic-attach-disk.md).



- U hebt de namen van de virtuele Machine als 'Machine1' ingesteld voor de primaire VM en "Machine2" voor de stand-by-VM op de klassieke Azure portal.

- U hebt ingesteld dat de variabele **ORACLE_HOME** verwijzen naar dezelfde oracle installatie pad naar de hoofdmap in de primaire en standby virtuele Machines, zoals `C:\OracleDatabase\product\11.2.0\dbhome_1\database`.

- U aanmelden met uw Windows-server als een lid van de groep **Administrators** of als lid van de groep **ORA_DBA** .

In deze zelfstudie wordt u:

De stand-by-fysieke databaseomgeving implementeren

1. Een primaire database maken

2. De primaire database voorbereiden op stand-by-database maken

    1. Geforceerde logboekregistratie inschakelen

    2. Maak een wachtwoordbestand

    3. Een logboek met stand-by opnieuw configureren

    4. Archivering inschakelen

    5. Initialisatieparameters primaire database instellen

Maak een fysieke database stand-by

1. Een parameter initialisatiebestand voor stand-by-database voorbereiden

2. De listener en tnsnames voor de database van de primaire en standby-computers configureren

    1. Listener.ora op beide servers voor het opslaan van gegevens voor beide databases configureren

    2. Voor vermeldingen voor primaire en standby databases tnsnames.ora op de primaire en standby virtuele Machines te configureren. 

    3. Start de listener en tnsping op zowel virtuele Machines om beide services te controleren.

3. De stand-by-instantie in staat nomount starten

4. RMAN kopie van de database en voor het maken van een stand-by-database gebruiken

5. De stand-by fysieke database starten in de herstelmodus beheerde

6. Controleer of de fysieke database stand-by

> [AZURE.IMPORTANT] Deze zelfstudie is ingesteld en getest met de volgende configuratie van hardware en software:
>
>|                      | **Primaire Database**                      | **Stand-by-Database**                      |
>|----------------------|-------------------------------------------|-------------------------------------------|
>| **Oracle-Release**   | Oracle11g Enterprise-versie (11.2.0.4.0) | Oracle11g Enterprise-versie (11.2.0.4.0) |
>| **Naam van de computer**     | Machine1                                  | Machine2                                  |
>| **Besturingssysteem** | Windows 2008 R2                           | Windows 2008 R2                           |
>| **Oracle-SID**       | TEST                                      | TEST\_STBY                                |
>| **Geheugen**           | Min 2 GB                                  | Min 2 GB                                  |
>| **Schijfruimte**       | Min 5 GB                                  | Min 5 GB                                  |

Voor latere versies van Oracle Database en Oracle Data Guard mogelijk enkele aanvullende wijzigingen die u moet implementeren. Zie voor de meest recente versie-specifieke informatie-documentatie op de website van Oracle [Data Guard](http://www.oracle.com/technetwork/database/features/availability/data-guard-documentation-152848.html) en [Oracle-Database](http://www.oracle.com/us/corporate/features/database-12c/index.html) .

##<a name="implement-the-physical-standby-database-environment"></a>De stand-by-fysieke databaseomgeving implementeren
### <a name="1-create-a-primary-database"></a>1. Maak een primaire database

- Maak een primaire database "TEST" in de primaire virtuele Machine. Zie voor meer informatie, maken en configureren van een Oracle-Database.
- Overzicht van de naam van uw database verbinding maken met uw database als de gebruiker SYS aan SYSDBA rol in de SQL * Plus de MS-DOS-prompt en uitvoeren met de volgende instructie:

        SQL> select name from v$database;

        The result will display like the following:

        NAME
        ---------
        TEST
- De namen van de bestanden van de weergave dba_data_files systeem vervolgens een query:

        SQL> select file_name from dba_data_files;
        FILE_NAME
        -------------------------------------------------------------------------------
        C:\ <YourLocalFolder>\TEST\USERS01.DBF
        C:\ <YourLocalFolder>\TEST\UNDOTBS01.DBF
        C:\ <YourLocalFolder>\TEST\SYSAUX01.DBF
        C:\<YourLocalFolder>\TEST\SYSTEM01.DBF
        C:\<YourLocalFolder>\TEST\EXAMPLE01.DBF

### <a name="2-prepare-the-primary-database-for-standby-database-creation"></a>2. de primaire database voorbereiden op stand-by-database maken

Voordat u een stand-by-database, verdient u ervoor zorgen dat de primaire database juist is geconfigureerd. Hier volgt een lijst met de stappen die u moet uitvoeren:

1. Geforceerde logboekregistratie inschakelen
2. Maak een wachtwoordbestand
3. Een logboek met stand-by opnieuw configureren
4. Archivering inschakelen
5. Initialisatieparameters primaire database instellen

#### <a name="enable-forced-logging"></a>Geforceerde logboekregistratie inschakelen

Voor het implementeren van een stand-by-Database, moeten we gedwongen logboekregistratie inschakelen in de hoofddatabase. Deze optie zorgt ervoor dat zelfs als een 'nologging'-bewerking is voltooid, voorrang voor logboekregistratie van kracht en alle bewerkingen in de logboeken opnieuw worden geregistreerd. We Zorg er daarom voor dat alles in de primaire database is aangemeld en replicatie op de stand-by alle bewerkingen in de hoofddatabase omvat. Als u logboekregistratie in werking treden, voert de instructie alter database:

    SQL> ALTER DATABASE FORCE LOGGING;

    Database altered.

#### <a name="create-a-password-file"></a>Maak een wachtwoordbestand

Om te kunnen verzenden en gearchiveerde logboeken van de primaire server van toepassing op de stand-by-server, moet het sys-wachtwoord identiek zijn op de primaire en de stand-by-servers. Dat is de reden waarom u een wachtwoordbestand op de primaire database maken en deze kopiëren naar de stand-by-server.

>[AZURE.IMPORTANT] Als met een Oracle Database 12c, is er een nieuwe gebruiker, **SYSDG**, die u gebruiken kunt voor het beheer van Oracle Data Guard. Zie voor meer informatie, [wijzigingen in de Oracle Database 12 c Release](http://docs.oracle.com/database/121/UNXAR/release_changes.htm#UNXAR404).

Daarnaast Zorg ervoor dat de ORACLE\_thuis omgeving is al gedefinieerd in Machine1. Zo niet, als een omgevingsvariabele met behulp van het dialoogvenster omgevingsvariabelen definiëren. Als u dit dialoogvenster, start u het hulpprogramma **systeem** door te dubbelklikken op het pictogram systeem in het **Configuratiescherm**. Klik op het tabblad **Geavanceerd** en kies **Omgevingsvariabelen**. De omgevingsvariabelen instellen, klikt u op de knop **Nieuw** onder **Systeemvariabelen**. Na het instellen van de omgevingsvariabelen, de bestaande Windows-opdrachtprompt sluiten en openen van een nieuwe.

Voer de volgende instructie om te schakelen naar de Oracle\_Home map, zoals C:\\OracleDatabase\\product\\11.2.0\\dbhome\_1\\database.

    cd %ORACLE_HOME%\database

Maak vervolgens een wachtwoordbestand met het hulpprogramma voor wachtwoord bestand maken, [ORAPWD](http://docs.oracle.com/cd/B28359_01/server.111/b28310/dba007.htm). In dezelfde Windows-opdrachtprompt in Machine1, moet u de volgende opdracht uitvoeren door de wachtwoordwaarde als wachtwoord voor de **SYS**:

    ORAPWD FILE=PWDTEST.ora PASSWORD=password FORCE=y

Deze opdracht maakt u een wachtwoordbestand met de naam als PWDTEST.ora in de ORACLE\_HOME\\de database met adreslijstservices. Kopieer dit bestand naar ORACLE %\_HOME %\\database directory in Machine2 handmatig.

#### <a name="configure-a-standby-redo-log"></a>Een logboek met stand-by opnieuw configureren

Vervolgens moet u een stand-by opnieuw logboek zodanig configureren dat de primaire het opnieuw goed ontvangen kan wanneer deze een stand-by is. Vooraf hier maakt, kunt ook de logboeken opnieuw stand-bymodus automatisch wordt gemaakt op de stand-by. Het is belangrijk dat de stand-by opnieuw Logboeken (SRL) configureren met dezelfde grootte als de online logboeken opnieuw. De grootte van de logboekbestanden van de huidige stand-by opnieuw moet exact overeenkomen met de grootte van de huidige primaire opnieuw on line databaselogboekbestanden.

Voer de volgende instructie in de SQL\*PLUS de MS-DOS-prompt in Machine1. De v$ logfile is de systeemweergave van een dat informatie over de logboekbestanden opnieuw bevat.

    SQL> select * from v$logfile;
    GROUP# STATUS  TYPE    MEMBER                                                       IS_
    ---------- ------- ------- ------------------------------------------------------------ ---
    3         ONLINE  C:\<YourLocalFolder>\TEST\REDO03.LOG               NO
    2         ONLINE  C:\<YourLocalFolder>\TEST\REDO02.LOG               NO
    1         ONLINE  C:\<YourLocalFolder>\TEST\REDO01.LOG               NO
    Next, query the v$log system view, displays log file information from the control file.
    SQL> select bytes from v$log;
    BYTES
    ----------
    52428800
    52428800
    52428800


Houd er rekening mee dat 52428800 50 MB is.

Klik in de SQL\*Plus venster uitvoeren met de volgende instructies om een nieuwe stand-by logboekbestandsgroep toevoegen aan een database met stand-by en geef een getal op waarmee de groep met behulp van de GROUP-component. Groep getallen kunt aanbrengen stand-by-log bestandsgroepen gemakkelijker beheren:

    SQL> ALTER DATABASE ADD STANDBY LOGFILE GROUP 4 'C:\<YourLocalFolder>\TEST\REDO04.LOG' SIZE 50M;
    Database altered.
    SQL> ALTER DATABASE ADD STANDBY LOGFILE GROUP 5 'C:\<YourLocalFolder>\TEST\REDO05.LOG' SIZE 50M;
    Database altered.
    SQL> ALTER DATABASE ADD STANDBY LOGFILE GROUP 6 'C:\<YourLocalFolder>\TEST\REDO06.LOG' SIZE 50M;
    Database altered.

Voer de volgende systeemweergave gegevens weergeven over opnieuw logboekbestanden. Deze bewerking wordt ook gecontroleerd of de stand-by-log bestandsgroepen zijn gemaakt:

    SQL> select * from v$logfile;
    GROUP# STATUS  TYPE MEMBER IS_
    ---------- ------- ------- --------------------------------------------- ---
    3         ONLINE C:\<YourLocalFolder>\TEST\REDO03.LOG NO
    2         ONLINE C:\<YourLocalFolder>\TEST\REDO02.LOG NO
    1         ONLINE C:\<YourLocalFolder>\TEST\REDO01.LOG NO
    4         STANDBY C:\<YourLocalFolder>\TEST\REDO04.LOG
    5         STANDBY C:\<YourLocalFolder>\TEST\REDO05.LOG NO
    6         STANDBY C:\<YourLocalFolder>\TEST\REDO06.LOG NO
    6 rows selected.

#### <a name="enable-archiving"></a>Archivering inschakelen

Schakel vervolgens archiveren door het uitvoeren van de volgende instructies om de primaire database in ARCHIVELOG modus en automatisch archiveren inschakelen. Archief logmodus kunt u door de database te koppelen en vervolgens de opdracht archivelog wordt uitgevoerd.

Meld u eerst aan als sysdba. In de Windows-opdrachtprompt uitvoeren:

    sqlplus /nolog

    connect / as sysdba

Vervolgens afsluiten van de in de SQL-database\*Plus de MS-DOS-prompt:

    SQL> shutdown immediate;
    Database closed.
    Database dismounted.
    ORACLE instance shut down.

Vervolgens worden de startopdracht koppelen aan de database koppelen uitgevoerd. Dit zorgt ervoor dat Oracle worden gekoppeld aan het exemplaar van de opgegeven database.

    SQL> startup mount;
    ORACLE instance started.
    Total System Global Area 1503199232 bytes
    Fixed Size                  2281416 bytes
    Variable Size             922746936 bytes
    Database Buffers          570425344 bytes
    Redo Buffers                7745536 bytes
    Database mounted.

Vervolgens wordt uitgevoerd:

    SQL> alter database archivelog;
    Database altered.

Voert u het Alter database-instructie met de Open-component beschikbaar voor normaal gebruik maken van de database:

    SQL> alter database open;

    Database altered.

#### <a name="set-primary-database-initialization-parameters"></a>Initialisatieparameters primaire database instellen

Configureren van de beveiliging van de gegevens die u wilt maken en configureren van de stand-by-parameters op een gewone pfile (initialisatie parameter tekstbestand) eerste. Wanneer de pfile gereed is, moet u deze converteren naar een bestand van de server-parameter (SPFILE).

U kunt bepalen met behulp van de parameters in de Initialisatie van Data Guard milieu. ORA-bestand. Wanneer deze zelfstudie te volgen, moet u de primaire database INIT bijwerken. ORA zodanig dat deze beide rollen bevatten kan: primaire of stand-by.

    SQL> create pfile from spfile;
    File created.

Vervolgens moet u voor het bewerken van de pfile als de stand-by-parameters wilt toevoegen. Open hiervoor de INITTEST. ORA-bestand op de locatie van ORACLE %\_HOME %\\database. Vervolgens de volgende instructies toevoegen aan het bestand INITTEST.ora. De naamgevingsconventie voor de Initialisatie. ORA bestand INIT is\<YourDatabaseName\>. ORA.

    db_name='TEST'
    db_unique_name='TEST'
    LOG_ARCHIVE_CONFIG='DG_CONFIG=(TEST,TEST_STBY)'
    LOG_ARCHIVE_DEST_1= 'LOCATION=C:\OracleDatabase\archive   VALID_FOR=(ALL_LOGFILES,ALL_ROLES) DB_UNIQUE_NAME=TEST'
    LOG_ARCHIVE_DEST_2= 'SERVICE=TEST_STBY LGWR ASYNC VALID_FOR=(ONLINE_LOGFILES,PRIMARY_ROLE) DB_UNIQUE_NAME=TEST_STBY'
    LOG_ARCHIVE_DEST_STATE_1=ENABLE
    LOG_ARCHIVE_DEST_STATE_2=ENABLE
    REMOTE_LOGIN_PASSWORDFILE=EXCLUSIVE
    LOG_ARCHIVE_FORMAT=%t_%s_%r.arc
    LOG_ARCHIVE_MAX_PROCESSES=30
    # Standby role parameters --------------------------------------------------------------------
    fal_server=TEST_STBY
    fal_client=TEST
    standby_file_management=auto
    db_file_name_convert='TEST_STBY','TEST'
    log_file_name_convert='TEST_STBY','TEST'
    # ---------------------------------------------------------------------------------------------


De vorige instructie blok bevat drie belangrijke setup items:
-   **... LOG_ARCHIVE_CONFIG:** U definieert de database met unieke id's met deze instructie.
-   **... LOG_ARCHIVE_DEST_1:** U definieert de locatie van de lokale archief met deze instructie. Wij raden aan dat u een nieuwe map voor archivering behoeften van uw database maken en geef de archieflocatie van het lokale met behulp van deze verklaring uitdrukkelijk in plaats van met behulp van Oracle standaard map % ORACLE_HOME%\database\archive.
-   **LOG_ARCHIVE_DEST_2... LGWR ASYNC...:** definieert u een asynchrone logboek schrijver proces (LGWR) voor de transactie opnieuw gegevens verzamelen en verzenden deze naar stand-by-bestemmingen. De DB_UNIQUE_NAME geeft hier een unieke naam voor de database op de stand-by-server.

Nadat de nieuwe parameterbestand klaar is, moet u de spfile maken.

Eerste, afsluiten van de database

    SQL> shutdown immediate;

    Database closed.

    Database dismounted.

    ORACLE instance shut down.

Voer de opdracht nomount gestart als volgt:

    SQL> startup nomount pfile='c:\OracleDatabase\product\11.2.0\dbhome_1\database\initTEST.ora';
    ORACLE instance started.
    Total System Global Area 1503199232 bytes
    Fixed Size                  2281416 bytes
    Variable Size             922746936 bytes
    Database Buffers          570425344 bytes
    Redo Buffers                7745536 bytes

Maak nu een spfile:

    SQL>create spfile frompfile='c:\OracleDatabase\product\11.2.0\dbhome\_1\database\initTEST.ora';

    File created.

Vervolgens afsluiten van de database

    SQL> shutdown immediate;

    ORA-01507: database not mounted

Gebruik vervolgens de opdracht gestart om een exemplaar te starten:

    SQL> startup;
    ORACLE instance started.
    Total System Global Area 1503199232 bytes
    Fixed Size                  2281416 bytes
    Variable Size             922746936 bytes
    Database Buffers          570425344 bytes
    Redo Buffers                7745536 bytes
    Database mounted.
    Database opened.

##<a name="create-a-physical-standby-database"></a>Maak een fysieke database stand-by
Deze sectie richt zich op de stappen die u in Machine2 uitvoeren moet ter voorbereiding van de fysieke database stand-by.

Eerst moet u extern bureaublad op Machine2 via de klassieke Azure portal.

Maak alle mappen voor de stand-by-database, zoals C: op stand-by-Server (Machine2)\\\<YourLocalFolder\>\\TEST. Tijdens deze zelfstudie te volgen, zorg dat de mapstructuur overeenkomt met de mapstructuur op Machine1 houden alle noodzakelijke bestanden, zoals controlfile, datafiles, redologfiles, udump, bdump en cdump. Bovendien bepalen de ORACLE\_thuis en ORACLE\_basis omgevingsvariabelen in Machine2. Als dat niet het geval is, kunt u deze als een omgevingsvariabele met behulp van het dialoogvenster omgevingsvariabelen definiëren. Als u dit dialoogvenster, start u het hulpprogramma **systeem** door te dubbelklikken op het pictogram systeem in het **Configuratiescherm**. Klik op het tabblad **Geavanceerd** en kies **Omgevingsvariabelen**. De omgevingsvariabelen instellen, klikt u op de knop **Nieuw** onder **Systeemvariabelen**. Na het instellen van de omgevingsvariabelen, moet u de bestaande Windows-opdrachtprompt te sluiten en openen van een nieuw om de wijzigingen te bekijken.

Vervolgens als volgt te werk:

1. Een parameter initialisatiebestand voor stand-by-database voorbereiden

2. De listener en tnsnames voor de database van de primaire en standby-computers configureren

    1. Listener.ora op beide servers voor het opslaan van gegevens voor beide databases configureren

    2. Tnsnames.ora op de primaire en standby virtuele Machines voor vermeldingen voor primaire en standby databases configureren

    3. Start de listener en tnsping op zowel virtuele Machines om beide services te controleren.

3. De stand-by-instantie in staat nomount starten

4. RMAN kopie van de database en voor het maken van een stand-by-database gebruiken

5. De stand-by fysieke database starten in de herstelmodus beheerde

6. Controleer of de fysieke database stand-by

### <a name="1-prepare-an-initialization-parameter-file-for-standby-database"></a>1. een initialisatiebestand parameter voor stand-by-database voorbereiden

In deze sectie wordt beschreven hoe een initialisatiebestand parameter voor de stand-by-database voorbereiden. U doet dit door eerst de INITTEST kopiëren. ORA bestand van computer 1 naar Machine2 handmatig. U moet de INITTEST zien. ORA-bestand in de map % ORACLE\_% HOME\\de databasemap in beide machines. Vervolgens wijzigt u het bestand INITTEST.ora in Machine2 in te stellen voor de stand-by-functie zoals hieronder aangegeven:

    db_name='TEST'
    db_unique_name='TEST_STBY'
    db_create_file_dest='c:\OracleDatabase\oradata\test_stby’
    db_file_name_convert=’TEST’,’TEST_STBY’
    log_file_name_convert='TEST','TEST_STBY'


    job_queue_processes=10
    LOG_ARCHIVE_CONFIG='DG_CONFIG=(TEST,TEST_STBY)'
    LOG_ARCHIVE_DEST_1='LOCATION=c:\OracleDatabase\TEST_STBY\archives VALID_FOR=(ALL_LOGFILES,ALL_ROLES) DB_UNIQUE_NAME=’TEST'
    LOG_ARCHIVE_DEST_2='SERVICE=TEST LGWR ASYNC VALID_FOR=(ONLINE_LOGFILES,PRIMARY_ROLE)
    LOG_ARCHIVE_DEST_STATE_1='ENABLE'
    LOG_ARCHIVE_DEST_STATE_2='ENABLE'
    LOG_ARCHIVE_FORMAT='%t_%s_%r.arc'
    LOG_ARCHIVE_MAX_PROCESSES=30


De vorige instructie blok bevat twee belangrijke instellingen items:

-   **\*. LOG_ARCHIVE_DEST_1:** moet u de map c:\OracleDatabase\TEST_STBY\archives in Machine2 handmatig maken.
-   **\*. LOG_ARCHIVE_DEST_2:** dit is een optionele stap. U instellen deze als het nodig zijn kan als de primaire computer onderhoud en de computer stand-by een primaire database wordt.

Vervolgens moet u de stand-by-sessie gestart. Voer de volgende opdracht achter de opdrachtprompt van Windows een Oracle-exemplaar maken met het maken van een Windows-service op de stand-by-databaseserver:

    oradim -NEW -SID TEST\_STBY -STARTMODE MANUAL

De opdracht **Oradim** maakt een exemplaar van Oracle, maar wordt niet gestart. U vindt het in de C:\\OracleDatabase\\product\\11.2.0\\dbhome\_1\\BIN-directory.

##<a name="configure-the-listener-and-tnsnames-to-support-the-database-on-primary-and-standby-machines"></a>De listener en tnsnames voor de database van de primaire en standby-computers configureren
Voordat u een stand-by-database maakt, moet u om ervoor te zorgen dat de primaire en standby databases in uw configuratie met elkaar kunnen praten. Hiervoor moet u de listener- en TNSNames configureren, handmatig of met behulp van het hulpprogramma voor systeemconfiguratie in het netwerk NETCA. Dit is een verplichte taak wanneer u het hulpprogramma Systeemherstel Manager (RMAN).

### <a name="configure-listenerora-on-both-servers-to-hold-entries-for-both-databases"></a>Listener.ora op beide servers voor het opslaan van gegevens voor beide databases configureren

Extern bureaublad Machine1 te bewerken dat het bestand listener.ora als hieronder. Wanneer u het bestand listener.ora bewerkt, Zorg altijd dat de opening en het sluithaakje regel omhoog in dezelfde kolom. U vindt het bestand listener.ora in de volgende map: c:\\OracleDatabase\\product\\11.2.0\\dbhome\_1\\netwerk\\ADMIN\\.

    # listener.ora Network Configuration File: C:\OracleDatabase\product\11.2.0\dbhome_1\network\admin\listener.ora

    # Generated by Oracle configuration tools.

    SID_LIST_LISTENER =
      (SID_LIST =
        (SID_DESC =
          (SID_NAME = test)
          (ORACLE_HOME = C:\OracleDatabase\product\11.2.0\dbhome_1)
          (PROGRAM = extproc)
          (ENVS = "EXTPROC_DLLS=ONLY:C:\OracleDatabase\product\11.2.0\dbhome_1\bin\oraclr11.dll")
        )
      )

    LISTENER =
      (DESCRIPTION_LIST =
        (DESCRIPTION =
          (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE1)(PORT = 1521))
          (ADDRESS = (PROTOCOL = IPC)(KEY = EXTPROC1521))
        )
      )

Next, extern bureaublad Machine2 en het listener.ora-bestand als volgt bewerken: listener.ora # netwerk configuratiebestand: C:\OracleDatabase\product\11.2.0\dbhome_1\network\admin\listener.ora

    # Generated by Oracle configuration tools.

    SID_LIST_LISTENER =
      (SID_LIST =
        (SID_DESC =
          (SID_NAME = test_stby)
          (ORACLE_HOME = C:\OracleDatabase\product\11.2.0\dbhome_1)
          (PROGRAM = extproc)
          (ENVS = "EXTPROC_DLLS=ONLY:C:\OracleDatabase\product\11.2.0\dbhome_1\bin\oraclr11.dll")
        )
      )

    LISTENER =
      (DESCRIPTION_LIST =
        (DESCRIPTION =
          (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE2)(PORT = 1521))
          (ADDRESS = (PROTOCOL = IPC)(KEY = EXTPROC1521))
        )
      )


### <a name="configure-tnsnamesora-on-the-primary-and-standby-virtual-machines-to-hold-entries-for-both-primary-and-standby-databases"></a>Tnsnames.ora op de primaire en standby virtuele Machines voor vermeldingen voor primaire en standby databases configureren

Extern bureaublad Machine1 en bewerken het bestand tnsnames.ora als hieronder. Het bestand tnsnames.ora vindt u in de volgende map: c:\\OracleDatabase\\product\\11.2.0\\dbhome\_1\\netwerk\\ADMIN\\.

    TEST =
      (DESCRIPTION =
        (ADDRESS_LIST =
          (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE1)(PORT = 1521))
        )
        (CONNECT_DATA =
          (SERVICE_NAME = test)
        )
      )

    TEST_STBY =
      (DESCRIPTION =
        (ADDRESS_LIST =
          (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE2)(PORT = 1521))
        )
        (CONNECT_DATA =
          (SERVICE_NAME = test_stby)
        )
      )

Extern bureaublad voor Machine2 en bewerken de tnsnames.ora bestand als volgt:

    TEST =
      (DESCRIPTION =
        (ADDRESS_LIST =
          (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE1)(PORT = 1521))
        )
        (CONNECT_DATA =
          (SERVICE_NAME = test)
        )
      )

    TEST_STBY =
      (DESCRIPTION =
        (ADDRESS_LIST =
          (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE2)(PORT = 1521))
        )
        (CONNECT_DATA =
          (SERVICE_NAME = test_stby)
        )
      )


### <a name="start-the-listener-and-check-tnsping-on-both-virtual-machines-to-both-services"></a>Start de listener en tnsping op zowel virtuele Machines om beide services te controleren.

Een nieuwe opdrachtprompt van Windows in de primaire en standby virtuele Machines te openen en uitvoeren van de volgende instructies:

    C:\Users\DBAdmin>tnsping test

    TNS Ping Utility for 64-bit Windows: Version 11.2.0.1.0 - Production on 14-NOV-2013 06:29:08
    Copyright (c) 1997, 2010, Oracle.  All rights reserved.
    Used parameter files:
    C:\OracleDatabase\product\11.2.0\dbhome_1\network\admin\sqlnet.ora
    Used TNSNAMES adapter to resolve the alias
    Attempting to contact (DESCRIPTION = (ADDRESS_LIST = (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE1)(PORT = 1521))) (CONNECT_DATA = (SER
    VICE_NAME = test)))
    OK (0 msec)


    C:\Users\DBAdmin>tnsping test_stby

    TNS Ping Utility for 64-bit Windows: Version 11.2.0.1.0 - Production on 14-NOV-2013 06:29:16
    Copyright (c) 1997, 2010, Oracle.  All rights reserved.
    Used parameter files:
    C:\OracleDatabase\product\11.2.0\dbhome_1\network\admin\sqlnet.ora
    Used TNSNAMES adapter to resolve the alias
    Attempting to contact (DESCRIPTION = (ADDRESS_LIST = (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE2)(PORT = 1521))) (CONNECT_DATA = (SER
    VICE_NAME = test_stby)))
    OK (260 msec)


##<a name="start-up-the-standby-instance-in-nomount-state"></a>De stand-by-instantie in staat nomount starten
Voor het instellen van de omgeving voor de ondersteuning van de stand-by-database op de stand-by virtuele Machine (MACHINE2).

Ten eerste het wachtwoordbestand van de primaire computer (Machine1) naar de stand-by-machine (Machine2) handmatig kopiëren. Dit is nodig als het **sys** -wachtwoord identiek op beide computers zijn moet.

Vervolgens opent u de opdrachtprompt van Windows in Machine2 en instellen van de omgevingsvariabelen die verwijzen naar de stand-by-database als volgt:

    SET ORACLE_HOME=C:\OracleDatabase\product\11.2.0\dbhome_1
    SET ORACLE_SID=TEST_STBY

Vervolgens start u de stand-by-database in nomount staat en vervolgens een spfile te genereren.

Starten van de database:

    SQL>shutdown immediate;

    SQL>startup nomount
    ORACLE instance started.

    Total System Global Area  747417600 bytes
    Fixed Size                  2179496 bytes
    Variable Size             473960024 bytes
    Database Buffers          264241152 bytes
    Redo Buffers                7036928 bytes


##<a name="use-rman-to-clone-the-database-and-to-create-a-standby-database"></a>RMAN kopie van de database en voor het maken van een stand-by-database gebruiken
U kunt het hulpprogramma Systeemherstel Manager (RMAN) nemen van een back-up van de primaire database op de stand-by fysieke database maken.

Extern bureaublad in de stand-by Virtual Machine (MACHINE2) en het hulpprogramma RMAN uitvoeren door op te geven van een volledige verbinding string, voor zowel het doel (primaire database Machine1) en HULPAPPARAAT (stand-by-database, Machine2) exemplaren.

>[AZURE.IMPORTANT] De verificatie van het besturingssysteem niet gebruiken als er nog geen database in de servercomputer stand-by.

    C:\> RMAN TARGET sys/password@test AUXILIARY sys/password@test_STBY

    RMAN>DUPLICATE TARGET DATABASE
      FOR STANDBY
      FROM ACTIVE DATABASE
      DORECOVER
        NOFILENAMECHECK;

##<a name="start-the-physical-standby-database-in-managed-recovery-mode"></a>De stand-by fysieke database starten in de herstelmodus beheerde
Deze zelfstudie laat zien hoe een fysieke stand-by-database te maken. Zie de documentatie van Oracle voor meer informatie over het maken van een logische stand-by-database.

Open SQL\*Plus de opdrachtprompt en de afscherming van de gegevens op de stand-by virtuele Machine of server (MACHINE2) als volgt inschakelen:

    SHUTDOWN IMMEDIATE;
    STARTUP MOUNT;
    ALTER DATABASE RECOVER MANAGED STANDBY DATABASE DISCONNECT FROM SESSION;

Als u de stand-by-database opent in de modus **koppelen** het logboek archiveren verzending blijft en het herstelproces beheerde logboek toe te passen op de stand-by database blijft. Dit zorgt ervoor dat de stand-by-database met de primaire database up-to-date blijft. Houd er rekening mee dat de stand-by-database kan niet toegankelijk voor rapportagedoeleinden gedurende deze periode.

Als u de stand-by-database opent in de modus **Alleen lezen** , het archief Meld verzending wordt voortgezet. Maar stopt met het herstelproces beheerde. Hierdoor wordt de stand-by-database worden steeds bijgewerkt totdat de beheerde recovery-verwerking wordt hervat. Maar u kunt de stand-by-database voor rapportagedoeleinden gedurende deze tijd gegevens mogelijk niet de laatste wijzigingen.

In het algemeen is het raadzaam de stand-by-database in de modus **koppelen** aan de gegevens in de stand-by-database up-to-date houden als er een storing in de primaire database te houden. Echter, kunt u de stand-by-database in de modus **Lezen alleen** voor rapportagedoeleinden, afhankelijk van de vereisten van uw toepassing. De volgende stappen geven aan het inschakelen van de afscherming van de gegevens in de modus alleen-lezen met behulp van SQL\*Plus:

    SHUTDOWN IMMEDIATE;
    STARTUP MOUNT;
    ALTER DATABASE OPEN READ ONLY;


##<a name="verify-the-physical-standby-database"></a>Controleer of de fysieke database stand-by
In deze sectie wordt beschreven hoe om te controleren of de configuratie met hoge beschikbaarheid als beheerder.

Open SQL\*Plus een opdrachtpromptvenster en selectievakje gearchiveerde logboek op de stand-by virtuele Machine (Machine2) opnieuw:

    SQL> show parameters db_unique_name;

    NAME                                TYPE       VALUE
    ------------------------------------ ----------- ------------------------------
    db_unique_name                      string     TEST_STBY

    SQL> SELECT NAME FROM V$DATABASE

    SQL> SELECT SEQUENCE#, FIRST_TIME, NEXT_TIME, APPLIED FROM V$ARCHIVED_LOG ORDER BY SEQUENCE#;

    SEQUENCE# FIRST_TIM NEXT_TIM APPLIED
    ----------------  ---------------  --------------- ------------
    45                    23-FEB-14   23-FEB-14   YES
    45                    23-FEB-14   23-FEB-14   NO
    46                    23-FEB-14   23-FEB-14   NO
    46                    23-FEB-14   23-FEB-14   YES
    47                    23-FEB-14   23-FEB-14   NO
    47                    23-FEB-14   23-FEB-14   NO

Open SQL * Plus opdrachtprompt venster- en logboekbestanden op de primaire computer (Machine1):

    SQL> alter system switch logfile;
    System altered.

    SQL> archive log list
    Database log mode              Archive Mode
    Automatic archival             Enabled
    Archive destination            C:\OracleDatabase\archive
    Oldest online log sequence     69
    Next log sequence to archive   71
    Current log sequence           71

Controleer opnieuw gearchiveerde logboek op de stand-by virtuele Machine (Machine2):

    SQL> SELECT SEQUENCE#, FIRST_TIME, NEXT_TIME, APPLIED FROM V$ARCHIVED_LOG ORDER BY SEQUENCE#;

    SEQUENCE# FIRST_TIM NEXT_TIM APPLIED
    ----------------  ---------------  --------------- ------------
    45                    23-FEB-14   23-FEB-14   YES
    46                    23-FEB-14   23-FEB-14   YES
    47                    23-FEB-14   23-FEB-14   YES
    48                    23-FEB-14   23-FEB-14   YES

    49                    23-FEB-14   23-FEB-14   YES
    50                    23-FEB-14   23-FEB-14   IN-MEMORY

Controleren op eventuele ruimte op de stand-by virtuele Machine (Machine2):

    SQL> SELECT * FROM V$ARCHIVE_GAP;
    no rows selected.

Een andere verificatiemethode kan worden om failover naar de stand-by-database en test indien mogelijk voor failback naar primaire database. Om de stand-by-database als primaire database activeert, gebruikt u de volgende instructies:

    SQL> ALTER DATABASE RECOVER MANAGED STANDBY DATABASE FINISH;
    SQL> ALTER DATABASE ACTIVATE STANDBY DATABASE;

Als u flashback op de oorspronkelijke primaire database niet hebt ingeschakeld, het raadzaam dat u de oorspronkelijke primaire database weghalen en opnieuw als een stand-by-database maken.

Wij raden u aan in te schakelen flashback-database op de primaire en de stand-by-databases. Als een failover gebeurt, kunt u de primaire database geknipperd terug naar de tijd vóór de overname en snel worden omgezet in een stand-by-database.

##<a name="additional-resources"></a>Aanvullende bronnen
[Oracle VM afbeeldingen voor Azure](virtual-machines-windows-classic-oracle-images.md)
