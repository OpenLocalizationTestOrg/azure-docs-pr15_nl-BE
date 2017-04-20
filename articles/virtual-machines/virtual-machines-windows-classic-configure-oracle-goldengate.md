<properties
    pageTitle="Oracle GoldenGate configureren in VMs | Microsoft Azure"
    description="Stap voor stap een zelfstudie voor het opzetten en implementeren van Oracle GoldenGate op Azure Windows Server VMs voor hoge beschikbaarheid en disaster recovery."
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


#<a name="configuring-oracle-goldengate-for-azure"></a>Oracle GoldenGate configureren voor Azure


Deze zelfstudie laat zien hoe om Oracle-GoldenGate voor virtuele Machines van Azure-omgeving voor hoge beschikbaarheid en noodherstel. De zelfstudie is gericht op [bidirectionele replicatie](http://docs.oracle.com/goldengate/1212/gg-winux/GWUAD/wu_about_gg.htm) voor RAC Oracle-databases en moet beide sites actief zijn.

GoldenGate Oracle ondersteunt gegevensdistributie en integratie van gegevens. Deze kunt u voor het instellen van een verdeling van de gegevens en een oplossing voor synchronisatie met de replicatieconfiguratie voor Oracle-Oracle en biedt een hoge beschikbaarheid flexibele oplossing. Oracle Data Guard vult Oracle GoldenGate met de Replicatiemogelijkheden om informatie van de hele onderneming distributie en zero downtime upgrades en migraties. Zie voor gedetailleerde informatie [Met behulp van Oracle GoldenGate met Oracle Data Guard](http://docs.oracle.com/cd/E11882_01/server.112/e17157/unplanned.htm).

Oracle GoldenGate bevat de volgende hoofdonderdelen: extraheren, gegevens pomp, Replicat, spoor of uitpakken van bestanden, controlepunten, Manager en verzamelaar. Als bidirectionele replicatie tussen twee sites, die u wilt maken en alle onderdelen op beide sites starten. Zie voor gedetailleerde informatie over de GoldenGate van Oracle-architectuur, [Oracle GoldenGate gids](http://docs.oracle.com/goldengate/1212/gg-winux/index.html).

In deze zelfstudie wordt ervan uitgegaan dat u al theoretische en praktische kennis op Oracle Database hoge beschikbaarheid en noodherstel-begrippen zoals [Oracle GoldenGate](http://docs.oracle.com/goldengate/1212/gg-winux/index.html). Zie de [website van Oracle](http://www.oracle.com/technetwork/database/features/availability/index.html)voor meer informatie.

Bovendien de zelfstudie wordt ervan uitgegaan dat u de volgende vereiste onderdelen al hebt geïmplementeerd:

- U hebt het gedeelte voor hoge beschikbaarheid en herstel na storing herstel overwegingen in het onderwerp [afbeeldingen Oracle Virtual Machine - diverse overwegingen](virtual-machines-windows-classic-oracle-considerations.md) al hebt bekeken. Houd er rekening mee dat Azure Oracle Database-exemplaren voor zelfstandige maar niet Oracle Real Application Clusters (Oracle RAC) op dit moment ondersteunt.

- U kunt de GoldenGate van Oracle-software hebt gedownload van de website met [Downloads van Oracle](http://www.oracle.com/us/downloads/index.html) . U hebt het Product Pack Oracle Fusion Middleware – Data Integration geselecteerd. Vervolgens geselecteerde Oracle GoldenGate op Oracle v11.2.1 Media Pack voor Microsoft Windows x64 (64-bits) voor een database van Oracle 11 g. Vervolgens downloadt Oracle GoldenGate V11.2.1.0.3 voor Oracle 11g 64-bits Windows 2008 (64-bits).

- U hebt twee virtuele Machines (VMs) in Azure met Oracle Enterprise Edition op Windows Server gemaakt. Zorg ervoor dat de virtuele Machines in de [dezelfde cloud service](virtual-machines-linux-load-balance.md) en in hetzelfde [Virtuele netwerk](https://azure.microsoft.com/documentation/services/virtual-network/) zodat ze toegang tot elkaar via het permanente persoonlijke IP-adres zijn.

- U kunt de namen van de virtuele Machine hebt ingesteld als 'MachineGG1' voor Site A en Site b "MachineGG2" op de klassieke Azure portal.

- Gemaakte test databases 'TestGG1' op de Site A en "TestGG2" op de Site B.

- U aanmelden met uw Windows-server als een lid van de groep Administrators of als lid van de groep **ORA_DBA** .

In deze zelfstudie wordt u:

1. Setup-database op de Site A en Site B  

    1. Eerste import van gegevens uitvoeren

2. Site A en Site B voorbereiden voor databasereplicatie

3. Alle nodig ter ondersteuning van de DDL-replicatie-objecten maken

4. GoldenGate Manager configureren op Site A en Site B

5. Pak groep maken en Data Pump processen op de Site A en Site B

    1. Extract en Data Pump processen op een Site maken

    2. Maken van een tabel met controlepunten GoldenGate op Site B

    3. REPLICAT op B-Site toevoegen

    4. Extract en Data Pump processen maken op Site B

    5. Een tabel met controlepunten GoldenGate op een Site maken

    6. REPLICAT op een Site toevoegen

    7. Trandata toevoegen aan Site A en Site B

    8. Extract en Data Pump processen starten op een Site

    9. Extract en Data Pump processen starten op Site B

    10. REPLICAT starten op een Site

    11. REPLICAT op Site B starten

6. Controleer of het replicatieproces bi-directionele

>[AZURE.IMPORTANT] Deze zelfstudie is setup en getest met de volgende configuratie:
>
>|                        | **Een Database-site**              | **Database met site B**              |
>|------------------------|----------------------------------|----------------------------------|
>| **Oracle-Release**     | Oracle11g versie 2-(11.2.0.1) | Oracle11g versie 2-(11.2.0.1) |
>| **Naam van de computer**       | MachineGG1                       | MachineGG2                       |
>| **Besturingssysteem**   | Windows 2008 R2                  | Windows 2008 R2                  |
>| **Oracle-SID**         | TESTGG1                          | TESTGG2                          |
>| **Schema van replicatie** | SCOTT                            | SCOTT                            |

Voor latere versies van Oracle Database en Oracle GoldenGate, is er mogelijk enkele aanvullende wijzigingen die u moet implementeren. Zie voor de meest recente versie informatie-documentatie op de website van Oracle [Oracle Database](http://www.oracle.com/us/corporate/features/database-12c/index.html) en [Oracle GoldenGate](http://docs.oracle.com/goldengate/1212/gg-winux/index.html) . Bijvoorbeeld, voor een brondatabase release 11.2.0.4 en later, wordt de opname van DDL asynchroon wordt uitgevoerd door de server logmining en vereist geen speciale triggers, tabellen of andere objecten in de database worden geïnstalleerd. Oracle GoldenGate upgrades kunnen worden uitgevoerd zonder de gebruikerstoepassingen stoppen. Het gebruik van een DDL-trigger en ondersteunende objecten is vereist wanneer het uittreksel is in de geïntegreerde modus met een Oracle 11g source-database die ouder is dan versie 11.2.0.4. Zie voor gedetailleerde richtsnoeren [installeren en configureren van Oracle GoldenGate voor Oracle-Database](http://docs.oracle.com/goldengate/1212/gg-winux/GIORA.pdf).

##<a name="1-setup-database-on-site-a-and-site-b"></a>1. de database op de Site A en Site B instellen
In dit gedeelte wordt uitgelegd hoe u de vereisten voor de database uitvoeren op zowel Site A en Site b heeft. U moet alle stappen van dit gedeelte uitvoeren op beide sites: Site A en Site b heeft.

Eerste, extern bureaublad Site A en Site B via de klassieke Azure portal. Open een opdrachtprompt van Windows en maakt u een basismap voor Oracle GoldenGate setup-bestanden:

    mkdir C:\OracleGG

Vervolgens unzip en installeer de GoldenGate van Oracle-software in deze map. Na deze stap kan de GoldenGate Software opdracht-Interpreter (GGSCI) worden geactiveerd door het uitvoeren van de volgende opdracht:

    C:\OracleGG\.\ggsci

U kunt [GGSCI](http://docs.oracle.com/goldengate/1212/gg-winux/GWUAD/wu_gettingstarted.htm) verschillende opdrachten configureren, controle en monitor Oracle GoldenGate.

Voer de volgende opdracht om alle onderliggende mappen uit het installatiepakket maken:

    GGSCI (Hostname) 1> CREATE SUBDIRS

Sluit de opdrachtprompt GGSCI de volgende opdracht uitvoeren:

    GGSCI (Hostname) 1> EXIT

Vervolgens moet u voor het maken van een databasegebruiker, die worden gebruikt door de processen van Oracle GoldenGate Manager, Extract en replicatie. Opmerking afzonderlijke gebruikers voor elk proces of slechts één gemeenschappelijke gebruiker configureren. In deze zelfstudie maken we een gebruiker die als ggate wordt genoemd. Geven wij die gebruiker de benodigde bevoegdheden. Houd er rekening mee dat u de volgende bewerkingen uitvoeren op Site A en Site b heeft moet.

Open SQL\*Plus opdrachtvenster op Site A en Site B met administrator-bevoegdheden van een database met **SYSDBA**, zoals:

    Enter username: / as sysdba

En uit te voeren:

    SQL> create tablespace ggs_data   datafile 'c:\OracleDatabase\oradata\<DBNAME>\<DBNAME>ggs_data01.dbf' size 200m;
    SQL> create user ggate identified by ggate default tablespace ggs_data  temporary tablespace temp;
          grant connect, resource to ggate;
          grant select any dictionary, select any table to ggate;
          grant create table to ggate;
          grant flashback any table to ggate;
          grant execute on dbms_flashback to ggate;
          grant execute on utl_file to ggate;
          grant create any table to ggate;
          grant insert any table to ggate;
          grant update any table to ggate;
          grant delete any table to ggate;
          grant drop any table to ggate;

Zoek vervolgens de INIT\<DatabaseSID\>. ORA-bestand in de map % ORACLE\_HOME %\\map op Site A en Site B en de volgende databaseparameters toevoegen aan INITTEST.ora:

    UNDO\_MANAGEMENT=AUTO
    UNDO\_RETENTION=86400

Zie [Naslaginformatie voor Oracle GoldenGate voor Windows](http://docs.oracle.com/goldengate/1212/gg-winux/GWURF/ggsci_commands.htm)voor een volledige lijst met alle opdrachten van Oracle GoldenGate GGSCI.

### <a name="perform-initial-data-load"></a>Eerste import van gegevens uitvoeren

Door verschillende methoden kunt u de eerste import van gegevens in de database uitvoeren. Bijvoorbeeld, kunt u de [Directe belasting van Oracle GoldenGate](http://docs.oracle.com/goldengate/1212/gg-winux/GWUAD/wu_initsync.htm) of gewone hulpprogramma's importeren en exporteren naar gegevens in een tabel exporteren van Site A naar Site b heeft.

Voorbeelden van het replicatieproces Oracle GoldenGate, ziet u deze zelfstudie maken van een tabel met de volgende opdrachten op zowel Site A en site B.

Open eerst een SQL\*Plus de opdracht venster en voer de volgende opdracht voor het maken van een voorraadtabel in Site A en Site B databases:

    create table scott.inventory
    (prod_id number,
    prod_category varchar2(20),
    qty_in_stock number,
    last_dml timestamp default systimestamp);

Vervolgens wordt een beperking toevoegen aan de nieuwe tabel op de Site A en Site B-databases:

    alter table scott.inventory add constraint pk_inventory primary key (prod_id) ;

Vervolgens verlenen alle bevoegdheden die op de nieuwe tabel op de ggate van de gebruiker op de Site A en Site b

    grant all on scott.inventory to ggate;

Vervolgens maken en inschakelen van een databasetrigger, INVENTORY_CDR_TRG, op de nieuwe tabel om ervoor te zorgen dat alle transacties aan de nieuwe tabel worden geregistreerd als de gebruiker geen ggate. Deze bewerking uitvoeren op de Site A en Site b heeft.

    CREATE OR REPLACE TRIGGER INVENTORY_CDR_TRG
    BEFORE UPDATE
    ON SCOTT.INVENTORY
    REFERENCING NEW AS New OLD AS Old
    FOR EACH ROW
    BEGIN
    IF SYS_CONTEXT ('USERENV', 'SESSION_USER') != 'GGATE'
    THEN
    :NEW.LAST_DML := SYSTIMESTAMP;
    END IF;
    END;
    /


##<a name="2-prepare-site-a-and-site-b-for-database-replication"></a>2. voorbereiden Site A en Site B databasereplicatie
In dit gedeelte wordt uitgelegd hoe u Site A en Site B voorbereiden voor databasereplicatie. U moet alle stappen van dit gedeelte uitvoeren op beide sites: Site A en Site b heeft.

Eerste, extern bureaublad Site A en Site B via de klassieke Azure portal. Activeer de database archivelog modus met behulp van de SQL * Plus opdrachtvenster:

    sql>shutdown immediate
    sql>startup mount
    sql>alter database archivelog;
    sql>alter database open;


Vervolgens minimaal [aanvullende logboekregistratie](http://docs.oracle.com/cd/E11882_01/server.112/e22490/logminer.htm) als volgt inschakelen:

    SQL> ALTER DATABASE ADD SUPPLEMENTAL LOG DATA (ALL) COLUMNS;

Bereid vervolgens de database ter ondersteuning van DDL (data definition language)-replicatie:

    SQL> alter system set recyclebin=off scope=spfile;

Vervolgens, afsluiten en opnieuw opstarten van de database:

    sql>shutdown immediate
    sql>startup


##<a name="3-create-all-necessary-objects-to-support-ddl-replication"></a>3. alle noodzakelijk ter ondersteuning van de DDL-replicatie-objecten maken
In deze sectie worden de scripts die u gebruiken wilt voor het maken van alle noodzakelijke objecten ter ondersteuning van de DDL-replicatie. Voer de scripts die zijn opgegeven in deze sectie op zowel Site A en Site b heeft.

Open een opdrachtprompt van Windows en Ga naar de map GoldenGate van Oracle, zoals C:\\OracleGG. Start SQL\*Plus een opdrachtprompt met beheerdersbevoegdheden database, zoals het gebruik van **SYSDBA** op Site A en Site b heeft.

Voer de volgende scripts:

    SQL> @marker_setup.sql  
    Enter GoldenGate schema name: ggate
    SQL> @ddl_setup.sql  
    Enter GoldenGate schema name: ggate
    SQL> @role_setup.sql
    Enter GoldenGate schema name: ggate
    SQL> grant ggs_ggsuser_role to ggate;
     Grant succeeded.
     SQL> @ddl_enable
     Trigger altered.
     SQL> @ddl_pin ggate


Een tabel niveau aanmelding vereist Oracle GoldenGate gereedschap voor ondersteuning van DDL (data definition language). Daarom, aanvullende logboekregistratie op het tabelniveau van de met de opdracht ADD TRANDATA inschakelen. Oracle GoldenGate opdracht-interpreter venster database-aanmeldingen te openen en vervolgens de opdracht ADD TRANDATA uitvoeren:

    GGSCI 5> DBLOGIN USERID ggate, PASSWORD ggate

    GGSCI(Hostname) 6> add trandata scott.inventory

##<a name="4-configure-goldengate-manager-on-site-a-and-site-b"></a>4. GoldenGate Manager configureren op Site A en Site B
De Oracle GoldenGate Manager voert een aantal functies zoals het starten van de andere GoldenGate processen, beheer van logboekbestanden trail en rapportage.

U moet het beheer van Oracle-GoldenGate configureren op zowel Site A en Site b heeft. U doet dit door de volgende stappen uitvoeren op de Site A en Site b heeft.

Geopende vensters opdracht venster en het opdrachtverwerkingsprogramma Oracle GoldenGate starten:

    cd C:\OracleGG\
    c:\OracleGG>ggsci
    Oracle GoldenGate Command Interpreter for Oracle
    Version 11.2.1.0.3 14400833 OGGCORE_11.2.1.0.3_PLATFORMS_120823.1258
    Windows x64 (optimized), Oracle 11g on Aug 23 2012 16:50:36
    Copyright (C) 1995, 2012, Oracle and/or its affiliates. All rights reserved.


De sessie GGSCI registreert in een database, zodat u opdrachten die betrekking hebben op de database kunt uitvoeren:

    GGSCI (HostName) 1> DBLOGIN USERID ggate, PASSWORD ggate
    Successfully logged into database.

De status en vertragingstijd (indien relevant) voor beheer, uittreksel en Replicat processen op een systeem:

    GGSCI (HostName) 2> info all
    Program     Status      Group       Lag           Time Since Chkpt
    MANAGER     STOPPED

De parameterbestand met de opdracht parameters bewerken te openen en vervolgens de volgende informatie toevoegen:

    GGSCI (HostName) 3> edit params mgr
    PORT 7809
    USERID ggate, PASSWORD ggate
    PURGEOLDEXTRACTS  C:\OracleGG\dirdat\ex, USECHECKPOINTS

De status en vertragingstijd (indien relevant) voor beheer, uittreksel en Replicat processen op een systeem:

    GGSCI (HostName) 46> info all
    Program     Status      Group       Lag           Time Since Chkpt
    MANAGER     STOPPED

De sessie GGSCI registreert in een database, zodat u opdrachten die betrekking hebben op de database kunt uitvoeren:

    GGSCI (HostName) 47> dblogin USERID ggate, PASSWORD ggate

    Successfully logged into database.

Start het proces van het beheer:

    GGSCI (HostName) 48> start manager
    Manager started.

##<a name="5-create-extract-group-and-data-pump-processes-on-site-a-and-site-b"></a>5. Maak Extract en Data Pump processen op de Site A en Site B

###<a name="create-extract-and-data-pump-processes-on-site-a"></a>Extract en Data Pump processen op een Site maken

U moet het uittreksel en Data Pump processen op Site A en Site b extern bureaublad Site A en Site B via de klassieke Azure portal maken. GGSCI opdracht-interpreter-venster geopend. Voer de volgende opdrachten op de Site A:

    GGSCI (MachineGG1) 14> add extract ext1 tranlog begin now
    EXTRACT added.
    GGSCI (MachineGG1) 4> add exttrail C:\OracleGG\dirdat\lt, extract ext1
    EXTTRAIL added.
    GGSCI (MachineGG1) 16> add extract dpump1 exttrailsource C:\OracleGG\dirdat\aa
    EXTRACT added.
    GGSCI (MachineGG1) 17> add rmttrail C:\OracleGG\dirdat\ab extract dpump1
    RMTTRAIL added.

De parameterbestand met de opdracht parameters bewerken te openen en voegt u de volgende informatie: GGSCI (MachineGG1) 18 > bewerken params ext1 EXTRACT ext1 gebruikers-id ggate, wachtwoord-ggate EXTTRAIL C:\OracleGG\dirdat\aa TRANLOGOPTIONS EXCLUDEUSER ggate tabel scott.inventory, GETBEFORECOLS (ON UPDATE KEYINCLUDING (prod_category, qty_in_stock, last_dml), op verwijderen KEYINCLUDING (prod_category, qty_in_stock, last_dml));

De parameterbestand met de opdracht parameters bewerken te openen en vervolgens de volgende informatie toevoegen:

    GGSCI (MachineGG1) 15> edit params dpump1
    EXTRACT dpump1
     USERID ggate, PASSWORD ggate
     RMTHOST ActiveGG2orcldb, MGRPORT 7809, TCPBUFSIZE 100000
     RMTTRAIL C:\OracleGG\dirdat\ab
     PASSTHRU
     TABLE scott.inventory;

###<a name="create-a-goldengate-checkpoint-table-on-site-b"></a>Maken van een tabel met controlepunten GoldenGate op Site B

Vervolgens moet u een tabel met controlepunten toevoegen op Site b heeft. Hiervoor moet u een GoldenGate opdracht-interpreter venster openen en uitvoeren:

    C:\OracleGG\ggsci
    GGSCI (MachineGG2) 1> DBLOGIN USERID ggate, PASSWORD ggate
    Successfully logged into database.

En vervolgens de tabel met controlepunten toevoegen aan de database, waarbij ggate staat voor de eigenaar:

    GGSCI (MachineGG2) 2> ADD CHECKPOINTTABLE ggate.checkpointtable
    Successfully created checkpoint table ggate.checkpointtable.

De naam van de tabel selectievakje punt toevoegen aan het bestand GLOBALS op de doelserver die Site B in deze stap. Bewerk het bestand GLOBALS op Site B:

    GGSCI (MachineGG2) 1> EDIT PARAMS ./GLOBALS

Vervolgens voegt u de parameter CHECKPOINTTABLE aan bestaande GLOBALS:

    GGSCHEMA ggate
    CHECKPOINTTABLE ggate.checkpointtable

Een laatste stap opslaan en sluit het bestand GLOBALS parameter.


###<a name="add-replicat-on-site-b"></a>REPLICAT op B-Site toevoegen
In deze sectie wordt beschreven hoe u een REPLICAT-proces 'REP2' toevoegen aan Site B.

De opdracht REPLICAT ADD gebruiken voor het maken van een groep Replicat op Site B:

    GGSCI (MachineGG2) 37> add replicat rep2 exttrail C:\OracleGG\dirdatab, checkpointtable ggate.checkpointtable

De parameterbestand met de opdracht parameters bewerken te openen en vervolgens de volgende informatie toevoegen:

    GGSCI (MachineGG2) 10> edit params rep2
    REPLICAT rep2
    ASSUMETARGETDEFS
    USERID ggate, PASSWORD ggate
    DISCARDFILE C:\OracleGG\dirdat\discard.txt, append,megabytes 10
    MAP scott.inventory, TARGET scott.inventory;

###<a name="create-extract-and-data-pump-processes-on-site-b"></a>Extract en Data Pump processen maken op Site B

In deze sectie wordt beschreven hoe u een nieuwe extract 'EXT2' en een nieuwe gegevens pomp-proces 'DPUMP2' op de Site B:

    GGSCI (MachineGG2) 3> add extract ext2 tranlog begin now
     EXTRACT added.
    GGSCI (MachineGG2) 4> add exttrail C:\OracleGG\dirdat\ac extract ext2
     EXTTRAIL added.
    GGSCI (MachineGG2) 5> add extract dpump2 exttrailsource C:\OracleGG\dirdat\ac
     EXTRACT added.
    GGSCI (MachineGG2) 6> add rmttrail C:\OracleGG\dirdat\ad extract dpump2
     RMTTRAIL added.

De parameterbestand met de opdracht parameters bewerken te openen en vervolgens de volgende informatie toevoegen:

    GGSCI (MachineGG2) 31> edit params ext2
    EXTRACT ext2
    USERID ggate, PASSWORD ggate
    EXTTRAIL C:\OracleGG\dirdat\ac
    TRANLOGOPTIONS EXCLUDEUSER ggate
    TABLE scott.inventory,
    GETBEFORECOLS (
    ON UPDATE KEYINCLUDING (prod_category,qty_in_stock, last_dml),
    ON DELETE KEYINCLUDING (prod_category,qty_in_stock, last_dml));

De parameterbestand met de opdracht parameters bewerken te openen en vervolgens de volgende informatie toevoegen:

    GGSCI (MachineGG2) 32> edit params dpump2
    EXTRACT dpump2
    USERID ggate, PASSWORD ggate
    RMTHOST MachineGG1, MGRPORT 7809, TCPBUFSIZE 100000
    RMTTRAIL C:\OracleGG\dirdat\ad
    PASSTHRU
    TABLE scott.inventory;

###<a name="create-a-goldengate-checkpoint-table-on-site-a"></a>Een tabel met controlepunten GoldenGate op een Site maken

Oracle GoldenGate-interpreter opdrachtvenster te openen en maak een tabel met controlepunten:

    GGSCI (MachineGG1) 1> DBLOGIN USERID ggate, PASSWORD ggate
    Successfully logged into database.

    GGSCI (MachineGG1) 2> ADD CHECKPOINTTABLE ggate.checkpointtable

    Successfully created checkpoint table ggate.checkpointtable.

Ook moet u de naam van de tabel selectievakje punt toevoegen aan het bestand GLOBALS op Site A.

Oracle GoldenGate-interpreter opdrachtvenster te openen en bewerken van het bestand GLOBALS op Site A:

    GGSCI (MachineGG1) 1> EDIT PARAMS ./GLOBALS
    Add the CHECKPOINTTABLE parameter to the existing GLOBALS file as follows:
    GGSCHEMA ggate
    CHECKPOINTTABLE ggate.checkpointtable

Opslaan en sluit het bestand GLOBALS parameter.

###<a name="add-replicat-on-site-a"></a>REPLICAT op een Site toevoegen

In deze sectie wordt beschreven hoe u een REPLICAT-proces 'REP1' toevoegen aan Site A.

De volgende opdracht maakt een groep Replicat rep1 met de naam van een spoor en de bijbehorende checkpointtable.

    GGSCI (MachineGG1) 21> add replicat rep1 exttrail C:\OracleGG\dirdat\ad,checkpointtable ggate.checkpointtable
     REPLICAT added.

De parameterbestand met de opdracht parameters bewerken te openen en vervolgens de volgende informatie toevoegen:

    GGSCI (MachineGG1) 10> edit params rep1
    REPLICAT rep1
    ASSUMETARGETDEFS
    USERID ggate, PASSWORD ggate
    DISCARDFILE C:\OracleGG\dirdat\discard.txt, append, megabytes 10
    MAP scott.inventory, TARGET scott.inventory;

### <a name="add-trandata-on-site-a-and-site-b"></a>Trandata toevoegen aan Site A en Site B

Aanvullende logboekregistratie op tabelniveau inschakelen met de opdracht ADD-TRANDATA. Oracle GoldenGate opdracht-interpreter venster database-aanmeldingen te openen en voer vervolgens de opdracht ADD-TRANDATA.

Extern bureaublad voor MachineGG1, Oracle GoldenGate opdracht-interpreter te openen en uitvoeren:

    GGSCI (MachineGG1) 11> dblogin userid ggate password ggate
     Successfully logged into database.
    GGSCI (MachineGG1) 12> add trandata scott.inventory cols (prod_category,qty_in_stock, last_dml)
    GGSCI (MachineGG1) 13> info trandata scott.inventory
    Logging of supplemental redo log data is enabled for table SCOTT.INVENTORY.
    Columns supplementally logged for table SCOTT.INVENTORY: PROD_ID, PROD_CATEGORY, QTY_IN_STOCK, LAST_DML.

Extern bureaublad voor MachineGG2, Oracle GoldenGate opdracht-interpreter te openen en uitvoeren:

    GGSCI (MachineGG2) 18> dblogin userid ggate password ggate
     Successfully logged into database.
    GGSCI (MachineGG2) 14> add trandata scott.inventory cols (prod_category,qty_in_stock, last_dml)
    Logging of supplemental redo data enabled for table SCOTT.INVENTORY.

Informatie weergeven over de status van de tabel niveau aanvullende logboekregistratie:

    GGSCI (MachineGG2) 15> info trandata scott.inventory
    Logging of supplemental redo log data is enabled for table SCOTT.INVENTORY.
    Columns supplementally logged for table SCOTT.INVENTORY: PROD_ID, PROD_CATEGORY, QTY_IN_STOCK, LAST_DML.

###<a name="add-trandata-on-site-a-and-site-b"></a>Trandata toevoegen aan Site A en Site B

Aanvullende logboekregistratie op tabelniveau inschakelen met de opdracht ADD-TRANDATA. Oracle GoldenGate opdracht-interpreter venster database-aanmeldingen te openen en voer vervolgens de opdracht ADD-TRANDATA.

Extern bureaublad voor MachineGG1, Oracle GoldenGate opdracht-interpreter te openen en uitvoeren:

    GGSCI (MachineGG1) 11> dblogin userid ggate password ggate
     Successfully logged into database.
    GGSCI (MachineGG1) 12> add trandata scott.inventory cols (prod_category,qty_in_stock, last_dml)
    GGSCI (MachineGG1) 13> info trandata scott.inventory
    Logging of supplemental redo log data is enabled for table SCOTT.INVENTORY.
    Columns supplementally logged for table SCOTT.INVENTORY: PROD_ID, PROD_CATEGORY, QTY_IN_STOCK, LAST_DML.

Extern bureaublad voor MachineGG2, Oracle GoldenGate opdracht-interpreter te openen en uitvoeren:

    GGSCI (MachineGG2) 18> dblogin userid ggate password ggate
     Successfully logged into database.
    GGSCI (MachineGG2) 14> add trandata scott.inventory cols (prod_category,qty_in_stock, last_dml)
    Logging of supplemental redo data enabled for table SCOTT.INVENTORY.
    Display information about the state of table-level supplemental logging:
    GGSCI (MachineGG2) 15> info trandata scott.inventory
    Logging of supplemental redo log data is enabled for table SCOTT.INVENTORY.
    Columns supplementally logged for table SCOTT.INVENTORY: PROD_ID, PROD_CATEGORY, QTY_IN_STOCK, LAST_DML.

###<a name="start-extract-and-data-pump-processes-on-site-a"></a>Extract en Data Pump processen starten op een Site

Start het proces Extract ext1 op Site A:

    GGSCI (MachineGG1) 31> start extract ext1
    Sending START request to MANAGER …
    EXTRACT EXT1 starting

De dpump1 gegevens pomp proces starten op Site A:

    GGSCI (MachineGG1) 23> start extract dpump1
    Sending START request to MANAGER …
    EXTRACT DPUMP1 starting
Informatie weergeven over de groep Extract ext1: GGSCI (MachineGG1) 32 > info uitpakken van ext1 PAKKEN EXT1 laatste gestart 2013-11-25 08:03 Status actief controlepunt Lag 00:00:00 (bijgewerkte 00:00:02 geleden) logboek lezen Checkpoint Oracle opnieuw logboeken 2013-11-25 08:03:18 het Seqno 6, RBA 3230720 SCN 0.1074371 (1074371)

De status en vertragingstijd (indien relevant) voor beheer, uittreksel en Replicat processen op een systeem:

    GGSCI (MachineGG1) 16> info all
    Program     Status      Group       Lag at Chkpt  Time Since Chkpt

    MANAGER     RUNNING
    EXTRACT     RUNNING     DPUMP1      00:00:00      00:46:33
    EXTRACT     RUNNING     EXT1        00:00:00      00:00:04

###<a name="start-extract-and-data-pump-processes-on-site-b"></a>Extract en Data Pump processen starten op Site B

Start het proces Extract ext2 op Site B:

    GGSCI (MachineGG2) 22> start extract ext2
    Sending START request to MANAGER …
    EXTRACT EXT2 starting

De dpump2 gegevens pomp proces starten op Site B:

    GGSCI (MachineGG2) 23> start extract dpump2
    Sending START request to MANAGER …
    EXTRACT DPUMP2 starting

De status en vertragingstijd (indien relevant) voor beheer, uittreksel en Replicat processen op een systeem:

    GGSCI (ActiveGG2orcldb) 6> info all
    Program     Status      Group       Lag at Chkpt  Time Since Chkpt

    MANAGER     RUNNING
    EXTRACT     RUNNING     DPUMP2      00:00:00      136:13:33
    EXTRACT     RUNNING     EXT2        00:00:00      00:00:04

### <a name="start-replicat-process-on-site-a"></a>REPLICAT starten op een Site

In deze sectie wordt beschreven hoe u start de REPLICAT 'REP1' op Site A.

Start de Replicat op de Site A:

    GGSCI (MachineGG1) 38> start replicat rep1
    Sending START request to MANAGER …
    REPLICAT REP1 starting

De status van een groep Replicat weergegeven:

    GGSCI (MachineGG1) 39> status replicat rep1
     REPLICAT REP1: RUNNING

###<a name="start-replicat-process-on-site-b"></a>REPLICAT op Site B starten

In deze sectie wordt beschreven hoe u start de REPLICAT 'REP2' op de Site B.

Start de Replicat op de Site B:

    GGSCI (MachineGG2) 26> start replicat rep2
    Sending START request to MANAGER …
    REPLICAT REP2 starting

De status van een groep Replicat weergegeven:

    GGSCI (MachineGG2) 27> status replicat rep2
    REPLICAT REP2: RUNNING

##<a name="6-verify-the-bi-directional-replication-process"></a>6. Controleer of het replicatieproces bi-directionele

Om te controleren of de configuratie van Oracle GoldenGate, voegt u een rij in de database op de Site A. extern bureaublad op Site A. Open up SQL * Plus een opdrachtvenster en uitvoeren: SQL > Selecteer de naam van v$ database.

    NAME
    ———
    TESTGG

    SQL> insert into inventory values  (100,’TV’,100,sysdate);

    1 row created.

    SQL> commit;

    Commit complete.

Controleer als die rij wordt gerepliceerd op Site b heeft. U doet dit door extern bureaublad op Site B. Open SQL Plus venster omhoog en uitvoeren:

    SQL> select name from v$database;

    NAME
    ———
    TESTGG

    SQL> select * from inventory;

    PROD_ID PROD_CATEGORY QTY_IN_STOCK LAST_DML
    ———- ——————– ———— ———
    100 TV 100 22-MAR-13

Een nieuwe record invoegen op Site B:

    SQL> insert into inventory  values  (101,’DVD’,10,sysdate);
    1 row created.

    SQL> commit;

    Commit complete.

Extern bureaublad voor Site A en controleer of de replicatie heeft plaatsgevonden:

    SQL> select * from inventory;

    PROD_ID PROD_CATEGORY QTY_IN_STOCK LAST_DML
    ———- ——————– ———— ———
    100 TV 100 22-MAR-13
    101 DVD 10 22-MAR-13

##<a name="additional-resources"></a>Aanvullende bronnen
[Oracle VM afbeeldingen voor Azure](virtual-machines-linux-classic-oracle-images.md)
