
<properties
    pageTitle="Met gezondheid verbinding Azure AD met sync | Microsoft Azure"
    description="Dit is de gezondheid verbinding Azure AD-pagina die wordt besproken hoe sync Azure AD-verbinding controleren."
    services="active-directory"
    documentationCenter=""
    authors="karavar"
    manager="samueld"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="vakarand"/>

# <a name="using-azure-ad-connect-health-for-sync"></a>Met behulp van gezondheid Azure AD verbinden voor synchronisatie
De volgende documentatie is specifiek voor Azure AD verbinding maken (synchroniseren) met gezondheid Azure AD verbinding controleren.  Zie voor meer informatie over het controleren van AD FS met Azure AD verbinding maken met de gezondheid [Met behulp van Azure AD gezondheid verbinding met AD FS](active-directory-aadconnect-health-adfs.md). Zie daarnaast voor informatie over het controleren van Active Directory Domain Services met Azure AD verbinding maken met de gezondheid van [Azure met behulp van AD gezondheid verbinding met AD DS](active-directory-aadconnect-health-adds.md).

![Azure AD gezondheid verbinden voor synchronisatie](./media/active-directory-aadconnect-health-sync/sync-blade.png)

## <a name="alerts-for-azure-ad-connect-health-for-sync"></a>Waarschuwingen voor Azure AD verbinding maken met de gezondheid voor synchronisatie
Azure AD verbinding maken met de gezondheid van waarschuwingen voor de sync sectie kunt u de lijst met actieve waarschuwingen. Elke melding bevat relevante informatie, stappen voor het oplossen en koppelingen naar verwante documentatie. Door het selecteren van een actief of afgesloten waarschuwing ziet u een nieuwe blade met aanvullende informatie, evenals de stappen die u kunt uitvoeren voor het herleiden van de waarschuwing en koppelingen naar aanvullende documentatie. U kunt ook historische gegevens bekijken over de waarschuwingen die zijn opgelost in het verleden.

Door het selecteren van een waarschuwing die u als u meer informatie, alsmede de stappen krijgt kunt u nemen op te lossen de waarschuwing en koppelingen naar aanvullende documentatie.

![Azure AD verbinden synchronisatiefout](./media/active-directory-aadconnect-health-sync/alert.png)

### <a name="limited-evaluation-of-alerts"></a>Beperkte evaluatie van waarschuwingen
Als Azure AD verbinding maken met de standaardconfiguratie niet wordt gebruikt (bijvoorbeeld, als het kenmerk filter wordt gewijzigd van de standaardconfiguratie in een aangepaste configuratie), vervolgens in de Azure AD verbinding Health agent niet de foutgebeurtenissen met betrekking tot Azure AD verbinden uploaden.

Dit houdt in dat de evaluatie van de waarschuwingen door de service. Dan ziet u een spandoek geeft aan deze voorwaarde in de Portal Azure onder uw service.

![Azure AD gezondheid verbinden voor synchronisatie](./media/active-directory-aadconnect-health-sync/banner.png)

U kunt dit wijzigen door te klikken op 'Instellingen' en Azure AD verbinding Health agent voor het uploaden van alle logboekbestanden.

![Azure AD gezondheid verbinden voor synchronisatie](./media/active-directory-aadconnect-health-sync/banner2.png)

## <a name="sync-insight"></a>Sync inzicht
Veel beheerders wilt weten over de tijd die nodig is om wijzigingen synchroniseren naar Azure AD en het bedrag van de wijzigingen plaatsvinden. Deze functie biedt een eenvoudige manier om dit te visualiseren met behulp van de onderstaande grafieken:   

- Latentie van de synchronisatie
- Trend van object wijzigen

### <a name="sync-latency"></a>Sync latentie
Deze functie biedt een grafische trend van de latentie van de sync-bewerkingen (invoer, uitvoer, enz.) voor verbindingslijnen.  Dit biedt een snelle en eenvoudige manier om te begrijpen, niet alleen de latentie van de bewerkingen (groter als u beschikt over een groot aantal veranderingen), maar ook een manier om de anomalieën in de vertraging die u mogelijk verder onderzoek.

![Sync latentie](./media/active-directory-aadconnect-health-sync/synclatency02.png)

Standaard worden alleen de latentie van de "Export"-bewerking voor de Azure AD-connector wordt weergegeven.  Meer bewerkingen op de connector of om bewerkingen uit andere connectors, weer te geven met de rechtermuisknop op de grafiek, selecteer grafiek bewerken of klik op de knop 'Bewerken latentie Chart' en kies specifieke bewerking en verbindingslijnen.

### <a name="sync-object-changes"></a>Sync-Object wijzigt
Deze functie biedt een grafische trend van het aantal wijzigingen dat wordt geëvalueerd en uitgevoerd naar AD Azure.  Vandaag, is probeert deze om informatie te verzamelen uit de logboeken sync moeilijk.  De grafiek biedt niet alleen een eenvoudigere manier controleren het aantal wijzigingen die optreden in uw omgeving, maar ook een visuele weergave van de fouten die optreden.

![Sync latentie](./media/active-directory-aadconnect-health-sync/syncobjectchanges02.png)

## <a name="object-level-synchronization-error-report-preview"></a>Het foutenrapport synchronisatie op niveau van object (voorbeeld)
Deze functie voorziet in een rapport over synchronisatiefouten die optreden kunnen als de id-gegevens worden gesynchroniseerd tussen Windows Server AD en Azure AD Azure AD verbinden met.

- Het rapport heeft betrekking op fouten vastgelegd door de synchronisatie-client (verbinding maken met Azure AD versie 1.1.281.0 of hoger)
- De fouten die zijn opgetreden bevat in de laatste synchronisatiebewerking op de sync-engine. ("Uitvoer" van de Connector met Azure AD.)
- Azure AD verbinding Health agent voor synchronisatie hebt uitgaande verbindingen met de vereiste eindpunten voor de lijst wilt opnemen, de meest recente gegevens. Zie de [sectie vereisten](active-directory-aadconnect-health-agent-install.md#Requirements) voor meer informatie.
- Het rapport is **bijgewerkt na elke 30 minuten** met behulp van de gegevens die zijn geüpload door Azure AD verbinding Health agent voor synchronisatie.
Het biedt de volgende belangrijke mogelijkheden

    - Classificatie van fouten
    - Lijst met objecten met fouten per categorie
    - De gegevens over de fouten op één plaats
    - Aan de zijkant door vergelijking van de zijde van objecten met fout vanwege een conflict
    - Download het rapport als een CVS (binnenkort beschikbaar)

### <a name="categorization-of-errors"></a>Classificatie van fouten
Het rapport de bestaande synchronisatiefouten in de volgende categorieën ingedeeld:

| Categorie | Beschrijving |
| -------------- | ----------- |
| Dubbel kenmerk | Fouten bij het verbinden met Azure AD probeert maken of bijwerken van objecten met dubbele waarden van een of meer kenmerken in Azure AD die uniek in een huurder, zoals proxyAddresses, UserPrincipalName zijn. |
| Gegevens komen niet overeen | Fouten bij het soft-match niet overeenkomen met de objecten die leiden fouten bij de gegevenssynchronisatie tot. |
| Gegevens van mislukte validatie | Fouten als gevolg van ongeldige gegevens, zoals niet-ondersteunde tekens in essentiële kenmerken zoals UserPrincipalName, opmaken fouten die niet gevalideerd voordat deze werd geschreven in Azure AD.|
| Grote kenmerk | Fouten bij een of meer kenmerken groter dan de toegestane grootte, lengte of het aantal zijn.|
| Andere | Alle andere fouten die niet in de bovenstaande categorieën passen. Op basis van feedback, wordt deze categorie gesplitst in subcategorieën.

![Fout rapport overzicht synchroniseren](./media/active-directory-aadconnect-health-sync/errorreport01.png)
![fout rapportcategorieën synchroniseren](./media/active-directory-aadconnect-health-sync/errorreport02.png)

### <a name="list-of-objects-with-error-per-category"></a>Lijst met objecten met fouten per categorie
Boren in elke categorie krijgt u de lijst met objecten die de fout in die categorie.
![Synchronisatielijst fout rapport](./media/active-directory-aadconnect-health-sync/errorreport03.png)

### <a name="error-details"></a>Foutdetails
Volgende gegevens vindt u in de gedetailleerde weergave voor elke fout

- Id's van het *AD Object* betrokken
- Id's voor *Azure AD Object* die betrokken zijn (indien van toepassing)
- Beschrijving van de fout en het oplossen van
- Verwante artikelen

![Details van foutenrapport Sync](./media/active-directory-aadconnect-health-sync/errorreport04.png)

### <a name="download-the-error-report-as-csv"></a>Het rapport downloaden als CSV
Deze mogelijkheid is binnenkort beschikbaar. Bezoek ons regelmatig voor meer updates.



## <a name="related-links"></a>Verwante koppelingen
* [Het oplossen van fouten tijdens synchronisatie](active-directory-aadconnect-troubleshoot-sync-errors.md)
* [Dubbel kenmerk tolerantie](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md)
* [Azure AD verbinding gezondheid](active-directory-aadconnect-health.md)
* [Azure AD Health Agent-installatie aansluiten](active-directory-aadconnect-health-agent-install.md)
* [Azure AD verbinding gezondheid bewerkingen](active-directory-aadconnect-health-operations.md)
* [Met behulp van Azure AD gezondheid verbinding met AD FS](active-directory-aadconnect-health-adfs.md)
* [Met behulp van Azure AD gezondheid verbinding met AD DS](active-directory-aadconnect-health-adds.md)
* [Azure AD gezondheid Veelgestelde vragen over verbinding](active-directory-aadconnect-health-faq.md)
* [Azure AD verbinding gezondheid versiegeschiedenis](active-directory-aadconnect-health-version-history.md)
