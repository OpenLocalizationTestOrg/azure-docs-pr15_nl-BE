<properties
    pageTitle="Maak een Android app op mobiele Apps Azure App Service | Microsoft Azure"
    description="Volg deze zelfstudie aan de slag met Azure mobiele app backends gebruiken voor de ontwikkeling van Android"
    services="app-service\mobile"
    documentationCenter="android"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="na"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="hero-article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>

#<a name="create-an-android-app"></a>Maak een Android app

[AZURE.INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a>Overzicht

In deze zelfstudie wordt beschreven hoe u een cloud-gebaseerde back-end-service toevoegen aan een Android mobiele app met behulp van een backend Azure mobiele app.  U maakt een eenvoudige _takenlijst_ Android app die app gegevens worden opgeslagen in Azure en een nieuwe mobiele app backend.

Voltooien van deze zelfstudie is een vereiste voor alle andere Android zelfstudies over het gebruik van de functie van mobiele Apps in Azure App-Service.

## <a name="prerequisites"></a>Vereisten

Deze zelfstudie hebt u het volgende nodig:

* [Android Developer Tools](https://developer.android.com/sdk/index.html), waaronder de geïntegreerde ontwikkelomgeving van Android Studio en de nieuwste Android-platform.
* Azure mobiele Android SDK, die automatisch wordt verwezen als onderdeel van de quickstart-project dat u wilt downloaden.
* Een [actieve Azure account](https://azure.microsoft.com/pricing/free-trial/).

## <a name="create-a-new-azure-mobile-app-backend"></a>Maak een nieuwe Azure mobiele app backend.

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

## <a name="configure-the-server-project"></a>De serverproject configureren

[AZURE.INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-the-android-app"></a>Download en installeer de Android app

[AZURE.INCLUDE [app-service-mobile-android-run-app](../../includes/app-service-mobile-android-run-app.md)]

<!-- URLs -->
[Azure portal]: https://portal.azure.com/
[Visual Studio Community 2013]: https://go.microsoft.com/fwLink/p/?LinkID=534203
