<properties
    pageTitle="Aantekeningen voor inzichten toepassing vrij | Microsoft Azure"
    description="Implementatie toevoegen of samenstellen markeringen metrics explorer grafieken in inzichten van toepassing."
    services="application-insights"
    documentationCenter=".net"
    authors="alancameronwills"
    manager="douge"/>

<tags
    ms.service="application-insights"
    ms.workload="tbd"
    ms.tgt_pltfrm="ibiza"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/28/2016"
    ms.author="awills"/>

# <a name="release-annotations-in-application-insights"></a>Release-aantekeningen in inzichten van toepassing

Laat de aantekeningen op [Metrics Explorer](app-insights-metrics-explorer.md) grafieken weergeven waarin u een nieuwe build geïmplementeerd. Ze maken het gemakkelijk om te zien of uw wijzigingen geen invloed op de prestaties van de toepassing hadden. Automatisch worden gemaakt door de [Services van Visual Studio Team system bouwen](https://www.visualstudio.com/en-us/get-started/build/build-your-app-vs)en u kunt ook [maakt ze vanuit PowerShell](#create-annotations-from-powershell).

![Voorbeeld van de aantekeningen met zichtbare correlatie met responstijd van de server](./media/app-insights-annotations/00.png)

Release-aantekeningen zijn een functie van de cloud-gebaseerde build en versie service van Visual Studio Team Services. 

## <a name="install-the-annotations-extension-one-time"></a>Installeer de extensie aantekeningen (eenmalig)

Om te kunnen vrijgeven aantekeningen te maken, moet u een van de vele Team extensies beschikbaar in de Visual Studio-markt te installeren.

1. Log in om uw project van [Visual Studio Team Services](https://www.visualstudio.com/en-us/get-started/setup/sign-up-for-visual-studio-online) .
2. In Visual Studio Marketplace, [krijgt de extensie Release-aantekeningen](https://marketplace.visualstudio.com/items/ms-appinsights.appinsightsreleaseannotations), en toe te voegen aan uw account Team Services.

![AT top rechts van Team Services-webpagina, open markt. Selecteer Visual Team Services en onder de Build en versie Kies meer.](./media/app-insights-annotations/10.png)

U hoeft hiervoor eenmaal voor de Visual Studio Team Services-account. Release-aantekeningen kunnen nu worden geconfigureerd voor een project in uw account. 

## <a name="get-an-api-key-from-application-insights"></a>U een API-sleutel van de inzichten van toepassing

U moet dit doen voor elke release sjabloon die u wilt maken van de release-aantekeningen.


1. Aanmelden bij de [Microsoft Azure Portal](https://portal.azure.com) en opent u de resource van toepassing inzichten die uw toepassing controleert. (Of [er nu een maken](app-insights-overview.md), als u dat nog niet hebt gedaan).
2. Open **API-toegang**en een kopie van de **Inzichten-toepassings-Id**.

    ![In portal.azure.com, opent u de resource toepassing inzichten en kies instellingen. Open API-toegang. Kopieer de toepassings-ID](./media/app-insights-annotations/20.png)

2. In een apart browservenster openen of maken de release-sjabloon die u de distributie van Visual Studio Team Services beheert. 

    Een taak toevoegen en selecteert u de taak toepassing inzichten Release aantekening in het menu.

    Plak de **Toepassings-Id** die u hebt gekopieerd uit de blade API-toegang.

    ![In Visual Studio Team Services versie openen, selecteer een definitie release en kies bewerken. Klik op taak toevoegen en toepassing inzichten Release aantekening selecteren. Plak de inzichten toepassings-id.](./media/app-insights-annotations/30.png)

3. Het veld **APIKey** ingesteld op een variabele `$(ApiKey)`.

4. Terug in de blade-API toegang maakt een nieuwe API-sleutel en een kopie van deze.

    ![Klik in de blade-API-toegang in het venster Azure, API-sleutel maken. Een commentaar, schrijven, aantekeningen, en klik op sleutel genereren. Kopieer de nieuwe sleutel.](./media/app-insights-annotations/40.png)

4. Open het tabblad configuratie van de sjabloon release.

    Maak een variabele-definitie voor `ApiKey`.

    De API-sleutel tot de variabele definitie ApiKey plakken.

    ![Klik op het tabblad configuratie en klik op variabele toevoegen in het venster Services Team. Stel name in op ApiKey en in de waarde, plakt u de sleutel die u zojuist hebt gemaakt.](./media/app-insights-annotations/50.png)


5. De definitie van de release ten slotte **Opslaan** .

## <a name="create-annotations-from-powershell"></a>Aantekeningen maken op basis van PowerShell

U kunt ook aantekeningen maken van elk proces dat u wilt (zonder VS Team System). 

De [Powershell-script van GitHub](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/API/CreateReleaseAnnotation.ps1)ophalen.

Deze als volgt gebruiken:

    .\CreateReleaseAnnotation.ps1 `
      -applicationId "<applicationId>" `
      -apiKey "<apiKey>" `
      -releaseName "<myReleaseName>" `
      -releaseProperties @{
          "ReleaseDescription"="a description";
          "TriggerBy"="My Name" }

Krijgen de `applicationId` en een `apiKey` van uw toepassing inzichten resource: instellingen openen, API-toegang en de id van de toepassing kopiëren Klik op API-sleutel maken en kopiëren van de sleutel. 

## <a name="release-annotations"></a>Release-aantekeningen

Nu wanneer u de release-sjabloon voor de implementatie van een nieuwe release, wordt een aantekening verzonden naar de inzichten van toepassing. De aantekeningen worden weergegeven in grafieken in cijfers Explorer.

Klik op een willekeurig markeerteken annotation details over de release, met inbegrip van de aanvrager, source control filiaal openen, laat de definitie en omgeving.


![Klik op een willekeurig markeerteken release-aantekeningen.](./media/app-insights-annotations/60.png)
