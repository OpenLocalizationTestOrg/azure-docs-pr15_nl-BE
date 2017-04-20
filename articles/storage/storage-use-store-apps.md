<properties
    pageTitle="Azure opslag gebruiken in de Windows Store apps | Microsoft Azure"
    description="Informatie over het maken van een Windows Store-app die gebruikmaakt van Azure Blob, wachtrij, tabel of bestand opslag."
    services="storage"
    documentationCenter=""
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="mobile-windows-store"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>
    
# <a name="how-to-use-azure-storage-in-windows-store-apps"></a>Azure opslag gebruiken in Windows Store apps

## <a name="overview"></a>Overzicht

Deze handleiding wordt beschreven hoe aan de slag met het ontwikkelen van een Windows Store app die gebruik maakt van Azure opslag.

## <a name="download-required-tools"></a>Vereiste hulpprogramma's downloaden

- [Visual Studio](https://www.visualstudio.com/en-us/visual-studio-homepage-vs.aspx) kunt u gemakkelijk bouwen, opsporen, lokaliseren, pakket, en Windows Store apps implementeren. Visual Studio 2012 of hoger is vereist.
- De [Client-bibliotheek van Azure opslag](https://www.nuget.org/packages/WindowsAzure.Storage) biedt een Windows Runtime class-bibliotheek voor het werken met Azure opslag.
- [WCF Data Services hulpprogramma's voor Windows Store Apps](http://www.microsoft.com/download/details.aspx?id=30714) breidt de Serviceverwijzing toevoegen-ervaring met client-side OData-ondersteuning voor Windows Store apps in Visual Studio.

## <a name="develop-apps"></a>Apps ontwikkelen

### <a name="getting-ready"></a>Voorbereiden

Maak een nieuw Windows Store app-project in Visual Studio 2012 of later:

![Store-apps-opslag-vs-project][store-apps-storage-vs-project]

Vervolgens wordt een verwijzing naar de Client-bibliotheek van Azure opslag toevoegen door met de rechtermuisknop op **References**, op **Add Reference**te klikken en vervolgens Bladeren naar opslag Client Library voor Windows Runtime die u hebt gedownload:

![Store-apps-opslag-Kies-bibliotheek][store-apps-storage-choose-library]

### <a name="using-the-library-with-the-blob-and-queue-services"></a>Met behulp van de bibliotheek met de Blob- en wachtrij

Uw app is nu klaar om te bellen Azure Blob en wachtrij. De volgende instructies voor het **gebruik van** toevoegen zodat Azure opslagtypen rechtstreeks kunnen worden verwezen:

    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Auth;

Vervolgens een knop toevoegen aan uw pagina. De volgende code toevoegen aan de gebeurtenis **Click** en de gebeurtenis-handler-methode met de [asynchrone trefwoord](http://msdn.microsoft.com/library/vstudio/hh156513.aspx)wijzigen:

    var credentials = new StorageCredentials(accountName, accountKey);
    var account = new CloudStorageAccount(credentials, true);
    var blobClient = account.CreateCloudBlobClient();
    var container = blobClient.GetContainerReference("container1");
    await container.CreateIfNotExistsAsync();

Deze code wordt aangenomen dat er twee tekenreeksvariabelen, *accountnaam* en *accountKey*. De naam van uw account voor de opslag en de account-sleutel die is gekoppeld aan die account staan.

Bouwen en uitvoeren van de toepassing. Op de knop controleren of een container met de naam *container1* in uw account bestaat en maken als dat niet.

### <a name="using-the-library-with-the-table-service"></a>Met behulp van de bibliotheek met de tabel service

Typen die worden gebruikt om te communiceren met de tabel Azure-service is afhankelijk van WCF-Services gegevens voor de Windows Store app bibliotheek. Vervolgens wordt een verwijzing naar WCF vereiste bibliotheken toevoegen met behulp van de Package Manager-Console:

![Store-apps-opslag-pakket-manager][store-apps-storage-package-manager]

Gebruik de volgende opdracht om punt Package Manager naar de locatie op uw computer:

    Install-Package Microsoft.Data.OData.WindowsStore -Source "C:\Program Files (x86)\Microsoft WCF Data Services\5.0\bin\NuGet"

Met deze opdracht worden alle vereiste verwijzingen naar uw project automatisch toegevoegd. Als u niet gebruiken de Package Manager-Console wilt, kunt u de map WCF Data Services NuGet op uw lokale computer toevoegen aan de lijst met bronnen pakket en voegt u de verwijzing via de gebruikersinterface, zoals wordt beschreven in het [Beheren van NuGet pakketten met behulp van het dialoogvenster](http://docs.nuget.org/docs/start-here/Managing-NuGet-Packages-Using-The-Dialog).

Als u verwijst naar de WCF Data Services NuGet package, wijzig de code in de gebeurtenis **Click** van de knop:

    var credentials = new StorageCredentials(accountName, accountKey);
    var account = new CloudStorageAccount(credentials, true);
    var tableClient = account.CreateCloudTableClient();
    var table = tableClient.GetTableReference("table1");
    await table.CreateIfNotExistsAsync();

Deze code controleert of een tabel met de naam *tabel1* in uw account bestaat en maakt zo niet.

U kunt een verwijzing naar Microsoft.WindowsAzure.Storage.Table.dll, ook toevoegen die beschikbaar is in hetzelfde pakket dat u hebt gedownload. Deze bibliotheek bevat aanvullende functionaliteit, zoals serialisatie op basis van reflectie en algemene query's. Houd er rekening mee dat deze bibliotheek biedt geen ondersteuning voor JavaScript.



[store-apps-storage-vs-project]: ./media/storage-use-store-apps/store-apps-storage-vs-project.png
[store-apps-storage-choose-library]: ./media/storage-use-store-apps/store-apps-storage-choose-library.png
[store-apps-storage-package-manager]: ./media/storage-use-store-apps/store-apps-storage-package-manager.png
