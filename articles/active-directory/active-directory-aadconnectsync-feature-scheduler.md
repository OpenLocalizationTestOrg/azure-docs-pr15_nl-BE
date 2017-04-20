<properties
   pageTitle="Azure AD verbinden sync: planner | Microsoft Azure"
   description="Dit onderwerp beschrijft de functie ingebouwde scheduler in Azure AD verbinden sync."
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/04/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-sync-scheduler"></a>Azure AD verbinden sync: planner
Dit onderwerp beschrijft de ingebouwde scheduler synchroon Azure AD verbinden (ook synchronisatie-engine).

Deze functie is geïntroduceerd met build 1.1.105.0 (februari 2016 uitgebracht).

## <a name="overview"></a>Overzicht
Azure AD verbinden sync worden wijzigingen in de map op locatie is met een scheduler gebeurt gesynchroniseerd. Er zijn twee scheduler processen, een voor het wachtwoord synchronisatie en een voor het kenmerk object/sync en onderhoudstaken. In dit onderwerp wordt uitgelegd hoe deze laatste.

In eerdere versies werd de planner voor objecten en kenmerken buiten de synchronisatie-engine en de Taakplanner van Windows of een afzonderlijke Windows-service werd gebruikt voor het starten van het synchronisatieproces. De scheduler is met de ingebouwde 1.1 releases voor de synchronisatie van de motor en zorg ervoor dat enige aanpassing. De nieuwe standaard Synchronisatiefrequentie is 30 minuten.

De planner is verantwoordelijk voor twee taken:

- **Synchronisatie-cyclus**. Tijdens het importeren en exporteren van wijzigingen synchroniseren.
- **Onderhoudstaken uitvoeren**. Sleutels en certificaten voor het opnieuw instellen van wachtwoorden en apparaat registratie Service (DRS) vernieuwen. Opschonen van oude vermeldingen in het logboek voor bewerkingen.

De scheduler zelf is altijd actief, maar kan worden geconfigureerd om alleen een of geen van deze taken worden uitgevoerd. Bijvoorbeeld als u uw eigen synchronisatieproces cyclus hebt wilt, kunt u deze taak in de Taakplanner uitschakelen maar nog steeds uitvoeren van de onderhoudstaak.

## <a name="scheduler-configuration"></a>Taakplanner configureren
De huidige configuratieinstellingen Ga PowerShell en run `Get-ADSyncScheduler`. Het zal laten zien er ongeveer als volgt:

![GetSyncScheduler](./media/active-directory-aadconnectsync-feature-scheduler/getsynccyclesettings.png)

Als **de synchronisatieopdracht of cmdlet is niet beschikbaar** wanneer u deze cmdlet uitvoert, is niet de PowerShell-module geladen. Dit kan gebeuren als u verbinden met Azure AD op een domeincontroller of op een server met hogere niveaus van PowerShell beperking dan de standaardinstellingen uitvoert. Als dit foutbericht wordt weergegeven, voert u `Import-Module ADSync` de cmdlet beschikbaar te maken.

- **AllowedSyncCycleInterval**. Het meest kunt Azure AD synchronisaties met optreden. U vaker dan dit kan niet worden gesynchroniseerd en worden nog steeds ondersteund.
- **CurrentlyEffectiveSyncCycleInterval**. Het schema dat momenteel van kracht. Heeft dezelfde waarde als de CustomizedSyncInterval (indien ingesteld) als het niet vaker dan AllowedSyncInterval. Als u de CustomizedSyncCycleInterval wijzigt, dit wordt van kracht na de volgende cyclus van de synchronisatie.
- **CustomizedSyncCycleInterval**. Als u de Taakplanner worden uitgevoerd op een andere frequentie dan de standaard 30 minuten, kunt u deze instelling configureert. In de afbeelding hierboven de scheduler is ingesteld in plaats daarvan elk uur uitgevoerd. Als u deze waarde lager is dan AllowedSyncInterval, worden deze laatste gebruikt.
- **NextSyncCyclePolicyType**. Delta of eerste initiaal. Wordt gedefinieerd als de volgende keer wordt uitgevoerd, alleen deltawijzigingen moet, of de volgende sessie moet doen als een volledige importeren en synchroniseren, die ook opnieuw een nieuwe of gewijzigde regels zou verwerken.
- **NextSyncCycleStartTimeInUTC**. Zodra de planner begint de volgende cyclus van de synchronisatie.
- **PurgeRunHistoryInterval**. De tijd moeten logboeken worden bewaard. Deze kunnen worden gecontroleerd in de synchronisatie servicemanager. De standaardwaarde is te houden voor 7 dagen.
- **SyncCycleEnabled**. Geeft aan dat als de planner de import, synchronisatie en processen exporteren wordt uitgevoerd als onderdeel van de werking ervan.
- **MaintenanceEnabled**. Geeft als het proces onderhoud is ingeschakeld. De certificaten/sleutels werken worden en het activiteiten logboek leegmaken.
- **IsStagingModeEnabled**. Geeft als [staging-modus](active-directory-aadconnectsync-operations.md#staging-mode) is ingeschakeld.

Kunt u sommige van deze instellingen met `Set-ADSyncScheduler`. De volgende parameters kunnen worden gewijzigd:

- CustomizedSyncCycleInterval
- NextSyncCyclePolicyType
- PurgeRunHistoryInterval
- SyncCycleEnabled
- MaintenanceEnabled

De configuratie voor Taakplanner wordt opgeslagen in Azure AD. Als u een testserver hebt, wordt de staging-server (met uitzondering van IsStagingModeEnabled) ook gevolgen voor elke wijziging van de primaire server.

### <a name="customizedsynccycleinterval"></a>CustomizedSyncCycleInterval
Syntaxis:`Set-ADSyncScheduler -CustomizedSyncCycleInterval d.HH:mm:ss`  
d - dagen, uu - uur, mm - minuten, ss - seconden

Voorbeeld:`Set-ADSyncScheduler -CustomizedSyncCycleInterval 03:00:00`  
Verandert de scheduler aan elke drie uur uitgevoerd.

Voorbeeld:`Set-ADSyncScheduler -CustomizedSyncCycleInterval 1.0:0:0`  
De scheduler dagelijks verandert.

## <a name="start-the-scheduler"></a>Start de planner
De scheduler wordt standaard elke 30 minuten uitgevoerd. In sommige gevallen wilt u mogelijk een cyclus van de synchronisatie tussen de geplande cycli worden uitgevoerd of moet u een ander type uitvoeren.

**Delta sync cyclus**  
Een delta sync cyclus omvat de volgende stappen uit:

- Delta importeren op alle verbindingslijnen
- Delta sync op alle verbindingslijnen
- Op alle verbindingen exporteren

Kan het zijn dat u hebt dringende wijzigen die moeten worden gesynchroniseerd onmiddellijk dat is de reden waarom moet u handmatig uitvoeren van een cyclus. Als u handmatig uitvoeren een cyclus, vervolgens vanuit PowerShell uitvoeren `Start-ADSyncSyncCycle -PolicyType Delta`.

**Volledige synchronisatie cyclus**  
Als u een van de volgende configuratiewijzigingen hebt aangebracht, moet u een volledige synchronisatie-cyclus (ook uitvoeren Eerste):

- Meer objecten of -kenmerken te importeren uit een bronmap toegevoegd
- Wijzigingen aangebracht in de regels voor synchronisatie
- [Filter](active-directory-aadconnectsync-configure-filtering.md) gewijzigd zodat een verschillend aantal objecten opgenomen worden moet

Als u een van deze wijzigingen hebt aangebracht, moet u de cyclus van een volledige synchronisatie uitvoeren zodat de synchronisatie-engine heeft de gelegenheid de connector ruimten opnieuw samenvoegen. Een volledige synchronisatie cyclus omvat de volgende stappen uit:

- Volledige importbewerking op alle verbindingslijnen
- Volledige synchronisatie van alle verbindingslijnen
- Op alle verbindingen exporteren

Uitvoeren om een volledige synchronisatie cyclus, `Start-ADSyncSyncCycle -PolicyType Initial` van PowerShell-prompt. Hiermee start u een volledige synchronisatie-cyclus.

## <a name="stop-the-scheduler"></a>De scheduler stoppen
Als de scheduler wordt uitgevoerd een synchronisatie cyclus die mogelijk moet u het stopt. Als u de installatiewizard te starten en u krijgt deze fout bijvoorbeeld:

![SyncCycleRunningError](./media/active-directory-aadconnectsync-feature-scheduler/synccyclerunningerror.png)

Wanneer een cyclus synchronisatie wordt uitgevoerd, kunt u geen configuratiewijzigingen aanbrengen. U kan wachten tot de scheduler het proces is voltooid, maar u deze ook uitschakelen kunt zodat u kunt uw wijzigingen onmiddellijk. De huidige cyclus gestopt is niet schadelijk en wijzigingen die nog niet verwerkt worden verwerkt met de volgende keer wordt uitgevoerd.

1. Start door te vertellen de scheduler stoppen van de huidige cyclus met de PowerShell-cmdlet `Stop-ADSyncSyncCycle`.
2. De Taakplanner wordt gestopt, wordt de huidige Connector van de huidige taak niet gestopt. Als u wilt dat de verbindingslijn om te stoppen, de volgende acties uitvoeren: ![StopAConnector](./media/active-directory-aadconnectsync-feature-scheduler/stopaconnector.png)
    - **Synchronisatie** starten vanuit het startmenu. Ga naar **Connectors**, selecteert u de verbindingslijn met de status **actief** en selecteer **stoppen** van de acties.

De scheduler nog steeds actief is en wordt opnieuw gestart op de volgende gelegenheid.

## <a name="custom-scheduler"></a>Aangepaste scheduler
De cmdlets beschreven in deze sectie zijn alleen beschikbaar in build [1.1.130.0](active-directory-aadconnect-version-history.md#111300) en hoger.

Als de ingebouwde scheduler niet voldoet aan uw behoeften, kunt u de verbindingslijnen met PowerShell plannen.

### <a name="invoke-adsyncrunprofile"></a>Aanroepen van ADSyncRunProfile
U kunt een profiel voor een verbindingslijn op deze manier starten:

```
Invoke-ADSyncRunProfile -ConnectorName "name of connector" -RunProfileName "name of profile"
```

De namen voor [de namen van de Connector](active-directory-aadconnectsync-service-manager-ui-connectors.md) en [profiel uitvoeren](active-directory-aadconnectsync-service-manager-ui-connectors.md#configure-run-profiles) vindt u in de [UI Service Synchronisatiebeheer](active-directory-aadconnectsync-service-manager-ui.md).

![Aanroepen uitvoeren profiel](./media/active-directory-aadconnectsync-feature-scheduler/invokerunprofile.png)  

De `Invoke-ADSyncRunProfile` cmdlet is synchroon, dat wil zeggen wordt niet geretourneerd besturingselement totdat de verbindingslijn de bewerking heeft voltooid is of met een fout.

Wanneer u uw verbindingslijnen plant, is de aanbeveling deze plannen in de volgende volgorde:

1. (Volledige/Delta) Importeren uit de mappen voor ruimten, zoals Active Directory
2. (Volledige/Delta) Importeren vanuit Azure AD
3. (Volledige/Delta) De synchronisatie van mappen voor ruimten, zoals Active Directory
4. (Volledige/Delta) Synchronisatie van Azure AD
5. Exporteren naar Azure AD
6. Exporteren naar mappen voor ruimten, zoals Active Directory

Als u naar de ingebouwde scheduler kijkt, is dit de volgorde die de verbindingslijnen worden uitgevoerd.

### <a name="get-adsyncconnectorrunstatus"></a>Get-ADSyncConnectorRunStatus
U kunt ook controleren de synchronisatie-engine als bezet of niet actief is. Deze cmdlet retourneert een leeg resultaat als de synchronisatie-engine niet actief is en een verbindingslijn wordt niet uitgevoerd. Als u een verbindingslijn wordt uitgevoerd, wordt de naam van de Connector worden geretourneerd.

```
Get-ADSyncConnectorRunStatus
```

![Connector uitvoeren Status](./media/active-directory-aadconnectsync-feature-scheduler/getconnectorrunstatus.png)  
In de bovenstaande afbeelding is de eerste regel van een staat waar de synchronisatie-engine niet actief is. De tweede regel uit wanneer de Azure AD-Connector wordt uitgevoerd.

## <a name="scheduler-and-installation-wizard"></a>Taakplanner en de installatie-wizard
Als u de installatiewizard start, wordt vervolgens de scheduler tijdelijk opgeschort. Dit komt omdat wordt aangenomen brengt u wijzigingen in de configuratie en deze kunnen niet worden toegepast als de synchronisatie-engine actief wordt uitgevoerd. Om deze reden laat niet de installatiewizard geopend sinds stopt de motor synchroniseren synchronisatie acties kan uitvoeren.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over het configureren van [Azure AD Connect worden gesynchroniseerd](active-directory-aadconnectsync-whatis.md) .

Meer informatie over de [integratie van uw identiteiten op ruimten met Azure Active Directory](active-directory-aadconnect.md).
