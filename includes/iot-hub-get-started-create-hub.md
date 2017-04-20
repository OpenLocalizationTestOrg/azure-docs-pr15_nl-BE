## <a name="create-an-iot-hub"></a>Maak een hub IoT

Een IoT hub voor het gesimuleerde apparaat om verbinding te maken. De volgende stappen uit hoe u deze taak uitvoeren met behulp van de portal Azure.

1. Aanmelden bij de [Azure portal][lnk-portal].

2. Klik op **Nieuw**in de Jumpbar, > **Internet van dingen** > **Azure IoT Hub**.

    ![Azure portal Jumpbar][1]

3. Kies de configuratie voor uw IoT hub in de blade **IoT hub** .

    ![IoT hub blade][2]

    * Voer in het vak **naam** een naam voor uw hub IoT. Als de **naam** geldig is en beschikbaar is, verschijnt er een groen vinkje in het vak **naam** .
    * Selecteer een [laag van prijs- en schaal][lnk-pricing]. Deze zelfstudie is niet vereist voor een bepaalde laag. Gebruik de gratis F1-laag voor deze zelfstudie.
    * Een nieuwe resourcegroep maken in de **resourcegroep**, of Selecteer een bestaande. Zie [werken met resourcegroepen Azure resources beheren]voor meer informatie,[lnk-resource-groups].
    * Selecteer de locatie voor het hosten van uw hub-IoT op **locatie**. Kies uw dichtstbijzijnde locatie voor deze zelfstudie.

4. Wanneer u ervoor uw hub IoT configuratie-opties gekozen hebt, klikt u op **maken**.  Het kan enkele minuten duren voordat Azure maken uw hub IoT. U kunt de status controleren, kunt u de voortgang van de Startboard of in het deelvenster meldingen controleren.

    ![Nieuwe IoT hub status][3]

5. Als de hub IoT is gemaakt, klikt u op de nieuwe tegel voor uw hub IoT in Azure portal voor het openen van het blad voor de nieuwe IoT-hub. Maak een notitie van de **host-naam**en klik vervolgens op **gedeelde-beleid**.

    ![Nieuwe IoT hub blade][4]

6. Klik op het beleid **iothubowner** en kopieer en Let op de verbindingsreeks in het blad **iothubowner** in het blad **Shared-beleid** . Zie [toegangsbeheer] voor meer informatie[ lnk-access-control] in de 'Azure IoT Hub developer guide.'

    ![Gedeelde toegang beleid blade][5]


<!-- Images. -->
[1]: ./media/iot-hub-get-started-create-hub/create-iot-hub1.png
[2]: ./media/iot-hub-get-started-create-hub/create-iot-hub2.png
[3]: ./media/iot-hub-get-started-create-hub/create-iot-hub3.png
[4]: ./media/iot-hub-get-started-create-hub/create-iot-hub4.png
[5]: ./media/iot-hub-get-started-create-hub/create-iot-hub5.png

<!-- Links -->
[lnk-resource-groups]: ../articles/azure-portal/resource-group-portal.md
[lnk-portal]: https://portal.azure.com/
[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub/
[lnk-access-control]: ../articles/iot-hub/iot-hub-devguide-security.md
