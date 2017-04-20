<properties
    pageTitle="Groep altijd op beschikbaarheid in Azure VM - klassieke configureren"
    description="Een permanente groep van beschikbaarheid met Azure virtuele Machines maken. In deze zelfstudie wordt voornamelijk de gebruikersinterface en de hulpprogramma's in plaats van het uitvoeren van scripts."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="MikeRayMSFT"
    manager="jhubbard"
    editor=""
    tags="azure-service-management" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="09/22/2016"
    ms.author="mikeray" />

# <a name="configure-always-on-availability-group-in-azure-vm---classic"></a>Groep altijd op beschikbaarheid in Azure VM - klassieke configureren

> [AZURE.SELECTOR]
- [Resource Manager: sjabloon](virtual-machines-windows-portal-sql-alwayson-availability-groups.md)
- [Resource Manager: handleiding](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md)
- [Klassiek: UI](virtual-machines-windows-classic-portal-sql-alwayson-availability-groups.md)
- [Klassiek: PowerShell](virtual-machines-windows-classic-ps-sql-alwayson-availability-groups.md)

<br/>

> [AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Deze end-to-end-zelfstudie wordt beschreven hoe u beschikbaarheidsgroepen met SQL Server altijd op Azure virtuele machines waarop implementeren.

Uw oplossing SQL Server altijd op in Azure wordt aan het einde van de zelfstudie bestaat uit de volgende elementen:

- Een virtueel netwerk meerdere subnetten, met inbegrip van een front-end- en back-end subnet met

- Een domeincontroller met een domein van Active Directory (AD)

- Twee SQL Server VMs geïmplementeerd op het subnet van de back-end en toegevoegd aan het domein AD

- Een cluster 3-node WSFC met de meerderheid van de knooppunt quorum model

- Een beschikbaarheidsgroep met twee replica's van synchrone doorvoeren van een database met beschikbaarheid

De onderstaande figuur is een grafische weergave van de oplossing.

![Testlab architectuur voor AG in Azure](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC791912.png)

Houd er rekening mee dat dit een mogelijke configuratie is. Bijvoorbeeld, kunt u het aantal VMs voor een beschikbaarheidsgroep van twee replica om op te slaan op uren in Azure berekenen met behulp van de domeincontroller als het quorum bestandssharewitness in een cluster met 2 nodes WSFC minimaliseren. Deze methode vermindert het aantal VM door een van de bovenstaande configuratie.

In deze zelfstudie wordt het volgende verondersteld:

- U hebt al een account met Azure.

- Weet u al hoe u een klassieke SQL Server VM uit de galerie met virtuele machine met behulp van de GUI inrichten.

- U hebt al een goed begrip van altijd op beschikbaarheidsgroepen. Zie voor meer informatie [Altijd op beschikbaarheidsgroepen (SQL Server)](https://msdn.microsoft.com/library/hh510230.aspx).

>[AZURE.NOTE] Als u geïnteresseerd bent in altijd op beschikbaarheidsgroepen gebruiken met SharePoint, Zie ook [Configureren van SQL Server 2012 altijd op beschikbaarheid groepen voor SharePoint 2013](https://technet.microsoft.com/library/jj715261.aspx).

## <a name="create-the-virtual-network-and-domain-controller-server"></a>Het virtuele netwerk en Domain controllerserver maken

U start met een nieuwe account met Azure evaluatieversie. Zodra u klaar bent met het instellen van uw account, moet u in het beginscherm van de klassieke Azure portal.

1. Klik op de knop **Nieuw** op de linkerbenedenhoek van de pagina, zoals hieronder wordt weergegeven.

    ![Klik op Nieuw in de portal](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665511.gif)

1. Klik op **Netwerkservices**, en vervolgens **Virtueel netwerk** op en klik op **Aangepaste maken**, zoals hieronder wordt weergegeven.

    ![Virtueel netwerk maken](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665512.gif)

1. Maak een nieuw virtueel netwerk door door de pagina's met de volgende instellingen in het dialoogvenster **VIRTUEEL netwerk maken** . 

  	|Pagina|Instellingen|
|---|---|
|Virtueel netwerkdetails|**NAAM = ContosoNET**<br/>**REGIO West VS =**|
|DNS-Servers en VPN-verbindingen|Geen|
|Virtueel netwerk adresruimten|Instellingen worden weergegeven in het screenshot hieronder: ![Virtueel netwerk maken](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784620.png)|

1. Maak vervolgens de VM die u als de domeincontroller (DC gebruiken wilt). Klik op **Nieuw** , en vervolgens **berekenen**, en vervolgens **virtuele Machine**, en vervolgens **Uit galerie**, zoals hieronder wordt weergegeven.

    ![Een VM maken](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784621.png)

1. Configureer een nieuwe VM door door de pagina's met de volgende instellingen in het dialoogvenster **een virtuele MACHINE maken** . 

  	|Pagina|Instellingen|
|---|---|
|Selecteer het besturingssysteem van de virtuele machine|Windows Server 2012 R2 Datacenter|
|Configuratie van de virtuele machine|**Versie datum** = (meest recente)<br/>**Naam van de virtuele MACHINE** = ContosoDC<br/>**Laag** = standaard<br/>**Grootte** = A2 (2 cores)<br/>**Nieuwe gebruikersnaam** = AzureAdmin<br/>**Nieuw wachtwoord** = Contoso! 000<br/>**Bevestig** = Contoso! 000|
|Configuratie van de virtuele machine|**CLOUD SERVICE** = een nieuwe wolk service maken<br/>**CLOUD SERVICE DNS-naam** = naam van een unieke cloud-service<br/>**DNS-naam** een unieke naam = (ex: ContosoDC123)<br/>**Regio/AFFINITEIT groep/VIRTUEEL netwerk** = ContosoNET<br/>**VIRTUEEL NETWERKSUBNETTEN** = Back(10.10.2.0/24)<br/>**Opslag ACCOUNT** = gebruik een automatisch gegenereerde opslag account<br/>**Beschikbaar** (geen) =|
|Opties voor virtuele machine|Standaardinstellingen gebruiken|

Zodra u klaar bent met het configureren van de nieuwe VM, wachten op de VM worden provsioned. Dit proces duurt enige tijd en als u naar het tabblad **virtuele Machine** in de klassieke Azure portal klikt, ziet u de ContosoDC per fiets met ingang van **begin (creëren)** **gestopt**, **starten**, **uitvoeren (Provisioning)**en ten slotte **uitgevoerd**.

De DC-server is nu zijn ingericht. Vervolgens configureert u het domein van Active Directory op deze server DC.

## <a name="configure-the-domain-controller"></a>De domeincontroller configureren

In de volgende stappen kunt u de ContosoDC machine configureren als een domeincontroller voor corp.contoso.com.

1. Selecteer in het portal voor de **ContosoDC** -machine. Klik op **verbinden** om het openen van een RDP-bestand voor toegang tot extern bureaublad op het tabblad **Dashboard** .

    ![Verbinding maken met computer Vritual](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784622.png)

1. Meld u aan met uw geconfigureerde administrator-account (**\AzureAdmin**) en wachtwoord (**Contoso! 000**).

1. Standaard wordt het dashboard **Server Manager** weergegeven.

1. Klik op de koppeling **toevoegen, rollen en functies** op het dashboard.

    ![Server Explorer rollen toevoegen](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784623.png)

1. Selecteert de **volgende** totdat u in de sectie **Server-rollen** .

1. Selecteer de rollen voor **Active Directory Domain Services** en **DNS-Server** . Aanvullende functies vereist door deze rollen toevoegen wanneer u wordt gevraagd,

    >[AZURE.NOTE] U krijgt een waarschuwing dat er geen statisch IP-adres is validatie. Als u de configuratie test, klik op Doorgaan. Voor de productie scenario [gebruikt u PowerShell om het statische IP-adres van de machine domain controller instellen](./virtual-network/virtual-networks-reserved-private-ip.md).

    ![Dialoogvenster rollen toevoegen](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784624.png)

1. Klik op **volgende** totdat u bij de sectie **bevestiging** . Schakel het selectievakje **opnieuw opstarten van de doelserver automatisch indien nodig** .

1. Klik op **installeren**.

1. Nadat u de functies hebt geïnstalleerd, retourneren aan het dashboard van **Server Manager** .

1. De optie nieuwe **AD DS** in het linkerdeelvenster.

1. Klik op de koppeling **meer** op de gele balk met waarschuwing.

    ![AD DS-dialoogvenster op de DNS-Server VM](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784625.png)

1. In de kolom **actie** van het dialoogvenster **Details van alle Server-taak** , klikt u op **deze server tot domeincontroller promoveren**.

1. In de **Active Directory Domain Services-configuratiewizard**gebruikt de volgende waarden:

  	|Pagina|Instelling|
|---|---|
|Implementatieconfiguratie|**Een nieuw forest toevoegen** = geselecteerd<br/>**Hoofddomein** corp.contoso.com =|
|Domeincontrolleropties|**Wachtwoord** = Contoso! 000<br/>**Bevestig wachtwoord** = Contoso! 000|

1. Klik op **volgende** om door te gaan door de andere pagina's in de wizard. Controleer op de pagina **Controle van vereisten** ziet u het volgende bericht: **alle vereiste controles is doorgegeven**. Houd er rekening mee dat eventuele toepasselijke waarschuwingsberichten moeten worden gecontroleerd, maar het is mogelijk om door te gaan met de installatie.

1. Klik op **installeren**. De **ContosoDC** virtuele machine wordt automatisch opnieuw opgestart.

## <a name="configure-domain-accounts"></a>Domeinaccounts configureren

De volgende procedure configureert u de accounts Active Directory (AD) voor later gebruik.

1. Meld u weer in de machine **ContosoDC** .

1. Selecteer **Extra** en klik vervolgens op **Beheerderscentrum voor Active Directory**in **Serverbeheer** .

    ![Active Directory Administrative Center](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784626.png)

1. In het **Beheerderscentrum voor Active Directory** selecteert **corp (lokaal)** in het linkerdeelvenster.

1. In het rechter deelvenster **taken** , selecteer **Nieuw** en klik vervolgens op **gebruiker**. Gebruik de volgende instellingen:

  	|Instelling|Waarde|
|---|---|
|**Voornaam**|Installeren|
|**SamAccountName van gebruiker**|Installeren|
|**Wachtwoord**|Contoso! 000|
|**Bevestig wachtwoord**|Contoso! 000|
|**Andere opties voor wachtwoord**|Geselecteerd|
|**Wachtwoord verloopt nooit**|Gecontroleerd|

1. Klik op **OK** om het maken van de gebruiker **installeren** . Deze rekening wordt gebruikt voor het configureren van het failover-cluster en de van beschikbaarheidsgroep.

1. Twee extra gebruikers met dezelfde stappen te maken: **CORP\SQLSvc1** en **CORP\SQLSvc2**. Deze accounts worden gebruikt voor de SQL Server-exemplaren. Vervolgens moet u **CORP\Install** geeft u de benodigde machtigingen voor het configureren van Windows Service Failover Clustering (WSFC).

1. Selecteer in het **Beheerderscentrum voor Active Directory** **corp (lokaal)** in het linkerdeelvenster. Klik in **het rechter deelvenster,** Klik op **Eigenschappen**.

    ![Gebruikerseigenschappen CORP](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784627.png)

1. Selecteer **extensies**en klik vervolgens op de knop **Geavanceerd** op het tabblad **beveiliging** .

1. In het dialoogvenster **Geavanceerde beveiligingsinstellingen voor corp** . Klik op **toevoegen**.

1. Klik op **een principal selecteren**. Zoek naar **CORP\Install**. Klik op **OK**.

1. Selecteer de machtigingen **alle eigenschappen lezen** en **Computerobjecten maken** .

    ![Gebruikersmachtigingen Corp](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784628.png)

1. Klik op **OK**en klik vervolgens op **OK** . Sluit het venster corp eigenschappen.

Nu dat u klaar bent met het configureren van Active Directory en de gebruikersobjecten die u maakt drie SQL Server VMs en voeg ze toe aan dit domein.

## <a name="create-the-sql-server-vms"></a>Maak de SQL Server-VMs

Maak drie VMs, met inbegrip van een clusterknooppunt WSFC en twee SQL Server VMs. Elk van de VMs maken, gaat u terug naar de klassieke Azure portal, klikt u op **Nieuw**, **berekenen**, **virtuele Machine**, en vervolgens **Uit de galerie**. Vervolgens de sjablonen in de volgende tabel gebruiken om te maken van het VMs.

|Pagina|VM1|VM2|VM3|
|---|---|---|---|
|Selecteer het besturingssysteem van de virtuele machine|**Windows Server 2012 R2 Datacenter**|**SQL Server 2014 RTM Enterprise**|**SQL Server 2014 RTM Enterprise**|
|Configuratie van de virtuele machine|**Versie datum** = (meest recente)<br/>**Naam van de virtuele MACHINE** = ContosoWSFCNode<br/>**Laag** = standaard<br/>**Grootte** = A2 (2 cores)<br/>**Nieuwe gebruikersnaam** = AzureAdmin<br/>**Nieuw wachtwoord** = Contoso! 000<br/>**Bevestig** = Contoso! 000|**Versie datum** = (meest recente)<br/>**Naam van de virtuele MACHINE** = ContosoSQL1<br/>**Laag** = standaard<br/>**Grootte** = A3 (4 cores)<br/>**Nieuwe gebruikersnaam** = AzureAdmin<br/>**Nieuw wachtwoord** = Contoso! 000<br/>**Bevestig** = Contoso! 000|**Versie datum** = (meest recente)<br/>**Naam van de virtuele MACHINE** = ContosoSQL2<br/>**Laag** = standaard<br/>**Grootte** = A3 (4 cores)<br/>**Nieuwe gebruikersnaam** = AzureAdmin<br/>**Nieuw wachtwoord** = Contoso! 000<br/>**Bevestig** = Contoso! 000|
|Configuratie van de virtuele machine|**CLOUD SERVICE** = eerder gemaakte unieke Cloud Service DNS-naam (ex: ContosoDC123)<br/>**Regio/AFFINITEIT groep/VIRTUEEL netwerk** = ContosoNET<br/>**VIRTUEEL NETWERKSUBNETTEN** = Back(10.10.2.0/24)<br/>**Opslag ACCOUNT** = gebruik een automatisch gegenereerde opslag account<br/>**Beschikbaar** maken een beschikbaarheid = instellen<br/>**Beschikbaarheid SET NAME** = SQLHADR|**CLOUD SERVICE** = eerder gemaakte unieke Cloud Service DNS-naam (ex: ContosoDC123)<br/>**Regio/AFFINITEIT groep/VIRTUEEL netwerk** = ContosoNET<br/>**VIRTUEEL NETWERKSUBNETTEN** = Back(10.10.2.0/24)<br/>**Opslag ACCOUNT** = gebruik een automatisch gegenereerde opslag account<br/>**Beschikbaarheid instellen** = SQLHADR (u kunt ook de beschikbaarheid ingesteld nadat de computer is gemaakt. Alle drie de machines moeten worden toegewezen aan de set SQLHADR beschikbaarheid.)|**CLOUD SERVICE** = eerder gemaakte unieke Cloud Service DNS-naam (ex: ContosoDC123)<br/>**Regio/AFFINITEIT groep/VIRTUEEL netwerk** = ContosoNET<br/>**VIRTUEEL NETWERKSUBNETTEN** = Back(10.10.2.0/24)<br/>**Opslag ACCOUNT** = gebruik een automatisch gegenereerde opslag account<br/>**Beschikbaarheid instellen** = SQLHADR (u kunt ook de beschikbaarheid ingesteld nadat de computer is gemaakt. Alle drie de machines moeten worden toegewezen aan de set SQLHADR beschikbaarheid.)|
|Opties voor virtuele machine|Standaardinstellingen gebruiken|Standaardinstellingen gebruiken|Standaardinstellingen gebruiken|

<br/>

>[AZURE.NOTE] De vorige configuratie stelt standaard laag virtuele machines, omdat BASIC tier systemen bieden geen ondersteuning voor taakverdeling eindpunten die zijn vereist voor het maken van een groep beschikbaarheid listeners later. Ook zijn de grootte van de machine hier voorgesteld bedoeld voor het testen van de beschikbaarheid van groepen in Azure VMs. Zie de aanbevelingen voor de grootte van SQL Server-computer en de configuratie in [Aanbevolen procedures voor SQL Server in Azure virtuele Machines prestaties](virtual-machines-windows-sql-performance.md)voor de beste prestaties op de werkbelasting van de productie.

Zodra de drie VMs volledig zijn ingericht, moet u ze toevoegen aan het domein **corp.contoso.com** en CORP\Install beheerdersrechten verlenen aan de machines. Gebruik hiervoor de volgende stappen uit voor elk van de drie VMs.

1. Ten eerste voorkeur adres van de DNS-server wijzigen. Begin met het downloaden (van elke VM remote desktop RDP)-bestand naar uw lokale map door de VM te selecteren in de lijst en klikt u op de knop **verbinding maken** . Selecteer een VM, klikt u ergens maar de eerste cel in de rij, zoals hieronder wordt weergegeven.

    ![Het RDP-bestand downloaden](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC664953.jpg)

1. Start het RDP-bestand dat u hebt gedownload en log in met uw geconfigureerde VM administrator-account (**BUILTIN\AzureAdmin**) en wachtwoord (**Contoso! 000**).

1. Zodra u bent aangemeld, ziet u het dashboard van **Server Manager** . Klik op **Lokale Server** in het linkerdeelvenster.

1. Selecteer de koppeling **IPv4-adres toegewezen door DHCP, IPv6 ingeschakeld** .

1. Selecteer het netwerkpictogram in het venster **Netwerkverbindingen** .

    ![De VM voorkeurs-DNS-Server wijzigen](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784629.png)

1. Klik op de opdrachtbalk en **de instellingen van deze verbinding wijzigen** (afhankelijk van de grootte van het venster wellicht te klikken op de dubbele pijl naar rechts om deze opdracht wordt weergegeven).

1. Selecteer **Internet Protocol versie 4 (TCP/IPv4)** en klik op Eigenschappen.

1. Selecteer Gebruik de volgende DNS-server adressen en **10.10.2.4** in de **voorkeurs-DNS-server**opgeven.

1. Het adres **10.10.2.4** is het adres dat is toegewezen aan een VM in het subnet van de 10.10.2.0/24 in een virtueel netwerk van Azure en de dat VM **ContosoDC**is. Gebruik de **nslookup-contosodc** om te controleren of de **ContosoDC**van IP-adres, in de MS-DOS-prompt, zoals hieronder wordt weergegeven.

    ![NSLOOKUP gebruiken om IP-adres voor DC](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC664954.jpg)

1. Klik op O**K** en vervolgens op **sluiten** als de wijzigingen wilt doorvoeren. U bent nu lid van de VM voor **corp.contoso.com**.

1. Klik op de koppeling van de **werkgroep** terug in het venster **Lokale Server** .

1. Klik in het gedeelte **Computernaam** op **wijzigen**.

1. Schakel het selectievakje voor het **domein** en **corp.contoso.com** typt in het tekstvak. Klik op **OK**.

1. Geef de referenties voor de domein administrator-account (**CORP\AzureAdmin**) en het wachtwoord in het dialoogvenster **Windows-beveiliging** -pop-up (**Contoso! 000**).

1. Klik op **OK**wanneer het bericht 'Welkom bij het domein corp.contoso.com' wordt weergegeven.

1. Klik op **sluiten**en klik vervolgens op **Nu opnieuw opstarten** in het dialoogvenster pop-upmenu.

### <a name="add-the-corpinstall-user-as-an-administrator-on-each-vm"></a>De Corp\Install-gebruiker toevoegen als beheerder op elk VM:

1. Wacht totdat de VM opnieuw is opgestart en vervolgens het RDP-bestand opnieuw aan te melden bij de VM met de account **BUILTIN\AzureAdmin** starten.

1. Selecteer **Extra**in **Serverbeheer** en klik vervolgens op **Computerbeheer**.

    ![Computerbeheer](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784630.png)

1. **Lokale gebruikers**en groepen in het venster **Computerbeheer** en selecteer vervolgens de **groepen**.

1. Dubbelklik op de groep **Administrators** .

1. Klik op de knop **toevoegen** in het dialoogvenster **Eigenschappen voor Administrators** .

1. Voer de **CORP\Install**van de gebruiker en klik vervolgens op **OK**. Wanneer u wordt gevraagd om referenties, gebruikt u de **AzureAdmin** rekening met de **Contoso! 000** wachtwoord.

1. Klik op **OK** om te sluiten van het dialoogvenster **Eigenschappen van de beheerder** .

### <a name="add-the-failover-clustering-feature-to-each-vm"></a>Het **Onderdeel Failover** clustering op elke VM toevoegen.

1. Klik op **functies toevoegen en onderdelen**in het dashboard **Server Manager** .

1. In de **functies toevoegen en de Wizard functies**, klik op **volgende** totdat u op de pagina **onderdelen** .

1. Selecteer **Failover Clustering**. Andere afhankelijke functies toevoegen wanneer u wordt gevraagd,

    ![Onderdeel Failover Clustering op VM toevoegen](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784631.png)

1. Klik op **volgende**en klik vervolgens op de pagina **bevestiging** op **installeren** .

1. Wanneer de functie **Failover Clustering** installatie is voltooid, klikt u op **sluiten**.

1. Meld u bij de VM.

1. Herhaal de stappen in deze sectie voor alle drie servers-- **ContosoWSFCNode**, **ContosoSQL1**en **ContosoSQL2**.

De SQL Server-VMs nu zijn ingericht en uitgevoerd, maar ze worden geïnstalleerd met SQL Server met de standaardopties.

## <a name="create-the-wsfc-cluster"></a>De WSFC Cluster maken

In dit gedeelte maakt u het cluster WSFC die als host fungeert voor de beschikbaarheidsgroep u later maakt. Nu moet u elk van de drie VMs in het cluster WSFC wilt gebruiken de volgende hebben gedaan:

- Volledig ingericht in Azure

- Gekoppelde VM aan het domein

- **CORP\Install** zijn toegevoegd aan de lokale groep Administrators

- Het onderdeel Failover Clustering toegevoegd

Deze zijn vereisten op elke VM voordat u deze aan het cluster WSFC deelnemen kunt.

Let er ook op dat de Azure virtueel netwerk niet meer op dezelfde manier als een netwerk op gebouwen werkt. U moet het cluster maken in de volgende volgorde:

1. Maak een cluster met één knooppunt op een van de knooppunten (**ContosoSQL1**).

1. Wijzig het cluster-IP-adres naar een ongebruikt IP-adres (**10.10.2.101**).

1. De clusternaam on line te brengen.

1. Voeg de andere knooppunten (**ContosoSQL2** en **ContosoWSFCNode**).

Volg de volgende stappen om deze taken uitvoeren die het cluster volledig geconfigureerd.

1. Start het RDP-bestand voor **ContosoSQL1** en meld u aan met de account voor een **CORP\Install**.

1. Selecteer **Extra**in het dashboard **Server Manager** en klik op **Failover-clusterbeheer**.

1. In het linkerdeelvenster met de rechtermuisknop op **Failover-clusterbeheer**en vervolgens klikt u op **een Cluster maken**, zoals hieronder wordt weergegeven.

    ![Cluster maken](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784632.png)

1. Maak een cluster met één knooppunt door door de pagina's met de volgende instellingen in de Wizard Cluster maken:

  	|Pagina|Instellingen|
|---|---|
|Voordat u begint|Standaardinstellingen gebruiken|
|Servers selecteren|Typ **ContosoSQL1** in **Enter servernaam** en klik op **toevoegen**|
|Validatiewaarschuwing|Selecteer **nr. ik geen ondersteuning van Microsoft voor dit cluster vereist en daarom niet wilt dat de validatietests uitvoeren. Als ik op Volgende klikt, doorgaan met het maken van het cluster**.|
|Toegangspunt voor het beheer van het Cluster|Type **Cluster1** in **de naam van Cluster**|
|Bevestiging|Standaardinstellingen gebruiken tenzij u opslagruimtes. Zie de opmerking onder deze tabel.|

    >[AZURE.WARNING] Als u van [Opslagruimten](https://technet.microsoft.com/library/hh831739), die meerdere schijven in opslagpools groepen gebruikmaakt, moet u het selectievakje **alle in aanmerking komende opslag aan het cluster toevoegen** op de pagina **bevestiging** uitschakelen. Als u geen Schakel deze optie uit wordt de virtuele schijven worden losgekoppeld tijdens de clustering. Als gevolg hiervan ook niet weergegeven in Schijfbeheer of Explorer totdat de opslagruimten van het cluster worden verwijderd en vervolgens opnieuw met PowerShell.

1. **Failoverclusterbeheer**Vouw in het linkerdeelvenster en klik op **Cluster1.corp.contoso.com**.

1. In het middelste deelvenster, Ga naar sectie **Clusterbronnen Core** en vouw de **naam: Clutser1** details. Ziet u zowel de **naam** als de bronnen **IP-adres** in de status **mislukt** . De IP-adresbron kan niet on line worden gebracht omdat het cluster toegewezen is aan hetzelfde IP-adres als die van de machine zelf, die een dubbel adres is.

1. Klik met de rechtermuisknop op de defecte **IP-** adresbron en klik vervolgens op **Eigenschappen**.

    ![Eigenschappen van cluster](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784633.png)

1. Selecteer **Statisch IP-adres** en **10.10.2.101** opgeven in het tekstvak adres. Klik vervolgens op **OK**.

1. Met de rechtermuisknop in het gedeelte **Clusterbronnen Core** **naam: Cluster1** en klik op **On line brengen**. Wacht vervolgens totdat beide bronnen on line zijn. Wanneer de naam-clusterbron weer on line komt, de DC-server wordt bijgewerkt met een nieuwe AD-account. Deze advertentie account wordt later uit te voeren de beschikbaarheid groep Cluster-service gebruikt.

1. U kunt ten slotte de resterende knooppunten toevoegen aan het cluster. Klik met de rechtermuisknop op **Cluster1.corp.contoso.com** en klik op **Knooppunt toevoegen**, zoals hieronder wordt weergegeven in de structuur van de browser.

    ![Knooppunt aan het Cluster toevoegen](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784634.png)

1. Klik op **volgende**in de **Wizard knooppunt toevoegen**. Op de pagina **Servers selecteren** , **ContosoSQL2** en **ContosoWSFCNode** aan de lijst toevoegen door de naam van de server in **Enter naam** te typen en vervolgens op **toevoegen**. Wanneer u klaar bent, klikt u op **volgende**.

1. Klik op **Nee** in de pagina **Validatiewaarschuwing** (in een productiescenario voor als u de validatietests uitvoert). Klik vervolgens op **volgende**.

1. Klik op **volgende** als u de knooppunten toevoegen in **het bevestigingsvenster** .

    >[AZURE.WARNING] Als u van [Opslagruimten](https://technet.microsoft.com/library/hh831739), die meerdere schijven in opslagpools groepen gebruikmaakt, moet u het selectievakje **alle in aanmerking komende opslag aan het cluster toevoegen** uitschakelen. Als u geen Schakel deze optie uit wordt de virtuele schijven worden losgekoppeld tijdens de clustering. Als gevolg hiervan ook niet weergegeven in Schijfbeheer of Explorer totdat de opslagruimten van het cluster worden verwijderd en vervolgens opnieuw met PowerShell.

1. Wanneer de knooppunten aan het cluster worden toegevoegd, klikt u op **Voltooien**. Failover-clusterbeheer wordt nu weergegeven dat het cluster heeft drie knooppunten en plaatst u deze in de container van de **knooppunten** .

1. Meld u bij de extern bureaublad-sessie.

## <a name="prepare-the-sql-server-instances-for-availability-group"></a>De SQL Server-exemplaren voorbereiden voor beschikbaarheid groep

In deze sectie, wordt u op zowel **ContosoSQL1** als **contosoSQL2**de volgende doen:

- Een aanmelding voor **NT AUTHORITY\System** toevoegen met een vereiste machtigingen instellen met het standaardexemplaar van SQL Server

- **CORP\Install** als een rol sysadmin toevoegen met het standaardexemplaar van SQL Server

- Open de firewall voor externe toegang van SQL Server

- De altijd op beschikbaarheidsgroepen inschakelen

- Wijzigen in de SQL Server-serviceaccount **CORP\SQLSvc1** en **CORP\SQLSvc2**, respectievelijk

Deze acties kunnen in willekeurige volgorde worden uitgevoerd. Niettemin, de volgende stappen doorlopen die ze in volgorde. Volg de stappen voor zowel **ContosoSQL1** als **ContosoSQL2**:

1. Als u zich niet hebt aangemeld bij de sessie van extern bureaublad voor VM, moet u dat nu doen.

1. Start de RDP-bestanden voor **ContosoSQL1** en **ContosoSQL2** en meld u aan als **BUILTIN\AzureAdmin**.

1. **NT AUTHORITY\System** eerst toevoegen aan de SQL Server-aanmeldingen en de benodigde machtigingen. Start **SQL Server Management Studio**.

1. Klik op **verbinding maken met** verbinding maken met het standaardexemplaar van SQL Server.

1. In **Object Explorer** **beveiliging**en vervolgens **Logins**uit.

1. Klik met de rechtermuisknop op de **NT AUTHORITY\System** aanmelding en klik op **Eigenschappen**.

1. Voor de lokale server op de pagina **Securables** **verlenen** voor de volgende machtigingen selecteren en klik op **OK**.

    - Beschikbaarheid groepen wijzigen

    - Verbinding maken met SQL

    - Status weergeven

1. Vervolgens voegt u **CORP\Install** als een rol **sysadmin** met het standaardexemplaar van SQL Server. In **Object Explorer**met de rechtermuisknop op **aanmeldingen** en klik op **Nieuwe aanmelding**.

1. Typ **CORP\Install** in de **aanmeldingsnaam**.

1. Selecteer op de pagina **Serverrollen** **sysadmin**. Klik vervolgens op **OK**. Zodra de aanmelding is gemaakt, kunt u deze zien door **aanmeldingen** in **Object Explorer**uitbreiden.

1. Vervolgens maakt u een firewallregel voor SQL Server. Start vanaf **het startscherm van** **Windows Firewall met geavanceerde beveiliging**.

1. Selecteer in het linkerdeelvenster **Binnenkomende regels**. Klik op **Nieuwe regel**in het rechterdeelvenster.

1. Op de pagina **Regeltype** **programma**selecteren en klik op **volgende**.

1. **Programma** op de pagina Selecteer **Dit programmapad** en typ **%ProgramFiles%\Microsoft SQL Server\MSSQL12. MSSQLSERVER\MSSQL\Binn\sqlservr.exe** in het tekstvak (als u deze aanwijzingen volgt, maar met behulp van SQL Server 2012, de SQL Server-map **MSSQL11 wordt. MSSQLSERVER**). Klik vervolgens op **volgende**.

1. **Actie** op de pagina behouden **de verbinding toestaan** is geselecteerd en klik op **volgende**.

1. Accepteer de standaardinstellingen op de pagina **profiel** en klik op **volgende**.

1. Op de pagina **de naam** opgeven van de naam van een regel, zoals **SQL Server (regel)** in het tekstvak **naam** klik vervolgens op **Voltooien**.

1. Schakel vervolgens de functie **Altijd op beschikbaarheidsgroepen** . Start vanaf **het startscherm van** **SQL Server Configuration Manager**.

1. Klik in de browser klikt u op **SQL Server-Services**, en vervolgens met de rechtermuisknop op de service **SQL Server (MSSQLSERVER)** en op **Eigenschappen**.

1. Klik op het tabblad **Altijd in hoge mate van beschikbaarheid** en vervolgens selecteert u **Inschakelen altijd op beschikbaarheidsgroepen**, zoals hieronder wordt weergegeven en klik op **toepassen**. Klik op **OK** in het pop-upvenster en sluit het eigenschappenvenster niet nog. U kunt de SQL Server-service wordt opnieuw opgestart nadat u de service-account wijzigen.

    ![Schakel altijd op beschikbaarheidsgroepen](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665520.gif)

1. Vervolgens wijzigt u de SQL Server-serviceaccount. Klik op het tabblad **Aanmelden** en vervolgens **CORP\SQLSvc1** (voor **ContosoSQL1**) of **CORP\SQLSvc2** (voor **ContosoSQL2**) typen in het vak **Accountnaam**en vervolgens in te vullen en Bevestig het wachtwoord en klik op **OK**.

1. In het pop-upvenster klikt u op **Ja** om de SQL Server-service opnieuw te starten. Nadat u de SQL Server-service opnieuw wordt gestart, zijn de wijzigingen in het eigenschappenvenster van kracht.

1. Meld u bij de VMs.

## <a name="create-the-availability-group"></a>De groep beschikbaar maken

U bent nu gereed voor het configureren van een groep van beschikbaarheid. Hieronder volgt een overzicht van wat u doet:

- Maak een nieuwe database (**MyDB1**) op **ContosoSQL1**

- Zowel een volledige back-up en een transactie back-up van de database

- Het volledige herstel en back-ups op **ContosoSQL2** registreren met de optie **NORECOVERY**

- De beschikbaarheidsgroep (**AG1**) met synchrone commit, automatische failover en leesbare secundaire replica's maken

### <a name="create-the-mydb1-database-on-contososql1"></a>Maak de database MyDB1 op ContosoSQL1:

1. Als u hebt niet al aangemeld bij de extern bureaublad-sessies voor **ContosoSQL1** en **ContosoSQL2**, moet u dat nu doen.

1. Start het RDP-bestand voor **ContosoSQL1** en meld u aan als **CORP\Install**.

1. In **File Explorer**onder * *C:\**, maakt u een map met de naam * *back-up**. U gebruikt deze directory, gebruiken om een back-up maken en terugzetten van uw database.

1. Met de rechtermuisknop op de nieuwe map, wijst u **delen met**en klik vervolgens op **specifieke personen**, zoals hieronder wordt weergegeven.

    ![Maak een back-upmap](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665521.gif)

1. **CORP\SQLSvc1** toevoegen en geef deze de machtiging **Lezen/schrijven** en vervolgens toevoegen **CORP\SQLSvc2** geven de machtiging **lezen** , zoals hieronder wordt weergegeven en klik vervolgens op **delen**. Zodra het proces delen van bestanden voltooid is, klikt u op **Gereed**.

    ![Machtigingen voor de back-upmap](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665522.gif)

1. Maak vervolgens de database. Start **SQL Server Management Studio**uit het menu **Start** en klik op **verbinding maken** met verbinding maken met het standaardexemplaar van SQL Server.

1. In **Object Explorer**met de rechtermuisknop op **Databases** en klik op **Nieuwe Database**.

1. Typ **MyDB1**in het vak **naam van de Database**en klikt u op **OK**.

### <a name="take-a-full-backup-of-mydb1-and-restore-it-on-contososql2"></a>Een volledige back-up van MyDB1 nemen en terugzetten op ContosoSQL2:

1. Vervolgens neemt u een volledige back-up van de database. In **Object Explorer**Vouw **Databases**, en vervolgens met de rechtermuisknop op **MyDB1**, en vervolgens wijst **taken**en klik vervolgens op **Back-Up**.

1. In de sectie **bron** **type back-up** ingesteld op **volledig**te behouden. Klik op **verwijderen** om het standaardpad voor back-up te verwijderen in de sectie **bestemming** .

1. Klik op **toevoegen**in de sectie **bestemming** .

1. Typ in het tekstvak **bestandsnaam** ** \\ContosoSQL1\backup\MyDB1.bak**. Vervolgens klikt u op **OK**en klik vervolgens op **OK** om te back-up van de database. Als de back-up is voltooid, klikt u op **OK** om te sluiten van het dialoogvenster.

1. Vervolgens neemt u een transactielogboek back-up van de database. In **Object Explorer**Vouw **Databases**, en vervolgens met de rechtermuisknop op **MyDB1**, en vervolgens wijst **taken**en klik vervolgens op **Back-Up**.

1. Selecteer in het vak type **back-up** **Transactielogboek**. Houd het pad van het **doel** ingesteld op die u eerder opgegeven en klik op **OK**. Nadat de back-up is voltooid, klikt u nogmaals op **OK** .

1. Vervolgens kunt u de volledige en transactie log back-ups op de **ContosoSQL2**terugzetten. Start het RDP-bestand voor **ContosoSQL2** en meld u aan als **CORP\Install**. Sluit de extern bureaublad-sessies voor **ContosoSQL1** .

1. Start **SQL Server Management Studio**uit het menu **Start** en klik op **verbinding maken** met verbinding maken met het standaardexemplaar van SQL Server.

1. Het **Object Explorer**met de rechtermuisknop op **Databases** en klik op **Database terugzetten**.

1. In de sectie **bron** **apparaat**selecteren en klik op de **...** de knop.

1. **Selecteer back-apparaten**, klik op **toevoegen**.

1. Typ in locatie van back-up \\ContosoSQL1\backup, klikt u op vernieuwen en vervolgens selecteert u MyDB1.bak, en vervolgens klikt u op OK en klik op OK. U ziet nu de volledige back-up en de back-up in de back-up ingesteld voor het deelvenster herstellen.

1. Ga naar de pagina opties en vervolgens selecteert u wellicht herstellen in herstel staat en klik op OK als u de database herstellen. Nadat de herstelbewerking is voltooid, klikt u op OK.

### <a name="create-the-availability-group"></a>De beschikbaarheidsgroep maken:

1. Ga terug naar de extern bureaublad-sessie met **ContosoSQL1**. In **Object Explorer** in SSMS **Altijd op hoge beschikbaarheid** met de rechtermuisknop en klik op **Wizard Nieuwe beschikbaarheid**, zoals hieronder wordt weergegeven.

    ![De Wizard Nieuwe groep beschikbaarheid starten](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665523.gif)

1. Klik in de pagina **Introductie** op **volgende**. Op de pagina **Beschikbaarheid groepsnaam opgeven** **AG1** typt in **naam van beschikbaarheid**en klik op **volgende** opnieuw.

    ![Wizard Nieuwe AG AG opgeven](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665524.gif)

1. **Databases selecteren** op de pagina Selecteer **MyDB1** en klik op **volgende**. De database voldoet aan de vereisten voor een beschikbaarheidsgroep omdat u ten minste één volledige back-up hebt gemaakt voor het beoogde primaire replica.

    ![Wizard Nieuwe AG Databases selecteren](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665525.gif)

1. Klik op de pagina **Geef replica's** **Replica toevoegen**.

    ![Wizard Nieuwe AG, replica's opgeven](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665526.gif)

1. Het dialoogvenster **verbinding maken met Server** verschijnt. Typ **ContosoSQL2** in de **naam van de Server**en klik vervolgens op **verbinden**.

    ![AG-Wizard Nieuwe verbinding maken met Server](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665527.gif)

1. Terug op de pagina **Geef replica's** , nu ziet u **ContosoSQL2** weergegeven in het **Beschikbare replica's**. Configureer de replica's zoals hieronder wordt weergegeven. Wanneer u klaar bent, klikt u op **volgende**.

    ![Wizard Nieuwe AG, geeft u replica's (voltooid)](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665528.gif)

1. **Selecteer eerste synchronisatie van gegevens** op de pagina Selecteer **alleen deelnemen** en op **volgende**. U hebt al uitgevoerd gegevenssynchronisatie handmatig wanneer u vond de volledige en transactie back-ups op **ContosoSQL1** en de bestanden zijn teruggezet op **ContosoSQL2**. In plaats daarvan kunt u niet de back-up en terugzetten van uw database en selecteer **volledig** om de beschikbaarheid van Wizard nieuwe synchronisatie van gegevens voor u uitvoeren. Dit is echter niet aanbevolen voor zeer grote databases die zijn gevonden in sommige ondernemingen.

    ![Wizard Nieuwe AG, selecteer eerste gegevenssynchronisatie](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665529.gif)

1. Klik op **volgende**op de pagina **validatie** . Deze pagina moet uitzien hieronder. Er is een waarschuwing voor de configuratie van de listener omdat een groep beschikbaarheid listener niet is geconfigureerd. Omdat in deze zelfstudie wordt een listener niet geconfigureerd, kunt u deze waarschuwing negeren. Zie [een listener ILB voor altijd op beschikbaarheidsgroepen in Azure configureren](virtual-machines-windows-classic-ps-sql-int-listener.md)voor meer informatie over het configureren van de listener na het voltooien van deze zelfstudie.

    ![Wizard Nieuwe AG, validatie](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665530.gif)

1. Klikt u op **Voltooien**op de pagina **Samenvatting** en geduld terwijl de wizard configureert u de nieuwe groep van beschikbaarheid. In de pagina **voortgang van** klikt u op **meer informatie** om de gedetailleerde voortgang weer te geven. Nadat de wizard voltooid is, controleren **de pagina om te controleren dat de van beschikbaarheidsgroep is gemaakt, zoals hieronder wordt weergegeven en klik vervolgens op **sluiten** om de wizard te sluiten** .

    ![Resultaten van de Wizard Nieuwe AG](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665531.gif)

1. In **Object Explorer**Vouw **Altijd in hoge mate van beschikbaarheid**en **Beschikbaarheid**van de groepen. U ziet nu de nieuwe beschikbaarheidsgroep in deze container. Klik met de rechtermuisknop **AG1 (primaire)** en klik op **Dashboard weergeven**.

    ![AG Dashboard weergeven](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665532.gif)

1. Het **Altijd op Dashboard** moet uitzien Zie hieronder. De replica's, de failover-modus van elke replica en de synchronisatiestatus weergegeven.

    ![AG-Dashboard](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665533.gif)

1. Terug naar de **Server Manager**, selecteert u **gereedschappen**en start vervolgens **Failoverclusterbeheer**.

1. Vouw **Cluster1.corp.contoso.com**uit en vouw **Services en toepassingen**. Selecteer **rollen** en de rol van **AG1** beschikbaarheid groep zijn gemaakt. Houd er rekening mee dat AG1 geen IP-adres door welke database clients verbinding met de beschikbaarheidsgroep maken kunnen omdat u een listener niet hebt geconfigureerd. U kunt rechtstreeks naar het primaire knooppunt voor lezen / schrijven bewerkingen en het tweede knooppunt voor query's alleen-lezen.

    ![AG in Failover-clusterbeheer](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665534.gif)

>[AZURE.WARNING] Probeer geen failover van de groep van de beschikbaarheid van het Failover-Cluster Manager. Alle failover-bewerkingen moeten worden uitgevoerd binnen **Altijd op Dashboard** in SSMS. Zie [beperkingen op met behulp van de WSFC van Failover-clusterbeheer met beschikbaarheidsgroepen](https://msdn.microsoft.com/library/ff929171.aspx)voor meer informatie.

## <a name="next-steps"></a>Volgende stappen
U hebt nu met succes geïmplementeerd SQL Server altijd op door het maken van een beschikbaarheidsgroep in Azure. Zie [een listener ILB voor altijd op beschikbaarheidsgroepen in Azure configureren](virtual-machines-windows-classic-ps-sql-int-listener.md)configureren een listener voor deze beschikbaarheidsgroep.

Zie voor meer informatie over het gebruik van SQL Server in Azure, [SQL Server Azure virtuele Machines](virtual-machines-windows-sql-server-iaas-overview.md).
