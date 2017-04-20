<properties
    pageTitle="Best Practices: Azure AD wachtwoordbeheer | Microsoft Azure"
    description="Implementatie en het gebruik van beste praktijken, monster eindgebruikers documentatie en handleidingen voor wachtwoordbeheer in Azure Active Directory."
    services="active-directory"
    documentationCenter=""
    authors="asteen"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/12/2016"
    ms.author="asteen"/>

# <a name="deploying-password-management-and-training-users-to-use-it"></a>Wachtwoordbeheer implementatie en training van gebruikers om het te gebruiken

> [AZURE.IMPORTANT] **Weet u hier omdat u ondervindt problemen met inloggen?** In dat geval [hier hoe u kunt wijzigen en uw eigen wachtwoord opnieuw instellen](active-directory-passwords-update-your-own-password.md).

Nadat het wachtwoord inschakelen, is de volgende stap die u moet uitvoeren om gebruikers met de service in uw organisatie. Hiertoe moet u controleren of uw gebruikers worden geconfigureerd voor het gebruik van de service correct en ook dat uw gebruikers beschikken over de opleiding die ze nodig hebben bij het beheren van hun eigen wachtwoord lukt. In dit artikel wordt uitgelegd voor u de volgende concepten:

* [**Hoe u uw gebruikers geconfigureerd voor wachtwoordbeheer**](#how-to-get-users-configured-for-password-reset)
  * [Wat maakt een account die is geconfigureerd voor het opnieuw instellen van wachtwoorden](#what-makes-an-account-configured)
  * [Manieren om te vullen van verificatiegegevens](#ways-to-populate-authentication-data)
* [**De beste manieren om het wachtwoord opnieuw instellen om uw organisatie te zetten**](#what-is-the-best-way-to-roll-out-password-reset-for-users)
  * [Implementatie op basis van een e-mailadres + monster e-mail communicatie](#email-based-rollout)
  * [Maak uw eigen aangepaste wachtwoord management portal voor gebruikers](#creating-your-own-password-portal)
  * [Het gebruik van registratie afgedwongen gebruikers bij inloggen registreren](#using-enforced-registration)
  * [Het uploaden van verificatiegegevens voor gebruikersaccounts](#uploading-data-yourself)
* [**Voorbeeldgebruiker en trainingsmateriaal ondersteuning (binnenkort!)**](#sample-training-materials)

## <a name="how-to-get-users-configured-for-password-reset"></a>Hoe gebruikers worden geconfigureerd voor het opnieuw instellen van wachtwoorden
Deze sectie beschrijft u diverse methoden waarmee u ervoor dat Self-service wachtwoord opnieuw effectief zorgen kunt wanneer ze hun wachtwoord vergeten door elke gebruiker in uw organisatie kunt gebruiken.

### <a name="what-makes-an-account-configured"></a>Wat maakt een account
Voordat een gebruiker wachtwoord opnieuw instellen gebruikt kan, is **alle** van de volgende voorwaarden voldaan:

1.  Opnieuw instellen van wachtwoorden moet zijn ingeschakeld in Active directory.  Meer informatie over het wachtwoord opnieuw instellen door te lezen [zodat gebruikers hun Azure AD wachtwoorden opnieuw instellen](active-directory-passwords-getting-started.md#enable-users-to-reset-their-azure-ad-passwords) of [gebruikers opnieuw instellen of wijzigen van hun wachtwoord AD](active-directory-passwords-getting-started.md#enable-users-to-reset-or-change-their-ad-passwords) inschakelen
2.  De gebruiker moet een licentie hebben.
 - Voor gebruikers van de wolk moet de gebruiker **een betaalde licentie voor Office 365**, of een **Eenvoudige AAD** of **AAD Premium licentie** toegewezen.
 - Voor prem gebruikers (federatieve of hash gesynchroniseerd), de gebruiker **moet beschikken over een AAD Premium licentie toegewezen**.
3.  De gebruiker moet beschikken over de **minimale set met verificatiegegevens die zijn gedefinieerd** volgens het huidige beleid voor wachtwoord opnieuw instellen.
 - Verificatiegegevens beschouwd als gedefinieerd als het overeenkomstige veld in de directory juist opgemaakte gegevens bevat.
 - Een minimum van verificatiegegevens wordt gedefinieerd op **ten minste één** van de verificatieopties worden ingeschakeld als u een beleid met één poort is geconfigureerd, of op **ten minste twee** van de verificatieopties worden ingeschakeld als een twee-gate-beleid is geconfigureerd.
4.  Als de gebruiker een account op lokalen, moet vervolgens [Wachtwoord Writeback](active-directory-passwords-getting-started.md#enable-users-to-reset-or-change-their-ad-passwords) worden ingeschakeld en geactiveerd

### <a name="ways-to-populate-authentication-data"></a>Manieren om in te vullen verificatiegegevens
U hebt verschillende opties voor het opgeven van gegevens voor gebruikers in uw organisatie wordt gebruikt voor het opnieuw instellen van wachtwoorden.

- Gebruikers in de [Azure Management Portal](https://manage.windowsazure.com) of de [Office 365-beheerportal](https://portal.microsoftonline.com) bewerken
- Azure AD Sync gebruiken voor het synchroniseren van gebruikersgegevens in Azure AD van een Active Directory-domein op ruimten
- Gebruik Windows PowerShell gebruikerseigenschappen bewerken met de [volgende stappen hier](active-directory-passwords-learn-more.md#how-to-access-password-reset-data-for-your-users).
- Toestaan dat gebruikers hun eigen gegevens te registreren door ze naar de portal Registratie op [http://aka.ms/ssprsetup](http://aka.ms/ssprsetup)
- Gebruikers moeten registreren wachtwoord opnieuw instellen wanneer ze zich bij hun account Azure AD instellen aanmelden voor de [**vereisen dat gebruikers registreren als u zich aanmeldt?**](active-directory-passwords-customize.md#require-users-to-register-when-signing-in) optie op **Ja**.

Gebruikers hoeft niet te registreren voor het wachtwoord opnieuw instellen voor het systeem te werken.  Als u bestaande mobiele of office telefoonnummers in uw lokale map, bijvoorbeeld in Azure AD synchroniseren en we ze gebruiken voor wachtwoord automatisch opnieuw ingesteld.

Ook vindt u meer informatie over het [hoe gegevens worden gebruikt door het wachtwoord opnieuw instellen](active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset) en [hoe u verificatie voor afzonderlijke velden met PowerShell kunt vullen](active-directory-passwords-learn-more.md#how-to-access-password-reset-data-for-your-users).

## <a name="what-is-the-best-way-to-roll-out-password-reset-for-users"></a>Wat is de beste manier om het wachtwoord opnieuw instellen voor gebruikers zetten?
De invoering van algemene stappen voor het opnieuw instellen van wachtwoorden zijn:

1.  Wachtwoord opnieuw instellen in de map door te gaan naar het tabblad **configureren** in de [Azure Management Portal](https://manage.windowsazure.com) en als u **Ja** selecteert voor de optie **Gebruikers zijn ingeschakeld voor het opnieuw instellen van wachtwoorden** inschakelen.
2.  De juiste licenties toewijzen aan elke gebruiker aan wie u wilt graag bieden wachtwoord opnieuw instellen in het door naar de tab **licenties** in [Azure Management Portal](https://manage.windowsazure.com)te gaan.
3.  Beperk eventueel wachtwoord opnieuw instellen op een groep gebruikers naar de functie langzaam na verloop van tijd de **Toegang beperken tot het wachtwoord opnieuw instellen** in-/ uitschakelen instelt op **Ja** en selecteert u een beveiligingsgroep inschakelen voor het opnieuw instellen van wachtwoorden (Opmerking: deze gebruikers alle licenties moeten zijn toegewezen).
4.  Moeten uw wachtwoord opnieuw instellen of door ze te sturen een e-mail om te registreren, dat gebruikers inschakelen registratie in het access-venster, of door de juiste verificatiegegevens voor die gebruikers zelf uploaden via DirSync, PowerShell of de [Azure Management Portal](https://manage.windowsazure.com)wordt afgedwongen.  Hieronder vindt u meer details over dit.
5.  Bekijk gebruikers registreren door te navigeren naar het tabblad rapporten en het activiteitenoverzicht [**wachtwoord opnieuw registreren**](active-directory-passwords-get-insights.md#view-password-reset-registration-activity) na verloop van tijd.
6.  Zodra een goede aantal gebruikers hebben geregistreerd, kijken ze het wachtwoord opnieuw instellen door te navigeren naar het tabblad rapporten en het weergeven van het activiteitenverslag [**wachtwoord opnieuw instellen**](active-directory-passwords-get-insights.md#view-password-reset-activity) .

Er zijn verschillende manieren in kennis te stellen van uw gebruikers dat zij kunnen registreren voor en wachtwoord opnieuw instellen in uw organisatie gebruiken.  Ze worden hieronder beschreven.

### <a name="email-based-rollout"></a>Implementatie op basis van e-mail
Misschien is de eenvoudigste oplossing om uw gebruikers informeren over de voor registratie of wachtwoord opnieuw instellen door het verzenden van een e-mailbericht dat te doen.  Hieronder vindt u een sjabloon die u gebruiken kunt om dit te doen.  Voel vrij om de kleuren te vervangen / logo's met die van uw eigen aanpassen aan uw behoeften te kiezen.

  ![][001]

U kunt [downloaden van de e-mailsjabloon](http://1drv.ms/1xWFtQM).

### <a name="creating-your-own-password-portal"></a>De portal van uw eigen wachtwoord maken
Een strategie die geschikt voor grote klanten implementeren beheermogelijkheden wachtwoord is het maken van een enkel "wachtwoord portal" die uw gebruikers gebruiken kunnen voor het beheren van alle zaken met betrekking tot hun wachtwoorden op één plaats.  

Veel van onze grootste klanten kiezen voor het maken van een hoofd-DNS-vermelding, zoals https://passwords.contoso.com met koppelingen naar de Azure AD-wachtwoord opnieuw portal instellen, wachtwoord portal Registratie en wachtwoord pagina's wijzigen.  Op deze manier alle e-mailberichten of Folders die u verstuurt, kunt u opnemen in een enkele, gemakkelijk te onthouden, URL waarmee gebruikers om te kunnen gaan wanneer er een tweede aan de slag met de service.

Als u wilt gaan hier, we een eenvoudige pagina die gebruikmaakt van de nieuwste responsieve UI ontwerp paradigma's hebt gemaakt en werkt op alle browsers en mobiele apparaten.

  ![][007]

U kunt [de website sjabloon hier downloaden](https://github.com/kenhoff/password-reset-page).  Wij raden u aan het logo en de kleuren aan de behoeften van uw organisatie aanpassen.

### <a name="using-enforced-registration"></a>Met behulp van afgedwongen registratie
Als u wilt dat uw gebruikers te registreren voor zichzelf opnieuw in te stellen, kunt u ook dwingen om te registreren wanneer zij zich aan het deelvenster toegang op [http://myapps.microsoft.com aanmelden](http://myapps.microsoft.com).  Deze optie **configureren** op het tabblad van de map kunt u door het inschakelen van de optie **Gebruikers te registreren tijdens het aanmelden bij het Configuratiescherm toegang nodig hebben** .  

U kunt desgewenst ook definiëren wel of niet wordt gevraagd opnieuw registreren na een configureerbare periode doordat de optie **aantal dagen voordat de gebruikers moeten hun contactgegevens bevestigen** aan een niet-nul waarde. Zie [Gebruiker wachtwoord Management gedrag aanpassen](active-directory-passwords-customize.md#password-management-behavior) voor meer informatie.

  ![][002]

Nadat u deze optie inschakelt wanneer gebruikers zich bij het Configuratiescherm toegang aanmelden, zien ze een pop-up die meldt dat de beheerder om te controleren of hun contactgegevens is vereist. Ze kunnen gebruiken hun wachtwoord opnieuw instellen, als ze ooit geen toegang meer tot hun account.

  ![][003]

Te klikken op **Nu controleren of** brengt ze naar de **wachtwoordhersteldiskette-portal Registratie** op [http://aka.ms/ssprsetup](http://aka.ms/ssprsetup) en is nodig om te registreren.  Registratie via deze methode kan worden gesloten door op de knop **Annuleren** te klikken of het venster sluiten, maar gebruikers een herinnering telkens wanneer ze zich aanmelden als ze niet registreert.

  ![][004]

### <a name="uploading-data-yourself"></a>Uploaden van gegevens
Als u uploaden van verificatiegegevens wilt, vervolgens gebruikers hoeft niet te registreren voor het wachtwoord opnieuw instellen te kunnen hun wachtwoorden opnieuw instellen.  Als gebruikers beschikken over de verificatiegegevens die zijn gedefinieerd op de account die voldoet aan het wachtwoord opnieuw instellen van beleid dat u hebt gedefinieerd en vervolgens die gebruikers mogelijk hun wachtwoorden opnieuw instellen.

Als u wilt weten welke eigenschappen u via AAD verbinding of Windows PowerShell instellen kunt, zien [welke gegevens wordt gebruikt door het wachtwoord opnieuw instellen](active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset).

De verificatiegegevens via het [Azure Management Portal](https://manage.windowsazure.com) kunt u uploaden door de onderstaande stappen te volgen:

1.  Navigeer naar de map in de **uitbreiding voor Active Directory** in de [Azure Management Portal](https://manage.windowsazure.com).
2.  Klik op het tabblad **gebruikers** .
3.  Selecteer de gebruiker die u geïnteresseerd in de lijst bent.
4.  Op het eerste tabblad vindt u **Alternatieve e-mailadres**, die kunnen worden gebruikt als een eigenschap opnieuw instellen van wachtwoorden inschakelen.

    ![][005]

5.  Klik op het tabblad **Info werk** .
6.  Op deze pagina vindt u **Zakelijke telefoon**, **gsm**, **Telefoon verificatie**en **Verificatie-e-mailadres**.  Deze eigenschappen kunnen ook worden ingesteld op toestaan dat een gebruiker zijn of haar wachtwoord opnieuw instellen.

    ![][006]

Zien [welke gegevens door het wachtwoord opnieuw instellen wordt gebruikt](active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset) om te zien hoe elk van deze eigenschappen kan worden gebruikt.

Zie [toegang tot wachtwoord opnieuw instellen van gegevens voor gebruikers van PowerShell](active-directory-passwords-learn-more.md#how-to-access-password-reset-data-for-your-users) om te zien hoe u kunt lezen en deze gegevens met PowerShell.

## <a name="sample-training-materials"></a>Lesmateriaal van monster
Wij werken aan monster trainingsmateriaal kunt u snel uw onderneming en uw gebruikers aan de slag voor het implementeren en gebruiken van wachtwoord opnieuw instellen.  Bezoek ons regelmatig!


<br/>
<br/>
<br/>

## <a name="links-to-password-reset-documentation"></a>Koppelingen naar wachtwoord opnieuw documentatie
Hieronder vindt u koppelingen naar alle pagina's documentatie Azure AD-wachtwoord opnieuw instellen:

* **Weet u hier omdat u ondervindt problemen met inloggen?** In dat geval [hier hoe u kunt wijzigen en uw eigen wachtwoord opnieuw instellen](active-directory-passwords-update-your-own-password.md).
* [**Hoe het werkt**](active-directory-passwords-how-it-works.md) - informatie over de zes verschillende onderdelen van de service en wat elk wordt
* [**Aan de slag**](active-directory-passwords-getting-started.md) - leren hoe u toestaan dat gebruikers hun wachtwoord cloud of op locatie wijzigen en instellen
* [**Aanpassen**](active-directory-passwords-customize.md) - informatie over het aanpassen van de look en feel en gedrag van de service aan de behoeften van uw organisatie
* [**Krijg inzicht**](active-directory-passwords-get-insights.md) - meer informatie over onze geïntegreerde rapportagemogelijkheden.
* [**Veelgestelde vragen**](active-directory-passwords-faq.md) - antwoorden vinden op veelgestelde vragen
* [**Problemen oplossen**](active-directory-passwords-troubleshoot.md) - informatie over het oplossen van problemen met de service snel
* [**Meer informatie**](active-directory-passwords-learn-more.md) - Ga diep in de technische details van de werking van de service



[001]: ./media/active-directory-passwords-best-practices/001.jpg "Image_001.jpg"
[002]: ./media/active-directory-passwords-best-practices/002.jpg "Image_002.jpg"
[003]: ./media/active-directory-passwords-best-practices/003.jpg "Image_003.jpg"
[004]: ./media/active-directory-passwords-best-practices/004.jpg "Image_004.jpg"
[005]: ./media/active-directory-passwords-best-practices/005.jpg "Image_005.jpg"
[006]: ./media/active-directory-passwords-best-practices/006.jpg "Image_006.jpg"
[007]: ./media/active-directory-passwords-best-practices/007.jpg "Image_007.jpg"
