<properties
    pageTitle="Voorwaardelijke toegang op basis van het apparaat het beleid instellen voor Azure Active Directory-toepassingen | Microsoft Azure"
    description="Een beleid van voorwaardelijke toegang op basis van het apparaat voor Azure AD-toepassingen instellen."
    services="active-directory"
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
    ms.date="09/14/2016"
    ms.author="markvi"/>


# <a name="set-device-based-conditional-access-policy-for-azure-active-directory-connected-applications"></a>Beleid voor voorwaardelijke toegang op basis van het apparaat voor Azure Active Directory-toepassingen instellen


Azure Active Directory (AD Azure) op basis van het apparaat voorwaardelijke toegang kunt u resources uit organisatie beschermen:

- Er wordt geprobeerd van onbekende of niet-beheerde apparaten.
- Apparaten die niet voldoen aan het beveiligingsbeleid van uw organisatie.

Zie voor een overzicht van de voorwaardelijke toegang, [voorwaardelijke toegang Azure Active Directory](active-directory-conditional-access.md).

U kunt voorwaardelijke toegang op basis van het apparaat beleid ter bescherming van deze toepassingen instellen:

- Office 365 SharePoint Online-sites en documenten van uw organisatie beveiligen
- Office 365 Exchange Online, e-mailadres van uw organisatie beveiligen
- Software als een service (SaaS)-toepassingen die zijn verbonden met Azure AD voor verificatie
- Toepassingen die worden gepubliceerd door middel van AD-toepassingsproxy Azure services ruimten

Als een beleid van voorwaardelijke toegang op basis van het apparaat in de Azure portal, gaat u naar de specifieke toepassing in de directory.


  ![Lijst met toepassingen in Azure portal directory] (./media/active-directory-conditional-access-policy-connected-applications/01.png "Toepassingen")


Selecteer de toepassing en klik vervolgens op het tabblad **configureren** om het beleid voor voorwaardelijke toegang instellen.  


  ![De toepassing configureren] (./media/active-directory-conditional-access-policy-connected-applications/02.png "Apparaat op basis van de regels voor access")




Een beleid van voorwaardelijke toegang op basis van het apparaat in de sectie **apparaat op basis van toegangsregels** in **Toegangsregels inschakelen**selecteren **op**.

Een beleid van voorwaardelijke toegang op basis van het apparaat bestaat uit drie onderdelen:

- **Van toepassing op**. Het bereik van het beleid van toepassing is op gebruikers.

- **Apparaat regels**. De voorwaarden een apparaat moet voldoen voordat deze toegang de toepassing tot.

- **Handhaving van de toepassing**. De clienttoepassingen (native versus browser) het beleid van toepassing op.

  ![De drie onderdelen van een beleid op basis van een apparaat] (./media/active-directory-conditional-access-policy-connected-applications/03.png "Apparaat op basis van de regels voor access")


## <a name="select-the-users-the-policy-applies-to"></a>Selecteer de gebruikers die het beleid van toepassing is op

U kunt in de sectie ' **Van toepassing op** de werkingssfeer van dit beleid is van toepassing op gebruikers selecteren.

U hebt twee opties wanneer u een scope van het beleid toegang voor gebruikers maken:

- **Alle gebruikers**. Het beleid van toepassing op alle gebruikers die toegang hebben tot de toepassing.

- **Groepen**. Het beleid voor gebruikers die lid van een specifieke groep zijn beperken.

![Beleid toepassen op alle gebruikers of aan een groep] (./media/active-directory-conditional-access-policy-connected-applications/11.png "Van toepassing op")


 Als u wilt uitsluiten van een gebruiker van een beleid, schakel het selectievakje **behalve** in. Dit is handig wanneer u wilt machtigen voor een specifieke gebruiker tijdelijk toegang tot de toepassing. Deze optie, bijvoorbeeld als sommige gebruikers apparaten die niet klaar voor voorwaardelijke toegang zijn hebben. De apparaten kunnen niet worden geregistreerd, of zij zijn niet voldaan.


## <a name="select-the-conditions-that-devices-must-meet"></a>De voorwaarden waaraan hulpmiddelen moeten voldoen aan

**Apparaat regels** gebruiken om de voorwaarden voor een apparaat instellen moet voor toegang tot de toepassing voldoen.

U kunt voorwaardelijke toegang op basis van een apparaat instellen voor dit type apparaat:

- Update voor Windows 10 verjaardag, Windows 8.1 en Windows 7
- Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 en Windows Server 2008 R2
- iOS-apparaten (iPad, iPhone)
- Android-apparaten

Ondersteuning voor Mac is binnenkort beschikbaar.

  ![Beleid toepassen op apparaten] (./media/active-directory-conditional-access-policy-connected-applications/04.png "Toepassingen")

 >[AZURE.NOTE] Zie voor meer informatie over de verschillen tussen een domein behoren en Azure AD verbonden apparaten [met behulp van Windows 10-apparaten in uw werkplek](active-directory-azureadjoin-windows10-devices.md).

U hebt twee opties voor het apparaat regels:

- **Alle apparaten moeten compatibel zijn**. Alle platforms die toegang hebben tot de toepassing moeten compatibel zijn. Apparaten die worden uitgevoerd op de platforms die geen ondersteuning voor voorwaardelijke toegang op basis van het apparaat bieden de toegang geweigerd.

- **Alleen geselecteerde apparaten moeten compatibel zijn**. Alleen specifieke apparaat platforms moeten compatibel zijn. Andere platforms of andere platforms die toegang heeft tot de toepassing hebben toegang.

  ![Het bereik voor het apparaat regels] (./media/active-directory-conditional-access-policy-connected-applications/05.png "Toepassingen")

Azure AD verbonden apparaten zijn compatibel als ze zijn gemarkeerd als **compatibel** in de directory door Intune of met een mobiel apparaat van derden managementsysteem dat is geïntegreerd met AD Azure.

Compatibel is met een apparaat voor een domein behoren als:

- Het is geregistreerd met Azure Active Directory. Veel organisaties behandelen vertrouwde apparaten apparaten domein behoren.
- Het is gemarkeerd als **compatibel** in Azure AD door System Center Configuration Manager 2016.

 ![Domein behoren apparaten die compatibel zijn] (./media/active-directory-conditional-access-policy-connected-applications/06.png "Regels voor apparaat")

Windows persoonlijke apparaten zijn compatibel als ze zijn gemarkeerd als **compatibel** in de directory door Intune of met een mobiel apparaat van derden managementsysteem dat is geïntegreerd met AD Azure.

Niet-Windows-apparaten zijn compatibel als ze zijn gemarkeerd als **compatibel** in de directory door Intune.

 >[AZURE.NOTE] Zie [voorwaardelijke toegang Azure Active Directory](active-directory-conditional-access.md)voor meer informatie over het instellen van Azure AD apparaat voldoet in de verschillende beheersystemen.


U kunt één of meerdere apparaat platforms voor een beleid op basis van een apparaat. Dit omvat Android, iOS, Windows Mobile (Windows 8.1 telefoons en tablets) en Windows (alle andere Windows-apparaten, met inbegrip van alle Windows 10-apparaten).
Evaluatie van beleid doet zich alleen op de geselecteerde platforms. Als een apparaat dat toegang probeert niet actief is een van de geselecteerde platforms, het apparaat toegang tot de toepassing als de gebruiker toegang heeft. Geen apparaatbeleid wordt geëvalueerd.

![Platforms voor apparaat regels selecteren] (./media/active-directory-conditional-access-policy-connected-applications/07.png "Regels voor apparaat")


## <a name="set-policy-evaluation-for-a-type-of-application"></a>Evaluatie van beleid voor een toepassing instellen

Stel in de sectie **Tenuitvoerlegging van toepassing** , het type van het beleid wordt geëvalueerd voor de gebruiker of Apparaattoegang aanvragen.

U hebt twee opties voor het type toepassing wilt opnemen:

- Browser en toepassingen
- Alleen toepassingen

![Browser kiezen of eigen toepassingen] (./media/active-directory-conditional-access-policy-connected-applications/08.png "Toepassingen")

Selecteren om af te dwingen-beleid voor toepassingen, **voor de browser en toepassingen**. Vervolgens kunt u opnemen:

- Browsers (bijvoorbeeld Microsoft Edge in Windows 10) of Safari in iOS.
- Toepassingen die gebruikmaken van Active Directory verificatie bibliotheek (ADAL) op elk platform of die gebruik maken van de WebAccountManager (WAM) API in Windows 10.

>[AZURE.NOTE] Zie voor meer informatie over browserondersteuning en andere overwegingen voor een gebruiker die toegang heeft tot een apparaat op basis van certificaten autoriteit beveiligde toepassing [voorwaardelijke toegang Azure Active Directory](active-directory-conditional-access.md).

## <a name="help-protect-email-access-from-exchange-activesync-based-applications"></a>Toegang tot e-mail beschermen tegen Exchange ActiveSync-toepassingen

In Office 365 Exchange Online toepassingen, kunt u Exchange ActiveSync e-mail toegang tot toepassingen op basis van Exchange ActiveSync e-mail blokkeren.

![Exchange ActiveSync-opties voor naleving] (./media/active-directory-conditional-access-policy-connected-applications/09.png "Toepassingen")

![Een compatibel apparaat toegang tot e-mailadres is vereist] (./media/active-directory-conditional-access-policy-connected-applications/10.png "Toepassingen")


## <a name="next-steps"></a>Volgende stappen

- [Azure Active Directory voorwaardelijke toegang](active-directory-conditional-access.md)
