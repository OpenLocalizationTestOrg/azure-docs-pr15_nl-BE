<properties
 pageTitle="Maak een Azure functie app en Azure opslag account | Microsoft Azure"
 description="De functie Azure app luistert naar Azure IoT hub gebeurtenissen binnenkomende berichten verwerkt en schrijft deze naar Azure tabelopslag."
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

# <a name="31-create-an-azure-function-app-and-azure-storage-account"></a>3.1 een Azure functie app en Azure opslag account maken

[Azure-functies](../../articles/azure-functions/functions-overview.md) is een oplossing voor het uitvoeren van eenvoudige kleine stukjes code, zogenaamde "functies", in de cloud. Een app Azure functie host van de uitvoering van de functies in Azure.

## <a name="311-what-will-you-do"></a>3.1.1 Wat doet u

Een sjabloon Azure Resource Manager gebruiken voor het maken van een app Azure functie en een account voor de opslag van Azure. De functie Azure app luistert naar Azure IoT hub gebeurtenissen binnenkomende berichten verwerkt en schrijft deze naar Azure tabelopslag. Als u aan eventuele problemen, oplossingen in de [pagina voor probleemoplossing met](iot-hub-raspberry-pi-kit-node-troubleshooting.md)zoeken.

## <a name="312-what-will-you-learn"></a>3.1.2 leert

- Het [Azure Resource Manager](../../articles/azure-resource-manager/resource-group-overview.md) gebruiken voor het implementeren van Azure bronnen.
- Het gebruik van een app Azure functie IoT hub berichten verwerken en schrijf deze op een tabel in de Azure tabelopslag.

## <a name="313-what-do-you-need"></a>3.1.3 wat hebt u nodig

- U moet hebben voltooid de vorige lessen: [aan de slag met uw Pi frambozen 3](iot-hub-raspberry-pi-kit-node-get-started.md) en [maken uw Azure IoT-hub](iot-hub-raspberry-pi-kit-node-get-started.md).

## <a name="314-open-the-sample-app"></a>3.1.4 de monster app openen

Open het voorbeeldproject in Visual Studio-Code door de volgende opdrachten:

```bash
cd Lesson3
code .
```

![Repo-structuur](media/iot-hub-raspberry-pi-lessons/lesson3/repo_structure.png)

- De `app.js` -bestand in de `app` submap is de belangrijkste bronbestand. Dit bestand bevat de code voor 20 keer een bericht verzenden naar uw hub IoT en knippert de LED voor elk bericht dat wordt verzonden.
- De `arm-template.json` -bestand is de sjabloon Azure Resource Manager met een app Azure functie en een account voor de opslag van Azure.
- De `arm-template-param.json` bestand is het configuratiebestand dat door de sjabloon Azure Resource Manager gebruikt.
- De `ReceiveDeviceMessages` submap bevat de code voor de functie Azure Node.js.

## <a name="315-configure-azure-resource-manager-templates-and-create-resources-in-azure"></a>3.1.5 sjablonen Azure resourcemanager configureren en maken van bronnen in Azure

Update de `arm-template-param.json` bestand in de Visual Studio-Code.

![Sjabloonparameters met Azure Resource Manager](media/iot-hub-raspberry-pi-lessons/lesson3/arm_para.png)

- **[Naam van uw IoT Hub]** vervangen door **{Mijn hubnaam}** die u in [Les 2](iot-hub-raspberry-pi-kit-node-lesson2-prepare-azure-iot-hub.md)hebt opgegeven.
- **[Voorvoegseltekenreeks voor nieuwe resources]** vervangen door een voorvoegsel dat u wilt. Het voorvoegsel zorgt ervoor dat de bronnaam is uniek conflicten te vermijden. Gebruik geen streepje of de eerste in het voorvoegsel van het nummer.

> [AZURE.NOTE] U hoeft geen `azure_storage_connection_string` in deze sectie. Houd het is.

Na het bijwerken van de `arm-template-param.json` bestand, de bronnen op Azure implementeren door de volgende opdracht uit te voeren:

```bash
az resource group deployment create --template-file-path arm-template.json --parameters-file-path arm-template-param.json -g iot-sample -n mydeployment
```

Het duurt ongeveer vijf minuten voor het maken van deze bronnen. Tijdens het maken van de resource uitgevoerd wordt, kunt u op verplaatsen naar de volgende sectie.

## <a name="316-summary"></a>3.1.6 samenvatting

U kunt uw app Azure-functie voor het verwerken van berichten voor IoT hub en een account Azure opslag voor het opslaan van deze berichten hebt gemaakt. U kunt op verplaatsen naar de volgende sectie om te implementeren en uitvoeren van het monster apparaat naar cloud om berichten te verzenden met uw Pi.

## <a name="next-steps"></a>Volgende stappen

[3.2 voorbeeldtoepassing apparaat naar cloud om berichten te verzenden op de Pi frambozen 3 uitvoeren](iot-hub-raspberry-pi-kit-node-lesson3-run-azure-blink.md)

