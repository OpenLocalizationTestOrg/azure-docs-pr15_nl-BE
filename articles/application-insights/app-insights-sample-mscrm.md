<properties 
    pageTitle="Procedure: Microsoft Dynamics CRM met inzichten van de toepassing controleren" 
    description="Telemetrie u vanuit Microsoft Dynamics CRM Online met behulp van inzichten van toepassing. Overzicht van setup, het ophalen van gegevens, visualisatie en export." 
    services="application-insights" 
    documentationCenter=""
    authors="mazharmicrosoft" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="11/17/2015" 
    ms.author="awills"/>
 
# <a name="walkthrough-enabling-telemetry-for-microsoft-dynamics-crm-online-using-application-insights"></a>Procedure: Telemetrie inschakelen voor Microsoft Dynamics CRM Online met behulp van inzichten van toepassing

In dit artikel wordt beschreven hoe u met telemetriegegevens ophalen uit [Microsoft Dynamics CRM Online](https://www.dynamics.com/) met behulp van [Visual Studio-toepassing inzichten](https://azure.microsoft.com/services/application-insights/). We zien door het volledige proces van toepassing inzichten script toe te voegen aan uw toepassing, het vastleggen van gegevens en weergave van gegevens.

>[AZURE.NOTE] De [Bladeren van de monsteroplossing](https://dynamicsandappinsights.codeplex.com/).

## <a name="add-application-insights-to-new-or-existing-crm-online-instance"></a>Inzichten toepassing toevoegen aan nieuwe of bestaande CRM Online exemplaar 

Voor het controleren van uw toepassing, kunt u een toepassing inzichten SDK toevoegen aan uw toepassing. Telemetrie verzendt de SDK naar de [portal Application inzichten](https://portal.azure.com), kunt u onze krachtige analyse en diagnostische hulpprogramma's, of de gegevens exporteren naar opslag.

### <a name="create-an-application-insights-resource-in-azure"></a>Maakt u een bron toepassing inzichten in Azure

1. [Een account in Microsoft Azure](http://azure.com/pricing)aanvragen. 
2. Inloggen op de [portal Azure](https://portal.azure.com) en een nieuwe toepassing inzichten resource toevoegen. Dit is waar uw gegevens worden verwerkt en weergegeven.

    ![Klik op +, Services Developer, Application inzichten.](./media/app-insights-sample-mscrm/01.png)

    ASP.NET te kiezen als het toepassingstype.

3. Open het tabblad Quick Start en het script code.

    ![](./media/app-insights-sample-mscrm/03.png)

**De pagina open te houden** terwijl u de volgende stap in een ander browservenster. U moet de code snel. 

### <a name="create-a-javascript-web-resource-in-microsoft-dynamics-crm"></a>Een JavaScript web-bron maakt in Microsoft Dynamics CRM

1. Open uw exemplaar van CRM Online en u aanmelden met beheerdersbevoegdheden.
2. Open Microsoft Dynamics CRM-instellingen, aanpassingen, aanpassen het systeem

    ![](./media/app-insights-sample-mscrm/04.png)
    
    ![](./media/app-insights-sample-mscrm/05.png)


    ![](./media/app-insights-sample-mscrm/06.png)

3. Een JavaScript-bron maken.

    ![](./media/app-insights-sample-mscrm/07.png)

    Een naam geven en selecteer **Script (JScript)** de teksteditor openen.

    ![](./media/app-insights-sample-mscrm/08.png)
    
4. Kopieer de code van de inzichten van toepassing. Tijdens het kopiëren van zorg script-tags wordt genegeerd. Zie hieronder screenshot:

    ![](./media/app-insights-sample-mscrm/09.png)

    De code bevat de sleutel instrumenten waarmee de toepassingsbron inzichten.

5. Opslaan en publiceren.

    ![](./media/app-insights-sample-mscrm/10.png)

### <a name="instrument-forms"></a>Instrument formulieren

1. Open het formulier Account in Microsoft CRM Online.

    ![](./media/app-insights-sample-mscrm/11.png)

2. Open de eigenschappen van het formulier

    ![](./media/app-insights-sample-mscrm/12.png)

3. De JavaScript webresource die u hebt gemaakt toevoegen

    ![](./media/app-insights-sample-mscrm/13.png)

    ![](./media/app-insights-sample-mscrm/14.png)

4. Opslaan en publiceren van uw aanpassingen.


## <a name="metrics-captured"></a>Metrische gegevens vastgelegd

U hebt nu ingesteld telemetrie vastleggen voor het formulier. Wanneer het wordt gebruikt, worden gegevens verzonden naar de bron van de inzichten van toepassing.

Hier vindt u voorbeelden van de gegevens die u ziet.

#### <a name="application-health"></a>Gezondheid toepassing

![](./media/app-insights-sample-mscrm/15.png)

![](./media/app-insights-sample-mscrm/16.png)

Browser uitzonderingen:

![](./media/app-insights-sample-mscrm/17.png)

Klik op de grafiek als u meer informatie:

![](./media/app-insights-sample-mscrm/18.png)

#### <a name="usage"></a>Gebruik

![](./media/app-insights-sample-mscrm/19.png)

![](./media/app-insights-sample-mscrm/20.png)

![](./media/app-insights-sample-mscrm/21.png)

#### <a name="browsers"></a>Browsers

![](./media/app-insights-sample-mscrm/22.png)

![](./media/app-insights-sample-mscrm/23.png)

#### <a name="geolocation"></a>Geolocation

![](./media/app-insights-sample-mscrm/24.png)

![](./media/app-insights-sample-mscrm/25.png)

#### <a name="inside-page-view-request"></a>Aanvraag voor Inside pagina weergeven

![](./media/app-insights-sample-mscrm/26.png)

![](./media/app-insights-sample-mscrm/27.png)

![](./media/app-insights-sample-mscrm/28.png)

![](./media/app-insights-sample-mscrm/29.png)

![](./media/app-insights-sample-mscrm/30.png)

## <a name="sample-code"></a>Voorbeeld van code

[De voorbeeldcode bladeren](https://dynamicsandappinsights.codeplex.com/).

## <a name="power-bi"></a>Power BI

U kunt zelfs grondigere analyse doen als u [de gegevens exporteren naar Microsoft Power BI](app-insights-export-power-bi.md).

## <a name="sample-microsoft-dynamics-crm-solution"></a>Microsoft Dynamics CRM oplossing

[Hier is de oplossing geïmplementeerd in Microsoft Dynamics CRM] (https://dynamicsandappinsights.codeplex.com/).

## <a name="learn-more"></a>Meer informatie

* [Wat is de inzichten van toepassing?](app-insights-overview.md)
* [Toepassing inzichten voor webpagina 's](app-insights-javascript.md)
* [Meer voorbeelden en zelfstudies](app-insights-code-samples.md)

 
