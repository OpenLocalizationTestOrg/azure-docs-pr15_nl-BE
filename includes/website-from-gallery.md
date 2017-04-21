De markt Azure beschikbaar voor tal van populaire web apps ontwikkeld door Microsoft, andere bedrijven en initiatieven van open source software. Installatie van software dan de browser verbinding maken met de [Azure Preview Portal](http://go.microsoft.com/fwlink/?LinkId=529715)gebruikt nodig web apps gemaakt op basis van de markt Azure niet. 

In deze zelfstudie leert u hoe:

- Het maken van een nieuwe web-app via de markt Azure.

- Het implementeren van de web-app via de Portal Azure voorbeeld.
 
U zult een WordPress blog die gebruikmaakt van een standaardsjabloon maken. In de volgende afbeelding ziet u de voltooide toepassing:


![WordPress blog][13]

>[AZURE.NOTE] Als u wilt aan de slag met Azure App-Service voordat u aanmelden voor een account met Azure, gaat u naar de [App-Service probeert](http://go.microsoft.com/fwlink/?LinkId=523751), waar u direct een starter tijdelijk web app in de App-Service maken kunt. Geen creditcard vereist; geen verplichtingen.

## <a name="create-a-web-app-in-the-portal"></a>Een webtoepassing maken in de portal

1. Log in op de voorvertoning Azure-Portal.

2. De markt Azure openen door te klikken op het pictogram van de **markt** :

    ![Marketplace-pictogram][marketplace]

    Of door te klikken op het pictogram **Nieuw** op de rechterbovenhoek van het dashboard en **Marketplace** op de bottow van de lijst te selecteren.
    
    ![Nieuw maken][5]
    
3. Selecteer **Web + Mobile**. Zoeken naar **WordPress** en klik op het pictogram **WordPress** .

    ![WordPress uit lijst][7]
    
5. Selecteer na het lezen van de beschrijving van de WordPress app **maken**.

6. Klik op **Web app**en de vereiste waarden leveren voor het configureren van uw web app.
    
    ![Configureer uw app][8]

7. Klik op **Database**en bevatten de vereiste waarden voor het configureren van de MySQL-database. 

    ![database configureren][database]

8. Geef een naam voor een nieuwe resourcegroep.

    ![Resourcegroep instellen][groupname]

8. Indien nodig, **abonnement**op en geeft u het abonnement wilt gebruiken. 

7. Wanneer u klaar bent met het definiëren van de web app, klikt u op **maken**en is bezig met de nieuwe web app is gemaakt.

   Als de toepassing is gemaakt, ziet u de bronnengroep met web app en de database.

   ![groep weergeven][resourcegroup]

## <a name="launch-and-manage-your-wordpress-web-app"></a>Opzetten en beheren van uw WordPress web app
    
1. Klik op het nieuwe web app voor informatie over uw app.

    ![dashboard starten][10]

2. Klik op de pagina **Essentials** **Bladeren** of de koppeling onder **Url** voor het openen van de web-app welkomstpagina.

    ![URL van de site][browse]

3. Voer de juiste configuratiegegevens vereist door WordPress WordPress niet is geïnstalleerd, en klik op **WordPress installeren** om de configuratie te voltooien en de web-app login pagina openen.

4. Klik op **aanmelden** en voer uw referenties.  

5. U hebt een nieuwe WordPress web app dat op de onderstaande web app lijkt.    

    ![uw WordPress site][13]






[5]: ./media/website-from-gallery/start-marketplace.png
[6]: ./media/website-from-gallery/wordpressgallery-02.png
[7]: ./media/website-from-gallery/search-web-app.png
[8]: ./media/website-from-gallery/set-web-app.png
[9]: ./media/website-from-gallery/wordpressgallery-05.png
[10]: ./media/website-from-gallery/select-web.png
[13]: ./media/website-from-gallery/wordpressgallery-09.png
[webapps]: ./media/website-from-gallery/selectwebapps.png
[database]: ./media/website-from-gallery/set-db.png
[resourcegroup]: ./media/website-from-gallery/show-rg.png
[browse]: ./media/website-from-gallery/browse-web.png
[marketplace]: ./media/website-from-gallery/marketplace-icon.png
[groupname]: ./media/website-from-gallery/set-rg.png
