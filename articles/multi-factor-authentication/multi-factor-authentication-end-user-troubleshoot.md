<properties
    pageTitle="Problemen met verificatie in twee stappen | Microsoft Azure"
    description="Dit document krijgt de gebruiker informatie over wat te doen als ze een probleem optreden met Azure meerledige verificatie uitvoeren."
    services="multi-factor-authentication"
    keywords = "multifactor-verificatie-client verificatieprobleem, correlatie-ID"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="yossib"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="kgremban"/>

# <a name="having-trouble-with-two-step-verification"></a>Problemen met verificatie in twee stappen

Dit artikel worden enkele problemen beschreven die met verificatie in twee stappen optreden kunnen. Als het probleem dat u ondervindt niet opgenomen hier, Geef gedetailleerde feedback in de sectie Opmerkingen, zodat we kunnen verbeteren.

## <a name="i-lost-my-phone-or-it-was-stolen"></a>Ik heb mijn telefoon verloren of gestolen was

Er zijn twee manieren om terug te gaan op uw account. De eerste is aan te melden met uw telefoonnummer alternatieve verificatie als u één hebt ingesteld. De tweede is de vraag de beheerder om uw instellingen te wissen.

Als uw telefoon is verloren of gestolen, wordt ook aangeraden dat u de beheerder uw app wachtwoorden opnieuw instellen en schakel alle apparaten onthouden. Als uw beheerder niet zeker hoe om dit te doen, verwijzen ze naar dit artikel: [gebruikers beheren en apparaten](multi-factor-authentication-manage-users-and-devices.md#delete-users-existing-app-passwords).


### <a name="use-an-alternate-phone-number"></a>Gebruik een alternatief telefoonnummer

Als u meerdere opties voor verificatie, met inbegrip van een tweede telefoonnummer of een authenticator app op een ander apparaat hebt ingesteld kunt u een van deze aan te melden.

Om aan te melden met behulp van het alternatieve telefoonnummer, als volgt te werk:

1. Log in als normaal.
2. Wanneer u wordt gevraagd om verder te controleren of uw account, kies **Gebruik een andere verificatie-optie**.

    ![Andere controle](./media/multi-factor-authentication-end-user-manage/differentverification.png)

3. Selecteer het telefoonnummer dat u toegang hebt.

    ![Alternatief telefoonnummer](./media/multi-factor-authentication-end-user-manage/altphone2.png)

4. Nadat u terug in uw account, het [beheren van uw instellingen bent](multi-factor-authentication-end-user-manage-settings.md) wijzigen van uw telefoonnummer verificatie.

>[AZURE.IMPORTANT]
>Het is belangrijk dat u een telefoonnummer van de tweede verificatie configureert. Als uw telefoonnummer en uw mobiele app op de telefoon dezelfde, u moet een derde optie als uw telefoon verloren of gestolen is.

### <a name="clear-your-settings"></a>Uw instellingen wissen

Als u een telefoonnummer van de tweede verificatie niet hebt geconfigureerd, hebt u contact op met uw beheerder voor hulp. Zijn uw instellingen te wissen, zodat de volgende keer dat u zich aanmeldt, wordt u gevraagd [uw account](multi-factor-authentication-end-user-first-time.md) instellen opnieuw.


## <a name="i-am-not-receiving-a-text-or-call-on-my-phone"></a>Ik ontvang geen tekst of bel op mijn telefoon

Er zijn verschillende redenen waarom u wilt mogelijk aanmelden, maar niet de tekst of een telefoongesprek ontvangt. Als je hebt ontvangen teksten of telefoongesprekken op uw telefoon in het verleden, is dit waarschijnlijk een probleem met de provider van de telefoon, niet op uw account. Zorg ervoor dat je goede cel signaal hebt en als u probeert te ontvangen een bericht met tekst ervoor zorgen dat uw plan voor telefoon- en SMS-berichten ondersteunen.

Als u enkele minuten voor een tekst of een gesprek hebt gewacht, is de snelste manier om uw rekening naar een andere optie proberen.

1. **Gebruik een andere verificatie-optie** selecteert op de pagina die op uw controle wachten.

    ![Andere controle](./media/multi-factor-authentication-end-user-troubleshoot/diff_option.png)

2. Selecteer de telefoon nummer of levering methode die u wilt gebruiken.

    Als er meerdere codes voor verificatie, werkt alleen met de nieuwste versie.

Als u niet een andere manier geconfigureerd hebt, neem contact op met de beheerder en vraag deze om uw instellingen te wissen. De volgende keer dat u zich aanmeldt, wordt u gevraagd voor het [instellen van een meerledige verificatie](multi-factor-authentication-end-user-first-time.md) opnieuw.


Hebt u vaak vertragingen als gevolg van beschadigde cel signaal, kunt u het beste gebruik van de [verificator Microsoft app](multi-factor-authentication-microsoft-authenticator.md) op uw smartphone. De app kan genereren willekeurige beveiligingscodes waarmee u aan te melden en deze codes niet elke cel signaal of een internet-verbinding vereist.


## <a name="app-passwords-are-not-working"></a>App wachtwoord werkt niet

Controleer eerst of u het app-wachtwoord juist hebt ingevoerd.  Als u nog steeds niet werkt probeer ondertekenen in en [Maak een nieuw wachtwoord voor de app](multi-factor-authentication-end-user-app-passwords.md).  Als dit niet werkt, neem contact op met de beheerder en ze kunnen [uw bestaande app wachtwoorden verwijderen](multi-factor-authentication-manage-users-and-devices.md#delete-users-existing-app-passwords) en u vervolgens een nieuwe maken.

## <a name="i-didnt-find-an-answer-to-my-problem"></a>Ik heb een antwoord voor mijn probleem vinden.

Neem contact op met uw beheerder of de persoon die een meerledige verificatie voor u instellen als u deze stappen hebt geprobeerd, maar nog steeds actief op problemen. Zij moeten mogelijk zijn om u te helpen.

Ook kunt u een vraag op de [Azure AD Forums](https://social.msdn.microsoft.com/forums/azure/home?forum=WindowsAzureAD) of [Neem contact op met ondersteuning voor](https://support.microsoft.com/contactus) boeken en wij reageren op uw probleem, zodra we kunnen.

Als u contact op met de ondersteuning, de volgende gegevens bevatten:

- **Gebruikersnaam** : Wat is het e-mailadres dat u hebt geprobeerd te melden met?
- **Algemene beschrijving van de fout** : welke exacte foutbericht hebt u zien?  Als er geen foutbericht wordt weergegeven, wordt het onverwachte gedrag dat u in detail opgemerkt beschreven.
- **Pagina** – welke pagina u op wanneer u de fout gezien waren (inclusief de URL)?
- **ErrorCode** - de specifieke foutcode die u ontvangt.
- **SessionId** - de specifieke sessie-id die u ontvangt.
- **Correlatie-ID** – wat is de correlatie-id-code wanneer de gebruiker de fout werd gegenereerd.
- **Tijdstempel** – wat is de precieze datum en tijd die u hebt gezien de fout (inclusief de tijdzone)?

Veel van deze informatie kunt u vinden op de pagina aanmelden. Als u niet controleren of het aanmelden in de tijd, selecteer **details weergeven**.

![Details van deze fout melden](./media/multi-factor-authentication-end-user-troubleshoot/view_details.png)

Deze informatie helpt ons uw probleem zo snel mogelijk oplossen.

## <a name="related-topics"></a>Verwante onderwerpen
- [Beheren van uw instellingen voor verificatie in twee stappen](multi-factor-authentication-end-user-manage-settings.md)  
- [Veelgestelde vragen over Microsoft-Authenticator-toepassing](multi-factor-authentication-app-faq.md)
