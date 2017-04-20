<properties 
   pageTitle="StorSimple hardware component vervangen | Microsoft Azure"
   description="Wordt beschreven hoe u de PCMs, accu controller modules, EBOD-controllers, schijfstations en chassis van een apparaat StorSimple veilig te vervangen."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="10/11/2016"
   ms.author="alkohli" />

# <a name="storsimple-hardware-component-replacement"></a>StorSimple hardware component vervangen

## <a name="overview"></a>Overzicht

De zelfstudies voor hardware-onderdeel vervanging beschrijven de hardwareonderdelen van uw apparaat Microsoft Azure StorSimple 8000-serie en de noodzakelijke stappen voor het verwijderen en vervangen. Dit artikel beschrijft de pictogrammen veiligheid biedt aanwijzers naar de gedetailleerde zelfstudies en geeft een overzicht van de onderdelen die vervangen worden.

>[AZURE.IMPORTANT] Voordat u probeert te verwijderen of te vervangen enig onderdeel van de StorSimple, moet u controleren de [veiligheid pictogram conventies](#safety-icon-conventions) en andere [voorzorgsmaatregelen](storsimple-safety.md).
 
### <a name="safety-icon-conventions"></a>Veiligheid pictogram verdragen

De volgende tabel beschrijft de veiligheid pictogrammen in deze zelfstudies. Aandacht besteden aan deze pictogrammen veiligheid terwijl u door de stappen om het Apparaatonderdelen verwijderen en vervangen.

| Pictogram | Tekst | Als u meer informatie |
|:---- |:---- |:-----------|
|![Waarschuwingspictogram](./media/storsimple-hardware-component-replacement/Warning.png)| **GEVAAR!** | Geeft aan dat een gevaarlijke situatie die, indien niet vermeden, tot de dood of ernstig letsel leidt. Dit woord signaal is beperkt tot de meest extreme situaties.|
|![Waarschuwingspictogram](./media/storsimple-hardware-component-replacement/Warning.png)| **WAARSCHUWING!** | Geeft aan dat een gevaarlijke situatie die, indien niet vermeden, in de dood of ernstig letsel resulteren kan.|
|![Pictogram Let op](./media/storsimple-hardware-component-replacement/Caution.png)| **LET OP!** |Geeft aan dat een gevaarlijke situatie die, indien niet vermeden, in kleine of gemiddelde schade resulteren kan.|
|![Aankondiging-pictogram](./media/storsimple-hardware-component-replacement/NoticeIcon.png)| **LET OP:** | Beschouwd als belangrijk, maar geen gevaar-gerelateerde informatie aangeeft.|
|![Pictogram elektrische schok](./media/storsimple-hardware-component-replacement/Electric.png) | **Gevaar van elektrische schokken** | Hoge voltage aangeeft.|
|![Zwaar gewicht-pictogram](./media/storsimple-hardware-component-replacement/Weight.png)| **Zwaar gewicht**| |
|![Geen Gebruikerspictogram bouwtechnisch onderdelen](./media/storsimple-hardware-component-replacement/NoUserServiceableParts.png)| **Geen gebruiker bouwtechnisch onderdelen** | Geen toegang tot tenzij goed opgeleid.|
|![Pictogram voor gelezen instructies](./media/storsimple-hardware-component-replacement/ReadInstructions.png)|**Lees de instructies eerst**| |
|![Pictogram gevaar voor een tip](./media/storsimple-hardware-component-replacement/TipHazard.png)|**Tip gevaar**| |

### <a name="before-you-begin"></a>Voordat u begint

Lees de veiligheidsinformatie over de pictogrammen op het apparaat en de veiligheid in deze zelfstudie wordt gebruikt. Ga naar [veilig installeren en gebruiken van uw apparaat StorSimple](storsimple-safety.md) voor volledige informatie. Zorg ervoor dat de [voorzorgsmaatregelen](storsimple-safety.md#handling-precautions) bekijken voordat u het apparaat StorSimple verwerken. 

Voordat u een onderdeel vervangen probeert, kunt u de volgende informatie.

![Waarschuwingspictogram](./media/storsimple-hardware-component-replacement/Warning.png) ![elektrische schok pictogram](./media/storsimple-hardware-component-replacement/Electric.png) **Waarschuwing!** 

- Op de grond zelf goed met behulp van een elektrostatische ontlading of antistatische mat bij het verwerken van modules en onderdelen van het apparaat StorSimple.

- Alle circuits niet aanraakt. Geleverde grepen en hulplijnen gebruiken tijdens het verwerken van componenten die circuits kunnen zijn blootgesteld.

![Waarschuwingspictogram](./media/storsimple-hardware-component-replacement/Warning.png) ![u ziet het pictogram](./media/storsimple-hardware-component-replacement/NoticeIcon.png) **bericht:**

Wanneer u een module, **nooit een lege bay aan de achterzijde van de behuizing te laten**vervangen. Een vervangende of lege module verkrijgen voordat het verwijderen van het deel van het probleem.

## <a name="hardware-component-replacement-procedures"></a>Hardware-onderdeel vervanging procedures

Het apparaat StorSimple 8000-serie bestaat uit verschillende Plug-ins in de primaire en/of EBOD-behuizingen. De 8100 heeft één primaire behuizing, terwijl de 8600 een dual-behuizing met een primaire behuizing en een EBOD behuizing is.

De belangrijkste hardware-onderdelen in uw apparaat zijn samengevat in de volgende tabellen. Klik op de koppeling in de kolom **vervangingsprocedure** gaat u naar de bijbehorende handleiding.

|Onderdelen|# Aanwezig|Plug-in voor?|Vervangingsprocedure
|:---------|:--------|:--------------|:---------------------|
| Chassis|1|Nee|[Het chassis op uw apparaat StorSimple vervangen](storsimple-chassis-replacement.md) |
|Primaire domeincontrollers|2|Ja| [Een controllermodule op uw apparaat StorSimple vervangen](storsimple-controller-replacement.md) |
|764W voeding en koeling Modules (PCMs)|2|Ja| [Vervangen van een voeding en koeling Module op uw apparaat StorSimple](storsimple-power-cooling-module-replacement.md) |
|Back-up batterij|2|Ja| [De back-up batterij module op uw apparaat StorSimple vervangen](storsimple-battery-replacement.md) |
|Harde schijven|12|Ja| [Vervangen van een schijf op uw apparaat StorSimple](storsimple-disk-drive-replacement.md) |

**Tabel 1** De hardwareonderdelen in de primaire behuizing

De behuizing van de primaire en de EBOD verschillen in de i/o-modules. Ook hebben de PCMs verschillende vermogen. De PCMs in de primaire behuizing zijn 764 W, die in de ruimte EBOD 580 zijn W. De PCMs in de primaire ruimte bevatten ook een back-up batterij module.

|Onderdelen|# Aanwezig|Plug-in voor?| Vervangingsprocedure
|:---------|:--------|:--------------|:---------------------|
|Chassis|1|Nee| [Het chassis op uw apparaat StorSimple vervangen](storsimple-chassis-replacement.md) |
|EBOD-controllers|2|Ja| [Een EBOD-controller op uw apparaat StorSimple vervangen](storsimple-ebod-controller-replacement.md) |
|580 w bij voedings- en koelingsmodules (PCMs)|2|Ja| [Vervangen van een voeding en koeling Module op uw apparaat StorSimple](storsimple-power-cooling-module-replacement.md) |
|Harde schijven|12|Ja| [Vervangen van een schijf op uw apparaat StorSimple](storsimple-disk-drive-replacement.md) |

**Tabel 2** De hardwareonderdelen in de ruimte EBOD

De Plug-ins op het apparaat worden gemarkeerd in de volgende voor- en achterzijde diagrammen. Deze diagrammen kunt u de locatie van de verschillende Plug-ins te bepalen als een vervanging vereist is. Het voorste diagram toont de schijven en de diagrammen achterkant van de behuizing van de EBOD en het weergeven van de primaire behuizing Plug-ins.

![Frontplane van het apparaat met harde schijven](./media/storsimple-hardware-component-replacement/IC741028.png)

**Figuur 1** Voorkant van het apparaat

|Label|Beschrijving|
|:----|:----------|
|0 - 11|Schijfstations (totaal van 12)|

Zowel de primaire behuizing en de EBOD hebben station vervoerders modules. Het chassis ook nieuwste twaalf 3,5-inch harde schijven in een 3-bij-4-indeling zijn gerangschikt.

![Backplane van apparaatmodules primaire behuizing](./media/storsimple-hardware-component-replacement/IC740994.png)

**Figuur 2** Achterzijde van de behuizing van de primaire

|Label|Beschrijving|
|:----|:----------|
|1|PCM-0|
|2|PCM-1|
|3|0-controller|
|4|1-controller|

![Backplane van apparaat EBOD behuizing Plug-ins](./media/storsimple-hardware-component-replacement/IC769599.png)

**Figuur 3** Achterzijde van de behuizing EBOD

|Label|Beschrijving|
|:----|:----------|
|1|PCM-0|
|2|PCM-1|
|3|EBOD-Controller 0|
|4|1 EBOD-Controller|

## <a name="field-replaceable-units"></a>Veld vervangbare eenheden

Het volgende veld vervangbare eenheden (FRU) zijn beschikbaar voor uw apparaat StorSimple:

- Chassis (met inbegrip van de geïntegreerde activiteiten Configuratiescherm)

- 764 W AC PCM

- 580 W AC PCM

- Vaste schijf met station vervoerders module

- Controllermodule

- EBOD-controller-module

- Back-up batterij module

- Rack rail montageset

Neem [contact op met Microsoft Support](storsimple-contact-microsoft-support.md) om het bestellen van een van deze eenheden voor vervanging.

## <a name="next-steps"></a>Volgende stappen

Bekijk alle [veiligheidsinformatie](storsimple-safety.md) voordat u probeert een StorSimple hardwarecomponent vervangen.
