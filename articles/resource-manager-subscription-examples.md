<properties
   pageTitle="Scenario's en voorbeelden voor governance abonnement | Microsoft Azure"
   description="Bevat voorbeelden van het abonnement Azure governance voor veelvoorkomende scenario's te implementeren."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="rdendtler"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/05/2016"
   ms.author="rodend;karlku;tomfitz"/>

# <a name="examples-of-implementing-azure-enterprise-scaffold"></a>Voorbeelden van de implementatie van Azure enterprise scaffold

Dit onderwerp bevat voorbeelden van hoe een onderneming de aanbevelingen voor een [onderneming Azure scaffold](resource-manager-subscription-governance.md)kunt implementeren. Een fictief bedrijf met de naam Contoso wordt gebruikt ter illustratie van de aanbevolen procedures voor veelvoorkomende scenario's. 

## <a name="background"></a>Achtergrond

Contoso is een wereldwijd bedrijf dat supply chain oplossingen voor klanten in alles, van een model 'Software als een Service' aan een ingepakte model biedt geïmplementeerd op gebouwen.  Deze ontwikkelen software over de hele wereld met belangrijke ontwikkelingscentra in India, de Verenigde Staten en Canada. 

De ISV-gedeelte van het bedrijf is opgesplitst in verschillende onafhankelijke business units die producten in een grote onderneming te beheren. Elke business unit heeft een eigen ontwikkelaars, productmanagers en architecten. 

De business unit van Enterprise Technology Services (ETS) biedt gecentraliseerde IT-capaciteit en beheert meerdere datacenters waar bedrijfsunits hun toepassingen hosten. Bij het beheren van de datacenters, ETS en de organisatie biedt gecentraliseerde samenwerking (zoals e-mail en websites) en network/telefonie services beheert. Ze beheren klantgerichte werkbelasting voor kleinere business units die geen operationele personeel. 

In dit onderwerp worden de volgende personas gebruikt:

- Dave is de beheerder van de ETS Azure.
- Alice is directeur van de ontwikkeling van Contoso in de business unit van de supply chain. 

Contoso moet een bedrijfs app en een klantgerichte app bouwen. Zij heeft besloten uit te voeren van de apps op Azure. Dave leest het onderwerp [governance uitgebreide abonnement](resource-manager-subscription-governance.md) en is nu klaar voor de uitvoering van de aanbevelingen. 

## <a name="scenario-1-line-of-business-application"></a>Scenario 1: van branchespecifieke toepassingen

Contoso is bezig met een bron code management system (BitBucket) moet worden gebruikt door ontwikkelaars over de hele wereld.  Infrastructuur gebruikt als een Service (IaaS) voor het hosten van de toepassing en bestaat uit web-servers en een databaseserver. Ontwikkelaars toegang tot servers in hun ontwikkeling omgevingen, maar ze niet nodig hebt voor toegang tot de servers in Azure. Contoso ETS wil toestaan dat de eigenaar van de toepassing en het team voor het beheer van de toepassing. De toepassing is alleen beschikbaar in het bedrijfsnetwerk van Contoso. Dave moet voor het instellen van het abonnement voor deze toepassing. Het abonnement wordt ook in de toekomst host andere software ontwikkelaars.  

### <a name="naming-standards--resource-groups"></a>Naamgevingsstandaarden & resourcegroepen

Dave maakt een abonnement voor de ondersteuning van ontwikkelprogramma's die gemeenschappelijk voor alle business units zijn. Zo moet hij herkenbare namen voor het abonnement en resource groepen (voor de toepassing en de netwerken). Hij maakt de volgende abonnement en resource-groepen:

| Artikel | Naam | Beschrijving |
| ---- | ---- | ----------- |
| Abonnement | ETS Contoso DeveloperTools productie | Ondersteunt gemeenschappelijk ontwikkelprogramma 's |
| Resourcegroep | rgBitBucket | De toepassing webserver en databaseserver |
| Resourcegroep | rgCoreNetworks | Bevat de virtuele netwerken en verbinding van site naar site gateway |


### <a name="role-based-access-control"></a>Toegangsbeheer op basis van rollen

Na het maken van zijn abonnement wil Dave om ervoor te zorgen dat de juiste teams en eigenaren van de toepassing toegang heeft tot de bronnen ervan. Dave herkent dat elk team verschillende vereisten heeft. Hij maakt gebruik van de groepen die zijn gesynchroniseerd van Contoso op-ruimten Active Directory (AD) naar Azure Active Directory en biedt het juiste niveau van toegang tot de teams. 

Dave wordt toegewezen voor de volgende functies voor het abonnement: 

| Rol | Toegewezen aan | Beschrijving |
| ---- | ----------- | ----------- |
| [Eigenaar](./active-directory/role-based-access-built-in-roles.md#owner) | ID van Contoso van beheerde AD | Deze ID wordt beheerd met Just in Time (JIT)-toegang via van Contoso Identity Management tool en zorgt ervoor dat volledig abonnement eigenaar toegang wordt gecontroleerd. |
| [Security Manager](./active-directory/role-based-access-built-in-roles.md#security-manager) | Beveiliging en risico management afdeling | Deze rol kan gebruikers de Azure Security Center en de status van de resources bekijken. |
| [Medewerker netwerk](./active-directory/role-based-access-built-in-roles.md##network-contributor) | Netwerkteam | Deze functie kan netwerk-team van Contoso de VPN-verbinding van Site naar Site en virtuele netwerken beheren. |
| *Aangepaste rol* | De eigenaar van toepassing | Dave maakt een rol die het biedt de mogelijkheid voor het wijzigen van de resources in de resourcegroep. Zie [Aangepaste rollen in Azure RBAC](./active-directory/role-based-access-control-custom-roles.md) voor meer informatie. |

### <a name="policies"></a>Beleid

Dave gelden de volgende vereisten voor het beheren van bronnen in het abonnement:

- Omdat de ontwikkelprogramma's ondersteuning voor ontwikkelaars over de hele wereld, wil hij niet verhinderen dat gebruikers maken van bronnen in elke regio. Hij moet echter wel weten waar de resources worden gemaakt. 
- Hij is betrokken bij de kosten. Daarom wil hij dat eigenaren van toepassing onnodig dure virtuele machines maken.  
- Omdat deze toepassing ontwikkelaars in vele business units dient, wil hij met de eigenaar van de business unit en de toepassing van elke resource label. Met behulp van deze codes kunt ETS rekening de juiste teams.

Hij maakt de volgende [Resource Manager-beleid](resource-manager-policy.md): 

| Veld | Effect | Beschrijving |
| ----- | ------ | ----------- |
| locatie | audit | Het maken van de bronnen in elke regio controleren |
| type | weigeren | Het maken van virtuele machines voor G-serie weigeren |
| tags | weigeren | Toepassing eigenaar tag vereist |
| tags | weigeren | De tag center kosten vereisen |
| tags | toevoegen | Tagnaam **Business Unit** en de labelwaarde **ETS** toevoegen aan alle resources |


### <a name="resource-tags"></a>Bron codes

Dave begrijpt dat hij nodig heeft om specifieke informatie over het identificeren van de kostenplaats voor de uitvoering van BitBucket voor de factuur. Dave wil bovendien de middelen waarin ETS weten. 

Hij wordt de volgende [codes](resource-group-using-tags.md) toegevoegd aan de bronnengroepen en bronnen. 
 
| Labelnaam | Labelwaarde |
| -------- | --------- |
| ApplicationOwner | De naam van de persoon die deze toepassing beheert. |
| CostCenter | De kostenplaats van de groep die wordt betaald om het verbruik van Azure. |
| Business Unit | **ETS** (de business unit die is gekoppeld aan het abonnement) |

### <a name="core-network"></a>Core netwerk

Het ETS Contoso informatie beveiliging en risico managementteam beoordeelt de voorgestelde plan van Dave te verplaatsen van de toepassing op Azure. Ze willen om ervoor te zorgen dat de toepassing niet wordt blootgesteld aan internet.  Dave heeft ook de developer apps die in de toekomst zullen worden verplaatst naar Azure. Deze apps vereisen voor openbare interfaces.  Aan deze voorwaarden voldoen, geeft hij zowel interne als externe virtuele netwerken en een beveiligingsgroep netwerk toegang te beperken.

Hij maakt de volgende bronnen:

| Resourcetype | Naam | Beschrijving |
| ------------- | ---- | ----------- |
| Virtueel netwerk | vnInternal | Met de toepassing BitBucket gebruikt en via ExpressRoute is verbonden met het bedrijfsnetwerk van Contoso.  Een subnet (sbBitBucket) biedt de toepassing met een specifiek IP-adresruimte. |
| Virtueel netwerk | vnExternal | Beschikbaar voor toekomstige toepassingen waarvoor openbare eindpunten. |
| Network Security Group | nsgBitBucket | Zorgt ervoor dat de kwetsbaarheid van de werklast is geminimaliseerd doordat verbindingen op poort 443 voor het subnet waarin de toepassing zich (sbBitBucket). |

### <a name="resource-locks"></a>Resource-sloten 

Dave herkent dat de verbinding met het interne netwerk virtuele bedrijfsnetwerk van Contoso moet worden beschermd tegen wayward script of per ongeluk wordt verwijderd. 

Hij maakt de volgende [resource vergrendelen](resource-group-lock-resources.md):

| Vergrendelingstype | Resource | Beschrijving |
| --------- | -------- | ----------- |
| **CanNotDelete** | vnInternal | Voorkomen dat gebruikers verwijderen van virtuele netwerken of subnetten, maar wordt niet voorkomen dat de toevoeging van nieuwe subnetten. |

### <a name="azure-automation"></a>Azure automatisering 

Dave heeft niets te automatiseren voor deze toepassing. Hoewel hij een Azure automatisering-account hebt gemaakt, niet hij het in eerste instantie gebruiken. 

### <a name="azure-security-center"></a>Azure Security Center 

Contoso IT service management moet snel identificeren en bedreigingen te verwerken. Ze willen ook weten welke problemen zich kunnen voordoen.  

Om te voldoen aan deze vereisten, Dave kan de [Azure Security Center](./security-center/security-center-intro.md)en biedt toegang tot de functie Security Manager. 

## <a name="scenario-2-customer-facing-app"></a>Scenario 2: klantgerichte app

Het zakelijke leiderschap in de supply chain business unit heeft verschillende mogelijkheden om de betrokkenheid bij klanten van Contoso vergroten door een loyaliteitskaart geïdentificeerd. Alice's team deze toepassing moet maken en besluit dat Azure verhoogt het vermogen om te voldoen aan de zakelijke behoeften. Lisa werkt met Dave van ETS twee abonnementen voor het ontwikkelen en gebruiken deze toepassing te configureren.

### <a name="azure-subscriptions"></a>Azure-abonnementen 

Dave zich aanmelden bij Enterprise Portal Azure en ziet de afdeling supply chain al bestaat.  Dit project is het eerste ontwikkelingsproject voor het supply chain team in Azure, herkent Dave de noodzaak voor een nieuwe account voor het ontwikkelteam van Alice.  Hij maakt van de 'R & D' account voor haar team en access wijst Alice. Lisa zich aanmeldt via de portal van de Account en maakt u twee abonnementen: één waarin de Ontwikkelingsservers en één waarin de productieservers.  Zij de eerder vastgestelde normen voor naamgeving wordt gebruikt bij het maken van de volgende abonnementen: 

| Abonnement gebruik | Naam |
| ---------------- | ---- |
| Ontwikkeling | Ontwikkeling van Supply Chain ResearchDevelopment LoyaltyCard |
| Productie | Supply Chain Operations LoyaltyCard productie |

### <a name="policies"></a>Beleid

Dave en Alice de toepassing te bespreken en vaststellen dat deze toepassing alleen klanten in de regio Noord-Amerikaanse fungeert.  Lisa en haar team plannen van Azure Service toepassingsomgeving en Azure SQL gebruiken om de toepassing te maken. Deze nodig hebben voor het maken van virtuele machines tijdens de ontwikkeling.  Lisa om ervoor te zorgen dat haar ontwikkelaars beschikken over de middelen die ze nodig hebben om te verkennen en onderzoeken problemen zonder trekken in ETS. 

Ze maken het volgende beleid voor de **ontwikkeling abonnement**:

| Veld | Effect | Beschrijving |
| ----- | ------ | ----------- |
| locatie | audit | Controle van het maken van de bronnen in elke regio. |

Ze vormen geen beperking van het type van een gebruiker in ontwikkeling maken kan sku en tags is niet vereist voor resources of resourcegroepen.

Ze maken de volgende beleidsregels voor het **abonnement van de productie**:

| Veld | Effect | Beschrijving |
| ----- | ------ | ----------- |
| locatie | weigeren | Het maken van bronnen buiten de V.S.-gegevenscentra ontzeggen. |
| tags | weigeren | Toepassing eigenaar tag vereist |
| tags | weigeren | Afdeling tag vereist. | 
| tags | toevoegen | Tag toevoegen aan elke bronnengroep waarin de productie-omgeving. |

Het type van een gebruiker in de productie maken kan sku beperken niet.

### <a name="resource-tags"></a>Bron codes 

Dave begrijpt dat hij nodig heeft om specifieke informatie voor het identificeren van de juiste business groepen voor facturering en eigendom. Hij definieert de bron codes voor de bronnengroepen en bronnen. 
 
Labelnaam | Labelwaarde |
| -------- | --------- |
| ApplicationOwner | De naam van de persoon die deze toepassing beheert. |
| Afdeling | De kostenplaats van de groep die wordt betaald om het verbruik van Azure. |
| EnvironmentType | **Productie** (Hoewel het abonnement omvat de **productie** in de naam, met inbegrip van deze code kan gemakkelijk kunt herkennen bij het onderzoeken van bronnen in het portal of op de factuur.) |

### <a name="core-networks"></a>Core-netwerken

Het ETS Contoso informatie beveiliging en risico managementteam beoordeelt de voorgestelde plan van Dave te verplaatsen van de toepassing op Azure. Ze willen om ervoor te zorgen dat de toepassing loyaliteitskaart goed geïsoleerd en beschermd zijn in een DMZ-netwerk.  Maak een extern virtueel netwerk en een beveiligingsgroep netwerk te isoleren van de loyaliteitskaart toepassing met het bedrijfsnetwerk Contoso om te voldoen aan deze eis, Dave en Alice.  

Ze maken voor het **abonnement ontwikkeling**:

| Resourcetype | Naam | Beschrijving |
| ------------- | ---- | ----------- |
| Virtueel netwerk | vnInternal | De ontwikkelomgeving van Contoso loyaliteitskaart fungeert en via ExpressRoute is verbonden met het bedrijfsnetwerk van Contoso. |

Ze maken voor het **abonnement van de productie**:

| Resourcetype | Naam | Beschrijving |
| ------------- | ---- | ----------- |
| Virtueel netwerk | vnExternal | De toepassing loyaliteitskaart gehost en niet rechtstreeks is aangesloten op de ExpressRoute van Contoso. Code wordt rechtstreeks aan de PaaS-services via hun systeem van broncode geduwd. |
| Network Security Group | nsgBitBucket | Zorgt ervoor dat de kwetsbaarheid van de werklast is geminimaliseerd doordat alleen de communicatie-gebonden op TCP-443.  Contoso onderzoekt ook met behulp van een Web Application Firewall voor extra bescherming. |  

### <a name="resource-locks"></a>Resource-sloten 

Dave en Alice verlenen en vergrendelingen resource toevoegen aan enkele van de belangrijkste bronnen in de omgeving om te voorkomen dat per ongeluk wordt verwijderd tijdens een onjuiste code push bepalen. 

Ze maken de volgende vergrendeling:

| Vergrendelingstype | Resource | Beschrijving |
| --------- | -------- | ----------- |
| **CanNotDelete** | vnExternal | Om te voorkomen dat mensen de virtuele netwerken of subnetten verwijderen. De vergrendeling voorkomt niet dat de toevoeging van nieuwe subnetten. |

### <a name="azure-automation"></a>Azure automatisering 

Lisa en haar ontwikkelingsteam hebben uitgebreide runbooks voor het beheer van het milieu voor deze toepassing. De runbooks kunnen de toevoegen/verwijderen van knooppunten voor de toepassing en andere taken van DevOps. 

Voor het gebruik van deze runbooks kunnen [automatiseren](./automation/automation-intro.md).

### <a name="azure-security-center"></a>Azure Security Center 

Contoso IT service management moet snel identificeren en bedreigingen te verwerken. Ze willen ook weten welke problemen zich kunnen voordoen.  

Om te voldoen aan deze vereisten voldoet, kunt u Dave Azure Security Center. Hij zorgt ervoor dat Azure Beveiligingscentrum controleert de bronnen biedt toegang tot de teams van DevOps en beveiliging. 

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het maken van sjablonen voor bronbeheer, Zie [Aanbevolen procedures voor het maken van sjablonen voor Azure Resource Manager](resource-manager-template-best-practices.md).

*[Karl Kuhnhausen](https://github.com/karlkuhnhausen) bijgedragen aan dit onderwerp.*
