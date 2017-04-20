<properties 
    pageTitle="Het gebruik van io.js met Azure App Service Web Apps" 
    description="Informatie over het gebruik van een web app in Azure App Service met io.js." 
    services="app-service\web" 
    documentationCenter="nodejs" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="08/11/2016"
    ms.author="robmcm" />

# <a name="how-to-use-iojs-with-azure-app-service-web-apps"></a>Het gebruik van io.js met Azure App Service Web Apps

De populaire knooppunt fork [io.js] biedt diverse verschillen van Joyent Node.js project, met inbegrip van een meer open governance-model, een snellere release-cyclus en een snellere invoering van nieuwe en experimentele JavaScript-functies.

Terwijl [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) Web Apps veel Node.js versies is geïnstalleerd heeft, kunnen voor een gebruiker opgegeven Node.js binair bestand. Dit artikel bevat twee methoden om het gebruik van io.js op het Web Apps van App-Service: het gebruik van een uitgebreide implementatiescript Azure voor het gebruik van de nieuwste versie van io.js beschikbaar, evenals het handmatig uploaden van een binair io.js automatisch te configureren. 

<a id="deploymentscript"></a>
## <a name="using-a-deployment-script"></a>Met behulp van een Script voor implementatie

Bij de implementatie van een app Node.js uitgevoerd App Service Web Apps een aantal kleine opdrachten om ervoor te zorgen dat het milieu goed is geconfigureerd. Met behulp van een script voor implementatie, kan dit proces de download en de configuratie van de io.js worden aangepast.

De [io.js implementatiescript](https://github.com/felixrieseberg/iojs-azure) is beschikbaar op GitHub. Io.js op uw web app, **.deployment**, **deploy.cmd** en **IISNode.yml** gewoon kopiëren naar de hoofdmap van de toepassingsmap van uw en implementatie van Web Apps.  

Het eerste bestand, **.deployment**, zorgt Web-Apps uit te voeren **deploy.cmd** na implementatie. Dit script wordt uitgevoerd de gebruikelijke stappen voor de toepassing van een Node.js, maar ook de nieuwste versie van io.js gedownload. Tot slot configureert **IISNode.yml** Web Apps gewoon de gedownloade io.js binary gebruikt in plaats van een vooraf geïnstalleerde Node.js binair.

> [AZURE.NOTE] De gebruikte io.js binary bijwerken, net opnieuw uw toepassing - wordt het script gedownload een nieuwe versie van io.js enkel wanneer die de toepassing wordt gedistribueerd.

<a id="manualinstallation"></a>
## <a name="using-manual-installation"></a>Met behulp van handmatige installatie

De handmatige installatie van een versie met aangepaste io.js bevat slechts twee stappen. Eerst **win x64** binaire downloaden rechtstreeks van de [verdeling van de io.js]. Vereist zijn twee bestanden - **iojs.exe** en **iojs.lib**. Beide bestanden opslaan op een map in uw web app, bijvoorbeeld in een **bak/iojs**.

Web Apps gebruik **iojs.exe** in plaats van een vooraf geïnstalleerde versie van knooppunt configureren, een **IISNode.yml** -bestand maken in de hoofdmap van de toepassing en voeg de volgende regel.

    nodeProcessCommandLine: "D:\home\site\wwwroot\bin\iojs\iojs.exe"

<a id="nextsteps"></a>
## <a name="next-steps"></a>Volgende stappen

In dit artikel die hebt u geleerd hoe u verstrekt io.js met App Service Web Apps met behulp van zowel implementatie scripts ook als handmatige installatie. 

> [AZURE.NOTE] IO.js is in ontwikkeling zwaar en vaker dan Node.js bijgewerkt. Een aantal modules Node.js mogelijk niet werken met io.js - Neem consult [io.js op GitHub] voor het oplossen van problemen.

## <a name="whats-changed"></a>Wat er veranderd
* Zie voor een handleiding voor het wijzigen van Websites met App-Service: [Azure App Service en de Impact op de bestaande Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)

>[AZURE.NOTE] Als u wilt aan de slag met Azure App-Service voordat u aanmelden voor een account met Azure, gaat u naar de [App-Service probeert](http://go.microsoft.com/fwlink/?LinkId=523751), waar u direct een starter tijdelijk web app in de App-Service maken kunt. Geen creditcard vereist; geen verplichtingen.

[IO.js]: https://iojs.org
[IO.js distributie]: https://iojs.org/dist/
[IO.js op GitHub]: https://github.com/iojs/io.js
[io.js Deployment Script]: https://github.com/felixrieseberg/iojs-azure
 