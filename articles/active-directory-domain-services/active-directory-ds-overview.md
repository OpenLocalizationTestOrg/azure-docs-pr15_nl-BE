<properties
    pageTitle="Overzicht van Azure Active Directory Domain Services | Microsoft Azure"
    description="Azure Active Directory Domain Services-overzicht"
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

## <a name="overview"></a>Overzicht
Azure infrastructuurservices kunt u een breed scala van oplossingen voor computergebruik op een flexibele manier implementeren. Met Azure virtuele Machines kunt u bijna onmiddellijk distribueren en u betaalt slechts per minuut. Ondersteuning voor Windows, Linux, SQL Server, Oracle, IBM, SAP en BizTalk gebruikt, kunt u de werkbelasting, elke taal op vrijwel elk besturingssysteem implementeren. Deze voordelen kunt u oudere toepassingen geïmplementeerd op ruimten migreren naar Azure op te slaan op de operationele kosten.

Een belangrijk aspect van de migratie op ruimten toepassingen Azure verwerkt de identiteiten van deze toepassingen. Directory-toepassingen kunnen gebruikmaken van LDAP voor lees- of schrijftoegang tot de map zakelijke of die gebruikmaken van geïntegreerde Windows-verificatie (Kerberos of NTLM-verificatie) voor het verifiëren van gebruikers. Line of business (LOB)-toepassingen die worden uitgevoerd op Windows Server meestal worden ingezet op domein gekoppeld machines, zodat ze kunnen worden beheerd veilig met behulp van Groepsbeleid. 'Lift en-ploegen' op ruimten toepassingen naar de cloud, deze afhankelijkheden op de infrastructuur bedrijfsidentiteit opgelost moeten worden.

Beheerders schakelen vaak op een van de volgende oplossingen om te voldoen aan de behoeften van de identiteit van hun toepassingen in Azure geïmplementeerd:

- Een VPN-verbinding van site naar site tussen werklasten in Azure infrastructuurservices en de adreslijst het bedrijf in ruimten met implementeren.
- AD-domein-forest bedrijfsinfrastructuur uitbreiden door het instellen van de replica-domeincontrollers met behulp van Azure virtual machines.
- Implementeer een zelfstandige domein in Azure met domeincontrollers die zijn geïmplementeerd als Azure virtuele machines.

Alle deze benaderingen ten koste gaan van de hoge kosten en administratieve overhead. Beheerders moeten implementeren met behulp van virtuele machines in Azure domeincontrollers. Bovendien moeten ze beheren, beveiligen, patch, bewaken, back-up maken en oplossen van problemen met deze virtuele machines. De afhankelijkheid van VPN-verbindingen naar de map op locatie zorgt ervoor dat de werklast in Azure kwetsbaar voor tijdelijke netwerk storingen of storingen worden geïmplementeerd. Deze netwerkstoringen op zijn beurt leiden tot lagere uptime en verminderde betrouwbaarheid voor deze toepassingen.

We ontworpen Azure AD Domain Services bieden een beter alternatief.


## <a name="introducing-azure-ad-domain-services"></a>Introductie van Azure AD Domain Services
Azure AD Domain Services biedt beheerde domeinservices zoals domeinlidmaatschap, group policy, LDAP, Kerberos/NTLM-verificatie die volledig compatibel zijn met Windows Server Active Directory. U kunt deze domeinservices zonder de behoefte te implementeren, beheren en domeincontrollers in de cloud patch verbruiken. Azure AD Domain Services integreert met uw bestaande huurder Azure AD, waardoor het mogelijk dat gebruikers zich aanmelden met hun zakelijke referenties. Bovendien kunt u bestaande groepen en gebruikersaccounts toegang te krijgen tot bronnen, zodat een vloeiender 'lift-en-ploeg' van on-premises resources infrastructuurservices Azure.

Azure AD Domain Services-functionaliteit werkt naadloos ongeacht of de huurder Azure AD alleen cloud of gesynchroniseerd met uw Active Directory op gebouwen.

### <a name="azure-ad-domain-services-for-cloud-only-organizations"></a>Azure AD Domain Services alleen cloud-organisaties
Een wolk alleen Azure AD huurder (vaak aangeduid als 'beheerde huurders') beschikt niet over een identiteit op ruimten footprint. Accounts van gebruikers, hun wachtwoorden en groepslidmaatschappen zijn met andere woorden, alle native naar de cloud - dat wil zeggen, gemaakt en beheerd in Azure AD. Kijk eens naar dat Contoso een wolk alleen-lezen is AD Azure huurder. Zoals in de volgende afbeelding, is de Contoso-beheerder een virtueel netwerk in Azure infrastructuurservices geconfigureerd. Toepassingen en werklast van servers worden in deze virtuele netwerk in Azure virtuele machines geïmplementeerd. Contoso is een huurder wolk alleen-lezen, worden de identiteit van alle gebruikers hun referenties en groepslidmaatschappen gemaakt en beheerd in Azure AD.

![Azure AD Domain Services-overzicht](./media/active-directory-domain-services-overview/aadds-overview.png)

Contoso de IT-beheerder kan Azure AD Domain Services voor de huurder Azure AD en domeinservices beschikbaar maken in deze virtuele netwerk kiezen. Nadien Azure AD Domain Services een beheerde domein bepalingen en beschikbaar gemaakt in het virtuele netwerk. Alle gebruikersaccounts, groepslidmaatschappen en referenties van de gebruiker beschikbaar in Azure AD-huurder van Contoso zijn ook beschikbaar in dit nieuwe domein. Deze functie kunnen gebruikers in de organisatie aan te melden bij het domein met hun corporate-referenties - bijvoorbeeld op afstand verbinding met computers via Extern bureaublad domein behoren. Beheerders kunnen de toegang tot bronnen in het domein met behulp van bestaande groepslidmaatschappen inrichten. Toepassingen die worden gebruikt in virtuele machines op het virtuele netwerk kunnen functies zoals aan domein toevoegen, LDAP lezen LDAP bind, NTLM en Kerberos-verificatie en Groepsbeleid gebruiken.

Een aantal belangrijke aspecten van het beheerde domein dat is ingericht door Azure AD Domain Services zijn als volgt:

- Contoso de IT-beheerder hoeft niet te beheren, patch of dit domein of alle domeincontrollers voor dit beheerde domein controleren.
- Er is niet nodig voor het beheren van AD-replicatie voor dit domein. Gebruikersaccounts, groepslidmaatschappen en -referenties van van Contoso Azure AD huurder automatisch beschikbaar zijn in deze beheerde domein.
- Omdat het domein wordt beheerd door Azure AD Domain Services, Contoso de IT-beheerder heeft geen domeinbeheerder of Ondernemingsadministrator-bevoegdheden in dit domein.


### <a name="azure-ad-domain-services-for-hybrid-organizations"></a>Azure AD Domain Services voor hybride organisaties
Organisaties met een hybride IT-infrastructuur verbruiken een mix van middelen op gebouwen en wolken. Deze organisaties synchroniseren identiteitsgegevens uit hun directory op ruimten aan de huurder Azure AD. Hybride organisaties zoals de migratie van meer van hun toepassingen op het bedrijf naar de cloud, met name oudere directory-toepassingen, Azure AD Domain Services kunnen het nuttig zijn voor hen.

Litware Corporation is [Azure AD verbinden](../active-directory/active-directory-aadconnect.md)om te synchroniseren van identiteitsgegevens uit hun directory op ruimten aan de huurder AD Azure geïmplementeerd. Informatie over de wordt gesynchroniseerd bevat gebruikersaccounts, hun hashes referenties voor verificatie (wachtwoord synchronisatie) en groepslidmaatschappen.

> [AZURE.NOTE] **Wachtwoordsynchronisatie is verplicht voor hybride organisaties Azure AD Domain Services te gebruiken**. Deze eis is omdat de referenties van gebruikers in de beheerde domein Azure AD Domain Services, die nodig zijn om deze gebruikers via NTLM of Kerberos-verificatiemethoden te verifiëren.

![Azure AD Domain Services voor Litware Corporation](./media/active-directory-domain-services-overview/aadds-overview-synced-tenant.png)

In de voorgaande afbeelding ziet u hoe organisaties met een hybride IT-infrastructuur, zoals Litware Corporation, Azure AD Domain Services. Litware van toepassingen en werklast van servers waarvoor domain services worden geïmplementeerd in een virtueel netwerk in Azure infrastructuurservices. Litware van IT-beheerders kan Azure AD Domain Services voor de huurder Azure AD en kiest u een beheerde domein beschikbaar maken in deze virtuele netwerk. Aangezien Litware een organisatie met een hybride IT-infrastructuur is, worden gebruikersaccounts, groepen en referenties gesynchroniseerd met de huurder Azure AD uit hun directory op gebouwen. Deze functie kan gebruikers zich aanmelden bij het domein met hun corporate-referenties - bijvoorbeeld als u op afstand verbinding te maken met computers die zijn toegevoegd aan het domein via Extern bureaublad. Beheerders kunnen de toegang tot bronnen in het domein met behulp van bestaande groepslidmaatschappen inrichten. Toepassingen die worden gebruikt in virtuele machines op het virtuele netwerk kunnen functies zoals aan domein toevoegen, LDAP lezen LDAP bind, NTLM en Kerberos-verificatie en Groepsbeleid gebruiken.

Een aantal belangrijke aspecten van het beheerde domein dat is ingericht door Azure AD Domain Services zijn als volgt:

- Het beheerde domein is een zelfstandige domein. Het is niet een uitbreiding van Litware op ruimten domein.
- Litware van IT-beheerder hoeft niet te beheren, patch, of domeincontrollers voor deze beheerde domein controleren.
- Er is niet nodig voor het beheren van AD-replicatie aan dit domein. Gebruikersaccounts, groepslidmaatschappen en referenties uit de Litware op ruimten directory worden gesynchroniseerd naar Azure AD via Azure AD verbinden. Deze gebruikersaccounts, groepslidmaatschappen en referenties automatisch beschikbaar zijn in de beheerde domein.
- Omdat het domein wordt beheerd door Azure AD Domain Services, Litware van IT-beheerder heeft geen domeinbeheerder of Ondernemingsadministrator-bevoegdheden in dit domein.


## <a name="benefits"></a>Voordelen
Azure AD Domain Services, kunt u genieten van de volgende voordelen:

-   **Eenvoudig** : U kunt voldoen aan de behoeften van de identiteit van virtuele machines is geïmplementeerd op Azure Infrastructure services met een paar eenvoudige klikken. U hoeft niet te implementeren en beheren van infrastructuur identiteit in setup of Azure connectiviteit met uw identiteit op gebouwen infrastructuur.

-   **Geïntegreerde** – Azure AD Domain Services is nauw geïntegreerd met uw huurder Azure AD. U kunt nu Azure AD als een geïntegreerde cloud-gebaseerde enterprise-map die aan de behoeften van de moderne toepassingen en de traditionele directory-toepassingen caters.

-   **Compatibel** : Azure AD Domain Services is gebaseerd op de beproefde enterprise grade-infrastructuur van Windows Server Active Directory. Daarom kunnen uw toepassingen vertrouwen op een grotere mate van compatibiliteit met Windows Server Active Directory-functies. Niet alle functies die beschikbaar zijn in Windows Server AD zijn momenteel beschikbaar in Azure AD Domain Services. Beschikbare functies zijn echter compatibel met de bijbehorende Windows Server AD-functies die u op de infrastructuur in gebouwen vertrouwen. De mogelijkheden van de join Kerberos, LDAP, NTLM, Groepsbeleid en domein vormen een volwassen aanbieden die getest en verfijnd over de verschillende versies van Windows Server.

-   **Voordelige** – met Azure AD Domain Services, kunt u voorkomen dat de infrastructuur en het beheer van lasten die is gekoppeld aan de identiteit infrastructuur voor de ondersteuning van de traditionele directory-toepassingen te beheren. U kunt deze toepassingen Azure infrastructuurservices verplaatsen en profiteren van lagere operationele kosten.
