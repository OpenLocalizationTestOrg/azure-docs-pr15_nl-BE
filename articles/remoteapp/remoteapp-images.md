<properties
    pageTitle="Wat is in de sjabloon Azure RemoteApp-afbeeldingen? | Microsoft Azure"
    description="Meer informatie over de afbeeldingen met Azure RemoteApp-sjabloon."
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/15/2016"
    ms.author="elizapo" />

# <a name="what-is-in-the-azure-remoteapp-template-images"></a>Wat is in de sjabloon Azure RemoteApp-afbeeldingen?

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt stopgezet. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

Uw abonnement op Azure RemoteApp-omvat drie afbeeldingen van de sjabloon:


- Windows Server 2012
- Microsoft Office 365 ProPlus (Office 365-abonnement nodig)
- Microsoft Office 2013 Professional Plus (alleen proef)

> [AZURE.IMPORTANT]Uw abonnement Azure RemoteApp verleent dat u toegang tot de software in de afbeeldingen, met uitzondering van Office 365 ProPlus, waarvoor een apart abonnement en Office 2013, dat kan worden gebruikt in de productie. Dit betekent dat u de programma's of apps op de sjabloon-afbeeldingen met uw gebruikers delen kunt. Als u een collectie die gebruikmaakt van de Windows Server 2012 R2-afbeelding maakt, kunt u bijvoorbeeld System Center Endpoint Protection voor gebruikers te openen via RemoteApp publiceren.
>
> Bekijk de [RemoteApp-licentie-informatie](remoteapp-licensing.md) voor meer informatie. En [Met behulp van Office met Azure RemoteApp-](remoteapp-o365.md) voor de Office-licenties info.

Lees verder voor meer informatie over wat elke afbeelding bevat.

## <a name="windows-server-2012-r2--the-vanilla-image"></a>Windows Server 2012 R2 ("de vanille image")
Deze afbeelding is gebaseerd op het besturingssysteem Microsoft Windows Server 2012 R2 Datacenter en heeft de volgende functies en onderdelen die worden geïnstalleerd om te voldoen aan de vereisten voor Azure RemoteApp-sjabloon-afbeeldingen:


- .NET framework 4.5, 3.5.1, 3,5-inch
- Bureaubladbelevenis
- En handschriftservices
- Media Foundation
- Host voor extern bureaublad-sessies
- Windows PowerShell 4.0
- Windows PowerShell ISE
- WoW64-ondersteuning

Deze afbeelding heeft ook de volgende toepassingen zijn geïnstalleerd:

- Adobe Flash Player
- Microsoft Silverlight
- Microsoft System Center 2012 Endpoint Protection
- Microsoft Windows MediaPlayer


## <a name="microsoft-office-365-proplus-subscription-required"></a>Microsoft Office 365 ProPlus (abonnement vereist)
Office 365 is de meest gevraagde toepassing, zodat wij voor u werken met een 'custom' afbeelding gemaakt.

Deze afbeelding is een uitbreiding van de vanille afbeelding en heeft de volgende onderdelen van Microsoft Office 365 ProPlus naast de onderdelen die worden beschreven in de installatiekopie van Windows Server 2012 R2 is geïnstalleerd:


- Toegang
- Excel
- Lync
- OneNote
- OneDrive voor bedrijven (Let op de sync-agent wordt niet ondersteund voor gebruik met Azure RemoteApp)
- Outlook
- PowerPoint
- Word
- Microsoft Office-taalprogramma 's

De afbeelding bevat ook Visio Pro en Pro-Project.

En de volgende toepassingen ook:

- SQL Native client
- ODBC-stuurprogramma
- SQL Server Data Mining client
- MasterDataServices-client
- Microsoft Publisher
- PowerQuery
- PowerMap


Volledige functionaliteit van Office 365 ProPlus apps is alleen beschikbaar voor gebruikers met een Office 365 ProPlus plan. Abonnement plannen Zie voor meer informatie over de Office 365 [serviceplannen voor Office 365](http://technet.microsoft.com/library/office-365-plan-options.aspx). Nog vragen? Bekijk de [Office 365 + RemoteApp-](remoteapp-o365.md) informatie. Controleer ook het nieuwe artikel, [het gebruik van uw abonnement op Office 365 met Azure RemoteApp](remoteapp-officesubscription.md).

Overigens moet u afzonderlijk - licentie voor Office 365 ProPlus, Visio Pro en Project Pro ze elk hun eigen licentie hebben.

## <a name="microsoft-office-2013-professional-plus-trial-only"></a>Microsoft Office 2013 Professional Plus (alleen proef)
Tijdens de gratis proefperiode kunt u de service met de afbeelding van Office 2013 te testen.

Deze afbeelding is een uitbreiding van de vanille afbeelding en heeft de volgende onderdelen van Microsoft Office 2013 Professional Plus naast de onderdelen die worden beschreven in de installatiekopie van Windows Server 2012 R2 is geïnstalleerd:


- Toegang
- Excel
- Lync
- OneNote
- OneDrive voor bedrijven (Let op de sync-agent wordt niet ondersteund voor gebruik met Azure RemoteApp)
- Outlook
- PowerPoint
- Project
- Visio
- Word
- Microsoft Office-taalprogramma 's

> [AZURE.IMPORTANT]**Juridische informatie:** Deze afbeelding bevat geen een Microsoft Office-licentie en *kan niet worden gebruikt voor de productie*. De Office 2013 Professional Plus afbeelding is bedoeld voor het testgebruik alleen. Als u Office apps in Azure RemoteApp gebruiken voor de productie wilt, moet u de afbeelding met Office 365 ProPlus gebruiken. Zie voor meer informatie over licenties Office [met behulp van Office 365 met Azure RemoteApp](remoteapp-o365.md)
