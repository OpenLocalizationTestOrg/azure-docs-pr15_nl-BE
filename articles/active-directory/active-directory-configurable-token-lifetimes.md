<properties
   pageTitle="Configureerbare levensduur van Token in Azure Active Directory | Microsoft Azure"
   description="Deze functie wordt gebruikt door beheerders en ontwikkelaars geeft u de levensduur van tokens die worden uitgegeven door AD Azure."
   services="active-directory"
   documentationCenter=""
   authors="billmath"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"  
   ms.workload="identity"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="10/06/2016"
   ms.author="billmath"/>


# <a name="configurable-token-lifetimes-in-azure-active-directory-public-preview"></a>Configureerbare levensduur van Token in Azure Active Directory (Public Preview)

>[AZURE.NOTE]
>Deze functionaliteit is momenteel in openbare preview.  Wees voorbereid om te herstellen of verwijderen van eventuele wijzigingen.  We zijn deze voorziening voor iedereen proberen tijdens de public preview openstelling, bepaalde aspecten kunnen evenwel een [Azure AD Premium-abonnement](active-directory-get-started-premium.md) als algemeen beschikbaar stellen.


## <a name="introduction"></a>Inleiding
Deze functie wordt gebruikt door beheerders en ontwikkelaars geeft u de levensduur van tokens die worden uitgegeven door AD Azure. Levensduur van token kunnen worden geconfigureerd voor alle toepassingen in een huurder voor een toepassing met meerdere huurder of voor een specifieke Service Principal in een huurder.

In Azure AD vertegenwoordigt een beleidsobject een reeks regels die worden toegepast op afzonderlijke of alle toepassingen in een huurder.  Elk type beleid heeft een unieke structuur met een set eigenschappen die worden vervolgens toegepast op de objecten waaraan zij zijn toegewezen.

Een beleid kan worden aangewezen als de standaardwaarde voor een huurder. Dit beleid wordt vervolgens van kracht in elke toepassing die zich bevindt binnen pachters, zolang het niet wordt overschreven door een beleid met een hogere prioriteit. Beleid kan ook worden toegewezen aan specifieke toepassingen. De volgorde van prioriteit, verschilt per beleidstype.

Levensduur van tokens beleid kunnen worden geconfigureerd voor vernieuwen tokens, toegangstokens sessie tokens en id-tokens.


### <a name="access-tokens"></a>Toegangstokens
Een toegangstoken wordt gebruikt door een client toegang te krijgen tot een beveiligde bron. Een toegangstoken kan alleen worden gebruikt voor een specifieke combinatie van gebruiker, de client en de resource. Toegangstokens kunnen niet worden ingetrokken en zijn geldig totdat ze aflopen. Een kwaadwillende acteur die een toegangstoken verkregen kunt gebruiken voor de omvang van de levensduur.  Aanpassen access token levensduur is een compromis tussen de prestaties van het systeem te verbeteren en verhogen van de hoeveelheid tijd dat de client toegang blijft nadat de account is uitgeschakeld.  Verbeterde systeemprestaties wordt bereikt door het verminderen van het aantal keren dat een client moet een toegangstoken vers verkrijgen. 

### <a name="refresh-tokens"></a>Tokens vernieuwen
Wanneer een client een toegangstoken verkrijgt voor toegang tot een beveiligde bron, ontvangt deze een token vernieuwen én een toegangstoken. Het token vernieuwen wordt gebruikt voor nieuwe access/vernieuwen token paren als het huidige toegangstoken verloopt. Vernieuwen combinaties van gebruikers- en zijn afhankelijk van tokens. Zij kunnen worden ingetrokken en hun geldigheid gecontroleerd telkens wanneer ze worden gebruikt.

Het is belangrijk onderscheid tussen clients vertrouwelijk en openbaar maken. Vertrouwelijke clients zijn toepassingen die kunnen worden veilig opgeslagen clientwachtwoord op, zodat ze kunnen bewijzen dat aanvragen afkomstig zijn van de clienttoepassing en niet een schadelijke actor. Als deze stromen veiliger, de standaardlevensduur van vernieuwen tokens die worden uitgegeven aan deze overdrachtssnelheden hoger zijn en kunnen niet worden gewijzigd met behulp van beleid zijn.

Als gevolg van beperkingen van de omgeving waarin de toepassingen worden uitgevoerd in openbare clients kunnen veilig opslaan van het wachtwoord van een client. Beleid kunnen worden ingesteld op hulpbronnen om te voorkomen dat het verkrijgen van een nieuwe access/vernieuwen token paar (vernieuwen Token Max inactieve tijd) vernieuwen tokens van openbare oudere clients dan een opgegeven periode.  Bovendien kunnen beleid worden gebruikt voor het instellen van een periode waarbij de vernieuwing van de tokens niet langer zijn aanvaard (Token Max Age vernieuwen).  Aanpassen kunt vernieuwen token u bepalen wanneer en hoe vaak de gebruiker opnieuw in te voeren van referenties in plaats van achtergrond opnieuw worden geverifieerd bij het gebruik van een openbare-clienttoepassing nodig is.


### <a name="id-tokens"></a>ID tokens
ID-tokens worden doorgegeven aan websites en native clients en bevatten profielinformatie over een gebruiker. Een ID-token is gebonden aan een specifieke combinatie van gebruiker en de client. ID-tokens worden beschouwd als geldig tot verstrijken.  Normaal gesproken in een webtoepassing een gebruiker overeenkomt met de levensduur van de sessie in de toepassing op de levensduur van de ID-token verleend voor de gebruiker.  Aanpassen kunt ID token u bepalen hoe vaak de webtoepassing, wordt de toepassingssessie verloopt en dat de gebruiker moet opnieuw worden geverifieerd met AD Azure (achtergrond of interactief).

### <a name="single-sign-on-session-token"></a>Token-on sessie
Wanneer een gebruiker wordt geverifieerd met Azure Active Directory, een sessie voor eenmalige aanmelding is tot stand gebracht met de browser van de gebruiker en de Azure AD.  De Single Sign-On sessie Token, in de vorm van een cookie, geeft deze sessie. Het is belangrijk te weten dat de token van de SSO-sessie niet is gebonden aan een bepaalde resource/client-toepassing. SSO-sessie tokens kunnen worden ingetrokken en hun geldigheid gecontroleerd telkens wanneer ze worden gebruikt.

Er zijn twee soorten tokens van SSO-sessie. Permanente sessie tokens worden opgeslagen als permanente cookies door de browser en niet-permanente sessie tokens worden opgeslagen als cookies per sessie (deze worden vernietigd wanneer de browser wordt gesloten).

Niet-permanente sessie tokens hebben een levensduur van 24 uur dat permanente tokens een levensduur van 180 dagen hebben. Elk gewenst moment die de sessie SSO-token wordt gebruikt binnen de geldigheidsduur wordt de geldigheidsduur verlengd andere 24 uur of 180 dagen. Als u de SSO-sessie token niet is gebruikt binnen de geldigheidsduur wordt geacht verlopen en niet meer worden geaccepteerd. 

Beleid kunnen worden gebruikt voor het instellen van een periode na de eerste sessie token is uitgegeven waarboven de sessie-tokens niet langer zijn aanvaard (sessie Token Max Age).  Aanpassen kunt sessie token u bepalen wanneer en hoe vaak de gebruiker opnieuw referenties moet invoeren in plaats van de achtergrond wordt geverifieerd bij het gebruik van een webtoepassing is vereist.

### <a name="token-lifetime-policy-properties"></a>Eigenschappen van levensduur van tokens
Een levensduur van tokens beleid is een beleidsobject dat de levensduur van tokens regels bevat.  De eigenschappen van het beleid worden gebruikt voor het bepalen van de opgegeven levensduur van token.  Als er geen beleid is ingesteld, wordt de standaardwaarde levensduur afgedwongen door het systeem.


### <a name="configurable-token-lifetime-properties"></a>Configureerbare levensduur van tokens eigenschappen
Eigenschap|De tekenreeks voor de eigenschap beleid|Is van invloed op|Standaard|Minimum|Maximum|
----- | ----- | ----- |----- | ----- | ----- |
Access Token Lifetime|  AccessTokenLifetime|Toegangstokens, ID tokens, SAML2-tokens|1 uur|10 minuten|1 dag|
Token Max inactieve tijd vernieuwen|    MaxInactiveTime |Tokens vernieuwen |14 dagen|10 minuten|    90 dagen|
Single Factor vernieuwen Token Max leeftijd|    MaxAgeSingleFactor  |Vernieuwen van tokens (voor gebruikers) |90 dagen|10 minuten |Pas ingetrokken *|
Het Token Max leeftijd meerledige vernieuwen| MaxAgeMultiFactor|  Vernieuwen van tokens (voor gebruikers)| 90 dagen|10 minuten|Pas ingetrokken *|
Single Factor sessie Token Max leeftijd |MaxAgeSessionSingleFactor **    |Sessie-tokens (permanente en niet-permanente)| Pas ingetrokken   |10 minuten |Pas ingetrokken *|
Meerledige sessie Token Max leeftijd| MaxAgeSessionMultiFactor ***|    Sessie-tokens (permanente en niet-permanente)| Pas ingetrokken|  10 minuten| Pas ingetrokken *



- * 365 dagen is de expliciete maximumlengte voor deze kenmerken kan worden ingesteld.
- ** Als MaxAgeSessionSingleFactor is niet ingesteld wordt deze waarde de waarde MaxAgeSingleFactor. Als de parameter niet is ingesteld, krijgt de eigenschap de standaardwaarde (pas ingetrokken).
- Als MaxAgeSessionMultiFactor niet is ingesteld wordt deze waarde de waarde MaxAgeMultiFactor. Als de parameter niet is ingesteld, krijgt de eigenschap de standaardwaarde (pas ingetrokken).

### <a name="exceptions"></a>Uitzonderingen
Eigenschap|Is van invloed op|Standaard|
----- | ----- | ----- |
Token Max inactieve tijd (federatieve gebruikers met onvoldoende intrekkingsgegevens) vernieuwen|Tokens (uitgegeven voor federatieve gebruikers met onvoldoende intrekkingsgegevens) vernieuwen|12 uur|
Vernieuwen Token Max inactieve tijd (vertrouwelijke Clients)| Tokens (uitgegeven voor Clients vertrouwelijke) vernieuwen|90 dagen|
Token Max Age (uitgegeven voor Clients vertrouwelijke) vernieuwen |   Tokens (uitgegeven voor Clients vertrouwelijke) vernieuwen |Pas ingetrokken

### <a name="priority-and-evaluation-of-policies"></a>Prioriteit en de evaluatie van beleid

Token Lifetime beleid kunnen worden gemaakt en toegewezen aan specifieke toepassingen, huurders en service beveiligings-principals. Dit betekent dat het is mogelijk meerdere beleidsinstellingen toe te passen op een bepaalde toepassing. De levensduur van tokens beleid dat van kracht wordt, volgt deze regels:


- Als u een beleid expliciet is toegewezen aan de service principal, zal worden afgedwongen. 
- Als er geen beleid expliciet is toegewezen aan de service principal, worden een beleid expliciet is toegewezen aan de bovenliggende pachter van de service principal afgedwongen. 
- Als er geen beleid expliciet is toegewezen aan de service principal of de huurder, worden het beleid dat is toegewezen aan de toepassing afgedwongen. 
- Als er geen beleid is toegewezen aan de service principal, de huurder of het application-object, de standaardwaarden worden afgedwongen (Zie bovenstaande tabel).

Zie voor meer informatie over de relatie tussen toepassing en service principal objecten in Azure AD, [toepassing en service-principals in Azure Active Directory](active-directory-application-objects.md).

De geldigheid van een token wordt geëvalueerd op het moment dat deze wordt gebruikt. Het beleid met de hoogste prioriteit op de toepassing die wordt geopend van kracht.


>[AZURE.NOTE]
>Voorbeeld
>
>Een gebruiker wil toegang tot 2 webtoepassingen, A en B. 
>
>
>- Beide toepassingen zijn in de pachter met dezelfde bovenliggende. 
>- Levensduur van tokens is 1 met een sessie Token Max Age van 8 uur ingesteld als standaard van de bovenliggende huurder.
>- A-webtoepassing is een webtoepassing regelmatig gebruik en is niet gekoppeld aan het beleid. 
>- B-webtoepassing wordt gebruikt voor zeer gevoelige processen en de service principal is gekoppeld aan de levensduur van tokens beleid 2 met een sessie Token Max Age van 30 minuten.
>
>Om 12:00 uur wordt de gebruiker om een nieuwe browsersessie opent en probeert toegang te krijgen tot de webtoepassing A. de gebruiker wordt omgeleid naar Azure AD en wordt gevraagd om aan te melden. Dit wordt een cookie met een sessietoken neergezet in de browser. Terugkeren naar een toepassing met een ID-token waarmee ze de toepassing toegang tot het web, wordt de gebruiker omgeleid.
>
>Om 12:15 uur, vervolgens probeert de gebruiker toegang te krijgen tot de webtoepassing B. De browser wordt omgeleid naar Azure AD opsporen van de sessie-cookie. Web application B service principal is gekoppeld aan een beleid 1, maar ook deel uitmaakt van de huurder van bovenliggende beleid standaard 2. Beleid 2 kracht Aangezien beleidsregels die zijn gekoppeld aan beveiligings-principals service een hogere prioriteit dan de huurder standaardbeleid hebben. Het sessietoken oorspronkelijk werd uitgegeven in de laatste 30 minuten zodat als geldig wordt beschouwd. De gebruiker weer omgeleid naar de webtoepassing B met een ID-token toegang verlenen.
>
>De gebruiker probeert om 13:00 uur navigeren naar web application A. De gebruiker wordt omgeleid naar Azure AD. A-webtoepassing niet is gekoppeld aan een beleid, maar omdat deze een huurder met standaardbeleid 1, dit beleid van kracht. De sessie cookie wordt gedetecteerd die oorspronkelijk werd uitgegeven in de laatste 8 uur en de gebruiker zonder bericht weer omgeleid naar de webtoepassing A met een nieuwe ID-token zonder te verifiëren.
>
>De gebruiker probeert direct toegang te krijgen tot de webtoepassing B. De gebruiker wordt omgeleid naar Azure AD. Als voorheen, 2 beleid van kracht. Als het token langer dan 30 minuten geleden uitgegeven is, de gebruiker vervolgens gevraagd hun referenties opnieuw invoeren en een nieuwe sessie en id-token worden uitgegeven. De gebruiker vervolgens toegang webtoepassing B.

## <a name="configurable-policy-properties-in-depth"></a>Configureerbare eigenschappen: diepgaand

### <a name="access-token-lifetime"></a>Access token lifetime

**String:** AccessTokenLifetime

**Is van invloed op:** Toegangstokens, ID tokens

**Samenvatting:** Dit beleid bepaalt hoe lang toegang en tokens voor deze resource ID als geldig worden beschouwd. Vermindering van de levensduur van tokens toegang vermindert het risico van een access- of id-token wordt gebruikt door een kwaadwillende actor voor langere tijd (als ze niet kunnen worden ingetrokken) maar ook nadelig invloed op prestaties als de tokens moeten vaker worden vervangen.

### <a name="refresh-token-max-inactive-time"></a>Token max inactieve tijd vernieuwen

**String:** MaxInactiveTime

**Is van invloed op:** Tokens vernieuwen

**Samenvatting:** Dit beleid bepaalt hoe oud een token vernieuwen kan zijn voordat een client kan niet langer worden gebruikt voor het ophalen van een nieuw sleutelpaar bestaande access/vernieuwen token bij een poging toegang te krijgen tot deze bron. Aangezien een nieuwe vernieuwen-token teruggestuurd meestal dat een token vernieuwen wordt gebruikt, moet de client niet hebt bereikt van alle bronnen met behulp van het token van de huidige vernieuwen voor de opgegeven periode van tijd voordat dit beleid voorkomen toegang dat wordt. 

Dit beleid kan gebruikers die niet actief op de client opnieuw worden geverifieerd zijn voor het ophalen van een nieuwe vernieuwen token worden gedwongen. 

Het is belangrijk te weten dat het vernieuwen Token Max inactieve tijd moet worden ingesteld op een lagere waarde dan de Single Factor Token Max Age en de meerledige vernieuwen Token Max Age.

### <a name="single-factor-refresh-token-max-age"></a>Single factor vernieuwen token maximale leeftijd

**String:** MaxAgeSingleFactor

**Is van invloed op:** Tokens vernieuwen

**Samenvatting:** Deze beleid bepaalt hoe lang een gebruiker kunt blijven gebruiken tokens vernieuwen om nieuwe toegang en vernieuwen token paren na de laatste keer dat deze geverifieerd is met slechts één factor. Zodra een gebruiker wordt geverifieerd en ontvangt een nieuwe beveiligingssleutel voor vernieuwen, mogelijk gebruik van de token stroom vernieuwen (zo lang de huidige vernieuwen token niet is ingetrokken en is niet langer dan de niet-actieve tijd ongebruikt gelaten) voor de opgegeven periode. Op dat moment worden gebruikers gedwongen om hernieuwde verificatie als u wilt een nieuwe vernieuwen token ontvangen. 

Vermindering van de maximale leeftijd zal dwingen gebruikers vaker te verifiëren. Aangezien één factor verificatie wordt beschouwd als minder veilig dan een meerledige verificatie, verdient het aanbeveling dat dit beleid is ingesteld op een waarde gelijk of lager dan het meerledige vernieuwen Token Max leeftijd beleid.

### <a name="multi-factor-refresh-token-max-age"></a>Token maximale leeftijd meerledige vernieuwen

**String:** MaxAgeMultiFactor

**Is van invloed op:** Tokens vernieuwen

**Samenvatting:** Deze beleid bepaalt hoe lang een gebruiker kunt blijven vernieuwen tokens gebruiken om nieuwe toegang en vernieuwen token paren na de laatste keer dat deze geverifieerd is met meerdere factoren. Zodra een gebruiker wordt geverifieerd en ontvangt een nieuwe beveiligingssleutel voor vernieuwen, mogelijk gebruik van de token stroom vernieuwen (zo lang de huidige vernieuwen token niet is ingetrokken en is niet langer dan de niet-actieve tijd ongebruikt gelaten) voor de opgegeven periode. Op dat moment worden gebruikers gedwongen om hernieuwde verificatie als u wilt een nieuwe vernieuwen token ontvangen. 

Vermindering van de maximale leeftijd zal dwingen gebruikers vaker te verifiëren. Aangezien één factor verificatie wordt beschouwd als minder veilig dan een meerledige verificatie, verdient het aanbeveling dat dit beleid is ingesteld op een waarde gelijk of groter is dan het één Factor vernieuwen Token Max leeftijd beleid.

### <a name="single-factor-session-token-max-age"></a>Single factor sessie token maximale leeftijd

**String:** MaxAgeSessionSingleFactor

**Is van invloed op:** Sessie-tokens (permanente en niet-permanente)

**Samenvatting:** Deze beleid bepaalt hoe lang een gebruiker kunt blijven sessie tokens gebruiken om nieuwe sessie-ID en de tokens na de laatste keer dat deze met succes geverifieerd met slechts één factor. Zodra een gebruiker wordt geverifieerd en een nieuwe sessietoken ontvangt, worden ze kunnen de sessie token stroom gebruiken (als het token voor de huidige sessie is niet ingetrokken of verlopen) voor de opgegeven periode. Op dat moment worden gebruikers gedwongen om een nieuwe sessietoken ontvangen opnieuw worden geverifieerd. 

Vermindering van de maximale leeftijd zal dwingen gebruikers vaker te verifiëren. Aangezien één factor verificatie wordt beschouwd als minder veilig dan een meerledige verificatie, verdient het aanbeveling dat dit beleid is ingesteld op een waarde gelijk of lager dan het meerledige sessie Token Max leeftijd beleid.

### <a name="multi-factor-session-token-max-age"></a>Meerledige sessie token maximale leeftijd

**String:** MaxAgeSessionMultiFactor

**Is van invloed op:** Sessie-tokens (permanente en niet-permanente)

**Samenvatting:** Deze beleid bepaalt hoe lang een gebruiker kunt blijven sessie tokens gebruiken om een nieuwe sessie-ID en de tokens krijgen na de laatste keer dat deze met succes geverifieerd met meerdere factoren. Zodra een gebruiker wordt geverifieerd en een nieuwe sessietoken ontvangt, worden ze kunnen de sessie token stroom gebruiken (als het token voor de huidige sessie is niet ingetrokken of verlopen) voor de opgegeven periode. Op dat moment worden gebruikers gedwongen om een nieuwe sessietoken ontvangen opnieuw worden geverifieerd. 

Vermindering van de maximale leeftijd zal dwingen gebruikers vaker te verifiëren. Aangezien één factor verificatie wordt beschouwd als minder veilig dan een meerledige verificatie, verdient het aanbeveling dat dit beleid is ingesteld op een waarde gelijk of groter is dan één Factor sessie Token Max leeftijd beleid.

## <a name="sample-token-lifetime-policies"></a>Monster levensduur van tokens beleid

Kunt maken en beheren van token levensduur voor apps, service-principals en uw algehele huurder beschrijft alle soorten nieuwe scenario's mogelijk in Azure AD.  Gaan we stapsgewijs door enkele algemene scenario's voor beleidsregels waarmee u nieuwe regels voor te leggen:


- Levensduur van token
- Inactieve tijden token Max
- Maximale leeftijd token

We zien een paar scenario's met inbegrip van:


- Beheren van een huurder standaardbeleid
- Een beleid maken voor website aanmelden
- Een beleid maken voor Native Apps een Web-API wordt aangeroepen
- Een geavanceerde beleid beheren 

### <a name="prerequisites"></a>Vereisten
In het voorbeeldscenario's worden we maken, bijwerken, koppelen en beleid voor apps, service-principals en uw algehele huurder verwijderen.  Bent u nieuw in Azure AD, afhandeling van [dit artikel](active-directory-howto-tenant.md) om u te helpen de slag voordat u verdergaat met deze monsters.  


1. Download de nieuwste [Azure AD PowerShell-Cmdlet voorbeeld](https://www.powershellgallery.com/packages/AzureADPreview)om te beginnen. 
2.  Zodra u de Azure AD PowerShell-Cmdlets hebt, verbinding maken met opdracht uitvoeren om te melden bij uw beheerdersaccount Azure AD. U moet dit doen wanneer u een nieuwe sessie start.
        
        Connect-AzureAD -Confirm

3.  Voer de volgende opdracht voor een overzicht van alle beleidsregels die zijn gemaakt in uw huurder.  Deze opdracht moet na de meeste bewerkingen in de volgende scenario's worden gebruikt.  Ook kunt u de **Object-ID** van uw beleid. 
        
        Get-AzureADPolicy

### <a name="sample-managing-a-tenants-default-policy"></a>Voorbeeld: Beheren van het standaardbeleid van de huurder

In dit voorbeeld maken we een beleid dat de gebruikers minder vaak aanmelden via uw hele huurder. 

Hiervoor maken we een beleid voor de levensduur van tokens voor één Factor vernieuwen-Tokens die wordt toegepast op de huurder. Dit beleid zal worden toegepast op elke toepassing in uw huurder en elke service principal die nog niet beschikt over een beleid dat is ingesteld op het. 

1.  **Maak een beleid voor de levensduur van tokens.** 

Stelt het Token voor het vernieuwen van één Factor op "pas ingetrokken" zin die verlopen Won't pas toegang wordt ingetrokken.  Het beleid van onderstaande definitie is wat we gaat maken:
        
        @("{
          `"TokenLifetimePolicy`":
              {
                 `"Version`":1, 
                 `"MaxAgeSingleFactor`":`"until-revoked`"
              }
        }")

Voer de volgende opdracht om dit beleid te maken. 

        
    New-AzureADPolicy -Definition @("{`"TokenLifetimePolicy`":{`"Version`":1, `"MaxAgeSingleFactor`":`"until-revoked`"}}") -DisplayName TenantDefaultPolicyScenario -IsTenantDefault $true -Type TokenLifetimePolicy
        
Het nieuwe beleid en de object-id ophalen, moet u de volgende opdracht uitvoeren.

    Get-AzureADPolicy
&nbsp;&nbsp;2. **het beleid bijwerken**

U hebt besloten het eerste beleid is niet zo streng als de service vereist en u wilt dat uw Single Factor vernieuwen Tokens verloopt in 2 dagen hebben besloten. Voer de volgende opdracht. 
        
    Set-AzureADPolicy -ObjectId <ObjectID FROM GET COMMAND> -DisplayName TenantDefaultPolicyUpdatedScenario -Definition @("{`"TokenLifetimePolicy`":{`"Version`":1,`"MaxAgeSingleFactor`":`"2.00:00:00`"}}")
        
&nbsp;&nbsp;3. **u bent klaar!** 

### <a name="sample-creating-a-policy-for-web-sign-in"></a>Voorbeeld: Een beleid maken voor website aanmelden

In dit voorbeeld maken we een beleid waarin wordt gevraagd voor het verifiëren van gebruikers vaker in uw Web App. Dit beleid stelt u de levensduur van de Tokens Id toegang en de Max Age van een meerledige sessie Token op de service principal van uw web app.

1.  **Maak een beleid voor de levensduur van tokens.**

Dit beleid voor aanmelding bij de webpagina wordt de levensduur van tokens Id toegang en Max Single Factor sessie Token leeftijd ingesteld op 2 uur.

    New-AzureADPolicy -Definition @("{`"TokenLifetimePolicy`":{`"Version`":1,`"AccessTokenLifetime`":`"02:00:00`",`"MaxAgeSessionSingleFactor`":`"02:00:00`"}}") -DisplayName WebPolicyScenario -IsTenantDefault $false -Type TokenLifetimePolicy

Het nieuwe beleid en de object-id ophalen, moet u de volgende opdracht uitvoeren.

    Get-AzureADPolicy
&nbsp;&nbsp;2. **het beleid toewijzen aan uw service principal.**

Gaan we dit nieuwe beleid met een service principal koppelen.  U moet ook toegang krijgen tot het **object-id** van uw service principal. U kunt query uitvoeren op de [Microsoft Graph](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity) of Ga naar onze [Grafiekgereedschap Explorer](https://graphexplorer.cloudapp.net/) en inloggen op uw account Azure AD voor een overzicht van de huurder service beveiligings-principals. 

Als u de **object-id**hebt, kunt u de volgende opdracht uitvoeren.
        
    Add-AzureADServicePrincipalPolicy -ObjectId <ObjectID of the Service Principal> -RefObjectId <ObjectId of the Policy>
&nbsp;&nbsp;3. **u bent klaar!** 

 

### <a name="sample-creating-a-policy-for-native-apps-calling-a-web-api"></a>Voorbeeld: Een beleid maken voor native apps een Web-API wordt aangeroepen

>[AZURE.NOTE]
>Koppelen van beleidsregels voor toepassingen is momenteel uitgeschakeld.  Wij werken voor het inschakelen van dit binnenkort.  Deze pagina wordt bijgewerkt zodra de functie beschikbaar is.

In dit voorbeeld maakt u een beleid waarbij gebruikers worden geverifieerd minder we en wordt de hoeveelheid tijd die ze niet actief worden kunnen zonder opnieuw te identificeren verlengen. Het beleid wordt toegepast op de Web-API die manier wanneer het Native App hierom vraagt een bron dat dit beleid wordt toegepast.

1.  **Maak een beleid voor de levensduur van tokens.** 

Met deze opdracht maakt u een strikt beleid voor Web-API. 
        
    New-AzureADPolicy -Definition @("{`"TokenLifetimePolicy`":{`"Version`":1,`"MaxInactiveTime`":`"30.00:00:00`",`"MaxAgeMultiFactor`":`"until-revoked`",`"MaxAgeSingleFactor`":`"180.00:00:00`"}}") -DisplayName WebApiDefaultPolicyScenario -IsTenantDefault $false -Type TokenLifetimePolicy
         
Het nieuwe beleid en de object-id ophalen, moet u de volgende opdracht uitvoeren.

    Get-AzureADPolicy

&nbsp;&nbsp;2. **Wijs het beleid voor uw Web-API**.

Gaan we dit nieuwe beleid met een toepassing koppelen.  U moet ook toegang krijgen tot het **object-id** van de toepassing. De beste manier om het vinden van uw app **object-id** is de [Azure Portal](https://portal.azure.com/)gebruiken. 

Als u de **object-id**hebt, kunt u de volgende opdracht uitvoeren.

    Add-AzureADApplicationPolicy -ObjectId <ObjectID of the App> -RefObjectId <ObjectId of the Policy>

&nbsp;&nbsp;3. **u bent klaar!** 

### <a name="sample-managing-an-advanced-policy"></a>Voorbeeld: Een geavanceerde beleid beheren 

In dit voorbeeld maken we een paar beleid om aan te tonen hoe het systeem prioriteit werkt en hoe u meerdere beleidsinstellingen toegepast op verschillende objecten kunt beheren. Dit geeft enig inzicht in de prioriteit van het beleid is uiteengezet en ook kunt u meer complexe scenario's te beheren. 

1.  **Maak een beleid voor de levensduur van tokens**

Tot nu toe vrij eenvoudig. Wij hebben een standaardbeleid huurder die de levensduur van één Factor vernieuwen tokens worden ingesteld op 30 dagen gemaakt. 

    New-AzureADPolicy -Definition @("{`"TokenLifetimePolicy`":{`"Version`":1,`"MaxAgeSingleFactor`":`"30.00:00:00`"}}") -DisplayName ComplexPolicyScenario -IsTenantDefault $true -Type TokenLifetimePolicy
Is het nieuwe beleid en deze object-id, de volgende opdracht uitvoeren.
 
    Get-AzureADPolicy

&nbsp;&nbsp;2. **het beleid van een Service Principal toewijzen**

Nu hebben we een beleid op de gehele huurder.  Stel dat we willen dat dit beleid van 30 dagen voor een specifieke service principal behouden, maar het standaardbeleid van de huurder om de bovengrens van de "pas ingetrokken" wijzigen. 

Eerst gaan We dit nieuwe beleid met onze service principal koppelen.  U moet ook toegang krijgen tot het **object-id** van uw service principal. U kunt query uitvoeren op de [Microsoft Graph](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity) of Ga naar onze [Grafiekgereedschap Explorer](https://graphexplorer.cloudapp.net/) en inloggen op uw account Azure AD voor een overzicht van de huurder service beveiligings-principals. 

Als u de **object-id**hebt, kunt u de volgende opdracht uitvoeren.

    Add-AzureADServicePrincipalPolicy -ObjectId <ObjectID of the Service Principal> -RefObjectId <ObjectId of the Policy>

&nbsp;&nbsp;3. **Stel de vlag IsTenantDefault false met de volgende opdracht**. 

    Set-AzureADPolicy -ObjectId <ObjectId of Policy> -DisplayName ComplexPolicyScenario -IsTenantDefault $false
&nbsp;&nbsp;4. **een nieuwe huurder standaardbeleid maken**

    New-AzureADPolicy -Definition @("{`"TokenLifetimePolicy`":{`"Version`":1,`"MaxAgeSingleFactor`":`"until-revoked`"}}") -DisplayName ComplexPolicyScenarioTwo -IsTenantDefault $true -Type TokenLifetimePolicy

&nbsp;&nbsp;5. **u bent klaar!** 

U hebt nu het oorspronkelijke beleid dat is gekoppeld aan uw service principal en de nieuwe instelling als standaardbeleid voor de huurder.  Het is belangrijk te onthouden dat beleid toegepast op service-principals prioriteit boven het standaardbeleid huurder. 


## <a name="cmdlet-reference"></a>Cmdlet referentie

### <a name="manage-policies"></a>Beleid beheren
De volgende cmdlets kunnen worden gebruikt voor het beheren van beleid.</br></br>



#### <a name="new-azureadpolicy"></a>Nieuwe AzureADPolicy
Hiermee maakt u een nieuw beleid.

    New-AzureADPolicy -Definition <Array of Rules> -DisplayName <Name of Policy> -IsTenantDefault <boolean> -Type <Policy Type> 

Parameters|Beschrijving|Voorbeeld|
-----| ----- |-----|
-Definitie| De matrix van stringified JSON met de regels van het beleid.|-Definition @("{`"TokenLifetimePolicy`":{`"Version`":1,`"MaxInactiveTime`":`"20:00:00`"}}") 
DisplayName-|De tekenreeks van de naam van het beleid|DisplayName - MyTokenPolicy
-IsTenantDefault|Als true wordt ingesteld voor het beleid als de huurder het standaardbeleid als de waarde false, gebeurt er niets|-IsTenantDefault $true
-Type|Het type van het beleid voor token levensduur altijd gebruik van "TokenLifetimePolicy"|-TokenLifetimePolicy type
-AlternativeIdentifier [optioneel]|Hiermee stelt een alternatieve id aan het beleid.|MyAltId - AlternativeIdentifier
</br></br>

#### <a name="get-azureadpolicy"></a>Get-AzureADPolicy         
Deze eigenschap haalt alle AzureAD beleid of het opgegeven beleid 
        
    Get-AzureADPolicy 

Parameters|Beschrijving|Voorbeeld|
-----| ----- |-----|
-ObjectId [optioneel]|De object-Id van het beleid dat u wilt ophalen. |Object-id - &lt;object-id van beleid&gt; 
</br></br>

#### <a name="get-azureadpolicyappliedobject"></a>Get-AzureADPolicyAppliedObject         
Deze methode haalt alle apps en service beveiligings-principals die zijn gekoppeld aan een beleid
        
    Get-AzureADPolicyAppliedObject -ObjectId <object id of policy> 

Parameters|Beschrijving|Voorbeeld|
-----| ----- |-----|
-ObjectId|De object-Id van het beleid dat u wilt ophalen.|Object-id - &lt;object-id van beleid&gt;
</br></br>

#### <a name="set-azureadpolicy"></a>Set AzureADPolicy
Updates van een bestaand beleid
        
    Set-AzureADPolicy -ObjectId <object id of policy> -DisplayName <string> 
 
Parameters|Beschrijving|Voorbeeld|
-----| ----- |-----|
-ObjectId|De object-Id van het beleid dat u wilt ophalen.|Object-id - &lt;object-id van beleid&gt;
DisplayName-|De tekenreeks van de naam van het beleid|DisplayName - MyTokenPolicy
-Definitie [optioneel]|De matrix van stringified JSON met de regels van het beleid.|-Definition @("{`"TokenLifetimePolicy`":{`"Version`":1,`"MaxInactiveTime`":`"20:00:00`"}}") 
-IsTenantDefault [optioneel]|Als true wordt ingesteld voor het beleid als de huurder het standaardbeleid als de waarde false, gebeurt er niets|-IsTenantDefault $true
-Type [optioneel]|Het type van het beleid voor token levensduur altijd gebruik van "TokenLifetimePolicy"|-TokenLifetimePolicy type
-AlternativeIdentifier [optioneel]|Hiermee stelt een alternatieve id aan het beleid.|MyAltId - AlternativeIdentifier
</br></br>

#### <a name="remove-azureadpolicy"></a>Verwijderen AzureADPolicy         
Hiermee verwijdert u het opgegeven beleid

     Remove-AzureADPolicy -ObjectId <object id of policy>

Parameters|Beschrijving|Voorbeeld|
-----| ----- |-----|
-ObjectId|De object-Id van het beleid dat u wilt ophalen.|Object-id - &lt;object-id van beleid&gt;
</br></br>

### <a name="application-policies"></a>Toepassingenbeleid
De volgende cmdlets kan worden gebruikt voor beleidsregels voor toepassingen.</br></br>

#### <a name="add-azureadapplicationpolicy"></a>Toevoegen-AzureADApplicationPolicy         
Het opgegeven beleid wordt gekoppeld aan een toepassing

    Add-AzureADApplicationPolicy -ObjectId <object id of application> -RefObjectId <object id of policy>

Parameters|Beschrijving|Voorbeeld|
-----| ----- |-----|
-ObjectId|De object-Id van de toepassing.|Object-id - &lt;object-id van de toepassing&gt; 
-RefObjectId|De object-Id van het beleid. |RefObjectId - &lt;object-id van beleid&gt;
</br></br>

#### <a name="get-azureadapplicationpolicy"></a>Get-AzureADApplicationPolicy        
Met deze eigenschap wordt het beleid dat is toegewezen aan een toepassing

    Get-AzureADApplicationPolicy -ObjectId <object id of application>

Parameters|Beschrijving|Voorbeeld|
-----| ----- |-----|
-ObjectId|De object-Id van de toepassing.|Object-id - &lt;object-id van de toepassing&gt; 
</br></br>

#### <a name="remove-azureadapplicationpolicy"></a>Verwijderen AzureADApplicationPolicy        
Hiermee verwijdert u een beleid vanuit een toepassing

    Remove-AzureADApplicationPolicy -ObjectId <object id of application> -PolicyId <object id of policy>

Parameters|Beschrijving|Voorbeeld|
-----| ----- |-----|
-ObjectId|De object-Id van de toepassing.|Object-id - &lt;object-id van de toepassing&gt; 
-PolicyId| De object-id van het beleid.|PolicyId - &lt;object-id van beleid&gt;
</br></br>

### <a name="service-principal-policies"></a>Service principal-beleid
De volgende cmdlets kan worden gebruikt voor service principal-beleid.</br></br>

#### <a name="add-azureadserviceprincipalpolicy"></a>Toevoegen-AzureADServicePrincipalPolicy         
Het opgegeven beleid wordt gekoppeld aan een service principal

    Add-AzureADServicePrincipalPolicy -ObjectId <object id of service principal> -RefObjectId <object id of policy>

Parameters|Beschrijving|Voorbeeld|
-----| ----- |-----|
-ObjectId|De object-Id van de toepassing.|Object-id - &lt;object-id van de toepassing&gt; 
-RefObjectId|De object-Id van het beleid. |RefObjectId - &lt;object-id van beleid&gt;
</br></br>

#### <a name="get-azureadserviceprincipalpolicy"></a>Get-AzureADServicePrincipalPolicy        
Met deze eigenschap wordt een beleid dat is gekoppeld aan de opgegeven service principal

    Get-AzureADServicePrincipalPolicy -ObjectId <object id of service principal>
 
Parameters|Beschrijving|Voorbeeld|
-----| ----- |-----|
-ObjectId|De object-Id van de toepassing.|Object-id - &lt;object-id van de toepassing&gt; 
</br></br>

#### <a name="remove-azureadserviceprincipalpolicy"></a>Verwijderen AzureADServicePrincipalPolicy         
Hiermee verwijdert u het beleid van de opgegeven service principal

    Remove-AzureADServicePrincipalPolicy -ObjectId <object id of service principal>  -PolicyId <object id of policy>
 
Parameters|Beschrijving|Voorbeeld|
-----| ----- |-----|
-ObjectId|De object-Id van de toepassing.|Object-id - &lt;object-id van de toepassing&gt; 
-PolicyId| De object-id van het beleid.|PolicyId - &lt;object-id van beleid&gt;
