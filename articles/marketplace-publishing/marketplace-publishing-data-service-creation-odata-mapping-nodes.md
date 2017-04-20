<properties
   pageTitle="Handleiding voor het maken van een Service voor de markt | Microsoft Azure"
   description="Gedetailleerde instructies over het maken, certificeren en implementeren van een Service voor gegevens kopen op de markt Azure."
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

   <tags
      ms.service="marketplace"
      ms.devlang="na"
      ms.topic="article"
      ms.tgt_pltfrm="na"
      ms.workload="na"
      ms.date="08/26/2016"
      ms.author="hascipio; avikova" />

# <a name="understanding-the-nodes-schema-for-mapping-an-existing-web-service-to-odata-through-csdl"></a>Informatie over het schema van de knooppunten voor het toewijzen van een bestaand webservice aan OData via CSDL

>[AZURE.IMPORTANT] **Op dit moment we zijn niet langer onboarding alle nieuwe Data Service uitgevers. Nieuwe dataservices zal niet krijgen goedgekeurd voor de aanbieding.** Als u beschikt over een SaaS zakelijke toepassing die u wilt publiceren op AppSource vindt u meer informatie [hier](https://appsource.microsoft.com/partners). Als u een IaaS toepassingen of developer service die u wilt publiceren op Azure Marketplace meer informatie vindt [hier](https://azure.microsoft.com/marketplace/programs/certified/).

Dit document helpt de structuur van het knooppunt voor het toewijzen van een OData-protocol om CSDL te verduidelijken. Het is belangrijk te weten dat het knooppunt structuur goed is opgemaakte XML-codes. Hoofdmap, bovenliggende en onderliggende schema kan worden gebruikt bij het ontwerpen van de OData-toewijzing.

## <a name="ignored-elements"></a>Genegeerde elementen
Hieronder vindt een hoog niveau CSDL elementen (XML-knooppunten) die zullen worden gebruikt door de backend Azure Marketplace tijdens het importeren van metagegevens voor de webservice. Zij kunnen aanwezig zijn, maar worden genegeerd.

| Element | Toepassingsgebied |
|----|----|
| Met behulp van Element | Het knooppunt, subknooppunten en alle kenmerken |
| Documentatie-Element | Het knooppunt, subknooppunten en alle kenmerken |
| ComplexType | Het knooppunt, subknooppunten en alle kenmerken |
| Element van de associatie | Het knooppunt, subknooppunten en alle kenmerken |
| Uitgebreide eigenschappen | Het knooppunt, subknooppunten en alle kenmerken |
| EntityContainer | Alleen de volgende kenmerken worden genegeerd: *uitgebreid* en *AssociationSet* |
| Schema | Alleen de volgende kenmerken worden genegeerd: *Namespace* |
| FunctionImport | Alleen de volgende kenmerken worden genegeerd: *modus* (de standaardwaarde van een RG wordt gebruikt) |
| EntityType | Alleen de volgende subknooppunten worden genegeerd: *sleutel* - en *PropertyRef* |

De volgende beschrijving van de wijzigingen (toegevoegd en elementen genegeerd) naar de verschillende CSDL XML-knooppunten in detail.

## <a name="functionimport-node"></a>Knooppunt FunctionImport
Een FunctionImport-knooppunt vertegenwoordigt één URL (toegangspunt) die een service aan de eindgebruiker beschikbaar worden gesteld. Het knooppunt kunt beschrijven hoe de URL is gericht, welke parameters zijn beschikbaar voor de eindgebruiker en hoe deze parameters worden geleverd.

Meer informatie over dit knooppunt zijn gevonden op [hier] [MSDNFunctionImportLink]

[MSDNFunctionImportLink]: (https://msdn.microsoft.com/library/cc716710 (v=vs.100).aspx)

Hieronder vindt u de extra kenmerken (of toevoegingen aan kenmerken) die door het knooppunt FunctionImport worden blootgesteld:

**d:BaseUri** -
de URI-sjabloon voor de REST resource die beschikbaar zijn voor de markt. Marketplace wordt de sjabloon gebruikt om query's op de webservice REST samen te stellen. De URI-sjabloon bevat tijdelijke aanduidingen voor de parameters in de vorm van {parameterName}, waarbij parameterName de naam van de parameter is. Excl. apiVersion = {apiVersion}.
Parameters zijn toegestaan als URI parameters of als onderdeel van het pad van de URI weergegeven. Voor de weergave in het pad zijn ze altijd verplicht (kan niet worden gemarkeerd als null-waarden). *Voorbeeld:*`d:BaseUri="http://api.MyWeb.com/Site/{url}/v1/visits?start={start}&amp;end={end}&amp;ApiKey=3fadcaa&amp;Format=XML"`

**Name** - de naam van de geïmporteerde functie.  Hetzelfde kan niet zijn als andere gedefinieerde namen in de CSDL.  Excl. Name = "GetModelUsageFile"

**EntitySet** *(optioneel)* - als de functie een collectie van de Entiteitstypen retourneert, de waarde van de **EntitySet** moet de entiteit die is ingesteld op de collectie bij hoort. In alle andere gevallen moet het kenmerk **EntitySet** niet worden gebruikt. *Voorbeeld:*`EntitySet="GetUsageStatisticsEntitySet"`

**Retourtype** *(Optioneel)* - geeft het type van de elementen die worden geretourneerd door de URI.  Gebruik dit kenmerk niet als de functie geen waarde retourneert. Hieronder volgen de ondersteunde typen:

 - **Collectie (<Entity type name>)**: Hiermee geeft u een collectie van de Entiteitstypen gedefinieerd. De naam is in het kenmerk Name van het knooppunt EntityType aanwezig. Een voorbeeld is de verzameling (WXC. HourlyResult).
 - **Raw (<mime type>)**: Hiermee geeft u een ruwe document/blob die aan de gebruiker wordt geretourneerd. Een voorbeeld is Raw(image/jpeg) andere voorbeelden:

  - ReturnType="Raw(text/plain)"
  - ReturnType = "Collection (sage. DeleteAllUsageFilesEntity) ' *

**d:Paging** - geeft aan hoe het wisselbestand wordt verwerkt door de resource voor de REST. De parameterwaarden worden gebruikt binnen accolades braches, bijvoorbeeld pagina {$page} = & itemsperpage = {$size} de beschikbare opties zijn:

- **Geen:** geen paginering is beschikbaar
- **Overslaan:** paginering wordt uitgedrukt door middel van een logische "overslaan" en "nemen" (boven). Overslaan vroom M elementen en nemen vervolgens retourneert de volgende N-elementen. Parameterwaarde: $skip
- **Nemen:** Deze eigenschap retourneert de volgende N elementen in beslag nemen. Parameterwaarde: $take
- **PageSize:** paginering wordt uitgedrukt door middel van een logische pagina en de grootte (items per pagina). Pagina staat voor de huidige pagina die wordt geretourneerd. Parameterwaarde: $page
- **Grootte:** grootte staat voor het aantal artikelen dat is teruggezonden voor elke pagina. Parameterwaarde: $size

**d:AllowedHttpMethods** *(Optioneel)* - Hiermee geeft u op welke bewerking wordt afgehandeld door de bron van de REST. Bovendien beperkt aanvaarde opdracht aan de opgegeven waarde.  Standaard = boeken.  *Voorbeeld:* `d:AllowedHttpMethods="GET"` De beschikbare opties zijn:

- **Ophalen:** meestal wordt gebruikt om gegevens te retourneren
- **Boeken:** meestal gebruikt voor het invoegen van nieuwe gegevens
- **Plaatsen:** meestal gebruikt voor het bijwerken van gegevens
- **Verwijderen:** gebruikt om gegevens te verwijderen

Extra onderliggende knooppunten (niet gedekt door de documentatie van de CSDL) in het knooppunt FunctionImport zijn:

**d:RequestBody** *(Optioneel)* - het hoofdgedeelte van de aanvraag wordt gebruikt om aan te geven dat de aanvraag een instantie verwacht moet worden verzonden. Parameters kunnen in het hoofdgedeelte van de aanvraag krijgen. Ze zijn uitgedrukt binnen accolades, bijvoorbeeld {parameterName}. Deze parameters worden toegewezen van de invoer van de gebruiker in het lichaam dat wordt overgebracht naar de provider-service. Het element requestBody is een kenmerk met de naam httpMethod. Het kenmerk kunt twee waarden:

- **Boeken:** Als de aanvraag een HTTP POST is gebruikt
- **Ophalen:** Als de aanvraag een HTTP-GET is gebruikt

    Voorbeeld:

        `<d:RequestBody d:httpMethod="POST">
        <![CDATA[
        <req1:Request xmlns:r1="http://schemas.mysite.com//generic/requests/1" Version="1.0">
        <req1:Query>{Query}</req1:Query><req1:AppId>D453474</req1:AppId>
        <req:DestinationSchemas><req1:Schema>Generic.RequestResponse[1.0]</req1:Schema>
        </req1:DestinationSchemas>
        </req1: Request>
        ]]>
        </d:RequestBody>`

**d:Namespaces** en **d:Namespace** - hier dit knooppunt de naamruimten die zijn gedefinieerd in het XML-bestand dat wordt geretourneerd door de functie import (URI eindpunt) wordt beschreven. De XML die wordt geretourneerd door de backend-service bevat mogelijk een aantal naamruimten te onderscheiden van de inhoud die wordt geretourneerd. **Alle deze naamruimten als gebruikt in d:Map of d:Match XPath-query's moeten worden weergegeven.** Het knooppunt d:Namespaces bevat een setlijst van d:Namespace-knooppunten. Elk van deze lijsten een naamruimte die wordt gebruikt in de backend-service. Het kenmerk van het knooppunt d:Namespace zijn:

-   **d:Prefix:** Het voorvoegsel van de naamruimte, zoals in de XML-resultaten van de service, zoals f:FirstName, f:LastName, waarbij f het voorvoegsel is.
- **d:Uri:** De volledige URI van de naamruimte die wordt gebruikt in het document van het resultaat. Hiermee geeft u de waarde die het voorvoegsel wordt omgezet naar tijdens runtime.

**d:ErrorHandling** *(Optioneel)* - dit knooppunt bevat de voorwaarden voor het afhandelen van fouten. Elk van de voorwaarden wordt gevalideerd tegen het resultaat dat wordt geretourneerd door de provider-service. Als u een voorwaarde komt overeen met het voorgestelde HTTP-foutcode dat een foutbericht wordt geretourneerd aan de eindgebruiker.

**d:ErrorHandling** *(Optioneel)* en **d:Condition** bevat *(optioneel)* - een knooppunt voorwaarde een voorwaarde die is geselecteerd in het resultaat dat wordt geretourneerd door de provider-service. De **vereiste** kenmerken zijn:

- **d:Match:** Een XPath-expressie die wordt gecontroleerd of een bepaalde knooppuntwaarde aanwezig in de inhoudsprovider uitvoer XML is. Het XPath wordt uitgevoerd aan de uitvoer en moet geeft true als resultaat als de voorwaarde een overeenkomst of false anders.
- **d:HttpStatusCode:** De HTTP-statuscode op die moet worden geretourneerd door de markt in het geval de conditie overeenkomt. Marktplaats signalizes fouten via HTTP-statuscodes. Een lijst met HTTP-statuscodes zijn beschikbaar op http://en.wikipedia.org/wiki/HTTP_status_code
- **d:ErrorMessage:** Het foutbericht dat wordt teruggestuurd – met de HTTP-statuscode – naar de eindgebruiker. Dit moet een aangepaste foutbericht geen geheimen bevat.

**d:title** *(Optioneel)* - kunnen met een beschrijving van de titel van de functie. De waarde voor de titel is afkomstig van

- Optionele map kenmerk (een xpath) waarin wordt aangegeven waar de titel in het antwoord van de serviceaanvraag.
- - Of - de naam is opgegeven als de waarde van het knooppunt.

**d:Rights** *(Optioneel)* - (bijv. copyright) van de rechten die aan de functie zijn gekoppeld. De waarde van de rechten is afkomstig van:

- De optionele map-kenmerk (een xpath) waarin wordt aangegeven waar de rechten in het antwoord van de serviceaanvraag.
-   - Of - de rechten als de waarde van het knooppunt opgegeven.

**d:Description** *(Optioneel)* - een korte beschrijving van de functie. De waarde voor de beschrijving is afkomstig van

- Optionele map kenmerk (een xpath) waarin wordt aangegeven waar vind ik de beschrijving in het antwoord van de serviceaanvraag.
- - Of – de omschrijving die is opgegeven als de waarde van het knooppunt.

**d:EmitSelfLink** - *Zie het bovenstaande voorbeeld 'FunctionImport 'Gewisseld' door middel van gegevens opgehaald'*

**d:EncodeParameterValue** - optionele uitbreiding van OData

**d:QueryResourceCost** - optionele uitbreiding van OData

**d:Map** - optionele uitbreiding van OData

**d:Headers** - optionele uitbreiding van OData

**d:Headers** - optionele uitbreiding van OData

**d:Value** - optionele uitbreiding van OData

**d:HttpStatusCode** - optionele uitbreiding van OData

**d:ErrorMessage** - optionele uitbreiding van OData

## <a name="parameter-node"></a>Parameterknooppunt

Dit knooppunt vertegenwoordigt één parameter die wordt weergegeven als onderdeel van de sjabloon URI / instantie die is opgegeven in het knooppunt FunctionImport aanvragen.

Een pagina van het document zeer nuttige informatie over het ' Parameter ' elementknooppunt is gevonden op [hier](http://msdn.microsoft.com/library/ee473431.aspx) (Gebruik de vervolgkeuzelijst **Andere versie** naar een andere versie Selecteer zo nodig de documentatie). *Voorbeeld:*`<Parameter Name="Query" Nullable="false" Mode="In" Type="String" d:Description="Query" d:SampleValues="Rudy Duck" d:EncodeParameterValue="true" MaxLength="255" FixedLength="false" Unicode="false" annotation:StoreGeneratedPattern="Identity"/>`

| Parameter-kenmerk | Is vereist | Waarde |
|----|----|----|
| Naam | Ja | De naam van de parameter. Hoofdletters en kleine letters!  BaseUri hoofdletters. **Voorbeeld:**`<Property Name="IsDormant" Type="Byte" />` |
| Type | Ja | De parametertype. De waarde moet een **EDMSimpleType** of een complex type die binnen het toepassingsgebied van het model. Zie "6 ondersteunde eigenschap-Parameter types" voor meer informatie.  (Hoofdlettergevoelig! Eerste teken wordt met een hoofdletter, rest in kleine letters.)  Zie ook, [conceptueel Model typen (CSDL)] [MSDNParameterLink]. **Voorbeeld:**`<Property Name="LimitedPartnershipID " Type="Int32" />` |
| Modus | Nee | **In**, uit, of InOut afhankelijk van of de parameter een invoer, uitvoer of invoer/uitvoer-parameter. ("IN" is alleen beschikbaar in de markt Azure.) **Voorbeeld:**`<Parameter Name="StudentID" Mode="In" Type="Int32" />` |
| MaxLength | Nee | De maximaal toegestane lengte van de parameter. **Voorbeeld:**`<Property Name="URI" Type="String" MaxLength="100" FixedLength="false" Unicode="false" />` |
| Precisie | Nee | De precisie van de parameter. **Voorbeeld:**`<Property Name="PreviousDate" Type="DateTime" Precision="0" />` |
| Schaal | Nee | De schaal van de parameter. **Voorbeeld:**`<Property Name="SICCode" Type="Decimal" Precision="10" Scale="0" />` |

[MSDNParameterLink]: (http://msdn.microsoft.com/library/bb399548 (v=VS.100).aspx)

Dit zijn de kenmerken die zijn toegevoegd aan de CSDL-specificatie:

| Parameter-kenmerk | Beschrijving |
|----|----|
| **d:Regex** *(Optioneel)* | Een regex-instructie die wordt gebruikt voor het valideren van de ingevoerde waarde voor de parameter. Als de ingevoerde waarde niet overeenkomt met de instructie wordt de waarde geweigerd. Hiermee geeft u ook een set van waarden, bijvoorbeeld ^ [0-9] +? $ dat alleen getallen zijn toegestaan. **Voorbeeld:** ' < parameternaam = "naam" modus = "In" Type = "Tekenreeks" d: null-waarden = "false" d:Regex = "^ [a-zA-Z] * $" d:Description = "een naam die geen spaties of niet-alfabetische niet-Engelse tekens kan bevatten" d:SampleValues = "George|John|Thomas|James "/ >" |
| **d:Enum** *(Optioneel)* | Een pijp gescheiden lijst met waarden voor de parameter. Het type van de waarden moet overeenkomen met het opgegeven type van de parameter. Voorbeeld: ' Engels|metric|RAW`. Enum will display as a selectable dropdown list of parameters in the UI (service explorer). **Example:** `< parameternaam = "Duur" Type = "Tekenreeks" modus = "null-waarden bevatten In" = "true" d:Enum = "1 jaar|5years|10years "/ >" |
| **d: null-waarden** *(Optioneel)* | U kunt definiëren of een parameter kan niet null zijn. De standaardwaarde is: true. Parameters die beschikbaar zijn als onderdeel van het pad in de URI-sjabloon kunnen echter niet null. Als het kenmerk is ingesteld op false voor deze parameters: de invoer van de gebruiker wordt overschreven. **Voorbeeld:**`<Parameter Name="BikeType" Type="String" Mode="In" Nullable="false"/>` |
| **d:SampleValue** *(Optioneel)* | Een voorbeeldwaarde weer te geven als een notitie naar de Client in de gebruikersinterface.  Het is mogelijk om meerdere waarden toevoegen met behulp van een pipe-gescheiden lijst, dat wil zeggen ' een|b|c` **Example:** `< parameternaam = "BikeOwner" Type = "Tekenreeks" Mode = "In" d:SampleValues = "George|John|Thomas|James "/ >" |

## <a name="entitytype-node"></a>Knooppunt EntityType

Dit knooppunt vertegenwoordigt een van de typen die worden geretourneerd uit de markt voor de eindgebruiker. Het bevat ook de toewijzing van de uitvoer die wordt geretourneerd door de provider-service op de waarden die worden geretourneerd aan de eindgebruiker.

Meer informatie over dit knooppunt worden gevonden [hier](http://msdn.microsoft.com/library/bb399206.aspx) (Gebruik de vervolgkeuzelijst **Andere versie** naar een andere versie Selecteer zo nodig de documentatie weergeven.)

| Naam van kenmerk | Is vereist | Waarde |
|----|----|----|
| Naam | Ja | De naam van het entiteitstype. **Voorbeeld:**`<EntityType Name="ListOfAllEntities" d:Map="//EntityModel">` |
| BaseType | Nee | De naam van een ander type van de entiteit die het basistype van het entiteitstype die wordt gedefinieerd. **Voorbeeld:**`<EntityType Name="PhoneRecord" BaseType="dqs:RequestRecord">` |

Dit zijn de kenmerken die zijn toegevoegd aan de CSDL-specificatie:

**d:Map** - tegen de uitvoer van de service wordt uitgevoerd een XPath-expressie. De aanname hier is dat de uitvoer van de service een reeks elementen die worden herhaald, bevat zoals een ATOM-feed waar er is een set van post-knooppunten die worden herhaald. Elk van deze herhalende knooppunten bevat één record. Het XPath wordt vervolgens opgegeven aan te wijzen voor de afzonderlijke herhalende knooppunt in de inhoudsprovider service resultaat waarin de waarden voor een afzonderlijke record. Voorbeeld van de uitvoer van de service:

        `<foo>
          <bar> … content … </bar>
          <bar> … content … </bar>
          <bar> … content … </bar>
        </foo>`

De XPath-expressie zou worden /foo/staaf omdat elk van de balk knooppunt het herhalende knooppunt in de uitvoer en het bevat de feitelijke inhoud die wordt geretourneerd aan de eindgebruiker.

**Sleutel** - dit kenmerk wordt genegeerd door de markt. REST webservices gebaseerd, in het algemeen geen primaire sleutel worden blootgesteld.


## <a name="property-node"></a>Eigenschapsknooppunt

Dit knooppunt bevat een eigenschap van de record.

Meer informatie over dit knooppunt zijn gevonden op [http://msdn.microsoft.com/library/bb399546.aspx](http://msdn.microsoft.com/library/bb399546.aspx) (Gebruik de vervolgkeuzelijst **Andere versie** naar een andere versie Selecteer zo nodig de documentatie weergeven.) *Voorbeeld:*
        `<EntityType Name="MetaDataEntityType" d:Map="/MyXMLPath">
        <Property Name="Name"   Type="String" Nullable="true" d:Map="./Service/Name" d:IsPrimaryKey="true" DefaultValue=”Joe Doh” MaxLength="25" FixedLength="true" />
        ...
        </EntityType>`

| Kenmerknaam | Vereist | Waarde |
|----|----|----|
| Naam | Ja | De naam van de eigenschap. |
| Type | Ja | Het type van de waarde van de eigenschap. Het waardetype moet een **EDMSimpleType** of een complex type (aangegeven door een FQDN-naam) die zich binnen het bereik van het model. Zie conceptuele Model typen (CSDL) voor meer informatie. |
| Null-waarden | Nee | **True** (de standaardwaarde) of **False** afhankelijk van of de eigenschap null-waarde kan hebben. Opmerking: De versie van de CSDL aangegeven door de naamruimte [http://schemas.microsoft.com/ado/2006/04/edm](http://schemas.microsoft.com/ado/2006/04/edm) , een complexe eigenschap hebben null-waarden bevatten = "False". |
| Standaardwaarde | Nee | De standaardwaarde van de eigenschap. |
|MaxLength | Nee | De maximale lengte van de waarde van de eigenschap. |
| FixedLength | Nee | **De waarde True** of **False** afhankelijk van of de waarde van de eigenschap wordt opgeslagen als een fiexed met lengte nul. |
| Precisie | Nee | Verwijst naar het maximum aantal cijfers in de numerieke waarde behouden. |
| Schaal | Nee | Maximum aantal decimalen wilt behouden in de numerieke waarde. |
| Unicode | Nee | **De waarde True** of **False** afhankelijk van of de waarde van de eigenschap opgeslagen als een Unicode-tekenreeks. |
| Sortering | Nee | Een tekenreeks die de sorteervolgorde in de gegevensbron wordt gebruikt. |
| ConcurrencyMode | Nee | **Geen** (de standaardwaarde) of **vast**. Als de waarde is ingesteld op **vast**, wordt de waarde van de eigenschap optimistische gelijktijdigheid controles worden gebruikt. |

Dit zijn de extra kenmerken die zijn toegevoegd aan de CSDL-specificatie:

**d:Map** - XPath-expressie uitgevoerd tegen de service wordt uitgevoerd en haalt een eigenschap van de uitvoer. Het opgegeven XPath is ten opzichte van het herhalende knooppunt dat is geselecteerd in het XPath van het knooppunt EntityType. Het is ook mogelijk een absolute XPath om een statische bron in elk van de uitvoer-knooppunten, zoals bijvoorbeeld een copyright-instructie die slechts één keer wordt gevonden in de oorspronkelijke service inclusief uitvoer maar moet aanwezig zijn in elk van de rijen in de OData-uitvoer opgeven. Voorbeeld van de service:

        `<foo>
          <bar>
           <baz0>… value …</baz0>
           <baz1>… value …</baz1>
           <baz2>… value …</baz2>
          </bar>
        </foo>`

De XPath-expressie zou worden ./bar/baz0 het knooppunt baz0 ophalen van de provider-service.

**d:CharMaxLength** - string type, kunt u de maximale lengte. Zie voorbeeld DataService CSDL

**d:IsPrimaryKey** - geeft aan of de kolom de primaire sleutel in de tabelweergave. Zie voorbeeld DataService CSDL.

**d:isExposed** - wordt bepaald als het tabelschema wordt weergegeven (in het algemeen true). Zie voorbeeld DataService CSDL

**d:IsView** *(Optioneel)* - true als dit is gebaseerd op een weergave in plaats van een tabel.  Zie voorbeeld DataService CSDL

**d:Tableschema** - Zie DataService CSDL voorbeeld

**d:ColumnName** - Is de naam van de kolom in de tabelweergave.  Zie voorbeeld DataService CSDL

**d:IsReturned** - Is de Boole-waarde waarmee wordt bepaald als de Service deze waarde aan de client geeft.  Zie voorbeeld DataService CSDL

**d:IsQueryable** - Is de Boole-waarde waarmee wordt bepaald als de kolom kan worden gebruikt in een databasequery.   Zie voorbeeld DataService CSDL

**d:OrdinalPosition** - Is de positie van de kolom numerieke van uiterlijk, x, in de tabel of de weergave, waarbij x 1 is het aantal kolommen in de tabel.  Zie voorbeeld DataService CSDL

**d:DatabaseDataType** - Is het gegevenstype van de kolom in de database, d.w.z. SQL-gegevenstype. Zie voorbeeld DataService CSDL

## <a name="supported-parametersproperty-types"></a>Ondersteunde Parameters/soorten
Hieronder volgen de ondersteunde typen parameters en eigenschappen. (Hoofdlettergevoelig)

| Primitieve typen | Beschrijving |
|----|----|
| Null | Het ontbreken van een waarde aangeeft |
| Boole-waarde | Hiermee geeft u het begrip wiskundige logica binaire waarde|
| Byte | 8-bits geheel-getalwaarde|
|Datum/tijd| Datum en tijd vertegenwoordigt met waarden tussen 00:00:00 uur, 1 januari 1753 A.D. tot en met 11:59:59 uur, December 9999 A.D.|
|Decimaal | Hiermee geeft u numerieke waarden met een vaste precisie en schaal. Dit type kunt beschrijven een numerieke waarde tussen negatieve 10 ^ 255 + 1 tot en met 10 positieve ^ 255 -1|
| Getal met dubbele precisie | Vertegenwoordigt een drijvende komma getal met een precisie van 15 cijfers waarmee de waarden bij benadering bereik van ± 2.23E-308 tot en met ± 1.79E +308. **Decimaal Exel uitvoer probleem gebruiken**|
| Enkele | Vertegenwoordigt een drijvende komma getal met een precisie van 7 cijfers waarmee de waarden bij benadering bereik van ± 1.18E-38 tot en met ± 3.40E +38|
|GUID |Hiermee geeft u een unieke id van 16 bytes (128-bits)-waarde |
|Int16|Hiermee geeft u een integer met 16-bits waarde |
|Int32|Hiermee geeft u een 32-bits integer met waarde |
|Int64|Hiermee geeft u een ondertekend 64-bits geheel getal |
|Tekenreeks | Hiermee geeft u gegevens van vaste of variabele-lengte tekens |


## <a name="see-also"></a>Zie ook
- Als u geïnteresseerd bent in wat de algehele OData toewijzingsproces en het doel, Lees dit artikel [Gegevenstoewijzing Service OData](marketplace-publishing-data-service-creation-odata-mapping.md) definities, structuren en instructies bekijken.
- Als u geïnteresseerd bent in voorbeelden bekijken, lezen [Gegevens Service OData Mapping voorbeelden](marketplace-publishing-data-service-creation-odata-mapping-examples.md) Zie voorbeeldcode en begrijpen de syntaxis en de context van dit artikel.
- Als u wilt terugkeren naar de voorgeschreven pad voor het publiceren van een Service op de markt Azure, Lees dit artikel [Data Service Publishing Guide](marketplace-publishing-data-service-creation.md).
