<properties
    pageTitle="Azure SQL Database JSON-functies | Microsoft Azure"
    description="Azure SQL-Database kan parseren, een query of gegevens opmaken in de notatie JavaScript Object Notation (JSON)."
    services="sql-database"
    documentationCenter=""
    authors="jovanpop-msft"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="08/17/2016"
    ms.author="jovanpop"
   ms.workload="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>



# <a name="getting-started-with-json-features-in-azure-sql-database"></a>Aan de slag met de functies van JSON in Azure SQL-Database

Azure SQL-Database kunt u parseren en relationele gegevens geëxporteerd als JSON tekst weergegeven in de notatie JavaScript Object Notation [(JSON)](http://www.json.org/) gegevens opvragen.

JSON is een populaire indeling die wordt gebruikt voor het uitwisselen van gegevens in de moderne web- en mobiele toepassingen. JSON is ook gebruikt voor de semi-gestructureerde gegevens opslaan in logboekbestanden of in NoSQL databases zoals [Azure DocumentDB](https://azure.microsoft.com/services/documentdb/). Veel REST-webservices worden opgemaakt als JSON tekst of gegevens accepteren opgemaakt als JSON. Meest Azure services zoals [Azure Search](https://azure.microsoft.com/services/search/), [Azure opslag](https://azure.microsoft.com/services/storage/)en [Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) hebben REST eindpunten die terug of JSON verbruiken.

Azure SQL-Database, kunt u gemakkelijk werken met de JSON-gegevens en uw database te integreren met moderne services.

## <a name="overview"></a>Overzicht

Azure SQL-Database bevat de volgende functies voor het werken met de JSON-gegevens:

![JSON-functies](./media/sql-database-json-features/image_1.png)

Als u tekst JSON, kunt u gegevens ophalen uit een JSON of Controleer of JSON is juist ingedeeld met behulp van de ingebouwde functies, [JSON_VALUE](https://msdn.microsoft.com/library/dn921898.aspx), [JSON_QUERY](https://msdn.microsoft.com/library/dn921884.aspx)en [ISJSON](https://msdn.microsoft.com/library/dn921896.aspx). De [JSON_MODIFY](https://msdn.microsoft.com/library/dn921892.aspx) -functie kunt u de waarde in JSON tekst bijwerken. Voor meer geavanceerde query's en -analyse, kan [OPENJSON](https://msdn.microsoft.com/library/dn921885.aspx) functie een array van objecten JSON transformeren in een set rijen. Een SQL-query kan worden uitgevoerd op de resultaatset geretourneerd. Er is ten slotte een [Voor JSON](https://msdn.microsoft.com/library/dn921882.aspx) -component waarmee u gegevens in relationele tabellen opgeslagen als JSON tekst opmaken.

## <a name="formatting-relational-data-in-json-format"></a>Opmaken van relationele gegevens in JSON-indeling
Als u een webservice dat wordt gegevens uit de database laag en biedt een antwoord in JSON opmaken of client-side JavaScript-raamwerken, of bibliotheken die gegevens accepteren die zijn opgemaakt als JSON hebt, kunt u de database-inhoud opmaken als JSON rechtstreeks in een SQL-query. U niet langer toepassingscode schrijven die resultaten van Azure SQL Database als JSON indelingen moet nemen, of sommige JSON serialisatie bibliotheek in tabelvorm queryresultaten converteren en vervolgens geserialiseerd objecten naar JSON-indeling. In plaats daarvan kunt u de component FOR JSON SQL query-resultaten opmaken als JSON in Azure SQL-Database en deze rechtstreeks in uw toepassing gebruiken.

In het volgende voorbeeld worden rijen uit de tabel Sales.Customer met behulp van de component FOR JSON opgemaakt als JSON:

```
select CustomerName, PhoneNumber, FaxNumber
from Sales.Customers
FOR JSON PATH
```

Het pad voor JSON-component worden de resultaten van de query als JSON tekst opgemaakt. Kolomnamen worden gebruikt als sleutels, terwijl de celwaarden worden gegenereerd als JSON-waarden:

```
[
{"CustomerName":"Eric Torres","PhoneNumber":"(307) 555-0100","FaxNumber":"(307) 555-0101"},
{"CustomerName":"Cosmina Vlad","PhoneNumber":"(505) 555-0100","FaxNumber":"(505) 555-0101"},
{"CustomerName":"Bala Dixit","PhoneNumber":"(209) 555-0100","FaxNumber":"(209) 555-0101"}
]
```

De resultaatset wordt opgemaakt als een JSON-matrix waarin elke rij is opgemaakt als een afzonderlijk JSON-object.

PAD geeft aan dat u de opmaak van uw resultaat JSON aanpassen kunt met behulp van puntnotatie in Kolomaliassen. De volgende query wordt de naam van de sleutel "CustomerName" in de uitvoerindeling JSON en telefoon-en faxnummers in het onderliggende object 'Contactpersoon' worden geplaatst:

```
select CustomerName as Name, PhoneNumber as [Contact.Phone], FaxNumber as [Contact.Fax]
from Sales.Customers
where CustomerID = 931
FOR JSON PATH, WITHOUT_ARRAY_WRAPPER
```

De uitvoer van deze query ziet er zo uit:

```
{
    "Name":"Nada Jovanovic",
    "Contact":{
           "Phone":"(215) 555-0100",
           "Fax":"(215) 555-0101"
    }
}
```

In dit voorbeeld wij een JSON-object in plaats van een matrix geretourneerd door de optie [WITHOUT_ARRAY_WRAPPER](https://msdn.microsoft.com/library/mt631354.aspx) . U kunt deze optie gebruiken als u weet dat u een object als resultaat van de query retourneert.

De belangrijkste waarde van de component FOR JSON is dat u kunt complexe hiërarchische gegevens terug uit de database die is opgemaakt als geneste JSON-objecten of -matrices. In het volgende voorbeeld ziet u hoe u Orders die deel uitmaken van de klant als een geneste reeks Orders opnemen:

```
select CustomerName as Name, PhoneNumber as Phone, FaxNumber as Fax,
        Orders.OrderID, Orders.OrderDate, Orders.ExpectedDeliveryDate
from Sales.Customers Customer
    join Sales.Orders Orders
        on Customer.CustomerID = Orders.CustomerID
where Customer.CustomerID = 931
FOR JSON AUTO, WITHOUT_ARRAY_WRAPPER

```

In plaats van afzonderlijke query's zodat u de gegevens van de klant en vervolgens om een lijst met gerelateerde Orders ophalen, krijgt u alle gegevens met één query, zoals aangegeven in de volgende voorbeelduitvoer:

```
{
  "Name":"Nada Jovanovic",
  "Phone":"(215) 555-0100",
  "Fax":"(215) 555-0101",
  "Orders":[
    {"OrderID":382,"OrderDate":"2013-01-07","ExpectedDeliveryDate":"2013-01-08"},
    {"OrderID":395,"OrderDate":"2013-01-07","ExpectedDeliveryDate":"2013-01-08"},
    {"OrderID":1657,"OrderDate":"2013-01-31","ExpectedDeliveryDate":"2013-02-01"}
]
}
```

## <a name="working-with-json-data"></a>Werken met de JSON-gegevens

Als u geen strikt gestructureerde gegevens, hebt u complexe onderliggende objecten, matrices of hiërarchische gegevens of als uw gegevensstructuren mettertijd, kunt de JSON-indeling u een complexe structuur vertegenwoordigen.

JSON is een tekstindeling die kan worden gebruikt als andere tekenreekstype in Azure SQL-Database. U kunt verzenden of JSON-gegevens opslaan als een standaard NVARCHAR:

```
CREATE TABLE Products (
  Id int identity primary key,
  Title nvarchar(200),
  Data nvarchar(max)
)
go
CREATE PROCEDURE InsertProduct(@title nvarchar(200), @json nvarchar(max))
AS BEGIN
    insert into Products(Title, Data)
    values(@title, @json)
END
```

De JSON-gegevens die worden gebruikt in dit voorbeeld wordt met behulp van het type NVARCHAR(MAX) vertegenwoordigd. JSON kan worden ingevoegd in deze tabel of als een argument van de opgeslagen procedure met behulp van standaard Transact-SQL-syntaxis in het volgende voorbeeld wordt getoond:

```
EXEC InsertProduct 'Toy car', '{"Price":50,"Color":"White","tags":["toy","children","games"]}'
```

Een client-side taal of de bibliotheek die geschikt is voor het gegevenstype string in Azure SQL-Database werkt ook met de JSON-gegevens. JSON kan worden opgeslagen in een tabel die het type NVARCHAR, zoals een tabel geoptimaliseerd voor geheugen of een systeem versioned ondersteunt. JSON voorziet niet in een beperking in de client-side code of in de databaselaag.

## <a name="querying-json-data"></a>JSON-gegevens opvragen

Als u gegevens die zijn opgemaakt als JSON opgeslagen in Azure SQL-tabellen, kunnen JSON-functies u deze gegevens in een SQL-query.

JSON-functies die beschikbaar in Azure SQL-database kunt zijn u gegevens die zijn opgemaakt als JSON als elke andere SQL-gegevenstype behandelen. U kunt gemakkelijk waarden ophalen uit de tekst JSON en JSON-gegevens in een query gebruiken:

```
select Id, Title, JSON_VALUE(Data, '$.Color'), JSON_QUERY(Data, '$.tags')
from Products
where JSON_VALUE(Data, '$.Color') = 'White'

update Products
set Data = JSON_MODIFY(Data, '$.Price', 60)
where Id = 1
```

De functie JSON_VALUE haalt een waarde uit een JSON tekst opgeslagen in de kolom gegevens. Deze functie gebruikt een JavaScript-achtige pad om te verwijzen naar een waarde in JSON tekst uitpakken. De opgehaalde waarde kan worden gebruikt in elk deel van een SQL-query.

De functie JSON_QUERY is vergelijkbaar met JSON_VALUE. In tegenstelling tot JSON_VALUE haalt deze functie complexe onderliggende object zoals arrays of objecten die in JSON tekst worden geplaatst.

De functie JSON_MODIFY geeft u het pad van de waarde in de JSON-tekst die moet worden bijgewerkt, als een nieuwe waarde die de oude versie wordt overschreven. Op deze manier kunt u gemakkelijk JSON tekst bijwerken zonder de gehele structuur reparsing.

Aangezien JSON is opgeslagen in een standaardtekst, zijn er geen garanties dat de waarden in de kolommen correct zijn opgemaakt. U kunt controleren of dat tekst die is opgeslagen in de kolom JSON juist is geformatteerd met behulp van standaard Azure SQL Database check-beperkingen en de functie ISJSON:

```
ALTER TABLE Products
    ADD CONSTRAINT [Data should be formatted as JSON]
        CHECK (ISJSON(Data) > 0)
```

Als de ingevoerde tekst is juist opgemaakt JSON, de functie ISJSON retourneert de waarde 1. Bij elke invoegen of bijwerken van JSON kolom controleren deze beperking is nieuwe tekstwaarde niet verkeerd JSON.

## <a name="transforming-json-into-tabular-format"></a>Omzetten van JSON in tabelvorm

Azure SQL-Database kunt u JSON-collecties in een tabelvormige indeling en laden of query JSON-gegevens te transformeren.

OPENJSON is een tabelwaarde functie, JSON tekst parseert, wordt gezocht naar een array van objecten JSON, de elementen van de array doorlopen en retourneert één rij in de resulterende uitvoer voor elk element van de matrix.

![JSON in tabelvorm](./media/sql-database-json-features/image_2.png)

In het bovenstaande voorbeeld kunt we opgeven waar u de JSON-matrix die moet worden geopend (in de $. Orders pad), welke kolommen moeten worden geretourneerd als resultaat en waar vind ik de JSON-waarden die worden geretourneerd als cellen.

We kunnen transformeren een JSON-array in de @orders variabele in een set met rijen, deze resultaatset analyseren of rijen in een standaard tabel invoegen:

```
CREATE PROCEDURE InsertOrders(@orders nvarchar(max))
AS BEGIN

    insert into Orders(Number, Date, Customer, Quantity)
    select Number, Date, Customer, Quantity
    OPENJSON (@orders)
     WITH (
            Number varchar(200),
            Date datetime,
            Customer varchar(200),
            Quantity int
     )

END
```
Het verzamelen van orders opgemaakt als een matrix JSON en aangeboden als een parameter voor de opgeslagen procedure kan worden geparseerd en ingevoegd in de tabel Orders.

## <a name="next-steps"></a>Volgende stappen

Bekijk meer informatie over het integreren van JSON in uw toepassing, deze bronnen:

- [TechNet-Blog](https://blogs.technet.microsoft.com/dataplatforminsider/2016/01/05/json-in-sql-server-2016-part-1-of-4/)
- [MSDN-documentatie](https://msdn.microsoft.com/library/dn921897.aspx)
- [9 video kanaal](https://channel9.msdn.com/Shows/Data-Exposed/SQL-Server-2016-and-JSON-Support)

Meer informatie over de verschillende scenario's voor het integreren van JSON in uw toepassing, Zie de demo's van dit [kanaal 9 video](https://channel9.msdn.com/Events/DataDriven/SQLServer2016/JSON-as-a-bridge-betwen-NoSQL-and-relational-worlds) of een scenario dat overeenkomt met de use-case in [JSON blogberichten](http://blogs.msdn.com/b/sqlserverstorageengine/archive/tags/json/)zoeken.
