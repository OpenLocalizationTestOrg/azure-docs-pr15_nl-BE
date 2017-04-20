## <a name="view-device-telemetry-in-the-dashboard"></a>Telemetrie van apparaat weergeven in het dashboard

Het dashboard in de externe monitoring oplossing kunt u de telemetrie dat uw apparaten IoT Hub verzenden weergeven.

1. Terug naar de externe monitoring oplossing dashboard, klik op **apparaten** in het deelvenster links om te navigeren naar de **lijst met apparaten**in uw browser.

2. In de **lijst met apparaten**ziet u dat de status van het apparaat is nu **actief**.

    ![][18]

3. Klik op **Dashboard** om terug te keren naar het dashboard, selecteert u het apparaat in het **apparaat naar de weergave** -omlaag weer te geven van de telemetrie. De telemetrie van de voorbeeldtoepassing is 50 eenheden voor interne temperatuur, 55 eenheden voor externe temperatuur en vochtigheid 50 eenheden. Houd er rekening mee dat standaard het dashboard wordt weergegeven alleen waarden van temperatuur en vochtigheid.

    ![][img-telemetry]

## <a name="send-a-command-to-your-device"></a>Een opdracht naar het apparaat verzonden

Het dashboard in de externe monitoring oplossing kunt u opdrachten verzenden naar uw apparaten via de IoT Hub. In de externe monitoring oplossing kunt u bijvoorbeeld een opdracht waarmee de inwendige temperatuur van een apparaat verzenden.

1. Klik op **apparaten** in het deelvenster links om te navigeren naar de **lijst met apparaten**in het dashboard externe oplossing voor controle.

2. Klik op **Apparaat-ID** voor het apparaat in de **lijst met apparaten**.

3. Klik op **opdrachten**in het deelvenster **details van het apparaat** .

    ![][13]

4. In de vervolgkeuzelijst **opdracht** **SetTemperature**Selecteer en voer vervolgens een nieuwe waarde voor de temperatuur in de **temperatuur** . Klik op de **opdracht** voor het verzenden van de opdracht naar het apparaat.

    ![][14]

    > [AZURE.NOTE] De opdrachtgeschiedenis bevat in eerste instantie de Opdrachtstatus **in behandeling**. Wanneer het apparaat de opdracht bevestigt, wordt de status gewijzigd in **voltooid**.

5. Op het dashboard, Controleer of het apparaat is nu verzenden 75 als de nieuwe waarde voor de temperatuur.

## <a name="next-steps"></a>Volgende stappen

Het artikel [aanpassen vooraf geconfigureerde oplossingen] [ lnk-customize] beschrijft enkele manieren waarop u kunt dit voorbeeld uitbreiden. Mogelijke uitbreidingen zijn met echte sensoren en implementatie van extra opdrachten.

U kunt meer informatie over de [machtigingen voor de site azureiotsuite.com][lnk-permissions].

[13]: ./media/iot-suite-visualize-connecting/suite4.png
[14]: ./media/iot-suite-visualize-connecting/suite7-1.png
[18]: ./media/iot-suite-visualize-connecting/suite10.png
[img-telemetry]: ./media/iot-suite-visualize-connecting/telemetry.png
[lnk-customize]: ../articles/iot-suite/iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-permissions]: ../articles/iot-suite/iot-suite-permissions.md
