<properties
   pageTitle="Projecten upgraden naar de huidige versie van de hulpprogramma's voor Azure | Microsoft Azure"
   description="Informatie over het upgraden van een Azure-project in Visual Studio aan de huidige versie van de hulpprogramma's voor Azure"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="how-to-upgrade-projects-to-the-current-version-of-the-azure-tools-for-visual-studio"></a>Projecten upgraden naar de huidige versie van de Azure-hulpprogramma's voor Visual Studio

## <a name="overview"></a>Overzicht

Nadat u de huidige versie van de Azure (of een eerdere versie die nieuwer is dan 1,6) projecten die zijn gemaakt met behulp van een extra Azure voordat u 1.6 release (November 2011) worden automatisch bijgewerkt zodra u deze opent. Als u projecten hebt gemaakt met behulp van de 1,6 (November 2011) versie van deze hulpprogramma's en u nog steeds dat versie is geïnstalleerd, kunt u projecten openen in de oudere versie en later beslissen of u deze opwaarderen.

## <a name="how-your-project-changes-when-you-upgrade-it"></a>Hoe het project verandert wanneer u deze upgrade

Als een project automatisch wordt bijgewerkt of als u opgeeft dat u wilt upgraden, het project is gewijzigd om te werken met de huidige versies van een bepaalde assembly's en sommige eigenschappen ook gewijzigd zoals in deze sectie wordt beschreven. Als uw project compatibel met de nieuwere versie van de hulpprogramma's van andere wijzigingen zijn vereist, moet u deze wijzigingen handmatig aanbrengen.

- Het bestand web.config voor web-rollen en het bestand app.config voor werknemer rollen worden bijgewerkt als u verwijst naar de nieuwere versie van Microsoft.WindowsAzure.Diagnostics.DiagnosticMonitoirTraceListener.dll.

- De assembly's Microsoft.WindowsAzure.StorageClient.dll, Microsoft.WindowsAzure.Diagnostics.dll en Microsoft.WindowsAzure.ServiceRuntime.dll worden bijgewerkt naar de nieuwe versies.

- Publicatieprofielen die zijn opgeslagen in het projectbestand Azure (.ccproj) worden verplaatst naar een apart bestand met de extensie .azurePubXml, in de submap **publiceren** .

- Sommige eigenschappen in het publicatieprofiel worden ter ondersteuning van nieuwe en gewijzigde functies bijgewerkt. Omdat u een service voor gedistribueerde cloud, gelijktijdig of gefaseerd bijwerken kunt **AllowUpgrade** vervangen door **DeploymentReplacementMethod** .

- De eigenschap **UseIISExpressByDefault** wordt toegevoegd en wordt ingesteld op false zodat de webserver die wordt gebruikt voor het opsporen van fouten niet automatisch gewijzigd voor Internet Information Services (IIS) voor IIS. IIS Express is de standaardwebserver voor projecten die zijn gemaakt met de nieuwere versies van de hulpprogramma's.

- Als Azure Caching wordt gehost in een of meer van de functies van uw project, worden bepaalde eigenschappen in de configuratie van de service (.cscfg-bestand) en de definitie van service (.csdef-bestand) gewijzigd wanneer een project wordt bijgewerkt. Als het pakket Azure Caching NuGet gebruikt in het project, wordt het project bijgewerkt naar de meest recente versie van het pakket. U moet het bestand web.config te openen en controleren of de configuratie van de client correct is behouden tijdens de upgrade. Deze assembly's niet bijgewerkt als u de verwijzingen naar de client-assembly's Azure Caching toegevoegd zonder het pakket NuGet, u moet deze verwijzingen naar de nieuwe versies handmatig bijwerken.

>[AZURE.IMPORTANT] F # projecten, moet u verwijzingen naar Azure assembly's handmatig bijwerken zodat ze verwijzen naar de nieuwere versies van deze assembly's.

### <a name="how-to-upgrade-an-azure-project-to-the-current-release"></a>Een project Azure upgraden naar de huidige versie

1. De huidige versie van de Azure-hulpprogramma's installeren in de installatie van Visual Studio die u wilt gebruiken voor het bijgewerkte project en open het project dat u wilt bijwerken. Als het project is gemaakt met een extra Azure voordat u 1.6 release (November 2011), het project automatisch bijgewerkt naar de huidige versie. Als het project is gemaakt met de November 2011 vrijgeven en die release nog steeds geïnstalleerd is, het project wordt geopend in deze release.

1. Opent het snelmenu voor het projectknooppunt, kies **Eigenschappen**en klik op het tabblad **vereffening** in het dialoogvenster dat wordt weergegeven in Solution Explorer.

    Op het tabblad **toepassingen** wordt de extra-versie die is gekoppeld aan het project. Als de huidige versie van Azure's wordt weergegeven, is het project al bijgewerkt. Als u een nieuwere versie van de hulpprogramma's dan wat het tabblad ziet u hebt geïnstalleerd, verschijnt een knop **bijwerken** .

1. Klik op de knop **Upgrade** een project bijwerken naar de huidige versie van de hulpprogramma's.

1. Compileer het project en vervolgens los eventuele fouten die uit wijzigingen van de API voortvloeien. Raadpleeg de documentatie bij de specifieke API voor meer informatie over het wijzigen van de code voor de nieuwe versie.
