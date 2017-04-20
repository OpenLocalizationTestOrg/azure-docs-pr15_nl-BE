<properties
    pageTitle="Azure eenmalige aanmelding SAML Protocol | Microsoft Azure"
    description="Dit artikel wordt beschreven dat het protocol van één teken op SAML in Azure Active Directory"
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

# <a name="single-sign-on-saml-protocol"></a>Single Sign-On SAML protocol

Dit artikel behandelt de SAML 2.0 verificatie-aanvragen en antwoorden die Azure Active Directory (AD Azure) voor eenmalige aanmelding ondersteunt.

Het onderstaande diagram van protocol beschrijft de volgorde voor eenmalige aanmelding. De cloud-service (de serviceprovider) gebruikt een HTTP-omleiding binding te geven een `AuthnRequest` (verificatieaanvraag) element naar Azure AD (identiteitsprovider). Vervolgens Azure AD maakt gebruik van een HTTP post binding voor het boeken van een `Response` -element in de cloud-service.

![Eenmalige aanmelding Workflow](media/active-directory-single-sign-on-protocol-reference/active-directory-saml-single-sign-on-workflow.png)

## <a name="authnrequest"></a>AuthnRequest

Als een gebruiker die verificatie aanvraagt, cloud services verzenden een `AuthnRequest` element naar Azure AD. Een voorbeeld van SAML 2.0 `AuthnRequest` kan er als volgt:

```
<samlp:AuthnRequest
xmlns="urn:oasis:names:tc:SAML:2.0:metadata"
ID="id6c1c178c166d486687be4aaf5e482730"
Version="2.0" IssueInstant="2013-03-18T03:28:54.1839884Z"
xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
<Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://www.contoso.com</Issuer>
</samlp:AuthnRequest>
```


| Parameter | | Beschrijving |
| ----------------------- | ------------------------------- | --------------- |
| ID | Vereist | Azure AD maakt gebruik van dit kenmerk voor het vullen van de `InResponseTo` kenmerk van het geretourneerde antwoord. ID moet niet beginnen met een getal, een gemeenschappelijke strategie wordt Voeg een tekenreeks zoals 'id' als de tekenreeksweergave van een GUID. Bijvoorbeeld: `id6c1c178c166d486687be4aaf5e482730` -id is een geldig. |
| Versie | Vereist | Dit moet **2.0**.|
| IssueInstant | Vereist | Dit is een tekenreeks DateTime met een UTC-waarde en de [round trip-indeling ("o")](https://msdn.microsoft.com/library/az4se3k1.aspx). Azure AD verwacht een DateTime-waarde van dit type, maar niet beoordelen of de waarde. |
| AssertionConsumerServiceUrl | optioneel | Indien beschikbaar, deze moet overeenkomen met de `RedirectUri` van de service cloud in Azure AD. |
| ForceAuthn | optioneel | Als, moet dit de waarde false. Een fout wordt veroorzaakt door een andere waarde.|
| IsPassive | optioneel | Als, moet dit de waarde false. Een fout wordt veroorzaakt door een andere waarde. |  

Alle andere `AuthnRequest` kenmerken, zoals toestemming, bestemming, AssertionConsumerServiceIndex, AttributeConsumerServiceIndex en ProviderName worden **genegeerd**.

Azure AD negeert ook de `Conditions` -element in `AuthnRequest`.

### <a name="issuer"></a>Uitgevende instelling

De `Issuer` -element in een `AuthnRequest` moet exact overeenkomen met een van de **ServicePrincipalNames** in de cloud-service in AD Azure. Dit is normaal gesproken ingesteld op de **URI van App-ID** die is opgegeven tijdens de registratie van toepassing.

Een monster SAML fragment met de `Issuer` element zien er zo uit:

```
<Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://www.contoso.com</Issuer>
```

### <a name="nameidpolicy"></a>NameIDPolicy

Dit element, vraagt u de indeling van een bepaalde naam-ID in het antwoord en is optioneel in `AuthnRequest` elementen naar Azure AD verzonden.

Een voorbeeld van een `NameIdPolicy` element zien er zo uit:

```
<NameIDPolicy Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent"/>
```

Als `NameIDPolicy` is, kunt u de optionele opnemen `Format` kenmerk. De `Format` kenmerk kan hebben slechts één van de volgende waarden; een andere waarde resulteert in een fout.

-  `urn:oasis:names:tc:SAML:2.0:nameid-format:persistent`: De claim NameID problemen azure Active Directory als een paarsgewijze-id.
- `urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress`: Azure Active Directory problemen de claim NameID in e-mailadresindeling.
- `urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified`: Deze waarde staat Azure Active Directory om de indeling van de claim te selecteren. De NameID problemen Azure Active Directory als een paarsgewijze-id.

Neem niet de `SPNameQualifer` kenmerk. Azure AD negeert de `AllowCreate` kenmerk.

### <a name="requestauthncontext"></a>RequestAuthnContext

De `RequestedAuthnContext` element geeft de gewenste verificatiemethoden. Het is optioneel in `AuthnRequest` elementen naar Azure AD verzonden. Azure AD ondersteunt slechts één `AuthnContextClassRef` waarde: `urn:oasis:names:tc:SAML:2.0:ac:classes:Password`.

### <a name="scoping"></a>Scopes

De `Scoping` -element een lijst met id-providers bevat, is optioneel in `AuthnRequest` elementen naar Azure AD verzonden.

Als gegeven, neem niet het `ProxyCount` kenmerk, `IDPListOption` of `RequesterID` -element, als deze niet worden ondersteund.

### <a name="signature"></a>Handtekening

Neem niet een `Signature` -element in `AuthnRequest` elementen, zoals AD Azure biedt geen ondersteuning voor verificatieaanvragen ondertekend.

### <a name="subject"></a>Onderwerp

Azure AD negeert de `Subject` element van `AuthnRequest` elementen.

## <a name="response"></a>Reactie

Wanneer u een aangevraagde aanmelding is voltooid, geboekt Azure AD een antwoord naar de cloud-service. Een monster reactie op een geslaagde poging tot aanmelding ziet er zo uit:

```
<samlp:Response ID="_a4958bfd-e107-4e67-b06d-0d85ade2e76a" Version="2.0" IssueInstant="2013-03-18T07:38:15.144Z" Destination="https://contoso.com/identity/inboundsso.aspx" InResponseTo="id758d0ef385634593a77bdf7e632984b6" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion"> https://login.microsoftonline.com/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
  <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
    ...
  </ds:Signature>
  <samlp:Status>
    <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Success" />
  </samlp:Status>
  <Assertion ID="_bf9c623d-cc20-407a-9a59-c2d0aee84d12" IssueInstant="2013-03-18T07:38:15.144Z" Version="2.0" xmlns="urn:oasis:names:tc:SAML:2.0:assertion">
    <Issuer>https://login.microsoftonline.com/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
    <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
      ...
    </ds:Signature>
    <Subject>
      <NameID>Uz2Pqz1X7pxe4XLWxV9KJQ+n59d573SepSAkuYKSde8=</NameID>
      <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer">
        <SubjectConfirmationData InResponseTo="id758d0ef385634593a77bdf7e632984b6" NotOnOrAfter="2013-03-18T07:43:15.144Z" Recipient="https://contoso.com/identity/inboundsso.aspx" />
      </SubjectConfirmation>
    </Subject>
    <Conditions NotBefore="2013-03-18T07:38:15.128Z" NotOnOrAfter="2013-03-18T08:48:15.128Z">
      <AudienceRestriction>
        <Audience>https://www.contoso.com</Audience>
      </AudienceRestriction>
    </Conditions>
    <AttributeStatement>
      <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name">
        <AttributeValue>testuser@contoso.com</AttributeValue>
      </Attribute>
      <Attribute Name="http://schemas.microsoft.com/identity/claims/objectidentifier">
        <AttributeValue>3F2504E0-4F89-11D3-9A0C-0305E82C3301</AttributeValue>
      </Attribute>
      ...
    </AttributeStatement>
    <AuthnStatement AuthnInstant="2013-03-18T07:33:56.000Z" SessionIndex="_bf9c623d-cc20-407a-9a59-c2d0aee84d12">
      <AuthnContext>
        <AuthnContextClassRef> urn:oasis:names:tc:SAML:2.0:ac:classes:Password</AuthnContextClassRef>
      </AuthnContext>
    </AuthnStatement>
  </Assertion>
</samlp:Response>
```

### <a name="response"></a>Reactie

De `Response` element bevat het resultaat van de aanvraag van de vergunning. Azure AD stelt de `ID`, `Version` en `IssueInstant` worden de waarden in de `Response` element. Bovendien wordt de volgende kenmerken ingesteld:

- `Destination`: Wanneer de aanmelding is voltooid, wordt deze ingesteld op de `RedirectUri` van de dienstverlener (cloud-service).
- `InResponseTo`: Dit is ingesteld op de `ID` kenmerk van de `AuthnRequest` -element dat de reactie wordt gestart.

### <a name="issuer"></a>Uitgevende instelling

Azure AD stelt de `Issuer` element `https://login.microsoftonline.com/<TenantIDGUID>/` waarbij <TenantIDGUID> is de ID van de huurder van de pachter Azure AD.

Bijvoorbeeld een antwoord monster met een element van de uitgevende instelling kan ziet er zo uit:

```
<Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion"> https://login.microsoftonline.com/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
```

### <a name="status"></a>Status

De `Status` element het slagen of mislukken van de aanmelding worden weergegeven. Het bevat de `StatusCode` -element bevat een code of een reeks geneste codes die de status van de aanvraag. Bevat ook de `StatusMessage` -element bevat berichten voor aangepaste fouten die worden gegenereerd tijdens de aanmelding.

<!-- TODO: Add a authentication protocol error reference -->

Het volgende is een SAML-reactie op aanmelding gelukt.

```
<samlp:Response ID="_f0961a83-d071-4be5-a18c-9ae7b22987a4" Version="2.0" IssueInstant="2013-03-18T08:49:24.405Z" InResponseTo="iddce91f96e56747b5ace6d2e2aa9d4f8c" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://sts.windows.net/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
  <samlp:Status>
    <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Requester">
      <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:RequestUnsupported" />
    </samlp:StatusCode>
    <samlp:StatusMessage>AADSTS75006: An error occurred while processing a SAML2 Authentication request. AADSTS90011: The SAML authentication request property 'NameIdentifierPolicy/SPNameQualifier' is not supported.
Trace ID: 66febed4-e737-49ff-ac23-464ba090d57c
Timestamp: 2013-03-18 08:49:24Z</samlp:StatusMessage>
  </samlp:Status>
```

### <a name="assertion"></a>Bevestiging

In aanvulling op de `ID`, `IssueInstant` en `Version`, Azure AD stelt de volgende elementen in de `Assertion` -element van het antwoord.

#### <a name="issuer"></a>Uitgevende instelling

Dit is ingesteld op `https://sts.windows.net/<TenantIDGUID>/`waarbij <TenantIDGUID> is de ID van de huurder van de pachter Azure AD.

```
<Issuer>https://login.microsoftonline.com/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
```

#### <a name="signature"></a>Handtekening

Azure AD ondertekent de verklaring in reactie op een geslaagde aanmelding. De `Signature` -element bevat een digitale handtekening die de cloud-service kan worden gebruikt voor het verifiëren van de bron om te controleren of de integriteit van de bevestiging.

Voor het genereren van de digitale handtekening, Azure AD maakt gebruik van de handtekeningsleutel in de `IDPSSODescriptor` -element van het metagegevensdocument.

```
<ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
      digital_signature_here
    </ds:Signature>
```

#### <a name="subject"></a>Onderwerp

Hiermee geeft u de hoofdsom die het voorwerp uitmaakt van de instructies in de bevestiging. Het bevat een `NameID` -element de geverifieerde gebruiker vertegenwoordigt. De `NameID` waarde is een gerichte-id die is gericht op de serviceprovider die de doelgroep van het token is alleen. Het permanente is - deze kan worden ingetrokken, maar nooit opnieuw is toegewezen. Ook is ondoorzichtig, omdat het niet iets over de gebruiker onthullen en kan niet worden gebruikt als id voor het kenmerk query's.

De `Method` kenmerk van de `SubjectConfirmation` element wordt altijd ingesteld op `urn:oasis:names:tc:SAML:2.0:cm:bearer`.

```
<Subject>
      <NameID>Uz2Pqz1X7pxe4XLWxV9KJQ+n59d573SepSAkuYKSde8=</NameID>
      <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer">
        <SubjectConfirmationData InResponseTo="id758d0ef385634593a77bdf7e632984b6" NotOnOrAfter="2013-03-18T07:43:15.144Z" Recipient="https://contoso.com/identity/inboundsso.aspx" />
      </SubjectConfirmation>
</Subject>
```

#### <a name="conditions"></a>Voorwaarden

Dit element geeft de voorwaarden die het acceptabele gebruik van SAML assertions definiëren.

```
<Conditions NotBefore="2013-03-18T07:38:15.128Z" NotOnOrAfter="2013-03-18T08:48:15.128Z">
      <AudienceRestriction>
        <Audience>https://www.contoso.com</Audience>
      </AudienceRestriction>
</Conditions>
```

De `NotBefore` en `NotOnOrAfter` kenmerken Geef het interval op waarmee de verklaring geldig is.

- De waarde van de `NotBefore` kenmerk gelijk is aan of iets (minder dan een seconde) hoger is dan de waarde van `IssueInstant` kenmerk van de `Assertion` element. Azure AD wordt geen rekening gehouden met eventuele tijdsverschil tussen zichzelf en de cloud-service (serviceprovider) en wordt de buffer niet toegevoegd aan deze tijd.
- De waarde van de `NotOnOrAfter` kenmerk is 70 minuten later is dan de waarde van de `NotBefore` kenmerk.

#### <a name="audience"></a>Doelgroep

Dit document bevat een URI die een bepaalde doelgroep identificeert. Azure AD stelt de waarde van dit element met de waarde van `Issuer` -element van de `AuthnRequest` die de aanmelding gestart. Voor de evaluatie van de `Audience` de waarde, gebruikt u de waarde van de `App ID URI` die is opgegeven tijdens de registratie van toepassing.

```
<AudienceRestriction>
        <Audience>https://www.contoso.com</Audience>
</AudienceRestriction>
```

Net als de `Issuer` -waarde, de `Audience` waarde moet exact overeenkomen met een van de SPN-namen die de service cloud in Azure AD vertegenwoordigt. Echter, als de waarde van de `Issuer` element is niet een URI-waarde de `Audience` waarde in het antwoord is de `Issuer` waarde voorafgegaan door `spn:`.

#### <a name="attributestatement"></a>AttributeStatement

Deze bevat beweringen over het onderwerp of de gebruiker. Het volgende fragment bevat een voorbeeld van een `AttributeStatement` element. Het weglatingsteken geeft aan dat het element meerdere kenmerken en kenmerkwaarden bevatten kan.

```
<AttributeStatement>
      <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name">
        <AttributeValue>testuser@contoso.com</AttributeValue>
      </Attribute>
      <Attribute Name="http://schemas.microsoft.com/identity/claims/objectidentifier">
        <AttributeValue>3F2504E0-4F89-11D3-9A0C-0305E82C3301</AttributeValue>
      </Attribute>
      ...
</AttributeStatement>
```     

- **Naam van Claim** : de waarde van de `Name` kenmerk (`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`) is de principal-naam van de geverifieerde gebruiker, zoals `testuser@managedtenant.com`.
- **ObjectIdentifier Claim** : de waarde van de `ObjectIdentifier` kenmerk (`http://schemas.microsoft.com/identity/claims/objectidentifier`) is de `ObjectId` van de directory-object dat staat voor de geverifieerde gebruiker in Azure AD. `ObjectId`is een onveranderbare uniek en veilige id van de geverifieerde gebruiker opnieuw te gebruiken.

#### <a name="authnstatement"></a>AuthnStatement

Dit element wordt gesteld dat het onderwerp van de bevestiging is geverifieerd door een bepaalde manier op een bepaald moment.

- De `AuthnInstant` kenmerk bevat de tijd waarop de gebruiker geverifieerd met Azure Active Directory.
- De `AuthnContext` element geeft de context van de verificatie die wordt gebruikt om de gebruiker te verifiëren.

```
<AuthnStatement AuthnInstant="2013-03-18T07:33:56.000Z" SessionIndex="_bf9c623d-cc20-407a-9a59-c2d0aee84d12">
      <AuthnContext>
        <AuthnContextClassRef> urn:oasis:names:tc:SAML:2.0:ac:classes:Password</AuthnContextClassRef>
      </AuthnContext>
</AuthnStatement>
```
