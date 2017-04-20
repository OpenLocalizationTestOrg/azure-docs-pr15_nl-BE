<properties
   pageTitle="Azure AD verbinden: Automatische upgrade | Microsoft Azure"
   description="Dit onderwerp beschrijft de ingebouwde automatische upgrade functie in Azure AD verbinden sync."
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
   ms.date="08/24/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-automatic-upgrade"></a>Azure AD verbinden: Automatische upgrade
Deze functie is geïntroduceerd met build 1.1.105.0 (februari 2016 uitgebracht).

## <a name="overview"></a>Overzicht
Zorg ervoor dat uw installatie Azure AD verbinden altijd up-to-date is is nooit gemakkelijker met de **Automatische upgrade** functie. Deze functie is standaard ingeschakeld voor express-installaties en upgrades voor DirSync. Wanneer een nieuwe versie wordt uitgebracht, wordt de installatie automatisch bijgewerkt.

Automatische upgrade is standaard ingeschakeld voor het volgende:

- Instellingen voor installatie en upgrades van DirSync Express.
- Met de SQL Express-LocalDB, dat is wat Express instellingen altijd gebruiken. DirSync met SQL Express ook LocalDB gebruiken.
- De AD-account is de standaardaccount MSOL_ Express instellingen en DirSync gemaakt.
- Minder dan 100.000 objecten hebben in de metaverse.

De huidige status van de automatische upgrade kan worden bekeken met de PowerShell-cmdlet `Get-ADSyncAutoUpgrade`. Heeft de volgende staten:

Staat | Opmerking
---- | ----
Ingeschakeld | Automatisch bijwerken is ingeschakeld.
Geschorst | Alleen het systeem instellen. Het systeem is niet langer in aanmerking voor automatische upgrades.
Uitgeschakeld | Automatische upgrade is uitgeschakeld.

U kunt schakelen tussen **ingeschakeld** en **uitgeschakeld** met `Set-ADSyncAutoUpgrade`. Alleen het systeem moet de status **onderbroken**instellen.

Automatische upgrade voor de infrastructuur upgrade gezondheid Azure AD verbinding gebruikt. Zorg ervoor dat u hebt de URL geopend in uw proxy-server voor de **Gezondheid verbinding Azure AD** zoals beschreven in [Office 365-URL's en IP-adresbereiken](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2)voor automatische upgrade werkt.

Als de **Service Synchronisatiebeheer** UI wordt uitgevoerd op de server, is vervolgens de upgrade geschorst totdat de UI wordt gesloten.

## <a name="troubleshooting"></a>Het oplossen van problemen
Als de installatie van uw verbinding niet zelf bijgewerkt zoals verwacht, vervolgens deze stappen om erachter te komen wat verkeerd kan zijn.

Eerst moet u de automatische upgrade naar de eerste dag die is een nieuwe versie uitgebracht worden geprobeerd niet verwachten. Er is een opzettelijke willekeurigheid voordat een upgrade is uitgevoerd, dus Schrik niet als de installatie is niet onmiddellijk bijgewerkt.

Als u denkt er iets niet klopt dat, vervolgens voert u eerst `Get-ADSyncAutoUpgrade` om automatisch bijwerken is ingeschakeld.

Controleer vervolgens of dat u de gewenste URL's in uw proxy- of firewall hebt geopend. Automatische updates maakt gebruik van Azure AD verbinding gezondheid zoals beschreven in het [Overzicht](#overview). Als u een proxy gebruikt, moet dat de gezondheid is geconfigureerd voor het gebruik van een [proxyserver](active-directory-aadconnect-health-agent-install.md#configure-azure-ad-connect-health-agents-to-use-http-proxy). Ook de [gezondheid connectiviteit](active-directory-aadconnect-health-agent-install.md#test-connectivity-to-azure-ad-connect-health-service) naar Azure AD testen.

Met de connectiviteit met Azure AD gecontroleerd, is het tijd om te zoeken in de gebeurtenislogboeken. Start Logboeken en kijk in het gebeurtenislogboek van de **toepassing** . Een gebeurtenislogboek filteren voor de bron **Azure AD Connect Upgrade** en de gebeurtenis-id bereik **300 399**toevoegen.  
![Eventlog-filter voor automatische upgrade](./media/active-directory-aadconnect-feature-automatic-upgrade/eventlogfilter.png)  

Nu ziet u de gebeurtenislogboeken die zijn gekoppeld aan de status van de automatische upgrade.  
![Eventlog-filter voor automatische upgrade](./media/active-directory-aadconnect-feature-automatic-upgrade/eventlogresult.png)  

De resultaatcode heeft een voorvoegsel met een overzicht van de staat.

Het voorvoegsel van de resultaten | Beschrijving
--- | ---
Succes | De installatie is met succes bijgewerkt.
UpgradeAborted | De upgrade is een tijdelijke voorwaarde gestopt. Er wordt opnieuw geprobeerd weer en de verwachting is dat dit later lukt.
UpgradeNotSupported | Het systeem heeft een configuratie die wordt geblokkeerd door het systeem automatisch wordt bijgewerkt. Er wordt opnieuw geprobeerd om te zien als de status wordt gewijzigd, maar de verwachting is dat het systeem handmatig moet worden bijgewerkt.

Hier volgt een lijst van de meest gangbare berichten die u kunt vinden. Deze lijst bevat niet alle, maar het weergegeven resultaat moet duidelijk aan wat het probleem is.

Bericht van de resultaten | Beschrijving
--- | ---
**UpgradeAborted** |
UpgradeAbortedCouldNotSetUpgradeMarker | Kan niet schrijven naar het register.
UpgradeAbortedInsufficientDatabasePermissions | De ingebouwde groep administrators geen machtigingen voor de database. Handmatig bijwerken naar de nieuwste versie van Azure AD verbinden om dit probleem te verhelpen.
UpgradeAbortedInsufficientDiskSpace | Er is onvoldoende schijfruimte om ondersteuning voor een upgrade.
UpgradeAbortedSecurityGroupsNotPresent | Kan niet vinden en oplossen van alle beveiligingsgroepen gebruikt door de synchronisatie-engine.
UpgradeAbortedServiceCanNotBeStarted | De NT-Service **Microsoft Azure AD synchronisatie** kan niet worden gestart.
UpgradeAbortedServiceCanNotBeStopped | De NT-Service **Microsoft Azure AD Sync** kan niet stoppen.
UpgradeAbortedServiceIsNotRunning | De NT-Service **Microsoft Azure AD Sync** is niet actief.
UpgradeAbortedSyncCycleDisabled | De optie SyncCycle in de [Taakplanner](active-directory-aadconnectsync-feature-scheduler.md) is uitgeschakeld.
UpgradeAbortedSyncExeInUse | De [service Synchronisatiebeheer UI](active-directory-aadconnectsync-service-manager-ui.md) is geopend op de server.
UpgradeAbortedSyncOrConfigurationInProgress | De installatiewizard wordt uitgevoerd of een synchronisatie buiten de Taakplanner is gepland.
**UpgradeNotSupported** |
UpgradeNotSupportedCustomizedSyncRules | U kunt uw eigen aangepaste regels hebt toegevoegd aan de configuratie.
UpgradeNotSupportedDeviceWritebackEnabled | U kunt het [apparaat Write-back](active-directory-aadconnect-feature-device-writeback.md) -functie hebt ingeschakeld.
UpgradeNotSupportedGroupWritebackEnabled | U kunt de [groep Write-back](active-directory-aadconnect-feature-preview.md#group-writeback) -functie hebt ingeschakeld.
UpgradeNotSupportedInvalidPersistedState | De installatie is niet een Express-instellingen of een upgrade DirSync.
UpgradeNotSupportedMetaverseSizeExceeeded | U hebt meer dan 100.000 objecten in de metaverse.
UpgradeNotSupportedMultiForestSetup | U gaat aansluiten op meer dan één forest. Snelle installatie maakt alleen verbinding met één forest.
UpgradeNotSupportedNonLocalDbInstall | Geen gebruik van een database van SQL Server Express-LocalDB.
UpgradeNotSupportedNonMsolAccount | De [Connector van de AD-account](active-directory-aadconnect-accounts-permissions.md#active-directory-account) is niet de standaard MSOL_-account niet meer.
UpgradeNotSupportedStagingModeEnabled | De server is ingesteld in de [staging-modus](active-directory-aadconnectsync-operations.md#staging-mode).
UpgradeNotSupportedUserWritebackEnabled | U kunt de [gebruiker Write-back](active-directory-aadconnect-feature-preview.md#user-writeback) -functie hebt ingeschakeld.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over de [integratie van uw identiteiten op ruimten met Azure Active Directory](active-directory-aadconnect.md).
