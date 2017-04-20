<properties
    pageTitle="Azure AD Domain Services: Vergelijken Azure AD Domain Services ZELFOPLOSSING domeincontrollers | Microsoft Azure"
    description="Azure Active Directory Domain Services op domeincontrollers ZELFOPLOSSING vergelijken"
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
    ms.date="10/01/2016"
    ms.author="maheshu"/>

# <a name="how-to-decide-if-azure-ad-domain-services-is-right-for-your-use-case"></a>Hoe u kunt bepalen als Azure AD Domain Services-is geschikt voor uw gebruiksvoorbeeld
Azure AD Domain Services kunt u de werklast in Azure infrastructuurservices implementeren zonder te hoeven maken over het onderhoud van de infrastructuur van uw identiteit. Deze beheerde service verschilt van een typische Windows Server Active Directory-implementatie die u implementeren en beheren op uw eigen. De service is bedoeld voor het gemak van implementatie, geautomatiseerde controle en herstel, en een eenvoudige identiteit infrastructuur voor de cloud. We zijn de service voor het toevoegen van ondersteuning voor algemene situaties voortdurend in ontwikkeling.

Om te beslissen of u wilt gebruikmaken van domeinservices Azure AD of omhoog draaien en beheren van uw eigen infrastructuur AD Azure (klussen):

- Overzicht van de [functies die worden aangeboden door Azure AD Domain Services](active-directory-ds-features.md).

- [Implementatiescenario's voor Azure AD Domain Services](active-directory-ds-scenarios.md)bekijken.

- Ten slotte [een optie voor klussen AD Azure AD Domain Services vergelijken](active-directory-ds-comparison.md#compare-azure-ad-domain-services-to-diy-ad-domain-in-azure).


## <a name="compare-azure-ad-domain-services-to-diy-ad-domain-in-azure"></a>Azure AD Domain Services-domein ZELFOPLOSSING AD in Azure vergelijken
De volgende tabel kunt u kiezen tussen Azure AD Domain Services gebruiken en beheren van uw eigen infrastructuur AD in Azure.

|**Functie**|**Azure AD Domain Services**|**'Klussen' AD in Azure VMs**|
|---|:---:|:---:|
|[**Beheerde service**](active-directory-ds-comparison.md#managed-service)|**& #x 2713;**|**& #x en met 2715;**|
|[**Beveiligde implementaties**](active-directory-ds-comparison.md#secure-deployments)|**& #x 2713;**|Beheerder nodig heeft voor het beveiligen van de implementatie.|
|[**DNS-server**](active-directory-ds-comparison.md#dns-server)|**& #x 2713;** (beheerde service)|**& #x 2713;**|
|[**Domein- of administrator-bevoegdheden**](active-directory-ds-comparison.md#domain-or-enterprise-administrator-privileges)|**& #x en met 2715;**|**& #x 2713;**|
|[**Aan domein toevoegen**](active-directory-ds-comparison.md#domain-join)|**& #x 2713;**|**& #x 2713;**|
|[**Domeinverificatie met NTLM en Kerberos**](active-directory-ds-comparison.md#domain-authentication-using-ntlm-and-kerberos)|**& #x 2713;**|**& #x 2713;**|
|[**Aangepaste structuur van organisatie-eenheid**](active-directory-ds-comparison.md#custom-ou-structure)|**& #x 2713;**|**& #x 2713;**|
|[**Schema-uitbreidingen**](active-directory-ds-comparison.md#schema-extensions)|**& #x en met 2715;**|**& #x 2713;**|
|[**AD forest-domeinen en vertrouwensrelaties**](active-directory-ds-comparison.md#ad-domain-or-forest-trusts)|**& #x en met 2715;**|**& #x 2713;**|
|[**LDAP lezen**](active-directory-ds-comparison.md#ldap-read)|**& #x 2713;**|**& #x 2713;**|
|[**Veilige LDAP (LDAPS)**](active-directory-ds-comparison.md#secure-ldap)|**& #x 2713;**|**& #x 2713;**|
|[**LDAP-schrijven**](active-directory-ds-comparison.md#ldap-write)|**& #x en met 2715;**|**& #x 2713;**|
|[**Groepsbeleid**](active-directory-ds-comparison.md#group-policy)|Eenvoudige|Volledige|
|[**Geo gedistribueerde implementaties**](active-directory-ds-comparison.md#geo-dispersed-deployments)|**& #x en met 2715;**|**& #x 2713;**|


#### <a name="managed-service"></a>Beheerde service
Azure AD domeinen worden beheerd door Microsoft. U hoeft niet bang te zijn patches, updates, bewaking, back-ups, en ervoor te zorgen dat de beschikbaarheid van uw domein. Deze taken worden aangeboden als een service door Microsoft Azure voor uw beheerde domeinen.

#### <a name="secure-deployments"></a>Beveiligde implementaties
Het beheerde domein is veilig aan de hand van Microsoft aanbevolen beveiligingsprocedures voor implementaties van AD vergrendeld. Deze best practices vloeien voort uit de AD-productteam decennia ervaring in engineering en AD-implementaties ondersteunen. Voor klussen implementaties moet u de specifieke implementatiestappen ondernemen om te vergrendelen down/beveiligen uw implementatie.

#### <a name="dns-server"></a>DNS-server
Een beheerde Azure AD Domain Services-domein bevat beheerde DNS-services. Leden van de groep 'Beheerders AAD DC' kunnen DNS gebruiken op het beheerde domein beheren. Leden van deze groep krijgen volledige bevoegdheden voor DNS-beheer voor het domein beheerd. DNS-beheer kan worden uitgevoerd met behulp van de 'DNS-beheerconsole' opgenomen in het pakket Remote Server Administration Tools (RSAT).

#### <a name="domain-or-enterprise-administrator-privileges"></a>Domein- of Ondernemingsadministrator-bevoegdheden
Deze verhoogde bevoegdheden zijn niet beschikbaar op een beheerde AAD-DS-domein. Toepassingen die worden geïnstalleerd/uitvoeren deze bevoegdheden nodig tegen beheerde domeinen niet uitvoeren. Een subset van de administratieve bevoegdheden is beschikbaar voor leden van de gedelegeerd beheergroep 'Beheerders AAD DC' genoemd. Deze bevoegdheden zijn bevoegdheden DNS configureren, Groepsbeleid configureren, beheerdersbevoegdheden krijgen op een domein behoren machines enz.

#### <a name="domain-join"></a>Aan domein toevoegen
U kunt virtuele machines toevoegen aan de beheerde domein lijkt op hoe u computers aan een AD-domein toevoegen.

#### <a name="domain-authentication-using-ntlm-and-kerberos"></a>Domeinverificatie met NTLM en Kerberos
Azure AD Domain Services, kunt u uw zakelijke gegevens te verifiëren met het domein beheerd. Referenties worden gesynchroniseerd met uw huurder Azure AD. Voor gesynchroniseerde huurders, Azure AD Connect zorgt u ervoor dat wijzigingen in de referenties die zijn aangebracht in ruimten worden gesynchroniseerd naar Azure AD. Met een ZELFOPLOSSING domain-installatie moet u een domein vertrouwensrelatie met een forest op ruimten account voor gebruikers worden geverifieerd met hun zakelijke referenties instellen. U wilt ook AD replicatie instellen om ervoor te zorgen dat de gebruikerswachtwoorden met uw Azure domain controller virtuele machines synchroniseren.

#### <a name="custom-ou-structure"></a>Aangepaste structuur van organisatie-eenheid
Leden van de groep 'Beheerders AAD DC' kunnen maken van aangepaste organisatie-eenheden binnen het domein beheerd.

#### <a name="schema-extensions"></a>Schema-uitbreidingen
Het basisschema van een beheerde Azure AD Domain Services-domein kunnen niet worden uitgebreid. Toepassingen die afhankelijk van uitbreidingen van AD-schema (bijvoorbeeld nieuwe kenmerken onder het gebruikersobject zijn) kunnen niet worden opgeheven en daarom verschoven naar AAD DS-domeinen.

#### <a name="ad-domain-or-forest-trusts"></a>AD-domein of Forest-vertrouwensrelaties
Beheerde domeinen worden niet geconfigureerd voor het instellen van vertrouwensrelaties (inkomende/uitgaande) met andere domeinen. Daarom scenario's zoals het bron-forest implementaties of in gevallen waar u liever niet synchroniseren van wachtwoorden naar Azure AD Azure AD Domain Services niet gebruiken.

#### <a name="ldap-read"></a>LDAP-kenmerk
Het beheerde domein ondersteunt LDAP werklasten lezen. Daarom kunt u toepassingen die LDAP-leesbewerkingen ten opzichte van de beheerde domein implementeren.

#### <a name="secure-ldap"></a>Veilige LDAP
U kunt de Azure AD Domain Services om u te voorzien van veilige LDAP-toegang tot uw beheerde domein, ook via het internet.

#### <a name="ldap-write"></a>LDAP-schrijven
Het beheerde domein is alleen-lezen voor gebruikersobjecten. Toepassingen die LDAP-schrijfbewerkingen tegen de kenmerken van het gebruikersobject werkt daarom niet in een beheerde domein. Bovendien kunnen niet wachtwoorden worden gewijzigd in het domein beheerd. Een ander voorbeeld is de wijziging van groepslidmaatschappen of Groepkenmerken binnen het domein beheerd, dat is niet toegestaan. Alle wijzigingen gebruikerskenmerken of wachtwoorden die in Azure AD (via PowerShell Azure/portal) of op lokalen AD worden gesynchroniseerd met de beheerde AAD-DS-domein.

#### <a name="group-policy"></a>Groepsbeleid
Geavanceerde Groepsbeleid constructies worden niet ondersteund op het beheerde AAD-DS-domein. Bijvoorbeeld: u kunt maken en afzonderlijke groepsbeleidsobjecten implementeren voor elke aangepaste organisatie-eenheid in het domein of WMI filtering voor GP doelitems gebruiken. Er is een ingebouwde Groepsbeleidobject elke voor de containers 'AADDC Computers' en 'AADDC Users', die kan worden aangepast voor het configureren van Groepsbeleid.

#### <a name="geo-dispersed-deployments"></a>Geo werkende implementaties
Azure AD beheerde domeinen zijn beschikbaar in een enkele virtuele netwerk in Azure. Voor scenario's waarin domeincontrollers beschikbaar zijn in meerdere Azure regio's over de hele wereld vereisen, instellen van domeincontrollers in Azure IaaS VMs mogelijk beter alternatief.


## <a name="do-it-yourself-diy-ad-deployment-options"></a>'Klussen' (DIY) AD implementatie-opties
Mogelijk hebt u gebruiksvoorbeelden implementatie waar u enkele van de mogelijkheden die worden aangeboden door een installatie van Windows Server AD nodig hebt. In deze gevallen kunt u op een van de volgende opties voor klussen (DIY):

- **Zelfstandige wolk domein:** U kunt een zelfstandige wolk domein' met Azure virtuele machines die zijn geconfigureerd als domeincontrollers instellen. Deze infrastructuur is geen interactie met uw locatie op AD-omgeving. Deze optie vereist een andere reeks 'cloud referenties' aanmelden/beheren van VMs in de cloud.

- **Bron-forest implementatie:** U kunt instellen een domein in het bron-forest-topologie met Azure virtuele machines die zijn geconfigureerd als domeincontroller. Vervolgens kunt u een AD-vertrouwensrelatie met uw locatie op AD-omgeving. U kunt domeinen aan computers (VMs Azure) aan dit forest met de bronnen in de cloud. Verificatie van de gebruiker gebeurt via een VPN-/ ExpressRoute verbinding met de map op locatie.

- **Uw domein op het bedrijf uitbreiden naar Azure:** Kunt u een virtueel netwerk van Azure uw op gebouwen-netwerk via een VPN-/ ExpressRoute, zodat de Azure VMs kan lid zijn van uw locatie op AD. Een ander alternatief is het promoveren van replicadomeincontrollers van uw domein op gebouwen in Azure als een VM. U kunt vervolgens instellen dat om te repliceren via een VPN-/ ExpressRoute verbinding naar de map op locatie. In deze modus implementatie breidt effectief uw domein op ruimten naar Azure.

> [AZURE.NOTE] U kan bepalen dat een optie ZELFOPLOSSING is beter geschikt voor uw implementatie gebruiksvoorbeelden. Kunt u [delen van feedback](active-directory-ds-contact-us.md) helpt ons begrijpen hoe functies kunt u in de toekomst Azure AD Domain Services kiest. Deze feedback helpt ons de service aan uw implementatievereisten en use-cases te ontwikkelen.

We hebben gepubliceerd voor [richtlijnen voor het implementeren van Windows Server Active Directory Azure virtuele Machines](https://msdn.microsoft.com/library/azure/jj156090.aspx) ZELFOPLOSSING installaties te vereenvoudigen.


## <a name="related-content"></a>Verwante inhoud
- [Functies - Azure AD Domain Services](active-directory-ds-features.md)

- [Implementatiescenario's - Azure AD Domain Services](active-directory-ds-scenarios.md)

- [Richtlijnen voor de implementatie van Windows Server Active Directory op Azure virtuele Machines](https://msdn.microsoft.com/library/azure/jj156090.aspx)
