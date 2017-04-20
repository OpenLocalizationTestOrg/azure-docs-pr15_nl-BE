Identiteit beheren is net zo belangrijk in de openbare cloud als in gebouwen. Om te helpen met deze, Azure biedt ondersteuning voor verschillende technologieën voor verschillende cloud-identiteit. Deze cd bevat:

- U kunt Windows Server Active Directory (gewoonlijk alleen AD genoemd) uitvoeren in de cloud met behulp van virtuele machines met Azure Virtual machines gemaakt. Deze benadering is logisch wanneer u uw datacenter op ruimten uitbreiden naar de cloud via Azure.


- Azure Active Directory kunt u uw gebruikers eenmalige aanmelding geven tot toepassingen waarin [Software als Service (SaaS)](https://azure.microsoft.com/overview/what-is-saas/) . Van Microsoft Office 365 gebruikt deze technologie, bijvoorbeeld en ook door toepassingen die op de Azure of andere wolk platforms kunnen gebruiken.


- Toepassingen die worden uitgevoerd in de cloud of op locatie kunt Azure Active Directory-toegangsbeheer laat gebruikers zich aanmelden bij het gebruik van identiteiten van Facebook, Google, Microsoft en andere leveranciers van identiteit.


Alle drie van deze opties beschreven.

## <a name="table-of-contents"></a>Inhoudsopgave

- [Windows Server Active Directory uitvoeren in virtuele machines](#adinvm)

- [Met behulp van Azure Active Directory](#ad)

- [Azure Active Directory-toegangsbeheer](#ac)


## <a name="adinvm"></a>Windows Server Active Directory uitvoeren in virtuele machines

Windows Server AD uitgevoerd in Azure virtuele machines lijkt op locatie uitvoeren. [Figuur 1](#fig1) toont een voorbeeld van hoe dit er uitziet.

![Azure Active Directory in de virtuele Machine](./media/identity/identity_01_ADinVM.png)


<a name="Fig1"></a>Figuur 1: Windows Server Active Directory kan worden uitgevoerd in Azure virtuele machines van een organisatie op ruimten datacenter met Azure virtueel netwerk verbonden.

In het voorbeeld is Windows Server AD uitgevoerd in VMs gemaakt met behulp van Azure Virtual Machines, IaaS-technologie van het platform. Deze VMs en enkele andere zijn gegroepeerd in een virtueel netwerk is verbonden met een in de lokalen datacenter met Azure Virtual Network. Het virtuele netwerk carves uit een groep van cloud virtuele machines die met het netwerk op locatie via een virtueel particulier netwerk (VPN) verbinding communiceren. Deze kunt doen deze Azure virtuele machines zien eruit als een ander subnet naar het datacenter op gebouwen. Zoals in de afbeelding ziet, zijn twee van de VMs actief AD van Windows Server-domeincontrollers. De andere virtuele machines in het virtuele netwerk mogelijk toepassingen, zoals SharePoint of op een andere manier wordt gebruikt, zoals voor het ontwikkelen en testen worden uitgevoerd. Het datacenter in gebouwen wordt ook twee domeincontrollers van AD van Windows Server uitgevoerd.

Er zijn verschillende opties voor het aansluiten van de domeincontrollers in de cloud met die ruimten waarop:

- Maken allemaal deel uit van één Active Directory-domein.

- Maken van afzonderlijke AD domeinen op-lokalen en in de cloud die deel uitmaken van hetzelfde forest.

- Afzonderlijke forests voor AD maken in de cloud en op gebouwen en verbind de forests met cross-forest-vertrouwensrelaties of Windows Server Active Directory Federation Services (AD FS), die ook in de virtuele machines op Azure kunt uitvoeren.

Welke keuze wordt gemaakt, een beheerder moet ervoor zorgen dat verificatieaanvragen van gebruikers voor gebouwen gaat u naar de cloud domeincontrollers alleen indien nodig, omdat de koppeling naar de cloud is waarschijnlijk langzamer dan netwerken voor gebouwen. Een andere afweging bij de aansluitende wolk en domeincontrollers op de locatie is het verkeer dat wordt gegenereerd door middel van replicatie. Domeincontrollers in de cloud zijn meestal in hun eigen AD-site waarmee een beheerder om te plannen hoe vaak replicatie is uitgevoerd. Azure toeslagen voor verkeer dat wordt verzonden van een datacenter Azure, hoewel er geen bytes verzonden, die van invloed kunnen zijn op de keuzen van de beheerder van de replicatie. Het is ook ongepast dat terwijl Azure biedt eigen ondersteuning voor Domain Name System (DNS), deze service die vereist zijn voor Active Directory (bijvoorbeeld ondersteuning voor dynamische DNS- en SRV-records ontbreekt). Hierdoor moet Windows Server AD uitgevoerd op Azure instellen van uw eigen DNS-servers in de cloud.

Windows Server AD uitgevoerd in Azure VMs kan zinvol zijn in verscheidene verschillende situaties. Hier volgen enkele voorbeelden:

- Als u een uitbreiding van uw eigen datacenter Azure virtuele Machines gebruikt, kunt u toepassingen in de cloud die moeten lokale domeincontrollers te verwerken, zoals geïntegreerde Windows-verificatie aanvragen of LDAP-query's kunt uitvoeren. SharePoint, bijvoorbeeld communiceert regelmatig met Active Directory, en dus het is mogelijk uit te voeren van een SharePoint-farm op Azure met behulp van een directory op lokalen, instellen van domeincontrollers in de cloud wordt prestaties aanzienlijk verbeteren. (Het is belangrijk om te realiseren dat dit niet noodzakelijkerwijs vereist, echter; veel toepassingen kunnen probleemloos worden uitgevoerd in de cloud met alleen domeincontrollers op gebouwen).

- Stel dat veel filialen niet over de middelen om een eigen domeincontrollers worden uitgevoerd. Vandaag de dag moeten worden geverifieerd door de gebruikers op domeincontrollers aan de andere kant van de wereld - aanmeldingen zijn traag. Op Azure Active Directory wordt uitgevoerd in een Microsoft-datacenter dichter kunt versnellen dit zonder meer servers in het filiaal.

- Een organisatie die gebruikmaakt van Azure voor noodherstel kan een kleine set actieve VMs in de cloud, met inbegrip van een domeincontroller te handhaven. Deze kan vervolgens worden bereid om deze site zo nodig over te nemen voor niet-werkende ergens anders weer te geven.

Er zijn ook andere mogelijkheden. Zo bent u niet vereist voor Windows Server AD in de cloud verbinden met een datacenter op gebouwen. Als u wilt uitvoeren van een SharePoint-farm die een bepaalde groep gebruikers, bijvoorbeeld, van wie zou aanmelden uitsluitend met identiteiten cloud-gebaseerde, u kunt een zelfstandige forest maken op Azure. Het gebruik van deze technologie, is afhankelijk van wat uw doelstellingen zijn. (Voor meer gedetailleerde informatie over het gebruik van Windows Server AD met Azure, [Zie hier](http://msdn.microsoft.com/library/windowsazure/jj156090.aspx).)

## <a name="ad"></a>Met behulp van Azure Active Directory

SaaS-toepassingen komen meer en meer gemeenschappelijk, verhogen zij een duidelijke vraag: wat voor soort Active Directory moeten deze cloud-gebaseerde toepassingen gebruiken? Microsoft's antwoord op die vraag is Azure Active Directory.

Er zijn twee mogelijkheden voor het gebruik van deze adreslijstservice in de cloud:

- Personen en organisaties die gebruikmaken van SaaS-toepassingen kunnen vertrouwen op Azure Active Directory als hun enige Active Directory.

- Organisaties die Windows Server Active Directory uitvoeren kunnen verbinden hun directory op ruimten met Azure Active Directory en vervolgens gebruiken om aan te geven hun gebruikers eenmalige aanmelding tot SaaS-toepassingen.


[In afbeelding 2](#fig2) ziet u de eerste van deze twee opties, waar Azure Active Directory, is dit nodig is.

![Azure Active Directory in de virtuele Machine](./media/identity/identity_02_AD.png)

<a name="fig2"></a>Afbeelding 2: Azure Active Directory biedt een organisatie van gebruikers eenmalige aanmelding voor SaaS-toepassingen, waaronder Office 365.

Zoals in de afbeelding ziet, is Azure AD een huurder met meerdere service. Dit betekent dat veel verschillende organisaties het opslaan van informatie over gebruikers op elk van deze directory gelijktijdig kan worden ondersteund. In dit voorbeeld wordt probeert een gebruiker in een organisatie te krijgen een SaaS-toepassing. Deze toepassing kan onderdeel zijn van Office 365, zoals SharePoint Online of is het misschien iets anders - niet-Microsoft-toepassingen kunnen ook gebruikmaken van deze technologie. Omdat AD Azure ondersteunt het protocol SAML 2.0, die vereist is uit een toepassing is de mogelijkheid om te communiceren met behulp van deze standaard. (In feite toepassingen die gebruikmaken van AD Azure kunnen worden uitgevoerd in een datacenter, niet alleen een Azure datacenter.)

Het proces begint wanneer de gebruiker een SaaS-toepassing (stap 1 opent). Om deze toepassing gebruikt, moet de gebruiker een token dat wordt uitgegeven door AD Azure presenteren.

Deze token bevat informatie die de gebruiker identificeert en het digitaal is ondertekend door AD Azure. Als u de token, de gebruiker geverifieerd zelf naar Azure AD door middel van een gebruikersnaam en wachtwoord (stap 2). Azure AD vervolgens het token wordt hij nodig heeft (stap 3).

Deze token wordt vervolgens verzonden naar de SaaS-toepassing (stap 4), die het token van de handtekening worden gevalideerd en wordt de inhoud (stap 5). De toepassing gebruikt meestal informatie over de die het token bevat om te bepalen welke gegevens de gebruiker toegang en misschien op een andere manier is toegestaan.

Als de toepassing meer informatie over de gebruiker moet dan wat in het token opgenomen, kan hij dit verzoeken rechtstreeks uit Azure Active Directory met de Azure AD Graph API (stap 6). In de eerste versie van Azure AD het directory-schema is heel eenvoudig: het bevat alleen gebruikers en groepen en verbanden. Toepassingen kunnen deze informatie gebruiken voor meer informatie over verbindingen tussen gebruikers. Stel bijvoorbeeld dat een toepassing nodig heeft om te weten wie van de gebruiker manager is om te beslissen of hij de toegang tot een bepaalde hoeveelheid gegevens is toegestaan. Het kan dit meer opvragen Azure AD via de API van de grafiek.

De Graph API maakt gebruik van een gewone RESTful protocol, waardoor het eenvoudig te gebruiken van de meeste clients, met inbegrip van mobiele apparaten. De API ondersteunt ook de extensies die zijn gedefinieerd door de OData, bijvoorbeeld een querytaal waarmee clients toegang tot gegevens op meer nuttige manieren toe te voegen. (Zie voor meer informatie over OData [Introductie van OData](http://download.microsoft.com/download/E/5/A/E5A59052-EE48-4D64-897B-5F7C608165B8/IntroducingOData.pdf).) Omdat de Graph API kan worden gebruikt voor meer informatie over relaties tussen gebruikers, kunt toepassingen begrijpen de sociale grafiek die ingesloten in de Azure AD-schema voor een bepaalde organisatie (dat is de reden waarom het de Graph API wordt genoemd). En om te worden geverifieerd naar Azure AD voor aanvragen van Graph API, een toepassing gebruikt OAuth 2.0.

Als een organisatie niet Windows Server Active Directory gebruikt-heeft geen servers op de bedrijfsruimten of domeinen - en uitsluitend gebruikmaakt van cloud-toepassingen die gebruikmaken van AD Azure, zou alleen deze map wolk met behulp van de onderneming gebruikers eenmalige aanmelding al deze geven. Nog terwijl dit scenario meestal elke dag wordt, nog steeds gebruik van de meeste organisaties op-premises domeinen met Windows Server Active Directory gemaakt. Azure AD heeft een nuttige rol spelen hier ook, zoals [in figuur 3](#fig3) ziet.

![Azure Active Directory in de virtuele Machine](./media/identity/identity_03_AD.png)
<a id="fig3"></a>in figuur 3: een organisatie Windows Server Active Directory met Azure Active Directory voor de gebruikers eenmalige aanmelding tot SaaS-toepassingen kan communiceren.

In dit scenario krijgen B organisatie van een gebruiker kan toegang een SaaS-toepassing. Voordat ze dit doet, moeten een federation-relatie met Azure Active Directory met behulp van AD FS, zoals in de afbeelding maken voor directory-beheerders van de organisatie. Deze beheerders moet ook configureren voor synchronisatie van gegevens tussen de Server binnen de organisatie op ruimten Windows AD en Azure AD. Dit automatisch gekopieerd gebruikers- en groepsgegevens vanuit de directory op ruimten naar Azure AD. U ziet wat hierdoor: de organisatie is van kracht zijn op ruimten directory uitbreiden in de cloud. Combineert Windows Server AD en Azure AD op deze manier geeft de organisatie een directory-service die als één eenheid kan worden beheerd en toch een oppervlak hebben beide ruimten op en in de cloud.

Voor het gebruik van AD Azure, de gebruiker meldt zich eerst bij haar Active Directory-domein op ruimten zoals gebruikelijk (stap 1). Wanneer ze probeert te krijgen tot de SaaS-toepassing (stap 2), wordt het proces federation Azure AD haar afgifte van een token voor deze toepassing (stap 3). (Zie voor meer informatie over de werking van de Federatie [identiteit op basis van Claims voor Windows: technologieën en scenario's voor](http://www.davidchappell.com/writing/white_papers/Claims-Based_Identity_for_Windows_v3.0--Chappell.docx).) Vóór deze token bevat informatie die de gebruiker identificeert en het digitaal is ondertekend door AD Azure. Deze token wordt vervolgens verzonden naar de SaaS-toepassing (stap 4), die het token van de handtekening worden gevalideerd en wordt de inhoud (stap 5). En in het vorige scenario de toepassing de Graph API gebruiken kunt om meer te weten over deze gebruiker als SaaS nodig (stap 6).

Azure AD is vandaag de dag niet voor bedrijfsruimten Windows Server AD volledig te vervangen. Zoals reeds vermeld, de cloud-directory is een veel eenvoudiger schema en ontbreekt ook dingen zoals Groepsbeleid, de mogelijkheid voor het opslaan van informatie over machines, en ondersteuning voor LDAP. (In feite een Windows-computer kan niet worden geconfigureerd zodat gebruikers zich kunnen aanmelden via niets anders dan AD Azure - dit is een ondersteund scenario niet.) In plaats daarvan de oorspronkelijke doelstellingen van Azure advertentie opnemen zodat gebruikers toegang tot bedrijfstoepassingen in de cloud zonder behoud van een aparte login en vrij te maken op-premises directory-beheerders handmatig synchroniseert hun directory op ruimten met elke SaaS-applicatie die maakt gebruik van hun organisatie. Na verloop van tijd echter verwachten dat deze adreslijstservice wolk met een oplossing voor een breder scala van scenario's.

## <a name="ac"></a>Azure Active Directory-toegangsbeheer

Identiteit cloud-gebaseerde technologieën kunnen worden gebruikt om een groot aantal problemen oplossen. Azure Active Directory kunt geven van een organisatie gebruikers eenmalige aanmelding meerdere SaaS-toepassingen, bijvoorbeeld. Maar identiteit technologieën in de cloud kunnen ook op andere manieren worden gebruikt.

Stel bijvoorbeeld dat een toepassing wil haar gebruikers laten aanmelden met tokens die worden uitgegeven door meerdere *id-providers (IdPs)*. Veel providers van andere identiteit bestaat vandaag, met inbegrip van Facebook, Google, Microsoft en anderen, en toepassingen vaak laat gebruikers zich kunnen aanmelden met behulp van een van deze identiteiten. Waarom moet een toepassing banders behoudt een eigen lijst met gebruikers en wachtwoorden wanneer deze in plaats daarvan vertrouwen op identiteiten die al bestaan? Accepteert bestaande identiteiten maakt uw leven eenvoudiger voor gebruikers met een verminderd met gebruikersnaam en wachtwoord te onthouden en voor de mensen die de toepassing maken die niet meer hoeft te onderhouden hun eigen lijst met gebruikersnamen en wachtwoorden.

Maar terwijl elke identiteitsprovider een soort token problemen, deze tokens worden standaard niet - elke IdP een eigen indeling heeft. Bovendien, de informatie in deze tokens ook niet standaard. Een toepassing die u wenst te aanvaarden van tokens die worden uitgegeven door, zeg, Facebook, Google en Microsoft wordt geconfronteerd met de uitdaging van de unieke code schrijven voor het verwerken van elk van de verschillende indelingen.

Maar waarom dit doen? Waarom niet in plaats daarvan maakt een tussenpersoon die één token indeling met een algemene voorstelling van identiteitsgegevens kunt genereren? Deze aanpak zou een eenvoudiger leven voor de ontwikkelaars die toepassingen maken omdat ze nu nodig voor het verwerken van slechts één soort token. Precies dit wordt Azure Active Directory-toegangsbeheer voorzien in een tussenpersoon in de cloud werken met diverse tokens. [In figuur 4](#fig4) ziet hoe het werkt

![Azure Active Directory in de virtuele Machine](./media/identity/identity_04_IdentityProviders.png)
<a id="fig4"></a>in figuur 4: Azure Active Directory-toegangsbeheer kunt u gemakkelijker toepassingen identiteitstokens die zijn uitgegeven door een andere identiteitsproviders accepteren.

Het proces begint wanneer een gebruiker probeert toegang te krijgen tot de toepassing van een browser. De toepassing leidt tot een IdP van haar keuze (en de toepassing ook vertrouwensrelaties). Ze verifieert zichzelf aan deze IdP, bijvoorbeeld door het invoeren van een gebruikersnaam en wachtwoord (stap 1) en de IdP retourneert een token dat informatie bevat over haar (stap 2).

Zoals in de afbeelding kunt zien, ondersteunt toegangsbeheer op tal van verschillende cloud-gebaseerde IdPs, met inbegrip van de accounts die zijn gemaakt door Google, Yahoo, Facebook, een OpenID provider en Microsoft Windows Live ID (voorheen). Ondersteunt ook identiteiten met Azure Active Directory gemaakt en via federatie met AD FS, Windows Server Active Directory. De doelstelling is vandaag de dag voor de meest gebruikte identiteiten of ze zijn uitgegeven door IdPs in de cloud of op locatie.

Als een browser van de gebruiker heeft een token IdP van haar gekozen IdP, wordt dit token naar Access Control (stap 3). Toegangsbeheer valideert het token, ervoor te zorgen dat het echt is uitgegeven door deze IdP, en vervolgens maakt u een nieuwe beveiligingssleutel volgens de regels die zijn gedefinieerd voor deze toepassing. Toegangsbeheer is een service met meerdere huurder zoals Azure Active Directory, maar de huurder zijn toepassingen in plaats van organisaties van de klant. Elke toepassing een eigen naamruimte, zoals in de afbeelding kunt krijgen, en verschillende regels over verificatie en meer kunt definiëren.

Deze regels kunnen de beheerder van de toepassing definiëren hoe tokens van verschillende IdPs moeten worden omgezet in een token voor toegangsbeheer. Bijvoorbeeld als verschillende IdPs met verschillende typen voor het weergeven van gebruikersnamen, kunnen Access Control regels transformeren deze in een algemeen type in de gebruikersnaam. Deze nieuwe token stuurt toegangsbeheer naar de browser (stap 4), die worden ingediend door de toepassing (stap 5). Nadat het token van de toegang tot heeft, wordt de toepassing gecontroleerd dat deze token echt is uitgegeven door Access Control, en vervolgens gebruikt de informatie die deze bevat (Zie stap 6).

Tijdens dit proces een beetje ingewikkeld lijkt, eigenlijk is het leven aanzienlijk eenvoudiger voor de maker van de toepassing. In plaats van diverse tokens met verschillende gegevens verwerken, kan de toepassing identiteiten uitgegeven door meerdere identiteitsproviders tijdens het nog steeds slechts één token met bekende gegevens accepteren. In plaats van elke toepassing moet worden geconfigureerd voor vertrouwen in verschillende IdPs is vereist, ook deze vertrouwensrelaties in plaats daarvan worden beheerd door Access Control - alleen door een toepassing moet vertrouwen.

Het is ongepast dat niets over toegangsbeheer is gebonden aan de Windows - kan net zo goed worden gebruikt door een Linux-toepassing die alleen Google en Facebook identiteiten geaccepteerd. En hoewel Access Control onderdeel van de familie Azure Active Directory is, u kunt dit beschouwen als een geheel afzonderlijk service van wat in de vorige sectie is beschreven. Hoewel beide technologieën met identiteit werken, aanpakken ze heel anders problemen (Hoewel Microsoft heeft gezegd dat het voor de integratie van de twee op een bepaald moment verwacht).

Werken met identiteit is belangrijk in bijna elke toepassing. Het doel van toegangsbeheer is het makkelijker voor ontwikkelaars toepassingen maken die identiteiten uit diverse id-providers te accepteren. De toevoeging van deze service in de cloud, heeft Microsoft gemaakt beschikbaar voor elke toepassing op elk platform worden uitgevoerd.

##<a name="about-the-author"></a>Over de auteur

David Chappell is Principal van Chappell & Associates [www.davidchappell.com](http://www.davidchappell.com) in San Francisco, Californië.
