<properties
    pageTitle="Azure MVR gesloten aanmelding ervaring met Azure meerledige verificatie"
    description="Deze pagina geeft u instructies voor het bekijken van de verschillende aanmelding methoden met Azure MVR gesloten."
    keywords="gebruikersverificatie, -in-ervaring, aanmelden met een mobiele telefoon, aanmelden bij office telefoon"
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

# <a name="the-sign-in-experience-with-azure-multi-factor-authentication"></a>De ervaring met Azure meerledige verificatie aanmelden
> [AZURE.NOTE]  De volgende documentatie op deze pagina ziet u een typische ervaring-in.  Zie [problemen met Azure meerledige verificatie](multi-factor-authentication-end-user-manage-settings.md) voor hulp bij het aanmelden



## <a name="what-will-your-sign-in-experience-be"></a>Wat is uw aanmelding ervaring?
Afhankelijk van hoe u zich aanmeldt en meerledige verificatie, verschillen uw ervaring.  In deze sectie zullen we informatie verschaffen over wat u kunt verwachten wanneer u zich aanmeldt.  Kies de methode die het best beschrijft wat u doet:


Wat ben je aan het doen?|Beschrijving
:------------- | :------------- |
[Aanmelden met telefoon mobiel of office](#signing-in-with-mobile-or-office-phone) | Dit is wat u kunt verwachten van aanmelden via uw telefoon mobiel of office.
[Aanmelden met de Microsoft Authenticator app met melding](#signing-in-with-the-microsoft-authenticator-app-using-notification) | Dit is wat u kunt verwachten met de Microsoft Authenticator app met meldingen.
[Aanmelden met de Microsoft Authenticator app gebruikt-verificatiecode in](#signing-in-with-the-microsoft-authenticator-app-using-verification-code)|Dit is wat u kunt verwachten met de Microsoft Authenticator thapp met een verificatiecode.
[Aanmelden met een alternatieve methode](#signing-in-with-an-alternate-method)|Hier ziet u wat u kunt verwachten als u wilt een alternatieve methode gebruiken.

## <a name="signing-in-with-mobile-or-office-phone"></a>Aanmelden met telefoon mobiel of office

De volgende informatie beschrijft de ervaring van het meerledige verificatie mogelijk met uw telefoon mobiel of office.

### <a name="to-sign-in-with-a-call-to-your-office-or-mobile-phone"></a>Aan te melden met een aanroep van uw kantoor of mobiele telefoon

- Log in om een toepassing of service zoals Office 365 met uw gebruikersnaam en wachtwoord.
- Microsoft zal u bellen.

![Aanroepen van Microsoft](./media/multi-factor-authentication-end-user-signin-phone/call.png)

- De telefoon opnemen en druk op de toets #.

![Antwoord](./media/multi-factor-authentication-end-user-signin-phone/phone.png)

- U moet nu worden aangemeld.</li>

## <a name="signing-in-with-the-microsoft-authenticator-app-using-notification"></a>Aanmelden met de Microsoft Authenticator app met melding

De volgende informatie beschrijft de ervaringen van een meerledige verificatie gebruiken met de Microsoft Authenticator app wanneer u een bericht worden verzonden.

### <a name="to-sign-in-with-a-notification-sent-the-microsoft-authenticator-app"></a>Als u wilt zich aanmelden met een melding verzonden de Microsoft Authenticator app

- Log in om een toepassing of service zoals Office 365 met uw gebruikersnaam en wachtwoord.
- Microsoft stuurt een melding.

![Microsoft stuurt melding](./media/multi-factor-authentication-end-user-signin-app-notify/notify.png)


- De telefoon opnemen en druk op de sleutel controleren.  Als uw bedrijf een PINCODE vereist wordt u gevraagd voor het hier.

![Controleer of](./media/multi-factor-authentication-end-user-signin-app-notify/phone.png)

![Setup](./media/multi-factor-authentication-end-user-first-time-mobile-app/scan3.png)

- U moet nu worden aangemeld.


## <a name="signing-in-with-the-microsoft-authenticator-app-using-verification-code"></a>Aanmelden met de Microsoft Authenticator app gebruikt-verificatiecode in

De volgende informatie beschrijft de ervaringen van een meerledige verificatie gebruiken met de Microsoft Authenticator app wanneer u deze met een verificatiecode.

### <a name="to-sign-in-using-a-verification-code-with-the-microsoft-authenticator-app"></a>Aanmelden met een verificatiecode die met de Microsoft Authenticator app

- Log in om een toepassing of service zoals Office 365 met uw gebruikersnaam en wachtwoord.
- Microsoft vraagt u om een verificatiecode.

![Voer de verificatiecode in](./media/multi-factor-authentication-end-user-signin-app-verify/verify.png)

- Open de Microsoft Authenticator app op je telefoon en voer de code in het vak waar u zich wilt aanmelden.

![Code ophalen](./media/multi-factor-authentication-end-user-signin-app-verify/phone.png)



- U moet nu worden aangemeld.


## <a name="signing-in-with-an-alternate-method"></a>Aanmelden met een alternatieve methode


In het volgende gedeelte wordt beschreven hoe u aan te melden met een alternatieve methode als uw primaire methode mogelijk niet beschikbaar.

### <a name="to-sign-in-with-an-alternate-method"></a>Aan te melden met een alternatieve methode

- Log in om een toepassing of service zoals Office 365 met uw gebruikersnaam en wachtwoord.
- Selecteer een andere verificatieoptie gebruiken.  U zijn met een keuze uit verschillende opties aanwezig. Het nummer er zal worden gebaseerd op hoeveel u ingesteld hebt.

![Alternatieve methode gebruiken](./media/multi-factor-authentication-end-user-signin-alt/alt.png)

- Een alternatieve methode kiezen en log in.
