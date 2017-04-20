<properties
    pageTitle="Azure AD verbinden sync: Service Synchronisatiebeheer UI | Microsoft Azure"
    description="Het tabblad verbindingslijnen in de Service Synchronisatiebeheer begrijpen voor Azure AD verbinden."
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

![Sync Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/connectors.png)

Het tabblad verbindingslijnen wordt gebruikt voor het beheer van alle systemen die op de sync-engine is aangesloten.

## <a name="connector-actions"></a>Acties voor Connector

Actie | Opmerking
--- | ---
Maken | Niet gebruiken. Voor het aansluiten van extra AD-forests, de installatiewizard te gebruiken.
Eigenschappen | Gebruikt voor het domein en de organisatie-eenheid filteren.
[Verwijderen](#delete) | Gebruikt om de gegevens in de ruimte connector verwijderen of verbinding met een forest verwijderen.
[Uitvoeren van profielen configureren](#configure-run-profiles) | Met uitzondering van domein filteren, niets hier configureren. U kunt deze actie Zie reeds geconfigureerde uitvoeren profielen.
Uitvoeren | Gebruikt voor het starten van een eenmalige uitvoering van een profiel.
Stop | Hiermee stopt u een verbindingslijn op dit moment met een profiel.
Connector exporteren | Niet gebruiken.
Connector importeren | Niet gebruiken.
Update-Connector | Niet gebruiken.
Schema vernieuwen | Het schema in de cache vernieuwd. Het verdient de voorkeur om in plaats daarvan gebruikt u de optie in de installatiewizard sinds dat updates ook regels synchroniseren.
[Search-Connector ruimte](#search-connector-space) | Gebruikt om objecten te zoeken en te [volgen van een object en de bijbehorende gegevens in het systeem](#follow-an-object-and-its-data-through-the-system).

### <a name="delete"></a>Verwijderen
De verwijderactie wordt gebruikt voor twee verschillende dingen.
![Sync Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/connectordelete.png)

De optie **alleen space connector verwijderen** verwijdert u alle gegevens, maar de configuratie behouden.

De optie **Connector verwijderen en de ruimte op de verbindingslijn** verwijdert u de gegevens en de configuratie. Deze optie wordt gebruikt wanneer u niet langer verbinding maken met een forest wilt.

Beide opties voor alle objecten synchroniseren en de metaverse objecten bijwerken. Deze actie is een langdurige bewerking.

### <a name="configure-run-profiles"></a>Uitvoeren van profielen configureren
Met deze optie kunt u de uitvoering profielen geconfigureerd voor een verbindingslijn.

![Sync Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/configurerunprofiles.png)

### <a name="search-connector-space"></a>Search-Connector ruimte
De actie zoeken connector ruimte is handig om objecten te vinden en oplossen van problemen met gegevens.

![Sync Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/cssearch.png)

Begin met het selecteren van een **bereik**. U kunt zoeken op basis van gegevens (Relative Distinguished Name DN, anker, substructuur), of de status van het object (alle andere opties).  
![Sync Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/cssearchscope.png)  
Als u bijvoorbeeld een substructuur zoekopdracht doet, krijgt u alle objecten in één organisatie-eenheid.
![Sync Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/cssearchsubtree.png) uit dit raster kunt u een object, selecteer **Eigenschappen**en [volgen](#follow-an-object-and-its-data-through-the-system) van de connector kleurruimte, via de metaverse, en aan de doel-connector.

## <a name="follow-an-object-and-its-data-through-the-system"></a>Volg een object en de bijbehorende gegevens via het systeem
Als u een probleem met gegevens oplossen wilt, gaat u als volgt een object uit de connector kleurruimte naar de metaverse en naar het doel connector ruimte is een belangrijke procedure om te begrijpen waarom gegevens niet de verwachte waarden hebben.

### <a name="connector-space-object-properties"></a>Connector ruimte objecteigenschappen
**Importeren**  
Als u een cs-object opent, zijn er meerdere tabbladen bovenaan. Het tabblad **importeren** worden de gegevens die na importeren wordt klaargezet.
![Sync Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/csimport.png) de **Oude waarde** geeft wat momenteel wordt opgeslagen in het systeem en de **Nieuwe waarde** wat van het bronsysteem is ontvangen en nog niet is vereffend. Aangezien er een synchronisatiefout is, kunnen de wijziging in dit geval kan niet worden toegepast.

**Fout**  
De foutpagina wordt alleen weergegeven als er een probleem met het object is. Zie de details op de pagina bewerkingen voor meer informatie over het [oplossen van synchronisatiefouten](active-directory-aadconnectsync-service-manager-ui-operations.md#troubleshoot-errors-in-operations-tab).

**Lineage**  
Het tabblad lineage ziet u hoe het object connector ruimte is gerelateerd aan het object metaverse. U kunt zien wanneer een wijziging de Connector laatste geïmporteerd uit het verbonden systeem en welke regels toegepast op de gegevens in de metaverse te vullen.
![Sync Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/cslineage.png) In de kolom **actie** ziet u er is een **Inkomend** sync-regel bij de **bepaling**van de actie. Dat, zolang deze connector ruimte object aanwezig is, wordt het object metaverse blijft aangeeft. Als de lijst met regels voor synchronisatie in plaats daarvan ziet u een regel synchroniseren met richting **Uitgaand** en **bepaling**, geeft aan dat dit object wordt verwijderd wanneer de metaverse-object wordt verwijderd.
![Sync Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/cslineageout.png) kunt u ook zien in de kolom **PasswordSync** dat de ruimte connector binnenkomende wijzigingen in het wachtwoord bijdragen kan omdat een sync-regel de waarde **True heeft**. Dit wachtwoord wordt vervolgens verzonden naar Azure AD via de uitgaande regel.

Op het tabblad lineage krijgt u aan de metaverse door te klikken op [Eigenschappen van het Object Metaverse](#metaverse-object-properties).

Onder alle tabbladen staan twee knoppen: **voorvertoning** en **logboek**.

**Voorbeeld**  
Voorbeeld van de pagina wordt gebruikt voor het synchroniseren van één enkel object. Het is handig als u problemen met sommige regels klant synchronisatie en het effect zien van een wijziging op een enkel object. U kunt kiezen tussen **Volledige Sync** en **sync Delta**. Ook kunt u kiezen tussen het **Genereren van voorvertoning**, blijft alleen de wijziging in het geheugen, en **Voorbeeld Commit**, welke alle fasen doel connector spaties verandert.
![Sync Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/preview1.png) kunt u het object en welke regel wordt toegepast voor een bepaald kenmerk stroom controleren.
![Sync Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/preview2.png)

**Logboek**  
De pagina wordt gebruikt voor een overzicht van de wachtwoord sync-status en de geschiedenis, Zie [problemen met Wachtwoordsynchronisatie](active-directory-aadconnectsync-implement-password-synchronization.md#troubleshoot-password-synchronization) voor meer informatie.

### <a name="metaverse-object-properties"></a>Eigenschappen van het Object Metaverse
**Kenmerken**  
Op het tabblad kenmerken kunt u de waarden zien en welke Connector bijgedragen het.
![Sync Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/mvattributes.png)
**verbindingslijnen**  
Het tabblad verbindingslijnen bevat alle connector ruimten een weergave van het object zijn.
![Sync Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/mvconnectors.png) op dit tabblad kunt u navigeren naar de [connector space-object](#connector-space-object-properties).

## <a name="next-steps"></a>Volgende stappen
Meer informatie over het configureren van [Azure AD Connect worden gesynchroniseerd](active-directory-aadconnectsync-whatis.md) .

Meer informatie over de [integratie van uw identiteiten op ruimten met Azure Active Directory](active-directory-aadconnect.md).
