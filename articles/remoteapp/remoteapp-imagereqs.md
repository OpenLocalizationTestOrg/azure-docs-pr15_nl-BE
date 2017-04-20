
<properties
    pageTitle="Vereisten voor Azure RemoteApp-afbeelding | Microsoft Azure"
    description="Meer informatie over de vereisten voor het maken van afbeeldingen voor gebruik met Azure RemoteApp"
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



# <a name="requirements-for-azure-remoteapp-images"></a>Vereisten voor Azure RemoteApp-afbeeldingen

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt stopgezet. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

Een installatiekopie van Windows Server 2012 R2 Azure RemoteApp gebruikt als host voor de programma's die u wilt delen met uw gebruikers. Om een aangepaste afbeelding maken, kunt u beginnen met een bestaande afbeelding of [Maak een nieuwe](remoteapp-create-custom-image.md).

> [AZURE.TIP] Wist u dat uw Azure RemoteApp-abonnement hebt u toegang naar een Windows Server 2012 R2 afbeelding in de galerie met Azure VM die u gebruiken kunt om uw eigen Sjabloonafbeelding te maken? [Controleren](remoteapp-image-on-azurevm.md).  


De voorschriften voor de afbeelding die u voor gebruik met Azure RemoteApp uploaden kunt zijn.


- Aangepaste toepassingen opslaan geen gegevens lokaal op de afbeelding. Deze afbeeldingen zijn ' stateless ' en mag alleen toepassingen bevatten.
- De afbeelding bevat geen gegevens die verloren kunnen gaan.
- De grootte van de afbeelding moet een veelvoud van MBs. Als u een afbeelding die niet is een exact veelvoud uploaden probeert, mislukt het uploaden van bestand.
- De grootte van de afbeelding moet 127 GB of kleiner.
- Het moet op een VHD-bestand (VHDX bestanden worden momenteel niet ondersteund).
- De VHD moet een generatie 2 virtuele machine.
- De VHD zijn vaste grootte en dynamisch uitbreidbare. Een dynamisch uitbreidbare VHD wordt aanbevolen omdat er minder tijd om te uploaden naar Azure dan een vaste grootte VHD-bestand nodig.
- De schijf moet worden geïnitialiseerd met behulp van de MBR Master Boot Record () partitioneringsstijl. De partitiestijl GUID partition table (GPT) wordt niet ondersteund.
- De VHD moet bevatten één installatie van Windows Server 2012 R2. Het kan meerdere volumes, maar slechts één met een installatie van Windows bevatten.
- De functie Remote Desktop Session Host (RDSH) en de functie Bureaubladbelevenis moeten zijn geïnstalleerd.
- De functie Remote Desktop Connection Broker moet *niet* worden geïnstalleerd.
- Encrypting File System (EFS) moet worden uitgeschakeld.
- De afbeelding moet worden SYSPREPed met de parameters **/oobe / generalize/shutdown** (niet gebruiken met de parameter **/mode:vm** ).
- De VHD van de keten van een momentopname te uploaden, wordt niet ondersteund.

Zie [een Azure RemoteApp-afbeelding maken](remoteapp-imageoptions.md) voor meer informatie over het maken van afbeeldingen voor Azure RemoteApp.
