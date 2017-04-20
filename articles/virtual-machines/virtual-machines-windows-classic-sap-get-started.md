<properties
   pageTitle="Met behulp van SAP op Windows virtuele machines | Microsoft Azure"
   description="Wissen, over het gebruik van SAP op virtuele machines (VMs) in Microsoft Azure"
   services="virtual-machines-windows,virtual-network,storage"
   documentationCenter="saponazure"
   authors="MSSedusch"
   manager="timlt"
   editor=""
   tags="azure-service-management"
   keywords=""/>
<tags
   ms.service="virtual-machines-windows"
   ms.devlang="NA"
   ms.topic="campaign-page"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="na"
   ms.date="10/04/2016"
   ms.author="sedusch"/>

# <a name="using-sap-on-windows-virtual-machines-in-azure"></a>Met behulp van SAP op Windows virtuele machines in Azure

Cloud Computing is een veelgebruikte term die meer en meer belang binnen de IT-branche van kleine bedrijven tot grote en multinationale ondernemingen winnen wordt. Microsoft Azure is het Platform van de Cloud Services van Microsoft een breed spectrum van nieuwe mogelijkheden biedt. Klanten kunnen nu snel inrichten en toepassingen uit inrichten als Cloud-Services, zodat ze niet beperkt tot technische of budgettering beperkingen. In plaats van tijd en geld investeren in hardware, infrastructuur, bedrijven zich kunnen richten op de toepassing, bedrijfsprocessen en de voordelen voor klanten en gebruikers.

Met Microsoft Azure virtuele machines biedt Microsoft een uitgebreide infrastructuur als een platform voor de Service (IaaS). SAP NetWeaver gebaseerde toepassingen worden ondersteund op Azure virtuele Machines (IaaS). De witboeken die hieronder wordt beschreven hoe voor planning en implementatie van SAP NetWeaver gebaseerde toepassingen op Windows virtuele machines in Azure. SAP NetWeaver gebaseerde toepassingen te implementeren op [Linux virtuele machines](virtual-machines-linux-classic-sap-get-started.md).

[AZURE.INCLUDE [virtual-machines-common-classic-sap-get-started](../../includes/virtual-machines-common-classic-sap-get-started.md)]

## <a name="sap-netweaver-on-azure---ha"></a>SAP NetWeaver op Azure - HA

Titel: SAP NetWeaver op Azure - Clustering SAP ASC's / SCS exemplaren met Windows Server-Failover-Cluster op Azure met SIOS DataKeeper

Samenvatting: ' dit document wordt beschreven hoe u SIOS DataKeeper gebruiken voor het instellen van een maximaal beschikbare SAP ASC's / SCS-configuratie op Azure. SAP beschermt hun potentieel storing onderdelen zoals SAP ASC's / SCS of plaatsen Replication Services met Windows Server-Failover-Cluster configuraties die gedeelde schijven nodig hebben. Deze SAP-componenten zijn essentieel voor de functionaliteit van een SAP-systeem. Functies voor hoge beschikbaarheid moet nemen om ervoor te zorgen dat deze onderdelen is bestand tegen het uitvallen van een server of een VM zoals uitgevoerd met Windows-Cluster configuraties voor kaal en Hyper-V-omgevingen. Vanaf augustus 2015 kan Azure op zichzelf geen maximaal beschikbare configuraties die nodig zijn voor deze essentiële componenten van SAP op basis van gedeelde schijven die nodig zijn voor de Windows. Met behulp van het product DataKeeper door SIOS kunnen echter Windows Server-Failover-Cluster-configuraties die nodig zijn voor SAP ASC's / SCS worden opgebouwd op de Azure IaaS platform. In dit artikel wordt beschreven in een stap-voor-stap-benadering een configuratie van Windows Server-Failover-Cluster met gedeelde schijf geleverd door Datakeeper in Azure SIOS installeren. Het papier wordt informatie in configuraties aan de Azure, Windows en SAP die de hoge beschikbaarheid configuratie werkt op een optimale wijze uitgelegd. Het papier is een aanvulling op de documentatie voor installatie van SAP en SAP-opmerkingen staan voor de primaire bronnen voor installaties en implementaties van SAP-software op bepaalde platforms.

Bijgewerkt: Augustus 2015

[Deze handleiding nu downloaden](http://go.microsoft.com/fwlink/?LinkId=613056)
