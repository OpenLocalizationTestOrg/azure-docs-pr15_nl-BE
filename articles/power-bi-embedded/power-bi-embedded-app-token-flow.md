<properties
   pageTitle="Verificatie en autorisatie met Power BI ingesloten"
   description="Verificatie en autorisatie met Power BI ingesloten"
   services="power-bi-embedded"
   documentationCenter=""
   authors="guyinacube"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="10/04/2016"
   ms.author="asaxton"/>

# <a name="authenticating-and-authorizing-with-power-bi-embedded"></a>Verificatie en autorisatie met Power BI ingesloten

Power BI ingesloten gebruikt de service **sleutels** en **App Tokens** voor verificatie en autorisatie in plaats van expliciete eindgebruiker verificatie. In dit model beheert de toepassing verificatie en machtiging van uw eindgebruikers. Wanneer dat nodig is, wordt uw app maakt en verzendt de Tokens App waarin onze service om het gewenste rapport weer te geven. Dit ontwerp nodig niet uw app Azure Active Directory gebruiken voor verificatie en autorisatie, maar u nog steeds kunt.

## <a name="two-ways-to-authenticate"></a>Twee manieren om te verifiëren

**Sleutel** - kunt u toetsen gebruiken voor alle Power BI ingesloten REST API-aanroepen. De toetsen vindt u in de **Azure portal** door te klikken op **alle instellingen** en klik vervolgens op **Access-toetsen**. Uw sleutel altijd behandelen alsof het een wachtwoord. Deze sleutels hebben machtigingen voor het maken van REST API's aanroepen voor een bepaalde werkruimte-collectie.

Voor informatie over het gebruik van een sleutel voor een oproep voor de REST toevoegen de volgende authorization-header:            

    Authorization: AppKey {your key}

**App token** - verificatietokens worden gebruikt voor alle aanvragen voor insluiting App. Zij geschikt zijn om te worden uitgevoerd aan de clientzijde, zodat zij tot één rapport beperkt zich en het is raadzaam een verlooptijd instellen.

App-tokens zijn JWT (JSON Web Token) is ondertekend door een van de sleutels.

Je token app kan de volgende claims bevatten:

| Claim      | Beschrijving        |
|--------------|------------|
| **ver**      | De versie van de app-token. 0.2.0 is de huidige versie.       |
| **AUD**      | De geadresseerde van het token. Voor Power BI ingesloten: 'https://analysis.windows.net/powerbi/api'.  |
| **ISS**      |  Een tekenreeks die aangeeft van de toepassing die het token heeft afgegeven.    |
| **type**     | Het type van app-token dat wordt gemaakt. Huidige de enige ondersteunde type is **ingesloten**.   |
| **draadloze**      | Collectienaam werkruimte het token wordt uitgegeven.  |
| **WID**      | Werkruimte-ID het token wordt uitgegeven.  |
| **verwijderen**      | Lijst-ID van het token wordt uitgegeven.     |
| **gebruikersnaam** (optioneel) |  Met RLS wordt gebruikt, is dit een tekenreeks waarmee de gebruiker te identificeren bij het toepassen van regels voor RLS. |
| **rollen** (optioneel)   |   Een tekenreeks met de rollen die bij de toepassing van de regels voor de beveiliging op rij selecteren. Als meer dan één functie doorgeeft, moet het worden doorgegeven als een String-matrix.    |
| **EXP** (optioneel)    |   Geeft de tijd waarin het token zal verlopen. Deze moeten worden doorgegeven als de tijdstempels voor Unix.   |
| **NBF** (optioneel)    |   Geeft de tijd waarin het token begint geldig. Deze moeten worden doorgegeven als de tijdstempels voor Unix.   |

Een token monster app ziet er zo uit:

![](media\power-bi-embedded-app-token-flow\power-bi-embedded-app-token-flow-sample-coded.png)


Gedecodeerd, wordt er uitzien als volgt:

![](media\power-bi-embedded-app-token-flow\power-bi-embedded-app-token-flow-sample-decoded.png)


## <a name="heres-how-the-flow-works"></a>Hier wordt de werking van de stroom

1. De API-toetsen aan uw toepassing kopiëren. U kunt de toetsen in **Azure Portal**.

    ![](media\powerbi-embedded-get-started-sample\azure-portal.png)

2. Token een claim wordt gesteld en heeft een verlooptijd.

    ![](media\powerbi-embedded-get-started-sample\power-bi-embedded-token-2.png)

3. Token wordt ondertekend met een API toegangstoetsen.

    ![](media\powerbi-embedded-get-started-sample\power-bi-embedded-token-3.png)

4. Verzoeken om een rapport weer te geven.

    ![](media\powerbi-embedded-get-started-sample\power-bi-embedded-token-4.png)

5.  Token is geverifieerd met een API toegangstoetsen.

    ![](media\powerbi-embedded-get-started-sample\power-bi-embedded-token-5.png)

6.  Power BI ingesloten is een rapport verzendt naar een gebruiker.

    ![](media\powerbi-embedded-get-started-sample\power-bi-embedded-token-6.png)

Nadat de **Power BI ingesloten** is, wordt een rapport naar de gebruiker verzonden, kan de gebruiker het rapport weergeven in uw aangepaste app. Bijvoorbeeld, als u het [PBIX van verkoop-gegevens analyseren monster](http://download.microsoft.com/download/1/4/E/14EDED28-6C58-4055-A65C-23B4DA81C4DE/Analyzing_Sales_Data.pbix)hebt geïmporteerd, eruit de steekproef web app als volgt:

![](media\powerbi-embedded-get-started-sample\sample-web-app.png)

## <a name="see-also"></a>Zie ook
- [Aan de slag met Microsoft Power BI ingesloten voorbeeld](power-bi-embedded-get-started-sample.md)
- [Algemene scenario's voor Power BI ingesloten](power-bi-embedded-scenarios.md)
- [Aan de slag met Microsoft Power BI ingesloten](power-bi-embedded-get-started.md)
