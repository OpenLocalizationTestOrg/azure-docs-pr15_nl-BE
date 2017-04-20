<properties
    pageTitle="Overzicht Microsoft Azure IoT Suite | Microsoft Azure"
    description="Overzicht van de Azure IoT Suite biedt internet dingen vooraf geconfigureerde oplossingen om te verzamelen, analyseren, en opslag van gegevens bieden visualisaties en integreren met andere systemen."
    services=""
    suite="iot-suite"
    documentationCenter=""
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-suite"
     ms.devlang="na"
     ms.topic="get-started-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/09/2016"
     ms.author="dobett"/>

# <a name="what-is-azure-iot-suite"></a>Wat is Azure IoT Suite?

Azure internet van dingen (IoT) services bieden een breed scala aan mogelijkheden. Deze klasse enterprise services kunnen u:

- Gegevens verzamelen van apparaten
- Analyseren van gegevensstromen in de beweging
- Opslaan en opvragen van grote gegevenssets
- Real-time en historische gegevens visualiseren
- Integratie met back-office systemen

Voor deze mogelijkheden, Azure IoT Suite pakketten samen meerdere Azure services met aangepaste extensies als *vooraf geconfigureerde oplossingen*. Deze vooraf geconfigureerde oplossingen zijn basis implementaties van algemene IoT oplossing patronen die helpen bij het verminderen van de tijd die u voor uw IoT oplossingen. Met de [IoT software development kits][lnk-sdks], u kunt aanpassen en uitbreiden van deze oplossingen om uw eigen vereisten te voldoen. U kunt deze oplossingen ook gebruiken als voorbeelden of sjablonen wanneer u nieuwe IoT oplossingen ontwikkelt.

De volgende video bevat een inleiding tot Azure IoT Suite:

> [AZURE.VIDEO azurecon-2015-introducing-the-microsoft-azure-iot-suite]

## <a name="azure-iot-services-in-azure-iot-suite"></a>Azure IoT services in Azure IoT Suite

De vooraf geconfigureerde oplossingen gebruiken meestal de volgende services:

- Core Azure IoT Suite is de [Azure IoT Hub] [ lnk-iot-hub] service. Deze service biedt het apparaat naar cloud en wolk naar apparaat messaging-mogelijkheden en fungeert als gateway naar de cloud en de andere belangrijke IoT Suite-services. De service kunt u opdrachten naar uw apparaten verzenden en ontvangen van berichten van uw apparaten op schaal.

- [Azure Stream Analytics] [ lnk-asa] biedt gegevensanalyse in beweging. IoT Suite maakt gebruik van deze service om te verwerken binnenkomende telemetrie, samenvoeging uitvoeren en gebeurtenissen te detecteren. De vooraf geconfigureerde oplossingen gebruiken ook stroom analytics informatieve berichten die gegevens zoals metagegevens of opdracht antwoorden van de apparaten bevatten te verwerken. Stream Analytics oplossingen gebruiken voor verwerking van de berichten van uw apparaten en die berichten leveren aan andere diensten.

- [Azure opslag] [ lnk-azure-storage] en [Azure DocumentDB] [ lnk-document-db] de opslagmogelijkheden bieden. De vooraf geconfigureerde oplossingen gebruiken blob-opslag voor het opslaan van Telemetrie en beschikbaar voor analyse. De oplossingen gebruiken een DocumentDB Apparaatmetagegevens worden opgeslagen en het inschakelen van de mogelijkheden van de apparaat beheer van de oplossingen.

- [Azure Web Apps] [ lnk-web-apps] en [Microsoft Power BI] [ lnk-power-bi] de visualisatie mogelijkheden bieden. De flexibiliteit van de Power BI kunt u snel uw eigen interactieve dashboards die gebruikmaken van gegevens uit de IoT te bouwen.

Zie voor een overzicht van de architectuur van een standaardoplossing IoT [Microsoft Azure en het Internet van dingen (IoT)][iot-suite-what-is-azure-iot].

## <a name="preconfigured-solutions"></a>Vooraf geconfigureerde oplossingen

IoT Suite bevat vooraf geconfigureerde oplossingen waarmee u snel aan de slag met en Verken de IoT gevallen zoals *Remote monitoring* en *preventief onderhoud*, die Azure IoT Suite mogelijk maakt. U kunt deze oplossingen implementeren op uw abonnement Azure en voer vervolgens een volledige, end-to-end IoT scenario.

## <a name="next-steps"></a>Volgende stappen

Nu dat u een overzicht van IoT Suite kunt doen en wat zijn de belangrijkste componenten hebt, kunt u meer informatie over de vooraf geconfigureerde oplossingen in IoT Suite, Zie [Wat de Azure IoT zijn vooraf geconfigureerde oplossingen?][lnk-what-are-preconfig]

[lnk-sdks]: https://azure.microsoft.com/documentation/articles/iot-hub-sdks-summary/
[lnk-iot-hub]: https://azure.microsoft.com/documentation/services/iot-hub/
[lnk-asa]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-azure-storage]: https://azure.microsoft.com/documentation/services/storage/
[lnk-document-db]: https://azure.microsoft.com/documentation/services/documentdb/
[lnk-power-bi]: https://powerbi.microsoft.com/
[lnk-web-apps]: https://azure.microsoft.com/documentation/services/app-service/web/
[iot-suite-what-is-azure-iot]: iot-suite-what-is-azure-iot.md
[lnk-what-are-preconfig]: iot-suite-what-are-preconfigured-solutions.md
