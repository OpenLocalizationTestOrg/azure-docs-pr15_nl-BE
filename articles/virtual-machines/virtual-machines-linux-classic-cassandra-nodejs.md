<properties
    pageTitle="Cassandra uitvoeren met Linux op Azure | Microsoft Azure"
    description="Het uitvoeren van een cluster van Cassandra op Linux in Azure virtuele Machines vanuit een app Node.js"
    services="virtual-machines-linux"
    documentationCenter="nodejs"
    authors="hanuk"
    manager="wpickett"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="hanuk;robmcm"/>

# <a name="running-cassandra-with-linux-on-azure-and-accessing-it-from-nodejs"></a>Met Cassandra en Linux op Azure en benaderen van Node.js

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Meer informatie over hoe u [deze stappen uitvoert met behulp van het model Resource Manager](https://azure.microsoft.com/documentation/templates/datastax-on-ubuntu/).

## <a name="overview"></a>Overzicht
Microsoft Azure is een open cloud-platform die zowel Microsoft als besturingssystemen, toepassingsservers, messaging middleware, evenals databases SQL en NoSQL uit zowel commerciële en open source-modellen bevat ook als niet-Microsoft-software wordt uitgevoerd. Opzetten van robuuste services op openbare wolken, met inbegrip van Azure vereist zorgvuldige planning en architectuur van doelbewuste voor beide toepassingsservers als lagen en opslag. Cassandra van opslag voor gedistribueerde architectuur kunt u natuurlijk bij het bouwen van uitermate beschikbare systemen die voor clusterstoringen fouttolerant zijn. Cassandra is een wolk schaal NoSQL database die wordt onderhouden door de Apache Software Foundation op cassandra.apache.org; Cassandra is geschreven in Java en dus worden uitgevoerd op beide op zowel Windows als Linux platforms.

De focus van dit artikel is Cassandra implementatie op Ubuntu weergeven als een enkele en meerdere Datacenter cluster gebruik te maken van Microsoft Azure virtuele Machines en virtuele netwerken. De implementatie van het cluster voor geoptimaliseerde productie werkbelasting is buiten het bestek van dit artikel vereist configuratie schijf met meerdere knooppunten, passende ring topologie ontwerpen en gegevensmodellering ter ondersteuning van de benodigde replicatie, consistentie van de gegevens, doorvoer en hoge beschikbaarheid eisen.

In dit artikel wordt een fundamentele aanpak om weer te geven van wat is betrokken bij het bouwen van het cluster Cassandra vergeleken Docker, chef-kok of Puppet waardoor de infrastructuur veel eenvoudiger.  

## <a name="the-deployment-models"></a>De implementatiemodellen
Microsoft Azure-netwerk kunt de implementatie van geïsoleerde particuliere clusters, de toegang beperkt worden kan tot de beveiliging van het netwerk probleemloos korrelig bereiken.  Omdat dit artikel over het weergeven van de implementatie van Cassandra op een fundamenteel niveau, we niet gericht op het niveau van de consistentie en het opslagontwerp van de optimale voor doorvoer. Hier is de lijst met vereisten voor onze hypothetische cluster toegang:

- Externe systemen geen toegang tot database van de Cassandra van binnen of buiten de Azure
- Cassandra cluster heeft zijn achter een load balancer voor thrift verkeer
- Cassandra knooppunten in twee groepen in elk Datacenter voor de clusterbeschikbaarheid van een verbeterde implementeren
- Vergrendelt het cluster zo dat alleen application server-farm toegang tot de database rechtstreeks heeft
- Geen openbare netwerken eindpunten dan SSH
- Elk knooppunt Cassandra moet een vaste interne IP-adres

Cassandra kan worden geïmplementeerd naar één gebied Azure of meerdere regio's op basis van de gedistribueerde aard van de werkbelasting. Implementatiemodel in meerdere landen/regio kan worden gebruikt voor eindgebruikers dichter bij een bepaalde geografische via dezelfde Cassandra-infrastructuur. Cassandra van ingebouwde knooppunt replicatie zorgt voor de synchronisatie van meerdere model schrijft afkomstig zijn uit meerdere datacenters en biedt een consistente weergave van de gegevens naar toepassingen. Implementatie van meerdere regio kan ook helpen met het beperken van de risico's van de bredere Azure storingen. Instelbare consistentie van Cassandra en de replicatietopologie helpt in de behoeften van diverse vrijgegeven Productieorder van toepassingen.

### <a name="single-region-deployment"></a>Implementatie van één regio
We starten met de implementatie van een bepaalde regio en de geleerde lessen bij het maken van een model met meerdere regio harvest. Azure virtuele netwerken wordt gebruikt voor het geïsoleerde subnetten maken zodat de bovengenoemde vereisten voor beveiliging kunnen worden voldaan.  Het proces dat wordt beschreven in het maken van de implementatie van één regio gebruikt Ubuntu 14.04 LTS en Cassandra 2.08; het proces kan gemakkelijk worden vastgesteld met de andere varianten van Linux. Hier volgen enkele van de systemische kenmerken van de implementatie van één regio.  

**Hoge beschikbaarheid:** Cassandra knooppunten weergegeven in figuur 1 zijn geïmplementeerd op twee sets van beschikbaarheid zodat de knooppunten worden verdeeld tussen meerdere domeinen voor fouttolerantie voor hoge beschikbaarheid. VMs, waarbij elke set beschikbaarheid van aantekeningen voorzien met 2 fout met betrekking tot domeinen is toegewezen.  Microsoft Azure wordt het begrip fouttolerantie domein beheren ongeplande productieonderbreking (bv. hardware of software fouten), terwijl het concept van upgrade-domein (bijvoorbeeld host of Gast OS patches/upgrades, toepassingsupgrades) wordt gebruikt voor het beheren van geplande uitvaltijd. Zie [noodherstel en hoge beschikbaarheid voor Azure toepassingen](http://msdn.microsoft.com/library/dn251004.aspx) voor de rol van fouttolerantie en upgraden van domeinen in het bereiken van hoge beschikbaarheid.

![Implementatie van één regio](./media/virtual-machines-linux-classic-cassandra-nodejs/cassandra-linux1.png)

Figuur 1: Één gebied implementeren

Houd er rekening mee dat op het moment van schrijven van dit Azure niet toestaan dat de expliciete toewijzing van een groep van VMs tot een fout met betrekking tot specifieke domein. zelfs met het implementatiemodel is afgebeeld in figuur 1, is het daarom statistisch gezien waarschijnlijk is dat de virtuele machines kunnen worden toegewezen aan twee fout met betrekking tot domeinen in plaats van vier.

**Taakverdeling Thrift verkeer:** Thrift clientbibliotheken binnen de server verbinding maken met het cluster via een interne taakverdeling. Hiervoor moet het proces van de interne taakverdeling toe te voegen aan het subnet 'data' (Zie figuur 1) in het kader van de cloud-service die als host fungeert voor het cluster Cassandra. Zodra de interne taakverdeling is gedefinieerd, moet elk knooppunt het eindpunt taakverdeling met de aantekeningen van een taakverdeling instellen met vooraf gedefinieerde load balancer naam worden toegevoegd. Zie [Azure interne taakverdeling ](../load-balancer/load-balancer-internal-overview.md)voor meer informatie.

**Cluster zaden:** Het is belangrijk dat de meeste maximaal beschikbare knooppunten voor zaden selecteren als nieuwe knooppunten met zaad knooppunten communiceren te ontdekken van de topologie van het cluster. Een knooppunt uit elke set beschikbaarheid wordt aangewezen als zaad knooppunten om te voorkomen dat potentieel risico.

**Replicatie Factor en consistent niveau:** Cassandra van build in hoge beschikbaarheid en duurzaamheid wordt gekenmerkt door de replicatie Factor (RF - aantal exemplaren van elke rij in het cluster worden opgeslagen) en de consistentie (aantal replica's worden gelezen of weggeschreven voordat het resultaat wordt geretourneerd aan de oproeper). Replicatie factor is opgegeven tijdens het maken van de KEYSPACE (vergelijkbaar met een relationele database) dat het niveau van de consistentie is opgegeven tijdens het verzenden van de query CRUD. Zie de documentatie van de Cassandra bij [configureren op consistentie](http://www.datastax.com/documentation/cassandra/2.0/cassandra/dml/dml_config_consistency_c.html) voor meer informatie de consistentie en de formule voor het berekenen van de quorumbron.

Cassandra ondersteunt twee soorten integriteit gegevensmodellen – consistentie en uiteindelijke algehele consistentie; de replicatie Factor en de consistentie wordt samen bepalen of de gegevens consistent als een schrijfbewerking voltooid is of uiteindelijk consistent worden. Bijvoorbeeld QUORUM opgeven als het niveau van de consistentie zal altijd zorgt voor consistentie van gegevens terwijl u elk niveau van de consistentie, onder het aantal replica's moet worden geschreven wanneer dat nodig is voor de verwezenlijking van QUORUM (bv. een) gegevens uiteindelijk consistente resultaten.

De bovenstaande, met een factor 3 en het QUORUM van replicatie cluster met 8 knooppunten (2 knooppunten worden gelezen of geschreven voor consistentie) lezen niveau consistentie, de theoretische verlies van ten hoogste 1 knooppunt per replicatiegroep vóór de start van de toepassing daarvan de storing kunnen overleven. Hierbij wordt ervan uitgegaan dat alle hebben ook de belangrijkste ruimten in balans lezen/schrijven aanvragen.  Dit zijn de parameters die we voor het cluster geïmplementeerd gebruiken:

Configuratie van één regio Cassandra het cluster:

| De Parameter cluster | Waarde | Opmerkingen |
| ----------------- | ----- | ------- |
| Aantal knooppunten (N) | 8   | Totaal aantal knooppunten in het cluster |
| Replicatie Factor (RF) | 3 | Aantal replica's van een bepaalde rij |
| Consistentie-niveau (schrijven) | QUORUM[(RF/2) +1) = 2] het resultaat van de formule naar beneden afgerond | Ten hoogste 2 replica's worden geschreven voordat het antwoord wordt verzonden aan de aanvrager; 3e replica is geschreven in uiteindelijk consequent. |
| Consistentie-niveau (lezen) | QUORUM [(RF/2) + 1 = 2] het resultaat van de formule naar beneden afgerond | Leest 2 replica's moet worden verzonden naar de aanroepende functie. |
| Replicatiestrategie voor | NetworkTopologyStrategy Zie [Gegevensreplicatie](http://www.datastax.com/documentation/cassandra/2.0/cassandra/architecture/architectureDataDistributeReplication_c.html) in Cassandra-documentatie voor meer informatie | De topologie van de implementatie begrijpt en replica's op knooppunten plaatst, zodat de replica's niet op hetzelfde rack terecht |
| Snitch | GossipingPropertyFileSnitch Zie [Snitches](http://www.datastax.com/documentation/cassandra/2.0/cassandra/architecture/architectureSnitchesAbout_c.html) in Cassandra-documentatie voor meer informatie | Een concept van snitch NetworkTopologyStrategy gebruikt om te begrijpen van de topologie. GossipingPropertyFileSnitch geeft een betere controle in elk knooppunt toe te wijzen aan datacenter en rek. Het cluster wordt roddels doorgeven van deze informatie. Dit is veel eenvoudiger in dynamische IP-instelling ten opzichte van PropertyFileSnitch |


**Azure overwegingen voor Cassandra Cluster:** De mogelijkheid Microsoft Azure virtuele Machines gebruikt Azure Blob-opslag voor persistentie van de schijf; Azure opslag bespaart 3 replica's van elke schijf voor hoge duurzaamheid. Dit betekent dat elke rij met gegevens invoegen in een tabel Cassandra al in 3 replica's zijn opgeslagen en vandaar consistentie van de gegevens is al afgehandeld als replicatie Factor (RF) 1. Het belangrijkste probleem met replicatie Factor 1 is dat de toepassing uitvaltijd treedt zelfs als één Cassandra-knooppunt is mislukt. Als een knooppunt niet actief voor de problemen (bijvoorbeeld hardware, software systeemfouten is) herkend door Azure Fabric-Controller, wordt er een nieuw knooppunt in plaats daarvan met de dezelfde opslagstations inrichten. Inrichten van een nieuw knooppunt ter vervanging van de oude kan enkele minuten duren.  Op dezelfde manier voor gepland onderhoudsactiviteiten als gast OS wijzigingen, Cassandra upgrades en wijzigingen in de toepassing Azure Fabric Controller voert rolling upgrades van de knooppunten in het cluster.  Rolling upgrades kan ook omlaag een paar knooppunten tegelijk en dus korte uitval van een aantal partities kan optreden in het cluster. Echter, de gegevens worden niet verloren gaat als gevolg van de ingebouwde redundantie voor de opslag van Azure.  

Voor systemen geïmplementeerd op Azure die niet hoge beschikbaarheid (bv. rond 99,9 die gelijk is aan 8.76 uur per jaar vereist; [Maximale beschikbaarheid](http://en.wikipedia.org/wiki/High_availability) voor meer informatie Zie) kunt u mogelijk uitvoeren met RF = 1 en consistent niveau = een.  Voor toepassingen met hoge beschikbaarheid eisen, RF = 3 en consistent niveau = QUORUM geplaatst en de tijd van een van de knooppunten van de replica's. RF = 1 in traditionele implementaties (bv. lokale) kunnen niet worden gebruikt vanwege de mogelijke gegevens verliest als gevolg van problemen zoals schijffouten.   

## <a name="multi-region-deployment"></a>Implementatie in meerdere landen/regio
Cassandra van data center hoogte replicatie en consistent model helpt bij de implementatie van meerdere regio uit de doos zonder de noodzaak van een externe tooling hierboven. Dit is heel anders dan de traditionele relationele databases waarbij de instellingen voor database-mirroring voor meerdere masters schrijft er ingewikkeld kan zijn. Cassandra in een multi-regio instellen kunt met de gebruiksscenario's waaronder de volgende:

**Implementatie op basis van nabijheid:** Huurder met meerdere toepassingen, met duidelijk overzicht van de gebruikers van de huurder-naar-regio, door de lage vertragingstijden van het cluster met meerdere regio kan worden geprofiteerd. Bijvoorbeeld een learning management systemen voor educatieve instellingen in Oost-Verenigde Staten en West-Amerikaanse regio's dienen de respectieve campussen voor een gedistribueerde-cluster kunnen implementeren transactionele en analytics. De gegevens kan lokaal consistent kunnen zijn op de tijd lees- en schrijfbewerkingen en uiteindelijk overeenstemming tussen de beide regio's. Er zijn andere voorbeelden, zoals media-distributie, e-commerce en iets en alles dat geo geconcentreerde gebruiker fungeert als basis een goede use-case voor dit implementatiemodel is.

**Hoge beschikbaarheid:** Redundantie is een belangrijke factor bij het bereiken van hoge beschikbaarheid van software en hardware; Gebouw betrouwbare Cloud systemen op Microsoft Azure Zie voor meer informatie. Op Microsoft Azure is het alleen betrouwbare manier waar redundantie te bereiken door het implementeren van een cluster met meerdere regio. Toepassingen kunnen worden geïmplementeerd in een actieve of actieve-passieve modus en als een van de regio's naar beneden, Manager van Azure verkeer verkeer kunt omleiden naar het actieve gebied.  Bij de implementatie van één gebied, als de beschikbaarheid van 99,9, is een implementatie van de twee regio kan bereiken een beschikbaarheid van 99.9999, berekend door de formule: (1-(1-0.999) *(1-0,999))*100); Raadpleeg het artikel hierboven voor meer informatie.

**Noodherstel:** Cluster met meerdere regio Cassandra, als goed ontworpen, bestand is tegen storingen van catastrofale data center. Als één regio niet actief is, kan de toepassing gedistribueerd naar andere regio's kunt starten voor de eindgebruikers. De toepassing heeft net als alle andere business continuity implementaties, worden sommige gegevens verloren zijn gegaan ten gevolge van de gegevens in de asynchrone pijpleiding fouttolerante. Cassandra is echter het herstel veel sneller dan de tijd die door traditionele database herstel processen. Figuur 2 toont de typische meerdere regio implementatiemodel met acht knooppunten in elke regio. Beide gebieden zijn spiegelbeeld van elkaar voor dezelfde hoogte; ontwerpen van de echte wereld zijn afhankelijk van het type werklast (bv. transactioneel of analytische), vrijgegeven Productieorder, RTO, consistentie van de gegevens en de vereiste beschikbaarheid.

![Implementatie van multi-regio](./media/virtual-machines-linux-classic-cassandra-nodejs/cassandra-linux2.png)

Afbeelding 2: Meerdere regio Cassandra implementatie

### <a name="network-integration"></a>Netwerkintegratie
Sets van virtuele machines is geïmplementeerd op particuliere netwerken zich op twee gebieden communiceert met elkaar met behulp van een VPN-tunnel. De VPN-tunnel verbindt twee software-gateways tijdens de implementatie van netwerk ingericht. Beide gebieden hebben soortgelijke netwerkarchitectuur voor 'web' en 'data' subnetten; Azure netwerken, het maken van zo veel subnetten zo nodig en ACL's van toepassing als die nodig is voor de beveiliging van het netwerk. Tijdens het ontwerpen de topologie cluster onder data center communicatie latentie en de economische gevolgen van het netwerkverkeer moeten rekening worden gehouden.

### <a name="data-consistency-for-multi-data-center-deployment"></a>Consistentie van de gegevens voor de implementatie van meerdere Data Center
Gedistribueerde implementaties moeten zich bewust zijn van de impact van de topologie cluster op de doorvoer en hoge beschikbaarheid. De RF en de consistentie moet worden geselecteerd, op zodanige wijze dat het quorum niet afhankelijk van de beschikbaarheid van de datacenters.
Voor een systeem dat hoge consistentie nodig, zorgt een LOCAL_QUORUM voor consistentie niveau (voor lees- en schrijfbewerkingen) ervoor dat de lokale lees- en schrijfbewerkingen wordt voldaan uit de lokale knooppunten terwijl gegevens asynchroon worden gerepliceerd naar de externe datacenters.  Tabel 2 geeft een overzicht van de configuratiegegevens voor het gebied met meerdere cluster later in het schrijven van beschreven.

**Configuratie van het cluster twee regio Cassandra**


| De Parameter cluster | Waarde | Opmerkingen |
| ----------------- | ----- | ------- |
| Aantal knooppunten (N) | 8 + 8 | Totaal aantal knooppunten in het cluster |
| Replicatie Factor (RF) | 3 | Aantal replica's van een bepaalde rij |
| Consistentie-niveau (schrijven) | LOCAL_QUORUM [(sum(RF)/2) +1) = 4] het resultaat van de formule naar beneden afgerond | 2 knooppunten worden geschreven naar het eerste Datacenter synchroon; de extra 2 knooppunten die nodig zijn voor het quorum worden asynchroon naar de 2e Datacenter geschreven. |
| Consistentie-niveau (lezen) | LOCAL_QUORUM ((RF/2) + 1) = 2 is het resultaat van de formule naar beneden afgerond | Alleen aanvragen wordt voldaan uit slechts één gebied; 2 knooppunten worden gelezen voordat het antwoord naar de client wordt verzonden. |
| Replicatiestrategie voor | NetworkTopologyStrategy Zie [Gegevensreplicatie](http://www.datastax.com/documentation/cassandra/2.0/cassandra/architecture/architectureDataDistributeReplication_c.html) in Cassandra-documentatie voor meer informatie | De topologie van de implementatie begrijpt en replica's op knooppunten plaatst, zodat de replica's niet op hetzelfde rack terecht |
| Snitch | GossipingPropertyFileSnitch Zie [Snitches](http://www.datastax.com/documentation/cassandra/2.0/cassandra/architecture/architectureSnitchesAbout_c.html) in Cassandra-documentatie voor meer informatie | Een concept van snitch NetworkTopologyStrategy gebruikt om te begrijpen van de topologie. GossipingPropertyFileSnitch geeft een betere controle in elk knooppunt toe te wijzen aan datacenter en rek. Het cluster wordt roddels doorgeven van deze informatie. Dit is veel eenvoudiger in dynamische IP-instelling ten opzichte van PropertyFileSnitch |


##<a name="the-software-configuration"></a>DE SOFTWARECONFIGURATIE
De volgende versies van software worden gebruikt tijdens de implementatie:

<table>
<tr><th>Software</th><th>Bron</th><th>Versie</th></tr>
<tr><td>JAVA RUNTIME ENVIRONMENT </td><td>[JRE 8](http://www.oracle.com/technetwork/java/javase/downloads/server-jre8-downloads-2133154.html) </td><td>8U5</td></tr>
<tr><td>JNA </td><td>[JNA](https://github.com/twall/jna) </td><td> 3.2.7</td></tr>
<tr><td>Cassandra</td><td>[Apache-Cassandra 2.0.8](http://www.apache.org/dist/cassandra/2.0.8/apache-cassandra-2.0.8-bin.tar.gz)</td><td> 2.0.8</td></tr>
<tr><td>Ubuntu  </td><td>[Microsoft Azure](https://azure.microsoft.com/) </td><td>14.04 LTS</td></tr>
</table>

Aangezien het downloaden van de Java Runtime Environment handmatige aanvaarding van Oracle licentie vereist, ter vereenvoudiging van de distributie, de vereiste software naar uw bureaublad gedownload voor het later uploaden naar de Ubuntu Sjabloonafbeelding die we als een precursor voor de implementatie van het cluster maakt.

De bovenstaande software downloaden naar een bekende downloadmap (bv. %TEMP%/downloads op Windows of ~/Downloads op de meeste distributies van Linux of Mac) op de lokale computer.

### <a name="create-ubuntu-vm"></a>UBUNTU VM MAKEN
In deze stap van het proces zal maken we Ubuntu afbeelding met de vooraf de nodige software zodat de afbeelding opnieuw kan worden gebruikt voor het inrichten van verschillende Cassandra knooppunten.  
####<a name="step-1-generate-ssh-key-pair"></a>STAP 1: SSH-sleutelpaar genereren
Azure moet een openbare sleutel die is PEM of DER-gecodeerd tijdens het inrichtingsproces X509. Een door de aanwijzingen te vinden op het gebruik SSH met Linux op Azure openbaar/persoonlijk sleutelpaar genereren. Als u van plan bent te gebruiken putty.exe als een SSH-client op Windows of Linux, hebt u converteren de PEM gecodeerde persoonlijke sleutel van RSA PPK-indeling met behulp van puttygen.exe; de instructies hiervoor vindt u in de bovenstaande webpagina.

####<a name="step-2-create-ubuntu-template-vm"></a>STAP 2: Maak een sjabloon Ubuntu VM
Als u de sjabloon VM, log in op de klassieke Azure portal en de volgende procedure gebruiken: klik op Nieuw, COMPUTE, virtuele MACHINE, FROM GALERIE, UBUNTU, Ubuntu Server 14.04 LTS en klik vervolgens op de pijl-rechts. Zie voor een zelfstudie met instructies voor het maken van een Linux VM, maken een Linux virtuele Machine wordt uitgevoerd.

Voer de volgende gegevens op het scherm 'configuratie van de virtuele machine' #1:

<table>
<tr><th>VELDNAAM              </td><td>       DE WAARDE VAN VELD               </td><td>         OPMERKINGEN                </td><tr>
<tr><td>VERSIE RELEASEDATUM    </td><td> Selecteer een datum uit de drow omlaag</td><td></td><tr>
<tr><td>NAAM VAN DE VIRTUELE MACHINE    </td><td> Cass-sjabloon                </td><td> Dit is de hostnaam van de VM </td><tr>
<tr><td>LAAG                     </td><td> STANDAARD                        </td><td> Laat de standaardinstelling              </td><tr>
<tr><td>GROOTTE                     </td><td> A1                              </td><td>Selecteer de VM op basis van de i/o-behoeften; Laat de standaardinstelling voor dit doel. </td><tr>
<tr><td> NIEUWE GEBRUIKERSNAAM           </td><td> localadmin                      </td><td> 'admin' is een gereserveerde naam in Ubuntu 12. xx en na</td><tr>
<tr><td> VERIFICATIE      </td><td> Klik op selectievakje                 </td><td>Als u wilt beveiligen met een SSH-sleutel controleren </td><tr>
<tr><td> CERTIFICAAT             </td><td> de naam van het certificaat met openbare sleutel </td><td> Gebruik de openbare sleutel gegenereerd eerder</td><tr>
<tr><td> Nieuw wachtwoord   </td><td> sterk wachtwoord </td><td> </td><tr>
<tr><td> Bevestig wachtwoord   </td><td> sterk wachtwoord </td><td></td><tr>
</table>

Voer de volgende gegevens op het scherm 'configuratie van de virtuele machine' #2:

<table>
<tr><th>VELDNAAM             </th><th> DE WAARDE VAN VELD                       </th><th> OPMERKINGEN                                 </th></tr>
<tr><td> CLOUD-SERVICE  </td><td> Maak een nieuwe wolk service    </td><td>Cloud-service is een container compute-bronnen zoals virtuele machines</td></tr>
<tr><td> DE NAAM VAN DE DNS-SERVICE CLOUD </td><td>Ubuntu-template.cloudapp.net   </td><td>Geef een machinenaam agnostische load balancer</td></tr>
<tr><td> REGIO/AFFINITEIT GROEP/VIRTUEEL NETWERK </td><td>    West-Verenigde Staten </td><td> Selecteer een regio waarvan de toegang tot het cluster Cassandra van uw webtoepassingen</td></tr>
<tr><td>OPSLAG ACCOUNT </td><td>   Standaard gebruiken </td><td>Gebruik de standaard opslag-account of een account met vooraf gemaakte opslag in een bepaald gebied</td></tr>
<tr><td>BESCHIKBAARHEID INSTELLEN </td><td>  Geen </td><td>  Laat leeg</td></tr>
<tr><td>EINDPUNTEN   </td><td>Standaard gebruiken </td><td>  Gebruik de standaardconfiguratie van SSH </td></tr>
</table>

Klik op de pijl-rechts, laat u de standaardinstellingen op het scherm #3 en klik op de knop 'controleren' om het inrichtingsproces VM. Na een paar minuten moet de VM met de naam "ubuntu-sjabloon' status 'uitvoeren'.

###<a name="install-the-necessary-software"></a>DE BENODIGDE SOFTWARE INSTALLEREN
####<a name="step-1-upload-tarballs"></a>STAP 1: Upload tarballs
Met scp of pscp, de eerder gedownloade software kopiëren naar ~/downloads directory met de volgende syntaxis:

#####<a name="pscp-server-jre-8u5-linux-x64targz-localadminhk-cas-templatecloudappnethomelocaladmindownloadsserver-jre-8u5-linux-x64targz"></a>pscp server-jre-8u5-linux-x64.tar.gzlocaladmin@hk-cas-template.cloudapp.net:/home/localadmin/downloads/server-jre-8u5-linux-x64.tar.gz

Herhaal de bovenstaande opdracht voor JRE alsmede voor de bits Cassandra.

####<a name="step-2-prepare-the-directory-structure-and-extract-the-archives"></a>STAP 2: Voorbereiden van de mapstructuur en de archieven uitpakken
Log in op de VM en maak de mapstructuur en uitpakken van software als een super-gebruiker met behulp van de onderstaande bash script:

    #!/bin/bash
    CASS_INSTALL_DIR="/opt/cassandra"
    JRE_INSTALL_DIR="/opt/java"
    CASS_DATA_DIR="/var/lib/cassandra"
    CASS_LOG_DIR="/var/log/cassandra"
    DOWNLOADS_DIR="~/downloads"
    JRE_TARBALL="server-jre-8u5-linux-x64.tar.gz"
    CASS_TARBALL="apache-cassandra-2.0.8-bin.tar.gz"
    SVC_USER="localadmin"

    RESET_ERROR=1
    MKDIR_ERROR=2

    reset_installation ()
    {
       rm -rf $CASS_INSTALL_DIR 2> /dev/null
       rm -rf $JRE_INSTALL_DIR 2> /dev/null
       rm -rf $CASS_DATA_DIR 2> /dev/null
       rm -rf $CASS_LOG_DIR 2> /dev/null
    }
    make_dir ()
    {
       if [ -z "$1" ]
       then
          echo "make_dir: invalid directory name"
          exit $MKDIR_ERROR
       fi

       if [ -d "$1" ]
       then
          echo "make_dir: directory already exists"
          exit $MKDIR_ERROR
       fi

       mkdir $1 2>/dev/null
       if [ $? != 0 ]
       then
          echo "directory creation failed"
          exit $MKDIR_ERROR
       fi
    }

    unzip()
    {
       if [ $# == 2 ]
       then
          tar xzf $1 -C $2
       else
          echo "archive error"
       fi

    }

    if [ -n "$1" ]
    then
       SVC_USER=$1
    fi

    reset_installation
    make_dir $CASS_INSTALL_DIR
    make_dir $JRE_INSTALL_DIR
    make_dir $CASS_DATA_DIR
    make_dir $CASS_LOG_DIR

    #unzip JRE and Cassandra
    unzip $HOME/downloads/$JRE_TARBALL $JRE_INSTALL_DIR
    unzip $HOME/downloads/$CASS_TARBALL $CASS_INSTALL_DIR

    #Change the ownership to the service credentials

    chown -R $SVC_USER:$GROUP $CASS_DATA_DIR
    chown -R $SVC_USER:$GROUP $CASS_LOG_DIR
    echo "edit /etc/profile to add JRE to the PATH"
    echo "installation is complete"


Als u dit script in vim-venster plakken, zorg ervoor dat u van de regelterugloop ('\r ') met de volgende opdracht:

    tr -d '\r' <infile.sh >outfile.sh

####<a name="step-3-edit-etcprofile"></a>Stap 3: Etc/profiel bewerken
Het volgende aan het eind toevoegen:

    JAVA_HOME=/opt/java/jdk1.8.0_05
    CASS_HOME= /opt/cassandra/apache-cassandra-2.0.8
    PATH=$PATH:$HOME/bin:$JAVA_HOME/bin:$CASS_HOME/bin
    export JAVA_HOME
    export CASS_HOME
    export PATH

####<a name="step-4-install-jna-for-production-systems"></a>Stap 4: Installatie JNA voor productiesystemen
De opdrachtvolgorde van de volgende gebruiken: de volgende opdracht installeren jna 3.2.7.jar en jna platform 3.2.7.jar naar de map /usr/share.java sudo apt-get libjna-java installeren

Symbolische koppelingen in $CASS_HOME/lib directory maken zodat het opstartscript Cassandra deze potten kunt vinden:

    ln -s /usr/share/java/jna-3.2.7.jar $CASS_HOME/lib/jna.jar

    ln -s /usr/share/java/jna-platform-3.2.7.jar $CASS_HOME/lib/jna-platform.jar

####<a name="step-5-configure-cassandrayaml"></a>Stap 5: Cassandra.yaml configureren
Cassandra.yaml op elke VM aan configuratie nodig voor alle virtuele machines [we zullen dit aanpassen tijdens het werkelijke inrichten] bewerken:

<table>
<tr><th>Veldnaam   </th><th> Waarde  </th><th> Opmerkingen </th></tr>
<tr><td>clusternaam </td><td>  "CustomerService"   </td><td> Gebruik de naam die overeenkomt met de implementatie</td></tr>
<tr><td>listen_address  </td><td>[leeg laten]   </td><td> "Localhost" verwijderen </td></tr>
<tr><td>rpc_addres   </td><td>[leeg laten]  </td><td> "Localhost" verwijderen </td></tr>
<tr><td>zaden   </td><td>"10.1.2.4, 10.1.2.6, 10.1.2.8" </td><td>Lijst met alle IP-adressen die zijn aangewezen als zaden.</td></tr>
<tr><td>endpoint_snitch </td><td> org.apache.cassandra.locator.GossipingPropertyFileSnitch </td><td> Dit wordt gebruikt door de NetworkTopologyStrateg voor het afleiden van het gegevenscentrum en de rek van de VM</td></tr>
</table>

####<a name="step-6-capture-the-vm-image"></a>Stap 6: De VM-installatiekopie vastleggen
Log in op de virtuele machine met behulp van de hostnaam (hk-cas-template.cloudapp.net) en de persoonlijke sleutel van SSH eerder hebt gemaakt. Zie hoe gebruik SSH met Linux op Azure voor meer informatie over hoe u zich aanmeldt met behulp van de opdracht ssh of putty.exe.

Voer de volgende reeks van acties voor het vastleggen van de afbeelding:
#####<a name="1-deprovision"></a>1. de identiteitsgegevens
Gebruik de opdracht ' sudo waagent – identiteitsgegevens + user "virtuele Machine instantie bepaalde gegevens te verwijderen. Zie voor het [vastleggen van een virtuele Linux Machine](virtual-machines-linux-classic-capture-image.md) te gebruiken als een sjabloon meer details op de installatiekopie vastleggen.

#####<a name="2-shutdown-the-vm"></a>2: de VM afsluiten
Zorg ervoor dat de virtuele machine is geselecteerd en klik op de koppeling van het afsluiten van de opdrachtbalk onder.

#####<a name="3-capture-the-image"></a>3: de installatiekopie vastleggen
Zorg ervoor dat de virtuele machine is geselecteerd en klik op de koppeling van het VASTLEGGEN van de opdrachtbalk onder. In het volgende scherm geeft de naam van een afbeelding (bijvoorbeeld hk-cas-2-08-ub-14-04-2014071), passende beschrijving van afbeelding en klikt u op het ' vinkje ' om het proces van VASTLEGGEN.

Dit duurt een paar seconden en de afbeelding in het gedeelte van de afbeeldinggalerie Mijn afbeeldingen beschikbaar moet zijn. De bron VM, worden automatisch delated nadat de afbeelding correct wordt vastgelegd.

##<a name="single-region-deployment-process"></a>Het implementatieproces van één regio
**Stap 1: Maak het virtuele netwerk** Log in op de klassieke Azure portal en een virtueel netwerk maken met de kenmerken weergeven in de tabel. Zie [een virtuele netwerk Cloud-Only in de klassieke Azure portal configureren](../virtual-network/virtual-networks-create-vnet-classic-portal.md) voor gedetailleerde stappen van het proces.      

<table>
<tr><th>Kenmerknaam-VM</th><th>Waarde</th><th>Opmerkingen</th></tr>
<tr><td>Naam</td><td>vnet-cass-west-ons</td><td></td></tr>
<tr><td>Regio</td><td>West-Verenigde Staten</td><td></td></tr>
<tr><td>DNS-Servers </td><td>Geen</td><td>Dit negeren als wij niet met behulp van een DNS-Server</td></tr>
<tr><td>Een point-to-site VPN configureren</td><td>Geen</td><td> Dit negeren</td></tr>
<tr><td>Configureren van een VPN website</td><td>Nnone</td><td> Dit negeren</td></tr>
<tr><td>Adresruimte</td><td>10.1.0.0/16</td><td></td></tr>
<tr><td>IP starten</td><td>10.1.0.0</td><td></td></tr>
<tr><td>CIDR </td><td>/ 16 (65531)</td><td></td></tr>
</table>

Voeg de volgende subnetten:

<table>
<tr><th>Naam</th><th>IP starten</th><th>CIDR</th><th>Opmerkingen</th></tr>
<tr><td>Web</td><td>10.1.1.0</td><td>/ 24 (251)</td><td>Subnet voor de web-farm</td></tr>
<tr><td>gegevens</td><td>10.1.2.0</td><td>/ 24 (251)</td><td>Subnet voor de databaseknooppunten</td></tr>
</table>

Gegevens en Web subnetten worden beveiligd via netwerk-beveiligingsgroepen de dekking van die buiten het bereik van dit artikel valt.  

**Stap 2: inrichten van virtuele Machines** Met behulp van de afbeelding hebt gemaakt, we maken de volgende virtuele machines in de cloud server "hk-c-svc-west" en om de respectieve subnetten te binden, zoals hieronder wordt weergegeven:

<table>
<tr><th>Naam van de computer    </th><th>Subnet </th><th>IP-adres </th><th>Beschikbaarheid instellen</th><th>DC/rek</th><th>Zaad?</th></tr>
<tr><td>HK-c1-west-ons   </td><td>gegevens   </td><td>10.1.2.4   </td><td>HK-c-uit-1    </td><td>DC = rack WESTUS = rack1 </td><td>Ja</td></tr>
<tr><td>HK-c2-west-ons   </td><td>gegevens   </td><td>10.1.2.5   </td><td>HK-c-uit-1    </td><td>DC = rack WESTUS = rack1 </td><td>Nee </td></tr>
<tr><td>HK-c3-west-ons   </td><td>gegevens   </td><td>10.1.2.6   </td><td>HK-c-uit-1    </td><td>DC = rack WESTUS = rack2 </td><td>Ja</td></tr>
<tr><td>HK-c4-west-ons   </td><td>gegevens   </td><td>10.1.2.7   </td><td>HK-c-uit-1    </td><td>DC = rack WESTUS = rack2 </td><td>Nee </td></tr>
<tr><td>HK-c5-west-ons   </td><td>gegevens   </td><td>10.1.2.8   </td><td>HK-c-uit-2    </td><td>DC = rack WESTUS = rack3 </td><td>Ja</td></tr>
<tr><td>HK-c6-west-ons   </td><td>gegevens   </td><td>10.1.2.9   </td><td>HK-c-uit-2    </td><td>DC = rack WESTUS = rack3 </td><td>Nee </td></tr>
<tr><td>HK-c7-west-ons   </td><td>gegevens   </td><td>10.1.2.10  </td><td>HK-c-uit-2    </td><td>DC = rack WESTUS = rack4 </td><td>Ja</td></tr>
<tr><td>HK-c8-west-ons   </td><td>gegevens   </td><td>10.1.2.11  </td><td>HK-c-uit-2    </td><td>DC = rack WESTUS = rack4 </td><td>Nee </td></tr>
<tr><td>HK-w1-west-ons   </td><td>Web    </td><td>10.1.1.4   </td><td>HK-w-uit-1    </td><td>                       </td><td>N.V.T.</td></tr>
<tr><td>HK-w2-west-ons   </td><td>Web    </td><td>10.1.1.5   </td><td>HK-w-uit-1    </td><td>                       </td><td>N.V.T.</td></tr>
</table>

Het maken van de bovenstaande lijst met VMs vereist het volgende proces:

1.  Maak een lege cloud-service in een bepaald gebied
2.  Een VM maken op basis van de eerder vastgelegde afbeelding en koppel deze aan het virtuele netwerk gemaakt; Herhaal deze stap voor alle VMs
3.  Een interne taakverdeling toevoegen aan de service cloud en koppel deze aan het subnet "gegevens"
4.  Voor elke VM eerder hebt gemaakt, voegt u een eindpunt taakverdeling voor thrift verkeer via een taakverdeling set aangesloten op de eerder gemaakte interne taakverdeling

Dit proces kan worden uitgevoerd met Azure klassieke portal; Gebruik een Windows-machine (gebruik een VM in Azure als u geen toegang tot een Windows-computer hebt), gebruikt de volgende PowerShell script automatisch alle 8 VMs inrichten.

**Lijst 1: PowerShell-script voor het inrichten van virtuele machines**

        #Tested with Azure Powershell - November 2014
        #This powershell script deployes a number of VMs from an existing image inside an Azure region
        #Import your Azure subscription into the current Powershell session before proceeding
        #The process: 1. create Azure Storage account, 2. create virtual network, 3.create the VM template, 2. crate a list of VMs from the template

        #fundamental variables - change these to reflect your subscription
        $country="us"; $region="west"; $vnetName = "your_vnet_name";$storageAccount="your_storage_account"
        $numVMs=8;$prefix = "hk-cass";$ilbIP="your_ilb_ip"
        $subscriptionName = "Azure_subscription_name";
        $vmSize="ExtraSmall"; $imageName="your_linux_image_name"
        $ilbName="ThriftInternalLB"; $thriftEndPoint="ThriftEndPoint"

        #generated variables
        $serviceName = "$prefix-svc-$region-$country"; $azureRegion = "$region $country"

        $vmNames = @()
        for ($i=0; $i -lt $numVMs; $i++)
        {
           $vmNames+=("$prefix-vm"+($i+1) + "-$region-$country" );
        }

        #select an Azure subscription already imported into Powershell session
        Select-AzureSubscription -SubscriptionName $subscriptionName -Current
        Set-AzureSubscription -SubscriptionName $subscriptionName -CurrentStorageAccountName $storageAccount

        #create an empty cloud service
        New-AzureService -ServiceName $serviceName -Label "hkcass$region" -Location $azureRegion
        Write-Host "Created $serviceName"

        $VMList= @()   # stores the list of azure vm configuration objects
        #create the list of VMs
        foreach($vmName in $vmNames)
        {
           $VMList += New-AzureVMConfig -Name $vmName -InstanceSize ExtraSmall -ImageName $imageName |
           Add-AzureProvisioningConfig -Linux -LinuxUser "localadmin" -Password "Local123" |
           Set-AzureSubnet "data"
        }

        New-AzureVM -ServiceName $serviceName -VNetName $vnetName -VMs $VMList

        #Create internal load balancer
        Add-AzureInternalLoadBalancer -ServiceName $serviceName -InternalLoadBalancerName $ilbName -SubnetName "data" -StaticVNetIPAddress "$ilbIP"
        Write-Host "Created $ilbName"
        #Add add the thrift endpoint to the internal load balancer for all the VMs
        foreach($vmName in $vmNames)
        {
            Get-AzureVM -ServiceName $serviceName -Name $vmName |
                Add-AzureEndpoint -Name $thriftEndPoint -LBSetName "ThriftLBSet" -Protocol tcp -LocalPort 9160 -PublicPort 9160 -ProbePort 9160 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 -InternalLoadBalancerName $ilbName |
                Update-AzureVM

            Write-Host "created $vmName"     
        }

**Stap 3: Cassandra configureren op elke VM**

Log in op de VM en als volgt te werk:

* $CASS_HOME/conf/cassandra-rackdc.properties om op te geven van het data center en rek eigenschappen bewerken:

       dc =EASTUS, rack =rack1

* Bewerken cassandra.yaml zaad knooppunten configureren zoals hieronder:

       Seeds: "10.1.2.4,10.1.2.6,10.1.2.8,10.1.2.10"

**Stap 4: Start de VMs en testen van het cluster**

Log in op een van de knooppunten (bijvoorbeeld hk-c1-west-us) en voer de volgende opdracht om de status van het cluster:

       nodetool –h 10.1.2.4 –p 7199 status

De weergave die vergelijkbaar zijn met die hieronder voor een cluster met 8 knooppunten worden weergegeven:

<table>
<tr><th>Status</th><th>Adres  </th><th>Belasting   </th><th>Tokens </th><th>Eigenaar van </th><th>Host-ID  </th><th>Rek</th></tr>
<tr><th>UN  </td><td>10.1.2.4   </td><td>87.81 KB   </td><td>256    </td><td>38.0%  </td><td>GUID (verwijderd)</td><td>rack1</td></tr>
<tr><th>UN  </td><td>10.1.2.5   </td><td>41.08 KB   </td><td>256    </td><td>68,9%  </td><td>GUID (verwijderd)</td><td>rack1</td></tr>
<tr><th>UN  </td><td>10.1.2.6   </td><td>55.29 KB   </td><td>256    </td><td>68.8%  </td><td>GUID (verwijderd)</td><td>rack2</td></tr>
<tr><th>UN  </td><td>10.1.2.7   </td><td>55.29 KB   </td><td>256    </td><td>68.8%  </td><td>GUID (verwijderd)</td><td>rack2</td></tr>
<tr><th>UN  </td><td>10.1.2.8   </td><td>55.29 KB   </td><td>256    </td><td>68.8%  </td><td>GUID (verwijderd)</td><td>rack3</td></tr>
<tr><th>UN  </td><td>10.1.2.9   </td><td>55.29 KB   </td><td>256    </td><td>68.8%  </td><td>GUID (verwijderd)</td><td>rack3</td></tr>
<tr><th>UN  </td><td>10.1.2.10  </td><td>55.29 KB   </td><td>256    </td><td>68.8%  </td><td>GUID (verwijderd)</td><td>rack4</td></tr>
<tr><th>UN  </td><td>10.1.2.11  </td><td>55.29 KB   </td><td>256    </td><td>68.8%  </td><td>GUID (verwijderd)</td><td>rack4</td></tr>
</table>

## <a name="test-the-single-region-cluster"></a>Het Cluster met één gebied testen
Gebruik de volgende stappen uit voor het testen van het cluster:

1.    Het IP-adres van de interne taakverdeling (bijvoorbeeld met de Powershell-opdracht Get-AzureInternalLoadbalancer-commandlet, verkrijgen  10.1.2.101). De syntaxis van de opdracht wordt hieronder weergegeven: Get AzureLoadbalancer – servicenaam "hk-c-svc-west-ons" [ziet u de details van de interne taakverdeling met het IP-adres]
2.  Meld u aan bij de web-farm VM (bv. hk-w1-west-us) met stopverf of ssh
3.  Uitvoeren van $CASS_HOME/bin/cqlsh 10.1.2.101 9160
4.  De volgende CQL-opdrachten gebruiken om te controleren of het cluster werkt:

        CREATE KEYSPACE customers_ks WITH REPLICATION = { 'class' : 'SimpleStrategy', 'replication_factor' : 3 };
        USE customers_ks;
        CREATE TABLE Customers(customer_id int PRIMARY KEY, firstname text, lastname text);
        INSERT INTO Customers(customer_id, firstname, lastname) VALUES(1, 'John', 'Doe');
        INSERT INTO Customers(customer_id, firstname, lastname) VALUES (2, 'Jane', 'Doe');

        SELECT * FROM Customers;

Hier ziet u een scherm zoals hieronder:

<table>
  <tr><th> customer_id </th><th> Voornaam </th><th> Achternaam </th></tr>
  <tr><td> 1 </td><td> John </td><td> De Vries </td></tr>
  <tr><td> 2 </td><td> Jane </td><td> De Vries </td></tr>
</table>

Houd er rekening mee dat de keyspace in stap 4 hebt gemaakt met een replication_factor van 3 SimpleStrategy gebruikt. SimpleStrategy wordt aanbevolen voor enkele data center implementaties dat NetworkTopologyStrategy voor meerdere data center-implementaties. Een replication_factor van 3 krijgt tolerantie voor storingen in knooppunten.

##<a id="tworegion"> </a>Implementatie in meerdere landen/regio
Zullen profiteren van de implementatie van één gebied voltooid en herhaalt hetzelfde proces voor het installeren van het tweede gebied. Het belangrijkste verschil tussen de implementatie van één of meerdere regio is de VPN-tunnel-instellingen voor de communicatie tussen de regio; We beginnen met de installatie via het netwerk, het VMs inrichten en Cassandra configureren.

###<a name="step-1-create-the-virtual-network-at-the-2nd-region"></a>Stap 1: Maak het virtuele netwerk op de 2e regio
Log in op de klassieke Azure portal en een virtueel netwerk maken met de kenmerken weergeven in de tabel. Zie [een virtuele netwerk Cloud-Only in de klassieke Azure portal configureren](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) voor gedetailleerde stappen van het proces.      

<table>
<tr><th>Naam van kenmerk    </th><th>Waarde    </th><th>Opmerkingen</th></tr>
<tr><td>Naam    </td><td>vnet-cass-Oost-nl</td><td></td></tr>
<tr><td>Regio  </td><td>Oost-Verenigde Staten</td><td></td></tr>
<tr><td>DNS-Servers     </td><td></td><td>Dit negeren als wij niet met behulp van een DNS-Server</td></tr>
<tr><td>Een point-to-site VPN configureren</td><td></td><td>     Dit negeren</td></tr>
<tr><td>Configureren van een VPN website</td><td></td><td>      Dit negeren</td></tr>
<tr><td>Adresruimte   </td><td>10.2.0.0/16</td><td></td></tr>
<tr><td>IP starten </td><td>10.2.0.0   </td><td></td></tr>
<tr><td>CIDR    </td><td>/ 16 (65531)</td><td></td></tr>
</table>

Voeg de volgende subnetten:
<table>
<tr><th>Naam    </th><th>IP starten    </th><th>CIDR   </th><th>Opmerkingen</th></tr>
<tr><td>Web </td><td>10.2.1.0   </td><td>/ 24 (251)  </td><td>Subnet voor de web-farm</td></tr>
<tr><td>gegevens    </td><td>10.2.2.0   </td><td>/ 24 (251)  </td><td>Subnet voor de databaseknooppunten</td></tr>
</table>


###<a name="step-2-create-local-networks"></a>Stap 2: Maak een lokale netwerken
Een lokaal netwerk in Azure virtueel netwerk is een proxy-adresruimte die is toegewezen aan een externe site, met inbegrip van een private cloud of een ander Azure regio. Deze proxy-adresruimte is gebonden aan een externe gateway voor routing gebaseerd netwerk naar de juiste bestemmingen netwerken. Zie [een VNet aan VNet-verbinding configureren](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md) voor de instructies voor het maken van VNET-VNET-verbinding.

Maak twee lokale netwerken per de volgende gegevens:

| Naam van netwerk | VPN-Gateway-adres | Adresruimte | Opmerkingen |
| ------------ | ------------------- | ------------- | ------- |
| HK-lnet-map-to-East-US | 23.1.1.1  | 10.2.0.0/16   | Tijdens het maken van het lokale netwerk geven u een tijdelijke aanduiding voor gateway-adres. Het echte gateway-adres wordt ingevuld als de gateway is gemaakt. Zorg ervoor dat de adresruimte exact overeenkomt met de desbetreffende externe VNET; in dit geval de VNET gemaakt in de regio Oost-VS. |
| HK-lnet-map-to-West-US | 23.2.2.2  | 10.1.0.0/16   | Tijdens het maken van het lokale netwerk geven u een tijdelijke aanduiding voor gateway-adres. Het echte gateway-adres wordt ingevuld als de gateway is gemaakt. Zorg ervoor dat de adresruimte exact overeenkomt met de desbetreffende externe VNET; in dit geval worden de VNET in de regio West VS gemaakt. |


###<a name="step-3-map-local-network-to-the-respective-vnets"></a>Stap 3: "Local" netwerkverbinding naar de respectieve VNETs
Vanuit de klassieke Azure portal selecteert u elke vnet, klikt u op 'Configureren' controleren 'Verbinding maken met het lokale netwerk' en selecteert u de lokale netwerken per de volgende gegevens:


| Virtueel netwerk | Lokaal netwerk |
| --------------- | ------------- |
| HK-vnet-west-ons | HK-lnet-map-to-East-US |
| HK-vnet-Oost-nl | HK-lnet-map-to-West-US |


###<a name="step-4-create-gateways-on-vnet1-and-vnet2"></a>Stap 4: Gateways maken op VNET1 en VNET2
Klik op GATEWAY maken die de VPN-gateway inrichtingsproces wordt gestart van het dashboard van de virtuele netwerken. Het dashboard van elke virtuele netwerk moet na een paar minuten het werkelijke gateway-adres worden weergegeven.

###<a name="step-5-update-local-networks-with-the-respective-gateway-addresses"></a>Stap 5: Update "Local" netwerken met de respectieve "Gateway"-adressen###
Bewerk het lokale netwerken ter vervanging van de tijdelijke aanduiding voor gateway-IP-adres met de echte IP-adres NET ingerichte gateways. Gebruik de volgende koppeling:

<table>
<tr><th>Lokaal netwerk    </th><th>Virtueel netwerkgateway</th></tr>
<tr><td>HK-lnet-map-to-East-US </td><td>Gateway van hk-vnet-west-ons</td></tr>
<tr><td>HK-lnet-map-to-West-US </td><td>Gateway van hk-vnet-Oost-nl</td></tr>
</table>

###<a name="step-6-update-the-shared-key"></a>Stap 6: De gedeelde sleutel wordt bijgewerkt
De volgende Powershell script gebruiken voor het bijwerken van de IPSec-sleutel van elke VPN-gateway [Gebruik de sleutel mogelijk te houden voor beide gateways]: Set AzureVNetGatewayKey - VNetName hk-vnet-Oost-ons LocalNetworkSiteName - hk-lnet-map-to-west-us - SharedKey D9E76BKK Set-AzureVNetGatewayKey - VNetName hk-vnet-west-ons LocalNetworkSiteName - hk-lnet-map-to-east-us - SharedKey D9E76BKK

###<a name="step-7-establish-the-vnet-to-vnet-connection"></a>Stap 7: De VNET-VNET-verbinding
Gebruik het menu 'DASHBOARD' van de virtuele netwerken gateway-naar-gateway-verbinding tot stand brengen vanuit de klassieke Azure portal. Gebruik de 'CONNECT' menu-items in de werkbalk onderin. Na een paar minuten moet het dashboard de verbindingsgegevens grafisch weergeven.

###<a name="step-8-create-the-virtual-machines-in-region-2"></a>Stap 8: Maak de virtuele machines in regio #2
Maak de afbeelding Ubuntu zoals beschreven in regio #1-implementatie door het werk of de kopie het VHD-bestand voor de afbeelding naar de rekening Azure opslag in regio #2 en de installatiekopie maken. Deze afbeelding gebruiken en de volgende lijst van virtuele machines maken in een nieuwe wolk service hk-c-svc-Oost-ons:


| Naam van de computer | Subnet | IP-adres | Beschikbaarheid instellen | DC/rek | Zaad? |
| ------------ | ------ | ---------- | ---------------- | ------- | ----- |
| HK-c1-Oost-nl | gegevens  | 10.2.2.4   | HK-c-uit-1      | DC = rack EASTUS = rack1 | Ja |
| HK-c2-Oost-nl | gegevens  | 10.2.2.5   | HK-c-uit-1      | DC = rack EASTUS = rack1 | Nee  |
| HK-c3-Oost-nl | gegevens  | 10.2.2.6   | HK-c-uit-1      | DC = rack EASTUS = rack2 | Ja |
| HK-c5-Oost-nl | gegevens  | 10.2.2.8   | HK-c-uit-2      | DC = rack EASTUS = rack3 | Ja |
| HK-c6-Oost-nl | gegevens  | 10.2.2.9   | HK-c-uit-2      | DC = rack EASTUS = rack3 | Nee  |
| HK-c7-Oost-nl | gegevens  | 10.2.2.10  | HK-c-uit-2      | DC = rack EASTUS = rack4 | Ja |
| HK-c8-Oost-nl | gegevens  | 10.2.2.11  | HK-c-uit-2      | DC = rack EASTUS = rack4 | Nee  |
| HK-w1-Oost-nl | Web   | 10.2.1.4   | HK-w-uit-1      | N.V.T.                    | N.V.T. |
| HK-w2-Oost-nl | Web   | 10.2.1.5   | HK-w-uit-1      | N.V.T.                    | N.V.T. |


Volg de instructies van de regio #1 maar 10.2.xxx.xxx-adresruimte gebruiken.

###<a name="step-9-configure-cassandra-on-each-vm"></a>Stap 9: Cassandra configureren op elke VM
Log in op de VM en als volgt te werk:

1. $CASS_HOME/conf/cassandra-rackdc.properties om op te geven van de eigenschappen van data center en rek in de indeling bewerken: dc = rack EASTUS = rack1
2. Bewerken cassandra.yaml zaad knooppunten configureren: zaden: "10.1.2.4,10.1.2.6,10.1.2.8,10.1.2.10,10.2.2.4,10.2.2.6,10.2.2.8,10.2.2.10"

###<a name="step-10-start-cassandra"></a>Stap 10: Cassandra starten
Log in op elke VM en Cassandra op de achtergrond gestart met de volgende opdracht: $CASS_HOME/bin/cassandra

## <a name="test-the-multi-region-cluster"></a>Het Cluster met meerdere regio testen
Cassandra is nu met 16 knooppunten met 8 knooppunten in elke regio Azure geïmplementeerd. Er zijn deze knooppunten in het cluster dezelfde algemene naam van het cluster en de seed-knooppuntconfiguratie. Gebruik de volgende procedure voor het testen van het cluster:

###<a name="step-1-get-the-internal-load-balancer-ip-for-both-the-regions-using-powershell"></a>Stap 1: De interne load balancer IP verkrijgen voor beide regio's met PowerShell
- Get-AzureInternalLoadbalancer - servicenaam "hk-c-svc-west-ons"
- Get-AzureInternalLoadbalancer - servicenaam "hk-c-svc-Oost-ons"  

    Opmerking de IP-adressen (bijvoorbeeld west - 10.1.2.101, Oost - 10.2.2.101) weergegeven.

###<a name="step-2-execute-the-following-in-the-west-region-after-logging-into-hk-w1-west-us"></a>Stap 2: Het volgende in de regio west uitvoeren nadat de aanmelding bij hk-w1-west-ons
1.    Uitvoeren van $CASS_HOME/bin/cqlsh 10.1.2.101 9160
2.  De volgende CQL opdrachten uitvoeren:

        CREATE KEYSPACE customers_ks
        WITH REPLICATION = { 'class' : 'NetworkToplogyStrategy', 'WESTUS' : 3, 'EASTUS' : 3};
        USE customers_ks;
        CREATE TABLE Customers(customer_id int PRIMARY KEY, firstname text, lastname text);
        INSERT INTO Customers(customer_id, firstname, lastname) VALUES(1, 'John', 'Doe');
        INSERT INTO Customers(customer_id, firstname, lastname) VALUES (2, 'Jane', 'Doe');
        SELECT * FROM Customers;

Hier ziet u een scherm zoals hieronder:

| customer_id | Voornaam | Achternaam |
| ----------- | --------- | -------- |
| 1           | John      | De Vries      |
| 2           | Jane      | De Vries      |


###<a name="step-3-execute-the-following-in-the-east-region-after-logging-into-hk-w1-east-us"></a>Stap 3: Uitvoeren van de volgende in de regio Oost na aanmelden bij hk-w1-Oost-ons:
1.    Uitvoeren van $CASS_HOME/bin/cqlsh 10.2.2.101 9160
2.  De volgende CQL opdrachten uitvoeren:

        USE customers_ks;
        CREATE TABLE Customers(customer_id int PRIMARY KEY, firstname text, lastname text);
        INSERT INTO Customers(customer_id, firstname, lastname) VALUES(1, 'John', 'Doe');
        INSERT INTO Customers(customer_id, firstname, lastname) VALUES (2, 'Jane', 'Doe');
        SELECT * FROM Customers;

Zoals voor de regio West worden weergegeven bij een vergelijkbaar scherm:


| customer_id | Voornaam | Achternaam   |
|------------ | --------- | ---------- |
| 1           | John      | De Vries        |
| 2           | Jane      | De Vries        |


Een paar meer wordt uitgevoerd en zien dat die gerepliceerd naar west-ons deel uitmaken van het cluster.

## <a name="test-cassandra-cluster-from-nodejs"></a>Test Cassandra Cluster van Node.js
Met behulp van een trapsgewijs eerder van de Linux VMs gewijzigd in 'het web', we een eenvoudig script Node.js om te lezen van de eerder ingevoegde gegevens wordt uitgevoerd

**Stap 1: Node.js en Cassandra-Client installeren**

1. Node.js en npm installeren
2. Knooppunt package 'cassandra-client' installeren met behulp van npm
3. Het volgende script op de shell prompt waarin de json-tekenreeks van de opgehaalde gegevens uitvoeren:

        var pooledCon = require('cassandra-client').PooledConnection;
        var ksName = "custsupport_ks";
        var cfName = "customers_cf";
        var hostList = ['internal_loadbalancer_ip:9160'];
        var ksConOptions = { hosts: hostList,
                             keyspace: ksName, use_bigints: false };

        function createKeyspace(callback){
           var cql = 'CREATE KEYSPACE ' + ksName + ' WITH strategy_class=SimpleStrategy AND strategy_options:replication_factor=1';
           var sysConOptions = { hosts: hostList,  
                                 keyspace: 'system', use_bigints: false };
           var con = new pooledCon(sysConOptions);
           con.execute(cql,[],function(err) {
           if (err) {
             console.log("Failed to create Keyspace: " + ksName);
             console.log(err);
           }
           else {
             console.log("Created Keyspace: " + ksName);
             callback(ksConOptions, populateCustomerData);
           }
           });
           con.shutdown();
        }

        function createColumnFamily(ksConOptions, callback){
          var params = ['customers_cf','custid','varint','custname',
                        'text','custaddress','text'];
          var cql = 'CREATE COLUMNFAMILY ? (? ? PRIMARY KEY,? ?, ? ?)';
        var con =  new pooledCon(ksConOptions);
          con.execute(cql,params,function(err) {
              if (err) {
                 console.log("Failed to create column family: " + params[0]);
                 console.log(err);
              }
              else {
                 console.log("Created column family: " + params[0]);
                 callback();
              }
          });
          con.shutdown();
        }

        //populate Data
        function populateCustomerData() {
           var params = ['John','Infinity Dr, TX', 1];
           updateCustomer(ksConOptions,params);

           params = ['Tom','Fermat Ln, WA', 2];
           updateCustomer(ksConOptions,params);
        }

        //update will also insert the record if none exists
        function updateCustomer(ksConOptions,params)
        {
          var cql = 'UPDATE customers_cf SET custname=?,custaddress=? where custid=?';
          var con = new pooledCon(ksConOptions);
          con.execute(cql,params,function(err) {
              if (err) console.log(err);
              else console.log("Inserted customer : " + params[0]);
          });
          con.shutdown();
        }

        //read the two rows inserted above
        function readCustomer(ksConOptions)
        {
          var cql = 'SELECT * FROM customers_cf WHERE custid IN (1,2)';
          var con = new pooledCon(ksConOptions);
          con.execute(cql,[],function(err,rows) {
              if (err)
                 console.log(err);
              else
                 for (var i=0; i<rows.length; i++)
                    console.log(JSON.stringify(rows[i]));
            });
           con.shutdown();
        }

        //exectue the code
        createKeyspace(createColumnFamily);
        readCustomer(ksConOptions)


## <a name="conclusion"></a>Conclusie
Microsoft Azure is een flexibel platform waarmee de werking van zowel Microsoft als open sourcesoftware zoals in deze oefening wordt gedemonstreerd. Clusters met hoge beschikbaarheid Cassandra kunnen worden geïmplementeerd op één datacenter door middel van de verspreiding van de clusterknooppunten in meerdere domeinen voor fouttolerantie. Cassandra clusters kunnen ook worden geïmplementeerd via meerdere Azure geografisch afgelegen gebieden voor noodherstel bewijs systemen. Azure en Cassandra samen de bouw van een hoge mate van schaalbaarheid en beschikbaarheid kunnen en noodherstel herstelbare cloud services nodig via het hedendaagse internet schalen services.  

##<a name="references"></a>Verwijzingen##
- [http://cassandra.apache.org](http://cassandra.apache.org)
- [http://www.datastax.com](http://www.datastax.com)
- [http://www.nodejs.org](http://www.nodejs.org)
