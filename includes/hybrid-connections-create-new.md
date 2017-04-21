
1. Op de machine op locatie bij aanmelden [Azure Management Portal](http://manager.windowsazure.com) (dit is de oude portal).

2. Onderaan in het navigatiedeelvenster, selecteer **+ Nieuw** > **App Services** > **Service BizTalk-** > **Custom maken**.

3. Een **BizTalk-servicenaam** en een **editie**te selecteren. 

    In deze zelfstudie wordt **mobile1**. U moet een unieke naam voor de nieuwe BizTalk-Service te leveren.

4. Wanneer u de BizTalk-Service hebt gemaakt, selecteert u het tabblad **Hybride verbindingen** en klik vervolgens op **toevoegen**.

    ![Hybride verbinding toevoegen](./media/hybrid-connections-create-new/3.png)

    Hiermee maakt u een nieuwe hybride verbinding.

5. Geef een **naam** en de **Host-naam** voor de verbinding van de hybride en **poort** ingesteld op `1433`. 
  
    ![Hybride verbinding configureren](./media/hybrid-connections-create-new/4.png)

    De hostnaam is de naam van de server op locatie. Hiermee wordt de verbinding hybride toegang tot SQL Server op poort 1433. Als u van een benoemd exemplaar van SQL Server gebruikmaakt, gebruik dan de statische poort die u eerder hebt gedefinieerd.

6. Nadat de nieuwe verbinding is gemaakt, wordt de status van de ziet **On-premises onvolledige installatie**van de nieuwe verbinding.

7. Terug te keren naar uw mobiele service, klikt u op **configureren**, Ga naar **hybride verbindingen** klikt u op **verbinding toevoegen-hybride**, de hybride-verbinding die u zojuist hebt gemaakt en klik op **OK**.

    Hierdoor kunt uw mobiele service om uw nieuwe hybride verbinding te gebruiken.

Vervolgens moet u de hybride Connection Manager installeren op de computer op gebouwen.