<properties
    pageTitle="Active Directory Federation Services in Azure | Microsoft Azure"
    description="In dit document leert u het implementeren van AD FS in Azure voor hoge beschikbaarheid."
    keywords="implementeren van AD FS in azure, azure adfs, azure adfs, azure ad fs implementeren, implementeren van AD FS, implementeren van ad fs implementeren van AD FS in azure, adfs in azure, implementeren van AD FS in azure, adfs azure, inleiding tot AD FS, Azure, AD FS in Azure, iaas, ADFS, adfs naar azure verplaatsen"
    services="active-directory"
    documentationCenter=""
    authors="anandyadavmsft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/03/2016"
    ms.author="anandy;billmath"/>

# <a name="ad-fs-deployment-in-azure"></a>AD FS-implementatie in Azure 

AD FS biedt een vereenvoudigde en beveiligde id-Federatie en de mogelijkheden voor eenmalige aanmelding (SSO) Web. Federatie met Azure AD of O365 kunnen gebruikers verifiëren met behulp van referenties op gebouwen en toegang tot alle bronnen in de cloud. Hierdoor wordt het belangrijk om ervoor te zorgen toegang tot bronnen voor beide ruimten op een hoge beschikbaarheid AD FS-infrastructuur hebt en in de cloud. Implementeren van AD FS in Azure kunt bereiken, de hoge beschikbaarheid met minimale inspanningen vereist.
Er zijn diverse voordelen van de implementatie van AD FS in Azure, een aantal worden hieronder weergegeven:

* **Hoge beschikbaarheid** - met de kracht van Azure beschikbaar stelt, zorgt u ervoor een infrastructuur voor hoge beschikbaarheid.
* **Gemakkelijk op schaal** – meer prestaties nodig? Eenvoudig migreert naar krachtige machines met slechts enkele muisklikken in Azure
* **Cross-Geo redundantie** – met Azure Geo redundantie u kunt erop vertrouwen dat uw infrastructuur maximaal beschikbaar over de hele wereld is
* **Gemakkelijk te beheren** – met sterk vereenvoudigd beheeropties in Azure portal voor beheer van uw infrastructuur is zeer eenvoudig en probleemloos 

## <a name="design-principles"></a>Principes van databaseontwerp

![Implementatie-ontwerp](./media/active-directory-aadconnect-azure-adfs/deployment.png)

Het bovenstaande diagram toont de aanbevolen topologieën om te starten met het implementeren van uw AD FS-infrastructuur in Azure. De principes achter de verschillende onderdelen van de topologie worden hieronder weergegeven:

* **DC / AD FS-Servers**: als er minder dan 1000 gebruikers kunt u de functie van AD FS te installeren op uw domeincontrollers. Als u niet dat invloed op de prestaties van de domeincontrollers wilt of als er meer dan 1000 gebruikers, vervolgens implementeren van AD FS op afzonderlijke servers.
* **WAP-Server** – is om te implementeren, Web Application Proxy servers, zodat gebruikers de AD FS wanneer ze zich niet op het netwerk van het bedrijf ook kunnen bereiken.
* **DMZ**: The Web Application Proxy servers worden geplaatst in de DMZ en alleen TCP-/ 443 toegang is toegestaan tussen de DMZ en intern subnet.
* **Balancers laden**: hoge beschikbaarheid van AD FS en Web Application Proxy servers, zodat het beste met behulp van een interne taakverdeling voor AD FS-servers en Azure Load Balancer voor Web Application Proxy servers.
* **Beschikbaarheid Sets**: om redundantie voor uw AD FS-implementatie, is het raadzaam dat u twee of meer virtuele machines in een beschikbaarheid instellen voor soortgelijke werkbelasting groeperen. Deze configuratie zorgt ervoor dat tijdens een een gebeurtenis of niet gepland onderhoud, ten minste één virtuele machine beschikbaar zal zijn
* **Opslag rekeningen**: het wordt aanbevolen om twee accounts voor opslag. Met een account is één opslag kan leiden tot het maken van een potentieel risico en kan ertoe leiden dat de installatie niet meer beschikbaar zijn in een onwaarschijnlijk scenario waar de opslag account uitvalt. Twee opslag-accounts kunt voor elke regel veroorzaakt een opslag-account koppelen.
* **Netwerk voor scheiding**: Web Application Proxy-servers moeten worden geïmplementeerd in een afzonderlijk DMZ-netwerk. U kunt een virtueel netwerk verdelen over twee subnetten en implementeert u de toepassingsproxy Web Server (s) in een geïsoleerde subnet. U kunt gewoon het netwerk groep beveiligingsinstellingen configureren voor elk subnet en alleen vereist communicatie tussen de twee subnetten. Meer details worden gegeven per volgende implementatiescenario

##<a name="steps-to-deploy-ad-fs-in-azure"></a>Stappen voor het implementeren van AD FS in Azure

De handleiding voor het implementeren van een overzicht van de stappen in deze sectie worden vermeld de hieronder beschreven AD FS-infrastructuur in Azure.

### <a name="1-deploying-the-network"></a>1. distribueren het netwerk

Zoals hierboven beschreven, kunt u op twee subnetten maken in één virtueel netwerk anders maken twee volledig verschillende virtuele netwerken (VNet). Dit artikel zal zich richten op het implementeren van een enkele virtuele netwerk en het verdelen over twee subnetten. Dit is momenteel een eenvoudiger aanpak als twee afzonderlijke VNets een VNet VNet-gateway voor communicatie vereist.

**1.1 virtueel netwerk maken**

![Virtueel netwerk maken](./media/active-directory-aadconnect-azure-adfs/deploynetwork1.png)
    
In het portal voor Azure kunt virtueel netwerk selecteren en u implementeren het virtuele netwerk en één subnet onmiddellijk met één muisklik. INT subnet ook bepaald en is nu klaar voor VMs moet worden toegevoegd.
De volgende stap is een ander subnet toevoegen aan het netwerk, dat wil zeggen het subnet DMZ. Gewoon de DMZ-subnet maken

* Selecteer het zojuist gemaakte netwerk
* Selecteer in de eigenschappen voor subnet
* In het subnet Configuratiescherm klikt u op de knop toevoegen
* De naam en het adres ruimte subnetgegevens om te maken van het subnet te bieden

![Subnet](./media/active-directory-aadconnect-azure-adfs/deploynetwork2.png)


![Subnet DMZ](./media/active-directory-aadconnect-azure-adfs/deploynetwork3.png)

**1.2. het netwerk beveiligingsgroepen maken**

Een beveiligingsgroep netwerk (NSG) bevat een lijst met regels die toestaan of weigeren netwerkverkeer naar de VM-exemplaren in een virtueel netwerk voor lijst ACL (Access Control). NSGs kunnen worden gekoppeld aan de subnetten of afzonderlijke exemplaren van de VM op dat subnet. Wanneer een NSG gekoppeld aan een subnet is, gelden de regels van de ACL voor de VM exemplaren in dat subnet.
Voor de toepassing van deze richtsnoeren, maken we twee NSGs: één voor een intern netwerk en een DMZ. Ze worden gelabeld NSG_INT en NSG_DMZ respectievelijk.

![NSG maken](./media/active-directory-aadconnect-azure-adfs/creatensg1.png)

Nadat de NSG is gemaakt, zal er 0 0 en binnenkomend uitgaande regels. Zodra de rollen op de respectieve servers geïnstalleerd en correct functioneren zijn, kunnen de regels voor binnenkomende en uitgaande worden gemaakt volgens het gewenste beveiligingsniveau.

![NSG initialiseren](./media/active-directory-aadconnect-azure-adfs/nsgint1.png)

Nadat de NSGs zijn gemaakt, NSG_INT aan het subnet koppelen INT en NSG_DMZ met subnetmasker DMZ. Hieronder volgt een voorbeeld screenshot:

![NSG configureren](./media/active-directory-aadconnect-azure-adfs/nsgconfigure1.png)

* Klik op subnetten voor het openen van het Configuratiescherm voor subnetten
* Selecteer het subnet moet worden gekoppeld aan de NSG 

Na de configuratie er het deelvenster voor subnetten als hieronder:

![Subnetten na NSG](./media/active-directory-aadconnect-azure-adfs/nsgconfigure2.png)

**1.3. verbinding met on-premises maken**

Een verbinding met de op het bedrijf moeten we de domeincontroller (DC) in azure implementeren. Azure biedt verschillende connectiviteitsopties voor uw infrastructuur op ruimten verbinding met uw Azure-infrastructuur.

* Punt-tot-site
* Virtueel netwerk van Site naar site
* ExpressRoute

Het verdient aanbeveling ExpressRoute gebruiken. ExpressRoute kunt u particuliere verbindingen maken tussen infrastructuur die op uw locatie of in een omgeving met collocatie en Azure datacenters. ExpressRoute verbindingen gaan niet via het openbare Internet. Zij bieden meer betrouwbaarheid, hogere snelheden, lagere vertragingstijden en een hogere mate van beveiliging dan de normale verbindingen via het Internet.
Terwijl het verdient ExpressRoute gebruiken, kunt u elke verbindingsmethode meest geschikt zijn voor uw organisatie. Lees meer over ExpressRoute en de verschillende connectiviteitsopties met ExpressRoute, [ExpressRoute technisch overzicht](https://aka.ms/Azure/ExpressRoute).

### <a name="2-create-storage-accounts"></a>2. opslag accounts maken

Hoge beschikbaarheid te onderhouden en te voorkomen dat de afhankelijkheid van een enkele opslag-account, kunt u twee accounts voor opslag. De machines in elke set beschikbaarheid in twee groepen te verdelen en vervolgens elke groep een aparte opslag account toewijzen.

![Opslag-accounts maken](./media/active-directory-aadconnect-azure-adfs/storageaccount1.png)

### <a name="3-create-availability-sets"></a>3. beschikbaarheid sets maken

Voor elke rol (DC/AD FS en WAP) beschikbaarheid-sets met 2 machines ten minste te maken. Hierdoor kan een hogere mate van beschikbaarheid voor elke rol. Tijdens het maken van de beschikbaarheid wordt ingesteld, is het van essentieel belang om te beslissen over het volgende:
* **Fout met betrekking tot domeinen**: virtuele machines in de fout met betrekking tot hetzelfde domein delen dezelfde energiebron en fysieke netwerkswitch. Een minimum van 2 fout met betrekking tot domeinen worden aanbevolen. De standaardwaarde is 3 en kunt u deze zo voor de toepassing van deze distributie
* **Domeinen werken**: computers die deel uitmaken van hetzelfde domein update tegelijk opnieuw worden gestart tijdens een update. Wilt u minimaal 2 update domeinen hebben. De standaardwaarde is 5 en kunt u deze zo voor de toepassing van deze distributie

![Beschikbaarheid van sets](./media/active-directory-aadconnect-azure-adfs/availabilityset1.png)

De volgende beschikbaarheid sets maken

| Beschikbaarheid instellen | Rol | Fout met betrekking tot domeinen | Domeinen werken |
|:----------------:|:----:|:-----------:|:-----------|
| contosodcset | DC/ADFS | 3 | 5 |
| contosowapset | WAP | 3 | 5 |

### <a name="4--deploy-virtual-machines"></a>4. implementeren van virtuele machines
De volgende stap is het implementeren van virtuele machines die als host voor de verschillende rollen in uw infrastructuur fungeert. Een minimum van twee machines in elke set beschikbaarheid worden aanbevolen. Zes virtuele machines voor de eenvoudige installatie maken.

| Machine | Rol | Subnet | Beschikbaarheid instellen | Opslag account | IP-adres |
|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|
|contosodc1|DC/ADFS|INT|contosodcset|contososac1|Statische|
|contosodc2|DC/ADFS|INT|contosodcset|contososac2|Statische|
|contosowap1|WAP|DMZ|contosowapset|contososac1|Statische|
|contosowap2|WAP|DMZ|contosowapset|contososac2|Statische|

Zoals u wellicht hebt opgemerkt, is er geen NSG opgegeven. Dit komt doordat de azure kunt u NSG op subnetniveau. Vervolgens kunt u de computer netwerkverkeer bepalen met behulp van de afzonderlijke NSG op het subnet of anders het NIC-object gekoppeld. Lees meer over [Wat is een Network Security Group (NSG)](https://aka.ms/Azure/NSG).
Statisch IP-adres wordt aanbevolen als u de DNS-server beheert. U kunt DNS Azure gebruiken en in plaats daarvan in de DNS-records voor uw domein, verwijzen naar de nieuwe machines door hun FQDN Azure.
De virtuele machine-deelvenster er als hieronder nadat de implementatie is voltooid:

![Virtuele Machines geïmplementeerd](./media/active-directory-aadconnect-azure-adfs/virtualmachinesdeployed_noadfs.png)

### <a name="5-configuring-the-domain-controller--ad-fs-servers"></a>5. de domeincontroller configureren / AD FS-servers
 Om alle inkomende verzoeken worden geverifieerd, moet AD FS Neem contact op met de domeincontroller. Het is raadzaam de dure reis van Azure om op te slaan voor bedrijfsruimten DC voor verificatie, implementatie van een replica van de domeincontroller in Azure. Bereiken van hoge beschikbaarheid, wordt het aanbevolen om een set beschikbaarheid van minimale 2-domeincontrollers te maken.

|Domeincontroller|Rol|Opslag account|
|:-----:|:-----:|:-----:|
|contosodc1|Replica|contososac1|
|contosodc2|Replica|contososac2|

* Bevordering van de twee servers als replicadomeincontrollers met DNS
* De AD FS-servers configureren door het installeren van de functie van AD FS met server manager.

###<a name="6---deploying-internal-load-balancer-ilb"></a>6. implementeren interne taakverdeling (ILB)

**6.1. het ILB maken**

Als u wilt implementeren op een ILB, toevoegen Schakel Netwerktaakverdeling in Azure portal en klik op (+)
>[AZURE.NOTE] Als u **Netwerktaakverdeling** niet in het menu wordt weergegeven, klikt u op **Bladeren** in de linkerbenedenhoek van de portal en schuiven totdat er **Netwerktaakverdeling**.  Klik vervolgens op de gele ster toe te voegen aan het menu. Selecteer nu het nieuwe load balancer pictogram om het deelvenster openen om te beginnen met de configuratie van de taakverdeling.

![Taakverdeling bladeren](./media/active-directory-aadconnect-azure-adfs/browseloadbalancer.png)

* **Naam**: een passende naam geven aan de taakverdeling
* **Schema**: aangezien deze taakverdeling wordt voor de AD FS-servers worden geplaatst en is bedoeld voor het interne netwerkverbindingen "Internal" alleen selecteren
* **Virtueel netwerk**: Kies het virtuele netwerk waar u uw AD FS implementeert
* **Subnetmasker**: Kies hier de interne subnet
* **Toewijzing van IP-adres**: dynamische

![Interne taakverdeling](./media/active-directory-aadconnect-azure-adfs/ilbdeployment1.png)
 
Nadat u op maken en het ILB wordt geïmplementeerd, moet u deze zien in de lijst van netwerktaakverdeling:

![Netwerktaakverdeling na het ILB](./media/active-directory-aadconnect-azure-adfs/ilbdeployment2.png)
 
Volgende stap is het configureren van de back-end-toepassingen en de backend-sonde.

**6.2. ILB back-end-toepassingen configureren**

Selecteer de zojuist gemaakte ILB in het deelvenster met netwerktaakverdeling. Het instellingenvenster wordt geopend. 
1.  Back-end-toepassingen selecteert in het instellingenpaneel
2.  In het deelvenster toevoegen backend van toepassingen, klikt u op toevoegen virtuele machine
3.  U krijgt een paneel u de beschikbaarheid van set kunt
4.  De beschikbaarheid van AD FS set kiezen

![ILB back-end-toepassingen configureren](./media/active-directory-aadconnect-azure-adfs/ilbdeployment3.png)
 
**6.3. sonde configureren**

Selecteer in het paneel ILB instellingen sondes.
1.  Klik op toevoegen
2.  De bijzonderheden van de sonde een. **Naam**: naam b Probe. **Protocol**: TCP-c. **Poort**: d 443 (HTTPS). **Interval**: 5 (standaardwaarde) – Dit is het interval waarmee ILB computers in de groep met back-end e zal onderzoeken. **Beschadigde drempel limiet**: 2 (standaard val ue) – Dit is de drempel van de sonde opeenvolgende fouten waarna ILB een machine in de back-end-toepassingen wordt gedeclareerd niet meer reageert en stop het verkeer te verzenden.

![ILB sonde configureren](./media/active-directory-aadconnect-azure-adfs/ilbdeployment4.png)
 
**6.4. taakverdeling regels maken**

Om een goed evenwicht van het verkeer, moet het ILB zijn geconfigureerd met regels voor de taakverdeling. Om te kunnen maken van een regel voor taakverdeling 
1.  Selecteer de regel uit het instellingenvenster van het ILB taakverdeling
2.  Klik op toevoegen in het deelvenster regel voor taakverdeling
3.  In de regel paneel voor taakverdeling van toevoegen een. **Naam**: Geef een naam voor de regel b. **Protocol**: Selecteer TCP-c. **Poort**: 443 d. **Back-end-poort**: 443 e. **Back-end-toepassingen**: Selecteer de toepassingen die u hebt gemaakt voor de AD FS-cluster eerder f. **Sonde**: Selecteer de sonde eerder hebt gemaakt voor de AD FS-servers

![ILB regels voor netwerktaakverdeling configureren](./media/active-directory-aadconnect-azure-adfs/ilbdeployment5.png)

**6.5. DNS bijwerken met ILB**

Ga naar de DNS-server en maak een CNAME voor het ILB. De CNAME moet zijn voor de federation-service met het IP-adres het IP-adres van het ILB aan te wijzen. Bijvoorbeeld als het ILB DIP-adres 10.3.0.8 en de federation-service is geïnstalleerd, is de fs.contoso.com, maakt u een CNAME voor fs.contoso.com 10.3.0.8 aan te wijzen.
Hierdoor weet u zeker dat alle communicatie met betrekking tot het einde van de fs.contoso.com aan het ILB boven en op de juiste wijze worden doorgestuurd.

###<a name="7---configuring-the-web-application-proxy-server"></a>7. configureren de Web Application Proxy-server

**7.1. configureren de Web Application Proxy servers voor het bereiken van AD FS-servers**

Om ervoor te zorgen dat Web Application Proxy servers kunnen bereiken van de AD FS-servers achter het ILB zijn, moet u een record maken in de %systemroot%\system32\drivers\etc\hosts voor het ILB. Houd er rekening mee dat de geldige naam (DN) de federation service-naam, bijvoorbeeld fs.contoso.com moet. En de IP-vermelding van het ILB van IP-adres (10.3.0.8 zoals in het voorbeeld).

**7.2. installeren de rol van de Web-toepassingsproxy**

Nadat u ervoor zorgen dat Web Application Proxy servers kunnen de AD FS-servers achter ILB bereiken, kunt u vervolgens de Web Application Proxy servers installeren. Web Application Proxy servers niet worden toegevoegd aan het domein. De webproxy toepassing rollen op de twee Web Application Proxy servers installeren door de functie Externe toegang. De server manager helpt u om de installatie van de WAP.
Lees [de Web Application Proxy-Server installeren en configureren](https://technet.microsoft.com/library/dn383662.aspx)voor meer informatie over het implementeren van de WAP.

###<a name="8---deploying-the-internet-facing-public-load-balancer"></a>8. implementatie van de internetverbinding (openbare) Load Balancer

**8.1. taakverdeling (openbare) internetverbinding maken**
 
Schakel Netwerktaakverdeling in de Azure portal en klik op toevoegen. Voer de volgende gegevens in het deelvenster maken load balancer
1. **Naam**: naam voor de verdeling van de belasting
2. **Schema**: openbare – deze optie zorgt ervoor dat Azure deze taakverdeling moet een openbaar adres.
3. **IP-adres**: een nieuw IP-adres (dynamisch)

![Internet-gerichte taakverdeling](./media/active-directory-aadconnect-azure-adfs/elbdeployment1.png)

Na de installatie wordt de taakverdeling in de lijst Load balancers weergegeven.

![Load balancer-lijst](./media/active-directory-aadconnect-azure-adfs/elbdeployment2.png)
 
**8.2. een DNS-label op het openbare IP-adres toewijzen**

Klik op de vermelding van de zojuist gemaakte load balancer in het deelvenster Load balancers om het deelvenster voor configuratie. Volg de onderstaande stappen te volgen voor het configureren van de DNS-naam voor het openbare IP:
1.  Klik op het openbare IP-adres. Hiermee opent u het deelvenster voor en de instellingen van het openbare IP-
2.  Klik op configuratie
3.  Een DNS-label bevatten. Hierdoor worden de openbare DNS-label die u vanaf elke gewenste locatie, bijvoorbeeld contosofs.westus.cloudapp.azure.com openen kunt. U kunt een vermelding toevoegen in de externe DNS-server voor de federation-service (zoals fs.contoso.com) die wordt omgezet in de DNS-label van de externe-taakverdeling (contosofs.westus.cloudapp.azure.com).

![Taakverdeling internetverbinding configureren](./media/active-directory-aadconnect-azure-adfs/elbdeployment3.png) 

![Taakverdeling (DNS) van de internetverbinding configureren](./media/active-directory-aadconnect-azure-adfs/elbdeployment4.png)

**8.3. back-end-toepassingen configureren voor taakverdeling Internet Facing (openbaar)** 

Volg de stappen die in het maken van de interne taakverdeling configureren van de back-end-toepassingen voor Internet Facing (openbare) Load Balancer als de beschikbaarheid voor de WAP-server instellen. Bijvoorbeeld contosowapset.

![Back-end-toepassingen van Internet Facing taakverdeling configureren](./media/active-directory-aadconnect-azure-adfs/elbdeployment5.png)
 
**8.4. sonde configureren**

Volg de stappen die in de interne taakverdeling configureren van de sonde voor de back-end-toepassingen van WAP-servers configureren.

![Sonde van Internet Facing taakverdeling configureren](./media/active-directory-aadconnect-azure-adfs/elbdeployment6.png)
 
**8.5. taakverdeling (s) maken**

Volg de stappen die in het ILB voor het configureren van de regel voor taakverdeling voor TCP-443.

![Regels van Internet Facing Load Balancer voor netwerktaakverdeling configureren](./media/active-directory-aadconnect-azure-adfs/elbdeployment7.png)
 
###<a name="9---securing-the-network"></a>9. beveiligen het netwerk

**9.1. beveiligen de interne subnet**

Over het algemeen moet u de volgende regels voor het beveiligen van uw interne subnet (in het onderstaande) efficiënt

|Regel|Beschrijving|Stroom|
|:----|:----|:------:|
|AllowHTTPSFromDMZ| De HTTPS-communicatie van DMZ toestaan | Inkomend |
|DenyInternetOutbound| Geen toegang tot internet | Uitgaande |

![Toegangsregels INT (inkomend)](./media/active-directory-aadconnect-azure-adfs/nsg_int.png)

[opmerking]: <> (![toegangsregels INT (inkomend)](./media/active-directory-aadconnect-azure-adfs/nsgintinbound.png)) [opmerking]: <> (![toegangsregels INT (uitgaand)](./media/active-directory-aadconnect-azure-adfs/nsgintoutbound.png))
 
**9.2. beveiligen het subnet DMZ**

|Regel|Beschrijving|Stroom|
|:----|:----|:------:|
|AllowHTTPSFromInternet| HTTPS vanaf internet naar de DMZ toestaan | Inkomend|
|DenyInternetOutbound|  Alles behalve HTTPS met internet wordt geblokkeerd. | Uitgaande |

![Toegangsregels EXT (inkomend)](./media/active-directory-aadconnect-azure-adfs/nsg_dmz.png)

[opmerking]: <> (![EXT toegangsregels (inkomend)](./media/active-directory-aadconnect-azure-adfs/nsgdmzinbound.png)) [opmerking]: <> (![EXT toegangsregels (uitgaand)](./media/active-directory-aadconnect-azure-adfs/nsgdmzoutbound.png))

>[AZURE.NOTE] Als clientgebruiker authentication Certificate (clientTLS-verificatie met behulp van X509 certificaten) nodig is, en vervolgens AD FS TCP vereist poort 49443 worden ingeschakeld voor binnenkomende toegang.

###<a name="10--test-the-ad-fs-sign-in"></a>10. test de AD FS-aanmelden

De eenvoudigste manier is voor het testen van AD FS op de pagina IdpInitiatedSignon.aspx. Om te kunnen doen, is vereist voor de IdpInitiatedSignOn van de AD FS-eigenschappen. Volg de onderstaande stappen om te controleren of de installatie van AD FS
1.  Voer de onderstaande cmdlet op de AD FS-server met PowerShell, instellen op ingeschakeld.
    $True set-AdfsProperties - EnableIdPInitiatedSignonPage 
2.  Vanaf een externe computer toegang https://adfs.thecloudadvocate.com/adfs/ls/IdpInitiatedSignon.aspx  
3.  U ziet de AD FS-pagina, zoals hieronder:

![Login pagina](./media/active-directory-aadconnect-azure-adfs/test1.png)

Bij succesvolle aanmelden krijgt deze u een bericht zoals hieronder wordt weergegeven:

![Test geslaagd](./media/active-directory-aadconnect-azure-adfs/test2.png)

## <a name="template-for-deploying-ad-fs-in-azure"></a>Sjabloon voor het implementeren van AD FS in Azure

De sjabloon implementeert een machine 6 setup, 2 voor domeincontrollers, AD FS en WAP.

[AD FS in de sjabloon Azure-implementatie](https://github.com/paulomarquesc/adfs-6vms-regular-template-based)

U kunt een bestaande virtuele netwerk gebruiken of een nieuwe VNET bij het implementeren van deze sjabloon maken. De verschillende parameters beschikbaar voor het aanpassen van de implementatie worden hieronder weergegeven met de beschrijving van het gebruik van de parameter in het implementatieproces. 

| Parameter | Beschrijving |
|:--------|:-----|
|Locatie| De regio voor de implementatie van de bronnen in bijvoorbeeld Oost ons. |
|StorageAccountType| Het type van de opslag-Account gemaakt|
|VirtualNetworkUsage| Geeft aan of een nieuw virtueel netwerk wordt gemaakt of gebruik een bestaande|
|VirtualNetworkName| De naam van het virtuele netwerk te maken, verplicht is over het gebruik van bestaande of nieuwe virtueel netwerk|
|VirtualNetworkResourceGroupName| Hiermee geeft u de naam van de resourcegroep waarop de bestaande virtuele netwerk bevindt. Als u een bestaande virtuele netwerk, deze worden omgezet in een verplichte parameter zodat de implementatie van de ID van het bestaande virtuele netwerk kunt zoeken|
|VirtualNetworkAddressRange| Het adresbereik van de nieuwe VNET, verplicht als u een nieuw virtueel netwerk maken|
|InternalSubnetName| De naam van het interne subnet verplicht op beide gebruiksopties virtueel netwerk (nieuwe of bestaande)|
|InternalSubnetAddressRange| Het adresbereik van het subnet interne, waarin de domeincontrollers en AD FS-servers, verplicht als u een nieuw virtueel netwerk maken.|
|DMZSubnetAddressRange| Het adresbereik van het subnet dmz, waarin de Windows application proxyservers, verplicht als u een nieuw virtueel netwerk maken.|
|DMZSubnetName| De naam van het interne subnet verplicht op beide gebruiksopties virtueel netwerk (nieuwe of bestaande). |
|ADDC01NICIPAddress| Het interne IP-adres van de eerste domeincontroller dit IP-adres statisch worden toegewezen aan de domeincontroller en moet een geldig IP-adres binnen het subnet van de interne|
|ADDC02NICIPAddress| Het interne IP-adres van de tweede domeincontroller dit IP-adres statisch worden toegewezen aan de domeincontroller en moet een geldig IP-adres binnen het subnet van de interne|
|ADFS01NICIPAddress| Het interne IP-adres van de eerste AD FS-server dit IP-adres statisch worden toegewezen aan de AD FS-server en moet een geldig IP-adres binnen het subnet van de interne|
|ADFS02NICIPAddress| Het interne IP-adres van de tweede AD FS-server dit IP-adres statisch worden toegewezen aan de AD FS-server en moet een geldig IP-adres binnen het subnet van de interne|
|WAP01NICIPAddress| Het interne IP-adres van de eerste WAP-server dit IP-adres statisch worden toegewezen aan de WAP-server en moet een geldig IP-adres binnen het subnet DMZ|
|WAP02NICIPAddress| Het interne IP-adres van de tweede WAP-server dit IP-adres statisch worden toegewezen aan de WAP-server en moet een geldig IP-adres binnen het subnet DMZ|
|ADFSLoadBalancerPrivateIPAddress| Het interne IP-adres van de AD FS-taakverdeling, dit IP-adres statisch worden toegewezen aan de taakverdeling en moet een geldig IP-adres binnen het subnet van de interne|
|ADDCVMNamePrefix| Voorvoegsel van virtuele Machine voor domeincontrollers|
|ADFSVMNamePrefix| Voorvoegsel van virtuele Machine voor AD FS-servers|
|WAPVMNamePrefix| Voorvoegsel van virtuele Machine voor WAP-servers|
|ADDCVMSize| De vm-grootte van de domeincontrollers|
|ADFSVMSize| De vm-grootte van de AD FS-servers|
|WAPVMSize| De vm-grootte van de WAP-servers|
|AdminUserName| De naam van de lokale beheerder van de virtuele machines|
|AdminPassword| Het wachtwoord voor de lokale Administrator-account van de virtuele machines|

## <a name="additional-resources"></a>Aanvullende bronnen
* [Beschikbaarheid van Sets](https://aka.ms/Azure/Availability ) 
* [Azure Load Balancer](https://aka.ms/Azure/ILB)
* [Interne taakverdeling](https://aka.ms/Azure/ILB/Internal)
* [Internet Facing Load Balancer](https://aka.ms/Azure/ILB/Internet)
* [Opslag-Accounts](https://aka.ms/Azure/Storage )
* [Azure virtuele netwerken](https://aka.ms/Azure/VNet)
* [AD FS en koppelingen naar het Web Application Proxy](http://aka.ms/ADFSLinks) 

## <a name="next-steps"></a>Volgende stappen

* [Integratie van uw identiteiten op ruimten met Azure Active Directory](active-directory-aadconnect.md)
* [Configureren en beheren van uw AD FS met Azure AD verbinden](active-directory-aadconnectfed-whatis.md)
* [Hoge beschikbaarheid cross-geografische AD FS-implementatie in Azure Azure verkeer Manager](active-directory-adfs-in-azure-with-azure-traffic-manager.md)




