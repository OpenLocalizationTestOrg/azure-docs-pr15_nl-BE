<properties
    pageTitle="Aan de slag met de SDK IoT Hub Gateway | Microsoft Azure"
    description="In dit scenario Azure IoT Gateway SDK wordt Linux gebruikt ter illustratie van basisbegrippen, die u weten moet wanneer u de SDK van Azure IoT Gateway."
    services="iot-hub"
    documentationCenter=""
    authors="chipalost"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="cpp"
     ms.topic="get-started-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/25/2016"
     ms.author="andbuc"/>


# <a name="azure-iot-gateway-sdk-beta---get-started-using-linux"></a>Azure IoT Gateway SDK (bèta) - aan de slag met Linux

[AZURE.INCLUDE [iot-hub-gateway-sdk-getstarted-selector](../../includes/iot-hub-gateway-sdk-getstarted-selector.md)]

## <a name="how-to-build-the-sample"></a>Het bouwen van het monster

Voordat u begint, moet u [uw ontwikkelomgeving instellen] [ lnk-setupdevbox] voor het werken met de SDK op Linux.

1. Open een shell.
2. Ga naar de hoofdmap in het lokale exemplaar van de opslagplaats **azure iot-gateway sdk** .
3. Voer het script **tools/build.sh** . Dit script maakt gebruik van het hulpprogramma **cmake** Maak een map **maken** in de hoofdmap van uw lokale kopie van de opslagplaats **azure iot-gateway sdk** en een make-bestand genereren. Het script vervolgens maakt de oplossing en de test wordt uitgevoerd.

> [AZURE.NOTE]  Telkens wanneer u het script **build.sh** uitvoeren, wordt verwijderd en vervolgens wordt de **build** -map in de hoofdmap van uw lokale kopie van de opslagplaats **azure iot-gateway sdk** opnieuw gemaakt.

## <a name="how-to-run-the-sample"></a>Het uitvoeren van de steekproef

1. Het script **build.sh** wordt de uitvoer weergegeven in de map **maken** in het lokale exemplaar van de opslagplaats **azure iot-gateway sdk** . Dit geldt ook voor de twee modules die in dit voorbeeld wordt gebruikt.

    Het build-script plaatst **liblogger_hl.so** in de **modules-build/logger/** map- en **libhello_world_hl.so** in de **modules-build/hello_world/** map. Gebruik deze paden voor de waarde van het **pad van de module** , zoals in het instellingenbestand JSON.

2. Het bestand **hello_world_lin.json** in de map **samples/hello_world/src** is een voorbeeld van de JSON-instellingenbestand voor Linux die u gebruiken kunt voor het uitvoeren van de steekproef. Het onderstaande voorbeeld JSON-instellingen wordt ervan uitgegaan dat u het monster uit de hoofdmap van uw lokale kopie van de opslagplaats **azure iot-gateway sdk** wordt uitgevoerd.

3. Stel de waarde **bestandsnaam** aan de naam en het pad van het bestand waarin u de logboekgegevens voor de module **logger_hl** in de sectie **args** .

    Dit is een voorbeeld van een instellingenbestand JSON Linux die worden geschreven naar de **log.txt** in de map waar u het voorbeeld uitvoert.

    ```
    {
      "modules" :
      [ 
        {
          "module name" : "logger_hl",
          "module path" : "./build/modules/logger/liblogger_hl.so",
          "args" : 
          {
            "filename":"./log.txt"
          }
        },
        {
          "module name" : "hello_world",
          "module path" : "./build/modules/hello_world/libhello_world_hl.so",
          "args" : null
        }
      ],
      "links" :
      [
        {
          "source": "hello_world",
          "sink": "logger_hl"
        }
      ]
    }
    ```

3. Ga naar **azure iot-gateway sdk** -map in uw Windows-shell.
4. Voer de volgende opdracht:
  
  ```
  ./build/samples/hello_world/hello_world_sample ./samples/hello_world/src/hello_world_lin.json
  ``` 

[AZURE.INCLUDE [iot-hub-gateway-sdk-getstarted-code](../../includes/iot-hub-gateway-sdk-getstarted-code.md)]

<!-- Links -->
[lnk-setupdevbox]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/doc/devbox_setup.md
