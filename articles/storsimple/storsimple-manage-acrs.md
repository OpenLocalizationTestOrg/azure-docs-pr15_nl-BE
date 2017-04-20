<properties 
   pageTitle="Records uit access control in StorSimple beheren | Microsoft Azure"
   description="Wordt beschreven hoe u access control records (ACRs) gebruiken om te bepalen welke hosts kunnen verbinding maken met een volume op het apparaat StorSimple."
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
   ms.date="08/18/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-manage-access-control-records"></a>De StorSimple Manager-service gebruiken voor het beheren van records uit access control

## <a name="overview"></a>Overzicht

Records uit Access control (ACRs) kunnen u opgeven welke hosts kunnen verbinding maken met een volume op het apparaat StorSimple. ACRs zijn ingesteld op een specifiek volume en de iSCSI-gekwalificeerde namen bevatten (IQN's de gebruikershandleiding) van de hosts. Als een host verbinding probeert te maken met een volume, wordt de ACR voor de naam IQN volume gekoppeld en de verbinding gemaakt als er een overeenkomst wordt gevonden, het apparaat gecontroleerd. De sectie access control records op de pagina **configureren** alle access control worden de records weergegeven met de bijbehorende IQN's de gebruikershandleiding van de hosts.

In deze zelfstudie wordt uitgelegd dat de volgende algemene taken die betrekking hebben op ACR:

- Een access control-record toevoegen 
- Record voor een besturingselement bewerken 
- Een access control-record verwijderen 

> [AZURE.IMPORTANT] 
> 
> - Wanneer een ACR toe te wijzen aan een volume, ervoor te zorgen dat het volume is niet gelijktijdig geopend door meer dan één niet-geclusterde host omdat dit het volume kan beschadigen. 
> - Wanneer een ACR verwijderen van een volume, ervoor zorgen dat de corresponderende host geen toegang heeft tot het volume omdat de verwijdering leiden een verstoring van de alleen-lezen tot kan.

## <a name="add-an-access-control-record"></a>Een access control-record toevoegen

U gebruikt de pagina StorSimple Manager service **configureren** ACRs toevoegen. U wordt normaal gesproken één ACR koppelen aan één volume.

Voer de volgende stappen voor het toevoegen van een ACR.

#### <a name="to-add-an-access-control-record"></a>Een access control-record toe te voegen

1. Selecteer uw service op de openingspagina van de service, dubbelklik op de servicenaam van de en klikt u vervolgens op het tabblad **configureren** .

2. In de aanbieding in tabelvorm in **Access control-records**, Geef een **naam** voor uw ACR.

3. Geef de naam IQN van uw Windows-host onder **iSCSI Initiator-naam**. Als u de IQN van uw Windows Server-host, het volgende doen:

   - Start de Microsoft iSCSI-initiator op de Windows-host.
   - In het venster **iSCSI Initiator-eigenschappen** op het tabblad **configuratie** en selecteer en kopieer de tekenreeks uit het veld **Naam van de Initiator** .
   - Deze tekenreeks in het veld **iSCSI Initiator-naam** voor de tabel ACRs in de klassieke Azure portal plakken.

4. Klik op **Opslaan** om de nieuwe ACR opslaan. De aanbieding in tabelvorm worden bijgewerkt om deze toevoeging weer te geven.

## <a name="edit-an-access-control-record"></a>Record voor een besturingselement bewerken

U kunt de pagina **configureren** in de klassieke Azure portal ACRs bewerken. 

> [AZURE.NOTE] Kunt u alleen deze ACRs die momenteel niet in gebruik. Als u wilt bewerken een ACR is gekoppeld aan een volume dat wordt gebruikt, moet u het volume eerst off line nemen.

Voer de volgende stappen uit om het bewerken van een ACR.

#### <a name="to-edit-an-access-control-record"></a>Record voor een besturingselement bewerken

1. Selecteer uw service op de openingspagina van de service, dubbelklik op de servicenaam van de en klikt u vervolgens op het tabblad **configureren** .

2. In de aanbieding in tabelvorm van de controle toegang tot records, de muisaanwijzer op de ACR die u wilt wijzigen.

3. Geef een nieuwe naam en/of IQN voor de ACR.

4. Klik op **Opslaan** om het opslaan van de gewijzigde ACR. De aanbieding in tabelvorm worden bijgewerkt om deze wijziging te geven.

## <a name="delete-an-access-control-record"></a>Een access control-record verwijderen

U kunt de pagina **configureren** in de klassieke Azure portal ACRs verwijderen. 

> [AZURE.NOTE] Alleen deze ACRs die momenteel niet in gebruik is, kunt u verwijderen. Als u wilt verwijderen van een ACR is gekoppeld aan een volume dat wordt gebruikt, moet u het volume eerst off line nemen.

Voer de volgende stappen uit om een access control-record te verwijderen.

#### <a name="to-delete-an-access-control-record"></a>Record voor een besturingselement verwijderen

1. Selecteer uw service op de openingspagina van de service, dubbelklik op de servicenaam van de en klikt u vervolgens op het tabblad **configureren** .

2. In het overzicht in tabelvorm van de records in access control ACRs (), de muisaanwijzer op de ACR die u wilt verwijderen.

3. Een verwijderpictogram (**x**) worden weergegeven in de rechterkolom extreme voor de ACR die u selecteert. Klik op het pictogram **x** om de ACR te verwijderen.

4. Wanneer u om bevestiging wordt gevraagd, klikt u op **Ja** om door te gaan met het verwijderen. De aanbieding in tabelvorm worden bijgewerkt zodat de verwijdering.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [beheren van StorSimple volumes](storsimple-manage-volumes.md).

- Meer informatie over het [gebruik van de StorSimple Manager-service voor het beheren van uw apparaat StorSimple](storsimple-manager-service-administration.md).
 
