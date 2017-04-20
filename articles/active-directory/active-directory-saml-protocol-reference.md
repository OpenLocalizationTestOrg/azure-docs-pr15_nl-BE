<properties
    pageTitle="Azure AD SAML Protocol verwijzing | Microsoft Azure"
    description="Dit artikel bevat een overzicht van de profielen voor eenmalige aanmelding en Single Sign-Out SAML in Azure Active Directory."
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
    ms.date="06/23/2016"
    ms.author="priyamo"/>


# <a name="how-azure-active-directory-uses-the-saml-protocol"></a>Hoe het SAML-protocol wordt gebruikt in Azure Active Directory

Azure Active Directory (AD Azure) gebruikt de SAML 2.0 protocol om toepassingen te bieden hun gebruikers een ervaring voor eenmalige aanmelding. De [Single Sign-On](active-directory-single-sign-on-protocol-reference.md) en [één afmeldingstijden](active-directory-single-sign-out-protocol-reference.md) SAML profielen van Azure advertentie wordt uitgelegd hoe SAML assertions, protocollen en bindingen in de identity provider-service worden gebruikt.

SAML-Protocol is vereist voor de identiteitsprovider (Azure AD) en de serviceprovider (de toepassing) voor de uitwisseling van informatie over zichzelf.

Wanneer een toepassing met Azure Active Directory is geregistreerd, registreert de app ontwikkelaar informatie met betrekking tot de Federatie met Azure Active Directory. Dit omvat de **URI omgeleid** en **Metagegevens URI** van de toepassing.

Azure AD maakt gebruik van de **URI van de metagegevens** van de cloud-service voor het ophalen van de handtekeningsleutel en de logout URI van de cloud-service. Als de toepassing geen ondersteuning biedt voor een URI-metagegevens, de ontwikkelaar moet Neem contact op met Microsoft support bieden de logout URI en handtekeningsleutel.

Azure Active Directory stelt de huurder-specifieke en algemene (huurder onafhankelijk) single sign-on en één afmelden eindpunten. Deze URL's vertegenwoordigen adresseerbare locaties, ze worden niet alleen id--gaat u naar het eindpunt voor het lezen van de metagegevens.

 - Het eindpunt van de huurder-specifieke bevindt zich op `https://login.microsoftonline.com/<TenantDomainName>/FederationMetadata/2007-06/FederationMetadata.xml`.  De <TenantDomainName> aanduiding een geregistreerde domeinnaam of de GUID van een huurder Azure AD TenantID. De metagegevens van de Federatie van de pachter contoso.com is bijvoorbeeld op: https://login.microsoftonline.com/contoso.com/FederationMetadata/2007-06/FederationMetadata.xml

- Het eindpunt van de huurder-onafhankelijke bevindt zich op `https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml`. In dit eindpuntadres **gemeenschappelijke** wordt weergegeven, in plaats van een domeinnaam huurder-ID.

Zie voor meer informatie over de documenten Federation metagegevens die Azure AD publiceert [Federation-metagegevens](active-directory-federation-metadata.md).
