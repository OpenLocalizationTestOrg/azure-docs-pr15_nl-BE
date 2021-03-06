<properties
    pageTitle="Wat is er gebeurd met mijn 5 ASP.NET-project (Visual Studio verbonden services) | Microsoft Azure opslag"
    description="Hierin wordt beschreven wat er gebeurt nadat verbinding te maken met een account met Azure opslag in een project met behulp van Visual Studio Visual Studio ASP.NET 5 services verbonden"
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

# <a name="what-happened-to-my-aspnet-5-project-visual-studio-azure-storage-connected-services"></a>Wat is er gebeurd met mijn 5 ASP.NET-project (Visual Studio Azure opslag verbonden services)?

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

Ook het pakket NuGet **Microsoft.Framework.Configuration.Json** is toegevoegd.

## <a name="connection-string-for-azure-storage-added"></a>De verbindingsreeks voor de opslag van Azure toegevoegd
Een element is in het bestand config.json van uw project met de verbindingsreeks en de sleutel van de geselecteerde opslag account gemaakt.

Zie [ASP.NET-5](http://www.asp.net/vnext)voor meer informatie.
