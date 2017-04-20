<properties
   pageTitle="Problemen met trage back-up van bestanden en mappen in Azure back-up | Microsoft Azure"
   description="Bevat richtlijnen voor het oplossen van problemen vaststellen van de oorzaak van prestatieproblemen Azure back-up"
   services="backup"
   documentationCenter=""
   authors="genlin"
   manager="jimpark"
   editor=""/>

<tags
    ms.service="backup"
    ms.workload="storage-backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/13/2016"
    ms.author="genli"/>

# <a name="troubleshoot-slow-backup-of-files-and-folders-in-azure-backup"></a>Trage back-up van bestanden en mappen in Azure back-up oplossen

Dit artikel bevat richtlijnen voor het oplossen van problemen vaststellen van de oorzaak van de trage back-prestaties voor bestanden en mappen wanneer u back-Azure. Wanneer u de reservekopieagent Azure met back-up van bestanden, kan back-up langer duren dan verwacht. Dit kan worden veroorzaakt door een of meer van de volgende opties:

-   [Er zijn knelpunten op de computer die wordt back-up gemaakt.](#cause1)
-   [Een ander proces of antivirussoftware verstoort de Azure back-up-proces.](#cause2)
-   [De reservekopie-agent wordt uitgevoerd op een Azure VM (virtual machine).](#cause3)  
-   [U een back-up van een groot aantal (in miljoenen) bestanden.](#cause4)

Voordat u het oplossen van problemen, wordt aangeraden te downloaden en installeren van de [meest recente back-up Azure agent](http://aka.ms/azurebackup_agent). Wij geven regelmatig updates met het hulpprogramma Back-up diverse problemen opgelost, functies toevoegen en de prestaties verbeteren.

Ook wordt aangeraden de [Veelgestelde vragen over Azure back-up-service](backup-azure-backup-faq.md) om te controleren of dat u een van de algemene configuratieproblemen geen problemen te controleren.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

<a id="cause1"></a>
## <a name="cause-performance-bottlenecks-on-the-computer"></a>Oorzaak: Knelpunten op de computer

Knelpunten op de computer die wordt back-up gemaakt, kunnen leiden tot vertragingen. De computer kunt lezen of schrijven naar de schijf of de beschikbare bandbreedte om gegevens te verzenden via het netwerk kan bijvoorbeeld knelpunten veroorzaken.

Windows biedt een ingebouwde functie die is aangeroepen [Prestatiemeter](https://technet.microsoft.com/magazine/2008.08.pulse.aspx) (Perfmon) om deze knelpunten opspoort.

Hier zijn sommige prestatiemeters en bereiken die handig zijn bij het oplossen van knelpunten voor optimale back-ups.

| Teller  | Status  |
|---|---|
|Logische schijf (schijven)--% niet-actief   | • 100% tot 50% niet-actief inactief = orde</br>• 49% tot 20% actief inactief = waarschuwing of Monitor</br>• 19% tot 0% actief inactief = Kritiek of Out of Spec|
|  Logische schijf (schijven)--% gem. Schijf seconde lezen of schrijven |  • 0,001 ms 0.015 MS = orde</br>• 0.015 ms 0.025 MS = waarschuwing of Monitor</br>• 0.026 ms of meer = kritieke en Out of Spec|
|  Logische schijf (schijven)--huidige wachtrijlengte voor schijf (voor alle exemplaren) | 80 aanvragen voor meer dan 6 minuten |
| Geheugen--niet wisselbaar geheugen: Bytes|• Minder dan 60% van de groep verbruikt = orde<br>• 61% tot 80% van de groep verbruikt = waarschuwing of Monitor</br>• Meer dan 80% groep verbruikt = Kritiek of Out of Spec|
| Geheugen - wisselbaar geheugen: Bytes |• Minder dan 60% van de groep verbruikt = orde</br>• 61% tot 80% van de groep verbruikt = waarschuwing of Monitor</br>• Meer dan 80% groep verbruikt = Kritiek of Out of Spec|
| Geheugen--MB beschikbaar| • 50% van het beschikbare geheugen of meer = orde</br>• 25% van het beschikbare geheugen = Monitor</br>• 10% van het beschikbare geheugen = waarschuwing</br>• Minder dan 100 MB of 5% van het beschikbare geheugen = Kritiek of Out of Spec|
|Processor--\%processortijd (alle exemplaren)|• Minder dan 60% verbruikt = orde</br>• 61% tot 90% verbruikt = Monitor of de waarschuwing</br>• 91% en 100% verbruikt = kritiek|


> [AZURE.NOTE] Als u vaststelt dat de infrastructuur, de boosdoener is, wordt u aangeraden het defragmenteren van de schijven regelmatig voor betere prestaties.

<a id="cause2"></a>
## <a name="cause-another-process-or-antivirus-software-interfering-with-azure-backup"></a>Oorzaak: Een ander proces of antivirussoftware storen Azure back-up

We hebt meerdere gevallen waar andere processen in het Windows-systeem een negatieve invloed prestaties van het agent-proces Azure back-up ondervinden hebben gezien. Als u zowel de Azure Backup agent en een ander programma gebruiken om back-up van gegevens of als antivirussoftware wordt uitgevoerd en een vergrendeling op bestanden die u heeft wilt back-up worden gemaakt, de meervoudige vergrendelingen op kunnen bestanden conflicten veroorzaken. In deze situatie mislukken van de back-up of de taak kan langer duren dan verwacht.

Aanbeveling in dit scenario is het uitschakelen van het back-upprogramma om te zien of de back-tijd voor de Azure reservekopie-agent verandert. Ervoor te zorgen dat meerdere back-uptaken niet worden uitgevoerd op hetzelfde moment is meestal voldoende om te voorkomen dat ze elkaar beïnvloeden.

Voor antivirusprogramma's, raden we aan dat u de volgende bestanden en locaties uitsluiten:

- C:\Program Files\Microsoft Azure Recovery Services Agent\bin\cbengine.exe als een proces
- C:\Program Files\Microsoft Azure Recovery Services Agent\ mappen
- Scratch-locatie (als u niet de standaard locatie)

<a id="cause3"></a>
## <a name="cause-backup-agent-running-on-an-azure-virtual-machine"></a>Oorzaak: Back-up agent wordt uitgevoerd op een Azure virtuele machine

Als u de reservekopie-agent op een VM, zijn prestaties zijn langzamer dan wanneer u deze op een fysieke machine uitvoeren. Dit is normaal als gevolg van beperkingen IOP's.  U kunt echter de prestaties optimaliseren door over te schakelen van de gegevensstations die back-up naar Azure Premium opslag gemaakt zijn. We proberen op het oplossen van dit probleem en de oplossing in een toekomstige release beschikbaar zal zijn.

<a id="cause4"></a>
## <a name="cause-backing-up-a-large-number-millions-of-files"></a>Oorzaak: Back-up van bestanden een groot getal (in miljoenen)

Verplaatsen van een grote hoeveelheid gegevens duurt langer dan een kleinere hoeveelheid gegevens te verplaatsen. Back-up is in sommige gevallen gerelateerd aan niet alleen de grootte van de gegevens, maar ook het aantal bestanden of mappen. Dit is met name het geval als miljoenen kleine bestanden (een paar bytes naar een paar kilobytes) zijn back-up gemaakt.

Dit probleem treedt op omdat terwijl u back-ups van de gegevens en verplaatsen naar Azure, Azure is tegelijkertijd bestanden catalogiseren. In sommige zeldzame gevallen kan de catalogusbewerking duurt mogelijk langer dan verwacht.

De volgende indicatoren kunt u het knelpunt begrijpen en dienovereenkomstig werken op de volgende stappen uit:

- **Gebruikersinterface van de voortgang van de overdracht van gegevens wordt weergegeven**. De gegevens worden nog steeds overgebracht. De bandbreedte van het netwerk of de grootte van gegevens kan worden veroorzaakt door vertragingen.

- **UI, de voortgang van de overdracht van gegevens niet weergegeven**. Open de logboekbestanden te vinden op C:\Microsoft Azure Recovery Services Agent\Temp en controleer of de vermelding FileProvider::EndData in de logboeken. Deze vermelding geeft aan dat de overdracht van gegevens is voltooid en de catalogusbewerking plaatsvindt. Geen back-uptaken annuleren. In plaats daarvan een beetje langer wachten voor de catalogusbewerking te voltooien. Als het probleem zich blijft voordoen, neem contact op met [ondersteuning van Azure](https://portal.azure.com/#create/Microsoft.Support).
