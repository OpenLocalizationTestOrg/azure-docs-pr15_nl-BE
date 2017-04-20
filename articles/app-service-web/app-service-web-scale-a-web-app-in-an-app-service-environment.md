<properties 
    pageTitle="Het schalen van een App in een App-omgeving" 
    description="Schaal van een app in een App-omgeving" 
    services="app-service" 
    documentationCenter="" 
    authors="ccompy" 
    manager="stefsch" 
    editor="jimbe"/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/17/2016" 
    ms.author="ccompy"/>

# <a name="scaling-apps-in-an-app-service-environment"></a>Apps schalen in een App-omgeving #

In de App Azure Service zijn er normaal gesproken drie dingen die u kunt schalen:

- prijzen plan
- grootte van de werknemer 
- aantal exemplaren.

In een ASE behoeft niet selecteren of wijzigen van de planning van de prijzen.  In termen van mogelijkheden is het al op een premie prijzenniveau mogelijkheden.  

ASE admin kunt met betrekking tot de grootte van de werknemer, de grootte van de compute resource moet worden gebruikt voor elke werknemer van toepassingen.  Dat betekent dat er Pool 1 werknemer met P4 berekenen bronnen en werknemer Pool 2 met P1 compute bronnen, indien gewenst.  Ze hoeven niet te zijn in volgorde van grootte.  Gedetailleerde informatie rond de formaten en hun prijzen Zie het document hier [Azure App serviceprijzen][AppServicePricing].  Dit laat de schaalopties voor web apps en App serviceplannen in een App-omgeving te zijn:

- werknemer van toepassingen selecteren
- aantal exemplaren

Door de juiste gebruikersinterface weergegeven voor de ASE serviceplannen voor App gehost wordt wijzigen van een item uitgevoerd.  

![][1]

U kan niet de ASP dan het aantal beschikbare compute resources in de groep werknemers die de ASP in schalen.  Als u de resources in die groep werknemer moet berekenen moet u krijgen de ASE-beheerder deze toe te voegen.  Voor informatie over het opnieuw configureren van uw ASE hier de informatie te lezen: [het configureren van een App-omgeving][HowtoConfigureASE].  U kunt ook profiteren van de ASE automatisch schalen functies toe te voegen capaciteit op basis van schema- of maatstaven.  Voor meer informatie over het configureren van automatisch schalen voor de ASE omgeving zichzelf [automatisch schalen voor een App-omgeving configureren]Zie[ASEAutoscale].

Kunt u meerdere app serviceplannen compute bronnen uit een andere werknemer van toepassingen gebruiken of kunt u dezelfde groep van werknemers.  Bijvoorbeeld als werknemer Pool 1 bevatten (10) berekenen van beschikbare resources, kunt u kiezen om één app serviceplan (6) compute-bronnen te maken en plannen voor een tweede app-service die wordt gebruikt (4) resources te berekenen.

### <a name="scaling-the-number-of-instances"></a>Het aantal exemplaren schalen ###

Wanneer u uw web app in een omgeving van App-Service maakt wordt gestart 1 exemplaar.  U kunt vervolgens schalen uit om extra exemplaren te voorzien in uw app compute aanvullende bronnen.   

Als uw ASE onvoldoende capaciteit heeft is dit vrij eenvoudig.  Gaat u naar uw App Service plannen die in het bezit van de sites die u wilt vergroten en selecteer schaal.  Hiermee opent u de gebruikersinterface kunt u handmatig de schaal instellen voor uw ASP of regels automatisch schalen voor de ASP configureren.  Handmatig schalen stelt uw app gewoon ***schaal door*** op ***een aantal exemplaar die ik handmatig invoeren***.  Sleep de schuifregelaar naar de gewenste hoeveelheid of invoeren in het vak naast de schuifregelaar vanaf hier.  

![][2] 

De regels automatisch schalen voor een ASP in een ASE werken hetzelfde als normaal.  U kunt ***CPU-Percentage*** onder ***schaal door te*** selecteren en automatisch schalen regels maken voor de ASP is gebaseerd op een Percentage van de CPU of kunt u complexere regels met de ***regels voor planning en prestaties***.  Zie meer details over het configureren van automatisch schalen gebruikt u de handleiding hier [een app in de App-Service Azure schaal][AppScale]. 


### <a name="worker-pool-selection"></a>Werknemer van toepassingen selecteren ###

Zoals eerder opgemerkt, wordt de selectie van de groep werknemer vanuit de ASP-gebruikersinterface.  Open het blad voor de ASP-code die u wilt aanpassen en selecteer toepassingen van de werknemer.  Ziet u alle van de werknemer van toepassingen die u hebt geconfigureerd in uw omgeving App-Service.  Als er slechts één werknemer groep vervolgens ziet u alleen de ene groep vermeld.  Als u toepassingen werknemer uw ASP is in, selecteert u gewoon de werknemer toepassingen die u wilt dat uw App-Service wilt verplaatsen naar.  

![][3]

Voordat u de ASP uit één werknemer groep verplaatst naar de andere is het belangrijk om ervoor te zorgen dat er voldoende capaciteit voor de ASP.  In de lijst met groepen van werknemers, niet alleen wordt de naam van de werknemer toepassingen weergegeven maar kunt u ook zien hoeveel medewerkers beschikbaar zijn in die groep werknemers.  Zorg ervoor dat er voldoende exemplaren bevatten uw App Service plannen beschikbaar zijn.  Als u meer resources in de groep van de werknemer die te verplaatsen naar de gewenste berekenen moet, krijgen vervolgens de ASE-beheerder deze toe te voegen.  

> [AZURE.NOTE] Een ASP uit groep één werknemer, koude zal verplaatsen begint van de apps in ASP.  Hierdoor kunnen aanvragen langzaam uw app koud is gestart op het nieuwe compute-bronnen.  Koude start kan worden vermeden via de [toepassing warm up mogelijkheid] [ AppWarmup] in Azure App-Service.  Initialisatie van de toepassing werkt de module beschreven in het artikel ook koude start omdat het initialisatieproces ook aangeroepen wordt als apps koud gestart op nieuwe compute resources zijn. 

## <a name="getting-started"></a>Aan de slag

Zie [Hoe te maken een App-omgeving] aan de slag met App Service-omgevingen.[HowtoCreateASE]

Zie voor meer informatie over het platform Azure App Service [Azure App Service][AzureAppService].

<!--Image references-->
[1]: ./media/app-service-web-scale-a-web-app-in-an-app-service-environment/aseappscale-aspblade.png
[2]: ./media/app-service-web-scale-a-web-app-in-an-app-service-environment/aseappscale-manualscale.png
[3]: ./media/app-service-web-scale-a-web-app-in-an-app-service-environment/aseappscale-sizescale.png

<!--Links-->
[WhatisASE]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[ScaleWebapp]: http://azure.microsoft.com/documentation/articles/web-sites-scale/
[HowtoCreateASE]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[HowtoConfigureASE]: http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment/
[CreateWebappinASE]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-a-web-app-in-an-ase/
[Appserviceplans]: http://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/ 
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[ASEAutoscale]: http://azure.microsoft.com/documentation/articles/app-service-environment-auto-scale/
[AppScale]: http://azure.microsoft.com/documentation/articles/web-sites-scale/
[AppWarmup]: http://ruslany.net/2015/09/how-to-warm-up-azure-web-app-during-deployment-slots-swap/
