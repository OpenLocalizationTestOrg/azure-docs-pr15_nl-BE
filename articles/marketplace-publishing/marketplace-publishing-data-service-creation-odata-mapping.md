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

# <a name="mapping-an-existing-web-service-to-odata-through-csdl"></a>Een bestaand webservice toewijzen aan OData via CSDL

>[AZURE.IMPORTANT] **Op dit moment we zijn niet langer onboarding alle nieuwe Data Service uitgevers. Nieuwe dataservices zal niet krijgen goedgekeurd voor de aanbieding.** Als u beschikt over een SaaS zakelijke toepassing die u wilt publiceren op AppSource vindt u meer informatie [hier](https://appsource.microsoft.com/partners). Als u een IaaS toepassingen of developer service die u wilt publiceren op Azure Marketplace meer informatie vindt [hier](https://azure.microsoft.com/marketplace/programs/certified/).

Dit artikel geeft een overzicht over het gebruik van een CSDL een bestaande service toewijzen aan een compatibele OData-service. Hierin wordt uitgelegd hoe het document toewijzen (CSDL) die de input-aanvraag van de client via een aanroep van een transformeert en (data) van de uitvoer naar de client via een compatibele OData-feed back. Services voor de eindgebruikers beschrijft van Microsoft Azure Marketplace met behulp van de OData-protocol. Services die worden gebruikt door aanbieders van inhoud (gegevens eigenaren) worden blootgesteld in verschillende vormen, zoals REST, SOAP, enz.

## <a name="what-is-a-csdl-and-its-structure"></a>Wat is een CSDL en de structuur?
Een CSDL (conceptuele Schema Definition Language) is een specificatie definiëren van de webservice of databaseservice in algemene bewoordingen van XML naar Azure Marketplace te beschrijven.

Eenvoudig overzicht van de **stroom aanvragen:**

  `Client -> Azure Marketplace -> Content Provider’s Web Service (Get, Post, Delete, Put)`

De **gegevensstroom** is in de tegenovergestelde richting:

  `Client <- Azure Marketplace <- Content Provider’s WebService`

**Figuur 1** diagrammen hoe een client gegevens zou krijgen van een inhoudsprovider (de "service") door te gaan via de markt Azure.  De CSDL door de toewijzing/transformatie-component wordt gebruikt om de aanvraag te verwerken en gegevens uitwisselen tussen de aanbieder (s) en de aanvragende client.

*Figuur 1: Gedetailleerde stroom van de aanvragende client om inhoud via Azure Marketplace-provider*

  ![tekening](media/marketplace-publishing-data-service-creation-odata-mapping/figure-1.png)

Voor achtergrondinformatie over Atom, Atom Pub en de OData-protocol waarop de serverextensies Azure marktplaats bouwen, moeten u controleren: [http://msdn.microsoft.com/library/ff478141.aspx](http://msdn.microsoft.com/library/ff478141.aspx)

Uittreksel boven koppeling:     *"het doel van de Open Data protocol (hierna OData) is te voorzien in een protocol op basis van REST CRUD-achtige bewerkingen (maken, lezen, bijwerken en verwijderen) tegen middelen als data services weergegeven. Een "service" is een eindpunt wanneer blootgesteld aan een of meer "collecties" elk met nul of meer "posten", die bestaan uit gegevens hebt ingevoerd met de naam / waarde-paren. OData is gepubliceerd door Microsoft onder OASIS (organisatie voor de bevordering van gestructureerde informatie normen) normen zodat iedereen die wil met servers, clients of extra zonder royalty's of beperkingen maken kan."*

### <a name="three-critical-pieces-that-have-to-be-defined-by-the-csdl-are"></a>Er zijn drie belangrijke gegevens die moeten worden gedefinieerd door de CSDL:

- Het **eindpunt** van de Service Provider het Web adres (URI) van de Service
- **Gegevensparameters** worden doorgegeven als invoer naar de serviceprovider de definities van de parameters worden verzonden met de inhoudsprovider service naar het gegevenstype.
- **Schema** van de gegevens die worden geretourneerd naar de Service voor het aanvragen van het schema van de gegevens die worden geleverd door de aanbieder service, met inbegrip van de Container, collecties/tabellen variabelen/kolommen en gegevenstypen.

In het volgende diagram ziet u een overzicht van de stroom uit waarbij de client de OData-instructie (aanroep van de inhoudsprovider webservice) invoert om de resultaten/gegevens terug te krijgen.

  ![tekening](media/marketplace-publishing-data-service-creation-odata-mapping/figure-2.png)

### <a name="steps"></a>Stappen:

1. Client verzendt via een aanroep van aanvraag compleet met Input Parameters in XML-indeling naar Azure Marketplace
2. CSDL wordt gebruikt om de aanroep te valideren.
    - Serviceoproep opgemaakt wordt vervolgens verzonden naar de Content Providers Service door de markt Azure
3. De Webservice wordt uitgevoerd en de actie van het HTTP-woord tekstknooppunten wordt uitgevoerd (dat wil zeggen krijgt) worden de gegevens teruggestuurd naar Azure Marketplace waar de gevraagde gegevens (indien aanwezig) is gegarandeerd in XML-indeling naar de Client met behulp van de toewijzing van de CSDL.
4. De Client wordt verzonden gegevens (indien aanwezig) in de indeling XML of JSON

## <a name="definitions"></a>Definities

### <a name="odata-atom-pub"></a>ATOM OData-pub

Een uitbreiding van de ATOM pub waar elk item één rij van een resultaat vertegenwoordigt ingesteld. Het gedeelte inhoud van de post wordt verbeterd om de waarden van de rij – als sleutel-waardeparen bevatten. Meer informatie is hier vinden: [https://www.odata.org/documentation/odata-version-3-0/atom-format/](https://www.odata.org/documentation/odata-version-3-0/atom-format/)

### <a name="csdl---conceptual-schema-definition-language"></a>CSDL - conceptuele Schema Definition Language

Kan het definiëren van functies (SPROCs) en entiteiten die worden weergegeven via een database. Meer informatie hier vinden: [http://msdn.microsoft.com/library/bb399292.aspx](http://msdn.microsoft.com/library/bb399292.aspx)  

> [AZURE.TIP] Klik op de vervolgkeuzelijst voor **andere versies** en selecteer een versie als het artikel niet wordt weergegeven.

### <a name="edm---entry-data-model"></a>EDM - gegevensmodel post
- Overzicht: [http://msdn.microsoft.com/library/vstudio/ee382825 (v=vs.100).aspx][OverviewLink] [OverviewLink]: http://msdn.microsoft.com/library/vstudio/ee382825 (v=vs.100).aspx
- Voorbeeld: [http://msdn.microsoft.com/library/aa697428 (v=vs.80).aspx][PreviewLink] [PreviewLink]: http://msdn.microsoft.com/library/aa697428 (v=vs.80).aspx
- Gegevenstypen: [http://msdn.microsoft.com/library/bb399548 (v=VS.100).aspx][DataTypesLink] [DataTypesLink]: http://msdn.microsoft.com/library/bb399548 (v=VS.100).aspx

De volgende bevat gedetailleerde links naar rechts lopen uit waarbij de client de OData-instructie (aanroep van de inhoudsprovider webservice) invoert aan het ophalen van de resultaten/gegevens terug:

  ![tekening](media/marketplace-publishing-data-service-creation-odata-mapping/figure-3.png)


## <a name="csdl-basics"></a>Grondbeginselen van CSDL

Een CSDL (conceptuele Schema Definition Language) is een specificatie definiëren van de webservice of databaseservice in algemene bewoordingen van XML naar Azure Marketplace te beschrijven. CSDL beschrijving van de essentiële stuks die **is het doorgeven van gegevens uit de gegevensbron op de markt Azure mogelijk.** De belangrijkste stukken worden hier beschreven:

- Informatie over de interface met een beschrijving van alle algemeen beschikbare functies (knooppunt FunctionImport)
- Gegevenstype-informatie over alle bericht-requests(input) en responses(outputs) bericht (knooppunten EntitySet-EntityContainer/EntityType)
- Bindende inlichtingen over het transportprotocol moet worden gebruikt (knooppunt kop)
- Adresgegevens voor het zoeken van de opgegeven service (kenmerk BaseURI)

Kortom, vertegenwoordigt de CSDL een platform-onafhankelijke en taal contract tussen de aanvrager van de service en de serviceprovider. Met behulp van de CSDL, een client zoekt een Internet-service of-database en de openbaar beschikbare functies aanroepen.

### <a name="relating-a-csdl-to-a-database-or-a-collection"></a>Een CSDL die betrekking hebben op een Database of een collectie
**De specificatie van CSDL**

CSDL is een XML-grammatica voor het beschrijven van een webservice. De specificatie zelf is verdeeld in 4 belangrijke elementen: EntitySet, FunctionImport; Naamruimte, en EntityType.

Deze onttrekking gemakkelijker te begrijpen, kunt een CSDL koppelen aan een tabel.

Houd er rekening mee;

  CSDL geeft een platform-onafhankelijke en taal contract tussen de **aanvrager van de service** en de **serviceprovider**. CSDL, een **client** kan naar een **web service/database service** en aanroepen van het openbaar **functies.**

De vier delen van een CSDL voor een Service kunnen worden beschouwd in een Database, tabel, kolom- en Procedure-archief.

Die betrekking hebben op deze voor een Service als volgt:

- EntityContainer ~ = Database
- EntitySet ~ = Table
- EntityType ~ = kolommen
- FunctionImport ~ = opgeslagen Procedure

**HTTP-woorden die zijn toegestaan**
- GET – geeft waarden uit de database (geeft als resultaat een collectie)
- BOEKEN – wordt gebruikt voor het doorsturen van gegevens en optionele retourwaarden van de db (maken een nieuw item in de geretourneerde id /-URI-collectie)
- VERWIJDEREN: Hiermee verwijdert u gegevens uit de database (verwijdert een collectie)
- PLAATS: gegevens in een database bijwerken (een verzameling vervangen of maken)

## <a name="metadatamapping-document"></a>Document metagegevens/koppeling

Het document metagegevens/koppeling wordt gebruikt voor een aanbieder van de inhoud van bestaande webservices toewijzen, zodat deze kan worden weergegeven als een webservice OData door het systeem Azure Marketplace. Is gebaseerd op CSDL en implementeert via Azure Marketplace webservices op basis van enkele uitbreidingen voor CSDL aangepast aan de behoeften van de REST. De extensies zijn gevonden in de naamruimte [http://schemas.microsoft.com/dallas/2010/04](http://schemas.microsoft.com/dallas/2010/04) .

Volgt een voorbeeld van de CSDL: (kopiëren en plakken het onderstaande voorbeeld CSDL in een XML-editor en wijzigen zodat deze overeenkomen met uw Service.  Plak in CSDL Mapping DataService tabblad bij het maken van uw service in [Azure Marketplace Portal voor publiceren](https://publish.windowsazure.com)).

**Voorwaarden:** De voorwaarden van de gebruikersinterface (PPUI) [Portal voor publiceren](https://publish.windowsazure.com) betreffende de voorwaarden voor CSDL.
- Bieden "Titel" in de PPUI heeft betrekking op MyWebOffer
- Mijnbedrijf in de PPUI heeft betrekking op de **Weergavenaam van Publisher** in [Microsoft Developer Center](http://dev.windows.com/registration?accountprogram=azure) UI
- De API heeft betrekking op een website of Service van gegevens (een Plan in de PPUI)

**Hiërarchie:** 
 (aanbieder) van een bedrijf eigenaar is van de voorstellen die Plan(s) hebben, namelijk service(s), welke regel omhoog met een API.

### <a name="webservice-csdl-example"></a>Voorbeeld van de WebService-CSDL

Maakt verbinding met een service die van een eindpunt van de web-toepassing (zoals een C#-toepassing weergeeft)

        <?xml version="1.0" encoding="utf-8"?>
        <!-- The namespace attribute below is used by our system to generate C#. You can change “MyCompany.MyOffer” to something that makes sense for you, but change “MyOffer” consistently throughout the document. -->
        <Schema Namespace="MyCompany.MyWebOffer" Alias="MyOffer" xmlns="http://schemas.microsoft.com/ado/2009/08/edm" xmlns:d="http://schemas.microsoft.com/dallas/2010/04" >
        <!-- EntityContainer groups all the web service calls together into a single offering. Every web service call has a FunctionImport definition. -->
          <EntityContainer Name="MyOffer">
        <!-- EntitySet is defined for CSDL compatibility reasons, not required for ReturnType=”Raw”
        @Name is used as reference by FunctionImport @EntitySet. And is used in the customer facing UI as name of the Service.
        @EntityType is used to point at the type definition near the bottom of this file. -->
            <EntitySet Name="MyEntities" EntityType="MyOffer.MyEntityType" />
        <!-- Add a FunctionImport for every service method. Multiple FunctionImports can share a single return type (EntityType). -->
        <!-- ReturnType is either Raw() for a stream or Collection() for an Atom feed. Ex. of Raw: ReturnType=”Raw(text/plain)” -->
        <!—EntitySet is the entityset defined above, and is needed if ReturnType is not Raw -->
        <!-- BaseURI attribute defines the service call, replace & with the encode value (&amp;).
        In the input name value pairs {param} represents passed in value.
        Or the value can be hard coded as with AccountKey. -->
        <!-- AllowedHttpMethods optional (default = “GET”), allows the CSDL to specifically specify the verb of the service, “Get”, “Post”, “Put”, or “Delete”. -->
        <!-- EncodeParameterValues, True encodes the parameter values, false does not. -->
        <!-- BaseURI is translated into an URITemplate which defines how the web service call is exposed to marketplace customers.
        Ex. https://api.datamarket.azure.com/mycompany/MyOfferPlan?name={name}
        BaseURI is XML encoded, the {...} point to the parameters defined below.
        Marketplace will read the parameters from this URITemplate and fill the values into the corresponding parameters of the BaseUri or RequestBody (below) during calls to your service.  
        It is okay for @d:BaseUri to include information only for Marketplace consumption, it will not be exposed to end users. i.e. the hardcoded AccountKey in the above BaseURI does not show up in the client facing URITemplate. -->
            <FunctionImport Name="MyWebServiceMethod"
                            EntitySet="MyEntities"
                            ReturnType="Collection(MyOffer.MyEntityType)"
        d:AllowedHttpMethods="GET"
        d:EncodeParameterValues="true"
        d:BaseUri="http://services.organization.net/MyServicePath?name={name}&amp;AccountKey=22AC643">
        <!-- Definition of the RequestBody is only required for HTTP POST requests and is optional for HTTP GET requests. -->
        <d:RequestBody d:httpMethod="POST">
                <!-- Use {} for placeholders to insert parameters. -->
                <!-- This example uses SOAP formatting, but any POST body can be used. -->
            <!-- This example shows how to pass userid and password via the header -->
                <![CDATA[<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:MyOffer="http://services.organization.net/MyServicePath">
                  <soapenv:Header/>
                  <soapenv:Body>
                    <MyOffer:ws_MyWebServiceMethod>
                      <myWebServiceMethodRequest>
                        <!--This information is not exposed to end users. -->
                        <UserId>userid</UserId>
                        <Password>password</Password>
                        <!-- {name} is replaced with the value read from @d:UriTemplate above -->
                        <Name>{name}</Name>
                        <!-- Parameters can be used more than once and are not limited to appearing as the value of an element -->
                        <CustomField Name="{name}" />
                        <MyField>Static content</MyField>
                      </myWebServiceMethodRequest>
                    </MyOffer:ws_MyWebServiceMethod>
                  </soapenv:Body>
                </soapenv:Envelope>      
              ]]>
        </d:RequestBody>
        <!-- Title, Rights and Description are optional and used to specify values to insert into the ATOM feed returned to the end user.  You can specify the element to contain a fixed message by providing a value for the element (this is the default value).  @d:Map is an XPath expression that points into the response returned by your service and is optional.  -->
        <d:Title d:Map="/MyResponse/Title">Default title.</d:Title>
        <d:Rights>© My copyright. This is a fixed response. It is okay to also add a d:Map attribute to override this text.</d:Rights>
        <d:Description d:Map="/MyResponse/Description"></d:Description>
        <d:Namespaces>
        <d:Namespace d:Prefix="p"  d:Uri="http://schemas.organization.net/2010/04/myNamespace" />
        <d:Namespace d:Prefix="p2" d:Uri="http://schemas.organization.net/2010/04/MyNamespace2" />
        </d:Namespaces>
        <!-- Parameters of the web service call:
        @Name should match exactly (case sensitive) the {…} placeholders in the @d:BaseUri, @d:UriTemplate, and d:RequestBody, i.e. “name” parameter in above BaseURI.
        @Mode is always "In", compatibility with CSDL
        @Type is the EDM.SimpleType of the parameter, see http://msdn.microsoft.com/library/bb399548(v=VS.100).aspx
        @d:Nullable indicates whether the parameter is required.
        @d:Regex - optional, attribute to describe the string, limiting unwanted input at the entry of the system
        @d:Description - optional, is used by Service Explorer as help information
        @d:SampleValues - optional, is used by Service Explorer as help information. Multiple Sample values are separated by '|', e.g. "804735132|234534224|23409823234"
        @d:Enum - optional for string type. Contains an enumeration of possible values separated by a '|', e.g. d:enum="english|metric|raw". Will be converted in a dropdown list in the Service Explorer.
        -->
        <Parameter name="name" Mode="In" Type="String" d:Nullable="false" d:Regex="^[a-zA-Z]*$" d:Description="A name that cannot contain any spaces or non-alpha non-English characters"
        d:Enum="George|John|Thomas|James"
        d:SampleValues="George"/>
        <Parameter Name=" AccountKey" Mode="In" Type="String" d:Nullable="false" />

        <!-- d:ErrorHandling is an optional element. Use it define standardized errors by evaluating the service response. -->
        <d:ErrorHandling>
        <!-- Any number of d:Condition elements are allowed, they are evaluated in the order listed.
        @d:Match is an Xpath query on the service response, it should return true or false where true indicates an error.
        @d:httpStatusCode is the error code to return if an response matches the error.
        @d:errorMessage is the user friendly message to return when an error occurs.
        -->
        <d:Condition d:Match="/Result/ErrorMessage[text()='Invalid token']" d:HttpStatusCode="403" d:ErrorMessage="User cannot connect to the service." />
        </d:ErrorHandling>
           </FunctionImport>

            <!-- The EntityContainer defines the output data schema -->
        </EntityContainer>
        <!-- The EntityType @d:Map defines the repeating node (an XPath query) in the response (output data schema). -->
        <!-- If these nodes are outside a namespace, add the prefix in the xpath. -->
        <!--
        @Name - define your user readable name, will become an XML element in the ATOM feed, so comply with the XML element naming restrictions (no spaces or other illegal characters).
        @Type is the EDM.SimpleType of the parameter, see http://msdn.microsoft.com/library/bb399548(v=VS.100).aspx.
        @d:Map uses an Xpath query to point at the location to extract the content from your services response.
        The "." is relative to the repeating node in the EntityType @d:Map Xpath expression.
        -->
            <EntityType Name="MyEntityType" d:Map="/MyResponse/MyEntities">
        <Property Name="ID" d:IsPrimaryKey="True" Type="Int32"  Nullable="false" d:Map="./Remaining[@Amount]"/>
        <Property Name="Amount" Type="Double"   Nullable="false" d:Map="./Remaining[@Amount]"/>
        <Property Name="City"   Type="String"   Nullable="false" d:Map="./City"/>
        <Property Name="State"  Type="String"   Nullable="false" d:Map="./State"/>
        <Property Name="Zip"    Type="Int32"    Nullable="false" d:Map="./Zip"/>
        <Property Name="Updated"    Type="DateTime" Nullable="false" d:Map="./Updated"/>
        <Property Name="AdditionalInfo" Type="String" Nullable="true"
        d:Map="./Info/More[1]"/>
            </EntityType>
        </Schema>

> [AZURE.TIP] Meer voorbeelden van webservice-CSDL weergeven in het artikel [voorbeelden voor het toewijzen van een bestaand webservice aan OData via CSDLs](marketplace-publishing-data-service-creation-odata-mapping-examples.md)

###<a name="dataservice-csdl-example"></a>Voorbeeld van de CSDL DataService

Maakt verbinding met een service die bloot een databasetabel of weergave als een eindpunt onder voorbeeld ziet u dat twee API's voor de databank op basis van API-CSDL (met weergaven plaats tabellen).

        <?xml version="1.0"?>
        <!-- The namespace attribute below is used by our system to generate C#. You can change “MyCompany.MyOffer” to something that makes sense for you, but change “MyOffer” consistently throughout the document. -->
        <Schema Namespace="MyCompany.MyDataOffer" Alias="MyOffer" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ado/2009/08/edm">
        <!-- EntityContainer groups all the data service calls together into a single offering. Every web service call has a FunctionImport definition. -->
        <EntityContainer Name="MyOfferContainer">
        <!-- EntitySet is defined for CSDL compatibility reasons, not required for ReturnType=”Raw”
            Think of the EntitySet as a Service
        @Name is used in the customer facing UI as name of the Service.
        @EntityType is used to point at the type definition (returned set of table columns). -->
        <EntitySet Name="CompanyInfoEntitySet" EntityType="MyOffer.CompanyInfo" />
        <EntitySet Name="ProductInfoEntitySet" EntityType="MyOffer.ProductInfo" />
        </EntityContainer>
        <!-- EntityType defines result (output); the table (or view) and columns to be returned by the data service.)
            Map is the schema.tabel or schema.view
            dals.TableName is the table Name
            Name is the name identifier for the EntityType and the Name of the service exposed to the client via the UI.
            dals:IsExposed determines if the table schema is exposed (generally true).
            dals:IsView (optional) true if this is based on a view rather than a table
            dals:TableSchema is the schema name of the table/view
        -->
        <EntityType
        Map="[dbo].[CompanyInfo]"
        dals:TableName="CompanyInfo"
        Name="CompanyInfo"
        dals:IsExposed="true"
        dals:IsView="false"
        dals:TableSchema="dbo"
        xmlns:dals="http://schemas.microsoft.com/dallas/2010/04">
        <!-- Property defines the column properties and the output of the service.
            dals:ColumnName is the name of the column in the table /view.
            Type is the emd.SimpleType
            Nullable determines if NULL is a valid output value
            dals.CharMaxLenght is the maximum length of the output value
            Name is the name of the Property and is exposed to the client facing UI
            dals:IsReturned is the Boolean that determines if the Service exposes this value to the client.
            IsQueryable is the Boolean that determines if the column can be used in a database query
            (For data Services: To improve Performance make sure that columns marked ISQueryable=”true” are in an index.)
            dals:OrdinalPosition is the numerical position x in the table or the View, where x is from 1 to the number of columns in the table.
            dals:DatabaseDataType is the data type of the column in the database, i.e. SQL data type dals:IsPrimaryKey indicates if the column is the Primary key in the table/view.  (The columns marked ISPrimaryKey are used in the Order by clause when returning data.)
        -->
        <Property dals:ColumnName="data" Type="String" Nullable="true" dals:CharMaxLength="-1" Name="data" dals:IsReturned="true" dals:IsQueryable="false" dals:IsPrimaryKey="false" dals:OrdinalPosition="3" dals:DatabaseDataType="nvarchar" />
        <Property dals:ColumnName="id" Type="Int32" Nullable="false" Name="id" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="true" dals:OrdinalPosition="1" dals:NumericPrecision="10" dals:DatabaseDataType="int" />
        <Property dals:ColumnName="ticker" Type="String" Nullable="true" dals:CharMaxLength="10" Name="ticker" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="false" dals:OrdinalPosition="2" dals:DatabaseDataType="nvarchar" />
        </EntityType>
        <EntityType Map="[dbo].[ProductInfo]" dals:TableName="ProductInfo" Name="ProductInfo" dals:IsExposed="true" dals:IsView="false" dals:TableSchema="dbo" xmlns:dals="http://schemas.microsoft.com/dallas/2010/04">
        <Property dals:ColumnName="companyid" Type="Int32" Nullable="true" Name="companyid" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="false" dals:OrdinalPosition="2" dals:NumericPrecision="10" dals:DatabaseDataType="int" />
        <Property dals:ColumnName="id" Type="Int32" Nullable="false" Name="id" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="true" dals:OrdinalPosition="1" dals:NumericPrecision="10" dals:DatabaseDataType="int" />
        <Property dals:ColumnName="product" Type="String" Nullable="true" dals:CharMaxLength="50" Name="product" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="false" dals:OrdinalPosition="3" dals:DatabaseDataType="nvarchar" />
        </EntityType>
        </Schema>

## <a name="see-also"></a>Zie ook
- Als u geïnteresseerd bent in leren en wat zijn de specifieke knooppunten en hun parameters, Lees dit artikel [Service OData Mapping gegevensknooppunten](marketplace-publishing-data-service-creation-odata-mapping-nodes.md) voor definities en uitleg, voorbeelden en case context gebruikt.
- Als u geïnteresseerd bent in voorbeelden bekijken, lezen [Gegevens Service OData Mapping voorbeelden](marketplace-publishing-data-service-creation-odata-mapping-examples.md) Zie voorbeeldcode en begrijpen de syntaxis en de context van dit artikel.
- Als u wilt terugkeren naar de voorgeschreven pad voor het publiceren van een Service op de markt Azure, Lees dit artikel [Data Service Publishing Guide](marketplace-publishing-data-service-creation.md).
