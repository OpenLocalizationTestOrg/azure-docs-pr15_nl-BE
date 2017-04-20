<properties 
   pageTitle="Weergeven en beheren van taken StorSimple | Microsoft Azure"
   description="Beschrijving van de pagina taken StorSimple beheer en het gebruik ervan bij te houden van de recente, huidige en geplande back-uptaken."
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
   ms.workload="TBD"
   ms.date="08/17/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-view-and-manage-storsimple-jobs"></a>Gebruik de StorSimple Manager-service weergeven en beheren van taken StorSimple

[AZURE.INCLUDE [storsimple-version-selector-manage-jobs](../../includes/storsimple-version-selector-manage-jobs.md)]

## <a name="overview"></a>Overzicht

De pagina **taken** bevat één centraal portal voor weergeven en beheren van taken die zijn gestart op apparaten verbonden met uw service Manager StorSimple. U kunt taken gepland, uitgevoerd, voltooid en mislukt voor meerdere apparaten weergeven. Resultaten worden in tabelvorm gepresenteerd. 

![De pagina taken](./media/storsimple-manage-jobs/HCS_JobsPage.png)

U vindt snel de taken die door te filteren op velden zoals zijn geïnteresseerd in:

- **Status** : taken kunnen uitvoeren, geplande, mislukte, voltooid, annuleren of geannuleerd.

- **Type** : taken kunnen worden gemaakt als gevolg van een geplande of een back-op-verzoek (**Back-up nemen**), klonen, het herstellen van een apparaat of een bijwerkbewerking.

- **Apparaten** : taken worden gestart op een bepaald apparaat verbonden met uw service.

- **En** -projecten kunnen worden gefilterd op basis van de datum- en tijdbereik.

De tabelindeling van de gefilterde taken worden vervolgens aan de hand van de volgende kenmerken:

- **Type** : back-up, klonen, failover, herstellen of update.

- **Status** : met geplande, mislukte, voltooid, annuleren of geannuleerd.

- **Entiteit** : de taken kunnen worden gekoppeld aan een volume, een back-up beleid of een apparaat. Een kloon van project is gekoppeld aan een volume, dat een geplande back-uptaak gekoppeld aan een back-up beleid is. Een taak van het apparaat is gemaakt als gevolg van een noodherstel (Debet) of een bewerking voor terugzetten.

- **Apparaat** : de naam van het apparaat op waarop de taak is begonnen.

- **Gestart op** het tijdstip waarop de taak is gestart.

- **Voortgang** -het voltooiingspercentage van een taak uitgevoerd. Dit moet altijd 100% zijn voor een voltooide taak.

De lijst met taken wordt elke 30 seconden vernieuwd.

Op deze pagina kunt u de volgende taak-gerelateerde acties uitvoeren:

- Details van taak

- Een taak annuleren

## <a name="view-job-details"></a>Details van taak

Voer de volgende stappen uit om de details van een taak weer te geven.

#### <a name="to-view-job-details"></a>Taakgegevens weergeven

1. De taken die door een query uit te voeren met de juiste filters zijn geïnteresseerd in op de pagina **taken** worden weergegeven. U kunt zoeken voor voltooid, uitgevoerd of taken geannuleerd.

2. Selecteer een taak.

3. Onderaan op de pagina, klikt u op **Details**.

4. U kunt de status, gegevens, statistieken en statistische gegevens weergeven in het dialoogvenster **Details van back-up** .

## <a name="cancel-a-job"></a>Een taak annuleren

Voer de volgende stappen uit om een actieve taak annuleren.

### <a name="to-cancel-a-job"></a>Een taak annuleren

1. De actieve taak of taken die u Annuleren wilt door het uitvoeren van een query met de gewenste filters op de pagina **taken** worden weergegeven.

1. Selecteer de taak.

1. Onderaan op de pagina, klikt u op **Annuleren**.

1. Klik op **Ja**ter bevestiging.

Deze taak is nu geannuleerd.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [beheren van uw back-up beleid StorSimple](storsimple-manage-backup-policies.md).

- Meer informatie over het [gebruik van de StorSimple Manager-service voor het beheren van uw apparaat StorSimple](storsimple-manager-service-administration.md).
