<properties
    pageTitle="Het verzenden van meldingen geplande | Microsoft Azure"
    description="In dit onderwerp wordt beschreven hoe meldingen gepland met Azure melding Hubs."
    services="notification-hubs"
    documentationCenter=".net"
    keywords="push-meldingen, push-bericht, planning, push-meldingen"
    authors="ysxu"
    manager="erikre"
    editor=""/>
<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

# <a name="how-to-send-scheduled-notifications"></a>Procedure: Een geplande meldingen verzenden


##<a name="overview"></a>Overzicht

Als u een scenario waarin u een bericht verzenden op een bepaald moment in de toekomst, maar hoeft niet een gemakkelijke manier om uit de slaapstand halen om uw back-end-code om de melding te verzenden. Standaard laag melding Hubs ondersteunt een functie waarmee u meldingen van plannen tot 7 dagen in de toekomst.

Wanneer u een bericht verzendt, gebruikt de klasse [ScheduledNotification](https://msdn.microsoft.com/library/microsoft.azure.notificationhubs.schedulednotification.aspx) in de kennisgeving Hubs SDK zoals in het volgende voorbeeld:

    Notification notification = new AppleNotification("{\"aps\":{\"alert\":\"Happy birthday!\"}}");
    var scheduled = await hub.ScheduleNotificationAsync(notification, new DateTime(2014, 7, 19, 0, 0, 0));

U kunt ook een eerder geplande kennisgeving met behulp van de notificationId annuleren:

    await hub.CancelNotificationAsync(scheduled.ScheduledNotificationId);

Er zijn geen beperkingen op het aantal geplande meldingen die kunt u verzenden.