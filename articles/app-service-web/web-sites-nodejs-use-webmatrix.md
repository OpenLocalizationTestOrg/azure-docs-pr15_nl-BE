<properties 
    pageTitle="Bouwen en implementeren van een Node.js web app met Azure met WebMatrix" 
    description="Een zelfstudie u hoe u WebMatrix gebruiken leert om een toepassing Node.js ontwikkelen en deze implementeren in Azure App Service Web Apps." 
    services="app-service\web" 
    documentationCenter="nodejs" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="08/11/2016"
    ms.author="robmcm"/>


# <a name="build-and-deploy-a-nodejs-web-app-to-azure-using-webmatrix"></a>Bouwen en implementeren van een Node.js web app met Azure met WebMatrix

In deze zelfstudie wordt beschreven hoe u WebMatrix gebruiken om een toepassing Node.js ontwikkelen en deze implementeren in [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) Web Apps. WebMatrix is een gratis hulpprogramma van Microsoft die alles wat die u nodig hebt voor de ontwikkeling van website of web app. WebMatrix bevat verschillende functies waarmee u eenvoudig te gebruiken Node.js, met inbegrip van de code is voltooid, vooraf gemaakte sjablonen en editor ondersteuning voor Jade, minder en CoffeeScript. Meer informatie over [WebMatrix](https://www.microsoft.com/web/webmatrix/).

Bij de voltooiing van deze handleiding, hebt u een Node.js web app in Azure App-Service wordt uitgevoerd.
 
Een schermafbeelding van de voltooide toepassing lager is dan:

![Azure knooppunt website][webmatrix-node-completed]

[AZURE.INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

>[AZURE.NOTE] Als u wilt aan de slag met Azure App-Service voordat u aanmelden voor een account met Azure, gaat u naar de [App-Service probeert](http://go.microsoft.com/fwlink/?LinkId=523751), waar u direct een starter tijdelijk web app in de App-Service maken kunt. Geen creditcard vereist; geen verplichtingen.

## <a name="sign-into-azure"></a>Log in Azure

Volg deze stappen voor het maken van een web app in Azure App-Service.

1. WebMatrix starten
2. Als dit de eerste keer dat hebt u gebruikt WebMatrix, wordt u gevraagd om te ondertekenen in Azure.  U kunt in alle andere gevallen klikt u op de knop **Aanmelden** en kies **Rekening toevoegen**.  **Log in** met je Microsoft-Account selecteren.

    ![Account toevoegen][addaccount]

3. Als u zich hebt aangemeld voor een account met Azure, kan u zich aanmelden met uw Microsoft-Account:

    ![Log in Azure][signin]  


## <a name="create-a-site-using-a-built-in-template-for-azure"></a>Maak een site met behulp van een ingebouwde sjabloon voor Azure

1. Klik op de knop **Nieuw** op het startscherm en kies **Galerie met sitesjablonen** voor het maken van een nieuwe site uit de galerie met lijstsjablonen:

    ![Nieuwe site in de galerie met sitesjablonen][sitefromtemplate]

2. Klik in het dialoogvenster **locatie van sjabloon** **knooppunt** selecteren en selecteer **Site Express**. Klik ten slotte op **volgende**. Als u de vereisten voor de sitesjabloon **Express** ontbreekt, wordt u gevraagd deze te installeren.

    ![Selecteer sjabloon express][webmatrix-templates]

3. Als u bent aangemeld in Azure, hebt u nu de optie voor het maken van een App Service web app voor uw lokale site.  Kies een unieke naam en selecteer het datacenter waar u uw web app in App-Service dat wilt moet worden gemaakt: 

    ![-Site maken op Azure][nodesitefromtemplateazure]
    
4. Nadat WebMatrix is het bouwen van de lokale site en het maken van de web App Service app, wordt de IDE WebMatrix weergegeven.

    ![webmatrix ide][webmatrix-ide]

##<a name="publish-your-application-to-azure"></a>Publiceer de toepassing naar Azure

1. In WebMatrix, klik op **publiceren** in het lint **Start** om het dialoogvenster **Voorvertoning publiceren** voor de site weer te geven.

    ![Voorvertoning publiceren][webmatrix-node-publishpreview]

2. Klik op **Doorgaan**. Wanneer het publiceren voltooid is, wordt de URL van de web App Service app weergegeven onder aan de WebMatrix IDE

    ![publiceren voltooid][webmatrix-publish-complete]

3. Klik op de koppeling om de App Service web app te openen in de browser.

    ![Express web app.][webmatrix-node-express-site]

##<a name="modify-and-republish-your-application"></a>Wijzigen en opnieuw publiceren van uw toepassing

U kunt gemakkelijk wijzigen en opnieuw publiceren van uw toepassing. U maakt hier een eenvoudig naar de kop in in het bestand **index.jade** wijzigen en opnieuw publiceren van de toepassing.

1. Selecteer **bestanden**in WebMatrix, en vouw vervolgens de map **weergaven** . Open het bestand **index.jade** door erop te dubbelklikken.

    ![webmatrix weergeven index.jade][webmatrix-modify-index]

2. De regel van de alinea als volgt wijzigen:

        p Welcome to #{title} with WebMatrix on Azure!

3. Sla uw wijzigingen en klik vervolgens op het pictogram publiceren. Ten slotte klikt u op **Doorgaan** in het dialoogvenster **Voorvertoning publiceren** en wachten op de update moet worden gepubliceerd.

    ![Voorvertoning publiceren][webmatrix-republish]

4. Bij het publiceren is voltooid, gebruikt u de koppeling die wordt geretourneerd als het publiceren voltooid is voor een overzicht van de bijgewerkte App Service web app.

    ![Azure knooppunt web app.][webmatrix-node-completed]

##<a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de versies van Node.js die met Azure en het opgeven van de versie kan worden gebruikt bij de toepassing worden geleverd, de [geven een Node.js versie in een toepassing Azure](../nodejs-specify-node-version-azure-apps.md).

Als u problemen met uw toepassing ondervindt nadat deze is geïmplementeerd in Azure, Zie [fouten opsporen in Azure App Service van een Node.js web app](web-sites-nodejs-debug.md) voor meer informatie over het oplossen van het probleem.

## <a name="whats-changed"></a>Wat er veranderd
* Zie voor een handleiding voor het wijzigen van Websites met App-Service: [Azure App Service en de Impact op de bestaande Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)

[WebMatrix WebSite]: http://www.microsoft.com/click/services/Redirect2.ashx?CR_CC=200106398
[WebMatrix for Azure]: http://go.microsoft.com/fwlink/?LinkID=253622&clcid=0x409

[webmatrix-node-completed]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-node-complete.png
[webmatrix-templates]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-templates.png

[webmatrix-node-publishpreview]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-publishpreview.png

[webmatrix-ide]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-ide.png
[webmatrix-publish-complete]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-publish-complete.png
[webmatrix-node-express-site]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-express-webiste.png
[webmatrix-modify-index]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-node-edit.png
[webmatrix-republish]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-republish.png
[addaccount]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-add-account.png
[signin]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-sign-in.png
[sitefromtemplate]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-site-from-template.png
[nodesitefromtemplateazure]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-node-site-azure.png
 