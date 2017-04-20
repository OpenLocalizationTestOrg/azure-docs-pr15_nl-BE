<properties
    pageTitle="Het publiceren van native clienttoepassingen met proxy toepassingen inschakelen | Microsoft Azure"
    description="Het inschakelen van native clienttoepassingen om te communiceren met Azure AD Application Proxy Connector naar veilige externe toegang tot uw toepassingen in gebouwen bedekt."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/22/2016"
    ms.author="kgremban"/>

# <a name="how-to-enable-native-client-apps-to-interact-with-proxy-applications"></a>Het inschakelen van native Clienttoepassingen communiceren met toepassingen van proxy

Azure Active Directory Application Proxy wordt veelal gebruikt voor het publiceren van browsertoepassingen zoals SharePoint, Outlook Web Access en aangepaste aantal zakelijke toepassingen. Het kan ook worden gebruikt voor het publiceren van native clienttoepassingen van web apps verschillen omdat ze geïnstalleerd op een apparaat. Dit wordt gedaan door de ondersteuning van Azure AD uitgegeven tokens die worden verzonden als standaard toestaan van HTTP-headers.

![Relatie tussen eindgebruikers en Azure Active Directory uitgegeven toepassingen](./media/active-directory-application-proxy-native-client/richclientflow.png)

De aanbevolen methode voor het publiceren van dergelijke toepassingen is de Azure AD verificatie Library, die zorgt voor alle verificatie-gedoe en ondersteunt veel verschillende client-omgevingen gebruiken. Toepassingsproxy past in de [Oorspronkelijke toepassing Web API scenario](active-directory-authentication-scenarios.md#native-application-to-web-api). Het proces voor het uitvoeren van deze is als volgt:

## <a name="step-1-publish-your-application"></a>Stap 1: Uw toepassing publiceren

Uw proxy-toepassing publiceert als een andere toepassing, gebruikers toewijzen en geeft u premium of basic licenties. Zie [toepassingen publiceren met de Proxy](active-directory-application-proxy-publish.md)voor meer informatie.

## <a name="step-2-configure-your-application"></a>Stap 2: Uw toepassing configureren

De oorspronkelijke toepassing als volgt configureren:

1. Log in om de klassieke Azure portal.
2. Selecteer het pictogram Active Directory in het linker menu en selecteer vervolgens de map.
3. Klik in het bovenste menu op **toepassingen**. Als er geen apps zijn toegevoegd aan de map, wordt deze pagina alleen de koppeling **toevoegen een App** weergegeven. Klik op de link of u kunt ook klikken op op de knop **toevoegen** in de werkbalk.
4. Klik op de koppeling naar **het ontwikkelen van mijn organisatie toepassing toevoegen**op de pagina **Wat wilt u doen** .
5. Geef een naam op voor uw toepassing en **Native clienttoepassing**kiezen op de pagina **Vertel ons over uw toepassing** . Klik op de pijl om door te gaan.
6. De **URI omgeleid** bieden voor de native clienttoepassing op de pagina **gegevens van toepassing** , vervolgens klikt u op het vinkje om te voltooien.

De toepassing is toegevoegd, en u komt dan op de pagina Quick Start voor uw toepassing.

## <a name="step-3-grant-access-to-other-applications"></a>Stap 3: Verlenen toegang tot andere toepassingen

Schakel in de oorspronkelijke toepassing worden blootgesteld aan andere toepassingen in de map:

1. In het bovenste menu, klik op **toepassingen**, selecteert u de oorspronkelijke toepassing van nieuwe en klik vervolgens op **configureren**.
2. Ga naar de sectie **machtigingen voor andere toepassingen** . Klik op de knop **toepassing toevoegen** en selecteert u de proxy-toepassing die u wilt de oorspronkelijke toepassing toegang verlenen tot en klikt u op het vinkje in de rechterbenedenhoek. Selecteer de nieuwe machtiging **Overgedragen machtigingen** in de vervolgkeuzelijst.

![Machtigingen voor andere toepassingen screenshot - toevoegen toepassing](./media/active-directory-application-proxy-native-client/delegate_native_app.png)

## <a name="step-4-edit-the-active-directory-authentication-library"></a>Stap 4: Active Directory-verificatie bibliotheek bewerken

De oorspronkelijke toepassingscode bewerken in de context van verificatie van de Active Directory verificatie bibliotheek (ADAL) het volgende:

        // Acquire Access Token from AAD for Proxy Application
        AuthenticationContext authContext = new AuthenticationContext("https://login.microsoftonline.com/<TenantId>");
        AuthenticationResult result = authContext.AcquireToken("< Frontend Url of Proxy App >",
                                                        "< Client Id of the Native app>",
                                                        new Uri("< Redirect Uri of the Native App>"),
                                                        PromptBehavior.Never);

        //Use the Access Token to access the Proxy Application
        HttpClient httpClient = new HttpClient();
        httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);
        HttpResponseMessage response = await httpClient.GetAsync("< Proxy App API Url >");

De variabelen moeten worden vervangen door:

- **TenantId** kan worden gevonden in de GUID in de URL van de pagina **configuratie** van de toepassing direct na '/ map /'.
- **Frontend-URL** is de URL van de front-end u hebt opgegeven in de Proxy-toepassing en kunt u vinden op de pagina **configuratie** van de proxy-app.
- **Client-Id** van de native app kunt u vinden op de pagina van het **configureren** van de oorspronkelijke toepassing.
- **Redirect URI van de native app** kunt u vinden op de pagina van het **configureren** van de oorspronkelijke toepassing.

![Nieuwe originele toepassing screenshot pagina configureren](./media/active-directory-application-proxy-native-client/new_native_app.png)

Zie voor meer informatie over de oorspronkelijke toepassing stroom [in de toepassing waarmee het web API](active-directory-authentication-scenarios.md#native-application-to-web-api).


## <a name="see-also"></a>Zie ook

- [Uitgeven van toepassingen met behulp van uw eigen domeinnaam](active-directory-application-proxy-custom-domains.md)
- [Voorwaardelijke toegang](active-directory-application-proxy-conditional-access.md)
- [Werken met vorderingen op toepassingen](active-directory-application-proxy-claims-aware-apps.md)
- [Eenmalige inschakelen](active-directory-application-proxy-sso-using-kcd.md)

Bekijk de [toepassingsproxy blog](http://blogs.technet.com/b/applicationproxyblog/) voor het laatste nieuws en updates
