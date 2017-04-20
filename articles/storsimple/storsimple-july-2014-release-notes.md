<properties 
   pageTitle="StorSimple 8000 Release versie release-informatie voor | Microsoft Azure"
   description="Beschrijving van de nieuwe functies, de openstaande kwesties en de beschikbare tijdelijke oplossingen voor het juli 2014 Microsoft Azure StorSimple release."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carmonm"
   editor="" />
 <tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="04/18/2016"
   ms.author="v-sharos" />

# <a name="storsimple-8000-series-release-version-release-notes---july-2014"></a>StorSimple 8000-serie versie release notes - juli 2014 

## <a name="overview"></a>Overzicht

De releaseopmerkingen volgen de essentiële openstaande kwesties voor de 8000-serie StorSimple identificeren juli 2014 algemene beschikbaarheid (GA) versie van Microsoft Azure StorSimple. Deze versie overeenkomt met softwareversie 6.3.9600.17215.  

Tenzij anders vermeld, worden deze releaseopmerkingen van toepassing op alle modellen van de StorSimple apparaat. De release-opmerkingen worden voortdurend bijgewerkt; Als u kritieke problemen waarvoor een oplossing worden gevonden, worden deze toegevoegd. Voordat u uw Microsoft Azure StorSimple oplossing implementeert, moet u rekening houden met de volgende informatie.  

## <a name="known-issues-in-this-release"></a>Bekende problemen in deze versie
De volgende tabel bevat een overzicht van bekende problemen in deze release.  
 
| Nr. | Functie | Probleem | Opmerkingen/oplossing | Bij toepassing op het fysieke apparaat. | Van toepassing is op een virtueel apparaat |
|-----|---------|-------|----------------------------|----------------------------|---------------------------|
| 1 | Factory reset | In sommige gevallen, wanneer u een factory-reset het apparaat StorSimple vastgelopen en dit bericht weergeven: **opnieuw instellen van de fabriek wordt uitgevoerd (fase 8)**. Dit gebeurt als u op CTRL + C drukt terwijl de cmdlet uitgevoerd wordt. | Druk CTRL + C niet op na een factory reset wordt gestart. Bent u al in deze toestand, neem u contact op met Microsoft Support voor volgende stappen. | Ja | Nee |
| 2 | Quorum schijf | In uitzonderlijke gevallen, als het merendeel van de schijven in de behuizing van de EBOD van een 8600 apparaat wordt verbroken, waardoor er geen quorum schijf vervolgens de groep worden off line. Het wordt off line blijven, zelfs als de schijven opnieuw zijn verbonden. | Moet u het apparaat opnieuw wordt opgestart. Als het probleem zich blijft voordoen, neem u contact op met Microsoft Support voor volgende stappen. | Ja | Nee |
| 3 | Wolk momentopname storingen | In zeldzame gevallen kan een momentopname van een wolk mislukken met de fout **Maximum back-limiet is bereikt**. Dit gebeurt als u meer dan 255 online klonen op hetzelfde apparaat uit hetzelfde oorspronkelijke volume dat is verwijderd. | | Ja | Ja |
| 4 | Onjuiste besturings-ID | Als een controller vervanging wordt uitgevoerd, kan controller 0 als controller 1 weergegeven. Tijdens de controller vervangen, wanneer de afbeelding wordt geladen vanuit het knooppunt op hetzelfde niveau, kunt de besturings-ID weergegeven in eerste instantie als peer-controller-ID. In zeldzame gevallen kan dit gedrag ook nadat het systeem opnieuw moet worden gezien. | Er is geen gebruikersactie vereist. Deze situatie oplossen zelf na de vervanging van de domeincontroller voltooid is. | Ja | Nee |
| 5 | Grafieken controle apparaat | In de service Manager StorSimple grafieken controle apparaat werken niet als Basic of NTLM-verificatie is ingeschakeld in de configuratie van de proxyserver voor het apparaat. | Wijzig de web proxy-configuratie voor het apparaat geregistreerd bij uw StorSimple Manager-service zodanig dat verificatie is ingesteld op geen. Voer hiervoor de de Windows PowerShell voor de cmdlet Set-HcsWebProxy van StorSimple. | Ja | Ja |
| 6 | Opslag-accounts | Met behulp van de Storage-service te verwijderen van de account van de opslag is een niet-ondersteund scenario. Dit zal leiden tot een situatie waarin de gebruikersgegevens kan niet worden opgehaald. | | Ja | Ja |
| 7 | Failback | Binnen 24 uur na herstel (DR) een failback-bewerking wordt niet ondersteund. | | Ja | Nee |
| 8 | Failover van apparaat | Meerdere failovers van een container volume van het bronapparaat met dezelfde naar andere doelapparaten wordt niet ondersteund. Failover van één dode apparaat met meerdere apparaten maakt de containers volume op het eerste apparaat failover geen eigenaar van de gegevens. Deze containers volume wordt na een failover worden weergegeven of anders werken wanneer u ze in de klassieke Azure portal weergeeft. | | Ja | Nee |
| 9 | Installatie | Tijdens de StorSimple Adapter voor SharePoint-installatie moet u een apparaat IP-adres voor de installatie is voltooid. | | Ja | Nee |
| 10 | Netwerkinterfaces | Netwerkinterfaces gegevens 2 en 3 van de gegevens zijn in de software omgewisseld. | Neem contact op met Microsoft Support als u nodig hebt voor het configureren van deze interfaces. | Ja | Nee |


 
