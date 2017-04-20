<properties
    pageTitle="Inrichten van een SQL Server virtuele Machine | Microsoft Azure"
    description="Maak en verbinding maken met een SQL Server virtuele machine in Azure met behulp van de Portal. In deze zelfstudie wordt de Resource Manager-modus."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    editor=""
    manager="jhubbard"
    tags="azure-resource-manager" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="09/21/2016"
    ms.author="jroth" />

# <a name="provision-a-sql-server-virtual-machine-in-the-azure-portal"></a>Inrichten van een SQL Server virtuele machine in de Portal Azure

> [AZURE.SELECTOR]
- [Portal](virtual-machines-windows-portal-sql-server-provision.md)
- [PowerShell](virtual-machines-windows-ps-sql-create.md)

Deze end-to-end-zelfstudie wordt beschreven hoe u met behulp van de Portal Azure inrichten van een virtuele machine waarop SQL Server wordt uitgevoerd.

De galerie Azure VM (virtual machine) bevat verschillende afbeeldingen met Microsoft SQL Server. U kunt met enkele muisklikken, selecteer een van de VM SQL-afbeeldingen in de galerie en inrichten in uw Azure-omgeving.

In deze zelfstudie wordt u:

- [Selecteer een SQL VM-afbeelding in de galerie](#select-a-sql-vm-image-from-the-gallery)
- [Configureren en maken van de VM](#configure-the-vm)
- [Open de VM met extern bureaublad](#open-the-vm-with-remote-desktop)
- [Extern verbinding maken met SQL Server](#connect-to-sql-server-remotely)

## <a name="select-a-sql-vm-image-from-the-gallery"></a>Selecteer een SQL VM-afbeelding in de galerie

1. Log in op de [Azure portal](https://portal.azure.com) met uw account.

    >[AZURE.NOTE] Als u niet een Azure-account hebt, gaat u naar [Azure gratis proefperiode](https://azure.microsoft.com/pricing/free-trial/).

1. Klik op **Nieuw**op de Azure portal. De portal wordt het **nieuwe** blad geopend. De SQL Server VM resources zijn in de groep van **virtuele Machines** van de markt.

1. Klik op de **virtuele Machines**in de blade **Nieuw** .

1. Overzicht van de beschikbare afbeeldingen, klikt u op **alle** in de blade **virtuele Machines** .

    ![Blade Azure virtuele Machines](./media/virtual-machines-windows-portal-sql-server-provision/azure-compute-blade.png)

1. Klik onder **databaseservers**, **SQL-Server**. Mogelijk moet omlaag schuiven om **Database-servers**te zoeken. Bekijk de beschikbare sjablonen voor SQL Server.

    ![Virtuele Machine SQL-afbeeldingen](./media/virtual-machines-windows-portal-sql-server-provision/virtual-machine-gallery-sql-server.png)

1. Elke sjabloon bevat een versie van SQL Server en een besturingssysteem. Selecteer een van deze afbeeldingen in de lijst. Bekijk de details bladeserver die een beschrijving van de afbeelding van de virtuele machine bevat.

    >[AZURE.NOTE] SQL VM-afbeeldingen bevatten de licentiekosten voor SQL Server in de prijs per minuut van de VM die u maakt. Er is een andere optie te brengen-uw-eigen-licentie (BYOL) en betaal slechts voor VM. De naam van het image worden voorafgegaan door {BYOL}. Zie [aan de slag met Azure virtuele Machines van SQL Server](virtual-machines-windows-sql-server-iaas-overview.md)voor meer informatie over deze optie.

1. Klik onder **Selecteer een implementatiemodel**, controleren dat **Bronbeheer** is ingeschakeld. Bronbeheer is het aanbevolen implementatiemodel voor nieuwe virtuele machines. Klik op **maken**.

    ![SQL-VM met Resource Manager maken](./media/virtual-machines-windows-portal-sql-server-provision/azure-compute-sql-deployment-model.png)

## <a name="configure-the-vm"></a>Configureer de VM
Er zijn vijf voor het configureren van een virtuele machine van SQL Server-blades.

| Stap               | Beschrijving                          |
|---------------------|-------------------------------|
| **Grondbeginselen**              | [Basisinstellingen configureren](#1-configure-basic-settings)      |
| **Grootte**                | [Kies de grootte van de virtuele machine](#2-choose-virtual-machine-size)   |
| **Instellingen**            | [Optionele functies configureren](#3-configure-optional-features)   |
| **SQL Server-instellingen** | [SQL server-instellingen configureren](#4-configure-sql-server-settings) |
| **Samenvatting**             | [Bekijk het overzicht](#5-review-the-summary)            |

## <a name="1-configure-basic-settings"></a>1. basisinstellingen configureren
Op het blad **Grondbeginselen** bevatten de volgende informatie:

* Voer een unieke **naam**voor virtuele machine.
* Geef een **gebruikersnaam** voor de lokale administrator-account op de VM. Deze account wordt ook toegevoegd aan **de SQL-Server voor de vaste serverrol sysadmin** .
* Een sterk **wachtwoord**opgeven.
* Als er meerdere abonnementen, Controleer of het abonnement juist voor de nieuwe VM.
* Typ een naam voor een nieuwe resourcegroep in de **resourcegroep** . U kunt ook een bestaande resource groep Klik op **Selecteer bestaande**. Een resourcegroep is een verzameling van verwante bronnen in Azure (virtuele machines, opslag rekeningen, virtuele netwerken, enz.).

    >[AZURE.NOTE] Een nieuwe resourcegroep is nuttig als u alleen testen of leren over SQL Server-implementaties in Azure. Nadat u klaar met de test bent, de resourcegroep automatisch verwijderen van de VM en alle resources die zijn gekoppeld aan deze resourcegroep verwijderen. Voor meer informatie over resourcegroepen overzicht [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).

* Selecteer een **locatie** voor deze installatie.
* Klik op **OK** als de instellingen wilt opslaan.

    ![SQL grondbeginselen Blade](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-basic.png)

## <a name="2-choose-virtual-machine-size"></a>2. Kies de grootte van de virtuele machine
Kies de grootte van een virtuele machine in de blade **Kies een grootte** van de stap **grootte** . Het blad wordt in eerste instantie machine aanbevolen grootte op basis van de sjabloon die u hebt geselecteerd. Deze ook maakt een schatting van de maandelijkse kosten voor de VM uitvoeren.

![Opties voor SQL-VM](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-vm-choose-a-size.png)

Voor de productie van werkbelasting, wij raden u aan de grootte van een virtuele machine die [Premie opslag](../storage/storage-premium-storage.md)ondersteunt. Als u geen behoefte aan dat niveau van prestaties hebt, gebruik de knop **Alles weergeven** , waarin alle opties voor computer. Zo kunt u een kleinere machine voor een ontwikkel- of testomgeving.

>[AZURE.NOTE] Zie voor meer informatie over virtuele machine formaten, [formaten voor virtuele machines](virtual-machines-windows-sizes.md). Zie [Aanbevolen procedures voor SQL Server in Azure virtuele Machines prestaties](virtual-machines-windows-sql-performance.md)voor overwegingen over SQL Server VM formaten.

Kies de grootte van uw computer en klik vervolgens op **selecteren**.

## <a name="3-configure-optional-features"></a>3. optionele functies configureren
Op het blad **Instellingen** Azure opslag, netwerken en het toezicht op voor de virtuele machine te configureren.

- Geef een **diskette typen** Standard of Premium (SSD) onder **opslag**. Premium-opslagruimte wordt aanbevolen voor de werkbelasting van de productie.

>[AZURE.NOTE] Als u voor de grootte van een computer die geen ondersteuning biedt voor opslag van Premium Premium (SSD), worden automatisch de grootte van uw computer.  

- **Opslag-account**, kunt u de naam van de opslag automatisch ingerichte accepteren. U kunt ook klikken op **opslag account** met een bestaande account kiezen en het soort opslag configureren. Azure wordt standaard een nieuwe opslag-account gemaakt met lokaal redundante opslag. Zie voor meer informatie over opties voor opslag, [opslag Azure-replicatie](../storage/storage-redundancy.md).

- **Netwerk**, kunt u de automatisch ingevulde waarden accepteren. U kunt ook klikken op elke functie handmatig configureren van het **virtuele netwerk** **Subnet**, **openbaar IP-adres**en **Netwerk Security Group**. Voor de toepassing van deze zelfstudie, behouden de standaardwaarden.

- Azure schakelt **controle** standaard met de opslag-account opgegeven voor de VM. U kunt deze instellingen wijzigen.

- Geef onder **beschikbaar**, een set van beschikbaarheid. U kunt **geen**selecteren voor de toepassing van deze zelfstudie. Configureer de beschikbaarheid om te voorkomen dat de virtuele machine opnieuw te maken als u van plan bent SQL AlwaysOn beschikbaarheidsgroepen instellen.  Voor meer informatie, Zie [beheren van de beschikbaarheid van virtuele Machines](virtual-machines-windows-manage-availability.md).

Wanneer u klaar bent met het configureren van deze instellingen, klik op **OK**.

## <a name="4-configure-sql-server-settings"></a>4. instellingen voor SQL server configureren
Configureer op de **instellingen van de SQL Server** -blade specifieke instellingen en optimalisatie voor SQL Server. Dit zijn de instellingen die u voor SQL Server configureren kunt.

| Instelling               |
|---------------------|
| [Connectiviteit](#connectivity)              |
| [Verificatie](#authentication)                |
| [Opslagconfiguratie](#storage-configuration)            |
| [Automatisch herstellen](#automated-patching) |
| [Automatische back-up](#automated-backup)             |
| [Integratie van de Azure sleutel kluis](#azure-key-vault-integration)             |
| [R-Services](#r-services) |

### <a name="connectivity"></a>Connectiviteit
Geef het type toegang dat u het exemplaar van SQL Server op deze VM wilt onder **SQL-verbindingen**. Voor de toepassing van deze zelfstudie, selecteer **openbare (internet)** voor verbindingen met SQL Server toestaan van computers of services op het internet. Met deze optie is geselecteerd, Azure automatisch geconfigureerd door de firewall en het netwerkgroep toestaan van verkeer op poort 1433.  

![SQL-connectiviteitsopties](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-connectivity-alt.png)

Als u wilt verbinden met SQL Server via het internet, moet u ook SQL Server-verificatie die wordt beschreven in de volgende sectie inschakelen.

>[AZURE.NOTE] Het is mogelijk meer beperkingen voor de netwerkcommunicatie toevoegen aan uw SQL Server-VM. U kunt dit doen door het bewerken van de beveiligingsgroep netwerk nadat de VM is gemaakt. Zie voor meer informatie [Wat is een Network Security Group (NSG)?](../virtual-network/virtual-networks-nsg.md)

Als u liever geen verbindingen met de Database-Engine via het internet inschakelen, kiest u een van de volgende opties:

- **Lokale (binnen de VM)** voor verbindingen met SQL-Server alleen van de VM.
- **(Binnen het virtuele netwerk) particuliere** verbindingen met SQL Server toestaan van computers of services in hetzelfde virtuele netwerk.

>[AZURE.NOTE] Afbeelding van de virtuele machine voor SQL Server Express edition wordt het TCP/IP-protocol niet automatisch ingeschakeld. Dit geldt ook voor de openbare en particuliere connectiviteitsopties. Voor Express edition, moet u SQL Server Configuration Manager [het TCP/IP-protocol handmatig](#configure-sql-server-to-listen-on-the-tcp-protocol) inschakelen na het maken van de VM.

In het algemeen de beveiliging verbeteren door het kiezen van de meest beperkende connectiviteit waarmee uw scenario. Maar alle opties kunnen worden beveiligd door middel van netwerk Security Group regels en SQL, Windows-verificatie.

**Poort** standaard 1433. U kunt een ander poortnummer kunt opgeven.
Zie voor meer informatie verbinding maken met een SQL Server virtuele Machine (Resource Manager) [| Microsoft Azure](virtual-machines-windows-sql-connect.md).

### <a name="authentication"></a>Verificatie
Als u SQL Server-verificatie vereist, klik op **SQL**-verificatie **inschakelen** .

![SQL Server-verificatie](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-authentication.png)

>[AZURE.NOTE] Als u toegang tot SQL Server via internet (dat wil zeggen de verbindingsoptie openbare), moet u hier de SQL-verificatie inschakelen. Publieke toegang tot de SQL-Server is vereist voor het gebruik van SQL-verificatie.

Als u SQL Server-verificatie inschakelt, geeft u een **aanmeldingsnaam** en **wachtwoord**. Deze gebruikersnaam is geconfigureerd als lid van de vaste serverrol **sysadmin** en aanmelding voor SQL Server-verificatie. [Kies een modus voor verificatie](http://msdn.microsoft.com/library/ms144284.aspx) Zie voor meer informatie over verificatiemodi.

Als u niet SQL Server-verificatie inschakelt, kunt vervolgens u de lokale Administrator-account op de VM verbinding maken met de SQL Server-exemplaar.

### <a name="storage-configuration"></a>Opslagconfiguratie
Klik op **opslag configureren** om op te geven van de opslagvereisten.

![SQL opslagconfiguratie](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-storage.png)

>[AZURE.NOTE] Deze optie is niet beschikbaar als u standaard opslag. Optimalisatie voor automatische opslag is alleen beschikbaar voor Premium-opslag.

Als de i/o-bewerkingen per seconde (IOP's), doorvoer in MB/s en de totale omvang kunt u vereisten opgeven. Deze waarden configureren met behulp van de beweegbare schalen. De portal berekent automatisch het nummer van de schijven op basis van deze vereisten.

Standaard wordt Azure bij de opslag 5000 IOP's, 200 MB aan ruimte en 1 TB aan opslagruimte. U kunt deze Opslaginstellingen op basis van werkbelasting. Selecteer onder **opslag is geoptimaliseerd voor**, een van de volgende opties:

- **Algemeen** is de standaardinstelling en ondersteunt de meeste werkbelasting.
- Verwerking voor **transactionele** optimaliseert de opslaglocatie voor de database voor traditionele OLTP workloads.
- **Data warehousing** optimaliseert de opslag voor een standaardwerkbelasting analytisch en rapportage.

>[AZURE.NOTE] De bovengrenzen van de schuifregelaars variëren, afhankelijk van de grootte van het geselecteerde virtuele machine.

### <a name="automated-patching"></a>Automatisch herstellen
**Automatisch herstellen** is standaard ingeschakeld. Geautomatiseerde patches kunt Azure patch automatisch SQL Server en het besturingssysteem. Geef een dag van de week, tijd en duur voor het onderhoudsvenster. Azure voert patches in dit onderhoudsvenster. Het venster onderhoudsschema gebruikt de landinstelling VM voor tijd. Klik op **uitschakelen**als u niet dat Azure wilt patch automatisch SQL Server en het besturingssysteem.  

![SQL automatisch herstellen](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-patching.png)

Zie voor meer informatie [Automatisch patches voor SQL Server in Azure virtuele Machines](virtual-machines-windows-sql-automated-patching.md).

### <a name="automated-backup"></a>Automatische back-up
Inschakelen automatische back-ups voor alle databases onder **Automatische back-up**. Automatische back-up is standaard uitgeschakeld.

Als u automatische back-up van SQL inschakelt, kunt u het volgende configureren:

- Periode (dagen) voor back-ups
- Opslag-account te gebruiken voor back-ups
- De optie codering en het wachtwoord voor de back-ups

Klik op **inschakelen**voor het coderen van de back-up. Geef het **wachtwoord**. Azure maakt een certificaat voor het coderen van de back-ups en het opgegeven wachtwoord te beschermen dat certificaat gebruikt.

![Geautomatiseerde back-ups van SQL](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-autobackup.png)

 Zie voor meer informatie, [Geautomatiseerde back-up van SQL Server in Azure virtuele Machines](virtual-machines-windows-sql-automated-backup.md).

### <a name="azure-key-vault-integration"></a>Azure sleutel kluis integratie
**Integratie van de Azure sleutel kluis** opslaan beveiliging geheimen in Azure voor codering, en klik op **inschakelen**.

![Integratie van SQL Azure sleutel kluis](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-akv.png)

De volgende tabel worden de parameters die nodig zijn voor het configureren van Azure sleutel kluis integratie.

|PARAMETER|BESCHRIJVING|VOORBEELD|
|----------|----------|-------|
|**Sleutel kluis URL** |De locatie van de sleutel kluis.|https://contosokeyvault.Vault.Azure.NET/ |
|**Principal-naam** |Azure Active Directory service principal-naam. Deze naam wordt ook wel de Client-ID.  |fde2b411 33d 5-4e11-af04eb07b669ccf2|
| **Belangrijkste geheim**|Azure Active Directory service principal geheim. Dit geheim wordt ook wel genoemd het geheim van de Client. | 9VTJSQwzlFepD8XODnzy8n2V01Jd8dAjwm/azF1XDKM =|
|**Naam van de referentie**|**Referentie-naam**: AKV integratie maakt een referentie binnen SQL Server, zodat de VM toegang hebben tot de sleutel kluis. Kies een naam voor deze referentie.| mycred1|

Voor meer informatie Zie [Azure sleutel kluis integratie configureren voor SQL Server op Azure VMs](virtual-machines-windows-ps-sql-keyvault.md).

Wanneer u klaar bent met het configureren van instellingen voor SQL Server, klikt u op **OK**.

### <a name="r-services"></a>R-services
U hebt de optie [SQL Server R Services](https://msdn.microsoft.com/library/mt604845.aspx)inschakelen voor SQL Server 2016 Enterprise edition. Hiermee kunt u geavanceerde analytics gebruiken met SQL Server 2016. Klik op **inschakelen** in de **Instellingen van de SQL Server** -blade.

![SQL Server R Services inschakelen](./media/virtual-machines-windows-portal-sql-server-provision/azure-vm-sql-server-r-services.png)

>[AZURE.NOTE] Voor afbeeldingen die geen Enterprise-editie 2016 SQL Server is de optie R Services inschakelen uitgeschakeld.

## <a name="5-review-the-summary"></a>5. Controleer het overzicht
De **Samenvatting** blade, bekijk het overzicht en klik op **OK** als u wilt maken van SQL Server, resourcegroep en resources die zijn opgegeven voor deze VM.

U kunt de implementatie van de portal azure controleren. De knop **meldingen** aan de bovenkant van het scherm ziet u eenvoudig de status van de implementatie van.

>[AZURE.NOTE] Waarmee u een idee van implementatietijden, ik een VM SQL op de Oost-Amerikaanse regio geïmplementeerd met standaardinstellingen. De implementatie van deze test duurde een totaal van 26 minuten. Maar u kunt ondervinden een sneller of langzamer implementatietijd op basis van uw regio en instellingen geselecteerd.

## <a name="open-the-vm-with-remote-desktop"></a>Open de VM met extern bureaublad

De volgende stappen uit om verbinding met de virtuele machine met extern bureaublad te gebruiken:

1. Nadat de Azure VM is gebouwd, verschijnt het pictogram voor de VM op uw dashboard Azure. U kunt het ook vinden door te bladeren in uw bestaande virtuele machines. Klik op de nieuwe virtuele machine voor SQL. Een **virtuele machine** blade geeft de details van uw virtuele machine.
1. Aan de bovenkant van de **virtuele machine** blade, klikt u op **verbinden**.
1. Een RDP-bestand in de browser gedownload voor VM. Open het RDP-bestand.
    ![Extern bureaublad voor SQL-VM](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-vm-remote-desktop.png)
1. Verbinding met extern bureaublad wordt u gewaarschuwd dat de uitgever van deze externe verbinding niet kan worden geïdentificeerd. Klik op **verbinding maken** om door te gaan.
1. Klik in het dialoogvenster **Windows-beveiliging** op **een andere account gebruiken**.
1. Voor type **gebruikersnaam** ** \<gebruikersnaam >**, waarbij <user name> is de gebruikersnaam die u hebt opgegeven bij het instellen van de VM. U moet een eerste backslash voor de naam toevoegen.
1. Typ het **wachtwoord** dat u eerder voor deze VM hebt geconfigureerd en klik vervolgens op **OK** om verbinding te maken.
1. Als u een andere **Verbinding met extern bureaublad** -dialoogvenster wordt gevraagd of u verbinding wilt, klikt u op **Ja**.

U kunt nadat u verbinding met de virtuele machine van SQL Server, SQL Server Management Studio te starten en verbinding maken met Windows-verificatie met de lokale administrator-referenties. Als u SQL Server-verificatie hebt ingeschakeld, kunt u ook verbinding maken met SQL-verificatie met behulp van de SQL-aanmelding en wachtwoord die u tijdens het inrichten hebt geconfigureerd.

Toegang tot de computer, kunt u machine en SQL Server-instellingen op basis van uw behoeften rechtstreeks wijzigen. U kunt bijvoorbeeld de firewall configureren of SQL Server-configuratie-instellingen wijzigen.

## <a name="connect-to-sql-server-remotely"></a>Extern verbinding maken met SQL Server

In deze zelfstudie, we **openbare** toegang voor de virtuele machine en **SQL Server-verificatie**geselecteerd. Deze instellingen automatisch geconfigureerd de virtuele machine als u wilt dat SQL Server-verbindingen vanaf elke client via het internet (ervan uitgaande dat ze de juiste SQL-aanmelding hebt).

>[AZURE.NOTE] Als u openbare niet tijdens het inrichten hebt geselecteerd, zijn extra stappen vereist voor toegang tot uw SQL Server-exemplaar via het internet. Zie [verbinding maken met een SQL Server virtuele Machine](virtual-machines-windows-sql-connect.md)voor meer informatie.

In de volgende secties wordt aangegeven hoe verbinding maken met de SQL Server-exemplaar op uw VM vanaf een andere computer via het internet.

> [AZURE.INCLUDE [Connect to SQL Server in a VM Resource Manager](../../includes/virtual-machines-sql-server-connection-steps-resource-manager.md)]

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het gebruik van SQL Server in Azure, [SQL Server Azure virtuele Machines](virtual-machines-windows-sql-server-iaas-overview.md) en de [Veelgestelde vragen](virtual-machines-windows-sql-server-iaas-faq.md).

Bekijk de [Azure VM is het beste platform voor SQL Server 2016](https://channel9.msdn.com/Events/DataDriven/SQLServer2016/Azure-VM-is-the-best-platform-for-SQL-Server-2016)voor een video-overzicht van SQL Server op Azure virtuele Machines.

[Het pad leren verkennen](https://azure.microsoft.com/documentation/learning-paths/sql-azure-vm/) voor SQL Server op Azure virtuele machines.
