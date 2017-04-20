<properties
    pageTitle="Maak een aangepast dashboard in logboek Analytics | Microsoft Azure"
    description="Deze handleiding helpt u bij het begrijpen hoe Log Analytics Dashboards kunnen visualiseren al uw zoekopdrachten opgeslagen logboek waardoor u met een enkele lens om uw omgeving weer te geven."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="banders"/>

# <a name="create-a-custom-dashboard-in-log-analytics"></a>Maak een aangepast dashboard in logboek Analytics

Deze handleiding helpt u bij het begrijpen hoe Log Analytics dashboards visualiseren al uw zoekopdrachten opgeslagen logboek waardoor u met een enkele lens om uw omgeving weer te geven.

![Voorbeeld van Dashboard](./media/log-analytics-dashboards/oms-dashboards-example-dash.png)

De aangepaste dashboards die u in de OMS portal maakt zijn ook beschikbaar in de OMS Mobile App. Zie de volgende pagina's voor meer informatie over de apps.

- [OMS mobiele app van de Microsoft Store](http://www.windowsphone.com/store/app/operational-insights/4823b935-83ce-466c-82bb-bd0a3f58d865)
- [Mobiele app van Apple iTunes OMS](https://itunes.apple.com/app/microsoft-operations-management/id1042424859?mt=8)

![mobiele dashboard](./media/log-analytics-dashboards/oms-search-mobile.png)

## <a name="how-do-i-create-my-dashboard"></a>Hoe maak ik mijn dashboard

Ga naar de pagina OMS-overzicht. Aan de linkerkant ziet u de tegel **Mijn Dashboard** . Klik op het inzoomen op uw dashboard.

![Overzicht](./media/log-analytics-dashboards/oms-dashboards-overview.png)


## <a name="adding-a-tile"></a>Een tegel toe te voegen

Tegels zijn in dashboards aangedreven door je zoekopdrachten opgeslagen logboek. OMS wordt geleverd met veel en-klare logboek opgeslagen zoekopdrachten, zodat u meteen kunt beginnen. Gebruik de volgende stappen waarin wordt uiteengezet hoe u kunt beginnen.

In het Dashboard van Mijn weergave, klikt u **aanpassen** in te voeren-modus aanpassen.

![Afbeeldingen](./media/log-analytics-dashboards/oms-dashboards-pictorial01.png)

 Het deelvenster dat wordt geopend aan de rechterkant van de pagina bevat alle van uw werkruimte opgeslagen logboek zoekopdrachten. Plaats de muisaanwijzer op een opgeslagen zoekactie om een zoekactie opgeslagen logboek als een steen, en klik op het **plus** -symbool.

![Tegels 1 toevoegen](./media/log-analytics-dashboards/oms-dashboards-pictorial02.png)

Wanneer u op het **plus** -symbool klikt, verschijnt er een nieuwe tegel in de weergave Mijn Dashboard.

![2 tegels toevoegen](./media/log-analytics-dashboards/oms-dashboards-pictorial03.png)


## <a name="edit-a-tile"></a>Een steen bewerken

In het Dashboard van Mijn weergave, klikt u **aanpassen** in te voeren-modus aanpassen. Klik op de tegel die u wilt bewerken. Het rechterpaneel wijzigingen te bewerken, en biedt een aantal opties:

![Steen bewerken](./media/log-analytics-dashboards/oms-dashboards-pictorial04.png)

![Steen bewerken](./media/log-analytics-dashboards/oms-dashboards-pictorial05.png)

### <a name="tile-visualizations"></a>Visualisaties naast elkaar#
Er zijn drie soorten visualisaties naast elkaar om uit te kiezen:

|grafiektype|wat het doet|
|---|---|
|![Staafdiagram](./media/log-analytics-dashboards/oms-dashboards-bar-chart.png)|Geeft een tijdlijn van de resultaten van uw zoekactie opgeslagen logboek als een staafdiagram of een lijst met resultaten van een veld afhankelijk van als het logboek zoeken resultaten door een veld worden samengevoegd of niet.
|![metric](./media/log-analytics-dashboards/oms-dashboards-metric.png)|Uw totale logboek zoeken resultaat treffers weergegeven als een getal in een tegel. Metrische tegels kunnen u een drempel die de tegel wordt gemarkeerd, wanneer de drempel is bereikt.|
|![regel](./media/log-analytics-dashboards/oms-dashboards-line.png)|Geeft een tijdlijn van de treffers opgeslagen logboek zoeken resultaat met waarden in als een lijndiagram.|

### <a name="threshold"></a>Drempel
U kunt een drempelwaarde op een tegel met de visualisatie van de metrische maken. Selecteer op een drempelwaarde te maken op de tegel. Kies of u de tegel markeren als de waarde boven of onder de gekozen drempel is en stel vervolgens de volgende drempelwaarde.

## <a name="organizing-the-dashboard"></a>Het dashboard ordenen
Als u wilt uw dashboard ordenen, gaat u naar de weergave Mijn Dashboard en klik op aanpassen **aanpassen** om op te geven-modus. Klik en sleep de tegel die u wilt verplaatsen en verplaatsen naar waar u uw tegel te zijn.

![Het Dashboard ordenen](./media/log-analytics-dashboards/oms-dashboards-organize.png)

## <a name="remove-a-tile"></a>Een tegel verwijderen
Verwijderen van een steen, gaat u naar de weergave Mijn Dashboard en klik op aanpassen **aanpassen** om op te geven-modus. Selecteer de tegel die u wilt verwijderen en vervolgens op het juiste paneel **Verwijderen naast elkaar**.

![Een tegel verwijderen](./media/log-analytics-dashboards/oms-dashboards-remove-tile.png)

## <a name="next-steps"></a>Volgende stappen

- [Waarschuwingen](log-analytics-alerts.md) maken in logboek Analytics om meldingen te genereren en om problemen te verhelpen.
