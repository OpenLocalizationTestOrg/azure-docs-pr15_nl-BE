<properties
    pageTitle="Azure AD verbinden sync: het beheren van de Azure AD-account | Microsoft Azure"
    description="In dit onderwerp worden de Azure AD-account te herstellen."
    services="active-directory"
    keywords="AADSTS70002, AADSTS50054, het opnieuw instellen van het wachtwoord voor de synchronisatie van Azure AD verbinden Connector service-account"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/01/2016"
    ms.author="billmath"/>

# <a name="azure-ad-connect-sync-how-to-manage-the-azure-ad-service-account"></a>Azure AD verbinden sync: de Azure AD-account beheren
De serviceaccount die door de Connector Azure AD moet gratis zijn. Als u de referenties opnieuw instellen, wordt de in dit onderwerp voor u. Bijvoorbeeld, als er per ongeluk een globale beheerder het wachtwoord opnieuw instellen op de serviceaccount met PowerShell.

## <a name="reset-the-credentials"></a>De referenties opnieuw instellen
Als de service-account die is gedefinieerd op de verbindingslijn Azure AD geen contact met Azure AD vanwege verificatieproblemen met, kan het wachtwoord opnieuw worden ingesteld.

1. Aanmelden bij de server verbinden met Azure AD sync en PowerShell start.
2. Uitvoeren van `Add-ADSyncAADServiceAccount`.  
![PowerShell-cmdlet-addadsyncaadserviceaccount](./media/active-directory-aadconnectsync-howto-azureadaccount/addadsyncaadserviceaccount.png)
3. Azure AD globale admin referenties opgeven.

Deze cmdlet wordt het wachtwoord voor de serviceaccount en Azure advertentie en in de synchronisatie-engine bijwerken.

## <a name="known-issues-these-steps-can-solve"></a>Bekende problemen met die deze stappen oplossen kunnen.
In deze sectie wordt een lijst met fouten gemeld door klanten die met een referenties opnieuw instellen op de Azure AD-account verholpen waren.

-----------
Gebeurtenis 6900  
Een onverwachte fout opgetreden tijdens het verwerken van een melding van de wijziging wachtwoord:  
AADSTS70002: Fout bij het valideren referenties. AADSTS50054: Oud wachtwoord voor verificatie wordt gebruikt.

----------
Gebeurtenis 659  
Fout bij het ophalen van de configuratie van wachtwoord beleid voor synchronisatie. Microsoft.IdentityModel.Clients.ActiveDirectory.AdalServiceException:  
AADSTS70002: Fout bij het valideren referenties. AADSTS50054: Oud wachtwoord voor verificatie wordt gebruikt.

## <a name="next-steps"></a>Volgende stappen

**Van overzichtsonderwerpen**

- [Azure AD verbinden sync: begrijpen en synchronisatie aanpassen](active-directory-aadconnectsync-whatis.md)
- [Integratie van uw identiteiten op ruimten met Azure Active Directory](active-directory-aadconnect.md)
