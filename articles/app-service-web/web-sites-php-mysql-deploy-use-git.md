<properties
    pageTitle="Een PHP-MySQL web app in Azure App Service maken en implementeren met behulp van Git"
    description="Een zelfstudie die laat zien hoe een PHP web app waarin gegevens worden opgeslagen in een MySQL maken en gebruiken van Git distributie naar Azure."
    services="app-service\web"
    documentationCenter="php"
    authors="rmcmurray"
    manager="wpickett"
    editor=""
    tags="mysql"/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="PHP"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

# <a name="create-a-php-mysql-web-app-in-azure-app-service-and-deploy-using-git"></a>Een PHP-MySQL web app in Azure App Service maken en implementeren met behulp van Git

Deze zelfstudie ziet u het maken van een PHP-MySQL web app en het implementeren van deze service [App](http://go.microsoft.com/fwlink/?LinkId=529714) met Git. U gebruikt [PHP][install-php], de MySQL Command-Line Tool (onderdeel van [MySQL][install-mysql]), en [Git] [ install-git] op uw computer geïnstalleerd. De instructies in deze handleiding kunnen worden gevolgd op een willekeurig besturingssysteem, zoals Windows, Mac en Linux. Bij de voltooiing van deze handleiding, hebt u een PHP/MySQL web app met Azure.

U leert:

* Het maken van een web app en een MySQL-database met behulp van de [Portal Azure][management-portal]. Omdat PHP is standaard ingeschakeld in [App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714) , vereist geen speciale uw PHP-code uit te voeren.
* Het publiceren en Azure met Git de toepassing opnieuw te publiceren.
* Het inschakelen van de extensie componist componist automatiseren taken op elke `git push`.

Door deze zelfstudie te volgen, bouwt u een eenvoudig registratie in PHP web app. De toepassing zal worden gehost in Web Apps. Een schermafbeelding van de voltooide toepassing lager is dan:

![Azure PHP-website][running-app]

## <a name="set-up-the-development-environment"></a>De ontwikkelomgeving instellen

In deze zelfstudie wordt ervan uitgegaan dat u beschikt over [PHP][install-php], de MySQL Command-Line Tool (onderdeel van [MySQL][install-mysql]), en [Git] [ install-git] op uw computer geïnstalleerd.

<a id="create-web-site-and-set-up-git"></a>
## <a name="create-a-web-app-and-set-up-git-publishing"></a>Een web app maken en instellen van Git publiceren

Ga als volgt te werk om een web-app en een MySQL-database te maken:

1. Meld u aan bij de [Azure Portal][management-portal].
2. Klik op het pictogram **Nieuw** .

3. Klik op **Alle Zie** naast **Marketplace**. 

4. Klik op **Web + Mobile**en vervolgens **Web app + MySQL**. Klik vervolgens op **maken**.

4. Voer een geldige naam voor de resourcegroep.

    ![Setnaam resourcegroep][resource-group]

5. Voer waarden in voor uw nieuwe web app.

    ![Web app maken][new-web-app]

6. Voer waarden in voor de nieuwe database, inclusief de juridische voorwaarden akkoord.

    ![Nieuwe MySQL-database maken][new-mysql-db]

7. Wanneer de web app is gemaakt, ziet u het nieuwe web app blade.

7. In de **Instellingen** op **Continue implementatie**Klik op _configureren, instellingen vereist_.

    ![Git publicatie instellen][setup-publishing]

8. Selecteer **Lokale Git opslagplaats** voor de bron.

    ![Git repository instellen][setup-repository]


9. Als u wilt dat Git publiceren, moet u een gebruikersnaam en wachtwoord opgeven. Maak een notitie van de gebruikersnaam en het wachtwoord dat u maakt. (Als u een Git repository voordat hebt ingesteld, wordt deze stap overgeslagen.)

    ![Publishing referenties maken][credentials]


## <a name="get-remote-mysql-connection-information"></a>Informatie voor externe MySQL verbinding

Verbinding maken met de MySQL-database die wordt uitgevoerd in het Web Apps, je zal de verbindingsgegevens nodig. Als u de gegevens van de MySQL-verbinding, als volgt te werk:

1. Klik op de database uit de resourcegroep:

    ![Database selecteren][select-database]

2. Selecteer **Eigenschappen**uit de database- **Instellingen**.

    ![Selecteer Eigenschappen][select-properties]

2. Noteer de waarden voor `Database`, `Host`, `User Id`, en `Password`.

    ![Notitie-eigenschappen][note-properties]

## <a name="build-and-test-your-app-locally"></a>Bouwen en testen van uw app lokaal

Nu dat u een web app hebt gemaakt, kunt u uw toepassing lokaal ontwikkelen vervolgens implementeert u deze na het testen.

De aanvraag is een eenvoudig PHP-toepassing waarmee u registreren voor een evenement door middel van uw naam en e-mailadres. Informatie over vorige geregistreerde wordt weergegeven in een tabel. Registratie-informatie wordt opgeslagen in een MySQL-database. De toepassing bestaat uit een bestand (kopiëren en plakken code hieronder):

* **index.php**: een formulier voor registratie en een tabel met informatie over de registrant wordt weergegeven.

Te bouwen en de toepassing lokaal uitvoeren, volgt u de onderstaande stappen. Houd er rekening mee dat deze stappen wordt ervan uitgegaan dat u PHP en MySQL Command-Line Tool (onderdeel van MySQL) is ingesteld op uw lokale computer hebt en dat u de [extensie voor MySQL PDO]hebt ingeschakeld[pdo-mysql].

1. Verbinding maken met de MySQL server, met behulp van de waarde voor `Data Source`, `User Id`, `Password`, en `Database` die u eerder hebt opgehaald:

        mysql -h{Data Source] -u[User Id] -p[Password] -D[Database]

2. De MySQL-opdrachtprompt weergegeven:

        mysql>

3. Plakken in de volgende `CREATE TABLE` opdracht voor het maken van de `registration_tbl` de tabel in de database:

        CREATE TABLE registration_tbl(id INT NOT NULL AUTO_INCREMENT, PRIMARY KEY(id), name VARCHAR(30), email VARCHAR(30), date DATE);

4. In de hoofdmap van uw lokale map application **index.php** bestand te maken.

5. Open het **index.php** bestand in een teksteditor of de IDE en voeg de volgende code toe en voer de nodige wijzigingen gemarkeerd met `//TODO:` opmerkingen.


        <html>
        <head>
        <Title>Registration Form</Title>
        <style type="text/css">
            body { background-color: #fff; border-top: solid 10px #000;
                color: #333; font-size: .85em; margin: 20; padding: 20;
                font-family: "Segoe UI", Verdana, Helvetica, Sans-Serif;
            }
            h1, h2, h3,{ color: #000; margin-bottom: 0; padding-bottom: 0; }
            h1 { font-size: 2em; }
            h2 { font-size: 1.75em; }
            h3 { font-size: 1.2em; }
            table { margin-top: 0.75em; }
            th { font-size: 1.2em; text-align: left; border: none; padding-left: 0; }
            td { padding: 0.25em 2em 0.25em 0em; border: 0 none; }
        </style>
        </head>
        <body>
        <h1>Register here!</h1>
        <p>Fill in your name and email address, then click <strong>Submit</strong> to register.</p>
        <form method="post" action="index.php" enctype="multipart/form-data" >
              Name  <input type="text" name="name" id="name"/></br>
              Email <input type="text" name="email" id="email"/></br>
              <input type="submit" name="submit" value="Submit" />
        </form>
        <?php
            // DB connection info
            //TODO: Update the values for $host, $user, $pwd, and $db
            //using the values you retrieved earlier from the Azure Portal.
            $host = "value of Data Source";
            $user = "value of User Id";
            $pwd = "value of Password";
            $db = "value of Database";
            // Connect to database.
            try {
                $conn = new PDO( "mysql:host=$host;dbname=$db", $user, $pwd);
                $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );
            }
            catch(Exception $e){
                die(var_dump($e));
            }
            // Insert registration info
            if(!empty($_POST)) {
            try {
                $name = $_POST['name'];
                $email = $_POST['email'];
                $date = date("Y-m-d");
                // Insert data
                $sql_insert = "INSERT INTO registration_tbl (name, email, date)
                           VALUES (?,?,?)";
                $stmt = $conn->prepare($sql_insert);
                $stmt->bindValue(1, $name);
                $stmt->bindValue(2, $email);
                $stmt->bindValue(3, $date);
                $stmt->execute();
            }
            catch(Exception $e) {
                die(var_dump($e));
            }
            echo "<h3>Your're registered!</h3>";
            }
            // Retrieve data
            $sql_select = "SELECT * FROM registration_tbl";
            $stmt = $conn->query($sql_select);
            $registrants = $stmt->fetchAll();
            if(count($registrants) > 0) {
                echo "<h2>People who are registered:</h2>";
                echo "<table>";
                echo "<tr><th>Name</th>";
                echo "<th>Email</th>";
                echo "<th>Date</th></tr>";
                foreach($registrants as $registrant) {
                    echo "<tr><td>".$registrant['name']."</td>";
                    echo "<td>".$registrant['email']."</td>";
                    echo "<td>".$registrant['date']."</td></tr>";
                }
                echo "</table>";
            } else {
                echo "<h3>No one is currently registered.</h3>";
            }
        ?>
        </body>
        </html>

4.  Ga naar de toepassingsmap van uw in een terminal en typ de volgende opdracht:

        php -S localhost:8000

U kunt nu bladeren naar **http://localhost:8000 /** om de toepassing te testen.


## <a name="publish-your-app"></a>Publiceren van uw app

Nadat u uw app lokaal hebt getest, kunt u deze publiceren naar Web Apps met behulp van Git. U zal uw lokale Git repository initialiseren en publiceer de toepassing.

> [AZURE.NOTE]
> Dit zijn de stappen die in de Portal Azure aan het einde van een web app maken en Set up Git Publishing hierboven weergegeven.

1. (Optioneel)  Als u bent vergeten of je Git remote repostitory URL verkeerd geplaatste, navigeer naar de eigenschappen van het web app op de Azure-Portal.

1. Open GitBash (of een terminal als Git in de `PATH`), wijzigt u mappen in de hoofdmap van de toepassing en voer de volgende opdrachten:

        git init
        git add .
        git commit -m "initial commit"
        git remote add azure [URL for remote repository]
        git push azure master

    U wordt gevraagd het wachtwoord dat u eerder hebt gemaakt.

    ![Eerste Push naar Azure via Git][git-initial-push]

2. Ga naar **http://[site name].azurewebsites.net/index.php** voor de toepassing (deze informatie wordt opgeslagen in uw account dashboard) te gebruiken:

    ![Azure PHP-website][running-app]

Nadat u uw app hebt gepubliceerd, kunt u beginnen met het aanbrengen van wijzigingen in deze en publiceren met Git.

## <a name="publish-changes-to-your-app"></a>Wijzigingen publiceren naar uw app

Om wijzigingen te publiceren naar uw app, als volgt te werk:

1. Wijzigingen aanbrengen in uw app lokaal.
2. Open GitBash (of een terminal, it Git is in de `PATH`), mappen naar de hoofdmap van uw app wijzigen en voer de volgende opdrachten:

        git add .
        git commit -m "comment describing changes"
        git push azure master

    U wordt gevraagd het wachtwoord dat u eerder hebt gemaakt.

    ![Veranderingen in de site naar Azure pushen via Git][git-change-push]

3. Ga naar **http://[site name].azurewebsites.net/index.php** voor een overzicht van uw app en eventuele wijzigingen die u hebt aangebracht:

    ![Azure PHP-website][running-app]

>[AZURE.NOTE] Als u wilt aan de slag met Azure App-Service voordat u aanmelden voor een account met Azure, gaat u naar de [App-Service probeert](http://go.microsoft.com/fwlink/?LinkId=523751), waar u direct een starter tijdelijk web app in de App-Service maken kunt. Geen creditcard vereist; geen verplichtingen.

<a name="composer"></a>
## <a name="enable-composer-automation-with-the-composer-extension"></a>Met de extensie componist componist inschakelen

Standaard doen het implementatieproces git in App-Service niet iets met composer.json, als u in uw project PHP hebt. Kunt u tijdens het verwerken van composer.json `git push` door de componist-extensie inschakelen.

1. In de PHP web app van blade in [Azure portal][management-portal], klikt u op **Extra** > **extensies**.

    ![Composer extensie-instellingen][composer-extension-settings]

2. Klik op **toevoegen**en klik vervolgens op **Composer**.

    ![Composer uitbreiding toevoegen][composer-extension-add]
    
3. Klik op **OK** om de juridische voorwaarden accepteren. Klik op **OK** om opnieuw toe te voegen de extensie.

    De blade **-extensies geïnstalleerd** laten nu zien de extensie Composer.  
    ![Composer extensie weergeven][composer-extension-view]
    
4. Nu, `git add`, `git commit`, en `git push` , zoals in de vorige sectie. U ziet nu Composer installeren van afhankelijkheden die zijn gedefinieerd in composer.json.

    ![Composer uitbreiding succes][composer-extension-success]

## <a name="next-steps"></a>Volgende stappen

Zie de [PHP Developer Center](/develop/php/)voor meer informatie.

<!-- URL List -->

[install-php]: http://www.php.net/manual/en/install.php
[install-SQLExpress]: http://www.microsoft.com/download/details.aspx?id=29062
[install-Drivers]: http://www.microsoft.com/download/details.aspx?id=20098
[install-git]: http://git-scm.com/
[install-mysql]: http://dev.mysql.com/downloads/mysql/
[pdo-mysql]: http://www.php.net/manual/en/ref.pdo-mysql.php
[management-portal]: https://portal.azure.com
[sql-database-editions]: http://msdn.microsoft.com/library/windowsazure/ee621788.aspx

<!-- IMG List -->

[running-app]: ./media/web-sites-php-mysql-deploy-use-git/running_app_2.png
[new-website]: ./media/web-sites-php-mysql-deploy-use-git/new_website2.png
[custom-create]: ./media/web-sites-php-mysql-deploy-use-git/create_web_mysql.png
[new-mysql-db]: ./media/web-sites-php-mysql-deploy-use-git/create_db.png
[go-to-webapp]: ./media/web-sites-php-mysql-deploy-use-git/select_webapp.png
[setup-git-publishing]: ./media/web-sites-php-mysql-deploy-use-git/setup_git_publishing.png
[credentials]: ./media/web-sites-php-mysql-deploy-use-git/save_credentials.png
[resource-group]: ./media/web-sites-php-mysql-deploy-use-git/set_group.png
[new-web-app]: ./media/web-sites-php-mysql-deploy-use-git/create_wa.png
[setup-publishing]: ./media/web-sites-php-mysql-deploy-use-git/setup_deploy.png
[setup-repository]: ./media/web-sites-php-mysql-deploy-use-git/select_local_git.png
[select-database]: ./media/web-sites-php-mysql-deploy-use-git/select_database.png
[select-properties]: ./media/web-sites-php-mysql-deploy-use-git/select_properties.png
[note-properties]: ./media/web-sites-php-mysql-deploy-use-git/note-properties.png

[git-instructions]: ./media/web-sites-php-mysql-deploy-use-git/git-instructions.png
[git-change-push]: ./media/web-sites-php-mysql-deploy-use-git/php-git-change-push.png
[git-initial-push]: ./media/web-sites-php-mysql-deploy-use-git/php-git-initial-push.png

[composer-extension-settings]: ./media/web-sites-php-mysql-deploy-use-git/composer-extension-settings.png
[composer-extension-add]: ./media/web-sites-php-mysql-deploy-use-git/composer-extension-add.png
[composer-extension-view]: ./media/web-sites-php-mysql-deploy-use-git/composer-extension-view.png
[composer-extension-success]: ./media/web-sites-php-mysql-deploy-use-git/composer-extension-success.png
