<properties 
    pageTitle="Herstellen van een app in Azure" 
    description="Informatie over het herstellen van uw app vanaf een back-up." 
    services="app-service" 
    documentationCenter="" 
    authors="cephalin" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/06/2016" 
    ms.author="cephalin"/>

# <a name="restore-an-app-in-azure"></a>Herstellen van een app in Azure

In dit artikel wordt beschreven hoe u een app in [Azure App Service](../app-service/app-service-value-prop-what-is.md) die u eerder een reservekopie hebt (Zie [Back-up van uw app in Azure](web-sites-backup.md)) herstellen. U kunt uw app met de gekoppelde databases (SQL-Database of MySQL) op verzoek terugzetten in een vorige staat of een nieuwe toepassing op basis van uw oorspronkelijke app back-up maken. Maken van een nieuwe toepassing die wordt uitgevoerd in parallel aan de meest recente versie is handig voor A / B testen.

Terugzetten uit back-ups is beschikbaar voor toepassingen die worden uitgevoerd in de **Standard** - en **Premium** -laag. Voor meer informatie over het aanpassen van uw app Zie [schalen van een app in Azure](web-sites-scale.md). **Premium** -laag kunt een groter aantal dagelijkse back-ups moeten worden uitgevoerd dan de **standaard** laag.

<a name="PreviousBackup"></a>
## <a name="restore-an-app-from-an-existing-backup"></a>Toepassing van een bestaande back-up terugzetten

1. Klik op het blad van de **Instellingen** van uw app in de Portal Azure, **back-ups** om de **back-ups** blade weer te geven. Klik vervolgens op **Nu terugzetten** op de opdrachtbalk. 
    
    ![Kies nu terugzetten][ChooseRestoreNow]

3. In de blade **terugzetten** selecteert u eerst de bron van de back-up. 

    ![](./media/web-sites-restore/021ChooseSource.png)
    
    De optie **back-up App** ziet u alle bestaande back-ups van de huidige app en u deze eenvoudig kunt selecteren. 
    De optie **opslag** kunt u een back-up-ZIP-bestand selecteren in een bestaande account Azure opslag en container op uw abonnement. 
    Als u probeert te herstellen van een back-up van een andere toepassing, moet u de optie **opslag** gebruiken.

4. Geef de bestemming voor het herstellen van de app in de **bestemming te herstellen**.

    ![](./media/web-sites-restore/022ChooseDestination.png)
    
    >[AZURE.WARNING] Als u **overschrijven**selecteert, worden alle bestaande gegevens in uw huidige app gewist. Controleer voordat u op **OK**klikt, of dat is precies wat u wilt doen.
    
    U kunt **Bestaande App** de app back-up terugzetten naar een andere toepassing in dezelfde groep resoure. Voordat u deze optie gebruikt, moet u reeds een andere toepassing in de resourcegroep met het spiegelen van de configuratie van de database naar een gedefinieerd in de app back-up. 
    
5. Klik op **OK**.

<a name="StorageAccount"></a>
## <a name="download-or-delete-a-backup-from-a-storage-account"></a>Downloaden of een back-up van een opslag-account verwijderen
    
1. Vanuit het hoofdvenster **Bladeren** blade van de Azure-Portal **opslag accounts**selecteren.
    
    Een lijst van uw bestaande accounts voor de opslag wordt weergegeven. 
    
2. Selecteer de opslag rekening met de back-up die u wilt downloaden of verwijderen.
    
    Het blad voor de opslag wordt weergegeven.

3. Selecteer de gewenste container in de blade opslag-accountn
    
    ![Containers weergeven][ViewContainers]

4. Selecteer back-upbestand dat u wilt downloaden of verwijderen.

    ![ViewContainers](./media/web-sites-restore/03ViewFiles.png)

5. Klik op **downloaden** of **verwijderen** afhankelijk van wat u wilt doen.  

<a name="OperationLogs"></a>
## <a name="monitor-a-restore-operation"></a>Een bewerking voor terugzetten controleren
    
1. Ga naar het **Controlelogboek** blade in Azure portal voor details over het slagen of mislukken van de bewerking voor terugzetten app. 
    
    De blade **controlelogboeken** worden alle bedrijfsactiviteiten, niveau, status, bron en details van de tijd weergegeven.
    
2. Schuif naar beneden om te zoeken naar de gewenste bewerking voor terugzetten en selecteer het.

De beschikbare gegevens die betrekking hebben op de bewerking voor terugzetten weergegeven de blade details.
    
## <a name="next-steps"></a>Volgende stappen

Ook een back-up en terugzetten App Service apps met REST API (Zie [Overige gebruik een back-up en terugzetten App Service apps](websites-csm-backup.md)).

>[AZURE.NOTE] Als u wilt aan de slag met Azure App-Service voordat u aanmelden voor een account met Azure, gaat u naar de [App-Service probeert](http://go.microsoft.com/fwlink/?LinkId=523751), waar u direct een starter tijdelijk web app in de App-Service maken kunt. Geen creditcard vereist; geen verplichtingen.


<!-- IMAGES -->
[ChooseRestoreNow]: ./media/web-sites-restore/02ChooseRestoreNow.png
[ViewContainers]: ./media/web-sites-restore/03ViewContainers.png
[StorageAccountFile]: ./media/web-sites-restore/02StorageAccountFile.png
[BrowseCloudStorage]: ./media/web-sites-restore/03BrowseCloudStorage.png
[StorageAccountFileSelected]: ./media/web-sites-restore/04StorageAccountFileSelected.png
[ChooseRestoreSettings]: ./media/web-sites-restore/05ChooseRestoreSettings.png
[ChooseDBServer]: ./media/web-sites-restore/06ChooseDBServer.png
[RestoreToNewSQLDB]: ./media/web-sites-restore/07RestoreToNewSQLDB.png
[NewSQLDBConfig]: ./media/web-sites-restore/08NewSQLDBConfig.png
[RestoredContosoWebSite]: ./media/web-sites-restore/09RestoredContosoWebSite.png
[DashboardOperationLogsLink]: ./media/web-sites-restore/10DashboardOperationLogsLink.png
[ManagementServicesOperationLogsList]: ./media/web-sites-restore/11ManagementServicesOperationLogsList.png
[DetailsButton]: ./media/web-sites-restore/12DetailsButton.png
[OperationDetails]: ./media/web-sites-restore/13OperationDetails.png
 
