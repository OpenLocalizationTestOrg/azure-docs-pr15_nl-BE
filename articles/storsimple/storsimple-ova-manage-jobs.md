<properties 
   pageTitle="Weergeven en beheren van virtuele matrix StorSimple taken | Microsoft Azure"
   description="Beschrijving van de pagina taken StorSimple beheer en het gebruik van het recente en huidige taken bijhouden voor de virtuele matrix StorSimple."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor=""/>
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="na"
   ms.date="06/07/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-view-jobs-for-the-storsimple-virtual-array"></a>De StorSimple Manager-service gebruiken om taken voor de virtuele matrix StorSimple weer te geven

## <a name="overview"></a>Overzicht

De pagina **taken** bevat één centraal portal voor het weergeven en beheren van taken die zijn gestart op virtuele Arrays (ook bekend als op ruimten virtuele apparaten) die zijn aangesloten op uw StorSimple Manager-service. U kunt taken uitgevoerd, voltooid en mislukt voor meerdere virtuele apparaten weergeven. Resultaten worden in tabelvorm gepresenteerd. 

![De pagina taken](./media/storsimple-ova-manage-jobs/ovajobs1.png)

U vindt snel de taken die door te filteren op velden zoals zijn geïnteresseerd in:

- **Status** : U kunt zoeken naar alle, de taken uitgevoerd, voltooid of mislukt.
- **En** -projecten kunnen worden gefilterd op basis van de datum- en tijdbereik.
- **Type** – het taaktype kunt all, back-up en terugzetten, failover worden downloaden van updates of updates installeren.
- **Apparaten** : taken worden gestart op een specifiek apparaat verbonden met uw service. De tabelindeling van de gefilterde taken worden vervolgens aan de hand van de volgende kenmerken:

    - **Type** – het taaktype kunt all, back-up en terugzetten, failover worden downloaden van updates of updates installeren.

    - **Status** : taken kan worden alle uitgevoerd, voltooid of mislukt.

    - **Entiteit** : de taken kunnen worden gekoppeld aan een volume, share of apparaat. 

    - **Apparaat** : de naam van het apparaat op waarop de taak is begonnen.

    - **Gestart op** : de tijd waarop de taak is gestart.

    - **Voortgang** -het voltooiingspercentage van een taak uitgevoerd. Dit moet altijd 100% zijn voor een voltooide taak.

De lijst met taken wordt elke 30 seconden vernieuwd.

## <a name="view-job-details"></a>Details van taak

Voer de volgende stappen uit om de details van een taak weer te geven.

#### <a name="to-view-job-details"></a>Taakgegevens weergeven

1. De taken die door een query uit te voeren met de juiste filters zijn geïnteresseerd in op de pagina **taken** worden weergegeven. U kunt zoeken naar taken voltooid of wordt uitgevoerd.

2. Selecteer een taak in de lijst met taken in tabelvorm.

3. Onderaan op de pagina, klikt u op **Details**.

4. In het dialoogvenster **Details** kunt u de status, gegevens en statistieken weergeven. In de volgende afbeelding ziet u een voorbeeld van het dialoogvenster **Details van back-up** .
 
    ![De detailpagina van taak](./media/storsimple-ova-manage-jobs/ovajobs2.png)

#### <a name="job-failures-when-the-virtual-machine-is-paused-in-the-hypervisor"></a>Taak fouten wanneer de virtuele machine in de hypervisor is onderbroken

Wanneer een taak in de voortgang van uw virtuele matrix StorSimple en het apparaat (virtuele machine in hypervisor ingericht) is onderbroken voor meer dan 15 minuten, de taak niet worden uitgevoerd. Dit wordt als gevolg van uw tijd StorSimple virtuele matrix gesynchroniseerd met de Microsoft Azure-tijd. Een voorbeeld van een fout bij het terugzetten taak wordt weergegeven in de volgende schermafdruk.

![Fout bij het project herstellen](./media/storsimple-ova-manage-jobs/restorejobfailure.png)

Deze storingen worden toegepast op back-up, herstellen, bijwerken en failover-taken. Als de virtuele machine in Hyper-V ingericht wordt, wordt de machine uiteindelijk tijd synchroniseren met de hypervisor. Als dat gebeurt, kunt u de taak opnieuw te starten. 

## <a name="next-steps"></a>Volgende stappen

[Informatie over het gebruik van het lokale web UI voor het beheer van uw virtuele matrix StorSimple](storsimple-ova-web-ui-admin.md).
