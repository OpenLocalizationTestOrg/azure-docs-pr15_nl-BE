<properties
    pageTitle="Aan de slag met Azure opslag in vijf minuten | Microsoft Azure"
    description="Snel aangeeft hoeveel van de op Microsoft Azure BLOB's, tabellen en wachtrijen met Azure snel begonnen, Visual Studio en de opslag van Azure-emulator. De eerste toepassing van Azure opslag in vijf minuten worden uitgevoerd."
    services="storage"
    documentationCenter=".net"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="tamram"/>

# <a name="get-started-with-azure-storage-in-five-minutes"></a>Aan de slag met Azure opslag in vijf minuten

## <a name="overview"></a>Overzicht

Het is eenvoudig aan de slag met Azure opslag ontwikkelen. In deze zelfstudie wordt beschreven hoe u een toepassing Azure opslag van snel gebruiksklaar. Gebruikt u de Quick Start sjablonen met de Azure SDK voor .NET. Deze snelle start bevatten kant-en-klaar-code enkele standaardscenario programmeren met Azure opslag demonstreert.

Zie voor meer informatie over opslag Azure vooraleer de code, de [Volgende stappen](#next-steps).

## <a name="prerequisites"></a>Vereisten

U moet de volgende vereisten voordat u begint:

1. Voor het compileren en bouwen van de toepassing, moet u een versie van [Visual Studio](https://www.visualstudio.com/) is geïnstalleerd op uw computer.

2. Installeer de nieuwste versie van [Azure SDK voor .NET](https://azure.microsoft.com/downloads/). De SDK bevat de voorbeeldprojecten Azure QuickStart, de emulator Azure opslag en de [Clientbibliotheek van Azure opslag voor .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).

3. Zorg dat u [.NET Framework 4.5](http://www.microsoft.com/download/details.aspx?id=30653) worden geïnstalleerd op uw computer, als dat nodig is voor de Azure QuickStart voorbeeldprojecten die we in deze zelfstudie wilt gebruiken.

    Als u niet zeker welke versie van .NET Framework op uw computer is geïnstalleerd weet, raadpleegt u [procedure: bepalen welke .NET Framework-versies zijn geïnstalleerd](https://msdn.microsoft.com/vstudio/hh925568.aspx). Of druk op de knop **Start** of de Windows-toets, typ **Configuratiescherm**. Klik vervolgens op **programma's** > **programma's en onderdelen**, en te bepalen of het .NET Framework 4.5 tussen de geïnstalleerde programma's wordt weergegeven.

4. Je hebt een Azure-abonnement en een account Azure opslag.

    - Als u een abonnement op Azure, Zie [Gratis proefversie](https://azure.microsoft.com/pricing/free-trial/) [Inkoopopties](https://azure.microsoft.com/pricing/purchase-options/)en [Biedt de](https://azure.microsoft.com/pricing/member-offers/) (voor leden van MSDN, de Microsoft Partner Network en BizSpark en andere Microsoft-programma's).
    - Zie maken van een account voor opslag in Azure [een opslag-account maken](storage-create-storage-account.md#create-a-storage-account).

## <a name="run-your-first-azure-storage-application-against-azure-storage-in-the-cloud"></a>Uw eerste Azure opslag toepassing uitvoeren op Azure opslag in de cloud

Als u een account hebt, kunt u een eenvoudige opslag Azure-toepassing met een van de voorbeeldprojecten Azure snel begint in Visual Studio. Deze zelfstudie is gericht op de voorbeeldprojecten voor Azure opslag: **Azure opslag: BLOB's**, **opslag Azure: bestanden**, **opslag Azure: wachtrijen**, en **Azure opslag: tabellen**:

1. Start Visual Studio.
2. Klik in het menu **bestand** op **Nieuw Project**.
3. Klik in het dialoogvenster **Nieuw Project** **Installed** > **sjablonen** > **Visual C#** > **wolk** > **QuickStart** > **Data Services**.
    een. Kies een van de volgende sjablonen: **Azure opslag: BLOB's**, **opslag Azure: bestanden**, **opslag Azure: wachtrijen**, of **Azure opslag: tabellen**.
    b. Zorg ervoor dat **.NET Framework 4.5** is geselecteerd als de target-kader.
    - 3.c. Geef een naam op voor uw project en de nieuwe Visual Studio-oplossing te maken zoals:

    ![Azure snel aan de slag][Image1]

U kunt de broncode bekijken voordat u de toepassing uitvoert. Selecteert u de code bekijken, **Solution Explorer** in het menu **Beeld** in Visual Studio. Vervolgens dubbelklikt u op het bestand Program.cs.

Voer de voorbeeldtoepassing:

1.  Selecteer in Visual Studio **Solution Explorer** in het menu **Beeld** . Open de App.config bestand en het commentaar van de verbindingsreeks voor de opslag van Azure-emulator:

    `<!--<add key="StorageConnectionString" value = "UseDevelopmentStorage=true;"/>-->`

2.  Opmerking de verbindingsreeks voor de Azure Storage-Service verwijderen en de opslag account naam en toegang tot sleutel opgeven in het bestand App.config:`<add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=[AccountName];AccountKey=[AccountKey]"`

    Voor het ophalen van uw sleutel opslag account toegang, Zie [beheren van uw opslag toegangstoetsen](storage-create-storage-account.md#manage-your-storage-access-keys).

3.  Nadat u de naam van de opslag en toegangssleutel in het bestand App.config in het menu **bestand** klikt u op **Alles opslaan** om alle projectbestanden opslaan.
4.  Klik op het menu **Build** **Oplossing bouwen**.
5.  Druk op **F11** om de oplossing van stap voor stap uitvoeren of druk op **F5** om de oplossing uitvoeren in het menu **Foutopsporing** .


## <a name="run-your-first-azure-storage-application-locally-against-the-azure-storage-emulator"></a>De eerste toepassing van de opslag van Azure lokaal uitvoeren tegen de Emulator Azure opslag

De [Opslag-Emulator Azure](storage-use-emulator.md) biedt een lokale omgeving dat de Azure Blob, wachtrij en tabel diensten voor ontwikkelingsdoeleinden emuleert. De emulator opslag kunt u de opslag toepassing lokaal testen zonder dat er een abonnement op Azure of opslag account en zonder enige kosten.

Probeer het maken we een eenvoudige opslag Azure-toepassing met een van de voorbeeldprojecten Azure snel begint in Visual Studio. Deze zelfstudie is gericht op de **Azure Blob-opslag**, **Opslag van Azure tabel**en **Azure Queue Storage** voorbeeldprojecten:

1. Start Visual Studio.
2. Klik in het menu **bestand** op **Nieuw Project**.
3. Klik in het dialoogvenster **Nieuw Project** **Installed** > **sjablonen** > **Visual C#** > **wolk** > **QuickStart** > **Data Services**.
    een. Kies een van de volgende sjablonen: **Azure opslag: BLOB's**, **opslag Azure: bestanden**, **opslag Azure: wachtrijen**, of **Azure opslag: tabellen**.
    b. Zorg ervoor dat **.NET Framework 4.5** is geselecteerd als de target-kader.
    c. Geef een naam op voor uw project en de nieuwe Visual Studio-oplossing te maken zoals:

    ![Azure snel aan de slag][Image1]

4.  Selecteer in Visual Studio **Solution Explorer** in het menu **Beeld** . Het App.config bestand openen en de verbindingsreeks voor uw account Azure opslag commentaar als u deze al hebt toegevoegd. Vervolgens behandelen de verbindingsreeks voor de opslag van Azure-emulator:

    `<add key="StorageConnectionString" value = "UseDevelopmentStorage=true;"/>`

U kunt de broncode bekijken voordat u de toepassing uitvoert. Selecteert u de code bekijken, **Solution Explorer** in het menu **Beeld** in Visual Studio. Vervolgens dubbelklikt u op het bestand Program.cs.

Voer de voorbeeldtoepassing in de Emulator Azure opslag:

1.  Druk op de knop **Start** of de Windows-toets, zoekt *Microsoft Azure Storage emulator*, en start de toepassing. Wanneer de emulator wordt gestart, ziet u een pictogram en een bericht in de taakweergave van Windows.
2.  In Visual Studio, klikt u op **Oplossing bouwen** in het menu **Build** .
3.  Druk op **F11** om de oplossing van stap voor stap uitvoeren in het menu **Foutopsporing** of druk op **F5** om de oplossing van begin tot einde uitvoeren.

## <a name="next-steps"></a>Volgende stappen

Zie de volgende bronnen voor meer informatie over Azure opslag:

* [Inleiding tot Microsoft Azure opslag](storage-introduction.md)
* [Aan de slag met Azure Opslagverkenner](../vs-azure-tools-storage-manage-with-storage-explorer.md)
* [Aan de slag met Azure Blob-opslag met behulp van .NET](storage-dotnet-how-to-use-blobs.md)
* [Aan de slag met Azure tabel opslag met behulp van .NET](storage-dotnet-how-to-use-tables.md)
* [Aan de slag met Azure Queue Storage met .NET](storage-dotnet-how-to-use-queues.md)
* [Aan de slag met Azure opslaan van bestanden in Windows](storage-dotnet-how-to-use-files.md)
* [Overdracht van gegevens met het hulpprogramma AzCopy voor opdrachtregel](storage-use-azcopy.md)
* [Azure opslag documentatie](https://azure.microsoft.com/documentation/services/storage/)
* [Microsoft Azure opslag Client Library for .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx)
* [Azure opslagservices REST API](https://msdn.microsoft.com/library/azure/dd179355.aspx)

[Image1]: ./media/storage-getting-started-guide/QuickStart.png
