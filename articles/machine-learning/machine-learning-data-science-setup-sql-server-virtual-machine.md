<properties
    pageTitle="Instellen van een virtuele machine van SQL Server als een server IPython laptop | Microsoft Azure"
    description="Instellen van een virtuele Machine met SQL Server en de IPython Server in Data Science."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev" 
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="xibingao;bradsev" />

# <a name="set-up-an-azure-sql-server-virtual-machine-as-an-ipython-notebook-server-for-advanced-analytics"></a>Instellen van een virtuele machine van Azure SQL Server als een server IPython-laptops voor geavanceerde analytics

In dit onderwerp wordt beschreven hoe creëren en configureren van een virtuele machine van SQL Server moet worden gebruikt als onderdeel van een cloud-gebaseerde wetenschap gegevensomgeving. De virtuele Windows-computer is geconfigureerd met hulpmiddelen zoals IPython-laptop, Azure Opslagverkenner AzCopy en andere hulpprogramma's die gebruikt voor data science projecten worden ondersteunen. Azure Opslagverkenner en AzCopy, bijvoorbeeld handige manieren gegevens uploaden naar Azure blob-opslag van uw lokale computer of op de lokale computer downloaden vanaf de blob-opslag te bieden.

De galerie Azure virtuele machine bevat meerdere afbeeldingen met een Microsoft SQL Server. Selecteer een afbeelding VM van SQL Server die geschikt is voor de gegevensbehoeften van uw. Aanbevolen afbeeldingen zijn:

- SQL Server 2012 SP2 Enterprise voor kleine tot middelgrote data formaten
- SQL Server 2012 SP2 Enterprise geoptimaliseerd voor DataWarehousing werkbelasting voor grote tot zeer grote formaten

 > [AZURE.NOTE] SQL Server 2012 SP2 Enterprise afbeelding **bevat geen schijf met gegevens**. U moet toevoegen en/of een of meer virtuele harde schijven voor het opslaan van uw gegevens te koppelen. Wanneer u een Azure virtuele machine maakt, heeft een schijf voor het besturingssysteem die is toegewezen aan station C en een tijdelijke schijf is toegewezen aan het station D. Gebruik niet de D-schijf voor het opslaan van gegevens. Zoals de naam al aangeeft, biedt alleen tijdelijke opslag. Het biedt geen redundantie of back-up omdat deze zich niet in Azure opslag.


##<a name="Provision"></a>Verbinding maken met de klassieke Portal Azure en inrichten van een virtuele machine van SQL Server

1.  Log in op de [Azure klassieke Portal](http://manage.windowsazure.com/) met uw account.
    Als u niet een Azure-account hebt, gaat u naar [Azure gratis proefperiode](https://azure.microsoft.com/pricing/free-trial/).

2.  Op de klassieke Portal van Azure in de linkerbenedenhoek van de pagina, klik op **+ Nieuw**, klikt u op **berekenen**, klikt u op de **virtuele MACHINE**en klik op **GALERIE van**.

3.  Selecteer de afbeelding van een virtuele machine met SQL Server op basis van de gegevensbehoeften van uw op de pagina **maken van een virtuele Machine** en klik vervolgens op de pijl volgende gemarkeerd in de rechterbenedenhoek van de pagina. Voor de meest recente informatie over de ondersteunde SQL Server afbeeldingen op Azure Zie [Aan de slag met SQL Server in Azure virtuele Machines](http://go.microsoft.com/fwlink/p/?LinkId=294720) in de documentatie bij [SQL Server in Azure virtuele Machines](http://go.microsoft.com/fwlink/p/?LinkId=294719) ingesteld.

    ![Selecteer SQL Server VM][1]

4.  Geef op de eerste pagina van de **Configuratie van de virtuele Machine** de volgende informatie:

    -   Geef een **naam van de virtuele MACHINE**.
    -   Typ in het vak **Nieuwe naam** unieke gebruikersnaam voor de lokale beheerdersaccount VM.
    -   Typ in het vak **Nieuw wachtwoord** een sterk wachtwoord. Zie [Sterke wachtwoorden](http://msdn.microsoft.com/library/ms161962.aspx)voor meer informatie.
    -   Typ het wachtwoord nogmaals in het vak **Wachtwoord bevestigen** .
    -   Selecteer de juiste **grootte** in de vervolgkeuzelijst.

     > [AZURE.NOTE] De grootte van de virtuele machine wordt opgegeven tijdens het inrichten: A2 is de kleinste grootte aanbevolen voor de werkbelasting van de productie. De aanbevolen minimumgrootte voor een virtuele machine is A3 als u SQL Server Enterprise Edition. Selecteer A3 of hoger als u SQL Server Enterprise Edition. Selecteer A4 bij gebruik van SQL Server 2012 of 2014 onderneming-geoptimaliseerd voor transactionele belasting afbeeldingen.
A7 selecteren wanneer u SQL Server 2012 of 2014 onderneming-geoptimaliseerd voor Data Warehousing Workloads afbeeldingen. De geselecteerde grootte beperkt het aantal gegevensschijven die u kunt configureren. Zie voor de meest actuele gegevens over de grootte van de virtuele machine en het aantal gegevensschijven die u aan een virtuele machine koppelen kunt, [Virtual Machine formaten voor Azure](http://msdn.microsoft.com/library/azure/dn197896.aspx). Zie [Virtuele Macines prijzen](https://azure.microsoft.com/pricing/details/virtual-machines/)voor prijsinformatie.

    Klik op de pijl volgende gemarkeerd in de rechterbenedenhoek om door te gaan.

    ![VM-configuratie][2]

5.  Op de tweede pagina van de **virtuele machine configuratie** bronnen voor networking, storage en beschikbaarheid te configureren:

    -   Kies in de **Cloud Service** **maken een nieuwe wolk service**.
    -   In het vak **De naam van de DNS-Service Cloud** leveren het eerste gedeelte van een DNS-naam van uw keuze, zodat een naam in de notatie **TESTNAME.cloudapp.net** is voltooid
    -   Selecteer in het **Gebied AFFINITEIT/groep/VIRTUEEL netwerk** een regio waar u deze virtuele afbeelding wordt gehost.
    -   Selecteer een bestaande account voor opslag in de **Opslag-Account**of een automatisch gegenereerde selecteren.
    -   Selecteer in het vak **Beschikbaar** **(geen)**.
    -   Lees en accepteer de prijsinformatie.

6.  In de **ENDPOINTS** , sectie, klikt u in de vervolgkeuzelijst leeg onder **de naam**en selecteer **MSSQL** en typ vervolgens het poortnummer van het exemplaar van de Database-Engine (voor het standaardexemplaar**1433** ).

7.  De SQL Server-VM kan ook fungeren als een IPython-laptop-Server die wordt geconfigureerd in een latere stap.
    Voeg een nieuw eindpunt om de poort te gebruiken voor uw laptop IPython-server opgeven. Voer een naam in de kolom **naam** , selecteert u een poortnummer van uw keuze voor de openbare poort en 9999 voor de particuliere poort.

    Klik op de pijl volgende gemarkeerd in de rechterbenedenhoek om door te gaan.

    ![Selecteer de poorten MSSQL en IPython][3]

8.  **Agent VM installeren** de optie gecontroleerd en klik op accepteren de het vinkje in de rechterbenedenhoek van de wizard voor het voltooien van het aanvraagproces VM.

    `![Laatste VM-opties][4]

9.  Wacht terwijl de Azure wordt voorbereid voor uw virtuele machine. Ga door met de status van de virtuele machine wordt verwacht:

    -   Starten (inrichting)
    -   Gestopt
    -   Starten (inrichting)
    -   Actief (inrichting)
    -   Wordt uitgevoerd


##<a name="RemoteDesktop"></a>Open de virtuele machine met behulp van extern bureaublad en volledige installatie

1.  Bij het inrichten is voltooid, klik op de naam van de virtuele machine te gaan naar de pagina van het DASHBOARD. Onderaan op de pagina, klikt u op **verbinden**.

2.  Kies het rpd-bestand met het programma Extern bureaublad van Windows te openen (`%windir%\system32\mstsc.exe`).

3.  Geef het wachtwoord voor de lokale administrator-account die u hebt opgegeven in een eerdere stap in het dialoogvenster **Windows-beveiliging** .
    (Wordt u mogelijk gevraagd om te controleren of de referenties van de virtuele machine.)

4.  De eerste keer dat u zich aanmeldt bij deze virtuele machine, wilt verschillende processen voltooien, met inbegrip van de instellingen van het bureaublad, Windows-updates en voltooiing van de initiële configuratietaken (sysprep) van Windows. Nadat Windows sysprep is voltooid, voltooit de installatie van SQL Server configureren. Deze taken mogelijk een vertraging van enkele minuten terwijl ze voltooien. `SELECT @@SERVERNAME`kan de juiste naam pas weer nadat SQL Server setup is voltooid, en SQL Server Management Studio niet visable op de startpagina.

Zodra u met de virtuele machine met Windows Extern bureaublad verbonden bent, wordt de virtuele machine werkt net zoals een andere computer. Verbinding maken met het standaardexemplaar van SQL Server met SQL Server Management Studio (waarop de virtuele machine) op de normale manier.


##<a name="InstallIPython"></a>IPython-laptops en andere ondersteunende hulpprogramma's installeren

Configureren van de nieuwe SQL Server-VM te fungeren als een laptop IPython-server extra ondersteunende hulpmiddelen installeren die AzCopy, Azure Opslagverkenner, nuttige gegevens wetenschap Python pakketten en anderen is een bijzondere aanpassing script verleend. Installeren:

- Klik met de rechtermuisknop op het pictogram **Start van Windows** en klik op **MS-DOS-Prompt (Admin)**
- De volgende opdrachten kopiëren en plakken vanaf een opdrachtprompt.

        set script='https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/MachineSetup/Azure_VM_Setup_Windows.ps1'
        @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString(%script%))"

- Wanneer daarom wordt gevraagd, voert u een wachtwoord van uw keuze voor de server IPython-laptop.
- Het script aanpassen automatiseert verschillende procedures voor na de installatie, waaronder:
    + Installatie en configuratie van de laptop IPython
    + TCP-poorten openen in Windows firewall voor de eindpunten eerder hebt gemaakt:
    + Voor de externe SQL Server-verbindingen
    + Voor externe serverconnectiviteit IPython-laptop
    + Bezig met ophalen van monster IPython-laptops en SQL-scripts
    + Downloaden en installeren van nuttige gegevens wetenschap Python pakketten
    + Downloaden en installeren van Azure hulpmiddelen zoals AzCopy en Azure Opslagverkenner  
<br>
- Externe toegang en laptop IPython uitvoeren vanaf een lokale of externe browser met behulp van een URL van het formulier `https://<virtual_machine_DNS_name>:<port>`, waarbij de IPython openbare poort die u hebt geselecteerd tijdens het inrichten van de virtuele machine.
- IPython-laptop-server wordt uitgevoerd als een achtergrondservice en automatisch worden gestart wanneer u de virtuele machine opnieuw.

##<a name="Optional"></a>Gegevensschijf toegevoegd als nodig is

Als uw afbeelding VM geen gegevensschijven, dat wil zeggen schijven dan station C (schijf OS) en station D (tijdelijke schijf), moet u het toevoegen van een of meer gegevensschijven opslaan van gegevens. De afbeelding VM voor SQL Server 2012 SP2 Enterprise geoptimaliseerd voor DataWarehousing werklasten komt vooraf geconfigureerd met extra schijven voor SQL Server gegevens- en logboekbestanden.

 > [AZURE.NOTE] Gebruik niet de D-schijf voor het opslaan van gegevens. Zoals de naam al aangeeft, biedt alleen tijdelijke opslag. Het biedt geen redundantie of back-up omdat deze zich niet in Azure opslag.

Om aanvullende gegevensschijven koppelen, de stappen die worden beschreven in [het toevoegen van een schijf met gegevens naar een virtuele Windows-computer](virtual-machines-windows-classic-attach-disk.md), die u begeleidt:

1. Lege schijven koppelen aan de virtuele machine ingericht in eerdere stappen
2. Initialisatie van de nieuwe schijf of schijven in de virtuele machine


##<a name="SSMS"></a>Verbinding maken met SQL Server Management Studio en de verificatiemodus mixed inschakelen

De SQL Server-Database-Engine kan geen Windows-verificatie gebruiken zonder domeinomgeving. Voor verbinding met de Database-Engine van een andere computer, moet u SQL Server configureren voor de verificatiemodus mixed. De verificatiemodus Mixed kunt zowel SQL Server-verificatie en Windows-verificatie. SQL-verificatiemodus is vereist voor het nemen van de gegevens rechtstreeks vanuit uw databases SQL Server VM in [Azure Machine Learning Studio](https://studio.azureml.net) met behulp van de module gegevens importeren.

1.  Verbinding gemaakt met de virtuele machine met behulp van extern bureaublad en het deelvenster **Zoeken** van Windows gebruiken en typt u **SQL Server Management Studio** (SMSS). Hiermee start u SQL Server Management Studio (SSMS). U kunt een snelkoppeling toevoegen aan SSMS op het bureaublad voor toekomstig gebruik.

    ![SSMS starten][5]

    De eerste keer u Management Studio opent moet het Management Studio-omgeving voor gebruikers maken. Dit kan enkele minuten duren.

2.  Bij het openen, presenteert Management Studio in het dialoogvenster **verbinding maken met Server** . Typ in het vak **servernaam** de naam van de virtuele machine verbinding maken met de Database-Engine met de Explorer-Object.
    (In plaats van de naam van de virtuele machine kunt ook u **(local)** of een punt als de **naam van de Server**. Selecteer **Windows-verificatie**en laat ** *uw\_VM\_naam*\\uw\_lokale\_administrator** in het vak **gebruikersnaam** . Klik op **verbinden**.

    ![Verbinding maken met Server][6]

    <br>

     > [AZURE.TIP] U kunt de SQL Server-verificatiemodus met een wijziging van de registersleutel Windows of SQL Server Management Studio. Start een **Nieuwe Query** wilt wijzigen met behulp van de wijziging van de registersleutel verificatiemodus, en het volgende script uitvoeren:

        USE master
        go

        EXEC xp_instance_regwrite N'HKEY_LOCAL_MACHINE', N'Software\Microsoft\MSSQLServer\MSSQLServer', N'LoginMode', REG_DWORD, 2
        go


    De verificatiemodus met behulp van SQL Server management Studio wijzigen:

3.  In **SQL Server Management Studio Object Explorer**met de rechtermuisknop op de naam van het exemplaar van SQL Server (de naam van de virtuele machine) en klik vervolgens op **Eigenschappen**.

    ![Eigenschappen van server][7]

4.  Op de pagina **beveiliging** onder **Server-verificatie**selecteert u **SQL Server en Windows-verificatiemodus**en klik op **OK**.

    ![Selecteer de verificatiemodus][8]

5.  Klik op **OK** om te bevestigen de vereiste SQL Server opnieuw te starten in het dialoogvenster **SQL Server Management Studio** .

6.  In **Object Explorer**met de rechtermuisknop op de server en klik vervolgens op **opnieuw starten**. (Als SQL Server Agent wordt uitgevoerd, er moet ook opnieuw worden gestart.)

    ![Opnieuw opstarten][9]

7.  Klik op **Ja** om stemt ermee in dat u wilt opstarten van SQL Server in het dialoogvenster **SQL Server Management Studio** .

##<a name="Logins"></a>SQL Server-verificatie-aanmeldingen maken

Als u verbinding maakt met de Database-Engine van een andere computer, moet u ten minste een aanmelding voor SQL Server-verificatie.  

U kunt via programmering nieuwe SQL Server-aanmeldingen maken of met SQL Server Management Studio. Start een **Nieuwe Query** om een nieuwe gebruiker voor sysadmin SQL-verificatie via programmering, en het volgende script uitvoeren. Vervang < gebruikersnaam\> en < nieuwe wachtwoord\> met uw keuze van *gebruikersnaam* en *wachtwoord*. 


    USE master
    go

    CREATE LOGIN <new user name> WITH PASSWORD = N'<new password>',
        CHECK_POLICY = OFF,
        CHECK_EXPIRATION = OFF;

    EXEC sp_addsrvrolemember @loginame = N'<new user name>', @rolename = N'sysadmin';


Pas het wachtwoordbeleid zo nodig (de steekproef code schakelt beleid controleren en het wachtwoord verlopen). Zie voor meer informatie over SQL Server-aanmeldingen [maken een aanmelding](http://msdn.microsoft.com/library/aa337562.aspx).  

Nieuwe SQL Server om aanmeldingen te maken met SQL Server Management Studio:

1.  Vouw in **SQL Server Management Studio Object Explorer**de map van het exemplaar van de server die u wilt maken van nieuwe aanmelding.

2.  Klik met de rechtermuisknop op de map **Security** , wijs **Nieuw**aan en selecteer **aanmelding...**.

    ![Nieuwe aanmelding][10]

3.  Voer de naam van de nieuwe gebruiker in het vak **aanmeldingsnaam** in op de pagina **Algemeen** van het dialoogvenster **aanmelden - nieuwe** .

4.  Selecteer **SQL Server-verificatie**.

5.  Voer in het vak **wachtwoord** een wachtwoord voor de nieuwe gebruiker. Typ dat wachtwoord nogmaals in het vak **Wachtwoord bevestigen** .

6.  Als u opties voor wachtwoord beleid voor complexiteit en handhaving, selecteert u **wachtwoordbeleid afdwingen** (aanbevolen). Dit is een standaardoptie wanneer SQL Server-verificatie is ingeschakeld.

7.  Als u opties voor verlopen wachtwoord, selecteer **vervallen van wachtwoorden forceren** (aanbevolen). Afdwingen wachtwoordbeleid moet worden geselecteerd om dit selectievakje inschakelen. Dit is een standaardoptie wanneer SQL Server-verificatie is ingeschakeld.

8.  Als u wilt dat de gebruiker na de eerste keer dat de aanmelding wordt gebruikt om een nieuw wachtwoord, selecteer **gebruiker moet wachtwoord bij volgende aanmelding wijzigen** (aanbevolen als deze aanmelding voor iemand anders is te gebruiken. Als de aanmelding voor eigen gebruik, schakel deze optie.) Afdwingen vervallen van wachtwoorden moet worden geselecteerd om dit selectievakje inschakelen. Dit is een standaardoptie wanneer SQL Server-verificatie is ingeschakeld.

9.  Selecteer in de lijst **standaarddatabase** een standaarddatabase voor de aanmelding. **master** is de standaardinstelling voor deze optie. Als u nog geen gebruiker-database hebt gemaakt, laat deze **master**.

10. Laat de **standaard** als de waarde in de lijst **taal** .

    ![Aanmeldingseigenschappen][11]

11. Als dit de eerste aanmelding die u maakt, kunt u deze aanmelding als een SQL Server-beheerder aanwijzen. Als dit het geval is, controleert u op de pagina **Serverrollen** **sysadmin**.

    > [AZURE.IMPORTANT] Leden van de vaste serverrol sysadmin hebben volledige controle van de Database-Engine. Om veiligheidsredenen moet u zorgvuldig lidmaatschap in deze rol te beperken.

    ![sysadmin][12]

12. Klik op OK.

##<a name="DNS"></a>Bepaal de DNS-naam van de virtuele machine

Als u verbinding maakt met de SQL Server-Database-Engine van een andere computer, moet u de naam van de virtuele machine van de Domain Name System (DNS) weten.

(Dit is de naam van die het internet wordt gebruikt voor het identificeren van de virtuele machine. U kunt het IP-adres, maar het IP-adres kan veranderen wanneer Azure bronnen voor redundantie of onderhoud beweegt. De DNS-naam worden stabiel omdat deze kan worden omgeleid naar een nieuw IP-adres.)

1.  Selecteer de **virtuele MACHINES**in de klassieke Azure-Portal (of uit de vorige stap).

2.  Zoeken op de pagina **Virtuele MACHINE exemplaren** in de kolom **Naam van DNS-** en de DNS-naam voor de virtuele machine wordt voorafgegaan door **http://**kopiëren. (De gebruikersinterface mogelijk niet de volledige naam weergegeven, maar u kunt met de rechtermuisknop op het en selecteer kopiëren.)

##<a name="cde"></a>Verbinding maken met de Database-Engine van een andere computer

1.  Open SQL Server Management Studio op een computer die verbinding heeft met internet.

2.  Voer in het dialoogvenster **verbinding maken met een Server** of **verbinding maken met de Database-Engine** in het vak **servernaam** de DNS-naam van de virtuele machine (bepaald in de vorige taak) en een eindpunt openbare poortnummer in de indeling van *DNS-naam, poortnummer* zoals **tutorialtestVM.cloudapp.net,57500**.

3.  Selecteer in het vak **verificatie** **SQL Server-verificatie**.

4.  Typ de naam van een aanmelding die u hebt gemaakt in een eerdere taak in het vak **aanmelden** .

5.  Typ in het vak **wachtwoord** het wachtwoord van de aanmelding die u in een eerdere taak maken.

6.  Klik op **verbinden**.

##<a name="amlconnect"></a>Verbinding maken met de Database-Engine van Azure Machine Learning

In latere stadia van de procedure voor het Team gegevens wetenschap gebruikt u de [Azure Machine Learning Studio](https://studio.azureml.net) kunnen bouwen en implementeren van machine learning modellen. Gegevens uit uw databases SQL Server VM in Azure Machine Learning training nemen of scoren, gebruikt u de module **Gegevens importeren** in een nieuwe [Azure Machine Learning Studio](https://studio.azureml.net) experiment. In dit onderwerp vindt u in meer details via de procedure voor het Team gegevens wetenschap handleiding koppelingen. Zie voor een inleiding, [Wat is Azure Machine Learning Studio?](machine-learning-what-is-ml-studio.md).

2.  Selecteer in het deelvenster **Eigenschappen** van de [module gegevens importeren](https://msdn.microsoft.com/library/azure/dn905997.aspx) **Azure SQL-Database** in de vervolgkeuzelijst voor de **Gegevensbron** .

3.  Voer in het tekstvak **naam van de databaseserver**`tcp:<DNS name of your virtual machine>,1433`

4.  Voer de SQL-naam in het tekstvak **naam van gebruikersaccount Server** .

5.  Het wachtwoord van de sql-gebruiker in het tekstvak **serverwachtwoord** invoeren.

    ![Gegevens importeren met Azure ML][13]

##<a name="shutdown"></a>Afsluiten en de toewijzing van de virtuele machine wanneer niet in gebruik

Azure virtuele Machines worden geprijsd als u **betaalt alleen voor wat u gebruikt**. Om ervoor te zorgen dat u bent niet in rekening wordt gebracht wanneer de virtuele machine niet wordt gebruikt, heeft deze de status **(Deallocated) gestopt** .

> [AZURE.NOTE] De virtuele machine wordt afgesloten van binnen (met energiebeheer van Windows), de VM is gestopt maar blijft verdeeld. U bent niet in rekening wordt gebracht, zodat virtuele machines van de [Klassieke Portal Azure](http://manage.windowsazure.com/)altijd te stoppen. U kunt ook de VM via Powershell stoppen door het aanroepen van ShutdownRoleOperation met 'PostShutdownAction' gelijk is aan 'StoppedDeallocated'.

Afsluiten en de toewijzing van de virtuele machine:

1. Log in op de [Azure klassieke Portal](http://manage.windowsazure.com/) met uw account.  

2. Selecteer de **virtuele MACHINES** in de linkernavigatiebalk.

3. Klik op de naam van de virtuele machine en Ga naar **de dashboardpagina** in de lijst van virtuele machines.

4. Onderaan op de pagina, klikt u op **Afsluiten**.

![VM afsluiten][15]

De virtuele machine worden vrijgegeven maar niet verwijderd. U kan uw virtuele machine opnieuw op elk gewenst moment uit de klassieke Azure-Portal.

## <a name="your-azure-sql-server-vm-is-ready-to-use-whats-next"></a>Uw VM Azure SQL Server is klaar voor gebruik: Wat is het volgende?

De virtuele machine is nu klaar voor gebruik in uw gegevens wetenschap oefeningen. De virtuele machine is ook klaar voor gebruik als een laptop IPython-server voor de exploratie en verwerking van gegevens en andere taken in combinatie met Azure Machine Learning en Team gegevens wetenschap proces (TDSP).

De volgende stappen in het proces van de wetenschappelijke gegevens in het [Team gegevens wetenschap proces](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) zijn toegewezen en kunnen bevatten stappen waarmee u gegevens naar een HDInsight, verwerken en het monster in de voorbereiding op het leren van de gegevens met de computer leren werken met Azure.


[1]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/selectsqlvmimg.png
[2]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/4vm-config.png
[3]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/sqlvmports.png
[4]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/vmpostopts.png
[5]:./media/machine-learning-data-science-setup-sql-server-virtual-machine/searchssms.png
[6]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/19connect-to-server.png
[7]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/20server-properties.png
[8]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/21mixed-mode.png
[9]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/22restart2.png
[10]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/23new-login.png
[11]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/24test-login.png
[12]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/25sysadmin.png
[13]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/amlreader.png
[15]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/vmshutdown.png
 
