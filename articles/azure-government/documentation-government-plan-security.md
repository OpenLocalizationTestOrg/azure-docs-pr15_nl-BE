<properties
    pageTitle="Azure overheidsdiensten | Microsoft Azure"
    description="Biedt en overzicht van de beschikbare services in Azure overheid"
    services="Azure-Government"
    cloud="gov"
    documentationCenter=""
    authors="zakramer"
    manager="liki"
    editor="" />

<tags
    ms.service="multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="azure-government"
    ms.date="10/18/2016"
    ms.author="ryansoc" />


#  <a name="security"></a>Beveiliging

##  <a name="principles-for-securing-customer-data-in-azure-government"></a>Beginselen voor het beveiligen van gegevens van de klant in Azure overheid

Azure overheid biedt een scala aan functies en services die u gebruiken kunt om cloud oplossingen aan uw wensen geregeld/gecontroleerde gegevens te maken. Een compatibele klantoplossing is niets meer dan de daadwerkelijke uitvoering van out-of-the-box Azure overheid mogelijkheden, in combinatie met vaste gegevens uit veiligheidsoverwegingen.

Wanneer u een oplossing in Azure overheid host, verwerkt Microsoft veel van deze voorschriften op het niveau van de cloud-infrastructuur.

Het volgende diagram toont de Azure ingrijpende model. Bijvoorbeeld, biedt Microsoft basic cloud infrastructuur DDOS, samen met de klant mogelijkheden zoals beveiligingsapparaten voor klant-specifieke toepassing die DDOS nodig heeft.

![ALT-tekst](./media/azure-government-Defenseindepth.png)

Deze pagina geeft een overzicht van het basissysteem beginselen voor het beveiligen van uw Services en toepassingen en biedt richtlijnen en aanbevolen procedures voor het toepassen van deze beginselen; met andere woorden, hoe klanten moeten maken slim gebruik van Azure regering om te voldoen aan de verplichtingen en verantwoordelijkheden die vereist zijn voor een oplossing die ITAR informatie verwerkt.

 De overkoepelende beginselen voor het beveiligen van gegevens van de klant zijn:

- Bescherming van gegevens met codering
- Geheimen beheren
- Isolatie voor het beperken van toegang tot gegevens

###  <a name="protecting-customer-data-using-encryption"></a>Bescherming van gegevens met codering

Risico beperken en voldoen aan wettelijke verplichtingen zijn het toenemende belang van gegevenscodering en focus te rijden. Een implementatie van effectieve codering gebruiken voor het verbeteren van huidige netwerk- en veiligheidsmaatregelen, en verlagen het algehele risico van uw cloud-omgeving.

#### <a name="encryption-at-rest"></a>Codering in rust
De codering van gegevens in rust is van toepassing op de bescherming van de klant inhoud schijf opgeslagen. Er zijn verschillende manieren waarop die dit kan gebeuren:

#### <a name="storage-service-encryption"></a>Opslag Service codering

Azure Storage Service-codering is ingeschakeld op het niveau van de account opslag blok BLOB's en pagina-BLOB's worden automatisch gecodeerd als geschreven naar Azure opslag. Als u de gegevens uit de opslag van Azure leest, wordt gedecodeerd door de storage-service voordat het wordt geretourneerd. Gebruik deze optie als u uw gegevens te beveiligen zonder te wijzigen of toevoegen van code aan toepassingen.

#### <a name="client-side-encryption"></a>Client-Side-codering
Client-Side-codering is ingebouwd in de Java en .NET opslag clientbibliotheken, die van Azure sleutel kluis API's gebruikmaken kunnen, waardoor dit eenvoudig te implementeren. Azure sleutel kluis gebruiken om toegang te krijgen tot de geheimen in Azure sleutel kluis voor bepaalde personen met Azure Active Directory.

#### <a name="encryption-in-transit"></a>Codering in transit

De basic codering beschikbaar voor connectiviteit met Azure overheid ondersteunt protocol beveiliging TLS (Transport Level) 1.2 en x.509-certificaten. Federal Information Processing Standard (FIPS) 140-2 niveau 1 cryptografische algoritmen ook voor infrastructuur netwerkverbindingen tussen datacenters Azure regering gebruikt worden.  Windows Server 2012 R2 en Windows 8-plus VMs en bestandsshares Azure kunt SMB 3.0 gebruiken voor codering tussen de VM en de bestandsshare. Client-Side-codering gebruikt om de gegevens te coderen voordat het op te slaan in een clienttoepassing overbrengen, en om de gegevens na het decoderen geen opslagruimte wordt overgebracht.

#### <a name="best-practices-for-encryption"></a>Aanbevolen procedures voor codering

- IaaS VMs: Azure schijf codering gebruiken. Opslag Service versleuteling voor het coderen van de VHD-bestanden die worden gebruikt voor back-up van deze schijven in Azure opslag inschakelen, maar dit zijn onlangs schriftelijke gegevens alleen gecodeerd. Dit betekent dat als u een VM maken en vervolgens Storage Service-codering inschakelen voor de opslag rekening met het VHD-bestand, alleen de wijzigingen worden gecodeerd, niet het oorspronkelijke VHD-bestand.
- Client-Side codering: Dit is de veiligste methode voor het coderen van uw gegevens, omdat het codeert het voor communautair douanevervoer en de gegevens in rust codeert. Echter, is het vereist dat u code toevoegt aan uw toepassingen met behulp van opslag, waarmee u mogelijk niet wilt doen. In dat geval kunt u HTTPs voor uw gegevens in doorvoer en opslag Service codering voor het coderen van de gegevens in rust. Client-Side codering ook betrekking heeft op meer belasting op de client, hebt u voor deze account in uw plannen voor schaalbaarheid, vooral als u zijn gecodeerd en een grote hoeveelheid gegevens.

###  <a name="protecting-customer-data-by-managing-secrets"></a>Klantgegevens beschermen door het beheer van geheimen

Veilig beheer van sleutels is van wezenlijk belang voor de bescherming van gegevens in de cloud. Klanten moeten streven naar de belangrijkste beheer vereenvoudigen en het beheer van sleutels die door cloud-toepassingen en services worden gebruikt om gegevens te coderen.

#### <a name="best-practices-for-managing-secrets"></a>Aanbevolen procedures voor het beheren van geheimen

- Met sleutel kluis kunt u het risico van geheimen worden blootgesteld via vastgelegde configuratiebestanden, scripts, of in de broncode. Azure sleutel kluis toetsen (zoals de coderingssleutels voor Azure schijfversleuteling) en geheimen (zoals wachtwoorden) worden gecodeerd, doordat ze in FIPS 140-2 niveau 2 gevalideerd hardware security modules (HSM's). U kunt extra garantie, importeren of het genereren van sleutels in deze HSM's.
- Toepassing en sjablonen moeten alleen URI verwijzingen bevatten naar de geheimen (wat betekent dat de werkelijke geheimen zijn niet in de code, de configuratie of de broncode opslagplaatsen). Hiermee voorkomt u dat sleutels phishing-aanvallen op interne of externe repo's, zoals de oogst-bots in de GitHub.
- Gebruik van sterke RBAC besturingselementen binnen sleutel kluis. Als een vertrouwde operator het bedrijf of de overdracht aan een nieuwe groep binnen het bedrijf verlaat, moeten ze niet mogen toegang te krijgen tot de geheimen.

Voor meer informatie <a href="https://azure.microsoft.com/documentation/services/key-vault">openbare documentatie Azure sleutel kluis.</a>

###  <a name="isolation-to-restrict-data-access"></a>Isolatie voor het beperken van toegang tot gegevens

Isolatie is alles over gegevenstoegang beperken tot alleen geautoriseerde gebruikers, services en toepassingen met behulp van de grenzen, segmentatie en containers. De scheiding tussen de huurders is bijvoorbeeld een essentieel beveiligingsmechanisme voor multitenant wolk platforms zoals Microsoft Azure. Logische isolatie helpt voorkomen dat een huurder verstoren de werking van een andere huurder.

#### <a name="environment-isolation"></a>Isolatie van de omgeving
De regering van Azure-omgeving is een fysieke instantie die losstaat van de rest van het netwerk van Microsoft. Dit wordt bereikt door een reeks fysieke en logische controles die het volgende bevatten:

- Het beveiligen van fysieke barrières met biometrische apparaten en camera's.
- Het gebruik van specifieke referenties en multifactor-verificatie door personeel van Microsoft die logische verbinding met de productie-omgeving.
- Alle-infrastructuur voor Azure regering bevindt zich in de Verenigde Staten.

#### <a name="per-customer-isolation"></a>Isolatie per klant
Netwerktoegangsbeheer geïmplementeerd op Azure en scheiding door middel van VLAN-isolatie, ACL's, balancers en IP-filters laden

Klanten kunnen hun bronnen verder isoleren via abonnementen, resourcegroepen, virtuele netwerken en subnetten.

## <a name="screening"></a>Screening

De onlangs aangekondigde FedRAMP hoog en de erkenning van de Department of Defense (DoD) gevolgen niveau 4. Dit is de beveiliging en naleving van de balk tussen de regering van Azure-omgeving verhoogd.

We zijn nu onze operators op het nationale bureau controleren rasteren met wet- en krediet (NACLC) zoals gedefinieerd in de sectie 5.6.2.2 van het DoD Cloud Computing Security eisen Guide (SRG):

>[AZURE.NOTE] Het minimale achtergrond onderzoek voor CSP medewerkers die toegang hebben tot niveau 4 en 5 informatie op basis van een "niet-kritieke gevoelige' (bijv. van DoD ADP-2) is een nationaal bureau contact op met wet- en krediet (NACLC) (voor"niet-kritieke gevoelige"aannemers) of een matig risico achtergrond onderzoek (MBI) voor de aanwijzing van een"beperkt risico"positie.

In de volgende tabel bevat een overzicht van onze huidige screening voor Azure overheid operators:

Azure finan screenings- en achtergrondcontroles | Beschrijving|
---|---|
Amerikaanse burgerschap |Controle van de Amerikaanse burgerschap.
Microsoft cloud achtergrondcontrole (om de twee jaar)|Sofi-nummer zoeken, selectievakje criminele geschiedenis, lijst van buitenlandse activa besturingselement Office (OFAC), Bureau van industrie en veiligheid lijst (BIS) lijst met Office van defensie handel besturingselementen uitgesloten personen.
Selectievakje National Agency met wet- en krediet (NACLC) (om de vijf jaar) | Vingerafdruk achtergrond gecontroleerd FBI databases toevoegen. Ga naar de<a href="https://www.opm.gov/investigations/background-investigations/federal-investigations-notices/1997/fin97-02/"> Site van Office personeel Management</a>voor meer informatie. | 
<a href="https://www.microsoft.com/en-us/TrustCenter/Compliance/CJIS">Strafrechtelijke Justitie Information Services (CJIS)</a> | CJIS is een staat, lokale en FBI overheid welke processen vingerafdruk screening vastgelegd en criminele geschiedenis op operationele medewerkers die toegang tot essentiële crimineel Justitie gegevens (CJI) kan worden gevalideerd.  Elke staat heeft een eigen achtergrond controleren en de latere goedkeuring van alle werknemers met mogelijke toegang tot CJI.|

Azure operationeel personeel, de volgende beginselen voor de toegang van toepassing:

- Rechten moeten duidelijk omschreven zijn, met afzonderlijke verantwoordelijkheden voor het aanvragen, goedkeuren en implementeren van wijzigingen.
- De toegang is via gedefinieerde interfaces die specifieke functionaliteit hebben.
- Just in time (JIT) is en alleen op basis van per incident of voor een gebeurtenis van het specifieke onderhoud en altijd voor een beperkte duur wordt verleend.
- Toegang is regel-gebaseerd, met de gedefinieerde rollen die alleen de machtigingen die nodig zijn voor het oplossen van zijn toegewezen.

Screening normen zijn de validatie van Amerikaanse burgerschap van alle Microsoft-ondersteuning en operationele medewerkers voordat toegang wordt verleend aan systemen Azure overheid zijn ondergebracht. Ondersteunend personeel die nodig is om gegevens te brengen gebruiken de veilige mogelijkheden binnen de overheid Azure. Veilige gegevensoverdracht moet een aparte set met verificatiereferenties om toegang te krijgen. Bijvoorbeeld voor toegang tot de metagegevens van het systeem, beheerders en operators gebruiken specifieke interne management web gebaseerde hulpprogramma's, alleen-lezen-API's en JIT tot misbruik van bevoegdheden.

## <a name="next-steps"></a>Volgende stappen

Voor aanvullende informatie en updates Abonneer u op de <a href="https://blogs.msdn.microsoft.com/azuregov/">Microsoft Azure overheid Blog.</a>
