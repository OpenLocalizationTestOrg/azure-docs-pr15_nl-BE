<properties
    pageTitle="Een SQL Server-database migreren naar SQL Server op een VM | Microsoft Azure"
    description="Meer informatie over het migreren van een gebruikersdatabase in ruimten met SQL Server in een Azure virtuele machine."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="sabotta"
    manager="jhubbard"
    editor=""
    tags="azure-service-management" />
<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="carlasab"/>


# <a name="migrate-a-sql-server-database-to-sql-server-in-an-azure-vm"></a>Een SQL Server-database te migreren naar SQL Server in een Azure VM

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]Model van Resource Manager.


Er zijn een aantal methoden voor het migreren van een gebruikersdatabase in lokalen SQL Server met SQL Server in een VM Azure. In dit artikel wordt kort bespreken verschillende methoden, de beste methode voor verschillende scenario's het beste en bevatten een [zelfstudie](#azure-vm-deployment-wizard-tutorial) om u te helpen met behulp van de wizard **SQL Server-Database naar een Microsoft Azure VM implementeren** . 

De methode met het **implementeren van een SQL Server-Database naar een Microsoft Azure VM** wizard beschreven in de [zelfstudie](#azure-vm-deployment-wizard-tutorial) is voor de klassieke implementatiemodel. 

## <a name="what-are-the-primary-migration-methods"></a>Wat zijn de primaire migratiemethoden?

De van primaire migratiemethoden zijn:

- Het implementeren van een SQL Server-Database naar een Microsoft Azure VM-wizard gebruiken
- Back-up op ruimten met compressie en kopieert u de back-up naar de Azure virtual machine
- Uitvoeren van een back-up maken en terugzetten in de Azure virtual machine van URL van de URL
- Loskoppelen en kopieer de bestanden van de gegevens- en logboekbestanden naar Azure blob-opslag en vervolgens koppelen aan SQL Server in Azure VM vanaf URL
- Fysieke machine op de ruimten converteren naar Hyper-V VHD en vervolgens te implementeren als nieuwe VM met VHD geüpload uploaden naar Azure Blob-opslag
- Schip van de harde schijf met de Windows-Service voor importeren/exporteren
- Als u een implementatie AlwaysOn op gebouwen, de [Azure Replica Wizard toevoegen](virtual-machines-windows-classic-sql-onprem-availability.md) gebruiken voor het maken van een replica in Azure en vervolgens failover, gebruikers de Azure-instantie aan te wijzen
- Met SQL Server- [transactionele replicatie](https://msdn.microsoft.com/library/ms151176.aspx) kunt configureren de Azure SQL Server-exemplaar als abonnee en Schakel replicatie, gebruikers de Azure-instantie aan te wijzen



## <a name="choosing-your-migration-method"></a>De migratiemethode kiezen

Migratie van de bestanden in een gecomprimeerde back-upbestand met Azure VM is voor de optimale overdracht prestaties in het algemeen de beste methode. Dit is de methode die wordt gebruikt voor het [implementeren van een SQL Server-Database naar een Microsoft Azure VM-wizard](#azure-vm-deployment-wizard-tutorial) . Deze wizard is de aanbevolen methode voor het migreren van een actief op gebouwen gebruiker-database op SQL Server 2005 of hoger naar SQL Server 2014 of groter wanneer het back-upbestand van de database gecomprimeerd minder dan 1 TB is.

Om uitvaltijd te minimaliseren tijdens het databasemigratieproces, gebruikt u de AlwaysOn-optie of de optie voor transactionele replicatie.

Als het niet mogelijk om met de bovenstaande methoden, moet u handmatig uw database migreren. Met deze methode wordt u in het algemeen begint met een database back-up, gevolgd door een kopie van de de database een back-up in Azure en voert u het terugzetten van een database. U kunt ook kopiëren van bestanden van de database zelf in Azure en deze vervolgens te koppelen. Er verschillende manieren waarop u dit handmatige proces van een database in een Azure VM migratie kunt uitvoeren.

> [AZURE.NOTE] Wanneer u een upgrade naar SQL Server 2014 of SQL Server 2016 uit oudere versies van SQL Server uitvoert, moet u nagaan of wijzigingen nodig zijn. U wordt aangeraden alle afhankelijkheden van functies die niet worden ondersteund door de nieuwe versie van SQL Server als onderdeel van uw migratieproject te adresseren. Zie [een Upgrade naar SQL Server](https://msdn.microsoft.com/library/bb677622.aspx)voor meer informatie over de ondersteunde versies en scenario's.

In de volgende tabel geeft een overzicht van de van primaire migratiemethoden en worden beschreven als het gebruik van elke methode is het meest geschikt.

| Methode  | Bron-databaseversie  |  Bestemming-databaseversie | Beperking op de grootte van de back-Source database  | Notities  |
|---|---|---|---|---|
| [Het implementeren van een SQL Server-Database naar een Microsoft Azure VM-wizard gebruiken](#azure-vm-deployment-wizard-tutorial) | SQL Server 2005 of hoger | SQL Server 2014 of hoger | < 1 TB  | Snelste en eenvoudigste methode gebruiken zo veel mogelijk te migreren naar een nieuwe of bestaande SQL Server-exemplaar in een Azure virtuele machine | 
| [Gebruik de Wizard Azure Replica toevoegen](virtual-machines-windows-classic-sql-onprem-availability.md) | SQL Server 2012 of hoger | SQL Server 2012 of hoger | [Azure VM opslaglimiet](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) | Hiermee minimaliseert u uitvaltijd gebruiken wanneer u een implementatie AlwaysOn op ruimten |
| [Transactionele replicatie van SQL Server gebruiken](https://msdn.microsoft.com/library/ms151176.aspx) | SQL Server 2005 of hoger | SQL Server 2005 of hoger | [Azure VM opslaglimiet](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) | Wanneer u de uitvaltijd tot een minimum moet gebruiken en hoeft niet een implementatie AlwaysOn op ruimten |
| [Back-up op ruimten met compressie en kopieert u de back-up naar de Azure virtual machine](#backup-to-file-and-copy-to-vm-and-restore) | SQL Server 2005 of hoger | SQL Server 2005 of hoger | [Azure VM opslaglimiet](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) | Gebruik alleen wanneer u de wizard, zoals wanneer de doelversie van de database kleiner dan SQL Server 2012 SP1 CU2 is niet gebruiken of de grootte van de back-database groter is dan 1 TB (12,8 TB met SQL Server 2016) |
| [Uitvoeren van een back-up maken en terugzetten in de Azure virtual machine van URL van de URL](#backup-to-url-and-restore) | CU2 voor SQL Server 2012 SP1 of hoger | CU2 voor SQL Server 2012 SP1 of hoger | < 12,8 TB voor SQL Server 2016, anders < 1 TB | In het algemeen met behulp van [back-up naar URL](https://msdn.microsoft.com/library/dn435916.aspx) is gelijkwaardige prestaties met de wizard en niet zo eenvoudig |
| [Loskoppelen en kopieer de bestanden van de gegevens- en logboekbestanden naar Azure blob-opslag en vervolgens koppelen aan SQL Server in Azure virtuele machine van URL](#detach-and-copy-to-url-and-attach-from-url) | SQL Server 2005 of hoger | SQL Server 2014 of hoger | [Azure VM opslaglimiet](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) | Gebruik deze methode als u van plan voor het [opslaan van de bestanden op de Azure Blob storage-service bent](https://msdn.microsoft.com/library/dn385720.aspx) en koppelen aan SQL Server wordt uitgevoerd in een VM Azure, met name met zeer grote databases |
| [Machine op de ruimten converteren naar Hyper-V VHD en implementeert u een nieuwe virtuele machine met geüploade VHD uploaden naar Azure Blob-opslag](#convert-to-vm-and-upload-to-url-and-deploy-as-new-vm) | SQL Server 2005 of hoger | SQL Server 2005 of hoger | [Azure VM opslaglimiet](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) | Gebruik bij elkaar [brengen van uw eigen SQL Server-licentie](../sql-database/sql-database-paas-vs-sql-server-iaas.md), bij het migreren van een database die u op een oudere versie van SQL Server of uitvoeren wilt bij het migreren van systeem- en databases als onderdeel van de migratie van de database hangt af van de andere gebruikersdatabases en systeemdatabases. |
| [Schip van de harde schijf met de Windows-Service voor importeren/exporteren](#ship-hard-drive) | SQL Server 2005 of hoger | SQL Server 2005 of hoger | [Azure VM opslaglimiet](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) | De [Windows-Service voor importeren/exporteren](../storage/storage-import-export-service.md) gebruiken bij kopiëren van handmatige methode te langzaam, zoals met zeer grote databases is |

## <a name="azure-vm-deployment-wizard-tutorial"></a>Azure VM deployment wizard zelfstudie

De wizard **SQL Server-Database naar een Microsoft Azure VM implementeren** in Microsoft SQL Server Management Studio gebruiken voor het migreren van een SQL Server 2005, SQL Server 2008, SQL Server 2008 R2, SQL Server 2012, 2014 van SQL Server of SQL Server 2016 op ruimten gebruiker (maximaal 1 TB) 2014 van SQL Server of SQL Server 2016-database in een Azure virtuele machine. Met deze wizard kunt u een gebruikersdatabase te migreren naar een bestaande Azure virtuele machine of een Azure VM met SQL Server door de wizard wordt gemaakt tijdens het migratieproces. Wanneer u een database hebt gemigreerd naar een nieuwere versie van SQL Server, wordt de database automatisch bijgewerkt tijdens het proces.

De methode is voor de klassieke implementatiemodel. 

### <a name="get-latest-version-of-the-deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard"></a>Nieuwste versie van de implementeren krijgen een SQL Server-Database naar een Microsoft Azure VM-wizard

Gebruik de nieuwste versie van Microsoft SQL Server Management Studio voor SQL Server om ervoor te zorgen dat u de nieuwste versie van de wizard **SQL Server-Database naar een Microsoft Azure VM implementeren hebt** . De meest recente versie van deze wizard is uitgerust met de meest recente updates voor de Azure klassieke portal en ondersteunt de nieuwste Azure VM-afbeeldingen in de galerie (oudere versies van de wizard werkt niet). Voor het ophalen van de meest recente versie van Microsoft SQL Server Management Studio voor SQL Server, [het downloaden](http://go.microsoft.com/fwlink/?LinkId=616025) en installeren op een clientcomputer met de verbinding met de database die u van plan om te migreren en met het internet bent.

### <a name="configure-the-existing-azure-virtual-machine-and-sql-server-instance-if-applicable"></a>De bestaande Azure virtuele machine en een exemplaar van SQL Server configureren (indien van toepassing)

Als u naar een bestaande Azure VM migreert, zijn de volgende configuratiestappen vereist:

- Azure VM en het exemplaar van SQL Server mogelijk verbinding vanaf een andere computer met de volgende stappen om [verbinding](virtual-machines-windows-sql-connect.md)met het exemplaar van SQL Server VM van SSMS op een andere computer te configureren. Alleen de 2014 van SQL Server en SQL Server 2016 afbeeldingen in de galerie worden ondersteund als u migreert met de wizard.
- Een open eindpunt voor de service SQL Server Cloud-Adapter configureren op de Microsoft Azure gateway met private poort van 11435. Deze poort wordt gemaakt als onderdeel van de SQL Server 2014 of SQL Server 2016 op een Azure Microsoft VM wordt ingericht. De Cloud-Adapter maakt ook een Windows Firewall-regel voor het toestaan van de binnenkomende TCP-verbindingen op de standaardpoort 11435. Dit eindpunt kunt de wizard om de service Cloud-adapter te kopiëren van de back-upbestanden van het exemplaar in de lokalen voor Azure VM. Zie [Wolk Adapter voor SQL Server](https://msdn.microsoft.com/library/dn169301.aspx)voor meer informatie.

    ![Wolk Adapter eindpunt maken](./media/virtual-machines-windows-migrate-sql/cloud-adapter-endpoint.png)

### <a name="run-the-use-the-deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard"></a>Het gebruik van het implementeren van een SQL Server-Database een wizard Microsoft Azure VM uitvoeren

1. Microsoft SQL Server Management Studio voor Microsoft SQL Server 2016 opent en verbinding maken met de SQL Server-sessie met de database die u voor een Azure VM migreren.
2. Klik met de rechtermuisknop op de database die u wilt migreren, taken en klik vervolgens op distribueren naar een Microsoft Azure VM.

    ![Start de Wizard](./media/virtual-machines-windows-migrate-sql/start-wizard.png)

3. Klik op de pagina Introductie op volgende.
4. Klik op de pagina instellingen voor offlinegegevensbronnen verbinding maken met de SQL Server-sessie met de database die u wilt migreren naar een Azure VM.
5. Een tijdelijke locatie voor de back-upbestanden opgeven. Als u verbinding maakt met een externe server, moet u een netwerkstation.

    ![Broninstellingen](./media/virtual-machines-windows-migrate-sql/source-settings.png)

6. Klik op volgende.
7. Klik op aanmelden en aanmelden bij uw account Azure op de pagina aanmelden bij Microsoft Azure.
8. Selecteer het abonnement dat u wilt gebruiken en klik op volgende.

    ![Azure-aanmelden](./media/virtual-machines-windows-migrate-sql/azure-signin.png)

9. Klik op de pagina instellingen voor inhoudsdistributie kunt u een nieuwe of een bestaande Cloud Service naam en de naam van de virtuele Machine:

 - Geef een nieuwe naam voor de Cloud servicenaam en een virtuele Machine voor het maken van een nieuwe wolk Service met een nieuwe Azure virtuele machine met behulp van een SQL Server 2014 of SQL Server 2016 galerij afbeelding.

     - Als u een nieuwe Service Cloud-naam opgeeft, de opslag account opgeven die u wilt gebruiken.

     - Als u een bestaande Service Cloud-naam opgeeft, wordt de account opslag opgehaald en voor u ingevoerd.

 - Bestaande Service Cloud-naam en een nieuwe virtuele machinenaam een nieuwe Azure virtuele machine maken in een bestaande Cloud-Service opgeven. Geef alleen een afbeelding 2014 van SQL Server of SQL Server 2016.
 - Geef een bestaande Service Cloud-naam en virtuele Machine in een bestaande Azure virtual machine gebruiken. Dit moet een afbeelding die is gemaakt met behulp van een galerijafbeelding 2014 van SQL Server of SQL Server 2016.

        ![Deployment Settings](./media/virtual-machines-windows-migrate-sql/deployment-settings.png)

10. Klik op instellingen
  - Als u een bestaande Service Cloud-naam en de naam van de virtuele Machine hebt opgegeven, wordt u gevraagd om de gebruikersnaam en het wachtwoord.

        ![Instellingen van de computer Azure](./media/virtual-machines-windows-migrate-sql/azure-machine-settings.png)

    - Als u een nieuwe naam voor de virtuele Machine hebt opgegeven, wordt u gevraagd om een afbeelding te selecteren uit de lijst met afbeeldingen en geef de volgende informatie:
      - Afbeeldingen: Selecteer alleen SQL Server 2014 of SQL Server 2016
        - Gebruikersnaam
        - Nieuw wachtwoord
        - Bevestig wachtwoord
        - Locatie
        - Grootte.
    - Bovendien op voor het accepteren van het certificaat zelf gegenereerd voor deze nieuwe Microsoft Azure Virtual Machine en klik op OK.

    ![Azure nieuwe instellingen van de computer](./media/virtual-machines-windows-migrate-sql/azure-new-machine-settings.png)

11. Geef de naam van de database als deze verschilt van de naam van de bron. Als de doeldatabase al bestaat, het systeem wordt automatisch verhogen de naam van de database in plaats van de bestaande database overschrijven.
12. Klik op volgende en klik vervolgens op voltooien.

    ![Resultaten](./media/virtual-machines-windows-migrate-sql/results.png)

13. Wanneer de wizard is voltooid, verbinding maken met uw virtuele computer en controleer of de database is gemigreerd.
14. Als u een nieuwe virtuele machine hebt gemaakt, configureert u de Azure virtuele machine en het exemplaar van SQL Server verbinding maken [met het exemplaar van SQL Server VM van SSMS op een andere computer](virtual-machines-windows-sql-connect.md)via de stappen.

## <a name="backup-to-file-and-copy-to-vm-and-restore"></a>Back-up bestand te kopiëren naar de VM en terugzetten

Gebruik deze methode wanneer u het implementeren van een SQL Server-Database naar een Microsoft Azure VM wizard niet gebruiken omdat u wilt migreren naar een versie van SQL Server dan SQL Server 2014 of het back-upbestand groter is dan 1 TB omdat. Als het back-upbestand groter dan 1 TB is, moet u het stripe omdat de maximale grootte van een VM schijf 1 TB. De volgende algemene stappen voor het migreren van een gebruikersdatabase met behulp van deze handmatige methode gebruiken:

1.  Een volledige databaseback-up naar een locatie op het bedrijf uitvoeren.
2.  Maken of uploaden van een virtuele machine met de versie van SQL Server nodig.
3.  Setup-connectiviteit op basis van uw vereisten. Zie [verbinding maken met een SQL Server virtuele Machine op Azure (Resource Manager)](virtual-machines-windows-sql-connect.md).
4.  Kopieer uw back-bestanden naar uw VM met extern bureaublad, Windows Verkenner of de opdracht kopiëren vanaf een opdrachtprompt.

## <a name="backup-to-url-and-restore"></a>Back-up en terugzetten van URL

De [back-up naar URL](https://msdn.microsoft.com/library/dn435916.aspx) methode gebruiken als u het implementeren van een SQL Server-Database naar een Microsoft Azure VM wizard niet gebruiken omdat het back-upbestand groter dan 1 TB is en u van en naar SQL Server 2016 migreert. Voor databases die kleiner is dan 1 TB of een versie van SQL Server dan SQL Server 2016, wordt gebruik van de wizard aanbevolen. Met SQL Server 2016, worden striped reservekopieseries worden ondersteund, aanbevolen voor prestaties en moet groter zijn dan de maximale grootte per blob. Zeer grote databases verdient het gebruik van de [Windows-Service voor importeren/exporteren](../storage/storage-import-export-service.md) .

## <a name="detach-and-copy-to-url-and-attach-from-url"></a>Loskoppelen en naar de URL kopiëren en toevoegen van URL

Gebruik deze methode wanneer u van plan voor het [opslaan van de bestanden op de Azure Blob storage-service bent](https://msdn.microsoft.com/library/dn385720.aspx) en deze koppelen aan SQL Server wordt uitgevoerd in een VM Azure, met name met zeer grote databases. De volgende algemene stappen voor het migreren van een gebruikersdatabase met behulp van deze handmatige methode gebruiken:

1.  De databasebestanden loskoppelen van de database-instantie op gebouwen.
2.  Kopieer de losgekoppelde databasebestanden naar Azure blob-opslag met behulp van het [opdrachtregelprogramma AZCopy](../storage/storage-use-azcopy.md).
3.  De databasebestanden uit de Azure-URL toevoegen aan de SQL Server-exemplaar in Azure VM.

## <a name="convert-to-vm-and-upload-to-url-and-deploy-as-new-vm"></a>Converteren naar de VM en uploaden naar de URL en implementeren van nieuwe VM

Gebruik deze methode voor het migreren van alle systeem- en gebruikersdatabases in een SQL Server-exemplaar op ruimten Azure virtual machine. De volgende algemene stappen voor het migreren van een volledige SQL Server-exemplaar met behulp van deze handmatige methode gebruiken:

1.  Omzetten in fysieke of virtuele machines Hyper-V VHD's met behulp van [Microsoft Virtual Machine-conversieprogramma](http://technet.microsoft.com/library/dn873998.aspx).
2.  VHD-bestanden uploaden naar Azure opslag via de [cmdlet Add-AzureVHD](https://msdn.microsoft.com/library/windowsazure/dn495173.aspx).
3.  Een nieuwe virtuele machine met behulp van de geüploade VHD implementeren.

> [AZURE.NOTE] De migratie van een volledige-toepassing, kunt u overwegen [Azure Site herstellen](../site-recovery/site-recovery-overview.md)].

## <a name="ship-hard-drive"></a>Verzenden van de harde schijf

De [methode voor Windows importeren/exporteren](../storage/storage-import-export-service.md) gebruiken grote hoeveelheden gegevens overbrengen naar Azure Blob-opslag uploaden via het netwerk beschikbaar is onevenredig duur of niet haalbaar is. Met deze service kunt u een of meer vaste schijven met gegevens die u wilt een Azure Datacenter waar uw gegevens worden geüpload naar uw account opslag verzenden.

## <a name="next-steps"></a>Volgende stappen

Zie [SQL Server op de virtuele Machines van Azure-overzicht](virtual-machines-windows-sql-server-iaas-overview.md)voor meer informatie over het uitvoeren van SQL Server op Azure virtuele Machines.

Zie voor instructies voor het maken van een virtuele Machine voor Azure SQL Server van een vastgelegde afbeelding, [Tips & trucs over klonen Azure SQL virtuele machines van de vastgelegde beelden](https://blogs.msdn.microsoft.com/psssql/2016/07/06/tips-tricks-on-cloning-azure-sql-virtual-machines-from-captured-images/) op de blog CSS SQL Server Engineers.
