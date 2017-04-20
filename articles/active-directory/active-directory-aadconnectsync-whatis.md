<properties
    pageTitle="Azure AD verbinden sync: begrijpen en synchronisatie aanpassen | Microsoft Azure"
    description="Legt uit hoe Azure AD verbinden synchroniseren werkt en hoe u kunt aanpassen."
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/29/2016"
    ms.author="markusvi;andkjell"/>


# <a name="azure-ad-connect-sync-understand-and-customize-synchronization"></a>Azure AD verbinden sync: begrijpen en synchronisatie aanpassen
Verbinding Azure Active Directory synchronisatie diensten (synchronisatie van Azure AD Connect) is een hoofdonderdeel van Azure AD verbinden. Dit zorgt voor alle bewerkingen die betrekking hebben op het synchroniseren van identiteitsgegevens tussen uw omgeving op gebouwen en Azure AD. Azure AD verbinden sync is de opvolger van DirSync en Azure AD Sync Forefront Identiteitenbeheer met Azure Active Directory Connector geconfigureerd.

In dit onderwerp wordt de startpagina voor **sync Azure AD verbinden** (ook wel **sync-engine**) en vindt u koppelingen naar andere onderwerpen die zijn gerelateerd aan het. Azure AD verbinden, Zie voor [integratie van identiteiten in ruimten met Azure Active Directory](active-directory-aadconnect.md).

De synchronisatieservice bestaat uit twee onderdelen, het onderdeel op ruimten **sync Azure AD verbinden** en de kant van de service in Azure AD **Azure AD verbinden synchronisatieservice**genoemd. De service is gebruikelijk voor DirSync en Azure AD Sync Azure AD verbinden.

## <a name="azure-ad-connect-sync-topics"></a>Azure AD verbinden sync onderwerpen

Onderwerp | Wat dekt en het lezen
----- | -----
**Azure AD verbinden sync fundamentals** |
[Inzicht in de architectuur](active-directory-aadconnectsync-understanding-architecture.md) | Voor mensen die nieuw zijn in de synchronisatie-engine en wil meer weten over de architectuur en de gebruikte termen.
[Technische concepten](active-directory-aadconnectsync-technical-concepts.md) | Een korte versie van het onderwerp van de architectuur en kort wordt uitgelegd dat de gebruikte termen.
[Topologieën voor Azure AD verbinding](active-directory-aadconnect-topologies.md) | Beschrijving van de verschillende topologieën en scenario's voor die de synchronisatie-engine ondersteunt.
**Aangepaste configuratie** |
[De installatiewizard opnieuw uit te voeren](active-directory-aadconnectsync-installation-wizard.md) | Legt uit wat voor mogelijkheden u beschikbaar zijn wanneer u de wizard Azure AD verbinden installatie opnieuw uitvoeren.
[Wat is declaratieve inrichten?](active-directory-aadconnectsync-understanding-declarative-provisioning.md)| Beschrijving van de configuratiemodel ' declaratieve provisioning ' genoemd.
[Wat zijn expressies van declaratieve inrichten](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md) | Beschrijving van de syntaxis van de expressietaal die in declaratieve inrichten.
[Wat is de standaardconfiguratie?](active-directory-aadconnectsync-understanding-default-configuration.md)| Beschrijving van de out-of-box-regels en de standaardconfiguratie. Ook wordt beschreven hoe de regels samenwerken voor de out-of-box-scenario's te werken.
[Informatie over gebruikers en contactpersonen](active-directory-aadconnectsync-understanding-users-and-contacts.md) | In het vorige onderwerp blijft en wordt beschreven hoe de configuratie voor gebruikers en contactpersonen werkt samen met name in een omgeving met meerdere forests.
[Hoe u een wijziging aanbrengt in de standaardconfiguratie](active-directory-aadconnectsync-change-the-configuration.md) | Helpt u bij het maken van een algemene configuratie wijzigen kenmerk doorloopt.
[Aanbevolen procedures voor het wijzigen van de standaardconfiguratie](active-directory-aadconnectsync-best-practices-changing-default-configuration.md) | Beperkingen voor de ondersteuning en voor het aanbrengen van wijzigingen in de configuratie van de out-of-box.
[Filtering configureren](active-directory-aadconnectsync-configure-filtering.md) | Beschrijving van de verschillende opties voor het beperken van welke objecten worden gesynchroniseerd naar Azure AD en stapsgewijze procedure voor het configureren van deze opties.
**Functies en scenario 's** |
[Niet per ongeluk verwijderen](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md) | Beschrijving van de functie *niet per ongeluk verwijderen* en hoe u deze moet configureren.
[Scheduler](active-directory-aadconnectsync-feature-scheduler.md) | Beschrijving van de ingebouwde scheduler die is, synchroniseren, gegevens importeren en exporteren.
[Wachtwoordsynchronisatie implementeren](active-directory-aadconnectsync-implement-password-synchronization.md) | Beschrijft hoe werkt Wachtwoordsynchronisatie implementeren en hoe werken en op te lossen.
[Apparaat Write-back](active-directory-aadconnect-feature-device-writeback.md) | Wordt de werking van apparaat Write-back in Azure AD verbinding beschreven.
[Directory-extensies](active-directory-aadconnectsync-feature-directory-extensions.md) | Beschrijving van het uitbreiden van het schema Azure AD met uw eigen aangepaste kenmerken.
**Synchronisatieservice** |
[Azure AD verbinden sync servicefuncties](active-directory-aadconnectsyncservice-features.md) | Beschrijving van service aan van de synchronisatie en synchronisatie-instellingen wijzigen in Azure AD.
[Dubbel kenmerk tolerantie](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md) | Beschrijving van het inschakelen en gebruiken van waarden tolerantie voor dubbel kenmerk **userPrincipalName** en **proxyAddresses** .
**Bewerkingen en UI** |
[Synchronisatiebeheer Service](active-directory-aadconnectsync-service-manager-ui.md) | Beschrijving van de Service Synchronisatiebeheer UI, met inbegrip van [bewerkingen](active-directory-aadconnectsync-service-manager-ui-operations.md), [verbindingslijnen](active-directory-aadconnectsync-service-manager-ui-connectors.md), [Metaverse Designer](active-directory-aadconnectsync-service-manager-ui-mvdesigner.md)en tabbladen [Metaverse zoeken](active-directory-aadconnectsync-service-manager-ui-mvsearch.md) .
[Operationele taken en aandachtspunten](active-directory-aadconnectsync-operations.md) | Beschrijving van operationele eisen, zoals noodherstel.
**Procedures...** |
[De Azure AD-account opnieuw instellen](active-directory-aadconnectsync-howto-azureadaccount.md) | Het opnieuw instellen van de referenties van de serviceaccount te maken van synchronisatie Azure AD verbinden met Azure AD.
**Meer informatie en verwijzingen** |
[Poorten](active-directory-aadconnect-ports.md) | Welke poorten u moet openen tussen de synchronisatie-engine en de mappen op gebouwen en Azure AD staan.
[Kenmerken die worden gesynchroniseerd met Azure Active Directory](active-directory-aadconnectsync-attributes-synchronized.md) | Hier worden alle kenmerken die worden gesynchroniseerd tussen op ruimten AD en Azure AD.
[Functies, naslag](active-directory-aadconnectsync-functions-reference.md) | Hier worden alle beschikbare functies in declaratieve inrichten.

## <a name="additional-resources"></a>Aanvullende bronnen

* [Integratie van uw identiteiten op ruimten met Azure Active Directory](active-directory-aadconnect.md)
