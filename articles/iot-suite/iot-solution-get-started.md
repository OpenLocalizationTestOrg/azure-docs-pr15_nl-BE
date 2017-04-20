<properties
    pageTitle="Voorbeeld van de MyDriving Azure IoT: Quick start | Microsoft Azure"
    description="Aan de slag met een app die een uitgebreide demonstratie van het bouwen van een IoT-systeem met behulp van Microsoft Azure, met inbegrip van de Stream Analytics, Machine Learning en Hubs gebeurtenis is."
    services=""
    documentationCenter=".net"
    suite=""
    authors="harikmenon"
    manager="douge"/>

<tags
    ms.service="multiple"
    ms.workload="tbd"
    ms.tgt_pltfrm="ibiza"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="03/25/2016"
    ms.author="harikm"/>

# <a name="mydriving-iot-system-quick-start"></a>MyDriving IoT systeem: Quick start

MyDriving is een systeem waarop u het ontwerp en de implementatie van een typische [Internet van dingen](iot-suite-overview.md) (IoT) oplossing die telemetrie op apparaten verzamelt, verwerkt deze gegevens in de cloud en leren een adaptieve antwoord geven machine van toepassing is. Gegevens over uw auto trips, de demonstratie geregistreerd met behulp van de gegevens van uw mobiele telefoon en een adapter die u gegevens van het systeem van uw auto verzamelt. Deze gegevens wordt gebruikt om feedback te geven op de aangedreven stijl ten opzichte van andere gebruikers.

Het echte doel van MyDriving is om u op weg bij het maken van uw eigen oplossing IoT. Maar voordat u dat we u aan de slag met de MyDriving-app--als lid van ons team test gebruiker. U krijgt een ervaring van de app en het systeem achter als consument, voordat u dieper ingaan op de architectuur. Deze ook maakt u kennis met HockeyApp, een leuke manier van het beheer van de alfa en beta verdelingen van uw toepassingen te testen van gebruikers.

## <a name="use-the-mobile-experience"></a>Gebruik de mobiele ervaring

Als u een Android, iOS of Windows 10-apparaat hebt, kunt u de app MyDriving gebruiken.

### <a name="android-and-windows-10-mobile-installation"></a>Android en de installatie van Windows 10 Mobile

Op het apparaat:

1.  Ontwikkeling apps toestaan:

    -   Android: In de **Instellingen voor** > ,**beveiliging**, apps van **onbekende bronnen**toestaan.

    -   Windows 10: In de **Instellingen** > **Updates** > **Voor ontwikkelaars**, stel de **modus voor ontwikkelaars**.

2.  Deelnemen aan ons testteam van beta door met aanmelden of aanmelden bij [HockeyApp](https://rink.hockeyapp.net). HockeyApp kunt u eerdere versies van uw app testen gebruikers distribueren.

    Als u Windows 10, de Edge-browser gebruiken.

    Als u een genodigde Build 2016, meld u aan met hetzelfde Microsoft account e-mail u geregistreerd voor de Conferentie, met behulp van een van de Microsoft-knoppen. U bent al aangemeld bij HockeyApp.

    ![HockeyApp-in het scherm](./media/iot-solution-get-started/image1.png)

3.  Download en installeer de app vanaf hier:

    -   [Android](http://rink.io/spMyDrivingAndroid)

    -   [Windows 10](http://rink.io/spMyDrivingUWP)

    Er zijn twee items. Het certificaat installeren in **Vertrouwde personen**. Installeer de app.

*Problemen bij het starten van de app op Windows 10 Mobile?* Uw telefoon is mogelijk een update of twee achter. Zorg ervoor dat u de meest recente updates beschikt of installeren:

 - [Microsoft.NET.Native.Framework.1.2.appx](https://download.hockeyapp.net/packages/win10/Microsoft.NET.Native.Framework.1.2.appx) 

 - [Microsoft.NET.Native.Runtime.1.1.appx](https://download.hockeyapp.net/packages/win10/Microsoft.NET.Native.Runtime.1.1.appx) 

 - [Microsoft.VCLibs.ARM.14.00.appx](https://download.hockeyapp.net/packages/win10/Microsoft.VCLibs.ARM.14.00.appx)


### <a name="ios-installation"></a>iOS-installatie

Als u bijgewoond Build 2016, download de app als lid van ons testteam op HockeyApp:

1.  Log in op uw iOS-apparaat te [HockeyApp](https://rink.hockeyapp.net).
    Gebruik een van de Microsoft-in knoppen en meld u aan met hetzelfde Microsoft account e-mailbericht dat u hebt geregistreerd bij de conferentie. (Gebruik niet de velden e-mailadres en wachtwoord).

    ![HockeyApp-in het scherm](./media/iot-solution-get-started/image1.png)

2.  Selecteer MyDriving in het dashboard HockeyApp en download het.

3.  Toestaan dat de bèta-versie van HockeyApp:

    een. Ga naar **Instellingen** > **algemene** > **-profielen en Apparaatbeheer.**

    b. Vertrouw het certificaat **Bits stadion GmbH** .

U kunt als u Build 2016 niet hebben bijgewoond, bouwen en implementeren van de app:

1.   De code downloaden [van GitHub].

2.   Bouwen en implementeren met [behulp van Xamarin].

In de [MyDriving Naslaggids](http://aka.ms/mydrivingdocs)vindt u meer details.

## <a name="get-an-obd-adapter-optional"></a>Ophalen van een OBD-adapter (optioneel)

Dit is het gedeelte dat dit een echt systeem van Internet van de dingen is! Kunt u de app zonder meer plezier met de echte is maar ze zijn niet duur.

Diagnostische boordsystemen (OBD) is de functie van uw auto die in de garage gebruikt voor het afstemmen van uw auto en oneven geluiden en waarschuwing lampen. Tenzij uw auto van geweldige antiquity, vindt u ergens een socket in de cabine, meestal achter een flap onder het dashboard. U kunt met de juiste connector maatstaven van de prestaties van de engine ophalen en bepaalde wijzigingen aanbrengen. Een OBD-connector kan goedkoop worden aangeschaft op de gebruikelijke plaatsen. Verbinding wordt gemaakt met behulp van Bluetooth- of Wi-Fi met een app op je telefoon.

In dit geval gaan we verbinding maken met uw auto naar de cloud. De directe verbinding met het OBD-systeem is op uw telefoon, maar onze app werkt als een relay. Uw auto telemetrie verzonden rechtstreeks naar de hub MyDriving IoT waar deze wordt verwerkt om uw ritten registreren en beoordelen van de bestuurder stijl.

Verbinding maken met een OBD-apparaat:

1.  Controleer of uw auto een OBD-socket heeft.

2.  Verkrijgen van een OBD-adapter:

    -   Als u een mobiele telefoon met Android of Windows, moet u een Bluetooth-compatibele OBD-II-videoadapter. We [BAFX producten 34t5 Bluetooth-OBDII Scan Tool]gebruikt.

    -   Als u een mobiele telefoon met iOS gebruikt, moet u een adapter OBD-Wi-Fi-ondersteuning. We gebruiken [ScanTool OBDLink MX Wi-Fi: OBD-Adapter/diagnose Scanner].

3.  Volg de instructies die worden geleverd bij de OBD-adapter aan te sluiten op uw telefoon. Houd het volgende in gedachten:

    -   Een Bluetooth-adapter moet worden gekoppeld aan de telefoon, op de pagina **Instellingen** .

    -   Een Wi-Fi-adapter moet een adres in het bereik 192.168.xxx.xxx.

4.  Als u meerdere auto's hebt, kunt u afzonderlijke adapter krijgen voor elke (maximaal drie).

Als er geen een OBD-adapter, wordt de app verstuurd locatie en snelheid van de GPS-ontvanger van de telefoon naar de back-end en wordt u gevraagd of u wilt simuleren van een OBD-systeem.

U kunt meer informatie over hoe de app gegevens van de OBD-adapter gebruikt en opties voor het maken van uw eigen OBD-apparaat in punt 2.1, 'IoT apparaten,' in de [Naslaggids voor MyDriving](http://aka.ms/mydrivingdocs).

## <a name="use-the-app"></a>Gebruik de app.

Start de app. Er is een eerste Quickstart u stapsgewijs door hoe het werkt.

### <a name="track-your-trips"></a>Bijhouden van uw trips

Tik op de knop opnemen (grote rode cirkel onderin het scherm) om te beginnen een reis en tik nogmaals op om te beëindigen.

![Afbeelding van de knop opnemen voor bijhouden van reis](./media/iot-solution-get-started/image2.png)

Telkens wanneer die u een reis start, als er geen apparaat OBD-systeem, u gevraagd of u wilt de simulator gebruiken.

Tik op de knop stoppen en krijgt u een overzicht aan het einde van een reis.

![Voorbeeld van een reis-overzicht](./media/iot-solution-get-started/image3.png)

### <a name="review-your-trips"></a>Bekijk uw trips

![Voorbeeld van een laatste reis](./media/iot-solution-get-started/image4.png)

### <a name="review-your-profile"></a>Je profiel bekijken

![Voorbeeld van een profiel te rijden-stijl](./media/iot-solution-get-started/image5.png)

## <a name="send-us-your-test-feedback"></a>Stuur ons uw feedback test

Omdat wij MyDriving om aan de slag met uw eigen systemen IoT, willen we zeker horen over hoe goed het werkt. Laat het ons weten als:

- U ondervindt problemen of uitdagingen.

- Er is een uitbreidingspunt waardoor deze beter geschikt is voor uw scenario zou zijn.

- U vindt een efficiëntere manier voor het uitvoeren van bepaalde behoeften.

- Hebt u suggesties voor verbetering van MyDriving of deze documentatie.

Binnen de MyDriving app zelf, kunt u de ingebouwde HockeyApp feedback mechanisme: op iOS en Android, maar geeft uw telefoon een shake, of gebruikt u de opdracht **Feedback** . Dit koppelen een schermafbeelding, zodat we wat je praat weten je over automatisch. En als er een ongelukkige crashes, HockeyApp de crash logs om ons te laten over hen verzamelt. U kunt ook feedback via het [portal voor HockeyApp].

U kunt ook een [probleem op GitHub]bestand of een hieronder commentaar (en-us edition).

Wij hopen graag van u!

## <a name="next-steps"></a>Volgende stappen

-   Verken de [MyDriving referentie handleiding](http://aka.ms/mydrivingdocs) om te begrijpen hoe wij hebben ontworpen en gebouwd, het hele systeem van MyDriving.

-   [Maken en implementeren van een systeem van uw eigen](iot-solution-build-system.md) met behulp van onze scripts Azure Resource Manager. De [MyDriving Reference Guide](http://aka.ms/mydrivingdocs) begeleidt u door de gebieden waar u de meeste aanpassingen kunt aanbrengen.

  [van GitHub]: https://github.com/Azure-Samples/MyDriving
  [met behulp van Xamarin]: https://developer.xamarin.com/guides/ios/getting_started/installation/
  [BAFX producten 34t5 Bluetooth-OBDII Scan Tool]: http://www.amazon.com/gp/product/B005NLQAHS
  [ScanTool OBDLink MX Wi-Fi: OBD-Adapter/diagnose Scanner]: http://www.amazon.com/gp/product/B00OCYXTYY/ref=s9_simh_gw_g263_i1_r?pf_rd_m=ATVPDKIKX0DER&pf_rd_s=desktop-2&pf_rd_r=1MWRMKXK4KK9VYMJ44MP
  [HockeyApp-portal]: https://rink.hockeyapp.org
  [afgeven op GitHub]: https://github.com/Azure-Samples/MyDriving/issues
