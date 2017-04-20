<properties
    pageTitle="Azure Mobile Engagement demo app | Microsoft Azure"
    description="Hier wordt beschreven waar u kunt downloaden en het gebruik van de voordelen van Azure Mobile Engagement demo app"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/10/2016"
    ms.author="piyushjo" />

# <a name="azure-mobile-engagement-demo-app"></a>Azure Mobile Engagement demo app

We hebben een Azure Mobile Engagement demo app voor **iOS**, **Android**en **Windows** -platforms om te zoeken naar nuttige bronnen en meer informatie over Mobile Engagement gepubliceerd.

De app kunt u:

- Nuttige koppelingen naar betrokkenheid bij de mobiele bronnen zoals video's, documentatie, het ondersteuningsforum en waar u wilt verhogen, functie-aanvragen gemakkelijk vinden.
- Ervaring monster meldingen die worden ondersteund door Mobile Engagement voor ideeën voor uw eigen mobiele toepassingen.
- Een referentie-implementatie gebruiken voor de analyse van het implementeren van Mobile Engagement in uw eigen app. U leert:

    - Analytics-gegevens verzamelen.
    - Melding voor geavanceerde scenario's bestandstypen zoals *verspreide volledig scherm* of *pop-*implementeren.
    - Implementeer enquêtes en polls.
    - Gegevens en push silent push-scenario's implementeert.   

## <a name="app-installation"></a>App-installatie
Dit app is beschikbaar in de volgende app-winkels:

- **Universele Windows demo app**:

    - Download de app in de [Windows App slaan](https://www.microsoft.com/en-us/store/apps/azure-mobile-engagement/9nblggh4qmh2).
    - De app is ontwikkeld als een Windows 10 Universal app. De broncode is beschikbaar op [GitHub](https://github.com/Azure/azure-mobile-engagement-app-windows).

- **iOS app demo**:

    - Download de app in de [opslag van Apple](https://itunes.apple.com/us/app/azure%20mobile%20engagement/id1105090090).
    - De app is ontwikkeld in iOS Swift. De broncode is beschikbaar op [GitHub](https://github.com/Azure/azure-mobile-engagement-app-ios).

- **Demo van android app**:

    - Download de app op [Google Play opslaan](https://play.google.com/store/apps/details?id=com.microsoft.azure.engagement).
    - De broncode is beschikbaar op [GitHub](https://github.com/Azure/azure-mobile-engagement-app-android).

![Universele Windows app voor demo][1]

![iOS app demo][2]
![demo Android app][3]


## <a name="usage"></a>Gebruik

U kunt deze toepassing op de volgende manieren gebruiken:

**Download de app op uw apparaat uit de toepassing opgeslagen koppelingen (eerder):**

>[AZURE.IMPORTANT] U hoeft niet een Azure account of de noodzaak de app verbinding met een back-end. De app werkt onafhankelijk van elkaar.

- Nadat u de app op uw apparaat hebt, kunt u de koppelingen in het menu links naar nuttige informatiebronnen over Mobile Engagement doorlopen.
- We hebt de [RSS-feed van de service](https://aka.ms/azmerssfeed) in deze toepassing toegevoegd, zodat u altijd over de meest recente productupdates worden bijgewerkt.
- U kunt ook de melding voorbeeldscenario's genieten van het type van de meldingen die worden ondersteund door Mobile Engagement voor elk platform doorlopen. Deze meldingen kunnen worden ervaren lokaal--dat is, kunt u op de knoppen op het scherm zodat u de meldingen-ervaring gelijk is aan het verzenden van de meldingen van de betrokkenheid bij de mobiele platform.

![App menu voor Windows][4]

![Menu voor iOS App][5]
![menu App voor Android][6]

**Download de broncode van de GitHub koppelingen (eerder):**

- Nadat u de code hebt gedownload, opent u het in de respectieve ontwikkelomgeving--XCode voor iOS, Android Studio voor Android en Visual Studio voor Windows.
- Vervolgens moet u onze [SDK integratie basisstappen](mobile-engagement-windows-store-dotnet-get-started.md) volgen zodat u verbinding maken met deze toepassing een eigen exemplaar van de back-end Mobile Engagement.
    - U moet een verbindingstekenreeks configureren in de app.
    - U moet ook de push notification platform voor uw app configureren.
- U zult merken dat de app zelf is geïnstrumenteerd met Mobile Engagement. Dus als u de app opent na het maken van verbinding met de back-end, zult u zien de gebruikerssessie, activiteiten, gebeurtenissen, enzovoort, op het tabblad **Monitor** .
- Ook zult u berichten wilt verzenden naar deze toepassing vanuit uw eigen exemplaar van Mobile Engagement, in plaats van lokale berichten.
    - Hier kunt u uw apparaat als een testapparaat toevoegen met behulp van het menu-item met **de apparaat-ID te verkrijgen** in de app. Dit geeft u de apparaat-ID die u als een testapparaat met uw exemplaar van de back-end-platform registreert.

    ![Apparaat-ID van Windows][7]

    ![Op een iOS-apparaat-ID][8]
    ![op Android-apparaat-ID][9]

## <a name="key-features-of-the-demo-app"></a>Belangrijkste functies van de demo-app

- Zoals hierboven vermeld met dit app, hebt u de belangrijke bronnen voor Mobile Engagement in uw hand. U kunt gaan via de koppelingen in het menu links.

- Out van app-meldingen voor elk platform kunnen optreden. Deze meldingen kunnen worden bezorgd als **kennisgeving**, waar op de melding gewoon wordt geopend van een eigen scherm van de toepassing (met behulp van **diepe koppelen**)-- of een **aankondiging voor websites**, waar u extra HTML-inhoud kunt leveren vanuit de back-end Mobile Engagement moet worden weergegeven wanneer u op de melding klikt.

    ![Out van app-meldingen][29]

- U hebt op iOS, sluit de app om te zien de push-meldingen uit-app of het systeem. U kunt de uitvoering voor het toevoegen van **Actieknoppen**, zoals de knoppen die worden toegevoegd aan deze melding out-app voor *Feedback* en *delen* (zodat de gebruiker rechts van de melding zelf actie) bekijken.

    ![Meldingen van out-app op iOS][11] ![Out van app-meldingen weergeven op iOS][14]

- Op Android, de opties die worden ondersteund zijn toe te voegen met meerdere regels tekst (**Grote letters**) of van een kennisgeving (**Grote foto**) op de melding, samen met de **knoppen voor handelingen** (zoals ondersteund door iOS).

    ![Meldingen van out-app op Android][12] ![Out van app-meldingen weergeven op Android][15]

- Op Windows 10 ziet u hoe de meldingen bekijken op de PC. Dit bericht wordt ook weergegeven in het Windows 10 **Notification Center**. Er is geen ondersteuning voor het toevoegen van **Actieknoppen** op het moment dat in de Windows SDK.

    ![Out van app-meldingen in Windows][10] ![Out-app weergeven in Windows][13]

- Er kan standaard "in-app" meldingen voor elk platform. Dit is een ervaring van twee stappen waarbij **een venster** eerst weergegeven. Als u erop klikt, wordt geopend van een volledig scherm **aankondiging**, zoals weergegeven in de volgende schermafdruk. De inhoud van deze aankondiging afkomstig is van uw exemplaar van de back-end Mobile Engagement. De SDK bevat de sjablonen voor berichten en aankondigingen. U kunt gemakkelijk aanpassen, zoals in dit app demo met de toevoeging van het logo en de kleuren.  

    ![Meldingen van Windows-app][16]

    ![Meldingen op iOS-app][17]  ![Meldingen op de Android-app][18]

    **iOS**, **Android**

- U kunt ook de functie van de **categorie** van Mobile Engagement deze standaard aanpassen. In de demo-app hebben we twee algemene manier om te wijzigen van de ervaring van de kennisgevingen aangetoond. Houd er rekening mee dat de categorie-functie is nog niet ondersteund in de Windows SDK.

    **Volledig scherm verspreide:**

    ![Melding in app - verspreide categorie][30]

    ![Verspreide categorie op iOS][21]  ![Verspreide categorie op Android][22]

    **Pop-upbericht:**

    ![Melding in app - pop-categorie][31]

    ![Pop-upbericht op iOS][19]   ![Pop-upbericht op Android][20]

**iOS**, **Android**

- Mobile Engagement ondersteunt ook een speciale soort-app bericht **Polls**genoemd. Hiermee kunt u snel enquêtes gesegmenteerde app gebruikers verzenden. U kunt vragen en opties voor elke vraag zoals in de volgende schermafdruk toevoegen. Dit wordt vervolgens ophalen weergegeven als een in-app-melding aan de gebruiker van de app.   

    ![Poll-meldingen][32]

    ![Enquête onder Windows][26]

    ![Enquête over iOS][27]   ![Enquête op Android][28]

**iOS**, **Android**

- Mobile Engagement ondersteunt ook silent **Gegevens Push-** meldingen te verzenden. Met deze meldingen, kunt u gegevens verzenden van uw service (zoals de JSON-gegevens in het volgende voorbeeld), die u kunt verwerken in uw app en sommige actie ondernemen. Een voorbeeld is hoe we wijzigt de prijs van een artikel selectief met behulp van gegevens push-meldingen.

    ![Data push-bericht][33]

    ![Data push-bericht op Windows][23]

    ![Data push-bericht op iOS][24]  ![Data push-bericht op Android][25]

**iOS**, **Android**

> [AZURE.NOTE] U kunt gedetailleerde stapsgewijze instructies voor elk van deze meldingen door te klikken op **Klik hier voor instructies over het verzenden van deze meldingen van Mobile Engagement platform** op een kennisgeving monster scherm weergeven.


[1]: ./media/mobile-engagement-demo-apps/home-windows.png
[2]: ./media/mobile-engagement-demo-apps/home-ios.png
[3]: ./media/mobile-engagement-demo-apps/home-android.png
[4]: ./media/mobile-engagement-demo-apps/menu-windows.png
[5]: ./media/mobile-engagement-demo-apps/menu-ios.png
[6]: ./media/mobile-engagement-demo-apps/menu-android.png
[7]: ./media/mobile-engagement-demo-apps/device-id-windows.png
[8]: ./media/mobile-engagement-demo-apps/device-id-ios.png
[9]: ./media/mobile-engagement-demo-apps/device-id-android.png
[10]: ./media/mobile-engagement-demo-apps/out-of-app-windows.png
[11]: ./media/mobile-engagement-demo-apps/out-of-app-ios.png
[12]: ./media/mobile-engagement-demo-apps/out-of-app-android.png
[13]: ./media/mobile-engagement-demo-apps/out-of-app-display-windows.png
[14]: ./media/mobile-engagement-demo-apps/out-of-app-display-ios.png
[15]: ./media/mobile-engagement-demo-apps/out-of-app-display-android.png
[16]: ./media/mobile-engagement-demo-apps/in-app-windows.png
[17]: ./media/mobile-engagement-demo-apps/in-app-ios.png
[18]: ./media/mobile-engagement-demo-apps/in-app-android.png
[19]: ./media/mobile-engagement-demo-apps/pop-up-ios.png
[20]: ./media/mobile-engagement-demo-apps/pop-up-android.png
[21]: ./media/mobile-engagement-demo-apps/interstitial-ios.png
[22]: ./media/mobile-engagement-demo-apps/interstitial-android.png
[23]: ./media/mobile-engagement-demo-apps/data-push-windows.png
[24]: ./media/mobile-engagement-demo-apps/data-push-ios.png
[25]: ./media/mobile-engagement-demo-apps/data-push-android.png
[26]: ./media/mobile-engagement-demo-apps/survey-windows.png
[27]: ./media/mobile-engagement-demo-apps/survey-ios.png
[28]: ./media/mobile-engagement-demo-apps/survey-android.png
[29]: ./media/mobile-engagement-demo-apps/out-of-app.png
[30]: ./media/mobile-engagement-demo-apps/in-app-interstitial.png
[31]: ./media/mobile-engagement-demo-apps/in-app-pop-up.png
[32]: ./media/mobile-engagement-demo-apps/notification-poll.png
[33]: ./media/mobile-engagement-demo-apps/notification-data-push.png
