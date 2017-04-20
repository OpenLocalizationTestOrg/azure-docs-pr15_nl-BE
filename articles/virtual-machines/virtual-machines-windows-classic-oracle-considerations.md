<properties
pageTitle="Overwegingen bij het gebruik van Oracle VM afbeeldingen | Microsoft Azure"
description="Meer informatie over ondersteunde configuraties en beperkingen voor een Oracle VM op Windows Server in Azure voordat u gaat implementeren."
services="virtual-machines-windows"
documentationCenter=""
manager="timlt"
authors="rickstercdn"
tags="azure-service-management"/>

<tags
ms.service="virtual-machines-windows"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="vm-windows"
ms.workload="infrastructure-services"
ms.date="09/06/2016"
ms.author="rclaus" />

#<a name="miscellaneous-considerations-for-oracle-virtual-machine-images"></a>Diverse overwegingen voor Oracle VM afbeeldingen



Dit artikel heeft betrekking op overwegingen voor Oracle virtuele machines in Azure, die zijn gebaseerd op Oracle software-images door Oracle wordt geleverd.  

-  Oracle Database virtuele machine afbeeldingen
-  Afbeeldingen van Oracle WebLogic Server virtuele machine
-  Oracle JDK virtuele machine afbeeldingen

##<a name="oracle-database-virtual-machine-images"></a>Oracle Database virtuele machine afbeeldingen
### <a name="clustering-rac-is-not-supported"></a>Clusters (RAC) wordt niet ondersteund.

Azure biedt momenteel geen ondersteuning voor Oracle Real Application Clusters (RAC) van de Oracle-Database. Alleen zelfstandige exemplaren van de Oracle-Database zijn mogelijk. Dit komt doordat de Azure biedt momenteel geen ondersteuning virtuele schijf te delen op een wijze die lezen/schrijven over verschillende virtuele machine. Multicast-UDP wordt niet ondersteund.

### <a name="no-static-internal-ip"></a>Geen statische interne IP

Azure elke virtuele machine een intern IP-adres worden toegewezen. Tenzij de virtuele machine deel uit van een virtueel netwerk maakt, wordt het IP-adres van de virtuele machine is dynamisch en kan veranderen nadat u de virtuele machine opnieuw wordt opgestart. Dit kan problemen veroorzaken omdat de Oracle-Database het IP-adres verwacht statisch. Als u wilt voorkomen dat het probleem, kunt u de virtuele machine toe te voegen aan een virtueel netwerk Azure. Zie [Virtuele netwerken](https://azure.microsoft.com/documentation/services/virtual-network/) en [maken een virtueel netwerk in Azure](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) voor meer informatie.

### <a name="attached-disk-configuration-options"></a>Aangesloten schijf configuratie-opties

U kunt gegevensbestanden plaatsen op de schijf besturingssysteem van de virtuele machine of op een aangesloten schijven, ook bekend als gegevensschijven. Aangesloten schijven biedt betere prestaties en grootte flexibiliteit dan de schijf mogelijk. De schijf is mogelijk beter alleen voor databases onder 10 gigabytes (GB).

Aangesloten schijven zijn gebaseerd op de Azure Blob storage-service. Elke schijf zijn een theoretisch maximum van ongeveer 500 invoer/uitvoer-bewerkingen per seconde (IOP's). De prestaties van gekoppelde schijven mogelijk niet optimaal in eerste instantie en IOP's de prestaties mogelijk sterk na 'inbranden' periode van ongeveer 60-90 minuten van continue werking. Als een schijf vervolgens actief blijft, kan pas een andere branden in periode duurtest van IOP's prestaties verminderen. Kortom, de meer actieve een schijf, hoe groter de kans is voor aanpak IOP's optimaal.

Hoewel de eenvoudigste oplossing is een enkele schijf toevoegen aan de virtuele machine en plaatst u de databasebestanden op de schijf, is deze benadering ook de meest beperkende in termen van prestaties. In plaats daarvan kunt u vaak de effectieve IOP's prestaties verbeteren als u meerdere gekoppelde schijven gegevens verspreid over deze en Oracle automatische Storage Management (ASM) gebruiken. Zie [automatische opslag van Oracle-overzicht](http://www.oracle.com/technetwork/database/index-100339.html) voor meer informatie. Striping van meerdere schijven op het niveau van het besturingssysteem gebruiken, maar dat wordt afgeraden omdat deze niet bekend is IOP's prestaties te verbeteren.

Houd rekening met twee verschillende benaderingen voor het koppelen van meerdere schijven op basis van de vraag of u wilt de prioriteit van de prestaties van leesbewerkingen en schrijfbewerkingen voor uw database:

- **ASM Oracle zelf** is waarschijnlijk leiden tot betere schrijfprestaties bewerking maar erger IOP's voor leesbewerkingen in vergelijking met de benadering met disk arrays. In de volgende afbeelding ziet u logisch deze rangschikking.  
    ![](media/virtual-machines-windows-classic-oracle-considerations/image2.png)

>[AZURE.IMPORTANT] De verhouding tussen prestaties van schrijven en lezen op een per geval te evalueren. De werkelijke resultaten kunnen verschillen wanneer u deze gebruikt.

### <a name="high-availability-and-disaster-recovery-considerations"></a>Hoge beschikbaarheid en herstel na storing herstel overwegingen

Wanneer u de Oracle-Database in Azure virtuele machines, bent u verantwoordelijk voor de uitvoering van een hoge beschikbaarheid en noodherstel recovery oplossing om de uitvaltijd te voorkomen. U bent ook verantwoordelijk voor de back-ups van uw eigen gegevens en toepassingen.

Hoge beschikbaarheid en herstel na storing herstel voor Oracle Database Enterprise Edition (zonder RAC) op Azure kan worden bereikt met [Data Guard Active Data Guard](http://www.oracle.com/technetwork/articles/oem/dataguardoverview-083155.html)of [Oracle Golden Gate](http://www.oracle.com/technetwork/middleware/goldengate), met twee databases in twee afzonderlijke virtuele machines. Beide virtuele machines moeten in dezelfde [service cloud](virtual-machines-linux-classic-connect-vms.md) en hetzelfde [virtuele netwerk](https://azure.microsoft.com/documentation/services/virtual-network/) zodat ze toegang tot elkaar via het persoonlijke permanente IP-adres.  Bovendien is het raadzaam de virtuele machines te brengen in de dezelfde [beschikbaar](virtual-machines-windows-manage-availability.md) dat Azure plaatsen in de fout met betrekking tot afzonderlijke domeinen en domeinen upgraden. Alleen virtuele machines in dezelfde service cloud deel uitmaken van dezelfde set beschikbaarheid. Elke virtuele machine moet ten minste 2 GB geheugen en 5 GB schijfruimte hebben.

Met Oracle Data Guard, hoge beschikbaarheid kan worden bereikt met een primaire database in één virtuele machine, een secundaire (stand-by) database in een andere virtuele machine, en unilaterale replicatie instellen tussen hen. Het resultaat is leestoegang tot de kopie van de database. U kunt met Oracle GoldenGate, bidirectionele replicatie tussen de twee databases. Zie meer informatie over het instellen van een oplossing met hoge beschikbaarheid voor uw databases met behulp van deze hulpprogramma's, documentatie van [Active Data Guard](http://www.oracle.com/technetwork/database/features/availability/data-guard-documentation-152848.html) en [GoldenGate](http://docs.oracle.com/goldengate/1212/gg-winux/index.html) op de Oracle-website. Als u moet lees-schrijftoegang tot het exemplaar van de database, kunt u [De actieve Data Guard Oracle](http://www.oracle.com/uk/products/database/options/active-data-guard/overview/index.html).

##<a name="oracle-weblogic-server-virtual-machine-images"></a>Afbeeldingen van Oracle WebLogic Server virtuele machine

-  **Clustering wordt ondersteund door Enterprise Edition alleen.** U mag u WebLogic alleen wanneer de Enterprise Edition van WebLogic Server clusters. Gebruik geen clustering met WebLogic Server Standard Edition.

-  **Time-outs voor verbindingen:** Als uw toepassing afhankelijk is van verbindingen met openbare eindpunten van een andere Azure cloud service (bijvoorbeeld een database laag), mogelijk Azure deze open verbindingen gesloten na vier minuten van inactiviteit. Dit kan van invloed op functies en toepassingen die steunen op de verbinding van toepassingen, omdat verbindingen die voor meer dan de limiet die inactief zijn mogelijk niet meer geldig blijven. Als dit van invloed is op uw toepassing, kunt u "keep-alive" logica in uw toepassingen verbinding inschakelen.

    Als een eindpunt is *intern* voor uw implementatie Azure cloud-service (bijvoorbeeld een zelfstandige database virtuele machine binnen *dezelfde* service cloud als uw WebLogic virtuele machines), vervolgens de verbinding is directe en niet afhankelijk is van de Azure taakverdeling en is dus niet onderworpen aan een time-out van verbinding.

-  **Multicast-UDP wordt niet ondersteund.** Azure biedt ondersteuning voor UDP unicast-verzending, maar geen multicasting of uitzenden. WebLogic Server kan vertrouwen op Azure UDP unicast-mogelijkheden. Voor de beste resultaten steunen op UDP unicast, wordt aangeraden dat de clustergrootte WebLogic statisch, blijven of worden gehouden met niet meer dan 10 beheerde servers in het cluster wordt opgenomen.

-  **WebLogic Server verwacht dat openbare en particuliere poorten voor T3 toegang (bijvoorbeeld wanneer u Enterprise JavaBeans) hetzelfde te zijn.** Neem een meerlaags scenario waar een servicetoepassing laag (EJB) wordt uitgevoerd op een servercluster met WebLogic die bestaat uit twee of meer beheerde servers in een cloud-service met de naam **SLWLS**. De clientlaag is in een andere cloud-service, een eenvoudige Java-programma probeert te bellen EJB in de laag van de service. Omdat het voor het verdelen van de laag service nodig is, moet een openbare taakverdeling eindpunt voor de virtuele Machines in het cluster WebLogic Server worden gemaakt. Als de privé-poort die u voor dat eindpunt opgeeft verschilt van de openbare poort (bijvoorbeeld 7006:7008), treedt er een fout als volgt op:

        [java] javax.naming.CommunicationException [Root exception is java.net.ConnectException: t3://example.cloudapp.net:7006:

        Bootstrap to: example.cloudapp.net/138.91.142.178:7006' over: 't3' got an error or timed out]

    Dit komt omdat voor elk RAS T3 WebLogic Server verwacht de load balancer poort en de poort die beheerd WebLogic server hetzelfde te zijn. In het bovenstaande geval de client toegang heeft tot poort 7006 (load balancer poort) en de beheerde server luistert op 7008 (private poort). Deze beperking is alleen van toepassing op access T3, en niet het HTTP.

    U voorkomt dit probleem, gebruikt u een van de volgende oplossingen:

    -  Gebruik dezelfde particuliere en openbare poortnummers voor taakverdeling eindpunten toegewezen voor toegang tot de T3.

    -  De volgende parameter voor JVM opnemen bij het opstarten van de WebLogic Server:

            -Dweblogic.rjvm.enableprotocolswitch=true

Zie KB-artikel **860340.1** op <http://support.oracle.com>voor meer informatie.

-  **Dynamische clustering en werklastverdeling beperkingen.** Stel dat u een dynamische cluster in WebLogic Server en deze weergeven via een enkele, openbare taakverdeling eindpunt in Azure. U kunt dit doen als u een vaste poortnummer voor elk van de beheerde servers gebruiken (niet dynamisch toegewezen uit een bereik) en beter beheerde servers dan de machines, het bijhouden van de beheerder niet worden gestart (dat wil zeggen, niet meer dan een beheerde server per virtuele machine). Als uw configuratie in meer WebLogic servers resulteert dan de virtuele machines worden gestart (dat wil zeggen, waar meerdere exemplaren van de WebLogic Server delen dezelfde virtuele machine), dan is het niet mogelijk om meer dan een van de instanties van de servers te koppelen aan een specifiek poortnummer WebLogic Server – anderen in die virtuele machine zal mislukken.

    Aan de andere kant, als u de server admin unieke poortnummers automatisch toewijzen aan de beheerde servers configureren, is vervolgens taakverdeling niet mogelijk omdat Azure biedt geen ondersteuning voor toewijzing van een enkele openbare poort naar meerdere particuliere poorten, zoals nodig is voor deze configuratie.

-  **Meerdere exemplaren van Weblogic Server op een virtuele machine.** Afhankelijk van uw implementatievereisten, kunt u overwegen de mogelijkheid om meerdere exemplaren van WebLogic Server uitgevoerd op dezelfde virtuele machine als de virtuele machine groot genoeg is. Op een virtuele machine van gemiddelde grootte, met twee cores, kunt u bijvoorbeeld twee exemplaren van WebLogic Server uitvoeren. Let wel dat wij nog steeds aanraden om invoering van potentiële risico in uw architectuur die het geval zijn zou als u slechts één virtuele machine met meerdere exemplaren van WebLogic Server gebruikt. Met behulp van ten minste twee virtuele machines kan een betere benadering, en elk van deze virtuele machines kan dan meerdere exemplaren van WebLogic Server uitvoeren. Elk van deze instanties van WebLogic Server kan nog steeds deel uitmaken van hetzelfde cluster. Opmerking, maar het is momenteel niet mogelijk te gebruiken Azure-taakverdeling eindpunten die worden gebruikt door dergelijke implementaties WebLogic Server binnen dezelfde virtuele machine, omdat de servers verdeeld moet worden verdeeld over de unieke virtuele machines voor Azure taakverdeling.

##<a name="oracle-jdk-virtual-machine-images"></a>Oracle JDK virtuele machine afbeeldingen

-  **JDK 6 en 7 van de meest recente updates.** Terwijl het is raadzaam de meest recente openbare, ondersteunde versie van Java (momenteel Java 8), Azure ook uitgerust met JDK 6 en 7 afbeeldingen. Dit is bedoeld voor oudere toepassingen die nog niet klaar om te upgraden naar 8 JDK. Terwijl de vorige JDK afbeeldingen mogelijk updates niet meer beschikbaar zijn voor het publiek, het Microsoft-partnerschap met Oracle, gegeven de JDK 6 en 7 afbeeldingen geleverd door Azure willen bevatten een meer recente update voor niet-openbare die normaal door Oracle wordt aangeboden aan een selecte groep ondersteunde klanten van Oracle. Nieuwe versies van de JDK-afbeeldingen wordt beschikbaar gesteld in de tijd met bijgewerkte versies van de JDK 6 en 7.

    De JDK beschikbaar in deze JDK 6 en 7 beelden, en de virtuele machines en afbeeldingen zijn afgeleid, kan alleen worden gebruikt in Azure.

-  **64-bits JDK.** De afbeeldingen van Oracle WebLogic Server virtuele machine en de afbeeldingen van Oracle JDK virtuele machine geleverd door Azure bevatten de 64-bits versies van Windows Server en van de JDK.

##<a name="additional-resources"></a>Aanvullende bronnen
[Oracle VM afbeeldingen voor Azure](virtual-machines-linux-classic-oracle-images.md)
