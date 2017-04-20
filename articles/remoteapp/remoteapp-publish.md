<properties
    pageTitle="Het publiceren van een app in Azure RemoteApp | Microsoft Azure"
    description="Informatie over het publiceren van toepassingen en bronnen in Azure RemoteApp."
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />


# <a name="how-to-publish-an-app-in-remoteapp"></a>Het publiceren van een app in RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt stopgezet. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

Nadat u uw verzameling RemoteApp maakt, moet u de apps of bronnen die u beschikbaar wilt maken voor uw gebruikers publiceren. De sjabloon-afbeeldingen die bij uw abonnement hebben alleen een paar apps - standaard voor het delen van de andere apps gepubliceerd, moet u deze publiceren.

> [AZURE.NOTE] Hebt u nodig voor het bijwerken van een app? U moet voor het [bijwerken van de afbeelding](remoteapp-update.md) eerst.

Klik op het tabblad **publiceren** in de portal voor **publiceren**. Het toevoegen van een app uit het menu **Start** van de Sjabloonafbeelding van de of het pad naar waar de app is geïnstalleerd op de Sjabloonafbeelding. Als u vanuit het menu **Start** toevoegen, kiest u de app publiceren in de lijst. Als u het pad naar de app, voer een naam in voor het pad en de app App. Met de variabelen in het pad - bijvoorbeeld "% systeemstation %" in plaats van "c:\".

> [AZURE.NOTE] Als u uw app uit het menu **Start** toevoegen wilt, moet u *toegevoegd die app aan de * *Start* * menu op de Sjabloonafbeelding van uw.* Anders RemoteApp ziet alleen wat *is* in het menu **Start** en u zal worden verward. 

>Plaats een snelkoppelingsbestand - **lnk** - in de map %systemdrive%\ProgramData\Microsoft\Windows\Start Start\Programma 's om te controleren of dat uw app is in het menu **Start** .

> Als u bent vergeten de app toevoegen aan het menu **Start** wanneer u de sjabloon hebt gemaakt, kiest u toevoegen het pad naar de toepassing. (Of de Sjabloonafbeelding opnieuw te maken, maar dat is heel iets meer werk.)


 