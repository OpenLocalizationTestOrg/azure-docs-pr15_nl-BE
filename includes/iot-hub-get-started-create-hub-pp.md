## <a name="create-a-device-management-enabled-iot-hub"></a>Maken van een apparaat beheer IoT Hub ingeschakeld

Omdat IoT Hub device management in voorvertoning, moet u een hub device management ingeschakeld IoT maken. Als Apparaatbeheer IoT Hub algemene beschikbaarheid bereikt, is in deze zelfstudie wordt bijgewerkt. De volgende stappen uit hoe u dit doet met de Azure portal.

1.  Log in om de [Azure portal].
2.  In de Jumpbar, klik op **Nieuw**en vervolgens klikt u op **Internet van dingen**en klik op **Azure IoT Hub**.

    ![][img-new-hub]

3.  Kies de configuratie voor uw IoT Hub in de blade **IoT Hub** .

    ![][img-configure-hub]

  -   Voer in het vak **naam** een naam voor uw IoT Hub. Als de **naam** geldig is en beschikbaar is, verschijnt er een groen vinkje in het vak **naam** .
  -   Selecteer een **laag van prijs- en schaal**. Deze zelfstudie is niet vereist voor een bepaalde laag.
  -   Een nieuwe resourcegroep maken in de **resourcegroep**, of Selecteer een bestaande. Zie [werken met resourcegroepen Azure resources beheren]voor meer informatie.
  -   Schakel het selectievakje in te **Schakelen Device Management - voorbeeld**.
  -   Selecteer de locatie voor het hosten van uw Hub-IoT op **locatie**. IoT Hub device management is alleen beschikbaar in Oost-VS, Noord-Europa en Azië tijdens de openbare preview.

    > [AZURE.NOTE]Als u niet het selectievakje voor het **Inschakelen van Apparaatbeheer**, werken de monsters niet.<br/>**Apparaatbeheer inschakelen**inschakelt, kunt u een voorbeeld IoT Hub alleen ondersteund in Oost-VS, Noord-Europa en Oost-Azië en niet bestemd voor productie scenario's maken. Apparaten kunnen niet worden gemigreerd naar en vanuit device management ingeschakeld hubs.

4.  Wanneer u ervoor uw IoT Hub configuratie-opties gekozen hebt, klikt u op **maken**. Het kan enkele minuten duren voordat Azure maken uw IoT Hub. U kunt de status controleren, kunt u de voortgang van de **Startboard** of in het deelvenster **meldingen** controleren.

    ![][img-monitor]

5.  Als de IoT Hub is gemaakt, wordt het blad voor de hub wordt automatisch geopend. Maak een notitie van de **host-naam**en klik vervolgens op **gedeelde-beleid**.

    ![][img-keys]

6.  Klik op het beleid **iothubowner** en vervolgens kopieert en Let op de verbindingsreeks in het blad **iothubowner** . Kopiëren naar een locatie die u later gebruiken kunt omdat u nodig hebt voor het voltooien van deze zelfstudie.

    > [AZURE.NOTE] Zorg dat u zich onthouden van het gebruik van de referenties van de **iothubowner** in productie scenario's.

    ![][img-connection]

U hebt nu een apparaat gemaakt management IoT Hub ingeschakeld. U moet de verbindingsreeks voor het voltooien van deze zelfstudie.

## <a name="create-a-device-identity"></a>Een apparaat-id maken

In dit gedeelte vindt u een knooppunt hulpmiddel [IoT Hub Explorer] [ iot-hub-explorer] voor het maken van een apparaat-id voor deze zelfstudie.

1. Voer de volgende in uw omgeving vanaf de opdrachtregel:

    npm installeren -giothub-explorer@latest

2. Voer de volgende opdracht om in te loggen op uw hub, onthouden vervangen door `{service connection string}` met de IoT Hub verbindingstekenreeks die u eerder hebt gekopieerd:

    iothub explorer aanmelding "{service verbindingsreeks}"

3. Tot slot maakt u een nieuw apparaat-id genoemd `myDeviceId` met de opdracht:

    iothub explorer maken myDeviceId--verbindingsreeks

Maak een notitie van de verbindingsreeks apparaat uit de lijst met resultaten. Deze verbindingsreeks wordt gebruikt door de app apparaat verbinding maken met uw IoT Hub als een apparaat.

![][img-identity]

Raadpleeg [aan de slag met IoT Hub] [ lnk-getstarted] voor een manier voor het maken van identiteiten apparaat via programmacode.

<!-- images and links -->
[img-new-hub]: media/iot-hub-get-started-create-hub-pp/image1.png
[img-configure-hub]: media/iot-hub-get-started-create-hub-pp/image2.png
[img-monitor]: media/iot-hub-get-started-create-hub-pp/image3.png
[img-keys]: media/iot-hub-get-started-create-hub-pp/image4.png
[img-connection]: media/iot-hub-get-started-create-hub-pp/image5.png
[img-identity]: media/iot-hub-get-started-create-hub-pp/devidentity.png

[Azure portal]: https://portal.azure.com/
[iot-hub-explorer]: https://github.com/Azure/azure-iot-sdks/tree/master/tools/iothub-explorer

[lnk-getstarted]: ../articles/iot-hub/iot-hub-csharp-csharp-getstarted.md
[Met behulp van de resourcegroepen Azure resources beheren]: ../articles/azure-portal/resource-group-portal.md
