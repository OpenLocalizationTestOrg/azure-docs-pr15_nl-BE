<properties
    pageTitle="Verplaatsen van gegevens naar en van Azure opslag | Microsoft Azure"
    description="Dit artikel biedt een overzicht van de verschillende methoden voor het verplaatsen van gegevens naar en van Azure opslag."
    services="storage"
    documentationCenter=""
    authors="micurd"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/21/2016"
    ms.author="micurd"/>

# <a name="moving-data-to-and-from-azure-storage"></a>Verplaatsen van gegevens naar en van Azure opslag

Als u wilt verplaatsen op locatie gegevens opslag Azure (of vice versa), zijn er verschillende manieren om dit te doen. De benadering die het meest geschikt voor u is afhankelijk van uw scenario. Dit artikel biedt een snel overzicht van de verschillende scenario's en passende oplossingen voor elke.

## <a name="building-applications"></a>Toepassingen maken

Als u een toepassing maakt, is ontwikkelen ten opzichte van de REST API of een van onze vele clientbibliotheken een uitstekende manier om gegevens te verplaatsen naar en van Azure opslag.

Azure opslag biedt rijke bibliotheken voor .NET, iOS, Java, Android, Universal Windows Platform (UWP), Xamarin, C++, Node.JS, PHP, Ruby en Python. De client libraries bieden geavanceerde mogelijkheden zoals opnieuw logica, logboekregistratie en parallelle uploads. U kunt ook rechtstreeks tegen de REST-API kan worden aangeroepen door een taal waarmee HTTP/HTTPS-verzoeken ontwikkelen.

Zie [Aan de slag met Azure Blob-opslag](storage-dotnet-how-to-use-blobs.md) voor meer informatie.

Daarnaast bieden wij ook de [Azure opslag Data verkeer Library](https://www.nuget.org/packages/Microsoft.Azure.Storage.DataMovement) een bibliotheek die is ontworpen is voor krachtige kopiëren van gegevens van en naar Azure. Raadpleeg onze Data verkeer Library- [documentatie](https://github.com/Azure/azure-storage-net-data-movement) voor meer informatie. 

## <a name="quickly-viewinginteracting-with-your-data"></a>Snel interactie bekijken/met uw gegevens

Als u wilt dat een eenvoudige manier om uw Azure opslag gegevens weergeven en ook hebben de mogelijkheid om te uploaden en downloaden van uw gegevens vervolgens kunt u overwegen een Opslagverkenner Azure.

Bekijk onze lijst van [Azure opslag Explorers](storage-explorers.md) voor meer informatie.

## <a name="system-administration"></a>System Administration

Als u nodig hebben of meer vertrouwd met een hulpprogramma voor de opdrachtregel (bv. voor systeembeheerders bent), zijn hier een aantal opties die u kunt overwegen:

### <a name="azcopy"></a>AzCopy

AzCopy is een opdrachtregelprogramma van Windows ontworpen voor hoge prestaties kopiëren van gegevens van en naar Azure opslag. U kunt ook kopiëren van gegevens binnen een account voor opslag of tussen verschillende opslag rekeningen.

Zie de [overdracht van gegevens met het hulpprogramma AzCopy voor Command-Line](storage-use-azcopy.md) voor meer informatie.

### <a name="azure-powershell"></a>Azure PowerShell

Azure PowerShell is een module waarmee cmdlets voor het beheren van services op Azure. Het is een taak gebaseerde opdrachtregel shell en scripttaal speciaal ontworpen voor systeembeheer.

Zie [Met behulp van Azure PowerShell met Azure opslag](storage-powershell-guide-full.md) voor meer informatie.

### <a name="azure-cli"></a>Azure CLI

Azure CLI biedt een set van open source platforms opdrachten voor het werken met Azure services. Azure CLI is beschikbaar op Windows, OSX en Linux.

Zie [met behulp van de CLI Azure met Azure opslag](storage-azure-cli.md) voor meer informatie.

## <a name="moving-large-amounts-of-data-with-a-slow-network"></a>Verplaatsen van grote hoeveelheden gegevens met een langzaam netwerk

Een van de grootste uitdagingen die is gekoppeld aan het verplaatsen van grote hoeveelheden gegevens is de overdrachtstijd. Als u gegevens ophalen uit de opslag Azure wilt zonder zorgen te maken over de kosten van netwerken of u code schrijft, wordt een passende oplossing met Azure importeren/exporteren.

Zie [Azure importeren/exporteren](storage-import-export-service.md) voor meer informatie.

## <a name="backing-up-your-data"></a>Back-ups van uw gegevens

Als u gewoon back-up van uw gegevens naar de opslag in Azure, is Azure back-up de manier om te gaan. Dit is een krachtige oplossing voor back-ups van gegevens op gebouwen en Azure VMs.

Zie [Azure back-up](../backup/backup-introduction-to-azure-backup.md) voor meer informatie.

## <a name="accessing-your-data-on-premises-and-from-the-cloud"></a>Toegang tot uw gegevens op locatie en vanuit de cloud

Als u een oplossing nodig voor toegang tot uw gegevens op locatie en vanuit de cloud, moet vervolgens u overwegen de Azure hybride cloud storage oplossing, StorSimple. Deze oplossing bestaat uit een fysieke StorSimple apparaat dat op intelligente wijze winkels vaak gegevens gebruikt op een SSD, af en toe gebruikte gegevens op harde schijven en inactieve/backup/archief data in Azure opslag.

Zie [StorSimple](../storsimple/storsimple-overview.md) voor meer informatie.

## <a name="recovering-your-data"></a>Uw gegevens herstellen

Wanneer u werkbelasting op gebouwen en toepassingen hebt, moet u een oplossing waarmee uw bedrijf worden uitgevoerd in geval van een ramp. Azure-Site-Recovery zorgt voor replicatie, failover en herstel van virtuele machines en fysieke servers. Gerepliceerde gegevens worden opgeslagen in Azure, opslag, zodat u geen omkijken naar een secundaire on-site datacenter.

Zie [Herstel van Azure-Site](../site-recovery/site-recovery-overview.md) voor meer informatie.
