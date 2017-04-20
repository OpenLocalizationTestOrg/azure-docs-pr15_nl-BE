<properties 
    pageTitle="Problemen met Analytics - de krachtige zoekfunctie van toepassing inzichten | Microsoft Azure" 
    description="Problemen met de toepassing inzichten analytics? Start hier. " 
    services="application-insights" 
    documentationCenter=""
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/11/2016" 
    ms.author="awills"/>


# <a name="troubleshoot-analytics-in-application-insights"></a>Problemen oplossen met Analytics in inzichten van toepassing


Problemen met de [toepassing inzichten Analytics](app-insights-analytics.md)? Start hier. Analytics is de krachtige zoekfunctie van Visual Studio-toepassing inzichten.



## <a name="limits"></a>Limieten

* Queryresultaten zijn momenteel beperkt tot alleen gedurende een week van oude gegevens.
* We testen op browsers: meest recente edities van chroom, rand en Internet Explorer.


## <a name="known-incompatible-browser-extensions"></a>Bekende niet-compatibele browser-extensies

* Ghostery

De extensie uitschakelen of een andere browser gebruikt.


##<a name="e-a"></a>'Onverwachte fout'

![Onverwachte fout-scherm](./media/app-insights-analytics-troubleshooting/010.png)

Er is een interne fout opgetreden tijdens runtime portal – niet-verwerkte uitzondering.

* Reinig de cache van de browser. 

## <a name="e-b"></a>403... Probeer het opnieuw laden

![403... Probeer het opnieuw laden](./media/app-insights-analytics-troubleshooting/020.png)

Een verificatie gerelateerd is een fout opgetreden (tijdens de verificatie of tijdens het genereren van access token). De portal kan er niet kan herstellen zonder het wijzigen van browserinstellingen.

* Controleer of [indirecte cookies zijn ingeschakeld](#cookies) in de browser. 


## <a name="authentication"></a>403... beveiligingszone controleren

![403.. .verify beveiligingszone](./media/app-insights-analytics-troubleshooting/030.png)

Een verificatie gerelateerd is een fout opgetreden (tijdens de verificatie of tijdens het genereren van access token). De portal kan er niet kan herstellen zonder het wijzigen van browserinstellingen.

1. Controleer of [indirecte cookies zijn ingeschakeld](#cookies) in de browser. 

2. Hebt u een favoriet, bladwijzer of koppeling opgeslagen gebruikt voor het openen van het portaal Analytics? Bent u aangemeld bij andere referenties dan u gebruikt wanneer u de koppeling opgeslagen?

2. Probeer een browservenster in-particuliere/incognito (na het sluiten van alle vensters). U hebt uw referenties op te geven. 

2. Een andere (gewone) venster geopend en Ga naar [Azure](https://portal.azure.com). Afmelden. Open uw link en meld u aan met de juiste referenties.

2. Rand en Internet Explorer-gebruikers kunnen deze fout ook ophalen als vertrouwde zone-instellingen worden niet ondersteund.

    Controleer of u zowel [Analytics portal](https://analytics.applicationinsights.io) en [portal Azure Active Directory](https://portal.azure.com) in de dezelfde zone:

 * Open **Internet-opties**, **beveiliging**, **Vertrouwde sites**, **Sites**in Internet Explorer:

    ![Dialoogvenster Internet-opties, een site aan vertrouwde websites toe te voegen.](./media/app-insights-analytics-troubleshooting/033.png)

    In de lijst Websites als een van de volgende URL's opgenomen zijn, zorg dat de andere zijn opgenomen:

    https://Analytics.applicationinsights.IO<br/>
   https://login.microsoftonline.com<br/>
   https://login.Windows.NET


## <a name="e-d"></a>404... Bron niet gevonden

![404... bron niet gevonden](./media/app-insights-analytics-troubleshooting/040.png)

Bron van toepassing is verwijderd uit de inzichten van de toepassing en niet meer beschikbaar. Dit kan gebeuren als u de URL naar de pagina Analytics opgeslagen.


## <a name="e-e"></a>403... Er wordt geen toelating

![403... niet toegestaan](./media/app-insights-analytics-troubleshooting/050.png)

U bent niet gemachtigd om deze toepassing te openen in Analytics.

* Kom je aan de koppeling van iemand anders? Vraag hen om ervoor te zorgen dat de [lezers of inzenders voor de resourcegroep](app-insights-resources-roles-access-control.md).
* U de koppeling met andere referenties opslaan? Open de [Azure portal](https://portal.azure.com), log uit en probeer deze koppeling weer de juiste referenties opgeeft.

## <a name="html-storage"></a>403... HTML5 opslag

Onze portal gebruikt HTML5, localStorage en sessionStorage.

* Chroom: Instellingen, privacy, instellingen van de inhoud.
* Internet Explorer: Internet-opties, tabblad Geavanceerd, beveiliging, DOM opslag inschakelen


![403... Probeer HTML5 opslag inschakelen](./media/app-insights-analytics-troubleshooting/060.png)

## <a name="e-g"></a>404... Abonnement is niet gevonden


![404... Abonnement is niet gevonden](./media/app-insights-analytics-troubleshooting/070.png)

De URL is ongeldig. 

* Open de bron app in de [portal Application inzichten](https://portal.azure.com). Klik de knop Analytics gebruiken.

## <a name="e-h"></a>... de 404 pagina niet bestaat.

![404... Pagina bestaat niet](./media/app-insights-analytics-troubleshooting/080.png)

De URL is ongeldig.

* Open de bron app in de [portal Application inzichten](https://portal.azure.com). Klik de knop Analytics gebruiken.

## <a name="cookies"></a>Cookies van derden inschakelen

  Zie [cookies van derden uitschakelen](http://www.digitalcitizen.life/how-disable-third-party-cookies-all-major-browsers), maar u ziet dat we ze nodig hebben om in te **schakelen** .

## <a name="e-x"></a>Als laatste redmiddel    

[Contact met ons opnemen](app-insights-get-dev-support.md).
 
[AZURE.INCLUDE [app-insights-analytics-footer](../../includes/app-insights-analytics-footer.md)]

