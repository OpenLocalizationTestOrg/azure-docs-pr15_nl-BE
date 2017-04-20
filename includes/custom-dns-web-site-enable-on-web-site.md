Nadat de records voor uw domein hebt doorgegeven, moet u deze koppelen aan uw Web App. Gebruik de volgende stappen uit om de domeinnamen in uw webbrowser.

> [AZURE.NOTE] Het kan even duren voor TXT-records die zijn gemaakt in de vorige stappen doorgeven via het DNS-systeem. U kan de naam van het domein toevoegen aan uw web app totdat de TXT-record heeft doorgegeven. Als u een A-record, kunt u de domeinnaam van een record toevoegen aan uw web app totdat de TXT-record gemaakt in de vorige stap is doorgegeven.
>
> Een service zoals <a href="http://www.digwebinterface.com/">http://www.digwebinterface.com/</a> kunt u controleren of de TXT-record beschikbaar is.

1. Open in de browser de [Azure-Portal](https://portal.azure.com).

2. Klik op de naam van uw web app en selecteer **Custom domains** op het tabblad **Web Apps**

    ![](./media/custom-dns-web-site/dncmntask-cname-6.png)

3. Klik op **Add host-naam**in de **Custom domains** -blade.
    
4. Gebruik de **hostnaam** tekstvakken voor de domeinnamen koppelen aan dit web app.

    ![](./media/custom-dns-web-site/add-custom-domain.png)

6.  Klik op **valideren**.

7.  Na het klikken op **valideren** Azure zal ere van domeinverificatie workflow. Dit wordt gecontroleerd op eigendom van domein als Hostname beschikbaarheid en rapport succes of gedetailleerde fout met duidelijke guidence voor het oplossen van de fout.    

Op dit punt moet u de aangepaste domeinnaam opgeven in uw browser en Zie dat deze met succes u naar uw web app gaat.
