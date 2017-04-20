

1. Op uw Mac, **Sleutelketen Access**te starten. Open **Mijn certificaten** onder **categorie** op de balk links navigationn. Het SSL-certificaat dat u in de vorige sectie hebt gedownload zoeken en de inhoud ervan vermelden. Alleen het certificaat selecteren (Selecteer de persoonlijke sleutel), en [deze te exporteren](https://support.apple.com/kb/PH20122?locale=en_US).

2. In de [portal Azure](https://portal.azure.com/), klikt u op **Alles zoeken** > **App Services** > uw backend Mobile App. Onder **Instellingen**op **Push-App Service**Klik op de naam van de hub van uw melding. Ga naar de **Apple Push Notification Services** > **certificaat uploaden**. Upload het bestand .p12, de juiste **modus** te selecteren (afhankelijk van of de SSL-client het certificaat van eerder is productie of Sandbox). Sla eventuele wijzigingen.

De service is nu geconfigureerd voor het werken met push-meldingen op iOS!

[1]: ./media/app-service-mobile-apns-configure-push/mobile-push-notification-hub.png
