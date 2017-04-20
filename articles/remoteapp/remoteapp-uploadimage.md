
<properties
    pageTitle="Een aangepaste afbeelding uploaden voor RemoteApp Azure | Microsoft Azure"
    description="Meer informatie over het uploaden van een aangepaste afbeelding voor Azure RemoteApp"
    services="remoteapp"
    documentationCenter=""
    authors="ericorman"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="ericor" />



# <a name="upload-a-custom-image-for-azure-remoteapp"></a>Een aangepaste afbeelding uploaden voor Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt stopgezet. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

Nu u uw image aangepaste sjabloon hebt gemaakt of met de wijzigingen bijgewerkt, moet u die afbeelding uploaden naar uw fotobibliotheek Azure RemoteApp. Met de volgende stappen.


## <a name="before-you-start"></a>Voordat u begint

1.      Controleer of uw aangepaste afbeelding voldoet aan de [eisen van de afbeelding](remoteapp-imagereqs.md) en de [vereisten van de toepassing](remoteapp-appreqs.md).
2.      Installeer de [module Azure PowerShell](../powershell-install-configure.md).

## <a name="step-by-step-on-how-to-upload-custom-image"></a>Stap voor stap op hoe u een aangepaste afbeelding uploaden

1.      Azure Management Portal Open en navigeer naar de pagina RemoteApp.
2.      Klik op het tabblad **sjabloon afbeeldingen** **uploaden** onder aan de pagina.
4.      Geef een beschrijvende naam voor de afbeelding en geef de locatie van het account. Controleer dat de locatie is dezelfde locatie als de RemoteApp-collectie of een locatie waar u wilt maken.
5.      Wanneer dat wordt gevraagd, download het script op de lokale computer.
6.      De opdrachtparameters in het tekstvak naar het Klembord kopiëren.
7.      Een verhoogde Windows PowerShell-venster openen.
8.      Ga naar de map waarin u het script hebt gedownload uit de verhoogde Windows PowerShell-venster.
9.      Plak de gekopieerde opdracht en druk op **Enter**.

    Het uploadproces begint en de duur afhankelijk zijn van veel factoren, waaronder de netwerksnelheid en de grootte van de afbeelding

11.    U kunt het uploaden mislukt vanwege netwerk onderbreking of dingen die, altijd het uploadproces dat u begon te hervatten. Als u wilt doorgaan met uploaden, het script opnieuw met behulp van de opdrachtregel worden uitgevoerd.

> [AZURE.WARNING] Wijzig nooit het script uploaden. Specifieke controles zijn uitgevoerd om ervoor te zorgen dat de afbeelding voldoet aan de eisen van de afbeelding en de vereisten van de toepassing.

## <a name="common-problems"></a>Veelvoorkomende problemen

- Zorg ervoor dat u Windows PowerShell, niet Azure PowerShell gebruiken. U moet de module Azure PowerShell installeren omdat bepaalde modules nodig zijn tijdens het uploaden.
- Het script nooit wijzigen, validaties zijn er voor uw gemak.
- Als het vhd-bestand is vergrendeld tijdens het uploaden, het bestand kopiëren of verplaatsen naar een nieuwe locatie en de poging het uploaden opnieuw. Er is mogelijk een Windows-proces waardoor uploaden.  
