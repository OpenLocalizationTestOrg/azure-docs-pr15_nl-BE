<properties
    pageTitle="Azure Single Sign Out SAML Protocol | Microsoft Azure"
    description="Dit artikel beschrijft de Single Sign-Out SAML-Protocol in Azure Active Directory"
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


# <a name="single-sign-out-saml-protocol"></a>Enkele afmeldingstijden SAML-Protocol

Azure Active Directory (AD Azure) ondersteunt de SAML 2.0 web browser één afmeldingstijden profiel. Voor één afmelden om correct, Azure AD moet de metagegevens URL registreren tijdens de registratie van toepassing. Azure AD haalt de URL logout en de handtekeningsleutel van de cloud-service van de metagegevens. Azure AD de handtekeningsleutel gebruikt om te controleren of de handtekening van de inkomende LogoutRequest en de LogoutURL wordt gebruikt om gebruikers te leiden nadat ze zijn afgemeld.

Als de service cloud niet een eindpunt metagegevens, ondersteunt nadat de toepassing is geregistreerd, moet de ontwikkelaar contact opnemen met Microsoft support bieden de logout URL en de bijbehorende sleutel.

Dit diagram toont de workflow van de Azure AD afmeldingstijden proces.

![Single Sign Out Workflow](media/active-directory-single-sign-out-protocol-reference/active-directory-saml-single-sign-out-workflow.png)

## <a name="logoutrequest"></a>LogoutRequest

De cloud-service verzendt een `LogoutRequest` bericht op de Azure AD om aan te geven dat een sessie is beëindigd. Het volgende fragment toont een voorbeeld van een `LogoutRequest` element.

```
<samlp:LogoutRequest xmlns="urn:oasis:names:tc:SAML:2.0:metadata" ID="idaa6ebe6839094fe4abc4ebd5281ec780" Version="2.0" IssueInstant="2013-03-28T07:10:49.6004822Z" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://www.workaad.com</Issuer>
  <NameID xmlns="urn:oasis:names:tc:SAML:2.0:assertion"> Uz2Pqz1X7pxe4XLWxV9KJQ+n59d573SepSAkuYKSde8=</NameID>
</samlp:LogoutRequest>
```

### <a name="logoutrequest"></a>LogoutRequest

De `LogoutRequest` element naar Azure AD verzonden, moet de volgende kenmerken:

- `ID`: Dit geeft de aanvraag afmelden. De waarde van `ID` mag niet beginnen met een getal. De gangbare praktijk is **id** toevoegen aan de tekenreeksweergave van een GUID.

- `Version`: Stel de waarde van dit element op **2.0**. Deze waarde is vereist.

- `IssueInstant`: Dit is een `DateTime` een tekenreeks met een waarde van Universal Time Coordinate (UTC) en [round trip-indeling ("o")](https://msdn.microsoft.com/library/az4se3k1.aspx). Azure AD verwacht een waarde van dit type, maar wordt deze niet toepassen.

- De `Consent`, `Destination`, `NotOnOrAfter` en `Reason` kenmerken worden genegeerd als ze zijn opgenomen een `LogoutRequest` element.

### <a name="issuer"></a>Uitgevende instelling

De `Issuer` -element in een `LogoutRequest` moet exact overeenkomen met een van de **ServicePrincipalNames** in de cloud-service in AD Azure. Dit is normaal gesproken ingesteld op de **URI van App-ID** die is opgegeven tijdens de registratie van toepassing.

### <a name="nameid"></a>NameID

De waarde van de `NameID` element moet exact overeenkomen met de `NameID` van de gebruiker die wordt afgemeld.
## <a name="logoutresponse"></a>LogoutResponse

Azure AD stuurt een `LogoutResponse` in antwoord op een `LogoutRequest` element. Het volgende fragment toont een voorbeeld van een `LogoutResponse`.

```
<samlp:LogoutResponse ID="_f0961a83-d071-4be5-a18c-9ae7b22987a4" Version="2.0" IssueInstant="2013-03-18T08:49:24.405Z" InResponseTo="iddce91f96e56747b5ace6d2e2aa9d4f8c" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://sts.windows.net/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
  <samlp:Status>
    <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Success" />
  </samlp:Status>
</samlp:LogoutResponse>
```

### <a name="logoutresponse"></a>LogoutResponse

Azure AD stelt de `ID`, `Version` en `IssueInstant` worden de waarden in de `LogoutResponse` element. Stelt ook de `InResponseTo` element met de waarde van de `ID` kenmerk van de `LogoutRequest` die opgewekt door het antwoord.

### <a name="issuer"></a>Uitgevende instelling

Azure AD wordt deze waarde ingesteld op `https://login.microsoftonline.com/<TenantIdGUID>/` waarbij <TenantIdGUID> is de ID van de huurder van de pachter Azure AD.

Voor de evaluatie van de waarde van de `Issuer` -element, gebruik de waarde van de **App-ID URI** tijdens de registratie van toepassing opgegeven.

### <a name="status"></a>Status

Azure AD gebruikt de `StatusCode` -element in de `Status` element om aan te geven voor het slagen of mislukken van afmelden. Bij het afmelden niet lukt, de `StatusCode` element kan ook aangepaste foutberichten bevatten.
