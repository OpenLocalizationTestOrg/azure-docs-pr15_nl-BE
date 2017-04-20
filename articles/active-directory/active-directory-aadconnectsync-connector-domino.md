<properties
   pageTitle="Connector voor Lotus Domino | Microsoft Azure"
   description="In dit artikel wordt beschreven hoe Microsoft Lotus Domino-Connector configureren."
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.workload="identity"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="08/30/2016"
   ms.author="billmath"/>

# <a name="lotus-domino-connector-technical-reference"></a>Technische naslaginformatie voor Lotus Domino-Connector
Dit artikel beschrijft de Lotus Domino-Connector. Het artikel is van toepassing op de volgende producten:

- Microsoft Identity Manager 2016 (MIM2016)
- Forefront Identiteitenbeheer 2010 R2 (FIM2010R2)
    -   Moet hotfix 4.1.3671.0 of later [KB3092178](https://support.microsoft.com/kb/3092178)gebruiken.

De Connector is voor MIM2016 en FIM2010R2, worden gedownload vanaf het [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=717495).

## <a name="overview-of-the-lotus-domino-connector"></a>Overzicht van de Lotus Domino-Connector
De Lotus Domino-Connector kunt u de synchronisatieservice integreren met IBM Lotus Domino-server.

Vanuit een perspectief op hoog niveau, worden de volgende functies worden ondersteund door de huidige versie van de verbindingslijn:

Functie | Ondersteuning
--- | ---
Verbonden gegevensbron | Server: <li>Lotus Domino 8.5.x</li><li>Lotus Domino 9.x</li>Client:<li>Lotus Notes-9.x</li>
Scenario 's | <li>Levenscyclusbeheer voor uw object</li><li>Groepsbeheer</li><li>Wachtwoordbeheer</li>
Bewerkingen | <li>Volledige en Delta importeren</li><li>Exporteren</li><li>Instellen en wijzigen van het wachtwoord op HTTP wachtwoord</li>
Schema | <li>Persoon (zwervende gebruiker, contactpersoon (voor personen met geen certificaat))</li><li>Groep</li><li>Bron (bron, kamer, on line vergadering)</li><li>E-mailberichten in database</li><li>Dynamische detectie van kenmerken voor ondersteunde objecten</li>

De connector voor Lotus Domino wordt de Lotus Notes-client gebruikt om te communiceren met Lotus Domino-Server. Als gevolg van deze afhankelijkheid, moet een ondersteunde Lotus Notes-Client zijn geïnstalleerd op de server voor de synchronisatie. De communicatie tussen de client en de server wordt geïmplementeerd via de interface van Lotus Notes .NET Interop (Interop.domino.dll). Deze interface vergemakkelijkt de communicatie tussen de Microsoft.NET platform en Lotus Notes-client en ondersteunt toegang tot Lotus Domino-documenten en weergaven. Voor het importeren van delta is het ook mogelijk dat de systeemeigen C++-interface wordt gebruikt (afhankelijk van de importeermethode van geselecteerde delta).

### <a name="prerequisites"></a>Vereisten
Voordat u de verbindingslijn gebruiken, moet u het volgende op de server voor de synchronisatie:

- 4.5.2 van Microsoft .NET Framework of later
- De Lotus Notes-client moet worden geïnstalleerd op uw server voor de synchronisatie
- De Connector Lotus Domino vereist Lotus Domino LDAP schema standaarddatabase (schema.nsf) aanwezig zijn op de Domino Directory-server. Als niet aanwezig is, kunt u deze installeren door het uitvoeren of de LDAP-service op de Domino-server opnieuw te starten.

### <a name="connected-data-source-permissions"></a>Verbonden gegevensbron machtigingen
Als u een van de ondersteunde taken in-connector voor Lotus Domino, moet u lid zijn van de volgende groepen:

- Volledige toegang beheerders
- Beheerders
- Databasebeheerders

De volgende tabel geeft een overzicht van de machtigingen die vereist voor elke bewerking zijn:

Bewerking | Toegangsrechten
--- | ---
Importeren | <li>Openbare documenten lezen</li><li> Volledige Administrator toegang (als u lid van de groep administrators volledige toegang bent, beschikt u automatisch over de daadwerkelijke toegang tot in de ACL.)</li>
Exporteren en het wachtwoord instellen | Effectieve toegang: <li>Documenten maken</li><li>Documenten verwijderen</li><li>Openbare documenten lezen</li><li>Openbare documenten schrijven</li><li>Repliceren of kopie van documenten</li>Voor uitvoer-bewerkingen moet u ook de volgende functies: <li>CreateResource</li><li>GroupCreator</li><li>GroupModifier</li><li>UserCreator</li><li>UserModifier</li>

### <a name="direct-operations-and-adminp"></a>Directe verrichtingen en AdminP
Bewerkingen Ga rechtstreeks naar de Domino directory of via het AdminP proces. De volgende tabellen ondersteunde lijstobjecten, bewerkingen en, indien van toepassing, de bijbehorende implementatie methode:

**Het primaire adresboek**

Object | Maken | Update | Verwijderen
--- | --- | --- | ---
Persoon | AdminP | Directe | AdminP
Groep | AdminP | Directe | AdminP
MailInDB | Directe | Directe | Directe
Resource | AdminP | Directe | AdminP

**Secundaire adresboek**

Object | Maken | Update | Verwijderen
--- | --- | --- | ---
Persoon | N.V.T. | Directe | Directe
Groep | Directe | Directe | Directe
MailInDB | Directe | Directe | Directe
Resource | N.V.T. | N.V.T. | N.V.T.

Wanneer een resource wordt gemaakt, wordt een Notes-document gemaakt. Op dezelfde manier als een resource wordt verwijderd, wordt het Notes-document verwijderd.

### <a name="ports-and-protocols"></a>Poorten en protocollen
IBM Lotus Notes-client en Domino-servers communiceren met behulp van notities Remote Procedure Call (NRPC) waar NRPC van TCP/IP gebruikmaken moeten. Het standaardpoortnummer is 1352, maar kan worden gewijzigd door de beheerder Domino.

### <a name="not-supported"></a>Niet ondersteund
De volgende bewerkingen worden niet ondersteund door de huidige versie van de connector voor Lotus Domino:

- Postvak verplaatsen tussen servers.

## <a name="create-a-new-connector"></a>Een nieuwe verbindingslijn maken

### <a name="client-software-installation-and-configuration"></a>Client Software-installatie en configuratie
Lotus Notes moeten worden geïnstalleerd op de server **voordat u** die de Connector is geïnstalleerd.

Als u installeert, moet dat u doen met een **Installatie voor één gebruiker**. De standaard **Installatie met meerdere gebruikers** werkt niet.  
![Notes1](./media/active-directory-aadconnectsync-connector-domino/notes1.png)

Installeer alleen de vereiste functies van Lotus Notes en **Eenmalige aanmelding Client**op de pagina onderdelen. Eenmalige aanmelding is vereist voor de connector te kunnen aanmelden bij het Domino-server.  
![Notes2](./media/active-directory-aadconnectsync-connector-domino/notes2.png)

**Opmerking:** Lotus Notes eenmaal beginnen met een gebruiker die zich bevindt op dezelfde server als de account die u als de serviceaccount van de verbindingslijn gebruikt. Controleer ook of de Lotus Notes-client op de server te sluiten. Het kan niet worden uitgevoerd op het moment dat de Connector probeert verbinding te maken met de Domino-server.

### <a name="create-connector"></a>Verbindingslijn maken
Als u een Lotus Domino-connector in de **Tijdsynchronisatie-Service** selecteert **Management Agent** en **maken**. Selecteer de verbindingslijn **Lotus Domino (Microsoft)** .  
![CreateConnector](./media/active-directory-aadconnectsync-connector-domino/createconnector.png)

Als uw versie van de tijdsynchronisatie-service de mogelijkheid om te configureren **architectuur biedt**, zorg ervoor dat de connector is ingesteld op de standaardwaarde in het **proces**uit te voeren.

### <a name="connectivity"></a>Connectiviteit
Op de pagina verbindingen moet u de Lotus Domino-server opgeven en de aanmeldingsgegevens invoeren.  
![Connectiviteit](./media/active-directory-aadconnectsync-connector-domino/connectivity.png)

De eigenschap Domino-Server ondersteunt twee verschillende indelingen voor de server:

- Servernaam
- Servernaam/DirectoryName

De **Servernaam/DirectoryName** is de beste indeling voor dit kenmerk omdat hiermee de snellere respons bij Domino-Server contact met de connector.

De opgegeven gebruikers-id-bestand wordt opgeslagen in de configuratiedatabase van de synchronisatieservice.

U hebt deze opties voor het **Importeren van Delta** :

- **Geen**. De verbindingslijn wordt niet ingevoerd delta.
- **Toevoegen/bijwerken**. De verbindingslijn wordt delta-import toevoegen en bijwerken. Verwijderen is een **Volledige** importbewerking vereist. Deze bewerking wordt gebruikt door de .net interop.
- **Toevoegen, bijwerken of verwijderen**. De verbindingslijn wordt delta importeren toevoegen, bijwerken en verwijderen. Deze bewerking wordt de systeemeigen C++-interfaces gebruikt.

U hebt de volgende opties in de **Schema-opties** :

- **Standaard Schema**. De Connector herkent het schema van de Domino-server. Dit is de standaardoptie.
- **DSML-Schema**. Alleen gebruikt als het Domino-server niet voor het schema weergegeven wordt. Vervolgens kunt u een DSML-bestand maken met het schema en deze beter importeren. Zie voor meer informatie over DSML, [OASIS](https://www.oasis-open.org/committees/tc_home.php?wg_abbrev=dsml).

Als u op Volgende klikt, worden de configuratieparameters voor de gebruikers-id en het wachtwoord gecontroleerd.

### <a name="global-parameters"></a>Algemene Parameters
Op de pagina Algemene Parameters configureren voor de tijdzone en het importeren en exporteren de optie bij bewerking.  
![Algemene Parameters](./media/active-directory-aadconnectsync-connector-domino/globalparameters.png)

De parameter **tijdzone Domino-Server** bepaalt de locatie van uw Domino-Server.

Deze optie is vereist voor **delta importeren** omdat de synchronisatie service bepalen tussen de twee laatste invoer.

#### <a name="import-settings-method"></a>Instellingen voor importeren, methode
Bevat de **Volledige importeren uitvoeren door** deze opties:

- Zoeken
- Weergave (aanbevolen)

**Zoeken** in Domino indexeren wordt gebruikt, maar is het gebruikelijk dat de indexen niet in real-time bijgewerkt worden en de gegevens die zijn geretourneerd door de server niet altijd juist is. Voor een systeem met veel wijzigingen deze optie gewoonlijk werkt niet goed en biedt onwaar wordt verwijderd in bepaalde situaties. **Zoeken** is echter sneller dan de **weergave**.

**Weergave** is de aanbevolen optie, omdat het biedt de juiste status van de gegevens. Het is iets trager dan zoeken.

#### <a name="creation-of-virtual-contact-objects"></a>Het maken van virtuele contactpersoonobjecten
De **kunnen maken \_object Contact** heeft de volgende opties:

- Geen
- Niet-referentiewaarden
- Naslaggids voor niet waarden

In Domino bevatten verwijzing kenmerken veel verschillende indelingen om te verwijzen naar andere objecten. Kunnen verschillende variaties, de Connector implementeert \_contact op met objecten, ook wel bekend als **Virtuele contactpersonen** (VC). Deze objecten worden gemaakt zodat u ze kunnen koppelen aan bestaande MV objecten of als nieuwe objecten worden geprojecteerd. Op deze manier kunnen kenmerk verwijzingen worden behouden.

Door deze instelling inschakelt en als de inhoud van het kenmerk van een verwijzing niet de indeling van een DN-naam is, een \_Contact-object is gemaakt. Een Lidkenmerk van een groep kan bijvoorbeeld de SMTP-adressen bevatten. Het is ook mogelijk om korte naam en andere kenmerken aanwezig zijn in de kenmerken van de verwijzing. Selecteer in dit scenario **Niet referentiewaarden**. Deze configuratie is de meestgebruikte instelling voor de Domino-implementaties.

Wanneer u Lotus Domino is geconfigureerd als afzonderlijke adresboeken hebt met een andere DN-namen voor hetzelfde object, is het mogelijk te maken ook \_contact op met objecten voor alle referentiewaarden die zijn gevonden in een adresboek. Selecteer de optie **referentie- en niet-referentiewaarden** voor dit scenario.

Als er meerdere waarden in het kenmerk **volledige naam** in Domino, vervolgens wilt u ook dat het maken van virtuele contactpersonen zodat verwijzingen kunnen worden opgelost. Dit kenmerk kan bijvoorbeeld meerdere waarden hebben na een huwelijk of echtscheiding. Schakel het selectievakje **inschakelen... FullName meerdere waarden bevat** voor dit scenario.

Door lid te worden op de juiste kenmerken, de \_contactpersoonobjecten zou worden toegevoegd aan het object MV.

Deze objecten hebben VC =\_contactpersoon toegevoegd aan de DN-naam.

#### <a name="import-settings-conflict-object"></a>Instellingen importeren, conflict, object
**Uitsluiten van Conflict-Object**

In een grote Domino-implementatie is het mogelijk dat meerdere objecten de dezelfde DN vanwege replicatie op te lossen hebben. In deze gevallen ziet de verbindingslijn twee objecten met verschillende UniversalIDs, maar met dezelfde DN-naam. Dit conflict zou leiden tot een tijdelijk object dat wordt gemaakt in de ruimte van de connector. De verbindingslijn kunt negeren de objecten die zijn geselecteerd in Domino als slachtoffers van replicatie. De aanbeveling is dit selectievakje ingeschakeld te houden.

#### <a name="export-settings"></a>Instellingen exporteren
Als de optie **AdminP voor het bijwerken van verwijzingen gebruiken** geselecteerd is, export van verwijzing kenmerken, zoals de lidstaten, is een directe aanroep en maakt geen gebruik van het AdminP-proces. Gebruik deze optie alleen als AdminP niet is geconfigureerd om referentiële integriteit te behouden.

#### <a name="routing-information"></a>Informatie over routering
In Domino is het mogelijk dat een kenmerk verwijzing routeringsgegevens ingesloten als achtervoegsel aan de DN-naam heeft. Het Lidkenmerk in een groep kan bevatten bijvoorbeeld **CN=example/organization@ABC**. Het achtervoegsel @ABC is de informatie over routering. De routeringsgegevens wordt voor het verzenden van e-mails naar de juiste Domino waarin een systeem in een andere organisatie kan worden gebruikt door Domino. In het veld informatie routering kunt u de achtervoegsels routeren gebruikt binnen de organisatie in het bereik van de verbindingslijn. Als een van deze waarden in een verwijzing kenmerk als achtervoegsel wordt gevonden, wordt de routeringsgegevens verwijderd uit de verwijzing. Als het achtervoegsel routering op de referentie-waarde komt niet met een van die waarden die zijn opgegeven overeen, een \_Contact-object is gemaakt. Deze \_objecten Contact worden gemaakt met ** RO=@ ** ingevoegd in de DN-naam. Voor deze \_Contact objecten de volgende kenmerken worden ook toegevoegd aan kunnen lid worden van een echt object indien nodig: \_routingName, \_contactName, \_displayName, en UniversalID.

#### <a name="additional-address-books"></a>Extra adresboeken
Als u nog geen **telefoonboek** geïnstalleerd, waarmee de naam van de secundaire adresboeken, kunt u deze adresboeken handmatig invoeren.

#### <a name="multivalued-transformation"></a>Transformatie met meerdere waarden
Er zijn veel kenmerken in Lotus Domino met meerdere waarden. De bijbehorende metaverse zijn meestal één gewaardeerd. Door de invoer en de bewerking exportoptie te configureren, moet u de verbindingslijn om te helpen bij de vereiste omrekening van de gewijzigde kenmerken inschakelen.

**Exporteren**  
De bewerking exportoptie ondersteunt twee modi:

- Item toevoegen
- Item vervangen

**Item vervangen** – wanneer u deze optie selecteert, de connector wordt altijd de huidige waarden van het kenmerk in Domino verwijderen en vervangen door de opgegeven waarden. De opgegeven waarde één waarde of meerdere waarden kunnen worden.

Voorbeeld: Het kenmerk van de assistent van een persoon-object heeft de volgende waarden:

- CN = Greg Winston/OU=Contoso/O=Americas,NAB=names.nsf
- CN = Jan Smith/OU=Contoso/O=Americas,NAB=names.nsf

Als u een nieuwe assistent met de naam **David Alexander** is toegewezen aan deze persoon-object, is het resultaat:

- CN = David Alexander/OU=Contoso/O=Americas,NAB=names.nsf

**Item toevoegen** – wanneer u deze optie selecteert, de connector blijft de bestaande waarden voor het kenmerk in het Domino en nieuwe waarden invoegen boven aan de lijst.

Voorbeeld: Het kenmerk van de assistent van een persoon-object heeft de volgende waarden:

- CN = Greg Winston/OU=Contoso/O=Americas,NAB=names.nsf
- CN = Jan Smith/OU=Contoso/O=Americas,NAB=names.nsf

Als u een nieuwe assistent met de naam **David Alexander** is toegewezen aan deze persoon-object, is het resultaat:

- CN = David Alexander/OU=Contoso/O=Americas,NAB=names.nsf
- CN = Greg Winston/OU=Contoso/O=Americas,NAB=names.nsf
- CN = Jan Smith/OU=Contoso/O=Americas,NAB=names.nsf

**Importeren**  
De optie importeren bewerking ondersteunt twee modi:

- Standaard
- Enkele waarde met meerdere waarden

Alle waarden van alle kenmerken **standaard** – wanneer u de optie selecteert, worden geïmporteerd.

**Multivalued enkele waarde** – wanneer u deze optie, een kenmerk met meerdere waarden selecteert wordt omgezet in een kenmerk met één waarde. Als er meer dan één waarde bestaat, wordt de waarde op de bovenkant (deze waarde is meestal ook de meest recente) gebruikt.

Voorbeeld: Het kenmerk van de assistent van een persoon-object heeft de volgende waarden:

- CN = David Alexander/OU=Contoso/O=Americas,NAB=names.nsf
- CN = Greg Winston/OU=Contoso/O=Americas,NAB=names.nsf
- CN = Jan Smith/OU=Contoso/O=Americas,NAB=names.nsf

De meest recente update voor dit kenmerk is **David Alexander**. Omdat de optie importeren bewerking Multivalued enkele waarde is ingesteld, importeert connector alleen **David Alexander** in de ruimte van de connector.

De logica voor kenmerken met meerdere waarden converteren naar kenmerken met één waarde is niet van toepassing op het kenmerk groep lid en met het kenmerk persoon fullname.

Het is ook mogelijk voor het configureren van importeren en exporteren van regels voor kenmerken met meerdere waarden per kenmerk transformatie als uitzondering op de algemene regel. Voer voor het configureren van deze optie [objecttype]. [attributename] in de **lijst met uitgesloten kenmerken importeren** en **exporteren van de lijst met uitgesloten kenmerken** tekstvakken. Bijvoorbeeld, als u Person.Assistant invoert en de globale vlag is ingesteld op alle waarden importeren, is alleen de eerste waarde geïmporteerd voor de Office-assistent.

#### <a name="certifiers"></a>Certifiers
Alle organisatie/organisatorische eenheden worden weergegeven door de connector. Als u persoon objecten exporteren naar het primaire adresboek, is een certifier met het wachtwoord vereist.

Als alle certifiers hetzelfde wachtwoord hebt, kan het **wachtwoord voor alle Certifers** worden gebruikt. Vervolgens kunt u hier het wachtwoord invoeren en geeft u alleen het bestand certifier.

Als u alleen importeren vervolgens hoeft niet u certifiers opgeven.

### <a name="configure-provisioning-hierarchy"></a>Provisioning hiërarchie configureren
Bij het configureren van de connector voor Lotus Domino, slaat u deze pagina van het dialoogvenster. De connector voor Lotus Domino ondersteunt geen hiërarchie wordt ingericht.  
![Inrichten van hiërarchie](./media/active-directory-aadconnectsync-connector-domino/provisioninghierarchy.png)

### <a name="configure-partitions-and-hierarchies"></a>Configureren, partities en hiërarchieën
Als u partities en hiërarchieën configureert, moet u het primaire adresboek NAB=names.nsf genoemd. Naast de primaire adresboek kunt u secundaire adresboeken indien aanwezig.  
![Partities](./media/active-directory-aadconnectsync-connector-domino/partitions.png)

### <a name="select-attributes"></a>Selecteer kenmerken
Als u de kenmerken wilt configureren, moet u alle kenmerken die worden voorafgegaan door ** \_mms://-URL\_**. Deze kenmerken zijn vereist bij het inrichten van nieuwe Lotus Domino-objecten

![Kenmerken](./media/active-directory-aadconnectsync-connector-domino/attributes.png)

## <a name="object-lifecycle-management"></a>Levenscyclusbeheer voor uw object
Deze sectie bevat een overzicht van de verschillende objecten in Domino.

### <a name="person-objects"></a>Persoon-objecten
Het object persoon stelt gebruikers in de organisatie en organisatie-eenheden. Naast de standaardkenmerken kunt de Domino-beheerder aangepaste kenmerken toevoegen aan een persoon-object. Een persoon object omvat ten minste alle verplichte kenmerken. Zie voor een volledige lijst van verplichte kenmerken [Lotus Notes-eigenschappen](#lotus-notes-properties). Om een object persoon registreren, moeten de volgende vereisten worden voldaan:

- Het adresboek (names.nsf) moet zijn gedefinieerd en moet het primaire adresboek.
- Hebt u de certifier O/OU-Id en het wachtwoord voor het registreren van een bepaalde gebruiker in de organisatie / organisatie-eenheid.
- U moet een specifieke set van Lotus Notes-eigenschappen voor een persoon-object ingesteld. Deze eigenschappen worden gebruikt voor het inrichten van het object persoon. Zie de sectie [Lotus Notes-eigenschappen](#lotus-notes-properties) verderop in dit document voor meer informatie.
- Het initiële HTTP-wachtwoord voor een persoon is een kenmerk en set tijdens het inrichten.
- Het object persoon moet een van de volgende drie typen ondersteund:
    1. Normale gebruiker die beschikt over een e-mail en een gebruiker-id-bestand
    2. Zwervende gebruiker (een normale gebruiker met alle databasebestanden zwervend)
    3. Contactpersonen (gebruiker met geen id-bestand)

Personen (met uitzondering van contactpersonen) kunnen verder worden gegroepeerd in ons gebruikers en internationale gebruikers, zoals gedefinieerd door de waarde van de \_MMS\_IDRegType eigenschap. Deze mensen gebruiken de Notes-Client toegang tot Lotus Domino-servers hebben een notities-Id en een document van de persoon. Als ze Notes mail hebt ze ook een e-mailbestand. De gebruiker moet worden geregistreerd om te worden geactiveerd. Zie voor meer informatie:

- [Notes-gebruikers instellen](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_SETTING_UP_NOTES_USERS.html)
- [Gebruiker registreren](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_REGISTERING_USERS.html)
- [Gebruikers beheren](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_MANAGING_USERS_5151.html)
- [De naam van gebruikers](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_RENAMING_A_USER_AUTOMATICALLY.html)

Alle volgende bewerkingen worden uitgevoerd in Lotus Domino en vervolgens geïmporteerd in de tijdsynchronisatie-service.

### <a name="resources-and-rooms"></a>Bronnen en kamers
Een bron is een ander type van een database in Lotus Domino. Resources kunnen vergaderruimten met verschillende soorten uitrusting, zoals projectors worden. Er zijn subtypen van bronnen die worden ondersteund door Lotus Domino-connector die zijn gedefinieerd door het kenmerk Type Resource:

Type Resource | Kenmerk Type resource
--- | ---
Kamer | 1
Resource (Overig) | 2
On line vergadering | 3

Het volgende is vereist voor het type Resource-object te werken:

- Resource Reservation database moet reeds bestaan in de verbonden Domino-server
- De site is al gedefinieerd voor de Resource

De reservering van de Resource-database bevat drie soorten documenten:

- Profiel van uw site
- Resource
- Reservering

Zie voor meer informatie over het instellen van database Resource Reservation [instellen van de database Resource reserveringen](https://www-01.ibm.com/support/knowledgecenter/SSKTMJ_8.0.1/com.ibm.help.domino.admin.doc/DOC/H_SETTING_UP_THE_RESOURCE_RESERVATIONS_DATABASE.html).

**Maken, bijwerken en verwijderen van bronnen**  
De bewerkingen maken, bijwerken en verwijderen worden uitgevoerd door de connector voor Lotus Domino in de database Resource reserveren. Resources worden gemaakt als documenten in Names.nsf (dat wil zeggen, de primaire adresboek). Zie voor meer informatie over het bewerken en verwijderen van bronnen, [bewerken en verwijderen van brondocumenten](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_EDITING_AND_DELETING_RESOURCE_DOCUMENTS.html).

**Importeren en exporteren voor Resources**  
De Resources kunnen worden geïmporteerd in en geëxporteerd vanuit de synchronisatieservice, net als een ander objecttype. Selecteer het objecttype als bron tijdens de configuratie. Voor de exportbewerking is voltooid, moet u de details voor het brontype conferentie Database en Site-naam hebben.

### <a name="mail-in-databases"></a>E-mailberichten In Databases
Een Mail-In-Database is een database die speciaal is ontworpen voor het ontvangen van e-mailberichten. Een die niet is gekoppeld aan een specifieke gebruikersaccount voor Lotus Domino Lotus Domino-postvak is (dat wil zeggen, het hoeft niet een eigen bestand ID en wachtwoord). Een mail-in-database heeft een unieke gebruikers-id ("korte naam") en een eigen e-mailadres gekoppeld.

Als er is behoefte aan een afzonderlijke postbus met een eigen e-mailadres dat kan worden gedeeld door verschillende gebruikers (bijvoorbeeld, group@contoso.com), een mail-in-database wordt gemaakt. De toegang tot dit postvak wordt bepaald door de lijst ACL (Access Control), die de namen van de Notes-gebruikers die zijn toegestaan bevat voor het openen van de postbus.

Zie de sectie [Verplichte kenmerken](#mandatory-attributes) verderop in dit artikel voor een lijst van de vereiste kenmerken.

Wanneer een database is ontworpen om een e-mail te ontvangen, wordt een document voor Afdruk In de Database gemaakt in Lotus Domino. Dit document moet bestaan in Domino Directory van elke server waarop een kopie van de database worden opgeslagen. Zie voor een gedetailleerde beschrijving over het maken van een databasedocument voor afdruk in [maken van een document voor Afdruk In de Database](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_CREATING_A_MAILIN_DATABASE_DOCUMENT_FOR_A_NEW_DATABASE_OVERVIEW.html).

Voordat u een afdruk In Database maakt, moet de database al bestaan (moeten worden gemaakt door Lotus Admin) op de Domino server.

### <a name="group-management"></a>Groepsbeheer
U krijgt een gedetailleerd overzicht van het beheer van Lotus Domino-groep uit de volgende bronnen:

- [Werken met groepen](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_USING_GROUPS_OVER.html)
- [Een groep maken](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_CREATING_AND_MODIFYING_GROUPS_STEPS_MIDTOPIC_55038956829238418.html)
- [Maken en wijzigen van groepen](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_CREATING_AND_MODIFYING_GROUPS_STEPS.html)
- [Groepen beheren](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_MANAGING_GROUPS_1804.html)
- [De naam van een groep](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_RENAMING_A_GROUP_STEPS.html)

### <a name="password-management"></a>Wachtwoordbeheer
Er zijn twee typen wachtwoorden voor een geregistreerde gebruiker van de Lotus Domino:

1. Gebruikerswachtwoord (opgeslagen in het bestand User.id)
2. Internet / HTTP wachtwoord

De connector voor Lotus Domino ondersteunt alleen bewerkingen met HTTP-wachtwoord.

U moet voor het uitvoeren van wachtwoordbeheer wachtwoordbeheer voor de connector in de ontwerpfunctie voor Management Agent inschakelen. Als u wachtwoord management, schakelt **Wachtwoordbeheer inschakelen** in het dialoogvenster **Extensies configureren** pagina.  
![Uitbreidingen configureren](./media/active-directory-aadconnectsync-connector-domino/configureextensions.png)

De Lotus Domino-connector ondersteuning voor volgende bewerkingen op het Internet-wachtwoord:

- Instellen van het wachtwoord: Wachtwoord instellen wordt een nieuw wachtwoord voor HTTP-/ Internet op de gebruiker in Domino. Standaard is de account ook ontgrendeld. De vlag ontgrendelen wordt weergegeven op de WMI-interface van de synchronisatie-Engine.
- Wachtwoord wijzigen: In dit scenario kan een gebruiker mogelijk wilt wijzigen van het wachtwoord of wordt gevraagd wachtwoord te wijzigen na een bepaalde tijd. Voor deze bewerking op te nemen, dat zowel (het oude en het nieuwe wachtwoord) zijn verplicht. Zodra gewijzigd, wordt het nieuwe wachtwoord wordt bijgewerkt in Lotus Domino.

Zie voor meer informatie:

- [Met behulp van de functie Internet-vergrendeling](http://www.ibm.com/developerworks/lotus/library/domino8-lockout/)
- [Internet-wachtwoorden beheren](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_NOTES_AND_INTERNET_PASSWORD_SYNCHRONIZATION_7570_OVER.html)

## <a name="reference-information"></a>Naslaginformatie
Deze sectie bevat zoals beschrijvingen van kenmerken en vereisten voor de connector voor Lotus Domino kenmerk.

### <a name="lotus-notes-properties"></a>Lotus Notes-eigenschappen
Wanneer u objecten van de persoon in de map Lotus Domino inrichten, moeten uw objecten een bepaalde set eigenschappen hebben met specifieke waarden ingevuld. Deze waarden zijn alleen vereist voor bewerkingen maken.

De volgende tabel geeft een overzicht van deze eigenschappen en bevat een beschrijving van deze.

Eigenschap | Beschrijving
--- | ---
\_MMS_AltFullName | De alternatieve volledige naam van de gebruiker.
\_MMS_AltFullNameLanguage | De taal voor die de alternatieve naam van de gebruiker.
\_MMS_CertDaysToExpire | Het aantal dagen vanaf de huidige datum vóór het certificaat verloopt. Als u niet opgeeft, wordt standaard twee jaar vanaf de huidige datum.
\_MMS_Certifier | Eigenschap met de naam van de organisatie-hiërarchie van de certifier. Bijvoorbeeld: OU = OrganizationUnit, O = Org, C = land.
\_MMS_IDPath | Als de eigenschap leeg is, wordt er geen gebruiker-id-bestand lokaal op de Server synchroniseren gemaakt. Als de eigenschap de naam van een bestand bevat, wordt een gebruikers-id-bestand gemaakt in de map madata. De eigenschap kan ook een volledig pad bevatten.
\_MMS_IDRegType | Personen kunnen worden geclassificeerd als contactpersonen, ons en internationale gebruikers. De volgende tabel worden de mogelijke waarden: <li>0 - contact</li><li>1 - gebruiker Verenigde Staten</li><li>2 - internationale gebruiker</li>
\_MMS_IDStoreType | Vereiste eigenschap voor Amerikaanse en internationale gebruikers. De eigenschap bevat een integer-waarde die aangeeft of de id van de gebruiker wordt opgeslagen als een bijlage in het Notes-adresboek of in een e-mailbestand van de persoon. Als de gebruikers-ID een bijlage in het adresboek is, het eventueel kan worden gemaakt als een bestand met \_MMS_IDPath. <li>Lege - winkel-id-bestand in een kluis-ID, geen id-bestand (gebruikt voor contactpersonen).</li><li> 1 - bijlage in het adresboek van notities. De \_MMS_Password-eigenschap moet worden ingesteld voor de gebruiker-id-bestanden die bijlagen</li><li>2 - ID in van de persoon e-bestand opslaan. De \_MMS_UseAdminP moet worden ingesteld op false zodat het e-bestand worden gemaakt tijdens de registratie van de persoon. De \_MMS_Password-eigenschap moet worden ingesteld voor de identificatie van gebruikersbestanden.</li>
\_MMS_MailQuotaSizeLimit | Het aantal megabytes die zijn toegestaan voor de e-bestand-database.
\_MMS_MailQuotaWarningThreshold | Het aantal megabytes dat is toegestaan voor het bestand e-database voordat een waarschuwing.
\_MMS_MailTemplateName | Het e-sjabloonbestand dat wordt gebruikt voor het maken van de gebruiker e-bestand. Als een sjabloon is opgegeven, wordt het e-bestand gemaakt met behulp van de opgegeven sjabloon. Als u geen sjabloon is opgegeven, wordt het standaardsjabloonbestand gebruikt om het bestand te maken.
\_MMS_OU | Optionele eigenschap met de naam van de organisatie-eenheid onder de certifier. Deze eigenschap moet leeg zijn voor contactpersonen.
\_MMS_Password | Vereiste eigenschap voor gebruikers. De eigenschap bevat het wachtwoord voor het identificatiebestand van het object.
\_MMS_UseAdminP | De eigenschap moet worden ingesteld op true als de e-bestand moet worden gemaakt door het AdminP-proces op de Domino-server (asynchrone om het exportproces). Als de eigenschap is ingesteld op false, is mail-bestand gemaakt met de gebruiker Domino (synchrone tijdens het exportproces).

Voor een gebruiker met een bijbehorende id-bestand, de \_MMS_Password-eigenschap moet een waarde bevatten. Voor toegang tot e-mail via de Lotus Notes-client, moeten de MailServer en MailFile eigenschappen van een gebruiker een waarde bevatten.

Voor toegang tot e-mail via een webbrowser, moeten de volgende eigenschappen waarden bevatten:

- MailFile - vereiste eigenschap met het pad op de Lotus Domino-server waarop de e-mail-bestand is opgeslagen.
- MailServer - eigenschap die de naam van de Lotus Domino-server bevat vereist. Deze waarde is de naam die moet worden gebruikt wanneer u het e-mailbestand Lotus op de Domino-server gemaakt.
- HTTPPassword - optionele eigenschap die het Web access-wachtwoord voor het object bevat.

De eigenschap HTTPPassword voor toegang tot de Domino-Server zonder e-capaciteit, moet een waarde bevatten. De eigenschap MailFile en de eigenschap MailServer kunnen niet leeg zijn.

Met \_MMS_ IDStoreType = 2 (winkel-id in de Mail-bestand), de eigenschap MailSystem van NotesRegistrationclass is ingesteld op REG_MAILSYSTEM_INOTES (3).

### <a name="mandatory-attributes"></a>Verplichte kenmerken
De connector voor Lotus Domino ondersteunt hoofdzakelijk de volgende typen objecten (documenttypen):

- Groep
- E-mailberichten In Database
- Persoon
- Contact (persoon met geen certifier)
- Resource

In deze sectie worden de kenmerken die verplicht voor elk ondersteund object zijn exporteren naar een Domino-server.

Objecttype | Verplichte kenmerken
--- | ---
Groep | <li>ListName</li>
Main In Database | <li>Volledige naam</li><li>MailFile</li><li>MailServer</li><li>MailDomain</li>
Persoon | <li>Achternaam</li><li>MailFile</li><li>Korte naam</li><li>\_MMS_Password</li><li>\_MMS_IDStoreType</li><li>\_MMS_Certifier</li><li>\_MMS_IDRegType</li><li>\_MMS_UseAdminP</li>
Contact (persoon met geen certifier) | <li>\_MMS_IDRegType</li>
Resource | <li>Volledige naam</li><li>Brontype</li><li>ConfDB</li><li>Resourcecapaciteit</li><li>Site</li><li>DisplayName</li><li>MailFile</li><li>MailServer</li><li>MailDomain</li>

## <a name="common-issues-and-questions"></a>Voorkomende problemen en vragen

### <a name="schema-detection-does-not-work"></a>Schema-detectie werkt niet
Als u het schema te detecteren, is het noodzakelijk dat het bestand schema.nsf aanwezig op de Domino-server is. Dit bestand wordt alleen weergegeven als LDAP is geïnstalleerd op de server. Als het schema niet aantoonbaar is, controleert u of:

- Het bestand schema.nsf is aanwezig op de hoofdmap van de Domino-Server
- De gebruiker heeft machtigingen voor het bestand schema.nsf.
- Ervoor zorgen dat de LDAP-server opnieuw op te starten. **Lotus Domino-Console** openen en het schema opnieuw laden met opdracht **LDAP ReloadSchema vertellen** .

### <a name="not-all-secondary-address-books-are-visible"></a>Niet alle secundaire adresboeken worden weergegeven
De Connector Domino is afhankelijk van de functie **Geholpen** om te kunnen zoeken naar de secundaire adresboeken. Als de secundaire adresboeken ontbreken, controleert u of als [Geholpen](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_ABOUT_DIRECTORY_ASSISTANCE.html) heeft ingeschakeld en geconfigureerd op de Domino Server.

### <a name="custom-attributes-in-domino"></a>Aangepaste kenmerken in Domino
Er zijn verschillende manieren in Domino aan het schema uitbreiden, zodat deze wordt weergegeven als een aangepast attribuut verbruikbare door de Connector.

**Methode 1: Lotus Domino schema uitbreiden**

1. Maak een kopie van de sjabloon voor Domino Directory {PUBNAMES. NTF} door het volgende (u moet niet de standaardmap voor IBM Lotus Domino sjabloon aanpassen) [als volgt](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_CREATING_A_COPY_OF_THE_DEFAULT_PUBIC_ADDRESS_BOOK_TEMPLATE.html) :
2. Open de kopie van Domino directory sjabloon {CONTOSO. Sjabloon NTF} dat is gemaakt in Domino Designer en de volgende stappen:
    - Klik op gedeelde elementen en subformulieren uitvouwen
    - Dubbelklik op ${objectnaam} InheritableSchema subformulier (waarbij {Objectnaam} de naam van de standaardklasse structurele object bijvoorbeeld is: persoon).
    - Naam van het kenmerk dat u wilt toevoegen in het schema {MyPersonAtrribute} en het corresponderende aan dat kenmerk. Een veld maken door te selecteren het Menu **maken** en selecteert u vervolgens in het menu van **veld** .
    - In het veld is toegevoegd, moet u de eigenschappen ervan instellen door het selecteren van het Type, stijl, grootte, lettertype en andere gerelateerde parameters op het veld van het venster Eigenschappen.
    - De standaardwaarde is dezelfde als de naam die is opgegeven voor dat kenmerk kenmerk behouden (bijvoorbeeld als de naam van kenmerk MyPersonAttribute, houden de standaardwaarde met dezelfde naam).
    - Sla het subformulier ${objectnaam} InheritableSchema met bijgewerkte waarden.
3. Vervang de Domino Directory sjabloon {PUBNAMES. NTF} met de nieuwe aangepaste sjabloon {CONTOSO. NTF} door het volgende [te werk](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_ABOUT_RULES_FOR_CUSTOMIZING_THE_PUBLIC_ADDRESS_BOOK.html).
4. Domino Admin sluit en open de Console Domino de LDAP-service opnieuw starten en het LDAP-Schema opnieuw laden:
    - Voeg de opdracht onder **Domino** opdrachttekst ingediend om de LDAP-service - [Taak LDAP opnieuw]( http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_STARTING_AND_STOPPING_THE_LDAP_SERVER_OVER.html)starten in Domino-Console.
    - Om opnieuw te laden LDAP schema gebruikt LDAP vertellen opdracht - LDAP ReloadSchema vertellen
5. Open Domino Admin en selecteer tabblad van mensen en groepen zien toegevoegde kenmerk komt tot uiting in domino persoon toevoegen.
6. Schema.nsf op het tabblad **bestanden** opent en toegevoegde kenmerk wordt weerspiegeld in de LDAP-objectklasse dominoPerson.

**Methode 2: Een auxClass met aangepast kenmerk te maken en koppelen aan de objectklasse**

1. Maak een kopie van de sjabloon voor Domino Directory {PUBNAMES. NTF} door het volgende [te werk](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_CREATING_A_COPY_OF_THE_DEFAULT_PUBIC_ADDRESS_BOOK_TEMPLATE.html) (nooit de standaardmap voor IBM Lotus Domino sjabloon aanpassen):
2. Open de kopie van Domino directory sjabloon {CONTOSO. Sjabloon NTF} dat is gemaakt in Domino Designer.
3. Selecteer in het linkerdeelvenster gedeelde Code en subformulieren.
4. Klik op nieuw subformulier
5. Ga als volgt te werk om de eigenschappen voor het nieuwe subformulier opgeven:
    - Met het nieuwe subformulier openen, ontwerp - eigenschappen van het subformulier te kiezen
    - Naast de eigenschap naam een naam invoeren voor de aanvullende objectklasse bijvoorbeeld TestSubform.
    - Houd de eigenschap Options 'Opnemen in het subformulier invoegen... dialoogvenster' geselecteerd
    - Hef de selectie van de opties voor eigenschap 'Render pass through HTML in notities."
    - Laat de andere eigenschappen die hetzelfde en sluit het dialoogvenster subformulier.
    - Opslaan en sluiten van het nieuwe subformulier.
6. Doe het volgende als u wilt toevoegen om te definiëren van de objectklasse Aux:
    - Open het subformulier dat u hebt gemaakt.
    - Kies maken - veld.
    - Naast de naam op het tabblad basis van het dialoogvenster veld, Geef een naam, bijvoorbeeld: {MyPersonTestAttribute}.
    - In het veld is toegevoegd, moet u de eigenschappen ervan instellen door het selecteren van het Type, stijl, grootte, lettertype en verwante eigenschappen.
    - De standaardwaarde is dezelfde als de naam die is opgegeven voor dat kenmerk kenmerk behouden (bijvoorbeeld als de naam van kenmerk MyPersonTestAttribute, houden de standaardwaarde met dezelfde naam).
    - Sla het subformulier met de bijgewerkte waarden en voer de volgende handelingen uit:
        - Selecteer in het linkerdeelvenster gedeelde Code en subformulieren
        - Selecteer het nieuwe subformulier en ontwerp - Design eigenschappen kiezen.
        - Klik op het derde tabblad vanaf de linkerkant en selecteer **doorgeven dit verbod van ontwerp wijzigen**.
7. ${Objectnaam} ExtensibleSchema subformulier, (waarbij {objectnaam} is de naam van de standaard structurele objectklasse, bijvoorbeeld – persoon) te openen.
8. Resource invoegen en selecteert u het subformulier (die u hebt gemaakt, bijvoorbeeld: TestSubform) en het subformulier ${objectnaam} ExtensibleSchema opslaan.
9. Vervang de Domino Directory sjabloon {PUBNAMES. NTF} met de nieuwe aangepaste sjabloon {CONTOSO. NTF} door het volgende [te werk](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_ABOUT_RULES_FOR_CUSTOMIZING_THE_PUBLIC_ADDRESS_BOOK.html).
10. Domino Admin sluit en open de Console Domino de LDAP-service opnieuw starten en het LDAP-Schema opnieuw laden:
    - Voeg de opdracht onder **Domino** opdrachttekst ingediend om de LDAP-service - [Taak LDAP opnieuw](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_STARTING_AND_STOPPING_THE_LDAP_SERVER_OVER.html)starten in Domino-Console.
    - Opnieuw laden van LDAP-schema gebruikt LDAP vertellen opdracht **LDAP-ReloadSchema zien**.
11. Open Domino Admin en mensen en groepen tabblad Zie toegevoegde kenmerk komt tot uiting in domino persoon toevoegen (onder andere tab).
12. Schema.nsf op het tabblad **bestanden** opent en toegevoegde kenmerk wordt aangegeven bij TestSubform LDAP-object hulpklasse.

**Methode 3: Het aangepaste kenmerk toevoegen aan de klasse ExtensibleObject**

1. Bestand openen {Schema.nsf} op de hoofdmap geplaatst
2. LDAP-objectklassen in het linkermenu onder **Alle documenten van het Schema** en klik op de knop **klasse Object toevoegen** :
3. Bieden in de vorm van {zzzExtensibleSchema} (zzz is waar de naam van de standaard structurele objectklasse, bijvoorbeeld persoon)-naam voor LDAP. Schema uitbreiden voor de persoon die de objectklasse, bieden u bijvoorbeeld {PersonExtensibleSchema}-naam voor LDAP.
4. Klassenaam van het bovenliggende Object waarvoor u het schema uitbreidt wilt bieden. Bijvoorbeeld voor de klasse persoon schema uitbreiden, bieden superieure klasse objectnaam {dominoPerson}:
5. Geef een geldige OID die overeenkomt met de objectklasse.
6. Selecteer uitgebreid/aangepaste kenmerken onder de verplichte of optionele kenmerktypen velden aan de hand van de vereiste:
7. Na de vereiste kenmerken toevoegen aan de ExtensibleObjectClass, klikt u op **Opslaan en sluiten**.
8. Een ExtensibleObjectClass wordt gemaakt voor de respectieve standaard de objectklasse met uitgebreide kenmerken.

## <a name="troubleshooting"></a>Het oplossen van problemen

-   Zie voor meer informatie over het inschakelen van logboekregistratie voor het oplossen van de connector [hoe inschakelen ETW-tracering voor verbindingslijnen](http://go.microsoft.com/fwlink/?LinkId=335731).
