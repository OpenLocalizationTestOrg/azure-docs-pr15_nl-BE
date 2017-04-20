<properties
    pageTitle="Groep altijd op beschikbaarheid in Azure VM automatisch - Resource Manager configureren"
    description="Maak een groep altijd op beschikbaarheid met Azure virtuele machines in de modus Azure Resource Manager. In deze zelfstudie wordt voornamelijk de gebruikersinterface maken automatisch de volledige oplossing."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="MikeRayMSFT"
    manager="jhubbard"
    editor=""
    tags="azure-resource-manager" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/20/2016"
    ms.author="MikeRayMSFT" />

# <a name="configure-always-on-availability-group-in-azure-vm-automatically---resource-manager"></a>Groep altijd op beschikbaarheid in Azure VM automatisch - Resource Manager configureren

> [AZURE.SELECTOR]
- [Resource Manager: sjabloon](virtual-machines-windows-portal-sql-alwayson-availability-groups.md)
- [Resource Manager: handleiding](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md)
- [Klassiek: UI](virtual-machines-windows-classic-portal-sql-alwayson-availability-groups.md)
- [Klassiek: PowerShell](virtual-machines-windows-classic-ps-sql-alwayson-availability-groups.md)

<br/>

Deze end-to-end-zelfstudie wordt beschreven hoe u een beschikbaarheid van SQL-servergroep maken met Azure Resource Manager virtuele machines. De zelfstudie wordt Azure blades gebruikt voor het configureren van een sjabloon. U zal de standaard instellingen vereiste instellingen en typ de bladen in de portal bijwerken als u deze zelfstudie hebt doorlopen.

Aan het einde van de zelfstudie voor bestaat uw SQL Server-oplossing beschikbaarheid groep in Azure uit de volgende elementen:

- Een virtueel netwerk meerdere subnetten, met inbegrip van een front-end- en back-end subnet met

- Twee domeincontrollers met een domein van Active Directory (AD)

- Twee SQL Server VMs geïmplementeerd op het subnet van de back-end en toegevoegd aan het domein AD

- Een cluster 3-node WSFC met de meerderheid van de knooppunt quorum model

- Een beschikbaarheidsgroep met twee replica's van synchrone doorvoeren van een database met beschikbaarheid

De onderstaande figuur is een grafische weergave van de oplossing.

![Testlab architectuur voor AG in Azure](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/0-EndstateSample.png)

Alle bronnen van deze oplossing deel uitmaken van één brongroep.

In deze zelfstudie wordt het volgende verondersteld:

- U hebt al een account met Azure. Als u nog geen, [Meld u aan voor een proefversie hebt](http://azure.microsoft.com/pricing/free-trial/).

- Weet u al hoe u een SQL Server-VM uit de galerie met virtuele machine met behulp van de GUI inrichten. Zie voor meer informatie, [een SQL Server virtuele machine op Azure inrichten](virtual-machines-windows-portal-sql-server-provision.md)

- U hebt al een goed begrip van de beschikbaarheid. Zie voor meer informatie [altijd op beschikbaarheidsgroepen (SQL Server)](http://msdn.microsoft.com/library/hh510230.aspx).

>[AZURE.NOTE] Als u geïnteresseerd bent in beschikbaarheidsgroepen gebruiken met SharePoint, Zie ook [configureren van SQL Server 2012 altijd op beschikbaarheidsgroepen voor SharePoint 2013](http://technet.microsoft.com/library/jj715261.aspx).

In deze zelfstudie wordt u de Azure portal te gebruiken:

- Selecteer de de sjabloon altijd op vanaf de portal

- Controleer de sjablooninstellingen en een paar configuratie-instellingen voor uw omgeving bijwerken

- Azure monitor die wordt gemaakt van de gehele omgeving

- Verbinding maken met een van de domeincontrollers en vervolgens op een van de SQL-Servers

[AZURE.INCLUDE [availability-group-template](../../includes/virtual-machines-windows-portal-sql-alwayson-ag-template.md)]


## <a name="provision-the-cluster-from-the-gallery"></a>Bepaling van het cluster uit de galerie

Azure biedt een afbeelding voor de volledige oplossing. Om te zoeken naar de sjabloon:

1.  Log in op de Azure portal met uw account.
1.  Op de portal Azure Klik **+ Nieuw.** De portal wordt het nieuwe blad geopend.
1.  Op de nieuwe blade zoeken **AlwaysOn**.
![AlwaysOn sjabloon zoeken](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/16-findalwayson.png)
1.  Zoek in de lijst met zoekresultaten **AlwaysOn Cluster van SQL Server**.
![AlwaysOn-sjabloon](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/17-alwaysontemplate.png)
1.  Kies op het **selecteren van een implementatiemodel** **Resource Manager**.

### <a name="basics"></a>Grondbeginselen

Klik op de **Grondbeginselen** en het volgende configureren:

- **Gebruikersnaam beheerder** is een gebruikersaccount met beheerdersmachtigingen en een lid van de SQL-Server voor de vaste serverrol sysadmin op beide exemplaren van SQL Server. Voor deze zelfstudie gebruiken **DomainAdmin**.

- **Wachtwoord** is het wachtwoord voor de administrator-account van het domein. Een complex wachtwoord te gebruiken. Bevestig het wachtwoord.

- **Abonnement** is het abonnement dat Azure in rekening zal worden uitgevoerd van de middelen die worden ingezet voor de beschikbaarheidsgroep. U kunt een ander abonnement opgeven als uw account meerdere abonnementen heeft.

- **Resourcegroep** is de naam voor de groep die alle bronnen Azure door deze zelfstudie gemaakt moet behoren. Voor deze zelfstudie gebruiken **SQL-HA-RG**. Zie voor meer informatie (overzicht Azure Resource Manager) [resource-groep-overview.md / #-resourcegroepen].

- **De locatie** is de Azure regio waar de middelen voor deze zelfstudie worden gemaakt. Selecteer een Azure regio als host voor de infrastructuur.

Hieronder ziet u de **Grondbeginselen van** blade zoeken:

![Grondbeginselen](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/1-basics.png)

- Klik op **OK**.

### <a name="domain-and-network-settings"></a>Domein-en netwerkinstellingen

Deze galerie Azure-sjabloon maakt een nieuw domein met nieuwe domeincontrollers. Maakt ook een nieuw netwerk en twee subnetten. De sjabloon wordt niet ingeschakeld voor het maken van de servers in een domein of een virtueel netwerk. De volgende stap is het domein en netwerk-instellingen configureren.

Bekijk de vooraf ingestelde waarden voor de domein- en instellingen op **domein-en netwerkinstellingen** blade:

- **Forest-hoofddomein** is de domeinnaam die wordt gebruikt voor het AD-domein dat als host voor het cluster optreedt. Gebruik voor de zelfstudie **contoso.com**.

- **Virtuele-netwerknaam** is de naam van het netwerk voor de Azure virtueel netwerk. Voor deze zelfstudie gebruiken **autohaVNET**.

- **Domeincontroller** is subnet de naam van een deel van het virtuele netwerk dat de host de domeincontroller. Voor deze zelfstudie gebruiken **subnet 1**. Dit subnet gebruikt adres voorvoegsel **10.0.0.0/24**.

- **Subnet SQL-servernaam** is de naam van een deel van het virtuele netwerk dat hosts de SQL-Servers en de getuige bestandsshare. Voor deze zelfstudie gebruiken **subnet 2**. Dit subnet gebruikt adres voorvoegsel **10.0.1.0/26**.

Voor meer informatie over virtuele netwerken in [Azure Virtual Network overzicht](../virtual-network/virtual-networks-overview.md).  

De **domein-en netwerkinstellingen** ziet er zo uit:

![Domein-en netwerkinstellingen](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/2-domain.png)

Indien nodig, kunt u deze waarden kan wijzigen. Voor deze zelfstudie gebruiken we de vooraf ingestelde waarden.

- Controleer de instellingen en klik op **OK**.

###<a name="availability-group-settings"></a>instellingen voor de beschikbaarheid

Controleer de vooraf ingestelde waarden voor de beschikbaarheidsgroep en de listener op de **Instellingen voor de beschikbaarheid** .

- **Beschikbaarheid groepsnaam** is de naam van het geclusterde bron voor de beschikbaarheidsgroep. Voor deze zelfstudie gebruiken **Contoso ag**.

- **beschikbaarheid listener groepsnaam** wordt gebruikt door het cluster en de interne taakverdeling. Clients die verbinding maken met SQL Server kunnen u deze naam gebruiken voor verbinding met de juiste replica van de database. Voor deze zelfstudie gebruiken **Contoso-listener**.

-  **beschikbaarheid groep luisterpoort** geeft dat de TCP-poort de listener van SQL Server wordt gebruikt. Voor deze zelfstudie gebruiken de standaardpoort, **1433**.

Indien nodig, kunt u deze waarden kan wijzigen. Gebruik de vooraf ingestelde waarden voor deze zelfstudie.  

![instellingen voor de beschikbaarheid](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/3-availabilitygroup.png)

- Klik op **OK**.

###<a name="vm-size-storage-settings"></a>VM-grootte, Opslaginstellingen voor

Kies een grootte van SQL Server virtuele machine op **VM-grootte, Opslaginstellingen** en bekijk de andere instellingen.

- **SQL Server** is virtuele machine Azure VM groot voor zowel SQL Server. Kies een virtuele machine grootte die geschikt is voor uw werkbelasting. Als u deze omgeving voor de zelfstudie gebruiken **DS2** Werkbelasting Kies de grootte van een virtuele machine die ondersteuning voor de werkbelasting bieden voor productie. Veel productie werklasten **DS4** vereist of groter. De sjabloon wordt twee virtuele machines van deze grootte bouwen en installeren van SQL Server op elk. Zie voor meer informatie [voor virtuele machines](virtual-machines-linux-sizes.md).

>[AZURE.NOTE]Azure installeert Enterprise Edition van SQL Server. De kosten zijn afhankelijk van de editie en de grootte van de virtuele machine. Zie voor gedetailleerde informatie over de huidige kosten [prijzen virtuele machines](http://azure.microsoft.com/pricing/details/virtual-machines/#Sql).

- **Virtuele machine hierbij** is de grootte van de virtuele machine voor de domeincontrollers. Voor deze zelfstudie gebruiken **D2**.

- **Bestand delen getuige virtuele machine grootte** is de grootte van de virtuele machine voor de bestandssharewitness. Voor deze zelfstudie gebruiken **A1**.

- **Opslag van SQL-account** is de naam van de account van de opslag voor de SQL Server-gegevens en het besturingssysteem schijven. Voor deze zelfstudie gebruiken **alwaysonsql01**.

- **DC-opslag** is de naam van de account van de opslag voor de domeincontrollers. Voor deze zelfstudie gebruiken **alwaysondc01**.

- **SQL Server-gegevens schijfgrootte** TB is de grootte van de schijf met de SQL Server-gegevens in TB. Geef een getal tussen 1 en 4. Dit is de grootte van de schijf met gegevens die wordt gekoppeld aan elke SQL-Server. Gebruik **1**voor deze zelfstudie.

- **Optimalisatie van Storage** opslag specifieke configuratie-instellingen voor de SQL Server virtuele machines op basis van het type werklast ingesteld. Alle SQL Server in dit scenario gebruiken premium-opslagruimte met Azure host schijfcache instellen op alleen-lezen. Bovendien kunt u instellingen voor de werkbelasting van de SQL Server optimaliseren door een van deze drie instellingen te kiezen:

    - **Algemene werkbelasting** stelt geen specifieke configuratie-instellingen

    - **Verwerking van afhandeling van transactionele** traceringsvlag 1117 en 1118 ingesteld

    - **Data warehousing** traceringsvlag 1117 ingesteld en 610

Gebruik **algemene werkbelasting**voor deze zelfstudie.

![De instellingen voor opslag VM](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/4-vm.png)

- Controleer de instellingen en klik op **OK**.

####<a name="a-note-about-storage"></a>Een opmerking over opslag

Aanvullende optimalisatie is afhankelijk van de grootte van de SQL Server-gegevens-schijven. Voor elke terabyte schijf gegevens toegevoegd Azure een aanvullende premie 1 TB aan opslag (SSD). Wanneer een server 2 TB of meer vereist, maakt de sjabloon u een opslaggroep op elke SQL-Server. Een opslaggroep is een vorm van virtualisatie van opslag, waarbij meerdere schijven zijn geconfigureerd voor hogere capaciteit, tolerantie en prestaties.  De sjabloon vervolgens maakt u een opslagruimte op de toepassingen van de opslag en presenteert deze als een enkele gegevens aan het besturingssysteem. De sjabloon wordt deze schijf als de gegevensschijf voor SQL Server. De sjabloon verbetert de de opslaggroep prestatie voor SQL Server met de volgende instellingen:

- Stripe-grootte is de interleave-instelling voor de virtuele schijf. Dit is voor transactionele belasting ingesteld op 64 KB. De instelling is voor data warehousing workloads 256 KB.

- Tolerantie is eenvoudig (geen tolerantie).

>[AZURE.NOTE] Azure premium opslag is lokaal overbodig en drie kopieën van de gegevens blijft binnen één regio meer tolerantie op de opslag van toepassingen en is dus niet vereist.

- Aantal kolommen is gelijk aan het aantal schijven in de groep.

Zie voor meer informatie over opslag ruimte en de opslag van toepassingen:

- [Opslag-overzicht van spaties](http://technet.microsoft.com/library/hh831739.aspx).

- [Windows Server Backup en opslag van toepassingen](http://technet.microsoft.com/library/dn390929.aspx)

Zie [Aanbevolen procedures voor SQL Server in Azure virtuele machines prestaties](virtual-machines-windows-sql-performance.md) voor meer informatie over aanbevolen procedures voor SQL Server-configuratie


###<a name="sql-server-settings"></a>SQL Server-instellingen

Op de **SQL Server-instellingen** controleren en wijzigen van voorvoegsel van de SQL Server VM versie van SQL Server, SQL Server-serviceaccount en wachtwoord en de SQL-automatische onderhoudsplanning patchen.

- Een naam maken voor elke SQL Server wordt **SQL Server voorvoegsel** gebruikt. Voor deze zelfstudie gebruiken **Contoso ag**. De SQL Server-namen worden *Contoso-ag-0* *Contoso-ag-1*.

- **SQL Server-versie** is de versie van SQL Server. Voor deze zelfstudie gebruiken **SQL Server 2014**. U kunt ook **SQL Server 2012** of **SQL Server 2016**.

- **SQL Server service account-gebruikersnaam** is de naam van het domein voor de SQL Server-service. Voor deze zelfstudie gebruiken **sqlservice**.

- **Wachtwoord** is het wachtwoord voor de SQL Server-serviceaccount.  Een complex wachtwoord te gebruiken. Bevestig het wachtwoord.

- **Onderhoudsplanning SQL automatisch patches** geeft de weekdag Azure zal automatisch patch voor de SQL-Servers. Typ voor deze zelfstudie **zondag**.

- **SQL automatisch patches onderhoud uur start** is het tijd voor de regio Azure begint automatisch te herstellen.

>[AZURE.NOTE]Het venster patch voor elke VM is één uur gespreid. Slechts één virtuele machine tegelijk gerepareerd om te voorkomen dat de verstoring van diensten.

![SQL Server-instellingen](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/5-sql.png)

Controleer de instellingen en klik op **OK**.

###<a name="summary"></a>Samenvatting

Azure valideert op de pagina Samenvatting de instellingen. U kunt de sjabloon ook downloaden. Bekijk het overzicht. Klik op **OK**.

###<a name="buy"></a>Kopen

Deze laatste blade bevat **Gebruiksvoorwaarden**en het **privacybeleid**. Lees deze informatie. Wanneer u klaar voor Azure om te beginnen met het maken van de virtuele machines en alle andere vereiste resources voor de van beschikbaarheidsgroep bent, klikt u op **maken**.

De Azure portal maakt de resourcegroep en alle bronnen.

##<a name="monitor-deployment"></a>Implementatie van monitor

De voortgang van de implementatie van het portal voor Azure volgen. Een pictogram voor de installatie is automatisch aan de Azure portal dashboard vastgemaakt.

![Azure Dashboard](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/11-deploydashboard.png)

##<a name="connect-to-sql-server"></a>Verbinding maken met SQL Server

De nieuwe exemplaren van SQL Server uitvoert op virtuele machines die geen verbindingen met het internet. Domeincontrollers hebben echter een verbinding met internet. De verbinding met de SQL-servers met extern bureaublad, eerste RDP op een van de domeincontrollers. Open een tweede RDP naar de SQL-Server van de domeincontroller.

Met RDP naar de primaire domeincontroller, als volgt te werk:

1.  Uit de Azure zeer dat de installatie is geslaagd portal dashboard.

1.  Klik op **Resources**.

1.  Klik op **ad-primair-dc** de naam van de virtuele machine voor de primaire domeincontroller is in het blad **bronnen** .

1.  Klik op het blad voor **ad-primair-dc** **verbinden**. Uw browser wordt gevraagd of u wilt openen of opslaan van de remote connection-object. Klik op **openen**.
![Verbinding maken met domeincontroller](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/13-ad-primary-dc-connect.png)
1.  **Verbinding met extern bureaublad** kan u worden gewaarschuwd dat de uitgever van deze externe verbinding niet kan worden geïdentificeerd. Klik op **verbinden**.

1.  Windows-beveiliging moet u uw referenties voor verbinding met het IP-adres van de primaire domeincontroller invoeren. Klik op **een andere account gebruiken**. Typ **contoso\DomainAdmin**voor de **gebruikersnaam** . Dit is de account die u hebt gekozen voor gebruikersnaam beheerder. Het complex wachtwoord die u hebt gekozen bij het instellen van de sjabloon gebruiken.

1.  **Extern bureaublad** kan u worden gewaarschuwd dat de externe computer niet kan worden geverifieerd door problemen met het beveiligingscertificaat. Dit ziet u de naam van de beveiliging. Als u de zelfstudie gevolgd worden de naam van de **primaire-ad-dc.contoso.com**. Klik op **Ja**.

U bent nu verbonden met de primaire domeincontroller. Met RDP naar SQL Server, als volgt te werk:

1.  Open **Verbinding met extern bureaublad**op de domeincontroller.

1.  **Computer**, typt u de naam van een van de SQL-Servers. Voor deze zelfstudie, typt u **SQL Server-0**.

1.  Gebruik dezelfde gebruikersaccount en wachtwoord dat u voor RDP naar de domeincontroller gebruikt.

U bent nu verbonden met RDP naar SQL Server. U kunt SQL Server management studio te openen, verbinding maken met het standaardexemplaar van SQL Server en controleer of dat de availabilty-groep is geconfigureerd.


