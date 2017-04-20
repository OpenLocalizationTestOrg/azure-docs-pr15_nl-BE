<properties 
   pageTitle="Beheren van uw back-up beleid StorSimple | Microsoft Azure"
   description="Wordt uitgelegd hoe u kunt de StorSimple Manager-service maken en beheren van handmatige back-ups back-up schema's en back-up bewaren."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carmonm"
   editor=""/>
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="05/10/2016"
   ms.author="v-sharos"/>

# <a name="use-the-storsimple-manager-service-to-manage-backup-policies"></a>De StorSimple Manager-service gebruiken voor het beheren van back-beleid

[AZURE.INCLUDE [storsimple-version-selector-manage-backup-policies](../../includes/storsimple-version-selector-manage-backup-policies.md)]

## <a name="overview"></a>Overzicht

In deze zelfstudie wordt uitgelegd hoe de pagina **Back-beleid** StorSimple Manager gebruiken om back-upprocessen en back-up bewaren voor de StorSimple volumes. Ook wordt beschreven hoe een handmatige back-up.

De pagina **Beleid voor back-up** kunt u back-beleid beheren en plannen van lokale en momentopnamen van de wolk. (Back-beleid worden gebruikt voor het configureren van back-up schema's en back-up bewaren van een collectie van volumes). Back-beleid kunnen u een momentopname van meerdere volumes tegelijkertijd. Dit betekent dat de back-ups gemaakt door een back-up beleid consistent crash kopieën. Op deze pagina worden de back-beleid, de typen gekoppelde volumes, het aantal back-ups bewaard en dit beleid inschakelen.

De pagina **Beleid voor back-up** kunt u de bestaande back-beleid door een of meer van de volgende velden filteren:

- **Beleidsnaam** : de naam die is gekoppeld aan het beleid. De verschillende soorten beleid omvatten:

   - Geplande beleidsregels die expliciet door de gebruiker worden gemaakt.
   - Automatische beleidsregels die worden gemaakt wanneer de standaard back-up voor dit volume-optie is ingeschakeld op het moment van het volume maken. Deze beleidsregels worden genoemd als VolumeName_Default, waarbij de naam van Volume naar de naam van het volume StorSimple is ingesteld door de gebruiker in de klassieke Azure portal verwijst. Het beleid voor automatische dagelijkse wolk momentopnamen vanaf 22:30 apparaat tijd tot gevolg hebben.
   - Geïmporteerde beleid, StorSimple Snapshot Manager oorspronkelijk zijn gemaakt. Deze hebben een code die de host StorSimple Snapshot Manager die het beleid beschrijft van zijn geïmporteerd.

- **Volumes** – de volumes die zijn gekoppeld aan het beleid. Alle volumes die zijn gekoppeld aan een back-up beleid worden gegroepeerd wanneer back-ups worden gemaakt.

- **Laatste geslaagde back-up** : de datum en tijd van de laatste geslaagde back-up die is gemaakt met dit beleid.

- **Volgende back-up** : de datum en tijd van de volgende geplande back-up wordt gestart door dit beleid.

- **Schema's** – het nummer van de schema's die zijn gekoppeld aan de back-beleid.

De veelgebruikte bewerkingen die u op deze pagina uitvoeren kunt zijn:

- Een back-up beleid toevoegen 
- Toevoegen of wijzigen van een schema 
- Een back-beleid verwijderen 
- Een handmatige back-up maken 
- Een aangepast beleid voor back-up maken met meerdere volumes en planningen 

## <a name="add-a-backup-policy"></a>Een back-up beleid toevoegen

Voeg een back-up beleid automatisch uw back-ups plannen. De volgende stappen uitvoeren in de Azure klassieke portal voor uw apparaat StorSimple back-beleid toe te voegen. Nadat u het beleid hebt toegevoegd, kunt u een planning definiëren (Zie [toevoegen of wijzigen van een schema](#add-or-modify-a-schedule)).

[AZURE.INCLUDE [storsimple-add-backup-policy](../../includes/storsimple-add-backup-policy.md)]

![Video beschikbaar](./media/storsimple-manage-backup-policies/Video_icon.png) **Video beschikbaar**

Een video die laat zien hoe het maken van een lokale of cloud back-beleid bekijken, klik [hier](https://azure.microsoft.com/documentation/videos/create-storsimple-backup-policies/).


## <a name="add-or-modify-a-schedule"></a>Toevoegen of wijzigen van een schema

U kunt toevoegen of wijzigen van een schema dat is gekoppeld aan een bestaand back-beleid op het apparaat StorSimple. De volgende stappen uitvoeren in de Azure klassieke portal toevoegen of wijzigen van een schema.

[AZURE.INCLUDE [storsimple-add-modify-backup-schedule](../../includes/storsimple-add-modify-backup-schedule.md)]

## <a name="delete-a-backup-policy"></a>Een back-beleid verwijderen

De volgende stappen uitvoeren in de Azure klassieke portal een back-up beleid op uw apparaat StorSimple verwijderen.

[AZURE.INCLUDE [storsimple-delete-backup-policy](../../includes/storsimple-delete-backup-policy.md)]


## <a name="take-a-manual-backup"></a>Een handmatige back-up maken

De volgende stappen uitvoeren in de portal voor Azure klassiek (een-op-verzoek handmatige) back-up voor één volume maken.

[AZURE.INCLUDE [storsimple-create-manual-backup](../../includes/storsimple-create-manual-backup.md)]

## <a name="create-a-custom-backup-policy-with-multiple-volumes-and-schedules"></a>Een aangepast beleid voor back-up maken met meerdere volumes en planningen

De volgende stappen uitvoeren in de Azure klassieke portal een aangepaste back-beleid met meerdere volumes en planningen maken.

[AZURE.INCLUDE [storsimple-create-custom-backup-policy](../../includes/storsimple-create-custom-backup-policy.md)]


## <a name="next-steps"></a>Volgende stappen

Meer informatie over het [gebruik van de StorSimple Manager-service voor het beheren van uw apparaat StorSimple](storsimple-manager-service-administration.md).
