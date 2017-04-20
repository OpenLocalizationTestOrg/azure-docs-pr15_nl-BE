<properties 
   pageTitle="Vervang het chassis op een apparaat StorSimple | Microsoft Azure"
   description="Beschrijving van het verwijderen en vervangen van het chassis voor uw primaire behuizing StorSimple of EBOD behuizing."
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
   ms.date="08/17/2016"
   ms.author="alkohli" />

# <a name="replace-the-chassis-on-your-storsimple-device"></a>Het chassis op uw apparaat StorSimple vervangen

## <a name="overview"></a>Overzicht

In deze zelfstudie wordt uitgelegd hoe verwijderen en vervangen van een chassis met een apparaat StorSimple 8000-serie. Het model 8100 StorSimple is een apparaat in één behuizing (één chassis), dat de 8600 een dual-behuizing is (twee-chassis). Voor een model 8600 mogelijk twee chassis die in het apparaat kan niet zijn: het chassis voor de primaire ruimte of het chassis voor de EBOD behuizing.

In beide gevallen zijn de vervanging chassis dat wordt geleverd door Microsoft is leeg. Geen voeding en koeling Modules (PCMs), modules, controller, solid state harde schijven (SSD), vaste-schijfstations (harde schijven) of EBOD modules worden opgenomen.

>[AZURE.IMPORTANT] Vóór verwijderen en vervangen van het chassis, bekijk de veiligheidsinformatie in [StorSimple hardware component vervangen](storsimple-hardware-component-replacement.md).

## <a name="remove-the-chassis"></a>Verwijder de behuizing

Voer de volgende stappen uit om het chassis op uw apparaat StorSimple te verwijderen.

#### <a name="to-remove-a-chassis"></a>Voor het verwijderen van een chassis

1. Zorg ervoor dat het apparaat StorSimple is afgesloten en alle energiebronnen is verbroken.

2. Verwijder alle netwerk- en SAS-kabels, indien van toepassing.

3. Verwijder de eenheid van het rack.

4. Verwijder elk van de stations en Let op de "slots" waaruit ze zijn verwijderd. Voor meer informatie, Zie [de schijf verwijderen](storsimple-disk-drive-replacement.md#remove-the-disk-drive).

5. Op de EBOD behuizing (als dit het chassis dat is mislukt is), de EBOD-controller-modules verwijderen. Zie [een domeincontroller EBOD verwijderen](storsimple-ebod-controller-replacement.md#remove-an-ebod-controller)voor meer informatie. 

    Op de primaire behuizing (als dit het chassis dat is mislukt is), verwijder de controllers en Let op de "slots" waaruit ze zijn verwijderd. Zie [een domeincontroller verwijderen](storsimple-controller-replacement.md#remove-a-controller)voor meer informatie.

## <a name="install-the-chassis"></a>Het chassis installeren

Voer de volgende stappen uit om het chassis installeren op uw apparaat StorSimple.

#### <a name="to-install-a-chassis"></a>Een chassis installeren

1. Koppel het in het rack-chassis. Zie voor meer informatie, [rackmontage uw apparaat StorSimple 8100](storsimple-8100-hardware-installation.md#rack-mount-your-storsimple-8100-device) of [uw apparaat StorSimple 8600 rackmontage](storsimple-8600-hardware-installation.md#rack-mount-your-storsimple-8600-device).

2. Nadat het chassis is in het rek worden gemonteerd, installeer de controller modules op dezelfde positie die ze eerder zijn geïnstalleerd in.

3. Installeer de stations in dezelfde positie en sleuven die ze eerder zijn geïnstalleerd in.

    >[AZURE.NOTE] Het is raadzaam eerst de SSD installeren in de sleuven en installeer de harde schijven.

2. Met het apparaat gemonteerd in het rack en de onderdelen zijn geïnstalleerd, sluit u het apparaat op de juiste voedingsbronnen en schakel het apparaat. Zie voor details de [kabel uw StorSimple 8100-apparaat](storsimple-8100-hardware-installation.md#cable-your-storsimple-8100-device) of [het apparaat StorSimple 8600-kabel](storsimple-8600-hardware-installation.md#cable-your-storsimple-8600-device).

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [StorSimple hardware component vervangen](storsimple-hardware-component-replacement.md).

