<properties
    pageTitle="Aan de slag met de SDK IoT Hub Gateway | Microsoft Azure"
    description="Azure IoT Gateway SDK scenario met behulp van Windows ter illustratie van basisbegrippen, dat moet u weten wanneer u de SDK van Azure IoT Gateway gebruikt."
    services="iot-hub"
    documentationCenter=""
    authors="chipalost"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="cpp"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/25/2016"
     ms.author="andbuc"/>


# <a name="azure-iot-gateway-sdk-beta---get-started-using-windows"></a>Azure IoT Gateway SDK (bèta) - aan de slag met Windows

[AZURE.INCLUDE [iot-hub-gateway-sdk-getstarted-selector](../../includes/iot-hub-gateway-sdk-getstarted-selector.md)]

## <a name="how-to-build-the-sample"></a>Het bouwen van het monster

Voordat u begint, moet u [uw ontwikkelomgeving instellen] [ lnk-setupdevbox] voor de samenwerking met de SDK van Windows.

1. Open **de opdrachtprompt Developer voor VS2015** vanaf de opdrachtprompt.
2. Ga naar de hoofdmap in het lokale exemplaar van de opslagplaats **azure iot-gateway sdk** .
3. Voer de **Extra\\build.cmd** script. Dit script maakt een bestand Visual Studio-oplossing, de oplossing is gebaseerd en de tests uitgevoerd. De Visual Studio-oplossing vindt u in de map **maken** in het lokale exemplaar van de opslagplaats **azure iot-gateway sdk** .

## <a name="how-to-run-the-sample"></a>Het uitvoeren van de steekproef

1. Het script **build.cmd** maakt een map **maken** in het lokale exemplaar van de bibliotheek. Deze map bevat de twee modules die in dit voorbeeld wordt gebruikt.

    Het build-script plaatst **logger_hl.dll** in de **bouwen\\modules\\logger\\Debug** map- en **hello_world_hl.dll** in de **bouwen\\modules\\hello_world\\Debug** map. Gebruik deze paden voor de waarde van het **pad van de module** , zoals in het instellingenbestand JSON.

2. Het bestand **hello_world_win.json** in de **monsters\\hello_world\\src** map is een voorbeeld van JSON instellingenbestand voor Windows die u gebruiken kunt voor het uitvoeren van de steekproef. Het onderstaande voorbeeld JSON-instellingen wordt ervan uitgegaan dat u de bibliotheek IoT Gateway SDK naar de hoofdmap van station **C:** gekloond. Als u deze naar een andere locatie hebt gedownload, moet u de waarden van de **module pad** in aanpassen de **monsters\\hello_world\\src\\hello_world_win.json** bestand dienovereenkomstig.

3. Stel de waarde **bestandsnaam** aan de naam en het pad van het bestand waarin u de logboekgegevens voor de module **logger_hl** in de sectie **args** .

    Dit is een voorbeeld van een JSON-instellingenbestand voor Windows die worden geschreven naar het bestand **log.txt** in de hoofdmap van station **c** bevindt.

    ```
    {
      "modules" :
      [
        {
          "module name" : "logger_hl",
          "module path" : "C:\\azure-iot-gateway-sdk\\build\\modules\\logger\\Debug\\logger_hl.dll",
          "args" : 
          {
            "filename":"C:\\log.txt"
          }
        },
        {
          "module name" : "hello_world",
          "module path" : "C:\\azure-iot-gateway-sdk\\build\\\\modules\\hello_world\\Debug\\hello_world_hl.dll",
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

3. Bij een opdrachtprompt, Ga naar de hoofdmap van uw lokale kopie van de opslagplaats **azure iot-gateway sdk** .
4. Voer de volgende opdracht:
  
  ```
  build\samples\hello_world\Debug\hello_world_sample.exe samples\hello_world\src\hello_world_win.json
  ```

[AZURE.INCLUDE [iot-hub-gateway-sdk-getstarted-code](../../includes/iot-hub-gateway-sdk-getstarted-code.md)]

<!-- Links -->
[lnk-setupdevbox]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/doc/devbox_setup.md