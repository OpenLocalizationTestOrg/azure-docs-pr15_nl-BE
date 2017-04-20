<properties
 pageTitle="Gelezen berichten bewaard in Azure opslag | Microsoft Azure"
 description="Het apparaat naar cloud-berichten worden gecontroleerd als ze zijn geschreven naar de tabelopslag van uw Azure."
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

# <a name="33-read-messages-persisted-in-azure-storage"></a>3.3 lezen berichten bewaard in Azure opslag

## <a name="331-what-will-you-do"></a>3.3.1 Wat doet u

Het apparaat naar cloud berichten die afkomstig zijn uit uw Pi frambozen 3 op uw hub IoT als de berichten worden geschreven naar de Azure tabelopslag controleren. Als u aan eventuele problemen, oplossingen in de [pagina voor probleemoplossing met](iot-hub-raspberry-pi-kit-node-troubleshooting.md)zoeken.

## <a name="332-what-will-you-learn"></a>3.3.2 leert

- De gulp bericht lezen taak gebruiken om berichten te lezen zijn vastgelegd in Azure tabelopslag.

## <a name="333-what-do-you-need"></a>3.3.3 wat hebt u nodig

- U moet hebben voltooid de vorige sectie [de voorbeeldtoepassing Azure blink op uw Pi frambozen 3 worden uitgevoerd](iot-hub-raspberry-pi-kit-node-lesson3-run-azure-blink.md).

## <a name="334-read-new-messages-from-your-storage-account"></a>3.3.4 nieuwe berichten van uw account opslag lezen

U hebt een voorbeeldtoepassing op uw Pi uitgevoerd in de vorige sectie. De steekproef toepassing verzonden berichten op uw hub Azure IoT. De berichten op uw hub IoT worden opgeslagen in de Azure tabelopslag via de app Azure functie. Moet u de verbindingstekenreeks Azure opslag om berichten te lezen uit de tabelopslag voor uw Azure.

Voor het lezen van berichten die zijn opgeslagen in de Azure tabelopslag als volgt:

1. De verbindingsreeks ophalen door de volgende opdrachten:

    ```bash
    az storage account list -g iot-sample --query [].name
    az storage account show-connection-string -g iot-sample -n {storage name}
    ```

    De eerste opdracht haalt de `storage name` die in de tweede opdracht wordt gebruikt om de verbindingsreeks. `iot-sample`de standaardwaarde is `{resource group name}` als u niet de waarde die u in les 2.

2. Open het configuratiebestand `config-raspberrypi.json` bestand in de Visual Studio-Code door de volgende opdracht uit te voeren:

    ```bash
    # For Windows command prompt
    code %USERPROFILE%\.iot-hub-getting-started\config-raspberrypi.json

    # For macOS or Ubuntu
    code ~/.iot-hub-getting-started/config-raspberrypi.json
    ```

3. Vervangen `[Azure Storage connection string]` met de verbindingstekenreeks die u in stap 1 hebt gekregen.
4. Sla de `config-raspberrypi.json` bestand.
5. Berichten opnieuw verzenden en lezen van uw tabelopslag Azure door de volgende opdracht uit te voeren:

    ```bash
    gulp run --read-storage
    ```

    De logica voor het lezen van Azure tabelopslag bevindt zich in de `azure-table.js` bestand.

    ![gulp uitgevoerd--alleen opslag](media/iot-hub-raspberry-pi-lessons/lesson3/gulp_read_message.png)

## <a name="335-summary"></a>3.3.5-overzicht

U hebt met succes uw Pi verbonden met de hub IoT in de cloud en gebruikt de voorbeeldtoepassing blink apparaat-wolk-berichten te verzenden. U ook de app Azure-functie gebruikt voor het opslaan van binnenkomende berichten voor IoT hub aan uw Azure tabelopslag. U kunt op verplaatsen naar de volgende les over het wolk naar apparaat om berichten te verzenden vanaf uw hub-IoT aan uw Pi.

## <a name="next-steps"></a>Volgende stappen

[Les 4: Wolk naar apparaat verzenden](iot-hub-raspberry-pi-kit-node-lesson4-send-cloud-to-device-messages.md)
