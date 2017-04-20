<properties
   pageTitle="Handleiding voor de handmatige installatie van SAP HANA in Azure VMs | Microsoft Azure"
   description="Handleiding voor de handmatige installatie van SAP HANA in Azure VMs"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="hermanndms"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"
   keywords=""/>
<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="09/15/2016"
   ms.author="hermannd"/>

# <a name="quickstart-guide-for-manual-installation-of-single-instance-sap-hana-on-azure-vms"></a>Handleiding voor de handmatige installatie van één exemplaar SAP HANA in Azure VMs

## <a name="introduction"></a>Inleiding

Deze handleiding helpt om een single-instance SAP HANA prototype/demo systeem in Azure VMs door een handmatige installatie van SAP NetWeaver 7.5 en SAP HANA SP12 in te stellen.
De handleiding wordt ervan uitgegaan dat de lezer vertrouwd met Azure IaaS basisbeginselen zoals het implementeren van virtuele machines of virtuele netwerken, hetzij via de portal Azure of Powershell/CLI is-met inbegrip van de optie json-sjablonen. Voorts wordt verwacht dat de lezer vertrouwd met SAP HANA, SAP NetWeaver en het installeren van is op gebouwen.

Verwacht wordt dat de lezer op de hoogte van de algemene SAP Azure documentatie is vermeld in de sectie Algemene informatie aan het einde van het artikel is.

Als gevolg van de beperking niet voor productie-systemen in deze handleiding wordt niet betrekking op onderwerpen zoals HA, een back-up, DR, hoge prestaties of speciale beveiligingsmaatregelen.

De instellingen van de steekproef is uitgevoerd met twee virtuele machines voor een gedistribueerde installatie SAP NetWeaver via het model Azure Resource Manager als SAP-Linux-Azure wordt alleen ondersteund op Azure Resource Manager en niet het klassieke model. In de sectie Algemene informatie aan het einde van dit artikel vindt u koppelingen naar meer informatie over Azure Resource Manager.

Dit zijn de twee test VMs gebruikt voor de installatie van de steekproef:

* Hana-appsrv (type DS3) als host voor de instantie NW 7.5 ASC's + PAS
* Hana-dbsrv (type GS4) naar host HANA SP12
* beide VMs deel uitmaakten van een Azure virtueel netwerk (azure-hana-test-vnet)
* OS in beide gevallen is SLES 12 SP1


Zich bewust zijn van het feit echter dat vanaf juli 2016 SAP HANA is alleen volledig ondersteund voor OLAP (BW) productiesystemen voor Azure VM type GS5. Voor testdoeleinden waar een SAP overheidssteun niet wordt verwacht is het fijn een kleinere zoals bijvoorbeeld GS4 gebruikt.
Wat moet altijd worden gebruikt voor SAP HANA op Azure Azure premie opslag voor HANA gegevens en logboekbestanden is - Zie 'Disk Setup' sectie verder naar beneden. Raadpleeg de sectie Algemene informatie aan het einde van dit artikel voor meer informatie over welke SAP producten op Azure worden ondersteund.


De gids beschrijft twee verschillende manieren SAP HANA handmatig installeren op Azure VMs:

* installeren van SAP HANA via SAP Software inrichten Manager (SWPM) als onderdeel van een gedistribueerde installatie NetWeaver in de stap "database-exemplaar
* installeren SAP HANA met behulp van het hulpprogramma HANA levenscyclus Manager hdblcm en vervolgens NetWeaver achteraf

Het is ook van alle onderdelen (SAP HANA, SAP application server, SAP GUI-instantie ASC's) in één enkele VM installeren en gebruiken van SWPM. Deze optie wordt niet beschreven in dit artikel, maar de items die moeten worden beschouwd als identiek zijn.

Voordat u begint met een installatie moet het gedeelte na de controlelijsten hieronder over het instellen van de VMs Azure test om te voorkomen dat verschillende fundamentele fouten die gebeurt er als u met behulp van een standaard Azure VM configuratie alleen worden gelezen.


## <a name="checklist-sap-hana-installation-via-sap-swpm"></a>Controlelijst SAP HANA installatie via SAP-SWPM

Dit is een eenvoudige checklist van de items die betrekking hebben op een handmatige installatie van één exemplaar SAP HANA voor demonstratie of het maken van prototypen pursposes via SAP SWPM doet een gedistribueerde SAP NW 7.5 installeren sleutel. De afzonderlijke artikelen worden toegelicht en in de vorm van schermopnamen in meer detail in het artikel wordt weergegeven:

* een Azure virtueel netwerk, waaronder de VMs twee test maken 
* twee Azure VMs met OS SLES 12 SP1 implementeren via Azure Resource Manager model 
* twee standaard schijven koppelen aan de app server VM (bv. 75GB en 500GB)
* vier schijven koppelen aan de HANA DB server VM - 2 standaard schijven net als voor de app server VM + 2 premium schijven (bv. 2x512GB)
* afhankelijk van de grootte en/of doorvoer eisen koppelen meerdere schijven en een striped volumes maken met lvm of mdadm op OS niveau binnen de VM
* XFS bestandssystemen te maken op de aangesloten schijven / logische volumes
* Koppel de nieuwe XFS bestandssystemen op OS niveau. Een bestandssysteem gebruiken om alle SAP-software en de andere bijvoorbeeld de map sapmnt en misschien back-ups. Op de SAP HANA DB server mount het XFS-bestandssystemen op de schijven als /hana en /usr/sap, dit allemaal nodig om te voorkomen is dat het root-bestandssysteem die niet te groot op Linux Azure VMs vol premium
* Geef het lokale IP-adressen van de test VMs in/etc/hosts
* Geef de parameter nofail in /etc/fstab
* kernel parameters instellen volgens de notitie SAP HANA-SLES-12 (Zie details verder naar beneden in de kernel maximumconcentratie sectie)
* swap ruimte toevoegen
* Als - Installeer een grafisch bureaublad met de VMs-test. Gebruik anders een sapinst voor externe installatie
* de SAP-software downloaden van SAP service marketplace
* Het ASC's SAP-exemplaar op de server app VM installeren
* deelt u de map sapmnt via NFS tussen de VMs-test (app server VM is de NFS-server)
* de database-instantie waaronder HANA via SWPM op de server DB VM installeren
* de PAS op de app server VM installeren
* Start SAP MC en bijvoorbeeld verbinding via SAP GUI / HANA Studio 



## <a name="checklist-sap-hana-installation-via-hdblcm"></a>Controlelijst SAP HANA installatie via hdblcm

Dit is een eenvoudige checklist van de items die betrekking hebben op een handmatige installatie van één exemplaar SAP HANA voor demonstratie of het maken van prototypen pursposes via SAP SWPM doet een gedistribueerde SAP NW 7.5 installeren sleutel. De afzonderlijke artikelen worden toegelicht en in de vorm van schermopnamen in meer detail in het artikel wordt weergegeven:

* een Azure virtueel netwerk, waaronder de VMs twee test maken 
* twee Azure VMs met OS SLES 12 SP1 implementeren via Azure Resource Manager model 
* twee standaard schijven koppelen aan de app server VM (bv. 75GB en 500GB)
* vier schijven koppelen aan de HANA DB server VM - 2 standaard opslag zoals de app server VM + 2 premium schijven (bv. 2x512GB)
* afhankelijk van de grootte en/of doorvoer eisen koppelen meerdere schijven en een striped volumes maken met lvm of mdadm op OS niveau binnen de VM
* XFS bestandssystemen te maken op de aangesloten schijven / logische volumes
* Koppel de nieuwe XFS bestandssystemen op OS niveau. Een bestandssysteem gebruiken om alle SAP-software en de andere bijvoorbeeld de map sapmnt en misschien back-ups. Op de SAP HANA DB server mount het XFS-bestandssystemen op de schijven als /hana en /usr/sap, dit allemaal nodig om te voorkomen is dat het root-bestandssysteem die niet te groot op Linux Azure VMs vol premium
* Geef het lokale IP-adressen van de test VMs in/etc/hosts
* Geef de parameter nofail in /etc/fstab
* kernel parameters instellen volgens de notitie SAP HANA-SLES-12 (Zie details verder naar beneden in de kernel maximumconcentratie sectie)
* swap ruimte toevoegen
* Als - Installeer een grafisch bureaublad met de VMs-test. Gebruik anders een sapinst voor externe installatie
* de SAP-software downloaden van SAP service marketplace
* groep 'sapsys' met groep-id 1001 op de VM HANA DB-Server maken
* SAP HANA op de databaseserver VM installeren met behulp van hdblcm
* Het ASC's SAP-exemplaar op de server app VM installeren
* deelt u de map sapmnt via NFS tussen de VMs-test (app server VM is de NFS-server)
* de database-instantie waaronder HANA via SWPM op de server DB VM installeren
* de PAS op de app server VM installeren
* Start SAP MC en bijvoorbeeld verbinding via SAP GUI / HANA Studio 




## <a name="prepare-azure-vms-for-manual-installation-of-sap-hana"></a>Azure VMs voorbereiden voor handmatige installatie van SAP HANA

In dit hoofdstuk over het voorbereiden van het VMs Azure voor handmatige installatie van SAP HANA bestaat uit vijf secties die betrekking hebben op de volgende onderwerpen:

* Disk Setup
* Kernel Parameters
* FILESYSTEMS
* / etc/hosts
* / etc/fstab


### <a name="disk-setup"></a>Schijf-setup

Het root-bestandssysteem in een Linux VM in Azure is van beperkte omvang. Daarom is het nodig extra ruimte toevoegen aan een VM voor het uitvoeren van SAP. In het geval van een SAP-app server die VM in een zuivere prototype/demo-omgeving gebruikt is het fijn Azure standaardopslag schijven te gebruiken. Overwegende dat voor de bestanden van de gegevens- en logboekbestanden voor SAP HANA DB - moeten zelfs in een niet-productieve liggend Azure Premium schijven worden gebruikt.

Sommige details over het koppelen van schijven naar een Linux VM [hier](virtual-machines-linux-add-disk.md)

Wanneer het gaat om schijfcaching Azure - moet een 'Geen' voor schijven die worden gebruikt voor het opslaan van de transactielogboeken HANA gebruiken. Voor HANA lezen gegevensbestanden die het ok is om te gebruiken in de cache opslaan. HANA is een in-memory database die afhankelijk van de algehele gebruikspatroon hoeveel de cache op het niveau van Azure schijf lezen is verbeteren prestaties (bijvoorbeeld HANA starten en gegevens lezen van de schijf in het geheugen).

Zie voor meer informatie over Azure Premium opslag [hier](../storage/storage-premium-storage.md)

[Hier](https://github.com/Azure/azure-quickstart-templates) vindt u json voorbeeldsjablonen VMs maken.
De "101-vm-eenvoudig-linux" ziet u hoe een standaardsjabloon eruitziet met inbegrip van de opslag-sectie waarmee een schijf van 100GB gegevens worden toegevoegd.

[Dit artikel](virtual-machines-linux-sap-on-suse-quickstart.md) bevat informatie over het zoeken naar een afbeelding SUSE via Powershell of CLI en het belang voor het koppelen van een schijf via UUID.


Afhankelijk van de grootte van de eisen en de doorvoer kan het nodig zijn voor het koppelen van meerdere schijven in plaats van één en later bij het maken van een stripeset op deze schijven op OS niveau. Dit zijn de twee aspecten, waarom zou een een stripeset op meerdere schijven van Azure maken:

* de doorvoer te verhogen
* een enkel bestandssysteem > 1TB nodig omdat de huidige limiet van Azure schijf 1TB (met ingang van juli 2016)


Meer informatie over de twee belangrijkste hulpprogramma's voor het configureren van striping vindt u hier:

[Artikel over het configureren van Linux software raid op een Azure VM met mdadm](virtual-machines-linux-configure-raid.md)

[Artikel over het beheer van logische volumes configureren op een Linux Azure VM](virtual-machines-linux-configure-lvm.md)



![](./media/virtual-machines-linux-sap-hana-get-started/image003.jpg)

In de test zijn omgeving twee Azure standaard schijven aangesloten op de SAP-app server VM. Een is gebruikt voor het opslaan van de SAP-software voor de installatie (bijvoorbeeld NetWeaver 7.5, GUI, SAP, SAP HANA... ) en de andere aan onvoldoende ruimte voor wat mogelijk vereist (bv. een back-up, testgegevens) en de sapmnt-map (bijvoorbeeld SAP profielen) worden gedeeld door alle VMs die deel uitmaken van het landschap met hetzelfde SAP.

![](./media/virtual-machines-linux-sap-hana-get-started/image004.jpg)

VM vier schijven zijn in strijd is met de app server aangesloten op de server SAP HANA VM. Zoals voordat twee schijven werden gebruikt voor het bijhouden van de SAP-software (een kan ook de SAP-software schijf via NFS delen) en met voldoende ruimte voor back-up bijvoorbeeld. De twee schijven zijn schijven voor Azure Premium SAP HANA gegevens- en logboekbestanden bestanden als de map /usr/sap te houden.


### <a name="kernel-parameters"></a>Kernel parameters


SAP HANA vereist specifieke Linux kernel instellingen die geen deel uitmaken van de standaard Azure-afbeeldingen en moeten handmatig worden ingesteld. Er is een specifieke SAP notitie waarin de instellingen worden beschreven. 


Opmerking voor SAP SAP HANA DB: Aanbevolen instellingen voor SLES 12 / SLES voor SAP-toepassingen 12: [SAP notitie 2205917](https://launchpad.support.sap.com/#/notes/2205917)

Een extra onderwerp reagrding-cache voor pagina voor SAP HANA uitgevoerd op SLES vindt u [hier](https://www.suse.com/documentation/sles_for_sap/singlehtml/sles_for_sap_guide/sles_for_sap_guide.html#sec.s4s.configure.page-cache) in hoofdstuk 6.1 Kernel: Cache voor pagina limiet

Er is ook een notitie SAP met betrekking tot de cache voor pagina limiet [SAP notitie 1557506](https://service.sap.com/sap/support/notes/1557506)

SLES 12 heeft een nieuwe tool die het oude sapconf-hulpprogramma vervangt. Het is 'afgestemd adm' en er is een speciale SAP HANA profiel beschikbaar. Een vindt meer informatie over dit hulpprogramma de onderstaande twee koppelingen te volgen.

Documentatie over afgestemd adm profiel sap-hana SLES vindt u [hier](https://www.suse.com/documentation/sles-for-sap-12/book_s4s/data/sec_s4s_configure_sapconf.html)

Documentatie over afgestemd adm SLES sap-hana - profiel afstemmen hoofdstuk 6.2-systemen voor SAP werkbelasting met afgestemd adm - vindt u [hier](https://www.suse.com/documentation/sles-for-sap-12/pdfdoc/book_s4s/book_s4s.pdf)


![](./media/virtual-machines-linux-sap-hana-get-started/image005.jpg)

Hier kunnen zien hoe de transparent_hugepage als de numa_balancing-waarden volgens de vereiste instellingen voor SAP HANA worden gewijzigd in 'afgestemd adm'.


Een heeft grub2 op SLES 12 gebruiken om de instellingen van de kernel SAP HANA permanent maken. Meer informatie over grub2 vindt u [hier](https://www.suse.com/documentation/sled-12/book_sle_admin/data/sec_grub2_file_structure.html)


![](./media/virtual-machines-linux-sap-hana-get-started/image006.jpg)

Deze schermafbeelding ziet u hoe de kernel-instellingen zijn gewijzigd in het configuratiebestand en vervolgens "compiled" via grub2-mkconfig


![](./media/virtual-machines-linux-sap-hana-get-started/image007.jpg)

Een andere optie zou zijn voor het wijzigen van de instellingen via Yast en instellingen van de kernel Boot Loader.


### <a name="filesystems"></a>FILESYSTEMS 

![](./media/virtual-machines-linux-sap-hana-get-started/image008.jpg)

Hier kunnen de twee systemen die zijn gemaakt op de SAP app server VM op de twee gekoppelde Azure standaard schijven zien. Beide filesystems zijn van het type XFS en gekoppeld aan /sapdata en /sapsoftware.

Het is niet verplicht om het te doen op deze manier. Er zijn verschillende opties voor het sturcture van de schijfruimte.
Het belangrijkste is om te voorkomen dat het root-bestandssysteem geen ruimte meer wordt uitgevoerd. 


![](./media/virtual-machines-linux-sap-hana-get-started/image009.jpg)

Met betrekking tot de VM SAP HANA DB is het belangrijk om te weten dat tijdens de installatie van een database via sapinst (swpm) en alleen met de optie simple "standaardinstallatie" wordt geïnstalleerd dingen standaard onder /hana en /usr/sap. De standaardinstelling voor back-up van SAP HANA is bijvoorbeeld onder /usr/sap.
Dergelijke voordat deze sleutel om te voorkomen dat het root-bestandssysteem wordt uitgevoerd niet voldoende ruimte is. Daarom moet een Zorg ervoor dat er voldoende vrije ruimte onder /hana en /usr/sap voor de installatie van SAP HANA via swpm.

[In dit artikel](http://help.sap.com/saphelp_hanaplatform/helpdata/en/4c/24d332a37b4a3caad3e634f9900a45/frameset.htm) uit SAP beschrijft de indeling standaard bestandssysteem van SAP HANA 


![](./media/virtual-machines-linux-sap-hana-get-started/image010.jpg)

Wanneer u SAP NetWeaver installeert op een standaard afbeelding SLES 12 Azure zal er een bericht dat er geen swap-ruimte. Als u wilt verwijderen van dit bericht kan een bijvoorbeeld handmatig toevoegen een wisselbestand zoals beschreven in dit document via dd, mkswap en swapon. Alleen zoeken naar 'Toe te voegen een Swap-bestand handmatig' in [dit artikel](https://www.suse.com/documentation/sled-12/book_sle_deployment/data/sec_yast2_i_y2_part_expert.html)

Een andere mogelijkheid is swap ruimte via de Linux VM-agent configureren. Meer informatie vindt u [hier](virtual-machines-linux-agent-user-guide.md)


### <a name="etchosts"></a>/ etc/hosts

![](./media/virtual-machines-linux-sap-hana-get-started/image011.jpg)

Er is een ander belangrijk aspect voordat u begint met het installeren van SAP hostnamen en IP-adressen van de SAP-VMs opnemen in het bestand/etc/hosts. Een moet alle SAP VMs binnen één Azure virtueel netwerk implementeren en gebruik vervolgens de interne IP-adressen.

### <a name="etcfstab"></a>/ etc/fstab

![](./media/virtual-machines-linux-sap-hana-get-started/image000c.jpg)

Tijdens de testfase blijkt te zijn een goed idee om de parameter nofail toevoegen aan de fstab. Als er iets met de schijven misgaat zou vervolgens de VM nog steeds komen en niet vastloopt tijdens het opstartproces. Maar een Kijk uit zoals in dit geval de extra schijfruimte mogelijk niet beschikbaar en processen van het root-bestandssysteem kunnen vullen. In het geval /hana ontbrekende worden zou wouldn't SAP HANA start al helemaal.


## <a name="install-graphical-gnome-desktop-on-sles-12"></a>Grafische Gnome desktop installeren op SLES 12

Dit hoofdstuk bestaat uit twee secitions die betrekking hebben op de volgende onderwerpen:

* Installatie van Gnome desktop en xrdp op SLES 12
* Uitgevoerd op basis van Java SAP MC met Firefox op SLES 12

Een ook alternatieven zoals Xterminal, VNC kan gebruiken, maar vanaf Sep 2016 alleen beschreven xrdp.

### <a name="installation-of-gnome-desktop-and-xrdp-on-sles-12"></a>Installatie van Gnome desktop en xrdp op SLES 12

Met name voor degenen die de Microsoft Windows-achtergrond en wilt een grafisch bureaublad direct in de SAP-Linux VMs gebruiken voor het uitvoeren van Firefox, Sapinst, SAP GUI, MC SAP of HANA Studio en misschien de VM via RDP verbinding vanaf een computer met Microsoft Windows is er een eenvoudige manier om dit te bereiken. Hoewel dit mogelijk niet geschikt is voor een databaseserver productie b.v. is ok voor een zuivere prototype/demo-omgeving. Hier volgen de stappen voor het installeren van de Gnome desktop op een VM Azure SLES 12:

de gnome desktop installeren door de volgende opdracht (bijvoorbeeld in een venster stopverf):

   zypper -t patroon gnome basic

xrdp zodat de VM via RDP-verbinding installeren:

   zypper in xrdp

/etc/sysconfig/windowmanager bewerken en stel de standaard windows manager op Gnome:

   DEFAULT_WM = "gnome"

chkconfig om ervoor te zorgen dat xrdp automatisch wordt gestart na het opnieuw opstarten worden uitgevoerd: 

  chkconfig-xrdp op niveau 3

in het geval moet er een probleem met de RDP-verbinding probeert op te starten (misschien uit een venster stopverf):

  /etc/xrdp/xrdp.sh opnieuw starten

in het geval het xrdp opnieuw starten als bovengenoemde selectievakje werken als er een bestand .pid is en niet verwijderen:

  Controleer /var/run en zoek naar xrdp.pid   
  verwijdert u het en probeer het vervolgens opnieuw het opnieuw opstarten



### <a name="sap-mc"></a>SAP-MC


Bureaubaldachtergrond zoals beschreven in de vorige sectie een krijgt u de grafische Java gebaseerde SAP MC van Firefox in een VM Azure SLES 12 uitgevoerd na de installatie van de Gnome start een fout wegens het ontbreken van Java browser plugin.

De URL van de SAP-MC start is <server>: < instance_number > 13 5

Meer informatie vindt u [hier](https://help.sap.com/saphelp_nwce10/helpdata/en/48/6b7c6178dc4f93e10000000a42189d/frameset.htm)


![](./media/virtual-machines-linux-sap-hana-get-started/image013.jpg)

Op het screenshot hierboven kunt zien hoe het foutbericht kan eruit als de Browser Java-invoegtoepassing ontbreekt. 

![](./media/virtual-machines-linux-sap-hana-get-started/image014.jpg)

Één optie die het probleem op te lossen is eenvoudig te installeren de ontbrekende plugin via Yast.

![](./media/virtual-machines-linux-sap-hana-get-started/image015.jpg)

De URL van de SAP-MC herhalende weergeven wat dialoogvenster de eerste keer gevraagd de invoegtoepassing te activeren.


Één extra probleem dat kan worden weergegeven, wordt een foutbericht over een ontbrekend bestand: dit is zeer waarschijnlijk gerelateerd aan de installatie van Java 1.8 vereist voor SAP GUI 7.4 is javafx.properties

De IBM Java versie via Yast gezien omvat dit bestand niet. De oplossing is een Java-download van Oracle.
Een artikel die gesprekken over dit specifieke probleem voert vindt u [hier](https://scn.sap.com/thread/3908306)



## <a name="manual-sap-hana-installation-via-swpm-as-part-of-a-netweaver-75-installation"></a>Handmatige SAP HANA installatie via SWPM als onderdeel van een installatie NetWeaver 7.5


De volgende lijst met screenshots toont de belangrijkste stappen van het installeren van SAP NetWeaver 7.5 en SAP HANA SP12 via SWPM (sapinst). Als onderdeel van een NW 7.5 heeft installatie SWPM de mogelijkheden ook de HANA-database te installeren als een enkel exemplaar.


![](./media/virtual-machines-linux-sap-hana-get-started/image012.jpg)

Voor de steekproef test is omgeving slechts één ABAP app server geïnstalleerd. De optie "Distributed System" werd gebruikt om het exemplaar ASC's en het serverexemplaar primaire App installeren in een VM in Azure en SAP HANA als databasesysteem in een andere Azure VM.


![](./media/virtual-machines-linux-sap-hana-get-started/image016.jpg)

Zodra de ASC's exemplaar op de server app VM is geïnstalleerd en is ingesteld op 'groen' in de SAP MC de map sapmnt waarin bijvoorbeeld de profielmap SAP kan worden gedeeld met de databaseserver van SAP HANA VM heeft.
De DB installatiestap moet toegang tot deze informatie. De beste manier is gebruik van NFS die kan worden geconfigureerd met Yast.


![](./media/virtual-machines-linux-sap-hana-get-started/image017b.jpg)

Op de app moet server VM de map sapmnt worden gedeeld via NFS met behulp van de opties "rw" en "no_root_squash". De standaardwaarde is 'ro' en 'root_squash' die tot problemen leiden kunnen bij het installeren van de database-instantie.


![](./media/virtual-machines-linux-sap-hana-get-started/image018b.jpg)

Op de server van SAP HANA DB VM de sapmnt delen van de app server VM moet worden geconfigureerd via "Client voor NFS" (bv. met behulp van Yast)


![](./media/virtual-machines-linux-sap-hana-get-started/image019.jpg)

Een heeft vervolgens aanmelden bij de server SAP HANA DB VM en SWPM hiertoe de volgende stap van een gedistribueerde installatie van NW 7.5 - "Database-exemplaar starten.


![](./media/virtual-machines-linux-sap-hana-get-started/image035b.jpg)

Met betrekking tot de installatie van SAP HANA er niet in feite te veel om op te geven na het selecteren van een "standaardinstallatie". Naast heeft het pad naar de installatiom van een medium een SID DB, de hostnaam, het exemplaarnummer en het DB Sys Admin-wachtwoord invoeren.

 
![](./media/virtual-machines-linux-sap-hana-get-started/image036b.jpg)

Volgende stap is het wachtwoord invoeren voor het schema van de DBACOCKPIT.



![](./media/virtual-machines-linux-sap-hana-get-started/image037b.jpg)

Vervolgens komt de vraag om het wachtwoord SAPABAP1 schema.


![](./media/virtual-machines-linux-sap-hana-get-started/image023.jpg)

Aan het einde moet er alleen een groen vinkje voor elke fase van het installatieproces DB en het weinig bericht dat verschijnt moet Zeg "uitvoering van... Database-instantie is voltooid'.

 
![](./media/virtual-machines-linux-sap-hana-get-started/image024.jpg)

Na een geslaagde installatie weergeven de MC SAP moet ook de DB-instantie als 'groen' en de volledige lijst van SAP HANA processen (bv. hdbindexserver, hdbcompileserver)


![](./media/virtual-machines-linux-sap-hana-get-started/image025.jpg)

Deze screenshot toont delen van de structuur onder de /hana/shared die SWPM tijdens de installatie van HANA gemaakt. Er is geen optie om een ander pad opgeven. Daarom is het zo belangrijk voor het koppelen van extra schijfruimte onder een /hana vóór de installatie van SAP HANA via SWPM om te voorkomen dat het root-bestandssysteem wordt uitgevoerd niet voldoende vrije ruimte.


![](./media/virtual-machines-linux-sap-hana-get-started/image026.jpg)

Hier ziet een precies hetzelfde als beschreven voordat u voor de map /usr/sap.


![](./media/virtual-machines-linux-sap-hana-get-started/image027b.jpg)

De laatste stap van de installatie van gedistribueerde ABAP is de "primaire Application Server-exemplaar"


![](./media/virtual-machines-linux-sap-hana-get-started/image028b.jpg)

Nadat de PAS als SAP GUI hebt geïnstalleerd, kunt een controleren via de transactie "dbacockpit" die alles ziet er met de installatie van SAP HANA.

 
![](./media/virtual-machines-linux-sap-hana-get-started/image038b.jpg)

Stap 1 kan als een laatste SAP HANA Studio in de SAP-app server VM hebt geïnstalleerd en verbinding maken met de SAP HANA-exemplaar op de server DB VM.




## <a name="manual-sap-hana-installation-via-hana-life-cycle-manager-tool-hdblcm"></a>Handmatige installatie van SAP HANA via HANA levenscyclus beheer tool hdblcm


Naast het installeren van SAP HANA als onderdeel van een gedistribueerde installatie via SWPM is ook realistisch mogelijke HANA zelfstandige met hdblcm voor het eerst installeert en installeer vervolgens bijvoorbeeld SAP NetWeaver 7.5 achteraf. De lijst van de screenshots hieronder ziet u hoe dit werkt.

Hier zijn drie bronnen van informatie over het hulpprogramma HANA hdblcm:

[De juiste SAP HANA HDBLCM voor de taak kiezen](https://help.sap.com/saphelp_hanaplatform/helpdata/en/68/5cff570bb745d48c0ab6d50123ca60/content.htm)

[SAP HANA Lifecycle Management Tools](http://saphanatutorial.com/sap-hana-lifecycle-management-tools/)

[Update handleiding en installatie van SAP HANA](http://help.sap.com/hana/SAP_HANA_Server_Installation_Guide_en.pdf)



![](./media/virtual-machines-linux-sap-hana-get-started/image030.jpg)

Om te voorkomen dat problemen later met groep-id opgeven voor het uitvoeren de \<HANA SID\>adm gebruiker (gemaakt met het hulpprogramma hdblcm) een een nieuwe groep met de naam 'sapsys' met de groeps-id 1001 voor de installatie van SAP HANA met behulp van hdblcm te definiëren.




![](./media/virtual-machines-linux-sap-hana-get-started/image031.jpg)

Hdblcm de eerste keer zal er een eenvoudige startmenu waarin één item selecteren heeft vanaf 1 "nieuw systeem installeren"



![](./media/virtual-machines-linux-sap-hana-get-started/image032.jpg)

Op deze schermafbeelding ziet een alle belangrijke opties die zijn ingevoerd voordat. Belangrijk - mappen die zijn genoemd naar HANA logboek- en volumes, alsmede het installatiepad (/ hana/gedeeld in dit voorbeeld) en usr/sap niet deel uitmaken van het root-bestandssysteem, maar behoren tot de Azure data schijven die zijn aangesloten op de VM zoals beschreven in de sectie Azure VM setup. Hierdoor vermijdt het risico dat het root-bestandssysteem onvoldoende ruimte beschikbaar is mogelijk.
Ook kunnen zien dat de HANA admin-gebruiker gebruikers-id 1005 is en deel uitmaakt van de groep sapsys (id 1001) die is gedefinieerd voor de installatie.



![](./media/virtual-machines-linux-sap-hana-get-started/image033.jpg)

Een kunt controleren de HANA \<HANA SID\>(azdadm in dit voorbeeld) adm gebruikersgegevens in/etc/passwd



![](./media/virtual-machines-linux-sap-hana-get-started/image034.jpg)

Na de installatie van SAP HANA hdblcm met deze ziet in SAP HANA Studio. Het schema SAPABAP1, waaronder bijvoorbeeld alle SAP NetWeaver tabellen is nog niet beschikbaar.



![](./media/virtual-machines-linux-sap-hana-get-started/image035b.jpg)

Na de installatie van SAP HANA kunt een SAP NetWeaver erop te installeren. In dit voorbeeld werd met behulp van 'gedistribueerde installatie' via SWPM zoals beschreven in de bijbehorende sectie hierboven.
Wanneer het database-exemplaar via SWPM een gewoon installeren heeft dezelfde gegevens als voordat u met de hdblcm (bv. naam, HANA SID, exemplaarnummer) in te voeren. SWPM wordt vervolgens de bestaande installatie van HANA gebruiken en aanvullende schema's toevoegen.



![](./media/virtual-machines-linux-sap-hana-get-started/image036b.jpg)

Dit is de afbeelding van de SWPM installatiestap waar een heeft gegevens met betrekking tot het schema van de DBACOCKPIT in te voeren.


![](./media/virtual-machines-linux-sap-hana-get-started/image037b.jpg)

SWPM verwacht vervolgens gegevens over het schema van de SAPABAP1 in te voeren.


![](./media/virtual-machines-linux-sap-hana-get-started/image038b.jpg)

Het schema van de SAPABAP1 in HANA Studio kunnen zien zodra de SWPM database-exemplaar is geïnstalleerd.



![](./media/virtual-machines-linux-sap-hana-get-started/image039b.jpg)

En ten slotte na de installatie van de toepassingsserver SAP en SAP GUI een moet in staat zijn om te controleren of het exemplaar HANA DB met transactie "dbacockpit".




## <a name="general-information-related-to-sap-azure-certifications-running-sap-hana-on-azure-and-sap-software-download"></a>Algemene informatie over certificeringen voor SAP Azure, SAP HANA uitgevoerd op Azure en SAP-software downloaden

* algemene SAP Azure docu over SAP op Azure met Windows-besturingssysteem wordt uitgevoerd in de klassieke modus: [Met behulp van SAP op Windows virtuele machines in Azure](virtual-machines-windows-classic-sap-get-started.md)

* informatie over bestaande SAP-sjablonen voor gebruik door klanten: [Azure Quickstart sjablonen voor SAP](https://blogs.msdn.microsoft.com/saponsqlserver/2016/05/16/azure-quickstart-templates-for-sap/)

* algemene SAP Azure docu over het uitvoeren van SAP op Azure met Linux OS in Azure Resource Manager model: [Met behulp van SAP op Linux virtuele machines (VMs)](virtual-machines-linux-sap-get-started.md)

* gecertificeerde SAP HANA hardware directory waarin wordt aangegeven welke typen Azure VM voor productie worden ondersteund: [SAP HANA Certified® Hardware Directory](https://global.sap.com/community/ebook/2014-09-02-hana-hardware/enEN/iaas.html)

* informatie over formaten voor virtuele machine met name voor Linux-workloads: [formaten voor virtuele machines in Azure](virtual-machines-linux-sizes.md)

* SAP notitie waarin u alle producten van SAP op Azure ondersteund en ondersteunde typen Azure VM voor SAP: [SAP notitie 1928533](https://launchpad.support.sap.com/#/notes/1928533/E)

* Opmerking over SAP SAP "verbeterde monitoring" met Linux VMs op Azure: [SAP notitie 2191498](https://launchpad.support.sap.com/#/notes/2191498/E)

* SAP HANA aanbiedt op Azure "Grote exemplaren". Het is belangrijk te begrijpen dat dit niet is over het uitvoeren van SAP HANA op Azure VMs, maar in een hybride omgeving waar de SAP-app servers in Azure VMs maar SAP HANA uitvoeren wordt uitgevoerd op servers kaal: [SAP notitie 2316233](https://launchpad.support.sap.com/#/notes/2316233/E)

* Opmerking met informatie over SAPOSCOL op Linux SAP: [SAP notitie 1102124](https://launchpad.support.sap.com/#/notes/1102124/E)

* Belangrijke maatstaven voor SAP op Microsoft Azure Monitoring: [SAP notitie 2178632](https://launchpad.support.sap.com/#/notes/2178632/E)

* Informatie over Azure Resource Manager: [Azure Resource Manager-overzicht](../azure-resource-manager/resource-group-overview.md)

* Informatie over de implementatie van Linux VMs via sjablonen: [implementeren en beheren van virtuele machines met behulp van bronbeheer Azure sjablonen en de CLI Azure](virtual-machines-linux-cli-deploy-templates.md)

* Vergelijking van de implementatie van tussen Azure Resource Manager en klassieke modellen: [Azure Resource Manager versus klassiek implementatie: implementatiemodellen en de status van uw resources begrijpen](../resource-manager-deployment-model.md)

* NetWeaver 7.5 voor Linux/HANA uit SAP Service Marketplace downloaden:![](./media/virtual-machines-linux-sap-hana-get-started/image001.jpg)

* HANA SP12 Platform editie van SAP Service Marketplace downloaden:![](./media/virtual-machines-linux-sap-hana-get-started/image002.jpg)

