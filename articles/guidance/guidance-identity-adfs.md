<properties
   pageTitle="Het implementeren van AD FS in Azure | Microsoft Azure"
   description="Het implementeren van een beveiligde hybride netwerkarchitectuur met verificatie van Active Directory Federation Service in Azure."
   services="guidance,vpn-gateway,expressroute,load-balancer,virtual-network,active-directory"
   documentationCenter="na"
   authors="telmosampaio"
   manager="christb"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/13/2016"
   ms.author="telmos"/>

# <a name="implementing-active-directory-federation-services-adfs-in-azure"></a>Het implementeren van Active Directory Federation Services (ADFS) in Azure

[AZURE.INCLUDE [pnp-RA-branding](../../includes/guidance-pnp-header-include.md)]

Dit artikel bevat verschillende aanbevelingen voor de implementatie van een beveiligde hybride netwerk die uw netwerk op ruimten naar Azure uitbreidt en die gebruikmaakt van [Active Directory Federation Services (ADFS)] [ active-directory-federation-services] federatieve verificatie en machtiging uitvoeren voor onderdelen die worden uitgevoerd in de cloud. Deze architectuur breidt de structuur die wordt beschreven door [Active Directory uitbreiden naar Azure][implementing-active-directory].

> [AZURE.NOTE] Azure heeft twee verschillende modellen: [Resource Manager] [ resource-manager-overview] en klassiek. Deze referentiearchitectuur gebruikt resourcemanager, wordt aangeraden voor nieuwe implementaties.

ADFS kan op locatie worden uitgevoerd kan, maar in een hybride scenario waar toepassingen zich in Azure efficiënter voor het implementeren van deze functionaliteit in de cloud. Typische use-cases voor deze architectuur zijn:

- Hybride toepassingen waar werklasten uitvoeren deels op-lokalen en deels in Azure.

- Oplossingen die gebruikmaken van federatieve vergunning partnerorganisaties webtoepassingen worden blootgesteld.

- Systemen die toegang van buiten de firewall van de organisatie webbrowsers ondersteunen.

- Systemen waarmee gebruikers toegang hebben tot webtoepassingen door verbinding te maken naar geautoriseerde externe apparaten, zoals externe computers, laptops en andere mobiele apparaten. 

Zie [Overzicht van Active Directory Federation Services]voor meer informatie over de werking van AD FS,[active-directory-federation-services-overview]. Bovendien artikel [AD FS-implementatie in Azure] [ adfs-intro] bevat een gedetailleerde stapsgewijze inleiding op het implementeren van AD FS in Azure.

## <a name="architecture-diagram"></a>Architectuurdiagram

In het volgende diagram worden de belangrijke onderdelen van deze architectuur (*Klik om in te zoomen*). Lees voor meer informatie over elk element dat niet gerelateerd aan AD FS [implementeert een beveiligde hybride netwerkarchitectuur in Azure][implementing-a-secure-hybrid-network-architecture], [uitvoering van een beveiligde hybride netwerkarchitectuur met toegang tot het Internet in Azure][implementing-a-secure-hybrid-network-architecture-with-internet-access], en de [uitvoering van een beveiligde hybride netwerkarchitectuur met identiteiten in Azure Active Directory][implementing-active-directory]:

[! [0]][0]

>[AZURE.NOTE] In dit diagram ziet u de volgende gevallen van gebruik:
>
>- Toepassingscode uitgevoerd binnen een andere organisatie toegang heeft tot een webtoepassing opgenomen binnen uw VNet Azure.
>
>- Een externe, de geregistreerde gebruiker (met de referenties die zijn opgeslagen in ADDS) een webtoepassing opgenomen binnen uw VNet Azure.
>
>- Een gebruiker verbinding maakt met uw VNet door een geautoriseerde apparaat en een webtoepassing opgenomen binnen uw VNet Azure.
>
>Ook deze architectuur ligt de nadruk op passieve Federatie, waar de federation-servers hoe en wanneer bepalen een gebruiker te verifiëren. De gebruiker wordt verwacht inloggegevens opgeven wanneer een toepassing wordt gestart. Dit is het meest worden gebruikt door webbrowsers mechanisme en bestaat uit een protocol dat wordt de browser omgeleid naar een site waar de gebruikers hun referenties kan bieden. AD FS ondersteunt ook actieve federation waarbij een toepassing neemt de verantwoordelijkheid voor het verstrekken van referenties zonder verdere tussenkomst van de gebruiker, maar in dit geval valt buiten het bereik van deze architectuur.

- **Subnet toevoegen.** Het telt servers bevinden zich in hun eigen subnet. NSG regels beschermen de ADDS-servers en bieden een firewall tegen verkeer uit onverwachte bronnen.

- **Servers toevoegen.** Dit zijn domeincontrollers als VMs in de cloud. Deze servers kunnen bieden verificatie van lokale identiteiten in het domein.

- **ADFS-subnet.** De AD FS-servers kunnen worden gevonden in hun eigen subnet, NSG-regels die fungeert als een firewall.

- **AD FS-servers.** De AD FS-servers bieden federatieve autorisatie en verificatie. In deze architectuur uitvoeren zij de volgende taken:

    - Ze kunnen beveiligingstokens met claims door een federatieserver partner uitgevoerd in opdracht van een gebruiker ontvangen. AD FS kunt controleren of deze tokens geldig zijn voordat de claims naar uitgevoerd in Azure-webtoepassing wordt doorgegeven. De corporate webtoepassing (in Azure) kunt u deze claims gebruiken aanvragen toe te staan. In dit scenario wordt de corporate webtoepassing is het *vertrouwen van de partij*en het is de verantwoordelijkheid van de federation-server van de partner om claims die worden begrepen door de corporate-webtoepassing. De partner federation-servers worden aangeduid als *accountpartners* omdat het indienen van aanvragen voor geverifieerde accounts in de andere organisatie. De AD FS-servers worden *Bronpartners* genoemd omdat ze toegang tot bronnen (in dit geval webtoepassingen bieden).

    - Kunnen worden geverifieerd (via toevoegen en de [Active Directory-apparaat registratieservice][ADDRS]) en binnenkomende aanvragen van externe gebruikers met een webbrowser of een apparaat dat toegang tot uw zakelijke webtoepassingen te autoriseren. 

    De AD FS-servers zijn geconfigureerd als een bedrijf via een Azure taakverdeling. Deze structuur helpt beschikbaarheid en schaalbaarheid te verbeteren. Let er ook op dat de AD FS-servers rechtstreeks met het Internet niet beschikbaar, maar alle Internet-verkeer wordt gefilterd door AD FS web application proxyservers en een DMZ.

- **ADFS proxy subnet.** De AD FS-proxyservers kunnen worden opgenomen in hun eigen subnet, NSG-regels die bescherming bieden. De servers in dit subnet worden blootgesteld aan het Internet via een reeks virtuele netwerkapparaten die een firewall tussen uw Azure virtueel netwerk en het Internet bieden.

- **AD FS-toepassingen (WAP) webproxyservers.** Deze computers fungeren als AD FS-servers voor binnenkomende verzoeken van partnerorganisaties en externe apparaten. De WAP-servers fungeren als een filter, afscherming van de AD FS-servers uit directe toegang vanaf het openbare Internet. Net als bij de AD FS-servers implementeren van het draadloze Toegangspunt servers in een farm met taakverdeling kunt u grotere beschikbaarheid en schaalbaarheid dan het implementeren van een collectie van zelfstandige servers.

    >[AZURE.NOTE] Zie voor gedetailleerde informatie over het installeren van WAP-servers [installeren en configureren van de Web Application Proxy Server][install_and_configure_the_web_application_proxy_server]

- **Organisatie van de bronpartner.** Dit is een voorbeeld van de partnerorganisatie, die wordt uitgevoerd een webtoepassing die vraagt om toegang tot de webtoepassing die wordt uitgevoerd in Azure. De federation-server van de andere organisatie aanvragen lokaal wordt geverifieerd en claims naar AD FS in Azure uitgevoerd met beveiligingstokens indient. AD FS in Azure worden de beveiligingstokens gevalideerd en als ze geldig zijn de vorderingen in de webtoepassing toe te staan in Azure uitgevoerd wordt doorgegeven.

    >[AZURE.NOTE] U kunt ook een VPN-tunnel met Azure Gateway voor directe toegang tot AD FS voor vertrouwde partners. Aanvragen ontvangen van deze partners passeren de WAP-servers niet.

## <a name="recommendations"></a>Aanbevelingen

Deze sectie bevat een overzicht van aanbevelingen voor de implementatie van AD FS in Azure, die betrekking hebben op:

- VM-aanbevelingen.

- Aanbevelingen voor netwerken.

- Aanbevelingen van de beschikbaarheid.

- Aanbevelingen voor beveiliging.

- Aanbevelingen voor AD FS-installatie.

- ADFS-Trust aanbevelingen.

### <a name="vm-recommendations"></a>VM-aanbevelingen

VMs maken met voldoende middelen voor het verwerken van de verwachte hoeveelheid verkeer. De grootte van de bestaande machines die als host fungeert voor AD FS in gebouwen als uitgangspunt gebruiken. Het Resourcegebruik controleren. U kunt het formaat van het VMs en schalen naar beneden als ze te groot zijn.

Volg de aanbevelingen die worden vermeld in het [uitvoeren van een Windows VM in Azure][vm-recommendations].

### <a name="networking-recommendations"></a>Aanbevelingen voor netwerken

De netwerkinterface configureren voor elk van de VMs ADFS- en WAP-servers met statische particuliere IP-adressen die als host fungeert.

Geeft de ADFS VMs openbare IP-adressen. Zie [Security considerations]voor meer informatie[security-considerations].

Stel het IP-adres van de voorkeurs- en secundaire DNS-servers voor de netwerkinterfaces voor elke AD FS en WAP VM te verwijzen naar de VOEGT VMs (die moet worden waarop DNS wordt uitgevoerd). Deze stap is noodzakelijk om elke VM aan het domein kan inschakelen.

### <a name="availability-recommendations"></a>Beschikbaarheid van aanbevelingen

Maak een AD FS-serverfarm met ten minste twee servers te verhogen van de beschikbaarheid van de service van ADFS.

Opslag van verschillende rekeningen gebruikt voor elk ADFS VM in de farm. Deze aanpak zorgt ervoor dat een storing in een account met één opslag geen volledige farm niet toegankelijk maakt.

Beschikbaarheid van afzonderlijke Azure sets maken voor de AD FS en WAP VMs. Zorg ervoor dat er ten minste twee VMs in elke set. Elke set beschikbaarheid moet ten minste twee update domeinen en domeinen met twee veroorzaakt hebben.

De netwerktaakverdeling voor de AD FS-VMs en WAP VMs als volgt configureren:

- Gebruik een Azure taakverdeling voor externe toegang tot de WAP VMs en een interne taakverdeling de belasting over de AD FS-servers in de farm ADFS verdelen.

- Alleen verkeer op poort 443 (HTTPS) naar de WAP AD FS-servers doorgegeven.

- Een statisch IP-adres voor de taakverdeling bieden.

- Maak een sonde met gezondheid met behulp van het TCP-protocol in plaats van HTTPS. U kunt de opdracht ping poort 443 om te controleren of een AD FS-server functioneert.

    >[AZURE.NOTE] AD FS-servers Server naam aanduiding (SNI) protocol, dus probeert te zoeken met behulp van een HTTPS-eindpunt van de load balancer niet gebruiken.

- Een DNS- *A* -record toevoegen aan het domein voor de taakverdeling van AD FS. 

    Geef het IP-adres van de taakverdeling en hieraan een naam in het domein (bijvoorbeeld adfs.contoso.com). Dit is de naam waarmee clients en servers WAP toegang krijgen de AD FS-serverfarm tot.

### <a name="security-recommendations"></a>Aanbevelingen voor beveiliging

Voorkomen dat de rechtstreekse blootstelling van de AD FS-servers met het Internet. AD FS-servers zijn computers deel uitmaakt van een domein met volledige vergunning voor het verlenen van beveiligingstokens. Als een AD FS-server is ingebroken, kan een kwaadwillende gebruiker tokens voor volledige toegang verlenen alle webtoepassingen en federatieservers die worden beveiligd met AD FS. Als uw systeem van aanvragen van externe gebruikers die niet noodzakelijkerwijs van vertrouwde partnersites verbinden verwerken moet, kunt u de WAP servers gebruiken om deze aanvragen te verwerken. Zie voor meer informatie, [waar een Federation-serverproxy][where-to-place-an-fs-proxy].

Plaats AD FS-servers en servers in verschillende subnetten met een eigen firewall WAP. NSG-regels kunt u firewall-regels definiëren. Als u een ruimere bescherming nodig kunt u een extra beveiliging omtrek rond servers implementeren met behulp van een paar van de subnetten en NVAs, zoals beschreven door de [uitvoering van een beveiligde hybride netwerkarchitectuur met toegang tot het Internet in Azure]document[implementing-a-secure-hybrid-network-architecture-with-internet-access]. Houd er rekening mee dat alle firewalls moeten toestaan dat verkeer op poort 443 (HTTPS).

Directe login toegang tot het AD FS- en WAP-servers beperken. Alleen personeel van DevOps moet verbinding kunnen maken.

De WAP-servers niet toevoegen aan het domein.

### <a name="adfs-installation-recommendations"></a>Aanbevelingen voor AD FS-installatie

Het [implementeren van een federatieserverfarm] artikel[ Deploying_a_federation_server_farm] vindt u gedetailleerde instructies voor het installeren en configureren van AD FS. De volgende taken uitvoeren voordat u de eerste server die AD FS in de farm configureren:

1. Vraag een openbaar vertrouwde certificaat voor het uitvoeren van server-verificatie. De *objectnaam* mag de naam waarmee clients toegang krijgen de federation-service tot. Dit is de DNS-naam geregistreerd voor de verdeling van de belasting, bijvoorbeeld *adfs.contoso.com* (gebruik geen namen met jokertekens, zoals **. contoso.com*, om veiligheidsredenen). Gebruik hetzelfde certificaat op alle VMs voor AD FS-server. U kunt een certificaat aanschaffen bij een vertrouwde certificeringsinstantie, maar als uw organisatie gebruikmaakt van Active Directory Certificate Services kunt u uw eigen. 

    De *alternatieve naam voor onderwerp* wordt gebruikt door de DRS om toegang te krijgen naar externe apparaten. Dit moet het formulier *enterpriseregistration.contoso.com*.

    Zie voor meer informatie [verkrijgen en configureren van een SSL-certificaat voor AD FS][adfs_certificates].

2. Genereer een nieuwe hoofdsleutel voor het Key Distribution Service op de domeincontroller. Stel de effectieve tijd wordt de huidige tijd min 10 uur (deze configuratie wordt beperkt de vertraging die optreden kan in de distributie en het synchroniseren van sleutels in het domein). Deze stap is noodzakelijk voor het maken van de groep-serviceaccount die worden gebruikt voor het uitvoeren van de service van ADFS. De volgende Powershell-opdracht ziet u een voorbeeld van hoe u dit doet:

    ```powershell
    Add-KdsRootKey -EffectiveTime (Get-Date).AddHours(-10)
    ```

3. Elke AD FS-server VM toevoegen aan het domein.

>[AZURE.NOTE] AD FS installeren, moet de domeincontroller met de PDC-emulator FSMO-functie voor het domein zijn uitgevoerd en is toegankelijk via de VMs ADFS.

### <a name="adfs-trust-recommendations"></a>ADFS-Trust aanbevelingen

Tot stand brengen tussen uw AD FS-installatie en van eventuele partnerorganisaties de federation-servers, federation-vertrouwensrelatie. Configureren van eventuele claims filteren en toewijzing vereist. Dit proces is vereist:

- DevOps medewerkers **op de organisatie van de bronpartner** een gebruikmakende partij vertrouwen voor de webtoepassingen die toegankelijk is via uw AD FS-servers toevoegen.

- DevOps medewerkers **in uw organisatie** vertrouwen zodat uw AD FS-servers de claims die partnerorganisaties vertrouwen claims provider configureren.

- DevOps medewerkers **in uw organisatie** voor het configureren van AD FS om te slagen voor de vorderingen bij de webtoepassingen van uw organisatie.

    Zie voor meer informatie, [Federation-vertrouwensrelatie tot stand brengen van][establishing-federation-trust].

Publiceren van webtoepassingen van uw organisatie en deze beschikbaar maken voor externe partners met behulp van vooraf-verificatie via de WAP-servers. Zie voor meer informatie, [toepassingen publiceren met behulp van AD FS vooraf-verificatie][publish_applications_using_AD_FS_preauthentication]

Houd er rekening mee dat AD FS token transformatie en uitdiepen ondersteunt. Azure Active Directory beschikt niet over deze functie. Met AD FS, bij het instellen van de vertrouwensrelaties kunt u:

- Claim transformaties voor autorisatieregels configureren. Bijvoorbeeld, wijst u beveiliging van een vertegenwoordiging door een partnerorganisatie voor niet-Microsoft-aan een item dat hiermee in uw organisatie machtigen kunt wordt gebruikt.

- Vorderingen van de ene indeling naar de andere transformeren. Bijvoorbeeld: u kunt toewijzen van SAML 2.0 aan SAML 1.1 als de toepassing alleen vorderingen SAML 1.1 ondersteunt. 

## <a name="availability-considerations"></a>Overwegingen met betrekking tot beschikbaarheid

U kunt SQL Server of Windows interne Database (WID) bevatten informatie over de AD FS-configuratie. WID biedt elementaire redundantie. Wijzigingen aan slechts een van de databases voor AD FS in het cluster ADFS geschreven terwijl de andere servers pull-replicatie gebruiken om hun database up-to-date te houden. Met behulp van SQL Server kan bieden volledige redundantie en hoge beschikbaarheid met behulp van failover clustering of spiegelen.

## <a name="security-considerations"></a>Beveiligingsoverwegingen

AD FS maakt gebruik van het HTTPS-protocol, dus zorg ervoor dat de NSG-regels voor het subnet met het web trapsgewijs VMs vergunning HTTPS-verzoeken. Deze aanvragen kunnen afkomstig zijn van het netwerk op gebouwen de subnetten met de weblaag, business tier gegevenslaag, persoonlijke DMZ, DMZ openbare en het subnet waarin de AD FS-servers.

Overweeg het gebruik van een set van virtuele netwerkapparaten die registreert gedetailleerde informatie over het verkeer doorlopen van de rand van het virtuele netwerk voor controledoeleinden.

## <a name="scalability-considerations"></a>Overwegingen voor schaalbaarheid

De volgende overwegingen, samengevat in het document [uw AD FS-implementatie plannen][plan-your-adfs-deployment], geven een beginpunt voor het aanpassen van AD FS-farms:

- Als er minder dan 1000 gebruikers, geen specifieke AD FS-servers maken, maar in plaats daarvan AD FS te installeren op alle servers in de cloud ADDS (Zorg ervoor dat er ten minste twee servers toevoegen, zodat de beschikbaarheid). Maak één WAP-server.

- Als er tussen de 1000 tot 15000 gebruikers, twee specifieke AD FS-servers en twee speciale WAP-servers maken.

- Als er tussen de 15000 en 60000 gebruikers, tussen drie en vijf specifieke AD FS-servers en ten minste twee speciale WAP-servers maken.

Deze cijfers wordt ervan uitgegaan dat u twee quad core VMs (standaard D4_v2 of beter) naar de hostservers in Azure.

Houd er rekening mee dat als u de Windows Internal Database voor het opslaan van gegevens van AD FS-configuratie gebruikt, u beperkt tot acht AD FS-servers in de farm bent. Als u dat u meer nodig hebt verwacht, gebruikt u SQL Server. Zie [De functie van de configuratiedatabase van AD FS]voor meer informatie[adfs-configuration-database].

## <a name="management-considerations"></a>Overwegingen bij

DevOps personeel moet worden voorbereid voor de volgende taken uitvoeren:

- De federation-servers (de AD FS-serverfarm beheren, vertrouwensbeleid van de federation-servers beheren en het beheer van de certificaten die worden gebruikt door de federation-services) beheren.

- Beheer van de WAP-server (de WAP-farm beheren van de WAP-certificaten beheren).

- Beheren van webtoepassingen (gebruikmakende partijen, verificatiemethoden en claims toewijzingen configureren).

- Back-ups maken van AD FS-onderdelen.

## <a name="monitoring-considerations"></a>Overwegingen met betrekking tot controle

De [Microsoft System Center Management Pack voor Active Directory Federation Services 2012 R2] [ oms-adfs-pack] biedt proactieve en reactieve monitoring van uw AD FS-implementatie voor de federation-server. Dit management pack monitoren:

- Gebeurtenissen die de AD FS-service records in de gebeurtenislogboeken van AD FS.

- De prestatiegegevens die de AD FS-prestatiemeteritems verzamelt. 

- De algehele gezondheid van de AD FS-systeem en de web-toepassingen (gebruikmakende partijen) en waarschuwingen voor kritieke problemen en waarschuwingen.

## <a name="solution-components"></a>Onderdelen van oplossingen

Een oplossing voorbeeldscript, [Deploy ReferenceArchitecture.ps1][solution-script], is beschikbaar die u gebruiken kunt voor het implementeren van de architectuur die volgt op de aanbevelingen in dit artikel beschreven. Dit script maakt gebruik van sjablonen Azure Resource Manager. De sjablonen zijn beschikbaar als een set van fundamentele bouwstenen, die elk een bepaalde actie, zoals het maken van een VNet of een NSG configureren uitvoert. Het doel van het script is goedkeuringen door sjabloonimplementatie.

De sjablonen zijn parameters met de parameters die in aparte JSON bestanden bewaard. U kunt de parameters in de volgende bestanden voor het configureren van het aan uw eigen vereisten aanpassen. U hoeft niet te wijzigen van de sjablonen zelf. Houd er rekening mee dat u niet de schema's van de objecten in de parameter moet wijzigen.

Wanneer u sjablonen bewerkt, objecten maken die de naamconventies die worden beschreven in de [Conventies aanbevolen voor bronnen Azure][naming-conventions].

De oplossing maakt en configureert de omgeving in de cloud die bestaat uit de ADDS subnet en servers met de AD FS-subnet en servers, ADFS proxy subnet en servers, DMZ, tier web business-tier en laag voor gegevenstoegang, VPN-gateway en management-laag. De oplossing bevat ook een optionele configuratie voor het maken van een gesimuleerde op gebouwen-omgeving.

In de volgende secties beschrijven de elementen van het pand op en configuraties van de wolk.

### <a name="on-premises-components"></a>Onderdelen van gebouwen

>[AZURE.NOTE] Deze onderdelen zijn niet de primaire focus van de architectuur wordt beschreven in dit document, en gewoon voor geeft u de gelegenheid voor het testen van de cloud-omgeving veilig, in plaats van een echte productieomgeving. Daarom in deze sectie alleen geeft een overzicht van de belangrijkste parameterbestanden. Kunt u instellingen voor de IP-adressen of de grootte van het VMs, maar het is aan te raden om veel van de andere parameters ongewijzigd laat.

Deze omgeving bestaat uit een AD forest voor een domein met de naam contoso.com. Het domein bevat twee ADDS servers met IP-adressen 192.168.0.4 en 192.168.0.5. Deze twee servers ook de DNS-service worden uitgevoerd. De lokale administrator-account op beide VMs heet `testuser` wachtwoord `AweS0me@PW`. Daarnaast stelt de configuratie van een VPN-gateway voor de verbinding met de VNet in de cloud. U kunt de configuratie wijzigen door het bewerken van de volgende JSON bestanden in de [**parameters/lokale**] [ on-premises-folder] map:

- ** [virtualNetwork.parameters.json][on-premises-vnet-parameters]**. Dit bestand definieert de netwerk-adresruimte voor het milieu op het bedrijf.

- ** [virtualMachines adds.parameters.json][on-premises-virtualmachines-adds-parameters]**. Dit bestand bevat de configuratie voor het in lokalen VMs ADDS hostingservices. Twee *standaard-DS3-v2* VMs worden standaard gemaakt.

- ** [virtualNetworkGateway.parameters.json] [ on-premises-virtualnetworkgateway-parameters] ** en ** [connection.parameters.json][on-premises-connection-parameters]**. Deze bestanden bevatten de instellingen voor de VPN-verbinding op de Azure VPN-gateway in de cloud, met inbegrip van de gedeelde sleutel moet worden gebruikt voor het beveiligen van verkeer via de gateway worden verzonden.

De overige bestanden in de map bevat de configuratie-informatie voor het maken van het domein op ruimten is met behulp van deze infrastructuur. U kunt deze ADDS installeren, setup DNS een forest te maken en configureren van de replicatie-sites voor het forest.

### <a name="cloud-components"></a>Onderdelen van de cloud

Deze onderdelen vormen de kern van deze architectuur. De [**parameters/azure**] [ azure-folder] map bevat de volgende parameterbestanden voor het configureren van deze onderdelen:

- ** [virtualNetwork.parameters.json][vnet-parameters]**. Dit bestand definieert de structuur van de VNet voor de VMs en andere onderdelen in de cloud. Het bevat de instellingen, zoals de naam, adres, subnetten en de adressen van de DNS-servers vereist. Houd er rekening mee dat de DNS-adressen weergegeven in dit voorbeeld verwijst naar de IP-adressen van DNS-servers op de gebouwen en de standaard Azure DNS-server. Deze adressen om te verwijzen naar uw eigen DNS-instellingen als u niet de voorbeeldomgeving op ruimten te wijzigen:

    ```json
    {
        "virtualNetworkSettings": {
            "value": {
                "name": "ra-adfs-vnet",
                "resourceGroup": "ra-adfs-network-rg",
                "addressPrefixes": [
                    "10.0.0.0/16"
                ],
                "subnets": [
                    {
                        "name": "dmz-private-in",
                        "addressPrefix": "10.0.0.0/27"
                    },
                    {
                        "name": "dmz-private-out",
                        "addressPrefix": "10.0.0.32/27"
                    },
                    {
                        "name": "dmz-public-in",
                        "addressPrefix": "10.0.0.64/27"
                    },
                    {
                        "name": "dmz-public-out",
                        "addressPrefix": "10.0.0.96/27"
                    },
                    {
                        "name": "mgmt",
                        "addressPrefix": "10.0.0.128/25"
                    },
                    {
                        "name": "GatewaySubnet",
                        "addressPrefix": "10.0.255.224/27"
                    },
                    {
                        "name": "web",
                        "addressPrefix": "10.0.1.0/24"
                    },
                    {
                        "name": "biz",
                        "addressPrefix": "10.0.2.0/24"
                    },
                    {
                        "name": "data",
                        "addressPrefix": "10.0.3.0/24"
                    },
                    {
                        "name": "adds",
                        "addressPrefix": "10.0.4.0/27"
                    },
                    {
                        "name": "adfs",
                        "addressPrefix": "10.0.5.0/27"
                    },
                    {
                        "name": "proxy",
                        "addressPrefix": "10.0.6.0/27"
                    }
                ],
                "dnsServers": [
                    "192.168.0.4",
                    "192.168.0.5",
                    "168.63.129.16"
                ]
            }
        }
    }
    ```

- ** [virtualMachines adds.parameters.json ] [ virtualmachines-adds-parameters] **. Dit bestand wordt geconfigureerd voor het VMs ADDS uitgevoerd in de cloud. De configuratie bestaat uit twee VMs. Moet u de gebruikersnaam admin en het wachtwoord in de `virtualMachineSettings` sectie, en u kunt desgewenst zo wijzigen dat de grootte van de VM aan de vereisten van het domein:

    Zie voor meer informatie de [Active Directory uitbreiden naar Azure][extending-ad-to-azure].

    ```json
            "virtualMachinesSettings": {
                "value": {
                    "namePrefix": "ra-adfs-ad",
                    "computerNamePrefix": "aad",
                    "size": "Standard_DS3_v2",
                    "osType": "Windows",
                    "adminUsername": "testuser",
                    "adminPassword": "AweS0me@PW",
                    "osAuthenticationType": "password",
                    "nics": [
                        {
                            "isPublic": "false",
                            "subnetName": "adds",
                            "privateIPAllocationMethod": "Static",
                            "startingIPAddress": "10.0.4.4",
                            "enableIPForwarding": false,
                            "dnsServers": [
                            ],
                            "isPrimary": "true"
                        }
                    ],
                    "imageReference": {
                        "publisher": "MicrosoftWindowsServer",
                        "offer": "WindowsServer",
                        "sku": "2012-R2-Datacenter",
                        "version": "latest"
                    },
                    "dataDisks": {
                        "count": 1,
                        "properties": {
                            "diskSizeGB": 127,
                            "caching": "None",
                            "createOption": "Empty"
                        }
                    },
                    "osDisk": {
                        "caching": "ReadWrite"
                    },
                    "extensions": [
                    ],
                    "availabilitySet": {
                        "useExistingAvailabilitySet": "No",
                        "name": "ra-adfs-as"
                    }
                }
            },
            "virtualNetworkSettings": {
                "value": {
                    "name": "ra-adfs-vnet",
                    "resourceGroup": "ra-adfs-network-rg"
                }
            },
            "buildingBlockSettings": {
                "value": {
                    "storageAccountsCount": 2,
                    "vmCount": 2,
                    "vmStartIndex": 1
                }
            }
        }
    ```

- ** [Toevoegen-voegt-domein-controller.parameters.json][add-adds-domain-controller-parameters]**. Dit bestand bevat de instellingen voor het maken van het domein CONTOSO is spanning van de servers toevoegen. Aangepaste extensies die tot stand brengen van het domein en de servers toevoegen aan toe te voegen wordt gebruikt. Tenzij u extra ADDS servers (in dat geval moet u deze toevoegen aan de `vms` matrix), de namen wijzigen van de standaardinstelling of wilt maken van een domein met een andere naam die u niet nodig om dit bestand te wijzigen.

- ** [loadBalancer adfs.parameters.json] [loadbalancer-adfs-parameters] ** Het bestand bevat twee sets van configuratieparameters. De `virtualMachineSettings` gedeelte definieert de VMs die als host van de AD FS-service in de cloud. Standaard maakt het script twee van deze VMs in bepaalde beschikbaarheid:

    ```json
        "virtualMachinesSettings": {
            "value": {
                "namePrefix": "ra-adfs-adfs",
                "computerNamePrefix": "adfs",
                "size": "Standard_DS1_v2",
                "osType": "windows",
                "adminUsername": "testuser",
                "adminPassword": "AweS0me@PW",
                "osAuthenticationType": "password",
                "nics": [
                    {
                        "isPublic": "false",
                        "subnetName": "adfs",
                        "privateIPAllocationMethod": "Static",
                        "startingIPAddress": "10.0.5.4",
                        "isPrimary": "true",
                        "enableIPForwarding": false,
                        "dnsServers": [ ]
                    }
                ],
                "imageReference": {
                    "publisher": "MicrosoftWindowsServer",
                    "offer": "WindowsServer",
                    "sku": "2012-R2-Datacenter",
                    "version": "latest"
                },
                "dataDisks": {
                    "count": 1,
                    "properties": {
                        "diskSizeGB": 128,
                        "caching": "None",
                        "createOption": "Empty"
                    }
                },
                "osDisk": {
                    "caching": "ReadWrite"
                },
                "extensions": [ ],
                "availabilitySet": {
                    "useExistingAvailabilitySet": "No",
                    "name": "ra-adfs-adfs-vm-as"
                }
            }
        }
        ...
        "buildingBlockSettings": {
            "value": {
                "storageAccountsCount": 2,
                "vmCount": 2,
                "vmStartIndex": 1
            }
        }
    ```

    De `loadBalancerSettings` vindt u de beschrijving van de verdeling van de belasting voor deze VMs. Verdeling van de belasting wordt doorgegeven verkeer op poort 443 (HTTPS) naar een of andere van de VMs:

    ```json
        "loadBalancerSettings": {
            "value": {
                "name": "ra-adfs-adfs-lb",
                "frontendIPConfigurations": [
                    {
                        "name": "ra-adfs-adfs-lb-fe",
                        "loadBalancerType": "internal",
                        "internalLoadBalancerSettings": {
                            "privateIPAddress": "10.0.5.30",
                            "subnetName": "adfs"
                        }
                    }
                ],
                "backendPools": [
                    {
                        "name": "ra-adfs-adfs-lb-bep",
                        "nicIndex": 0
                    }
                ],
                "loadBalancingRules": [
                    {
                        "name": "https-rule",
                        "frontendPort": 443,
                        "backendPort": 443,
                        "protocol": "Tcp",
                        "backendPoolName": "ra-adfs-adfs-lb-bep",
                        "frontendIPConfigurationName": "ra-adfs-adfs-lb-fe",
                        "probeName": "https-probe",
                        "enableFloatingIP": false
                    }
                ],
                "probes": [
                    {
                        "name": "https-probe",
                        "port": 443,
                        "protocol": "Tcp",
                        "requestPath": null
                    }
                ],
                "inboundNatRules": [ ]
            }
        },
        "virtualNetworkSettings": {
            "value": {
                "name": "ra-adfs-vnet",
                "resourceGroup": "johns-adfs-network-rg"
            }
        }
    ```

- ** [AD FS-farm-domein-join.parameters.json ] [ adfs-farm-domain-join-parameters] **. Dit bestand bevat de instellingen voor de AD FS-servers toevoegen aan het domein CONTOSO. Alleen moet u dit bestand wijzigen als u extra AD FS-servers hebt gemaakt (werken de `vms` array, in dit geval), of u de naam hebt gewijzigd.

- ** [gmsa.parameters.json][gmsa-parameters]**, ** [adfs-farm-first.parameters.json][adfs-farm-first-parameters]**, en ** [adfs-farm-rest.parameters.json][adfs-farm-rest-parameters]**. Het script maakt gebruik van de instellingen in deze bestanden te maken van de AD FS-serverfarm. 

    Het bestand *gmsa.parameters.json* bevat de instellingen voor de groep beheerd serviceaccount wordt gebruikt door de service van ADFS. U kunt dit bestand wijzigen als u wilt dat de naam van de account of het domein wijzigen.

    De *AD FS-farm-first.parameters.json* -bestand bevat de informatie die nodig is voor de AD FS-serverfarm maken en toevoegen van de eerste server. Als u het domein of de naam van de groep beheerde service-account hebt gewijzigd, moet u dit bestand bijwerken.

    De *AD FS-farm-rest.parameters.json* -bestand wordt gebruikt voor de resterende AD FS-servers toevoegen aan de farm. Nogmaals, als u het domein of de naam van de groep beheerde service-account hebt gewijzigd, moet u dit bestand bijwerken. Update de `vms` een matrix als u extra AD FS-servers hebt gemaakt.

- ** [loadBalancer adfsproxy.parameters.json][loadBalancer-adfsproxy-parameters]**. Dit bestand is qua structuur en inhoud in het bestand *loadBalancer-adfs.parameters.json* . Het bevat de gegevens voor het bouwen van de AD FS-proxyservers en taakverdeling.

- ** [adfsproxy-farm-first.parameters.json][adfsproxy-farm-first-parameters]**, en ** [adfsproxy-farm-rest.parameters.json][adfsproxy-farm-rest-parameters]**. Het script maakt gebruik van de instellingen in deze bestanden te maken van de AD FS-proxy server-farm. 

- ** [virtualNetworkGateway.parameters.json][virtualnetworkgateway-parameters]**. Dit bestand bevat de instellingen voor de Azure VPN-gateway maken in de cloud gebruikt voor verbinding met het netwerk op gebouwen. U moet wijzigen de `sharedKey` de waarde in de `connectionsSettings` sectie aangepast aan die van het VPN-apparaat voor op gebouwen. Zie voor meer informatie, [een hybride netwerkarchitectuur met Azure en On-premises VPN-implementatie van][hybrid-azure-on-prem-vpn].

- ** [dmz private.parameters.json] [ dmz-private-parameters] ** en ** [dmz public.parameters.json ] [ dmz-public-parameters] **. Deze bestanden configureren in de binnenkomende (openbaar) en uitgaande (particuliere) zijden van het VMs bestaat uit de DMZ, bescherming van de servers in de cloud. Zie voor meer informatie over deze elementen en hun configuratie, [implementatie van een DMZ tussen Azure en Internet][implementing-a-secure-hybrid-network-architecture-with-internet-access].

- ** [loadBalancer-web.parameters-json][loadBalancer-web-parameters]**, ** [loadBalancer-biz.parameters-json][loadBalancer-biz-parameters]**, en ** [loadBalancer-data.parameters-json][loadBalancer-data-parameters]**. Deze parameters-bestanden bevatten de VM-specificaties voor de toegang tot web-, business- en onderlaag en configureren van netwerktaakverdeling voor elke laag. Dit zijn de VMs die host web apps en databases, en de werklast business uitvoeren voor de organisatie. Desgewenst kunt u de grootte en aantal VMs in elke laag wijzigen.

- ** [virtualMachines mgmt.parameters.json][virtualMachines-mgmt-parameters]**. Dit bestand bevat de configuratie voor het vak sprong/beheer VMs. Het kan alleen aanmeldings- en beheertoegang tot de VMs in het web, zakelijke en lagen van gegevens uit het vak sprong krijgen. Het script maakt een enkele *Standard_DS1_v2* VM, maar kunt u dit bestand groter of meer VMs maken als u de werkbelasting management is waarschijnlijk beduidend zal zijn.

## <a name="solution-deployment"></a>Implementatie van oplossingen

De oplossing wordt ervan uitgegaan dat de volgende vereisten:

- U hebt een Azure abonnement waar kunt u resourcegroepen maken.

- U hebt gedownload en geïnstalleerd van de meest recente versie van Azure Powershell. Zie [hier] [ azure-powershell-download] voor instructies.

Het script uitvoeren dat de oplossing implementeert:

1. Verplaatsen naar een geschikte map op uw lokale computer en maak de volgende submappen:

    - Scripts

    - Scripts/Parameters

    - Parameters-scripts/lokale

    - Parameters-scripts/Azure

2. Downloaden van de [Implementatie ReferenceArchitecture.ps1] [ solution-script] bestand in de map Scripts

3. Downloaden van de inhoud van de [parameters/lokale] [ on-premises-folder] map naar de map Parameters-Scripts/lokale:

4. De inhoud van de [parameters/azure] downloaden[ azure-folder] map naar de map Parameters-Scripts/Azure.

5. Het bestand distribueren ReferenceArchitecture.ps1 in de map Scripts bewerken en wijzigt u de volgende regels als u wilt aangeven welke resourcegroepen die moeten worden gemaakt of voor het opslaan van de resources die zijn gemaakt door het script:

    ```powershell
    # Azure Onpremise Deployments
        $onpremiseNetworkResourceGroupName = "ra-adfs-onpremise-rg"
        
        # Azure ADDS Deployments
        $azureNetworkResourceGroupName = "ra-adfs-network-rg"
        $workloadResourceGroupName = "ra-adfs-workload-rg"
        $securityResourceGroupName = "ra-adfs-security-rg"
        $addsResourceGroupName = "ra-adfs-adds-rg"
        $adfsResourceGroupName = "ra-adfs-adfs-rg"
        $adfsproxyResourceGroupName = "ra-adfs-proxy-rg"
    ```

6. De parameterbestanden in de mappen van de Parameters-Scripts/Azure en Parameters-Scripts/lokale bewerken. Verwijzingen naar de resource in deze bestanden die overeenkomen met de namen van de resourcegroepen is toegewezen aan de variabelen in het bestand distribueren ReferenceArchitecture.ps1 bijwerken. De volgende tabel ziet u welke parameterbestanden verwijst naar welke resourcegroep. Overigens de *ra adfs-werkbelasting rg*, *ra adfs-beveiliging rg* *ra-adfs-toegevoegd-rg*, *ra adfs-adfs rg*en *ra adfs-proxy rg* -groepen worden alleen gebruikt in het PowerShell script en niet op bij de parameterbestanden.

  	|Resourcegroep|Parameter bestand(en)|
  	|--------------|--------------|
  	|Ra adfs-lokale rg|parameters\onpremise\connection.parameters.JSON<br /> parameters\onpremise\virtualMachines-ADDS.parameters.JSON<br />parameters\onpremise\virtualNetwork-ADDS-DNS.parameters.JSON<br />parameters\onpremise\virtualNetwork.parameters.JSON<br />parameters\onpremise\virtualNetworkGateway.parameters.JSON<br />parameters\azure\virtualNetworkGateway.parameters.JSON
  	|Ra adfs-netwerk rg|parameters\onpremise\connection.parameters.JSON<br />parameters\azure\dmz-Private.parameters.JSON<br />parameters\azure\dmz-Public.parameters.JSON<br />parameters\azure\loadBalancer-ADFS.parameters.JSON<br />parameters\azure\loadBalancer-adfsproxy.parameters.JSON<br />parameters\azure\loadBalancer-Biz.parameters.JSON<br />parameters\azure\loadBalancer-Data.parameters.JSON<br />parameters\azure\loadBalancer-web.parameters.JSON<br />parameters\azure\virtualMachines-ADDS.parameters.JSON<br />parameters\azure\virtualMachines-Mgmt.parameters.JSON<br />parameters\azure\virtualNetwork-with-onpremise-and-Azure-DNS.parameters.JSON<br />parameters\azure\virtualNetwork.parameters.JSON<br />parameters\azure\virtualNetworkGateway.parameters.JSON (*twee keer*)

    Daarnaast stelt de configuratie voor de lokalen op en cloud-onderdelen, zoals wordt beschreven in de sectie oplossingsonderdelen [oplossingsonderdelen].

7. Een Azure PowerShell-venster openen, verplaatsen naar de map Scripts en voer de volgende opdracht uit:

    ```powershell
    .\Deploy-ReferenceArchitecture.ps1 <subscription id> <location> <mode>
    ```

    Vervangen `<subscription id>` bij de Azure abonnement-ID.

    Voor `<location>`, Geef een Azure regio, zoals `eastus` of `westus`.

    De `<mode>` parameter kan een van de volgende waarden hebben:

    - `Onpremise`, de gesimuleerde op ruimten omgeving maken.

    - `Infrastructure`, voor het maken van de infrastructuur van VNet en springen vak in de cloud.

    - `CreateVpn`, Azure virtueel netwerkgateway bouwen en verbinden met het netwerk op gebouwen.

    - `AzureADDS`, om het VMs fungeren als servers ADDS samenstellen, implementeren van Active Directory op deze VMs en het domein maken in de cloud.

    - `AdfsVm`het VMs ADFS maken en deze toevoegen aan het domein in de cloud.

    - `ProxyVm`de AD FS-proxy VMs maken en deze toevoegen aan het domein in de cloud.

    - `Prepare`, de bovengenoemde taken uitvoert. **Dit is de aanbevolen optie als u een volledig nieuwe installatie bouwt en u niet een bestaande op gebouwen-infrastructuur hoeft.**

    >[AZURE.NOTE] U kunt het script ook uitvoeren een `<mode>` parameter van `Workload` voor het maken van het web, business, en de gegevenslaag VMs en netwerk. Dit is niet opgenomen als onderdeel van de `Prepare` modus.

    Als u de `Prepare` , het script neemt enkele uren en afwerking van het bericht *preparaat is voltooid. Installeer een certificaat naar alle AD FS en proxy VMs.*

8.  Het vak sprong (*ra adfs-mgmt vm1* in de groep *ra adfs-beveiliging rg* ) opnieuw als u wilt dat de DNS-instellingen worden pas van kracht.

9.  [Een SSL-certificaat verkrijgen voor AD FS] [ adfs_certificates] en dit certificaat installeren op de AD FS-VMs. Houd er rekening mee dat u met het VMs ADFS via het vak sprong verbinden kunt. De IP-adressen zijn *10.0.5.4* en *10.0.5.5*. De standaardgebruikersnaam is *contoso\testuser* met wachtwoord *AweSome@PW*.

    >[AZURE.NOTE] Op dit moment bieden gedetailleerde instructies voor het maken van een zelfondertekend testcertificaat en autoriteit met behulp van de opmerkingen in het script implementeren-ReferenceArchitecture.ps1 de `makecert` opdracht. Gebruik echter niet de certificaten die zijn gegenereerd door makecert in een productieomgeving.

10. De volgende Powershell-opdracht voor het configureren van de AD FS-serverfarm uitvoeren:

    ```powershell
    .\Deploy-ReferenceArchitecture.ps1 <subscription id> <location> Adfs
    ```

11. Ga naar *https://adfs.contoso.com/adfs/ls/idpinitiatedsignon.htm* voor het testen van de installatie van AD FS (verschijnt een waarschuwing, die voor deze test kunt u negeren) in het vak koppeling. Controleer of de Contoso Corporation-in pagina wordt weergegeven. Meld je aan als *contoso\testuser* met wachtwoord *AweS0me@PW*.

12. Het SSL-certificaat installeren op de AD FS-proxy VMs. De IP-adressen zijn *10.0.6.4* en *10.0.6.5*.

13. Voer de volgende Powershell-opdracht om de eerste AD FS-proxyserver te configureren:

    ```powershell
    .\Deploy-ReferenceArchitecture.ps1 <subscription id> <location> Proxy1
    ```

14. Volg de instructies weergegeven door het script te testen van de installatie van de eerste AD FS-proxyserver.

15. Voer de volgende Powershell-opdracht om de tweede eerste ADFS-proxyserver te configureren:

    ```powershell
    .\Deploy-ReferenceArchitecture.ps1 <subscription id> <location> Proxy2
    ```

16. Volg de instructies weergegeven door het script te testen van de volledige configuratie van de AD FS-proxy.

## <a name="next-steps"></a>Volgende stappen

- [Azure Active Directory][aad].

- [B2C Azure Active Directory][aadb2c].

<!-- links -->

[vm-recommendations]: ./guidance-compute-single-vm.md#Recommendations
[naming-conventions]: ./guidance-naming-conventions.md
[implementing-active-directory]: ./guidance-identity-adds-extend-domain.md
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[implementing-a-secure-hybrid-network-architecture]: ./guidance-iaas-ra-secure-vnet-hybrid.md
[implementing-a-secure-hybrid-network-architecture-with-internet-access]: ./guidance-iaas-ra-secure-vnet-dmz.md
[DRS]: https://technet.microsoft.com/library/dn280945.aspx
[where-to-place-an-fs-proxy]: https://technet.microsoft.com/library/dd807048.aspx
[ADDRS]: https://technet.microsoft.com/library/dn486831.aspx
[plan-your-adfs-deployment]: https://msdn.microsoft.com/library/azure/dn151324.aspx
[ad_network_recommendations]: #network_configuration_recommendations_for_AD_DS_VMs
[domain_and_forests]: https://technet.microsoft.com/library/cc759073(v=ws.10).aspx
[adfs_certificates]: https://technet.microsoft.com/library/dn781428(v=ws.11).aspx
[create_service_account_for_adfs_farm]: https://technet.microsoft.com/library/dd807078.aspx
[import_server_authentication_certificate]: https://technet.microsoft.com/library/dd807088.aspx
[adfs-configuration-database]: https://technet.microsoft.com/en-us/library/ee913581(v=ws.11).aspx
[active-directory-federation-services]: https://technet.microsoft.com/windowsserver/dd448613.aspx
[security-considerations]: #security-considerations
[recommendations]: #recommendations
[claims-aware applications]: https://msdn.microsoft.com/en-us/library/windows/desktop/bb736227(v=vs.85).aspx
[active-directory-federation-services-overview]: https://technet.microsoft.com/en-us/library/hh831502(v=ws.11).aspx
[establishing-federation-trust]: https://blogs.msdn.microsoft.com/alextch/2011/06/27/establishing-federation-trust/
[Deploying_a_federation_server_farm]: https://technet.microsoft.com/library/dn486775.aspx
[install_and_configure_the_web_application_proxy_server]: https://technet.microsoft.com/library/dn383662.aspx
[publish_applications_using_AD_FS_preauthentication]: https://technet.microsoft.com/library/dn383640.aspx
[managing-adfs-components]: https://technet.microsoft.com/library/cc759026.aspx
[oms-adfs-pack]: https://www.microsoft.com/download/details.aspx?id=41184
[azure-powershell-download]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[aad]: https://azure.microsoft.com/documentation/services/active-directory/
[aadb2c]: https://azure.microsoft.com/documentation/services/active-directory-b2c/
[adfs-intro]: ../active-directory/active-directory-aadconnect-azure-adfs.md
[solution-script]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/Deploy-ReferenceArchitecture.ps1
[on-premises-folder]: https://github.com/mspnp/reference-architectures/tree/master/guidance-identity-adfs/parameters/onpremise
[on-premises-vnet-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/onpremise/virtualNetwork.parameters.json
[on-premises-virtualmachines-adds-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/onpremise/virtualMachines-adds.parameters.json
[on-premises-virtualnetworkgateway-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/onpremise/virtualNetworkGateway.parameters.json
[on-premises-connection-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/onpremise/connection.parameters.json
[azure-folder]: https://github.com/mspnp/reference-architectures/tree/master/guidance-identity-adfs/parameters/azure
[vnet-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/virtualNetwork.parameters.json
[dmz-private-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/dmz-private.parameters.json
[dmz-public-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/dmz-public.parameters.json
[virtualnetworkgateway-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/virtualNetworkGateway.parameters.json
[hybrid-azure-on-prem-vpn]: ./guidance-hybrid-network-vpn.md
[virtualmachines-adds-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/virtualMachines-adds.parameters.json
[extending-ad-to-azure]: ./guidance-identity-adds-extend-domain.md
[loadbalancer-adfs-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/loadBalancer-adfs.parameters.json
[add-adds-domain-controller-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/add-adds-domain-controller.parameters.json
[gmsa-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/gmsa.parameters.json
[adfs-farm-first-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/adfs-farm-first.parameters.json
[adfs-farm-rest-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/adfs-farm-rest.parameters.json
[adfs-farm-domain-join-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/adfs-farm-domain-join.parameters.json
[loadBalancer-web-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/loadBalancer-web.parameters.json
[loadBalancer-biz-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/loadBalancer-biz.parameters.json
[loadBalancer-data-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/loadBalancer-data.parameters.json
[virtualMachines-mgmt-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/virtualMachines-mgmt.parameters.json
[loadBalancer-adfsproxy-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/loadBalancer-adfsproxy.parameters.json
[adfsproxy-farm-first-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/adfsproxy-farm-first.parameters.json
[adfsproxy-farm-rest-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/adfsproxy-farm-rest.parameters.json
[0]: ./media/guidance-iaas-ra-secure-vnet-adfs/figure1.png "Hybride netwerkarchitectuur met Active Directory beveiligen"
