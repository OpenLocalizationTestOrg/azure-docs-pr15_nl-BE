<properties
    pageTitle="Azure AD verbinden sync: Service Synchronisatiebeheer UI | Microsoft Azure"
    description="Het tabblad bewerkingen in de Service Synchronisatiebeheer begrijpen voor Azure AD verbinden."
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
    ms.date="09/07/2016"
    ms.author="billmath"/>


# <a name="azure-ad-connect-sync-synchronization-service-manager"></a>Azure AD verbinden sync: Service Synchronisatiebeheer

[Bewerkingen](active-directory-aadconnectsync-service-manager-ui-operations.md) | [Verbindingslijnen](active-directory-aadconnectsync-service-manager-ui-connectors.md) | [Metaverse Designer](active-directory-aadconnectsync-service-manager-ui-mvdesigner.md) | [Metaverse zoeken](active-directory-aadconnectsync-service-manager-ui-mvsearch.md)
--- | --- | --- | ---

![Sync Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/operations.png)

Op het tabblad activiteiten worden de resultaten van de meest recente bewerkingen. Dit tabblad is de sleutel tot het begrijpen en oplossen van problemen.

## <a name="understand-the-information-visible-in-the-operations-tab"></a>De informatie op het tabblad bewerkingen zichtbaar te begrijpen
De bovenste helft ziet u alle sessies in chronische volgorde. Het logboek voor bewerkingen houdt informatie over de afgelopen zeven dagen, maar deze instelling kan worden gewijzigd met de [Taakplanner](active-directory-aadconnectsync-feature-scheduler.md). U wilt zoeken naar alle uitvoeren die niet de status voltooid weergegeven. Kunt u het sorteren door te klikken op de kop.

De kolom **Status** wordt de belangrijkste informatie en geeft het ernstigste probleem voor een run. Hier volgt een kort overzicht van de meest voorkomende statussen op volgorde van prioriteit te onderzoeken (waarbij * verschillende mogelijke foutmeldingen geven).

Status | Opmerking
--- | ---
gestopt-* | De sessie kan niet worden voltooid. Als bijvoorbeeld het externe systeem niet actief is en is niet bereikbaar.
gestopt fout beperken | Er zijn meer dan 5.000. De sessie is automatisch door het grote aantal fouten gestopt.
voltooide -\*-fouten | De uitvoering is voltooid, maar er zijn fouten (minder dan 5000) die moeten worden onderzocht.
voltooide -\*-waarschuwingen | De uitvoering is voltooid, maar sommige gegevens is niet de verwachte status. Als er fouten zijn, is dit bericht meestal slechts een symptoom. Totdat de fouten hebt opgelost, moet u waarschuwingen niet onderzoeken.
succes | Geen problemen.

Wanneer u een rij selecteert, werkt onder wilt weergeven van de details van die worden uitgevoerd. Aan de linkerkant van de onderkant hebt u een lijst **stap #**spreken. Deze lijst wordt alleen weergegeven als er meerdere domeinen in het forest waar elk domein wordt vertegenwoordigd door een stap. Naam van het domein kunt u vinden onder de kop **partitie**. Onder **Statistieken voor synchronisatie**vindt u meer informatie over het aantal wijzigingen dat is verwerkt. U kunt klikken op de links voor een overzicht van de gewijzigde objecten. Als u objecten met fouten, die fouten weergegeven onder **Synchronisatiefouten**.

## <a name="troubleshoot-errors-in-operations-tab"></a>Oplossen van fouten in het tabblad bewerkingen
![Sync Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/errorsync.png)  
Als er fouten zijn, worden zowel het object in de fout en de fout zelf koppelingen vindt u meer informatie.

Start door te klikken op de tekenreeks (**sync-regel-fout-functie-geactiveerd** in de afbeelding). Er worden eerst weergegeven met een overzicht van het object. Overzicht van de werkelijke fout, klikt u op de **Stack Trace**. Deze tracering biedt debug-niveau-informatie voor de fout.

**TIP:** U kunt met de rechtermuisknop in het vak **informatie over stack met opgeroepen** , kiest u **Alles selecteren**en **kopiëren**. U kunt vervolgens kopiëren van de stapel en bekijk de fout in uw favoriete teksteditor, zoals Kladblok.

- Als de fout afkomstig van **SyncRulesEngine is**, heeft de informatie over de call stack eerst een lijst met alle kenmerken van het object. Schuif naar beneden totdat u ziet dat de kop **InnerException = >**.  
![Sync Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/errorinnerexception.png)  
De regel na de fout bevat. In de bovenstaande afbeelding is de fout van een aangepaste synchronisatie-regel Fabrikam gemaakt.

Als de fout zich niet voldoende informatie geboden, is het tijd om de gegevens zelf te bekijken. U kunt klikken op de koppeling met de object-id en het [volgen van een object en de bijbehorende gegevens in het systeem](active-directory-aadconnectsync-service-manager-ui-connectors.md#follow-an-object-and-its-data-through-the-system).

## <a name="next-steps"></a>Volgende stappen
Meer informatie over het configureren van [Azure AD Connect worden gesynchroniseerd](active-directory-aadconnectsync-whatis.md) .

Meer informatie over de [integratie van uw identiteiten op ruimten met Azure Active Directory](active-directory-aadconnect.md).
