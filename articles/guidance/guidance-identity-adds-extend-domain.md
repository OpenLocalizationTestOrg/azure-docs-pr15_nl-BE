<properties
   pageTitle="Referentie architectuur Azure - IaaS: Active Directory uitbreiden naar Azure | Microsoft Azure"
   description="Het implementeren van een beveiligde hybride netwerkarchitectuur met verificatie van Active Directory in Azure."
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
   ms.date="07/19/2016"
   ms.author="telmos"/>

# <a name="extending-active-directory-directory-services-adds-to-azure"></a>Active Directory directoryservices (ADDS) uitbreiden naar Azure

[AZURE.INCLUDE [pnp-RA-branding](../../includes/guidance-pnp-header-include.md)]

Dit artikel bevat aanbevelingen voor uw Active Directory (AD)-omgeving uitbreiden naar Azure gedistribueerde verificatie om diensten te verlenen met behulp van [Active Directory Domain Services (AD DS)][active-directory-domain-services]. Deze architectuur wordt uitgebreid die in de [uitvoering van een beveiligde hybride netwerkarchitectuur in Azure] artikelen beschreven[ implementing-a-secure-hybrid-network-architecture] en de [uitvoering van een beveiligde hybride netwerkarchitectuur met toegang tot het Internet in Azure][implementing-a-secure-hybrid-network-architecture-with-internet-access].

> [AZURE.NOTE] Azure heeft twee verschillende modellen: [Resource Manager] [ resource-manager-overview] en klassiek. Deze referentiearchitectuur gebruikt resourcemanager, wordt aangeraden voor nieuwe implementaties.

U kunt AD DS gebruikt om identiteiten te verifiëren. Deze identiteiten kunnen behoren tot gebruikers, computers, toepassingen en andere bronnen die deel uitmaken van een beveiligingsdomein. Kunt u AD DS op ruimten host kan, maar in een hybride scenario waar de elementen van een toepassing in Azure bevinden zich efficiënter voor het repliceren van deze functionaliteit en de opslagplaats AD naar de cloud. Deze benadering verkleint de vertraging veroorzaakt door verificatie verzenden en lokale vergunning aanvragen uit de cloud terug naar AD DS op locatie uitgevoerd. 

Er zijn twee manieren voor het hosten van uw directoryservices in Azure:

1. U kunt [Azure Active Directory (AAD)] [ azure-active-directory] een nieuw AD-domein in de cloud maken en koppelen aan een bedrijf op AD-domein. Vervolgens setup [Azure AD verbinden] [ azure-ad-connect] op beloften voor het repliceren van identiteiten gehouden de de bibliotheek op het bedrijf naar de cloud. Opmerking: de map in de cloud is **niet** een uitbreiding van het systeem in de lokalen, het is een exemplaar met dezelfde identiteiten. Wijzigingen in deze identiteiten op het bedrijf wordt gekopieerd naar de cloud, maar wijzigingen in de cloud **niet** worden weer gerepliceerd naar het domein op gebouwen. Opnieuw instellen van wachtwoorden moeten bijvoorbeeld worden uitgevoerd op gebouwen en Azure AD Connect gebruiken om de wijziging te kopiëren naar de cloud. Let ook op dat het hetzelfde exemplaar van AAD kan worden gekoppeld aan meer dan één exemplaar van AD DS; AAD bevat de identiteit van elke opslagplaats AD waaraan deze is gekoppeld.

    AAD is nuttig in situaties waarin de netwerk op gebouwen en Azure virtueel netwerk die als host optreedt voor de bronnen van de wolk niet rechtstreeks zijn gekoppeld met behulp van een VPN-tunnel of ExpressRoute circuit. Hoewel deze oplossing eenvoudig is, het mogelijk niet geschikt voor systemen waarbij onderdelen over de grens op-ruimten/cloud kunnen migreren omdat dit nodig kan herconfiguratie van AAD. Ook zorgt AAD alleen voor verificatie van de gebruiker in plaats van verificatie. Sommige toepassingen en services, zoals SQL Server, kunnen verificatie vereisen deze oplossing is in dat geval niet geschikt.

2. U kunt een VM AD DS wordt uitgevoerd als een domeincontroller in Azure, het uitbreiden van uw bestaande infrastructuur van AD van uw datacenter op ruimten implementeren. Deze benadering is het meest voorkomende scenario's waarbij het netwerk op gebouwen en Azure virtueel netwerk zijn verbonden door een verbinding van VPN-en/of ExpressRoute. Deze oplossing biedt ook ondersteuning voor bidirectionele replicatie waarmee u wijzigingen aanbrengen in de cloud en in ruimten waar het meest geschikt is. De installatie van AD in de cloud kan afhankelijk van uw beveiligingsvereisten zijn:
    - deel uitmaken van hetzelfde domein als dat vastgehouden op-gebouwen
    - een nieuw domein binnen een forest gedeelde
    - een afzonderlijk forest

<!-- we might want to add info on how to choose from the three options above -->

Deze architectuur richt zich op oplossing 2, met de dezelfde AD DS-domein in Azure en op gebouwen.

Typische use-cases voor deze architectuur zijn:

- Hybride toepassingen waar werklasten uitvoeren deels op-lokalen en deels in Azure.

- Toepassingen en services die verificatie uitvoeren met behulp van Active Directory.

## <a name="architecture-diagram"></a>Architectuurdiagram

In het volgende diagram worden de belangrijke onderdelen van deze architectuur (*Klik om in te zoomen*). Lees voor meer informatie over de elementen beschikbaar [ter uitvoering van een beveiligde hybride netwerkarchitectuur in Azure] [ implementing-a-secure-hybrid-network-architecture] en de [uitvoering van een beveiligde hybride netwerkarchitectuur met toegang tot het Internet in Azure][implementing-a-secure-hybrid-network-architecture-with-internet-access]:

[! [0]][0]

- **Netwerk van gebouwen.** Het netwerk op ruimten bevat lokale AD-servers die verificatie en machtiging voor onderdelen die zich in ruimten kunnen uitvoeren.

- **AD-Servers.** Dit zijn domeincontrollers implementeren van directoryservices (AD DS) die als VMs in de cloud. Deze servers kunnen de verificatie van de onderdelen die worden uitgevoerd in uw Azure virtueel netwerk bieden.

- **Active Directory-subnet.** De AD DS-servers worden gehost in een apart subnet. NSG-regels beschermen de AD DS-servers en biedt een firewall tegen verkeer uit onverwachte bronnen.

- **Gateway azure en AD synchronisatie.**. De gateway Azure biedt een verbinding tussen het netwerk van gebouwen en de Azure-VNet. Dit is een [VPN-verbinding] [ azure-vpn-gateway] of [Azure ExpressRoute][azure-expressroute]. Alle aanvragen voor synchronisatie tussen AD servers in de cloud en op locatie doorgeven via de gateway. Door de gebruiker gedefinieerde routes (UDRs) verwerkt routering voor synchronisatie-verkeer dat wordt doorgegeven aan de AD-server in de cloud rechtstreeks en niet langs het bestaande netwerk virtuele toestellen (NVAs) gebruikt in dit scenario.

    Zie voor meer informatie over het configureren van UDRs en de NVAs [implementeert een beveiligde hybride netwerkarchitectuur in Azure][implementing-a-secure-hybrid-network-architecture].

## <a name="recommendations"></a>Aanbevelingen

Deze sectie bevat een overzicht van aanbevelingen voor de implementatie van AD DS in Azure, die betrekking hebben op:

- VM-aanbevelingen.

- Aanbevelingen voor netwerken.

- Aanbevelingen voor beveiliging. 

- Aanbevelingen voor Active Directory-Site.

- Active Directory FSMO-plaatsing aanbevelingen.

>[AZURE.NOTE] Het document [richtlijnen voor het implementeren van Windows Server Active Directory Azure virtuele Machines] [ ad-azure-guidelines] bevat gedetailleerde informatie over veel van deze punten.

### <a name="vm-recommendations"></a>VM-aanbevelingen

VMs maken met voldoende middelen voor het verwerken van de verwachte hoeveelheid verkeer. De grootte van de machines die als host fungeert voor AD DS in gebouwen als uitgangspunt gebruiken. Monitor het Resourcegebruik; u kunt het formaat van het VMs en schalen naar beneden als ze te groot zijn. Zie [Capaciteitsplanning voor Active Directory Domain Services]voor meer informatie over het aanpassen van AD DS-domeincontrollers,[capacity-planning-for-adds].

Maak een afzonderlijke gegevens van de virtuele schijf voor het opslaan van de database, logboekbestanden en SYSVOL voor advertentie. Sla deze items niet op dezelfde schijf als het besturingssysteem. Houd er rekening mee dat gegevensschijven die zijn gekoppeld aan een VM standaard write-through caching gebruiken. Echter, deze vorm van caching kan conflicteren met de eisen van AD DS. Daarom stelt de voorkeursinstelling voor *Host* -Cache op de gegevensschijf op *geen*. Zie voor meer informatie, [plaatsing van de Windows Server AD DS-database en SYSVOL][adds-data-disks].

Implementeren van ten minste twee VMs AD DS-domeincontrollers uitvoert met uw Azure virtual network omwille van de [beschikbaarheid](#Availability-considerations) .

### <a name="networking-recommendations"></a>Aanbevelingen voor netwerken

De netwerkinterface configureren voor elk van de VMs AD DS met statische particuliere IP-adressen die als host fungeert. Deze configuratie wordt beter DNS ondersteunt op elk van de VMs AD. Zie [het statische particulier IP-adres in de Azure portal instellen]voor meer informatie,[set-a-static-ip-address].

> [AZURE.NOTE] Geeft de AD DS-VMs openbare IP-adressen. Zie [beveiligingsoverwegingen] [ security-considerations] voor meer informatie.

U moet ervoor zorgen dat verkeer tussen de AD-servers in de cloud en op ruimten vrij kan stromen:

- NSG regels toevoegen aan de AD-subnet dat binnenkomend verkeer toestaan op ruimten. Zie voor gedetailleerde informatie over de poorten die gebruikmaken van AD DS [Active Directory en Active Directory Domain Services poort eisen][ad-ds-ports].

- Zorg ervoor dat tabellen UDR AD DS-verkeer via de NVAs gebruikt in dit scenario niet doorsturen. Voor uw eigen implementaties, afhankelijk van welke NVAs u gebruikt, kunt u dat verkeer omleiden.

### <a name="security-recommendations"></a>Aanbevelingen voor beveiliging

AD DS-servers verwerken van verificatie en zijn daarom zeer gevoelige goederen. Voorkomen dat de rechtstreekse blootstelling van de AD DS-servers met het Internet. AD DS-servers in een afzonderlijk subnet, met een eigen firewall plaatsen. Zorg ervoor dat de poorten die nodig zijn om AD DS gebruiken voor verificatie en autorisatie en de synchronzing servers open blijven, maar alle andere poorten sluit. Voor meer informatie Zie [Active Directory en Active Directory Domain Services poort eisen][ad-ds-ports]. De [onderdelen van de oplossing] [ solution-components] sectie verderop in dit document wordt de NSG regels dat de monsteroplossing wordt gebruikt voor het openen van poorten.

NSG-regels kunt u een eenvoudige firewall. Als u een ruimere bescherming nodig kunt u een extra beveiliging omtrek rond servers implementeren met behulp van een paar van de subnetten en NVAs, zoals beschreven door de [uitvoering van een beveiligde hybride netwerkarchitectuur met toegang tot het Internet in Azure]document[implementing-a-secure-hybrid-network-architecture-with-internet-access].

BitLocker of Azure schijf codering gebruikt voor het coderen van de schijf die als host fungeert voor de AD DS-database.

### <a name="active-directory-site-recommendations"></a>Aanbevelingen voor Active Directory-Site

U kunt sites in AD DS naar groep samen domeincontrollers die zijn verbonden via een snelle koppeling. Domeincontrollers op dezelfde AD DS-site hun directorywijzigingen repliceren automatisch en weinig configuratie is noodzakelijk voor het verwerken van de replicatie.

Om te bepalen het replicatieverkeer tussen Azure en uw datacenter op gebouwen, verdient het toevoegen van een AD DS-site afzonderlijk de adresruimte die door Azure worden gebruikt voor de weergave. U kunt een site-koppeling tussen uw locatie op AD DS sites en replicatie van niet-sitegebonden effectiever beheren.

U kunt ook scheiding van de site toe te passen verschillende groepsbeleidsobjecten (GPO's) voor computers in Azure tot en gebruik te maken van toepassingen die "site op de hoogte", zoals System Center Configuration Manager zijn gebruiken.

### <a name="active-directory-fsmo-placement-recommendations"></a>Aanbevelingen voor Active Directory FSMO-plaatsing

Flexibele bewerking van één Master (FSMO)-servers zijn domeincontrollers gespecialiseerde, reposnsible voor de consistentie van de gegevens over de verschillende instellingen in AD DS, die hieronder worden beschreven.

- **Schema-Master**. Dit is een hele forest rol die wordt onderhouden door de structuur van het schema door AD DS gebruikt.

- **Domain Naming Master**. Dit is een hele forest rol die informatie over domeinnamen binnen het forest wordt beheerd.

- De **primaire domeincontroller (PDC)**. Dit is een functie van het hele domein. De primaire domeincontroller verwerkt bijwerken van wachtwoorden en accountvergrendelingen. Deze wordt door andere DCs geraadpleegd wanneer verificatieaanvragen hebben de verkeerde wachtwoorden. De PDC ook updates voor Groepsbeleid verwerkt en wordt het doel-DC voor oudere toepassingen en sommige admin tools die sommige beschrijfbare bewerkingen uitvoeren.

- **Relatieve ID (RID) model**. RID's worden gebruikt voor het uniek identificeren van objecten in een map. Deze server is verantwoordelijk voor het genereren van een groep domeinomvattende RID's voor gebruik door alle AD servers binnen het domein.

- **De functie van infrastructuur**. Een object in het ene domein kan verwijzen naar een object in een ander domein. Deze rol van het hele domein is verantwoordelijk voor het bijwerken van de SID van een object en de DN-naam in de verwijzing naar een object tussen domeinen. Houd er rekening mee dat een server ter uitvoering van deze rol ook kan niet als een server voor de globale catalogus (GC fungeren).

Zie [Active Directory-FSMO-rollen in Windows]voor meer informatie[AD-FSMO-roles-in-windows].

In dit scenario is het raadzaam dat u FSMO-functies implementeren op de domeincontrollers in Azure voorkomen. 

## <a name="availability-considerations"></a>Overwegingen met betrekking tot beschikbaarheid

Maak een beschikbaarheid van de AD-servers instellen. Zorg ervoor dat er ten minste twee servers in de set. De AD-servers in de cloud moet domeincontrollers binnen hetzelfde domein. Hiermee schakelt u automatische replicatie tussen servers.

Bedenk ook servers wijzen als [stand-by operations-masters] [ standby-operations-masters] in het geval de verbinding met een server die fungeert als een FSMO-functie is mislukt.

## <a name="security-considerations"></a>Beveiligingsoverwegingen

Als alle taken van domein admin dreigen te worden uitgevoerd met de DCs van gebouwen, overweeg DC's in de cloud alleen-lezen. Een alleen-lezen domeincontroller onderhoudt een subset van de referenties van gebruikers (voldoende lokaal verificatie uitvoeren) en kan worden geconfigureerd om de cache-informatie alleen voor bepaalde gebruikers alleen. Dus als een alleen-lezen domeincontroller wordt aangetast, worden alleen de informatie voor gebruikers in de cache beïnvloed, in plaats van de details van elke account in het domein. Zie voor meer informatie, [Alleen-lezen domeincontrollers][read-only-dc].

Volg de beste praktijken voor het instellen en onderhouden van wachtwoorden van gebruikers in AD helpen de kwetsbaarheid van afzonderlijke gebruikersaccounts te minimaliseren en pogingen tot inbraak ontmoedigen. Zie [Aanbevolen procedures voor het afdwingen van beleid voor wachtwoorden]voor meer informatie,[best_practices_ad_password_policy]. Ook Let op welke groepen u toewijzen aan gebruikers. Bijvoorbeeld gewone gebruikers lid van de groep Ondernemingsadministrators, de groep Schema-Administrators en de groep Domeinadministrators niet maken.

## <a name="scalability-considerations"></a>Overwegingen voor schaalbaarheid

AD wordt automatisch aangepast voor domeincontrollers die deel van hetzelfde domein uitmaken. Aanvragen worden gedistribueerd over alle domeincontrollers in een domein. U kunt een andere domeincontroller toevoegen en wordt het automatisch synchroniseren met het domein. Configureer een aparte load balancer rechtstreeks verkeer naar domeincontrollers binnen het domein. Zorg ervoor dat alle domeincontrollers beschikken over voldoende geheugen en opslagbronnen voor het verwerken van de domeindatabase. in het ideale geval domeincontroller alle VMs-dezelfde grootte maken.

## <a name="management-considerations"></a>Overwegingen bij

Kopieer het VHD-bestanden van domeincontrollers in plaats van het uitvoeren van regelmatige back-ups omdat deze terug te zetten leiden inconsistenties in staat tussen domeincontrollers tot kan niet.

Afsluiten en opnieuw opstarten van een die wordt uitgevoerd als de functie van domeincontroller Azure binnen het gastbesturingssysteem in plaats van de optie Afsluiten in het Portal voor Azure VM. Met behulp van de Portal Azure een VM afgesloten zorgt ervoor dat de VM te worden opgeheven. Deze actie stelt de VM GenerationID die ongewenste voor een domeincontroller is. Wanneer de VM-GenerationID opnieuw wordt ingesteld, de invocationID van de AD-bibliotheek is ook opnieuw de RID-groep wordt verwijderd en SYSVOL is gemarkeerd als niet-bindend.

## <a name="monitoring-considerations"></a>Overwegingen met betrekking tot controle

Niet te controleren en onderhouden van een netwerk van AD servers kan leiden tot problemen, zoals:

- **Aanmelding mislukt**. Aanmeldingsfout kan optreden in het domein of forest als een vertrouwde relatie of naam oplossing niet werkt of als een globale-catalogusserver universeel groepslidmaatschap kan niet bepalen.

- **Account vergrendelen**. Gebruikers-en kunnen worden uitgesloten als de primaire domeincontroller niet beschikbaar is of replicatie tussen meerdere domeincontrollers mislukt.

- **Domeincontroller mislukt**. Als het station met het bestand Ntds.dit onvoldoende schijfruimte, de domeincontroller niet meer werken.

- **Storing in de toepassing**. Toepassingen die cruciaal voor uw bedrijf, zoals Microsoft Exchange of een andere e-mailtoepassing zijn kunnen mislukken als adresboek query's naar de map niet.

- **Inconsistente Active directory-gegevens**. Replicatie voor een langere periode mislukt, dubbele objecten kunnen worden gemaakt in de map als mogelijk uitgebreide diagnose en tijd uit te schakelen.

- **Account maken is mislukt**. Een domeincontroller is geen gebruikers- of computeraccounts maken als het leidt tot uitputting van de levering van het relatieve id's en de RID-master niet beschikbaar is.

- **Beveiligingsfout beleid**. Als de gedeelde map SYSVOL niet goed wordt gerepliceerd, wordt groepsbeleidsobjecten en beveiligingsbeleid niet goed toegepast op clients.

AD-servers zorgvuldig te bewaken en voorbereid snel corrigerende maatregelen nemen. Een controlelijst maken met taken die moeten worden uitgevoerd na een adequate wachttijd controle. U kan bijvoorbeeld de volgende kritieke taken dagelijks plannen:

- Onderzoeken en oplossen van waarschuwingen gemeld door domeincontrollers 

- Controleer of alle domeincontrollers kunnen communiceren en replicatie werkt.

- Zorg ervoor dat de gedeelde SYSVOL blijft.

- Tijd synchronisatie problemen verhelpen.

- Controleer de schijfruimte op de fysieke schijven die worden gebruikt door AD.

U kunt andere routinetaken minder vaak kan uitvoeren. Bijvoorbeeld kan u controleren de vertrouwensrelaties wekelijks voor verouderde of beschadigde vertrouwensrelaties controleren en controleer of alle domeincontrollers worden uitgevoerd met behulp van de dezelfde servicepacks en patches hot fix maandelijks.

Zie [Active Directory controleren]voor meer informatie[monitoring_ad]. Installatie van hulpprogramma's zoals [Microsoft Systems Center] [ microsoft_systems_center] op de controlerende server (Zie het [Architectuurdiagram][architecture]) om deze taken uit te voeren. 

## <a name="solution-components"></a>Onderdelen van oplossingen

De oplossing, mits voor deze architectuur een beveiligde hybride netwerk, maakt zoals beschreven door de [uitvoering van een beveiligde hybride netwerkarchitectuur in Azure] documenten[ implementing-a-secure-hybrid-network-architecture] en de [uitvoering van een beveiligde hybride netwerkarchitectuur met toegang tot het Internet in Azure][implementing-a-secure-hybrid-network-architecture-with-internet-access], maar met de toevoeging van de volgende items:

- Een Azure brongroep met de naam *basename*- dns-rg, waar *basename* een voorvoegsel dat is u opgeeft bij de implementatie van de oplossing.

- Twee Azure VMs *basename*genoemd-ad1-vm en *basename*-ad2-vm, gemaakt in de resourcegroep *basename*dns - rg. Deze VMs zijn geconfigureerd als AD servers met Directory Services en DNS is geïnstalleerd en geconfigureerd.

- De naam van een NSG *basename*-ad-nsg in de resourcegroep *basename*- ntwk rg Azure. Deze resourcegroep deel uitmaakt van de infrastructuur die de beveiligde hybride netwerk vormen, maar de nieuwe NSG is een toevoeging die binnenkomende regels voor de AD-servers zoals in de volgende tabel worden gedefinieerd:


    Prioriteit|Naam|Bron|Bestemming|Service|Actie|
    --------|----|------|-----------|-------|------|
    170|vnet-port53-|10.0.0.0/16|Alle|Custom(ANY/53)|Toestaan|
    180|vnet-port88-|10.0.0.0/16|Alle|Custom(ANY/88)|Toestaan|
    190|vnet-port135-|10.0.0.0/16|Alle|Custom(ANY/135)|Toestaan|
    200|vnet-naar-port137-9|10.0.0.0/16|Alle|Custom(ANY/137-139)|Toestaan|
    210|vnet-port389-|10.0.0.0/16|Alle|Custom(ANY/389)|Toestaan|
    220|vnet-port464-|10.0.0.0/16|Alle|Custom(ANY/464)|Toestaan|
    230|vnet rpc dynamisch-|10.0.0.0/16|Alle|Custom(ANY/49152-65535)|Toestaan|
    240|onprem-ad-port53|192.168.0.0/24|Alle|Custom(ANY/53)|Toestaan|
    250|onprem-ad-port88|192.168.0.0/24|Alle|Custom(ANY/88)|Toestaan|
    260|onprem-ad-port135|192.168.0.0/24|Alle|Custom(ANY/135)|Toestaan|
    270|onprem-ad-port389|192.168.0.0/24|Alle|Custom(ANY/389)|Toestaan|
    280|onprem-ad-port464|192.168.0.0/24|Alle|Custom(ANY/464)|Toestaan|
    290|Mgmt-rdp toestaan|10.0.0.128/25|Alle|Custom(ANY/3389)|Toestaan|
    300|gateway toestaan|10.0.255.224/27|Alle|Custom(ANY/ANY)|Toestaan|
    310|niet toestaan|10.0.255.192/27|Alle|Custom(ANY/ANY)|Toestaan|
    320|vnet-weigeren|Alle|Alle|Custom(ANY/ANY)|Toestaan|

    AD DS gebruikt poort 53, 89, 135, 389 en 464 binnenkomende replicatie en verificatieverkeer accepteren. In deze tabel wordt de domeincontroller op de locatie in het adres ruimte 192.168.0.0/24 is (uw adresruimte kan variëren - u deze gegevens opgeven als een parameter aan de sjablonen die door de oplossing worden ingezet.

    De NSG definieert ook de volgende regels voor de beveiliging van uitgaande gegevens waarmee u de synchronisatie en -autorisatie verkeer naar het netwerk van gebouwen:

    Prioriteit|Naam|Bron|Bestemming|Service|Actie|
    --------|----|------|-----------|-------|------|
    100|out-port53|Alle|192.168.0.0/24|Custom(ANY/53)|Toestaan|
    110|out-port88|Alle|192.168.0.0/24|Custom(ANY/88)|Toestaan|
    120|out-port135|Alle|192.168.0.0/24|Custom(ANY/135)|Toestaan|
    130|out-port389|Alle|192.168.0.0/24|Custom(ANY/389)|Toestaan|
    140|out-poort 445 toe|Alle|192.168.0.0/24|Custom(ANY/445)|Toestaan|
    150|out-port464|Alle|192.168.0.0/24|Custom(ANY/464)|Toestaan|
    160|out rpc dynamisch|Alle|192.168.0.0/24|Custom(ANY/49152-65535)|Toestaan|

Het script met de oplossing die ook de volgende taken uitvoeren:

- Wordt de *basename*-ad1-vm en *basename*-ad2-servers vm-domeincontrollers aan het domein. U kunt deze servers in de console *Active Directory: gebruikers en Computers* in de domeincontroller op de locatie bekijken:

![[1]][1]

- Een nieuw subnet (10.0.0.0/16) wordt gemaakt voor een AD-site met de naam Azure-VNet-Ad-Site op het domein. Deze site bevat de *basename*-ad1-vm en *basename*-ad2-servers vm. 

- Instellingen voor IP-intersite-transport die het replicatie-interval tussen de site op de gebouwen en de domeincontrollers in de cloud configureren wordt toegevoegd. Hier ziet u de instellingen voor het subnet, sites en transport-instellingen in de console *Active Directory: Sites en Servers* in de domeincontroller op de locatie:

![[2]][2]

## <a name="deployment"></a>Implementatie

De oplossing bestaat uit de volgende prerequsites:

- U uw domein op gebouwen al hebt geconfigureerd en u hebt ingesteld en Routing and Remote Access services ter ondersteuning van een VPN hebt geïnstalleerd de Azure VPN-gateway verbinding maken.


- U kunt de nieuwste versie van de CLI Azure hebt geïnstalleerd. [Volg deze instructies voor meer informatie][cli-install].

- Als u de oplossing van Windows, moet u een hulpprogramma waarmee een bash-shell, zoals [GitHub Desktop]installeren[github-desktop].

>[AZURE.NOTE] Als u geen toegang tot een bestaand op gebouwen-domein, kunt u een testomgeving met behulp van de [onpremdeploy.sh] [ onpremdeploy] bash script. Dit script maakt een VM en een netwerk in de cloud die een instelling zeer elementaire op gebouwen simuleert. Dit script voordat u bewerken en stel de volgende variabelen aan het begin van het bestand gedefinieerd:
>
> - **BASE_NAME**. Een door de gebruiker gedefinieerde voorvoegsel voor de resourcegroep en VM gemaakt door het script. Dit object moet is **niet meer dan 5 tekens** anders die het script probeert te genereren een VM met een ongeldige naam en niet worden.
> 
> - **Abonnement**. Uw abonnement op Azure. De resourcegroep worden in deze suscription worden gemaakt.
> 
> - **Locatie**. De Azure locatie waarin u de resourcegroep, zoals *eastus* of *westus*te maken.
> 
> - **ADMIN_USER_NAME**. De naam van de administrator-account in de VM.
> 
> - **ADMIN_PASSWORD**. Het wachtwoord voor de administrator-account.

Voer de volgende stappen uit om samen te stellen van de monsteroplossing:

1. downloaden en bewerken van de [azuredeploy.sh] [ azuredeploy] script en de volgende parameters instellen aan het begin van het bestand:

    - **BASE_NAME**. Een door de gebruiker gedefinieerde voorvoegsel voor de bronnengroepen en VMs gemaakt door het script. Dit item moet als voorheen **niet meer dan 5 tekens**.

    - **Abonnement**. Uw abonnement op Azure.

    - **OS-type**. Het besturingssysteem (*Windows* of *Linux*) gebruiken voor het web, zakelijke en data access-tier VMs. Houd er rekening mee dat alle AD servers gemaakt door het script uitgevoerd van Windows Server 2012, ongeacht deze instelling.

    - **Domeinnaam**. De naam van het domein op gebouwen. Houd er rekening mee dat als u de omgeving gemaakt door het script onpremdeploy.sh, dit *contoso.com moet*.

    - **Locatie**. De Azure locatie waarin de resourcegroepen maken.

    - **ADMIN_USER_NAME**. De naam van de administrator-accounts in de verschillende VMs.

    - **ADMIN_PASSWORD**. Het wachtwoord voor de administrator-account.

    - **ON_PREMISES_PUBLIC_IP**. Het openbare IP-adres van de machine op locatie VPN.

    - **ON_PREMISES_ADDRESS_SPACE**. De interne adresruimte van het netwerk op gebouwen. Als u de omgeving gemaakt door het onpremdeploy.sh script gebruikt, moet dit 192.168.0.0/16.

    - **VPN_IPSEC_SHARED_KEY**. De IPSec-gedeelde sleutel die wordt gebruikt voor het maken van de VPN-verbinding tussen het netwerk van gebouwen en de Azure VPN-gateway.

    - **ON_PREMISES_DNS_SERVER_ADDRESS**. Het IP-adres van de DNS-server op locatie. Als u de omgeving gemaakt door het onpremdeploy.sh script gebruikt, moet dit 192.168.0.4

    - **ON_PREMISES_DNS_SUBNET_PREFIX** Het adresvoorvoegsel van het subnet op gebouwen. Als u de omgeving gemaakt door het onpremdeploy.sh script gebruikt, moet dit 192.168.0.0/24.

    >[AZURE.NOTE] OM tijd en middelen besparen, maakt het script de toegang tot bedrijfs- of lagen. Als u deze items nodig hebt, kunt u de volgende sectie in het script azuredeploy.sh opmerkingen bij:
    >
    >
    > ```
    > #### # create biz tier
    > #### TEMPLATE_URI=${URI_BASE}/ARMBuildingBlocks/Templates/bb-ilb-backend-http-https.json
    > #### SUBNET_NAME_PREFIX=${DEPLOYED_BIZ_SUBNET_NAME_PREFIX}
    > #### ILB_IP_ADDRESS=${BIZ_ILB_IP_ADDRESS}
    > #### NUMBER_VMS=${BIZ_NUMBER_VMS}
    > #### 
    > #### RESOURCE_GROUP=${BASE_NAME}-${SUBNET_NAME_PREFIX}-tier-rg
    > #### VM_NAME_PREFIX=${SUBNET_NAME_PREFIX}
    > #### VM_COMPUTER_NAME_PREFIX=${SUBNET_NAME_PREFIX}
    > #### VNET_RESOURCE_GROUP=${NTWK_RESOURCE_GROUP}
    > #### VNET_NAME=${DEPLOYED_VNET_NAME}
    > #### SUBNET_NAME=${DEPLOYED_BIZ_SUBNET_NAME}
    > #### PARAMETERS="{\"baseName\":{\"value\":\"${BASE_NAME}\"},\"vnetResourceGroup\":{\"value\":\"${VNET_RESOURCE_GROUP}\"},\"vnetName\":{\"value\":\"${VNET_NAME}\"},\"subnetName\":{\"value\":\"${SUBNET_NAME}\"},\"adminUsername\":{\"value\":\"${ADMIN_USER_NAME}\"},\"adminPassword\":{\"value\":\"${ADMIN_PASSWORD}\"},\"subnetNamePrefix\":{\"value\":\"${SUBNET_NAME_PREFIX}\"},\"ilbIpAddress\":{\"value\":\"${ILB_IP_ADDRESS}\"},\"osType\":{\"value\":\"${OS_TYPE}\"},\"numberVMs\":{\"value\":${NUMBER_VMS}},\"vmNamePrefix\":{\"value\":\"${VM_NAME_PREFIX}\"},\"vmComputerNamePrefix\":{\"value\":\"${VM_COMPUTER_NAME_PREFIX}\"}}"
    > #### 
    > #### echo
    > #### echo
    > #### echo azure group create --name ${RESOURCE_GROUP} --location ${LOCATION} --subscription ${SUBSCRIPTION}
    > ####      azure group create --name ${RESOURCE_GROUP} --location ${LOCATION} --subscription ${SUBSCRIPTION}
    > #### echo
    > #### echo
    > #### echo azure group deployment create --template-uri ${TEMPLATE_URI} -g ${RESOURCE_GROUP} -p ${PARAMETERS} --subscription ${SUBSCRIPTION}
    > ####      azure group deployment create --template-uri ${TEMPLATE_URI} -g ${RESOURCE_GROUP} -p ${PARAMETERS} --subscription ${SUBSCRIPTION}
    > #### 
    > #### # create db tier
    > #### TEMPLATE_URI=${URI_BASE}/ARMBuildingBlocks/Templates/bb-ilb-backend-http-https.json
    > #### SUBNET_NAME_PREFIX=${DEPLOYED_DB_SUBNET_NAME_PREFIX}
    > #### ILB_IP_ADDRESS=${DB_ILB_IP_ADDRESS}
    > #### NUMBER_VMS=${DB_NUMBER_VMS}
    > #### 
    > #### RESOURCE_GROUP=${BASE_NAME}-${SUBNET_NAME_PREFIX}-tier-rg
    > #### VM_NAME_PREFIX=${SUBNET_NAME_PREFIX}
    > #### VM_COMPUTER_NAME_PREFIX=${SUBNET_NAME_PREFIX}
    > #### VNET_RESOURCE_GROUP=${NTWK_RESOURCE_GROUP}
    > #### VNET_NAME=${DEPLOYED_VNET_NAME}
    > #### SUBNET_NAME=${DEPLOYED_DB_SUBNET_NAME}
    > #### PARAMETERS="{\"baseName\":{\"value\":\"${BASE_NAME}\"},\"vnetResourceGroup\":{\"value\":\"${VNET_RESOURCE_GROUP}\"},\"vnetName\":{\"value\":\"${VNET_NAME}\"},\"subnetName\":{\"value\":\"${SUBNET_NAME}\"},\"adminUsername\":{\"value\":\"${ADMIN_USER_NAME}\"},\"adminPassword\":{\"value\":\"${ADMIN_PASSWORD}\"},\"subnetNamePrefix\":{\"value\":\"${SUBNET_NAME_PREFIX}\"},\"ilbIpAddress\":{\"value\":\"${ILB_IP_ADDRESS}\"},\"osType\":{\"value\":\"${OS_TYPE}\"},\"numberVMs\":{\"value\":${NUMBER_VMS}},\"vmNamePrefix\":{\"value\":\"${VM_NAME_PREFIX}\"},\"vmComputerNamePrefix\":{\"value\":\"${VM_COMPUTER_NAME_PREFIX}\"}}"
    > #### 
    > #### echo
    > #### echo
    > #### echo azure group create --name ${RESOURCE_GROUP} --location ${LOCATION} --subscription ${SUBSCRIPTION}
    > ####      azure group create --name ${RESOURCE_GROUP} --location ${LOCATION} --subscription ${SUBSCRIPTION}
    > #### echo
    > #### echo
    > #### echo azure group deployment create --template-uri ${TEMPLATE_URI} -g ${RESOURCE_GROUP} -p ${PARAMETERS} --subscription ${SUBSCRIPTION}
    > ####      azure group deployment create --template-uri ${TEMPLATE_URI} -g ${RESOURCE_GROUP} -p ${PARAMETERS} --subscription ${SUBSCRIPTION}
    > ```

2. Een bash shell-prompt te openen en verplaatsen naar de map met het script azuredeploy.sh.

3. Log in op uw account Azure. Voer de volgende opdracht in de shell bash:

    ```
    azure login
    ```

    Volg de instructies om verbinding met Azure.

4. Voer de opdracht `./azuredeploy.sh`, en wacht terwijl het script wordt gemaakt van de netwerkinfrastructuur.

5. Achter de prompt *Controleer of de VNet is gemaakt*, de Azure portal te gebruiken om te controleren dat een groep met de naam *basename*- ntwk-rg is gemaakt en bevat items die overeenkomen met die in de volgende afbeelding wordt getoond:

    ![[3]][3]

    >[AZURE.NOTE] *Basename* is ingesteld op *wolk* in de voorbeelden weergegeven wanneer het script wordt uitgevoerd.

    *Basename*- vnet VNet Klik op *subnetten*op en controleer of de onderstaande subnetten zijn gemaakt:

    ![[4]][4]

6. Druk op een toets op de opdrachtregel in het venster van de bash-shell, en geduld terwijl de weblaag en taakverdeling worden gemaakt.

7. Achter de prompt *Controleer of de weblaag correct is gemaakt*, de Azure portal te gebruiken om te controleren dat een resourcegroep *basename*web tier rg genoemd is gemaakt en dat deze vergelijkbaar met de onderstaande items bevat:

    ![[5]][5]

8. Druk op een toets op de opdrachtregel in het venster van de bash-shell, en is bezig met het NVAs worden gemaakt.

9. Achter de prompt *Controleer of de NVA correct is gemaakt*, de Azure portal te gebruiken om te controleren dat een resourcegroep *basename*- mgmt-rg genoemd is gemaakt met de volgende inhoud:

    ![[6]][6]

10. Druk op een toets op de opdrachtregel in het venster van de bash-shell, en wachten terwijl de jumpbox wordt gemaakt.

11. Achter de prompt *Controleer of de jumpbox correct is gemaakt*, de Azure portal te gebruiken om te controleren dat de volgende items zijn toegevoegd aan de resourcegroep *basename*- mgmt rg:

    - Een set beschikbaarheid genoemd *basename*- jb-als.

    - De naam van een VM - jb *basename*-vm.

    - Een netwerkinterface genoemd *basename*- jb-NIC

12. Druk op een toets op de opdrachtregel in het venster van de bash-shell, en geduld terwijl de Azure VPN-gateway en een verbinding worden gemaakt. Houd er rekening mee dat deze stap duurt maximaal 30 minuten.

13. Achter de prompt *Controleer of de VPN-gateway correct is gemaakt*, de Azure portal te gebruiken om te controleren dat de volgende items zijn toegevoegd aan de resourcegroep *basename*- ntwk rg:

    - Een lokaal netwerkgateway op de gebouwen lgw genoemd.
    
    - Een virtueel netwerkgateway *basename*- vpngw genoemd.

    - Een gateway verbinding met de naam *basename*- vnet-vpnconn. Houd er rekening mee dat de status van deze verbinding is mogelijk *niet aangesloten* als u nog niet hebt het einde op de bedrijfsruimten van de verbinding; u heeft betrekking op dit later.

14. Druk op een toets op de opdrachtregel in het venster van de bash-shell, en is bezig met het VMs en andere bronnen voor de DMZ worden gemaakt.

15. Achter de prompt *Controleer of de DMZ correct is gemaakt*, de Azure portal te gebruiken om te controleren dat een resourcegroep *basename*-dmz-rg genoemd is gemaakt met de volgende inhoud:

    ![[7]][7]

16. Achter de prompt, in de bash-shell venster, druk op een toets. De volgende vragen moeten worden weergegeven:

    ```text
    Manual Step...

    Please configure your on-premises network to connect to the Azure VNet

    Make sure that you can connect to the on-premises AD server from the Azure VMs
    ```

    Log in op uw computer op de locatie die is verbonden met de Azure-gateway en de verbinding op de juiste wijze configureren. Statische routes toevoegen aan de op het bedrijf gateway-apparaat dat zorgt ervoor het adresbereik 10.0.0.0/16 via de gateway aan de VNet requestsfor dat. De stappen om dit te doen hangt af van hoe u verbinding maakt. [Implementatie van een hybride netwerkarchitectuur met Azure en VPN-On-premises] Zie[ implementing-a-hybrid-network-architecture-with-vpn] voor meer informatie.

    Houd er rekening mee dat u het openbare IP-adres van de gateway VPN-Azure vinden kunt met behulp van de portal Azure te onderzoeken van de gateway *basename*- vpngw in de resourcegroep *basename*- ntwk rg:

    ![[8]][8]

    U kunt bepalen of de verbinding correct is gemaakt door te kijken naar de status van de verbinding *basename*- vnet-vpnconn. Deze moet worden ingesteld op *verbonden*.

    Open het om verbinding te testen, een verbinding met extern bureaublad naar de jumpbox (10.0.0.254 beschikbaar) van een computer die zich in uw netwerk op gebouwen.

17. Achter de prompt, in de bash-shell venster, druk op een toets. Op de volgende vragen, *Druk op een toets voor het bijwerken van de instelling van de VNet voor de VNet om te verwijzen naar de DNS van gebouwen*, op een toets drukt en de DNS-instellingen voor de VNet zijn bijgewerkt met de waarde die u hebt opgegeven als de parameter **ON_PREMISES_DNS_SERVER_ADDRESS** in het script azuredeploy.sh.

18. Achter de prompt, *Controleer of de DNS-server de instelling op de VNet is bijgewerkt*, controleert de instelling van de *DNS-servers* van de *basename*- vnet VNet in de resourcegroep *basename*- ntwk rg via de Azure portal. Deze moet worden ingesteld op de *Aangepaste DNS*en het *primaire DNS-server* moet het adres van uw DNS-server op locatie:

    ![[9]][9]

19. Achter de prompt *Druk op een toets om de bronnengroep voor de AD-servers maken* in het venster bash shell op een toets drukt en wacht terwijl de resourcegroep voor het houden van de AD-servers in de cloud wordt gemaakt.

20. Achter de prompt *Druk op een toets voor het maken van de VMs de AD-servers* in het venster bash shell op een toets drukt en wachten op het VMs kan worden gemaakt en toegevoegd aan de resourcegroep.

21. Wanneer de *Druk op een toets als u wilt deelnemen aan het VMs aan het domein op gebouwen* wordt weergegeven, gaat u naar de portal Azure en controleer of de groep *basename*- dns-rg is gemaakt en bevat twee VMS (*basename*-ad1-vm en *basename*-ad2-vm):

    ![[10]][10]

22. In de resourcegroep *basename*- ntwk rg controle is een NSG gemaakt *basename*genoemd-ad-nsg. Bekijk de regels voor binnenkomende en uitgaande voor deze NSG. Ze moeten overeenkomen met die welke zijn vermeld in de tabellen van de [onderdelen van de oplossing] [ solution-components] sectie.

23. Druk op een toets op de opdrachtregel in het venster van de bash-shell, en is bezig met het VMs worden toegevoegd aan het domein op gebouwen.

24. Op de *Ga je naar de lokalen op Active Directory-server om te controleren of de computers zijn toegevoegd aan de domeinen* vragen, aansluiten op uw computer op gebouwen en het gebruik van de console *Active Directory: gebruikers en Computers* om te controleren of beide VMs zijn toegevoegd aan het domein:

    ![[11]][11]

25. Druk op een toets op de opdrachtregel in het venster van de bash-shell, en wacht totdat de replicatie AD site is gemaakt in het domein.

26. Op de *Ga je naar de lokalen op Active Directory-server om te controleren of de replicatie-site is gemaakt* vragen, gebruikt u de console *Active Directory: Sites en Services* om te controleren dat een replicatie-site met de naam *Azure-Vnet-Ad-Site* is gemaakt, samen met een IP-intersite-transport maken en een subnet dat verwijst naar de VNet en *AzureToOnpremLink*:

    ![[12]][12]

27. Druk op een toets op de opdrachtregel in het venster van de bash-shell, en het script Directory en DNS installeert op elk van de VMs AD.

28. De vraag *moet aanmelden bij elke Azure Active Directory-server om te controleren of Directory Services zijn geconfigureerd* , opent een verbinding met extern bureaublad vanaf een computer op ruimten aan de jumpbox (*basename*- jb-vm) en vervolgens een andere verbinding met extern bureaublad openen vanaf de jumpbox naar de eerste AD-server (*basename*-ad1-vm). Meld u aan met de **domeinnaam**, **ADMIN_USER_NAME**en **ADMIN_PASSWORD** die u hebt opgegeven in het script azuredeploy.sh. Met behulp van Serverbeheer, Controleer of de rollen AD DS en DNS-beide zijn toegevoegd. Herhaal dit proces voor de tweede AD-server (*basename*-ad2-vm).

29. Achter de prompt, in de bash-shell venster, druk op een toets. Wanneer de prompt *Druk op een toets om te verwijzen naar de DNS-server in Azure de Azure VNet DNS-instellingen* wordt weergegeven, drukt u op een toets en toestaan dat het script voor het bijwerken van de DNS-instellingen voor de VNet.

30. Wanneer de prompt *Controleer dat de VNet DNS instelling is bijgewerkt referentie de Azure VM DNS servers* verschijnt, gebruik van het Azure portal de instelling van de *DNS-Servers* van de *basename*- vnet VNet in de resourcegroep *basename*- ntwk rg. De primaire en secundaire DNS-servers moeten nu verwijst naar de twee AD VMs:

    ![[13]][13]

31. Elk van de VMs AD starten voordat u verdergaat. Deze stap is noodzakelijk om ervoor te zorgen dat ze elke de juiste DNS-instellingen bij Azure halen. Wacht totdat beide VMs worden uitgevoerd voordat u verdergaat.

32. Achter de prompt, in de bash-shell venster, druk op een toets. Druk op een toets achter de volgende prompt *Druk op een toets om toe te passen, de gateway UDR naar het subnet gateway is (deze is verwijderd)*, en het script voor het vernieuwen van de gateway UDR toestaan.

33. Controleer of het script is voltooid.

## <a name="next-steps"></a>Volgende stappen

- Informatie over de aanbevolen procedures voor het [maken van een forest met bronnen toevoegen] [ adds-resource-forest] in Azure.

- Informatie over de aanbevolen procedures voor het [maken van een AD FS-infrastructuur] [ adfs] in Azure.

<!-- links -->

[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[adfs]: ./guidance-identity-adfs.md
[guidance-vpn-gateway]: ./guidance-hybrid-network-vpn.md
[adds-resource-forest]: ./guidance-identity-adds-resource-forest.md
[script]: #sample-solution-script
[implementing-a-multi-tier-architecture-on-Azure]: ./guidance-compute-3-tier-vm.md
[implementing-a-secure-hybrid-network-architecture-with-internet-access]: ./guidance-iaas-ra-secure-vnet-dmz.md
[implementing-a-secure-hybrid-network-architecture]: ./guidance-iaas-ra-secure-vnet-hybrid.md
[implementing-a-hybrid-network-architecture-with-vpn]: ./guidance-hybrid-network-vpn.md
[active-directory-domain-services]: https://technet.microsoft.com/library/dd448614.aspx
[active-directory-federation-services]: https://technet.microsoft.com/windowsserver/dd448613.aspx
[azure-active-directory]: ../active-directory-domain-services/active-directory-ds-overview.md
[azure-ad-connect]: ../active-directory/active-directory-aadconnect.md
[architecture]: #architecture_diagram
[security-considerations]: #security-considerations
[recommendations]: #recommendations
[azure-vpn-gateway]: https://azure.microsoft.com/documentation/articles/vpn-gateway-about-vpngateways/
[azure-expressroute]: https://azure.microsoft.com/documentation/articles/expressroute-introduction/
[claims-aware applications]: https://msdn.microsoft.com/en-us/library/windows/desktop/bb736227(v=vs.85).aspx
[active-directory-federation-services-overview]: https://technet.microsoft.com/en-us/library/hh831502(v=ws.11).aspx
[capacity-planning-for-adds]: http://social.technet.microsoft.com/wiki/contents/articles/14355.capacity-planning-for-active-directory-domain-services.aspx
[ad-ds-ports]: https://technet.microsoft.com/library/dd772723(v=ws.11).aspx
[where-to-place-an-fs-proxy]: https://technet.microsoft.com/library/dd807048(v=ws.11).aspx
[powershell-ad]: https://technet.microsoft.com/en-us/library/ee617195.aspx
[ad_network_recommendations]: #network_configuration_recommendations_for_AD_DS_VMs
[domain_and_forests]: https://technet.microsoft.com/library/cc759073(v=ws.10).aspx
[best_practices_ad_password_policy]: https://technet.microsoft.com/magazine/ff741764.aspx
[monitoring_ad]: https://msdn.microsoft.com/library/bb727046.aspx
[microsoft_systems_center]: https://www.microsoft.com/server-cloud/products/system-center-2016/
[cli-install]: https://azure.microsoft.com/documentation/articles/xplat-cli-install
[github-desktop]: https://desktop.github.com/
[sssd-and-active-directory]: https://help.ubuntu.com/lts/serverguide/sssd-ad.html
[set-a-static-ip-address]: https://azure.microsoft.com/documentation/articles/virtual-networks-static-private-ip-arm-pportal/
[ad-azure-guidelines]: https://msdn.microsoft.com/library/azure/jj156090.aspx
[adds-data-disks]: https://msdn.microsoft.com/library/azure/jj156090.aspx#BKMK_PlaceDB
[AD-FSMO-recommendations]: #active_directory_FSMO_placement_recommendations
[AD-FSMO-roles-in-windows]: https://support.microsoft.com/en-gb/kb/197132
[standby-operations-masters]: https://technet.microsoft.com/library/cc794737(v=ws.10).aspx
[transfer-FSMO-roles]: https://technet.microsoft.com/library/cc816946(v=ws.10).aspx
[view-fsmo-roles]: https://technet.microsoft.com/library/cc816893(v=ws.10).aspx
[read-only-dc]: https://technet.microsoft.com/library/cc732801(v=ws.10).aspx
[AD-sites-and-subnets]: https://blogs.technet.microsoft.com/canitpro/2015/03/03/step-by-step-setting-up-active-directory-sites-subnets-site-links/
[sites-overview]: https://technet.microsoft.com/library/cc782048(v=ws.10).aspx
[implementing-adfs]: ./guidance-iaas-ra-secure-vnet-adfs.md
[onpremdeploy]: https://github.com/mspnp/blueprints/blob/master/ARMBuildingBlocks/guidance-iaas-ra-ad-extension/onpremdeploy.sh
[azuredeploy]: https://github.com/mspnp/blueprints/blob/master/ARMBuildingBlocks/guidance-iaas-ra-ad-extension/azuredeploy.sh
[solution-components]: #solution_components

[0]: ./media/guidance-iaas-ra-secure-vnet-ad/figure1.png "Hybride netwerkarchitectuur met Active Directory beveiligen"
[1]: ./media/guidance-iaas-ra-secure-vnet-ad/figure2.png "De aanbieding van de twee Azure VMs als servers console van Active Directory: gebruikers en Computers"
[2]: ./media/guidance-iaas-ra-secure-vnet-ad/figure3.png "Active Directory: Sites en Services-console met de replicatie-instellingen voor de site in de cloud"
[3]: ./media/guidance-iaas-ra-secure-vnet-ad/figure4.png "De inhoud van de resourcegroep basename-ntwk-rg"
[4]: ./media/guidance-iaas-ra-secure-vnet-ad/figure5.png "De subnetten in het basename vnet VNet"
[5]: ./media/guidance-iaas-ra-secure-vnet-ad/figure6.png "De items in de weblaag"
[6]: ./media/guidance-iaas-ra-secure-vnet-ad/figure7.png "De NVAs in de resourcegroep basename-mgmt-rg"
[7]: ./media/guidance-iaas-ra-secure-vnet-ad/figure8.png "De resources in de resourcegroep basename-dmz-rg"
[8]: ./media/guidance-iaas-ra-secure-vnet-ad/figure9.png "Het openbare IP-adres van de gateway VPN-Azure zoeken"
[9]: ./media/guidance-iaas-ra-secure-vnet-ad/figure10.png "De instellingen van de DNS-server voor de * basename *-vnet, VNet"
[10]: ./media/guidance-iaas-ra-secure-vnet-ad/figure11.png "De * basename *-dns-rg bronnengroep waarin de AD-server VMs"
[11]: ./media/guidance-iaas-ra-secure-vnet-ad/figure12.png "De aanbieding van de AD-server VMs als leden van het domein console van Active Directory: gebruikers en Computers"
[12]: ./media/guidance-iaas-ra-secure-vnet-ad/figure13.png "Active Directory: Sites en Services-console met de site replication voor Azure AD servers"
[13]: ./media/guidance-iaas-ra-secure-vnet-ad/figure14.png "De DNS-serverinstellingen voor de basename vnet VNet verwijst naar de AD-servers in de cloud"