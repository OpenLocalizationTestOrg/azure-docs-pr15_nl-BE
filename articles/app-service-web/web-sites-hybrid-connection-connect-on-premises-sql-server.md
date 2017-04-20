<properties
    pageTitle="Verbinding maken met SQL-Server voor ruimten vanuit een web app in Azure App-Service met behulp van hybride verbindingen"
    description="Maak een web app op Microsoft Azure en verbinden met een SQL Server-database op de gebouwen"
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/09/2016"
    ms.author="cephalin"/>

# <a name="connect-to-on-premises-sql-server-from-a-web-app-in-azure-app-service-using-hybrid-connections"></a>Verbinding maken met SQL-Server voor ruimten vanuit een web app in Azure App-Service met behulp van hybride verbindingen

Hybride verbindingen kunnen verbinden met [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) Web Apps bronnen voor ruimten met een statisch TCP-poort. Ondersteunde bronnen bevatten Microsoft SQL Server, MySQL, HTTP-Web-API's, App-Service en de meeste aangepaste Web Services.

In deze zelfstudie leert u hoe u een web App Service app maken in de [Portal Azure](http://go.microsoft.com/fwlink/?LinkId=529715), de web app verbinding met uw lokale op ruimten SQL Server-database met de nieuwe functie van hybride-verbinding, maakt een eenvoudige ASP.NET-toepassingen die gebruikmaken van de verbinding van hybride en implementatie van de toepassing op de App Service web app. De voltooide web app op Azure worden gebruikersreferenties opgeslagen in een lidmaatschap-database in gebouwen. De zelfstudie wordt ervan uitgegaan dat er geen eerdere ervaringen met Azure of ASP.NET.

>[AZURE.NOTE] Als u wilt aan de slag met Azure App-Service voordat u aanmelden voor een account met Azure, gaat u naar de [App-Service probeert](http://go.microsoft.com/fwlink/?LinkId=523751), waar u direct een starter tijdelijk web app in de App-Service maken kunt. Geen creditcard vereist; geen verplichtingen.
>
>Het Web Apps gedeelte van de functie hybride verbindingen is alleen beschikbaar in de [Portal Azure](https://portal.azure.com). Zie een verbinding wilt maken in de BizTalk-Services, [Hybride verbindingen](http://go.microsoft.com/fwlink/p/?LinkID=397274).  

## <a name="prerequisites"></a>Vereisten ##

Als u deze zelfstudie hebt voltooid, moet u de volgende producten. Allemaal beschikbaar in gratis versies, zodat u kunt ontwikkelen voor Azure volledig voor gratis.

- **Azure abonnement** - Zie [Azure gratis proefversie](/pricing/free-trial/)voor een gratis abonnement.

- **Visual Studio 2013** - download een gratis evaluatieversie van Visual Studio 2013, Zie [Visual Studio downloadt](http://www.visualstudio.com/downloads/download-visual-studio-vs). Dit installeren voordat u verdergaat.

- **Microsoft.NET Framework 3.5 servicepack 1** - als het besturingssysteem Windows 8.1, Windows Server 2012 R2, Windows 8, Windows Server 2012, Windows 7 of Windows Server 2008 R2 is, kunt u dit inschakelen in het Configuratiescherm > programma's en onderdelen > Windows-onderdelen in- of uitschakelen. Anders kunt u deze downloaden vanaf het [Microsoft Download Center](http://www.microsoft.com/download/en/details.aspx?displaylang=en&id=22).

- **SQL Server 2014 Express met extra** - Microsoft SQL Server Express op de [pagina Database met Microsoft Web Platform](http://www.microsoft.com/web/platform/database.aspx)gratis downloaden. Kies de versie **Express** (niet LocalDB). De versie **met extra Express** bevat SQL Server Management Studio die u in deze zelfstudie wilt gebruiken.

- **SQL Server Management Studio Express** - dit is opgenomen in SQL Server 2014 Express met hulpprogramma's downloaden van bovengenoemde, maar als u afzonderlijk te installeren, kunt u downloaden en installeren vanaf de [downloadpagina van SQL Server Express](http://www.microsoft.com/web/platform/database.aspx).

De zelfstudie wordt ervan uitgegaan dat u een Azure-abonnement, dat u Visual Studio 2013 hebt geïnstalleerd en u hebt geïnstalleerd of ingeschakeld .NET Framework 3.5. De zelfstudie wordt beschreven hoe u SQL Server Express 2014 installeren in een configuratie die werkt goed met de functie Azure hybride verbindingen (een exemplaar met een statisch TCP-poort samen). Voordat u de zelfstudie start, download SQL Server 2014 Express met hulpprogramma's van de locatie die hierboven wordt vermeld, hebt u geen SQL Server is geïnstalleerd.

### <a name="notes"></a>Notities ###
Voor het gebruik van een database van SQL Server of SQL Server Express op gebouwen met een hybride-verbinding, moet TCP/IP voor een statische poort worden ingeschakeld. Standaardexemplaren van SQL Server gebruik van statische poort 1433, dat niet het benoemde exemplaren.

De computer waarop u de agent op ruimten hybride Verbindingsbeheer installeert:

- Moet uitgaande verbindingen met Azure via:

Poort|Waarom
---|---
80|**Vereist** voor HTTP-poort om certificaten te valideren en eventueel voor gegevensconnectiviteit.
443|**Optioneel** voor gegevensconnectiviteit. Als uitgaande verbindingen met 443 niet beschikbaar is, wordt de TCP-poort 80 gebruikt.
5671 en 9352|**Aanbevolen** , maar optioneel voor gegevensconnectiviteit. Opmerking dat in deze modus levert doorgaans hogere doorvoer. Als u uitgaande verbindingen op deze poorten niet beschikbaar is, wordt TCP-poort 443 gebruikt.
- Kunnen bereiken de *hostnaam*:*poortnummer* van de bron op gebouwen.

De stappen in dit artikel wordt ervan uitgegaan dat u de browser van de computer die als host voor de agent op ruimten hybride verbinding fungeert gebruikt.

Hebt u al SQL Server is geïnstalleerd in een configuratie en in een omgeving die voldoet aan de bovenstaande voorwaarden, kunt u overslaan en beginnen met het [maken van een SQL Server-database op gebouwen](#CreateSQLDB).

<a name="InstallSQL"></a>
## <a name="a-install-sql-server-express-enable-tcpip-and-create-a-sql-server-database-on-premises"></a>A. SQL Server Express installeren, TCP/IP inschakelen en een SQL Server-database maken op locatie ##

In deze sectie wordt beschreven hoe u SQL Server Express installeren en een database maken, zodat uw webtoepassing met Azure Portal werkt TCP/IP inschakelen.

### <a name="install-sql-server-express"></a>SQL Server Express installeren ###

1. SQL Server Express installeert het bestand **SQLEXPRWT_x64_ENU.exe** of **SQLEXPR_x86_ENU.exe** die u hebt gedownload te voeren. De wizard SQL Server Installation Center wordt weergegeven.

    ![Installatie van SQL Server][SQLServerInstall]

2. Kies **zelfstandige nieuwe SQL Server-installatie of functies toevoegen aan een bestaande installatie van**. Volg de instructies en accepteert de standaardwaarden en instellingen, totdat u op de pagina **Configuratie van het exemplaar** .

3. Kies op de pagina **Configuratie van exemplaar** **standaard exemplaar**.

    ![Kies standaardexemplaar][ChooseDefaultInstance]

    Het standaardexemplaar van SQL Server luistert standaard voor aanvragen van clients van SQL Server op statische poort 1433, dat is wat de functie hybride verbindingen vereist. Benoemde instanties gebruiken dynamische poorten en UDP, worden niet ondersteund door hybride verbindingen.

4. Accepteer de standaardinstellingen op de pagina **Configuratie van de Server** .

5. Op de pagina **Configuratie van de Database-Engine** met de **Verificatiemodus**, kiest u **De gemengde modus (SQL Server-verificatie en Windows-verificatie)**en een wachtwoord opgeven.

    ![Kies in de gemengde modus][ChooseMixedMode]

    In deze zelfstudie wordt u SQL Server-verificatie. Zorg ervoor dat het onthouden van het wachtwoord dat u opgeeft, omdat u deze later moet.

6. Stap voor stap de rest van de wizard om de installatie te voltooien.

### <a name="enable-tcpip"></a>TCP/IP inschakelen ###
Als u wilt dat TCP/IP, gebruikt u SQL Server Configuration Manager, die werd geïnstalleerd tijdens de installatie van SQL Server Express. Volg de stappen in de [TCP/IP-netwerk-Protocol inschakelen voor SQL Server](http://technet.microsoft.com/library/hh231672%28v=sql.110%29.aspx) voordat u verdergaat.

<a name="CreateSQLDB"></a>
### <a name="create-a-sql-server-database-on-premises"></a>Een SQL Server-database maken op locatie ###

Uw webtoepassing Visual Studio vereist een lidmaatschap-database die kan worden benaderd door Azure. Dit vereist een database SQL Server of SQL Server Express (niet de LocalDB database die de sjabloon MVC standaard gebruikt), zodat u de database lidmaatschap volgende zult maken.

1. In SQL Server Management Studio verbinding met de SQL-Server die u zojuist hebt geïnstalleerd. (Als de **Server verbinding maken met** dialoogvenster niet automatisch wordt weergegeven in het linker deelvenster **Object** Explorer navigeert, klikt u op **verbinden**en klik vervolgens op **Database-Engine**.)  ![Verbinding maken met Server][SSMSConnectToServer]

    Kies voor **type Server**, **Database-Engine**. Voor de **naam van de Server**, kunt u **localhost** of de naam van de computer die u gebruikt. **SQL Server-verificatie**kiest en meld u vervolgens met de sa-gebruikersnaam en het wachtwoord dat u eerder hebt gemaakt.

2. Een nieuwe database maken met behulp van SQL Server Management Studio, **Databases** in Object Explorer met de rechtermuisknop en klik vervolgens op **Nieuwe Database**.

    ![Nieuwe database maken][SSMScreateNewDB]

3. MembershipDB voor de naam van de database invoeren in het dialoogvenster **Nieuwe Database** en klik vervolgens op **OK**.

    ![Naam van de database][SSMSprovideDBname]

    Houd er rekening mee dat u niet naar de database op dit punt wijzigen doet. De gegevens over het groepslidmaatschap zullen later worden toegevoegd automatisch door uw webtoepassing tijdens de uitvoering.

4. In Object Explorer als u **Databases**, ziet u dat de database lidmaatschap is gemaakt.

    ![MembershipDB gemaakt][SSMSMembershipDBCreated]

<a name="CreateSite"></a>
## <a name="b-create-a-web-app-in-the-azure-portal"></a>B. Een webtoepassing maken in de Portal Azure ##

> [AZURE.NOTE] U kunt als u al een web app in de Azure-Portal die u wilt gebruiken voor deze zelfstudie hebt gemaakt, gaat u verder met [een hybride-verbinding en een BizTalk-Service maken](#CreateHC) en van daaruit verder.

1. Klik op **Nieuw**in [Azure Portal](https://portal.azure.com) > **Web + Mobile** > **Web app**.

    ![Knop Nieuw][New]

2. Uw web app configureren en klik vervolgens op **maken**.

    ![Naam van website][WebsiteCreationBlade]

3. Na een paar seconden de web app gemaakt en wordt de web app blade weergegeven. Het blad is een verticaal overzicht dashboard waarmee u uw web app beheren.

    ![Website met][WebSiteRunningBlade]

    Als u wilt controleren of dat de web app is live, kunt u het pictogram **Bladeren** om de standaardpagina weer te geven.

Vervolgens maakt u een hybride-verbinding als een BizTalk-service voor het web app.

<a name="CreateHC"></a>
## <a name="c-create-a-hybrid-connection-and-a-biztalk-service"></a>C. Een hybride verbinding en een BizTalk-Service maken ##

1. Terug in de Portal, Ga naar instellingen en klik op **netwerk** > **uw verbinding hybride eindpunten configureren**.

    ![Hybride verbindingen][CreateHCHCIcon]

2. Klik op **toevoegen**op het blad hybride verbindingen, > **nieuwe hybride verbinding**.

3. Op het blad **hybride-verbinding maken** :
    - Voor **naam**, Geef een naam voor de verbinding.
    - Voer voor de **hostnaam**, de naam van uw SQL Server host-computer.
    - Voer voor de **poort**, 1433 (de standaardpoort voor SQL Server).
    - Klik op de **BizTalk-Service** > **Nieuwe BizTalk-Service** en voer een naam voor de BizTalk-service.

    ![Een hybride verbinding maken][TwinCreateHCBlades]

5. Klik tweemaal op **OK** .

    Wanneer het proces is voltooid, de **meldingen** gebied wordt een groene **SUCCES** flash en de **hybride verbinding** blade wordt de nieuwe hybride verbinding met de status **niet**verbonden weergegeven.

    ![Een hybride verbinding gemaakt][CreateHCOneConnectionCreated]

U hebt nu een belangrijk onderdeel van de infrastructuur voor cloud hybride verbinding voltooid. Vervolgens maakt u een bijbehorende op ruimten stuk.

<a name="InstallHCM"></a>
## <a name="d-install-the-on-premises-hybrid-connection-manager-to-complete-the-connection"></a>D. Installeren van lokalen hybride Verbindingsbeheer om de verbinding te voltooien ##

[AZURE.INCLUDE [app-service-hybrid-connections-manager-install](../../includes/app-service-hybrid-connections-manager-install.md)]

Nu dat de infrastructuur hybride verbinding voltooid is, maakt u een webtoepassing die wordt gebruikt.

<a name="CreateASPNET"></a>
## <a name="e-create-a-basic-aspnet-web-project-edit-the-database-connection-string-and-run-the-project-locally"></a>E. Een standaard ASP.NET-webproject maken, de verbindingsreeks voor de database en het project lokaal uitvoeren ##

### <a name="create-a-basic-aspnet-project"></a>Een standaard ASP.NET-project maken ###
1. Maak een nieuw Project in het menu **bestand** in Visual Studio:

    ![Nieuwe Visual Studio-project][HCVSNewProject]

2. **In de sectie **sjablonen** in het dialoogvenster **Nieuw Project** **Web** selecteren en kiest u **ASP.NET-webtoepassingen**.**

    ![Kies de ASP.NET-webtoepassing][HCVSChooseASPNET]

3. In het dialoogvenster **Nieuw Project voor ASP.NET** **MVC**kiezen en klik vervolgens op **OK**.

    ![MVC kiezen][HCVSChooseMVC]

4. Wanneer het project is gemaakt, de Leesmij-pagina wordt weergegeven. Het webproject niet nog uitgevoerd.

    ![Leesmij-pagina][HCVSReadmePage]

### <a name="edit-the-database-connection-string-for-the-application"></a>De verbindingsreeks voor de database voor de toepassing ###

In deze stap geeft u de verbindingsreeks die de toepassing aangeeft waar vindt u de lokale database van SQL Server Express. De verbindingsreeks is in de toepassing van Web.config-bestand configuratiegegevens voor de toepassing bevat.

> [AZURE.NOTE] Om ervoor te zorgen dat uw toepassing gebruikmaakt van de database die u hebt gemaakt in SQL Server Express en niet de in de Visual Studio-standaard LocalDB, is het belangrijk dat u deze stap uitvoert voordat u uw project.

1. Dubbelklik op het bestand Web.config in de Solution Explorer.

    ![Web.config][HCVSChooseWebConfig]

2. Bewerken in de sectie **connectionStrings** om te verwijzen naar de SQL Server-database op de lokale computer, volgt de syntaxis in het volgende voorbeeld:

    ![Verbindingsreeks][HCVSConnectionString]

    Houd bij het opstellen van de verbindingsreeks, rekening met het volgende:

    - Als u verbinding maakt met een benoemde in plaats van een standaardexemplaar (bijvoorbeeld YourServer\SQLEXPRESS), moet u de SQL Server om statische poorten te gebruiken. Zie voor meer informatie over het configureren van statische poorten [configureren van SQL Server op een specifieke poort luistert](http://support.microsoft.com/kb/823938). Benoemde instanties gebruiken UDP en dynamische poorten die niet worden ondersteund door hybride verbindingen.

    - Het wordt aanbevolen dat u de poort opgeven (1433 standaard weergegeven in het voorbeeld) in de verbindingsreeks zodat u kunt er zeker van dat SQL Server TCP ingeschakeld is en de juiste poort wordt gebruikt.

    - Moet u SQL Server-verificatie gebruiken om verbinding te maken de gebruikers-ID en het wachtwoord opgeven in de verbindingsreeks.

3. Klik op **Opslaan** in Visual Studio het Web.config-bestand wilt opslaan.

### <a name="run-the-project-locally-and-register-a-new-user"></a>Het project lokaal worden uitgevoerd en een nieuwe gebruiker registreren ###

1. Nu het nieuwe webproject lokaal uitvoeren door te klikken op de bladerknop in Foutopsporing. In dit voorbeeld gebruikt Internet Explorer.

    ![Project uitvoeren][HCVSRunProject]

2. Kies in de rechterbovenhoek van de standaardwebpagina, **Registreer** een nieuwe account registreren:

    ![Registreer een nieuwe account][HCVSRegisterLocally]

3. Voer een gebruikersnaam en wachtwoord in:

    ![Voer de gebruikersnaam en wachtwoord][HCVSCreateNewAccount]

    Dit maakt automatisch een database op uw lokale SQL Server die in het bezit van de gegevens over het groepslidmaatschap voor uw toepassing. Een van de tabellen (**dbo. AspNetUsers**) bevat web app gebruikersreferenties zoals de knoppen die u zojuist hebt ingevoerd. In deze tabel ziet u verderop in de handleiding.

4. Sluit het browservenster van de standaardwebpagina. Hierdoor wordt voorkomen dat de toepassing in Visual Studio.

U bent nu gereed voor de volgende stap is de Azure-toepassing publiceren en testen.

<a name="PubNTest"></a>
## <a name="f-publish-the-web-application-to-azure-and-test-it"></a>F. De webtoepassing Azure publiceren en testen ##

Nu u de toepassing uitgeeft aan uw web app in App Service en test deze om te zien hoe de hybride-verbinding die u eerder hebt geconfigureerd voor uw web app verbinding met de database op uw lokale computer wordt gebruikt.

### <a name="publish-the-web-application"></a>De webtoepassing publiceren ###

1. U kunt uw profiel publishing voor het web App Service app in Azure Portal downloaden. Klik op **Get profiel publiceren**op het blad voor uw web app, en sla het bestand op uw computer.

    ![Download profiel publiceren][PortalDownloadPublishProfile]

    Vervolgens zult u dit bestand importeren in uw webtoepassing Visual Studio.

2. Klik met de rechtermuisknop op de naam van het project in de Solution Explorer en selecteer **publiceren**in Visual Studio.

    ![Selecteer publiceren][HCVSRightClickProjectSelectPublish]

3. Kies in het dialoogvenster **Web publiceren** , klikt u op het tabblad **profiel** **importeren**.

    ![Importeren][HCVSPublishWebDialogImport]

4. Ga naar het profiel voor uw gedownloade publiceren, selecteert u het en klik op **OK**.

    ![Ga naar profiel][HCVSBrowseToImportPubProfile]

5. Uw gegevens worden gepubliceerd wordt geïmporteerd en weergegeven op het tabblad **verbinding** van het dialoogvenster.

    ![Klik op publiceren][HCVSClickPublish]

    Klik op **publiceren**.

    Bij het publiceren is voltooid, wordt uw browser starten en de inmiddels bekende ASP.NET-toepassing--weergeven, met dien verstande dat nu live in de cloud Azure is!

Vervolgens gebruikt u uw live webtoepassing voor een overzicht van de hybride-verbinding in actie.

### <a name="test-the-completed-web-application-on-azure"></a>De voltooide webtoepassing op Azure testen ###

1. Op de bovenste rechts van de webpagina op Azure, kiest u **aanmelden**.

    ![Log in testen][HCTestLogIn]

2. Uw web App Service app is nu verbonden met uw webtoepassing lidmaatschap database op uw lokale computer. Meld u aan om dit te controleren, met dezelfde referenties die u hebt ingevoerd in de lokale database eerder.

    ![Hallo begroeting][HCTestHelloContoso]

3. Meld u af bij uw webtoepassing Azure verder de nieuwe hybride om verbinding te testen, en als een andere gebruiker registreren. Een nieuwe gebruikersnaam en wachtwoord en klik vervolgens op **registreren**.

    ![Test een andere gebruiker registreren][HCTestRegisterRelecloud]

4. Open SQL Management Studio op de lokale computer om na te gaan dat de referenties van de nieuwe gebruiker zijn opgeslagen in de lokale database via de hybride-verbinding. Vouw de database **MembershipDB** uit en vouw vervolgens de **tabellen**in Object Explorer. Met de rechtermuisknop op de **dbo. AspNetUsers** lidmaatschap tabel en kies **rijen boven 1000 selecteren** om de resultaten te bekijken.

    ![De resultaten bekijken][HCTestSSMSTree]

5. Het lidmaatschap van lokale tabel wordt nu beide accounts - dat u lokaal hebt gemaakt en die u hebt gemaakt in de Azure cloud. Die u hebt gemaakt in de cloud is opgeslagen in de database op de gebouwen via Azure van hybride verbinding functie.

    ![Geregistreerde gebruikers in de database voor gebouwen][HCTestShowMemberDb]

U hebt nu gemaakt en geïmplementeerd, een ASP.NET-webtoepassing die gebruikmaakt van een hybride verbinding tussen een web app in de Azure cloud en een SQL Server-database op gebouwen. Gefeliciteerd!

## <a name="see-also"></a>Zie ook ##
[Hybride verbindingen-overzicht](http://go.microsoft.com/fwlink/p/?LinkID=397274)

[Josh Twist introduceert hybride verbindingen (video Channel 9)](http://channel9.msdn.com/Shows/Azure-Friday/Josh-Twist-introduces-hybrid-connections)

[Hybride verbindingen-overzicht](/services/biztalk-services/)

[BizTalk-Services: Dashboard, Monitor, schaal, configureren en verbinding Hybrid tabbladen](../biztalk-services/biztalk-dashboard-monitor-scale-tabs.md)

[Bouwen van een Real-World hybride Cloud met naadloze overdraagbaarheid van toepassing (video Channel 9)](http://channel9.msdn.com/events/TechEd/NorthAmerica/2014/DCIM-B323#fbid=)

[Toegang op ruimten resources hybride verbindingen met Azure App Service](../app-service-web/web-sites-hybrid-connection-get-started.md)

[Overzicht van ASP.NET-identiteit](http://www.asp.net/identity)

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]


<!-- IMAGES -->
[SQLServerInstall]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A01SQLServerInstall.png
[ChooseDefaultInstance]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A02ChooseDefaultInstance.png
[ChooseMixedMode]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A03ChooseMixedMode.png
[SSMSConnectToServer]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A04SSMSConnectToServer.png
[SSMScreateNewDB]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A05SSMScreateNewDBlh.png
[SSMSprovideDBname]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A06SSMSprovideDBname.png
[SSMSMembershipDBCreated]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A07SSMSMembershipDBCreated.png
[New]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/B01New.png
[NewWebsite]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/B02NewWebsite.png
[WebsiteCreationBlade]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/B03WebsiteCreationBlade.png
[WebSiteRunningBlade]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/B04WebSiteRunningBlade.png
[Browse]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/B05Browse.png
[DefaultWebSitePage]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/B06DefaultWebSitePage.png
[CreateHCHCIcon]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C01CreateHCHCIcon.png
[CreateHCAddHC]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C02CreateHCAddHC.png
[TwinCreateHCBlades]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C03TwinCreateHCBlades.png
[CreateHCCreateBTS]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C04CreateHCCreateBTS.png
[CreateBTScomplete]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C05CreateBTScomplete.png
[CreateHCSuccessNotification]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C06CreateHCSuccessNotification.png
[CreateHCOneConnectionCreated]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C07CreateHCOneConnectionCreated.png
[HCIcon]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D01HCIcon.png
[NotConnected]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D02NotConnected.png
[NotConnectedBlade]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D03NotConnectedBlade.png
[ClickListenerSetup]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D04ClickListenerSetup.png
[ClickToInstallHCM]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D05ClickToInstallHCM.png
[ApplicationRunWarning]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D06ApplicationRunWarning.png
[UAC]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D07UAC.png
[HCMInstalling]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D08HCMInstalling.png
[HCMInstallComplete]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D09HCMInstallComplete.png
[HCStatusConnected]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D10HCStatusConnected.png
[HCVSNewProject]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E01HCVSNewProject.png
[HCVSChooseASPNET]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E02HCVSChooseASPNET.png
[HCVSChooseMVC]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E03HCVSChooseMVC.png
[HCVSReadmePage]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E04HCVSReadmePage.png
[HCVSChooseWebConfig]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E05HCVSChooseWebConfig.png
[HCVSConnectionString]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E06HCVSConnectionString.png
[HCVSRunProject]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E06HCVSRunProject.png
[HCVSRegisterLocally]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E07HCVSRegisterLocally.png
[HCVSCreateNewAccount]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E08HCVSCreateNewAccount.png
[PortalDownloadPublishProfile]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F01PortalDownloadPublishProfile.png
[HCVSPublishProfileInDownloadsFolder]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F02HCVSPublishProfileInDownloadsFolder.png
[HCVSRightClickProjectSelectPublish]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F03HCVSRightClickProjectSelectPublish.png
[HCVSPublishWebDialogImport]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F04HCVSPublishWebDialogImport.png
[HCVSBrowseToImportPubProfile]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F05HCVSBrowseToImportPubProfile.png
[HCVSClickPublish]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F06HCVSClickPublish.png
[HCTestLogIn]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F07HCTestLogIn.png
[HCTestHelloContoso]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F08HCTestHelloContoso.png
[HCTestRegisterRelecloud]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F09HCTestRegisterRelecloud.png
[HCTestSSMSTree]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F10HCTestSSMSTree.png
[HCTestShowMemberDb]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F11HCTestShowMemberDb.png
