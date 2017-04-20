<properties
    pageTitle="Toepassingen die gebruikmaken van regels voor voorwaardelijke toegang in Azure Active Directory | Microsoft Azure"
    description="Azure Active Directory gecontroleerd met voorwaardelijke toegangsbeheer voor specifieke omstandigheden tijdens de verificatie van de gebruiker en de toepassing toegang te verlenen."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="10/26/2016"
    ms.author="markvi"/>

# <a name="applications-that-use-conditional-access-rules-in-azure-active-directory"></a>Toepassingen die gebruikmaken van regels voor voorwaardelijke toegang in Azure Active Directory

Regels voor voorwaardelijke toegang worden ondersteund in Azure Active Directory (AD Azure)-toepassingen aangesloten, vooraf geïntegreerde federatieve software als service (SaaS)-toepassingen, toepassingen die gebruikmaken van wachtwoord eenmalige aanmelding (SSO), line-of-business-toepassingen en toepassingen die gebruikmaken van AD-toepassingsproxy Azure. Zie voor een gedetailleerde lijst van toepassingen waarvoor u voorwaardelijke toegang kunt [Services ingeschakeld met voorwaardelijke toegang](active-directory-conditional-access-technical-reference.md#Services-enabled-with-conditional-access). Voorwaardelijke toegang werkt zowel met mobiele en desktop toepassingen die gebruikmaken van moderne verificatie. In dit artikel dekken we hoe toegang werkt in mobiele en desktop apps.

In toepassingen die gebruikmaken van moderne verificatie kunt u Azure AD-in pagina's. Een gebruiker wordt gevraagd met een aanmeldingspagina voor een meerledige verificatie. Een bericht wordt weergegeven als de gebruiker de toegang is geblokkeerd. Moderne verificatie is vereist voor het apparaat te verifiëren met Azure Active Directory, zodat het beleid van voorwaardelijke toegang op basis van het apparaat worden geëvalueerd.

Het is belangrijk om te weten welke toepassingen regels voor voorwaardelijke toegang en de stappen die u mogelijk moet ondernemen voor het beveiligen van de ingangspunten voor andere toepassingen kunnen gebruiken.

## <a name="applications-that-use-modern-authentication"></a>Toepassingen die gebruikmaken van moderne verificatie

> [AZURE.NOTE] Als u een beleid voor voorwaardelijke toegang in Azure AD met een gelijkwaardige in Office 365 hebt, moet u beide voorwaardelijke-beleid configureren. Dit zou van toepassing, bijvoorbeeld voorwaardelijke-beleid voor Exchange Online of SharePoint Online.

De volgende toepassingen ondersteunen voorwaardelijke toegang voor Office 365 en andere toepassingen Azure AD connected-service:

| Target-service  | Platform  | Toepassing                                                  |
|--------------|-----------------|----------------------------------------------------------------|
|Office 365 Exchange Online | Windows 10|Kalender-mail/personen app, 2016 voor Outlook, Outlook 2013 (met moderne verificatie), Skype voor bedrijven (met moderne verificatie)|
|Office 365 Exchange Online| Windows 8.1, Windows 7 |Outlook 2016, Outlook 2013 (met moderne verificatie), Skype voor bedrijven (met moderne verificatie)|
|Office 365 Exchange Online|iOS, Android|  Outlook mobile app|
|Office 365 Exchange Online|Mac OS X| Outlook 2016 voor meerledige verificatie en locatie. ondersteuning voor Groepsbeleid op basis van het apparaat is gepland voor de toekomst, Skype voor Business support is gepland voor de toekomst|
|Office 365 SharePoint Online|Windows 10| Office 2016 apps, universele Office apps, Office 2013 (met moderne verificatie), OneDrive voor ondersteuning van Business app (volgende generatie Sync Client of NGSC) die zijn gepland voor de toekomst zijn gepland voor de toekomst zijn gepland voor de toekomst van SharePoint app ondersteuning groepen Office-ondersteuning|
|Office 365 SharePoint Online|Windows 8.1, Windows 7|Office 2016 apps, Office 2013 (met moderne verificatie), OneDrive voor Business app (Groove sync client)|
|Office 365 SharePoint Online|iOS, Android|  Office mobile apps |
|Office 365 SharePoint Online|Mac OS X| Office 2016 apps voor meerledige verificatie en locatie. ondersteuning voor Groepsbeleid op basis van het apparaat is gepland voor de toekomst|
|Office 365 Yammer|10 voor Windows, iOS en Android | Office Yammer app|
|Dynamics CRM|Windows 10, Windows 8.1, Windows 7, iOS en Android | Dynamics CRM app|
|PowerBI service|Windows 10, Windows 8.1, Windows 7, iOS en Android | PowerBI app|
|Azure Remote App-service|Windows 10, Windows 8.1, Windows 7, iOS, Android en Mac OS X |Azure Remote app|
|Elke service mijn Apps app|Android en iOS|Elke service mijn Apps app |


## <a name="applications-that-do-not-use-modern-authentication"></a>Toepassingen die geen van moderne verificatie gebruikmaken

Op dit moment moet u andere methoden gebruiken voor het blokkeren van toegang tot toepassingen die geen van moderne verificatie gebruikmaken. Toegangsregels voor toepassingen die geen van moderne verificatie gebruikmaken worden niet afgedwongen door voorwaardelijke toegang. Dit is voornamelijk een tegenprestatie voor de toegang tot Exchange en SharePoint. De meeste eerdere versies van de apps oudere access control-protocollen gebruiken.

### <a name="control-access-in-office-365-sharepoint-online"></a>Toegangsbeheer in Office 365 SharePoint Online
U kunt oudere protocollen voor toegang tot SharePoint uitschakelen met behulp van de cmdlet Set-SPOTenant. Deze cmdlet gebruikt om te voorkomen dat de Office-clients die gebruikmaken van de verificatieprotocollen niet modern tot SharePoint Online bronnen.

**Voorbeeld van de opdracht**:    `Set-SPOTenant -LegacyAuthProtocolsEnabled $false`

### <a name="control-access-in-office-365-exchange-online"></a>Toegang in Office 365 Exchange Online beheren

Exchange biedt twee hoofdcategorieën van protocollen. Bekijk de volgende opties en selecteer vervolgens het beleid dat geschikt is voor uw organisatie.

-   **Exchange ActiveSync**. Voorwaardelijke-beleid voor een meerledige verificatie en de locatie zijn voor Exchange ActiveSync niet standaard afgedwongen. U moet toegang tot deze diensten te beschermen door het configureren van Exchange ActiveSync-beleid rechtstreeks of door het blokkeren van Exchange ActiveSync met behulp van Active Directory Federation Services (AD FS) regels.
-   **Legacy-protocollen**. U kunt oudere protocollen met AD FS blokkeren. Dit blokkeert de toegang tot oudere Office-clients, zoals Office 2013 zonder moderne verificatie is ingeschakeld, en eerdere versies van Office.


### <a name="use-ad-fs-to-block-legacy-protocol"></a>AD FS gebruiken om het oudere protocol blokkeren

De regels van het volgende voorbeeld kunt u oudere protocol toegang op het niveau van AD FS blokkeren. Kies uit twee algemene configuraties.

#### <a name="option-1-allow-exchange-activesync-and-allow-legacy-apps-but-only-on-the-intranet"></a>Optie 1: Exchange ActiveSync toestaan en kunnen oudere toepassingen, maar alleen op het intranet

Door de volgende drie regels toepassen op de AD FS partij vertrouwen voor Microsoft Office 365 identiteit Platform, Exchange ActiveSync-verkeer en browser en moderne verificatieverkeer vertrouwt, toegang te hebben. Oude apps zijn geblokkeerd voor het extranet.

##### <a name="rule-1"></a>Regel 1

    @RuleName = “Allow all intranet traffic”
    c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "true"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");

##### <a name="rule-2"></a>Regel 2

    @RuleName = “Allow Exchange ActiveSync ”
    c1:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application", Value == "Microsoft.Exchange.ActiveSync"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");

##### <a name="rule-3"></a>Regel 3

    @RuleName = “Allow extranet browser and browser dialog traffic”
    c1:[Type == " http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] &&
    c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value =~ "(/adfs/ls)|(/adfs/oauth2)"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");

#### <a name="option-2-allow-exchange-activesync-and-block-legacy-apps"></a>Optie 2: Exchange ActiveSync toestaan en blokkeren van oudere toepassingen

Door de volgende drie regels toepassen op de AD FS partij vertrouwen voor Microsoft Office 365 identiteit Platform, Exchange ActiveSync-verkeer en browser en moderne verificatieverkeer vertrouwt, toegang te hebben. Oudere toepassingen worden vanaf elke locatie geblokkeerd.

##### <a name="rule-1"></a>Regel 1

    @RuleName = “Allow all intranet traffic only for browser and modern authentication clients”
    c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "true"] &&
    c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value =~ "(/adfs/ls)|(/adfs/oauth2)"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");


##### <a name="rule-2"></a>Regel 2

    @RuleName = “Allow Exchange ActiveSync”
    c1:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application", Value == "Microsoft.Exchange.ActiveSync"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");


##### <a name="rule-3"></a>Regel 3

    @RuleName = “Allow extranet browser and browser dialog traffic”
    c1:[Type == " http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] &&
    c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value =~ "(/adfs/ls)|(/adfs/oauth2)"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");
