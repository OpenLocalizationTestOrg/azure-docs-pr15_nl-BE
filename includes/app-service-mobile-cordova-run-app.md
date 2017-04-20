
1. Ga naar de [Portal Azure]. Klik op **Alles zoeken** > **Mobile Apps** > de backend die u zojuist hebt gemaakt. Klik in de instellingen van de mobiele app **Quickstart** > **Cordova**. Onder **de clienttoepassing configureren**, selecteert u **een nieuwe toepassing maken**, klik op **downloaden**. Hiermee downloadt u een volledig project Cordova voor een app die vooraf is geconfigureerd voor verbinding met uw back-end.

2. Het gedownloade ZIP-bestand naar een map op de vaste schijf uitpakken, navigeer naar het oplossingsbestand (.sln) en openen met behulp van Visual Studio.

5. In Visual Studio solution-platform (Android, iOS of Windows) kiezen in de vervolgkeuzelijst naast de pijl start en selecteer vervolgens een specifieke Implementatieapparaat of een emulator door te klikken op de vervolgkeuzelijst op de groene pijl. Opmerking u kunt de standaard Android platform en Rimpel-emulator. Meer geavanceerde zelfstudies, moet u een ondersteund apparaat of emulator te selecteren. 

6. Druk op F5 of klik op de groene pijl om te bouwen en en voer uw app Cordova. Als er een dialoogvenster security in de emulator die toegang aanvraagt tot het netwerk, accepteren.   

7. Nadat het is gestart met de app op het apparaat of de emulator zinvolle tekst typt in de **nieuwe tekst invoeren**, zoals _de zelfstudie voltooien_ en klik vervolgens op de knop **toevoegen** .  
Dit verzendt een POST-aanvraag naar de Azure backend die u eerder hebt geïmplementeerd. De back-end voegt gegevens uit de aanvraag is in de tabel TodoItem in de SQL-Database en geeft als resultaat informatie over de zojuist opgeslagen items terug naar de mobiele app. Deze gegevens worden weergegeven in de lijst van de mobiele app.

    ![](./media/app-service-mobile-cordova-quickstart/quickstart-startup.png)
    
8. Herhaal de vorige drie stappen voor elk apparaatplatform dat die u wilt ondersteunen.

[Azure Portal]: https://portal.azure.com/
