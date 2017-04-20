<properties
    pageTitle="Azure cool opslag voor BLOB's | Microsoft Azure"
    description="Niveaus van opslag voor Azure Blob-opslag bieden rendabele opslag van gegevens van het object op basis van access-patronen. De laag cool opslag is geoptimaliseerd voor gegevens die minder vaak wordt benaderd."
    services="storage"
    documentationCenter=""
    authors="michaelhauss"
    manager="vamshik"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="mihauss"/>


# <a name="azure-blob-storage-hot-and-cool-storage-tiers"></a>Azure Blob-opslag: Hot en cool opslag lagen

## <a name="overview"></a>Overzicht

Azure opslag biedt nu twee niveaus van opslag voor Blob-opslag (opslag van objecten), zodat u gegevens meest voordelig afhankelijk van hoe u deze kunt opslaan. De Azure **hot opslag laag** is geoptimaliseerd voor het opslaan van gegevens die vaak wordt geopend. De Azure **tier cool opslag** is geoptimaliseerd voor het opslaan van gegevens die niet vaak worden benaderd en lange levensduur. Gegevens in de laag cool opslag kan een iets lagere beschikbaarheid tolereren, maar nog steeds hoge duurzaamheid en vergelijkbare tijd voor toegang tot en doorvoer kenmerken als hot gegevens vereist. Zijn aanvaardbaar compromis voor een veel lagere opslagkosten voor leuke gegevens, iets lager beschikbaarheid SLA en hogere kosten van de toegang.

Gegevens die zijn opgeslagen in de cloud is vandaag de dag een exponentiële tempo groeien. Voor het beheren van kosten voor uw groeiende opslagbehoeften, is het handig om uw gegevens op basis van kenmerken zoals de frequentie van access en de geplande periode. Gegevens die zijn opgeslagen in de cloud is heel anders in termen van hoe deze wordt gegenereerd, verwerkt en toegankelijk via de levensduur. Sommige gegevens is actief geopend en gewijzigd gedurende de levensduur. Sommige gegevens is zeer vaak vroeg in de levensduur, toegankelijk voor access drastisch als de leeftijd gegevens neer te zetten. Sommige gegevens actief blijft in de cloud en zelden, opgeslagen als ooit een keer hebt geopend.

Elk van deze bovenstaande voordelen van een gedifferentieerde laag van opslag die is geoptimaliseerd voor een bepaald access-patroon-scenario's. Met de introductie van hot en cool opslag niveaus afzonderlijke Azure Blob storage lost nu deze behoefte aan gedifferentieerde opslag lagen met modellen prijzen.

## <a name="blob-storage-accounts"></a>Accounts van BLOB-opslag

**BLOB storage accounts** zijn gespecialiseerde opslag voor uw ongestructureerde gegevens opgeslagen als BLOB's (objecten) in Azure opslag rekeningen. Met Blob storage accounts, kunt u nu kiezen tussen hot en cool opslag lagen op te slaan cool gegevens minder vaak gebruikt bij de opslag van een lagere kosten en gegevensopslag meer veelgebruikte hot op een lagere kosten van de toegang. BLOB-opslag rekeningen zijn vergelijkbaar met de bestaande algemene opslag-accounts en de uitstekende duurzaamheid, beschikbaarheid, schaalbaarheid en prestatiefuncties waarmee u vandaag, met inbegrip van de consistentie van 100% API voor blok BLOB's delen en BLOB's toevoegen.

> [AZURE.NOTE] BLOB storage accounts ondersteunen alleen blokkeren en BLOB's en BLOB geen pagina's toevoegen.

BLOB storage accounts worden blootgesteld het kenmerk **Access laag** waarmee u de opslag laag als **warm** of **koel** afhankelijk van de gegevens in de account opgeven. Als er een wijziging in het gebruikspatroon van uw gegevens, kunt u ook schakelen tussen deze lagen opslag op elk gewenst moment.

> [AZURE.NOTE] De opslag laag wijzigen, kan dit leiden tot extra kosten. Zie de sectie [voor prijzen en facturering](storage-blob-storage-tiers.md#pricing-and-billing) voor meer informatie.

Voorbeeld van de gebruiksscenario's voor de opslag van warm laag zijn:

- Gegevens die in gebruik is of moet worden geopend (gelezen van en geschreven naar) regelmatig.
- Gegevens die voor verwerking en eventuele migratie naar de leuke opslag laag wordt klaargezet.

Voorbeeld gebruiksscenario's voor de leuke opslag laag zijn:

- Back-up, archivering en disaster recovery datasets.
- Oudere media-inhoud niet vaak meer bekeken maar is naar verwachting beschikbaar bij direct toegankelijk.
- Grote gegevenssets die worden opgeslagen kosten effectief moeten terwijl er meer gegevens worden verzameld voor toekomstige verwerking. (*bv.*de langetermijnopslag van wetenschappelijke gegevens, onbewerkte telemetriegegevens van een fabriek)
- (Oorspronkelijke raw)-gegevens die moeten worden bewaard, zelfs nadat deze is verwerkt in de uiteindelijke bruikbare vorm. (*bv.*ruwe mediabestanden na transcodering in andere indelingen)
- Conformiteit en archivering van de gegevens die moet worden opgeslagen voor een lange tijd en nauwelijks ooit wordt geopend. (*bijv.*, beveiliging camera beelden, oude X-Rays/MRIs voor organisaties in de gezondheidszorg, audio-opnamen en transcripts van klant oproepen voor financiële diensten)

Zie [over Azure opslag accounts](storage-create-storage-account.md) voor meer informatie over opslag rekeningen.

Voor toepassingen waarbij alleen blokkeren of blob-opslag wilt toevoegen, kunt u het beste Blob storage-accounts om te profiteren van de gedifferentieerde prijsmodel van gelaagde opslag gebruiken. Wij begrijpen echter dat dit niet mogelijk dat onder bepaalde omstandigheden als de manier om te gaan, zoals met behulp van algemene opslag rekeningen zou zijn:

- U moet tabellen, wachtrijen of bestanden gebruiken en wilt uw BLOB's opgeslagen in dezelfde account voor opslag. Opmerking Er is geen technische voordelen om op te slaan in dezelfde account dan met dezelfde gedeelde sleutels.
- U moet nog steeds het model Klassiek implementatie gebruiken. BLOB storage accounts zijn alleen beschikbaar via het implementatiemodel Azure Resource Manager.
- U moet de pagina BLOB's gebruiken. BLOB storage accounts bieden geen ondersteuning voor BLOB's pagina. In het algemeen het beste blok BLOB's gebruiken tenzij u een specifieke behoefte aan BLOB's pagina.
- U gebruikt een versie van de [Opslag Services REST API](https://msdn.microsoft.com/library/azure/dd894041.aspx) die ouder is dan 2014-02-14 of een clientbibliotheek met een lagere versie dan 4.x en kan geen upgrade uitvoeren voor uw toepassing.

> [AZURE.NOTE] BLOB storage accounts worden ondersteund in een meerderheid van Azure gebieden met meer te volgen. Hier vindt u de bijgewerkte lijst van beschikbare regio's op de pagina [Azure Services per regio](https://azure.microsoft.com/regions/#services) .

## <a name="comparison-between-the-storage-tiers"></a>Vergelijking tussen de niveaus van opslag

De volgende tabel bevat de vergelijking tussen de twee lagen van de opslag:

<table border="1" cellspacing="0" cellpadding="0" style="border: 1px solid #000000;">
<col width="250">
<col width="250">
<col width="250">
<tbody>
<tr>
    <td><strong><center></center></strong></td>
    <td><strong><center>Hot opslag laag</center></strong></td>
    <td><strong><center>Leuke opslag laag</center></strong></td
</tr>
<tr>
    <td><strong><center>Beschikbaarheid</center></strong></td>
    <td><center>99,9%</center></td>
    <td><center>99%</center></td>
</tr>
<tr>
    <td><strong><center>Beschikbaarheid<br>(RA GRS leest)</center></strong></td>
    <td><center>99,99%</center></td>
    <td><center>99,9%</center></td>
</tr>
<tr>
    <td><strong><center>Kosten</center></strong></td>
    <td><center>Hogere kosten voor opslag<br>Lagere kosten voor toegang tot en de transactie</center></td>
    <td><center>Lagere kosten voor opslag<br>Hogere kosten voor toegang tot en de transactie</center></td>
</tr>
<tr>
    <td><strong><center>Object minimale grootte<center></strong></td>
    <td colspan="2"><center>N.V.T.</center></td>
</tr>
<tr>
    <td><strong><center>Duur van de minimale opslag<center></strong></td>
    <td colspan="2"><center>N.V.T.</center></td>
</tr>
<tr>
    <td><strong><center>Latentie<br>(Tijd voor de eerste byte)<center></strong></td>
    <td colspan="2"><center>milliseconden</center></td>
</tr>
<tr>
    <td><strong><center>Doelen voor schaalbaarheid en prestaties<center></strong></td>
    <td colspan="2"><center>Zelfde als algemene opslag rekeningen</center></td>
</tr>
</tbody>
</table>

> [AZURE.NOTE] BLOB storage accounts ondersteunen de dezelfde prestaties en schaalbaarheid doelen als algemene opslag rekeningen. [Azure opslag schaalbaarheid en prestaties, doelen](storage-scalability-targets.md) Zie voor meer informatie.

## <a name="pricing-and-billing"></a>Prijzen en facturering

Een nieuwe prijsmodel BLOB storage accounts gebruiken voor blob-opslag op basis van de opslag laag. Wanneer u een Blob storage-account gebruikt, gelden de volgende overwegingen rekening:

- **Kosten voor opslag**: naast de hoeveelheid gegevens die zijn opgeslagen, de kosten van het opslaan van gegevens hangt af van de opslag laag. De kosten per gigabyte is lager voor de leuke opslag laag dan voor de opslag van warm laag.
- **Data access-kosten**: voor de gegevens in de koele opslag laag, brengt een toeslag per gigabyte data access voor lees- en schrijfbewerkingen.
- **Transactiekosten**: Er is een vergoeding per transactie voor beide lagen. De kosten per transactie voor de leuke opslag laag is echter hoger dan die voor de opslag van warm laag.
- **Kosten van de gegevensoverdracht geo-replicatie**: dit geldt alleen voor accounts met geo-replicatie wordt geconfigureerd, met inbegrip van GRS en RA GRS. Geo-replicatie gegevensoverdracht systeem kosten per gigabyte.
- **Uitgaande van de gegevensoverdracht kosten**: uitgaande gegevensoverdracht (gegevens die worden overgebracht uit een regio Azure) tot facturering van bandbreedtegebruik op een basis per gigabyte, consistent zijn met algemene opslag rekeningen.
- **De opslag laag wijzigen**: wijzigen van de laag voor de opslag van koel aan hot kosten gelijk is aan het lezen van alle gegevens in de opslag-account voor elke overgang bestaande zal oplopen. Aan de andere kant te wijzigen in de laag voor de opslag van warm koel worden vrij van kosten.

> [AZURE.NOTE] Gebruikers naar de nieuwe opslag lagen uitproberen en valideren van functionaliteit na de introductie, de kosten voor het wijzigen van de opslag laag van koel aan hot uit doorloopt tot en met 30 juni-2016. Vanaf juli 1e 2016 wordt wordt de toeslag toegepast op alle overgangen vanuit koel aan hot. Zie voor meer informatie op het prijsmodel voor Blob-opslag rekeningen, [Azure opslag prijzen](https://azure.microsoft.com/pricing/details/storage/) pagina. Zie voor meer informatie op de uitgaande gegevens overdracht toeslagen, [Data overdrachten prijzen](https://azure.microsoft.com/pricing/details/data-transfers/) pagina.

## <a name="quick-start"></a>Snel starten

In deze sectie wordt ingegaan op de volgende scenario's met de Azure portal:

- Het maken van een account Blob-opslag.
- Het beheren van een Blob storage-account.

### <a name="using-the-azure-portal"></a>Met behulp van de portal Azure

#### <a name="create-a-blob-storage-account-using-the-azure-portal"></a>Een Blob storage account maken met de Azure portal

1. Log in om de [Azure portal](https://portal.azure.com).

2. Selecteer **Nieuw**in het menu Hub > **Data- +** > **opslag account**.

3. Voer een naam voor de account van uw opslag.

    Deze naam moet uniek; het wordt gebruikt als onderdeel van de URL die wordt gebruikt voor toegang tot de objecten in de opslag.  

4. Selecteer de **Resource Manager** als implementatiemodel voor de.

    Gelaagde opslag kan alleen worden gebruikt met Resource Manager opslag rekeningen; Dit is de aanbevolen implementatie voor nieuwe resources. Bekijk de [Azure Resource-beheer-overzicht](../azure-resource-manager/resource-group-overview.md)voor meer informatie.  

5. Selecteer in de vervolgkeuzelijst type Account **Blob-opslag**.

    Dit is waar u het type opslag account selecteren. Gelaagde opslag is niet beschikbaar in opslag voor algemene doeleinden; de optie is alleen beschikbaar in de Blob storage type account.    

    Houd er rekening mee dat wanneer u deze optie selecteert, de prestaties laag is ingesteld op standaard. Gelaagde opslag is niet beschikbaar in de laag Premium prestaties.

6. Selecteer de replicatieoptie voor de opslag: **LRS**, **GRS**of **RA GRS**. De standaardwaarde is **RA GRS**.

    LRS = lokaal redundante opslag; GRS = geo-redundante opslag (2 regio's); RA GRS is leestoegang geo-redundante opslag (2 regio's met leestoegang tot de tweede).

    Bekijk voor meer informatie over replicatie Azure opslagopties, [Azure Storage replicatie](storage-redundancy.md).

7. Selecteer de juiste opslag laag voor uw behoeften: de **toegang tot laag** ingesteld op **koel** of **warm**. De standaardwaarde is **Hot**.

8. Selecteer het abonnement waar u wilt de nieuwe opslag-account maken.

9. Een nieuwe resourcegroep of Selecteer een bestaande brongroep. Zie voor meer informatie over resourcegroepen, [Azure Resource Manager-overzicht](../azure-resource-manager/resource-group-overview.md).

10. Selecteer de regio voor uw opslag-account.

11. Klik op **maken** om de opslag-account te maken.

#### <a name="change-the-storage-tier-of-a-blob-storage-account-using-the-azure-portal"></a>De opslag-laag van een met de portal Azure Blob opslag-account wijzigen

1. Log in om de [Azure portal](https://portal.azure.com).

2. Ga naar uw account voor de opslag, selecteert u alle Resources en selecteer vervolgens uw account voor opslag.

3. Klik op **configuratie** te bekijken en/of de accountconfiguratie wijzigen in de instellingen-blade.

4. Selecteer de juiste opslag laag voor uw behoeften: de **toegang tot laag** ingesteld op **koel** of **warm**.

5. Klik op opslaan op de bovenkant van het blad.

> [AZURE.NOTE] De opslag laag wijzigen, kan dit leiden tot extra kosten. Zie de sectie [voor prijzen en facturering](storage-blob-storage-tiers.md#pricing-and-billing) voor meer informatie.

## <a name="evaluating-and-migrating-to-blob-storage-accounts"></a>Evaluatie van en migratie naar rekeningen van Blob-opslag

Het doel van deze sectie is om een soepele overgang naar een Blob-opslag accounts met de gebruiker. Er zijn twee scenario's voor gebruiker:

- U hebt een bestaande account voor algemene opslag en voor de evaluatie van een wijziging in een Blob storage-account met de juiste opslag laag.
- U hebt besloten een Blob storage-account gebruiken of reeds hebt en wilt evalueren of u moet de laag voor de opslag van warm of koel.

In beide gevallen is de eerste order van business raming van de kosten van opslag en toegang tot de gegevens die zijn opgeslagen in een Blob-opslag account en die vergelijken met de huidige kosten.

### <a name="evaluating-blob-storage-account-tiers"></a>Evaluatie van de Blob storage account lagen

Om een schatting van de kosten van opslag en toegang tot gegevens die zijn opgeslagen in een Blob-opslag-account, moet u uw bestaande gebruikspatroon evalueren of het verwachte gebruikspatroon benaderen. In het algemeen zult u weten:

- Het verbruik van uw opslag - hoe veel gegevens worden opgeslagen en hoe wordt dit gewijzigd op maandelijkse basis?
- De toegangstijden opslag - hoeveel gegevens worden gelezen van en geschreven naar de account (met inbegrip van nieuwe gegevens)? Het aantal transacties worden gebruikt voor toegang tot gegevens en welke soorten transacties zijn ze?

#### <a name="monitoring-existing-storage-accounts"></a>Bestaande opslag accounts controleren

Als u wilt uw bestaande accounts voor opslag controleren en deze gegevens te verzamelen, kunt u gebruik van Azure opslag Analytics metrische gegevens levert voor een opslag die registratie wordt uitgevoerd.
Opslag Analytics metrics met transactie geaggregeerde statistieken en capaciteit gegevens over aanvragen om de Blob storage-service voor zowel algemene opslag rekeningen als Blob storage rekeningen kunnen worden opgeslagen.
Deze gegevens worden opgeslagen in bekende tabellen in dezelfde account voor opslag.

Voor meer informatie raadpleegt u [Opslag Analytics Metrics](https://msdn.microsoft.com/library/azure/hh343258.aspx) en [Opslag Analytics Metrics tabelschema](https://msdn.microsoft.com/library/azure/hh343264.aspx)

> [AZURE.NOTE] BLOB storage accounts worden blootgesteld de tabel service-eindpunt alleen voor het opslaan en toegang tot de metrische gegevens voor die account.

Voor het controleren van de opslag van het verbruik voor de Blob storage-service, moet u de cijfers over de capaciteit te schakelen.
Met deze ingeschakeld is capaciteit dagelijks voor Blob-service van de account van een opslag en opgenomen als de vermelding die is geschreven aan de tabel *$MetricsCapacityBlob* in dezelfde opslag account geregistreerd.

Als u wilt controleren op de data access-patroon voor de Blob storage-service, moet u het uurtarief metrics transactie op het niveau van API inschakelen.
Met deze ingeschakeld per API worden transacties samengevoegd om het uur, en geregistreerd als de vermelding die is geschreven aan de tabel *$MetricsHourPrimaryTransactionsBlob* in dezelfde account voor de opslag. De tabel *$MetricsHourSecondaryTransactionsBlob* registreert de transacties naar het secundaire eindpunt bij RA GRS opslag rekeningen.

> [AZURE.NOTE] In het geval er een algemene opslag-account die u hebt opgeslagen pagina BLOB's en virtuele machine schijven naast blok en blobgegevens toevoegen, is dit ramingsproces niet van toepassing. Dit is omdat er geen manier van opvallende capaciteit en alleen op basis van het type blob voor transactie-metrics blokkeren en BLOB's die kunnen worden gemigreerd naar een Blob-opslag account toevoegen.

Als u een goede benadering van u gegevens en de toegangstijden, wordt aangeraden u kiest een bewaartermijn voor de maatstaven die representatief is voor het normale gebruik en geëxtrapoleerd.
Eén optie is 7 dagen bewaren de metrische gegevens en de gegevens worden verzameld voor analyse aan het eind van de maand elke week.
Een andere mogelijkheid is de metrische gegevens voor de laatste 30 dagen bewaren en verzamelen en analyseren van de gegevens aan het einde van de periode van 30 dagen.

Voor meer informatie over het inschakelen van Zie het verzamelen en weergeven van gegevens, statistieken, [Azure opslag inschakelen maatstaven en metrische gegevens bekijken](storage-enable-and-view-metrics.md).

> [AZURE.NOTE] Opslaan, openen en downloaden van analytics-gegevens ook in rekening gebracht net als gewone gebruikersgegevens.

#### <a name="utilizing-usage-metrics-to-estimate-costs"></a>Gebruik Maatstelsel van een kostenraming maken gebruik van

##### <a name="storage-costs"></a>Kosten voor opslag

De meest recente vermelding in capaciteit metrics tabel *$MetricsCapacityBlob* met de rij belangrijke *'data'* ziet u de opslagcapaciteit die wordt verbruikt door de gegevens van de gebruiker.
De meest recente vermelding in capaciteit metrics tabel *$MetricsCapacityBlob* met de rij belangrijke *'analytics'* ziet u de opslagcapaciteit die wordt verbruikt door de logboeken analytics.

Deze totale capaciteit wordt gebruikt door zowel de gebruikersgegevens en analytics Logboeken (indien ingeschakeld), kunnen vervolgens worden gebruikt voor het schatten van de kosten van het opslaan van gegevens in de opslag.
Dezelfde methode kan ook worden gebruikt voor de schatting van de opslagkosten voor blok en BLOB's in algemene opslag-accounts toevoegen.

##### <a name="transaction-costs"></a>Transactiekosten

De som van de *'TotalBillableRequests'*, in alle items voor een API in de tabel transactie metrics geeft het totale aantal transacties voor die specifieke API. *bijvoorbeeld*, het totale aantal transacties in een bepaalde periode *'GetBlob'* kan worden berekend door de som van totaal aantal, te factureren aanvragen voor alle posten met de rijsleutel *' gebruiker; GetBlob'*.

Te schatten transactiekosten voor Blob storage accounts, moet u de transacties in drie groepen opsplitsen, omdat ze anders zijn geprijsd.

- Transacties, zoals *'PutBlob'*, *'PutBlock'*, *'PutBlockList'*, *'AppendBlock'*, *'ListBlobs'*, *'ListContainers'*, *'CreateContainer'*, *'SnapshotBlob'*en *'CopyBlob'*schrijven.
- Transacties zoals *'DeleteBlob'* en *'DeleteContainer'*te verwijderen.
- Andere transacties.

U moet schatten transactiekosten voor algemene opslag rekeningen, alle transacties, ongeacht de bewerking/API samen te voegen.

##### <a name="data-access-and-geo-replication-data-transfer-costs"></a>Data access- en geo-replicatie van de gegevensoverdracht kosten

Terwijl opslag analytics geen gegevens lezen van en geschreven naar een opslag, kan het ongeveer door te kijken naar de tabel transactie metrics geraamd.
De som van de *'TotalIngress'* in alle items voor een API in de tabel transactie parameters geeft de totale hoeveelheid gegevens in bytes ingress voor die bepaalde API.
Ook de som van de *'TotalEgress'* geeft aan dat het totale bedrag van de egress-gegevens, in bytes.

Om een schatting van de kosten van de toegang tot gegevens voor Blob storage accounts moet u de transacties in twee groepen onderverdelen.

- De hoeveelheid gegevens die zijn opgehaald uit de opslag-account kan worden geraamd door de som van de *'TotalEgress'* voor voornamelijk de *'GetBlob'* en *'CopyBlob'* -bewerkingen.
- De hoeveelheid gegevens die worden geschreven naar de rekening van de opslag kan worden geraamd door de som van de *'TotalIngress'* voor voornamelijk *'PutBlob'*, *'PutBlock'*, *'CopyBlob'* en *'AppendBlock'* -bewerkingen.

De kosten van gegevensoverdracht geo-replicatie voor accounts van Blob-opslag kan ook worden berekend met behulp van de schatting voor de hoeveelheid gegevens in het geval van een account GRS of RA GRS opslag geschreven.

> [AZURE.NOTE] Nemen voor een meer gedetailleerd voorbeeld over het berekenen van de kosten voor het gebruik van de opslag van warm of koel laag, bekijk de veelgestelde vragen met de titel *"wat access lagen voor warm en koud zijn en hoe bepaal ik welke een use?"* in de [Azure opslag prijzen pagina](https://azure.microsoft.com/pricing/details/storage/).

### <a name="migrating-existing-data"></a>Migratie van bestaande gegevens

Een Blob storage is speciaal voor de opslag van alleen blok en BLOB's toevoegen. Bestaande accounts voor algemene opslag, waarmee u kunt tabellen, wachtrijen, bestanden en schijven, alsmede BLOB's worden opgeslagen, kunnen niet worden geconverteerd naar Blob storage rekeningen. Als u wilt gebruiken voor de niveaus van opslag, moet u nieuwe accounts voor Blob-opslag en uw bestaande gegevens migreren naar de nieuwe accounts.
U kunt de volgende methoden voor het migreren van bestaande gegevens in Blob storage rekeningen van lokale opslagapparaten, derden cloud storage providers of uw bestaande accounts algemene opslag in Azure:

#### <a name="azcopy"></a>AzCopy

AzCopy is een opdrachtregelprogramma van Windows ontworpen voor hoge prestaties kopiëren van gegevens van en naar Azure opslag. U kunt AzCopy gebruiken om gegevens te kopiëren naar uw rekening Blob-opslag van uw bestaande accounts van opslag voor algemene doeleinden of voor het uploaden van gegevens van de opslagapparaten op ruimten in uw account Blob-opslag.

Zie de [overdracht van gegevens met het hulpprogramma AzCopy voor Command-Line](storage-use-azcopy.md)voor meer informatie.

#### <a name="data-movement-library"></a>Data verkeer Library

Azure opslag data verkeer bibliotheek voor .NET is gebaseerd op het core data verkeer framework dat AzCopy een stimulans. De bibliotheek is ontworpen voor krachtige, betrouwbare en gemakkelijke gegevensoverdrachten vergelijkbaar met AzCopy. Dit kunt u de voordelen van de functies van AzCopy in uw toepassing ingebouwde zonder te worden uitgevoerd en de controle van externe instanties van AzCopy behandelt.

Zie voor meer details, [Azure opslag Data verkeer Library for .net](https://github.com/Azure/azure-storage-net-data-movement)

#### <a name="rest-api-or-client-library"></a>REST API of de clientbibliotheek

U kunt een aangepaste toepassing uw gegevens migreren naar een Blob-opslag-account met behulp van een van de clientbibliotheken Azure of Azure opslagservices REST API. Azure opslag biedt rijke bibliotheken voor meerdere talen en platformen zoals .NET, Java, C++, Node.JS, PHP, Ruby en Python. De client libraries bieden geavanceerde mogelijkheden zoals opnieuw logica, logboekregistratie en parallelle uploads. U kunt ook rechtstreeks tegen de REST-API kan worden aangeroepen door een taal waarmee HTTP/HTTPS-verzoeken ontwikkelen.

Zie voor meer details [Aan de slag met Azure Blob-opslag](storage-dotnet-how-to-use-blobs.md).

> [AZURE.NOTE] BLOB's met behulp van client-side codering gecodeerd opgeslagen metagegevens met betrekking tot codering opgeslagen met de blob. Het is absoluut essentieel dat een kopie mechanisme ervoor zorgen moet dat de blob-metagegevens en met name de codering metagegevens, behouden blijft. Als u de BLOB's zonder deze metagegevens kopieert, de blob inhoud wordt niet opvraagbare opnieuw. Zie voor meer informatie over metagegevens met betrekking tot codering [Azure opslag client side codering](storage-client-side-encryption.md).

## <a name="faqs"></a>Veelgestelde vragen

1. **Bestaande accounts voor opslag nog beschikbaar zijn?**

    Ja, bestaande accounts voor opslag nog beschikbaar zijn en blijven ongewijzigd in de prijs of functionaliteit.  Ze hebben niet de mogelijkheid om te kiezen van een laag opslag en mogelijkheden voor trapsgewijs schakelen niet in de toekomst hebben.

2. **Waarom en wanneer moet ik beginnen met Blob storage accounts?**

    BLOB storage voor het opslaan van BLOB's zijn gespecialiseerd en kunnen we introduceren nieuwe blob-georiënteerde functies. In de toekomst zullen Blob storage accounts zijn de beste manier voor het opslaan van BLOB's, als toekomstige capaciteiten zoals hiërarchische opslag en trapsgewijs schakelen zal worden ingevoerd op basis van dit accounttype. Het is echter tot wanneer u wilt migreren op basis van uw zakelijke behoeften.

3. **Kan ik mijn bestaande opslag account omzetten naar een Blob-opslag?**

    Nr. BLOB storage is een ander soort opslag account en moet u nieuwe maken en uw gegevens migreren: Zie uitleg hierboven.

4. **Kan ik objecten opslaan in beide lagen opslag in dezelfde account?**

    Het *Access-Tier* kenmerk geeft de opslag laag op het niveau van een account is ingesteld en geldt voor alle objecten in die account. U instellen het attribuut laag niet op het objectniveau van een.

5. **Kan ik de laag van de opslag van mijn account Blob-opslag wijzigen?**

    Ja. U kunt de opslag laag wijzigen door het *Access-Tier* -kenmerk voor de account van de opslag worden. Wijzigen van de laag voor de opslag van toepassing op alle objecten die zijn opgeslagen in de rekening. Wijziging de laag opslag van warm naar koel niet eventuele toeslagen, oplopen zal tijdens het wijzigen van koel aan hot wiens rekening een per GB kosten voor het lezen van de gegevens in de account.

6. **Hoe vaak kan ik de laag van de opslag van mijn account Blob-opslag wijzigen?**

    Hoewel we niet dat een beperking afdwingen op hoe vaak de opslag laag kan worden gewijzigd, moet je erop letten dat de opslag laag wijzigen van koel aan hot wordt aanzienlijke kosten oplopen. Vaak wijzigen van de opslag laag wordt niet aanbevolen.

7. **Wordt de BLOB's in de koele opslag laag zich anders gedragen dan de in de opslag van warm laag?**

    BLOB's in de laag hot opslag hebben de dezelfde vertraging als BLOB's in algemene opslag rekeningen. BLOB's in de koele opslag laag hebben een soortgelijke vertraging (in milliseconden) als BLOB's in algemene opslag rekeningen.

    BLOB's in de koele opslag laag hebben een iets lagere beschikbaarheid serviceniveau (SLA) dan de BLOB's opgeslagen in de opslag van warm laag. Zie voor meer details, [SLA voor opslag](https://azure.microsoft.com/support/legal/sla/storage).

8. **Kan ik pagina BLOB's en schijven van de virtuele machine opslaan in Blob storage accounts?**

    BLOB storage accounts ondersteunen alleen blokkeren en BLOB's en BLOB geen pagina's toevoegen. Azure VM schijven worden ondersteund door de pagina BLOB's en daardoor Blob storage accounts kunnen niet worden gebruikt voor het opslaan van de schijven van de virtuele machine. Het is echter mogelijk back-ups van de virtuele machine schijven opgeslagen als BLOB's blokkeren in een Blob storage-account.

9. **Moet ik mijn bestaande toepassingen kunnen gebruikmaken van Blob storage accounts wijzigen?**

    BLOB storage accounts zijn 100%-API die consistent zijn met algemene opslag rekeningen voor het blok en BLOB's toevoegen. Als uw toepassing met behulp van BLOB's blokkeren of BLOB's, toe te voegen en u de versie 2014-02-14 van de [Opslag Services REST API](https://msdn.microsoft.com/library/azure/dd894041.aspx) of hoger is uw toepassing moet werken. Als u een oudere versie van het protocol gebruikt, moet u uw toepassing aan de nieuwe versie om probleemloos met beide soorten opslag accounts bijwerken. In het algemeen gebruiken we altijd aangeraden de laatste versie, ongeacht welke opslag account u typt.

10. **Is er een wijziging in de gebruikerservaring?**

    BLOB storage-accounts is vergelijkbaar met een algemene opslag rekeningen voor het opslaan van blok BLOB's toevoegen en ondersteuning van de belangrijkste functies van Azure opslag, met inbegrip van hoge duurzaamheid en beschikbaarheid, schaalbaarheid, prestaties en beveiliging. Uitzondering van de functies en beperkingen voor Blob storage accounts en de opslag lagen die zijn genoemd boven al het andere blijft hetzelfde.

## <a name="next-steps"></a>Volgende stappen

### <a name="evaluate-blob-storage-accounts"></a>Blob storage accounts evalueren

[Controleer de beschikbaarheid van Blob storage accounts per regio](https://azure.microsoft.com/regions/#services)

[Gebruik van uw huidige opslag rekeningen doordat Azure opslag metrics evalueren](storage-enable-and-view-metrics.md)

[Controleer de Blob-opslag prijzen per regio](https://azure.microsoft.com/pricing/details/storage/)

[Selectievakje gegevensoverdracht prijzen](https://azure.microsoft.com/pricing/details/data-transfers/)

### <a name="start-using-blob-storage-accounts"></a>Blob storage accounts gebruiken

[Aan de slag met Azure Blob-opslag](storage-dotnet-how-to-use-blobs.md)

[Verplaatsen van gegevens naar en van Azure opslag](storage-moving-data.md)

[Overdracht van gegevens met het hulpprogramma AzCopy voor opdrachtregel](storage-use-azcopy.md)

[Bladeren en het verkennen van uw opslag rekeningen](http://storageexplorer.com/)
