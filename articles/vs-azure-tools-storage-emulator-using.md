<properties 
   pageTitle="Configureren en gebruiken van de Emulator opslag met Visual Studio | Microsoft Azure"
   description="Configureren en gebruiken van de Emulator opslag met Visual Studio"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags 
   ms.service="storage"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/18/2016"
   ms.author="tarcher" />

# <a name="configuring-and-using-the-storage-emulator-with-visual-studio"></a>Configureren en gebruiken van de Emulator opslag met Visual Studio

[AZURE.INCLUDE [storage-try-azure-tools](../includes/storage-try-azure-tools.md)]

## <a name="overview"></a>Overzicht
De ontwikkelomgeving Azure SDK bevat de emulator opslag, een hulpprogramma dat de Blob, wachtrij en tabel storage-services op de computer van de plaatselijke ontwikkeling in Azure beschikbaar simuleert. Als u de Azure storage-services bouwen van een cloud-service die in dienst zijn of elke externe toepassing die de opslagservices roept schrijft, kunt u uw code te vergelijken met de emulator opslag lokaal testen. Beheer van de opslag-emulator integreren de Azure-hulpprogramma's voor Microsoft Visual Studio in Visual Studio. De hulpprogramma's voor Azure initialiseren van de database van de emulator opslag bij eerste gebruik de emulator storage-service wordt gestart wanneer u uitvoert of de foutopsporing van Visual Studio en biedt alleen-lezen toegang tot de gegevens in de emulator opslag via de Opslagverkenner Azure.

Zie voor gedetailleerde informatie over de emulator opslag, met inbegrip van de systeemvereisten en de aangepaste configuratie-instructies [gebruiken de Emulator Azure opslag voor ontwikkeling en testen](./storage/storage-use-emulator.md).

>[AZURE.NOTE] Er zijn enkele verschillen in functionaliteit tussen de emulator opslag simulatie en de Azure storage-services. Zie [verschillen tussen de opslag Emulator en opslagservices Azure](./storage/storage-use-emulator.md) in de Azure SDK-documentatie voor meer informatie over de specifieke verschillen.

## <a name="configuring-a-connection-string-for-the-storage-emulator"></a>Een verbindingsreeks configureren voor de opslag-emulatie

U wilt een verbindingsreeks die verwijst naar de opslag-emulator en die later kunnen worden gewijzigd om te verwijzen naar een account Azure opslag configureren voor toegang tot de opslag-emulator van de code binnen een rol. Een verbindingsreeks is een configuratie-instelling die uw rol kan worden gelezen tijdens runtime voor verbinding met een account voor opslag. Zie [de Azure-toepassingen configureren](https://msdn.microsoft.com/library/azure/2da5d6ce-f74d-45a9-bf6b-b3a60c5ef74e#BK_SettingsPage)voor meer informatie over het maken van verbindingsreeksen.

>[AZURE.NOTE] U kunt een verwijzing naar de rekening van de emulator opslag vanuit uw code met behulp van de eigenschap **DevelopmentStorageAccount** terug. Deze benadering werkt correct als u toegang wilt tot de emulator opslag vanuit uw code, maar als u van plan bent om uw toepassing uitgeeft aan Azure, u moet voor het maken van een verbindingsreeks om toegang tot uw account Azure opslag en wijzig de code voor het gebruik van deze verbindingsreeks voordat u deze publiceert. Als u tussen de emulator opslag account en een account Azure opslag vaak overschakelt, wordt een verbindingsreeks vereenvoudigen van dit proces.

## <a name="initializing-and-running-the-storage-emulator"></a>Bezig met initialiseren en het uitvoeren van de emulator opslag

U kunt opgeven dat tijdens het uitvoeren of fouten opsporen in uw service in Visual Studio, Visual Studio automatisch de emulator opslag start. Open het snelmenu voor het project **Azure** in Solution Explorer en kies **Eigenschappen**. Klik op het tabblad **ontwikkeling** in de lijst **Start Azure opslag Emulator** Kies **True** (als deze nog niet is ingesteld op die waarde).

De eerste keer dat u uitvoert of fouten opsporen in uw service vanuit Visual Studio in de emulator opslag wordt een initialisatieproces gestart. Dit proces lokale poorten gereserveerd voor de emulator van de opslag en de opslag-emulator database wordt gemaakt. Als het eenmaal is voltooid, hoeft dit proces niet opnieuw uit te voeren tenzij de opslag emulator database wordt verwijderd.

>[AZURE.NOTE] Vanaf juni 2012 Azure hulpmiddelen, de opslag-emulator wordt uitgevoerd, standaard in SQL Express-LocalDB. In eerdere versies van de hulpprogramma's voor Azure, de opslag-emulator wordt uitgevoerd op een standaardexemplaar van SQL Express 2005 of 2008, die u moet installeren voordat u de SDK Azure kunnen installeren. U kunt ook de emulator opslag tegen een benoemd exemplaar van SQL Express of een naam of een standaardinstantie van Microsoft SQL Server uitvoeren. Als u de emulator opslag uit te voeren tegen een andere instantie dan de standaardinstantie configureren, Zie [Gebruik de Emulator Azure opslag voor ontwikkeling en testen](./storage/storage-use-emulator.md).

De emulator opslag biedt een gebruikersinterface voor het weergeven van de status van de diensten van de lokale opslag en starten, stoppen en opnieuw ingesteld. Zodra de emulator storage-service is gestart, kan de gebruikersinterface weergeven of start of stopt u de service met de rechtermuisknop op het pictogram voor de Microsoft Azure-Emulator in de taakbalk van Windows.

## <a name="viewing-storage-emulator-data-in-server-explorer"></a>Emulator-opslaggegevens bekijken in Server Explorer

Het knooppunt Azure opslag in Server Explorer kunt u gegevens weergeven en wijzigen van instellingen voor blob en tabel gegevens in uw opslag rekeningen, met inbegrip van de opslag-emulator. [Surfen en opslagbronnen beheren met Server Explorer](https://msdn.microsoft.com/library/azure/ff683677.aspx) Zie voor meer informatie.
