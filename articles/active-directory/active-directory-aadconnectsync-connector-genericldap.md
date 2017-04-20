<properties
   pageTitle="Algemene LDAP-Connector | Microsoft Azure"
   description="In dit artikel wordt beschreven hoe Microsoft algemene LDAP-Connector configureren."
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

# <a name="generic-ldap-connector-technical-reference"></a>Technische naslaginformatie over algemene LDAP-Connector
Dit artikel beschrijft de algemene LDAP-Connector. Het artikel is van toepassing op de volgende producten:

- Microsoft Identity Manager 2016 (MIM2016)
- Forefront Identiteitenbeheer 2010 R2 (FIM2010R2)
    -   Moet hotfix 4.1.3671.0 of later [KB3092178](https://support.microsoft.com/kb/3092178)gebruiken.

De Connector is voor MIM2016 en FIM2010R2, worden gedownload vanaf het [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=717495).

Met betrekking tot de IETF RFC's, dit document met behulp van de indeling (RFC [RFC-nummer] / [sectie in het document RFC]), bijvoorbeeld (RFC 4512/4.3).
U kunt meer informatie vinden op http://tools.ietf.org/html/rfc4500 (u moet 4500 vervangen door de juiste RFC-nummer).

## <a name="overview-of-the-generic-ldap-connector"></a>Overzicht van de algemene LDAP-Connector
De algemene LDAP-Connector kunt u de synchronisatieservice integreren met een LDAP-server v3.

Bepaalde bewerkingen en schema-elementen, zoals die welke nodig zijn voor het uitvoeren van delta-import, zijn niet opgegeven in de IETF RFC's. Voor deze bewerkingen, worden alleen expliciet opgegeven LDAP-adreslijsten ondersteund.

Vanuit een perspectief op hoog niveau, worden de volgende functies worden ondersteund door de huidige versie van de verbindingslijn:

Functie | Ondersteuning
--- | --- |
Verbonden gegevensbron | De verbindingslijn wordt ondersteund door alle LDAP v3 servers (compatibel met RFC 4510). Het is getest met de volgende: <li>Microsoft Active Directory Lightweight directoryservices (AD LDS)</li><li>Globale catalogus van Microsoft Active Directory (AD GC)</li><li>389 directoryserver</li><li>Active Directory-Server Apache</li><li>IBM Tivoli DS</li><li>Map Isode</li><li>NetIQ eDirectory</li><li>Novell eDirectory</li><li>DJ openen</li><li>Open Active Directory</li><li>Open LDAP (openldap.org)</li><li>Oracle (voorheen Sun) Directory Server Enterprise Edition</li><li>RadiantOne virtuele directoryserver (VDS)</li><li>Sun één Active Directory-Server</li>**Belangrijke mappen worden niet ondersteund:** <li>Microsoft Active Directory Domain Services (AD DS) [gebruik in plaats daarvan de ingebouwde Active Directory Connector]</li><li>Oracle Internet Directory (OID)</li>
Scenario 's   | <li>Levenscyclusbeheer voor uw object</li><li>Groepsbeheer</li><li>Wachtwoordbeheer</li>
Bewerkingen |De volgende bewerkingen worden op alle LDAP-directory's ondersteund: <li>Volledige importbewerking</li><li>Exporteren</li>De volgende bewerkingen worden alleen ondersteund op de opgegeven mappen:<li>Delta-import</li><li>Wachtwoord, wachtwoord wijzigen</li>
Schema | <li>Schema is gedetecteerd door de LDAP-schema (RFC3673 en RFC4512/4.2)</li><li>Ondersteunt structurele klassen en klassen aux extensibleObject klasse (RFC4512/4.3)</li>

### <a name="delta-import-and-password-management-support"></a>Delta-ondersteuning voor invoer- en
Mappen voor Delta importeren en wachtwoordbeheer ondersteund:

- Microsoft Active Directory Lightweight directoryservices (AD LDS)
    - Ondersteunt alle bewerkingen voor het importeren van delta
    - Ondersteunt wachtwoord instellen
- Globale catalogus van Microsoft Active Directory (AD GC)
    - Ondersteunt alle bewerkingen voor het importeren van delta
    - Ondersteunt wachtwoord instellen
- 389 directoryserver
    - Ondersteunt alle bewerkingen voor het importeren van delta
    - Ondersteunt ingesteld wachtwoord en wachtwoord wijzigen
- Active Directory-Server Apache
    - Biedt geen ondersteuning voor delta importeren omdat deze map beschikt niet over een permanente wijzigingslogbestand
    - Ondersteunt wachtwoord instellen
- IBM Tivoli DS
    - Ondersteunt alle bewerkingen voor het importeren van delta
    - Ondersteunt ingesteld wachtwoord en wachtwoord wijzigen
- Map Isode
    - Ondersteunt alle bewerkingen voor het importeren van delta
    - Ondersteunt ingesteld wachtwoord en wachtwoord wijzigen
- Novell eDirectory en NetIQ eDirectory
    - Bewerkingen toevoegen, bijwerken en wijzigen voor het importeren van delta ondersteunt
    - Biedt geen ondersteuning voor verwijderbewerkingen voor delta-import
    - Ondersteunt ingesteld wachtwoord en wachtwoord wijzigen
- DJ openen
    - Ondersteunt alle bewerkingen voor het importeren van delta
    - Ondersteunt ingesteld wachtwoord en wachtwoord wijzigen
- Open Active Directory
    - Ondersteunt alle bewerkingen voor het importeren van delta
    - Ondersteunt ingesteld wachtwoord en wachtwoord wijzigen
- Open LDAP (openldap.org)
    - Ondersteunt alle bewerkingen voor het importeren van delta
    - Ondersteunt wachtwoord instellen
    - Biedt geen ondersteuning voor wachtwoord wijzigen
- Oracle (voorheen Sun) Directory Server Enterprise Edition
    - Ondersteunt alle bewerkingen voor het importeren van delta
    - Ondersteunt ingesteld wachtwoord en wachtwoord wijzigen
- RadiantOne virtuele directoryserver (VDS)
    - Versie 7.1.1 of hoger
    - Ondersteunt alle bewerkingen voor het importeren van delta
    - Ondersteunt ingesteld wachtwoord en wachtwoord wijzigen
-  Sun één Active Directory-Server
    - Ondersteunt alle bewerkingen voor het importeren van delta
    - Ondersteunt ingesteld wachtwoord en wachtwoord wijzigen

### <a name="prerequisites"></a>Vereisten
Voordat u de verbindingslijn gebruiken, moet u het volgende op de server voor de synchronisatie:

- 4.5.2 van Microsoft .NET Framework of later

### <a name="detecting-the-ldap-server"></a>Detecteren van de LDAP-server
De Connector maakt gebruik van verschillende technieken voor het opsporen en identificeren van de LDAP-server. De Connector de DSE Root, leverancier naam/versie gebruikt en zij keurt het schema om unieke objecten en kenmerken bekend in bepaalde LDAP-servers te vinden. Deze gegevens, indien gevonden, kunt u vooraf de configuratieopties in de Connector in te vullen.

### <a name="connected-data-source-permissions"></a>Verbonden gegevensbron machtigingen
Als u wilt importeren en exporteren van bewerkingen op de objecten in de directory verbonden, moet de connector-account voldoende machtigingen hebben. De connector moet schrijfmachtigingen voor het exporteren en importeren kunnen lezen. Configuratie van de machtiging wordt uitgevoerd binnen de ervaringen van het management van de doelmap zelf.

### <a name="ports-and-protocols"></a>Poorten en protocollen
De verbindingslijn wordt het poortnummer dat is opgegeven in de configuratie, die standaard 389 voor LDAP en 636 voor LDAPS is.

Voor LDAPS, moet u SSL 3.0 of TLS gebruiken. SSL 2.0 wordt niet ondersteund en kan niet worden geactiveerd.

### <a name="required-controls-and-features"></a>Vereiste besturingselementen en functies
De volgende besturingselementen LDAP/functies moet beschikbaar zijn op de LDAP-server voor de connector goed te laten werken:  
`1.3.6.1.4.1.4203.1.5.3`Waar/onwaar-filters

Het filter waar/onwaar wordt vaak niet gemeld worden ondersteund door de LDAP-directory's en kan worden weergegeven op de **Pagina Algemene** onder **Verplichte functies niet gevonden**. Het wordt gebruikt **of** om filters te maken in de LDAP-query's, bijvoorbeeld bij het importeren van meerdere objecttypen. Als u meer dan één type van het object importeren kunt, ondersteunt deze functie met de LDAP-server.

Als u een map waarin een unieke id is het anker moet de volgende ook beschikbaar (Zie de sectie [Ankers configureren](#configure-anchors) verderop in dit artikel voor meer informatie):  
`1.3.6.1.4.1.4203.1.5.1`Alle operationele kenmerken

Als de map meer heeft dan wat in één aanroep van de directory past-objecten, wordt aanbevolen gebruik van wisselbestand. Voor wisselbestanden wilt werken, moet u een van de volgende opties:

**Optie 1:**  
`1.2.840.113556.1.4.319`pagedResultsControl

**Optie 2:**  
`2.16.840.1.113730.3.4.9`VLVControl  
`1.2.840.113556.1.4.473`SortControl

Als beide opties zijn ingeschakeld in de configuratie van de verbindingslijn, wordt pagedResultsControl gebruikt.

`1.2.840.113556.1.4.417`ShowDeletedControl

ShowDeletedControl wordt alleen gebruikt met de importeermethode van USNChanged delta kunnen verwijderde objecten zien.

De verbindingslijn wordt gezocht naar de opties die op de server aanwezig zijn. Als de opties kunnen niet worden gedetecteerd, wordt een waarschuwing in de Connectoreigenschappen op de algemene pagina aanwezig is. Niet alle LDAP-servers aanwezig zijn alle besturingselementen/functies die ze ondersteunen en zelfs als deze waarschuwing aanwezig is, de connector kan werken zonder problemen.

### <a name="delta-import"></a>Delta-import
Delta importeren is alleen beschikbaar wanneer u een map support is aangetroffen. Momenteel worden de volgende methoden gebruikt:

- Accesslog LDAP. Zie [Logboekregistratie van http://www.openldap.org/doc/admin24/overlays.html#Access](http://www.openldap.org/doc/admin24/overlays.html#Access Logging)
- LDAP-Changelog. Zie [http://tools.ietf.org/html/draft-good-ldap-changelog-04](http://tools.ietf.org/html/draft-good-ldap-changelog-04)
- Tijdstempel. De Connector gebruikt voor Novell/NetIQ eDirectory, laatste datum en tijd ophalen gemaakt en objecten bijgewerkt. Novell/NetIQ eDirectory beschikt niet over dat een gelijkwaardige betekent dat verwijderde objecten ophalen. Deze optie kan ook worden gebruikt als er geen andere delta importeermethode actief op de LDAP-server is. Deze optie kan geen objecten importeren verwijderd.
- USNChanged. Zie: [https://msdn.microsoft.com/library/ms677627.aspx](https://msdn.microsoft.com/library/ms677627.aspx)

### <a name="not-supported"></a>Niet ondersteund
De volgende LDAP-functies worden niet ondersteund:

- LDAP-verwijzingen tussen servers (RFC 4511/4.1.10)

## <a name="create-a-new-connector"></a>Een nieuwe verbindingslijn maken
Voor het maken van een algemene LDAP-connector in de **Tijdsynchronisatie-Service** selecteert **Management Agent** en **maken**. Selecteer de verbindingslijn **algemene LDAP (Microsoft)** .

![CreateConnector](./media/active-directory-aadconnectsync-connector-genericldap/createconnector.png)

### <a name="connectivity"></a>Connectiviteit
Op de pagina verbindingen, moet u de Host, poort en bindende informatie. Afhankelijk van de Binding ingeschakeld, worden extra is mogelijk inlichtingen worden verstrekt in de volgende secties.

![Connectiviteit](./media/active-directory-aadconnectsync-connector-genericldap/connectivity.png)

- De instelling time-out van verbinding wordt alleen gebruikt voor de eerste verbinding met de server bij het opsporen van het schema.
- Als de Binding is anoniem, vervolgens geen gebruikersnaam / wachtwoord of certificaat worden gebruikt.
- Andere bindingen, informatie opgeven voor beide in gebruikersnaam / wachtwoord of Selecteer een certificaat.
- Als u verificatie via Kerberos, ook geeft u de Realm of het domein van de gebruiker.

In het tekstvak **kenmerk aliassen** wordt gebruikt voor kenmerken in het schema met de syntaxis RFC4522 gedefinieerd. Deze kenmerken kunnen niet worden gedetecteerd tijdens de detectie van schema en de verbindingslijn moet worden geïdentificeerd die kenmerken. Het volgende is bijvoorbeeld nodig om te worden ingevoerd in het vak kenmerk aliassen te herkennen het kenmerk userCertificate als een binaire kenmerk:

`userCertificate;binary`

Hier volgt een voorbeeld van hoe deze configuratie kan eruit:

![Connectiviteit](./media/active-directory-aadconnectsync-connector-genericldap/connectivityattributes.png)

Schakel het selectievakje **operationele kenmerken in het schema bevatten** ook kenmerken die worden gemaakt door de server opnemen. Deze bevatten kenmerken, zoals wanneer het object is gemaakt en de datum van laatste update.

Selecteer **inclusief uitbreidbare kenmerken in het schema** als extensible objecten (RFC4512/4.3) worden gebruikt en u deze optie inschakelt kan elk kenmerk moet worden gebruikt op alle objecten. Met deze optie maakt het schema zeer groot zodat tenzij de gekoppelde map met behulp van deze functie de aanbeveling dat de optie uitgeschakeld is.

### <a name="global-parameters"></a>Algemene Parameters
Op de pagina Algemene Parameters configureert u de DN-naam voor het wijzigingslogbestand delta en extra functies voor LDAP. De pagina is vooraf ingevuld met de informatie die door de LDAP-server.

![Connectiviteit](./media/active-directory-aadconnectsync-connector-genericldap/globalparameters.png)

De bovenste sectie bevat informatie die door de server zelf, zoals de naam van de server. De Connector bevestigt ook dat de verplichte controles aanwezig in de hoofdmap DSE zijn. Als u deze besturingselementen niet worden vermeld, wordt een waarschuwing weergegeven. Bepaalde LDAP-directory's niet alle functies in de hoofdmap DSE aanbieden en het is mogelijk dat de Connector zonder problemen werkt ook als een waarschuwing aanwezig is.

De selectievakjes **besturingselementen ondersteund** bepalen het gedrag van bepaalde bewerkingen:

- Met structuur verwijderen hebt geselecteerd, een hiërarchie wordt verwijderd met een LDAP-aanroep. Met boomstructuur verwijderen is uitgeschakeld, wordt de verbindingslijn een recursief verwijderen als dat nodig is.
- Met wisselbare resultaten is geselecteerd, wordt de verbindingslijn een wisselbare importeren met de grootte die is opgegeven op de stappen uitvoeren.
- De VLVControl en de SortControl is een alternatief voor de pagedResultsControl om gegevens te lezen van de LDAP-directory.
- Als alle drie de opties (pagedResultsControl, VLVControl en SortControl) niet-geselecteerde zijn importeert alle objecten in één bewerking kan mislukken als een grote map is met de Connector.
- ShowDeletedControl wordt alleen gebruikt wanneer de methode Delta-import USNChanged is.

Het wijzigingslogbestand DN-naam is de naamgevingscontext die bijvoorbeeld wordt gebruikt door het wijzigingslogbestand delta **cn = changelog**. Deze waarde moet worden opgegeven kunnen doen delta importeren.

Hieronder volgt een lijst met standaard wijzigingslogbestand DNs:

Directory | Het wijzigingenlogboek delta
--- | ---
Microsoft AD LDS en AD GC | Automatisch gedetecteerd. USNChanged.
Active Directory-Server Apache | Niet beschikbaar.
389 Directory | Logboek met wijzigingen. Standaardwaarde gebruiken: **cn = changelog**
IBM Tivoli DS | Logboek met wijzigingen. Standaardwaarde gebruiken: **cn = changelog**
Map Isode | Logboek met wijzigingen. Standaardwaarde gebruiken: **cn = changelog**
Novell/NetIQ eDirectory | Niet beschikbaar. Tijdstempel. Laatst bijgewerkt datum/tijd om de verbindingslijn wordt toegevoegd en de records.
Open DJ Active Directory | Logboek met wijzigingen.  Standaardwaarde gebruiken: **cn = changelog**
Open LDAP | Toegang logboek. Standaardwaarde gebruiken: **cn = accesslog**
Oracle DSEE | Logboek met wijzigingen. Standaardwaarde gebruiken: **cn = changelog**
VDS-RadiantOne | Virtuele map. Afhankelijk van de map die is verbonden met VDS.
Sun één Active Directory-Server | Logboek met wijzigingen. Standaardwaarde gebruiken: **cn = changelog**

Het kenmerk wachtwoord is de naam van het kenmerk dat de Connector gebruiken moet voor het instellen van het wachtwoord in wachtwoord wijzigen en bewerkingen voor het wachtwoord instellen.
Deze waarde is standaard ingesteld op **userPassword** maar kan worden gewijzigd wanneer dit nodig is voor een bepaalde LDAP-systeem.

In de lijst extra partities is het mogelijk om toe te voegen aanvullende naamruimten niet automatisch wordt gedetecteerd. Deze instelling kan bijvoorbeeld worden gebruikt als er meerdere servers vormen een logische cluster, moet alle tegelijk worden geïmporteerd. Als Active Directory meerdere domeinen in het ene forest hebben kan, maar alle domeinen één schema delen, kunnen dezelfde worden gesimuleerd met aanvullende naamruimten voeren in dit vak. Elke naamruimte kan importeren uit verschillende servers en verder op de pagina partities configureren en hiërarchieën is geconfigureerd. Gebruik Ctrl + Enter om een nieuwe regel.

### <a name="configure-provisioning-hierarchy"></a>Provisioning hiërarchie configureren
Deze pagina is onderdeel van de DN-naam, bijvoorbeeld OU toewijzen aan het objecttype dat moet worden ingericht, bijvoorbeeld organizationalUnit gebruikt.

![Inrichten van hiërarchie](./media/active-directory-aadconnectsync-connector-genericldap/provisioninghierarchy.png)

Provisioning hiërarchie configureert, kunt u de verbindingslijn automatisch een om structuur te maken als dat nodig is. Bijvoorbeeld als er een naamruimte dc = contoso, dc com en een nieuw object cn = Jan, ou = = Rotterdam, c = US, dc = contoso, dc = com wordt ingericht, wordt de verbindingslijn een object van het type land voor de Verenigde Staten en een organizationalUnit voor Seattle kunt maken als die nog niet aanwezig in de directory.

### <a name="configure-partitions-and-hierarchies"></a>Configureren, partities en hiërarchieën
Selecteer op de pagina partities en hiërarchieën alle naamruimten met objecten die u wilt importeren en exporteren.

![Partities](./media/active-directory-aadconnectsync-connector-genericldap/partitions.png)

Elke naamruimte is het ook mogelijk voor het configureren van instellingen voor verbindingen die de waarden die zijn opgegeven op het scherm connectiviteit wilt overschrijven. Als deze waarden worden gelaten op hun lege standaardwaarde, wordt de informatie in het scherm verbindingen gebruikt.

Het is ook mogelijk om te selecteren welke containers en de verbindingslijn moet importeren uit en naar exporteren organisatie-eenheden.

### <a name="configure-anchors"></a>Ankers configureren
Deze pagina hebben altijd een vooraf geconfigureerde waarde en kan niet worden gewijzigd. De leverancier is geconstateerd, kan het anker worden gevuld met een onveranderbare kenmerk, bijvoorbeeld de GUID van een object. Als deze niet is gevonden of niet een onveranderbare kenmerk heeft, wordt de verbindingslijn dn (distinguished name) als het anker.

![ankers](./media/active-directory-aadconnectsync-connector-genericldap/anchors.png)

Hieronder volgt een lijst met LDAP-servers en het anker wordt gebruikt:

Directory | , Ankerkenmerk
--- | ---
Microsoft AD LDS en AD GC | objectGUID
389 directoryserver | DN-naam
Apache-Directory | DN-naam
IBM Tivoli DS | DN-naam
Map Isode | DN-naam
Novell/NetIQ eDirectory | GUID
Open DJ Active Directory | DN-naam
Open LDAP | DN-naam
Oracle ODSEE | DN-naam
VDS-RadiantOne | DN-naam
Sun één Active Directory-Server | DN-naam

## <a name="other-notes"></a>Andere opmerkingen
Deze sectie bevat informatie van aspecten die specifiek zijn voor deze Connector of om andere redenen zijn belangrijk om te weten.

### <a name="delta-import"></a>Delta-import
Het watermerk delta in Open LDAP is UTC-datum/tijd. Daarom moeten de klokken tussen FIM tijdsynchronisatie-Service en de Open LDAP worden gesynchroniseerd. Als dat niet het geval is, kunnen sommige gegevens in het logboekbestand delta worden weggelaten.

De delta-import voor Novell eDirectory, wordt niet een verwijderd object gedetecteerd. Daarom is het nodig om een volledige importbewerking regelmatig om te zoeken naar alle verwijderde objecten uitvoeren.

Voor mappen met een wijzigingslogbestand delta op basis van datum/tijd, verdient een volledige importbewerking uitvoeren op periodieke tijden. Dit proces kan de synchronisatie-engine te vinden en bedrijfssegment tussen de LDAP-server en wat is de connector ruimte.

## <a name="troubleshooting"></a>Het oplossen van problemen

-   Zie voor meer informatie over het inschakelen van logboekregistratie voor het oplossen van de connector [hoe inschakelen ETW-tracering voor verbindingslijnen](http://go.microsoft.com/fwlink/?LinkId=335731).
