<properties
   pageTitle="Azure AD verbinden sync: inzicht in de architectuur | Microsoft Azure"
   description="Dit onderwerp beschrijft de architectuur van Azure AD verbinden sync en de gebruikte termen uitgelegd."
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
   ms.date="08/31/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-sync-understanding-the-architecture"></a>Azure AD verbinden sync: inzicht in de architectuur
In dit onderwerp worden de basisarchitectuur voor Azure verbinden met AD-synchronisatie. In vele aspecten is vergelijkbaar met de voorafgaande MIIS 2003 ILM 2007 en 2010 FIM. Azure AD verbinden sync is de ontwikkeling van deze technologieën. Als u bekend met een van deze eerdere technologieën bent, wordt de inhoud van dit onderwerp ook aan u bekend zijn. Als u synchronisatie, wordt de in dit onderwerp voor u. Het is echter niet willen weten van dit onderwerp kan alleen slagen in het aanbrengen van aanpassingen in Azure AD verbinden sync (sync engine genoemd in dit onderwerp).

## <a name="architecture"></a>Architectuur
De synchronisatie-engine een geïntegreerde weergave van objecten die zijn opgeslagen in meerdere verbonden gegevensbronnen maken en beheren van identiteitsgegevens in die gegevensbronnen. Deze geïntegreerde weergave wordt bepaald door de identiteitsgegevens van verbonden gegevensbronnen en een set regels die bepalen hoe de verwerking van deze gegevens opgehaald.

### <a name="connected-data-sources-and-connectors"></a>Gekoppelde gegevensbronnen en verbindingslijnen
De synchronisatie-engine verwerkt identiteitsgegevens uit verschillende gegevensopslaglocaties, zoals Active Directory of een SQL Server-database. Elke gegevensopslagplaats die de gegevens in een database-achtige indeling organiseert en die zorgt voor gegevenstoegang standaard methoden is een potentiële data source kandidaat voor de synchronisatie-engine. De opslagplaatsen met gegevens die worden gesynchroniseerd door synchronisatie-engine worden **verbonden gegevensbronnen** of **mappen verbonden** (CD) genoemd.

De synchronisatie-engine ingekapseld interactie met een verbonden gegevensbron in een module met de naam van een **Connector**. Elk type van de verbonden gegevensbron heeft een bepaalde verbindingslijn. De Connector zet een vereiste bewerking in de indeling die de verbonden gegevensbron begrijpt.

Verbindingslijnen maken voor een gekoppelde gegevensbron API-aanroepen voor de uitwisseling van informatie (lezen en schrijven). Het kan ook een aangepaste verbindingslijn met extensible connectivity framework toevoegen. De volgende afbeelding ziet u hoe een Connector is verbonden gegevensbron verbonden aan de sync-engine.

![Arch1](./media/active-directory-aadconnectsync-understanding-architecture/arch1.png)

Gegevens in beide richtingen kunnen lopen, maar het kan niet doorlopen in beide richtingen tegelijk. Met andere woorden, een verbindingslijn kan worden geconfigureerd om te kunnen gegevens van de verbonden gegevensbron in de synchronisatie-engine of synchronisatie-engine verbonden gegevensbron, maar slechts een van deze bewerkingen kan optreden op elk gewenst moment één voor één object en kenmerk. De richting kan afwijken van andere objecten en andere kenmerken.

Configureren van een verbindingslijn geven u de objecttypen die u wilt synchroniseren. De objecttypen opgeven, definieert het bereik van objecten die deel uitmaken van het synchronisatieproces. De volgende stap is om de kenmerken te synchroniseren, selecteren die staat bekend als een lijst met kenmerken opgenomen. Deze instellingen kunnen worden aangepast in reactie op wijzigingen aan uw bedrijfsregels. Wanneer u de installatiewizard Azure AD verbinden, worden deze instellingen voor u geconfigureerd.

Wanneer objecten wilt exporteren naar een verbonden gegevensbron, moet de lijst met bestandstypen met kenmerk ten minste de kenmerken die zijn vereist voor het maken van een bepaald type in een gekoppelde gegevensbron. Bijvoorbeeld moet het kenmerk **sAMAccountName** worden opgenomen in de lijst exporteren een gebruikersobject in Active Directory omdat alle gebruikersobjecten in Active Directory een **sAMAccountName** kenmerk gedefinieerd hebben kenmerk. De installatiewizard wordt opnieuw, deze configuratie voor u.

Als de gekoppelde gegevensbron wordt gebruikt voor structurele onderdelen, zoals partities of containers voor het ordenen van objecten, kunt u de gebieden in de verbonden gegevensbron die voor een bepaalde oplossing worden gebruikt te beperken.

### <a name="internal-structure-of-the-sync-engine-namespace"></a>Interne structuur van de naamruimte sync-engine
De volledige synchronisatie-engine naamruimte bestaat uit twee naamruimten die de identiteitsgegevens worden opgeslagen. De twee naamruimten zijn:

- De ruimte connector (CS)
- De metaverse (MV)

De **ruimte van de connector** is een testlocatie met voorstellingen van de opgegeven objecten in een verbonden gegevensbron en de kenmerken die zijn opgegeven in de lijst opgenomen. De synchronisatie-engine gebruikt de ruimte connector om te bepalen wat er in de gekoppelde gegevensbron is gewijzigd en binnenkomende wijzigingen fase. De synchronisatie-engine gebruikt ook de connector-ruimte klaarzetten uitgaande wijzigingen voor uitvoer naar de verbonden gegevensbron. De synchronisatie-engine houdt een ruimte verschillende connector als een testlocatie voor iedere verbindingslijn.

Met behulp van een staging-ruimte, de synchronisatie-engine blijft onafhankelijk van de verbonden gegevensbronnen en wordt niet beïnvloed door de beschikbaarheid en toegankelijkheid. Hierdoor kunt u identiteitsgegevens op elk gewenst moment verwerken met behulp van de gegevens in de staging-ruimte. De synchronisatie-engine kan aanvragen alleen de wijzigingen in de verbonden gegevensbron sinds de laatste communicatie sessie beëindigd of push om alleen de wijzigingen in identiteitsgegevens die de verbonden gegevensbron nog niet ontvangen, waardoor het netwerkverkeer tussen de synchronisatie-engine en de gekoppelde gegevensbron.

Bovendien sync engine wordt statusinformatie opgeslagen over alle objecten die deze in de ruimte van de connector fasen. Wanneer nieuwe gegevens worden ontvangen, evalueert sync engine altijd of de gegevens al is gesynchroniseerd.

De **metaverse** is een opslaggebied dat de verzamelde identiteitsgegevens van meerdere verbonden gegevensbronnen en die één globale, geïntegreerde weergave van alle gecombineerde objecten bevat. Metaverse objecten zijn gemaakt op basis van de informatie die wordt opgehaald uit de gekoppelde gegevensbronnen en een verzameling regels waarmee u tijdens de synchronisatie aanpassen.

De volgende afbeelding ziet u de naamruimte connector ruimte en de metaverse-naamruimte in de synchronisatie-engine.

![Arch2](./media/active-directory-aadconnectsync-understanding-architecture/arch2.png)

## <a name="sync-engine-identity-objects"></a>Synchronisatie-engine identiteit-objecten
De objecten in de synchronisatie-engine zijn representaties van de objecten in de verbonden gegevensbron of de geïntegreerde weergave die synchronisatie-engine heeft van die objecten. Elk object sync-engine moet een globally unique identifier (GUID). GUID's bieden integriteit van gegevens en express relaties tussen objecten.

### <a name="connector-space-objects"></a>Connector ruimteobjecten
Wanneer de synchronisatie-engine communiceert met een verbonden gegevensbron, leest de identiteitsgegevens in de verbonden gegevensbron en die gegevens worden gebruikt voor het maken van een representatie van de identiteit-object in de ruimte van de connector. U kunt maken of verwijderen van deze objecten afzonderlijk. Echter, kunt u alle objecten in de ruimte van een verbindingslijn handmatig verwijderen.

Alle objecten in de ruimte connector hebben twee kenmerken:

- Een globally unique identifier (GUID)
- Een DN-naam (Distinguished Name)

Als een uniek kenmerk de verbonden gegevensbron aan het object toewijst, klik-objecten in de ruimte connector kunnen ook ankerkenmerk hebben. Het ankerkenmerk is een unieke aanduiding van een object in de gekoppelde gegevensbron. De synchronisatie-engine maakt gebruik van het anker op zoek naar de overeenkomstige representatie van dit object in de verbonden gegevensbron. Synchronisatie-engine wordt ervan uitgegaan dat het anker van een object niet gedurende de levensduur van het object verandert.

Veel van de verbindingslijnen gebruiken een bekende unieke id voor het genereren van een anker automatisch voor elk object wanneer deze wordt geïmporteerd. Active Directory Connector gebruikt bijvoorbeeld de **kenmerken objectGUID** kenmerk voor een anker. Voor gekoppelde gegevensbronnen die niet van een unieke id voor duidelijk gedefinieerde voorzien kunt u generatie anker als onderdeel van de configuratie van de Connector.

In dat geval wordt het anker wordt samengesteld uit een of meer unieke kenmerken van een object, noch welke wijzigingen en unieke typt geeft het object in de ruimte van de connector (bijvoorbeeld een werknemernummer of een gebruikers-ID).

Een object connector ruimte zijn:

- Een staging-object
- Een tijdelijke aanduiding

### <a name="staging-objects"></a>Tijdelijke objecten
Een staging-object vertegenwoordigt een exemplaar van de opgegeven objecttypen uit de gekoppelde gegevensbron. En de DN-naam van de GUID heeft een staging-object altijd een waarde die het objecttype aangeeft.

Tijdelijke objecten die altijd zijn geïmporteerd hebben een waarde voor het anker. Tijdelijke objecten die zojuist zijn voorzien van synchronisatie-engine en nog moet worden gemaakt in de gekoppelde gegevensbron hoeft niet een waarde voor het anker.

Tijdelijke objecten ook uitvoeren huidige waarden van kenmerken van zakelijke en operationele informatie nodig om uit te voeren tijdens de synchronisatie sync engine. Operationele informatie bevat vlaggen die aangeven welke typen updates die zijn gefaseerd op de staging-object. Als een staging-object heeft ontvangen van nieuwe identiteitsgegevens van de verbonden gegevensbron die nog niet zijn verwerkt, wordt het object is gemarkeerd als **import in behandeling**. Als een staging-object heeft een nieuwe identiteit informatie die nog niet is geëxporteerd naar de verbonden gegevensbron, wordt het gemarkeerd als **uitvoer in behandeling**.

Een gefaseerde installatie-object wordt een import of export object. De synchronisatie-engine maakt een object importeren met behulp van Objectinformatie ontvangen van de verbonden gegevensbron. Wanneer synchronisatie-engine ontvangt informatie over het bestaan van een nieuw object dat overeenkomt met een van de objecttypen in de verbindingslijn hebt geselecteerd, wordt een object importeren in de ruimte van de connector als een representatie van het object in de gekoppelde gegevensbron.

De volgende afbeelding ziet u een importobject dat een object in de verbonden gegevensbron vertegenwoordigt.

![Arch3](./media/active-directory-aadconnectsync-understanding-architecture/arch3.png)

De synchronisatie-engine maakt een object exporteren met behulp van de gegevens in de metaverse. Objecten exporteren worden geëxporteerd naar de verbonden gegevensbron tijdens de volgende mededeling. Vanuit het perspectief van de synchronisatie-engine bestaan objecten exporteren niet in de verbonden gegevensbron nog. Daarom is het ankerkenmerk voor een object exporteren niet beschikbaar. Nadat het object van synchronisatie-engine ontvangen, maakt de verbonden gegevensbron een unieke waarde voor het ankerkenmerk van het object.

De volgende afbeelding ziet u hoe een export-object is gemaakt met behulp van identiteitsgegevens in de metaverse.

![Arch4](./media/active-directory-aadconnectsync-understanding-architecture/arch4.png)

De synchronisatie-engine bevestigt het exporteren van het object door het object vanuit de verbonden gegevensbron opnieuw te importeren. Objecten exporteren worden objecten importeren wanneer synchronisatie-engine ze tijdens de volgende importeren uit de gekoppelde gegevensbron ontvangt.

### <a name="placeholders"></a>Tijdelijke aanduidingen
Een platte naamruimte de synchronisatie-engine gebruikt voor het opslaan van objecten. Echter bepaalde gekoppelde gegevensbronnen, zoals Active Directory een hiërarchische naamruimte gebruiken. Synchronisatie-engine worden tijdelijke aanduidingen wilt transformeren informatie uit een hiërarchische naamruimte in een platte naamruimte, gebruikt voor het behoud van de hiërarchie.

Elke tijdelijke aanduiding (bijvoorbeeld een organisatie-eenheid) onderdeel van een hiërarchische objectnaam die is vereist om de naam van de hiërarchische samen te stellen, maar niet in de synchronisatie-engine is ingevoerd. Gemaakt door verwijzingen naar objecten die niet zijn staging-objecten in de ruimte van de connector in de verbonden gegevensbron lacunes te vullen.

De synchronisatie-engine worden ook tijdelijke aanduidingen gebruikt voor het opslaan van objecten die nog niet zijn geïmporteerd waarnaar wordt verwezen. Als synchronisatie is geconfigureerd voor het kenmerk manager voor het object *Abbie Spencer* en de ontvangen waarde is bijvoorbeeld een object dat niet geïmporteerd, zoals is *CN Lee Sperry, CN = Users, DC = fabrikam, DC = com =*, de manager-informatie wordt opgeslagen als tijdelijke aanduidingen in de ruimte van de connector. Als het managerobject later wordt geïmporteerd, wordt de tijdelijke aanduiding voor object wordt overschreven door de staging-object dat de manager vertegenwoordigt.

### <a name="metaverse-objects"></a>Metaverse-objecten
Een object metaverse bevat de samengevoegde weergave die synchronisatie-engine heeft tijdelijke objecten in de ruimte van de connector. Synchronisatie-engine maakt metaverse objecten met behulp van de gegevens in de objecten importeren. Diverse connector ruimteobjecten kunnen worden gekoppeld aan een object één metaverse, maar een connector space-object kan niet worden gekoppeld aan meer dan één object van de metaverse.

Metaverse objecten kunnen niet handmatig worden gemaakt of verwijderd. De synchronisatie-engine wordt metaverse-objecten die u een koppeling naar een object connector ruimte in de ruimte van de verbindingslijn hebt automatisch verwijderd.

Synchronisatie-engine biedt objecten in een verbonden gegevensbron worden toegewezen aan een bijbehorende objecttype in de metaverse, een uitgebreid schema met een aantal vooraf gedefinieerde objecttypen en de bijbehorende kenmerken. U kunt nieuwe objecttypen en kenmerken voor metaverse objecten maken. Kenmerken kunnen één waarde of meerdere waarden, en de kenmerktypen kunnen tekenreeksen, verwijzingen, getallen en Boole-waarden.

### <a name="relationships-between-staging-objects-and-metaverse-objects"></a>Relaties tussen tijdelijke en metaverse objecten
De gegevensstroom binnen de naamruimte sync-engine is ingeschakeld door de relatie van de koppeling tussen tijdelijke en metaverse objecten. Een staging-object dat is gekoppeld aan een object metaverse wordt een **gekoppeld object** (of een **verbindingslijn-object**) genoemd. Een staging-object dat niet is gekoppeld aan een metaverse-object is een **object niet langer lid** (of een **disconnector-object**) genoemd. De termen toegevoegd en niet langer lid verdienen de voorkeur boven niet verwarren met de verbindingslijnen die verantwoordelijk is voor het importeren en exporteren van gegevens uit een gekoppelde map.

Tijdelijke aanduidingen zijn nooit gekoppeld aan een object metaverse

Een gekoppelde object bestaat uit een staging-object en de relatie gekoppeld aan een object één metaverse. Gekoppelde objecten worden gebruikt voor het synchroniseren van kenmerkwaarden tussen een connector space-object en een object metaverse.

Wanneer een staging-object verandert in een gekoppelde object tijdens de synchronisatie, wordt kenmerken kunnen lopen tussen de tijdelijke objecten en de metaverse. Kenmerk stroom bidirectionele en met behulp van regels voor kenmerk import en export kenmerk is geconfigureerd.

Een object één connector ruimte kan worden gekoppeld aan slechts één metaverse-object. Echter kan elk object metaverse worden gekoppeld aan meerdere connector ruimteobjecten in hetzelfde of in verschillende connector-ruimten, zoals in de volgende afbeelding.

![Arch5](./media/active-directory-aadconnectsync-understanding-architecture/arch5.png)

De gekoppelde relatie tussen de staging-object en een object metaverse is permanent en kan worden verwijderd door de regels die u opgeeft.

Een object niet langer lid is een staging-object dat niet is gekoppeld aan een willekeurig object metaverse. Het attribuut waarden van een object niet langer lid niet zijn verwerkt alle verder in de metaverse. De waarden van het kenmerk van het overeenkomende object in de verbonden gegevensbron worden niet bijgewerkt door synchronisatie-engine.

Met behulp van afzonderlijke objecten, kunt u identiteitsgegevens worden opgeslagen in de synchronisatie-engine en deze later verwerken. Houden van een staging-object als een afzonderlijke object in de ruimte connector heeft veel voordelen. Het systeem heeft de vereiste informatie over dit object al klaargezet, is het niet nodig voor het maken van een weergave van dit object opnieuw tijdens het importeren van het volgende uit de gekoppelde gegevensbron. Op deze manier synchronisatie-engine heeft altijd een volledige momentopname van de verbonden gegevensbron, zelfs als er momenteel geen verbinding met de verbonden gegevensbron. Afzonderlijke objecten worden geconverteerd naar gekoppelde objecten, en omgekeerd, afhankelijk van de regels die u opgeeft.

Een object importeren is als een object niet langer lid gemaakt. Een export-object moet een gekoppelde object. De systeemlogica wordt deze regel afgedwongen en wordt elk object exporteren die geen gekoppelde object is verwijderd.

## <a name="sync-engine-identity-management-process"></a>Sync-engine identity management proces
Het proces voor identiteit bepaalt hoe informatie tussen verschillende verbonden gegevensbronnen wordt bijgewerkt. Identiteitsbeheer vindt plaats in drie processen:

- Importeren
- Synchronisatie
- Exporteren

Synchronisatie-engine geëvalueerd tijdens het importeren de binnenkomende identiteitsgegevens van een gekoppelde gegevensbron. Wanneer wijzigingen worden gedetecteerd, deze nieuwe tijdelijke objecten maakt of bestaande tijdelijke objecten in de ruimte connector voor synchronisatie wordt bijgewerkt.

Tijdens het synchronisatieproces sync-engine werkt de metaverse met wijzigingen die zijn opgetreden in de ruimte van de connector en de ruimte van de connector met de wijzigingen die zich hebben voorgedaan in de metaverse bijgewerkt.

Tijdens het exportproces duwt sync-engine om de wijzigingen die op tijdelijke objecten worden klaargezet en die zijn gemarkeerd als in afwachting van uitvoer.

De volgende afbeelding ziet u waar elk van de processen als identiteit informatiestromen in een verbonden gegevensbron naar een andere plaatsvindt.

![Arch6](./media/active-directory-aadconnectsync-understanding-architecture/arch6.png)

### <a name="import-process"></a>Importproces
Synchronisatie-engine geëvalueerd tijdens het importproces updates van identiteitsgegevens. Synchronisatie-engine vergelijkt de identiteitsgegevens van de verbonden gegevensbron met informatie over een staging-object hebt ontvangen en bepaalt of het object staging updates vereist. Indien nodig de staging-object bijgewerkt met nieuwe gegevens, wordt de staging-object gemarkeerd zoals in afwachting van importeren.

Doordat de objecten in de connector de ruimte vóór synchronisatie verwerken sync-engine alleen informatie over de gewijzigde. Dit proces heeft de volgende voordelen:

- **Efficiënte synchronisatie**. De hoeveelheid gegevens die worden verwerkt tijdens de synchronisatie wordt geminimaliseerd.
- **Efficiënte opnieuw synchroniseren**. U kunt wijzigen hoe sync engine identiteitsgegevens verwerkt zonder verbinding te maken van de synchronisatie-engine met de gegevensbron.
- **Voorproefje van synchronisatie**. U kunt synchronisatie om te controleren of uw veronderstellingen over het proces van identity management kloppen bekijken.

Voor elk object dat is opgegeven in de Connector, is de synchronisatie-engine eerst wordt gezocht naar een weergave van het object in de ruimte van de connector van de verbindingslijn. Synchronisatie-engine controleert alle tijdelijke objecten in de ruimte connector en gezocht naar een bijbehorende staging-object met een overeenkomende ankerkenmerk. Als er geen bestaande staging-object heeft een overeenkomende ankerkenmerk, wordt sync een bijbehorende staging-object met dezelfde naam DN zoeken.

Als synchronisatie-engine een staging-object dat overeenkomt met de door de DN-naam, maar niet door anker vindt, gebeurt het volgende speciale:

- Als het object zich bevindt in de ruimte connector geen anker heeft, sync-engine wordt dit object verwijderd uit de connector en markeert de metaverse-object die is gekoppeld aan als het **opnieuw inrichten van de volgende synchronisatie uitvoeren**. Vervolgens wordt het nieuwe object importeren gemaakt.
- Als het object zich bevindt in de connector ruimte een anker heeft, vervolgens sync-engine wordt ervan uitgegaan dat dit object is gewijzigd of verwijderd uit de map verbonden. Een tijdelijke, nieuwe DN-naam voor de connector ruimte object wilt toewijzen, zodat deze de binnenkomende object kunt voorbereiden. Het oude object wordt dan **tijdelijk**, wachten op de verbindingslijn te importeren, het wijzigen of verwijderen om de situatie te verhelpen.

Als de synchronisatie-engine wordt gezocht naar een staging-object dat overeenkomt met het object dat is opgegeven in de Connector, bepaalt wat voor soort wijzigingen toe te passen. Bijvoorbeeld synchronisatie-engine kan hernoemen of verwijderen van het object in de verbonden gegevensbron of het kan alleen werken met waarden van de kenmerken van het object.

Tijdelijke objecten met bijgewerkte gegevens afwachting importeren. Er zijn verschillende soorten in afwachting van invoer beschikbaar. Afhankelijk van het resultaat van het importproces is een van de volgende Importtypen in afwachting van een staging-object in de ruimte voor de verbindingslijn:

- **Geen**. Er zijn geen wijzigingen aangebracht in een van de kenmerken van de staging-object beschikbaar. Synchronisatie-engine vlag dit type niet zoals in afwachting van importeren.
- **Toevoegen**. De staging-object is een nieuw object importeren in de ruimte van de connector. Dit type vlaggen synchronisatie-engine zoals in afwachting van de import voor verdere verwerking in de metaverse.
- **Update**. Synchronisatie-engine vindt u een bijbehorende staging-object in de ruimte connector en vlaggen die dit type als in behandeling importeren zodat updates naar de kenmerken kunnen worden verwerkt in de metaverse. Updates bevatten de naam van object wijzigen.
- **Verwijderen**. Synchronisatie-engine zoekt een bijbehorende staging-object in de ruimte connector en vlaggen die dit type als in behandeling importeren zodat het gekoppelde object kan worden verwijderd.
- **Verwijderen/toevoegen**. Synchronisatie-engine zoekt een bijbehorende staging-object in de ruimte van de connector, maar de typen komen niet overeen. In dit geval een verwijderen toevoegen wijziging wordt klaargezet. Een delete-add wijziging geeft aan dat de synchronisatie-engine dat een volledig opnieuw synchroniseren van dit object omdat u verschillende sets van regels toepassen op dit object als het object typt wijzigingen moet plaatsvinden.

De status in behandeling importeren van een staging-object instelt, is het mogelijk aanzienlijk verminderen van de hoeveelheid gegevens die tijdens de synchronisatie omdat hierdoor het systeem voor het verwerken van objecten die gegevens bijgewerkt kan worden verwerkt.

### <a name="synchronization-process"></a>Tijdens synchronisatie
Synchronisatie bestaat uit twee verwante processen:

- Inkomende synchronisatie, wanneer de inhoud van de metaverse is bijgewerkt met de gegevens in de ruimte van de connector.
- Uitgaande synchronisatie, wanneer de inhoud van de ruimte van de connector is bijgewerkt met gegevens in de metaverse.

Met behulp van de informatie klaargezet in de connector ruimte maakt tijdens de synchronisatie van binnenkomende in de metaverse de geïntegreerde weergave van de gegevens die zijn opgeslagen in de gekoppelde gegevensbronnen. Alle tijdelijke objecten of alleen die met een in behandeling zijnde gegevens worden samengevoegd, afhankelijk van de configuratie van de regels.

De uitgaande synchronisatie proces updates exporteren objecten als metaverse objecten wijzigt.

Inkomende synchronisatie maakt de geïntegreerde weergave in de metaverse van de informatie die is ontvangen van de gekoppelde gegevensbronnen. Synchronisatie-engine identiteitsgegevens op elk gewenst moment kan worden verwerkt met behulp van de meest recente informatie identiteit heeft uit de gekoppelde gegevensbron.

**Inkomende synchronisatie**

Inkomende synchronisatie omvat de volgende processen:

- **Bepaling** (ook wel **projectie** als het is belangrijk dat dit proces onderscheiden van het inrichten van uitgaande synchronisatie). De synchronisatie-engine een nieuwe metaverse-object op basis van een staging-object maakt en koppelt deze. Er is een bewerking op objectniveau.
- **Deelnemen aan**. De synchronisatie-engine een staging-object bevat koppelingen naar een bestaand object in de metaverse. Een join is een bewerking op objectniveau.
- **Import-kenmerk stroom**. Synchronisatie-engine werkt de waarden van het kenmerk kenmerk flow, van het object in de metaverse genoemd. Import-kenmerk stroom is een kenmerk op toepassingsniveau bewerking waarvoor een koppeling tussen een staging-object en een object metaverse.

Bepaling is het enige proces waarmee objecten in de metaverse. Bepaling beïnvloedt alleen importeren objecten afzonderlijke objecten zijn. Bepaling, tijdens de maakt synchronisatie-engine een metaverse die overeenkomt met het objecttype van het object importeren en ontstaat er een koppeling tussen beide objecten, zodat u een gekoppelde object.

Het proces wordt ook een koppeling tussen objecten importeren en een object metaverse. Het verschil tussen de join en de levering is het proces is vereist dat de importobject zijn gekoppeld aan een bestaand object metaverse, waarbij het proces bepaling maakt een nieuw object in de metaverse.

Synchronisatie-engine probeert een object importeren toevoegen aan een object metaverse op basis van criteria die is opgegeven in de configuratie van de regel van de synchronisatie.

Tijdens het verrichten en join, sync-engine een object niet langer lid gekoppeld aan een object metaverse, waardoor ze verbonden. Nadat deze op objectniveau bewerkingen zijn voltooid, kunt synchronisatie-engine de kenmerkwaarden van de metaverse gekoppeld object bijwerken. Dit proces heet import-kenmerk stroom.

Import-kenmerk flow plaatsvindt op alle objecten voor importeren die nieuwe gegevens bevatten en die zijn gekoppeld aan een object metaverse.

**Uitgaande synchronisatie**

Uitgaande synchronisatie updates exporteren objecten als een metaverse object wijzigen, maar niet verwijderd. Het doel van uitgaande synchronisatie is te beoordelen of de wijzigingen in de metaverse objecten vereisen updates van tijdelijke objecten in de ruimten van de connector. In sommige gevallen de wijzigingen kunnen verplicht stellen dat tijdelijke objecten in alle ruimten van de connector worden bijgewerkt. Staging-objecten die zijn gewijzigd, worden gemarkeerd als in afwachting van uitvoer, waardoor ze objecten exporteren. Deze objecten later naar buiten wijzen naar de verbonden gegevensbron tijdens het exporteren exporteren.

Uitgaande synchronisatie heeft drie processen:

- **Inrichten**
- **Deprovisioning**
- **Kenmerk flow exporteren**

Inrichting en deprovisioning zijn beide bewerkingen op objectniveau. Deprovisioning is afhankelijk van het inrichten omdat alleen ingericht kunt starten. Deprovisioning wordt geactiveerd bij het inrichten van verwijdert u de koppeling tussen een metaverse-object en een object exporteren.

Provisioning wordt altijd geactiveerd wanneer wijzigingen worden toegepast op objecten in de metaverse. Wanneer wijzigingen worden aangebracht in objecten metaverse, kunt sync engine uitvoeren van de volgende taken als onderdeel van het inrichtingsproces:

- Gekoppelde objecten, waarbij een metaverse-object is gekoppeld aan een nieuwe export-object maken.
- Wijzig de naam van een gekoppelde object.
- Meld koppelingen tussen een object metaverse en tijdelijke objecten, het maken van een object niet langer lid.

Als inrichten sync-engine een nieuwe connectorobject te maken, is de staging-object waaraan de metaverse-object is gekoppeld altijd een object exporteren, omdat het object nog niet in de gekoppelde gegevensbron bestaat.

Als synchronisatie-engine een gekoppelde object maken van een object niet langer lid, meld inrichten vereist wordt deprovisioning geactiveerd. De procedure deprovisioning verwijdert het object.

Tijdens de deprovisioning verwijdert verwijderen van een object exporteren niet fysiek het object. Het object is gemarkeerd als **verwijderd**, wat betekent dat de bewerking delete op het object wordt klaargezet.

Export kenmerk stroom treedt ook op tijdens de synchronisatie van uitgaande, vergelijkbaar met de manier waarop dat import-kenmerk flow tijdens de synchronisatie van binnenkomende plaatsvindt. Export kenmerk flow plaatsvindt tussen metaverse en exporteren van objecten die deel uitmaken.

### <a name="export-process"></a>Exporteren
Synchronisatie-engine controleert tijdens het exporteren alle export-objecten die zijn gemarkeerd als in afwachting van uitvoer in de ruimte van de verbindingslijn en vervolgens updates verzonden naar de verbonden gegevensbron.

De synchronisatie-engine kunt bepalen het succes van een export maar voldoende niet bepalen dat de identity management voltooid is. Objecten in de verbonden gegevensbron kunnen altijd worden gewijzigd door andere processen. Synchronisatie-engine geen een permanente verbinding heeft met de verbonden gegevensbron, is het niet voldoende om hypothesen over de eigenschappen van een object in de verbonden gegevensbron alleen gebaseerd is op een kennisgeving te exporteren.

Een proces in de verbonden gegevensbron kan bijvoorbeeld de kenmerken van het object veranderen terug naar hun oorspronkelijke waarden (dat wil zeggen, de verbonden gegevensbron kan overschrijven de waarden onmiddellijk nadat de gegevens worden door sync motor geactiveerd en in de verbonden gegevensbron toegepast).

De synchronisatie-engine winkels exporteren en importeren van statusinformatie over elk staging-object. Als de waarden van de kenmerken die zijn opgegeven in het kenmerklijst hebt gewijzigd sinds de laatste uitvoertransactie, wordt de opslag van importeren en exporteren van status kunt sync engine om te reageren op de juiste manier. Synchronisatie-engine gebruikt tijdens het importeren te bevestigen, waarden die zijn geëxporteerd naar de verbonden gegevensbron. Een vergelijking tussen de geïmporteerde en geëxporteerde informatie, kunt zoals in de volgende afbeelding u synchronisatie-engine om te bepalen of het exporteren is voltooid, of als deze moeten worden herhaald.

![Arch7](./media/active-directory-aadconnectsync-understanding-architecture/arch7.png)

Bijvoorbeeld als de synchronisatie-engine exporteert kenmerk C, met een waarde van 5, aan een gegevensbron is verbonden, wordt opgeslagen C = 5 in het geheugen van de status exporteren. Elke extra uitvoer van de resultaten van dit object in een poging opnieuw te exporteren C = 5 met de verbonden gegevensbron omdat synchronisatie-engine wordt ervan uitgegaan dat deze waarde niet aanhoudend op het object toegepast is (tenzij een andere waarde is onlangs geïmporteerd vanuit de verbonden gegevensbron). Het geheugen van de uitvoer wordt gewist wanneer C = 5 tijdens een importbewerking op het object is ontvangen.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over het configureren van [Azure AD Connect worden gesynchroniseerd](active-directory-aadconnectsync-whatis.md) .

Meer informatie over de [integratie van uw identiteiten op ruimten met Azure Active Directory](active-directory-aadconnect.md).
