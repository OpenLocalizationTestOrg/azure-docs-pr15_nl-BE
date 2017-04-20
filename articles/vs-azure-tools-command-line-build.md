<properties
   pageTitle="Opdrachtregelprogramma build voor Azure | Microsoft Azure"
   description="Opdrachtregelprogramma build voor Azure"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="command-line-build-for-azure"></a>Opdrachtregelprogramma Build voor Azure

## <a name="overview"></a>Overzicht

U kunt een pakket voor Azure-implementatie door MSBuild uit te voeren vanaf een opdrachtprompt. U kunt configureren en opbouw voor foutopsporing-, ontwikkel- en productiecomputers, naast het automatiseren van het bouwproces definiëren.


## <a name="microsoft-build-engine-msbuild"></a>Microsoft Build Engine (MSBuild)

Met behulp van de Microsoft Build-Engine (MSBuild), kunt u producten in build bouwen lab omgevingen waar Visual Studio niet is geïnstalleerd. MSBuild maakt gebruik van een XML-indeling voor project-bestanden extensible en volledig wordt ondersteund door Microsoft. In deze bestandsindeling kunt u aangeven welke artikelen moeten worden gemaakt voor een of meer platforms en configuraties.

U kunt MSBuild ook uitvoeren vanaf een opdrachtprompt en dit onderwerp wordt beschreven die benadering. Door het instellen van eigenschappen vanaf een opdrachtprompt, kunt u specifieke configuraties van een project maken. Zo ook kunt u de doelen die de opdracht MSBuild wordt opgebouwd. Zie [MSBuild opdrachtregel-naslaginformatie](https://msdn.microsoft.com/library/ms164311.aspx)voor meer informatie over opdrachtregelparameters en MSBuild.

## <a name="installation"></a>Installatie

Als u de volgende procedure wordt beschreven, moet u installeren software en hulpprogramma's op de server maken voordat u een Azure-pakket maken kunt met behulp van MSBuild:

1. Installeer .NET Framework 4 of hoger, waaronder MSBuild.

1. Installeer de [Azure Authoring Tools](http://go.microsoft.com/fwlink/?LinkId=394615) (zoek naar MicrosoftAzureAuthoringTools x64.msi of MicrosoftAzureAuthoringTools x86.msi.

1. Installeer de [Azure bibliotheken voor .NET](http://go.microsoft.com/fwlink/?LinkId=394616) (zoek naar MicrosoftAzureLibsForNet x64.msi of MicrosoftAzureLibs x86.msi.

1. Kopieer het bestand Microsoft.WebApplication.targets van een installatie van Visual Studio op een andere computer.

    Het bestand bevindt zich in de map C:\Program Files (x86) \MSBuild\Microsoft\Visual Studio\v12.0\WebApplications (v11.0 voor Visual Studio 2012) en moet u deze kopiëren naar dezelfde map op de server maken.

1. Installeer de [Azure Tools voor Visual Studio](http://go.microsoft.com/fwlink/?LinkId=394616).

    Zoek naar WindowsAzureTools.vs120.exe Visual Studio 2013 projecten bouwen.

## <a name="msbuild-parameters"></a>MSBuild-Parameters

De eenvoudigste manier om een pakket te maken is voor het uitvoeren van MSBuild met de `/t:Publish` optie. Met deze opdracht maakt standaard een map in de hoofdmap van het project, zoals ProjectDir\bin\Configuration\app.publish\. bij het samenstellen van een project Azure u twee bestanden, het pakketbestand zelf en de bijbehorende configuratiebestand genereren:

- Project.cspkg

- ServiceConfiguration.TargetProfile.cscfg

Standaard, elk project Azure, bevat een service-configuratiebestand voor lokale (foutopsporing) is gebaseerd en een andere voor de opbouw van de wolk (gefaseerde installatie of productie), maar u kunt toevoegen of verwijderen van service-configuratiebestanden wanneer dat nodig is. Wanneer u een pakket binnen Visual Studio, wordt u gevraagd welke service configuratiebestand naast het pakket opnemen. Wanneer u een pakket maken met behulp van MSBuild, is het lokale bestand met configuratie-service standaard opgenomen. Instellen als u wilt opnemen in een ander bestand voor de configuratie van de service, de `TargetProfile` eigenschap van de opdracht MSBuild (`MSBuild /t:Publish /p:TargetProfile=ProfileName`).

Als u wilt een alternatieve map voor opgeslagen pakket en configuratiebestanden gebruiken het pad instellen met behulp van de `/p:PublishDir=Directory\` optie, met inbegrip van de afsluitende backslash scheidingsteken.

## <a name="deployment"></a>Implementatie

Nadat het pakket is opgebouwd, kunt u deze kunt implementeren in Azure. Zie voor een zelfstudie waarin wordt gedemonstreerd dat proces, de Azure website. Zie voor informatie over hoe dat proces automatiseren, [Continue levering voor Cloud Services in Azure](./cloud-services/cloud-services-dotnet-continuous-delivery.md).
