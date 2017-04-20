<properties 
    pageTitle="Het maken van een Web App met Service App op Linux | Microsoft Azure" 
    description="Web app werkstroom maken voor de App Service op Linux." 
    keywords="service Azure app, web app, linux, oss"
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

# <a name="create-a-web-app-with-app-service-on-linux"></a>Maken van een Web App met Service App op Linux

## <a name="using-the-management-portal-to-create-your-web-app"></a>De Portal voor beheer gebruiken voor het maken van uw web app
U kunt beginnen met het maken van uw Web App op Linux vanaf de [portal management](https://portal.azure.com) zoals in de onderstaande afbeelding.

![][1]

Wanneer u hieronder de optie selecteert, worden weergegeven de blade maken zoals in de onderstaande afbeelding. 

![][2]

-   Uw web app een naam geven.
-   Kies een bestaande resourcegroep of een nieuwe maken. (Zie regio's beschikbaar in de [sectie beperkingen](./app-service-linux-intro.md)).
-   Kies een bestaande app serviceplan of maak een nieuwe een (Zie app service plan opmerkingen in de [sectie beperkingen](./app-service-linux-intro.md)). 
-   Kies de toepassing stack die u wilt gebruiken. U krijgt om te kiezen tussen verschillende versies van Node.js en PHP. 

Zodra u de app gemaakt hebt, kunt u de stack van de toepassing van de instellingen zoals in de onderstaande afbeelding.

![][3]

## <a name="deploying-your-web-app"></a>Implementatie van uw web app

' Implementatieopties' kiezen in de portal beheren kunt u lokaal een Git repository of een opslagplaats GitHub gebruiken voor de implementatie van uw toepassing. De instructies daarna zijn op dezelfde manier aan een niet-Linux web app en u kunt de instructies in de [lokale Git distributie](./app-service-deploy-local-git.md) of onze [continue distributie](./app-service-continuous-deployment.md) artikel voor GitHub.

U kunt FTP ook gebruiken voor het uploaden van uw toepassing naar uw site. Uit de diagnostische logboeken sectie zoals in de onderstaande afbeelding kunt u het eindpunt van de FTP voor uw web app krijgen.

![][4]


## <a name="next-steps"></a>Volgende stappen ##

* [Wat is de App Service op Linux?](./app-service-linux-intro.md)
* [Met de configuratie van de PM2 voor Node.js in Web Apps op Linux](./app-service-linux-using-nodejs-pm2.md)

<!--Image references-->
[1]: ./media/app-service-linux-how-to-create-a-web-app/top-level-create.png
[2]: ./media/app-service-linux-how-to-create-a-web-app/create-blade.png
[3]: ./media/app-service-linux-how-to-create-a-web-app/application-settings-change-stack.png
[4]: ./media/app-service-linux-how-to-create-a-web-app/diagnostic-logs-ftp.png
