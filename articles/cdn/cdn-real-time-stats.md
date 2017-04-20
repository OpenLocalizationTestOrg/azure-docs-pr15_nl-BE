<properties
    pageTitle="Real-eenmalige-Stats in Azure CDN | Microsoft Azure"
    description="Real-Time statistieken biedt real-time gegevens over de prestaties van CDN Azure inhoud leveren aan uw klanten."
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="casoper"/>

# <a name="real-time-stats-in-microsoft-azure-cdn"></a>Real-time statistieken in Microsoft Azure CDN

[AZURE.INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a>Overzicht

Dit document wordt uitgelegd in Microsoft Azure CDN-real-time statistieken.  Deze functionaliteit biedt realtime-gegevens, zoals bandbreedte, status van de cache en gelijktijdige verbindingen met uw profiel CDN inhoud leveren aan uw klanten. Hierdoor kunnen de continue monitoring van de gezondheid van uw service op elk gewenst moment, met inbegrip van de go-live gebeurtenissen.

De volgende grafieken zijn beschikbaar:

* [Bandbreedte](#bandwidth)
* [Statuscodes](#status-codes)
* [Cache status](#cache-statuses)
* [Verbindingen](#connections)


## <a name="accessing-real-time-stats"></a>Toegang tot real-time statistieken

1. Ga naar uw profiel CDN in [Azure Portal](https://portal.azure.com).

    ![CDN profiel blade](./media/cdn-real-time-stats/cdn-profile-blade.png)

2. Klik op de knop **beheren** van de bladeserver CDN-profiel.

    ![Knop CDN profiel blade beheren](./media/cdn-real-time-stats/cdn-manage-btn.png)

    De portal CDN management wordt geopend.

3. Zweven boven de tab **Analytics** en vervolgens zweven over het doel van de **Real-Time statistieken** .  Klik op **http-groot Object**.

    ![CDN management portal](./media/cdn-real-time-stats/cdn-premium-portal.png)

    De statistieken real-time grafieken worden weergegeven.
    
Elk van de grafieken real-time geeft statistieken weer voor de geselecteerde tijdspanne vanaf het moment dat de pagina wordt geladen.  De grafieken automatisch bijgewerkt om de paar seconden.  De knop **Grafiek vernieuwen** , wordt indien aanwezig, de grafiek, waarna deze alleen de geselecteerde gegevens weergegeven gewist.

## <a name="bandwidth"></a>Bandbreedte

![Grafiek van bandbreedte](./media/cdn-real-time-stats/cdn-bandwidth.png)

De **bandbreedte** -diagram toont de hoeveelheid bandbreedte die wordt gebruikt voor het huidige platform via de geselecteerde tijdspanne. Het grijze gedeelte van de grafiek geeft aan bandbreedte. De exacte hoeveelheid bandbreedte die wordt gebruikt, wordt direct onder de lijngrafiek weergegeven.

## <a name="status-codes"></a>Statuscodes

![De status code-grafiek](./media/cdn-real-time-stats/cdn-status-codes.png)

De **Statuscodes** grafiek geeft aan hoe vaak bepaalde HTTP responscodes optreden via de geselecteerde tijdspanne.

> [AZURE.TIP]  Zie voor een beschrijving van elke optie HTTP status code [Azure CDN HTTP-statuscodes](https://msdn.microsoft.com/library/mt759238.aspx).

Een lijst met HTTP-statuscodes direct boven de grafiek weergegeven. Deze lijst geeft de statuscode die kan worden opgenomen in de lijngrafiek en het aantal exemplaren per seconde voor die statuscode. Standaard wordt een regel weergegeven voor elk van deze statuscodes in de grafiek. U kunt echter alleen controleren de statuscodes die een speciale betekenis voor de configuratie van uw CDN hebben. Hiertoe controleert u de gewenste statuscodes en schakel alle andere opties en klik vervolgens op **Grafiek vernieuwen**. 

Geregistreerde gegevens voor een bepaalde status, kunt u tijdelijk verbergen.  Van de legenda onder de grafiek, klikt u op de statuscode die u wilt verbergen. De statuscode worden direct verborgen in de grafiek. Die statuscode opnieuw te klikken wordt die optie moet opnieuw worden weergegeven.

## <a name="cache-statuses"></a>Cache status

![Cache status grafiek](./media/cdn-real-time-stats/cdn-cache-status.png)

De **Status van de Cache** grafiek geeft aan hoe vaak bepaalde soorten status van de cache op de geselecteerde tijdspanne optreden. 

> [AZURE.TIP]  Zie voor een beschrijving van elke optie cache status code [Azure CDN Cache Status Codes](https://msdn.microsoft.com/library/mt759237.aspx).

Een lijst met cache statuscodes wordt weergegeven direct boven de grafiek. Deze lijst geeft de statuscode die kan worden opgenomen in de lijngrafiek en het aantal exemplaren per seconde voor die statuscode. Standaard wordt een regel weergegeven voor elk van deze statuscodes in de grafiek. U kunt echter alleen controleren de statuscodes die een speciale betekenis voor de configuratie van uw CDN hebben. Hiertoe controleert u de gewenste statuscodes en schakel alle andere opties en klik vervolgens op **Grafiek vernieuwen**. 

Geregistreerde gegevens voor een bepaalde status, kunt u tijdelijk verbergen.  Van de legenda onder de grafiek, klikt u op de statuscode die u wilt verbergen. De statuscode worden direct verborgen in de grafiek. Die statuscode opnieuw te klikken wordt die optie moet opnieuw worden weergegeven.

## <a name="connections"></a>Verbindingen

![Grafiek van verbindingen](./media/cdn-real-time-stats/cdn-connections.png)

Deze grafiek geeft aan hoeveel verbindingen zijn vastgesteld om de servers aan de periferie. Elke aanvraag voor een actief dat wordt doorgegeven via onze CDN resultaten in een verbinding.

## <a name="next-steps"></a>Volgende stappen

- Ontvang een bericht met [Real-time waarschuwingen in Azure CDN](cdn-real-time-alerts.md)
- De afdruk met [Geavanceerde HTTP-rapporten](cdn-advanced-http-reports.md)
- Analyseren van [gebruikspatronen](cdn-analyze-usage-patterns.md)

