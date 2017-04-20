<properties
    pageTitle="Maken van een app Cordova op mobiele Apps Azure App Service | Microsoft Azure"
    description="Volg deze zelfstudie aan de slag met Azure mobiele app backends gebruiken voor de ontwikkeling van Apache Cordova"
    services="app-service\mobile"
    documentationCenter="javascript"
    authors="adrianhall"
    manager="erikre"
    editor=""
    tags=""
    keywords="cordova, javascript, mobiele, client" />

<tags
    ms.service="app-service-mobile"
    ms.workload="na"
    ms.tgt_pltfrm="mobile-html"
    ms.devlang="javascript"
    ms.topic="hero-article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

#<a name="create-an-apache-cordova-app"></a>Maak een Apache Cordova app

[AZURE.INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a>Overzicht

In deze zelfstudie wordt beschreven hoe u een cloud-gebaseerde back-end-service toevoegen aan een Apache Cordova mobiele app met behulp van een backend Azure mobiele app.  U maakt een eenvoudige _lijst van Todo_ Apache Cordova app die app gegevens worden opgeslagen in Azure en een nieuwe mobiele app backend.

Voltooien van deze zelfstudie is een vereiste voor alle andere Apache Cordova zelfstudies over het gebruik van de functie van mobiele Apps in Azure App-Service.

## <a name="prerequisites"></a>Vereisten

Deze zelfstudie hebt u het volgende nodig:

* Een PC met [Visual Studio Community 2015] of nieuwer.
* [Visual Studio Tools for Apache Cordova].
* Een [actieve Azure account](https://azure.microsoft.com/pricing/free-trial/).

U kunt ook overslaan van Visual Studio en rechtstreeks gebruik van de opdrachtregel Apache Cordova.  Dit is handig bij het voltooien van de zelfstudie op een Mac-computer.  Compileren van Apache Cordova client-toepassingen met behulp van de opdrachtregel, wordt niet gedekt door deze zelfstudie.

## <a name="create-a-new-azure-mobile-app-backend"></a>Maak een nieuwe Azure mobiele app backend.

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

[Bekijk een video met vergelijkbare stappen](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-1-Create-an-Azure-Mobile-App)

## <a name="configure-the-server-project"></a>De serverproject configureren

[AZURE.INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-the-apache-cordova-app"></a>Download en installeer de app Apache Cordova

[AZURE.INCLUDE [app-service-mobile-cordova-run-app](../../includes/app-service-mobile-cordova-run-app.md)]

## <a name="next-steps"></a>Volgende stappen

Nu dat u deze handleiding Snel starten hebt voltooid, gaat u verder met een van de volgende cursussen:

* [Verificatie toevoegen] aan uw Apache Cordova app.
* [Push-meldingen toevoegen] aan uw Apache Cordova app.

Meer informatie over de basisbegrippen met Azure App-Service.

* [Verificatie]
* [Push-meldingen]

Informatie over het gebruik van de SDK's.

* [Apache Cordova SDK]
* [ASP.NET Server SDK]
* [Node.js Server SDK]

<!-- Images. -->

<!-- URLs -->
[Azure portal]: https://portal.azure.com/
[Visual Studio Community 2015]: http://www.visualstudio.com/
[Visual Studio Tools for Apache Cordova]: https://www.visualstudio.com/en-us/features/cordova-vs.aspx
[Verificatie toevoegen]: app-service-mobile-cordova-get-started-users.md
[Push-meldingen toevoegen]: app-service-mobile-cordova-get-started-push.md
[Verificatie]: app-service-mobile-auth.md
[Push-meldingen]: ../notification-hubs/notification-hubs-push-notification-overview.md
[Apache Cordova SDK]: app-service-mobile-cordova-how-to-use-client-library.md
[ASP.NET Server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Node.js Server SDK]: app-service-mobile-node-backend-how-to-use-server-sdk.md
