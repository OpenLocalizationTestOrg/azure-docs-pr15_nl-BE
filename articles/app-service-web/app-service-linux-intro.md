<properties 
    pageTitle="Inleiding tot Service App op Linux | Microsoft Azure" 
    description="Informatie over Service App op Linux." 
    keywords="Azure app service, linux, oss"
    services="app-service" 
    documentationCenter="" 
    authors="naziml" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/10/2016" 
    ms.author="naziml"/>

# <a name="introduction-to-app-service-on-linux"></a>Inleiding tot Service App op Linux
App Service op Linux is nu in Public Preview en actieve web apps op Linux native ondersteunt. 

## <a name="overview"></a>Overzicht ##
Klanten kunt App Service op Linux host web apps op Linux native naar ondersteunde toepassing stacks. De volgende functies sectie worden de ondersteunde toepassing stacks.

## <a name="features"></a>Functies ##
De App Service op Linux ondersteunt momenteel de volgende toepassing stacks

- Node.js
- PHP

Klanten kunnen hun toepassingen met behulp van implementeren

- FTP.
- Lokale Git.
- GitHub of BitBucket.

Voor het schalen van toepassingen


- Klanten kunnen hun web app omhoog en omlaag schalen door het wijzigen van de laag in hun App Service Plan. 
- Klanten kunnen schaal van hun toepassingen uit en hun toepassing uitvoeren via meerdere exemplaren binnen de grenzen van de SKU.

Voor de Kudu werken sommige van de basisfunctionaliteit

- Omgeving.
- Implementaties.
- Basic-console.

## <a name="limitations"></a>Beperkingen ##

De Azure management portal worden alleen ondersteunde functies weergeven voor de App Service op Linux en de rest verbergen. Als ons team van meer functies waardoor we zal houden als gevolg van dit op de portal beheren. Sommige functies, zoals VNET, integratie en AAD / indirecte verificatie of Kudu site uitbreidingen die momenteel werken niet. Maar als we deze werken wij onze documentatie en blog over wijzigingen worden bijgewerkt.

Deze openbare preview is momenteel alleen beschikbaar in de volgende regio 's

-   West VS.
-   West-Europa.
-   Zuidoost-Azië.

Web app op Linux wordt alleen ondersteund in de speciale App plannen en hoeft niet een vrije of gedeelde laag. App serviceplannen voor normale en Linux web apps zijn ook sluiten elkaar uit, zodat u kunt een Linux web app in een niet-Linux app serviceplan maken.

Web app op Linux moet worden gemaakt in een bronnengroep die geen niet-Linux web apps in dezelfde regio bevatten.

Door het ontbreken van overlappende recycling van het web apps, verwachten klanten dat een kleine uitval in geval van een web app opnieuw hebt gestart. 

## <a name="next-steps"></a>Volgende stappen ##

Volg de onderstaande koppelingen om aan de slag met App Service op Linux. Boeken vragen en bekommernissen op [ons forum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazurewebsitespreview).

* [Het maken van Web Apps in App-Service op Linux](./app-service-linux-how-to-create-a-web-app.md)
* [Met de configuratie van de PM2 voor Node.js in Web Apps op Linux](./app-service-linux-using-nodejs-pm2.md)

