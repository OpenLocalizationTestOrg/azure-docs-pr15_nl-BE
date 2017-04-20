<properties
    pageTitle="Maak een afbeelding van Azure RemoteApp-op basis van een Azure VM | Microsoft Azure"
    description="Informatie over het maken van een afbeelding voor Azure RemoteApp door te starten met een Azure virtuele machine."
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



# <a name="create-a-azure-remoteapp-image-based-on-an-azure-virtual-machine"></a>Een Azure RemoteApp-afbeelding op basis van een Azure virtuele machine maken

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt stopgezet. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

Azure RemoteApp-afbeeldingen (die in het bezit van de apps die u in uw verzameling delen) kunt u vanaf een Azure virtuele machine maken. Ook kunt u een virtuele machine gebruiken die we toegevoegd aan de galerie met Azure VM-installatiekopie die voldoet aan de vereisten voor de afbeelding Azure RemoteApp - u kunt die afbeelding VM gebruiken als uitgangspunt voor uw eigen VM, als u wilt. Kijk voor de afbeelding "Windows Server extern bureaublad-sessiehost" in de bibliotheek.

Er zijn twee stappen voor het maken van uw eigen afbeelding op basis van een Azure VM - de afbeelding maken en vervolgens uploaden naar Azure RemoteApp uit de bibliotheek Azure VM.

## <a name="create-a-custom-image-based-on-an-azure-vm"></a>Een aangepaste afbeelding op basis van een Azure VM maken

Voer deze stappen uit voor het maken van een afbeelding op basis van een Azure VM.

1. Een Azure virtuele machine maken. Kunt u de "Windows Server extern bureaublad-sessiehost" of de "Windows Server Remote Desktop Session Host met Microsoft Office 365 ProPlus" afbeelding in de galerie Azure VM afbeelding. Deze afbeelding de Azure RemoteApp-sjabloon afbeelding aan alle voorwaarden voldoet.

    Zie [een VM maken waarop Windows wordt uitgevoerd](../virtual-machines/virtual-machines-windows-hero-tutorial.md).

2. Verbinding maken met de VM en installeer en configureer de toepassingen die u wilt delen via RemoteApp. Zorg ervoor dat de extra Windows-configuraties vereist door uw toepassingen uitvoeren.

    Zie voor meer informatie [hoe u zich aanmeldt bij een Windows Server virtuele Machine wordt uitgevoerd](../virtual-machines/virtual-machines-windows-classic-connect-logon.md).

3. Als u een van de Windows Server extern bureaublad-sessiehost afbeeldingen gebruikt, is er een opgenomen validatiescript zo uw VM voldoet aan de RemoteApp-pre-reqs. dat Als script wilt uitvoeren, dubbelklikt u op het bureaublad op **ValidateRemoteAppImage** . Zorg ervoor dat alle fouten die door het script worden opgelost voordat u verdergaat met de volgende stap.

4. SYSPREP generaliseren en vastleggen van de afbeelding. Zie de [procedure voor het vastleggen van een virtuele Windows-computer te gebruiken als een sjabloon](../virtual-machines/virtual-machines-windows-classic-capture-image.md) voor instructies.



## <a name="import-the-image-into-the-azure-remoteapp-image-library"></a>De afbeelding importeren in de afbeeldingsbibliotheek Azure RemoteApp

Voer deze stappen uit als u de nieuwe afbeelding importeren in Azure RemoteApp:

1. Op het tabblad **Afbeeldingen van sjabloon** :
    - Als u geen bestaande afbeeldingen hebt, klikt u op **uploaden of een afbeelding van de sjabloon importeren**.
    - Als u al hebt ten minste één afbeelding, klikt u op **+** voor een nieuwe installatiekopie toevoegen.

2. Selecteer de bibliotheek **importeren een afbeelding van uw virtuele Machines** en klik op **volgende**.

3. Uw aangepaste afbeelding in de lijst selecteren en bevestigen dat u de stappen die worden vermeld bij het maken van uw image op de volgende pagina. Klik op **volgende**.
4. Voer een naam voor de nieuwe afbeelding van RemoteApp- en kies de locatie en klik op het selectievakje om het importproces te starten.

> [AZURE.NOTE] U kunt afbeeldingen importeren van elke Azure locatie ondersteund door Azure virtuele Machines op een Azure locatie Azure RemoteApp wordt ondersteund. Het importeren kan maximaal 25 minuten duren afhankelijk van de locaties.

U bent nu gereed voor het maken van de nieuwe collectie, ofwel een collectie [cloud](remoteapp-create-cloud-deployment.md) of een [hybride](remoteapp-create-hybrid-deployment.md), afhankelijk van uw behoeften.
