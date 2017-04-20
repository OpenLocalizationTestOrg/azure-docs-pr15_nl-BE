<properties
   pageTitle="Het volume van uw StorSimple klonen | Microsoft Azure"
   description="Beschrijving van de kloon van verschillende typen en wanneer u ze gebruikt en wordt uitgelegd hoe u een back-up ingesteld op een afzonderlijk volume klonen kunt gebruiken."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="07/27/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-clone-a-volume-update-2"></a>De StorSimple Manager-service gebruiken voor het klonen van een volume (Update 2)

[AZURE.INCLUDE [storsimple-version-selector-clone-volume](../../includes/storsimple-version-selector-clone-volume.md)]

## <a name="overview"></a>Overzicht

De StorSimple Manager service **Back-upcatalogus** pagina toont alle reservekopieseries die worden gemaakt wanneer de handmatige of automatische back-ups worden genomen. U kunt deze pagina gebruiken om alle de back-ups weergeven voor een back-up beleid of een volume, selecteren of verwijderen van back-ups of een back-up kunt terugzetten of het klonen van een volume.

![Back-upcatalogus pagina](./media/storsimple-clone-volume-u2/backupCatalog.png)  

In deze zelfstudie wordt beschreven hoe u kunt een back-up ingesteld op het klonen van een afzonderlijk volume. Ook wordt het verschil tussen *tijdelijke* en *permanente* klonen uitgelegd.

>[AZURE.NOTE] 
>
>Een lokaal vaste volume wordt gekloond als een gelaagde volume. Als u het volume gekloonde lokaal worden vastgemaakt, kunt u de kloon converteren naar een lokaal vaste volume nadat de kopieerbewerking is voltooid. Ga naar [het volumetype wijzigen](storsimple-manage-volumes-u2.md#change-the-volume-type)voor meer informatie over het converteren van een gelaagde volume naar een lokaal vaste volume.
>
>Als u probeert te converteren van een gekloonde volume van mislukt trapsgewijs geschakeld lokaal vastgehouden onmiddellijk na het klonen (als dit nog steeds een kloon van voorbijgaande aard), de conversie en het volgende foutbericht weergegeven:
>
>`Unable to modify the usage type for volume {0}. This can happen if the volume being modified is a transient clone and hasn’t been made permanent. Take a cloud snapshot of this volume and then retry the modify operation.` 
>
>Deze fout wordt alleen weergegeven als u op een ander apparaat zijn klonen ontvangen. Lokaal vastgehouden als u de tijdelijke kloon eerst converteren naar een permanente kloon van het volume kunt converteren. Als u wilt de tijdelijke kloon omzetten in een permanente kloon, een momentopname van een wolk van het te nemen.

## <a name="create-a-clone-of-a-volume"></a>Een kloon van een volume maken

U kunt een kloon op hetzelfde apparaat, een ander apparaat of zelfs een virtuele machine maken met behulp van een lokale of een momentopname van de wolk.

#### <a name="to-clone-a-volume"></a>Klonen van een volume

1. Klik op het tabblad **back-up-catalogus** en selecteert u een back-upset op de pagina StorSimple Manager-service.

2. De back-up ingesteld op de bijbehorende volumes uitbreiden. Klik op en selecteer een volume in de back-upset.

     ![Klonen van een volume](./media/storsimple-clone-volume-u2/CloneVol.png) 

3. Klik op **klonen** om te beginnen met het klonen van het geselecteerde volume.

4. In de wizard Volume klonen onder **locatie en naam opgeven**:

  1. Identificeren van een doelapparaat. Dit is de locatie waar de kloon wordt gemaakt. Kies het apparaat of een ander apparaat opgeven. Als u een volume dat is gekoppeld aan andere cloud-aanbieders (niet Azure) fysieke apparaten alleen weergegeven in de vervolgkeuzelijst voor het doelapparaat. U kunt een volume dat is gekoppeld aan andere cloud-serviceproviders op een virtueel apparaat kan niet klonen.

        >[AZURE.NOTE] Zorg ervoor dat de vereiste capaciteit voor de kloon lager dan de capaciteit die beschikbaar is op het doelapparaat is.

  2. Geef een unieke volumenaam voor de kloon. De naam moet tussen 3 en 127 tekens bevatten. 
    
        >[AZURE.NOTE] Het veld **Volume kloon als** worden **Tiered** , zelfs als u een lokaal vaste volume zijn klonen. U kunt deze instelling; niet wijzigen. echter als u nodig hebt voor het gekloonde volume ook lokaal worden vastgemaakt, kunt u converteren de kloon op een lokaal vaste volume nadat u de kloon is gemaakt. Ga naar [het volumetype wijzigen](storsimple-manage-volumes-u2.md#change-the-volume-type)voor meer informatie over het converteren van een gelaagde volume naar een lokaal vaste volume.

        ![Kloon wizard 1](./media/storsimple-clone-volume-u2/clone1.png) 

  3. Klik op het pijlpictogram ![pictogram pijl](./media/storsimple-clone-volume-u2/HCS_ArrowIcon.png) om verder te gaan naar de volgende pagina.

5. Klik onder **Geef op hosts die dit volume kunnen gebruiken**:

  1. Geef een access control-record (ACR) voor de kloon. U kunt een nieuwe ACR toevoegen of kies uit de bestaande lijst.

        ![Kloon wizard 2](./media/storsimple-clone-volume-u2/clone2.png) 

  2. Klik op het pictogram ![pictogram](./media/storsimple-clone-volume-u2/HCS_CheckIcon.png)voor deze bewerking.

6. Een kloon van project wordt gestart en u wordt gewaarschuwd wanneer de kloon is gemaakt. Klik op **Taak weergeven** voor het controleren van de kloon taak op de pagina **taken** . U ziet het volgende bericht weergegeven wanneer het kopiëren is voltooid:

    ![Bericht van de kloon](./media/storsimple-clone-volume-u2/CloneMsg.png) 

7. Na de voltooiing van de taak klonen:

  1. Ga naar de pagina **apparaten** en klik op het tabblad **Volume Containers** . 
  2. Selecteer het volume container die is gekoppeld aan het bronvolume die u gekloond. U ziet in de lijst met volumes, de kloon die zojuist is gemaakt.

>[AZURE.NOTE] En de standaard back-up op een gekloonde volume automatisch uitgeschakeld.

Een kloon die is gemaakt op deze manier is een kloon van voorbijgaande aard. Voor meer informatie over typen klonen Zie [tijdelijke versus blijvende klonen](#transient-vs.-permanent-clones).

Deze kloon is nu een normaal volume en elke bewerking die is mogelijk op een volume beschikbaar zal zijn voor de kloon. U moet dit volume voor alle back-ups configureren.

## <a name="transient-vs-permanent-clones"></a>Tijdelijk versus permanente klonen

Tijdelijke klonen worden alleen gemaakt wanneer u naar een ander apparaat zijn klonen. U kunt een specifiek volume vanaf een back-up ingesteld op een ander apparaat, beheerd door de StorSimple Manager klonen. De tijdelijke kloon hebben verwijzingen naar de gegevens in het oorspronkelijke volume en deze gegevens gebruiken om te lezen en schrijven van lokaal op het doelapparaat. 

Nadat u een momentopname van een wolk van een tijdelijke kloon, worden de resulterende kloon een *permanente* kloon. Tijdens dit proces wordt een kopie van de gegevens van de wolk wordt gemaakt en de tijd voor het kopiëren van deze gegevens wordt bepaald door de grootte van de gegevens en de Azure vertragingstijden (dit is een kopie van Azure naar Azure). Dit proces kan dagen tot weken duren. De tijdelijke kloon een permanente kloon wordt op deze manier en geen verwijzingen naar het oorspronkelijke volumegegevens dat deze werd gekloond uit. 

## <a name="scenarios-for-transient-and-permanent-clones"></a>Scenario's voor tijdelijke en permanente klonen

De volgende secties beschreven voorbeeld situaties waarin een tijdelijke en permanente klonen kunnen worden gebruikt.

### <a name="item-level-recovery-with-a-transient-clone"></a>Herstel op itemniveau met een kloon van voorbijgaande aard

U moet één jaar oude Microsoft PowerPoint-presentatie-bestand te herstellen. Uw IT-beheerder identificeert de specifieke back-up van die periode, en het volume vervolgens gefilterd. De beheerder vervolgens klonen van het volume, wordt gezocht naar het bestand dat u zoekt, en biedt dit aan u. In dit scenario wordt wordt een tijdelijke kopie gebruikt. 
 
![Video beschikbaar](./media/storsimple-clone-volume-u2/Video_icon.png) **Video beschikbaar**

Om een video waarin wordt gedemonstreerd hoe u kunt de kloon en functies in StorSimple herstellen van verwijderde bestanden terugzetten, klik [hier](https://azure.microsoft.com/documentation/videos/storsimple-recover-deleted-files-with-storsimple/).

### <a name="testing-in-the-production-environment-with-a-permanent-clone"></a>Testen in de productie-omgeving met een permanente kloon

U moet controleren of een bug testen in de productieomgeving. U maakt een kloon van het volume in de productieomgeving en vervolgens een momentopname wolk van deze kloon maken van een onafhankelijke gekloonde volume. In dit scenario wordt wordt een permanente kloon gebruikt.  

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over het [herstellen van een StorSimple-volume vanaf een back-upset](storsimple-restore-from-backup-set-u2.md).

- Meer informatie over het [gebruik van de StorSimple Manager-service voor het beheren van uw apparaat StorSimple](storsimple-manager-service-administration.md).

 
