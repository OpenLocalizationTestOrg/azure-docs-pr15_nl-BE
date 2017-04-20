<properties
    pageTitle="Opslagconfiguratie voor SQL Server VMs | Microsoft Azure"
    description="In dit onderwerp wordt beschreven hoe Azure configureert u opslag voor SQL Server VMs tijdens het inrichten (distributiemodel Resource Manager). Ook wordt uitgelegd hoe u opslag voor uw bestaande SQL Server VMs kunt configureren."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="ninarn"
    manager="jhubbard"    
    tags="azure-resource-manager"/>
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="08/04/2016"
    ms.author="ninarn" />

# <a name="storage-configuration-for-sql-server-vms"></a>Opslagconfiguratie voor SQL Server VMs

Wanneer u een afbeelding van de virtuele machine SQL Server in Azure configureren, kunt u de Portal de opslagconfiguratie automatiseren. Dit omvat de opslag koppelen aan de VM die opslag toegankelijk voor SQL Server te maken en te configureren om te optimaliseren voor uw specifieke prestatie-eisen.

In dit onderwerp wordt uitgelegd hoe Azure configureert u opslag voor uw SQL Server VMs tijdens het inrichten en bestaande VMs. Deze configuratie is gebaseerd op de [beste prestaties](virtual-machines-windows-sql-performance.md) voor Azure VMs waarop SQL Server wordt uitgevoerd.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]klassieke implementatiemodel.

## <a name="prerequisites"></a>Vereisten
Voor het gebruik van de geautomatiseerde opslag configuratie-instellingen, moet u uw virtuele machine de volgende kenmerken:

- Ingericht met een [afbeelding van SQL Server](virtual-machines-windows-sql-server-iaas-overview.md#option-1-deploy-a-sql-vm-per-minute-licensing).
- Het [implementatiemodel Resource Manager](../resource-manager-deployment-model.md)gebruikt.
- [Premium-opslagruimte](../storage/storage-premium-storage.md)gebruikt.

## <a name="new-vms"></a>Nieuwe VMs
In de volgende secties wordt beschreven hoe u opslag voor nieuwe SQL Server virtuele machines configureren.

### <a name="azure-portal"></a>Azure Portal
Tijdens het inrichten van een Azure VM met SQL Server een afbeelding, kunt u de opslag automatisch configureert voor uw nieuwe VM. Geeft u de maximale grootte, maximale prestaties en type werkbelasting. Het volgende screenshot toont de opslag configuratie blade gebruikt tijdens het VM SQL wordt ingericht.

![Configuratie van SQL Server VM opslag tijdens het inrichten](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-configuration-provisioning.png)

Op basis van uw keuzen, voert Azure u de volgende configuratietaken voor opslag na het maken van de VM:

- Maakt en premium gegevensschijven koppelt aan de virtuele machine.
- Hiermee configureert u de gegevensschijven zijn toegankelijk voor SQL Server.
- Configureert de gegevensschijven in een groep op basis van de opgegeven grootte en de prestaties (IOP's en doorvoer) eisen.
- De opslag van toepassingen worden gekoppeld aan een nieuw station op de virtuele machine.
- Hiermee optimaliseert u deze nieuwe schijf op basis van het type opgegeven werkbelasting (datawarehousing, transactioneel verwerking of algemene).

Zie de [sectie van de configuratie van opslag](#storage-configuration)voor meer informatie over hoe Azure configureert u Opslaginstellingen voor. Zie voor een volledig overzicht van het maken van een SQL Server-VM in Azure Portal [provisioning zelfstudie](virtual-machines-windows-portal-sql-server-provision.md).

### <a name="resource-manage-templates"></a>Sjablonen voor resource beheren
Als u de volgende Resource Manager-sjablonen gebruikt, zijn twee premium gegevensschijven standaard met geen opslagconfiguratie groep gekoppeld. U kunt echter deze sjablonen als het aantal premium data schijven die zijn gekoppeld aan de virtuele machine wilt wijzigen.

- [VM met geautomatiseerde back-ups maken](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-sql-full-autobackup)
- [VM met geautomatiseerde patchen maken](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-sql-full-autopatching)
- [VM maken met AKV-integratie](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-sql-full-keyvault)

## <a name="existing-vms"></a>Bestaande VMs
Voor bestaande SQL Server-VMs kunt u enkele Opslaginstellingen in de portal Azure. Selecteer uw VM, gaat u naar het gebied Instellingen en selecteer SQL Server-configuratie. Het huidige gebruik van de opslag van de VM ziet u de configuratie van SQL Server-blade. Alle stations die aanwezig zijn op uw VM worden in deze grafiek weergegeven. Voor elk station wordt de opslagruimte in vier secties:

- SQL-gegevens
- SQL-logboek
- Andere (niet-SQL-opslag)
- Beschikbaar

![Opslag configureren voor een bestaande SQL Server VM](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-configuration-existing.png)

De opslag een nieuw station toevoegen of uitbreiden van een bestaand station configureren, klikt u op de link Bewerken boven de grafiek.

De configuratieopties die u ziet is afhankelijk van of u deze functie eerst hebt gebruikt. Als u voor de eerste keer gebruikt, kunt u de opslagvereisten voor een nieuw station. Als u deze functie eerder gebruikt om een station te maken, kunt u uitbreiden van de opslag van het station.

### <a name="use-for-the-first-time"></a>Gebruik voor de eerste keer
Als dit de eerste keer dat u deze functie gebruikt, kunt u de opslaglimiet voor grootte en prestaties voor een nieuw station. Deze ervaring is vergelijkbaar met wat u ziet op tijd wordt ingericht. Het belangrijkste verschil is dat u niet toegestaan om op te geven van het type werklast. Deze beperking voorkomt dat de bestaande SQL Server-configuraties op de virtuele machine onderbreken.

![Schuifregelaars voor SQL Server-opslag configureren](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-usage-sliders.png)

Azure maakt een nieuw station op basis van uw specificaties. In dit geval Azure configuratietaken worden uitgevoerd door de volgende opslag:

- Maakt en premium gegevensschijven koppelt aan de virtuele machine.
- Hiermee configureert u de gegevensschijven zijn toegankelijk voor SQL Server.
- Configureert de gegevensschijven in een groep op basis van de opgegeven grootte en de prestaties (IOP's en doorvoer) eisen.
- De opslag van toepassingen worden gekoppeld aan een nieuw station op de virtuele machine.

Zie de [sectie van de configuratie van opslag](#storage-configuration)voor meer informatie over hoe Azure configureert u Opslaginstellingen voor.

### <a name="add-a-new-drive"></a>Een nieuw station toevoegen
Als u op uw SQL Server-VM opslag al hebt geconfigureerd, opslag uit te breiden, twee nieuwe opties verschijnt. De eerste optie is het toevoegen van een nieuwe schijf, waarvoor het prestatieniveau van uw VM kunt verhogen.

![Een nieuw station toevoegt aan een VM SQL](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-configuration-add-new-drive.png)

Na het toevoegen van het station, moet u echter enkele extra handmatige configuratie te bereiken de prestatieverhoging uitvoeren.

### <a name="extend-the-drive"></a>Uitbreiden van het station
De andere optie voor het uitbreiden van de opslag is het uitbreiden van het bestaande station. Deze optie verhoogt de beschikbare opslagruimte voor uw station, maar de prestaties niet wordt verhoogd. Met opslagpools, u kunt het aantal kolommen niet wijzigen nadat u de groep hebt gemaakt. Het aantal kolommen, bepaalt het aantal parallelle schrijfbewerkingen, die kan worden striped worden verdeeld over de gegevensschijven. Daarom kunnen niet alle toegevoegde gegevensschijven prestaties verhogen. Zij kunnen alleen meer opslagruimte bieden voor de gegevens die worden weggeschreven. Deze beperking betekent ook dat, wanneer u het station uitbreidt, het aantal kolommen bepaalt u het minimum aantal gegevensschijven die u kunt toevoegen. Als u een groep met voor vier gegevensschijven maakt, is het aantal kolommen dus ook vier. Wanneer die u uitbreiden van de opslag, moet u ten minste vier gegevensschijven toevoegen.

![Een station voor een SQL-VM uitbreiden](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-extend-a-drive.png)

## <a name="storage-configuration"></a>Opslagconfiguratie
Deze sectie bevat een verwijzing naar de opslag configuratiewijzigingen die Azure automatisch uitvoert tijdens SQL VM inrichten of de configuratie in de Portal Azure.

- Als u minder dan twee TBs van opslag voor uw VM hebt geselecteerd, maakt Azure een opslaggroep.
- Als u ten minste twee TBs van opslag voor uw VM hebt geselecteerd, wordt een opslaggroep in Azure geconfigureerd. De volgende sectie van dit onderwerp vindt u de details van de configuratie van de opslag.
- Automatische configuratie altijd gegevensschijven [premium opslag](../storage/storage-premium-storage.md) P30 gebruikt. Er is dus een 1:1 koppeling tussen het geselecteerde aantal TB en het aantal aangesloten op uw VM gegevensschijven.

Zie de pagina met [prijzen van opslag](https://azure.microsoft.com/pricing/details/storage) op het tabblad **Schijfopslag** voor prijsinformatie.

### <a name="creation-of-the-storage-pool"></a>Het maken van de opslag van toepassingen
Azure kent de volgende instellingen voor het maken van de opslaggroep op SQL Server VMs.

| Instelling | Waarde |
|-----|-----|
| Stripe-grootte  | 256 KB (gegevensopslag); 64 KB (transactionele) |
| Schijfgrootten | 1 TB |
| Cache | Lezen |
| Toegewezen grootte | De clustergrootte van 64 KB NTFS |
| Initialisatie van Instant-bestand | Ingeschakeld |
| Pagina's in geheugen vergrendelen | Ingeschakeld |
| Herstel | Eenvoudig herstel (geen tolerantie) |
| Aantal kolommen | <sup>1</sup> -aantal schijven |
| TempDB-locatie | Opgeslagen onder data schijven<sup>2</sup> |

<sup>1</sup> nadat de groep is gemaakt, kunt u het aantal kolommen in de groep niet wijzigen.

<sup>2</sup> deze instelling is alleen van toepassing op het eerste station dat u maakt met de functie opslag configuratie.

## <a name="workload-optimization-settings"></a>Werkbelasting van optimalisatie-instellingen
De volgende tabel beschrijft de tekstopties drie werklast en hun bijbehorende optimalisatie:

| Type werkbelasting | Beschrijving | Optimalisatie |
|-----|-----|-----|
| **Algemeen** | De standaardinstelling die de meeste werkbelasting ondersteunt | Geen |
| **Transactionele bewerkingen** | Hiermee optimaliseert u de opslaglocatie voor de database voor traditionele OLTP workloads | Traceringsvlag 1117<br/>Traceringsvlag 1118 |
| **Gegevensopslag** | Hiermee optimaliseert u de opslag voor een standaardwerkbelasting analytisch en rapportage | Traceringsvlag 610<br/>Traceringsvlag 1117 |

>[AZURE.NOTE] Wanneer u een virtuele machine van SQL inrichten door deze te selecteren in de stap van de configuratie van opslag, kunt u alleen het type werklast opgeven.

## <a name="next-steps"></a>Volgende stappen
Zie [SQL Server Azure virtuele Machines](virtual-machines-windows-sql-server-iaas-overview.md)voor andere onderwerpen die betrekking hebben op het SQL Server wordt uitgevoerd in Azure VMs.
