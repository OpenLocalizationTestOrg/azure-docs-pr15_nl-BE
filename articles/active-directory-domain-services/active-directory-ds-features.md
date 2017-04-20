<properties
    pageTitle="Azure Active Directory Domain Services: Functies | Microsoft Azure"
    description="Functies van Azure Active Directory Domain Services"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/07/2016"
    ms.author="maheshu"/>

# <a name="azure-ad-domain-services"></a>Azure AD Domain Services

## <a name="features"></a>Functies
De volgende functies zijn beschikbaar in Azure AD beheerd domeinen.

- **Eenvoudige implementatie ervaring:** Azure AD Domain Services voor uw Azure AD huurder met een paar muisklikken kunt u inschakelen. Ongeacht of de huurder Azure AD is een cloud-huurder of gesynchroniseerd met uw directory op de gebouwen, worden de beheerde domein snel ingericht.

- **Ondersteuning voor domeinen aan:** U kunt gemakkelijk domeinen aan computers in het Azure virtuele netwerk dat uw beheerde domein beschikbaar is in. De ervaring van domeinen aan op de Windows-client en Server-besturingssystemen werkt naadloos tegen domeinen bediend door Azure AD Domain Services. U kunt ook geautomatiseerde domeinlidmaatschap gereedschap tegen dergelijke domeinen gebruiken.

- **Domein één exemplaar per map Azure AD:** U kunt één Active Directory-domein voor elke map Azure AD maken.

- **Domeinen maken met aangepaste namen:** U kunt domeinen met aangepaste namen (bijvoorbeeld ' contoso100.com') met behulp van AD-domein Azure Services maken. U kunt beide domeinnamen geverifieerd of niet is geverifieerd. U kunt eventueel ook maken een domein met de ingebouwde domeinachtervoegsel (dat wil zeggen, ' *. onmicrosoft.com') aangeboden door de directory Azure AD.

- **Geïntegreerd met AD Azure:** U hoeft niet te configureren of replicatie Azure AD Domain Services beheren. Gebruikersaccounts, groepslidmaatschappen en referenties van de gebruiker (wachtwoorden) uit de map Azure AD zijn automatisch beschikbaar in Azure AD Domain Services. Nieuwe gebruikers, groepen of wijzigingen in de kenmerken van de huurder Azure AD of de map op de ruimten worden automatisch gesynchroniseerd naar Azure AD Domain Services.

- **NTLM en Kerberos-verificatie:** Met ondersteuning voor NTLM en Kerberos-verificatie, kunt u toepassingen die van geïntegreerde Windows-verificatie gebruikmaken gebruiken.

- **Uw zakelijke referenties/wachtwoorden gebruiken:** Wachtwoorden voor gebruikers in uw huurder Azure AD werken met Azure AD Domain Services. Gebruikers kunnen hun zakelijke referenties voor domeinen aan computers gebruiken, interactief of via Extern bureaublad aanmelden en verifiëren met het domein beheerd.

- **LDAP-binding & LDAP lezen ondersteuning:** U kunt toepassingen die van LDAP-bindingen gebruikmaken voor de verificatie van gebruikers in domeinen die worden bediend door Azure AD Domain Services. Daarnaast kunnen ook toepassingen die gebruikmaken van LDAP-leesbewerkingen op de kenmerken van de gebruiker of computer opvragen uit de map werken tegen Azure AD Domain Services.

- **Veilige LDAP (LDAPS):** U kunt toegang tot de map inschakelen via veilige LDAP (LDAPS). Veilige LDAP-toegang is beschikbaar in het virtuele netwerk standaard. U kunt echter desgewenst ook veilige LDAP-toegang via het internet inschakelen.

- **Groepsbeleid:** U kunt één ingebouwde groepsbeleidsobject elke voor de gebruikers en computers containers afdwingen van de naleving van de vereiste beveiligingsbeleid voor gebruikersaccounts en computers die deel uitmaakt van een domein.

- **Voor het beheren van DNS:** Leden van de groep 'Beheerders AAD DC' kunnen u DNS beheren voor uw beheerde domein met de bekende DNS-beheerprogramma's zoals de DNS-beheer MMC-module.

- **Maken van aangepaste organisatie-eenheden (OU's):** Leden van de groep 'Beheerders AAD DC' kunnen u aangepaste organisatie-eenheden maken in het domein beheerd. Deze gebruikers krijgen volledige beheerdersrechten via aangepaste organisatie-eenheden, zodat u ze kunnen toevoegen of verwijderen-serviceaccounts, computers, groepen enz. binnen deze aangepaste organisatie-eenheden.

- **Beschikbaar in meerdere Azure regio's:** Zie de pagina [Azure services per regio](https://azure.microsoft.com/regions/#services/) te weten de Azure gebieden waarin Azure AD Domain Services beschikbaar is.

- **Hoge beschikbaarheid:** Azure AD Domain Services biedt een hoge beschikbaarheid van uw domein. Deze functie biedt de garantie van een hogere service uptime en veerkracht op storingen. Ingebouwde health monitoring voorstellen automatisch herstel na storingen door nieuwe exemplaren vervangen defecte exemplaren en doorlopende service voor uw domein draaien.

- **Bekende beheerprogramma's gebruiken:** U kunt vertrouwde Windows Server Active Directory-beheerprogramma's zoals het Beheerderscentrum voor Active Directory of Active Directory PowerShell beheerde domeinen beheert.
