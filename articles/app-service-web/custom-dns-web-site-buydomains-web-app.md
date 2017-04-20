<properties
    pageTitle="Hoe koopt u een aangepaste domeinnaam in Azure App Service Web Apps"
    description="Informatie over het kopen van een aangepaste domeinnaam met een web app in Azure App-Service."
    services="app-service\web"
    documentationCenter=""
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

# <a name="buy-and-configure-a-custom-domain-name-in-azure-app-service"></a>Kopen en een aangepaste domeinnaam in Azure App-Service configureren

[AZURE.INCLUDE [web-selector](../../includes/websites-custom-domain-selector.md)]

Bij het maken van een web app Azure wordt toegewezen aan een subdomein van azurewebsites.net. Als uw web app met de naam **contoso**, is de URL **contoso.azurewebsites.net**. Azure wordt ook een virtuele IP-adres toegewezen.

Voor een web app van productie wilt u waarschijnlijk gebruikers zien de naam van een aangepaste domein. In dit artikel wordt uitgelegd hoe u een aangepast domein configureren met [App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714)en koop. 

[AZURE.INCLUDE [introfooter](../../includes/custom-dns-web-site-intro-notes.md)]


## <a name="overview"></a>Overzicht

Als u een domeinnaam voor uw web app niet hebt, kunt u gemakkelijk op [Azure Portal](https://portal.azure.com/)kopen. Tijdens het aankoopproces kunt u WWW en de hoofdmap van het domein van DNS-records automatisch worden gekoppeld aan uw web app. U kunt ook uw domein recht in Azure Portal beheren.


Gebruik de volgende stappen te kopen domeinnamen toewijzen aan uw web app.

1. Open in de browser de [Azure-Portal](https://portal.azure.com/).

2. Klik op de naam van uw web app, selecteer **Instellingen**en selecteer **Custom domains** op het tabblad **Web Apps**

    ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-6.png)

3. Klik op **kopen domeinen**in de **Custom domains** -blade.

    ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-1.png)

4. In het blad **Kopen domeinen** gebruiken in het tekstvak typt u de naam van het domein dat u wilt kopen en druk op Enter. De voorgestelde beschikbare domeinen wordt direct onder het tekstvak weergegeven. Selecteer welke domein dat u wilt kopen. U kunt meerdere domeinen in één keer aanschaffen. 

  ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-2.png)

5. Klik op de **Contactgegevens** en het domein van contactgegevens formulier invullen.

  ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-3.png)

    > [AZURE.NOTE] Het is zeer belangrijk dat u alle vereiste velden met zo nauwkeurig mogelijk, met name het e-mailadres invullen. Bij de aanschaf van het domein zonder 'Privacy protection', wordt u mogelijk gevraagd om te controleren of uw e-mailadres voordat u het domein wordt geactiveerd. In sommige gevallen leidt onjuiste gegevens voor contactinformatie bij het kopen van domeinen. 

6. Nu kunt u,

    een) "automatisch verlengen" uw domein per jaar
    
    b) Opt-in voor "bescherming van Privacy' die is opgenomen in de aankoopprijs gratis (met uitzondering van de TLD's die het register niet ondersteuning voor Privacy. For example:. co.in,. co.uk, enz.)  
    
    c) "standaard hostnamen toewijzen" voor WWW en hoofddomein aan het huidige Web App. 

  ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-2.5.png)
  
    > [AZURE.NOTE] Optie C DNS-bindingen en Hostname bindingen automatisch voor u geconfigureerd.  Op deze manier uw Web App toegankelijk met aangepaste domein als de aankoop voltooid is (baring DNS-doorgifte vertragingen in enkele gevallen). In geval van, uw Web App achter Azure verkeer Manager, ziet u niet een optie voor het toewijzen van het hoofddomein, zoals A-Records niet met de Manager van verkeer werken. Altijd kunt u de domeinen/subformulier-domains aangeschaft via een Web App naar een andere Web App en vice versa. Zie stap 8 voor meer details. 
    
7. Klik op de **selecteren** op **Domeinen kopen** blade, ziet u de gegevens op de bladeserver **bevestiging kopen** . Als u akkoord met de juridische voorwaarden en klik op **kopen**, uw bestelling worden verzonden en kunt u het inkoopproces van **melding**controleren. Aankoop domein kan enkele minuten duren. 

  ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-4.png)

  ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-5.png)

8. Als u een domein heeft besteld, kunt u het domein beheren en toewijzen aan uw web app. Klik op de **"..."** aan de rechterkant van uw domein. Vervolgens kunt u de **aankoop te annuleren** of het **domein beheren**. Klik op **domein beheren**, we kunnen vervolgens **subdomein** binden aan onze web app op de bladeserver **domein beheren** . Voer deze stap uit in het kader van de respectieve Web App als u wilt een **subdomein** binden aan een ander Web App. Hier u een kiezen voor het toewijzen van het domein met verkeer manager eindpunt (als Web App achter TM) door simpelweg te selecteren verkeer manager een naam uit de vervolgkeuzelijst. Op deze manier domein/subdomein automatisch toegewezen aan het Web Apps achter dat eindpunt van het beheer van verkeer. 

    ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-6.png)

    > [AZURE.NOTE] U kunt 'annuleren aankoop"binnen 5 dagen voor volledige restitutie. Na 5 dagen kun je het niet mogelijk om te "Aankoop annuleren", in plaats daarvan ziet u een optie ' Verwijderen ' van het domein. Als u het domein leidt tot het vrijgeven van uw abonnement zonder restitutie en worden de beschikbare domein. 

Nadat de configuratie is voltooid, wordt de aangepaste naam worden vermeld in de sectie **Bindingen hostnaam** van uw web app.

Op dit punt moet u de aangepaste domeinnaam opgeven in uw browser en Zie dat deze met succes u naar uw web app gaat.
 
## <a name="what-happens-to-the-custom-domain-you-bought"></a>Wat gebeurt er met het aangepaste domein dat u hebt gekocht

Het aangepaste domein dat u hebt gekocht in de blade **Custom domains en SSL** is gekoppeld aan het abonnement Azure. Als een resource Azure is deze aangepaste domein afzonderlijke en onafhankelijke vanuit de App Service app dat u eerst het domein voor gekocht. Dit betekent dat:

- Binnen de Azure portal kunt u de aangepaste domein dat u hebt gekocht voor meer dan één App Service app en niet alleen voor de toepassing die u eerst het aangepaste domein voor gekocht. 
- U kunt de aangepaste domeinen die u hebt gekocht in de Azure abonnement via de **Custom domains- en SSL** -blade van *elke* App Service app in abonnement beheren.
- U kunt elk app App-Service uit de dezelfde Azure abonnement toewijzen aan een subdomein binnen dat domein aangepast.
- Als u een App Service app verwijderen, kunt u niet verwijderen van het aangepaste domein dat deze is gebonden aan als u wilt blijven gebruiken voor andere toepassingen.

## <a name="if-you-cant-see-the-custom-domain-you-bought"></a>Als u niet de aangepaste domein ziet u hebt gekocht

Als u de aangepaste domein vanuit de blade **Custom domains en SSL** hebt gekocht, maar niet het aangepaste domein **domeinen beheerd**, controleert u of de volgende zaken:

- Het maken van aangepaste domein kan niet hebt voltooid. Controleer de bell kennisgeving aan de bovenkant van de Azure portal voor de voortgang.
- Het maken van aangepaste domein kan om een bepaalde reden. Controleer de bell kennisgeving aan de bovenkant van de Azure portal voor de voortgang.
- Het aangepaste domein kan zijn geslaagd, maar de bladeserver kan niet worden vernieuwd. Probeer het opnieuw openen van de bladeserver **Custom domains en SSL** .
- Mogelijk is het aangepaste domein op een bepaald moment verwijderd. Controleer de controlelogboeken door te klikken op **Instellingen** > **Audit Logs** van uw app in de belangrijkste blade. 
- De **Custom domains- en SSL** -blade kijkt u in kan deel uitmaken van een app die gemaakt in een ander abonnement Azure. Overschakelen naar een andere toepassing in een ander abonnement en controleer de blade **Custom domains en SSL** .  
  Binnen de portal niet mogelijk om te bekijken of beheren van aangepaste domeinen in een ander Azure abonnement dan de app gemaakt. Echter, als u op **Geavanceerd beheer** in van het domein **beheren domein** blade, u zult worden omgeleid naar de domeinprovider website, waar u kunnen   [uw aangepaste domein zoals alle externe aangepaste domein handmatig configureren zult](web-sites-custom-domain-name.md)  
   voor toepassingen die zijn gemaakt in een ander abonnement Azure. 


