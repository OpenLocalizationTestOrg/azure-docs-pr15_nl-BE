<properties
   pageTitle="Configureer een Host Docker met VirtualBox | Microsoft Azure"
   description="Stapsgewijze instructies voor het configureren van een standaardexemplaar Docker Docker Machine en VirtualBox gebruiken"
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
   ms.date="06/08/2016"
   ms.author="mlearned" />

# <a name="configure-a-docker-host-with-virtualbox"></a>Configureer een Host Docker met VirtualBox

## <a name="overview"></a>Overzicht
Dit artikel helpt u bij het configureren van een standaardexemplaar Docker Docker Machine en VirtualBox gebruiken. Als u de [bètaversie van Docker voor Windows](http://beta.docker.com/)gebruikt, is deze configuratie niet nodig.

## <a name="prerequisites"></a>Vereisten
De volgende hulpprogramma's moeten worden geïnstalleerd.

- [Werkset docker](https://www.docker.com/products/overview#/docker_toolbox)

## <a name="configuring-the-docker-client-with-windows-powershell"></a>De client Docker configureren met Windows PowerShell

Als u een client Docker configureren, gewoon open Windows PowerShell en voer de volgende stappen uit:

1. Maak een standaardexemplaar docker-host.

    ```PowerShell
    docker-machine create --driver virtualbox default
    ```
 
1. Controleer of dat het standaardexemplaar is geconfigureerd en wordt uitgevoerd. (Ziet u een exemplaar met de naam standaard uitgevoerd.

    ```PowerShell
    docker-machine ls 
    ```
        
    ![docker machine ls-uitvoer][0]
 
1. Als de huidige host instellen en configureren van uw shell.

    ```PowerShell
    docker-machine env default | Invoke-Expression
    ```

1. De actieve Docker containers weergegeven. De lijst moet leeg zijn.

    ```PowerShell
    docker ps
    ```

    ![docker ps-uitvoer][1]
 
> [AZURE.NOTE]Telkens wanneer die u opnieuw uw ontwikkelcomputer opstarten, moet u opnieuw uw lokale docker host.
> Hiertoe verlenen de volgende opdracht bij een opdrachtprompt: `docker-machine start default`.

[0]: ./media/vs-azure-tools-docker-setup/docker-machine-ls.png
[1]: ./media/vs-azure-tools-docker-setup/docker-ps.png
