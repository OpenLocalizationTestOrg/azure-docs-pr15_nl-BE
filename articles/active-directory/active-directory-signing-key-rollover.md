<properties
    pageTitle="Belangrijke Rollover Azure AD aanmelden | Microsoft Azure"
    description="Dit artikel wordt de ondertekening sleutel rollover best practices voor Azure Active Directory"
    services="active-directory"
    documentationCenter=".net"
    authors="gsacavdm"
    manager="krassk"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/18/2016"
    ms.author="gsacavdm"/>

# <a name="signing-key-rollover-in-azure-active-directory"></a>Belangrijkste rollover in Azure Active Directory ondertekenen

In dit onderwerp wordt beschreven wat u moet weten over de openbare sleutels die worden gebruikt in Azure Active Directory (AD Azure) voor het ondertekenen van beveiligingstokens. Het is belangrijk te weten dat deze toetsen rollover op regelmatige basis en in noodgevallen onmiddellijk kan worden overgebracht. Alle toepassingen die gebruikmaken van AD Azure moet kunnen via programmering de belangrijkste rollover-proces verwerken of een periodieke handmatige rollover-proces tot stand brengen. Blijven lezen om te begrijpen hoe de toetsen werken, hoe de beoordeling van de gevolgen van de rollover aan uw toepassing en het bijwerken van uw toepassing of tot stand brengen van een periodieke handmatige rollover-proces voor het afhandelen van belangrijke rollover indien nodig.

## <a name="overview-of-signing-keys-in-azure-ad"></a>Overzicht van het ondertekenen van sleutels in Azure AD

Azure AD maakt gebruik van cryptografie met openbare sleutels gebaseerd op industriestandaards vertrouwensrelatie tussen zichzelf en de toepassingen die gebruikmaken van het tot stand brengen. In de praktijk werkt dit op de volgende manier: Azure AD een handtekeningsleutel die uit een combinatie van openbare en persoonlijke sleutel bestaat wordt gebruikt. Wanneer een gebruiker zich bij een toepassing die gebruikmaakt van Azure AD voor verificatie aanmeldt, maakt Azure AD een beveiligingstoken dat informatie over de gebruiker bevat. Deze token wordt ondertekend door de Azure AD met behulp van de persoonlijke sleutel is voordat het wordt verzonden naar de toepassing. Als u wilt controleren of het token is geldig en daadwerkelijk uit Azure Active Directory is, moet de toepassing van het token handtekening met behulp van de openbare sleutel blootgesteld door Azure AD die is opgenomen in van de huurder [OpenID verbinding discovery](http://openid.net/specs/openid-connect-discovery-1_0.html) of WS-SAML-Fed [federation metagegevensdocument](active-directory-federation-metadata.md)valideren.

Om veiligheidsredenen kan Azure AD ondertekening belangrijke rollen op een periodieke basis en in het geval van een noodsituatie, worden teruggedraaid via onmiddellijk. Elke toepassing die is geïntegreerd met AD Azure moet worden voorbereid voor het verwerken van een rollover belangrijke gebeurtenis, ongeacht hoe vaak deze kan optreden. Als dat niet het geval, de toepassing probeert een verlopen-toets gebruiken om te controleren of de handtekening van een token, mislukt de aanvraag voor aanmelden.

Er is altijd meer dan één geldige sleutel beschikbaar in de OpenID verbinding discovery-document en het document federation-metagegevens. Uw toepassing moet een van de sleutels die zijn opgegeven in het document te gebruiken, omdat één toets snel kan worden gerold, andere kan worden van de vervanging, enzovoort.

## <a name="how-to-assess-if-your-application-will-be-affected-and-what-to-do-about-it"></a>Hoe beoordelen als uw toepassing worden beïnvloed en wat u ermee wilt doen

Hoe uw toepassing verwerkt sleutel rollover is afhankelijk van variabelen zoals het type van toepassing of welke identiteit protocol en bibliotheek is gebruikt. In de volgende secties wordt nagegaan of de meest voorkomende toepassingen zijn getroffen door de sleutel rollover en richtsnoeren te verschaffen voor het bijwerken van de toepassing ondersteunt automatische rollover of handmatig bijwerken van de sleutel.

* [Native client-toepassingen toegang tot bronnen](#nativeclient)
* [Webtoepassingen / API's toegang tot bronnen](#webclient)
* [Webtoepassingen / API's, bescherming van de bronnen en gebouwd met behulp van Azure App Services](#appservices)
* [Webtoepassingen / bescherming van de bronnen met .NET OWIN OpenID, WS Fed of verbinden WindowsAzureActiveDirectoryBearerAuthentication middleware-API's](#owin)
* [Webtoepassingen / bescherming van de bronnen met behulp van .NET Core OpenID verbinding of JwtBearerAuthentication middleware-API's](#owincore)
* [Webtoepassingen / bescherming van de bronnen met Node.js passport-azure-ad module-API's](#passport)
* [Webtoepassingen / API's, bescherming van de bronnen en gemaakt met Visual Studio 2015](#vs2015)
* [Webtoepassingen die bescherming van de bronnen en gemaakt met Visual Studio 2013](#vs2013)
* [Web-API's bescherming van de bronnen en gemaakt met Visual Studio 2013](#vs2013_webapi)
* [Webtoepassingen die bescherming van de bronnen en gemaakt met Visual Studio 2012](#vs2012)
* [Webtoepassingen die bescherming van de bronnen en gemaakt met Visual Studio 2010, 2008 o met behulp van Windows Identity Foundation](#vs2010)
* [Webtoepassingen / bescherming van de bronnen met behulp van een andere bibliotheken of uitvoering van de ondersteunde protocollen handmatig van API's](#other)

Deze richtlijn is **niet** van toepassing op:

* Toepassingen toegevoegd in Azure AD toepassing galerie (inclusief aangepaste) hebben aparte richtlijnen met betrekking tot het ondertekenen van sleutels. [Meer informatie.](active-directory-sso-certs.md)
* Op-gebouwen gepubliceerd via de toepassingsproxy toepassingen geen zorgen te maken over het ondertekenen van sleutels.

### <a name="nativeclient"></a>Native client-toepassingen toegang tot bronnen

Toepassingen die alleen toegang hebben tot bronnen (Internet Explorer Microsoft Graph, KeyVault, API in Outlook en andere Microsoft APIs) in het algemeen alleen een token verkrijgen en deze doorgeven aan de eigenaar van de resource. Gezien het feit dat zij niet voor de beveiliging van bronnen, zij doen het token niet controleren en derhalve niet nodig om ervoor te zorgen dat goed is ondertekend.

Native client-toepassingen, pc of mobiel, in deze categorie vallen en dus niet van invloed is de rollover.

### <a name="webclient"></a>Webtoepassingen / API's toegang tot bronnen

Toepassingen die alleen toegang hebben tot bronnen (Internet Explorer Microsoft Graph, KeyVault, API in Outlook en andere Microsoft APIs) in het algemeen alleen een token verkrijgen en deze doorgeven aan de eigenaar van de resource. Gezien het feit dat zij niet voor de beveiliging van bronnen, zij doen het token niet controleren en derhalve niet nodig om ervoor te zorgen dat goed is ondertekend.

Webtoepassingen en -API's die van de stroom alleen-app gebruikmaken (clientreferenties / clientcertificaat), in deze categorie vallen en dus niet van invloed is de rollover.

### <a name="appservices"></a>Webtoepassingen / API's, bescherming van de bronnen en gebouwd met behulp van Azure App Services

Azure App Services Authentication / Authorization (EasyAuth) functionaliteit heeft al de nodige logica rollover sleutel automatisch verwerken.

### <a name="owin"></a>Webtoepassingen / bescherming van de bronnen met .NET OWIN OpenID, WS Fed of verbinden WindowsAzureActiveDirectoryBearerAuthentication middleware-API's

Als uw toepassing van de .NET OWIN OpenID verbinden, WS Fed of middleware, WindowsAzureActiveDirectoryBearerAuthentication gebruikmaakt, is het al de benodigde logica rollover sleutel automatisch verwerken.

U kunt bevestigen dat uw toepassing gebruikmaakt van een van deze door te zoeken naar een van de volgende fragmenten in de Startup.cs of Startup.Auth.cs van uw toepassing

```
app.UseOpenIdConnectAuthentication(
     new OpenIdConnectAuthenticationOptions
     {
         // ...
     });
```
```
app.UseWsFederationAuthentication(
    new WsFederationAuthenticationOptions
    {
     // ...
    });
```
```
 app.UseWindowsAzureActiveDirectoryBearerAuthentication(
     new WindowsAzureActiveDirectoryBearerAuthenticationOptions
     {
     // ...
     });
```

### <a name="owincore"></a>Webtoepassingen / bescherming van de bronnen met behulp van .NET Core OpenID verbinding of JwtBearerAuthentication middleware-API's

Als uw toepassing van de middleware .NET Core OWIN OpenID verbinding of JwtBearerAuthentication gebruikmaakt, is het al de benodigde logica rollover sleutel automatisch verwerken.

U kunt bevestigen dat uw toepassing gebruikmaakt van een van deze door te zoeken naar een van de volgende fragmenten in de Startup.cs of Startup.Auth.cs van uw toepassing

```
app.UseOpenIdConnectAuthentication(
     new OpenIdConnectAuthenticationOptions
     {
         // ...
     });
```
```
app.UseJwtBearerAuthentication(
    new JwtBearerAuthenticationOptions
    {
     // ...
    });
```

### <a name="passport"></a>Webtoepassingen / bescherming van de bronnen met Node.js passport-azure-ad module-API's

Als uw toepassing van de passport-ad Node.js module gebruikmaakt, is het al de benodigde logica rollover sleutel automatisch verwerken.

U kunt bevestigen dat uw toepassing passport-advertentie door te zoeken naar het volgende fragment in uw toepassing van app.js

```
var OIDCStrategy = require('passport-azure-ad').OIDCStrategy;

passport.use(new OIDCStrategy({
    //...
));
```

### <a name="vs2015"></a>Webtoepassingen / API's, bescherming van de bronnen en gemaakt met Visual Studio 2015

Als uw toepassing is gemaakt met een sjabloon in Visual Studio 2015 en u **Werk en School-Accounts** hebt geselecteerd in het menu **Wijzigen verificatie** , al er de nodige logica rollover sleutel automatisch verwerken. Deze logica, ingesloten in de middleware OWIN OpenID verbinding haalt en de sleutels van de OpenID verbinding discovery-document opslaat in de cache en regelmatig wordt vernieuwd.

Als u verificatie handmatig toegevoegd aan uw oplossing, is uw toepassing mogelijk niet de benodigde sleutel rollover logica. U moet zelf schrijven of volg de stappen in [webtoepassingen / API's met behulp van een andere bibliotheken of handmatig implementeren van een van de ondersteunde protocollen.](#other).

### <a name="vs2013"></a>Webtoepassingen die bescherming van de bronnen en gemaakt met Visual Studio 2013

Als uw toepassing is gemaakt met een sjabloon in Visual Studio 2013 en u **Organisatie-Accounts** hebt geselecteerd in het menu **Wijzigen verificatie** , al er de nodige logica rollover sleutel automatisch verwerken. Deze logica wordt de unieke id van uw organisatie en de belangrijkste handtekeninginformatie in twee databasetabellen die zijn gekoppeld aan het project opgeslagen. U vindt de verbindingsreeks voor de database in het Web.config-bestand van het project.

Als u verificatie handmatig toegevoegd aan uw oplossing, is uw toepassing mogelijk niet de benodigde sleutel rollover logica. U moet zelf schrijven of volg de stappen in [webtoepassingen / API's met behulp van een andere bibliotheken of handmatig implementeren van een van de ondersteunde protocollen.](#other).

De volgende stappen kunt u controleren of de logica goed werkt in uw toepassing.

1. Open de oplossing in Visual Studio 2013, en klik op het tabblad **Server Explorer** in het rechtervenster.
2. Vouw de **gegevensverbindingen**, **DefaultConnection**en **tabellen**. Ga naar de tabel **IssuingAuthorityKeys** , met de rechtermuisknop op en klik op **Gegevens in een tabel weergeven**.
3. In de tabel **IssuingAuthorityKeys** moet er ten minste één rij die overeenkomt met de waarde voor de sleutel. Alle rijen in de tabel verwijderen.
4. Klik met de rechtermuisknop op de tabel **huurders** en klik vervolgens op **Gegevens in een tabel weergeven**.
5. In de tabel **huurders** moet er ten minste één rij die overeenkomt met de id van een unieke map huurder. Alle rijen in de tabel verwijderen. Als u de rijen in de tabel van de **huurder** en de **IssuingAuthorityKeys** -tabel niet verwijdert, krijgt u een fout tijdens runtime.
6. Bouwen en uitvoeren van de toepassing. Nadat u zich hebt aangemeld bij uw account, kunt u de toepassing kunt stoppen.
7. Terug naar de **Server Explorer** en bekijkt u de waarden in de tabel **IssuingAuthorityKeys** en **huurders** . U zult zien dat ze hebben is automatisch opnieuw gevuld met de bijbehorende gegevens uit het document federation-metagegevens.

### <a name="vs2013"></a>Web-API's bescherming van de bronnen en gemaakt met Visual Studio 2013

Als u een web API toepassing gemaakt in Visual Studio 2013 met de sjabloon voor Web-API en vervolgens **Organisatie-Accounts** in het menu **Wijzigen verificatie** geselecteerd, hebt u al de nodige logica in uw toepassing.

Als u handmatig geconfigureerd voor verificatie, volgt u de onderstaande instructies voor meer informatie over het configureren van uw Web-API om de sleutel gegevens automatisch bijgewerkt.

Het volgende stukje code laat zien hoe de nieuwste sleutels ophalen uit het document federation-metagegevens en gebruik vervolgens het [Token JWT-Handler](https://msdn.microsoft.com/library/dn205065.aspx) voor het valideren van het token. Het codefragment wordt verondersteld wordt te gebruiken uw eigen caching mechanisme voor persistent maken van de sleutel voor het valideren van toekomstige tokens van Azure AD, ongeacht of deze in een database, een configuratiebestand of elders.

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.IdentityModel.Tokens;
using System.Configuration;
using System.Security.Cryptography.X509Certificates;
using System.Xml;
using System.IdentityModel.Metadata;
using System.ServiceModel.Security;
using System.Threading;

namespace JWTValidation
{
    public class JWTValidator
    {
        private string MetadataAddress = "[Your Federation Metadata document address goes here]";

        // Validates the JWT Token that's part of the Authorization header in an HTTP request.
        public void ValidateJwtToken(string token)
        {
            JwtSecurityTokenHandler tokenHandler = new JwtSecurityTokenHandler()
            {
                // Do not disable for production code
                CertificateValidator = X509CertificateValidator.None
            };

            TokenValidationParameters validationParams = new TokenValidationParameters()
            {
                AllowedAudience = "[Your App ID URI goes here, as registered in the Azure Classic Portal]",
                ValidIssuer = "[The issuer for the token goes here, such as https://sts.windows.net/68b98905-130e-4d7c-b6e1-a158a9ed8449/]",
                SigningTokens = GetSigningCertificates(MetadataAddress)

                // Cache the signing tokens by your desired mechanism
            };

            Thread.CurrentPrincipal = tokenHandler.ValidateToken(token, validationParams);
        }

        // Returns a list of certificates from the specified metadata document.
        public List<X509SecurityToken> GetSigningCertificates(string metadataAddress)
        {
            List<X509SecurityToken> tokens = new List<X509SecurityToken>();

            if (metadataAddress == null)
            {
                throw new ArgumentNullException(metadataAddress);
            }

            using (XmlReader metadataReader = XmlReader.Create(metadataAddress))
            {
                MetadataSerializer serializer = new MetadataSerializer()
                {
                    // Do not disable for production code
                    CertificateValidationMode = X509CertificateValidationMode.None
                };

                EntityDescriptor metadata = serializer.ReadMetadata(metadataReader) as EntityDescriptor;

                if (metadata != null)
                {
                    SecurityTokenServiceDescriptor stsd = metadata.RoleDescriptors.OfType<SecurityTokenServiceDescriptor>().First();

                    if (stsd != null)
                    {
                        IEnumerable<X509RawDataKeyIdentifierClause> x509DataClauses = stsd.Keys.Where(key => key.KeyInfo != null && (key.Use == KeyType.Signing || key.Use == KeyType.Unspecified)).
                                                             Select(key => key.KeyInfo.OfType<X509RawDataKeyIdentifierClause>().First());

                        tokens.AddRange(x509DataClauses.Select(token => new X509SecurityToken(new X509Certificate2(token.GetX509RawData()))));
                    }
                    else
                    {
                        throw new InvalidOperationException("There is no RoleDescriptor of type SecurityTokenServiceType in the metadata");
                    }
                }
                else
                {
                    throw new Exception("Invalid Federation Metadata document");
                }
            }
            return tokens;
        }
    }
}
```

### <a name="vs2012"></a>Webtoepassingen die bescherming van de bronnen en gemaakt met Visual Studio 2012

Als uw toepassing is gemaakt in Visual Studio 2012, u waarschijnlijk gebruikt de identiteit en de Access-hulpprogramma voor het configureren van uw toepassing. Ook is het waarschijnlijk dat u het [Valideren van de uitgever naam register (VINR)](https://msdn.microsoft.com/library/dn205067.aspx)gebruikt. De VINR is verantwoordelijk voor het onderhouden van informatie over vertrouwde id-providers (Azure AD) en de sleutels die worden gebruikt om door hen uitgegeven tokens te valideren. De VINR kunt u gemakkelijk de belangrijke informatie opgeslagen in een Web.config-bestand door het downloaden van het meest recente federation metagegevensdocument dat is gekoppeld aan de map, automatisch bijwerken als de configuratie bijgewerkt met de meest recente document is controleren en bijwerken van de toepassing voor het gebruik van de nieuwe sleutel als u nodig hebt.

Als u uw toepassing met behulp van een van de voorbeelden van code of scenario documentatie van Microsoft hebt gemaakt, wordt de sleutel rollover logica is al opgenomen in uw project. U ziet dat de onderstaande code al in uw project bestaat. Als uw toepassing niet al deze logica, de volgende stappen toe te voegen en te controleren of deze correct werkt.

1. Voeg een verwijzing naar de assembly **System.IdentityModel** voor het juiste project in de **Solution Explorer**.
2. Open het bestand **Global.asax.cs** en voeg de volgende richtlijnen gebruiken:
```
using System.Configuration;
using System.IdentityModel.Tokens;
```
3. De volgende methode toevoegen aan het bestand **Global.asax.cs** :
```
protected void RefreshValidationSettings()
{
    string configPath = AppDomain.CurrentDomain.BaseDirectory + "\\" + "Web.config";
    string metadataAddress =
                  ConfigurationManager.AppSettings["ida:FederationMetadataLocation"];
    ValidatingIssuerNameRegistry.WriteToConfig(metadataAddress, configPath);
}
```
4. Roep de methode **RefreshValidationSettings()** in de methode **Application_Start()** in **Global.asax.cs** zoals:
```
protected void Application_Start()
{
    AreaRegistration.RegisterAllAreas();
    ...
    RefreshValidationSettings();
}
```

Nadat u deze stappen hebt gevolgd, wordt Web.config van uw toepassing bijgewerkt met de meest recente informatie uit het document federation metagegevens, met inbegrip van de meest recente toetsen. Deze wordt bijgewerkt elke keer dat de groep van toepassingen wordt gerecycled in IIS. IIS is standaard ingesteld op elke 29 uur herhaald.

Volg de onderstaande stappen om te controleren dat de belangrijkste rollover logica werkt.

1. Nadat u hebt vastgesteld dat uw toepassing met behulp van de bovenstaande code, open het bestand **Web.config** en Ga naar de **<issuerNameRegistry>** blok, met name op zoek naar de volgende paar regels:
```
<issuerNameRegistry type="System.IdentityModel.Tokens.ValidatingIssuerNameRegistry, System.IdentityModel.Tokens.ValidatingIssuerNameRegistry">
        <authority name="https://sts.windows.net/ec4187af-07da-4f01-b18f-64c2f5abecea/">
          <keys>
            <add thumbprint="3A38FA984E8560F19AADC9F86FE9594BB6AD049B" />
          </keys>
```
2. In de **<add thumbprint=””>** de waarde instellen, wijzigen door een teken vervangen door een andere. Sla het bestand **Web.config** .

3. De toepassing bouwen en uitvoeren. Als u het aanmeldingsproces voltooien kunt, wordt uw toepassing de sleutel is bijgewerkt door het downloaden van de vereiste informatie uit uw map federation metagegevensdocument. Als er problemen met het aanmelden, of de wijzigingen in uw toepassing correct zijn door het onderwerp [Toe te voegen aanmelding bij uw Web Application Using Azure AD](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) lezen of te downloaden en te controleren in het volgende voorbeeld: [Meerdere huurder wolk toepassing voor Azure Active Directory](https://code.msdn.microsoft.com/multi-tenant-cloud-8015b84b).


### <a name="vs2010"></a>Webtoepassingen die bescherming van de bronnen en gemaakt met Visual Studio 2008 en 2010 en Windows Identity Foundation (WIF) v1.0 voor .NET 3.5

Als u een toepassing op WIF v1.0 gebouwd, is er geen bedoeld mechanisme voor het automatisch vernieuwen van de configuratie van uw toepassing om een nieuwe sleutel te gebruiken.

- *Eenvoudigste manier* Gebruik de gereedschappen FedUtil is opgenomen in de SDK WIF, die kan het meest recente metagegevensdocument ophalen en bijwerken van uw configuratie.
- Uw toepassing .NET 4.5, waaronder de nieuwste versie van WIF zich in de naamruimte van het systeem worden bijgewerkt. Vervolgens kunt u de [Validatie van de uitgever naam register (VINR)](https://msdn.microsoft.com/library/dn205067.aspx) voor het uitvoeren van automatische updates van de configuratie van de toepassing.
- Een rollover handmatig aan de hand van de instructies aan het einde van dit document richtsnoeren uitvoeren.

De FedUtil gebruiken voor het bijwerken van de configuratie-instructies:

1. Controleer of het WIF v1.0 SDK is geïnstalleerd op de ontwikkelcomputer voor Visual Studio 2008 en 2010. U kunt [downloaden vanaf hier](https://www.microsoft.com/en-us/download/details.aspx?id=4451) , als u nog niet hebt geïnstalleerd dit.
2. In Visual Studio opent u de oplossing, klik met de rechtermuisknop op het desbetreffende project en selecteer **federation-metagegevens bijwerkt**. Als deze optie niet beschikbaar is, is FedUtil en/of het WIF v1.0 SDK niet geïnstalleerd.
3. Vanaf de prompt Selecteer **Update** om te beginnen met het bijwerken van metagegevens van de Federatie. Als u toegang tot de server-omgeving waarin de toepassing wordt gehost hebt, kunt u desgewenst de FedUtil [automatisch metagegevens scheduler bijwerken](https://msdn.microsoft.com/library/ee517272.aspx).
4. Klik op **Voltooien** om de update te voltooien.

### <a name="other"></a>Webtoepassingen / bescherming van de bronnen met behulp van een andere bibliotheken of uitvoering van de ondersteunde protocollen handmatig van API's

Als u met behulp van een andere bibliotheek of een van de ondersteunde protocollen handmatig geïmplementeerd, moet u de bibliotheek of de implementatie om ervoor te zorgen dat de sleutel wordt opgehaald uit het document discovery OpenID verbinding of de federation-metagegevensdocument bekijken. Zoek in uw code of code van de bibliotheek voor het oproepen van de OpenID discovery-document of de federation-metagegevensdocument is één manier om dit te controleren.

Als zij belangrijke ergens worden opgeslagen of hardcoded in uw toepassing, u handmatig kunt ophalen van de sleutel en het dienovereenkomstig door uitvoeren van een handmatige rollover aan de hand van de instructies aan het einde van dit document richtsnoeren update. Overzicht **is het aangeraden dat u uw toepassing ter ondersteuning van automatische rollover verbeteren** met behulp van de methoden in dit artikel om te voorkomen dat toekomstige onderbrekingen en overhead als Azure AD stijgt cadence rollover wordt of is een emergency out-of-band rollover.

## <a name="how-to-test-your-application-to-determine-if-it-will-be-affected"></a>Het testen van de toepassing om te bepalen als zij worden beïnvloed

U kunt nagaan of uw toepassing automatische sleutel rollover ondersteunt door te downloaden van de scripts en de instructies in [deze opslagplaats GitHub.](https://github.com/AzureAD/azure-activedirectory-powershell-tokenkey)

## <a name="how-to-perform-a-manual-rollover-if-you-application-does-not-support-automatic-rollover"></a>Een rollover handmatig uitvoeren als u de toepassing biedt geen ondersteuning voor automatische rollover

Als uw toepassing **geen** ondersteuning voor automatische rollover biedt, moet u een proces dat periodiek controleert de handtekeningsleutels Azure AD en voert een handmatige rollover dienovereenkomstig instellen. [Deze GitHub opslagplaats](https://github.com/AzureAD/azure-activedirectory-powershell-tokenkey) bevat scripts en instructies over hoe u dit doet.
