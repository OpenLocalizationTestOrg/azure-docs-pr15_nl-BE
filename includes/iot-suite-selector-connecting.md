> [AZURE.SELECTOR]
- [C op Windows](../articles/iot-suite/iot-suite-connecting-devices.md)
- [C op Linux](../articles/iot-suite/iot-suite-connecting-devices-linux.md)
- [C op mbed](../articles/iot-suite/iot-suite-connecting-devices-mbed.md)
- [Node.js](../articles/iot-suite/iot-suite-connecting-devices-node.md)

## <a name="scenario-overview"></a>Scenario-overzicht

In dit scenario maakt u een apparaat dat de volgende telemetrie verzendt naar de externe controle [vooraf geconfigureerde oplossing][lnk-what-are-preconfig-solutions]:

- Externe temperatuur
- Inwendige temperatuur
- Vochtigheid

De code op het apparaat genereert voorbeeldwaarden voor eenvoud, maar raden we u aan het uitbreiden van het monster door echte sensoren aansluiten op het apparaat en het verzenden van echte telemetrie.

Als u deze zelfstudie hebt voltooid, moet u een actieve account Azure. Als u geen account hebt, kunt u een gratis proefperiode account in een paar minuten. Zie voor meer informatie, [Gratis proefperiode van Azure][lnk-free-trial].

## <a name="before-you-start"></a>Voordat u begint

Voordat u code voor uw apparaat schrijven, moet u uw externe controle vooraf geconfigureerde oplossing richt en richt een nieuwe aangepaste apparaat in deze oplossing.

### <a name="provision-your-remote-monitoring-preconfigured-solution"></a>Inrichten van uw externe controle vooraf geconfigureerde oplossing

Het apparaat u in deze zelfstudie maakt verzendt gegevens naar een exemplaar van de [externe controle] [ lnk-remote-monitoring] vooraf geconfigureerde oplossing. Als u nog niet in uw account Azure al en de externe controle vooraf geconfigureerde oplossing ingericht, de volgende stappen:

1. Klik op de pagina <https://www.azureiotsuite.com/> op **+** voor het maken van een nieuwe oplossing.

2. Klik op **selecteren** in het deelvenster **controle op afstand** om uw nieuwe oplossing te maken.

3. Voer een **naam voor oplossing** van uw keuze op de pagina **maken Remote monitoring oplossing** , selecteert u de **regio** die u implementeren wilt op en selecteer de Azure abonnement wilt gebruiken. Klik vervolgens op **Create oplossing**.

4. Wacht totdat het inrichtingsproces is voltooid.

> [AZURE.WARNING] De vooraf geconfigureerde oplossingen te factureren Azure services gebruiken. Zorg ervoor dat de vooraf geconfigureerde oplossing verwijderen uit uw abonnement als u klaar bent met het eventuele onnodige kosten te vermijden. U kunt een vooraf geconfigureerde oplossing volledig verwijderen uit uw abonnement via de pagina <https://www.azureiotsuite.com/> .

Wanneer het inrichtingsproces voor de oplossing voor externe controle is voltooid, klikt u op **starten** om het dashboard oplossing in uw browser openen.

![][img-dashboard]

### <a name="provision-your-device-in-the-remote-monitoring-solution"></a>Inrichten van uw apparaat in de oplossing voor externe controle

> [AZURE.NOTE] Als u al een apparaat in uw oplossing ingericht hebt, kunt u deze stap overslaan. U moet op de hoogte van de inloggegevens bij het maken van de clienttoepassing.

Voor een apparaat verbinding maken met de vooraf geconfigureerde oplossing moet het zichzelf identificeren met IoT Hub met geldige referenties. U kunt de inloggegevens ophalen uit het dashboard oplossing. De inloggegevens in de clienttoepassing verderop in deze zelfstudie te nemen. 

Als een nieuw apparaat toevoegen aan uw oplossing voor externe controle, de volgende stappen in het dashboard oplossing:

1.  Klik op **een apparaat toevoegen**in de linkerbenedenhoek van het dashboard.

    ![][1]

2.  Klik op **Nieuw toevoegen**in het deelvenster **Aangepaste apparaat** .

    ![][2]

3.  Kies **Ik wil mijn eigen apparaat-ID definiëren**, voert u de apparaat-ID zoals **mydevice**, klikt u op **ID controleren** om te controleren of dat deze naam nog niet in gebruik, en klik op **maken** om het apparaat te creëren.

    ![][3]

5. Noteer de inloggegevens (apparaat-ID, IoT Hub hostnaam en de sleutel apparaat), de clienttoepassing moet zij verbinding maken met de externe monitoring oplossing. Klik vervolgens op **Gereed**.

    ![][4]

6. Controleer of dat het apparaat wordt weergegeven in de sectie devices. Status van het apparaat is **in behandeling** totdat het apparaat een verbinding met de externe monitoring oplossing maakt.

    ![][5]

[img-dashboard]: ./media/iot-suite-selector-connecting/dashboard.png
[1]: ./media/iot-suite-selector-connecting/suite0.png
[2]: ./media/iot-suite-selector-connecting/suite1.png
[3]: ./media/iot-suite-selector-connecting/suite2.png
[4]: ./media/iot-suite-selector-connecting/suite3.png
[5]: ./media/iot-suite-selector-connecting/suite5.png

[lnk-what-are-preconfig-solutions]: ../articles/iot-suite/iot-suite-what-are-preconfigured-solutions.md
[lnk-remote-monitoring]: ../articles/iot-suite/iot-suite-remote-monitoring-sample-walkthrough.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/