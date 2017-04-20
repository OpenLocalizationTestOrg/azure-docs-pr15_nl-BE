<properties
    pageTitle="Aan de slag met Azure Active Directory identiteit bescherming en Microsoft Graph | Microsoft Azure"
    description="Biedt een inleiding tot de Microsoft Graph voor een lijst met risico's en de bijbehorende informatie van Azure Active Directory."
    services="active-directory"
    keywords="Azure active directory identiteitsbeveiliging, onvoorziene gebeurtenis, beveiligingslek, beveiligingsbeleid, Microsoft Graph"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="markvi"/>

# <a name="get-started-with-azure-active-directory-identity-protection-and-microsoft-graph"></a>Aan de slag met Azure Active Directory identiteit bescherming en Microsoft Graph

Microsoft Graph is van Microsoft unified API eindpunt en het huis van [Azure Active Directory identiteitsbeveiliging van](active-directory-identityprotection.md) API's. Onze eerste API, **identityRiskEvents**, kunt u Microsoft Graph-query voor een lijst met [risico's](active-directory-identityprotection-risk-events-types.md) en de bijbehorende informatie. Dit artikel helpt u op weg bij het controleren van deze API. Voor een diepgaande inleiding, volledige documentatie en toegang tot de Explorer grafiek Zie de [website van Microsoft Graph](https://graph.microsoft.io/).

Er zijn identiteit bescherming van gegevens benaderen via Microsoft Graph in drie stappen:

1. Een toepassing met een geheim van de client wilt toevoegen. 

2. Gebruik dit geheim en een paar andere stukjes informatie om Microsoft Graph, waarbij er een verificatietoken te verifiëren. 

3. Gebruik deze token aanvragen naar het eindpunt van de API en identiteit bescherming van gegevens terug te krijgen.

Voordat u begint, hebt u nodig:

- Administrator-bevoegdheden voor de toepassing in Azure advertentie maken
- De naam van het domein van de huurder (bijvoorbeeld contoso.onmicrosoft.com)



## <a name="add-an-application-with-a-client-secret"></a>Een toepassing met een geheim client toevoegen


1. [Log in](https://manage.windowsazure.com) om uw Azure klassieke portal als beheerder. 

1. Klik in het linkernavigatievenster op **Active Directory**. 

    ![Een toepassing maken](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_01.png)

2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. Klik in het menu aan de bovenkant, **toepassingen**.

    ![Een toepassing maken](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_02.png)

4. Klik op **toevoegen** onder aan de pagina.

    ![Een toepassing maken](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_03.png)

5. Klik in het dialoogvenster **Wat wilt u doen** op **Mijn organisatie ontwikkelt toepassing toevoegen**.

    ![Een toepassing maken](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_04.png)


5. Voer de volgende stappen uit in het dialoogvenster **Vertel ons over uw toepassing** :

    ![Een toepassing maken](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_05.png)

    een. Typ in het tekstvak **naam** een naam voor de toepassing (bijvoorbeeld: AADIP risico Event API Application).

    b. Selecteer als **Type**, **toepassing en/of Web API Web**.

    c. Klik op **volgende**.


5. Voer de volgende stappen uit in het dialoogvenster **Eigenschappen van App** :

    ![Een toepassing maken](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_06.png)

    een. Typ in het tekstvak **URL-On** `http://localhost`.

    b. Typ in het tekstvak **App ID URI** `http://localhost`.

    c. Klik op **Voltooien**.


U kunt nu uw toepassing configureren.

![Een toepassing maken](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_07.png)



## <a name="grant-your-application-permission-to-use-the-api"></a>Uw toepassing machtigen voor gebruik van de API


1. Klik op de pagina van de toepassing in het menu aan de bovenkant, klikt u op **configureren**. 

    ![Een toepassing maken](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_08.png)

2. Klik op **toepassing toevoegen**in de sectie **machtigingen voor andere toepassingen** .

    ![Een toepassing maken](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_09.png)


2. In het dialoogvenster **machtigingen voor andere toepassingen** , moet u de volgende stappen uitvoeren:

    ![Een toepassing maken](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_10.png)

    een. Selecteer **Microsoft Graph**.

    b. Klik op **Voltooien**.


1. Klik op **machtigingen: 0**, en selecteer vervolgens **alle risico gebeurtenis identiteitsgegevens te lezen**.

    ![Een toepassing maken](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_11.png)

1. Klik op **Opslaan** onder aan de pagina.

    ![Een toepassing maken](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_12.png)


## <a name="get-an-access-key"></a>Een toegangstoets ophalen

1. Selecteer op de pagina van de toepassing in de sectie **sleutels** 1 jaar als duur.

    ![Een toepassing maken](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_13.png)

1. Klik op **Opslaan** onder aan de pagina.

    ![Een toepassing maken](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_12.png)

1. de waarde van de zojuist gemaakte sleutel en plak deze in een veilige plaats in de sectie sleutels.

    ![Een toepassing maken](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_14.png)

    > [AZURE.NOTE] Als u deze sleutel kwijtraakt, moet u terug naar deze sectie en maak een nieuwe sleutel. Deze sleutel geheim houden: iedereen die deze heeft toegang tot uw gegevens.


1. Kopieer de **Client-ID**in de sectie **-Eigenschappen** en vervolgens plakken in een veilige plaats. 


## <a name="authenticate-to-microsoft-graph-and-query-the-identity-risk-events-api"></a>Verificatie van Microsoft Graph en de identiteit risico gebeurtenissen API query

Op dit punt moet u hebben:

- De client-ID die u hierboven hebt gekopieerd

- De sleutel die u hierboven hebt gekopieerd

- De naam van het domein van de huurder


Als u wilt verifiëren, verzenden naar een post-aanvraag `https://login.microsoft.com` met de volgende parameters in de hoofdtekst:

- grant_type: "**client_credentials**"

- bron: "**https://graph.microsoft.com**"

- client_id:<your client ID>

- client_secret:<your key>


> [AZURE.NOTE] U moet waarden opgeven voor de **client_id** en de parameter **client_secret** .

Als resultaat oplevert, dit een verificatietoken.  
Maak de API aanroepen, een koptekst met de volgende parameter:

    `Authorization`=”<token_type> <access_token>"


Bij de verificatie, vindt u het type token en access token in het resulterende token.

Deze koptekst als een aanvraag verzenden naar de volgende API-URL:`https://graph.microsoft.com/beta/identityRiskEvents`

Het antwoord, is als dat lukt, een collectie van identiteit risico's en de bijbehorende gegevens in de OData JSON-indeling die kan worden geanalyseerd en verwerkt als geschikt.

Hier is voorbeeldcode voor verificatie en het gebruik van Powershell API aanroept.  
Alleen de client-ID toe te voegen sleutel en pachters domein.

    $ClientID       = "<your client ID here>"        # Should be a ~36 hex character string; insert your info here
    $ClientSecret   = "<your client secret here>"    # Should be a ~44 character string; insert your info here
    $tenantdomain   = "<your tenant domain here>"    # For example, contoso.onmicrosoft.com

    $loginURL       = "https://login.microsoft.com"
    $resource       = "https://graph.microsoft.com"

    $body       = @{grant_type="client_credentials";resource=$resource;client_id=$ClientID;client_secret=$ClientSecret}
    $oauth      = Invoke-RestMethod -Method Post -Uri $loginURL/$tenantdomain/oauth2/token?api-version=1.0 -Body $body

    Write-Output $oauth

    if ($oauth.access_token -ne $null) {
        $headerParams = @{'Authorization'="$($oauth.token_type) $($oauth.access_token)"}

        $url = "https://graph.microsoft.com/beta/identityRiskEvents"
        Write-Output $url

        $myReport = (Invoke-WebRequest -UseBasicParsing -Headers $headerParams -Uri $url)

        foreach ($event in ($myReport.Content | ConvertFrom-Json).value) {
            Write-Output $event
        }

    } else {
        Write-Host "ERROR: No Access Token"
    } 


## <a name="next-steps"></a>Volgende stappen

Gefeliciteerd, u zojuist uw eerste aanroep naar Microsoft Graph gemaakt!  
U kunt nu query identiteit risico's en de gegevens echter naar eigen inzicht te gebruiken.

Meer informatie over Microsoft Graph en het bouwen van toepassingen met behulp van de API van de grafiek, Controleer de [documentatie](https://graph.microsoft.io/docs) en veel meer op de [site van Microsoft Graph](https://graph.microsoft.io/). Zorg ervoor dat u bookmark de pagina [Azure AD identiteitsbeveiliging API](https://graph.microsoft.io/docs/api-reference/beta/resources/identityprotection_root) met alle van de identiteit bescherming-API's beschikbaar zijn in de grafiek. Als we nieuwe manieren om te werken met de bescherming van identiteiten via API toevoegt, ziet u ze op die pagina.


## <a name="additional-resources"></a>Aanvullende bronnen

- [Identiteitsbeveiliging Azure Active Directory](active-directory-identityprotection.md)

- [Soorten risico's door Azure Active Directory identiteitsbeveiliging gedetecteerd](active-directory-identityprotection-risk-events-types.md)

- [Microsoft Graph](https://graph.microsoft.io/)

- [Overzicht van Microsoft Graph](https://graph.microsoft.io/docs)

- [Azure AD Identity Protection Service Root](https://graph.microsoft.io/docs/api-reference/beta/resources/identityprotection_root)
