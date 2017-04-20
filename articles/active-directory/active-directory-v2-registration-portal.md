<properties
    pageTitle="App registratie Portal Help-onderwerpen | Microsoft Azure"
    description="Een beschrijving van verschillende functies in de Microsoft-portal Registratie van app."
    services="active-directory"
    documentationCenter=""
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="dastrock"/>

# <a name="app-registration-reference"></a>App registratie verwijzing
Dit document bevat de context en de beschrijvingen van de verschillende functies van de Microsoft-Portal registratie van App [https://apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList).

## <a name="my-applications"></a>Mijn toepassingen
Deze lijst bevat alle toepassingen voor gebruik met het eindpunt Azure AD v2.0 geregistreerd.  Deze toepassingen hebben de mogelijkheid om het aanmelden van gebruikers met zowel persoonlijke accounts van Microsoft-account en werk/school accounts van Azure Active Directory.  Zie voor meer informatie over het eindpunt Azure AD v2.0, onze [v2.0-overzicht](active-directory-appmodel-v2-overview.md).  Deze toepassingen kunnen ook worden gebruikt voor de integratie met Microsoft-account verificatie eindpunt, `https://login.live.com`.

## <a name="live-sdk-applications"></a>Live SDK toepassingen
Deze lijst bevat alle toepassingen geregistreerd voor gebruik alleen met Microsoft-account.  Ze zijn niet ingeschakeld voor gebruik met Active Directory Azure dan ook.  Dit is waar vindt u alle toepassingen die u eerder had geregistreerd bij de MSA developer-portal op `https://account.live.com/developers/applications`.  Alle functies die u eerder hebt uitgevoerd op `https://account.live.com/developers/applications` kunnen nu worden uitgevoerd in deze nieuwe portal `https://apps.dev.microsoft.com`.  Als u meer vragen over de toepassingen van Microsoft-account hebt, neem dan contact met ons.

## <a name="application-secrets"></a>Geheimen van toepassing
Geheimen van toepassing zijn referenties waardoor uw toepassing betrouwbare [clientverificatie](http://tools.ietf.org/html/rfc6749#section-2.3) met Azure AD uitvoeren.  In OAuth en OpenID verbinding, de geheimen van een toepassing wordt vaak genoemd een `client_secret`.  In het protocol versie 2.0, elke toepassing die een beveiligingssleutel voor een weblocatie adresseerbare ontvangt (met behulp van een `https` stelsel) een geheim toepassing moet gebruiken om zichzelf te identificeren naar Azure AD bij de aflossing van die het beveiligingstoken.  Bovendien een native client die krijgen tokens op een apparaat via een geheim toepassing voor het uitvoeren van verificatie van de client, om te voorkomen dat de opslag van geheimen in onveilige omgevingen wordt verboden.

Elke app bevat twee geldige aanvraag geheimen op elk gewenst moment in de tijd.  Door twee geheimen, hebt u de ablilty voor het uitvoeren van periodieke sleutel rollover in de hele omgeving van uw toepassing.  Zodra u het geheel van de toepassing naar een nieuw geheim hebt gemigreerd, mag het geheim van de oude verwijderen en inrichten van een nieuw.

Op dit moment mogen slechts twee soorten toepassingen geheimen in de portal Registratie van app.  **Nieuw wachtwoord genereren** kiezen genereren en een gedeeld geheim in de respectieve gegevensopslag, kunt u in uw toepassing worden opgeslagen.  **Sleutelpaar genereren** kiezen maakt een nieuw openbaar/persoonlijk sleutelpaar sleutel die kan worden gedownload en gebruikt voor de clientverificatie naar Azure AD.

## <a name="profile"></a>Profiel
De profielsectie van de portal Registratie van toepassing kan worden gebruikt voor het aanpassen van de aanmeldingspagina voor uw toepassing.  Op dit moment kunt u het teken in de pagina toepassing logo, gebruiksvoorwaarden privacyverklaring en de URL te wijzigen.  Het logo moet een transparante 48 x 48 of 50 x 50 pixels afbeelding in een GIF-, PNG- of JPEG-bestand dat is 15 KB of kleiner.  Probeer het wijzigen van de waarden en de resulterende ondertekening weergeven op pagina!

## <a name="live-sdk-support"></a>Live ondersteuning SDK
Als u "Live SDK Support" inschakelt, geen toepassing geheimen die u maakt in de Azure AD ingericht en gegevensopslag Microsoft-Account.  Hierdoor kan uw toepassing rechtstreeks te integreren met de service Microsoft-Account (login.live.com).  Als u wilt bouwen van een app met behulp van Microsoft-Account rechtstreeks (in plaats van het eindpunt van de v2.0 Azure AD), moet u ervoor zorgen dat Live SDK-ondersteuning is ingeschakeld.

Live SDK-ondersteuning uitschakelen zorgt ervoor dat het geheim van toepassing is opgesteld in de Azure AD-gegevens worden opgeslagen.  De gegevens van AD Azure winkel is uitgerust met enterprise-grade verordeningen waardoor voldoen aan bepaalde normen, zoals de naleving van de FISMA.  Als u Live SDK-ondersteuning inschakelt, kan de toepassing niet naleving van deze normen bereiken.

Als u alleen ooit het eindpunt Azure AD v2.0 gebruiken, kunt u veilig Live SDK-ondersteuning uitschakelen.

