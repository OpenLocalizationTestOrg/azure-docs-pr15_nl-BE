<properties 
    pageTitle="Met PM2 configuratie voor NodeJS in Web Apps op Linux | Microsoft Azure" 
    description="Met de configuratie van de PM2 voor NodeJS in Web Apps op Linux" 
    keywords="service Azure app, web app, nodejs, pm2, linux, oss"
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

# <a name="using-pm2-configuration-for-nodejs-in-web-apps-on-linux"></a>Met de configuratie van de PM2 voor Node.js in Web Apps op Linux

Als u de stapel toepassing op Node.js voor Web Apps op Linux instelt, krijgt u de optie voor het instellen van een opstartbestand Node.js zoals in de onderstaande afbeelding.

![][1]

U kunt dit gebruiken om een

-   Het opstartscript voor uw app Node.js opgeven (bijvoorbeeld: /bin/server.js)
-   Geef het configuratiebestand PM2 voor uw app Node.js (bijvoorbeeld: /foo/process.json)

 >[AZURE.NOTE] Als u uw processen knooppunt opnieuw wordt opgestart wilt als bepaalde bestanden worden gewijzigd, moet u PM2 configuratie te gebruiken. Anders is de toepassing wordt niet opnieuw opgestart als deze meldingen van zaken als continu distributie ontvangt wanneer de toepassingscode wordt gewijzigd.

U kunt controleren de Node.js [Procesdocumentatie bestand](http://pm2.keymetrics.io/docs/usage/application-declaration/) voor alle opties, maar hieronder is een voorbeeld van wat u als het bestand process.json gebruiken zou

        {
          "name"        : "worker",
          "script"      : "/bin/server.js",
          "instances"   : 1,
          "merge_logs"  : true,
          "log_date_format" : "YYYY-MM-DD HH:mm Z",
          "watch": ["/bin/server.js", "foo.txt"],
          "watch_options": {
            "followSymlinks": true,
            "usePolling"   : true,
            "interval"    : 5
          }
        }

Belangrijk om te weten in deze configuratie zijn 

-   De eigenschap 'script' geeft van uw toepassing start script.
-   De eigenschap 'exemplaren' geeft aan hoeveel exemplaren van het knooppunt-proces te starten. Als u uw toepassing op VM grotere met meerdere cores uitvoert, wilt u Maximaliseer uw bedrijfsmiddelen door hier een hogere waarde.
-   De matrix "controle" geeft alle bestanden waarvan de wijziging die u wilt uw processen knooppunt opnieuw.
-   Voor de 'watch_options' moet u op dit moment 'usePolling' opgeven als waar door de manier waarop die de toepassingsinhoud is gekoppeld.


## <a name="next-steps"></a>Volgende stappen ##

* [Wat is de App Service op Linux?](./app-service-linux-intro.md)

<!--Image references-->
[1]: ./media/app-service-linux-using-nodejs-pm2/nodejs-startup-file.png