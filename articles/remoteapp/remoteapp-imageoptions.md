<properties
    pageTitle="Maak een afbeelding van Azure RemoteApp | Microsoft Azure"
    description="Meer informatie over de beschikbare opties voor het maken van installatiekopieën voor Azure RemoteApp"
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />



# <a name="create-an-azure-remoteapp-image"></a>Maak een afbeelding van Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt stopgezet. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

Azure RemoteApp wordt houdt de apps die u met gebruikers van uw deelt afbeeldingen. (Wij nemen uw image en VMs - dat is wat de gebruikerstoegang wanneer zij zich aanmelden in Azure RemoteApp maken.) Om een collectie Azure RemoteApp-toepassingen naar keuze cloud of hybride, begint u met het maken van een afbeelding met deze toepassingen geïnstalleerd. Apps publiceren naar gebruikers, maakt u een collectie met die afbeelding en gebruikers toewijzen aan de collectie.

U hebt verschillende opties voor het maken of afbeeldingen gebruiken. Een eerste [vereiste](remoteapp-imagereqs.md) voor een afbeelding is het uitvoeren van Windows Server 2012 R2 en de rol van Remote Desktop Session Host (RDSH) geïnstalleerd. Hoe krijgt u dat is waar de dingen interessant vinden.

Wanneer het gaat om de afbeeldingen hebt u de volgende opties:

- U kunt importeren en gebruiken van een [afbeelding op basis van een Azure virtuele machine](remoteapp-image-on-azurevm.md). Dit is goed voor line-of-business-toepassingen waarvoor aangepaste instellingen. U kunt de afbeelding voor de app aan het werk.
- U kunt [maken en een aangepaste afbeelding te uploaden](remoteapp-create-custom-image.md). Dit is goed als er al een afbeelding die u kunt voor de implementatie van de extern bureaublad-Services op locatie gebruiken.
- U kunt een van de [sjabloon-afbeeldingen](remoteapp-images.md) RemoteApp-abonnement. Deze afbeeldingen worden gemaakt en beheerd door het team van RemoteApp- en bevatten enkele standaard toepassingen (zoals de Office-suite) die u voor uw gebruikers maken kunt. Houd er rekening mee dat alleen de Office 365 Pro Plus de afbeelding kan worden gebruikt in een productie-omgeving.

Ongeacht waar u de afbeelding krijgen of hoe u deze hebt gemaakt, wilt u Zorg ervoor dat u bekend bent met de [eisen van de app](remoteapp-appreqs.md) om ervoor te zorgen dat uw app goed in RemoteApp werkt. De volgende stap is een verzameling [cloud](remoteapp-create-cloud-deployment.md) of [hybride](remoteapp-create-hybrid-deployment.md) maken.
