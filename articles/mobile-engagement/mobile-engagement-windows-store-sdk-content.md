<properties 
    pageTitle="Inhoud van de SDK van Windows Universal Apps" 
    description="Meer informatie over de inhoud van de SDK van Windows Universal Apps voor Azure Mobile Engagement"                    
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows-store" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="windows-universal-apps-sdk-content"></a>Inhoud van de SDK van Windows Universal Apps

Dit document geeft een overzicht en beschrijving van de inhoud door de SDK in uw toepassing is geïmplementeerd.

##<a name="the-resources-folder"></a>De `/Resources` map

Deze map bevat alle resources die Mobile Engagement nodig heeft. U kunt ook aanpassen aan uw app.

- `EngagementConfiguration.xml`: De betrokkenheid van de mobiele configuratiebestand, dit is waar u kunt Mobile Engagement instellingen (Mobile Engagement verbindingsreeks, rapport crash...).

### <a name="html-folder"></a>map/HTML

- `EngagementNotification.html`: De `Notification` web-view HTML-ontwerp voor spandoeken-app.

- `EngagementAnnouncement.html`: De `Announcement` webontwerp weergave html voor verspreide weergaven-app.

### <a name="images-folder"></a>de map/images

- `EngagementIconNotification.png`: Het merk pictogram dat wordt weergegeven aan de linkerkant van een melding, vervangen deze door het pictogram van uw merk.

- `EngagementIconOk.png`: De `Ok` pictogram van de inhoudspagina's bereiken voor de actie of de validatie knop.

- `EngagementIconNOK.png`: De `NOK` pictogram dat wordt gebruikt wanneer de validatie knop reach inhoudspagina's is uitgeschakeld.
 
- `EngagementIconClose.png`: De `Close` pictogram van de reach-meldingen en de inhoud voor de knop verwijderen klikt.

### <a name="overlay-folder"></a>map /overlay

- `EngagementPageOverlay.cs`: De overlay-pagina die verantwoordelijk is voor het toevoegen van de betrokkenheid bij de UI-app naar de onderliggende bereiken.
  
