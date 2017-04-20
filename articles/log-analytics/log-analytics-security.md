<properties
    pageTitle="Logboek Analytics gegevens | Microsoft Azure"
    description="Informatie over het logboek Analytics uw privacy beschermt en beveiligt uw gegevens."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/23/2016"
    ms.author="banders"/>

# <a name="log-analytics-data-security"></a>Logboek Analytics gegevens

Microsoft hecht zeer aan de bescherming van uw privacy en beveiliging van uw gegevens bij de levering van software en services die u helpen bij het beheren van de IT-infrastructuur van uw organisatie. We erkennen dat wanneer u uw gegevens aan anderen toevertrouwen, vertrouwensrelatie strenge beveiliging vereist. Microsoft voldoet aan strenge richtlijnen voor compatibiliteit en beveiliging — van het coderen van een dienst.

Bevestiging en bescherming van gegevens is een topprioriteit bij Microsoft. Neem contact met ons opnemen met vragen, suggesties of problemen met een van de volgende gegevens, met inbegrip van onze beveiligingsmaatregelen op [Azure ondersteuningsopties](http://azure.microsoft.com/support/options/).

In dit artikel wordt uitgelegd hoe gegevens is verzameld, behandeld en beveiligd door Analytics logboek Operations Management Suite (OMS). Agenten kunt u verbinding maken met de webservice of gegevens ophalen uit Azure diagnostische gegevens voor gebruik door logboek Analytics System Center Operations Manager gebruiken om operationele gegevens te verzamelen. De verzamelde gegevens worden verzonden via Internet met behulp van op certificaten gebaseerde verificatie en SSL-3 met de logboek-Analytics-service wordt gehost in Microsoft Azure. Gegevens worden door de agent worden gecomprimeerd voordat deze wordt verzonden.

Het logboek Analytics-service beheert de cloud-gebaseerde gegevens veilig met behulp van de volgende methoden:

- scheiding van de gegevens
- gegevens bewaren
- fysieke beveiliging
- Oproepbeheer
- conformiteit
- Security standards-certificeringen


## <a name="data-segregation"></a>Scheiding van de gegevens

Gegevens van de klant is logisch gescheiden gehouden op elk onderdeel in de OMS-service. Alle gegevens per organisatie wordt gelabeld. Op deze manier blijft bestaan gedurende de levenscyclus van gegevens en deze wordt toegepast op elke laag van de service. Elke klant heeft een speciale Azure blob met de gegevens op lange termijn

## <a name="data-retention"></a>Gegevens bewaren

Geïndexeerde logboekgegevens zoekopdracht is opgeslagen en behouden volgens uw plan prijsstelling. Zie het [Logboek Analytics prijzen](https://azure.microsoft.com/pricing/details/log-analytics/)voor meer informatie.

Microsoft verwijdert klantgegevens 30 dagen nadat de OMS-werkruimte is gesloten. Microsoft verwijdert ook de Azure opslag rekening waar de gegevens zich bevindt. Het verwijderen van gegevens van de klant is worden geen fysieke schijven vernietigd.

De volgende tabel worden enkele van de beschikbare oplossingen in OMS en voorbeelden de soorten gegevens die zij verzamelen.

| **Oplossing** | **Gegevenstypen** |
| --- | --- |
| Beoordeling van de configuratie | Configuratiegegevens, metagegevens en gegevens |
| Capaciteitsplanning | Prestatiegegevens en metagegevens |
| Antimalware | Van configuratiegegevens en metagegevens |
| Beoordeling van System Update | Metagegevens en de statusgegevens |
| Log-beheer | Door de gebruiker gedefinieerde gebeurtenislogboeken, logboeken van Windows en/of de IIS-logboeken |
| Wijzigingen bijhouden | Software-inventarisatie en metagegevens van de Windows-Service |
| SQL en beoordeling van Active Directory | WMI-gegevens, registergegevens prestatiegegevens en beheer van SQL Server dynamische resultaten weergeven |

De volgende tabel ziet u voorbeelden van gegevenstypen:

| **Gegevenstype** | **Velden** |
| --- | --- |
| Een waarschuwing | Waarschuw de naam, beschrijving van waarschuwing, BaseManagedEntityId, probleem-ID, IsMonitorAlert, RuleId, ResolutionState, prioriteit, prioriteit, categorie, eigenaar, ResolvedBy, TimeRaised, TimeAdded, LastModified, LastModifiedBy, LastModifiedExceptRepeatCount, TimeResolved, TimeResolutionStateLastModified, TimeResolutionStateLastModifiedInDB, RepeatCount |
| Configuratie | Klant-id, AgentID, id van de entiteit, ManagedTypeID, ManagedTypePropertyID, CurrentValue, ChangeDate |
| Gebeurtenis | Gebeurtenis-id, EventOriginalID, BaseManagedEntityInternalId, RuleId, PublisherId, PublisherName, FullNumber, nummer, categorie, ChannelLevel, LoggingComputer, EventData, EventParameters, TimeGenerated, TimeAdded <br>**Opmerking:** Wanneer u gebeurtenissen met aangepaste velden zich in het gebeurtenislogboek van Windows, worden ze OMS verzameld. |
| Metagegevens | BaseManagedEntityId, ObjectStatus, OrganizationalUnit, ActiveDirectoryObjectSid, PhysicalProcessors, netwerknaam, IP-adres, ForestDNSName, NetbiosComputerName, VirtualMachineName, LastInventoryDate, HostServerNameIsVirtualMachine, IP-adres, NetbiosDomainName, LogicalProcessors, DNS-naam, naam, DomainDnsName, ActiveDirectorySite, PrincipalName, OffsetInMinuteFromGreenwichTime |
| Prestaties | Objectnaam, CounterName, PerfmonInstanceName, PerformanceDataId, PerformanceSourceInternalID, SampleValue, TimeSampled, TimeAdded |
| Staat | StateChangeEventId, StateId, NewHealthState, OldHealthState, Context, TimeGenerated, TimeAdded, StateId2, BaseManagedEntityId, MonitorId, HealthState, LastModified, LastGreenAlertGenerated, DatabaseTimeModified |

## <a name="physical-security"></a>Fysieke beveiliging

Het logboek Analytics in OMS-service is bemand door Microsoft-personeel en alle activiteiten worden vastgelegd en kunnen worden gecontroleerd. De service in Azure volledig wordt uitgevoerd en de Azure gemeenschappelijke technische criteria voldoen. U kunt details over de fysieke beveiliging van Azure activa weergeven op pagina 18 van de [Beveiliging van Microsoft Azure-overzicht](http://download.microsoft.com/download/6/0/2/6028B1AE-4AEE-46CE-9187-641DA97FC1EE/Windows%20Azure%20Security%20Overview%20v1.01.pdf). Fysieke toegang worden tot beveiligde gebieden gewijzigd binnen één werkdag voor iedereen die niet langer verantwoordelijk voor de OMS-service is, met inbegrip van de overdracht en beëindiging. U kunt lezen over de algemene fysieke infrastructuur die we op [Microsoft Datacenters gebruiken](https://www.microsoft.com/en-us/server-cloud/cloud-os/global-datacenters.aspx).

## <a name="incident-management"></a>Oproepbeheer

OMS is een incident management proces die voor alle Microsoft-services worden aangehouden. Om samen te vatten, we:

- Gebruik een gedeelde verantwoordelijkheid model waar een deel van de verantwoordelijkheid van de beveiliging van Microsoft hoort tijdens een gedeelte bij de klant hoort
- Azure-incidenten beheren
  - Detecteren van een incident op de eerste vermelding, een onderzoek starten
  - Een evaluatie van de gevolgen en de ernst van het incident door een lid op afroep incident response team. Op basis van bewijsmateriaal, de beoordeling kan of niet kan leiden tot verdere escalatie naar het security response team.
  - Diagnose van een incident door reactie van beveiligingsexperts aan het technische en forensische onderzoek verrichten, identificeren strategieën voor insluiting, risicobeperking en tijdelijke oplossing. Als het security team is van mening dat gegevens van de klant kan hebben worden blootgesteld aan een onwettige of niet-geautoriseerde persoon, begint de parallelle uitvoering van het proces van de klant Incident melding parallel.  
  - Stabiliseren en herstellen van het incident. Het incident response team maakt een herstelplan te verminderen van het probleem. Crisis insluiting werk zoals risico systemen isoleert treedt op onmiddellijk en in samenspraak met de diagnose. Oplossingen voor langere termijn kunnen worden gepland die optreden nadat het directe risico is verstreken.  
  - Sluit het incident en een autopsie uitvoeren. Het incident response team maakt een post-mortem-waarin de details van het incident, met het voornemen te herzien beleid, procedures en processen om te voorkomen dat een herhaling van de gebeurtenis.
- Informeer klanten over veiligheidsincidenten
  - Bepalend voor de omvang van risico klanten en iedereen die wordt beïnvloed als een aankondiging mogelijk gedetailleerde
  - Maak een aankondiging voor klanten met gedetailleerde voldoende informatie zodat ze kunnen uitvoeren van een onderzoek op hun einde en voldoen aan alle verplichtingen die zij in hun eindgebruikers hebt aangebracht terwijl het meldingsproces niet onnodig te vertragen.
  - Bevestigen en verklaart het incident, indien nodig.
  - Klanten met een melding van een incident zonder onredelijke vertraging en volgens alle wettelijke of contractuele verbintenis in kennis. Meldingen van beveiligingsincidenten wordt geleverd aan een of meer van de beheerders van de klant door middel van die Microsoft selecteert, inclusief via e-mail.
- Team gereedheid en training
  - Personeel van Microsoft zijn vereist voor het voltooien van de beveiliging en het bewustzijn opleiding, die helpt hen te identificeren en rapporteren van verdachte beveiligingsproblemen.  
  - Operatoren die werken op de service Microsoft Azure hebben toevoeging training verplichtingen rond hun toegang tot gevoelige gegevens van de klant die als host fungeert-systemen.
  - Personeel van Microsoft security response krijgen gespecialiseerde opleiding voor hun rollen


Bij verlies van de gegevens van elke klant kennis we elke klant binnen één dag. Gegevensverlies klant is echter nooit opgetreden met OMS. Ook wij kopieën van gegevens die zijn gemaakt en het geografisch wordt gedistribueerd.

Zie voor meer informatie over hoe Microsoft moet op beveiligingsincidenten reageren [Microsoft Azure Security Response in de Cloud](https://gallery.technet.microsoft.com/Azure-Security-Response-in-dd18c678/file/150826/1/Microsoft Azure Security Response in the cloud.pdf).

## <a name="compliance"></a>Conformiteit

OMS software ontwikkeling en service van het team informatie beveiliging en governance programma ondersteunt de bedrijfsvereisten en voldoet aan de wettelijke en bestuursrechtelijke bepalingen, zoals is beschreven aan het [Vertrouwenscentrum van Microsoft Azure](https://azure.microsoft.com/support/trust-center/) en [Naleving van Microsoft Trust Center](https://www.microsoft.com/en-us/TrustCenter/Compliance/default.aspx). Hoe OMS beveiligingsvereisten vaststelt, bevat besturingselementen voor beveiliging, beheerd en gecontroleerd risico's zijn er ook beschreven. Eenmaal per jaar, we een evaluatie uitvoeren van beleid, normen, procedures en richtlijnen.

Elk teamlid OMS ontwikkeling ontvangt beveiligingstraining van formele aanvraag. We gebruiken een versiebeheersysteem intern voor softwareontwikkeling. Elk project software wordt beschermd door het versiebeheersysteem.

Microsoft heeft een beveiliging en naleving team dat toezicht houdt op en beoordeelt alle Microsoft-services. Informatie veiligheidsbeambten maken van het team en zijn niet gekoppeld aan de technische diensten die OMS ontwikkelen. De veiligheidsbeambten hebben hun eigen keten beheer en uitvoering van een onafhankelijke beoordelingen van de producten en diensten te garanderen, beveiliging en naleving.

Ontvangt de Raad van bestuur van Microsoft en jaarlijks een verslag over de gegevensbeveiliging alle programma's van Microsoft.

De OMS software ontwikkel- en service-team werkt actief met de teams Microsoft Legal en conformiteit en andere IT-partners een groot aantal certificaten verkrijgen.

## <a name="security-standards-certifications"></a>Security standards-certificeringen

Aanmelden Analytics OMS op dit moment voldoen aan de volgende normen voor beveiliging:

- [ISO/IEC 27001](http://www.iso.org/iso/home/standards/management-standards/iso27001.htm) en [ISO/IEC 27018:2014](http://www.iso.org/iso/home/store/catalogue_tc/catalogue_detail.htm?csnumber=61498) -compatibel
- Betaling-kaart (PCI-compatibele) gegevens beveiliging industriestandaard (PCI DSS) door de PCI-Veiligheidsraad normen.
- [Service organisatie besturingselementen (SOC) 1 Type 1 en 2 Type-1 SOC](https://www.microsoft.com/en-us/TrustCenter/Compliance/SOC1-and-2) compatibele
- Windows gemeenschappelijke technische Criteria
- Microsoft Trustworthy Computing-certificering
- Als een Azure service voldoen de OMS onderdelen aan conformiteitsvereisten Azure. U kunt meer lezen op de [Naleving van Microsoft Trust Center](https://www.microsoft.com/en-us/TrustCenter/Compliance/default.aspx).


## <a name="cloud-computing-security-data-flow"></a>Cloud computing gegevensstroom van de beveiliging
Het volgende diagram toont de beveiligingsarchitectuur van een wolk als de stroom van informatie van uw bedrijf en hoe wordt beveiligd is verplaatst naar het logboek Analytics-service, uiteindelijk wordt gezien door u in de OMS-portal. Meer informatie over elke stap volgt het diagram.

![Afbeelding van OMS het verzamelen van gegevens en beveiliging](./media/log-analytics-security/log-analytics-security-diagram.png)

## <a name="1-sign-up-for-log-analytics-and-collect-data"></a>1. Meld u aan voor de analyse van de Log en gegevens verzamelen

Voor uw organisatie om gegevens te verzenden naar het logboek Analytics u Windows agenten, agenten met Azure virtuele machines of OMS agenten voor Linux. Als u Operations Manager agenten, vervolgens gebruikt u een configuratiewizard in de console bewerkingen ze te configureren. Gebruikers (die mogelijk u afzonderlijke gebruikers of een groep personen) een of meer OMS-accounts (OMS werkruimten) maken en agenten registreren met behulp van een van de volgende accounts:


- [Organisatie-ID](../active-directory/sign-up-organization.md)

- [Microsoft-Account - Outlook, Office Live MSN](http://www.microsoft.com/account/default.aspx)

Een werkruimte OMS is waarbij gegevens verzameld, samengevoegd, geanalyseerd en gepresenteerd. Een werkruimte wordt voornamelijk gebruikt als een manier om partitiegegevens en elke werkruimte is uniek. U wilt uw productiegegevens die worden beheerd met één OMS werkruimte en uw testgegevens die worden beheerd met een andere werkruimte. Werkruimten met behulp van een beheerder gebruikerstoegang tot de gegevens. Elke werkruimte kan meerdere gebruikersaccounts zijn gekoppeld, en elke gebruikersaccount toegang heeft tot meerdere OMS-werkruimte. U maakt op basis van regio datacenter werkruimten. Elke werkruimte wordt gerepliceerd naar andere datacenters in de regio, voornamelijk voor OMS service beschikbaar.

Wanneer de configuratiewizard is voltooid, maakt elke groep Operations Manager management voor Operations Manager, verbinding met de logboek-Analytics-service. Vervolgens gebruikt u de Wizard Computers toevoegen te kiezen welke computers in de groep management zijn toegestaan om gegevens te verzenden naar de service. Voor andere typen agent verbindt elk veilig met de OMS-service.

Alle communicatie tussen verbonden systemen en het logboek voor Analytics-service gecodeerd.  Het protocol TLS (HTTPS) wordt gebruikt voor codering.  Het proces van Microsoft SDL wordt logboek Analytics wordt bijgewerkt met de meest recente vorderingen in cryptografische protocollen gevolgd.

Elk type agent verzamelt gegevens voor logboek Analytics. Het type gegevens dat wordt verzameld, is afhankelijk van de typen oplossingen die worden gebruikt. Hier ziet u een overzicht van het verzamelen van gegevens op [oplossingen logboek Analytics toevoegen uit de galerie met oplossingen](log-analytics-add-solutions.md). Meer gedetailleerde informatie van de collectie is ook beschikbaar voor de meeste oplossingen. Een oplossing is een bundel van vooraf gedefinieerde weergaven, logboek zoekopdrachten gegevens verzameling regels en logica voor verwerking. Alleen beheerders kunnen Log Analytics gebruiken voor het importeren van een oplossing. Nadat de oplossing is geïmporteerd, wordt deze verplaatst naar de beheerservers Operations Manager (indien gebruikt), en vervolgens naar alle agenten die u hebt gekozen. Daarna wordt verzamelen de agenten de gegevens.

## <a name="2-send-data-from-agents"></a>2. verzenden van gegevens van agents

U registreert alle agent typen met een sleutel voor inschrijving en een beveiligde verbinding tussen de agent en de logboekbestanden Analytics-service met behulp van op certificaten gebaseerde verificatie en SSL met poort 443. OMS gebruikt een geheime winkel te genereren en onderhouden van sleutels. Persoonlijke sleutels worden gedraaid om de 90 dagen en in Azure worden opgeslagen en worden beheerd door de Azure bewerkingen die op een strikte regelgeving en de naleving van procedures.

Met Operations Manager, registreert u een werkruimte met de logboek-Analytics-service en een beveiligde HTTPS-verbinding wordt tot stand gebracht tussen de Operations Manager server.

Een opslag voor alleen-lezen-sleutel wordt gebruikt voor de agenten Windows Azure virtuele machines waarop, diagnostische gebeurtenissen in Azure tabellen lezen.

Als de agent kan niet communiceren met de service om welke reden, de verzamelde gegevens lokaal opgeslagen in een tijdelijke cache en de server probeert te verzenden de gegevens elke 8 minuten gedurende 2 uur. In cache opgeslagen gegevens van de agent wordt beschermd door het archief met referenties van het besturingssysteem. Als de service de gegevens niet na 2 uur verwerken kan, zal de gegevens de agenten een wachtrij. Als de wachtrij vol raakt, begint OMS gegevenstypen, beginnend met de prestatiegegevens neer te zetten. De wachtrijlimiet van agent is een registersleutel kunt u, indien nodig. Verzamelde gegevens wordt gecomprimeerd en verzonden naar de service, het overslaan van databases op gebouwen, zodat wordt extra belasting voor hen. Nadat de verzamelde gegevens is verzonden, wordt deze verwijderd uit de cache.

Zoals hierboven beschreven, wordt gegevens uit uw agenten naar Microsoft Azure datacenters verzonden via SSL. Desgewenst kunt u ExpressRoute extra beveiliging biedt voor de gegevens. ExpressRoute is een manier om rechtstreeks verbinding maken met Azure uit het bestaande WAN-netwerk, zoals een multiprotocol label switching (MPLS), VPN, verstrekt door een netwerkprovider. Zie [ExpressRoute](https://azure.microsoft.com/services/expressroute/)voor meer informatie.


## <a name="3-the-log-analytics-service-receives-and-processes-data"></a>3. de Log Analytics-service ontvangt en verwerkt gegevens

Het logboek voor Analytics-service zorgt ervoor dat binnenkomende gegevens van een vertrouwde bron door het valideren van de certificaten en de integriteit van gegevens met Azure-verificatie. De onbewerkte gegevens van niet-verwerkte wordt vervolgens opgeslagen als een blob in [Microsoft Azure opslag](../storage/storage-introduction.md) en is niet gecodeerd. Elke opslag Azure blob heeft echter een aantal unieke set van sleutels is alleen toegankelijk voor die gebruiker. Het type gegevens dat is opgeslagen, is afhankelijk van de typen oplossingen die zijn geïmporteerd en gebruikt om gegevens te verzamelen. Het logboek voor Analytics-service verwerkt de onbewerkte gegevens voor de opslag van Azure blog.

## <a name="4-use-log-analytics-to-access-the-data"></a>4. logboek Analytics gebruiken voor toegang tot de gegevens

U kunt aanmelden voor Analytics logboek in de portal OMS met behulp van de organisatie-account of een Microsoft-account die u eerder hebt ingesteld. Al het verkeer tussen de OMS portal en Log Analytics in OMS wordt verzonden via een beveiligde HTTPS-verbinding. Wanneer u de portal OMS, een sessie-ID wordt gegenereerd op de client van de gebruiker (webbrowser) en gegevens worden opgeslagen in een lokale cache totdat de sessie wordt beëindigd. Wanneer beëindigd, wordt de cache verwijderd. Client-side cookies die geen persoonlijke gegevens bevatten, worden niet automatisch verwijderd. Cookies per sessie HTTPOnly zijn gemarkeerd en worden beveiligd. Na een vooraf bepaalde niet-actieve periode wordt OMS portal sessie beëindigd.

Met behulp van de OMS-portal kunt u gegevens exporteren naar een CSV-bestand en u hebt toegang tot gegevens met behulp van API's zoeken. CSV-export is beperkt tot 50.000 rijen per exporteren en API-gegevens is beperkt tot 5.000 rijen per zoekopdracht.

## <a name="next-steps"></a>Volgende stappen

- [Aan de slag met Analytics logboek](log-analytics-get-started.md) voor meer informatie over logboekbestanden Analytics en get up en wordt uitgevoerd in minuten.
