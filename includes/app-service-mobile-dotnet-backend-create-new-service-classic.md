1. Inloggen op de [Portal Azure].

2. Klik op **+ Nieuw** > **Web + Mobile** > **Mobile App**, Geef een naam op voor uw mobiele App backend.

3. Selecteer een bestaande brongroep voor de **Resourcegroep**of een nieuwe maken (met dezelfde naam als uw app). 
 
    Selecteer een andere App Service plan of een nieuwe maken. Voor meer informatie over App Services plannen en maken van een nieuw plan in verschillende prijzen trapsgewijs en Zie [Azure App Service plannen diepgaand overzicht](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)op de gewenste locatie.

4. Voor de **App serviceplan**is het standaardschema (in de [standaard laag](https://azure.microsoft.com/pricing/details/app-service/)) geselecteerd. U kunt ook een ander schema, of [Maak een nieuwe](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md#create-an-app-service-plan)selecteren. De App Service-schema-instellingen bepalen de [locatie, functies, de kosten en resources te berekenen](https://azure.microsoft.com/pricing/details/app-service/) die zijn gekoppeld aan uw app. 

    Nadat u een over het plan besluit, klikt u op **maken**. Hiermee maakt u de back-end mobiele App. 
    
6. In het blad van de **Instellingen** voor de nieuwe mobiele App backend, klikt u op **Snel starten** > uw client app platform > **verbinding maken met een database**. 

    ![](./media/app-service-mobile-dotnet-backend-create-new-service/dotnet-backend-create-data-connection.png)

7. Klik op **SQL-Database**in de blade **gegevensverbinding toevoegen**  > **een nieuwe database maken**, typt u de **naam**van de database, kies een prijzen laag en klik vervolgens op **Server**.  U kunt deze nieuwe database opnieuw gebruiken. Als u al een database op dezelfde locatie, kunt u in plaats daarvan **gebruik van een bestaande database**. Het gebruik van een database op een andere locatie wordt niet aanbevolen omdat de kosten voor bandbreedte en een hogere latentie.
 
    ![](./media/app-service-mobile-dotnet-backend-create-new-service/dotnet-backend-create-db.png)

8. Typ een unieke naam in het veld **servernaam** in de **nieuwe server** -blade, bieden een aanmeldings-id en wachtwoord **toestaan azure services voor toegang tot de server**controleren en klik op **OK**. Hiermee maakt u de nieuwe database.

9. Terug in de blade **gegevensverbinding toevoegen** klikt u op de **verbindingsreeks**, typ waarden voor de aanmeldingsnaam en het wachtwoord voor de database en klikt u op **OK**. Wacht een paar minuten om de database te worden geïmplementeerd voordat u verdergaat.

<!-- URLs. -->
[Azure Portal]: https://portal.azure.com/
