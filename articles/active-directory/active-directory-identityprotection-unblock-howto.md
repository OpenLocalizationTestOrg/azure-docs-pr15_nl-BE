<properties
    pageTitle="Azure Active Directory identiteitsbeveiliging - blokkering opheffen van gebruikers | Microsoft Azure"
    description="Meer informatie over het deblokkeren van gebruikers die zijn geblokkeerd door een beleid voor identiteitsbeveiliging van Azure Active Directory."
    services="active-directory"
    keywords="Azure active directory identiteitsbeveiliging, blokkering gebruiker opheffen"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/20/2016"
    ms.author="markvi"/>

#<a name="azure-active-directory-identity-protection---how-to-unblock-users"></a>Azure Active Directory identiteitsbeveiliging - blokkering opheffen van gebruikers

U kunt met Azure Active Directory identiteit bescherming beleid om te verhinderen dat gebruikers als de geconfigureerde voorwaarden wordt voldaan. Normaal gesproken een geblokkeerde gebruiker contactpersonen helpdesk te worden opgeheven. Deze onderwerpen worden de stappen beschreven die u kunt uitvoeren om een geblokkeerde gebruiker de blokkering opheffen.


## <a name="determine-the-reason-for-blocking"></a>De reden voor het blokkeren van bepalen

U moet als eerste stap om de blokkering van een gebruiker te bepalen van het beleid dat door de gebruiker is geblokkeerd omdat de volgende stappen zijn afhankelijk van het type. Met Azure Active Directory identiteit bescherming kan een gebruiker ofwel worden geblokkeerd door een beleid-in gevaar of een risico voor gebruikersbeleid. 

Het soort beleid dat een gebruiker uit de kop in het dialoogvenster dat tijdens een aanmeldingspoging werd gepresenteerd aan de gebruiker is geblokkeerd, kunt u opvragen:

|Beleid | Gebruikersdialoogvenster|
|--- | --- |
|-In gevaar | ![Geblokkeerde aanmelden](./media/active-directory-identityprotection-unblock-howto/02.png) |
|Risico van de gebruiker | ![Geblokkeerde account](./media/active-directory-identityprotection-unblock-howto/104.png) |


Een gebruiker die wordt geblokkeerd door:

- Een beleid-in gevaar is ook bekend als verdachte aanmelden
- Een gebruikersbeleid risico wordt ook wel een account risico

 
## <a name="unblocking-suspicious-sign-ins"></a>Deblokkeren verdachte aanmeldingen

Als u wilt deblokkeren een verdachte aanmelden, hebt u de volgende opties:

1. **Aanmelden van een bekende locatie of apparaat** - een gemeenschappelijke reden voor geblokkeerde verdachte aanmeldingen zijn aanmeldingspogingen van onbekende locaties of apparaten. De gebruikers kunnen snel vaststellen of dit de reden van de blokkering door te proberen om aan te melden vanaf een vertrouwde locatie of apparaat.


3. **Uitsluiten van het beleid** - als u denkt dat de huidige configuratie van uw beleid aanmelden problemen voor specifieke gebruikers veroorzaakt, kunt u de gebruikers hiervan uitsluiten. Zie [risico - in Groepsbeleid](active-directory-identityprotection.md#sign-in-risk-policy) voor meer informatie.
 
4. **Beleid uitschakelen** - als u denkt de configuratie van het beleid dat dat wordt veroorzaakt door problemen voor alle gebruikers, kunt u dit beleid uitschakelt. Zie [risico - in Groepsbeleid](active-directory-identityprotection.md#sign-in-risk-policy) voor meer informatie.


## <a name="unblocking-accounts-at-risk"></a>Blokkering van de rekeningen risico

U blokkering risico, hebt u de volgende opties:

1. **Wachtwoord opnieuw instellen** - kunt u het wachtwoord opnieuw instellen. Zie [beveiligd wachtwoord handmatig opnieuw instellen](active-directory-identityprotection.md#manual-secure-password-reset) voor meer informatie.

2. **Negeren van alle risico's** - het gebruikersbeleid risico kan een gebruiker als de gebruiker geconfigureerde risiconiveau voor het blokkeren van toegang is bereikt. U kunt beperken dat een gebruiker het risiconiveau door handmatig sluiten risico's gerapporteerd. Zie [handmatig sluiten van risico's](active-directory-identityprotection.md#closing-risk-events-manually)voor meer informatie.

3. **Uitsluiten van het beleid** - als u denkt dat de huidige configuratie van uw beleid aanmelden problemen voor specifieke gebruikers veroorzaakt, kunt u de gebruikers hiervan uitsluiten. Zie [risico gebruikersbeleid](active-directory-identityprotection.md#user-risk-policy) voor meer informatie.
 
4. **Beleid uitschakelen** - als u denkt de configuratie van het beleid dat dat wordt veroorzaakt door problemen voor alle gebruikers, kunt u dit beleid uitschakelt. Zie [risico gebruikersbeleid](active-directory-identityprotection.md#user-risk-policy) voor meer informatie.




## <a name="next-steps"></a>Volgende stappen

 Wilt u meer weten over Azure AD identiteit bescherming? [Azure Active Directory identiteitsbeveiliging](active-directory-identityprotection.md)uitchecken.
 

