<properties
    pageTitle="Ins meerdere fouten melden"
    description="Een rapport dat geeft aan gebruikers die zich heeft aangemeld dat na meerdere opeenvolgende aanmelding bij pogingen mislukte."
    services="active-directory"
    documentationCenter=""
    authors="SSalahAhmed"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="03/04/2016"
    ms.author="saah;kenhoff"/>

# <a name="sign-ins-after-multiple-failures"></a>Na meerdere mislukte aanmeldingen
Dit rapport geeft aan dat gebruikers die zich heeft aangemeld na meerdere opeenvolgende aanmelding bij pogingen mislukte. Mogelijke zijn oorzaken:

- Gebruiker had hun wachtwoord vergeten</li><li>Gebruiker is het slachtoffer van een succesvolle wachtwoord raden brute-force aanvallen

Resultaten van dit rapport wordt het aantal opeenvolgende mislukte aanmeldingspogingen gemaakt voor de succesvolle aanmelden en een tijdstempel die is gekoppeld aan de eerste succesvolle aanmelden weergegeven.

**Instellingen**: U kunt het minimum aantal opeenvolgende mislukte aanmelding bij pogingen moeten plaatsvinden voordat het kan worden weergegeven in het rapport. Wanneer u wijzigingen in deze instelling aanbrengt is het belangrijk te weten dat deze wijzigingen niet zal worden toegepast op bestaande mislukte tekenen modules die momenteel worden weergegeven in uw bestaand rapport. Zij zullen echter worden toegepast op alle toekomstige aanmeldingen. Dit rapport kan alleen worden gewijzigd door de beheerders van een licentie.


![Ins meerdere fouten melden](./media/active-directory-reporting-sign-ins-after-multiple-failures/signInsAfterMultipleFailures.PNG)
