
1. In Visual Studio Solution Explorer met de rechtermuisknop op de Windows Store-app-project, klikt u op **winkel** > **App koppelen met de winkel...**.

    ![App koppelen aan Windows Store](./media/app-service-mobile-register-wns/notification-hub-associate-win8-app.png)

2. In de wizard, klikt u op **volgende**, meld u aan met uw Microsoft-account, typ een naam voor uw app in **Reserve een nieuwe naam voor de toepassing**en klik vervolgens op **Reserve**.

3. Nadat de registratie app is gemaakt, selecteert u de naam van de nieuwe app, klik op **volgende**en klik op **koppelen**. Hiermee voegt u de vereiste Windows Store registratiegegevens toe aan manifest van de toepassing.

7. Herhaal stap 1 en 3 voor de Windows Phone Store app project met dezelfde registratie die u eerder hebt gemaakt voor de Windows Store-app.  

7. Ga naar de [Dev Center van Windows](https://dev.windows.com/en-us/overview)aanmelden met uw Microsoft-account, klik op de nieuwe inschrijving app in **Mijn apps**en vouw **Services** > **Push-meldingen**.

8. Op de pagina **Push-meldingen** klikt u op de **site van Live-Services** onder **Windows Push Notification Services (WNS) en Microsoft Azure mobiele Apps**en noteer de waarde van de **SID pakket** en de *huidige* waarde in het **Geheim van toepassing**. 

    ![App-instelling in het developer center](./media/app-service-mobile-register-wns/mobile-services-win8-app-push-auth.png)

    > [AZURE.IMPORTANT] Het geheim van de toepassing en de SID-pakket zijn belangrijke referenties. Deze waarden te delen met iedereen of ze distribueren met uw app niet.
