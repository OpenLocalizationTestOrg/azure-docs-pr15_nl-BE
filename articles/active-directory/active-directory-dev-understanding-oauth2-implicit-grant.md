<properties
   pageTitle="Wat zijn de impliciete OAuth2 verlenen stroom in Azure Active Directory | Microsoft Azure"
   description="Lees meer over Azure Active Directory-implementatie van de impliciete OAuth2 flow, verlenen en of dit geschikt is voor uw toepassing."
   services="active-directory"
   documentationCenter="dev-center-name"
   authors="vibronet"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/17/2016"
   ms.author="vittorib;bryanla"/>

# <a name="understanding-the-oauth2-implicit-grant-flow-in-azure-active-directory-ad"></a>Wat zijn de OAuth2 impliciete subsidie stroom in Azure Active Directory (AD)

De impliciete OAuth2-subsidie is algemeen wordt de subsidie met de langste lijst van beveiligingsproblemen in de specificatie van OAuth2. En nog, dat is de benadering die wordt geïmplementeerd door ADAL JS en het die is raadzaam bij het schrijven van toepassingen voor beveiligd-WACHTWOORDVERIFICATIE. Wat geeft? Het is een kwestie van het gebruik van systeembronnen: en het blijkt de impliciete subsidie is de beste benadering kunt u oefenen voor toepassingen die een Web via JavaScript-API vanuit een browser gebruiken.

## <a name="what-is-the-oauth2-implicit-grant"></a>Wat is de impliciete verlening van OAuth2?

De ultieme [OAuth2 autorisatiecode verlenen](https://tools.ietf.org/html/rfc6749#section-1.3.1) is de vergunning verlenen die gebruikmaakt van twee afzonderlijke eindpunten. Het eindpunt van de vergunning wordt gebruikt voor de gebruiker interactie fase, wat in een autorisatiecode resulteert. Het eindpunt van de token wordt vervolgens door de client gebruikt voor het uitwisselen van de code voor een toegangstoken en vaak een vernieuwen-token. Webtoepassingen moeten hun eigen toepassing referenties naar het eindpunt token, zodat de server verificatie kan de client worden geverifieerd.

De [impliciete OAuth2 verlenen](https://tools.ietf.org/html/rfc6749#section-1.3.2) is een variant waarmee een client verkrijgen een toegangstoken (en id_token voor het [Verbinden van OpenId](http://openid.net/specs/openid-connect-core-1_0.html)) rechtstreeks vanuit het eindpunt van de vergunning, zonder contact met het token eindpunt noch de clienttoepassing te verifiëren. Deze variant is speciaal ontworpen voor JavaScript gebaseerde toepassingen die worden uitgevoerd in een webbrowser: in de oorspronkelijke OAuth2-specificatie tokens worden geretourneerd in een URI-fragment. Dat maakt de token bits beschikbaar voor de JavaScript-code op de client, maar het garandeert dat ze niet opgenomen in omgeleid naar de server. Tokens opvragen via de browser wordt omgeleid rechtstreeks vanuit het eindpunt van de vergunning. Ook heeft het voordeel van het elimineren van alle vereisten voor meerdere aanroepen van oorsprong nodig zijn als u de JavaScript-toepassing moet contact opnemen met het token eindpunt.

Een belangrijk kenmerk van de impliciete verlening van OAuth2 is het feit dat dergelijke tokens vernieuwen nooit terug naar de client overdrachten. Zoals we in de volgende sectie zien, die niet echt nodig en zou in feite een beveiligingsprobleem.

## <a name="suitable-scenarios-for-the-oauth2-implicit-grant"></a>Geschikte scenario's voor de impliciete verlening van OAuth2

Als de OAuth2-specificatie zelf verklaart, is de impliciete subsidie werkte als de gebruiker-agent toepassingen – dat wil zeggen, JavaScript-toepassingen uitvoeren in een browser. Het bepalend kenmerk van dergelijke toepassingen is dat de JavaScript-code wordt gebruikt voor toegang tot serverbronnen (meestal Web API) en voor de toepassing UX dienovereenkomstig bijwerken. Van toepassingen, zoals Gmail of Outlook Web Access: wanneer u een bericht in uw postvak in selecteert, alleen het bericht visualisatie deelvenster wordt gewijzigd zodat de nieuwe selectie, terwijl de rest van de pagina ongewijzigd blijft. Dit is anders dan bij traditionele redirect gebaseerde Web apps, waarbij elke interactie van de gebruiker leidt tot een volledige pagina terugsturen en een volledige paginaweergave van het antwoord van de nieuwe server.

Toepassingen die de benadering op basis van JavaScript om de extreme uitvoeren toepassingen voor één pagina of kuuroorden worden genoemd: het idee is dat deze toepassingen alleen maar een eerste HTML-pagina en de bijbehorende JavaScript, met alle opeenvolgende interacties via JavaScript worden aangedreven door Web API-aanroepen uitgevoerd. Echter, hybride benaderingen, waarbij de toepassing is grotendeels terugposten gereden, maar incidenteel JS-aanroepen uitvoert, zijn niet ongebruikelijk: de discussie over het gebruik van de impliciete stroom is ook relevant voor die.

Omleiding-toepassingen beveiligen meestal hun aanvragen via cookies echter die aanpak niet ook voor JavaScript-toepassingen werkt. Cookies werken alleen voor het domein dat ze zijn gegenereerd, terwijl JavaScript aanroepen kunnen worden gericht op andere domeinen. In feite, dat vaak het geval zal zijn: het beste vergelijken met toepassingen die aanroepen van Microsoft Graph API, Office API, Azure API – alle die zich buiten het domein waar de toepassing wordt aangeboden. Een stijgende trend voor JavaScript-toepassingen is dat geen back-end, 100% op 3de partij Web-API's voor de uitvoering van hun zakelijke functie steunen.

Gebruik de OAuth2 aan toonder token benadering, waarbij elke aanroep vergezeld gaat van een toegangstoken OAuth2 is momenteel de geprefereerde methode voor het aanroepen van een Web-API te beschermen. De Web-API onderzoekt het toegangstoken binnenkomende en als er in deze scopes nodig, dat toegang verleent tot de aangevraagde bewerking. De impliciete stroom biedt een vereenvoudigd mechanisme voor JavaScript-toepassingen voor toegangstokens voor Web-API biedt talrijke voordelen ten opzichte van cookies:

- Tokens kunnen op betrouwbare wijze worden verkregen hoeven de cross-oorsprong oproepen – verplichte registratie van de omleiding URI tokens retour zijn garandeert geen verplaatsing van tokens
- JavaScript-toepassingen kunnen verkrijgen toegangstokens zo veel als ze nodig hebben, voor zoveel Web-API's ze als doel – met geen beperking voor domeinen
- HTML5 functies zoals sessie of lokale opslag verlenen volledige controle over het token-caching en beheer van de levensduur, dat het beheer van cookies is ondoorzichtig App
- Toegangstokens zijn niet gevoelig voor aanvallen van Cross-site request vervalsing (CSRF)

De impliciete subsidie stroom komt vernieuwen tokens, voornamelijk om veiligheidsredenen niet uitgeven. Een token vernieuwen is niet zo nauwkeuriger binnen het bereik als toegangstokens, veel meer energie dus veel meer schade wordt toegebracht in het geval dat is uitgelekt van toekenning. In de impliciete stroom tokens in de URL worden geleverd, dus het risico van onderschepping hoger is dan in de vergunning code subsidie.

Bedenk wel dat een JavaScript-toepassing een ander mechanisme beschikt voor het vernieuwen van toegangstokens zonder herhaaldelijk de gebruiker referenties. De toepassing een verborgen iframe kunt gebruiken om nieuwe token aanvragen ten opzichte van het eindpunt van de vergunning van Azure AD: als de browser nog steeds een actieve sessie heeft (Lees: een sessiecookie heeft) voor het domein Azure AD de verificatieaanvraag is doet zich voor zonder enige interactie van de gebruiker. 

Dit model biedt de JavaScript-toepassing de mogelijkheid onafhankelijk toegangstokens vernieuwen en zelfs nieuwe gemaakt voor een nieuwe API verkrijgen (vooropgesteld dat de gebruiker ingestemd eerder voor hen. Zo voorkomt u de extra belasting van aanschaf, onderhoud en bescherming van een hoge waarde artefact zoals een token vernieuwen. Het artefact waardoor stille verlenging mogelijk, de sessie-cookie Azure AD, wordt buiten de toepassing beheerd. Een ander voordeel van deze aanpak is dat een gebruiker kunt afmelden van Azure AD, met een van de toepassingen die zijn ondertekend in Azure AD, uitgevoerd in een van de tabbladen in de browser. Dit leidt tot het verwijderen van de sessie-cookie Azure AD en verliezen de JavaScript-toepassing automatisch de mogelijkheid voor het vernieuwen van beveiligingstokens voor de gebruiker uit ondertekend.

## <a name="is-the-implicit-grant-suitable-for-my-app"></a>De impliciete subsidie geschikt is voor mijn app?

De impliciete subsidie brengt meer risico's dan andere subsidies. Het moet u aandacht besteden aan gebieden zijn duidelijk gedocumenteerd (Zie bijvoorbeeld [Misbruik van Access Token imiteren Resource eigenaar in impliciete Flow] [ OAuth2-Spec-Implicit-Misuse] en [OAuth 2.0 bedreiging Model en beveiligingsoverwegingen][OAuth2-Threat-Model-And-Security-Implications]). De hogere risicoprofiel is echter grotendeels vanwege het feit dat het is bedoeld voor toepassingen die actieve code, die wordt bediend door een externe bron naar een browser worden uitgevoerd. Als u van plan bent een architectuur SPA hebben geen back-end componenten of van plan bent een Web via JavaScript-API aanroepen, verdient de impliciete stroom gebruiken voor de aanschaf van token.

Als uw toepassing een client is, niet de impliciete stroom is een prima geschikt. Het ontbreken van de sessie-cookie Azure AD in de context van een native client ontneemt de toepassing van de middelen van het onderhouden van een sessie met lange levensduur. Wat betekent dat uw toepassing wordt de gebruiker herhaaldelijk gevraagd bij het verkrijgen van toegangstokens voor nieuwe resources.

Als een webtoepassing die bestaat uit een back-end en een API van de back-end-code die u ontwikkelt, is de impliciete stroom ook niet thuishoort. Andere subsidies geven u veel meer energie. Bijvoorbeeld kunt de OAuth2 client referenties subsidie verkrijgen van tokens die overeenkomen met de machtigingen die zijn toegewezen aan de toepassing zelf, in plaats van de delegaties van de gebruiker. Dit betekent dat de client heeft de mogelijkheid om zelfs wanneer een gebruiker is niet actief in een sessie die programmatische toegang tot bronnen te onderhouden. Niet alleen dat, maar dergelijke subsidies geven hogere veiligheid waarborgen. Bijvoorbeeld toegangstokens nooit doorvoer via de browser van de gebruiker, die geen risico's worden opgeslagen in de browsegeschiedenis, enzovoort. De clienttoepassing kan ook sterke verificatie uitvoeren bij het aanvragen van een token.

## <a name="next-steps"></a>Volgende stappen

- Voor een volledige lijst van bronnen voor ontwikkelaars, met inbegrip van informatie over de protocollen en de OAuth2 vergunning verlenen ondersteuning door AD Azure stromen, raadpleegt u de [Azure AD Developer's Guide][AAD-Developers-Guide]
- [Het integreren van een toepassing met Azure Active Directory] Zie [ ACOM-How-To-Integrate] voor meer diepte in het integratieproces van toepassing.

<!--Image references-->

<!--Reference style links in use-->
[AAD-Developers-Guide]: active-directory-developers-guide.md
[ACOM-How-And-Why-Apps-Added-To-AAD]: active-directory-how-applications-are-added.md
[ACOM-How-To-Integrate]: active-directory-how-to-integrate.md
[OAuth2-Spec-Implicit-Misuse]: https://tools.ietf.org/html/rfc6749#section-10.16 
[OAuth2-Threat-Model-And-Security-Implications]: https://tools.ietf.org/html/rfc6819

