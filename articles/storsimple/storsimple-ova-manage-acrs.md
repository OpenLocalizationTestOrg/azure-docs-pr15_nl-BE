<properties 
   pageTitle="Access control-records beheren voor de virtuele matrix StorSimple | Microsoft Azure"
   description="Beschrijving van het beheren van access control-records (ACRs) om te bepalen welke hosts kunnen verbinding maken met een volume op de virtuele matrix StorSimple."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="05/03/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-manage-access-control-records-for-the-storsimple-virtual-array"></a>De StorSimple Manager-service gebruiken voor het beheren van access control-records voor de StorSimple virtuele matrix 

## <a name="overview"></a>Overzicht

Records uit Access control (ACRs) kunnen u opgeven welke hosts kunnen verbinding maken met een volume op de StorSimple virtuele matrix (ook bekend als de StorSimple op ruimten virtueel apparaat). ACRs zijn ingesteld op een specifiek volume en de iSCSI-gekwalificeerde namen bevatten (IQN's de gebruikershandleiding) van de hosts. Wanneer een host verbinding probeert te maken met een volume, het apparaat de ACR is gekoppeld aan het volume voor de naam IQN gecontroleerd en de verbinding gemaakt als er een overeenkomst wordt gevonden. De sectie **records uit access control** op de pagina **configureren** wordt weergegeven de access control met alle records de bijbehorende IQN's de gebruikershandleiding van de hosts.

In deze zelfstudie wordt uitgelegd dat de volgende algemene taken die betrekking hebben op ACR:

- De IQN ophalen
- Een access control-record toevoegen 
- Record voor een besturingselement bewerken 
- Een access control-record verwijderen 

> [AZURE.IMPORTANT] 
> 
> - Wanneer een ACR toe te wijzen aan een volume, ervoor te zorgen dat het volume is niet gelijktijdig geopend door meer dan één niet-geclusterde host omdat dit het volume kan beschadigen. 
> - Wanneer een ACR verwijderen van een volume, ervoor zorgen dat de corresponderende host geen toegang heeft tot het volume omdat de verwijdering leiden een verstoring van de alleen-lezen tot kan.

## <a name="get-the-iqn"></a>De IQN ophalen

Voer de volgende stappen uit als u de IQN van een Windows-host met Windows Server 2012.

[AZURE.INCLUDE [storsimple-get-iqn](../../includes/storsimple-get-iqn.md)]

## <a name="add-an-acr"></a>Een ACR toevoegen

De pagina **configuratie** StorSimple Manager kunt u ACRs toevoegen. U wordt normaal gesproken één ACR koppelen aan één volume.

Ga voor meer informatie over het koppelen van een ACR met een volume [een volume](storsimple-ova-deploy3-iscsi-setup.md#step-3-add-a-volume)toevoegen.

>[AZURE.IMPORTANT] 
> 
>Wanneer een ACR toe te wijzen aan een volume, ervoor te zorgen dat het volume is niet gelijktijdig geopend door meer dan één niet-geclusterde host omdat dit het volume kan beschadigen.
 
Voer de volgende stappen voor het toevoegen van een ACR.

#### <a name="to-add-an-acr"></a>Een ACR toevoegen

1. Selecteer uw service op de openingspagina van de service, dubbelklik op de servicenaam van de en klikt u vervolgens op het tabblad **configuratie** .

    ![tabblad configuratie](./media/storsimple-ova-manage-acrs/acr1.png)

2. In de aanbieding in tabelvorm in **Access control-records**, Geef een **naam** voor uw ACR.

3. Onder de **iSCSI-initiatornaam**, de IQN naam opgeven van de Windows-host. 

4. Klik op **Opslaan** onder aan de pagina om op te slaan van de zojuist gemaakte ACR. U ziet het volgende bevestigingsbericht weergegeven.

    ![bevestigingsbericht](./media/storsimple-ova-manage-acrs/acr2.png)

5. Klik op het pictogram ![Controleer het pictogram](./media/storsimple-ova-manage-acrs/check-icon.png). De aanbieding in tabelvorm worden bijgewerkt om deze toevoeging weer te geven.

## <a name="edit-an-acr"></a>Een ACR bewerken

U kunt de pagina **configuratie** in de klassieke Azure portal ACRs bewerken. 

> [AZURE.NOTE] U alleen deze ACRs die momenteel niet in gebruik. Als u wilt bewerken een ACR is gekoppeld aan een volume dat wordt gebruikt, moet u het volume eerst offline halen.

Voer de volgende stappen uit om het bewerken van een ACR.

#### <a name="to-edit-an-acr"></a>Voor het bewerken van een ACR

1. Selecteer uw service op de openingspagina van de service, dubbelklik op de servicenaam van de en klikt u vervolgens op het tabblad **configuratie** .

2. In de aanbieding in tabelvorm van de controle toegang tot records, de muisaanwijzer op de ACR die u wilt wijzigen.

3. Geef een nieuwe naam en/of IQN voor de ACR.

4. Klik op **Opslaan** onder aan de pagina om te slaan de gewijzigde ACR. Ziet u een bevestigingsbericht. 

5. Klik op het pictogram ![Controleer het pictogram](./media/storsimple-ova-manage-acrs/check-icon.png). De aanbieding in tabelvorm worden bijgewerkt om deze wijziging te geven.

## <a name="delete-an-access-control-record"></a>Een access control-record verwijderen

U kunt de pagina **configuratie** in de klassieke Azure portal ACRs verwijderen. 

> [AZURE.NOTE] 
> 
> - Verwijder alleen de ACRs die momenteel niet in gebruik. Als u wilt verwijderen van een ACR is gekoppeld aan een volume dat wordt gebruikt, moet u het volume eerst offline halen.
> - Wanneer een ACR verwijderen van een volume, ervoor zorgen dat de corresponderende host geen toegang heeft tot het volume omdat de verwijdering leiden een verstoring van de alleen-lezen tot kan.

Voer de volgende stappen uit om een access control-record te verwijderen.

#### <a name="to-delete-an-access-control-record"></a>Record voor een besturingselement verwijderen

1. Selecteer uw service op de openingspagina van de service, dubbelklik op de servicenaam van de en klikt u vervolgens op het tabblad **configuratie** .

2. In het overzicht in tabelvorm van de records in access control ACRs (), de muisaanwijzer op de ACR die u wilt verwijderen.

3. Een verwijderpictogram (**x**) worden weergegeven in de rechterkolom extreme voor de ACR die u selecteert. Klik op het pictogram **x** om de ACR te verwijderen. U ziet het volgende bevestigingsbericht weergegeven.

    ![bevestigingsbericht](./media/storsimple-ova-manage-acrs/acr3.png)

5. Klik op het pictogram ![Controleer het pictogram](./media/storsimple-ova-manage-acrs/check-icon.png). De aanbieding in tabelvorm worden bijgewerkt zodat de verwijdering.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [toevoegen van volumes en ACRs configureren](storsimple-ova-deploy3-iscsi-setup.md#step-3-add-a-volume).
