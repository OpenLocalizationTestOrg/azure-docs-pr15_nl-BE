<properties
   pageTitle="Het migreren en publiceren van webtoepassingen naar een Azure Cloud-Service vanuit Visual Studio | Microsoft Azure"
   description="Informatie over het migreren en publiceren van uw webtoepassing een Azure cloud-service met behulp van Visual Studio."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="how-to-migrate-and-publish-a-web-application-to-an-azure-cloud-service-from-visual-studio"></a>Procedure: migreren en publiceren van webtoepassingen naar een Azure Cloud-Service vanuit Visual Studio

U kunt als u wilt profiteren van de hosting-services en de schaalbaarheid van Azure, migreren en publiceren van uw webtoepassing een Azure cloud-service. U kunt een webtoepassing in Azure met minimale wijzigingen aan uw bestaande toepassing uitvoeren.

>[AZURE.NOTE] Dit onderwerp is over het implementeren van cloud-diensten, niet naar websites. Zie voor meer informatie over het implementeren van websites [implementeren een web app in Azure App-Service](./app-service-web/web-sites-deploy.md).

Zie de sectie **Ondersteunde projectsjablonen** verderop in dit onderwerp voor een lijst van specifieke sjablonen die worden ondersteund voor zowel Visual C# en Visual Basic.

Uw webtoepassing voor Azure van Visual Studio moet u eerst inschakelen. De volgende afbeelding ziet u de belangrijkste stappen voor het publiceren van uw webtoepassing door een Azure project wilt gebruiken voor de implementatie toe te voegen. Dit proces wordt een Azure project met de functie vereiste web toegevoegd aan uw oplossing. Op basis van het type webproject die u hebt, worden de Projecteigenschappen voor assembly's ook bijgewerkt als het servicepakket extra assembly's voor implementatie vereist.

![Publiceren van webtoepassingen naar Microsoft Azure](./media/vs-azure-tools-migrate-publish-web-app-to-cloud-service/IC748917.png)

>[AZURE.NOTE] Het **converteren**, **omzetten in Azure Cloud Service Project** opdracht wordt alleen weergegeven voor het webproject in uw oplossing. Bijvoorbeeld, is de opdracht niet beschikbaar voor een Silverlight-project in de oplossing.
Wanneer u een servicepakket maken of uw toepassing aan Azure uitgeeft, optreden waarschuwingen of fouten. Deze waarschuwingen en fouten kunt u problemen oplossen voordat u kunt op Azure implementeren. U ontvangt mogelijk een waarschuwing over een ontbrekende assembly. Zie voor meer informatie over het behandelen van eventuele waarschuwingen als fouten [configureren een Azure Cloud Service-Project met Visual Studio](vs-azure-tools-configuring-an-azure-project.md). Als u uw toepassing maken, lokaal met behulp van de compute-emulator worden uitgevoerd of naar Azure publiceren, ziet u mogelijk de volgende fout in het venster **Lijst met fout** : **het opgegeven pad, de bestandsnaam, of beide zijn te lang**. Deze fout treedt op omdat de lengte van de volledig gekwalificeerde Azure naam te lang is. De lengte van de naam van het project, inclusief het volledige pad mag niet meer dan 146 tekens. Dit is bijvoorbeeld de naam van het volledige project met inbegrip van het pad voor een Azure-project dat is gemaakt voor een Silverlight-toepassing: `c:\users\<user name>\documents\visual studio 2015\Projects\SilverlightApplication4\SilverlightApplication4.Web.Azure.ccproj`. Mogelijk moet uw oplossing te verplaatsen naar een andere map met een korter pad ter vermindering van de lengte van de volledig gekwalificeerde naam.

Ga als volgt te werk om te migreren en te publiceren van webtoepassingen naar Azure vanuit Visual Studio.

## <a name="enable-a-web-application-for-deployment-to-azure"></a>Het inschakelen van een webtoepassing voor Azure-implementatie

### <a name="to-enable-a-web-application-for-deployment-to-azure"></a>Een webtoepassing voor implementatie van Azure inschakelen

1. Als u wilt dat uw webtoepassing voor implementatie in Azure, opent u het snelmenu voor een webproject in de oplossing en kies Azure implementatieproject toevoegen.

    De volgende acties uitgevoerd:

    - Een project Azure genoemd `<name of the web project>.Azure` wordt toegevoegd aan de oplossing voor uw toepassing.

    - Een Webrol voor het webproject wordt toegevoegd aan dit project Azure.

    - De **Lokale kopie** van de eigenschap is ingesteld op true voor alle assembly's die vereist voor MVC MVC 2, MVC 3 zijn, 4 en Silverlight zakelijke toepassingen. Hiermee voegt u deze assembly's toe aan het servicepakket dat wordt gebruikt voor de implementatie.

  >[AZURE.IMPORTANT] Als u andere assembly's of bestanden die nodig voor deze webtoepassing zijn hebt, moet u de eigenschappen voor deze bestanden handmatig instellen. Zie de sectie **Bestanden opnemen in het pakket Service** verderop in dit artikel voor meer informatie over het instellen van deze eigenschappen.

  >[AZURE.NOTE] Als u een web-functie voor een bepaald webproject bestaat al in Azure-project in de oplossing, **converteren**, wordt **omzetten in Azure Cloud Service Project** niet weergegeven in het snelmenu voor dit webproject.

  Als u meerdere webprojecten in uw webtoepassing hebt en u wilt maken voor elk webproject web-rollen, voert u de stappen in deze procedure voor elke webproject. Hiermee maakt u afzonderlijke Azure projecten voor elke Webrol. Elk webproject kan afzonderlijk worden gepubliceerd. Ook kunt u handmatig toevoegen een andere web-functie aan een bestaand project Azure in uw webtoepassing. Hiertoe opent u het snelmenu voor de map **rollen** in het project Azure, kies **toevoegen**en vervolgens de **Functie webproject in oplossing**, kies het project toe te voegen als de rol van een webpagina en klik op **OK** .

## <a name="use-an-azure-sql-database-for-your-application"></a>Azure SQL-Database gebruiken voor uw toepassing

Als u een verbindingsreeks voor uw webtoepassing die gebruikmaakt van een SQL Server-database die zich in het pand, moet u deze verbindingsreeks gebruiken een instantie van SQL-Database die Azure hosts in plaats daarvan.

>[AZURE.IMPORTANT] Uw abonnement moet kunt u SQL-Database. Als u uw abonnement via de [Azure klassieke portal](http://go.microsoft.com/fwlink/?LinkID=213885), kunt u bepalen welke services uw abonnement beschikt. De volgende instructies gelden voor de vrijgegeven [Azure klassieke portal](http://go.microsoft.com/fwlink/?LinkID=213885). Als u de [Azure portal](http://portal.microsoft.com)gebruikt, gaat u verder met de volgende procedure.

### <a name="to-use-a-sql-database-instance-in-your-web-role-for-your-connection-string"></a>Een SQL-Database-instantie gebruiken in uw web-functie voor de verbindingsreeks

1. Een instantie van SQL-Database maken in [Azure klassieke portal](http://go.microsoft.com/fwlink/?LinkID=213885), volg de stappen in het volgende artikel: [maken van een SQL-databaseserver](http://go.microsoft.com/fwlink/?LinkId=225109).

    >[AZURE.NOTE] Bij het instellen van de firewall-regels voor uw exemplaar van de SQL-Database, moet u het selectievakje **toestaan dat andere Azure services toegang tot deze server** .

1. U maakt een exemplaar van de SQL-Database moet worden gebruikt voor de verbindingsreeks, volg de stappen in de volgende sectie in het volgende artikel: [een SQL-Database maken](http://go.microsoft.com/fwlink/?LinkId=225110).

1. Als u wilt kopiëren in de ADO.NET-verbindingsreeks voor de verbindingsreeks, moet u de volgende stappen uitvoeren in [Azure klassieke portal](http://go.microsoft.com/fwlink/?LinkID=213885).  

  1. Klik op de knop van de **Database** en open vervolgens het knooppunt voor het abonnement dat u gebruikt voor het maken van uw exemplaar van de SQL-Database.

  1. Kies het knooppunt **SQL-Databases** zodat de beschikbare exemplaren van SQL-Database.

  1. Kies de eigenschappen weergegeven van de database, de database. De weergave **Eigenschappen** wordt weergegeven.

      >[AZURE.NOTE] Als de weergave **Eigenschappen** niet wordt weergegeven, is het mogelijk om deze te openen met behulp van de scheidslijn.

  1. Kies de knop met het weglatingsteken (...) naast beeld zodat de verbindingsreeksen.

    Het dialoogvenster **Verbindingsreeks** wordt weergegeven.

  1. Als u wilt kopiëren in de verbindingsreeks ADO.NET, markeer de tekst en kiest u de toetsen Ctrl + C.

  1. Kies de knop **sluiten** om het dialoogvenster te sluiten.

1. Ter vervanging van de verbindingsreeks in het web.config-bestand dit exemplaar van de SQL-Database gebruiken, open het bestand web.config, selecteer bestaande vermelding voor de verbinding string en kies vervolgens de toetsen Ctrl + V. De ADO.NET-verbindingsreeks voor de SQL-Database-instantie vervangt de bestaande verbindingsreeks.

1. U moet ook de parameter toevoegen `MultipleActiveResultSets=True` voor de verbindingsreeks. De tekenreeks moet de volgende notatie hebben:

    ```
    connectionString=”Server=tcp:<database_server>.database.windows.net,1433;Database=<database_name>;User ID=<user_name>@<database_server>;Password=<myPassword>;Trusted_Connection=False;Encrypt=True;MultipleActiveResultSets=True"
    ```

1. (Optioneel) Een alternatieve methode voor het wijzigen van de verbindingsreeks rechtstreeks in het bestand web.config is een sectie in een van de bestanden web.config transformatie, afhankelijk van de configuratie die u gebruikt voor het maken van het servicepakket toevoegen. Open het bestand Web.Debug.Config of het bestand Web.Release.Config. De volgende sectie in dit bestand toevoegen:

    ```
    XMLCopy<connectionStrings><addname="DefaultConnection"connectionString="Server=tcp:<database_server>.database.windows.net,1433;Database=<database_name>;User ID=<user_name>@<database_server>;Password=<myPassword>;Trusted_Connection=False;Encrypt=True;MultipleActiveResultSets=True"xdt:Transform="SetAttributes"xdt:Locator="Match(name)"/></connectionStrings>
    ```

1. Sla het bestand dat u hebt gewijzigd en uw toepassing opnieuw publiceren.

### <a name="to-use-an-instance-of-sql-database-by-using-the-azure-classic-portal"></a>Een exemplaar van de SQL-Database gebruiken met het klassieke Azure portal

1. Kies het knooppunt SQL-Databases in de [Azure klassieke portal](http://go.microsoft.com/fwlink/?LinkID=213885).

  - Als het exemplaar van SQL-Database die u wilt gebruiken, kunt u kiezen om deze te openen.

  - Als u alle exemplaren hebt gemaakt, kiest u de gewenste koppeling en maakt u een exemplaar.

1. Nadat u opent of maakt een database-instantie, kiest u de koppeling van **Tekenreeksen** .

1. Kies onderaan de pagina op de koppeling firewall-instellingen te configureren en de standaardwaarden accepteren of configureer de waarden die u nodig hebt.

1. De verbindingsreeks ADO.NET kopiëren, plak deze in uw web.config-bestand via de oude verbindingsreeks voor de database op gebouwen en moet u toevoegen `MultipleActiveResultSets=True`.

## <a name="publish-a-web-application-to-azure"></a>Publiceren van webtoepassingen naar Azure

### <a name="to-publish-a-web-application-to-azure"></a>Voor het publiceren van webtoepassingen naar Azure

1. Voor het testen van de toepassing in de lokale ontwikkeling met behulp van de Azure berekenen emulator, opent het snelmenu voor het project Azure voor de Webrol en kies **instellen als Project opstarten**. Kies de **Debug**, **Foutopsporing starten** (toetsenbord: **F5**).

    In het dialoogvenster **de Azure-omgeving voor foutopsporing starten** wordt geopend en de toepassing wordt gestart in de browser. Zie de tabel in deze sectie voor meer informatie over het starten van elk type webtoepassing in de compute-emulator.

1. Als u de services voor uw toepassing te publiceren naar Azure instelt, moet u een Microsoft-account en een Azure-abonnement hebben. Gebruik de stappen in het volgende onderwerp voor het instellen van uw diensten: [voorbereiden om te publiceren of een Azure toepassing vanuit Visual Studio](vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md).

1. Als u wilt publiceren in de webtoepassing Azure, opent u het snelmenu voor het webproject en kies **publiceren op Azure**.

    Hiermee opent u het dialoogvenster **Azure-toepassing publiceren** en Visual Studio begint het implementatieproces. Zie de sectie **Azure vanuit Visual Studio toepassingen uitgeven** in [een Cloud-Service met behulp van de hulpprogramma's voor Azure publiceren](vs-azure-tools-publishing-a-cloud-service.md)voor meer informatie over het publiceren van de toepassing.

    >[AZURE.NOTE] U kunt ook de webtoepassing uit het project Azure publiceren. Hiertoe opent u het snelmenu voor het project Azure en kies **publiceren**.

1. Overzicht van de voortgang van de installatie, kunt u het **Logboek met faxactiviteit Azure** -venster weergeven. Dit logboek wordt automatisch weergegeven bij het starten van het implementatieproces. Zoals in de volgende afbeelding kunt u het artikel in het logboek voor faxactiviteit gedetailleerde gegevens uitvouwen:

    ![VST_AzureActivityLog](./media/vs-azure-tools-migrate-publish-web-app-to-cloud-service/IC744149.png)

1. (Optioneel) Het implementatieproces annuleren, open het snelmenu voor het artikel in het logboek voor faxactiviteit en kiest u **Annuleren en verwijderen**. Hiermee stopt het implementatieproces en verwijdert de implementatieomgeving van Azure.

    >[AZURE.NOTE] Als u wilt deze implementatieomgeving verwijderen nadat deze is geïmplementeerd, moet u de [Azure klassieke portal](http://go.microsoft.com/fwlink/?LinkID=213885).

1. (Optioneel) Nadat de exemplaren van uw rol hebt gestart, weergegeven Visual Studio de implementatieomgeving automatisch in het knooppunt **Azure berekenen** in de **Cloud Explorer** of **Server Explorer**. Hier kunt u de status van de rol van de afzonderlijke exemplaren bekijken.

    De volgende afbeelding ziet de rol van exemplaren in **Server Explorer** terwijl ze nog steeds in de status Bezig met initialiseren:

    ![VST_DeployComputeNode](./media/vs-azure-tools-migrate-publish-web-app-to-cloud-service/IC744134.png)

1. Als u uw toepassing na implementatie, klik op de pijl naast de implementatie wanneer de status **voltooid** wordt weergegeven in het **logboek voor faxactiviteit Azure**. Hiermee geeft de URL van uw webtoepassing in Azure. Zie de volgende tabel voor informatie over het starten van een specifiek type webtoepassing van Azure.

    De volgende tabel worden de details over het specifieke toepassingen starten vanaf Azure of uitgevoerd of fouten opsporen in een webtoepassing lokaal met behulp van de Emulator Azure berekenen:

  	|Type van Web-toepassing|Run/Debug lokaal met behulp van de Compute-Emulator|Uitgevoerd in Azure|
  	|---|---|---|
  	|ASP.NET-webtoepassing|Kies in het menu **Foutopsporing**, **Foutopsporing starten** (toetsenbord: Kies de toets **F5** .).|Kies de URL-hyperlink weergegeven op het tabblad **distributie** voor het **logboek voor faxactiviteit Azure** de startpagina in de browser geladen.|
  	|ASP.NET MVC 2-webtoepassing|Kies in het menu **Foutopsporing**, **Foutopsporing starten** (toetsenbord: Kies de toets **F5** .).|Kies de URL-hyperlink weergegeven op het tabblad **distributie** voor het **logboek voor faxactiviteit Azure** de startpagina in de browser geladen.|
  	|ASP.NET MVC 3-webtoepassing|Kies in het menu **Foutopsporing**, **Foutopsporing starten** (toetsenbord: Kies de toets **F5** .).|Kies de URL-hyperlink weergegeven op het tabblad **distributie** voor het **logboek voor faxactiviteit Azure** de startpagina in de browser geladen.|
  	|ASP.NET MVC 4-webtoepassing|Kies in het menu **Foutopsporing**, **Foutopsporing starten** (toetsenbord: Kies de toets **F5** .).|Kies de URL-hyperlink weergegeven op het tabblad **distributie** voor het **logboek voor faxactiviteit Azure** de startpagina in de browser geladen.|
  	|Lege ASP.NET-webtoepassing|In de toepassing die u als de startpagina voor uw webproject instelt, moet u een ASPX-pagina toevoegen. Kies in het menu **Foutopsporing**, **Foutopsporing starten** (toetsenbord: Kies de toets **F5** .).|Als u een standaard ASPX-pagina in uw toepassing, kiest u de URL-hyperlink weergegeven op het tabblad **distributie** voor het **logboek voor faxactiviteit Azure** en deze pagina in de browser wordt geladen. Als u een andere ASPX-pagina, moet u Ga naar deze specifieke pagina de volgende notatie gebruiken voor uw url:`<url for deployment>/<name of page>.aspx`|
  	|Silverlight-toepassing|Kies in het menu **Foutopsporing**, **Foutopsporing starten** (toetsenbord: Kies de toets **F5** .).|U moet gaan naar de specifieke pagina voor uw toepassing de volgende notatie gebruiken voor uw url:`<url for deployment>/<name of page>.aspx`|
  	|Silverlight Business Application|Kies in het menu **Foutopsporing**, **Foutopsporing starten** (toetsenbord: Kies de toets **F5** .).|U moet gaan naar de specifieke pagina voor uw toepassing de volgende notatie gebruiken voor uw url:`<url for deployment>/<name of page>.aspx`|
  	|Silverlight navigatie-toepassing|Kies in het menu **Foutopsporing**, **Foutopsporing starten** (toetsenbord: Kies de toets **F5** .).|U moet gaan naar de specifieke pagina voor uw toepassing de volgende notatie gebruiken voor uw url:`<url for deployment>/<name of page>.aspx`|
  	|WCF-servicetoepassing|U moet het .svc-bestand als startpagina instellen voor uw project WCF-Service. Kies in het menu **Foutopsporing**, **Foutopsporing starten** (toetsenbord: Kies de toets **F5** .).|U moet om te navigeren naar het svc-bestand voor de toepassing in de volgende notatie voor de url:`<url for deployment>/<name of service file>.svc`|
  	|WCF Workflow Service-toepassing|U moet het .svc-bestand als startpagina instellen voor uw project WCF-Service. Kies in het menu **Foutopsporing**, **Foutopsporing starten** (toetsenbord: Kies de toets **F5** .).|U moet om te navigeren naar het svc-bestand voor de toepassing in de volgende notatie voor de url:`<url for deployment>/<name of service file>.svc`|
  	|ASP.NET Dynamic entiteiten|Kies in het menu **Foutopsporing**, **Foutopsporing starten** (toetsenbord: Kies de toets **F5** .).|Moet u de verbindingstekenreeks bijwerken (Zie volgende sectie). U moet ook navigeren naar de specifieke pagina voor uw gebruik van de volgende syntaxis voor de url van uw toepassing:`<url for deployment>/<name of page>.aspx`|
  	|ASP.NET Dynamic Data Linq aan SQL|Kies in het menu **Foutopsporing**, **Foutopsporing starten** (toetsenbord: Kies de toets **F5** .).|U moet de stappen in deze procedure: een database SQL Azure gebruiken voor de toepassing (Zie de eerdere sectie in dit onderwerp). U moet ook navigeren naar de specifieke pagina voor uw gebruik van de volgende syntaxis voor de url van uw toepassing:`<url for deployment>/<name of page>.aspx`|

## <a name="update-a-connection-string-for-aspnet-dynamic-entities"></a>Een verbindingstekenreeks voor ASP.NET Dynamic entiteiten bijwerken

### <a name="to-update-a-connection-string-for-aspnet-dynamic-entities"></a>Een verbindingstekenreeks voor ASP.NET Dynamic entiteiten bijwerken

1. Als u wilt een Azure SQL-database die kan worden gebruikt voor een dynamische entiteiten ASP.NET-webtoepassing maakt, volg de stappen in de procedure **voor uw toepassing een Azure SQL-database** , eerder in dit onderwerp.

1. De tabellen en velden die u nodig hebt voor deze database vanuit [Azure klassieke portal](http://go.microsoft.com/fwlink/?LinkID=213885)toevoegen.

1. De verbindingsreeks voor dit type toepassing heeft de volgende indeling in het bestand web.config:  

    ```
    <addname="tempdbEntities"connectionString="metadata=res://*/Model1.csdl|res://*/Model1.ssdl|res://*/Model1.msl;provider=System.Data.SqlClient;provider connection string=&quot;data source=<server name>\SQLEXPRESS;initial catalog=<database name>;integrated security=True;multipleactiveresultsets=True;App=EntityFramework&quot;"providerName="System.Data.EntityClient"/>
    ```

    De *connectionString* -waarde met de ADO.NET-verbindingsreeks voor de database SQL Azure als volgt bijwerken:

    ```
    XMLCopy<addname="tempdbEntities"connectionString="metadata=res://*/Model1.csdl|res://*/Model1.ssdl|res://*/Model1.msl;provider=System.Data.SqlClient;provider connection string=&quot;Server=tcp:<SQL Azure server name>.database.windows.net,1433;Database=<database name>;User ID=<user name>;Password=<password>;Trusted_Connection=False;Encrypt=True;multipleactiveresultsets=True;App=EntityFramework&quot;"providerName="System.Data.EntityClient"/>
    ```

1. Sla het web.config-bestand met de wijzigingen die u hebt aangebracht in de verbindingstekenreeks in het menu Kies **bestand**, **opslaan van web.config**.

## <a name="supported-project-templates"></a>Ondersteunde Project-sjablonen

Voor het publiceren van webtoepassingen naar Azure, moet de toepassing een van de projectsjablonen gebruiken voor C# of Visual Basic die wordt vermeld in de onderstaande tabel.

|Projectgroep sjabloon|Project-sjabloon|
|---|---|
|Web|ASP.NET-webtoepassing|
|Web|ASP.NET MVC 2-webtoepassing|
|Web|ASP.NET MVC 3-webtoepassing|
|Web|MVC4 van de ASP.NET-webtoepassing|
|Web|Lege ASP.NET-webtoepassing|
|Web|ASP.NET MVC 2 leeg Web Application|
|Web|Entiteiten voor ASP.NET Dynamic Data webtoepassing|
|Web|ASP.NET Dynamic Data Linq to SQL Web Application|
|Silverlight|Silverlight-toepassing|
|Silverlight|Silverlight Business Application|
|Silverlight|Silverlight navigatie-toepassing|
|WCF|WCF-servicetoepassing|
|WCF|WCF Workflow Service-toepassing|
|Workflow|WCF Workflow Service-toepassing|

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over publiceren, [voorbereiden, publiceren of distribueren Azure toepassing in Visual Studio](vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md). Controleer ook [Instelling Up met de naam verificatiereferenties](vs-azure-tools-setting-up-named-authentication-credentials.md).
