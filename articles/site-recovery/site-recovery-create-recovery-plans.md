<properties
    pageTitle="De herstelplannen maken | Microsoft Azure" 
    description="De herstelplannen maken met Azure Site herstellen om failover en groepen van virtuele machines en fysieke servers te herstellen." 
    services="site-recovery" 
    documentationCenter="" 
    authors="rayne-wiselman" 
    manager="jwhit" 
    editor=""/>

<tags 
    ms.service="site-recovery" 
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery" 
    ms.date="10/05/2016"
    ms.author="raynew"/>

# <a name="create-recovery-plans"></a>De herstelplannen maken

De Azure-Site-Recovery-service draagt bij aan de continuïteit en noodherstel recovery (BCDR) bedrijfsstrategie door regie van replicatie, failover en herstel van virtuele machines en fysieke servers. Machines kunnen worden gerepliceerd naar Azure, of naar een secundaire op ruimten Datacenter. Voor een snel overzicht lezen [Wat is Azure Site terugzetten?](site-recovery-overview.md).


## <a name="overview"></a>Overzicht

Dit artikel bevat informatie over het maken en aanpassen van de herstelplannen. 

De herstelplannen bestaan uit een of meer bestelde groepen met virtuele machines of fysieke servers die u wilt samen failover. De herstelplannen als volgt:

- Definieer groepen van machines die failover en tegelijk van start.
- Model afhankelijkheden tussen computers door te groeperen in een groep recovery plan. Voor bijvoorbeeld als u wilt een failover en brengen van een bepaalde toepassing de virtuele machines voor de toepassing in dezelfde recovery plan groep groeperen.
- Automatiseren en uitbreiden van failover. U kunt een test, gepland, of niet-geplande failover uitvoeren op een herstelplan. U kunt de herstelplannen met scripts, Azure automatiseren en handmatige handelingen.

De herstelplannen worden weergegeven op de **Herstelplannen** in de portal-Site terugzetten.


Opmerkingen of vragen aan de onderkant van dit artikel, of op het [Forum van Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)boeken.

## <a name="before-you-start"></a>Voordat u begint

Let op het volgende:

- Een herstelplan niet moet VMs mengen met één of meerdere netwerkadapters. Dit is omdat deze VMs mengen in een Azure cloud-service niet is toegestaan.
- U kunt de herstelplannen met scripts en handmatige acties uitbreiden. Let op het volgende:
    - Met Windows PowerShell scripts schrijven.
    - Ervoor zorgen dat scripts try-catch-blokken gebruiken zodat de uitzonderingen worden afgehandeld, zonder problemen. Er is een uitzondering in het script stopt de uitvoering als de taak wordt weergegeven als mislukt.  Als er een fout voordoet zich, wordt elk resterend deel van het script niet uitgevoerd. Als dit gebeurt wanneer u een niet-geplande failover worden uitgevoerd, wordt het herstelplan voortgezet. Als dit gebeurt wanneer u een geplande failover wordt uitgevoerd, stopt het herstelplan. Als dit gebeurt, herstellen van het script, zorg ervoor dat deze wordt uitgevoerd zoals verwacht en het herstelplan vervolgens opnieuw uitvoeren.
    - De opdracht Write-Host werkt niet in een script recovery plan en het script mislukt. Als u maken van de uitvoer wilt, maakt een proxyscript dat op zijn beurt de belangrijkste script wordt uitgevoerd en ervoor te zorgen dat alle uitvoer wordt doorgesluisd om via de >> opdracht.
    - Het script wordt geblokkeerd als het niet binnen 600 seconden.
    - Als uit naar STDERR wordt geschreven, wordt het script als mislukt worden ingedeeld. Deze informatie wordt weergegeven in de details van de uitvoering van script.
    - Houd rekening met het volgende als u in uw implementatie VMM:

        - Scripts in een herstelplan worden uitgevoerd in de context van de VMM-account. Zorg ervoor dat deze account de machtiging lezen heeft voor de externe share waarop het script opgeslagen is, en test het script uit te voeren op het niveau van VMM service account machtiging.
        - VMM-cmdlets worden geleverd in een module voor Windows PowerShell. De module VMM Windows PowerShell is geïnstalleerd tijdens de installatie van de VMM-console. De VMM-module in uw script met de volgende opdracht in het script kan worden geladen: Import-Module-naam virtualmachinemanager. Voor [meer informatie](hhttps://technet.microsoft.com/library/hh875013.aspx).
        - Zorg er ten minste één server van de bibliotheek in uw implementatie van VMM. Het pad naar share bibliotheek voor een VMM-server bevindt zich standaard lokaal op de VMM-server met de naam van de map MSCVMMLibrary.
        - Als uw bibliotheekpad RAS is (of lokale maar wordt niet gedeeld met MSCVMMLibrary, de share als volgt configureren (met \\libserver2.contoso.com\share\ als voorbeeld):
            - Open de Register-Editor en Ga naar HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\Azure Site Recovery\Registration.
            -  Bewerk de waarde ScriptLibraryPath en plaats de waarde als \\libserver2.contoso.com\share\. de volledige FQDN opgeven. Machtigingen voor de gedeelde locatie bieden.
            -  Zorg ervoor dat het script met een gebruikersaccount met dezelfde machtigingen als de serviceaccount VMM om ervoor te zorgen dat zelfstandige geteste scripts worden uitgevoerd op dezelfde manier dat zij in de herstelplannen worden getest. Instellen op de server VMM het uitvoeringsbeleid als volgt omzeilen:
                -  Open de console van 64-bits Windows PowerShell met verhoogde bevoegdheden.
                -  Type: **Set uitvoeringsbeleid overslaan**. Voor [meer informatie](https://technet.microsoft.com/library/ee176961.aspx).

## <a name="create-a-recovery-plan"></a>Maak een herstelplan

De manier waarop u een herstelplan is afhankelijk van uw implementatie van het herstel van de Site.

- **VMware VMs repliceren en fysieke servers**, u een plan maken en toevoegen van replicatiegroepen die virtuele machines en fysieke servers op een herstelplan bevatten.
- **Hyper-V VMs repliceren (in de wolken VMM)**— u een plan maken en beveiligde Hyper-V virtuele machines in een wolk VMM toevoegen aan een herstelplan.
- **Hyper-V VMs repliceren (niet in de wolken VMM)**— Maak een plan en beveiligde Hyper-V virtuele machines uit een groep beveiliging toevoegen aan een herstelplan.
- **SAN-replicatie**: een plan maken en toevoegen van een replicatiegroep met het herstelplan voor virtuele machines. Selecteer een replicatiegroep in plaats van specifieke virtuele machines omdat alle virtuele machines in een replicatiegroep moeten samen failover (failover plaatsvindt op de opslaglaag eerst).


Een herstelplan als volgt maken:

1. Klik op tabblad **Recovery plan** > **Herstelplan maken**.
Geef een naam voor het herstelplan en een bron en doel. De bronserver hebt virtuele machines die zijn ingeschakeld voor failover en herstel.

    - Als u die van VMM VMM repliceren bent **Type gegevensbron**selecteert > **VMM**en de bron- en doeldomein VMM-servers. Klik op **Hyper-V** om te zien wolken die zijn geconfigureerd voor het gebruik van Hyper-V-Replica. 
    - Als u bent die repliceren van VMM VMM SAN select **Source Type** > **VMM**en de bron- en doeldomein VMM-servers. Klik op **SAN** overzicht wolken die zijn geconfigureerd voor de replicatie van SAN.
    - Als u die van VMM Azure repliceren bent **Type gegevensbron**selecteert > **VMM**.  Selecteer de bron VMM- en **Azure** als doel.
    - Als u vanaf een site voor Hyper-V repliceren wilt **Type gegevensbron**selecteert > **site van Hyper-V**. Selecteer de site als de bron- en **Azure **als doel.
    - Als u van VMware of een fysieke op vestigingen server naar Azure repliceert bent, selecteert u een server configureren als de bron- en **Azure** als doel

2. Selecteer de virtuele machines (of de groep voor replicatie) die u wilt toevoegen aan de standaard-groep (groep 1) in het herstelplan in **virtuele machines te selecteren** .

## <a name="customize-recovery-plans"></a>De herstelplannen aanpassen

Nadat u beveiligde virtuele machines of replicatiegroepen aan de groep standaard recovery plan en het plan dat kunt u deze aanpassen gemaakt hebt toegevoegd:

- **Nieuwe groepen toevoegen**, kunt u extra recovery planningsgroepen toevoegen. Groepen die u toevoegt, worden genummerd in de volgorde waarin u ze toevoegt. U kunt maximaal zeven groepen toevoegen. U kunt meer machines of replicatiegroepen aan deze nieuwe groepen toevoegen. Houd er rekening mee dat een virtuele machine of een replicatiegroep alleen kan worden opgenomen in de groep van een recovery plan.
- **Een script toevoegen **, kunt u scripts die voor of na een herstel groep toevoegen. Wanneer u een script toevoegen dat wordt een nieuwe set van acties voor de groep toegevoegd. Bijvoorbeeld een set vooraf stappen voor groep 1 wordt gemaakt met de naam: groep 1: stappen vooraf. Alle voorlopige maatregelen wordt in deze set weergegeven. Houd er rekening mee dat u alleen een script op de primaire site toevoegen kunt hebt u een VMM server geïmplementeerd.
- **Een handmatige actie toevoegen**, kunt u handmatig acties die worden uitgevoerd voor of na een recovery plan groep toevoegen. Het herstelplan wordt uitgevoerd, stopt op het punt waar u de actie handmatig ingevoegd als een dialoogvenster wordt gevraagd om aan te geven dat de handmatige actie is voltooid.

## <a name="extend-recovery-plans-with-scripts"></a>Uitbreiden van de herstelplannen met behulp van scripts

U kunt een script toevoegen aan uw herstelplan:

- Als u een site van de bron VMM kunt u een script op de server VMM maken en opnemen in uw herstelplan.
- Als u naar Azure repliceren wilt kunt u Azure automatisering runbooks integreren in uw herstelplan

### <a name="create-a-vmm-script"></a>Een VMM-script maken


Het script als volgt maken:

1. Maak een nieuwe map in de bibliotheek-share, bijvoorbeeld \<VMMServerName > \MSSCVMMLibrary\RPScripts. Plaats op de bron- en doelservers VMM.
2. Maak het script (bijvoorbeeld RPScript) en controleer dat het werkt zoals verwacht.
3. Plaats het script op de locatie \<VMMServerName > \MSSCVMMLibrary op de bron- en doeldomein VMM-servers.

### <a name="create-an-azure-automation-runbook"></a>Maak een runbook Azure automatisering

U kunt uw herstelplan uitbreiden door een Azure automatisering runbook uitgevoerd als onderdeel van het plan. [Lees meer](site-recovery-runbook-automation.md).


### <a name="add-custom-settings-to-a-recovery-plan"></a>Aangepaste instellingen toevoegen aan een herstelplan

1. Open het herstelplan dat u wilt aanpassen.
2. Klik hier om een virtuele machines of een nieuwe groep.
3. Een script of handmatige actie klikt u op een item in de lijst van **stap** toevoegen en klik vervolgens op **Handmatige actie**of **Script** . Geef op of het script of de actie vóór of na het geselecteerde item wilt toevoegen. Gebruik de knoppen **Omhoog** en **Omlaag** om de positie van het script omhoog of omlaag te verplaatsen.
4. Als u een script VMM toevoegt, selecteert u **Failover naar VMM script**en typ in **Pad naar Script** in het relatieve pad naar de share. In dat geval in ons voorbeeld waar de share bevindt zich \\ <VMMServerName>\MSSCVMMLibrary\RPScripts, geef het pad op: \RPScripts\RPScript.PS1.
5. Als u een Azure automatisering boek uitvoeren toevoegen wilt, geef **Azure automatisering Account** waarin de runbook zich bevindt en selecteer het juiste **Azure Runbook Script**.
5. Voer een failover van het herstelplan om ervoor te zorgen dat het script werkt zoals verwacht.


## <a name="next-steps"></a>Volgende stappen

U kunt verschillende soorten failover uitvoeren op herstelplan, met inbegrip van een failover testen om te controleren van uw omgeving en failover of niet gepland. [Meer informatie](site-recovery-failover.md).


 
