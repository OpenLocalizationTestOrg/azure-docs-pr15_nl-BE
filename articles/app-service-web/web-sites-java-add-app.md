<properties 
    pageTitle="Een Java-toepassing naar Azure App Service Web Apps toevoegen" 
    description="In deze zelfstudie wordt beschreven hoe u een pagina of uw exemplaar van Azure App Service Web Apps die al is geconfigureerd voor het gebruik van Java-toepassing toevoegen." 
    services="app-service\web" 
    documentationCenter="java" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

# <a name="add-a-java-application-to-azure-app-service-web-apps"></a>Een Java-toepassing naar Azure App Service Web Apps toevoegen

Zodra u hebt uw Java web app in [Azure App Service][] geïnitialiseerd zoals beschreven bij het [maken van een Java web app in Azure App-Service](web-sites-java-get-started.md), kunt u uw toepassing door de WAR brengen in de map **webapps** uploaden.

De navigatie-pad naar de map **webapps** verschilt op basis van hoe u uw Web Apps-instantie ingesteld.

- Als u uw web app instellen met behulp van de markt Azure, het pad naar de map **webapps** is in de vorm **d:\home\site\wwwroot\bin\application\_server\webapps**, waarbij **toepassing\_server** is de naam van de toepassingsserver van kracht voor uw exemplaar van het Web Apps. 
- Als u uw web app met behulp van de gebruikersinterface voor Azure configuratie instelt, is het pad naar de map met **webapps** in het formulier **d:\home\site\wwwroot\webapps**. 

Houd er rekening mee dat kunt u het besturingselement voor het uploaden van uw toepassing of webpagina's, inclusief [scenario's continue integratie](app-service-continuous-deployment.md). FTP is ook een optie voor het uploaden van uw toepassing of webpagina's.

Opmerking voor Tomcat web apps: zodra u het WAR-bestand hebt geüpload naar de map **webapps** , detecteert de Tomcat-toepassingsserver die u hebt toegevoegd en wordt automatisch geladen. Opmerking Als u de bestanden (met uitzondering van WAR-bestanden) naar de hoofdmap kopiëren, de application server moet opnieuw worden opgestart voordat de bestanden worden gebruikt. De autoload functionaliteit voor de Tomcat Java web apps op Azure is gebaseerd op een nieuwe WAR-bestand wordt toegevoegd, of nieuwe bestanden of mappen die worden toegevoegd aan de map **webapps** . 

## <a name="next-steps"></a>Volgende stappen

Zie [Java Developer Center](/develop/java/)voor meer informatie.

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- External Links -->
[Azure App-Service]: http://go.microsoft.com/fwlink/?LinkId=529714
