<properties 
    pageTitle="Gegevens verplaatsen en naar Azure Blob-opslag met Opslagverkenner Azure | Microsoft Azure" 
    description="Gegevens verplaatsen en naar Azure Blob-opslag met Azure Opslagverkenner" 
    services="machine-learning,storage" 
    documentationCenter="" 
    authors="bradsev" 
    manager="jhubbard" 
    editor="cgronlun" />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/31/2016"
    ms.author="bradsev" />

# <a name="move-data-to-and-from-azure-blob-storage-using-azure-storage-explorer"></a>Gegevens verplaatsen en naar Azure Blob-opslag met Azure Opslagverkenner

Azure Opslagverkenner is een gratis hulpprogramma van Microsoft waarmee u kunt werken met gegevens in Azure opslag op Windows, macOS en Linux. In dit onderwerp wordt beschreven hoe om te uploaden en downloaden van gegevens uit de Azure blobopslag. Het hulpprogramma kan worden gedownload van [Microsoft Azure Opslagverkenner](http://storageexplorer.com/).

Advies over technologieën die worden gebruikt om gegevens te verplaatsen naar of uit de Azure Blob-opslag hier zijn gekoppeld:
 
[AZURE.INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]   

 
> [AZURE.NOTE] Als u van VM die is ingesteld met de scripts die [gegevens wetenschap virtuele machines in Azure gebruikmaakt](machine-learning-data-science-virtual-machines.md), is vervolgens Opslagverkenner Azure al geïnstalleerd op de VM.
 
> [AZURE.NOTE] Verwijzen naar [Azure Blob grondbeginselen](../storage/storage-dotnet-how-to-use-blobs.md) en [Azure Blob-Service](https://msdn.microsoft.com/library/azure/dd179376.aspx)voor een volledige inleiding op Azure blob-opslag.   

## <a name="prerequisites"></a>Vereisten

Dit document wordt ervan uitgegaan dat er een Azure-abonnement, een opslag-account en de bijbehorende sleutel opslag voor die account. Voor het uploaden/downloaden van gegevens, moet u uw account Azure opslag de sleutel de naam en account weten. 

- Zie instellen van een Azure-abonnement, [gratis proefperiode van een maand](https://azure.microsoft.com/pricing/free-trial/).
- Zie voor instructies over het maken van een account voor opslag en voor rekening en belangrijke informatie ophalen [over Azure opslag rekeningen](../storage/storage-create-storage-account.md). Noteer de toegangstoets voor uw opslag-account als u deze sleutel verbinding maken met de account met het hulpprogramma Azure Opslagverkenner nodig.
- Het hulpprogramma Azure Opslagverkenner kan worden gedownload van [Microsoft Azure Opslagverkenner](http://storageexplorer.com/). Accepteer de standaardinstellingen tijdens de installatie.


<a id="explorer"></a>
## <a name="use-azure-storage-explorer"></a>Azure Opslagverkenner gebruiken 

De volgende stappen documenteren hoe uploaden/downloaden van gegevens met behulp van Opslagverkenner Azure. 

1.  Start Microsoft Azure Opslagverkenner.
2.  Selecteer **Accountinstellingen Azure** -pictogram, vervolgens **een account toevoegen** om de wizard **aanmelden bij uw account** , en geef de referenties. ![](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/add-an-azure-store-account.png)
3.  Om de wizard **verbinding maken met Azure opslag** , selecteer het pictogram **verbinding maken met Azure opslag** . ![](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/connect-to-azure-storage-1.png)
4. Voer de toegangstoets uit uw account Azure opslag op de wizard **verbinding maken met Azure opslag** en klik op **volgende**. ![](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/connect-to-azure-storage-2.png)
5. Opslag accountnaam invoeren in het vak **accountnaam** en vervolgens **volgende**. ![](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/attach-external-storage.png)
6. De opslag account toegevoegd moet nu worden weergegeven. Een blob-container in een opslag-account maken, met de rechtermuisknop op het knooppunt **Blob Containers** in die account, **Maken Blob Container**selecteren en voer een naam in.
7. Uploaden van gegevens naar een container, de doelcontainer selecteren en klik op de knop **uploaden** .![](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/storage-accounts.png)
8. Klik op de **...** rechts van **het vak** , selecteert u een of meerdere bestanden voor het uploaden van het bestandssysteem en klik op **uploaden** om te beginnen met het uploaden van de bestanden.![](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/upload-files-to-blob.png)
7. Voor het downloaden van gegevens, de blob te selecteren in de bijbehorende container wilt downloaden en klik op **downloaden**. ![](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/download-files-from-blob.png)


