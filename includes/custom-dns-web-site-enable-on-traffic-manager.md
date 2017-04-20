Nadat de records voor uw domein hebt doorgegeven, moet u uw browser gebruiken om te controleren dat uw aangepaste domeinnaam kan worden gebruikt voor toegang tot uw web app in Azure App-Service.

> [AZURE.NOTE] Het kan even duren voor uw CNAME doorgeven via het DNS-systeem. Een service zoals <a href="http://www.digwebinterface.com/">http://www.digwebinterface.com/</a> kunt u controleren of de CNAME beschikbaar is.

Als u uw web app nog niet als een eindpunt verkeer Manager hebt toegevoegd, moet u dit doen voordat de naamomzetting werkt als de aangepaste domein naam routes naar verkeer Manager. Beheer van netwerkverkeer wordt vervolgens doorgestuurd naar uw web app. Gebruik de informatie [toevoegen of verwijderen van eindpunten](../articles/traffic-manager/traffic-manager-endpoints.md) in uw web app toevoegen als een eindpunt in uw serviceprofiel verkeer.

> [AZURE.NOTE] Als uw web app niet wordt vermeld bij het toevoegen van een eindpunt, controleert u of deze is geconfigureerd voor de **standaard** App Service plan modus. Om te kunnen werken met beheer van verkeer moet u **de standaardmodus voor uw web app** .

1. Open in de browser de [Azure-Portal](https://portal.azure.com).

1. Klik op de naam van uw web app, selecteer **Instellingen**en selecteer **Custom domains** op het tabblad **Web Apps**

    ![](./media/custom-dns-web-site/dncmntask-cname-6.png)

1. Klik op **Add host-naam**in de **Custom domains** -blade.
    
1. Gebruik de **hostnaam** tekstvakken voor het beheer van verkeer domeinnaam koppelen aan dit web app.

    ![](./media/custom-dns-web-site/dncmntask-cname-8.png)

1. Klik op **valideren** om de configuratie van domain name opslaan.

7.  Na het klikken op **valideren** Azure zal ere van domeinverificatie workflow. Dit wordt gecontroleerd op eigendom van domein als Hostname beschikbaarheid en rapport succes of gedetailleerde fout met duidelijke guidence voor het oplossen van de fout.    

8.  Na een succesvolle validatie **host-naam toevoegen** knop wordt actief wordt en is het mogelijk om de host-naam toewijzen. Ga naar uw aangepaste domeinnaam in een browser. U ziet nu de app uitgevoerd met behulp van uw aangepaste domeinnaam. 

    Nadat de configuratie is voltooid, wordt de aangepaste naam worden vermeld in de sectie **domeinnamen** van uw web app.

Op dit punt moet u kan de naam naam Manager verkeer invoeren in uw browser en Zie dat deze met succes u naar uw web app gaat.
