
1. Ga naar de [Portal Azure]. Klik op **Alles zoeken** > **Mobile Apps** > de backend die u zojuist hebt gemaakt. Klik in de instellingen van de mobiele app **Quickstart** > **Android)**. **De clienttoepassing configureren**, klik op **downloaden**. Hiermee downloadt u een volledige Android project voor een app die vooraf is geconfigureerd voor verbinding met uw back-end. 

2. Open het project met behulp van **Android Studio**, met behulp van **project importeren (Eclips ADT, Gradle, enz.)**. Zorg ervoor dat u deze selectie importeren om te voorkomen dat eventuele fouten JDK maakt.

3. Druk op de knop **'app' uitvoeren** voor het bouwen van het project en de app in de Android simulator start.

4. In de app, typ zinvolle tekst, zoals _de zelfstudie voltooien_ en klik vervolgens op de knop 'Toevoegen'. Dit verzendt een POST-aanvraag naar de Azure backend die u eerder hebt geïmplementeerd. Voegt gegevens in de back-end uit de aanvraag is in de TodoItem SQL-tabel en geeft als resultaat informatie over de zojuist opgeslagen items terug naar de mobiele app. Deze gegevens worden weergegeven in de lijst van de mobiele app. 

    ![](./media/app-service-mobile-android-quickstart/mobile-quickstart-startup-android.png)

[Azure Portal]: https://portal.azure.com/
