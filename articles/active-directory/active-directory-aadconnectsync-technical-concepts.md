<properties
    pageTitle="Azure AD verbinden sync: technische concepten | Microsoft Azure"
    description="Worden de technische concepten van Azure AD verbinden synchronisatie uitgelegd."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="markusvi;andkjell"/>


# <a name="azure-ad-connect-sync-technical-concepts"></a>Azure AD verbinden sync: technische concepten
In dit artikel wordt een overzicht van het onderwerp [Wat architectuur](active-directory-aadconnectsync-technical-concepts.md).

Azure AD verbinden sync is gebaseerd op een effen metadirectory synchronisatie-platform.
De volgende secties introduceren de concepten voor metadirectory synchronisatie.
Voortbouwende op MIIS ILM en FIM, biedt Azure Active Directory Sync Services de volgende platform voor verbinding met gegevensbronnen, synchroniseren van gegevens tussen gegevensbronnen, alsmede de inrichting en deprovisioning van identiteiten.

![Technische concepten](./media/active-directory-aadconnectsync-technical-concepts/scenario.png)

De volgende secties bevatten meer informatie over de volgende aspecten van de synchronisatieservice FIM:

- Connector
- Kenmerk stroom
- Ruimte voor Connector
- Metaverse
- Inrichten

## <a name="connector"></a>Connector

De codemodules die worden gebruikt om te communiceren met een gekoppelde map worden verbindingslijnen (voorheen bekend als management-agents (MAs)) genoemd.

Deze zijn geïnstalleerd op de computer synchroniseren Azure AD verbinden met.
De verbindingslijnen kunnen u agentless manier converseren met behulp van protocollen voor externe systeem niet vertrouwen op de implementatie van gespecialiseerde agenten. Dit betekent minder risico's en implementatietijden, vooral tijdens het afhandelen van kritieke toepassingen en systemen.

In de bovenstaande afbeelding is de connector is synoniem met de connector ruimte, maar omvat alle communicatie met het externe systeem.

De connector is verantwoordelijk voor alle import en functionaliteit exporteren naar het systeem en ontwikkelaars van hoeft te weten hoe u kunt verbinding maken met elk systeem zichzelf bij het gebruik van declaratieve ingericht voor het aanpassen van gegevenstransformaties wordt vrijgemaakt.

Invoer en uitvoer alleen optreden wanneer gepland, waardoor verdere isolatie van de veranderingen die zich in het systeem, omdat de wijzigingen worden niet automatisch doorgeven aan de verbonden gegevensbron. Ontwikkelaars kunnen bovendien ook hun eigen connectors voor de aansluiting op vrijwel elke gegevensbron maken.

## <a name="attribute-flow"></a>Kenmerk stroom

De metaverse is de geconsolideerde weergave van alle gekoppelde identiteiten van de aangrenzende ruimten connector. Kenmerk stroom wordt in de figuur is afgebeeld door lijnen met pijlkoppen voor inkomende en uitgaande stroom. Kenmerk stroom is het proces van het kopiëren of het omzetten van gegevens van het ene systeem naar het andere en alle kenmerken van het loopt (inkomend of uitgaand).

Kenmerk flow plaatsvindt tussen de ruimte van de connector en de metaverse bi andersom bij de planning van synchronisatiebewerkingen (volledig of delta) uit te voeren.

Kenmerk stroom treedt alleen op wanneer deze synchronisaties worden uitgevoerd. Kenmerk stromen worden gedefinieerd in de regels van de synchronisatie. Deze kunnen worden binnenkomende ISR (in de bovenstaande afbeelding) of uitgaande (OSR in de bovenstaande afbeelding).

## <a name="connected-system"></a>Verbonden systeem

Verbonden systeem (aka verbonden directory) die verwijzen naar het externe systeem Azure AD verbinden sync is verbonden met en leest van en schrijft id-gegevens van en naar.

## <a name="connector-space"></a>Ruimte voor Connector

Elke verbonden gegevensbron wordt voorgesteld als een gefilterde subset van objecten en kenmerken in de ruimte van de connector.
Dit kan de synchronisatieservice lokaal werken zonder de noodzaak contact opnemen met het externe systeem bij het synchroniseren van de objecten en beperkt de interactie op de invoer en uitvoer alleen.

Als de gegevensbron en de verbindingslijn hebt de mogelijkheid te bieden een lijst met wijzigingen (een delta importeren), vervolgens stijgt de operationele efficiëntie aanzienlijk als alleen de wijzigingen sinds de laatste polling cycle worden uitgewisseld. De connector ruimte schermt de verbonden gegevensbron uit wijzigingen automatisch doorgeven door te vereisen dat de planning van de connector importeert en exporteert. Deze extra verzekering geeft u gemoedsrust tijdens het testen, voorvertonen of de volgende update te bevestigen.

## <a name="metaverse"></a>Metaverse

De metaverse is de geconsolideerde weergave van alle gekoppelde identiteiten van de aangrenzende ruimten connector.

Identiteiten aan elkaar zijn gekoppeld en de instantie is toegewezen voor de diverse kenmerken, door middel van flow-toewijzingen importeren, wordt het object central metaverse verzamelde gegevens uit meerdere systemen. Van deze overdracht van het kenmerk object overdracht toewijzingen van uitgaande systemen.

Objecten worden gemaakt wanneer een bindend systeem deze in de metaverse projecten. Als alle verbindingen zijn verwijderd, wordt het object metaverse verwijderd.

Objecten in de metaverse kunnen niet rechtstreeks worden bewerkt. Alle gegevens in het object moet worden bijgedragen door kenmerk-stroom. De metaverse onderhoudt permanente verbindingslijnen met elke ruimte connector. Deze aansluitingen hoeven geen nieuwe evaluatie voor elke synchronisatie uitvoeren. Dit betekent dat Azure AD verbinden sync hoeft niet te zoeken naar het overeenkomende RAS-object telkens. Dit voorkomt de noodzaak voor dure agenten om te voorkomen dat wijzigingen in kenmerken die normaal gesproken verantwoordelijk is voor het correleren van de objecten.

Bij het ontdekken van nieuwe gegevensbronnen met bestaande objecten die moeten worden beheerd, sync Azure AD verbinding maken met een proces dat een regel join genoemd gebruikt voor de evaluatie van potentiële kandidaten waarmee een koppeling tot stand brengen.
Wanneer de koppeling is gemaakt, deze evaluatie niet meer optreedt en normale kenmerk stroom kan plaatsvinden tussen de verbonden externe gegevensbron en de metaverse.

## <a name="provisioning"></a>Inrichten

Wanneer een gezaghebbende bronprojecten kan een nieuw object in de metaverse een nieuwe verbindingslijn space-object in een andere Connector voor een downstream verbonden gegevensbron worden gemaakt.

Op deze manier per definitie een koppeling maakt, en stroom kenmerk bi andersom kunt doorgaan.

Wanneer een regel bepaalt dat een nieuwe verbindingslijn space-object moet worden gemaakt, wordt dit genoemd wordt ingericht. Echter omdat deze bewerking alleen plaats in de ruimte connector vindt, wordt het niet overgenomen in de verbonden gegevensbron totdat een exporteren wordt uitgevoerd.

## <a name="additional-resources"></a>Aanvullende bronnen

* [Azure AD verbinding Sync: Synchronisatie-opties aanpassen](active-directory-aadconnectsync-whatis.md)
* [Integratie van uw identiteiten op ruimten met Azure Active Directory](active-directory-aadconnect.md)

<!--Image references-->
[1]: ./media/active-directory-aadsync-technical-concepts/ic750598.png
