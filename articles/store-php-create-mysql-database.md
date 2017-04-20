<properties
    pageTitle="Maken en maak verbinding met een MySQL-database in Azure"
    description="Informatie over het gebruik van de portal Azure een MySQL-database maken en vervolgens verbinding maken vanuit een PHP web app in Azure."
    documentationCenter="php"
    services="app-service\web"
    authors="cephalin"
    manager="wpickett"
    editor=""
    tags="mysql"/>

<tags
    ms.service="multiple"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="PHP"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm;cephalin"/>

# <a name="create-and-connect-to-a-mysql-database-in-azure"></a>Maken en maak verbinding met een MySQL-database in Azure

In deze zelfstudie wordt beschreven hoe een MySQL-database in de [Azure portal](https://portal.azure.com) (provider is [ClearDB](http://www.cleardb.com/)) en het tot stand te brengen vanuit een PHP web app in [Azure App-Service](./app-service/app-service-value-prop-what-is.md)wordt uitgevoerd. 

> [AZURE.NOTE] U kunt ook een MySQL-database als onderdeel van een [marktplaats app sjabloon](./app-service-web/app-service-web-create-web-app-from-marketplace.md)maken.

## <a name="create-a-mysql-database-in-azure-portal"></a>Het maken van een MySQL-database in Azure portal

Als u wilt een MySQL-database maken in de portal Azure, het volgende doen:

1. Log in op de [Azure portal](https://portal.azure.com).

2. Klik in het linkermenu op **Nieuw** > **Data- +** > **MySQL-Database**.

    ![Het maken van een MySQL-database in Azure - start](./media/store-php-create-mysql-database/create-db-1-start.png)

2. In de nieuwe MySQL Database [blade](azure-portal-overview.md)configureert u uw nieuwe MySQL-database als volgt (*blade*: een portal-pagina die wordt geopend horizontaal):

    - **Naam**: Typ de naam van een uniek identificeerbaar
    - **Abonnement**: Kies het abonnement te gebruiken
    - **Type Database**: Selecteer **gedeelde** voor goedkope of gratis lagen of **speciaal** toegewezen resources ophalen. 
    - **Resourcegroep**: de MySQL-database toevoegen aan een bestaande [groep](../azure-resource-manager/resource-group-overview.md) of in een nieuwe plaatsen. Bronnen in dezelfde groep kunnen eenvoudig worden beheerd samen.
    - **Locatie**: Selecteer een locatie dicht bij u. Wanneer u deze toevoegt aan een bestaande resourcegroep, bent u naar de locatie van de resourcegroep vergrendeld.
    - **Prijzen laag**: klik op **Prijzen laag**en selecteer vervolgens een optie voor de prijs (laag**kwik** vrij is), en klik vervolgens op **selecteren**. 
    - **Juridische voorwaarden**: **Juridische voorwaarden**op, bekijk de details en klik op **kopen**.
    - **Pincode voor dashboard**: Selecteer deze optie als u toegang wilt tot deze rechtstreeks vanuit het dashboard. Dit is vooral handig als u bekend bent met de navigatie in de portal nog niet.
    
    De volgende schermafdruk is slechts een voorbeeld van hoe u uw MySQL-database kunt configureren.  
    ![Maak een MySQL-database in Azure - configureren](./media/store-php-create-mysql-database/create-db-2-configure.png)

3. Wanneer u klaar bent configureren, klikt u op **maken**.

    ![Het maken van een MySQL-database in Azure - maken](./media/store-php-create-mysql-database/create-db-3-create.png)

    Er verschijnt een pop-upbericht verhuur die u weet dat de implementatie is gestart.

    ![Maak een MySQL-database in Azure - in uitvoering](./media/store-php-create-mysql-database/create-db-4-started-status.png)

    U krijgt een andere pop-nadat de implementatie is voltooid. De portal voor uw database MySQL blade ook automatisch geopend.

<a name="connect"></a>
## <a name="connect-to-your-mysql-database"></a>Verbinding maken met uw database MySQL

Overzicht van de verbindingsgegevens voor uw nieuwe MySQL-database, klikt u op **Eigenschappen** in uw webtoepassing blade.
    
![Het maken van een MySQL-database in Azure - MySQL database blade](./media/store-php-create-mysql-database/create-db-5-finished-db-blade.png)

Nu kunt u verbindingsgegevens in een web app. Een voorbeeld dat laat zien hoe met de gegevens van een eenvoudig PHP-app is beschikbaar [hier](https://github.com/WindowsAzure/azure-sdk-for-php-samples/tree/master/tasklist-mysql).

## <a name="connect-a-laravel-web-app-from-the-php-get-started-tutorial"></a>Verbinding maken met een Laravel web app (van de PHP get begonnen-zelfstudie)

Stel dat u de zelfstudie [maken, configureren en implementeren van een PHP web app met Azure](./app-service-web/app-service-web-php-get-started.md) net klaar en hebben een [Laravel](https://www.laravel.com/) web app met Azure. Mogelijkheden van een database kunt u eenvoudig toevoegen aan uw app Laravel. Volg de volgende stappen uit:

>[AZURE.NOTE] De volgende stappen wordt ervan uitgegaan dat u klaar bent met de zelfstudie [maken, configureren en implementeren van een PHP web app met Azure](./app-service-web/app-service-web-php-get-started.md).

1. De app Laravel in uw lokale ontwikkelomgeving om te verwijzen naar de MySQL-database configureren. Open hiertoe `.env` van uw Laravel app van de hoofdmap en de MySQL-database-opties configureren.

        DB_CONNECTION=mysql
        DB_HOST=<HOSTNAME_from_properties_blade>
        DB_PORT=<PORT_from_properties_blade>
        DB_DATABASE=<see_note_below>
        DB_USERNAME=<USERNAME_from_properties_blade>
        DB_PASSWORD=<PASSWORD_from_properties_blade>

    >[AZURE.NOTE] In het blad **Eigenschappen** de naam van uw database MySQL kan of kan niet worden weergegeven in het veld **DATABASENAAM** . Is het beter om de parameter Database in het veld **CONNECTION STRING** . 
    >
    >![Maak een MySQL-database in Azure - in uitvoering](./media/store-php-create-mysql-database/connect-db-1-database-name.png)

2. De snelste manier om te controleren of u toegang tot MySQL nu is te gebruiken [van Laravel standaard verificatie steigers](https://laravel.com/docs/5.2/authentication#authentication-quickstart). Voer de volgende opdrachten vanaf de hoofdmap van uw app Laravel in de opdrachtregel terminal:

         php artisan migrate
         php artisan make:auth

    De eerste opdracht maakt u de tabellen in Azure op basis van vooraf gedefinieerde migraties in de `database/migrations` directory en de tweede opdracht scaffolds de fundamentele weergaven en routes voor de verificatie en gebruikersregistratie.

3. De ontwikkelingsserver nu uitvoeren:

        php artisan serve

4. Ga naar http://localhost:8000 en een nieuwe gebruiker registreren, zoals wordt weergegeven in de browser:

    ![Verbinding maken met MySQL-database in Azure - gebruiker registreren](./media/store-php-create-mysql-database/connect-db-2-development-server.png)

    Volg de volledige gebruikersinterface-prompt de registratie. Nadat de registratie is voltooid, wordt u aangemeld.
    
    ![Verbinding maken met MySQL-database in Azure - gebruiker registreren](./media/store-php-create-mysql-database/connect-db-3-registered-user.png)

    U ontwikkelt nu uw app ten opzichte van de MySQL-database in Azure.

5. Nu hoeft u voor het repliceren van de `.env` instellingen van uw Azure web app. Voer de volgende Azure CLI-opdrachten:

        azure site appsetting add DB_CONNECTION=mysql
        azure site appsetting add DB_HOST=<HOSTNAME_from_properties_blade>
        azure site appsetting add DB_PORT=<PORT_from_properties_blade>
        azure site appsetting add DB_DATABASE=<Database_param_from_CONNECTION_INFO_from_properties_blade>
        azure site appsetting add DB_USERNAME=<USERNAME_from_properties_blade>
        azure site appsetting add DB_PASSWORD=<PASSWORD_from_properties_blade>

    Lees hoe dit in [de Azure web app configureren werkt](./app-service-web/app-service-web-php-get-started.md#configure).

6. Daarna commit en push-lokale wijzigingen in de eerder bij het uitvoeren naar Azure `php artisan make:auth`.

        git add .
        git commit -m "scaffold auth views and routes"
        git push azure master

7. Ga naar de Azure web app.

        azure site browse

8. Meld u aan met de referenties van de gebruiker die u eerder hebt gemaakt.

    ![Verbinding maken met MySQL-database in Azure - Ga naar Azure web app.](./media/store-php-create-mysql-database/connect-db-4-browse-azure-webapp.png)

    Nadat u zich hebt aangemeld, moet u de beschrijvende na login scherm zien.
    
    ![Verbinding maken met MySQL-database in Azure - ingelogd](./media/store-php-create-mysql-database/connect-db-5-logged-in.png)

    Gefeliciteerd, uw PHP web app in Azure is nu toegang krijgen tot gegevens uit de database MySQL. 

## <a name="next-steps"></a>Volgende stappen

Zie de [PHP Developer Center](/develop/php/)voor meer informatie.
