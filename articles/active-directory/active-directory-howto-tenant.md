<properties
    pageTitle="Het verkrijgen van een huurder Azure AD | Microsoft Azure"
    description="Het verkrijgen van een huurder Azure Active Directory voor de registratie en het bouwen van toepassingen."
    services="active-directory"
    documentationCenter=""
    authors="dstrockis"
    manager="terrylan"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="09/28/2015"
    ms.author="dastrock"/>

# <a name="how-to-get-an-azure-active-directory-tenant"></a>Het verkrijgen van een huurder Azure Active Directory

In Azure Active Directory (AD Azure) is de [huurder](https://msdn.microsoft.com/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant) van een vertegenwoordiger van een organisatie.  Het is een apart exemplaar van de Azure AD-service die een organisatie ontvangt en wanneer deze zich voor een Microsoft-cloud service zoals Azure, Microsoft Intune of Office 365 aanmeldt bezit.  Elke huurder Azure AD is afzonderlijk en los van andere huurders Azure AD.  

Een huurder ook nieuwste de gebruikers in een bedrijf en de informatie over deze - hun wachtwoorden, gebruikersprofielgegevens, machtigingen, enzovoort.  Het bevat ook groepen, toepassingen en andere informatie die deel uitmaakt van een organisatie en de beveiliging.

Als gebruikers Azure AD aan te melden uw toepassing, moet u de toepassing in een huurder van uw eigen registreren.  Uitgeven van een toepassing in een huurder Azure AD is **absoluut gratis**.  In feite maakt de meeste ontwikkelaars verschillende huurders en toepassingen voor experimenten, ontwikkeling, staging- en testdoeleinden.  Organisaties die zich aanmeldt voor en verbruikt uw toepassing kunnen eventueel licenties kopen als ze willen profiteren van de geavanceerde directory.

Dus hoe doet u dit over het verkrijgen van een huurder Azure AD?  Het proces is mogelijk een beetje anders als u:

- [Een bestaand Office 365-abonnement hebt](#use-an-existing-office-365-subscription)
- [Een bestaande Azure abonnement is gekoppeld aan een Microsoft-Account hebt](#use-an-msa-azure-subscription)
- [Een bestaande Azure abonnement is gekoppeld aan een organisatie-account hebt](#use-an-organizational-azure-subscription)
- [Geen van de bovenstaande & wilt maken](#start-from-scratch)

## <a name="use-an-existing-office-365-subscription"></a>Gebruik een bestaand Office 365-abonnement
Als u een bestaand Office 365-abonnement hebt, al hebt u een huurder Azure AD! U kunt de [Azure portal](https://portal.azure.com) aanmelden met uw account O365 en Azure AD gebruiken.

## <a name="use-an-msa-azure-subscription"></a>Gebruik een MSA Azure-abonnement
Als u hebt eerder aangemeld voor een abonnement op Azure met uw afzonderlijke Microsoft-Account, al hebt u een huurder!  Wanneer u zich bij de [Azure Portal aanmeldt](https://portal.azure.com), wordt u automatisch aangemeld bij de huurder standaard. U gebruikt deze huurder naar wens - gratis maar kunt u een organisatie-administrator-account maken.

Ga hiervoor als volgt te werk.  U kunt ook een nieuwe huurder maken en een beheerder maken in deze huurder na een soortgelijk proces.

1.  Log in op de [Azure Portal](https://portal.azure.com) met uw particuliere account
2.  Ga naar de sectie 'Azure Active Directory' in de portal (gevonden in de linker navigatiebalk onder **Meer Services**)
3.  U moet automatisch worden aangemeld bij 'Default Directory' als niet, kunt u mappen overschakelen door te klikken op de accountnaam van uw in de rechterbovenhoek.
4.  Kies **een gebruiker toevoegen**in de sectie **Taken snel** .
5.  In het formulier gebruiker toevoegen kunt u de volgende gegevens bevatten:

    - Naam: (Kies een geschikte waarde)
    - Gebruikersnaam: (Kies een naam voor deze beheerder)
    - Profiel: (Vul de juiste waarden voor voornaam, laatste naam, functie en afdeling)
    - Rol: Globale beheerder

6.  Wanneer u klaar bent met het formulier toevoegen en het tijdelijke wachtwoord voor de nieuwe administratieve gebruiker ontvangt, moet u dit wachtwoord opnemen terwijl u inloggen met deze nieuwe gebruiker moet om het wachtwoord te wijzigen. U kunt het wachtwoord ook rechtstreeks aan de gebruiker en met een alternatief e-mailbericht verzenden.
7.  Klik op **maken** om een nieuwe gebruiker maken.
8.  Als u wilt het tijdelijke wachtwoord wijzigen, log in [https://login.microsoftonline.com](https://login.microsoftonline.com) met deze nieuwe gebruikersaccount en het wachtwoord te wijzigen.


## <a name="use-an-organizational-azure-subscription"></a>Gebruik een Azure-organisatie-abonnement
Als u hebt eerder aangemeld voor een abonnement op Azure met uw organisatie-account, al hebt u een huurder!  In de [Portal Azure](https://portal.azure.com)zult u een huurder wanneer u navigeert naar 'Meer Services' en "Azure Active Directory."  U bent vrij gebruiken deze huurder wens naar. 


## <a name="start-from-scratch"></a>Opnieuw beginnen
Als u alle bovenstaande onzin aan u is, geen probleem.  Ga naar [https://account.windowsazure.com/organization](https://account.windowsazure.com/organization) voor Azure aanmelden bij een nieuwe organisatie.  Als u de procedure hebt uitgevoerd, hebt u uw eigen Azure AD huurder met de domeinnaam die u hebt gekozen tijdens het aanmelden van.  In de [Azure Portal](https://portal.azure.com)vindt u de huurder door te navigeren naar 'Azure Active Directory' in de linkerhand nav.

Als onderdeel van het proces van aanmelden voor Azure, u moet creditcardgegevens opgeven.  U kunt verdergaan met het vertrouwen - niet brengt voor het uitgeven van toepassingen in Azure AD of maken van nieuwe huurders.
