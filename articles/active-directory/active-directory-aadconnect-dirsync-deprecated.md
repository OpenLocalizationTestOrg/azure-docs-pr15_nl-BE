<properties
    pageTitle="Upgrade van DirSync en Azure AD Sync | Microsoft Azure"
    description="Beschrijving van het upgraden van DirSync en Azure AD synchroniseren naar Azure AD verbinden."
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/27/2016"
    ms.author="billmath"/>


# <a name="upgrade-windows-azure-active-directory-sync-dirsync-and-azure-active-directory-sync-azure-ad-sync"></a>Upgrade van Windows Azure Active Directory-synchronisatie ("DirSync") en Azure Active Directory-synchronisatie ('Azure AD Sync')
Azure AD Connect is de beste manier om uw directory op ruimten verbinden met AD Azure en Office 365. Dit is een geweldige tijd voor een upgrade uitvoeren naar Azure AD verbinding maken met Windows Azure Active Directory-synchronisatie (DirSync) of Azure AD synchroniseren als deze hulpprogramma's zijn nu verouderd en einde van de ondersteuning op 13 April 2017 zullen bereiken.

De identiteit van twee synchronisatie-hulpprogramma's die zijn afgeschaft zijn aangeboden voor klanten met één forest (DirSync) en voor meerdere forests en andere geavanceerde klanten (Azure AD synchronisatie). Deze oudere hulpprogramma's zijn vervangen door één enkele oplossing die beschikbaar is voor alle scenario's: Azure AD verbinden. Het biedt nieuwe functies, verbeteringen en ondersteuning voor nieuwe scenario's. Om te kunnen doorgaan met het synchroniseren van uw identiteitsgegevens op ruimten naar AD Azure en Office 365, wordt aangeraden om te upgraden naar Azure AD verbinden.

De laatste release van DirSync werd uitgebracht in juli 2014 en de laatste versie van Azure AD synchronisatie werd uitgebracht in mei 2015.

## <a name="what-is-azure-ad-connect"></a>Wat is Azure AD verbinden
Azure AD Connect is de opvolger van DirSync en Azure AD Sync. Het combineert deze twee ondersteund alle scenario's. U kunt meer lezen over deze [identiteiten in ruimten met Azure Active Directory](active-directory-aadconnect.md)te integreren.

## <a name="deprecation-schedule"></a>Afschrijving planning

Datum | Opmerking
 --- | ---
13 april 2016 | Windows Azure Active Directory-synchronisatie ("DirSync") en Microsoft Azure Active Directory-synchronisatie ('Azure AD Sync') worden aangekondigd als verouderd.
13 april 2017 | Ondersteuning voor uiteinden. Klanten langer niet een aanvraag ondersteuning openen zonder eerst een upgrade naar Azure AD verbinden.

## <a name="how-to-transition-to-azure-ad-connect"></a>Het overstappen naar Azure AD verbinden
Als u DirSync op twee manieren kunt u een upgrade uitvoert: In-place upgrade en parallelle implementatie. Een in-place upgrade wordt aanbevolen voor de meeste klanten en als er een recente werkzaam zijn en minder dan 50.000 objecten. In andere gevallen verdient het doen van een parallelle installatie waar uw configuratie DirSync wordt verplaatst naar een nieuwe server met Azure AD verbinden.

Azure AD synchroniseren als u wordt een upgrade aanbevolen. Als u wilt, is het mogelijk te installeren van een nieuwe Azure AD verbinding maken met server parallel en een swing migratie van de server van Azure AD Sync Azure AD verbinding te doen.

Oplossing | Scenario
----- | -----
[DirSync upgraden](./connect/active-directory-aadconnect-dirsync-upgrade-get-started.md) | <li>Als u een bestaande DirSync server al wordt uitgevoerd.</li>
[Een upgrade uitvoeren van Azure AD Sync](active-directory-aadconnect-upgrade-previous-version.md)| <li>Als u van Azure AD Sync verplaatst.</li>

Als u zien hoe u een upgrade uitvoeren vanuit DirSync op Azure AD Connect wilt, klik Zie deze video Channel 9:

> [AZURE.VIDEO azure-active-directory-connect-in-place-upgrade-from-legacy-tools]

## <a name="faq"></a>FAQ
**V: ik heb een e-mailbericht ontvangen van het Team Azure en/of een bericht vanuit het berichtencentrum Office 365, maar ik gebruik verbinden.**  
De melding is ook verzonden naar klanten met Azure AD verbinding maken met een versienummer 1.0. \*.0 (met behulp van een pre-1.1-release). Microsoft raadt klanten aan de nieuwste versies Azure AD verbinden. Met de functie [Automatische upgrade](active-directory-aadconnect-feature-automatic-upgrade.md) kunt u 1.1 geïnstalleerd heel gemakkelijk hebben altijd een recente versie van Azure AD verbinden.

**Q: zal DirSync/Azure AD Sync niet meer werken op 13 April 2017?**  
Nr. De datum voor wanneer deze niet langer kan communiceren met Azure Active Directory wordt aangekondigd op een later tijdstip. U zult vinden die informatie in dit onderwerp, indien beschikbaar.

**V: welke versies DirSync kan ik upgraden van**  
Deze wordt ondersteund als u wilt upgraden van een willekeurige DirSync release die momenteel wordt gebruikt.

**V: hoe zit het Azure AD-Connector voor de FIM/MIM?**  
De Azure AD-Connector voor de FIM/MIM **niet** is aangekondigd als verouderd. Het is aan de **functie blokkeren**; geen nieuwe functionaliteit wordt toegevoegd en geen bugfixes ontvangen. Microsoft raadt klanten die gebruikmaken van het ene Azure AD verbinding te plannen. Het is raadzaam niet alle nieuwe implementaties met het starten. Deze Connector wordt aangekondigd in de toekomst worden afgeschaft.

## <a name="additional-resources"></a>Aanvullende bronnen

* [Integratie van uw identiteiten op ruimten met Azure Active Directory](active-directory-aadconnect.md)
