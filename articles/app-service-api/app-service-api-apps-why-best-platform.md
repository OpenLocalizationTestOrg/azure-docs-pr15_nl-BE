<properties 
    pageTitle="Inleiding API Apps | Microsoft Azure" 
    description="Informatie over hoe Azure App Service helpt u ontwikkelt, host en RESTful API's in beslag nemen." 
    services="app-service\api" 
    documentationCenter=".net" 
    authors="tdykstra" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-api" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="08/23/2016" 
    ms.author="rachelap"/>

# <a name="api-apps-overview"></a>API Apps-overzicht

API apps in Azure App-Service bieden functies waarmee u gemakkelijker te ontwikkelen, host en API's in de cloud en op gebouwen verbruiken. Met de API apps krijgt u kwaliteit bedrijfsbeveiliging, eenvoudige toegang tot hybride verbindingen, automatisch genereren van SDK en naadloze integratie met [Logica Apps](../app-service-logic/app-service-logic-what-are-logic-apps.md).

[Azure App Service](../app-service/app-service-value-prop-what-is.md) is een volledig beheerde platform voor web, mobiele, scenario's en integratie. API Apps is een van de vier app typen aangeboden door [Azure App-Service](../app-service/app-service-value-prop-what-is.md).

![Typen in Azure App Service App](./media/app-service-api-apps-why-best-platform/appservicesuite.png)

## <a name="why-use-api-apps"></a>Waarom API Apps gebruiken?

Hier volgen enkele belangrijke functies van de API Apps:

- **Doen om uw bestaande API als-is** -u hoeft niet te wijzigen van de code in uw bestaande API's om te profiteren van de API Apps--gewoon uw code te implementeren op een API-app. De API kunt gebruiken voor elke taal of het kader wordt ondersteund door de App Service, inclusief ASP.NET en C#, Java, PHP, Node.js en Python.

- **Eenvoudig consumptie** - geïntegreerde ondersteuning voor [metagegevens Swagger API](http://swagger.io/) kunt u uw API's eenvoudig verbruikbare door verschillende clients.  Automatisch genereren van clientcode voor de API's in verschillende talen waaronder C#, Java en Javascript. [CORS](app-service-api-cors-consume-javascript.md) eenvoudig configureren zonder de code te wijzigen. Zie [metagegevens voor het genereren van code voor het opsporen en API App Service API Apps](app-service-api-metadata.md) en [verbruiken een API app uit JavaScript aan met CORS](app-service-api-cors-consume-javascript.md)voor meer informatie. 

- **Eenvoudige toegangscontrole** - een API app beschermen tegen niet-geverifieerde toegang zonder wijzigingen aan uw code. Ingebouwde verificatieservices secure API's voor toegang door andere services of clients die gebruikers vertegenwoordigt. Ondersteunde id-providers zijn Azure Active Directory, Facebook, Twitter, Google en Microsoft-Account. Clients kunnen gebruiken voor Active Directory-verificatie-bibliotheek (ADAL) of de SDK van mobiele Apps. Zie [verificatie en machtiging van Apps in Azure App Service API](app-service-api-authentication.md)voor meer informatie.

- **Integratie met visual Studio** - specifieke tools in Visual Studio stroomlijnen het werk van maken, implementeren, verbruikt, foutopsporing en API apps te beheren. Voor meer informatie, Zie [de Azure SDK voor .NET van punt 2.8.1 aankondigen](/blog/announcing-azure-sdk-2-8-1-for-net/).

- **Integratie met logica Apps** - API apps die u maakt kan worden gebruikt door de [App Service logica Apps](../app-service-logic/app-service-logic-what-are-logic-apps.md).  Zie [werken met uw aangepaste API op App Service met logica apps gehost](../app-service-logic/app-service-logic-custom-hosted-api.md) en [nieuwe schema versie 2015-08-01-voorbeeld](../app-service-logic/app-service-logic-schema-2015-08-01.md)voor meer informatie.

Bovendien kunt een API app profiteren van de functies van [Web Apps](../app-service-web/app-service-web-overview.md) en [Mobiele Apps](../app-service-mobile/app-service-mobile-value-prop.md). Het omgekeerde geldt ook: als u een web app of mobiele app voor het hosten van een API, deze kunt profiteren van functies zoals Swagger metagegevens voor client API Apps code genereren en CORS voor interdomein-browser toegang. Het enige verschil tussen de drie app (API, web, mobiele) is de naam en het pictogram dat wordt gebruikt voor hen in de portal Azure.

## <a name="whats-the-difference-between-api-apps-and-azure-api-management"></a>Wat is het verschil tussen API Apps en Azure API Management?

Zijn aanvullende services API Apps en het [Beheer van Azure-API](../api-management/api-management-key-concepts.md) :

* Beheer-API is over het beheren van API's. U een front-end Management API op een API plaatsen aan het gebruik van de monitor en de gashendel, invoer en uitvoer te manipuleren, verschillende API's samenvoegen tot één eindpunt, enzovoort. De API's worden beheerd, kunnen overal worden gehost.
* API Apps is over het hosten van API's. De service bevat functies die de API's ontwikkelen en in beslag nemen, te vergemakkelijken, maar het niet de soorten bewaking, bandbreedtebeperking, manipuleren of consolideren dat beheer-API biedt. Als u API-beheerfuncties niet hoeft, kunt u de API's hosten in API apps zonder beheer-API.

Hier is een diagram waarin u ziet u de API-beheer gebruikt voor API's die worden gehost in API apps en elders.

![Azure API Management en API Apps](./media/app-service-api-apps-why-best-platform/apia-apim.png)

Sommige functies van de API-Management en API Apps hebben dezelfde functies.  Bijvoorbeeld, kunnen beide CORS ondersteuning automatiseren. Wanneer u de twee services samen gebruikt, gebruikt u beheer-API voor CORS omdat het fungeert als front-end voor de API apps. 

## <a name="getting-started"></a>Aan de slag

Zie de zelfstudie voor het gewenste kader aan de slag met API Apps door de voorbeeldcode een implementeren:

* [ASP.NET](app-service-api-dotnet-get-started.md) 
* [Node.js](app-service-api-nodejs-api-app.md) 
* [Java](app-service-api-java-api-app.md) 

Start een thread in het [forum API Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureAPIApps)om te vragen over API apps. 
