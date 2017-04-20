<properties
   pageTitle="Azure AD verbinden: Upgrade van een vorige versie | Microsoft Azure"
   description="De verschillende methoden om te upgraden naar de nieuwste versie van Azure Active Directory verbinding maken, inclusief in-place ' upgrade en migratie van swing uitgelegd."
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
   ms.workload="Identity"
   ms.date="10/12/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-upgrade-from-a-previous-version-to-the-latest"></a>Azure AD verbinden: Upgrade van een vorige versie naar de nieuwste
In dit onderwerp worden de verschillende methoden die kunt u uw Azure AD Connect upgrade naar de meest recente versie. Wij raden aan dat u zelf actueel met de release van Azure AD verbinden houden. De stappen in het [migratie swing](#swing-migration) worden ook gebruikt wanneer u een belangrijke configuratie wijzigen.

Als u upgraden vanuit DirSync wilt, Zie [upgraden van Azure AD synchronisatietool (DirSync)](./connect/active-directory-aadconnect-dirsync-upgrade-get-started.md) in plaats daarvan.

Er zijn een paar verschillende strategieën Azure AD verbinding bijwerken.

Methode | Beschrijving
--- | ---
[Automatische upgrade](active-directory-aadconnect-feature-automatic-upgrade.md) | Voor klanten met een snelle installatie is dit de eenvoudigste methode.
[In-place ' upgrade](#in-place-upgrade) | Als u één server hebt, werken de installatie ter plekke op dezelfde server.
[Swing-migratie](#swing-migration) | U kunt met twee servers voorbereiden op een van de servers met de nieuwe release of configuratie en active server wijzigen wanneer u klaar bent.

Voor de vereiste machtigingen Zie [machtigingen vereist voor de upgrade](./connect/active-directory-aadconnect-accounts-permissions.md#upgrade).

## <a name="in-place-upgrade"></a>In-place ' upgrade
Een in-place upgrade werkt voor het verplaatsen van Azure AD Sync of Azure AD verbinden. Het werkt niet voor DirSync of voor een oplossing met de FIM + Azure AD-Connector.

Deze methode heeft de voorkeur als u beschikt over één server en minder dan ongeveer 100.000 objecten. Als er wijzigingen in de regels van de out-of-box sync, een volledige import en een volledige synchronisatie optreden na de upgrade. Dit zorgt ervoor dat de nieuwe configuratie wordt toegepast op alle bestaande objecten in het systeem. Dit duurt een paar uur, afhankelijk van het aantal objecten in een bereik van de synchronisatie-engine. De normale delta synchronisatie-scheduler standaard elke 30 minuten wordt onderbroken, maar wel Wachtwoordsynchronisatie. U kunt de upgrade ter plaatse tijdens een weekend. Als er geen wijzigingen in de configuratie van de out-of-box met de nieuwe release van Azure AD verbinden, wordt in plaats daarvan een normale delta import/synchronisatie starten.  
![In-Place Upgrade](./media/active-directory-aadconnect-upgrade-previous-version/inplaceupgrade.png)

Als u out-of-box-synchronisatieregels hebt gewijzigd, wordt deze opnieuw ingesteld standaardconfiguratie op de upgrade. Om te controleren of dat de configuratie tussen upgrades wordt gehouden, moet dat de wijzigingen worden aangebracht, zoals beschreven in [Aanbevolen procedures voor het wijzigen van de standaardconfiguratie](active-directory-aadconnectsync-best-practices-changing-default-configuration.md).

## <a name="swing-migration"></a>Swing-migratie
Als u een complexe implementatie of een zeer groot aantal objecten mogelijk niet praktisch om te doen een in-place upgrade van live systeem. Dit kan voor sommige klanten meerdere dagen duren en gedurende deze periode geen deltawijzigingen worden verwerkt. Met deze methode wordt ook gebruikt wanneer u van plan bent een substantiële wijzigingen aanbrengen in de configuratie en u wilt uit proberen, voordat deze worden doorgegeven naar de cloud.

De aanbevolen methode voor deze scenario's is het gebruik van een migratie swing. U moet (ten minste) twee servers, een actief en een staging-server. De actieve server (ononderbroken blauwe lijnen in de volgende afbeelding) is verantwoordelijk voor de productie van actieve belasting. De staging-server (paarse stippellijnen in de volgende afbeelding) wordt bereid met de nieuwe release of configuratie en volledig gereed is, wordt deze server actief gemaakt. De vorige actieve server nu met de oude versie of de configuratie is geïnstalleerd, wordt de staging-server wordt gemaakt en bijgewerkt.

De twee servers kunnen verschillende versies te gebruiken. Bijvoorbeeld de actieve server die u van plan bent te ontmantelen Azure AD Sync kunt gebruiken en de nieuwe testserver Azure AD verbinden kunt gebruiken. Als u de swing migratie gebruiken voor het ontwikkelen van een nieuwe configuratie is een goed idee om dezelfde versies hebben op twee servers.  
![Staging-server](./media/active-directory-aadconnect-upgrade-previous-version/stagingserver1.png)

Opmerking: Er is geconstateerd dat sommige klanten liever drie of vier servers voor dit scenario. Wanneer de staging-server een upgrade is uitgevoerd, hoeft u niet een back-up server bij een [Noodherstel](active-directory-aadconnectsync-operations.md#disaster-recovery). Met drie of vier servers, kan één set primary/standby servers met de nieuwe versie worden voorbereid, ervoor te zorgen dat zijn altijd een testserver wilt overnemen.

Deze stappen werkt ook als van Azure AD Sync of een oplossing met de FIM + Azure AD verbindingslijn wilt verplaatsen. Deze stappen werken niet voor DirSync, maar dezelfde migratie (ook wel een parallelle installatie genoemd) methode met stappen voor DirSync vindt u in [Upgraden Azure Active Directory synchroniseren (DirSync)](./connect/active-directory-aadconnect-dirsync-upgrade-get-started.md).

### <a name="swing-migration-steps"></a>Swing migratiestappen

1. Als u Azure AD verbinden op beide servers gebruiken en alleen een wijziging in de configuratie uitvoeren, zorg ervoor dat de actieve server en staging-server beide gebruiken dezelfde versie. Die gemakkelijker later verschillen vergelijken. Als u een van Azure AD Sync upgrade, hebben deze servers verschillende versies. Als u een upgrade van een oudere versie van Azure AD verbinden uitvoert, is het verstandig om te beginnen met de twee servers met behulp van dezelfde versie, maar is niet vereist.
2. Als u een aangepaste configuratie hebt aangebracht en de staging-server beschikt niet over, volg de stappen onder [aangepaste configuratie van actief naar de staging-server verplaatsen](#move-custom-configuration-from-active-to-staging-server).
3. Als u een upgrade van een eerdere versie van Azure AD Connect uitvoert upgrade de staging-server naar de nieuwste versie. Als u van Azure AD Sync verplaatst, kunt u vervolgens Azure AD verbinden op de staging-server installeren.
4. Laat de synchronisatie-engine volledige importbewerking en een volledige synchronisatie uitvoeren op uw testserver.
5. Controleer of de nieuwe configuratie er onverwachte wijzigingen in [de configuratie van een server controleren](active-directory-aadconnectsync-operations.md#verify-the-configuration-of-a-server)met behulp van de stappen onder **controle** hebben. Als er iets is niet zoals verwacht, correct, run importeren en synchroniseren, en controleer of totdat de gegevens er goed uitziet. Deze stappen kunnen in het gekoppelde onderwerp vinden.
6. Activeer de staging-server als u de actieve server. Dit is de laatste stap **Ga active server** in [de configuratie van een server controleren](active-directory-aadconnectsync-operations.md#verify-the-configuration-of-a-server).
7. Als u Azure AD verbinden upgraden wilt, werk de server nu in de staging-modus naar de meest recente versie. Volg de stappen hierboven om de gegevens en configuratie bijgewerkt. Als u een upgrade hebt uitgevoerd van Azure AD Sync, kunt u nu uitschakelen en uw oude server buiten gebruik stellen.

### <a name="move-custom-configuration-from-active-to-staging-server"></a>Aangepaste configuratie van actieve naar de staging-server verplaatsen
Als u wijzigingen hebt aangebracht in de active server, moet u controleren of dat de wijzigingen worden toegepast op de staging-server.

De aangepaste synchronisatie-regels die u hebt gemaakt, kunnen worden verplaatst met PowerShell. Andere wijzigingen moeten worden toegepast op beide systemen dezelfde manier en kunnen niet worden gemigreerd.

U moet ervoor zorgen dat is geconfigureerd op beide servers dezelfde manier:

- Verbinding met de dezelfde bossen.
- Elk domein en elke organisatie-eenheid filteren.
- Dezelfde optionele functies, zoals synchronisatie van wachtwoord en wachtwoord Write-back.

**Synchronisatieregels verplaatsen**  
Als u een aangepaste synchronisatie-regel, het volgende doen:

1. **Synchronisatie regeleditor** openen op de actieve server.
2. Selecteer de aangepaste regel. Klik op **exporteren**. Dit brengt een Kladblok-venster. Het tijdelijke bestand opslaan met de extensie PS1. Dit maakt het een PowerShell-script. Kopieer het bestand ps1 naar de staging-server.  
![Sync regel exporteren](./media/active-directory-aadconnect-upgrade-previous-version/exportrule.png)
3. De Connector-GUID is anders op de staging-server en moet worden gewijzigd. Als u de GUID, **Regeleditor synchronisatie**starten, selecteert u een van de kant-en-klare regels die hetzelfde systeem verbonden en klik op **exporteren**. De GUID in het PS1-bestand vervangen door de GUID van de staging-server.
4. Voer de PS1-bestand in PowerShell-prompt. Hiermee maakt u de regel aangepaste synchronisatie op de staging-server.
5. Als u meerdere aangepaste regels hebt, herhaalt u voor alle aangepaste regels.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over de [integratie van uw identiteiten op ruimten met Azure Active Directory](active-directory-aadconnect.md).
