<properties 
    pageTitle="Het overdragen van gebruiker registratie- en abonnement" 
    description="Informatie over het registratie- en gebruikerabonnement aan een derde partij in Azure API beheer delegeren." 
    services="api-management" 
    documentationCenter="" 
    authors="antonba" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="antonba"/>

# <a name="how-to-delegate-user-registration-and-product-subscription"></a>Het overdragen van gebruiker registratie- en abonnement

Overdracht kunt u uw bestaande website gebruiken voor het verwerken van ontwikkelaar sign-in/sign-up-to-date en een abonnement op producten in plaats van de ingebouwde functionaliteit in de developer-portal. Hiermee kunt uw website aan de eigenaar van de gegevens van de gebruiker en de validatie van deze stappen uitvoeren op een aangepaste manier.

## <a name="delegate-signin-up"> </a>Overdragen developer aanmelden en aanmelden

Overdragen van ontwikkelaar aanmelden en aanmelden met uw bestaande website, moet u een speciale delegatie eindpunt maken op uw site die als het ingangspunt voor een dergelijk verzoek afkomstig zijn van de beheer-API developer-portal fungeert.

De uiteindelijke workflow zijn als volgt:

1. Klikken op de link Aanmelden of aanmelden bij de developer-portal Management API Developer
2. Browser wordt omgeleid naar het eindpunt van de delegatie
3. Eindpunt van de delegatie wordt omgeleid naar in ruil of vraagt de gebruiker aanmelden of aanmelden UI presenteert
4. Op succes, wordt de gebruiker omgeleid naar de beheer-API developer portal-pagina die ze vanuit gestart


Om te beginnen aanvragen gaan we eerste set-up API-beheer voor het routeren van via het eindpunt van de delegatie. Klik op **beveiliging** en klik vervolgens op het tabblad **overdracht** in de portal Management API voor publisher. Schakel het selectievakje in om in te schakelen gemachtigde-in- en aanmelding.

![Overdracht-pagina][api-management-delegation-signin-up]

* Bepalen wat de URL van het eindpunt van de speciale delegatie wordt en deze invoeren in het veld **overdracht eindpunt-URL** . 

* Voer een geheim dat wordt gebruikt voor het berekenen van een handtekening die u ter controle om ervoor te zorgen dat verzoek inderdaad is afkomstig van Azure API beheer binnen het veld **verificatiesleutel overdracht** . Kunt u de knop **genereren** als u wilt dat de API Managemnet willekeurig sleutel genereren voor u.

U moet nu om de **overdracht eindpunt**te maken. Het heeft een aantal acties uitvoeren:

1. Een aanvraag wordt in de volgende vorm:

    > *http://www.yourwebsite.com/apimdelegation?operation=Signin&returnUrl= {URL van bronpagina} & zout = {string} & sig = {string}*

    Query-parameters voor de case-in- / aanmelding:
    - **bewerking**: geeft aan welk soort overdracht verzoek is - **aanmelding** kan alleen in dit geval worden
    - **returnUrl**: de URL van de pagina waar de gebruiker op een koppeling aanmelden of aanmelden klikt
    - **zout**: een speciale tekenreeks die wordt gebruikt voor het berekenen van een hash beveiliging
    - **SIG**: een hash berekende beveiliging moet worden gebruikt voor de vergelijking met uw eigen berekende hash

2. Verifiëren dat de aanvraag is afkomstig van Azure API Management (optioneel, maar sterk aanbevolen voor beveiliging)

    * Berekent een HMAC SHA512-hash van een string op basis van de queryparameters **returnUrl** en **zout** ([voorbeeldcode hieronder]):
        > HMAC (**zout** + '\n' + **returnUrl**)
         
    * De hierboven berekende hash op de waarde van de queryparameter **sig** vergelijken. Als de twee hashwaarden vergelijken, gaat u verder met de volgende stap, anders de aanvraag afwijzen.

2. Controleer dat u een verzoek om sign-in/teken-up ontvangt: de **bewerking** query-parameter wordt ingesteld op '**aanmelden**'.

3. Presenteren aan de gebruiker met de gebruikersinterface voor aanmelden of aanmelden

4. Als de gebruiker ondertekening-up is hebt u een bijbehorende account voor hen maken in de beheer-API. [Een gebruiker] met de API beheer REST API. Wanneer u dit doet, moet u ervoor zorgen de gebruikers-ID in te stellen op hetzelfde is in het archief van de gebruiker of een id die u kunt bijhouden van.

5. Wanneer de gebruiker is geverifieerd:

    * [een eenmalige aanmelding (SSO)-token aanvragen] via de API beheer REST API

    * een queryparameter returnUrl toevoegen aan de SSO-URL die u hebt ontvangen van de API-aanroep hierboven:
        > bv. https://customer.portal.azure-api.net/signin-sso?token&returnUrl=/return/url 

    * de gebruiker omleiden naar de bovenstaande geproduceerde URL

Bovenop het **inloggen** , kunt u ook account beheren door de vorige stappen uit te voeren en met behulp van een van de volgende bewerkingen uitvoeren.

-   **ChangePassword**
-   **ChangeProfile**
-   **CloseAccount**

U moet de volgende parameters query van het account beheer.

-   **bewerking**: geeft aan welk type verzoek om taakoverdracht (ChangePassword, ChangeProfile of CloseAccount)
-   **gebruikers-id**: de id van de account beheren
-   **zout**: een speciale tekenreeks die wordt gebruikt voor het berekenen van een hash beveiliging
-   **SIG**: een hash berekende beveiliging moet worden gebruikt voor de vergelijking met uw eigen berekende hash

## <a name="delegate-product-subscription"> </a>Abonnement product overdragen

Overdragen van abonnement product werkt net als gebruiker aanmelden /-up overdragen. De uiteindelijke workflow zou als volgt zijn:

1. Ontwikkelaar een product selecteert in de beheer-API developer portal en klikt op de knop abonneren
2. Browser wordt omgeleid naar het eindpunt van de delegatie
3. Overdracht eindpunt product vereist abonnement stappen uitvoert - dit is aan u en kan leiden tot het omleiden naar een andere pagina voor het aanvragen van informatie over facturering, extra vragen, of gewoon de informatie op te slaan en geen actie van de gebruiker vereist


Klik op de pagina **overdracht** zodat de functionaliteit **gemachtigde product abonnement**.

Zorg er vervolgens voor dat het eindpunt van de delegatie worden de volgende acties uitgevoerd:


1. Een aanvraag wordt in de volgende vorm:

    > *http://www.yourwebsite.com/apimdelegation?operation= {bewerking} & productId {product wilt abonneren op} = & gebruikers-id = {gebruiker aanvraag} & zout = {string} & sig = {string}*

    Query-parameters voor het geval product abonnement:
    - **bewerking**: geeft aan welk type verzoek om overdracht is. Voor product-abonnement zijn aanvragen geldige opties:
        - "Abonneren": een aanvraag voor een abonnement van de gebruiker naar een bepaald product met die ID (Zie hieronder)
        - "Afmelden": een verzoek te willen ontvangen voor een gebruiker van een product
        - "Vernieuwen": een verzoek om verlenging van een abonnement (bv. die kan worden verlopen)
    - **product-id**: de ID van het product die de gebruiker heeft aangevraagd voor een abonnement op
    - **gebruikers-id**: de ID van de gebruiker voor wie het verzoek is ingediend
    - **zout**: een speciale tekenreeks die wordt gebruikt voor het berekenen van een hash beveiliging
    - **SIG**: een hash berekende beveiliging moet worden gebruikt voor de vergelijking met uw eigen berekende hash


2. Verifiëren dat de aanvraag is afkomstig van Azure API Management (optioneel, maar sterk aanbevolen voor beveiliging)

    * Een HMAC-SHA512 van een tekenreeks op basis van de **product-id**, **gebruikersnaam** en **zout** queryparameters berekenen:
        > HMAC (**zout** '\n' + **productId** + '\n' + **userId**)
         
    * De hierboven berekende hash op de waarde van de queryparameter **sig** vergelijken. Als de twee hashwaarden vergelijken, gaat u verder met de volgende stap, anders de aanvraag afwijzen.
    
3. Uitvoeren van product abonnement verwerking op basis van het type bewerking in **bewerking** - bijvoorbeeld facturering vragen, enz.

4. Abonneren op abonneren is het product aan de kant van de gebruiker, de gebruiker aan het product Management API door het [aanroepen van de overige API voor product-abonnement].

## <a name="delegate-example-code"></a> Voorbeeldcode ##

Deze codevoorbeelden hoe neemt u de *validatiesleutel delegatie*, die in het scherm van de delegatie van de portal voor publisher is ingesteld voor het maken van een HMAC die vervolgens wordt gebruikt voor het valideren van de handtekening, waaruit blijkt dat de geldigheid van de returnUrl doorgegeven. Dezelfde code werkt voor de product-id en de gebruikersnaam met een kleine wijziging.

**C#-code genereren hash van returnUrl**

    using System.Security.Cryptography;

    string key = "delegation validation key";
    string returnUrl = "returnUrl query parameter";
    string salt = "salt query parameter";
    string signature;
    using (var encoder = new HMACSHA512(Convert.FromBase64String(key)))
    {
        signature = Convert.ToBase64String(encoder.ComputeHash(Encoding.UTF8.GetBytes(salt + "\n" + returnUrl)));
        // change to (salt + "\n" + productId + "\n" + userId) when delegating product subscription
        // compare signature to sig query parameter
    }


**NodeJS code genereren hash van returnUrl**

    var crypto = require('crypto');
    
    var key = 'delegation validation key'; 
    var returnUrl = 'returnUrl query parameter';
    var salt = 'salt query parameter';
    
    var hmac = crypto.createHmac('sha512', new Buffer(key, 'base64'));
    var digest = hmac.update(salt + '\n' + returnUrl).digest();
    // change to (salt + "\n" + productId + "\n" + userId) when delegating product subscription
    // compare signature to sig query parameter
    
    var signature = digest.toString('base64');

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over overdracht, de volgende video.

> [AZURE.VIDEO delegating-user-authentication-and-product-subscription-to-a-3rd-party-site]

[Delegating developer sign-in and sign-up]: #delegate-signin-up
[Delegating product subscription]: #delegate-product-subscription
[aanvraag voor een eenmalige aanmelding (SSO)-token]: http://go.microsoft.com/fwlink/?LinkId=507409
[een gebruiker maken]: http://go.microsoft.com/fwlink/?LinkId=507655#CreateUser
[de REST API aanroepen voor product-abonnement]: http://go.microsoft.com/fwlink/?LinkId=507655#SSO
[Next steps]: #next-steps
[van de voorbeeldcode hieronder]: #delegate-example-code

[api-management-delegation-signin-up]: ./media/api-management-howto-setup-delegation/api-management-delegation-signin-up.png 