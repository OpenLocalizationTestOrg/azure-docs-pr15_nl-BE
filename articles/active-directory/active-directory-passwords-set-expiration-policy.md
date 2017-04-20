<properties
    pageTitle="Verloopbeleid wachtwoord instellen in Azure Active Directory | Microsoft Azure"
    description="Meer informatie over het verloopbeleid controleren en wijzigen van gebruiker wachtwoord verlopen afzonderlijk of in bulk voor Azure Active directory wachtwoorden"
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="curtand"/>


# <a name="set-password-expiration-policies-in-azure-active-directory"></a>Wachtwoord verloopbeleid in Azure Active Directory instellen

> [AZURE.IMPORTANT] **Weet u hier omdat u ondervindt problemen met inloggen?** In dat geval [hier hoe u kunt wijzigen en uw eigen wachtwoord opnieuw instellen](active-directory-passwords-update-your-own-password.md).

Als een globale beheerder voor een cloud-service van Microsoft, kunt u de Microsoft Azure Active Directory-Module voor Windows PowerShell voor het instellen van wachtwoorden van gebruikers niet te laten verlopen. U kunt ook Windows PowerShell-cmdlets voor het verwijderen van de-verloopt nooit configuratie of om te zien welke gebruikers wachtwoorden zijn ingesteld op niet verlopen. Dit artikel bevat help voor cloud-services, zoals Microsoft Intune en Office 365, die gebaseerd op Microsoft Azure Active Directory voor identiteits- en services zijn.

  > [AZURE.NOTE] Alleen wachtwoorden voor gebruikersaccounts die niet zijn gesynchroniseerd via de directory-synchronisatie kunnen worden geconfigureerd niet verlopen. Zie de lijst van onderwerpen in de [Routekaart voor Directory-synchronisatie](https://msdn.microsoft.com/library/azure/hh967642.aspx)voor meer informatie over adreslijstsynchronisatie.

Voor het gebruik van Windows PowerShell-cmdlets, moet u eerst installeren ze.

## <a name="what-do-you-want-to-do"></a>Wat wilt u doen?

- [Het verloopbeleid voor een wachtwoord controleren](#how-to-check-expiration-policy-for-a-password)

- [Stel een wachtwoord verloopt](#set-a-password-to-expire)

- [Een wachtwoord instellen dat niet verloopt](#set-a-password-to-never-expire)

## <a name="how-to-check-expiration-policy-for-a-password"></a>Het verloopbeleid voor een wachtwoord controleren

1.  Verbinding maken met Windows PowerShell met administrator-referenties van uw bedrijf.

2.  Een van de volgende handelingen uit:

    - Als u wilt zien of een enkele gebruiker, wachtwoord verloopt nooit is ingesteld, de volgende cmdlet worden uitgevoerd met behulp van de UPN (user Principal name) (bijvoorbeeld, aprilr@contoso.onmicrosoft.com) of de gebruikers-ID van de gebruiker die u wilt controleren:`Get-MSOLUser -UserPrincipalName <user ID> | Select PasswordNeverExpires`

    - Overzicht van de instelling 'Wachtwoord verloopt nooit' voor alle gebruikers de volgende cmdlet worden uitgevoerd:`Get-MSOLUser | Select UserPrincipalName, PasswordNeverExpires`

## <a name="set-a-password-to-expire"></a>Stel een wachtwoord verloopt

1.  Verbinding maken met Windows PowerShell met administrator-referenties van uw bedrijf.

2.  Een van de volgende handelingen uit:

    - Het wachtwoord van een gebruiker instellen zodat het wachtwoord verloopt, voeren de volgende cmdlet met behulp van de UPN (user Principal name) of de ID van de gebruiker:`Set-MsolUser -UserPrincipalName <user ID> -PasswordNeverExpires $false`

    - Gebruik de volgende cmdlet stelt de wachtwoorden van alle gebruikers in de organisatie, zodat zij verloopt:`Get-MSOLUser | Set-MsolUser -PasswordNeverExpires $false`

## <a name="set-a-password-to-never-expire"></a>Stel een wachtwoord verloopt nooit

1. Verbinding maken met Windows PowerShell met administrator-referenties van uw bedrijf.

2.  Een van de volgende handelingen uit:

    - Als u wilt dat het wachtwoord van een gebruiker nooit verloopt, voeren de volgende cmdlet met behulp van de UPN (user Principal name) of de ID van de gebruiker:`Set-MsolUser -UserPrincipalName <user ID> -PasswordNeverExpires $true`

    - Als de wachtwoorden van alle gebruikers in een organisatie die nooit verloopt, voert u de volgende cmdlet:`Get-MSOLUser | Set-MsolUser -PasswordNeverExpires $true`

## <a name="next-steps"></a>Volgende stappen

* **Weet u hier omdat u ondervindt problemen met inloggen?** In dat geval [hier hoe u kunt wijzigen en uw eigen wachtwoord opnieuw instellen](active-directory-passwords-update-your-own-password.md).
