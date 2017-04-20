<properties
   pageTitle="Richtlijnen voor toepassingen huisstijl | Microsoft Azure"
   description="Een uitgebreide handleiding resources ontwikkelaar-georiënteerde voor Azure Active Directory"
   services="active-directory"
   documentationCenter="dev-center-name"
   authors="msmbaldwin"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="06/23/2016"
   ms.author="mbaldwin"/>


# <a name="branding-guidelines-for-applications"></a>Huisstijl richtlijnen voor toepassingen


In dit onderwerp worden de huisstijl richtlijnen die moet u bij het ontwikkelen van toepassingen met Azure Active Directory (AD Azure) beschreven. Deze richtlijnen helpen uw klanten direct als ze hun werk of school account willen, beheerd in Azure AD, of hun persoonlijke account voor aanmelden en aanmelden bij uw toepassing.

## <a name="personal-accounts-vs-work-or-school-accounts-from-microsoft"></a>Persoonlijke versus werk of school grootboekrekeningen van Microsoft

Microsoft beheert twee typen gebruikersaccounts:

- **Persoonlijke accounts** (voorheen Windows Live ID). Deze rekeningen staan voor de relatie tussen *individuele* gebruikers en Microsoft en worden gebruikt voor toegang tot apparaten en services van Microsoft. Deze accounts zijn bedoeld voor persoonlijk gebruik.

- **Accounts voor werk of school.** Deze accounts worden beheerd door Microsoft voor organisaties die gebruikmaken van Azure Active Directory. Deze rekeningen worden gebruikt voor het aanmelden bij Office 365 en overige zakelijke diensten van Microsoft.

Microsoft accounts voor werk of school meestal aan eindgebruikers (werknemers, studenten, federale werknemers) door hun organisaties (bedrijf, school, een overheidsinstelling toegewezen zijn). Deze accounts zijn onder de knie direct in de cloud, in het AD Azure-platform of gesynchroniseerd met Azure AD van een directory op ruimten, zoals Windows Server Active Directory. Microsoft is de *beheerder* van de rekeningen voor werk of school, maar de rekeningen zijn eigendom en wordt beheerd door de organisatie.

## <a name="referring-to-azure-ad-accounts-in-your-application"></a>Verwijzen naar Azure AD accounts in uw toepassing

Microsoft blootstellen niet eindgebruikers de Azure of merknamen van de Active Directory en geen van beide moet u.

- Als gebruikers zijn aangemeld, moet u de naam en het logo zo veel mogelijk van de organisatie. Dit is beter dan het gebruik van generieke termen als 'de organisatie'.

- Als gebruikers niet zijn aangemeld, moet u verwijzen naar hun rekeningen als "werk of school accounts" en het Microsoft-logo gebruiken om aan te geven dat deze rekeningen worden beheerd door Microsoft. Gebruik geen termen als "account voor de onderneming", "business account" of "corporate account" die de gebruiker verwarring kan ontstaan.

## <a name="user-account-pictogram"></a>Pictogram voor gebruiker account
In een eerdere versie van deze richtsnoeren is het raadzaam met behulp van een pictogram "blue badge". Op basis van feedback van gebruikers en ontwikkelaars, nu het beste het gebruik van het Microsoft-logo in plaats daarvan. Dit helpt gebruikers begrijpen dat zij de rekening die ze gebruiken met Office 365 of andere Microsoft-diensten aan ondernemingen aan te melden uw app opnieuw kunnen gebruiken.

## <a name="signing-up-and-signing-in-with-azure-ad"></a>Aanmelden en aanmelden met Azure AD

Uw app afzonderlijke paden voor het aanmelden en inloggen kan opleveren en de volgende een visuele leidraad voor beide scenario's.

**Als uw app eindgebruiker teken ondersteunt omhoog (bijv. gratis proefperiode of freemium model)**: U kunt een **aanmeld -** knop waarmee gebruikers toegang hebben tot uw app met hun werk of hun persoonlijke account weergeven. Azure AD geven toestemming gevraagd de eerste keer dat zij toegang krijgen uw app tot.

**Als uw app zijn vereist voor dat alleen beheerders kunnen staan, of als uw app vereist organisatie-licentieverlening**: verwerving van gebruiker inloggen admin moet worden gescheiden. De **knop 'get dit app'** worden omgeleid beheerders om te melden en vervolgens vragen om toestemming voor de gebruikers in hun organisatie. Dit heeft het voordeel van eindgebruikers toestemming vragen om uw app te onderdrukken.

## <a name="visual-guidance-for-app-acquisition"></a>Visuele richtlijnen voor het ingaan van app

De koppeling "Kennismaking met de app" moet de gebruiker omleiden de Azure AD om toegang te verlenen (toestaan) pagina, zodat de beheerder van een organisatie te machtigen uw app toegang hebben tot de gegevens van de organisatie die door Microsoft wordt gehost. Meer informatie over het aanvragen van toegang worden besproken in het artikel [Integratie van toepassingen met Azure Active Directory](active-directory-integrating-applications.md) .

Als beheerders toestemming voor uw app, kunt ze toe te voegen aan hun gebruikers Office 365 app launcher ervaring (via de waffle en [https://portal.office.com/myapps](https://portal.office.com/myapps)). Als u wilt dat om deze mogelijkheid bekend te maken, kunt u termen zoals "Dit app aan uw organisatie toevoegen" en een knop als volgt weergeven:

![Toepassingstypen en scenario 's](./media/active-directory-branding-guidelines/add-to-my-org.png)

We raden echter aan dat u verklarende tekst schrijft niet vertrouwen op de knoppen. Bijvoorbeeld:
> *Als u al Office 365 of andere zakelijke service van Microsoft, kunt u gewoon < your_app_name > toegang verlenen tot de gegevens van uw organisatie. Hierdoor kunnen uw gebruikers met hun bestaande werk-accounts < your_app_name >.*


## <a name="visual-guidance-for-sign-in"></a>Visuele richtlijnen voor aanmelden
Een teken in knop waarmee gebruikers worden omgeleid naar het eindpunt-in die overeenkomt met het protocol dat u gebruikt om te integreren met AD Azure uw app moet worden weergegeven. De volgende sectie bevat informatie op welke knop moet er als volgt uitzien.

### <a name="pictogram-and-sign-in-with-microsoft"></a>Pictogram met "Meld u aan met Microsoft"
Dit is de koppeling van het Microsoft-logo en de voorwaarden van de 'Sign in met Microsoft' Azure AD onder een andere identiteit aanbieders een unieke aanduiding vormt uw app kan ondersteunen. Als er niet voldoende ruimte voor 'Sign in met Microsoft', is het ok om het naar "Inloggen" korter te maken.

![Toepassingstypen en scenario 's](./media/active-directory-branding-guidelines/sign-in-with-microsoft-light.png)

![Toepassingstypen en scenario 's](./media/active-directory-branding-guidelines/sign-in-light.png)

U kunt ook een kleurenschema Donker voor de knoppen.

![Toepassingstypen en scenario 's](./media/active-directory-branding-guidelines/sign-in-with-microsoft-dark.png)

![Toepassingstypen en scenario 's](./media/active-directory-branding-guidelines/sign-in-dark.png)

## <a name="branding-dos-and-donts"></a>Do's en dont huismerk

**Gebruik 'account voor werk of school' in combinatie met de knop 'Sign in met Microsoft' om extra uitleg waarmee eindgebruikers herkennen of ze het kunnen gebruiken.** Gebruik **geen** andere termen als "account voor de onderneming", "business account" of "corporate account".

Gebruik **geen** 'Office 365-ID' of 'Azure-ID'. Office 365 is ook de naam van een consument aanbieden van Microsoft Azure AD niet voor verificatie gebruiken.

Veranderen **niets** aan het Microsoft-logo.

Eindgebruikers van de merken Azure of Active Directory **niet** worden blootgesteld. Het is echter wel ok gebruik van deze termen met ontwikkelaars, IT-professionals en beheerders.

## <a name="navigation-dos-and-donts"></a>Navigatie's en dont 's

**Bieden een manier voor gebruikers afmelden en overschakelen naar een andere gebruikersaccount.** Terwijl de meeste mensen hebben een enkele persoonlijke rekening van Microsoft/Facebook/Google/Twitter, zijn mensen vaak gekoppeld aan meer dan één organisatie. Ondersteuning voor meerdere gebruikers ingelogd is binnenkort beschikbaar.
