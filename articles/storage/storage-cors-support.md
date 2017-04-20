<properties
    pageTitle="Ondersteuning voor cross-oorsprong bron delen (CORS) | Microsoft Azure"
    description="Informatie over CORS-ondersteuning inschakelen voor de Microsoft Azure Storage-Services."
    services="storage"
    documentationCenter=".net"
    authors="cbrooks"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/07/2016"
    ms.author="cbrooks"/>

# <a name="cross-origin-resource-sharing-cors-support-for-the-azure-storage-services"></a>Cross-oorsprong Resource Sharing (CORS) ondersteuning voor de van Azure opslagservices

Vanaf versie 2013-08-15 ondersteunen de Azure opslagservices Cross oorsprong Resource Sharing (CORS) voor de Blob, tabel, wachtrij en bestand services. CORS is een HTTP-functie waarmee u een webtoepassing in een domein met toegang tot bronnen in een ander domein. Webbrowsers implementeren een beveiligingsbeperking bekend als [dezelfde oorsprong beleid](http://www.w3.org/Security/wiki/Same_Origin_Policy) waarmee wordt voorkomen een webpagina vanuit de oproepende API's in een ander domein dat; CORS biedt een veilige manier om een domein (het domein van oorsprong) API's aanroepen in een ander domein. Zie de [specificatie CORS](http://www.w3.org/TR/cors/) voor meer informatie over CORS.

U kunt instellen CORS regels afzonderlijk voor elk van de storage-services door het aanroepen van [Blob Service-eigenschappen instellen](https://msdn.microsoft.com/library/hh452235.aspx), [Wachtrijeigenschappen Service instellen](https://msdn.microsoft.com/library/hh452232.aspx)en [Tabel Service-eigenschappen instellen](https://msdn.microsoft.com/library/hh452240.aspx). Nadat u de CORS regels voor de service instellen, wordt een correct geverifieerde aanvraag ten opzichte van de service van een ander domein worden geëvalueerd om te bepalen of dit is toegestaan volgens de regels die u hebt opgegeven.

>[AZURE.NOTE] Opmerking CORS is niet een verificatiemechanisme. Ieder verzoek tegen een resource opslag als CORS is ingeschakeld, moet een handtekening geverifieerd, of tegen een openbare bron moet worden ingesteld.

## <a name="understanding-cors-requests"></a>Wat is CORS aanvragen?

Een verzoek CORS van een domein van oorsprong kan bestaan uit twee afzonderlijke aanvragen:

- Een Preflight-aanvraag, die de CORS beperkingen opgelegd door de service. De Preflight-aanvraag is vereist, tenzij de aanvraag een [eenvoudige methode is](http://www.w3.org/TR/cors/), dus GET, HEAD of POST.

- De werkelijke aanvraag ten opzichte van de gewenste resource.

### <a name="preflight-request"></a>Preflight-aanvraag

De Preflight-aanvraag query's de CORS beperkingen die zijn vastgesteld voor de storage-service door de eigenaar van de account. De webbrowser (of een andere gebruikersagent) stuurt een OPTIONS-verzoek met de aanvraagheaders, methode en de oorsprong domein. De storage-service wordt geëvalueerd op basis van vooraf geconfigureerde regels opgeven welke domeinen oorsprong, aanvraagmethoden en aanvraagheaders mag worden opgegeven voor een werkelijke verzoek aan een resource opslag voor CORS de bewerking.

CORS voor de service is ingeschakeld en er is een CORS regel die overeenkomt met de Preflight-aanvraag, de service reageert met statuscode 200 (OK) als de vereiste Access Control-headers bevat in het antwoord.

Als CORS is niet ingeschakeld voor de service of geen CORS regel komt overeen met de Preflight-aanvraag, reageert de service met statuscode 403 (niet toegestaan).

Als het OPTIONS-verzoek niet de vereiste CORS-headers (de oorsprong en Access Control-aanvraag methode headers) bevat, reageert de service met statuscode 400 (onjuiste aanvraag).

Houd er rekening mee dat een Preflight-aanvraag geëvalueerd tegen de service (Blob, wachtrij en tabel) en niet de gewenste bron. CORS als onderdeel van de eigenschappen van een service voor het verzoek om te slagen moet de eigenaar van de account ingeschakeld.

### <a name="actual-request"></a>Werkelijke aanvraag

Zodra de Preflight-aanvraag wordt geaccepteerd en het antwoord wordt geretourneerd, zorgt ervoor dat de browser is de werkelijke aanvraag bij de opslagbron. De browser weigert de werkelijke aanvraag onmiddellijk als de Preflight-aanvraag is afgewezen.

De werkelijke aanvraag wordt behandeld als een normale aanvraag ten opzichte van de storage-service. De aanwezigheid van de koptekst van de oorsprong geeft aan dat de aanvraag een aanvraag voor een CORS en de overeenkomende CORS regels wordt gecontroleerd door de service. Als een overeenkomst wordt gevonden, worden de headers Access Control toegevoegd aan het antwoord en terug naar de client verzonden. Als een overeenkomst wordt gevonden, worden de headers CORS toegangsbeheer niet geretourneerd.

## <a name="enabling-cors-for-the-azure-storage-services"></a>CORS voor de opslag van Azure services inschakelen

CORS regels zijn ingesteld op het serviceniveau van dat u wilt in- of uitschakelen van CORS voor elke service (Blob, wachtrij en tabel) afzonderlijk. CORS is standaard uitgeschakeld voor elke service. Om te schakelen van CORS, moet u de juiste service-eigenschappen met versie 2013-08-15 of hoger, en CORS regels toevoegen aan de eigenschappen van de service. Raadpleeg [Blob Service-eigenschappen instellen](https://msdn.microsoft.com/library/hh452235.aspx), [Wachtrijeigenschappen Service instellen](https://msdn.microsoft.com/library/hh452232.aspx)en [Tabel Service-eigenschappen instellen](https://msdn.microsoft.com/library/hh452240.aspx)voor meer informatie over het inschakelen of uitschakelen van CORS voor een service en het CORS regels in te stellen.

Hier volgt een voorbeeld van een enkele regel CORS, opgegeven via een bewerking voor Service-eigenschappen instellen:

    <Cors>    
        <CorsRule>
            <AllowedOrigins>http://www.contoso.com, http://www.fabrikam.com</AllowedOrigins>
            <AllowedMethods>PUT,GET</AllowedMethods>
            <AllowedHeaders>x-ms-meta-data*,x-ms-meta-target*,x-ms-meta-abc</AllowedHeaders>
            <ExposedHeaders>x-ms-meta-*</ExposedHeaders>
            <MaxAgeInSeconds>200</MaxAgeInSeconds>
        </CorsRule>
    <Cors>

Elk element is opgenomen in de regel CORS wordt hieronder beschreven:

- **AllowedOrigins**: de oorsprong domeinen die een verzoek ten opzichte van de storage-service via CORS zijn toegestaan. Het oorspronkelijke domein is het domein waaruit de aanvraag afkomstig is. Zorg ervoor dat de oorsprong hoofdlettergevoelig exact overeen met de oorsprong die de leeftijd van de gebruiker naar de service verzendt. U kunt ook het jokerteken ' *' om alle domeinen van oorsprong aanvragen via CORS. In het bovenstaande voorbeeld de domeinen [http://www.contoso.com](http://www.contoso.com) en [http://www.fabrikam.com](http://www.fabrikam.com) kunt u aanvragen tegen de service met behulp van CORS.

- **AllowedMethods**: de methoden die het oorspronkelijke domein voor een aanvraag van CORS gebruiken kan (HTTP-verzoek woorden). In het bovenstaande voorbeeld zijn alleen PUT en GET-aanvragen toegestaan.

- **AllowedHeaders**: de aanvraagheaders die het domein van oorsprong kan op verzoek CORS opgeeft. In het bovenstaande voorbeeld worden alle metagegevens headers beginnend met x-ms-metagegevens, x-ms-meta-doel en x-ms-meta-abc zijn toegestaan. Houd er rekening mee dat het jokerteken ' *' geeft aan dat een kop die begint met het opgegeven voorvoegsel is toegestaan.

- **ExposedHeaders**: de reactie headers die mogelijk in het antwoord op de aanvraag CORS verzonden en weergegeven door de browser naar de uitgever van de aanvraag. In het bovenstaande voorbeeld de browser is volgens de instructies om de koptekst die beginnen met x-ms-meta beschikbaar te maken.

- **MaxAgeInSeconds**: de maximale tijd dat een browser cache plaatst de Preflight-opties aanvragen.

De van Azure opslagservices ondersteuning voorvoegsels kopteksten voor elementen van de **AllowedHeaders** en de **ExposedHeaders** op te geven. Als u wilt toestaan dat een categorie van koppen, kunt u een algemeen voorvoegsel aan die categorie. Bijvoorbeeld, geven de *x-ms-meta** als u een koptekst voorvoegsels maakt een regel die overeenkomen met alle headers die met x-ms-meta beginnen.

CORS voorschriften gelden de volgende beperkingen:

- U kunt maximaal vijf regels van CORS per storage-service (Blob-, tabel- en wachtrij).
- De maximale grootte van alle CORS regelinstellingen op verzoek, met uitzondering van de XML-codes, mag niet hoger zijn dan 2 KB.
- De lengte van een toegestane koptekst, blootgestelde koptekst of oorsprong toegestane moet niet meer dan 256 tekens.
- Toegestane kopteksten en blootgestelde headers kunnen zijn:
  - Letterlijke headers, waarbij de exacte header-naam wordt geleverd, zoals **x-ms-meta-verwerkt**. Een maximum van 64 letterlijke headers kan op verzoek worden opgegeven.
  - Headers, waarbij een voorvoegsel van de kop wordt geleverd, zoals **x-ms-metagegevens**voorafgegaan *. Een voorvoegsel op te geven op deze manier kunt of heeft een header die met het opgegeven voorvoegsel begint. Een maximum van twee voorvoegsels headers kan op verzoek worden opgegeven.
- De methoden (of de HTTP-woorden) opgegeven in het element **AllowedMethods** moeten voldoen aan de methoden die worden ondersteund door de service Azure storage API's. Ondersteunde methoden zijn verwijderen, GET, HEAD, samenvoegen, boeken, opties en PUT.

## <a name="understanding-cors-rule-evaluation-logic"></a>Wat zijn CORS regel evaluatie logica

Wanneer een storage-service een Preflight- of werkelijke aanvraag ontvangt, wordt het als die aanvraag op basis van de CORS regels die u hebt ingesteld voor de service via de juiste bewerking voor Service-eigenschappen instellen. CORS regels worden geëvalueerd in de volgorde waarin ze zijn ingesteld in het hoofdgedeelte van de aanvraag van de bewerking voor de Service-eigenschappen instellen.

CORS regels zijn als volgt beoordeeld:

1. Het oorspronkelijke domein van het verzoek wordt eerst vergeleken met de domeinen vermeld voor het element **AllowedOrigins** . Als het domein van oorsprong in de lijst is opgenomen, of alle domeinen zijn toegestaan met het jokerteken ' *', vervolgens opbrengst evaluatie van regels. Als het domein van oorsprong niet opgenomen is, mislukt de aanvraag.

2. De methode of HTTP-woord van de aanvraag wordt vervolgens vergeleken met de methoden die in het element **AllowedMethods** . Als de methode in de lijst is opgenomen, gaat regels evaluatie; Als dit niet het geval is, mislukt de aanvraag.

3. Als de aanvraag overeenkomt met een regel in het domein van oorsprong en de methode, wordt die regel geselecteerd proces die het verzoek en geen nadere voorschriften worden geëvalueerd. Voordat de aanvraag slagen kan, maar eventuele headers die in de aanvraag vermeld vergeleken met de headers die worden vermeld in het element **AllowedHeaders** . Als de headers verzonden de toegestane headers niet overeenkomen, mislukt de aanvraag.

Aangezien de regels worden verwerkt in de volgorde waarin dat ze aanwezig zijn in het hoofdgedeelte van de aanvraag, wordt aanbevolen dat u de meest beperkende regels voor de oorsprongen eerst in de lijst opgeven, zodat deze als eerste geëvalueerd. Regels die minder restrictief zijn – bijvoorbeeld een regel voor het toestaan van alle oorsprongen – aan het einde van de lijst opgeven.

### <a name="example--cors-rules-evaluation"></a>Voorbeeld – CORS evaluatie van regels

In het volgende voorbeeld wordt een instantie gedeeltelijke aanvraag voor een bewerking CORS regels instellen voor de storage-services. Zie [Blob Service-eigenschappen instellen](https://msdn.microsoft.com/library/hh452235.aspx), [Wachtrijeigenschappen Service instellen](https://msdn.microsoft.com/library/hh452232.aspx)en [Tabel Service-eigenschappen instellen](https://msdn.microsoft.com/library/hh452240.aspx) voor meer informatie over het maken van de aanvraag.

    <Cors>
        <CorsRule>
            <AllowedOrigins>http://www.contoso.com</AllowedOrigins>
            <AllowedMethods>PUT,HEAD</AllowedMethods>
            <MaxAgeInSeconds>5</MaxAgeInSeconds>
            <ExposedHeaders>x-ms-*</ExposedHeaders>
            <AllowedHeaders>x-ms-blob-content-type, x-ms-blob-content-disposition</AllowedHeaders>
        </CorsRule>
        <CorsRule>
            <AllowedOrigins>*</AllowedOrigins>
            <AllowedMethods>PUT,GET</AllowedMethods>
            <MaxAgeInSeconds>5</MaxAgeInSeconds>
            <ExposedHeaders>x-ms-*</ExposedHeaders>
            <AllowedHeaders>x-ms-blob-content-type, x-ms-blob-content-disposition</AllowedHeaders>
        </CorsRule>
        <CorsRule>
            <AllowedOrigins>http://www.contoso.com</AllowedOrigins>
            <AllowedMethods>GET</AllowedMethods>
            <MaxAgeInSeconds>5</MaxAgeInSeconds>
            <ExposedHeaders>x-ms-*</ExposedHeaders>
            <AllowedHeaders>x-ms-client-request-id</AllowedHeaders>
        </CorsRule>
    </Cors>

Vervolgens kunt u de volgende CORS aanvragen:

Aanvraag||| Reactie||
---|---|---|---|---
**Methode** |**Oorsprong** |**Aanvraagheaders** |**Regel vergelijken** |**Resultaat**
**PLAATSEN** | http://www.contoso.com |x-ms-blob-content-type | Eerste regel |Succes
**Toevoegen** | http://www.contoso.com| x-ms-blob-content-type | Tweede regel |Succes
**Toevoegen** | http://www.contoso.com| x-ms-blob-content-type | Tweede regel | Storing

De eerste aanvraag overeenkomt met de eerste regel – het oorspronkelijke domein overeenkomt met de toegestane oorsprongen, de methode komt overeen met de toegestane methoden en de header komt overeen met de toegestane headers – en dus slaagt.

Het tweede verzoek komt niet overeen met de eerste regel, omdat de methode komt niet overeen met de toegestane methoden. Komt, echter overeen met de tweede regel, zodat deze kan worden uitgevoerd.

De tweede regel in het domein van oorsprong en de methode komt overeen met het derde verzoek dus geen nadere voorschriften worden geëvalueerd. De *header x-ms-client-aanvraag-id* is echter niet toegestaan door de tweede regel zodat het verzoek niet, ondanks het feit dat de semantiek van de derde regel zouden hebben gemaakt om te slagen.

>[AZURE.NOTE] Hoewel in dit voorbeeld ziet u een minder beperkende regel vóór een meer beperkend, is in het algemeen de beste manier om eerst een lijst met de strengste voorschriften.

## <a name="understanding-how-the-vary-header-is-set"></a>Begrijpen hoe de variëren-header is ingesteld

De kop *variëren* is een standaard HTTP/1.1-header die bestaat uit een verzameling request header-velden die in de browser of de gebruiker-agent over de criteria die zijn geselecteerd door de server voor het verwerken van de aanvraag in kennis. De kop *variëren* wordt voornamelijk gebruikt voor caching proxy's, browsers en CDN die gebruiken om te bepalen hoe het antwoord mag worden opgeslagen. Voor meer informatie, Zie de specificatie voor de [kop zijn verschillend](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).

Wanneer de browser of een andere gebruikersagent het antwoord van een aanvraag van CORS slaat, het domein van oorsprong is in de cache opgeslagen als de toegestane oorsprong. Wanneer een tweede domein hetzelfde verzoek voor een resource opslag terwijl de cache actief is, haalt de gebruikersagent het domein van oorsprong uit de cache. Het tweede domein komt niet overeen met het domein in de cache opgeslagen, zodat het verzoek mislukt wanneer het anders zou slagen. In bepaalde gevallen stelt Azure opslag de kop variëren naar **oorsprong** als instructie voor de gebruikersagent het volgende CORS-aanvraag naar de service verzenden wanneer het desbetreffende domein van de oorsprong in de cache verschilt.

Azure opslag stelt de kop *variëren* op **oorsprong** voor werkelijke GET/HEAD-aanvragen in de volgende gevallen:

- Wanneer de oorsprong van de aanvraag komt exact overeen met de toegestane oorsprong gedefinieerd door een regel CORS. Als u een exacte overeenkomst, de regel CORS mag niet inhouden dat een jokerteken ' * ' teken.

- Er is geen regel die overeenkomt met de oorsprong van de aanvraag, maar CORS voor de storage-service is ingeschakeld.

In het geval waarbij een GET/HEAD-verzoek overeenkomt met een regel CORS waarmee alle oorsprongen, de reactie geeft aan dat alle oorsprongen zijn toegestaan, de cache van de gebruiker-agent kan de volgende aanvragen uit elk willekeurig domein van oorsprong terwijl de cache actief is.

Houd er rekening mee dat voor aanvragen met behulp van andere methoden dan GET/hoofd, de storage-services wordt niet ingesteld de kop variëren, aangezien de antwoorden op deze methoden niet in de cache door gebruikersagenten opgeslagen.

De volgende tabel wordt aangegeven hoe Azure opslag zal reageren op GET/HEAD-aanvragen op basis van de eerder genoemde gevallen:

Aanvraag|Instellingen van een account en het resultaat van de evaluatie van de regels|||Reactie|||
---|---|---|---|---|---|---|---|---
**Koptekst van oorsprong aanwezig op aanvraag** | **CORS (s) voor deze service gespecificeerde** | **Overeenkomende regel bestaat waarmee alle oorsprongen (*)** | **Overeenkomende regel bestaat voor de exacte oorsprong vergelijken** | **Antwoord bevat variëren-header ingesteld op oorsprong** | **Antwoord bestaat uit Access Control-toegestaan-oorsprong: "*"** | **Antwoord bestaat uit Access-besturingselement-blootgesteld-Headers**
Nee|Nee|Nee|Nee|Nee|Nee|Nee
Nee|Ja|Nee|Nee|Ja|Nee|Nee
Nee|Ja|Ja|Nee|Nee|Ja|Ja
Ja|Nee|Nee|Nee|Nee|Nee|Nee
Ja|Ja|Nee|Ja|Ja|Nee|Ja
Ja|Ja|Nee|Nee|Ja|Nee|Nee
Ja|Ja|Ja|Nee|Nee|Ja|Ja


## <a name="billing-for-cors-requests"></a>Facturering van CORS aanvragen

Preflight-aanvragen worden in rekening gebracht als u CORS voor een van de diensten van de opslag voor uw account hebt ingeschakeld (door het aanroepen van [Blob Service-eigenschappen instellen](https://msdn.microsoft.com/library/hh452235.aspx), [Wachtrij Service-eigenschappen instellen](https://msdn.microsoft.com/library/hh452232.aspx)of [Tabel Service-eigenschappen instellen](https://msdn.microsoft.com/library/hh452240.aspx)). Om te minimaliseren kosten, kunt u het element **MaxAgeInSeconds** in CORS regels op een hoge waarde instellen zodat de gebruikersagent de aanvraag in de cache opgeslagen.

Mislukte aanvragen voor Preflight-zal niet worden gefactureerd.

## <a name="next-steps"></a>Volgende stappen

[Blob-Service-eigenschappen instellen](https://msdn.microsoft.com/library/hh452235.aspx)

[Service-eigenschappen voor wachtrijen instellen](https://msdn.microsoft.com/library/hh452232.aspx)

[Tabel Service-eigenschappen instellen](https://msdn.microsoft.com/library/hh452240.aspx)

[W3C Cross oorsprong resources delen specificatie](http://www.w3.org/TR/cors/)
