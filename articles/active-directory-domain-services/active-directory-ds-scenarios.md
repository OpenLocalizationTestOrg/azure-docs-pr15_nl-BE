<properties
    pageTitle="Azure Active Directory Domain Services: Implementatiescenario's | Microsoft Azure"
    description="Implementatiescenario's voor Azure AD Domain Services"
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
    ms.date="09/21/2016"
    ms.author="maheshu"/>


# <a name="deployment-scenarios-and-use-cases"></a>Implementatiescenario's en use-cases
In dit gedeelte bekijken we enkele scenario's en in de gevallen die van domeinservices Azure Active Directory (AD profiteren).

## <a name="secure-easy-administration-of-azure-virtual-machines"></a>Veilig en eenvoudig beheer van Azure virtual machines
Azure Active Directory Domain Services kunt u uw Azure virtuele machines op een gestroomlijnde manier beheren. Azure virtuele machines kunnen worden gekoppeld aan het beheerde domein, zodat u uw bedrijf AD-referenties gebruikt aan te melden. Deze aanpak voorkomt referentie management problemen zoals het onderhoud van de lokale administrator-accounts op elk van uw Azure virtuele machines.

Server virtuele machines die worden samengevoegd met het domein beheerd kan ook worden beheerd en beveiligd met behulp van Groepsbeleid. Als u vereiste basislijnen van toepassing op uw Azure virtuele machines en vergrendelen volgens bedrijfsbreed richtsnoeren. Bijvoorbeeld, kunt u group policy management mogelijkheden voor het beperken van de typen toepassingen die op deze virtuele computers kunnen worden gestart.

![Gestroomlijnd beheer van Azure virtual machines](./media/active-directory-domain-services-scenarios/streamlined-vm-administration.png)

Zoals servers en andere infrastructuur is einde van de levenscyclus bereikt, verplaatsen van Contoso is veel toepassingen die momenteel worden gehost op ruimten naar de cloud. Hun huidige IT-standaard bepaalt dat servers die als host fungeert voor bedrijfstoepassingen moeten domein behoren en beheerd met behulp van Groepsbeleid. Contoso de IT-beheerder liever domein join virtuele machines in Azure, gedistribueerd beheer te vereenvoudigen. Hierdoor kunnen kunnen beheerders en gebruikers aanmelden met hun zakelijke referenties. Op hetzelfde moment, kunnen de machines worden geconfigureerd om te voldoen aan de vereiste basislijnen met behulp van Groepsbeleid. Contoso liever niet te implementeren, bewaken en beheren van domeincontrollers in Azure Azure virtuele machines beveiligen. Azure AD Domain Services is daarom een prima geschikt voor deze gebruiksvoorbeeld.

**Implementatie van notities**

Houd rekening met de volgende belangrijke punten in dit scenario voor implementatie:

- Beheerde domeinen die door het AD-Domain Azure Services bieden een enkele, platte structuur OU (organisatie-eenheid) standaard. Alle computers die deel uitmaakt van een domein zich in een enkele platte organisatie-eenheid bevinden. U kunt echter wel aangepaste organisatie-eenheden maken.

- Azure AD Domain Services biedt ondersteuning voor eenvoudige Groepsbeleid in de vorm van een ingebouwde Groepsbeleidobject elke voor de gebruikers en computers containers. U kan niet richten GP per organisatie-eenheid/afdeling, WMI-filters uitvoeren of aangepaste groepsbeleidsobjecten maken.

- Azure AD Domain Services ondersteunt het AD computer object basisschema. U kunt het computerobject schema niet uitbreiden.


## <a name="lift-and-shift-an-on-premises-application-that-uses-ldap-bind-authentication-to-azure-infrastructure-services"></a>Lift-en-shift een toepassing op de ruimten die gebruikmaakt van verificatie van de LDAP bind Azure infrastructuur Services

![LDAP-binding](./media/active-directory-domain-services-scenarios/ldap-bind.png)

Contoso heeft een toepassing op de ruimten die is gekocht bij een ISV vele jaren geleden. De toepassing is momenteel in de onderhoudsmodus van de ISV en wijzigingen aanvragen op de aanvraag is onevenredig duur voor Contoso. Deze toepassing heeft een web-gebaseerde frontend die via een webformulier gebruikersreferenties worden verzameld en vervolgens gebruikers worden geverifieerd door het uitvoeren van een LDAP-binding aan corporate Active Directory. Contoso wilt migreren Azure infrastructuurservices van deze toepassing. Het is wenselijk dat de toepassing wordt zonder wijzigingen werkt. Gebruikers moeten ook kunnen worden geverifieerd met hun bestaande zakelijke referenties en zonder retrain gebruikers om dingen anders te doen. Met andere woorden, moeten eindgebruikers oblivious van waar de toepassing wordt uitgevoerd en de migratie moet transparant zijn voor hen.

**Implementatie van notities**

Houd rekening met de volgende belangrijke punten in dit scenario voor implementatie:

- Zorg ervoor dat de toepassing niet hoeft te wijzigen/schrijven naar de map. LDAP-schrijftoegang tot beheerde domeinen die door het AD-Domain Azure Services wordt niet ondersteund.

- U kunt wachtwoorden rechtstreeks ten opzichte van de beheerde domein niet wijzigen. Eindgebruikers kunnen ofwel hun wachtwoord kunnen wijzigen met Azure AD zelf wachtwoord wijzigen mechanisme of ten opzichte van de map op locatie. Deze wijzigingen worden automatisch gesynchroniseerd en beschikbaar zijn in het domein beheerd.


## <a name="lift-and-shift-an-on-premises-application-that-uses-ldap-read-to-access-the-directory-to-azure-infrastructure-services"></a>Lift en shift een toepassing op de ruimten die met behulp van LDAP lezen toegang tot de map Azure-infrastructuurservices
Contoso heeft een in de lokalen line of business (LOB)-toepassing die bijna een decennium geleden is ontwikkeld. Deze toepassing is op de hoogte van map en is ontworpen voor gebruik met Windows Server AD. De toepassing gebruikt LDAP (Lightweight Directory Access Protocol) voor het lezen van informatie/kenmerken over gebruikers van Active Directory. De toepassing niet kenmerken wijzigen of anderszins te schrijven naar de map. Contoso wilt migreren van deze toepassing Azure-infrastructuurservices en intrekken van de veroudering op ruimten hardware momenteel deze toepassing als host optreedt. De toepassing kan niet worden herschreven als moderne directory API's zoals de Azure AD Graph REST gebaseerde API wilt gebruiken. Een optie voor de lift en shift is daarom gewenst waarbij de toepassing wilt uitvoeren in de cloud, zonder de code te wijzigen of de toepassing opnieuw kan worden gemigreerd.

**Implementatie van notities**

Houd rekening met de volgende belangrijke punten in dit scenario voor implementatie:

- Zorg ervoor dat de toepassing niet hoeft te wijzigen/schrijven naar de map. LDAP-schrijftoegang tot beheerde domeinen die door het AD-Domain Azure Services wordt niet ondersteund.

- Zorg ervoor dat de toepassing een aangepaste/extended Active Directory-schema niet hoeft. Schema-uitbreidingen worden niet ondersteund in Azure AD Domain Services.


## <a name="migrate-an-on-premises-service-or-daemon-application-to-azure-infrastructure-services"></a>Een toepassing op ruimten service of -daemon Azure infrastructuur Services migreren
Sommige toepassingen bestaan uit meerdere niveaus, waarin een van de lagen moet voor het uitvoeren van een reeks back-end zoals een databaselaag geverifieerde oproepen. Active Directory service-accounts vaak worden gebruikt voor deze gebruiksvoorbeelden. U kunt lift en shift dergelijke toepassingen Azure-infrastructuurservices en Azure AD Domain Services voor de behoeften van de identiteit van deze toepassingen gebruikt. U kunt de service-account van uw directory op de ruimten wordt gesynchroniseerd met Azure AD gebruiken. U kunt ook eerst een aangepaste organisatie-eenheid maken en vervolgens een afzonderlijke service-account maken in die organisatie-eenheid voor de implementatie van dergelijke toepassingen.

![Met behulp van WIA-serviceaccount](./media/active-directory-domain-services-scenarios/wia-service-account.png)

Contoso heeft een op maat gemaakte kluis softwaretoepassing met een web-front-end, een SQL-server en een back-FTP-endserver. Geïntegreerde Windows-verificatie van serviceaccounts wordt gebruikt voor het verifiëren van de web-front-end op de FTP-server. Front-end voor het web wordt uitgevoerd als een service-account instellen. De back endserver is geconfigureerd voor het toestaan van toegang van de serviceaccount voor het web-front-end. Contoso liever niet een domeincontroller virtuele machine in de cloud Azure infrastructuurservices van deze toepassing gaan implementeren. Van Contoso IT-beheerders de servers waarop de web-front-end, SQL server en de FTP-server naar Azure virtuele machines kunt implementeren. Deze machines worden vervolgens toegevoegd aan een beheerde Azure AD Domain Services-domein. Vervolgens kunnen ze dezelfde serviceaccount gebruiken in hun directory op gebouwen ter verificatie van de app. Deze serviceaccount is gesynchroniseerd met de beheerde Azure AD Domain Services-domein en is beschikbaar voor gebruik.

**Implementatie van notities**

Houd rekening met de volgende belangrijke punten in dit scenario voor implementatie:

- Zorg ervoor dat de toepassing gebruikmaakt van gebruikersnaam en wachtwoord voor verificatie. Op basis van certificaat/Smartcard-verificatie wordt niet ondersteund door Azure AD Domain Services.

- U kunt wachtwoorden rechtstreeks ten opzichte van de beheerde domein niet wijzigen. Eindgebruikers kunnen ofwel hun wachtwoord kunnen wijzigen met Azure AD zelf wachtwoord wijzigen mechanisme of ten opzichte van de map op locatie. Deze wijzigingen worden automatisch gesynchroniseerd en beschikbaar zijn in het domein beheerd.


## <a name="azure-remoteapp"></a>Azure RemoteApp
Azure RemoteApp kan van Contoso-beheerder voor het maken van een collectie domein behoren. Met deze functie kunt externe toepassingen die worden bediend door Azure RemoteApp uit te voeren op computers die deel uitmaakt van een domein en toegang krijgen tot andere bronnen met behulp van geïntegreerde Windows-verificatie. Contoso kunt Azure AD Domain Services bieden een beheerde domein dat wordt gebruikt door de collecties van Azure RemoteApp-domein behoren.

![Azure RemoteApp](./media/active-directory-domain-services-scenarios/azure-remoteapp.png)

Zie het artikel Blog van extern bureaublad-Services voor meer informatie over deze implementatiescenario [Lift en shift uw werkbelasting met Azure RemoteApp- en Azure AD Domain Services](http://blogs.msdn.com/b/rds/archive/2016/01/19/lift-and-shift-your-workloads-with-azure-remoteapp-and-azure-ad-domain-services.aspx).
