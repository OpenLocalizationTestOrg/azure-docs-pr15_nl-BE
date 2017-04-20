<properties 
    pageTitle="Een PHP-MySQL web app in Azure App Service maken en implementeren met behulp van FTP" 
    description="Een zelfstudie die laat zien hoe het maken van een PHP web app waarin gegevens worden opgeslagen in een MySQL- en FTP-implementatie met Azure." 
    services="app-service\web" 
    documentationCenter="php" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="PHP" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>


#<a name="create-a-php-mysql-web-app-in-azure-app-service-and-deploy-using-ftp"></a>Een PHP-MySQL web app in Azure App Service maken en implementeren met behulp van FTP

Deze zelfstudie ziet u hoe een PHP-MySQL web app maken en implementeren via FTP. In deze zelfstudie wordt ervan uitgegaan dat u beschikt over [PHP][install-php], [MySQL][install-mysql], een webserver en een FTP-client op uw computer geïnstalleerd. De instructies in deze handleiding kunnen worden gevolgd op een willekeurig besturingssysteem, zoals Windows, Mac en Linux. Bij de voltooiing van deze handleiding, hebt u een PHP/MySQL web app met Azure.
 
U leert:

* Het maken van een web app en een MySQL-database met behulp van de Portal Azure. Omdat PHP is standaard ingeschakeld in Web Apps, vereist geen speciale uw PHP-code uit te voeren.
* Het publiceren van uw toepassing naar Azure via FTP.
 
Door deze zelfstudie te volgen, bouwt u een eenvoudig registratie in PHP web app. De toepassing zal worden gehost in een Web App. Een schermafbeelding van de voltooide toepassing lager is dan:

![Azure PHP-website][running-app]

>[AZURE.NOTE] Als u wilt aan de slag met Azure App-Service voordat u aanmelden voor een account, gaat u naar de [App-Service probeert](http://go.microsoft.com/fwlink/?LinkId=523751), waar u direct een starter tijdelijk web app in de App-Service maken kunt. Geen creditcard nodig, geen verplichtingen. 


##<a name="create-a-web-app-and-set-up-ftp-publishing"></a>Een web app maken en instellen van FTP-publicatie

Ga als volgt te werk om een web-app en een MySQL-database te maken:

1. Meld u aan bij de [Azure Portal][management-portal].
2. Klik op het pictogram **+ nieuwe** op de bovenkant van de Portal Azure links.

    ![Nieuwe website Azure maken][new-website]

3. Typ in het zoeken **Web app + MySQL** en klik op **Web app + MySQL**.

    ![Een nieuwe website aangepaste maken][custom-create]

4. Klik op **maken**. Geef een naam op voor de unieke app, een geldige naam voor de resourcegroep en een nieuwe.

    ![Setnaam resourcegroep][resource-group]


6. Voer waarden in voor de nieuwe database, inclusief de juridische voorwaarden akkoord.

    ![Nieuwe MySQL-database maken][new-mysql-db]
    
7. Wanneer de web app is gemaakt, ziet u de nieuwe app service blade.


6. Klik op **Instellingen** > **implementatie referenties**. 

    ![Set implementatie referenties][set-deployment-credentials]

7. Als u wilt dat FTP-publicatie, moet u een gebruikersnaam en wachtwoord opgeven. De referenties op te slaan en noteer de gebruikersnaam en het wachtwoord dat u maakt.

    ![Publishing referenties maken][portal-ftp-username-password]

##<a name="build-and-test-your-app-locally"></a>Bouwen en testen van uw app lokaal

De aanvraag is een eenvoudig PHP-toepassing waarmee u registreren voor een evenement door middel van uw naam en e-mailadres. Informatie over vorige geregistreerde wordt weergegeven in een tabel. Registratie-informatie wordt opgeslagen in een MySQL-database. De app bestaat uit twee bestanden:

* **index.php**: een formulier voor registratie en een tabel met informatie over de registrant wordt weergegeven.
* **CreateTable.php**: de tabel MySQL voor de toepassing wordt gemaakt. Dit bestand wordt slechts eenmaal worden gebruikt.

Te bouwen en de app lokaal uitvoeren, volgt u de onderstaande stappen. Merk op dat deze stappen wordt ervan uitgegaan dat u PHP, MySQL, en een webserver instellen op uw lokale computer en dat u de [extensie voor MySQL PDO]hebt ingeschakeld[pdo-mysql].

1. Maken van een MySQL-database genaamd `registration`. U kunt dit doen vanaf de opdrachtprompt MySQL met deze opdracht:

        mysql> create database registration;

2. Maak een map in de hoofdmap van uw webserver, `registration` en maakt u twee bestanden erin - een met de naam `createtable.php` en een met de naam `index.php`.

3. Open de `createtable.php` -bestand in een teksteditor of de IDE en voeg de volgende code toe. Deze code wordt gebruikt voor het maken van de `registration_tbl` tabel de `registration` database.

        <?php
        // DB connection info
        $host = "localhost";
        $user = "user name";
        $pwd = "password";
        $db = "registration";
        try{
            $conn = new PDO( "mysql:host=$host;dbname=$db", $user, $pwd);
            $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );
            $sql = "CREATE TABLE registration_tbl(
                        id INT NOT NULL AUTO_INCREMENT, 
                        PRIMARY KEY(id),
                        name VARCHAR(30),
                        email VARCHAR(30),
                        date DATE)";
            $conn->query($sql);
        }
        catch(Exception $e){
            die(print_r($e));
        }
        echo "<h3>Table created.</h3>";
        ?>

    > [AZURE.NOTE] 
    > U moet de waarden voor <code>$user</code> en <code>$pwd</code> met uw lokale MySQL gebruikersnaam en wachtwoord.

4. Open een webbrowser en Ga naar [http://localhost/registration/createtable.php][localhost-createtable]. Hiermee maakt u de `registration_tbl` tabel in de database.

5. Open het **index.php** bestand in een teksteditor of de IDE en de eenvoudige HTML- en CSS-code voor de pagina (de PHP-code wordt toegevoegd in latere stappen) toe te voegen.

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

        ?>
        </body>
        </html>

6. Toevoegen van PHP-code, PHP-code voor de verbinding met de database.

        // DB connection info
        $host = "localhost";
        $user = "user name";
        $pwd = "password";
        $db = "registration";
        // Connect to database.
        try {
            $conn = new PDO( "mysql:host=$host;dbname=$db", $user, $pwd);
            $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );
        }
        catch(Exception $e){
            die(var_dump($e));
        }

    > [AZURE.NOTE]
    > Nogmaals, moet u de waarden voor <code>$user</code> en <code>$pwd</code> met uw lokale MySQL gebruikersnaam en wachtwoord.

7. Voeg na de database verbinding code, code voor het invoegen van registratie-informatie in de database.

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

8. Ten slotte, na de code, code voor het ophalen van gegevens uit de database toevoegen.

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

U kunt nu bladeren naar [http://localhost/registration/index.php] [ localhost-index] voor het testen van de toepassing.

##<a name="get-mysql-and-ftp-connection-information"></a>MySQL- en FTP-verbindingsgegevens ophalen

Verbinding maken met de MySQL-database die wordt uitgevoerd in het Web Apps, je zal de verbindingsgegevens nodig. Als u de gegevens van de MySQL-verbinding, als volgt te werk:

1. Van de service app web app blade Klik op de koppeling bron groep:

    ![De resourcegroep selecteren][select-resourcegroup]

1. Klik op de database uit de resourcegroep:

    ![Database selecteren][select-database]

2. Selecteer **Instellingen**uit de database summary, > **Eigenschappen**.

    ![Selecteer Eigenschappen][select-properties]
    
2. Noteer de waarden voor `Database`, `Host`, `User Id`, en `Password`.

    ![Notitie-eigenschappen][note-properties]

3. Klik op de koppeling **downloaden profiel publiceren** in de rechterbenedenhoek van de pagina in uw web app:

    ![Download profiel publiceren][download-publish-profile]

4. Open de `.publishsettings` bestand in een XML-editor. 

3. Zoek de `<publishProfile >` -element met `publishMethod="FTP"` die er ongeveer zo uitziet:

        <publishProfile publishMethod="FTP" publishUrl="ftp://[mysite].azurewebsites.net/site/wwwroot" ftpPassiveMode="True" userName="[username]" userPWD="[password]" destinationAppUrl="http://[name].antdf0.antares-test.windows-int.net" 
            ...
        </publishProfile>
    
Noteer de `publishUrl`, `userName`, en `userPWD` kenmerken.

##<a name="publish-your-app"></a>Publiceren van uw app

Nadat u uw app lokaal hebt getest, kunt u deze publiceren naar uw web app via FTP. U moet eerst de verbindingsgegevens voor de database in de toepassing bijwerken. U hebt verkregen informatie voor de databaseverbinding met eerder (in de sectie **MySQL opvragen en FTP-verbindingsgegevens** ), werken de volgende informatie in **zowel** de `createdatabase.php` en `index.php` bestanden met de juiste waarden:

    // DB connection info
    $host = "value of Data Source";
    $user = "value of User Id";
    $pwd = "value of Password";
    $db = "value of Database";

U bent nu gereed voor het publiceren van uw app via FTP.

1. Open uw FTP-client van keuze.

2. Voer het *hostgedeelte van de naam* van de `publishUrl` kenmerk dat u hierboven hebt genoteerd in uw FTP-client.

3. Voer het `userName` en `userPWD` kenmerken die u hierboven hebt genoteerd ongewijzigd in uw FTP-client.

4. Een verbinding tot stand te brengen.

Nadat u verbinding hebt gemaakt zal u kunnen uploaden en downloaden van bestanden naar wens zijn. Zorg ervoor dat u uploaden van bestanden naar de hoofdmap `/site/wwwroot`.

Na het uploaden van beide `index.php` en `createtable.php`, Ga naar **http://[site name].azurewebsites.net/createtable.php** de MySQL tabel maken voor de toepassing en vervolgens bladert u naar **http://[site name].azurewebsites.net/index.php** voor de toepassing te gebruiken.
 
## <a name="next-steps"></a>Volgende stappen

Zie de [PHP Developer Center](/develop/php/)voor meer informatie.

[install-php]: http://www.php.net/manual/en/install.php
[install-mysql]: http://dev.mysql.com/doc/refman/5.6/en/installing.html
[pdo-mysql]: http://www.php.net/manual/en/ref.pdo-mysql.php
[localhost-createtable]: http://localhost/tasklist/createtable.php
[localhost-index]: http://localhost/tasklist/index.php
[running-app]: ./media/web-sites-php-mysql-deploy-use-ftp/running_app_2.png
[new-website]: ./media/web-sites-php-mysql-deploy-use-ftp/new_website2.png
[custom-create]: ./media/web-sites-php-mysql-deploy-use-ftp/create_web_mysql.png
[website-details]: ./media/web-sites-php-web-site-mysql-deploy-use-ftp/website_details.jpg
[new-mysql-db]: ./media/web-sites-php-mysql-deploy-use-ftp/create_db.png
[go-to-webapp]: ./media/web-sites-php-mysql-deploy-use-ftp/select_webapp.png
[set-deployment-credentials]: ./media/web-sites-php-mysql-deploy-use-ftp/set_credentials.png
[portal-ftp-username-password]: ./media/web-sites-php-mysql-deploy-use-ftp/save_credentials.png
[resource-group]: ./media/web-sites-php-mysql-deploy-use-ftp/set_group.png
[new-web-app]: ./media/web-sites-php-mysql-deploy-use-ftp/create_wa.png
[select-database]: ./media/web-sites-php-mysql-deploy-use-ftp/select_database.png
[select-resourcegroup]: ./media/web-sites-php-mysql-deploy-use-ftp/select_resourcegroup.png
[select-properties]: ./media/web-sites-php-mysql-deploy-use-ftp/select_properties.png
[note-properties]: ./media/web-sites-php-mysql-deploy-use-ftp/note-properties.png

[connection-string-info]: ./media/web-sites-php-web-site-mysql-deploy-use-ftp/connection_string_info.png
[management-portal]: https://portal.azure.com
[download-publish-profile]: ./media/web-sites-php-mysql-deploy-use-ftp/download_publish_profile_3.png
 
