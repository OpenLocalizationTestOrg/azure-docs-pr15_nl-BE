<properties 
    pageTitle="Het instellen van de Computer voor Media Services ontwikkelen met .NET" 
    description="Meer informatie over de vereisten voor de Media-Services met behulp van de Media Services SDK voor .NET. Ook informatie over het maken van een Visual Studio-app." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="10/24/2016"  
    ms.author="juliako"/>

#<a name="media-services-development-with-net"></a>Media Services ontwikkelen met .NET

[AZURE.INCLUDE [media-services-selector-setup](../../includes/media-services-selector-setup.md)]

In dit onderwerp wordt beschreven hoe de ontwikkeling van Media Services-toepassingen met .NET.

De bibliotheek **Azure Media Services.NET SDK** kunt u programmeren in Media-Services met behulp van .NET. Om het nog gemakkelijker te ontwikkelen met .NET, wordt de bibliotheek **Uitbreidingen van Azure Media Services .NET SDK** geleverd. Deze bibliotheek bevat een set van uitbreidingsmethoden en Help-functies die uw .NET-code wordt vereenvoudigd. Beide bibliotheken zijn beschikbaar via **NuGet** en **GitHub**.


##<a name="prerequisites"></a>Vereisten

-   Een Media-Services-account in een nieuwe of bestaande Azure abonnement. Zie het onderwerp [een Media-Services-Account te maken](media-services-portal-create-account.md).
-   Besturingssystemen: Windows 10, Windows 7, Windows 2008 R2 of Windows 8.
-   .NET framework 4.5.
-    Visual Studio 2015, Visual Studio 2013, Visual Studio 2012 of Visual Studio 2010 SP1 (Professional, Premium, Ultimate of Express).


##<a name="create-and-configure-a-visual-studio-project"></a>Maken en configureren van een Visual Studio-project

In deze sectie wordt beschreven hoe u een project maken in Visual Studio en instellen voor de ontwikkeling van Media Services.  In dit geval het project is een consoletoepassing C# Windows, maar de installatiestappen die hier worden weergegeven van toepassing op andere soorten projecten die kunt u Media Services-toepassingen (bijvoorbeeld een Windows Forms-toepassing of een ASP.NET-webtoepassing) maken.

In dit gedeelte ziet u hoe met **NuGet** Media Services .NET SDK en de andere afhankelijke bibliotheken toevoegt.

U kunt ook ophalen van de meest recente Media Services .NET SDK bits van GitHub ([github.com/Azure/azure-sdk-for-media-services](https://github.com/Azure/azure-sdk-for-media-services) en [github.com/Azure/azure-sdk-for-media-services-extensions](https://github.com/Azure/azure-sdk-for-media-services-extensions)), maak de oplossing en de verwijzingen naar de client-project toevoegen. Houd er rekening mee dat alle vereiste afhankelijkheden gedownload en automatisch uitgepakt.

1. Maak een nieuwe C#-consoletoepassing in Visual Studio 2010 SP1 of hoger van de VS. Voer de **naam**, de **locatie**en de **naam van de oplossing**en klik vervolgens op OK.

2. Maak de oplossing.

2. Gebruik **NuGet** installeren en **Azure Media Services .NET SDK extensies**toevoegen. Installatie van dit pakket ook **.NET SDK van Media Services** wordt geïnstalleerd en alle vereiste afhankelijkheden toegevoegd.

    Zorg ervoor dat u de nieuwste versie van NuGet geïnstalleerd hebben. Zie voor meer informatie en installatie-instructies, [NuGet](http://nuget.codeplex.com/).

2. In Solution Explorer met de rechtermuisknop op de naam van het project en kies pakketten NuGet beheren...

    Het dialoogvenster NuGet pakketten beheren wordt weergegeven.

3. In de on line galerie Azure MediaServices extensies zoekt, kies Azure Media Services .NET SDK-extensies en klik vervolgens op de knop installeren.

    Het project is gewijzigd en verwijzingen naar de Media Services .NET SDK-uitbreidingen, Media Services .NET SDK en andere afhankelijke assembly's worden toegevoegd.

4. Ter bevordering van een schonere ontwikkelomgeving in te schakelen NuGet pakket herstellen. Zie voor meer informatie [NuGet pakket herstellen '](http://docs.nuget.org/consume/package-restore).

3. Voeg een verwijzing naar de assembly **System.Configuration** . Deze assembly bevat de System.Configuration. **ConfigurationManager** -klasse die wordt gebruikt voor toegang tot de configuratiebestanden (bijvoorbeeld App.config).

    Met het dialoogvenster referenties beheren referentie wilt toevoegen met de rechtermuisknop op de naam van het project in de Solution Explorer. Selecteer toevoegen en verwijzingen.

    Het dialoogvenster referenties beheren verschijnt.

4. Onder het .NET framework-assemblages, zoeken, selecteer de assembly System.Configuration en druk op OK.
5. Open het bestand App.config (het bestand toevoegen aan uw project als het is niet standaard toegevoegd) en een sectie *appSettings* toevoegen aan het bestand.     
Stel de waarden voor uw Azure Media Services-account de naam en account sleutel, zoals in het volgende voorbeeld wordt getoond.

    Als de naam en sleutel waarden, gaat u naar de portal Azure en selecteer uw account. Het venster instellingen verschijnt aan de rechterkant. Selecteer in het venster Instellingen toetsen. De waarde te klikken op het pictogram naast elk tekstvak worden gekopieerd naar het systeemklembord.


        <configuration>
        ...
            <appSettings>
              <add key="MediaServicesAccountName" value="Media-Services-Account-Name" />
              <add key="MediaServicesAccountKey" value="Media-Services-Account-Key" />
            </appSettings>

        </configuration>

6. De bestaande **met behulp van** instructies aan het begin van het Program.cs-bestand overschrijven met de volgende code.

        using System;
        using System.Collections.Generic;
        using System.Linq;
        using System.Text;
        using System.Threading.Tasks;
        using System.Configuration;
        using System.Threading;
        using System.IO;
        using Microsoft.WindowsAzure.MediaServices.Client;

U bent nu klaar om te gaan met het ontwikkelen van een toepassing Media Services.    


##<a name="media-services-learning-paths"></a>Leerfasen Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
