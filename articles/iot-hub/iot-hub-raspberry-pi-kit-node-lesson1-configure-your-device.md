<properties
 pageTitle="Configureer het apparaat | Microsoft Azure"
 description="De frambozen Pi 3 voor het eerste gebruik configureren en installeren van het besturingssysteem Raspbian, een gratis besturingssysteem dat is geoptimaliseerd voor de frambozen Pi hardware."
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

# <a name="11-configure-your-device"></a>1.1 het apparaat configureren

## <a name="111-what-you-will-do"></a>1.1.1 wat u doet

Configureren uw Pi voor het eerste gebruik en het besturingssysteem Raspbian, een gratis besturingssysteem dat is geoptimaliseerd voor de frambozen Pi hardware installeren. Als u aan eventuele problemen, oplossingen in de [pagina voor probleemoplossing met](iot-hub-raspberry-pi-kit-node-troubleshooting.md)zoeken.

## <a name="112-what-you-will-learn"></a>1.1.2 Wat leert u

In dit gedeelte wordt beschreven:

- Raspbian installeren op uw Pi
- Het opstarten van uw Pi met behulp van een USB-kabel
- Uw Pi aansluiten op het netwerk via een Ethernet-kabel of een Wi-Fi
- Het toevoegen van een LED aan het breadboard en verbinden met uw Pi

## <a name="113-what-you-need"></a>1.1.3 wat u nodig hebt

Om deze sectie hebt uitgevoerd, moet u de volgende onderdelen van het startpakket frambozen Pi 3:

- De Raad van frambozen Pi 3
- De 16GB MicroSD-kaart
- Van de 5V 2A voeding met de zes voet micro USB-kabel
- Het breadboard
- Connector kabels
- Een 560 Ohm weerstand
- Diffuus 10mm LED
- De Ethernet-kabel

![Dingen in de starterskit](media/iot-hub-raspberry-pi-lessons/lesson1/starter_kit.jpg)

Moet u ook:

- Een bekabelde of draadloze verbinding voor uw Pi verbinding maken met
- Een USB-SD adapter of mini-SD-kaart voor het branden van de installatiekopie van het besturingssysteem in de MicroSD-kaart.
- Een computer met Windows, Mac of Linux. De computer wordt gebruikt om Raspbian installeren op de MicroSD-kaart.
- Een Internet-verbinding voor het downloaden van de benodigde hulpprogramma's en software

## <a name="114-install-raspbian-on-the-microsd-card"></a>1.1.4 Raspbian installeren op de MicroSD-kaart

Bereid de MicroSD-kaart de afbeelding Raspbian te schrijven.

1. Raspbian downloaden.
  1. [Download](https://www.raspberrypi.org/downloads/raspbian/) het zip-bestand voor Raspbian Jessie met Pixel.
  2. De afbeelding Raspbian uitpakken naar een map op uw computer.
2. Raspbian installeren naar de MicroSD-kaart.
  1. [Download](https://www.etcher.io) en installeer het hulpprogramma Etcher SD-kaart-brander.
  2. Etcher uitvoeren en selecteer de Raspbian-afbeelding die u hebt uitgepakt in stap 1.
  3. Selecteer het station MicroSD-kaart.
    Opmerking: Etcher kan al hebt geselecteerd het juiste station.
  4. Klik op Flash Raspbian installeren naar de MicroSD-kaart.
  5. De MicroSD-kaart verwijderen van uw computer als het eenmaal is voltooid.
    Opmerking: Het is veilig voor het verwijderen van de MicroSD-kaart direct omdat Etcher automatisch uitwerpen of ontkoppelt van de kaart MicroSD na voltooiing.
  6. Plaats de MicroSD-kaart in uw Pi.

![Plaats de SD-kaart](media/iot-hub-raspberry-pi-lessons/lesson1/insert_sdcard.jpg)

## <a name="115-power-on-your-pi"></a>1.1.5 zet uw Pi

Zet uw Pi met behulp van de micro USB-kabel en de voeding.

![Inschakelen](media/iot-hub-raspberry-pi-lessons/lesson1/micro_usb_power_on.jpg)

> [AZURE.NOTE] Het is belangrijk dat de voeding gebruiken in de kit die ten minste 2 bis om ervoor te zorgen dat uw frambozen met voldoende goed is ingevoerd.

## <a name="116-connect-your-raspberry-pi-3-to-the-network"></a>1.1.6 uw Pi frambozen 3 verbinding met het netwerk

U kunt uw Pi verbinden met een bekabeld netwerk of een draadloos netwerk. Zorg ervoor dat uw Pi is aangesloten op hetzelfde netwerk bevinden als uw computer. Zo kunt u uw Pi met dezelfde switch die op uw computer is aangesloten.

### <a name="1161-connect-to-a-wired-network"></a>1.1.6.1 verbinding maakt met een bekabeld netwerk

Uw Pi verbinden met het bekabelde netwerk via de Ethernet-kabel. De twee LED's op de Pi weer inschakelen als de verbinding tot stand is gebracht.

![Verbinding maken via Ethernet-kabel](media/iot-hub-raspberry-pi-lessons/lesson1/connect_ethernet.jpg)

### <a name="1162-connect-to-a-wireless-network"></a>1.1.6.2 verbinding maken met een draadloos netwerk

Volg de [instructies](https://www.raspberrypi.org/learning/software-guide/wifi/) van de Stichting frambozen Pi uw Pi verbinding met uw draadloze netwerk. Deze instructies moeten u eerst verbinding maken met een monitor en toetsenbord uw Pi.

## <a name="117-connect-the-led-to-your-pi"></a>1.1.7 LED verbinding met uw Pi

Gebruik het [breadboard](https://learn.sparkfun.com/tutorials/how-to-use-a-breadboard), de draden van de connector, de LED en de weerstand om deze taak. U aansluit deze op de [algemene input/output](https://www.raspberrypi.org/documentation/usage/gpio/) (GPIO)-poorten van de Pi. 

![Breadboard, LED en weerstand](media/iot-hub-raspberry-pi-lessons/lesson1/breadboard_led_resistor.jpg)

1. Verbinding maken met de kortere zijde van LED **GPIO GND (Pin 6)**.
2. Verbinding maken met een arm van de weerstand van de langere zijde van de LED.
3. Verbinding maken met de andere zijde van de weerstand **GPIO 4 (Pin 7)**.

Houd er rekening mee dat de polariteit van LED belangrijk is. Deze instelling polariteit bekend staat als de actieve laag.

![Pin-out](media/iot-hub-raspberry-pi-lessons/lesson1/pinout_breadboard.png)

Gefeliciteerd! U hebt uw Pi geconfigureerd.

## <a name="118-summary"></a>1.1.8 overzicht

In deze sectie kunt hebt u het configureren van uw Pi met Raspbian installeren en aansluiten van een LED op de Pi uw Pi te verbinden met een netwerk geleerd. Houd er rekening mee dat de LED nog niet oplichten. In de volgende sectie installeert u de vereiste hulpprogramma's en software in de voorbereiding voor het uitvoeren van een voorbeeldtoepassing op uw Pi.

![Hardware is gereed](media/iot-hub-raspberry-pi-lessons/lesson1/hardware_ready.jpg)

## <a name="next-steps"></a>Volgende stappen

[1.2 de hulpprogramma's ophalen](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-win32.md)
