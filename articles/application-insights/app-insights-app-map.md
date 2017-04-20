<properties 
    pageTitle="Kaart toepassing in inzichten toepassing | Microsoft Azure" 
    description="Een visuele presentatie van de afhankelijkheden tussen componenten van app, voorzien van KPI's en waarschuwingen." 
    services="application-insights" 
    documentationCenter=""
    authors="SoubhagyaDash" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="06/15/2016" 
    ms.author="awills"/>
 
# <a name="application-map-in-application-insights"></a>Kaart van de toepassing in inzichten van toepassing

Toewijzing van toepassing is in [Visual Studio-toepassing inzichten](app-insights-overview.md), een visuele indeling van de afhankelijkheidsrelaties van de toepassingsonderdelen van uw. Elk onderdeel toont KPI's zoals belasting, prestaties, fouten en waarschuwingen, kunt u elk onderdeel dat wordt veroorzaakt door een probleem met de prestaties, storing of ontdekken. Kunt u door middel van enig onderdeel van de voor meer diagnostische gegevens van de toepassing van inzichten, en, als uw app Azure services gebruikt - Azure diagnostische gegevens, zoals de aanbevelingen van de SQL-Database Advisor.

Net als bij andere diagrammen, kunt u een toepassing toewijzen aan de Azure dashboard, waar het is volledig functioneel vastmaken. 

## <a name="open-the-application-map"></a>Open de map toepassingen

Open de plattegrond van het blad Overzicht voor uw toepassing:

![app-kaart openen](./media/app-insights-app-map/01.png)

![App-overzicht](./media/app-insights-app-map/02.png)

De kaart wordt weergegeven:

* Beschikbaarheid van tests
* Component op de client (gecontroleerd met de SDK voor JavaScript)
* Server-side component
* Afhankelijkheden van de onderdelen client en server

U kunt groepen uitvouwen en samenvouwen afhankelijkheid koppelen:

![samenvouwen](./media/app-insights-app-map/03.png)
 
Als er een groot aantal afhankelijkheden van een bepaald type (SQL, HTTP enz.), kunnen deze gegroepeerd weergegeven. 


![gegroepeerde afhankelijkheden](./media/app-insights-app-map/03-2.png)
 
 
## <a name="spot-problems"></a>Problemen met steunkleuren

Elk knooppunt heeft relevante prestatie-indicatoren, zoals de belasting, prestaties en storing tarieven voor dat onderdeel. 

Waarschuwing pictogrammen markeren voor mogelijke problemen. Een oranje waarschuwing zijn er fouten in aanvragen, bezoekers of afhankelijkheid oproepen. Rood betekent een storing die hoger is dan 5%.


![Fout-pictogrammen](./media/app-insights-app-map/04.png)

 
Waarschuwingen van actieve ook weergeven: 


![actieve berichten](./media/app-insights-app-map/05.png)
 
Als u SQL Azure, er is een pictogram dat wordt weergegeven wanneer er zijn aanbevelingen voor de manier waarop u prestaties kunt verbeteren. 


![Azure aanbeveling](./media/app-insights-app-map/06.png)

Klik op een pictogram als u meer informatie:


![Azure aanbeveling](./media/app-insights-app-map/07.png)
 
 
## <a name="diagnostic-click-through"></a>Diagnostische klik door

Elk van de knooppunten op de kaart biedt gerichte klik door voor de diagnostische test. De opties variëren, afhankelijk van het type van het knooppunt.

![Serveropties](./media/app-insights-app-map/09.png)

 
De opties omvatten voor onderdelen die worden gehost in Azure, directe koppelingen naar deze.


## <a name="filters-and-time-range"></a>Filters en bereik

Standaard wordt de kaart samengevat alle gegevens die beschikbaar zijn voor de gekozen periode. Maar u kunt filteren om alleen de namen van specifieke bewerking of afhankelijkheden.

* Naam van bewerking: dit omvat zowel paginaweergaven en server side-aanvraagtypen. Met deze optie wordt ziet de kaart u de KPI op het knooppunt van de server-client side voor alleen de geselecteerde bewerkingen. De afhankelijkheden die zijn genoemd in het kader van deze specifieke bewerkingen worden weergegeven.
* Naam van de afhankelijkheid: dit omvat de afhankelijkheden op AJAX browser en afhankelijkheden op de server. Als u aangepaste afhankelijkheid telemetrie met de API TrackDependency, zal ook geven ze hier. Kunt u de afhankelijkheden wilt weergeven op de kaart. Houd er rekening mee dat op dit moment is dit niet met de server-side aanvragen of paginaweergaven op de client overeen.


![Filters instellen](./media/app-insights-app-map/11.png)

 
 
## <a name="save-filters"></a>Filters opslaan

Vastmaken om op te slaan in de filters die u hebt toegepast, de gefilterde weergave op een [dashboard](app-insights-dashboards.md).


![Vastzetten op dashboard](./media/app-insights-app-map/12.png)
 


## <a name="feedback"></a>Feedback

U kunt [feedback geven via de feedbackoptie portal](app-insights-get-dev-support.md).


![Afbeelding van de MapLink-1](./media/app-insights-app-map/13.png)


