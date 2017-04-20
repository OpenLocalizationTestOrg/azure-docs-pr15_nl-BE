<properties
   pageTitle="Richtlijnen voor de implementatie van Windows Server Active Directory op Azure virtuele Machines | Microsoft Azure"
   description="Als u het implementeren van AD Domain en AD Federation Services in gebouwen, leren hoe ze werken op Azure virtuele machines."
   services="active-directory"
   documentationCenter=""
   authors="femila"
   manager="stevenpo"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/27/2016"
   ms.author="femila"/>

# <a name="guidelines-for-deploying-windows-server-active-directory-on-azure-virtual-machines"></a>Richtlijnen voor de implementatie van Windows Server Active Directory op Azure virtuele machines

In dit artikel wordt uitgelegd dat de belangrijke verschillen tussen implementeren Windows Server Active Directory Domain Services (AD DS) en Active Directory Federation Services (AD FS) op-lokalen en deze implementeren op Microsoft Azure virtuele machines.

## <a name="scope-and-audience"></a>Toepassingsgebied en doelgroep

Dit artikel is bedoeld voor degenen die al ervaring heeft met het implementeren van Active Directory op gebouwen. Dit omvat de verschillen tussen het distribueren van Active Directory op Microsoft Azure virtuele machines/Azure virtuele netwerken en traditionele op ruimten Active Directory-implementaties. Azure virtuele machines en Azure virtuele netwerken uitmaken deel van een infrastructuur-as-a-Service (IaaS) aanbod voor organisaties gebruikmaken van bronnen in de cloud.

Zie de [Implementatiehandleiding voor AD DS](https://technet.microsoft.com/library/cc753963) of [de AD FS-implementatie plannen](https://technet.microsoft.com/library/dn151324.aspx) zo nodig voor degenen die niet bekend zijn met de implementatie van AD.

In dit artikel wordt ervan uitgegaan dat de lezer vertrouwd met de volgende concepten is:

- Windows Server AD DS-implementatie en beheer
- Implementatie en configuratie van DNS ter ondersteuning van een Windows Server AD DS-infrastructuur
- Windows Server AD FS-implementatie en beheer
- Implementeren, configureren en beheren van gebruikmakende partij toepassingen (websites en web services) die Windows Server AD FS-tokens kunnen verbruiken
- Algemene virtuele machine concepten, zoals het configureren van een virtuele machine virtuele schijven en virtuele netwerken

In dit artikel worden de vereisten beschreven voor een hybride implementatiescenario waarin Windows Server in AD DS of AD FS zijn gedeeltelijk geïmplementeerd op gebouwen en deels geïmplementeerd op Azure virtuele machines. De belangrijke verschillen tussen Azure virtuele machines versus lokale en belangrijke beslissingen die invloed hebben op het ontwerp en de implementatie van AD DS en AD FS van Windows Server waarop eerst heeft betrekking op het document. De rest van het papier wordt richtsnoeren voor elk van de punten van de beschikking in meer detail en het toepassen van de richtlijnen op verschillende implementatiescenario's beschreven.

In dit artikel wordt niet de configuratie van [Active Directory Azure](http://azure.microsoft.com/services/active-directory/), beschreven die is een REST-service identiteits- en functies van access control voor cloud-toepassingen biedt. Azure Active Directory (AD Azure) en Windows Server in AD DS zijn, echter ontworpen om samen te werken om een oplossing voor identiteits- en toegangsbeheer voor hedendaagse hybride IT-omgevingen en moderne toepassingen. Om te begrijpen van de verschillen en de relaties tussen Windows Server in AD DS en AD Azure, het volgende overwegen:

1. U kunt Windows Server AD DS in de cloud op Azure virtuele machines uitvoeren als u via de Azure uw datacenter op ruimten uitbreiden naar de cloud.
2. Azure AD kunt u uw gebruikers eenmalige aanmelding verlenen tot toepassingen in Software-as-a-Service (SaaS). Microsoft Office 365 gebruikt deze technologie, bijvoorbeeld, en van Azure of andere platforms cloud-toepassingen kunnen ook gebruiken.
3. Kunt u Azure AD (de Access Control Service) kunnen gebruikers zich aanmelden bij het gebruik van identiteiten van Facebook, Google, Microsoft en andere leveranciers van identiteit voor toepassingen die worden gehost in de cloud of op locatie.

Zie voor meer informatie over deze verschillen, [Azure identiteit](fundamentals-identity.md).

## <a name="related-resources"></a>Verwante bronnen

U kunt downloaden en uitvoeren van de [Evaluatie van gereedheid voor Azure Virtual Machine](https://www.microsoft.com/download/details.aspx?id=40898). De beoordeling zal automatisch uw omgeving op bedrijfsruimten inspecteren en een aangepast rapport genereren op basis van de richtlijnen in dit onderwerp voor het migreren van de omgeving op Azure gevonden.

We raden u ook eerst de zelfstudies, handleidingen en video's die betrekking hebben op de volgende onderwerpen:

- [Een virtueel netwerk alleen Cloud-configureren in de Portal voor Azure](../virtual-network/virtual-networks-create-vnet-arm-pportal.md)
- [Configureren van een VPN sites in de Portal voor Azure](../vpn-gateway/vpn-gateway-site-to-site-create.md)
- [Een nieuw Active Directory-forest installeren op een virtueel netwerk van Azure](active-directory-new-forest-virtual-machine.md)
- [Een replica Active Directory-domeincontroller installeren op Azure](../active-directory/active-directory-install-replica-active-directory-domain-controller.md)
- [Microsoft Azure IT Pro IaaS: Fundamentals (01) Virtual Machine](https://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/01)
- [Microsoft Azure IT Pro IaaS: (05) maken van virtuele netwerken en connectiviteit tussen ruimten](https://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/05)

## <a name="introduction"></a>Inleiding

De fundamentele eisen voor de implementatie van Windows Server Active Directory op Azure virtuele machines verschilt weinig van in voor bedrijfsruimten virtuele machines (en tot op zekere hoogte fysieke machines) implementeren. Bijvoorbeeld in het geval van Windows Server in AD DS, als de domeincontrollers (DC's) die u op Azure virtuele machines implementeert replica's in een bestaande zijn op-ruimten corporate/domein, en vervolgens de Azure-implementatie kan grotendeels op dezelfde manier worden behandeld als elke andere aanvullende Windows Server Active Directory-site kan worden beschouwd. Dat wil zeggen, moeten subnetten worden gedefinieerd in Windows Server in AD DS, een site die is gemaakt, de subnetten gekoppeld aan deze site en andere sites met behulp van de juiste site-koppelingen verbonden. Er zijn echter enkele verschillen die gemeenschappelijk zijn voor alle Azure implementaties en sommige die variëren afhankelijk van het specifieke scenario. Twee fundamentele verschillen worden hieronder nader belicht:

### <a name="azure-virtual-machines-may-need-connectivity-to-the-on-premises-corporate-network"></a>Azure virtuele machines mogelijk verbinding met het bedrijfsnetwerk op gebouwen.

Opnieuw verbinding maakt Azure virtuele machines met een bedrijfsnetwerk op ruimten vereist Azure virtueel netwerk, dat bestaat uit een site naar site of site-naar-punt virtuele particuliere netwerk (VPN) onderdeel naadloos verbinding Azure virtuele machines en machines gebouwen. Deze VPN-component kan ook domeinleden toegang te krijgen tot een Windows Server Active Directory-domein met domeincontrollers uitsluitend op Azure virtuele machines worden gehost op gebouwen inschakelen. Het is belangrijk te weten, dat als de VPN-verbinding mislukt, andere bewerkingen die afhankelijk van Windows Server Active Directory zijn en verificatie ook niet. Terwijl gebruikers mogelijk aanmelden met behulp van bestaande in de cache opgeslagen referenties, alle peer-to-peer of verificatie van client naar server waarvoor tickets nog moeten worden afgegeven of verouderd zijn geworden, niet.

Zie [Virtuele netwerken](http://azure.microsoft.com/documentation/services/virtual-network/) voor een demonstratie video en een lijst met stapsgewijze zelfstudies, met inbegrip van [VPN in Azure portal - sites configureren](../vpn-gateway/vpn-gateway-site-to-site-create.md).

> [AZURE.NOTE] U kunt ook Windows Server Active Directory implementeren op een Azure virtueel netwerk dat geen verbinding met een netwerk op gebouwen. De richtlijnen in dit onderwerp, wordt er echter van uitgegaan dat een Azure virtueel netwerk wordt gebruikt omdat hiermee de IP-adressen van functies die essentieel voor Windows Server zijn.

### <a name="static-ip-addresses-must-be-configured-with-azure-powershell"></a>Statische IP-adressen moeten worden geconfigureerd met Azure PowerShell.

Dynamische adressen worden standaard toegewezen, maar de cmdlet Set-AzureStaticVNetIP gebruiken voor het toewijzen van een statisch IP-adres in plaats daarvan. Die wordt ingesteld met een statisch IP-adres dat door middel van service retoucheren en VM uitschakelen/opnieuw opstarten blijft bestaan. Zie [statische interne IP-adres voor virtuele machines](http://azure.microsoft.com/blog/static-internal-ip-address-for-virtual-machines/)voor meer informatie.

## <a name="BKMK_Glossary"></a>Termen en definities

Het volgende is een niet-limitatieve lijst van termen voor de diverse Azure technologieën die in dit artikel wordt verwezen.

- **Azure virtuele machines**: de IaaS biedt in Azure, waarmee klanten kunnen gebruiken met vrijwel elke traditioneel VMs op-premises serverwerklast.

- **Virtueel netwerk Azure**: de Netwerkservice in Azure waarmee klanten maken en beheren van virtuele netwerken in Azure en ze veilig te koppelen aan hun eigen op-premises netwerkinfrastructuur met behulp van een virtueel particulier netwerk (VPN).

- **Virtuele IP-adres**: een internetgerichte IP-adres dat niet is gebonden aan een specifieke computer of netwerk-interfacekaart. Cloud-services zijn een virtueel IP-adres voor het ontvangen van netwerkverkeer dat wordt omgeleid naar een Azure VM toegewezen. Een virtueel IP-adres is een eigenschap van een cloud-service die één of meer Azure virtuele machines kan bevatten. Vergeet niet dat een Azure virtueel netwerk een of meer cloud-services kan bevatten. Virtuele IP-adressen verstrekken native load balancing-mogelijkheden.

- **Dynamisch IP-adres**: dit is het IP-adres is intern. Deze moet worden geconfigureerd als een statisch IP-adres (via de cmdlet Set-AzureStaticVNetIP) voor VMs die host zijn van de domeincontroller en DNS-serverfuncties.

- **Service Retoucheerpenseel**: het proces waarbij Azure automatisch terug een service naar actief opnieuw nadat wordt gedetecteerd dat de service is mislukt. Retoucheren-service is een van de aspecten van Azure die beschikbaarheid en betere beveiliging ondersteunt. Hoewel onwaarschijnlijk, is het resultaat na een incident Retoucheerpenseel voor een domeincontroller waarop een VM service is vergelijkbaar met een niet-gepland opnieuw opstarten, maar heeft enkele neveneffecten:

 - De virtuele netwerkadapter in de VM wordt gewijzigd.
 - Het MAC-adres van de virtuele netwerkadapter wordt gewijzigd
 - De Processor/CPU-ID van de VM wordt gewijzigd.
 - De IP-configuratie van de virtuele netwerkadapter wordt niet gewijzigd als de VM is gekoppeld aan een virtueel netwerk en van de VM-IP-adres statisch is.

 Geen van deze gedragingen van invloed op Windows Server Active Directory omdat er geen afhankelijkheid van het MAC-adres of de ID van de Processor/CPU en alle Windows Server Active Directory-implementaties op Azure worden aanbevolen om te worden uitgevoerd op een virtueel netwerk van Azure als hierboven beschreven.

## <a name="is-it-safe-to-virtualize-windows-server-active-directory-domain-controllers"></a>Is het veilig aan Windows Server Active Directory-domeincontrollers virtualiseren?

Windows Server Active Directory-domeincontrollers distribueren op Azure virtuele machines is onderworpen aan dezelfde richtlijnen op ruimten DCs uitgevoerd op een virtuele machine. Uitvoeren van gevirtualiseerde DCs is een veilige praktijken als richtsnoeren voor back-ups maken en terugzetten van DCs worden gevolgd. Zie voor meer informatie over beperkingen en richtlijnen voor het uitvoeren van gevirtualiseerde DCs [Domeincontrollers in Hyper-V wordt uitgevoerd](https://technet.microsoft.com/library/dd363553).

Hypervisors bieden of trivialize technologieën die problemen voor veel gedistribueerde systemen veroorzaken kunnen, inclusief Windows Server Active Directory. Bijvoorbeeld op een fysieke server, kunt u een schijf klonen of niet-ondersteunde methoden gebruiken om het terugdraaien van de status van een server, inclusief het gebruik van SAN's enzovoort, maar doet dat op een fysieke server is veel moeilijker dan het herstellen van een momentopname van een virtuele machine in een hypervisor. Azure biedt functionaliteit die leiden ongewenste dezelfde voorwaarde tot kan. Zo moet u VHD van DCs-bestanden niet kopiëren in plaats van het uitvoeren van regelmatige back-ups omdat deze terug te zetten in een situatie vergelijkbaar met het gebruik van voorzieningen voor het terugzetten van snapshot kan opleveren.

Dergelijke terugdraaiversies introduceren USN-Bolletjes die tot definitief divergerende lidstaten tussen domeincontrollers leiden kunnen. Dat kan problemen veroorzaken, zoals:

- Achtergebleven objecten
- Inconsistente wachtwoorden
- Inconsistente kenmerkwaarden
- Schema komt niet overeen als de schema-master wordt hersteld

Zie voor meer informatie over hoe DCs ondervindt [USN- en USN-hersteloptie](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv.aspx#usn_and_usn_rollback).

Begin met Windows Server 2012 [Extra garanties zijn ingebouwd in AD DS](https://technet.microsoft.com/library/hh831734.aspx). Deze beveiligingen beschermen gevirtualiseerde domeincontrollers ten opzichte van de bovengenoemde problemen zolang het onderliggende platform voor hypervisor VM-GenerationID ondersteunt. Azure ondersteunt VM-GenerationID, wat betekent dat domeincontrollers met Windows Server 2012 of later op Azure virtuele machines de aanvullende beveiligingsfuncties.

> [AZURE.NOTE] U moet afsluiten en opnieuw opstarten van een VM die de functie van domeincontroller in Azure binnen het gastbesturingssysteem in plaats van de optie **Afsluiten** in de klassieke Azure portal wordt uitgevoerd. Vandaag, met de klassieke portal voor het afsluiten van een VM zorgt ervoor dat de VM te worden opgeheven. Een VM deallocated heeft het voordeel van de toeslagen niet aangaan, maar dan ook herstelt de VM-GenerationID die ongewenste voor een domeincontroller is. Wanneer de VM-GenerationID opnieuw is ingesteld, wordt ook de invocationID van de AD DS-database teruggezet, de RID-groep wordt verwijderd en SYSVOL is gemarkeerd als niet-gemachtigde. Zie [Inleiding tot Active Directory Domain Services (AD DS), virtualisatie](https://technet.microsoft.com/library/hh831734.aspx) en [DFSR veilig virtualisatie](http://blogs.technet.com/b/filecab/archive/2013/04/05/safely-virtualizing-dfsr.aspx)voor meer informatie.

## <a name="why-deploy-windows-server-ad-ds-on-azure-virtual-machines"></a>Waarom Windows Server AD DS Azure virtuele Machines implementeren?

Veel Windows Server AD DS deployment-scenario's zijn zeer geschikt voor implementatie VMs op Azure. Stel dat u een bedrijf in Europa die nodig zijn voor de verificatie van gebruikers op een externe locatie in Azië. Het bedrijf heeft Windows Server Active Directory-domeincontrollers in Azië niet eerder hebt geïmplementeerd als gevolg van de kosten voor de implementatie van deze en beperkte expertise voor het beheren van de servers na de implementatie. Als gevolg hiervan worden verificatieaanvragen vanuit Azië bediend door DCs in Europa met niet-optimale resultaten. In dit geval kunt u een domeincontroller op een VM die u hebt opgegeven moet worden uitgevoerd binnen het computercentrum Azure in Azië implementeren. De DC die verbonden zijn aan een Azure virtueel netwerk die rechtstreeks is aangesloten op de externe locatie verbeteren verificatie prestaties.

Azure is ook zeer geschikt als vervanging voor anders dure disaster recovery (DR) sites. De relatief lage kosten van het hosten van een klein aantal domeincontrollers en één virtuele netwerk op Azure vertegenwoordigt een aantrekkelijk alternatief.

U wilt tenslotte op Azure zoals SharePoint, Windows Server Active Directory is vereist, maar heeft geen afhankelijkheid van het netwerk van lokalen of corporate Windows Server Active Directory-netwerk toepassingen distribueren. In dit geval een geïsoleerd forest op Azure om te voldoen aan de SharePoint implementatie eisen van de server is optimaal. Nogmaals, deploying network '-toepassingen waarvoor de verbinding met het netwerk op gebouwen en corporate Active Directory wordt ook ondersteund.

> [AZURE.NOTE] Aangezien het een layer 3-verbinding, kan de VPN-onderdelen die connectiviteit tussen een Azure virtual network en een netwerk van lokalen biedt lidservers die worden uitgevoerd op gebouwen om te profiteren van DC's die worden uitgevoerd als Azure virtuele machines op Azure virtueel netwerk ook inschakelen. Maar als de VPN-verbinding niet beschikbaar is, de communicatie tussen-lokale computers en domeincontrollers op basis van Azure zal niet werken, aanleiding geeft tot verificatie en verschillende andere fouten.  

## <a name="contrasts-between-deploying-windows-server-active-directory-domain-controllers-on-azure-virtual-machines-versus-on-premises"></a>Dus iets anders dan tussen de implementatie van Windows Server Active Directory-domeincontrollers Azure virtuele Machines ten opzichte van gebouwen

- Voor elk implementatiescenario Windows Server Active Directory met meer dan een enkele VM, is het noodzakelijk een Azure virtueel netwerk gebruiken voor de consistentie van IP-adres. Houd er rekening mee dat in deze handleiding wordt ervan uitgegaan dat DCs op een virtueel netwerk van Azure.

- Zoals bij DCs voor gebouwen, worden statische IP-adressen aanbevolen. Een statisch IP-adres kan alleen worden geconfigureerd met behulp van Azure PowerShell. Zie [statische interne IP-adres voor VMs](http://azure.microsoft.com/blog/static-internal-ip-address-for-virtual-machines/) voor meer informatie. Hebt u controle systemen of andere oplossingen voor statische IP-adresconfiguratie in het gastbesturingssysteem controleren, kunt u hetzelfde statische IP-adres toewijzen aan de eigenschappen van de netwerk-adapter van de VM. Maar houd er rekening mee dat de netwerkadapter worden verwijderd als de VM service retoucheren ondergaat of in de klassieke portal is afgesloten en is het adres opgeheven. In dat geval wordt moet het statische IP-adres binnen de Gast worden ingesteld.

- VMs implementeren op een virtueel netwerk niet impliceren (of moeten) connectiviteit met een netwerk van gebouwen; het virtuele netwerk kan alleen deze mogelijkheid. U moet een virtueel netwerk voor persoonlijke communicatie tussen Azure en uw netwerk op gebouwen. U moet voor de implementatie van een VPN-eindpunt in het netwerk op gebouwen. De VPN-verbinding wordt geopend vanuit Azure met het netwerk op gebouwen. Zie [Virtuele netwerk-overzicht](../virtual-network/virtual-networks-overview.md) en [een VPN sites in de Portal Azure configureren](../vpn-gateway/vpn-gateway-site-to-site-create.md)voor meer informatie.

> [AZURE.NOTE] Een optie voor het [maken van een VPN punt-naar-site](../vpn-gateway/vpn-gateway-point-to-site-create.md) is beschikbaar voor afzonderlijke Windows-gebaseerde computers rechtstreeks aansluiten op een virtueel netwerk van Azure.

- Ongeacht of u een virtuele maakt netwerken of niet, Azure toeslagen voor egress-verkeer, maar niet binnendringen. Verschillende opties voor Windows Server Active Directory-ontwerp kunnen invloed hebben op hoeveel egress-netwerkverkeer wordt gegenereerd door een implementatie. Bijvoorbeeld beperkt voor het implementeren van een alleen-lezen domeincontroller (RODC) egress verkeer omdat deze geen uitgaande repliceert. Maar het besluit tot het implementeren van een RODC moet worden afgewogen tegen de noodzaak om uit te voeren tegen de DC en de [compatibiliteit](https://technet.microsoft.com/library/cc755190) met toepassingen en services op de site met de RODC schrijven-bewerkingen. Zie [Azure prijzen bij-overzicht](http://azure.microsoft.com/pricing/)voor meer informatie over kosten voor verkeer.

- Terwijl er volledige bepalen via welke server-bronnen voor VMs op-ruimten, zoals RAM-geheugen, schijf grootte en enzovoort, op Azure moet u selecteren uit een lijst met vooraf geconfigureerde server-formaten. Een schijf met gegevens is voor een domeincontroller, naast de schijf nodig om de Windows Server Active Directory-database opslaan.

## <a name="can-you-deploy-windows-server-ad-fs-on-azure-virtual-machines"></a>Kunt u Windows Server AD FS op Azure virtuele machines implementeren?

Ja, u kunt implementeren Windows Server AD FS op Azure virtuele machines en de [beste praktijken voor de implementatie van AD FS](https://technet.microsoft.com/library/dn151324.aspx) op ruimten gelden voor zowel AD FS-implementatie op Azure. Maar sommige van de beste praktijken, zoals taakverdeling en hoge beschikbaarheid vereisen technologieën dan wat AD FS biedt zelf. Zij moeten door de onderliggende infrastructuur worden geleverd. Laten we enkele van de beste praktijken en Zie hoe ze kunnen worden bereikt met behulp van Azure VMs en een Azure virtueel netwerk.

1. **Security token service (STS) servers rechtstreeks met het Internet nooit worden blootgesteld.**

    Dit is belangrijk omdat de STS-beveiligingstokens. Als gevolg hiervan moeten STS-servers zoals AD FS-servers worden behandeld met hetzelfde niveau van bescherming biedt als een domeincontroller. Als u een STS gevaar loopt, kunnen kwaadwillende gebruikers de toegangstokens mogelijk met de vorderingen van hun keuze gebruikmakende partij toepassingen en andere servers STS in organisaties vertrouwen geven.

2. **Active Directory-domeincontrollers voor alle gebruikersdomeinen in hetzelfde netwerk als de AD FS-servers implementeren.**

    Active Directory Domain Services AD FS-servers gebruiken om gebruikers te verifiëren. Het is raadzaam om domeincontrollers op hetzelfde netwerk als de AD FS-servers implementeren. Dit zorgt voor bedrijfscontinuïteit voor het geval de koppeling tussen het Azure netwerk en uw netwerk op ruimten verbroken wordt en kan lagere latentie en hogere prestaties voor aanmeldingen.

3. **Implementeren op meerdere knooppunten voor AD FS voor hoge beschikbaarheid en de load balancing.**

    In de meeste gevallen is het mislukken van een toepassing waarmee AD FS onaanvaardbaar omdat bedrijfskritieke toepassingen waarvoor beveiligingstokens zijn vaak. Als gevolg hiervan en omdat AD FS bevindt zich nu in het kritieke pad voor toegang tot bedrijfskritieke toepassingen, de service van AD FS moet maximaal beschikbaar is via verschillende proxy's voor AD FS en de AD FS-servers. Voor de verdeling van aanvragen, Netwerktaakverdeling meestal worden ingezet voor zowel de AD FS-proxy en de AD FS-servers.

4. **Een of meer knooppunten Web Application Proxy voor internet-toegang implementeren.**

    Als gebruikers moeten toegang tot toepassingen die worden beveiligd door het AD FS-service, moet de AD FS-service beschikbaar zijn via internet. Dit wordt bereikt door het implementeren van het Web Application Proxy-service. Het is raadzaam om meer dan één knooppunt ten behoeve van hoge beschikbaarheid implementeren en taakverdeling.

5. **Toegang vanaf het Web Application Proxy knooppunten beperken tot interne netwerkbronnen.**

    Zodat externe gebruikers toegang krijgen tot AD FS in het internet moet u een Web Application Proxy knooppunten (of AD FS-Proxy in eerdere versies van Windows Server) implementeren. De webtoepassing proxy knooppunten worden rechtstreeks blootgesteld aan Internet. Ze zijn niet verplicht te zijn van een domein behoren en moeten zij alleen toegang tot het AD FS-servers via TCP-poort 443 en 80. Het is raadzaam dat communicatie met alle andere computers (met name voor domeincontrollers) is geblokkeerd.

    Dit is meestal bereikte op-ruimten met behulp van een DMZ. Firewalls gebruikt een "witte" lijst bewerking te voorkomen dat verkeer de DMZ met het netwerk op gebouwen (dat wil zeggen, alleen verkeer van het opgegeven IP-adressen en poorten opgegeven wordt toegestaan en alle andere verkeer wordt geblokkeerd).

Het volgende diagram ziet u een traditionele op-premises AD FS-implementatie.

![Diagram van een traditionele op ruimten Active Directory Federation Services-implementatie](media/active-directory-deploying-ws-ad-guidelines/ADFS_onprem.png)

Echter omdat Azure geen eigen biedt, complete firewall-mogelijkheden, andere opties moeten worden gebruikt voor het beperken van verkeer. In de volgende tabel ziet u de opties en voordelen en nadelen.

| Optie | Voordeel | Nadeel |
| ------ | --------- | ------------ |
| [Azure netwerk ACL 's](virtual-networks-acl.md) | Minder prijzig en eenvoudigere configuratie | ACL extra configuratie vereist als alle nieuwe VMs worden toegevoegd aan de implementatie |
| [Barracuda NG firewall](https://www.barracuda.com/products/ngfirewall) | In de modus "witte" lijst van de bewerking en het is geen ACL netwerkconfiguratie | Hogere kosten en complexiteit voor eerste installatie |

De stappen op hoog niveau voor de implementatie van AD FS zijn in dit geval:

1. Een virtueel netwerk maken met meerdere gebouwen connectiviteit, met behulp van een VPN- of [ExpressRoute](http://azure.microsoft.com/services/expressroute/).

2. Domeincontrollers in het virtuele netwerk distribueren. Deze stap is optioneel maar aanbevolen.

3. Implementeren van AD FS-servers in het virtuele netwerk domein behoren.

4. Maak een [interne Netwerktaakverdeling instellen](http://azure.microsoft.com/blog/internal-load-balancing/) waarin de AD FS-servers en een nieuwe particuliere IP-adres binnen het virtuele netwerk (een dynamisch IP-adres) wordt gebruikt.

  1. Bijwerken van DNS om te maken van de FQDN-naam om te verwijzen naar de persoonlijke (dynamische) IP-adres het interne taakverdeling instellen.

5. Een cloud-service (of een afzonderlijke virtuele netwerk) maken voor het Web Application Proxy-knooppunten.

6. Web Application Proxy knooppunten in het virtuele netwerk of cloud service implementeren

  1. Maak een externe taakverdeling die de Web Application Proxy knooppunten bevat.

  2. De externe DNS-naam (Fully Qualified Domain Name) om te verwijzen naar de cloud service openbare IP-adres (het virtuele IP-adres) worden bijgewerkt.

  3. AD FS-proxy's voor het gebruik van de FQDN-naam die overeenkomt met de interne taakverdeling is ingesteld voor de AD FS-servers configureren.

  4. Bijwerken van websites voor het gebruik van de externe FQDN-naam voor de voorziening van hun vorderingen op basis van claims.

7. Beperk de toegang tussen Web Application Proxy naar een willekeurige computer in het virtuele netwerk van AD FS.

Om het verkeer te beperken, de taakverdeling is ingesteld voor de Azure interne taakverdeling moet worden geconfigureerd voor alleen verkeer op TCP-poort 80 en 443 en al het andere verkeer naar de interne dynamisch IP-adres van de set taakverdeling is weggehaald.

![Diagram van ACL's ADFS-netwerk met 443 TCP + 80 is toegestaan.](media/active-directory-deploying-ws-ad-guidelines/ADFS_ACLs.png)

Verkeer naar de AD FS-servers zou mag alleen door de volgende bronnen:

- De Azure interne taakverdeling.
- Het IP-adres van een beheerder van het netwerk op gebouwen.

> [AZURE.WARNING] Het ontwerp moet voorkomen dat Web Application Proxy knooppunten eventuele andere VMs in de Azure virtueel netwerk of locaties op het netwerk op ruimten te bereiken. Dat kan worden gedaan door firewallregels configureren in het toestel in de lokalen voor Route Express-verbindingen of het VPN-apparaat voor VPN-verbindingen van site naar site.

Een nadeel van deze optie is het nodig om het netwerk ACL's voor meerdere apparaten, met inbegrip van interne taakverdeling, de AD FS-servers en andere servers die worden toegevoegd aan het virtuele netwerk. Als een apparaat wordt toegevoegd aan de implementatie zonder netwerk ACL's om het verkeer te beperken, kan de volledige implementatie mogelijk risico. Als de IP-adressen van de knooppunten Web Application Proxy verandert, het netwerk van ACL's moet opnieuw worden ingesteld (wat betekent dat de proxy moeten worden geconfigureerd voor [statische dynamische IP-](http://azure.microsoft.com/blog/static-internal-ip-address-for-virtual-machines/)adressen).

![AD FS op Azure met ACL's netwerk.](media/active-directory-deploying-ws-ad-guidelines/ADFS_Azure.png)

Een andere mogelijkheid is het gebruik van het toestel [Barracuda NG Firewall](https://www.barracuda.com/products/ngfirewall) om verkeer tussen proxyservers AD FS en de AD FS-servers. Deze optie voldoet aan de aanbevolen procedures voor beveiliging en hoge beschikbaarheid en minder administratie bij de oorspronkelijke installatie is vereist omdat het toestel Barracuda NG Firewall een modus "witte" lijst van beheer van de firewall biedt en rechtstreeks op een Azure virtueel netwerk kan worden geïnstalleerd. Dat het niet nodig elk gewenst moment die een nieuwe server wordt toegevoegd aan de implementatie van netwerk ACL's configureren. Maar deze optie verhoogt de aanvankelijke implementatiecomplexiteit en kosten.

In dit geval worden twee virtuele netwerken geïmplementeerd in plaats van één. Wij bellen u ze VNet1 en VNet2. VNet1 bevat de proxy's en VNet2 de STSs en de netwerkverbinding weer met het bedrijfsnetwerk. VNet1 is dus fysiek (zij vrijwel) geïsoleerd uit VNet2 en op zijn beurt met het bedrijfsnetwerk. VNet1 aan VNet2 is verbonden met een speciale tunneling-technologie bekend als Transport onafhankelijke netwerk architectuur (TINA). De TINA-tunnel is gekoppeld aan elk van de virtuele netwerken met behulp van een Barracuda NG firewall, een Barracuda op elk van de virtuele netwerken.  Voor hoge beschikbaarheid verdient het implementeren van twee barracuda's op elk virtueel netwerk; een actief, de andere passieve. Zij bieden een zeer uitgebreide mogelijkheden gebruik waardoor ons om na te bootsen de werking van een traditionele op ruimten DMZ in Azure.

![AD FS op Azure met firewall.](media/active-directory-deploying-ws-ad-guidelines/ADFS_Azure_firewall.png)

Zie voor meer informatie [AD FS: uitbreiden van een front-end-toepassing claimbewuste in ruimten met Internet](#BKMK_CloudOnlyFed).

### <a name="an-alternative-to-ad-fs-deployment-if-the-goal-is-office-365-sso-alone"></a>Een alternatief voor de implementatie van AD FS als de doelstelling Office 365 eenmalige aanmelding alleen is

Er is een ander alternatief voor het implementeren van AD FS, helemaal als u alleen voor het aanmelden voor Office 365 inschakelen. In dat geval kunt u gewoon DirSync implementeren met wachtwoord synchronisatie op de ruimten en bereiken hetzelfde eindresultaat met minimale implementatiecomplexiteit omdat deze aanpak geen AD FS of Azure vereist.

De volgende tabel ziet hoe de processen-in werken met en zonder het implementeren van AD FS.

| Office 365 single sign-on met AD FS en DirSync | Office 365 dezelfde aanmelding met DirSync + Password Sync |
| ------------- | ------------- |
| 1. de gebruiker zich aanmeldt met een bedrijfsnetwerk en voor Windows Server Active Directory is geverifieerd. | 1. de gebruiker zich aanmeldt met een bedrijfsnetwerk en voor Windows Server Active Directory is geverifieerd. |
| 2. de gebruiker probeert toegang te krijgen tot Office 365 (ik ben @contoso.com). | 2. de gebruiker probeert toegang te krijgen tot Office 365 (ik ben @contoso.com). |
| 3. office 365 wordt de gebruiker omgeleid naar Azure AD. | 3. office 365 wordt de gebruiker omgeleid naar Azure AD. |
| 4. Aangezien Azure AD de gebruiker niet verifiëren en er is een vertrouwensrelatie met AD FS op ruimten begrijpt, wordt de gebruiker omgeleid naar AD FS. | 4. azure AD rechtstreeks Kerberos-tickets niet accepteren en geen vertrouwensrelatie bestaat dus het verzoek dat de gebruiker referenties invoeren. |
| 5. de gebruiker verzendt een Kerberos-ticket naar de AD FS-STS. | 5. de gebruiker typt het wachtwoord op gebouwen en Azure AD ze tegen de gebruikersnaam en het wachtwoord die is gesynchroniseerd met DirSync valideert. |
| 6. AD FS transformeert het Kerberos-ticket als de vereiste token indeling/vorderingen en wordt de gebruiker omgeleid naar Azure AD. | 6. azure AD wordt de gebruiker omgeleid naar Office 365. |
| 7. de gebruiker geverifieerd bij AD Azure (een andere transformatie plaatsvindt). |  7. de gebruiker zich aanmelden bij Office 365 en Azure AD token gebruikt de OWA. |
| 8. azure AD wordt de gebruiker omgeleid naar Office 365. |  |
| 9. de gebruiker is zonder waarschuwing aangemeld bij Office 365. |  |

In de Office 365 met DirSync met wachtwoord synchronisatie scenario (geen AD FS), wordt eenmalige aanmelding vervangen door "dezelfde aanmelding' waar 'same' betekent dat gebruikers opnieuw hun dezelfde on-premises referenties invoeren moeten bij het openen van Office 365. Houd er rekening mee dat deze gegevens door de browser van de gebruiker om te helpen verminderen de volgende vragen kan worden onthouden.

### <a name="additional-food-for-thought"></a>Extra voedingsmiddelen voor gedachte

- Als u een AD FS-proxy op een Azure VM implementeert, is de verbinding met het AD FS-servers nodig. Als ze op ruimten zijn, is het raadzaam gebruikmaken van de site naar site VPN-verbindingen die door het virtuele netwerk kunnen de knooppunten Web Application Proxy te communiceren met de AD FS-servers.

- Als u een AD FS-server op een Azure virtuele machine, connectiviteit voor Windows Server Active Directory-domeincontrollers, kenmerk winkels en configuratiedatabases nodig is en wellicht ook een ExpressRoute of een VPN-verbinding van site naar site tussen de Azure virtueel netwerk en het netwerk op gebouwen.

- Kosten worden toegepast op alle verkeer van Azure virtual machines (egress-verkeer). Als kosten de drijvende factor is, is het raadzaam voor de implementatie van de knooppunten Web Application Proxy op Azure, zodat de AD FS-servers in-bedrijfsruimte. Als de AD FS-servers worden geïmplementeerd op Azure virtuele machines alsmede, worden extra kosten gemaakt voor het verifiëren van gebruikers op gebouwen. Verkeer egress maakt kosten ongeacht al dan niet via de ExpressRoute of de site naar site VPN-verbinding.

- Als u van Azure native taakverdeling van servers mogelijkheden voor hoge beschikbaarheid van AD FS-servers gebruikt, houd er rekening mee dat load balancing sondes die worden gebruikt voor het bepalen van de gezondheid van de virtuele machines in de cloud-service biedt. In het geval van Azure virtual machines (in plaats van rollen voor web of werknemer), worden een aangepaste sonde gebruikt omdat de agent die op de sondes standaard reageert niet aanwezig op Azure virtuele machines is. Voor eenvoud, kunt u een aangepaste sonde met TCP, hiervoor alleen een TCP-verbinding (een segment TCP SYN verzonden en waarop is gereageerd met een TCP SYN-ACK-segment) worden gebracht om te bepalen van de gezondheid van de virtuele machine. U kunt de aangepaste sonde voor het gebruik van de TCP-poort waarop de virtuele machines actief luisteren.

> [AZURE.NOTE] Machines die dezelfde reeks poorten rechtstreeks met het Internet (bijvoorbeeld poort 80 en 443) moeten delen niet dezelfde cloud service. Daarom is aanbevolen dat u een speciaal wolk maakt service voor uw Windows Server AD FS-servers om te voorkomen dat potentiële overlapping tussen de Poortvereisten voor een toepassing- en Windows Server AD FS.

## <a name="deployment-scenarios"></a>Implementatiescenario 's

De volgende sectie bevat alledaagse implementatiescenario's om de aandacht te vestigen op belangrijke overwegingen die moeten worden genomen in aanmerking. Elk scenario bevat koppelingen naar meer informatie over de besluiten en de factoren te overwegen.

1. [AD DS: Een AD DS-aware toepassing met geen vereiste voor een bedrijfsnetwerk verbinding implementeren](#BKMK_CloudOnly)

    Bijvoorbeeld wordt een internetgerichte SharePoint-service geïmplementeerd op een Azure virtuele machine. De toepassing heeft geen afhankelijkheden op het zakelijke netwerk. De toepassing Windows Server in AD DS is vereist, maar de zakelijke Windows Server AD DS is niet vereist.

2. [AD FS: Een front-end-toepassing claimbewuste in ruimten met Internet uitbreiden](#BKMK_CloudOnlyFed)

    Een claimbewuste toepassing die is met succes geïmplementeerd op gebouwen en gebruikt door gebruikers binnen het bedrijf moet toegankelijk zijn vanaf Internet komen. De toepassing moet rechtstreeks via Internet toegankelijk zowel door zakenpartners via hun eigen zakelijke identiteit en bestaande zakelijke gebruikers.

3. [AD DS: Windows Server AD DS-aware toepassingen die verbinding met het bedrijfsnetwerk, moet distribueren](#BKMK_HybridExt)

    Bijvoorbeeld is een LDAP-toepassing die Windows Server in AD DS als opslagplaats gebruikt voor de configuratie en het profiel van de gebruiker en ondersteunt geïntegreerde Windows-verificatie geïmplementeerd op een Azure virtuele machine. Het is wenselijk voor de toepassing om te profiteren van de bestaande corporate Windows Server in AD DS en eenmalige aanmelding. De toepassing is geen claimbewuste.

### <a name="BKMK_CloudOnly"></a>1. AD DS: Een AD DS-aware toepassing met geen vereiste voor een bedrijfsnetwerk verbinding implementeren

![Wolk alleen-lezen AD DS-implementatie](media/active-directory-deploying-ws-ad-guidelines/ADDS_cloud.png)
**figuur 1**

#### <a name="description"></a>Beschrijving

SharePoint is geïmplementeerd op een Azure virtuele machine en de toepassing heeft geen afhankelijkheden op het zakelijke netwerk. De toepassing hoeft u Windows Server in AD DS, maar *niet* de corporate Windows Server in AD DS is vereist. Geen Kerberos of Federatieve vertrouwensrelaties zijn vereist omdat gebruikers zelf worden ingericht via de toepassing in het Windows Server AD DS-domein waarvoor ook in de cloud op Azure virtuele machines.

#### <a name="scenario-considerations-and-how-technology-areas-apply-to-the-scenario"></a>Overwegingen bij het scenario en hoe technologische gebieden van toepassing op het scenario

- [Netwerktopologie](#BKMK_NetworkTopology): een Azure virtueel netwerk zonder verbinding tussen ruimten (ook wel bekend als de site naar site connectivity) maken.

- [Implementatieconfiguratie DC](#BKMK_DeploymentConfig): een nieuwe domeincontroller implementeren in een nieuw, één domein Windows Server Active Directory-forest. Dit moet worden gecombineerd met de Windows DNS-server.

- [Windows Server Active Directory-site-topologie](#BKMK_ADSiteTopology): de standaard Windows Server Active Directory-site (alle computers worden in de standaard-eerste-Site-Name) gebruiken.

- [IP-adressen en DNS](#BKMK_IPAddressDNS):

 - Een statisch IP-adres instellen voor de DC via de cmdlet Set-AzureStaticVNetIP Azure PowerShell.
 - Installeren en configureren van Windows Server DNS op de domeincontroller (s) op Azure.
 - De netwerkeigenschappen van het virtuele configureren met de naam en het IP-adres van de VM die fungeert als host voor de domeincontroller en DNS-serverfuncties.

- [Globale catalogus](#BKMK_GC): de eerste domeincontroller in het forest moet een globale-catalogusserver. Extra domeincontrollers moeten ook worden geconfigureerd als GC omdat in een forest met één domein de globale catalogus niet geen extra werk bij de domeincontroller vereist.

- [Plaatsing van de Windows Server AD DS-database en SYSVOL](#BKMK_PlaceDB): een gegevensschijf toevoegen op domeincontrollers uitgevoerd als Azure VMs om het opslaan van de Windows Server Active Directory-database, logboekbestanden en SYSVOL.

- [Back-up en terugzetten](#BKMK_BUR): bepalen waar u wilt opslaan, back-ups van de staat. Zo nodig toevoegen een andere schijf met gegevens voor de DC-VM back-ups opslaat.

### <a name="BKMK_CloudOnlyFed"></a>2 AD FS: een front-end-toepassing claimbewuste in ruimten met Internet uitbreiden

![Federatie met connectiviteit tussen gebouwen](media/active-directory-deploying-ws-ad-guidelines/Federation_xprem.png)
**in figuur 2**

#### <a name="description"></a>Beschrijving

Een claimbewuste toepassing die is met succes geïmplementeerd op gebouwen en gebruikt door gebruikers binnen het bedrijf moet zijn direct toegankelijk vanaf het Internet. De toepassing fungeert als een web front-end voor een SQL-database waarin gegevens worden opgeslagen. De SQL-servers die worden gebruikt door de toepassing bevinden zich eveneens op het bedrijfsnetwerk. Twee Windows Server AD FS STSs en een taakverdeling zijn geïmplementeerd op-ruimten voor toegang tot de zakelijke gebruikers. Nu de toepassing moet worden ook rechtstreeks via Internet toegankelijk zowel door zakenpartners via hun eigen zakelijke identiteit als zakelijke gebruikers bestaande.

In een poging te vereenvoudigen en te voldoen aan de behoeften van de implementatie en configuratie van deze nieuwe vereiste, wordt besloten dat twee extra frontends en twee web-proxyservers van Windows Server AD FS op Azure virtuele machines worden geïnstalleerd. Alle vier VMs zullen worden blootgesteld rechtstreeks met het Internet en de connectiviteit met de op gebouwen-netwerk met behulp van VPN-functie van Azure Virtual Network-website worden geleverd.

#### <a name="scenario-considerations-and-how-technology-areas-apply-to-the-scenario"></a>Overwegingen bij het scenario en hoe technologische gebieden van toepassing op het scenario

- [Netwerktopologie](#BKMK_NetworkTopology): Maak een Azure virtueel netwerk en [connectiviteit tussen lokalen configureren](../vpn-gateway/vpn-gateway-site-to-site-create.md).

 > [AZURE.NOTE] Zorg ervoor dat de URL die is gedefinieerd in de sjabloon en de resulterende certificaten kan worden bereikt door de Windows Server AD FS instanties op Azure voor elk van de Windows Server AD FS-certificaten. Mogelijk dient u meerdere ruimten connectiviteit met onderdelen van de PKI-infrastructuur. Voor het voorbeeld als eindpunt van de CRL gebaseerd op LDAP is en uitsluitend op-lokalen gehost, vervolgens cross-premises connectivity vereist zal zijn. Als dit niet wenselijk is, kan het zijn nodig om certificaten uitgegeven door een Certificeringsinstantie waarvan de CRL toegankelijk via het Internet is te gebruiken.

- [Configuratie van cloud services](#BKMK_CloudSvcConfig): Controleer of er twee cloud services in volgorde twee verdeeld virtuele IP-adressen. Het virtuele IP-adres van de eerste cloud-dienst wordt doorgestuurd naar de twee Windows Server AD FS proxy VMs op poorten 80 en 443. De Windows Server AD FS proxy VMs wordt geconfigureerd om te verwijzen naar het IP-adres van de taakverdeling in ruimten die voorkanten de STSs Windows Server AD FS. Het virtuele IP-adres van de tweede cloud-dienst wordt doorgestuurd naar de twee VMs de frontend web opnieuw uitgevoerd op poort 80 en 443. Een aangepaste sonde om ervoor te zorgen dat de taakverdelingsvoorziening stuurt alleen verkeer naar een werkende Windows Server AD FS-proxy en web frontend VMs configureren.

- [Federation-serverconfiguratie](#BKMK_FedSrvConfig): Windows Server configureren AD FS federatieserver (STS) voor het genereren van beveiligingstokens voor het Windows Server Active Directory-forest dat is gemaakt in de cloud. Federatie claims provider vertrouwensrelaties met de verschillende partners die u wilt accepteren identiteiten uit, en configureren gebruikmakende partij vertrouwensrelaties met de andere toepassingen om tokens te genereren.

    Windows Server AD FS-proxyservers worden geïmplementeerd in een internetgerichte capaciteit uit veiligheidsoverwegingen in de meeste gevallen terwijl hun tegenhangers Windows Server AD FS-federatie geïsoleerd van rechtstreekse Internet-verbinding blijven. Ongeacht het implementatiescenario moet u uw cloud-service configureren met een virtueel IP-adres waarmee een vrij toegankelijke IP-adres en poort, kan de werklast over de twee exemplaren van Windows Server AD FS STS- of proxy-exemplaren.

- [Hoge beschikbaarheid-configuratie AD FS van Windows Server](#BKMK_ADFSHighAvail): het is raadzaam om de implementatie van een Windows Server AD FS-farm met ten minste twee servers voor failover en taakverdeling. U kunt overwegen Windows interne Database (WID) voor AD FS voor Windows Server-configuratiegegevens en interne load balancing van het vermogen Azure gebruiken voor het distribueren van binnenkomende aanvragen voor de servers in de farm.

Zie [AD DS Deployment Guide](https://technet.microsoft.com/library/cc753963)voor meer informatie.


### <a name="BKMK_HybridExt"></a>3. AD DS: Implementeren in een Windows Server AD DS-aware-toepassing waarvoor u de verbinding met het bedrijfsnetwerk.

![AD DS-implementatie van cross-gebouwen](media/active-directory-deploying-ws-ad-guidelines/ADDS_xprem.png)
**in figuur 3**

#### <a name="description"></a>Beschrijving

Een LDAP-toepassing is geïmplementeerd op een Azure virtuele machine. Het ondersteunt geïntegreerde Windows-verificatie en Windows Server in AD DS als opslagplaats gebruikt voor de configuratie- en profielgegevens. Het doel is voor de toepassing om te profiteren van de bestaande corporate Windows Server AD DS en op. De toepassing is geen claimbewuste. Gebruikers moeten ook direct toegang tot de toepassing via het Internet. Optimaliseren voor prestaties en kosten, wordt besloten dat twee extra domeincontrollers die deel uitmaken van het bedrijfsdomein naast de toepassing op Azure worden geïmplementeerd.

#### <a name="scenario-considerations-and-how-technology-areas-apply-to-the-scenario"></a>Overwegingen bij het scenario en hoe technologische gebieden van toepassing op het scenario

- [Netwerktopologie](#BKMK_NetworkTopology): een Azure virtueel netwerk maken met [cross-ruimten connectiviteit](../vpn-gateway/vpn-gateway-site-to-site-create.md).

- [Installatiemethode](#BKMK_InstallMethod): replica-DC's uit het zakelijke Windows Server Active Directory-domein te implementeren. U kunt voor een replica-DC Windows Server AD DS installeren op de VM en eventueel gebruiken installeren vanaf medium (IFM) voor de hoeveelheid gegevens die moeten worden gerepliceerd naar de nieuwe domeincontroller tijdens de installatie. Zie voor een zelfstudie, [een replica Active Directory-domeincontroller op Azure installeren](../active-directory/active-directory-install-replica-active-directory-domain-controller.md). Zelfs als u IFM gebruikt, kan het zijn efficiënter voor het bouwen van de virtuele DC op-ruimten en de hele virtuele vasteschijf (VHD) verplaatsen naar de cloud in plaats van tijdens de installatie van Windows Server in AD DS te repliceren. Voor de veiligheid, is het raadzaam de VHD uit het netwerk van gebouwen te verwijderen nadat is gekopieerd naar Azure.

- [Windows Server Active Directory-site-topologie](#BKMK_ADSiteTopology): een nieuwe Azure-site maken in Active Directory: Sites en Services. Maak een subnetobject voor Windows Server Active Directory vertegenwoordigen de Azure virtueel netwerk en het subnet aan de site toevoegen. Maak een nieuwe site-koppeling met de nieuwe Azure-site en de site waar Azure virtueel netwerk VPN-eindpunt gevestigd is te beheren en optimaliseren van Windows Server Active Directory-verkeer van en naar Azure.

- [IP-adressen en DNS](#BKMK_IPAddressDNS):

 - Een statisch IP-adres instellen voor de DC via de cmdlet Set-AzureStaticVNetIP Azure PowerShell.
 - Installeren en configureren van Windows Server DNS op de domeincontroller (s) op Azure.
 - De netwerkeigenschappen van het virtuele configureren met de naam en het IP-adres van de VM die fungeert als host voor de domeincontroller en DNS-serverfuncties.

- [DCs geo verdeeld](#BKMK_DistributedDCs): extra virtuele netwerken configureren, indien nodig. Als de sitetopologie van Active Directory is vereist voor DC's in regio's die met de verschillende regio's Azure overeenkomen dan u wilt maken van Active Directory: sites dienovereenkomstig.

- [Alleen-lezen domeincontrollers](#BKMK_RODC): U kunt een RODC in de site Azure implementeert, afhankelijk van uw vereisten voor het uitvoeren van schrijven bewerkingen op de DC en de compatibiliteit van toepassingen en services op de site met de RODC. Zie de [handleiding voor alleen - lezen domeincontrollers compatibiliteit](https://technet.microsoft.com/library/cc755190)voor meer informatie over compatibiliteit van toepassingen.

- [Globale catalogus](#BKMK_GC): GS aanvragen aanmelden in forests met meerdere domeinen nodig zijn. Als u een globale Catalogus op de Azure-site niet implementeren kan, zal u kosten egress verkeer als verificatieaanvragen query's GS in andere sites veroorzaakt. Als u wilt dat verkeer beperken, kunt u universele groepslidmaatschappen voor de Azure site in Active Directory: Sites en Services.

    Als u een globale Catalogus, site-koppelingen en site-koppelingen kosten zodanig configureren dat de globale Catalogus op de Azure-site is niet de aanbevolen als een bron-DC door andere GS die nodig is voor het repliceren van de dezelfde gedeeltelijke domeinpartities.

- [Plaatsing van de Windows Server AD DS-database en SYSVOL](#BKMK_PlaceDB): een gegevensschijf toevoegen aan DCs uitgevoerd op Azure VMs om het opslaan van de Windows Server Active Directory-database, logboekbestanden en SYSVOL.

- [Back-up en terugzetten](#BKMK_BUR): bepalen waar u wilt opslaan, back-ups van de staat. Zo nodig toevoegen een andere schijf met gegevens voor de DC-VM back-ups opslaat.

## <a name="deployment-decisions-and-factors"></a>Implementatie beslissingen en factoren

Deze tabel bevat een overzicht van de Windows Server Active Directory-technologie gebieden die zijn getroffen in de bovenstaande scenario's en de bijbehorende besluiten rekening te houden met koppelingen naar meer gedetailleerde informatie hieronder. Sommige gebieden van de technologie mogelijk niet van toepassing op elk implementatiescenario en het is mogelijk dat sommige gebieden van de technologie belangrijker om een inzetten dan andere gebieden van de technologie.

Bijvoorbeeld als u een replica-DC op een virtueel netwerk implementeren en uw forest slechts één domein heeft, te kiezen voor de implementatie van een globale-catalogusserver in dat geval worden niet essentieel zijn voor het scenario omdat het niet een extra replicatievereisten zal maken. Aan de andere kant als het forest meerdere domeinen heeft, vervolgens de beslissing voor de implementatie van een globale catalogus op een virtueel netwerk heeft gevolgen voor de beschikbare bandbreedte, prestaties, verificatie, zoekacties voor adreslijsten enzovoort.

| Gebied voor Windows Server Active Directory-technologie | Besluiten | Factoren |
| ---- | ---- | ---- |
| [Netwerktopologie](#BKMK_NetworkTopology) | Maakt u een virtueel netwerk? | <li>Vereisten voor toegang tot bronnen Corp</li> <li>Verificatie</li> <li>Accountbeheer</li> |
| [Implementatieconfiguratie DC](#BKMK_DeploymentConfig) | <li>Een afzonderlijk forest zonder alle vertrouwensrelaties te implementeren?</li> <li>Een nieuw forest met federation implementeren?</li> <li>Een nieuw forest met Windows Server Active Directory-forest-vertrouwensrelatie of Kerberos implementeren?</li> <li>Forest Corp uitbreiden door het implementeren van een replica-DC?</li> <li>Forest Corp uitbreiden door een nieuw onderliggend domein of een domeinstructuur implementeren?</li> | <li>Beveiliging</li> <li>Conformiteit</li> <li>Kosten</li> <li>Tolerantie en fouttolerantie</li> <li>Compatibiliteit van toepassingen</li> |
| [Windows Server Active Directory-site-topologie](#BKMK_ADSiteTopology) | Hoe kan u subnetten, sites en site-koppelingen configureren met Azure virtueel netwerk verkeer te optimaliseren en kosten te minimaliseren? | <li>Site-en subnet</li> <li>Site-Koppelingseigenschappen en waarschuwing wijzigen</li> <li>Compressie van replicatie</li> |
| [IP-adressen en DNS](#BKMK_IPAddressDNS) | Het configureren van IP-adressen en naamomzetting? | <li>Het gebruik van de Set-AzureStaticVNetIP-cmdlet gebruiken voor het toewijzen van een statisch IP-adres</li> <li>Windows Server-DNS-server installeren en configureren van de netwerkeigenschappen van het virtuele met de naam en het IP-adres van de VM die fungeert als host voor de domeincontroller en DNS-serverfuncties</li> |
| [DCs geo verdeeld](#BKMK_DistributedDCs) | Hoe worden gerepliceerd naar domeincontrollers op afzonderlijke virtuele netwerken? | Als de sitetopologie van Active Directory is vereist voor DC's in regio's die overeenkomt met verschillende regio's Azure dan u wilt maken van Active Directory: sites dienovereenkomstig. [Virtueel netwerk configureren met virtual network Connectivity](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md) om te repliceren tussen domeincontrollers op afzonderlijke virtuele netwerken. |
| [Alleen-lezen domeincontrollers](#BKMK_RODC) | Alleen-lezen of beschrijfbare domeincontrollers gebruiken? | <li>HBI/PII filterkenmerken</li> <li>Filteren van geheimen</li> <li>Limiet voor uitgaand verkeer</li> |
| [Globale catalogus](#BKMK_GC) | Globale catalogus installeren? | <li>Zorg voor één domein-forest, alle DC's GS</li> <li>Forest met meerdere domeinen zijn GS vereist voor verificatie</li> |
| [Installatiemethode](#BKMK_InstallMethod) | Het installeren van DC in Azure? | Ofwel: <li>Installatie van AD DS met Dcpromo of met Windows PowerShell</li> <li>VHD van een virtuele DC op locatie verplaatsen</li> |
| [Plaatsing van de Windows Server AD DS-database en SYSVOL](#BKMK_PlaceDB) | Waar wilt u Windows Server AD DS-database, logboekbestanden en SYSVOL opgeslagen? | Dcpromo.exe standaardwaarden wijzigen. Deze essentiële Active Directory-bestanden *moeten* worden geplaatst op gegevensschijven Azure niet Operating System-schijven die de schrijfcache implementeren. |
| [Back-up en terugzetten](#BKMK_BUR) | Hoe beschermen en herstellen van gegevens? | Systeem voor back-ups maken |
| [Configuratie van de Federation-server](#BKMK_FedSrvConfig) | <li>Implementeren in een nieuw forest met de Federatie in de cloud?</li> <li>Implementeren van AD FS op gebouwen en een proxy in de cloud worden blootgesteld?</li> | <li>Beveiliging</li> <li>Conformiteit</li> <li>Kosten</li> <li>Toegang tot toepassingen met zakelijke partners</li> |
| [Configuratie van cloud services](#BKMK_CloudSvcConfig) | Een cloud-service wordt impliciet geïmplementeerd voor de eerste keer dat u een virtuele machine maken. Moet u extra cloud-services implementeren? | <li>Directe blootstelling aan het Internet vereisen een VM of VMs?</li> <li> De service vereist belasting?</li> |
| [Vereisten voor Federation-server voor openbare en particuliere IP-adressen voor (dynamische IP vs. virtuele IP)](#BKMK_FedReqVIPDIP) | <li>Moet het exemplaar van Windows Server AD FS rechtstreeks vanaf Internet bereikbaar?</li> <li>Vereist de toepassing wordt geïmplementeerd in de cloud een eigen internetverbinding IP-adres en poort?</li> | Cloud-service voor elk virtueel IP-adres dat voor uw distributie vereist is maken |
| [Windows Server AD FS-configuratie voor hoge beschikbaarheid](#BKMK_ADFSHighAvail) | <li>Hoeveel knooppunten er in mijn Windows Server AD FS-serverfarm?</li> <li>Hoeveel knooppunten te implementeren in de farm Windows Server AD FS-proxy?</li> | Tolerantie en fouttolerantie |

### <a name="BKMK_NetworkTopology"></a>Netwerktopologie

Om te voldoen aan de consistentie van IP-adres en DNS-vereisten voor Windows Server AD DS, is het nodig om eerst een [virtueel netwerk Azure](../virtual-network/virtual-networks-overview.md) maakt en koppelt uw virtuele machines. Tijdens het maken, moet u beslissen of u verbinding met uw bedrijfsnetwerk op gebouwen, die transparant verbinding Azure virtuele machines met machines voor gebouwen desgewenst uitbreiden, dit wordt bereikt met behulp van traditionele technologieën voor VPN- en vereist dat een VPN endpoint worden blootgesteld aan de rand van het bedrijfsnetwerk. Dat wil zeggen, wordt de VPN-verbinding geïnitieerd vanuit Azure met het bedrijfsnetwerk, niet omgekeerd.

Houd er rekening mee dat extra kosten van toepassing zijn bij het uitbreiden van een virtueel netwerk met het netwerk op ruimten buiten de standaard de kosten voor elke VM. Met name zijn kosten voor CPU-tijd van de gateway Azure Virtual Network en egress verkeer dat wordt gegenereerd door elk VM die met machines voor ruimten tussen de VPN-verbinding communiceert. Zie [Azure prijzen bij-overzicht](http://azure.microsoft.com/pricing/)voor meer informatie over het netwerkverkeer toeslagen.

### <a name="BKMK_DeploymentConfig"></a>Implementatieconfiguratie DC

De manier waarop u de domeincontroller configureren, is afhankelijk van de vereisten voor de service die u wilt uitvoeren op Azure. U kunt bijvoorbeeld een nieuw forest, geïsoleerd uit uw eigen corporate forest, voor het testen van een bewijs van concept, een nieuwe toepassing of sommige andere kortdurend project waarvoor directoryservices, maar niet specifiek toegang tot interne bedrijfsnetwerken implementeren.

Als een voordeel, een geïsoleerd forest die DC niet gerepliceerd met op-premises DCs, waardoor er minder uitgaand netwerkverkeer gegenereerd door het systeem zelf rechtstreeks kosten te verlagen. Zie [Azure prijzen bij-overzicht](http://azure.microsoft.com/pricing/)voor meer informatie over het netwerkverkeer toeslagen.

Stel bijvoorbeeld dat u privacy vereisten voor een service, maar de service is afhankelijk van de toegang tot uw interne Windows Server Active Directory. Als u mag hostgegevens voor de service in de cloud, kunt u een nieuw onderliggend domein implementeren voor het interne forest op Azure. In dit geval kunt u een domeincontroller voor het nieuwe onderliggende domein (zonder de globale catalogus om privacyoverwegingen adres) implementeren. In dit scenario met een replica-DC-implementatie vereist een virtueel netwerk voor connectiviteit met uw DCs op gebouwen.

Als u een nieuw forest maakt, kiest u of u [Active Directory-vertrouwensrelaties](https://technet.microsoft.com/library/cc771397) of [Federation-vertrouwensrelaties](https://technet.microsoft.com/library/dd807036). Het saldo van de eisen die door de compatibiliteit, beveiliging, naleving, kosten en tolerantie. Bijvoorbeeld om te profiteren van de [Selectieve verificatie](https://technet.microsoft.com/library/cc755844) kan u een nieuw forest op Azure implementeren en een Windows Server Active Directory-vertrouwensrelatie tussen de in de lokalen en het cloud forest maken. Als de toepassing claimbewuste, kan u echter implementeren in plaats van forest-vertrouwensrelaties van Active Directory federation-vertrouwensrelaties. Een andere factor zijn de kosten meer gegevens worden gerepliceerd door het uitbreiden van uw op ruimten Windows Server Active Directory naar de cloud of genereren meer uitgaand verkeer als gevolg van de verificatie en query laden.

Vereisten voor beschikbaarheid en fouttolerantie hebben ook invloed op uw keuze. Bijvoorbeeld als de koppeling wordt verbroken, vertrouwen toepassingen gebruik te maken van Kerberos-vertrouwensrelatie of een Federatie zijn alle waarschijnlijk volledig gebroken tenzij u voldoende infrastructuur op Azure hebt geïmplementeerd. Alternatieve implementatieconfiguraties zoals replica-DC (beschrijfbare of RODC) vergroot de kans dat bestand tegen storingen van de koppeling.

### <a name="BKMK_ADSiteTopology"></a>Windows Server Active Directory-site-topologie

U moet goed definiëren sites en sitekoppelingen om verkeer te optimaliseren en kosten te minimaliseren. Sites, site-koppelingen en subnetten van invloed zijn op de replicatietopologie tussen DC en de stroom van de verificatie. Houd rekening met de volgende kosten van verkeer implementeren en DCs overeenkomstig de voorschriften van het implementatiescenario configureren:

- Er is een nominale vergoeding per uur voor de gateway zelf:

 - Worden kan gestart en gestopt wens naar

 - Als gestopt, zijn Azure VMs geïsoleerd van het bedrijfsnetwerk.

- Binnenkomend verkeer is gratis

- Uitgaand verkeer wordt belast volgens [Azure op overzichtelijke prijsstelling](http://azure.microsoft.com/pricing/). U kunt eigenschappen tussen sites op gebouwen en de wolk als volgt optimaliseren:

 - Als u meerdere virtuele netwerken, de site-koppelingen en hun kosten hiervoor configureren om te voorkomen dat de Azure site prioriteit boven een die qua niveau van service gratis Windows Server in AD DS. U kunt ook het uitschakelen van de Bridge alle-(BASL)-koppelingsoptie site (die standaard is ingeschakeld). Dit zorgt ervoor dat alleen direct-verbonden sites met elkaar repliceren. DCs in transitief verbonden sites kunnen niet meer met elkaar repliceren, maar moeten worden gerepliceerd via een gemeenschappelijke site of sites. Als de tussenliggende sites niet meer beschikbaar voor een of andere reden, replicatie tussen domeincontrollers in transitief verbonden sites niet wordt uitgevoerd zelfs als verbinding tussen de sites beschikbaar is. Ten slotte, waarbij gedeelten van de werking van replicatie transitief wenselijk blijven, sitekoppelingsbruggen maken die de juiste site-koppelingen en sites, zoals in lokalen, bedrijfsnetwerk sites bevatten.

 - [Site-koppelingskosten configureren](https://technet.microsoft.com/library/cc794882) op de juiste manier om te voorkomen dat onbedoelde verkeer. Bijvoorbeeld als **Probeer dichtstbijzijnde Site** instelling is ingeschakeld, controleert u of het virtuele netwerksites niet het eerstvolgende door stijging van de kosten van de site-link-object dat de Azure site terug naar het bedrijfsnetwerk verbindt wordt gekoppeld.

 - Configureren van site-koppeling [intervallen](https://technet.microsoft.com/library/cc794878) en [schema's](https://technet.microsoft.com/library/cc816906) volgens vereisten voor consistentie en snelheid van het object wordt gewijzigd. Replicatieschema uitlijnen met latentie tolerantie. DCs repliceren alleen het laatste frame van een waarde, zodat kosten verlagen het replicatie-interval kunt opslaan als er een voldoende object wijzigen.

- Als een minimum aan kosten een prioriteit is, Controleer replicatie is gepland en wijzigingsbericht niet is ingeschakeld. Dit is de standaardconfiguratie replicatie tussen sites. Dit is niet van belang als u een RODC op een virtueel netwerk implementeren wilt, omdat alle uitgaande wijzigingen worden niet gerepliceerd door de RODC. Maar als u een beschrijfbare domeincontroller implementeert, moet u controleren of dat de site-koppeling is niet geconfigureerd voor het repliceren van updates met onnodige frequentie. Als u een globale-catalogusserver (GC) implementeert, moet u dat elke site met een globale Catalogus domeinpartities repliceren vanaf een domeincontroller in een site die is gekoppeld aan een site-koppeling of site-koppelingen met een lagere prijs dan de globale Catalogus in de Azure-site-bron.


- Het is mogelijk om netwerkverkeer gegenereerd door middel van replicatie tussen sites door het compressiealgoritme replicatie nog verder te verminderen. De compressiealgoritme wordt bepaald door het compressiealgoritme REG_DWORD register post HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NTDS\Parameters\Replicator. De standaardwaarde is 3, die betrekking heeft op de Xpress compressie-algoritme. U kunt de waarde wijzigen op 2, waardoor het algoritme gewijzigd in MSZip. Hiermee vergroot u de compressie in de meeste gevallen, maar dit gebeurt ten koste van CPU-gebruik. Voor meer informatie, Zie [hoe Active Directory-replicatietopologie](https://technet.microsoft.com/library/cc755994).

### <a name="BKMK_IPAddressDNS"></a>IP-adressen en DNS

Azure virtuele machines worden 'DHCP-lease-adressen' standaard toegewezen. Omdat dynamische adressen Azure virtueel netwerk blijft bestaan met een virtuele machine voor de levensduur van de virtuele machine, wordt de eisen van de Windows Server AD DS voldaan.

Als gevolg hiervan, wanneer u een dynamisch adres op Azure, bent u in effect met een statisch IP-adres, omdat het geschikt voor de duur van de lease-overeenkomst is en de duur van de lease gelijk aan de levensduur van de cloud-service is.

Het dynamische adres wordt echter vrijgegeven als de VM afgesloten wordt. Als u wilt voorkomen dat het IP-adres wordt opgeheven, kunt u [Set-AzureStaticVNetIP om een statisch IP-adres toewijzen](http://social.technet.microsoft.com/wiki/contents/articles/23447.how-to-assign-a-private-static-ip-to-an-azure-vm.aspx).

Voor naamomzetting, implementeren van uw eigen (of maak gebruik van uw bestaande) DNS-serverinfrastructuur; DNS Azure-voorwaarde voldoet niet aan de behoeften van de naam van geavanceerde resolutie van Windows Server in AD DS. Bijvoorbeeld het geen ondersteuning voor dynamische SRV-records, enzovoort. Naamomzetting is een belangrijke configuratie-item voor domeincontrollers en clients domein behoren. De DCs van bronrecords registreert en het oplossen van andere DC's resource records kunnen worden.
Omwille van fouttolerantie fouttolerantie en prestaties is de service Windows-DNS-Server installeren op de DC's op Azure optimaal. Vervolgens moet u de eigenschappen Azure virtueel netwerk configureren met de naam en het IP-adres van de DNS-server. Wanneer andere VMs in het virtuele netwerk wordt gestart, wordt de omzettingscache van DNS-clientinstellingen worden geconfigureerd met DNS-server als onderdeel van de dynamische toewijzing van IP-adres.

> [AZURE.NOTE] U kunt niet deelnemen aan computers op ruimten naar een Windows Server AD DS Active Directory-domein dat wordt gehost op Azure rechtstreeks via het Internet. De Poortvereisten voor Active Directory en de domein-join-bewerking niet praktisch om rechtstreeks weergeven blootstellen de vereiste poorten en in feite een hele DC met het Internet.

VMs registreren hun DNS-naam automatisch bij het opstarten of wanneer er een naamswijziging.

Voor meer informatie over dit voorbeeld en een ander voorbeeld van de eerste VM inrichten en AD DS installeren op deze Zie [installeren van een nieuw Active Directory-forest op Microsoft Azure](active-directory-new-forest-virtual-machine.md). Zie voor meer informatie over het gebruik van Windows PowerShell [Installeren Azure PowerShell](../powershell-install-configure.md) en [Azure Management Cmdlets](https://msdn.microsoft.com/library/azure/jj152841).

### <a name="BKMK_DistributedDCs"></a>DCs geo verdeeld

Azure biedt voordelen wanneer meerdere domeincontrollers in verschillende netwerken, virtuele host:

- Fouttolerantie meerdere locaties

- Fysieke nabijheid van filialen (lagere latentie)

Zie [virtuele netwerken configureren voor verbinding met het virtuele netwerk](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md)voor meer informatie over het configureren van rechtstreekse communicatie tussen virtuele netwerken.

### <a name="BKMK_RODC"></a>Alleen-lezen domeincontrollers

U moet kiezen of u alleen-lezen of beschrijfbare domeincontrollers implementeren. Kan helling ten opzichte RODC implementeren omdat geen fysieke controle over hen, maar de RODC zijn ontworpen om te worden geïmplementeerd op locaties waar hun fysieke veiligheid bedreigd, zoals filialen wordt.

Azure is geen gevaar de fysieke beveiliging van een filiaal, maar de RODC kunnen nog blijken efficiënte omdat ze bieden functies zijn geschikt voor deze omgevingen zij het om verschillende redenen. Bijvoorbeeld RODC geen uitgaande replicatie hebt en kunnen selectief geheimen (wachtwoorden) te vullen. Neerwaarts gericht, het ontbreken van deze geheimen zult uitgaand verkeer op het verzoek om te verifiëren als een gebruiker of computer wordt geverifieerd. Maar geheimen kunnen selectief worden vooraf ingevuld en in de cache opgeslagen.

RODC bieden een extra voordeel in en rond HBI en persoonlijke gegevens vragen omdat u kenmerken gevoelige gegevens naar de RODC bevatten gefilterde kenmerkset (FAS) kunt toevoegen. De FAS is een aanpasbare reeks kenmerken die niet worden gerepliceerd naar de RODC. U kunt de FAS als beveiliging voor het geval u niet zijn toegestaan of niet wilt opslaan van persoonlijke gegevens of HBI op Azure. Zie voor meer informatie [gefilterde kenmerk alleen-lezen domeincontroller instellen [(https://technet.microsoft.com/library/cc753459)].

Zorg ervoor dat toepassingen zijn compatibel met de RODC die u wilt gebruiken. Veel Windows Server Active Directory-toepassingen goed werken met RODC, maar sommige toepassingen kunnen uitvoeren inefficiënt of als ze geen toegang tot een beschrijfbare domeincontroller mislukken. Voor meer informatie, Zie de [handleiding voor compatibiliteit van alleen-lezen domeincontrollers](https://technet.microsoft.com/library/cc755190).

### <a name="BKMK_GC"></a>Globale catalogus

U moet kiezen of u een globale catalogus (GC) installeren. In één domein-forest, moet u alle domeincontrollers als globale-catalogusservers te configureren. Kosten wordt niet verhoogd omdat er geen extra replicatieverkeer.

In een forest met meerdere domeinen zijn GS nodig voor universele groepslidmaatschappen vouwen tijdens het verificatieproces. Als u een globale Catalogus niet implementeren kan, werklast op het virtuele netwerk die worden geverifieerd bij een domeincontroller op Azure uitgaande verificatieverkeer GS op ruimten opvraagt tijdens elke verificatiepoging indirect genereert.

De kosten van GS zijn minder voorspelbaar omdat host voor elk domein (in deel). Als de werklast fungeert als host voor een internetgerichte-service verifieert gebruikers met Windows Server in AD DS, is het mogelijk dat de kosten volledig onvoorspelbaar. Als u wilt verminderen GC query's buiten de site wolk tijdens de verificatie, kunt u de [Universeel groepslidmaatschap in cache opslaan inschakelen](https://technet.microsoft.com/library/cc816928).

### <a name="BKMK_InstallMethod"></a>Installatiemethode

U moet het installeren van de DC's op het virtuele netwerk kiezen:

- Bevordering van nieuwe domeincontrollers. Zie [een nieuw Active Directory-forest op een Azure virtueel netwerk installeren](active-directory-new-forest-virtual-machine.md)voor meer informatie.

- De VHD van een virtuele DC op locatie verplaatsen naar de cloud. In dit geval moet u ervoor zorgen dat de virtuele DC on-premises 'verplaatst,"niet"gekopieerd"of"gekloonde".

Gebruik alleen Azure virtuele machines voor DC's (in plaats van Azure "web" of "werknemer" rol VMs). Ze zijn duurzaam en duurzaamheid van de staat voor een domeincontroller vereist is. Azure virtuele machines zijn ontworpen voor de werklast zoals DCs.

Gebruik SYSPREP niet te implementeren of DCs klonen. De mogelijkheid om het klonen van DCs is alleen beschikbaar begin in Windows Server 2012. De klonen functie vereist de ondersteuning voor VMGenerationID in de onderliggende hypervisor. Hyper-V in een Windows Server 2012 en Azure virtuele netwerken zowel ondersteuning voor VMGenerationID, zoals virtualisatie van derde leveranciers doen.

### <a name="BKMK_PlaceDB"></a>Plaatsing van de Windows Server AD DS-database en SYSVOL

Selecteer waar u de Windows Server AD DS-database, logboekbestanden en SYSVOL. Ze moeten worden geïmplementeerd op Azure gegevensschijven.

> [AZURE.NOTE] Azure gegevensschijven zijn beperkt tot 1 TB.

Schijven gegevens niet cache schrijft standaard gebeurt. Gegevensschijven die zijn gekoppeld aan een VM gebruiken write-through caching. Write-through caching zorgt ervoor dat het schrijven streeft naar duurzame Azure opslag voordat de transactie voltooid, vanuit het perspectief van het besturingssysteem van de VM is. Het biedt duurzaamheid ten koste van iets trager schrijft.

Dit is belangrijk voor Windows Server AD DS omdat write-behind schijfcaching hypothesen voor de DC retouraanroepen. Uitschakelen schrijfcache Server Windows AD DS wordt geprobeerd, maar het is tot de schijf-i/o-systeem het inwilligen. Fout bij het uitschakelen van de schrijfcache kan, onder bepaalde omstandigheden, USN-hersteloptie lingering objecten en andere problemen waardoor introduceren.

Beste voor virtuele DC, het volgende doen:

- Stel de voorkeursinstelling Host-Cache op de gegevensschijf Azure voor geen. Hiermee voorkomt u problemen met de schrijfcache voor AD DS-bewerkingen.

- Opslaan van de database, logboekbestanden en SYSVOL op beide dezelfde gegevensschijf of schijven van afzonderlijke gegevens. Dit is meestal een andere schijf dan de schijf waarop het besturingssysteem zelf. De belangrijkste takeaway is dat Windows Server AD DS-database en SYSVOL niet moeten worden opgeslagen op een schijftype besturingssysteem Azure. Het installatieproces van AD DS installeert deze onderdelen standaard in de map % systemroot %, die niet voor Azure aanbevolen wordt.

### <a name="BKMK_BUR"></a>Back-up en terugzetten

Zich bewust zijn van wat is en wordt niet ondersteund voor back-up en herstel van een domeincontroller in het algemeen, en meer, met name in een VM wordt uitgevoerd. Zie [back-up en terugzetten overwegingen voor gevirtualiseerde DC](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv#backup_and_restore_considerations_for_virtualized_domain_controllers).

Maak systeem back-ups met alleen back-upsoftware die specifiek op de hoogte van de back-up van de vereisten voor Windows Server in AD DS, zoals Windows Server Backup.

Niet kopiëren of klonen VHD van DCs-bestanden in plaats van het uitvoeren van regelmatige back-ups. Een herstelpunt moet ooit vereist zijn, doen, met behulp van gekloonde of gekopieerde VHD's zonder Windows Server 2012 en een ondersteunde hypervisor USN-Bolletjes zal brengen.

### <a name="BKMK_FedSrvConfig"></a>Configuratie van de Federation-server

De configuratie van Windows Server AD FS-federation-servers (STSs) hangt gedeeltelijk af of de toepassingen die u wilt implementeren op Azure nodig hebt voor toegang tot bronnen in het netwerk op gebouwen.

Als de toepassingen voldoen aan de volgende criteria, kan u de toepassingen in een geïsoleerde omgeving implementeren van uw netwerk op gebouwen.

- Zij aanvaarden SAML-tokens voor beveiliging

- Ze zijn exposable met het Internet

- Ze geen toegang krijgen tot bronnen op de gebouwen

In dit geval Windows Server AD FS STSs als volgt configureren:

1. Een geïsoleerd forest met één domein configureren op Azure.

2. Federatieve toegang bieden tot het forest met het configureren van een Windows Server AD FS-federation-serverfarm.

3. Configureren Windows Server AD FS (federation-serverfarm en proxy federatieserverfarm) in het forest op gebouwen.

4. Een federation-vertrouwensrelatie tussen lokale en Azure exemplaren van AD FS van Windows Server instellen.

Aan de andere kant als de toepassingen toegang tot bronnen voor gebouwen, kan u AD FS van Windows Server met de toepassing op Azure als volgt:

1. Configureren verbindingen tussen netwerken op gebouwen en Azure.

2. Een Windows Server AD FS-federation-serverfarm configureren in het forest op gebouwen.

3. Een AD FS van Windows Server proxy federatieserverfarm configureren op Azure.

Deze configuratie heeft het voordeel van de vermindering van de blootstelling van de middelen voor gebouwen, vergelijkbaar met het configureren van Windows Server AD FS met toepassingen in een perimeternetwerk.

Houd er rekening mee dat in beide scenario's moet u relaties tot stand vertrouwensrelaties met meer identiteitsaanbieders van brengen kunt, als business-to-business samenwerking nodig is.

### <a name="BKMK_CloudSvcConfig"></a>Configuratie van cloud services

Cloud-services zijn nodig als u wilt een VM rechtstreeks met het Internet maken of een internetgerichte afzonderlijke toepassingen met taakverdeling worden blootgesteld. Dit is mogelijk omdat elke cloud-service één configureerbare virtuele IP-adres biedt.

### <a name="BKMK_FedReqVIPDIP"></a>Vereisten voor Federation-server voor openbare en particuliere IP-adressen voor (dynamische IP vs. virtuele IP)

Elke Azure virtuele machine ontvangt een dynamisch IP-adres. Een dynamisch IP-adres is een particulier adres toegankelijk alleen in Azure. In de meeste gevallen echter zal het nodig zijn voor het configureren van een virtueel IP-adres voor de implementatie van AD FS van Windows Server. De virtuele IP is nodig om AD FS van Windows Server-eindpunten op het Internet beschikbaar te maken en door federatieve partners en -clients wordt gebruikt voor verificatie en het beheer. Een virtueel IP-adres is een eigenschap van een cloud-service bevat een of meer Azure virtuele machines. Als de claimbewuste toepassing wordt geïmplementeerd op Azure en Windows Server AD FS verbonden met Internet en gemeenschappelijke poorten delen zijn, elk vereist een virtueel IP-adres van de eigen en daarom moet men een cloud service voor de toepassing en een tweede voor Windows Server AD FS te maken.

Zie voor definities van de termen virtueel IP-adres en een dynamisch IP-adres, [termen en definities](#BKMK_Glossary).

### <a name="BKMK_ADFSHighAvail"></a>Windows Server AD FS-configuratie voor hoge beschikbaarheid

Het is mogelijk op een zelfstandige Windows Server AD FS-federation-services implementeren, verdient een farm met ten minste twee knooppunten voor AD FS STS en productieomgevingen-proxy's implementeren.

Zie [AD FS-implementatie 2.0 topologie overwegingen](https://technet.microsoft.com/library/gg982489) in de [AD FS 2.0 Design Guide](https://technet.microsoft.com/library/dd807036) om te beslissen welke configuratieopties implementatie best aanpassen aan uw specifieke behoeften.

> [AZURE.NOTE] Alle leden van de AD FS van Windows Server-farm configureren in dezelfde service cloud om taakverdeling voor Windows Server AD FS eindpunten op Azure en load balancing mogelijkheden van Azure voor HTTP (standaard 80) en HTTPS-poorten (standaard 443) gebruiken. Zie voor meer informatie [de sonde Azure taakverdeling](https://msdn.microsoft.com/library/azure/jj151530).
Windows Server Network Load Balancing (NLB) wordt niet ondersteund op Azure.
