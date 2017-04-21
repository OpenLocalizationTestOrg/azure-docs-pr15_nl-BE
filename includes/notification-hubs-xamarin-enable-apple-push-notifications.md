

Om te registreren de app voor push-meldingen via Apple Push Notification Service (APNS), moet u een nieuw certificaat van push, App-ID en provisioning profiel voor het project maken op de Apple developer-portal. De App-ID bevat de configuratie-instellingen waarmee u uw app voor het verzenden en ontvangen van push-meldingen. Deze instellingen vindt u de push notification certificaat verifiëren met Apple Push Notification Service (APNS) bij het verzenden en ontvangen van push-meldingen nodig. Zie de officiële [Apple Push Notification Service](http://go.microsoft.com/fwlink/p/?LinkId=272584) -documentatie voor meer informatie over deze concepten.


####<a name="generate-the-certificate-signing-request-file-for-the-push-certificate"></a>Genereer het bestand certificaat ondertekenen van aanvragen voor het push-certificaat

Deze stappen helpt u bij het maken van het certificaat ondertekenen van de aanvraag. Deze wordt gebruikt voor het genereren van een push-certificaat kan worden gebruikt bij APNS.

1. Op uw Macintosh-computer, voert u het hulpprogramma sleutelketen toegang. Het kan worden geopend vanuit de map **Utilities** of de **andere** map in het pad starten.

2. **Sleutelketen toegang**, vouw **-Assistent certificaat**, klik op **een certificaat aanvragen bij een Certificate Authority...**.

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-request-cert-from-ca.png)

3. Selecteer uw **E-mailadres gebruiker** en de **Algemene naam** , zorg ervoor dat **opgeslagen op de schijf** is geselecteerd en klik op **Doorgaan**. Laat het veld **CA e-mailadres** leeg is niet vereist.

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-csr-info.png)

4. Typ een naam voor het certificaat ondertekenen Request (CSR)-bestand in het dialoogvenster **OpslaanAls**, selecteer de locatie **waar**in en klik op **Opslaan**.

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-save-csr.png)

    Dit wordt de CSR-bestand opgeslagen op de geselecteerde locatie; de standaardlocatie is op het bureaublad. Vergeet niet de locatie gekozen voor dit bestand.


####<a name="register-your-app-for-push-notifications"></a>Uw app voor push-meldingen registreren

Maak een nieuwe App-ID expliciet voor uw toepassing met Apple en ook voor push-meldingen configureren.  

1. Ga naar de [iOS Portal creëren](http://go.microsoft.com/fwlink/p/?LinkId=272456) op de Apple Developer Center, meld u aan met uw Apple-ID **id's**, klikt u op en vervolgens klikt u op **App-id's**en ten slotte klikt u op de **+** voor het registreren van een nieuwe toepassing ondertekenen.

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-ios-appids.png)

2. De volgende drie velden voor uw nieuwe app bijwerken en klik op **Doorgaan**:

    * **Naam**: Typ een beschrijvende naam voor uw app in het veld **naam** in de sectie **Beschrijving van App-ID** .

    * **Bundel-id**: een **Bundel-id** in het formulier invoeren en onder de sectie **Expliciete App-ID** `<Organization Identifier>.<Product Name>` zoals vermeld in de [App Distribution Guide](https://developer.apple.com/library/mac/documentation/IDEs/Conceptual/AppDistributionGuide/ConfiguringYourApp/ConfiguringYourApp.html#//apple_ref/doc/uid/TP40012582-CH28-SW8). Deze moet overeenkomen met wat wordt ook gebruikt in het project XCode, Xamarin of Cordova voor uw app.

    * **Push-meldingen**: Schakel de optie **Push-meldingen** in de sectie **Services App** .

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-new-appid-info.png)

3.  Op het scherm-ID App bevestigen de instellingen bekijken en nadat u hebt gecontroleerd op ze **verzenden**

4.  Nadat u de nieuwe App-ID hebt ingediend, ziet u het **volledige** scherm. Klik op **Gereed**.

5. Zoek in het Developer Center, onder het App-id's, de app-ID die u zojuist hebt gemaakt en klik op een rij. Als u op de rij app-ID wordt de app details weer. Klik op de knop **bewerken** onder.

6. Ga naar de onderkant van het scherm en klikt u op de knop **Certificaat maken** in de sectie **SSL-certificaat met Push-ontwikkeling**.

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-appid-create-cert.png)

    De assistent 'IOS certificaat toevoegen' wordt weergegeven.

    > [AZURE.NOTE] In deze zelfstudie wordt een certificaat voor de ontwikkeling. Hetzelfde proces wordt gebruikt bij het registreren van een productiecertificaat. Zorg ervoor dat u hetzelfde certificaattype te gebruiken bij het verzenden van meldingen.

7. Klik op **Bestand kiezen**, blader naar de locatie waar u de medewerker van de Klantenservice voor uw push-certificaat opgeslagen. Klik op **genereren**.

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-appid-cert-choose-csr.png)

8. Nadat het certificaat door de portal is gemaakt, klikt u op de knop **downloaden** .

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-appid-download-cert.png)

    Dit downloadt het handtekeningcertificaat en opgeslagen op uw computer in de map Downloads.

    ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-cert-downloaded.png)

    > [AZURE.NOTE] Standaard het gedownloade bestand development certificaat met de naam **aps_development.cer**.

9. Dubbelklik op het gedownloade push certificaat **aps_development.cer**. Hiermee installeert u het nieuwe certificaat in de sleutelketen, zoals hieronder wordt weergegeven:

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-cert-in-keychain.png)

    > [AZURE.NOTE] De naam in het certificaat kan afwijken, maar zal worden voorafgegaan door **Development Apple iOS Push-Services:**.

10. In de sleutelketen toegang, met de rechtermuisknop op het nieuwe push-certificaat dat u hebt gemaakt in de categorie **certificaten** . Klik op **exporteren**, het bestand de naam, de **.p12** selecteren en klik vervolgens op **Opslaan**.

    Vergeet niet de naam en locatie van de geëxporteerde .p12 push-certificaat. Deze worden verificatie in te schakelen met APNS te uploaden naar de klassieke Portal Azure gebruikt.



####<a name="create-a-provisioning-profile-for-the-app"></a>Een provisioning profiel maken voor de app.

1. Terug in de <a href="http://go.microsoft.com/fwlink/p/?LinkId=272456" target="_blank">iOS Provisioning Portal</a> **Inrichten profielen**selecteren, selecteert u **alle**en klik vervolgens op de **+** om een nieuw profiel maken. Dit start de Wizard **toevoegen iOS Provisiong profiel**

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-new-provisioning-profile.png)

2. **IOS App Development** in **ontwikkeling** als de provisiong profiel selecteren en klik op **Doorgaan**.


3. Selecteer vervolgens de app-ID die u zojuist hebt gemaakt in de lijst **-ID App** , en klik op **Doorgaan**

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-select-appid-for-provisioning.png)


4. Selecteer uw ontwikkeling certificaat voor ondertekening van programmacode in het scherm **Selecteer certificaten** en klik op **Doorgaan**. Dit is een handtekeningcertificaat, niet het push-certificaat u zojuist hebt gemaakt.

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-provisioning-select-cert.png)


5. Vervolgens selecteert u de **apparaten** wilt gebruiken voor het testen en klik op **Doorgaan**

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-provisioning-select-devices.png)


6. Ten slotte, kies een naam voor het profiel in **De naam van het profiel**, klik op **genereren**.

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-provisioning-name-profile.png)
