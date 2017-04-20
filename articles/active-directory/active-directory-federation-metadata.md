<properties
    pageTitle="Azure AD Federation metagegevens | Microsoft Azure"
    description="Dit artikel beschrijft de Federatie metagegevensdocument die Azure Active Directory publiceert voor services die tokens Azure Active Directory worden geaccepteerd."
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


# <a name="federation-metadata"></a>Federation-metagegevens

Azure Active Directory (AD Azure) geeft een federation-metagegevensdocument voor services die worden geconfigureerd voor het toestaan van de beveiligingstokens die Azure AD uitgeeft. De federation-metagegevens document-indeling wordt beschreven in de [Web Services Federation (WS-Federation) versie 1.2](http://docs.oasis-open.org/wsfed/federation/v1.2/os/ws-federation-1.2-spec-os.html), die een uitbreiding van de [metagegevens voor de v2.0 OASIS Security Assertion Markup Language (SAML)](http://docs.oasis-open.org/security/saml/v2.0/saml-metadata-2.0-os.pdf).

## <a name="tenant-specific-and-tenant-independent-metadata-endpoints"></a>Huurder-specifieke en huurder onafhankelijke metagegevens eindpunten

Azure AD publiceert specifieke huurder en de huurder onafhankelijke eindpunten.

Huurder-specifieke eindpunten zijn ontworpen voor een bepaalde huurder. De huurder-specifieke federation-metagegevens bevat informatie over de huurder, met inbegrip van specifieke huurder uitgever en eindpunt. Toepassingen die de toegang tot een enkele huurder beperken gebruik huurder-specifieke eindpunten.

Huurder-onafhankelijke eindpunten bevatten gegevens die gemeenschappelijk zijn voor alle Azure AD huurders. Deze informatie geldt voor huurders die worden gehost op *login.microsoftonline.com* en door huurders wordt gedeeld. Huurder-onafhankelijke eindpunten worden aanbevolen voor toepassingen met meerdere huurder, omdat ze niet gekoppeld aan een bepaalde huurder zijn.

## <a name="federation-metadata-endpoints"></a>Eindpunten van Federation-metagegevens

Azure AD publiceert federation metagegevens op `https://login.microsoftonline.com/<TenantDomainName>/FederationMetadata/2007-06/FederationMetadata.xml`.

Voor de **huurder bepaalde eindpunten**, de `TenantDomainName` kan een van de volgende typen zijn:

- Een geregistreerde domeinnaam van een advertentie Azure pachters, zoals: `contoso.onmicrosoft.com`.
- Het onveranderbare pachters-ID van het domein, zoals `72f988bf-86f1-41af-91ab-2d7cd011db45`.

Voor de **eindpunten van de huurder-onafhankelijke**, de `TenantDomainName` is `common`. Dit document geeft een overzicht van de Federation-metagegevens-elementen die gemeenschappelijk zijn voor alle huurders van Azure AD die worden gehost op login.microsoftonline.com.

Een huurder specifiek eindpunt kan bijvoorbeeld `https:// login.microsoftonline.com/contoso.onmicrosoft.com/FederationMetadata/2007-06/FederationMetadata.xml`. Het eindpunt van de huurder-onafhankelijke is [https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml](https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml). U kunt de Federatie metagegevensdocument door deze URL te typen in een browser bekijken.

## <a name="contents-of-federation-metadata"></a>Inhoud van de federation-metagegevens

De volgende sectie bevat informatie die nodig is voor services die de tokens die zijn uitgegeven door AD Azure verbruiken.

### <a name="entity-id"></a>Entiteits-ID

De `EntityDescriptor` -element bevat een `EntityID` kenmerk. De waarde van de `EntityID` kenmerk vertegenwoordigt de uitgevende instelling, dat wil zeggen, de beveiligingstokenservice (STS) die het token uitgegeven. Het is belangrijk dat de uitgevende instelling te valideren wanneer u een token ontvangen.

De volgende metagegevens ziet u specifieke huurder- `EntityDescriptor` -element met een `EntityID` element.

```
<EntityDescriptor
xmlns="urn:oasis:names:tc:SAML:2.0:metadata"
ID="_b827a749-cfcb-46b3-ab8b-9f6d14a1294b"
entityID="https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db45/">
```
U kunt de huurder-ID in het eindpunt van de huurder-onafhankelijke vervangen door de huurder-ID voor het maken van een huurder-specifieke `EntityID` waarde. De resulterende waarde is hetzelfde als de uitgever van het token. De strategie kan een toepassing meerdere huurder voor het valideren van de uitgevende instelling voor een bepaalde huurder.

De volgende metagegevens ziet u huurder-onafhankelijke `EntityID` element. Houd er rekening mee dat de `{tenant}` is een letterlijke waarde, niet een tijdelijke aanduiding.

```
<EntityDescriptor
xmlns="urn:oasis:names:tc:SAML:2.0:metadata"
ID="="_0e5bd9d0-49ef-4258-bc15-21ce143b61bd"
entityID="https://sts.windows.net/{tenant}/">
```

### <a name="token-signing-certificates"></a>Token handtekeningcertificaten

Als een service een token dat wordt uitgegeven door een huurder Azure AD ontvangt, moet de handtekening van de token met een bijbehorende sleutel die is gepubliceerd in de federation-metagegevensdocument worden gevalideerd. De federation-metagegevens bevat het openbare gedeelte van de certificaten die door de huurder wordt gebruikt voor het ondertekenen van tokens. De onbewerkte bytes certificaat weergegeven in de `KeyDescriptor` element. Het token certificaat geldig is voor ondertekenen alleen wanneer de waarde van de `use` kenmerk is `signing`.

Een federation-metagegevensdocument gepubliceerd door AD Azure hebben meerdere handtekeningsleutels zoals wanneer Azure AD wordt voorbereid voor het bijwerken van het handtekeningcertificaat. Wanneer een document federation-metagegevens meer dan één certificaat bevat, moet een service die de tokens wordt gevalideerd ondersteuning voor alle certificaten in het document.

De volgende metagegevens bevat een voorbeeld van een `KeyDescriptor` -element met een handtekeningsleutel.

```
<KeyDescriptor use="signing">
<KeyInfo xmlns="http://www.w3.org/2000/09/xmldsig#">
<X509Data>
<X509Certificate>
MIIDPjCCAiqgAwIBAgIQVWmXY/+9RqFA/OG9kFulHDAJBgUrDgMCHQUAMC0xKzApBgNVBAMTImFjY291bnRzLmFjY2Vzc2NvbnRyb2wud2luZG93cy5uZXQwHhcNMTIwNjA3MDcwMDAwWhcNMTQwNjA3MDcwMDAwWjAtMSswKQYDVQQDEyJhY2NvdW50cy5hY2Nlc3Njb250cm9sLndpbmRvd3MubmV0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEArCz8Sn3GGXmikH2MdTeGY1D711EORX/lVXpr+ecGgqfUWF8MPB07XkYuJ54DAuYT318+2XrzMjOtqkT94VkXmxv6dFGhG8YZ8vNMPd4tdj9c0lpvWQdqXtL1TlFRpD/P6UMEigfN0c9oWDg9U7Ilymgei0UXtf1gtcQbc5sSQU0S4vr9YJp2gLFIGK11Iqg4XSGdcI0QWLLkkC6cBukhVnd6BCYbLjTYy3fNs4DzNdemJlxGl8sLexFytBF6YApvSdus3nFXaMCtBGx16HzkK9ne3lobAwL2o79bP4imEGqg+ibvyNmbrwFGnQrBc1jTF9LyQX9q+louxVfHs6ZiVwIDAQABo2IwYDBeBgNVHQEEVzBVgBCxDDsLd8xkfOLKm4Q/SzjtoS8wLTErMCkGA1UEAxMiYWNjb3VudHMuYWNjZXNzY29udHJvbC53aW5kb3dzLm5ldIIQVWmXY/+9RqFA/OG9kFulHDAJBgUrDgMCHQUAA4IBAQAkJtxxm/ErgySlNk69+1odTMP8Oy6L0H17z7XGG3w4TqvTUSWaxD4hSFJ0e7mHLQLQD7oV/erACXwSZn2pMoZ89MBDjOMQA+e6QzGB7jmSzPTNmQgMLA8fWCfqPrz6zgH+1F1gNp8hJY57kfeVPBiyjuBmlTEBsBlzolY9dd/55qqfQk6cgSeCbHCy/RU/iep0+UsRMlSgPNNmqhj5gmN2AFVCN96zF694LwuPae5CeR2ZcVknexOWHYjFM0MgUSw0ubnGl0h9AJgGyhvNGcjQqu9vd1xkupFgaN+f7P3p3EVN5csBg5H94jEcQZT7EKeTiZ6bTrpDAnrr8tDCy8ng
</X509Certificate>
</X509Data>
</KeyInfo>
</KeyDescriptor>
  ```

De `KeyDescriptor` -element wordt weergegeven op twee plaatsen in de federation-metagegevensdocument; in de WS-Federation-specifieke en de SAML-specifieke sectie. De certificaten die zijn uitgegeven in beide secties zijn hetzelfde.

In de sectie WS-Federation-specifieke WS-Federation metagegevens lezer leest de certificaten van een `RoleDescriptor` -element met de `SecurityTokenServiceType` type.

De volgende metagegevens bevat een voorbeeld van een `RoleDescriptor` element.

```
<RoleDescriptor xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:fed="http://docs.oasis-open.org/wsfed/federation/200706" xsi:type="fed:SecurityTokenServiceType"protocolSupportEnumeration="http://docs.oasis-open.org/wsfed/federation/200706">
```

In de sectie SAML-specifieke WS-Federation metagegevens lezer leest de certificaten van een `IDPSSODescriptor` element.

De volgende metagegevens bevat een voorbeeld van een `IDPSSODescriptor` element.

```
<IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
```
Er zijn geen verschillen in de opmaak van specifieke huurder en de huurder onafhankelijke certificaten.

### <a name="ws-federation-endpoint-url"></a>WS-Federation eindpunt-URL

De federation-metagegevens bevat de URL die wordt gebruikt voor-in- en afmelden in het protocol WS-Federation enkel Azure AD. Dit eindpunt wordt weergegeven in de `PassiveRequestorEndpoint` element.

De volgende metagegevens bevat een voorbeeld van een `PassiveRequestorEndpoint` -element voor een huurder specifiek eindpunt.

```
<fed:PassiveRequestorEndpoint>
<EndpointReference xmlns="http://www.w3.org/2005/08/addressing">
<Address>
https://login.microsoftonline.com/72f988bf-86f1-41af-91ab-2d7cd011db45/wsfed
</Address>
</EndpointReference>
</fed:PassiveRequestorEndpoint>
```
Voor het eindpunt van de huurder-onafhankelijke, de WS-Federation URL wordt weergegeven in het eindpunt van WS-Federation, zoals in het volgende voorbeeld.

```
<fed:PassiveRequestorEndpoint>
<EndpointReference xmlns="http://www.w3.org/2005/08/addressing">
<Address>
https://login.microsoftonline.com/common/wsfed
</Address>
</EndpointReference>
</fed:PassiveRequestorEndpoint>
```

### <a name="saml-protocol-endpoint-url"></a>SAML protocol eindpunt-URL

De federation-metagegevens bevat de URL die Azure AD voor-in- en afmeldingstijden van SAML 2.0 protocol enkel worden gebruikt. Deze eindpunten worden weergegeven de `IDPSSODescriptor` element.

De aanmeldings- en afmeldingstijden URL's worden weergegeven de `SingleSignOnService` en `SingleLogoutService` elementen.

De volgende metagegevens bevat een voorbeeld van een `PassiveResistorEndpoint` voor een huurder specifiek eindpunt.

```
<IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
…
    <SingleLogoutService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/contoso.onmicrosoft.com/saml2" />
    <SingleSignOnService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/contoso.onmicrosoft.com /saml2" />
  </IDPSSODescriptor>
```

De eindpunten voor de gemeenschappelijke SAML 2.0 protocoleindpunten worden ook gepubliceerd in de huurder onafhankelijke federation-metagegevens, zoals in het volgende voorbeeld.

```
<IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
…
    <SingleLogoutService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/common/saml2" />
    <SingleSignOnService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/common/saml2" />
  </IDPSSODescriptor>
```
