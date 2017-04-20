<properties
    pageTitle="Controle twee stappen voor mijn werk of school-account instellen"
    description="Wanneer uw bedrijf Azure meerledige verificatie configureert, wordt u gevraagd om aan te melden voor verificatie in twee stappen. Informatie over hoe u instelt. "
    services="multi-factor-authentication"
    keywords="het gebruik van azure directory, active directory in de cloud, active directory-zelfstudie"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="pblachar"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="kgremban"/>

# <a name="set-up-my-account-for-two-step-verification"></a>Mijn account voor verificatie in twee stappen instellen

Verificatie in twee stappen is een extra beveiliging stap die helpt bij het beschermen van uw account door waardoor het moeilijker voor andere mensen om op te splitsen. Als u dit artikel leest, hebt u waarschijnlijk een e-mailbericht van uw werk of school beheerder over meerledige verificatie. Of misschien probeert aan te melden en krijg een bericht waarin u wordt gevraagd om extra beveiliging controle in te stellen. Als dat het geval is, **dat u kunt zich niet aanmelden totdat u het proces voor automatische inschrijving hebt voltooid**.

In dit artikel kunt u uw **werk of school account**instellen. Zie [verificatie in twee stappen](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification)als u controle van twee stappen voor uw eigen, persoonlijke Microsoft-account inschakelen.

## <a name="determine-how-you-will-use-multi-factor-authentication"></a>Bepalen hoe u meerledige verificatie wordt gebruikt

Verificatie in twee stappen werkt door het vragen voor twee stuks van identificatie wanneer u zich aanmeldt. Eerst vragen we om uw gebruikersnaam en wachtwoord zoals gebruikelijk. Vervolgens bevestigen we contact een telefoon die we weten en u behoort dat de aanmeldingspoging gerechtvaardigd was.  

Om te beginnen met het setup-proces probeert aan te melden bij uw account zoals u gewoonlijk doet. Als uw beheerder heeft uw account voor verificatie in twee stappen worden geconfigureerd, wordt u gevraagd om te beginnen met automatische inschrijving. Dit proces begint door te klikken op **het nu instellen.**

![Setup](./media/multi-factor-authentication-end-user-first-time/first.png)

De eerste vraag in het inschrijvingsproces is hoe u wilt dat wij contact met u opnemen. Bekijk de opties in de tabel en de koppelingen naar de installatiestappen voor elke methode te gaan gebruiken.

| Neem contact op met de methode | Beschrijving |
| --- | --- |
[Mobiele app](#use-a-mobile-app-as-the-contact-method) | - **Meldingen ontvangen voor verificatie.** Deze optie geeft een melding authenticator App op uw smartphone of tablet. De melding weergeven en selecteer **verifiëren** als rechtmatig is in de app. Uw werk of op school verlangen dat u een PINCODE invoeren voordat u verifiëren.<br>- **Gebruik de verificatiecode in.** De authenticator app genereert in deze modus een verificatiecode die elke 30 seconden bijgewerkt. De meest recente verificatiecode invoeren in de interface-in.<br>De Microsoft Authenticator app is beschikbaar voor [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072)en [IOS](http://go.microsoft.com/fwlink/?Linkid=825073). |
[Mobiele telefoongesprek of tekst](#use-your-mobile-phone-as-the-contact-method) | - **Telefoongesprek** plaatst een geautomatiseerd telefoongesprek op het telefoonnummer dat u opgeeft. De oproep wordt beantwoord en druk op # in het toetsenblok van de telefoon te verifiëren.<br>- **Tekstbericht** eindigt een tekstbericht met een verificatiecode. Beantwoord het tekstbericht of Voer de verificatiecode in die in de interface-in na de prompt in de tekst. |  
[Office, telefoongesprek](#use-your-office-phone-as-the-contact-method) | Hiermee plaatst u een geautomatiseerd telefoongesprek op het telefoonnummer dat u opgeeft. De oproep wordt beantwoord en op # drukt in het toetsenblok van de telefoon te verifiëren. |

## <a name="use-a-mobile-app-as-the-contact-method"></a>Een mobiele app gebruiken als de methode van het contact

Met deze methode moet u een authenticator app installeren op uw telefoon of tablet. De stappen in dit artikel zijn gebaseerd op de Microsoft-Authenticator app, die beschikbaar voor [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072)en [IOS is](http://go.microsoft.com/fwlink/?Linkid=825073).

1. **Mobiele app** selecteren in de vervolgkeuzelijst.
2. Selecteer **ontvangen van meldingen voor verificatie** of **verificatie-code gebruiken**en vervolgens **instellen**.

    ![Extra beveiliging controle scherm](./media/multi-factor-authentication-end-user-first-time-mobile-app/mobileapp.png)

3. Open de app op je telefoon of tablet en selecteer **+** een account toe te voegen. (Selecteer de drie puntjes, klik vervolgens op **account toevoegen**op Android-apparaten).
4. Opgeven dat u wilt toevoegen een account voor werk of school. Hiermee opent u de scanner QR-code op uw telefoon. Als uw camera niet goed werkt, kunt u uw bedrijfsgegevens handmatig invoeren. Zie [handmatig een account toevoegen](#add-an-account-manually)voor meer informatie.  
5. Scan de QR Codeafbeelding dat werd weergegeven met het scherm voor het configureren van de mobiele app.  Selecteer **gedaan** de QR-code scherm sluiten.  

    ![QR-code scherm](./media/multi-factor-authentication-end-user-first-time-mobile-app/scan2.png)

6. Wanneer de activering is voltooid op de telefoon, selecteert u **Contact met mij opnemen**.  Deze stap stuurt een melding of een verificatiecode naar uw telefoon. Selecteer **controleren**.  
7. Als uw bedrijf is een PINCODE vereist voor het goedkeuren van de controle-in, voert u deze.

    ![Vak voor het invoeren van een PINCODE](./media/multi-factor-authentication-end-user-first-time-mobile-app/scan3.png)

8. Na voltooiing van de PIN-code-item **sluiten**selecteren. De controle moet op dit moment werken.
9. Wij raden aan dat u uw gsm-nummer invoert in het geval u geen toegang meer tot uw mobiele app. Geef uw land uit de vervolgkeuzelijst en voer je mobiele telefoonnummer in het vak naast de landnaam van het. Selecteer **volgende**.
10. Op dit punt moet u voor het instellen van wachtwoorden voor browser-toepassingen zoals Outlook 2010 of ouder app of de app eigen e-mailadres op Apple-apparaten. Dit komt omdat sommige apps bieden geen ondersteuning voor verificatie in twee stappen. Als u deze apps niet gebruikt, klikt u op **Gereed** en de rest van de stappen overslaan.
11. Als u deze apps is het app-wachtwoord kopiëren geleverd en plak deze in uw toepassing in plaats van uw wachtwoord regelmatig. U kunt hetzelfde wachtwoord app voor meerdere toepassingen. Voor meer info [help met wachtwoorden app].
12. Klik op **Gereed**.


### <a name="add-an-account-manually"></a>Handmatig een account toevoegen
Ga als volgt te werk als u een account handmatig toevoegen aan de mobiele app, in plaats van de QR-reader.

1. Selecteer de **account handmatig invoeren** .  
2. Voer de code en de URL die beschikbaar zijn op dezelfde pagina waarin u de streepjescode. Deze informatie gaat in de vakken **en de **URL** ** op de mobiele app.

    ![Setup](./media/multi-factor-authentication-end-user-first-time-mobile-app/barcode2.png)

3. Wanneer de activering is voltooid, selecteert u **Contact met mij opnemen**. Deze stap stuurt een melding of een verificatiecode naar uw telefoon. Selecteer **controleren**.

## <a name="use-your-mobile-phone-as-the-contact-method"></a>Uw mobiele telefoon gebruiken als de methode van het contact

1. **Telefonische verificatie** selecteren uit de vervolgkeuzelijst.  

    ![Setup](./media/multi-factor-authentication-end-user-first-time-mobile-phone/phone.png)  

2. Kies uw land in de vervolgkeuzelijst en voer het nummer van uw mobiele telefoon.
3. Selecteer de methode die u wilt gebruiken met uw mobiele telefoon - tekst of een oproep.
4. Selecteer **Contact met mij opnemen** om te controleren of uw telefoonnummer. Afhankelijk van de geselecteerde modus, wij sturen u een tekst of gebeld worden. Volg de instructies op het scherm en selecteer vervolgens **controleren**.
5. Op dit punt moet u voor het instellen van wachtwoorden voor browser-toepassingen zoals Outlook 2010 of ouder app of de app eigen e-mailadres op Apple-apparaten. Dit komt omdat sommige apps bieden geen ondersteuning voor verificatie in twee stappen. Als u deze apps niet gebruikt, klikt u op **Gereed** en de rest van de stappen overslaan.
6. Als u deze apps is het app-wachtwoord kopiëren geleverd en plak deze in uw toepassing in plaats van uw wachtwoord regelmatig. U kunt hetzelfde wachtwoord app voor meerdere toepassingen. Voor meer info [help met wachtwoorden app].
7. Klik op **Gereed**.

## <a name="use-your-office-phone-as-the-contact-method"></a>Uw zakelijke telefoon gebruiken als de methode van het contact

1. **Telefoon (werk)** selecteert in de vervolgkeuzelijst  

    ![Setup](./media/multi-factor-authentication-end-user-first-time-office-phone/office.png)  

2. Het vak telefoonnummer wordt automatisch gevuld met de contactgegevens van uw bedrijf. Als het nummer onjuist of ontbreekt is, door de systeembeheerder te wijzigen.
4. Selecteer **Contact met mij opnemen** om te controleren of uw telefoonnummer in en we bellen je nummer. Volg de instructies op het scherm en selecteer vervolgens **controleren**.
5. Op dit punt moet u voor het instellen van wachtwoorden voor browser-toepassingen zoals Outlook 2010 of ouder app of de app eigen e-mailadres op Apple-apparaten. Dit komt omdat sommige apps bieden geen ondersteuning voor verificatie in twee stappen. Als u deze apps niet gebruikt, klikt u op **Gereed** en de rest van de stappen overslaan.
6. Als u deze apps is het app-wachtwoord kopiëren geleverd en plak deze in uw toepassing in plaats van uw wachtwoord regelmatig. U kunt hetzelfde wachtwoord app voor meerdere toepassingen. Zie [Wat zijn App wachtwoorden](multi-factor-authentication-end-user-app-passwords.md)voor meer informatie.
7. Klik op **Gereed**.

## <a name="next-steps"></a>Volgende stappen

- Wijzig de gewenste opties en [beheren van uw instellingen voor verificatie in twee stappen](multi-factor-authentication-end-user-manage-settings.md)
- [App wachtwoorden](multi-factor-authentication-end-user-app-passwords.md) voor apparaat native apps die geen ondersteuning voor verificatie in twee stappen bieden instellen.
- De [verificator Microsoft app](multi-factor-authentication-microsoft-authenticator.md) voor snelle en veilige verificatie uitchecken, zelfs als er geen cel service.
