<properties
    pageTitle="Azure opslag gebruiken voor SQL Server back-up en terugzetten | Microsoft Azure"
    description="Informatie over het back-up van SQL Server naar Azure opslag. De voordelen van back-ups van SQL-databases naar Azure opslag uitgelegd."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="MikeRayMSFT"
    manager="jhubbard"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="07/22/2016"
    ms.author="mikeray"/>

# <a name="use-azure-storage-for-sql-server-backup-and-restore"></a>Azure opslag gebruiken voor SQL Server back-up en herstel

## <a name="overview"></a>Overzicht

U start met SQL Server 2012 SP1 CU2, kunt u back-ups van SQL Server schrijven rechtstreeks naar de Azure Blob storage-service. U kunt deze functionaliteit tot maken en terugzetten van de Azure Blob-service met een SQL Server-database op het bedrijf of een SQL Server-database in een Azure virtuele machine. Back-up naar de cloud biedt de voordelen van de beschikbaarheid, onbegrensd geo gerepliceerd off-site op te slaan en gemakkelijk van de migratie van gegevens naar en vanuit de cloud. Back-up of terugzetten overzichten kunt u met behulp van Transact-SQL of SMO uitgeven.

SQL Server 2016 introduceert nieuwe mogelijkheden; [back-up bestand momentopname](http://msdn.microsoft.com/library/mt169363.aspx) kunt u vrijwel ogenblikkelijke back-ups en herstelt de ongelooflijk snel uitvoeren.

In dit onderwerp wordt uitgelegd waarom u Azure opslag gebruiken voor back-ups van SQL en vervolgens de betrokken onderdelen. De informatiebronnen die aan het einde van het artikel kunt u toegang krijgen tot de scenario's en aanvullende informatie bij het gebruik van deze service met uw back-ups van SQL Server.

## <a name="benefits-of-using-the-azure-blob-service-for-sql-server-backups"></a>Voordelen van het gebruik van de Service Azure Blob voor back-ups van SQL Server

Er zijn verschillende uitdagingen waarmee u wordt geconfronteerd wanneer u back-ups van SQL Server. Deze uitdagingen omvatten opslagbeheer risico opslag, toegang tot off-site op te slaan en de configuratie van de hardware. Veel van deze problemen worden opgelost met behulp van de service van de winkel Azure Blob voor back-ups van SQL Server. Houd rekening met de volgende voordelen:

- **Gebruiksgemak**: uw back-ups op te slaan in Azure BLOB's is een gemakkelijke, flexibele en gemakkelijk toegang te krijgen tot externe optie. Off-site opslag voor uw SQL Server back-ups maken is net zo eenvoudig als het wijzigen van uw bestaande scripts/taken naar de **BACKUP-URL** -syntaxis gebruiken. Externe opslag moet meestal ver genoeg van de locatie van de productie-database om te voorkomen dat een enkele ramp die mogelijk van invloed op zowel de buiten- en productie database-locaties. [Geo-repliceren uw Azure BLOB's](../storage/storage-redundancy.md)kiest, beschikt u over een extra niveau van bescherming in geval van een ramp die van invloed kan zijn op de hele regio.
- **Back-up archiveren**: de Azure Blob Storage-service biedt een beter alternatief voor de optie vaak gebruikte tape te archiveren, back-ups. Storage op tape mogelijk fysieke vervoer naar een off-site faciliteit en maatregelen ter bescherming van de media. Uw back-ups op te slaan in Azure Blob-opslag biedt een instant hoge beschikbaarheid en een duurzame optie archiveren.
- **Beheerde hardware**: Er is geen overhead van de hardwarebeheer met Azure services. Azure services beheren van de hardware en geo-replicatie voor redundantie en bescherming tegen hardwarestoringen.
- **Onbeperkte opslag**: als u een directe back-up naar Azure BLOB's inschakelt, hebt u toegang tot vrijwel onbeperkte opslag. Ook heeft tot een back-up een schijf Azure VM limieten op basis van de computer. Er is een limiet aan het aantal schijven die u aan een Azure virtual machine voor back-ups koppelen kunt. Deze limiet is 16 schijven voor een extra grote exemplaar en minder voor kleinere exemplaren.
- **Beschikbaarheid voor back-up**: back-ups opgeslagen in Azure BLOB's zijn beschikbaar vanaf elke locatie en op elk gewenst moment en gemakkelijk toegankelijk is voor het terugzetten naar een SQL-Server op locatie of een andere SQL Server wordt uitgevoerd in een Azure Virtual Machine, zonder de noodzaak van een database koppelen/ontkoppelen of te downloaden en te koppelen van de VHD.
- **Kosten**: betalen alleen voor de service die wordt gebruikt. Zijn voordelige optie off-site en back-up archiveren. Zie de [Azure prijzen calculator](http://go.microsoft.com/fwlink/?LinkId=277060 "Rekenmachine prijzen")en de [prijzen van Azure artikel](http://go.microsoft.com/fwlink/?LinkId=277059 "prijzen artikel") voor meer informatie.
- **Momentopnamen van de opslag**: als u met behulp van SQL Server 2016 databasebestanden worden opgeslagen in een Azure blob kunt u [back-up bestand momentopname](http://msdn.microsoft.com/library/mt169363.aspx) vrijwel ogenblikkelijke back-ups en herstelt zeer snel uit te voeren.

Zie [SQL Server-back-up en terugzetten met Azure Blob Storage-Service](http://go.microsoft.com/fwlink/?LinkId=271617)voor meer informatie.

De volgende twee secties introduceren de Azure Blob storage-service, met inbegrip van de vereiste onderdelen van SQL Server. Het is belangrijk om te begrijpen van de onderdelen en de interactie ervan met succes met back-up en terugzetten uit de Azure Blob storage-service.

## <a name="azure-blob-storage-service-components"></a>Azure Blob Storage Service-onderdelen

De volgende onderdelen Azure worden gebruikt als back-up met de Azure Blob storage-service.

| Onderdeel               | Beschrijving                          |
|---------------------|-------------------------------|
| **Opslag Account** | De opslag is het beginpunt voor alle opslagservices. Voor toegang tot een Azure Blob Storage-service, maakt u eerst een account Azure opslag. Zie voor meer informatie over Azure Blob storage-service, [het gebruik van de Azure Blob Storage-Service](https://azure.microsoft.com/develop/net/how-to-guides/blob-storage/) |
| **Container** | Een container een groepering van een aantal BLOB's biedt, en een onbeperkt aantal BLOB's kunt opslaan. Als u wilt schrijven, een SQL Server back-up naar een service Azure Blob moet ten minste de basiscontainer gemaakt. |
| **BLOB** | Een bestand van het type en de grootte. BLOB's worden gebruikt met de volgende URL-notatie: **https://[storage account].blob.core.windows.net/[container]/[blob]**. Zie voor meer informatie over pagina-BLOB's [Wat blok en BLOB's pagina](http://msdn.microsoft.com/library/azure/ee691964.aspx) |

## <a name="sql-server-components"></a>SQL Server-onderdelen

De volgende onderdelen van SQL Server worden gebruikt als back-up met de Azure Blob storage-service.

| Onderdeel               | Beschrijving                          |
|---------------------|-------------------------------|
| **URL** | Een URL bevat een URI Uniform Resource Identifier () een unieke back-upbestand. De URL wordt gebruikt om de locatie en de naam van de back-up van SQL Server. De URL moet verwijzen naar een werkelijke blob, niet slechts een container. Als de blob niet bestaat, wordt deze gemaakt. Als een bestaande blob is opgegeven, back-up mislukt, tenzij de > met optie is opgegeven. Het volgende is een voorbeeld van de URL u in de opdracht back-up geeft: **http[s]://[storageaccount].blob.core.windows.net/[container]/[FILENAME.bak]**. HTTPS wordt aanbevolen maar niet verplicht. |
| **Referentie** | De informatie die nodig is om te verbinden en te verifiëren met Azure Blob storage-service wordt opgeslagen als een referentie.  SQL Server back-ups schrijven naar een Azure Blob of terugzetten uit worden een SQL Server-referenties gemaakt. Zie [SQL Server-referenties](https://msdn.microsoft.com/library/ms189522.aspx)voor meer informatie. |

> [AZURE.NOTE] Als u ervoor kiest om te kopiëren en een back-up bestand uploaden naar de Azure Blob storage-service, moet u een paginatype blob als opslagoptie als u van plan bent dit bestand te gebruiken voor het terugzetten van de systeemstatus. TERUGZETTEN van een blob Bloktype mislukt met een fout.

## <a name="next-steps"></a>Volgende stappen

1. Maak een Azure-account als u nog niet hebt. Als u Azure evalueert, kunt u de [gratis proefversie](https://azure.microsoft.com/free/).

1. Ga via een van de volgende zelfstudies die u helpt bij het maken van een account voor de opslag en het uitvoeren van een herstelbewerking.

    - **SQL Server 2014**: [Zelfstudie: SQL Server 2014 back-up en herstel van Microsoft Azure Blob Storage-Service](https://msdn.microsoft.com/library/jj720558\(v=sql.120\).aspx).
    - **SQL Server 2016**: [Zelfstudie: met behulp van de Microsoft Azure Blob storage-service met databases van SQL Server 2016](https://msdn.microsoft.com/library/dn466438.aspx)

1. Bekijk aanvullende documentatie beginnen met [SQL Server back-up en terugzetten met Microsoft Azure Blob Storage-Service](https://msdn.microsoft.com/library/jj919148.aspx).

Als u problemen hebt, bekijk het onderwerp op [SQL Server back-up URL Best Practices en probleemoplossing](https://msdn.microsoft.com/library/jj919149.aspx).

Voor andere SQL Server back-up maken en opties voor terugzetten, [back-up](../virtual-machines/virtual-machines-windows-sql-backup-recovery.md)en herstel voor SQL Server in Azure virtuele Machines.
