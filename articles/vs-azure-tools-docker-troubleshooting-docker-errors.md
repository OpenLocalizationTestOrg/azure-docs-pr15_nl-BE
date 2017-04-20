<properties
   pageTitle="Het oplossen van problemen van Docker-Client op Windows met behulp van Visual Studio | Microsoft Azure"
   description="Oplossen van problemen die optreden bij het gebruik van Visual Studio maken en implementeren van web apps aan Docker in Windows met behulp van Visual Studio."
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
   ms.author="allclark" />

# <a name="troubleshooting-visual-studio-docker-development"></a>Problemen met Visual Studio Docker ontwikkeling

Als u werkt met Visual Studio Tools voor Docker voorbeeld, kunt u enkele problemen vanwege de aard van de voorvertoning tegenkomen.
Hier volgen enkele veelvoorkomende problemen en oplossingen.


## <a name="unable-to-validate-volume-mapping"></a>Kan niet worden gevalideerd volume toewijzen
Volume-toewijzing is vereist voor het delen van de broncode en binaire bestanden van uw toepassing met de map app in de container.  Specifiek volume toewijzingen zijn opgenomen in de bestanden docker compose.dev.debug.yml en docker compose.dev.release.yml. Als bestanden worden gewijzigd op de hostcomputer, weerspiegelen de containers deze wijzigingen in een mapstructuur vergelijkbaar.

Toewijzing van volume, **...-instellingen** opent vanuit het pictogram in het systeemvak Docker voor Windows 'moby' en selecteer vervolgens het tabblad **Gedeelde stations** .  Zorg ervoor dat de stationsletter die fungeert als host voor uw project, alsmede de stationsaanduiding waarop % USERPROFILE % bevindt worden gedeeld door ze te controleren en klik vervolgens op **toepassen**te klikken.

U kunt controleren of het volume toewijzing functioneert nadat u het station hebt gedeeld, opnieuw opbouwen en F5 van binnen Visual Studio of probeer het van de opdracht de volgende vragen:

*In een Windows-opdrachtprompt*

*[Opmerking: dit wordt ervan uitgegaan dat uw gebruikers-map bevindt zich op het station "C" en dat deze is gedeeld.  Zo nodig bijwerken, als u een ander station hebt gedeeld]*
```
docker run -it -v /c/Users/Public:/wormhole busybox
```

*In de container voor Linux*

```
/ # ls
```

Hier ziet u een map uit de map gebruikers/openbare aanbieding.
Als er geen bestanden worden weergegeven en de map /c/Users/Public niet leeg is, wordt toewijzing volume is niet correct geconfigureerd. 

```
bin       etc       proc      sys       usr       wormhole
dev       home      root      tmp       var
```

Wijzigen naar de wormhole-map voor een overzicht van de inhoud van de `/c/Users/Public` map:

```
/ # cd wormhole/
/wormhole # ls
AccountPictures  Downloads        Music            Videos
Desktop          Host             NuGet.Config     desktop.ini
Documents        Libraries        Pictures
/wormhole #
```

**Opmerking:** *Bij het werken met Linux VMs, het container-bestandssysteem hoofdlettergevoelig is.*

##<a name="build--prepareforbuild-task-failed-unexpectedly"></a>Build: De taak 'PrepareForBuild' is onverwacht mislukt.

Microsoft.DotNet.Docker.CommandLine.ClientException: Fout verbinding probeert te maken:

Controleer of dat de standaardhost docker wordt uitgevoerd. Open een opdrachtprompt en uitvoeren:

```
docker info
```

Dit geeft een fout probeert te starten de **Docker voor Windows** desktop app.  Als de desktop app actief is vervolgens de **moby** -pictogram in de lade moet zichtbaar zijn. Klik met de rechtermuisknop op het pictogram in het systeemvak en **Instellingen**te openen.  Klik op het tabblad **opnieuw instellen** en vervolgens **opnieuw Docker..**.

##<a name="manually-upgrading-from-version-031-to-040"></a>Handmatig upgraden van versie 0.31-0,40


1. Back-up van het project
1. Verwijder de volgende bestanden in het project:

    ```
      Dockerfile
      Dockerfile.debug
      DockerTask.ps1
      docker-compose-yml
      docker-compose.debug.yml
      .dockerignore
      Properties\Docker.props
      Properties\Docker.targets
    ```

1. De oplossing sluit en verwijder de volgende regels uit het bestand .xproj:

    ```
      <DockerToolsMinVersion>0.xx</DockerToolsMinVersion>
      <Import Project="Properties\Docker.props" />
      <Import Project="Properties\Docker.targets" />
    ```

1. De oplossing opnieuw openen
1. Verwijder de volgende regels uit het bestand Properties\launchSettings.json:

    ```
      "Docker": {
        "executablePath": "%WINDIR%\\System32\\WindowsPowerShell\\v1.0\\powershell.exe",
        "commandLineArgs": "-ExecutionPolicy RemoteSigned .\\DockerTask.ps1 -Run -Environment $(Configuration) -Machine '$(DockerMachineName)'"
      }
    ```

1. Verwijder de volgende bestanden gerelateerd aan Docker van project.json in de publishOptions:

    ```
    "publishOptions": {
      "include": [
        ...
        "docker-compose.yml",
        "docker-compose.debug.yml",
        "Dockerfile.debug",
        "Dockerfile",
        ".dockerignore"
      ]
    },
    ```

1. De vorige versie verwijderen en installeer Docker extra 0,40 en vervolgens op **toevoegen -> Docker ondersteuning** opnieuw in het contextmenu voor uw ASP.Net-webpagina voor Core of Console-programma. Dit wordt de nieuwe vereiste Docker artefacten opnieuw toevoegen aan uw project. 

## <a name="an-error-dialog-occurs-when-attempting-to-add-docker-support-or-debug-f5-an-aspnet-core-application-in-a-container"></a>Dialoogvenster met een foutbericht weergegeven bij het **toevoegen -> Docker ondersteuning** of Debug (F5) een Core ASP.NET-toepassing in een container

Af en toe wij hebben na het verwijderen en installeren van extensies, Visual Studio MEF (Managed Framework voor uitbreidbaarheid) cache kan beschadigd. Wanneer dit gebeurt kan dit leiden tot verschillende fout bij het toevoegen van ondersteuning voor Docker dialogs en/of wilt uitvoeren of uw ASP.NET-toepassing Core voor foutopsporing (F5). Voer de volgende stappen uit als u wilt verwijderen en opnieuw genereren van de cache MEF als tijdelijke oplossing.

1. Sluit alle exemplaren van Visual Studio
1. %USERPROFILE%\AppData\Local\Microsoft\VisualStudio\14.0\ openen
1. Verwijder de volgende mappen
     ```
       ComponentModelCache
       Extensions
       MEFCacheBackup
    ```
1. Visual Studio openen
1. Het scenario opnieuw proberen 
