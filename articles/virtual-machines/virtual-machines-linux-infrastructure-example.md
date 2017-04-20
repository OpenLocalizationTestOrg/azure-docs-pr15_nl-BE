<properties
    pageTitle="Voorbeeld van de infrastructuur scenario | Microsoft Azure"
    description="Meer informatie over de belangrijke ontwerp en de implementatie van de richtlijnen voor de implementatie van een voorbeeld van de infrastructuur in Azure."
    documentationCenter=""
    services="virtual-machines-linux"
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/08/2016"
    ms.author="iainfou"/>

# <a name="example-azure-infrastructure-walkthrough"></a>Voorbeeld van de infrastructuur voor Azure scenario

[AZURE.INCLUDE [virtual-machines-linux-infrastructure-guidelines-intro](../../includes/virtual-machines-linux-infrastructure-guidelines-intro.md)] 

Dit artikel helpt bij het gebouw van de infrastructuur van een voorbeeld van de toepassing. Een infrastructuur voor een eenvoudige online winkel die de richtsnoeren en besluiten rond de naamgeving, beschikbaarheid sets, virtuele netwerken en netwerktaakverdeling samenbrengt ontwerpen en distribueren van uw virtuele machines (VMs) detail te beschrijven.


## <a name="example-workload"></a>Voorbeeld van de werkbelasting

Adventure Works Cycle wil maken van een toepassing voor on line winkel in Azure die bestaat uit:

- Twee nginx-servers waarop de client in een weblaag front-end
- Twee nginx servers verwerken van gegevens en orders in een toepassingslaag
- Twee MongoDB-servers is een onderdeel van een cluster een laptopgeheugen om productgegevens en -orders op te slaan in een databaselaag
- Twee Active Directory-domeincontrollers voor klanten en leveranciers in een laag verificatie
- Alle servers bevinden zich in twee subnetten:
    - een subnet front-end voor het webservers 
    - een back-end-subnet voor toepassingsservers, MongoDB cluster en -domeincontrollers

![Diagram van verschillende niveaus voor de infrastructuur van de toepassing](./media/virtual-machines-common-infrastructure-service-guidelines/example-tiers.png)

Inkomende beveiligde Internet-verkeer moet worden verdeeld tussen de webservers als klanten bladeren in de on line winkel. Order verwerking van netwerkverkeer in de vorm van HTTP-aanvragen van het web servers moeten worden verdeeld over de toepassingsservers. Bovendien moet de infrastructuur worden ontworpen voor hoge beschikbaarheid.

Het resulterende ontwerp omvat:

- Een Azure abonnement en account
- Één brongroep
- Opslag-accounts
- Een virtueel netwerk met twee subnetten
- Beschikbaarheid wordt ingesteld voor de VMs met een vergelijkbare rol
- Virtuele machines

Alle bovenstaande voldoen aan deze naamgeving:

- Adventure Works Cycle gebruikt **[IT werkbelasting]-[locatie]-[Azure bron]** als voorvoegsel
    - Bijvoorbeeld: '**azos**' (Azure On-line Store) is de naam van de werkbelasting van IT en '**gebruiken**' (Oost-VS 2) de locatie
- Opslag accounts adventureazosusesa**[beschrijving]** gebruiken
    - 'adventure' is toegevoegd aan het voorvoegsel voor uniekheid en accountnamen opslag biedt geen ondersteuning voor het gebruik van afbreekstreepjes.
- Virtuele netwerken maken gebruik van AZOS-gebruik-VN**[nummer]**
- Beschikbaarheid gebruik azos-gebruikt-als-**[functie]**
- Namen van de virtuele machine gebruikt azos-gebruik-vm -**[vmname]**


## <a name="azure-subscriptions-and-accounts"></a>Azure abonnementen en accounts

Adventure Works Cycle gebruikt om de facturering van de werklast van IT hun Enterprise-abonnement met de naam Adventure Works onderneming abonnement.


## <a name="storage-accounts"></a>Opslag-accounts

Adventure Works Cycle bepaald dat deze nodig twee opslag rekeningen zijn:

- **adventureazosusesawebapp** voor de standaard opslag van webservers, toepassingsservers, en domeincontrollers en de schijf gegevens.
- **adventureazosusesadbclust** voor de opslag van de premie van de MongoDB een laptopgeheugen van cluster-servers en hun gegevensschijven.


## <a name="virtual-network-and-subnets"></a>Virtueel netwerk en de subnetten

Omdat het virtuele netwerk niet continue verbinding met het netwerk Adventure werk cycli op gebouwen hoeft, besloten ze op een virtueel netwerk alleen-wolk.

Ze een virtueel netwerk alleen cloud-gemaakt met de volgende instellingen gebruiken de Azure portal:

- Naam: AZOS-gebruik-VN01
- Locatie: Oost-VS 2
- Netwerk van virtuele adresruimte: 10.0.0.0/8
- Eerste subnet:
    - Naam: FrontEnd
    - Adresruimte: 10.0.1.0/24
- Tweede subnet:
    - Naam: back-end
    - Adresruimte: 10.0.2.0/24


## <a name="availability-sets"></a>Beschikbaarheid van sets

Adventure Works Cycle besloten om de beschikbaarheid van alle vier de niveaus van de on line winkel onderhoudt, vier sets van beschikbaarheid:

- **azos gebruiken als webpagina** voor webservers
- **azos gebruik als app** voor de toepassingsservers
- **azos gebruiken als db** voor de servers in het cluster van MongoDB-een laptopgeheugen
- **azos gebruiken als dc** voor de domeincontrollers


## <a name="virtual-machines"></a>Virtuele machines

Adventure Works Cycle heeft besloten de volgende namen voor hun Azure VMs:

- **azos gebruik-vm web01** voor de eerste webserver
- **azos gebruik-vm web02** voor de tweede webserver
- **azos gebruik-vm app01** voor de eerste AOS-server
- **azos gebruik-vm app02** voor de tweede application server
- **azos-gebruik-vm-db01** voor de eerste MongoDB-server in het cluster
- **azos gebruik-vm db02** voor de tweede MongoDB-server in het cluster
- **azos gebruik-vm dc01** voor de eerste domeincontroller
- **azos gebruik-vm dc02** op de tweede domeincontroller

Hier is de resulterende configuratie.

![De toepassing uiteindelijk infrastructuur in Azure geïmplementeerd](./media/virtual-machines-common-infrastructure-service-guidelines/example-config.png)

Deze configuratie bevat:

- Een virtueel netwerk alleen cloud met twee subnetten (front-end en back-end)
- Twee opslag rekeningen
- Vier sets voor beschikbaarheid, één voor elke laag van de on line winkel
- De virtuele machines voor de vier niveaus
- Een externe taakverdeling instellen voor HTTPS-gebaseerd webverkeer vanaf het Internet naar de webservers
- Een interne Netwerktaakverdeling instellen voor niet-gecodeerde webverkeer van de webservers naar de application servers
- Één brongroep


## <a name="next-steps"></a>Volgende stappen

[AZURE.INCLUDE [virtual-machines-linux-infrastructure-guidelines-next-steps](../../includes/virtual-machines-linux-infrastructure-guidelines-next-steps.md)] 