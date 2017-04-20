###<a name="server-auth"></a>Procedure: verifiëren bij een Provider (Server-stroom)

Als u wilt dat mobiele Apps het verificatieproces in uw app beheren, moet u uw app registreren bij de identiteitsprovider van uw. In uw Azure App-Service moet u voor het configureren van de toepassings-ID en een geheim dat is verstrekt door uw provider.
Zie voor meer informatie de handleiding [authentication toevoegen aan uw app].

Nadat u uw identiteitsprovider hebt geregistreerd, belt u gewoon de .login()-methode met de naam van uw provider. Als u bijvoorbeeld aanmelden met Facebook-gebruik de volgende code.

```
client.login("facebook").done(function (results) {
     alert("You are now logged in as: " + results.userId);
}, function (err) {
     alert("Error: " + err);
});
```

Als u een identiteit Facebook-voorziening gebruikt, wijzigt de waarde doorgegeven aan de methode aanmelden boven aan een van de volgende: `microsoftaccount`, `facebook`, `twitter`, `google`, of `aad`.

In dit geval beheert Azure App-Service de verificatie OAuth 2.0 stroom door de aanmeldingspagina van de geselecteerde provider weer te geven en het genereren van een verificatietoken App-Service na een geslaagde aanmelding met de id-provider. De functie login na afloop een JSON-object retourneert (gebruiker) die de gebruikers-ID en de App Service verificatietoken in de velden gebruikersnaam en authenticationToken, respectievelijk. Dit token kan worden in de cache opgeslagen en opnieuw gebruikt totdat deze verloopt.

###<a name="client-auth"></a>Procedure: verifiëren bij een Provider (Client-stroom)

Uw app kan ook onafhankelijk van elkaar contact op met de identiteitsprovider en geef vervolgens het resulterende token met uw App Service voor verificatie. Deze client-overdracht kunt u biedt gebruikers een ervaring voor aanmelden of meer gegevens ophaalt uit de identiteitsprovider.

#### <a name="social-authentication-basic-example"></a>Sociale basic Authentication-voorbeeld

In dit voorbeeld wordt de Facebook client SDK voor verificatie:

```
client.login(
     "facebook",
     {"access_token": token})
.done(function (results) {
     alert("You are now logged in as: " + results.userId);
}, function (err) {
     alert("Error: " + err);
});
```
In dit voorbeeld wordt ervan uitgegaan dat het token dat is opgegeven door de desbetreffende provider SDK is opgeslagen in de variabele token.

#### <a name="microsoft-account-example"></a>Voorbeeld van de Microsoft-Account

In het volgende voorbeeld wordt de Live SDK ondersteunt single-sign-on voor Windows Store apps met behulp van Microsoft-Account:

```
WL.login({ scope: "wl.basic"}).then(function (result) {
      client.login(
            "microsoftaccount",
            {"authenticationToken": result.session.authentication_token})
      .done(function(results){
            alert("You are now logged in as: " + results.userId);
      },
      function(error){
            alert("Error: " + err);
      });
});
```

In het volgende voorbeeld haalt een token uit Live verbinding, dat wordt geleverd met uw App Service door het aanroepen van de functie van de aanmelding.

###<a name="auth-getinfo"></a>Procedure: informatie over de geverifieerde gebruiker ophalen

De verificatiegegevens voor de huidige gebruiker kan worden opgehaald uit de `/.auth/me` eindpunt met elke AJAX-methode.  Stel u de `X-ZUMO-AUTH` verificatietoken voor de koptekst.  De verificatietoken wordt opgeslagen in `client.currentUser.mobileServiceAuthenticationToken`.  Als u bijvoorbeeld het ophalen van API gebruiken:

```
var url = client.applicationUrl + '/.auth/me';
var headers = new Headers();
headers.append('X-ZUMO-AUTH', client.currentUser.mobileServiceAuthenticationToken);
fetch(url, { headers: headers })
    .then(function (data) {
        return data.json()
    }).then(function (user) {
        // The user object contains the claims for the authenticated user
    });
```

Ophalen is alleen beschikbaar als een pakket npm of browser downloaden vanaf CDNJS. U kunt ook jQuery of een andere AJAX API voor het ophalen van de gegevens.  Gegevens worden ontvangen als een JSON-object.
