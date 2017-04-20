<properties
    pageTitle="Wat is er gebeurd met mijn project cloud service? | Microsoft Azure | Visual Studio verbonden services"
    description="Hierin wordt beschreven wat er gebeurt in een cloud services-project nadat services verbinding maken met een opslag van Azure-account met behulp van Visual Studio worden verbonden"
    services="storage"
    documentationCenter=""
    authors="TomArcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="storage"
    ms.workload="web"
    ms.tgt_pltfrm="vs-what-happened"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="tarcher"/>

# <a name="what-happened-to-my-cloud-services-project-visual-studio-azure-storage-connected-service"></a>Wat is er gebeurd met mijn cloud services-project (Visual Studio Azure opslag verbonden service)?

## <a name="references-added"></a>Verwijzingen toegevoegd

Het pakket Azure opslag NuGet is toegevoegd aan de Visual Studio-project.  
Dit pakket wordt de volgende referenties voor .NET toegevoegd:

- **Microsoft.Data.Edm**
- **Microsoft.Data.OData**
- **Microsoft.Data.Services.Client**
- **Microsoft.WindowsAzure.Configuration**
- **Microsoft.WindowsAzure.Storage**
- **Newtonsoft.Json**
- **System.Data**
- **System.Spatial**

## <a name="connection-string-for-azure-storage-added"></a>De verbindingsreeks voor de opslag van Azure toegevoegd
Elementen zijn gemaakt met de verbindingsreeks en de sleutel van de geselecteerde opslag account. Wijzigingen zijn aangebracht in de volgende bestanden:

- **ServiceDefinition.csdef**
- **ServiceConfiguration.Cloud.cscfg**
- **ServiceConfiguration.Local.cscfg**
