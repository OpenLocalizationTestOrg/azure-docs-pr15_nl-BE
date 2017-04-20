<properties
   pageTitle="Implementeren van Azure Active Directory | Microsoft Azure"
   description="Het implementeren van een beveiligde hybride netwerkarchitectuur met Azure Active Directory."
   services="guidance,virtual-network,active-directory"
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
   ms.date="09/28/2016"
   ms.author="telmos"/>

# <a name="implementing-azure-active-directory"></a>Implementeren van Azure Active Directory

[AZURE.INCLUDE [pnp-RA-branding](../../includes/guidance-pnp-header-include.md)]

Dit artikel bevat aanbevolen procedures voor het integreren van lokalen Active Directory (AD) domeinen en forests met Azure Active Directory om identiteitsverificatie cloud-gebaseerde.

> [AZURE.NOTE] Azure heeft twee verschillende modellen: [Resource Manager] [ resource-manager-overview] en klassiek. Deze referentiearchitectuur gebruikt resourcemanager, wordt aangeraden voor nieuwe implementaties.

U kunt services directory en identiteit, zoals omschreven door AD Directory Services (AD DS) om identiteiten te verifiëren. Deze identiteiten kunnen behoren tot gebruikers, computers, toepassingen en andere bronnen die deel uitmaken van een beveiligingsgrens. U kunt host directory kan en services-identiteit op lokalen, maar in een hybride scenario waar de elementen van een toepassing in Azure bevinden zich efficiënter uit te breiden deze functionaliteit in de cloud. Deze benadering kan helpen verminderen de vertraging veroorzaakt door het verzenden van verificatie en lokale vergunning aanvragen uit de cloud terug naar de directory en de services die worden uitgevoerd op gebouwen.

Azure biedt twee oplossingen voor de uitvoering van en de directory-services in de cloud:

1. U kunt [Azure Active Directory (AAD)] [ azure-active-directory] een AD-domein in de cloud maken en koppelen aan een bedrijf op AD-domein. AAD kunt u eenmalige aanmelding (SSO) configureren voor gebruikers die werken met toepassingen die toegankelijk is via de cloud. [Azure AD Connect] maakt gebruik van AAD[ azure-ad-connect] uitgevoerd in ruimten voor het repliceren van identiteiten en objecten in de opslagplaats van gebouwen gehouden naar de cloud.

    U kunt ook AAD implementeren zonder gebruik van een directory op gebouwen. In dit geval fungeert AAD als de primaire bron van alle identiteitsgegevens in plaats van met gegevens die zijn gerepliceerd vanuit een map op de locatie.

    Opmerking: de map in de cloud is **niet** een uitbreiding van een directory op gebouwen. In plaats daarvan is een exemplaar met dezelfde identiteiten en objecten. Wijzigingen in deze artikelen op gebouwen worden gekopieerd naar de cloud, maar wijzigingen aangebracht de cloud **niet** weer worden gerepliceerd naar het domein op gebouwen.

    >[AZURE.NOTE] De Azure Active Directory Premium-versies ondersteunen de terugneming van de wachtwoorden van gebruikers, zodat uw gebruikers op gebouwen uit te voeren zelf opnieuw instellen van wachtwoorden in de cloud. Dit is de enige informatie die AAD worden gesynchroniseerd naar de map op locatie. Zie voor meer informatie over de verschillende edities van hun functies en AAD [Azure Active Directory edities][aad-editions].

2. U kunt een VM AD DS wordt uitgevoerd als een domeincontroller in Azure, het uitbreiden van uw bestaande AD-infrastructuur (inclusief AD DS en AD FS) implementeren van uw datacenter in ruimten. Deze benadering is het meest voorkomende scenario's waarbij het netwerk op gebouwen en Azure virtueel netwerk zijn verbonden door een verbinding van VPN-en/of ExpressRoute. Deze oplossing biedt ook ondersteuning voor bidirectionele replicatie waarmee u wijzigingen aanbrengen in de cloud en in ruimten waar het meest geschikt is.

Deze architectuur is gericht op oplossing 1. Zie voor meer informatie over de tweede oplossing [Active Directory uitbreiden naar Azure][guidance-adds].

Typische use-cases voor deze architectuur zijn:

- SSO biedt voor eindgebruikers met SaaS en andere toepassingen in de cloud, met dezelfde referenties die zij opgeven voor toepassingen voor gebouwen.

- Publiceren op-premises webtoepassingen via de cloud voor toegang tot externe gebruikers in uw organisatie.

- Self-service mogelijkheden voor eindgebruikers, zoals hun wachtwoorden opnieuw instellen en overdragen groepsbeheer implementeren. 

    >[AZURE.NOTE] Deze mogelijkheden kunnen worden beheerd door een beheerder via Groepsbeleid.

- Situaties waarin de netwerk op gebouwen en Azure virtueel netwerk cloud-toepassingen die als host fungeert niet rechtstreeks zijn gekoppeld met behulp van een VPN-tunnel of ExpressRoute circuit.

Houd er rekening mee dat AAD niet in de functionaliteit van AD voorziet. Bijvoorbeeld: AAD momenteel verwerkt alleen verificatie van de gebruiker in plaats van verificatie. Sommige toepassingen en services, zoals SQL Server, kunnen verificatie vereisen deze oplossing is in dat geval niet geschikt. AAD mogelijk ook niet geschikt voor systemen waarbij onderdelen over de grens op-ruimten/cloud kunnen migreren in plaats van alleen maar te kopiëren.

> [AZURE.NOTE] Bekijk voor een gedetailleerde uitleg van de werking van Azure Active Directory, [Microsoft Active Directory beschreven][aad-explained].

## <a name="architecture-diagram"></a>Architectuurdiagram

In het volgende diagram worden de belangrijke onderdelen van deze architectuur. Lees voor meer informatie over de elementen van de werkbelasting in Azure, [VMs uitvoeren voor een N-lagige architectuur op Azure][implementing-a-multi-tier-architecture-on-Azure]:

> [AZURE.NOTE] Eenvoud, dit diagram alleen de verbindingen die rechtstreeks verband houden met AAD en heeft geen omleidingen van web browser verzoek weer of voor andere protocol-gerelateerde verkeer die kan optreden als onderdeel van de federation-verificatie en de identiteit. Bijvoorbeeld: een gebruiker (op de bedrijfsruimten of extern) wordt meestal een web app via een browser en de web app kan de browser voor verificatie van de aanvraag via AAD transparant leiden. Zodra u geverifieerd, kan de aanvraag terug naar de web app met de juiste identiteitsgegevens worden doorgegeven.

[! [0]][0]

- **De huurder azure Active Directory**. Dit is een exemplaar van AAD die door uw organisatie zijn gemaakt. Het fungeert als een eenvoudige adreslijstservice voor cloud-toepassingen (het bevat objecten die zijn gekopieerd uit de ruimten op AD), en biedt services identiteit.

- **Web tier subnet**. Dit subnet bevat VMs die een aangepaste cloud-gebaseerde toepassing die is ontwikkeld door uw organisatie en voor welke AAD als een broker identiteit fungeren kan te implementeren.

- **On-premises AD DS-server**. Uw organisatie waarschijnlijk al een bestaande directoryservice, zoals AD DS. U kunt veel van de items in uw AD DS-map (zoals gebruikers- en groepsinformatie) met AAD AAD deze informatie gebruikt om identiteiten te verifiëren om synchroniseren.

- **Synchronisatieserver azure AD verbinden**. Dit is een computer op de ruimten die de Azure AD verbinden sync-service wordt uitgevoerd. U installeren deze service met behulp van de software Azure AD verbinden. De synchronisatieservice Azure AD Connect worden gesynchroniseerd informatie (gebruikers, groepen, contactpersonen, enz.) in de lokalen op AD naar AAD in de cloud. Bijvoorbeeld, kunt u groepen en gebruikers op-gebouwen deprovision of inrichten en deze wijzigingen worden doorgegeven aan AAD. De synchronisatieservice Azure AD verbinden geeft informatie aan de huurder AAD.

    >[AZURE.NOTE] Omwille van de beveiliging worden van wachtwoorden niet rechtstreeks opgeslagen in AAD. In plaats daarvan bevat AAD een hash elk wachtwoord. Dit is voldoende om te controleren of het wachtwoord van een gebruiker. Als een gebruiker een wachtwoord opnieuw instellen, moet dit worden uitgevoerd op gebouwen en de nieuwe hash verzonden naar AAD. AAD Premium editions beschikken over functies waarmee deze taak als u wilt dat gebruikers hun wachtwoorden opnieuw instellen.

## <a name="recommendations"></a>Aanbevelingen

Deze sectie bevat een overzicht van aanbevelingen voor AAD als volgt implementeren.

- AD-verbinding
- Beveiliging

### <a name="azure-ad-connect-sync-service-recommendations"></a>Azure AD verbinden sync service-aanbevelingen

Synchronisatie is ervoor te zorgen dat de identiteitsgegevens van gebruikers in de wolk consistent met gehouden in gebouwen is betrokken. Deze consistentie is het doel van de synchronisatieservice Azure AD verbinden. De volgende punten samenvatten aanbevelingen voor de implementatie van de synchronisatieservice Azure AD verbinden:

- Voordat u sync Azure AD Connect implementeert, moet u de synchronisatie-behoeften van uw organisatie bepalen (wat u wilt synchroniseren, uit welke domeinen, en hoe vaak. Het artikel [bepalen directory synchronisatie eisen] [ aad-sync-requirements] beschrijving van de punten waarmee u rekening moet houden.

- De synchronisatieservice van Azure AD verbinding maken met een VM kan worden uitgevoerd of een computer staan op gebouwen. Afhankelijk van de volatiliteit van de gegevens in de map van uw advertentie, nadat de initiële synchronisatie met AAD is uitgevoerd is de belasting van de synchronisatieservice Azure AD verbinden onwaarschijnlijk hoog. Een VM kunt u gemakkelijker de server (monitor de activiteit, zoals beschreven in de sectie [Overwegingen voor controle](#monitoring-considerations) om te bepalen of dit nodig is).

- Als er meerdere op ruimten domeinen in een forest, kunt u opslaan en synchroniseren van gegevens voor het hele forest met een enkel AAD huurder (dit is de aanbevolen werkwijze). Filteren van gegevens voor identiteiten die in meer dan één domein optreden zodat elke identiteit wordt slechts één keer in de AAD in plaats van te dupliceren weergegeven als dit tot inconsistenties leiden kan wanneer gegevens worden gesynchroniseerd. Hiervoor zijn meerdere Azure AD verbinden synchronisatie servers implementeren. Zie voor meer informatie het meerdere AAD scenario in de sectie [topologie overwegingen](#topology-considerations) . 

- Met filteren kunt u beperken in AAD opgeslagen met alleen die gegevens die nodig zijn. Uw organisatie kan bijvoorbeeld niet voor het opslaan van informatie over niet-actief of niet-persoonlijke accounts in AAD wilt. Filteren kan groep, domein, organisatie-eenheid of kenmerk, en u kunt filters voor het genereren van complexere regels combineren. Zo kunt u selecteren alleen objecten die in een domein met een specifieke waarde in een geselecteerde kenmerk te synchroniseren. Zie voor meer informatie [sync Azure AD verbinden: filter configureren][aad-filtering].

- Voor het implementeren van hoge beschikbaarheid voor de synchronisatieservice AD verbinden, een secundaire staging-server worden uitgevoerd. Zie voor meer informatie de sectie [topologie overwegingen](#topology-considerations) .

### <a name="security-recommendations"></a>Aanbevelingen voor beveiliging

De volgende items vatten de primaire beveiligingsaanbevelingen voor de implementatie van AAD, afhankelijk van de vereisten van uw organisatie:

- Wachtwoorden van gebruikers beheren.

    Als u een premium edition van AAD, kunt u wachtwoord terugneming van AAD naar de map op locatie door het uitvoeren van een aangepaste installatie van Azure AD verbinden:

    [! [9]][9]

    Deze functie kan gebruikers hun wachtwoorden uit binnen de portal Azure opnieuw, maar moet alleen worden ingeschakeld nadat u hebt bekeken, wachtwoord voor beveiligingsbeleid van uw organisatie. Bijvoorbeeld, kunt u beperken welke gebruikers hun wachtwoord kunnen wijzigen en u kunt het wachtwoord management-ervaring op maat maken. Zie voor meer informatie, [Wachtwoordbeheer aanpassen aan de behoeften van uw organisatie][aad-password-management].

- Het onderhouden van beveiliging voor toepassingen voor ruimten die extern kunnen worden benaderd.

    Gebruik de Azure AD toepassingsproxy voor gecontroleerde toegang tot webtoepassingen voor bedrijven van externe gebruikers via AAD. Deze aanpak beschermt uw toepassingen door dat ze niet rechtstreeks met het Internet. Alleen gebruikers met geldige referenties in uw directory Azure kunnen bereiken van uw toepassingen. Zie voor meer informatie het artikel [Toepassingsproxy inschakelen in Azure portal][aad-application-proxy].

- AAD nauwlettend op tekenen van verdachte activiteiten in de gaten.

    Overweeg het gebruik van AAD Premium P2 edition, inclusief identiteitsbeveiliging AAD. Bescherming van de identiteit wordt geavanceerde machine learning algoritmen en methodiek anomalieën en risico-gebeurtenissen die erop wijzen kunnen dat een identiteit is ingebroken. Het kan bijvoorbeeld mogelijk afwijkende activiteit zoals onregelmatige-in activiteiten, aanmeldingen van onbekende bronnen of van IP-adressen met verdachte activiteiten of aanmeldingen van apparaten die kunnen worden geïnfecteerd detecteren. Met behulp van deze gegevens, bescherming van identiteiten genereert rapporten en waarschuwingen, waarmee u deze risico's onderzoeken en passend herstel of beperking actie ondernemen. Zie voor meer informatie, [Azure Active Directory identiteitsbeveiliging][aad-identity-protection].

    U kunt de rapportagefunctie van AAD in Azure portal ook verdachte en andere aan beveiliging gerelateerde activiteiten die binnen het systeem controleren. AAD kan een serie samenvattende rapporten genereren:

    [! [17]][17]

    Zie voor meer informatie over het gebruik van deze rapporten [Azure Active Directory Reporting Guide][aad-reporting-guide].

## <a name="topology-considerations"></a>Topologie overwegingen

Als u een map op het bedrijf met AAD integreert, configureert u Azure AD verbinden om een topologie die het meest overeenkomt met de vereisten van uw organisatie implementeren. Topologieën die Azure AD Connect ondersteunt zijn de volgende:

- **Één forest, één AAD directory**. In deze topologie kunt u Azure AD-verbinding gebruiken voor het synchroniseren van objecten en identiteitsgegevens in een of meer domeinen in één op-premises forest met een enkel AAD huurder. Dit is de standaard topologie geïmplementeerd door de aangepaste installatie van Azure AD verbinden.

    [! [1]][1]

    Maak geen meerdere Azure AD verbinden synchronisatie servers verbinding van verschillende domeinen in hetzelfde forest op gebouwen de dezelfde AAD huurder tenzij een synchronisatieserver Azure AD verbinden in de staging-modus wordt uitgevoerd (Zie de Staging-Server hieronder).

- **Meerdere forests, één AAD directory**. Deze topologie gebruiken als u meer dan één forest met op gebouwen. U kunt identiteitsgegevens consolideren zodat elke unieke gebruiker eenmaal wordt weergegeven in de map AAD, zelfs als dezelfde gebruiker in meer dan één forest bestaat. Alle forests dezelfde Azure AD verbinden synchronisatieserver gebruiken. De server verbinden met Azure AD sync geen deel uitmaakt van een domein, maar uit alle forests bereikbaar moet zijn:

    [! [2]][2]

    In deze topologie, hoeft u afzonderlijke Azure AD verbinding maken met servers in elk forest op ruimten verbinding kunnen maken met een enkele AAD huurder synchroniseren. Dit kan leiden tot dubbele identiteitsgegevens in AAD als gebruikers in meer dan één forest aanwezig zijn.

- **Meerdere forests, verschillende topologieën**. Deze benadering kunt u identiteitsgegevens uit afzonderlijke forests samenvoegen in een enkele AAD huurder. Deze strategie is handig als u bij het combineren van forests vanuit verschillende organisaties (na een fusie of overname, bijvoorbeeld) en de identiteitsgegevens voor elke gebruiker in slechts één forest wordt aangehouden:

    Als de algemene adreslijsten in elk forest zijn gesynchroniseerd, kan een gebruiker in het ene forest aanwezig in de andere als een contactpersoon zijn. Dit kan gebeuren als uw organisatie is bijvoorbeeld GALSync geïmplementeerd met Forefront Identiteitenbeheer 2010 of Microsoft Identity Manager 2016. In dit scenario kunt u opgeven dat gebruikers moeten worden geïdentificeerd door hun *e* -mailkenmerk. U kunt ook met de kenmerken *ObjectSID* en *msExchMasterAccountSID* identiteiten vergelijken. Dit is handig als u een of meer resources forests met uitgeschakelde accounts hebt.

- De **staging-server**. In deze configuratie kunt u een tweede exemplaar van de server verbinden met Azure AD synchronisatie uitvoeren in samenspraak met de eerste. Deze structuur ondersteunt de scenario's, zoals:

    - Hoge beschikbaarheid.

    - Testen en implementeren van een nieuwe configuratie van de server verbinden met Azure AD sync.

    - Introductie van een nieuwe server en ontmanteling van een oude configuratie. 

    In deze scenario's, is het tweede exemplaar wordt uitgevoerd in de *staging-modus*. De server geïmporteerde objecten en synchronisatiegegevens wordt geregistreerd in de database, maar niet de gegevens doorgeeft aan AAD. Alleen wanneer u de staging-modus uitschakelt wordt de server gestart gegevens schrijven naar AAD en begint presterende wachtwoord schrijven-wordt een reservekopie gemaakt in de mappen in de lokalen waar nodig:

    [! [4]][4]

    Zie voor meer informatie [sync Azure AD verbinden: operationele taken en overwegingen met betrekking tot][aad-connect-sync-operational-tasks].

- **AAD van meerdere mappen**. Het is raadzaam dat u één map AAD voor een organisatie maken, maar er zijn situaties waarin u informatie over toepassingsmappartities in aparte mappen voor AAD moet. In dit geval moet u ervoor zorgen dat elk object in het forest op gebouwen wordt alleen in één map AAD weergegeven, om te voorkomen terugneming problemen met synchronisatie en wachtwoord.

    Configureren voor het implementeren van dit scenario, afzonderlijke Azure AD verbinding maken met servers voor elke map AAD synchronisatie en gebruik filteren zodat elke server verbinden met Azure AD sync op een verzameling objecten die elkaar wederzijds uitsluiten werkt: 

    [! [5]][5]

Zie voor meer informatie over deze topologieën [topologieën voor verbinden met Azure AD][aad-topologies].

## <a name="user-sign-in-considerations"></a>Gebruiker aanmelden overwegingen

Standaard wordt de service AAD aangenomen dat gebruikers zich aanmelden door het wachtwoord dat ze op de gebouwen gebruiken en de sync Azure AD verbinding maken met server configureert u Wachtwoordsynchronisatie tussen het domein op gebouwen en AAD. In veel organisaties wordt deze instelling gebruikt, maar u moet rekening houden met het bestaande beleid en de infrastructuur van uw organisatie. Bijvoorbeeld:

- Het beveiligingsbeleid van uw organisatie kan verbieden wachtwoord-hashes naar de cloud synchroniseren.

- U vereist mogelijk dat de gebruikers naadloze SSO ondervinden (zonder extra wachtwoord wordt gevraagd) toen machines op het bedrijfsnetwerk toegang tot cloud resources uit domein toegevoegd.

- Uw organisatie heeft mogelijk al ADFS of een derde partij federation provider geïmplementeerd. U kunt de AAD voor het gebruik van deze infrastructuur te implementeren, verificatie en SSO, in plaats van wachtwoordgegevens die zijn opgeslagen in de cloud.

Voor meer informatie Zie [Azure AD verbinding gebruiker ondertekenen op Opties voor][aad-user-sign-in].

## <a name="azure-ad-application-proxy-considerations"></a>Azure AD application proxy overwegingen

Azure AD gebruiken voor toegang tot toepassingen voor gebouwen.

- Webtoepassingen op ruimten met connectors door de component AD Azure application proxy beheren toepassingsproxy worden blootgesteld. De proxy-connector toepassing opent een uitgaande verbinding naar de toepassingsproxy Azure AD. Externe gebruikers aanvragen van AAD via deze verbinding doorgestuurd naar de web-apps. Dit mechanisme wordt verwijderd moet worden inkomende poorten openen in de firewall op de gebouwen, de kwetsbaarheid die door uw organisatie verminderen.

Zie voor meer informatie [publiceren toepassingen met Azure toepassingsproxy AD][aad-application-proxy].

## <a name="object-synchronization-considerations"></a>Overwegingen bij het object synchronisatie

De standaardconfiguratie van Azure AD verbinden synchroniseert u objecten van uw lokale AD-map op basis van de set regels die zijn opgegeven in het artikel [sync Azure AD verbinden: wat zijn de standaardconfiguratie][aad-connect-sync-default-rules]. Alleen objecten die voldoen aan deze regels worden gesynchroniseerd, anderen worden genegeerd. Bijvoorbeeld moeten gebruikersobjecten het kenmerk van een unieke *sourceAnchor* en het kenmerk *accounEnabled* moet worden ingevuld. User-objecten die een kenmerk *sAMAccountName* niet hebben of die begint met de tekst *AAD_* of *MSOL_* worden niet gesynchroniseerd. Azure AD verbinden toepassing verschillende regels op gebruiker, contactpersoon, groep, ForeignSecurityPrincipal en Computer-objecten. Als u wilt dat de reeks regels wijzigen, gebruiken de regeleditor synchronisatie is geïnstalleerd met Azure AD verbinden (ook beschreven in [sync Azure AD verbinden: wat zijn de standaardconfiguratie][aad-connect-sync-default-rules]).

U kunt uw eigen filters definiëren om te beperken van de objecten die u wilt synchroniseren met een domein of organisatie-eenheid. Of implementeren van complexe aangepaste filters toepassen, zoals wordt beschreven in [sync Azure AD verbinden: filter configureren][aad-filtering].

## <a name="security-considerations"></a>Beveiligingsoverwegingen

Voorwaardelijke toegang weigeren verificatieaanvragen van onverwachte bronnen gebruiken:

- Trigger [MVR Azure meerledige verificatie (gesloten)] [ azure-multifactor-authentication] als een gebruiker probeert verbinding te maken vanaf een niet-vertrouwde locatie (zoals via het Internet) in plaats van een vertrouwd netwerk.

- Het apparaattype platform van de gebruiker (iOS, Android, Windows Mobile, Windows) gebruiken om te bepalen voor toepassingen en functies.

- De ingeschakelde/uitgeschakelde status van de gebruikers apparaten opnemen en deze gegevens opnemen in het beleid voor toegangscontrole. Bijvoorbeeld, als de telefoon van de gebruiker verloren of gestolen is moet dat worden geregistreerd als uitgeschakeld om te voorkomen dat het wordt gebruikt om toegang te krijgen.

- Het toegangsniveau van een gebruiker op basis van groepslidmaatschap beheren. [AAD lidmaatschap van dynamische] regels[ aad-dynamic-membership-rules] in groep-beheer te vereenvoudigen. Zie [Inleiding tot dynamische lidmaatschappen voor groepen]voor een beknopt overzicht van hoe dit werkt,[aad-dynamic-memberships].

- Gebruik voorwaardelijke toegang risico beleid met AAD identiteit bescherming te bieden geavanceerde beveiliging op basis van ongebruikelijke activiteiten aanmelden of andere gebeurtenissen.

Zie [voorwaardelijke toegang Azure Active Directory]voor meer informatie[aad-conditional-access].

## <a name="scalability-considerations"></a>Overwegingen voor schaalbaarheid

Schaalbaarheid is gericht door de AAD-service en de configuratie van de server verbinden met Azure AD sync:

- Voor de service AAD er geen voor het configureren van opties voor het implementeren van schaalbaarheid. De service AAD ondersteunt schaalbaarheid op basis van replica's. AAD implementeert een enkele primaire replica, welke ingangen schrijfbewerkingen, en meerdere alleen-lezen secundaire replica's. AAD wordt geprobeerd schrijft ten opzichte van secundaire replica's naar de primaire replica omgeleid. AAD biedt uiteindelijk consistentie. Alle wijzigingen in de primaire replica worden doorgegeven aan de secundaire replica's. Aangezien de meeste bewerkingen tegen AAD leest in plaats van schrijven, wordt deze architectuur ook aangepast.

    Zie voor meer informatie [AD Azure: achter de schermen van onze cloud geo-redundante, hoge beschikbaarheid, gedistribueerde directory][aad-scalability].

- Voor de server verbinden met Azure AD synchroniseren, moet u bepalen hoeveel objecten u waarschijnlijk bent worden gesynchroniseerd van uw lokale map. Als u minder dan 100.000 objecten hebt, kunt u de standaard SQL Server Express LocalDB software die wordt geleverd met Azure AD verbinden. Hebt u een groter aantal objecten, moet u een productieversie van SQL Server installeren en uitvoeren van een aangepaste installatie van de Azure AD verbinden opgeven waarop een bestaand exemplaar van SQL Server moet worden gebruikt.

## <a name="availability-considerations"></a>Overwegingen met betrekking tot beschikbaarheid

Net als bij problemen met schaalbaarheid, omvat beschikbaarheid de AAD-service en de configuratie van Azure AD verbinden:

- De AAD-service is ontworpen voor hoge beschikbaarheid bieden. Er zijn geen beschikbaarheidopties van de gebruiker worden geconfigureerd. Geo verdeeld is en wordt uitgevoerd in meerdere data centers verspreid over de hele wereld, met automatische failover. Als een datacenter uitvalt, AAD zorgt ervoor dat uw Active directory-gegevens toegankelijk zijn voor exemplaar in ten minste twee meer regionaal verspreide datacenters.

    >[AZURE.NOTE] De SLA voor AAD Basic en Premium services garanties ten minste 99,9% beschikbaarheid. Er is geen SLA voor de vrij laag van AAD. Zie voor meer informatie, [SLA voor Azure Active Directory][sla-aad].

- Als u wilt verhogen de beschikbaarheid van de server verbinden met Azure AD sync kunt u een tweede exemplaar uitvoeren in staging-modus, zoals beschreven in de sectie [topologie overwegingen](#topology-considerations) . 

    Ook als u geen van het exemplaar van SQL Server Express LocalDB dat wordt geleverd met Azure AD verbinden gebruikmaakt, moet vervolgens u overwegen hoge beschikbaarheid voor SQL Server. Houd er rekening mee dat de oplossing alleen hoge beschikbaarheid ondersteund SQL clustering is. Oplossingen voor mirroring en altijd worden niet ondersteund door Azure AD verbinden.

    Zie voor meer overwegingen over het beheren van de beschikbaarheid van de server verbinden met Azure AD synchronisatie en het herstellen na een storing [sync Azure AD verbinden: operationele taken en overwegingen - Disaster Recovery][aad-sync-disaster-recovery].

## <a name="management-considerations"></a>Overwegingen bij

Er zijn twee aspecten AAD beheren:

1. Beheer van AAD in de cloud.

2. Het onderhouden van de synchronisatie Azure AD verbinding maken met servers.

AAD biedt de volgende opties voor het beheren van domeinen en mappen in de cloud:

- [Azure Active Directory PowerShell-Module][aad-powershell]. In deze module gebruiken als u nodig hebt om een script Azure AD administratieve taken zoals Gebruikersbeheer, domeinbeheer van het en voor het configureren van eenmalige aanmelding.

- Azure AD management blade in Azure portal. Deze blade biedt een interactieve management-weergave van de map, en kunt u beheren en configureren van de meeste aspecten van AAD.

    [! [10]][10]

Azure AD verbinding maken met installeert de volgende hulpprogramma's waarmee u de Azure AD verbinden sync services van uw machines op gebouwen onderhouden:

- De console Microsoft Azure Active Directory-verbinding. Met dit hulpprogramma kunt u de configuratie van de server Azure AD synchronisatie wijzigen, aanpassen hoe synchronisatie plaatsvindt inschakelen of staging-modus uitschakelt en schakelen de-in gebruikersmodus (kunt u AD FS Meld u aan met uw infrastructuur in gebouwen).

- De synchronisatie van de Service Manager. Gebruik het tabblad *bewerkingen* in dit hulpprogramma voor het beheren van het synchronisatieproces en detecteren of alle onderdelen van het proces is mislukt. U kunt synchronisatie handmatig met behulp van dit programma kunt activeren. 

    [! [12]][12]

    Het tabblad *verbindingslijnen* kunt u de verbindingen voor de domeinen (in gebouwen en in de cloud) het synchronisatieprogramma is gekoppeld:

    [! [13]][13]

-  De regeleditor synchronisatie. Met dit hulpprogramma kunt u de manier aanpassen waarop objecten worden getransformeerd, als ze worden gekopieerd tussen een directory op gebouwen en AAD in de cloud. Met dit hulpprogramma kunt u extra kenmerken en objecten voor synchronisatie en filters om te bepalen welke instanties moeten worden of niet worden gesynchroniseerd.

    Zie de sectie regeleditor synchronisatie in het document voor meer informatie [sync Azure AD verbinden: wat zijn de standaardconfiguratie][aad-connect-sync-default-rules].

De pagina [sync Azure AD verbinden: aanbevolen procedures voor het wijzigen van de standaardconfiguratie] [ aad-sync-best-practices] bevat extra informatie en tips voor het beheren van Azure AD verbinden.

## <a name="monitoring-considerations"></a>Overwegingen met betrekking tot controle

Controle wordt uitgevoerd met behulp van een reeks agenten geïnstalleerd in lokalen:

- Azure AD verbinding installeert een agent die informatie over synchronisaties worden vastgelegd. De blade Azure Active Directory verbinding maken met de gezondheid in Azure portal gebruiken voor het controleren van de gezondheid en de prestaties van Azure AD verbinden. Zie voor meer informatie, [Met behulp van Azure AD gezondheid verbinden voor synchronisatie][aad-health].

- Voor het controleren van de gezondheid van de AD DS-domeinen en -mappen van Azure de Azure AD verbinding maken met de gezondheid van AD DS-agent te installeren op een computer binnen het domein van gebouwen. Gebruik de blade Azure Active Directory verbinding maken met de gezondheid in de Azure portal voor het bewaken van AD DS. Zie voor meer informatie, [Met behulp van Azure AD gezondheid verbinding met AD DS][aad-health-adds]

- De Azure AD verbinding maken met de gezondheid van AD FS-agent monitor van de gezondheid van AD FS op gebouwen waarop en de blade Azure Active Directory verbinding maken met de gezondheid in de Azure portal gebruiken om te controleren van AD DS te installeren. Zie [Met behulp van Azure AD gezondheid verbinding met AD FS] voor meer informatie.[aad-health-adfs]

Zie voor meer informatie over het installeren van de AD verbinding Health agents en de bijbehorende vereisten [Azure AD verbinding Health Agent-installatie][aad-agent-installation].

## <a name="sample-solution"></a>Monsteroplossing

In deze sectie worden de stappen voor het bouwen van een oplossing die u gebruiken kunt voor het testen van de configuratie van AAD. De oplossing omvat de volgende elementen:

- Een n-tier-webtoepassing, volgens de structuur die wordt beschreven door [VMs uitvoeren voor een N-lagige architectuur op Azure][implementing-a-multi-tier-architecture-on-Azure].

- Een test-clientcomputer. Wij raden u aan met behulp van een andere VM voor deze computer. De configuratie van deze VM is niet belangrijk, zolang u toegang hebt en deze verbinding met de webtoepassing van de n-tier maken kan.

- Een gesimuleerde op gebouwen-omgeving waarin het eigen domein gebouwd met behulp van AD DS.

De scenario's die deze stappen zijn:

- Waardoor de toegang tot de webtoepassing van n-tier uitgevoerd in de cloud voor externe gebruikers, met AAD wachtwoord verificatie.

- Waardoor de toegang tot de n-tier-webtoepassing in de cloud aan gebruikers binnen de organisatie, met AAD wachtwoord verificatie uitgevoerd.

### <a name="prerequisites"></a>Vereisten

De volgende stappen wordt uitgegaan van de volgende vereisten:

- U hebt een Azure abonnement waar kunt u resourcegroepen maken.

- U hebt geïnstalleerd de [opdrachtregelinterface Azure][azure-cli].

- U hebt gedownload en geïnstalleerd van de meest recente versie. Zie [hier] [ azure-powershell-download] voor instructies.

- U hebt geïnstalleerd een kopie van de [makecert] [ makecert] op de clientcomputer test utility.

- U hebt toegang tot een computer development met Visual Studio is geïnstalleerd.

### <a name="create-the-n-tier-web-application-architecture"></a>De architectuur van n-tier web-toepassing maken

1. Maak een map met de naam `Scripts` bevat een submap met de naam `Parameters`.

2. Downloaden van de [Implementatie ReferenceArchitecture.ps1] [ solution-script] PowerShell script naar de map.

3. Maak een nieuwe submap met de naam Windows in de map Parameters.

4. Download de volgende bestanden naar de map Windows Parameters:

    - [virtualNetwork.parameters.json][vnet-parameters-windows]

    - [networkSecurityGroup.parameters.json][nsg-parameters-windows]

    - [webTierParameters.json][webtier-parameters-windows]

    - [businessTierParameters.json][businesstier-parameters-windows]

    - [dataTierParameters.json][datatier-parameters-windows]

    - [managementTierParameters.json][managementtier-parameters-windows]

5. Het bestand **Distribueren ReferenceArchitecture.ps**1 in de map Scripts bewerken en wijzig de volgende regel om aan te geven van de resourcegroep die moet worden gemaakt of voor het opslaan van de VM en bronnen die zijn gemaakt door het script:

        ```powershell
        # PowerShell
        $resourceGroupName = "ra-aad-ntier-rg"
        ```

6. De volgende bestanden in de map **parameters/venster**bewerken en stel de `resourceGroup` waarde in de `virtualNetworkSettings` sectie in elk van deze bestanden zijn hetzelfde als die u hebt opgegeven in het scriptbestand **Distribueren ReferenceArchitecture.ps1** .

    - networkSecurityGroup.parameters.json

    - webTierParameters.json

    - businessTierParameters.json

    - dataTierParameters.json

    - managementTierParameters.json

7. Een Azure PowerShell-venster openen, verplaatsen naar de map Scripts en voer de volgende opdracht uit:

        ```powershell
        .\Deploy-ReferenceArchitecture.ps1 <subscription id> <location> Windows ntier
        ```

    Vervangen **<subscription id>** bij de Azure abonnement-ID.

    Voor **<location>**, een Azure-regio, zoals *eastus* of *westus*.

8. Wanneer het script is voltooid, kunt u de portal Azure het openbare IP-adres van de web-tier-taakverdeling (*ra-aad-ntier-web-lb*):

    [! [18]][18]

9. Log in op uw test client computer (of VM) en controleer of u toegang hebt tot de weblaag met behulp van Internet Explorer om te bladeren naar het openbare IP-adres van de web-tier taakverdeling. De standaard IIS-pagina moet worden weergegeven.

### <a name="simulate-configuration-of-a-public-web-site"></a>Configuratie van een openbare website simuleren

>[AZURE.NOTE] De volgende stappen simuleren de weblaag koppelen aan de DNS-naam www.contoso.com doordat het hosts-bestand op de clientcomputer test. Bovendien zijn de VMs web tier geconfigureerd met zelfondertekende certificaten en vals autoriteit die als host fungeert. Dit is voor het testen van uitsluitend en **dat moet u deze technieken in een productieomgeving niet gebruiken**.

1. Op de clientcomputer test het bestand **C:\Windows\System32\drivers\etc\hosts** bewerken met Kladblok en voeg een vermelding die de naam www.contoso.com worden gekoppeld aan het openbare IP-adres van de web-tier taakverdeling toe:

        ```text
        # Copyright (c) 1993-2009 Microsoft Corp.
        #
        # This is a sample HOSTS file used by Microsoft TCP/IP for Windows.
        #
        # This file contains the mappings of IP addresses to host names. Each
        # entry should be kept on an individual line. The IP address should
        # be placed in the first column followed by the corresponding host name.
        # The IP address and the host name should be separated by at least one
        # space.
        #
        # Additionally, comments (such as these) may be inserted on individual
        # lines or following the machine name denoted by a '#' symbol.
        #
        # For example:
        #
        #      102.54.94.97     rhino.acme.com          # source server
        #       38.25.63.10     x.acme.com              # x client host
        
        # localhost name resolution is handled within DNS itself.
        #   127.0.0.1       localhost
        #   ::1             localhost
        
        52.165.38.64    www.contoso.com
        ```

2. Controleer of dat u nu naar www.contoso.com vanaf de clientcomputer test bladeren kunt. De pagina dezelfde standaard IIS weergegeven als voorheen.

3. Op de clientcomputer test openen vanaf de opdrachtprompt als Administrator en gebruik het hulpprogramma makecert c
4. een certificaat van een basiscertificeringsinstantie valse maken:

        ```
        makecert -sky exchange -pe -a sha256 -n "CN=MyFakeRootCertificateAuthority" -r -sv MyFakeRootCertificateAuthority.pvk MyFakeRootCertificateAuthority.cer -len 2048
        ```

    Controleer of de volgende bestanden zijn gemaakt:

    - MyFakeRootCertificateAuthority.cer

    - MyFakeRootCertificateAuthority.pvk

4. Voer de volgende opdracht om de test basiscertificeringsinstantie installeren:

        ```
        certutil.exe -addstore Root MyFakeRootCertificateAuthority.cer
        ```

5. Gebruik de test certificeringsinstantie om een certificaat voor www.contoso.com te genereren:

        ```
        makecert -sk pkey -iv MyFakeRootCertificateAuthority.pvk -a sha256 -n "CN=www.contoso.com" -ic MyFakeRootCertificateAuthority.cer -sr localmachine -ss my -sky exchange -pe
        ```

6. Voer de opdracht **mmc** en voeg de module Certificaten voor de computeraccount voor de lokale computer.

7. In de */Certificates (lokale Computer) / persoonlijk/certificaten/* opgeslagen, het www.contoso.com certificaat exporteren met de persoonlijke sleutel naar een bestand met de naam www.contoso.com.pfx:

    [! [20]][20]

8. Terug naar de portal Azure en verbinding maken met de management laag VM (*ra-aad-ntier-mgmt-vm1*). De standaardgebruikersnaam is het *testgebruiker* met wachtwoord *AweS0me@PW*:

    [! [21]][21]
    
9. Verbinden aan elk van de weblaag VMs weer met de gebruikersnaam *' testuser '* en het wachtwoord van de laag van het management VM, *AweS0me@PW*, en de volgende taken uitvoeren. Houd er rekening mee dat het VMs de particuliere adressen IP 10.0.1.4, 10.0.1.5 en 10.0.1.6:

    >[AZURE.NOTE] De weblaag VMs alleen particuliere IP-adressen hebben. U kunt alleen verbinding maken met deze met behulp van de management laag VM.

    1. Kopieer de bestanden *www.contoso.com.pfx* en *MyFakeRootCertificateAuthority.cer* van de clientcomputer test.
    
    2. Open een opdrachtprompt als Administrator, verplaatsen naar de map die de bestanden die u hebt gekopieerd en voer de volgende opdrachten:
    
        ```
        certutil.exe -privatekey -importPFX my www.contoso.com.pfx NoExport

        certutil.exe -addstore Root MyFakeRootCertificateAuthority.cer
        ```

    3. Voer de `mmc` de opdracht, de module Certificaten voor de computeraccount voor de lokale computer toevoegen en controleer of de volgende certificaten zijn geïnstalleerd:

        - \Certificates (lokale Computer) \Personal\Certificates\ www.contoso.com

        - \Certificates (lokale Computer) \Trusted Root Certification Authorities\Certificates\MyFakeRootCertificateAuthority

    4. Start de console Beheer van Internet Information Services (IIS) en Ga naar *de website Sites\Default* op de computer.

    5. In het deelvenster *Acties* op *Bindingen*en een https-binding met het SSL-certificaat www.contoso.com toevoegen:

        [! [22]][22]

10. Terug naar de clientcomputer test en controleer of u kunt nu bladeren naar https://www.contoso.com.

### <a name="create-an-azure-active-directory-tenant"></a>Een huurder Azure Active Directory maken

1. Met de Azure portal maken een huurder Azure Active Directory zoals *myaadname*. onmicrosoft.com, waarbij *myaadname* de naam van een map die u hebt geselecteerd is.

2. Een *beheerder* met de rol van GlobalAdmin naar de map met de naam gebruiker toevoegen. Maak een notitie van het nieuwe wachtwoord.

3. Internet Explorer gebruikt, Ga naar https://account.activedirectory.windowsazure.com/ en meld u aan als admin@ *myaadname*. onmicrosoft.com. Wijzig uw wachtwoord wanneer daarom wordt gevraagd.

### <a name="create-and-deploy-a-test-web-application"></a>Maken en implementeren van een webtoepassing testen

1. Met behulp van Visual Studio op de ontwikkelcomputer maken een ASP.NET-webtoepassing met de naam ContosoWebApp1 (gebruik van het .NET Framework 4.5.2):

    [! [23]][23]

2. Selecteer de sjabloon *MVC* en geef de naam van de huurder AAD de verificatie wijzigen naar *werk en School*. Maak geen een App-Service in de cloud.

    [! [24]][24]

3. Bouwen en uitvoeren van de toepassing voor het testen van de verificatie. Voer op de pagina-in de naam van de admin@ *myaadname*. onmicrosoft.com, het wachtwoord en klik vervolgens op *aanmelden*:

    [! [25]][25]

4. Controleer of AAD vraagt toestemming voor het aanmelden en het lezen van uw profiel en start vervolgens de webtoepassing wordt uitgevoerd met de identiteit van de beheerder.

5. Internet Explorer sluiten en terugkeren naar Visual Studio.

6. Open het bestand web.config in de `<appSettings>` sectie, wijzigt u de waarde van de sleutel *ida: PostLogoutRedirectUri* *https://www.contoso.com:443 /*. Sla het bestand.

7. In het venster *Solution Explorer* met de rechtermuisknop op het ContosoWebApp1-project, klikt u op *publiceren*.

8. Klik op *aangepast*in het venster *Web publiceren* . Maak een aangepast profiel met de naam *ContosoWebApp1*.

9. De *methode Publish* instellen op het *Bestandssysteem* op de pagina *verbinding* en geef een map met de naam *ContosoWebApp*, zich op een handige locatie op de computer.

10. Instellen op de pagina *Instellingen voor* de *configuratie van* *release*.

11. Klik op de pagina *voorvertoning* *publiceren*.

12. Verbinding maken met elke webserver weer (via de management laag VM) en de volgende taken uitvoeren:

    1. De map *ContosoWebApp* en de inhoud ervan kopiëren vanaf de ontwikkelcomputer naar de map *C:\inetpub* .

    2. Met de console Beheer van Internet Information Services (IIS), Ga naar *de website Sites\Default* op de computer.

    3. In het deelvenster *Acties* op *Basis instellingen*en het fysieke pad van de website wijzigen in *%SystemDrive%\inetpub\ContosoWebApp*:

        [! [26]][26]

### <a name="publish-the-test-web-application-through-aad"></a>De webtoepassing testen via AAD publiceren

1. Log in op de Azure portal en navigeer naar de map AAD.

2. Klik op het tabblad *toepassingen* op de toepassing van de ContosoWebApp1.

3. Controleer of uw toepassing is toegevoegd aan de map en klik vervolgens op het tabblad *configureren* .

4. De *SIGN-ON-URL* wijzigen in https://www.contoso.com:443 en het *Antwoord URL* ingesteld op https://www.contoso.com:443 (de URL).

5. De configuratie op te slaan.

6. Ga naar https://www.contoso.com op de clientcomputer test. Controleer dat AAD wordt u gevraagd uw referenties en meld u vervolgens aan.

>[AZURE.NOTE] Dit is het eindpunt voor het eerste scenario: toegang tot de webtoepassing van n-tier uitgevoerd in de cloud voor externe gebruikers, met AAD wachtwoord verificatie inschakelen.

### <a name="create-a-simulated-on-premises-environment-with-active-directory"></a>Een gesimuleerde op ruimten omgeving maken met Active Directory

De omgeving van ruimten bevat twee domeincontrollers voor de `contoso.com` domein en twee servers als host voor de Azure AD Connect service synchroniseert. De servers voor het hosten van Azure AD verbinding zijn geen domein behoren.

1. In File Explorer, terug naar de map met het script gebruikt om de N-tier-webtoepassing te maken.

2. In de map Parameters toevoegen een ander sub map met de naam `Onpremise`.

3. De volgende bestanden naar de map Parameters/lokale downloaden:

    - [toevoegen-voegt-domein-controller.parameters.json][add-adds-domain-controller-parameters]

    - [maken-toegevoegd-forest-extension.parameters.json][create-adds-forest-extension-parameters]

    - [virtualMachines adc.parameters.json][virtualMachines-adc-parameters]

    - [adc-virtualMachines-joindomain.parameters.json][virtualMachines-adc-joindomain-parameters]

    - [virtualMachines adds.parameters.json][virtualMachines-adds-parameters]

    - [virtualNetwork.parameters.json][virtualNetwork-parameters]

    - [virtualNetwork-voegt dns.parameters.json][virtualNetwork-adds-dns-parameters]

5. Het bestand distribueren ReferenceArchitecture.ps1 in de map Scripts bewerken en wijzig de volgende regel om aan te geven van de resourcegroep die moet worden gemaakt of voor het opslaan van de VM en bronnen die zijn gemaakt door het script:

        ```powershell
        # PowerShell
        $resourceGroupName = "ra-aad-onpremise-rg"
        ```

6. De volgende bestanden in de map Parameters/lokale bewerken en stel de `resourceGroup` waarde in de `virtualNetworkSettings` sectie in elk van deze bestanden zijn hetzelfde als die u hebt opgegeven in het scriptbestand distribueren ReferenceArchitecture.ps1.

    - virtualMachines adds.parameters.json

    - virtualMachines adc.parameters.json

    - virtualNetwork.parameters.json

    - virtualNetwork-voegt dns.parameters.json

7. Een Azure PowerShell-venster openen, verplaatsen naar de map Scripts en voer de volgende opdracht uit:

        ```powershell
        .\Deploy-ReferenceArchitecture.ps1 <subscription id> <location> Windows onpremise
        ```

    Vervangen `<subscription id>` bij de Azure abonnement-ID.

    Voor `<location>`, Geef een Azure regio, zoals `eastus` of `westus`.

### <a name="install-and-configure-the-azure-ad-connect-sync-service"></a>Het verbinden van Azure AD sync-service installeren en configureren

De configuratie wordt geïllustreerd in de volgende stappen bestaat uit twee exemplaren van de synchronisatieservice Azure AD verbinden. De eerste is actief, terwijl de tweede staging-modus te snelle failover en hoge beschikbaarheid bieden als de eerste server niet is geconfigureerd.

1. De portal Azure gaat u naar de resourcegroep die de VMs voor de Azure AD verbinden sync services (*ra aad-lokale rg* standaard) en verbinding maken met de *ra-aad-lokale-adc-vm1* VM. Meld u aan als *' testuser '* wachtwoord *AweS0me@PW*.

2. Azure AD verbinden downloaden vanaf [hier][aad-connect-download].

3. Voer het installatieprogramma Azure AD verbinding maken en uitvoeren van een installatie met de optie *aanpassen* .

    >[AZURE.NOTE] U kunt de optie *Express-instellingen* niet gebruiken omdat de VM Azure verbinden met AD-sync hostingservice niet deel uitmaakt van een domein.

4. Klik op de pagina *vereiste onderdelen installeren* niets de optionele configuratie-instellingen om de standaardopties te accepteren.

5. Selecteer op de pagina *gebruiker aanmelden* *Wachtwoordsynchronisatie*.

6. Voer op de pagina *verbinding maken met Azure AD* admin@ *myaadname*. onmicrosoft.com, waarbij *myaadname* de naam van de huurder AAD is. Voer het wachtwoord voor de Administrator-account.

7. Geef op de pagina *verbinding maken met uw mappen* contoso.com voor het forest (Typ de waarde in omdat deze niet wordt weergegeven in de vervolgkeuzelijst) CONTOSO\testuser invoeren voor de gebruikersnaam, opgeven AweS0me@PW voor het wachtwoord, en klik vervolgens op *Map toevoegen*.

8. Accepteer de standaardwaarde voor de UPN-naam op de pagina *configuratie van-in Azure AD* . *Doorgaan zonder alle gecontroleerde domeinen te*controleren.

    >[AZURE.NOTE] De map contoso.com worden weergegeven als *Niet gecontroleerd*. Als u wilt controleren of de naam van een domein, moet u een TXT-record van uw registratieservice domein naam. In dit voorbeeld wordt is het domein op het bedrijf niet geregistreerd extern. Zie [een aangepaste domeinnaam naar Azure Active Directory toevoegen]voor meer informatie[aad-custom-directory].

9. Selecteer op de pagina *domeinen en organisatie-eenheid filteren* *synchroniseren voor alle domeinen en organisatie-eenheden* (de standaardwaarde).

10. Accepteer de standaardwaarden op de pagina *een unieke identificatie van uw gebruikers* .

11. Selecteer op de pagina *filteren, gebruikers en apparaten* *synchroniseren voor alle gebruikers en apparaten* (de standaardinstelling).

12. Selecteer op de pagina *optionele onderdelen* *wachtwoord Write-back*.

13. Op de pagina *Gereed om te configureren* , selecteert u *Start het synchronisatieproces wanneer de configuratie is voltooid*, maar laat *staging-modus inschakelen* is uitgeschakeld.

14. Controleer dat het configuratieproces is voltooid zonder fouten en vervolgens het installatieprogramma af te sluiten.

15. Vanaf de portal Azure, verbinding maken met de *ra-aad-lokale-adc-vm2* VM. Meld u aan als *' testuser '* wachtwoord *AweS0me@PW*.

16. Azure AD verbinden downloaden en vervolgens het installatieprogramma uitvoeren.

17. De wizard stap voor stap en reageren zoals beschreven in stap 3 tot en met 12.

18. Selecteer op de pagina *Gereed voor het configureren van* *Start het synchronisatieproces wanneer de configuratie is voltooid*, en **ook** *staging-modus inschakelen*. Hierdoor wordt de synchronisatieservice Azure AD verbinding maken voor meer informatie over accounts en objecten ophalen uit het domein contoso.com en opgeslagen in de database, maar dat deze gegevens aan uw huurder AAD niet verzenden.

14. Controleer dat het configuratieproces is voltooid zonder fouten en vervolgens het installatieprogramma af te sluiten.

### <a name="test-the-aad-configuration"></a>AAD configuratie testen

1. Met behulp van de portal Azure, Ga naar de map AAD, opent de blade Azure Active Directory klikt u op *gebruikers en groepen*en klikt u op *alle gebruikers* om de lijst met gebruikers en groepen die zijn gesynchroniseerd met de map weer te geven. Gebruikers van de volgende rekeningen worden weergegeven:

    - Admin (admin@ *myaadname*. onmicrosoft.com)

    - On-Premises Directory synchronisatie-serviceaccount (Sync_ADC1_*nnnnnnnnnnnn*@*myaadname*. onmicrosoft.com)

    - On-Premises Directory synchronisatie-serviceaccount (Sync_ADC2_*nnnnnnnnnnnn*@*myaadname*. onmicrosoft.com)

2. In de Azure portal, navigeer naar de resourcegroep die de VMs voor AD DS-domeincontrollers (*ra aad-lokale rg* standaard) en verbinding maken met de *ra-aad-lokale-ad-vm1* VM. Meld u aan als *' testuser '* wachtwoord *AweS0me@PW*.

3. Met de console *Active Directory: gebruikers en Computers* toevoegen een nieuwe domeingebruiker met de aanmeldingsnaam van de naam Diane Tibbot, dianet@contoso.com. Geef een wachtwoord van uw keuze. De gebruiker lid maken van de lokale groep Administrators (dit is zodat u kunt zich lokaal aanmeldt als deze gebruiker later - alleen computers in het domein DC's zijn).

4. Ga naar *ra-aad-lokale-adc-vm1* VM, open een venster PowerShell en voer de volgende opdrachten:

        ```[powershell]
        Import-Module ADSync
        Start-ADSyncSyncCycle -PolicyType Delta
        ```

    Controleer of de opdracht *succes*als resultaat gegeven.

    >[AZURE.NOTE] Deze stap is noodzakelijk omdat de standaard gepland tijdens de synchronisatie uit te voeren om de 30 minuten. Deze opdrachten zorgen ervoor dat een synchronisatie moet worden uitgevoerd.

5. Terug naar de portal Azure, overschakelen naar de directory AAD, open de blade Azure Active Directory, klikt u op *gebruikers en groepen*en klikt u op *alle gebruikers*. U ziet nu Diane Tibbot (dianet@ *myaadname*. onmicrosoft.com) worden weergegeven in de lijst met gebruikers.

6. In het blad Azure Active Directory op *Zakelijke toepassingen*en klik vervolgens op *alle toepassingen*. Klik op de toepassing van de *ContosoWebApp1* en klik vervolgens op *gebruikers en groepen*. Klik op *toevoegen*op de werkbalk. Klik op *gebruikers en groepen*en *Diane Tibbot*selecteren. Klik op *toewijzen*.

7. Internet Explorer gebruikt, gaat u naar de site https://account.activedirectory.windowsazure.com. Meld u aan als dianet@ *myaadname*. onmicrosoft.com met het wachtwoord dat u eerder hebt opgegeven.

    >[AZURE.NOTE] Klik niet op het pictogram van de ContosoWebApp in de lijst met toepassingen. AAD gezocht naar de webtoepassing op het openbaar vermelde DNS-adres voor www.contoso.com, die verschilt van het adres van uw web-tier-taakverdeling.

8. Klik op het tabblad *profiel* . De details van de gebruiker (als u een opgegeven toen deze werd gemaakt) moeten worden weergegeven.

    >[AZURE.NOTE] Als u op *wachtwoord wijzigen*, wordt u niet toegestaan deze taak kunt uitvoeren als deze bewerking moet eerst door de beheerder worden ingeschakeld. Zie voor meer informatie, [aan de slag met wachtwoord Management][aad-password-management].

### <a name="enable-on-premises-users-to-run-the-application-by-using-authentication-through-aad"></a>Kunnen gebruikers de toepassing uitvoeren met behulp van verificatie via AAD op ruimten

1. Terug naar de domeincontroller *ra-aad-lokale-ad-vm1* VM.

2. Open de console *DNS-beheer* en een nieuwe hostrecord voor www.contoso.com toevoegen. Geef het openbare IP-adres van de web-tier taakverdeling.

3. Kopieer het bestand *MyFakeRootCertificateAuthority.cer* van de test-clientcomputer (u deze bestanden gemaakt in de procedure [Simulate configuratie van een openbare website](#simulate-configuration-of-a-public-web-site)
    
4. Open een opdrachtprompt als Administrator, verplaatsen naar de map die het bestand dat u zojuist hebt gekopieerd en voer de volgende opdracht uit:

        ```
        certutil.exe -addstore Root MyFakeRootCertificateAuthority.cer
        ```

5. Internet Explorer gebruikt, gaat u naar https://www.contoso.com. Controleer of de AAD-aanmeldingspagina voor de webtoepassing ContosoWebApp1 wordt weergegeven.

6. Meld u aan als dianet@ *myaadname*. onmicrosoft.com. De toepassing moet worden uitgevoerd en u goed log in.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over de aanbevolen procedures voor het [uitbreiden van uw domein op ruimten ADDS Azure][adds-extend-domain]

- Informatie over de aanbevolen procedures voor het [maken van een forest met bronnen toevoegen] [ adds-resource-forest] in Azure

<!-- links -->
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[script]: #sample-solution-script
[implementing-a-multi-tier-architecture-on-Azure]: ./guidance-compute-n-tier-vm.md
[active-directory-domain-services]: https://technet.microsoft.com/library/dd448614.aspx
[active-directory-federation-services]: https://technet.microsoft.com/windowsserver/dd448613.aspx
[azure-active-directory]: ../active-directory-domain-services/active-directory-ds-overview.md
[azure-ad-connect]: ../active-directory/active-directory-aadconnect.md
[ad-azure-guidelines]: https://msdn.microsoft.com/library/azure/jj156090.aspx
[aad-explained]: https://youtu.be/tj_0d4tR6aM
[aad-editions]: ../active-directory/active-directory-editions.md
[guidance-adds]: ./guidance-iaas-ra-secure-vnet-ad.md
[sla-aad]: https://azure.microsoft.com/support/legal/sla/active-directory/v1_0/
[azure-multifactor-authentication]: ../multi-factor-authentication/multi-factor-authentication.md
[aad-conditional-access]: ../active-directory//active-directory-conditional-access.md
[aad-dynamic-membership-rules]: ../active-directory/active-directory-accessmanagement-groups-with-advanced-rules.md
[aad-dynamic-memberships]: https://youtu.be/Tdiz2JqCl9Q
[aad-user-sign-in]: ../active-directory/active-directory-aadconnect-user-signin.md
[aad-sync-requirements]: ../active-directory/active-directory-hybrid-identity-design-considerations-directory-sync-requirements.md
[aad-topologies]: ../active-directory/active-directory-aadconnect-topologies.md
[aad-filtering]: ../active-directory/active-directory-aadconnectsync-configure-filtering.md
[aad-scalability]: https://blogs.technet.microsoft.com/enterprisemobility/2014/09/02/azure-ad-under-the-hood-of-our-geo-redundant-highly-available-distributed-cloud-directory/
[aad-connect-sync-default-rules]: ../active-directory/active-directory-aadconnectsync-understanding-default-configuration.md
[aad-identity-protection]: ../active-directory/active-directory-identityprotection.md
[aad-password-management]: ../active-directory/active-directory-passwords-customize.md
[aad-application-proxy]: ../active-directory/active-directory-application-proxy-enable.md
[aad-connect-sync-operational-tasks]: ../active-directory/active-directory-aadconnectsync-operations.md#staging-mode
[aad-custom-domain]: ../active-directory/active-directory-add-domain.md
[aad-powershell]: https://msdn.microsoft.com/library/azure/mt757189.aspx
[aad-sync-disaster-recovery]: ../active-directory/active-directory-aadconnectsync-operations.md#disaster-recovery
[aad-sync-best-practices]: ../active-directory/active-directory-aadconnectsync-best-practices-changing-default-configuration.md
[aad-adfs]: ../active-directory/active-directory-aadconnect-get-started-custom.md#configuring-federation-with-ad-fs
[aad-health]: ../active-directory/active-directory-aadconnect-health-sync.md
[aad-health-adds]: ../active-directory/active-directory-aadconnect-health-adds.md
[aad-health-adfs]: ../active-directory/active-directory-aadconnect-health-adfs.md
[aad-agent-installation]: ../active-directory/active-directory-aadconnect-health-agent-install.md
[aad-reporting-guide]: ../active-directory/active-directory-reporting-guide.md
[azure-cli]: ../virtual-machines-command-line-tools.md
[azure-powershell-download]: ../powershell-install-configure.md
[solution-script]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/Deploy-ReferenceArchitecture.ps1
[vnet-parameters-windows]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/windows/virtualNetwork.parameters.json
[nsg-parameters-windows]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/windows/networkSecurityGroups.parameters.json
[webtier-parameters-windows]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/windows/webTier.parameters.json
[businesstier-parameters-windows]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/windows/businessTier.parameters.json
[datatier-parameters-windows]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/windows/dataTier.parameters.json
[managementtier-parameters-windows]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/windows/managementTier.parameters.json
[makecert]: https://msdn.microsoft.com/library/windows/desktop/aa386968(v=vs.85).aspx
[add-adds-domain-controller-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/onpremise/add-adds-domain-controller.parameters.json
[create-adds-forest-extension-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/onpremise/create-adds-forest-extension.parameters.json
[virtualMachines-adds-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/onpremise/virtualMachines-adds.parameters.json
[virtualNetwork-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/onpremise/virtualNetwork.parameters.json
[virtualNetwork-adds-dns-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/onpremise/virtualNetwork-adds-dns.parameters.json
[virtualMachines-adc-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/onpremise/virtualMachines-adc.parameters.json
[virtualMachines-adc-joindomain-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/onpremise/virtualMachines-adc-joindomain.parameters.json
[aad-connect-download]: http://www.microsoft.com/download/details.aspx?id=47594
[aad-custom-directory]: ../active-directory/active-directory-add-domain.md
[aad-password-management]: ../active-directory/active-directory-passwords-getting-started.md#enable-users-to-reset-their-azure-ad-passwords
[adds-extend-domain]: ./guidance-identity-adds-extend-domain.md
[adds-resource-forest]: ./guidance-identity-adds-resource-forest.md
[0]: ./media/guidance-identity-aad/figure1.png "Cloud identity-architectuur met Azure Active Directory"
[1]: ./media/guidance-identity-aad/figure2.png "Eén forest, één AAD directory-topologie"
[2]: ./media/guidance-identity-aad/figure3.png "Meerdere forests, één AAD directory-topologie"
[4]: ./media/guidance-identity-aad/figure5.png "Servertopologie staging"
[5]: ./media/guidance-identity-aad/figure6.png "Topologie met meerdere AAD mappen"
[6]: ./media/guidance-identity-aad/figure7.png "Een aangepaste installatie van Azure AD verbinding synchroniseren met een specifiek exemplaar van SQL Server selecteren"
[7]: ./media/guidance-identity-aad/figure8.png "De SSO-methode voor het aanmelden van gebruiker opgeven"
[8]: ./media/guidance-identity-aad/figure9.png "Domein en organisatie-eenheid filteropties opgeven"
[9]: ./media/guidance-identity-aad/figure10.png "Terugneming wachtwoord inschakelen"
[10]: ./media/guidance-identity-aad/figure11.png "De blade Azure AD management in de portal"
[11]: ./media/guidance-identity-aad/figure12.png "De console Azure AD verbinden"
[12]: ./media/guidance-identity-aad/figure13.png "Het tabblad bewerkingen in de Service Synchronisatiebeheer"
[13]: ./media/guidance-identity-aad/figure14.png "Het tabblad verbindingslijnen synchronisatie Service Manager"
[14]: ./media/guidance-identity-aad/figure15.png "De regeleditor synchronisatie"
[15]: ./media/guidance-identity-aad/figure16.png "De blade Azure Active Directory verbinding maken met de gezondheid in de portal voor de gezondheid van de synchronisatie met Azure"
[16]: ./media/guidance-identity-aad/figure17.png "De blade Azure Active Directory verbinding maken met de gezondheid in de Azure portal met de gezondheid van AD DS"
[17]: ./media/guidance-identity-aad/figure18.png "Beveiligingsrapporten beschikbaar in de portal Azure"
[18]: ./media/guidance-identity-aad/figure19.png "De Azure portal markeren het openbare IP-adres van de web-tier-taakverdeling"
[19]: ./media/guidance-identity-aad/figure20.png "Met behulp van Internet Explorer om te bladeren naar het openbare IP-adres van de web-tier-taakverdeling"
[20]: ./media/guidance-identity-aad/figure21.png "De module Certificaten met het certificaat www.contoso.com"
[21]: ./media/guidance-identity-aad/figure22.png "Verbinding maken met de management laag VM"
[22]: ./media/guidance-identity-aad/figure23.png "De HTTPS-binding voor de standaardwebsite maken"
[23]: ./media/guidance-identity-aad/figure24.png "De ContosoWebApp1-webtoepassing maken"
[24]: ./media/guidance-identity-aad/figure25.png "De verificatie-eigenschappen van de ContosoWebApp1-webtoepassing instellen"
[25]: ./media/guidance-identity-aad/figure26.png "Aanmelden bij Azure AAD van de webtoepassing ContosoWebApp1"
[26]: ./media/guidance-identity-aad/figure27.png "De map voor de standaardwebsite"