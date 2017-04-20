<properties
 pageTitle="Hulpprogramma's (Windows 7 +) | Microsoft Azure"
 description="Download en installeer de vereiste hulpprogramma's en software voor de toepassing van het eerste monster voor uw Pi in Windows 7 en latere versies."
 services="iot-hub"
 documentationCenter=""
 authors="shizn"
 manager="timlt"
 tags=""
 keywords=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="10/21/2016"
 ms.author="xshi"/>

# <a name="12-get-the-tools-windows-7-"></a>1.2 de hulpprogramma's ophalen (Windows 7 +) 

> [AZURE.SELECTOR]
- [Windows 7 +](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-win32.md)
- [Ubuntu-16.04](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-ubuntu.md)
- [macOS 10.10](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-mac.md)

## <a name="121-what-you-will-do"></a>1.2.1 wat u doet

Download de ontwikkelprogramma's en de software voor de eerste voorbeeldtoepassing voor uw Pi frambozen 3. Als u aan eventuele problemen, oplossingen in de [pagina voor probleemoplossing met](iot-hub-raspberry-pi-kit-node-troubleshooting.md)zoeken.

## <a name="122-what-you-will-learn"></a>1.2.2 Wat leert u
- Git en Node.js installeren
  - [GIT](https://git-scm.com) is een open-source gedistribueerd versiebeheersysteem. De voorbeeldtoepassing voor deze les wordt opgeslagen in Git.
  - [Node.js](https://nodejs.org/en/) is een JavaScript-runtime met een rijk pakket ecosysteem.
- Het gebruik van NPM extra Node.js development tools installeren.
  - De minimale versievereisten van Node.js is 4,5 LTS.
  - [NPM](https://www.npmjs.com) is een van de managers van het pakket voor Node.js.

## <a name="123-what-you-need"></a>1.2.3 wat u nodig hebt

- Een Internet-verbinding voor het downloaden van de ontwikkelingsprogramma's en de software
- Een computer waarop Windows wordt uitgevoerd

## <a name="124-install-git-and-nodejs"></a>1.2.4 Git en Node.js installeren

Klik op de onderstaande koppelingen om Git en Node.js LTS voor Windows downloaden en installeren.

- [Git ophalen voor Windows](https://git-scm.com/download/win/)
- [LTS Node.js voor Windows ophalen](https://nodejs.org/en/)

## <a name="125-install-additional-nodejs-development-tools"></a>1.2.5 extra Node.js ontwikkelprogramma's installeren

U [gulp.js](http://gulpjs.com) gebruiken voor het automatiseren van de implementatie van de voorbeeldtoepassing voor uw Pi. U kunt ook de [apparaat-discovery-cli](https://github.com/Azure/device-discovery-cli) gebruiken netwerkinformatie ophalen over uw IoT-apparaten.

Start een opdrachtprompt als administrator. Installeren `gulp` en `device-discovery-cli` met de volgende opdracht:

```bash
npm install -g device-discovery-cli gulp
```
    
Als u problemen hebt met het Node.js en deze extra Node.js ontwikkelprogramma's installeren op uw computer, Zie [troubleshooting guide](iot-hub-raspberry-pi-kit-node-troubleshooting.md) voor oplossingen voor veelvoorkomende problemen.

## <a name="126-install-visual-studio-code"></a>1.2.6 Visual Studio Code installeren

[Download](https://code.visualstudio.com/docs/setup/windows) en installeer Visual Studio Code. Visual Studio Code is een lichtgewicht maar krachtige broncode-editor voor Windows, Linux en macOS. U gebruikt deze editor verderop in de handleiding voor het bewerken van de voorbeeldcode.

## <a name="127-summary"></a>1.2.7 overzicht

U kunt de vereiste ontwikkelprogramma's en software voor de eerste voorbeeldtoepassing hebt geïnstalleerd. In het volgende gedeelte maken, implementeren en uitvoeren van de voorbeeldtoepassing op uw Pi.

## <a name="next-steps"></a>Volgende stappen

[1.3 maken en implementeren van de voorbeeldtoepassing knipperen](iot-hub-raspberry-pi-kit-node-lesson1-deploy-blink-app.md)
