<properties 
   pageTitle="StorSimple Manager servicedashboard - virtuele matrix | Microsoft Azure"
   description="Het servicedashboard StorSimple Manager wordt beschreven en wordt uitgelegd hoe u deze gebruiken voor het controleren van de gezondheid van uw virtuele matrix StorSimple."
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
   ms.date="04/07/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-dashboard-for-the-storsimple-virtual-array"></a>De Manager StorSimple servicedashboard gebruiken voor de virtuele matrix StorSimple

## <a name="overview"></a>Overzicht

De dashboardpagina StorSimple Manager service biedt een samenvattend overzicht van de StorSimple virtuele matrices (ook bekend als StorSimple op-premises virtuele apparaten of virtuele apparaten) die zijn verbonden met de service Manager StorSimple markeren die een systeembeheerder aandacht behoeven. Deze zelfstudie introduceert de dashboardpagina Dashboardinhoud en functie uitgelegd en beschrijft de taken die u vanaf deze pagina uitvoeren kunt.

![Service-dashboard](./media/storsimple-ova-service-dashboard/dashboard1.png)

Het servicedashboard StorSimple Manager-bevat de volgende informatie:

- In het gebied **grafiek** aan de bovenkant van de pagina ziet u de relevante parameters voor uw virtuele apparaten. U kunt de primaire opslag gebruikt op alle virtuele apparaten, alsmede de cloud opslag worden gebruikt door virtuele apparaten in een bepaalde tijdsperiode weergeven. Gebruik de besturingselementen in de rechterbovenhoek van het diagram geeft de relatieve of absolute gebruik en ziet u een tijdschaal 1 week, 1 maand, 3 maanden of 1 jaar. Met de opdracht Vernieuwen ![vernieuwen-control](./media/storsimple-ova-service-dashboard/refresh-control.png) voor het vernieuwen van de grafiek.

- Het **overzicht van de syntaxis** bevat de primaire opslag die is ingericht en worden gebruikt door alle virtuele apparaten ten opzichte van de totale opslagruimte beschikbaar op alle virtuele apparaten. **Provisioned** verwijst naar de hoeveelheid opslagruimte die is voorbereid en toegewezen voor gebruik en **capaciteit Max.** wordt de maximale capaciteit van alle virtuele apparaten **gebruikt** , verwijst naar gebruik van aandelen of volumes zoals weergegeven door de initiators die op de virtuele apparaten zijn aangesloten.

- Het gebied met **meldingen** biedt een momentopname van de actieve waarschuwingen via virtuele apparaten, gegroepeerd op prioriteit voor meldingen. Te klikken op de ernst opent de pagina **waarschuwingen** binnen het bereik van deze waarschuwingen weergegeven. Op de pagina **meldingen** kunt u een afzonderlijke waarschuwing om aanvullende informatie over deze waarschuwing, met inbegrip van alle aanbevolen acties weer te geven. Als het probleem is opgelost, kunt u de waarschuwing uitschakelen.

- De module **projecten** biedt een overzicht van recente projecten op alle virtuele apparaten die zijn verbonden met uw service. Er zijn koppelingen die u gebruiken kunt om taken die momenteel in uitvoering zijn en die is geslaagd of mislukt in de afgelopen 24 uur te bekijken. 

- Het gebied **snelle blik** op de rechterkant van de pagina bevat nuttige informatie zoals de status van service Totaal aantal virtuele apparaten die zijn verbonden met de service, de locatie van de service en de details van het abonnement dat is gekoppeld aan de service. Er is ook een koppeling naar het logboek voor bewerkingen. Klik op de link voor een overzicht van alle voltooide StorSimple Manager service-activiteiten. 

U kunt de dashboardpagina StorSimple Manager service tot stand brengen van de volgende taken:

- Ophalen van de sleutel voor de inschrijving.
- De logboeken bekijken.

## <a name="get-the-service-registration-key"></a>Ophalen van de sleutel voor de inschrijving

De sleutel voor de inschrijving wordt gebruikt voor het registreren van een virtueel apparaat StorSimple met de service Manager StorSimple zodat het apparaat wordt weergegeven in Azure klassieke portal voor verdere acties. De sleutel wordt op de eerste virtuele apparaat gemaakt en gedeeld met de overige virtuele apparaten. 

Op **Registratie sleutel** (onderaan de pagina), wordt het dialoogvenster **Sleutel voor de inschrijving** , waar u de huidige registratie sleutel naar het Klembord kopiëren of opnieuw genereren van de sleutel voor de inschrijving geopend.

![registratie sleutel](./media/storsimple-ova-service-dashboard/service-dashboard3.png)

Opnieuw genereren van de sleutel heeft geen invloed op eerder geregistreerde virtuele apparaten: heeft dit gevolgen voor alleen de virtuele apparaten met de service zijn geregistreerd nadat de sleutel wordt opnieuw gegenereerd.

Ga voor meer informatie over het ophalen van de sleutel voor de inschrijving, ophalen van [de sleutel voor de inschrijving](storsimple-ova-manage-service.md#get-the-service-registration-key).

## <a name="view-the-operations-logs"></a>De bewerkingen logboeken weergeven

U kunt de logboeken weergeven door te klikken op de bewerking logboeken beschikbaar in het deelvenster **Overzicht** van het dashboard. Hiermee gaat u naar de beheerpagina services, kunt u filteren en Zie de logboeken die specifiek voor uw service Manager StorSimple.

![Logboek voor bewerkingen](./media/storsimple-ova-service-dashboard/ops-log.png)

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het [gebruik van het lokale web UI voor het beheer van uw virtuele matrix StorSimple](storsimple-ova-web-ui-admin.md).