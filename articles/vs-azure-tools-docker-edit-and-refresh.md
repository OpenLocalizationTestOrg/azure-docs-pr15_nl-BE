<properties
   pageTitle="Foutopsporing in toepassingen in een lokale Docker container | Microsoft Azure"
   description="Meer informatie over het wijzigen van een toepassing die wordt uitgevoerd in een lokale Docker-container, de container via bewerken en vernieuwen vernieuwen en fouten opsporen onderbrekingspunten instellen"
   services="azure-container-service"
   documentationCenter="na"
   authors="mlearned"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="07/22/2016"
   ms.author="mlearned" />

# <a name="debugging-apps-in-a-local-docker-container"></a>Foutopsporing in toepassingen in een container met lokale Docker

## <a name="overview"></a>Overzicht
Visual Studio Tools voor Docker biedt een consistente manier te ontwikkelen een en valideren van uw toepassing lokaal in een container Linux Docker.
U hoeft niet opnieuw op te starten van de container telkens wanneer u een code wijzigen.
In dit artikel zal illustreren hoe u met de functie "Bewerken en vernieuwen" start een ASP.NET Core Web app in een lokale Docker container, breng de gewenste wijzigingen en vernieuw de browser om deze wijzigingen te zien.
Het wordt ook hoe u onderbrekingspunten voor foutopsporing instellen.

> [AZURE.NOTE] Ondersteuning voor Windows-Container die een toekomstige versie

## <a name="prerequisites"></a>Vereisten
De volgende hulpprogramma's moeten worden geïnstalleerd.

- [Visual Studio 2015 Update 2](https://go.microsoft.com/fwlink/?LinkId=691978)
- Update voor [Visual Studio 2015 3](https://go.microsoft.com/fwlink/?LinkId=691129)
- [Microsoft ASP.NET Core 1.0 SDK](https://go.microsoft.com/fwlink/?LinkID=809122)

Uitvoeren van Docker containers lokaal, moet u een lokale docker client.
Kunt u vrijgegeven [Docker werkset](https://www.docker.com/products/overview#/docker_toolbox) waarvoor u Hyper-V worden uitgeschakeld of kunt u [Docker voor Windows Beta](https://beta.docker.com) die Hyper-V wordt gebruikt, en moet Windows 10.

Als de werkset Docker, moet u [de Docker-client](./vs-azure-tools-docker-setup.md) configureren

## <a name="1-create-a-web-app"></a>1. Maak een web app.

[AZURE.INCLUDE [create-aspnet5-app](../includes/create-aspnet5-app.md)]

## <a name="2-add-docker-support"></a>2. ondersteuning van Docker toevoegen

[AZURE.INCLUDE [Add docker support](../includes/vs-azure-tools-docker-add-docker-support.md)]


## <a name="3-edit-your-code-and-refresh"></a>3. Bewerk uw code en vernieuwen

U kunt u snel wijzigingen doorlopen, start de toepassing in een container en nog wijzigingen aanbrengen, weergeven, zoals u zou met IIS Express doen.

1. De configuratie van de oplossing wordt ingesteld op `Debug` en druk op ** &lt;CTRL + F5 >** voor het bouwen van uw image docker en lokaal uitvoeren.

    Nadat de installatiekopie van de container is gebouwd en wordt uitgevoerd in een container Docker, Visual Studio de Web app in uw standaardbrowser gestart.
    Als u met behulp van de browser Microsoft Edge of anders fouten, Zie de sectie [problemen oplossen hebben](vs-azure-tools-docker-troubleshooting-docker-errors.md) .

1. Ga naar de pagina over, dat is waar gaan we onze wijzigingen aanbrengen.

1. Terug naar Visual Studio en open `Views\Home\About.cshtml`.

1. De volgende HTML-inhoud toevoegen aan het einde van het bestand en sla de wijzigingen.

    ```
    <h1>Hello from a Docker Container!</h1>
    ```

1.  Het uitvoervenster weergeven wanneer de .NET build is voltooid en u deze regels zien, gaat u terug naar uw browser en vernieuw de pagina over.

    ```
    Now listening on: http://*:80
    Application started. Press Ctrl+C to shut down
    ```

1.  De wijzigingen zijn toegepast.

## <a name="4-debug-with-breakpoints"></a>4. foutopsporing met onderbrekingspunten

Vaak moet wijzigingen verder onderzoek, gebruik te maken van de functies voor foutopsporing van Visual Studio.

1.  Terug naar Visual Studio en openen`Controllers\HomeController.cs`

1.  De inhoud van de methode About() wordt vervangen door het volgende:

    ```
    string message = "Your application description page from wthin a Container";
    ViewData["Message"] = message;
    ````

1.  Een onderbrekingspunt instellen links van de `string message`... lijn.

1.  Klik op ** &lt;F5 >** foutopsporing te starten.

1.  Navigeer naar de pagina over naar het onderbrekingspunt.

1.  Ga naar Visual Studio het onderbrekingspunt weergeven en inspecteren van de waarde van het bericht.

    ![][2]

##<a name="summary"></a>Samenvatting

Met [Visual Studio 2015 Tools voor Docker](https://aka.ms/DockerToolsForVS)krijgt u de productiviteit van het lokaal, werken met het realisme productie binnen een container Docker ontwikkelen.

## <a name="troubleshooting"></a>Het oplossen van problemen

[Problemen met Visual Studio Docker ontwikkeling](vs-azure-tools-docker-troubleshooting-docker-errors.md)

## <a name="more-about-docker-with-visual-studio-windows-and-azure"></a>Meer informatie over Docker met Visual Studio, Windows en Azure

- [Docker Tools for Visual Studio](http://aka.ms/dockertoolsforvs) - ontwikkeling van de Core .NET code in een container
- [Docker Tools for Visual Studio Team Services](http://aka.ms/dockertoolsforvsts) - bouwen en implementeren van docker containers
- [Docker Tools for Visual Studio Code](http://aka.ms/dockertoolsforvscode) - Language services voor docker-bestanden bewerken met meer e2e scenario's die afkomstig zijn
- [Informatie over Windows-Container](http://aka.ms/containers)- Windows Server en Nano Server informatie
- [Azure Container Service](https://azure.microsoft.com/services/container-service/) - [Azure Container Service inhoud](http://aka.ms/AzureContainerService)
-    Zie [werken met Docker](https://github.com/Microsoft/HealthClinic.biz/wiki/Working-with-Docker) van de [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz) 2015 Connect [demo](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/)voor meer voorbeelden van het werken met Docker. Zie voor meer QuickStart uit de HealthClinic.biz-demo [Azure Developer Tools QuickStart](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts).

## <a name="various-docker-tools"></a>Verschillende hulpprogramma's voor Docker

[Sommige gereedschappen geweldige docker (blog van Steve Lasker)](https://blogs.msdn.microsoft.com/stevelasker/2016/03/25/some-great-docker-tools/)

## <a name="good-articles"></a>Goede artikelen

[Inleiding tot Microservices van NGINX](https://www.nginx.com/blog/introduction-to-microservices/)

## <a name="presentations"></a>Presentaties

- [Steve Lasker: VS Live 2016 in Las Vegas - Docker e2e](https://github.com/SteveLasker/Presentations/blob/master/VSLive2016/Vegas/)
- [Inleiding tot ASP.NET Core @ 2016 - waar u op Demo maken](https://channel9.msdn.com/Events/Build/2016/B810)
- [Ontwikkelen van .NET-toepassingen in de containers, kanaal 9](https://blogs.msdn.microsoft.com/stevelasker/2016/02/19/developing-asp-net-apps-in-docker-containers/)

[2]: ./media/vs-azure-tools-docker-edit-and-refresh/breakpoint.png
