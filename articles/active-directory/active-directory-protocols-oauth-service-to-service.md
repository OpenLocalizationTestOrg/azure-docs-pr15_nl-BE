<properties
    pageTitle="Azure AD Services Auth met OAuth2.0 | Microsoft Azure"
    description="Dit artikel wordt beschreven hoe u HTTP-berichten voor het implementeren van verificatie van services met behulp van de stroom OAuth2.0 client referenties subsidie."
    services="active-directory"
    documentationCenter=".net"
    authors="priyamohanram"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="priyamo"/>

# <a name="service-to-service-calls-using-client-credentials"></a>Telefonische contacten met de clientreferenties-service

OAuth 2.0-Client referenties subsidie stromen kunnen een webservice (een *vertrouwelijke client*) naar zijn eigen referenties worden gebruikt voor verificatie bij het aanroepen van een andere webservice in plaats van een gebruiker te imiteren. In dit scenario wordt de client is meestal een middle-tier-webservice, een daemon-service of website.

## <a name="client-credentials-grant-flow-diagram"></a>Clientreferenties verlenen gegevensstroomdiagram

In het volgende diagram wordt uitgelegd hoe de clientreferenties stroom works in Azure Active Directory (AD Azure) verlenen.

![OAuth2.0 Client referenties subsidie stroom](media/active-directory-protocols-oauth-service-to-service/active-directory-protocols-oauth-client-credentials-grant-flow.jpg)

1. De clienttoepassing wordt geverifieerd bij het eindpunt van de token uitgifte Azure AD en vraagt een toegangstoken.
2. Het eindpunt van de token uitgifte Azure AD problemen het toegangstoken.
3. Het toegangstoken wordt gebruikt voor het verifiëren van de beveiligde bron.
4. Gegevens van de beveiligde bron wordt teruggestuurd naar de webtoepassing.

## <a name="register-the-services-in-azure-ad"></a>De Services registreren in Azure AD

De aanvragende en de ontvangende service registreren in Azure Active Directory (AD Azure). Zie voor gedetailleerde instructies, [toevoegen, bijwerken, en een App verwijderen](active-directory-integrating-applications.md#BKMK_Native)

## <a name="request-an-access-token"></a>Een toegangstoken aanvragen

Voor het aanvragen van een toegangstoken, gebruik van een HTTP POST naar de huurder-specifieke Azure AD eindpunt.

```
https://login.microsoftonline.com/<tenant id>/oauth2/token
```

## <a name="service-to-service-access-token-request"></a>De tokenaanvraag service-service-toegang

Een aanvraag voor een service-service-access token bevat de volgende parameters.

| Parameter | | Beschrijving |
|-----------|------|------------|
| response_type | Vereist | Hiermee geeft u het type gevraagde reactie. De waarde moet in een stroom Client referenties subsidie **client_credentials**.|
| client_id | Vereist | Hiermee geeft u de Azure AD client-id van de webservice aanroepen. Client-ID van de aanroepende toepassing, zoeken in Portal Management Azure, **Active Directory**, klikt u op de map, klik op de toepassing en klik vervolgens op **configureren**.|
| client_secret | Vereist |  Geef een sleutel voor de webservice aanroepen in Azure AD geregistreerd. Voor het maken van een sleutel in Azure Management Portal, **Active Directory**, klikt u op de map, klik op de toepassing en klik vervolgens op **configureren**. |
| resource | Vereist | Voer de URI App-ID van de webservice ontvangen. Met de App ID-URI vinden in Azure Management Portal, **Active Directory**, klikt u op de map, klik op de toepassing en klik vervolgens op **configureren**. |

## <a name="example"></a>Voorbeeld

De volgende HTTP POST-aanvragen een toegangstoken voor de webservice https://service.contoso.com/. De `client_id` geeft de webservice die het toegangstoken vraagt.

```
POST contoso.com/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=client_credentials&client_id=625bc9f6-3bf6-4b6d-94ba-e97cf07a22de&client_secret=qkDwDJlDfig2IpeuUZYKH1Wb8q1V0ju6sILxQQqhJ+s=&resource=https%3A%2F%2Fservice.contoso.com%2F
```

## <a name="service-to-service-access-token-response"></a>Service-Service-Access Token antwoord

Een geslaagde antwoord bevat een antwoord JSON OAuth 2.0 met de volgende parameters.

| Parameter   | Beschrijving |
|-------------|-------------|
|access_token |De aangevraagde toegangstoken. De webservice aanroepen kunt deze token voor het verifiëren van de ontvangende webservice. |
|access_type  | Geeft de waarde van het type token. Het enige type dat AD Azure ondersteunt is **aan toonder**. Zie voor meer informatie over tokens aan toonder, de [OAuth 2.0 vergunning Framework: aan toonder Token gebruik (RFC 6750)](http://www.rfc-editor.org/rfc/rfc6750.txt).
|expires_in   | Hoe lang het toegangstoken geldig is (in seconden).|
|expires_on   |Het tijdstip waarop het toegangstoken verloopt. De datum wordt weergegeven als het aantal seconden vanaf 1970-01-01T0:0:0Z UTC tot de verlooptijd. Deze waarde wordt gebruikt om te bepalen van de levensduur van tokens in de cache. |
|resource     | De App-ID de URI van de ontvangende webservice. |

## <a name="example"></a>Voorbeeld

In het volgende voorbeeld ziet u een geslaagde antwoord op een aanvraag voor een toegangstoken met een webservice.

```
{
"access_token":"eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJodHRwczovL3NlcnZpY2UuY29udG9zby5jb20vIiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlLyIsImlhdCI6MTM4ODQ0ODI2NywibmJmIjoxMzg4NDQ4MjY3LCJleHAiOjEzODg0NTIxNjcsInZlciI6IjEuMCIsInRpZCI6IjdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZSIsIm9pZCI6ImE5OTE5MTYyLTkyMTctNDlkYS1hZTIyLWYxMTM3YzI1Y2RlYSIsInN1YiI6ImE5OTE5MTYyLTkyMTctNDlkYS1hZTIyLWYxMTM3YzI1Y2RlYSIsImlkcCI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZS8iLCJhcHBpZCI6ImQxN2QxNWJjLWM1NzYtNDFlNS05MjdmLWRiNWYzMGRkNThmMSIsImFwcGlkYWNyIjoiMSJ9.aqtfJ7G37CpKV901Vm9sGiQhde0WMg6luYJR4wuNR2ffaQsVPPpKirM5rbc6o5CmW1OtmaAIdwDcL6i9ZT9ooIIicSRrjCYMYWHX08ip-tj-uWUihGztI02xKdWiycItpWiHxapQm0a8Ti1CWRjJghORC1B1-fah_yWx6Cjuf4QE8xJcu-ZHX0pVZNPX22PHYV5Km-vPTq2HtIqdboKyZy3Y4y3geOrRIFElZYoqjqSv5q9Jgtj5ERsNQIjefpyxW3EwPtFqMcDm4ebiAEpoEWRN4QYOMxnC9OUBeG9oLA0lTfmhgHLAtvJogJcYFzwngTsVo6HznsvPWy7UP3MINA",
"token_type":"Bearer",
"expires_in":"3599",
"expires_on":"1388452167",
"resource":"https://service.contoso.com/"
}
```

## <a name="see-also"></a>Zie ook

* [OAuth 2.0 in Azure AD](active-directory-protocols-oauth-code.md)
