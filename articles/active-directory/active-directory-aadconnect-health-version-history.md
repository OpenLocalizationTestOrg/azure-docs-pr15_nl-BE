<properties
    pageTitle="Azure AD verbinding gezondheid versiegeschiedenis"
    description="Dit document beschrijft de releases voor Azure AD verbinding maken met de gezondheid en wat is opgenomen in deze versies."
    services="active-directory"
    documentationCenter=""
    authors="karavar"
    manager="samueld"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="vakarand"/>

# <a name="azure-ad-connect-health-version-release-history"></a>Azure AD verbinden gezondheid: Versie Release-geschiedenis

Azure AD verbinding maken met de gezondheid van de Azure Active Directory-team regelmatig bijgewerkt met nieuwe functies en functionaliteit. In dit artikel worden de versies en de functies die beschikbaar zijn.

## <a name="october-2016"></a>Oktober 2016
**Update Agent:**
- Azure AD verbinding Health agent voor AD FS \(versie 2.6.408.0\)
    1. Verbeteringen in het opsporen van client-IP-adressen in de verificatie-aanvragen
    2. Correcties die zijn gerelateerd aan meldingen
- Azure AD verbinding Health agent voor AD DS (versie 2.6.408.0)
    1. Correcties die verband houden met waarschuwingen.
- Azure AD verbinding Health agent voor Sync (versie 2.6.353.0) uitgebracht met Azure AD Connect versie 1.1.281.0
    1. De vereiste gegevens leveren voor de synchronisatie foutenrapporten
    2. Correcties die zijn gerelateerd aan meldingen

**Nieuwe functies van voorbeeld:**
- Foutenrapporten synchronisatie voor Azure AD verbinding

**Nieuwe functies:**
- Azure AD verbinding maken met de gezondheid voor AD FS - veld IP-adres vindt u in het rapport over top 50 gebruikers met onjuiste gebruikersnaam en wachtwoord.

## <a name="july-2016"></a>Juli 2016

**Nieuwe functies van voorbeeld:**

- [Azure AD verbinding gezondheid voor AD DS](active-directory-aadconnect-health-adds.md).


## <a name="january-2016"></a>Januari 2016


**Update Agent:**

- Azure AD verbinding Health agent voor AD FS (versie 2.6.91.1512)


**Nieuwe functies:**

- [Test Connectivity Tool voor Azure AD Health Agents verbinding](active-directory-aadconnect-health-agent-install.md#test-connectivity-to-azure-ad-connect-health-service)


## <a name="november-2015"></a>November 2015


**Nieuwe functies:**

- Ondersteuning voor [toegangsbeheer op basis van rollen](active-directory-aadconnect-health-operations.md#manage-access-with-role-based-access-control)


**Nieuwe functies van voorbeeld:**

- [Azure AD verbinding maken met de gezondheid voor synchronisatie](active-directory-aadconnect-health-sync.md).

**Vaste problemen:**

- Correcties voor fouten tijdens registraties agent gezien.

## <a name="september-2015"></a>September 2015

**Nieuwe functies:**

- Onjuiste gebruikersnaam wachtwoord rapport voor AD FS
- Ondersteuning van niet-geverifieerde HTTP-Proxy configureren
- Ondersteuning voor het configureren van de agent op Server core
- Verbeteringen voor de waarschuwingen voor AD FS
- Verbeteringen in Azure AD verbinding Health Agent voor AD FS voor connectiviteit en gegevens uploaden.


**Vaste problemen:**

- Correcties in gebruik inzichten voor AD FS fout typen.


## <a name="june-2015"></a>Juni 2015

**Eerste release van gezondheid verbinding Azure AD voor AD FS en de AD FS-Proxy.**

**Nieuwe functies:**

- Waarschuwingen voor AD FS en de AD FS-Proxy servers met e-mailberichten controleren.
- Eenvoudige toegang tot AD FS-topologie en patronen in de AD FS-prestatiemeteritems.
- De trend in een token aanvragen op de AD FS-servers die zijn gegroepeerd op toepassingen, verificatiemethoden, aanvragen netwerk locatie enz.
- Ontwikkeling van mislukte aanvragen voor AD FS-servers die zijn gegroepeerd op toepassingen, fout typen enz.
- Implementatie eenvoudiger Agent met Azure AD globale Admin referenties.  




## <a name="next-steps"></a>Volgende stappen
Meer informatie over [uw locatie op Monitor identiteit infrastructuur en synchronisatie diensten in de cloud](active-directory-aadconnect-health.md).
