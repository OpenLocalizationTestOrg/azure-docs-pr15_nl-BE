<properties 
   pageTitle="Debugging Azure Cloud Services | Microsoft Azure"
   description="Debugging Azure Cloud Services"
   services="visual-studio-online"
   documentationCenter="n/a"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags 
   ms.service="visual-studio-online"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="multiple"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="debugging-cloud-services"></a>Cloud services voor foutopsporing

U kunt verschillende benaderingen een Azure-toepassing met behulp van de hulpprogramma's voor Azure voor Microsoft Visual Studio en de SDK Azure debug:

- Kunt u fouten opsporen van de toepassing in Visual Studio Azure wanneer u het ontwerpt, net zoals u een Visual C# of Visual Basic-toepassing. Zie [fouten opsporen in uw cloud-service op de lokale computer](vs-azure-tools-debug-cloud-services-virtual-machines.md#debug-your-cloud-service-on-your-local-computer)voor meer informatie.

- Azure diagnostische gegevens kunt u gedetailleerde informatie van de code die wordt uitgevoerd binnen rollen, melden of de rollen worden uitgevoerd in de ontwikkelomgeving of in Azure. Zie voor meer informatie [verzamelen gegevens met behulp van Azure diagnostische gegevens vastleggen](http://go.microsoft.com/fwlink/p/?LinkId=400450).

- Als u Visual Studio Enterprise schrijven gericht op het .NET Framework 4 of het .NET Framework 4.5-functies gebruikt, kunt u IntelliTrace op het moment dat u vanuit Visual Studio een Azure cloud-service implementeert. IntelliTrace biedt een logboek dat u met Visual Studio gebruiken kunt voor foutopsporing van de toepassing als deze zijn uitgevoerd in Azure. Zie [Foutopsporing in een gepubliceerde cloud service met IntelliTrace en Visual Studio]( http://go.microsoft.com/fwlink/p/?LinkId=623016)voor meer informatie.

- U kunt foutopsporing op afstand inschakelen op uw cloud-services op het moment waarop het implementeren van de cloud-service vanuit Visual Studio. Als u foutopsporing op afstand voor een installatie inschakelen, worden externe debugging services geïnstalleerd op de virtuele machines die in elk exemplaar van de functie uitvoeren. Deze services, zoals msvsmon.exe, geen invloed op de prestaties en leiden tot extra kosten. Zie [Foutopsporing in Azure cloud service](vs-azure-tools-debug-cloud-services-virtual-machines.md#debug-a-cloud-service-in-azure)voor meer informatie.



