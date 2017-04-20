<properties
   pageTitle="Integratie van toepassingen met Azure Active Directory | Microsoft Azure"
   description="Meer informatie over het toevoegen, bijwerken of verwijderen van een toepassing in Azure Active Directory (AD Azure)."
   services="active-directory"
   documentationCenter=""
   authors="bryanla"
   manager="mbaldwin"
   editor="mbaldwin" />
<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/29/2016"
   ms.author="mbaldwin;bryanla" />

# <a name="integrating-applications-with-azure-active-directory"></a>Integratie van toepassingen met Azure Active Directory

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Ontwikkelaars van ondernemingen en leveranciers van software as a service (SaaS) kunnen ontwikkelen commerciële cloud services of zakelijke toepassingen die kunnen worden geïntegreerd met Azure Active Directory (AD Azure) om veilig inloggen en autorisatie voor hun diensten. Voor het integreren van een toepassing of service met Azure Active Directory, moet een ontwikkelaar de gegevens over de toepassing eerst registreren met Azure Active Directory via de klassieke Azure portal.

In dit artikel wordt beschreven hoe u wilt toevoegen, bijwerken of verwijderen van toepassingen in Azure AD. U leert over de verschillende soorten toepassingen die kunnen worden geïntegreerd met AD Azure, het configureren van uw toepassingen toegang krijgen tot andere bronnen zoals web-API's en nog veel meer.

Zie voor meer informatie over de twee Azure AD-objecten die staan voor een geregistreerde toepassing en de relatie tussen hen, [toepassing en Service Principal objecten](active-directory-application-objects.md); Zie voor meer informatie over de huisstijl richtlijnen die moet u bij het ontwikkelen van toepassingen met Azure Active Directory, [Huisstijl richtlijnen voor geïntegreerde Apps](active-directory-branding-guidelines.md).

## <a name="adding-an-application"></a>Een toepassing toe te voegen

Elke toepassing die u wil gebruiken, de mogelijkheden van Azure AD moet eerst worden geregistreerd in een huurder Azure AD. Het registratieproces bestaat uit met Azure AD gedetailleerde informatie over de toepassing, bijvoorbeeld de URL waar dit zich, de URL naar antwoorden verzenden nadat een gebruiker is geverifieerd, de URI die de app, enzovoort identificeert.

Als u een webtoepassing die moeten-in ondersteuning voor gebruikers in Azure AD maakt, kunt u de onderstaande instructies te volgen. Als uw toepassing moet referenties of machtigingen voor toegang tot een web API of moet toestaan dat gebruikers andere Azure AD huurders toegang tot het [bijwerken van een toepassing](#updating-an-application) Zie de sectie om verder te gaan met het configureren van uw toepassing.

### <a name="to-register-a-new-application-in-the-azure-classic-portal"></a>Voor het registreren van een nieuwe toepassing in de klassieke Azure portal

1. Log in om de [Azure klassieke portal](https://manage.windowsazure.com).

1. Klik op het pictogram Active Directory in het linker menu en klik vervolgens op de gewenste map op.

1. Klik in het bovenste menu op **toepassingen**. Als er geen apps zijn toegevoegd aan de directory, wordt deze pagina alleen weergegeven toevoegen een App maken. Klik op de link of u kunt ook klikken op op de knop **toevoegen** in de werkbalk.

1. Op de pagina Wat wilt u pagina, klikt u op de koppeling naar **het ontwikkelen van mijn organisatie toepassing toevoegen**.

1. Op de Tell ons over de pagina met uw toepassing, moet u een naam voor uw toepassing en geven het type toepassing dat u met Azure Active Directory registreert.  U kunt kiezen uit een [WebClient toepassing](active-directory-dev-glossary.md#client-application) / [web resource/API](active-directory-dev-glossary.md#resource-server) -toepassing (kan ook fungeren als beide) of een [native client](active-directory-dev-glossary.md#native-client) -toepassing. Zodra u klaar bent, klikt u op het pictogram in de rechterbenedenhoek van de pagina.

1. Op de eigenschappenpagina van App bieden de aanmelding URL en de URI van App-ID als u een webtoepassing of alleen de Redirect URI voor een native clienttoepassing wilt registreren en klik u vervolgens op het selectievakje in de onderste rechterhoek van de pagina.

1. De toepassing is toegevoegd, en u komt dan op de pagina Quick Start voor uw toepassing. Afhankelijk van uw toepassing een webpagina of oorspronkelijke toepassing, ziet u verschillende opties voor het toevoegen van extra mogelijkheden voor uw toepassing. Wanneer u uw toepassing hebt toegevoegd, kunt u beginnen met het bijwerken van uw toepassing om gebruikers te ondertekenen in access web API's in andere toepassingen of meerdere huurder toepassing (die andere organisaties toegang te krijgen tot uw toepassing kunt) configureren.

>[AZURE.NOTE] Standaard wordt de registratie van nieuwe toepassing geconfigureerd zodat gebruikers van de map aan te melden uw toepassing.

## <a name="updating-an-application"></a>Een toepassing bijwerken

Nadat uw aanvraag is geregistreerd met Azure Active Directory, kan deze moeten worden bijgewerkt bieden toegang tot web-API's, worden beschikbaar gesteld in andere organisaties, en nog veel meer. Deze sectie beschrijft verschillende manieren waarin u wilt uw toepassing verder configureren. We zullen eerst met een overzicht van het Framework toestemming is belangrijk om te weten als u resource/API toepassingen bouwt die wordt verbruikt door de clienttoepassingen die zijn ontwikkeld door ontwikkelaars in uw organisatie of een andere organisatie.

Zie voor meer informatie over de manier waarop verificatie werkt in Azure AD, [Scenario's voor Azure advertentie voor verificatie](active-directory-authentication-scenarios.md).

### <a name="overview-of-the-consent-framework"></a>Overzicht van het kader van de toestemming

Azure AD toestemming kader eenvoudiger te ontwikkelen met meerdere huurder web- en native client die toegang moeten krijgen tot web API's die worden beveiligd door een huurder Azure AD verschilt van de plaats waar de clienttoepassing wordt geregistreerd. Deze web-API's zijn de Graph API, Office 365 en andere Microsoft-services, naast uw eigen web-API's. Het kader is gebaseerd op een gebruiker of een administrator toestemming geven aan een toepassing waarin u wordt gevraagd in hun directory, waarbij toegang tot Active directory-gegevens worden geregistreerd.

Bijvoorbeeld, als een webtoepassing aanroepen van een Office 365-web API of de resource van toepassing moet voor het lezen van agenda-informatie over de gebruiker, die gebruiker moet toestemming voor de clienttoepassing. Nadat toestemming is gegeven, worden de clienttoepassing kunnen het Office 365-web API namens de gebruiker belt en de agenda-informatie wanneer dat nodig is.

Het kader van de toestemming is gebaseerd op OAuth 2.0 en de verschillende stromen, zoals code subsidie en client verificatiereferenties verlenen met behulp van openbare of vertrouwelijke clients. Met behulp van OAuth 2.0 maakt Azure AD het mogelijk te maken van verschillende soorten toepassingen, zoals op een telefoon, Tablet PC, server of een webtoepassing toegang te krijgen tot de vereiste bronnen.

Zie voor meer gedetailleerde informatie over de toestemming framework [2.0 OAuth in Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx), [Verificatie-scenario's voor AD Azure](active-directory-authentication-scenarios.md)en Office 365 onderwerp [Wat verificatie met Office 365-API's](https://msdn.microsoft.com/office/office365/howto/common-app-authentication-tasks).

#### <a name="example-of-the-consent-experience"></a>Voorbeeld van de ervaring van de toestemming

De volgende stappen ziet u hoe de toestemming works voor de ontwikkelaar van toepassingen en de gebruiker ondervindt.

1. Stel de machtigingen voor die uw toepassing vereist is met behulp van de vervolgkeuzelijsten in de machtigingen voor het beheer van andere toepassingen op de configuratiepagina van uw webtoepassing clients in de klassieke Azure portal.

    ![Machtigingen voor andere toepassingen](./media/active-directory-integrating-applications/permissions.png)

1. Rekening houden met de machtigingen van uw toepassing wordt bijgewerkt, de toepassing wordt uitgevoerd en een gebruiker is voor de eerste keer gebruiken. Als de toepassing niet al een access- of vernieuwen verkregen token, de toepassing moet gaat u naar het eindpunt van de Azure AD vergunning te verkrijgen van een vergunning code die kan worden gebruikt bij het aanschaffen van een nieuwe toegang en vernieuwen token.

1. Als de gebruiker niet al is geverifieerd, wordt ze gevraagd voor aanmelding bij AD Azure.

    ![Gebruiker of beheerder inloggen naar Azure AD](./media/active-directory-integrating-applications/useradminsignin.png)

1. Nadat de gebruiker heeft zich aangemeld, bepaalt Azure AD of de gebruiker moet een pagina toestemming worden weergegeven. Deze bepaling is gebaseerd op de vraag of de gebruiker (of de beheerder van de organisatie) reeds de toestemming voor de toepassing verleende heeft. Toestemming heeft geen toestemming gekregen, Azure AD wordt de gebruiker om toestemming gevraagd als de vereiste machtigingen beschikt, moet deze functie wordt weergegeven. De verzameling machtigingen die worden weergegeven in het dialoogvenster toestemming zijn hetzelfde als wat u in de machtigingen voor andere besturingselementen voor toepassingen in de klassieke Azure portal is geselecteerd.

    ![Ervaring van de gebruiker toestemming](./media/active-directory-integrating-applications/userconsent.png)

1. Nadat de gebruiker toestemming verleent, wordt een autorisatiecode terug naar de toepassing, die kan worden ingewisseld bij het aanschaffen van een toegangstoken en token vernieuwen. Zie voor meer informatie over deze overdracht, de [toepassing naar de sectie API webpagina web](active-directory-authentication-scenarios.md#web-application-to-web-api) in [Verificatie-scenario's voor Azure AD](active-directory-authentication-scenarios.md).

### <a name="configuring-a-client-application-to-access-web-apis"></a>Een clienttoepassing toegang tot web API's configureren

Om een web/vertrouwelijke clienttoepassing kunnen deelnemen aan een vergunning verlenen stroom waarvoor verificatie vereist is (en een toegangstoken verkrijgen), moet het veilige referenties vast. De standaardverificatiemethode wordt ondersteund door de Azure portal is de Client-ID + symmetrische sleutel. In dit gedeelte wordt uitgelegd hoe de configuratiestappen vereist om de referenties van de geheime sleutel van de client.

Bovendien voordat een client kan toegang tot een web API beschikbaar gesteld door de toepassing van een resource (ie: Azure AD Graph API), het kader van de toestemming ervoor zorgen de client verkrijgt de toekenning van de machtiging vereist, op basis van de machtigingen die zijn aangevraagd. Standaard kunt alle toepassingen machtigingen van Azure Active Directory (Graph API) en Azure Service Management API met Azure AD "teken op inschakelen en lezen gebruikersprofiel" toestemming is standaard geselecteerd. Als de clienttoepassing wordt geregistreerd in een Office 365 Azure AD huurder, machtigingen voor SharePoint en Exchange Online en web-API's worden ook beschikbaar zijn voor selectie. U kunt kiezen uit [twee typen machtigingen](active-directory-dev-glossary.md#permissions) in het vervolgkeuzemenu's naast de gewenste web API:

- Machtigingen: De clienttoepassing toegang nodig heeft tot het web API als zelf (geen gebruikerscontext). Dit type machtiging administrator toestemming nodig en is ook niet beschikbaar voor native client-toepassingen.

- Overgedragen machtigingen: De clienttoepassing toegang nodig heeft tot het web API als de aangemelde gebruiker, maar door de geselecteerde machtiging beperkte toegang. Dit type machtiging kan worden verleend door een gebruiker, tenzij de machtiging administrator toestemming vereist is geconfigureerd.

#### <a name="to-add-credentials-or-permissions-to-access-web-apis"></a>Referenties of machtigingen voor toegang tot web API's toe te voegen

1. Meld u aan bij de [Azure klassieke portal.](https://manage.windowsazure.com)

1. Klik op het pictogram Active Directory in het linker menu en klik vervolgens op de gewenste map op.

1. In het bovenste menu, klik op **toepassingen**en klik op de toepassing die u wilt configureren. De pagina Quick Start wordt met eenmalige aanmelding en andere configuratie-informatie weergegeven. Klik op de koppeling **configureren** aan de bovenkant van de pagina die u u naar de pagina configuratie van de toepassing gaat.

1. Om een geheime sleutel om referenties van uw webtoepassing toevoegen, Ga naar de "Sleutels"-sectie.  

    - Klik eerst op "Selecteer duur" vervolgkeuzelijst en selecteer een 1 of 2 jaar duur. 
    - Vervolgens ziet u een nieuwe rij, met de "Geldig vanaf" en 'Vervalt op' datum ingevuld.
    - De meest rechtse kolom bevat de waarde van de sleutel, nadat u de wijzigingen in de configuratie (onder) opgeslagen. Zorg ervoor dat terugkomen op deze afdeling en kopiëren nadat u opslaan raakt, zodat u deze voor gebruik in de clienttoepassing tijdens verificatie tijdens runtime.

2. Machtiging(en) voor bron-API's van de client toevoegen, Ga naar de sectie 'Machtigingen voor andere toepassingen'. 
    - Klik eerst op de knop 'Toepassing toevoegen'.
    - Gebruik de vervolgkeuzelijst 'Show' om het type van de resources die u kiezen wilt uit.
    - De eerste kolom kunt u selecteren uit de beschikbare bron-toepassingen in de map waarin een web API. De resource die u geïnteresseerd bent in, klik op het vinkje in de rechterbenedenhoek.
    - Nadat u hebt geselecteerd, ziet u de bron toegevoegd aan de lijst 'Machtigingen voor andere toepassingen'.
    - Met behulp van de vervolgkeuzelijsten ' toepassing "en 'Overgedragen machtigingen', selecteert u de gewenste machtigingen voor de clienttoepassing.

1. Wanneer u klaar bent, klikt u op de knop **Opslaan** op de opdrachtbalk. Als u een sleutel voor uw toepassing hebt opgegeven, wordt het ook gegenereerd nadat u op opslaan.

>[AZURE.NOTE] Ook automatisch op de knop Opslaan te klikken, stelt de machtigingen voor de toepassing in de map op basis van de machtigingen voor andere toepassingen die u hebt geconfigureerd.  U ziet deze machtigingen op het tabblad van de eigenschappen van toepassing.

### <a name="configuring-a-resource-application-to-expose-web-apis"></a>Een resource-toepassing toegankelijk maken van web-API's configureren

U kunt een web API ontwikkelen en beschikbaar voor clienttoepassingen maken door access [scopes](active-directory-dev-glossary.md#scopes) , [rollen](active-directory-dev-glossary.md#roles)en bloot. Een juist geconfigureerde web API bestaat net als de andere Microsoft web-API's, met inbegrip van de Graph API en de Office 365-API's beschikbaar. Access scopes, rollen en zijn toegankelijk via het [manifest van de toepassing](active-directory-dev-glossary.md#application-manifest), een JSON-bestand met de configuratie van de identiteit van uw toepassing.  

In de volgende sectie wordt beschreven hoe u access scopes, doordat het manifest van de toepassing van de resource weergegeven.

#### <a name="adding-access-scopes-to-your-resource-application"></a>Scopes toegang toe te voegen aan uw toepassing resource

1. Log in om de [Azure klassieke portal](https://manage.windowsazure.com).

1. Klik op het pictogram Active Directory in het linker menu en klik vervolgens op de gewenste map op.

1. In het bovenste menu, klik op **toepassingen**en klik vervolgens op de brontoepassing die u wilt configureren. De pagina Quick Start wordt met eenmalige aanmelding en andere configuratie-informatie weergegeven.

1. Klik op de knop **beheren manifest** in de werkbalk en selecteer **manifest te downloaden**.

1. Het manifestbestand van JSON toepassing open en knooppunt "oauth2Permissions" vervangen door het volgende fragment van JSON. Dit fragment is een voorbeeld van hoe u een scope bekend als 'gebruikersimitatie", waarmee de eigenaar van een resource voor een clienttoepassing een type gedelegeerde toegang tot een bron kan worden blootgesteld. Zorg ervoor dat u de tekst en waarden voor uw eigen toepassing wijzigen:

        "oauth2Permissions": [
        {
            "adminConsentDescription": "Allow the application full access to the Todo List service on behalf of the signed-in   user",
            "adminConsentDisplayName": "Have full access to the Todo List service",
            "id": "b69ee3c9-c40d-4f2a-ac80-961cd1534e40",
            "isEnabled": true,
            "type": "User",
            "userConsentDescription": "Allow the application full access to the todo service on your behalf",
            "userConsentDisplayName": "Have full access to the todo service",
            "value": "user_impersonation"
            }
        ],

    De id-waarde moet een nieuw gegenereerde GUID die u maakt met behulp van een [programma voor het genereren van GUID](https://msdn.microsoft.com/library/ms241442%28v=vs.80%29.aspx) of via programmacode. Hiermee geeft u een unieke id voor de machtiging die wordt weergegeven door het web API. Zodra de client correct is ingesteld voor het aanvragen van toegang tot uw web API en de web-API-aanroepen, wordt er een token OAuth 2.0 JWT met de claim scope (scp) is ingesteld op de waarde erboven, in dit geval is de user_impersonation aanwezig.

    >[AZURE.NOTE] Extra scopes later zo nodig kan worden blootgesteld. U kunt dat meerdere scopes die zijn gekoppeld aan allerlei verschillende functies kan worden blootgesteld aan uw web-API. U kunt nu toegang tot het web API bepalen met behulp van de scope (scp) claim in het token van de ontvangen OAuth 2.0 JWT.

1. Sla het bijgewerkte JSON-bestand en upload deze door te klikken op de knop **beheren manifest** in de opdrachtbalk, **manifest te uploaden**, bladeren naar uw bijgewerkte manifestbestand selecteren en vervolgens te selecteren. Eenmaal geüpload, wordt uw web-API is nu geconfigureerd om te worden gebruikt door andere toepassingen in de map.

#### <a name="to-verify-the-web-api-is-exposed-to-other-applications-in-your-directory"></a>Om te controleren of het web API beschikbaar wordt gesteld aan andere toepassingen in de directory

1. In het bovenste menu, klik op **toepassingen**, selecteert u de gewenste clienttoepassing die u wilt configureren, toegang tot het web API en klik vervolgens op **configureren**.

1. Ga naar de machtigingen voor andere toepassingen-sectie. Klik op de vervolgkeuzelijst selecteren toepassing en kunt u kunt selecteren van het web API die u zojuist de machtiging voor beschikbaar. Selecteer de nieuwe machtiging overgedragen machtigingen in de vervolgkeuzelijst.

![Todo lijst machtigingen worden weergegeven.](./media/active-directory-integrating-applications/listpermissions.png)

#### <a name="more-on-the-application-manifest"></a>Meer informatie over het manifest van de toepassing
Manifest voor de toepassing fungeert eigenlijk als een mechanisme voor het bijwerken van de Toepassingsentiteit, waarin alle kenmerken van een Azure AD identiteit de configuratie van toepassing, met inbegrip van de API-toegang bereiken die we besproken. Zie de [documentatie van de toepassing van Graph API entiteit](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity)voor meer informatie op de entiteit voor de toepassing. Daarin vindt u volledige naslaginformatie over de toepassing entiteitleden gebruikt om machtigingen opgeven voor de API:  

- een lid van de appRoles die een verzameling van [AppRole](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#approle-type) entiteiten die kunnen worden gebruikt voor het definiëren van de **Machtigingen** voor een web-API  
- een lid van de oauth2Permissions die een verzameling van [OAuth2Permission](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permission-type) entiteiten die kunnen worden gebruikt voor het definiëren van de **Overgedragen machtigingen** voor een web-API

Voor meer informatie over toepassing Zie manifest concepten in het algemeen, [wat het toepassingsmanifest Azure Active Directory](active-directory-application-manifest.md).

### <a name="accessing-the-azure-ad-graph-and-office-365-apis"></a>Toegang tot de API's voor Office 365 en Azure AD grafiek

Zoals eerder gezegd bloot/hebt u toegang tot API's op uw eigen toepassingen resource kunt u ook uw clienttoepassing toegang tot API's die door Microsoft-bronnen bijwerken.  De Azure AD Graph API, die heet 'Azure Active Directory' in de lijst met machtigingen voor andere toepassingen, is standaard beschikbaar voor alle toepassingen die met Azure Active Directory zijn geregistreerd. Als u de clienttoepassing in een huurder Azure AD die in Office 365 is ingericht registreert, u kunt ook toegang tot alle rechten die door de API's van verschillende bronnen voor Office 365.

Voor een volledige bespreking van access-scopes weergegeven door:  

- Azure AD Graph API, raadpleegt u de machtiging [bereiken | Graph API concepten](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-permission-scopes) artikel.
- Office 365-API's, Zie het artikel [verificatie en autorisatie met algemene instemming Framework](https://msdn.microsoft.com/office/office365/howto/application-manifest) . Zie [de Office 365-ontwikkelomgeving instellen](https://msdn.microsoft.com/office/office365/HowTo/setup-development-environment) voor de grotere bespreking over het bouwen van een clienttoepassing die is geïntegreerd met Office 365-API's.

>[AZURE.NOTE] Vanwege een beperking kunnen native client-toepassingen alleen oproepen in de Azure AD Graph API als ze met de machtiging 'Toegang krijgen tot de directory van uw organisatie'.  Deze beperking geldt niet voor webtoepassingen.

### <a name="configuring-multi-tenant-applications"></a>Huurder met meerdere toepassingen configureren

Wanneer u een toepassing naar Azure AD toevoegt, kunt u uw toepassing kan alleen worden geopend door gebruikers in uw organisatie. U kunt ook uw toepassing kan worden geopend door gebruikers in externe organisaties. Deze twee toepassingstypen worden één huurder en de huurder met meerdere toepassingen genoemd. U kunt de configuratie van een toepassing één huurder te maken van een toepassing met meerdere huurder, die in deze sectie wordt beschreven onder wijzigen.

Het is belangrijk dat u Let op de verschillen tussen een enkele huurder en de huurder met meerdere toepassingen:  

- Een toepassing één huurder is bedoeld voor gebruik in een organisatie. Ze zijn meestal een line of business (LoB) toepassing die is geschreven door de ontwikkelaar van een onderneming. Een toepassing één huurder moet alleen worden geopend door gebruikers in een bepaalde map en daardoor alleen moet worden ingericht in een bepaalde map.
- Een toepassing met meerdere huurder is bedoeld voor gebruik in veel organisaties. Ze zijn een software as a service (SaaS)-webtoepassing meestal geschreven door een onafhankelijke softwareleverancier (ISV). Toepassingen met meerdere huurder moeten worden ingericht in elke map waarin ze worden gebruikt, die de gebruiker of beheerder toestemming registreren ze ondersteund via het framework Azure AD toestemming nodig. Merk op dat alle native client-toepassingen met meerdere huurder standaard, die worden geïnstalleerd op een apparaat van de eigenaar van de resource. Zie het overzicht van de sectie Framework staan hierboven voor meer informatie in het kader van de toestemming.

#### <a name="enabling-external-users-to-grant-your-application-access-to-their-resources"></a>Externe gebruikers uw toepassing toegang verlenen tot hun bronnen inschakelen

Als u een toepassing die u beschikbaar wilt stellen aan uw klanten of partners buiten uw organisatie schrijft, moet u voor het bijwerken van de toepassingsdefinitie in de klassieke Azure portal.

>[AZURE.NOTE] Als u meerdere huurder inschakelt, moet u ervoor zorgen dat App-ID-URI van de toepassing in een gecontroleerde domein behoort. Bovendien moet de retour-URL begint met https://. Zie voor meer informatie, de [toepassing en Service Principal-objecten](active-directory-application-objects.md).

Om toegang te krijgen tot uw app voor externe gebruikers: 

1. Meld u aan bij de [Azure klassieke portal.](https://manage.windowsazure.com)

1. Klik op het pictogram Active Directory in het linker menu en klik vervolgens op de gewenste map op.

1. In het bovenste menu, klik op **toepassingen**en klik op de toepassing die u wilt configureren. De pagina Quick Start weergegeven met configuratie-opties.

1. Vouw de sectie **Configureren met meerdere huurder toepassing** van de Quick Start en klik vervolgens op de koppeling **nu configureren** in de sectie toegang inschakelen. De eigenschappenpagina van de toepassing wordt weergegeven.

1. Klik op de knop **Ja** naast toepassing is met meerdere pachters en klik vervolgens op de knop **Opslaan** op de opdrachtbalk.

Nadat u de bovenstaande wijziging hebt aangebracht, gebruikers en beheerders in andere organisaties kan worden uw toepassing toegang verlenen tot de map en andere gegevens.

#### <a name="triggering-the-azure-ad-consent-framework-at-runtime"></a>Het framework Azure AD toestemming tijdens runtime activeren

Als u wilt gebruiken in het kader van de toestemming, moeten meerdere huurder clienttoepassingen OAuth 2.0 met vergunning aanvragen. [Voorbeelden van code](https://azure.microsoft.com/documentation/samples/?service=active-directory&term=multi-tenant) zijn beschikbaar om aan te tonen hoe een webtoepassing, oorspronkelijke toepassing of server/daemon toepassing vraagt vergunning codes en toegangstokens web API's aanroepen.

Uw webtoepassing kan bieden ook de aanmeldingsprocedure voor gebruikers. Als u een aanmeldingsprocedure bieden, verwacht wordt dat de gebruiker klikt op op een bord omhoog dat zal de Azure AD-OAuth2.0 van de browser omleiden toestaan eindpunt of een eindpunt userinfo OpenID verbinden. Eindpunten kunnen de toepassing voor informatie over de nieuwe gebruiker met een controle op de id_token. Na de fase van de aanmelding de gebruiker krijgt zoals hierboven getoond in het overzicht van de sectie Framework instemming toestemming gevraagd.

U kunt ook kan uw webtoepassing bieden ook een ervaring die beheerders van 'Mijn bedrijf aanmelden'. Deze ervaring zou ook de gebruiker omleiden naar de Azure AD OAuth 2.0 eindpunt machtigen. In dit geval, het doorgeven van een prompt = admin_consent parameter naar het eindpunt van de machtigen te dwingen de administrator toestemming ervaring, waarbij de beheerder toestemming voor hun organisatie verleent. Alleen een gebruiker die wordt geverifieerd met een account die tot de rol van globale beheerder behoort krijgt toestemming; anderen wordt een foutbericht weergegeven. Op geslaagde toestemming te geven, bevat het antwoord admin_consent = true. Bij het inruilen van een toegangstoken ook ontvangt u een id_token met informatie over de organisatie en de beheerder die zich voor uw toepassing aanmeldde.

### <a name="enabling-oauth-20-implicit-grant-for-single-page-applications"></a>Impliciete 2.0 verlenen OAuth inschakelen voor één pagina toepassingen

Één pagina van de toepassing (kuuroorden) zijn meestal met een JavaScript-zware front-end die wordt uitgevoerd in de browser van de toepassing web API wordt teruggebeld einde voor het uitvoeren van de bedrijfslogica gestructureerd. Voor kuuroorden in Azure AD gehost, kunt u OAuth 2.0 impliciete subsidie Azure AD aan de gebruiker worden geverifieerd en krijgen een token dat u beveiligde gesprekken van JavaScript-client van de toepassing naar de back-end web API kunt. Nadat de gebruiker kan toestemming heeft verleend, kan dit verificatieprotocol dezelfde verkrijgen van tokens beveiligde gesprekken tussen de client en andere web API-bronnen configureren voor de toepassing worden gebruikt. Zie [Wat de stroom OAuth2 impliciete verlening in Azure Active Directory ](active-directory-dev-understanding-oauth2-implicit-grant.md)voor meer informatie over de impliciete toestemming verlenen en help u bepalen of deze geschikt voor uw scenario toepassing is.

Impliciete subsidie OAuth 2.0 is standaard uitgeschakeld voor toepassingen. OAuth 2.0 impliciete subsidie kunt u inschakelen voor uw toepassing door de `oauth2AllowImplicitFlow`' ' waarde in het [manifest van de toepassing](active-directory-application-manifest.md), een JSON-bestand met de configuratie van de identiteit van uw toepassing.

#### <a name="to-enable-oauth-20-implicit-grant"></a>Impliciete subsidie OAuth 2.0 inschakelen 

1. Meld u aan bij de [Azure klassieke portal.](https://manage.windowsazure.com)
1. Klik op het pictogram **Active Directory** in het linker menu en klik vervolgens op de gewenste map op.
1. In het bovenste menu, klik op **toepassingen**en klik op de toepassing die u wilt configureren. De pagina Quick Start wordt met eenmalige aanmelding en andere configuratie-informatie weergegeven.
1. Klik op de knop **beheren manifest** in de werkbalk en selecteer **manifest te downloaden**.
Het manifestbestand van JSON toepassing open en stelt u de waarde 'oauth2AllowImplicitFlow' op 'true'. Standaard is "false".

    `"oauth2AllowImplicitFlow": true,`

1. Sla het bijgewerkte JSON-bestand en upload deze door te klikken op de knop **beheren manifest** in de opdrachtbalk, **manifest te uploaden**, bladeren naar uw bijgewerkte manifestbestand selecteren en vervolgens te selecteren. Eenmaal geüpload, wordt uw web API is nu geconfigureerd voor het gebruik OAuth 2.0 impliciete toekennen om gebruikers te verifiëren.


### <a name="legacy-experiences-for-granting-access"></a>Oude ervaringen voor het verlenen van toegang

Deze sectie beschrijft de ervaring van de oudere toestemming vóór 12 maart 2014. Deze ervaring wordt nog steeds ondersteund en wordt hieronder beschreven. Voordat de nieuwe functionaliteit kunt u alleen de volgende machtigingen verlenen:

- Gebruikers van hun organisatie aanmelden

- Aanmelden van gebruikers en lees de directory-gegevens van hun bedrijf (als de toepassing alleen)

- Aanmelden van gebruikers en gegevens lezen en schrijven van hun organisatie directory (als de toepassing alleen)

U kunt de stappen in de [Huurder met meerdere Web-toepassingen ontwikkelen met Azure Active Directory](https://msdn.microsoft.com/library/azure/dn151789.aspx) -toegang voor nieuwe toepassingen die zijn geregistreerd in Azure AD. Het is belangrijk om te weten het nieuwe kader voor toestemming kan veel krachtigere toepassingen en kan ook gebruikers toe te staan deze toepassingen in plaats van alleen beheerders.

#### <a name="building-the-link-that-grants-access-for-external-users-legacy"></a>Bouwen van de koppeling die verleent toegang voor externe gebruikers (verouderd)

Om externe gebruikers zich aanmelden voor uw app in hun organisatie-accounts, moet u voor het bijwerken van uw app zodat een knop die is gekoppeld aan de pagina op Azure AD waarmee ze toegang te verlenen. Huisstijl richtlijnen voor aanmelding voor deze knop wordt besproken in het onderwerp [Huisstijl richtlijnen voor geïntegreerde toepassingen](active-directory-branding-guidelines.md) . Nadat de gebruiker verleent of weigert u toegang, wordt de pagina Azure AD verlenen toegang tot de browser omleiden naar uw app met een antwoord. Zie voor meer informatie over eigenschappen voor toepassingen, [toepassingsobjecten en beginselen van de Service](active-directory-application-objects.md).

De subsidie access-pagina wordt gemaakt door AD Azure en in vindt u een koppeling naar deze pagina van de configuratie van uw app in de klassieke Azure portal. Als u naar de pagina configuratie, klik op de koppeling van **toepassingen** in het bovenste menu van de huurder Azure AD, klikt u op de toepassing die u wilt configureren en klik vervolgens op **configureren** in het menu boven aan de pagina Quick Start.

De koppeling voor uw toepassing wordt ziet er zo uit: `http://account.activedirectory.windowsazure.com/Consent.aspx?ClientID=058eb9b2-4f49-4850-9b78-469e3176e247&RequestedPermissions=DirectoryReaders&ConsentReturnURL=https%3A%2F%2Fadatum.com%2FExpenseReport.aspx%3FContextId%3D123456`. De volgende tabel worden de onderdelen van de koppeling:

|Parameter|Beschrijving|
|---|---|
|ClientId|Vereist. De Client-ID die u hebt verkregen als onderdeel van uw app toe te voegen.|
|RequestedPermissions|Dit is optioneel. Toegangsniveau die uw app aanvraagt, dat wordt weergegeven aan de gebruiker die de app toegang verlenen. Als niet wordt opgegeven, wordt het gewenste toegangsniveau voor eenmalige aanmelding alleen standaard. De andere opties zijn DirectoryReaders en DirectoryWriters. Zie de toegangsniveaus toepassing voor meer informatie over deze toegangsniveaus.|
|ConsentReturnUrl|Dit is optioneel. De URL die u wilt dat de subsidie toegang tot antwoord aan. Deze URL-gecodeerd moet zijn en moet in hetzelfde domein bevinden als de URL antwoord is geconfigureerd in de definitie van de app. Als u niet opgeeft, wordt het antwoord verlenen toegang omgeleid naar de geconfigureerde URL van uw antwoord.|

Een ConsentReturnUrl die los staat van de URL van het antwoord op te geven, kunt uw app voor de uitvoering van afzonderlijke logica die het antwoord op een andere URL van de antwoord-URL (die normaal gesproken SAML-tokens voor teken worden verwerkt op) kunt verwerken. U kunt ook opgeven dat extra parameters in de ConsentReturnURL gecodeerde URL; Deze zal worden doorgegeven als queryreeksparameters aan uw app bij omleiding.  Dit mechanisme kan onderhouden aanvullende informatie of een aanvraag voor een subsidie voor de toegang van uw app koppelen aan de reactie van Azure AD worden gebruikt.

#### <a name="grant-access-user-experience-and-response-legacy"></a>De ervaring van de gebruiker toegang verlenen en antwoord (verouderd)

Wanneer een toepassing wordt omgeleid naar de koppeling verlenen toegang, de volgende afbeeldingen laten zien wat de gebruiker ervaring.

Als de gebruiker is nog niet aangemeld, wordt ze gevraagd om dit te doen:

![Aanmelden bij AAD](./media/active-directory-integrating-applications/signintoaad.png)

Als de gebruiker is geverifieerd, wordt Azure AD de gebruiker omleiden naar de subsidie access-pagina:

![Toegang verlenen](./media/active-directory-integrating-applications/grantaccess.png)

>[AZURE.NOTE] Alleen de bedrijfsbeheerder van de externe organisatie kunt toegang verlenen tot uw app. Als de gebruiker geen beheerder van een bedrijf is, krijgt zij de optie om e-mail te verzenden naar de bedrijfsbeheerder van hun aan te vragen dat deze app toegang worden verleend.

Nadat de klant heeft toegang voor uw app door te klikken op toegang verlenen, of als u ze hebt geweigerd door op Annuleren te klikken, Azure AD een antwoord naar de ConsentReturnUrl of de URL van uw geconfigureerde antwoord stuurt. Dit antwoord bevat de volgende parameters:

|Parameter|Beschrijving|
|---|---|
|TenantId|De unieke ID van de organisatie in Azure AD die voor uw app toegang heeft verleend. Deze parameter wordt alleen worden opgegeven als de klant toegang verleend.|
|Toestemming|De waarde zal worden ingesteld op verleend als de app is toegang verleend of geweigerd als de aanvraag is afgewezen.|

Extra parameters worden geretourneerd naar de toepassing als deze zijn opgegeven als onderdeel van de ConsentReturnUrl URL gecodeerd. De volgende is een voorbeeld van de reactie op een toegangsaanvraag subsidie die geeft aan dat de aanvraag is goedgekeurd en bevat een context-id die is opgegeven in het access-request subsidie: `https://adatum.com/ExpenseReport.aspx?ContextID=123456&Consent=Granted&TenantId=f03dcba3-d693-47ad-9983-011650e64134`.

>[AZURE.NOTE] Het antwoord toegang verlenen geen een beveiligingstoken voor de gebruiker. de app moet je inloggen de gebruiker afzonderlijk.

Hier volgt een voorbeeld van de reactie op een subsidie toegangsaanvraag is geweigerd:`https://adatum.com/ExpenseReport.aspx?ContextID=123456&Consent=Denied`

#### <a name="rolling-app-keys-for-uninterrupted-access-to-the-graph-api-legacy"></a>Rolling app toetsen om ononderbroken toegang tot de Graph API (verouderd)

Gedurende de levensduur van uw app moet u de sleutels die u gebruikt wanneer u belt in Azure AD aan te schaffen een toegangstoken de Graph API aanroepen als u wilt wijzigen.  Wijzigen van sleutels in het algemeen worden onderverdeeld in twee categorieën: emergency rollover waar de sleutel is aangetast of een rollover wanneer uw huidige sleutel is verlopen. De volgende procedure moet worden gevolgd om te voorzien van uw app continu toegang hebben terwijl u uw sleutels (voornamelijk voor het tweede geval) vernieuwen.

1. In de klassieke Azure portal, klikt u op de map huurder, **toepassingen** in het bovenste menu, klik op de toepassing die u wilt configureren. De pagina Quick Start wordt met eenmalige aanmelding en andere configuratie-informatie weergegeven.

1. Klik op **configureren** in het bovenste menu voor een overzicht van de eigenschappen van uw app, en ziet u een lijst van uw sleutels.

1. Onder toetsen, klik op de vervolgkeuzelijst **Selecteer duur** zegt 1 of 2 jaar pick en klik op **Opslaan** op de opdrachtbalk. Hiermee wordt een nieuwe wachtwoordsleutel voor uw app gegenereerd. Kopieer dit nieuwe wachtwoordsleutel. Op dit moment kan zowel de bestaande en nieuwe sleutel worden gebruikt door uw app voor een toegangstoken van Azure AD.

1. Ga terug naar uw app en de configuratie om te starten met het nieuwe wachtwoord bijwerken. Zie [De Graph API Azure AD opvraagt](https://msdn.microsoft.com/library/azure/dn151791.aspx) voor een voorbeeld van waar u deze update moet gebeuren.

1. U moet nu zetten deze wijziging in de productieomgeving: controle van de eerst op een knooppunt, alvorens in de rest.

1. Zodra de update voltooid in de productie-implementatie is, bent u vrij te komen terug naar de klassieke Azure portal en verwijdert u de oude sleutel.

#### <a name="changing-app-properties-after-enabling-access-legacy"></a>App-eigenschappen wijzigen na het inschakelen van toegang (verouderd)

Wanneer u voor externe gebruikers toegang tot uw app hebt ingeschakeld, kan u nog wijzigingen aanbrengen in de eigenschappen van uw app in de klassieke Azure portal blijven. Klanten die al toegang tot uw app hebt toegekend voordat u de app wijzigingen ziet die wijzigingen doorgevoerd bij het bekijken van details van uw app in de klassieke Azure portal echter niet. Nadat de app is beschikbaar gesteld aan klanten, moet u zeer zorgvuldig te werk wanneer u bepaalde wijzigingen aanbrengt. Bijvoorbeeld als u de URI App ID bijwerkt, vervolgens bestaande klanten die toegang voordat u deze wijziging niet aan te melden uw app met behulp van de rekeningen van hun bedrijf of school.

Als u een wijziging in de RequestedPermissions aanbrengt voor het aanvragen van een hoger toegangsniveau bestaande klanten met app-functionaliteit die u nu maakt gebruik van nieuwe API-aanroepen met deze toegenomen krijgt toegangsniveau mogelijk de toegang is geweigerd, reactie van de Graph API.  Uw app moet voor deze gevallen en vraag de klant toegang verlenen tot uw app bij de aanvraag voor een verhoogde toegangsniveau.

## <a name="removing-an-application"></a>Een toepassing verwijderen

In deze sectie wordt beschreven hoe een toepassing verwijderen uit uw huurder Azure AD.

### <a name="removing-an-application-authored-by-your-organization"></a>Verwijderen van een toepassing die is geschreven door uw organisatie
Dit zijn de toepassingen die worden weergegeven onder het filter "Toepassingen mijn bedrijf eigenaar is van" op de hoofdpagina 'Toepassingen' voor uw huurder Azure AD. Dit zijn toepassingen die u hebt geregistreerd via de portal voor Azure klassieke handmatig of via programmering via PowerShell of de Graph API in technische termen. Meer in het bijzonder, worden zij vertegenwoordigd door een toepassing en Service Principal object in uw huurder. Zie de [toepassing en Service Principal-objecten](active-directory-application-objects.md) voor meer informatie.

#### <a name="to-remove-a-single-tenant-application-from-your-directory"></a>Een toepassing één huurder verwijderen uit de map

1. Meld u aan bij de [Azure klassieke portal.](https://manage.windowsazure.com)

1. Klik op het pictogram Active Directory in het linker menu en klik vervolgens op de gewenste map op.

1. In het bovenste menu, klik op **toepassingen**en klik op de toepassing die u wilt configureren. De pagina Quick Start wordt met eenmalige aanmelding en andere configuratie-informatie weergegeven.

1. Klik op de knop **verwijderen** op de werkbalk.

1. Klik op **Ja** in het bevestigingsbericht.

#### <a name="to-remove-a-multi-tenant-application-from-your-directory"></a>Een toepassing met meerdere huurder verwijderen uit de map

1. Meld u aan bij de [Azure klassieke portal.](https://manage.windowsazure.com)

1. Klik op het pictogram Active Directory in het linker menu en klik vervolgens op de gewenste map op.

1. In het bovenste menu, klik op **toepassingen**en klik op de toepassing die u wilt configureren. De pagina Quick Start wordt met eenmalige aanmelding en andere configuratie-informatie weergegeven.

1. Over de toepassing is met meerdere huurder sectie, klikt u op **Nee**. Hiermee zet u de toepassing kan één huurder, maar de toepassing blijven bestaan in een organisatie die al heeft ingestemd met het.

1. Klik op de knop **verwijderen** op de werkbalk.

1. Klik op **Ja** in het bevestigingsbericht.

### <a name="removing-a-multi-tenant-application-authorized-by-another-organization"></a>Verwijderen van een toepassing meerdere huurder is toegestaan door een andere organisatie
Dit zijn een subset van de toepassingen die voor de huurder Azure AD, speciaal onder de lijst 'Toepassingen mijn bedrijf eigenaar' die worden weergegeven onder het filter "Toepassingen gebruikt voor mijn bedrijf" op de pagina 'Toepassingen' belangrijkste. In technische termen zijn huurder met meerdere toepassingen die zijn geregistreerd tijdens de toestemming. Meer in het bijzonder, ze krijgen alleen een object in uw huurder Service Principal. Zie de [toepassing en Service Principal-objecten](active-directory-application-objects.md) voor meer informatie.

Wilt verwijderen van een toepassing met meerdere huurder toegang tot uw map (na heeft toestemming verleend), moet de systeembeheerder van het bedrijf een Azure-abonnement toegang via de klassieke Azure portal te verwijderen. U gewoon navigeren naar de pagina configuratie van de toepassing en klik op de knop 'Toegang beheren' aan de onderkant. Systeembeheerder van het bedrijf kan ook de [Azure AD PowerShell-Cmdlets](http://go.microsoft.com/fwlink/?LinkId=294151) gebruiken om toegang te verwijderen.

## <a name="next-steps"></a>Volgende stappen

- Zie de [Richtlijnen voor geïntegreerde toepassingen huisstijl](active-directory-branding-guidelines.md) voor tips over visuele richtlijnen voor uw app.

- Zie voor meer informatie over de relatie tussen de toepassing en Service Principal objecten van een toepassing, [toepassing en Service Principal-objecten](active-directory-application-objects.md).

- Zie voor meer informatie over de rol van de app-manifest wordt afgespeeld, [wat het toepassingsmanifest Azure Active Directory?](active-directory-application-manifest.md)

- Zie de [verklarende woordenlijst Azure AD-ontwikkelaar](active-directory-dev-glossary.md) voor definities van de belangrijkste Azure Active Directory (AD) developer begrippen.

- Ga naar de [handleiding voor ontwikkelaars van Active Directory](active-directory-developers-guide.md) voor een overzicht van alle ontwikkelaars verwante inhoud.
