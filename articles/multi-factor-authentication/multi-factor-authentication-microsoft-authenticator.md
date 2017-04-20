<properties
    pageTitle="Microsoft Authenticator app voor mobiele telefoons | Microsoft Azure"
    description="Informatie over het upgraden naar de nieuwste versie van de verificator Azure."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtland"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="kgremban"/>

# <a name="microsoft-authenticator"></a>Microsoft-verificator

De Microsoft Authenticator app biedt een extra niveau van beveiliging in uw account Azure (bijvoorbeeld bsimon@contoso.onmicrosoft.com), uw locatie op account werken (bijvoorbeeld, bsimon@contoso.com), of uw Microsoft-account (bijvoorbeeld bsimon@outlook.com).

De app werkt op twee manieren:

- **Melding**. De app kunt u onbevoegde toegang tot accounts voorkomen en frauduleuze transacties beëindigen door een melding naar uw smartphone of tablet te duwen. Gewoon de melding weergeven en als legitieme, selecteer **verifiëren**. Selecteer anders **weigeren**. Zie voor informatie over het weigeren van meldingen, weigeren en fraude functie voor meerledige verificatie gebruiken.

- **Wachtwoord met een verificatiecode**. De app kan worden gebruikt als een software-token te genereren een verificatiecode OAuth. U voert de code die door de app in het aanmeldingsscherm, samen met de gebruikersnaam en wachtwoord wanneer daarom wordt gevraagd. De verificatiecode die hier een tweede vorm van verificatie.

Met de release van de Microsoft-Authenticator app, wordt de oude Azure Authenticator app vervangen.  De verificator Azure app blijft werken, maar als u besluit om te verplaatsen naar de nieuwe Microsoft Authenticator app, dit artikel kan u helpen.  

## <a name="install-the-app"></a>De app installeren

De Microsoft Authenticator app is beschikbaar voor [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072)en [IOS](http://go.microsoft.com/fwlink/?Linkid=825073).

## <a name="add-accounts-to-the-app"></a>Accounts toevoegen aan de app.

Voor elke rekening die u wilt toevoegen aan de Microsoft Authenticator app, gebruikt u een van de volgende procedures.

### <a name="add-an-account-to-the-app-by-using-the-qr-code-scanner"></a>Een account toevoegen aan de app via de QR-code scanner

1. Ga naar het scherm beveiliging verificatie-instellingen.  Zie [de beveiligingsinstellingen wijzigen](multi-factor-authentication-end-user-manage-settings.md)voor meer informatie over hoe u naar dit scherm.

2. Selecteer **configureren**.

    ![De knop configureren op het scherm beveiliging verificatie-instellingen](./media/multi-factor-authentication-azure-authenticator/azureauthe.png)

    Verschijnt een scherm met een QR-code erop.

    ![Scherm waarmee de QR-code](./media/multi-factor-authentication-azure-authenticator/barcode2.png)

3. Open de Microsoft Authenticator app. Selecteer op het scherm van de **rekeningen** , **+**, en vervolgens opgeven dat u wilt toevoegen een account voor werk of school.

    ![Het scherm van de rekeningen met een plusteken (+)](./media/multi-factor-authentication-azure-authenticator/addaccount3.png)

    ![Scherm voor het opgeven van een werk of school account](./media/multi-factor-authentication-end-user-first-time-mobile-app/scan.png)

4. De camera voor het scannen van de QR-code te gebruiken en selecteer vervolgens **klaar** om af te sluiten, het scherm QR-code.

    ![Scherm voor het scannen van een QR-code](./media/multi-factor-authentication-end-user-first-time-mobile-app/scan2.png)

    Als uw camera niet goed werkt, kunt u de QR-code en de URL handmatig invoeren. Zie [een account met de app handmatig toevoegen](#add-an-account-to-the-app-manually)voor meer informatie.

5. Wacht tot de account is geactiveerd. Wanneer de activering is voltooid, selecteert u **Contact met mij opnemen**.  Dit stuurt een melding of een verificatiecode naar uw telefoon.  Selecteer **controleren**.

    ![Scherm waar u controleren of aan te melden](./media/multi-factor-authentication-end-user-first-time-mobile-app/verify.png)

6. Als uw bedrijf is een PINCODE vereist voor het goedkeuren van de controle-in, voert u deze.

    ![Vak voor het invoeren van een PINCODE](./media/multi-factor-authentication-end-user-first-time-mobile-app/scan3.png)

7. Na voltooiing van de PIN-code-item **sluiten**selecteren. De controle moet op dit moment werken.
8. Wij raden aan dat u uw gsm-nummer invoert in het geval u geen toegang meer tot uw app. Geef uw land uit de vervolgkeuzelijst en voer je mobiele telefoonnummer in het vak naast de landnaam van het. Selecteer **volgende**.
9. Op dit punt hebt u uw methode van contact instellen. Nu is het tijd voor het instellen van wachtwoorden voor browser-toepassingen, zoals Outlook 2010 of ouder app. Als u deze apps niet gebruikt, selecteert u **gedaan**. Ga anders verder met de volgende stap.

    ![Scherm voor het maken van een app wachtwoord](./media/multi-factor-authentication-end-user-first-time-mobile-app/step4.png)

10. Als u browser-apps, kopiëren van het wachtwoord voor de opgegeven toepassing en het wachtwoord in uw toepassingen te plakken. Zie voor stapsgewijze instructies voor afzonderlijke toepassingen zoals Outlook en Lync, het wijzigen van het wachtwoord in je e-mailadres in het app-wachtwoord en het wachtwoord in uw toepassing het app-wachtwoord wijzigen.
11. Selecteer **gedaan**.

De nieuwe account wordt nu weergegeven op het scherm van de **rekeningen** .

![Scherm van rekeningen](./media/multi-factor-authentication-azure-authenticator/accounts.png)

### <a name="add-an-account-to-the-app-manually"></a>Handmatig een account toevoegen aan de app.

1. Ga naar het scherm beveiliging verificatie-instellingen.  Zie [de beveiligingsinstellingen wijzigen](multi-factor-authentication-end-user-manage-settings.md)voor meer informatie over hoe u naar dit scherm.

2. Selecteer **configureren**.

    ![De knop configureren op het scherm beveiliging verificatie-instellingen](./media/multi-factor-authentication-azure-authenticator/azureauthe.png)

    Verschijnt een scherm met een QR-code erop.  Opmerking de code en de URL.

    ![Scherm met QR-code en URL](./media/multi-factor-authentication-azure-authenticator/barcode2.png)

3. Open de Microsoft Authenticator app. Selecteer op het scherm van de **rekeningen** , **+**, en vervolgens opgeven dat u wilt toevoegen een account voor werk of school.

    ![Het scherm van de rekeningen met een plusteken (+)](./media/multi-factor-authentication-azure-authenticator/addaccount3.png)

    ![Scherm voor het opgeven van een werk of school account](./media/multi-factor-authentication-end-user-first-time-mobile-app/scan.png)

4. In de scanner, selecteert u de **code handmatig invoeren**.

    ![Scherm voor het scannen van een QR-code](./media/multi-factor-authentication-end-user-first-time-mobile-app/scan2.png)

5. Voer de code en de URL in de juiste vakken in de app.

    ![Scherm voor het invoeren van code en URL](./media/multi-factor-authentication-azure-authenticator/manual.png)

    ![Scherm voor het invoeren van code en URL](./media/multi-factor-authentication-end-user-first-time-mobile-app/addaccount2.png)

6. Wacht tot de account is geactiveerd. Wanneer de activering is voltooid, selecteert u **Contact met mij opnemen**. Dit stuurt een melding of een verificatiecode naar uw telefoon. Selecteer **controleren**.

De nieuwe account wordt nu weergegeven op het scherm van de **rekeningen** .

![Scherm van rekeningen](./media/multi-factor-authentication-azure-authenticator/accounts.png)

### <a name="add-an-account-to-the-app-by-using-touch-id"></a>Een account toevoegen aan de app met Touch ID

De Microsoft Authenticator app op iOS ondersteunt Touch-ID.  Azure meerledige verificatie kunnen organisaties een PIN nodig voor apparaten. Met Touch ID iOS gebruikers hoeven niet te een pincode. In plaats daarvan kunnen hun vingerafdruk scannen en selecteer **goedkeuren**.

Instellen van een Touch-ID met Microsoft Authenticator is eenvoudig. Voltooi de uitdaging van een normale verificatie met een PINCODE. Als uw apparaat Touch ID ondersteunt, wordt Microsoft Authenticator deze automatisch ingesteld voor die account.

![Controle van de Touch-ID instellen](./media/multi-factor-authentication-azure-authenticator/touchid1.png)

Die wijst naar voren, wanneer u verplicht bent om te controleren of het aanmelden, u de melding ontvangen van push en scan uw vingerafdruk uw PINCODE invoeren.

![Push-bericht](./media/multi-factor-authentication-azure-authenticator/touchid2.png)

## <a name="uninstall-the-old-azure-authentication-app"></a>Verwijder de oude Azure verificatie app

Nadat u alle accounts hebt toegevoegd aan de nieuwe app, kunt u de oude app verwijderen uit de telefoon.

## <a name="delete-an-account"></a>Een account verwijderen

Als een account uit de Microsoft Authenticator app wilt verwijderen, selecteert u de account en selecteert u **verwijderen**.

![Knop verwijderen](./media/multi-factor-authentication-azure-authenticator/remove.png)
