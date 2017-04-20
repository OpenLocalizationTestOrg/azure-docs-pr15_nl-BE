<properties
    pageTitle="Hoe het werkt: Azure AD wachtwoordbeheer | Microsoft Azure"
    description="Meer informatie over de verschillende onderdelen van Azure AD wachtwoordbeheer, inclusief waar gebruikers registreren, opnieuw instellen en wijzigen van wachtwoorden, en waar beheerders configureren, rapporteren over en beheer van Active Directory-wachtwoorden voor gebouwen inschakelen."
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

# <a name="how-password-management-works"></a>De werking van wachtwoordbeheer

> [AZURE.IMPORTANT] **Weet u hier omdat u ondervindt problemen met inloggen?** In dat geval [hier hoe u kunt wijzigen en uw eigen wachtwoord opnieuw instellen](active-directory-passwords-update-your-own-password.md).

Wachtwoordbeheer in Azure Active Directory bestaat uit verschillende logische onderdelen die hieronder worden beschreven.  Klik op de koppelingen voor meer informatie over het desbetreffende onderdeel.

- [**Wachtwoord Management configuratie van Portal**](#password-management-configuration-portal) -beheerders kunnen de verschillende facetten van de manier waarop wachtwoorden worden beheerd in hun huurders door te navigeren naar het tabblad voor het configureren in de [Azure Management Portal](https://manage.windowsazure.com)van hun map controleren.
- [**Portal voor registratie van gebruiker**](#user-registration-portal) – gebruikers zichzelf kunnen registreren voor wachtwoord opnieuw instellen via dit webportaal.
- [**Gebruiker wachtwoord resetten Portal**](#user-password-reset-portal) -gebruikers hun wachtwoorden met een aantal verschillende uitdagingen volgens de beheerders gecontroleerde reset wachtwoordbeleid kunnen herstellen
- [**Gebruiker wachtwoord wijzigen Portal**](#user-password-change-portal) -gebruikers kunnen hun wachtwoorden wijzigen op elk gewenst moment door te voeren hun oude wachtwoord en selecteert een nieuw wachtwoord met behulp van deze webportal
- [**Wachtwoord Management rapporten**](#password-management-reports) – beheerders kunnen bekijken en analyseren van wachtwoord opnieuw instellen en de registratie-activiteit in de huurder door te gaan naar de sectie 'Activiteitenoverzicht' van van de map 'Rapporten' tab in de [Azure Management Portal](https://manage.windowsazure.com)
- [**Wachtwoord Writeback onderdeel van Azure AD verbinden**](#password-writeback-component-of-azure-ad-connect) – beheerders eventueel de functie wachtwoord Write-back kunnen inschakelen bij het installeren van Azure AD verbinden zodat beheer van federatieve of wachtwoord wachtwoorden van de cloud gesynchroniseerd.

## <a name="password-management-configuration-portal"></a>Wachtwoord Management configuratie van Portal
U kunt wachtwoord beleid voor een specifieke map met de [Azure Management Portal](https://manage.windowsazure.com) door naar de sectie **Wachtwoord opnieuw instellen voor gebruikersbeleid** **configureren** -tabblad van de map te gaan.  Vanaf deze configuratiepagina kunt u vele aspecten van de manier waarop wachtwoorden worden beheerd in uw organisatie beheren met inbegrip van:

- In- en uitschakelen voor alle gebruikers in een map opnieuw instellen wachtwoord
- Instellen van het aantal uitdagingen (één of twee) van een gebruiker moet gaan via zijn of haar wachtwoord opnieuw instellen
- Het instellen van de specifieke soorten uitdagingen die u wilt inschakelen voor gebruikers in uw organisatie uit de onderstaande mogelijkheden:
 - Mobiele telefoon (via de tekst of een telefoongesprek een verificatiecode)
 - Telefoonnummer (een telefoongesprek)
 - Alternatieve e-mailadres (een verificatiecode per e-mail)
 - Vragen over beveiliging (kennis gebaseerde verificatie)
- Het aantal vragen als u een gebruiker moet registreren om te kunnen gebruiken, de vragen verificatiemethode (alleen zichtbaar als de beveiliging vragen zijn ingeschakeld)
- Het aantal vragen als u een gebruiker moet opgeven tijdens opnieuw instellen te gebruiken, de vragen verificatiemethode (alleen zichtbaar als de beveiliging vragen zijn ingeschakeld)
- Met behulp van vooraf Ingeblikte, gelokaliseerde, beveiliging vragen die een gebruiker kiezen kan om te gebruiken bij het registreren voor een wachtwoord opnieuw instellen (alleen zichtbaar als de beveiliging vragen zijn ingeschakeld)
- Definiëren van de aangepaste beveiligingsinstellingen vragen die een gebruiker kiezen kan om te gebruiken bij het registreren voor een wachtwoord opnieuw instellen (alleen zichtbaar als de beveiliging vragen zijn ingeschakeld)
- Dat gebruikers hoeven te registreren om wachtwoord opnieuw instellen wanneer ze de toepassing toegang tot Configuratiescherm op [http://myapps.microsoft.com](http://myapps.microsoft.com).
- Vereisen gebruikers bevestigen opnieuw hun eerder geregistreerde gegevens na een configureerbare aantal dagen is verstreken (alleen zichtbaar als afgedwongen registratie is ingeschakeld)
- Middel van een helpdesk aangepaste e-mailadres of de URL die wordt weergegeven wanneer gebruikers in het geval er een probleem dat hun wachtwoorden opnieuw instellen
- In- of uitschakelen van de functionaliteit wachtwoord Write-back (als wachtwoord Write-back is geïmplementeerd met behulp van AAD verbinding)
- Weergeven van de status van de agent Writeback wachtwoord (als wachtwoord Write-back is geïmplementeerd met behulp van AAD verbinding)
- Inschakelen van e-mailberichten naar gebruikers wanneer hun wachtwoord opnieuw is ingesteld (zoals aangegeven in de sectie **meldingen** van de [Azure Management Portal](https://manage.windowsazure.com))
- Inschakelen van e-mailberichten naar beheerders wanneer andere beheerders eigen (te vinden in de sectie **meldingen** van de [Azure Management Portal](https://manage.windowsazure.com) wachtwoorden opnieuw instellen
- Portal huismerk van het wachtwoord opnieuw in en wachtwoord opnieuw instellen e-mails met het logo en de naam van uw organisatie met de huurder aanpassing functie (in de **Eigenschappen van de map** -sectie van de [Azure Management Portal](https://manage.windowsazure.com) huismerk

Zie voor meer informatie over het configureren van wachtwoordbeheer in uw organisatie, [aan de slag: wachtwoordbeheer Azure AD](active-directory-passwords-getting-started.md).

##<a name="user-registration-portal"></a>Portal voor registratie van gebruiker
Voordat gebruikers wachtwoord opnieuw instellen gebruikt kunnen, moeten hun cloud-accounts worden bijgewerkt met de gegevens van de juiste verificatie om ervoor te zorgen dat zij het juiste aantal uitdagingen van wachtwoord opnieuw instellen die is gedefinieerd door de beheerder kunnen passeren.  Beheerders kunnen ook deze verificatie-informatie ten behoeve van de gebruiker definiëren met behulp van de Azure of Office webportals, DirSync / Azure AD verbinding maken of Windows PowerShell.

Echter, als u liever uw gebruikers hun eigen gegevens te registreren, wij bieden ook een webpagina die gebruikers om deze informatie te verstrekken om te kunnen gaan.  Deze pagina kunnen gebruikers verificatiegegevens volgens het wachtwoord opnieuw instellen voor beleidsregels die zijn ingeschakeld in hun organisatie opgeven.  Nadat deze gegevens zijn gecontroleerd, wordt deze opgeslagen in de cloud-gebruikersaccount moet worden gebruikt voor accountherstel op een later tijdstip. Dit is wat de registratie portal lijkt op:

  ![][001]

Zie voor meer informatie [aan de slag: wachtwoordbeheer Azure AD](active-directory-passwords-getting-started.md) en [Best Practices: wachtwoordbeheer Azure AD](active-directory-passwords-best-practices.md).

##<a name="user-password-reset-portal"></a>Portal voor gebruiker wachtwoord opnieuw instellen
Als u eenmaal hebt ingeschakeld Self-service wachtwoord opnieuw instellen van uw organisatie zelf opnieuw instellen van wachtwoordbeleid en ervoor gezorgd dat uw gebruikers beschikken over de juiste gegevens voor de contactpersoon in Active directory, gebruikers in uw organisatie kunnen hun eigen wachtwoorden automatisch vanuit een webpagina die een account voor werk of op School voor aanmelden (zoals [portal.microsoftonline.com gebruikt](https://portal.microsoftonline.com)) opnieuw instellen. Op de pagina's zoals deze gebruikers zien een **geen toegang tot uw account?** koppeling.

  ![][002]

Op deze link te klikken wordt de portal zelf wachtwoord opnieuw instellen wordt gestart.

  ![][003]

Zie voor meer informatie over de manier waarop gebruikers hun eigen wachtwoord kunnen herstellen, [aan de slag: wachtwoordbeheer Azure AD](active-directory-passwords-getting-started.md).

##<a name="user-password-change-portal"></a>Portal voor gebruiker wachtwoord wijzigen
Als gebruikers hun eigen wachtwoord wijzigen, kunnen ze dit doen met behulp van de portal wachtwoord wijzigen op elk gewenst moment.  Gebruikers kunnen toegang krijgen tot de portal wachtwoord wijzigen via het Configuratiescherm toegang profielpagina of te klikken op de koppeling 'wachtwoord wijzigen' uit in toepassingen van Office 365.  In het geval wanneer hun wachtwoord vervalt, gebruikers ook gevraagd automatisch wijzigen tijdens het aanmelden.

  ![][004]

In beide gevallen als wachtwoord Write-back is ingeschakeld en de gebruiker is een federatieve of wachtwoord synchronisatie zou worden deze gewijzigde wachtwoorden opgeslagen in Active Directory op gebouwen. Dit is wat het wachtwoord wijzigen portal ziet eruit als:

  ![][005]

Zie voor meer informatie over hoe gebruikers hun wachtwoorden op ruimten Active Directory kunnen wijzigen, [aan de slag: wachtwoordbeheer Azure AD](active-directory-passwords-getting-started.md).

##<a name="password-management-reports"></a>Wachtwoord Management rapporten
Navigeren naar het tabblad **rapporten** en zoek in de sectie **Logboeken voor faxactiviteit** , ziet u twee wachtwoordbeheer rapporten: **activiteit opnieuw instellen van wachtwoord** en **wachtwoord opnieuw instellen registratie activiteit**.  Met deze twee verslagen, krijgt u een weergave van gebruikers voor registratie en wachtwoord opnieuw instellen in uw organisatie. Hier ziet u hoe deze rapporten eruit in [Azure Management Portal](https://manage.windowsazure.com):

  ![][006]

Zie voor meer informatie [inzichten krijgen: rapporten wachtwoordbeheer Azure AD](active-directory-passwords-get-insights.md).

##<a name="password-writeback-component-of-azure-ad-connect"></a>Wachtwoord Writeback onderdeel van Azure AD verbinden
Als de wachtwoorden van gebruikers in uw organisatie zijn afkomstig uit uw omgeving op gebouwen (hetzij via de federation of synchronisatie van wachtwoorden), kunt u de nieuwste versie van Azure AD verbinden zodat deze wachtwoorden vanuit de cloud rechtstreeks bijwerken installeren.  Dit betekent dat wanneer uw gebruikers vergeten of wilt wijzigen van hun wachtwoord AD, ze dus direct vanaf het web doen kunnen.  Waar vindt u in de installatiewizard Azure AD verbinden wachtwoord Write-back is:

  ![][007]

Zie voor meer informatie over het verbinden van Azure AD [aan de slag: verbinden met Azure AD](active-directory-aadconnect.md). Zie voor meer informatie over het wachtwoord Write-back [aan de slag: wachtwoordbeheer Azure AD](active-directory-passwords-getting-started.md).


<br/>
<br/>
<br/>

## <a name="links-to-password-reset-documentation"></a>Koppelingen naar wachtwoord opnieuw documentatie
Hieronder vindt u koppelingen naar alle pagina's documentatie Azure AD-wachtwoord opnieuw instellen:

* **Weet u hier omdat u ondervindt problemen met inloggen?** In dat geval [hier hoe u kunt wijzigen en uw eigen wachtwoord opnieuw instellen](active-directory-passwords-update-your-own-password.md).
* [**Aan de slag**](active-directory-passwords-getting-started.md) - leren hoe u toestaan dat gebruikers hun wachtwoord cloud of op locatie wijzigen en instellen
* [**Aanpassen**](active-directory-passwords-customize.md) - informatie over het aanpassen van de look en feel en gedrag van de service aan de behoeften van uw organisatie
* [**Best practices**](active-directory-passwords-best-practices.md) - informatie over het snel en effectief beheren van wachtwoorden in uw organisatie
* [**Krijg inzicht**](active-directory-passwords-get-insights.md) - meer informatie over onze geïntegreerde rapportagemogelijkheden.
* [**Veelgestelde vragen**](active-directory-passwords-faq.md) - antwoorden vinden op veelgestelde vragen
* [**Problemen oplossen**](active-directory-passwords-troubleshoot.md) - informatie over het oplossen van problemen met de service snel
* [**Meer informatie**](active-directory-passwords-learn-more.md) - Ga diep in de technische details van de werking van de service



[001]: ./media/active-directory-passwords-how-it-works/001.jpg "Image_001.jpg"
[002]: ./media/active-directory-passwords-how-it-works/002.jpg "Image_002.jpg"
[003]: ./media/active-directory-passwords-how-it-works/003.jpg "Image_003.jpg"
[004]: ./media/active-directory-passwords-how-it-works/004.jpg "Image_004.jpg"
[005]: ./media/active-directory-passwords-how-it-works/005.jpg "Image_005.jpg"
[006]: ./media/active-directory-passwords-how-it-works/006.jpg "Image_006.jpg"
[007]: ./media/active-directory-passwords-how-it-works/007.jpg "Image_007.jpg"
