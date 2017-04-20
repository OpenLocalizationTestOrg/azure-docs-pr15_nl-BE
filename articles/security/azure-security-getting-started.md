<properties
   pageTitle="Aan de slag met Azure Microsoft security | Microsoft Azure"
   description="Dit artikel bevat een overzicht van de beveiligingsmogelijkheden van Microsoft Azure en algemene overwegingen voor organisaties die hun activa naar een cloud provider migreert."
   services="security"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="05/19/2016"
   ms.author="yuridio"/>

#<a name="getting-started-with-microsoft-azure-security"></a>Aan de slag met Azure Microsoft beveiliging

Als u bouwt of migreren van IT-middelen op een wolk provider, u erop vertrouwt dat de mogelijkheden van die organisatie ter bescherming van de toepassingen en gegevens die u opdragen hun diensten en bieden ze u voor het beveiligen van uw activa cloud-gebaseerde beveiliging besturingselementen.

Infrastructuur van Azure is ontworpen van de faciliteit voor toepassingen voor miljoenen klanten tegelijk hosting en een betrouwbare basis waarop bedrijven hun beveiliging behoeften kunnen bieden. Bovendien biedt Azure u een groot aantal configureerbare beveiligingsopties en de mogelijkheid om ze te beheren zodat u kunt beveiliging om te voldoen aan de unieke vereisten voor de distributie.

In dit overzichtsartikel op Azure beveiliging, zullen we:

-   Azure services en functies die u gebruiken kunt voor de beveiliging van uw services en gegevens in Azure

-   Hoe Microsoft beveiligt de Azure infrastructuur voor het beschermen van uw gegevens en toepassingen

##<a name="identity-and-access-management"></a>Identiteits-en


Toegang tot IT-infrastructuur, gegevens en toepassingen beheren is essentieel. Deze mogelijkheden worden in Microsoft Azure geleverd door services zoals Azure Active Directory en Azure Storage support voor talloze standaarden en API's.

[Azure Active Directory](../active-directory/active-directory-whatis.md) (Azure AD) is een opslagplaats van de identiteit en de motor met verificatie, autorisatie en toegangsbeheer voor gebruikers, groepen en objecten van een organisatie. AD Azure biedt ontwikkelaars ook een effectieve manier om te integreren in hun toepassingen van identity management. Industriestandaardprotocollen zoals [SAML 2.0](https://en.wikipedia.org/wiki/SAML_2.0), [WS-Federation](https://msdn.microsoft.com/library/bb498017.aspx)en [OpenID verbinding](http://openid.net/connect/) maakt mogelijk aanmelden op verschillende platforms zoals Java, .NET, Node.js en PHP.

De grafiek op basis van REST API kunnen ontwikkelaars om te lezen en schrijven naar de map van een willekeurig platform. Dankzij de ondersteuning voor [OAuth 2.0](http://oauth.net/2/), kunnen ontwikkelaars mobiele en webtoepassingen die worden geïntegreerd met Microsoft en derden web API's en hun eigen secure web API's. Open-source client-bibliotheken zijn beschikbaar voor .net, Windows Store, iOS en Android met aanvullende bibliotheken in ontwikkeling.

### <a name="how-azure-enables-identity-and-access-management"></a>Hoe Azure identiteits-en kunnen

Azure AD kan worden gebruikt als een zelfstandige cloud-map voor uw organisatie of als een geïntegreerde oplossing met de bestaande Active Directory op gebouwen. Sommige integratiefuncties zijn directory-synchronisatie en eenmalige aanmelding (SSO). Deze vergroot het bereik van bestaande identiteiten op gebouwen in de cloud en het beheer en de eindgebruiker verbeteren.

Enkele andere mogelijkheden voor het beheer van identiteits- en toegangsbeheer zijn:

-   Azure AD kunt [SSO](https://azure.microsoft.com/documentation/videos/overview-of-single-sign-on/) SaaS-toepassingen, ongeacht waar deze worden gehost. Sommige toepassingen zijn federatieve met Azure Active Directory en andere wachtwoord SSO gebruiken. Federatieve toepassingen ondersteunen ook gebruikersaanvragen en vaulting wachtwoord.

-   Toegang tot gegevens in [Azure opslag](https://azure.microsoft.com/services/storage/) wordt geregeld via verificatie. Elke Account opslag heeft een primaire sleutel ([Opslag Account sleutel](https://msdn.microsoft.com/library/azure/ee460785.aspx)of SAK) en secundaire geheime sleutel (gedeelde toegang handtekening of SAS).

-   Azure AD biedt identiteit als een Service door middel van federation (met [Active Directory Federation Services](../active-directory/fundamentals-identity.md), synchronisatie en replicatie van mappen voor gebouwen.

-   [Azure meerledige verificatie (MVR gesloten)](../multi-factor-authentication/multi-factor-authentication.md) is de service meerledige verificatie moet gebruikers ook verifiëren van aanmeldingen met behulp van een mobiele app, een telefoongesprek of een SMS-bericht. Het is beschikbaar voor gebruik met AD Azure, voor het beveiligen van resources in ruimten met de Server van de MVR gesloten Azure en met aangepaste toepassingen en de mappen met de SDK.

-   [Azure AD Domain Services](https://azure.microsoft.com/services/active-directory-ds/) kunt u Azure virtuele machines toevoegen aan een domein zonder te hoeven implementeren van domeincontrollers. Gebruikers kunnen zich aanmelden bij deze virtuele machines met hun corporate Active Directory-referenties en beheren van een domein behoren virtuele machines met behulp van groepsbeleid afdwingen van beveiliging basislijnen voor al uw Azure virtuele machines.

-   [Azure Active Directory B2C](https://azure.microsoft.com/services/active-directory-b2c/) biedt een hoge beschikbaarheid, wereldwijde, identiteit management service voor de consument gerichte toepassingen die wordt aangepast aan de honderden miljoenen identiteiten. Het kan worden geïntegreerd in mobiele en web-platforms. De consumenten kunnen aanmelden bij uw toepassingen via aanpasbare ervaringen met hun bestaande sociale accounts of door het maken van nieuwe referenties.

##<a name="data-access-control-and-encryption"></a>Toegangsbeheer voor gegevens en codering

Microsoft maakt gebruik van het beginsel van scheiding van taken en [Minimale bevoegdheden](https://en.wikipedia.org/wiki/Principle_of_least_privilege) in Azure bewerkingen. Toegang tot gegevens door ondersteuningspersoneel Azure uw uitdrukkelijke toestemming is vereist en wordt toegekend op "just-in-time" basis die is vastgelegd en gecontroleerd, vervolgens ingetrokken na de voltooiing van de overeenkomst.

Azure biedt bovendien meerdere mogelijkheden voor het beveiligen van gegevens in transit en in rust, met inbegrip van versleuteling voor gegevens, bestanden, toepassingen, services, communicatie en stations. U hebt de optie voor het coderen van gegevens voordat in Azure plaatst als sleutels op te slaan in de datacenters van gebouwen.

![Microsoft Antimalware in Azure](./media/azure-security-getting-started/sec-azgsfig1.PNG)

### <a name="azure-encryption-technologies"></a>Azure coderingstechnologieën

U kunt details op beheerderstoegang via [Azure AD-rapportage](../active-directory/active-directory-reporting-audit-events.md)voor uw abonnement omgeving verzamelen. U hebt de optie voor het configureren van [BitLocker-stationsversleuteling](https://technet.microsoft.com/library/cc732774.aspx) op VHD's met gevoelige informatie in Azure.

Er zijn andere mogelijkheden in Azure waarmee u uw gegevens te beveiligen:

-   Softwareontwikkelaars kunnen maken codering in de toepassingen die ze in Azure implementeren met behulp van het Windows [CryptoAPI](https://msdn.microsoft.com/library/ms867086.aspx) en .NET Framework.

- Client side codering voor Microsoft blob-opslag kunt u de sleutels volledig beheren.  De storage-service nooit ziet de sleutels en is niet in staat de gegevens te decoderen.

-   [Azure RMS](https://technet.microsoft.com/library/jj585026.aspx) (met de [RMS-SDK](https://msdn.microsoft.com/library/dn758244.aspx) bevat bestands- en niveau codering en gegevens lekken te voorkomen door middel van toegangsbeheer op basis van beleid.

-   Azure ondersteunt [codering van tabel- en kolomniveau TDE/CLE ()](http://blogs.msdn.com/b/sqlsecurity/archive/2015/05/12/recommendations-for-using-cell-level-encryption-in-azure-sql-database.aspx) in SQL Server virtuele Machines en derden ondersteunt op-premises Sleutelbeheer servers in datacenters van klanten.

-   Opslag Account sleutels, gedeelde toegang handtekeningen, beheer van certificaten en andere sleutels zijn uniek voor elke huurder Azure.

-   Azure [StorSimple](http://www.microsoft.com/server-cloud/products/storsimple/overview.aspx) hybride opslag codeert gegevens via een 128-bits openbare / persoonlijke sleutelpaar voor uploaden naar Azure opslag.

-   Azure ondersteunt en talloze methoden, zoals SSL/TLS, IPsec en AES, afhankelijk van de gegevenstypen en containers en transporten gebruikt.

##<a name="virtualization"></a>Virtualisatie

De Azure platform maakt gebruik van een gevirtualiseerde omgeving. Gebruikersexemplaren kunnen fungeren als zelfstandige virtuele machines die geen toegang tot een fysieke hostserver en deze isolatie wordt afgedwongen met fysieke [processor niveaus (ring-0/ring-3)](https://en.wikipedia.org/wiki/Protection_ring).

Ring 0 is de beschermde en 3 de minste. Het gastbesturingssysteem wordt uitgevoerd in een minder bevoorrechte Ring 1 en toepassingen in de laatste 3 Ring bevoorrecht. Deze virtualisatie van fysieke middelen leidt tot een duidelijke scheiding tussen Gast OS en hypervisor, waardoor de scheiding tussen de twee extra beveiliging.

De Hypervisor van Azure fungeert als een micro-kernel en geeft alle hardware-aanvragen van VMs Gast aan de host voor verwerking via een gedeeld geheugen interface VMBus aangeroepen. Dit voorkomt dat gebruikers ruwe lezen/schrijven/uitvoeren toegang krijgen tot het systeem en vermindert het risico van het delen van systeembronnen.

![Microsoft Antimalware in Azure](./media/azure-security-getting-started/sec-azgsfig2.PNG)

### <a name="how-azure-implements-virtualization"></a>Hoe Azure geïmplementeerd voor virtualisatie

Azure maakt gebruik van een firewall, hypervisor (packet filter), die is geïmplementeerd in de hypervisor en geconfigureerd door een agent fabric-controller. Dit helpt huurders te beschermen tegen onrechtmatige toegang. Standaard bij het maken van een VM al het gegevensverkeer en de agent fabric-controller wordt geconfigureerd de pakketfilter toevoegen, *regels en uitzonderingen* voor verkeer toegelaten.

Er zijn twee categorieën met regels die hier worden geprogrammeerd:

-   **Configuratie van de machine of infrastructuur regels**: standaard alle communicatie wordt geblokkeerd. Er zijn uitzonderingen op een VM voor het verzenden en ontvangen van DHCP en DNS-verkeer toestaan. VMs ook verkeer worden verzonden naar het internet 'public' en verkeer worden verzonden naar andere VMs binnen het cluster en OS-activeringsserver. De VMs bevat geen lijst met bestemmingen uitgaande Azure router subnetten, Azure management back-end en andere eigenschappen van Microsoft toegestaan.

-   **Rol-configuratiebestand**: Hiermee definieert u de binnenkomende ACL's op basis van de huurder service-model. Bijvoorbeeld, als een huurder een Web front-end voor poort 80 op een bepaalde VM heeft, Azure wordt geopend TCP-poort 80 naar alle IP-adressen als u een eindpunt in het model [Azure Service Management configureren bent](../resource-manager-deployment-model.md) . Als een back-end of werknemer rol met de VM, openen we de rol van de werknemer alleen voor VM binnen de dezelfde huurder.

##<a name="isolation"></a>Isolatie

Scheiding om te voorkomen dat onbevoegde of onbedoelde overdracht van gegevens tussen de implementaties in een gedeelde architectuur met meerdere huurder onderhouden is een andere belangrijke wolk beveiligingseisen.

Azure implements [Netwerktoegangsbeheer](https://azure.microsoft.com/blog/network-isolation-options-for-machines-in-windows-azure-virtual-networks/) en scheiding door middel van VLAN-isolatie, ACL's, balancers en IP-filters laden. Extern netwerkverkeer dat uw virtual machine(s) is beperkt tot de poorten en protocollen die u definieert. Netwerk filteren om te voorkomen dat vervalste verkeer wordt geïmplementeerd en binnenkomende en uitgaande verkeer wordt beperkt tot vertrouwde platformonderdelen. Verkeer stroom beleid wordt geïmplementeerd op de grens beveiligingsinrichtingen die verkeer weigeren standaard.

![Microsoft Antimalware in Azure](./media/azure-security-getting-started/sec-azgsfig3.PNG)

NAT (Network Address Translation) wordt gebruikt voor het interne netwerkverkeer van extern verkeer te scheiden. Intern verkeer is niet extern routeerbaar. Extern routeerbaar zijn [virtuele IP-adressen](http://blogs.msdn.com/b/cloud_solution_architect/archive/2014/11/08/vips-dips-and-pips-in-microsoft-azure.aspx) worden omgezet in [Interne dynamische IP-](http://blogs.msdn.com/b/cloud_solution_architect/archive/2014/11/08/vips-dips-and-pips-in-microsoft-azure.aspx) adressen die zijn alleen geschikt in Azure.

Extern verkeer Azure virtuele machines wordt met firewall via toegangsbeheerlijsten (ACL's) op de routers, Netwerktaakverdeling, en Layer 3-switches. Alleen specifieke bekende protocollen zijn toegestaan. ACL's zijn voor het beperken van verkeer dat afkomstig is van VMs Gast aan andere VLAN's gebruikt voor beheer. Daarnaast netwerkverkeer via IP-filters op de host OS gefilterd, verder beperken het verkeer op beide data-link en netwerk lagen.

### <a name="how-azure-implements-isolation"></a>Hoe Azure geïmplementeerd isolatie

De Controller Azure Fabric is verantwoordelijk voor het toewijzen van resources om de werklast pachters infrastructuur en beheert unidirectionele communicatie van de host de VMs. De hypervisor Azure geheugen- en scheiding tussen VMs afdwingt en veilig netwerkverkeer routeert naar de Gast OS huurders. Azure implementeert ook isolatie voor huurders, opslag en virtuele netwerken:

-   Elke huurder Azure AD is logisch geïsoleerd met beveiligingsgrenzen.

-   Azure opslag rekeningen zijn uniek voor elk abonnement en toegang moet worden geverifieerd met behulp van een sleutel opslag Account.

-   Virtuele netwerken zijn logisch gescheiden door middel van een combinatie van unieke particuliere IP-adressen, firewalls en IP-ACL's. Load balancers route verkeer naar de juiste huurder op basis van de definities van het eindpunt.

##<a name="virtual-network-and-firewall"></a>Virtueel netwerk- en firewallvereisten

De [gedistribueerde en virtuele netwerken](http://download.microsoft.com/download/4/3/9/43902EC9-410E-4875-8800-0788BE146A3D/Windows%20Azure%20Network%20Security%20Whitepaper%20-%20FINAL.docx) in Azure ervoor te zorgen dat uw particuliere netwerkverkeer van verkeer op andere Azure virtuele netwerken logisch gescheiden.

![Microsoft Antimalware in Azure](./media/azure-security-getting-started/sec-azgsfig4.PNG)

Uw abonnement en kan bevatten meerdere geïsoleerde particuliere netwerken (firewall, load balancing en netwerkadresomzetting).

Azure biedt drie primaire netwerk scheiding in elke cluster Azure logisch verkeer te scheiden. [Virtual Local Area Networks](https://azure.microsoft.com/services/virtual-network/) (VLAN's) worden gebruikt voor klantverkeer wordt gescheiden van de rest van het netwerk Azure. Toegang tot de Azure netwerk van buiten het cluster is beperkt tot en met netwerktaakverdeling.

Netwerkverkeer naar en van VMs verloopt via de hypervisor virtual switch. Het onderdeel IP-filter in het basis-besturingssysteem de hoofdmap VM uit de VMs Gast en de Gast VMs van elkaar worden geïsoleerd. Het filteren van verkeer voor het beperken van de communicatie tussen de knooppunten van de huurder en het openbare Internet (op basis van de configuratie van de service van de klant), ze scheiden van andere huurders wordt uitgevoerd.

Het IP-filter voorkomt VMs Gast uit:

- Vervalste verkeer genereren

- Ontvangen van verkeer dat niet tot hen gerichte

- Het routeren van verkeer naar beveiligde infrastructuur eindpunten

- Verzenden of ontvangen van broadcast-verkeer niet geschikt

U kunt uw virtuele machines op [Virtuele netwerken Azure](https://azure.microsoft.com/documentation/services/virtual-network/)plaatsen. Deze virtuele netwerken zijn vergelijkbaar met de netwerken die u in een omgeving voor ruimten configureert waar ze zijn meestal gekoppeld aan een virtuele switch. Virtuele machines verbonden met hetzelfde virtuele Azure netwerk communiceren met elkaar zonder aanvullende configuratie. U hebt ook de optie voor het configureren van verschillende subnetten binnen het virtuele netwerk Azure.

U kunt de volgende technologieën van Azure Virtual Network om beveiligde communicatie in het virtuele netwerk Azure:

-   [**Netwerk-beveiligingsgroepen (NSG)**](../virtual-network/virtual-networks-nsg.md). U kunt een NSG om verkeer naar een of meer exemplaren van de virtuele machine (VM) voor beheer in het virtuele netwerk. Een NSG bevat regels voor toegang tot die toestaan of weigeren op basis van het verkeer richting, protocol, bronadres en poort, en doeladres en poort verkeer.

-   [**Door de gebruiker gedefinieerde routering**](../virtual-network/virtual-networks-udr-overview.md). U kunt bepalen de routering van pakketten via een virtueel toestel door de gebruiker gedefinieerde routes die de volgende hop voor pakketten die doorloopt op een bepaald subnet geven wilt gaan maken een uw beveiligingsapparaat virtueel netwerk.

-   [**Doorsturen via IP**](../virtual-network/virtual-networks-udr-overview.md). Een virtueel netwerk beveiliging moet zich kunnen ontvangen van binnenkomend verkeer dat niet aan zichzelf wordt behandeld. Als u wilt toestaan dat een VM verkeer is geadresseerd aan andere bestemmingen ontvangt, u IP-doorsturen inschakelen voor VM.

-   [**Gedwongen tunneling**](../vpn-gateway/vpn-gateway-about-forced-tunneling.md). Geforceerde tunneling kunt u omleiden of alle Internet-verkeer dat gegenereerd door uw virtuele machines in een virtuele Azure ' dwingen' terug naar uw locatie op de locatie via een VPN-tunnel van site naar site voor inspectie en controle

-   [ **Eindpunt** ACL's](../virtual-machines/virtual-machines-windows-classic-setup-endpoints.md). U kunt bepalen welke computers zijn toegestaan binnenkomende verbindingen vanaf het Internet met een virtuele machine op uw Azure virtueel netwerk door te definiëren, eindpunt ACL's.

-   [**Oplossingen voor netwerkbeveiliging partner**](https://azure.microsoft.com/marketplace/). Er zijn een aantal partneroplossing voor netwerkbeveiliging die u vanuit de markt Azure openen kunt.

### <a name="how-azure-implements-virtual-networks-and-firewall"></a>Hoe Azure geïmplementeerd virtuele netwerken en firewall

Firewalls met pakketfiltering Azure geïmplementeerd op alle host en Gast VMs standaard. Afbeeldingen in de galerie Azure Windows OS hebben ook Windows Firewall standaard ingeschakeld. Netwerktaakverdeling op de omtrek van de Azure openbare gerichte netwerken controleren van communicatie op basis van IP-ACL's beheerd door beheerders van de klant.

Als de gegevens van een klant Azure als onderdeel van de normale werking of tijdens een ramp verplaatst, dit gebeurt via versleutelde communicatiekanalen. Er zijn andere mogelijkheden die Azure ontregeld in virtuele netwerken en een firewall te gebruiken:

-   **Native hostfirewall**: Azure weefsel en opslag uitvoeren op een native OS waarvoor geen hypervisor en dus de windows firewall is geconfigureerd met de bovenstaande twee sets van regels. Opslag optimaliseren native uitgevoerd.

-   **Host-Firewall**: de hostfirewall is het beschermen van het hostbesturingssysteem, waarbij de hypervisor wordt uitgevoerd. De regels zijn geprogrammeerd om alleen de controller fabric toestaan en vakken te praten met de host OS op een specifieke poort te springen. Andere uitzonderingen zijn reactie van de DHCP-en DNS-antwoorden. Azure gebruikt een machine configuratiebestand die de sjabloon van firewall-regels voor het hostbesturingssysteem. De host zelf wordt beschermd tegen externe aanvallen door Windows firewall zodanig geconfigureerd dat alleen communicatie via bekende, geverifieerde bronnen.

-   **Gast Firewall**: de regels in het pakketfilter VM Switch maar geprogrammeerde in verschillende software (dat wil zeggen de Windows Firewall stuk van de Gast OS) wordt gerepliceerd. De Gast VM firewall kan worden geconfigureerd om communicatie beperken of naar de Gast VM, zelfs als de communicatie is toegestaan door de configuraties op de host-IP-Filter. U kunt bijvoorbeeld de firewall Gast VM gebruiken voor het beperken van de communicatie tussen twee van de VNets die zijn geconfigureerd om met elkaar te verbinden.

-   **Opslag-Firewall (FW)**: de firewall op de front-end-opslag verkeer op poorten 80/443 en andere noodzakelijke hulpprogramma poorten worden gefilterd. De firewall op de back-end opslag beperkt de communicatie om alleen afkomstig zijn van front-end opslagservers.

-   **Virtuele netwerkgateway**: [Azure virtuele netwerkgateways](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md) fungeren als het kruispunt bedrijfsruimten gateways uw werkbelasting in Azure virtueel netwerk verbinding te maken met uw op sites van gebouwen. Verbinding maken met op ruimten sites via [IPSec-site naar site VPN-tunnels](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md)of [ExpressRoute](../expressroute/expressroute-introduction.md) circuits is verplicht. Gateways IKE handshakes uitvoeren en tot stand brengen van de IPSec-S2S VPN-tunnels tussen de virtuele netwerken en op de sites van de lokalen voor IPsec IKE/VPN-tunnels. Virtueel netwerkgateways beëindigen ook [punt-naar-site VPN-verbindingen](../vpn-gateway/vpn-gateway-point-to-site-create.md).

##<a name="secure-remote-access"></a>Externe toegang beveiligen

Gegevens die zijn opgeslagen in de cloud moet voldoende waarborgen om misbruik te voorkomen en de vertrouwelijkheid en integriteit terwijl in transit is ingeschakeld. Dit omvat netwerkfuncties die bij een organisatie van beleid, controleerbare identiteits- en regelingen voor het beheer aansluiten.

Ingebouwde cryptographic-technologie kunt u versleutelen van de communicatie binnen en tussen tussen Azure regio's en van Azure implementaties voor datacenters voor gebouwen. Beheerderstoegang tot de virtuele machines via [Extern bureaublad-sessies](../virtual-machines/virtual-machines-windows-classic-connect-logon.md), [Externe Windows PowerShell](http://blogs.technet.com/b/heyscriptingguy/archive/2013/09/07/weekend-scripter-remoting-the-cloud-with-windows-azure-and-powershell.aspx)en de Azure Management Portal worden altijd gecodeerd.

Om uit te breiden veilig uw datacenter op het bedrijf naar de cloud, Azure biedt zowel [VPN van site tot site](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md) en [punt-naar-site VPN](../vpn-gateway/vpn-gateway-point-to-site-create.md), alsmede specifieke verbindingen met [ExpressRoute](../expressroute/expressroute-introduction.md) (verbindingen met Azure virtuele netwerken via het VPN gecodeerd).

### <a name="how-azure-implements-secure-remote-access"></a>Hoe Azure geïmplementeerd voor veilige externe toegang

Verbindingen met Azure Portal altijd moeten worden geverifieerd en SSL/TLS vereist. U kunt certificaten beheer zodat een veilige manier beheren. Beveiligde standaardprotocollen zoals [SSTP](https://technet.microsoft.com/magazine/2007.06.cableguy.aspx) en [IPsec](https://en.wikipedia.org/wiki/IPsec) worden volledig ondersteund.

[Azure ExpressRoute](../expressroute/expressroute-introduction.md) kunt u particuliere verbindingen maken tussen infrastructuur die op uw locatie of in een omgeving met collocatie en Azure datacenters. ExpressRoute verbindingen gaan niet via het openbare Internet. Zij bieden meer betrouwbaarheid, hogere snelheden, lagere vertragingstijden en een hogere mate van beveiliging dan typische koppelingen op basis van het Internet. In sommige gevallen ExpressRoute verbindingen gebruiken om gegevens te verzenden tussen lokale en Azure kan ook worden verkregen aanzienlijke kostenvoordelen.

##<a name="logging-and-monitoring"></a>Het vastleggen en bewaken

Azure biedt geverifieerde beveiliging relevante gebeurtenissen die een audittrail genereren vastleggen en is ontworpen om te zijn tegen manipulatie. Het gaat hierbij om systeemgegevens, zoals het beveiligingslogboek in Azure VMs-infrastructuur en AD Azure. Security event-bewaking omvat het verzamelen van gebeurtenissen zoals wijzigingen in de DHCP- of DNS-server IP-adressen, poging tot toegang tot havens, protocollen of IP-adressen die worden geblokkeerd door ontwerpen, wijzigingen in de beveiligingsinstellingen voor Groepsbeleid of firewall, account of groep maken, onverwachte processen of installatie van het stuurprogramma.

![Microsoft Antimalware in Azure](./media/azure-security-getting-started/sec-azgsfig5.PNG)

Controlelogboeken bevoorrechte toegang en activiteiten, gemachtigde en niet-geautoriseerde toegangspogingen systeem uitzonderingen en beveiligingsgebeurtenissen worden bewaard gedurende een bepaalde tijd informatie opnemen. De handhaving van de logboekbestanden is naar eigen inzicht, omdat u verzamelen en bewaren aan uw eigen behoeften.

### <a name="how-azure-implements-logging-and-monitoring"></a>Hoe Azure geïmplementeerd voor het vastleggen en bewaken

Azure implementeert Management-Agents (MA) en Azure Security Monitor (ASM)-agents aan elk compute-, opslag- of fabric knooppunt onder beheer van native of virtuele. Elke Management Agent is geconfigureerd om te worden geverifieerd bij een service-account team opslag met een certificaat uit het certificaatarchief Azure verkregen en vooruit vooraf geconfigureerd diagnostische en gebeurtenisgegevens naar de rekening voor de opslag. Deze agenten zijn niet geïmplementeerd op virtuele machines voor klanten.

Azure beheerders toegang tot logboeken via een webportal voor geverifieerde en gecontroleerde toegang tot de logboeken. Een beheerder kan parseren, filteren, correleren en logboekbestanden analyseren. De Azure team opslag serviceaccounts voor logboeken worden beschermd tegen directe toegang om te voorkomen dat tegen knoeien logboek.

Microsoft verzamelt logboeken van netwerkapparaten met behulp van het protocol Syslog en host-servers met behulp van Audit collectie Services (ACS). Deze logboekbestanden worden opgeslagen in een logboekdatabase waarvan waarschuwingen worden gegenereerd voor verdachte gebeurtenissen rechtstreeks naar een Microsoft-beheerder. De beheerder kan bekijken en analyseren van deze logboeken.

[Azure diagnostische gegevens](https://msdn.microsoft.com/library/azure/gg433048.aspx) is een functie van Azure waarmee u kunt diagnostische gegevens verzamelen van een toepassing wordt uitgevoerd in Azure. Dit is de diagnostische gegevens voor foutopsporing en probleemoplossing, meten van de prestaties, Resourcegebruik, analyse van het netwerkverkeer en capaciteitsplanning monitoring en controle. Nadat de diagnostische gegevens worden verzameld, kan deze worden overgebracht naar een account Azure opslag voor persistentie. Overdrachten of kunnen worden gepland of op aanvraag.

##<a name="threat-mitigation"></a>Beperkende factoren voor bedreiging

Naast isolatie, codering en filters in Azure dienst een aantal bedreiging mitigation mechanismen en processen te beschermen van infrastructuur en diensten. Hieronder vallen interne controles en technologieën voor het opsporen en verhelpen van geavanceerde bedreigingen zoals DDoS escalatie van bevoegdheden en de [OWASP Top 10](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project).

De besturingselementen voor beveiliging en risicobeheerprocedures die Microsoft heeft voor het beveiligen van de cloud-infrastructuur verminderen het risico van beveiligingsincidenten. Maar in het geval van een incident voordoet, het team Security Incident Management (SIM) in het team Services van Microsoft on line beveiliging & naleving (OSSC) is nu 24 x 7 te beantwoorden.

### <a name="how-azure-implements-threat-mitigation"></a>Hoe Azure geïmplementeerd gevaar beperken

Azure beveiliging heeft genomen risicobeperkende bedreiging implementeren en klanten helpen beperken mogelijke dreigingen in hun omgeving. De onderstaande lijst bevat een overzicht van de bedreiging risicobeperking mogelijkheden aangeboden door Azure:

-   [Azure Anti-Malware](../security/azure-security-antimalware.md) is standaard ingeschakeld op alle infrastructuurservers. Desgewenst kunt u deze in uw eigen VMs.

-   Microsoft onderhoudt een voortdurende controle op servers, netwerken en toepassingen bedreigingen detecteren en voorkomen van misbruik. Geautomatiseerde waarschuwingen melden beheerders van afwijkende gedrag, waardoor de corrigerende maatregelen nemen op interne en externe bedreigingen.

-   U hebt de optie voor het implementeren van 3de partij beveiligingsoplossingen in uw abonnementen, zoals web application firewalls van [Barracuda](https://techlib.barracuda.com/ng54/deployonazure).

-   Aanpak van Microsoft voor het testen van penetratie bevat '[Rood teamwork](http://download.microsoft.com/download/C/1/9/C1990DBA-502F-4C2A-848D-392B93D9B9C3/Microsoft_Enterprise_Cloud_Red_Teaming.pdf)", waarbij Microsoft security professionals (niet-klant) live productiesystemen in Azure testen verdedigingen tegen real-world, geavanceerde persistente dreigingen aanvallen.

-   Implementatie geïntegreerde systemen beheren de distributie en installatie van beveiligingspatches op de Azure-platform.

##<a name="next-steps"></a>Volgende stappen

[Azure Vertrouwenscentrum](https://azure.microsoft.com/support/trust-center/)

[Azure Security teamblog](http://blogs.msdn.com/b/azuresecurity/)

[Microsoft Security Response Center](https://technet.microsoft.com/library/dn440717.aspx)

[Active Directory-Blog](http://blogs.technet.com/b/ad/)
