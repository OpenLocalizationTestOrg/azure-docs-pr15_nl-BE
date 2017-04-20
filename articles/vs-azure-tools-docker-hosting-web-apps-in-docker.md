<properties
   pageTitle="Een container voor ASP.NET Core Linux Docker implementeren met een externe host van Docker | Microsoft Azure"
   description="Informatie over het gebruik van Visual Studio Tools for Docker een Core ASP.NET web app implementeren op een Docker container op een Azure Docker Host Linux VM"   
   services="azure-container-service"
   documentationCenter=".net"
   authors="mlearned"
   manager="douge"
   editor=""/>

<tags
   ms.service="azure-container-service"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="06/08/2016"
   ms.author="mlearned"/>

# <a name="deploy-an-aspnet-container-to-a-remote-docker-host"></a>Een container voor ASP.NET distribueren naar een externe host voor Docker

## <a name="overview"></a>Overzicht
Docker is een lichtgewicht container engine, lijkt in sommige opzichten op een virtuele machine, kunt u host voor toepassingen en services.
Deze handleiding begeleidt u bij het bestand met de extensie [Visual Studio 2015 Tools voor Docker](http://aka.ms/DockerToolsForVS) Core ASP.NET-toepassing implementeren op een host Docker op Azure PowerShell gebruiken.

## <a name="prerequisites"></a>Vereisten
Het volgende is vereist voor het voltooien van deze zelfstudie:

- Maak een VM Azure Docker Host zoals beschreven in [het gebruik van docker-machine met Azure](./virtual-machines/virtual-machines-linux-docker-machine.md)
- Update voor [Visual Studio 2015 3](https://go.microsoft.com/fwlink/?LinkId=691129)
- [Microsoft ASP.NET Core 1.0 SDK](https://go.microsoft.com/fwlink/?LinkID=809122)
- Installeer [Visual Studio 2015 Tools voor Docker - voorbeeld](http://aka.ms/DockerToolsForVS)

## <a name="1-create-an-aspnet-core-web-app"></a>1. Maak een Core ASP.NET web app.
De volgende stappen begeleidt u bij het maken van een eenvoudige ASP.NET Core app die in deze zelfstudie worden gebruikt.

[AZURE.INCLUDE [create-aspnet5-app](../includes/create-aspnet5-app.md)]

## <a name="2-add-docker-support"></a>2. ondersteuning van Docker toevoegen

[AZURE.INCLUDE [create-aspnet5-app](../includes/vs-azure-tools-docker-add-docker-support.md)]

## <a name="3-use-the-dockertaskps1-powershell-script"></a>3. Gebruik de PowerShell-Script DockerTask.ps1 

1.  Open een opdrachtprompt PowerShell naar de hoofdmap van het project. 

    ```
    PS C:\Src\WebApplication1>
    ```

1.  Valideren van de externe host wordt uitgevoerd. Ziet u toestand = actief 

    ```
    docker-machine ls
    NAME         ACTIVE   DRIVER   STATE     URL                        SWARM   DOCKER    ERRORS
    MyDockerHost -        azure    Running   tcp://xxx.xxx.xxx.xxx:2376         v1.10.3
    ```

    > [AZURE.NOTE] Als u de bètaversie van Docker gebruikt, wordt de host niet hier weergegeven.

1.  Het gebruik van app bouwen de Build parameter -

    ```
    PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Build -Environment Release -Machine mydockerhost
    ```  

    > [AZURE.NOTE] Als u de bètaversie van Docker, weglaten-argument van de Machine
    > 
    > ```
    > PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Build -Environment Release 
    > ```  


1.  Uitvoeren van de app, met behulp van de - parameter uitvoeren

    ```
    PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Run -Environment Release -Machine mydockerhost
    ```

    > [AZURE.NOTE] Als u de bètaversie van Docker, weglaten-argument van de Machine
    > 
    > ```
    > PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Run -Environment Release 
    > ```

    Nadat docker is voltooid, ziet u de volgende resultaten:

    ![Uw app weergeven][3]

[0]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/docker-props-in-solution-explorer.png
[1]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/change-docker-machine-name.png
[2]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/launch-application.png
[3]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/view-application.png
