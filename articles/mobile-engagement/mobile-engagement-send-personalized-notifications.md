<properties 
    pageTitle="Persoonlijk bericht met Azure Mobile Engagement verzenden" 
    description="Het verzenden van persoonlijke meldingen door informatie over gebruikersprofielen in de meldingen als hun namen"        
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="all" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="personalize-notifications-by-including-user-name"></a>Meldingen aanpassen door de naam van gebruiker

In uw zoektocht naar meldingen meer aantrekkelijker te maken voor de gebruikers van de app, moet u overwegen deze aanpassen aan uw wensen. Een krachtige aanpak bestaat uit de namen van de gebruikers app selectief te gebruiken om de meldingen zodat ze meer persoonlijke. Waarschuwing hier - u moet benadert gebruikersnamen toevoegen aan de berichten zorgvuldig omdat als u overmatig gebruik van deze strategie vervolgens deze als griezelig voor app gebruikers tegenkomt kan. Ook moet u zorgen dat u de gebruiker laat te voorzien van deze persoonlijke gegevens aan u volledige toestemming voor het in de mobiele app voordat u begint met deze opt-in. 

Technisch gesproken met Azure Mobile Engagement, kunt u uitvoeren de meldingen aanpassen door de onderstaande stappen uit om in die we het scenario gebruiken van de gebruikersnaam in de kennisgevingen. U gebruikt het concept van App-Info of Tags kunnen ofwel waarvan de waarden worden doorgegeven door de SDK's worden geïntegreerd in de mobiele App of via de API's. Deze App Infos of Tags kunnen vervolgens worden gebruikt:

1. voor het afstemmen van meldingen aan specifieke gebruikers op basis van de waarden van de App-Info of 
2. Als tijdelijke aanduidingen in de kennisgevingen die worden vervangen door waarden die specifiek zijn voor het apparaat/de gebruiker tijdens het verzenden van meldingen aan dat apparaat. 

> [AZURE.IMPORTANT] Let op: de snelheid van het verzenden van meldingen wordt een vermindering vanwege deze aanvullende verwerking van het app-info waarden vervangen door elke meldingen. 

##<a name="register-app-info-in-the-mobile-engagement-portal"></a>App-Info registreren in de betrokkenheid bij de mobiele Portal

1) U begint met het registreren van App Info of codes in de Azure portal. Ga naar **Instellingen** -> **(App Info) code** voor dit.  

![][1]  

2) Klik op het **nieuwe label (app info)** en de naam als de *gebruikersnaam* en het type *tekenreeks* en klik op **verzenden**. 

![][2]

3) Ten slotte ziet u dit app-info geregistreerd als volgt uit:

![][3]

##<a name="send-app-info-from-the-client-sdk"></a>App-Info verzenden vanaf de client-SDK

Hier gebruiken we het voorbeeld van de universele Windows app maar gelijkwaardige methoden voor onze andere SDK's ook. 

Ervan uitgaande dat er een methode in de mobiele app waar u de profielgegevens van de gebruiker als de namen waarschijnlijk nadat ze daarbij wilt verifiëren, roept u `SendAppInfo` methode hier en vullen de waarde van de `user_name` app info die eerder in de backend betrokkenheid bij de mobiele service geregistreerd. 

    Dictionary<object, object> appInfo = new Dictionary<object, object>();
    appInfo.Add("user_name", str);
    EngagementAgent.Instance.SendAppInfo(appInfo); 

##<a name="send-personalized-notifications"></a>Persoonlijke meldingen verzenden

U bent nu alle instellen voor het verzenden van berichten met deze **gebruikersnaam**. 

1) Ga naar Mobile Engagement Portal op het tabblad **bereiken** voor het maken van een melding en kunt u deze tijdelijke aanduiding in de volgende notatie ergens in de titel van het bericht of de instantie. 

![][4]  

> [AZURE.NOTE] Gebruikers die de app-info gebruikersnaam niet is ingesteld, krijgt geen melding. Als u de melding campagne in de testmodus en hebt u geen app-info instellen en vervolgens we sturen "?" teken ter vervanging van de tijdelijke aanduiding. 

2) Mobile Engagement selecteert wanneer een apparaat voor het verzenden van dit bericht en vervolgens het bekijken dit app-info en vervang de waarde in de tijdelijke aanduiding.  
Stel dat we hebben ingesteld `str = "Scott"` voor een gebruiker dan het apparaat registratie krijgt gekoppeld aan de gegevens van de app van **gebruikersnaam = SCOTT** voor deze gebruiker en deze gebruiker een push-bericht app in de volgende indeling buiten zien. 

![][5]  

<!-- Images. -->
[1]: ./media/mobile-engagement-send-personalized-notifications/app-info.png
[2]: ./media/mobile-engagement-send-personalized-notifications/create-app-info.png
[3]: ./media/mobile-engagement-send-personalized-notifications/app-info-user-name.png
[4]: ./media/mobile-engagement-send-personalized-notifications/personal-notification.png
[5]: ./media/mobile-engagement-send-personalized-notifications/notification.png

