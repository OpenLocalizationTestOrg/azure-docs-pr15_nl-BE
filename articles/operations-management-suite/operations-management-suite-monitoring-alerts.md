<properties 
   pageTitle="Melding beheer in Microsoft-producten controleren | Microsoft Azure"
   description="Een waarschuwing geeft aan dat een probleem dat aandacht van de beheerder vereist.  In dit artikel worden de verschillen in hoe berichten worden gemaakt en beheerd in System Center Operations Manager (SCOM) en logboek Analytics beschreven en best practices voor het gebruik van de twee producten voor een hybride alert strategie voor." 
   services="operations-management-suite"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags 
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/06/2016"
   ms.author="bwren" />

# <a name="managing-alerts-with-microsoft-monitoring"></a>Beheren van berichten met toezicht van Microsoft 

Een waarschuwing geeft aan dat een probleem dat aandacht van de beheerder vereist.  Er zijn duidelijke verschillen tussen System Center Operations Manager (SCOM) en logboek Analytics in Operations Management Suite (OMS) hoe waarschuwingen worden gemaakt, hoe ze worden beheerd en geanalyseerd en hoe u een melding krijgt een kritiek probleem is aangetroffen.

## <a name="alerts-in-operations-manager"></a>Waarschuwingen in Operations Manager
Waarschuwingen in SCOM worden gegenereerd door afzonderlijke regels of monitoren om aan te geven van een bepaalde uitgifte.  Een monitor kan een waarschuwing wordt gegenereerd als deze in een foutstatus wanneer een regel genereert een waarschuwing geven sommige kritieke probleem dat niet rechtstreeks verband met de gezondheid van een beheerde object houden.  Beheerpakketten bevatten een groot aantal werkstromen die meldingen instellen voor de toepassing of service die ze beheren.  Onderdeel van het proces van het configureren van een nieuwe management pack is afstemmen om ervoor te zorgen dat u geen buitensporige waarschuwingen voor problemen die u niet van essentieel belang.

![SCOM waarschuwing weergeven](media/operations-management-suite-monitoring-alerts/scom-alert-view.png)

SCOM biedt volledige beheer van waarschuwingen waarschuwingen met een status die door beheerders kan worden gewijzigd als ze werken om het probleem te verhelpen.  Wanneer het probleem is opgelost, stelt de beheerder de waarschuwing gesloten op welk moment wordt niet meer weergegeven in de actieve waarschuwingen weer te geven.  Meldingen die worden gegenereerd uit de monitoren kunnen automatisch worden opgelost wanneer de monitor weer aan de orde zijn.

![Waarschuwingsstatus](media/operations-management-suite-monitoring-alerts/scom-alert-status.png)

## <a name="alerts-in-log-analytics"></a>Waarschuwingen in logboek Analytics
Een waarschuwing in het logboek Analytics wordt gemaakt van een logboek-query die op gezette tijden automatisch wordt uitgevoerd.  U kunt een waarschuwingsregel maken van een query log.  Als de query resultaten die overeenkomen met de criteria die u opgeeft retourneert, wordt een melding gemaakt.  Dit kan een specifieke query waarmee u een waarschuwing als een bepaalde gebeurtenis is gedetecteerd, of kunt u een meer algemene query waarmee wordt gezocht naar een foutgebeurtenis betrekking hebben op een bepaalde toepassing.

Meld Analytics waarschuwingen naar de opslagplaats OMS als een gebeurtenis geschreven en kunnen worden opgehaald met een query log.  Ze hebben niet een status als SCOM gebeurtenissen zodat u aangeven kunt wanneer het probleem is opgelost.

![OMS-melding](media/operations-management-suite-monitoring-alerts/oms-alert.png)

Wanneer SCOM wordt gebruikt als gegevensbron voor het logboek Analytics, worden SCOM waarschuwingen naar de opslagplaats OMS geschreven zijn gemaakt en gewijzigd.  

![Waarschuwing voor SCOM](media/operations-management-suite-monitoring-alerts/scom-alert.png)

De [Waarschuwing Management-oplossing](http://technet.microsoft.com/library/mt484092.aspx) biedt een overzicht van actieve waarschuwingen en verschillende algemene query's voor het ophalen van verschillende reeksen van waarschuwingen.  Zo krijgt u meer effectieve analyse van uw waarschuwingen dan een rapport in SCOM.  U kunt inzoomen op uit de samenvattingen naar gedetailleerde gegevens en ad-hoc query's voor het ophalen van verschillende sets van waarschuwingen maken.

![Oplossing voor beheer van waarschuwingen](media/operations-management-suite-monitoring-alerts/alert-management.png)

## <a name="notifications"></a>Meldingen
Meldingen in SCOM sturen u een e-mail- of tekst in reactie op berichten die aan bepaalde criteria voldoen.  U kunt verschillende abonnementen met verschillende mensen, afhankelijk van de criteria zoals het object wordt gecontroleerd, de ernst van de waarschuwing, het soort probleem dat gevonden melding of het tijdstip van de dag.

Enkele abonnementen kunnen worden gebruikt voor het implementeren van een strategie voor volledige aanmelding voor een groot aantal beheerpakketten.

![Waarschuwingen voor SCOM](media/operations-management-suite-monitoring-alerts/alerts-overview-scom.png)

Logboek Analytics kan een melding via e-mail een waarschuwing is gemaakt door een actie e-mailmelding voor elke [waarschuwingsregel](http://technet.microsoft.com/library/mt614775.aspx).  Het hoeft de abonneren op meerdere waarschuwingen met een enkele regel niet de mogelijkheden van SCOM.  U moet ook uw eigen waarschuwingsregels maken omdat OMS geen vooraf geconfigureerd biedt.

![Logboek Analytics waarschuwingen](media/operations-management-suite-monitoring-alerts/alerts-overview-oms.png)

Kunt u niet volledig beheren SCOM waarschuwingen in logboek Analytics maar omdat u alleen in de Console bewerkingen wijzigen kunt.  Logboek Analytics is nuttig als onderdeel van een alert beheer verwerken maar voor analyseprogramma's bieden die alleen SCOM hoeft niet.

## <a name="alert-remediation"></a>Melding herstel
[Herstel](http://technet.microsoft.com/library/mt614775.aspx) verwijst naar een poging voor het automatisch corrigeren van het probleem dat wordt aangeduid met een waarschuwing.
  
SCOM kunt u diagnose en herstel uitvoeren in reactie op een monitor een beschadigde status invoeren.  Dit gebeurt tegelijkertijd naar de monitor het signaal te maken.  Diagnose en herstel worden meestal geïmplementeerd als een script dat wordt uitgevoerd op de agent.  Een diagnose probeert meer om informatie te verzamelen over het gevonden probleem terwijl een herstelbewerking probeert het probleem te verhelpen.

Logboek Analytics kunt u een [Azure automatisering runbook](https://azure.microsoft.com/documentation/services/automation/) starten of een webhook in reactie op de waarschuwing in een logboek Analytics aanroepen.  Runbooks kan bevatten complexe logica geïmplementeerd in PowerShell.  Het script wordt uitgevoerd in Azure en toegang tot alle bronnen Azure of externe bronnen beschikbaar vanuit de cloud.  Azure automatisering beschikt over de mogelijkheid om runbooks op een server in uw lokale datacenter uitvoeren, maar deze functie is momenteel niet beschikbaar bij het starten van de runbook in reactie op signalen logboek Analytics.

Zowel de terugvinding in SCOM en runbooks in OMS PowerShell scripts bevatten, maar herstel zijn moeilijker te maken en te beheren, omdat ze moeten worden opgenomen in een management pack.  Runbooks worden opgeslagen in Azure automatisering die beschikt over functies voor het ontwerpen, testen en beheren van runbooks.

Als u voor logboek Analytics SCOM als gegevensbron gebruikt, kunt u een waarschuwing in een logboek Analytics SCOM waarschuwingen die zijn opgeslagen in de opslagplaats OMS ophalen met een query log maken.  Hierdoor zou u een Azure automatisering runbook uitgevoerd in reactie op een signaal van SCOM.  Natuurlijk, aangezien de runbook kan worden uitgevoerd in Azure, zou dit niet een levensvatbare strategie voor het herstellen van problemen voor gebouwen.

## <a name="next-steps"></a>Volgende stappen

- Informatie over de details van de [waarschuwingen in System Center Operations Manager (SCOM)](https://technet.microsoft.com/library/hh212913.aspx).