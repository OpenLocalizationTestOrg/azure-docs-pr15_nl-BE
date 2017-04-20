<properties
    pageTitle="Azure Active Directory B2C: Aanpassingen van de gebruikersinterface (UI) | Microsoft Azure"
    description="Een onderwerp op de gebruiker aanpassingsfuncties in Azure Active Directory B2C de gebruikersinterface (UI)"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/25/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-customize-the-azure-ad-b2c-user-interface-ui"></a>Azure Active Directory B2C: De Azure AD B2C-gebruikersinterface (UI) aanpassen

Gebruikerservaring is uitermate belangrijk in een consument gerichte toepassing. Het verschil tussen een goede toepassing en een fantastische en tussen consumenten alleen actief en echt ingeschakeld is. Azure Active Directory (AD Azure) B2C kunt u aanpassen consument aanmelding aanmelden (*Zie de onderstaande opmerking*), profiel bewerken, en pagina's met pixel perfect-besturingselement opnieuw instellen van wachtwoord.

> [AZURE.NOTE]
Op dit moment lokale account aanmelden pagina's, het wachtwoord van de accompaning's herstellen en verificatie-e-mailberichten alleen met behulp van de [huisstijl functie bedrijf](../active-directory/active-directory-add-company-branding.md) en niet door de mechanismen die in dit artikel worden beschreven kunnen worden aangepast.

In dit artikel leest u over:

- De pagina gebruiker gebruikersinterface (UI) functie voor het aanpassen.
- Een hulpprogramma waarmee u de functie pagina gebruikersinterface aanpassen met behulp van onze voorbeeldinhoud testen.
- Core elementen van de gebruikersinterface in elk type pagina.
- Aanbevolen procedures bij het uitoefenen van deze functie.

## <a name="the-page-ui-customization-feature"></a>De functie aanpassing UI

Met de functie pagina UI-aanpassingen kunt u het uiterlijk van de consument aanmelden, inloggen, wachtwoord opnieuw instellen en bewerken van profiel pagina's (door het [beleid](active-directory-b2c-reference-policies.md)configureren). De consument heeft bij het navigeren tussen de toepassing en de pagina's die worden bediend door Azure AD B2C naadloze oplossingen.

In tegenstelling tot andere services waar de opties voor de gebruikersinterface zijn beperkt of zijn alleen beschikbaar via API's gebruikt Azure AD B2C een moderne (en eenvoudiger) benadering aan pagina aanpassen.

Hier ziet u hoe het werkt: Azure AD B2C code uitgevoerd in de browser van de consument en een moderne aanpak genoemd [Cross oorsprong Resource Sharing (CORS)](http://www.w3.org/TR/cors/) gebruikt om inhoud te laden vanuit een URL die u in een beleid opgeeft. Kunt u verschillende URL's voor verschillende pagina's. De code van Azure AD B2C gebruikersinterface-elementen samengevoegd met de inhoud van de URL wordt geladen en de pagina wordt weergegeven voor de consument. U hoeft te doen is:

1. Maken van juist opgemaakte HTML5 inhoud met een `<div id="api"></div>` element (moet zijn van een leeg element) zich ergens in de `<body>`. Dit element geplaatst waar de Azure AD B2C-inhoud wordt ingevoegd.
2. Host uw inhoud op een HTTPS-eindpunt (met CORS toegestaan).
3. Stijl elementen van de gebruikersinterface die in Azure AD B2C worden ingevoegd.

## <a name="test-out-the-ui-customization-feature"></a>Testen van de functie van de gebruikersinterface aanpassen

Als u uitproberen van de functie van de gebruikersinterface aanpassen wilt met behulp van ons voorbeeld HTML en CSS-inhoud, hebben we geleverd, kunt u [een eenvoudig hulpprogramma hulpprogramma](active-directory-b2c-reference-ui-customization-helper-tool.md) worden geüpload en voorbeeldinhoud configureert op uw Azure Blob-opslag.

> [AZURE.NOTE]
U kunt de inhoud van uw UI ergens hosten: op webservers CDN, AWS S3, bestandssystemen delen, enz. Als de inhoud wordt gehost op een openbaar toegankelijke HTTPS-eindpunt (met CORS toegestaan), bent u goed om te gaan. We ter illustratie alleen Azure Blob-opslag gebruiken.

### <a name="the-most-basic-html-content-for-testing"></a>De meest eenvoudige HTML-inhoud voor het testen

Hieronder is de meest eenvoudige HTML-inhoud die u gebruiken kunt voor het testen van deze mogelijkheid. Gebruik het dezelfde helper eerder aangeboden om te uploaden en deze inhoud op uw Azure Blob-opslag configureren. U kunt vervolgens controleren of de knoppen voor eenvoudige, niet-vorm van een gestileerde & formuliervelden op elke pagina weergegeven en functionele worden.

```HTML

<!DOCTYPE html>
<html>
    <head>
        <title>!Add your title here!</title>
    </head>
    <body>
        <div id="api"></div>    <!-- IMP: This element is intentionally empty; don't enter anything here -->
    </body>
</html>

```

## <a name="the-core-ui-elements-in-each-type-of-page"></a>De core UI elementen in elk type pagina

In de volgende secties vindt u voorbeelden van HTML5 fragmenten die Azure AD B2C worden samengevoegd in de `<div id="api"></div>` element zich in uw inhoud. **Geen deze fragmenten invoegen in uw HTML-5-inhoud.** De service Azure AD B2C ingevoegd in tijdens runtime. Gebruik deze voorbeelden voor het ontwerpen van uw eigen opmaakmodellen.

### <a name="azure-ad-b2c-content-inserted-into-the-identity-provider-selection-page"></a>Azure AD B2C inhoud ingevoegd in de 'Identity providerpagina"

Deze pagina bevat een lijst met id-providers die de gebruiker uit tijdens het aanmelden of aanmelden kiezen kan. Dit zijn sociale identiteitsproviders zoals Facebook en Google + of lokale accounts (gebaseerd op e-mailadres of groepsnaam).

```HTML

<div id="api" data-name="IdpSelections">
    <div class="intro">
         <p>Sign up</p>
    </div>

    <div>
        <ul>
            <li>
                <button class="accountButton" id="FacebookExchange">Facebook</button>
            </li>
            <li>
                <button class="accountButton" id="GoogleExchange">Google+</button>
            </li>
            <li>
                <button class="accountButton" id="SignUpWithLogonEmailExchange">Email</button>
            </li>
        </ul>
    </div>
</div>

```

### <a name="azure-ad-b2c-content-inserted-into-the-local-account-sign-up-page"></a>Azure AD B2C inhoud ingevoegd in de 'lokale account aanmelden pagina'

Deze pagina bevat een inschrijfformulier die de gebruiker heeft om in te vullen bij het aanmelden voor een lokale account die is gebaseerd op een e-mailadres of gebruikersnaam. Het formulier kan bevatten besturingselementen zoals tekstinvoervak wachtwoordinvoervak, keuzerondje, één vervolgkeuzelijsten en selectievakjes multi input.

```HTML

<div id="api" data-name="SelfAsserted">
    <div class="intro">
        <p>Create your account by providing the following details</p>
    </div>

    <div id="attributeVerification">
        <div class="errorText" id="passwordEntryMismatch" style="display: none;">The password entry fields do not match. Please enter the same password in both fields and try again.</div>
        <div class="errorText" id="requiredFieldMissing" style="display: none;">A required field is missing. Please fill out all required fields and try again.</div>
        <div class="errorText" id="fieldIncorrect" style="display: none;">One or more fields are filled out incorrectly. Please check your entries and try again.</div>
        <div class="errorText" id="claimVerificationServerError" style="display: none;"></div>
        <div class="attr" id="attributeList">
            <ul>
                <li>
                    <div class="attrEntry validate">
                        <div>
                            <div class="verificationInfoText" id="email_intro" style="display: inline;">Verification is necessary. Please click Send button.</div>
                            <div class="verificationInfoText" id="email_info" style="display:none">Verification code has been sent to your inbox. Please copy it to the input box below.</div>
                            <div class="verificationSuccessText" id="email_success" style="display:none">E-mail address verified. You can now continue.</div>
                            <div class="verificationErrorText" id="email_fail_retry" style="display:none">Incorrect code, try again.</div>
                            <div class="verificationErrorText" id="email_fail_no_retry" style="display:none">Exceeded number of retries you need to send new code.</div>
                            <div class="verificationErrorText" id="email_fail_server" style="display:none">Server error, please try again</div>
                            <div class="verificationErrorText" id="email_incorrect_format" style="display:none">Incorect format.</div>
                        </div>

                    <div class="helpText show">This information is required</div>
                        <label>Email</label>
                        <input id="email" class="textInput" type="text" placeholder="Email" required="" autofocus=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Email address that can be used to contact you.');" class="tiny">What is this?</a>

                    <div class="buttons verify" claim_id="email">
                        <div id="email_ver_wait" class="working" style="display: none;"></div>
                            <label id="email_ver_input_label" for="email_ver_input" style="display: none;">Verification code</label>
                            <input id="email_ver_input" type="text" placeholder="Verification code" style="display:none">
                            <button id="email_ver_but_send" class="sendButton" type="button" style="display: inline;">Send verification code</button>
                            <button id="email_ver_but_verify" class="verifyButton" type="button" style="display:none">Verify code</button>
                            <button id="email_ver_but_resend" class="sendButton" type="button" style="display:none">Send new code</button>
                            <button id="email_ver_but_edit" class="editButton" type="button" style="display:none">Change e-mail</button>
                            <button id="email_ver_but_default" class="defaultButton" type="button" style="display:none">Default</button>
                        </div>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText">8-16 characters, containing 3 out of 4 of the following: Lowercase characters, uppercase characters, digits (0-9), and one or more of the following symbols: @ # $ % ^ &amp; * - _ + = [ ] { } | \ : ' , ? / ` ~ " ( ) ; .This information is required</div>
                        <label>Enter password</label>
                        <input id="password" class="textInput" type="password" placeholder="Enter password" pattern="^((?=.*[a-z])(?=.*[A-Z])(?=.*\d)|(?=.*[a-z])(?=.*[A-Z])(?=.*[^A-Za-z0-9])|(?=.*[a-z])(?=.*\d)(?=.*[^A-Za-z0-9])|(?=.*[A-Z])(?=.*\d)(?=.*[^A-Za-z0-9]))([A-Za-z\d@#$%^&amp;*\-_+=[\]{}|\\:',?/`~&quot;();!]|\.(?!@)){8,16}$" title="8-16 characters, containing 3 out of 4 of the following: Lowercase characters, uppercase characters, digits (0-9), and one or more of the following symbols: @ # $ % ^ &amp; * - _ + = [ ] { } | \ : ' , ? / ` ~ &quot; ( ) ; ." required=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Enter password');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText"> This information is required</div>
                        <label>Reenter password</label>
                        <input id="reenterPassword" class="textInput" type="password" placeholder="Reenter password" pattern="^((?=.*[a-z])(?=.*[A-Z])(?=.*\d)|(?=.*[a-z])(?=.*[A-Z])(?=.*[^A-Za-z0-9])|(?=.*[a-z])(?=.*\d)(?=.*[^A-Za-z0-9])|(?=.*[A-Z])(?=.*\d)(?=.*[^A-Za-z0-9]))([A-Za-z\d@#$%^&amp;*\-_+=[\]{}|\\:',?/`~&quot;();!]|\.(?!@)){8,16}$" title=" " required=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Reenter password');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText">This information is required</div>
                        <label>Name</label>
                        <input id="displayName" class="textInput" type="text" placeholder="Name" required=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Your display name.');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText"></div>
                        <label>Gender</label>
                        <input id="extension_Gender_F" name="extension_Gender" type="radio" value="F" autofocus="">
                        <label for="extension_Gender_F">Female</label>
                        <input id="extension_Gender_M" name="extension_Gender" type="radio" value="M">
                        <label for="extension_Gender_M">Male</label>
                        <a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText"></div>
                        <label>Loyalty number</label>
                        <input id="extension_MemNum" class="textInput" type="text" placeholder="Loyalty number"><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Membership number');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText"></div>
                        <label>State</label>
                        <select class="dropdown_single" id="state">
                            <option style="display:none" disabled="disabled" value="placeholder" selected="">State</option>
                            <option value="WA">Washington</option>
                            <option value="NY">New York</option>
                            <option value="CA">California</option>
                        </select>
                        <a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Your residential state or province.');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText">This information is required</div>
                        <label>Zip code</label>
                        <input id="postalCode" class="textInput" type="text" placeholder="Zip code" required=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('The postal code of your address.');" class="tiny">What is this?</a>
                    </div>
                </li>
            </ul>
        </div>
        <div class="buttons"> <button id="continue" disabled="">Create</button> <button id="cancel">Cancel</button></div>
    </div>
    <div class="verifying-modal">
        <div class="preloader"> <img src="https://login.microsoftonline.com/static/img/win8loader.gif" alt="Please wait"></div>
        <div id="verifying_blurb"></div>
    </div>
</div>

```

### <a name="azure-ad-b2c-content-inserted-into-the-social-account-sign-up-page"></a>Azure AD B2C inhoud ingevoegd in de '' sociale account aanmelden pagina'

Deze pagina bevat een inschrijfformulier met de consument in te vullen bij het aanmelden met een bestaande account bij een identiteitsprovider van sociale zoals Facebook of Google +. Deze pagina is vergelijkbaar met de lokale account aanmeldpagina (weergegeven in de vorige sectie) met uitzondering van de velden wachtwoord invoeren.

### <a name="azure-ad-b2c-content-inserted-into-the-unified-sign-up-or-sign-in-page"></a>Azure AD B2C inhoud ingevoegd in de 'Unified aanmelden of aanmelden pagina'

Deze pagina handelt zowel aanmelding & aanmelden van consumenten, die sociale identiteitsproviders zoals Facebook of Google + of lokale accounts kunnen gebruiken.

```HTML

<div id="api" data-name="Unified">
        <div class="social" role="form">
               <div class="intro">
                       <h2>Sign in with your social account</h2>
               </div>
               <div class="options">
                       <div><button class="accountButton firstButton" id="MicrosoftAccountExchange" tabindex="1">msa</button></div>
                       <div><button class="accountButton" id="FacebookExchange" tabindex="1">fb</button></div>
               </div>
        </div>
        <div class="divider">
               <h2>OR</h2>
        </div>
        <div class="localAccount" role="form">
               <div class="intro">
                       <h2>Sign in with your existing account</h2>
               </div>
               <div class="error pageLevel" aria-hidden="true" style="display: none;">
                       <p role="alert"></p>
               </div>
               <div class="entry">
                       <div class="entry-item">
                               <label for="logonIdentifier">Email Address</label> 
                               <div class="error itemLevel" aria-hidden="true" style="display: none;">
                                      <p role="alert"></p>
                               </div>
                               <input type="email" id="logonIdentifier" name="LogonIdentifier" pattern="^[a-zA-Z0-9.!#$%&amp;’*+/=?^_`{|}~-]+@[a-zA-Z0-9-]+(?:\.[a-zA-Z0-9-]+)*$" placeholder="LogonIdentifier" value="" tabindex="1">
                       </div>
                       <div class="entry-item">
                               <div class="password-label"> <label for="password">Password</label><a id="forgotPassword" tabindex="2">Forgot your password?</a></div>
                               <div class="error itemLevel" aria-hidden="true" style="display: none;">
                                      <p role="alert"></p>
                               </div>
                               <input type="password" id="password" name="Password" placeholder="Password" tabindex="1">
                       </div>
                       <div class="working"></div>
                       <div class="buttons"> <button id="next" tabindex="1">Sign in</button> </div>
               </div>
               <div class="divider">
                       <h2>OR</h2>
               </div>
               <div class="create">
                       <p>Don't have an account?<a id="createAccount" tabindex="1">Sign up now</a> </p>
               </div>
        </div>
</div>

```

### <a name="azure-ad-b2c-content-inserted-into-the-multi-factor-authentication-page"></a>Azure AD B2C-inhoud op de pagina' meerledige verificatie"ingevoegd

Gebruikers kunnen hun telefoonnummers (met tekst inspreken of spraakopdrachten) controleren tijdens het aanmelden of aanmelden op deze pagina.

```HTML

<div id="api" data-name="Phonefactor">
    <div id="phonefactor_initial">
        <div class="intro">
            <p>Enter a number below that we can send a code via SMS or phone to authenticate you.</p>
        </div>
        <div class="errorText" id="errorMessage" style="display:none"></div>
        <div class="phoneEntry" id="phoneEntry">
            <div class="phoneNumber">
                <select id="countryCode" style="display:inline-block">
                    <option value="+93">Afghanistan (+93)</option>
                    <!-- Not all country codes listed -->
                    <option value="+44">United Kingdom (+44)</option>
                    <option value="+1" selected="">United States (+1)</option>
                    <!-- Not all country codes listed -->
                </select>
            </div>
            <div class="phoneNumber">
                <input type="text" id="localNumber" style="display:inline-block" placeholder="Phone number">
            </div>
        </div>
        <div id="codeVerification" style="display:none">
            <div>
                <p>Enter your verification code below, or <button id="retryCode" class="textButton">send a new code</button></p>
                <input type="text" id="verificationCode" placeholder="Verification code">
            </div>
        </div>
        <div class="buttons">
            <button id="verifyCode" class="sendInitialCodeButton">Send Code</button>
            <button id="verifyPhone" style="display:inline-block">Call Me</button>
            <button id="cancel" style="display:inline-block">Cancel</button>
        </div>
    </div>
    <div class="dialing-modal">
        <div class="preloader"> <img src="https://login.microsoftonline.com/static/img/win8loader.gif" alt="Please wait"></div>
        <div id="dialing_blurb"></div><div id="dialing_number"></div>
    </div>
</div>

```

### <a name="azure-ad-b2c-content-inserted-into-the-error-page"></a>Azure AD B2C inhoud ingevoegd in de '' foutpagina'

```HTML

<div id="api" class="error-page-content" data-name="GlobalException">
    <h2>Sorry, but we're having trouble signing you in.</h2>
    <div class="error-page-help">We track these errors automatically, but if the problem persists feel free to contact us. In the meantime, please try again.</div>
    <div class="error-page-messagedetails">Your administrator hasn't provided any contact details.</div>
    <div class="error-page-messagedetails">
        <div class="error-page-correlationid">Correlation ID:1c4f0397-c6e4-4afe-bf74-42f488f2f15f</div>
        <div>Timestamp:2015-09-14 23:22:35Z</div>
        <div class="error-page-detail">AADB2C90065: A B2C client-side error 'Access is denied.' has occurred requesting the remote resource.</div>
    </div>
</div>

```

## <a name="things-to-remember-when-building-your-own-content"></a>Om te onthouden bij het bouwen van uw eigen inhoud

Als u van plan bent met de functie pagina UI-aanpassingen, lees de volgende richtlijnen:

- Niet de Azure AD B2C standaardinhoud kopiëren en probeert te wijzigen. Het is raadzaam voor het samenstellen van de inhoud van uw HTML5 helemaal en standaardinhoud gebruikt als referentie.
- Alle pagina's (met uitzondering van de foutpagina's) die worden bediend door de aanmelden, aanmelding en het beleid profiel bewerken, trapsgewijze opmaakmodellen die u opgeeft moet overschrijft de standaard stijlpagina's die we toevoegen aan deze pagina's in de <head> fragmenten. In alle pagina's die worden bediend door de aanmelding of -in en wachtwoord opnieuw instellen van beleid en de foutpagina's in alle soorten beleid, hebt u alle opmaak voor uzelf.
- Om veiligheidsredenen kunt niet u alle JavaScript opnemen in uw inhoud. De meeste van wat u nodig hebt zijn uit de doos beschikbaar. Als dit niet het geval is, [Gebruiker spraak](http://feedback.azure.com/forums/169401-azure-active-directory) gebruiken voor het aanvragen van nieuwe functionaliteit.
- Ondersteunde browserversies:
    - Internet Explorer 11
    - Internet Explorer 10
    - Internet Explorer 9 (beperkt)
    - Internet Explorer 8 (beperkt)
    - Google Chrome 43.0
    - Google Chrome 42.0
    - Mozilla Firefox 38.0
    - Mozilla Firefox 37.0
