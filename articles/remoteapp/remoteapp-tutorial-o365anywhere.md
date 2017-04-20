<properties
   pageTitle="Office 365 dezelfde mogelijkheden krijgen op elk apparaat met Azure RemoteApp | Microsoft Azure"
   description="Informatie over het delen van een Office 365-app met uw gebruikers via Azure RemoteApp."
   services="remoteapp"
   documentationCenter=""
   authors="guscatalano"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="remoteapp"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="compute"
   ms.date="08/15/2016"
   ms.author="guscatal;elizapo"/>


# <a name="get-the-same-office-365-experience-on-any-device-with-azure-remoteapp"></a>Office 365 dezelfde ervaring op elk apparaat met Azure RemoteApp-ophalen

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt stopgezet. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

In dit artikel wordt uitgelegd hoe het implementeren van Office 365 op elk apparaat in uw bedrijf. Uw gebruikers krijgt de mogelijkheden en ervaring op Android, Apple en Windows UI.

Wij doen als deze door het hosten van Office 365 op schaal kunnen virtuele machines in Azure waarmee gebruikers verbinding kunnen maken met behulp van RemoteApp Azure. Deze set van virtuele machines noemen we "wolk collection".

## <a name="create-a-cloud-collection"></a>Een collectie cloud maken

Eerst nadat u een Azure-account hebt gemaakt, Ga naar **RemoteApp** door te klikken op de link aan de linkerkant.
![Azure RemoteApp weergegeven op de Portal Azure](./media/remoteapp-tutorial-o365anywhere/1-menu.png)

Ga verder door te klikken op **Nieuw** in de onder- en "snelle maken" een collectie. Geef een naam, de regio, het abonnement, het plan en de afbeelding die we aanbieden 'Proffesional Office 2013'.
![Dialoogvenster maken](./media/remoteapp-tutorial-o365anywhere/2-quickcreate.png)

Zodra u klaar bent met het proces voor het maken van de collectie van het formulier moet worden gestart. Dit kan duren een uur of zo.

![Een ogenblik geduld](./media/remoteapp-tutorial-o365anywhere/3-waiting.png)

Zodra het proces is voltooid, zal er ongeveer als volgt uitzien. Als we **Publishing** we zien dat de meeste Office-toepassingen zijn gepubliceerd voor ons al op.
![Collectie gemaakt](./media/remoteapp-tutorial-o365anywhere/4-done.png)

![Gepubliceerde apps](./media/remoteapp-tutorial-o365anywhere/5-publish.png)

U kunt nu ook meer gebruikers die toegang tot deze collectie hebben door te klikken op de **Toegang van gebruikers**toevoegen.
![Configureer de gebruikerstoegang](./media/remoteapp-tutorial-o365anywhere/6-user.png)

Nu gaan we proberen om verbinding te maken met Office 365!

## <a name="connect-to-office-365"></a>Verbinding maken met Office 365

We head over naar [https://www.remoteapp.windowsazure.com/](https://www.remoteapp.windowsazure.com/), schuif naar beneden en klik op **downloaden clients** de Azure RemoteApp-client installeert op het apparaat dat u onderweg bent. De screenshots hieronder zijn voor Windows.

Zodra de toepassing wordt gestart wordt u gevraagd om te ondertekenen met uw Microsoft-account (voorheen een "Live-ID"), gebruiken hetzelfde als Azure-account nu. Als u bent aangemeld in moet een kennisgeving over nieuwe uitnodigingen, klikt u er en ziet u een lijst zoals hieronder. De uitnodiging die overeenkomt met uw e-mailadres van Azure account eigenaar accepteren.

![Nieuwe uitnodiging](./media/remoteapp-tutorial-o365anywhere/7-araclient.png)

Hoe ziet wanneer er nieuwe uitnodigingen.

![Een aanvraag accepteren](./media/remoteapp-tutorial-o365anywhere/8-invitation.png)

Zodra u de uitnodiging accepteert, moet u alle Office-toepassingen in de Azure RemoteApp-client zien.

![Lijst van apps](./media/remoteapp-tutorial-o365anywhere/9-work.png)

Wanneer u op moet een van deze die de toepassing moet worden gestart op de Azure virtual machine en u alle! Veel plezier!

![starten](./media/remoteapp-tutorial-o365anywhere/10-arastart.png)

![PowerPoint](./media/remoteapp-tutorial-o365anywhere/11-pp.png)
