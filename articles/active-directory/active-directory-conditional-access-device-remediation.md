<properties
    pageTitle="Het oplossen van toegangsproblemen Azure Active Directory | Microsoft Azure"
    description="Informatie over de stappen die u ondernemen kunt om het oplossen van problemen met de on line bronnen van uw organisatie."
    services="active-directory"
    keywords="voorwaardelijke toegang op basis van het apparaat, de registratie, de registratie, de registratie en MDM inschakelen"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/23/2016"
    ms.author="markvi"/>


# <a name="troubleshooting-for-azure-active-directory-access-issues"></a>Het oplossen van problemen met Azure Active Directory toegang

U probeert toegang te krijgen tot SharePoint Online intranet van uw organisatie en u krijgt een foutbericht 'toegang geweigerd' weergegeven. Wat voor werk doe je?

In dit artikel worden stappen oplossing voor problemen die kunnen helpen oplossen van problemen met de on line bronnen van uw organisatie.

Toegang tot problemen op te lossen Azure Active Directory (AD Azure), gaat u naar de sectie van het artikel die betrekking heeft op uw apparaatplatform:

-   Windows-apparaat
-   iOS-apparaat (Controleer terug spoedig voor hulp bij iPhones en iPads.)
-   Android apparaat (verontschuldigingen spoedig voor hulp bij Android telefoons en tablets.)

## <a name="access-from-a-windows-device"></a>Toegang vanaf een Windows-apparaat

Als het apparaat een van de volgende platforms, zoeken in de volgende secties voor het foutbericht dat wordt weergegeven wanneer u probeert toegang te krijgen tot een toepassing of service:

- Windows 10
- Windows 8.1
- Windows 8
- Windows 7
- Windows Server 2016
- Windows Server 2012 R2
- Windows Server 2012
- Windows Server 2008 R2

### <a name="device-is-not-registered"></a>Apparaat is niet geregistreerd.

Als het apparaat niet met Azure Active Directory is geregistreerd en de toepassing is beveiligd met een beleid op basis van het apparaat, kan een pagina met een van deze foutberichten worden weergegeven:

!["U kunt geen daar hier" berichten voor niet-geregistreerde apparaten] (./media/active-directory-conditional-access-device-remediation/01.png "Scenario")

Als het apparaat een domein behoren tot Active Directory in uw organisatie is, kunt u dit:

1.  Zorg ervoor dat u zich bij Windows aanmeldt met uw werk-account (uw Active Directory-account).
2.  Verbinding maken met het bedrijfsnetwerk via een virtueel particulier netwerk (VPN) of DirectAccess.
3.  Nadat u verbinding hebt gemaakt, drukt u op de Windows-toets + de L-toets te vergrendelen van uw Windows-sessie.
4.  Voer uw werk referenties om te ontgrendelen uw Windows-sessie.
5.  Wacht een paar minuten en probeer het vervolgens opnieuw toegang te krijgen tot de toepassing of service.
6.  Als u dezelfde pagina wordt weergegeven, klikt u op de koppeling **meer informatie** en neem contact op met de beheerder met de details.

Als uw apparaat geen deel uitmaakt van een domein en Windows 10 wordt uitgevoerd, hebt u twee opties:

- Azure AD Join wordt uitgevoerd
- Uw werk of school-account toevoegen aan Windows

Zie voor informatie over hoe deze opties verschillen, [met behulp van Windows 10-apparaten in uw werkplek](active-directory-azureadjoin-windows10-devices.md).

Azure AD Join uitvoert, gaat de volgende stappen uit voor het platform uw apparaat wordt uitgevoerd. (Azure AD Join is niet beschikbaar op Windows-telefoons).

**Update voor Windows 10 trouwdag**

1.  Open de app **Instellingen** .
2.  Klik op **Accounts** > **toegang tot werk of op school**.
3.  Klik op **verbinden**.
4.  Klik op **deelnemen aan dit apparaat Azure AD**.
5.  Om uw organisatie te verifiëren, meerledige verificatie bieden als dat wordt gevraagd en volg de stappen die worden weergegeven.
6.  Meldt u zich af en meldt u zich met uw account werk.
7.  Probeer opnieuw toegang te krijgen tot de toepassing.


**Update voor Windows 10 November 2015**

1.  Open de app **Instellingen** .
2.  Klik op **systeem** > **over**.
3.  Klik op **deelnemen aan AD Azure**.
4.  Om uw organisatie te verifiëren, meerledige verificatie bieden als dat wordt gevraagd en volg de stappen die worden weergegeven.
5.  Meldt u zich af en meldt u zich met uw werk-account (uw Azure AD-account).
6.  Probeer opnieuw toegang te krijgen tot de toepassing.

Uw werk of school als account wilt toevoegen, gaat u als volgt:

**Update voor Windows 10 trouwdag**

1.  Open de app **Instellingen** .
2.  Klik op **Accounts** > **toegang tot werk of op school**.
3.  Klik op **verbinden**.
4.  Om uw organisatie te verifiëren, meerledige verificatie bieden als dat wordt gevraagd en volg de stappen die worden weergegeven.
5.  Probeer opnieuw toegang te krijgen tot de toepassing.


**Update voor Windows 10 November 2015**

1.  Open de app **Instellingen** .
2.  Klik op **Accounts** > **uw rekeningen**.
3.  Klik op **toevoegen van werk of school account**.
4.  Om uw organisatie te verifiëren, meerledige verificatie bieden als dat wordt gevraagd en volg de stappen die worden weergegeven.
5.  Probeer opnieuw toegang te krijgen tot de toepassing.

Voer de volgende stappen uit als uw apparaat geen deel uitmaakt van een domein en Windows 8.1, om te doen een werkplek Join en inschrijven voor Microsoft Intune, wordt uitgevoerd:

1.  **PC-instellingen**openen.
2.  Klik op **netwerk** > **Werkplek**.
3.  Klik op **toevoegen**.
4.  Om uw organisatie te verifiëren, meerledige verificatie bieden als dat wordt gevraagd en volg de stappen die worden weergegeven.
5.  Klik **op inschakelen**.
6.  Probeer opnieuw toegang te krijgen tot de toepassing.


### <a name="browser-is-not-supported"></a>Browser wordt niet ondersteund.

U kan toegang worden geweigerd als u probeert toegang te krijgen tot een toepassing of service met behulp van een van de volgende browsers:

- Chrome, Firefox of een andere browser dan Microsoft Edge of Microsoft Internet Explorer in Windows 10 of Windows Server 2016
- Firefox in Windows 8.1, Windows 7, Windows Server 2012 R2, Windows Server 2008 R2 of Windows Server 2012

Hier ziet u een foutpagina die er als volgt uit:

![Bericht "U kunt geen daar hier" voor niet-ondersteunde browsers] (./media/active-directory-conditional-access-device-remediation/02.png "Scenario")

De enige oplossing is het gebruik van een browser die de toepassing voor uw apparaatplatform ondersteunt.

## <a name="next-steps"></a>Volgende stappen

[Azure Active Directory voorwaardelijke toegang](active-directory-conditional-access.md)
